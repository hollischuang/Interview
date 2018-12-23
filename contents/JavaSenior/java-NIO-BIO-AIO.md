# Java BIO、NIO、AIO

## java io包

- 基于字节操作的 I/O 接口：InputStream 和 OutputStream
- 基于字符操作的 I/O 接口：Writer 和 Reader
- 基于磁盘操作的 I/O 接口：File
- 基于网络操作的 I/O 接口：Socket

## java BIO (同步阻塞) 的特点：

### 面向流

Java NIO和IO之间第一个最大的区别是，IO是面向流的，NIO是面向缓冲区的，面向流的意思是，每次从流中读一个或多个字节，直至读取所有字节，它们没有被缓存在任何地方。

虽然stream等有buffer开头的扩展类，但只是流的包装类，还是从流读到缓冲区，而NIO却是直接读到buffer中进行操作

### 字节流
字节流主要是操作byte类型数据，以byte数组为准，主要操作类就是OutputStream、InputStream

### 字节流的继承关系


### 字符流的继承关系


### java BIO 文件操作

~~~java
private void copy(String in, String out) throws Exception {  
    FileInputStream fin = new FileInputStream(in);
    FileOutputStream fout = new FileOutputStream(out);

    byte[] buf = new byte[4096];
    int read;
    while ((read = fin.read(buf)) > 0) {
        fout.write(buf, 0, read);
    }

    fin.close();
    fout.close();
}

~~~

### 字符流

在程序中一个字符等于两个字节，那么java提供了Reader、Writer两个专门操作字符流的类。

~~~java
class FireWriterDemo {  
    public static void main(String[] args) throws IOException {     
       FileWriter fw = new FileWriter("F:\\1.txt");//目的是明确数据要存放的目的地。  

        fw.write("hello world!");  

        //刷新流对象缓冲中的数据，将数据刷到目的地中  
        fw.flush();  

        //关闭流资源，但是关闭之前会刷新一次内部缓冲中的数据。当我们结束输入时候，必须close();  
        fw.write("first_test");  
        fw.close();  
        //flush和close的区别：flush刷新后可以继续输入，close刷新后不能继续输入。  
    }  
}
~~~
### 字节流，字符流的区别

- 字节流是最基本的，都是基于InputStream和OutputStream主要用在处理二进制数据，它是按字节来处理的。

- 字符流是基于Reader，Wirter来处理字符的

- 字节流在操作的时候本身是不会用到缓冲区（内存）的，是与文件本身直接操作的，而字符流在操作的时候是使用到缓冲区的

- 字节流在操作文件时，即使不关闭资源（close方法），文件也能输出，但是如果字符流不使用close方法的话，则不会输出任何内容，说明字符流用的是缓冲区，并且可以使用flush方法强制进行刷新缓冲区，这时才能在不close的情况下输出内容

- 在所有的硬盘上保存文件或进行传输的时候都是以字节的方法进行的，包括图片也是按字节完成，而字符是只有在内存中才会形成的，所以使用字节的操作是最多的。

- 如果要java程序实现一个拷贝功能，应该选用字节流进行操作（可能拷贝的是图片），并且采用边读边写的方式（节省内存）

### 字节流，字符流的关联

- 很多的数据是文本，又提出了字符流的概念，它是按虚拟机的encode来处理，也就是要进行字符集的转化这两个之间通过 InputStreamReader,OutputStreamWriter来关联。

- 实际上是通过byte[]和String来关联在实际开发中出现的汉字问题实际上都是在字符流和字节流之间转化不统一而造成的在从字节流转化为字符流时，实际上就是byte[]转化为String时，public String(byte bytes[], String charsetName)有一个关键的参数字符集编码，通常我们都省略了，那系统就用操作系统的lang而在字符流转为字节流时，实际上是String转化为byte[]时，byte[] String.getBytes(String charsetName)也是一样的道理至于java.io中还出现了许多其他的流，按主要是为了提高性能和使用方便，BufferedInputStream,PipedInputStream等

### 同步阻塞IO：

- 同步阻塞IO，BIO 即blocking IO是java最早的io实现方式，服务器实现模式为一个连接一个线程，即客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情，这个线程也要占用当前系统资源，可以通过线程池机制改善，但是如果线程过多，每个线程都在占用这资源，就可能会造成cpu或者内存使用率过高。

- 我们知道阻塞I/O在调用InputStream.read()方法时是阻塞的，它会一直等到数据到来时（或超 时）才会返回；同样，在调用ServerSocket.accept()方法时，也会一直阻塞到有客户端连接才会返回，每个客户端连接过来后，服务端都会 启动一个线程去处理该客户端的请求

### 同步阻塞IO的缺点：
- 当客户端多时，会创建大量的线程。
    
- 每个线程都要占用栈空间和一些CPU时间，且不会释放资源

### 用BIO实现一个阻塞的socket

- 服务器端：

~~~java
import java.io.IOException;  
import java.io.InputStream;  
import java.io.OutputStream;  
import java.net.ServerSocket;  
import java.net.Socket;  

public class BlockSocket extends Thread {  

    private int port = 8000;  
    /** 为客户端分配编号  */  
    private static int sequence = 0;  

    public BlockSocket (int port) {  
        this.port = port;  
    }  

    @Override  
    public void run() {  
        Socket socket = null;  
        try {  
            ServerSocket serverSocket = new ServerSocket(this.port);  
            while(true) {  
                socket = serverSocket.accept(); // 监听  
                this.handleMessage(socket); // 处理一个连接过来的客户端请求  
            }  
        } catch (IOException e) {  
            e.printStackTrace();  
        }  
    }  

