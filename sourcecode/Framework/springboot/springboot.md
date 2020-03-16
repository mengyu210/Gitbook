# Springboot
## 什么是Spring boot，Spring boot 有什么特性?
   Spring Boot 是 Spring 开源组织下的子项目，是 Spring 组件一站式解决方案，主要是简化了使用 Spring 的难度，简省了繁重的配置，提供了各种启动器，开发者能快速上手。

## Spring Boot有哪些优点？
   
## Spring Boot 的配置文件有哪几种格式？它们有什么区别？
   v.properties 和 .yml，它们的区别主要是书写格式不同。

## Spring Boot 的核心注解是哪个？它主要由哪几个注解组成的？
   启动类上面的注解是@SpringBootApplication，它也是 Spring Boot 的核心注解 主要组合包含了以下 3 个注解：
   - @SpringBootConfiguration：组合了 @Configuration 注解，实现配置文件的功能。
   - @EnableAutoConfiguration：打开自动配置的功能，也可以关闭某个自动配置的选项，如关闭数据源自动配置功能： @SpringBootApplication(exclude = { DataSourceAutoConfiguration.class })。
   - @ComponentScan：Spring组件扫描。
## 开启 Spring Boot 特性有哪几种方式？
  - 继承spring-boot-starter-parent项目
  - 导入spring-boot-dependencies项目依赖
## Spring Boot 需要独立的容器运行吗？
   可以不需要，内置了 Tomcat/ Jetty 等容器。

## 运行 Spring Boot 有哪几种方式？
   - 打包用命令或者放到容器中运行
   - 用 Maven/ Gradle 插件运行
   - 直接执行 main 方法运行

## Spring Boot 自动配置原理是什么？
   注解 @EnableAutoConfiguration, @Configuration, @ConditionalOnClass 就是自动配置的核心，首先它得是一个配置文件，其次根据类路径下是否有这个类去自动配置。

## Spring Boot 2.X 有什么新特性？与 1.X 有什么区别？
  - 配置变更
  - JDK 版本升级
  - 第三方类库升级
  - 响应式 Spring 编程支持
  - HTTP/2 支持
  - 配置属性绑定
  -  更多改进与加强…
## 如何实现Spring Boot应用程序的安全性？
   为了实现Spring Boot的安全性，我们使用 spring-boot-starter-security依赖项，并且必须添加安全配置。它只需要很少的代码。配置类将必须扩展WebSecurityConfigurerAdapter并覆盖其方法。

## 什么是YAML？
   YAML是一种人类可读的数据序列化语言。它通常用于配置文件。 与属性文件相比，如果我们想要在配置文件中添加复杂的属性，YAML文件就更加结构化，而且更少混淆。可以看出YAML具有分层配置数据。

## Spring Boot 可以兼容老 Spring 项目吗，如何做？
   可以兼容，使用 @ImportResource 注解导入老 Spring 项目配置文件。

## 如何使用Spring Boot实现异常处理？
   Spring提供了一种使用ControllerAdvice处理异常的非常有用的方法。 我们通过实现一个ControlerAdvice类，来处理控制器类抛出的所有异常。

## 什么是Swagger？你用Spring Boot实现了它吗？
   Swagger广泛用于可视化API，使用Swagger UI为前端开发人员提供在线沙箱。Swagger是用于生成RESTful Web服务的可视化表示的工具，规范和完整框架实现。它使文档能够以与服务器相同的速度更新。当通过Swagger正确定义时，消费者可以使用最少量的实现逻辑来理解远程服务并与其进行交互。因此，Swagger消除了调用服务时的猜测。

## Spring Boot中的监视器是什么？
   Spring boot actuator是spring启动框架中的重要功能之一。Spring boot监视器可帮助您访问生产环境中正在运行的应用程序的当前状态。有几个指标必须在生产环境中进行检查和监控。即使一些外部应用程序可能正在使用这些服务来向相关人员触发警报消息。监视器模块公开了一组可直接作为HTTP URL访问的REST端点来检查状态。

## springboot自动配置的原理
   在spring程序main方法中 添加@SpringBootApplication或者@EnableAutoConfiguration
会自动去maven中读取每个starter中的spring.factories文件 该文件里配置了所有需要被创建spring容器中的bean

## Springboot 中application.yml和bootStrap.yml
   加载顺序
bootstrap.yml（bootstrap.properties）先加载
application.yml（application.properties）后加载
bootstrap.yml 用于应用程序上下文的引导阶段。
bootstrap.yml 由父Spring ApplicationContext加载。
父ApplicationContext 被加载到使用 application.yml 的之前。
<br>
配置区别
bootstrap.yml 和application.yml 都可以用来配置参数。
bootstrap.yml 可以理解成系统级别的一些参数配置，这些参数一般是不会变动的。
application.yml 可以用来定义应用级别的，如果搭配 spring-cloud-config 使用 application.yml 里面定义的文件可以实现动态替换。
使用Spring Cloud Config Server时，应在 bootstrap.yml 中指定：
- 1.spring.application.name
- 2.spring.cloud.config.server.git.uri
- 3.一些加密/解密信息

## Spring boot 和Spring cloud 的区别
  - Spring boot 是 Spring 的一套快速配置脚手架，可以基于spring boot 快速开发单个微服务；Spring Cloud是一个基于Spring Boot实现的云应用开发工具；
  - Spring boot专注于快速、方便集成的单个个体，Spring Cloud是关注全局的服务治理框架；
  - spring boot使用了默认大于配置的理念，很多集成方案已经帮你选择好了，能不配置就不配置，Spring Cloud很大的一部分是基于Spring boot来实现。
  - Spring boot可以离开Spring Cloud独立使用开发项目，但是Spring Cloud离不开Spring boot，属于依赖的关系。



