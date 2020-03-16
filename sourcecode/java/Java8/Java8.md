# Java 8 新特性
## 函数式接口
   ### 定义
   Java 8 引入的一个核心概念是函数式接口（Functional Interfaces）。通过在接口里面添加一个抽象方法，这些方法可以直接从接口中运行。如果一个接口定义个唯一一个抽象方法，那么这个接口就成为函数式接口。同时，引入了一个新的注解：@FunctionalInterface。可以把他它放在一个接口前，表示这个接口是一个函数式接口。这个注解是非必须的，只要接口只包含一个方法的接口，虚拟机会自动判断，不过最好在接口上使用注解 @FunctionalInterface 进行声明。在接口中添加了 @FunctionalInterface 的接口，只允许有一个抽象方法，否则编译器也会报错。
   ### 注意点
   - 接口中只能有一个接口方法（抽象）
   - 可以有静态方法和默认方法
   - 使用 @FunctionalInterface 标记（非必须，建议）
   - 默认方法可以被覆写
  
   Lambda表达式对比匿名内部类使用<br>
　　1. 简化了代码结构<br>
　　2. 节约了内存资源<br>
　　3. 让程序员更加关注，我要做什么，而不是为了做什么需要完成什么<br>
   ### 代码
```
@FunctionalInterface
public interface FuntionInterface {

    final static String dreamrain="梦见下雨";

    void seeSnow();

    default void add(){
        System.out.println("11");
    }

    static void  leave(){
        System.out.println("离去时"+dreamrain);
    }
}
```
```

```

## Lambda 表达式
 ### 定义
  Lambda 表达式 也可称为闭包
 函数式接口的重要属性是：我们能够使用 Lambda 实例化它们，Lambda 表达式让你能够将函数作为方法参数，或者将代码作为数据对待。Lambda 表达式的引入给开发者带来了不少优点：在 Java 8 之前，匿名内部类，监听器和事件处理器的使用都显得很冗长，代码可读性很差，Lambda 表达式的应用则使代码变得更加紧凑，可读性增强；Lambda 表达式使并行操作大集合变得很方便，可以充分发挥多核 CPU 的优势，更易于为多核处理器编写代码
 ### Lambda 表达式由三个部分组成
 - 第一部分为一个括号内用逗号分隔的形式参数，参数是函数式接口里面方法的参数；
 - 第二部分为一个箭头符号：->；
 - 第三部分为方法体，可以是表达式和代码块。
 
  #### Lambda表达式
  - 方法体为表达式，该表达式的值作为返回值返回 expression：单条语句表达式
```
 public String conver(Integer i){


        Function<Integer,String> function= (i) ->  String.valueOf(i);
        return function.apply(i);
    }

```
  - statement：语句块 方法体为代码块，必须用 {} 来包裹起来，且需要一个 return 返回值，但若函数式接口里面方法返回值是 void，则无需返回值。
```
无返回的
Consumer<String> consumer = s -> System.out.println(s);
        consumer.accept("下雪了");
有返回值的

 public String get(){
        Supplier<String> supplier = () ->  "7月初七";
       return  supplier.get();
    }


```
  - reference：方法引用 如果某个方法在结构上与lambda表达式中对应方法是匹配的，那么就可以直接引用给lambda表达式。其总共包含4种引用类型

|类型	|语法	|
|--	|--	|
|基于实例方法引用	|	object::methodName	|
|构造方法引用	|className::new	|
|基于参数实例方法引用	|className::methodName	|
|静态方法引用	|className::staticMethodName	|
```

```


   

  #### lambda表达式常见应用场景
  - 对象排序
  - 动态代理
  - 事件监听
  - 条件过滤
  - 启动线程




## 全球化功能
  ava 8 版本还完善了全球化功能：支持新的 Unicode 6.2.0 标准，新增了日历和本地化的 API，改进了日期时间的管理等。
```
LocalDate now = LocalDate.now();  //获取当前时间
now= LocalDate.of(2020,Month.JULY,21); //获取指定天数的日期
LocalTime now1 = LocalTime.now(); //获取当前时间点
 now1= LocalTime.of(10,10,50);// 指定时间
 Clock clock = Clock.systemDefaultZone();//获取系统默认时区 
        long millis = clock.millis();// (当前瞬时时间 )
```

## 集合之流式操作
## 接口的增强
## 安全性
   支持更强的基于密码的加密算法。基于 AES 的加密算法，例如 PBEWithSHA256AndAES_128 和 PBEWithSHA512AndAES_256，已经被加入进来。
## IO/NIO 的改进
   Java 8 对 IO/NIO 也做了一些改进。主要包括：改进了 java.nio.charset.Charset 的实现，使编码和解码的效率得以提升，也精简了 jre/lib/charsets.jar 包；优化了 String(byte[],*) 构造方法和 String.getBytes() 方法的性能；还增加了一些新的 IO/NIO 方法，使用这些方法可以从文件或者输入流中获取流（java.util.stream.Stream），通过对流的操作，可以简化文本行处理、目录遍历和文件查找。
   - BufferedReader.line(): 返回文本行的流 Stream<String>
   - File.lines(Path, Charset):返回文本行的流 Stream<String>
   - File.list(Path): 遍历当前目录下的文件和目录
   - File.walk(Path, int, FileVisitOption): 遍历某一个目录下的所有文件和指定深度的子目录
   - File.find(Path, int, BiPredicate, FileVisitOption... ): 查找相应的文件
```


```
## 注解的更新
   Java 8 主要有两点改进：类型注解和重复注解。
   Java 8 的类型注解扩展了注解使用的范围。在该版本之前，注解只能是在声明的地方使用。现在几乎可以为任何东西添加注解：局部变量、类与接口，就连方法的异常也能添加注解。新增的两个注释的程序元素类型 ElementType.TYPE_USE 和 ElementType.TYPE_PARAMETER 用来描述注解的新场合。ElementType.TYPE_PARAMETER 表示该注解能写在类型变量的声明语句中。而 ElementType.TYPE_USE 表示该注解能写在使用类型的任何语句中（例如声明语句、泛型和强制转换语句中的类型）。