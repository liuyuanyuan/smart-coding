## Java集合篇

#### 参考(based jdk1.8)：

[Java Platform Standard Edition 8 Documentation](https://docs.oracle.com/javase/8/docs/)



#### 何为集合框架？

集合框架是用于表示和操作集合的统一体系结构。所有集合框架都包含以下内容：

- **接口（Interfaces）**：这些是代表集合的抽象数据类型。接口允许独立于其表示的细节来操纵集合。在面向对象的语言中，接口通常形成层次结构。
- **实现（Implementations）**：这些是collection接口的具体实现。本质上，它们是可重用的数据结构。
- **算法（Algorithms）**：

#### 集合框架的效益

- 减少编程工作量：

- 提高程序速度和质量:

- 允许无关 APIs 之间的相互操作:

- 减少学习和使用新APIs的精力:

- 减少设计新APIs的精力:

- 促进软件复用:

#### 核心集合接口（interface）

![image-20200131201445632](/Users/liuyuanyuan/github/StrongCode/java/images/collection-interfaces.png)

- Collection - 集合层次架构的根；表示一组对象（称作元素）的集合。

- List - 有序集合（有时称为序列）。列表可以包含重复的元素。

- Set - 不能包含重复元素的集合。

  - SortedSet(升序Set) - 按升序管理元素的Set。

- Queue - 用于在处理之前保存多个元素的集合。

  队列通常但不一定以FIFO（先进先出）的方式对元素进行排序。

- Deque - 用于在处理之前容纳多个元素的集合。

  Deque可同时用作FIFO（先进先出）和LIFO（先进先出）。

  

- Map -  将键（key）映射到值（value）的对象。映射不能包含重复的键；每个键最多可以映射到一个值。

  - SortedMap（升序Map） —一个按键升序维护其映射的Map。

  

要实现集合元素的排序和不可重复，元素对象必须正确实现 [equals() 和 hashCode() 方法](https://howtodoinjava.com/java/basics/java-hashcode-equals-methods/) 。



#### 核心集合的实现（Implements）

通用实现

| Interfaces | Hash table Implementations | Resizable array Implementations | Tree Implementations    | Linked list Implementations | Hash table + Linked list Implementations | thread-safe implementations |
| ---------- | -------------------------- | ------------------------------- | ----------------------- | --------------------------- | ---------------------------------------- | --------------------------- |
| `Set`      | `HashSet`                  |                                 | `TreeSet`               |                             | `LinkedHashSet`                          |                             |
| `List`     |                            | `ArrayList`                     |                         | `LinkedList`                |                                          | Vector                      |
| `Queue`    |                            |                                 |                         |                             |                                          |                             |
| `Deque`    |                            | `ArrayDeque`                    |                         | `LinkedList`                |                                          |                             |
| `Map`      | `HashMap`                  |                                 | `TreeMap` （SortedMap） |                             | `LinkedHashMap`                          | Hashtable                   |

并发实现

| Concurrent Interfaces          | Concurrent Implementation(thread-safe) |      |
| ------------------------------ | -------------------------------------- | ---- |
| BlockingQueue  (extends Queue) |                                        |      |
| ConcurrentMap  (extends Map)   | `ConcurrentHashMap`                    |      |
|                                |                                        |      |

#### 集合的综述

- List（元素有序）

  - Arraylist: 可增长的Object数组；（get和set块，插入和删除慢）

  - LinkedList: 双向链表(jdk1.6之前为循环链表，jdk1.7取消了循环) ；（插入和删除快，但不容易作索引）
  - Vector（**线程安全的**）: 可增长的Object数组；

- [Map](https://docs.oracle.com/javase/tutorial/collections/implementations/map.html)（元素为键值对，键唯一）

  **常规用途实现**

  - [`HashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html):  jdk1.8之前由**数组+链表**组成，数组是HashMap的主体，链表则主要为了解决哈希冲突而存在的(**“拉链法”**解决冲突)。jdk1.8以后在解决哈希冲突时有了较大的变化，当链表长度大于阈值(默认为8)时，将**链表转化为红黑树**，以减少搜索时间。**性能达到最大化。**

  - [`LinkedHashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedHashMap.html):  继承自 HashMap，所以它的底层仍然是基于拉链式散列结构，即由数组和链表或红黑树组成。另外，LinkedHashMap 在上面结构的基础上，增加了一条双向链表，使得上面的结构可以保持键值对的插入顺序。同时通过对链表进行相应的操作，实现了访问顺序相关逻辑。**近乎HashMap的性能和插入顺序迭代。**

  - [`TreeMap`](https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html) :  红黑树(自平衡的排序二叉树)。**实现了SortedMap接口方法，key按升序排列**

  - HashTable（**线程安全的**）:  和jdk1.8之前的 HashMap 的底层数据结构类似，都是采用**数组+链表**的形式，数组是主体，链表则主要为了解决哈希冲突而存在的。

  **并发实现  **

  - [`ConcurrentHashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentHashMap.html) 实现了 [`java.util.concurrent`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/package-summary.html) 包中的  [`ConcurrentMap`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentMap.html) 接口 （该接口继承了 `Map` 的原子方法 `putIfAbsent`, `remove` 和 `replace` ）。

    ConcurrentHashMap是基于哈希表的高度并发的高性能实现。此实现在执行检索时不会阻塞，并允许客户端选择用于更新的并发级别。它旨在替代Hashtable：除了实现ConcurrentMap外，它还支持Hashtable特有的所有传统方法。同样，如果您不需要旧版操作，请小心使用ConcurrentMap接口对其进行操作。

  **特殊用途实现**

  - [`EnumMap`](https://docs.oracle.com/javase/8/docs/api/java/util/EnumMap.html) : EnumMap 在内部实现为数组，是一种用于枚举键的高性能Map实现。此实现将Map接口的丰富性和安全性与接近数组的速度结合在一起。如果要将枚举映射到值，则应始终使用EnumMap优先于数组。

  - [`WeakHashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/WeakHashMap.html) : WeakHashMap 是Map接口的实现，该接口仅存储对其键的弱引用。当不再在WeakHashMap外部引用键值对时，仅存储弱引用将允许对键值对进行垃圾回收。此类提供了利用弱引用功能的最简单方法。这对于实现“类似注册表的”数据结构非常有用，在这种结构中，当任何线程都无法再访问其键时，该条目的实用程序就会消失.

  - [`IdentityHashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/IdentityHashMap.html) : IdentityHashMap 是基于哈希表的基于身份的Map实现。此类对于保留拓扑的对象图转换（例如序列化或深度复制）很有用。要执行这样的转换，您需要维护一个基于身份的“节点表”，以跟踪已经看到的对象。基于身份的映射还用于在动态调试器和类似系统中维护对象到元信息的映射。最后，基于身份的映射在阻止“欺骗攻击”中很有用，这是故意不正当的equals方法的结果，因为IdentityHashMap从未在其键上调用equals方法。此实现的另一个好处是它速度很快。

- Set（元素不重复）

  - HashSet(无序，唯一)：基于 HashMap 实现的，底层采用HashMap来保存元素；

- LinkedHashSet：LinkedHashSet 继承于 HashSet，并且其内部是通过 LinkedHashMap 来实现的。有点类似于我们之前说的 LinkedHashMap ，其内部是基于 HashMap 实现一样，不过还是有一点点区别的。

  - TreeSet(有序，唯一)：红黑树(自平衡的排序二叉树)。

- Queue

- Deque

#### 集合的遍历

- 使用 for循环（适用于简单操作）：

  ```java
  //for循环在简单操作时，比以下等效的forEach代码更简洁
  for (Person p : roster) {
      System.out.println(p.getName());
  }
  //等效的forEach
  roster
      .stream()
      .forEach(e -> System.out.println(e.getName());          
  ```

- 使用聚合操作 forEach（适用于复杂操作）：

  ```
  //forEach比以下等效的for循环代码更简洁
  roster
      .stream()
      .filter(e -> e.getGender() == Person.Sex.MALE)
      .forEach(e -> System.out.println(e.getName()));      
  <==>
  for (Person p : roster) {
      if (p.getGender() == Person.Sex.MALE) {
          System.out.println(p.getName());
      }
  }
  ```

​       [聚合操作](https://docs.oracle.com/javase/tutorial/collections/streams/index.html)

Pipelines - A *pipeline* is a sequence of aggregate operations. The following example prints the male members contained in the collection `roster` with a pipeline that consists of the aggregate operations `filter` and `forEach`:

A pipeline contains the following components:

- A source: This could be a collection, an array, a generator function, or an I/O channel. In this example, the source is the collection `roster`.

- Zero or more *intermediate operations*. An intermediate operation, such as `filter`, produces a new stream.

  A *stream* is a sequence of elements. Unlike a collection, it is not a data structure that stores elements. Instead, a stream carries values from a source through a pipeline. This example creates a stream from the collection `roster` by invoking the method `stream`.

  The `filter` operation returns a new stream that contains elements that match its predicate (this operation's parameter). In this example, the predicate is the lambda expression `e -> e.getGender() == Person.Sex.MALE`. It returns the boolean value `true` if the `gender` field of object `e` has the value `Person.Sex.MALE`. Consequently, the `filter` operation in this example returns a stream that contains all male members in the collection `roster`.

- A *terminal operation*. A terminal operation, such as `forEach`, produces a non-stream result, such as a primitive value (like a double value), a collection, or in the case of `forEach`, no value at all. In this example, the parameter of the `forEach` operation is the lambda expression `e -> System.out.println(e.getName())`, which invokes the method `getName` on the object `e`. (The Java runtime and compiler infer that the type of the object `e` is `Person`.)

The following example calculates the average age of all male members contained in the collection `roster` with a pipeline that consists of the aggregate operations `filter`, `mapToInt`, and `average`:

```java
double average = roster
    .stream()
    .filter(p -> p.getGender() == Person.Sex.MALE)
    .mapToInt(Person::getAge)
    .average()
    .getAsDouble();
```

- 使用 Iterators 迭代器

  ```java
  for(Iterator<Person> it=roster.iterator(); it.hasNext();){
    	System.out.println(it.next().getName());
  }        
  ```

Aggregate operations, like `forEach`, appear to be like iterators. However, they have several fundamental differences:

- **They use internal iteration**: Aggregate operations do not contain a method like `next` to instruct them to process the next element of the collection. With *internal delegation*, your application determines *what* collection it iterates, but the JDK determines *how* to iterate the collection. With *external iteration*, your application determines both what collection it iterates and how it iterates it. However, external iteration can only iterate over the elements of a collection sequentially. Internal iteration does not have this limitation. It can more easily take advantage of parallel computing, which involves dividing a problem into subproblems, solving those problems simultaneously, and then combining the results of the solutions to the subproblems. See the section [Parallelism](https://docs.oracle.com/javase/tutorial/collections/streams/parallelism.html) for more information.
- **They process elements from a stream**: Aggregate operations process elements from a stream, not directly from a collection. Consequently, they are also called *stream operations*.
- **They support behavior as parameters**: You can specify [lambda expressions](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html) as parameters for most aggregate operations. This enables you to customize the behavior of a particular aggregate operation.



#### 1 序列：ArrayList、LinkedList

**线程安全：**都不是同步的，不保证线程安全。

**元素的插入删除：**

ArrayList是采用数组存储结构，所以add(E e)是在末尾追加，时间复杂度为O(1); add(int i, E e)和remove(int i)的时间复杂度就是O(n-i)，因为此时第i个和第i个元素之后的（n-i）个元素都要执行向后/向前移一位的操作。

LinkedList采用双向链表存储，所以插入、删除元素的时间复杂度都不受元素位置影响。

**元素的快速随机访问：**

 LinkedList 不支持高效的随机元素访问，而 ArrayList 支持。

快速随机访问就是通过元素的序号快速获取元素对象(对应于get(int index)方法)。实现了RandomAccess接口类（该类仅用于标识，内部什么也没有）的就是支持快速随机访问的。

```java
/*
 * Marker interface used by {@code List} implementations to indicate that
 * they support fast (generally constant time) random access.  The primary
 * purpose of this interface is to allow generic algorithms to alter their
 * behavior to provide good performance when applied to either random or
 * sequential access lists.
 */
public interface RandomAccess {
}
```

**遍历方式:**

实现了RandomAccess接口的，优先选择for循环，其次foreach；

未实现RandomAccess接口的，优先选择iterator遍历(foreach遍历底层也是通过iterator实现的)；

**大size的数据，千万不要使用普通for循环。**

**内存空间占用：**

ArrayList的空间浪费主要是结尾会预留一定容量空间；

LinkedList每个元素需要存放直接前驱、直接后继及数据，这比ArrayList元素耗费更多空间。

##### **底层数据结构：**

**List** is An ordered collection (also known as a <i>sequence</i>).

**ArrayList** is Resizable-array implementation of the {@code List} interface.Implements all optional list operations, and permits all elements, including {@code null}.

```java
public class ArrayList<E> extends AbstractList<E> 
  implements List<E>, RandomAccess, Cloneable, java.io.Serializable{
  
 private static final int DEFAULT_CAPACITY = 10;
 public ArrayList(int initialCapacity) {
 		this.elementData = new Object[initialCapacity];
 ...}
 public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
 }
```

**LinkedList** is Doubly-linked list implementation of the {@code List} and {@code Deque} interfaces. Implements all optional list operations, and permits all elements (including {@code null}).

```java
public class LinkedList<E> extends AbstractSequentialList<E>
  implements List<E>, Deque<E>, Cloneable, java.io.Serializable{
  
	transient int size = 0;
	// Pointer to first node.
	transient Node<E> first;
	//Pointer to last node.
	transient Node<E> last;
```

**Vector** The {@code Vector} class implements a growable array of objects. Like an array, it contains components that can be accessed using an integer index. However, the size of a {@code Vector} can grow or shrink as needed to accommodate adding and removing items after the {@code Vector} has been created. 

```java
public class Vector<E> extends AbstractList<E>
    implements List<E>, RandomAccess, Cloneable, java.io.Serializable{
  
    /**
     * The array buffer into which the components of the vector are
     * stored. The capacity of the vector is the length of this array buffer,
     * and is at least large enough to contain all the vector's elements.
     *
     * <p>Any array elements following the last element in the Vector are null.
     *
     * @serial
     */
    protected Object[] elementData;
     /**
     * Constructs an empty vector so that its internal data array
     * has size {@code 10} and its standard capacity increment is zero.
     */
    public Vector() {
        this(10);
    }
```

>**Vector 与 ArrayList**
>
>底层都是数组实现的；
>
>ArrayList不是同步的，不能保证线程安全；Vector是同步的，可以保证线程安全，要保证同步需要额外的时间消耗。因此单线程时优先使用ArrayList，需要保证线程安全时只能用Vector。



> **数据结构基础之双向链表**
>
> 双向链表也叫双链表，是链表的一种，它的每个数据结点中都有两个指针，分别指向直接后继和直接前驱。所以，从双向链表中的任意一个结点开始，都可以很方便地访问它的前驱结点和后继结点。一般我们都构造双向循环链表，如下图所示，同时下图也是LinkedList底层使用的双向循环链表数据结构。
>
> ![image-20200122101924557](/Users/liuyuanyuan/github/StrongCode/java/images/linkedlist.png)



#### 2 映射：HashMap、Hashtable、ConcurrentHashMap

Java为数据结构中的映射定义了一个接口java.util.Map，此接口主要有四个常用的实现类，分别是HashMap、Hashtable、LinkedHashMap和TreeMap。

**定义：**

**HashMap**的底层是**数组和链表**结合起来使用，也就是**链表散列**。HashMap通过key.hashCode()经过扰动函数（即hash(Object key)）处理过后得到hash值，然后通过判断当前元素存放的位置(这里的n指的是数组的长度)；如果当前位置存在元素的话，就判断该元素与要存入的元素的hash值以及key是否相同，如果相同的话直接覆盖，不相同就通过拉链法解决冲突。

所谓**扰动函数**指的就是HashMap的hash(Object key)方法。使用hash方法是为了防止一些实现比较差的hashCode()方法，换句话说使用扰动函数之后可以减少碰撞（碰撞在这里意思是不同的key值，得到了相同的hash值）。

所谓**拉链法**指的就是将链表和数组相结合。也就是说创建一个链表数组，数组中每一格就是一个链表。若遇到哈希冲突，则将冲突的值加到链表中即可。



**线程安全**：

HashMap 非线程安全的；

ConcurrentHashMap是线程安全的，

Hashtable 是线程安全的，**它使用 synchronized 来保证线程安全，效率低下**。当一个线程访问同步方法时，其他线程也访问同步方法，可能会进入阻塞或轮询状态，如使用put添加元素，另一个线程不能使用put添加元素，也不能使用get，竞争越激烈效率越低。

**效率：**

因为保证线程安全会产生额外的消耗，因此效率上: 

​	HashMap  >  ConcurrentHashMap > Hashtable。

**对Null值的支持：**

HashMap支持null用作key；

Hashtable必须non-null用作key;

ConcurrentHashMap必须non-null用作key。

**初始容量和每次的扩充容量：**



**底层数据结构：**

jdk1.8之后的HashMap在解决哈希冲突时有较大变化，当链表长度大于阀值（默认为8）时，将链表转化为红黑树，以减少搜索时间。

> ConcurrentHashMap是高效并发的Hashtable；
>
> 因此，在需要保证线程安全时推荐使用ConcurrentHashMap，否则使用HashMap，基本不推荐使用Hashtable。



**ConcurrentHashMap的线程安全的底层实现：**



##### **底层数据结构**

**HashMap** is **Hash table based** implementation of the **Map** interface. This implementation provides all of the optional map operations, and **permits {@code null} values and the {@code null} key**.  (The {@code HashMap} class is roughly equivalent to {@code Hashtable}, except that it is unsynchronized and permits nulls.) This class makes no guarantees as tothe order of the map; in particular, it does not guarantee that the order will remain constant over time.

```java
public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable {
    // The default initial capacity - MUST be a power of two.
    static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
    // Returns a power of two size for the given target capacity.
    static final int tableSizeFor(int cap) {
        int n = -1 >>> Integer.numberOfLeadingZeros(cap - 1);
        return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
    }
 
    /* ---------------- Static utilities -------------- */
    /**
     * Computes key.hashCode() and spreads (XORs) higher bits of hash
     * to lower.  Because the table uses power-of-two masking, sets of
     * hashes that vary only in bits above the current mask will
     * always collide. (Among known examples are sets of Float keys
     * holding consecutive whole numbers in small tables.)  So we
     * apply a transform that spreads the impact of higher bits
     * downward. There is a tradeoff between speed, utility, and
     * quality of bit-spreading. Because many common sets of hashes
     * are already reasonably distributed (so don't benefit from
     * spreading), and because we use trees to handle large sets of
     * collisions in bins, we just XOR some shifted bits in the
     * cheapest possible way to reduce systematic lossage, as well as
     * to incorporate impact of the highest bits that would otherwise
     * never be used in index calculations because of table bounds.
     */
    static final int hash(Object key) {
        int h;
        /*
        * key.hashCode():返回散列值也就是hashcode
				* ^ :按位异或
				* >>>:无符号右移，忽略符号位；空位都以0补齐
				*/
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }

    /* ---------------- Fields -------------- */
    /**
     * The table, initialized on first use, and resized as
     * necessary. When allocated, length is always a power of two.
     * (We also tolerate length zero in some operations to allow
     * bootstrapping mechanics that are currently not needed.)
     */
    transient Node<K,V>[] table;
    // Holds cached entrySet(). Note that AbstractMap fields are used for keySet() and values().
    transient Set<Map.Entry<K,V>> entrySet;
    
```

![image-20200122140241123](/Users/liuyuanyuan/github/StrongCode/java/images/hashmap.png)



**ConcurrentHashMap** is a hash table supporting full concurrency of retrievals and high expected concurrency for updates. This class obeys the same functional specification as {@link java.util.Hashtable}, and includes versions of methods corresponding to each method of  {@code Hashtable}. However, even though all operations are thread-safe, retrieval operations do <em>not</em> entail locking, and there is <em>not</em> any support for locking the entire table in a way that prevents all access. This class is fully interoperable with {@code Hashtable} in programs that rely on its thread safety but not on its synchronization details.

```java
public class ConcurrentHashMap<K,V> extends AbstractMap<K,V>
    implements ConcurrentMap<K,V>, Serializable {
  
```

  

**Hashtable(注意table首字母小写)** class implements a hash table, which maps keys to values. **Any non-{@code null} object can be used as a key or as a value**. To successfully store and retrieve objects from a hashtable, the objects used as keys must implement the {@code hashCode} method and the {@code equals} method. 

```java
public class Hashtable<K,V> extends Dictionary<K,V>
    implements Map<K,V>, Cloneable, java.io.Serializable {
    // The hash table data.
    private transient Entry<?,?>[] table;
    // The total number of entries in the hash table.
    private transient int count;
```

 

**LinkedHashMap** is hash table and linked list implementation of the {@code Map} interface, with predictable iteration order. This implementation differs from {@code HashMap} in that it maintains a doubly-linked list running through all of its entries. This linked list defines the iteration ordering, which is normally the order in which keys were inserted into the map  (<i>insertion-order</i>). Note that insertion order is not affected if a key is <i>re-inserted</i> into the map. (A key {@code k} is reinserted into a map {@code m} if {@code m.put(k, v)} is invoked when {@code m.containsKey(k)} would return {@code true} immediately prior to the invocation.)

``` java
public class LinkedHashMap<K,V> extends HashMap<K,V>
    implements Map<K,V> {
    private static final long serialVersionUID = 3801124242820219131L;

    // The head (eldest) of the doubly linked list.
    transient LinkedHashMap.Entry<K,V> head;

    // The tail (youngest) of the doubly linked list.
    transient LinkedHashMap.Entry<K,V> tail;
  
```



**TreeSet** is a Red-Black tree based {@link NavigableMap} implementation. The map is sorted according to the {@linkplain Comparable natural ordering} of its keys, or by a {@link Comparator} provided at map creation time, depending on which constructor is used.

```
public class TreeMap<K,V> extends AbstractMap<K,V>
    implements NavigableMap<K,V>, Cloneable, java.io.Serializable
{

```

 
