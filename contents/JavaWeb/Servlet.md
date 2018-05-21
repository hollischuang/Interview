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
- Servlet 通过调用 init() 方法进行初始化  
- Servlet 调用 service() 方法来处理客户端的请求  
- Servlet 通过调用 destroy() 方法终止（结束）  
- Servlet 最终由 JVM 的垃圾回收器进行垃圾回收  
```  

**2.2) 一个典型的 Servlet 生命周期方案**  

```bash  
- 第一个到达服务器的 HTTP 请求被委派到 Servlet 容器  
- Servlet 容器在调用 service() 方法之前加载 Servlet  
- 然后 Servlet 容器处理由多个线程产生的多个请求，每个线程执行一个单一的Servlet实例的 service() 方法  
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
当用户使用浏览器访问服务器的资源时，通过运行HttpServletRequest对象的 getSession()方法，Web容器就会获取已经存在的HttpSession实例或创建一个新HttpSession实例。
由于Web容器本身是执行于JVM中的一个Java程序，HttpSession是Web容器中的一个Java对象。
```  

- Session ID：

```bash  
每个HttpSession对象都有一个特殊的Session ID作为标识，可以通过执行HttpSession的 getId()方法取得这个Session ID。Session ID 默认使用Cookie存放在浏览器中。在Tomcat中，Cookie的名称是JSESSIONID（在PHP中为PHPSESSID）。
```  
<br/>

**3.2) Session设置超时的方式**  
- 方式一：

```bash  
在Servlet中执行HttpSession的 setMaxInactiveInterval()方法，参数单位是“秒”；
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
所以再次打开浏览器向服务器发送请求时，HttpSession尝试 getSession()，Web容器会产生新的HttpSession实例。
要让HttpSession立即失效必须调用 invalidate()方法，
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
- Filter接口有三个要实现的方法， init()、 doFilter() 与 destroy()。
```
- 过滤器的执行过程：

```bash  
- 当过滤器类被载入容器并实例化后，容器会运行 init()方法并传入FilterConfig对象作为参数。
- 当请求来到过滤器时，会调用 doFilter()方法， doFilter()上除了ServletRequest和ServletResponse之外，还有一个FilterChain参数；
- 如果调用了FilterChain的 doFilter()方法，就会运行下一个过滤器，如果没有下一个过滤器，就调用请求目标Servlet的 service()方法；
- 在到达 service()方法之后，流程会以堆栈顺序返回，所以在FilterChain的 doFilter()运行完毕后，就可以针对 service()方法做后续处理；
- 如果因为某个条件（如用户没有通过验证）而不调用FilterChain的 doFilter()，则请求就不会继续至目标Servlet，此时过滤器就起到了拦截请求的作用；
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
doFilter方法类似于Servlet接口的 service()方法。
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
Web容器调用 destroy()方法指示过滤器的生命周期结束。
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
##### 回答：  
**5.1) 概念：**   
* 5.1.1) 静态include：  
```bash  
- <%@include file=”xxx.jsp”%> 是JSP指示（或者指令）元素的一种，用于告知容器将其他JSP页面包括进来进行转译。
- 使用include来包括其他页面内容时，会在转译时就决定转译后的Servlet内容，所以说是一种静态的包括方式。
```  

* 5.1.2) 动态include：    
```bash  
- <jsp:include page="xxx.jsp" /> 是JSP行为（或者动作）元素的一种，使用动态include将页面包含进来，当前页面会生成一个Servlet类，被include的页面也会独立生成一个Servlet类。
- 当前页面转译而成的Servlet中，会取得RequestDispatcher对象，并执行include()方法，也就是请求时将动态include的页面转交给另一个Servlet，而后再回到自己的Servlet。
```  

**5.2) 区别**  
* 5.2.1) 执行时间不同:    
```bash  
- 静态include是在转译阶段执行  
- 动态include在请求处理阶段执行.  
```  
* 5.2.2) 引入内容不同:  
```bash  
- 静态include引入静态文本(html,jsp)，在JSP页面被转化成servlet之前和它融和到一起  
- 动态include引入执行页面或servlet所生成的应答文本.  
```  

