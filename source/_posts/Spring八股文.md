---
title: Spring八股文
date: 2024-06-22 18:11:04
tags:
  - Java
  - Spring
  - 八股文
categories: Java
cover: /img/springbaguwen.png
---

## Spring 常见面试题总结

### 什么是Spring框架？

Spring 是一款开源的轻量级Java开发框架，提高开发人员的开发效率以及系统的维护性。



### 谈谈自己对于Spring IoC的了解

**IoC（Inversion of Control 控制反转）**是一种设计思想，而不是一个具体的技术实现。

IoC 的思想就是将原本在程序中手动创建对象的控制权，交给 Spring 框架来管理。



#### 为什么叫控制反转？

- **控制**：指的是对象创建（实例化、管理）的权利
- **反转**：控制权交给外部环境（Spring框架、IoC容器）

将对象之间的相互依赖关系交给 IoC 容器来管理，并由 IoC 容器完成对象的注入。这样可以很大程度上简化应用开发，把应用从复杂的依赖关系中解放出来。IoC 容器就像是一个工厂一样，当我们需要创建一个对象的时候，只需要配置好配置文件或注解即可，完全不用考虑对象是如何被创建出来的。

在 Spring 中，IoC 容器是Spring用来实现 IoC 的载体，IoC 容器实际上就是个Map。

Spring 框架一般通过XML文件来配置 Bean,因为 XML 文件配置很麻烦，然后就开始用 SpringBoot 注解配置。



### 什么是 Bean？

Bean 就是哪些被 IoC 容器所管理的对象。



### @Component和@Bean的区别是什么？

- @Component 注解用于类，@Bean 注解用于方法。
- @Component 是通过类路径扫描来自动侦测以及自动装配到 Spring 容器中(我们可以使用 @ComponentScan 注解定义要扫描的路径从中找出标识了需要装配的类自动装配到 Spring 的 IoC 容器中)。@Bean 注解通常是我们在标有该注解的方法中定义产生这个 bean，@Bean 注解告诉类 Spring 这是某个类的实例，当我需要的时候给我。
- @Bean 注解比 @Component 注解的自定义性更强，而且很多地方只能通过 @Bean 注解来实现。比如当我们引用第三方库中的类需要装配到 Spring 容器时，就只能通过 @Bean 来实现。



### 注入Bean的注解有哪些？

Spring 内置的Autowired 还有 JDK 内置的 @Resource 和 @Inject 都可以用于注入 Bean。



### @Autowired和@Resource的区别是什么？

- @Autowired 是 Spring 提供的注解，@Resource 是 JDK 提供的注解。
- 当一个接口存在多个实现类的情况下，@Autowired 和 @Resource 都需要通过名称才能正确匹配到对应的  Bean。Autowired 可以通过 @Qualifier 注解来显式指定名称，@Resource 可以通过 name 属性来显式指定名称。
- @Autowired 支持在构造函数、方法、字段和参数上使用。@Resource 主要用于字段和方法上的注入，不支持在构造函数或参数上使用。



### Bean 的作用域有哪些?

- **Singleton** Srping 中的bean 默认都是单例的，在整个 IoC 容器中只创建一个 bean 实例，无论多少次请求该bean 都返回同一个实例。
- **prototype**  每次请求都会创建一个新的实例。
- **Request** 每次 Http 请求都会创建一个新的 bean 实例。
- **Session** 每个 Http Session 中会有一个 bean 实例。



### Bean 是线程安全的吗？

在 Spring 中 Bean 是否安全，取决于其作用域和状态。

单例 Bean 在 IoC 中只创建一个实例，这个实例会被多个线程共享。所以单例 Bean 不是线程安全的，如果考虑线程安全问题，可以使用同步（synchronization）或者无状态（stateless）设计。原型 Bean 因为每次请求都会创建新的实例，所以原型作用域的 Bean 是线程安全的。



### Bean的生命周期了解么？

Todo



### 谈谈对于AOP的了解

