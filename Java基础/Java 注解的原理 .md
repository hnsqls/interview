## Java 注解的原理

Java 注解（Annotation）是一种元数据，用于为代码提供额外的信息,不会直接影响代码的执行，但可以通过反射机制在运行时或编译时被读取和处理。

1. 注解的定义

注解通过 `@interface` 关键字定义，类似于接口的定义。注解可以包含元素（类似于方法），这些元素可以有默认值。

通过元注解来规定注解使用的范围。元注解是用于定义其他注解的注解。Java 提供了以下几种元注解：

+ **`@Retention`**：指定注解的保留策略。
  + `RetentionPolicy.SOURCE`：注解仅在源码中保留，编译时丢弃。
  + `RetentionPolicy.CLASS`：注解在编译时保留，但运行时不可见（默认策略）。
  + `RetentionPolicy.RUNTIME`：注解在运行时保留，可以通过反射读取。
+ **`@Target`**：指定注解可以应用的目标元素。
  + `ElementType.TYPE`：类、接口、枚举。
  + `ElementType.METHOD`：方法。
  + `ElementType.FIELD`：字段。
  + `ElementType.PARAMETER`：参数。
  + 其他目标元素。
+ **`@Documented`**：指定注解是否包含在 Javadoc 中。
+ **`@Inherited`**：指定注解是否可以被继承。
+ **`@Repeatable`**：指定注解是否可以重复使用。

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface MyAnnotation {
    String value() default "";
    int count() default 0;
}
```

2. 注解的使用

```java
@MyAnnotation(value = "example", count = 10)
public class MyClass {
    @MyAnnotation(count = 5)
    public void myMethod() {
        // 方法体
    }
}
```

3. 注解的处理机制

* 编译时处理

  在编译阶段，通过注解处理器（Annotation Processor）读取和处理注解。注解处理器可以生成新的代码、资源文件，或者对代码进行静态检查。

* 运行时处理

  在程序运行时，通过**反射机制**读取注解信息，并根据注解的内容执行相应的逻辑。

  使用 Java 反射 API（如 `Class`、`Method`、`Field` 等）获取注解信息。

  ```java
  Class<?> clazz = MyClass.class;
  MyAnnotation annotation = clazz.getAnnotation(MyAnnotation.class);
  if (annotation != null) {
      System.out.println("Value: " + annotation.value());
      System.out.println("Count: " + annotation.count());
  }
  ```

  应用场景

  + 框架中的依赖注入（如 Spring 的 `@Autowired`）。
  + 动态代理（如 JDK 动态代理）。

  

**元数据**（Metadata）是“关于数据的数据”，即描述其他数据的信息。

### 