* 5.2.3) 属性区别：    
```bash  
- 静态include通过file属性指定被包含的文件，并且file属性不支持任何表达式；  
- 动态include通过page属性指定被包含的文件，且page属性支持JSP表达式；  
```  
* 5.2.4) 编译区别：  
```bash  
- 静态include时，被包含的文件内容会原封不动的插入到包含页中，然后JSP编译器再将合成后的文件最终编译成一个Java文件，因此被导入页面甚至不需要是一个完整的页面；  
- 动态inlcude时，当该标识被执行时，程序会将请求转发（不是请求重定向）到被包含的页面，并将执行结果输出到浏览器中，然后返回包含页继续执行后面的代码。因为服务器执行的是多个文件，所以JSP编译器会分别对这些文件进行编译；  
```  
* 5.2.5) 编译指令区别：  
```bash  
- 静态inlcude时被导入页面的编译指令会起作用；  
- 而动态include时被导入页面的编译指令则失去作用，只是插入被导入页面的body内容。  
```  
* 5.2.6) 变量作用域区别：  
```bash  
- 静态include时，由于被包含的文件最终会生成一个文件，所以在被包含、包含文件中不能有重名的变量或方法；  
- 动态include时，由于每个文件是单独编译的，所以在被包含文件和包含文件中重名的变量和方法是不相冲突的。  
```  
* 5.2.7) 传参区别：  
```html  
- 静态include会导致两个jsp合并成为同一个java文件，所以不存在传参问题  
- 动态include对被包含的jsp文件进行了一次独立的访问，通过在<jsp:include>标签中嵌入子标签<jsp:param>的形式传递参数  
<jsp:include page="xxx.jsp">  
	<jsp:param name="参数名" value="参数值" />  
</jsp:include>  
在被包含的JSP页面中通request.getParameter("参数名")获取参数值  
<p>获取的参数值在此显示：<%=request.getParameter("参数名")%></p>  
```  

<br/>  

##### 参考来源：  
a) [JSP include](http://how2j.cn/k/jsp/jsp-include/576.html#step1660)  
b) [JSP之静态include指令、动态Include指令](https://www.cnblogs.com/zhouhb/archive/2015/09/18/4818534.html)  
c) [JSP中动态INCLUDE与静态INCLUDE的区别](http://meiowei.iteye.com/blog/413976)  
d) [《JSP&Servlet学习笔记(第2版)》作者林信良](http://www.tup.tsinghua.edu.cn/booksCenter/book_04495001.html)  
<br/>

#### %6、web.xml中常用配置及作用
##### 回答  
**6.1) 常用web.xml配置场景**  
* 6.1.1) 指定欢迎页面：  
```xml  
<welcome-file-list>
  <welcome-file>index.jsp</welcome-file>
  <welcome-file>index1.jsp</welcome-file>
</welcome-file-list>

上面的例子指定了2个欢迎页面，显示时按顺序从第一个找起，
如果第一个存在，就显示第一个，后面的不起作用。
如果第一个不存在，就找第二个，以此类推。
```  

- 6.1.2) 命名与定制URL  
```xml  
我们可以为Servlet和JSP文件命名并定制URL,其中定制URL是依赖一命名的，命名必须在定制URL前  
(1) 为Servlet命名：  
<servlet>  
  <servlet-name>servlet1</servlet-name>  
  <servlet-class>net.test.TestServlet</servlet-class>  
</servlet>  

(2) 为Servlet定制URL
<servlet-mapping>  
  <servlet-name>servlet1</servlet-name>  
  <url-pattern>*.do</url-pattern>  
</servlet-mapping>  
```  

- 6.1.3) 定制初始化参数：  
```xml  
可以定制servlet、JSP、Context的初始化参数，然后可以再servlet、JSP、Context中获取这些参数值。
<servlet>
  <servlet-name>servlet1</servlet-name>
  <servlet-class>net.test.TestServlet</servlet-class>
  <init-param>
    <param-name>userName</param-name>
    <param-value>Tommy</param-value>
  </init-param>
  <init-param>
    <param-name>E-mail</param-name>
    <param-value>Tommy@163.com</param-value>
  </init-param>
</servlet>
经过上面的配置，
在servlet中能够调用getServletConfig().getInitParameter("param1")获得参数名对应的值。
```  

