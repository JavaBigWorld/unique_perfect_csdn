# 深入理解JVM-篇章2
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210716111239959.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1  垃圾回收
### 1.1 什么是垃圾
```markdown
Java = (C++)--

什么是垃圾（ Garbage） 呢？
垃圾是指在运行程序中没有任何指针指向的对象，这个对象就是
需要被回收的垃圾。
外文： An object is considered garbage when it can no
longer be reached from any pointer in the runningprogram.
如果不及时对内存中的垃圾进行清理，那么，这些垃圾对象所占
的内存空
间会一直保留到应用程序结束，被保留的空间无法被其他对象使用。
甚至可能导致内存溢出。

垃圾收集，不是Java语言的伴生产物。早在1960年，第一门开始
使用内存动态分配和垃圾收集技术的Lisp语言诞生。

关于垃圾收集有三个经典问题：

哪些内存需要回收？
什么时候回收？
如何回收？


垃圾收集机制是Java的招牌能力，极大地提高了开发效率。
如今，垃圾收
集几乎成为现代语言的标配，即使经过如此长时间的发展，
Java的垃圾收集机制仍然在不断的演进中，不同大小的设备、
不同特征的应用场景，对垃圾收集提出了新的挑战，这当然
也是面试的热点。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227203028633.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

### 1.2 为什么需要GC
```markdown
对于高级语言来说，一个基本认知是如果不进行垃圾回收，
内存迟早都会被消耗完，因为不断地分配内存空间而不进
行回收，就好像不停地生产生活垃圾而从来不打扫一样。
除了释放没用的对象，垃圾回收也可以清除内存里的记
录碎片。碎片整理将所占用的堆内存移到堆的一端，以便
JVM 将整理出的内存分配给新的对象。
随着应用程序所应付的业务越来越庞大、复杂，用户越来
越多，没有GC就不能保证应用程序的正常进行。而经常造
成STW的GC又跟不上实际的需求，所以才会不断地尝试
对GC进行优化。
```
### 1.3 早期垃圾回收
```markdown
在早期的C/C++时代，垃圾回收基本.上是手工进行的。开
发人员可以使用 new关键字进行内存申请，并使用delete
关键字进行内存释放。比如以下代码
```
```java
MibBridge *pBridge = new cmBaseGroupBridge （） ；
//如果注册失败，使用Delete释放该对象所占内存区域
if （pBridge->Register（kDestroy）！= NO_ERROR）
delete pBridge；
```
```markdown
这种方式可以灵活控制内存释放的时间，但是会给开发人
员带来频繁申请和释放内存的管理负担。倘若有一处内存
区间由于程序员编码的问题忘记被回收，那么就会产生
内存泄漏，垃圾对象永远无法被清除，随着系统运行时间
的不断增长，垃圾对象所耗内存可能持续上升，直到出
现内存溢出并造成应用程序崩溃。
在有了垃圾回收机制后，上述代码块极有可能变成这样：
```
```java
MibBridge *pBridge = new cmBaseGroupBridge()；
pBridge 一> Register（kDestroy）；
```
```markdown
现在，除了Java以外，C#、Python、 Ruby等语言都使
用了自动垃圾回收的思想，也是未来发展趋势。可以说，
这种自动化的内存分配和垃圾回收的方式己经成为现代
开发语言必备的标准。
```
### 1.4 Java垃圾回收机制
```markdown
自动内存管理，无需开发人员手动参与内存的分配与回收，这
样降低内存泄漏和内存溢出的风险

没有垃圾回收器，java也会和cpp一样，各种悬垂指针，野
指针，泄露问题让你头疼不已。


自动内存管理机制，将程序员从繁重的内存管理中释放出来，
可以更专心地专注于业务开发

对于Java开发人员而言，自动内存管理就像是一个黑匣子，
如果过度依赖于
“自动”，那么这将会是一场灾难，最严重的就会弱化Java开
发人员在程序出现内存溢出时定位问题和解决问题的能力。
此时，了 解JVM的自动内存分配和内存回收原理就显得非
常重要，只有在真
正了解JVM是如何管理内存后，我们才能够在
遇见OutOfMemoryError时，
快速地根据错误异常日志定位问题和解决问题。
当需要排查各种内存溢出、内存泄漏问题时，当垃圾收集成为
系统达到更高
并发量的瓶颈时，我们就必须对这些“自动化”的技术实施必要的监
控和调节。
垃圾回收器可以对年轻代回收，也可以对老年代回收，甚至是全堆
和方法区的回收。

其中，Java堆是垃圾收集器的工作重点。


从次数上讲：

频繁收集Young区
较少收集Old区
基本不动Perm区
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227203559572.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 1.5 垃圾回收相关算法
```markdown
垃圾标记阶段:对象存活判断

在堆里存放着几乎所有的Java对象实例，在GC执行垃圾回收之前，
首先需要区分出内存中哪些是存活对象，哪些是已经死亡的
对象。只有被标记为己经死亡的对象，GC才会在执行垃圾回
收时，释放掉其所占用的内存空间，因此这个过程我们可以称
为垃圾标记阶段。
那么在JVM中究竟是如何标记一个死亡对象呢？简单来说，
当一个对象已经不再被任何的存活对象继续引用时，就可以
宣判为已经死亡。
判断对象存活一般有两种方式：引用计数算法和可达性分
析算法。
```
#### 1.5.1 标记阶段:法1_引用计数法 (java没有采用)
```markdown
引用计数算法（Reference Counting）比较简单，对每个对象
保存一个整型 的引用计数器属性。用于记录对象被引用的情况。
对于一个对象A，只要有任何一个对象引用了A，则A的引用计
数器就加1；当引用失效时，引用计数器就减1。只要对象A的
引用计数器的值为0，即表示对象A不可能再被使用，可进行回收。
优点：实现简单，垃圾对象便于辨识；判定效率高，回收没
有延迟性。
缺点：

它需要单独的字段存储计数器，这样的做法增加了存储空间的开销。
每次赋值都需要更新计数器，伴随着加法和减法操作，这增加了
时间开销。
引用计数器有一个严重的问题，即无法处理循环引用的情况。这
是一 条致命缺陷，导致在Java的垃圾回收器中没有使用这类算法。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227204041137.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
图示分析证明java没有采用引用计数法
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227204113940.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
如果不下小心直接把Obj1 一reference和Obj2 一reference
置null。 则在Java堆当中的两块内存依然保持着互相引用，
无法回收。
```
```java
/**
 * -XX:+PrintGCDetails
 * 证明：java使用的不是引用计数算法
 */
public class RefCountGC {
    //这个成员属性唯一的作用就是占用一点内存
    private byte[] bigSize = new byte[5 * 1024 * 1024];//5MB

    Object reference = null;

    public static void main(String[] args) {
        RefCountGC obj1 = new RefCountGC();
        RefCountGC obj2 = new RefCountGC();

        obj1.reference = obj2;
        obj2.reference = obj1;

        obj1 = null;
        obj2 = null;
        //显式的执行垃圾回收行为
        //这里发生GC，obj1和obj2能否被回收？
        System.gc();

        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```
```java
小结

引用计数算法， 是很多语言的资源回收选择，例如因人
工智能而更加火热的Python，它更是同时支持引用计数
和垃圾收集机制。
具体哪种最优是要看场景的，业界有大规模实践中仅
保留引用计数机制，以提高吞吐量的尝试。
Java并没有选择引用计数，是因为其存在一个基本的
难题，也就是很难处理循环引用关系。
Python 如何解决循环引用？

手动解除： 很好理解，就是在合适的时机，解除引用关系。
使用弱引用weakref，weakref是Python提供的标准库，
旨在解决循环引用。
```
#### 1.5.2 标记阶段:法2_可达性分析算法
```markdown
也叫根搜索算法或追踪性垃圾收集

相对于引用计数算法而言，可达性分析算法不仅同样具
备实现简单和执行高
效等特点，更重要的是该算法可以有效地解决在引用计数
算法中循环引用的问题，防止内存泄漏的发生。
相较于引用计数算法，这里的可达性分析就是Java、C#选择的。
这种类型的垃圾收集通常也叫作追踪性垃圾收集
（Tracing GarbageCollection）。
所谓"GC Roots"根集合就是一组必须活跃的引用。
基本思路：

可达性分析算法是以根对象集合(GCRoots）为起始点，
按照从上至下的方式搜索被根对象集合所连接的目标对
象是否可达。
使用可达性分析算法后，内存中的存活对象都会被根对
象集合直接或间接连接着，搜索所走过的路径称为引
用链（Reference Chain）
如果目标对象没有任何引用链相连，则是不可达的，就
意味着该对象己经死亡，可以标记为垃圾对象。
在可达性分析算法中，只有能够被根对象集合直接或
者间接连接的对象才是存活对象。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227204334859.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 1.5.2.1 GC Roots
```markdown
在Java语言中，GC Roots包括以下几类元素：

虚拟机栈中引用的对象

比如：各个线程被调用的方法中使用到的参数、局部变量等。


本地方法栈内JNI（通常说的本地方法）引用的对象
方法区中类静态属性引用的对象

比如：Java类的引用类型静态变量


方法区中常量引用的对象

比如：字符串常量池（string Table） 里的引用


所有被同步锁synchronized持有的对象
Java虚拟机内部的引用。

基本数据类型对应的Class对象，一些常驻的异常对象（如：
NullPointerException、OutOfMemoryError） ，
系统类加载器。


反映java虚拟机内部情况的JMXBean、JVMTI中注册的回调、
本地代码缓存等
除了这些固定的GCRoots集合以外，根据用户所选用的垃圾
收集器以及当
前回收的内存区域不同，还可以有其他对象“临时性”地加入，
共同构成完整GC Roots集合。比如：分代收集和局部
回收（Partial GC）。

如果只针对Java堆中的某一块区域进行垃圾回收（比如：典型的只针
对新生代），必须考虑到内存区域是虚拟机自己的实现细节，
更不是孤立封闭的，这个区域的对象完全有可能被其他区域的对
象所引用，这时候就需要一并将关联的区域对象也加入GC Roots
集合中去考虑，才能保证可达性分析的准确性。


小技巧：由于Root采用栈方式存放变量和指针，所以如果一个
指针，它保存了堆内存里面的对象，但是自己又不存放在堆
内存里面，那它就是一个Root
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227204508819.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
注意

如果要使用可达性分析算法来判断内存是否可回收，那么分析
工作必须在
一个能保障一致性的快照中进行。这点不满足的话分析结果的
准确性就无法保证。
这点也是导致GC进行时必须“StopTheWorld"的一个重要原因。

即使是号称（几乎）不会发生停顿的CMS收集器中，枚举根节
点时也是必须要停顿的。
```
#### 1.5.3 对象的finalization机制
```markdown
Java语言提供了对象终止（finalization）机制来允许开发人员提
供对象被销毁之前的自定义处理逻辑。
当垃圾回收器发现没有引用指向一个对象，即：垃圾回收此对象
之前，总会先调用这个对象的finalize（）方法。
finalize（）方法允许在子类中被重写，用于在对象被回收时
进行资源释放。通常在这个方法中进行一些资源释放和清理的工
作，比如关闭文件、套接字和数据库连接等。
应该交给垃圾回收机制调用。理由包括下面三点：永远不要主
动调用某个对象的finalize （）方法

在finalize()时可能会导致对象复活。
finalize()方法的执行时间是没有保障的，它完全由Gc线程决定，
极端情况下，若不发生GC，则finalize() 方法将没有执行机会。
一个糟糕的finalize()会严重影响GC的性能。


从功能上来说，finalize()方法与C++ 中的析构函数比较相似，
但是Java采用的是基于垃圾回收器的自动内存管理机制，所以
finalize()方法在本质，上不同于C++ 中的析构函数。
```
```markdown
对象是否"死亡"

由于finalize ()方法的存在，虚拟机中的对象一般处于三种可能的
状态
如果从所有的根节点都无法访问到某个对象，说明对象己经不再
使用了。一般来说，此对象需要被回收。但事实上，也并非是“非
死不可”的，这时候它们暂时处于“缓刑”阶段。一个无法触及的对
象有可能在某一个条件下“复活”自己，如果这样，那么对它的回
收就是不合理的，为此，定义虚拟机中的对象可能的三种状态。如下：

可触及的：从根节点开始，可以到达这个对象。
可复活的：对象的所有引用都被释放，但是对象有可能
在finalize()中复活。
不可触及的：对象的finalize()被调用，并且没有复活，那么
就会进入不可触及状态。不可触及的对象不可能被复活，因
为finalize()只会被调用一一次。


以上3种状态中，是由于finalize()方法的存在，进行的区分。
只有在对象不可触及时才可以被回收。
判定是否可以回收具体过程
判定一个对象objA是否可回收，至少要经历两次标记过程：


如果对象objA到GC Roots没有引用链，则进行第一 次标记。
进行筛选，判断此对象是否有必要执行finalize（）方法

1 如果对 象objA没有重写finalize（）方法，或者finalize （）
方法已经被虚拟机调用过，则虚拟机视为“没有必要执行”，
objA被判定为不可触及的。
2 如果对象objA重写了finalize（）方法，且还未执行过，那
么objA会被插入到F一Queue队列中，由一个虚拟机自动创
建的、低优先级的Finalizer线程触发其finalize（）方法执行。
会对F一Queue队列中的对象进行第二次标记。
如果objA在finalize（）方法中与引用链上的任何一个对象
建立了联系，那么在第二次标记时，objA会被移出“即将
回收”集合。之后，对象会再次出现没有引用存在的情况。
在这个情况下，finalize方法不会被再次调用，对象会
直接变成不可触及的状态，也就是说，一个对象的finalize方
法只会被调用一次。
```
##### 1.5.3.1 代码测试可复活的对象
```java
/**
 * 测试Object类中finalize()方法，即对象的finalization机制。
 *
 */
public class CanReliveObj {
    public static CanReliveObj obj;//类变量，属于 GC Root


    //此方法只能被调用一次
    @Override
    protected void finalize() throws Throwable {
        super.finalize();
        System.out.println("调用当前类重写的finalize()方法");
        obj = this;//当前待回收的对象在finalize()方法中与引用链上的一个对象obj建立了联系
    }


