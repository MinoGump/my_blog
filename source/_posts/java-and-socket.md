title: Java Socket 编程
date: 2015-12-08 19:11:39
tags:
- Java
- Socket
categories:
- Tech
- Web
- Socket
---

# Java Socket 编程

### 1. 异常

**异常与错误的区别**
错误(error)指程序运行时遇到的硬件或操作系统的错误，比如 OutOfMemoryError、ThreadDeath等。这些异常发生时，Java 虚拟机(JVM)一般会选择线程终止。异常(exception)指在硬件和操作系统正常时，程序遇到的运行错，程序本身可以处理的异常。

**Java的异常处理**
Java的异常就像是bug，需要程序员在代码中用if式的判断throw出异常、进行处理。Java的异常类继承了Exception的基类。下面的代码中演示了OutOfMoney异常类的实现：
![OutOfMoney](/img/java_socket/OutOfMoney.png  "OutOfMoney")

**代码运行结果**
![AccountException](/img/java_socket/AccountException.png  "AccountException")

---

### 2. Java多线程

- #### (1) 应用 Runnable 创建线程

Runnable是一个简单的接口,其中定义了所有线程类必须实现的抽象方法run()。实现Runnable接口的线程类可节约Java程序中宝贵的单继承指标。例如，一个Applet程序常常以这种方式实现多线程,因为Applet本身需要继承javax.swing.JApplet类。完成一个实现了 Runnable接口的线程类后，可创建该线程类的一个实例，然后再以该实例为参数创建Thread类的一个实例，最后调用该Thread实例的start()方法，从而创建了一个新线程。

**代码**
![Runable](/img/java_socket/Runable.png  "Runable")

**代码运行结果**
![RunableThread](/img/java_socket/RunableThread.png  "RunableThread")

**代码分析**
在该主函数中，首先新建了一个Counter类，交给Thread类的实例thread执行，然后主函数结束。由结果观察出因为主函数这个线程不耗时，因此先执行这个线程，最后再执行含有Counter的线程。

- #### (2) 继承 Thread 类

继承 Thread 类的线程类可用更简单的代码完成同样的事情。在利用继承 Thread 类的线程类创建一个新线程时，可直接创建该线程类的一个实例。

**代码**
![SubclassThread](/img/java_socket/SubclassThread.png  "SubclassThread")

**代码运行结果**
![SubclassThreadResult](/img/java_socket/SubclassThreadResult.png  "SubclassThreadResult")

**代码分析**
在该主函数中，首先新建了一个SubclassThread类，该类继承了Thread类。由结果观察出因为主函数这个线程不耗时，因此先执行这个线程，最后再执行含有SubclassThread的线程。

---

### 3. Java序列化

在分布式计算环境下，进程间是通过相互之间传递消息实现远程通信，消息中包含有各种类型的数据。而无论是何种类型的数据，都是以二进制序列的形式在网络上传送。这就需要发送进程将对象转换为字节序列，才能在网络上传送；接收进程则需要把字节序列再恢复为对象。在进程间消息通信过程中，把Java对象转换为字节序列的过程称为对象的序列化。把字节序列恢复为Java对象的过程称为对象的反序列化。由此，可以看出，对象的序列化主要有两种用途：将对象的字节序列持久化，保存在文件系统中；在网络上传送对象的字节序列。

**代码**
![AccountSerialiable](/img/java_socket/AccountSerializable.png  "AccountSerialiable")

**代码运行结果**
![AccountSerialiableResult](/img/java_socket/AccountSerializableResult.png  "AccountSerialiableResult")

**代码分析**
在该代码中，主函数新建了一个二进制流文件，然后进行写Object操作，将两个Account写入到该文件中，最后从该文件中按顺序读出来，获得Account的信息。

注意Java语言中要求只有实现了java.io.Serializable接口的类的对象才能被序列化及
反序列化。JDK类库中的有些类(如String类、包装类、Date类等)都实现了Serializable接口。 对象的序列化包括以下步骤：创建一个对象输出流，它可以包装一个其他类型的输出流，比如文件输出流；通过对象输出流写对象。对象的反序列化包括以下步骤：创建一个对象输入流，它可以包装一个其他类型的输入流，比如文件输入流；通过对象输入流读取对象。另外，在对象的序列化和反序列化过程当中，必须注意的是：为了能读出正确的数据，必须保证对象输出流的写对象的顺序与对象输入流读对象的顺序是一致的。

---


### 4. Java 语言的反射机制

- #### (1) Java 语言的反射机制原理

通常将程序运行时，可以程序结构或变量类型的语言为动态语言。从这个观点看，perl、phthon、ruby 是动态语言，C++、JAVA、C#不是动态语言。Java 语言的反射(Reflection)机制则可以在程序运行时判断任意一个对象所属的类，构造任意一个类的对象，判断任意一个类所具有的成员变量和方法，调用任意一个对象的方法，或者生成动态代理。

在 JDK 中，主要由 java.lang.reflect 包中的 Class 类、Field 类、Method 类、Constructor类、Array 类来实现反射机制，它们分别代表类、类的成员变量类的方法、类的构造方法，动态创建数组以及访问数组元素的静态方法。

