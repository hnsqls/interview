## Springbean 是线程安全的吗？

答：不是线程安全的。
Spring框架中有一个@Scope注解，默认的值就是singleton，单例的。
因为一般在spring的bean的中都是注入无状态的对象（比如service,dao类），没有线程安全问题，如果在bean中定义了可修改的成员变量，是要考虑线程安全问题的，可以使用多例或者加锁来解决。