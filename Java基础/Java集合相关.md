## Changelog
- 2020-09-02 in coffee shop ：补充sparseArray和ArrayMap

## Java集合的类结构图
![4308d938e43b220adece987467281cae.png](evernotecid://F45BDC07-29D2-4550-A919-05E046B11E09/appyinxiangcom/24066108/ENResource/p5253)

>> todo：图太模糊了，要进行替换

### list、set、map的特点？优点和限制？常用场景？
- List：有序、可重复；索引查询速度快；插入、删除伴随数据移动，速度慢；
- Set：无序，不可重复；
- Map：键值对，键唯一，值多个、

>> todo：画表格，或者找一篇博客，不要重复造轮子，这种东西太成熟了，又不是新的。


### ArrayList和LinkedList怎么动态扩容的吗？

- ArrayList : ArrayList 的初始大小是0，然后，当add第一个元素的时候大小则变成10。并且，在后续扩容的时候会变成当前容量的1.5倍大小。
- LinkedList : linkedList 是一个双向链表，没有初始化大小，也没有扩容的机制，就是一直在前面或者后面新增就好。


### HashSet与TreeSet的区别和适用场景

1.TreeSet 是二叉树（红黑树的树据结构）实现的，TreeSet中的数据是自动排好序的，不允许放入null值。

2.HashSet 是哈希表实现的，HashSet中的数据是无序的可以放入null，但只能放入一个null，两者中的值都不重复，就如数据库中唯一约束。


适用场景分析:
HashSet是基于Hash算法实现的，其性能通常都优于TreeSet。为快速查找而设计的Set，我们通常都应该使用HashSet，在我们需要排序的功能时，我们才使用TreeSet。

扩展问题：hash算法是怎样实现的？
>> todo 百度
链接：https://www.hollischuang.com/archives/2091

### HashMap与TreeMap、HashTable的区别及适用场景


- HashMap 非线程安全；基于哈希表(散列表)实现。使用HashMap要求的键类明确定义了hashCode()和equals()[可以重写hasCode()和equals()]，为了优化HashMap空间的使用，您可以调优初始容量和负载因子。其中散列表的冲突处理主分两种，一种是开放定址法，另一种是链表法。HashMap实现中采用的是链表法。

- TreeMap：非线程安全基于红黑树实现。TreeMap没有调优选项，因为该树总处于平衡状态。

>> TODO ：画表格，文字描述太多了，不喜欢


### HashMap相关 
10，hashmap的原理如何。hashcode和equal这两者有什么区别吗
1. 什么时候会使用HashMap？他有什么特点？
是基于Map接口的实现，存储键值对时，它可以接收null的键值，是非同步的，HashMap存储着Entry(hash, key, value, next)对象。

2. 你知道HashMap的工作原理吗？
通过hash的方法，通过put和get存储和获取对象。存储对象时，我们将K/V传给put方法时，它调用hashCode计算hash从而得到bucket位置，进一步存储，HashMap会根据当前bucket的占用情况自动调整容量(超过Load Facotr则resize为原来的2倍)。获取对象时，我们将K传给get，它调用hashCode计算hash从而得到bucket位置，并进一步调用equals()方法确定键值对。如果发生碰撞的时候，Hashmap通过链表将产生碰撞冲突的元素组织起来，在Java 8中，如果一个bucket中碰撞冲突的元素超过某个限制(默认是8)，则使用红黑树来替换链表，从而提高速度。

3. 你知道get和put的原理吗？equals()和hashCode()的都有什么作用？
通过对key的hashCode()进行hashing，并计算下标( n-1 & hash)，从而获得buckets的位置。如果产生碰撞，则利用key.equals()方法去链表或树中去查找对应的节点

4. 你知道hash的实现吗？为什么要这样实现？
在Java 1.8的实现中，是通过hashCode()的高16位异或低16位实现的：(h = k.hashCode()) ^ (h >>> 16)，主要是从速度、功效、质量来考虑的，这么做可以在bucket的n比较小的时候，也能保证考虑到高低bit都参与到hash的计算中，同时不会有太大的开销。

5. 如果HashMap的大小超过了负载因子(load factor)定义的容量，怎么办？
如果超过了负载因子(默认0.75)，则会重新resize一个原来长度两倍的HashMap，并且重新调用hash方法。

###  set集合从原理上如何保证不重复？

当向HashSet中添加元素的时候，首先计算元素的hashcode值，然后用这个（元素的hashcode）%（HashMap集合的大小）+1计算出这个元素的存储位置，如果这个位置为空，就将元素添加进去；如果不为空，则用equals方法比较元素是否相等，相等就不添加，否则找一个空位添加

### HashMap和HashTable的主要区别是什么？，两者底层实现的数据结构是什么？

- HashMap 没有排序，允许一个null 键和多个null 值,而Hashtable 不允许；
- Hashtable 的方法是Synchronized 的，而HashMap 不是

HashMap和Hashtable的底层实现都是数组 + 链表结构实现的（jdk8以前）

### HashMap、


HashMap 1.7原理：
HashMap 底层是基于 数组 + 链表 组成的，不过在 jdk1.7 和 1.8 中具体实现稍有不同。

负载因子：给定的默认容量为 16，负载因子为 0.75。Map 在使用过程中不断的往里面存放数据，当数量达到了 16 * 0.75 = 12 就需要将当前 16 的容量进行扩容，而扩容这个过程涉及到 rehash、复制数据等操作，所以非常消耗性能。因此通常建议能提前预估 HashMap 的大小最好，尽量的减少扩容带来的性能损耗。

> 扩展思考：为什么默认扩容是16？为什么不是10？为什么规定
长度只能是2的倍数？

答：为了提高效率；在HashMap的indexFor方法中，要使用得到的hash值去找到位置，我们常规的做法是 hash值 % 容量，在这里可以直接用hash值 & 容量，为什么呢？因为 `X % 2^n = X & (2^n – 1)`  ,而
**位运算(&)效率要比代替取模运算(%)高很多，主要原因是位运算直接对内存数据进行操作，不需要转成十进制，因此处理速度非常快**。


put()方法和get()流程解析：
>> todo：流程图解析

HashMap 1.8的原理：

- 优化背景：当 Hash 冲突严重时，在桶上形成的链表会变的越来越长，这样在查询时的效率就会越来越低；时间复杂度为 O(N)，因此 1.8 中重点优化了这个查询效率。

TREEIFY_THRESHOLD 用于判断是否需要将链表转换为红黑树的阈值。

HashEntry 修改为 Node。

>> 查找文章，最好自己找一个源码


### hash()相关原理解析
1.8 中 HashMap的hash()代码：
````java
/** * Computes key.hashCode() and spreads (XORs) higher bits of 
hash * to lower.  Because the table uses power-of-two masking, sets 
of * hashes that vary only in bits above the current mask will * always collide. (Among known examples are sets of Float keys * holding consecutive whole numbers in small tables.)  So we * apply a transform that spreads the impact of higher bits * downward. There is a tradeoff between speed, utility, and * quality of bit-spreading. Because many common sets of hashes * are already reasonably distributed (so don't benefit from * spreading), and because we use trees to handle large sets of * collisions in bins, we just XOR some shifted bits in the * cheapest possible way to reduce systematic lossage, as well 
as * to incorporate impact of the highest bits that would 
otherwise * never be used in index calculations because of table bounds. */

static final int hash(Object key) {    
int h;    
return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 
16);
}