    public static void main(String[] args) {
        try {
            obj = new CanReliveObj();
            // 对象第一次成功拯救自己
            obj = null;
            System.gc();//调用垃圾回收器
            System.out.println("第1次 gc");
            // 因为Finalizer线程优先级很低，暂停2秒，以等待它
            Thread.sleep(2000);
            if (obj == null) {
                System.out.println("obj is dead");
            } else {
                System.out.println("obj is still alive");
            }
            System.out.println("第2次 gc");
            // 下面这段代码与上面的完全相同，但是这次自救却失败了
            obj = null;
            System.gc();
            // 因为Finalizer线程优先级很低，暂停2秒，以等待它
            Thread.sleep(2000);
            if (obj == null) {
                System.out.println("obj is dead");
            } else {
                System.out.println("obj is still alive");
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

```
```markdown
第1次 gc
调用当前类重写的finalize()方法
obj is still alive
第2次 gc
obj is dead
```
#### 1.5.4 MAT与JProfiler的GC Roots溯源
```markdown
MAT是Memory Analyzer的简称，它是一 款功能强大的Java堆
内存分析器。用于查找内存泄漏以及查看内存消耗情况。
MAT是基于Eclipse开发的，是一款免费的性能分析工具。
```
```markdown
获取dump文件
方式1: 命令行使用jmap

jps
jmap -dump:format=b,live,file=test1.bin {进程id}

方式2：使用JVisualVM导出
捕获的heap dump文件是一个临时文件，关闭JVisua1VM后自
动删除，若要保留，需要将其另存为文件。
可通过以下方法捕获heap dump：

在左侧“Application”（应用程序）子窗口中右击相应的应用程
序，选择Heap Dump（堆Dump)。
在Monitor （监视）子标签页中点击Heap Dump （堆Dump）按钮。


本地应用程序的Heap dumps作为应用程序标签页的一个子标签
页打开。同时，heap dump在左侧的Application （应用程序）
栏中对应一个含有时间戳的节点。右击这个节点选择save as 
（另存为）即可将heap dump保存到本地。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227205407906.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
GC Roots分析
```
```java
public class GCRootsTest {
    public static void main(String[] args) {
        List<Object> numList = new ArrayList<>();
        Date birth = new Date();

        for (int i = 0; i < 100; i++) {
            numList.add(String.valueOf(i));
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        System.out.println("数据添加完毕，请操作：");
        new Scanner(System.in).next();
        numList = null;
        birth = null;

        System.out.println("numList、birth已置空，请操作：");
        new Scanner(System.in).next();

        System.out.println("结束");
    }
}

```
```java
使用MAT查看GC Roots
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227205516715.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
使用jProfiler进行GC溯源
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227205551614.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
使用Jprofiler分析OOM
```
```java
/**
 * -Xms8m -Xmx8m -XX:+HeapDumpOnOutOfMemoryError
 *
 */
public class HeapOOM {
    byte[] buffer = new byte[1 * 1024 * 1024];//1MB

    public static void main(String[] args) {
        ArrayList<HeapOOM> list = new ArrayList<>();

        int count = 0;
        try{
            while(true){
                list.add(new HeapOOM());
                count++;
            }
        }catch (Throwable e){
            System.out.println("count = " + count);
            e.printStackTrace();
        }
    }
}

```

```markdown
控制台输出

java.lang.OutOfMemoryError: Java heap space
Dumping heap to java_pid45386.hprof ...
Heap dump file created [7390812 bytes in 0.019 secs]
count = 6
java.lang.OutOfMemoryError: Java heap space
	at com.dsh.jvm.gc.algorithm.HeapOOM.<init>(HeapOOM.java:12)
	at com.dsh.jvm.gc.algorithm.HeapOOM.main(HeapOOM.java:20)

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227205729339.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
出现OOM的代码
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227205804650.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 1.5.5 清除阶段:法1_标记-清除算法
```markdown
当成功区分出内存中存活对象和死亡对象后，GC接下来的任务
就是执行垃圾回收，释放掉无用对象所占用的内存空间，以便有
足够的可用内存空间为新对象分配内存.
目前在JVM中比较常见的三种垃圾收集算法是标记一清除
算法（ Mark一Sweep）、复制算法（Copying）、
标记一压缩算法（Mark一Compact）
  
背景：
标记一清除算法（Mark一Sweep）是一种非常基础和常见的
垃圾收集算法，该算法被J . McCarthy等人在1960年提出并
并应用于Lisp语言。


执行过程：
当堆中的有效内存空间（available memory） 被耗尽的时候，
就会停止整个程序（也被称为stop the world），然后进行两
项工作，第一项则是标记，第二项则是清除。

标记： Collector从引用根节点开始遍历，标记所有被引用
的对象。一般是在对象的Header中记录为可达对象。
清除： Collector对堆 内存从头到尾进行线性的遍历，如果发
现某个对象在其Header中没有标记为可达对象，则将其回收。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210227212446938.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
缺点

效率不算高
在进行Gc的时候，需要停止整个应用程序，导致用户体验差
这种方式清理出来的空闲内存是不连续的，产生内存碎片。
需要维护一个空闲列表

注意：何为清除？

这里所谓的清除并不是真的置空，而是把需要清除的对
象地址保存在空闲
的地址列表里。下次有新对象需要加载时，判断垃圾的位置
空间是否够，如果够，就存放。
```
#### 1.5.6 清除阶段:法2_复制算法
```markdown
背景：
为了解决标记一清除算法在垃圾收集效率方面的缺陷，M.L.Minsky于1963年发表了著名的论文，“ 使用双存储区的Li sp语言垃圾收集器CALISP Garbage Collector Algorithm Using SerialSecondary Storage ）”。M.L. Minsky在该论文中描述的算法被人们称为复制（Copying）算法，它也被M. L.Minsky本人成功地引入到了Lisp语言的一个实现版本中。

核心思想：
将活着的内存空间分为两块，每次只使用其中一块，在垃圾回
收时将正在.使用的内存中的存活对象复制到未被使用的内存
块中，之后清除正在使用的内存块中的所有对象，交换两个
内存的角色，最后完成垃圾回收。

堆中S0和S1使用的就是复制算法
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210228102601129.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
优点：

没有标记和清除过程，实现简单，运行高效
复制过去以后保证空间的连续性，不会出现“碎片”问题。

缺点：

此算法的缺点也是很明显的，就是需要两倍的内存空间。
对于G1这种分拆成为大量region的GC，复制而不是移动，
意味着GC需要维护region之间对象引用关系，不管是内存
占用或者时间开销也不小。
特别的
如果系统中的垃圾对象很多，复制算法不会很理想,复制算法
需要复制的存活对象数量并不会太大，或者说非常低才行。
```

```markdown
应用场景：
在新生代，对常规应用的垃圾回收，一次通常可以回收70%一 99%的
内存空间。回收性价比很高。所以现在的商业虚拟机都是用这种收
集算法回收新生代。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210228102840784.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 1.5.7 清除阶段:法3_标记-压缩(整理,Mark-Compact)算法
```markdown
背景：
复制算法的高效性是建立在存活对象少、垃圾对象多的前提下的。
这种情况在新生代经常发生，但是在老年代，更常见的情况是
大部分对象都是存活对象。如果依然使用复制算法，由于存活对
象较多，复制的成本也将很高。因此，基于老年代垃圾回收的特
性，需要使用其他的算法。
标记一清除算法的确可以应用在老年代中，但是该算法不仅执行
效率低下，而且在执行完内存回收后还会产生内存碎片，所以
JVM的设计者需要在此基础之上进行改进。标记一压缩
（Mark一Compact） 算法由此诞生。
1970年前后，G. L. Steele 、C. J. Chene和D.S. Wise 
等研究者发布标记一压缩算法。在许多现代的垃圾收集器中，人们
都使用了标记一压缩算法或其改进版本。
执行过程：


第一阶段和标记一清除算法一样，从根节点开始标记所有被引用对象.


第二阶段将所有的存活对象压缩到内存的一端，按顺序排放。


之后，清理边界外所有的空间。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210228104227631.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
标记一压缩算法的最终效果等同于标记一清除算法执行完成后，再
进行一次内存碎片整理，因此，也可以把它称为标记一清除一压缩
（Mark一 Sweep一Compact）算法。


二者的本质差异在于标记一清除算法是一种非移动式的回
收算法，标记一压.缩是移动式的。是否移动回收后的存活对象
是一项优缺点并存的风险决策。


可以看到，标记的存活对象将会被整理，按照内存地址依次排列，而未被标记的内存会被清理掉。如此一来，当我们需要给新对象分配内存时，JVM只需要持有一个内存的起始地址即可，这比维护一个空闲列表显然少了许多开销。
```
##### 1.5.7.1 指针碰撞（Bump the Pointer ）
```markdown
如果内存空间以规整和有序的方式分布，即已用和未用的内存都各
自一边，彼此之间维系着一个记录下一次分配起始点的标记指针，
当为新对象分配内存时，只需要通过修改指针的偏移量将新对象
分配在第一个空闲内存位置上，这种分配方式就叫做指针
碰撞（Bump the Pointer） 。
```

```markdown
优点

消除了标记一清除算法当中，内存区域分散的缺点，我们需要给
新对象分配内存时，JVM只 需要持有一个内存的起始地址即可。
消除了复制算法当中，内存减半的高额代价。

缺点

从效率.上来说，标记一整理算法要低于复制算法。
移动对象的同时，如果对象被其他对象引用，则还需要调整
引用的地址。移动过程中，需要全程暂停用户应用程序。即： STW
```
#### 1.5.8 小结
```markdown
效率上来说，复制算法是当之无愧的老大，但是却浪费了太多内存。
而为了尽量兼顾上面提到的三个指标，标记一整理算法相对来说
更平滑一些，但是效率.上不尽如人意，它比复制算法多了一个标记
的阶段，比标记一清除多了一个整理内存的阶段。
```
|  |Mark-Sweep  | Mark-Compact | Copying |
|--|--|--|--|
|速度  |中等  |最慢  |最快  |
|空间开销  |少(但会堆积碎片)  |少(不堆积碎片)  |通常需要活对象的2倍大小(不堆积碎片)  |
|移动对象  |否  |是  |是  |

#### 1.5.9 分代收集算法

```markdown
难道就没有一种最优的算法么?
没有最好的算法,只有更合适的算法
前面所有这些算法中，并没有一种算法可以完全替代其他算
法，它们都具有自己独特的优势和特点。分代收集算法应运而生。
分代收集算法，是基于这样一个事实：不同的对象的生命周
期是不一样的。因此，不同生命周期的对象可以采取不同的收
集方式，以便提高回收效率。一般是把Java堆分为新生代和老年代，
这样就可以根据各个年代的特点使用不同的回收算法，以提高垃圾回收的效率。

在Java程序运行的过程中，会产生大量的对象，其中有
些对象是与业务信息相关，比如Http请求中的Session对象、线程、
Socket连接， 这类对象跟业务直接挂钩，因此生命周期比较长。
但是还有一些对象，主要是程序运行过程中生成的临时变量，这
些对象生命周期会比较短，比如： String对象， 由于其不变类
的特性，系统会产生大量的这些对象，有些对象甚至只用一次即可回收。
  目前几乎所有的GC都是采用分代收集（Generational Collecting）
算法执行垃圾回收的。
  在HotSpot中，基于分代的概念，GC所使用的内存回收算法必须结
合年轻代和老年代各自的特点。


年轻代（Young Gen）

年轻代特点：区域相对老年代较小，对象生命周期短、存活率低，
回收频繁。
这种情况复制算法的回收整理，速度是最快的。复制算法的效率只和当
前存活对象大小有关，因此很适用于年轻代的回收。而复制算法内存利
用率不高的问题，通过hotspot中的两个survivor的设计得到缓解。



老年代（Tenured Gen）

老年代特点：区域较大，对象生命周期长、存活率高，回收不
及年轻代频繁。
这种情况存在大量存活率高的对象，复制算法明显变得不合适。
一般是由标记一清除或者是标记一清除与标记一整理的混合实现。

Mark阶段的开销与存活对象的数量成正比。
Sweep阶段的开销与所管理区域的大小成正相关。
Compact阶 段的开销与存活对象的数据成正比。





以HotSpot中的CMS回收器为例，CMS是基于Mark一 Sweep实现
的，对于对象的回收效率很高。而对于碎片问题，CMS采用基
于Mark一Compact算法的Serial 0ld回收器作为补偿措施：
当内存回收不佳（碎片导致的Concurrent Mode
分代的思想被现有的虚拟机广泛使用。几乎所有的垃圾回收器
都区分新生代和老年代。
```
#### 1.5.10 增量收集算法、分区算法
##### 1.5.10.1 增量收集算法
```markdown
上述现有的算法，在垃圾回收过程中，应用软件将处于一
种stop the World的状态。在Stop the World状态下，应用程
序所有的线程都会挂起，暂停一切正常的工作，等待垃圾回收的完成。
如果垃圾回收时间过长，应用程序会被挂起很久，将严重影响用户
体验或者系统的稳定性。为了解决这个问题，即对实时垃圾收集算法
的研究直接导致了增量收集（Incremental Collecting） 算法的
诞生。

基本思想
  如果一次性将所有的垃圾进行处理，需要造成系统长时间的停顿，
那么就可以让垃圾收集线程和应用程序线程交替执行。每次，垃圾
收集线程只收集一小片区域的内存空间，接着切换到应用程序
线程。依次反复，直到垃圾收集完成。
  总的来说，增量收集算法的基础仍是传统的标记一清除和复制算法。
增量收集算法通过对线程间冲突的妥善处理，允许垃圾收集线程
以分阶段的方式完成标记、清理或复制工作。
缺点：
使用这种方式，由于在垃圾回收过程中，间断性地还执行了应用程序
代码，所以能减少系统的停顿时间。但是，因为线程切换和上下
文转换的消耗，会使得垃圾回收的总体成本上升，造成系统吞吐量
的下降。
```

##### 1.5.10.2 分区算法

```markdown
一般来说，在相同条件下，堆空间越大，一次GC时所需要的时间就
越长，有关GC产生的停顿也越长。为了更好地控制GC产生的停顿时
间，将一块 大的内存区域分割成多个小块，根据目标的停顿时间，
每次合理地回收若干个小区间，而不是整个堆空间，从而减少一次
GC所产生的停顿。
  分代算法将按照对象的生命周期长短划分成两个部分，分区算法
将整个堆空间划分成连续的不同小区间。
  每一个小区间都独立使用，独立回收。这种算法的好处是可以控
制一次回收多少个小区间。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210228133109788.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
注意，这些只是基本的算法思路，实际GC实现过程要复杂的多，
目前还在发展中的前沿GC都是复合算法，并且并行和并发兼备。
```

### 1.6 垃圾回收相关概念

```markdown
System.gc()的理解

在默认情况下，通过System.gc （）
或者Runtime . getRuntime（） .gc（）的调用，
会显式触发Full GC，同时对老年代和新生代进行回收，尝试释放被丢
弃对象占用的内存。
然而System.gc（）调用附带一个免责声明，无法保证对垃圾收集
器的调用(无法保证马上触发GC)。
JVM实现者可以通过system.gc（）调用来决定JVM的GC行为。而一
般情况下，垃圾回收应该是自动进行的，无须手动触发，否则就太过于
麻烦了。在一些特殊情况下，如我们正在编写一个性能基准，我们可
以在运行之，间调用System.gc（）。
以下代码,如果注掉System.runFinalization(); 那么控制台不
保证一定打印,证明了System.gc（）无法保证GC一定执行。
```
```java
public class SystemGCTest {
    public static void main(String[] args) {
        new SystemGCTest();
        System.gc();//提醒jvm的垃圾回收器执行gc,但是不确定是否马上执行gc
        //与Runtime.getRuntime().gc();的作用一样。
        System.runFinalization();//强制调用使用引用的对象的finalize()方法
    }

    @Override
    protected void finalize() throws Throwable {
        super.finalize();
        System.out.println("SystemGCTest 重写了finalize()");
    }
}

```
```markdown
手动gc理解不可达对象的回收行为
```
```java
public class LocalVarGC {
    public void localvarGC1() {
        byte[] buffer = new byte[10 * 1024 * 1024];//10MB
        System.gc();
        //输出: 不会被回收, FullGC时被放入老年代
        //[GC (System.gc()) [PSYoungGen: 14174K->10736K(76288K)] 14174K->10788K(251392K), 0.0089741 secs] [Times: user=0.01 sys=0.00, real=0.01 secs]
        //[Full GC (System.gc()) [PSYoungGen: 10736K->0K(76288K)] [ParOldGen: 52K->10649K(175104K)] 10788K->10649K(251392K), [Metaspace: 3253K->3253K(1056768K)], 0.0074098 secs] [Times: user=0.01 sys=0.02, real=0.01 secs]
    }

    public void localvarGC2() {
        byte[] buffer = new byte[10 * 1024 * 1024];
        buffer = null;
        System.gc();
        //输出: 正常被回收
        //[GC (System.gc()) [PSYoungGen: 14174K->544K(76288K)] 14174K->552K(251392K), 0.0011742 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
        //[Full GC (System.gc()) [PSYoungGen: 544K->0K(76288K)] [ParOldGen: 8K->410K(175104K)] 552K->410K(251392K), [Metaspace: 3277K->3277K(1056768K)], 0.0054702 secs] [Times: user=0.01 sys=0.00, real=0.01 secs]

    }

    public void localvarGC3() {
        {
            byte[] buffer = new byte[10 * 1024 * 1024];
        }
        System.gc();
        //输出: 不会被回收, FullGC时被放入老年代
        //[GC (System.gc()) [PSYoungGen: 14174K->10736K(76288K)] 14174K->10784K(251392K), 0.0076032 secs] [Times: user=0.02 sys=0.00, real=0.01 secs]
        //[Full GC (System.gc()) [PSYoungGen: 10736K->0K(76288K)] [ParOldGen: 48K->10649K(175104K)] 10784K->10649K(251392K), [Metaspace: 3252K->3252K(1056768K)], 0.0096328 secs] [Times: user=0.01 sys=0.01, real=0.01 secs]
    }

    public void localvarGC4() {
        {
            byte[] buffer = new byte[10 * 1024 * 1024];
        }
        int value = 10;
        System.gc();
        //输出: 正常被回收
        //[GC (System.gc()) [PSYoungGen: 14174K->496K(76288K)] 14174K->504K(251392K), 0.0016517 secs] [Times: user=0.01 sys=0.00, real=0.00 secs]
        //[Full GC (System.gc()) [PSYoungGen: 496K->0K(76288K)] [ParOldGen: 8K->410K(175104K)] 504K->410K(251392K), [Metaspace: 3279K->3279K(1056768K)], 0.0055183 secs] [Times: user=0.00 sys=0.00, real=0.01 secs]
    }

    public void localvarGC5() {
        localvarGC1();
        System.gc();
        //输出: 正常被回收
        //[GC (System.gc()) [PSYoungGen: 14174K->10720K(76288K)] 14174K->10744K(251392K), 0.0121568 secs] [Times: user=0.02 sys=0.00, real=0.02 secs]
        //[Full GC (System.gc()) [PSYoungGen: 10720K->0K(76288K)] [ParOldGen: 24K->10650K(175104K)] 10744K->10650K(251392K), [Metaspace: 3279K->3279K(1056768K)], 0.0101068 secs] [Times: user=0.01 sys=0.02, real=0.01 secs]
        //[GC (System.gc()) [PSYoungGen: 0K->0K(76288K)] 10650K->10650K(251392K), 0.0005717 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
        //[Full GC (System.gc()) [PSYoungGen: 0K->0K(76288K)] [ParOldGen: 10650K->410K(175104K)] 10650K->410K(251392K), [Metaspace: 3279K->3279K(1056768K)], 0.0045963 secs] [Times: user=0.01 sys=0.00, real=0.00 secs]
    }

    public static void main(String[] args) {
        LocalVarGC local = new LocalVarGC();
        local.localvarGC5();
    }
}

```
### 1.7 内存溢出与内存泄漏

```markdown
内存溢出相对于内存泄漏来说，尽管更容易被理解，但是同样的，
内存溢出也是引发程序崩溃的罪魁祸首之一。
由于GC一直在发展，所有一般情况下，除非应用程序占用的内存
增长速度非常快，造成垃圾回收已经跟不上内存消耗的速度，否
则不太容易出现O0M的情况。
大多数情况下，GC会进行各种年龄段的垃圾回收，实在不行
了就放大招，来一次独占式的Full GC操作，这时候会回收大
量的内存，供应用程序继续使用。
javadoc中对OutOfMemoryError的解释是，没有空闲内存，
并且垃圾收集器也无法提供更多内存。

内存溢出

首先说没有空闲内存的情况：说明Java虚拟机的堆内存不够。
原因有二：

（1） Java虚拟机的堆内存设置不够。
比如：可能存在内存泄漏问题；也很有可能就是堆的大小不合理，比
如我们要处理比较可观的数据量，但是没有显式指定JVM堆大小或者指
定数值偏小。我们可以通过参数一Xms、一Xmx来调整。
（2）代码中创建了大量大对象，并且长时间不能被垃圾收集器收集
（存在被引用）对于老版本的Oracle JDK，因为永久代的大小是有
限的，并且JVM对永久代垃圾回收（如，常量池回收、卸载不再
需要的类型）非常不积极，所以当我们不断添加新类型的时候，永久
代出现OutOfMemoryError也非常多见，尤其是在运行时存在大量动
态类型生成的场合；类似intern字符串缓存占用太多空间，也会导致
0OM问题。对应的异常信息，会标记出来和永久代相关：
 "java. lang. OutOfMemoryError： PermGen space"。
随着元数据区的引入，方法区内存已经不再那么窘迫，所以相应
的00M有所改观，出现00M，异常信息则变成了：
“java. lang. OutOfMemoryError： Metaspace"。 直接内
存不足，也会导致0OM。


这里面隐含着一层意思是，在抛出0utOfMemoryError之 前，通常垃
圾收集器会被触发，尽其所能去清理出空间。

例如：在引用机制分析中，涉及到JVM会去尝试回收软引用指向的
对象等。
在java.nio.BIts.reserveMemory（）方法中，我们能清楚的
看到，System.gc（）会被调用，以清理空间。


当然，也不是在任何情况下垃圾收集器都会被触发的

比如，我们去分配一一个超大对象，类似一个超大数组超过堆的
最大值，JVM可以判断出垃圾收集并不能解决这个问题，所以直
接拋出OutOfMemoryError



内存泄漏(Memory Leak)

也称作“存储渗漏”。严格来说，只有对象不会再被程序用到了，
但是GC又不能回收他们的情况，才叫内存泄漏。
但实际情况很多时候一些不太好的实践（或疏忽）会导致对象的生命
周期变得很长甚至导致OOM，也可以叫做宽泛意义上的“内存泄漏
尽管内存泄漏并不会立刻引起程序崩溃，但是一旦发生内存泄漏，程
序中的可用内存就会被逐步蚕食，直至耗尽所有内存，最终出现
OutOfMemory异常，导致程序崩溃。
注意，这里的存储空间并不是指物理内存，而是指虚拟内存大小，
这个虚拟内存大小取决于磁盘交换区设定的大小。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210228153738565.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
举例

1 单例模式
单例的生命周期和应用程序是一样长的，所以单例程序中，如果持
有对外部对象的引用的话，那么这个外部对象是不能被回收的，则
会导致内存泄漏的产生。
2 一些提供close的资源未关闭导致内存泄漏
数据库连接（ dataSourse. getConnection（）），网络连接
（socket）和io连接必须手动close，否则是不能被回收的。
```
### 1.8 Stop The World
```markdown
Stop一the一World，简称STW，指的是Gc事件发生过程中，会产
生应用程序的停顿。停顿产生时整个应用程序线程都会被暂停，
没有任何响应，有点像卡死的感觉，这个停顿称为STW。.

可达性分析算法中枚举根节点（GC Roots）会导致所有Java执行
线程停顿。.

分析工作必须在一个能确保一致性的快照 中进行
一致性指整个分析期间整个执行系统看起来像被冻结在某个时
点上V- - 如果出现分析过程中对象引用关系还在不断变化，则
分析结果的准确性无法保证




被STW中断的应用程序线程会在完成GC之后恢复，频繁中断会让
用户感觉像是网速不快造成电影卡带一样， 所以我们需要减少
STW的发生。
STW事件和采用哪款GC无关，所有的GC都有这个事件。
哪怕是G1也不能完全避免Stop一the一world情况发生，只能说
垃圾回收器越来越优秀，回收效率越来越高，尽可能地缩短了
暂停时间。STW是JVM在后台自动发起和自动完成的。在用户
不可见的情况下，
把用户正常的工作线程全部停掉。
开发中不要用System.gc（）；会导致Stop一the一world的发生。
```
```java
public class StopTheWorldDemo {
    public static class WorkThread extends Thread {
        List<byte[]> list = new ArrayList<byte[]>();

        public void run() {
            try {
                while (true) {
                    for(int i = 0;i < 1000;i++){
                        byte[] buffer = new byte[1024];
                        list.add(buffer);
                    }

                    if(list.size() > 10000){
                        list.clear();
                        System.gc();//会触发full gc，进而会出现STW事件
                    }
                }
            } catch (Exception ex) {
                ex.printStackTrace();
            }
        }
    }

    public static class PrintThread extends Thread {
        public final long startTime = System.currentTimeMillis();

        public void run() {
            try {
                while (true) {
                    // 每秒打印时间信息
                    long t = System.currentTimeMillis() - startTime;
                    System.out.println(t / 1000 + "." + t % 1000);
                    Thread.sleep(1000);
                }
            } catch (Exception ex) {
                ex.printStackTrace();
            }
        }
    }

    public static void main(String[] args) {
        WorkThread w = new WorkThread();
        PrintThread p = new PrintThread();
        w.start();
        p.start();
    }
}

```
### 1.9 垃圾回收的并行与并发
```markdown
并发(Concurrent)

在操作系统中，是指一个时间段中有几个程序都处于己启动运行到
运行完毕之间，且这几个程序都是在同一个处理器_上运行。
并发不是真正意义上的“同时进行”，只是CPU把一个时间段划分成
几个时间片段（时间区间），然后在这几个时间区间之间来回切
换，由于CPU处理的速度非常快，只要时间间隔处理得当，即可让
用户感觉是多个应用程序同时在进行。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210228161153139.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
并行(Parallel)

当系统有一个以上CPU时，当一个CPU执行一个进程时，另一个CPU
可以执行另一个进程，两个进程互不抢占CPU资源，可以同时进
行，我们称之为并行（Parallel）。
其实决定并行的因素不是CPU的数量，而是CPU的核心数量，比
如一个CPU多个核也可以
并行。
适合科学计算，后台处理等弱交互场景
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021022816125882.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
垃圾回收的并发与并行
并发和并行，在谈论垃圾收集器的上下文语境中，它们可以解释如下：


并行（Parallel） ：指多条垃圾收集线程并行工作，但此时用户
线程仍处于等待状态。

如ParNew、 Parallel Scavenge、 Parallel 0ld；



串行（Serial）

相较于并行的概念，单线程执行。
如果内存不够，则程序暂停，启动JVM垃圾回收器进行垃圾回收。
回收完，再启动程序的线程。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021022816133762.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
并发（Concurrent） ：指用户线程与垃圾收集线程同时执行
（但不一定是并行的，可能会交替执行），垃圾回收线程在执
行时不会停顿用户程序的运行。

用户程序在继续运行，而垃圾收集程序线程运行于另一
个CPU上；
如： CMS、G1
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210228161443597.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 1.10 安全点与安全区域
```markdown
安全点(Safepoint)

程序执行时并非在所有地方都能停顿下来开始GC，只有在特定的位置
才能停顿下来开始GC，这些位置称为“安全点（Safepoint） ”
Safe Point的选择很重要，如果太少可能导致GC等待的时间太长，
如果太频繁可能导致运行时的性能问题。大部分指令的执行时间都非
常短暂，通常会根据“是否具有让程序长时间执行的特征”为标准。
比如：选择些执行时间较长的指令作为Safe Point， 如方法调用、
循环跳转和异常跳转等。


如何在GC发生时，检查所有线程都跑到最近的安全点停顿下来呢？

抢先式中断： （目前没有虚拟机采用了）
首先中断所有线程。如果还有线程不在安全点，就恢复线程，让线程
跑到安全点。
主动式中断：
设置一个中断标志，各个线程运行到Safe Point的时候主动轮询这个
标志，如果中断标志为真，则将自己进行中断挂起。


安全区域(Safe Region)
  Safepoint机制保证了程序执行时，在不太长的时间内就会遇到可进入GC的Safepoint 。但是，程序“不执行”的时候呢？例如线程处于Sleep 状态或Blocked状态，这时候线程无法响应JVM的中断请求，“走” 到安全点去中断挂起，JVM也不太可能等待线程被唤醒。对于这种情况，就需要安全区域（Safe Region）来解决。
  安全区域是指在一段代码片段中，对象的引用关系不会发生
变化，在这个区域中的任何位置开始GC都是安全的。我们也可以
把Safe Region 看做是被扩展了的Safepoint。

实际执行时:

1 当线程运行到Safe Region的代码时，首先标识已经进入
了Safe Region，如果这段时间内发生GC，JVM会 忽略标识
为Safe Region状态 的线程；
2 当线程即将离开Safe Region时， 会检查JVM是否已经完成GC，
如果完成了，则继续运行，否则线程必须等待直到收到可以安全
离开SafeRegion的信号为止；
```
### 1.11 引用
```markdown
我们希望能描述这样一类对象： 当内存空间还足够时，则能保留
在内存中；如果内存空间在进行垃圾收集后还是很紧张，则可以
抛弃这些对象。
【既偏门又非常高频的面试题】强引用、软引用、弱引用、虚引用有什么区别？具体使用.场景是什么？
在JDK 1.2版之后，Java对引用的概念进行了扩充，将引用分为
强引用（Strong Reference）、软引用（Soft Reference） 、弱引用（Weak Reference） 和虚引用（Phantom Reference） 
4种，这4种引用强度依次逐渐减弱。
除强引用外，其他3种引用均可以在java.lang.ref包中找到
它们的身影。如下图，显示了这3种引用类型对应的类，开发人员可
以在应用程序
中直接使用它们。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210228200524849.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
Reference子类中只有终结器引用是包内可见的，其他3种引用类
型均为public，可以在应用程序中直接使用

强引用（StrongReference）：最传统的“引用”的定义，是指在程
序代码之中普遍存在的引用赋值，即类似“0bject obj=new object（ ）”
这种引用关系。无论任何情况下，只要强引用关系还存在，垃圾收集
器就永远不会回收掉被引用的对象。
软引用（SoftReference） ：在系统将要发生内存溢出之前，
将会把这些对象列入回收范围之中进行第二次回收。如果这次回收
后还没有足够的内存，才会抛出内存溢出异常。
弱引用（WeakReference） ：被弱引用关联的对象只能生存到下一
次垃圾收集之前。当垃圾收集器工作时，无论内存空间是否足够，都
会回收掉被弱引用关联的对象。
虚引用（PhantomReference） ：一个对象是否有虛引用的存在，
完全不会对其生存时
间构成影响，也无法通过虚引用来获得一个对象的实例。为一个对
象设置虛引用关联的唯一目的就是能在这个对象被收集器回收时
收到一个系统通知(回收跟踪)。
```
#### 1.11.1 强引用: 不回收
```markdown
在Java程序中，最常见的引用类型是强引用（普通系统99%以上都
是强引用），也就是我们最常见的普通对象引用，也是默认的引用
类型。当在Java语言中使用new操作符创建一个新的对象， 并将其
赋值给一个变量的时候，这个变量就成为指向该对象的一个强引用。
强引用的对象是可触及的，垃圾收集器就永远不会回收掉被引用的对象。
对于一一个普通的对象，如果没有其他的引用关系，只要超过了引用
的作用域或者显式地将相应（强）引用赋值为null，就是可以当做
垃圾被收集了，当然具体回收时机还是要看垃圾收集策略。
相对的，软引用、 弱引用和虚引用的对象是软可触及、弱可触及和
虛可触及的，在一定条件下，都是可以被回收的。所以，强引用是造成
Java内存泄漏的主要原因之一。
```
```java
public class StrongReferenceTest {
    public static void main(String[] args) {
        StringBuffer str = new StringBuffer ("Hello,尚硅谷");
        StringBuffer str1 = str;

        str = null;
        System.gc();

        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println(str1);
    }
}

```
```markdown
强引用可以直接访问目标对象。
强引用所指向的对象在任何时候都不会被系统回收，虚拟机宁愿抛
出OOM异常，也不会回收强引用所指向对象。
强引用可能导致内存泄漏。
```
#### 1.11.2 软引用: 内存不足即回收
```markdown
软引用是用来描述一 些还有用，但非必需的对象。只被软引用
关联着的对象，在系统将要发生内存溢出异常前，会把这些对象
列进回收范围之中进行第二次回收，如果这次回收还没有足够的
内存，才会抛出内存溢出异常。

软引用通常用来实现内存敏感的缓存。比如：高速缓存就有用到软引
用。如果还有空闲内存，就可以暂时保留缓存，当内存不足时清
理掉，这样就保证了使用缓存的同时，不会耗尽内存。
垃圾回收器在某个时刻决定回收软可达的对象的时候，会清理软引
用，并可选地把引用存放到一个引用队列（ Reference Queue）。
类似弱引用，只不过Java虚拟机会尽量让软引用的存活时间长一些，
迫不得.已才清理。
软引用：

当内存足够: 不会回收软引|用的可达对象
当内存不够时: 会回收软引用的可达对象


在JDK 1. 2版之后提供了java.lang.ref.SoftReference类来实
现软引用。
```
```java
Object obj = new object（）； //声明强引用
SoftReference<0bject> sf = new SoftReference<0bject>（obj）；
obj = null； //销毁强引用

```
```java
/**
 * 软引用的测试：内存不足即回收
 * -Xms10m -Xmx10m -XX:+PrintGCDetails
 */
public class SoftReferenceTest {
    public static class User {
        public User(int id, String name) {
            this.id = id;
            this.name = name;
        }

        public int id;
        public String name;

        @Override
        public String toString() {
            return "[id=" + id + ", name=" + name + "] ";
        }
    }

    public static void main(String[] args) {
        //创建对象，建立软引用
//        SoftReference<User> userSoftRef = new SoftReference<User>(new User(1, "songhk"));
        //上面的一行代码，等价于如下的三行代码
        User u1 = new User(1,"songhk");
        SoftReference<User> userSoftRef = new SoftReference<User>(u1);
        u1 = null;//取消强引用


        //从软引用中重新获得强引用对象
        System.out.println(userSoftRef.get());

        System.gc();
        System.out.println("After GC:");
//        //垃圾回收之后获得软引用中的对象
        System.out.println(userSoftRef.get());//由于堆空间内存足够，所有不会回收软引用的可达对象。
//
        try {
            //让系统认为内存资源紧张、不够
//            byte[] b = new byte[1024 * 1024 * 7];
            byte[] b = new byte[1024 * 7168 - 399 * 1024];//恰好能放下数组又放不下u1的内存分配大小 不会报OOM
        } catch (Throwable e) {
            e.printStackTrace();
        } finally {
            //再次从软引用中获取数据
            System.out.println(userSoftRef.get());//在报OOM之前，垃圾回收器会回收软引用的可达对象。
        }
    }
}

```
#### 1.11.3 弱引用: 发现即回收
```markdown
弱引用也是用来描述那些非必需对象，被弱引用关联的对象只能生存
到下一次垃圾收集发生为止。在系统GC时，只要发现弱引用，不管
系统堆空间使用是否充足，都会回收掉只被弱引用关联的对象。
但是，由于垃圾回收器的线程通常优先级很低，因此，并不一 定
能很快地发现持有弱引用的对象。在这种情况下，弱引用对象可
以存在较长的时间。
弱引用和软引用一样，在构造弱引用时，也可以指定一个引用队
列，当弱引用对象被回收时，就会加入指定的引用队列，通过
这个队列可以跟踪对象的回收情况。
软引用、弱引用都非常适合来保存那些可有可无的缓存数据。如
果这么做，当系统内存不足时，这些缓存数据会被回收，不
会导致内存溢出。而当内存资源充足时，这些缓存数据又可以
存在相当长的时间，从而起到加速系统的作用。
在JDK1.2版之后提后了java.lang.ref.WeakReference类来
实现弱引用
```
```java
Object obj = new object（）； //声明强引用
WeakReference<0bject> sf = new WeakReference<0bject>（obj）；
obj = null； //销毁强引用
```
```java
弱引用对象与软引用对象的最大不同就在于，当GC在进行回收时，
需要通过算法检查是否回收软引用对象，而对于弱引用对象，
GC总是进行回收。弱引用对象更容易、更快被GC回收。
面试题：你开发中使用过WeakHashMap吗？

通过查看WeakHashMap源码,可以看到其内部类Entry使用
的就是弱引用
line 702 -> private static class Entry<K,V> extends WeakReference<Object> implements Map.Entry<K,V> {...}
```
```java
public class WeakReferenceTest {
    public static class User {
        public User(int id, String name) {
            this.id = id;
            this.name = name;
        }

        public int id;
        public String name;

        @Override
        public String toString() {
            return "[id=" + id + ", name=" + name + "] ";
        }
    }

    public static void main(String[] args) {
        //构造了弱引用
        WeakReference<User> userWeakRef = new WeakReference<User>(new User(1, "songhk"));
        //从弱引用中重新获取对象
        System.out.println(userWeakRef.get());

        System.gc();
        // 不管当前内存空间足够与否，都会回收它的内存
        System.out.println("After GC:");
        //重新尝试从弱引用中获取对象
        System.out.println(userWeakRef.get());
    }
}

```
#### 1.11.4 虚引用: 对象回收跟踪

```markdown
虚引用(Phantom Reference),也称为“幽灵引用”或者“幻影引用”，是所有
引用类型中最弱的一个。
一个对象是否有虚引用的存在，完全不会决定对象的生命周期。如
果一个对象仅持有虚引用，那么它和没有引用几乎是一样的，随时
都可能被垃圾回收器回收。
它不能单独使用，也无法通过虚引用来获取被引用的对象。当试图
通过虚引用的get（）方法取得对象时，总是null。
为一个对象设置虚引用关联的唯一目的在于跟踪垃圾回收过程。
比如：能在这个对象被收集器回收时收到一个系统通知。
虚引用必须和引用队列一起使用。虚引用在创建时必须提供一个
引用队列作为参数。当垃圾回收器准备回收一个对象时，如果发
现它还有虛引用，就会在回收对象后，将这个虚引用加入引用队
列，以通知应用程序对象的回收情况。
由于虚引用可以跟踪对象的回收时间，因此，也可以将一些资
源释放操作放置在虛引用中执行和记录。
在JDK 1. 2版之后提供了PhantomReference类来实现虚引用。
```

```java
object obj = new object();
ReferenceQueuephantomQueue = new ReferenceQueue( ) ;
PhantomReference<object> pf = new PhantomReference<object>(obj, phantomQueue); 
obj = null;
```
```java
public class PhantomReferenceTest {
    public static PhantomReferenceTest obj;//当前类对象的声明
    static ReferenceQueue<PhantomReferenceTest> phantomQueue = null;//引用队列

    public static class CheckRefQueue extends Thread {
        @Override
        public void run() {
            while (true) {
                if (phantomQueue != null) {
                    PhantomReference<PhantomReferenceTest> objt = null;
                    try {
                        objt = (PhantomReference<PhantomReferenceTest>) phantomQueue.remove();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    if (objt != null) {
                        System.out.println("追踪垃圾回收过程：PhantomReferenceTest实例被GC了");
                    }
                }
            }
        }
    }

    @Override
    protected void finalize() throws Throwable { //finalize()方法只能被调用一次！
        super.finalize();
        System.out.println("调用当前类的finalize()方法");
        obj = this;
    }

    public static void main(String[] args) {
        Thread t = new CheckRefQueue();
        t.setDaemon(true);//设置为守护线程：当程序中没有非守护线程时，守护线程也就执行结束。
        t.start();

        phantomQueue = new ReferenceQueue<PhantomReferenceTest>();
        obj = new PhantomReferenceTest();
        //构造了 PhantomReferenceTest 对象的虚引用，并指定了引用队列
        PhantomReference<PhantomReferenceTest> phantomRef = new PhantomReference<PhantomReferenceTest>(obj, phantomQueue);

        try {
            //不可获取虚引用中的对象
            System.out.println(phantomRef.get());

            //将强引用去除
            obj = null;
            //第一次进行GC,由于对象可复活，GC无法回收该对象
            System.gc();
            Thread.sleep(1000);
            if (obj == null) {
                System.out.println("obj 是 null");
            } else {
                System.out.println("obj 可用");
            }
            System.out.println("第 2 次 gc");
            obj = null;
            System.gc(); //一旦将obj对象回收，就会将此虚引用存放到引用队列中。
            Thread.sleep(1000);
            if (obj == null) {
                System.out.println("obj 是 null");
            } else {
                System.out.println("obj 可用");
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

```
```markdown
null
调用当前类的finalize()方法
obj 可用
第 2 次 gc
追踪垃圾回收过程：PhantomReferenceTest实例被GC了
obj 是 null

```
#### 1.11.5 终结器引用
```markdown
它用以实现对象的finalize（）方法，也可以称为终结器引用。
无需手动编码， 其内部配合引用队列使用。
在GC时， 终结器引用入队。由Finali zer线程通过终结器引用
找到被引用对象并调用它的finalize（）方法，第二次GC时才
能回收被引用对象。
```
### 1.12 垃圾回收器
#### 1.12.1 GC的分类与性能指标
```markdown
垃圾收集器没有在规范中进行过多的规定，可以由不同的厂商、
不同版本的JVM来实现。
由于JDK的版本处于高速迭代过程中，因此Java发展至今已经衍生
了众多的GC版本。
从不同角度分析垃圾收集器，可以将GC分为不同的类型。

按线程数分，可以分为串行垃圾回收器和并行垃圾回收器
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021022822340920.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
串行回收指的是在同一时间段内只允许有一个CPU用于执行垃圾回
收操作，此时工作线程被暂停，直至垃圾收集工作结束。

在诸如单CPU处理器或者较小的应用内存等硬件平台不是特别优
越的场
合，串行回收器的性能表现可以超过并行回收器和并发回收器。
所以，串行回收默认被应用在客户端的Client模式下的JVM中
在并发能力比较强的CPU上，并行回收器产生的停顿时间要短于
串行回收器。


和串行回收相反，并行收集可以运用多个CPU同时执行垃圾回收，
因此提升了应用的吞吐量，不过并行回收仍然与串行回收一样，
采用独占式，使用了“ Stop一the一world”机制。

按照工作模式分，可以分为并发式垃圾回收器和独占式垃圾回收器
并发式垃圾回收器与应用程序线程交替工作，以尽可能减少应用
程序的停顿时间。
独占式垃圾回收器（Stop the world）一旦运行，就停止应用程序
中的所有用户线程，直到垃圾回收过程完全结束。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210228223527351.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
按碎片处理方式分，可分为压缩式垃圾回收器和非压缩式垃圾回收器

压缩式垃圾回收器会在回收完成后，对存活对象进行压缩整理，消除
回收后的碎片。
再分配对象空间使用: 指针碰撞
非压缩式的垃圾回收器不进行这步操作。
再分配对象空间使用: 空闲列表

按工作的内存区间分，又可分为年轻代垃圾回收器和老年代垃圾回收器
```
#### 1.12.2 评估GC的性能指标
```markdown
吞吐量：运行用户代码的时间占总运行时间的比例
（总运行时间：程序的运行时间十内存回收的时间）



垃圾收集开销：吞吐量的补数，垃圾收集所用时间与总运行时间的比例。


暂停时间：执行垃圾收集时，程序的工作线程被暂停的时间


收集频率：相对于应用程序的执行，收集操作发生的频率。


内存占用： Java堆区所占的内存大小


快速：一个对象从诞生到被回收所经历的时间。


这三者共同构成一个“不可能三角”。三者总体的表现会随着技术
进步而越来越好。一款优秀的收集器通常最多同时满足其中的两项。


这三项里，暂停时间的重要性日益凸显。因为随着硬件发展，内存占用
多些越来越能容忍，硬件性能的提升也有助于降低收集器运行时对
应用程序的影响，即提高了吞吐量。而内存的扩大，对延迟反而带来
负面效果。


简单来说，主要抓住两点：

吞吐量
暂停时间



吞吐量

吞吐量就是CPU用于运行用户代码的时间与CPU总消耗时间的比值，
即吞吐量运行用户代码时间/ （运行用户代码时间+垃圾收集时间）

比如：虚拟机总共运行了100分钟，其中垃圾收集花掉1分钟，那吞
吐量就是99%


这种情况下，应用程序能容忍较高的暂停时间，因此，高吞吐量的应
用程序有更长的时间基准，快速响应是不必考虑的。
吞吐量优先，意味着在单位时间内，STW的时间最短： 0.2 + 0.2 = 0.4

暂停时间

“暂停时间”是指一个时间段内应用程序线程暂停，让GC线程执行的状态

例如，GC期间100毫秒的暂停时间意味着在这100毫秒期间内没有应用程
序线程是活动的。.


暂停时间优先，意味着尽可能让单次STW的时间最短： 0.1 + 0.1 + 0.1 +
0.1+0.1=0.5
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210228223830712.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
高吞吐量较好因为这会让应用程序的最终用户感觉只有应用程序线程在
做“生产性”工作。直觉上，吞吐量越高程序运行越快。
低暂停时间（低延迟）较好因为从最终用户的角度来看不管是GC还
是其他原因导致一个应用被挂起始终是不好的。这取决于应用程序
的类型，有时候甚至短暂的200毫秒暂停都可能打断终端用户体验。
因此，具有低的较大暂停时间是非常重要的，特别是对于一一个
交互式应用程序。
不幸的是”高吞吐量”和”低暂停时间”是一对相互竞争的目标（矛盾）。

因为如果选择以吞吐量优先，那么必然需要降低内存回收的执行
频率，但是这样会导致GC需要更长的暂停时间来执行内存回收。
相反的，如果选择以低延迟优先为原则，那么为了降低每次执行内
存回收时的暂停时间，也只能频繁地执行内存回收，但这又引起了
年轻代内存的缩诚和导致程序吞吐量的下降。


在设计（或使用） GC算法时，我们必须确定我们的目标： 一个GC
算法只可能针对两个目标之一（即只专注于较大吞吐量或最小
暂停时间），或.尝试找到一个二者的折衷。
现在标准：在最大吞吐量优先的情况下，降低停顿时间。
```
#### 1.12.3 不同的垃圾回收器概述
```markdown
垃圾收集机制是Java的招牌能力，极大地提高了开发效率。这当然
也是面试的热点。
那么，Java常见的垃圾收集器有哪些？
```
##### 1.12.3.1 垃圾收集器发展史
```markdown
有了虚拟机，就一定需要收集垃圾的机制，这就是
Garbage Collection， 对应的产品我们称
为Garbage Collector.

1999年随JDK1.3.1一 起来的是串行方式的Serial GC，它是第一
款GC。ParNew垃圾收集器是Serial收集器的多线程版本
2002年2月26日，Parallel GC和Concurrent Mark Sweep GC
跟随JDK1.4.2一起发布
Parallel GC在JDK6之后成为HotSpot默认GC。
2012年，在JDK1.7u4版本中，G1可用。
2017年，JDK9中G1变成默认的垃圾收集器，以替代CMS。
2018年3月，JDK10中G1垃圾回收器的并行完整垃圾回收，
实现并行性来改善最坏情况下的延迟。
------------分水岭------------
2018年9月，JDK11发布。引入Epsilon垃圾回收器，又被称
为"No一0p （无操作） "回收器。同时，引入ZGC：可伸缩的低延
迟垃圾回收器（Experimental）。
2019年3月，JDK12发布。 增强G1，自动返回未用堆内存给操作
系统。同时，引入Shenandoah GC：低停顿时间的GC （Experimental）。
2019年9月，JDK13发布。增强ZGC，自动返回未用堆内存给操作系统。
2020年3月，JDK14发布。删除CMS垃圾回收器。扩展ZGC在macOS和
Windows上的应用
```
##### 1.12.3.2 7款经典的垃圾收集器
```markdown
串行回收器：Serial. Serial Old
并行回收器：ParNew. Parallel Scavenge. Parallel Old
并发回收器：CMS. G1
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210228224210680.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 1.12.3.3 7款经典的垃圾收集器与垃圾分代之间的关系
```markdown
新生代收集器： Serial、 ParNeW、Parallel Scavenge；
老年代收集器： Serial 0ld、 Parallel 0ld、 CMS；
整堆收集器： G1；
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210228224255198.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 1.12.3.4 垃圾收集器的组合关系
```markdown
两个收集器间有连线，表明它们可以搭配使用：
Serial/Serial 01d、Serial/CMS、 ParNew/Serial 01d、ParNew/CMS、
Parallel Scavenge/Serial 01d、Parallel Scavenge/Parallel 0ld、G1；
其中Serial 0ld作为CMS 出现"Concurrent Mode Failure"失败的后 备预案。
3.（红色虚线）由于维护和兼容性测试的成本，在JDK 8时将Serial+CMS、
ParNew+Serial 01d这两个组合声明为废弃（JEP 173） ，并在JDK 9中
完全取消了这些组合的支持（JEP214），即：移除。
（绿色虚线）JDK 14中：弃用Parallel Scavenge和Serial0ld GC组合（JEP366 ）
（青色虚线）JDK 14中：删除CMS垃圾回收器 （JEP 363）


为什么要有很多收集器个不够吗？ 因为Java的使用场景很多， 移动端，
服务器等。所以就需要针对不同的场景，提供不同的垃圾收集器，提高
垃圾收集的性能。
虽然我们会对各个收集器进行比较，但并非为了挑选一个最好的收集器
出来。没有一种放之四海皆准、任何场景下都适用的完美收集器存在，
更加没有万能的收集器。所以我们选择的只是对具体应用最合适的收集器。
```
```markdown
查看默认的垃圾收集器
-xx：+PrintCommandLineFlags： 查看命令行相关参数（包含使用的
垃圾收集器）
使用命令行指令： jinfo 一flag相关垃圾回收器参数进程ID
```
```java
/**
 *  -XX:+PrintCommandLineFlags
 *
 *  -XX:+UseSerialGC:表明新生代使用Serial GC ，同时老年代使用Serial Old GC
 *
 *  -XX:+UseParNewGC：标明新生代使用ParNew GC
 *
 *  -XX:+UseParallelGC:表明新生代使用Parallel GC
 *  -XX:+UseParallelOldGC : 表明老年代使用 Parallel Old GC
 *  说明：二者可以相互激活
 *
 *  -XX:+UseConcMarkSweepGC：表明老年代使用CMS GC。同时，年轻代会触发对ParNew 的使用
 */
public class GCUseTest {
    public static void main(String[] args) {
        ArrayList<byte[]> list = new ArrayList<>();

        while(true){
            byte[] arr = new byte[100];
            list.add(arr);
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
-XX:InitialHeapSize=268435456 -XX:MaxHeapSize=4294967296 -XX:+PrintCommandLineFlags 
-XX:+UseCompressedClassPointers 
-XX:+UseCompressedOops -XX:+UseParallelGC 
```
```markdown
或命令行
jdk8 使用的是parallel
jdk9 使用的是G1jdk9 使用的是G1
```
#### 1.12.4 Serial回收器:串行回收
```markdown
Serial收集器是最基本、历史最悠久的垃圾收集器了。JDK1.3之
前回收新生代唯一的选择。
Serial收集器作为HotSpot中Client模式下的默认新生代垃圾收
集器。Serial收集器采用复制算法、串行回收
和"Stop一the一World"机制的方式执行内存回收。）
除了年轻代之外，Serial收集器还提供用于执行老年代垃圾收集的
Serial 0ld收集器。 Serial 0ld收集器同样也采用了串行回收
和"Stop the World"机制，只不过内存回收算法使用的是标记一
压缩算
法。

Serial 0ld是运行在Client模式下默认的老年代的垃圾回收器
Serial 0ld在Server模式下主要有两个用途：1 与新生代的
ParallelScavenge配合使用; 2 作为老年代CMS收集器的后备
垃圾收集方案


这个收集器是一个单线程的收集器，但它的“单线程”的意义并不仅
仅说明它只会使用一个CPU或一条收集线程去完成垃圾收集工作，
更重要的是在它进行垃圾收集时，必须暂停其他所有的工作线程，
直到它收集结束（Stop The World ）。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210228224939947.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
优势

简单而高效（与其他收集器的单线程比），对于限定单个CPU的环境
来说，Seria1收集器由于没有线程交互的开销，专心做垃圾收集自然
可以
获得最高的单线程收集效率。

运行在Client模式下的虛拟机是个不错的选择。


在用户的桌面应用场景中，可用内存一般不大（几十MB至一两百MB），
可以在较短时间内完成垃圾收集（几十ms至一百多ms） ，只要不频繁
发生，使用串行回收器是可以接受的。
在HotSpot虛拟机中，使用一XX： +UseSerialGC 参数可以指定年轻
代和老年代都使用串行收集器。

等价于新生代用Serial GC，且老年代用Serial 0ld GC
控制台输出 -XX:InitialHeapSize=268435456 
-XX:MaxHeapSize=4294967296 -XX:+PrintCommandLineFlags 
-XX:+UseCompressedClassPointers -XX:+UseCompressedOops 
-XX:+UseSerialGC

总结
这种垃圾收集器大家了解，现在已经不用串行的了。而且在限定
单核cpu才可以用。现在都不是单核的了。
对于交互较强的应用而言，这种垃圾收集器是不能接受的。一般
在Javaweb应用程序中是不会采用串行垃圾收集器的。
```
#### 1.12.5 ParNew回收器:并行回收
```markdown
如果说Serial GC是年轻代中的单线程垃圾收集器，那么ParNew收
集器则是Serial收集器的多线程版本。

Par是Paralle1的缩写，New： 只能处理的是新生代



ParNew收集器除了采用并行回收的方式执行内存回收外，两款
垃圾收集器之间几乎没有任何区别。ParNew收集器在年轻代中同
样也是采用复制算法、"Stop一 the一World"机制。


ParNew是很多JVM运行在Server模式下新生代的默认垃圾收集器。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210228225236435.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
对于新生代，回收次数频繁，使用并行方式高效。


对于老年代，回收次数少，使用串行方式节省资源。（CPU并行 
需要切换线程，串行可以省去切换线程的资源）


由于ParNew收集器是基于并行回收，那么是否可以断定ParNew收
集器的回收效率在任何场景下都会比Serial收集器更高效？

ParNew 收集器运行在多CPU的环境下，由于可以充分利用多CPU、
多核心等物理硬件资源优势，可以更快速地完成垃圾收集，提升
程序的吞吐量。
但是在单个CPU的环境下，ParNew收 集器不比Serial收集器更高
效。虽然Serial收集器是基于串行回收，但是由于CPU不需要频繁地
做任务切换，因此可以有效避免多线程交互过程中产生的一些额外开销。



因为除Serial外，目前只有ParNew GC能与CMS收集器配合工作


在程序中，开发人员可以通过选项"一XX： +UseParNewGC"手
动指定使用.ParNew收集器执行内存回收任务。它表示年轻代使用
并行收集器，不影响老年代。


-XX：ParallelGCThreads 限制线程数量，默认开启和CPU数据相
同的线程数。.
```
#### 1.12.6 Parallel回收器:吞吐量优先
```markdown
HotSpot的年轻代中除了拥有ParNew收集器是基于并行回收的以外，
Parallel Scavenge收集器同样也采用了复制算法、并行回收
和"Stop the World"机制。
那么Parallel收集器的出现是否多此一举？

和ParNew收集器不同，Parallel Scavenge收集 器的目标则是达到
一个可控制的吞吐量（Throughput），它也被称为吞吐量优先的
垃圾收集器。
自适应调节策略也是Parallel Scavenge 与ParNew一个重要区别。


高吞吐量则可以高效率地利用CPU 时间，尽快完成程序的运算任务|，
主
要适合在后台运算而不需要太多交互的任务。因此，常见在服务器环
境中使用。例如，那些执行批量处理、订单处理、工资支付、科学计
算的应用程序。
Parallel收集器在JDK1.6时提供了用于执行老年代垃圾收集的
Parallel 0ld收集器，用来代替老年代的Serial 0ld收集器。
Parallel 0ld收集器采用了标记一压缩算法，但同样也是基于并行
回收和”Stop一the一World"机制。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021030100100930.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
在程序吞吐量优先的应用场景中，Parallel 收集器
和Parallel 0ld收集器的组合，在Server模式下的内存
回收性能很不错。
在Java8中，默认是此垃圾收集器
```
```markdown
参数配置

-XX： +UseParallelGC手动指定 年轻代使用Parallel并行收集器
执行内存回收任务。
-XX： +UseParallel0ldGc手 动指定老年代都是使用并行回收
收集器。

分别适用于新生代和老年代。默认jdk8是开启的。
上面两个参数，默认开启一个，另一个也会被开启。（互相激活）


-XX： ParallelGCThreads设置年轻代并行收集器的线程数。
一般地，最好与CPU数量相等，以避免过多的线程数影响垃圾收集性能。

在默认情况下，当CPU数量小于8个， Paralle lGCThreads 的值
等于CPU数量。
当CPU数量大于8个， ParallelGCThreads的值等于
3+[5*CPU_ Count]/8]


-XX ：MaxGCPau3eMillis设置垃圾收集器最大停顿时间（即STw的
时间）。单位是毫秒。

为了尽可能地把停顿时间控制在MaxGCPauseMills以内，收集器
在.工作时会调整Java堆大小或者其他一些参数。
对于用户来讲，停顿时间越短体验越好。但是在服务器端，我们注重
高并发，整体的吞吐量。所以服务器端适合Parallel，进行控制。
该参数使用需谨慎。


-XX：GCTimeRatio垃圾收集时间占总时间的比例（= 1 / （N + 1））
用于衡量吞吐量的大小。

取值范围（0， 100）。默认值99，也就是垃圾回收时间不超过1号。
与前一个-XX：MaxGCPauseMillis参数有一定矛盾性。暂停时
间越长，Radio参数就容易超过设定的比例。


-XX： +UseAdaptiveSizePolicy设 置Parallel Scavenge收
集器具有自适应调节策略

在这种模式下，年轻代的大小、Eden和Survivor的比例、晋升老年
代的对象年龄等参数会被自动调整，已达到在堆大小、吞吐量和停
顿时间之间的平衡点。
在手动调优比较困难的场合，可以直接使用这种自适应的方式，仅指
定虚拟机的最大堆、目标的吞吐量（GCTimeRatio）和
停顿时间（MaxGCPauseMills），让虚拟机自己完成调优工作。
```
#### 1.12.7 CMS回收器:低延迟
```markdown
在JDK1.5时期， HotSpot推出了一款在强交互应用中几乎可认为有划
时代意义的垃圾收集器： CMS （Concurrent 一Mark 一 Sweep）
收集器，这款收集器是HotSpot虚拟机中第一款真正意义上的并发收
集器，它第一次实现了让垃圾收集线程与用户线程同时工作。
CMS收集器的关注点是尽可能缩短垃圾收集时用户线程的停顿时间。
停顿时
间越短（低延迟）就越适合与用户交互的程序，良好的响应速度能
提升用户体验。

目前很大一部分的Java应用集中在互联网站或者B/s系统的服务端上，
这类应用尤其重视服务的响应速度，希望系统停顿时间最短，以
给用户带来较好的体验。CMS收集器就非常符合这类应用的需求。


CMS的垃圾 收集算法采用标记一清除算法，并且
也会" stop一the一world"不幸的是，CMS 作为老年代的收集器，
却无法与JDK 1.4.0 中已经存在的新生代收集器Parallel 
Scavenge配合工作，所以在JDK 1. 5中使用CMS来收集老年代的时候，
新生代只能选择ParNew或者Serial收集器中的一个。
在G1出现之前，CMS使用还是非常广泛的。一直到今天，仍然有很
多系统使用CMS GC。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021030100122548.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
CMS整个过程比之前的收集器要复杂，整个过程分为4个主要阶段，
即初始标记阶段、并发标记阶段、重新标记阶段和并发清除阶段。

初始标记（Initial一Mark） 阶段：在这个阶段中，程序中所有的
工作线程都将会因为.
“Stop一the一World"机制而出现短暂的暂停，这个阶段的主要任务仅
仅只是标记出GCRoots能直接关联到的对象。一旦标记完成之后就会
恢复之前被暂停的所有应用.线程。由于直接关联对象比较小，所以
这里的速度非常快。
并发标记（Concurrent一Mark）阶段：从GC Roots的 直接关联对象
开始遍历整个对
象图的过程，这个过程耗时较长但是不需要停顿用户线程，可以
与垃圾收集线程一起并发运行。
重新标记（Remark） 阶段：由于在并发标记阶段中，程序的工
作线程会和垃圾收集线程同时运行或者交叉运行，因此为了修正并
发标记期间，因用户程序继续运作而导致标记产生变动的那一部分
对象的标记记录，这个阶段的停顿时间通常会比初始标记阶段稍长
一些，但也远比并发标记阶段的时间短。
并发清除（ Concurrent一Sweep）阶段：此阶段清理删除掉标记阶
段判断的已经死亡的对象，释放内存空间。由于不需要移动存活对
象，所以这个阶段也是可以与用户线程同时并发的

  尽管CMS收集器采用的是并发回收（非独占式），但是在其初始
化标记和再次标记这两个阶段中仍然需要执行“Stop一the一World”机制暂
停程序中的工作线程，不过暂停时间并不会太长，因此可以说明目前
所有的垃圾收集器都做不到完全不需要“Stop一the一World”，只是尽可
能地缩短暂停时间。
  由于最耗费时间的并发标记与并发清除阶段都不需要暂停工作，
所以整体的回收是低停顿的。
  另外，由于在垃圾收集阶段用户线程没有中断，所以在CMS回
收过程中，还应该确保应用程序用户线程有足够的内存可用。因此，
CMS收集器不能像其他收集器那样等到老年代几乎完全被填满了再
进行收集，而是当堆内存使用率达到某一阈值时，便开始进行回收，
以确保应用程序在CMS工作过程中依然有足够的空间支持应用程序运
行。要是CMS运行期间预留的内存无法满足程序需要，就会出现一
次“Concurrent Mode Failure”失败，这时虚拟机将启动后备预案：
临时启用Serial 0ld收集器来重新进行老年代的垃圾收集，这样
停顿时间就很长了。
  CMS收集器的垃圾收集算法采用的是标记一清除算法，这意味
着每次执行完内存回收后，由于被执行内存回收的无用对象所占
用的内存空间极有可能是不连续的一些内存块，不可避免地将会产生
一些内存碎片。 那么CMS在为新对象分配内存空间时，将无法使用
指针碰撞（Bump the Pointer） 技术，而只能够选择空闲列表（Free List） 
执行内存分配。
有人会觉得既然Mark Sweep会造成内存碎片，那么为什么不把算法
换成Mark Compact呢？
答案其实很简答，因为当并发清除的时候，用Compact整理内存
的话，原来的用户线程使用的内存还怎么用呢？要保证用户线程能继
续执行，前提的它运行的资源不受影响嘛。Mark Compact更
适合“Stop the World”这种场景”下使用
·CMS的优点： .

并发收集
低延迟

CMS的弊端：

1 会产生内存碎片，导致并发清除后，用户线程可用的空间不足。
在无法分配大对象的情况下，不得不提前触发Full GC。
2  CMS收集器对CPU资源非常敏感。在并发阶段，它虽然不会导致用
户停顿，但是会因为占用了一部分线程而导致应用程序变慢，总吞吐量
会降低。
3 CMS收集器无法处理浮动垃圾。可能出现“Concurrent Mode Failure" 失
败而导致另一次Full GC的产生。在并发标记阶段由于程序的工作线程和
垃圾收集线程是同时运行或者交叉运行的，那么在并发标记阶段如果
产生新的垃圾对象，CMS将 无法对这些垃圾对象进行标记，最终会导
致这些新产生的垃圾对象没有被及时回收，从而只能在下一次执行GC时
释放这些之前未被回收的内存空间。

参数设置

-XX：+UseConcMarkSweepGc 手动指定使用CMS收集器执行内存回收任务。

开启该参数后会自动将-XX： +UseParNewGc打开。
即： ParNew （Young区用） +CMS （0ld区用） +Serial 0ld的组合。


-XX：CMS1ni tiatingOccupanyFraction设置堆内存使用率的阈值，
一旦达到该阈值，便开始进行回收。

JDK5及以前版本的默认值为68，即当老年代的空间使用率达到
68号时，会执行一 次CMS 回收。 JDK6 5及以上版本默认值为92号
如果内存增长缓慢，则可以设置一个稍大的值，大的阈值可以有效
降低CMS的触发频率，减少老年代回收的次数可以较为明显地改善
应用程序性能。反之，如果应用程序内存使用率增长很快，则应该
降低这个阈值，以避免频繁触发老年代串行收集器。因此通过该选
项便可以有效降低Full GC的执行次数。


-XX： +UseCMSCompactAtFullCollection用于指定在执行完Full
GC后对内存空间进行压缩整理，以此避免内存碎片的产生。不过由
于内存压缩整理过程无法并发执行，所带来的问题就是停顿时间变得
更长了。
-XX：CMSFullGCsBeforeCompaction设置在执行多少次
Full GC后对内存空间进行压缩整理。
-XX：ParallelCMSThreads 设置CMS的线程数量。

CMS 默认启动的线程数是(ParallelGCThreads+3)/4，
ParallelGCThreads是年轻代并行收集器的线程数。
当CPU资源比较紧张时，受到CMs收集器线程的影响，
应用程序的性能在垃圾回收阶段可能会非常糟糕。
```
#### 1.12.8 小结
```markdown
HotSpot有这么多的垃圾回收器，那么如果有人问，Serial GC、
Parallel GC、Concurrent Mark Sweep GC这三个GC有什么不同呢？
请记住以下口令：
如果你想要最小化地使用内存和并行开销，请选Serial GC；
如果你想要最大化应用程序的吞吐量，请选Parallel GC；
如果你想要最小化GC的中断或停顿时间，请选CMS GC。

JDK 后续版本中CMS的变化

JDK9新特性： CMS被标记为Deprecate了（JEP291）

如果对JDK 9及以上版本的HotSpot虚拟机使用参数
一XX：+UseConcMarkSweepGC来开启CMS收集
器的话，用户会收到一个警告信息，提示CMS未来将会被废弃。


JDK14新特性： 删除CMS垃圾回收器（JEP363）

移除了CMS垃圾收集器，如果在JDK14中使用
一XX： +UseConcMarkSweepGC的话，JVM不会报错，
只是给出一个warning信息，但是不会exit。JVM会自动回退
以默认GC方式启动JVM
```
### 1.13 G1回收器:区域化分代式
```markdown
既然我们已经有了前面几个强大的GC，为什么还要发布Garbage First （G1）GC？
  原因就在于应用程序所应对的业务越来越庞大、复杂，用户越来
越多，没有GC就不能保证应用程序正常进行，而经常造成STW的GC
又跟不上实际的需求，所以才会不断地尝试对GC进行优化。G1 
（Garbage一First） 垃圾回收器是在Java7 update4之后引入的一个
新的垃圾回收器，是当今收集器技术发展的最前沿成果之一。
  与此同时，为了适应现在不断扩大的内存和不断增加的处理器数
量，进一步降低暂停时间（pause time） ，同时兼顾良好的吞吐量。
  官方给G1设定的目标是在延迟可控的情况下获得尽可能高的吞吐
量，所以才担当起“全功能收集器”的重任与期望

为什么名字叫做Garbage First （G1）呢？

因为G1是一个并行回收器，它把堆内存分割为很多不相关的区域
（Region） （物理上不连续的）。使用不同的Region来表示Eden、
幸存者0区，幸存者1区，老年代等。
G1 GC有计划地避免在整个Java 堆中进行全区域的垃圾收集。
G1跟踪各个Region
里面的垃圾堆积的价值大小（回收所获得的空间大小以及回收所需
时间的经验值），在后台维护一个优先列表，每次根据允许的收集
时间，优先回收价值最大的Region。
由于这种方式的侧重点在于回收垃圾最大量的区间（Region），
所以我们给G1一个名字：垃圾优先（Garbage First） 。
G1 （Garbage一First） 是一款面向服务端应用的垃圾收集器，
主要针对配备多核CPU及大容量内存的机器，以极高概率满足GC停
顿时间的同时，还兼具高吞吐量的性能特征。
在JDK1. 7版本正式启用，移除了Experimental的标识，是JDK 9以
后的默认垃圾回收器，取代了CMS回收器以及Parallel + Parallel Old组
合。被Oracle官方称为“全功能的垃圾收集器” 。
与此同时，CMS已经在JDK 9中被标记为废弃（deprecated） 。
在jdk8中还不是默认的垃圾回收器，需要使用一XX： +UseG1GC来启用。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301105305830.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
优势
与其他GC收集器相比，G1使用了全新的分区算法，其特点如下所示：

并行与并发

并行性： G1在回收期间，可以有多个Gc线程同时工作，有效利用多核计
算能力。此时用户线程STW
并发性： G1拥有与应用程序交替执行的能力，部分工作可以和应用程序同
时执行，因此，一般来说，不会在整个回收阶段发生完全阻塞应用程序的
情况


分代收集

从分代上看，G1依然属于分代型垃圾回收器，它会区分年轻代和老年代，
年轻代依然有Eden区和Survivor区。但从堆的结构，上看，它不要求整
个Eden区、年轻代或者老年代都是连续的，也不再坚持固定大小和固
定数量。
将堆空间分为若干个区域（Region） ，这些区域中包含了逻辑上的
年轻代和老年代。
和之前的各类回收器不同，它同时兼顾年轻代和老年代。对比其他回
收器，或者工作在年轻代，或者工作在老年代；
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301122817512.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
空间整合

CMS： “标记一清除”算法、内存碎片、若干次Gc后进行一次碎片整理
G1将内存划分为一个个的region。 内存的回收是以region作为基
本单位的.Region之间是复制算法，但整体上实际可看作是标记一压缩
（Mark一Compact）算法，两种算法都可以避免内存碎片。这种特性有
利于程序长时间运行，分配大对象时不会因为无法找到连续内存空间
而提前触发下一次GC。尤其是当Java堆非常大的时候，G1的优势更加
明显。


可预测的停顿时间模型（即：软实时soft real一time）
这是G1相对于CMS的另一大优势，G1除了追求低停顿外，还能建立
可预测的停顿时间模型，能让使用者明确指定在一个长度为M毫秒的时
间片段
内，消耗在垃圾收集上的时间不得超过N毫秒。

由于分区的原因，G1可以只选取部分区域进行内存回收，这样缩小了
回收的范围，因此对于全局停顿情况的发生也能得到较好的控制。
G1跟踪各个Region里面的垃圾堆积的价值大小（回收所获得的空间大
小以
及回收所需时间的经验值），在后台维护一个优先列表，每次根
据允许的
收集时间，优先回收价值最大的Region。保证了G1 收集器在有
限的时间
内可以获取尽可能高的收集效率。
相比于CMSGC，G1未必能做到CMS在最好情况下的延时停顿，但是
最差情况要.好很多。



缺点

相较于CMS，G1还不具备全方位、压倒性优势。比如在用户程序运
行过程中，G1无论是为了垃圾收集产生的内存占用（Footprint） 
还是程序运行时的额外执行负载（overload） 都要比CMS要高。
从经验上来说，在小内存应用上CMS的表现大概率会优于G1，而G1在
大内存应用，上则发挥其优势。平衡点在6一8GB之间。

参数设置

-XX：+UseG1GC 手动指定使用G1收集器执行内存回收任务。
-XX：G1HeapRegionSize 设置每个Region的大小。值是2的幂，
范围是1MB
到32MB之间，目标是根据最小的Java堆大小划分出约2048个区域。
默认是堆内存的1/2000。
-XX：MaxGCPauseMillis 设置期望达到的最大Gc停顿时间指标
（JVM会尽力实现，但不保证达到）。默认值是200ms
-XX：ParallelGCThread 设置sTw.工作线程数的值。最多设置为8
-XX：ConcGCThreads 设置并发标记的线程数。将n设置为并行垃圾
回收线程数（ParallelGCThreads）的1/4左右。
-XX：Ini tiatingHeapOccupancyPercent 设置触发并发GC周
期的Java堆占用率阈值。超过此值，就触发GC。默认值是45。
```
#### 1.13.1 G1回收器的常见操作步骤
```markdown
G1的设计原则就是简化JVM性能调优，开发人员只需要简单的三步
即可完成调优：

第一步：开启G1垃圾收集器
第二步：设置堆的最大内存
第三步：设置最大的停顿时间

G1中提供了三种垃圾回收模式： YoungGC、 Mixed GC和Full GC， 
在不同的条件下被触发。
适用场景

面向服务端应用，针对具有大内存、多处理器的机器。（在普通大小
的堆里表现并不.惊喜）
最主要的应用是需要低GC延迟，并具有大堆的应用程序提供解决方案；
如：在堆大小约6GB或更大时，可预测的暂停时间可以低于0.5秒； 
（ G1通过每次只清理一部分而不是全部的Region的增量式清理来保
证每次GC停顿时间不会过长）。
用来替换掉JDK1.5中的CMS收集器；
在下面的情况时，使用G1可能比CMS好：
1 超过50%的Java堆被活动数据占用；
2 对象分配频率或年代提升频率变化很大；
3 GC停顿时间过长（长于0. 5至1秒）。
HotSpot垃圾收集器里，除了G1以外，其他的垃圾收集器使用内
置的JVM
线程执行
GC的多线程操作，而G1 GC可以采用应用线程承担后台运行的GC工作，
即当JVM的GC线程处理速度慢时，系统会调用应用程序线程帮助加速
垃圾回收过程。

分区region,化整为零
  使用G1收集器时，它将整个Java堆划分成约2048个大小相同的独
立Region块，每个Region块大小根据堆空间的实际大小而定，整体
被控制在1MB到32MB之间，且为2的N次幂，
即1MB， 2MB， 4MB， 8MB， 1 6MB， 32MB。可以通过一
XX：G1HeapRegionSize设定。所有的Region大小相同，且在JVM生
命周期内不会被改变。
  虽然还保留有新生代和老年代的概念，但新生代和老年代不再是物理
隔离的了，它们都是一部分Region （不需要连续）的集合。通
过Region的动态分配方式实现逻辑_上的连续。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301141646708.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
一个region 有可能属于Eden， Survivor 或者0ld/Tenured 内
存区域。但是一个region只可能属于一个角色。图中的E表示该
region属于Eden内存区域，s表示属于Survivor内存区域，0表
示属于0ld内存区域。图中空白的表示未使用的内存空间。
G1垃圾收集器还增加了一种新的内存区域，叫做Humongous内
存区域，如图中的H块。主要用于存储大对象，如果超过1.5个region，
就放到H。

设置H的原因：
对于堆中的大对象，默认直接会被分配到老年代，但是如果它是一个短
期存在的大对象，就会对垃圾收集器造成负面影响。为了解决这个问题，
G1划分了一个Humongous区，它用来专门存放大对象。如果一个H
区装不下一个大对象，那么G1会寻找连续的H区来存储。为了能找到
连续的H区，有时候不得不启动Full GC。G1的大多数行为都把H区作
为老年代的一部分来看待。
```
#### 1.13.2 G1回收器垃圾回收过程
```markdown
G1 GC的垃圾回收过程主要包括如下三个环节：

年轻代GC （Young GC ）
老年代并发标记过程（ Concurrent Marking）
混合回收（Mixed GC ）
（如果需要，单线程、独占式、高强度的Full GC还是继续存在
的。它针对GC的评估失败提供了一种失败保护机制，即强力回收。）
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301164951272.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
顺时针， young gc 一> young gc + concurrent mark 一> Mixed GC顺序，
进行垃圾回收。
应用程序分配内存，当年轻代的Eden区用尽时开始年轻代回收过程；
G1的年轻代收集阶段是一个并行的独占式收集器。在年轻代回收
期，G1 GC暂停所有应用程序线程，启动多线程执行年轻代回
收。然后从年轻代区间移动存活对象到Survivor区间或者老年区
间，也有可能是两个区间都会涉及。
当堆内存使用达到一定值（默认45%）时，开始老年代并发
标记过程。
标记完成马.上开始混合回收过程。对于一个混合回收期，G1 GC
从老年区间移动存活对象到空闲区间，这些空闲区间也就成为
了老年代的一部分。和年轻代不同，老年代的G1回收器和其他GC
不同，G1的老年代回收器不需要整个老年代被回收，一次只需
要扫描/回收一小部分老年代的Region就可以了。同时，这个老
年代Region是和年轻代一起 被回收的。
举个例子：一个web服务器，Java进程最大堆内存为4G，每分钟
响应1500个请求，每45秒钟会新分配大约2G的内存。G1会每4
5秒钟进行一次年轻代回收，每31 个小时整个堆的使用率会达
到45号，会开始老年代并发标记过程，标记完成后开始四到五
次的混合回收。
```
#### 1.13.3 记忆集与写屏障
```markdown
一个对象被不同区域引用的问题(分代引用问题)
一个Region不可能是孤立的，一个Region中的对象可能被其他任
意Region中对象引用，判断对象存活时，是否需要扫描整个Java堆
才能保证准确？
在其他的分代收集器，也存在这样的问题（ 而G1更突出）
回收新生代也不得不同时扫描老年代？
这样的话会降低MinorGC的效率；
解决方法：

无论G1还是其他分代收集器，JVM都是使用RememberedSet
来避免全局扫描：
每个Region都有 一个对应的Remembered Set；
每次Reference类 型数据写操作时，都会产生一个Write Barrier
暂时中断操作； .
然后检查将要写入的引用指向的对象是否和该Reference类型
数据在不同的Region （其他收集器：检查老年代对象是否引用
了新生代对象） ；
如果不同，通过CardTable把相关引用信息记录到引用指向对
象的所在Region对应的Remembered Set中；
当进行垃圾收集时，在GC根节点的枚举范围加入Remembered Set；
就可以保证不进行全局扫描，也不会有遗漏
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301165239704.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 1.13.4 G1回收过程详解
```markdown
1  年轻代GC
JVM启动时，G1 先准备好Eden区，程序在运行过程中不断创建
对象到Eden区，当Eden空间耗尽时，G1会启动一次年轻代垃
圾回收过程。
年轻代垃圾回收只会回收Eden区和Survivor区。
YGC时，首先G1停止应用程序的执行（Stop一The一World），
G1创建回收集（Collection Set），回收集是指需要被回收的内存
分段的集合，年轻代回收过程的回收集包含年轻代Eden区和
Survivor区所有的内存分段。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301165333156.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
然后开始如下回收过程：

第一阶段，扫描根。
根是指static变量指向的对象，正在执行的方法调用链条上的局
部变量等。根引用连同RSet记录的外部引用作为扫描存活对象的入口。
第二阶段，更新RSet.
处理dirty card queue（ 见备注）中的card，更新RSet。 
此阶段完成后，RSet可 以准确的反映老年代对所在的内存分段中
对象的引用。

dirty card queue: 对于应用程序的引用赋值语句
object.field=object，JVM会在之前和之后执行特殊的操作
以在dirty card queue中入队一个保存了对象引用信息的card。
在年轻代回收的时候，
G1会对Dirty Card
Queue中所有的card进行处理，以更新RSet，保证RSet实时准确
的反映引用关系。
那为什么不在引用赋值语句处直接更新RSet呢？这是为了性能的需
要，RSet的处理需要线程同步，开销会很大，使用队列性能会好很多。


第三阶段，处理RSet。
识别被老年代对象指向的Eden中的对象，这些被指向的Eden中
的对象被认为是存活的对象。
第四阶段，复制对象。
此阶段，对象树被遍历，Eden区 内存段中存活的对象
会被复制到Survivor区中空的内存分段，Survivor区内存段
中存活的对象如果年龄未达阈值，年龄会加1，达到阀值会被
会被复制到01d区中空的内存分段。如果Survivor空间不够，
Eden空间的 部分数据会直接晋升到老年代空间。
第五阶段，处理引用。
处理Soft，Weak， Phantom， Final， JNI Weak等引用。
最终Eden空间的数据为空，GC停止工作，而目标内存中的对
象都是连续存储的，没有碎片，所以复制过程可以达到内存整理
的效果，减少碎片。


2 并发标记过程

初始标记阶段：标记从根节点直接可达的对象。这个阶段是STW的，
并且会触发一.次年轻代GC。
根区域扫描（Root Region Scanning） ： G1 GC扫描
Survivor区 直接可达的老年代区域对象，并标记被引用的对象。
这一过程必 须在young GC之前完成。
并发标记（Concurrent Marking）： 在整个堆中进行并发标记
（和应用程序并发执行），此过程可能被young GC中断。在
并发标记阶段，若发现区域对象中的所有对象都是垃圾，那这个
区域会被立即回收。同时，并发标记过程中，会计算每个区域的
对象活性（区域中存活对象的比例）。
再次标记（Remark）： 由 于应用程序持续进行，需要修正
上一次的标记结果。是STW的。G1中采用了比CMS更快的初
始快照算法：snapshot一at一the一beginning （SATB）。
独占清理（cleanup，STW）：计算各个区域的存活对象和GC回
收比例，并进行排序，识别可以混合回收的区域。为下阶段做
铺垫。是STW的。

这个阶段并不会实际上去做垃圾的收集


并发清理阶段：识别并清理完全空闲的区域。

3 混合回收
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301165521443.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
当越来越多的对象晋升到老年代oldregion时，为了避免堆内存被耗尽，
虚拟机会触发一个混合的垃圾收集器，即Mixed GC， 该算法并不是
一个oldGC，除了回收整个Young Region，还会回收一部分的OldRegion。
这里需要注意：是一部分老年代， 而不是全部老年代。可以选择哪些
OldRegion进行收集，从而可以对垃圾回收的耗时时间进行控制。也要
注意的是Mixed GC并不是Full GC。

并发标记结束以后，老年代中百分百为垃圾的内存分段被回收了，
部分为垃圾的内存分段被计算了出来。默认情况下，这些老年代
的内存分段会分8次（可以通-XX： G1MixedGCCountTarget设置）
被回收。
混合回收的回收集（Collection Set） 包括八分之一的老年代
内存分段，Eden区内存分段，Survivor区内存分段。混合回收的算
法和年轻代回收的算法完全一样，只是回收集多了老年代的内存分段。
具体过程请参考上面的年轻代回收过程。
由于老年代中的内存分段默认分8次回收，G1会优先回收垃圾多的
内存分段。垃圾占内存分段比例越高的，越会被先回收。并且有一个
阈值会决定内存分段是否被回收，
-XX： G1MixedGCLiveThresholdPercent，
默认为65%，意思是垃圾占内存分段比例要达到65%才会被回收。如果
垃圾占比太低，意味着存活的对象占比高，在复制的时候会花费更多
的时间。
混合回收并不一定要进行8次。有一个阈值-XX:G1HeapWastePercent，
默认值为10%，意思是允许整个堆内存中有10%的空间被浪费，意味着
如果发现可以回收的垃圾占堆内存的比例低于10%，则不再进行混合回
收。因为GC会花费很多的时间但是回收到的内存却很少。

4 Full GC
  G1的初衷就是要避免Full GC的出现。但是如果上述方式不能正常工
作，G1会停止应用程序的执行（Stop一 The一World），使用单线程的内
存回收算法进行垃圾回收，性能会非常差，应用程序停顿时间会很长。
  要避免Full GC的发生，一旦发生需要进行调整。什么时候会发生
Full GC呢？比如堆内存太小，当G1在复制存活对象的时候没有空
的内存分段可用，则会回退到full gc， 这种情况可以通过增大内存
解决。
  导致G1Full GC的原因可能有两个：

1 Evacuation的时候没有足够的to一 space来存放晋升的对象；
2 并发处理过程完成之前空间耗尽。

补充
从Oracle官方透露出来的信息可获知，回收阶段（Evacuation）其实
本也有想过设计成与用户程序一起并发执行，但这件事情做起来比较复
杂，考虑到G1只是回收一部分Region， 停顿时间是用户可控制的，
所以并不迫切去实现，而选择把这个特性放到了G1之后出现的低延
迟垃圾收集器（即ZGC）中。另外，还考虑到G1不是仅仅面向低延
迟，停顿用户线程能够最大幅度提高垃圾收集效率，为了保证吞
吐量所以才选择了完全暂停用户线程的实现方案。
优化建议

年轻代大小

避免使用一Xmn或一XX：NewRatio等相关选项显式设置年轻代大小
固定年轻代的大小会覆盖暂停时间目标


暂停时间目标不要太过严苛

G1 GC的吞吐量目标是90%的应用程序时间和10%的垃圾回收时间
评估G1 GC的吞吐量时，暂停时间目标不要太严苛。目标太过严苛表
示你愿意承受更多的垃圾回收开销，而这些会直接影响到吞吐量。
```
### 1.14 垃圾回收器总结
```markdown
截止JDK 1.8，一共有7款不同的垃圾收集器。每一款不同的垃圾
收集器都有不同的特点，在具体使用的时候，需要根据具体的情
况选用不同的垃圾收集器。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021030116584038.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
不同厂商、不同版本的虚拟机实现差别很大。HotSpot 虚拟
机在JDK7/8后所有收集器及组合（连线），如下图：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301165913609.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
1 两个收集器间有连线，表明它们可以搭配使用：
Serial/Serial 0ld、Serial /CMS、ParNew/Serial 0ld、ParNew/CMS、
Parallel Scavenge/Serial 01d、Parallel Scavenge/Parallel 0ld、G1；
2 其中Serial 0ld作 为CMS出现"Concurrent Mode Failure"失败 的后备预案。
3 (红色虚线)由于维护和兼容性测试的成本，在JDK 8时将Serial+CMS、
ParNew+Serial 0ld这两个组合声明为Deprecated （JEP 173），并
在JDK 9中完全取消了这些组合的支持（JEP214），即：移除。
4 (绿色虚线)JDK 14中：弃用ParallelScavenge 
和Serial0ld GC组合（JEP 366）
5 (青色虚线)JDK 14中：删除CMS垃圾回收器 （JEP 363 ）
GC发展阶段：
Serial => Parallel （并行） => CMS （并发） => G1 => ZGC
```
```markdown
怎么选择垃圾回收器

Java垃圾收集器的配置对于JVM优化来说是一个很重要的选择，
选择合适的垃圾收集器可以让JVM的性能有一个很大的提升。
怎么选择垃圾收集器？

1 优先调整堆的大小让JVM自适应完成。
2 如果内存小于100M，使用串行收集器
3 如果是单核、单机程序，并且没有停顿时间的要求，串行收集器
4 如果是多CPU、需要高吞吐量、允许停顿时间超过1秒，选择并行
或者JVM自己选择
5 如果是多CPU、追求低停顿时间，需快速响应（比如延迟不能
超过1秒，如互联网应用），使用并发收集器
官方推荐G1，性能高。现在互联网的项目，基本都是使用G1。


最后需要明确一一个观点：

1.没有最好的收集器，更没有万能的收集；
2.调优永远是针对特定场景、特定需求，不存在一劳永逸的收集器
```
### 1.15 GC日志分析
```markdown
通过阅读GC日志，我们可以了解Java虛拟机内存分配与回收策略。
内存分配与垃圾回收的参数列表

-XX： +PrintGC 输出Gc日志。类似： 一verbose：gc
-XX： +PrintGCDetails 输出GC的详细日志
-XX： +PrintGCTimeStamps 输出GC的时间戳（以基准时间的形式）
-XX： +PrintGCDateStamps输出GC的时间戳（以日期的形式，如2013一05一04T21 ： 53：59.234+0800 ）
-XX： +PrintHeapAtGC 在进行GC的前后打印出堆的信息
-Xloggc：. . /logs/gc. log日志文件的输出路径
```
#### 1.15.1 +PrintGC
```markdown
打开GC日志：-verbose：gc
这个只会显示总的GC堆的变化， 如下：
[GC （Allocation Failure） 80832K一>19298K（227840K），0.0084018 secs]
[GC （Metadata GC Threshold） 109499K一>21465K （228352K），0.0184066 secs]
[Full GC （Metadata GC Threshold） 21 465K一>16716K （201728K），0.0619261 secs ]

参数解析：

GC、Full GC： GC的类型，GC只在新生代上进行，Full GC包括永
生代，新生代， 老年代。
Allocation Failure： GC发生的原因。
80832K一> 19298K：堆在GC前的大小和GC后的大小。
228840k：现在的堆大小。
0.0084018 secs： GC持续的时间。

```
#### 1.15.2 PrintGCDetails
```markdown
-打开GC日志： -verbose：gc一 XX：+PrintGCDetaiis

输入信息如下：
[GC （Allocation Failure） [ PSYoungGen： 70640K一> 10116K（141312K） ] 80541K一>20017K （227328K），0.0172573 secs] [Times： user=0.03 sys=0.00， real=0.02 secs ]
[GC （Metadata GC Threshold） [PSYoungGen：98859K一>8154K（142336K） ] 108760K一>21261K （228352K），
0.0151573 secs] [Times： user=0.00 sys=0.01， real=0.02 secs]
[Full GC （Metadata GC Threshold） [PSYoungGen： 8154K一>0K（142336K） ] [ParOldGen： 13107K一>16809K（62464K） ] 21261K一>16809K （204800K），[Metaspace： 20599K一>20599K （1067008K） ]，0.0639732 secs]
[Times： user=0.14 sys=0.00， real=0.06 secs]

参数解析：
GC，Full FC：同样是GC的类型
Allocation Failure： GC原因
PSYoungGen：使用了Parallel Scavenge并行垃圾收集器的新生
代GC前后大小的变化
ParOldGen：使用了Parallel Old并行垃圾收集器的老年代Gc前后大
小的变化
Metaspace： 元数据区GC前后大小的变化，JDK1.8中引入了 
元数据区以替代永久代
xxx secs ： 指Gc花费的时间
Times： user： 指的是垃圾收集器花费的所有CPU时间，
sys： 花费在等待系统调用或系统事件的时间， 
real ：GC从开始到结束的时间，包括其他进程占用时间片的实际时间。

```
#### 1.15.3 PrintGCTimeStamps
```markdown
打开GC日志：
 -verbose：gc -XX： +PrintGCDetails -XX：+PrintGCTimeStamps  -XX： +PrintGCDateStamps

输入信息如下：
2019一09一24T22：15：24.518+0800：3.287： [GC（Allocation Failure） [ PSYoungGen： 1361 62K一>5113K（136192K） ] 141425K一>17632K （222208K） ，0.0248249 secs] [Times： user=0.05sys=0.00， real=0.03 secs ]
2019一09一24T22：15：25.559+0800：4.329： [ GC（Metadata GC Threshold）[PSYoungGen：97578K一>10068K（274944K） ] 110096K一>22658K （360960K），0.0094071 secs]
[Times： user=0. 00sys=0.00， real=0. 01 secs]
2019一09一24T22：15：25.569+0800：4.338： [Full GC （Metadata GC Threshold）[ PSYoungGen：10068K一>0K（274944K） ] [ ParoldGen： 12590K一>13564K （56320K） ] 22658K一>13564K （331264K） ，
[Metaspace： 20590K一>20590K（1067008K）]， 0. 0494875 secs]
[Times： user=0.17 sys=0. 02，real=0.05 secs ]     
说明：带上了日期和时间

补充说明

"[GC"和"[Full GC"说明了这次垃圾收集的停顿类型，如果有"Full"则说
明GC发生了"StopThe World"
使用Serial收集器在新生代的名字是De fault New Generation， 
因此显示的是" [DefNew"
使用ParNew收集器在新生代的名字会变成" 【ParNew"，意思
是"Parallel New Generation"
使用Parallel Scavenge收 集器在新生代的名字是" 
【PSYoungGen"老年代的收集和新生代道理一样，名字也是收集器决
定的使用G1收集器的话，会显示为"garbage一 first heap"
Allocation Failure
表明本次引起GC的原因是因为在年轻代中没有足够的空间能够存储新的
数据了。
[PSYoungGen： 5986K一>696K（8704K）] 5986K一> 704K （9216K）
中括号内： GC回收前年轻代大小，回收后大小，
（ 年轻代总大小）
括号外： GC回收前年轻代和老年代大小，回收后大小，
（ 年轻代和老年代总大小）
user代表用户态回收耗时，sys 内核态回收耗时， 
rea实际耗时。由于多核的原因，时间总和可能会超过real时间
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301184429933.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301184440487.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301184446543.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
/**
 * 在jdk7 和 jdk8中分别执行
 * -verbose:gc -Xms20M -Xmx20M -Xmn10M -XX:+PrintGCDetails -XX:SurvivorRatio=8 -XX:+UseSerialGC
 */
public class GCLogTest1 {
    private static final int _1MB = 1024 * 1024;

    public static void testAllocation() {
        byte[] allocation1, allocation2, allocation3, allocation4;
        allocation1 = new byte[2 * _1MB];
        allocation2 = new byte[2 * _1MB];
        allocation3 = new byte[2 * _1MB];
        allocation4 = new byte[4 * _1MB];
    }

    public static void main(String[] agrs) {
        testAllocation();
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/202103011845102.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301184518260.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 1.15.4 日志分析工具使用
```markdown
可以用一些工具去分析这些gc日志。
常用的日志分析.工具有： 
GCViewer、GCEasy、GCHisto、GCLogViewer 、Hpjmeter、
garbagecat等。
```
### 1.16 垃圾回收器的新发展
```markdown
 GC仍然处于飞速发展之中，目前的默认选项G1 GC在不断的进行改
进，很多我们原来认为的缺点，例如串行的Full GC、Card Table
扫描的低效等，都已经被大幅改进，例如，JDK 10以后，Fu1l GC
已经是并行运行，在很多场景下，其表现还略优于Parallel GC的
并行Full GC实现。
  即使是Serial GC，虽然比较古老，但是简单的设计和实现未必
就是过时的，它本身的开销，不管是GC相关数据结构的开销，还是
线程的开销，都是非常小的，所以随着云计算的兴起，在Serverless
等新的应用场景下，Serial GC找到了新的舞台。
  比较不幸的是CMS GC，    因为其算法的理论缺陷等原因，虽
然现在还有非常大的用户群体，但在JDK9中已经被标记为废弃，并在
JDK14版本中
移除。
JDK11 新特性

JEP318 :
Epsilon: A No一Op Garbage
Collector (Epsilon 垃圾回收器,"No一Op (无操作) "回收器)
http: / /openidk.java.net/ieps/318
JEP333:
ZGC: A Scalable Low一 Latency ;Garbage Collector
(Experimental) ( ZGC:可伸縮的低延退竝坂回收器，处于试验
性阶段)

Open JDK12的Shenandoah GC

现在G1回收器已成为默认回收器好几年了。
我们还看到了引入了两个新的收集器：
ZGC （ JDK11出现）和Shenandoah（Open JDK12） 。
主打特点：低停顿时间



Open JDK12 的Shenandoah GC：低停顿时间的GC （实验性）

Shenandoah，无疑是众多GC中最孤独的一个。是第一款不
由Oracle公司团队领导开发的HotSpot垃圾收集器。不可避免的受到
官方的排挤。比如号称OpenJDK和OracleJDK没有区别的Oracle公
司仍拒绝在OracleJDK12中支持Shenandoah。
Shenandoah垃圾回收器最初由RedHat进行的一项垃 圾收
集器研究项目PauselessGC的实现，旨在针对JVM上的内存回收实
现低停顿的需求。在2014年贡献给OpenJDK。
Red Hat研发Shenandoah团队对外宣称，Shenandoah垃圾回收器的
暂停时间与堆大小无关，这意味着无论将堆设置为200MB还是200GB，
99.9%的目标都可以把垃圾收集的停顿时间限制在十毫秒以内。不
过实际使用性能将取决于实际工作堆的大小和工作负载。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301184711919.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
这是RedHat在2016年发表的论文数据，测试内容是使用Es对200GB
的维基百科数据进行索引。从结果看：

停顿时间比其他几款收集器确实有了质的飞跃，但也未实现最大停
顿时间控制在十毫秒以内的目标。
而吞吐量方面出现了明显的下降，总运行时间是所有测试收集器里
最长的。


Shenandoah GC的弱项：高运行负担下的吞吐量下降。
Shenandoah GC的强项：低延迟时间。

革命性的ZGC
  ZGC与Shenandoah目标高度相似，在尽可能对吞吐量影响不大
的前提下，实现在任意堆内存大小下都可以把垃圾收集的停顿时间
限制在十毫秒以内的低延迟。
  《深入理解Java虚拟机》一书中这样定义ZGC： ZGC收集器是
一款基于Region内存布局的，（暂时） 不设分代的，使用了读屏障、染
色指针和内存多重映射等技术来实现可并发的标记一压缩算法的，以低延
迟为首要目标的一款垃圾收集器。
  ZGC的工作过程可以分为4个阶段：并发标记一并发预备重分
配一并发重分配一并发重映射等。
  ZGC几乎在所有地方并发执行的，除了初始标记的是sTW的。所以停
顿时间.几乎就耗费在初始标记上，这部分的实际时间是非常少的。
测试数据如图:
```
```markdown
劣势比较
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301184836270.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
优势比较
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301184850976.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
在ZGC的强项停顿时间测试上，它毫不留情的将Parallel、G1拉开了
两个数量级的差距。无论平均停顿、958停顿、998停顿、99. 98停
顿，还是最大停顿时间，ZGC 都能毫不费劲控制在10毫秒以内。

JDK14新特性
JEP 364： ZGC应用在macOS上
JEP 365： ZGC应用在windows上
JDK14之前，ZGC仅Linux才支持

尽管许多使用ZGC的用户都使用类Linux的环境，但在Windows
和macOS 上，人们也需要ZGC进行开发部署和测试。许多桌面
应用也可以从ZGC中受益。因此，ZGC特性被移植到了Windows
和macOs.上。
现在mac或Windows 上也能使用zGC了，示例如下：
一XX： +Unloc kExperimentalVMOptions 一XX： +UseZGC 

其他垃圾回收器:AliGC
AliGC是阿里巴巴JVM团队基于G1算法，面 向大堆（LargeHeap）
应用场景。指定场景下的对比：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/202103011849460.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
## 2 Class文件结构
### 2.1 字节码文件的跨平台性
```markdown
1 Java语言：跨平台的语言( write once, run anywhere)
当]ava源代码成功编译成字节码后，如果想在不同的平台上面运行，
则无须再次编译
这个优势不再那么吸引人了。 Python、PHP、Per1、Ruby、Lisp等
有强大的解释器。
跨平台似乎己经快成为一门语言必选的特性。
2 Java虚拟机：跨语言的平台
Jav虚拟机不和包括]ava在内的任何语言绑定，它只与“Class文件
”这种特定的二进制文件格式所关联。无论使用何种语言进行软件开
发，只要能将源文件编译为正确的Class文件，那么这种语言就
可以在Java虚拟机上执行。可以说，统一而强大的Class文件结
构，就是Java虚拟机的基石、桥梁。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301194804213.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
所有的JVM全部遵守Java虚拟机规范，也就是说所有的JVM环
境都是一样的，这样一来字节码文件可以在各种JVM上运行
```
```markdown
3 想要让一个Java程序正确地运行在JVM中，Java源码就必须要被
编译为符合JVM规范的字节码。
前端编译器的主要任务就是负责将符合Java语法规范的Java代码转
换为符合JVM规范的字节码文件。
javac是一种能够将Java源码编译为字节码的前端编译器。
]javac编译器在将]ava源码编译为一个有效的字节码文件过程中
经历了4个步骤，分别是词法解析、语法解析、语义解析以及生成字节
码
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301195418192.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
Oracle的JDK软件包括两部分内容：
一部分是将Java源代码编译成Java虚拟机的指令集的编译器
另一部分是用于实现]ava虚拟机的运行时环境。
```
```markdown
Java的前端编译器
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301195753734.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
前端编译器vs后端编译器
Java源代码的编译结果是字节码，那么肯定需要有一种编译器能够
将Java源码编译为字节码，承担这个重要责任的就是配置在path环境
变量中的 javac编译器。 javac是一种能够将]ava源码编译为字节
码的前端编译器。
```
### 2.2 透过字节码看代码执行细节举例1
```java
package com.yxj.java;


