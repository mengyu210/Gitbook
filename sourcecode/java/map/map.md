
## hashMap原理，java8做的改变
   从结构实现来讲，HashMap是数组+链表+红黑树（JDK1.8增加了红黑树部分）实现的。

## List 和 Set的区别
  ### List 
    - 可以允许重复的对象。
	- 可以插入多个null元素。
	- 有序容器
  ### Set 
    - 不允许重复的对象
	- 只能插入1个null元素
	- 无序容器，可以使用TreeSet实现有序
  <br>**List 是按照元素的添加顺序来存储的。而 Set 的实现类都有一套自己的排序算法，每添加一个元素，都会按照其内部算法将元素添加到合适的位置，所以不能保证内部存储是按元素添加的顺序而存储的。**
  
## Set和hashCode以及equals方法的联系
     set集合中存放的数据有一个特点，那就是无序且不重复。无序是因为set集合中的元素没有下坐标。不重复的原因就是因为set集合中有hashcode与equals这两个方法。

## List 和 Map 区别
   - 结构：List为单列结构，Map为双列结构
   - 重复性：List为可以重复，Map为双列集合（key-value）,key不可以重复
   - 有序性：List为有序集合，Map的key是无序的

## Arraylist 与 LinkedList 区别
   - 数据结构：ArrayList基于动态数组的数据结构。LinkedList基于链表的数据结构。
   - 操作性：ArrayList查询快，增删慢，LinkedList查询慢，增删快。
## ArrayList 与 Vector 区别
   - 同步性：Vector线程安全，用synchronized实现线程安全。ArrayList线程不安全。
   - 数据容量增长：二者都有一个初始容量大小，采用线性连续存储空间，当存储的元素的个数超过了容量时，就需要增加二者的存储空间，Vector增长原来的一倍，ArrayList增加原来的0.5倍。
   
   - 两者都是基于索引的，内部由一个数组支持。
   - 两者维护插入的顺序，我们可以根据插入顺序来获取元素
   - ArrayList和Vector的迭代器实现都是fail-fast的。
   - ArrayList和Vector两者允许null值，也可以使用索引值对元素进行随机访问。
##  HashMap 和 Hashtable 的区别
   - 继承不同父类：Hashtable继承自Dictionary类，而HashMap继承自AbstractMap类。但二者都实现了Map接口。
   - 线程安全：Hashtable线程安全，HashMap线程不安全
   - 是否提供contains方法： HashMap把Hashtable的contains方法去掉了，改成containsValue和containsKey。Hashtable则保留了contains，containsValue和containsKey三个方法，其中contains和containsValue功能相同。
   - key和value的是否可以为null值：Hashtable中，key和value都不允许出现null值。HashMap中，null可以作为键，这样的键只有一个；可以有一个或多个键所对应的值为null。

##  HashSet 和 HashMap 区别
|HashMap	|HashSet	|
|--	|--	|
|HashMap实现了Map接口	|HashSet实现了Set接口	|
|HashMap储存键值对	|HashSet仅仅存储对象	|
|使用put()方法将元素放入map中	|使用add()方法将元素放入set中	|
|HashMap中使用键对象来计算hashcode值	|HashSet使用成员对象来计算hashcode值，对于两个对象来说hashcode可能相同，所以equals()方法用来判断对象的相等性，如果两个对象不同的话，那么返回false	|
|HashMap比较快，因为是使用唯一的键来获取对象	|HashSet较HashMap来说比较慢 |

## HashMap 和 ConcurrentHashMap 的区别
  - 线程安全性：HashMap线程不安全，ConcurrentHashMap线程安全
  - ConcurrentHashMap特性：性能与安全性兼得。加入分段锁的概念，相当于一个大集合，根据hashcode值分成多个hashtable，根据key.hashCode()来决定把key放到哪个HashTable中。

