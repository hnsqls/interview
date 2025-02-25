## Session和Cookie

当tomcat，第一次接收到客户端的请求，会开辟一个session空间，建立一个session对象，表明这一次会话，同时生成session id,通过响应头的方式保存在客户端浏览器的Cookie中，以后客户端的每次请求都会在请求头带上sessionid,可以根据session id 对应服务端保存的session对象里一些相关信息，比如说用户的登录信息。

当应用，从单体到集群，分布式架构下，Cookie + session 的这种机制怎么扩展？

1. session黏贴：在集群模式下，通过一个机制（nginx IP轮询）保证同一客户端的所有请求都会转发到tomcat实例中。问题：该实列挂了被转发到其他实例还要重新登录。
2. session 复制：当一个tomcat上存了session信息，主动复制到集群中的其他实例。问题：复制需要时间，并且浪费空间。
3. session 共享：将服务端的session信息保存在第三方中，比如redis   



Session 登录的demo

```java
Result login(LoginFormDTO loginForm, HttpSession session);

 /**
     * 登录或注册用户
     * @param loginForm
     * @param session
     * @return
     */
    @Override
    public Result login(LoginFormDTO loginForm, HttpSession session) {
        //校验验证码
        String cacheCode = session.getAttribute("code").toString();
        String code = loginForm.getCode();
        if (!code.equals(cacheCode)){
            //验证码不相同
            return Result.fail("验证码错误");
        }

        //根据手机号判断用户是否存在
        LambdaQueryWrapper<User> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(User::getPhone,loginForm.getPhone());
        User user = super.getOne(queryWrapper);
        if (user ==null){
            //用户不存在，注册用户，填上基本信息,保存
            User user1 = new User();
            user1.setPhone(loginForm.getPhone());
            user1.setNickName("user_"+RandomUtil.randomString(4));
            user = user1;
            //保存到数据库中
            super.save(user1);
        }
        //用户存在就保存在session
        session.setAttribute("user",user);

        return Result.ok() ;
    }
```

分布式情况下 session共享

```java
      /**
     * 登录或注册用户
     * @param loginForm
     * @param session
     * @return
     */
    @Override
    public Result login(LoginFormDTO loginForm, HttpSession session) {
        //校验验证码
//        String cacheCode = session.getAttribute("code").toString();
        String cacheCode = redisTemplate.opsForValue().get(RedisConstants.LOGIN_CODE_KEY + loginForm.getPhone());
        String code = loginForm.getCode();
        if (!code.equals(cacheCode)){
            //验证码不相同
            return Result.fail("验证码错误");
        }

        //根据手机号判断用户是否存在
        LambdaQueryWrapper<User> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(User::getPhone,loginForm.getPhone());
        User user = super.getOne(queryWrapper);
        if (user ==null){
            //用户不存在，注册用户，填上基本信息,保存
            User user1 = new User();
            user1.setPhone(loginForm.getPhone());
            user1.setNickName("user_"+RandomUtil.randomString(4));
            user = user1;
            //保存到数据库中
            super.save(user1);
        }
        //用户存在就保存在session
        //session.setAttribute("user",user);

        //用户信息保存在redis中  以随机token为k,用户信息为v
        String token = UUID.randomUUID().toString(true);
        //将对象转为hash类型
        Map<String, Object> usermap = BeanUtil.beanToMap(user,new HashMap<>(),
                CopyOptions.create()
                        .setIgnoreNullValue(true)
                        .setFieldValueEditor((fieldName,fieldValue) -> fieldValue.toString()));
        //存在redis中
        redisTemplate.opsForHash().putAll(RedisConstants.LOGIN_USER_KEY+token,usermap);
        //设置token有效期
        redisTemplate.expire(RedisConstants.LOGIN_USER_KEY +token,12,TimeUnit.HOURS);

        //返回token
        return Result.ok(token) ;
    }
```



