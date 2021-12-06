# 深入理解JVM-篇章3
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210716141751722.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1 类的加载过程
```markdown
在Java中数据类型分为基本数据类型和引用数据类型。基本数据
类型由虚拟机预先定义，引用数据类型则需要进行类的加载
按照Java虚拟机规范，从class文件到加载到内存中的类，到
类卸载出内存为止，它的整个生命周期包括如下7个阶段
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210307164303716.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
其中，验证、准备、解析3个部分统称为链接( Linking)
从程序中类的使用过程看：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/202103071645595.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 1.1 加载阶段
#### 1.1.1 加载完成的操作
```markdown
加载的理解
所谓加载，简而言之就是将Java类的字节码文件加载到机器内存中，
并在内存中构建出Java类的原型一一类模板对象。所谓类模板对象，
其实就是Java类JVM内存中的一个快照JVMM将从字节码文件中
解析出的常量池、类字段、类方法等信息
存储到类模板中，这样JVM在运行期便能通过类模板而获取Java
类中的任意信息，能够对Java类的成员变量进行遍历，也能
进行Java方法的调用。
反射的机制即基于这一基础。如果JVM没有将Java类的声明信息
存储起来，则JVM在运行期也无法反射。
加载完成的操作
加载阶段，简言之，查找并加载类的二进制数据，生成Class的实例
在加载类时，Java虚拟机必须完成以下3件事情
通过类的全名，获取类的二进制数据流
解析类的二进制数据流为方法区内的数据结构(Java类模型)
创建java.lang.Class类的实例，表示该类型。作为方法区这个类的
各种数据的访问入口
```
#### 1.1.2 二进制流的获取方式
```markdown
对于类的二进制数据流，虚拟机可以通过多种途径产生或获得。
(只要所读取的字节码符合JVM规范即可)
虚拟机可能通过文件系统读入一个class后缀的文件（最常见）
读入jar、zip等归档数据包，提取类文件。
事先存放在数据库中的类的二进制数
使用类似于HTP之类的协议通过网络进行加载
在运行时生成一段Class的二进制信息等
在获取到类的二进制信息后，Java虚拟机就会处理这些数据，
并最终转为一个java.lang.Class的实例。
如果输入数据不是Classfilel的结构，则会抛出
Classformaterror。
```
#### 1.1.3 类模型与Class实例的位置
```markdown
1 类模型的位置
加载的类在JVM中创建相应的类结构，类结构会存储在方法区
(JDK1.8之前：永久代：JDK1.8及之后：元空间)。
2 Class实例的位置
类将.class文件加载至元空间后，会在堆中创建一个
]ava.lang.Class对象，用来封装类位于方法区内的数据结构，该
Class对象是在加载类的过程中创建的，每个类都对应有一个Class
类型的对象。
3 图示
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210307170157956.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
外部可以通过访问代表Order类的class对象来获取 Orderl的
类数据结构。
4 再说明
Class类的构造方法是私有的，只有JVM能够创建。
java.lang.Class实例是访问类型元数据的接口，也是实现反射的关
键数据、入口。通过Class类提供的接口，可以获得目标类所关
联.class文件中具体的数据结构：方法、字段等信息。
```
#### 1.1.4 数组类的加载
```markdown
创建数组类的情况稍微有些特殊，因为数组类本身并不是由类加载
器负责创建，而是由JVM在运行时根据需要而直接创建的
但数组的元素类型仍然需要依靠类加载器去创建。创建数组
类(下述简称A)的过程：
如果数组的元素类型是引用类型，那么就遵循定义的加载过程递
归加载和创建数组A的元素类型
JVM使用指定的元素类型和数组维度来创建新的数组类。
如果数组的元素类型是引用类型，数组类的可访问性就由元素类
型的可访问性决定。否则数组类的可访问性将被缺省定义为Public。
```
### 1.2 链接阶段
#### 1.2.1 验证阶段( Verification)
```markdown
当类加载到系统后，就开始链接操作，验证是链接操作的第一步。
它的目的是保证加载的字节码是合法、合理并符合规范的。
验证的步骤比较复杂，实际要验证的项目也很繁多，大体上Java虚
拟机需要做以下检査，如图所示
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021030717210128.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
整体说明：
验证的内容则涵盖了类数据信息的格式验证、语义检查、字节码验证，
以及符号引用验证等。
其中格式验证会和加载阶段一起执行。验证通过之后，类加载器
オ会成功将类的二进制数据信息加载到方法区中。
格式验证之外的验证操作将会在方法区中进行
链接阶段的验证虽然拖慢了加载速度但是它免了在字节码运行时
还需要进行各种检查。（磨刀不误砍柴工）
具体说明：
1 格式验证：是否以魔数 OXCAFEBABE开头，主版本和副版本号
是否在当前Java虚拟机的支持范围内，数据中每一个项是否都拥有
正确的长度等

2 Java拟机会进行字节码的语义检查，但凡在语义上不符合规范
的，虚拟机也不会给予验证通过。比如
是否所有的类都有父类的存在(在Java里，除了 object外，其他
类都应该有父类)
是否一些被定义为finaL的方法或者类被重写或继承了
非抽象类是否实现了所有抽象方法或者接口方法
是否存在不兼容的方法（比如方法的签名除了返回值不同，其
他都一样，这种方法会让虚拟机无从下手调度； abstract情况
下的方法，就不能是fina的了）

3 Java虚拟机还会进行字节码验证，字节码验证也是验证过程中最为
复杂的一个过程。它试图通过对字节码流的分析，判断字节
码是否可以被正确地执行。比如：
在字节码的执行过程中，是否会跳转到一条不存在的指令
函数的调用是否传递了正确类型的参数
变量的赋值是不是给了正确的数据类型等
栈映射帧( StackMaptable)就是在这个阶段，用于检测在特定的字
节码处，其局部变量表和操作数栈是否有着正确的数据类型。
但遗憾的是，100%准确地判断一段字节码是否可以被安全执行是
无法实现的，因此，该过程只是尽可能地检査出可以预知的明显
的问题。如果在这个阶段无法通过检查，虚拟机也不会正确装载这个
类。但是，如果通过了这个阶段的检查，也不能说明这个类是完全没
有问题的
```
#### 1.2.2 准备阶段
```markdown
准备阶段( Preparation),简言之，为类的静态变量分配内存，并将
其初始化为默认值
当一个类验证通过时，虚拟机就会进入准备阶段。在这个阶段，
虚拟机就会为这个类分配相应的内存空间，并设置默认初始值。
java虚拟机为各类型变量默认的初始值如表所示。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210307173814459.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
注意：Java并不支持 boolean类型，对于 boolean类型，内
部实现是int,由于int的默认值是0,故对应的，boolean的默认
值就是false

注意
1 这里不包含基本数据类型的字段用 static final修饰的情況，因
为final在编译的时候就会分配了，准备阶段会显式赋值
2 注意这里不会为实例变量分配初始化，类变量会分配在方法
区中，而实例变量是会随着对象一起分配到Java堆中。
3 在这个阶段并不会像初始化阶段中那样会有初始化或者代码被执行
```
#### 1.2.3 解析阶段
```markdown
解析阶段( Resolution),简言之，将类、接口、字段和方法的符号
引用转为直接引用。 
1 具体描述
符号引用就是一些字面量的引用，和虚拟机的内部数据结构和
和内存布局无关。比较容易理解的就是在Class类文件中
通过常量池进行了大量的符号引用。但是在程序实际运行时，只
有符号引用是不够的，比如当如下 printin（）方法被
调用时，系统需要明确知道该方法的位置。
举例：输出操作 System,out. printin（）对应的字节码
invokevirtual #24 <java/io/Printstream println>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021030717582067.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
以方法为例，Java虚拟机为每个类都准备了一张方法表，将其所
有的方法都列在表中，当需要调用一个类的方法的时位
只要知道这个方法在方法表中的偏移量就可以直接调用该方法。
通过解析操作，符号引用就可以转变为目标方法在类
中方法表中的位置，从而使得方法被成功调用

2 小结：
所谓解析就是将符号引用转为直接引用，也就是得到类、字段、
方法在内存中的指针或者偏移量。因此，可以说，如果
直接引用存在，那么可以肯定系统中存在该类、方法或者字段。但
只存在符号引用，不能确定系统中一定存在该结构。

不过Java虚拟机规范并没有明确要求解析阶段一定要按照顺序执行。
在 Hotspot VM中，加载、验证、准备和初始化会
按照顺序有条不素地执行，但链接阶段中的解析操作往往会伴随着
JVM在执行完初始化之后再执行。
字符串的复习
最后，再来看一下 CONSTANT_ String的解析。由于字符串在程序
开发中有着重要的作用，因此，读者有必要了解一下
String在Java虚拟机中的处理。当在Java代码中直接使用字符串
量时，就会在类中出现 CONSTANT_ String,它表示
字符串常量，并且会引用一个 CONSTANT_UTF8的常
在Java虚拟行中的常量池中，会维护一张字符串拘留表( intern),
它会保存所有出现过的字符串常量（井且没有重复项。只要以 
CONSTANT_ String形式出现的字符串也都会在这张表中。
使用 String. intern（）方法可以得到一个字行串在拘留表
中的引用，因为该表中没有重复项，所以任
何字面相同的字符串的 String.intern（）方法返回总是相等的
```
### 1.3 Initialization(初始化阶段)
```markdown
简言之，为类的静态变量赋予正确的初始值。
1 具体描述
类的初始化是类装载的最后一个阶段。如果前面的步骤都没有问题，
那么表示类可以顺利装载到系统中。此时，类才会开始执行Java字
节码。(即：到了初始化阶段，才真正开始执行类中定义的Java程序
代码。)
初始化阶段的重要工作是执行类的初始化方法：clinit>（）方法
该方法仅能由java编译器生成并由JVM调用，程序开发者无法自定
义一个同名的方法，更无法直接在Java程序中调用该方法，虽然该
方法也是由字节码指令所组成
它是由类静态成员的赋值语句以及static语句块合并产生的。

2 说明
2.1 在加载一个类之前，虚拟机总是会试图加载该类的父类，因此父
类的<clinit>总是在子类<c1init>之前被调用。
也就是说，父类的 static块优先级高于子类。
2.2 Java编译器并不会为所有的类都产生< clinit>（）初始化方法。
哪些类在编译为字节码后，字节码文件中将不会包括clinit>（）方法？
一个类中并没有声明任何的类变量，也没有静态代码块时
一个类中声明类变量，但是没有明确使用类变量的初始化语句以及
静态代码块来执行初始化操作时
一个类中包含 static final修饰的基本数据类型的字段，这些
类字段初始化语句采用编译时常量表达式
```
```markdown
< clinit>0的线程安全性
对于< clinit>（）方法的调用，也就是类的初始化，虚拟机会在内
部确保其多线程环境中的安全性
虚拟机会保证一个类的<clinit>（）方法在多线程环境中被正确地
加锁、同步，如果多个线程同时去初始化一个类，那么只会有一个
线程去执行这个类的<clinit>（）方法，其他线程都需要阻塞等待，
直到活动线程执行<c1init>（）方法完毕。
正是因为函数<clinit>（）帯锁线程安全的，因此，如果在一个类
的<clinit>（）方法中有耗时很长的操作，就可能造成多
个线程阻塞，引发死锁。并且这种死锁是很难发现的，因为
看起来它们并没有可用的锁信息。
如果之前的线程成功加载了类，则等在队列中的线程就没有机会
再执行<clinit>（）方法了。那么，当需要使用这个类时,虚拟机
会直接返回给它己经准备好的信息。


类的初始化情况：主动使用vs被动使用
Java程序对类的使用分为两种：主动使用和被动使用
主动使用
Class只有在必须要首次使用的时候才会被装载，Java虚拟机
不会无条件地装载Class类型。Java虚拟机规定，一个类或
接口在初次使用前，必须要进行初始化。这里指的“使用”，是指
主动使用，主动使用只有下列几种情况：（即：如果出现如
下的情况，则会对类进行初始化操作。而初始化操作之前的
加载、验证、准备己经完成。）

