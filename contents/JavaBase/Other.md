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

参考来源：

https://blog.csdn.net/haobaworenle/article/details/53819838

https://blog.csdn.net/fenglibing/article/details/8905007


##### ！4、Input/OutputStream和Reader/Writer有什么区别


##### ！5、如何在字符流和字节流之间转换？


##### ！6、switch可以使用那些数据类型

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


##### ！9、正则表达式

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


>Contributes: xxx
>
>Reviewers : Hollis, Kevin Lee