- 6.1.4) 指定错误处理页面  
```xml  
可以通过“异常类型”或“错误码”来指定错误处理页面。
<error-page>
  <error-code>404</error-code>
  <location>/error404.jsp</location>
</error-page>

<error-page>
  <exception-type>java.lang.Exception<exception-type>
  <location>/exception.jsp<location>
</error-page>
```  

- 6.1.5) 设置过滤器：  
```xml  
比如设置一个编码过滤器，过滤所有资源
<filter>  
  <filter-name>XXXCharaSetFilter</filter-name>
  <filter-class>net.test.CharSetFilter</filter-class>
</filter>

<filter-mapping>
  <filter-name>XXXCharaSetFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```  

- 6.1.6) 设置监听器：  
```xml  
<listener>
  <listener-class>net.test.XXXLisenet</listener-class>
</listener>
```  

- 6.1.7) 设置会话(Session)过期时间  
```xml  
时间以分钟为单位，假如设置60分钟超时：
<session-config>  
  <session-timeout>60</session-timeout>  
</session-config>  
```  

<br />  
**6.2) web.xml标签及说明**  
```xml  
<web-app>

	<!--定义了WEB应用的名字 -->
	<display-name></display-name>

	<!--声明WEB应用的描述信息 -->
	<description></description>

	<!--context-param元素声明应用范围内的初始化参数 -->
	<context-param></context-param>

	<!--过滤器元素将一个名字与一个实现javax.servlet.Filter接口的类相关联 -->
	<filter></filter>

	<!--一旦命名了一个过滤器，就要利用filter-mapping元素把它与一个或多个servlet或JSP页面相关联 -->
	<filter-mapping></filter-mapping>

	<!--servlet API的版本2.3增加了对事件监听程序的支持，事件监听程序在建立、修改和删除会话或servlet环境时得到通知。 Listener元素指出事件监听程序类 -->
	<listener></listener>

	<!--在向servlet或JSP页面制定初始化参数或定制URL时，必须首先命名servlet或JSP页面。 Servlet元素就是用来完成此项任务的 -->
	<servlet></servlet>

	<!--服务器一般为servlet提供一个缺省的URL：http://host/webAppPrefix/servlet/ServletName。 
		但是，常常会更改这个URL，以便servlet可以访问初始化参数或更容易地处理相对URL。 在更改缺省URL时，使用servlet-mapping元素 -->
	<servlet-mapping></servlet-mapping>

	<!--如果某个会话在一定时间内未被访问，服务器可以抛弃它以节省内存。可通过使用HttpSession的 setMaxInactiveInterval方法明确设置单个会话对象的超时值，或者可利用session-config元素制定缺省超时值 -->
	<session-config></session-config>

	<!--如果Web应用具有想到特殊的文件，希望能保证给他们分配特定的MIME类型，则mime-mapping元素提供这种保证 -->
	<mime-mapping></mime-mapping>

	<!--指示服务器在收到引用一个目录名而不是文件名的URL时，使用哪个文件 -->
	<welcome-file-list></welcome-file-list>

	<!--在返回特定HTTP状态代码时，或者特定类型的异常被抛出时，能够制定将要显示的页面 -->
	<error-page></error-page>

	<!--对标记库描述符文件（Tag Libraryu Descriptor file）指定别名。此功能使你能够更改TLD文件的位置， 而不用编辑使用这些文件的JSP页面 -->
	<taglib></taglib>

	<!--声明与资源相关的一个管理对象 -->
	<resource-env-ref></resource-env-ref>

	<!--声明一个资源工厂使用的外部资源 -->
	<resource-ref></resource-ref>

	<!--制定应该保护的URL。它与login-config元素联合使用 -->
	<security-constraint></security-constraint>

	<!--指定服务器应该怎样给试图访问受保护页面的用户授权。它与sercurity-constraint元素联合使用 -->
	<login-config></login-config>

	<!--给出安全角色的一个列表，这些角色将出现在servlet元素内的security-role-ref元素的role-name子元素中。 分别地声明角色可使高级IDE处理安全信息更为容易 -->
	<security-role></security-role>

	<!--声明Web应用的环境项 -->
	<env-entry></env-entry>

	<!--声明一个EJB的主目录的引用 -->
	<ejb-ref></ejb-ref>

	<!--声明一个EJB的本地主目录的应用 -->
	<ejb-local-ref></ejb-local-ref>

</web-app>
```  

