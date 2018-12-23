# Netty

## 1.为什么选择netty
在网络编程领域， Netty是Java的卓越框架。 

用较简单的抽象隐藏底层实现的复杂性

它驾驭了Java高级API的能力， 并将其隐藏在一个易于使用的API之后。 Netty使你可以专注于自己真正感兴趣的——你的应用程序的独一无二的
价值

| 分 类    |Netty 的特性
| ---------| --------------------------------------------------------------------------------------------------------------------------------|
|设计      | 统一的 API，支持多种传输类型，阻塞的和非阻塞的  简单而强大的线程模型<br> 真正的无连接数据报套接字支持 <br>链接逻辑组件以支持复用|
|易于使用  | 详实的Javadoc和大量的示例集<br> 不需要超过JDK 1.6+③的依赖。（一些可选的特性可能需要Java 1.7+和/或额外的依赖）                  |        
|性能      | 拥有比 Java 的核心 API 更高的吞吐量以及更低的延迟<br> 得益于池化和复用， 拥有更低的资源消耗<br> 最少的内存复制 |
|健壮性    | 不会因为慢速、快速或者超载的连接而导致 OutOfMemoryError<br> 消除在高速网络中 NIO 应用程序常见的不公平读/写比率 |
|安全性    | 完整的 SSL/TLS 以及 StartTLS 支持<br> 可用于受限环境下，如 Applet 和 OSGI|
|社区驱动  | 发布快速而且频繁|


## 2.业务中，netty的使用场景

有了Netty，你可以实现自己的HTTP服务器，FTP服务器，UDP服务器，RPC服务器，WebSocket服务器，Redis的Proxy服务器，MySQL的Proxy服务器等等。

HTTP服务器之所以称为HTTP服务器，是因为编码解码协议是HTTP协议，如果协议是Redis协议，那它就成了Redis服务器，如果协议是WebSocket，那它就成了WebSocket服务器，等等。

使用Netty你就可以定制编解码协议，实现自己的特定协议的服务器。


Netty是建立在NIO基础之上，Netty在NIO之上又提供了更高层次的抽象。

在Netty里面，Accept连接可以使用单独的线程池去处理，读写操作又是另外的线程池来处理。

Accept连接和读写操作也可以使用同一个线程池来进行处理。

而请求处理逻辑既可以使用单独的线程池进行处理，也可以跟放在读写线程一块处理。

线程池中的每一个线程都是NIO线程。用户可以根据实际情况进行组装，构造出满足系统需求的并发模型。

Netty提供了内置的常用编解码器，包括行编解码器［一行一个请求］，前缀长度编解码器［前N个字节定义请求的字节长度］，可重放解码器［记录半包消息的状态］，HTTP编解码器，WebSocket消息编解码器等等Netty提供了一些列生命周期回调接口，当一个完整的请求到达时，当一个连接关闭时，当一个连接建立时，用户都会收到回调事件，然后进行逻辑处理。

Netty可以同时管理多个端口，可以使用NIO客户端模型，这些对于RPC服务是很有必要的。

Netty除了可以处理TCP Socket之外，还可以处理UDP Socket。在消息读写过程中，需要大量使用ByteBuffer，Netty对ByteBuffer在性能和使用的便捷性上都进行了优化和抽象。

## 3.什么是Tcp 的粘包/拆包


## 4.Tcp 的粘包/拆包的解决办法


## 5.netty 的线程模型

线程模型指定了操作系统、编程语言、框架、或者应用程序的上下文中的线程管理的关键方面

显而易见地，如何以及何时创建线程将对应用程序代码的执行产生显著的影响