Object的hashCode()：
public int hashCode() {    
return identityHashCode(this);
}

````

1.7 HashMap hash()方法实现：
````java

final int hash(Object k) {
    int h = hashSeed;
    if (0 != h && k instanceof String) {
        return sun.misc.Hashing.stringHash32((String) k);
    }

    h ^= k.hashCode();
    h ^= (h >>> 20) ^ (h >>> 12);
    return h ^ (h >>> 7) ^ (h >>> 4);}

static int indexFor(int h, int length) {
    return h & (length-1);}
````

HashTable In Java 7
````java
private int hash(Object k) {
    // hashSeed will be zero if alternative hashing is disabled.
    return hashSeed ^ k.hashCode();
    }
   
````
我们可以发现，很简单，相当于只是对k做了个简单的hash，取了一下其hashCode。而HashTable中也没有indexOf方法，取而代之的是这段代码：int index = (hash & 0x7FFFFFFF) % tab.length;。也就是说，HashMap和HashTable对于计算数组下标这件事，采用了两种方法。HashMap采用的是位运算，而HashTable采用的是直接取模。

为啥要把hash值和0x7FFFFFFF做一次按位与操作呢，主要是为了保证得到的index的第一位为0，也就是为了得到一个正数。因为有符号数第一位0代表正数，1代表负数。

Q：为什么 HashTable采用简单的取模算法？是不考虑效率吗？
> HashTable默认的初始大小为11，之后每次扩充为原来的2n+1。也就是说，HashTable的链表数组的默认大小是一个素数、奇数。之后的每次扩充结果也都是奇数。由于HashTable会尽量使用素数、奇数作为容量的大小。当哈希表的大小为素数时，简单的取模哈希的结果会更加均匀。（这个是可以证明出来的，由于不是本文重点，暂不详细介绍，可参考：http://zhaox.github.io/algorithm/2015/06/29/hash）

总结：

- HashMap默认的初始化大小为16，之后每次扩充为原来的2倍。
- HashTable默认的初始大小为11，之后每次扩充为原来的2n+1。当哈希表的大小为素数时，简单的取模哈希的结果会更加均匀，所以单从这一点上看，HashTable的哈希表大小选择，似乎更高明些。因为hash结果越分散效果越好。
- 在取模计算时，如果模数是2的幂，那么我们可以直接使用位运算来得到结果，效率要大大高于做除法。所以从hash计算的效率上，又是HashMap更胜一筹。
- 但是，HashMap为了提高效率使用位运算代替哈希，这又引入了哈希分布不均匀的问题，所以HashMap为解决这问题，又对hash算法做了一些改进，进行了扰动计算。


