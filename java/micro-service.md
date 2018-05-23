# 微服务

## 1. 微服务的概念


## 2.前后端的分离的实现


## 3.RPC 的实现原理


## 4.dubbo 的实现原理


## 5.怎么理解RESTful


## 6.如何设计一个良好的api


1. 拼写要准确接口函数一旦发布就不能改了，要保持兼容性，拼写错误也不能改了，所以要仔细检查拼写，否则会被同行嘲笑很多年。著名悲剧：unix 的 creat

2. 不仅是英文单词不要拼错，时态也不要错。比如：返回bool的判断函数，单数要用 is 复数要用are，这样你的命名就和文档中的描述保持了一致性。表示状态的变量或者函数要注意时态，比如 onXxxxChanged 表示xxx已经变化了，isConnecting表示正在连接。 正确的时态可以给使用者传递更丰富的信息。

3. 函数最好是动宾结构动宾结构就是  doSomething，这样的函数命名含义明确比如： openFile, allocBuffer, setName如果这个函数的动词宾语就是这个对象本身，那么可以省略掉宾语

4. 属性命名最好是定语+名词比如 fileName, maxSize, textColor

5. 不要用生僻单词，这不是秀英语的地方，也不要用汉语拼音比如：rendezvous，估计大多数人要去查词典才知道什么意思，这个词源自法语，是约会的意思。Symbian OS里有个用它命名的函数，开发Symbian的是英国人，也许人家觉得很平常吧，反正我是查了词典才知道的。

6. 不要自己发明缩写除非是约定俗成已经被广泛使用的缩写，否则老老实实用完整拼写。坏例子:  count->cnt, manager->mngr password->pw button->btn现代的IDE都有很好的自动完成功能，名字长一点没关系的，可读性更重要。

7. 保持方法的对称性，有些方法一旦出现就应该是成对的，比如 有open就要有close，有alloc就要有free，有add就要有remove，这些单词基本是固定搭配的，使用者就很容易理解。如果 open对应clear就有点让人困惑了。

8. 站在使用者的角度去思考，API设计也要讲究用户体验。好的API设计应该是符合直觉，能望文生义的，让使用者能用尽量简洁的代码完成调用。

## 7.如何设计RESTful API 的幂等性


## 8. 如何保证接口的幂等性


## 9.说说CAP 定理 和 BASE 理论


## 10.怎么解决数据一致性问题


## 11. 说说最终一致性的实现方案


## 12. 微服务如何进行数据管理


## 13.如何应对微服务的链式调用异常


## 14.如何快速追踪与定位问题


## 15. 微服务的安全


>引用
1. [API_Design_Principles](http://wiki.qt.io/API_Design_Principles)
2. [microsoft design-guidelines](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/index)
3. [在高并发的核心技术中如何实现幂等性](https://www.jianshu.com/p/ce5f5aa3d17f)
4. [如何保证幂等性](https://help.aliyun.com/document_detail/25693.html)
5. [分布式系统互斥性与幂等性问题的分析与解决](https://zhuanlan.zhihu.com/p/22820761)

