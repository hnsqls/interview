## Spring的bean的生命周期

嗯！，这个步骤还是挺多的，我之前看过一些源码，它大概流程是这样的。

首先Spring容器在进行实例化时，会将xml配置的<bean>的信息封装成一个BeanDefinition对象，Spring根据BeanDefinition来创建Bean对象，里面有很多的属性用来描述Bean叫做BeanDefinition获取bean的定义信息，这里面就封装了bean的所有信息，比如，类的全路径，是否是延迟加载，是否是单例等等这些信息。

在创建bean的时候，第一步是调用构造函数实例化bean。

第二步是bean的依赖注入，比如一些set方法注入，像平时开发用的@Autowire都是这一步完成。

第三步是处理Aware接口，如果某一个bean实现了Aware接口就会重写方法执行。

第四步是bean的后置处理器BeanPostProcessor，这个是前置处理器。

第五步是初始化方法，比如实现了接口InitializingBean或者自定义了方法init-method标签或@PostContruct。

第六步是执行了bean的后置处理器BeanPostProcessor，主要是对bean进行增强，有可能在这里产生代理对象。

最后一步是销毁bean。

