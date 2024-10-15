## Spring SpringMvc Springboot常用注解

> Spring 常用注解

第一类是：声明bean，有@Component、@Service、@Repository、@Controller

第二类是：依赖注入相关的，有@Autowired、@Qualifier、@Resourse（java的注解）

第三类是：设置作用域 @Scope

第四类是：spring配置相关的，比如@Configuration，@ComponentScan 和 @Bean 

第五类是：跟aop相关做增强的注解  @Aspect，@Before，@After，@Around，@Pointcut

> SpringMVc 常用注解

有@RequestMapping：用于映射请求路径；还有像@PostMapping、@GetMapping这些。

@RequestBody：注解实现接收http请求的json数据，将json转换为java对象；

@RequestParam：指定请求参数的名称；

@PathViriable：从请求路径下中获取请求参数(/user/{id})，传递给方法的形式参数；

@ResponseBody：注解实现将controller方法返回对象转化为json对象响应给客户端。

@RequestHeader：获取指定的请求头数据



> Spring Boot 常用注解

Spring Boot的核心注解是@SpringBootApplication , 他由几个注解组成 : 

- @SpringBootConfiguration： 组合了- @Configuration注解，实现配置文件的功能；
- @EnableAutoConfiguration：打开自动配置的功能，也可以关闭某个自动配置的选项
- @ComponentScan：Spring组件扫描