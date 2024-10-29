## 缓存击穿

缓存击穿是指在高并发的情况下，当某个**热点数据的缓存突然失效**（过期或被删除）并且**缓存重建业务较复杂**，大量请求直接穿透到后端数据库，导致数据库负载过高，甚至崩溃的问题。由于并发用户特别多，同时读缓存没读到数据，又去数据库中取数据，引起数据库压力瞬间增大。

解决方法：互斥锁构建缓存和逻辑过期时间

> 互斥锁构建缓存

在热点key失效后，加锁，确保只有一个线程查询数据库并构建缓存。其他的线程等待并重试在缓存中取值。

优点

* 一致性高

缺点

* 由于使用了锁，线程等待，性能低，还可以能出现死锁。

代码实现

```java
 /**
     * 获取锁
     * @param key
     * @return
     */
    private boolean tryLock(String key){
        Boolean flag = stringRedisTemplate.opsForValue().setIfAbsent(key, "1", 20, TimeUnit.SECONDS);
        return BooleanUtil.isTrue(flag);

    }

    /**
     * 释放锁
     * @param key
     */
    private  void unlock(String key){
        stringRedisTemplate.delete(key);
    }

/**
     * 查询商户信息 缓存击穿互斥锁
     * @param id
     * @return
     */
    public Shop queryWithMutex(Long id){
        String shopKey = CACHE_SHOP_KEY+ id;

        // 1. 从redis中查询店铺缓存
        String shopJson = stringRedisTemplate.opsForValue().get(shopKey);

        //2.判断是否命中缓存  isnotblank false: "" or "/t/n" or "null"
        if(StrUtil.isNotBlank(shopJson)){
            // 3.若命中则返回信息
            Shop shop = JSONUtil.toBean(shopJson, Shop.class);
            return shop;
        }
        //数据穿透判空   不是null 就是空串 ""
        if (shopJson != null){
            //返回错误信息
//            return  Result.fail("没有该商户信息（缓存）");
            return null;
        }
        //4.没有命中缓存，查数据库
        //todo :解决缓存击穿  不能直接查数据库。 利用互斥锁解决

        /**
         * 实现缓存重建
         * 1. 获取互斥锁
         * 2. 判断是否成功
         * 3. 失败就休眠重试
         * 4.成功 查数据库
         * 5 数据库存在该数据写入缓存
         * 6 不存在返回错误信息并写入缓存“”
         * 7 释放锁
         *
         */

        //获取互斥锁 失败  休眠重试
        String lockKey = "lock:shop" + id;
        Shop shop=null;

        try {
            boolean isLock = tryLock(lockKey);
            //获取锁失败
            if (!isLock) {

                System.out.println("获取锁失败，重试");
                Thread.sleep(50);
                return queryWithMutex(id);//递归 重试
            }

            // 获取锁成功，再次检测缓存是否存在，存在就无需构建缓存，因为可能有的线程刚构建好缓存并释放锁，其他线程获取了锁
            //检测缓存是否存在  存在
            shopJson = stringRedisTemplate.opsForValue().get(shopKey);
            if (StrUtil.isNotBlank(shopJson)) {
                return JSONUtil.toBean(shopJson, Shop.class);
            }
            if (shopJson !=null){
                return null;
            }
            // 缓存不存在
            // 查数据库
             shop = super.getById(id);
            Thread.sleep(200);//模拟你测试环境 热点key失效模拟重建延迟
            if (shop == null){
                //没有该商户信息
                stringRedisTemplate.opsForValue().set(shopKey,"",CACHE_NULL_TTL,TimeUnit.SECONDS);
                return null;
            }
            //有该商户信息
            stringRedisTemplate.opsForValue().set(CACHE_SHOP_KEY+ id, JSONUtil.toJsonStr(shop),CACHE_SHOP_TTL, TimeUnit.MINUTES);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        } finally {
            unlock(lockKey);
        }
        return shop;

    }
```



> 逻辑过期时间

对热点key不设置过期时间，仅仅添加个expire 字段。

当使用该缓存时，根据过期字段判断是否更新缓存，若在期限内就直接使用改缓存，若不在期限内就更新缓存。

更新缓存的具体细节，利用锁确保一个线程重构缓存，防止数据库压力过大。在获得锁后，异步执行，新开线程执行重构缓存，同时原线程直接使用已经过期的数据。在此期间其他线程也发现缓存逻辑过期了，也会获得锁，但是获取锁失败，那就使用原来的老数据。

优点

* 性能高

缺点

* 数据不一致



代码实现

对类添加一个过期字段，为了满足开闭原则，可以自定义个新的类继承原来的类并添加expire字段，不过推荐如下写法

自定义个逻辑过期类，所有的逻辑过期类都可以使用（Object data 存原来的类）。

```java
/**
 * 逻辑过期类
 */
@Data
public class RedisData {
    private LocalDateTime expireTime;
    private Object data;
}
```

数据预热

```java
 /**
     * 添加逻辑过期时间
     * @param id
     * @param expireSeconds
     */
    public void savaShop2Redis(Long id ,Long expireSeconds){

        // 查询店铺数据
        Shop shop = getById(id);

        //封装逻辑过期时间
        RedisData redisData = new RedisData();
        redisData.setExpireTime(LocalDateTime.now().plusSeconds(expireSeconds));
        redisData.setData(shop);
        //写入redis
        stringRedisTemplate.opsForValue().set(CACHE_SHOP_KEY+id,JSONUtil.toJsonStr(redisData));
    }
```

正式代码

```java
private static final ExecutorService CACHE_REBUILD_EXECUTOR = Executors.newFixedThreadPool(10);
public Shop queryWithLogicalExpire( Long id ) {
    String key = CACHE_SHOP_KEY + id;
    // 1.从redis查询商铺缓存
    String json = stringRedisTemplate.opsForValue().get(key);
    // 2.判断是否存在
    if (StrUtil.isBlank(json)) {
        // 3.存在，直接返回
        return null;
    }
    // 4.命中，需要先把json反序列化为对象
    RedisData redisData = JSONUtil.toBean(json, RedisData.class);
    Shop shop = JSONUtil.toBean((JSONObject) redisData.getData(), Shop.class);
    LocalDateTime expireTime = redisData.getExpireTime();
    // 5.判断是否过期
    if(expireTime.isAfter(LocalDateTime.now())) {
        // 5.1.未过期，直接返回店铺信息
        return shop;
    }
    // 5.2.已过期，需要缓存重建
    // 6.缓存重建
    // 6.1.获取互斥锁
    String lockKey = LOCK_SHOP_KEY + id;
    boolean isLock = tryLock(lockKey);
    // 6.2.判断是否获取锁成功
    if (isLock){
        CACHE_REBUILD_EXECUTOR.submit( ()->{

            try{
                //重建缓存
                this.saveShop2Redis(id,20L);
            }catch (Exception e){
                throw new RuntimeException(e);
            }finally {
                unlock(lockKey);
            }
        });
    }
    // 6.4.返回过期的商铺信息
    return shop;
}
```