public class IntegerTest {
    public static void main(String[] args) {

        Integer x = 5;
        int y = 5;
        System.out.println(x == y);   // true

        Integer i1 = 10;
        Integer i2 = 10;
        System.out.println(i1 == i2);//true

        Integer i3 = 128;
        Integer i4 = 128;
        System.out.println(i3 == i4);//false

    }
}

```
```markdown
通过字节码文件可以看出来
通过观察源码可以看出-128-127不会new出新对象。
```
```markdown
 0 iconst_5
 1 invokestatic #2 <java/lang/Integer.valueOf>
 4 astore_1
 5 iconst_5
 6 istore_2
 7 getstatic #3 <java/lang/System.out>
10 aload_1
11 invokevirtual #4 <java/lang/Integer.intValue>
14 iload_2
15 if_icmpne 22 (+7)
18 iconst_1
19 goto 23 (+4)
22 iconst_0
23 invokevirtual #5 <java/io/PrintStream.println>
26 bipush 10
28 invokestatic #2 <java/lang/Integer.valueOf>
31 astore_3
32 bipush 10
34 invokestatic #2 <java/lang/Integer.valueOf>
37 astore 4
39 getstatic #3 <java/lang/System.out>
42 aload_3
43 aload 4
45 if_acmpne 52 (+7)
48 iconst_1
49 goto 53 (+4)
52 iconst_0
53 invokevirtual #5 <java/io/PrintStream.println>
56 sipush 128
59 invokestatic #2 <java/lang/Integer.valueOf>
62 astore 5
64 sipush 128
67 invokestatic #2 <java/lang/Integer.valueOf>
70 astore 6
72 getstatic #3 <java/lang/System.out>
75 aload 5
77 aload 6
79 if_acmpne 86 (+7)
82 iconst_1
83 goto 87 (+4)
86 iconst_0
87 invokevirtual #5 <java/io/PrintStream.println>
90 return

