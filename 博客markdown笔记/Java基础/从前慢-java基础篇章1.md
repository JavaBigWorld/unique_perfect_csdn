# java基础篇章1
![在这里插入图片描述](https://img-blog.csdnimg.cn/6f1ac02d945143e1ab75b917014cdd65.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAdW5pcXVlX3BlcmZlY3Q=,size_19,color_FFFFFF,t_70,g_se,x_16#pic_center)


## 1.1 java三种平台
```markdown
Java语言：完全面向对象（java语言底层实际上是C++实现的。）
Java被分为三大块：
	J2SE：标准版（基础，要学java，必须先学习SE。基础
	语法+基础库）
	J2EE：企业版（专门为企业开发软件，为企业提供解决方案。
	例如：OA办公系统，保险行业的系统，金融行业的系统，
	医院系统....）
	J2ME：微型版（专门为微型设备做嵌入式开发的。）
```
## 1.2 JDK、JRE、JVM三者之间的关系？
~~~markdown
JDK：Java开发工具箱
JRE：Java运行环境
JVM：Java虚拟机
JDK包括JRE，JRE包括JVM
JVM是不能独立安装的。JDK和JRE都是可以独立安装的。
安装JDK的时候：JRE就自动安装了，同时JRE内部的JVM
也就自动安装了。
~~~
## 1.3 如河查看版本信息
~~~markdown
javac -version(查看编译器版本)
java -version(查看java虚拟机的版本)
~~~
## 1.4 Java的三个步骤
~~~markdown
编写/编译/解释,编译期间检查Java语法,如果出现错误,则
不会产生类文件,
解释期间则将类文件装进JVM中,解释成二进制文件,然
后机器才能识别运行.
~~~
## 1.5 如河运次java文件
~~~markdown
javac 绝对路径或相对路径
java  类名
例子:比如有一个aa.java存在于C:/downloads/aa.java,则
编译为javac  C:/downloads/aa.java,或者为相对路径.
解释为java aa,记住,必须切换到类文件的所在目录,不可以跟路径
~~~
## 1.6 java的配置
~~~markdown
path:需要配置到java,javac所在的目录,即为路径名/bin
补充:windows操作系统是如何搜索硬盘上某个命令的呢？
首先会从当前目录下搜索,当前目录搜索不到的话，会从环境变量path
指定的路径当中搜索某个命令,如果还找不到则报错
~~~
## 1.7 "java HelloWorld"的执行过程以及原理。
```markdown
D:\course\JavaProjects\02-JavaSE\chapter01>java HelloWorld
敲完回车，都发生了什么？？？？？
第一步：会先启动JVM（java虚拟机）

第二步：JVM启动之后，JVM会去启动“类加载器classloader”
类加载器的作用：加载类的。本质上类加载器负责去硬盘上
找“类”对应的“字节码”文件。
假设是“java HelloWorld”，那么类加载器会去硬盘上
搜索：HelloWorld.class文件。
假设是“java Test”，那么类加载器会去硬盘上搜
索：Test.class文件。
.......

第三步：
类加载器如果在硬盘上找不到对应的字节码文件，会报错，报什么错？
错误: 找不到或无法加载主类
类加载器如果在硬盘上找到了对应的字节码文件，类加载
器会将该字节码
文件装载到JVM当中，JVM启动“解释器”将字节码
解释为“101010000...”这种
二进制码，操作系统执行二进制码和硬件交互。

问题？？？？？
默认情况下，类加载器去硬盘上找“字节码”文件的时候，
默认从哪找？？？？
默认情况下类加载器（classloader）会从当前路径下找。

此处应该有疑问，你可以提出哪些问题？？？？
能不能给类加载器指定一个路径，让类加载器去指定的
路径下加载字节码文件。
答案：可以的。但是我们需要设置一个环境变量，叫做：classpath

classpath是一个环境变量，是给谁指路的？
答案：是给“类加载器”指路的。

classpath环境变量不属于windows操作系统，
classpath环境变量隶属于java。		

你一定要理解classpath环境变量的作用是什么？
是给类加载器指路的。
在没有配置环境变量classpath的时候，默认从当前路径下加载。
如果配置了环境变量classpath的话，就只能从指定的路径下加载了。

path JAVA_HOME classpath，这3个环境变量path需要
配置，后面两个暂时不配置。
```
## 1.8 关于C与Java的不同
~~~markdown
Java没有无符号的byte，short，int和long，这一点
和C语言有很大的
不同。因此unsigned int m是错误的变量声明
与C不同，Java不允许在声明数组中的方括号内指定
数组元素，如int a[12]；
与C不同，Java允许数组元素的个数为int型变量。如
int size=10; double number[]=new double[size];     
与C不同，Java允许二维数组的列元素的个数可以不相同
~~~
## 1.9 Java的三种注释
```markdown
// 单行注释

/*
多行注释
*/

/**
* javadoc注释：这里的注释信息可以自动被javadoc.exe
* 命令解析提取并生成到帮助文档当中。
*/
```
## 1.10 标识符
```markdown
规则1：标识符只能由数字、字母（包括中文）、下划线_、美
元符号$组成，
	不能含有其它符号。

规则2：标识符不能以数字开头

规则3：关键字不能做标识符。
例如：public class static void 这些蓝色的字体
都是关键字，关键字是不能做标识符的。

规则4：标识符是严格区分大小写的。大写A和小写a不一样。

规则5：标识符理论上是没有长度限制的。
```
### 1.10.1 标识符命名规范
```markdown
具体的命名规范是哪些？

规范1：见名知意（这个标识符在起名的时候，最好
一看这个单词就知道啥意思。）

规范2：遵循驼峰命名方式，什么是驼峰（一高一低，一高一低...）
驼峰有利于单词与单词之间很好的进行分隔
BiaoShiFuTest，这个很好，一眼就能看出来是4个单词。

规范3：类名、接口名有特殊要求
类名和接口名首字母大写，后面每个单词首字母大写。
	StudentTest、UserTest ，这是类名、接口名。

规范4：变量名、方法名有特殊要求
变量名和方法名首字母小写，后面每个单词首字母大写。
	nianLing（NianLing这样就不符合了。）
	mingZi（MingZi这样也不符合了。）

规范5：所有“常量”名：全部大写，并且单词和单词
之间采用下划线衔接。
USER_AGE ：用户年龄
MATH_PI：固定不变的常量3.1415926.....
```
## 1.11 关键字
```markdown
在SUN公司开发Java语言的时候，提前定义好了一些具
有特殊含义的单词，
这些单词全部小写，具有特殊含义，不能用作标识符。比如：
关键字：
public 
static
void 
class
byte
short
int
long 
float
double
boolean
char
true
false
if
while
for
private
protected
........
```
## 1.12 字面量
```java
字面量就是数据

10 100 123 ：整型
1.34 3.14 2.0：浮点型
true false ：布尔型
'a' '国'：字符型
"a" "abc"  "国" "中国"：字符串型

10：整数，是一个数字
"10"：它不是数字，是一个字符串，或者说，它属于“文字类”的。
性质完全不同，在计算机中的对应的二进制码也是完全不同的。
```
## 1.13 变量
```markdown
java中的变量必须先声明，再赋值才能访问（必须手动赋值。）
int k; System.out.println(k); 这样是不行的.
1 关于变量的一个分类（这个需要“死记硬背”。）
变量根据出现的位置进行划分：
在方法体当中声明的变量：局部变量。
在方法体之外，类体内声明的变量：成员变量。
2 注意：局部变量只在方法体当中有效，方法体执行结束该
变量的内存就释放了。
```
## 1.14  public class 跟 class的区别:
~~~markdown
1 一个java源文件当中可以定义多个class.
2 一个java源文件当中public的class不是必须的.
3 一个class会定义生成一个xxx.class字节码文件.
4 一个java源文件当中定义公开的类的话,只能有
一个,并且该类名称必须和java源文件名称一致.
5 每一个class当中都可以编写main方法,都可以
设定程序的入口,想执行B.class中的main方法:java B,
想执行X.class当中的main方法:java X.
注意:当在命令窗口中执行java Hello,那么要求Hello.class
当中必须要有主方法.没有主方法会出现运行阶段的错误.
~~~
```markdown
面试题
一个".java"源文件中是否可以包括多个类（不是内部类）？有
什么限制？
可以有多个类，但只能有一个 public 的类，并且 public 的类名
必须与文件名相一致。
```
## 1.15 数据类型
```markdown
java中规定，任何一个浮点型数据默认被当做double来处理。
在任何情况下，整数型的“字面量/数据”默认被当做int类型处理。
```
```markdown
第一种：基本数据类型
		基本数据类型又可以划分为4大类8小种：
			第一类：整数型
				byte,short,int,long （没有小数的）
			第二类：浮点型 
				float,double （带有小数的）
			第三类：布尔型
				boolean：只有两个值true和false，true表示真，false表示假
			第四类：字符型
				char：java中规定字符型字面量必须使用单引号括起来。属于文字。
		8小种：
			byte,short,int,long
			float,double
			boolean
			char
第二种：引用数据类型
		字符串型String属于引用数据类型。
		String字符串不属于基本数据类型范畴。
		java中除了基本数据类型之外，剩下的都是引用数据类型。
		类型			占用字节数量(byte)
		------------------------------------
		byte				1
		short			    2
		int				    4
		long				8
		float				4
		double			    8
		boolean		        1  (1byte的1或0，00000001(true)或00000000(false))
		char				2
```
## 1.16 八种基本数据类型详解
```markdown
1 关于数据类型详解
字符型：char
整数型：byte short int long
在java中有一条非常重要的结论，必须记住：
在任何情况下，整数型的“字面量/数据”默认被当做int类型处理。

第一个结论：当一个整数赋值给char类型变量的时候，会自
动转换成char字符型，
最终的结果是一个字符。
第二个结论：当一个整数没有超出byte short char的取值范
围的时候，
这个整数可以直接赋值给byte short char类型的变量。
浮点型：float double
关于java语言中的浮点型数据
如果用在银行方面或者说使用在财务方面，double
也是远远不够的，在java中提供了一种精度更高的类型，
这种类型专门
使用在财务软件方面：java.math.BigDecimal 
（不是基本数据类型，属于
引用数据类型。）
类型转换
1 小容量可以直接赋值给大容量，称为自动类型转换。
2 大容量不能直接赋值给小容量，需要使用强制类型转换符进行强转。
大容量转换成小容量，要想编译通过，必须加强制类型转换符，
进行强制类型转换。
底层是怎么进行强制类型转换的呢？
long类型100L：00000000 00000000 00000000 00000000 00000000 00000000 00000000 01100100
以上的long类型100L强转为int类型，会自动将“前面”的4个字节砍掉：00000000 00000000 00000000 01100100
但需要注意的是：加强制类型转换符之后，虽然编译通过了，但是运行
的时候可能会损失精度。

2 综合的看一下，在类型转换的时候需要遵循哪些规则？（笔试题）

第一条：八种基本数据类型中，除 boolean 类型不能转换，剩下七种类型之间都可以
进行转换；

第二条：如果整数型字面量没有超出 byte,short,char 的取值范围，可以直接将其赋
值给byte,short,char 类型的变量；

第三条：小容量向大容量转换称为自动类型转换，容量从小到大的排序为：
byte < short(char) < int < long < float < double，其中 short和 char 
都占用两个字节，但是char 可以表示更大的正整数；

第四条：大容量转换成小容量，称为强制类型转换，编写时必须添加“强制类型转换符”，
但运行时可能出现精度损失，谨慎使用；

第五条：byte,short,char 类型混合运算时，先各自转换成 int 类型再做运算；

第六条：多种数据类型混合运算，各自先转换成容量最大的那一种再做运算；
其中第五条不符合这个规则（例外）。
例子1：
char c1 = 'a';
byte b = 1;
short s = c1 + b; // 编译器不知道这个加法最后的结果是多少。只知道是int类型。
解析：c1先转换成int类型，b先转换成int类型，然后两者合起来是int类型，无法编译通过。
例子2：
long a = 10L;
char c = 'a';
short s = 100;
int i = 30;
// 错误: 不兼容的类型: 从long转换到int可能会有损失
int x = a + c + s + i;  // 计算结果是long类型
解析：c、s、i各自转换为容量最大的long类型，然后参与运算，结果是long类型，编译不通过。需改成
int x = (int)(a + c + s + i);
注意：java中规定，int类型和int类型最终的结果还是int类型。
int temp = 10 / 3; // 结果是：3
int temp2 = 1 / 2;结果是：0
练习题：
byte b1 = 1000; // 编译报错，因为1000已经超出范围了。
byte b2 = 20; // 可以
short s = 1000; // 可以
int c = 1000; // 可以
long d = c; // 可以
int e = d; // 编译报错
int f = 10 / 3; // 可以
long g = 10; // 可以
int h = g / 3; // 编译报错
long m = g / 3; // 可以
byte x = (byte)g / 3; // 编译报错
short y = (short)(g / 3); // 可以
short i = 10; // 可以
byte j = 5; // 可以
short k = i + j; // 编译报错
int n = i + j; // 可以
char cc = 'a'; // 可以
int o = cc + 100; // 197，cc 会先自动转换成int类型，再做运算
```
## 2.1 运算符
```markdown
算术运算符：
+求和 -相减 * 乘积 /商 %求余数（求模） ++自加1 --自减1
对于++运算符来说：
	可以出现在变量前，也可以出现在变量后。
	不管出现在变量前还是后，总之++执行结束之后，变量的值一定会自加1。
关系运算符：
	> >= < <= == != 
一定要记住一个规则：
		所有的关系运算符的运算结果都是布尔类型，
		不是true就是false，不可能是其他值。
	
	在java语言中:
		= ： 赋值运算符
		== ：关系运算符，判断是否相等。
*/
逻辑运算符：
&	逻辑与（可以翻译成并且）
|	逻辑或（可以翻译成或者）
!	逻辑非（取反）
&&	短路与
||	短路或
重点短路与&& 和 逻辑与 &有什么区别？
首先这两个运算符的运算结果没有任何区别，完全相同。
只不过“短路与&&”会发生短路现象。
什么是短路现象呢？
右边表达式不执行，这种现象叫做短路现象。
什么时候使用&&，什么时候使用& ？
从效率方面来说，&&比&的效率高一些。
因为逻辑与&不管第一个表达式结果是什么，第二个表达式一定会执行。
以后的开发中，短路与&&和逻辑与还是需要同时并存的。
大部分情况下都建议使用短路与&&
只有当既需要左边表达式执行，又需要右边表达式执行的时候，才会
选择逻辑与&。
问题：重点短路或||和 逻辑或|有什么区别？
短路或||会发生短路,即当左边的表达式结果是true的时候，右边的表达式不需要执行，此时会短路。
赋值运算符：
1、赋值运算符包括“基本赋值运算符”和“扩展赋值运算符”：基本的、扩展的。
2、基本赋值运算符？
=
3、扩展的赋值运算符？
+= -= *= /= %=
4、很重要的语法机制：
使用扩展赋值运算符的时候，永远都不会改变运算结果类型。
byte x = 100;
x += 1;
x自诞生以来是byte类型，那么x变量的类型永远都是byte。不会变。不管后面是多大的数字.
例子:
int i = 10;
研究：
i += 10 和 i = i + 10 真的是完全一样吗？
答案：不一样，只能说相似，其实本质上并不是完全相同。
i += 10 等同于：i = (int)(i + 1);
解析:第一种因为是i是int类型,所以后面无论与什么类型的数参与运算,其类型不会变.
第二种就会变,符合前面提到的规则.
三目运算符(条件运算符)：
	布尔表达式 ? 表达式1 : 表达式2
	执行原理是什么？
		布尔表达式的结果为true时，表达式1的执行结果作为整个表达式的结果。
		布尔表达式的结果为false时，表达式2的执行结果作为整个表达式的结果。
字符串连接运算符：
+ 运算符：
	1、+ 运算符在java语言中有两个作用。
		作用1：求和
		作用2：字符串拼接

	2、什么时候求和？什么时候进行字符串的拼接呢？
		当 + 运算符两边都是数字类型的时候，求和。
		当 + 运算符两边的“任意一边”是字符串类型，那么这个+会进行字符串拼接操作。

	3、一定要记住：字符串拼接完之后的结果还是一个字符串。
```
```markdown
面试题
1.8	用最有效率的方法算出 2 乘以 8 等于几?
2 << 3，
因为将一个数左移 n 位，就相当于乘以了 2 的 n 次方，那么，一个数乘以 8 只要
将其左移 3 位即可，而位运算 cpu 直接支持的，效率最高，所以，2 乘以 8 等于
几的最效率的方法是 2 << 3。
```
## 2.2 控制语句
```markdown
选择语句/循环语句/转向语句
```
### 2.2.1  选择语句(分支语句)
```markdown
if语句
switch语句
```
#### 2.2.1.1  if语句
```java
第一种写法：
			int a = 100;
			int b = 200;
			if(布尔表达式){
				java语句;
				java语句;
			}
第二种写法：
			if(布尔表达式){  // 分支1
				java语句;     
			}else{            // 分支2
				java语句;
			}
if(布尔表达式1){ // 分支1
				java语句;
			}else if(布尔表达式2){ // 分支2
				java语句;
			}else if(布尔表达式3){
				java语句;
			}else if(布尔表达式4){
				java语句;
			}....
第四种写法：
			if(布尔表达式1){ // 分支1
				java语句;
			}else if(布尔表达式2){ // 分支2
				java语句;
			}else if(布尔表达式3){
				java语句;
			}else if(布尔表达式4){
				java语句;
			}else{
				java语句; // 以上条件没有一个成立的。这个else就执行了。
			}
注意：
		1、对于if语句来说，在任何情况下只能有1个分支执行，不可能
			存在2个或者更多个分支执行。if语句中只要有1个分支执行了，
			整个if语句就结束了。（对于1个完整的if语句来说的。）
		
		2、以上4种语法机制中，凡是带有else分支的，一定可以保证会有
		一个分支执行。以上4种当中，第一种和第三种没有else分支，这样
		的语句可能会导致最后一个分支都不执行。第二种和第四种肯定会有
		1个分支执行。

		3、当分支当中“java语句;”只有1条，那么大括号{}可以省略，但为了可读性，最好不要省略。
```
#### 2.2.1.2 switch语句
```markdown
switch(值){
		case 值1:
			java语句;
			java语句;...
			break;
		case 值2:
			java语句;
			java语句;...
			break;
		case 值3:
			java语句;
			java语句;...
			break;
		default:
			java语句;
		}
以上是一个完整的switch语句：
其中：break;语句不是必须的。default分支也不是必须的。

switch语句支持的值有哪些？
支持int类型以及String类型。
但一定要注意JDK的版本，JDK8之前不支持String类型，只支持int。
在JDK8之后，switch语句开始支持字符串String类型。

// 分析这个程序是否能够编译通过？
// switch只支持int和String类型。
// 错误: 不兼容的类型: 从long转换到int可能会有损失
	long x = 100L;
	switch(x){
	
	}
switch语句本质上是只支持int和String，但是byte,short,char也可以
使用在switch语句当中，因为byte short char可以进行自动类
型转换,转换为int类型。

switch语句中“值”与“值1”、“值2”比较的时候会使用“==”进行比较。

switch语句的执行原理
拿“值”与“值1”进行比较，如果相同，则执行该分支中的java语句，
然后遇到"break;"语句，switch语句就结束了。

如果“值”与“值1”不相等，会继续拿“值”与“值2”进行比较，如果相同，
则执行该分支中的java语句，然后遇到break;语句，switch结束。

注意：如果分支执行了，但是分支最后没有“break;”，此时会发生case
穿透现象。

所有的case都没有匹配成功，那么最后default分支会执行。
```
### 2.2.2  循环语句
```markdown
for循环
while循环
do..while..循环
```
#### 2.2.2.1 for循环
```markdown
for(初始化表达式; 条件表达式; 更新表达式){
			循环体; // 循环体由java语句构成
			java语句;
			java语句;
			java语句;
			java语句;
			....
		}
注意：
第一：初始化表达式最先执行，并且在整个循环中只执行一次。
第二：条件表达式结果必须是一个布尔类型，也就是：true或false
执行原理：
先执行初始化表达式，并且初始化表达式只执行1次。
然后判断条件表达式的结果，如果条件表达式结果为true，
则执行循环体。
循环体结束之后，执行更新表达式。
更新完之后，再判断条件表达式的结果，
如果还是true，继续执行循环体。

直到更新表达式执行结束之后，再次判断条件时，条件为false，
for循环终止。
```
#### 2.2.2.2 while语句
```markdown
while(布尔表达式){
				循环体;
			}
执行原理：
判断布尔表达式的结果，如果为true就执行循环体，
循环体结束之后，再次判断布尔表达式的结果，如果
还是true，继续执行循环体，直到布尔表达式结果
为false，while循环结束。
```
#### 2.2.2.3 do..while..循环
```markdown
do {
			循环体;
}while(布尔表达式);
注意：do..while循环最后的时候别漏掉“分号”
执行原理：
先执行循环体当中的代码，执行一次循环体之后，
判断布尔表达式的结果，如果为true，则继续执行
循环体，如果为false循环结束。

对于do..while循环来说，循环体至少执行1次。循环体的执行次
数是：1~n次。
对于while循环来说，循环体执行次数是：0~n次。
```	
```java
//面试题
//在 JAVA 中如何跳出当前的多重嵌套循环？
/*
Java 中，要想跳出多重循环，可以在外面的循环语句前定义一个标号，然后在里层循环体的代码中使用带有标号的 break 语句，即可跳出外层循环。例如，
*/
public class test {
public static void main(String[] args) {
	
//		ok:
//		for(int i=0;i<10;i++){
//			for(int j=0;j<10;j++){
//				System.out.println("i= "+ i + "j=" + j); if(j == 5) break ok;
//			}
//		}
/*
另外，我个人通常并不使用标号这种方式，而是让外层的循环条件表达式的结果可以受到里层循环体代码的控制，例如，要在二维数组中查找到某个数字。
*/
	int arr[][] = {{1,2,3},{4,5,6,7},{9}};

	boolean found = false;

	for(int i=0;i<arr.length && !found;i++){

		for(int j=0;j<arr[i].length;j++){
			System.out.println("i=" + i +","+ "j=" + j); 
			if(arr[i][j] == 5){
				found = true;
				break;

			}

		}

	}

}
}

```
### 2.2.3 转向语句
```markdown
break
continue
return
```
#### 2.2.3.1  break
```markdown
第一个位置：switch语句当中，用来终止switch语句的执行。
用在switch语句当中，防止case穿透现象，用来终止switch。

第二个位置：break;语句用在循环语句当中，用来终止循环的执行。
用在for当中
用在while当中
用在do....while..当中。

break;语句的执行并不会让整个方法结束，break;语句主要是用
来终止离它最近
的那个循环语句。
for(int i = 0; i < 10; i++){
		if(i == 5){
			// break;语句会让离它最近的循环终止结束掉。
			break; // break;终止的不是if，不是针对if的，而是针对离它最近的循环。
		}
		System.out.println("i = " + i); // 0 1 2 3 4
	}
```
#### 2.2.3.2  continue
```markdown
终止当前"本次"循环，直接进入下一次循环继续执行。
for(){
			if(){ // 当这个条件成立时，执行continue语句
				continue; //当这个continue语句执行时，continue下面的代码不执行，直接进入下一次循环执行。
			}
			// 以上的continue一旦执行，以下代码不执行，直接执行更新表达式。
			code1;
			code2;
			code3;
			code4;
		}
```
## 3.1 方法
```markdown
1 方法怎么定义，语法机制是什么？

	[修饰符列表] 返回值类型 方法名(形式参数列表){
		方法体; 
	}
	
返回值类型	
第一：当一个方法执行结束不返回任何值的时候，返回值
类型也不能空白，必须写上void关键字。所以void表示该
方法执行结束后不返回任何结果。

第二：如果返回值类型“不是void”，那么你在方法体执行结
束的时候必须使用
"return 值;"这样的语句来完成“值”的返回，如果没有“return 值;”这样
的语句
那么编译器会报错。
return 值; 这样的语句作用是什么？作用是“返回值”，返回方法的执行结果。

第三：只要有“return”关键字的语句执行，当前方法必然结束。
return只要执行，当前所在的方法结束，记住：不是整个程序结束。

第四：如果返回值类型是void，那么在方法体当中不能有“return 值;”这样的
语句。但是可以有“return;”语句。这个语句“return;”的作用就是用来终止当前
方法的。
public static void divide(int a, int b){
	return; // 用来终止这个方法的
}

第五：除了void之外，剩下的都必须有“return 值;”这样的语句。

方法名

1 方法名要见名知意。（驼峰命名方式）
方法名在标识符命名规范当中，要求首字母小写，后面每个单词首字母大写。
只要是合法的标识符就行。

2 形式参数列表
简称：形参
注意：形式参数列表中的每一个参数都是“局部变量”，方法结束之后内存释放。
形参的个数是：0~N个。
形参有多个的话使用“逗号,”隔开。逗号是英文的。
形参的数据类型起决定性作用，形参对应的变量名是随意的。

方法体：
由Java语句构成。java语句以“;”结尾。


注意:
1 在方法调用的时候，什么时候“类名.”是可以省略的。什么时候不能省略？
a()方法调用b()方法的时候，a和b方法都在同一个类中，“类名.”可以
省略。如果不在同一个类中“类名.”不能省略。
2 break;语句和return;语句有什么区别？
不是一个级别。
break;用来终止switch和离它最近的循环。
return;用来终止离它最近的一个方法。
```
### 3.1.1  方法重载
```markdown
什么时候代码会发生方法重载？	 
条件1：在同一个类当中
条件2：方法名相同
条件3：参数列表不同
参数的个数不同算不同
参数的类型不同算不同
参数的顺序不同算不同

只要同时满足以上3个条件，那么我们可以认定方法和方法之间发生了
重载机制。

方法重载和方法的“返回值类型”无关。
方法重载和方法的“修饰符列表”无关。

在java语言中，是怎么进行方法区分的呢？
首先java编译器会通过方法名进行区分。
但是在java语言中允许方法名相同的情况出现。
如果方法名相同的情况下，编译器会通过方法的参数类型进
行方法的区分。
```

```java
public class OverloadTest03{
public static void main(String[] args){
	m1();
	m1(100);

	m2(10, 3.14);
	m2(3.14, 10);

	m3(100);
	m3(3.14);

}

public static void m1(){
	System.out.println("m1无参数的执行！");
}

// 这个方法的参数个数和上面的方法的参数个数不同。
public static void m1(int a){
	System.out.println("m1有一个int参数执行！");
}

public static void m2(int x, double y){
	System.out.println("m2(int x, double y)");
}

// 参数的顺序不同，也算不同。
public static void m2(double y, int x){
	System.out.println("m2(double y, int x)");	
}

public static void m3(int x){
	System.out.println("m3(int x)");
}

// 参数的类型不同。
public static void m3(double d){
	System.out.println("m3(double d)");
}

//分析：以下两个方法有没有发生重载？
// 编译器报错了，不是重载，这是重复了：呵呵。
/*
public static void m4(int a, int b){

}
public static void m4(int x, int y){

}
*/

// 这两个方法有没有发生重载呢？
// 这不是重载，这是方法重复了。
/*
public static int m5(){
	return 1;
}
public static double m5(){
	return 1.0;
}
*/

//这两个方法重载了吗？
// 这个方法没有修饰符列表
// 这不是重载，是重复了。
/*
void m6(){

}
// 这个有修饰符列表
public static void m6(){

}
*/

}

class MyClass{
// 不在同一个类当中，不能叫做方法重载。
public static void m1(int x, int y){

}
}
```
### 3.1.2  方法递归
```markdown
方法递归？
1 什么是方法递归？
方法自己调用自己，这就是方法递归。

2 当递归时程序没有结束条件，一定会发生：
栈内存溢出错误：StackOverflowError
所以：递归必须要有结束条件。

JVM发生错误之后只有一个结果，就是退出JVM。

3 递归假设是有结束条件的，就一定不会发生栈内存溢出错误吗？
假设这个结束条件是对的，是合法的，递归有的时候也会出现
栈内存溢出错误。
因为有可能递归的太深，栈内存不够了。因为一直在压栈。

4 在实际的开发中，不建议轻易的选择递归，能用for循环while循
环代替的，尽量使用循环来做。因为循环的效率高，耗费的内存少。
递归耗费的内存比较大，另外递归的使用不当，会导致JVM死掉。


5 在实际的开发中，假设有一天你真正的遇到了：StackOverflowError
你怎么解决这个问题，可以谈一下你的思路吗？
我来谈一下我的个人思路：
首先第一步：
先检查递归的结束条件对不对。如果递归结束条件不对，
必须对条件进一步修改，直到正确为止。

第二步：假设递归条件没问题，怎么办？
这个时候需要手动的调整JVM的栈内存初始化大小。
可以将栈内存的空间调大点。（可以调整大一些。）

第三步：调整了大小，如果运行时还是出现这个错误，
没办法，只能继续扩大栈的内存大小。

(java -X)这个可以查看调整堆栈大小的参数
```
```java
// 使用递归的方式计算N的阶乘
// 5的阶乘：5 * 4 * 3 * 2 * 1

// 用递归的方式实现一个。
// 使用for循环的方式实现一个。
public class RecursionTest04{

	public static void main(String[] args){
		int n = 5;
		int jieGuo = jieCheng(n);
		System.out.println(jieGuo); // 120

		System.out.println(jieCheng2(5));
	}

	public static int jieCheng2(int n){
		int result = 1;
		for(int i = 2; i <= n; i++){
			result *= i;
		}
		return result;
	}

	public static int jieCheng(int n){
		// 5 * 4 * 3 * 2 * 1
		if(n == 1){
			return 1;
		}
		/*
		int result = n * jieCheng(n - 1);
		return result;
		*/

		return n * jieCheng(n - 1);
	}
}
```
## 4.1 类跟对象
```markdown
1 当我们采用面向对象的方式贯穿整个系统的话，涉及到三个术语：
OOA：面向对象分析
OOD：面向对象设计
OOP：面向对象编程
整个软件开发的过程，都是采用OO进行贯穿的。

实现一个软件的过程：
分析(A) --> 设计(D) --> 编程(P)
2 面向对象包括三大特征
封装
继承
多态

3 什么是类？
类就是一个模板：类中描述的是所有对象的“共同特征信息”
对象就是通过类创建出的个体。

这几个术语你需要自己能够阐述出来：
类：不存在的，人类大脑思考总结一个模板（这个模板当中描
述了共同特征。）
对象：实际存在的个体。
实例：对象还有另一个名字叫做实例。
实例化：通过类这个模板创建对象的过程，叫做：实例化。
抽象：多个对象具有共同特征，进行思考总结抽取共同特征的过程。

类 --【实例化】--> 对象(实例)
对象 --【抽象】--> 类


类的定义
	[修饰符列表] class 类名 {
		//类体 = 属性 + 方法
		// 属性在代码上以“变量”的形式存在（描述状态）
		// 方法描述动作/行为
	}

	注意：修饰符列表可以省略。
4 关于编译的过程
按说应该先编译XueSheng.java，然后再编译XueShengTest.java
但是对于编译器来说，编译XueShengTest.java文件的时候，会自动
找XueSheng.class.
第一种方式：	
javac XueSheng.java
javac XueShengTest.java
第二种方式：
javac XueShengTest.java
第三种方式：
javac *.java

5 在语法级别上是怎么完成对象创建的呢？
类名 变量名 = new 类名();
这样就完成了对象的创建。

6 什么是实例变量？
对象又被称为实例。
实例变量实际上就是：对象级别的变量,也叫成员变量。
注意：对于成员变量来说，没有手动赋值时，系统默认赋值。

类型				默认值
---------------------
byte				0
short			0
int				0
long				0L
float				0.0F
double			0.0
boolean			false
char				\u0000
引用数据类型	null
null是一个java关键字，全部小写，表示空。是引用类型的默认值。
7、对象和引用的区别？
对象是通过new出来的，在堆内存中存储。
引用是：是变量，并且该变量中保存了内存地址指向了堆内存当中
的对象的。
```
### 4.1.1 构造方法
```markdown
1、什么是构造方法，有什么用？
构造方法是一个比较特殊的方法，通过构造方法可以完成对象的创建，
以及实例变量的初始化。换句话说：构造方法是用来创建对象，并且
同时给对象的属性赋值。（注意：实例变量没有手动赋值的时候，系统
会赋默认值。）

2、重点（需要记忆）：当一个类没有提供任何构造方法，系统会默认提供
一个无参数的构造方法。（而这个构造方法被称为缺省构造器。）

3、调用构造方法怎么调用呢？
使用哪个运算符呢？
使用new运算符来调用构造方法。
语法格式：
new 构造方法名(实际参数列表);

4、构造方法的语法结构是？

[修饰符列表] 构造方法名(形式参数列表){
构造方法体;
通常在构造方法体当中给属性赋值，完成属性的初始化。
}

注意：
第一：构造方法名和类名必须一致。
第三：构造方法不需要指定返回值类型，也不能写void，写上void
表示普通方法，就不是构造方法了。

5.构造方法支持方法重载吗？
构造方法是支持方法重载的。
在一个类当中构造方法可以有多个。
并且所有的构造方法名字都是一样的。

6.实例变量在类加载是初始化吗？实例变量在什么时候初始化？
不是，实例变量是在构造方法执行的过程中完成初始化的，完成
赋值的。
```
```java
/*
	构造方法
		1、什么是构造方法，有什么用？
			构造方法是一个比较特殊的方法，通过构造方法可以完成对象的创建，
			以及实例变量的初始化。换句话说：构造方法是用来创建对象，并且
			同时给对象的属性赋值。（注意：实例变量没有手动赋值的时候，系统
			会赋默认值。）

		2、重点（需要记忆）：当一个类没有提供任何构造方法，系统会默认提供
		一个无参数的构造方法。（而这个构造方法被称为缺省构造器。）

		3、调用构造方法怎么调用呢？
			使用哪个运算符呢？
				使用new运算符来调用构造方法。
				语法格式：
					new 构造方法名(实际参数列表);
		
		4、构造方法的语法结构是？

			[修饰符列表] 构造方法名(形式参数列表){
				构造方法体;
				通常在构造方法体当中给属性赋值，完成属性的初始化。
			}

			注意：
				第一：修饰符列表目前统一写：public。千万不要写public static。

				第二：构造方法名和类名必须一致。

				第三：构造方法不需要指定返回值类型，也不能写void，写上void
				表示普通方法，就不是构造方法了。

			普通方法的语法结构是？
				[修饰符列表] 返回值类型 方法名(形式参数列表){
					方法体;
				}
*/
public class ConstructorTest01{
	public static void main(String[] args){

		// 调用Student类的无参数构造方法
		new Student();

		// 调用普通方法
		ConstructorTest01.doSome();
		doSome();

		// 创建Student类型的对象
		Student s1 = new Student();

		// 输出“引用”
		//只要输出结果不是null，说明这个对象一定是创建完成了。
		// 此处的输出结果大家目前是看不懂的，后期再说。
		System.out.println(s1); //Student@54bedef2

		// 这是调用另一个有参数的构造方法。
		Student s3 = new Student(100);
		System.out.println(s3); //Student@5caf905d
	}

	public static void doSome(){
		System.out.println("do some!!!!");
	}
}
```
### 4.1.2 封装
```markdown
1 封装的代码实现两步：

第一步：属性私有化

第二步：1个属性对外提供两个set和get方法。外部程序只能
通过set方法修改，只能通过get方法读取，
可以在set方法中设立关卡来保证数据的安全性。

在强调一下：
set和get方法都是实例方法，不能带static。
不带static的方法称为实例方法，实例方法的调用必须先new对象。

set和get方法写的时候有严格的规范要求：（大家要按照规矩来）
set方法长这个样子：
public void set+属性名首字母大写(1个参数){
	xxx = 1个参数;
}
get方法长这个样子：
public 返回值类型 get+属性名首字母大写(无参){
	return xxx;
}
```
```java
/*
	Person表示人类：
		每一个人都有年龄这样的属性。
		年龄age，int类型。
	
	我这里先不使用封装机制，分析程序存在什么缺点？
		Person类的age属性对外暴露，可以在外部程序中随意访问，导致了不安全。
	
	怎么解决这个问题？
		封装。
*/

// 这是没有封装的Person。
/*
public class Person{

	// 实例变量(属性)
	int age; //age属性是暴露的，在外部程序中可以随意访问。导致了不安全。

}
*/

// 尝试封装一下
// 不再对外暴露复杂的数据，封装起来
// 对外只提供简单的操作入口。
// 优点：第一数据安全了。第二调用者也方便了。
public class Person{
	// private 表示私有的，被这个关键字修饰之后，该数据只能在本类中访问。
	// 出了这个类，age属性就无法访问了。私有的。
	private int age; // 每一个人年龄值不同，对象级别的属性。

	// 对外提供简单的访问入口(电视机的遥控器就相当于是电视机的访问入口，简单明了。)
	// 外部程序只能通过调用以下的代码来完成访问
	// 思考：你应该对外提供几个访问入口?
	// 思考：这些操作入口是否应该是方法呢？
	// 写一个方法专门来完成读。(get)
	// 写一个方法专门来完成写。(set)
	// get和set方法应该带有static，还是不应该有static,get和set方法应该定义为实例方法吗？
	// get读年龄，set改年龄，这个读和改都是操作的一个对象的年龄。（没有对象何来年龄）
	// 封装的第二步：对外提供公开的set方法和get方法作为操作入口。并且都不带static。都是实例方法。
	/*
		[修饰符列表] 返回值类型 方法名(形式参数列表){
		}

		注意：
			java开发规范中有要求，set方法和get方法要满足以下格式。
				get方法的要求：
					public 返回值类型 get+属性名首字母大写(无参){
						return xxx;
					}
				set方法的要求：
					public void set+属性名首字母大写(有1个参数){
						xxx = 参数;
					}
			
			大家尽量按照java规范中要求的格式提供set和get方法。
			如果不按照这个规范格式来，那么你的程序将不是一个通用的程序。

	*/
	// get方法
	public int getAge(){
		return age;
	}

	// set方法
	public void setAge(int nianLing){
		// 能不能在这个位置上设置关卡！！！！
		if(nianLing < 0 || nianLing > 150){
			System.out.println("对不起，年龄值不合法，请重新赋值！");
			return; //直接终止程序的执行。
		}
		//程序能够执行到这里，说明年龄一定是合法的。
		age = nianLing;
	}

}

```
### 4.1.3 static关键字
```markdown
1 static翻译为“静态”
2 所有static关键字修饰的都是类相关的，类级别的。
3 所有static修饰的，都是采用“类名.”的方式访问。
4 static修饰的变量：静态变量
5 static修饰的方法：静态方法
变量的分类：
变量根据声明的位置进行划分：
在方法体当中声明的变量叫做：局部变量。
在方法体外声明的变量叫做：成员变量。

成员变量又可以分为：
实例变量
静态变量

什么时候变量声明为实例的，什么时候声明为静态的？
如果这个类型的所有对象的某个属性值都是一样的，
不建议定义为实例变量，浪费内存空间。建议定义
为类级别特征，定义为静态变量，在方法区中只保留
一份，节省内存开销。
```
### 4.1.4 静态代码块（特殊的时机：类加载时机。）
```markdown
static {
		java语句;
		java语句;
	}
1 static静态代码块在什么时候执行呢？
类加载时执行。并且只执行一次。
注意：静态代码块在类加载时执行，并且在main方法执行之前执行。

2 静态代码块一般是按照自上而下的顺序执行。
具体的业务：
项目经理说了：大家注意了，所有我们编写的程序中，只要是类
加载了，请记录一下
类加载的日志信息（在哪年哪月哪日几时几分几秒，哪个类加载
到JVM当中了）。
思考：这些记录日志的代码写到哪里呢？
写到静态代码块当中。
```
### 4.1.5 实例语句块(对象构建时机)
```markdown
1 实例语句在类加载是并没有执行。
2 实例语句语法？
{
	java语句;
	java语句;
	java语句;
}
3 实例语句块在什么时候执行？
只要是构造方法执行，必然在构造方法执行之前，自动执行“实例
语句块”中的代码。
实际上这也是SUN公司为java程序员准备一个特殊的时机，叫做
对象构建时机。有多少个
new 对象,就有多少个实例语句块执行.
```
### 4.1.6 静态代码块跟实例代码块的执行顺序
```java
//判断以下程序的执行顺序
public class CodeOrder{

// 静态代码块
static{
	System.out.println("A");
}

// 入口
// A X Y C B Z
public static void main(String[] args){
	System.out.println("Y");
	new CodeOrder();
	System.out.println("Z");
}

// 构造方法
public CodeOrder(){
	System.out.println("B");
}

// 实例语句块
{
	System.out.println("C");
}

// 静态代码块
static {
	System.out.println("X");
}

}
```
### 4.1.7 访问变量跟方法
```markdown
实例的：一定需要使用“引用.”来访问。

静态的：
建议使用“类名.”来访问，但使用“引用.”也行（不建议使用"引用."）。
静态的如果使用“引用.”来访问会让程序员产生困惑：程序
员以为是实例的呢。

结论：
空指针异常只有在什么情况下才会发生呢?
只有在“空引用”访问“实例”相关的，都会出现空指针异常。
例子: 
static String country = "中国";
Chinese c1 = null;
// 分析这里会不会出现空指针异常？
// 不会出现空指针异常。
// 因为静态变量不需要对象的存在。
// 实际上以下的代码在运行的时候，还是：System.out.println(Chinese.country);
System.out.println(c1.country);
```

```markdown
面试题1（静态变量和实例变量的区别？）
在语法定义上的区别：静态变量前要加 static 关键字，
而实例变量前则不加。
在程序运行时的区别：实例变量属于某个对象的属性，
必须创建了实例对象，其中的实例变量才会被分配空间，
才能使用这个实例变量。静态变量不属于某个实例对象，
而是属于类，所以也称为类变量，只要程序加载了类的字
节码，不用创建任何实例对象，静态变量就会被分配
空间，静态变量就可以被使用了。总之，实例变量必须
创建对象后才可以通过这个对象来使用，静态变量
则可以直接使用类名来引用。
例如，对于下面的程序，无论创建多少个实例对象，
永远都只分配了一个 staticVar 变量，并且每创建
一个实例对象，这个 staticVar 就会加 1；但是，每创
建一个实例对象，就会分配一个 instanceVar，即可
能分配多个 instanceVar，并且每个 instanceVar 的
值都只自加了 1 次。
```
```java
public class VariantTest

{

public static int staticVar = 0;
public int instanceVar = 0; 
public VariantTest()
{

	staticVar++; instanceVar++;
	System.out.println(“staticVar=” + staticVar + ”,instanceVar=” + instanceVar);

}

}
```
```markdown
面试题2（是否可以从一个 static 方法内部发出对非 static 方法的调用？）
不可以。因为非 static 方法是要与对象关联在一起的，必须创建一个对象后，才
可以在该对象上进行方法调用，而 static 方法调用时不需要创建对象，可以直接
调用。也就是说，当一个 static 方法被调用时，可能还没有创建任何实例对象，
如果从一个 static 方法中发出对非 static 方法的调用，那个非 static 方法是关联
到哪个对象上的呢？这个逻辑无法成立，所以，不可以一个 static 方法内部发出
对非 static 方法的调用。
```
### 4.1.8 this关键字
```markdown
this：
1、this
1.1 this是一个关键字，是一个引用，保存内存地址指向
自身,是一个特殊的变量,存在于对象内部,
即在堆内存中,不是存在于栈内存中。
1.2 this可以使用在实例方法中，也可以使用在构造方法中。
1.3 this出现在实例方法中其实代表的是当前对象。
1.4 this不能使用在静态方法中。
1.5 this. 大部分情况下可以省略，但是用来区分局部变量
和实例变量的时候不能省略.也就是说除了上述情况,编译器
会自动添加this.来补充省略的this.
1.6 this() 这种语法只能出现在构造方法第一行，表示当
前构造方法调用本类其他的
构造方法，目的是代码复用。
场景:
需求：
1 定义一个日期类，可以表示年月日信息。
2 需求中要求：
	如果调用无参数构造方法，默认创建的日期为：1970年1月1日。
	当然，除了调用无参数构造方法之外，也可以调用有
	参数的构造方法来创建日期对象。
```
### 4.1.9 类属性方法访问大总结
```markdown
在同一类中,如果是静态方法的话,则可以调用静态变量或
者静态方法,但是不可以调用实例变量跟实例方法.
除非new对象再来访问实例变量跟实例方法.在同一
类中,如果是实例方法的话,则可以调用实例变量,实例
方法,静态变量,静态方法.
```
### 4.1.10  继承
```markdown
继承的作用：
基本作用：子类继承父类，代码可以得到复
用。（这个不是重要的作用，是基本作用。）
主要(重要)作用：因为有了继承关系，才有了后期
的方法覆盖和多态机制。

继承的相关特性
1 B类继承A类，则称A类为超类(superclass)、父类、基类，
B类则称为子类(subclass)、派生类、扩展类。
class A{}
class B extends A{}

2 虽然 java中不支持多继承，但有的时候会产生间接继承的效果，
例如：class C extends B，class B extends A，也就
是说，C 直接继承 B，
其实 C 还间接继承 A。
子类继承父类之后，能使用子类对象直接调用父类方法吗？
可以，因为子类继承了父类之后，这个方法就属于子类了。
当然可以使用子类对象来调用。实际运行的时候,是调用子
类自己的东西,而不是父类的
东西.

3 java 中规定，子类继承父类，除构造方法不能继承之外，剩
下都可以继承。
但是私有的属性无法在子类中直接访问。(父类中private修饰
的不能在子类中
直接访问。可以通过间接的手段来访问。通过get方法来访问)

4 java 中的类没有显示的继承任何类，则默认继承 Object类，
Object类是 
java 语言提供的根类（老祖宗类），也就是说，一个
对象与生俱来就有 
Object类型中所有的特征。

5 继承也存在一些缺点，例如：CreditAccount 类继
承 Account 类会导致它
们之间的耦合度非常高，Account 类发生改变之后会马上
影响到 CreditAccount 类
```
### 4.1.11 方法覆盖
```markdown
方法覆盖又叫做：方法重写（重新编写），英语单词
叫做：Override、Overwrite，都可以。
什么时候考虑使用方法覆盖？
父类中的方法无法满足子类的业务需求，子类有必要
对继承过来的方法进行覆盖。

什么条件满足的时候构成方法覆盖？
第一：有继承关系的两个类
第二：具有相同方法名、返回值类型、形式参数列表
注意:对于返回值类型是基本数据类型来说，必须一致。
对于返回值类型是引用数据类型来说，重写之后返回值类
型可以变的更小（但意义不大，实际开发中没人这样写。）。
第三：访问权限不能更低。
第四：抛出异常不能更多。

这里还有几个注意事项：
注意1：方法覆盖只是针对于方法，和属性无关。
注意2：私有方法无法覆盖。
注意3：构造方法不能被继承，所以构造方法也不能被覆盖。
注意4：方法覆盖只是针对于“实例方法”，“静态方法覆盖”没有意义。
关于Object类中toString()方法的覆盖？
toString()方法存在的作用就是：将java对象转换成字符串形式。
大多数的java类toString()方法都是需要覆盖的。因为
Object类中提供的toString()
方法输出的是一个java对象的内存地址。


方法重载和方法覆盖有什么区别？

方法重载发生在同一个类当中。

方法覆盖是发生在具有继承关系的父子类之间。

方法重载是一个类中，方法名相同，参数列表不同。

方法覆盖是具有继承关系的父子类，并且重写之后的方法
必须和之前的方法一致：
方法名一致、参数列表一致、返回值类型一致。
```
```java
public class OverrideTest02{
public static void main(String[] args){
	Bird b = new Bird();
	b.move();
	b.sing(1000); //Animal sing....

	Cat c = new Cat();
	c.move();
}
}

class Animal{
public void move(){
	System.out.println("动物在移动！");
}

public void sing(int i){
	System.out.println("Animal sing....");
}
}

class Bird extends Animal{

// 对move方法进行方法覆盖，方法重写，override
// 最好将父类中的方法原封不动的复制过来。（不建议手动编写）
// 方法覆盖，就是将继承过来的那个方法给覆盖掉了。继承过来的方法没了。
public void move(){
	System.out.println("鸟儿在飞翔！！！");
}

//protected表示受保护的。没有public开放。
// 错误：正在尝试分配更低的访问权限; 以前为public
/*
protected void move(){
	System.out.println("鸟儿在飞翔！！！");
}
*/

//错误：被覆盖的方法未抛出Exception
/*
public void move() throws Exception{
	System.out.println("鸟儿在飞翔！！！");
}
*/

// 分析：这个sing()和父类中的sing(int i)有没有构成方法覆盖呢？
// 没有，原因是，这两个方法根本就是两个完全不同的方法。
// 可以说这两个方法构成了方法重载吗？可以。因为子类有继承父类public void sing(int i){代码};这个方法,两者构成重载.
public void sing(){
	System.out.println("Bird sing.....");
}
}

class Cat extends Animal{

// 方法重写
public void move(){
	System.out.println("猫在走猫步！！！");
}
}
```
### 4.1.12 多态
```markdown
向上转型和向下转型的概念。
向上转型：子--->父 (upcasting)
又被称为自动类型转换：Animal a = new Cat();

向下转型：父--->子 (downcasting)
又被称为强制类型转换：Cat c = (Cat)a; 需要
添加强制类型转换符。
什么时候需要向下转型？
需要调用或者执行子类对象中特有的方法。
必须进行向下转型，才可以调用。
向下转型有风险吗？
容易出现ClassCastException（类型转换异常）
怎么避免这个风险？
instanceof运算符，可以在程序运行阶段动态的判断某
个引用指向的对象是否为某一种类型。

不管是向上转型还是向下转型，首先他们之间必须有继承关
系，这样编译器就不会报错。

什么是多态。
多种形态，多种状态，编译和运行有两个不同的状态。
编译期叫做静态绑定。
运行期叫做动态绑定。
Animal a = new Cat();
a.move();
// 编译的时候编译器发现a的类型是Animal，所以
编译器会去Animal类中找move()方法
// 找到了，绑定，编译通过。但是运行的时候和底层
堆内存当中的实际对象有关
// 真正执行的时候会自动调用“堆内存中真实对象”的相关方法。



多态在开发中的作用是：
降低程序的耦合度，提高程序的扩展力。

	public class Master{
		public void feed(Dog d){}
		public void feed(Cat c){}
	}
以上的代码中表示：Master和Dog以及Cat的关系很紧密（耦合度高）。导致扩展力很差。

	public class Master{
		public void feed(Pet pet){
			pet.eat();
		}
	}
以上的代表中表示：Master和Dog以及Cat的关系就脱离了，Master关注的是Pet类。
这样Master和Dog以及Cat的耦合度就降低了，提高了软件的扩展性。

面向对象的三大特征：
封装、继承、多态
有了封装，有了这种整体的概念之后。
对象和对象之间产生了继承。
有了继承之后，才有了方法的覆盖和多态。

认识一个软件开发原则：
七大原则最基本的原则：OCP（对扩展开放，对修改关闭）
目的是：降低程序耦合度，提高程序扩展力。
面向抽象编程，不建议面向具体编程。
```
```java
public class Test01{

public static void main(String[] args){


	// 代码可以这样写吗？
	/*
		1、Animal和Cat之间有继承关系吗？有的。
		2、Animal是父类，Cat是子类。
		3、Cat is a Animal，这句话能不能说通？能。
		4、经过测试得知java中支持这样的一个语法：
			父类型的引用允许指向子类型的对象。
			Animal a2 = new Cat();
			a2就是父类型的引用。
			new Cat()是一个子类型的对象。
			允许a2这个父类型引用指向子类型的对象。
	*/
	Animal a2 = new Cat();

	// 调用a2的move()方法
	/*
		什么是多态？
			多种形态，多种状态。
		分析：a2.move();
			java程序分为编译阶段和运行阶段。
			先来分析编译阶段：
				对于编译器来说，编译器只知道a2的类型是Animal，
				所以编译器在检查语法的时候，会去Animal.class
				字节码文件中找move()方法，找到了，绑定上move()
				方法，编译通过，静态绑定成功。（编译阶段属于静态绑定。）
			再来分析运行阶段：
				运行阶段的时候，实际上在堆内存中创建的java对象是
				Cat对象，所以move的时候，真正参与move的对象是一只猫，
				所以运行阶段会动态执行Cat对象的move()方法。这个过程
				属于运行阶段绑定。（运行阶段绑定属于动态绑定。）

		多态表示多种形态：
			编译的时候一种形态。
			运行的时候另一种形态。
	*/
	a2.move(); //cat走猫步！
	
======================================================================
	Animal a5 = new Cat(); // 底层对象是一只猫。

	// 分析这个程序能否编译和运行呢？
	// 分析程序一定要分析编译阶段的静态绑定和运行阶段的动态绑定。
	// 只有编译通过的代码才能运行。没有编译，根本轮不到运行。
	// 错误: 找不到符号
	// why??? 因为编译器只知道a5的类型是Animal，去Animal.class文件中找catchMouse()方法
	// 结果没有找到，所以静态绑定失败，编译报错。无法运行。（语法不合法。）
	//a5.catchMouse(); 
	
	// 假设代码写到了这里，我非要调用catchMouse()方法怎么办？
	// 这个时候就必须使用“向下转型”了。（强制类型转换）
	// 以下这行代码为啥没报错？？？？
	// 因为a5是Animal类型，转成Cat，Animal和Cat之间存在继承关系。所以没报错。
	Cat x = (Cat)a5;
	x.catchMouse(); //猫正在抓老鼠！！！！

	// 向下转型有风险吗？
	Animal a6 = new Bird(); //表面上a6是一个Animal，运行的时候实际上是一只鸟儿。
	/*
		分析以下程序，编译报错还是运行报错？？？
			编译器检测到a6这个引用是Animal类型，
			而Animal和Cat之间存在继承关系，所以可以向下转型。
			编译没毛病。

			运行阶段，堆内存实际创建的对象是：Bird对象。
			在实际运行过程中，拿着Bird对象转换成Cat对象
			就不行了。因为Bird和Cat之间没有继承关系。
		
		运行时出现异常，这个异常和空指针异常一样非常重要，也非常经典：
			java.lang.ClassCastException：类型转换异常。
		
		java.lang.NullPointerException：空指针异常。这个也非常重要。
	*/
	//Cat y = (Cat)a6;
	//y.catchMouse();

	// 怎么避免ClassCastException异常的发生？？？
	/*	
		新的内容，运算符：
			instanceof （运行阶段动态判断）
		第一：instanceof可以在运行阶段动态判断引用指向的对象的类型。
		第二：instanceof的语法：
			(引用 instanceof 类型)
		第三：instanceof运算符的运算结果只能是：true/false
		第四：c是一个引用，c变量保存了内存地址指向了堆中的对象。
			假设(c instanceof Cat)为true表示:
				c引用指向的堆内存中的java对象是一个Cat。
			假设(c instanceof Cat)为false表示:
				c引用指向的堆内存中的java对象不是一个Cat。
		
		程序员要养成一个好习惯：
			任何时候，任何地点，对类型进行向下转型时，一定要使用
			instanceof 运算符进行判断。（java规范中要求的。）
			这样可以很好的避免：ClassCastException
	*/
	System.out.println(a6 instanceof Cat); //false

	if(a6 instanceof Cat){ // 如果a6是一只Cat
		Cat y = (Cat)a6;  // 再进行强制类型转换
		y.catchMouse();
	}
}
}
```
### 4.1.13 调试向下转型
```java
public class Animal {
public static void main(String[] args) {
	Animal animal = new Dog();
	Dog dog = (Dog)animal;
	System.out.println(dog instanceof Animal); //true
	System.out.println(dog instanceof Dog); //true

	
	
	//Exception in thread "main" java.lang.ClassCastException: Animal cannot be cast to Dog
	//at Animal.main(Animal.java:10)
	//Animal animal = new Animal();
	//Dog dog = (Dog) animal;
	
	//注意:向下转型之前必须先向上转型,否则会报错.
	//Animal animal = new Dog();
	//Dog dog = (Dog)animal;不会报错,因为运行的时候animal是dog类型.
	//Animal animal = new Animal();Dog dog = (Dog) animal;报错,因为运行的时候animal是
	//Animal,不是Dog,所以会报错.

}
}

class Dog extends Animal {

}
```
### 4.1.14 supper
```markdown
super能出现在实例方法和构造方法中。

super的语法是：“super.”、“super()”

super不能使用在静态方法中。

super. 大部分情况下是可以省略的。

super.什么时候不能省略呢？
父类和子类中有同名属性，或者说有同样的方法，
想在子类中访问父类的，super. 不能省略。

super() 只能出现在构造方法第一行，通过当前的构造
方法去调用“父类”中
的构造方法，目的是：创建子类对象的时候，先初始化父类型特征。

super的使用：
super.属性名				【访问父类的属性】
super.方法名(实参)		【访问父类的方法】
super(实参)					【调用父类的构造方法】

注意：在构造方法执行过程中一连串调用了父类的构造方法，
父类的构造方法又继续向下调用它的父类的构造方法，但是实际上
对象只创建了一个。
super 不是引用。super也不保存内存地址，super也不
指向任何对象。
super 只是代表当前对象内部的那一块父类型的特征。
System.out.println(super); //报错super不是引用
```
#### 4.1.14.1 supper之间的执行顺序
```java
/*
判断程序的输出结果
1
3
6
5
4

在java语言中不管是是new什么对象，最后老祖宗的Object类的无参数构造方法
一定会执行。（Object类的无参数构造方法是处于“栈顶部”）

栈顶的特点：
	最后调用，但是最先执行结束。
	后进先出原则。

大家要注意：
	以后写代码的时候，一个类的无参数构造方法还是建议大家手动的写出来。
	如果无参数构造方法丢失的话，可能会影响到“子类对象的构建”。

*/
public class SuperTest02{
public static void main(String[] args){
	new C();

}
}

/*
class Object{
public Object(){	
}
}
*/

class A extends Object{
public A(){
	System.out.println("1"); //1
}
}

class B extends A{
public B(){
	System.out.println("2"); //2
}
public B(String name){
	super();
	System.out.println("3"); // 3
}
}

class C extends B{
public C(){ // 这个是最先调用的。但是最后结束。
	this("zhangsan");
	System.out.println("4");//4
}
public C(String name){
	this(name, 20);
	System.out.println("5");//5
}
public C(String name, int age){
	super(name);
	System.out.println("6");//6
}
}
```
#### 4.1.14.2 super跟this的比较
```markdown
1、super是一个关键字，全部小写。
2、super和this对比着学习。
	this:
		this能出现在实例方法和构造方法中。
		this的语法是：“this.”、“this()”
		this不能使用在静态方法中。
		this. 大部分情况下是可以省略的。
		this.什
		么时候不能省略呢？ 在区分局部变量和实例变量的时候不能省略。
			public void setName(String name){
				this.name = name;
			}
		this() 只能出现在构造方法第一行，通过当前的构造方法去调用“本类”中
		其它的构造方法，目的是：代码复用。

	super:
		super能出现在实例方法和构造方法中。
		super的语法是：“super.”、“super()”
		super不能使用在静态方法中。
		super. 大部分情况下是可以省略的。
		super.什么时候不能省略呢？
		super() 只能出现在构造方法第一行，通过当前的构造方法去调用“父类”中
		的构造方法，目的是：创建子类对象的时候，先初始化父类型特征。

3、super()
	表示通过子类的构造方法调用父类的构造方法。
	模拟现实世界中的这种场景：要想有儿子，需要先有父亲。

4、重要的结论：
	当一个构造方法第一行：
		既没有this()又没有super()的话，默认会有一个super();
		表示通过当前子类的构造方法调用父类的无参数构造方法。
		所以必须保证父类的无参数构造方法是存在的。
		如果出现this()的话,那么当前构造方法一定没有supper().

5、注意：
this()和super() 不能共存，它们都是只能出现在构造方法第一行。

6、无论是怎样折腾，父类的构造方法是一定会执行的。（百分百的。）
```
### 4.1.15 final
```markdown
final修饰的类无法继承。
final修饰的方法无法覆盖。
final修饰的变量只能赋一次值。
final修饰的引用一旦指向某个对象，则不能再重新
指向其它对象，但该引用
指向的对象内部的数据是可以修改的。
final修饰的实例变量必须手动初始化，不能采用系统默
认值。(即要赶在默认初始化之前赋值就可以,也就是说可
以在构造方法中赋值也可以,因为赶在默认初始化之前赋值了)
例子:
public class Dog {
final int a; //编译不通过。
final int b; //编译通过。
final int c = 3; //编译通过。
public Dog() {
	b = 3;	
}
public static void main(String[] args) {
	Dog dog = new Dog();
	System.out.println(dog.b);
}
	
}

final修饰的实例变量一般和static联合使用，称为常量。
		public static final double PI = 3.1415926;
```
### 4.1.16 抽象类
```markdown
抽象类：
1.抽象类属于引用数据类型。

2.抽象类怎么定义？
语法：
[修饰符列表] abstract class 类名{
类体;
}

3.抽象类是无法实例化的，无法创建对象的，所以抽象
类是用来被子类继承的。

4.final和abstract不能联合使用，这两个关键字是对立的。

5.抽象类的子类可以是抽象类。也可以是非抽象类。

6.抽象类虽然无法实例化，但是抽象类有构造方法，这个
构造方法是供子类使用的。

7.抽象类关联到一个概念：抽象方法。什么是抽象方法呢？
抽象方法表示没有实现的方法，没有方法体的方法。例如：
public abstract void doSome();
抽象方法特点是：
特点1：没有方法体，以分号结尾。
特点2：前面修饰符列表中有abstract关键字。

8.抽象类中不一定有抽象方法，抽象方法必须出现在抽象类中。

9.一个非抽象的类，继承抽象类，必须将抽象类中的抽象
方法全部进行覆盖/重写/实现。一个抽象类继承
抽象类,可以不去实现.
```
### 4.1.17 接口
```markdown
接口：
1、接口也是一种“引用数据类型”。编译之后也是一个
class字节码文件。
2、接口是完全抽象的。（抽象类是半抽象。）或者也可以
说接口是特殊的抽象类。
3、接口怎么定义，语法是什么？
[修饰符列表] interface 接口名{}
4、接口支持多继承，一个接口可以继承多个接口。
5、接口中只包含两部分内容，一部分是：常量。一部分
是：抽象方法。接口中没有其它内容了。只有以上两部分。
6、接口中所有的元素都是public修饰的。（都是公开的。）
7、接口中的抽象方法定义时：public abstract修饰符可以省略。
8、接口中的方法都是抽象方法，所以接口中的方法不能有方法体。
9、接口中的常量的public static final可以省略。
```
### 4.1.18  抽象类和接口的区别。
```markdown
抽象类是半抽象的。
接口是完全抽象的。

抽象类中有构造方法。
接口中没有构造方法。

接口和接口之间支持多继承.即接口可以extends多个接口
类和类之间只能单继承。这里的类包括普通类跟抽象类

一个类可以同时实现多个接口。这里的类包括普通类跟抽象类

接口中只允许出现常量和抽象方法。
```
```markdown
面试题（判断题）：java语言中凡是没有方法体的方法都是抽象方法。
不对，错误的。
Object类中就有很多方法都没有方法体，都是以“;”结尾的，但他们
都不是抽象方法，例如：
public native int hashCode();
这个方法底层调用了C++写的动态链接库程序。
前面修饰符列表中没有：abstract。有一个native。表示
调用JVM本地程序。
```
### 4.1.19 类型和类型之间的关系：
```markdown
is a（继承）、has a（关联）、like a（实现）

		is a：
			Cat is a Animal（猫是一个动物）
			凡是能够满足is a的表示“继承关系”
			A extends B

		has a：
			I has a Pen（我有一支笔）
			凡是能够满足has a关系的表示“关联关系”
			关联关系通常以“属性”的形式存在。
			A{
				B b;
			}
		
		like a:
			Cooker like a FoodMenu（厨师像一个菜单一样）
			凡是能够满足like a关系的表示“实现关系”
			实现关系通常是：类实现接口。
			A implements B
```
### 4.1.20 关于java语言中的package和import机制：
```js
1、为什么要使用package？
package是java中包机制。包机制的作用是为了方便程序的管理。
不同功能的类分别存放在不同的包下。（按照功能划分的，不同的
软件包具有不同的功能。）

2、package怎么用？
package是一个关键字，后面加包名。例如：
package com.yxj.javase.chapter17;
注意：package语句只允许出现在java源代码的第一行。
如何编译跟运行:比如:com.yxj.javase.chapter17.HelloWorld.java
编译: 
第一种:建立  com.yxj.javase.chapter17
然后javac com.yxj.javase.chapter17.HelloWorld.java
第二种不需要建立包名目录
直接 java -d . HelloWorld.java,会自动生成  com.yxj.javase.chapter17.HelloWorld.class
运行:java com.yxj.javase.chapter17.HelloWorld
4.包名可以省略吗?
当java源文件在同一个包中时,package可以省略,否则不可以省略.
例子:

Test02在com包下。
HelloWorld在com.yxj.javase.chapter17下。

package com;
		不在同一个package下，包名可以省略吗？
			不能省略。
	*/
	//错误: 找不到符号
	/*
	HelloWorld hw = new HelloWorld();
	System.out.println(hw);


package com.yxj.javase.chapter17;
// 包名可以省略吗？
//可以,这里的包名之所以可以省略，是因为HelloWorld和Test01在同一个package下。
	HelloWorld hw2 = new HelloWorld();
	System.out.println(hw2); //com.yxj.javase.chapter17.HelloWorld@5305068a
3、包名有没有命名规范？有
一般都采用公司域名倒序的方式（因为公司域名具有全球唯一性。）
包名命名规范：
公司域名倒序 + 项目名 + 模块名 + 功能名

4、关于import的使用。

import什么时候使用？
A类中使用B类。
A和B类都在同一个包下。不需要import。
A和B类不在同一个包下。需要使用import。
java.lang.*;这个包下的类不需要使用import导入。

import怎么用？
import语句只能出现在package语句之下，class声明语句之上。
import语句还可以采用星号的方式。
```
### 4.1.21 访问控制权限
```markdown
以下的4个访问控制权限：控制的范围是什么？
public 表示公开的，在任何位置都可以访问
protected表示只能在本类、同包、子类中访问。
“默认”表示只能在本类，以及同包下访问。同一包下的子类也算同包，不同包下的子类就不是同包。
private 表示私有的，只能在本类中访问，跨类不可以，同一包下子类也算跨类

	访问控制修饰符			本类			同包			子类			任意位置
	---------------------------------------------------------------------------
	public					可以			可以			可以			可以
	protected				可以			可以			可以			不行
	默认						可以			可以			不行			不行
	private					可以			不行			不行			不行

	范围从大到小排序：public > protected > 默认 > private

访问控制权限修饰符可以修饰什么？
属性（4个都能用）
方法（4个都能用）
类（public和默认能用，其它不行。）
接口（public和默认能用，其它不行。）
```
### 4.1.22 JDK类库的根类：Object
```markdown
任何一个类默认继承Object。就算没有直接继承，最终也会间接继承。
目前为止我们只需要知道这几个方法即可：
protected Object clone()   // 负责对象克隆的。
int hashCode()	// 获取对象哈希值的一个方法。
boolean equals(Object obj)  // 判断两个对象是否相等
String toString()  // 将对象转换成字符串形式
protected void finalize()  // 垃圾回收器负责调用的方法
```
### 4.1.23  关于Object类中的toString()方法
```markdown
System.out.println(引用); 这里会自动调
用“引用”的toString()方法。
1、源代码长什么样？
public String toString() {
return this.getClass().getName() + "@" + Integer.toHexString(hashCode());
}
源代码上toString()方法的默认实现是：
类名@对象的内存地址转换为十六进制的形式

2、SUN公司设计toString()方法的目的是什么？
toString()方法的作用是什么？
toString()方法的设计目的是：通过调用这个方法可以将一个“java对象”转换成“字符串表示形式”

3、其实SUN公司开发java语言的时候，建议所有的子
类都去重写toString()方法。
toString()方法应该是一个简洁的、详实的、易阅读的.
```
```java
public class Test01{
public static void main(String[] args){
	MyTime t1 = new MyTime(1970, 1, 1);
	// 一个日期对象转换成字符串形式的话，我可能还是希望能看到具体的日期信息。
	String s1 = t1.toString();

	//MyTime类重写toString()方法之前
	//System.out.println(s1); // MyTime@28a418fc
	
	//MyTime类重写toString()方法之后
	System.out.println(s1); // 1970年1月1日

	
	//System.out.println(t1.toString()); //1970年1月1日

	// 注意：输出引用的时候，会自动调用该引用的toString()方法。
	System.out.println(t1);
}
}
class MyTime{
int year;
int month;
int day;

public MyTime(){

}

public MyTime(int year, int month, int day){
	this.year = year;
	this.month = month;
	this.day = day;
}

public String toString(){
	//return this.year + "年" + this.month + "月" + this.day + "日";
	return this.year + "/" + this.month + "/" + this.day;
}
}
```
### 4.1.24 String的equals方法以及toString方法。
```markdown
总结：
1、String类已经重写了equals方法，比较两个字符串不
能使用==，必须使用equals。
equals是通用的。

2、String类已经重写了toString方法。

大结论：
java中什么类型的数据可以使用“==”判断
java中基本数据类型比较是否相等，使用==
java中什么类型的数据需要使用equals判断
java中所有的引用数据类型统一使用equals方法来判断是否相等。

```
```java
public class Test03{
public static void main(String[] args){

	// 大部分情况下，采用这样的方式创建字符串对象
	String s1 = "hello";
	String s2 = "abc";

	// 实际上String也是一个类。不属于基本数据类型。
	// 既然String是一个类，那么一定存在构造方法。
	String s3 = new String("Test1");
	String s4 = new String("Test1");
	// new两次，两个对象内存地址，s3保存的内存地址和s4保存的内存地址不同。
	// == 判断的是内存地址。不是内容。
	System.out.println(s3 == s4); // false

	// 比较两个字符串能不能使用双等号？
	// 不能，必须调用equals方法。
	// String类已经重写equals方法了。
	System.out.println(s3.equals(s4)); // true

	// String类有没有重写toString方法呢？
	String x = new String("从前慢");
	// 如果String没有重写toString()方法，输出结果：java.lang.String@十六进制的地址
	// 经过测试：String类已经重写了toString()方法。
	System.out.println(x.toString()); //从前慢
	System.out.println(x); //从前慢
}
}
```
```java

// String对象比较的时候必须使用equals方法。
public class Test04{
public static void main(String[] args){
	/*
	Student s1 = new Student(111, "北京大兴亦庄二小");
	Student s2 = new Student(111, "北京大兴亦庄二小");
	System.out.println(s1 == s2); // false
	System.out.println(s1.equals(s2)); // true
	*/

	Student s1 = new Student(111, new String("北京大兴亦庄二小"));
	Student s2 = new Student(111, new String("北京大兴亦庄二小"));
	System.out.println(s1 == s2); // false
	System.out.println(s1.equals(s2)); // true
}
}

class Student{
// 学号
int no; //基本数据类型，比较时使用：==
// 所在学校
String school; //引用数据类型，比较时使用：equals方法。

public Student(){}
public Student(int no,String school){
	this.no = no;
	this.school = school;
}

// 重写toString方法
public String toString(){
	return "学号" + no + "，所在学校名称" + school;
}

// 重写equals方法
// 需求：当一个学生的学号相等，并且学校相同时，表示同一个学生。
// 思考：这个equals该怎么重写呢？
// equals方法的编写模式都是固定的。架子差不多。
public boolean equals(Object obj){
	if(obj == null || !(obj instanceof Student)) return false;
	if(this == obj) return true;
	Student s = (Student)obj;
	return this.no == s.no && this.school.equals(s.school);

	//字符串用双等号比较可以吗？
	// 不可以
	//return this.no == s.no && this.school == s.school;
}
}

```
### 4.1.25 关于Object类中的equals方法
```markdown
1、equals方法的源代码
public boolean equals(Object obj) {
	return (this == obj);
}
以上这个方法是Object类的默认实现。

2、判断两个java对象是否相等，不能使用“==”，因为“==”比
较的是两个
对象的内存地址。
```
```java
public class Test02{
public static void main(String[] args){

	// 判断两个基本数据类型的数据是否相等直接使用“==”就行。
	int a = 100;
	int b = 100;
	// 这个“==”是判断a中保存的100和b中保存的100是否相等。
	System.out.println(a == b); //true（相等） false(不相等)

	// 重写Object equals方法之后（比较的是内容。）
	MyTime t1 = new MyTime(2018,1,1);
	MyTime t2 = new MyTime(2018,1,1);
	boolean flag = t1.equals(t2);
	System.out.println(flag); //true

}
}

class MyTime { 
int year;
int month;
int day;

public MyTime(){

}
public MyTime(int year, int month, int day){
	this.year = year;
	this.month = month;
	this.day = day;
}

// 重写Object类的equals方法
// 怎么重写？复制粘贴。相同的返回值类型、相同的方法名、相同的形式参数列表。
// equals到底应该怎么重写？你自己定，你认为两个对象什么相等的时候表示相等，你就怎么重写。
public boolean equals(Object obj) {
	if(obj == null || !(obj instanceof MyTime)){
		return false;
	}
	if(this == obj){
		return true;
	}
	MyTime t = (MyTime)obj;
	return this.year == t.year && this.month == t.month && this.day == t.day ;
}

}

```
### 4.1.26 finalize()方法
```markdown
这个方法是protected修饰的，在Object类中这个方法的源代码是？
protected void finalize() throws Throwable { }
finalize()方法只有一个方法体，里面没有代码，而且这
个方法是protected修饰的。
GC：负责调用finalize()方法。
这个方法不需要程序员手动调用，JVM的垃圾回收器负责调用这个方法。
不像equals和toString()方法是需要你写代码调用的。
finalize()只需要重写，重写完将来自动会有程序来调用。

finalize()方法的执行时机：
当一个java对象即将被垃圾回收器回收的时候，垃圾回收器负责调用
finalize()方法。
finalize()方法实际上是SUN公司为java程序员准备的一个时机，
垃圾销毁时机。
如果希望在对象销毁时机执行一段代码的话，这段代码
要写到finalize()方法当中。
提示：	
java中的垃圾回收器不是轻易启动的，
垃圾太少，或者时间没到，种种条件下，
有可能启动，也有可能不启动。
```
```java

public class Test06{
public static void main(String[] args){
	/*
	// 创建对象
	Person p = new Person();

	// 怎么把Person对象变成垃圾？
	p = null;
	*/

	// 多造点垃圾
	/*
	for(int i = 0; i < 100000000; i++){
		Person p = new Person();
		p = null;
	}
	*/
	
	for(int i = 0; i < 1000; i++){
		Person p = new Person();
		p = null;

		// 有一段代码可以建议垃圾回收器启动。
		if(i % 2 == 0){
			System.gc(); // 建议启动垃圾回收器。（只是建议，可能不启动，也可能启动。启动的概率高了一些。）
		}
	}		

}
}

// 项目开发中有这样的业务需求：所有对象在JVM中被释放的时候，请记录一下释放时间！！！
// 记录对象被释放的时间点，这个负责记录的代码写到哪里？
// 写到finalize()方法中。
class Person{

// 重写finalize()方法
// Person类型的对象被垃圾回收器回收的时候，垃圾回收器负责调用：p.finalize();
protected void finalize() throws Throwable {
	// this代表当前对象
	System.out.println(this + "即将被销毁！");
}

}
```
### 4.1.27 hashCode方法：
```markdown
hashCode方法：
在Object中的hashCode方法是怎样的？
public native int hashCode();
这个方法不是抽象方法，带有native关键字，底层调用C++程序。

hashCode()方法返回的是哈希码：
实际上就是一个java对象的内存地址，经过哈希算法，得出的一个值。
所以hashCode()方法的执行结果可以等同看做一个java对象
的内存地址。
```
```java
public class Test07{
public static void main(String[] args){
	Object o = new Object();
	int hashCodeValue = o.hashCode();

	// 对象内存地址经过哈希算法转换的一个数字。可以等同看做内存地址。
	System.out.println(hashCodeValue); //798154996

	MyClass mc = new MyClass();
	int hashCodeValue2 = mc.hashCode();
	System.out.println(hashCodeValue2); //1392838282

	MyClass mc2 = new MyClass();
	System.out.println(mc2.hashCode()); // 523429237
}
}

class MyClass
{
}
```
### 4.1.28 内部类
```markdown
1、什么是内部类？
内部类：在类的内部又定义了一个新的类。被称为内部类。

2、内部类的分类：
静态内部类：类似于静态变量
实例内部类：类似于实例变量
局部内部类：类似于局部变量
匿名内部类:属于局部内部类的一种,应用于参数传递.

3、匿名内部类是局部内部类的一种。
因为这个类没有名字而得名，叫做匿名内部类。
```
```java
class Test01{

// 静态变量
static String country;
// 该类在类的内部，所以称为内部类
// 由于前面有static，所以称为“静态内部类”
static class Inner1{
}

// 实例变量
int age;
// 该类在类的内部，所以称为内部类
// 没有static叫做实例内部类。
class Inner2{
}

// 方法
public void doSome(){
	// 局部变量
	int i = 100;
	// 该类在类的内部，所以称为内部类
	// 局部内部类。
	class Inner3{
	}
}

public void doOther(){
	// doSome()方法中的局部内部类Inner3，在doOther()中不能用。
}

// main方法，入口
public static void main(String[] args){
	// 调用MyMath中的mySum方法。
	MyMath mm = new MyMath();
	/*
	Compute c = new ComputeImpl();
	mm.mySum(c, 100, 200);
	*/
	
	//合并（这样写代码，表示这个类名是有的。类名是：ComputeImpl）
	//mm.mySum(new ComputeImpl(), 100, 200);

	// 使用匿名内部类，表示这个ComputeImpl这个类没名字了。
	// 这里表面看上去好像是接口可以直接new了，实际上并不是接口可以new了。
	// 后面的{} 代表了对接口的实现。
	// 不建议使用匿名内部类，为什么？
	// 因为一个类没有名字，没有办法重复使用。另外代码太乱，可读性太差。
	mm.mySum(new Compute(){
		public int sum(int a, int b){
			return a + b;
		}
	}, 200, 300);



}

}

// 负责计算的接口
interface Compute{ 

// 抽象方法
int sum(int a, int b);
}

// 你自动会在这里编写一个Compute接口的实现类
/*
class ComputeImpl implements Compute{

// 对方法的实现
public int sum(int a, int b){
	return a + b;
}
}
*/

// 数学类
class MyMath{
// 数学求和方法
public void mySum(Compute c, int x, int y){
	int retValue = c.sum(x, y);
	System.out.println(x + "+" + y + "=" + retValue);
}	
}

```
### 4.1.29 数组
```markdown
Array
1、Java语言中的数组是一种引用数据类型。不属于基本数
据类型。数组的父类是Object。
2、数组实际上是一个容器，可以同时容纳多个元
素。（数组是一个数据的集合。）
3、数组当中可以存储“基本数据类型”的数据，也可
以存储“引用数据类型”的数据。
4、数组因为是引用类型，所以数组对象是堆内存
当中。（数组是存储在堆当中的）
5、数组当中如果存储的是“java对象”的话，实际上存储
的是对象的“引用（内存地址）”，数组中不能直接存储java对象。
6、数组一旦创建，在java中规定，长度不可
变。（数组长度不可变）
7、数组的分类：一维数组、二维数组、三维数
组、多维数组...（一维数组较多，二维数组偶尔使用！）
8、所有的数组对象都有length属性(java自带的)，用
来获取数组中元素的个数。
9、java中的数组要求数组中元素的类型统一。比如int类
型数组只能存储int类型，Person类型数组只能存储Person类型。
10、数组在内存方面存储的时候，数组中的元素内存
地址(存储的每一个元素都是有规则的挨着排列的)
是连续的。内存地址连续。
11、所有的数组都是拿“第一个小方框的内存地址”作为
整个数组对象的内存地址。
（数组中首元素的内存地址作为整个数组对象的内存地址。）
12、数组中每一个元素都是有下标的，下标从0开始，以1递
增。最后一个元素的下标是：length - 1
下标非常重要，因为我们对数组中元素进行“存取”
的时候，都需要通过下标来进行。
13、数组这种数据结构的优点和缺点是什么？
优点：查询/查找/检索某个下标上的元素时效率极
高。可以说是查询效率最高的一个数据结构。
为什么检索效率高？
第一：每一个元素的内存地址在空间存储上是连续的。
第二：每一个元素类型相同，所以占用空间大小一样。
第三：知道第一个元素内存地址，知道每一个元素占用
空间的大小，又知道下标，所以
通过一个数学表达式就可以计算出某个下标上元素的内
存地址。直接通过内存地址定位
元素，所以数组的检索效率是最高的。

数组中存储100个元素，或者存储100万个元素，在元素查
询/检索方面，效率是相同的，
因为数组中元素查找的时候不会一个一个找，是通
过数学表达式计算出来的。（算出一个
内存地址，直接定位的。）
缺点：
第一：由于为了保证数组中每个元素的内存地址连续，所
以在数组上随机删除或者增加元素的时候，
效率较低，因为随机增删元素会涉及到后面元素统一向前
或者向后位移的操作。
第二：数组不能存储大数据量，为什么？
因为很难在内存空间上找到一块特别大的连续的内存空间。

注意：对于数组中最后一个元素的增删，是没有效率影响的。

14、怎么声明/定义一个一维数组？
语法格式：
int[] array1;
double[] array2;
boolean[] array3;
String[] array4;
Object[] array5;
15、怎么初始化一个一维数组呢？
包括两种方式：静态初始化一维数组，动态初始化一维数组。
静态初始化语法格式：
int[] array = {100, 2100, 300, 55};
int[] a = new int[]{1,2,3};
动态初始化语法格式：
int[] array = new int[5]; // 这里的5表示数组的元素个数。
// 初始化一个5个长度的int类型数组，每个元素默认值0
String[] names = new String[6]; // 初始化6个长度的String类型
数组，每个元素默认值null。
注意:数组跟其他类型不一样,既可以在方法区内不赋
值,也可以在方法外不赋值,不赋值默认由系统赋值.
什么时候采用静态初始化方式，什么时候使用动态初始化方式呢？
当你创建数组的时候，确定数组中存储哪些具体
的元素时，采用静态初始化方式。
当你创建数组的时候，不确定将来数组中存储哪些数据
，你可以采用动态初始化的方式，预先分配内存空间。
```
```java
package com.yxj.javase.array;

public class ArrayTest01 {
   public static void main(String[] args) {
       // 声明一个int类型的数组，使用静态初始化的方式
       int[] a = {1, 100, 10, 20, 55, 689};
       // 这是C++风格，不建议java中使用。
       //int a[] = {1, 100, 10, 20, 55, 689};

       // 所有的数组对象都有length属性
       System.out.println("数组中元素的个数" + a.length);

       // 数组中每一个元素都有下标
       // 通过下标对数组中的元素进行存和取。
       // 取（读）
       System.out.println("第一个元素 = " + a[0]);
       System.out.println("最后一个元素 = " + a[5]);
       System.out.println("最后一个元素 = " + a[a.length - 1]);

       // 存（改）
       // 把第一个元素修改为111
       a[0] = 111;
       // 把最后一个元素修改为0
       a[a.length - 1] = 0;

       System.out.println("第一个元素 = " + a[0]);
       System.out.println("最后一个元素 = " + a[5]);

       // 一维数组怎么遍历呢？
       for(int i = 0; i < a.length; i++){
           System.out.println(a[i]); // i是从0到5，是下标
       }

       // 下标为6表示第7个元素，第7个元素没有，下标越界了。会出现什么异常呢？
       //System.out.println(a[6]); //ArrayIndexOutOfBoundsException（比较著名的异常。）

       // 从最后一个元素遍历到第1个元素
       for (int i = a.length - 1; i >= 0; i--) {
           System.out.println("颠倒顺序输出-->" + a[i]);
       }
   }
}

```
#### 4.1.29.1 认识main方法上面的“String[] args”
```java
package com.yxj.javase.array;

public class ArrayTest05 {
   // 这个方法程序员负责写出来，JVM负责调用。JVM调用的时候一定会传一个String数组过来。
   public static void main(String[] args) {
       // JVM默认传递过来的这个数组对象的长度？默认0
       // 通过测试得出：args不是null。
       System.out.println("JVM给传递过来的String数组参数，它这个数组的长度是？" + args.length);

       // 以下这一行代码表示的含义：数组对象创建了，但是数组中没有任何数据。
       //String[] strs = new String[0];
       //String[] strs = {}; // 静态初始化数组，里面没东西。
       //printLength(strs);

       // 这个数组什么时候里面会有值呢？
       // 其实这个数组是留给用户的，用户可以在控制台上输入参数，这个参数自动会被转换为“String[] args”
       // 例如这样运行程序：java ArrayTest05 abc def xyz
       // 那么这个时候JVM会自动将“abc def xyz”通过空格的方式进行分离，分离完成之后，自动放到“String[] args”数组当中。
       // 所以main方法上面的String[] args数组主要是用来接收用户输入参数的。
       // 把abc def xyz 转换成字符串数组：{"abc","def","xyz"}
       // 遍历数组
       for (int i = 0; i < args.length; i++) {
           System.out.println(args[i]);
       }

   }

   public static void printLength(String[] args){
       System.out.println(args.length); // 0
   }
}

```
```java
package com.yxj.javase.array;

/*
模拟一个系统，假设这个系统要使用，必须输入用户名和密码。
*/
public class ArrayTest06 {
   // 用户名和密码输入到String[] args数组当中。
   public static void main(String[] args) {
       if(args.length != 2){
           System.out.println("使用该系统时请输入程序参数，参数中包括用户名和密码信息，例如：zhangsan 123");
           return;
       }

       // 程序执行到此处说明用户确实提供了用户名和密码。
       // 接下来你应该判断用户名和密码是否正确。
       // 取出用户名
       String username = args[0];
       // 取出密码 
       String password = args[1];

       // 假设用户名是admin，密码是123的时候表示登录成功。其它一律失败。
       // 判断两个字符串是否相等，需要使用equals方法。
       //if(username.equals("admin") && password.equals("123")){
       // 这样编写是不是可以避免空指针异常。
       // 采用以下编码风格，及时username和password都是null，也不会出现空指针异常。（这是老程序员给的一条编程经验。）
       if("admin".equals(username) && "123".equals(password)){
           System.out.println("登录成功，欢迎[" + username + "]回来");
           System.out.println("您可以继续使用该系统....");
       }else{
           System.out.println("验证失败，用户名不存在或者密码错误！");
       }
   }
}

```
#### 4.1.29.2 一维数组的深入
```java
package com.yxj.javase.array;

/**
* 一维数组的深入，数组中存储的类型为：引用数据类型
* 对于数组来说，实际上只能存储java对象的“内存地址”。数组中存储的每个元素是“引用”。
*/
public class ArrayTest07 {
   public static void main(String[] args) {

       // a是一个数组
       // a[0] 是数组中的一个元素。
       // a[1] 是数组中的一个元素。
       int[] a = {100, 200, 300};
       System.out.println(a[1]);
       //a[2] = 400;

       int[] array = {1,2,3};
       for (int i = 0; i < array.length; i++) {
           /*int temp = array[i];
           System.out.println(temp);*/
           System.out.println(array[i]);
       }

       // 创建一个Animal类型的数组
       Animal a1 = new Animal();
       Animal a2 = new Animal();
       Animal[] animals = {a1, a2};

       // 对Animal数组进行遍历
       for (int i = 0; i < animals.length; i++) {
           /*Animal a = animals[i];
           a.move();*/
           // 代码合并
           animals[i].move(); // 这个move()方法不是数组的。是数组当中Animal对象的move()方法。
       }

       // 动态初始化一个长度为2的Animal类型数组。
       Animal[] ans = new Animal[2];
       // 创建一个Animal对象，放到数组的第一个盒子中。
       ans[0] = new Animal();

       // Animal数组中只能存放Animal类型，不能存放Product类型。
       //ans[1] = new Product();

       // Animal数组中可以存放Cat类型的数据，因为Cat是一个Animal。
       // Cat是Animal的子类。
       ans[1] = new Cat();

       // 创建一个Animal类型的数组，数组当中存储Cat和Bird
       Cat c = new Cat();
       Bird b = new Bird();
       Animal[] anis = {c, b};

       //Animal[] anis = {new Cat(), new Bird()}; // 该数组中存储了两个对象的内存地址。
       for (int i = 0; i < anis.length; i++){
           // 这个取出来的可能是Cat，也可能是Bird，不过肯定是一个Animal
           // 如果调用的方法是父类中存在的方法不需要向下转型。直接使用父类型引用调用即可。
           //anis[i]
           //Animal an = anis[i];
           //an.move();

           //Animal中没有sing()方法。
           //anis[i].sing();

           // 调用子对象特有方法的话，需要向下转型！！！
           if(anis[i] instanceof Cat){
               Cat cat = (Cat)anis[i];
               cat.catchMouse();
           }else if(anis[i] instanceof Bird){
               Bird bird = (Bird)anis[i];
               bird.sing();
           }
       }

   }
}

class Animal{
   public void move(){
       System.out.println("Animal move...");
   }
}

// 商品类
class Product{

}

// Cat是子类
class Cat extends Animal {
   public void move(){
       System.out.println("猫在走猫步！");
   }
   // 特有方法
   public void catchMouse(){
       System.out.println("猫抓老鼠！");
   }
}

// Bird子类
class Bird extends Animal {
   public void move(){
       System.out.println("Bird Fly!!!");
   }
   // 特有的方法
   public void sing(){
       System.out.println("鸟儿在歌唱！！！");
   }
}
```
#### 4.1.29.3 数组的拷贝
```markdown
public static native void arraycopy(Object src,  int  srcPos,Object dest, int destPos, int length);
Object src : 原数组
int srcPos : 从原数组的哪个位置开始
Object dest : 目标数组
int destPos : 目标数组的开始起始位置
int length  : 要copy的数组的长度 
```
```java
package com.yxj.javase.array;

/**
* 关于一维数组的扩容。
* 在java开发中，数组长度一旦确定不可变，那么数组满了怎么办？
*      数组满了，需要扩容。
*      java中对数组的扩容是：
*          先新建一个大容量的数组，然后将小容量数组中的数据一个一个拷贝到大数组当中。
*
* 结论：数组扩容效率较低。因为涉及到拷贝的问题。所以在以后的开发中请注意：尽可能少的进行数组的拷贝。
* 可以在创建数组对象的时候预估计以下多长合适，最好预估准确，这样可以减少数组的扩容次数。提高效率。
*/
public class ArrayTest08 {
   public static void main(String[] args) {
       // java中的数组是怎么进行拷贝的呢？
       //System.arraycopy(5个参数);

       // 拷贝源（从这个数组中拷贝）
       int[] src = {1, 11, 22, 3, 4};

       // 拷贝目标（拷贝到这个目标数组上）
       int[] dest = new int[20]; // 动态初始化一个长度为20的数组，每一个元素默认值0

       // 调用JDK System类中的arraycopy方法，来完成数组的拷贝
       //System.arraycopy(src, 1, dest, 3, 2);

       // 遍历目标数组
       /*
       for (int i = 0; i < dest.length; i++) {
           System.out.println(dest[i]); // 0 0 0 11 22 ... 0
       }
        */

       System.arraycopy(src, 0, dest, 0, src.length);
       for (int i = 0; i < dest.length; i++) {
           System.out.println(dest[i]);
       }

       // 数组中如果存储的元素是引用，可以拷贝吗？当然可以。
       String[] strs = {"hello", "world!", "study", "java", "oracle", "mysql", "jdbc"};
       String[] newStrs = new String[20];
       System.arraycopy(strs, 0, newStrs, 0, strs.length);
       for (int i = 0; i < newStrs.length; i++) {
           System.out.println(newStrs[i]);
       }

       System.out.println("================================");
       Object[] objs = {new Object(), new Object(), new Object()};
       Object[] newObjs = new Object[5];
       // 思考一下：这里拷贝的时候是拷贝对象，还是拷贝对象的地址。（地址。）
       System.arraycopy(objs, 0, newObjs, 0, objs.length);
       for (int i = 0; i < newObjs.length; i++) {
           System.out.println(newObjs[i]);
       }
   }
}

```
#### 4.1.29.4 二维数组
```markdown
关于java中的二维数组
1、二维数组其实是一个特殊的一维数组，特殊在这
个一维数组当中的每一个元素是一个一维数组。
2、二维数组静态初始化
int[][] array = {{1,1,1},{2,3,4,5},{0,0,0,0},{2,3,4,5},{2,3,4,5},{2,3,4,5},{2,3,4,5}};
或者
int[][] array = new int[][]{{1,2,3,4},{4,5,6,76},{1,23,4}};
```
```java
package com.yxj.javase.array;

/*
动态初始化二维数组。
*/
public class ArrayTest12 {
   public static void main(String[] args) {
       // 3行4列。
       // 3个一维数组，每一个一维数组当中4个元素。
       int[][] array = new int[3][4];

       // 二维数组遍历
       /*
       for (int i = 0; i < array.length; i++) { // 循环3次。
           for (int j = 0; j < array[i].length; j++) {
               System.out.print(array[i][j] + " ");
           }
           System.out.println();
       }
        */

       // 静态初始化
       int[][] a = {{1,2,3,4},{4,5,6,76},{1,23,4}};
       printArray(a);

       // 没有这种语法
       //printArray({{1,2,3,4},{4,5,6,76},{1,23,4}});

       // 可以这样写。
       printArray(new int[][]{{1,2,3,4},{4,5,6,76},{1,23,4}});
   }

   public static void printArray(int[][] array){
       // 遍历二维数组。
       for (int i = 0; i < array.length; i++) {
           for (int j = 0; j < array[i].length; j++) {
               System.out.print(array[i][j] + " ");
           }
           System.out.println();
       }
   }
}

```
### 4.1.30 常用类

#### 4.1.30.1 String
```markdown
关于Java JDK中内置的一个类：java.lang.String
1、String表示字符串类型，属于引用数据类型，不
属于基本数据类型。
2、在java中随便使用双引号括起来的都是String对象。
例如："abc"，"def"，"hello world!"，这是3个String对象。
3、java中规定，双引号括起来的字符串，是不可变的，也
就是说"abc"自出生到最终死亡，不可变，不能变成"abcd"，
也不能变成"ab"
4、在JDK当中双引号括起来的字符串，例如："abc" "def"都
是直接存储在“方法区”的“字符串常量池”当中的。
为什么SUN公司把字符串存储在一个“字符串常量池”当中
呢。因为字符串在实际的开发中使用太频繁。为了执行
效率，所以把字符串放到了方法区的字符串常量池当中。
```
```java
package com.yxj.javase.string;


public class StringTest01 {
   public static void main(String[] args) {
       // 这两行代码表示底层创建了3个字符串对象，都在字符串常量池当中。
       String s1 = "abcdef";
       String s2 = "abcdef" + "xy";

       // 分析：这是使用new的方式创建的字符串对象。这个代码中的"xy"是从哪里来的？
       // 凡是双引号括起来的都在字符串常量池中有一份。
       // new对象的时候一定在堆内存当中开辟空间。
       String s3 = new String("xy");

       // i变量中保存的是100这个值。
       int i = 100;
       // s变量中保存的是字符串对象的内存地址。
       // s引用中保存的不是"abc"，是0x1111
       // 而0x1111是"abc"字符串对象在“字符串常量池”当中的内存地址。
       String s = "abc";
   }
}

```
```java
package com.yxj.javase.string;

public class StringTest02 {
   public static void main(String[] args) {
       String s1 = "hello";
       // "hello"是存储在方法区的字符串常量池当中
       // 所以这个"hello"不会新建。（因为这个对象已经存在了！）
       String s2 = "hello";
       // 分析结果是true还是false？
       // == 双等号比较的是不是变量中保存的内存地址？是的。
       System.out.println(s1 == s2); // true

       String x = new String("xyz");
       String y = new String("xyz");
       // 分析结果是true还是false？
       // == 双等号比较的是不是变量中保存的内存地址？是的。
       System.out.println(x == y); //false

       // 通过这个案例的学习，我们知道了，字符串对象之间的比较不能使用“==”
       // "=="不保险。应该调用String类的equals方法。
       // String类已经重写了equals方法，以下的equals方法调用的是String重写之后的equals方法。
       System.out.println(x.equals(y)); // true

       String k = new String("testString");
       //String k = null;
       // "testString"这个字符串可以后面加"."呢？
       // 因为"testString"是一个String字符串对象。只要是对象都能调用方法。
       System.out.println("testString".equals(k)); // 建议使用这种方式，因为这个可以避免空指针异常。
       System.out.println(k.equals("testString")); // 存在空指针异常的风险。不建议这样写。
   }
}

```
```java
package com.yxj.javase.string;
/*
分析以下程序，一共创建了几个对象
*/
public class StringTest03 {
   public static void main(String[] args) {
       /*
       一共3个对象：
           方法区字符串常量池中有1个："hello"
           堆内存当中有两个String对象。
           一共3个。
        */
       String s1 = new String("hello");
       String s2 = new String("hello");
   }
}

```
#### 4.1.30.2 String的构造方法
```java
package com.yxj.javase.string;

/**
* 关于String类中的构造方法。
*  第一个：String s = new String("");
*  第二个：String s = ""; 最常用
*  第三个：String s = new String(char数组);
*  第四个：String s = new String(char数组,起始下标,长度);
*  第五个：String s = new String(byte数组);
*  第六个：String s = new String(byte数组,起始下标,长度)
*/
public class StringTest04 {
   public static void main(String[] args) {

       // 创建字符串对象最常用的一种方式
       String s1 =  "hello world!";
       // s1这个变量中保存的是一个内存地址。
       // 按说以下应该输出一个地址。
       // 但是输出一个字符串，说明String类已经重写了toString()方法。
       System.out.println(s1);//hello world!
       System.out.println(s1.toString()); //hello world!

       // 这里只掌握常用的构造方法。
       byte[] bytes = {97, 98, 99}; // 97是a，98是b，99是c
       String s2 = new String(bytes);

       // 前面说过：输出一个引用的时候，会自动调用toString()方法，默认Object的话，会自动输出对象的内存地址。
       // 通过输出结果我们得出一个结论：String类已经重写了toString()方法。
       // 输出字符串对象的话，输出的不是对象的内存地址，而是字符串本身。
       System.out.println(s2.toString()); //abc
       System.out.println(s2); //abc

       // String(字节数组,数组元素下标的起始位置,长度)
       // 将byte数组中的一部分转换成字符串。
       String s3 = new String(bytes, 1, 2);
       System.out.println(s3); // bc

     	  // 将char数组全部转换成字符串
       char[] chars = {'我','是','中','国','人'};
       String s4 = new String(chars); 
       System.out.println(s4); //我是中国人
       // 将char数组的一部分转换成字符串
       String s5 = new String(chars, 2, 3);
       System.out.println(s5); //中国人

       String s6 = new String("helloworld!");
       System.out.println(s6); //helloworld!
   }
}

```
#### 4.1.30.3 String的常用方法
```java
package com.yxj.javase.string;

public class StringTest05 {
   public static void main(String[] args) {

       // String类当中常用方法。
       //1（掌握）.char charAt(int index)
       char c = "中国人".charAt(1); // "中国人"是一个字符串String对象。只要是对象就能“点.”
       System.out.println(c); // 国

       // 2（了解）.int compareTo(String anotherString)
       // 字符串之间比较大小不能直接使用 > < ，需要使用compareTo方法。
       int result = "abc".compareTo("abc");
       System.out.println(result); //0（等于0） 前后一致  10 - 10 = 0

       int result2 = "abcd".compareTo("abce");
       System.out.println(result2); //-1（小于0） 前小后大 8 - 9 = -1

       int result3 = "abce".compareTo("abcd");
       System.out.println(result3); // 1（大于0） 前大后小 9 - 8 = 1

       // 拿着字符串第一个字母和后面字符串的第一个字母比较。能分胜负就不再比较了。
       System.out.println("xyz".compareTo("yxz")); // -1

       // 3（掌握）.boolean contains(CharSequence s)
       // 判断前面的字符串中是否包含后面的子字符串。
       System.out.println("HelloWorld.java".contains(".java")); // true
       System.out.println("http://www.baidu.com".contains("https://")); // false

       // 4（掌握）. boolean endsWith(String suffix)
       // 判断当前字符串是否以某个子字符串结尾。
       System.out.println("test.txt".endsWith(".java")); // false
       System.out.println("test.txt".endsWith(".txt")); // true
       System.out.println("fdsajklfhdkjlsahfjkdsahjklfdss".endsWith("ss")); // true

       // 5（掌握）.boolean equals(Object anObject)
       // 比较两个字符串必须使用equals方法，不能使用“==”
       // equals方法有没有调用compareTo方法？ 老版本可以看一下。JDK13中并没有调用compareTo()方法。
       // equals只能看出相等不相等。
       // compareTo方法可以看出是否相等，并且同时还可以看出谁大谁小。
       System.out.println("abc".equals("abc")); // true

       // 6（掌握）.boolean equalsIgnoreCase(String anotherString)
       // 判断两个字符串是否相等，并且同时忽略大小写。
       System.out.println("ABc".equalsIgnoreCase("abC")); // true

       // 7（掌握）.byte[] getBytes()
       // 将字符串对象转换成字节数组
       byte[] bytes = "abcdef".getBytes();
       for(int i = 0; i < bytes.length; i++){
           System.out.println(bytes[i]);
       }

       // 8（掌握）.int indexOf(String str)
       // 判断某个子字符串在当前字符串中第一次出现处的索引（下标）。
       System.out.println("oraclejavac++.netc#phppythonjavaoraclec++".indexOf("java")); // 6

       // 9（掌握）.boolean isEmpty()
       // 判断某个字符串是否为“空字符串”。底层源代码调用的应该是字符串的length()方法。
       //String s = "";
       String s = "a";
       System.out.println(s.isEmpty()); //false

       // 10（掌握）. int length()
       // 面试题：判断数组长度和判断字符串长度不一样
       // 判断数组长度是length属性，判断字符串长度是length()方法。
       System.out.println("abc".length()); // 3

       System.out.println("".length()); // 0

       // 11（掌握）.int lastIndexOf(String str)
       // 判断某个子字符串在当前字符串中最后一次出现的索引（下标）
       System.out.println("oraclejavac++javac#phpjavapython".lastIndexOf("java")); //22

       // 12（掌握）. String replace(CharSequence target, CharSequence replacement)
       // 替换。
       // String的父接口就是：CharSequence
       String newString = "http://www.baidu.com".replace("http://", "https://");
       System.out.println(newString); //https://www.baidu.com
       // 把以下字符串中的“=”替换成“:”
       String newString2 = "name=zhangsan&password=123&age=20".replace("=", ":");
       System.out.println(newString2); //name:zhangsan&password:123&age:20

       // 13（掌握）.String[] split(String regex)
       // 拆分字符串
       String[] ymd = "1980-10-11".split("-"); //"1980-10-11"以"-"分隔符进行拆分。
       for(int i = 0; i < ymd.length; i++){
           System.out.println(ymd[i]);
       }
       String param = "name=zhangsan&password=123&age=20";
       String[] params = param.split("&");
       for(int i = 0; i <params.length; i++){
           System.out.println(params[i]);
           // 可以继续向下拆分，可以通过“=”拆分。
       }

       // 14（掌握）.boolean startsWith(String prefix)
       // 判断某个字符串是否以某个子字符串开始。
       System.out.println("http://www.baidu.com".startsWith("http")); // true
       System.out.println("http://www.baidu.com".startsWith("https")); // false

       // 15（掌握）.String substring(int beginIndex) 参数是起始下标。
       // 截取字符串
       System.out.println("http://www.baidu.com".substring(7)); //www.baidu.com

       // 16（掌握）.String substring(int beginIndex, int endIndex)
       // beginIndex起始位置（包括）
       // endIndex结束位置（不包括）
       System.out.println("http://www.baidu.com".substring(7, 10)); //www

       // 17(掌握).char[] toCharArray()
       // 将字符串转换成char数组
       char[] chars = "我是中国人".toCharArray();
       for(int i = 0; i < chars.length; i++){
           System.out.println(chars[i]);
       }

       // 18（掌握）.String toLowerCase()
       // 转换为小写。
       System.out.println("ABCDefKXyz".toLowerCase());

       // 19（掌握）.String toUpperCase();
       System.out.println("ABCDefKXyz".toUpperCase());

       // 20（掌握）. String trim();
       // 去除字符串前后空白
       System.out.println("           hello      world             ".trim());

       // 21（掌握）. String中只有一个方法是静态的，不需要new对象
       // 这个方法叫做valueOf，这个底层会调用toString();
       // 作用：将“非字符串”转换成“字符串”
       //String s1 = String.valueOf(true);
       //String s1 = String.valueOf(100);
       //String s1 = String.valueOf(3.14);

       // 这个静态的valueOf()方法，参数是一个对象的时候，会自动调用该对象的toString()方法。
       String s1 = String.valueOf(new Customer());
       //System.out.println(s1); // 没有重写toString()方法之前是对象内存地址 com.yxj.javase.string.Customer@10f87f48
       System.out.println(s1); //重写toString()方法后，我是一个VIP客户！！！！

       // 我们是不是可以研究一下println()方法的源代码了？//这个变为字符串后再在控制台输出.System.out.println()该方法会调用valueOf(),valueOf()会调用toString();
       System.out.println(100);
       System.out.println(3.14);
       System.out.println(true);

   }
}

class Customer {
   // 重写toString()方法

   @Override
   public String toString() {
       return "我是一个VIP客户！！！！";
   }
}

```
#### 4.1.30.4 进行字符串的频繁拼接，会有什么问题
```java
package com.yxj.javase.stringbuffer;

/**
* 思考：我们在实际的开发中，如果需要进行字符串的频繁拼接，会有什么问题？
*      因为java中的字符串是不可变的，每一次拼接都会产生新字符串。
*      这样会占用大量的方法区内存。造成内存空间的浪费。
*          String s = "abc";
*          s += "hello";
*          就以上两行代码，就导致在方法区字符串常量池当中创建了3个对象：
*              "abc"
*              "hello"
*              "abchello"  
*/
public class StringBufferTest01 {
   public static void main(String[] args) {
       String s = "";
       // 这样做会给java的方法区字符串常量池带来很大的压力。
       for(int i = 0; i < 100; i++){
           //s += i;
           s = s + i;
           System.out.println(s);
       }
   }
}

```
#### 4.1.30.5 StringBuffer/StringBuilder解决进行字符串的频繁拼接
```java
package com.yxj.javase.stringbuffer;

/**
* 如果以后需要进行大量字符串的拼接操作，建议使用JDK中自带的：
*      java.lang.StringBuffer
*      java.lang.StringBuilder
*
* 如何优化StringBuffer的性能？
*      在创建StringBuffer的时候尽可能给定一个初始化容量。
*      最好减少底层数组的扩容次数。预估计一下，给一个大一些初始化容量。
*      关键点：给一个合适的初始化容量。可以提高程序的执行效率。
*/
public class StringBufferTest02 {
   public static void main(String[] args) {

       // 创建一个初始化容量为16个byte[] 数组。（字符串缓冲区对象）
       StringBuffer stringBuffer = new StringBuffer();

       // 拼接字符串，以后拼接字符串统一调用 append()方法。
       // append是追加的意思。
       stringBuffer.append("a");
       stringBuffer.append("b");
       stringBuffer.append("d");
       stringBuffer.append(3.14);
       stringBuffer.append(true);
       // append方法底层在进行追加的时候，如果byte数组满了，会自动扩容。
       stringBuffer.append(100L);

       System.out.println(stringBuffer.toString());

       // 指定初始化容量的StringBuffer对象（字符串缓冲区对象）
       StringBuffer sb = new StringBuffer(100);
       sb.append("hello");
       sb.append("world");
       sb.append("hello");
       sb.append("kitty");

       System.out.println(sb);
   }
}

```
#### 4.1.30.6 StringBuffer和StringBuilder的区别
```java
package com.yxj.javase.stringbuffer;

/*
java.lang.StringBuilder
StringBuffer和StringBuilder的区别？
   StringBuffer中的方法都有：synchronized关键字修饰。表示StringBuffer在多线程环境下运行是安全的。
   StringBuilder中的方法都没有：synchronized关键字修饰，表示StringBuilder在多线程环境下运行是不安全的。

   StringBuffer是线程安全的。
   StringBuilder是非线程安全的。
   
*/
public class StringBuilderTest01 {
   public static void main(String[] args) {

       // 使用StringBuilder也是可以完成字符串的拼接。
       StringBuilder sb = new StringBuilder();
       sb.append(100);
       sb.append(true);
       sb.append("hello");
       sb.append("kitty");
       System.out.println(sb);
   }
}

````

```java
String与StringBuilder/StringBuffer之间的区别?
package com.yxj.javase.stringbuffer;
/*
1、面试题：String为什么是不可变的？
   我看过源代码，String类中有一个byte[]数组，这个byte[]数组采用了final修饰，
   因为数组一旦创建长度不可变。并且被final修饰的引用一旦指向某个对象之后，不
   可再指向其它对象，所以String是不可变的！
       "abc" 无法变成 "abcd"

2、StringBuilder/StringBuffer为什么是可变的呢？
   我看过源代码，StringBuffer/StringBuilder内部实际上是一个byte[]数组，
   这个byte[]数组没有被final修饰，StringBuffer/StringBuilder的初始化
   容量我记得应该是16，当存满之后会进行扩容，底层调用了数组拷贝的方法
   System.arraycopy()...是这样扩容的。所以StringBuilder/StringBuffer
   适合于使用字符串的频繁拼接操作。

*/
public class StringBufferTest04 {
   public static void main(String[] args) {

       // 字符串不可变是什么意思？
       // 是说双引号里面的字符串对象一旦创建不可变。
       String s = "abc"; //"abc"放到了字符串常量池当中。"abc"不可变。

       // s变量是可以指向其它对象的。
       // 字符串不可变不是说以上变量s不可变。说的是"abc"这个对象不可变。
       s = "xyz";//"xyz"放到了字符串常量池当中。"xyz"不可变。

   }
}

```
### 4.1.31 包装类原理实现
```java
package com.yxj.javase.integer;
/*
1、java中为8种基本数据类型又对应准备了8种包装类型。8种包装类属于引用数据类型，父类是Object。
2、思考：为什么要再提供8种包装类呢？
   因为8种基本数据类型不够用。
   所以SUN又提供对应的8种包装类型。
*/
public class IntegerTest01 {

   //入口
   public static void main(String[] args) {

       // 有没有这种需求：调用doSome()方法的时候需要传一个数字进去。
       // 但是数字属于基本数据类型，而doSome()方法参数的类型是Object。
       // 可见doSome()方法无法接收基本数据类型的数字。那怎么办呢？可以传一个数字对应的包装类进去。

       // 把100这个数字经过构造方法包装成对象。
       MyInt myInt = new MyInt(100);
       // doSome()方法虽然不能直接传100，但是可以传一个100对应的包装类型。
       doSome(myInt);
   }

   public static void doSome(Object obj){
       //System.out.println(obj);
       System.out.println(obj.toString());
   }
}

```
```java
package com.yxj.javase.integer;
// 这种包装类目前是我自己写的。实际开发中我们不需要自己写。
// 8种基本数据类型对应的8种包装类，SUN公司已经写好了。我们直接用。
public class MyInt {
   int value;

   public MyInt() {
   }

   public MyInt(int value) {
       this.value = value;
   }

   @Override
   public String toString() {
       return String.valueOf(value);
   }
}

```
#### 4.1.31.1 八种包装类
```markdown
关于Integer类的构造方法，有两个：
Integer(int)
Integer(String)
System.out.println("int的最大值：" + Integer.MAX_VALUE);
System.out.println("int的最小值：" + Integer.MIN_VALUE);
System.out.println("byte的最大值：" + Byte.MAX_VALUE);
System.out.println("byte的最小值：" + Byte.MIN_VALUE);
其他各种Number类型依葫芦画瓢.
```
```java
package com.yxj.javase.integer;

/*
1、8种基本数据类型对应的包装类型名是什么？
   基本数据类型              包装类型
   -------------------------------------
   byte                    java.lang.Byte（父类Number）
   short                   java.lang.Short（父类Number）
   int                     java.lang.Integer（父类Number）
   long                    java.lang.Long（父类Number）
   float                   java.lang.Float（父类Number）
   double                  java.lang.Double（父类Number）
   boolean                 java.lang.Boolean（父类Object）
   char                    java.lang.Character（父类Object）

2、八种包装类中其中6个都是数字对应的包装类，他们的父类都是Number，可以先研究一下Number中公共的方法：
   Number是一个抽象类，无法实例化对象。
   Number类中有这样的方法：
       byte byteValue() 以 byte 形式返回指定的数值。
       abstract  double doubleValue()以 double 形式返回指定的数值。
       abstract  float floatValue()以 float 形式返回指定的数值。
       abstract  int intValue()以 int 形式返回指定的数值。
       abstract  long longValue()以 long 形式返回指定的数值。
       short shortValue()以 short 形式返回指定的数值。
       这些方法其实所有的数字包装类的子类都有，这些方法是负责拆箱的。

*/
public class IntegerTest02 {
   public static void main(String[] args) {

     
       // 基本数据类型 -(转换为)->引用数据类型（装箱）
       Integer i = new Integer(123);

       // 将引用数据类型--(转换为)-> 基本数据类型
       float f = i.floatValue();
       System.out.println(f); //123.0

       // 将引用数据类型--(转换为)-> 基本数据类型（拆箱）
       int retValue = i.intValue();
       System.out.println(retValue); //123
   }
}

```
#### 4.1.31.2 自动装箱跟自动拆箱
```java
package com.yxj.javase.integer;

/**
* 好消息：在java5之后，引入了一种新特性，自动装箱和自动拆箱
*  自动装箱：基本数据类型自动转换成包装类。
*  自动拆箱：包装类自动转换成基本数据类型。
*
* 有了自动拆箱之后，Number类中的方法就用不着了！
*
* 自动装箱和自动拆箱的好处？
*      方便编程。
*/
public class IntegerTest05 {
   public static void main(String[] args) {

       // 900是基本数据类型
       // x是包装类型
       // 基本数据类型 --(自动转换)--> 包装类型：自动装箱
       Integer x = 900;
       System.out.println(x);

       // x是包装类型
       // y是基本数据类型
       // 包装类型 --(自动转换)--> 基本数据类型：自动拆箱
       int y = x;
       System.out.println(y);

       // z是一个引用，z是一个变量，z还是保存了一个对象的内存地址。
       Integer z = 1000; // 等同于：Integer z = new Integer(1000);
       // 分析为什么这个没有报错呢？
       // +两边要求是基本数据类型的数字，z是包装类，不属于基本数据类型，这里会进行自动拆箱。将z转换成基本数据类型
       // 在java5之前你这样写肯定编译器报错。
       System.out.println(z + 1);

       Integer a = 1000; // Integer a = new Integer(1000); a是个引用，保存内存地址指向对象。
       Integer b = 1000; // Integer b = new Integer(1000); b是个引用，保存内存地址指向对象。
       // == 比较的是对象的内存地址，a和b两个引用中保存的对象内存地址不同。
       // == 这个运算符不会触发自动拆箱机制。（只有+ - * /等运算的时候才会。）
       System.out.println(a == b); //false
   }
}

```

```java
面试题
package com.yxj.javase.integer;

/*
这个题目是Integer非常重要的面试题。
*/
public class IntegerTest06 {
   public static void main(String[] args) {

       Integer a = 128;
       Integer b = 128;
       System.out.println(a == b); //false

       /*
       java中为了提高程序的执行效率，将[-128到127]之间所有的包装对象提前创建好，
       放到了一个方法区的“整数型常量池”当中了，目的是只要用这个区间的数据不需要
       再new了，直接从整数型常量池当中取出来。(缓冲机制,把访问效率高的数据放到内存中,
       下次访问直接从缓存中拿).

       原理：x变量中保存的对象的内存地址和y变量中保存的对象的内存地址是一样的。
        */
       Integer x = 127;
       Integer y = 127;
       // == 永远判断的都是两个对象的内存地址是否相同。
       System.out.println(x == y); //true
   }
}

```
#### 4.1.31.3  String int Integer之间的转换

```markdown
1.int和Integer之间的转换：

1） int----->Integer

1 自动装箱

2 Integer的构造方法

3 调用Integer的静态方法：static Integer valueOf(int i)：返回一个指定int值的Integer对象

代码如下：

int a = 10;
Integer i1 = a; //1 
Integer i2 = new Integer(a);  //2 
Integer i3 = Integer.valueOf(a);   //3 

2） Integer------>int            

1 自动拆箱

2 调用Integer的方法：int intValue()：以int类型返回该Integer的值

示例代码：

Integer a = new Integer(10);

int i1 = a;   //1
int i2 = a.intValue();   //2

2.String和Integer之间的转换：

1） Integer---->String

1 调用Integer的方法：String toString()：返回该Integer对象的字符串形式

3 调用Integer的静态方法：static String toString(int i)：返回一个指定整数的String对象

3 调用String的静态方法：static String valueOf(Object obj)：返回任意类型的字符串形式

示例代码：

Integer a = new Integer(20);

String str1 = a.toString();   //1 

String str2 = Integer.toString(a);  //2 

String str3 = String.valueOf(a);   //3

2） String---->Integer

1 调用Integer的静态方法：static Integer valueOf(String s)：返回指定的 String 的值的 Integer 对象。

注意：这里的参数s必须是可以解析成整数的字符串，否则会报异常：NumberFormatException

示例代码：

String str = "123";
Integer i = Integer.valueOf(str);  //1

3.int和String之间的转换：

1） int------>String

1 字符串拼接，使用+

2 调用Integer的静态方法：static String toString(int i)：返回一个指定整数的String对象

3 调用String的静态方法：static String valueOf(int i)：返回指定int值的字符串形式

示例代码：

int a = 5;
String s1 = a +""; //1
String s3 = Integer.toString(a);  //2
String s2 = String.valueOf(a);   //3

2） String----->int

1 调用Integer的静态方法：static int parseInt(String s)：将一个可以解析为整数的字符串解析为一个int值

2 调用Integer的静态方法：static Integer valueOf(String s)：返回指定的 String 的值的 Integer 对象。【自动拆箱】

示例代码：

String str = "123";
int m1 = Integer.parseInt(str);  //1
int m2 = Integer.valueOf(str);   //1--->自动拆箱
int m3 = Integer.valueOf(str).intValue();  //2--->手动拆箱
```
#### 4.1.31.4 日期的格式化

```java
package com.yxj.javase.date;

import java.text.SimpleDateFormat;
import java.util.Date;

/*
java中对日期的处理
   这个案例最主要掌握：
       知识点1：怎么获取系统当前时间
       知识点2：String ---> Date
       知识点3：Date ---> String
*/
public class DateTest01 {
   public static void main(String[] args) throws Exception {

       // 获取系统当前时间（精确到毫秒的系统当前时间）
       // 直接调用无参数构造方法就行。
       Date nowTime = new Date();

       // java.util.Date类的toString()方法已经被重写了。
       // 输出的应该不是一个对象的内存地址，应该是一个日期字符串。
       //System.out.println(nowTime); //Thu Mar 05 10:51:06 CST 2020

       // 日期可以格式化吗？
       // 将日期类型Date，按照指定的格式进行转换：Date --转换成具有一定格式的日期字符串-->String
       // SimpleDateFormat是java.text包下的。专门负责日期格式化的。
       /*
       yyyy 年(年是4位)
       MM 月（月是2位）
       dd 日
       HH 时
       mm 分
       ss 秒
       SSS 毫秒（毫秒3位，最高999。1000毫秒代表1秒）
       注意：在日期格式中，除了y M d H m s S这些字符不能随便写之外，剩下的符号格式自己随意组织。
        */
       SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss SSS");
       //SimpleDateFormat sdf = new SimpleDateFormat("dd/MM/yyyy");
       //SimpleDateFormat sdf = new SimpleDateFormat("yy/MM/dd HH:mm:ss");

       String nowTimeStr = sdf.format(nowTime);
       System.out.println(nowTimeStr);

       // 假设现在有一个日期字符串String，怎么转换成Date类型？
       // String --> Date
       String time = "2008-08-08 08:08:08 888";
       //SimpleDateFormat sdf2 = new SimpleDateFormat("格式不能随便写，要和日期字符串格式相同");
       // 注意：字符串的日期格式和SimpleDateFormat对象指定的日期格式要一致。不然会出现异常：java.text.ParseException
       SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss SSS");
       Date dateTime = sdf2.parse(time);
       System.out.println(dateTime); //Fri Aug 08 08:08:08 CST 2008

   }
}

```
```java
package com.yxj.javase.date;
/*
获取自1970年1月1日 00:00:00 000到当前系统时间的总毫秒数。
1秒 = 1000毫秒

简单总结一下System类的相关属性和方法：
   System.out 【out是System类的静态变量。】
   System.out.println() 【println()方法不是System类的，是PrintStream类的方法。】
   System.gc() 建议启动垃圾回收器
   System.currentTimeMillis() 获取自1970年1月1日到系统当前时间的总毫秒数。
   System.exit(0) 退出JVM。
*/
public class DateTest02 {
   public static void main(String[] args) {
       // 获取自1970年1月1日 00:00:00 000到当前系统时间的总毫秒数。
       long nowTimeMillis = System.currentTimeMillis();
       System.out.println(nowTimeMillis); //1583377912981

       // 统计一个方法耗时
       // 在调用目标方法之前记录一个毫秒数
       long begin = System.currentTimeMillis();
       print();
       // 在执行完目标方法之后记录一个毫秒数
       long end = System.currentTimeMillis();
       System.out.println("耗费时长"+(end - begin)+"毫秒");
   }

   // 需求：统计一个方法执行所耗费的时长
   public static void print(){
       for(int i = 0; i < 1000000000; i++){
           System.out.println("i = " + i);
       }
   }
}

```
```java
package com.yxj.javase.date;

import java.text.SimpleDateFormat;
import java.util.Date;

public class DateTest03 {
   public static void main(String[] args) {

       // 这个时间是什么时间？
       // 1970-01-01 00:00:00 001
       Date time = new Date(1); // 注意：参数是一个毫秒

       SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss SSS");
       String strTime = sdf.format(time);
       // 北京是东8区。差8个小时。
       System.out.println(strTime); // 1970-01-01 08:00:00 001

       // 获取昨天的此时的时间。
       Date time2 = new Date(System.currentTimeMillis() - 1000 * 60 * 60 * 24);
       String strTime2 = sdf.format(time2);
       System.out.println(strTime2); //2020-03-04 11:44:14 829

   }
}

```
#### 4.1.31.5 数字格式化
```java
package com.yxj.javase.number;

import java.math.BigDecimal;

/*
1、BigDecimal 属于大数据，精度极高。不属于基本数据类型，属于java对象（引用数据类型）
这是SUN提供的一个类。专门用在财务软件当中。

2、注意：财务软件中double是不够的。咱们之前有一个学生去用友面试，经理就问了这样一个问题：
   你处理过财务数据吗？用的哪一种类型？
       千万别说double，说java.math.BigDecimal
*/
public class BigDecimalTest01 {
   public static void main(String[] args) {

       // 这个100不是普通的100，是精度极高的100
       BigDecimal v1 = new BigDecimal(100);
       // 精度极高的200
       BigDecimal v2 = new BigDecimal(200);
       // 求和
       // v1 + v2; // 这样不行，v1和v2都是引用，不能直接使用+求和。
       BigDecimal v3 = v1.add(v2); // 调用方法求和。
       System.out.println(v3); //300

       BigDecimal v4 = v2.divide(v1);
       System.out.println(v4); // 2
   }
}
```
```java
package com.yxj.javase.number;

import java.text.DecimalFormat;

/*
关于数字的格式化。（了解）
*/
public class DecimalFormatTest01 {
   public static void main(String[] args) {
       // java.text.DecimalFormat专门负责数字格式化的。
       //DecimalFormat df = new DecimalFormat("数字格式");

       /*
       数字格式有哪些？
           # 代表任意数字
           , 代表千分位
           . 代表小数点
           0 代表不够时补0

           ###,###.##
               表示：加入千分位，保留2个小数。
        */
       DecimalFormat df = new DecimalFormat("###,###.##");
       //String s = df.format(1234.56);
       String s = df.format(1234.561232);
       System.out.println(s); // "1,234.56"

       DecimalFormat df2 = new DecimalFormat("###,###.0000"); //保留4个小数位，不够补上0
       String s2 = df2.format(1234.56);
       System.out.println(s2); // "1,234.5600"

   }
}

```
#### 4.1.31.6 random
```java
package com.yxj.javase.random;

import java.util.Random;

/**
* 随机数
*/
public class RandomTest01 {
   public static void main(String[] args) {
       // 创建随机数对象
       Random random = new Random();

       // 随机产生一个int类型取值范围内的数字。
       int num1 = random.nextInt();

       System.out.println(num1);

       // 产生[0~100]之间的随机数。不能产生101。
       // nextInt翻译为：下一个int类型的数据是101，表示只能取到100.
       int num2 = random.nextInt(101); //不包括101
       System.out.println(num2);
   }
}

```
#### 4.1.31.7 enum
```java
package com.yxj.javase.enum2;

// 采用枚举的方式改造程序
/*
总结：
   1、枚举是一种引用数据类型
   2、枚举类型怎么定义，语法是？
       enum 枚举类型名{
           枚举值1,枚举值2
       }
   3、结果只有两种情况的，建议使用布尔类型。
   结果超过两种并且还是可以一枚一枚列举出来的，建议使用枚举类型。
       例如：颜色、四季、星期等都可以使用枚举类型。
*/
public class EnumTest02 {
   public static void main(String[] args) {
       Result r = divide(10, 2);
       System.out.println(r == Result.SUCCESS ? "计算成功" : "计算失败");
   }

   /**
    * 计算两个int类型数据的商。
    * @param a int数据
    * @param b int数据
    * @return Result.SUCCESS表示成功，Result.FAIL表示失败！
    */
   public static Result divide(int a, int b){
       try {
           int c = a / b;
           return Result.SUCCESS;
       } catch (Exception e){
           return Result.FAIL;
       }
   }
}

// 枚举：一枚一枚可以列举出来的，才建议使用枚举类型。
// 枚举编译之后也是生成class文件。
// 枚举也是一种引用数据类型。
// 枚举中的每一个值可以看做是常量。
enum Result{
   // SUCCESS 是枚举Result类型中的一个值
   // FAIL 是枚举Result类型中的一个值
   // 枚举中的每一个值，可以看做是“常量”
   SUCCESS, FAIL
}
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复java1
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
