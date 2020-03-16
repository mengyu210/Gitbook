## 为什么程序计数器、虚拟机栈和本地方法栈是线程私有的呢？为什么堆和方法区是线程共享的呢？
### 程序计数器为什么是私有的？<br>
    - 程序计数器主要有下面两个作用：<br>
	  字节码解释器通过改变程序计数器来依次读取指令，从而实现代码的流程控制，如：顺序执行、选择、循环、异常处理。<br>
	  在多线程的情况下，程序计数器用于记录当前线程执行的位置，从而当线程被切换回来的时候能够知道该线程上次运行到哪儿了。<br>
	**需要注意的是，如果执行的是 native 方法，那么程序计数器记录的是 undefined 地址，只有执行的是 Java 代码时程序计数器记录的才是下一条指令的地址。**
	
	程序计数器私有主要是为了线程切换后能恢复到正确的执行位置。<br>
	
### 虚拟机栈和本地方法栈为什么是私有的?  
	- 虚拟机栈： 每个Java方法在执行的同时会创建一个栈帧用于存储局部变量表，操作数栈，常量池引用等信息。从方法调用直至执行完成的过程，就对应着一个栈帧在Java虚拟机栈中入栈和出栈的过程。
	- 本地方法栈： 和虚拟机栈所发挥的作用非常相似，区别是：虚拟机栈为虚拟机执行 Java 方法 （也就是字节码）服务，而本地方法栈则为虚拟机使用到的 Native 方法服务。
	所以，为了保证线程中的局部变量不被别的线程访问到，虚拟机栈和本地方法栈是线程私有的。
	 
	堆和方法区是所有线程共享的资源。其中<br>
	  - 堆是进程中最大的一块内存，主要用于存放新创建的对象（所有对象在这里分配内存）
	  - 方法区主要用于存放已被加载的类信息，常量，静态变量，即时编译器编译后的代码等数据。
## 	什么是上下文切换
   即单核处理器也支持多线程执行代码，CPU通过给每个线程分配CPU时间片来实现这个机制 。时间片是CPU分配给各个线程的时间，因为时间片非常短，所以CPU通过不停地切换线程执行，让我们感觉多个线程是同时执行的。
   <br>
   　CPU通过时间片分配算法来循环执行任务，当前任务执行一个时间片后会切换到下一个任务。但是，在切换前会保存上一个任务的状态，以便下次切换回这个任务时，可以再加载这个任务的状态。所以任务从**保存到加载的过程就是一次上下文切换。上下文切换会影响多线程的执行速度。**
##  并发与并行
   -  并发指的是多个任务交替进行
   -  并行则是指真正意义上的“同时进行”。
   **实际上，如果系统内只有一个CPU，使用多线程时，在真实系统环境下不能并行，只能通过切换时间片的方式交替进行，从而并发执行任务。真正的并行只能出现在拥有多个CPU的系统中。**
   
## 线程的生命周期和状态?
	   初始状态，运行状态，阻塞状态，等待状态，超时等待状态，终止状态

## 什么是线程死锁?如何避免死锁
   　多个线程同时被阻塞，它们中的一个或者全部都在等待某个资源被释放。由于线程被无限期地阻塞，因此程序不可能正常终止。
### 避免死锁的几个常见方法：
    - 避免一个线程同时获取多个锁
	- 避免一个线程在锁内同时占用多个资源，尽量保证每个锁只占用一个资源
	- 尝试使用定时锁，使用 lock.tryLock(timeout) 来代替使用内部锁机制。
	- 对于数据库锁，加锁和解锁必须在一个数据库连接里，否则会出现解锁失败的情况

## sleep() 方法和 wait() 方法区别和共同点?
   相同点：<br>
    - 两者都可以暂停线程的执行，都会让线程进入等待状态。
   不同点：<br>
	- sleep()方法没有释放锁，而 wait()方法释放了锁。
	- sleep()方法属于Thread类的静态方法，作用于当前线程；而wait()方法是Object类的实例方法，作用于对象本身。
	- 执行sleep()方法后，可以通过超时或者调用interrupt()方法唤醒休眠中的线程；执行wait()方法后，通过调用notify()或notifyAll()方法唤醒等待线程。
