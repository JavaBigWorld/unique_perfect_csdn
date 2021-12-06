# 深入理解JVM-篇章1
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210715185318340.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1 JVM与Java体系结构
### 1.1 虚拟机
```markdown
所谓虚拟机( Virtua Machine),就是一台虚拟的计算机。它是一款
软件，用来执行一系列虚拟计算机指令。大体上，虚拟机可以
分为系统虚拟机和程序虚拟机。
大名鼎鼎的visual Box, Vmware就属于系统虚拟机，它们完全是
对物理计算机的仿真，提供了一个可运行完整操作系统的软件平台

程序虚拟机的典型代表就是Java虚拟机，它专门为执行单个计
算机程序而设计，在Java虚拟机中执行的指令我们称为Java字
节码指令
无论是系统虚拟机还是程序虚拟机，在上面运行的软件都被限
制于虛拟机提供的资源中
```
### 1.2 Java虚拟机
```markdown
JaVa虚拟机是一台执行Java字节码的虚拟计算机，它拥有独
立的运行机制，其运行的Java字节码也未必由Java语言编译而成
JVM平台的各种语言可以共享Java虚拟机带来的跨平台性、优
秀的垃圾回收器，以及可靠的即时编译器。
Java技术的核心就是Java虚拟机(JVM, Java Virtual Machine)
因为所有的Java程序都运行在Java虚拟机内部。


作用
Java虚拟机就是二进制字节码的运行环境，负责装载字节
码到其内部，解释/编译为对应平台上的机器指令执行。每一
条Java指令，Java虚拟机规范中都有详细定义，如怎么取
操作数，怎么处理操作数，处理结果放在哪里。
特点
一次编译，到处运行
自动内存管理
自动垃圾回收功能
```
### 1.3 JVM的位置
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210202200056141.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
JVM是运行在操作系统之上的，它与硬件没有直接的交互
```

### 1.4 JVM的整体结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203101709503.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203101849570.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 1.5 Java代码执行流程
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203103259433.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 1.6 JVM的架构模型
```markdown
Java编译器输入的指令流基本上是一种基于栈的指令集架构，另
外一种指令集架构则
是基于寄存器的指令集架构。
具体来说：这两种架构之间的区别：
基于栈式架构的特点
设计和实现更简单，适用于资源受限的系统
避开了寄存器的分配难题：使用零地址指令方式分配。
指令流中的指令大部分是零地址指令，其执行过程依赖于操作栈。
指令集更小，
编译器容易实现。
不需要硬件支持，可移植性更好，更好实现跨平台

基于寄存器架构的特点
典型的应用是x86的二进制指令集：比如传统的PC以及 
Android的Davlik虚拟机
指令集架构则完全依赖硬件，可移植性差
性能优秀和执行更高效
花费更少的指令去完成一项操作。
在大部分情況下，基于寄存器架构的指令集往往都以一地址指
令、二地址指令
和三地址指令为主，而基于栈式架构的指令集却是以零地址指令为主
```
### 1.7 JVM生命周期
```markdown
虚拟机的启动
Java虚拟机的启动是通过引导类加载器
( bootstrap class loader)创建个初始类( initial class)
来完成的，这个类是由虚拟机的具体实现指定的。

虚拟机的执行
一个运行中的Java虚拟机有着一个清晰的任务：执行Java程序。
程序开始执行时他才运行，程序结束时他就停止
执行一个所谓的Java程序的时候，真真正正在执行的是一个
叫做Java虚拟机的进程。

虚拟机的退出
有如下的几种情况：
程序正常执行结束
程序在执行过程中遇到了异常或错误而异常终止
由于操作系统出现错误而导致Java虚拟机进程终止
某线程调用 Runtime类或 System类的exit方法，或 
Runtime类的halt方法，并且Java安全管理器也允许这
次exit或halt操作。除此之外，JNI( Java Native Interface)规范描述了用JNI
Invocation API来加载或卸载Java虚拟机时，Java虚拟
机的退出情況。
```
### 1.8 JVM发展历程
```markdown
Sun Classic VM
早在1996年Java1.0版本的时候，Sun公司发布了一款名
为Sun Classic VM的Java虚拟机，它同时也是世界上第一款
商用Java虚拟机，JDK1.4时完全被淘汰。
这款虚拟机内部只提供解释器。
如果使用JIT编译器，就需要进行外挂。但是一旦使用了JIT编译器，
JIT就会接管虚拟机的执行系统。解释器就不再工作。解释器和编
译器不能配合工作。现在 hotspot内置了此虚拟机

Exact VM
为了解决上一个虚拟机问题，jdk1.2时，sun提供了此虚拟机。
Exact Memory Management:准确式内存管理
也可以叫Non- Conservative/ Accurate Memory Management
虚拟机可以知道内存中某个位置的数据具体是什么类型。
具备现代高性能虚拟机的雏形
热点探测
编译器与解释器混合工作模式
只在Solaris平台短暂使用，其他平台上还是classic VM
英雄气短，终被 Hotspot虚拟机替换

SUN公司的 HotSpot VM
HotSpot历史
最初由一家名为“ Longview Technologies"的小公司设计
1997年，此公司被Sun收购；2009年，Sun公司被甲骨文收购。
JDK1.3时， Hotspot VM成为默认虚拟机
目前 Hotspot占有绝对的市场地位，称霸武林。
不管是现在仍在广泛使用的JDK6,还是使用比例较多的JDK8中，
默认的虚拟机都是Hotspot

Sun/Oracle JDK和 Open JDKI的默认虚拟机
因此本课程中默认介绍的虚拟机都是 Hotspot,相关机制也主要是
指 Hotspot的GC机制。（比如其他两个商用虚拟机都没有方法区的概念)
从服务器、桌面到移动端、嵌入式都有应用。
名称中的 Hotspot指的就是它的热点代码探测技术。
通过计数器找到最具编译价值代码，触发即时编译或栈上替换
通过编译器与解释器协同工作，在最优化的程序响应时间与最佳执行
性能中取得平衡

BEA的 JRockit
专注于服务器端应用
它可以不太关注程序启动速度，因此 JRockit内部不包含解析器实
现，全部代码都靠即时编译器编译后执行
大量的行业基准测试显示， JRockit JVM是世界上最快的JVM.
使用 JRockitl产品，客户已经体验到了显著的性能提高(一些超
过了70%)和硬件成本的减少(达50%).
优势：全面的Java运行时解决方案组合
JRockit面向延迟敏感型应用的解决方案 JRockit Real Time提供
以毫秒或微秒级的JVM响应时间，适合财务、军事指挥、电信网络
的需要MissionContro1服务套件，它是一组以极低的开销来监控、
管理和分析生产环境中的应用程序的工具。
2008年，BEA被Oracle收购。
Oracle表达了整合两大优秀虚拟机的工作，大致在JDK8中完成。
整合的方式是在Hotspot的基础上，移植 JRockit的优秀特性
高斯林：目前就职于谷歌，研究人工智能和水下机器


IBM的 J9
全称： IBM Technology for Java Virtual Machine,
简称IT4J,内部代号：J9
市场定位与 Hotspot接近，服务器端、桌面应用、嵌入式等多用途VM
广泛用于IBM的各种Java产品。
目前，有影响力的三大商用虚拟机之一，也号称是世界上最
快的Java虚拟机。
2017年左右，IBM发布了开源J9 VM,命名为 OpenJ9,
交给Eclipse基金会管理，也称为 Eclipse OpenJ9

KVM和CDC/ CLDC Hotspot
Oracle在 Java ME产品线上的两款虚拟机为：
CDC/ CLDC Hotspot
Implementation VM
KVM(Kilobyte)是CLDC-HI早期产品
目前移动领域地位尴尬，智能手机被 Android和iOS二分天下。
KVM简单、轻量、高度可移植，面向更低端的设备上还维持自己的一片
市场
智能控制器、传感器
老人手机、经济欠发达地区的功能手机
所有的虚拟机的原则：一次编译，到处运行。

Azul VM
前面三大“高性能Java虚拟机”使用在通用硬件平台上
这里Azul VM和 BEA Liquid VM是与特定硬件平台绑定、
软硬件配合的专有虚拟机
高性能Java虚拟机中的战斗机。
Azul VM是Azul Systems公司在 Hotspot基础上进行大量改进，
运行于
Azul Systems公司的专有硬件Vega系统上的Java虚拟机。
每个Azul VM实例都可以管理至少数十个CPU和数百GB内
存的硬件资源，并提供在巨大内存范围内实现可控的GC
时间的垃圾收集器、专有硬件优化的线程调度等优秀特性。
2010年，Azul Systems公司开始从硬件转向软件，发布了自
己的Zing JVM,可以在通用x86平台上提供接近于Vega系统的特性。

Liquid VM
高性能Java虚拟机中的战斗机。
BEA公司开发的，直接运行在自家 Hypervisor系统上
Liquid VM即是现在的 JRockit VE( Virtual Edition),
Liquid VM不需要操作系统的支持，或者说它自己本身实现了一个
专用操作系统的必要功能，如线程调度、文件系统、网络支持等。
随着 JRockit虚拟机终止开发， Liquid VM.项目也停止了。

Apache Harmony
Apache也曾经推出过与JDK1.5和JDK1.6兼容的Java运行平
台Aapache Harmony 它是IBM和Intel联合开发的开源JVM,受
到同样开源的 OPENJDK的压制，Sun坚决不让 Harmony获得
JCP认证，最终于2011年退役，IBM转而参与OPENJDK
虽然目前并没有 Apache Harmony被大规模商用的案例，但是
它的Java类库代码吸纳进了 Android SDK。

Microsoft JVM
微软为了在IE3浏览器中支持 Java Applets,开发
了 Microsoft JVM 只能在 window平台下运行。但
确是当时网windows下性能最好的 Java VM.1997年，Sun以
侵犯商标、不正当竞争罪名指控微软成功，赔了sun很多
钱。微软在 Windowsxp SP3中抹掉了其VM。现在 windows上
安装的jdk 都是Hotspot。

TaobaoJVM
由AliJVM团队发布。阿里，国内使用Java最强大的公司，覆
盖云计算、金融、物流、电商等众多领域，需要解决高并
发、高可用、分布式的复合问题。有大量的开源产品。
基于 OPENJDK开发了自己的定制版本AlibabaJDK,简称AJDK。是
整个阿里Java体系的基石。
基于 OpenJDK Hotspot VM发布的国内第一个优化、深度定
制且开源的高性能服务器\版Java虚拟机。
创新的GCIH( GC invisible heap)技术实现了off-heap,即将
生命周期较长的Java对象从heap中移到heap之外，并且GC不能
管理GCIH内部的Java对象，以此达到降低GC的回收频率和提
升GC的回收效率的目的。
GCIH中的对象还能够在多个Java虚拟机进程中实现共享
使用crc32指令实现 JVM intrinsic降低JNェ的调用开销
PMU hardware的 Java profiling too1和诊断协助功能
针对大数据场景的ZenGC
taobao vm应用在阿里产品上性能高，硬件严重依赖inte1的cpu,损
失了兼容性，但提高了性能
目前己经在淘宝、天猫上线，把Orac1e官方JWM版本全部替换了

Dalvik VM:
谷歌开发的，应用于 Android系统，并在 Android2.2中提供
了JIT,发展迅猛。
Dalvik VM只能称作虚拟机，而不能称作“Java虚拟机”，它
没有遵循Java虚拟机规范
不能直接执行Java的Class文件
基于寄存器架构，不是jvm的栈架构。
执行的是编译以后的dex(Dalvik Executable)文件。执
行效率比较高。
它执行的dex( Dalvik Executable)文件可以通过Class文
件转化而来，使用Java语法编写应用程序，可以直接使用
大部分的 Java API等。
Android5.0使用支持提前编译
( Ahead Of Time Compilation,AOT)的ART VM替换Dalvik VM

其他JVM:
Java Card VM、Squawk VM、JavaInjava、 Maxine VM、
Jikes RVM、IKVM.NET、 Jam VM、Cacao VM、 
Sable VM、Kaffe、 Jelatine JVM、 Nano VM、
MRP、Moxie JVM

Graal VM
2018年4月， Oracle Labs公开了Graal VM,号称
"Run Programs Faster Anywhere",勃勃野心。
与1995年java的”write once,run anywhere"遥相呼应。
Graal VM在 Hotspot VM基础上増强而成的跨语言全栈虚拟机，
可以作为“任何语言”的运行平台使用。语言包括：Java、 
Scala、 Groovy、Kotlin;C、javascript、Ruby、 Python、
R等
支持不同语言中混用对方的接口和对象，支持这些语言
使用已经编写好的木地库文件
工作原理是将这些语言的源代码或源代码编译后的中间格式，通
过解释器转换为能被
Graal VM接受的中间表示。Graal VM提供Truffle工具集快速构建
面向一种新语言的解释器。在运行时还能进行即时编译优化，获得比原生编译器
更优秀的执行效率。
如果说 HotSpot有一天真的被取代，Graal VM希望最大。但是Ja
va的软件生态没有丝毫变化。
```
## 2 类加载器子系统
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021021419112250.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
类加载器子系统负责从文件系统或者网络中加载Class文件，class文
件在文件开头有特定的文件标识。
ClassLoader只负责c1ass文件的加载，至于它是否可以运行，则
由Execution Engine决定。
加载的类信息存放于一块称为方法区的内存空间。除了类的信息
外，方法区中还会存放运行时常量池信息，可能述包括字符串字面量
和数字常量（这部分常量信息是Class文件中常量池部分的内存映射）
```
### 2.1 类的加载过程
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210214192056104.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
public class HelloLoader {

    public static void main(String[] args) {
        System.out.println("谢谢ClassLoader加载我....");
        System.out.println("你的大恩大德，我下辈子再报！");
    }

}

// 它的加载过程是怎么样的呢?
/*  执行 main( ) 方法（静态方法）就需要先加载承载类 HelloLoader
加载成功，则进行链接、初始化等操作，完成后调用 HelloLoader 类中的静态方法 main
加载失败则抛出异常*/

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215101519138.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
完整的流程如下所示：加载 --> 链接（验证 --> 准备 --> 解析） --> 初始化

加载：
1 通过一个类的全限定名获取定义此类的二进制字节流
2 将这个字节流所代表的静存储结构转化为方法区的运行时数据结构
3 在内存中生成一个代表这个类的java.lang.Class对象，作为方
法区这个类的各种数据的访问入口

补充：加载.c1ass文件的方式
从本地系统中直接加载
通过网络获取，典型场景： Web Applet
从zip压缩包中读取，成为日后jar、war格式的基础
运行时计算生成，使用最多的是：动态代理技术
由其他文件生成，典型场景：JSP应用
从专有数据库中提取.class文件，比较少见
从加密文件中获取，典型的防Class文件被反编译的保护措施

链接
链接分为三个子阶段：验证 --> 准备 --> 解析
验证( Verify):
目的在于确保Class文件的字节流中包含信息符合当前虚拟机要求，
保证被加载类的正确性，
不会危害虚拟机自身安全。
主要包括四种验证，文件格式验证，元数据验证，字节码验证，
符号引用验证。

准备( Prepare):
为类变量分配内存并且设置该类变量的默认初始值，即零值。
这里不包含用final修饰的 static,因为final在编译的时候就
会分配了，准备阶段会显式
这里不会为实例变量分配初始化，类变量会分配在方法区中，
而实例变量是会随着对象一起分配到Java堆中。

举例
public class HelloApp {
    private static int a = 1;   //prepare：a = 0 ---> initial : a = 1

    public static void main(String[] args) {
        System.out.println(a);
    }
}
解析：变量a在准备阶段会赋初始值，但不是1，而是0，在
初始化阶段会被赋值为 1



解析( Resolve)
将常量池内的符号引用转换为直接引用的过程
事实上，解析操作住往会伴随着JVM在执行完初始化之后再执行
符号引用就是一组符号来描述所引用的目标。符号引用的字面量
形式明确定义在《java虚拟机
规范》的class文件格式中。直接引用就是直接指向目标的指针、
相对偏移量或一个间接定位到目标的句柄。
解析动作主要针对类或接口、字段、类方法、接口方法、方法
类型等。对应常量池中的
CONSITANT_Class_info、 CONSTANT_Fieldref_info、 CONSTANT_Methodref_into等

初始化
初始化阶段就是执行类构造器方法<clinit>（）的过程。
此方法不需定义，是javac编译器自动收集类中的所有类变量
的赋值动作和静态代码块中的语句合并而来。
构造器方法中指令按语句在源文件中出现的顺序执行
<clinit>（）不同于类的构造器。(关联：构造器是虚拟机视
角下的<init>（）)若该类具有父类，JVM会保证子类
的<clinit>（）执行前，父类的<clinit>（）已经执行完毕。
虚拟机必须保证一个类的<clinit>（）方法在多线程下被
同步加锁。

举例
当我们代码中包含static变量的时候，就会有clinit方法
public class ClassInitTest {
    private static int num = 1;

    static {
        num = 3;
    }
    