AOP（面向切面编程）能够将哪些与业务无关，却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的扩展性和可维护性。

Spring AOP就是基于动态代理的，如果代理的对象，实现了某个接口，那么Spring AOP会使用JDK Proxy去创建代理对象，而对于没有实现接口的对象，就无法使用JDK Proxy去进行代理，这时候Spring AOP会使用Cglib生成一个被代理对象的子类作为代理



### Spring AOP 和 AspectJ AOP 有什么区别？

Spring AOP属于运行时增强，而AspectJ是编译时增强。

Spring AOP基于代理，而AspectJ基于字节码操作。

Spring AOP已经集成了AspectJ，AspectJ相对于Spring AOP功能更加强大，但是Spring AOP相对来说更简单，如果我们的切面比较少，那么两者性能差异不大。当切面太多的时候，最好选择AspectJ，它比Spring AOP快很多



### AspectJ 定义的通知类型有哪些？

- **Before**（前置通知）：目标对象的方法调用之前触发

- **After**（后置通知）：目标对象的方法调用之后触发

- **AfterReturning**（返回通知）：目标对象的方法调用完成，在返回结果值之后触发

- **AfterThorowing**（异常通知）：目标对象的方法运行中抛出/触发异常后触发。AfterReturning 和 AfterThrowing 两者互斥。如果方法调用成功无异常，则会返回值；如果方法抛出了异常，则不会有返回值。

- **Around**（环绕通知）：编程式控制目标对象调用。环绕通知是所有通知类型中可操作范围最大的一种，因为它可以直接拿到目标对象，以及要执行的方法，所以环绕通知可以任意的在目标对象的方法调用前后触发，可以不需要调用目标对象的方法

  

### 多个切面执行顺序如何控制？

1. 通常使用`@Order` 注解直接定义切面顺序
2. 实现`Ordered`接口重写`getOrder`方法



### 说说自己对于 Spring MVC 了解？

MVC 是模型、视图、控制器的简写，其核心思想是通过将业务逻辑、数据、显示分离来组织代码。MVC 是一种设计模式，Spring MVC 是一款优秀的 MVC框架。Spring MVC 可以帮助我们简化 Web 层的开发，并且可以与 Spring 框架集成。我们一般把后端项目分为 Service 层（处理业务）、Dao层（数据库操作）、Entity 层（实体类）、Controller层（控制层，返回数据给前台页面）。





### 统一异常处理怎么做？

使用注解的方式统一异常处理，使用`@controllerAdvice`和`@ExceptionHandler`这两个注解。



### Spring 中用到了哪些设计模式？

- **工厂设计模式**：Spring 使用工厂模式通过`BeanFactory`、`ApplicationContext`创建 bean 对象。
- **代理设计模式**：SpringAOP 功能实现。
- **单例设计模式**：Spring 中的 Bean 默认都是单例的。
- **模版方法模式**：Spring 中`jdbcTemplate`、`hibernateTemplate`等以 Template 结尾的对数据库操作的类，都使用到了模版模式。
- **包装器设计模式**：项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们刚开业根据客户的需求能够动态切换不同的数据。
- **观察者模式**：Spring 事件驱动模型就是观察者模式。
- **适配器模式**：Spring AOP 的增强或通知使用到了适配器模式、SpringMVC 中也用到了适配器模式适配`Controller`。



### Spring 循环依赖了解吗，怎么解决？

Spring 中的循环依赖就是指两个及以上的 Bean互相依赖，形成一个循环。比如 BeanA 依赖于 BenaB，而 BeanB 又依赖 BeanA。这种情况可能导致 Spring 容器无法正确初始化这些 Bean。

#### 使用 setter 注入

Spring 中可以通过 Setter 注入解决循环依赖问题，因为Spring 在注入 Bean 使得属性时会先创建 Bean 的实例，然后再注入依赖的树形。这样可以保证即使两个 Bean 互相依赖，Spring 也能正确的初始化它们。

#### 使用 @Lazy 注解

