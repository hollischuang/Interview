## Java Web

---

### Servlet
##### ！1、JSP和Servlet的区别，Servelt的概念。

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
d) [《JSP&Servlet学习笔记(第2版)》作者林信良](http://www.tup.tsinghua.edu.cn/booksCenter/book_04495001.html)  
e) [Servlet 简介(w3cschool)](https://www.w3cschool.cn/servlet/servlet-intro.html)  

<br/>  

##### ！2、Servlet的生命周期
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

##### ！3、Servlet中的session工作原理 ，以及设置过期时间的方式

##### ！4、Servlet中，filter的应用场景有哪些？

##### ？5、JSP的动态include和静态include

##### %6、web.xml中常用配置及作用

##### %7、Servlet的线程安全问题

##### 参考资料

---

### MVC框架
##### ！1、介绍几个常用的MVC框架

##### ！2、什么是MVC

##### ！3、Struts中请求的实现过程

##### %4、Spring mvc与Struts mvc的区别

##### ？5、Service嵌套事务处理，如何回滚

##### ！6、struts2 中拦截器与过滤器的区别及执行顺序

##### %7、struts2拦截器的实现原理

##### 参考资料

---

### http相关
##### ！1、session和cookie的区别

##### ！2、HTTP请求中session实现原理？

##### %3、如果客户端禁止Cookie能实现Session吗？

##### ！4、http中 get和post区别

##### ！5、redirect与forward的区别

##### ！6、常见的web请求返回的状态码。404、302、301、500分别代表什么

##### 参考资料

---

### SSM相关
##### ？1、Hibernate/Ibatis/MyBatis之间的区别

##### ？2、什么是OR Mapping

##### %3、MyBatis的缓存机制、一级和二级缓存

##### ！4、使用Spring的好处是什么，Spring的核心理念

##### ！5、什么是AOP和IOC，实现原理是什么

##### ！6、spring bean的初始化过程

##### ！7、Spring的事务管理 ，Spring bean注入的几种方式

##### %8、spring四种依赖注入方式

##### 参考资料

---

### 容器相关
##### ！1、什么是web服务器、什么是应用服务器

##### ！2、常用的web服务器有哪些？

##### ？3、Tomcat和weblogic的区别

##### 参考资料

---

### Web安全
##### ！1、什么是SQL注入 ，如何避免。

##### %2、什么是XSS攻击，如何避免

##### %3、什么是CSRF攻击，如何避免

##### 参考资料

---

### 动态代理
##### ！1、Java的动态代理的概念

##### %2、Java的动态代理的实现

##### 参考资料

---

### 编码问题
##### ！1、常用的字符编码

##### ！2、如何解决中文乱码问题

##### 参考资料

---

### 其它
##### %1、XML的解析方式，以及优缺点。

##### %2、什么是ajax，Ajax如何解决跨域问题

##### 参考资料

---

> Contributors : Hollis、Kevin Lee
>
> Reviewers : Hollis、Kevin Lee
