---
layout: post
title: Java集合详解
categories: Java
description: Java Collection Map 的简介。
keywords: Collection, Collection, 集合
---

# Java集合工具包：
Java 集合是 Java 提供的工具包，包含了常用的数据结构：集合、链表、队列、栈、数组、映射等。
Java 集合工具包的位置是 java.util.* 。
Java 集合主要可以分为以下 4 个部分：List列表、Set集合、Map映射、工具类（Iterator 迭代器、Enumeration 枚举类、Arrays 和 Collections）。

框架图(如下)：
![](/images/posts/java/2017-02-27-collection-map.png)



由上图可知，Java 集合主要由两部分构成，Collection 和 Map。

## Collection
Collection 接口是 List、Set、和 Queue 接口的父接口，定义了接口的基本操作，增、删、改、查。
Collection 接口 API 如下：
![](/images/posts/java/2017-02-27-collection-api.png)

### List 
1. List 是元素有序并且可以重复的集合，也被成为序列。
2. List 可以精确的控制每个元素的插入位置，或删除某个位置元素。
3. List 接口的常用子类： ArrayList 、 LinkedList 、 Vector 、 Stack 。

#### ArrayList
1. ArrayList 底层使用数组实现，实现了可变大小的数组，每个 ArrayList 实例都有一个容量（Capacity），即用于存储元素的数组的大小。
这个容量可以随着不断添加新元素而不断增加，默认长度是10，当ArrayList中的元素超过10个以后，会重新分配内存空间。容量变化的规则是 旧容量 * 1.5 + 1 。
2. ArrayList 是排列有序且可以重复的，它允许所有元素，包括 null。
3. ArrayList 的size()、isEmpty() 、get() 、set() 、iterator() 和 listIterator() 操作都以固定时间运行。
add() 操作以分摊的固定时间 运行，也就是说，添加 n 个元素需要 O(n) 时间。其他所有操作都以线性时间运行（大体上讲）。
与用于 LinkedList 实现的常数因子相比，此实现的常数因子较低。故，ArrayList 的获取和插入的速度快，而增删的速度较慢。
4. ArrayList 的用法大致类似于 Vector，但是 ArrayList 是非线程安全的。

#### LinkedList
1. LinkedList 与 ArrayList 一样，都是排列有序且可以重复的，允许null元素。但是LinkedList得底层使用的是双向循环链表的数据结构。它也可以被当作堆栈、队列或双端队列进行操作。
2. LinkedList提供额外的 get，remove，insert 方法在 LinkedList 的首部或尾部。这些操作使 LinkedList 可被用作堆栈（stack），队列（queue）或双向队列（deque）。
3. LinkedList 可以被当作堆栈、队列或双端队列进行操作。
4. LinkedList 随机访问效率低，但随机插入、随机删除效率低。
5. LinkedList 也是非线程安全的。

#### Vector
1. Vector 是矢量队列，和 ArrayList 一样，它也是一个动态数组，由数组实现。但是 ArrayList 是非线程安全的，而 Vector 是线程安全的。
2. ArrayList 支持序列化，而 Vector 不支持；即 ArrayList 有实现 java.io.Serializable 接口，而 Vector 没有实现该接口。
3. Vector 支持通过 Enumeration 去遍历，而 ArrayList 不支持.
4. Vector 的容量增长与 “增长系数有关” ，若指定了“增长系数”，且 “增长系数有效(即，大于 0 )”；那么，每次容量不足时，“新的容量” = “原始容量 + 增长系数”。若增长系数无效(即，小于/等于 0 )，则“新的容量” = “原始容量 x 2”。

#### Stack
1. Stack 是栈，它继承于Vector。它的特性是：先进后出(FILO, First In Last Out)。