## 为什么我们调用 start() 方法时会执行 run() 方法，为什么我们不能直接调用 run() 方法？
   - 调用 start 方法可启动线程并使线程进入就绪状态，而 run 方法只是 thread 的一个普通方法调用，还是在主线程里执行

## 多线程开发带来的问题与解决方法？
### 线程安全问题
   线程安全问题指的是在某一线程从开始访问到结束访问某一数据期间，该数据被其他的线程所修改，那么对于当前线程而言，该线程就发生了线程安全问题，表现形式为数据的缺失，数据不一致等。
#### 线程安全问题发生的条件：
    - 多线程环境下，即存在包括自己在内存在有多个线程。
	- 多线程环境下存在共享资源，且多线程操作该共享资源。
    - 多个线程必须对该共享资源有非原子性操作。
#### 线程安全问题的解决思路：
    - 尽量不使用共享变量，将不必要的共享变量变成局部变量来使用。
	- 使用synchronized关键字同步代码块，或者使用jdk包中提供的Lock为操作进行加锁。
	- 使用ThreadLocal为每一个线程建立一个变量的副本，各个线程间独立操作，互不影响。
### 性能问题
    线程的生命周期开销是非常大的，一个线程的创建到销毁都会占用大量的内存。同时如果不合理的创建了多个线程，cup的处理器数量小于了线程数量，那么将会有很多的线程被闲置，闲置的线程将会占用大量的内存，为垃圾回收带来很大压力，同时cup在分配线程时还会消耗其性能。<br>
	
	解决思路:<br>
	利用线程池，模拟一个池，预先创建有限合理个数的线程放入池中，当需要执行任务时从池中取出空闲的先去执行任务，执行完成后将线程归还到池中，这样就减少了线程的频繁创建和销毁，节省内存开销和减小了垃圾回收的压力。同时因为任务到来时本身线程已经存在，减少了创建线程时间，提高了执行效率，而且合理的创建线程池数量还会使各个线程都处于忙碌状态，提高任务执行效率，线程池还提供了拒绝策略，当任务数量到达某一临界区时，线程池将拒绝任务的进入，保持现有任务的顺利执行，减少池的压力。

### 活跃性问题
#### 死锁
	**假如线程 A 持有资源 2，线程 B 持有资源 1，他们同时都想申请对方的资源，所以这两个线程就会互相等待而进入死锁状态。多个线程环形占用资源也是一样的会产生死锁问题**
	
	解决方法:<br>
	- 避免一个线程同时获取多个锁
	- 避免一个线程在锁内同时占用多个资源，尽量保证每个锁只占用一个资源。
	- 尝试使用定时锁，使用 lock.tryLock(timeout) 来代替使用内部锁机制。
	
	**想要避免死锁，可以使用无锁函数（cas）或者使用重入锁（ReentrantLock），通过重入锁使线程中断或限时等待可以有效的规避死锁问题。**
#### 饥饿
	**指的是某一线程或多个线程因为某些原因一直获取不到资源，导致程序一直无法执行。如某一线程优先级太低导致一直分配不到资源，或者是某一线程一直占着某种资源不放，导致该线程无法执行等。**
	
	解决方法：<br>

　　与死锁相比，饥饿现象还是有可能在一段时间之后恢复执行的。可以设置合适的线程优先级来尽量避免饥饿的产生
#### 活锁
    活锁体现了一种谦让的美德，每个线程都想把资源让给对方，但是由于机器“智商”不够，可能会产生一直将资源让来让去，导致资源在两个线程间跳动而无法使某一线程真正的到资源并执行，这就是活锁的问题。
#### 阻塞
	阻塞是用来形容多线程的问题，几个线程之间共享临界区资源，那么当一个线程占用了临界区资源后，所有需要使用该资源的线程都需要进入该临界区等待，等待会导致线程挂起，一直不能工作，这种情况就是阻塞，如果某一线程一直都不释放资源，将会导致其他所有等待在这个临界区的线程都不能工作。当我们使用synchronized或重入锁时，我们得到的就是阻塞线程，如论是synchronized或者重入锁，都会在试图执行代码前，得到临界区的锁，如果得不到锁，线程将会被挂起等待，知道其他线程执行完成并释放锁且拿到锁为止。
	解决方法：<br>
	可以通过减少锁持有时间，读写锁分离，减小锁的粒度，锁分离，锁粗化等方式来优化锁的性能。
