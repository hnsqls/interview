## 什么是AOP,怎么用的？

AOP 是面向切面编程，是一种编程思想，Spring 的AOP是将与业务无关但影响多个对象的公共行为和逻辑的代码，抽取并封装为一个可重用的模块减少代码冗余，降低了模块间的耦合度，同时提高了系统的可维护性。

最常用的就是日志，缓存，事务。

先说一下日志，使用@ASpect定义切面，然后使用增强方法@Aroud编写增强方法,和切点表达式确定增强的位置。

切点表达式

1.**execution**：用于匹配方法的执行。常用于定义在哪些方法上应用切面。

```execution(* com.example.service.*.*(..))```

2.**@annotation**：用于匹配方法上存在特定注解的方法。

```java
@annotation(com.example.annotation.MyAnnotation)
```

spring的事务底层也是AOP。

