# BIO、NIO、AIO
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210715012522944.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1 BIO、NIO、AIO
```markdown
在Java的软件设计开发中，通信架构是不可避免的，我们在进行不
同系统或者不同进程之间的数据交互，或者在高并发下的通信场景
下都需要用到网络通信相关的技术，对于一些经验丰富的程序员来
说，Java早期的网络通信架构存在一些缺陷，其中最令人恼火
的是基于性能低下的同步阻塞式的I/O通信（BIO），随着互
联网开发下通信性能的高要求，Java在2002年开始支持了非阻
塞式的I/O通信技术(NIO)。大多数读者在学习网络通信相关技术
的时候，都只是接触到零碎的通信技术点，没有完整的技术体
系架构，以至于对于Java的通信场景总是没有清晰的解决方
案。本次课程将通过大量清晰直接的案例从最基础的BIO式通
信开始介绍到NIO , AIO，读者可以清晰的了解到阻塞、同步、
异步的现象、概念和特征以及优缺点。本课程结合了大量的
案例让读者可以快速了解每种通信架构的使用。
```

### 1.1 通信技术整体解决的问题
```markdown
局域网内的通信要求。
多系统间的底层消息传递机制。
高并发下，大数据量的通信场景需要。
游戏行业。无论是手游服务端，还是大型的网络游戏，Java
语言都得到越来越广泛的应用。
```

## 2 Java的I/O演进之路
### 2.1 I/O 模型基本说明
```markdown
I/O 模型：就是用什么样的通道或者说是通信模式和
架构进行数据的传输和接收，很大程度上决定了程序通信的
性能，Java 共支持 3 种网络编程的/IO 模型：BIO、NIO、AIO
实际通信需求下，要根据不同的业务场景和性能需求决
定选择不同的I/O模型
```
### 2.2 I/O模型
####  2.2.1 Java BIO
```markdown
同步并阻塞(传统阻塞型)，服务器实现模式为一个连接
一个线程，即客户端有连接请求时服务器端就需要启动
一个线程进行处理，如果这个连接不做任何事情会造成
不必要的线程开销
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210323082329416.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

#### 2.2.2 Java NIO
```markdown
Java NIO ： 同步非阻塞，服务器实现模式为一个线程处理
多个请求(连接)，即客户端发送的连接请求都会注册到多路
复用器上，多路复用器轮询到连接有 I/O 请求就进行处理 
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210323082552196.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)


#### 2.2.3  Java AIO
```markdown
Java AIO(NIO.2) ： 异步 异步非阻塞，服务器实现模式为一
个有效请求一个线程，客户端的I/O请求都是由OS先完成了
再通知服务器应用去启动线程进行处理，一般适用于连接数
较多且连接时间较长的应用
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210323082936216.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

### 2.3 BIO、NIO、AIO 适用场景分析
```markdown
1 BIO方式适用于连接数目比较小且固定的架构，这种方式对服
务器资源要求比较高，并发局限于应用中，JDK1.4以前的唯
一选择，但程序简单易理解。
2 NIO 方式适用于连接数目多且连接比较短（轻操作）的架
构，比如聊天服务器，弹幕系统，服务器间通讯等。
编程比较复杂，JDK1.4 开始支持。

3 AIO 方式使用于连接数目多且连接比较长（重操作）的架
构，比如相册服务器，充分调用 OS 参与并发操作，编程
比较复杂，JDK7 开始支持。
```
## 3 JAVA BIO深入剖析
### 3.1 Java BIO 基本介绍
```markdown
Java BIO 就是传统的 java io  编程，其相关的类和接
口在 java.io
BIO(blocking I/O) ： 同步阻塞，服务器实现模式为一个连
接一个线程，即客户端有连接请求时服务器端就需要启
动一个线程进行处理，如果这个连接不做任何事情会造成
不必要的线程开销，可以通过线程池机制改善(实现多
个客户连接服务器).
```
### 3.2 Java BIO 工作机制
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210323083953769.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
对 BIO  编程流程的梳理
1) 服务器端启动一个 ServerSocket，注册端口，调用accpet方
法监听客户端的Socket连接。
2) 客户端启动 Socket对服务器进行通信，默认情况下服务器端
需要对每个客户 建立一个线程与之通讯
```

### 3.3  传统的BIO编程实例回顾
```markdown
网络编程的基本模型是Client/Server模型，也就是两个进程之
间进行相互通信，其中服务端提供位置信（绑定IP地址和端口），
客户端通过连接操作向服务端监听的端口地址发起连接请
求，基于TCP协议下进行三次握手连接，连接成功后，双方
通过网络套接字（Socket）进行通信。

传统的同步阻塞模型开发中，服务端ServerSocket负责绑定
IP地址，启动监听端口；客户端Socket负责发起连接操作。连
接成功后，双方通过输入和输出流进行同步阻塞式通信。 
基于BIO模式下的通信，客户端 - 服务端是完全同步，完全耦合的。	  
```
#### 3.3.1 BIO模式下多发和多收消息
```markdown
客户端案例如下
```
```java
import java.io.OutputStream;
import java.io.PrintStream;
import java.net.Socket;
/**
    目标: Socket网络编程。

    Java提供了一个包：java.net下的类都是用于网络通信。
    Java提供了基于套接字（端口）Socket的网络通信模式，我们基于这种模式就可以直接实现TCP通信。
    只要用Socket通信，那么就是基于TCP可靠传输通信。

    功能1：客户端发送一个消息，服务端接口一个消息，通信结束！！

    创建客户端对象：
        （1）创建一个Socket的通信管道，请求与服务端的端口连接。
        （2）从Socket管道中得到一个字节输出流。
        （3）把字节流改装成自己需要的流进行数据的发送
    创建服务端对象：
        （1）注册端口
        （2）开始等待接收客户端的连接,得到一个端到端的Socket管道
        （3）从Socket管道中得到一个字节输入流。
        （4）把字节输入流包装成自己需要的流进行数据的读取。

    Socket的使用：
        构造器：public Socket(String host, int port)
        方法：  public OutputStream getOutputStream()：获取字节输出流
               public InputStream getInputStream() :获取字节输入流

    ServerSocket的使用：
        构造器：public ServerSocket(int port)

    小结：
        通信是很严格的，对方怎么发你就怎么收，对方发多少你就只能收多少！！

 */
public class ClientDemo {
    public static void main(String[] args) throws Exception {
        System.out.println("==客户端的启动==");
        // （1）创建一个Socket的通信管道，请求与服务端的端口连接。
        Socket socket = new Socket("127.0.0.1",8888);
        // （2）从Socket通信管道中得到一个字节输出流。
        OutputStream os = socket.getOutputStream();
        // （3）把字节流改装成自己需要的流进行数据的发送
        PrintStream ps = new PrintStream(os);
        // （4）开始发送消息
        ps.println("我是客户端，我想约你吃小龙虾！！！");
        ps.flush();
    }
}
```
```markdown
服务端案例如下
```
```java
import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.ServerSocket;
import java.net.Socket;

/**
 * 服务端
 */
public class ServerDemo {
    public static void main(String[] args) throws Exception {
        System.out.println("==服务器的启动==");
        // （1）注册端口
        ServerSocket serverSocket = new ServerSocket(8888);
        //（2）开始在这里暂停等待接收客户端的连接,得到一个端到端的Socket管道
        Socket socket = serverSocket.accept();
        //（3）从Socket管道中得到一个字节输入流。
        InputStream is = socket.getInputStream();
        //（4）把字节输入流包装成自己需要的流进行数据的读取。
        BufferedReader br = new BufferedReader(new InputStreamReader(is));
        //（5）读取数据
        String line ;
        while((line = br.readLine())!=null){
            System.out.println("服务端收到："+line);
        }
    }
}
```
```markdown
小结
在以上通信中，服务端会一致等待客户端的消息，如果客户
端没有进行消息的发送，服务端将一直进入阻塞状态。
同时服务端是按照行获取消息的，这意味着客户端也必须按
照行进行消息的发送，否则服务端将进入等待消息的阻塞状态！
```

#### 3.3.2 BIO模式下多发和多收消息
```markdown
在前面的案例中，只能实现客户端发送消息，服务端
接收消息，并不能实现反复的收消息和反复的发消息，我
们只需要在客户端案例中，加上反复按照行发送消息的
逻辑即可！案例代码如下：
```
```markdown
客户端代码如下
```

```java
import java.io.OutputStream;
import java.io.PrintStream;
import java.net.Socket;
import java.util.Scanner;

/**
    目标: Socket网络编程。

    功能1：客户端可以反复发消息，服务端可以反复收消息

    小结：
        通信是很严格的，对方怎么发你就怎么收，对方发多少你就只能收多少！！

 */
public class ClientDemo {
    public static void main(String[] args) throws Exception {
        System.out.println("==客户端的启动==");
        // （1）创建一个Socket的通信管道，请求与服务端的端口连接。
        Socket socket = new Socket("127.0.0.1",8888);
        // （2）从Socket通信管道中得到一个字节输出流。
        OutputStream os = socket.getOutputStream();
        // （3）把字节流改装成自己需要的流进行数据的发送
        PrintStream ps = new PrintStream(os);
        // （4）开始发送消息
        Scanner sc = new Scanner(System.in);
        while(true){
            System.out.print("请说:");
            String msg = sc.nextLine();
            ps.println(msg);
            ps.flush();
        }
    }
}
```
```markdown
服务端代码如下
```
```java
import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.ServerSocket;
import java.net.Socket;

/**
 * 服务端
 */
public class ServerDemo {
    public static void main(String[] args) throws Exception {
        String s = "886";
        System.out.println("886".equals(s));
        System.out.println("==服务器的启动==");
        //（1）注册端口
        ServerSocket serverSocket = new ServerSocket(8888);
        //（2）开始在这里暂停等待接收客户端的连接,得到一个端到端的Socket管道
        Socket socket = serverSocket.accept();
        //（3）从Socket管道中得到一个字节输入流。
        InputStream is = socket.getInputStream();
        //（4）把字节输入流包装成  自己需要的流进行数据的读取。
        BufferedReader br = new BufferedReader(new InputStreamReader(is));
        //（5）读取数据
        String line ;
        while((line = br.readLine())!=null){
            System.out.println("服务端收到："+line);
        }
    }
}
```
```markdown
本案例中确实可以实现客户端多发多收
但是服务端只能处理一个客户端的请求，因为服务端
是单线程的。一次只能与一个客户端进行消息通信。
```