<br />  
**6.3) 更多web.xml配置**  
- 6.3.1) Web应用图标：  
```xml  
<!-- 指出IDE和GUI工具用来表示Web应用的大图标和小图标 -->  
<icon> 
  <small-icon>/images/app_small.gif</small-icon>  
  <large-icon>/images/app_large.gif</large-icon>   
</icon> 
```  

- 6.3.2) Web 应用名称：  
```xml  
<!-- 提供GUI工具可能会用来标记这个特定Web应用的一个名称   -->
<display-name>Tomcat Example</display-name> 
```  
- 6.3.3) Web 应用描述：  
```xml  
<!--给出与此相关的说明性文本 -->
<disciption>Tomcat Example servlets and JSP pages.</disciption> 
```  
- 6.3.4) 上下文参数：  
```xml  
<!--声明应用范围内的初始化参数 -->
<context-param> 
  <param-name>ContextParameter</para-name> 
  <param-value>test</param-value> 
  <description>It is a test parameter.</description> 
</context-param> 

<!-- 在servlet里面可以通过getServletContext().getInitParameter("context/param")得到 -->
```  

- 6.3.5) 过滤器配置：  
```xml  
<!--将一个名字与一个实现javaxs.servlet.Filter接口的类相关联 -->
<filter> 
  <filter-name>setCharacterEncoding</filter-name> 
  <filter-class>com.myTest.setCharacterEncodingFilter</filter-class> 
  <init-param> 
  <param-name>encoding</param-name> 
  <param-value>GB2312</param-value> 
  </init-param> 
</filter> 

<filter-mapping> 
  <filter-name>setCharacterEncoding</filter-name> 
  <url-pattern>/*</url-pattern> 
</filter-mapping> 
```  

- 6.3.6) 监听器配置    
```xml  
<listener> 
  <listerner-class>listener.SessionListener</listener-class> 
</listener> 
```  
- 6.3.7) Servlet配置  
```xml  
<!--基本配置 -->
<servlet>  
  <servlet-name>snoop</servlet-name> 
  <servlet-class>SnoopServlet</servlet-class> 
</servlet> 

<servlet-mapping>  
  <servlet-name>snoop</servlet-name>  
  <url-pattern>/snoop</url-pattern>   
</servlet-mapping>  

<!-- 高级配置-->
<servlet> 
  <servlet-name>snoop</servlet-name>  
  <servlet-class>SnoopServlet</servlet-class>   
  <init-param>  
  <param-name>foo</param-name> 
  <param-value>bar</param-value> 
  </init-param> 
  <run-as> 
  <description>Security role for anonymous access</description> 
  <role-name>tomcat</role-name> 
  </run-as> 
</servlet> 

<servlet-mapping> 
  <servlet-name>snoop</servlet-name> 
  <url-pattern>/snoop</url-pattern> 
</servlet-mapping> 

<!--元素说明 -->  
<servlet></servlet> <!--用来声明一个servlet的数据，主要有以下子元素-->   
<servlet-name></servlet-name> <!--指定servlet的名称 --> 
<servlet-class></servlet-class> <!--指定servlet的类名称 --> 
<jsp-file></jsp-file> <!--指定web站台中的某个JSP网页的完整路径-->  
<init-param></init-param> <!--用来定义参数，可有多个init-param。在servlet类中通过getInitParamenter(String name)方法访问初始化参数 -->  
<load-on-startup></load-on-startup> <!--指定当Web应用启动时，装载Servlet的次序。 当值为正数或零时：Servlet容器先加载数值小的servlet，再依次加载其他数值大的servlet. 当值为负或未定义：Servlet容器将在Web客户首次访问这个servlet时加载它 -->  
<servlet-mapping></servlet-mapping> <!--用来定义servlet所对应的URL，包含两个子元素 --> 
<servlet-name></servlet-name> <!--指定servlet的名称 --> 
<url-pattern></url-pattern> <!--指定servlet所对应的URL --> 
```  

