# Java基础

## 进程与线程的区别

- 进程是操作系统分配资源的最小单位，线程是任务调度和程序执行的最小单位。
- 程序执行中至少存在一个进程，一个进程至少存在一个线程
- 系统在运行期间会为每个进程分配不同的内存空间，系统不会为线程分配内存，线程组所使用的资源来源与其所属进程的资源，线程组之间只能共享资源。


参考：
[进程和线程的主要区别](https://blog.csdn.net/kuangsonghan/article/details/80674777)

## 什么是值传递和引用传递

**Java的参数传递，不管是基本数据类型还是引用类型的参数，都是按值传递，没有按引用传递**

- 值传递：指的是在方法调用时，传递的参数是该值的拷贝传递，传递的是值的拷贝。
- 引用传递：指的是在方法调用时，传递的是引用的地址（即 变量所对应的内存空间的地址），也就是说传递前和传递后指向同一个引用。

## 基本数据类型与包装类型的区别

- 基本类型不能使用new关键字来创建，包装类型必须使用new关键字来创建。
- 使用场景不同。基本类型主要用于计算和赋值，包装类型用于泛型
- 初始值不同。基本类型int的初始值为0,boolean的初始值为false,包装类型的初始值都为null。
- 存储位置不同，基本类型存储在栈区，包装类型存储在堆区。
- 基本类型是值传递，包装类型是引用传递

参考：[基本数据类型及其包装数据类型](https://www.jianshu.com/p/cc44e2fd5b3a)

## 装箱和拆箱

- 装箱就是  自动将基本数据类型转换为包装器类型；
- 拆箱就是  自动将包装器类型转换为基本数据类型。

- 装箱过程是通过调用包装器的valueOf方法实现的，而拆箱过程是通过调用包装器的 xxxValue方法实现的。


参考：[深入剖析Java中的装箱和拆箱](https://www.cnblogs.com/dolphin0520/p/3780005.html)


## System.gc()和Runtime.gc()

- System.gc()和Runtime.gc()是没有任何区别的
<br>System.gc()的源码如下： 
```
    * <blockquote><pre>
     * Runtime.getRuntime().gc()
     * </pre></blockquote>
     *
     * @see     java.lang.Runtime#gc()
     */
    public static void gc() {
        Runtime.getRuntime().gc();
    }
``` 
- 他们用来提示JVM进行垃圾回收，但是是否立即回收还是延迟回收取决于JVM。

## 隐式类型转换与显式类型转换
- 隐式类型转换：由系统自动完成的类型转换。从存储范围小的类型到存储范围大的类型，由JVM自动完成
- 转换规则： 从存储范围小的类型到存储范围大的类型。
  <br>具体规则为： byte→short(char)→int→long→float→double
```
    short c = 1;
    c =c + 1;
    编译报错 （c+1）隐式转换 int 与等号左边基本类型不同
```

在整数之间进行类型转换时，数值不发生改变，**而将整数类型，特别是比较大的整数类型转换成小数类型时，由于存储方式不同，有可能存在数据精度的损失。**

- 显式类型转换：从存储范围大的类型到存储范围小的类型。该类类型转换很可能存在精度的损失。

- 转换规则： 从存储范围大的类型到存储范围小的类型。 
  <br>具体规则为： double→float→long→int→short(char)→byte 
```
 short c = 1;
 c +=1;            // 默认进行强制转换
 c = (short) (c + 1);  //int 转 short
```

注：
   - 强制类型转换通常都会存储精度的损失
   - 整数强制转换为整数时取数字的低位
   - 例如int类型的变量转换为byte类型时，则只去int类型的低8位(也就是最后一个字节)的值
```
        int d= 127;
        byte b= (byte) d;
        int d1= 157;
        byte b1= (byte) d1;
        ------------------
        b   127
        b1  -99
```

## 创建对象的4中方法
  - new 关键字直接创建
  - class 反射调用（使用class的newInstanse方法可以调用无参构造器创建对象）
```
try {
            Test test1 = Test.class.newInstance();
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }
```
  - 使用clone（）创建
```
 try {
            Test clone =(Test)test.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
```
  - 使用序列化（实现Serializable接口）
```
 Test test = new Test();
        File f = new File("test.obj");

        FileOutputStream fos = null;
        try {
            fos = new FileOutputStream(f);
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
        try {
            ObjectOutputStream oos = new ObjectOutputStream(fos);
            FileInputStream fis = new FileInputStream(f);
            ObjectInputStream ois = new ObjectInputStream(fis);

            oos.writeObject(test);
            Test test2 = null;
            try {
                test2 =(Test) ois.readObject();
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            }
            System.out.println(test2);

        } catch (IOException e) {
            e.printStackTrace();
        }
```
##  Collections.sort排序内部原理
   在Java 6中Arrays.sort()和Collections.sort()使用的是MergeSort（归并排序），而在Java 7中，内部实现换成了TimSort，其对对象间比较的实现要求更加严格 
##  String 和 StringBuilder 的区别
   - String不可变  每一次执行“+”都会新生成一个新对象，所以频繁改变字符串的情况中不用String，以节省内存
     StringBuilder 可变
   - String 和StringBuffer均线程安全，，StringBuilder并没有对方法进行加同步锁，所示是非线程安全的。
##  Vector 与 ArrayList 的区别
   - ArrayList在内存不足的情况下默认是扩展原容量的 0.5倍+1，Vector默认扩展一倍。
   - ArrayList默认大小10
   - Vector提供synchronized修饰方法，是线程安全版本的ArrayList

   - List 元素是有序的、可重复
   - ArrayList、Vector默认初始容量为10
   - Vector  线程安全，但速度慢
             底层数据结构是数组结构
             加载因子为1：即当 元素个数 超过 容量长度 时，进行扩容
			 扩容增量：原容量的 1倍
    - ArrayList 线程不安全，查询速度快
                底层数据结构是数组结构
				扩容增量：原容量的 0.5倍+1
	- Set(集) 元素无序的、不可重复。
	- HashSet  线程不安全，存取速度快
	           底层实现是一个HashMap（保存数据），实现Set接口
			   默认初始容量为16
			   加载因子为0.75：即当 元素个数 超过 容量长度的0.75倍 时，进行扩容
			   扩容增量：原容量的 1 倍
	- Map是一个双列集合
	- HashMap：默认初始容量为16
	           加载因子为0.75：即当 元素个数 超过 容量长度的0.75倍 时，进行扩容
	            扩容增量：原容量的 1 倍
##  TreeMap和TreeSet原理
    TreeMap底层是用红黑树来存储，每个Entry对应树的一个节点，TreeMap元素默认从小到大排序。V put(Key k, Value v)实质是二叉排序树的插入算法
## 	HashMap 与 Hashtable 的区别
    - Hashtable继承Dictonary类, HashMap继承自abstractMap
	- HashMap允许空的键值对, 但最多只有一个空对象，而HashTable不允许。
	- HashTable同步，而HashMap非同步，效率上比HashTable要高