···
临界区：

　　临界区是用来表示一种公共的资源（共享数据），它可以被多个线程使用，但是在每次只能有一个线程能够使用它，当临界区资源正在被一个线程使用时，其他的线程就只能等待当前线程执行完之后才能使用该临界区资源。
···

## synchronized 关键字
   synchronized关键字可以保证被它修饰的方法或者代码块在任意时刻只能有一个线程执行。<br>
   synchronized关键字最主要的三种使用方式：修饰实例方法:、修饰静态方法、修饰代码块。<br>
   - 对于普通同步方法，锁是当前实例对象。
   - 对于静态同步方法，锁是当前类的Class对象。
   - 对于同步代码块，锁是synchronized括号里配置的对象。
   **当一个线程试图访问同步代码块时，它首先必须得到锁，退出或抛出异常时必须释放锁。**
### synchronized用的锁是存在哪里的？
   synchronized用到的锁是存在Java对象头里的。
## 说说 JDK1.6 之后的synchronized 关键字底层做了哪些优化，可以详细介绍一下这些优化吗
   JDK1.6 对锁的实现引入了大量的优化，如偏向锁、轻量级锁、自旋锁、适应性自旋锁、锁消除、锁粗化等技术来减少锁操作的开销。
   <br>
   锁主要存在四种状态，依次是：无锁状态、偏向锁状态、轻量级锁状态、重量级锁状态，他们会随着竞争的激烈而逐渐升级。注意锁可以升级不可降级，这种策略是为了提高获得锁和释放锁的效率。

## synchronized和 Lock 的区别？
  - Lock是一个接口，而synchronized是Java中的关键字，synchronized是内置的语言实现；
  - synchronized在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生；而Lock在发生异常时，如果没有主动通过unLock()去释放锁，则很可能造成死锁现象，因此使用Lock时需要在finally块中释放锁；
  - Lock可以让等待锁的线程响应中断，而synchronized却不行，使用synchronized时，等待的线程会一直等待下去，不能够响应中断；
  - 通过Lock可以知道有没有成功获取锁（tryLock()方法：如果获取锁成功，则返回true），而synchronized却无法办到。
  - 　在性能上来说，如果竞争资源不激烈，两者的性能是差不多的，而当竞争资源非常激烈时（即有大量线程同时竞争），此时Lock的性能要远远优于synchronized。所以说，在具体使用时要根据适当情况选择。

## synchronized和ReentrantLock（重入锁） 　
   - 两者都是可重进入锁，就是能够支持一个线程对资源的重复加锁。sychnronized关键字隐式的支持重进入，
   - synchronized 依赖于 JVM 而 ReentrantLock 依赖于 API。ReentrantLock 是 JDK 层面实现的（也就是 API 层面，需要 lock() 和 unlock() 方法配合 try/finally 语句块来完成）
   - eentrantLock 比 synchronized 增加了一些高级功能，主要有3点：①等待可中断；②可实现公平锁；③可实现选择性通知（锁可以绑定多个条件）

##  volatile关键字
    保证共享变量的“可见性”。可见性的意思是当一个线程修改一个共享变量时，另外一个线程能读到这个修改的值。
	把变量声明为volatile，这就指示 JVM每次使用它都到主存中进行读取。

## synchronized 关键字和 volatile 关键字的区别
   - volatile关键字是线程同步的轻量级实现,所以volatile性能比synchronized关键字要好但是volatile关键字只能用于变量而synchronized关键字可以修饰方法以及代码块。
   - 多线程访问volatile关键字不会发生阻塞，而synchronized关键字可能会发生阻塞。、
   - volatile关键字主要用于解决变量在多个线程之间的可见性，而 synchronized关键字解决的是多个线程之间访问资源的同步性。
   - volatile关键字能保证数据的可见性，但不能保证数据的原子性。synchronized关键字两者都能保证。

