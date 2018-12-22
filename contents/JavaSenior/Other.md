### 其它

---

#### ？1、hashcode 有哪些算法
   * 加法Hash
   * 位运算Hash
   * 乘法Hash
   * 除法Hash
   * 查表Hash
   * 混合Hash

详见 [hash.md]()
#### ！2、反射的基本概念，反射是否可以调用私有方法
 * 定义： 主要是指程序可以访问，检测和修改它本身状态或行为的一种能力，并能根据自身行为的状态和结果，调整或修改应用所描述行为的状态和相关的语义

 * 可以调用私有方法
 ~~~
     Class c = Class.forName("User");  
     //获取id属性  
     Field idF = c.getDeclaredField("id");  
     //实例化这个类赋给o  
     Object o = c.newInstance();  
     //打破封装  
     idF.setAccessible(true); //使用反射机制可以打破封装性，导致了java对象的属性不安全。  
     //给o对象的id属性赋值"110"  
     idF.set(o, "110"); //set  
 ~~~
#### ！3、Java中范型的概念
   * 泛型程序设计（generic programming）是程序设计语言的一种风格或范式。

   * 泛型允许程序员在强类型程序设计语言中编写代码时使用一些以后才指定的类型，在实例化时作为参数指明这些类型。

   * 泛型的定义主要有以下两种：
        * 在程序编码中一些包含类型参数的类型，也就是说泛型的参数只可以代表类，不能代表个别对象。（这是当今较常见的定义）
        * 在程序编码中一些包含参数的类。其参数可以代表类或对象等等。（现在人们大多把这称作模板）
        不论使用那个定义，泛型的参数在真正使用泛型时都必须作出指明。
        
   * Java 泛型的参数只可以代表类，不能代表个别对象。由于Java泛型的类型参数之实际类型在编译时会被消除，所以无法在运行时得知其类型参数的类型，而且无法直接使用基本值类型作为泛型类型参数。Java编译程序在编译泛型时会自动加入类型转换的编码，故运行速度不会因为使用泛型而加快。
     
   * 由于运行时会消除泛型的对象实例类型信息等缺陷经常被人诟病，Java及JVM的开发方面也尝试解决这个问题，例如Java通过在生成字节码时添加类型推导辅助信息，从而可以通过反射接口获得部分泛型信息。通过改进泛型在JVM的实现，使其支持基本值类型泛型和直接获得泛型信息等。
     
   * Java允许对个别泛型的类型参数进行约束，包括以下两种形式（假设T是泛型的类型参数，C是一般类、泛类，或是泛型的类型参数）：
      * T实现接口I。
      * T是C，或继承自C
#### ？4、JVM启动参数，-Xms和 -Xmx

 |参数名称 |含义       |默认值               |说明 | 
 | ------- | --------- | ------------------- | ---------------------------------------------------- | 
 |-Xms     |初始堆大小 |物理内存的1/64(<1GB) |默认(MinHeapFreeRatio参数可以调整)空余堆内存小于40%时，JVM就会增大堆直到-Xmx的最大限制. |
 |-Xmx     |最大堆大小 |物理内存的1/4(<1GB)  |默认(MaxHeapFreeRatio参数可以调整)空余堆内存大于70%时，JVM会减少堆直到 -Xms的最小限制 |
 

####  %5、代理机制的实现

JAVA 代理实现
代理的实现分动态代理和静态代理，
* 静态代理的实现是对已经生成了的JAVA类进行封装。
* 动态代理则是在运行时生成了相关代理类，在JAVA中生成动态代理一般有两种方式:
##### 1. JDK自带实现方法

  JDK实现代理生成，是用类 java.lang.reflect.Proxy, 实现方式如下:

~~~java
public class JDKProxy {
      public static Object getPoxyObject(final Object c) {
            // JDK实现动态代理，但JDK实现必须需要接口
            return Proxy.newProxyInstance(c.getClass().getClassLoader(), c.getClass().getInterfaces(),
                        new InvocationHandler() {
                             public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                                    Object reObj = null;
                                    System.out.print("you say: ");
                                    reObj = method.invoke(c, args);
                                    System.out.println(" [" + Calendar.getInstance().get(Calendar.HOUR) + ":"
                                                + Calendar.getInstance().get(Calendar.MINUTE) + " "
                                                + Calendar.getInstance().get(Calendar.SECOND) + "]");
                                    return reObj;
                              }
                        });
      }
}
~~~
测试代理类方法
~~~java
public class TestForPoxy {
      public static void main(String[] args) {
            ServiceTest service = new ServiceTestImpl();
            System.out.println(service.getClass().getSimpleName());
            ServiceTest poxyService = (ServiceTest) JDKProxy.getPoxyObject(service);
            System.out.println(poxyService.getClass().getSuperclass());
            poxyService.saySomething("hello,My QQ code is 107966750.");
            poxyService.saySomething("what 's your name?");
            poxyService.saySomething("only for test,hehe.");
      }
}
~~~
- Proxy实现代理的目标类必须有实现接口

- 生成出来的代理类为接口实现类，和目标类不能进行转换，只能转为接口实现类进行调用

- 明显特点：通过此方法生成出来的类名叫做 $Proxy0

#####  2. 用CGLIB包实现

   - CGLIB实现方式是对代理的目标类进行继承

   - 生成出了的代理类可以没方法，生成出来的类可以直接转换成目标类或目标类实现接口的实现类


#####  3. Spring AOP 的代理类机制

Spring AOP中，当拦截对象实现了接口时，生成方式是用JDK的Proxy类。当没有实现任何接口时用的是GCLIB开源项目生成的拦截类的子类.

#### ！6、String s = new String("s")，创建了几个对象。


这个产生了2个对象，

一个是new关键字创建的new Sring（）；

另一个是"s"对象，abc在一个字符串池中s这个对象指向这个串池 

#### 参考资料


>Contributes: hueizhe
>
>Reviewers : Hollis, Kevin Lee