#### 3.3.3 BIO模式下接收多个客户端 
```markdown
在上述的案例中，一个服务端只能接收一个客户端
的通信请求，那么如果服务端需要处理很多个客户端的消
息通信请求应该如何处理呢，此时我们就需要在服务端
引入线程了，也就是说客户端每发起一个请求，服务
端就创建一个新的线程来处理这个客户端的请求，这样
就实现了一个客户端一个线程的模型，图解模式如下：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210323092007730.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
客户端案例代码如下
```
```java
/**
    目标: Socket网络编程。

    功能1：客户端可以反复发，一个服务端可以接收无数个客户端的消息！！

    小结：
         服务器如果想要接收多个客户端，那么必须引入线程，一个客户端一个线程处理！！

 */
public class ClientDemo {
    public static void main(String[] args) throws Exception {
        System.out.println("==客户端的启动==");
        // （1）创建一个Socket的通信管道，请求与服务端的端口连接。
        Socket socket = new Socket("127.0.0.1",7777);
        // （2）从Socket通信管道中得到一个字节输出流。
        OutputStream os = socket.getOutputStream();
        // （3）把字节流改装成自己需要的流进行数据的发送
        PrintStream ps = new PrintStream(os);
        // （4）开始发送消息
        Scanner sc = new Scanner(System.in);
        while(true){
            System.out.print("请说:");
            String msg = sc.nextLine();
            ps.println(msg);
            ps.flush();
        }
    }
}
```
```markdown
服务端案例代码如下
```
```java
/**
    服务端
 */
public class ServerDemo {
    public static void main(String[] args) throws Exception {
        System.out.println("==服务器的启动==");
        // （1）注册端口
        ServerSocket serverSocket = new ServerSocket(7777);
        while(true){
            //（2）开始在这里暂停等待接收客户端的连接,得到一个端到端的Socket管道
            Socket socket = serverSocket.accept();
            new ServerReadThread(socket).start();
            System.out.println(socket.getRemoteSocketAddress()+"上线了！");
        }
    }
}

class ServerReadThread extends Thread{
    private Socket socket;

    public ServerReadThread(Socket socket){
        this.socket = socket;
    }

    @Override
    public void run() {
        try{
            //（3）从Socket管道中得到一个字节输入流。
            InputStream is = socket.getInputStream();
            //（4）把字节输入流包装成自己需要的流进行数据的读取。
            BufferedReader br = new BufferedReader(new InputStreamReader(is));
            //（5）读取数据
            String line ;
            while((line = br.readLine())!=null){
                System.out.println("服务端收到："+socket.getRemoteSocketAddress()+":"+line);
            }
        }catch (Exception e){
            System.out.println(socket.getRemoteSocketAddress()+"下线了！");
        }
    }
}
```
```markdown
1 每个Socket接收到，都会创建一个线程，线程的竞争、切
换上下文影响性能；
2 每个线程都会占用栈空间和CPU资源；
3 并不是每个socket都进行IO操作，无意义的线程处理；  
4 客户端的并发访问增加时。服务端将呈现1:1的线程开销，访
问量越大，系统将发生线程栈溢出，线程创建失败，最终导致
进程宕机或者僵死，从而不能对外提供服务。
```
### 3.4 伪异步I/O编程
```markdown
在上述案例中：客户端的并发访问增加时。服务端将呈现1:1的
线程开销，访问量越大，系统将发生线程栈溢出，线程创建失
败，最终导致进程宕机或者僵死，从而不能对外提供服务。

接下来我们采用一个伪异步I/O的通信框架，采用线程池和任
务队列实现，当客户端接入时，将客户端的Socket封装
成一个Task(该任务实现java.lang.Runnable线程任务接口)
交给后端的线程池中进行处理。JDK的线程池维护一个消息
队列和N个活跃的线程，对消息队列中Socket任务进行处
理，由于线程池可以设置消息队列的大小和最大线程
数，因此，它的资源占用是可控的，无论多少个客户端
并发访问，都不会导致资源的耗尽和宕机。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210323141600962.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
客户端源码分析
```
```java
public class Client {
   public static void main(String[] args) {
      try {
         // 1.简历一个与服务端的Socket对象：套接字
         Socket socket = new Socket("127.0.0.1", 9999);
         // 2.从socket管道中获取一个输出流，写数据给服务端 
         OutputStream os = socket.getOutputStream() ;
         // 3.把输出流包装成一个打印流 
         PrintWriter pw = new PrintWriter(os);
         // 4.反复接收用户的输入 
         BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
         String line = null ;
         while((line = br.readLine()) != null){
            pw.println(line);
            pw.flush();
         }
      } catch (Exception e) {
         e.printStackTrace();
      }
   }
}
```
```markdown
线程池处理类
```

```java
// 线程池处理类
public class HandlerSocketThreadPool {
   
   // 线程池 
   private ExecutorService executor;
   
   public HandlerSocketThreadPool(int maxPoolSize, int queueSize){
      
      this.executor = new ThreadPoolExecutor(
            3, // 8
            maxPoolSize,  
            120L, 
            TimeUnit.SECONDS,
            new ArrayBlockingQueue<Runnable>(queueSize) );
   }
   
   public void execute(Runnable task){
      this.executor.execute(task);
   }
}
```
```markdown
服务端源码分析
```
```java
public class Server {
   public static void main(String[] args) {
      try {
         System.out.println("----------服务端启动成功------------");
         ServerSocket ss = new ServerSocket(9999);

         // 一个服务端只需要对应一个线程池
         HandlerSocketThreadPool handlerSocketThreadPool =
               new HandlerSocketThreadPool(3, 1000);

         // 客户端可能有很多个
         while(true){
            Socket socket = ss.accept() ; // 阻塞式的！
            System.out.println("有人上线了！！");
            // 每次收到一个客户端的socket请求，都需要为这个客户端分配一个
            // 独立的线程 专门负责对这个客户端的通信！！
            handlerSocketThreadPool.execute(new ReaderClientRunnable(socket));
         }

      } catch (Exception e) {
         e.printStackTrace();
      }
   }

}
class ReaderClientRunnable implements Runnable{

   private Socket socket ;

   public ReaderClientRunnable(Socket socket) {
      this.socket = socket;
   }

   @Override
   public void run() {
      try {
         // 读取一行数据
         InputStream is = socket.getInputStream() ;
         // 转成一个缓冲字符流
         Reader fr = new InputStreamReader(is);
         BufferedReader br = new BufferedReader(fr);
         // 一行一行的读取数据
         String line = null ;
         while((line = br.readLine())!=null){ // 阻塞式的！！
            System.out.println("服务端收到了数据："+line);
         }
      } catch (Exception e) {
         System.out.println("有人下线了");
      }

   }
}
```
```markdown
伪异步io采用了线程池实现，因此避免了为每个请求创建一个
独立线程造成线程资源耗尽的问题，但由于底层依然是采用
的同步阻塞模型，因此无法从根本上解决问题。
如果单个消息处理的缓慢，或者服务器线程池中的全部线程都
被阻塞，那么后续socket的i/o消息都将在队列中排队。新的
Socket请求将被拒绝，客户端会发生大量连接超时。
```
### 3.5 基于BIO形式下的文件上传
```markdown
支持任意类型文件形式的上传。
```
```markdown
客户端开发
```


```java
import java.io.DataOutputStream;
import java.io.FileInputStream;
import java.io.InputStream;
import java.net.Socket;

/**
    目标：实现客户端上传任意类型的文件数据给服务端保存起来。

 */
public class Client {
    public static void main(String[] args) {
        try(
                InputStream is = new FileInputStream("C:\\Users\\dlei\\Desktop\\BIO,NIO,AIO\\文件\\java.png");
        ){
            //  1、请求与服务端的Socket链接
            Socket socket = new Socket("127.0.0.1" , 8888);
            //  2、把字节输出流包装成一个数据输出流
            DataOutputStream dos = new DataOutputStream(socket.getOutputStream());
            //  3、先发送上传文件的后缀给服务端
            dos.writeUTF(".png");
            //  4、把文件数据发送给服务端进行接收
            byte[] buffer = new byte[1024];
            int len;
            while((len = is.read(buffer)) > 0 ){
                dos.write(buffer , 0 , len);
            }
            dos.flush();
            Thread.sleep(10000);
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```
```markdown
服务端开发
```
```java
import java.net.ServerSocket;
import java.net.Socket;

/**
    目标：服务端开发，可以实现接收客户端的任意类型文件，并保存到服务端磁盘。
 */
public class Server {
    public static void main(String[] args) {
        try{
            ServerSocket ss = new ServerSocket(8888);
            while (true){
                Socket socket = ss.accept();
                // 交给一个独立的线程来处理与这个客户端的文件通信需求。
                new ServerReaderThread(socket).start();
            }
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

```java
import java.io.DataInputStream;
import java.io.FileOutputStream;
import java.io.OutputStream;
import java.net.Socket;
import java.util.UUID;