1 当创建一个类的实例时，比如使用new关键字，或者通过
反射、克隆、反序列化。
2 当调用类的静态方法时，即当使用了字节码 invokestatic指令
3 当使用类、接口的静态字段时(fina修饰特殊考虑)，比如，
使用 getstatic或者 putstatic指令。（对应访问变量、
赋值变量操作）
4 当使用java.lang. reflect包中的方法反射类的方法时。
比如：Class. forName("com. atgulgu.java.Test")
5 当初始化子类时，如果发现其父类还没有进行过初始化，
则需要先触发其父类的初始化。
6 如果一个接口定义了 default方法，那么直接实现或者间接实现该
接口的类的初始化，该接口要在其之前被初始化
7 当虚拟机启动时，用户需要指定一个要执行的主类(包含main（）方
法的那个类），虚拟机会先初始化这个主类。
8 当初次调用 Methodhandle实例时，初始化该 Methodhandle指
向的方法所在的类。（涉及解析
REF_ getstatic、REF_ putstatic、REF_ invokestatic
方法句柄对应的类）
对5,补充说明
当Java虚拟机初始化一个类时，要求它的所有父类都已经被
初始化，但是这条规则并不适用于接口。
在初始化一个类时，并不会先初始化它所实现的接口

类的初始化情况：主动使用vs被动使用
Java程序对类的使用分为两种：主动使用和被动使用

一、主动使用
Class只有在必须要首次使用的时候オ会被装载，Java虚拟机不会
无条件地装载Ccass类型。Java虚拟机规定，一个类或接口在初次使
用前，必须要进行初始化。这里指的“使用”，是指主动使用，主
动使用只有下列几种情况：（即：如果出现如下的情况，则会对
类进行初始化操作。而初始化操作之前的加载、验证、准备己经完成。）
1.当创建一个类的实例时，比如使用new关键字，或者通过反射、
克隆、反序列化
2.当调用类的静态方法时，即当使用了字节码 invokestatic指令。
3.当使用类、接口的静态字段时(fina1修饰特殊考虑)，比如，
使用 getstatic或者 putstatic指令。（对应访问变量
、赋值变量操作）
4.当使用java.1ang. reflect包中的方法反射类的方法时。
比如：Class. forname("com. atguigu,java.Test")
5 当初始化子类时，如果发现其父类还没有进行过初始化，
则需要先触发其父类的初始化。
6 如果一个接口定义了 default方法，那么直接实现或者间接实现
该接口的类的初始化，该接口要在其之前被初始化。
7 当虚拟机启动时，用户需要指定一个要执行的主类(包含main（）方
法的那个类），虚拟机会先初始化这个主类。
8 当初次调用 Methodhandle实例时，初始化该 Methodhandle指
向的方法所在的类。（涉及解析REF_ getstatic、REF_ putstatic、
REF_ invokestatic方法句柄对应的类）
针对5,补充说明：
当Java虚拟机初始化一个类时，要求它的所有父类都已经被初始化，
但是这条规则并不适用于接口。
在初始化一个类时，并不会先初始化它所实现的接口

针对7 说明：
JVM启动的时候通过引导类加载器加载一个初始类。这个类在
调用public static void main( String [])方法之前被链接和
初始化。这个方法的执行将依次导致所需的类的加载，链接和初始化。

二、被动使用
除了以上的情况属于主动使用，其他的情况均属于被动使用。
被动使用不会引起类的初始化。
也就是说：并不是在代码中出现的类，就一定会被加载或者初始化。
如果不符合主如动使用的条件，类就不会初始
1 当访问一个静态字段时，只有真正声明这个字段的类才会被初始化。
当通过子类引用父类的静态变量，不会导致子类初始化
2 通过数组定义类引用，不会触发此类的初始化
3 引用常量不会触发此类或接口的初始化。因为常量在链接阶段就
己经被显式赋值了。
4 调用classloader类的loadClass（）方法加载一个类，并
不是对类的主动使用，不会导致类的初始化。
```
### 1.4 类的 Using（使用）
```markdown
任何一个类型在使用之前都必须经历过完整的加载、链接和
初始化3个类加载步骤。一旦一个类型成功经历过这3个
步骤之后，便“万事俱备，只欠东风”，就等着开发者使用了。
开发人员可以在程序中访问和调用它的静态类成员信
息(比如：静态字段、静态方法)，或者使用new关键字为其创
建对象实例。
```
### 1.5 类的 Unloading（卸载）
```markdown
类、类的加载器、类的实例之间的引用关系
在类加载器的内部实现中，用一个Java集合来存放所加载类的引用。
另一方面，一个Class对象总是会引用它的类加载器
调用Class对象的 getclassloader（）方法，就能获得它的类加
载器。由此可见，代表某个类的Class实例与其类的加
载器之间为双向关联关系。

一个类的实例总是引用代表这个类的Class对象。在 Object:类中定
义了getclass（）方法，这个方法返回代表对象所属类
的Class对象的引用。此外，所有的Java类都有一个静态属
性class,它引用代表这个类的Class对象。

类的生命周期
当 Sample类被加载、链接和初始化后，它的生命周期就开始了。
当代表Sample类的Class对象不再被引用，即不可触及
时，Class对象就会结東生命周期， Sample类在方法区内的
数据也会被卸载，从而结束 Sample类的生命周期。

一个类何时结東生命周期，取决于代表它的Class对象何时
结束生命周期。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210307215341641.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
loader1变量和obj变量间接应用代表Sample类的Class对象，
而objclass变量则直接引用它。
```
```markdown
如果程序运行过程中，将上图左侧三个引用变量都置为null,
此时 Sample对象结束生命周期，MyClassloader对象结束
生命周期，代表 Sample类的class对象也结東生命周期，
Sample类在方法区内的二进制数据被卸载
当再次有需要时，会检查 Sample类的Class对象是否存在，
如果存在会直接使用，不再重新加载：如果不存在 Sample类
会被重新加载，在Java虚拟机的堆区会生成一个新的代表Sample
类的Class实例（可以通过哈希码查看是否是同一个实例

四、类的卸载
(1)启动类加载器加载的类型在整个运行期间是不可能被卸
载的(jvm和jls规范)
(2)被系统类加载器和扩展类加载器加载的类型在运行期间不太可
能被卸载，因为系类加器实例或者扩展类的实例基本上在整个运行
期间总能直接或者间接的访问的到，其达到 unreachable的可
能性极小。
(3)被开发者自定义的类加载器实例加载的类型只有在很简单的上
下文环境中才能被卸载，而且一般还要借助于强制调用
虚拟机的垃圾收集功能才可以做到。可以预想，稍微复杂点的应
用场景中（比如：很多时候用户在开发自定义类加载器实
例的时候采用缓存的策略以提高系统性能），被加载的类型在运行
期间也是几乎不太可能被卸载的（至少卸载的时间是不确
定的）。
综合以上三点，一个已经加载的类型被卸载的几率很小至少被卸载
的时间是不确定的。同时我们可以看的出来，开发者在
开发代码时候，不应该对虚拟机的类型卸载做任何假设的前提下，
来实现系统中的特定功能。
```
## 2 类加载器
```markdown
类加载器是JVM执行类加载机制的前提。
Classloader的作用
Classloader是Java的核心组件，所有的Class都是由
Classloader进行加载的，Classloader负责通过各种方式将
Class信息的二进制数据流读入JVM内部，转换为一个与目标类
对应的java.lang.Class对象实例。然后交给Java虚拟
机进行链接、初始化等操作。因此，Classloader在整个装载阶段，
只能影响到类的加载，而无法通过Classloader去
改变类的链接和初始化行为。至于它是否可以运行，则由 
Execution Engine决定。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210307221522772.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
loader1变量和obj变量间接应用代表 Sample类的Class对象，
而objclass变量则直接引用它。
```
### 2.1 类加截的分类
```markdown
类的加载分类：显式加载vs隐式加载
class文件的显式加载与隐式加载的方式是指JVM加载class文
件到内存的方式。
显式加载指的是在代码中通过调用Classloader加载class对象，
如直接使用Class. forName(name)或
this. getClass().getClassloader().loadClass（）
加载class对象
隐式加载则是不直接在代码中调用ClassLoader的方法加载class对
象，而是通过虚拟机自动加载到内存中，如在
加载某个类的class文件时，该类的class文件中引用了另外一个
类的对象，此时额外引用的类将通过JVM自动加载到内存中。
在日常开发以上两种方式一般会混合使用
```
### 2.2 类加载器的必要性
```markdown
一般情况下，Java开发人员并不需要在程序中显式地使用类加载器，
但是了解类加载器的加载机制却显得至关重要。从以下几个方面说：
避免在开发中遇到java.lang.ClassNotFoundException异常
或java.lang.NoClassDefFoundError异常时
,手足无措。只有了解类加载器的加载机制才能够在出现异常的时候
快速地根据错误异常日志定位问题和解决问题
需要支持类的动态加载或需要对编译后的字节码文件进行加解密
操作时，就需要与类加载器打交道了。
开发人员可以在程序中编写自定义类加载器来重新定义类的加
载规则，以便实现一些自定义的处理逻辑。
```
### 2.3 命名空间
```markdown
1 何为类的唯一性？
对于任意一个类，都需要由  加载它的类加载器和这个类本身一
同确认其在]ava虚拟机中的唯一性。毎一个类加载器，都
拥有一个独立的类名称空间：比较两个类是否相等，只有在这
两个类是由同一个类加载器加载的前提下才有意义。否则
,即使这两个类源自同一个Class文件，被同一个虚拟机加载，
只要加载他们的类加载器不同，那这两个类就必定不相等
2 命名空间
每个类加载器都有自己的命名空间，命名空间由该加载器及
所有的父加载器所加载的类组成
在同一命名空间中，不会出现类的完整名字（包括类的包名）
相同的两个类
在不同的命名空间中，有可能会出现类的完整名字（包括类的
包名）相同的两个类
在大型应用中，我们往往借助这一特性，来运行同一个类的不同版本。
```
### 2.4 类的加载器分类
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021030723563344.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 2.4.1 引导类加戟器
```markdown
启动类加载器(引导类加载器， Bootstrap Classloader)
这个类加载使用C/C++语言实现的，嵌套在JVM内部
它用来加载Java的核心库
( JAVA HOME/jre/lib/rt.jar或sun.boot.class.path路径下的内容)。
用于提供JVM自身需要的类
并不继承自java.lang.ClassLoader,没有父加载器。
出于安全考虑， Bootstrap启动类加载器只加载包名为java、 javax、sun
等开头的类加载扩展类和应用程序类加载器，并指定为他们的父类加载器。

使用-XX:+TraceclassLoading参数得到
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210308000812166.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 2.4.2 扩展类加载器( Extension Classloader)
```markdown
Java语言编写，由sun.misc. Launcher$ExtClassloader实现
继承于Clas Loader类
父类加载器为启动类加载器
从java.ext,dirs系统属性所指定的目录中加载类库，或从JDK的
安装目录的jre/lib/ext子目录下加载类库。如果用户创建的JAR
放在此目录下，也会自动由扩展类加载器加载。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210308003121697.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 2.4.3 系统类加载器
```markdown
应用程序类加载器(系统类加载器，AppClassloader)
java语言编写，由sun.misc. Launchers$AppClassloader实现
继承于ClassLoader类
父类加载器为扩展类加载器
它负责加载环境变量classpath或系统属性java.class.path指定路
径下的类库应用程序中的类加载器默认是系统类加载器。
它是用户自定义类加载器的默认父加载器
通过Classloader的getSystemClassLoader()方法可以获取到该类加
载器
```
#### 2.4.4 用户自定义类加载器
```markdown
在Java的日常应用程序开发中，类的加载几乎是由上述3种类加载器
相互配合执行的。在必要时，我们还可以自定义类加载器，来定制
类的加载方式。
体现Java语言强大生命力和巨大魅力的关键因素之一便是，Java开
发者可以自定义类加载器来实现类库的动态加载
加载源可以是本地的JAR包，也可以是网络上的远程资源。
通过类加载器可以实现非常绝妙的插件机制，这方面的实际应
用案例举不胜举。例如，著名的OSGI组件框架，再如
Eclipse的插件机制。类加载器为应用程序提供了一种动态增加新功
能的机制，这种机制无须重新打包发布应用程序就能实现。

同时，自定义加载器能够实现应用隔离，例如 Tomcat, Spring等中
间件和组件框架都在内部实现了自定义的加载
器，并通过自定义加载器隔离不同的组件模块。这种机制比C/C++程
序要好太多，想不修改C/C++程序就能为其新
增功能，几乎是不可能的，仅仅一个兼容性便能阻挡住所有
美好的设想
自定义类加载器通常需要继承于ClassLoader
```
### 2.5 双亲委派模型
```markdown
类加载器用来把类加载到Java虚拟机中。从JDK1.2版本开始，类的
加载过程采用双亲委派机制，这种机制能更好地保证
Java平台的安全。
1 定义
如果一个类加载器在接到加载类的请求时，它首先不会自己尝试去
载这个类，而是把这个请求任务委托给父类加载器
去完成，依次递归，如果父类加载器可以完成类加载任务，就成
功返回。只有父类加载器无法完成此加载任务时，才自己去加载。
2 本质
规定了类加载的顺序是：引导类加载器先加载，若加载不到，由扩
展类加载器加载，若还加载不到，才会由系统类加载
器或自定义的类加载器进行加载。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210308144625299.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210308144707704.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 2.5.1 优势与劣势
```markdown
双亲委派机制优势
避免类的重复加载，确保一个类的全局唯一性
Java类随着它的类加载器一起具备了一种带有优先级的层次关系，
通过这种层级关可以避免类的重复加载，当父亲
已经加载了该类时，就没有必要子ClassLoader再加载一次
保护程序安全，防止核心API被随意纂改

2 代码支持
双亲委派机制在
java.lang,Classloader,loadClass( String, boolean)
接口中体现。该接口的逻辑如下：
(1)先在当前加载器的缓存中查找有无目标类，如果有，直接返回。
(2)判断当前加载器的父加載器是否为空，如果不为空，
则调用 parent.loadclass(nane, false)接口进行加载。
(3)反之，如果当前加载器的父类加载器为空，则调用
findbootstrapclassornul(name)接口，让引导类加载器进行
加载。
(4)如果通过以上3条路径都没能成功加载，则调用 
findclass(name)接口进行加载。该接口最终会调用
java.1ang.Classloader接口的 definedClass系列
的 native接口加载目标Java类。
双亲委派的模型就隐藏在这第2和第3步中。
3.举例
假设当前加载的是java.lang. objecti这个类，很显然，该
类属于JDK中核心得不能再核心的一个类，因此一定只能由
引导类加载器进行加载。当JVM准备加载javalang.Object时，
JVM默认会使用系统类加载器去加载，按照上面
的理转，在第1步从系统类的缓存中背定查找不到该类，于
是进入第2步由于从系统类加载器的父加载器是扩展类加载器，
于是扩展类加载器继续从第一步开始重复。由于扩展类加
载器的缓存中也一定查找不到这类，因此进入第二步。扩展类
的父加载器是null,因此系统调用 findClass( String),最终通
过引导类加载器进行加载。

4 思考
如果在自定义的类加载器中重写
java.lang.Classloader.loadClass( string)或java.lang.Classloader.loadClass( String, boolean)
方法，抹去其中的双亲委派机制，仅保留上面这4步中的第1步与第4步，
那么是不是就能够加载核心类库了呢？
这也不行！因为JDK还为核心类库提供了一层保护机制。不管是自定
义的类加载器，还是系统类加载器抑或扩展类加载
器，最终都必须调用
java.ang.Classloader. definedClass( String,byte],int,int,Protectiondomain)方法，
而该方法会执行 predefinedClass()接口，该接口中提供了对JDK核心类库
的保护。

5 双亲委托模式的弊端
检查类是否加载的委托过程是单向的，这个方式虽然从结构上说比较
清晰，使各个Classloaderl的职责非常明确，但是
同时会带来一个问题，即顶层的classloader无法访问底层的classl
oader所加载的类。
通常情况下，启动类加载器中的类为系统核心类，包括一些重要
的系统接口，而在应用类加载器中，为应用类。按照这
种模式，应用类访问系统类自然是没有问题，但是系统类访问应用
类就会出现问题。比如在系统类中提供了一个接口，
该接口需要在应用类中得以实现，该接口还绑定一个工厂方法，用
于创建该接口的实例，而接口和工厂方法都在启动类
加载器中。这时，就会出现该工厂方法无法创建由应用类加载器
加载的应用实例的问题。
结论
由于Java虚拟机规范并没有明确要求类加载器的加载机制一定要
使用双亲委派模型，只是建议采用这种方式而己
比如在 Tomcat中，类加载器所采用的加载机制就和传统的双系委
派模型有一定区别，当缺省的类加载器接收到一个类的
加载任务时，首先会由它自行加载，当它加载失败时，オ会将类的
加载任务委派给它的超类加载器去执行，这同时也是
Servlet规范推荐的一种做法
```
#### 2.5.2 破坏双亲委派机制
##### 2.5.2.1 破坏双亲委派机制1
```markdown
破坏双亲委派机制1
双亲委派模型并不是一个具有强制性约束的模型，而是Java设
计者推荐给开发者们的类加载器实现方式。
在Java的世界中大部分的类加载器都遵循这个模型，但也有例外
的情况，直到Java模块化出现为止，双亲委派模型主要
出现过3次较大规模“被破坏”的情况。
第一次破坏双亲委派机制
双亲委派模型的第一次“被破坏”其实发生在双亲委派模型出现之前
一即JDK1.2面世以前的“远古”时代。
由于双亲委派模型在JDK1.2之后才被引入，但是类加载器的概念
和抽象类java.lang.Classloaderl则在Java的第一个版本中
就已经存在，面对己经存在的用户自定义类加载器的代码，
Java设计者们引入双亲委派模型时不得不做出一些妥协，为了
兼容这些已有代码，无法再以技术手段避免lloadClass()被子
类覆盖的可能性，只能在JDK1.2之后的java.lang.ClassLoader中
添加一个新的 protected方法 findclass(),并引导用户编写
的类加载逻辑时尽可能去重写这个方法，而不是在loadClass()中
编写代码。上节我们已经分析过loadclass()方法，双亲委派的
具体逻辑就实现在这里面，按照loadClass()方法的逻辑，如果
父类加载失败，会自动调用自己的 findclass()方法来完成加载，
这样既不影响用户按照自己的意愿去加载类，又可以保证新写出
来的类加载器是符合双亲委派规则的。
```
##### 2.5.2.2 破坏双亲委派机制2
```markdown
第二次破坏双亲委派机制：线程上下文类加载器
双亲委派模型的第二次“被破坏”是由这个模型自身的缺陷导致的，
双亲委派很好地解决了各个类加载器协作时基础类
型的一致性问题（越基础的类由越上层的加载器进行加载），
基础类型之所以被称为“基础”，是因为它们总是作为被
用户代码继承、调用的API存在，但程序设计往往没有绝对不变的
完美规则，如果有基础类型又要调用回用户的代码
那该怎么办？
这并非是不可能出现的事情，一个典型的例子使是JNDI服务，
JNDI现在己经是Java的标准服务，它的代码由启动类加
载器来完成加载(在JDK1.3时加入到 rt jar的)，肯定属于Java中
很基础的类型了。但JNDI存在的目的就是对资源进行查找和集
中管理，它需要调用由其他厂商实现并部署在应用程序
的classpath下的JNDI服务提供者接口
(Service Provider Interface,SPI)
的代码，现在问题来了，启动类加载器是绝不可能认识、加载这些
代码的，那该怎么办？(SPT:在Java平台中，通常把核心类rt,jar中
提供外部服务、可由应用层自行实现的接口称为SPT)
为了解決这个困境，]ava的设计团队只好引入了一个不太优雅
的设计：线程上下文类加载器（ Thread Context
C1 assloader)。这个类加载器可以通过java.lang. Thread类的 
setcontextclassloader()方法进行设置，如果创建线程时还未
设置，它将会从父线程中继承一个，如果在应用程序的全局
范围内都没有设置过的话，那这个类加载器默认就是应用程序类
加载器。
有了线程上下文类加载器，程序就可以做一些“舞弊”的事情
了。JNDI服务使用这个线程上下文类加载器去加载所需的
SPI服务代码，这是一种父类加载器去请求子类加载器完成
类加载的行为，这种行为实际上是打通了双系委派模型的层次
结构来逆向使用类加载器，已经违背了双亲委派模型的一
般性原则，但也是无可奈何的事情。Java中涉及SPI的加载基
本上都采用这种方式来完成，例如JNDI、JDBC、JCE、JAXB和
JBI等。不过，当SPI的服务提供者多于一个的时候，代码就只能
根据具体提供者的类型来硬编码判断，为了消除这种极不优雅的
实现方式，在JDK6时，JDK提供了一种相对合理的解决方案。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210308154052991.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
默认上下文加载器就是应用类加载器，这样以上下文
加载器为中介，使得启动类加载器中的代码也可以访问
应用类加载器中的类
```
##### 2.5.2.3 破坏双亲委派机制3
```markdown
第三次破坏双亲委派机制：
双亲委派模型的第三次“被破坏”是由于用户对程序动态性的追
求而导致的。如：代码替換( Hot Swap)、
模块热部署( Hot Deployment)等
IBM公司主导的JSR-291(即 OSGI R4.2)实现模块化热部署
的关键是它自定义的类加载器机制的实现，每一个程序模
块(OSGi中称为 Bundle)都有一个自己的类加载器，当需要
更換一个 Bundle时，就把Bundle连同类加载器一起换掉以实
现代码的热替换。在OSGi环境下，类加载器不再双亲委派模型
推荐的树状结构，而是进一步发展为更加复杂的网状结构
当收到类加载请求时，OSGi将按照下面的顺序进行类搜索：
1)将以java.*开头的类，委派给父类加载器加载
2)否则，将委派列表名单内的类，委派给父类加载器加载。
3)否则，将 Import列表中的类，委派给 Export这个类的 
Bundle的类加载器加较。
4)否则，查找当前 Bundle的ClassPath,使用自己的类加载器加载。
5)否则，查找类是否在自己的 Fragment Bundle中，如果在，
则委派给 Fragment Bundle的类加载器加载。
6)否则，查找 Dynamic Import列表的 Bundle,委派给对应 
Bundle的类加载器加载。
7)否则，类查找失败。
说明：只有开头两点仍然符合双亲委派模型的原则，其余
的类查找都是在平级的类加载器中进行的
小结
这里，我们使用了“被破坏”这个词来形容上述不符合双亲委
派模型原则的行为，但这里“被破坏”并不一定是带有贬义的。
只要有明确的目的和充分的理由，突破旧有原则无疑是一种创新。

正如：OSGi中的类加载器的设计不符合传统的双亲委派的类
加载器架构，且业界对其为了实现热部署而带来
的额外的高复杂度还存在不少争议，但对这方面有了解的技术
人员基本还是能达成一个共识，认为
OSGi中对类加载器的运用是值得学习的，完全弄懂了OSGi的
实现，就算是掌握了类加载器的精梓。
```
## 3 JVM监控及诊断工具-命令行篇

### 3.1 概述
```markdown
简单命令行工具
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311204037786.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311204045487.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311204122816.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311204058932.png)
### 3.2 jps
```markdown
jps：查看正在运行的Java进程
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031120420896.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311204227312.png)

