### 其它

---

##### ！1、String和StringBuffer、StringBuilder的区别

- 属性

  String 为字符串常量，是不可变类；

  StringBuffer 和 StringBuilder 为字符串变量，是可变类；

- 线程安全

  String 线程安全

  StringBuffer 线程安全

  StringBuilder 线程不安全

- 速度

  String  <  StringBuffer  <  StringBuilder

  String：对 String 对象的操作实际上在不断创建新的对象并且回收旧对象，执行速度很慢；并且通过查看反编译后的字节码，对 String 对象进行加操作，实际上会自动被 JVM 优化成 StringBuilder 对象的 append 操作，并通过 toString 方法返回 String 对象；

  StringBuffer 和 StringBuilder：因为 StringBuffer 是线程安全的，所以在速度上 StringBuffer < StringBuilder；

总结：

​	String：适用于少量的字符串操作的情况

​	StringBuilder：适用于单线程下在字符缓冲区进行大量操作的情况

​	StringBuffer：适用多线程下在字符缓冲区进行大量操作的情况

##### ！2、==和equals的区别

在栈中，基本类型存储的是值，引用类型存储的是地址，指向堆中的实例；

无论是基本类型还是引用类型，“== ” 比较的都是值，引用类型的值实际上是地址，所以 “== ” 比较的是值实际上是地址

- 未重写 equals

  不重写 equals ，那么实际上 equals 是继承自 Object 类中的 equals 方法，Object 中的equals 方法也只是返回 “== ” 的结果， 此时 == 和 equals 是没有区别的。

- 重写 equals

  重写 equals 后，可以根据自己的需要确定 equals 比较的内容，一般都是比较两个对象的值。此时 “== ” 比较的是地址，equals 比较的是自定义的比较内容；

  例子：在 String 中，equals 已经被重写了，并且比较的是 String 对象所表示的字符串常量，所以 equals 比较的是 String 所指的常量值，“==” 比较的仍然是地址；

##### %3、hashCode的作用，和equals方法的关系

- hashCode的作用
  1. hashCode 方法返回对象的哈希码值，在 Object 中，返回对象地址 hash 之后的结果，自定义对象如果不重写 hashCode 方法，默认使用 Object 对象的 hashCode 方法，返回对象地址经过哈希函数计算之后的结果；
  2. .hashCode是为了提高在散列结构存储中查找的效率，如Hashtable，HashMap等，hashCode是用来在散列存储结构中确定对象的存储地址的； 
- hashCode() 与 equals() 的关系
  1. 若重写了 equals() 方法，则有必要重写 hashCode() 方法；
  2. 若两个对象 equals() 返回 true，则 hashCode() 有必要也返回相同的 int 数；
  3. 若两个对象 equals() 返回 false，则 hashCode() 不一定返回不同的 int 数；
  4. 若两个对象 hashCode() 返回相同的 int 数，则 equals() 不一定返回 true；
  5. 若两个对象 hashCode() 返回不同的 int 数，则 equals() 一定返回 false； 
  6. 同一对象在执行期间若已经存储在集合中，则不能修改影响 hashCode 值的相关信息，否则会导致内存泄露问题

##### ！4、Input/OutputStream和Reader/Writer有什么区别

``` bash
（1）Reader和Writer类（文本字符流读写类）：提供的对字符流处理的类，它们为抽象类。一般通过其子类来实现。
（2）InputStreamReader(InputStream in) 和OutputStreamWriter(OutputStream out)：它们可以使用指定的编码规范并基于字节流生成对应的字符流。
（3）BufferedReader(InputStreamReader isr, int size) 和 BufferedWriter(OutputStreamWriter osr, int size)：为提高字符流的处理效率，可以采用缓冲机制的流实现对字符流作成批的处理，避免了频繁的从物理设备中读取信息 。
```

##### ！5、如何在字符流和字节流之间转换？

```bash
- InputStreamReader 是字符流Reader的子类，是字节流通向字符流的桥梁。 你可以在构造器重指定编码的方式，如果不指定的话将采用底层操作系统的默认编码方式，例如 GBK 等 
- OutputStreamWriter 是字符流Writer的子类，是字符流通向字节流的桥梁。 每次调用 write() 方法都会导致在给定字符（或字符集）上调用编码转换器。在写入底层输出流之前，得到的这些字节将在缓冲区中累积
```

##### ！6、switch可以使用那些数据类型