public class ServerReaderThread extends Thread {
    private Socket socket;
    public ServerReaderThread(Socket socket){
        this.socket = socket;
    }
    @Override
    public void run() {
        try{
            // 1、得到一个数据输入流读取客户端发送过来的数据
            DataInputStream dis = new DataInputStream(socket.getInputStream());
            // 2、读取客户端发送过来的文件类型
            String suffix = dis.readUTF();
            System.out.println("服务端已经成功接收到了文件类型：" + suffix);
            // 3、定义一个字节输出管道负责把客户端发来的文件数据写出去
            OutputStream os = new FileOutputStream("C:\\Users\\dlei\\Desktop\\BIO,NIO,AIO\\文件\\server\\"+
                    UUID.randomUUID().toString()+suffix);
            // 4、从数据输入流中读取文件数据，写出到字节输出流中去
            byte[] buffer = new byte[1024];
            int len;
            while((len = dis.read(buffer)) > 0){
                os.write(buffer,0, len);
            }
            os.close();
            System.out.println("服务端接收文件保存成功！");

        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```
```markdown
客户端怎么发，服务端就怎么接收
```

### 3.6 Java BIO模式下的端口转发思想
```markdown
需求：需要实现一个客户端的消息可以发送给所有的客户
端去接收。（群聊实现）
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210323142048139.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
客户端开发
```
```java
import java.io.DataOutputStream;
import java.io.FileInputStream;
import java.io.InputStream;
import java.net.Socket;

/**
    目标：实现客户端上传任意类型的文件数据给服务端保存起来。

 */
public class Client {
    public static void main(String[] args) {
        try(
                InputStream is = new FileInputStream("C:\\Users\\dlei\\Desktop\\BIO,NIO,AIO\\文件\\java.png");
        ){
            //  1、请求与服务端的Socket链接
            Socket socket = new Socket("127.0.0.1" , 8888);
            //  2、把字节输出流包装成一个数据输出流
            DataOutputStream dos = new DataOutputStream(socket.getOutputStream());
            //  3、先发送上传文件的后缀给服务端
            dos.writeUTF(".png");
            //  4、把文件数据发送给服务端进行接收
            byte[] buffer = new byte[1024];
            int len;
            while((len = is.read(buffer)) > 0 ){
                dos.write(buffer , 0 , len);
            }
            dos.flush();
            Thread.sleep(10000);
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```
```markdown
服务端实现
```

### 3.7 基于BIO模式下即时通信
```markdown
基于BIO模式下的即时通信，我们需要解决客户端到客户端的通
信，也就是需要实现客户端与客户端的端口消息转发逻辑。
```

```markdown
项目功能演示
项目案例说明

本项目案例为即时通信的软件项目，适合基础加强的大
案例，具备综合性。学习本项目案例至少需要具
备如下Java SE技术点:

1 Java 面向对象设计，语法设计。
2 多线程技术。
3 IO流技术。
4 网络通信相关技术。
5 集合框架。
6 项目开发思维。
7 Java 常用 api 使用。
......

功能清单简单说明：

1 客户端登陆功能

可以启动客户端进行登录，客户端登陆只需要输入用户名
和服务端ip地址即可。

2 在线人数实时更新。
客户端用户户登陆以后，需要同步更新所有客户端的联
系人信息栏。

3 离线人数更新
检测到有客户端下线后，需要同步更新所有客户端
的联系人信息栏。

4 群聊
任意一个客户端的消息，可以推送给当前所有客户端接收。

5 私聊
可以选择某个员工，点击私聊按钮，然后发出的消息可以
被该客户端单独接收。

6 @消息
可以选择某个员工，然后发出的消息可以@该用户，但是
其他所有人都能

7 消息用户和消息时间点
服务端可以实时记录该用户的消息时间点，然后进行消息
的多路转发或者选择。
```
```markdown
项目启动与演示

项目代码结构演示。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210323142725547.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
1 首先需要启动服务端，点击ServerChat类直接右键启动，
显示服务端启动成功！

2 其次，点击客户端类ClientChat类，在弹出的方框中输入服
务端的ip和当前客户端的昵称
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210323142903690.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
3 登陆进入后的聊天界面如下，即可进行相关操作。
如果直接点击发送，默认发送群聊消息
如果选中右侧在线列表某个用户，默认发送@消息
如果选中右侧在线列表某个用户，然后选择右下侧私聊
按钮默，认发送私聊消息。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210323143047616.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210323143106874.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
技术选型分析

本项目案例涉及到Java基础加强的案例，具体涉及到的技术点如下：

1 Java 面向对象设计，语法设计。

2 多线程技术。

3 IO流技术。

4 网络通信相关技术。

5 集合框架。

6 项目开发思维。

7 Java 常用 api 使用。
......
```

```markdown
服务端设计
服务端接收多个客户端逻辑

服务端需要接收多个客户端的接入。

1 服务端需要接收多个客户端，目前我们采取的策略是一个
客户端对应一个服务端线程。
2 服务端除了要注册端口以外，还需要为每个客户端分配
一个独立线程处理与之通信。
```
```markdown
服务端主体代码，主要进行端口注册，和接收客户端，分配线
程处理该客户端请求

代码实现
```
```java
public class ServerChat {
    
    /** 定义一个集合存放所有在线的socket  */
	public static Map<Socket, String> onLineSockets = new HashMap<>();

   public static void main(String[] args) {
      try {
         /** 1.注册端口   */
         ServerSocket serverSocket = new ServerSocket(Constants.PORT);

         /** 2.循环一直等待所有可能的客户端连接 */
         while(true){
            Socket socket = serverSocket.accept();
            /**3. 把客户端的socket管道单独配置一个线程来处理 */
            new ServerReader(socket).start();
         }
      } catch (Exception e) {
         e.printStackTrace();
      }
   }
}
```
```markdown
服务端分配的独立线程类负责处理该客户端Socket的管道请求。
```

```java
class ServerReader extends Thread {
   private Socket socket;
   public ServerReader(Socket socket) {
      this.socket = socket;
   }
   @Override
   public void run() {
      try {
       
      } catch (Exception e) {
            e.printStackTrace();
      }
   }
}
```
```markdown
常量包负责做端口配置
```
```java
public class Constants {
   /** 常量 */
   public static final int PORT = 7778 ;

}
```
```markdown
本节实现了服务端可以接收多个客户端请求。
```
```markdown
服务端接收登陆消息以及监测离线
在上节我们实现了服务端可以接收多个客户端，然后
服务端可以接收多个客户端连接，接下来我们要接收
客户端的登陆消息。
实现步骤

需要在服务端处理客户端的线程的登陆消息。
需要注意的是，服务端需要接收客户端的消息可能有很多种。
  分别是登陆消息，群聊消息，私聊消息 和@消息。
  这里需要约定如果客户端发送消息之前需要先发送消息的类型，类
  型我们使用信号值标志（1，2，3）。
    1代表接收的是登陆消息
    2代表群发| @消息
    3代表了私聊消息
服务端的线程中有异常校验机制，一旦发现客户端下线会在异
常机制中处理，然后移除当前客户端用户，把最新的用户列表
发回给全部客户端进行在线人数更新。
```
```java
public class ServerReader extends Thread {
	private Socket socket;
	public ServerReader(Socket socket) {
		this.socket = socket;
	}

	@Override
	public void run() {
		DataInputStream dis = null;
		try {
			dis = new DataInputStream(socket.getInputStream());
			/** 1.循环一直等待客户端的消息 */
			while(true){
				/** 2.读取当前的消息类型 ：登录,群发,私聊 , @消息 */
				int flag = dis.readInt();
				if(flag == 1){
					/** 先将当前登录的客户端socket存到在线人数的socket集合中   */
					String name = dis.readUTF() ;
					System.out.println(name+"---->"+socket.getRemoteSocketAddress());
					ServerChat.onLineSockets.put(socket, name);
				}
				writeMsg(flag,dis);
			}
		} catch (Exception e) {
			System.out.println("--有人下线了--");
			// 从在线人数中将当前socket移出去  
			ServerChat.onLineSockets.remove(socket);
			try {
				// 从新更新在线人数并发给所有客户端 
				writeMsg(1,dis);
			} catch (Exception e1) {
				e1.printStackTrace();
			}
		}

	}

	private void writeMsg(int flag, DataInputStream dis) throws Exception {
        // DataOutputStream dos = new DataOutputStream(socket.getOutputStream()); 
		// 定义一个变量存放最终的消息形式 
		String msg = null ;
		if(flag == 1){
			/** 读取所有在线人数发给所有客户端去更新自己的在线人数列表 */
			/** onlineNames = [波仔,zhangsan,波妞]*/
			StringBuilder rs = new StringBuilder();
			Collection<String> onlineNames = ServerChat.onLineSockets.values();
			// 判断是否存在在线人数 
			if(onlineNames != null && onlineNames.size() > 0){
				for(String name : onlineNames){
					rs.append(name+ Constants.SPILIT);
				}
				// 波仔003197♣♣㏘♣④④♣zhangsan003197♣♣㏘♣④④♣波妞003197♣♣㏘♣④④♣
				// 去掉最后的一个分隔符 
				msg = rs.substring(0, rs.lastIndexOf(Constants.SPILIT));

				/** 将消息发送给所有的客户端 */
				sendMsgToAll(flag,msg);
			}
		}else if(flag == 2 || flag == 3){
			
			}
		}
	}
	
	private void sendMsgToAll(int flag, String msg) throws Exception {
		// 拿到所有的在线socket管道 给这些管道写出消息
		Set<Socket> allOnLineSockets = ServerChat.onLineSockets.keySet();
		for(Socket sk :  allOnLineSockets){
			DataOutputStream dos = new DataOutputStream(sk.getOutputStream());
			dos.writeInt(flag); // 消息类型
			dos.writeUTF(msg);
			dos.flush();
		}
	}
}
```
```markdown
此处实现了接收客户端的登陆消息，然后提取当前在线
的全部的用户名称和当前登陆的用户名称发送给全部在
线用户更新自己的在线人数列表。
```
```markdown
服务端接收群聊消息
在上节实现了接收客户端的登陆消息，然后提取当前在线的全
部的用户名称和当前登陆的用户名称发送给全部在线用户
更新自己的在线人数列表。接下来要接收客户端发来的群
聊消息推送给当前在线的所有客户端

实现步骤
接下来要接收客户端发来的群聊消息。
需要注意的是，服务端需要接收客户端的消息可能有很多种。
  分别是登陆消息，群聊消息，私聊消息 和@消息。
  这里需要约定如果客户端发送消息之前需要先发送消息的
  类型，类型我们使用信号值标志（1，2，3）。
    1代表接收的是登陆消息
    2代表群发| @消息
    3代表了私聊消息
```
```java
public class ServerReader extends Thread {
	private Socket socket;
	public ServerReader(Socket socket) {
		this.socket = socket;
	}

	@Override
	public void run() {
		DataInputStream dis = null;
		try {
			dis = new DataInputStream(socket.getInputStream());
			/** 1.循环一直等待客户端的消息 */
			while(true){
				/** 2.读取当前的消息类型 ：登录,群发,私聊 , @消息 */
				int flag = dis.readInt();
				if(flag == 1){
					/** 先将当前登录的客户端socket存到在线人数的socket集合中   */
					String name = dis.readUTF() ;
					System.out.println(name+"---->"+socket.getRemoteSocketAddress());
					ServerChat.onLineSockets.put(socket, name);
				}
				writeMsg(flag,dis);
			}
		} catch (Exception e) {
			System.out.println("--有人下线了--");
			// 从在线人数中将当前socket移出去  
			ServerChat.onLineSockets.remove(socket);
			try {
				// 从新更新在线人数并发给所有客户端 
				writeMsg(1,dis);
			} catch (Exception e1) {
				e1.printStackTrace();
			}
		}

	}

	private void writeMsg(int flag, DataInputStream dis) throws Exception {
        // DataOutputStream dos = new DataOutputStream(socket.getOutputStream()); 
		// 定义一个变量存放最终的消息形式 
		String msg = null ;
		if(flag == 1){
			/** 读取所有在线人数发给所有客户端去更新自己的在线人数列表 */
			/** onlineNames = [波仔,zhangsan,波妞]*/
			StringBuilder rs = new StringBuilder();
			Collection<String> onlineNames = ServerChat.onLineSockets.values();
			// 判断是否存在在线人数 
			if(onlineNames != null && onlineNames.size() > 0){
				for(String name : onlineNames){
					rs.append(name+ Constants.SPILIT);
				}
				// 波仔003197♣♣㏘♣④④♣zhangsan003197♣♣㏘♣④④♣波妞003197♣♣㏘♣④④♣
				// 去掉最后的一个分隔符 
				msg = rs.substring(0, rs.lastIndexOf(Constants.SPILIT));

				/** 将消息发送给所有的客户端 */
				sendMsgToAll(flag,msg);
			}
		}else if(flag == 2 || flag == 3){
			// 读到消息  群发的 或者 @消息
			String newMsg = dis.readUTF() ; // 消息
			// 得到发件人 
			String sendName = ServerChat.onLineSockets.get(socket);
	
			// 内容
			StringBuilder msgFinal = new StringBuilder();
			// 时间  
			SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss EEE");
			if(flag == 2){
				msgFinal.append(sendName).append("  ").append(sdf.format(System.currentTimeMillis())).append("\r\n");
				msgFinal.append("    ").append(newMsg).append("\r\n");
				sendMsgToAll(flag,msgFinal.toString());
			}else if(flag == 3){
	
			}
		}
	}
	

	private void sendMsgToAll(int flag, String msg) throws Exception {
		// 拿到所有的在线socket管道 给这些管道写出消息
		Set<Socket> allOnLineSockets = ServerChat.onLineSockets.keySet();
		for(Socket sk :  allOnLineSockets){
			DataOutputStream dos = new DataOutputStream(sk.getOutputStream());
			dos.writeInt(flag); // 消息类型
			dos.writeUTF(msg);
			dos.flush();
		}
	}
}
```
```markdown
此处根据消息的类型判断为群聊消息，然后把群聊消
息推送给当前在线的所有客户端。
```
```markdown
服务端接收私聊消息

在上节我们接收了客户端发来的群聊消息推送给当前在
线的所有客户端，接下来要解决私聊消息的推送逻辑
```
```markdown
实现步骤
解决私聊消息的推送逻辑，私聊消息需要知道推送给某个具
体的客户端
我们可以接收到客户端发来的私聊用户名称，根据用户名称定
位该用户的Socket管道，然后单独推送消息给该Socket管道。
需要注意的是，服务端需要接收客户端的消息可能有很多种。
  分别是登陆消息，群聊消息，私聊消息 和@消息。
  这里需要约定如果客户端发送消息之前需要先发送消息
  的类型，类型我们使用信号值标志（1，2，3）。
    1代表接收的是登陆消息
    2代表群发| @消息
    3代表了私聊消息
```

```java
public class ServerReader extends Thread {
	private Socket socket;
	public ServerReader(Socket socket) {
		this.socket = socket;
	}

	@Override
	public void run() {
		DataInputStream dis = null;
		try {
			dis = new DataInputStream(socket.getInputStream());
			/** 1.循环一直等待客户端的消息 */
			while(true){
				/** 2.读取当前的消息类型 ：登录,群发,私聊 , @消息 */
				int flag = dis.readInt();
				if(flag == 1){
					/** 先将当前登录的客户端socket存到在线人数的socket集合中   */
					String name = dis.readUTF() ;
					System.out.println(name+"---->"+socket.getRemoteSocketAddress());
					ServerChat.onLineSockets.put(socket, name);
				}
				writeMsg(flag,dis);
			}
		} catch (Exception e) {
			System.out.println("--有人下线了--");
			// 从在线人数中将当前socket移出去  
			ServerChat.onLineSockets.remove(socket);
			try {
				// 从新更新在线人数并发给所有客户端 
				writeMsg(1,dis);
			} catch (Exception e1) {
				e1.printStackTrace();
			}
		}

	}

	private void writeMsg(int flag, DataInputStream dis) throws Exception {
        // DataOutputStream dos = new DataOutputStream(socket.getOutputStream()); 
		// 定义一个变量存放最终的消息形式 
		String msg = null ;
		if(flag == 1){
			/** 读取所有在线人数发给所有客户端去更新自己的在线人数列表 */
			/** onlineNames = [波仔,zhangsan,波妞]*/
			StringBuilder rs = new StringBuilder();
			Collection<String> onlineNames = ServerChat.onLineSockets.values();
			// 判断是否存在在线人数 
			if(onlineNames != null && onlineNames.size() > 0){
				for(String name : onlineNames){
					rs.append(name+ Constants.SPILIT);
				}
				// 波仔003197♣♣㏘♣④④♣zhangsan003197♣♣㏘♣④④♣波妞003197♣♣㏘♣④④♣
				// 去掉最后的一个分隔符 
				msg = rs.substring(0, rs.lastIndexOf(Constants.SPILIT));

				/** 将消息发送给所有的客户端 */
				sendMsgToAll(flag,msg);
			}
		}else if(flag == 2 || flag == 3){
			// 读到消息  群发的 或者 @消息
			String newMsg = dis.readUTF() ; // 消息
			// 得到发件人 
			String sendName = ServerChat.onLineSockets.get(socket);
	
			// 内容
			StringBuilder msgFinal = new StringBuilder();
			// 时间  
			SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss EEE");
			if(flag == 2){
				msgFinal.append(sendName).append("  ").append(sdf.format(System.currentTimeMillis())).append("\r\n");
				msgFinal.append("    ").append(newMsg).append("\r\n");
				sendMsgToAll(flag,msgFinal.toString());
			}else if(flag == 3){
			msgFinal.append(sendName).append("  ").append(sdf.format(System.currentTimeMillis())).append("对您私发\r\n");
				msgFinal.append("    ").append(newMsg).append("\r\n");
				// 私发 
				// 得到给谁私发 
				String destName = dis.readUTF();
				sendMsgToOne(destName,msgFinal.toString());
			}
		}
	}
	/**
	 * @param destName 对谁私发 
	 * @param msg 发的消息内容 
	 * @throws Exception
	 */
	private void sendMsgToOne(String destName, String msg) throws Exception {
		// 拿到所有的在线socket管道 给这些管道写出消息
		Set<Socket> allOnLineSockets = ServerChat.onLineSockets.keySet();
		for(Socket sk :  allOnLineSockets){
			// 得到当前需要私发的socket 
			// 只对这个名字对应的socket私发消息
			if(ServerChat.onLineSockets.get(sk).trim().equals(destName)){
				DataOutputStream dos = new DataOutputStream(sk.getOutputStream());
				dos.writeInt(2); // 消息类型
				dos.writeUTF(msg);
				dos.flush();
			}
		}

	}
	

	private void sendMsgToAll(int flag, String msg) throws Exception {
		// 拿到所有的在线socket管道 给这些管道写出消息
		Set<Socket> allOnLineSockets = ServerChat.onLineSockets.keySet();
		for(Socket sk :  allOnLineSockets){
			DataOutputStream dos = new DataOutputStream(sk.getOutputStream());
			dos.writeInt(flag); // 消息类型
			dos.writeUTF(msg);
			dos.flush();
		}
	}
}
```
```markdown
本节我们解决了私聊消息的推送逻辑，私聊消息需要知道推
送给某个具体的客户端Socket管道
我们可以接收到客户端发来的私聊用户名称，根据用户名
称定位该用户的Socket管道，然后单独推送消息给该Socket管道。
```
```markdown
客户端设计
启动客户端界面 ,登陆，刷新在线

启动客户端界面，登陆，刷新在线人数列表

实现步骤
客户端界面主要是GUI设计，主体页面分为登陆界面和聊
天窗口，以及在线用户列表。
GUI界面读者可以自行复制使用。
登陆输入服务端ip和用户名后，要请求与服务端的登
陆，然后立即为当前客户端分配一个读线程处理客户端
的读数据消息。因为客户端可能随时会接收到服务端那
边转发过来的各种即时消息信息。
客户端登陆完成，服务端收到登陆的用户名后，会立即发
来最新的用户列表给客户端更新。
```
```markdown
客户端主体代码：
```

```java
public class ClientChat implements ActionListener {
   /** 1.设计界面  */
   private JFrame win = new JFrame();
   /** 2.消息内容框架 */
   public JTextArea smsContent =new JTextArea(23 , 50);
   /** 3.发送消息的框  */
   private JTextArea smsSend = new JTextArea(4,40);
   /** 4.在线人数的区域  */
   /** 存放人的数据 */
   /** 展示在线人数的窗口 */
   public JList<String> onLineUsers = new JList<>();

   // 是否私聊按钮
   private JCheckBox isPrivateBn = new JCheckBox("私聊");
   // 消息按钮
   private JButton sendBn  = new JButton("发送");

   // 登录界面
   private JFrame loginView;

   private JTextField ipEt , nameEt , idEt;

   private Socket socket ;

   public static void main(String[] args) {
      new ClientChat().initView();

   }

   private void initView() {
      /** 初始化聊天窗口的界面 */
      win.setSize(650, 600);

      /** 展示登录界面  */
      displayLoginView();

      /** 展示聊天界面 */
      //displayChatView();

   }

   private void displayChatView() {

      JPanel bottomPanel = new JPanel(new BorderLayout());
      //-----------------------------------------------
      // 将消息框和按钮 添加到窗口的底端
      win.add(bottomPanel, BorderLayout.SOUTH);
      bottomPanel.add(smsSend);
      JPanel btns = new JPanel(new FlowLayout(FlowLayout.LEFT));
      btns.add(sendBn);
      btns.add(isPrivateBn);
      bottomPanel.add(btns, BorderLayout.EAST);
      //-----------------------------------------------
      // 给发送消息按钮绑定点击事件监听器
      // 将展示消息区centerPanel添加到窗口的中间
      smsContent.setBackground(new Color(0xdd,0xdd,0xdd));
      // 让展示消息区可以滚动。
      win.add(new JScrollPane(smsContent), BorderLayout.CENTER);
      smsContent.setEditable(false);
      //-----------------------------------------------
      // 用户列表和是否私聊放到窗口的最右边
      Box rightBox = new Box(BoxLayout.Y_AXIS);
      onLineUsers.setFixedCellWidth(120);
      onLineUsers.setVisibleRowCount(13);
      rightBox.add(new JScrollPane(onLineUsers));
      win.add(rightBox, BorderLayout.EAST);
      //-----------------------------------------------
      // 关闭窗口退出当前程序
      win.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
      win.pack();  // swing 加上这句 就可以拥有关闭窗口的功能
      /** 设置窗口居中,显示出来  */
      setWindowCenter(win,650,600,true);
      // 发送按钮绑定点击事件
      sendBn.addActionListener(this);
   }

   private void displayLoginView(){

      /** 先让用户进行登录
       *  服务端ip
       *  用户名
       *  id
       *  */
      /** 显示一个qq的登录框     */
      loginView = new JFrame("登录");
      loginView.setLayout(new GridLayout(3, 1));
      loginView.setSize(400, 230);

      JPanel ip = new JPanel();
      JLabel label = new JLabel("   IP:");
      ip.add(label);
      ipEt = new JTextField(20);
      ip.add(ipEt);
      loginView.add(ip);

      JPanel name = new JPanel();
      JLabel label1 = new JLabel("姓名:");
      name.add(label1);
      nameEt = new JTextField(20);
      name.add(nameEt);
      loginView.add(name);

      JPanel btnView = new JPanel();
      JButton login = new JButton("登陆");
      btnView.add(login);
      JButton cancle = new JButton("取消");
      btnView.add(cancle);
      loginView.add(btnView);
      // 关闭窗口退出当前程序
      loginView.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
      setWindowCenter(loginView,400,260,true);

      /** 给登录和取消绑定点击事件 */
      login.addActionListener(this);
      cancle.addActionListener(this);

   }

   private static void setWindowCenter(JFrame frame, int width , int height, boolean flag) {
      /** 得到所在系统所在屏幕的宽高 */
      Dimension ds = frame.getToolkit().getScreenSize();

      /** 拿到电脑的宽 */
      int width1 = ds.width;
      /** 高 */
      int height1 = ds.height ;

      System.out.println(width1 +"*" + height1);
      /** 设置窗口的左上角坐标 */
      frame.setLocation(width1/2 - width/2, height1/2 -height/2);
      frame.setVisible(flag);
   }

   @Override
   public void actionPerformed(ActionEvent e) {
      /** 得到点击的事件源 */
      JButton btn = (JButton) e.getSource();
      switch(btn.getText()){
         case "登陆":
            String ip = ipEt.getText().toString();
            String name = nameEt.getText().toString();
            // 校验参数是否为空
            // 错误提示
            String msg = "" ;
            // 12.1.2.0
            // \d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\
            if(ip==null || !ip.matches("\\d{1,3}\\.\\d{1,3}\\.\\d{1,3}\\.\\d{1,3}")){
               msg = "请输入合法的服务端ip地址";
            }else if(name==null || !name.matches("\\S{1,}")){
               msg = "姓名必须1个字符以上";
            }

            if(!msg.equals("")){
               /** msg有内容说明参数有为空 */
               // 参数一：弹出放到哪个窗口里面
               JOptionPane.showMessageDialog(loginView, msg);
            }else{
               try {
                  // 参数都合法了
                  // 当前登录的用户,去服务端登陆
                  /** 先把当前用户的名称展示到界面 */
                  win.setTitle(name);
                  // 去服务端登陆连接一个socket管道
                  socket = new Socket(ip, Constants.PORT);

                  //为客户端的socket分配一个线程 专门负责收消息
                  new ClientReader(this,socket).start();

                  // 带上用户信息过去
                  DataOutputStream dos = new DataOutputStream(socket.getOutputStream());
                  dos.writeInt(1); // 登录消息
                  dos.writeUTF(name.trim());
                  dos.flush();

                  // 关系当前窗口 弹出聊天界面
                  loginView.dispose(); // 登录窗口销毁
                  displayChatView(); // 展示了聊天窗口了


               } catch (Exception e1) {
                  e1.printStackTrace();
               }
            }
            break;
         case "取消":
            /** 退出系统 */
            System.exit(0);
            break;
         case "发送":
            
            break;

      }

   }
}
```
```markdown
客户端socket处理线程：
```
```java
public class ClientReader extends Thread {

   private Socket socket;
    // 接收客户端界面，方便收到消息后，更新界面数据。
   private ClientChat clientChat ;

   public ClientReader(ClientChat clientChat, Socket socket) {
      this.clientChat = clientChat;
      this.socket = socket;
   }

   @Override
   public void run() {
      try {
         DataInputStream dis = new DataInputStream(socket.getInputStream());
         /** 循环一直等待客户端的消息 */
         while(true){
            /** 读取当前的消息类型 ：登录,群发,私聊 , @消息 */
            int flag = dis.readInt();
            if(flag == 1){
               // 在线人数消息回来了
               String nameDatas = dis.readUTF();
               // 展示到在线人数的界面
               String[] names = nameDatas.split(Constants.SPILIT);

               clientChat.onLineUsers.setListData(names);
            }else if(flag == 2){
              
            }
         }
      } catch (Exception e) {
         e.printStackTrace();
      }
   }
}
```
```markdown
此处说明了如果启动客户端界面，以及登陆功能后，服
务端收到新的登陆消息后，会响应一个在线列表用户
回来给客户端更新在线人数！
```
```markdown
客户端发送消息逻辑

目标
客户端发送群聊消息，@消息，以及私聊消息。

实现步骤
客户端启动后，在聊天界面需要通过发送按钮推送群
聊消息，@消息，以及私聊消息。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210323144459527.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
如果直接点击发送，默认发送群聊消息
如果选中右侧在线列表某个用户，默认发送@消息
如果选中右侧在线列表某个用户，然后选择右下侧私
聊按钮默，认发送私聊消息。
```
```markdown
客户端主体代码：
```
```java
public class ClientChat implements ActionListener {
	/** 1.设计界面  */
	private JFrame win = new JFrame();
	/** 2.消息内容框架 */
	public JTextArea smsContent =new JTextArea(23 , 50);
	/** 3.发送消息的框  */
	private JTextArea smsSend = new JTextArea(4,40);
	/** 4.在线人数的区域  */
	/** 存放人的数据 */
	/** 展示在线人数的窗口 */
	public JList<String> onLineUsers = new JList<>();

	// 是否私聊按钮
	private JCheckBox isPrivateBn = new JCheckBox("私聊");
	// 消息按钮
	private JButton sendBn  = new JButton("发送");

	// 登录界面
	private JFrame loginView;

	private JTextField ipEt , nameEt , idEt;

	private Socket socket ;

	public static void main(String[] args) {
		new ClientChat().initView();

	}

	private void initView() {
		/** 初始化聊天窗口的界面 */
		win.setSize(650, 600);

		/** 展示登录界面  */
		displayLoginView();

		/** 展示聊天界面 */
		//displayChatView();

	}

	private void displayChatView() {

		JPanel bottomPanel = new JPanel(new BorderLayout());
		//-----------------------------------------------
		// 将消息框和按钮 添加到窗口的底端
		win.add(bottomPanel, BorderLayout.SOUTH);
		bottomPanel.add(smsSend);
		JPanel btns = new JPanel(new FlowLayout(FlowLayout.LEFT));
		btns.add(sendBn);
		btns.add(isPrivateBn);
		bottomPanel.add(btns, BorderLayout.EAST);
		//-----------------------------------------------
		// 给发送消息按钮绑定点击事件监听器
		// 将展示消息区centerPanel添加到窗口的中间
		smsContent.setBackground(new Color(0xdd,0xdd,0xdd));
		// 让展示消息区可以滚动。
		win.add(new JScrollPane(smsContent), BorderLayout.CENTER);
		smsContent.setEditable(false);
		//-----------------------------------------------
		// 用户列表和是否私聊放到窗口的最右边
		Box rightBox = new Box(BoxLayout.Y_AXIS);
		onLineUsers.setFixedCellWidth(120);
		onLineUsers.setVisibleRowCount(13);
		rightBox.add(new JScrollPane(onLineUsers));
		win.add(rightBox, BorderLayout.EAST);
		//-----------------------------------------------
		// 关闭窗口退出当前程序
		win.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		win.pack();  // swing 加上这句 就可以拥有关闭窗口的功能
		/** 设置窗口居中,显示出来  */
		setWindowCenter(win,650,600,true);
		// 发送按钮绑定点击事件
		sendBn.addActionListener(this);
	}

	private void displayLoginView(){

		/** 先让用户进行登录
		 *  服务端ip
		 *  用户名
		 *  id
		 *  */
		/** 显示一个qq的登录框     */
		loginView = new JFrame("登录");
		loginView.setLayout(new GridLayout(3, 1));
		loginView.setSize(400, 230);

		JPanel ip = new JPanel();
		JLabel label = new JLabel("   IP:");
		ip.add(label);
		ipEt = new JTextField(20);
		ip.add(ipEt);
		loginView.add(ip);

		JPanel name = new JPanel();
		JLabel label1 = new JLabel("姓名:");
		name.add(label1);
		nameEt = new JTextField(20);
		name.add(nameEt);
		loginView.add(name);

		JPanel btnView = new JPanel();
		JButton login = new JButton("登陆");
		btnView.add(login);
		JButton cancle = new JButton("取消");
		btnView.add(cancle);
		loginView.add(btnView);
		// 关闭窗口退出当前程序
		loginView.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setWindowCenter(loginView,400,260,true);

		/** 给登录和取消绑定点击事件 */
		login.addActionListener(this);
		cancle.addActionListener(this);

	}

	private static void setWindowCenter(JFrame frame, int width , int height, boolean flag) {
		/** 得到所在系统所在屏幕的宽高 */
		Dimension ds = frame.getToolkit().getScreenSize();

		/** 拿到电脑的宽 */
		int width1 = ds.width;
		/** 高 */
		int height1 = ds.height ;

		System.out.println(width1 +"*" + height1);
		/** 设置窗口的左上角坐标 */
		frame.setLocation(width1/2 - width/2, height1/2 -height/2);
		frame.setVisible(flag);
	}

	@Override
	public void actionPerformed(ActionEvent e) {
		/** 得到点击的事件源 */
		JButton btn = (JButton) e.getSource();
		switch(btn.getText()){
			case "登陆":
				String ip = ipEt.getText().toString();
				String name = nameEt.getText().toString();
				// 校验参数是否为空
				// 错误提示
				String msg = "" ;
				// 12.1.2.0
				// \d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\
				if(ip==null || !ip.matches("\\d{1,3}\\.\\d{1,3}\\.\\d{1,3}\\.\\d{1,3}")){
					msg = "请输入合法的服务端ip地址";
				}else if(name==null || !name.matches("\\S{1,}")){
					msg = "姓名必须1个字符以上";
				}

				if(!msg.equals("")){
					/** msg有内容说明参数有为空 */
					// 参数一：弹出放到哪个窗口里面
					JOptionPane.showMessageDialog(loginView, msg);
				}else{
					try {
						// 参数都合法了
						// 当前登录的用户,去服务端登陆
						/** 先把当前用户的名称展示到界面 */
						win.setTitle(name);
						// 去服务端登陆连接一个socket管道
						socket = new Socket(ip, Constants.PORT);

						//为客户端的socket分配一个线程 专门负责收消息
						new ClientReader(this,socket).start();

						// 带上用户信息过去
						DataOutputStream dos = new DataOutputStream(socket.getOutputStream());
						dos.writeInt(1); // 登录消息
						dos.writeUTF(name.trim());
						dos.flush();

						// 关系当前窗口 弹出聊天界面
						loginView.dispose(); // 登录窗口销毁
						displayChatView(); // 展示了聊天窗口了


					} catch (Exception e1) {
						e1.printStackTrace();
					}
				}
				break;
			case "取消":
				/** 退出系统 */
				System.exit(0);
				break;
			case "发送":
				// 得到发送消息的内容
				String msgSend = smsSend.getText().toString();
				if(!msgSend.trim().equals("")){
					/** 发消息给服务端 */
					try {
						// 判断是否对谁发消息
						String selectName = onLineUsers.getSelectedValue();
						int flag = 2 ;// 群发 @消息
						if(selectName!=null&&!selectName.equals("")){
							msgSend =("@"+selectName+","+msgSend);
							/** 判断是否选中了私法 */
							if(isPrivateBn.isSelected()){
								/** 私法 */
								flag = 3 ;//私发消息
							}

						}

						DataOutputStream dos = new DataOutputStream(socket.getOutputStream());
						dos.writeInt(flag); // 群发消息  发送给所有人
						dos.writeUTF(msgSend);
						if(flag == 3){
							// 告诉服务端我对谁私发
							dos.writeUTF(selectName.trim());
						}
						dos.flush();

					} catch (Exception e1) {
						e1.printStackTrace();
					}

				}
				smsSend.setText(null);
				break;

		}

	}
}
```
```markdown
客户端socket处理线程：
```
```java
class ClientReader extends Thread {

	private Socket socket;
	private ClientChat clientChat ;

	public ClientReader(ClientChat clientChat, Socket socket) {
		this.clientChat = clientChat;
		this.socket = socket;
	}

	@Override
	public void run() {
		try {
			DataInputStream dis = new DataInputStream(socket.getInputStream());
			/** 循环一直等待客户端的消息 */
			while(true){
				/** 读取当前的消息类型 ：登录,群发,私聊 , @消息 */
				int flag = dis.readInt();
				if(flag == 1){
					// 在线人数消息回来了
					String nameDatas = dis.readUTF();
					// 展示到在线人数的界面
					String[] names = nameDatas.split(Constants.SPILIT);

					clientChat.onLineUsers.setListData(names);
				}else if(flag == 2){
					//群发,私聊 , @消息 都是直接显示的。
					String msg = dis.readUTF() ;
					clientChat.smsContent.append(msg);
					// 让消息界面滾動到底端
					clientChat.smsContent.setCaretPosition(clientChat.smsContent.getText().length());
				}
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```
```markdown
此处实现了客户端发送群聊消息，@消息，以及私聊消息。
如果直接点击发送，默认发送群聊消息
如果选中右侧在线列表某个用户，默认发送@消息
如果选中右侧在线列表某个用户，然后选择右下侧私聊
按钮默，认发送私聊消息。
```

## 4 JAVA NIO深入剖析
```markdown
在讲解利用NIO实现通信架构之前，我们需要先来了解一
下NIO的基本特点和使用。
```
### 4.1 Java NIO 基本介绍
```markdown
Java NIO（New IO）也有人称之为 java non-blocking IO是
从Java 1.4版本开始引入的一个新的IO API，可以替代标准
的Java IO API。NIO与原来的IO有同样的作用和目的，但
是使用的方式完全不同，NIO支持面向缓冲区的、基于
通道的IO操作。NIO将以更加高效的方式进行文件的读
写操作。NIO可以理解为非阻塞IO,传统的IO的read和write只
能阻塞执行，线程在读写IO期间不能干其他事情，比如
调用socket.read()时，如果服务器一直没有数据传输
过来，线程就一直阻塞，而NIO中可以配置socket为非阻塞模式。

NIO 相关类都被放在 java.nio 包及子包下，并且对
原 java.io 包中的很多类进行改写。

NIO 有三大核心部分：
Channel( 通道) ，Buffer( 缓冲区), Selector( 选择器)

Java NIO 的非阻塞模式，使一个线程从某通道发送
请求或者读取数据，但是它仅能得到目前可用的数据，如
果目前没有数据可用时，就什么都不会获取，而不是保
持线程阻塞，所以直至数据变的可以读取之前，该线程
可以继续做其他的事情。 非阻塞写也是如此，一个线程
请求写入一些数据到某通道，但不需要等待它完全写
入，这个线程同时可以去做别的事情。
通俗理解：NIO 是可以做到用一个线程来处理多个操
作的。假设有 1000 个请求过来,根据实际情况，可以分
配20 或者 80个线程来处理。不像之前的阻塞 IO 那
样，非得分配 1000 个。
```

### 4.2 NIO 和 BIO 的比较
```markdown
BIO 以流的方式处理数据,而 NIO 以块的方式处理数据
,块 I/O 的效率比流 I/O 高很多
BIO 是阻塞的，NIO 则是非阻塞的
BIO 基于字节流和字符流进行操作，而 NIO 基于 Channel(通道)和
Buffer(缓冲区)进行操作，数据总是从通道读取到缓冲区中，或
者从缓冲区写入到通道中。Selector(选择器)用于监听多个通
道的事件（比如：连接请求，数据到达等），因此使用单个
线程就可以监听多个客户端通道
```

| NIO                       | BIO                 |
| ------------------------- | ------------------- |
| 面向缓冲区（Buffer）      | 面向流（Stream）    |
| 非阻塞（Non Blocking IO） | 阻塞IO(Blocking IO) |
| 选择器（Selectors）       |                     |

### 4.3 NIO 三大核心原理示意图
```markdown
NIO 有三大核心部分：
Channel( 通道) ，Buffer( 缓冲区), Selector( 选择器)
```
```markdown
Buffer缓冲区
缓冲区本质上是一块可以写入数据，然后可以从中读取
数据的内存。这块内存被包装成NIO Buffer对象，并提供
了一组方法，用来方便的访问该块内存。相比较直接对
数组的操作，Buffer API更加容易操作和管理。

Channel（通道）
Java NIO的通道类似流，但又有些不同：既可以从通道
中读取数据，又可以写数据到通道。但流的（input或output)读写
通常是单向的。 通道可以非阻塞读取和写入通道，通道可
以支持读取或写入缓冲区，也支持异步地读写。

Selector选择器
Selector是 一个Java NIO组件，可以能够检查一个
或多个 NIO 通道，并确定哪些通道已经准备好进行读
取或写入。这样，一个单独的线程可以管理多
个channel，从而管理多个网络连接，提高效率
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210323145130359.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
每个 channel 都会对应一个 Buffer
一个线程对应Selector ， 一个Selector对应多个 
channel(连接)
程序切换到哪个 channel 是由事件决定的
Selector 会根据不同的事件，在各个通道上切换
Buffer 就是一个内存块 ， 底层是一个数组
数据的读取写入是通过 Buffer完成的 , BIO 中要么是输入流，或
者是输出流, 不能双向，但是 NIO 的 Buffer 是可以读也可以写。
Java NIO系统的核心在于：通道(Channel)和缓冲区 (Buffer)。
通道表示打开到 IO 设备(例如：文件、 套接字)的连接。若需
要使用 NIO 系统，需要获取 用于连接 IO 设备的通道以及
用于容纳数据的缓冲 区。然后操作缓冲区，对数据进
行处理。简而言之，Channel 负责传输， Buffer 负责
存取数据
```

### 4.4 NIO核心一：缓冲区(Buffer)
```markdown
缓冲区（Buffer）
一个用于特定基本数据类型的容器。由 java.nio 包定义的，所
有缓冲区 都是 Buffer 抽象类的子类.。Java NIO 中
的 Buffer 主要用于与 NIO 通道进行 交互，数据是从通道
读入缓冲区，从缓冲区写入通道中的
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210323153059284.png)

#### 4.4.1 Buffer 类及其子类
```markdown
Buffer就像一个数组，可以保存多个相同类型的数据。根据
数据类型不同 ，有以下 Buffer 常用子类： 

ByteBuffer 
CharBuffer 
ShortBuffer 
IntBuffer 
LongBuffer 
FloatBuffer 
DoubleBuffer 

上述 Buffer 类他们都采用相似的方法进行管理数据，只
是各自 管理的数据类型不同而已。都是通过如下方法获
取一个 Buffer 对象：
```
```java
static XxxBuffer allocate(int capacity) : 创建
一个容量为capacity 的 XxxBuffer 对象
```
```markdown
缓冲区的基本属性
Buffer 中的重要概念： 

容量 (capacity) ：作为一个内存块，Buffer具有一定的固定大小，
也称为"容量"，缓冲区容量不能为负，并且创建后不能更改。 
限制 (limit)：表示缓冲区中可以操作数据的大小
（limit 后数据不能进行读写）。缓冲区的限制不能
为负，并且不能大于其容量。 写入模式，限制等于
buffer的容量。读取模式下，limit等于写入的数据量。

位置 (position)：下一个要读取或写入的数据的索引。
缓冲区的位置不能为 负，并且不能大于其限制 

标记 (mark)与重置 (reset)：标记是一个索引，
通过 Buffer 中的 mark() 方法 指定 Buffer 中一个
特定的 position，之后可以通过调用 reset() 方法恢
复到这 个 position.
标记、位置、限制、容量遵守以下不变式： 
0 <= mark <= position <= limit <= capacity
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021032315392734.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```java
Buffer常见方法
Buffer clear() 清空缓冲区并返回对缓冲区的引用
Buffer flip() 为 将缓冲区的界限设置为当前位置，
并将当前位置充值为 0
int capacity() 返回 Buffer 的 capacity 大小
boolean hasRemaining() 判断缓冲区中是否还有元素
int limit() 返回 Buffer 的界限(limit) 的位置
Buffer limit(int n) 将设置缓冲区界限为 n,
 并返回一个具有新 limit 的缓冲区对象
Buffer mark() 对缓冲区设置标记
int position() 返回缓冲区的当前位置 position
Buffer position(int n) 将设置缓冲区的当前位置为 n，
并返回修改后的 Buffer 对象
int remaining() 返回 position 和 limit 之间的元素个数
Buffer reset() 将位置 position 转到以前设置的mark 
所在的位置
Buffer rewind() 将位置设为为 0， 取消设置的 mark
```

```java
缓冲区的数据操作
Buffer 所有子类提供了两个用于数据操作的方法：get()put() 
方法取获取 Buffer中的数据
get() ：读取单个字节
get(byte[] dst)：批量读取多个字节到 dst 中
get(int index)：读取指定索引位置的字节(不会移动 position)
    
放到 入数据到 Buffer 中 中
put(byte b)：将给定单个字节写入缓冲区的当前位置
put(byte[] src)：将 src 中的字节写入缓冲区的当前位置
put(int index, byte b)：将指定字节写入缓冲区的索引
位置(不会移动 position)
```
```markdown
使用Buffer读写数据一般遵循以下四个步骤：
1 写入数据到Buffer
2 调用flip()方法，转换为读取模式
3 从Buffer中读取数据
4 调用buffer.clear()方法或者buffer.compact()方
法清除缓冲区
```
```java
public class TestBuffer {
   @Test
   public void test3(){
      //分配直接缓冲区
      ByteBuffer buf = ByteBuffer.allocateDirect(1024);
      System.out.println(buf.isDirect());
   }
   
   @Test
   public void test2(){
      String str = "itheima";
      
      ByteBuffer buf = ByteBuffer.allocate(1024);
      
      buf.put(str.getBytes());
      
      buf.flip();
      
      byte[] dst = new byte[buf.limit()];
      buf.get(dst, 0, 2);
      System.out.println(new String(dst, 0, 2));
      System.out.println(buf.position());
      
      //mark() : 标记
      buf.mark();
      
      buf.get(dst, 2, 2);
      System.out.println(new String(dst, 2, 2));
      System.out.println(buf.position());
      
      //reset() : 恢复到 mark 的位置
      buf.reset();
      System.out.println(buf.position());
      
      //判断缓冲区中是否还有剩余数据
      if(buf.hasRemaining()){
         //获取缓冲区中可以操作的数量
         System.out.println(buf.remaining());
      }
   }
    
   @Test
   public void test1(){
      String str = "itheima";
      //1. 分配一个指定大小的缓冲区
      ByteBuffer buf = ByteBuffer.allocate(1024);
      System.out.println("-----------------allocate()----------------");
      System.out.println(buf.position());
      System.out.println(buf.limit());
      System.out.println(buf.capacity());
      
      //2. 利用 put() 存入数据到缓冲区中
      buf.put(str.getBytes());
      System.out.println("-----------------put()----------------");
      System.out.println(buf.position());
      System.out.println(buf.limit());
      System.out.println(buf.capacity());
      
      //3. 切换读取数据模式
      buf.flip();
      System.out.println("-----------------flip()----------------");
      System.out.println(buf.position());
      System.out.println(buf.limit());
      System.out.println(buf.capacity());
      
      //4. 利用 get() 读取缓冲区中的数据
      byte[] dst = new byte[buf.limit()];
      buf.get(dst);
      System.out.println(new String(dst, 0, dst.length));

      System.out.println("-----------------get()----------------");
      System.out.println(buf.position());
      System.out.println(buf.limit());
      System.out.println(buf.capacity());
      //5. rewind() : 可重复读
      buf.rewind();
      System.out.println("-----------------rewind()----------------");
      System.out.println(buf.position());
      System.out.println(buf.limit());
      System.out.println(buf.capacity());
      
      //6. clear() : 清空缓冲区. 但是缓冲区中的数据依然存在，但是处于“被遗忘”状态
      buf.clear();
      System.out.println("-----------------clear()----------------");
      System.out.println(buf.position());
      System.out.println(buf.limit());
      System.out.println(buf.capacity());
      System.out.println((char)buf.get());
      
   }

}
```

#### 4.4.2 直接与非直接缓冲区
```markdown
什么是直接内存与非直接内存

根据官方文档的描述：

byte byffer可以是两种类型，一种是基于直接内存（也就是
非堆内存）；另一种是非直接内存（也就是堆内存）。对于直
接内存来说，JVM将会在IO操作上具有更高的性能，因为它
直接作用于本地系统的IO操作。而非直接内存，也就是堆内
存中的数据，如果要作IO操作，会先从本进程内存复制到直接
内存，再利用本地IO处理。

从数据流的角度，非直接内存是下面这样的作用链：
本地IO-->直接内存-->非直接内存-->直接内存-->本地IO

而直接内存是：
本地IO-->直接内存-->本地IO

很明显，在做IO处理时，比如网络发送大量数据时，直接内
存会具有更高的效率。直接内存使用allocateDirect创建，但
是它比申请普通的堆内存需要耗费更高的性能。不过，这
部分的数据是在JVM之外的，因此它不会占用应用的内
存。所以呢，当你有很大的数据要缓存，并且它的生命
周期又很长，那么就比较适合使用直接内存。只是一般
来说，如果不是能带来很明显的性能提升，还是推荐直接
使用堆内存。字节缓冲区是直接缓冲区还是非直接缓冲
区可通过调用其 isDirect()  方法来确定。

使用场景

1 有很大的数据需要存储，它的生命周期又很长
2 适合频繁的IO操作，比如网络并发场景
```

### 4.5 NIO核心二：通道(Channel)
```markdown
通道Channe概述

通道（Channel）：由 java.nio.channels 包定义 的。
Channel 表示 IO 源与目标打开的连接。 
Channel 类似于传统的“流”。只不过 Channel 本身不
能直接访问数据，Channel 只能与 Buffer 进行交互。

1 NIO 的通道类似于流，但有些区别如下：

通道可以同时进行读写，而流只能读或者只能写

通道可以实现异步读写数据

通道可以从缓冲读数据，也可以写数据到缓冲:

2 BIO 中的 stream 是单向的，例如 FileInputStream 
对象只能进行读取数据的操作，而 NIO 中的通道(Channel)
  是双向的，可以读操作，也可以写操作。

3 Channel 在 NIO 中是一个接口
```

```java
public interface Channel extends Closeable{}
```

#### 4.5.1 常用的Channel实现类
```markdown
FileChannel：用于读取、写入、映射和操作文件的通道。
DatagramChannel：通过 UDP 读写网络中的数据通道。
SocketChannel：通过 TCP 读写网络中的数据。
ServerSocketChannel：可以监听新进来的 TCP 连接，
对每一个新进来的连接都会创建一个 SocketChannel。 
【ServerSocketChanne 类似 ServerSocket , SocketChannel 类似 Socket】

```
#### 4.5.2 FileChannel 类
```markdown
获取通道的一种方式是对支持通道的对象调用getChannel() 方法。
支持通道的类如下：

FileInputStream
FileOutputStream
RandomAccessFile
DatagramSocket
Socket
ServerSocket
获取通道的其他方式是使用 Files 类的静态方法 
newByteChannel() 获取字节通道。或者通过通道的静态
方法 open() 打开并返回指定通道
```

#### 4.5.3 FileChannel的常用方法

```java
int read(ByteBuffer dst) 从Channel到中读取数据到
ByteBuffer
long  read(ByteBuffer[] dsts) 将Channel到中的数
据“分散”到ByteBuffer[]
int  write(ByteBuffer src)将ByteBuffer 到中的
数据写入到  Channel
long write(ByteBuffer[] srcs)将ByteBuffer[] 到中
的数据“聚集”到  Channel
long position() 返回此通道的文件位置
FileChannel position(long p) 设置此通道的文件位置
long size() 返回此通道的文件的当前大小
FileChannel truncate(long s) 将此通道的文件截取为给定大小
void force(boolean metaData) 强制将所有对此通道的文
件更新写入到存储设备中
```

#### 4.5.4 案例1-本地文件写数据
```markdown
需求：使用前面学习后的 ByteBuffer(缓冲) 
和 FileChannel(通道)， 将 "hello,黑马Java程序员！" 写入
到 data.txt 中.
```


```java
import org.junit.Test;

import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.OutputStream;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;

public class ChannelTest {
    @Test
    public void write(){
        try {
            // 1、字节输出流通向目标文件
            FileOutputStream fos = new FileOutputStream("data01.txt");
            // 2、得到字节输出流对应的通道Channel
            FileChannel channel = fos.getChannel();
            // 3、分配缓冲区
            ByteBuffer buffer = ByteBuffer.allocate(1024);
            buffer.put("hello,黑马Java程序员！".getBytes());
            // 4、把缓冲区切换成写出模式
            buffer.flip();
            channel.write(buffer);
            channel.close();
            System.out.println("写数据到文件中！");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

#### 4.5.5 案例2-本地文件读数据
```markdown
需求：使用前面学习后的 ByteBuffer(缓冲) 
和 FileChannel(通道)，将data01.txt 中的数据读入到程序，
并显示在控制台屏幕
```
```java
public class ChannelTest {

    @Test
    public void read() throws Exception {
        // 1、定义一个文件字节输入流与源文件接通
        FileInputStream is = new FileInputStream("data01.txt");
        // 2、需要得到文件字节输入流的文件通道
        FileChannel channel = is.getChannel();
        // 3、定义一个缓冲区
        ByteBuffer buffer = ByteBuffer.allocate(1024);
        // 4、读取数据到缓冲区
        channel.read(buffer);
        buffer.flip();
        // 5、读取出缓冲区中的数据并输出即可
        String rs = new String(buffer.array(),0,buffer.remaining());
        System.out.println(rs);

    }
```

#### 4.5.6 案例3-使用Buffer完成文件复制
```markdown
使用 FileChannel(通道) ，完成文件的拷贝。
```
```java
@Test
public void copy() throws Exception {
    // 源文件
    File srcFile = new File("C:\\Users\\dlei\\Desktop\\BIO,NIO,AIO\\文件\\壁纸.jpg");
    File destFile = new File("C:\\Users\\dlei\\Desktop\\BIO,NIO,AIO\\文件\\壁纸new.jpg");
    // 得到一个字节字节输入流
    FileInputStream fis = new FileInputStream(srcFile);
    // 得到一个字节输出流
    FileOutputStream fos = new FileOutputStream(destFile);
    // 得到的是文件通道
    FileChannel isChannel = fis.getChannel();
    FileChannel osChannel = fos.getChannel();
    // 分配缓冲区
    ByteBuffer buffer = ByteBuffer.allocate(1024);
    while(true){
        // 必须先清空缓冲然后再写入数据到缓冲区
        buffer.clear();
        // 开始读取一次数据
        int flag = isChannel.read(buffer);
        if(flag == -1){
            break;
        }
        // 已经读取了数据 ，把缓冲区的模式切换成可读模式
        buffer.flip();
        // 把数据写出到
        osChannel.write(buffer);
    }
    isChannel.close();
    osChannel.close();
    System.out.println("复制完成！");
}
```

#### 4.5.7 案例4-分散 (Scatter) 和聚集 (Gather)
```markdown
分散读取（Scatter ）:是指把Channel通道的数据读入到
多个缓冲区中去

聚集写入（Gathering ）是指将多个 Buffer 中的数
据“聚集”到 Channel。
```
```java
//分散和聚集
@Test
public void test() throws IOException{
		RandomAccessFile raf1 = new RandomAccessFile("1.txt", "rw");
	//1. 获取通道
	FileChannel channel1 = raf1.getChannel();
	
	//2. 分配指定大小的缓冲区
	ByteBuffer buf1 = ByteBuffer.allocate(100);
	ByteBuffer buf2 = ByteBuffer.allocate(1024);
	
	//3. 分散读取
	ByteBuffer[] bufs = {buf1, buf2};
	channel1.read(bufs);
	
	for (ByteBuffer byteBuffer : bufs) {
		byteBuffer.flip();
	}
	
	System.out.println(new String(bufs[0].array(), 0, bufs[0].limit()));
	System.out.println("-----------------");
	System.out.println(new String(bufs[1].array(), 0, bufs[1].limit()));
	
	//4. 聚集写入
	RandomAccessFile raf2 = new RandomAccessFile("2.txt", "rw");
	FileChannel channel2 = raf2.getChannel();
	
	channel2.write(bufs);
}
```

#### 4.5.8 案例5-transferFrom()
```markdown
从目标通道中去复制原通道数据
```
```java
@Test
public void test02() throws Exception {
    // 1、字节输入管道
    FileInputStream is = new FileInputStream("data01.txt");
    FileChannel isChannel = is.getChannel();
    // 2、字节输出流管道
    FileOutputStream fos = new FileOutputStream("data03.txt");
    FileChannel osChannel = fos.getChannel();
    // 3、复制
    osChannel.transferFrom(isChannel,isChannel.position(),isChannel.size());
    isChannel.close();
    osChannel.close();
}
```

#### 4.5.9 案例6-transferTo()
```markdown
把原通道数据复制到目标通道
```
```java
@Test
public void test02() throws Exception {
    // 1、字节输入管道
    FileInputStream is = new FileInputStream("data01.txt");
    FileChannel isChannel = is.getChannel();
    // 2、字节输出流管道
    FileOutputStream fos = new FileOutputStream("data04.txt");
    FileChannel osChannel = fos.getChannel();
    // 3、复制
    isChannel.transferTo(isChannel.position() , isChannel.size() , osChannel);
    isChannel.close();
    osChannel.close();
}
```

### 4.6 NIO核心三：选择器(Selector)

#### 4.6.1 选择器(Selector)概述
```markdown
选择器（Selector） 是 SelectableChannle 对象的多路复用器，
Selector 可以同时监控多个 SelectableChannel 的 IO 状况，
也就是说，利用 Selector可使一个单独的线程管理多
个 Channel。Selector 是非阻塞 IO 的核心

Java 的 NIO，用非阻塞的 IO 方式。可以用一个线程，
处理多个的客户端连接，就会使用到 Selector(选择器)
Selector 能够检测多个注册的通道上是否有事件发生(注
意:多个 Channel 以事件的方式可以注册到同一个
Selector)，如果有事件发生，便获取事件然后针对每个事
件进行相应的处理。这样就可以只用一个单线程去管理多
个通道，也就是管理多个连接和请求。只有在 连接/通道 
真正有读写事件发生时，才会进行读写，就大大地减少
了系统开销，并且不必为每个连接都创建一个线程，
不用去维护多个线程
避免了多线程之间的上下文切换导致的开销
```


#### 4.6.2 选择器（Selector）的应用
```markdown
创建 Selector ：通过调用 Selector.open() 方法创建一个 
Selector。
```

```java
Selector selector = Selector.open();
向选择器注册通道：SelectableChannel.register(Selector sel, int ops)
```

```java
//1. 获取通道
ServerSocketChannel ssChannel = ServerSocketChannel.open();
//2. 切换非阻塞模式
ssChannel.configureBlocking(false);
//3. 绑定连接
ssChannel.bind(new InetSocketAddress(9898));
//4. 获取选择器
Selector selector = Selector.open();
//5. 将通道注册到选择器上, 并且指定“监听接收事件”
ssChannel.register(selector, SelectionKey.OP_ACCEPT);
```
```markdown
当调用 register(Selector sel, int ops) 将通道注册选择
器时，选择器对通道的监听事件，需要通过第二个参
数 ops 指定。可以监听的事件类型（用 可使
用 SelectionKey  的四个常量 表示）：

读 : SelectionKey.OP_READ （1）
写 : SelectionKey.OP_WRITE （4）
连接 : SelectionKey.OP_CONNECT （8）
接收 : SelectionKey.OP_ACCEPT （16）
若注册时不止监听一个事件，则可以使用“位或”操作符连接。
```
```java
int interestSet = SelectionKey.OP_READ|SelectionKey.OP_WRITE 
```

### 4.7 NIO非阻塞式网络通信原理分析

#### 4.7.1 Selector 示意图和特点说明
```markdown
Selector可以实现： 一个 I/O 线程可以并发处理 N 个客户
端连接和读写操作，这从根本上解决了传统同步阻塞 I/O 一
连接一线程模型，架构的性能、弹性伸缩能力和可靠性
都得到了极大的提升。
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210323172636252.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

#### 4.7.2 服务端流程
```markdown
1 当客户端连接服务端时，服务端会通过 
ServerSocketChannel 得到 SocketChannel：1 获取通道
ServerSocketChannel ssChannel = ServerSocketChannel.open();

2 切换非阻塞模式 
ssChannel.configureBlocking(false);

3 绑定连接
  ssChannel.bind(new InetSocketAddress(9999));
  
4 获取选择器
Selector selector = Selector.open();

5 将通道注册到选择器上, 并且指定“监听接收事件”
  ssChannel.register(selector, SelectionKey.OP_ACCEPT);

6  轮询式的获取选择器上已经“准备就绪”的事件
```


```java
  //轮询式的获取选择器上已经“准备就绪”的事件
   while (selector.select() > 0) {
          System.out.println("轮一轮");
          //7. 获取当前选择器中所有注册的“选择键(已就绪的监听事件)”
          Iterator<SelectionKey> it = selector.selectedKeys().iterator();
          while (it.hasNext()) {
              //8. 获取准备“就绪”的是事件
              SelectionKey sk = it.next();
              //9. 判断具体是什么事件准备就绪
              if (sk.isAcceptable()) {
                  //10. 若“接收就绪”，获取客户端连接
                  SocketChannel sChannel = ssChannel.accept();
                  //11. 切换非阻塞模式
                  sChannel.configureBlocking(false);
                  //12. 将该通道注册到选择器上
                  sChannel.register(selector, SelectionKey.OP_READ);
              } else if (sk.isReadable()) {
                  //13. 获取当前选择器上“读就绪”状态的通道
                  SocketChannel sChannel = (SocketChannel) sk.channel();
                  //14. 读取数据
                  ByteBuffer buf = ByteBuffer.allocate(1024);
                  int len = 0;
                  while ((len = sChannel.read(buf)) > 0) {
                      buf.flip();
                      System.out.println(new String(buf.array(), 0, len));
                      buf.clear();
                  }
              }
              //15. 取消选择键 SelectionKey
              it.remove();
          }
      }
  }
```

#### 4.7.3 客户端流程
```java
1 获取通道
SocketChannel sChannel = SocketChannel.open(new InetSocketAddress("127.0.0.1", 9999));


2 切换非阻塞模式
sChannel.configureBlocking(false);

3 分配指定大小的缓冲区

ByteBuffer buf = ByteBuffer.allocate(1024);

4 发送数据给服务端

  	Scanner scan = new Scanner(System.in);
  	while(scan.hasNext()){
  		String str = scan.nextLine();
  		buf.put((new SimpleDateFormat("yyyy/MM/dd HH:mm:ss").format(System.currentTimeMillis())
  				+ "\n" + str).getBytes());
  		buf.flip();
  		sChannel.write(buf);
  		buf.clear();
  	}
  	//关闭通道
  	sChannel.close();
```


### 4.8 NIO非阻塞式网络通信入门案例
```markdown
需求：服务端接收客户端的连接请求，并接收多个客户端发
送过来的事件。
```
```java
/**
  客户端
 */
public class Client {

	public static void main(String[] args) throws Exception {
		//1. 获取通道
		SocketChannel sChannel = SocketChannel.open(new InetSocketAddress("127.0.0.1", 9999));
		//2. 切换非阻塞模式
		sChannel.configureBlocking(false);
		//3. 分配指定大小的缓冲区
		ByteBuffer buf = ByteBuffer.allocate(1024);
		//4. 发送数据给服务端
		Scanner scan = new Scanner(System.in);
		while(scan.hasNext()){
			String str = scan.nextLine();
			buf.put((new SimpleDateFormat("yyyy/MM/dd HH:mm:ss").format(System.currentTimeMillis())
					+ "\n" + str).getBytes());
			buf.flip();
			sChannel.write(buf);
			buf.clear();
		}
		//5. 关闭通道
		sChannel.close();
	}
}

/**
 服务端
 */
public class Server {
    public static void main(String[] args) throws IOException {
        //1. 获取通道
        ServerSocketChannel ssChannel = ServerSocketChannel.open();
        //2. 切换非阻塞模式
        ssChannel.configureBlocking(false);
        //3. 绑定连接
        ssChannel.bind(new InetSocketAddress(9999));
        //4. 获取选择器
        Selector selector = Selector.open();
        //5. 将通道注册到选择器上, 并且指定“监听接收事件”
        ssChannel.register(selector, SelectionKey.OP_ACCEPT);
        //6. 轮询式的获取选择器上已经“准备就绪”的事件
        while (selector.select() > 0) {
            System.out.println("轮一轮");
            //7. 获取当前选择器中所有注册的“选择键(已就绪的监听事件)”
            Iterator<SelectionKey> it = selector.selectedKeys().iterator();
            while (it.hasNext()) {
                //8. 获取准备“就绪”的是事件
                SelectionKey sk = it.next();
                //9. 判断具体是什么事件准备就绪
                if (sk.isAcceptable()) {
                    //10. 若“接收就绪”，获取客户端连接
                    SocketChannel sChannel = ssChannel.accept();
                    //11. 切换非阻塞模式
                    sChannel.configureBlocking(false);
                    //12. 将该通道注册到选择器上
                    sChannel.register(selector, SelectionKey.OP_READ);
                } else if (sk.isReadable()) {
                    //13. 获取当前选择器上“读就绪”状态的通道
                    SocketChannel sChannel = (SocketChannel) sk.channel();
                    //14. 读取数据
                    ByteBuffer buf = ByteBuffer.allocate(1024);
                    int len = 0;
                    while ((len = sChannel.read(buf)) > 0) {
                        buf.flip();
                        System.out.println(new String(buf.array(), 0, len));
                        buf.clear();
                    }
                }
                //15. 取消选择键 SelectionKey
                it.remove();
            }
        }
    }
}

```

### 4.9 NIO 网络编程应用实例-群聊系统
```markdown
需求:进一步理解 NIO 非阻塞网络编程机制，实现多人群聊

编写一个 NIO 群聊系统，实现客户端与客户端的通信需求（非阻塞）
服务器端：可以监测用户上线，离线，并实现消息转发功能
客户端：通过 channel 可以无阻塞发送消息给其它所有客
户端用户，同时可以接受其它客户端用户通过服务端转发
来的消息
```


#### 4.9.1 服务端代码实现

```java
public class Server {
    //定义属性
    private Selector selector;
    private ServerSocketChannel ssChannel;
    private static final int PORT = 9999;
    //构造器
    //初始化工作
    public Server() {
        try {
            // 1、获取通道
            ssChannel = ServerSocketChannel.open();
            // 2、切换为非阻塞模式
            ssChannel.configureBlocking(false);
            // 3、绑定连接的端口
            ssChannel.bind(new InetSocketAddress(PORT));
            // 4、获取选择器Selector
            selector = Selector.open();
            // 5、将通道都注册到选择器上去，并且开始指定监听接收事件
            ssChannel.register(selector , SelectionKey.OP_ACCEPT);
        }catch (IOException e) {
            e.printStackTrace();
        }
    }
    //监听
    public void listen() {
        System.out.println("监听线程: " + Thread.currentThread().getName());
        try {
            while (selector.select() > 0){
                System.out.println("开始一轮事件处理~~~");
                // 7、获取选择器中的所有注册的通道中已经就绪好的事件
                Iterator<SelectionKey> it = selector.selectedKeys().iterator();
                // 8、开始遍历这些准备好的事件
                while (it.hasNext()){
                    // 提取当前这个事件
                    SelectionKey sk = it.next();
                    // 9、判断这个事件具体是什么
                    if(sk.isAcceptable()){
                        // 10、直接获取当前接入的客户端通道
                        SocketChannel schannel = ssChannel.accept();
                        // 11 、切换成非阻塞模式
                        schannel.configureBlocking(false);
                        // 12、将本客户端通道注册到选择器
                        System.out.println(schannel.getRemoteAddress() + " 上线 ");
                        schannel.register(selector , SelectionKey.OP_READ);
                        //提示
                    }else if(sk.isReadable()){
                        //处理读 (专门写方法..)
                        readData(sk);
                    }

                    it.remove(); // 处理完毕之后需要移除当前事件
                }
            }
        }catch (Exception e) {
            e.printStackTrace();
        }finally {
            //发生异常处理....

        }
    }

    //读取客户端消息
    private void readData(SelectionKey key) {
        //取到关联的channle
        SocketChannel channel = null;
        try {
           //得到channel
            channel = (SocketChannel) key.channel();
            //创建buffer
            ByteBuffer buffer = ByteBuffer.allocate(1024);
            int count = channel.read(buffer);
            //根据count的值做处理
            if(count > 0) {
                //把缓存区的数据转成字符串
                String msg = new String(buffer.array());
                //输出该消息
                System.out.println("form 客户端: " + msg);
                //向其它的客户端转发消息(去掉自己), 专门写一个方法来处理
                sendInfoToOtherClients(msg, channel);
            }
        }catch (IOException e) {
            try {
                System.out.println(channel.getRemoteAddress() + " 离线了..");
                e.printStackTrace();
                //取消注册
                key.cancel();
                //关闭通道
                channel.close();
            }catch (IOException e2) {
                e2.printStackTrace();;
            }
        }
    }

    //转发消息给其它客户(通道)
    private void sendInfoToOtherClients(String msg, SocketChannel self ) throws  IOException{
        System.out.println("服务器转发消息中...");
        System.out.println("服务器转发数据给客户端线程: " + Thread.currentThread().getName());
        //遍历 所有注册到selector 上的 SocketChannel,并排除 self
        for(SelectionKey key: selector.keys()) {
            //通过 key  取出对应的 SocketChannel
            Channel targetChannel = key.channel();
            //排除自己
            if(targetChannel instanceof  SocketChannel && targetChannel != self) {
                //转型
                SocketChannel dest = (SocketChannel)targetChannel;
                //将msg 存储到buffer
                ByteBuffer buffer = ByteBuffer.wrap(msg.getBytes());
                //将buffer 的数据写入 通道
                dest.write(buffer);
            }
        }
    }

    public static void main(String[] args) {
        //创建服务器对象
        Server groupChatServer = new Server();
        groupChatServer.listen();
    }
}
```

#### 4.9.2 客户端代码实现

```java
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.nio.channels.SocketChannel;
import java.util.Iterator;
import java.util.Scanner;

public class Client {
    //定义相关的属性
    private final String HOST = "127.0.0.1"; // 服务器的ip
    private final int PORT = 9999; //服务器端口
    private Selector selector;
    private SocketChannel socketChannel;
    private String username;

    //构造器, 完成初始化工作
    public Client() throws IOException {

        selector = Selector.open();
        //连接服务器
        socketChannel = socketChannel.open(new InetSocketAddress("127.0.0.1", PORT));
        //设置非阻塞
        socketChannel.configureBlocking(false);
        //将channel 注册到selector
        socketChannel.register(selector, SelectionKey.OP_READ);
        //得到username
        username = socketChannel.getLocalAddress().toString().substring(1);
        System.out.println(username + " is ok...");

    }

    //向服务器发送消息
    public void sendInfo(String info) {
        info = username + " 说：" + info;
        try {
            socketChannel.write(ByteBuffer.wrap(info.getBytes()));
        }catch (IOException e) {
            e.printStackTrace();
        }
    }

    //读取从服务器端回复的消息
    public void readInfo() {
        try {

            int readChannels = selector.select();
            if(readChannels > 0) {//有可以用的通道

                Iterator<SelectionKey> iterator = selector.selectedKeys().iterator();
                while (iterator.hasNext()) {

                    SelectionKey key = iterator.next();
                    if(key.isReadable()) {
                        //得到相关的通道
                       SocketChannel sc = (SocketChannel) key.channel();
                       //得到一个Buffer
                        ByteBuffer buffer = ByteBuffer.allocate(1024);
                        //读取
                        sc.read(buffer);
                        //把读到的缓冲区的数据转成字符串
                        String msg = new String(buffer.array());
                        System.out.println(msg.trim());
                    }
                }
                iterator.remove(); //删除当前的selectionKey, 防止重复操作
            } else {
                //System.out.println("没有可以用的通道...");

            }

        }catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) throws Exception {
        //启动我们客户端
        Client chatClient = new Client();
        //启动一个线程, 每个3秒，读取从服务器发送数据
        new Thread() {
            public void run() {

                while (true) {
                    chatClient.readInfo();
                    try {
                        Thread.currentThread().sleep(3000);
                    }catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }.start();

        //发送数据给服务器端
        Scanner scanner = new Scanner(System.in);

        while (scanner.hasNextLine()) {
            String s = scanner.nextLine();
            chatClient.sendInfo(s);
        }
    }
}
```


## 5 JAVA AIO深入剖析

### 5.1 AIO编程
```markdown
Java AIO(NIO.2) ： 异步非阻塞，服务器实现模式为一个有效
请求一个线程，客户端的I/O请求都是由OS先完成了再通知服
务器应用去启动线程进行处理。
```
```java
AIO
异步非阻塞，基于NIO的，可以称之为NIO2.0
    BIO                   NIO                              AIO        
Socket                SocketChannel                    AsynchronousSocketChannel
ServerSocket          ServerSocketChannel	       AsynchronousServerSocketChannel

与NIO不同，当进行读写操作时，只须直接调用API的
read或write方法即可, 这两种方法均为异步的，对于
读操作而言，当有流可读取时，操作系统会将可读的
流传入read方法的缓冲区,对于写操作而言，当操作
系统将write方法传递的流写入完毕时，操作系统
主动通知应用程序

即可以理解为，read/write方法都是异步的，完成后
会主动调用回调函数。在JDK1.7中，这部分内容被
称作NIO.2，主要在Java.nio.channels包下增加
了下面四个异步通道：
```

```java
AsynchronousSocketChannel
AsynchronousServerSocketChannel
AsynchronousFileChannel
AsynchronousDatagramChannel
```

## 6 总结
```markdown
BIO、NIO、AIO：
Java BIO ： 同步并阻塞，服务器实现模式为一个连接一个线程，
即客户端有连接请求时服务器端就需要启动一个线程进行处
理，如果这个连接不做任何事情会造成不必要的线程开销，
当然可以通过线程池机制改善。
Java NIO ： 同步非阻塞，服务器实现模式为一个请求一个
线程，即客户端发送的连接请求都会注册到多路复用器
上，多路复用器轮询到连接有I/O请求时才启动一个线程
进行处理。
Java AIO(NIO.2) ： 异步非阻塞，服务器实现模式为一个有
效请求一个线程，客户端的I/O请求都是由OS先完成了
再通知服务器应用去启动线程进行处理。


BIO、NIO、AIO适用场景分析:
BIO方式适用于连接数目比较小且固定的架构，这种方
式对服务器资源要求比较高，并发局限于应用中，
JDK1.4以前的唯一选择，但程序直观简单易理解。
NIO方式适用于连接数目多且连接比较短（轻操作）的
架构，比如聊天服务器，并发局限于应用中，编程比较
复杂，JDK1.4开始支持。
AIO方式使用于连接数目多且连接比较长（重操作）的
架构，比如相册服务器，充分调用OS参与并发操作，编
程比较复杂，JDK7开始支持。Netty!
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复bna
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)







