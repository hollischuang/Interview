## Java基础

#### 抽象、封装、继承、多态
##### ！1、Java中实现多态的机制是什么，动态多态和静态多态的区别

##### ！2、接口和抽象类的区别，如何选择

##### ！3、Java能不能多继承，可不可以多实现

##### %4、Static Nested Class 和 Inner Class的不同

##### ！5、重载和重写的区别。

- 重载(Overload)

  定义：方法重载是指同一个类中的多个方法具有相同的名字，但这些方法具有不同的参数列表，即参数的数量或参数类型不能完全相同。返回值类型可以相同也可以不相同。无法以返回类型作为重载函数的区分标准

  规则：

  1. 参数具有不同的参数列表；
  2. 可以有不同的返回类型，只要参数列表不同就可以
  3. 可以有不同的访问修饰符；
  4. 可以抛出不同的异常；

- 重写(Override)：

  定义：方法重写是存在子父类之间的,又称为方法覆盖，子类定义的方法与父类中的方法具有相同的方法名字,相同的参数表和相同的返回类型 

  规则：

  1. 参数列表必须完全与被重写的方法相同；
  2. 返回类型必须与被重写的方法的返回类型相同；
  3. 访问修饰符的限制一定要大于被重写方法的访问修饰符（public>protected>default>private）
  4. 重写方法一定不能抛出新的检查异常或者比被重写方法声明更宽泛的检查性异常；

参考来源：https://www.cnblogs.com/upcwanghaibo/p/6527354.html



##### ！6、是否可以继承String类

不可以，因为 String 类是 final 修饰的类，而 final 修饰的类是不能被继承的。

##### ！7、构造器是否可被override?

##### ！8、public,protected,private的区别?

##### 参考资料

---

### 集合相关
##### ！1、列举几个Java中Collection类库中的常用类

##### ！2、List、Set、Map是否都继承自Collection接口？存储特点分别是什么？

##### ！3、ArrayList、LinkedList和Vector之间的区别与联系

##### ！4、HashMap和Hashtable、TreeMap以及ConcurrentHashMap的区别

##### ！5、Collection 和 Collections的区别

##### %6、其他的集合类：treeset,linkedhashmap等。

##### 参考资料

---

### 异常相关
##### ！1、Error和Exception的区别

##### ！2、异常的类型，什么是运行时异常

##### ！3、final、finally和finalize的区别

##### %4、try-catch-finally中，如果在catch中return了，finally中的代码还会执行么，原理是什么？

##### ！5、列举3个以上的RuntimeException

##### ！6、Java中的异常处理机制的简单原理和应用

##### 参考资料

---

### 其它
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

[关于不可变对象与并发的补充](https://www.cnblogs.com/itZhy/p/7669951.html)



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

##### ！8、序列化与反序列化

##### ！9、正则表达式

##### ！10、int和Integer的区别，什么是自动装箱和自动拆箱

##### 参考资料

---

> Contributors : Hollis、Kevin Lee
>
> Reviewers : Hollis、Kevin Lee