##  HashMap 的工作原理及代码实现，什么时候用到红黑树
   ### 工作原理：
    通过hash的方法，通过put和get存储和获取对象。存储对象时，我们将K/V传给put方法时，它调用hashCode计算hash从而得到bucket位置，进一步存储，HashMap会根据当前bucket的占用情况（当键值对的数量大于容量（capacity）*负载因子(load factor 默认值0.75)时）自动调整容量(超过Load Facotr则resize为原来的2倍)。获  取对象时，我们将K传给get，它调用hashCode计算hash从而得到bucket位置，并进一步调用equals()方法确定键值对。如果发生碰撞的时候，Hashmap通过链表将产生碰撞冲突的元素组织起来，在Java 8中，如果一个bucket中碰撞冲突的元素超过某个限制(默认是8)，则使用红黑树来替换链表，从而提高速度。
   ### 什么时候用到红黑树：
       当链表（bucket）的数量大于8时，后面的使用红黑树。使用红黑树增加检索的速度。

## 多线程情况下HashMap死循环的问题
   - 原因：HashMap是采用链表解决Hash冲突，因为是链表结构，那么就很容易形成闭合的链路（原因：多线程操作时，可能会有两个或以上的线程同时触发rehash--重新计算hash值--操作，容易造成闭合的链路），这样在循环的时候只要有线程对这个HashMap进行get操作就会产生死循环。
   - 解决思路：用ConcurrentMap代替HashMap

## HashMap出现Hash DOS攻击的问题
   
  - 打补丁，修改hash算法。

  - 限制POST的参数个数，限制POST的请求长度。

  - 使用防火墙检测异常请求

  - 增加权限验证，最大可能的在jsonDecode()之前把非法用户拒绝

  - 重写jsonDecode()方法

## ConcurrentHashMap 的工作原理及代码实现，如何统计所有的元素个数
   - ConcurrentHashMap工作原理：容器中有多把锁，每一把锁锁一段数据，这样在多线程访问时不同段的数据时，就不会存在锁竞争了，这样便可以有效地提高并发效率。这就是ConcurrentHashMap所采用的"分段锁"思想。
   - 如何统计所有的元素个数：size()
## HashSet和TreeSet的比较。
   - HashSet是hash表实现的，TreeSet时二叉树实现的。
   - TreeSet中的数据是自动排好序的，不允许为null；HashSet中的元素是无序的，可以为null但是能有一个。TreeSet和  HashSet中的元素都不能重复。
   - HashSet要求放入的元素必须实现hashcode（）方法，因为HashSet判断两个元素是否相同实质上就是hashcode码是否相同。
  
   适用场景：<br>
因为HashSet是hash表实现的所以性能优于TreeSet，一般使用HashSet，当遇到排序问题时，则选择TreeSet（因为TreeSet实现了SortedSet接口）。 
## HashTable和ConcurrentHashMap的比较。
   两者都是线程安全的，但是HashTable是在成员方法前加Synchronized锁，当数据量较大时HashTable的性能比较低，因为迭代时需要被锁住很长时间；而ConcurrentHashMap使用的是分段锁机制（对局部加锁）

## 集合中线程安全的有：
   Vector、Stack、HashTable、Enumeration（枚举）
## 解决Hash冲突的方法：
   拉链法、开放地址法、建立公共溢出区、再哈希法。


## Iterater和ListIterator之间有什么区别？
   - 我们可以使用Iterator来遍历Set和List集合，而ListIterator只能遍历List。
   - Iterator只可以向前遍历，而LIstIterator可以双向遍历。
   - ListIterator从Iterator接口继承，然后添加了一些额外的功能，比如添加一个元素、替换一个元素、获取前面或后面元素的索引位置。

## hashCode()和equals()方法有何重要性？
   HashMap使用Key对象的hashCode()和equals()方法去决定key-value对的索引。当我们试着从HashMap中获取值的时候，这些方法也会被用到。如果这些方法没有被正确地实现，在这种情况下，两个不同Key也许会产生相同的hashCode()和equals()输出，HashMap将会认为它们是相同的，然后覆盖它们，而非把它们存储到不同的地方。同样的，所有不允许存储重复数据的集合类都使用hashCode()和equals()去查找重复，所以正确实现它们非常重要。equals()和hashCode()的实现应该遵循以下规则：
   
   - 如果o1.equals(o2)，那么o1.hashCode() == o2.hashCode()总是为true的。
   - 如果o1.hashCode() == o2.hashCode()，并不意味着o1.equals(o2)会为true。

 