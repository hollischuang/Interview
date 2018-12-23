# 线程

## 1.创建线程的方式及实现
Java多线程实现方式主要有四种：
* 继承Thread类
* 实现Runnable接口 
* 实现Callable接口通过FutureTask包装器来创建Thread线程 
* 使用ExecutorService、Callable、Future实现有返回结果的多线程。

其中前两种方式线程执行完后都没有返回值，后两种是带返回值的。

#### 1、继承Thread类创建线程
   Thread类本质上是实现了Runnable接口的一个实例，代表一个线程的实例。
   
   启动线程的唯一方法就是通过Thread类的start()实例方法。
   
   start()方法是一个native方法，它将启动一个新线程，并执行run()方法。
   
   这种方式实现多线程很简单，通过自己的类直接extend Thread，并复写run()方法，就可以启动新线程并执行自己定义的run()方法。例如：
~~~j
public class MyThread extends Thread {  
　　public void run() {  
　　 System.out.println("MyThread.run()");  
　　}  
}  
 
MyThread myThread1 = new MyThread();  
MyThread myThread2 = new MyThread();  
myThread1.start();  
myThread2.start();
~~~

#### 2、实现Runnable接口创建线程
   如果自己的类已经extends另一个类，就无法直接extends Thread，此时，可以实现一个Runnable接口，如下：
   ~~~java
   public class MyThread extends OtherClass implements Runnable {  
   　　public void run() {  
   　　 System.out.println("MyThread.run()");  
   　　}  
   }  
   ~~~
   为了启动MyThread，需要首先实例化一个Thread，并传入自己的MyThread实例：
   ~~~
   MyThread myThread = new MyThread();  
   Thread thread = new Thread(myThread);  
   thread.start();  
   ~~~
####   3、实现Callable接口通过FutureTask包装器来创建Thread线程
~~~java
public interface Callable<V>   {  V call() throws Exception;   } 
~~~
~~~java
public class SomeCallable<V> extends OtherClass implements Callable<V> {
    @Override
    public V call() throws Exception {
        return null;
    }
}
~~~

~~~
Callable<V> oneCallable = new SomeCallable<V>();   
//由Callable<Integer>创建一个FutureTask<Integer>对象：   
FutureTask<V> oneTask = new FutureTask<V>(oneCallable);   
//注释：FutureTask<Integer>是一个包装器，它通过接受Callable<Integer>来创建，它同时实现了Future和Runnable接口。 
//由FutureTask<Integer>创建一个Thread对象：   
Thread oneThread = new Thread(oneTask);   
oneThread.start();   
//至此，一个线程就创建完成了。
~~~
#### 4、使用ExecutorService、Callable、Future实现有返回结果的线程

ExecutorService、Callable、Future三个接口实际上都是属于Executor框架。

返回结果的线程是在JDK1.5中引入的新特征，有了这种特征就不需要再为了得到返回值而大费周折了。而且自己实现了也可能漏洞百出。

可返回值的任务必须实现Callable接口。类似的，无返回值的任务必须实现Runnable接口。

执行Callable任务后，可以获取一个Future的对象，在该对象上调用get就可以获取到Callable任务返回的Object了。

注意：get方法是阻塞的，即：线程无返回结果，get方法会一直等待。

再结合线程池接口ExecutorService就可以实现传说中有返回结果的多线程了。

下面提供了一个完整的有返回结果的多线程测试例子，在JDK1.5下验证过没问题可以直接使用。代码如下：
~~~java
import java.util.concurrent.*;  
import java.util.Date;  
import java.util.List;  
import java.util.ArrayList;  
  
/** 
* 有返回值的线程 
*/  
@SuppressWarnings("unchecked")  
public class Test {  
public static void main(String[] args) throws ExecutionException,  
    InterruptedException {  
   System.out.println("----程序开始运行----");  
   Date date1 = new Date();  
  
   int taskSize = 5;  
   // 创建一个线程池  
   ExecutorService pool = Executors.newFixedThreadPool(taskSize);  
   // 创建多个有返回值的任务  
   List<Future> list = new ArrayList<Future>();  
   for (int i = 0; i < taskSize; i++) {  
    Callable c = new MyCallable(i + " ");  
    // 执行任务并获取Future对象  
    Future f = pool.submit(c);  
    // System.out.println(">>>" + f.get().toString());  
    list.add(f);  
   }  
   // 关闭线程池  
   pool.shutdown();  
  
   // 获取所有并发任务的运行结果  
   for (Future f : list) {  
    // 从Future对象上获取任务的返回值，并输出到控制台  
    System.out.println(">>>" + f.get().toString());  
   }  
  
   Date date2 = new Date();  
   System.out.println("----程序结束运行----，程序运行时间【"  
     + (date2.getTime() - date1.getTime()) + "毫秒】");  
}  
}  
  
class MyCallable implements Callable<Object> {  
private String taskNum;  
  
MyCallable(String taskNum) {  
   this.taskNum = taskNum;  
}  
  
public Object call() throws Exception {  
   System.out.println(">>>" + taskNum + "任务启动");  
   Date dateTmp1 = new Date();  
   Thread.sleep(1000);  
   Date dateTmp2 = new Date();  
   long time = dateTmp2.getTime() - dateTmp1.getTime();  
   System.out.println(">>>" + taskNum + "任务终止");  
   return taskNum + "任务返回运行结果,当前任务时间【" + time + "毫秒】";  
}  
}  
~~~
## 2. sleep() , join() , yield() 有什么区别


## 3. CountDownLatch 原理


## 4 .CyclicBarier 原理


## 5. Semaphore 原理


## 6. Exchanger原理


## 7. CountDownLatch  与 CyclicBarier 区别


## 8.ThreadLocal 的原理分析

ThreadLocal类的作用是为每个线程都创建一个变量副本, 每个线程都可以修改自己所拥有的变量副本, 而不会影响其他线程的副本. 其实这也是解决线程安全的问题的一种方法

ThreadLocal是如何做到为每一个线程维护变量的副本的呢？其实实现的思路很简单：在ThreadLocal类中有一个Map，用于存储每一个线程的变量副本。

看看get()函数的源码：
~~~java
public T get() {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null) {
            ThreadLocalMap.Entry e = map.getEntry(this);
            if (e != null)
                return (T)e.value;
        }
        return setInitialValue();
    }
~~~

getMap(t);函数传入thread参数，获取到thread的map。然后调用map的getEntry()函数，得到最终的entry。

Map中元素的键为threadlocal对象，而值对应线程的变量副本。




---
ThreadLocal并不能替代同步机制，两者面向的问题领域不同。

同步机制是为了同步多个线程对相同资源的并发访问，是为了多个线程之间进行通信的有效方式；

而ThreadLocal是隔离多个线程的数据共享，从根本上就不在多个线程之间共享资源（变量），这样当然不需要对多个线程进行同步了。

所以，如果你需要进行多个线程之间进行通信，则使用同步机制；

如果需要隔离多个线程之间的共享冲突，可以使用ThreadLocal，这将极大地简化你的程序，使程序更加易读、简洁

[ThreadLocal工作原理](https://blog.csdn.net/imzoer/article/details/8262101)


## 9.线程池的实现原理


## 10 .线程的生命周期


## 11.锁机制


## 12. 线程的安全问题


## 13.volatile 实现原理


## 14.Synchronize 实现原理


## 15.Synchronized 与lock 的区别


## 16.CAS 乐观锁


## 17. ABA 的问题


## 18. 乐观锁的业务场景及实现方式