```
### 2.3 透过字节码看代码执行细节举例2
```java
package com.yxj.java;

public class StringTest {
    public static void main(String[] args) {
        String str = new String("hello") + new String("world");
        String str1 = "helloworld";
        System.out.println(str == str1);   // false
        String str2 = new String("helloworld");
        System.out.println(str == str2);   // false
    }

    public void method1(){

    }

    public void method1(int num){

    }

//    public int method1(int num){
//        return 1;
//    }
}

```
```markdown
 0 new #2 <java/lang/StringBuilder>
 3 dup
 4 invokespecial #3 <java/lang/StringBuilder.<init>>
 7 new #4 <java/lang/String>
10 dup
11 ldc #5 <hello>
13 invokespecial #6 <java/lang/String.<init>>
16 invokevirtual #7 <java/lang/StringBuilder.append>
19 new #4 <java/lang/String>
22 dup
23 ldc #8 <world>
25 invokespecial #6 <java/lang/String.<init>>
28 invokevirtual #7 <java/lang/StringBuilder.append>
31 invokevirtual #9 <java/lang/StringBuilder.toString>
34 astore_1
35 ldc #10 <helloworld>
37 astore_2
38 getstatic #11 <java/lang/System.out>
41 aload_1
42 aload_2
43 if_acmpne 50 (+7)
46 iconst_1
47 goto 51 (+4)
50 iconst_0
51 invokevirtual #12 <java/io/PrintStream.println>
54 new #4 <java/lang/String>
57 dup
58 ldc #10 <helloworld>
60 invokespecial #6 <java/lang/String.<init>>
63 astore_3
64 getstatic #11 <java/lang/System.out>
67 aload_1
68 aload_3
69 if_acmpne 76 (+7)
72 iconst_1
73 goto 77 (+4)
76 iconst_0
77 invokevirtual #12 <java/io/PrintStream.println>
80 return