```markdown
可以通过 jps -help 来查看对应的参数信息
```
```markdown
options参数
综合使用：
jps -l -m等价于jps -lm
如何将信息输出到同级文件中：
语法：命令 > 文件名称
例如：jps -l > a.txt
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311204253972.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
hostid参数
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311204322557.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 3.3 jstat:查看JVM统计信息
```markdown
jstat( JVM Statistics Monitoring TooL):用于监视虛拟机各种运行状
态信息
的命令行工具。它可以是示本地或者远程虚拟机进程中的类
装载、内存、垃圾收集、
JIT编译等运行数据。

在没有GUI图形界面，只提供了纯文本控制台环境的服务
器上，它将是运行期定位虚
拟机性能问题的首选工具。常用于检测垃圾回收问题以及
内存泄漏问题
```
```markdown
选项option可以由以下值构成。

类装载相关的：
-class:显示ClassLoaderl的相关信息：类的装载、卸载数量、
总空间、
类装载所消耗的时间等

垃圾回收相关的：
-gc: 显示与Gc相关的堆信息。包括Eden区、两个Survivor区、
老年代
永久代等的容量、已用空间、GC时间合计等信息。
-gccapacity:显示内容与-gc基本相同，但输出主要关注
Java堆各个区域
使用到的最大、最小空间。
-gcutil:显示内容与-gc基本相同，但输出主要关注己使用
空间占总空间
的百分比。
-gccause:与-gutil功能一样，但是会额外输出导致最后
一次或当前正
在发生的GC产生的原因
-gcnew:显示新生代GC状况
-gcnewcapacity:显示内容与-gcnew基本相同，输出主要
关注使用到的
最大、最小空间
-geold:显示老年代GC状况