详见[netty-threading-model](http://www.infoq.com/cn/articles/netty-threading-model)
## 6.netty 的零拷贝
根据 Wiki 对 Zero-copy 的定义:

"Zero-copy" describes computer operations in which the CPU does not perform the task of copying data from one memory area to another. This is frequently used to save CPU cycles and memory bandwidth when transmitting a file over a network.

即所谓的 Zero-copy, 就是在操作数据时, 不需要将数据 buffer 从一个内存区域拷贝到另一个内存区域. 因为少了一次内存的拷贝, 因此 CPU 的效率就得到的提升.

在 OS 层面上的 Zero-copy 通常指避免在 用户态(User-space) 与 内核态(Kernel-space) 之间来回拷贝数据. 例如 Linux 提供的 mmap 系统调用, 它可以将一段用户空间内存映射到内核空间, 当映射成功后, 用户对这段内存区域的修改可以直接反映到内核空间; 同样地, 内核空间对这段区域的修改也直接反映用户空间. 正因为有这样的映射关系, 我们就不需要在 用户态(User-space) 与 内核态(Kernel-space) 之间拷贝数据, 提高了数据传输的效率.

而需要注意的是, Netty 中的 Zero-copy 与上面我们所提到到 OS 层面上的 Zero-copy 不太一样, Netty的 Zero-coyp 完全是在用户态(Java 层面)的, 它的 Zero-copy 的更多的是偏向于 优化数据操作 这样的概念.

Netty 的 Zero-copy 体现在如下几个个方面:

* Netty 提供了 CompositeByteBuf 类, 它可以将多个 ByteBuf 合并为一个逻辑上的 ByteBuf, 避免了各个 ByteBuf 之间的拷贝.
* 通过 wrap 操作, 我们可以将 byte[] 数组、ByteBuf、ByteBuffer等包装成一个 Netty ByteBuf 对象, 进而避免了拷贝操作.
* ByteBuf 支持 slice 操作, 因此可以将 ByteBuf 分解为多个共享同一个存储区域的 ByteBuf, 避免了内存的拷贝.
* 通过 FileRegion 包装的FileChannel.tranferTo 实现文件传输, 可以直接将文件缓冲区的数据发送到目标 Channel, 避免了传统通过循环 write 方式导致的内存拷贝问题.

## 7.netty 的内部执行流程


## 8.netty 的重连接


当我们用Netty实现一个TCP client时，我们当然希望当连接断掉的时候Netty能够自动重连。
Netty Client有两种情况下需要重连：

Netty Client启动的时候需要重连
在程序运行中连接断掉需要重连。
对于第一种情况，Netty的作者在[stackoverflow](https://stackoverflow.com/questions/19739054/whats-the-best-way-to-reconnect-after-connection-closed-in-netty)上给出了解决方案，

Channel.closeFuture() returns a ChannelFuture that will notify you when the channel is closed. 

You can add a ChannelFutureListener to the future in B so that you can make another connection attempt there.

You probably want to repeat this until the connection attempt succeeds finally:
~~~java
private void doConnect() {
    Bootstrap b = ...;
    b.connect().addListener((ChannelFuture f) -> {
        if (!f.isSuccess()) {
            long nextRetryDelay = nextRetryDelay(...);
            f.channel().eventLoop().schedule(nextRetryDelay, ..., () -> {
                doConnect();
            }); // or you can give up at some point by just doing nothing.
        }
    });
}
~~~
对于第二种情况，Netty的例子uptime中实现了一种[解决方案]()。

而Thomas在他的文章中提供了这两种方式的实现的[例子](http://tterm.blogspot.jp/2014/03/netty-tcp-client-with-reconnect-handling.html)。

实现ChannelFutureListener 用来启动时监测是否连接成功，不成功的话重试：
~~~java
public class Client  
 {  
   private EventLoopGroup loop = new NioEventLoopGroup();  
   public static void main( String[] args )  
   {  
     new Client().run();  
   }  
   public Bootstrap createBootstrap(Bootstrap bootstrap, EventLoopGroup eventLoop) {  
     if (bootstrap != null) {  
       final MyInboundHandler handler = new MyInboundHandler(this);  
       bootstrap.group(eventLoop);  
       bootstrap.channel(NioSocketChannel.class);  
       bootstrap.option(ChannelOption.SO_KEEPALIVE, true);  
       bootstrap.handler(new ChannelInitializer<SocketChannel>() {  
         @Override 
         protected void initChannel(SocketChannel socketChannel) throws Exception {  
           socketChannel.pipeline().addLast(handler);  
         }  
       });  
       bootstrap.remoteAddress("localhost", 8888);
       bootstrap.connect().addListener(new ConnectionListener(this)); 
     }  
     return bootstrap;  
   }  
   public void run() {  
     createBootstrap(new Bootstrap(), loop);
   }  
 }
 ~~~
ConnectionListener 负责重连：
~~~java
public class ConnectionListener implements ChannelFutureListener {  
  private Client client;  
  public ConnectionListener(Client client) {  
    this.client = client;  
  }  
  @Override 
  public void operationComplete(ChannelFuture channelFuture) throws Exception {  
    if (!channelFuture.isSuccess()) {  
      System.out.println("Reconnect");  
      final EventLoop loop = channelFuture.channel().eventLoop();  
      loop.schedule(new Runnable() {  
        @Override 
        public void run() {  
          client.createBootstrap(new Bootstrap(), loop);  
        }  
      }, 1L, TimeUnit.SECONDS);  
    }  
  }  
}
~~~
同样在ChannelHandler监测连接是否断掉，断掉的话也要重连：
~~~java
public class MyInboundHandler extends SimpleChannelInboundHandler {  
   private Client client;  
   public MyInboundHandler(Client client) {  
     this.client = client;  
   }  
   @Override 
   public void channelInactive(ChannelHandlerContext ctx) throws Exception {  
     final EventLoop eventLoop = ctx.channel().eventLoop();  
     eventLoop.schedule(new Runnable() {  
       @Override 
       public void run() {  
         client.createBootstrap(new Bootstrap(), eventLoop);  
       }  
     }, 1L, TimeUnit.SECONDS);  
     super.channelInactive(ctx);  
   }  
 }
 ~~~