### List 小结
如果涉及到“栈”、“队列”、“链表”等操作，应该考虑用 List，具体的选择哪个 List，根据下面的标准来取舍。
1. 对于需要快速插入，删除元素，应该使用 LinkedList。
2. 于需要快速随机访问元素，应该使用 ArrayList。
3. 对于“单线程环境” 或者 “多线程环境，但 List 仅仅只会被单个线程操作”，此时应该使用非同步的类(如 ArrayList )。
4. 对于“多线程环境，且List可能同时被多个线程操作”，此时，应该使用同步的类(如Vector)。
5. 尽量返回接口而非实际的类型，如返回 List 而非 ArrayList ，这样如果以后需要将 ArrayList 换成 LinkedList 时，客户端代码不用改变。这就是针对抽象编程。

### Set
1. Set 中不能加入重复的元素，且 Set 中的元素是无序的。
2. Set 接口，常用的有两个子类，散列存放的 HashSet ，有序存放的 TreeSet。

#### HashSet
向 HashSet 集合中存入一个元素时，HashSet 会调用该对象的 hashCode() 方法来得到该对象的 hashCode 值，然后根据 hashCode 值来决定该对象在 HashSet 中存储位置。
简单的说，HashSet 集合判断两个元素相等的标准是两个对象通过 equals() 方法比较相等，并且两个对象的 hashCode() 方法返回值相等。
注意，如果要把一个对象放入 HashSet 中，重写该对象对应类的 equals() 方法，也应该重写其 hashCode() 方法。原因如果两个对象通过 equals() 方法比较返回 true 时，其 hashCode 也应该相同。
另外，对象中用作 equals() 比较标准的属性，都应该用来计算 hashCode 的值。
1. HashSet 不能保证元素的排列顺序，顺序有可能发生变化。
2. HashSet 是非线程安全的。
3. HashSet 的元素可以是null,但只能放入一个null。

#### TreeSet
1. TreeSet 是 SortedSet 接口为唯一实现类，可以确保集合元素处于排序状态。TreeSet 支持两种排序方式，“自然排序” 和 “定制排序”，其中自然排序为默认的排序方式。向 TreeSet 中加入的应该是同一个类的对象。
2. 自然排序是根据集合元素的大小，以升序排列，如果要定制排序，应该使用 Comparator 接口，实现 int compare(T o1,T o2) 方法。
3. TreeSet 判断两个对象不相等的方式是两个对象通过 equals() 方法返回 false ，或者通过 compareTo() 方法比较没有返回 0。

#### LinkedHashSet
1. LinkedHashSet 同样是根据元素的 hashCode 值来决定元素的存储位置，但是它同时使用链表维护元素的次序。这样使得元素看起 来像是以插入顺序保存的，也就是说，当遍历该集合时候，LinkedHashSet 将会以元素的添加顺序访问集合的元素。
2. LinkedHashSet 在迭代访问 Set 中的全部元素时，性能比 HashSet 好，但是插入时性能稍微逊色于 HashSet。
3. LinkedHashSet 也是非线程安全的。

### Set 小结
1. TreeSet 是二差树实现的,TreeSet 中的数据是自动排好序的，不允许放入 null 值。 
2. HashSet 是哈希表实现的,HashSet 中的数据是无序的，可以放入 null，但只能放入一个 null，两者中的值都不能重复，就如数据库中唯一约束。 
3. HashSet 要求放入的对象必须实现 hashCode() 方法，放入的对象，是以 hashcode 码作为标识的，而具有相同内容的对象，hashcode 是一样，所以放入的内容不能重复。但是同一个类的对象可以放入不同的实例 。
4. HashSet 是通过 HashMap 实现的,TreeSet 是通过 TreeMap 实现的,只不过 Set 用的只是 Map 的 key。

### Map
1.Map 提供了一种映射关系，其中的元素是以 键值对（key-value），的形式储存的，可以实现通过 key 快速查找到 value。
2.Map里的 键值对 以 Entry 类型的对象实例形式存在，可以通过 entrySet() 方法，获取 Entry 的 Set 集合。
3.键（key）是不可以重复的，而值（value）是可以重复的。
4.键（key）与值（value）得关系是一一对应的，一个键只能对应一个值。
5.Map 接口的常用子类：HashMap 、 TreeMap 、 Hashtable 。
6.Map 有个非常重要的子接口 ConcurrentMap。
Map 接口 API 如下：
![](/images/posts/java/2017-02-27-map-api.png) 