    public static void main(String[] args) {
        System.out.println(ClassInitTest.num);
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215102845637.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
如果当前类不存在static变量，那么它的字节码文件是不会
存在<clinit>( )

public class ClinitTest {
    private int a = 1;

    public static void main(String[] args) {
        int b = 2;
    }

}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215102942749.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
<clinit>()方法中的指令按语句在源文件中出现的顺序执行
```
```java
public class ClassInitTest {
    private static int num = 1;

    static {
        num = 3;
        number = 20;
        System.out.println(num);
        //System.out.println(number);    //报错：非法的前向引用（可以赋值，但不能调用）
    }

    //linking之prepare：number = 0 --> initial:20 --> 10
    private static int number = 10;

    public static void main(String[] args) {
        System.out.println(ClassInitTest.num); //3
        System.out.println(ClassInitTest.number); //10
    }
}

```

```markdown
构造器是虚拟机视角下的<init>()
```
```java
public class ClinitTest {
    //任何一个类声明以后，内部至少存在一个类的构造器
    private int a = 1;
    private static int c = 3;

    public static void main(String[] args) {
        int b = 2;
    }

    public ClinitTest(){
        a = 10;
        int d = 20;
    }

}

解析
在构造器中：
先将类变量 a 赋值为 10
再将局部变量d赋值为 20
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215104018897.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
若该类具有父类，JVM会保证子类的<clinit>()执行前，父
类的<clinit>()已经执行完毕
```
```java
public class ClinitTest1 {
    static class Father{
        public static int A = 1;
        static{
            A = 2;
        }
    }

    static class Son extends Father{
        public static int B = A;
    }

    public static void main(String[] args) {
        //加载Father类，其次加载Son类。
        System.out.println(Son.B); //2
    }
}


解析
首先，执行 main( ) 方法需要加载 ClinitTest1 类
获取 Son.B 静态变量，需要加载 Son 类
Son 类的父类是 Father 类，所以需要先执行 Father 类的
加载，再执行 Son 类的加载
```
```markdown
虚拟机必须保证一个类的<clinit>()方法在多线程下被同步加锁
```
```java
public class DeadThreadTest {
    public static void main(String[] args) {
        Runnable r = () -> {
            System.out.println(Thread.currentThread().getName() + "开始");
            DeadThread dead = new DeadThread();
            System.out.println(Thread.currentThread().getName() + "结束");
        };

        Thread t1 = new Thread(r, "线程1");
        Thread t2 = new Thread(r, "线程2");

        t1.start();
        t2.start();
    }
}

class DeadThread {
    static {
        if (true) {
            System.out.println(Thread.currentThread().getName() + "初始化当前类");
            while (true) {

            }
        }
    }
}

执行结果：
线程1开始
线程2开始
线程1初始化当前类

解析
程序卡死，分析原因：
两个线程同时去加载 DeadThread 类，而 DeadThread 类中静态代码块中有一处死循环
先加载 DeadThread 类的线程抢到了同步锁，然后在类的静态代码块中执行死循环，而另一个线程在等待同步锁的释放
所以无论哪个线程先执行 DeadThread 类的加载，另外一个类也不会继续执行


如果改成这样
public class DeadThreadTest {
    public static void main(String[] args) {
        Runnable r = () -> {
            System.out.println(Thread.currentThread().getName() + "开始");
            DeadThread dead = new DeadThread();
            System.out.println(Thread.currentThread().getName() + "结束");
        };

        Thread t1 = new Thread(r, "线程1");
        Thread t2 = new Thread(r, "线程2");

        t1.start();
        t2.start();
    }
}

class DeadThread {
    static {
        if (true) {
            System.out.println(Thread.currentThread().getName() + "初始化当前类");
        }
    }
}

执行结果
线程1开始
线程2开始
线程2初始化当前类
线程2结束
线程1结束
也就是静态代码块只会被执行一次
```

### 2.2 类加载器的分类
```markdown
JVM支持两种类型的类加载器 。分别为引导类加载器
（Bootstrap ClassLoader）和自定义类加载器
（User-Defined ClassLoader）
从概念上来讲，自定义类加载器一般指的是程序中由开发人
员自定义的一类类加载器，但是Java虚拟机规范却没有这
么定义，而是将所有派生于抽象类ClassLoader的类加载器
都划分为自定义类加载器
无论类加载器的类型如何划分，在程序中我们最常见的类
加载器始终只有3个，如下所示
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215112230206.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
为什么 ExtClassLoader 和 AppClassLoader 都属于自定义
加载器

规范定义：所有派生于抽象类ClassLoader的类加载器都划分
为自定义类加载器
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215112351942.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
ExtClassLoader 继承树
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215112449494.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
AppClassLoader 继承树
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215112517943.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
public class ClassLoaderTest {
    public static void main(String[] args) {

        //获取系统类加载器
        ClassLoader systemClassLoader = ClassLoader.getSystemClassLoader();
        System.out.println(systemClassLoader); //sun.misc.Launcher$AppClassLoader@18b4aac2

        //获取其上层：扩展类加载器
        ClassLoader extClassLoader = systemClassLoader.getParent();
        System.out.println(extClassLoader); //sun.misc.Launcher$ExtClassLoader@61bbe9ba

        //获取其上层：获取不到引导类加载器
        ClassLoader bootstrapClassLoader = extClassLoader.getParent();
        System.out.println(bootstrapClassLoader); //null

        //对于用户自定义类来说：默认使用系统类加载器进行加载
        ClassLoader classLoader = ClassLoaderTest.class.getClassLoader();
        System.out.println(classLoader); //sun.misc.Launcher$AppClassLoader@18b4aac2

        //String类使用引导类加载器进行加载的。--> Java的核心类库都是使用引导类加载器进行加载的。
        ClassLoader classLoader1 = String.class.getClassLoader();
        System.out.println(classLoader1); //null

    }
}


解析
我们尝试获取引导类加载器，获取到的值为 null ，这并
不代表引导类加载器不存在，因为引导类加载器是
由 C/C++ 语言构成的，所以我们是获取不到
```
### 2.3 虚拟机自带的加载器
```markdown
1 启动类加载器(引导类加载器)
启动类加载器（ 引导类加载器，Bootstrap ClassLoader ）

这个类加载使用C/C++语言实现的，嵌套在JVM内部

它用来加载Java的核心库
（JAVA_HOME / jre / lib / rt.jar、resources.jar 或 
sun.boot.class.path 路径下的内容），用于提供JVM自身
需要的类,并不继承自java.lang.ClassLoader，没有父加载器

加载扩展类和应用程序类加载器，并作为他们的父类加载器
出于安全考虑，Bootstrap启动类加载器只加载包名为java、
javax、sun等开头的类

2 扩展类加载器
扩展类加载器（Extension ClassLoader）

Java语言编写，由sun.misc.Launcher$ExtClassLoader实现
派生于ClassLoader类
父类加载器为启动类加载器
从java.ext.dirs系统属性所指定的目录中加载类库，
或从JDK的安装目录的 jre / lib / ext子目录（扩展目录）下
加载类库。如果用户创建的 JAR 放在此目录下，也会自
动由扩展类加载器加载


3 系统类加载器
应用程序类加载器（系统类加载器，AppClassLoader）

Java语言编写，由sun.misc.LaunchersAppClassLoader实现
派生于ClassLoader类
父类加载器为扩展类加载器
它负责加载环境变量 classpath 或 系统属性java.class.path指
定路径下的类库
该类加载是程序中默认的类加载器，一般来说，Java应用的类都
是由它来完成加载的
通过classLoader.getSystemclassLoader( )方法可以获取到
该类加载器
```
```java
public class ClassLoaderTest1 {
    public static void main(String[] args) {

        System.out.println("**********启动类加载器**************");
        //获取BootstrapClassLoader能够加载的api的路径
        URL[] urLs = sun.misc.Launcher.getBootstrapClassPath().getURLs();
        for (URL element : urLs) {
            System.out.println(element.toExternalForm());
        }
        //从上面的路径中随意选择一个类,来看看他的类加载器是什么:引导类加载器
        ClassLoader classLoader = Provider.class.getClassLoader();
        System.out.println(classLoader); //null

        System.out.println("***********扩展类加载器*************");
        String extDirs = System.getProperty("java.ext.dirs");
        for (String path : extDirs.split(";")) {
            System.out.println(path);
        }

        //从上面的路径中随意选择一个类,来看看他的类加载器是什么:扩展类加载器
        ClassLoader classLoader1 = CurveDB.class.getClassLoader();
        System.out.println(classLoader1);//sun.misc.Launcher$ExtClassLoader@1540e19d

    }
}
```
### 2.4  用户自定义类加载器
```markdown
为什么需要自定义类加载器？

在Java的日常应用程序开发中，类的加载几乎是由上述3种
类加载器相互配合执行的，在必要时，我们还可以自定义
类加载器，来定制类的加载方式。

那为什么还需要自定义类加载器？
隔离加载类
修改类加载的方式
扩展加载源
防止源码泄露

如何自定义类加载器？
开发人员可以通过继承抽象类java.lang.ClassLoader类的方式，
实现自己的类加载器，以满足一些特殊的需求
在JDK1.2之前，在自定义类加载器时，总会去继承ClassLoader类
并重写loadClass( )方法，从而实现自定义的类加载类，但是
在JDK1.2之后已不再建议用户去覆盖loadClass( )方法，而是建
议把自定义的类加载逻辑写在findclass( )方法中
在编写自定义类加载器时，如果没有太过于复杂的需求，可
以直接继承URIClassLoader类，这样就可以避免自己去编
写findclass( )方法及其获取字节码流的方式，使自定义类加载
器编写更加简洁。

例子
public class CustomClassLoader extends ClassLoader {
    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {

        try {
            byte[] result = getClassFromCustomPath(name);
            if (result == null) {
                throw new FileNotFoundException();
            } else {
                return defineClass(name, result, 0, result.length);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }

        throw new ClassNotFoundException(name);
    }

    private byte[] getClassFromCustomPath(String name) {
        //从自定义路径中加载指定类:细节略
        //如果指定路径的字节码文件进行了加密，则需要在此方法中进行解密操作。
        return null;
    }

    public static void main(String[] args) {
        CustomClassLoader customClassLoader = new CustomClassLoader();
        try {
            Class<?> clazz = Class.forName("One", true, customClassLoader);
            Object obj = clazz.newInstance();
            System.out.println(obj.getClass().getClassLoader());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
### 2.5 关于 ClassLoader
```markdown
ClassLoader类，它是一个抽象类，其后所有的类加载器都继
承自ClassLoader（不包括启动类加载器）
```

|  方法名称| 描述 |
|--|--|
| getParent( ) | 返回该类加载器的超类加载器 |
|  loadClass(String name)| 加载名称为name的类，返回结果为java.lang.Class类的实例 |
| findClass(String name) | 查找名称为name的类，返回结果为java.lang.Class类的实例 |
| findLoadedClass(String name) | 查找名称为name的已经被加载过的类，返回结果为java.lang.Class类的实例|
| defineClass(String name,byte[ ] b,int len) |  把字节数组b中的内容转换为一个Java类，返回结果为java.lang.Class类的实例|
|  resolveClass(Class<?> c)|  连接指定的一个Java类|


```markdown
sun.misc.Launcher 它是一个java虚拟机的入口应用
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215120802656.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 2.5.1 获取 ClassLoader 的途径
```markdown
方式一：获取当前类的ClassLoader
clazz. getClassLoader()
方式二：获取当前线程上下文的ClassLoader
Thread.currentThread().getContextClassLoader()
方式三：获取系统的ClassLoader
ClassLoader. getSystemClassLoader()
方式四：获取调用者ClassLoader
Drivermanager. getCallerClassLoader()
```
```java
public class ClassLoaderTest2 {
    public static void main(String[] args) {
        try {

            //1.Class.forName().getClassLoader()
            ClassLoader classLoader = Class.forName("java.lang.String").getClassLoader();
            System.out.println(classLoader); // String 类由启动类加载器加载，我们无法获取

            //2.Thread.currentThread().getContextClassLoader()
            ClassLoader classLoader1 = Thread.currentThread().getContextClassLoader();
            System.out.println(classLoader1); //sun.misc.Launcher$AppClassLoader@18b4aac2

            //3.ClassLoader.getSystemClassLoader().getParent()
            ClassLoader classLoader2 = ClassLoader.getSystemClassLoader().getParent();
            System.out.println(classLoader2); //sun.misc.Launcher$ExtClassLoader@61bbe9ba

        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```
### 2.6 双亲委派机制(面试常问)
#### 2.6.1 双亲委派机制原理
```markdown
Java虚拟机对 class 文件采用的是按需加载的方式，也就是
说当需要使用该类时才会将它的 class 文件加载到内存中生
成 class 对象。而且加载某个类的class文件时，Java虚拟
机采用的是双亲委派模式，即把请求交由父类处理，它是
一种任务委派模式
如果一个类加载器收到了类加载请求，它并不会自己先
去加载，而是把这个请求委托给父类的加载器去执行；
如果父类加载器还存在其父类加载器，则进一步向上
委托，依次递归，请求最终将到达顶层的启动类加载器；
如果父类加载器可以完成类加载任务，就成功返回，倘若
父类加载器无法完成此加载任务，子加载器才会尝试自
己去加载，这就是双亲委派模式。
父类加载器一层一层往下分配任务，如果子类加载器能
加载，则加载此类，如果将加载任务分配至系统类加
载器也无法加载此类，则抛出异常
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215132315706.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

#### 2.6.2 双亲委派机制代码示例
```java
//我们自己定义一个java.lang包，在其下面定义一个String类，里面声明了静态代码块
package java.lang;

public class String {

    static {
        System.out.println("我是自定义的String类的静态代码块");
    }

}

//在一个测试类中加载String类，看看加载的String类是JDK自带的，还是我们自己编写的

public class StringTest {
    public static void main(String[] args) {
        String str = new java.lang.String();
        System.out.println("你好，世界");
    }
}

结果：程序并没有输出我们静态代码块中的内容，可见仍然加载的是 JDK 自带的 String 类
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215132611753.png)
```java
//在我们自己定义的 String 类中整个 main( ) 方法
public class String {

    static {
        System.out.println("我是自定义的String类的静态代码块");
    }

    //错误: 在类 java.lang.String 中找不到 main 方法
    public static void main(String[] args) {
        System.out.println("hello,String");
    }

}

解析
原因：由于双亲委派机制，我们的String类是由引导
类加载器加载的，而引导类加载器并没有main方法，所以会报错
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215132756580.png)
```java
//SPI接口是由引导类加载器加载的，接口具体的实现类是由线程上下文类加载器加载的，而线程上下文类加载器就是系统类加载器，所以我们在加载的时候，会先进行双亲委派，在引导类加载器加载SPI核心类，然后加载SPI接口，最后在反向委托，通过系统类加载器进行实现类 jdbc.jar 的加载
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215133026382.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
//出于保护机制，java.lang 包下不允许我们自定义类
package java.lang;

public class ShkStart {
    public static void main(String[] args) {
        System.out.println("hello!");
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215133228701.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 2.6.3 双亲委派机制的优势
```markdown
避免类的重复加载
保护程序安全，防止核心API被随意篡改
自定义类：java.lang.String 没有调用
自定义类：java.lang.ShkStart（报错：阻止创
建 java.lang开头的类）
```
#### 2.6.4 沙箱安全机制
```markdown
自定义String类时：在加载自定义String类的时候会率
先使用引导类加载器加载，而引导类加载器在加载的过
程中会先加载jdk自带的文件
（rt.jar包中java.lang.String.class），
报错信息说没有main方法，就是因为加载的是rt.jar包
中的String类。这样可以保证对java核心源代码的保护，
这就是沙箱安全机制。
```
#### 2.6.5 其他
```markdown
如何判断两个class对象是否相同？

在JVM中表示两个class对象是否为同一个类存在两个必要条件：
类的完整类名必须一致，包括包名
加载这个类的 ClassLoader（指ClassLoader实例对象）必须相同
换句话说，在JVM中，即使这两个类对象（class对象）来源同
一个Class文件，被同一个虚拟机所加载，但只要加载它
们的 ClassLoader 实例对象不同，那么这两个类对象也是不相等的

对类加载器的引用
JVM必须知道一个类型是由启动加载器加载的还是由用户类加
载器加载的如果一个类型是由用户类加载器加载的，那么JVM
会将这个类加载器的一个引用作为类型信息的一部分保存在方
法区中。当解析一个类型到另一个类型的引用的时候，JVM需
要保证这两个类型的类加载器是相同的

类的主动使用和被动使用
Java程序对类的使用方式分为：主动使用 和 被动使用。

主动使用，又分为七种情况：

创建类的实例
访问某个类或接口的静态变量，或者对该静态变量赋值
调用类的静态方法
反射（ 比如：Class.forName(“cn.sxt.Test”) )
初始化一个类的子类
Java虚拟机启动时被标明为启动类的类
JDK7开始提供的动态语言支持：
java.lang.invoke.MethodHandle实例的解析结
果REF_getStatic、REF putStatic、REF_invokeStatic句
柄对应的类没有初始化，则初始化
除了以上七种情况，其他使用Java类的方式都被看作是对类的
被动使用，都不会导致类的初始化，即不会执行初始化阶
段（不会调用 clinit( ) 方法和 init( ) 方法）
```
## 3 运行时数据区
### 3.1 前言
```markdown
运行时数据区，也就是下图这部分，它是在类加载完成后的阶段
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215143115722.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
当我们通过前面的：
类的加载 --> 验证 --> 准备 --> 解析 --> 初始化，
这几个阶段完成后，就会用到执行引擎对我们的
类进行使用，同时执行引擎将会使用到我们的运行时数据区
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215143247272.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
比如大厨做饭，我们把大厨后面的东西（切好的菜，刀，调料），
比作是运行时数据区。而厨师可以类比于执行引擎，将通过
准备的东西制作成精美的菜品
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215143329243.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 3.2 运行时数据区结构
#### 3.2.1 运行时数据区与内存
```markdown
内存
内存是非常重要的系统资源，是硬盘和CPU的中间仓库及桥梁，
承载着操作系统和应用程序的实时运行。JVM内存布局规定
了Java在运行过程中内存申请、分配、管理的策略，保证
了JVM的高效稳定运行。
不同的JVM对于内存的划分方式和管理机制存在着部分差
异。结合JVM虚拟机规范，来探讨一下经典的JVM内存布局。
我们通过磁盘或者网络IO得到的数据，都需要先加载到内
存中，然后CPU从内存中获取数据进行读取，也就是说
内存充当了CPU和磁盘之间的桥梁
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215143633499.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021021514372135.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
方法区在JVM规范中是一个逻辑概念，由虚拟机自己进行
具体实现，jdk7和以前的版本使用的是堆上的永久代实
现的方法区，而在jdk8及以后使用的是元数据区实现方法区
```
#### 3.2.2 线程的内存空间
```markdown
Java虚拟机定义了若干种程序运行期间会使用到的运行时数据区，
其中有一些会随着虚拟机启动而创建，随着虚拟机的退出而销
毁。另外一些则是与线程一一对应的，这些与线程对应的数
据区域会随着线程开始和结束而创建和销毁。

灰色的为单独线程私有的，红色的为多个线程共享的。即：

线程独有：独立包括程序计数器、栈、本地方法栈
线程间共享：堆、堆外内存（永久代或元空间、代码缓存）
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215143946204.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
关于线程间共享的说明
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215144037754.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
每个JVM只有一个Runtime实例。即为运行时环境，相
当于上面那张图
```
#### 3.2.3 线程
##### 3.2.3.1 JVM线程
```markdown
线程是一个程序里的运行单元。JVM允许一个应用有多
个线程并行的执行
在Hotspot JVM里，每个线程都与操作系统的本地线程
直接映射
当一个Java线程准备好执行以后，此时一个操作系统的
本地线程也同时创建。Java线程执行终止后，本地线程也会回收
操作系统负责将线程安排调度到任何一个可用的CPU上。一
旦本地线程初始化成功，它就会调用Java线程中的run( )方法
如果一个线程抛异常，并且该线程是进程中最后一个守护线
程，那么进程将停止
```
##### 3.2.3.2 JVM系统线程
```markdown
如果你使用 jconsole 或者是任何一个调试工具，都能看到在
后台有许多线程在运行。
这些后台线程不包括调用
public static void main(String [ ])的main线程以及所有
由这个main方法自己创建的线程。

这些主要的后台系统线程在Hotspot JVM里主要是以下几个：

虚拟机线程：这种线程的操作是需要JVM达到安全点才会出现。
这些操作必须在不同的线程中发生的原因是他们都需要JVM达
到安全点，这样堆才不会变化。这种线程的执行类
型括"stop-the-world"的垃圾收集，线程栈收集，线程挂起以及
偏向锁撤销
周期任务线程：这种线程是时间周期事件的体现（比如中断），
他们一般用于周期性操作的调度执行
GC线程：这种线程对在JVM里不同种类的垃圾收集行为提供
了支持（重点）
编译线程：这种线程在运行时会将字节码编译成 本地代码
信号调度线程：这种线程接收信号并发送给JVM，在它内部
通过调用适当的方法进行处理
```
#### 3.2.4 程序计数器
##### 3.2.4.1 PC 寄存器介绍
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215150928621.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
JVM中的程序计数寄存器（Program Counter Register），
Register的命名源于CPU的寄存器，寄存器存储指令相关的现
场信息。CPU只有把数据装载到寄存器才能够运行。
这里，并非是广义上所指的物理寄存器，或许将其翻译为PC计数
器（或指令计数器）会更加贴切（也称为程序钩子），并且也不容
易引起一些不必要的误会。JVM中的PC寄存器是对物理PC寄存器
的一种抽象模拟。
它是一块很小的内存空间，几乎可以忽略不记。也是运行速度最
快的存储区域。
在JVM规范中，每个线程都有它自己的程序计数器，是线程私有
的，生命周期与线程的生命周期保持一致。
任何时间一个线程都只有一个方法在执行，也就是所谓的当前方法。
程序计数器会存储当前线程正在执行的Java方法的JVM指令地址；或
者，如果是在执行native方法，则是未指定值（undefined）。
它是程序控制流的指示器，分支、循环、跳转、异常处理、线程恢复
等基础功能都需要依赖这个计数器来完成
字节码解释器工作时就是通过改变这个计数器的值来选取下一条
需要执行的字节码指令。
它是唯一一个在Java虚拟机规范中没有规定任何
OutofMemoryError情况的区域。
```
##### 3.2.4.2 PC 寄存器的作用
```markdown
PC寄存器用来存储指向下一条指令的地址，也就是即将要执
行的指令代码。由执行引擎读取下一条指令，并执行该指令。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215151203191.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
public class PCRegisterTest {

    public static void main(String[] args) {
        int i = 10;
        int j = 20;
        int k = i + j;

        String s = "abc";
        System.out.println(i);
        System.out.println(k);

    }
}
```
```markdown
使用反编译：javap -v PCRegisterTest.class

左边的数字代表指令地址 (偏移地址)，即 PC 寄存器
中可能存储的值，然后执行引擎读取 PC 寄存器中的值，并执行该指令
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215151424145.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021021515152930.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 3.2.4.3 两个常见面试题
```markdown
使用 PC寄存器 存储字节码指令地址有什么用呢？

或者问
为什么使用 PC寄存器 来记录当前线程的执行地址呢？

因为线程是一个个的顺序执行流，CPU需要不停的切换各个
线程，这时候切换回来以后，就得知道接着从哪开始继续执行
JVM的字节码解释器就需要通过改变PC寄存器的值来明确下一
条应该执行什么样的字节码指令
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215155102284.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
PC寄存器为什么被设定为私有的？
我们都知道所谓的多线程在一个特定的时间段内只会执行其
中某一个线程的方法，CPU会不停地做任务切换，这样必然导
致经常中断或恢复，如何保证分毫无差呢？
为了能够准确地记录各个线程正在执行的当前字节码指令地址，最
好的办法自然是为每一个线程都分配一个PC寄存器，这样一来各
个线程之间便可以进行独立计算，从而不会出现相互干扰的情况。
由于CPU时间片轮限制，众多线程在并发执行过程中，任何一个
确定的时刻，一个处理器或者多核处理器中的一个内核，只会执
行某个线程中的一条指令。
这样必然导致经常中断或恢复，如何保证分毫无差呢？每个线
程在创建后，都会产生自己的程序计数器和栈帧，程序计
数器在各个线程之间互不影响。

CPU时间片
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215155254450.png)
```markdown
CPU时间片即CPU分配给各个程序的时间，每个线程被分配一
个时间段，称作它的时间片。
在宏观上：我们可以同时打开多个应用程序，每个程序并行
不悖，同时运行。
但在微观上：由于只有一个CPU，一次只能处理程序要求的
一部分，如何处理公平，一种方法就是引入时间片，每个程序
轮流执行。
简单一句话：宏观并行，微观并发
```
#### 3.2.5 虚拟机栈
##### 3.2.5.1 虚拟机栈
```markdown
由于跨平台性的设计，Java的指令都是根据栈来设计的。不
同平台CPU架构不同，所以不能设计为基于寄存器的。
优点是跨平台，指令集小，编译器容易实现，缺点是性能下
降，实现同样的功能需要更多的指令。
```
##### 3.2.5.2 内存中的栈与堆
```markdown
首先栈是运行时的单位，而堆是存储的单位

栈解决程序的运行问题，即程序如何执行，或者说如何处理数据。
堆解决的是数据存储的问题，即数据怎么放，放哪里
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215162151702.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 3.2.5.3 虚拟机栈的基本内容
```markdown
Java虚拟机栈是什么？

Java虚拟机栈（Java Virtual Machine Stack），
早期也叫Java栈。每个线程在创建时都会创建一个虚
拟机栈，其内部保存一个个的栈帧（Stack Frame），
对应着一次次的Java方法调用

栈是线程私有的
一个方法对应一个栈帧的入栈和出栈
```
```java
public class StackTest {

    public static void main(String[] args) {
        StackTest test = new StackTest();
        test.methodA();
    }

    public void methodA() {
        int i = 10;
        int j = 20;

        methodB();
    }

    public void methodB() {
        int k = 30;
        int m = 40;
    }

}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021021516233345.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
生命周期

生命周期和线程一致，也就是线程结束了，该虚拟机栈也销毁了

作用

主管Java程序的运行，它保存方法的局部变量（8 种基本数据类
型、对象的引用地址）、部分结果，并参与方法的调用和返回。

局部变量 VS 成员变量（属性）
基本数据类型变量 VS 引用类型变量（类、数组、接口）
```
##### 3.2.5.4 虚拟机栈的特点
```markdown
栈是一种快速有效的分配存储方式，访问速度仅次于程序计数器。

JVM直接对Java栈的操作只有两个：
每个方法执行，伴随着进栈（入栈、压栈）
执行结束后的出栈工作
对于栈来说不存在垃圾回收 (GC) 问题（栈存在溢出的情况）
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215173018872.png)
##### 3.2.5.5 虚拟机栈的异常
```markdown
栈中可能出现的异常
Java 虚拟机规范允许Java栈的大小是动态的或者是固定不变的。
如果采用固定大小的Java虚拟机栈，那每一个线程的Java虚拟机
栈容量可以在线程创建的时候独立选定。
如果线程请求分配的栈容量超过Java虚拟机栈允许的最大容量，
Java虚拟机将会抛出一个StackoverflowError 异常。简称：
栈溢出。如果Java虚拟机栈可以动态扩展，并且在尝试扩展
的时候无法申请到足够的内存，或者在创建新的线程时没有足够的
内存去创建对应的虚拟机栈，那Java虚拟机将会抛出一个 
OutOfMemoryError 异常。

```
```java
// 栈异常演示
public class StackError {

    public static void main(String[] args) {
        main(args);
    }

}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021021517324583.png)
##### 3.2.5.6 设置栈内存大小
```java
我们可以使用参数 -Xss选项来设置线程的最大栈空间，栈的大小直接决定了函数调用的最大可达深度。
-Xss1024m		// 栈内存为 1024MB
-Xss1024k		// 栈内存为 1024KB

设置线程的最大栈空间：256KB
public class StackError {

    private static int count = 1;

    public static void main(String[] args) {
        System.out.println(count);
        count++;
        main(args);
    }

}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021021517362078.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021021517371774.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 3.2.5.7 栈的存储单位
```java
栈中存储什么？

每个线程都有自己的栈，栈中的数据都是以栈帧（Stack Frame）为
基本单位存储的
在这个线程上正在执行的每个方法都各自对应一个栈帧
（Stack Frame）。一个方法的执行对应一个栈帧的入
栈，一个方法的执行结束对应一个栈帧的出栈
栈帧是一个内存区块，是一个数据集，维系着方法执行
过程中的各种数据信息。
```
##### 3.2.5.8 栈的运行原理

```markdown
JVM直接对Java栈的操作只有两个，就是对栈帧的压栈和出栈，
遵循先进后出（后进先出）原则
在一条活动线程中，一个时间点上，只会有一个活动的栈帧。
即只有当前正在执行的方法的栈帧（栈顶栈帧）是有效的，
这个栈帧被称为当前栈帧（Current Frame）
与当前栈帧相对应的方法就是当前方法（Current Method）
定义这个方法的类就是当前类（Current Class）
执行引擎运行的所有字节码指令只针对当前栈帧进行操作。
如果在该方法中调用了其他方法，对应的新的栈帧会被创建出来，
放在栈的顶端，成为新的当前帧。
不同线程中所包含的栈帧是不允许存在相互引用的，即不可能
在一个栈帧之中引用另外一个线程的栈帧。
如果当前方法调用了其他方法，方法返回之际，当前栈帧会传
回此方法的执行结果给前一个栈帧，接着，虚拟机会丢弃当
前栈帧，使得前一个栈帧重新成为当前栈帧。
Java方法有两种返回函数的方式，但不管使用哪种方式，都
会导致栈帧被弹出。
一种是正常的函数返回，使用 return 指令,另外一种是抛出异常
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215173919689.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
public class StackFrameTest {
    public static void main(String[] args) {
        StackFrameTest test = new StackFrameTest();
        test.method1();
    }

    public void method1() {
        System.out.println("method1()开始执行...");
        method2();
        System.out.println("method1()执行结束...");
    }

    public int method2() {
        System.out.println("method2()开始执行...");
        int i = 10;
        int m = (int)method3();
        System.out.println("method2()即将结束...");
        return i + m;
    }

    public double method3() {
        System.out.println("method3()开始执行...");
        double j = 20.0;
        System.out.println("method3()即将结束...");
        return j;
    }
}

执行结果
method1()开始执行...
method2()开始执行...
method3()开始执行...
method3()即将结束...
method2()即将结束...
method1()执行结束...
```
##### 3.2.5.9 栈帧的内部结构
```markdown
每个栈帧中存储着：
局部变量表（Local Variables）
操作数栈（Operand Stack）（或表达式栈）
动态链接（Dynamic Linking）（或指向运行时常量池的方法引用）
方法返回地址（Return Address）（或方法正常退出或者
异常退出的定义）一些附加信息
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215174113404.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
每个线程下的栈都是私有的，因此每个线程都有自己各自的栈，
并且每个栈里面都有很多栈帧，栈帧的大小主要由局部变量
表 和 操作数栈决定的
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215174151178.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 3.2.5.10 局部变量表
###### 3.2.5.10.1 局部变量表介绍
```markdown
局部变量表：Local Variables，也被称之为局部变量数组或本地
变量表
定义为一个数字数组，主要用于存储方法参数和定义在方法体
内的局部变量，这些数据类型包括各类基本数据类型、对象引
用（reference），以及 returnAddress(返回值) 类型。
由于局部变量表是建立在线程的栈上，是线程的私有数据，因此
不存在数据安全问题
局部变量表所需的容量大小是在编译期确定下来的，并保存在
方法的Code属性的maximum local variables数据项中。在方法运
行期间是不会改变局部变量表的大小的。
方法嵌套调用的次数由栈的大小决定。一般来说，栈越大，方
法嵌套调用次数越多。
对一个函数而言，它的参数和局部变量越多，使得局部变量表
膨胀，它的栈帧就越大，以满足方法调用所需传递的信息增大的需求。
进而函数调用就会占用更多的栈空间，导致其嵌套调用次数就会减少。
局部变量表中的变量只在当前方法调用中有效。
在方法执行时，虚拟机通过使用局部变量表完成参数值到参数变量
列表的传递过程。
当方法调用结束后，随着方法栈帧的销毁，局部变量表也会随之销毁。
```
```markdown
举例说明：局部变量表所需的容量大小是在编译期确定下来的
```
```java
public class LocalVariablesTest {
    private int count = 0;

    public static void main(String[] args) {
        LocalVariablesTest test = new LocalVariablesTest();
        int num = 10;
        test.test1();
    }

    public void test1() {
        Date date = new Date();
        String name1 = "baidu.com";
        String info = test2(date, name1);
        System.out.println(date + name1);
    }

    public String test2(Date dateP, String name2) {
        dateP = null;
        name2 = "xiexu";
        double weight = 185.5;//占据两个slot
        char gender = '男';
        return dateP + name2;
    }

}

解析
反编译后，可得结论：

在编译期间，局部变量的个数、每个局部变量的大小都已经被记录下来
所以局部变量表所需的容量大小是在编译期确定下来的
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215190443899.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
利用 JClassLib 也可以查看局部变量的个数
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215190515219.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215190538979.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215190623432.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/202102151906462.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215190709644.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
###### 3.2.5.10.2 关于 Slot 的理解
```markdown
参数值的存放总是从局部变量数组索引 0 的位置开始，到数
组长度-1的索引结束。
局部变量表，最基本的存储单元是Slot（变量槽）
局部变量表中存放编译期可知的各种基本数据类型（8种），引
用类型（reference），returnAddress类型的变量。
在局部变量表里，32位以内的类型只占用一个
slot（包括 引用类型、returnAddress类型），
64位的类型（long和double）占用两个slot。
byte、short、char 在存储前被转换为int，boolean 
也被转换为int，0 表示false，非0 表示true
long 和 double 则占据两个Slot
JVM会为局部变量表中的每一个Slot都分配一个访问索引，
通过这个索引即可成功访问到局部变量表中指定的局部变量值
当一个实例方法被调用的时候，它的方法参数和方法体内部定
义的局部变量将会按照顺序被复制到局部变量表中的每一个slot上
如果需要访问局部变量表中一个64bit的局部变量值时，只需
要使用前一个索引即可。（比如：访问long或double类型变量）
如果当前帧是由构造方法或者实例方法(非静态方法) 创建的，
那么该对象引用this 将会存放在index为0 的slot处，
其余的参数按照参数表顺序继续排列。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215191059445.png)
```markdown
在构造器以及实例方法中，对象引用this 都会存放在
索引为0的位置
```
```java
//构造器
public LocalVariablesTest() {
    this.count = 1;
}

//实例方法
public void test1() {
    Date date = new Date();
    String name1 = "baidu.com";
    test2(date, name1);
    System.out.println(date + name1);
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215191232770.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021021519125354.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
64位的类型（long和double）占用两个slot
```
```java
public String test2(Date dateP, String name2) {
        dateP = null;
        name2 = "xiexu";
        double weight = 185.5; //占据两个slot
        char gender = '男';
        return dateP + name2;
}
```
```markdown
可以看到，weight为double类型，索引从3直接跳到5，说明
double占据两个slot
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215191428794.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
static方法无法调用this
```
```java
public static void testStatic() {
        LocalVariablesTest test = new LocalVariablesTest();
        Date date = new Date();
        int count = 10;
        System.out.println(count);
        //因为this变量不存在于该静态方法的局部变量表中！！！
//        System.out.println(this.count);
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215191538369.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
###### 3.2.5.10.3 Slot 的重复利用
```java
栈帧中的局部变量表中的槽位是可以重用的，如果一个
局部变量出了其作用域，那么在其作用域之后声明新的局
部变量就很有可能会复用过期局部变量的槽位，从而达
到节省资源的目的。
```
```java
public void test4() {
        int a = 0;
        {
            int b = 0;
            b = a + 1;
        }
        //变量c使用 之前已经销毁的变量b占据的slot的位置
        int c = a + 1;
}

解析
可以看到，局部变量c 重用了 局部变量b 的slot位置
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215191732242.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
###### 3.2.5.10.4 静态变量与局部变量的对比
```markdown
参数表分配完毕之后，再根据方法体内定义的变量的顺序和作用域分配
我们知道类变量表有两次初始化的机会，第一次是在“准备阶段”，执
行系统初
始化，对类变量设置零值，另一次则是在“初始化”阶段，赋予程序员
在代码中定义的初始值。
和类变量初始化不同的是，局部变量表不存在系统初始化的过程，
这意味着一旦定义了局部变量则必须人为的初始化，否则无法使用。
public void test（）{
	int i;
	System. out. println(i);
}
这样的代码是错误的，没有赋值不能够使用。
```
###### 3.2.5.10.5 补充说明
```markdown
在栈帧中，与性能调优关系最为密切的部分就是前面提到的
局部变量表。
在方法执行时，虚拟机使用局部变量表完成方法的传递。
局部变量表中的变量也是重要的垃圾回收根节点，只要被
局部变量表中直接或间接引用的对象都不会被回收。
```
##### 3.2.5.11 操作数栈(Operand Stack)
###### 3.2.5.11.1 操作数栈的特点
```markdown
每一个独立的栈帧除了包含局部变量表以外，还包含一个后进
先出（Last - In - First -Out）的 操作数栈，也可以称之为表
达式栈（Expression Stack）
操作数栈，在方法执行过程中，根据字节码指令，往栈中写
入数据或提取数据，即入栈（push）和 出栈（pop）
某些字节码指令将值压入操作数栈，其余的字节码指令将操作
数取出栈。使用它们后再把结果压入栈，比如：执行复制、
交换、求和等操作
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215211008401.png)
###### 3.2.5.11.2 操作数栈的作用
```markdown
操作数栈，主要用于保存计算过程的中间结果，同时作为计算过
程中变量临时的存储空间。
操作数栈就是JVM执行引擎的一个工作区，当一个方法刚开始
执行的时候，一个新的栈帧也会随之被创建出来，这个时候
方法的操作数栈是空的（这个时候数组是创建好并且是长
度固定的，但数组的内容为空）
每一个操作数栈都会拥有一个明确的栈深度用于存储数值，
其所需的最大深度在编译期就定义好了，保存在方法的
Code属性中，为maxstack的值。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215213456934.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
栈中的任何一个元素都是可以任意的Java数据类型
32bit的类型占用一个栈单位深度
64bit的类型占用两个栈单位深度
操作数栈并非采用访问索引的方式来进行数据访问的，而是只
能通过标准的入栈和出栈操作来完成一次数据访问
如果被调用的方法带有返回值的话，其返回值将会被压入
当前栈帧的操作数栈中，并更新PC寄存器中下一条需要执行的
字节码指令。
操作数栈中元素的数据类型必须与字节码指令的序列严格匹配，这
由编译器在编译器期间进行验证，同时在类加载过程中的类检
验阶段的数据流分析阶段要再次验证。
另外，我们说Java虚拟机的解释引擎是基于栈的执行引擎，其中
的栈指的就是操作数栈。
```
```java
代码追踪
public void testAddOperation() {
        //byte、short、char、boolean：都以int型来保存
        byte i = 15;
        int j = 8;
        int k = i + j;
}

 0 bipush 15
 2 istore_1
 3 bipush 8
 5 istore_2
 6 iload_1
 7 iload_2
 8 iadd
 9 istore_3
10 return

```
```markdown
程序执行流程

首先执行第一条语句，PC寄存器指向的是0，也就是指
令地址为0，然后使用bipush让操作数15入操作数栈。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215213821408.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
执行完后，让PC寄存器 + 1，指向下一行代码，下一行代码就是
将操作数栈的元素存储到局部变量表索引1的位置，我们可以看到
局部变量表的已经增加了一个元素
解释为什么局部变量表索引从 1 开始，因为该方法为实例方法，局
部变量表索引为 0 的位置存放的是 this
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215213959476.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
然后PC寄存器+1，指向的是下一行。让操作数8也入栈，同
时执行 istore 操作，存入局部变量表中
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215214035559.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
然后从局部变量表中，依次将数据取出放在操作数栈中，等
待执行 add 操作
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215214118769.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
将操作数栈的两个元素出栈，执行iadd操作
这里的 iadd 操作具体是：执行引擎将字节码指令翻译成机
器指令，然后被CPU进行运算，得出结果，重新放入操作数栈中
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215214212114.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
然后执行 istore 操作，将操作数23 存储到局部变量表索
引为3的位置
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215214235205.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
关于 int j =8; 的说明

我们反编译得到的字节码指令如下
因为 8 可以存放在 byte 类型中，所以压入操作数栈的类
型为 byte ，而不是 int ，所以执行的字节码指令为 bipush 8
然后将数值 8 转换为int类型存储在局部变量表中：istore_2
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215214420550.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```java
如果被调用的方法带有返回值的话，其返回值将会被压入当
前栈帧的操作数栈中
	public int getSum() {
	        int m = 10;
	        int n = 20;
	        int k = m + n;
	        return k;
	    }

    public void testGetSum() {
        //获取上一个栈桢返回的结果，并保存在操作数栈中
        int i = getSum();
        int j = 10;
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215214545776.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021021521465480.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 3.2.5.12 栈顶缓存技术(Top Of Stack Cashing)
```markdown
前面提过，基于栈式架构的虚拟机所使用的零地址指令更加
紧凑，但完成一项操作的时候必然需要使用更多的入栈和出栈
指令，这同时也就意味着将需要更多的指令
分派（instruction dispatch）次数和内存读/写次数。
由于操作数是存储在内存中的，因此频繁地执行内存读/写操作必然
会影响执行速度。为了解决这个问题，HotSpot JVM的设计者
们提出了栈顶缓存（Tos，Top-of-Stack Cashing）技术，
将栈顶元素全部缓存在物理CPU的寄存器中，以此降低对内
存的读/写次数，提升执行引擎的执行效率。
寄存器的主要优点：指令更少，执行速度快
```
##### 3.2.5.13 动态链接(Dynamic Linking)
```markdown
动态链接（或指向运行时常量池的方法引用）
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215221452881.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
每一个栈帧内部都包含一个指向运行时常量池中该栈帧所属方
法的引用
包含这个引用的目的就是为了支持当前方法的代码能够实现动态
链接（Dynamic Linking），比如：invokedynamic指令
在Java源文件被编译到字节码文件中时，所有的变量和方法引
用都作为符号引用（Symbolic Reference）保存在class文件的
常量池里
比如：描述一个方法调用了另外的其他方法时，就是通过常量池
中指向方法的符号引用来表示的，那么动态链接的作用就是为
了将这些符号引用转换为调用方法的直接引用
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215221738756.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
在Java源文件被编译到字节码文件中时，所有的变量和方法引用都
作为符号引用（Symbolic Reference）保存在class文件
的常量池里
```
```java
public class DynamicLinkingTest {

    int num = 10;

    public void methodA(){
        System.out.println("methodA()....");
    }

    public void methodB(){
        System.out.println("methodB()....");
        methodA();
        num++;
    }

}

在字节码指令中，methodB( ) 方法中通过 invokevirtual #7 指令调用了方法 A
那么 #7 是什么呢？
public void methodB();
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=3, locals=1, args_size=1
         0: getstatic     #3                  // Field java/lang/System.out:Ljava/io/PrintStream;
         3: ldc           #6                  // String methodB()....
         5: invokevirtual #5                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
         8: aload_0
         9: invokevirtual #7                  // Method methodA:()V
        12: aload_0
        13: dup
        14: getfield      #2                  // Field num:I
        17: iconst_1
        18: iadd
        19: putfield      #2                  // Field num:I
        22: return
      LineNumberTable:
        line 12: 0
        line 13: 8
        line 14: 12
        line 15: 22
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      23     0  this   Lcn/sxt/java1/DynamicLinkingTest;

往上面翻，找到常量池的定义：#7 = Methodref #8.#31
先找 #8 ：
#8 = Class #32 ：去找 #32
#32 = Utf8 cn/sxt/java1/DynamicLinkingTest
结论：通过 #8 我们找到了 DynamicLinkingTest 这个类
再来找 #31：
#31 = NameAndType #19:#13 ：去找 #19 和 #13
#19 = Utf8 methodA ：方法名为 methodA
#13 = Utf8 ()V ：方法没有形参，返回值为 void
结论：通过 #7 我们就能找到需要调用的 methodA( ) 方法，并进行调用

Constant pool:
   #1 = Methodref          #9.#23         // java/lang/Object."<init>":()V
   #2 = Fieldref           #8.#24         // cn/sxt/java1/DynamicLinkingTest.num:I
   #3 = Fieldref           #25.#26        // java/lang/System.out:Ljava/io/PrintStream;
   #4 = String             #27            // methodA()....
   #5 = Methodref          #28.#29        // java/io/PrintStream.println:(Ljava/lang/String;)V
   #6 = String             #30            // methodB()....
   #7 = Methodref          #8.#31         // cn/sxt/java1/DynamicLinkingTest.methodA:()V
   #8 = Class              #32            // cn/sxt/java1/DynamicLinkingTest
   #9 = Class              #33            // java/lang/Object
  #10 = Utf8               num
  #11 = Utf8               I
  #12 = Utf8               <init>
  #13 = Utf8               ()V
  #14 = Utf8               Code
  #15 = Utf8               LineNumberTable
  #16 = Utf8               LocalVariableTable
  #17 = Utf8               this
  #18 = Utf8               Lcn/sxt/java1/DynamicLinkingTest;
  #19 = Utf8               methodA
  #20 = Utf8               methodB
  #21 = Utf8               SourceFile
  #22 = Utf8               DynamicLinkingTest.java
  #23 = NameAndType        #12:#13        // "<init>":()V
  #24 = NameAndType        #10:#11        // num:I
  #25 = Class              #34            // java/lang/System
  #26 = NameAndType        #35:#36        // out:Ljava/io/PrintStream;
  #27 = Utf8               methodA()....
  #28 = Class              #37            // java/io/PrintStream
  #29 = NameAndType        #38:#39        // println:(Ljava/lang/String;)V
  #30 = Utf8               methodB()....
  #31 = NameAndType        #19:#13        // methodA:()V
  #32 = Utf8               cn/sxt/java1/DynamicLinkingTest
  #33 = Utf8               java/lang/Object
  #34 = Utf8               java/lang/System
  #35 = Utf8               out
  #36 = Utf8               Ljava/io/PrintStream;
  #37 = Utf8               java/io/PrintStream
  #38 = Utf8               println
  #39 = Utf8               (Ljava/lang/String;)V


为什么要用常量池呢？

因为在不同的方法，都可能调用常量或者方法，所以
只需要存储一份即可，然后记录其引用即可，节省了空间
常量池的作用：就是为了提供一些符号和常量，便于指令的识别
```
##### 3.2.5.14 方法的调用
###### 3.2.5.14.1 静态链接与动态链接
```markdown
在JVM中，将符号引用转换为调用方法的直接引用与方法的
绑定机制相关

静态链接：
当一个字节码文件被装载进JVM内部时，如果被调用的目标方法
在编译期确定，且运行期保持不变时，这种情况下将调用方法
的符号引用转换为直接引用的过程称之为静态链接

动态链接：
如果被调用的方法在编译期无法被确定下来，也就是说，只
能够在程序运行期将调用的方法的符号转换为直接引用，由
于这种引用转换过程具备动态性，因此也被称之为动态链接。
```
###### 3.2.5.14.2 方法的绑定机制

```markdown
静态链接和动态链接对应的方法的绑定机制为：早期绑
定（Early Binding）和晚期绑定（Late Binding）。绑定是
一个字段、方法或者类在符号引用被替换为直接引用的过
程，这仅仅发生一次。

早期绑定
早期绑定就是指被调用的目标方法如果在编译期可知，
且运行期保持不变时，即可将这个方法与所属的类型进
行绑定，这样一来，由于明确了被调用的目标方法究竟是
哪一个，因此也就可以使用静态链接的方式将符号引用转
换为直接引用。

晚期绑定
如果被调用的方法在编译期无法被确定下来，只能够在程序运
行期根据实际的类型绑定相关的方法，这种绑定方式也就
被称之为晚期绑定。
随着高级语言的横空出世，类似于Java一样的基于面向对象
的编程语言如今越来越多，尽管这类编程语言在语法风格上
存在一定的差别，但是它们彼此之间始终保持着一个共性，
那就是都支持封装、继承和多态等面向对象特性，既然这
一类的编程语言具备多态特性，那么自然也就具备早期绑
定和晚期绑定两种绑定方式。

Java中任何一个普通的方法其实都具备虚函数的特征，
它们相当于C++语言中的虚函数（C++中则需要使用关键
字virtual来显式定义）。如果在Java程序中不希望某个
方法拥有虚函数的特征时，则可以使用关键字final来标记这个方法。
```
```java
/**
 * 说明早期绑定和晚期绑定的例子
 */
class Animal {
    public void eat() {
        System.out.println("动物进食");
    }
}

interface Huntable {
    void hunt();
}

class Dog extends Animal implements Huntable {
    @Override
    public void eat() {
        System.out.println("狗吃骨头");
    }

    @Override
    public void hunt() {
        System.out.println("捕食耗子，多管闲事");
    }
}

class Cat extends Animal implements Huntable {
    public Cat() {
        super(); //表现为：早期绑定
    }

    public Cat(String name) {
        this(); //表现为：早期绑定
    }

    @Override
    public void eat() {
        super.eat(); //表现为：早期绑定
        System.out.println("猫吃鱼");
    }

    @Override
    public void hunt() {
        System.out.println("捕食耗子，天经地义");
    }
}

public class AnimalTest {
    public void showAnimal(Animal animal) {
        animal.eat(); //表现为：晚期绑定
    }

    public void showHunt(Huntable h) {
        h.hunt(); //表现为：晚期绑定
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215233216633.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215233246371.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215233306467.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
###### 3.2.5.14.3 虚方法和非虚方法
```markdown
如果方法在编译期就确定了具体的调用版本，这个版本在运
行时是不可变的。这样的方法称为非虚方法。
静态方法、私有方法、final 方法、实例构造器、父类方法都
是非虚方法。
其他方法称为虚方法。


子类对象的多态性的使用前提
类的继承关系
方法的重写

虚拟机中调用方法的指令

普通调用指令：
invokestatic：调用静态方法，解析阶段确定唯一方法版本
invokespecial：调用<init>方法、私有及父类方法，解
析阶段确定唯一方法版本
invokevirtual：调用所有虚方法
invokeinterface：调用接口方法
动态调用指令
invokedynamic：动态解析出需要调用的方法，然后执行
区别
前四条指令固化在虚拟机内部，方法的调用执行不可人为干预
而invokedynamic指令则支持由用户确定方法版本
其中invokestatic指令和invokespecial指令调用的方法称为
非虚方法，其余的（final修饰的除外）称为虚方法。
```
```java
/**
 * 解析调用中非虚方法、虚方法的测试
 *
 * invokestatic指令和invokespecial指令调用的方法称为非虚方法
 */
class Father {
    public Father() {
        System.out.println("father的构造器");
    }

    public static void showStatic(String str) {
        System.out.println("father " + str);
    }

    public final void showFinal() {
        System.out.println("father show final");
    }

    public void showCommon() {
        System.out.println("father 普通方法");
    }
}

public class Son extends Father {
    public Son() {
        //invokespecial 非虚方法
        super();
    }

    public Son(int age) {
        //invokespecial 非虚方法
        this();
    }

    //不是重写的父类的静态方法，因为静态方法不能被重写！
    public static void showStatic(String str) {
        System.out.println("son " + str);
    }

    private void showPrivate(String str) {
        System.out.println("son private" + str);
    }

    public void show() {
        //invokestatic 非虚方法
        showStatic("baidu.com");

        //invokestatic 非虚方法
        super.showStatic("good!");

        //invokespecial 非虚方法
        showPrivate("hello!");

        //invokevirtual
        //虽然字节码指令中显示为invokevirtual，但因为此方法声明有final，不能被子类重写，所以也认为此方法是非虚方法。
        showFinal();

        //invokespecial 非虚方法
        super.showCommon();

        //invokevirtual 虚方法
        //有可能子类会重写父类的showCommon()方法
        showCommon();
        
        //invokevirtual 虚方法
      	//info()是普通方法，有可能被重写，所以是虚方法
        info();

        MethodInterface in = null;
        //invokeinterface 虚方法
        in.methodA();
    }

    public void info() {

    }

    public void display(Father f) {
        f.showCommon();
    }

    public static void main(String[] args) {
        Son so = new Son();
        so.show();
    }
}

interface MethodInterface {
    void methodA();
}
```
###### 3.2.5.14.4 关于 invokedynamic 指令
```markdown
JVM字节码指令集一直比较稳定，一直到Java7中才增加了
一个invokedynamic指令，这是Java为了实现【动态类型语言】
支持而做的一种改进。
但是在Java7中并没有提供直接生成invokedynamic指令的方法，
需要借助ASM这种底层字节码工具来产生invokedynamic指
令。直到Java8的 Lambda表达式 的出现，invokedynamic指令
的生成，在Java中才有了直接的生成方式。
Java7中增加的动态语言类型支持的本质是对Java虚拟机
规范的修改，而不是对Java语言规则的修改，这一块相对
来讲比较复杂，增加了虚拟机中的方法调用，最直接的受益
者就是运行在Java平台的动态语言的编译器。


动态类型语言和静态类型语言

动态类型语言和静态类型语言两者的区别就在于对类型的检查
是在编译期还是在运行期，满足前者就是静态类型语言，反之
是动态类型语言。
说的再直白一点就是，静态类型语言是判断变量自身的类型信
息；动态类型语言是判断变量值的类型信息，变量没有类型信
息，变量值才有类型信息，这是动态语言的一个重要特征。
Java语言：String info = "mogu blog";
(Java是静态类型语言的，编译进行类型检查)
JS语言：
var name = "shkstart";  var name = 10;（运行时才进行检查）
Python语言：info = 130.5;  （动态类型语言）
```
```java
/**
 * 体会invokedynamic 指令
 */
@FunctionalInterface
interface Func {
    public boolean func(String str);
}

public class Lambda {
    public void lambda(Func func) {
        return;
    }

    public static void main(String[] args) {
        Lambda lambda = new Lambda();

        Func func = s -> {
            return true;
        };

        lambda.lambda(func);

        lambda.lambda(s -> {
            return true;
        });
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215234132177.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
###### 3.2.5.14.5 方法重写的本质
```markdown
Java 语言中方法重写的本质：

找到操作数栈顶的第一个元素所执行的对象的实际类型，记作C。
如果在类型C中找到与常量中的描述符合简单名称都相符的方法，
则进行访问权限校验，如果通过则返回这个方法的直接引用，
查找过程结束，如果不通过，则返回
java.lang.IllegalAccessError 异常
否则，按照继承关系从下往上依次对C的各个父类进行第2步的
搜索和验证过程。如果始终没有找到合适的方法，则抛出
java.lang.AbstractMethodError异常。

IllegalAccessError介绍
程序试图访问或修改一个属性或调用一个方法，这个属性或方
法，你没有权限访问。
一般的，这个会引起编译器异常。这个错误如果发生在运行时，
就说明一个类发生了不兼容的改变。
比如，你把应该有的jar包放从工程中拿走了，或者Mave
n中存在jar包冲突
```
###### 3.2.5.14.6 虚方法表
```markdown
在面向对象的编程中，会很频繁的使用到动态分派，如果在每
次动态分派的过程中都要重新在类的方法元数据中搜索合适的
目标的话就可能影响到执行效率。
因此，为了提高性能，JVM采用在类的方法区建立一个虚方法
表（virtual method table）来实现，非虚方法不会出现在表
中。使用索引表来代替查找。
每个类中都有一个虚方法表，表中存放着各个方法的实际入口。
虚方法表是什么时候被创建的呢？ 虚方法表会在类加载
的链接阶段被创建并开始初始化，类的变量初始值准备
完成之后，JVM会把该类的虚方法表也初始化完毕。
如图所示：如果类中重写了方法，那么调用的时候，就
会直接在该类的虚方法表中查找
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210216001144194.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
回顾解析阶段

解析阶段就是将常量池内的符号引用转换为直接引用的过程
解析动作主要针对类或接口、字段、类方法、接口方法、
方法类型等。对应常量池中的CONSITANT_Class_info、 
CONSTANT_Fieldref_info、 CONSTANT_Methodref_into等
```
```java
public class VirtualMethodTable {

}

interface Friendly {
    void sayHello();
    void sayGoodbye();
}
class Dog {
    public void sayHello() {
    }
    @Override
    public String toString() {
        return "Dog";
    }
}

class Cat implements Friendly {
    public void eat() {
    }
    public void sayHello() {
    }
    public void sayGoodbye() {
    }
    protected void finalize() {
    }
    public String toString() {
        return "Cat";
    }
}

class CockerSpaniel extends Dog implements Friendly {
    public void sayHello() {
        super.sayHello();
    }
    public void sayGoodbye() {
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021021600220580.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210216002518294.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210216002854376.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
###### 3.2.5.14.7 方法返回地址(return address)
```markdown
存放调用该方法的pc寄存器的值。
一个方法的结束，有两种方式：
正常执行完成
出现未处理的异常，非正常退出
无论通过哪种方式退出，在方法退出后都返回到该方法被调
用的位置。方法正常退出时，调用者的pc计数器的值作为
返回地址，即调用该方法的指令的下一条指令的地址。而
通过异常退出的，返回地址是要通过异常表来确定，栈帧
中一般不会保存这部分信息。
本质上，方法的退出就是当前栈帧出栈的过程。此时，
需要恢复上层方法的局部变量表、操作数栈、将返回值
压入调用者栈帧的操作数栈、设置PC寄存器值等，让调
用者方法继续执行下去。
正常完成出口和异常完成出口的区别在于：通过异常
完成出口退出的不会给他的上层调用者产生任何的返回值。
```
```markdown
方法退出的两种方式
当一个方法开始执行后，只有两种方式可以退出这个方法：

执行引擎遇到任意一个方法返回的字节码指令（return），
会有返回值传递给上层的方法调用者，简称正常完成出口
一个方法在正常调用完成之后，究竟需要使用哪一个返回
指令，还需要根据方法返回值的实际数据类型而定。
在字节码指令中，返回指令包含：
ireturn：当返回值是boolean，byte，char，short和int类
型时使用
lreturn：Long类型
freturn：Float类型
dreturn：Double类型
areturn：引用类型
return：返回值类型为void的方法、构造器、类和接口的初始化方法
在方法执行过程中遇到异常（Exception），并且这个异常没有在
方法内进行处理，也就是只要在本方法的异常表中没有搜索到匹
配的异常处理器，就会导致方法退出，简称异常完成出口。
方法执行过程中，抛出异常时的异常处理，存储在一个异常处理
表，方便在发生异常的时候找到处理异常的代码
```
```markdown
异常处理表
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210217111515901.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
反编译字节码文件，可得到 Exception table
from ：字节码指令起始地址
to ：字节码指令结束地址
target ：出现异常跳转至地址为 11 的指令执行
type ：捕获异常的类型
```
```java
public class ReturnAddressTest {
    public boolean methodBoolean() {
        return false;
    }

    public byte methodByte() {
        return 0;
    }

    public short methodShort() {
        return 0;
    }

    public char methodChar() {
        return 'a';
    }

    public int methodInt() {
        return 0;
    }

    public long methodLong() {
        return 0L;
    }

    public float methodFloat() {
        return 0.0f;
    }

    public double methodDouble() {
        return 0.0;
    }

    public String methodString() {
        return null;
    }

    public Date methodDate() {
        return null;
    }

    public void methodVoid() {

    }

    static {
        int i = 10;
    }

    public void method2() {
        methodVoid();
        try {
            method1();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public void method1() throws IOException {
        FileReader fis = new FileReader("atguigu.txt");
        char[] cBuffer = new char[1024];
        int len;
        while ((len = fis.read(cBuffer)) != -1) {
            String str = new String(cBuffer, 0, len);
            System.out.println(str);
        }
        fis.close();
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210217111638965.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210217111706850.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210217111726583.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210217111743499.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
###### 3.2.5.14.8 一些附加信息
```markdown
栈帧中还允许携带与Java虚拟机实现相关的一些附加信
息。例如：对程序调试提供支持的信息。
```
###### 3.2.5.14.9 栈的相关面试题
```markdown
举例栈溢出的情况？（StackOverflowError）
通过 -Xss 设置栈的大小

递归很容易出现栈溢出

调整栈大小，就能保证不出现溢出么？

不能保证不出现溢出，只能让栈溢出出现的时间晚一点，不可
能不出现
分配的栈内存越大越好么？
不是，一定时间内降低了栈溢出的概率，但是会挤占其它的线
程空间，因为整个虚拟机的内存空间是有限的

垃圾回收是否涉及到虚拟机栈？
不涉及

方法中定义的局部变量是否线程安全？

何为线程安全？

如果只有一个线程才可以操作此数据，则必是线程安全的。
如果有多个线程操作此数据，则此数据是共享数据。如果
不考虑同步机制的话，会存在线程安全问题。

具体问题具体分析：
如果对象是在内部产生，并在内部消亡，没有返回到外部，那
么它就是线程安全的，反之则是线程不安全的。
```
```java
/**
 * 面试题：
 * 方法中定义的局部变量是否线程安全？具体情况具体分析
 *
 *   何为线程安全？
 *      如果只有一个线程才可以操作此数据，则必是线程安全的。
 *      如果有多个线程操作此数据，则此数据是共享数据。如果不考虑同步机制的话，会存在线程安全问题。
 */
public class StringBuilderTest {

    //s1的声明方式是线程安全的，因为s1只在方法内部操作，属于局部变量
    public static void method1(){
        //StringBuilder:线程不安全
        StringBuilder s1 = new StringBuilder();
        s1.append("a");
        s1.append("b");
        //...
    }

    //sBuilder通过参数传递方法内，存在线程不安全的问题
    public static void method2(StringBuilder sBuilder){
        sBuilder.append("a");
        sBuilder.append("b");
        //...
    }

    //操作s1之后，将s1作为返回值返回，这样可能被其他线程所调用，所以存在线程不安全的问题
    public static StringBuilder method3(){
        StringBuilder s1 = new StringBuilder();
        s1.append("a");
        s1.append("b");
        return s1;
    }

    //s1的操作：是线程安全的，因为String是线程安全的
    public static String method4(){
        StringBuilder s1 = new StringBuilder();
        s1.append("a");
        s1.append("b");
        return s1.toString();
    }

    public static void main(String[] args) {
        StringBuilder s = new StringBuilder();

        new Thread(() -> {
            s.append("a");
            s.append("b");
        }).start();

        method2(s);
    }
}

```
### 3.3 本地方法接口
#### 3.3.1 本地方法
```markdown
简单地讲，一个Native Method是一个Java调用非Java代
码的接囗
一个Native Method是这样一个Java方法：该方法的实现由
非Java语言实现，比如C。
这个特征并非Java所特有，很多其它的编程语言都有这
一机制，比如在C++中，你可以用extern "C"告知C++编译器去
调用一个C的函数。
“A native method is a Java method whose 
implementation is provided by non-java code.”（本地方
法是一个Java的方法，它的具体实现是非Java代码的实现）
在定义一个native method时，并不提供实现体（有些像
定义一个Java interface），因为其实现体是由非java语言
在外面实现的。
本地接口的作用是融合不同的编程语言为Java所用，它的初
衷是融合C/C++程序。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210217183424946.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 3.3.2 native 方法举例
```markdown
Object类的getClass( )方法

public final native Class<?> getClass();
```

```java
    public synchronized void start() {
        /**
         * This method is not invoked for the main method thread or "system"
         * group threads created/set up by the VM. Any new functionality added
         * to this method in the future may have to also be added to the VM.
         *
         * A zero status value corresponds to state "NEW".
         */
        if (threadStatus != 0)
            throw new IllegalThreadStateException();

        /* Notify the group that this thread is about to be started
         * so that it can be added to the group's list of threads
         * and the group's unstarted count can be decremented. */
        group.add(this);

        boolean started = false;
        try {
            start0();
            started = true;
        } finally {
            try {
                if (!started) {
                    group.threadStartFailed(this);
                }
            } catch (Throwable ignore) {
                /* do nothing. If start0 threw a Throwable then
                  it will be passed up the call stack */
            }
        }
    }

    private native void start0();

```
```markdown
自定义Native方法
注意：标识符native可以与其他java标识符连用，但是不
能与abstract连用
```
```java
public class IHaveNatives {
    public native void Native1(int x);

    public native static long Native2();

    private native synchronized float Native3(Object o);

    native void Native4(int[] ary) throws Exception;
    
}

```
#### 3.3.3 为什么要使用Native Method
```java
Java使用起来非常方便，然而有些层次的任务用Java实现起来不容易，或者我们对程序的效率很在意时，问题就来了。

与Java环境外交互
有时Java应用需要与Java外面的环境交互，这是本地方法存
在的主要原因。你可以想想Java需要与一些底层系统，如
操作系统或某些硬件交换信息时的情况。
本地方法正是这样一种交流机制：它为我们提供了一个非常
简洁的接口，而且我们无需去了解Java应用之外的繁琐的细节。
与操作系统的交互
JVM支持着Java语言本身和运行时库，它是Java程序赖以
生存的平台，它由一个解释器（解释字节码）和一些连接
到本地代码的库组成。
然而不管怎样，它毕竟不是一个完整的系统，它经常依赖
于一底层系统的支持。这些底层系统常常是强大的操作系统。
通过使用本地方法，我们得以用Java实现了jre的与底
层系统的交互，甚至JVM的一些部分就是用C写的。
还有，如果我们要使用一些Java语言本身没有提供封装
的操作系统的特性时，我们也需要使用本地方法。
Sun’s Java
Sun的解释器是用C实现的，这使得它能像一些普通
的C一样与外部交互。jre大部分是用Java实现的，它也通
过一些本地方法与外界交互。
例如：类java.lang.Thread的setPriority( )方法是用J
ava实现的，但是它实现调用的是该类里的本地方法
setPriority0( )。这个本地方法是用C实现的，并被植入JVM内
部在Windows 95的平台上，这个本地方法最终将
调用Win32 setpriority( ) API。
这是一个本地方法的具体实现由JVM直接提供，更多的情况是
本地方法由外部的动态链接库
（external dynamic link library）提供，然后被JVM调用。
```
#### 3.3.4 本地方法的现状
```markdown
目前该方法使用的越来越少了，除非是与硬件有关的应用，比
如通过Java程序驱动打印机或者Java系统管理生产设备，在
企业级应用中已经比较少见。因为现在的异构领域间的通信
很发达，比如可以使用Socket通信，也可以使用Web Service等等，
不多做介绍。
```
### 3.4 本地方法栈
#### 3.4.1 本地方法栈
```markdown
Java虚拟机栈用于管理Java方法的调用，而本地方法栈用于
管理本地方法的调用。
本地方法栈，也是线程私有的。
允许被实现成固定或者是可动态扩展的内存大小（在内存溢出方
面和虚拟机栈相同）
如果线程请求分配的栈容量超过本地方法栈允许的最大容量，Ja
va虚拟机将会抛出一个StackoverflowError 异常。
如果本地方法栈可以动态扩展，并且在尝试扩展的时候无法申请
到足够的内存，或者在创建新的线程时没有足够的内存去创建
对应的本地方法栈，那么Java虚拟机将会抛出一
个OutofMemoryError异常。
本地方法一般是使用C语言实现的。
它的具体做法是Native Method Stack中登记native方法，
在Execution Engine 执行时加载本地方法库。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210217184638227.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 3.4.2 注意事项
```markdown
当某个线程调用一个本地方法时，它就进入了一个全新的并且
不再受虚拟机限制的世界。它和虚拟机拥有同样的权限。
本地方法可以通过本地方法接口来访问虚拟机内部的运行时数据区
它甚至可以直接使用本地处理器中的寄存器
直接从本地内存的堆中分配任意数量的内存
并不是所有的JVM都支持本地方法。因为Java虚拟机规范并
没有明确要求本地方法栈的使用语言、具体实现方式、数据结构等。
如果JVM产品不打算支持native方法，也可以无需实现本地方法栈。
在Hotspot JVM中，直接将本地方法栈和虚拟机栈合二为一。
```
### 3.5 堆
#### 3.5.1 认识堆内存
```markdown
一个进程对应一个JVM实例
一个JVM实例对应一个堆空间
进程包含多个线程，所以线程之间共享同一个堆空间
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210217202748189.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
一个JVM实例只存在一个堆内存，堆也是Java内存管理的核心区域。
Java堆区在JVM启动的时候即被创建，其空间大小也就确定了，
堆是JVM管理的最大一块内存空间。
堆内存的大小是可以调节的。
《Java虚拟机规范》规定，堆可以处于物理上不连续的内存空
间中，但在逻辑上它应该被视为连续的。
所有的线程共享Java堆，在这里还可以划分线程私有的缓
冲区（Thread Local Allocation Buffer，TLAB）。
《Java虚拟机规范》中对Java堆的描述是：所有的对象实例
以及数组都应当在运行时分配在堆上。
（The heap is the run-time data area from which 
memory for all class instances and arrays is 
allocated）
从实际使用角度看的：“几乎”所有的对象实例都在这里分配
内存。因为还有一些对象是在栈上分配的（逃逸分析，标量替换）
数组和对象可能永远不会存储在栈上，因为栈帧中保存引用，这个
引用指向对象或者数组在堆中的位置。
在方法结束后，堆中的对象不会马上被移除，仅仅在垃圾收集的
时候才会被移除。
也就是触发了GC的时候，才会进行回收
如果堆中对象马上被回收，那么用户线程就会收到影响，因为
有 stop the word。堆，是GC（Garbage Collection，
垃圾收集器）执行
垃圾回收的重点区域。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210217202954585.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
public class SimpleHeap {
    private int id;//属性、成员变量

    public SimpleHeap(int id) {
        this.id = id;
    }

    public void show() {
        System.out.println("My ID is " + id);
    }

    public static void main(String[] args) {
        SimpleHeap sl = new SimpleHeap(1);
        SimpleHeap s2 = new SimpleHeap(2);
        int[] arr = new int[10];
        Object[] arr1 = new Object[10];
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210217210745571.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
一个JVM实例只存在一个堆内存，并且堆内存的大小是可以调节的
```
```java
public class HeapDemo {

    public static void main(String[] args) {
        System.out.println("start...");
        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("end...");
    }

}

public class HeapDemo1 {

    public static void main(String[] args) {
        System.out.println("start...");
        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("end...");
    }

}


```
```markdown
如何设置堆内存大小

进程1
-Xms10m -Xmx10m
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210218215224520.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
进程2
-Xms20m -Xmx20m
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210218215254482.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
查看堆内存
```
```markdown
进程1：堆内存为10M
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210218215630572.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
进程2：堆内存为20M
```
![ ](https://img-blog.csdnimg.cn/20210218215657789.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 3.5.2 堆内存分区(重要)
```markdown
堆内存细分

现代垃圾收集器大部分都基于分代收集理论设计，堆空间细分为：

Java 7及之前堆内存逻辑上分为三部分：新生区 + 养老区 + 永久区
Young Generation Space 新生区 Young/New
又被划分为 Eden区 和 Survivor区
Tenure generation space 养老区 Old/Tenure
Permanent Space永久区 Perm
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210218222708368.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
Java 8及之后堆内存逻辑上分为三部分：新生区 + 养老区 + 元空间
Young/New Generation Space 新生区，又被划分为Eden区和
Survivor区
Old/Tenure generation space 养老区
Meta Space 元空间 Meta
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021021822281079.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
约定：新生区 = 新生代 = 年轻代、 养老区 = 老年区 = 老年代、
永久区 = 永久代
堆空间内部结构，JDK1.8之前从永久代 替换成 元空间
堆空间逻辑上包括 永久代/元空间，实际上控制不到
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021021822292096.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 3.5.3 设置堆内存大小与OOM
```markdown
设置堆空间大小
Java堆区用于存储Java对象实例，那么堆的大小在JVM启动
时就已经设定好了，大家可以通过选项"-Xms"和"-Xmx"来进行设置。
-Xms 用于表示堆区的初始内存，等价于 -XX:InitialHeapSize
-Xmx 则用于表示堆区的最大内存，等价于 -XX:MaxHeapSize
一旦堆区中的内存大小超过“-Xmx”所指定的最大内存时，将会
抛出OutofMemoryError异常。
通常会将-Xms和-Xmx两个参数配置相同的值，其目的是为了
能够在Java垃圾回收机制清理完堆区后不需要重新分隔计算
堆区的大小，从而提高性能。
默认情况下:
初始内存大小：物理电脑内存大小/64
最大内存大小：物理电脑内存大小/4
```
```java
/**
 * 1. 设置堆空间大小的参数
 * -Xms 用来设置堆空间（年轻代+老年代）的初始内存大小
 *      -X 是jvm的运行参数
 *      ms 是memory start
 * -Xmx 用来设置堆空间（年轻代+老年代）的最大内存大小
 *
 * 2. 默认堆空间的大小
 *      初始内存大小：物理电脑内存大小 / 64
 *      最大内存大小：物理电脑内存大小 / 4
 *
 * 3. 手动设置：-Xms600m -Xmx600m
 *     开发中建议将初始堆内存和最大的堆内存设置成相同的值。
 *
 * 4. 查看设置的参数：方式一： jps   /  jstat -gc 进程id
 *                  方式二：-XX:+PrintGCDetails
 */
public class HeapSpaceInitial {
    public static void main(String[] args) {

        //返回Java虚拟机中的堆内存总量
        long initialMemory = Runtime.getRuntime().totalMemory() / 1024 / 1024;
        //返回Java虚拟机试图使用的最大堆内存量
        long maxMemory = Runtime.getRuntime().maxMemory() / 1024 / 1024;

        System.out.println("-Xms : " + initialMemory + "M");
        System.out.println("-Xmx : " + maxMemory + "M");
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210218224559283.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
两种查看堆内存的方式
方式一：命令行依次执行如下两个指令
jps
jstat -gc 进程id
方式二：设置虚拟机参数 -XX:+PrintGCDetails
为什么设置 600MB ，算出来只有 575MB 呢？
from区和to区只能有一个区存放对象，所以相加的时候只能加
上一个区的大小
可以看到新生区的大小 = 伊甸园区大小 + 幸存者 from/to 
区大小
即 179200KB = 153600KB + 25600KB
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210218224646283.png)
```markdown
OOM举例
```
```java
/**
 * -Xms600m -Xmx600m
 */
public class OOMTest {
    public static void main(String[] args) {
        ArrayList<Picture> list = new ArrayList<>();
        while(true){
            try {
                Thread.sleep(20);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            list.add(new Picture(new Random().nextInt(1024 * 1024)));
        }
    }
}

class Picture {
    private byte[] pixels;

    public Picture(int length) {
        this.pixels = new byte[length];
    }
}

```
```markdown
设置虚拟机参数
-Xms600m -Xmx600m
监控堆内存变化：Old 区域一点一点在变大，直到最后一次垃圾回
收器无法回收垃圾时，堆内存被撑爆，抛出 OutOfMemoryError 
错误
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210219093532525.png)

```java
堆内存变化图
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210219093709314.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
分析原因：大对象导致堆内存溢出
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210219093750499.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 3.5.4 年轻代与老年代
```markdown
Java 对象分类
存储在JVM中的Java对象可以被划分为两类：
一类是生命周期较短的瞬时对象，这类对象的创建和消亡都非常迅速
另外一类对象的生命周期却非常长，在某些极端的情况下还能够
与JVM的生命周期保持一致
Java堆区进一步细分的话，可以划分为年轻代（YoungGen）和老
年代（oldGen）
其中年轻代又可以划分为Eden空间、Survivor0空间和Survivor1
空间（有时也叫做from区、to区）
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210219093948817.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 3.5.5 配置新老比例
```markdown
配置新生代与老年代的比例

配置新生代与老年代在堆结构的占比（下面这些参数在开发中一
般不会调）

默认-XX:NewRatio=2，表示新生代占1，老年代占2，新生代占
整个堆的1/3
可以修改-XX:NewRatio=4，表示新生代占1，老年代占4，新生代
占整个堆的1/5
当发现在整个项目中，生命周期长的对象偏多，那么就可以通过
调整老年代的大小，来进行调优
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210219094127863.png)
```markdown
新生区中的比例

在HotSpot中，Eden空间和另外两个survivor空间缺省所占的比
例是8 : 1 : 1
当然开发人员可以通过选项-XX:SurvivorRatio调整这个空间比
例。比如-XX:SurvivorRatio=8
几乎所有的Java对象都是在Eden区被new出来的。
绝大部分的Java对象的销毁都在新生代进行了（有些大的对象
在Eden区无法存储时候，将直接进入老年代）
IBM公司的专门研究表明，新生代中80%的对象都是“朝生夕死”的。
可以使用选项"-Xmn"设置新生代最大内存大小，但这个参数
一般使用默认值就可以了。
新生区的对象默认生命周期超过 15 ，将进入老年代
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210219094327133.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
/**
 * -Xms600m -Xmx600m
 *
 * -XX:NewRatio ： 设置新生代与老年代的比例。默认值是2.
 * -XX:SurvivorRatio ：设置新生代中Eden区与Survivor区的比例。默认值是8
 * -XX:-UseAdaptiveSizePolicy ：关闭自适应的内存分配策略  （暂时用不到）
 * -Xmn:设置新生代的空间的大小。 （一般不设置）
 */
public class EdenSurvivorTest {
    public static void main(String[] args) {
        System.out.println("我只是来打个酱油~");
        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

```
```markdown
通过命令行查看各种比例

查看新生代与老年代的比例
1 jps
2 jinfo -flag NewRatios 进程id 


查看新生区中伊甸园区与幸存者区的比例
1 jps
2 jinfo -flag SurvivorRatio 进程id 

设置 JVM 参数
-Xms600m -Xmx600m -XX:NewRatio=2 -XX:SurvivorRatio=8
```
```markdown
新生区中：伊甸园区 : 幸存者 0 区 : 幸存者 1 区 = 8 : 1 : 1
新生区 : 老年区 = 1 : 2
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210219095115266.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 3.5.6 图解对象分配过程
```markdown
对象分配过程
```
```markdown
为新对象分配内存是一件非常严谨和复杂的任务，JVM的设计
者们不仅需要考虑内存如何分配、在哪里分配等问题，并且由
于内存分配算法与内存回收算法密切相关，所以还需要考
虑GC执行完内存回收后是否会在内存空间中产生内存碎片。

new的对象先放伊甸园区。此区有大小限制。
当伊甸园的空间填满时，程序又需要创建对象，JVM的垃圾回
收器将对伊甸园区进行垃圾回收（MinorGC），将伊甸园区中
的不再被其他对象所引用的对象进行销毁。再加载新的对象
放到伊甸园区。
然后将伊甸园中的剩余对象移动到幸存者0区。
如果再次触发垃圾回收，此时将伊甸园区和幸存者0区进行
垃圾回收，剩下的对象就会放到幸存者1区。
如果再次经历垃圾回收，此时会重新放回幸存者0区，接着再
去幸存者1区。
啥时候能去养老区呢？可以设置次数。默认是15次。可以设
置新生区进入养老区的年龄限制，设置 JVM 参数：
-XX:MaxTenuringThreshold=N 进行设置
在养老区，相对悠闲。当养老区内存不足时，再次
触发GC：Major GC，进行养老区的内存清理
若养老区执行了Major GC之后，发现依然无法进行对
象的保存，就会产生OOM异常。
```
```markdown
图解对象分配
我们创建的对象，一般都是存放在Eden区的，当我们的Eden区
满了后，就会触发GC操作，一般被称为 YGC / Minor GC操作
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210219102841883.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
当我们进行一次垃圾收集后，红色的对象将会被回收，而
绿色的独享还被占用着，存放在S0(Survivor From)区。同
时我们给每个对象设置了一个年龄计数器，经过一次回收
后还存在的对象，将其年龄加 1。
同时Eden区继续存放对象，当Eden区再次存满的时候，又
会触发一个MinorGC操作，此时GC将会把 
Eden和Survivor From中的对象进行一次垃圾收集，把存活的
对象放到 Survivor To区，同时让存活的对象年龄 + 1
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210219102913903.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
我们继续不断的进行对象生成和垃圾回收，当Survivor中的对象
的年龄达到15的时候，将会触发一次 Promotion 晋升的操
作，也就是将年轻代中的对象晋升到老年代中
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210219102947234.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
/**
 * -Xms600m -Xmx600m
 */
public class HeapInstanceTest {
    byte[] buffer = new byte[new Random().nextInt(1024 * 200)];

    public static void main(String[] args) {
        ArrayList<HeapInstanceTest> list = new ArrayList<HeapInstanceTest>();
        while (true) {
            list.add(new HeapInstanceTest());
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

```

```markdown
注意【伊甸园区、幸存者区、老年区】的内存变化趋势
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210220085929162.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
特殊情况说明
```

```markdown
思考：幸存区满了咋办？

特别注意，在Eden区满了的时候，才会触发MinorGC，而幸
存者区满了后，不会触发MinorGC操作
如果Survivor区满了后，将会触发一些特殊的规则，也就是可
能直接晋升老年代
对象分配的特殊情况

如果来了一个新对象，先看看 Eden 是否放的下？
如果 Eden 放得下，则直接放到 Eden 区
如果 Eden 放不下，则触发 YGC ，执行垃圾回收，看看还能
不能放下？放得下最好当然最好咯~~~
将对象放到老年区又有两种情况：
如果 Eden 执行了 YGC 还是无法放不下该对象，那没得办法，
只能说明是超大对象，只能直接怼到老年代
那万一老年代都放不下，则先触发重 GC ，再看看能不能放下，
放得下最好，但如果还是放不下，那只能报 OOM 啦~~~
如果 Eden 区满了，将对象往幸存区拷贝时，发现幸存区放不下
啦，那只能便宜了某些新对象，让他们直接晋升至老年区
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210220090425935.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 3.5.7 常用调优工具

```markdown
常用的 JVM 调优工具

JDK命令行
Eclipse：Memory Analyzer Tool
Jconsole
Visual VM（实时监控 推荐~）
Jprofiler（推荐~）
Java Flight Recorder（实时监控）
GCViewer
GCEasy
```
#### 3.5.8 总结

```markdown
针对于幸存者s0，s1区的总结：复制之后有交换，谁空谁是to
关于垃圾回收：频繁在新生区收集，很少在养老区收集，
几乎不在永久区/元空间收集
新生代采用复制算法的目的：是为了减少内碎片
```
#### 3.5.9 GC垃圾回收器
##### 3.5.9.1 分代收集思想
```markdown
Minor GC、Major GC、Full GC

我们都知道，JVM调优的一个环节，也就是垃圾收集，我们
需要尽量的避免垃圾回收，因为在垃圾回收的过程中，容易出
现STW（Stop the World）的问题，而 Major GC 和 Full GC出
现STW的时间，是Minor GC的10倍以上

JVM在进行GC时，并非每次都对上面三个内存( 新生代、老年代；
方法区 )区域一起回收的，大部分时候回收的都是指新生代。
针对Hotspot VM的实现，它里面的GC按照回收区域又分为两大
种类型：一种是部分收集（Partial GC），一种是整堆收
集（FullGC）

部分收集：不是完整收集整个Java堆的垃圾收集。其中又分为：
新生代收集（ Minor GC/Young GC ）：只是新生
代( Eden、S0/S1 )的垃圾收集
老年代收集（ Major GC/Old GC ）：只是老年代的垃圾收集。
目前，只有CMS GC会有单独收集老年代的行为。
注意，很多时候Major GC会和Full GC混淆使用，需要具体分辨
是老年代回收还是整堆回收。
混合收集（Mixed GC）：收集整个新生代以及部分老年代的垃圾
收集。
目前，只有G1 GC会有这种行为
整堆收集（Full GC）：收集整个java堆和方法区的垃圾收集。
```
##### 3.5.9.2 Young/Minor GC

```markdown
年轻代 GC（Minor GC）触发机制

当年轻代空间不足时，就会触发Minor GC，这里的年轻代满指的
是Eden区满，Survivor区满不会触发GC。（每次Minor GC会清
理年轻代的内存）
因为Java对象大多都具备朝生夕灭的特性，所以Minor GC非
常频繁，一般回收速度也比较快。这一定义既清晰又易于理解。
Minor GC会引发STW，暂停其它用户的线程，等待垃圾回收线
程结束，用户线程才恢复运行
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210220092112702.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 3.5.9.3 Major GC

```markdown
老年代 GC（MajorGC/Full GC）触发机制

指发生在老年代的GC，对象从老年代消失时，我们说 “Major Gc” 
或 “Full GC” 发生了
出现了MajorGc，经常会伴随至少一次的Minor GC
但非绝对的，在Parallel Scavenge收集器的收集策略里就有直接
进行Major GC的策略选择过程
也就是在老年代空间不足时，会先尝试触发Minor GC，如果之
后空间还不足，则触发Major GC
Major GC的速度一般会比Minor GC慢10倍以上，STW的时间更长
如果Major GC后，内存还不足，就报OOM了
```
##### 3.5.9.4 Full GC

```markdown
Full GC 触发机制（后面细讲）

触发Full GC执行的情况有如下五种：

调用System.gc( )时，系统建议执行Full GC，但是不必然执行
老年代空间不足
方法区空间不足
通过Minor GC后进入老年代的平均大小 大于 老年代的可用内存
由Eden区、survivor space0（From Space）区 向
survivor space1（To Space）区复制时，对象大小大于
To Space可用内存，则把该对象转存到老年代，且老年代的
可用内存 小于 该对象大小
说明：Full GC 是开发或调优中尽量要避免的。这样STW时间
会短一些
```
##### 3.5.9.5 GC 日志分析
```java
/**
 * 测试MinorGC、MajorGC、FullGC
 * -Xms9m -Xmx9m -XX:+PrintGCDetails
 */
public class GCTest {
    public static void main(String[] args) {
        int i = 0;
        try {
            List<String> list = new ArrayList<>();
            String a = "atguigu.com";
            while (true) {
                list.add(a);
                a = a + a;
                i++;
            }

        } catch (Throwable t) {
            t.printStackTrace();
            System.out.println("遍历次数为：" + i);
        }
    }
}

```

```markdown
JVM 参数
-Xms9m -Xmx9m -XX:+PrintGCDetails

GC 日志：在 OOM 之前，一定会触发一次 Full GC ，因
为只有在老年代空间不足且进行垃圾回收后仍然空间不足的
时候，才会爆出OOM异常
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210220092502679.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
[Full GC (Ergonomics) [PSYoungGen: 1319K->0K(2560K)] 
[ParOldGen: 6782K->4864K(7168K)] 
8102K->4864K(9728K)
[Metaspace: 3452K->3452K(1056768K)], 0.0050464 secs] 
[Times: user=0.00 sys=0.00, real=0.01 secs] 

```

```markdown
[PSYoungGen: 1319K->0K(2560K)] ：年轻代总空间为2560K，
当前占用 1319K ，经过垃圾回收后占用 0K
[ParOldGen: 6782K->4864K(7168K)] ：老年代总空
间为 7168K ，当前占用 6782K ，经过垃圾回收后占用 4864K
8102K->4864K(9728K)：堆内存总空间为 9728K ，
当前占用 8102K ，经过垃圾回收后占用 4864K
[Metaspace: 3452K->3452K(1056768K)] ：
元空间总空间为 1056768K ，当前占用 3452K ，经过垃圾
回收后占用 3452K
0.0050464 secs ：垃圾回收用时 0.0050464 secs
```
##### 3.5.9.6 堆空间分配思想

```markdown
为什么要把Java堆分代？不分代就不能正常工作了吗？

经研究，不同对象的生命周期不同。70%-99%的对象是临时对象。
新生代：有Eden、两块大小相同的Survivor
（又称为from/to，s0/s1）构成，to总为空。
老年代：存放新生代中经历多次GC之后仍然存活的对象。
其实不分代完全可以，分代的唯一理由就是优化GC性能。
如果没有分代，那所有的对象都在一块，就如同把一个学校的人都关
在一个教室。GC的时候要找到哪些对象没用，这样就会对堆的所有区
域进行扫描。
而很多对象都是朝生夕死的，如果分代的话，把新创建的对象放到
某一地方，当GC的时候先把这块存储“朝生夕死”对象的区域进行
回收，这样就会腾出很大的空间出来。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210220092906848.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 3.5.9.7 内存分配策略

```markdown
内存分配策略或对象提升（Promotion）规则

如果对象在Eden出生并经过第一次Minor GC后仍然存活，并且
能被Survivor容纳的话，将被移动到Survivor空间中，并将对象
年龄设为1。
对象在Survivor区中每熬过一次MinorGC，年龄就增加1岁，当
它的年龄增加到一定程度（默认为15岁，其实每个JVM、每个
GC都有所不同）时，就会被晋升到老年代
对象晋升老年代的年龄阀值，可以通过选项
-XX:MaxTenuringThreshold来设置

针对不同年龄段的对象分配原则如下所示：

优先分配到Eden
大对象直接分配到老年代：
尽量避免程序中出现过多的大对象
长期存活的对象分配到老年代
动态对象年龄判断：
如果Survivor区中相同年龄的所有对象大小的总和大于 
Survivor空间的一半，年龄大于或等于该年龄的对象
可以直接进入老年代，无须等到MaxTenuringThreshold中要求
的年龄。

空间分配担保：
-XX:HandlePromotionFailure ，也就是经过Minor GC后，所有
的对象都存活，因为Survivor比较小，所以就需要将Survivor无法
容纳的对象，存放到老年代中。
```
```java
/**
 * 测试：大对象直接进入老年代
 * -Xms60m -Xmx60m -XX:NewRatio=2 -XX:SurvivorRatio=8 -XX:+PrintGCDetails
 */
public class YoungOldAreaTest {
    public static void main(String[] args) {
        byte[] buffer = new byte[1024 * 1024 * 20]; //20m

    }
}

```
```markdown
JVM参数
-Xms60m -Xmx60m 
-XX:NewRatio=2 -XX:SurvivorRatio=8 
-XX:+PrintGCDetails
整个过程并没有进行垃圾回收，并且 ParOldGen 区直
接占用了 20MB 的空间，说明大对象直接怼到了老年代中
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021022009323584.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 3.5.9.8 为对象分配内存: TLAB
###### 3.5.9.8.1 为什么有 TLAB
```markdown
问题：堆空间都是共享的么？

不一定，因为还有TLAB这个概念，在堆中划分出一块区域，
为每个线程所独占

为什么有TLAB（Thread Local Allocation Buffer）？

TLAB：Thread Local Allocation Buffer，也就是为
每个线程单独分配了一个缓冲区
堆区是线程共享区域，任何线程都可以访问到堆区中的共享数据
由于对象实例的创建在JVM中非常频繁，因此在并发环境下从堆
区中划分内存空间是线程不安全的
为避免多个线程操作同一地址，需要使用加锁等机制，进而影
响分配速度。
```
###### 3.5.9.8.2 什么是 TLAB
```markdown
从内存模型而不是垃圾收集的角度，对Eden区域继续进行划分，
JVM为每个线程分配了一个私有缓存区域，它包含在Eden空间内。
多线程同时分配内存时，使用TLAB可以避免一系列的非线程安
全问题，同时还能够提升内存分配的吞吐量，因此我们可以将这种
内存分配方式称之为快速分配策略。
据我所知所有OpenJDK衍生出来的JVM都提供了TLAB的设计。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210220093653858.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
###### 3.5.9.8.3 TLAB 分配过程
```markdown
尽管不是所有的对象实例都能够在TLAB中成功分配内存，但JVM确
实是将TLAB作为内存分配的首选。
在程序中，开发人员可以通过选项“-XX:UseTLAB”设置是否开
启TLAB空间。
默认情况下，TLAB空间的内存非常小，仅占有整个Eden空间的1%，
当然我们可以通过选项“-XX:TLABWasteTargetPercent”
设置TLAB空间所占用Eden空间的百分比大小。
一旦对象在TLAB空间分配内存失败时，JVM就会尝试着通过使用加
锁机制确保数据操作的原子性，从而直接在Eden空间中分配内存。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210220093935476.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
/**
 * 测试-XX:UseTLAB参数是否开启的情况:默认情况是开启的
 */
public class TLABArgsTest {
    public static void main(String[] args) {
        System.out.println("我只是来打个酱油~");
        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

```
```markdown
查看 UseTLAB 标志位的状态
1  jps
2  jinfo -flag UseTLAB 38962

我并没有设置任何 JVM 参数，通过命令行查看 TLAB 是否
开启：结论是默认开启 TLAB的
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210220094221998.png)
##### 3.5.9.9 堆空间参数设置
```markdown
常用参数设置

-XX:+PrintFlagsInitial：查看所有的参数的默认初始值
-XX:+PrintFlagsFinal：查看所有的参数的最终值（可能会存在
修改，不再是初始值）
-Xms：初始堆空间内存（默认为物理内存的1/64）
-Xmx：最大堆空间内存（默认为物理内存的1/4）
-Xmn：设置新生代的大小（初始值及最大值）
-XX:NewRatio：配置新生代与老年代在堆结构的占比
-XX:SurvivorRatio：设置新生代中Eden和S0/S1空间的比例
-XX:MaxTenuringThreshold：设置新生代垃圾的最大年龄
-XX:+PrintGCDetails：输出详细的GC处理日志
-XX:+PrintGC 或 -verbose:gc ：打印gc简要信息
-XX:HandlePromotionFalilure：是否设置空间分配担保
```
##### 3.5.9.10 空间分配担保
```markdown
在发生Minor GC之前，虚拟机会检查老年代最大可用的连续空间是
否大于新生代所有对象的总空间。

如果大于，则此次Minor GC是安全的
如果小于，则虚拟机会查看-XX:HandlePromotionFailure设置
值是否允许担保失败。
如果HandlePromotionFailure=true，那么会继续检查老年代最
大可用连续空间是否大于历次晋升到老年代的对象的平均大小
如果大于，则尝试进行一次Minor GC，但这次Minor GC依然
是有风险的；
如果小于，则进行一次Full GC。
如果HandlePromotionFailure=false，则进行一次Full GC。

历史版本

在JDK6 Update 24之后 (JDK 7)，
HandlePromotionFailure参
数不会再影响到虚拟机的空间分配担保策略，观察openJDK中的
源码变化，虽然源码中还定义了HandlePromotionFailure参数，
但是在代码中已经不会再使用它。
JDK6 Update 24之后的规则变为只要老年代的连续空间大于
新生代对象总大小或者历次晋升的平均大小就会进行Minor GC，
否则将进行Full GC。即 HandlePromotionFailure=true
```
```java
/**
 * 测试堆空间常用的jvm参数：
 * -XX:+PrintFlagsInitial : 查看所有的参数的默认初始值
 * -XX:+PrintFlagsFinal  ：查看所有的参数的最终值（可能会存在修改，不再是初始值）
 * 具体查看某个参数的指令：
 *      jps：查看当前运行中的进程
 *      jinfo -flag SurvivorRatio 进程id
 * -Xms：初始堆空间内存 （默认为物理内存的1/64）
 * -Xmx：最大堆空间内存（默认为物理内存的1/4）
 * -Xmn：设置新生代的大小。(初始值及最大值)
 * -XX:NewRatio：配置新生代与老年代在堆结构的占比
 * -XX:SurvivorRatio：设置新生代中Eden和S0/S1空间的比例
 * -XX:MaxTenuringThreshold：设置新生代垃圾的最大年龄
 * -XX:+PrintGCDetails：输出详细的GC处理日志
 * 打印gc简要信息：① -XX:+PrintGC   ② -verbose:gc
 * -XX:HandlePromotionFailure：是否设置空间分配担保
 */
public class HeapArgsTest {
    public static void main(String[] args) {

    }
}

```
##### 3.5.9.11 对象存储
```markdown
堆是分配对象存储的唯一选择吗？

在《深入理解Java虚拟机》中关于Java堆内存有这样一段描述：

随着JIT编译期的发展与逃逸分析技术逐渐成熟，栈上分配、
标量替换优化技术将会导致一些微妙的变化，所有的对象都分配到
堆上也渐渐变得不那么“绝对”了。
在Java虚拟机中，对象是在Java堆中分配内存的，这是一个普
遍的常识。但是，有一种特殊情况，那就是如果经过逃逸分
析（Escape Analysis）后发现，一个对象并没有逃逸出方法
的话，那么就可能被优化成栈上分配。这样就无需在堆上分配内
存，也无须进行垃圾回收了。这也是最常见的堆外存储技术。
此外，前面提到的基于OpenJDK深度定制
的TaoBao VM( 淘宝虚拟机 )，其中创新的
GCIH（GC invisible heap）技术实现off-heap，将生命周期
较长的Java对象从heap中移至heap外，并且GC不能管
理GCIH内部的Java对象，以此达到降低GC的回收频率和提
升GC的回收效率的目的。
```
##### 3.5.9.12 逃逸分析
```markdown
如何将堆上的对象分配到栈，需要使用逃逸分析手段。

这是一种可以有效减少Java程序中同步负载和内存堆分配
压力的跨函数全局数据流分析算法。
通过逃逸分析，Java Hotspot编译器能够分析出一个新的
对象的引用的使用范围从而决定是否要将这个对象分配到堆上。
逃逸分析的基本行为就是分析对象动态作用域：
当一个对象在方法中被定义后，对象只在方法内部使用，则
认为没有发生逃逸。
当一个对象在方法中被定义后，它被外部方法所引用，则
认为发生逃逸。例如作为调用参数传递到其他地方中。
```
```java
1 没有发生逃逸的对象，则可以分配到栈上，随着方法执行的结束，栈空间就被移除
public void my_method() {
    V v = new V();
    // use v
    // ....
    v = null;
}

2 下面代码中的 StringBuffer sb 发生了逃逸
public static StringBuffer createStringBuffer(String s1, String s2) {
    StringBuffer sb = new StringBuffer();
    sb.append(s1);
    sb.append(s2);
    return sb;
}


3 如果想要StringBuffer sb不发生逃逸，可以这样写
public static String createStringBuffer(String s1, String s2) {
    StringBuffer sb = new StringBuffer();
    sb.append(s1);
    sb.append(s2);
    return sb.toString();
}


4 逃逸分析的举例
/**
 * 逃逸分析
 *
 * 如何快速的判断是否发生了逃逸分析，大家就看new的对象实体是否有可能在方法外被调用。
 */
public class EscapeAnalysis {

    public EscapeAnalysis obj;

    /*
    方法返回EscapeAnalysis对象，发生逃逸
     */
    public EscapeAnalysis getInstance(){
        return obj == null? new EscapeAnalysis() : obj;
    }

    /*
    为成员属性赋值，发生逃逸
     */
    public void setObj(){
        this.obj = new EscapeAnalysis();
    }
    //思考：如果当前的obj引用声明为static的？ 仍然会发生逃逸。

    /*
    对象的作用域仅在当前方法中有效，没有发生逃逸
     */
    public void useEscapeAnalysis(){
        EscapeAnalysis e = new EscapeAnalysis();
    }

    /*
    引用成员变量的值，发生逃逸
     */
    public void useEscapeAnalysis1(){
        EscapeAnalysis e = getInstance(); //这个e对象，本身就是从外面的方法逃逸进来的
        //getInstance().xxx()同样会发生逃逸
    }
}


5 逃逸分析参数设置
在JDK 1.7 版本之后，HotSpot中默认就已经开启了逃逸分析
如果使用的是较早的版本，开发人员则可以通过：
选项“-XX:+DoEscapeAnalysis"显式开启逃逸分析
通过选项“-XX:+PrintEscapeAnalysis"查看逃逸分析的筛选结果

结论
开发中能使用局部变量的，就不要使用在方法外定义。

6 逃逸分析之代码优化
使用逃逸分析，编译器可以对代码做如下优化：

栈上分配：将堆分配转化为栈分配。如果一个对象在子
程序中被分配，要使指向该对象的指针永远不会发生逃
逸，对象可能是栈上分配的候选，而不是堆上分配
同步省略：如果一个对象被发现只有一个线程被访问
到，那么对于这个对象的操作可以不考虑同步。
分离对象或标量替换：有的对象可能不需要作为一个
连续的内存结构存在也可以被访问到，那么对象的
部分（或全部）可以不存储在内存，而是存储在CPU寄存器中
```
###### 3.5.9.12.1 栈上分配
```markdown
JIT编译器在编译期间根据逃逸分析的结果，发现如果一个对
象并没有逃逸出方法的话，就可能被优化成栈上分配。
分配完成后，继续在调用栈内执行，最后线程结束，栈空间被
回收，局部变量对象也被回收。这样就无须进行垃圾回收了。
常见的栈上分配的场景：
在逃逸分析中，已经说明了，分别是给成员变量赋值、方法
返回值、实例引用传递。
```
```java
/**
 * 栈上分配测试
 * -Xmx256m -Xms256m -XX:-DoEscapeAnalysis -XX:+PrintGCDetails
 */
public class StackAllocation {
    public static void main(String[] args) {
        long start = System.currentTimeMillis();

        for (int i = 0; i < 10000000; i++) {
            alloc();
        }
        // 查看执行时间
        long end = System.currentTimeMillis();
        System.out.println("花费的时间为： " + (end - start) + " ms");
        // 为了方便查看堆内存中对象个数，线程sleep
        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e1) {
            e1.printStackTrace();
        }
    }

    private static void alloc() {
        User user = new User(); //未发生逃逸
    }

    static class User {

    }
}

```
```markdown
未开启逃逸分析的情况

JVM 参数设置
1 -Xmx256m -Xms256m -XX:-DoEscapeAnalysis -XX:+PrintGCDetails

日志打印：发生了 GC ，耗时 74ms
[GC (Allocation Failure) [PSYoungGen: 65536K->560K(76288K)] 65536K->568K(251392K), 0.0017179 secs] [Times: user=0.01 sys=0.00, real=0.00 secs] 
[GC (Allocation Failure) [PSYoungGen: 66096K->464K(76288K)] 66104K->480K(251392K), 0.0017602 secs] [Times: user=0.00 sys=0.00, real=0.01 secs] 
花费的时间为： 74 ms

```
```markdown
堆上面有好多 User 对象
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210220142326184.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
开启逃逸分析的情况

JVM 参数设置
-Xmx256m -Xms256m -XX:+DoEscapeAnalysis 
-XX:+PrintGCDetails

日志打印：并没有发生 GC ，耗时 3ms ，栈上分配是真的快啊
花费的时间为： 4 ms
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210220142435118.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

###### 3.5.9.12.2 同步省略
```java
线程同步的代价是相当高的，同步的后果是降低并发性和性能。

在动态编译同步块的时候，JIT编译器可以借助逃逸分析来
判断同步块所使用的锁对象是否只能够被一个线程访问而没有
被发布到其他线程。

如果没有，那么JIT编译器在编译这个同步块的时候就会取
消对这部分代码的同步。这样就能大大提高并发性和性能。这
个取消同步的过程就叫同步省略，也叫锁消除。

例如下面的智障代码，根本起不到锁的作用
public void f() {
    Object hellis = new Object();
    synchronized(hellis) {
        System.out.println(hellis);
    }
}

代码中对hellis这个对象加锁，但是hellis对象的生命周期只在f( )方法中，并不会被其他线程所访问到，所以在JIT编译阶段就会被优化掉，优化成：
public void f() {
  	Object hellis = new Object();
		System.out.println(hellis);
}
```
```java
/**
 * 同步省略说明
 * @author xiexu
 * @create 2020-11-27 7:01 下午
 */
public class SynchronizedTest {
    public void f() {
        Object hellis = new Object();
        synchronized(hellis) {
            System.out.println(hellis);
        }
    }
}

```
```markdown
注意：字节码文件中并没有进行优化，可以看到加锁和释放
锁的操作依然存在，同步省略操作是在解释运行时发生的
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210220142645804.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
###### 3.5.9.12.3 分离对象或标量替换
```markdown
标量（scalar）是指一个无法再分解成更小的数据的数据。Java
中的原始数据类型就是标量。
相对的，那些还可以分解的数据叫做聚合量（Aggregate），
Java中的对象就是聚合量，因为他可以分解成其他聚合量和标量。
在JIT阶段，如果经过逃逸分析，发现一个对象不会被外界访
问的话，那么经过JIT优化，就会把这个对象拆解成若干个其
中包含的若干个成员变量来代替。这个过程就是标量替换。
```
```java
public static void main(String args[]) {
    alloc();
}
class Point {
    private int x;
    private int y;
}
private static void alloc() {
    Point point = new Point(1,2);
    System.out.println("point.x" + point.x + ";point.y" + point.y);
}

以上代码，经过标量替换后，就会变成
private static void alloc() {
    int x = 1;
    int y = 2;
    System.out.println("point.x = " + x + "; point.y=" + y);
}


结论：

可以看到，Point这个聚合量经过逃逸分析后，发现他并没
有逃逸，就被替换成两个聚合量了。
那么标量替换有什么好处呢？就是可以大大减少堆内存的占
用。因为一旦不需要创建对象了，那么就不再需要分配堆内存了。
标量替换为栈上分配提供了很好的基础。

标量替换参数设置
参数 -XX:+ElimilnateAllocations：开启了标量
替换（默认打开），允许将对象打散分配在栈上。
```
```java
/**
 * 标量替换测试
 * -Xmx100m -Xms100m -XX:+DoEscapeAnalysis -XX:+PrintGC -XX:-EliminateAllocations
 */
public class ScalarReplace {
    public static class User {
        public int id;
        public String name;
    }

    public static void alloc() {
        User u = new User(); //未发生逃逸
        u.id = 5;
        u.name = "www.baidu.com";
    }

    public static void main(String[] args) {
        long start = System.currentTimeMillis();
        for (int i = 0; i < 10000000; i++) {
            alloc();
        }
        long end = System.currentTimeMillis();
        System.out.println("花费的时间为： " + (end - start) + " ms");
    }
}

```
```markdown
未开启标量替换

JVM 参数
-Xmx100m -Xms100m -XX:+DoEscapeAnalysis -XX:+PrintGC -XX:-EliminateAllocations

日志分析：伴随着 GC 的垃圾回收，用时 84ms

[GC (Allocation Failure)  25600K->536K(98304K), 0.0021681 secs]
[GC (Allocation Failure)  26136K->536K(98304K), 0.0019547 secs]
[GC (Allocation Failure)  26136K->472K(98304K), 0.0016708 secs]
[GC (Allocation Failure)  26072K->536K(98304K), 0.0016899 secs]
[GC (Allocation Failure)  26136K->584K(98304K), 0.0018258 secs]
[GC (Allocation Failure)  26184K->568K(101376K), 0.0015689 secs]
[GC (Allocation Failure)  32312K->461K(101376K), 0.0015208 secs]
[GC (Allocation Failure)  32205K->461K(101376K), 0.0010466 secs]
花费的时间为： 84 ms


开启标量替换

JVM 参数
-Xmx100m -Xms100m -XX:+DoEscapeAnalysis -XX:+PrintGC -XX:+EliminateAllocations

日志分析：无垃圾回收，用时 5ms
花费的时间为： 5 ms

逃逸分析参数设置总结

上述代码在主函数中调用了1亿次alloc( )方法，进行对象创建
由于User对象实例需要占据约16字节的空间，因此累计分配
空间达到将近1.5GB。
如果堆空间小于这个值，就必然会发生GC。使用如下参数
运行上述代码：
-server -Xmx100m -Xms100m -XX:+DoEscapeAnalysis -XX:+PrintGC -XX:+EliminateAllocations

这里设置参数如下：

参数 -server：启动Server模式，因为在server模式下，才可以启
用逃逸分析。
参数 -XX:+DoEscapeAnalysis：启用逃逸分析
参数 -Xmx10m：指定了堆空间最大为10MB
参数 -XX:+PrintGC：将打印GC日志。
参数 -XX:+EliminateAllocations：开启了标量替换（默认打开），允
许将对象打散分配在栈上，比如对象拥有id和name两个字段，那
么这两个字段将会被视为两个独立的局部变量进行分配

逃逸分析的不足

关于逃逸分析的论文在1999年就已经发表了，但直到JDK1.6才有
实现，而且这项技术到如今也并不是十分成熟的。
其根本原因就是无法保证逃逸分析的性能消耗一定能高于他的消
耗。虽然经过逃逸分析可以做标量替换、栈上分配、和锁消
除。但是逃逸分析自身也是需要进行一系列复杂的分析的，这
其实也是一个相对耗时的过程。一个极端的例子，就是经过
逃逸分析之后，发现没有一个对象是不逃逸的。那这个逃逸分
析的过程就白白浪费掉了。
虽然这项技术并不十分成熟，但是它也是即时编译器优化技
术中一个十分重要的手段。注意到有一些观点，认为通过
逃逸分析，JVM会在栈上分配那些不会逃逸的对象，这在
理论上是可行的，但是取决于JVM设计者的选择。
据我所知，Oracle Hotspot JVM中并未这么做，这一点在
逃逸分析相关的文档里已经说明，所以可以明确所有的
对象实例都是创建在堆上。
Oracle Hotspot JVM是通过标量替换实现逃逸分析的
目前很多书籍还是基于JDK7以前的版本，JDK已经发
生了很大变化，intern字符串的缓存和静态变量曾经都
被分配在永久代上，而永久代已经被元数据区取代。但
是intern字符串缓存和静态变量并不是被转移到元数据
区，而是直接在堆上分配，所以这一点同样符合前面
一点的结论：对象实例都是分配在堆上。
```
##### 3.5.9.13 堆小结
```markdown
轻代是对象的诞生、成长、消亡的区域，一个对象在这里产生、
应用，最后被垃圾回收器收集、结束生命。
老年代放置长生命周期的对象，通常都是从Survivor区域
筛选拷贝过来的Java对象。
当然，也有特殊情况，我们知道普通的对象可能会被分配在TLAB上
如果对象较大，无法分配在 TLAB 上，则JVM会试图直接
分配在Eden其他位置上
如果对象太大，完全无法在新生代找到足够长的连续空闲空
间，JVM就会直接分配到老年代
当GC只发生在年轻代中，回收年轻代对象的行为被称为Minor GC
当GC发生在老年代时则被称为Major GC或者Full GC
一般的，Minor GC的发生频率要比Major GC高很多，即老年代
中垃圾回收发生的频率将大大低于年轻代
```
### 3.6 方法区
#### 3.6.1 栈、堆、方法区的交互关系
```markdown
从内存结构看
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210223084659543.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
从线程共享与否的角度看
ThreadLocal：如何保证多个线程在并发环境下的安全性？典
型应用就是数据库连接管理，以及独立会话管理
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210223085024122.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
栈、堆、方法区的交互关系
Person 类的 .class 信息存放在方法区中
person 变量存放在 Java 栈的局部变量表中
真正的 person 对象存放在 Java 堆中
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210223085112596.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
在 person 对象中，有个指针指向方法区中的 person 类型数据，
表明这个 person 对象是用方法区中的 Person 类 new 出来的
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210223085240923.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 3.6.2 方法区的理解
```markdown
Java虚拟机有一个在所有Java虚拟机线程之间共享的方法区域。
方法区域类似于用于传统语言的编译代码的存储区域，或
者类似于操作系统进程中的“文本”段。它存储每个类的结
构，例如运行时常量池、字段和方法数据，以及方法和构造函
数的代码，包括类和实例初始化以及接口初始化中使用的
特殊方法
方法区域是在虚拟机启动时创建的。尽管方法区域在逻辑上
是堆的一部分，但简单的实现可能选择不垃圾收集或压缩
它。此规范不强制指定方法区域的位置或用于管理已编译代码
的策略。方法区域可以具有固定的大小，或者可以根据计算的
需要进行扩展，并且如果不需要更大的方法区域，则可以
收缩。方法区域的内存不需要是连续的。
Java虚拟机实现可提供程序员或用户对方法区域的初始大
小的控制，以及在大小可变的方法区域的情况下，对最大
和最小方法区域大小的控制。
以下例外情况与方法区域相关：
如果方法区域中的内存无法满足分配请求，Java虚拟
机将抛出OutOfMemoryError。
```
#### 3.6.3 方法区的位置
```markdown
《Java虚拟机规范》中明确说明：尽管所有的方法区在逻辑上
是属于堆的一部分，但一些简单的实现可能不会选择去进
行垃圾收集或者进行压缩。
但对于HotSpotJVM而言，方法区还有一个别名叫做
Non-Heap（非堆），目的就是要和堆分开。
所以，方法区可以看作是一块独立于Java堆的内存空间。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210223090028115.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 3.6.4 方法区的理解
```markdown
方法区主要存放的是 Class，而堆中主要存放的是实例化的对象

方法区（Method Area）与Java堆一样，是各个线程共享的内存区域
多个线程同时加载统一个类时，只能有一个线程能加载该类，其他
线程只能等等待该线程加载完毕，然后直接使用该类，即类只能加
载一次。
方法区在JVM启动的时候被创建，并且它的实际物理内存空间和
Java堆区一样都可以是不连续的。
方法区的大小，跟堆空间一样，可以选择固定大小或者可扩展。
方法区是接口，元空间或者永久代是方法区的实现
方法区的大小决定了系统可以保存多少个类，如果系统定义了太多
的类，导致方法区溢出，虚拟机同样会抛出内存溢出错误：
java.lang.OutofMemoryError:PermGen space（JDK7之前）
或者
java.lang.OutOfMemoryError:Metaspace（JDK8之后）
举例说明方法区 OOM
加载大量的第三方的jar包
Tomcat部署的工程过多（30~50个）
大量动态的生成反射类
关闭JVM就会释放这个区域的内存。
```
```java
/**
 * -Xms600m -Xmx600m
 */
public class EdenSurvivorTest {
    public static void main(String[] args) {
        System.out.println("我只是来打个酱油~");
        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

```
```markdown
简单的程序，加载了好多类
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210223090538873.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
可以看到设置堆内存为600M后，年轻代+老年代=600M，所
以说方法区是不存在堆中的
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210223090632367.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 3.6.5 Hotspot中方法区的演进过程
```markdown
在 JDK7 及以前，习惯上把方法区，称为永久代。JDK8开始，
使用元空间取代了永久代。JDK 1.8之后，元空间存放在堆外内存中
我们可以将方法区类比为Java中的接口，将永久代或元空间类比
为Java中具体的实现类
本质上，方法区和永久代并不等价。仅是对Hotspot而言的可以看
作等价。《Java虚拟机规范》对如何实现方法区，不做统一要求。
例如：BEAJRockit / IBM J9 中不存在永久代的概念。
现在来看，当年使用永久代，不是好的idea。导致Java程序更容
易OOm（超过-XX:MaxPermsize上限）
而到了JDK8，终于完全废弃了永久代的概念，改用与JRockit、
J9一样在本地内存中实现的元空间（Metaspace）来代替
元空间的本质和永久代类似，都是对JVM规范中方法区的实
现。不过元空间与永久代最大的区别在于：元空间不在虚拟
机设置的内存中，而是使用本地内存
永久代、元空间二者并不只是名字变了，内部结构也调整了
根据《Java虚拟机规范》的规定，如果方法区无法满足新的
内存分配需求时，将抛出OOM异常
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210223090845938.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210223090856739.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 3.6.6 设置方法区大小与OOM
```markdown
方法区的大小不必是固定的，JVM可以根据应用的需要动态调整
```
##### 3.6.6.1 JDK7 永久代
```markdown
通过-XX:Permsize来设置永久代初始分配空间。默认值是20.75M
-XX:MaxPermsize来设定永久代最大可分配空间。32位机器默
认是64M，64位机器模式是82M
当JVM加载的类信息容量超过了这个值，会报异常
OutofMemoryError:PermGen space。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210223092128583.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 3.6.6.2 JDK8 元空间
```markdown
元数据区大小可以使用参数 -XX:MetaspaceSize 和 
-XX:MaxMetaspaceSize 指定
默认值依赖于平台，Windows下，-XX:MetaspaceSize 约为21M，
-XX:MaxMetaspaceSize的值是-1，即没有限制。
与永久代不同，如果不指定大小，默认情况下，虚拟机会耗尽所
有的可用系统内存。如果元数据区发生溢出，虚拟机一样会抛
出异常OutOfMemoryError:Metaspace
-XX:MetaspaceSize：设置初始的元空间大小。对于一个
64位 的服务器端 JVM 来说，其默
认的-XX:MetaspaceSize值为21MB。这就是初始的高水位线，
一旦触及这个水位线，Full GC将会被触发并卸载没用的类
（即这些类对应的类加载器不再存活），然后这个高水位线将会
重置。新的高水位线的值取决于GC后释放了多少元空间。
如果释放的空间不足，那么在不超过MaxMetaspaceSize时，适
当提高该值。
如果释放空间过多，则适当降低该值。
如果初始化的高水位线设置过低，上述高水位线调整情况
会发生很多次。通过垃圾回收器的日志可以观察到Full GC多
次调用。为了避免频繁地GC，建议将-XX:MetaspaceSize设置为
一个相对较高的值。
```
```java
/**
 * 测试设置方法区大小参数的默认值
 *
 * jdk7及以前：
 * -XX:PermSize=100m -XX:MaxPermSize=100m
 *
 * jdk8及以后：
 * -XX:MetaspaceSize=100m  -XX:MaxMetaspaceSize=100m
 */
public class MethodAreaDemo {
    public static void main(String[] args) {
        System.out.println("start...");
        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("end...");
    }
}

```
```markdown
JVM参数
-XX:MetaspaceSize=100m  -XX:MaxMetaspaceSize=100m

终端命令查看设置的元空间大小
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210223092844177.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

#### 3.6.7 方法区OOM
```markdown
OOMTest 类继承 ClassLoader 类，获得 defineClass() 方法，可
自己进行类的加载
```
```java
/**
 * jdk6/7中：
 * -XX:PermSize=10m -XX:MaxPermSize=10m
 *
 * jdk8中：
 * -XX:MetaspaceSize=10m -XX:MaxMetaspaceSize=10m
 */
public class OOMTest extends ClassLoader {
    public static void main(String[] args) {
        int j = 0;
        try {
            OOMTest test = new OOMTest();
            for (int i = 0; i < 10000; i++) {
                //创建ClassWriter对象，用于生成类的二进制字节码
                ClassWriter classWriter = new ClassWriter(0);
                //指明版本号，修饰符，类名，包名，父类，接口
                classWriter.visit(Opcodes.V1_8, Opcodes.ACC_PUBLIC, "Class" + i, null, "java/lang/Object", null);
                //返回byte[]
                byte[] code = classWriter.toByteArray();
                //类的加载
                test.defineClass("Class" + i, code, 0, code.length); //Class对象
                j++;
            }
        } finally {
            System.out.println(j);
        }
    }
}

```
```markdown
不设置元空间的上限
使用默认的 JVM 参数，元空间不设置上限
10000

设置元空间的上限

JVM 参数
-XX:MetaspaceSize=10m -XX:MaxMetaspaceSize=10m
元空间出现 OOM
3331
Exception in thread "main" java.lang.OutOfMemoryError: Compressed class space
	at java.lang.ClassLoader.defineClass1(Native Method)
	at java.lang.ClassLoader.defineClass(ClassLoader.java:756)
	at java.lang.ClassLoader.defineClass(ClassLoader.java:635)
	at cn.sxt.java.OOMTest.main(OOMTest.java:26)

```
#### 3.6.8 如何解决OOM
```markdown
要解决OOM异常或heap space的异常，一般的手段是首先通
过内存映像分析工具（如Eclipse Memory Analyzer）对dump出
来的堆转储快照进行分析，重点是确认内存中的对象是否是
必要的，也就是要先分清楚到底是出现了内存泄漏（Memory Leak）
还是内存溢出（Memory Overflow）
内存泄漏就是有大量的引用指向某些对象，但是这些对象以
后不会使用了，但是因为它们还和GC ROOT有关联，所以导致
以后这些对象也不会被回收，这就是内存泄漏的问题
如果是内存泄漏，可进一步通过工具查看泄漏对象到GC Roots的
引用链。于是就能找到泄漏对象是通过怎样的路径与GC Roots相
关联并导致垃圾收集器无法自动回收它们的。掌握了泄漏对象的
类型信息，以及GC Roots引用链的信息，就可以比较准确地定位
出泄漏代码的位置。
如果不存在内存泄漏，换句话说就是内存中的对象确实都还必须存
活着，那就应当检查虚拟机的堆参数（-Xmx与-Xms），与机器
物理内存对比看是否还可以调大，从代码上检查是否存在某些对
象生命周期过长、持有状态时间过长的情况，尝试减少程序运
行期的内存消耗。
```
#### 3.6.9 方法区的内部结构
##### 3.6.9.1 方法区结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224084112734.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
《深入理解Java虚拟机》书中对方法区（Method Area）存储内
容描述如下：它用于存储已被虚拟机加载的类型信息、常量、
静态变量、即时编译器编译后的代码缓存等。
```
```markdown
类型信息
对每个加载的类型（类class、接口interface、枚举enum、
注解annotation），JVM必须在方法区中存储以下类型信息：
这个类型的完整有效名称（全类名=包名.类名）
这个类型直接父类的完整有效名（对于interface或是
java.lang.Object，都没有父类）
这个类型的修饰符（public，abstract，final的某个子集）
这个类型直接接口的一个有序列表

域（Field）信息
JVM必须在方法区中保存类型的所有域的相关信息以及域的声明顺序。
域信息通俗来讲是类的成员变量
域的相关信息包括：
域名称
域类型
域修饰符（public，private，protected，static，final，
volatile，transient的某个子集）


方法（Method）信息
JVM必须保存所有方法的以下信息，同域信息一样包括声明顺序：

方法名称
方法的返回类型（包括 void 返回类型），void 在Java中对应的
类为 void.class
方法参数的数量和类型（按顺序）
方法的修饰符（public，private，protected，static，
final，synchronized，native，abstract的一个子集）
方法的字节码（bytecodes）、操作数栈、局部变量表及大小
（abstract和native方法除外）
异常表（abstract和native方法除外），异常表记录每个异常处理
的开始位置、结束位置、代码处理在程序计数器中的偏移地址、被
捕获的异常类的常量池索引
```
```java
/**
 * 测试方法区的内部构成
 */
public class MethodInnerStrucTest extends Object implements Comparable<String>, Serializable {
    //属性
    public int num = 10;
    private static String str = "测试方法的内部结构";

    //构造器没写

    //方法
    public void test1() {
        int count = 20;
        System.out.println("count = " + count);
    }

    public static int test2(int cal) {
        int result = 0;
        try {
            int value = 30;
            result = value / cal;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return result;
    }

    @Override
    public int compareTo(String o) {
        return 0;
    }
}

```
```markdown
反编译字节码文件，并输出到文本文件中，便于查看
参数 -p 确保能查看 private 权限类型的字段或方法
javap -v -p MethodInnerStrucTest.class > Text.txt
```
```java
类型信息
在运行时方法区中，类信息中记录了哪个加载器加载了该
类，同时类加载器也记录了它加载了哪些类
从反编译文件可以看出，字节码文件记录了 
MethodInnerStrucTest 继承了哪些类，实现了哪些方法
//类型信息
public class cn.sxt.java.MethodInnerStrucTest extends java.lang.Object
implements java.lang.Comparable<java.lang.String>, java.io.Serializable

域信息
descriptor: I 表示字段类型为 Integer
flags: ACC_PUBLIC 表示字段权限修饰符为 public
//域信息
public int num;
descriptor: I
flags: ACC_PUBLIC

private static java.lang.String str;
descriptor: Ljava/lang/String;
flags: ACC_PRIVATE, ACC_STATIC

方法信息
descriptor: ( )V 表示方法返回值类型为 void
flags: ACC_PUBLIC 表示方法权限修饰符为 public
stack=3 表示操作数栈深度为 3
locals=2 表示局部变量个数为 2 个（实力方法包含 this）
test1( ) 方法虽然没有参数，但是其 args_size=1 ，这是因为将 this 作为了参数
public void test1();
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=3, locals=2, args_size=1
         0: bipush        20
         2: istore_1
         3: getstatic     #3                  // Field java/lang/System.out:Ljava/io/PrintStream;
         6: new           #4                  // class java/lang/StringBuilder
         9: dup
        10: invokespecial #5                  // Method java/lang/StringBuilder."<init>":()V
        13: ldc           #6                  // String count =
        15: invokevirtual #7                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
        18: iload_1
        19: invokevirtual #8                  // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
        22: invokevirtual #9                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
        25: invokevirtual #10                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        28: return
      LineNumberTable:
        line 17: 0
        line 18: 3
        line 19: 28
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      29     0  this   Lcn/sxt/java/MethodInnerStrucTest;
            3      26     1 count   I
```
#### 3.6.10 域信息特殊情况
```markdown
non-final 类型的类变量
静态变量和类关联在一起，随着类的加载而加载，他们成为类数
据在逻辑上的一部分
类变量被类的所有实例共享，即使没有类实例时，你也可以访问它
```
```java
如下代码所示，即使我们把order设置为null，也不会出现空指针异常
这更加表明了 static 类型的字段和方法随着类的加载而加载，并不属于特定的类实例

/**
 * non-final的类变量
 */
public class MethodAreaTest {
    public static void main(String[] args) {
        Order order = null;
        order.hello();
        System.out.println(order.count);
    }
}

class Order {
    public static int count = 1;
    public static final int number = 2;


    public static void hello() {
        System.out.println("hello!");
    }
}

程序运行结果
hello!
1

全局常量：static final

全局常量就是使用 static final 进行修饰
被声明为final的类变量的处理方法则不同，每个全局常
量在编译的时候就会被分配了。
class Order {
    public static int count = 1;
    public static final int number = 2;


    public static void hello() {
        System.out.println("hello!");
    }
}

反编译，查看字节码指令，可以发现 number 的值已经写死在
字节码文件中了
  public static int count;
    descriptor: I
    flags: ACC_PUBLIC, ACC_STATIC

  public static final int number;
    descriptor: I
    flags: ACC_PUBLIC, ACC_STATIC, ACC_FINAL
    ConstantValue: int 2



```
#### 3.6.11 运行时常量池
```markdown
运行时常量池 VS 常量池

方法区，内部包含了运行时常量池
字节码文件，内部包含了常量池
要弄清楚方法区，需要理解清楚ClassFile，因为加载类的
信息都在方法区。
要弄清楚方法区的运行时常量池，需要理解清楚ClassFile中
的常量池。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224090236448.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
常量池
一个有效的字节码文件中除了包含类的版本信息、字段、方法以
及接口等描述符信息外
还包含一项信息就是常量池表（Constant Pool Table），包括各
种字面量和对类型、域和方法的符号引用
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224090654946.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
为什么需要常量池？
一个java源文件中的类、接口，编译后产生一个字节码文件。而
Java中的字节码需要数据支持，通常这种数据会很大以至于不能
直接存到字节码里，换另一种方式，可以存到常量池
这个字节码包含了指向常量池的引用。在动态链接的时候会用
到运行时常量池，之前有介绍
比如：如下的代码：
public class SimpleClass {
    public void sayHello() {
        System.out.println("hello");
    }
}

虽然上述代码只有194字节，但是里面却使用了
String、System、PrintStream及Object等结构。
如果不使用常量池，就需要将用到的类信息、方法信
息等记录在当前的字节码文件中，造成文件臃肿
所以我们将所需用到的结构信息记录在常量池中，并通过
引用的方式，来加载、调用所需的结构
这里的代码量其实很少了，如果代码多的话，引用的结构将
会更多，这里就需要用到常量池了。
```
```markdown
常量池中有什么？
数量值
字符串值
类引用
字段引用
方法引用
```
```java
/**
 * 测试方法区的内部构成
 */
public class MethodInnerStrucTest extends Object implements Comparable<String>, Serializable {
    //属性
    public int num = 10;
    private static String str = "测试方法的内部结构";

    //构造器没写

    //方法
    public void test1() {
        int count = 20;
        System.out.println("count = " + count);
    }

    public static int test2(int cal) {
        int result = 0;
        try {
            int value = 30;
            result = value / cal;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return result;
    }

    @Override
    public int compareTo(String o) {
        return 0;
    }
}

```
```java
来看下最简单的 test1( ) 方法，带 # 的字节码指令，就
使用到了常量池的引用
通过字节码指令可以看出，拼接字符串时，编译器帮我们造
了个 StringBuilder 对象，然后调用其 append( ) 方法
完成了字符串的拼接


public void test1();
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=3, locals=2, args_size=1
         0: bipush        20
         2: istore_1
         3: getstatic     #3                  // Field java/lang/System.out:Ljava/io/PrintStream;
         6: new           #4                  // class java/lang/StringBuilder
         9: dup
        10: invokespecial #5                  // Method java/lang/StringBuilder."<init>":()V
        13: ldc           #6                  // String count =
        15: invokevirtual #7                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
        18: iload_1
        19: invokevirtual #8                  // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
        22: invokevirtual #9                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
        25: invokevirtual #10                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        28: return
      LineNumberTable:
        line 17: 0
        line 18: 3
        line 19: 28
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      29     0  this   Lcn/sxt/java/MethodInnerStrucTest;
            3      26     1 count   I


常量池
Constant pool:
   #1 = Methodref          #18.#52        // java/lang/Object."<init>":()V
   #2 = Fieldref           #17.#53        // cn/sxt/java/MethodInnerStrucTest.num:I
   #3 = Fieldref           #54.#55        // java/lang/System.out:Ljava/io/PrintStream;
   #4 = Class              #56            // java/lang/StringBuilder
   #5 = Methodref          #4.#52         // java/lang/StringBuilder."<init>":()V
   #6 = String             #57            // count =
   #7 = Methodref          #4.#58         // java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
   #8 = Methodref          #4.#59         // java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
   #9 = Methodref          #4.#60         // java/lang/StringBuilder.toString:()Ljava/lang/String;
  #10 = Methodref          #61.#62        // java/io/PrintStream.println:(Ljava/lang/String;)V
  #11 = Class              #63            // java/lang/Exception
  #12 = Methodref          #11.#64        // java/lang/Exception.printStackTrace:()V
  #13 = Class              #65            // java/lang/String
  #14 = Methodref          #17.#66        // cn/sxt/java/MethodInnerStrucTest.compareTo:(Ljava/lang/String;)I
  #15 = String             #67            // 测试方法的内部结构
  #16 = Fieldref           #17.#68        // cn/sxt/java/MethodInnerStrucTest.str:Ljava/lang/String;
  #17 = Class              #69            // cn/sxt/java/MethodInnerStrucTest
  #18 = Class              #70            // java/lang/Object
  #19 = Class              #71            // java/lang/Comparable
  #20 = Class              #72            // java/io/Serializable
  #21 = Utf8               num
  #22 = Utf8               I
  #23 = Utf8               str
  #24 = Utf8               Ljava/lang/String;
  #25 = Utf8               <init>
  #26 = Utf8               ()V
  #27 = Utf8               Code
  #28 = Utf8               LineNumberTable
  #29 = Utf8               LocalVariableTable
  #30 = Utf8               this
  #31 = Utf8               Lcn/sxt/java/MethodInnerStrucTest;
  #32 = Utf8               test1
  #33 = Utf8               count
  #34 = Utf8               test2
  #35 = Utf8               (I)I
  #36 = Utf8               value
  #37 = Utf8               e
  #38 = Utf8               Ljava/lang/Exception;
  #39 = Utf8               cal
  #40 = Utf8               result
  #41 = Utf8               StackMapTable
  #42 = Class              #63            // java/lang/Exception
  #43 = Utf8               compareTo
  #44 = Utf8               (Ljava/lang/String;)I
  #45 = Utf8               o
  #46 = Utf8               (Ljava/lang/Object;)I
  #47 = Utf8               <clinit>
  #48 = Utf8               Signature
  #49 = Utf8               Ljava/lang/Object;Ljava/lang/Comparable<Ljava/lang/String;>;Ljava/io/Serializable;
  #50 = Utf8               SourceFile
  #51 = Utf8               MethodInnerStrucTest.java
  #52 = NameAndType        #25:#26        // "<init>":()V
  #53 = NameAndType        #21:#22        // num:I
  #54 = Class              #73            // java/lang/System
  #55 = NameAndType        #74:#75        // out:Ljava/io/PrintStream;
  #56 = Utf8               java/lang/StringBuilder
  #57 = Utf8               count =
  #58 = NameAndType        #76:#77        // append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
  #59 = NameAndType        #76:#78        // append:(I)Ljava/lang/StringBuilder;
  #60 = NameAndType        #79:#80        // toString:()Ljava/lang/String;
  #61 = Class              #81            // java/io/PrintStream
  #62 = NameAndType        #82:#83        // println:(Ljava/lang/String;)V
  #63 = Utf8               java/lang/Exception
  #64 = NameAndType        #84:#26        // printStackTrace:()V
  #65 = Utf8               java/lang/String
  #66 = NameAndType        #43:#44        // compareTo:(Ljava/lang/String;)I
  #67 = Utf8               测试方法的内部结构
  #68 = NameAndType        #23:#24        // str:Ljava/lang/String;
  #69 = Utf8               cn/sxt/java/MethodInnerStrucTest
  #70 = Utf8               java/lang/Object
  #71 = Utf8               java/lang/Comparable
  #72 = Utf8               java/io/Serializable
  #73 = Utf8               java/lang/System
  #74 = Utf8               out
  #75 = Utf8               Ljava/io/PrintStream;
  #76 = Utf8               append
  #77 = Utf8               (Ljava/lang/String;)Ljava/lang/StringBuilder;
  #78 = Utf8               (I)Ljava/lang/StringBuilder;
  #79 = Utf8               toString
  #80 = Utf8               ()Ljava/lang/String;
  #81 = Utf8               java/io/PrintStream
  #82 = Utf8               println
  #83 = Utf8               (Ljava/lang/String;)V
  #84 = Utf8               printStackTrace
```
```markdown
常量池总结
常量池，可以看做是一张表，虚拟机指令根据这张常量表找到要
执行的类名、方法名、参数类型、字面量等信息
```
```markdown
运行时常量池
运行时常量池（Runtime Constant Pool）是方法区的一部分。
常量池表（Constant Pool Table）是Class字节码文件的一部分，
用于存放编译期生成的各种字面量与符号引用，这部分内容将在
类加载后存放到方法区的运行时常量池中。
运行时常量池，在加载类和接口到虚拟机后，就会创建对应的
运行时常量池。
JVM为每个已加载的类型（类或接口）都维护一个常量池。池
中的数据项像数组项一样，是通过索引访问的。
运行时常量池中包含多种不同的常量，包括编译期就已经明确
的数值字面量，也包括到运行期解析后才能够获得的方法或者
字段引用。此时不再是常量池中的符号地址了，这里换为真实地址。
运行时常量池，相对于Class文件常量池的另一重要特征是：
具备动态性。
运行时常量池类似于传统编程语言中的符号表（symbol table），
但是它所包含的数据却比符号表要更加丰富一些。
当创建类或接口的运行时常量池时，如果构造运行时常量池所需
的内存空间超过了方法区所能提供的最大值，则JVM会
抛OutOfMemoryError异常。
```
#### 3.6.12 方法区使用举例
```java
public class MethodAreaDemo {
    public static void main(String[] args) {
        int x = 500;
        int y = 100;
        int a = x / y;
        int b = 50;
        System.out.println(a + b);
    }
}
```
```markdown
图解字节码指令执行流程
字节码执行过程展示：初始状态
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224092708873.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
首先将操作数500压入操作数栈中
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224092837603.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
然后将操作数 500 从操作数栈中取出，存储到局部变
量表中索引为 1 的位置
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224092921361.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
将操作数100压入操作数栈中
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224092951295.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
然后操作数 100 从操作数栈中取出，存储到局部变量表中索引
为 2 的位置
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224093138397.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
读取本地变量 1 ，压入操作数栈
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224093228874.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
读取本地变量 2 ，压入操作数栈
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224093257902.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
两数相除，计算结果放在操作数栈顶，之后执行 istore_3 
指令，将计算结果从操作数栈中弹出，存入本地变量表 3 中
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224093353872.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
将操作数 50 压入操作数栈
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224093433972.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
将操作数 50 从栈顶弹出，保存在局部变量表 4 中
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224094330264.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
获取 System.out 输出流的引用
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224094530181.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
将本地变量表 3 的值取出，压入操作数栈中，准备进行加法运算
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224094700634.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
将本地变量表 4 的值取出，压入操作数栈中，准备进行加法运算
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224094753675.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
执行加法运算后，将计算结果放在操作数栈顶
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224094841606.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
调用静态方法 println( ) ，输出加法结果
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224095009721.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
main( ) 方法执行结束
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224095042475.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
关于【符号引用 --> 直接引用】的理解
上面代码调用 System.out.println( ) 方法时，首先需要看
System 类有没有加载，再看看 PrintStream 类有没有加载
如果没有加载，则执行加载，执行时，将常量池中的符号引用
（字面量）转换为直接引用（真正的地址值）

关于程序计数器的说明
程序计数器始终存储的都是当前字节码指令的索引地址，目的是为
了方便记录方法调用后能够正常返回，或者是进行了CPU切换后，
也能回到原来的代码继续执行。
```
#### 3.6.13 方法区的演进细节
##### 3.6.13.1 永久代演进过程
```markdown
首先明确：只有Hotspot才有永久代。
BEA JRockit、IBMJ9等来说，是不存在永久代的概念的。原则
上如何实现方法区属于虚拟机实现细节，不受《Java虚拟机规范》
管束，并不要求统一
Hotspot中方法区的变化：
```
| JDK 版本 |  演变细节|
|--|--|
|JDK1.6及以前	  |有永久代（permanent generation），静态变量存储在永久代上  |
| JDK1.7 |有永久代，但已经逐步 “去永久代”，字符串常量池、静态变量从永久代中移除，保存在堆中  |
| JDK1.8	 |  无永久代，类型信息，字段，方法，常量保存在本地内存的元空间，但字符串常量池、静态变量仍然在堆中。|


```markdown
JDK6
方法区由永久代实现，使用 JVM 虚拟机内存
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021022410092253.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
JDK7
方法区由永久代实现，使用 JVM 虚拟机内存
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224100957439.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
JDK8及以后
方法区由元空间实现，使用物理机本地内存
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224101041720.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 3.6.14 永久代为什么要被元空间替代
```markdown
官方的牵强解释：JRockit是和HotSpot融合后的结果，因为
JRockit没有永久代，所以他们不需要配置永久代，HotSpot也就取
消了永久代
随着Java8的到来，HotSpot VM中再也见不到永久代了。但是这并
不意味着类的元数据信息也消失了。这些数据被移到了一个与堆不相
连的本地内存区域，这个区域叫做元空间（Metaspace）。

由于类的元数据分配在本地内存中，元空间的最大可分配空
间就是系统可用内存空间，这项改动是很有必要的，原因有：
为永久代设置空间大小是很难确定的。
在某些场景下，如果动态加载类过多，容易产生Perm区的OOM。
比如某个实际Web工程中，因为功能点比较多，在运行过程中，
要不断动态地加载很多类，经常出现致命错误。
Exception in thread 'dubbo client x.x connector' 
java.lang.OutOfMemoryError:PermGen space
而元空间和永久代之间最大的区别在于：元空间并不在虚拟机中，
而是使用本地内存。因此，默认情况下，元空间的大小仅受本地
内存限制。
对永久代进行调优是很困难的。
方法区的垃圾收集主要回收两部分内容：常量池中废弃的常量和
不再用的类型，方法区的调优主要是为了降低Full GC
有些人认为方法区（如HotSpot虚拟机中的元空间或者永久代）
是没有垃圾收集行为的，其实不然。《Java虚拟机规范》对方
法区的约束是非常宽松的，提到过可以不要求虚拟机在方法区
中实现垃圾收集。事实上也确实有未实现或未能完整实现方法
区类型卸载的收集器存在（如JDK11时期的ZGC收集器就不支
持类卸载）。
一般来说这个区域的回收效果比较难令人满意，尤其是类型的
卸载，条件相当苛刻。但是这部分区域的回收有时又确实是必
要的。以前Sun公司的Bug列表中，曾出现过的若干个严重的
Bug就是由于低版本的HotSpot虚拟机对此区域未完全回收而
导致内存泄漏
```
#### 3.6.15 字符串常量池
```markdown
字符串常量池 StringTable 为什么要调整位置？
JDK7中将StringTable放到了堆空间中。因为永久代的回收效率
很低，在Full GC的时候才会执行永久代的垃圾回收，而Full GC
是老年代的空间不足、永久代不足时才会触发。
这就导致StringTable回收效率不高，而我们开发中会有大量
的字符串被创建，回收效率低，导致永久代内存不足。放到堆里，
能及时回收内存。
```
#### 3.6.16 静态变量位置
```java
静态变量存放在哪里？
/**
 * 结论：
 *  静态变量在jdk6/7存在与永久代中，在jdk8存在于堆中 //private static byte[] arr
 *  静态引用对应的对象实体始终都存在堆空间 //new byte[1024 * 1024 * 100];
 *
 * jdk7：
 * -Xms200m -Xmx200m -XX:PermSize=300m -XX:MaxPermSize=300m -XX:+PrintGCDetails
 * jdk 8：
 * -Xms200m -Xmx200m -XX:MetaspaceSize=300m -XX:MaxMetaspaceSize=300m -XX:+PrintGCDetails
 */
public class StaticFieldTest {
    private static byte[] arr = new byte[1024 * 1024 * 100]; //100MB

    public static void main(String[] args) {
        System.out.println(StaticFieldTest.arr);
    }
}


设置JVM参数
-Xms200m -Xmx200m -XX:MetaspaceSize=300m -XX:MaxMetaspaceSize=300m -XX:+PrintGCDetails
通过 GC 日志可以看出：静态变量引用对应的对象实体始
终都在堆空间中（arr 数组对象直接怼到老年区去了）
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225170052364.png)
```java
/**
 * 《深入理解Java虚拟机》中的案例：
 * staticObj、instanceObj、localObj存放在哪里？
 */
public class StaticObjTest {
    static class Test {
        //静态属性
        static ObjectHolder staticObj = new ObjectHolder();
        //非静态属性
        ObjectHolder instanceObj = new ObjectHolder();

        void foo() {
            //局部变量
            ObjectHolder localObj = new ObjectHolder();
            System.out.println("done");
        }
    }

    private static class ObjectHolder {
    }

    public static void main(String[] args) {
        Test test = new StaticObjTest.Test();
        test.foo();
    }
}

可以使用 JHSDB.exe，在JDK9的时候才引入的
分析：staticObj随着Test的类型信息存放在方法区，
instanceObj随着Test的对象实例存放在Java堆，
localObject则是存放在foo( )方法栈帧的局部变量表中。
测试发现：三个对象的数据在内存中的地址都落在Eden区
范围内，所以结论：只要是对象实例必然会在Java堆中分配。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225170336246.png)
```markdown
接着，找到了一个引用该staticObj对象的地方，是在一个
java.lang.Class的实例里，并且给出了这个实例的地址，
通过Inspector查看该对象实例，可以清楚看到这确实是
一个java.lang.Class类型的对象实例，里面有一个名
为staticobj的实例字段：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225170446256.png)

```markdown
从《Java虚拟机规范》所定义的概念模型来看，所有Class相
关的信息都应该存放在方法区之中，但方法区该如何实
现，《Java虚拟机规范》并未做出规定，这就成了一件允
许不同虚拟机自己灵活把握的事情。JDK7及其以后版本
的HotSpot虚拟机选择把静态变量与类型在Java语言一端的
映射Class对象存放在一起，存储于Java堆之中，从我们
的实验中也明确验证了这一点
```
#### 3.6.17 方法区的垃圾回收
```markdown
方法区的垃圾收集

有些人认为方法区（如Hotspot虚拟机中的元空间或者永久代）
是没有垃圾收集行为的，其实不然。
《Java虚拟机规范》对方法区的约束是非常宽松的，提到过
可以不要求虚拟机在方法区中实现垃圾收集。事实上也确实有未
实现或未能完整实现方法区类型卸载的收集器存在（如JDK11
时期的ZGC收集器就不支持类卸载）。
一般来说这个区域的回收效果比较难令人满意，尤其是类型的
卸载，条件相当苛刻。但是这部分区域的回收有时又确实
是必要的。以前sun公司的Bug列表中，曾出现过的若干个
严重的Bug就是由于低版本的HotSpot虚拟机对此区域未完
全回收而导致内存泄漏。
方法区的垃圾收集主要回收两部分内容：常量池中废弃的
常量和不再使用的类型。
```
##### 3.6.17.1 方法区常量的回收
```markdown
先来说说方法区内常量池之中主要存放的两大类常量：字面
量和符号引用
字面量比较接近Java语言层次的常量概念，如文本字符串、
被声明为final的常量值等
而符号引用则属于编译原理方面的概念，包括下面三类常量：
类和接口的全限定名
字段的名称和描述符
方法的名称和描述符
HotSpot虚拟机对常量池的回收策略是很明确的，只要常
量池中的常量没有被任何地方引用，就可以被回收。
回收废弃常量与回收Java堆中的对象非常类似。（关于常
量的回收比较简单，重点是类的回收）
```
##### 3.6.17.2 方法区类的回收
```markdown
判定一个常量是否“废弃”还是相对简单，而要判定一个类型是否
属于“不再被使用的类”的条件就比较苛刻了。需要同时满足下
面三个条件：
该类所有的实例都已经被回收，也就是Java堆中不存在该类及其
任何派生子类的实例。
加载该类的类加载器已经被回收，这个条件除非是经过精心设
计的可替换类加载器的场景，如OSGi、JSP的重加载等，否
则通常是很难达成的。
该类对应的java.lang.Class对象没有在任何地方被引用，无
法在任何地方通过反射访问该类的方法。
Java虚拟机被允许对满足上述三个条件的无用类进行回收，这
里说的仅仅是“被允许”，而并不是和对象一样，没有引用
了就必然会回收。关于是否要对类型进行回收，HotSpot虚
拟机提供了-Xnoclassgc参数进行控制，还可以使
用-verbose:class 以及 -XX：+TraceClass-Loading、
-XX：+TraceClassUnLoading查看类加载和卸载信息
在大量使用反射、动态代理、CGLib等字节码框架，
动态生成JSP以及 OSGi 这类频繁自定义类加载器的场景
中，通常都需要Java虚拟机具备类型卸载的能力，以
保证不会对方法区造成过大的内存压力。
```
#### 3.6.18 运行时数据区总结
```markdown
线程私有结构：程序计数器、虚拟机栈、本地方法栈
每个虚拟机栈由具体的栈帧组成，在栈帧的动态链接中，
保存至对方法的引用
方法区在 JDK7 之前，使用永久代实现，在 JDK8 之后，
使用元空间实现
Minor GC 针对于新生区，Major GC 针对于老年区，Full GC 
针对于整个堆空间和方法区
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225171418699.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
## 4 对象的实例化内存布局与访问定位
### 4.1 对象的实例化
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225171935184.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 4.2 创建对象的方式
```markdown
new：最常见的方式、单例类中调用getInstance的静态类方法、
XXXFactory的静态方法
Class的newInstance方法：反射的方式，在JDK9里面被标记
为过时的方法，因为只能调用空参构造器，并且权限必须为 public
Constructor的newInstance(Xxxx)：反射的方式，可以调用空参
或带参的构造器，权限没有要求
使用clone( )：不调用任何的构造器，要求当前的类需要实
现Cloneable接口中的clone( )方法
使用反序列化：序列化一般用于Socket的网络传输
第三方库 Objenesis
```
### 4.3 创建对象的步骤
```java
从字节码角度看待对象的创建过程
public class ObjectTest {
    public static void main(String[] args) {
        Object obj = new Object();
    }
}


main( ) 方法对应的字节码（后面细讲）
调用 new 指令后后，加载 Object 类
调用 Object 类的 init( ) 方法
0: new           #2                  // class java/lang/Object
3: dup
4: invokespecial #1                  // Method java/lang/Object."<init>":()V
7: astore_1
8: return

```
#### 4.3.1 判断对象对应的类是否加载、链接、初始化
```markdown
虚拟机遇到一条new指令，首先去检查这个指令的参数能否
在Metaspace的常量池中定位到一个类的符号引用，并且检查
这个符号引用代表的类是否已经被加载，解析和初始化。
（即判断类元信息是否存在）。
如果该类没有加载，那么在双亲委派模式下，使用当前类加
载器以ClassLoader + 包名 + 类名为key进行查找对应的.class
文件，如果没有找到文件，则抛出ClassNotFoundException异常，
如果找到，则进行类加载，并生成对应的 Class 类对象。
```
#### 4.3.2 为对象分配内存
```markdown
首先计算对象占用空间的大小，接着在堆中划分一块内存给新对象。
如果实例成员变量是引用变量，仅分配引用变量空间即可，即4个字
节大小
如果内存规整：采用指针碰撞分配内存
如果内存是规整的，那么虚拟机将采用的是指针碰撞法
（Bump The Point）来为对象分配内存。
意思是所有用过的内存在一边，空闲的内存放另外一边，中间放
着一个指针作为分界点的指示器，分配内存就仅仅是把指针往空
闲内存那边挪动一段与对象大小相等的距离罢了。
如果垃圾收集器选择的是Serial ，ParNew这种基于压缩算法的，
虚拟机采用这种分配方式。一般使用带Compact（整理）过程的收
集器时，使用指针碰撞。
标记压缩（整理）算法会整理内存碎片，堆内存一存对象，另一
边为空闲区域
如果内存不规整
如果内存不是规整的，已使用的内存和未使用的内存相互交错，那
么虚拟机将采用的是空闲列表来为对象分配内存。
意思是虚拟机维护了一个列表，记录上哪些内存块是可用的，再分
配的时候从列表中找到一块足够大的空间划分给对象实例，并更新
列表上的内容。这种分配方式成为了 “空闲列表（Free List）”
选择哪种分配方式由Java堆是否规整所决定，而Java堆是否规整又
由所采用的垃圾收集器是否带有压缩整理功能决定
标记清除算法清理过后的堆内存，就会存在很多内存碎片。
```
#### 4.3.3 处理并发安全问题
```markdown
采用CAS+失败重试、区域加锁保证更新的原子性
每个线程预先分配TLAB — 通过设置 -XX:+/-UseTLAB参数
来设置（区域加锁机制）
在Eden区给每个线程分配一块区域
```
#### 4.3.4 初始化分配到的空间
```markdown
所有属性设置默认值，保证对象实例字段在不赋值时可以直接使用
```
#### 4.3.5 设置对象的对象头
```markdown
将对象的所属类（即类的元数据信息）、对象的HashCode和对
象的GC信息、锁信息等数据存储在对象的对象头中。这个过
程的具体设置方式取决于JVM实现。
```
#### 4.3.6 执行init方法进行初始化
```markdown
在Java程序的视角看来，初始化才正式开始。初始化成员变量，
执行实例化代码块，调用类的构造方法，并把堆内对象的首
地址赋值给引用变量
因此一般来说（由字节码中跟随invokespecial指令所决定），
new指令之后会接着就是执行init方法，把对象按照程序员的意愿
进行初始化，这样一个真正可用的对象才算完成创建出来。
```
```markdown
从字节码角度看待init方法
```
```java
/**
 * 测试对象实例化的过程
 *  ① 加载类元信息 - ② 为对象分配内存 - ③ 处理并发问题  - ④ 属性的默认初始化（零值初始化）
 *  - ⑤ 设置对象头的信息 - ⑥ 属性的显式初始化、代码块中初始化、构造器中初始化
 *
 *  给对象的属性赋值的操作：
 *  ① 属性的默认初始化 - ② 显式初始化 / ③ 代码块中初始化 - ④ 构造器中初始化
 */
public class Customer{
    int id = 1001;
    String name;
    Account acct;

    {
        name = "匿名客户";
    }
    
    public Customer(){
        acct = new Account();
    }

}
class Account{

}


init( ) 方法的字节码指令：
属性的默认值初始化：id = 1001;
显示初始化/代码块初始化：name = "匿名客户";
构造器初始化：acct = new Account();

 0 aload_0
 1 invokespecial #1 <java/lang/Object.<init>>
 4 aload_0
 5 sipush 1001
 8 putfield #2 <cn/sxt/java/Customer.id>
11 aload_0
12 ldc #3 <匿名客户>
14 putfield #4 <cn/sxt/java/Customer.name>
17 aload_0
18 new #5 <cn/sxt/java/Account>
21 dup
22 invokespecial #6 <cn/sxt/java/Account.<init>>
25 putfield #7 <cn/sxt/java/Customer.acct>
28 return

```
### 4.4 对象的内存布局
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225174445642.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 4.4.1 对象头
```markdown
对象头包含两部分：运行时元数据（Mark Word）和类型指针

运行时元数据
哈希值（HashCode），可以看作是堆中对象的地址
GC分代年龄（年龄计数器）
锁状态标志
线程持有的锁
偏向线程ID
偏向时间戳
类型指针
指向类元数据InstanceKlass，确定该对象所属的类型。指向
的其实是方法区中存放的类元信息
说明：如果对象是数组，还需要记录数组的长度
```
#### 4.4.2 实例数据
```markdown
说明

它是对象真正存储的有效信息，包括程序代码中定义的各种
类型的字段（包括从父类继承下来的和本身拥有的字段）
规则

相同宽度的字段总是被分配在一起
父类中定义的变量会出现在子类之前（父类在子类之前加载）
如果CompactFields参数为true（默认为true）：子类的
窄变量可以插入到父类变量的空隙
```
#### 4.4.3 对齐填充
```markdown
不是必须的，也没特别含义，仅仅起到占位符的作用
```
#### 4.4.4 内存布局总结
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225175413999.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 4.5 对象的访问定位
```markdown
JVM是如何通过栈帧中的对象引用访问到其内部的对象实例呢？
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225175543972.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225175605745.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 4.5.1 句柄访问
```markdown
优点：reference中存储稳定句柄地址，对象被移动（垃圾收集
时移动对象很普遍）时只会改变句柄中实例数据指针
即可，reference本身不需要被修改

缺点：在堆空间中开辟了一块空间作为句柄池，句柄池本身也
会占用空间；通过两次指针访问才能访问到堆中的对象，效率低
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225175741942.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 4.5.2 直接指针(HotSpot采用)
```markdown
优点：直接指针是局部变量表中的引用，直接指向堆中的实例，
在对象实例中有类型指针，指向的是方法区中的对象类型数据
缺点：对象被移动（垃圾收集时移动对象很普遍）时需要修
改 reference 的值
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225175959513.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
## 5 直接内存
```markdown
不是虚拟机运行时数据区的一部分，也不是《Java虚拟机规范》中
定义的内存区域。
直接内存是在Java堆外的、直接向系统申请的内存区间。
来源于NIO，通过存在堆中的DirectByteBuffer操作Native内存
通常，访问直接内存的速度会优于Java堆。即读写性能高。
因此出于性能考虑，读写频繁的场合可能会考虑使用直接内存。
Java的NIO库允许Java程序使用直接内存，用于数据缓冲区
```
```java
/**
 *  IO                  NIO (New IO / Non-Blocking IO)
 *  byte[] / char[]     Buffer
 *  Stream              Channel
 *
 * 查看直接内存的占用与释放
 */
public class BufferTest {
    private static final int BUFFER = 1024 * 1024 * 1024; //1GB

    public static void main(String[] args){
        //直接分配本地内存空间
        ByteBuffer byteBuffer = ByteBuffer.allocateDirect(BUFFER);
        System.out.println("直接内存分配完毕，请求指示！");

        Scanner scanner = new Scanner(System.in);
        scanner.next();

        System.out.println("直接内存开始释放！");
        byteBuffer = null;
        System.gc();
        scanner.next();
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/202102251805286.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
直接占用了 1G 的本地内存
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225180607735.png)
```markdown
释放后，Java程序的内存占用明显减少
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225180715394.png)
### 5.1 BIO 与 NIO
```markdown
非直接缓冲区（BIO）
原来采用BIO的架构，在读写本地文件时，我们需要从用户态切
换成内核态
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021022518094590.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
直接缓冲区（NIO）

使用NIO时，如下图。操作系统划出的直接缓存区可以被Java代码直接访问，只有一份。NIO适合对大文件的读写操作
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225181057985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
public class BufferTest1 {

    private static final String TO = "F:\\test\\异界BD中字.mp4";
    private static final int _100Mb = 1024 * 1024 * 100;

    public static void main(String[] args) {
        long sum = 0;
        String src = "F:\\test\\异界BD中字.mp4";
        for (int i = 0; i < 3; i++) {
            String dest = "F:\\test\\异界BD中字_" + i + ".mp4";
            // sum += io(src,dest); //54606
            sum += directBuffer(src, dest); //50244
        }

        System.out.println("总花费的时间为：" + sum);
    }

    private static long directBuffer(String src, String dest) {
        long start = System.currentTimeMillis();

        FileChannel inChannel = null;
        FileChannel outChannel = null;
        try {
            inChannel = new FileInputStream(src).getChannel();
            outChannel = new FileOutputStream(dest).getChannel();

            ByteBuffer byteBuffer = ByteBuffer.allocateDirect(_100Mb);
            while (inChannel.read(byteBuffer) != -1) {
                byteBuffer.flip(); //修改为读数据模式
                outChannel.write(byteBuffer);
                byteBuffer.clear(); //清空
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (inChannel != null) {
                try {
                    inChannel.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
            if (outChannel != null) {
                try {
                    outChannel.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

        long end = System.currentTimeMillis();
        return end - start;
    }

    private static long io(String src, String dest) {
        long start = System.currentTimeMillis();
      
        FileInputStream fis = null;
        FileOutputStream fos = null;
        try {
            fis = new FileInputStream(src);
            fos = new FileOutputStream(dest);
            byte[] buffer = new byte[_100Mb];
            while (true) {
                int len = fis.read(buffer);
                if (len == -1) {
                    break;
                }
                fos.write(buffer, 0, len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fis != null) {
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
            if (fos != null) {
                try {
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
        }

        long end = System.currentTimeMillis();

        return end - start;
    }
}

```
```java
深入 ByteBuffer 源码

ByteBuffer.allocateDirect( ) 方法
public static ByteBuffer allocateDirect(int capacity) {
    return new DirectByteBuffer(capacity);
}

DirectByteBuffer 类的构造器用到了 Unsafe 类分配本地内存
DirectByteBuffer(int cap) {                   // package-private

    super(-1, 0, cap, cap);
    boolean pa = VM.isDirectMemoryPageAligned();
    int ps = Bits.pageSize();
    long size = Math.max(1L, (long)cap + (pa ? ps : 0));
    Bits.reserveMemory(size, cap);

    long base = 0;
    try {
        base = unsafe.allocateMemory(size);
    } catch (OutOfMemoryError x) {
        Bits.unreserveMemory(size, cap);
        throw x;
    }
    unsafe.setMemory(base, size, (byte) 0);
    if (pa && (base % ps != 0)) {
        // Round up to page boundary
        address = base + ps - (base & (ps - 1));
    } else {
        address = base;
    }
    cleaner = Cleaner.create(this, new Deallocator(base, size, cap));
    att = null;
}
```
### 5.2 直接内存与OOM
```markdown
直接内存也可能导致OutofMemoryError异常
由于直接内存在Java堆外，因此它的大小不会直接受限于-Xmx指定
的最大堆大小，但是系统内存是有限的，Java堆和直接内存的总和
依然受限于操作系统能给出的最大内存。
直接内存的缺点为：
分配回收成本较高
不受JVM内存回收管理
直接内存大小可以通过MaxDirectMemorySize设置
如果不指定，默认与堆的最大值-Xmx参数值一致
```
```java
/**
 * 本地内存的OOM:  OutOfMemoryError: Direct buffer memory
 */
public class BufferTest2 {
    private static final int BUFFER = 1024 * 1024 * 20; //20MB

    public static void main(String[] args) {
        ArrayList<ByteBuffer> list = new ArrayList<>();

        int count = 0;
        try {
            while(true){
                ByteBuffer byteBuffer = ByteBuffer.allocateDirect(BUFFER);
                list.add(byteBuffer);
                count++;
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        } finally {
            System.out.println(count);
        }

    }
}

```
```markdown
本地内存持续增长，直至程序抛出异常：
java.lang.OutOfMemoryError: Direct buffer memory
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225181542787.png)
```java
直接通过 Unsafe 类申请本地内存
/**
 * -Xmx20m -XX:MaxDirectMemorySize=10m
 */
public class MaxDirectMemorySizeTest {
    private static final long _1MB = 1024 * 1024;

    public static void main(String[] args) throws IllegalAccessException {
        Field unsafeField = Unsafe.class.getDeclaredFields()[0];
        unsafeField.setAccessible(true);
        Unsafe unsafe = (Unsafe)unsafeField.get(null);
        while(true){
            unsafe.allocateMemory(_1MB);
        }

    }
}

```
```markdown
设置JVM 参数
-Xmx20m -XX:MaxDirectMemorySize=10m

抛出 OOM 异常
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225181656308.png)

```markdown
JDK8 中元空间直接使用本地内存

java程序进程所占的内存空间 = 本地内存 + 堆空间
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225181732590.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

## 6 执行引擎

```markdown
执行引擎是Java虚拟机的核心组成部分之一
虚拟机是一个相对于“物理机”的概念，这两种机器都有代码执行能力，
其区别是物理机的执行引擎是直接建立在处理器、缓存、指令集和
操作系统层面上的，而虚拟机的执行引擎则是由软件自行实现的，
因此可以不受物理条件制约地定制指令集与执行引擎的结构体
系，能够执行那些不被硬件直接支持的指令集格式。
JVM的主要任务是负责装载字节码到其内部，但字节码并不能够
直接运行在操作系统之上，因为字节码指令并非等价于本地机器
指令，它内部包含的仅仅只是一些能够被JVM锁识别的字节码
指令、符号表和其他辅助信息
那么，如果想让一个Java程序运行起来、执行引擎的任务就
是将字节码指令解释/编译为对应平台上的本地机器指令才可。
简单来说，JVM中的执行引擎充当了将高级语言翻译为机
器语言的译者.
执行引擎的工作过程

从外观上来看，所有的Java虚拟机的执行引擎输入、输出都
是一致的：输入的是字节码二进制流，处理过程是字节码解
析执行的等效过程，输出的是执行结果。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227182419767.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)


```markdown
1 执行引擎在执行的过程中究竟需要执行什么样的字节码指令
完全依赖于PC寄存器。
2 每当执行完一项指令操作后，PC寄存器就会更新下一条需要
被执行的指令地址。
3 当然方法在执行的过程中，执行引擎有可能会通过存储在局部
变量表中的对象引用准确定位到存储在Java堆区中的对象实例信
息，以及通过对象头中的元数据指针定位到目标对象的类型信息。
```
### 6.1 Java代码编译和执行过程
```markdown
大部分的程序代码转换成物理机的目标代码或虚拟机能执行的指令集之前，都需要经过下面图中的各个步骤：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021022718301466.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
Java代码编译是由Java源码编译器来完成，流程图如下所示：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227183039107.png)

```markdown
Java字节码的执行是由JVM执行引擎来完成，流程图如下所示
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227183136489.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

### 6.2 什么是解释器（ Interpreter），什么是JIT编译器
```markdown
解释器：当Java虚拟机启动时会根据预定义的规范对字节码
采用逐行解释的方式执行，将每条字节码文件中的内容“翻
译”为对应平台的本地机器指令执行。
JIT （Just In Time Compiler）编译器（即时编译器）：就是
虚拟机将源代码直接编译成和本地机器平台相关的机器语言。
```

```markdown
为什么说Java是半编译半解释型语言？
JDK1.0时代，将Java语言定位为“解释执行”还是比较准
确的。再后来，Java也发展出可以直接生成本地代码的编译器。
现在JVM在执行Java代码的时候，通常都会将解释执行与编
译执行二者结合起来进行。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227183316654.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 6.3 解释器

```markdown
JVM设计者们的初衷仅仅只是单纯地为了满足Java程序实现
跨平台特性，因此避免采用静态编译的方式直接生成本地
机器指令，从而诞生了实现解释器在运行时采用逐行解释
字节码执行程序的想法。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021022718361337.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
解释器真正意义上所承担的角色就是一个运行时“翻译者”，将
字节码文件中的内容“翻译”为对应平台的本地机器指令执行。
当一条字节码指令被解释执行完成后，接着再根据PC寄存器
中记录的下一条需要被执行的字节码指令执行解释操作。

在Java的发展历史里，一共有两套解释执行器，即古老的字
节码解释器、现在普遍使用的模板解释器。

字节码解释器在执行时通过纯软件代码模拟字节码的执行，效率
非常低下。· - 而模板解释器将每一 条字节码和一个模板函数相关
联，模板函数中直接产生这条字节码执行时的机器码，从而很大
程度上提高了解释器的性能。

在HotSpot VM中，解释器主要由Interpreter模块和Code模块构成。

Interpreter模块：实现了解释器的核心功能
Code模块：用于管理HotSpot VM在运行时生成的本地机器指令


现状

由于解释器在设计和实现上非常简单，因此除了Java语言之外，
还有许多高级语言同样也是基于解释器执行的，
比如Python、 Perl、Ruby等。但是在今天，基于解释器执行
已经沦落为低效的代名词，并且时常被一些C/C+ +程序员所调侃。
为了解决这个问题，JVM平台支持一种叫作即时编译的技术。即
时编译的目的是避免函数被解释执行，而是将整个函数体编译成为
机器码，每次函数执行时，只执行编译后的机器码即可，这种
方式可以使执行效率大幅度提升。
不过无论如何，基于解释器的执行模式仍然为中间语言的发展做
出了不可磨灭的贡献。

```
### 6.4 HostSpot JVM的执行方式
```markdown
HotSpot VM 为何解释器与JIT编译器共存
java代码的执行分类：

第一种是将源代码编译成字节码文件，然后再运行时通过解释器将字节
码文件转为机器码执行
第二种是编译执行（直接编译成机器码）。现代虚拟机为了提高执行
效率，会使用即时编译技术（JIT,Just In Time）将方法编译成
机器码后再执行

HotSpot VM是目前市面上高性能虛拟机的代表作之一。它采用解释
器与即时编译器并存的架构。在Java虛拟机运行时，解释器和即
时编译器能够相互协作，各自取长补短，尽力去选择最合适的方
式来权衡编译本地代码的时间和直接解释执行代码的时间。
  在今天，Java程序的运行性能早已脱胎换骨，已经达到了
可以和C/C++程序一较高下的地步。
解释器依然存在的必要性
有些开发人员会感觉到诧异，既然HotSpotVM中已经内置JIT编译
器了，那么为什么还需要再使用解释器来“拖累”程序的执行性能
呢？比如JRockit VM内部就不包含解释器，字节码全部都依靠即
时编译器编译后执行。
首先明确：
当程序启动后，解释器可以马上发挥作用，省去编译的时间，
立即执行。
编译器要想发挥作用，把代码编译成本地代码，需要一定的执
行时间。但编译为本地代码后，执行效率高。
所以：
尽管JRockitVM中程序的执行性能会非常高效，但程序在启
动时必然需要花费更长的时间来进行编译。对于服务端应
用来说，启动时间并非是关注重点，但对于那些看中启动
时间的应用场景而言，或许就需要采用解释器与即时编译
器并存的架构来换取一一个平衡点。在此模式下，当Java虚拟
器启动时，解释器可以首先发挥作用，而不必等待即时编译
器全部编译完成后再执行，这样可以省去许多不必要的编
译时间。随着时间的推移，编译器发挥作用，把越来越
多的代码编译成本地代码，获得更高的执行效率。
同时，解释执行在编译器进行激进优化不成立的时候，作为
编译器的“逃生门”。

当虛拟机启动的时候，解释器可以首先发挥作用，而不必等待即时编
译器全部编译完成再执行，这样可以省去许多不必要的编译时间。
并且随着程序运行时间的推移，即时编译器逐渐发挥作用，根据热点
探测功能，将有价值的字节码编译为本地机器指令，以换取更高的
程序执行效率。
```

```markdown
案例
注意解释执行与编译执行在线上环境微妙的辩证关系。机器在
热机状态可以承受的负载要大于冷机状态。如果以热机状态时的流
量进行切流，可能使处于冷机状态的服务器因无法承载流量而假死。
在生产环境发布过程中，以分批的方式进行发布，根据机器数
量划分成多个批次，每个批次的机器数至多占到整个集群的1/8。曾
经有这样的故障案例：某程序员在发布平台进行分批发布，在输入发布
总批数时，误填写成分为两批发布。如果是热机状态，在正常情况下
一半的机器可以勉强承载流量，但由于刚启动的JVM均是解释执行，
还没有进行热点代码统计和JIT动态编译，导致机器启动之后，
当前1/2发布。
成功的服务器马上全部宕机，此故障说明了JIT的存在。一阿里团队
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227184015962.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 6.5 JIT编译器
```markdown
Java 语言的“编译器” 其实是一段“不确定”的操作过程，因为它可能是指一个前端编译器（其实叫“编译器的前端” 更准确一些）把.java文件转变成.class文件的过程；
也可能是指虚拟机的后端运行期编译器（JIT 编译器，Just In Time Compiler）把字节码转变成机器码的过程。
还可能是指使用静态提前编译器（AOT 编译器，Ahead Of Time Compiler）直接把. java文件编译成本地机器代码的过程。


前端编译器： 
Sun的Javac、 Eclipse JDT中的增量式编译器（ECJ）    
JIT编译器： 
HotSpot VM的C1、C2编译器。    
AOT编译器： 
GNU Compiler for the Java （GCJ） 、Excelsior JET。
```
### 6.6 热点代码及探测方式

```markdown
当然是否需要启动JIT编译器将字节码直接编译为对应平台的本地机
器指令，则需要根据代码被调用执行的频率而定。关于那些需要被编译
为本地代码的字节码，也被称之为“热点代码” ，JIT编译器在运行
时会针
对那些频繁被调用的“热点代码”做出深度优化，将其直接编译为对
应平台的本地机器指令，以此提升Java程序的执行性能。

一个被多次调用的方法，或者是一个方法体内部循环次数较多的循环
体都可以被称之为“热点代码”，因此都可以通过JIT编译器编译为本地
机器指令。由于这种编译方式发生在方法的执行过程中，因此也被称
之为栈上替换，或简称为OSR （On StackReplacement）编译。
一个方法究竟要被调用多少次，或者一个循环体究竟需要执行多少
次循环才可以达到这个标准？必然需要一个明确的阈值，JIT编译器
才会将这些“热点代码”编译为本地机器指令执行。这里主要依靠热点
探测功能。
目前HotSpot VM所采用的热点探测方式是基于计数器的热点探测。
采用基于计数器的热点探测，HotSpot VM将会为每一个 方法都建立
2个不同类型的计数器，分别为方法调用计数器（Invocation 
Counter） 和回边计数器（BackEdge Counter） 。

方法调用计数器用于统计方法的调用次数
回边计数器则用于统计循环体执行的循环次数
```
### 6.7 方法调用计数器

```markdown
这个计数器就用于统计方法被调用的次数，它的默认阈值在Client 
模式
下是1500 次，在Server 模式下是10000 次。超过这个阈值，就
会触发JIT编译。
这个阈值可以通过虚拟机参数一XX ：CompileThreshold来人为设
定。
当一个方法被调用时， 会先检查该方法是否存在被JIT编译过的版
本，如
果存在，则优先使用编译后的本地代码来执行。如果不存在已被编
译过的版本，则将此方法的调用计数器值加1，然后判断方法调用
计数器与回边计数器值之和是否超过方法调用计数器的阈值。如果
已超过阈值，那么将会向即时编译器提交一个该方法的代码编译请求。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021022718455372.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 6.8 热度衰减

```markdown
如果不做任何设置，方法调用计数器统计的并不是方法被调用的绝
对次数，而是一个相对的执行频率，即一段时间之内方法被调用的
次数。当超过一定的时间限度， 如果方法的调用次数仍然不足以让
它提交给即时编译器编译，那这个方法的调用计数器就会被减少一
半，这个过程称为方法调用计数器热度的衰减（Counter Decay） ，
而这段时间就称为此方法统计的半衰周期（Counter Half Life 
Time）。
进行热度衰减的动作是在虚拟机进行垃圾收集时顺便进行的，可以使
用虚拟机参数
-XX：-UseCounterDecay来关闭热度衰减，让方法计数器统计
方法调用的绝对次数，这样，只要系统运行时间足够长，绝大部
分方法都会被编译成本地代码。
另外， 可以使用-XX： CounterHalfLifeTime参数设置半衰周期的
时间，单位是秒。
```
### 6.9 回边计数器

```markdown
它的作用是统计一个方法中循环体代码执行的次数，在字节
码中遇到控制流向后跳转的指令称为“回边” （Back Edge）。显
然，建立回边计数器统计的目的就是为了触发OSR编译。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227184727374.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 6.10 HotSpot VM 可以设置程序执行方式

```markdown
缺省情况下HotSpot VM是采用解释器与即时编译器并存的架构，当
然开发人员可以根据具体的应用场景，通过命令显式地为Java虚拟机
指定在运行时到底是完全采用解释器执行，还是完全采用即时编译
器执行。如下所示：

-Xint： 完全采用解释器模式执行程序；
-Xcomp： 完全采用即时编译器模式执行程序。如果即时编译出现
问题，解释器会介入执行。
-Xmixed：采用解释器+即时编译器的混合模式共同执行程序。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227184939969.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
测试解释器模式和JIT编译模式
测试表明：

纯解释器模式速度最慢（JVM1.0版本用的就是纯解释器执行）
混合模式速度更快
```
```java
/**
 * 测试解释器模式和JIT编译模式
 *  -Xint  : 6520ms
 *  -Xcomp : 950ms
 *  -Xmixed : 936ms
 */
public class IntCompTest {
    public static void main(String[] args) {
        long start = System.currentTimeMillis();
        testPrimeNumber(1000000);
        long end = System.currentTimeMillis();
        System.out.println("花费的时间为：" + (end - start));
    }

    public static void testPrimeNumber(int count){
        for (int i = 0; i < count; i++) {
            //计算100以内的质数
            label:for(int j = 2;j <= 100;j++){
                for(int k = 2;k <= Math.sqrt(j);k++){
                    if(j % k == 0){
                        continue label;
                    }
                }
                //System.out.println(j);
            }

        }
    }
}

```
### 6.11 HotSpot VM 中的JIT分类
```markdown
在HotSpot VM中内嵌有两个JIT编译器，分别为
Client Compiler和ServerCompiler，但大多数情况下我们
简称为C1编译器和C2编译器。开发人员可以通过如下命令显
式指定Java虚拟机在运行时到底使用哪一种即时编译器，如下所示：

-client： 指定Java虚拟机运行在Client模式下，并
使用C1编译器；

C1编译器会对字节码进行简单和可靠的优化，耗时短。以达到
更快的编译速度。


-server： 指定Java虚拟机运行在Server模式下，并使用C2编译器。

C2进行耗时较长的优化，以及激进优化。但优化的代码执行效率更高。

C1和C2编译器不同的优化策略

在不同的编译器上有不同的优化策略，C1编译器上主要有方法内联，
去虚拟化、冗余消除。

方法内联：将引用的函数代码编译到引用点处，这样可以减少栈
帧的生成，减少参数传递以及跳转过程
去虚拟化：对唯一的实现类进行内联
冗余消除：在运行期间把一些不会执行的代码折叠掉


C2的优化主要是在全局层面，逃逸分析是优化的基础。基于逃逸
分析在C2.上有如下几种优化：（server模式下才会有这些优化，
64位系统默认就是server模式）

标量替换：用标量值代替聚合对象的属性值
栈上分配：对于未逃逸的对象分配对象在栈而不是堆
同步消除：清除同步操作，通常指synchronized



分层编译（Tiered Compilation）策略：程序解释执行
（不开启性能监控）可以触发C1编译，将字节码编译成机器码，可以
进行简单优化，也可以加上性能监控，C2编译会根据性能监控信息
进行激进优化。
不过在Java7版本之后，一旦开发人员在程序中显式指定命
令“一server"时，默认将会开启分层编译策略，由C1编译器和
C2编译器相互协作共同来执行编译任务。
总结

一般来讲，JIT编译出来的机器码性能比解释器高。
C2编译器启动时长比C1编译器慢，系统稳定执行以后，C2编译
器执行速度远远快于C1编译器。
```
### 6.12 Graal编译器与AOT编译器
```markdown
Graal编译器

自JDK10起，HotSpot又加入一个全新的即时编译器： Graal编译器
编译效果短短几年时间就追评了C2编译器。未来可期。
目前，带着“实验状态"标签，需要使用开关参数
-XX： +UnlockExperimentalVMOptions 一XX： +UseJVMCICompiler去激活，才可以使用。

AOT编译器

jdk9引入了AOT编译器（静态提前编译器，Ahead Of Time Compiler）
Java 9引入了实验性AOT编译工具jaotc。它借助了Graal 编译器，将所输
入的Java 类文件转换为机器码，并存放至生成的动态共享库之中。
所谓AOT编译，是与即时编译相对立的一个概念。我们知道，即时编译
指的是在程序的运行过程中，将字节码转换为可在硬件上直接运行的机器
码，并部署至托管环境中的过程。而AOT编译指的则是，在程序运行之前，
便将字节码转换为机器码的过程。
最大好处： Java虚拟机加载已经预编译成二进制库，可以直接执行。
不必等待即时编译器的预热，减少Java应用给人带来“第一次运行慢”的
不良体验。
缺点：

破坏了java"一次编译，到处运行”，必须为每个不同硬件、oS编译
对应的发行包。
降低了Java链接过程的动态性，加载的代码在编译期就必须全部已知。
还需要继续优化中，最初只支持Linux x64 java base
```
## 7 字符串常量池StringTable
### 7.1 String的基本特性
```markdown
String：字符串，使用一对""引起来表示。

String sl = "hello"；//字面量的定义方式
String s2 = new String（"hello"） ；


String声明为final的， 不可被继承
String实现了Serializable接口：表示字符串是支持序列化的。
实现了Comparable接口：表示String可以比较大小
String在jdk8及以前内部定义了final char[],value用于
存储字符串数据。jdk9时改为byte[]

结论： String再也不用char[] 来存储啦，改成了byte[] 加上
编码标记，节约了一些空间。StringBuffer和StringBuilder也做了
一些修改
```
```java
public final class String implements java.io.Serializable， Comparable<String>,CharSequence {
@Stable
private final byte[] value；
}
```
```markdown
String：代表不可变的字符序列。简称：不可变性。

当对字符串重新赋值时，需要重写指定内存区域赋值，不能使用原
有的value进行赋值。
当对现有的字符串进行连接操作时，也需要重新指定内存区域赋值，
不能使用原有的value进行赋值。
当调用String的replace（）方法修改指定字符或字符串时，也需要
重新指定内存区域赋值，不能使用原有的value进行赋值。


通过字面量的方式（区别于new）给一个字符串赋值，此时的字符
串值声明在字符串常量池中。
```
```java
/**
 * String的基本使用:体现String的不可变性
 */
public class StringTest1 {
    @Test
    public void test1() {
        String s1 = "abc";//字面量定义的方式，"abc"存储在字符串常量池中
        String s2 = "abc";
        s1 = "hello";

        System.out.println(s1 == s2);//判断地址：true  --> false

        System.out.println(s1);//
        System.out.println(s2);//abc

    }

    @Test
    public void test2() {
        String s1 = "abc";
        String s2 = "abc";
        s2 += "def";
        System.out.println(s2);//abcdef
        System.out.println(s1);//abc
    }

    @Test
    public void test3() {
        String s1 = "abc";
        String s2 = s1.replace('a', 'm');
        System.out.println(s1);//abc
        System.out.println(s2);//mbc
    }
}


```
```markdown
字符串常量池中是不会存储相同内容的字符串的。
String的String Pool 是一个固定大小的Hashtable，默认值大小长度是1009。如果放进StringPool的String非常多， 就会造成Hash冲突严重，从而导致链表会很长，而链表长了后直接会造成的影响就是当调用String. intern时性能会大幅下降。
使用一XX： StringTableSize可设置StringTable的长度
在jdk6中StringTable是固定的，就是1009的长度，所以如果常量池中的字符串过多就会导致效率下降很快。StringTableSize设 置没有要求
在jdk7中，StringTable的长度默认值是60013
jdk8开始,1009是StringTable长度可设置的最小值
```
### 7.2 String的内存分配
```markdown
在Java语言中有8种基本数据类型和一种比较特殊的类型String。
这些
类型为了使它们在运行过程中速度更快、更节省内存，都提供了
一种常量池的概念。
常量池就类似一个Java系统级别提供的缓存。8种基本数据类型
的常量
池都是系统协调的，String类 型的常量池比较特殊。它的主要使
用方法有两种。

直接使用双引号声明出来的String对象会直接存储在常量池中。

比如： String info = "abc" ；


如果不是用双引号声明的String对象，可以使用String提
供的intern（）方法。这个后面重点谈


Java 6及以前，字符串常量池存放在永久代。
Java 7中Oracle的工程师对字符串池的逻辑做了很大的改变，
即将字符串常量池的位置调整到Java堆内。

所有的字符串都保存在堆（Heap）中，和其他普通对象一样，这
样可以让你在进行调优应用时仅需要调整堆大小就可以了。
字符串常量池概念原本使用得比较多，但是这个改动使得我们有足
够的理由让我们重新考虑在Java 7中使用String. intern（）。


Java8元空间，字符串常量在堆
```
### 7.3 StringTable为什么要调整
```markdown
1 永久代permSize默认比较小;
2 永久代的垃圾回收频率低;
```
### 7.4 String的基本操作
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227190945666.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
class Memory {
    public static void main(String[] args) {//line 1
        int i = 1;//line 2
        Object obj = new Object();//line 3
        Memory mem = new Memory();//line 4
        mem.foo(obj);//line 5
    }//line 9

    private void foo(Object param) {//line 6
        String str = param.toString();//line 7
        System.out.println(str);
    }//line 8
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227191014248.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 7.5 字符串拼接操作
```markdown
1 常量与常量的拼接结果在常量池，原理是编译期优化
2 常量池中不会存在相同内容的常量。
3 只要其中有一个是变量，结果就在堆中。变量拼接的原理
是StringBuilder
4 如果拼接的结果调用intern（）方法，则主动将常量池中还没有
的字符串对象放入池中，并返回此对象地址。
```
```java
 @Test
    public void test1(){
        String s1 = "a" + "b" + "c";//编译期优化：等同于"abc"
        String s2 = "abc"; //"abc"一定是放在字符串常量池中，将此地址赋给s2
        /*
         * 最终.java编译成.class,再执行.class
         * String s1 = "abc";
         * String s2 = "abc"
         */
        System.out.println(s1 == s2); //true
        System.out.println(s1.equals(s2)); //true
    }

    @Test
    public void test2(){
        String s1 = "javaEE";
        String s2 = "hadoop";

        String s3 = "javaEEhadoop";
        String s4 = "javaEE" + "hadoop";//编译期优化
        //如果拼接符号的前后出现了变量，则相当于在堆空间中new String()，具体的内容为拼接的结果：javaEEhadoop
        String s5 = s1 + "hadoop";
        String s6 = "javaEE" + s2;
        String s7 = s1 + s2;

        System.out.println(s3 == s4);//true
        System.out.println(s3 == s5);//false
        System.out.println(s3 == s6);//false
        System.out.println(s3 == s7);//false
        System.out.println(s5 == s6);//false
        System.out.println(s5 == s7);//false
        System.out.println(s6 == s7);//false
        //intern():判断字符串常量池中是否存在javaEEhadoop值，如果存在，则返回常量池中javaEEhadoop的地址；
        //如果字符串常量池中不存在javaEEhadoop，则在常量池中加载一份javaEEhadoop，并返回次对象的地址。
        String s8 = s6.intern();
        System.out.println(s3 == s8);//true
    }

```
```markdown
字符串拼接
```
```java
@Test
    public void test3(){
        String s1 = "a";
        String s2 = "b";
        String s3 = "ab";
        /*
        如下的s1 + s2 的执行细节：(变量s是我临时定义的）
        ① StringBuilder s = new StringBuilder();
        ② s.append("a")
        ③ s.append("b")
        ④ s.toString()  --> 约等于 new String("ab")

        补充：在jdk5.0之后使用的是StringBuilder,
        在jdk5.0之前使用的是StringBuffer
         */
        String s4 = s1 + s2;//
        System.out.println(s3 == s4);//false
    }

    /*
    1. 字符串拼接操作不一定使用的是StringBuilder!
       如果拼接符号左右两边都是字符串常量或常量引用，则仍然使用编译期优化，即非StringBuilder的方式。
    2. 针对于final修饰类、方法、基本数据类型、引用数据类型的量的结构时，能使用上final的时候建议使用上。
     */
    @Test
    public void test4(){
        final String s1 = "a";
        final String s2 = "b";
        String s3 = "ab";
        String s4 = s1 + s2;
        System.out.println(s3 == s4);//true
    }
    
    //练习：
    @Test
    public void test5(){
        String s1 = "javaEEhadoop";
        String s2 = "javaEE";
        String s3 = s2 + "hadoop";
        System.out.println(s1 == s3);//false

        final String s4 = "javaEE";//s4:常量
        String s5 = s4 + "hadoop";
        System.out.println(s1 == s5);//true

    }
    

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227191248856.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 7.6 拼接操作与append的效率对比
```markdown
append效率要比字符串拼接高很多
```
```java
/*
    体会执行效率：通过StringBuilder的append()的方式添加字符串的效率要远高于使用String的字符串拼接方式！
    详情：① StringBuilder的append()的方式：自始至终中只创建过一个StringBuilder的对象
          使用String的字符串拼接方式：创建过多个StringBuilder和String的对象
         ② 使用String的字符串拼接方式：内存中由于创建了较多的StringBuilder和String的对象，内存占用更大；如果进行GC，需要花费额外的时间。

     改进的空间：在实际开发中，如果基本确定要前前后后添加的字符串长度不高于某个限定值highLevel的情况下,建议使用构造器实例化：
               StringBuilder s = new StringBuilder(highLevel);//new char[highLevel]
     */
    @Test
    public void test6(){

        long start = System.currentTimeMillis();

//        method1(100000);//4014
        method2(100000);//7

        long end = System.currentTimeMillis();

        System.out.println("花费的时间为：" + (end - start));
    }

    public void method1(int highLevel){
        String src = "";
        for(int i = 0;i < highLevel;i++){
            src = src + "a";//每次循环都会创建一个StringBuilder、String
        }
//        System.out.println(src);

    }

    public void method2(int highLevel){
        //只需要创建一个StringBuilder
        StringBuilder src = new StringBuilder();
        for (int i = 0; i < highLevel; i++) {
            src.append("a");
        }
//        System.out.println(src);
    }

```
### 7.7 intern()的使用
```markdown
如果不是用双引号声明的String对象，可以使用String提供
的intern方法： intern方法会从字符串常量池中查询当前字符
串是否存在，若不存在就会将当前字符串放入常量池中。

比如： String myInfo = new String("I love u").intern()；
也就是说，如果在任意字符串上调用String. intern方法，那么其
返回结果所指向的那个类实例，必须和直接以常量形式出现的
字符串实例完全相同。因此，下 列表达式的值必定是true：
（"a" + "b" + "c"）.intern（）== "abc";
通俗点讲，Interned String就是确保字符串在内存里只有一份拷
贝，这样可以节约内存空间，加快字符串操作任务的执行速
度。注意，这个值会被存放在字符串内
部池（String Intern Pool）。
```
```markdown
new String("ab")会创建几个对象,
new String("a")+new String("b")呢
```
```java
public class StringNewTest {
    public static void main(String[] args) {
//        String str = new String("ab");

        String str = new String("a") + new String("b");
    }
}

```
```markdown
new String("ab")会创建几个对象？看字节码，就知道是两个。
一个对象是：new关键字在堆空间创建的
另一个对象是：字符串常量池中的对象"ab"。 字节码指令：ldc
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227191624785.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
new String("a") + new String("b")呢？

对象1：new StringBuilder()
对象2： new String("a")
对象3： 常量池中的"a"
对象4： new String("b")
对象5： 常量池中的"b"
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021022719165924.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
深入剖析： StringBuilder的toString():
对象6 ：new String("ab")
强调一下，toString()的调用，在字符串常量池中，没有生成"ab"
```
```markdown
关于String.intern()的面试题
```

```java
/**
 * 如何保证变量s指向的是字符串常量池中的数据呢？
 * 有两种方式：
 * 方式一： String s = "shkstart";//字面量定义的方式
 * 方式二： 调用intern()
 *         String s = new String("shkstart").intern();
 *         String s = new StringBuilder("shkstart").toString().intern();
 *
 */
public class StringIntern {
    public static void main(String[] args) {
        String s = new String("1");
        String s1 = s.intern();//调用此方法之前，字符串常量池中已经存在了"1"
        String s2 = "1";
        //s  指向堆空间"1"的内存地址
        //s1 指向字符串常量池中"1"的内存地址
        //s2 指向字符串常量池已存在的"1"的内存地址  所以 s1==s2
        System.out.println(s == s2);//jdk6：false   jdk7/8：false
        System.out.println(s1 == s2);//jdk6: true   jdk7/8：true
        System.out.println(System.identityHashCode(s));//491044090
        System.out.println(System.identityHashCode(s1));//644117698
        System.out.println(System.identityHashCode(s2));//644117698

        //s3变量记录的地址为：new String("11")
        String s3 = new String("1") + new String("1");
        //执行完上一行代码以后，字符串常量池中，是否存在"11"呢？答案：不存在！！

        //在字符串常量池中生成"11"。如何理解：jdk6:创建了一个新的对象"11",也就有新的地址。
        //         jdk7:此时常量中并没有创建"11",而是创建一个指向堆空间中new String("11")的地址
        s3.intern();
        //s4变量记录的地址：使用的是上一行代码代码执行时，在常量池中生成的"11"的地址
        String s4 = "11";
        System.out.println(s3 == s4);//jdk6：false  jdk7/8：true
    }

}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227191805775.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227191814959.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
拓展
```
```java
public class StringIntern1 {
    public static void main(String[] args) {
        //StringIntern.java中练习的拓展：
        String s3 = new String("1") + new String("1");//new String("11")
        //执行完上一行代码以后，字符串常量池中，是否存在"11"呢？答案：不存在！！
        String s4 = "11";//在字符串常量池中生成对象"11"
        String s5 = s3.intern();
        System.out.println(s3 == s4);//false
        System.out.println(s5 == s4);//true
    }
}

```
```markdown
总结String的intern（）的使用

jdk1.6中，将这个字符串对象尝试放入串池。

如果字符串常量池中有，则并不会放入。返回已有的串池中的
对象的地址
如果没有，会把此对象复制一份，放入串池，并返回串池中的对
象地址


Jdk1.7起，将这个字符串对象尝试放入串池。

如果字符串常量池中有，则并不会放入。返回已有的串池中的对象
的地址
如果没有，则会把对象的引用地址复制一份，放入串池，并返回串池中
的引用地址
```
```java
public class StringExer1 {
    public static void main(String[] args) {
        //String x = "ab";
        String s = new String("a") + new String("b");//new String("ab")
        //在上一行代码执行完以后，字符串常量池中并没有"ab"

        String s2 = s.intern();//jdk6中：在串池中创建一个字符串"ab"
                               //jdk8中：串池中没有创建字符串"ab",而是创建一个引用，指向new String("ab")，将此引用返回

        System.out.println(s2 == "ab");//jdk6:true  jdk8:true
        System.out.println(s == "ab");//jdk6:false  jdk8:true
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227192006721.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227192018245.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227192044889.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
public class StringExer2 {
    public static void main(String[] args) {
        String s1 = new String("ab");//执行完以后，会在字符串常量池中会生成"ab"
//        String s1 = new String("a") + new String("b");////执行完以后，不会在字符串常量池中会生成"ab"
        s1.intern();
        String s2 = "ab";
        System.out.println(s1 == s2); //false
    }
}

```
### 7.8 intern()效率测试
```markdown
大的网站平台，需要内存中存储大量的字符串。比如社交网站，
很多人都存储：北京市、海淀区等信息。这时候如果字符串都调
用 intern（）方法，就会明显降低内存的大小。
```
```java
/**
 * 使用intern()测试执行效率：空间使用上
 *
 * 结论：对于程序中大量存在存在的字符串，尤其其中存在很多重复字符串时，使用intern()可以节省内存空间。
 *
 */
public class StringIntern2 {
    static final int MAX_COUNT = 1000 * 10000;
    static final String[] arr = new String[MAX_COUNT];

    public static void main(String[] args) {
        Integer[] data = new Integer[]{1,2,3,4,5,6,7,8,9,10};

        long start = System.currentTimeMillis();
        for (int i = 0; i < MAX_COUNT; i++) {
//            arr[i] = new String(String.valueOf(data[i % data.length]));
            arr[i] = new String(String.valueOf(data[i % data.length])).intern();

        }
        long end = System.currentTimeMillis();
        System.out.println("花费的时间为：" + (end - start));

        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.gc();
    }
}

```
### 7.9 StrtingTable的垃圾回收
```java
/**
 * String的垃圾回收:
 * -Xms15m -Xmx15m -XX:+PrintStringTableStatistics -XX:+PrintGCDetails
 *
 */
public class StringGCTest {
    public static void main(String[] args) {
//        for (int j = 0; j < 100; j++) {
//            String.valueOf(j).intern();
//        }
        //发生垃圾回收行为
        for (int j = 0; j < 100000; j++) {
            String.valueOf(j).intern();
        }
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227192359237.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 7.10 G1中的String去重操作
```markdown
背景：对许多Java应用（有大的也有小的）做的测试得出以下结果：

堆存活数据集合里面String对象占了25%
堆存活数据集合里面重复的String对象有13.5%
String对象的平均长度是45


许多大规模的Java应用的瓶颈在于内存，测试表明，在这些类型
的应用
里面，Java堆中存活的数据集合差不多258是String对象。
更进一一步，这里面差不多一半String对象是重复的，重复的
思是说：
string1. equals （string2）true。堆上存在重复的string对
象必然是一种内存的浪费。这个项目将在G1垃圾收集器
中实现自动持续对重复的String对象进行去重，这样就能避免浪
费内存。

实现

当垃圾收集器工作的时候，会访问堆上存活的对象。对每一个访问
的对象都会检查是否是候选的要去重的String对象。
如果是，把这个对象的一个引用插入到队列中等待后续的处理。一
个去重的线程在后台运行，处理这个队列。处理队列的一个元素意
味着从队列删除这个元素，然后尝试去重它引用的String对象。
使用一个hashtable来记录所有的被String对象使用的不重复
的char数组。
当去重的时候，会查这个hashtable，来看堆上是否已经存在一个一
模一样的char数组。
如果存在，String对象会被调整引用那个数组，释放对原来的数组的
引用，最终会被垃圾收集器回收掉。
如果查找失败，char数组会被插入到hashtable，这样以后的时候
就可以共享这个数组了。

命令行选项

UseStringDeduplication （bool） ：开启String去重，默认
是不开启的，需要手动开启。
PrintStringDedupl icationStatistics （bool） ：打印
详细的去重统计信息，
StringDedupl icationAgeThreshold （uintx） ：达到这个
年龄的string对象被认.为是去重的候选对象
```

```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复jvm1
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
