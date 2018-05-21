### Servlet

---

#### ！1、JSP和Servlet的区别，Servelt的概念。
##### 回答：
**1.1) JSP和Servlet的区别**  
- 二者联系： 
 
```bash
- JSP在本质上就是SERVLET。  
- JSP会被Web容器转译为Servlet的“.java”源文件，编译为“.class”文件，然后加载容器之中，所以最后提供服务的还是Servlet实例（Instance）。  
```  
  
- 主要区别：  

```bash
- Servlet在Java代码中通过HttpServletResponse对象动态输出HTML内容
- JSP在静态HTML内容中嵌入Java代码，Java代码被动态执行后生成HTML内容
```

- 各自的优缺点： 
 
```bash  
- Servlet能够很好地组织业务逻辑代码，但是在Java源文件中通过字符串拼接的方式生成动态HTML内容会导致代码维护困难、可读性差  
- JSP虽然规避了Servlet在生成HTML内容方面的劣势，但是在HTML中混入大量、复杂的业务逻辑同样也是不可取的
```  

- 解决方案：  

```bash
- 使用MVC模式规避JSP与Servlet各自的短板，提高代码的可读性和可维护性；  
- Servlet只负责业务逻辑而不会通过 out.append()动态生成HTML代码；  
- JSP中也不会充斥着大量的业务代码。  
```  

**1.2) Servlet的概念**  
- Servlet是什么？  

```bash  
Java Servlet 是运行在 Web 服务器或应用服务器上的程序。
```  

- Servlet在架构中的位置： 
 
```bash  
Web 浏览器或其他 HTTP 客户端的请求
中间层（servlet）  
HTTP 服务器上的数据库或应用程序
```   

![servlet架构图](https://img.w3cschool.cn/attachments/day_160820/201608201310026613.jpg)

- Servlet作用：

```bash  
- 读取客户端请求：
-- 显式数据：包括网页上的HTML 表单，applet 或自定义的HTTP 客户端程序的表单。
-- 隐式数据：包括 cookies、媒体类型和浏览器能理解的压缩格式等等。

- 处理数据并生成结果，这个过程可能需要：
-- 访问数据库
-- 执行 RMI 或 CORBA 调用
-- 调用 Web 服务
-- 直接计算

- 发送数据到客户端（浏览器）：
-- 显式文档：格式包括文本文件（HTML 或 XML）、二进制文件（GIF 图像）、Excel 等。
-- 隐式响应：包括告诉浏览器或其他客户端被返回的文档类型（例如 HTML），设置 cookies 和缓存参数，以及其他类似的任务。
```  


##### 参考来源：  
a) [Jsp和Servlet有什么区别？](https://www.zhihu.com/question/37962386/answer/87758781)  
b) [jsp与servlet有什么区别？](https://blog.csdn.net/yaohaibing576082210/article/details/5855444)  
c) [Servlet、Filter、Listener深入理解](https://blog.csdn.net/sunxianghuang/article/details/52107376)  
d) [Servlet 简介(w3cschool)](https://www.w3cschool.cn/servlet/servlet-intro.html)  
e) [《JSP&Servlet学习笔记(第2版)》作者林信良](http://www.tup.tsinghua.edu.cn/booksCenter/book_04495001.html)  

<br/>  


#### ！2、Servlet的生命周期
##### 回答：
**2.1) Servlet 生命周期描述**  

```bash  
Servlet 生命周期可被定义为从创建直到毁灭的整个过程  
- Servlet 通过调用 init () 方法进行初始化  
- Servlet 调用 service() 方法来处理客户端的请求  
- Servlet 通过调用 destroy() 方法终止（结束）  
- Servlet 最终由 JVM 的垃圾回收器进行垃圾回收  
```  

**2.2) 一个典型的 Servlet 生命周期方案**  

```bash  
- 第一个到达服务器的 HTTP 请求被委派到 Servlet 容器  
- Servlet 容器在调用 service() 方法之前加载 Servlet  
- 然后 Servlet 容器处理由多个线程产生的多个请求，每个线程执行一个单一的 Servlet 实例的 service() 方法  
```  
![架构图](https://img.w3cschool.cn/attachments/day_160820/201608201303222781.jpg)  

##### 参考来源：  
a) [Servlet 生命周期(w3cSchool)](https://www.w3cschool.cn/servlet/servlet-life-cycle.html)  

<br/>



#### ！3、Servlet中的session工作原理 ，以及设置过期时间的方式
##### 回答：
**3.1) Session工作原理**  
- 一句话概述

```bash  
每个HttpSession都有一个唯一的Session ID，
当浏览器访问应用程序时，会将Cookie中存放的Session ID一并发送给应用程序，
Web容器会根据Session ID找出对应的HttpSession对象，这样就能在不同请求中获取相同的数据。
```  

