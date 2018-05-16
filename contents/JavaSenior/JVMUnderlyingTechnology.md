### JVM底层技术

---

#### ！1、gc的概念，如果A和B对象循环引用，是否可以被GC？
在正式回答这个问题之前</b>先简单说说 Java运行时内存区，划分为线程私有区和线程共享区：

1. 线程私有区：
    * 程序计数器，记录正在执行的虚拟机字节码的地址；
    * 虚拟机栈：方法执行的内存区，每个方法执行时会在虚拟机栈中创建栈帧；
    * 本地方法栈：虚拟机的Native方法执行的内存区；
2. 线程共享区：
    * Java堆：对象分配内存的区域，这是垃圾回收的主战场；
    * 方法区：存放类信息、常量、静态变量、编译器编译后的代码等数据，另外还有一个常量池。当然垃圾回收也会在这个区域工作。


目前虚拟机基本都是采用<b>可达性算法</b>，

为什么不采用引用计数算法呢？下面就说说引用计数法是如何统计所有对象的引用计数的，再对比分析可达性算法是如何解决引用技术算法的不足。

先简单说说这两个算法：

* [引用计数算法](https://link.zhihu.com/?target=http%3A//www.memorymanagement.org/glossary/r.html%23reference.counting) ：
    每个对象有一个引用计数器，当对象被引用一次则计数器加1，当对象引用失效一次则计数器减1，对于计数器为0的对象意味着是垃圾对象，可以被GC回收。
    
* 可达性算法 (GC Roots Tracing)：从GC Roots作为起点开始搜索，那么整个连通图中的对象便都是活对象，对于GC Roots无法到达的对象便成了垃圾回收的对象，随时可被GC回收。

采用引用计数算法的</b>系统只需在每个实例对象创建之初，通过计数器来记录所有的引用次数即可。
而可达性算法，则需要再次GC时，遍历整个GC根节点来判断是否回收。下面通过一段代码来对比说明：
 ~~~java
 public class GcDemo {

    public static void main(String[] args) {
        //分为6个步骤
        GcObject obj1 = new GcObject(); //Step 1
        GcObject obj2 = new GcObject(); //Step 2

        obj1.instance = obj2; //Step 3
        obj2.instance = obj1; //Step 4

        obj1 = null; //Step 5
        obj2 = null; //Step 6
    }
}

class GcObject{
    public Object instance = null;
}
~~~
很多文章以及Java虚拟机相关的书籍，都会告诉你如果采用引用计数算法，上述代码中obj1和obj2指向的对象已经不可能再被访问，彼此互相引用对方导致引用计数都不为0，最终无法被GC回收，而可达性算法能解决这个问题。

但这些文章和书籍并没有真正从内存角度来阐述这个过程是如何统计的，很多时候大家都在相互借鉴、翻译，却也都没有明白。或者干脆装作讲明白，或者假定读者依然明白。 

其实很多人并不明白为什么引用计数法不为0，引用计数到底是如何维护所有对象引用的，可达性是如何可达的？ 

接下来结合实例，从Java内存模型以及数学的图论知识角度来说明，希望能让大家彻底明白该过程。

> 引用计数算法

如果采用的是引用计数算法：

![java-memory-model](https://pic3.zhimg.com/50/b9e5ecd5d1cfb1d045541c1f6e25e92c_hd.jpg)
 
再回到前面代码GcDemo的main方法共分为6个步骤：

Step1：GcObject实例1的引用计数加1，实例1的引用计数=1；

Step2：GcObject实例2的引用计数加1，实例2的引用计数=1；

Step3：GcObject实例2的引用计数再加1，实例2的引用计数=2；

Step4：GcObject实例1的引用计数再加1，实例1的引用计数=2； 

执行到Step 4，则GcObject实例1和实例2的引用计数都等于2。 接下来继续结果图：

![java-memory-model](https://pic2.zhimg.com/50/3c1246661a548f933cd886070f6c1af1_hd.jpg)

Step5：栈帧中obj1不再指向Java堆，GcObject实例1的引用计数减1，结果为1；
Step6：栈帧中obj2不再指向Java堆，GcObject实例2的引用计数减1，结果为1。

<p>到此，发现GcObject实例1和实例2的计数引用都不为0，那么如果采用的引用计数算法的话，那么这两个实例所占的内存将得不到释放，这便产生了内存泄露。</p>

> 可达性算法

这是目前主流的虚拟机都是采用GC Roots Tracing算法，比如Sun的Hotspot虚拟机便是采用该算法。 

该算法的核心算法是从GC Roots对象作为起始点，利用数学中图论知识，图中可达对象便是存活对象，而不可达对象则是需要回收的垃圾内存。

这里涉及两个概念，一是GC Roots，一是可达性。

那么可以作为<b>GC Roots的对象</b>（见下图）：

* 虚拟机栈的栈帧的局部变量表所引用的对象；
* 本地方法栈的JNI所引用的对象；
* 方法区的静态变量和常量所引用的对象；


关于<b>可达性</b>的对象，便是能与GC Roots构成连通图的对象，如下图：

![](https://pic4.zhimg.com/50/22f72b18415405c3e0207925a8de74fa_hd.jpg)
 
从上图，reference1、reference2、reference3都是GC Roots，

可以看出：
 
 * reference1-&gt; 对象实例1；<br> 
 * reference2-&gt;  对象实例2；<br>
 * reference3-&gt;  对象实例4；<br>
 * reference3-&gt;  对象实例4 -&gt;    对象实例6；
 
可以得出对象实例1、2、4、6都具有GC Roots可达性，也就是存活对象，不能被GC回收的对象。
 
而对于对象实例3、5直接虽然连通，但并没有任何一个GC Roots与之相连，这便是GC Roots不可达的对象， 这就是GC需要回收的垃圾对象。

到这里，相信大家应该能彻底明白引用计数算法和可达性算法的区别吧。
 
 再回过头来看看最前面的实例，GcObject实例1和实例2虽然从引用计数虽然都不为0，但从可达性算法来看，都是GC Roots不可达的对象。 
 
 > 总之，对于对象之间循环引用的情况，引用计数算法，则GC无法回收这两个对象，而可达性算法则可以正确回收。 


#### %2、jvm gc如何判断对象是否需要回收，有哪几种方式？


#### ！3、Java中能不能主动触发GC

答案是可以。

调用System.gc() 即可马上触发GC操作

但是一般不要主动去调用GC，System.gc主动进行垃圾回收时一个非常危险的动作。因为它要停止所有的响应，才能检查内存中是否有可回收的对象，这对一个应用系统风险极大。  
如果一个Web应用，所有的请求都会暂停，等待垃圾回收器执行完毕，若此时堆内存（Heap）中的对象少的话则可以接受，一旦对象较多（现在的Web项目越做越大，框架工具越来越多，加载到内存中的对象就更多了），这个过程非常耗时，可能是0.01秒，也可能是1秒，甚至可能是20秒，这就会严重影响到业务的正常运行。
#### ！4、JVM的内存结构，堆和栈的区别


#### ！5、JVM堆的分代


#### %6、Java中的内存溢出是什么，和内存泄露有什么关系
Java内存泄漏就是程序中动态分配内存给一些临时对象，但是对象不会被GC所回收，它始终占用内存。即被分配的对象可达但已无用,
Java内存溢出就是你要求分配的内存超出了系统能给你的，系统不能满足需求，于是产生溢出。
从定义上可以看出内存泄露是内存溢出的一种诱因，不是唯一因素。

一、内存泄漏的几种场景
1、长生命周期的对象持有短生命周期对象的引用

    这是内存泄露最常见的场景，也是代码设计中经常出现的问题。
    例如：在全局静态map中缓存局部变量，且没有清空操作，随着时间的推移，这个map会越来越大，造成内存泄露。

2、修改hashset中对象的参数值，且参数是计算哈希值的字段
 
     当一个对象被存储进HashSet集合中以后，就不能修改这个对象中的那些参与计算哈希值的字段，否则对象修改后的哈希值与最初存储进HashSet集合中时的哈希值就不同了，在这种情况下，即使在contains方法使用该对象的当前引用作为参数去HashSet集合中检索对象，也将返回找不到对象的结果，这也会导致无法从HashSet集合中删除当前对象，造成内存泄露。
 
3、机器的连接数和关闭时间设置
 
    长时间开启非常耗费资源的连接，也会造成内存泄露。

二、内存溢出的几种场景

1、堆内存溢出（outOfMemoryError：java heap space）
       在jvm规范中，堆中的内存是用来生成对象实例和数组的。
       如果细分，堆内存还可以分为年轻代和年老代，年轻代包括一个eden区和两个survivor区。
       当生成新对象时，内存的申请过程如下：
          a、jvm先尝试在eden区分配新建对象所需的内存；
          b、如果内存大小足够，申请结束，否则下一步；
          c、jvm启动youngGC，试图将eden区中不活跃的对象释放掉，释放后若Eden空间仍然不足以放入新对象，则试图将部分Eden中活跃对象放入Survivor区；
          d、Survivor区被用来作为Eden及old的中间交换区域，当OLD区空间足够时，Survivor区的对象会被移到Old区，否则会被保留在Survivor区；
          e、 当OLD区空间不够时，JVM会在OLD区进行full GC；
          f、full GC后，若Survivor及OLD区仍然无法存放从Eden复制过来的部分对象，导致JVM无法在Eden区为新对象创建内存区域，则出现”out of memory错误”：
                                   outOfMemoryError：java heap space
 
代码举例：
~~~java
/** 
 * 堆内存溢出 
 * 
 * jvm参数：-Xms5m -Xmx5m -Xmn2m -XX:NewSize=1m 
 * 
 */  
public class MemoryLeak {  
     
    private String[] s = new String[1000];  
   
    public static void main(String[] args) throws InterruptedException {  
        Map<String,Object> m =new HashMap<String,Object>();  
        int i =0;  
        int j=10000;  
        while(true){  
            for(;i<j;i++){  
                MemoryLeak memoryLeak = new MemoryLeak();  
                m.put(String.valueOf(i), memoryLeak);  
            }  
        }  
    }  
}  
~~~
2、方法区内存溢出（outOfMemoryError：permgem space）
       在jvm规范中，方法区主要存放的是类信息、常量、静态变量等。
       所以如果程序加载的类过多，或者使用反射、gclib等这种动态代理生成类的技术，就可能导致该区发生内存溢出，一般该区发生内存溢出时的错误信息为：
             outOfMemoryError：permgem space
 
举例：

    jvm参数：-XX:PermSize=2m -XX:MaxPermSize=2m  
    将方法区的大小设置很低即可，在启动加载类库时就会出现内存不足的情况  

 
 
3、线程栈溢出（java.lang.StackOverflowError）
       线程栈时线程独有的一块内存结构，所以线程栈发生问题必定是某个线程运行时产生的错误。
       一般线程栈溢出是由于递归太深或方法调用层级过多导致的。
       发生栈溢出的错误信息为：
              java.lang.StackOverflowError
 
代码举例：
~~~java
/** 
 * 线程操作栈溢出 
 * 
 * 参数：-Xms5m -Xmx5m -Xmn2m -XX:NewSize=1m -Xss64k 
 * 
 */  
public class StackOverflowTest {  
     
    public static void main(String[] args) {  
        int i =0;  
        digui(i);  
    }  
     
    private static void digui(int i){  
        System.out.println(i++);  
        String[] s = new String[50];  
        digui(i);  
    }  
  
}  
~~~

最后说一些建议：
* 使用字符串处理，避免使用String，应大量使用StringBuffer，每一个String对象都得独立占用内存一块区域
* 尽量少用静态变量，因为静态变量存放在永久代（方法区），永久代基本不参与垃圾回收
* 避免在循环中创建对象
* 开启大型文件或从数据库一次拿了太多的数据很容易造成内存溢出，所以在这些地方要大概计算一下数据量的最大值是多少，并且设定所需最小及最大的内存空间值。 

#### ！7、Java的类加载机制，什么是双亲委派


#### ！8、ClassLoader的类加载方式


#### 参考资料
[gc的概念，如果A和B对象循环引用，是否可以被GC](https://www.zhihu.com/question/21539353/answer/95667088?from=profile_answer_card)

>Contributes: hueizhe
>
>Reviewers : Hollis, Kevin Lee
