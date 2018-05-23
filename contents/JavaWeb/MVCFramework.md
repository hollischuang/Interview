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


#### %4、Spring mvc与Struts mvc的区别


#### ？5、Service嵌套事务处理，如何回滚


#### ！6、struts2 中拦截器与过滤器的区别及执行顺序


#### %7、struts2拦截器的实现原理


#### 参考资料


>Contributes: xxx
>
>Reviewers : Hollis, Kevin Lee