```
### 2.4 透过字节码看代码执行细节举例3
```java
package com.yxj.java;
/*
成员变量（非静态的）的赋值过程： ① 默认初始化 - ② 显式初始化 /代码块中初始化 - ③ 构造器中初始化 - ④ 有了对象之后，可以“对象.属性”或"对象.方法"
 的方式对成员变量进行赋值。
 */
class Father {
    int x = 10;

    public Father() {
        this.print();
        x = 20;
    }
    public void print() {
        System.out.println("Father.x = " + x);
    }
}

class Son extends Father {
    int x = 30;
//    float x = 30.1F;
    public Son() {
        this.print();
        x = 40;
    }
    public void print() {
        System.out.println("Son.x = " + x);
    }  //Son.x = 0 Son.x = 30
}

public class SonTest {
    public static void main(String[] args) {
        Father f = new Son();
        System.out.println(f.x); // 20
    }
}

```
```markdown
输出结果
Son.x = 0
Son.x = 30
20
```
```markdown
 0 aload_0
 1 invokespecial #1 <com/yxj/java/Father.<init>>
 4 aload_0
 5 bipush 30
 7 putfield #2 <com/yxj/java/Son.x>
10 aload_0
11 invokevirtual #3 <com/yxj/java/Son.print>
14 aload_0
15 bipush 40
17 putfield #2 <com/yxj/java/Son.x>
20 return

 0 getstatic #4 <java/lang/System.out>
 3 new #5 <java/lang/StringBuilder>
 6 dup
 7 invokespecial #6 <java/lang/StringBuilder.<init>>