- 为什么使用Session   
 
```bash  
Web应用程序的请求与响应基于Http，为无状态的通信协议，每次请求对服务器来说都是新请求。
``` 

- HttpSession对象：

```bash  
当用户使用浏览器访问服务器的资源时，通过运行HttpServletRequest对象的getSession()方法，Web容器就会获取已经存在的HttpSession实例或创建一个新HttpSession实例。
由于Web容器本身是执行于JVM中的一个Java程序，HttpSession是Web容器中的一个Java对象。
```  

- Session ID：

```bash  
每个HttpSession对象都有一个特殊的Session ID作为标识，可以通过执行HttpSession的getId()方法取得这个Session ID。Session ID 默认使用Cookie存放在浏览器中。在Tomcat中，Cookie的名称是JSESSIONID（在PHP中为PHPSESSID）。
```  
<br/>

**3.2) Session设置超时的方式**  
- 方式一：

```bash  
在Servlet中执行HttpSession的setMaxInactiveInterval()方法，参数单位是“秒”；
```  

- 方式二：

```xml  
<!-- 在程序中的web.xml里设置HttpSession默认失效时间，单位是“分钟” -->
<session-config>
  <session-timeout>30</session-timeout>
</session-config>
```  

- 方式三：

```xml  
<!-- 在Tomcat的/conf/web.xml中session-config，单位也是分钟 -->
<session-config>
     <session-timeout>30</session-timeout>
</session-config>
```  

- 注意：
  
```bash  
默认关闭浏览器马上失效的是浏览器上的Cookie，不是HttpSession。
存在Cookie中的Session ID随着Cookie失效而丢失，
所以再次打开浏览器向服务器发送请求时，HttpSession尝试getSession()，Web容器会产生新的HttpSession实例。
要让HttpSession立即失效必须调用invalidate()方法，
否则HttpSession实例要等到失效时间过后才会被容器销毁回收。
```  


##### 参考来源：  
a) [Servlet中不可不知的Session技术](https://blog.csdn.net/qq_15096707/article/details/71081313#session%E7%9A%84%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)  
b) [Java Web开发Session超时设置](http://www.cnblogs.com/sharpest/p/6185234.html)  
c) [《JSP&Servlet学习笔记(第2版)》作者林信良](http://www.tup.tsinghua.edu.cn/booksCenter/book_04495001.html)  
<br/>

#### ！4、Servlet中，filter的应用场景有哪些？
##### 回答：  
**4.1) Filter应用场景**  
- 统一请求或响应的字符集   

```bash  
如将编码统一为UTF-8
```  
- 替换特殊字符  

```bash  
如将请求参数中的一些HTML标签去掉
```  

- 控制浏览器缓存页面中的静态资源

```bash  
有些动态页面中引用了一些图片或css文件以修饰页面效果，
这些图片和css文件经常是不变化的，
所以为减轻服务器的压力，
可以使用filter控制浏览器缓存这些文件，
以提升服务器的性能。
```  

- 使用Filter实现URL级别的权限认证

```bash  
在实际开发中我们经常把一些执行敏感操作的servlet映射到一些特殊目录中，
并用filter把这些特殊目录保护起来，
限制只能拥有相应访问权限的用户才能访问这些目录下的资源。
从而在我们系统中实现一种URL级别的权限功能。
```  

- 实现用户自动登陆

```bash  
首先，在用户登陆成功后，
发送一个名称为user的cookie给客户端，
cookie的值为用户名和md5加密后的密码。
编写一个AutoLoginFilter，
这个filter检查用户是否带有名称为user的cookie，
如果有，则调用dao查询cookie的用户名和密码是否和数据库匹配，
匹配则向session中存入user对象（即用户登陆标记），
以实现程序完成自动登陆。
```  

<br/>
**4.2) 更多细分过滤器**  

```bash  
- 身份验证过滤器（Authentication Filters）。
- 数据压缩过滤器（Data compression Filters）。
- 加密过滤器（Encryption Filters）。
- 触发资源访问事件过滤器。
- 图像转换过滤器（Image Conversion Filters）。
- 日志记录和审核过滤器（Logging and Auditing Filters）。
- MIME-TYPE 链过滤器（MIME-TYPE Chain Filters）。
- 标记化过滤器（Tokenizing Filters）。
- XSL/T 过滤器（XSL/T Filters），转换 XML 内容。
```  

<br/>
**4.3) 过滤器的使用**  

- 为什么使用过滤器：  

```bash  
类似性能评测、用户验证、字符替换、编码设置这类需求，
与应用程序的业务需求没有直接关系，
应该设计为独立的元件，可以随意插拔，
或者随意修改设置而无需修改业务代码。
```  

