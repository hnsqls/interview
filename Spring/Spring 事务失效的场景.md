## Spring事务失效的场景

Spring 的支持的事务，包括编程式事务和声明式事务。但是一般都是使用声明式事务。

在需要使用事务的方法上添加`@Transactional`。

事务失效的情况

1. 异常信息被捕获。

原因 ：事务通知只有捉到了目标抛出的异常，才能进行后续的回滚处理，如果目标自己处理掉异常，事务通知无法知悉。

解决：在catch块添加throw new RuntimeException(e)抛出。

2. 抛出检查异常

原因：Spring 默认只会回滚非检查异常。

解决： 添加属性`@Transactional(rollbackFor=Exception.class)`

3. 非pubulic方法。

原因：spring事务的实现底层是代理， Spring 为方法创建代理、添加事务通知、前提条件都是该方法是 public 的。

解决：改为 public 方法

4. 方法内调用。

原因：spring事务底层是代理实现。方法内调用不走代理。

解决方法：不直接调用，而是获取当前的代理对象，通过代理调用方法。