JIT相关的：
-compiler: 显示JIT编译器编译过的方法、耗时等信息
-printcompilation:输出已经被JIT编译的方法

我们可以比较Java进程的启动时间以及总GC时间(GCT列)，或者
两次测量的间隔时间
以及总GC时间的增量，来得出GC时间占运行时间的比例。
如果该比例超过20%,则说明目前堆的压力较大：如果该比例超
过90%,则说明堆里几乎没有
可用空间，随时都可能抛出OOM异常。

jstat还可以用来判断是否出现内存泄漏。

第1步：
在长时间运行的Java程序中，我们可以运行jstat命令连续获
取多行性能数据，并取这几行
数据中OU列（即已占用的老年代内存）的最小值
第2步
然后，我们每隔一段较长的时间重复一次上述操作，来获得
多组OU最小值。如果这些值呈上涨趋势。则说明该Java程序
的老年代内存己使用量在不断上涨，这意味着无法回收的对象
在不断增加，因此很有可能存在存泄漏。
```

### 3.4 jinfo
```markdown
基本情况
jinfo(Configuration Info for Java)
查看虚拟机配置参数信思，也可用于调整虚拟机的配置参数

在很多情况下，Java应用程序不会指定所有的Java虚拟机参数。
而此时，开发人员可能不知道
某一个具体的Java虚拟机参数的默认值。在这种情况下，可能
需要通过查找文档获取某个参数
的默认值。这个查找过程可能是非常艰难的。但有了 jinfo工具，
开发人员可以很方便地找到
Java虚拟机参数的当前值。

jinfo不仅可以查看运行时某一个Java虚拟机参数的实际取值，
甚至可以在运行时修改部分参
数，并使之立即生效。
但是，并非所有参数都支持动态修改。参数只有被标记
为manageable的flag可以被实时修改。其实，这个修改能力是
极其有限的。

java -XX: -PrintFlagslnitial 查看所有JVM参数启动的初始值
java -XX: +PrintFlagsFinal 查看所有JVM参数的最终值
java -XX: +PrintCommandLineflags 查看那些已经被用户
或者JVM设置过的详细的XX参数的名称和值
```
### 3.5 jmap
```markdown
jmap:导出内存映像文件&内存使用情况

jmap( JVM Memory Map):作用一方面是获取dump文件(堆转储
快照文件，二进制文件)，
它还可以获取目标Java进程的内存相关信息，包括Java堆各区
域的使用情况、堆中对象的统
计信息、类加载信息等
开发人员可以在控制台中输入命令“jmap -help”查阅jmap工具的
具体使用方式和一些标准选项配置
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210309083203844.png)

```markdown
一般来说，使用jmap指令生成dump文件的操作算得上是最常用
的jmap命令之一，将堆中所有
存活对象导出至一个文件之中。
Heap Dump又叫做堆存储文件，指一个Java进程在某个时间点
的内存快照。 Heap Dump在触
发内存快照的时候会保存此刻的信息如下


All Objects
Class, fields, primitive values and references
All Classes
Classloader, name, super class, static fields
Garbage Collection Roots
Objects defined to be reachable by the JVM
Thread Stacks and Local Variables
The call-stacks of threads at the moment of the snapshot, and per-frame
information about local objects
说明
1 通常在写 Heap Dump文件前会触发一次Full GC,所以 
heap dump文件里保存的都是Fu116C后留下的对象信息。
2 由于生成dump文件比较耗时，因此大家需要耐心等待，尤其是大
内存镜像生成dump文件则需要耗费更长的时间来完成
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210309085232272.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
手动方式
由于jmap将访问堆中的所有对象，为了保证在此过程中不被应用
线程干扰，map需要借助安
全点机制，让所有线程停留在不改变堆中数据的状态。也就是说，
由jmap导出的堆快照必定
是安全点位置的。这可能导致基于该堆快照的分析结果存在偏差。
举个例子，假设在编译生成的机器码中，某些对象的生命周期在
两个安全点之间，那么
live选项将无法探知到这些对象。
另外，如果某个线程长时间无法跑到安全点，jmap将一直等下去。
与前面讲的 jstat则不同,垃圾回收器会主动将 jstat所需要的摘要
数据保存至固定位置之中，而jstat只需直接读取
即可。


自动的方式
当程序发生OOM退出系统时，一些瞬时信息都随着程序的终止而消失，
而重现OOM问题往往比
较困难或者耗时。此时若能在OOM时，自动导出dump文件就显
得非常迫切。
这里介绍一种比较常用的取得堆快照文件的方法，即使用
-XX:+HeapDumpOnOutOfMemoryError:在程序发生OOM时，
导出应用程序的当前堆快照。
-XX: HeapDumpPath:可以指定堆快照的保存位置。
比如
-Xmx100m -XX: +HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=D: \m.hprof
```
```markdown
导出内存映射文件
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021030909011798.png)
```markdown
jmap：如何显示堆内存等功能
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210309091240703.png)

### 3.6 jhat:JDK自带堆分析工具
```markdown
基本情况
jhat(JVM Heap Analysis Tool)
Sun JDK提供的jhat命令与jmap命令搭配使用，用于分析jmap生成
的 heap dump文件（堆转
储快照）。jhat内置了一个微型的HTTP/HTML服务器，生成
dump文件的分析结果后，用户
可以在浏览器中查看分析结果（分析虚拟机转储快照信息）。
使用了jhat命令，就启动了一个http服务，
端口是7000,Bihttp://localhost:7000/就可以在浏览器里分析。
说明:jhat命令在JDK9、JDK10中已经被删除，官方建议用 
VisualVM代替。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021030909261365.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 3.7 jstack:打印JVM中线程快照
```markdown
jstack( JVM Stack Trace):用于生成虚拟机指定进程当前时刻
的线程快照（虚拟机堆栈跟踪
)。线程快照就是当前虚拟机内指定进程的每一条线程正在执行的
方法堆栈的集合

生成线程快照的作用：可用于定位线程出现长时间停顿的原因，如
线程间死锁、死循环、请求
外部资源导致的长时间等待等问题。这些都是导致线程长时间停
顿的常见原因。当线程出现停
顿时，就可以用jstack显示各个线程调用的堆栈情况。

在thread dump中，要留意下面几种状态
死锁，，Deadlock（重点关注）
等待资源， Waiting on condition（重点关注）
等待获取监视器， Waiting on monitor entry（重点关注）
阻塞， Blocked（重点关注）
执行中， Runnable
暂停， Suspended
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210309094710284.png)
### 3.8 jcmd:多功能命令行
```markdown
在JDK1.7以后，新增了一个命令行工具jcmd
它是一个多功能的工具，可以用来实现前面除jsat之外所有命令的功
能。比如：用它来导出堆、内存使用、查看]ava进程、导出线程信
息、执行GC、JVM运行时间等。

jcmd拥有jmap的大部分功能，并且在Oracle的官方网站上也推荐使
用jcmd命令代jmap命令
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210309100226947.png)
### 3.9 jstad:远程主机信息收集
```markdown
之前的指令只涉及到监控本机的Java应用程序，而在这些工具中，
一些监控工具也支持对远
程计算机的监控(如jps、 jstat)。为了启用远程监控，则需要配合
使用 jstatd工具。
命令 jstatd,是一个RMI服务端程序，它的作用相当于代理服务器，
建立本地计算机与远程监
控工具的通信。 jstatd服务器将本机的Java应用程序信息
传递到远程计算机
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210309100934262.png)
## 4 JVM监控及诊断工具-GUI
### 4.1 工具概述
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311204634975.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031120464417.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

### 4.2 JConsole

```markdown
基本概述
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311081406805.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
启动
```

```markdown
在jdk安装目录中找到jconsole.exe，双击该可执行文件就可以
打开DOS窗口，直接输入jconsole就可以了
```
```markdown
三种连接方式
```
```markdown
Local
使用JConsole连接一个正在本地系统运行的JVM，并且执
行程序的和运行JConsole的需要是同一个用户。JConsole使
用文件系统的授权通过RMI连接起链接到平台的MBean的服务器上。
这种从本地连接的监控能力只有Sun的JDK具有。
```
```markdown
注意：本地连接要求 启动jconsole的用户和运行当前程序的用户是
同一个用户
 
具体操作如下：
1 在DOS窗口中输入jconsole
```

```markdown
2 在控制台上填写相关信息
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311081905650.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
3 选择不安全的连接
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031108193771.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
4 进入控制台页面
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311082013204.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
Remote

使用下面的URL通过RMI连接器连接到一个JMX代理，
service:jmx:rmi:///jndi/rmi://hostName:portNum/jmxrmi。JConsole
为建立连接，需要在环境变量中设置mx.remote.credentials来指
定用户名和密码，从而进行授权。
```

```markdown
Advanced

使用一个特殊的URL连接JMX代理。一般情况使用自己定制的
连接器而不是RMI提供的连接器来连接JMX代理，或者是一个使
用JDK1.4的实现了JMX和JMX Rmote的应用
```

```markdown
主要作用
```

```markdown
1 概览
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311082419795.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
2 内存
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311082456412.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
3 根据线程检测死锁
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311082530300.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
4 线程
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311082557256.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
5 VM 概要
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311082628936.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 4.3 Visual VM
```markdown
基本概述

使用：
在jdk安装目录中找到jvisualvm.exe，然后双击执行即可
打开DOS窗口，输入jvisualvm就可以打开该软件
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311082904566.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
插件的安装
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311082956351.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311083011121.png)

```markdown
首先在IDEA中搜索VisualVM Launcher插件并安装：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311083046855.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
2 重启IDEA，然后配置该插件
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311083107490.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
3 使用两种方式来运行程序
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311083152997.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
4 运行效果
还是打开jvisualvm界面，只是不需要我们手动打开jvisualvm而已
```

```markdown
连接方式
```

```markdown
本地连接

监控本地Java进程的CPU、类、线程等
```

```markdown
远程连接
```

```markdown
1 确定远程服务器的ip地址
2 添加JMX（通过JMX技术具体监控远程服务器哪个Java进程）
3 修改bin/catalina.sh文件，连接远程的tomcat
4 在…/conf中添加jmxremote.access和jmxremote.password文件
5 将服务器地址改成公网ip地址
6 设置阿里云安全策略和防火墙策略
7 启动tomcat，查看tomcat启动日志和端口监听
8 JMX中输入端口号、用户名、密码登录
```
```markdown
主要功能
```

```markdown
1 生成/读取堆内存快照
```

```markdown
一 生成堆内存快照
1 方式1：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311083720135.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
2 方式2：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311083753643.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
注意：
生成堆内存快照如下图：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311083822447.png)

```markdown
这些快照存储在内存中，当线程停止的时候快照就会丢失，如果还想
利用，可以将快照进行另存为操作，如下图：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311083846669.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
二 装入堆内存快照
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311084719586.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
2 查看JVM参数和系统属性
```

```markdown
3 查看运行中的虚拟机进程
```

```markdown
4 生成/读取线程快照
```

```markdown
一 生成线程快照
1 方式1
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031108491627.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
2 方式2：
 
