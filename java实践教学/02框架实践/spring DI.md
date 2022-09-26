## 一、注解基本信息



### 1.声明bean的注解

| Spring注解  | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| @Component  | 组件，没有明确的角色                                         |
| @Service    | 在业务逻辑层使用（service层）                                |
| @Repository | 标注在 DAO 类上，这是因为该注解的作用不只是将类识别为Bean，同时它还能将所标注的类中抛出的数据访问异常封装为 Spring 的数据访问异常类型。 Spring本身提供了一个丰富的并且是与具体的数据访问技术无关的数据访问异常结构，用于封装不同的持久层框架抛出的异常，使得异常独立于底层的框架 |
| @Controller | 定义文档的主体                                               |



### **2.注入bean的注解**

| Spring注解 | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| @Autowired | Spring提供的工具（由Spring的依赖注入工具（BeanPostProcessor、BeanFactoryPostProcessor）自动注入。）它可以对类成员变量、方法及构造函数进行标注，完成自动装配的工作。 通过 @Autowired的使用来消除 set ，get方法 |
| @Inject    | 由JSR-330提供                                                |
| @Resource  | 由JSR-250提供                                                |



### 3.**java配置类相关注解**

| Spring注解           | 描述                                                         |
| -------------------- | ------------------------------------------------------------ |
| @Configuration       | 声明当前类为配置类，相当于xml形式的Spring配置（类上）        |
| @Bean                | 注解在方法上，声明当前方法的返回值为一个bean，替代xml中的方式（方法上） |
| @Configuration       | 声明当前类为配置类，其中内部组合了@Component注解，表明这个类是一个bean（类上） |
| @ComponentScan       | 用于对Component进行扫描，相当于xml中的context:component-scan（类上） |
| @WishlyConfiguration | 为@Configuration与@ComponentScan的组合注解，可以替代这两个注解 |



### 4.**切面（AOP）相关注解**

| Spring注解 | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| @Aspect    | 声明一个切面（类上）,使用@After、@Before、@Around定义建言（advice），可直接将拦截规则（切点）作为参数。 |
| @After     | 在方法执行之后执行（方法上）                                 |
| @Before    | 在方法执行之前执行（方法上）                                 |
| @Around    | 在方法执行之前与之后执行（方法上）                           |
| @PointCut  | 声明切点, 在java配置类中使用@EnableAspectJAutoProxy注解开启Spring对AspectJ代理的支持（类上）https://www.cnblogs.com/liaojie970/p/7883687.html |



### 5.**@Bean的属性支持**

| Spring注解     | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| @Scope         | 设置Spring容器如何新建Bean实例（方法上，得有@Bean） 其设置类型包括：Singleton （单例,一个Spring容器中只有一个bean实例，默认模式）, Protetype （每次调用新建一个bean）, Request （web项目中，给每个http request新建一个bean）, Session （web项目中，给每个http session新建一个bean）, GlobalSession（给每一个 global http session新建一个Bean实例） |
| @StepScope     | 在Spring Batch中还有涉及                                     |
| @PostConstruct | 由JSR-250提供，在构造函数执行完之后执行，等价于xml配置文件中bean的initMethod |
| @PreDestory    | 由JSR-250提供，在Bean销毁之前执行，等价于xml配置文件中bean的destroyMethod |



### 6.@Value注解

@Value 为属性注入值（属性上）



### 7**.环境切换**

| Spring注解   | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| @Profile     | 通过设定Environment的ActiveProfiles来设定当前context需要使用的配置环境。（类或方法上） |
| @Conditional | Spring4中可以使用此注解定义条件话的bean，通过实现Condition接口，并重写matches方法，从而决定该bean是否被实例化。（方法上） |



### 8.异步相关

| Spring注解   | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| @EnableAsync | 配置类中，通过此注解开启对异步任务的支持，叙事性AsyncConfigurer接口（类上） |
| @Async       | 在实际执行的bean方法使用该注解来申明其是一个异步任务（方法上或类上所有的方法都将异步，需要@EnableAsync开启异步任务） |



### **9.定时任务相关**

| Spring注解        | 描述                                                         |
| ----------------- | ------------------------------------------------------------ |
| @EnableScheduling | 在配置类上使用，开启计划任务的支持（类上）                   |
| @Scheduled        | 来申明这是一个任务，包括cron,fixDelay,fixRate等类型（方法上，需先开启计划任务的支持） |



### **10.@Enable\*注解说明**

这些注解主要用来开启对xxx的支持。

| Spring注解                     | 描述                                             |
| ------------------------------ | ------------------------------------------------ |
| @EnableAspectJAutoProxy        | 开启对AspectJ自动代理的支持                      |
| @EnableAsync                   | 开启异步方法的支持                               |
| @EnableScheduling              | 开启计划任务的支持                               |
| @EnableWebMvc                  | 开启Web MVC的配置支持                            |
| @EnableConfigurationProperties | 开启对@ConfigurationProperties注解配置Bean的支持 |
| @EnableJpaRepositories         | 开启对SpringData JPA Repository的支持            |
| @EnableTransactionManagement   | 开启注解式事务的支持                             |
| @EnableCaching                 | 开启注解式的缓存支持                             |



### **11.测试相关注解**

| Spring注解            | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| @RunWith              | 运行器，Spring中通常用于对JUnit的支持                        |
| @ContextConfiguration | 用来加载配置ApplicationContext，其中classes属性用来加载配置类 |