10 ldc #7 <Son.x = >
12 invokevirtual #8 <java/lang/StringBuilder.append>
15 aload_0
16 getfield #2 <com/yxj/java/Son.x>
19 invokevirtual #9 <java/lang/StringBuilder.append>
22 invokevirtual #10 <java/lang/StringBuilder.toString>
25 invokevirtual #11 <java/io/PrintStream.println>
28 return
```
### 2.5  解读Class文件的三种方式
```markdown
虚拟机的基石：Class文件
字节码文件里是什么？
源代码经过编译器编译之后便会生成一个字节码文件，字节码是一种
二进制的类文件，它的内容是JVM的指令，而不像C、C++经由编译器
直接生成本地机器码
什么是字节码指令（byte code）？
Java虚拟机的指令由一个字节长度的，代表着某种特定操作含义的
操作码 (opcode)以及跟随其后的零至多个代表此操作所需参数的操
作数（operand）所构成
虚拟机中许多指令并不包含操作数，只有一个操作码
由于指令只有一个字节大小，所以最多只有256个，对于添加新的指
令要非常小心，超过256个可就完蛋了
如何解读供虚拟机解释执行的二进制字节码？
```
```markdown
1 使用Notepad++查看，安装HEX-Editor插件后进行查看
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301231851307.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
2 使用javap指令，JDK自带的反解析工具
3 使用IDEA插件，jclasslib或jclasslib bytecode viewer客户端工具
```
### 2.6 Class文件结构
```markdown
Class类的本质
任何一个Class文件都对应着唯一一个类或接口的定义信息，但反过来
说Class文件实际上它并不一定以磁盘文件形式存在。Class文件是
一组以字节为基础单位的二进制流，在内存中的表现形式为字节数组
Class文件格式
Class的结构不像XML等描述语言，由于它没有任何分隔符号。所以在
其中的数据项，无论是字节顺序还是数量，都是被严格限定的。哪个
字节代表什么含义，长度是多少，先后顺序如何，都不允许改变
Class文件格式采用一种类似于C语言结构体的方式进行数据存储，
这种结构中只有两种数据类型：无符号数和表
无符号数属于基本的数据类型，以u1、u2、u4、u8来分别代表1个
字节、2个字节、4个字节、8个字节的无符号数，无符号数可以用
来描述数字、索引引用、数量值或者按照UTF-8编码构成字符串值
表是由多个无符号数或者其他表作为数据项构成的复合数据类型，所
有表都习惯性地以"_info"结尾。表用于描述有层次关系的复合结构
的数据，整个Class文件本质上就是一张表。由于表没有固定长度，
所以通常会在其前面加上个数说明
```
```markdown
Class文件格式图示
```
| 类型           | 名称                | 说明                   | 长度    | 数量                  |
| -------------- | ------------------- | ---------------------- | ------- | --------------------- |
| u4             | magic               | 魔数,识别Class文件格式 | 4个字节 | 1                     |
| u2             | minor_version       | 副版本号(小版本)       | 2个字节 | 1                     |
| u2             | major_version       | 主版本号(大版本)       | 2个字节 | 1                     |
| u2             | constant_pool_count | 常量池计数器           | 2个字节 | 1                     |
| cp_info        | constant_pool       | 常量池表               | n个字节 | constant_pool_count-1 |
| u2             | access_flags        | 访问标识               | 2个字节 | 1                     |
| u2             | this_class          | 类索引                 | 2个字节 | 1                     |
| u2             | super_class         | 父类索引               | 2个字节 | 1                     |
| u2             | interfaces_count    | 接口计数器             | 2个字节 | 1                     |
| u2             | interfaces          | 接口索引集合           | 2个字节 | interfaces_count      |
| u2             | fields_count        | 字段计数器             | 2个字节 | 1                     |
| field_info     | fields              | 字段表                 | n个字节 | fields_count          |
| u2             | methods_count       | 方法计数器             | 2个字节 | 1                     |
| method_info    | methods             | 方法表                 | n个字节 | methods_count         |
| u2             | attributes_count    | 属性计数器             | 2个字节 | 1                     |
| attribute_info | attributes          | 属性表                 | n个字节 | attributes_count      |


### 2.7 Class文件结构概述
#### 2.7.1 魔数（Class文件的标志）
```markdown
Magic Number（魔数）
每个Class文件开头的4个字节的无符号整数称为魔数
（Magic Number）
它的唯一作用是确定这个文件是否为一个能被虚拟机接受的有效合法
的Class文件。即：魔数是Class文件的标识符
魔数值固定为0xCAFEBABE，不会改变
如果一个 Class 文件不以0xCAFEBABE开头，虚拟机在进行文件
校验的时候就会直接抛出以下错误：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210303075924932.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 2.7.2 Class文件版本号
```markdown
紧接着魔数的4个字节存储的是Class文件的版本号。同样也是4
个字节。第5个和第6个字节所代表的含义就是编译的副版本号
minor_version，而第7个和第8个字节就是编译的主版本号
major_version
它们共同构成了Class文件的格式版本号。譬如某个Class文件的
主版本号为M，服版本号为m，那么这个Class文件的格式版本号
就确定为M.m版本号和Java编译器的对应关系如下表：
```
| 主版本（十进制） | 副版本（十进制） | 编译器版本 |
| ---------------- | ---------------- | ---------- |
| 45               | 3                | 1.1        |
| 46               | 0                | 1.2        |
| 47               | 0                | 1.3        |
| 48               | 0                | 1.4        |
| 49               | 0                | 1.5        |
| 50               | 0                | 1.6        |
| 51               | 0                | 1.7        |
| 52               | 0                | 1.8        |
| 53               | 0                | 1.9        |
| 54               | 0                | 1.10       |
| 55               | 0                | 1.11       |