    private void handleMessage(Socket socket) throws IOException {  
        InputStream in = socket.getInputStream(); // 客户端->服务端  
        OutputStream out = socket.getOutputStream(); //服务端->客户端  
        int receiveBytes;  
        byte[] receiveBuffer = new byte[128];  
        String clientMessage = "";  
        if((receiveBytes=in.read(receiveBuffer))!=-1) {  
            clientMessage = new String(receiveBuffer, 0, receiveBytes);  
            if(clientMessage.startsWith("I am the client")) {  
                String serverResponseWords =   
                    "I am the server, and you are the " + (++sequence) + "th client.";  
                out.write(serverResponseWords.getBytes());  
            }  
        }  
        out.flush();  
        System.out.println("Server: receives clientMessage->" + clientMessage);  
    }  

    public static void main(String[] args) {  
        BlockSocket server = new BlockSocket (1983);  
        server.start();  
    }  
} 

~~~
- 客户端：

~~~java
import java.io.IOException;  
import java.io.InputStream;  
import java.io.OutputStream;  
import java.net.Socket;  
import java.net.UnknownHostException;  
import java.util.Date;  

public class BioSocketClient{  

    private String ipAddress;  
    private int port;  
    private static int pos = 0;  

    public BioSocketClient() {}  

    public BioSocketClient(String ipAddress, int port) {  
        this.ipAddress = ipAddress;  
        this.port = port;  
    }  

    public void send(byte[] data) {  
        Socket socket = null;  
        OutputStream out = null;  
        InputStream in = null;  
        try {  
            socket = new Socket(this.ipAddress, this.port);  
            // 发送请求  
            out = socket.getOutputStream();  
            out.write(data);   
            out.flush();  
            // 接收响应  
            in = socket.getInputStream();  
            int totalBytes = 0;  
            int receiveBytes = 0;  
            byte[] receiveBuffer = new byte[128];  
            if((receiveBytes=in.read(receiveBuffer))!=-1) {  
                totalBytes += receiveBytes;  
            }  
            String serverMessage = new String(receiveBuffer, 0, receiveBytes);  
            System.out.println("Client: receives serverMessage->" + serverMessage);  
        } catch (UnknownHostException e) {  
            e.printStackTrace();  
        } catch (IOException e) {  
            e.printStackTrace();  
        } catch (Exception e) {  
            e.printStackTrace();  
        } finally {  
            try {  
                // 发送请求并接收到响应，通信完成，关闭连接  
                out.close();  
                in.close();  
                socket.close();  
            } catch (IOException e) {  
                e.printStackTrace();  
            }  
        }  
    }  

    public static void main(String[] args) {  
        int n = 1;  
        StringBuffer data = new StringBuffer();  
        Date start = new Date();  
        for(int i=0; i<n; i++) {  
            data.delete(0, data.length());  
            data.append("I am the client ").append(++pos).append(".");  
            BioSocketClient= new BioSocketClient("localhost", 1983);  
            client.send(data.toString().getBytes());  
        }  
        Date end = new Date();  
        long cost = end.getTime() - start.getTime();  
        System.out.println(n + " requests cost " + cost + " ms.");  
    }  
}  
~~~
## java NIO(同步非阻塞) 的特点

### 面向块的I/O
　　BIO是面向流的I/O。流I/O一次处理一个字节。NIO则是面向块的I/O，是以数据块为单位。

　　NIO中引入了缓冲区（Buffer）的概念，缓冲区作为传输数据的基本单位块，所有对数据的操作都是基于将数据移进/移出缓冲区而来；读数据的时候从缓冲区中取，写的时候将数据填入缓冲区。BIO中也有相应的缓冲区过滤器流（BufferedInputStream等），但是移进/移出的操作是由程序员来包装的。NIO的缓冲区则不然，对缓冲区的移进/移出操作是由底层操作系统来实现的。

## 非阻塞的I/O

Java IO的各种流是阻塞的，当一个线程调用read() 或 write()时，该线程被阻塞，直到有一些数据被读取，或数据完全写入。

Java NIO的非阻塞模式，使一个线程从某通道发送请求读取数据，但是它仅能得到目前可用的数据，如果目前没有数据可用时，就什么都不会获取。而不是保持线程阻塞，所以直至数据变的可以读取之前，该线程可以继续做其他的事情。

　　NIO提供的Socket可以用非阻塞的方式工作，并且支持就绪性选择，减少了资源消耗和CPU在线程间的切换，在管理线程效率上比传统Socket高。

## 选择器（Selectors）--(Reactor模式结构)

Java NIO的选择器允许一个单独的线程来监视多个输入通道，你可以注册多个通道使用一个选择器，然后使用一个单独的线程来“选择”通道：这些通道里已经有可以处理的输入，或者选择已准备写入的通道。这种选择机制，使得一个单独的线程很容易来管理多个通道.

比如说Socket的处理方式，服务端初始化了一个Socket线程池，然后等着客户端连接。

BIO的处理方式：new 很多客户端线程和服务端线程建立连接，构成一对一结构，假如服务端的线程满了，那客户端只能等待

NIO的处理方式：不需要一对一的线程，这个连接会被注册到多路复用器上面，所有的连接只需要一个线程就可以，当这个线程 中的多路复用器进行轮询的时候，发现连接上有请求的话，才开启一个线程进行处理。