## 使用线程池的好处？
   - 降低资源消耗。通过重复利用已创建的线程，降低线程创建和销毁造成的消耗。
   - 提高响应速度。当任务到达时，任务可以不需要等到线程创建就能立即执行。
   - 提高线程的可管理性。线程是稀缺资源，如果无限制地创建，不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一分配、调优和监控。

##  说一说几种常见的线程池及适用场景？
    可以创建（Executors.newXXX）3种类型的ThreadPoolExecutor：FixedThreadPool、SingleThreadExecutor、CachedThreadPool。
	- FixedThreadPool：可重用固定线程数的线程池。（适用于负载比较重的服务器）
	   - FixedThreadPool使用无界队列LinkedBlockingQueue作为线程池的工作队列
	   - 该线程池中的线程数量始终不变。当有一个新的任务提交时，线程池中若有空闲线程，则立即执行。若没有，则新的任务会被暂存在一个任务队列中，待有线程空闲时，便处理在任务队列中的任务。
	- SingleThreadExecutor：只会创建一个线程执行任务。（适用于需要保证顺序执行各个任务；并且在任意时间点，没有多线程活动的场景。）
	   - SingleThreadExecutorl也使用无界队列LinkedBlockingQueue作为工作队列
	   - 若多余一个任务被提交到该线程池，任务会被保存在一个任务队列中，待线程空闲，按先入先出的顺序执行队列中的任务。
	-  CachedThreadPool：是一个会根据需要调整线程数量的线程池。（大小无界，适用于执行很多的短期异步任务的小程序，或负载较轻的服务器）
	   -  CachedThreadPool使用没有容量的SynchronousQueue作为线程池的工作队列，但CachedThreadPool的maximumPool是无界的。
	   -  线程池的线程数量不确定，但若有空闲线程可以复用，则会优先使用可复用的线程。若所有线程均在工作，又有新的任务提交，则会创建新的线程处理任务。所有线程在当前任务执行完毕后，将返回线程池进行复用。
	  
	-  ScheduledThreadPool：继承自ThreadPoolExecutor。它主要用来在给定的延迟之后运行任务，或者定期执行任务。使用DelayQueue作为任务队列。

## 线程池都有哪几种工作队列？
   - ArrayBlockingQueue：是一个基于数组结构的有界阻塞队列，此队列按FIFO（先进先出）原则对元素进行排序。
   - LinkedBlockingQueue：是一个基于链表结构的阻塞队列，此队列按FIFO排序元素，吞吐量通常要高于ArrayBlockingQueue。静态工厂方法Executors.newFixedThreadPool()使用了这个队列。
   - SynchronousQueue：是一个不存储元素的阻塞队列。每个插入操作必须等到另一个线程调用移除操作，否则插入操作一直处于阻塞状态，吞吐量通常要高于Linked-BlockingQueue，静态工厂方法Executors.newCachedThreadPool使用了这个队列
   - PriorityBlockingQueue：一个具有优先级的无限阻塞队列。

## 创建线程的几种方式？
   - 通过继承Thread类创建线程
```
public class MyThread extends Thread {//继承Thread类

　　//重写run方法

　　public void run(){

　　}

}
```
  - 通过实现Runnable接口来创建线程
```
public class MyThread2 implements Runnable {//实现Runnable接口

　　//重写run方法

　　public void run(){

　　}

}
```
    -  实现Callable接口来创建线程
```
public class ThreadC implements Callable {

    public static void main(String[] args) {
        ThreadC threadC = new ThreadC();
        FutureTask<Integer> futureTask = new FutureTask<Integer>((Callable<Integer>)() -> {return 5;});
       new Thread(futureTask).start();
       try{
           Integer integer = futureTask.get();
           System.out.println(integer);
       }catch (Exception e){
           e.printStackTrace();
       }
    }

    @Override
    public Object call() throws Exception {
        return null;
    }
}
```
   -  使用Executor框架来创建线程池
      -  Executor执行Runnable任务：
   通过Executors的以上四个静态工厂方法获得 ExecutorService实例，而后调用该实例的execute（Runnable command）方法即可。一旦Runnable任务传递到execute（）方法，该方法便会自动在一个线程上。
   <br>