```markdown
Java的版本号是从45开始的，JDK 1.1之后的每个JDK大版本发布主
版本号向上加1
不同版本的Java编译器编译的Class文件对应的版本是不一样的。
目前，高版本的Java虚拟机可以执行由低版本编译器生成的Class
文件，但是低版本的Java虚拟机不能执行由高版本编译器生成的Class文
件。否则JVM会抛出java.lang.UnsupportedClassVersionError
异常（向下兼容）
在实际应用中，由于开发环境和生产环境的不同，可能会导致该
问题的发生。因此，需要我们在开发时，特别注意开发编译的JDK
版本和生产环境的JDK版本是否一致 
虚拟机JDK版本为1.k (k >= 2)时，对应的Class文件格式版本号
的范围为45.0 - 44 + k.0（含两端）
```
#### 2.7.3 常量池
```markdown
常量池是Class文件中内容最为丰富的区域之一。常量池对于Class文
件中的字段和方法解析也有着至关重要的作用
随着Java虚拟机的不断发展，常量池的内容也日渐丰富，可以说常量
池是整个Class文件的基石
在版本号之后，紧跟着的是常量池的数量，以及若干个常量池表项
常量池中常量的数量是不固定的，所以在常量池的入口需要放置
一项u2类型的无符号数，代表常量池容量计数值
（constant_pool_count），
与Java中语言习惯不一样的是，这个容量计数是从1而不是0开始的
通常我们写代码时都是从0开始的，但是这里的常量池却是从1开始，
因为它把第0项常量空出来了
这是为了满足后面某些指向常量池的索引值的数据在特定情况下
需要表达"不引用任何一个常量池项目"的含义，这种情况可用索
引值0来表示
例如后面的父类索引，Object类没有父类，所以它的父类索引
在常量池中就是0
常量池表是一种表结构，索引为1到（constant_pool_count - 1）


常量池中主要存放两大常量：字面量和符号引用
字面量：使用""引起来的字符串、使用final修饰的基本数据类
型变量
符号引用：类和接口的全限定名、字段的名称和描述符、方法的
名称和描述符
常量项大概有14种，结构都是标记字节（1字节） + 其他。比较常
见的有CONSTANT_utf8_info、CONSTANT_class_info、
CONSTANT_Methodref_info、CONSTANT_Fieldref_info等
```
|类型|名称|数量|
|--|--|--|
|u2(无符号数) |constant_pool_count  |	1|
| cp_info(表) |constant_pool|constant_pool_count-1|
```markdown
由上表可见，Class件使用一个前置的容量计数器
( constant_pool_ count)加若个连续的数据项( constant_pool)的
形式来描述常量池内容。我们把这一系列连续常量池数据称为常
量池集合。

常量池表顶中，用于存放编译时期生成的各种字面量和符号引用，
这部分内容将在类加载后进入方法区的运行时常量池
```
#### 2.7.4 常量池计数器
```markdown
constant_pool_ count（常量池计数器）
由于常量池的数量不固定，时长时短，所以需要放置两个字节来表
示常量池容量计数值。
常量池容量计数值(u2类型)：从1开始，表示常量池中有多少
项常量。即 constant_pool_ count=1表示常量池中有个常量项
Demo的值为
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210303082056674.png)

```markdown
其值为0x0016,掐指一算，也就是22.
需要注意的是，这实际上只有21项常量。索引为范围是1-21。为什么呢？
通常我们写代码时都是从开始的，但是这里的常量池却是从1开
始，因为它把第0项常星空出来了。这是为了满足后面某些指向
常量池的素引值的数据在特定情况下需要表达"不引用任何一
个常量池项目”的含义，这种情況可用索引0来表示
```
#### 2.7.5 常量池表
```markdown
constant_pool[]（常量池）
constant_pool是一种表结构，以1~ constant_pool_ count-1为素引。
表明了后面有多少个常量项。
常量池主要存放两大类常量：字面量( Literal)和
符号引用( Symbolic References)
它包含了class文件结构及其子结构中引用的所有字符串常量、
类或接口名、字段名和其他常量。常量池中的每一项都具各相同的特
征。第1个字节作为类型标记，用于确定该项的格式，这个字
节称为 tag byte(标记字节、标签字节)。
```
| 类型                             | 标志(或标识) | 描述                   |
| -------------------------------- | ------------ | ---------------------- |
| CONSTANT_utf8_info               | 1            | UTF-8编码的字符串      |
| CONSTANT_Integer_info            | 3            | 整型字面量             |
| CONSTANT_Float_info              | 4            | 浮点型字面量           |
| CONSTANT_Long_info               | 5            | 长整型字面量           |
| CONSTANT_Double_info             | 6            | 双精度浮点型字面量     |
| CONSTANT_Class_info              | 7            | 类或接口的符号引用     |
| CONSTANT_String_info             | 8            | 字符串类型字面量       |
| CONSTANT_Fieldref_info           | 9            | 字段的符号引用         |
| CONSTANT_Methodref_info          | 10           | 类中方法的符号引用     |
| CONSTANT_InterfaceMethodref_info | 11           | 接口中方法的符号引用   |
| CONSTANT_NameAndType_info        | 12           | 字段或方法的符号引用   |
| CONSTANT_MethodHandle_info       | 15           | 表示方法句柄           |
| CONSTANT_MethodType_info         | 16           | 标志方法类型           |
| CONSTANT_InvokeDynamic_info      | 18           | 表示一个动态方法调用点 |


##### 2.7.5.1 字面量和符号引用
```markdown
在对这些常量解读前，我们需要搞清楚几个概念。
常量池主要存放两大类常量：字面量( Literal)和
符号引用( Symbolic References)。如下表：
```
|常量|具体的常量|
|--|--|
| 字面量 | 文本字符串 |
|  | 声明为final的常量值 |
| 符号引用 |类和接口的权限定名  |
|  | 字段的名称和描述符 |
|  | 方法的名称和描述符 |

```markdown
全限定名
com/yxj/test/Demo这个就是类的全限定名，仅仅是把
包名的"."替换成"/",为了使连续的多个全限定名之间不产生混淆，在
使用时最后一般会加入一个“;"表示全限定名结束。
简单名称
简单名称是指没有类型和参数修饰的方法或者字段名称，上面例
子中的类的add()方法和num字段的简单名称分别是aad和num
描述符
描述符的作用是用来描述字段的数据类型、方法的参数列
表(包括数量、类型以及顺序)和返回值。根据描述符规则，
基本数据类型(byte、char、 double、float、int、long、 short、 boolean)
以及代表无返回值的void类型都用一个大写字符来表示，而对
象类型则用字符L加对象的全限定名来表示，详见下表
```
| 标志符 | 含义                                                 |
| ------ | ---------------------------------------------------- |
| B      | 基本数据类型byte                                     |
| C      | 基本数据类型char                                     |
| D      | 基本数据类型double                                   |
| F      | 基本数据类型float                                    |
| I      | 基本数据类型int                                      |
| J      | 基本数据类型long                                     |
| S      | 基本数据类型short                                    |
| Z      | 基本数据类型boolean                                  |
| V      | 代表void类型                                         |
| L      | 对象类型，比如：Ljava/lang/Object;                 |
| [      | 数组类型，代表一维数组。比如：double[][][] is [[[D |

```markdown
用描述符来描述方法时，按照先参数列表，后返回值的顺序描述，参
数列表按照参数的严格顺序放在一组小括号"()"之内。如方法
java.lang String tostring()的描述符为()Ljava/lang/String;,方法 int abc(int [] x,int y)的描述符为([II)I。

补充说明：
虚拟机在加载class文件时オ会进行链接，也就是说，Class文件中不
会保存各个方法和字段的最终内存布局信息，因此，这些字段和
方法的符号引用不经过转换是无法直接被虚拟机使用的。当虚拟机运
行时，需要从常量池中获得对应的符号引用，再在类加载过程
中的解析阶段将其替换为直接引用，并翻译到具体的内存地址中
这里说明下符号引用和直接引用的区别与关联：
符号引用：符号引用以组符号来描述所引用的目标，符号可以
是任何形式的字面量，只要使用时能无歧义地定位到目标即可。
符号引用与虚拟机实现的内存布局无关，引用的目标并不一定
己经加载到了内存中。
直接引用：直接引用可以是直接指向目标的指针、相対偏移量或
是一个能间接定位到目标的句柄。直接引用是与虚拟机实现的
内存布局相关的，同一个符号引用在不同虚拟机实例上翻译出
来的直接引用一般不会相同。如果有了直接引用，那说明引用的
目标必定已经存在于内存之中了。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210303100534424.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 2.7.5.2 常量类型和结构
```markdown
总结1:
这14种表（或者常量项结构）的共同点是：表开始的第一位
是一个u1类型的标志位(tag),代表当前这个常量项使用的是哪种表结
构，即哪种常量类型。
在常量池列表中， CONSTANT_Utf8info常量项是一种使用改进过
的UTF-8编码格式来存储诸如文字字符串、类或者接口的全限定
名、字段或者方法的简单名称以及描述符等常量字符串信息。
这14种常量项结构还有一个特点是，其中13个常量项占用的字节固定，
只有 CONSTANT_Utf8_info占用字节不固定，其大小由length决定。
为什么呢？因为从常量池存放的内容可知，其存放的是字面量和符
号引用，最终这些内容都会是一个字符串，这些字符串的大小是
在编写程序时才确定，比如你定义一个类，类名可以取长取短，所以
在没编译前，大小不固定,编译后，通过utf-8编码，就可以知道其长度

总结2:
常量池：可以理解为class文件之中的资源仓库，它是Class文件结构
中与其他项目关联最多的数据类型（后面的很多数据类型都会指向此处），
也是占用class文件空间最大的数据项目之一
常量池中为什么要包含这些内容
Java代码在进行Javac编译的时候，并不像C和C++那样有“连接”这
一步骤，而是在虚拟机加载Class文件的时候进行动态链接。也
就是说，在Class文件中不会保存各个方法、字段的最终内存布局信
息，因此这些字段、方法的符号引用不经过运行期转换的话无法
得到真正的内存入口地址，也就无法直接被虚拟机使用。当虚拟
机运行时，需要从常量池获得对应的符号引用，再在类创建时或运
行时解析、翻译到具体的内存地址之中。关于类的创建和动态链接
的内容，在虚拟机类加载过程时再进行详细讲解
```
#### 2.7.6 访问标识
```markdown
在常量池后，紧跟着访问标记。该标记使用两个字节表示，用于
识別一些类或者接口层次的访问信息，包括：这个Class是
类还是接口：是否定义为 public类型：是否定义为 abstract类
型：如果是类的话，是否被声明为fin等。各种访问标记如下所示
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210305110719943.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
类的访问权限通常为ACC_开头的常量。
每一种类型的表示都是通过设置访问标记的32位中的特定位来实现的。
比如，若是public final的类，则该标记为
ACC_PUBLIC|ACC_FINAL
使用ACC_ SUPER可以让类更准确地定位到父类的方法 
super. method(),现代编译器都会设置并且使用这个标记。
补充说明
1 带有AC_ INTERFACE标志的class文件表示的是接口而不是类，
反之则表示的是类而不是接口。
1) 如果一个class文件被设置 ACC INTERFACE标志，那么同时也得
设置 ACC ABSTRACT标志。同时它不能再设置ACC_FINAL、
ACC_ SUPER或 ACC_ENUM标志
2)如果没有设置 ACC_INTERFACE标志，那么这个class文件可以具有
上表中除 ACC_ANNOTATION外的其他所有标志。当然，
ACC_FINAL和ACC_ABSTRACT这类互斥的标志除外。这两个标志不
得同时设置
2 ACC_ SUPER标志用于确定类或接口里面的 invokespecial指令使用
的是哪一种执行语义。针对Java虚拟机指令集的编译器都应当设置这个
标志。对于]Java SE 8及后续版本来说，无论class文件中这个标
志的实际值是什么，也不管class文件版本号是多少，Java虚拟机都
认为每个class文件均设置了 ACC_SUPER标志。

1) ACC_SUPER标志是为了向后兼容由旧Java编译器所编译的代码
而设计的。目前的 ACC_SUPER标志在由JDK1.0.2之前的编译器所生
成的 access_ flags中是没有确定含义的，如果设置了该标志，那
么 Oracle的Java虚拟机实现会将其忽略。
3 ACC_ SYNTHETIC标志意味着该类或接口是由编译器生成的，而
不是由源代码生成的
4 注解类型必须设置 ACC_ANNOTATION标志。如果设置了
ACC_ ANNOTATION标志，那么也必须设置ACC_INTERFACE标志。
5 ACC_ENUM标志表明该类或其父类为枚举类型。
6 表中没有使用的 access_ flags标志是为未来扩充而预留的，
这些预留的标志在编译器中应该设置为0,Java虚拟机实现也

应该忽略它们。
```
#### 2.7.7 类索引（this_class）、父类索引（super_class）、接口索引集合（interfaces_count、interfaces[]）
```markdown
类索引、父类索引、接口索引集合
在访问标记后，会指定该类的类別、父类类别以及实现的接口，
格式如下：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021030511213278.png)

```markdown
类索引（this_class）
类索引用于确定这个类的全限定名，2字节无符号整数，指向常量池
的索引
常量池在这个索引处的成员必须为CONSTANT_Class_info类型结
构体，该结构体表示这个Class文件所定义的类或接口

父类索引（super_class）
2字节无符号整数，指向常量池的索引。它提供了当前类的父类的
全限定名
如果我们没有继承任何类，其默认继承的是java/lang/Object 类。
同时，由 于Java不支持多继承，所以其父类只有一个

接口索引集合（interfaces_count、interfaces[]）
interfaces_count项的值表示当前类或接口的直接超接口数量
interfaces[]中每个成员的值必须是对常量池表中某项的有效索引
值，它的长 度为interfaces_count
每个成员interfaces[i]必须为CONSTANT_Class_info结构，
其中0 <= i < interfaces_count
在interfaces[]中，各成员所表示的接口顺序和对应的源代码中
给定的接口顺序（从左至右）一样，即 interfaces[0]对应的
是源代码中最左边的接口
```
#### 2.7.8 字段表集合
```markdown
fields
用于描述接口或类中声明的变量。字段(field)包括类级变量以及
实例级变量，但是不包括方法内部、代码块内部声明的局部变量。

字段叫什么名字、字段被定义为什么数据类型，这些都是无法
固定的，只能引用常量池中的常量来描述。
它指向常量池索引集合，它描述了每个字段的完整信息。
比如字段的标识符、访问修饰符（public、 private或
protected)、是类变量还是实例变量( static修饰符)、是
否是常量(final修饰符)等。

注意事项：
字段表集合中不会列出从父类或者实现的接口中继承而来的字段，
但有可能列出原本Java代码之中不存在的字段。比如在内部
类中为了保持对外部类的访问性，会自动添加指向外部类实例的字段。
在Java语言中字段是无法重载的，两个字段的数据类型、修饰
符不管是否相同，都必须使用不一样的名称，但是对于字节码来
讲，如果两个字段的描述符不一致，那字段重名就是合法的。
```
##### 2.7.8.1  字段计数器(fields_count)
```markdown
fields_count的值表示当前class文件fields表的成员个数。使用
两个字节来表示。
fields表中每个成员都是一个 field_info结构，用于表示该类或接
口所声明的所有类字段或者实例字段，不包括方法内部声明的变量，
也不包括从父类或父接口继承的那些字段。
```
##### 2.7.8.2 fields[]（字段表）
```markdown
fields表中的每个成员都必须是一个 fields_infd结构的数据项，
用手表示当前类或接口中某个字段的完整描述
一个字段的信息包括如下这些信息。这些信息中，各个修饰
符都是布尔值，要么有，要么没有

作用域( public、 private、 protected修饰符)
是实例变量还是类变量( static修饰符)
可变性( final)
并发可见性（volatile修饰符，是否强制从主内存读写
可否序列化( transient修饰符)
字段数据类型(基本数据类型、对象、数组)
字段名称
字段表结构
字段表作为一个表，同样有他自己的结构
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210305113800195.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
###### 2.7.8.2.1 字段访问标志（access_flags）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210305114001875.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
###### 2.7.8.2.2 字段名索引（name_index）
```markdown
根据字段名索引的值，查询常量池中的指定索引项即可
```
###### 2.7.8.2.3 描述符索引字段的数据类型（基本数据类型、引用数据类型和数组）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210305114138859.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
###### 2.7.8.2.4 属性表集合（属性个数和属性表数组）
```markdown
一个字段还可能拥有一些属性，用于存储更多的额外信息。比如
初始化值、 一些注释信息等
属性个数存放在attribute_count中
属性具体内容存放在attributes数组中
以常量属性为例，结构为：
常量属性结构
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210305114332450.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210305114341179.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 2.7.9 方法表集合
```markdown
分为方法表计数器以及方法表数组
方法表计数器：有多少个方法表
方法表：用于表示当前类或接口中某个方法的完整描述
```
```markdown
方法表结构图
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210305114531590.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
访问标志
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021030511454442.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
方法名索引：对应常量池中CONSTANT_Uft8_info方法的名称
描述符索引：对应常量池中符号引用方法的返回值和参数列表
属性计数器：有多少个属性
属性表：和前面字段表中的属性表类似，后面详细解释
```
#### 2.7.10 属性表集合
```markdown
属性计数器：有多少个属性表
属性表
属性表通用格式
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210305114650775.png)

```markdown
属性名索引：在常量池中的索引，其实引用的字符串常量
属性长度：有多少个字节，便于校验
```

```markdown
Java8中定义了23中属性表
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210305114725274.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 2.7.11 常见的Code属性表
```markdown
Code属性就是存放方法体里面的代码，但是并非所有方法表都
有Code属性，像接口或者抽象方法，他们没有具体的方法体，
因此也就不会有Code属性了
```
```markdown
Code属性表结构
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210305115711105.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 2.7.12 常见的LineNumberTable属性表
```markdown
LineNumberTable属性是可选变长属性，位于Code结构的属性表
LineNumberTable属性是用来描述Java源码行号与字节码行号之间
的对应关系，这个属性可以用来在调试的时候定位代码执行的行数
```
```markdown
LineNumberTable属性表结构
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210305115757193.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 2.7.13 常见的LocalVariableTable属性表
```markdown
LocalVariableTable是可选变长属性，位于Code属性的属性表中。
它被调试器用于确定方法在执行过程中局部变量的信息
```
```markdown
LocalVariableTable属性表结构
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210305115903744.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
attribute_name_index：属性表名称常量池表索引
attribute_length：属性表长度
local_variable_table_length：局部变量个数
start_pc + length：这个变量在方法内的作用于
name_index：变量名在常量池表的索引
descriptor_index：局部变量数据类型在常量池表的索引
index：变量在局部变量表中的槽位
```
#### 2.7.14 总结
```markdown
Class文件其实就是对类的整体描述
类有哪些字段？有哪些方法？类的全限定名？类的父类是谁？
类实现的接口有哪些？方法中具体的方法体是什么？类的访问权
限和修饰符等
Class文件中有一块非常重要的内容就是常量池，通过上面的分析可
以得知，常量池中存储的符号引用，其他描述的地方引用常量池中
的符号引用，最后都定位到字面量（字符串、基本数据类型）

小结
随着Java平台的不断发展，在将来Class文件的内容也一定会做进
一步的扩充，但是其基本的格式和结构不会做重大调整
从Java虚拟机的角度看，通过Class文件，可以让更多的计算机语言
支持Java虚拟机平台
因此，Class文件结构不仅仅是Java虚拟机的执行入口，更是Java
生态圈的基础和核心
```
### 2.8 javap主要参数的使用
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210305121851799.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
## 3 字节码指令集
```markdown
Java字节码对于虚拟机，就好像汇编语言对于计算机，属于基本执行
命令
Java虚拟机的指令由一个字节长度的，代表着某种特定操作含义的
数字（称为操作码：Opcode）以及跟随其后的零至多个代表此操作
所需参数（称为操作数：Operands）构成
由于Java虚拟机采用面向操作数栈而不是寄存器的结构，所以
大多数的指令都不包含操作数，只有一个操作码
由于限制了Java虚拟机操作码的长度为一个字节（即0 ~ 255），
这意味着指令集的操作码总数不可能超过256条 
```
```markdown
执行模型
如果不考虑异常处理的话，那么Java虚拟机的解释器可以
使用下面这个伪代码当做最基本的执行模型来理解
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210305122709968.png)
### 3.1 字节码与数据类型
```markdown
在Java虚拟机的指令集中，大多数的指令都包含了其操作所对应的
数据类型信息
例如，iload指令用于从局部变量表中加载int类型的数据到操作
数栈中，而fload指令加载的则是float类型的数据
对于大部分与数据类型相关的字节码指令，它们的操作码助记符中
都有特殊的字符来表明专门为哪种数据类型服务：

大多数对于boolean、byte、short和char类型数据的操作，实际
上都是使用相应的int类型作为运算类型
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210305122854514.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 3.2 指令的分类
```markdown
加载与存储指令
算术指令
类型转换指令
方法调用与返回指令
操作数栈管理指令
控制转移指令
异常处理指令
同步控制指令
```
#### 3.2.1 加载与存储指令
```markdown
作用
加载和存储指令用于将数据从栈帧的局部变量表和操作数栈之间来
回传递
```
```markdown
常用指令
1[局部变量压栈指令]将一个局部变量加载到操作数栈：
xload、xload_<n>(其中x为i、l、f、d
a,n0为到3)
2[常量入栈指令] 将一个常量加载到操作数栈： 
bipush、 sipush、ldc、ldc_w、ldc2_w、
aconst_null、 iconst_m1、 iconst_<i>、lconst<l>、
fconst_<f>、 dconst<d>
3[出栈装入局部变量表指令]将一个数值从操作数栈存储到局部变量表： 
xstore、 xstore_<n>(其中x为i、l、f、d、a,n为0到3); 
xastore(其中x为、l、f、d、a、b、c、s)
4 扩充局部变量表的访问索引的指令：wide
上面所列举的指令助记符中，有一部分是以尖括号结尾的
(例如iload_<n>)。这些指令助记符实际上代表了一组指令
(例如iloa_<n>代表了iload_0、iload_1、iload_2和 iload_3
这几个指令)。
这几组指令都是某个带有一个操作数的通用指令(例如iload)的
特殊形
式，对于这若干组特殊指令来说，它们表面上没有操作数，不需要
进行取操作数的动作，但操作数都隐含在指令中。
比如：
iload_0:将局部变量表中索引为0位置上的数据压入操作数栈中。
除此之外，它们的语义与原生的通用指令完全一致（例如iload_0
的语义与操作数为0时的iload指令语义完全一致。在尖括号之间的
字母指定了指令隐含操作数的数据类型，<n>代表非负的整数，
くi>代表是int类型数据，<l>代表long类型，<f>代表float类型，
<d>代表 double类型。
```
##### 3.2.1.1 局部变量压栈指令
```markdown
局部变量压栈指令将给定的局部变量表中的数据压入操作数栈
这类指令大体可以分为
x1oad_<n>(x为i、l、f、d、a,n0为到3)
xload(x为i、l、f、d、a)
说明：在这里，x的取值表示数据类型。
指令xload_n表示将第n个局部变量压入操作数栈，比如iload_1、
fload_0、 aload_0等指令。其中 aload_n表示将个对象引用压栈。
指令xload通过指定参数的形式，把局部变量压入操作数栈，当使用
这个命令时，表示局部变量的数量可能超过了4个，比如指令iload、
fload等。
```
##### 3.2.1.2 常量入栈指令
```markdown
常量入栈指令的功能是将常数压入操作数栈，根据数据类型和入
栈内容的不同，又可以分为const系列、push系列和ldc指令。
指令const系列：用于对特定的常量入栈，入栈的常量隐含在指
令本身里。指令有： iconst_<i>(i从-1到5)、l
const_<l>(l从0到1)、 fconst_<f>(f从0到2)、
 dconst_<d>(d从到1)、 aconst_null。

比如，
iconst_m1将-1压入操作数栈:
iconst_x(x为0到5)将x压入栈
lconst_0、lconst_1分别将长整数0和1压入栈：
fconst_0、 fconst_1、 fconst_2分别将浮点数0、1、2压入栈
dconst_0和 dconst_1分别将double型0和1压入栈
aconst_null将null压入操作数栈
从指令的命名上不难找出规律，指令助记符的第一个字符总是
喜欢表示数据类型，i表示整数，l表示长整数，f表示浮点数，d表示
双精度浮点，习惯上用a表示对象引用。如果指令隐含操作的参数，
会以下划线形式给出。
指令push系列：主要包括 bipusha和 sipush。它们的区别在于
接收数据类型的不同， bipasha接收8位整数作为参数，sipush
接收16位整数，它们都将参数压入栈。
指令ldc系列：如果以上指令都不能满足需求，那么可以使用万能
的ldc指令，它可以接收一个8位的参数，该参数指向常量池中的
int、float或者 Stringl的索引，将指定的内容压入堆栈。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210307120839196.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

