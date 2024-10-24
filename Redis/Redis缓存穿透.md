## 缓存穿透问题

访问数据库不存在的数据，会一直请求到数据库，被别有用心的人使用，可能会一直请求数据库，导致数据库宕机。解决方法有两

一：缓存空数据，二，使用布隆过滤器进行校验。

> 缓存空数据

在数据库查询到不存在的数据时，对该数据进行缓存为空（可以设置稍短的3~5分钟的TTL），之后相同的请求，就会在缓存中查到，而不去请求数据库。

代码案列

```java
  /**
     * 查询商户信息
     * @param id
     * @return
     */
    @Override
    public Result queryById(Long id) {
        //查询缓存
        String string = stringRedisTemplate.opsForValue().get(CACHE_SHOP_KEY+id);
        //hutool 工具类 符合条件“adc" 不符合条件“”，null, "/t/n"
        if (StrUtil.isNotBlank(string)){
            Shop shop = JSONUtil.toBean(string, Shop.class);
            return Result.ok(shop);
        }
        //若是 " " 上面已经判断了不是“” 不是null ,
        if(string != null){
            return Result.fail("商户不存在");
        }

        // 缓存不存在 查数据库
        Shop shop = getById(id);
        if (shop ==null) {
            //将空值写入缓存
            stringRedisTemplate.opsForValue().set(CACHE_SHOP_KEY + id, "", CACHE_NULL_TTL, TimeUnit.MINUTES);
            return Result.fail("商户不存在");
        }

        //写入缓存
        stringRedisTemplate.opsForValue().set(CACHE_SHOP_KEY+ id, JSONUtil.toJsonStr(shop), CACHE_SHOP_TTL, TimeUnit.MINUTES);

        return Result.ok(shop);
    }
```

优点

* 实现简单

缺点

* 缓存空值，会占用redis的内存空间（可以设置过期时间），可能会导致短期数据不一致问题（除非在新增数据的时候，删除redis中的数据）。





> 布隆过滤器







> 扩展

其实不仅仅是这两种解决缓存穿透的方案。

此外还可以

* 增强id的复杂性，避免id被猜到规律。
* 做好数据的基础格式校验
* 加强用户权限校验
* 做好热点数据限流。