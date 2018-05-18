### 异常相关

---

#### ！1、Error和Exception的区别

Error类和Exception类的父类都是throwable类，他们的区别是：

- Error类一般是指与虚拟机相关的问题，如系统崩溃，虚拟机错误，内存空间不足，方法调用栈溢等。对于这类错误的导致的应用程序中断，仅靠程序本身无法恢复和和预防，遇到这样的错误，建议让程序终止。
- Exception类表示程序可以处理的异常，可以捕获且可能恢复。遇到这类异常，应该尽可能处理异常，使程序恢复运行，而不应该随意终止异常。


#### ！2、异常的类型，什么是运行时异常

Exception类又分为运行时异常（Runtime Exception）和受检查的异常(Checked Exception ):

- 运行时异常;ArithmaticException,IllegalArgumentException，编译能通过，但是一运行就终止了，程序不会处理运行时异常，出现这类异常，程序会终止。

- 受检查的异常，要么用try-catch捕获，要么用throws字句声明抛出，交给它的父类处理，否则编译不会通过。

#### ！3、final、finally和finalize的区别

- final：用来修饰类，方法，变量等，被final修饰的类不可以继承扩展，变量不可以修改，方法不可以重写。是保证平台安全的必要手段。
- finally：表示代码块一定要被执行，一般用来关闭JDBC，保证Unlock锁等动作。但是也有例外，在finally执行之前强制退出进程，finally代码块不执行。
- finalize：在垃圾回收器回收对象之前调用的方法以回收特定的垃圾，现在已经不推荐使用，因为finalize执行具有不确定性，有可能造成严重后果。


#### %4、try-catch-finally中，如果在catch中return了，finally中的代码还会执行么，原理是什么？

#### ！5、列举3个以上的RuntimeException

```bash
NullPointerException - 空指针引用异常
ClassCastException - 类型强制转换异常。
IllegalArgumentException - 传递非法参数异常。
ArithmeticException - 算术运算异常
ArrayStoreException - 向数组中存放与声明类型不兼容对象异常
IndexOutOfBoundsException - 下标越界异常
NegativeArraySizeException - 创建一个大小为负数的数组错误异常
NumberFormatException - 数字格式异常
SecurityException - 安全异常
UnsupportedOperationException - 不支持的操作异常
```


#### ！6、Java中的异常处理机制的简单原理和应用


#### 参考资料


>Contributes: hueizhe, zhangyue
>
>Reviewers : Hollis, Kevin Lee