```
public class TestFixThreadPool {

    public static void main(String[] args) {
        ExecutorService executorService = Executors.newFixedThreadPool(5);
        for (int i = 0 ;i < 5; i++){
            executorService.execute(new TestRunable());
            System.out.println("i="+i);

        }
        executorService.shutdown();
    }
}

class TestRunable implements Runnable{

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()+"线程调用");
    }
}

```
       -  Executor执行Callable任务：
```
public class CallableDemo{   
    public static void main(String[] args){   
        ExecutorService executorService = Executors.newCachedThreadPool();   
        List<Future<String>> resultList = new ArrayList<Future<String>>();   
  
        //创建10个任务并执行   
        for (int i = 0; i < 10; i++){   
            //使用ExecutorService执行Callable类型的任务，并将结果保存在future变量中   
            Future<String> future = executorService.submit(new TaskWithResult(i));   
            //将任务执行结果存储到List中   
            resultList.add(future);   
        }   
  
        //遍历任务的结果   
        for (Future<String> fs : resultList){   
                try{   
                    while(!fs.isDone);//Future返回如果没有完成，则一直循环等待，直到Future返回完成  
                    System.out.println(fs.get());     //打印各个线程（任务）执行的结果   
                }catch(InterruptedException e){   
                    e.printStackTrace();   
                }catch(ExecutionException e){   
                    e.printStackTrace();   
                }finally{   
                    //启动一次顺序关闭，执行以前提交的任务，但不接受新任务  
                    executorService.shutdown();   
                }   
        }   
    }   
}   
class TaskWithResult implements Callable<String>{   
    private int id;   
  
    public TaskWithResult(int id){   
        this.id = id;   
    }   
  
    // 重写call()方法
    public String call() throws Exception {  
        System.out.println("call()方法被自动调用！！！    " + Thread.currentThread().getName());   
        //该返回结果将被Future的get方法得到  
        return "call()方法被自动调用，任务返回的结果是：" + id + "    " + Thread.currentThread().getName();   
    }   
}  
```
### 实现Runnable接口和Callable接口的区别？
     Runnable接口或Callable接口实现类都可以被ThreadPoolExecutor或ScheduledThreadPoolExecutor执行。两者的区别在于 Runnable 接口不会返回结果但是 Callable 接口可以返回结果。
	 
### 执行execute()方法和submit()方法的区别是什么呢？
    - execute() 方法用于提交不需要返回值的任务，所以无法判断任务是否被线程池执行成功与否
	-  submit() 方法用于提交需要返回值的任务。线程池会返回一个Future类型的对象，通过这个Future对象可以判断任务是否执行成功，并且可以通过future的get()方法来获取返回值，get()方法会阻塞当前线程直到任务完成，而使用 get（long timeout，TimeUnit unit）方法则会阻塞当前线程一段时间后立即返回，这时候有可能任务没有执行完。
 
 ## 线程池参数？
    - corePoolSize：线程池的基本大小，当提交一个任务到线程池时，线程池会创建一个线程来执行任务，即使其他空闲的基本线程能够执行新任务也会创建线程，等到需要执行的任务数大于线程池基本大小时就不再创建。说白了就是，即便是线程池里没有任何任务，也会有corePoolSize个线程在候着等任务。
	- maximumPoolSize:最大线程数，不管你提交多少任务，线程池里最多工作线程数就是maximumPoolSize。
	- keepAliveTime:线程的存活时间。当线程池里的线程数大于corePoolSize时，如果等了keepAliveTime时长还没有任务可执行，则线程退出。
	- unit：这个用来指定keepAliveTime的单位，比如秒:TimeUnit.SECONDS。
	- workQueue：用于保存等待执行任务的阻塞队列，提交的任务将会被放到这个队列里。
	- threadFactory：线程工厂，用来创建线程。主要是为了给线程起名字，默认工厂的线程名字：pool-1-thread-3。
	- handler：拒绝策略，即当线程和队列都已经满了的时候，应该采取什么样的策略来处理新提交的任务。默认策略是AbortPolicy（抛出异常），其他的策略还有：CallerRunsPolicy(只用调用者所在线程来运行任务)、DiscardOldestPolicy(丢弃队列里最近的一个任务，并执行当前任务)、DiscardPolicy(不处理，丢弃掉

    