注意：
生成线程快照如下图：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311084956126.png)

```markdown
这些快照存储在内存中，当线程停止的时候快照就会丢失，
如果还想利用，可以将快照进行另存为操作，如下图：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311085019811.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
 二 装入线程快照
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311085150144.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
5 程序资源的实时监控
```
```markdown
6 其他功能

JMX代理连接
远程环境监控
CPU分析和内存分析
```
### 4.4 Eclipse MAT
```markdown
基本概述
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311090227145.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311090234977.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031109032527.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
获取堆dump文件
```

```markdown
dump文件内存
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311090358704.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
两点说明
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311090426755.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
获取dump文件
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311090452776.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311090506685.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
分析堆dump文件
```

```markdown
histogram
展示了各个类的实例数目以及这些实例的Shallow heap或
者Retained heap的总和
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311090634230.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311090656267.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
thread overview

查看系统中的Java线程
查看局部变量的信息
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311090814975.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311090821795.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
获得对象互相引用的关系
```

```markdown
with outgoing references
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311090943123.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311090953667.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
with incoming references
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311091018240.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031109102860.png)

```markdown
浅堆与深堆
```

```markdown
shallow heap
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311091116925.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
对象头代表根据类创建的对象的对象头，还有对象的大小不是可
能向8字节对齐，而是就向8字节对齐
```

```markdown
retained heap
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311091157179.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
注意：
当前深堆大小 = 当前对象的浅堆大小 + 对象中所包含对象的
深堆大小
```
```markdown
补充：对象实际大小
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311091312306.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
练习
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311091344126.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311091429318.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311091440846.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
案例分析：StudentTrace
```
```java
代码：
/**
 * 有一个学生浏览网页的记录程序，它将记录 每个学生访问过的网站地址。
 * 它由三个部分组成：Student、WebPage和StudentTrace三个类
 *
 *  -XX:+HeapDumpBeforeFullGC -XX:HeapDumpPath=c:\code\student.hprof
 * 
 */
public class StudentTrace {
    static List<WebPage> webpages = new ArrayList<WebPage>();

    public static void createWebPages() {
        for (int i = 0; i < 100; i++) {
            WebPage wp = new WebPage();
            wp.setUrl("http://www." + Integer.toString(i) + ".com");
            wp.setContent(Integer.toString(i));
            webpages.add(wp);
        }
    }

    public static void main(String[] args) {
        createWebPages();//创建了100个网页
        //创建3个学生对象
        Student st3 = new Student(3, "Tom");
        Student st5 = new Student(5, "Jerry");
        Student st7 = new Student(7, "Lily");

        for (int i = 0; i < webpages.size(); i++) {
            if (i % st3.getId() == 0)
                st3.visit(webpages.get(i));
            if (i % st5.getId() == 0)
                st5.visit(webpages.get(i));
            if (i % st7.getId() == 0)
                st7.visit(webpages.get(i));
        }
        webpages.clear();
        System.gc();
    }
}

class Student {
    private int id;
    private String name;
    private List<WebPage> history = new ArrayList<>();

    public Student(int id, String name) {
        super();
        this.id = id;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public List<WebPage> getHistory() {
        return history;
    }

    public void setHistory(List<WebPage> history) {
        this.history = history;
    }

    public void visit(WebPage wp) {
        if (wp != null) {
            history.add(wp);
        }
    }
}


class WebPage {
    private String url;
    private String content;

    public String getUrl() {
        return url;
    }

    public void setUrl(String url) {
        this.url = url;
    }

    public String getContent() {
        return content;
    }

    public void setContent(String content) {
        this.content = content;
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/202103110916200.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
结论：
elementData数组的浅堆是80个字节，而elementData数组中的所
有WebPage对象的深堆之和是1208个字节，所以加在一起就
是elementData数组的深堆之和，也就是1288个字节
解释：
我说“elementData数组的浅堆是80个字节”，其中15个对象一共
是60个字节，对象头8个字节，数组对象本身4个字节，这些的和
是72个字节，然后总和要是8的倍数，所以“elementData数组的浅
堆是80个字节”
我说“WebPage对象的深堆之和是1208个字节”，一共有15个对象，
其中0、21、42、63、84、35、70不仅仅是7的倍数，还是3或者5的
倍数，所以这几个数值对应的i不能计算在深堆之内，这15个对象中
大多数的深堆是152个字节，但是i是0和7的那两个深堆是144个
字节，所以(13*152+144*2)-(6*152+144)=1208，所以这也印证
了我上面的话，即“WebPage对象的深堆之和是1208个字节”
因此“elementData数组的浅堆80个字节”加上“WebPage对象的
深堆之和1208个字节”，正好是1288个字节，说明“elementData数
组的浅堆1288个字节”
```
```markdown
支配树
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311091714709.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311091725690.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
注意：
跟随我一起来理解如何从“对象引用图---》支配树”，首先需要理解
支配者（如果要到达对象B，毕竟经过对象A，那么对象A就是对
象B的支配者，可以想到支配者大于等于1），然后需要理解直接
支配者（在支配者中距离对象B最近的对象A就是对象B的直接支
配者，你要明白直接支配者不一定就是对象B的上一级，然后直
接支配者只有一个），然后还需要理解支配树是怎么画的，其
实支配树中的对象与对象之间的关系就是直接支配关系，也就
是上一级是下一级的直接支配者，只要按照这样的方式来作
图，肯定能从“对象引用图---》支配树”
 
在Eclipse MAT工具中如何查看支配树：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031109175781.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
Tomcat堆溢出分析
```
```markdown
说明
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311091851254.png)

```markdown
分析过程
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311091912701.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311091936112.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311091944973.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031109200171.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311092016196.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311092027114.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311092036632.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311092046852.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
支持使用OQL语言查询对象信息
```
```markdown
SELECT子句
FROM子句
WHERE子句
内置对象与方法
```
### 4.5 内存泄漏
#### 4.5.1 内存泄露的理解与分析
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210309153133892.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210309153155630.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)


![在这里插入图片描述](https://img-blog.csdnimg.cn/2021030915332716.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210309153346688.png)
#### 4.5.2 Java中内存泄露的8种情况
```markdown
1 静态集合类
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210310233207876.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
2 单例模式
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210310233900291.png)

```markdown
3 内部类持有外部类
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210310233930113.png)

```markdown
4 各种连接，如数据库连接、网络连接和IO连接等
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210310233956864.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
5 变量不合理的作用域
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311075001545.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
6 改变哈希值
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311075022795.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
 例1：
/**
 * 演示内存泄漏
 *

 * @create 14:43
 */
public class ChangeHashCode {
    public static void main(String[] args) {
        HashSet set = new HashSet();
        Person p1 = new Person(1001, "AA");
        Person p2 = new Person(1002, "BB");

        set.add(p1);
        set.add(p2);

        p1.name = "CC";//导致了内存的泄漏
        set.remove(p1); //删除失败

        System.out.println(set);

        set.add(new Person(1001, "CC"));
        System.out.println(set);

        set.add(new Person(1001, "AA"));
        System.out.println(set);

    }
}

class Person {
    int id;
    String name;

    public Person(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Person)) return false;

        Person person = (Person) o;

        if (id != person.id) return false;
        return name != null ? name.equals(person.name) : person.name == null;
    }

    @Override
    public int hashCode() {
        int result = id;
        result = 31 * result + (name != null ? name.hashCode() : 0);
        return result;
    }

    @Override
    public String toString() {
        return "Person{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }
}
例2：
/**
 * 演示内存泄漏

 * @create 14:47
 */
public class ChangeHashCode1 {
    public static void main(String[] args) {
        HashSet<Point> hs = new HashSet<Point>();
        Point cc = new Point();
        cc.setX(10);//hashCode = 41
        hs.add(cc);

        cc.setX(20);//hashCode = 51  此行为导致了内存的泄漏

        System.out.println("hs.remove = " + hs.remove(cc));//false
        hs.add(cc);
        System.out.println("hs.size = " + hs.size());//size = 2

        System.out.println(hs);
    }

}

class Point {
    int x;

    public int getX() {
        return x;
    }

    public void setX(int x) {
        this.x = x;
    }

    @Override
    public int hashCode() {
        final int prime = 31;
        int result = 1;
        result = prime * result + x;
        return result;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null) return false;
        if (getClass() != obj.getClass()) return false;
        Point other = (Point) obj;
        if (x != other.x) return false;
        return true;
    }

    @Override
    public String toString() {
        return "Point{" +
                "x=" + x +
                '}';
    }
}
```
```markdown
7 缓存泄露
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311075350531.png)

```java
 例子：
/**
 * 演示内存泄漏
 *

 * @create 14:53
 */
public class MapTest {
    static Map wMap = new WeakHashMap();
    static Map map = new HashMap();

    public static void main(String[] args) {
        init();
        testWeakHashMap();
        testHashMap();
    }

    public static void init() {
        String ref1 = new String("obejct1");
        String ref2 = new String("obejct2");
        String ref3 = new String("obejct3");
        String ref4 = new String("obejct4");
        wMap.put(ref1, "cacheObject1");
        wMap.put(ref2, "cacheObject2");
        map.put(ref3, "cacheObject3");
        map.put(ref4, "cacheObject4");
        System.out.println("String引用ref1，ref2，ref3，ref4 消失");

    }