#### HashMap
1. HashMap 是非线程安全的。
2. HashMap 是 Hashtable 的轻量级实现（非线程安全的实现）。
3. HashMap 允许空（null）键值（key）,由于非线程安全，效率上可能高于Hashtable。
4. HashMap 把 Hashtable 的contains()方法去掉了，改成 containsValue() 和 containsKey()。因为 contains() 方法容易让人引起误解。
5. HashMap 可以通过下面的语句进行同步： Map m = Collections.synchronizeMap(hashMap);

#### Hashtable
1. Hashtable 是线程安全的一个 Collection，其方法是 Synchronize 的。
2. Hashtable 不允许空（null）键值（key）。
3. Hashtable 继承自 Dictionary 类。

#### TreeMap
1. TreeMap 实现 SortMap 接口，能够把它保存的记录根据键排序,默认是按键值的升序排序，也可以指定排序的比较器，当用 Iterator 遍历 TreeMap 时，得到的记录是排过序的。

#### LinkedHashMap
1. LinkedHashMap 是 HashMap 的一个子类，如果需要输出的顺序和输入的相同,那么用 LinkedHashMap 可以实现,它还可以按读取顺序来排列，像连接池中可以应用。


####  ConcurrentHashMap
Java 在 Java5 版本提供了 ConcurrentHashMap。
1. ConcurrentHashMap 是一个经常被使用的数据结构，相比于 Hashtable 以及Collections.synchronizedMap()，ConcurrentHashMap在线程安全的基础上提供了更好的写并发能力，但同时降低了对读一致性的要求。
2. ConcurrentHashMap 引入了一个“分段锁”的概念，具体可以理解为把一个大的 Map 拆分成 N 个小的 Hashtable，根据 key.hashCode() 来决定把 key 放到哪个 Hashtable 中。
可以提供相同的线程安全，但是相比 Hashtable 效率提升 N 倍，默认提升 16 倍。
3. Hashtable 虽然性能上不如 ConcurrentHashMap，但并不能完全被取代，两者的迭代器的一致性不同的，Hashtable 的迭代器是强一致性的，而 ConcurrentHashMap 是弱一致的。 ConcurrentHashMap 的get()，clear()，iterator() 都是弱一致性的。

### Comparable 和 Comparator
Comparable 和 Comparator 都是 Java 集合框架的成员。

#### Comparable 接口
1. 实现该接口表示：这个类的实例是可以比较大小，且可以进行自然排序的。
2. 其实现类需要实现 compareTo() 方法，定义默认的比较规则。
3. compareTo() 方法，返回正数代表 大于，负数代表 小于，0 代表相等。

#### Comparator 比较工具接口
1. 用于定义临时的比较规则，而不是默认的比较规则。
2. 其实现类，需要实现 compare() 方法。

### Iterator 接口
Iterator 接口 是专门的迭代输出接口


## 集合与数组的对比
1. 集合的长度是可变的，可以根据需求来改变集合的长度，而数据是定长的，在数据对象创建的时候，就已经指定了数组的长度。
2. 部分集合可以通过任意类型查找所映射的具体对象，而数组只能通过下标访问元素，并且元素的类型是固定的。



## 注意
1. Collection 接口没有 get() 方法来取得某个元素。只能通过 iterator() 遍历元素。 
2. Set 和 Collection 拥有一模一样的接口。
3. 可以使用 LinkedList 构造堆栈 Stack、队列 Queue。
4. Map 中元素，可以将 key 序列、value 序列单独抽取出来。
使用 keySet() 抽取 key 序列，将 Map 中的所有 keys 生成一个 Set。
使用 values() 抽取 value 序列，将 Map 中的所有 values 生成一个 Collection。
为什么一个生成 Set，一个生成 Collection？那是因为，key 总是独一无二的，value 允许重复。