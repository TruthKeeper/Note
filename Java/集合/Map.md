> 弥补了`Set`、`List`的不足，用来存放键值对

## HashMap

> 数组+链表的结构， **无序** ，又称拉链表的设计结构，内部有一个数组，Android中为`HashMapEntry[]`，JDK中为`Node[]`，又称桶 数组，桶结构是一个单链表结构。

![](http://img.blog.csdn.net/20161027004004320)

`put`方法会通过对KEY进行哈希运算（对hashcode进行二次哈希运算，将hashcode与低16位的进行异或运算），又称 **扰动函数** ，再进行取模运算，保证散列分布，如触发扩容操作会重新生成桶 结构，哈希值相同则发生了哈希碰撞，添加到桶 的 **顶部** 。

`get`方法会根据KEY的哈希值来获取到桶的对应索引，然后对桶进行迭代，满足KEY的内存地址对应相同 || KEY的`equals`返回true 则返回桶中的对应值。

扩容操作时每次都是长度翻倍，约束为 **2** 的幂。

`keySet`底层同样是`entrySet`的迭代规则，对桶数组、桶内进行迭代。

JDK 8中对HashMap进行了红黑树的优化，当桶元素大于 **64** ，且桶中元素长度大于 **8** 时（迭代链表效率较低），会转换为树形结构，小于 **6** 且满足条件时扩容时在转换回来。

![](https://user-gold-cdn.xitu.io/2017/9/20/9839d16b2f8d488f3400dda385f993e1)

## LinkedHashMap

在`HashMap`的基础上扩展了`Entry`，又称`Node`，使每个节点连接在一起额外形成了一个双向循环链表，并且支持开启访问记录，默认不开启，`Node`顺序就为插入顺序，开启后访问过后的`Node`会在`LinkedHashMap`链表的 **末尾** ，正是`LRU`算法的实现。

> 特点：继承`HashMap`，扩展了`Node`结构、`put`、`get`、`entrySet`等方法，额外的链表结构会多占用一点内存空间， **有序** 

## TreeMap

**有序** ，KEY为基本类型、String或者实现`Comparable`接口的类型，基于二叉树的分布结构，升序

## Hashtable

**线程安全** ，与`HashMap`的区别在于哈希算法不同，没有取模运算，没有桶结构的优化，不允许空的KEY和VALUE，而`HashMap`允许

## WeakHashMap

对Key进行弱引用的绑定，而不是Value

## ConcurrentHashMap

上文可知，HashMap是线程不安全的，而通过`Collections.synchronizedMap`和Hashtabel是对整个对象进行无脑加锁，性能不高，而ConcurrentMap接口的引入就是为了既能同步操作又能多线程访问

**JDK7**中`ConcurrentHashMap`设计成分段锁，不同于HashMap中的桶数组，`ConcurrentHashMap`中是一个Segment数组，数组中存放着桶数组

![](https://user-gold-cdn.xitu.io/2017/8/13/0e35ff8362ff1f829da5ea0288c69bf2?imageView2/0/w/1280/h/960)

因此并发读写上理论可以支持segment大小的并发数量

**JDK8**中`ConcurrentHashMap`不是分段锁，而是一个Node数组结构，通过对操作的节点放置forwardNode，进行加锁

[ConcurrentHashMap源码分析（JDK8版本）](http://blog.csdn.net/u010723709/article/details/48007881)