在其中一个 Bean 的依赖上使用`@Lazy`注解，这样 Spring 容器在首次访问该 Bean 时才会进行初始化，避免循环依赖问题。



### Spring 管理事务的方式有几种？

- **编程式事务**：在代码中硬编码（在分布式系统中推荐使用）：通过`TranscationTemplate`或者`TransactionManager`在手动管理事务，事务范围过大会出现事务未提交导致超时，因此事务要比锁的粒度更小。
- **声明式事务**：在 XML 配置文件中配置或者直接基于注解（单体应用或者简单业务系统推荐使用）：实际时通过 AOP 实现（基于`@Transcational`的全注解方式使用最多）



### Srping事务中哪几种事务传播行为？

> 事务传播行为是为了解决业务层方法之间互相调用的事务问题。
>
> 当事务方法被另一个事务方法调用时，必须制定事务应该如何传播。
>
> 例如：方法可能继续在现有的事务中运行，也可能开启一个新事物，并在自己的事物中运行。

1. **`TransactionDefinition.PROPAGATION_REQUIRED`**：如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事物。



### 什么是 SpringBoot？为什么要有 SpringBoot？

SpringBoot 可以简化 Spring 开发（减少配置文件、开箱即用 ）



### 如何在 SpringBoot 应用程序中使用 Jetty 而不是 Tomcat？

SpringBoot（spirng-boot-starter-web）使用Tomcat 作为默认的嵌入式 servlet 容器，如果想使用 Jetty 的话，只需要修改 pom.xml （Maven）文件就可以了。





### 介绍一下 @SpringBootApplication 注解

`@SpringBootApplication`可以看作是`@Configuration、@EnableAutoConfiguration、@ComponentScan`注解的集合。

- `@Configuration`：运行在上下文中注册额外的`bean`或导入其他配置类
- `@EnableAutoConfiguration`：启用 Springboot 的自动配置机制
- `@ComponentScan`：扫描被`@Component`(`@Service`,`@Controller`)注解的 bean，默认会扫描该类所在的包下的所有的类。



### SpringBoot 的自动配置是如何实现的？

当 SpringBoot 应用启动时，@EnableAutoConfiguration`注解会触发自动配置类的加载。

SpringBoot 会扫描`spring.factories`文件，并加载列出自动配置类。然后 SpringBoot 会根据条件注解的判断，决定是否进行相应的配置。



### SpringBoot 常用的两种配置文件

我们可以通过`applcation.properties`或者`application.yml`对SpringBoot 程序进行简单的配置。如果不进行配置的话，就是使用默认配置。



### 什么是 YAML ? YAML配置的优势在哪里？

YAML 是一种人类可读的数据序列化语言。它通常用于配置文件。与属性文件相比，如果我们想要在配置文件中添加复杂的树形，YAML 文件就更加结构化，而且更少混淆。可以看出 YAML 具有分层配置数据。

相比 Properties 配置文件，YAML 配置方式更加直观清晰，简洁明了，有层次感。

但是 YAML 配置文件不支持`@PropertySource` 注解导入自定义的 YAML 配置。



### SpringBoot 常用的读取配置文件的方法有哪些？

1. 通过`@Value("${property}")`读取比较简单的配置信息

   > `@value` 这种方式是不被推荐的

2. 通过`@ConfigurationProperties`读取并与 bean 绑定

3. 通过`@ConfigurationProperies`读取并校验

4. 通过`@PropertySource`读取指定的 properties 文件



### SpringBoot 如何做请求参数校验？

使用 JSR 提供的校验注解



### SpringBoot 如何监控系统运行状态？

可以使用 SpringBoot Actuator 来对 SpringBoot 项目进行简单的监控。





### SpringBoot 中如何实现定时任务？

我们使用`@Scheduled`注解就能很方便地创建一个定时任务。

还需要再 SpringBoot 启动类上添加`@EnableScheduling`注解，这个注解的作用是发现`@Scheduled`的任务并在后台执行该任务。