- 过滤器概念： 
 
```bash  
过滤器是一个实现了 javax.servlet.Filter接口的Java类，
作用于浏览器和Servlet中间，过滤请求与响应。
```  

- 过滤器的定义：  

```bash
- Servlet/JSP要实现过滤器，必须实现Filter接口，并在web.xml中定义过滤器，让过滤器知道加载哪个过滤器类。
- Filter接口有三个要实现的方法，init()、doFilter()与destroy()。
```
- 过滤器的执行过程：

```bash  
- 当过滤器类被载入容器并实例化后，容器会运行init()方法并传入FilterConfig对象作为参数。
- 当请求来到过滤器时，会调用doFilter()方法，doFilter()上除了ServletRequest和ServletResponse之外，还有一个FilterChain参数；
- 如果调用了FilterChain的doFilter()方法，就会运行下一个过滤器，如果没有下一个过滤器，就调用请求目标Servlet的service()方法；
- 在到达service()方法之后，流程会以堆栈顺序返回，所以在FilterChain的doFilter()运行完毕后，就可以针对service()方法做后续处理；
- 如果因为某个条件（如用户没有通过验证）而不调用FilterChain的doFilter()，则请求就不会继续至目标Servlet，此时过滤器就起到了拦截请求的作用；
```  


- Filter接口：

```java  
public interface Filter {  
    //用于完成Filter的初始化  
    //该方法由 Web 容器调用，指示一个过滤器被放入服务  
    public void init(FilterConfig filterConfig) throws ServletException;  

    //实现过滤功能  
    //该方法在每次一个请求/响应对因客户端在链的末端请求资源而通过链传递时由容器调用  
    public void doFilter ( ServletRequest request, ServletResponse response,   
			FilterChain chain ) throws IOException, ServletException;  

    //用于销毁Filter前，完成某些资源的回收  
    //该方法由 Web 容器调用，指示一个过滤器被取出服务。
    public void destroy();  
}  
```

<br/>
** 4.4) 过滤器的生命周期**  

```bash  
web.xml 中声明的每个 filter 在每个虚拟机中仅有一个实例。
```  
- (1) 加载和实例化 
 
```bash  
Web容器启动时，即会根据 web.xml中声明的 filter顺序依次实例化这些 filter。
```  
- (2) 初始化  

```bash  
Web容器调用init(FilterConfig)来初始化过滤器。
容器在调用该方法时，向过滤器传递 FilterConfig对象，FilterConfig的用法和 ServletConfig类似。
利用 FilterConfig对象可以得到 ServletContext对象，以及在 web.xml 中配置的过滤器的初始化参数。
在这个方法中，可以抛出 ServletException 异常，通知容器该过滤器不能正常工作。
此时的 Web 容器启动失败，整个应用程序不能够被访问。
实例化和初始化的操作只会在容器启动时执行，而且只会执行一次。
```  
-  (3) doFilter

```bash  
doFilter方法类似于Servlet接口的service()方法。
当客户端请求目标资源的时候，容器会筛选出符合 filter-mapping中url-pattern的filter，
并按照声明filter-mapping的顺序依次调用这些 filter的doFilter方法。
在这个链式调用过程中，
可以调用 chain.doFilter(ServletRequest, ServletResponse)将请求传给下一个过滤器(或目标资源)，
也可以直接向客户端返回响应信息，
或者利用 RequestDispatcher的forward和include方法，
以及 HttpServletResponse的sendRedirect方法将请求转向到其它资源。
需要注意的是，这个方法的请求和响应参数的类型是 ServletRequest和ServletResponse，
也就是说，过滤器的使用并不依赖于具体的协议。
```  
- (4)销毁  

```bash  
Web容器调用destroy()方法指示过滤器的生命周期结束。
在这个方法中，可以释放过滤器使用的资源。
```  

<br/>
**4.5) 过滤器的运行原理**

![原理](https://img-blog.csdn.net/20160808181905251?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
![流程](https://img-blog.csdn.net/20160808182011030?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

<br/>
##### 参考来源：
  
a) [Servlet、Filter、Listener深入理解](https://blog.csdn.net/sunxianghuang/article/details/52107376)  
b) [Servlet 编写过滤器](https://www.w3cschool.cn/servlet/servlet-writing-filters.html)  
c) [《JSP&Servlet学习笔记(第2版)》作者林信良](http://www.tup.tsinghua.edu.cn/booksCenter/book_04495001.html)  
<br/>


#### ？5、JSP的动态include和静态include


#### %6、web.xml中常用配置及作用


#### %7、Servlet的线程安全问题


#### 参考资料


>Contributes: xxx
>
>Reviewers : Hollis, Kevin Lee
