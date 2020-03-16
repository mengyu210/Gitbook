## 简单工厂模式 工厂方法模式 抽象工厂模式 的区别
   - 简单工厂模式，利用静态方法根据输入参数生成对应的产品，隐藏了产品实例化的细节。 违背了开放封闭原则。
   - 抽象工厂模式提供一个创建一系列相关或相互依赖对象的接口，而无需制定他们具体的类 实现了开发封闭原则
   - 工厂方法模式 将类的实例化（具体产品的创建）延迟到工厂类的子类（具体工厂）中完成，即由子类来决定应该实例化（创建）哪一个类。
## 单例模式
   - 懒汉模式
   - 饿汉模式
   - 双重锁模式
   - 静态内部类单例模式
   - 枚举单例模式

## 设计模式分类
   ### 创建模式
   - 工厂方法模式 Factory Method
   - 抽象工厂模式
   - 建造者模式 Builder
   - 原型模式 Prototype
   - 单例模式 Singleton
   ### 结构模式
   - 适配器模式
   - 桥接模式 Bridge
   - 过滤器模式
   - 组合模式  Composite
   - 装饰器模式 Decorator
   - 门面模式 / 外观模式（Facade）
   - 亨元模式 Flyweight
   - 代理模式 Proxy
   ### 行为模式
   - 解释器模式 Interpreter
   - 模板模式 Template Method
   - 责任链模式 Chain of Responsibility
   - 命令模式 Command
   - 迭代器模式 Iterator
   - 策略模式 strategy
   - 观察者模式 Visitor
   - 访问者模式 Visitor
   - 中介者模式 Mediator
   - 状态模式 State
   - 备忘录模式 Memento

## Spring中用了哪些设计模式？
   ### 1、 简单工厂模式
   又叫做静态工厂方法（StaticFactory Method）模式，但不属于23种GOF设计模式之一。 
   简单工厂模式的实质是由一个工厂类根据传入的参数，动态决定应该创建哪一个产品类。 
   <br>
   ### 2、工厂方法模式
   一般情况下,应用程序有自己的工厂对象来创建bean.如果将应用程序自己的工厂对象交给Spring管理,那么Spring管理的就不是普通的bean,而是工厂Bean。
   ### 3、单例模式
   保证一个类仅有一个实例，并提供一个访问它的全局访问点。
spring中的单例模式完成了后半句话，即提供了全局的访问点BeanFactory。但没有从构造器级别去控制单例，这是因为spring管理的是是任意的java对象。
核心提示点：Spring下默认的bean均为singleton，可以通过singleton=“true|false” 或者 scope="?"来指定。
   ### 4、适配器模式
   ### 5、包装器模式
   ### 6、代理模式
   spring的Proxy模式在aop中有体现，比如JdkDynamicAopProxy和Cglib2AopProxy。
   ### 7、观察者模式
   定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。
spring中Observer模式常用的地方是listener的实现。如ApplicationListener。
   ### 8、策略模式
   ### 9、模板方法模式