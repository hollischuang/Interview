### 集合相关

---

#### ！1、列举几个Java中Collection类库中的常用类

- Set接口：特点：没有重复元素，可以有最多一个null，无序
  - HashSet

    ```bash
    - 实现：基于HashMap
    - 特点：没有顺序，用equals()和hashCode()协同判断元素是否存在
    ```

  - LinkedHashSet

    ```bash
    - 实现：基于LinkedHashMap
    - 特点：有顺序(按照插入顺序
    ```

  - SortedSet接口

    ```bash
    特点：有顺序(按照元素的大小排序)
    ```

  - TreeSet

    ```bash
    - 实现：基于TreeMap
    - 特点：排过序的，如果里面的元素是自定义类型，需要指定排序方式
    ```

  - ConcurrentSkipListSet

    ```bash
    - 实现：基于ConcurrentSkipListMap
    - 特点：线程安全，支持并发，有排序(可以手动设置)
    ```

- List接口

  - LinkedList

    ```bash
    - 实现：基于双链表，元素由Entry对象维护
    - 特点：随机访问性能差，插入删除性能好，非线程安全
    ```

  - ArrayList

    ```bash
    - 实现：基于数组，初始长度10，扩展长度1.5倍+1
    - 特点：随机访问性能好，插入删除性能差，非线程安全
    ```

  - Vector

    ```bash
    - 实现：基于数组，初始长度10(可以手动设置)，扩展长度2倍(可以手动设置)
    - 特点：线程安全
    ```

#### ！2、List、Set、Map是否都继承自Collection接口？存储特点分别是什么？

- List 保证以某种特定插入顺序来维护元素顺序，即保持插入的顺序，另外元素可以重复
- Set 维持它自己的内部排序，随机访问不具有意义。另外元素不可重复。
- Map 保存key-value值，value可多值,key可为null。

#### ！3、ArrayList、LinkedList和Vector之间的区别与联系

- 联系：

  ```bash
  三者都来自于List，其中vector，ArrayList底层实现是数组，而LinkedList的底层实现是单链表。
  ```

- 三者各自特点：

  ```bash
  1.Vector，ArrayList查找快，增删慢。LinkedList相反，查找慢，增删快；
  2.vector是线程安全的。ArrayList，LinkedList是线程不安全的；
  ```

#### ！4、HashMap和Hashtable、TreeMap以及ConcurrentHashMap的区别

- HashTable和HashMap及ConcurrentHashMap是基于散列表实现的，但HashTable（线程安全）已被废弃,大多数场合不被建议使用，建议使用ConcurrentHashMap替代HashTable;TreeMap基于红黑树;
- HashMap适用于增删改查，TreeMap适用于遍历排序；HashMap是无序的，TreeMap实现了NavigableMap接口，支持一系列导航方法，是有序的；
- ConcurrentHashMap被用于代替HashTable,在JDK1.7版本中，ConcurrentHashMap采用了分段锁的设计，相比较于HashTable，拥有更高的性能，但相对应的,ConcurrentHashMap放弃了全局锁，使用了局部锁，使其无法提供强一致性的保障，如果有强一致性的需求，那么仍然需要是否使用HashTable；

#### ！5、Collection 和 Collections的区别

- Collection是集合类的上级接口，继承与他有关的接口主要有List和Set;
- Collections是针对集合类的一个帮助类，他提供一系列静态方法实现对各种集合的搜索、排序、线程安全等操作;常用的方法有sort，SynchronizedMap；

#### %6、其他的集合类：treeset,linkedhashmap等。

- treeset

  ```bash
  - TreeSet主要可用于排序，TreeSet是SortedSet的实现类；
  - 如果试图把一个对象添加到TreeSet时，则该对象的类必须实现Comparable接	口，否则程序会抛出异常java.lang.ClassCastException
  - 原因：TreeSet集合中添加两个Err对象，添加第一个对象时，TreeSet里没有	任何元素，所以不会出现任何问题；当添加第二个Err对象时，TreeSet就会	  调用该对象的compareTo(Object obj)方法与集合中的其他元素进行比较— 	  如果其对应的类没有实现Comparable 接口，则会引发ClassCastException 	  异常。
  - TreeSet判断两个对象是否相等的唯一标准是：
  	两个对象通过compareTo(Object o)方法比较是否返回0 —如果返回0， 	TreeSet则会认为它们相等：否则就认为它们不相等。
  ```

- linkedhashmap

  ```bash
  	LinkedHashMap是HashMap的子类，拥有HashMap所有的特性。HashMap和双向链表即是LinkedHashMap，由于LinkedHashMap额外维护了一个双向列表，所以可以保证迭代的顺序（HashMap是无序的），根据链表中元素的顺序可以将LinkedHashMap分为：保持插入顺序的LinkedHashMap 和 保持访问顺序的LinkedHashMap，其中LinkedHashMap的默认实现是按插入顺序排序的。
  ```


#### 参考资料


>Contributes:  木
>
>Reviewers : Hollis, Kevin Lee
