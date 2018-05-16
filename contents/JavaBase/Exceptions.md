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


#### %4、try-catch-finally中，如果在catch中return了，finally中的代码还会执行么，原理是什么？


#### ！5、列举3个以上的RuntimeException


#### ！6、Java中的异常处理机制的简单原理和应用


#### 参考资料


>Contributes: hueizhe
>
>Reviewers : Hollis, Kevin Lee