- JDK1.6以前5种， byte、char、short、int、枚举 ，JDK1.7 增加String 共六种

- 在JDK1.5之前,switch循环只支持byte short char int四种数据类型.

  JDK1.5 在switch循环中增加了枚举类与byte short char int的包装类,对四个包装类的支持是因为java编译器在底层手动进行拆箱,而对枚举类的支持是因为枚举类有一个ordinal方法,该方法实际上是一个int类型的数值.

  jdk1.7开始支持String类型,但实际上String类型有一个hashCode算法,结果也是int类型.而byte short char类型可以在不损失精度的情况下向上转型成int类型.所以总的来说,可以认为switch中只支持int.

##### %7、Java的四种引用

- 强引用（Strong Reference）

  我们平时所使用的引用就是强引用。 A a = new A(); 也就是通过关键字new创建的对象所关联的引用就是强引用。 只要强引用存在，该对象永远也不会被回收。

- 软引用（Soft Reference）

  只有当堆即将发生OOM异常时，JVM才会回收软引用所指向的对象。 软引用通过SoftReference类实现。 软引用的生命周期比强引用短一些。

- 弱引用（Weak Reference）

  只要垃圾收集器运行，软引用所指向的对象就会被回收。 弱引用通过WeakReference类实现。 弱引用的生命周期比软引用短。

- 虚引用（Phantom Reference）

  虚引用也叫幽灵引用，它和没有引用没有区别，无法通过虚引用取得一个对象实例。 一个对象关联虚引用唯一的作用就是在该对象被垃圾收集器回收之前会受到一条系统通知。 虚引用通过PhantomReference类来实现。

##### ！8、序列化与反序列化

- Java序列化与反序列化 

  ```bash
  Java序列化是指把Java对象转换为字节序列的过程；而Java反序列化是指把字节序列恢复为Java对象的过程。
  ```

- 为什么需要序列化与反序列化 

  ```bash
  在两个进程进行通信时，实现进程之间的对象传递；实现数据持久化，把数据永久地保存到硬盘上（通常存放在文件里）。
  ```

- 如何实现Java序列化与反序列化 

  ```bash
  - 只要一个类实现了java.io.Serializable接口，那么它就可以被序列化。
  - 通过ObjectOutputStream和ObjectInputStream对对象进行序列化及反序列化
  ```

- 相关类与接口

  ```bash
  - java.io.Serializable接口：标识可序列化的语义
  - java.io.Externalizable接口：继承了Serializable接口 需要重写 writeExternal()与readExternal()方法
  - ObjectOutput接口与ObjectInput接口
  - ObjectOutputStream类与ObjectInputStream类
  ```

##### ！9、正则表达式

​	调研了一下，正则表达式的面试题绝大多数都是以实际的题目,网上的相关学习教程也有很多，这里就不再赘述，仅仅以一片相关面试题为引，作为参考，地址：[正则表达式学习笔记](https://www.jianshu.com/p/42a5e07063be)

##### ！10、int和Integer的区别，什么是自动装箱和自动拆箱

- 区别：

  ```bash
  1. int是基本数据类型，Integer是int的包装类。
  2. int的默认值是0，Integerde默认值是null。
  3. int可以直接使用，Integer必须实例化才能使用。
  4. int指向实际值，Integer指向的是Integer对象。
  ```

- 自动装箱和自动拆箱：装箱就是Java把基本数据类型转变为其包装类，编译器通过调用包装类型的valueOf()方法实现自动装箱，调用xxxValue()方法自动拆箱。

##### 参考资料

- [Java基础之int和Integer有什么区别 ](https://blog.csdn.net/chenliguan/article/details/53888018)
- [InputStreamReader和OutputStreamWriter的用法](https://blog.csdn.net/zmx729618/article/details/51425955)
- [Java中switch都可以支持哪些数据类型](https://blog.csdn.net/qq_33811662/article/details/79069805)
- [Java序列化与反序列化](https://blog.csdn.net/wangloveall/article/details/7992448)
- [从一道面试题彻底搞懂hashCode与equals的作用与区别及应当注意的细节](https://blog.csdn.net/haobaworenle/article/details/53819838)
- [Java中hashCode的作用](https://blog.csdn.net/fenglibing/article/details/8905007)


>Contributes: Leo, zhangyue, 木
>
>Reviewers : Hollis, Kevin Lee
