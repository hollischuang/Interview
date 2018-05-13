# UML类图与设计模式

---

## UML类图
#### 类图的几种关系：依赖、实现、泛化、组合、聚合、关联。
1. 它们关系的强弱顺序：泛化= 实现> 组合> 聚合> 关联> 依赖
2. 关联、聚合、组合只能配合语义，结合上下文才能够判断出来，而只给出一段代码让我们判断是关联，聚合，还是组合关系，则是无法判断的
##### 依赖
> 依赖是一种比关联关系更弱的关系。依赖关系存在偶然性、临时性。
比如：我会开车，别人给我一辆车，我就能开。这时候车子和我的关系就是依赖关系.<br>
UML类图使用带箭头的虚线表示，箭头从使用类指向被依赖的类。<br>
![image](https://github.com/IKNOWJLY/Interview/blob/master/contents/img/Dependency.png)
##### 实现
>实现是另外一个对象之间耦合度最大的关系之一。它表示的是接口和实现类的关系。比如：ImplementsClass类实现Interface类。
UML类图使用带三角的虚线表示，由子类指向父类。<br>
![image](https://github.com/IKNOWJLY/Interview/blob/master/contents/img/Realization.png)

##### 继承(泛化)
>泛化是对象之间耦合度最大的关系之一。它表示的是继承关系。比如：son类继承Father类。
UML类图使用带三角的实线表示，由子类指向父类。<br>
![image](https://github.com/IKNOWJLY/Interview/blob/master/contents/img/Generalization.png)

##### 关联关系
>关联是次于聚合的关系。关联关系一般是长期性的，而且双方的关系一般是平等的、关联可以是单向、双向的。比如：我有一个车子，我和车子的关系就是单向关联关系。箭头指向车子。
UML类图使用一条直线箭头连接，箭头所指的方向为被关联的对象。<br>
![image](https://github.com/IKNOWJLY/Interview/blob/master/contents/img/Association.png)

##### 组合关系
>组合是次于继承(泛化)、实现的关系，它表示的是整体和部分的关系。比如人和手的关系，当人不在了，手也就没了。
UML类图使用用实心菱形+实线+箭头图示表示，菱形的一边指的的拥有者ClassA，实线所在的一边为被拥有者ClassB。<br>
![image](https://github.com/IKNOWJLY/Interview/blob/master/contents/img/Composition.png)

##### 聚合关系
>聚合是次于组合的关系，它表示的是是一个对象拥有另一个对象的关系。比如：车子和轮胎之间的关系。但是车子没了轮胎还是可以存在。
UML类图使用空心菱形+实线+箭头图示表示，菱形的一边指的的拥有者ClassA，实线所在的一边为被拥有者ClassB。<br>
![image](https://github.com/IKNOWJLY/Interview/blob/master/contents/img/Aggregation.png)

---

## 设计模式

### 创建型模式
这些设计模式提供了一种在创建对象的同时隐藏创建逻辑的方式，而不是使用 new 运算符直接实例化对象。

##### ！工厂模式（Factory Pattern）

##### ！抽象工厂模式（Abstract Factory Pattern）

##### ！单例模式（Singleton Pattern）

##### 建造者模式（Builder Pattern）

##### 原型模式（Prototype Pattern）

##### 参考资料

---

### 结构型模式
这些设计模式关注类和对象的组合。继承的概念被用来组合接口和定义组合对象获得新功能的方式。
##### ！适配器模式（Adapter Pattern）

##### 桥接模式（Bridge Pattern）

##### 过滤器模式（Filter、Criteria Pattern）

##### 组合模式（Composite Pattern）

##### ！装饰器模式（Decorator Pattern）

##### 外观模式（Facade Pattern）

##### 享元模式（Flyweight Pattern）

##### ！代理模式（Proxy Pattern）

##### 参考资料

---

### 行为型模式
这些设计模式特别关注对象之间的通信。
##### ！责任链模式（Chain of Responsibility Pattern）

##### 命令模式（Command Pattern）

##### 解释器模式（Interpreter Pattern）

##### 迭代器模式（Iterator Pattern）

##### 中介者模式（Mediator Pattern）

##### 备忘录模式（Memento Pattern）

##### ！观察者模式（Observer Pattern）

##### 状态模式（State Pattern）

##### 空对象模式（Null Object Pattern）

##### ！策略模式（Strategy Pattern）

##### ！模板模式（Template Pattern）

##### 访问者模式（Visitor Pattern）

##### 参考资料

---

> Contributors : Hollis、Kevin Lee
>
> Reviewers：Hollis、Kevin Lee