**代码**
![AccountReflect](/img/java_socket/AccountReflect.png  "AccountReflect")

**代码运行结果**
![AccountReflectResult](/img/java_socket/AccountReflectResult.png  "AccountReflectResult")

**代码分析**
该代码进行了一个类型复制，调用AccountReflect的copy函数，该函数调用被复制的类型的全部函数，然后进行了一次新的调用，输出结果，返回一个新的Object。

- #### (2) 应用反射机制实现远程方法调用

在学习了 Java 语言的反射机制后，我们很自然地会想到如何将这一机制应用于 Java 的网络程序设计中。我们可以试想，通过反射机制，客户端可以将向服务端发出的请求对象进行封装，客户端只需要知道远程对象名及其提供服务的方法名，就可以与服务端实现通信。本节应用反射机制实现远程方法调用例程综合采用了基于流的 Socket 编程，序列化机制、反射机制。

**代码**
服务器：
![AccountRemoteReflectServer](/img/java_socket/AccountRemoteReflectServer.png  "AccountRemoteReflectServer")
客户端：
![AccountRemoteReflectClient](/img/java_socket/AccountRemoteReflectClient.png  "AccountRemoteReflectClient")

**代码运行结果**
服务器：
![AccountRemoteReflectResult_Server](/img/java_socket/AccountRemoteReflectResult_Server.png  "AccountRemoteReflectResult_Server")
客户端：
![AccountRemoteReflectResult_Client](/img/java_socket/AccountRemoteReflectResult_Client.png  "AccountRemoteReflectResult_Client")

**代码分析**
在该代码中，客户端使用了封装的方法，将Account类和方法封装起来，通过数据流交给服务器远程调用。服务器收到流后，从AccountService中进行调用方法，将结果返回给客户端。
![ClientRemoteReflect](/img/java_socket/ClientRemoteReflect.png  "ClientRemoteReflect")

- #### (3) 应用代理模式实现远程方法调用

**代理模式**
首先理解代理模式，代理就是第三方，比如明星的经纪人，明星的事务都交给经纪人来处理，明星只要告诉经纪人去做什么，经纪人自然会想办法去做，做完之后再把结果告诉明星就好了。

代理模式是一个很大的模式，所以应用很广泛，从代理的种类就能看出来了：

- 远程代理：最经典的代理模式之一，远程代理负责与远程JVM通信，以实现本地调用者与远程被调用者之间的正常交互
- 虚拟代理：用来代替巨大对象，确保它在需要的时候才被创建
- 保护代理：给被调用者提供访问控制，确认调用者的权限
- 此外还有防火墙代理，智能引用代理，缓存代理，同步代理，复杂隐藏代理，写入时复制代理等等，都有各自特殊的用途

**代码运行结果**
服务器：
![Server](/img/java_socket/Server.png  "Server")
客户端：
![Client](/img/java_socket/Client.png  "Client")


**代码分析**
在该代码中，客户端分别调用了DynamicProxyService和StaticProxyService这两个类，其中静态代理类由程序员创建或由特定工具自动生成源代码，再对其编译。在程序运行前，代理类的.class 文件就已经存在了。而动态代理类是在程序运行时,运用反射机制动态创建而成。
![ServiceProxy](/img/java_socket/ServiceProxy.png  "ServiceProxy")
![ProxyClient](/img/java_socket/ProxyClient.png  "ProxyClient")
客户端将要调用的方法打包给代理，代理使用Connector类发送给服务器，服务器收到方法调用后调用这个方法，将结果返回给代理，最后返回给客户端。

---

### TCP Socket 编程

基于 socket 通信的服务程序负责监听对外发布的端口号，该端口专用于处理客户程序的连接请求。因而服务程序首先通过指定监听的端口号创建一个 ServerSocket 实例，然后调用该实例的 accept()方法；调用 accept()方法会引起阻塞，直至有一个客户程序发送连接请求到服务程序所监听的端口，服务程序收到连接请求后将分配一个新端口号建立与客户程序的连接，并返回该连接的一个 socket 。 然后，服务程序可调用该 socket 的getInputStream() 和getOutputStream()方法获取与客户程序的连接相关联的输入流和输出流，并依照预先约定的协议读输入流或写输出流。完成所有通信后，服务程序必须依次关闭所有输入流和输出流、所有已建立连接的 socket、以及专用于监听的 socket。

**代码**
服务器：
![TCPServer](/img/java_socket/TCPServer.png  "TCPServer")
客户端：
![TCPClient](/img/java_socket/TCPClient.png  "TCPClient")

**代码运行结果**
服务器：
![TCPResult_Server](/img/java_socket/TCPResult_Server.png  "TCPResult_Server")
客户端：
![TCPResult_Client](/img/java_socket/TCPResult_Client.png  "TCPResult_Client")

**代码分析**
在该代码中，服务器首先监听端口，客户端随后连接端口。服务器和客户端都建立socket，将客户端的请求由客户端的socket发送至服务器，服务器接受请求后将响应发送给客户端。当收到的请求是end时就关闭连接。