ConcurrentHashMap In Java 7
hash（）实现其实和HashMap如出一辙，都是通过位运算代替取模，然后再对hashcode进行扰动。区别在于，ConcurrentHashMap 使用了一种变种的Wang/Jenkins 哈希算法，其主要母的也是为了把高位和低位组合在一起，避免发生冲突。至于为啥不和HashMap采用同样的算法进行扰动，我猜这只是程序员自由意志的选择吧。至少我目前没有办法证明哪个更优。


### ConcurrentHashMap原理


Java 1.7时原理：

ConcurrentHashMap 采用了分段锁技术，其中 Segment 继承于 ReentrantLock。不会像 HashTable 那样不管是 put 还是 get 操作都需要做同步处理，理论上 ConcurrentHashMap 支持 CurrencyLevel (Segment 数组数量)的线程并发。每当一个线程占用锁访问一个 Segment 时，不会影响到其他的 Segment。



ConcurrentHashMap 1.8原理：

1.7 已经解决了并发问题，并且能支持 N 个 Segment 这么多次数的并发，但依然存在 HashMap 在 1.7 版本中的问题：那就是查询遍历链表效率太低。和 1.8 HashMap 结构类似：其中抛弃了原有的 Segment 分段锁，而采用了 CAS + synchronized 来保证并发安全性。

问：什么是CAS？
- 如果obj内的value和expect相等，就证明没有其他线程改变过这个变量，那么就更新它为update，如果这一步的CAS没有成功，那就采用自旋的方式继续进行CAS操作。



### Hashmap如何解决散列碰撞（必问）？


### Hashmap底层为什么是线程不安全的？


### ArrayMap跟SparseArray在HashMap上面的改进？

HashMap要存储完这些数据将要不断的扩容，而且在此过程中也需要不断的做hash运算，这将对我们的内存空间造成很大消耗和浪费。


SparseArray实现原理：
- 两个数组，一个是int[]，用于存放key；一个是Object[]，用于存放value
- put()方法：使用二分查找法，去查找到key对应的index，如果查找到，那么直接返回index，否则的话，返回比大稍大的值的索引 ~presentIndex，又或者~mKeys.length。如果index >= 0，说明找到了，那么直接赋值；否则需要取反index后进行扩容，再进行赋值
- get(int key, E valueIfKeyNotFound)方法：也是使用二分查找法，找到返回，没找到返回参数中设置的默认值

特点：
- SparseArray比HashMap更省内存，在某些条件下性能更好，主要是因为它避免了对key的自动装箱（int转为Integer类型），它内部则是通过两个数组来进行数据存储的，一个存储key，另外一个存储value，为了优化性能，它内部对数据还采取了压缩的方式来表示稀疏数组的数据，从而节约内存空间。
- 线程不安全的，允许value为null。
- 不适合大容量的数据存储。存储大量数据时，它的性能将退化至少50%。


优点：
- 避免了基本数据类型的装箱拆箱操作
- 在数据量不大的情况下，查找效率较高（二分查找法）
- 延迟了垃圾回收的时机，只在需要的时候才一次进进行

劣势：
- 插入新元素可能会导致移动大量的数组元素
- 数据量较大时，查找效率（二分查找法）会明显降低

适用场景：
- 数据量不大（千以内）
- 空间比时间重要
- 需要使用Map，且key为int类型。


ArrayMap的使用：
```java
        ArrayMap<String,String> arrayMap = new ArrayMap<>();
        arrayMap.put(null,"张大哥");
        

        Set<ArrayMap.Entry<String,String>> sets = arrayMap.entrySet();
        for (ArrayMap.Entry<String,String> set : sets) {
            Log.d(TAG, "arrayMapSample: key = " + set.getKey() + ";value = " + set.getValue());
        }

```

ArrayMap实现原理：
ArrayMap利用两个数组，mHashes用来保存每一个key的hash值，mArrray大小为mHashes的2倍，依次保存key和value。
````java
mHashes[index] = hash;
mArray[index<<1] = key;
mArray[(index<<1)+1] = value;

````

当插入时，根据key的hashcode()方法得到hash值，然后同样利用二分查找找到对应index，然后在index位置进行插入，当出现哈希冲突时，会在index的相邻位置插入。对应的value会在 index<<1 +1 位置插入；

总结：

假设数据量都在千级以内的情况下：
1、如果key的类型已经确定为int类型，那么使用SparseArray，因为它避免了自动装箱的过程，如果key为long类型，它还提供了一个LongSparseArray来确保key为long类型时的使用

2、如果key类型为其它的类型，则使用ArrayMap。

## 参考博客
- [SparseArray 源码解析](https://juejin.im/post/6844903843751264270)
