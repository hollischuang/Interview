### MVC框架

---
```bash  
！必看 ？了解 %进阶
```  

#### ！1、介绍几个常用的MVC框架
##### <回答>  
**1.1) Struts MVC**  
- Controller：FilterDispatcher  
- View：JSP，volecity等  
- Model：Action  
```bash  
系统核心控制器FilterDispatcher根据请求自动调用Action，
在Action中调用相应的业务逻辑组件完成处理后通过FilterDispatcher返回视图
```  

**1.2) Spring MVC**  
- Controller：DispatcherServlet  
- View：JSP，volecity等  
- Model：Controller  
```bash  
Spring MVC中，controller接收参数request和response，返回ModelAndView
```  

<br/>  

##### <参考来源>：  
a) [《Java Web整合开发实战——基于Struts 2+Hibernate+Spring》（作者：贾蓓 镇明敏 杜磊 等）](http://www.tup.tsinghua.edu.cn/booksCenter/book_04983901.html)  

<br/>  


---  


#### ！2、什么是MVC
##### <回答>  
**2.1) MVC定义**  
```bash  
MVC模式（Model–view–controller）是软件工程中的一种软件架构模式，
把软件系统分为三个基本部分：模型（Model）、视图（View）和控制器（Controller），
MVC遵循职责分离原则，通常由Controller处理消息，Model掌管数据源，View负责数据显示
```  

**2.2) MVC一般模型**  
![MVC](http://www.oracle.com/ocom/groups/public/@otn/documents/digitalasset/145267.gif)  

* Model  
```bash  
封装应用状态，对状态查询作出响应，暴露应用功能，通知视图改变
```  
* View  
```bash  
渲染模型，向模型请求更新，发送用户动作至Controller，允许Controller选择视图
```  
* Controller  
```bash  
定义应用行为，将用户动作映射到模型，选择视图用于响应，与模型中的功能一一对应
```  

**2.3) MVC具体实践**  

* Model  
```bash  
Model负责数据访问，
较现代的 Framework 都会建议使用独立的数据对象 (DTO, POCO, POJO 等)来替代弱类型的集合对象。
数据访问的代码会使用 Data Access的代码或是 ORM-based Framework，
也可以进一步使用 Repository Pattern与 Unit of Works Pattern来切割数据源的相依性。
```  
* View  
```bash  
View负责显示数据，这个部分多为前端应用，
而Controller会有一个机制将处理的结果 (可能是 Model, 集合或是状态等)交给 View，
然后由 View 来决定怎么显示。
例如 Spring Framework使用JSP或相应技术，
ASP.NET MVC则使用Razor处理数据的显示。
```  
* Controller  
```bash  
Controller负责处理消息，较高阶的 Framework会有一个默认的实现来作为 Controller的基础，
例如Spring的 DispatcherServlet或是 ASP.NET MVC的Controller等，
在职责分离原则的基础上，每个 Controller负责的部分不同，
因此会将各个 Controller切割成不同的文件以利维护。
```  

<br/>  

##### <参考来源>：  
a) [Java BluePrints Model-View-Controller](http://www.oracle.com/technetwork/java/mvc-detailed-136062.html)  
b) [维基百科:MVC](https://zh.wikipedia.org/wiki/MVC)  

<br/>  


---  


#### ！3、Struts中请求的实现过程  
##### <回答> 
**3.1) 从Struts2框架的角度描述工作流程**  
* 1. 发出请求：
```bash  
用户发出一个HttpServletRequest请求
```  
* 2. 过滤请求：
经过一系列过滤器过滤

3. 调用FilterDispatcher：
   FilterDispatcher是控制器的核心，
   通过询问ActionMapper来确定请求是否需要调用某个Action，
   如果需要就将请求转交给ActionProxy

4. ActionProxy找到Action类：
   通过配置管理器Configuration Manager询问框架配置文件struts.xml，
   找到要调用的Action类

5. ActionInvocation加载拦截器：
   ActionProxy创建一个ActionInvocation实例，
   该实例使用命名模式来调用，
   在Action执行的前后，
   ActionInvocation实例根据配置文件加载与Action相关的所有拦截器Interceptor

6. 返回结果：
   一旦Action执行完毕，
   ActionInvocation实例根据strtus.xml文件中的配置找到相对应的返回结果，
   返回结果通常是一个JSP或FreeMarker模板

7. 返回响应：
   最后，HttpServletResponse通过web.xml中配置的过滤器返回
```  

* 3.2) MVC角度描述Struts2工作流程：  
```bash  
1. 浏览器发出请求  
2. 控制层中的核心控制器FilterDispatcher根据请求调用相应的Action  
3. Struts2的拦截器链自动对请求调用一些通用的控制逻辑，如数据校验，数据封装和文件上传等  
4. 回调Action中的excute()方法并在方法体内调用业务逻辑组件，即自定义的Javabean来处理请求，如数据查询等  
5. execute()方法返回后会产生一个输出  
6. 该输出经过浏览器拦截链自动处理，和开始的拦截器链处理顺序相反  
7. 控制层最后将数据返还并更新视图层    
```  

<br/>  

##### <参考来源>：  
a) [《Java Web整合开发实战——基于Struts 2+Hibernate+Spring》（作者：贾蓓 镇明敏 杜磊 等）](http://www.tup.tsinghua.edu.cn/booksCenter/book_04983901.html)  

<br/>  



#### %4、Spring mvc与Struts mvc的区别
| Name | Academy | score | 
| - | :-: | -: | 
| Harry Potter | Gryffindor| 90 | 
| Hermione Granger | Gryffindor | 100 | 
| Draco Malfoy | Slytherin | 90 |




#### ？5、Service嵌套事务处理，如何回滚


#### ！6、struts2 中拦截器与过滤器的区别及执行顺序


#### %7、struts2拦截器的实现原理


#### 参考资料


>Contributes: xxx
>
>Reviewers : Hollis, Kevin Lee
