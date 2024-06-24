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