- 6.3.8) 会话超时配置（单位为分钟）   
```xml  
<session-config>
  <session-timeout>120</session-timeout> 
</session-config> 
```   
- 6.3.9) MIME类型配置  
```xml  
<mime-mapping> 
  <extension>htm</extension> 
  <mime-type>text/html</mime-type> 
</mime-mapping> 
```  

- 6.3.10) 指定欢迎文件页配置  
```xml  
<welcome-file-list> 
  <welcome-file>index.jsp</welcome-file> 
  <welcome-file>index.html</welcome-file> 
  <welcome-file>index.htm</welcome-file> 
</welcome-file-list> 
```  
- 6.3.11) 配置错误页面  
```xml  
<!-- 1. 通过错误码来配置error-page -->
<error-page> 
  <error-code>404</error-code> 
  <location>/NotFound.jsp</location> 
</error-page> 

<!-- 上面配置了当系统发生404错误时，跳转到错误处理页面NotFound.jsp -->

<!-- 2. 通过异常的类型配置error-page -->
<error-page> 
  <exception-type>java.lang.NullException</exception-type> 
  <location>/error.jsp</location> 
</error-page> 

<!--上面配置了当系统发生java.lang.NullException（即空指针异常）时，跳转到错误处理页面error.jsp -->
```  

- 6.3.12) TLD配置  
```xml  
<taglib> 
  <taglib-uri>http://jakarta.apache.org/tomcat/debug-taglib</taglib-uri> 
  <taglib-location>/WEB-INF/jsp/debug-taglib.tld</taglib-location> 
</taglib> 

<!-- 如果IDE一直在报错,应该把<taglib> 放到 <jsp-config>中-->
<jsp-config> 
  <taglib> 
    <taglib-uri>http://jakarta.apache.org/tomcat/debug-taglib</taglib-uri> 
    <taglib-location>/WEB-INF/pager-taglib.tld</taglib-location> 
  </taglib> 
</jsp-config> 
```  

- 6.3.13) 资源管理对象配置  
```xml  
<resource-env-ref> 
  <resource-env-ref-name>jms/StockQueue</resource-env-ref-name> 
</resource-env-ref>
```  
- 6.3.14) 资源工厂配置  
```xml  
<resource-ref> 
  <res-ref-name>mail/Session</res-ref-name> 
  <res-type>javax.mail.Session</res-type> 
  <res-auth>Container</res-auth> 
</resource-ref> 

<!--配置数据库连接池就可在此配置 -->
<resource-ref> 
  <description>JNDI JDBC DataSource of shop</description> 
  <res-ref-name>jdbc/sample_db</res-ref-name> 
  <res-type>javax.sql.DataSource</res-type> 
  <res-auth>Container</res-auth> 
</resource-ref> 
```  

- 6.3.15) 安全限制配置  
```xml  
<security-constraint> 
  <display-name>Example Security Constraint</display-name> 
  <web-resource-collection> 
    <web-resource-name>Protected Area</web-resource-name>  
    <url-pattern>/jsp/security/protected/*</url-pattern> 
    <http-method>DELETE</http-method> 
    <http-method>GET</http-method> 
    <http-method>POST</http-method> 
    <http-method>PUT</http-method> 
  </web-resource-collection> 
  <auth-constraint> 
    <role-name>tomcat</role-name> 
    <role-name>role1</role-name> 
  </auth-constraint> 
</security-constraint> 
```  

