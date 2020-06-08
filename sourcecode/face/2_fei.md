# MYSQL 篇
  ## left join   inner join

  ## 什么情况下会出现笛卡尔积

  ##  MYSQL 存储引擎 项目中用法
  ##  explain  用法
  ##  mysql 几种事务 
  ##  索引覆盖
  ##  组合索引以及 最左匹配原则
  ##  索引在数据库底层的实现  ，hash表 以及  B+tree
  ##  

# linux 
  ## 内存 查看   top , free ,vmstat

# JVM 
  ## JPS
  ## jstack  查看堆栈

# ES
  ##  ES 分片数设置多少
  ##  ES中节点的角色  Master Data  Coordinating Ingest Tribe 
  ##  ES的选举
 
# RabiitMQ
## 如何保证数据一致性   
   ACK消息确认机制
## RabbitMq的 结构
## 交换机模型

# Redis
## 使用过哪些数据类型  String ， set，Zset (底层是啥实现的)  怎么存储

# Git 
## 分支切换，来取等
  - git  checkout 分支名

# Springboot
## springboot run 之后启动加载流程

# 多线程
## 多线程的项目中的使用场景，
消息队列的消费 。。。 
## 多线程的创建，参数配置，拒绝策略等
  - 带参创建 多线程
```
 /**
     * Creates a new {@code ThreadPoolExecutor} with the given initial
     * parameters and default thread factory.
     *
     * @param corePoolSize 核心线程池数量 the number of threads to keep in the pool, even
     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set
     * @param maximumPoolSize 最大线程池数量 the maximum number of threads to allow in the
     *        pool
     * @param keepAliveTime  线程活跃时间 超时推出线程 when the number of threads is greater than
     *        the core, this is the maximum time that excess idle threads
     *        will wait for new tasks before terminating.
     * @param unit keepAliveTime的单位 the time unit for the {@code keepAliveTime} argument
     * @param workQueue 线程池中的任务队列 the queue to use for holding tasks before they are
     *        executed.  This queue will hold only the {@code Runnable}
     *        tasks submitted by the {@code execute} method.
     * @param handler 拒绝策略 the handler to use when execution is blocked
     *        because the thread bounds and queue capacities are reached
     * @throws IllegalArgumentException if one of the following holds:<br>
     *         {@code corePoolSize < 0}<br>
     *         {@code keepAliveTime < 0}<br>
     *         {@code maximumPoolSize <= 0}<br>
     *         {@code maximumPoolSize < corePoolSize}
     * @throws NullPointerException if {@code workQueue}
     *         or {@code handler} is null
     */
    public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue,
                              RejectedExecutionHandler handler) {
        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
             Executors.defaultThreadFactory(), handler);
    }
```
```
        int i = Runtime.getRuntime().availableProcessors();  返回可用处理器的Java虚拟机的数量。
        new  ThreadPoolExecutor(i,i*2,8,TimeUnit.SECONDS,new ArrayBlockingQueue<>(16),new ThreadPoolExecutor.AbortPolicy());
```
# 集合
## hashMap 1.7 与1.8 实现原理
 - 1.7 底层时 数组加链表
 - 1.8 底层   数组+ 链表+ 红黑数  （当链表长度>8 自动转换为红黑树）
[hashMap源码分析](https://zhuanlan.zhihu.com/p/28501879)
## ArrayList 与 LinkList实现原理
 - ArrayList 是基于动态数组的数据结构 ，LinKList 是基于链表的数据结构。 

# POI
## 2007之上的版本读取方式变化
##  内存溢出问题
##  大批量数据导出Excel问题