    public static void testWeakHashMap() {

        System.out.println("WeakHashMap GC之前");
        for (Object o : wMap.entrySet()) {
            System.out.println(o);
        }
        try {
            System.gc();
            TimeUnit.SECONDS.sleep(5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("WeakHashMap GC之后");
        for (Object o : wMap.entrySet()) {
            System.out.println(o);
        }
    }

    public static void testHashMap() {
        System.out.println("HashMap GC之前");
        for (Object o : map.entrySet()) {
            System.out.println(o);
        }
        try {
            System.gc();
            TimeUnit.SECONDS.sleep(5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("HashMap GC之后");
        for (Object o : map.entrySet()) {
            System.out.println(o);
        }
    }

}
结果：
String引用ref1，ref2，ref3，ref4 消失
WeakHashMap GC之前
obejct2=cacheObject2
obejct1=cacheObject1
WeakHashMap GC之后
HashMap GC之前
obejct4=cacheObject4
obejct3=cacheObject3
Disconnected from the target VM, address: '127.0.0.1:51628', transport: 'socket'
HashMap GC之后
obejct4=cacheObject4
obejct3=cacheObject3
分析：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031107555986.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
8 监听器和回调
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311075636628.png)
#### 4.5.3 内存泄露案例分析
```markdown
案例代码
```
```java
代码：
public class Stack {
    private Object[] elements;
    private int size = 0;
    private static final int DEFAULT_INITIAL_CAPACITY = 16;

    public Stack() {
        elements = new Object[DEFAULT_INITIAL_CAPACITY];
    }

    public void push(Object e) { //入栈
        ensureCapacity();
        elements[size++] = e;
    }

    public Object pop() {
        if (size == 0)
            throw new EmptyStackException();
        Object result = elements[--size];
        elements[size] = null;
        return result;
    }

    private void ensureCapacity() {
        if (elements.length == size)
            elements = Arrays.copyOf(elements, 2 * size + 1);
    }
}
```

```markdown
分析
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311093115910.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311093140166.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/202103110932199.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311093228612.png)

```markdown
解决办法
```
```java
将代码中的pop()方法变成如下方法：
public Object pop() { //出栈
    if (size == 0)
        throw new EmptyStackException();
    return elements[--size];
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031109332219.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)


### 4.6 支持使用OQL语言查询对象信息
```markdown
SELECT子句
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311080316759.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
FROM子句
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311080442417.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
WHERE子句
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031108050719.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
内置对象与方法
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311080528123.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
(Java Process Status)
显示指定系统内所有的HotSpot虚拟机进程(查看虚拟机进程信息），
可用于查询正在
运行的虚拟机进程
说明：对于本地虚拟机进程来说，进程的本地虚拟机ID与操作系
统的进程ID是一致的，是唯一的。

-q:仅仅显示 LVMID(loca1 virtual machine id),即本地虚拟机唯一id。
不显示主类的名称等

-l:输出应用程序主类的全类名或如果进程执行的是jar包，则输
出ar完整路径

-m:输出虚拟机进程启动时传递给主类main()的参数

-v:列出虚拟机进程启动时的JVM参数。比如：-Xms20m -Xmx5m是
启动程序指定的jvm参数。

说明：以上参数可以综合使用

补充:eperfdata
如果某Java进程关闭了默认开启的UserPerfData参数（即使用参数
-XX:-UsePerfdata),那么jps命令(以及下面介绍的jstat)将
无法探知该Java进程。
```
### 4.7 JProfiler
```markdown
基本概述
```
```markdown
介绍
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311152855778.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
特点
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311152928402.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
主要功能
```

```markdown
1 方法调用
对方法调用的分析可以帮助您了解应用程序正在做什么，并找到
提高其性能的方法
```
```markdown
2 内存分配
通过分析堆上对象、引用链和垃圾收集能帮您修复内存泄露问题，
优化内存使用
```
```markdown
3 线程和锁
JProfiler提供多种针对线程和锁的分析视图助您发现多线程问题
```
```markdown
4 高级子系统
许多性能问题都发生在更高的语义级别上。例如，对于JDBC调用，
您可能希望找出执行最慢的SQL语句。JProfiler支持对这些子系
统进行集成分析
```
```markdown
安装与配置
```
```markdown
下载与安装
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031115334111.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
JProfiler中配置IDEA
```

```markdown
1 IDE Integrations
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311154007815.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
2 选择合适的IDE版本
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031115403285.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
3 开始集成
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311154052116.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
4 正式集成
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311154114250.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
5 集成成功
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311154141394.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
IDEA集成JProfiler
```

```markdown
一 安装JProfiler插件
方式1：在线安装
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031115424241.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)


```markdown
方式2、离线安装
首先下载插件：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311154302707.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
 准备离线安装：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311154319291.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
正式离线安装：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311154350961.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
注意：无论采用方式1还是方式2都需要重启IDEA
```
```markdown
二 将JProfiler配置到IDEA中
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031115445713.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
具体使用
```

```markdown
数据采集方式
instrumentation重构模式
Sampling抽样模式
推荐使用Sampling方式，足够用来分析OOM问题了
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311154614732.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
遥感监测 Telemetries
其中Telemetries就是遥感监测的意思
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311154654952.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
内存视图 Live Memory
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311154731488.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311154928853.png)

```markdown
注意：
All Objects后面的Size大小是浅堆大小
Record Objects在判断内存泄露的时候使用，可以通过观察
Telemetries中的Memory，如果里面出现垃圾回收之后的内
存占用逐步提高，这就有可能出现内存泄露问题，所以可以
使用Record Objects查看，但是该分析默认不开启，毕竟占
用CPU性能太多
```
```markdown
堆遍历 heap walker
如果通过内存视图 Live Memory已经分析出哪个类的对象不能进
行垃圾回收，并且有可能导致内存溢出，如果想进一步分析，我
们可以在该对象上点击右键，
选择Show Selection In Heap Walker，如下图：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311155921984.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
之后进行溯源，操作如下：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311155943635.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
查看结果，并根据结果去看对应的图表：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311160012583.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
以下是图表的展示情况：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311160138821.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
cpu视图 cpu views
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311160520229.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
具体使用：
1 记录方法统计信息
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311160558166.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)


```markdown
2 方法统计
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311160625563.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
3 具体分析
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311160643863.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
线程视图 threads
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311160712379.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
1 查看线程运行情况
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311160741639.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
2 新建线程dump文件
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311160801376.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
监视器&锁 Monitors&locks
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311160827754.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
案例分析
```
```markdown
案例1
```
```java
/**
 * 功能演示测试

 * @create 12:19
 */
public class JProfilerTest {
    public static void main(String[] args) {
        while (true){
            ArrayList list = new ArrayList();
            for (int i = 0; i < 500; i++) {
                Data data = new Data();
                list.add(data);
            }
            try {
                TimeUnit.MILLISECONDS.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
class Data{
    private int size = 10;
    private byte[] buffer = new byte[1024 * 1024];//1mb
    private String info = "hello,atguigu";
}
 
```

```markdown

```

```markdown
案例2
```

```java
例子：
public class MemoryLeak {

    public static void main(String[] args) {
        while (true) {
            ArrayList beanList = new ArrayList();
            for (int i = 0; i < 500; i++) {
                Bean data = new Bean();
                data.list.add(new byte[1024 * 10]);//10kb
                beanList.add(data);
            }
            try {
                TimeUnit.MILLISECONDS.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

}

class Bean {
    int size = 10;
    String info = "hello,atguigu";
    static ArrayList list = new ArrayList();
}

```
```markdown
解释：
我们通过JProfiler来看一下，如下：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311161151768.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
你可以看到内存一个劲的往上涨，但是就是没有下降的趋势，说
明这肯定有问题，过不了多久就会出现OOM，我们来到Live memory
中，先标记看一下到底是哪些对象在进行内存增长，等一小下看看
会不会触发垃圾回收，如果不触发的话，我们自己来触发垃圾回收，
之后观察哪些对象没有被回收掉，如下：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311161223162.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
我上面点击了Mark Current，发现有些对象在持续增长，然后
点击了一下Run GC，结果如下所示：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311161309843.png)

```markdown
可以看出byte[]没有被回收，说明它是有问题的，我们点击
Show Selection In Heap Walker，如下：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031116133188.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
然后看一下该对象被谁引用，如下：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311161349416.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
结果如下：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311161405174.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
可以看出byte[]来自于Bean类是的list中，并且这个list是
ArrayList类型的静态集合，所以找到了：
static ArrayList list = new ArrayList();
发现list是静态的，这不妥，因为我们的目的是while结束
之后Bean对象被回收，并且Bena对象中的所有字段都被回收，
但是list是静态的，那就是类的，众所周知，类变量随类而生，
随类而灭，因此每次我们往list中添加值，都是往同一个list中
添加值，这会造成list不断增大，并且不能回收，所以最
终会导致OOM
```
### 4.8 Arthas
```markdown
基本概述
```
```markdown
背景
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311161616468.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311161657233.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311161708170.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
概述
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311161728321.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
基于哪些工具开发而来
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311161747238.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
安装与使用
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311161948109.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311161956590.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311162007172.png)

```markdown
工程目录
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311162116874.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
启动
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311162145175.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
查看进程
jps
```

```markdown
查看日志
cat ~/logs/arthas/arthas.log
```
```markdown
查看帮助
java -jar arthas-boot.jar -h
```

```markdown
web console
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311162330805.png)

```markdown
退出
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311162347987.png)

```markdown
相关诊断指令
```

```markdown
基础指令
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311162429952.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
jvm相关
```

```markdown
dashboard
作用：当前系统的实时数据面板
```

```markdown
thread
作用：查看当前线程信息，查看线程的堆栈
```
```markdown
jvm
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031116255026.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
其他
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311162607183.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
class/classloader相关
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311162726985.png)

```markdown
sc
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311162801309.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
sm
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311162821606.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
jad
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311162837232.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311162915661.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
mc、redefine
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311162934841.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
classloader
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031116300412.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
monitor/watch/trace相关
```

```markdown
monitor
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311163105850.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
watch
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311163123757.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311163134433.png)

```markdown
trace
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031116321921.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
stack
tt
```

```markdown
其他
profiler/火焰图
options
```
### 4.9 Java Misssion Control
```markdown
历史
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031116350643.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
启动
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311163528945.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
概述
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311163544346.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
功能：实时监控JVM运行时的状态
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311163604787.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311163619379.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
Java Flight Recorder
```

```markdown
事件类型
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311163659198.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
启动方式
```

```markdown
方式1：使用-XX:StartFlightRecording=参数
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031116380129.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
方式2：使用jcmd的JFR.*子命令
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311163823884.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
方式3：JMC的JFR插件
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311163851945.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
具体使用：
1 启动飞行记录仪
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311163955727.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
2 启动飞行记录
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311164058896.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
3 正式启动
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031116412854.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311164148877.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311164221140.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
Java Flight Recorder 取样分析
```

```java
/**
 * -Xms600m -Xmx600m -XX:SurvivorRatio=8
  shkstart@126.com
 * @create 2020  21:12
 */
public class OOMTest {
    public static void main(String[] args) {
        ArrayList<Picture> list = new ArrayList<>();
        while(true){
            try {
                Thread.sleep(5);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            list.add(new Picture(new Random().nextInt(100 * 50)));
        }
    }
}

class Picture{
    private byte[] pixels;

    public Picture(int length) {
        this.pixels = new byte[length];
    }

    public byte[] getPixels() {
        return pixels;
    }

    public void setPixels(byte[] pixels) {
        this.pixels = pixels;
    }
}

```
```markdown
结果
```

```markdown
1 一般信息
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311164410832.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
2 内存
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311164431834.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
3 代码
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311164459725.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
4 线程
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311164525782.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 4.10 其他工具
```markdown
Flame Graphs（火焰图）
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311164834854.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311164859891.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
Tprofiler
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311165004966.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311165038128.png)

```markdown
Btrace
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311165104400.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
YourKit
JProbe
Spring Insight
```
## 5 JVM运行时参数
### 5.1 JVM参数选项
```markdown
JVM参数选项
```

```markdown
类型一：标准参数选项
比较稳定，后续版本基本不会变化
以-开头
```

```markdown
各种选项
直接在DOS窗口中运行java或者java -help可以看到所有的标准选项

-d32          使用 32 位数据模型 (如果可用)
-d64          使用 64 位数据模型 (如果可用)
-server       选择 "server" VM
               默认 VM 是 server.
 
-cp <目录和 zip/jar 文件的类搜索路径>
-classpath <目录和 zip/jar 文件的类搜索路径>
               用 ; 分隔的目录, JAR 档案
               和 ZIP 档案列表, 用于搜索类文件。
-D<名称>=<值>
               设置系统属性
-verbose:[class|gc|jni]
               启用详细输出
-version      输出产品版本并退出
-version:<值>
               警告: 此功能已过时, 将在
               未来发行版中删除。
               需要指定的版本才能运行
-showversion  输出产品版本并继续
-jre-restrict-search | -no-jre-restrict-search
               警告: 此功能已过时, 将在
               未来发行版中删除。
               在版本搜索中包括/排除用户专用 JRE
-? -help      输出此帮助消息
-X            输出非标准选项的帮助
-ea[:<packagename>...|:<classname>]
-enableassertions[:<packagename>...|:<classname>]
               按指定的粒度启用断言
-da[:<packagename>...|:<classname>]
-disableassertions[:<packagename>...|:<classname>]
               禁用具有指定粒度的断言
-esa | -enablesystemassertions
               启用系统断言
-dsa | -disablesystemassertions
               禁用系统断言
-agentlib:<libname>[=<选项>]
               加载本机代理库 <libname>, 例如 -agentlib:hprof
               另请参阅 -agentlib:jdwp=help 和 -agentlib:hprof=help
-agentpath:<pathname>[=<选项>]
               按完整路径名加载本机代理库
-javaagent:<jarpath>[=<选项>]
               加载 Java 编程语言代理, 请参阅 java.lang.instrument
-splash:<imagepath>
               使用指定的图像显示启动屏幕
```

```markdown
补充内容：-server与-client
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311170109466.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
对于以上第2点，我们可以打开DOS窗口，输入java -version就
可以看到64位机器上用的server模式，如下所示：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311171020673.png)

```markdown
类型二：-X参数选项
```

```markdown
特点
非标准化参数
功能还是比较稳定的。但官方说后续版本可能会变更
以-X开头
```

```markdown
各种选项
直接在DOS窗口中运行java -X命令可以看到所有的X选项
-Xmixed        混合模式执行 (默认)
-Xint             仅解释模式执行
-Xcomp        仅采用即时编译器模式
-Xbootclasspath:<用 ; 分隔的目录和 zip/jar 文件>
                   设置搜索路径以引导类和资源
-Xbootclasspath/a:<用 ; 分隔的目录和 zip/jar 文件>
                   附加在引导类路径末尾
-Xbootclasspath/p:<用 ; 分隔的目录和 zip/jar 文件>
                   置于引导类路径之前
-Xdiag            显示附加诊断消息
-Xnoclassgc       禁用类垃圾收集
-Xincgc           启用增量垃圾收集
-Xloggc:<file>    将 GC 状态记录在文件中 (带时间戳)
-Xbatch           禁用后台编译
-Xms<size>        设置初始 Java 堆大小
-Xmx<size>        设置最大 Java 堆大小
-Xss<size>        设置 Java 线程堆栈大小
-Xprof            输出 cpu 配置文件数据
-Xfuture          启用最严格的检查, 预期将来的默认值
-Xrs              减少 Java/VM 对操作系统信号的使用 (请参阅文档)
-Xcheck:jni       对 JNI 函数执行其他检查
-Xshare:off       不尝试使用共享类数据
-Xshare:auto      在可能的情况下使用共享类数据 (默认)
-Xshare:on        要求使用共享类数据, 否则将失败。
-XshowSettings    显示所有设置并继续
-XshowSettings:all
                   显示所有设置并继续
-XshowSettings:vm 显示所有与 vm 相关的设置并继续
-XshowSettings:properties
                   显示所有属性设置并继续
-XshowSettings:locale
                   显示所有与区域设置相关的设置并继续
 
-X 选项是非标准选项，如有更改，恕不另行通知
 

```

```markdown
JVM的JIT编译模式相关的选项
```

```markdown
-Xint
只使用解释器：所有字节码都被解释执行，这个模式的速度是很慢的
```
```markdown
-Xcomp
只使用编译器：所有字节码第一次使用就被编译成本地代码，
然后在执行
```
```markdown
-Xmixed
混合模式：这是默认模式，刚开始的时候使用解释器慢慢解释
执行，后来让JIT即时编译器根据程序运行的情况，有选择地将某
些热点代码提前编译并缓存在本地，在执行的时候效率就非常高了
```
```markdown
特别地
-Xmx -Xms -Xss属于XX参数？
单位：k/K、m/M、g/G
设置：-Xmx、-Xms最好设置成一样的值，避免扩容带来的损耗
```
```markdown
-Xms<size>     设置初始Java堆大小，等价于-XX:InitialHeapSize
```

```markdown
查看该参数值的时候，应该使用InitialHeapSize，
例如jinfo flag InitialHeapSize 进程id
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311171930661.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311171957730.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
-Xmx<size>       设置最大Java堆大小，等价于-XX:MaxHeapSize
```

```markdown
查看该参数值的时候，应该使用MaxHeapSize，
例如jinfo flag InitialHeapSize 进程id
等价证明：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311172906439.png)

```markdown
-Xss<size>         设置Java线程堆栈大小，
等价于-XX:ThreadStackSize
查看该参数值的时候，
应该使用ThreadStackSize，例如jinfo flag InitialHeapSize 进程id
```
```markdown
类型三：-XX参数选项
```
```markdown
特点
非标准化参数
使用的最多的参数类型
这类选项属于实验性，不稳定
以-XX开头
```

```markdown
作用
用于开发和调试JVM
```
```markdown
分类
```
```markdown
Boolean类型格式
-XX:+<option>  表示启用option属性
-XX:-<option>表示禁用option属性
```

```markdown
举例
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311173330637.png)

```markdown
说明：因为有的指令默认是开启的，所以可以使用-关闭
```
```markdown
非Boolean类型格式（key-value类型）
```
```markdown
子类型1：数值型格式-XX:<option>=<number>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311173415671.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
子类型2：非数值型格式-XX:<name>=<string>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311173434605.png)

```markdown
特别地
-XX:+PrintFlagsFinal
输出所有参数的名称和默认值
默认不包括Diagnostic和Experimental的参数
可以配合-XX:+UnlockDiagnosticVMOptions和-XX:UnlockExperimentalVMOptions使用
```
### 5.2 添加JVM参数选项
```markdown
Eclipse
```

```markdown
1、在空白处单击右键，选择Run As，
在选择Run Configurations……
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311174633956.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
2 设置虚拟机参数
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311174718402.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
IDEA
1 Edit Configurations…
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311174759310.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
2 设置虚拟机参数
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311174817821.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
运行jar包
java -Xms50m -Xmx50m -XX:+PrintGCDetails -XX:+PrintGYTimeStamps -jar demo.jar

这是在java -jar demo.jar中的java -jar之间添加了虚拟机配置信息
```
```markdown
通过Tomcat运行war包
Linux系统下可以在tomcat/bin/catalina.sh中添加类似如下配置：
JAVA_OPTS="-Xms512M -Xmx1024M"

Windows系统下载catalina.bat中添加类似如下配置：
set "JAVA_OPTS=-Xms512M -Xmx1024M"
```
```markdown
程序运行过程中
使用jinfo -flag <name>=<value> <pid>设置非Boolean类型参数
使用jinfo -flag [+|-]<name> <pid>设置Boolean类型参数
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311175102804.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 5.3 常用的JVM参数选项
```markdown
打印设置的XX选项及值
```

```markdown
-XX:+PrintCommandLineFlags
可以让程序运行前打印出用户手动设置或者JVM自动设置的XX选项
```
```markdown
-XX:+PrintFlagsInitial
表示打印出所有XX选项的默认值
```
```markdown
-XX:+PrintFlagsFinal
表示打印出XX选项在运行程序时生效的值

如果值的前面加上了:=，说明该值不是初始值，该值可能被jvm自
动改变了，也可能被我们设置的参数改变了，如下所示：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311175542776.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
-XX:+PrintVMOptions
打印JVM的参数
```

```markdown
堆、栈、方法区等内存大小设置
```
```markdown
栈
-Xss128k
等价于-XX:ThreadStackSize，设置每个线程的栈大小为128k
```

```markdown
堆内存
-Xms3550m  等价于-XX:InitialHeapSize，设置JVM初始堆内存为3500M
-Xmx3550m   等价于-XX:MaxHeapSize，设置JVM最大堆内存为3500M
-Xmn2g  
设置年轻代大小为2G，即等价于-XX:NewSize=2g -XX:MaxNewSize=2g，
也就是设置年轻代初始值和年轻代最大值都是2G
官方推荐配置为整个堆大小的3/8
-XX:NewSize=1024m  设置年轻代初始值为1024M
-XX:MaxNewSize=1024m  设置年轻代最大值为1024M

-XX:SurvivorRatio=8
设置年轻代中Eden区与一个Survivor区的比值，默认为8
只有显示使用Eden区和Survivor区的比例，才会让比例生效，
否则比例都会自动设置，至于其中的原因，请看下面的
-XX:+UseAdaptiveSizePolicy中的解释，最后推荐使用默认
打开的
-XX:+UseAdaptiveSizePolicy设置，并且不显示设置-XX:SurvivorRatio
-XX:+UseAdaptiveSizePolicy 自动选择各区大小比例，默认开启
1 分析
默认开启，将会导致Eden区和Survivor区的比例自动分配，因此也会
引起我们默认值-XX:SurvivorRatio=8失效，所以真实比例可能不是8，
比如可能是6等
2 如何设置Eden区和Survivor区的比例
-XX:SurvivorRatio=8
显示使用Eden区和Survivor区的比例，那就使用我自己的
没有显示使用Eden区和Survivor区的比例，无论打开或者关闭
-XX:+UseAdaptiveSizePolicy，都会自动设置Eden区和Survivor区的比例
 
结论：
只有显示使用Eden区和Survivor区的比例，才会让比例生效，
否则比例都会自动设置，最后推荐使用默认打开的
-XX:+UseAdaptiveSizePolicy设置，并且不显示设置
-XX:SurvivorRatio
 
-XX:NewRatio=2   设置老年代与年轻代（包括1个Eden区和
2个Survivor区）的比值，默认为2
根据实际情况进行设置，主要根据对象生命周期来进行分配，
如果对象生命周期很长，那么让老年代大一点，否则让新生代大一点
-XX:PretenureSizeThreadshold=1024 
设置让大于此阈值的对象直接分配在老年代，单位为字节
只对Serial、ParNew收集器有效
不好控制

-XX:MaxTenuringThreshold=15  
默认值为15
新生代每次MinorGC后，还存活的对象年龄+1，当对象的
年龄大于设置的这个值时就进入老年代
使用比较少，一般用默认值

-XX:+PrintTenuringDistribution
让JVM在每次MinorGC后打印出当前使用的Survivor中对象的
年龄分布
-XX:TargetSurvivorRatio  表示MinorGC结束后Survivor区域中占
用空间的期望比例
```

```markdown
方法区
永久代
-XX:PermSize=256m  设置永久代初始值为256M
-XX:MaxPermSize=256m  设置永久代最大值为256M
元空间
-XX:MetaspaceSize  初始空间大小
-XX:MaxMetaspaceSize  最大空间，默认没有限制
-XX:+UseCompressedOops  使用压缩对象指针
-XX:+UseCompressedClassPointers  使用压缩类指针
-XX:CompressedClassSpaceSize  设置Klass Metaspace的大小，默认1G

```
```markdown
直接内存
-XX:MaxDirectMemorySize  指定DirectMemory容量，若未指定，
则默认与Java堆最大值一样
```
```markdown
OutOfMemory相关的选项
-XX:+HeapDumpOnOutMemoryError  表示在内存出现OOM的时候，
生成Heap转储文件，以便后续分析，
-XX:+HeapDumpBeforeFullGC和-XX:+HeapDumpOnOutMemoryError
只能设置1个
-XX:+HeapDumpBeforeFullGC  表示在出现FullGC之前，
生成Heap转储文件，以便后续分析，
-XX:+HeapDumpBeforeFullGC和-XX:+HeapDumpOnOutMemoryError
只能设置1个，
请注意FullGC可能出现多次，那么dump文件也会生成多个
-XX:HeapDumpPath=<path>  指定heap转存文件的存储路径，
如果不指定，就会将dump文件放在当前目录中
-XX:OnOutOfMemoryError  指定一个可行性程序或者脚本的路径，
当发生OOM的时候，去执行这个脚本
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311181354913.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
垃圾收集器相关选项
```
```markdown
查看默认的垃圾回收器 
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311181626527.png)

```markdown
以上两种方式都可以查看默认使用的垃圾回收器，第一种方式更
加准备，但是需要程序的支持；第二种方式需要去尝试，如果使
用了，返回的值中有+号，否则就是-号
```
```markdown
Serial回收器 
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311181700536.png)

```markdown
Parnew回收器
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311181735367.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311181742549.png)

```markdown
根据下图可知，该回收器最终将会没有搭档，那就相当于被遗弃了
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311181830787.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
Parallel回收器
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311181855824.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
注意：
Parallel回收器主打吞吐量，而CMS和G1主打低延迟，如果主打吞
吐量，那么就不应该限制最大停顿时间，所以-XX:MaxGCPauseMills不
应该设置
-XX:MaxGCPauseMills中的调整堆大小通过默认开启的
-XX:+UseAdaptiveSizePolicy来实现
-XX:GCTimeRatio用来衡量吞吐量，并且和-XX:MaxGCPauseMills矛盾，
因此不会同时使用
```

```markdown
CMS回收器
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311182002872.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
-XX:ParallelCMSThreads和ParallelGCThreads有关系，
ParallelGCThreads在上面Parnew回收器中有提到
```

```markdown
补充参数
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311182200383.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
特别说明
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311182230621.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
G1回收器
如果使用G1垃圾收集器，不建议设置-Xmn和-XX:NewRatio，
毕竟可能影响G1的自动调节
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311182300918.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
Mixed GC调优参数
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311182400839.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
怎么选择垃圾收集器
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311182426479.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
GC日志相关选项
```
```markdown
常用参数
```
```markdown
-verbose:gc 
输出日志信息，默认输出的标准输出
可以独立使用
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311191619151.png)

```markdown
-XX:+PrintGC
等同于-verbose:gc表示打开简化的日志
可以独立使用
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311192138869.png)


```markdown
-XX:+PrintGCDetails
在发生垃圾回收时打印内存回收详细的日志，
并在进程退出时输出当前内存各区域的分配情况
可以独立使用
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311192249920.png)

```markdown
-XX:+PrintGCTimeStamps
程序启动到GC发生的时间秒数
不可以独立使用，需要配合-XX:+PrintGCDetails使用
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311192331913.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
-XX:+PrintGCDateStamps
输出GC发生时的时间戳（以日期的形式，
例如：2013-05-04T21:53:59.234+0800）
不可以独立使用，可以配合-XX:+PrintGCDetails使用
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311192426800.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
-XX:+PrintHeapAtGC
每一次GC前和GC后，都打印堆信息
可以独立使用
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311192511986.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
-XIoggc:<file>
把GC日志写入到一个文件中去，而不是打印到标准输出中
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311192537840.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
其他参数
-XX:TraceClassLoading  监控类的加载
-XX:PrintGCApplicationStoppedTime  打印GC时线程的停顿时间
-XX:+PrintGCApplicationConcurrentTime  垃圾收集之前打印出应用未中断的执行时间
-XX:+PrintReferenceGC  记录回收了多少种不同引用类型的引用
-XX:+PrintTenuringDistribution  让JVM在每次MinorGC后打印出当前使用的Survivor中对象的年龄分布
-XX:+UseGCLogFileRotation  启用GC日志文件的自动转储
-XX:NumberOfGCLogFiles=1    GC日志文件的循环数目
-XX:GCLogFileSize=1M  控制GC日志文件的大小

```

```markdown
其他参数
-XX:+DisableExplicitGC   禁用hotspot执行System.gc()，默认禁用
-XX:ReservedCodeCacheSize=<n>[g|m|k]、-XX:InitialCodeCacheSize=<n>[g|m|k]   指定代码缓存的大小
-XX:+UseCodeCacheFlushing  使用该参数让jvm放弃一些被编译的代码，避免代码缓存被占满时JVM切换到interpreted-only的情况
-XX:+DoEscapeAnalysis  开启逃逸分析
-XX:+UseBiasedLocking  开启偏向锁
-XX:+UseLargePages  开启使用大页面
-XX:+PrintTLAB   打印TLAB的使用情况
-XX:TLABSize  设置TLAB大小
```
### 5.4 通过Java代码获取JVM参数
![在这里插入图片描述](https://img-blog.csdnimg.cn/202103111937230.png)

```java
/**
 *
 * 监控我们的应用服务器的堆内存使用情况，设置一些阈值进行报警等处理
 *

 * @create 15:23
 */
public class MemoryMonitor {
    public static void main(String[] args) {
        MemoryMXBean memorymbean = ManagementFactory.getMemoryMXBean();
        MemoryUsage usage = memorymbean.getHeapMemoryUsage();
        System.out.println("INIT HEAP: " + usage.getInit() / 1024 / 1024 + "m");
        System.out.println("MAX HEAP: " + usage.getMax() / 1024 / 1024 + "m");
        System.out.println("USE HEAP: " + usage.getUsed() / 1024 / 1024 + "m");
        System.out.println("\nFull Information:");
        System.out.println("Heap Memory Usage: " + memorymbean.getHeapMemoryUsage());
        System.out.println("Non-Heap Memory Usage: " + memorymbean.getNonHeapMemoryUsage());

        System.out.println("=======================通过java来获取相关系统状态============================ ");
        System.out.println("当前堆内存大小totalMemory " + (int) Runtime.getRuntime().totalMemory() / 1024 / 1024 + "m");// 当前堆内存大小
        System.out.println("空闲堆内存大小freeMemory " + (int) Runtime.getRuntime().freeMemory() / 1024 / 1024 + "m");// 空闲堆内存大小
        System.out.println("最大可用总堆内存maxMemory " + Runtime.getRuntime().maxMemory() / 1024 / 1024 + "m");// 最大可用总堆内存大小

    }
}

```
```markdown
上篇通过Runtime获取
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311193827875.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
## 6 分析GC日志
### 6.1 GC日志参数
```markdown
-verbose:gc  输出gc日志信息，默认输出到标准输出
-XX:+PrintGC  输出GC日志。类似：-verbose:gc
-XX:+PrintGCDetails  在发生垃圾回收时打印内存回收相处
的日志，并在进程退出时输出当前内存各区域分配情况
-XX:+PrintGCTimeStamps  输出GC发生时的时间戳
-XX:+PrintGCDateStamps  输出GC发生时的时间戳
（以日期的形式，例如：2013-05-04T21:53:59.234+0800）
-XX:+PrintHeapAtGC   每一次GC前和GC后，都打印堆信息
-Xloggc:<file>  表示把GC日志写入到一个文件中去，而不是打
印到标准输出中
```
### 6.2 GC日志格式
```markdown
GC分类
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311194527128.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
新生代收集：当Eden区满的时候就会进行新生代收集，所以新
生代收集和S0区域和S1区域无关
老年代收集和新生代收集的关系：进行老年代收集之前会先进
行一次年轻代的垃圾收集，原因如下：一个比较大的对象无法放
入新生代，那它自然会往老年代去放，如果老年代也放不下，那
会先进行一次新生代的垃圾收集，之后尝试往新生代放，如
果还是放不下，才会进行老年代的垃圾收集，之后在往老年
代去放，这是一个过程，我来说明一下为什么需要往老年代
放，但是放不下，而进行新生代垃圾收集的原因，这是
因为新生代垃圾收集比老年代垃圾收集更加简单，这样
做可以节省性能
进行垃圾收集的时候，堆包含新生代、老年代、元空间/永久
代：可以看出Heap后面包含着新生代、老年代、元空间，
但是我们设置堆空间大小的时候设置的只是新生代、老年
代而已，元空间是分开设置的
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311194552714.png)

```markdown
哪些情况会触发Full GC：老年代空间不足、方法区空间不足、
显示调用System.gc()、Minior GC进入老年代的数据的平均
大小 大于 老年代的可用内存、大对象直接进入老年代，而
老年代的可用空间不足
```
```markdown
不同GC分类的GC细节
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

 * @create 17:19
 */
public class GCUseTest {
    public static void main(String[] args) {
        ArrayList<byte[]> list = new ArrayList<>();

        while(true){
            byte[] arr = new byte[1024 * 10];//10kb
            list.add(arr);
//            try {
//                Thread.sleep(5);
//            } catch (InterruptedException e) {
//                e.printStackTrace();
//            }
        }
    }
} 

```
```markdown
老年代使用CMS GC
GC设置方法：参数中使用-XX:+UseConcMarkSweepGC，说明
老年代使用CMS GC，同时年轻代也会触发对ParNew的使用，
因此添加该参数之后，新生代使用ParNew GC，而老年代使
用CMS GC，整体是并发垃圾收集，主打低延迟
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311194834968.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
打印出来的GC细节：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311194858297.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
新生代使用Serial GC
GC设置方法：参数中使用-XX:+UseSerialGC，说明新生代使
用Serial GC，同时老年代也会触发对Serial Old GC的使用，
因此添加该参数之后，新生代使用Serial GC，而老年代使
用Serial Old GC，整体是串行垃圾收集
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311194935204.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
 打印出来的GC细节：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311194953515.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
DefNew代表新生代使用Serial GC，然后Tenured代表老年
代使用Serial Old GC
```
```markdown
GC日志分类
```
```markdown
MinorGC
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311195056916.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
FullGC
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311195117911.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
GC日志结构剖析
```
```markdown
垃圾收集器
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311195215791.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
GC前后情况
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311195233921.png)

```markdown
GC时间
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311195254625.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
Minor GC 日志解析
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311201116574.png)

```markdown
2020-11-20T17:19:43.265-0800   添加-XX:+PrintGCDateStamps参数
日志打印时间 日期格式 如
2013-05-04T21:53:59.234+0800

0.822:  添加-XX:+PrintGCTimeStamps该参数
gc发生时，Java虚拟机启动以来经过的秒数


```
```markdown
[GC(Allocation Failure)    
发生了一次垃圾回收，这是一次Minior GC。它不区分新生
代还是老年代GC，括号里的内容是gc发生的原因，这里
的Allocation Failure的原因是新生代中没有足够区域能够存
放需要分配的数据而失败
```
```markdown
[PSYoungGen:76800K->8433K(89600K)

PSYoungGen：表示GC发生的区域，区域名称与使用的GC收集器
是密切相关的
Serial收集器：Default New Generation 显示Defnew
ParNew收集器：ParNew
Parallel Scanvenge收集器：PSYoung
老年代和新生代同理，也是和收集器名称相关
```
```markdown
76800K->8433K(89600K)：GC前该内存区域已使用容量->GC后
盖区域容量(该区域总容量)
如果是新生代，总容量则会显示整个新生代内存的9/10，
即eden+from/to区
如果是老年代，总容量则是全身内存大小，无变化
```
```markdown
76800K->8449K(294400K)
虽然本次是Minor GC，只会进行新生代的垃圾收集，但是也肯
定会打印堆中总容量相关信息


在显示完区域容量GC的情况之后，会接着显示整个堆内存区域
的GC情况：GC前堆内存已使用容量->GC后堆内存容量
（堆内存总容量），并且堆内存总容量 = 9/10 新生代 + 老年代，
然后堆内存总容量肯定小于初始化的内存大小
```
```markdown
,0.0088371
整个GC所花费的时间，单位是秒
```
```markdown
[Times：user=0.02 sys=0.01,real=0.01 secs]
user：指CPU工作在用户态所花费的时间
sys：指CPU工作在内核态所花费的时间
real：指在此次事件中所花费的总时间
```
```markdown
Full GC 日志解析
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311201831496.png)

```markdown
2020-11-20T17:19:43.794-0800  添加-XX:+PrintGCDateStamps参数
日志打印时间 日期格式 如
2013-05-04T21:53:59.234+0800

1.351 添加-XX:+PrintGCTimeStamps该参数
gc发生时，Java虚拟机启动以来经过的秒数

Full GC(Metadata GCThreshold)
括号中是gc发生的原因，原因：Metaspace区不够用了。
除此之外，还有另外两种情况会引起Full GC，如下：
1 Full GC(FErgonomics)
原因：JVM自适应调整导致的GC
2 Full GC（System）
原因：调用了System.gc()方法

[PSYoungGen: 100082K->0K(89600K)]
PSYoungGen：表示GC发生的区域，区域名称与使用的GC收集
器是密切相关的
Serial收集器：Default New Generation 显示DefNew
ParNew收集器：ParNew
Parallel Scanvenge收集器：PSYoungGen
老年代和新生代同理，也是和收集器名称相关

10082K->0K(89600K)：GC前该内存区域已使用容量->GC该区域
容量(该区域总容量)
如果是新生代，总容量会显示整个新生代内存的9/10，
即eden+from/to区
如果是老年代，总容量则是全部内存大小，无变化
```
```markdown
ParOldGen：32K->9638K(204800K)  老年代区域没有发生GC，
因此本次GC是metaspace引起的
10114K->9638K(294400K),  在显示完区域容量GC的情况
之后，会接着显示整个堆内存区域的GC情况：GC前堆内存
已使用容量->GC后堆内存容量（堆内存总容量），并且堆
内存总容量 = 9/10 新生代 + 老年代，然后堆内存总容量
肯定小于初始化的内存大小

[Meatspace:20158K->20156K(1067008K)],  metaspace GC 回收2K空间

```
### 6.3 GC日志分析工具
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311202612930.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
GCEasy
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311202655526.png)

```markdown
GCViewer
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031120272874.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311202737377.png)

```markdown
其他工具
GChisto
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311202810647.png)

```markdown
官网上没有下载的地方，需要自己从SVN上拉下来编译
不过这个工具似乎没怎么维护了，存在不少bug
```
```markdown
HPjmeter
工具很强大，但是只能打开由以下参数生成的GC log，
-verbose:gc -Xloggc:gc.log。添加其他参数生成的gc.log无
法打开
HPjmeter集成了以前的HPjtune功能，可以分析在HP机器上产生
的垃圾回收日志文件
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复jvm3
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)