- 6.3.16) 登陆验证配置  
```xml  
<login-config> 
  <auth-method>FORM</auth-method> 
  <realm-name>Example-Based Authentiation Area</realm-name> 
  <form-login-config> 
    <form-login-page>/jsp/security/protected/login.jsp</form-login-page>
    <form-error-page>/jsp/security/protected/error.jsp</form-error-page>
  </form-login-config> 
</login-config> 
```  

- 6.3.17)  安全角色  
```xml  
<!-- security-role元素给出安全角色的一个列表 -->  
<!-- 这些角色将出现在servlet元素内security-role-ref元素的role-name子元素中-->  
<!-- 分别地声明角色可使高级IDE处理安全信息更为容易 -->  
<security-role>
  <role-name>tomcat</role-name> 
</security-role> 
```  

- 6.3.18) Web环境参数  
```xml  
<!-- env-entry元素声明Web应用的环境项 -->  
<env-entry> 
  <env-entry-name>minExemptions</env-entry-name> 
  <env-entry-value>1</env-entry-value> 
  <env-entry-type>java.lang.Integer</env-entry-type> 
</env-entry> 
```  

- 6.3.19) EJB 声明  
```xml  
<ejb-ref> 
  <description>Example EJB reference</decription> 
  <ejb-ref-name>ejb/Account</ejb-ref-name> 
  <ejb-ref-type>Entity</ejb-ref-type> 
  <home>com.mycompany.mypackage.AccountHome</home> 
  <remote>com.mycompany.mypackage.Account</remote> 
</ejb-ref> 
```  
- 6.3.20) 本地EJB声明  
```xml  
<ejb-local-ref> 
  <description>Example Loacal EJB reference</decription> 
  <ejb-ref-name>ejb/ProcessOrder</ejb-ref-name> 
  <ejb-ref-type>Session</ejb-ref-type> 
  <local-home>com.mycompany.mypackage.ProcessOrderHome</local-home> 
  <local>com.mycompany.mypackage.ProcessOrder</local> 
</ejb-local-ref> 
```  
- 6.3.21) 配置DWR  
```xml  
<servlet> 
  <servlet-name>dwr-invoker</servlet-name> 
  <servlet-class>uk.ltd.getahead.dwr.DWRServlet</servlet-class> 
</servlet> 

<servlet-mapping> 
  <servlet-name>dwr-invoker</servlet-name> 
  <url-pattern>/dwr/*</url-pattern> 
</servlet-mapping> 
```  
- 6.3.22) 配置Struts  
```xml  
<display-name>Struts Blank Application</display-name> 
<servlet> 
  <servlet-name>action</servlet-name> 
  <servlet-class> 
    org.apache.struts.action.ActionServlet 
  </servlet-class> 
  <init-param> 
    <param-name>detail</param-name> 
    <param-value>2</param-value>
  </init-param> 
  <init-param> 
    <param-name>debug</param-name> 
    <param-value>2</param-value> 
  </init-param> 
  <init-param> 
    <param-name>config</param-name> 
    <param-value>/WEB-INF/struts-config.xml</param-value> 
  </init-param> 
  <init-param> 
    <param-name>application</param-name> 
    <param-value>ApplicationResources</param-value> 
  </init-param> 
  <load-on-startup>2</load-on-startup> 
</servlet> 

<servlet-mapping> 
  <servlet-name>action</servlet-name> 
  <url-pattern>*.do</url-pattern> 
</servlet-mapping> 

<welcome-file-list> 
  <welcome-file>index.jsp</welcome-file> 
</welcome-file-list> 

<!-- Struts Tag Library Descriptors --> 
<taglib> 
  <taglib-uri>struts-bean</taglib-uri> 
  <taglib-location>/WEB-INF/tld/struts-bean.tld</taglib-location> 
</taglib> 

<taglib> 
  <taglib-uri>struts-html</taglib-uri> 
  <taglib-location>/WEB-INF/tld/struts-html.tld</taglib-location> 
</taglib> 

<taglib> 
  <taglib-uri>struts-nested</taglib-uri> 
  <taglib-location>/WEB-INF/tld/struts-nested.tld</taglib-location> 
</taglib> 

<taglib> 
  <taglib-uri>struts-logic</taglib-uri> 
  <taglib-location>/WEB-INF/tld/struts-logic.tld</taglib-location> 
</taglib> 

<taglib> 
  <taglib-uri>struts-tiles</taglib-uri> 
  <taglib-location>/WEB-INF/tld/struts-tiles.tld</taglib-location> 
</taglib>
```  
- 6.3.23) 配置spring  
```xml  
<!-- 指定spring配置文件位置 --> 
<context-param> 
  <param-name>contextConfigLocation</param-name> 
  <!--加载多个spring配置文件 --> 
  <param-value> 
    /WEB-INF/applicationContext.xml, /WEB-INF/action-servlet.xml 
  </param-value> 
</context-param> 

<!-- 定义SPRING监听器，加载spring --> 
<listener> 
  <listener-class>org.springframework.web.context.ContextLoaderListener
  </listener-class> 
</listener> 
<listener> 
  <listener-class> 
    org.springframework.web.context.request.RequestContextListener 
  </listener-class> 
</listener>
```  

