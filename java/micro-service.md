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

- 全局唯一ID

如果使用全局唯一ID，就是根据业务的操作和内容生成一个全局ID，在执行操作前先根据这个全局唯一ID是否存在，来判断这个操作是否已经执行。如果不存在则把全局ID，存储到存储系统中，比如数据库、redis等。如果存在则表示该方法已经执行。

从工程的角度来说，使用全局ID做幂等可以作为一个业务的基础的微服务存在，在很多的微服务中都会用到这样的服务，在每个微服务中都完成这样的功能，会存在工作量重复。另外打造一个高可靠的幂等服务还需要考虑很多问题，比如一台机器虽然把全局ID先写入了存储，但是在写入之后挂了，这就需要引入全局ID的超时机制。

使用全局唯一ID是一个通用方案，可以支持插入、更新、删除业务操作。但是这个方案看起来很美但是实现起来比较麻烦，下面的方案适用于特定的场景，但是实现起来比较简单。

- 去重表

这种方法适用于在业务中有唯一标的插入场景中，比如在以上的支付场景中，如果一个订单只会支付一次，所以订单ID可以作为唯一标识。这时，我们就可以建一张去重表，并且把唯一标识作为唯一索引，在我们实现时，把创建支付单据和写入去去重表，放在一个事务中，如果重复创建，数据库会抛出唯一约束异常，操作就会回滚。

- 插入或更新

这种方法插入并且有唯一索引的情况，比如我们要关联商品品类，其中商品的ID和品类的ID可以构成唯一索引，并且在数据表中也增加了唯一索引。这时就可以使用InsertOrUpdate操作。在mysql数据库中如下：
~~~sql
insert into goods_category (goods_id,category_id,create_time,update_time) 
       values(#{goodsId},#{categoryId},now(),now()) 
       on DUPLICATE KEY UPDATE
       update_time=now()
~~~       
- 多版本控制

这种方法适合在更新的场景中，比如我们要更新商品的名字，这时我们就可以在更新的接口中增加一个版本号，来做幂等
boolean updateGoodsName(int id,String newName,int version);
在实现时可以如下
~~~sql
update goods set name=#{newName},version=#{version} where id=#{id} and version<${version}
~~~
- 状态机控制

这种方法适合在有状态机流转的情况下，比如就会订单的创建和付款，订单的付款肯定是在之前，这时我们可以通过在设计状态字段时，使用int类型，并且通过值类型的大小来做幂等，比如订单的创建为0，付款成功为100。付款失败为99

在做状态机更新时，我们就这可以这样控制
~~~sql
update `order` set status=#{status} where id=#{id} and status<#{status}
~~~
## 8. 微服务的安全

## 9.说说CAP 定理 和 BASE 理论


## 10.怎么解决数据一致性问题


## 11. 说说最终一致性的实现方案


## 12. 微服务如何进行数据管理


## 13.如何应对微服务的链式调用异常


## 14.如何快速追踪与定位问题

- CPU占用较高场景

收集当前CPU占用较高的线程信息，执行如下命令：
~~~sh
top -H -p PID -b -d 1 -n 1 > top.log
~~~
或
~~~sh
top -H -p PID
~~~

显示的都是某一个进程内的线程信息，找到cpu消耗最高的线程id，再配合jstack来分析耗cpu的代码位置，那如何分析呢？

先执行jstack获取线程信息
~~~sh
jstack -l PID > jstackl.log
~~~
将PID（29978）转成16进制：0x751a，16进制转换工具很多可以在线随便搜索一个或者基本功好的自己计算。

打开jstackl.log，查找nid=0x751a的信息，这样就定位到了具体的代码位置，

通过上面的步骤就可以轻松的定位那个线程导致cpu过高，当然也可以通过其他方式来定位，下面介绍一个快捷的方式
~~~bash
#线程cpu占用
#!/bin/bash

[ $# -ne 1 ] && exit 1

jstack $1 >/tmp/jstack.log

for cpu_tid in `ps -mp $1 -o THREAD,tid,time|sort -k2nr| sed -n '2,15p' |awk '{print$2"_"$(NF-1)}'`;do

cpu=`echo $cpu_tid | cut -d_ -f1`

tid=`echo $cpu_tid | cut -d_ -f2`

xtid=`printf "%x\n" $tid`

echo -e "\033[31m========================$xtid $cpu%\033[0m"

cat /tmp/jstack.log | sed -n -e "/0x$xtid/,/^$/ p"

#cat /tmp/jstack.log | grep "$xtid" -A15

done

rm /tmp/jstack.log
~~~
上述命令会以百分比的方式来显示每个线程的cpu消耗百分比

- 内存消耗过高场景

收集当前活跃对象数据量信息，执行以下命令获取
~~~sh
jmap -histo:live pid > jmaplive.log
~~~
ps. jmap -histo:live 数据可以多进行几次，比如说间隔几分钟输出一次，然后对比两个文件的差异可以看出gc回收的对象，如果多次结果没有差异并且gc频繁执行，证明剩余对象在引用无法gc回收，这时就需要对服务进行限流给服务喘气的机会。

或者收集dump信息，通常这种获取方式需要较长时间执行，并产生大容量的dump文件，我们会考虑逐步废掉通过这个文件来分析。执行以下命令获取
~~~sh
jmap -dump:file=./dump.mdump pid
~~~
dump文件通过MAT工具来进行内存泄漏分析。

线程、内存分析工具
上面说过通过jstack生成的线程文件是可以通过工具来直接打开可视化分析的，这里我推荐使用：tda（Thread Dump Analyzer）

通过jmap -dump生成的dump文件也是可以通过工具来进行可视化分析的，这里我推荐使用MAT（Memory Analysis Tools）它可以通过eclipse plugin的方式使用或者独立的下载安装包使用。



>引用
1. [API_Design_Principles](http://wiki.qt.io/API_Design_Principles)
2. [microsoft design-guidelines](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/index)
3. [在高并发的核心技术中如何实现幂等性](https://www.jianshu.com/p/ce5f5aa3d17f)
4. [如何保证幂等性](https://help.aliyun.com/document_detail/25693.html)
5. [分布式系统互斥性与幂等性问题的分析与解决](https://zhuanlan.zhihu.com/p/22820761)
6. [设计 Restful API](http://www.importnew.com/28233.html)
7. [REST接口设计规范](http://wangwei.info/about-rest-api/)
8. [从 Feign 使用注意点到 RESTFUL 接口设计规范](http://www.importnew.com/27266.html)
9. [生产环境如何快速跟踪、分析、定位问题-Java](https://ningyu1.github.io/site/post/55-java-jvm-analysis/)