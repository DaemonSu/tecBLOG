# 使用spring mvc进行路径映射

在开始介绍spring mvc路径映射之前，我们需要介绍一下spring framework的相关知识。相关文档

[spring boot文档]: 

首先，我们整个系统是基于spring boot进行系统配置的。而spring boot又是基于spring framework进行开发的。spring framework这个模块呢，主要包含了两大特性：**控制翻转（ioc  inverse of control）  依赖注入（DI  dependency injection ）**和**面向切面编程（AOP）**  oo



spring  

​	framework  bean 管理，容器

​	mvc framework中的一部分，路径的映射

​	security 安全方面的，系统安全，登录，认证，授权，登出，xss攻击，csrf

​	boot 自动化的配置尽量减少配置

​	data 与数据交互相关，数据库中的数据，映射成java对象  hibernate  mybatis

​	webflow 简易的工作流框架  jbpm  activity

## 控制翻转说明



​	IoC 是一种通过描述来生成或者获取对象的技术，而这个技术不是 Spring 甚至不是 Java 独有的。对于 Java 初学者更多的时候所熟悉的是使用 new 关键字来创建对象，而在 Spring 中则不是，它是通主要是通过描述来创建对象。只是 Spring Boot 并不建议使用 XML，而是通过注解的描述生成对象，所以本章通过注解来介绍 Spring IoC 技术。一个系统可以生成各种对象，井且这些对象都需要进行管理。  还值得一提的是，对象之间并不是孤立的，它们之间还可能存在依赖的关系。例如， 一个班级是由多个老师和同学组成的，那么班级就依赖于多个老师和同学了。为此 Spring 还提供了依赖注入的功能，使得我们能够通过描述来管理各个对象之间的关系。
​	为了描述上述的班级、同学和老师这 3 个对象关系，我们需要一个容器。  在 Spring 中把每一个需要管理的对象称为Spring bean。而 Spring 管理这些 Bean 的容器，被我们称为 Spring IOC容器。

- 描述管理 Bean ，包括发布和获取 Bean;
- 描述完成 Bean 之间的依赖关系。

### 传统上构建对象

```java
package com.jlai.ioc.bean;

/**
 * 地址
 */
public class Address {

    String addressName;
    int postCode;

    public String getAddressName() {
        return addressName;
    }

    public void setAddressName(String addressName) {
        this.addressName = addressName;
    }

    public int getPostCode() {
        return postCode;
    }

    public void setPostCode(int postCode) {
        this.postCode = postCode;
    }
}
```



```java
package com.jlai.ioc.bean;

public class User {
    
    String name;
    String sex;
    int age;
    Address address;

 //getter 和setter

}
```

```java
public class Test {
    public static void main(String[] args){

        Address address=new Address();
        address.setAddressName("吉林省长春市");
        address.setPostCode(130001);

        User user=new User();

        user.setName("张三");
        user.setAge(18);
        user.setSex("male");
        
        user.setAddress(address);

        System.out.println(user.getAddress().getAddressName());
    }
}
```



从上面的代码中，我们可以看出来，整个User对象的创建需要经历下面两个过程：

1：通过手动new 创建出对象

2：通过set方法，配置对象的属性值和对象之间的依赖关系。



### Spring中构建对象的



```java
@Component("address")
public class Address {

    @Value("长春市")
    String addressName;
    @Value("130022")
    int postCode;
    
   //对应的getter setter方法 
}
```



```java
@Component("user")
public class User {

    @Value("李四")
    String name;
    @Value("female")
    String sex;
    @Value("19")
    int age;

    @Autowired
    Address address;

 //对应的getter和setter方法

}
```

这里，spring进行对象之间关系是通过注解来配置对象之间的关系的。

在java开发中，数据的类型要么是种基本的数据类型，要么就是对象类型的。

int long char  double float boolean short   byte  +String  采用value 注解

对象采用 @Autowired 注入

对于基本数据类型和字符串类型的数据，直接使用value注解即可。对于其他对象类型的数据，则需要采用 @Autowired 注解，那么，系统会自动的寻找该类型对象并进行设置属性。

扫描配置项

```java
@Configuration
@ComponentScan(basePackages = "com.jlai.*")
public  class  AppConfig  {
}
```

程序的运行

```java
public static void  test2(){
    ApplicationContext ctx=new AnnotationConfigApplicationContext(AppConfig.class);
        User  user=  ctx.getBean(User.class);
    System.out.println(user.getAddress().getAddressName());
}
```

有了对象之间的关系，就可以构造出对象了。那扫描是干什么用的呢，默认情况下，扫描配置本包以及子包下的所有的对象关系。那么通过basepackages配置，可以更改扫描的范围。



那么关系都有了，构建直接从容器中获取即可。

 User  user=  ctx.getBean(User.class); 

就是从系统容器中获得一个User类型的数据。

### 关于DI的其他的知识-scope

当你要获得一个对象的时候，那么这个对象是新创建的对象还是原来的那个对象呢？



- [singleton](https://so.csdn.net/so/search?q=singleton&spm=1001.2101.3001.7020) 表示在spring容器中的单例，通过spring容器获得该bean时总是返回唯一的实例
- prototype表示每次获得bean都会生成一个新的对象
- request表示在一次http请求内有效（只适用于web应用）
- [session](https://so.csdn.net/so/search?q=session&spm=1001.2101.3001.7020)表示在一个用户会话内有效（只适用于web应用）
- globalSession表示在全局会话内有效（只适用于web应用）

 在多数情况，我们只会使用singleton和prototype两种scope，如果在spring配置文件内未指定scope属性，默认为singleton。



## spring mvc

Spring Web MVC框架（通常简称为"Spring MVC"）是一个富"模型，视图，控制器"的web框架。 Spring MVC允许你创建特
定的@Controller或@RestController beans来处理传入的HTTP请求。 使用@RequestMapping注解可以将控制器中的方法映
射到相应的HTTP请求

```java
@RestController
@RequestMapping(value="/users")
public class MyRestController {
    @RequestMapping(value="/{user}", method=RequestMethod.GET)
    public User getUser(@PathVariable Long user) {
        // ...
    }
    @RequestMapping(value="/{user}/customers", method=RequestMethod.GET)
    List<Customer> getUserCustomers(@PathVariable Long user) {
        // ...
    }
    @RequestMapping(value="/{user}", method=RequestMethod.DELETE)
    public User deleteUser(@PathVariable Long user) {
        // ...
    }
}
```