##### 3.2.1.3 出栈装入局部变量表指令
```markdown
出栈装入局部变量表指令用于将操作数栈中栈顶元素弹出后，
装入局部变量表的指定位置，用于给局部变量赋值。
这类指令主要以store的形式存在，比如
xstore(x为i、l、f、d、a)、xstore_n(x为i、l、f、d、a,n为0至3).
其中，指令istore n将从操作数栈中弹出一个整数，并把它赋值
给局部变量索引n位置。
指令xstore由于没有隐含参数信息，故需要提供一个byte类型的参数
类指定目标局部变量表的位置。
说明：
一般说来，类似像 store这样的命令需要带一个参数，用来指明
将弹出的元素放在局部变量表的第几个位置。但是，为了尽可能
压缩指令大小，使用专门的 istore_1指令表示将弹出的元素放置在
局部变量表第1个位置。类似的还有
istore_0、 istore_2、 istore_3,它们分别表示从操作数栈顶弹出
一个元素，存放在局部变量表第0、2、3个位置。

由于局部变量表前几个位置总是非常常用，因此这种做法虽
然増加了指令数量，但是可以大大压缩生成的字节码的体积。如
果局部变量表很大，需要存储的槽位大于3,那么可以使用 istore指
令，外加一个参数，用来表示需要存放的槽位位
置
```
#### 3.2.2 算数指令
```markdown
算术指令用于对两个操作数梭上的值进行某种特定运算，并把结
果重新压入操作数。

分类
大体上算术指令可以分为两种：对整型数据进行运算的指令与对浮
直类型数据进行运算的指令。

byte、 short、char和 boolean类型说明
在每一大类中，都有针对Java虚拟机具体数据类型的专用算术指令。
但没有直接支持byte、 short、char和 boolean类型的算术指令，
对于这些数据的运算，都使用int类型的指令来处理。此外，
在处理 boolean、byte、 short和char类
型的数组时，也会转换为使用对应的int类型的字节码指令来处理。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210307103322793.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
运算时的溢出
数据运算可能会导致溢出，例如两个很大的正整数相加，结果可
能是一个负数。其实Java虚拟机规范并无明确规定过整型数据
溢出的具体结果，仅规定了在处理整型数据时，只有除法指令以
及求余指令中当出现除数为0时会导致虚拟机抛出
异常 Arithmeticexception

运算模式
向最接近数舍入模式：JVM要求在进行浮点数计算时，所有的运
算结果都必须舍入到适当的精度，非精确结果必须舍入为可被
表示的最接近的精确值，如果有两种可表示的形式与该值一样
接近，将优先选择最低有效位为零的；
向零舍入模式：将浮点数转换为整数时，采用该模式，该模式
将在目标数值类型中选择一个最接近但是不大于原值的数字作
为最精确的舍入结果

NaN值使用
当一个操作产生溢出时，将会使用有符号的无穷大表示，如果
某个操作结果没有明确的数学的话，将会使用NaN值来表示。而
且所有使用NaN值作为操作数的算术操作，结果都会返回NaN

所有的算术指令包括：
加法指令：iadd ladd、fadd、dadd
减去指令：isub、lsub、fsub、dsub
乘法指令：imul、lmul、fmul、dmul
除法指令：idiv、ldiv、fdiv、ddiv
求余指令：irem、lrem、frem、drem // remainder:余数
取反指令：ineg、lneg、fneg、dneg   // negation:取反
自增指令：iinc
位运算指令，又可分为
位移指令：ishl、ishr、 iushr、lshl、lshr、lushr
按位或指令：ior、lor
按位与指令：iand、land
按位异或指令：ixor、lxor
比较指令： dcmpg、dcmpl、 fcmpg、fcmpl、lcmp
```
##### 3.2.2.1 比较指令
```markdown
比较指令的作用是比较栈顶两个元素的大小，并将比较结果入栈
比较指令有： dcmpg,dcmpl、 fcmpg、 fcmpl、lcmp
与前面讲解的指令类似，首字符d表示 double类型，f表示float,
l表示long对于double和float类型的数字，由于NaN的存在，各有
两个版本的比较指令。以 float为例，有 fcmp和fcmpl两个指令，
它们的区别在于在数字比较时，若遇到NaN值，处理结果不同。
指令dcmpl和 dcmpg也是类似的，根据其命名可以推测其含义，
在此不再赘述。
指令lcmp针对long型整数，由于ong型整数没有NaN值，
故无需准备两套指令。
举例
指令fcmpg和fcmpl都从栈中弹出两个操作数，并将它们
做比较，设栈顶的元素为v2,栈顶顺位第2位的元素为v1,若v1=v2,
则压入0：若v1>v2则压入1:若v1<v2则压入-1。
两个指令的不同之处在于，如果遇到NaN值， fcmpg会压入1,
而fcmpl会压入-1。
```

#### 3.2.3 类型转换指令
```markdown
类型转换指令可以将两种不同的数值类型进行相互转换。
这些转换操作一般用于实现用户代码中的显式类型转換操作，
或者用来处理字节码指令集中数据类型相关指令无法与
数据类型一一对应的问题。
```

##### 3.2.3.1 宽化类型转换
```markdown
宽化类型转换( Widening Numeric Conversions)
1 转换规则
Java虚拟机直接支持以下数值的宽化类型转换
（ widening numeric conversion,小范围类型向大范围类型
的安全转换）。
也就是说，并不需要指令执行，包括从int类型到long、float
或者 double类型。对应的指令为：i21、i2f、i2d
从long类型到float、 double类型。对应的指令为：i2f、i2d
从float类型到double类型。对应的指令为：f2d
简化为：int-->long-->float-> double
2 精度损失问题
2.1 宽化类型转换是不会因为超过目标类型最大值而丢失信息的，
例如，从int转换到long,或者从int转换到double,都不会丢失任
何信息，转换前后的值是精确相等的。
2.2 从int、long类型数值转换到float,或者long类型数值转换到
double时，将可能发生精度丢失一一可能丢失掉几个最低有效
位上的值，转换后的浮点数值是根据IEEE754最接近含入模式
所得到的正确整数值。

尽管宽化类型转换实际上是可能发生精度丢失的，但是这种转换永
远不会导致Java虚拟机抛出运行时异常

3 补充说明
从byte、char和 short类型到int类型的宽化类型转换实际上
是不存在的。对于byte类型转为int,拟机并没有做实质性的
转化处理，只是简单地通过操作数栈交換了两个数据。而
将byte转为long时，使用的是i2l,可以看到在内部
byte在这里已经等同于int类型处理，类似的还有 short类型，
这种处理方式有两个特点：
一方面可以减少实际的数据类型，如果为 short和byte都准备
一套指令，那么指令的数量就会大増，而虚拟机目前的设计
上，只愿意使用一个字节表示指令，因此指令总数不能超
过256个，为了节省指令资源，将 short和byte当做
int处理也在情理之中。
另一方面，由于局部变量表中的槽位固定为32位，无论是byte或
者 short存入局部变量表，都会占用32位空间。从这个角度说，
也没有必要特意区分这几种数据类型。
```
##### 3.2.3.2 窄化类型转换
```markdown
窄化类型转换( Narrowing Numeric Conversion)
1 转换规则
Java虚拟机也直接支持以下窄化类型转换：
从主int类型至byte、 short或者char类型。对应的指令有：
i2b、i2c、i2s
从long类型到int类型。对应的指令有：l2i
从float类型到int或者long类型。对应的指令有：f2i、f2l
从double类型到int、long或者float类型。对应的指令有：
d2i、d2l、d2f

2 精度损失问题
窄化类型转换可能会导致转换结果具备不同的正负号、不同的数量
级，因此，转换过程很可能会导致数值丢失精度。
尽管数据类型窄化转换可能会发生上限溢出、下限溢出和精度丢
失等情况，但是Java虚拟机规范中明确规定数值类型的窄化转换指
令永远不可能导致虚拟机抛出运行时异常

3 补充说明
3.1 当将一个浮点值窄化转换为整数类型T(T限于int或long类
型之一)的时候，将遵循以下转换规则：
如果浮点值是NaN,那转换结果就是int或long类型的0.
如果浮点值不是无穷大的话，浮点值使用IEEE754的向零含入模
式取整，获得整数值Vv如果v在目标类型T(int或long)的表示范围
之内，那转换结果就是v。否则，将根据v的符号，转换为T所
能表示的最大或者最小正数

3.2 当将一个double类型窄化转换为float类型时，将遵循
以下转换规则
通过向最接近数舍入模式舍入一个可以使用float类型表示
的数字。最后结果根据下面这3条规则判断
如果转换结果的绝对值太小而无法使用float来表示，将返回float类
的正负零
如果转换结果的绝对值太大而无法使用float来表示，将返回float类
型的正负无穷大。
对于double类型的NaN值将按规定转換为float类型的NaN值。
```

#### 3.2.4 对象的创建与访问指令
```markdown
对象的创建与访问指令
Java是面向对象的程序设计语言，虚拟机平台从字节码层
面就对面向对象做了深层次的支持。有一系列指令专门用于对
象操作，可进一步细分为创建指令、字段访问指令、数组操作
指令、类型检查指令。
```
##### 3.2.4.1 创建指令
```markdown
创建指令
虽然类实例和数组都是对象，但Java虚拟机对类实例和数组的创
建与操作使用了不同的字节码指令：
1 创建类实例的指令：
创建类实例的指令 new
它接收一个操作数，为指向常量池的索引，表示要创建的类型，
执行完成后，将对象的引用压入栈。
2 创建数组的指令：
创建数组的指令： newarray、 anewarray、 multianewarray.
newarray:创建基本类型数组
anewarray:创建引用类型数组
multilanewarra/创建多维数组
上述创建指令可以用于创建对象或者数组，由于对象和数组在Java中
的广泛使用，这些指令的使用频率也非常高。
```
##### 3.2.4.2 字段访问指令
```markdown
字段访问指令
对象创建后，就可以通过对象访问指令获取对象实例或数组实例
中的字段或者数组元素。

访问类字段( static字段，或者称为类变量)的指令：
getstatic、 putstatic
访问类实例字段(非 static字段，或者称为实例变量)的指令： 
getfield、 putfield

举例：
以 getstatic指令为例，它含有一个操作数，为指向常量池
的Fieldref索引，它的作用就是获取 Fieldref指定的
对象或者值，并将其压入操作数栈。
public void sayhello（）{
	System. out.println("hello");
}
对应的字节码指令：
0 getstatic #8 <java/lang/System. out>
3 1dc #9 <hello>
5 invokevirtual #10 <java/io/Printstream println>
8 return
```

##### 3.2.4.3 数组操作指令
```markdown
数组操作指令主要有： xastore和 xload指令。具体为：
把一个数组元素加载到操作数栈的指令： 
baload、 caload、 saload、 iaload、laload、 faload、
daload、 aaload
将一个操作数栈的值存储到数组元素中的指令： 
bastore、 castore、 sastore、 iastore、lastore、
faster、 dastore、 aastore
即：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210306203435935.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
取数组长度的指令： arraylength
该指令弹出栈顶的数组元素，获取数组的长度，将长度压入栈。
说明
指令xaload表示将数组的元素压栈，比如saload、 caload分
别表示压入 short数组和char数组。指令
xaload在执行时，要求操作数中栈顶元素为数组索引i，栈顶顺
位第2个元素为数组引用a,该指令会弹出栈顶这
两个元素，并将a[i]重新压入堆栈。
xastor则专门针对数组操作，以 iastore为例，它用于
给一个int数组的给定索引赋值。在 iastore执行前
操作数栈顶需要以此准备3个元素：值、索引、数组引用， 
iastore会弹出这3个值，并将值赋给数组中指定索
引的位置。
```
##### 3.2.4.4 类型检查指令
```markdown
检查类实例或数组类型的指令： instanceof、 checkcast.
指令checkcast用于检查类型强制转换是否可以进行。如果
可以进行，那么checkcast指令不会改变操作数栈
否则它会抛出ClassCasteException异常。
指令 instanceof用来判断给定对象是否是某一个类的实例，
它会将判断结果压入操作数栈。
```
#### 3.2.5 方法的调用与返回指令
##### 3.2.5.1  方法调用指令
```markdown
方法调用指令： invokevirtual、 invokeinterface、
invokespecial、 invokestatic、 invokedynamic
以下5条指令用于方法调用：

invokeqlvirtual指令用于调用对象的实例方法，根据对象的实际类
型进行分派（虚方法分派），支持多态。这也是Java语言中最
常见的方法分派方式。
invokeinterface指令用于调用接口方法，它会在运行时搜索由特
定对象所实现的这个接口方法，并找出适合的方法进行调用
invokespecia指令用于调用一些需要特殊处理的实例方法，包括
实例初始化方法（构造器）、私有方法和父类方法。这些方法都
是静态类型绑定的，不会在调用时进行动态派发。
invokestatic指令用于调用命名类中的类方法( static方法)。
这是静态绑定的。
invokedynamic:调用动态绑定的方法，这个是JDK1.7后新加
入的指令。用于在运行时动态解析出调用点限定符所
引用的方法，并执行该方法。前面4条调用指令的分派逻辑
都固化在java虚拟机内部，而
invokedynamic指令的分派逻辑是由用户所设定的引导方法决定的。
```
##### 3.2.5.2 方法返回指令
```markdown
方法调用结束前，需要进行返回。方法返回指令是根据返
回值的类型区分的。
包括 ireturn(当返回值是 boolean、byte、char、 
short和int类型时使用)、lreturn、 freturn、dreturn
和areturn另外还有一条 return指令供声明为void的方法、实例初
始化方法以及类和接口的类初始化方法使用。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210306211618893.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
举例：
通过 ireturn指令，将当前函数操作数栈的顶层元素弹出，并将这
个元素压入调用者函数的操作数栈中（因为调用者  
非常关心函数的返回值），所有在当前函数操作数栈中的其他
元素都会被丢弃。
如果当前返回的是 synchronized方法，那么还会执行一个隐
含的 monitorexit指令，退出临界区。
```
#### 3.2.6 操作数栈管理指令
```markdown
如同操作一个普通数据结构中的堆栈那样，JVM提供的操作数栈管理
指令，可以用于直接操作操作数栈的指令。

这类指令包括如下内容
将一个或两个元素从栈顶弹出，并且直接废弃：pop,pop2
复制栈顶一个或两个数值并将复制值或双份的复制值重新压入栈顶：
dup,dup2,dup_×1，
dup2_x1, dup _x2, dup2_ x2;

将栈最顶端的两个Slot数值位置交换：swap。Java虚拟机没有提供
交换两个64位数据类型（long、 double)数值的指令
指令nop,是一个非常特殊的指令，它的字节码为0x00。和
汇编语言中的nop一样，它表示什么都不做。这条指令一般可
用于调试、占位等。

这些指令属于通用型，对栈的压入或者弹出无需指明数据类型。

说明：
不带_x的指令是复制栈顶数据并压入栈顶。包括两个指令，
dup和dup2.dup的系数代表要复制的Slot个数。

dup开头的指令用于复制1个Slot的数据。例如1个int或1个
reference类型数据
dup2开头的指令用于复制2个Slot的数据。例如1个long,或2个int,
或1个int+1个
float类型数据

带_x的指令是复制栈顶数据并插入栈顶以下的某个位置。共有4个指令，
dup_x1,dup2_x1,dup_x2,dup2_x2,对于带_x的复制插入指令，
只要将指令的dup和x的系数相加，结果即为需要插入得位置
因此
dup_x1插入位置：1+1=2,即顶2个Slot下面
dup_x2插入位置：1+2=3,即栈顶3个Slot下面
dup2_x1插入位置：2+1=3,即栈顶3个Slot下面
dup2_x2插入位置：2+2=4,即栈顶4个Slot下面
pop:将栈顶的1个Slot数值出栈。例如1个 short.类型数值
pop2:将栈顶的2个Slot数值出栈。例如1个double 类型数值，
或者2个int类型值
```
#### 3.2.7 控制转移指令
##### 3.2.7.1 比较指今
```markdown
比较指令的作用是比较占栈顶两个元素的大小，并将比较结果入栽。
比较指令有： dcmpg,dcmpl、 fcmp、fcmpl、lcmp
与前面讲解的指令类似，首字符d表示double类型，f表示float,l表
示long.对于double和float类型的数字，由于NaN的存在，各有两个
版本的比较指令。以float为例，有fcmpg和fcmpl两个指令，它
们的区别在于在数字比较时，若遇到NaN值，处理结果不同。
指令dcmpl和 dcmpg也是类似的，根据其命名可以推测其含义，
在此不再赘述。

举例
指令 fcmp和fcmpl都从中弹出两个操作数，并将它们做比较，
设栈顶的元素为v2,顶顺位第2位的元素为v1,若v1=v2,则压入0:
若v1>v2则压入1:若v1<v2则压入-1.
两个指令的不同之处在于，如果遇到NaN值， fcmpg会压入1,
而fcmpl会压入-1
```
```markdown
一 条件跳转指令
条件跳转指令通常和比较指令结合使用。在条件跳转指令执行前，
一般可以先用比较指令进行栈顶元素的准备，然后进行条件跳转。
条件跳转指令有：
ifeq,iflt,ifle,ifne,ifgt,ifge, ifnull, ifnonnull。这
些指令都接收两个字节的操作数
,用于计算跳转的位置(16位符号整数作为当前位置的offset).
它们的统一含义为：弹出栈顶元素，测试它是否满足某一条件，
如果满足条件，则跳转到给定位置
具体说明：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210306225859159.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

##### 3.2.7.2 比较条件跳转指令
```markdown
比较条件跳转指令类似于比较指令和条件跳转指令的结合体，它将
比较和跳转两个步骤合二为一
这类指令有：if_ icmpeg、if_ cmpne、if_ icmplt、
if_ icmpgt、if_ icmple、if_ icmpge、if_ acmped
和if_ acmpne
其中指令助记符加上“if_”后，以字符“i”开头的指令针对int型整数
操作(也包括 short和byte类型)，以字符“a”开
头的指令表示对象引用的比较。
具体说明：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210307002114452.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
这些指令都接收两个字节的操作数作为参数，用于计算跳转的位置。
同时在执行指令时，栈顶需要准备两个元素进行比较。
指令执行完成后，栈顶的这两个元素被清空，且没有任何数据
入栽。如果预设条件成立，则执行跳转，否则，继续执行下条语句。
```
##### 3.2.7.3 多条件分支跳转指令
```markdown
多条件分支跳转指令是专为 switch-case语句设计的，
主要有 tableswitch和lookupswitch
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210307081251330.png)
```markdown
从助记符上看，两者都是 switch语句的实现，它们的区别
tableswitch要求多个条件分支值是连续的，它内部只存放起始
值和终止值，以及若干个跳转偏移量，通过给定的操作数 index,
可以立即定位到跳转偏移量位置，因此效率比较高
指令lookupswitch内部存放着各个离散的case- offset对，每次执
行都要搜索全部的case- offset对，找到匹配的case值，并根据
对应的 offset计算跳转地址，因此效率较低。
指令tableswitchl的示意图如下图所示。由于tableswitch的
case值是连续的，因此只需要记录最低值和最高值，以及每项对应的 
offset偏移量，根据给定的 indext值通过简单的计算即可直接定
位到 offset。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210307081730907.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 3.2.7.4 无条件跳转指令
```markdown
目前主要的无条件跳转指令为goto。指令goto接收两个字节的操作
数，共同组成一个带符号的整数，用于指定指令的偏移量
指令执行的目的就是跳转到偏移暈给定的位置处。
如果指令偏移量太大，超过双字节的帯符号整数的范围，则可以使用
指令goto_w,它和goto有相同的作用，但是它接收4个字节的操作数，
可以表示更大的地址范围。
指令jsr、jsr_w、ret虽然也是无条件跳转的，但主
要用于try-final1y语句，且已经被虚拟机逐渐废弃，
故不在这里介绍这两个指令。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210307082507692.png)
#### 3.2.8 异常处理指令
##### 3.2.8.1 抛出异常指令
```markdown
(1) athrow指令
在]ava程序中显示抛出异常的操作( throw语句)都是由 athrow指
令来实现
除了使用 throw语句显示抛出异常情况之外，JVM规范还规定了许
多运行时异常会在其他]ava虚拟机指令检测到异常状况时自动抛出。
例如，在之前介绍的整数运算时，当除数为零时，虚拟机会在idiv或
ldiv指令中抛出
Arithmeticexception异常。
(2)注意
正常情况下，操作数栈的压入弹出都是一条条指令完成的。唯一的
例外情况是在抛异常时，]ava虚拟机会清除操作数根上
的所有内容，而后将异常实例压入调用者操作数上。
异常及异常的处理
过程一：异常对象的生成过程-> throw(手动/自动) -->指令： athrow
过程二: 异常的处理：抓抛模型：try-catch-finally --> 使用异常表
```
##### 3.2.8.2 异常处理与异常表
```markdown
1 处理异常：
在Java虚拟机中，处理异常( catch语句)不是由字节码指令来实
现的(早期使用jsr、ret指令)，而是采用异常表来完成
2 异常表
如果一个方法定义了一个try- catch或者try- final1y的异常处
理，就会创建一个异常表。它包含了每个异常处理或者finally块
的信息。
异常表保
存了每个异常处理信息。比如：
起始位置
结束位置
程序计数器记录的代码处理的偏移地址
被捕获的异常类在常量池中的索引
当一个异常被抛出时，3wM会在当前的方法里寻找一个匹配的处理，
如果没有找到，这个方法会强制结束并弹出当前栈帧，并且异常会重
新抛给上层调用的方法（在调用方法帧）。如果在所有帧弹出前仍
然没有找到合适的异常处理，这个线程将终止。如果这个异常在最
后一个非守护线程里抛出，将会导致JVM自己终止，比如这个线程
是个main线程。
不管什么时候抛出异常，如果异常处理最终匹配了所有异常类型，
代码就会继续执行。在这种情况下，如果方法结束后没有抛出异常，
仍然执行finally块，在 return前，它直接跳到 finally块来完成
目标
```
#### 3.2.9 同步控制指令
```java
1 方法级的同步
方法级的同步：是隐式的，即无须通过字节码指令来控制，它实
现在方法调用和返回操作之中。虚拟机可以从方法常量池的
方法表结构中的 ACC SYNCHRONIZED访问标志得知一个方法是否声明
为同步方法
当调用方法时，调用指令将会检査方法的 ACC SYNCHRONIZED访问标
志是否设置。
如果设置了，执行线程将先持有同步锁，然后执行方法。最后在方法完
成（无论是正常aa完成还是非正常完成）时释放同步锁。

在方法执行期间，执行线程持有了同步锁，其他任何线程都无法再获得
同一个锁。
如果一个同步方法执行期间抛出了异常，并且在方法内部无法处理此异
常，那这个同步方法所持有的锁将在异常抛到同步方法之外时自动释放。

举例
private int i = 0;
public synchronized void add(){
	i++;
}
对应的字节码：
e aload_0;
1 dup
2 getfield #2 < com/atguigu/javal/SynchronizedTest.i>
5 iconst_1
6 iadd
7 putfield #2 (com/atguigu/java1/SynchronizedTest.i>

2 方法内指定指令序列的同步
同步一段指令集序列：通常是由java中的 synchronized语句块来表
示的。jvm的指令集有 monitorenter和
monitorexit两条指令来支持 synchronized关键字的语义。
当一个线程进入同步代码坝时，它使用 monitorenter指令请求进入。
如果当前对象的监视器计数器为0,则它会被准许进入,若为1,则判断
持有当前监视器的线程是否为自己，如果是，则进入，否则进行等待，
直到对象的监视器计数器为0，才会被允许进入同步块。
当线程退出同步块时，需要使用monitorexiti声明退出。在Java虚
拟机中，任何对象都有一个监视器与之相关联，用来判断对象是否
被锁定，当监视器被持有后，对象处于锁定状态。
指令monitorenter和 monitorexit在执行时，都需要在操作数栈
顶压入对象，之后 monitorenter和 monitorexitl的锁定和释放
都是针对这个对象的监视器进行的。
下图展示了监视器如何保护临界区代码不同时被多个线程访问，
只有当线程4离开临界区后，线程1、2、3才有可能进入。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210307100126952.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复jvm2
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