<br/>  
##### 参考来源：  
a) [Java Web的web.xml文件作用及基本配置](https://www.cnblogs.com/EasonJim/p/6221952.html)  
b) [史上最全web.xml配置文件元素详解](https://www.cnblogs.com/hafiz/p/5715523.html)  
<br/>  

#### %7、Servlet的线程安全问题  
##### 回答:  

**7.1) Servlet多线程** 
```bash  
Servlet规范中定义，默认情况下（Servlet不是在分布式的环境中部署），
Servlet容器对声明的每一个Servlet，只创建一个实例。
如果有多个客户端请求同时访问这个Servlet，Servlet容器如何处理多个请求呢？
答案是采用多线程，Servlet容器维护一个线程池来服务请求。
当容器接收到一个访问Servlet的请求，调度者线程从线程池中选取一个工作线程，
将请求传递给该线程，然后由这个线程执行Servlet的service()方法。
```  

**7.2) 线程安全的Servlet** 
* 7.2.1) 变量的线程安全  
```bash  
Servlet是单实例多线程模型，多个线程共享一个Servlet实例，因此对于实例变量的访问是非线程安全的。
建议：在Servlet中尽可能的使用局部变量，应该只使用只读的实例变量和静态变量。
如果非得使用共享的实例变量或静态变量，在修改共享变量时应该注意线程同步。
```  

* 7.2.2) 属性的线程安全  
```bash  
在Servlet中，可以访问保存在ServletContext、HttpSession和ServletRequest对象中的属性。
那么这三种不同范围的对象，属性访问是否是线程安全的呢？
- ServletContext：
  该对象被Web应用程序的所有Servlet共享，多线程环境下肯定是非线程安全的。

- HttpSession：
  HttpSession对象只能在同属于一个Session的请求线程中共享。
  对于同一个Session，我们可能会认为在同一时刻只有一个用户请求。
  因此，Session对象的属性访问是线程安全的。
  但是，如果用户打开多个同属于一个进程的浏览器窗口，
  在这些窗口中的访问请求同属于一个Session，对于多个线程的并发修改显然不是线程安全的。

- ServletRequest：
  因为Servlet容器对它所接收到的每一个请求，
  都创建一个新的ServletRequest对象，
  所以ServletRequest对象只在一个线程中被访问，
  因此对ServletRequest的属性访问是线程安全的。
  但是，如果在Servlet中创建了自己的线程，
  那么对ServletRequest的属性访问的线程安全性就得自己去保证。
  此外，如果将当前请求的Servlet通过HttpSession或者ServletContext共享，
  当然也是非线程安全的。
```  

<br/>  
##### 参考来源：  
a) [Servlet、Filter、Listener深入理解](https://blog.csdn.net/sunxianghuang/article/details/52107376)  
<br/>  

---

>Contributes: bigablecat
>
>Reviewers : Hollis, Kevin Lee
