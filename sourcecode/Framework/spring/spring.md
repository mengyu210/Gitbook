## Spring


### 1.1、事务？及事务的4个属性
      事务是：把一系列动作当成一个独立的工作单元，这些工作要么全部完成，要么全部不起作用
      ACID ，原子性(atomicity)，一致性(consistency)，隔离性(isolation)，持久性(durability)

### 1.2、Spring 管理事务的方式有几种？
    - 1编程式事务，在代码中编程。（不推荐使用）
```
transactionManager.commit(status);
sqlSession.commit();
```
    - 2声明式事务，在配置文件中配置。（推荐使用）

  声明事务又分为两种：<br/>
    - 1基于XML的声明式事务
    - 2基于注解的声明式事务
### 1.3、将一个类声明为Spring的Bean的注解有哪些？
    - @Controller ：对应 Spring MVC 控制层，主要用户接受用户请求并调用Service层返回数据给前端页面
    - @Service ：对应服务层，主要涉及一些复杂的逻辑，需要用到dao层
    - @Repository 对应持久化层dao层，主要用于数据库的操作
    - @Component 用的注解，可标注任意类为 Spring 组件。如果一个Bean不知道属于拿个层，可以使用@Component 注解标注

### 1.4、Spring框架用到哪些设计模式？
    - 工厂设计模式：Spring使用工厂模式通过BeanFactory、ApplicationContext创建Bean对象。
    - 代理设计模式：springAOP功能的实现
    - Spring中的Bean默认都是单例的
    - 模板方法模式：Spring中的jdbcTemplate、hibernateTemplate等以Template的对数据库操作的类，它们使用到了模板模式。
    - 观察者模式：spring事件驱动模型就是观察者模式的一个经典应用。
### 1.5、Spring中的bean的作用域有哪些？
    - singleton：唯一bean实例,Spring的bean 默认都是单例的
    - prototype：每次请求都会创建一个新的bean实例。
    - request: 每一次HTTP请求都会产生一个新的bean，该bean仅在当前HTTP request内有效。
    - session：每一次HTTP请求都会产生一个新的 bean，该bean仅在当前 HTTP session 内有效。
