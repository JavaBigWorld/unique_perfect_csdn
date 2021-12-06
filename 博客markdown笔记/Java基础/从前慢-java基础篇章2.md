# java基础篇章2
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210715000617943.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1.1 异常
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200527090816742.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdownmarkdown
java的异常处理机制
Object
Object下有Throwable（可抛出的）
Throwable下有两个分支：Error（不可处理，直接退出JVM）和
Exception（可处理的）
Exception下有两个分支：
编译时异常（受检异常：CheckedException、受控异常）：
要求程序员在编写程序阶段必须预先对这些异常进行处理，
如果不处理编译器报错，因此得名编译时异常。
包括Exception，但不包括其子类RuntimeException
运行时异常（未受检异常：UnCheckedException、非受控异常）：
在编写程序阶段程序员可以预先处理，也可以不管，都行
包括：RuntimeException及其子类

编译时异常和运行时异常，都是发生在运行阶段。编译阶段
异常是不会发生的。
编译时异常因为什么而得名？
因为编译时异常必须在编译(编写)阶段预先处理，如果不处理编
译器报错，因此得名。
所有异常都是在运行阶段发生的。因为只有程序运行阶段才
可以new对象。
因为异常的发生就是new异常对象。

编译时异常和运行时异常的区别？
编译时异常一般发生的概率比较高。
运行时异常一般发生的概率比较低。

Java语言中对异常的处理包括两种方式：
第一种方式：在方法声明的位置上，使用throws关键字，抛给上一级。
谁调用我，我就抛给谁。抛给上一级。

第二种方式：使用try..catch语句进行异常的捕捉。
这件事发生了，谁也不知道，因为我给抓住了。

注意：Java中异常发生之后如果一直上抛，最终抛给了main方
法，main方法继续
向上抛，抛给了调用者JVM，JVM知道这个异常发生，只有
一个结果。终止java程序的执行。
```
```markdown
手动new出异常跟正常代码一样,不会影响下面代码的执行.
```
```java
import java.io.IOException;

public class Test {
   public static void main(String[] args) {
   	
   	aa();
   	System.out.println("Hello World");
   }
   
  static void aa() {
   	IOException ioException = new IOException("手动new出异常");
   }
}
```
```markdown
运行结果:Hello World
```
```markdown
非手动new出异常则底层会帮我们产生相应的异常,然
后抛给调用者,如果调用者不处理,则运行会报错,下面
的代码就不会执行
```

```java
未处理
public class Test {
   public static void main(String[] args) {
   	
   	aa();//因为这里没有处理java.lang.ArithmeticException,所以抛给JVM,结果就是直接报错,下面的代码不会运行
   	System.out.println("Hello World");
   }
   
  static void aa() {
   //这里底层会产生java.lang.ArithmeticException,然后抛给main方法这个调用者
   	int i = 1/0; 
   }
}

```
```markdown
运行结果:Exception in thread "main" java.lang.ArithmeticException: / by zero
```

```java
处理后
public class Test {
   public static void main(String[] args) {
   	try {
   		aa();//这里处理java.lang.ArithmeticException,下面代码就会运行,这里不包含跟try同一个块级的语句。
   		System.out.println("Hello World1"); //这里不执行
	} catch (Exception e) { //如果这里不是ArithmeticException或者ArithmeticException的父类，则会报错，跟没处理一样
		e.printStackTrace();
	}
   	
   	System.out.println("Hello World2");
   	
   }
   
  static void aa() {
   //这里底层会产生java.lang.ArithmeticException,然后抛给main方法这个调用者
   	int i = 1/0; 
   }
}
```
```markdown
运行结果:
java.lang.ArithmeticException: / by zero
at com.yao.Test.aa(Test.java:18)
at com.yao.Test.main(Test.java:6)
Hello World2
```
### 1.1.1  对异常的进一步理解
```java
package com.yxj.javase.exception;

public class ExceptionTest04 {
   public static void main(String[] args) {
       // main方法中调用doSome()方法
       // 因为doSome()方法声明位置上有：throws ClassNotFoundException
       // 我们在调用doSome()方法的时候必须对这种异常进行预先的处理。
       // 如果不处理，编译器就报错。
       //编译器报错信息： Unhandled exception: java.lang.ClassNotFoundException
       //doSome();
   }

   /**
    * doSome方法在方法声明的位置上使用了：throws ClassNotFoundException
    * 这个代码表示doSome()方法在执行过程中，有可能会出现ClassNotFoundException异常。
    * 叫做类没找到异常。这个异常直接父类是：Exception，所以ClassNotFoundException属于编译时异常。
    * @throws ClassNotFoundException
    */
   public static void doSome() throws ClassNotFoundException{
       System.out.println("doSome!!!!");
   }

}

```
```java
package com.yxj.javase.exception;

public class ExceptionTest05 {
   // 第一种处理方式：在方法声明的位置上继续使用：throws，来完成异常的继续上抛。抛给调用者。
   // 上抛类似于推卸责任。（继续把异常传递给调用者。）
   /*
   public static void main(String[] args) throws ClassNotFoundException {
       doSome();
   }
    */

   // 第二种处理方式：try..catch进行捕捉。
   // 捕捉等于把异常拦下了，异常真正的解决了。（调用者是不知道的。）
   public static void main(String[] args) {
       try {
           doSome();
       } catch (ClassNotFoundException e) {
           e.printStackTrace();
       }
   }

   public static void doSome() throws ClassNotFoundException{
       System.out.println("doSome!!!!");
   }

}

```
```java
package com.yxj.javase.exception;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

注意：
   只要异常没有捕捉，采用上抛的方式，此方法的后续代码不会执行。
   另外需要注意，try语句块中的某一行出现异常，该行后面的代码不会执行。
   try..catch捕捉异常之后，后续代码可以执行。

在以后的开发中，处理编译时异常，应该上抛还是捕捉呢，怎么选？
如果希望调用者来处理，选择throws上抛。
其它情况使用捕捉的方式。
*/
public class ExceptionTest06 {
   // 一般不建议在main方法上使用throws，因为这个异常如果真正的发生了，一定会抛给JVM。JVM只有终止。
   // 异常处理机制的作用就是增强程序的健壮性。怎么能做到，异常发生了也不影响程序的执行。所以
   // 一般main方法中的异常建议使用try..catch进行捕捉。main就不要继续上抛了。
   /*
   public static void main(String[] args) throws FileNotFoundException {
       System.out.println("main begin");
       m1();
       System.out.println("main over");
   }
    */
   public static void main(String[] args) {

       // 100 / 0这是算术异常，这个异常是运行时异常，你在编译阶段，可以处理，也可以不处理。编译器不管。
       //System.out.println(100 / 0); // 不处理编译器也不管
       // 你处理也可以。
       /*
       try {
           System.out.println(100 / 0);
       } catch(ArithmeticException e){
           System.out.println("算术异常了！！！！");
       }
        */

       System.out.println("main begin");
       try {
           // try尝试
           m1();
           // 以上代码出现异常，直接进入catch语句块中执行。
           System.out.println("hello world!");
       } catch (FileNotFoundException e){ // catch后面的好像一个方法的形参。
           // 这个分支中可以使用e引用，e引用保存的内存地址是那个new出来异常对象的内存地址。
           // catch是捕捉异常之后走的分支。
           // 在catch分支中干什么？处理异常。
           System.out.println("文件不存在，可能路径错误，也可能该文件被删除了！");
           System.out.println(e); //java.io.FileNotFoundException: D:\course\01-课\学习方法.txt (系统找不到指定的路径。)
       }

       // try..catch把异常抓住之后，这里的代码会继续执行。
       System.out.println("main over");
   }

   private static void m1() throws FileNotFoundException {
       System.out.println("m1 begin");
       m2();
       // 以上代码出异常，这里是无法执行的。
       System.out.println("m1 over");
   }

   // 抛别的不行，抛ClassCastException说明你还是没有对FileNotFoundException进行处理
   //private static void m2() throws ClassCastException{
   // 抛FileNotFoundException的父对象IOException，这样是可以的。因为IOException包括FileNotFoundException
   //private static void m2() throws IOException {
   // 这样也可以，因为Exception包括所有的异常。
   //private static void m2() throws Exception{
   // throws后面也可以写多个异常，可以使用逗号隔开。
   //private static void m2() throws ClassCastException, FileNotFoundException{
   private static void m2() throws FileNotFoundException {
       System.out.println("m2 begin");
       // 编译器报错原因是：m3()方法声明位置上有：throws FileNotFoundException
       // 我们在这里调用m3()没有对异常进行预处理，所以编译报错。
       // m3();

       m3();
       // 以上如果出现异常，这里是无法执行的！
       System.out.println("m2 over");
   }

   private static void m3() throws FileNotFoundException {
      
    /*   编译报错的原因是什么？
           第一：这里调用了一个构造方法：FileInputStream(String name)
           第二：这个构造方法的声明位置上有：throws FileNotFoundException
           第三：通过类的继承结构看到：FileNotFoundException父类是IOException，IOException的父类是Exception，
           最终得知，FileNotFoundException是编译时异常。

           错误原因？编译时异常要求程序员编写程序阶段必须对它进行处理，不处理编译器就报错。
     */
       //new FileInputStream("D:\\course\\01-开课\\学习方法.txt");

       // 我们采用第一种处理方式：在方法声明的位置上使用throws继续上抛。
       // 一个方法体当中的代码出现异常之后，如果上抛的话，此方法结束。
       new FileInputStream("D:\\course\\01-课\\学习方法.txt");

       System.out.println("如果以上代码出异常，这里会执行吗??????????????????不会！！！");
   }
}

```
```java
package com.yxj.javase.exception;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

/*
深入try..catch
   1、catch后面的小括号中的类型可以是具体的异常类型，也可以是该异常类型的父类型。
   2、catch可以写多个。建议catch的时候，精确的一个一个处理。这样有利于程序的调试。
   3、catch写多个的时候，从上到下，必须遵守从小到大。
*/
public class ExceptionTest07 {
   /*
   public static void main(String[] args) throws Exception, FileNotFoundException, NullPointerException {

   }
    */

   /*public static void main(String[] args) throws Exception {

   }*/

   public static void main(String[] args) {

       //编译报错
       /*try {
           FileInputStream fis = new FileInputStream("D:\\course\\02-JavaSE\\document\\JavaSE进阶讲义\\JavaSE进阶-01-面向对象.pdf");
       } catch(NullPointerException e) {

       }*/

       /*try {
           FileInputStream fis = new FileInputStream("D:\\curse\\02-JavaSE\\document\\JavaSE进阶讲义\\JavaSE进阶-01-面向对象.pdf");
           System.out.println("以上出现异常，这里无法执行！");
       } catch(FileNotFoundException e) {
           System.out.println("文件不存在！");
       }

       System.out.println("hello world!");*/

       /*try {
           FileInputStream fis = new FileInputStream("D:\\curse\\02-JavaSE\\document\\JavaSE进阶讲义\\JavaSE进阶-01-面向对象.pdf");
       } catch(IOException e) { // 多态：IOException e = new FileNotFoundException();
           System.out.println("文件不存在！");
       }*/

       /*try {
           FileInputStream fis = new FileInputStream("D:\\curse\\02-JavaSE\\document\\JavaSE进阶讲义\\JavaSE进阶-01-面向对象.pdf");
       } catch(Exception e) { // 多态：Exception e = new FileNotFoundException();
           System.out.println("文件不存在！");
       }*/

       /*try {
           //创建输入流
           FileInputStream fis = new FileInputStream("D:\\curse\\02-JavaSE\\document\\JavaSE进阶讲义\\JavaSE进阶-01-面向对象.pdf");
           //读文件
           fis.read();
       } catch(Exception e) { //所有的异常都走这个分支。
           System.out.println("文件不存在！");
       }*/

       /*try {
           //创建输入流
           FileInputStream fis = new FileInputStream("D:\\curse\\02-JavaSE\\document\\JavaSE进阶讲义\\JavaSE进阶-01-面向对象.pdf");
           //读文件
           fis.read();
       } catch(FileNotFoundException e) {
           System.out.println("文件不存在！");
       } catch(IOException e){
           System.out.println("读文件报错了！");
       }*/

       // 编译报错。
       /*
       try {
           //创建输入流
           FileInputStream fis = new FileInputStream("D:\\curse\\02-JavaSE\\document\\JavaSE进阶讲义\\JavaSE进阶-01-面向对象.pdf");
           //读文件
           fis.read();
       } catch(IOException e){
           System.out.println("读文件报错了！");
       } catch(FileNotFoundException e) {
           System.out.println("文件不存在！");
       }
        */

       // JDK8的新特性！
       try {
           //创建输入流
           FileInputStream fis = new FileInputStream("D:\\curse\\02-JavaSE\\document\\JavaSE进阶讲义\\JavaSE进阶-01-面向对象.pdf");
           // 进行数学运算
           System.out.println(100 / 0); // 这个异常是运行时异常，编写程序时可以处理，也可以不处理。
       } catch(FileNotFoundException | ArithmeticException | NullPointerException e) {
           System.out.println("文件不存在？数学异常？空指针异常？都有可能！");
       }
   }
}
```
### 1.1.2  获取异常的两个非常重要的方法
```java
package com.yxj.javase.exception;
/*
异常对象有两个非常重要的方法：

   获取异常简单的描述信息：
       String msg = exception.getMessage();

   打印异常追踪的堆栈信息：
       exception.printStackTrace();
*/
public class ExceptionTest08 {
   public static void main(String[] args) {
       // 这里只是为了测试getMessage()方法和printStackTrace()方法。
       // 这里只是new了异常对象，但是没有将异常对象抛出。JVM会认为这是一个普通的java对象。
       NullPointerException e = new NullPointerException("空指针异常fdsafdsafdsafds");

       // 获取异常简单描述信息：这个信息实际上就是构造方法上面String参数。
       String msg = e.getMessage(); //空指针异常fdsafdsafdsafds
       System.out.println(msg);

       // 打印异常堆栈信息
       // java后台打印异常堆栈追踪信息的时候，采用了异步线程的方式打印的。
       e.printStackTrace();

       for(int i = 0; i < 1000; i++){
           System.out.println("i = " + i);
       }

       System.out.println("Hello World!");
   }
}

```
### 1.1.3 关于try..catch中的finally子句
```java
package com.yxj.javase.exception;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

/*
关于try..catch中的finally子句：
   1、在finally子句中的代码是最后执行的，并且是一定会执行的，即使try语句块中的代码出现了异常。
       finally子句必须和try一起出现，不能单独编写。

   2、finally语句通常使用在哪些情况下呢？
       通常在finally语句块中完成资源的释放/关闭。
       因为finally中的代码比较有保障。
       即使try语句块中的代码出现异常，finally中代码也会正常执行。
*/
public class ExceptionTest10 {
   public static void main(String[] args) {
       FileInputStream fis = null; // 声明位置放到try外面。这样在finally中才能用。
       try {
           // 创建输入流对象
           fis = new FileInputStream("D:\\course\\02-JavaSE\\document\\JavaSE进阶讲义\\JavaSE进阶-01-面向对象.pdf");
           // 开始读文件....

           String s = null;
           // 这里一定会出现空指针异常！
           s.toString();
           System.out.println("hello world!");

           // 流使用完需要关闭，因为流是占用资源的。
           // 即使以上程序出现异常，流也必须要关闭！
           // 放在这里有可能流关不了。
           //fis.close();
       } catch (FileNotFoundException e) {
           e.printStackTrace();
       } catch(IOException e){
           e.printStackTrace();
       } catch(NullPointerException e) {
           e.printStackTrace();
       } finally {
           System.out.println("hello 浩克！");
           // 流的关闭放在这里比较保险。
           // finally中的代码是一定会执行的。
           // 即使try中出现了异常！
           if (fis != null) { // 避免空指针异常！
               try {
                   // close()方法有异常，采用捕捉的方式。
                   fis.close();
               } catch (IOException e) {
                   e.printStackTrace();
               }
           }
       }

       System.out.println("hello kitty!");

   }
}

```

```java
finally面试题
package com.yxj.javase.exception;

public class ExceptionTest13 {
   public static void main(String[] args) {
       int result = m();
       System.out.println(result); //100
   }

   /*
   java语法规则（有一些规则是不能破坏的，一旦这么说了，就必须这么做！）：
       java中有一条这样的规则：
           方法体中的代码必须遵循自上而下顺序依次逐行执行（亘古不变的语法！）
       java中还有一条语法规则：
           return语句一旦执行，整个方法必须结束（亘古不变的语法！）
    */
   public static int m(){
       int i = 100;
       try {
           // 这行代码出现在int i = 100;的下面，所以最终结果必须是返回100
           // return语句还必须保证是最后执行的。一旦执行，整个方法结束。
           return i;
       } finally {
           i++;
       }
   }
}

/*
反编译之后的效果
public static int m(){
   int i = 100;
   int j = i;
   i++;
   return j;
}
*/

```
### 1.1.4 final finally finalize有什么区别
```markdown
final 关键字
final修饰的类无法继承
final修饰的方法无法覆盖
final修饰的变量不能重新赋值。

finally 关键字
和try一起联合使用。
finally语句块中的代码是必须执行的。

finalize 标识符
是一个Object类中的方法名。
这个方法是由垃圾回收器GC负责调用的。
```
```java
package com.yxj.javase.exception;

public class ExceptionTest14 {
   public static void main(String[] args) {

       // final是一个关键字。表示最终的。不变的。
       final int i = 100;
       //i = 200;

       // finally也是一个关键字，和try联合使用，使用在异常处理机制中
       // 在fianlly语句块中的代码是一定会执行的。
       try {

       } finally {
           System.out.println("finally....");
       }

       // finalize()是Object类中的一个方法。作为方法名出现。
       // 所以finalize是标识符。
       // finalize()方法是JVM的GC垃圾回收器负责调用。
       Object obj;
   }
}

// final修饰的类无法继承
final class A {
   // 常量。
   public static final double MATH_PI = 3.1415926;
}

class B {
   // final修饰的方法无法覆盖
   public final void doSome(){

   }
}

```
### 1.1.5 异常总结
```java
public class ExceptionTest {
   public static void main(String[] args) {
   	
   	test();//如果这里不处理的话，下面的代码不会执行
   	System.out.println("不执行");
  
   }

   
   static void test() { //这里可以省略throws ArithmeticException，因为属于运行时异常，可以不用处理，自然也就可以省略了，结果一样
   	int i = 1/0;//java.lang.ArithmeticException: / by zero 这个属于运行时异常，底层会自动生成异常对象，然后抛给调用者
   }
}


-----------------------------------------------------------------------------
运行结果:Exception in thread "main" java.lang.ArithmeticException: / by zero
----------------------------------------------------------------------------

public class ExceptionTest {
   public static void main(String[] args) {
   	try {
   		test();
	} catch (Exception e) {
		// TODO: handle exception
	}
   	
   	System.out.println("执行");
  
   }

   
   static void test() {  //这里可以省略throws ArithmeticException，因为属于运行时异常，可以不用处理，自然也就可以省略了，结果一样
   	int i = 1/0;//java.lang.ArithmeticException: / by zero 这个属于运行时异常，底层会自动生成异常对象，然后抛给调用者
   }
}

-----------------------------------------------------------------------------
运行结果:执行
----------------------------------------------------------------------------

public class ExceptionTest {
   public static void main(String[] args) {
       //跟普通对象一样,不会影响代码的执行,不管是编译时异常还是运行时异常
   	NullPointerException nullPointerException = new NullPointerException(); 
   	ClassNotFoundException classNotFoundException = new ClassNotFoundException();
   	System.out.println("普通对象");
   	
  
   }

}

-----------------------------------------------------------------------------
运行结果:普通对象
----------------------------------------------------------------------------


public class ExceptionTest {
   public static void main(String[] args) {
   	test(); ////如果这里不处理的话，下面的代码不会执行
   	System.out.println("不执行");
  
   }

   
   static void test() {  //这里可以省略throws NullPointerException，因为属于运行时异常，可以不用处理，自然也就可以省略了，结果一样
   	throw new NullPointerException();//这个属于运行时异常，会手动抛给调用者
   }
}

----------------------------------------------------------------------------
运行结果:Exception in thread "main" java.lang.NullPointerException
----------------------------------------------------------------------------



public class ExceptionTest {
   public static void main(String[] args) {
   	try {
   		test(); 
	} catch (Exception e) {
		// TODO: handle exception
	}
   
   	System.out.println("执行");
  
   }

   
   static void test() {  //这里可以省略throws NullPointerException，因为属于运行时异常，可以不用处理，自然也就可以省略了，结果一样
   	throw new NullPointerException();//这个属于运行时异常，会手动抛给调用者
   }
}

----------------------------------------------------------------------------
运行结果:执行
----------------------------------------------------------------------------



public class ExceptionTest {
   public static void main(String[] args) {
   	 try {
		test(); //这里需要在编写时处理,因为接收到的是编译时异常,要么try---catch处理,要么throws,否则编译不通过.
	} catch (ClassNotFoundException e) {
		// TODO Auto-generated catch block
		
	}
   	 System.out.println("编译时异常");
   }

   
   static void test() throws ClassNotFoundException {  //如果采用throws,这里就不可以省略throws ClassNotFoundException，因为属于编译时异常，要处理，自然就不可以省略了
   	throw new ClassNotFoundException();//这个属于编译时异常，需要用try---catch处理或者throws
   }
}

----------------------------------------------------------------------------
运行结果:编译时异常
----------------------------------------------------------------------------

public class ExceptionTest {
   public static void main(String[] args) {
   	 test();
   	 System.out.println("执行");
   }

   
   static void test()  {  //如果采用throws,这里就不可以省略throws ClassNotFoundException，因为属于编译时异常，要处理，自然就不可以省略了
   	try {
		throw new ClassNotFoundException();
	} catch (ClassNotFoundException e) {
		// TODO Auto-generated catch block
		e.printStackTrace();
	}//这个属于编译时异常，需要用try---catch处理或者throws
   }
}

----------------------------------------------------------------------------
运行结果:java.lang.ClassNotFoundException
	执行
----------------------------------------------------------------------------


public class Test {
   public static void main(String[] args) {
   	try {
   		aa();//这里接受到的是ClassNotFoundException,处于编译时异常，不处理编译器报错
   		System.out.println("Hello World1"); //这里不执行
	} catch (NullPointerException e) { //没处理ClassNotFoundException
		e.printStackTrace();
	}
   	
   	System.out.println("Hello World2");
   	
   }
   
  static void aa() throws ClassNotFoundException {
   //这里底层会产生java.lang.ArithmeticException,然后抛给main方法这个调用者
   	int i = 1/0; 
   }
}


----------------------------------------------------------------------------
编译不通过,没运行结果.
----------------------------------------------------------------------------
```
## 2.1 集合
```markdown
集合概述
集合不能直接存储基本数据类型，另外集合也不能直接存储java对象，
集合当中存储的都是java对象的内存地址。（或者说集合中存
储的是引用。）
list.add(100); //自动装箱Integer
注意：
集合在java中本身是一个容器，是一个对象。
集合中任何时候存储的都是“引用”。



1、掌握Map接口中常用方法。

2、遍历Map集合的两种方式都要精通。
第一种：获取所有key，遍历每个key，通过key获取value.
第二种：获取Set<Map.Entry>即可，遍历Set集合中的Entry
	调用entry.getKey() entry.getValue()

3、了解哈希表数据结构。

4、存放在HashMap集合key部分和HashSet集合中的元
素需要同时重写hashCode和equals。

5、HashMap和Hashtable的区别。
HashMap：
	初始化容量16，扩容2倍。
	加载因子为0.75f
	非线程安全
	key和value可以为null。

Hashtable
	初始化容量11，扩容2倍+1
	加载因子为0.75f
	线程安全
	key和value都不能是null。

6、Properties类的常用两个方法。
setProperty
getProperty

8、TreeMap的key或者TreeSet集合中的元素要想排序，有
两种实现方式：
第一种：实现java.lang.Comparable接口。
第二种：单独编写一个比较器Comparator接口。

9、集合工具类Collections:
synchronizedList方法
sort方法（要求集合中元素实现Comparable接口。）



```
### 2.1.1  Collection
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200528121253244.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 2.1.2 Map 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200528121410499.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 2.1.3  Collection中的常用方法
```java
package com.yxj.javase.collection;

import java.util.ArrayList;
import java.util.Collection;

/*
关于java.util.Collection接口中常用的方法。
   1、Collection中能存放什么元素？
       没有使用“泛型”之前，Collection中可以存储Object的所有子类型。
       使用了“泛型”之后，Collection中只能存储某个具体的类型。
     
   2、Collection中的常用方法
       boolean add(Object e) 向集合中添加元素
       int size()  获取集合中元素的个数
       void clear() 清空集合
       boolean contains(Object o) 判断当前集合中是否包含元素o，包含返回true，不包含返回false
       boolean remove(Object o) 删除集合中的某个元素。
       boolean isEmpty()  判断该集合中元素的个数是否为0
       Object[] toArray()  调用这个方法可以把集合转换成数组。【作为了解，使用不多。】
*/
public class CollectionTest01 {
   public static void main(String[] args) {
       // 创建一个集合对象
       //Collection c = new Collection(); // 接口是抽象的，无法实例化。
       // 多态
       Collection c = new ArrayList();
       // 测试Collection接口中的常用方法
       c.add(1200); // 自动装箱(java5的新特性。),实际上是放进去了一个对象的内存地址。Integer x = new Integer(1200);
       c.add(3.14); // 自动装箱
       c.add(new Object());
       c.add(new Student());
       c.add(true); // 自动装箱

       // 获取集合中元素的个数
       System.out.println("集合中元素个数是：" + c.size()); // 5

       // 清空集合
       c.clear();
       System.out.println("集合中元素个数是：" + c.size()); // 0

       // 再向集合中添加元素
       c.add("hello"); // "hello"对象的内存地址放到了集合当中。
       c.add("world");
       c.add("浩克");
       c.add("绿巨人");
       c.add(1);

       // 判断集合中是否包含"绿巨人"
       boolean flag = c.contains("绿巨人");
       System.out.println(flag); // true
       boolean flag2 = c.contains("绿巨人2");
       System.out.println(flag2); // false
       System.out.println(c.contains(1)); // true

       System.out.println("集合中元素个数是：" + c.size()); // 5

       // 删除集合中某个元素
       c.remove(1);
       System.out.println("集合中元素个数是：" + c.size()); // 4

       // 判断集合是否为空（集合中是否存在元素）
       System.out.println(c.isEmpty()); // false
       // 清空
       c.clear();
       System.out.println(c.isEmpty()); // true（true表示集合中没有元素了！）

       c.add("abc");
       c.add("def");
       c.add(100);
       c.add("helloworld!");
       c.add(new Student());

       // 转换成数组（了解，使用不多。）
       Object[] objs = c.toArray();
       for(int i = 0; i < objs.length; i++){
           // 遍历数组
           Object o = objs[i];
           System.out.println(o);
       }
   }
}

class Student{

}

```
```java
package com.yxj.javase.collection;

import java.util.ArrayList;
import java.util.Collection;
import java.util.HashSet;
import java.util.Iterator;

/*
关于集合的迭代/遍历
*/
public class CollectionTest03 {
   public static void main(String[] args) {
       // 创建集合对象
       Collection c1  = new ArrayList(); // ArrayList集合：有序可重复
       // 添加元素
       c1.add(1);
       c1.add(2);
       c1.add(3);
       c1.add(4);
       c1.add(1);

       // 迭代集合
       Iterator it = c1.iterator();
       while(it.hasNext()){
           // 存进去是什么类型，取出来还是什么类型。
           Object obj = it.next();
           /*if(obj instanceof Integer){
               System.out.println("Integer类型");
           }*/
           // 只不过在输出的时候会转换成字符串。因为这里println会调用toString()方法。
           System.out.println(obj);
       }

       // HashSet集合：无序不可重复
       Collection c2 = new HashSet();
       // 无序：存进去和取出的顺序不一定相同。
       // 不可重复：存储100，不能再存储100.
       c2.add(100);
       c2.add(200);
       c2.add(300);
       c2.add(90);
       c2.add(400);
       c2.add(50);
       c2.add(60);
       c2.add(100);
       Iterator it2 = c2.iterator();
       while(it2.hasNext()){
           System.out.println(it2.next());
       }
   }
}


```
### 2.1.4 深入Collection集合的contains方法
```java
package com.yxj.javase.collection;

import java.util.ArrayList;
import java.util.Collection;

/*
深入Collection集合的contains方法：
   boolean contains(Object o)
       判断集合中是否包含某个对象o
       如果包含返回true， 如果不包含返回false。

   contains方法是用来判断集合中是否包含某个元素的方法，
   那么它在底层是怎么判断集合中是否包含某个元素的呢？
       调用了equals方法进行比对。
       equals方法返回true，就表示包含这个元素。
*/
public class CollectionTest04 {
   public static void main(String[] args) {
       // 创建集合对象
       Collection c = new ArrayList();

       // 向集合中存储元素
       String s1 = new String("abc"); // s1 = 0x1111
       c.add(s1); // 放进去了一个"abc"

       String s2 = new String("def"); // s2 = 0x2222
       c.add(s2);

       // 集合中元素的个数
       System.out.println("元素的个数是：" + c.size()); // 2

       // 新建的对象String
       String x = new String("abc"); // x = 0x5555
       // c集合中是否包含x？结果猜测一下是true还是false？
       System.out.println(c.contains(x)); //判断集合中是否存在"abc" true
   }
}

```
```java
package com.yxj.javase.collection;

import java.util.ArrayList;
import java.util.Collection;

/*
测试contains方法
测试remove方法。
结论：存放在一个集合中的类型，一定要重写equals方法。
*/
public class CollectionTest05 {
   public static void main(String[] args) {
       // 创建集合对象
       Collection c = new ArrayList();
       // 创建用户对象
       User u1 = new User("jack");
       // 加入集合
       c.add(u1);

       // 判断集合中是否包含u2
       User u2 = new User("jack");

       // 没有重写equals之前：这个结果是false
       //System.out.println(c.contains(u2)); // false
       // 重写equals方法之后，比较的时候会比较name。
       System.out.println(c.contains(u2)); // true

       c.remove(u2);
       System.out.println(c.size()); // 0

       /*Integer x = new Integer(10000);
       c.add(x);

       Integer y = new Integer(10000);
       System.out.println(c.contains(y)); // true*/

       // 创建集合对象
       Collection cc = new ArrayList();
       // 创建字符串对象
       String s1 = new String("hello");
       // 加进去。
       cc.add(s1);

       // 创建了一个新的字符串对象
       String s2 = new String("hello");
       // 删除s2
       cc.remove(s2); // s1.equals(s2) java认为s1和s2是一样的。删除s2就是删除s1。
       // 集合中元素个数是？
       System.out.println(cc.size()); // 0
   }
}

class User{
   private String name;
   public User(){}
   public User(String name){
       this.name = name;
   }

   // 重写equals方法
   // 将来调用equals方法的时候，一定是调用这个重写的equals方法。
   // 这个equals方法的比较原理是：只要姓名一样就表示同一个用户。
   public boolean equals(Object o) {
       if(o == null || !(o instanceof User)) return false;
       if(o == this) return true;
       User u = (User)o;
       // 如果名字一样表示同一个人。（不再比较对象的内存地址了。比较内容。）
       return u.name.equals(this.name);
   }

}

```

### 2.1.5 关于集合元素的remove
```java
package com..javase.collection;

import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;

/*
关于集合元素的remove
   重点：当集合的结构发生改变时，迭代器必须重新获取，如果还是用以前老的迭代器，会出现
   异常：java.util.ConcurrentModificationException

   重点：在迭代集合元素的过程中，不能调用集合对象的remove方法，删除元素：
       c.remove(o); 迭代过程中不能这样。
       会出现：java.util.ConcurrentModificationException

   重点：在迭代元素的过程当中，一定要使用迭代器Iterator的remove方法，删除元素，
   不要使用集合自带的remove方法删除元素。

*/
public class CollectionTest06 {
   public static void main(String[] args) {
       // 创建集合
       Collection c = new ArrayList();

       // 注意：此时获取的迭代器，指向的是那是集合中没有元素状态下的迭代器。
       // 一定要注意：集合结构只要发生改变，迭代器必须重新获取。
       // 当集合结构发生了改变，迭代器没有重新获取时，调用next()方法时：java.util.ConcurrentModificationException
       Iterator it = c.iterator();

       // 添加元素
       c.add(1); // Integer类型
       c.add(2);
       c.add(3);

       // 获取迭代器
       //Iterator it = c.iterator();
       /*while(it.hasNext()){
           // 编写代码时next()方法返回值类型必须是Object。
           // Integer i = it.next();
           Object obj = it.next();
           System.out.println(obj);
       }*/

       Collection c2 = new ArrayList();
       c2.add("abc");
       c2.add("def");
       c2.add("xyz");

       Iterator it2 = c2.iterator();
       while(it2.hasNext()){
           Object o = it2.next();
           // 删除元素
           // 删除元素之后，集合的结构发生了变化，应该重新去获取迭代器
           // 但是，循环下一次的时候并没有重新获取迭代器，所以会出现异常：java.util.ConcurrentModificationException
           // 出异常根本原因是：集合中元素删除了，但是没有更新迭代器（迭代器不知道集合变化了）
           //c2.remove(o); // 直接通过集合去删除元素，没有通知迭代器。（导致迭代器的快照和原集合状态不同。）
           // 使用迭代器来删除可以吗？
           // 迭代器去删除时，会自动更新迭代器，并且更新集合（删除集合中的元素）。
           it2.remove(); // 删除的一定是迭代器指向的当前元素。
           System.out.println(o);
       }

       System.out.println(c2.size()); //0
   }
}

```
### 2.1.6 List接口中常用方法
```markdown
计算机英语：
   增删改查这几个单词要知道：
       增：add、save、new
       删：delete、drop、remove
       改：update、set、modify
       查：find、get、query、select

```
```java
package com.yxj.javase.collection;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.LinkedList;
import java.util.List;

/*
测试List接口中常用方法
   1、List集合存储元素特点：有序可重复
       有序：List集合中的元素有下标。
       从0开始，以1递增。
       可重复：存储一个1，还可以再存储1.
   2、List既然是Collection接口的子接口，那么肯定List接口有自己“特色”的方法：
       以下只列出List接口特有的常用的方法：
           void add(int index, Object element)
           Object set(int index, Object element)
           Object get(int index)
           int indexOf(Object o)
           int lastIndexOf(Object o)
           Object remove(int index)

     
*/
public class ListTest01 {
   public static void main(String[] args) {
       // 创建List类型的集合。
       //List myList = new LinkedList();
       //List myList = new Vector();
       List myList = new ArrayList();

       // 添加元素
       myList.add("A"); // 默认都是向集合末尾添加元素。
       myList.add("B");
       myList.add("C");
       myList.add("C");
       myList.add("D");

       //在列表的指定位置插入指定元素（第一个参数是下标）
       // 这个方法使用不多，因为对于ArrayList集合来说效率比较低。
       myList.add(1, "KING");

       // 迭代
       Iterator it = myList.iterator();
       while(it.hasNext()){
           Object elt = it.next();
           System.out.println(elt);
       }

       // 根据下标获取元素
       Object firstObj = myList.get(0);
       System.out.println(firstObj);

       // 因为有下标，所以List集合有自己比较特殊的遍历方式
       // 通过下标遍历。【List集合特有的方式，Set没有。】
       for(int i = 0; i < myList.size(); i++){
           Object obj = myList.get(i);
           System.out.println(obj);
       }

       // 获取指定对象第一次出现处的索引。
       System.out.println(myList.indexOf("C")); // 3

       // 获取指定对象最后一次出现处的索引。
       System.out.println(myList.lastIndexOf("C")); // 4

       // 删除指定下标位置的元素
       // 删除下标为0的元素
       myList.remove(0);
       System.out.println(myList.size()); // 5

       System.out.println("====================================");

       // 修改指定位置的元素
       myList.set(2, "Soft");

       // 遍历集合
       for(int i = 0; i < myList.size(); i++){
           Object obj = myList.get(i);
           System.out.println(obj);
       }
   }
}


```
###  2.1.7 ArrayList
```java
package com.yxj.javase.collection;

import java.util.ArrayList;
import java.util.List;

/*
ArrayList集合：
   1、默认初始化容量10（底层先创建了一个长度为0的数组，当添加第一个元素的时候，初始化容量10。）
   2、集合底层是一个Object[]数组。
   3、构造方法：
       new ArrayList();
       new ArrayList(20);
   4、ArrayList集合的扩容：
       增长到原容量的1.5倍。
       ArrayList集合底层是数组，怎么优化？
           尽可能少的扩容。因为数组扩容效率比较低，建议在使用ArrayList集合
           的时候预估计元素的个数，给定一个初始化容量。
   5、数组优点：
       检索效率比较高。（每个元素占用空间大小相同，内存地址是连续的，知道首元素内存地址，
       然后知道下标，通过数学表达式计算出元素的内存地址，所以检索效率最高。）
   6、数组缺点：
       随机增删元素效率比较低。
       另外数组无法存储大数据量。（很难找到一块非常巨大的连续的内存空间。）
   7、向数组末尾添加元素，效率很高，不受影响。
   8、面试官经常问的一个问题？
       这么多的集合中，你用哪个集合最多？
           答：ArrayList集合。
           因为往数组末尾添加元素，效率不受影响。
           另外，我们检索/查找某个元素的操作比较多。

   7、ArrayList集合是非线程安全的。（不是线程安全的集合。）
*/
public class ArrayListTest01 {
   public static void main(String[] args) {

       // 默认初始化容量是10
       // 数组的长度是10
       List list1 = new ArrayList();
       // 集合的size()方法是获取当前集合中元素的个数。不是获取集合的容量。
       System.out.println(list1.size()); // 0

       // 指定初始化容量
       // 数组的长度是20
       List list2 = new ArrayList(20);
       // 集合的size()方法是获取当前集合中元素的个数。不是获取集合的容量。
       System.out.println(list2.size()); // 0

       list1.add(1);
       list1.add(2);
       list1.add(3);
       list1.add(4);
       list1.add(5);
       list1.add(6);
       list1.add(7);
       list1.add(8);
       list1.add(9);
       list1.add(10);

       System.out.println(list1.size());

       // 再加一个元素
       list1.add(11);
       System.out.println(list1.size()); // 11个元素。
       /*
       int newCapacity = ArraysSupport.newLength(oldCapacity,minCapacity - oldCapacity,oldCapacity >> 1);
        */
       // 100 二进制转换成10进制： 00000100右移一位 00000010 （2）  【4 / 2】
       // 原先是4、现在增长：2，增长之后是6，增长之后的容量是之前容量的：1.5倍。
       // 6是4的1.5倍
   }
}

```
```java
package com.yxj.javase.collection;

import java.util.ArrayList;
import java.util.Collection;
import java.util.HashSet;
import java.util.List;

/*
集合ArrayList的构造方法
*/
public class ArrayListTest02 {
   public static void main(String[] args) {

       // 默认初始化容量10
       List myList1 = new ArrayList();

       // 指定初始化容量100
       List myList2 = new ArrayList(100);

       // 创建一个HashSet集合
       Collection c = new HashSet();
       // 添加元素到Set集合
       c.add(100);
       c.add(200);
       c.add(900);
       c.add(50);

       // 通过这个构造方法就可以将HashSet集合转换成List集合。
       List myList3 = new ArrayList(c);
       for(int i = 0; i < myList3.size(); i++){
           System.out.println(myList3.get(i));
       }
   }
}

```
###  2.1.8 Vector
```java
package com.yxj.javase.collection;

import java.util.*;

/*
Vector：
   1、底层也是一个数组。
   2、初始化容量：10
   3、怎么扩容的？
       扩容之后是原容量的2倍。
       10--> 20 --> 40 --> 80

   4、ArrayList集合扩容特点：
       ArrayList集合扩容是原容量1.5倍。

   5、Vector中所有的方法都是线程同步的，都带有synchronized关键字，
   是线程安全的。效率比较低，使用较少了。

   6、怎么将一个线程不安全的ArrayList集合转换成线程安全的呢？
       使用集合工具类：
           java.util.Collections;

           java.util.Collection 是集合接口。
           java.util.Collections 是集合工具类。
*/
public class VectorTest {
   public static void main(String[] args) {
       // 创建一个Vector集合
       List vector = new Vector();
       //Vector vector = new Vector();

       // 添加元素
       // 默认容量10个。
       vector.add(1);
       vector.add(2);
       vector.add(3);
       vector.add(4);
       vector.add(5);
       vector.add(6);
       vector.add(7);
       vector.add(8);
       vector.add(9);
       vector.add(10);

       // 满了之后扩容（扩容之后的容量是20.）
       vector.add(11);

       Iterator it = vector.iterator();
       while(it.hasNext()){
           Object obj = it.next();
           System.out.println(obj);
       }

       // 这个可能以后要使用！！！！
       List myList = new ArrayList(); // 非线程安全的。

       // 变成线程安全的
       Collections.synchronizedList(myList); // 这里没有办法看效果，因为多线程没学，你记住先！

       // myList集合就是线程安全的了。
       myList.add("111");
       myList.add("222");
       myList.add("333");
   }
}

```
###  2.1.9  泛型
```java
package com.yxj.javase.collection;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

/*
1、JDK5.0之后推出的新特性：泛型


编译阶段起作用，只是给编译器参考的。（运行阶段泛型没用！）
3、使用了泛型好处是什么？
   第一：集合中存储的元素类型统一了。
   第二：从集合中取出的元素类型是泛型指定的类型，不需要进行大量的“向下转型”！

4、泛型的缺点是什么？
   导致集合中存储的元素缺乏多样性！
   大多数业务中，集合中元素的类型还是统一的。所以这种泛型特性被大家所认可。
*/
public class GenericTest01 {
   public static void main(String[] args) {

       /*
       // 不使用泛型机制，分析程序存在缺点
       List myList = new ArrayList();

       // 准备对象
       Cat c = new Cat();
       Bird b = new Bird();

       // 将对象添加到集合当中
       myList.add(c);
       myList.add(b);

       // 遍历集合，取出每个Animal，让它move
       Iterator it = myList.iterator();
       while(it.hasNext()) {
           // 没有这个语法，通过迭代器取出的就是Object
           //Animal a = it.next();

           Object obj = it.next();
           //obj中没有move方法，无法调用，需要向下转型！
           if(obj instanceof Animal){
               Animal a = (Animal)obj;
               a.move();
           }
       }
        */

       // 使用JDK5之后的泛型机制
       // 使用泛型List<Animal>之后，表示List集合中只允许存储Animal类型的数据。
       // 用泛型来指定集合中存储的数据类型。
       List<Animal> myList = new ArrayList<Animal>();

       // 指定List集合中只能存储Animal，那么存储String就编译报错了。
       // 这样用了泛型之后，集合中元素的数据类型更加统一了。
       //myList.add("abc");

       Cat c = new Cat();
       Bird b = new Bird();

       myList.add(c);
       myList.add(b);

       // 获取迭代器
       // 这个表示迭代器迭代的是Animal类型。
       Iterator<Animal> it = myList.iterator();
       while(it.hasNext()){
           // 使用泛型之后，每一次迭代返回的数据都是Animal类型。
           //Animal a = it.next();
           // 这里不需要进行强制类型转换了。直接调用。
           //a.move();

           // 调用子类型特有的方法还是需要向下转换的！
           Animal a = it.next();
           if(a instanceof Cat) {
               Cat x = (Cat)a;
               x.catchMouse();
           }
           if(a instanceof Bird) {
               Bird y = (Bird)a;
               y.fly();
           }
       }
   }
}

class Animal {
   // 父类自带方法
   public void move(){
       System.out.println("动物在移动！");
   }
}

class Cat extends Animal {
   // 特有方法
   public void catchMouse(){
       System.out.println("猫抓老鼠！");
   }
}

class Bird extends Animal {
   // 特有方法
   public void fly(){
       System.out.println("鸟儿在飞翔！");
   }
}
```
####  2.1.9.1  钻石表达式
```java
package com.yxj.javase.collection;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

/*
JDK之后引入了：自动类型推断机制。（又称为钻石表达式）
*/
public class GenericTest02 {
   public static void main(String[] args) {

       // ArrayList<这里的类型会自动推断>()，前提是JDK8之后才允许。
       // 自动类型推断，钻石表达式！
       List<Animal> myList = new ArrayList<>();

       myList.add(new Animal());
       myList.add(new Cat());
       myList.add(new Bird());

       // 遍历
       Iterator<Animal> it = myList.iterator();
       while(it.hasNext()){
           Animal a = it.next();
           a.move();
       }

       List<String> strList = new ArrayList<>();

       // 类型不匹配。
       //strList.add(new Cat());
       strList.add("http://www.126.com");
       strList.add("http://www.baidu.com");
       strList.add("http://www.yxj.com");

       // 类型不匹配。
       //strList.add(123);

       //System.out.println(strList.size());

       // 遍历
       Iterator<String> it2 = strList.iterator();
       while(it2.hasNext()){
           // 如果没有使用泛型
           /*
           Object obj = it2.next();
           if(obj instanceof String){
               String ss = (String)obj;
               ss.substring(7);
           }
            */
           // 直接通过迭代器获取了String类型的数据
           String s = it2.next();
           // 直接调用String类的substring方法截取字符串。
           String newString = s.substring(7);
           System.out.println(newString);
       }
   }
}

```
####  2.1.9.2  自定义泛型
```java
package com.yxj.javase.collection;

/*
自定义泛型可以吗？可以
   自定义泛型的时候，<> 尖括号中的是一个标识符，随便写。
   java源代码中经常出现的是：
       <E>和<T>
   E是Element单词首字母。
   T是Type单词首字母。
*/
public class GenericTest03<标识符随便写> {
 
   public void doSome(标识符随便写 o){
       System.out.println(o);
   }

   public static void main(String[] args) {

       // new对象的时候指定了泛型是：String类型
       GenericTest03<String> gt = new GenericTest03<>();

       // 类型不匹配
       //gt.doSome(100);

       gt.doSome("abc");

       // =============================================================
       GenericTest03<Integer> gt2 = new GenericTest03<>();
       gt2.doSome(100);

       // 类型不匹配
       //gt2.doSome("abc");

       MyIterator<String> mi = new MyIterator<>();
       String s1 = mi.get(); 

       MyIterator<Animal> mi2 = new MyIterator<>();
       Animal a = mi2.get();

       // 不用泛型就是Object类型。
       /*GenericTest03 gt3 = new GenericTest03();
       gt3.doSome(new Object());*/
   }
}

class MyIterator<T> {
   public T get(){
       return null;
   }
}
```
###  2.1.10 增强for循环
```java
package com.yxj.javase.collection;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

/*
集合使用foreach
*/
public class ForEachTest02 {
   public static void main(String[] args) {
       // 创建List集合
       List<String> strList = new ArrayList<>();

       // 添加元素
       strList.add("hello");
       strList.add("world!");
       strList.add("kitty!");

       // 遍历，使用迭代器方式
       Iterator<String> it = strList.iterator();
       while(it.hasNext()){
           String s = it.next();
           System.out.println(s);
       }

       // 使用下标方式（只针对于有下标的集合）
       for(int i = 0; i < strList.size(); i++){
           System.out.println(strList.get(i));
       }

       // 使用foreach
       for(String s : strList){ // 因为泛型使用的是String类型，所以是：String s
           System.out.println(s);
       }

       List<Integer> list = new ArrayList<>();
       list.add(100);
       list.add(200);
       list.add(300);
       for(Integer i : list){ // i代表集合中的元素
           System.out.println(i);
       }
   }
}

```
### 2.1.11 HashSet
```java
package com.yxj.javase.collection;

import java.util.HashSet;
import java.util.Set;

/*
HashSet集合：
   无序不可重复。
*/
public class HashSetTest01 {
   public static void main(String[] args) {
       // 演示一下HashSet集合特点
       Set<String> strs = new HashSet<>();

       // 添加元素
       strs.add("hello3");
       strs.add("hello4");
       strs.add("hello1");
       strs.add("hello2");
       strs.add("hello3");
       strs.add("hello3");
       strs.add("hello3");
       strs.add("hello3");

       // 遍历
       /*
       hello1
       hello4
       hello2
       hello3
       1、存储时顺序和取出的顺序不同。
       2、不可重复。
       3、放到HashSet集合中的元素实际上是放到HashMap集合的key部分了。
        */
       for(String s : strs){
           System.out.println(s);
       }
   }
}

```
### 2.1.12 TreeSet
```java
package com.yxj.javase.collection;

import java.util.Set;
import java.util.TreeSet;

/*
TreeSet集合存储元素特点：
   1、无序不可重复的，但是存储的元素可以自动按照大小顺序排序！
   称为：可排序集合。

   2、无序：这里的无序指的是存进去的顺序和取出来的顺序不同。并且没有下标。
*/
public class TreeSetTest01 {
   public static void main(String[] args) {
       // 创建集合对象
       Set<String> strs = new TreeSet<>();
       // 添加元素
       strs.add("A");
       strs.add("B");
       strs.add("Z");
       strs.add("Y");
       strs.add("Z");
       strs.add("K");
       strs.add("M");
       // 遍历
       /*
           A
           B
           K
           M
           Y
           Z
       从小到大自动排序！
        */
       for(String s : strs){
           System.out.println(s);
       }
   }
}

```
```java
package com.yxj.javase.collection;

import java.util.TreeSet;

public class TreeSetTest04 {
   public static void main(String[] args) {
       Customer c1 = new Customer(32);
       Customer c2 = new Customer(20);
       Customer c3 = new Customer(30);
       Customer c4 = new Customer(25);

       // 创建TreeSet集合
       TreeSet<Customer> customers = new TreeSet<>();
       // 添加元素
       customers.add(c1);
       customers.add(c2);
       customers.add(c3);
       customers.add(c4);

       // 遍历
       for (Customer c : customers){
           System.out.println(c);
       }
   }
}

// 放在TreeSet集合中的元素需要实现java.lang.Comparable接口。
// 并且实现compareTo方法。equals可以不写。
class Customer implements Comparable<Customer>{

   int age;
   public Customer(int age){
       this.age = age;
   }

   // 需要在这个方法中编写比较的逻辑，或者说比较的规则，按照什么进行比较！
   // k.compareTo(t.key)
   // 拿着参数k和集合中的每一个k进行比较，返回值可能是>0 <0 =0
   // 比较规则最终还是由程序员指定的：例如按照年龄升序。或者按照年龄降序。
   @Override
   public int compareTo(Customer c) { // c1.compareTo(c2);
       // this是c1
       // c是c2
       // c1和c2比较的时候，就是this和c比较。
       /*int age1 = this.age;
       int age2 = c.age;
       if(age1 == age2){
           return 0;
       } else if(age1 > age2) {
           return 1;
       } else {
           return -1;
       }*/
       //return this.age - c.age; // =0 >0 <0
       return c.age - this.age;
   }

   public String toString(){
       return "Customer[age="+age+"]";
   }
}
```
```java
package com.yxj.javase.collection;

import java.util.Comparator;
import java.util.TreeSet;

/*
TreeSet集合中元素可排序的第二种方式：使用比较器的方式。
最终的结论：
   放到TreeSet或者TreeMap集合key部分的元素要想做到排序,包括两种方式：
       第一种：放在集合中的元素实现java.lang.Comparable接口。
       第二种：在构造TreeSet或者TreeMap集合的时候给它传一个比较器对象。
Comparable和Comparator怎么选择呢？
   当比较规则不会发生改变的时候，或者说当比较规则只有1个的时候，建议实现Comparable接口。
   如果比较规则有多个，并且需要多个比较规则之间频繁切换，建议使用Comparator接口。

   Comparator接口的设计符合OCP原则。
*/
public class TreeSetTest06 {
   public static void main(String[] args) {
       // 创建TreeSet集合的时候，需要使用这个比较器。
       // TreeSet<WuGui> wuGuis = new TreeSet<>();//这样不行，没有通过构造方法传递一个比较器进去。

       // 给构造方法传递一个比较器。
       //TreeSet<WuGui> wuGuis = new TreeSet<>(new WuGuiComparator());

       // 大家可以使用匿名内部类的方式（这个类没有名字。直接new接口。）
       TreeSet<WuGui> wuGuis = new TreeSet<>(new Comparator<WuGui>() {
           @Override
           public int compare(WuGui o1, WuGui o2) {
               return o1.age - o2.age;
           }
       });

       wuGuis.add(new WuGui(1000));
       wuGuis.add(new WuGui(800));
       wuGuis.add(new WuGui(810));

       for(WuGui wuGui : wuGuis){
           System.out.println(wuGui);
       }
   }
}

// 乌龟
class WuGui{

   int age;

   public WuGui(int age){
       this.age = age;
   }

   @Override
   public String toString() {
       return "小乌龟[" +
               "age=" + age +
               ']';
   }
}

// 单独在这里编写一个比较器
// 比较器实现java.util.Comparator接口。（Comparable是java.lang包下的。Comparator是java.util包下的。）
/*
class WuGuiComparator implements Comparator<WuGui> {

   @Override
   public int compare(WuGui o1, WuGui o2) {
       // 指定比较规则
       // 按照年龄排序
       return o1.age - o2.age;
   }
}
*/

```

```java
CollectionsTest
package com.yxj.javase.collection;

import java.util.*;

/*
java.util.Collection 集合接口
java.util.Collections 集合工具类，方便集合的操作。
*/
public class CollectionsTest {
   public static void main(String[] args) {

       // ArrayList集合不是线程安全的。
       List<String> list = new ArrayList<>();

       // 变成线程安全的
       Collections.synchronizedList(list);

       // 排序
       list.add("abf");
       list.add("abx");
       list.add("abc");
       list.add("abe");

       Collections.sort(list);
       for(String s : list){
           System.out.println(s);
       }

       List<WuGui2> wuGuis = new ArrayList<>();
       wuGuis.add(new WuGui2(1000));
       wuGuis.add(new WuGui2(8000));
       wuGuis.add(new WuGui2(500));
       // 注意：对List集合中元素排序，需要保证List集合中的元素实现了：Comparable接口。
       Collections.sort(wuGuis);
       for(WuGui2 wg : wuGuis){
           System.out.println(wg);
       }

       // 对Set集合怎么排序呢？
       Set<String> set = new HashSet<>();
       set.add("king");
       set.add("kingsoft");
       set.add("king2");
       set.add("king1");
       // 将Set集合转换成List集合
       List<String> myList = new ArrayList<>(set);
       Collections.sort(myList);
       for(String s : myList) {
           System.out.println(s);
       }

       // 这种方式也可以排序。
       //Collections.sort(list集合, 比较器对象);
   }
}

class WuGui2 implements Comparable<WuGui2>{
   int age;
   public WuGui2(int age){
       this.age = age;
   }

   @Override
   public int compareTo(WuGui2 o) {
       return this.age - o.age;
   }

   @Override
   public String toString() {
       return "WuGui2{" +
               "age=" + age +
               '}';
   }
}



```
### 2.1.13 Map接口中常用方法
```java
package com.yxj.javase.collection;

import java.util.Collection;
import java.util.HashMap;
import java.util.Map;

/*
java.util.Map接口中常用的方法：
   1、Map和Collection没有继承关系。
   2、Map集合以key和value的方式存储数据：键值对
       key和value都是引用数据类型。
       key和value都是存储对象的内存地址。
       key起到主导的地位，value是key的一个附属品。
   3、Map接口中常用方法：
       V put(K key, V value) 向Map集合中添加键值对
       V get(Object key) 通过key获取value
       void clear()    清空Map集合
       boolean containsKey(Object key) 判断Map中是否包含某个key
       boolean containsValue(Object value) 判断Map中是否包含某个value
       boolean isEmpty()   判断M.ap集合中元素个数是否为0
       V remove(Object key) 通过key删除键值对
       int size() 获取Map集合中键值对的个数。
       Collection<V> values() 获取Map集合中所有的value，返回一个Collection

       Set<K> keySet() 获取Map集合所有的key（所有的键是一个set集合）

       Set<Map.Entry<K,V>> entrySet()
           将Map集合转换成Set集合
           假设现在有一个Map集合，如下所示：
               map1集合对象
               key             value
               ----------------------------
               1               zhangsan
               2               lisi
               3               wangwu
               4               zhaoliu

               Set set = map1.entrySet();
               set集合对象
               1=zhangsan 【注意：Map集合通过entrySet()方法转换成的这个Set集合，Set集合中元素的类型是 Map.Entry<K,V>】
               2=lisi     【Map.Entry和String一样，都是一种类型的名字，只不过：Map.Entry是静态内部类，是Map中的静态内部类】
               3=wangwu
               4=zhaoliu ---> 这个东西是个什么？Map.Entry
*/
public class MapTest01 {
   public static void main(String[] args) {
       // 创建Map集合对象
       Map<Integer, String> map = new HashMap<>();
       // 向Map集合中添加键值对
       map.put(1, "zhangsan"); // 1在这里进行了自动装箱。
       map.put(2, "lisi");
       map.put(3, "wangwu");
       map.put(4, "zhaoliu");
       // 通过key获取value
       String value = map.get(2);
       System.out.println(value);
       // 获取键值对的数量
       System.out.println("键值对的数量：" + map.size());
       // 通过key删除key-value
       map.remove(2);
       System.out.println("键值对的数量：" + map.size());
       // 判断是否包含某个key
       // contains方法底层调用的都是equals进行比对的，所以自定义的类型需要重写equals方法。
       System.out.println(map.containsKey(new Integer(4))); // true
       // 判断是否包含某个value
       System.out.println(map.containsValue(new String("wangwu"))); // true

       // 获取所有的value
       Collection<String> values = map.values();
       // foreach
       for(String s : values){
           System.out.println(s);
       }

       // 清空map集合
       map.clear();
       System.out.println("键值对的数量：" + map.size());
       // 判断是否为空
       System.out.println(map.isEmpty()); // true
   }
}

```
#### 2.1.13.1  Map集合的遍历。【非常重要】
```java
package com.yxj.javase.collection;

import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

/*
Map集合的遍历。【非常重要】
*/
public class MapTest02 {
   public static void main(String[] args) {

       // 第一种方式：获取所有的key，通过遍历key，来遍历value
       Map<Integer, String> map = new HashMap<>();
       map.put(1, "zhangsan");
       map.put(2, "lisi");
       map.put(3, "wangwu");
       map.put(4, "zhaoliu");
       // 遍历Map集合
       // 获取所有的key，所有的key是一个Set集合
       Set<Integer> keys = map.keySet();
       // 遍历key，通过key获取value
       // 迭代器可以
       /*Iterator<Integer> it = keys.iterator();
       while(it.hasNext()){
           // 取出其中一个key
           Integer key = it.next();
           // 通过key获取value
           String value = map.get(key);
           System.out.println(key + "=" + value);
       }*/
       // foreach也可以
       for(Integer key : keys){
           System.out.println(key + "=" + map.get(key));
       }

       // 第二种方式：Set<Map.Entry<K,V>> entrySet()
       // 以上这个方法是把Map集合直接全部转换成Set集合。
       // Set集合中元素的类型是：Map.Entry
       Set<Map.Entry<Integer,String>> set = map.entrySet();
       // 遍历Set集合，每一次取出一个Node
       // 迭代器
       /*Iterator<Map.Entry<Integer,String>> it2 = set.iterator();
       while(it2.hasNext()){
           Map.Entry<Integer,String> node = it2.next();
           Integer key = node.getKey();
           String value = node.getValue();
           System.out.println(key + "=" + value);
       }*/

       // foreach
       // 这种方式效率比较高，因为获取key和value都是直接从node对象中获取的属性值。
       // 这种方式比较适合于大数据量。
       for(Map.Entry<Integer,String> node : set){
           System.out.println(node.getKey() + "--->" + node.getValue());
       }
   }
}

```
```java
package com.yxj.javase.collection;

import java.util.HashMap;
import java.util.Map;
import java.util.Set;

/*
HashMap集合：
   1、HashMap集合底层是哈希表/散列表的数据结构。
   2、哈希表是一个怎样的数据结构呢？
       哈希表是一个数组和单向链表的结合体。
       数组：在查询方面效率很高，随机增删方面效率很低。
       单向链表：在随机增删方面效率较高，在查询方面效率很低。
       哈希表将以上的两种数据结构融合在一起，充分发挥它们各自的优点。
   3、HashMap集合底层的源代码：
       public class HashMap{
           // HashMap底层实际上就是一个数组。（一维数组）
           Node<K,V>[] table;
           // 静态的内部类HashMap.Node
           static class Node<K,V> {
               final int hash; // 哈希值(哈希值是key的hashCode()方法的执行结果)hash值通过哈希函数/算法可以转换存储成数组的下标。）
               final K key; // 存储到Map集合中的那个key
               V value; // 存储到Map集合中的那个value
               Node<K,V> next; // 下一个节点的内存地址。
           }
       }
       哈希表/散列表：一维数组，这个数组中每一个元素是一个单向链表。（数组和链表的结合体。）
   4、最主要掌握的是：
       map.put(k,v)
       v = map.get(k)
       以上这两个方法的实现原理，是必须掌握的。
   5、HashMap集合的key部分特点：
       无序，不可重复。
       为什么无序？ 因为不一定挂到哪个单向链表上。
       不可重复是怎么保证的？ equals方法来保证HashMap集合的key不可重复。
       如果key重复了，value会覆盖。
		放到HashSet集合中的元素实际上是放在HashMap集合key部分     
       HashSet集合中的元素也需要同时重写hashCode()+equals()方法。

   6、哈希表HashMap使用不当时无法发挥性能！
       假设将所有的hashCode()方法返回值固定为某个值，那么会导致底层哈希表变成了
       纯单向链表。这种情况我们成为：散列分布不均匀。
       什么是散列分布均匀？
           假设有100个元素，10个单向链表，那么每个单向链表上有10个节点，这是最好的，
           是散列分布均匀的。
       假设将所有的hashCode()方法返回值都设定为不一样的值，可以吗，有什么问题？
           不行，因为这样的话导致底层哈希表就成为一维数组了，没有链表的概念了。
           也是散列分布不均匀。
       散列分布均匀需要你重写hashCode()方法时有一定的技巧。
   7、重点：放在HashMap集合key部分的元素，以及放在HashSet集合中的元素，需要同时重写hashCode和equals方法。
   8、HashMap集合的默认初始化容量是16，默认加载因子是0.75
       这个默认加载因子是当HashMap集合底层数组的容量达到75%的时候，数组开始扩容。

       重点，记住：HashMap集合初始化容量必须是2的倍数，这也是官方推荐的，
       这是因为达到散列均匀，为了提高HashMap集合的存取效率，所必须的。

重写 equals() 方法 和 hashCode() 方法
向Map集合中存元素，以及从Map集合中取元素，都是先调用key的hashCode()方法，然后调用equals()方法.equals()方法可能调用，也可能不调用。

equals()方法什么时候不调用？k.hashCode()方法返回哈希值，哈希值转换为数组下标。数组下标的位置上如果是null，equals不需要执行。

在HashMap集合中，对于同一链表上的节点来说，它们的哈希值是相同的，也有不同的情况(哈希碰撞)，但最终通过哈希算法计算出来的数组下标是一样的。哈希值相同，一定在同一链表上！

 hashCode()、equals()方法用idea同时生成

在JDK8之后，如果哈希表单向链表中元素超过8个，单向链表这种数据结构就会变成红黑树数据结构，当红黑树上的节点数量小于6时，会把红黑树变成单向链表数据结构。这是因为树的查询效率比链表高，提高了检索效率。

*/
public class HashMapTest01 {
   public static void main(String[] args) {
       // 测试HashMap集合key部分的元素特点
       // Integer是key，它的hashCode和equals都重写了。
       Map<Integer,String> map = new HashMap<>();
       map.put(1111, "zhangsan");
       map.put(6666, "lisi");
       map.put(7777, "wangwu");
       map.put(2222, "zhaoliu");
       map.put(2222, "king"); //key重复的时候value会自动覆盖。

       System.out.println(map.size()); // 4

       // 遍历Map集合
       Set<Map.Entry<Integer,String>> set = map.entrySet();
       for(Map.Entry<Integer,String> entry : set){
           // 验证结果：HashMap集合key部分元素：无序不可重复。
           System.out.println(entry.getKey() + "=" + entry.getValue());
       }
   }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210315223536888.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

#### 2.1.13.2 Hashtable
```java
package com.yxj.javase.bean;

import java.util.Hashtable;
import java.util.Map;

/*
Hashtable的key可以为null吗？
   Hashtable的key和value都是不能为null的。
   HashMap集合的key和value都是可以为null的。

Hashtable方法都带有synchronized：线程安全的。
线程安全有其它的方案，这个Hashtable对线程的处理
导致效率较低，使用较少了。

Hashtable和HashMap一样，底层都是哈希表数据结构。
Hashtable的初始化容量是11，默认加载因子是：0.75f
Hashtable的扩容是：原容量 * 2 + 1
*/
public class HashtableTest01 {
   public static void main(String[] args) {
       Map map = new Hashtable();

       //map.put(null, "123");
       map.put(100, null);

   }
}

```
#### 2.1.13.3   Properties
```java
package com.yxj.javase.collection;

import java.util.Properties;

/*
目前只需要掌握Properties属性类对象的相关方法即可。
Properties是一个Map集合，继承Hashtable，Properties的key和value都是String类型。
Properties被称为属性类对象。
Properties是线程安全的。
*/
public class PropertiesTest01 {
   public static void main(String[] args) {

       // 创建一个Properties对象
       Properties pro = new Properties();

       // 需要掌握Properties的两个方法，一个存，一个取。
       pro.setProperty("url", "jdbc:mysql://localhost:3306/yxj");
       pro.setProperty("driver","com.mysql.jdbc.Driver");
       pro.setProperty("username", "root");
       pro.setProperty("password", "123");

       // 通过key获取value
       String url = pro.getProperty("url");
       String driver = pro.getProperty("driver");
       String username = pro.getProperty("username");
       String password = pro.getProperty("password");

       System.out.println(url);
       System.out.println(driver);
       System.out.println(username);
       System.out.println(password);

   }
}

```
#### 2.1.13.4 集合大总结
```java

package com.yxj.javase.review;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.LinkedList;

/*
1.1、每个集合对象的创建（new）
1.2、向集合中添加元素
1.3、从集合中取出某个元素
1.4、遍历集合
*/
public class ArrayListTest {
   public static void main(String[] args) {
       // 创建集合对象
       //ArrayList<String> list = new ArrayList<>();
       LinkedList<String> list = new LinkedList<>();
       // 添加元素
       list.add("zhangsan");
       list.add("lisi");
       list.add("wangwu");
       // 从集合中取出某个元素
       // List集合有下标
       String firstElt = list.get(0);
       System.out.println(firstElt);
       // 遍历（下标方式）
       for(int i = 0; i < list.size(); i++){
           String elt = list.get(i);
           System.out.println(elt);
       }
       // 遍历（迭代器方式，这个是通用的，所有Collection都能用）
       Iterator<String> it = list.iterator();
       while(it.hasNext()){
           System.out.println(it.next());
       }

       // while循环修改为for循环
       /*for(Iterator<String> it2 = list.iterator(); it2.hasNext(); ){
           System.out.println("====>" + it2.next());
       }*/

       // 遍历（foreach方式）
       for(String s : list){
           System.out.println(s);
       }
   }
}

----------------------------------------------------------------------------
package com.yxj.javase.review;

import java.util.HashSet;
import java.util.Iterator;
import java.util.Objects;
import java.util.Set;

/*
1.1、每个集合对象的创建（new）
1.2、向集合中添加元素
1.3、从集合中取出某个元素
1.4、遍历集合
1.5、测试HashSet集合的特点：无序不可重复。
*/
public class HashSetTest {
   public static void main(String[] args) {
       // 创建集合对象
       HashSet<String> set = new HashSet<>();

       // 添加元素
       set.add("abc");
       set.add("def");
       set.add("king");

       // set集合中的元素不能通过下标取了。没有下标
       // 遍历集合（迭代器）
       Iterator<String> it = set.iterator();
       while(it.hasNext()){
           System.out.println(it.next());
       }

       // 遍历集合（foreach）
       for(String s : set){
           System.out.println(s);
       }

       set.add("king");
       set.add("king");
       set.add("king");
       System.out.println(set.size()); //3 （后面3个king都没有加进去。）

       set.add("1");
       set.add("10");
       set.add("2");

       for(String s : set){
           System.out.println("--->" + s);
       }

       // 创建Set集合，存储Student数据
       Set<Student> students = new HashSet<>();

       Student s1 = new Student(111, "zhangsan");
       Student s2 = new Student(222, "lisi");
       Student s3 = new Student(111, "zhangsan");

       students.add(s1);
       students.add(s2);
       students.add(s3);

       System.out.println(students.size()); // 2

       // 遍历
       for(Student stu : students){
           System.out.println(stu);
       }

   }
}

class Student {
   int no;
   String name;

   public Student() {
   }

   public Student(int no, String name) {
       this.no = no;
       this.name = name;
   }

   @Override
   public String toString() {
       return "Student{" +
               "no=" + no +
               ", name='" + name + '\'' +
               '}';
   }

   @Override
   public boolean equals(Object o) {
       if (this == o) return true;
       if (o == null || getClass() != o.getClass()) return false;
       Student student = (Student) o;
       return no == student.no &&
               Objects.equals(name, student.name);
   }

   @Override
   public int hashCode() {
       return Objects.hash(no, name);
   }
}

----------------------------------------------------------------------------
package com.yxj.javase.review;

import java.util.Comparator;
import java.util.Iterator;
import java.util.TreeSet;

/*
1.1、每个集合对象的创建（new）
1.2、向集合中添加元素
1.3、从集合中取出某个元素
1.4、遍历集合
1.5、测试TreeSet集合中的元素是可排序的。
1.6、测试TreeSet集合中存储的类型是自定义的。
1.7、测试实现Comparable接口的方式
1.8、测试实现Comparator接口的方式（最好测试以下匿名内部类的方式）
*/
public class TreeSetTest {
   public static void main(String[] args) {
       // 集合的创建（可以测试以下TreeSet集合中存储String、Integer的。这些类都是SUN写好的。）
       //TreeSet<Integer> ts = new TreeSet<>();

       // 编写比较器可以改变规则。
       TreeSet<Integer> ts = new TreeSet<>(new Comparator<Integer>() {
           @Override
           public int compare(Integer o1, Integer o2) {
               return o2 - o1; // 自动拆箱
           }
       });

       // 添加元素
       ts.add(1);
       ts.add(100);
       ts.add(10);
       ts.add(10);
       ts.add(10);
       ts.add(10);
       ts.add(0);

       // 遍历（迭代器方式）
       Iterator<Integer> it = ts.iterator();
       while(it.hasNext()) {
           Integer i = it.next();
           System.out.println(i);
       }

       // 遍历（foreach）
       for(Integer x : ts){
           System.out.println(x);
       }

       // TreeSet集合中存储自定义类型
       TreeSet<A> atree = new TreeSet<>();

       atree.add(new A(100));
       atree.add(new A(200));
       atree.add(new A(500));
       atree.add(new A(300));
       atree.add(new A(400));
       atree.add(new A(1000));

       // 遍历
       for(A a : atree){
           System.out.println(a);
       }

       //TreeSet<B> btree = new TreeSet<>(new BComparator());
       // 匿名内部类方式。
       TreeSet<B> btree = new TreeSet<>(new Comparator<B>() {
           @Override
           public int compare(B o1, B o2) {
               return o1.i - o2.i;
           }
       });

       btree.add(new B(500));
       btree.add(new B(100));
       btree.add(new B(200));
       btree.add(new B(600));
       btree.add(new B(300));
       btree.add(new B(50));

       for(B b : btree){
           System.out.println(b);
       }
   }
}

// 第一种方式：实现Comparable接口
class A implements Comparable<A>{
   int i;

   public A(int i){
       this.i = i;
   }

   @Override
   public String toString() {
       return "A{" +
               "i=" + i +
               '}';
   }

   @Override
   public int compareTo(A o) {
       //return this.i - o.i;
       return o.i - this.i;
   }
}

class B {
   int i;
   public B(int i){
       this.i = i;
   }

   @Override
   public String toString() {
       return "B{" +
               "i=" + i +
               '}';
   }
}

// 比较器
class BComparator implements Comparator<B> {

   @Override
   public int compare(B o1, B o2) {
       return o1.i - o2.i;
   }
}

----------------------------------------------------------------------------
package com.yxj.javase.review;

import java.util.HashMap;
import java.util.Map;
import java.util.Set;

/*
1.1、每个集合对象的创建（new）
1.2、向集合中添加元素
1.3、从集合中取出某个元素
1.4、遍历集合
*/
public class HashMapTest {
   public static void main(String[] args) {
       // 创建Map集合
       Map<Integer, String> map = new HashMap<>();
       // 添加元素
       map.put(1, "zhangsan");
       map.put(9, "lisi");
       map.put(10, "wangwu");
       map.put(2, "king");
       map.put(2, "simth"); // key重复value会覆盖。
       // 获取元素个数
       System.out.println(map.size());
       // 取key是2的元素
       System.out.println(map.get(2)); // smith
       // 遍历Map集合很重要，几种方式都要会。
       // 第一种方式：先获取所有的key，遍历key的时候，通过key获取value
       Set<Integer> keys = map.keySet();
       for(Integer key : keys){
           System.out.println(key + "=" + map.get(key));
       }

       // 第二种方式：是将Map集合转换成Set集合，Set集合中每一个元素是Node
       // 这个Node节点中有key和value
       Set<Map.Entry<Integer,String>> nodes = map.entrySet();
       for(Map.Entry<Integer,String> node : nodes){
           System.out.println(node.getKey() + "=" + node.getValue());
       }
   }
}

----------------------------------------------------------------------------
package com.yxj.javase.review;

import java.util.Properties;

public class PropertiesTest {
   public static void main(String[] args) {
       // 创建对象
       Properties pro = new Properties();
       // 存
       pro.setProperty("username", "test");
       pro.setProperty("password", "test123");
       // 取
       String username = pro.getProperty("username");
       String password = pro.getProperty("password");
       System.out.println(username);
       System.out.println(password);

   }
}

```
#### 2.1.13.5   总结（集合所有的实现类）
```markdown
ArrayList：底层是数组。
LinkedList：底层是双向链表。
Vector：底层是数组，线程安全的，效率较低，使用较少。
HashSet:底层是 HashMap，放到 HashSet集合中的元素
等同于放到 Hash Map集合key部分了
TreeSet：底层是 TreeMap，放到 TreeSet集合中的元素等同于
放到 TreeMap集合key部分了。
HashMap：底层是哈希表。
Hashtable：底层也是哈希表，只不过线程安全的，效
率较低，使用较少。
Properties：是线程安全的，并且key和 value只能存储
字符串 String.
TreeMap：底层是二叉树。 TreeMap集合的key可以自动
按照大小顺序排序。
List集合存储元素的特点：
有序可重复
有序：存进去的顺序和取出的顺序相同，每一个元素都有下标。
可重复：存进去1，可以再存储一个1
Set（Map）集合存储元素的特点：
无序不可重复
无序：存进去的顺序和取出的顺序不一定相同。另外Set集
合中元素没有下标。
不可重复：存进去1，不能再存储1了。
SortedSet（ SortedMap）集合存储元素特点：
首先是无序不可重复的，但是 SortedSet集合中的元素是
可排序的。“
无序：存进去的顺序和取出的顺序不一定相同。另外set集合中
元素没有下标
不可重复：存进去1，不能再存储1了。
可排序：可以按照大小顺序排列。
Map集合的key，就是一个Set集合。
往Set集合中放效据，实际上放到了Map集合的key部分。
```
## 3.1 IO流

```markdown
InputStream和OutputStream继承结构图
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200530140236992.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
InputStream和OutputStream继承结构图
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210316082828511.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200530131435126.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
IO流的分类？

有多种分类方式：

一种方式是按照流的方向进行分类：
以内存作为参照物，
往内存中去，叫做输入(Input)。或者叫做读(Read)。
从内存中出来，叫做输出(Output)。或者叫做写(Write)。

另一种方式是按照读取数据方式不同进行分类：
有的流是按照字节的方式读取数据，一次读取1个字节byte，等
同于一次读取8个二进制位。
这种流是万能的，什么类型的文件都可以读取。包
括：文本文件，图片，声音文件，视频文件等....
假设文件file1.txt，采用字节流的话是这样读的：
a中国bc张三fe
第一次读：一个字节，正好读到'a'
第二次读：一个字节，正好读到'中'字符的一半。
第三次读：一个字节，正好读到'中'字符的另外一半。

有的流是按照字符的方式读取数据的，一次读取一个字符，
这种流是为了方便读取
普通文本文件而存在的，这种流不能读取：图片、声音、视
频等文件。只能读取纯
文本文件，连word文件都无法读取。
假设文件file1.txt，采用字符流的话是这样读的：
a中国bc张三fe
第一次读：'a'字符（'a'字符在windows系统中占用1个字节。）
第二次读：'中'字符（'中'字符在windows系统中占用2个字节。）

综上所述：流的分类
输入流、输出流
字节流、字符流


java IO流这块有四大家族：
四大家族的首领：
java.io.InputStream  字节输入流
java.io.OutputStream 字节输出流

java.io.Reader		字符输入流
java.io.Writer		字符输出流

四大家族的首领都是抽象类。(abstract class)

所有的流都实现了：
java.io.Closeable接口，都是可关闭的，都有close()方法。
流毕竟是一个管道，这个是内存和硬盘之间的通道，用完之
后一定要关闭，
不然会耗费(占用)很多资源。养成好习惯，用完流一定要关闭。

所有的输出流都实现了：
java.io.Flushable接口，都是可刷新的，都有flush()方法。
养成一个好习惯，输出流在最终输出之后，一定要记得flush()
刷新一下。这个刷新表示将通道/管道当中剩余未输出的数据
强行输出完（清空管道！）刷新的作用就是清空管道。
注意：如果没有flush()可能会导致丢失数据。


注意：在java中只要“类名”以Stream结尾的都是字节
流。以“Reader/Writer”结尾的都是字符流。

java.io包下需要掌握的流有16个：

文件专属：
java.io.FileInputStream（掌握）
java.io.FileOutputStream（掌握）
java.io.FileReader
java.io.FileWriter

转换流：（将字节流转换成字符流）
java.io.InputStreamReader
java.io.OutputStreamWriter

缓冲流专属：
java.io.BufferedReader
java.io.BufferedWriter
java.io.BufferedInputStream
java.io.BufferedOutputStream

数据流专属：
java.io.DataInputStream
java.io.DataOutputStream

标准输出流：
java.io.PrintWriter
java.io.PrintStream（掌握）

对象专属流：
java.io.ObjectInputStream（掌握）
java.io.ObjectOutputStream（掌握）

java.io.File类。
File类的常用方法。

8、java io这块还剩下什么内容：
第一：ObjectInputStream ObjectOutputStream的使用。
第二：IO流+Properties集合的联合使用。

2、关于对象流
ObjectInputStream
ObjectOutputStream
重点：
参与序列化的类型必须实现java.io.Serializable接口。
并且建议将序列化版本号手动的写出来。
private static final long serialVersionUID = 1L;

3、IO + Properties联合使用。
IO流：文件的读和写。
Properties:是一个Map集合，key和value都是String类型。
```
### 3.1.1  FileInputStream
```java
package com.yxj.java.io;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

/*
java.io.FileInputStream:
   1、文件字节输入流，万能的，任何类型的文件都可以采用这个流来读。
   2、字节的方式，完成输入的操作，完成读的操作（硬盘---> 内存）
*/
public class FileInputStreamTest01 {
   public static void main(String[] args) {
       FileInputStream fis = null;
       try {
           // 创建文件字节输入流对象
           // 文件路径：D:\course\JavaProjects\02-JavaSE\temp （IDEA会自动把\编程\\，因为java中\表示转义）
           // 以下都是采用了：绝对路径的方式。
           //FileInputStream fis = new FileInputStream("D:\\course\\JavaProjects\\02-JavaSE\\temp");
           // 写成这个/也是可以的。
           fis = new FileInputStream("D:/course/JavaProjects/02-JavaSE/temp");

           // 开始读
           int readData = fis.read(); // 这个方法的返回值是：读取到的“字节”本身。
           System.out.println(readData); //97

           readData = fis.read();
           System.out.println(readData); //98

           readData = fis.read();
           System.out.println(readData); //99

           readData = fis.read();
           System.out.println(readData); //100

           readData = fis.read();
           System.out.println(readData); //101

           readData = fis.read();
           System.out.println(readData); //102

           // 已经读到文件的末尾了，再读的时候读取不到任何数据，返回-1.
           readData = fis.read();
           System.out.println(readData);

      
       } catch (FileNotFoundException e) {
           e.printStackTrace();
       } catch (IOException e) {
           e.printStackTrace();
       } finally {
           // 在finally语句块当中确保流一定关闭。
           if (fis != null) { // 避免空指针异常！
               // 关闭流的前提是：流不是空。流是null的时候没必要关闭。
               try {
                   fis.close();
               } catch (IOException e) {
                   e.printStackTrace();
               }
           }
       }
   }
}

```
```java
package com.yxj.java.io;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

/*
int read(byte[] b)
   一次最多读取 b.length 个字节。
   减少硬盘和内存的交互，提高程序的执行效率。
   往byte[]数组当中读。
*/
public class FileInputStreamTest03 {
   public static void main(String[] args) {
       FileInputStream fis = null;
       try {
           // 相对路径的话呢？相对路径一定是从当前所在的位置作为起点开始找！
           // IDEA默认的当前路径是哪里？工程Project的根就是IDEA的默认当前路径。
           //fis = new FileInputStream("tempfile3");
           //fis = new FileInputStream("chapter23/tempfile2");
           //fis = new FileInputStream("chapter23/src/tempfile3");
           fis = new FileInputStream("chapter23/src/com/yxj/java/io/tempfile4");

           // 开始读，采用byte数组，一次读取多个字节。最多读取“数组.length”个字节。
           byte[] bytes = new byte[4]; // 准备一个4个长度的byte数组，一次最多读取4个字节。
           // 这个方法的返回值是：读取到的字节数量。（不是字节本身）
           int readCount = fis.read(bytes);
           System.out.println(readCount); // 第一次读到了4个字节。
           // 将字节数组全部转换成字符串
           //System.out.println(new String(bytes)); // abcd
           // 不应该全部都转换，应该是读取了多少个字节，转换多少个。
           System.out.println(new String(bytes,0, readCount));

           readCount = fis.read(bytes); // 第二次只能读取到2个字节。
           System.out.println(readCount); // 2
           // 将字节数组全部转换成字符串
           //System.out.println(new String(bytes)); // efcd
           // 不应该全部都转换，应该是读取了多少个字节，转换多少个。
           System.out.println(new String(bytes,0, readCount));

           readCount = fis.read(bytes); // 1个字节都没有读取到返回-1
           System.out.println(readCount); // -1

       } catch (FileNotFoundException e) {
           e.printStackTrace();
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
       }
   }
}

```
```java
package com.yxj.java.io;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

/*
最终版，需要掌握。
*/
public class FileInputStreamTest04 {
   public static void main(String[] args) {
       FileInputStream fis = null;
       try {
           fis = new FileInputStream("chapter23/src/tempfile3");
           // 准备一个byte数组
           byte[] bytes = new byte[4];
           /*while(true){
               int readCount = fis.read(bytes);
               if(readCount == -1){
                   break;
               }
               // 把byte数组转换成字符串，读到多少个转换多少个。
               System.out.print(new String(bytes, 0, readCount));
           }*/

           int readCount = 0;
           while((readCount = fis.read(bytes)) != -1) {
               System.out.print(new String(bytes, 0, readCount));
           }

       } catch (FileNotFoundException e) {
           e.printStackTrace();
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
       }
   }
}

```
```java
package com.yxj.java.io;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

/*
FileInputStream类的其它常用方法：
   int available()：返回流当中剩余的没有读到的字节数量
   long skip(long n)：跳过几个字节不读。
*/
public class FileInputStreamTest05 {
   public static void main(String[] args) {
       FileInputStream fis = null;
       try {
           fis = new FileInputStream("tempfile");
           System.out.println("总字节数量：" + fis.available());
           // 读1个字节
           //int readByte = fis.read();
           // 还剩下可以读的字节数量是：5
           //System.out.println("剩下多少个字节没有读：" + fis.available());
           // 这个方法有什么用？
           //byte[] bytes = new byte[fis.available()]; // 这种方式不太适合太大的文件，因为byte[]数组不能太大。
           // 不需要循环了。
           // 直接读一次就行了。
           //int readCount = fis.read(bytes); // 6
           //System.out.println(new String(bytes)); // abcdef

           // skip跳过几个字节不读取，这个方法也可能以后会用！
           fis.skip(3);
           System.out.println(fis.read()); //100

       } catch (FileNotFoundException e) {
           e.printStackTrace();
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
       }
   }
}

```
### 3.1.2  FileOutputStream
```java
package com.yxj.java.io;

import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

/**
* 文件字节输出流，负责写。
* 从内存到硬盘。
*/
public class FileOutputStreamTest01 {
   public static void main(String[] args) {
       FileOutputStream fos = null;
       try {
           // myfile文件不存在的时候会自动新建！
           // 这种方式谨慎使用，这种方式会先将原文件清空，然后重新写入。
           //fos = new FileOutputStream("myfile");
           //fos = new FileOutputStream("chapter23/src/tempfile3");

           // 以追加的方式在文件末尾写入。不会清空原文件内容。
           fos = new FileOutputStream("chapter23/src/tempfile3", true);
           // 开始写。
           byte[] bytes = {97, 98, 99, 100};
           // 将byte数组全部写出！
           fos.write(bytes); // abcd
           // 将byte数组的一部分写出！
           fos.write(bytes, 0, 2); // 再写出ab

           // 字符串
           String s = "我是一个中国人，我骄傲！！！";
           // 将字符串转换成byte数组。
           byte[] bs = s.getBytes();
           // 写
           fos.write(bs);

           // 写完之后，最后一定要刷新
           fos.flush();
       } catch (FileNotFoundException e) {
           e.printStackTrace();
       } catch (IOException e) {
           e.printStackTrace();
       } finally {
           if (fos != null) {
               try {
                   fos.close();
               } catch (IOException e) {
                   e.printStackTrace();
               }
           }
       }
   }
}

```
```java
copy
package com.yxj.java.io;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

/*
使用FileInputStream + FileOutputStream完成文件的拷贝。
拷贝的过程应该是一边读，一边写。
使用以上的字节流拷贝文件的时候，文件类型随意，万能的。什么样的文件都能拷贝。
*/
public class Copy01 {
   public static void main(String[] args) {
       FileInputStream fis = null;
       FileOutputStream fos = null;
       try {
           // 创建一个输入流对象
           fis = new FileInputStream("D:\\course\\02-JavaSE\\video\\chapter01\\动力节点-JavaSE-杜聚宾-001-文件扩展名的显示.avi");
           // 创建一个输出流对象
           fos = new FileOutputStream("C:\\动力节点-JavaSE-杜聚宾-001-文件扩展名的显示.avi");

           // 最核心的：一边读，一边写
           byte[] bytes = new byte[1024 * 1024]; // 1MB（一次最多拷贝1MB。）
           int readCount = 0;
           while((readCount = fis.read(bytes)) != -1) {
               fos.write(bytes, 0, readCount);
           }

           // 刷新，输出流最后要刷新
           fos.flush();
       } catch (FileNotFoundException e) {
           e.printStackTrace();
       } catch (IOException e) {
           e.printStackTrace();
       } finally {
           // 分开try，不要一起try。
           // 一起try的时候，其中一个出现异常，可能会影响到另一个流的关闭。
           if (fos != null) {
               try {
                   fos.close();
               } catch (IOException e) {
                   e.printStackTrace();
               }
           }
           if (fis != null) {
               try {
                   fis.close();
               } catch (IOException e) {
                   e.printStackTrace();
               }
           }
       }
   }
}

```
### 3.1.3 FileReader
```java
package com.yxj.java.io;

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;

/*
FileReader：
   文件字符输入流，只能读取普通文本。
   读取文本内容时，比较方便，快捷。
*/
public class FileReaderTest {
   public static void main(String[] args) {
       FileReader reader = null;
       try {
           // 创建文件字符输入流
           reader = new FileReader("tempfile");

           //准备一个char数组
           char[] chars = new char[4];
           // 往char数组中读
           reader.read(chars); // 按照字符的方式读取：第一次e，第二次f，第三次 风....
           for(char c : chars) {
               System.out.println(c);
           }

           /*// 开始读
           char[] chars = new char[4]; // 一次读取4个字符
           int readCount = 0;
           while((readCount = reader.read(chars)) != -1) {
               System.out.print(new String(chars,0,readCount));
           }*/
       } catch (FileNotFoundException e) {
           e.printStackTrace();
       } catch (IOException e) {
           e.printStackTrace();
       } finally {
           if (reader != null) {
               try {
                   reader.close();
               } catch (IOException e) {
                   e.printStackTrace();
               }
           }
       }
   }
}

```
### 3.1.4 FileWriter
```java
package com.yxj.java.io;

import java.io.FileWriter;
import java.io.IOException;

/*
FileWriter:
   文件字符输出流。写。
   只能输出普通文本。
*/
public class FileWriterTest {
   public static void main(String[] args) {
       FileWriter out = null;
       try {
           // 创建文件字符输出流对象
           //out = new FileWriter("file");
           out = new FileWriter("file", true);

           // 开始写。
           char[] chars = {'我','是','中','国','人'};
           out.write(chars);
           out.write(chars, 2, 3);

           out.write("我是一名java软件工程师！");
           // 写出一个换行符。
           out.write("\n");
           out.write("hello world!");

           // 刷新
           out.flush();
       } catch (IOException e) {
           e.printStackTrace();
       } finally {
           if (out != null) {
               try {
                   out.close();
               } catch (IOException e) {
                   e.printStackTrace();
               }
           }
       }
   }
}

```

```java
Copy02
package com.yxj.java.io;

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

/*
使用FileReader FileWriter进行拷贝的话，只能拷贝“普通文本”文件。
*/
public class Copy02 {
   public static void main(String[] args) {
       FileReader in = null;
       FileWriter out = null;
       try {
           // 读
           in = new FileReader("chapter23/src/com/yxj/java/io/Copy02.java");
           // 写
           out = new FileWriter("Copy02.java");

           // 一边读一边写：
           char[] chars = new char[1024 * 512]; // 1MB
           int readCount = 0;
           while((readCount = in.read(chars)) != -1){
               out.write(chars, 0, readCount);
           }

           // 刷新
           out.flush();
       } catch (FileNotFoundException e) {
           e.printStackTrace();
       } catch (IOException e) {
           e.printStackTrace();
       } finally {
           if (in != null) {
               try {
                   in.close();
               } catch (IOException e) {
                   e.printStackTrace();
               }
           }
           if (out != null) {
               try {
                   out.close();
               } catch (IOException e) {
                   e.printStackTrace();
               }
           }
       }

   }
}

```
### 3.1.5 BufferedReader
```java
package com.yxj.java.io;

import java.io.BufferedReader;
import java.io.FileReader;

/*
BufferedReader:
   带有缓冲区的字符输入流。
   使用这个流的时候不需要自定义char数组，或者说不需要自定义byte数组。自带缓冲。
*/
public class BufferedReaderTest01 {
   public static void main(String[] args) throws Exception{

       FileReader reader = new FileReader("Copy02.java");
       // 当一个流的构造方法中需要一个流的时候，这个被传进来的流叫做：节点流。
       // 外部负责包装的这个流，叫做：包装流，还有一个名字叫做：处理流。
       // 像当前这个程序来说：FileReader就是一个节点流。BufferedReader就是包装流/处理流。
       BufferedReader br = new BufferedReader(reader);

       // 读一行
       /*String firstLine = br.readLine();
       System.out.println(firstLine);

       String secondLine = br.readLine();
       System.out.println(secondLine);

       String line3 = br.readLine();
       System.out.println(line3);*/

       // br.readLine()方法读取一个文本行，但不带换行符。
       String s = null;
       while((s = br.readLine()) != null){
           System.out.print(s);
       }

       // 关闭流
       // 对于包装流来说，只需要关闭最外层流就行，里面的节点流会自动关闭。（可以看源代码。）
       br.close();
   }
}

```
```java
package com.yxj.java.io;

import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.InputStream;
import java.io.InputStreamReader;

/*
   转换流：InputStreamReader
*/
public class BufferedReaderTest02 {
   public static void main(String[] args) throws Exception{

       /*// 字节流
       FileInputStream in = new FileInputStream("Copy02.java");

       // 通过转换流转换（InputStreamReader将字节流转换成字符流。）
       // in是节点流。reader是包装流。
       InputStreamReader reader = new InputStreamReader(in);

       // 这个构造方法只能传一个字符流。不能传字节流。
       // reader是节点流。br是包装流。
       BufferedReader br = new BufferedReader(reader);*/

       // 合并
       BufferedReader br = new BufferedReader(new InputStreamReader(new FileInputStream("Copy02.java")));

       String line = null;
       while((line = br.readLine()) != null){
           System.out.println(line);
       }

       // 关闭最外层
       br.close();
   }
}

```
### 3.1.6 BufferWriter
```java
package com.yxj.java.io;

import java.io.BufferedWriter;
import java.io.FileOutputStream;
import java.io.FileWriter;
import java.io.OutputStreamWriter;

/*
BufferedWriter：带有缓冲的字符输出流。
OutputStreamWriter：转换流
*/
public class BufferedWriterTest {
   public static void main(String[] args) throws Exception{
       // 带有缓冲区的字符输出流
       //BufferedWriter out = new BufferedWriter(new FileWriter("copy"));

       BufferedWriter out = new BufferedWriter(new OutputStreamWriter(new FileOutputStream("copy", true)));
       // 开始写。
       out.write("hello world!");
       out.write("\n");
       out.write("hello kitty!");
       // 刷新
       out.flush();
       // 关闭最外层
       out.close();
   }
}

```
### 3.1.7 DataInputStream
```java
package com.yxj.java.io;

import java.io.DataInputStream;
import java.io.FileInputStream;

/*
DataInputStream:数据字节输入流。
DataOutputStream写的文件，只能使用DataInputStream去读。并且读的时候你需要提前知道写入的顺序。
读的顺序需要和写的顺序一致。才可以正常取出数据。

*/
public class DataInputStreamTest01 {
   public static void main(String[] args) throws Exception{
       DataInputStream dis = new DataInputStream(new FileInputStream("data"));
       // 开始读
       byte b = dis.readByte();
       short s = dis.readShort();
       int i = dis.readInt();
       long l = dis.readLong();
       float f = dis.readFloat();
       double d = dis.readDouble();
       boolean sex = dis.readBoolean();
       char c = dis.readChar();

       System.out.println(b);
       System.out.println(s);
       System.out.println(i + 1000);
       System.out.println(l);
       System.out.println(f);
       System.out.println(d);
       System.out.println(sex);
       System.out.println(c);

       dis.close();
   }
}

```
### 3.1.8 DataOutputStream
```java
package com.yxj.java.io;

import java.io.DataOutputStream;
import java.io.FileOutputStream;

/*
java.io.DataOutputStream：数据专属的流。
这个流可以将数据连同数据的类型一并写入文件。
注意：这个文件不是普通文本文档。（这个文件使用记事本打不开。）
*/
public class DataOutputStreamTest {
   public static void main(String[] args) throws Exception{
       // 创建数据专属的字节输出流
       DataOutputStream dos = new DataOutputStream(new FileOutputStream("data"));
       // 写数据
       byte b = 100;
       short s = 200;
       int i = 300;
       long l = 400L;
       float f = 3.0F;
       double d = 3.14;
       boolean sex = false;
       char c = 'a';
       // 写
       dos.writeByte(b); // 把数据以及数据的类型一并写入到文件当中。
       dos.writeShort(s);
       dos.writeInt(i);
       dos.writeLong(l);
       dos.writeFloat(f);
       dos.writeDouble(d);
       dos.writeBoolean(sex);
       dos.writeChar(c);

       // 刷新
       dos.flush();
       // 关闭最外层
       dos.close();
   }
}

```
### 3.1.9 PrintStream
```java
package com.yxj.java.io;

import java.io.FileOutputStream;
import java.io.PrintStream;

/*
java.io.PrintStream：标准的字节输出流。默认输出到控制台。
*/
public class PrintStreamTest {
   public static void main(String[] args) throws Exception{

       // 联合起来写
       System.out.println("hello world!");

       // 分开写
       PrintStream ps = System.out;
       ps.println("hello zhangsan");
       ps.println("hello lisi");
       ps.println("hello wangwu");

       // 标准输出流不需要手动close()关闭。
       // 可以改变标准输出流的输出方向吗？ 可以
       /*
       // 这些是之前System类使用过的方法和属性。
       System.gc();
       System.currentTimeMillis();
       PrintStream ps2 = System.out;
       System.exit(0);
       System.arraycopy(....);
        */

       // 标准输出流不再指向控制台，指向“log”文件。
       PrintStream printStream = new PrintStream(new FileOutputStream("log"));
       // 修改输出方向，将输出方向修改到"log"文件。
       System.setOut(printStream);
       // 再输出
       System.out.println("hello world");
       System.out.println("hello kitty");
       System.out.println("hello zhangsan");

   }
}

```
### 3.1.10 日志工具
```java
package com.yxj.java.io;

import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.PrintStream;
import java.text.SimpleDateFormat;
import java.util.Date;

/*
日志工具
*/
public class Logger {
   /*
   记录日志的方法。
    */
   public static void log(String msg) {
       try {
           // 指向一个日志文件
           PrintStream out = new PrintStream(new FileOutputStream("log.txt", true));
           // 改变输出方向
           System.setOut(out);
           // 日期当前时间
           Date nowTime = new Date();
           SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss SSS");
           String strTime = sdf.format(nowTime);

           System.out.println(strTime + ": " + msg);
       } catch (FileNotFoundException e) {
           e.printStackTrace();
       }
   }
}

```
```java
package com.yxj.java.io;

public class LogTest {
   public static void main(String[] args) {
       //测试工具类是否好用
       Logger.log("调用了System类的gc()方法，建议启动垃圾回收");
       Logger.log("调用了UserService的doSome()方法");
       Logger.log("用户尝试进行登录，验证失败");
       Logger.log("我非常喜欢这个记录日志的工具哦！");
   }
}

```
### 3.1.11 File
```java
package com.yxj.java.io;

import java.io.File;

/*
File
   1、File类和四大家族没有关系，所以File类不能完成文件的读和写。
   2、File对象代表什么？
       文件和目录路径名的抽象表示形式。
       C:\Drivers 这是一个File对象
       C:\Drivers\Lan\Realtek\Readme.txt 也是File对象。
       一个File对象有可能对应的是目录，也可能是文件。
       File只是一个路径名的抽象表示形式。
   3、需要掌握File类中常用的方法
*/
public class FileTest01 {
   public static void main(String[] args) throws Exception {
       // 创建一个File对象
       File f1 = new File("D:\\file");

       // 判断是否存在！
       System.out.println(f1.exists());

       // 如果D:\file不存在，则以文件的形式创建出来
       /*if(!f1.exists()) {
           // 以文件形式新建
           f1.createNewFile();
       }*/

       // 如果D:\file不存在，则以目录的形式创建出来
       /*if(!f1.exists()) {
           // 以目录的形式新建。
           f1.mkdir();
       }*/

       // 可以创建多重目录吗？
       File f2 = new File("D:/a/b/c/d/e/f");
       /*if(!f2.exists()) {
           // 多重目录的形式新建。
           f2.mkdirs();
       }*/

       File f3 = new File("D:\\course\\01-开课\\学习方法.txt");
       // 获取文件的父路径
       String parentPath = f3.getParent();
       System.out.println(parentPath); //D:\course\01-开课
       File parentFile = f3.getParentFile();
       System.out.println("获取绝对路径：" + parentFile.getAbsolutePath());

       File f4 = new File("copy");
       System.out.println("绝对路径：" + f4.getAbsolutePath()); // C:\Users\Administrator\IdeaProjects\javase\copy

   }
}

```
```java
package com.yxj.java.io;

import java.io.File;
import java.text.SimpleDateFormat;
import java.util.Date;

/*
File类的常用方法
*/
public class FileTest02 {
   public static void main(String[] args) {

       File f1 = new File("D:\\course\\01-开课\\开学典礼.ppt");
       // 获取文件名
       System.out.println("文件名：" + f1.getName());

       // 判断是否是一个目录
       System.out.println(f1.isDirectory()); // false

       // 判断是否是一个文件
       System.out.println(f1.isFile()); // true

       // 获取文件最后一次修改时间
       long haoMiao = f1.lastModified(); // 这个毫秒是从1970年到现在的总毫秒数。
       // 将总毫秒数转换成日期?????
       Date time = new Date(haoMiao);
       SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss SSS");
       String strTime = sdf.format(time);
       System.out.println(strTime);

       // 获取文件大小
       System.out.println(f1.length()); //216064字节。
   }
}

```
```java
package com.yxj.java.io;

import java.io.File;

/*
File中的listFiles方法。
*/
public class FileTest03 {
   public static void main(String[] args) {
       // File[] listFiles()
       // 获取当前目录下所有的子文件。
       File f = new File("D:\\course\\01-开课");
       File[] files = f.listFiles();
       // foreach
       for(File file : files){
           //System.out.println(file.getAbsolutePath());
           System.out.println(file.getName());
       }
   }
}

```
### 3.1.12 序列化
```java
ObjectOutputStreamTest01
package com.yxj.java.io;

import com.yxj.java.bean.Student;

import java.io.FileOutputStream;
import java.io.ObjectOutputStream;

/*
1、java.io.NotSerializableException:
   Student对象不支持序列化！！！！

2、参与序列化和反序列化的对象，必须实现Serializable接口。

3、注意：通过源代码发现，Serializable接口只是一个标志接口：
   public interface Serializable {
   }
   这个接口当中什么代码都没有。
   那么它起到一个什么作用呢？
       起到标识的作用，标志的作用，java虚拟机看到这个类实现了这个接口，可能会对这个类进行特殊待遇。
       Serializable这个标志接口是给java虚拟机参考的，java虚拟机看到这个接口之后，会为该类自动生成
       一个序列化版本号。

4、序列化版本号有什么用呢？
   java.io.InvalidClassException:
       com.yxj.java.bean.Student;
       local class incompatible:
           stream classdesc serialVersionUID = -684255398724514298（十年后）,
           local class serialVersionUID = -3463447116624555755（十年前）

   java语言中是采用什么机制来区分类的？
       第一：首先通过类名进行比对，如果类名不一样，肯定不是同一个类。
       第二：如果类名一样，再怎么进行类的区别？靠序列化版本号进行区分。

   小鹏编写了一个类：com.yxj.java.bean.Student implements Serializable
   胡浪编写了一个类：com.yxj.java.bean.Student implements Serializable
   不同的人编写了同一个类，但“这两个类确实不是同一个类”。这个时候序列化版本就起上作用了。
   对于java虚拟机来说，java虚拟机是可以区分开这两个类的，因为这两个类都实现了Serializable接口，
   都有默认的序列化版本号，他们的序列化版本号不一样。所以区分开了。（这是自动生成序列化版本号的好处）

   请思考？
       这种自动生成序列化版本号有什么缺陷？
           这种自动生成的序列化版本号缺点是：一旦代码确定之后，不能进行后续的修改，
           因为只要修改，必然会重新编译，此时会生成全新的序列化版本号，这个时候java
           虚拟机会认为这是一个全新的类。（这样就不好了！）

   最终结论：
       凡是一个类实现了Serializable接口，建议给该类提供一个固定不变的序列化版本号。
       这样，以后这个类即使代码修改了，但是版本号不变，java虚拟机会认为是同一个类。

*/
public class ObjectOutputStreamTest01 {
   public static void main(String[] args) throws Exception{
       // 创建java对象
       Student s = new Student(1111, "zhangsan");
       // 序列化
       ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("students"));

       // 序列化对象
       oos.writeObject(s);

       // 刷新
       oos.flush();
       // 关闭
       oos.close();
   }
}

```
```java
package com.yxj.java.bean;

import java.io.Serializable;

public class Student implements Serializable {

   // IDEA工具自动生成序列化版本号。
   //private static final long serialVersionUID = -7998917368642754840L;

   // Java虚拟机看到Serializable接口之后，会自动生成一个序列化版本号。
   // 这里没有手动写出来，java虚拟机会默认提供这个序列化版本号。
   // 建议将序列化版本号手动的写出来。不建议自动生成
   private static final long serialVersionUID = 1L; // java虚拟机识别一个类的时候先通过类名，如果类名一致，再通过序列化版本号。

   private int no;
   //private String name;

   // 过了很久，Student这个类源代码改动了。
   // 源代码改动之后，需要重新编译，编译之后生成了全新的字节码文件。
   // 并且class文件再次运行的时候，java虚拟机生成的序列化版本号也会发生相应的改变。
   private int age;
   private String email;
   private String address;

   public Student() {
   }

   public Student(int no, String name) {
       this.no = no;
       //this.name = name;
   }

   public int getNo() {
       return no;
   }

   public void setNo(int no) {
       this.no = no;
   }

   /*public String getName() {
       return name;
   }*/

   /*public void setName(String name) {
       this.name = name;
   }*/

   /*@Override
   public String toString() {
       return "Student{" +
               "no=" + no +
               ", name='" + name + '\'' +
               '}';
   }*/

   @Override
   public String toString() {
       return "Student{" +
               "no=" + no +
               ", age=" + age +
               ", email='" + email + '\'' +
               ", address='" + address + '\'' +
               '}';
   }
}

```
### 3.1.13 反序列化
```java
package com.yxj.java.io;

import com.yxj.java.bean.User;

import java.io.FileInputStream;
import java.io.ObjectInputStream;
import java.util.List;

/*
反序列化集合
*/
public class ObjectInputStreamTest02 {
   public static void main(String[] args) throws Exception{
       ObjectInputStream ois = new ObjectInputStream(new FileInputStream("users"));
       //Object obj = ois.readObject();
       //System.out.println(obj instanceof List);
       List<User> userList = (List<User>)ois.readObject();
       for(User user : userList){
           System.out.println(user);
       }
       ois.close();
   }
}

```
### 3.1.14 ObjectOutputStream
```java
package com.yxj.java.io;

import com.yxj.java.bean.Student;

import java.io.FileOutputStream;
import java.io.ObjectOutputStream;

/*
1、java.io.NotSerializableException:
   Student对象不支持序列化！！！！

2、参与序列化和反序列化的对象，必须实现Serializable接口。

3、注意：通过源代码发现，Serializable接口只是一个标志接口：
   public interface Serializable {
   }
   这个接口当中什么代码都没有。
   那么它起到一个什么作用呢？
       起到标识的作用，标志的作用，java虚拟机看到这个类实现了这个接口，可能会对这个类进行特殊待遇。
       Serializable这个标志接口是给java虚拟机参考的，java虚拟机看到这个接口之后，会为该类自动生成
       一个序列化版本号。

4、序列化版本号有什么用呢？
   java.io.InvalidClassException:
       com.yxj.java.bean.Student;
       local class incompatible:
           stream classdesc serialVersionUID = -684255398724514298（十年后）,
           local class serialVersionUID = -3463447116624555755（十年前）

   java语言中是采用什么机制来区分类的？
       第一：首先通过类名进行比对，如果类名不一样，肯定不是同一个类。
       第二：如果类名一样，再怎么进行类的区别？靠序列化版本号进行区分。

   小鹏编写了一个类：com.yxj.java.bean.Student implements Serializable
   胡浪编写了一个类：com.yxj.java.bean.Student implements Serializable
   不同的人编写了同一个类，但“这两个类确实不是同一个类”。这个时候序列化版本就起上作用了。
   对于java虚拟机来说，java虚拟机是可以区分开这两个类的，因为这两个类都实现了Serializable接口，
   都有默认的序列化版本号，他们的序列化版本号不一样。所以区分开了。（这是自动生成序列化版本号的好处）

   请思考？
       这种自动生成序列化版本号有什么缺陷？
           这种自动生成的序列化版本号缺点是：一旦代码确定之后，不能进行后续的修改，
           因为只要修改，必然会重新编译，此时会生成全新的序列化版本号，这个时候java
           虚拟机会认为这是一个全新的类。（这样就不好了！）

   最终结论：
       凡是一个类实现了Serializable接口，建议给该类提供一个固定不变的序列化版本号。
       这样，以后这个类即使代码修改了，但是版本号不变，java虚拟机会认为是同一个类。

*/
public class ObjectOutputStreamTest01 {
   public static void main(String[] args) throws Exception{
       // 创建java对象
       Student s = new Student(1111, "zhangsan");
       // 序列化
       ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("students"));

       // 序列化对象
       oos.writeObject(s);

       // 刷新
       oos.flush();
       // 关闭
       oos.close();
   }
}

```
```java
package com.yxj.java.io;

import com.yxj.java.bean.User;

import java.io.FileOutputStream;
import java.io.ObjectOutputStream;
import java.util.ArrayList;
import java.util.List;

/*
一次序列化多个对象呢？
   可以，可以将对象放到集合当中，序列化集合。
提示：
   参与序列化的ArrayList集合以及集合中的元素User都需要实现 java.io.Serializable接口。
*/
public class ObjectOutputStreamTest02 {
   public static void main(String[] args) throws Exception{
       List<User> userList = new ArrayList<>();
       userList.add(new User(1,"zhangsan"));
       userList.add(new User(2, "lisi"));
       userList.add(new User(3, "wangwu"));
       ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("users"));

       // 序列化一个集合，这个集合对象中放了很多其他对象。
       oos.writeObject(userList);

       oos.flush();
       oos.close();
   }
}

```

```java
IOPropertiesTest01.java
package com.yxj.java.io;

import java.io.FileReader;
import java.util.Properties;

/*
IO+Properties的联合应用。
非常好的一个设计理念：
   以后经常改变的数据，可以单独写到一个文件中，使用程序动态读取。
   将来只需要修改这个文件的内容，java代码不需要改动，不需要重新
   编译，服务器也不需要重启。就可以拿到动态的信息。

   类似于以上机制的这种文件被称为配置文件。
   并且当配置文件中的内容格式是：
       key1=value
       key2=value
   的时候，我们把这种配置文件叫做属性配置文件。

   java规范中有要求：属性配置文件建议以.properties结尾，但这不是必须的。
   这种以.properties结尾的文件在java中被称为：属性配置文件。
   其中Properties是专门存放属性配置文件内容的一个类。
*/
public class IoPropertiesTest01 {
   public static void main(String[] args) throws Exception{
       /*
       Properties是一个Map集合，key和value都是String类型。
       想将userinfo文件中的数据加载到Properties对象当中。
        */
       // 新建一个输入流对象
       FileReader reader = new FileReader("chapter23/userinfo.properties");

       // 新建一个Map集合
       Properties pro = new Properties();

       // 调用Properties对象的load方法将文件中的数据加载到Map集合中。
       pro.load(reader); // 文件中的数据顺着管道加载到Map集合中，其中等号=左边做key，右边做value

       // 通过key来获取value呢？
       String username = pro.getProperty("username");
       System.out.println(username);

       String password = pro.getProperty("password");
       System.out.println(password);

       String data = pro.getProperty("data");
       System.out.println(data);

       String usernamex = pro.getProperty("usernamex

");
       System.out.println(usernamex);
   }
}

```


## 4.1 线程
```markdown
1、关于线程的调度

1.1、常见的线程调度模型有哪些？
抢占式调度模型：
哪个线程的优先级比较高，抢到的CPU时间片的概率就高一些/多一些。
java采用的就是抢占式调度模型。

均分式调度模型：
平均分配CPU时间片。每个线程占有的CPU时间片时间长度一样。
平均分配，一切平等。
有一些编程语言，线程调度模型采用的是这种方式。

1.2、java中提供了哪些方法是和线程调度有关系的呢？

实例方法：
void setPriority(int newPriority) 设置线程的优先级
int getPriority() 获取线程优先级
最低优先级1
默认优先级是5
最高优先级10
优先级比较高的获取CPU时间片可能会多一些。（但也不完全是，大
概率是多的。）

静态方法：
static void yield()  让位方法
暂停当前正在执行的线程对象，并执行其他线程
yield()方法不是阻塞方法。让当前线程让位，让给其它线程使用。
yield()方法的执行会让当前线程从“运行状态”回到“就绪状态”。
注意：在回到就绪之后，有可能还会再次抢到。

实例方法：
void join()  
合并线程
class MyThread1 extends Thread {
public void doSome(){
MyThread2 t = new MyThread2();
t.join(); // 当前线程进入阻塞，t线程执行，直到t线程结束。当前线程才可以继续。
}
}

class MyThread2 extends Thread{

}

2、关于多线程并发环境下，数据的安全问题。

2.1、为什么这个是重点？
以后在开发中，我们的项目都是运行在服务器当中，
而服务器已经将线程的定义，线程对象的创建，线程
的启动等，都已经实现完了。这些代码我们都不需要
编写。

最重要的是：你要知道，你编写的程序需要放到一个
多线程的环境下运行，你更需要关注的是这些数据
在多线程并发的环境下是否是安全的。（重点：*****）

2.2、什么时候数据在多线程并发的环境下会存在安全问题呢？
三个条件：
条件1：多线程并发。
条件2：有共享数据。
条件3：共享数据有修改的行为。

满足以上3个条件之后，就会存在线程安全问题。

2.3、怎么解决线程安全问题呢？
当多线程并发的环境下，有共享数据，并且这个数据还会被修改，此
时就存在
线程安全问题，怎么解决这个问题？
线程排队执行。（不能并发）。
用排队执行解决线程安全问题。
这种机制被称为：线程同步机制。

专业术语叫做：线程同步，实际上就是线程不能并发了，线程
必须排队执行。

怎么解决线程安全问题呀？
使用“线程同步机制”。

线程同步就是线程排队了，线程排队了就会牺牲一部分效率，没办法，数据安全
第一位，只有数据安全了，我们才可以谈效率。数据不安全，没有效率的事儿。

2.4、说到线程同步这块，涉及到这两个专业术语：

异步编程模型：
线程t1和线程t2，各自执行各自的，t1不管t2，t2不管t1，
谁也不需要等谁，这种编程模型叫做：异步编程模型。
其实就是：多线程并发（效率较高。）

异步就是并发。

同步编程模型：
线程t1和线程t2，在线程t1执行的时候，必须等待t2线程执行
结束，或者说在t2线程执行的时候，必须等待t1线程执行结束，
两个线程之间发生了等待关系，这就是同步编程模型。
效率较低。线程排队执行。

同步就是排队。

3、Java中有三大变量？【重要的内容。】

实例变量：在堆中。

静态变量：在方法区。

局部变量：在栈中。

以上三大变量中：
局部变量永远都不会存在线程安全问题。
因为局部变量不共享。（一个线程一个栈。）
局部变量在栈中。所以局部变量永远都不会共享。

实例变量在堆中，堆只有1个。
静态变量在方法区中，方法区只有1个。
堆和方法区都是多线程共享的，所以可能存在线程安全问题。

局部变量+常量：不会有线程安全问题。
成员变量：可能会有线程安全问题。

4、如果使用局部变量的话：
建议使用：StringBuilder。
因为局部变量不存在线程安全问题。选择StringBuilder。
StringBuffer效率比较低。

ArrayList是非线程安全的。
Vector是线程安全的。
HashMap HashSet是非线程安全的。
Hashtable是线程安全的。

5、总结：
synchronized有三种写法：

第一种：同步代码块
灵活
synchronized(线程共享对象){
同步代码块;
}

第二种：在实例方法上使用synchronized
表示共享对象一定是this
并且同步代码块是整个方法体。

第三种：在静态方法上使用synchronized
表示找类锁。
类锁永远只有1把。
就算创建了100个对象，那类锁也只有一把。

对象锁：1个对象1把锁，100个对象100把锁。
类锁：100个对象，也可能只是1把类锁。

6、聊一聊，我们以后开发中应该怎么解决线程安全问题？

是一上来就选择线程同步吗？synchronized
不是，synchronized会让程序的执行效率降低，用户体验不好。
系统的用户吞吐量降低。用户体验差。在不得已的情况下再选择
线程同步机制。

第一种方案：尽量使用局部变量代替“实例变量和静态变量”。

第二种方案：如果必须是实例变量，那么可以考虑创建多个对象，这样
实例变量的内存就不共享了。（一个线程对应1个对象，100个线程对应100个对象，
对象不共享，就没有数据安全问题了。）

第三种方案：如果不能使用局部变量，对象也不能创建多个，这个时候
就只能选择synchronized了。线程同步机制。

7、线程这块还有那些内容呢？列举一下
7.1、守护线程
7.2、定时器
7.3、实现线程的第三种方式：FutureTask方式，实现Callable接口。（JDK8新特性。）
7.4、关于Object类中的wait和notify方法。（生产者和消费者模式！）

1、线程这块还有那些内容呢？列举一下

1.1、守护线程

java语言中线程分为两大类：
一类是：用户线程
一类是：守护线程（后台线程）
其中具有代表性的就是：垃圾回收线程（守护线程）。

守护线程的特点：
一般守护线程是一个死循环，所有的用户线程只要结束，
守护线程自动结束。

注意：主线程main方法是一个用户线程。

守护线程用在什么地方呢？
每天00:00的时候系统数据自动备份。
这个需要使用到定时器，并且我们可以将定时器设置为守护线程。
一直在那里看着，每到00:00的时候就备份一次。所有的用户线程
如果结束了，守护线程自动退出，没有必要进行数据备份了。

1.2、定时器
定时器的作用：
间隔特定的时间，执行特定的程序。

每周要进行银行账户的总账操作。
每天要进行数据的备份操作。

在实际的开发中，每隔多久执行一段特定的程序，这种需求是很常见的，
那么在java中其实可以采用多种方式实现：

可以使用sleep方法，睡眠，设置睡眠时间，每到这个时间点醒来，执行
任务。这种方式是最原始的定时器。（比较low）

在java的类库中已经写好了一个定时器：java.util.Timer，可以直接拿来用。
不过，这种方式在目前的开发中也很少用，因为现在有很多高级框架都是支持
定时任务的。

在实际的开发中，目前使用较多的是Spring框架中提供的SpringTask框架，
这个框架只要进行简单的配置，就可以完成定时器的任务。


1.3、实现线程的第三种方式：实现Callable接口。（JDK8新特性。）
这种方式实现的线程可以获取线程的返回值。
之前讲解的那两种方式是无法获取线程返回值的，因为run方法返回void。

思考：
系统委派一个线程去执行一个任务，该线程执行完任务之后，可能
会有一个执行结果，我们怎么能拿到这个执行结果呢？
使用第三种方式：实现Callable接口方式。


1.4、关于Object类中的wait和notify方法。（生产者和消费者模式！）

第一：wait和notify方法不是线程对象的方法，是java中任何一个java对象
都有的方法，因为这两个方式是Object类中自带的。
wait方法和notify方法不是通过线程对象调用，
不是这样的：t.wait()，也不是这样的：t.notify()..不对。

第二：wait()方法作用？
Object o = new Object();
o.wait();

表示：
让正在o对象上活动的线程进入等待状态，无期限等待，
直到被唤醒为止。
o.wait();方法的调用，会让“当前线程（正在o对象上
活动的线程）”进入等待状态。

第三：notify()方法作用？
Object o = new Object();
o.notify();

表示：
唤醒正在o对象上等待的线程。

还有一个notifyAll()方法：
这个方法是唤醒o对象上处于等待的所有线程。
```
### 4.1.1  实现线程的第一种方式
```java
package com.yxj.java.thread;
/*
实现线程的第一种方式：
编写一个类，直接继承java.lang.Thread，重写run方法。

怎么创建线程对象？ new就行了。
怎么启动线程呢？ 调用线程对象的start()方法。

注意：
    亘古不变的道理：
        方法体当中的代码永远都是自上而下的顺序依次逐行执行的。

以下程序的输出结果有这样的特点：
    有先有后。
    有多有少。

 */
public class ThreadTest02 {
    public static void main(String[] args) {
        // 这里是main方法，这里的代码属于主线程，在主栈中运行。
        // 新建一个分支线程对象
        MyThread t = new MyThread();
        // 启动线程
        //t.run(); // 不会启动线程，不会分配新的分支栈。（这种方式就是单线程。）
        // start()方法的作用是：启动一个分支线程，在JVM中开辟一个新的栈空间，这段代码任务完成之后，瞬间就结束了。
        // 这段代码的任务只是为了开启一个新的栈空间，只要新的栈空间开出来，start()方法就结束了。线程就启动成功了。
        // 启动成功的线程会自动调用run方法，并且run方法在分支栈的栈底部（压栈）。
        // run方法在分支栈的栈底部，main方法在主栈的栈底部。run和main是平级的。
        t.start();
        // 这里的代码还是运行在主线程中。
        for(int i = 0; i < 1000; i++){
            System.out.println("主线程--->" + i);
        }
    }
}

class MyThread extends Thread {
    @Override
    public void run() {
        // 编写程序，这段程序运行在分支线程中（分支栈）。
        for(int i = 0; i < 1000; i++){
            System.out.println("分支线程--->" + i);
        }
    }
}

```
### 4.1.2 实现线程的第二种方式
```java
package com.yxj.java.thread;
/*
实现线程的第二种方式，编写一个类实现java.lang.Runnable接口。
 */
public class ThreadTest03 {
    public static void main(String[] args) {
        // 创建一个可运行的对象
        //MyRunnable r = new MyRunnable();
        // 将可运行的对象封装成一个线程对象
        //Thread t = new Thread(r);
        Thread t = new Thread(new MyRunnable()); // 合并代码
        // 启动线程
        t.start();

        for(int i = 0; i < 100; i++){
            System.out.println("主线程--->" + i);
        }
    }
}

// 这并不是一个线程类，是一个可运行的类。它还不是一个线程。
class MyRunnable implements Runnable {

    @Override
    public void run() {
        for(int i = 0; i < 100; i++){
            System.out.println("分支线程--->" + i);
        }
    }
}


----------------------------------------------------------------------------
package com.yxj.java.thread;

/*
采用匿名内部类可以吗？
 */
public class ThreadTest04 {
    public static void main(String[] args) {
        // 创建线程对象，采用匿名内部类方式。
        // 这是通过一个没有名字的类，new出来的对象。
        Thread t = new Thread(new Runnable(){
            @Override
            public void run() {
                for(int i = 0; i < 100; i++){
                    System.out.println("t线程---> " + i);
                }
            }
        });

        // 启动线程
        t.start();

        for(int i = 0; i < 100; i++){
            System.out.println("main线程---> " + i);
        }
    }
}

```
```java
package com.yxj.java.thread;
/*
1、怎么获取当前线程对象？
    Thread t = Thread.currentThread();
    返回值t就是当前线程。

2、获取线程对象的名字
    String name = 线程对象.getName();

3、修改线程对象的名字
    线程对象.setName("线程名字");

4、当线程没有设置名字的时候，默认的名字有什么规律？（了解一下）
    Thread-0
    Thread-1
    Thread-2
    Thread-3
    .....
 */
public class ThreadTest05 {
    public void doSome(){
        // 这样就不行了
        //this.getName();
        //super.getName();
        // 但是这样可以
        String name = Thread.currentThread().getName();
        System.out.println("------->" + name);
    }

    public static void main(String[] args) {
        ThreadTest05 tt = new ThreadTest05();
        tt.doSome();

        //currentThread就是当前线程对象
        // 这个代码出现在main方法当中，所以当前线程就是主线程。
        Thread currentThread = Thread.currentThread();
        System.out.println(currentThread.getName()); //main

        // 创建线程对象
        MyThread2 t = new MyThread2();
        // 设置线程的名字
        t.setName("t1");
        // 获取线程的名字
        String tName = t.getName();
        System.out.println(tName); //Thread-0

        MyThread2 t2 = new MyThread2();
        t2.setName("t2");
        System.out.println(t2.getName()); //Thread-1\
        t2.start();

        // 启动线程
        t.start();
    }
}

class MyThread2 extends Thread {
    public void run(){
        for(int i = 0; i < 100; i++){
            // currentThread就是当前线程对象。当前线程是谁呢？
            // 当t1线程执行run方法，那么这个当前线程就是t1
            // 当t2线程执行run方法，那么这个当前线程就是t2
            Thread currentThread = Thread.currentThread();
            System.out.println(currentThread.getName() + "-->" + i);

            //System.out.println(super.getName() + "-->" + i);
            //System.out.println(this.getName() + "-->" + i);
        }
    }
}


---------------------------------------------------------------------------
package com.yxj.java.thread;
/*
关于线程的sleep方法：
    static void sleep(long millis)
    1、静态方法：Thread.sleep(1000);
    2、参数是毫秒
    3、作用：让当前线程进入休眠，进入“阻塞状态”，放弃占有CPU时间片，让给其它线程使用。
        这行代码出现在A线程中，A线程就会进入休眠。
        这行代码出现在B线程中，B线程就会进入休眠。
    4、Thread.sleep()方法，可以做到这种效果：
        间隔特定的时间，去执行一段特定的代码，每隔多久执行一次。
 */
public class ThreadTest06 {
    public static void main(String[] args) {

        // 让当前线程进入休眠，睡眠5秒
        // 当前线程是主线程！！！
        /*try {
            Thread.sleep(1000 * 5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }*/

        // 5秒之后执行这里的代码
        //System.out.println("hello world!");

        for(int i = 0; i < 10; i++){
            System.out.println(Thread.currentThread().getName() + "--->" + i);

            // 睡眠1秒
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

---------------------------------------------------------------------------
package com.yxj.java.thread;
/*
关于Thread.sleep()方法的一个面试题：
 */
public class ThreadTest07 {
    public static void main(String[] args) {
        // 创建线程对象
        Thread t = new MyThread3();
        t.setName("t");
        t.start();

        // 调用sleep方法
        try {
            // 问题：这行代码会让线程t进入休眠状态吗？
            t.sleep(1000 * 5); // 在执行的时候还是会转换成：Thread.sleep(1000 * 5);
                                     // 这行代码的作用是：让当前线程进入休眠，也就是说main线程进入休眠。
                                     // 这样代码出现在main方法中，main线程睡眠。
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // 5秒之后这里才会执行。
        System.out.println("hello World!");
    }
}

class MyThread3 extends Thread {
    public void run(){
        for(int i = 0; i < 10000; i++){
            System.out.println(Thread.currentThread().getName() + "--->" + i);
        }
    }
}


---------------------------------------------------------------------------
package com.yxj.java.thread;
/*
sleep睡眠太久了，如果希望半道上醒来，你应该怎么办？也就是说怎么叫醒一个正在睡眠的线程？？
    注意：这个不是终断线程的执行，是终止线程的睡眠。
 */
public class ThreadTest08 {
    public static void main(String[] args) {
        Thread t = new Thread(new MyRunnable2());
        t.setName("t");
        t.start();

        // 希望5秒之后，t线程醒来（5秒之后主线程手里的活儿干完了。）
        try {
            Thread.sleep(1000 * 5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // 终断t线程的睡眠（这种终断睡眠的方式依靠了java的异常处理机制。）
        t.interrupt(); // 干扰，一盆冷水过去！
    }
}

class MyRunnable2 implements Runnable {

    // 重点：run()当中的异常不能throws，只能try catch
    // 因为run()方法在父类中没有抛出任何异常，子类不能比父类抛出更多的异常。
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + "---> begin");
        try {
            // 睡眠1年
            Thread.sleep(1000 * 60 * 60 * 24 * 365);
        } catch (InterruptedException e) {
            // 打印异常信息
            //e.printStackTrace();
        }
        //1年之后才会执行这里
        System.out.println(Thread.currentThread().getName() + "---> end");

        // 调用doOther
        //doOther();
    }

    // 其它方法可以throws
    /*public void doOther() throws Exception{

    }*/
}

---------------------------------------------------------------------------
package com.yxj.java.thread;
/*
在java中怎么强行终止一个线程的执行。
    这种方式存在很大的缺点：容易丢失数据。因为这种方式是直接将线程杀死了，
    线程没有保存的数据将会丢失。不建议使用。
 */
public class ThreadTest09 {
    public static void main(String[] args) {
        Thread t = new Thread(new MyRunnable3());
        t.setName("t");
        t.start();

        // 模拟5秒
        try {
            Thread.sleep(1000 * 5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // 5秒之后强行终止t线程
        t.stop(); // 已过时（不建议使用。）
    }
}

class MyRunnable3 implements Runnable {

    @Override
    public void run() {
        for(int i = 0; i < 10; i++){
            System.out.println(Thread.currentThread().getName() + "--->" + i);
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}


---------------------------------------------------------------------------
package com.yxj.java.thread;
/*
怎么合理的终止一个线程的执行。这种方式是很常用的。
 */
public class ThreadTest10 {
    public static void main(String[] args) {
        MyRunable4 r = new MyRunable4();
        Thread t = new Thread(r);
        t.setName("t");
        t.start();

        // 模拟5秒
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // 终止线程
        // 你想要什么时候终止t的执行，那么你把标记修改为false，就结束了。
        r.run = false;
    }
}

class MyRunable4 implements Runnable {

    // 打一个布尔标记
    boolean run = true;

    @Override
    public void run() {
        for (int i = 0; i < 10; i++){
            if(run){
                System.out.println(Thread.currentThread().getName() + "--->" + i);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }else{
                // return就结束了，你在结束之前还有什么没保存的。
                // 在这里可以保存呀。
                //save....

                //终止当前线程
                return;
            }
        }
    }
}

```
### 4.1.3 线程的优先级
```java
package com.yxj.java.thread;

/*
了解：关于线程的优先级
 */
public class ThreadTest11 {
    public static void main(String[] args) {
        // 设置主线程的优先级为1
        Thread.currentThread().setPriority(1);

        /*System.out.println("最高优先级" + Thread.MAX_PRIORITY);
        System.out.println("最低优先级" + Thread.MIN_PRIORITY);
        System.out.println("默认优先级" + Thread.NORM_PRIORITY);*/

        // 获取当前线程对象，获取当前线程的优先级
        Thread currentThread = Thread.currentThread();
        // main线程的默认优先级是：5
        //System.out.println(currentThread.getName() + "线程的默认优先级是：" + currentThread.getPriority());

        Thread t = new Thread(new MyRunnable5());
        t.setPriority(10);
        t.setName("t");
        t.start();

        // 优先级较高的，只是抢到的CPU时间片相对多一些。
        // 大概率方向更偏向于优先级比较高的。
        for(int i = 0; i < 10000; i++){
            System.out.println(Thread.currentThread().getName() + "-->" + i);
        }


    }
}

class MyRunnable5 implements Runnable {

    @Override
    public void run() {
        // 获取线程优先级
        //System.out.println(Thread.currentThread().getName() + "线程的默认优先级：" + Thread.currentThread().getPriority());
        for(int i = 0; i < 10000; i++){
            System.out.println(Thread.currentThread().getName() + "-->" + i);
        }
    }
}

```

```java
Thread.yield()
package com.yxj.java.thread;

/*
让位，当前线程暂停，回到就绪状态，让给其它线程。
静态方法：Thread.yield();
 */
public class ThreadTest12 {
    public static void main(String[] args) {
        Thread t = new Thread(new MyRunnable6());
        t.setName("t");
        t.start();

        for(int i = 1; i <= 10000; i++) {
            System.out.println(Thread.currentThread().getName() + "--->" + i);
        }
    }
}

class MyRunnable6 implements Runnable {

    @Override
    public void run() {
        for(int i = 1; i <= 10000; i++) {
            //每100个让位一次。
            if(i % 100 == 0){
                Thread.yield(); // 当前线程暂停一下，让给主线程。
            }
            System.out.println(Thread.currentThread().getName() + "--->" + i);
        }
    }
}

```
```markdown
join()
```

```java
package com.yxj.java.thread;

/*
线程合并
 */
public class ThreadTest13 {
    public static void main(String[] args) {
        System.out.println("main begin");

        Thread t = new Thread(new MyRunnable7());
        t.setName("t");
        t.start();

        //合并线程
        try {
            t.join(); // t合并到当前线程中，当前线程受阻塞，t线程执行直到结束。
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("main over");
    }
}

class MyRunnable7 implements Runnable {

    @Override
    public void run() {
        for(int i = 0; i < 10000; i++){
            System.out.println(Thread.currentThread().getName() + "--->" + i);
        }
    }
}

```
### 4.1.4 测试多线程下引发的问题
```java
package com.yxj.java.threadsafe;
/*
银行账户
    不使用线程同步机制，多线程对同一个账户进行取款，出现线程安全问题。
 */
public class Account {
    // 账号
    private String actno;
    // 余额
    private double balance;

    public Account() {
    }

    public Account(String actno, double balance) {
        this.actno = actno;
        this.balance = balance;
    }

    public String getActno() {
        return actno;
    }

    public void setActno(String actno) {
        this.actno = actno;
    }

    public double getBalance() {
        return balance;
    }

    public void setBalance(double balance) {
        this.balance = balance;
    }

    //取款的方法
    public void withdraw(double money){
        // t1和t2并发这个方法。。。。（t1和t2是两个栈。两个栈操作堆中同一个对象。）
        // 取款之前的余额
        double before = this.getBalance(); // 10000
        // 取款之后的余额
        double after = before - money;

        // 在这里模拟一下网络延迟，100%会出现问题
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // 更新余额
        // 思考：t1执行到这里了，但还没有来得及执行这行代码，t2线程进来withdraw方法了。此时一定出问题。
        this.setBalance(after);
    }
}

--------------------------------------------------------------------
package com.yxj.java.threadsafe;

public class AccountThread extends Thread {

    // 两个线程必须共享同一个账户对象。
    private Account act;

    // 通过构造方法传递过来账户对象
    public AccountThread(Account act) {
        this.act = act;
    }

    public void run(){
        // run方法的执行表示取款操作。
        // 假设取款5000
        double money = 5000;
        // 取款
        // 多线程并发执行这个方法。
        act.withdraw(money);

        System.out.println(Thread.currentThread().getName() + "对"+act.getActno()+"取款"+money+"成功，余额" + act.getBalance());
    }
}

--------------------------------------------------------------------
package com.yxj.java.threadsafe;

public class Test {
    public static void main(String[] args) {
        // 创建账户对象（只创建1个）
        Account act = new Account("act-001", 10000);
        // 创建两个线程
        Thread t1 = new AccountThread(act);
        Thread t2 = new AccountThread(act);
        // 设置name
        t1.setName("t1");
        t2.setName("t2");
        // 启动线程取款
        t1.start();
        t2.start();
    }
}

```
### 4.1.5 解决线程安全问题
```java
package com.yxj.java.threadsafe2;
/*
银行账户
    使用线程同步机制，解决线程安全问题。
 */
public class Account {
    // 账号
    private String actno;
    // 余额
    private double balance; //实例变量。

    //对象
    Object obj = new Object(); // 实例变量。（Account对象是多线程共享的，Account对象中的实例变量obj也是共享的。）

    public Account() {
    }

    public Account(String actno, double balance) {
        this.actno = actno;
        this.balance = balance;
    }

    public String getActno() {
        return actno;
    }

    public void setActno(String actno) {
        this.actno = actno;
    }

    public double getBalance() {
        return balance;
    }

    public void setBalance(double balance) {
        this.balance = balance;
    }

    //取款的方法
    public void withdraw(double money){

        //int i = 100;
        //i = 101;

        // 以下这几行代码必须是线程排队的，不能并发。
        // 一个线程把这里的代码全部执行结束之后，另一个线程才能进来。
        /*
        线程同步机制的语法是：
            synchronized(){
                // 线程同步代码块。
            }
            synchronized后面小括号中传的这个“数据”是相当关键的。
            这个数据必须是多线程共享的数据。才能达到多线程排队。

            ()中写什么？
                那要看你想让哪些线程同步。
                假设t1、t2、t3、t4、t5，有5个线程，
                你只希望t1 t2 t3排队，t4 t5不需要排队。怎么办？
                你一定要在()中写一个t1 t2 t3共享的对象。而这个
                对象对于t4 t5来说不是共享的。

            这里的共享对象是：账户对象。
            账户对象是共享的，那么this就是账户对象吧！！！
            不一定是this，这里只要是多线程共享的那个对象就行。

            在java语言中，任何一个对象都有“一把锁”，其实这把锁就是标记。（只是把它叫做锁。）
            100个对象，100把锁。1个对象1把锁。

            以下代码的执行原理？
                1、假设t1和t2线程并发，开始执行以下代码的时候，肯定有一个先一个后。
                2、假设t1先执行了，遇到了synchronized，这个时候自动找“后面共享对象”的对象锁，
                找到之后，并占有这把锁，然后执行同步代码块中的程序，在程序执行过程中一直都是
                占有这把锁的。直到同步代码块代码结束，这把锁才会释放。
                3、假设t1已经占有这把锁，此时t2也遇到synchronized关键字，也会去占有后面
                共享对象的这把锁，结果这把锁被t1占有，t2只能在同步代码块外面等待t1的结束，
                直到t1把同步代码块执行结束了，t1会归还这把锁，此时t2终于等到这把锁，然后
                t2占有这把锁之后，进入同步代码块执行程序。

                这样就达到了线程排队执行。
                这里需要注意的是：这个共享对象一定要选好了。这个共享对象一定是你需要排队
                执行的这些线程对象所共享的。
         */
        //Object obj2 = new Object();
        //synchronized (this){
        //synchronized (obj) {
        //synchronized ("abc") { // "abc"在字符串常量池当中。
        //synchronized (null) { // 报错：空指针。
        //synchronized (obj2) { // 这样编写就不安全了。因为obj2不是共享对象。
            double before = this.getBalance();
            double after = before - money;
            try {
                Thread.sleep(1000)
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            this.setBalance(after);
        //}
    }
}

--------------------------------------------------------------------
package com.yxj.java.threadsafe2;

public class AccountThread extends Thread {

    // 两个线程必须共享同一个账户对象。
    private Account act;

    // 通过构造方法传递过来账户对象
    public AccountThread(Account act) {
        this.act = act;
    }

    public void run(){
        // run方法的执行表示取款操作。
        // 假设取款5000
        double money = 5000;
        // 取款
        // 多线程并发执行这个方法。
        //synchronized (this) { //这里的this是AccountThread对象，这个对象不共享！
        synchronized (act) { // 这种方式也可以，只不过扩大了同步的范围，效率更低了。
            act.withdraw(money);
        }

        System.out.println(Thread.currentThread().getName() + "对"+act.getActno()+"取款"+money+"成功，余额" + act.getBalance());
    }
}


--------------------------------------------------------------------
package com.yxj.java.threadsafe2;

public class Test {
    public static void main(String[] args) {
        // 创建账户对象（只创建1个）
        Account act = new Account("act-001", 10000);
        // 创建两个线程
        Thread t1 = new AccountThread(act);
        Thread t2 = new AccountThread(act);

        // 设置name
        t1.setName("t1");
        t2.setName("t2");
        // 启动线程取款
        t1.start();
        t2.start();
    }
}

```
```java
package com.yxj.java.threadsafe3;

public class Account {
    // 账号
    private String actno;
    // 余额
    private double balance;

    public Account() {
    }

    public Account(String actno, double balance) {
        this.actno = actno;
        this.balance = balance;
    }

    public String getActno() {
        return actno;
    }

    public void setActno(String actno) {
        this.actno = actno;
    }

    public double getBalance() {
        return balance;
    }

    public void setBalance(double balance) {
        this.balance = balance;
    }

    //取款的方法
    /*
    在实例方法上可以使用synchronized吗？可以的。
        synchronized出现在实例方法上，一定锁的是this。
        没得挑。只能是this。不能是其他的对象了。
        所以这种方式不灵活。

        另外还有一个缺点：synchronized出现在实例方法上，
        表示整个方法体都需要同步，可能会无故扩大同步的
        范围，导致程序的执行效率降低。所以这种方式不常用。

        synchronized使用在实例方法上有什么优点？
            代码写的少了。节俭了。

        如果共享的对象就是this，并且需要同步的代码块是整个方法体，
        建议使用这种方式。
     */
    public synchronized void withdraw(double money){
        double before = this.getBalance(); // 10000
        double after = before - money;
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        this.setBalance(after);
    }
}

--------------------------------------------------------------------
package com.yxj.java.threadsafe3;

public class AccountThread extends Thread {

    // 两个线程必须共享同一个账户对象。
    private Account act;

    // 通过构造方法传递过来账户对象
    public AccountThread(Account act) {
        this.act = act;
    }

    public void run(){
        // run方法的执行表示取款操作。
        // 假设取款5000
        double money = 5000;
        // 取款
        // 多线程并发执行这个方法。
        act.withdraw(money);

        System.out.println(Thread.currentThread().getName() + "对"+act.getActno()+"取款"+money+"成功，余额" + act.getBalance());
    }
}


--------------------------------------------------------------------
package com.yxj.java.threadsafe3;

public class Test {
    public static void main(String[] args) {
        // 创建账户对象（只创建1个）
        Account act = new Account("act-001", 10000);
        // 创建两个线程
        Thread t1 = new AccountThread(act);
        Thread t2 = new AccountThread(act);
        // 设置name
        t1.setName("t1");
        t2.setName("t2");
        // 启动线程取款
        t1.start();
        t2.start();
    }
}

```

```java
面试题
package com.yxj.java.exam1;

// 面试题：doOther方法执行的时候需要等待doSome方法的结束吗？
    //不需要，因为doOther()方法没有synchronized
public class Exam01 {
    public static void main(String[] args) throws InterruptedException {
        MyClass mc = new MyClass();

        Thread t1 = new MyThread(mc);
        Thread t2 = new MyThread(mc);

        t1.setName("t1");
        t2.setName("t2");

        t1.start();
        Thread.sleep(1000); //这个睡眠的作用是：为了保证t1线程先执行。
        t2.start();
    }
}

class MyThread extends Thread {
    private MyClass mc;
    public MyThread(MyClass mc){
        this.mc = mc;
    }
    public void run(){
        if(Thread.currentThread().getName().equals("t1")){
            mc.doSome();
        }
        if(Thread.currentThread().getName().equals("t2")){
            mc.doOther();
        }
    }
}

class MyClass {
    public synchronized void doSome(){
        System.out.println("doSome begin");
        try {
            Thread.sleep(1000 * 10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("doSome over");
    }
    public void doOther(){
        System.out.println("doOther begin");
        System.out.println("doOther over");
    }
}
--------------------------------------------------------------------
package com.yxj.java.exam2;

// 面试题：doOther方法执行的时候需要等待doSome方法的结束吗？
    //需要
public class Exam01 {
    public static void main(String[] args) throws InterruptedException {
        MyClass mc = new MyClass();

        Thread t1 = new MyThread(mc);
        Thread t2 = new MyThread(mc);

        t1.setName("t1");
        t2.setName("t2");

        t1.start();
        Thread.sleep(1000); //这个睡眠的作用是：为了保证t1线程先执行。
        t2.start();
    }
}

class MyThread extends Thread {
    private MyClass mc;
    public MyThread(MyClass mc){
        this.mc = mc;
    }
    public void run(){
        if(Thread.currentThread().getName().equals("t1")){
            mc.doSome();
        }
        if(Thread.currentThread().getName().equals("t2")){
            mc.doOther();
        }
    }
}

class MyClass {
    public synchronized void doSome(){
        System.out.println("doSome begin");
        try {
            Thread.sleep(1000 * 10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("doSome over");
    }
    public synchronized void doOther(){
        System.out.println("doOther begin");
        System.out.println("doOther over");
    }
}


--------------------------------------------------------------------
package com.yxj.java.exam3;

// 面试题：doOther方法执行的时候需要等待doSome方法的结束吗？
    //不需要，因为MyClass对象是两个，两把锁。
public class Exam01 {
    public static void main(String[] args) throws InterruptedException {
        MyClass mc1 = new MyClass();
        MyClass mc2 = new MyClass();

        Thread t1 = new MyThread(mc1);
        Thread t2 = new MyThread(mc2);

        t1.setName("t1");
        t2.setName("t2");

        t1.start();
        Thread.sleep(1000); //这个睡眠的作用是：为了保证t1线程先执行。
        t2.start();
    }
}

class MyThread extends Thread {
    private MyClass mc;
    public MyThread(MyClass mc){
        this.mc = mc;
    }
    public void run(){
        if(Thread.currentThread().getName().equals("t1")){
            mc.doSome();
        }
        if(Thread.currentThread().getName().equals("t2")){
            mc.doOther();
        }
    }
}

class MyClass {
    public synchronized void doSome(){
        System.out.println("doSome begin");
        try {
            Thread.sleep(1000 * 10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("doSome over");
    }
    public synchronized void doOther(){
        System.out.println("doOther begin");
        System.out.println("doOther over");
    }
}

--------------------------------------------------------------------
package com.yxj.java.exam4;

// 面试题：doOther方法执行的时候需要等待doSome方法的结束吗？
    //需要，因为静态方法是类锁，不管创建了几个对象，类锁只有1把。
public class Exam01 {
    public static void main(String[] args) throws InterruptedException {
        MyClass mc1 = new MyClass();
        MyClass mc2 = new MyClass();

        Thread t1 = new MyThread(mc1);
        Thread t2 = new MyThread(mc2);

        t1.setName("t1");
        t2.setName("t2");

        t1.start();
        Thread.sleep(1000); //这个睡眠的作用是：为了保证t1线程先执行。
        t2.start();
    }
}

class MyThread extends Thread {
    private MyClass mc;
    public MyThread(MyClass mc){
        this.mc = mc;
    }
    public void run(){
        if(Thread.currentThread().getName().equals("t1")){
            mc.doSome();
        }
        if(Thread.currentThread().getName().equals("t2")){
            mc.doOther();
        }
    }
}

class MyClass {
    // synchronized出现在静态方法上是找类锁。
    public synchronized static void doSome(){
        System.out.println("doSome begin");
        try {
            Thread.sleep(1000 * 10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("doSome over");
    }
    public synchronized static void doOther(){
        System.out.println("doOther begin");
        System.out.println("doOther over");
    }
}

```
### 4.1.6 死锁
```java
package com.yxj.java.deadlock;
/*
死锁代码要会写。
一般面试官要求你会写。
只有会写的，才会在以后的开发中注意这个事儿。
因为死锁很难调试。
 */
public class DeadLock {
    public static void main(String[] args) {
        Object o1 = new Object();
        Object o2 = new Object();

        // t1和t2两个线程共享o1,o2
        Thread t1 = new MyThread1(o1,o2);
        Thread t2 = new MyThread2(o1,o2);

        t1.start();
        t2.start();
    }
}

class MyThread1 extends Thread{
    Object o1;
    Object o2;
    public MyThread1(Object o1,Object o2){
        this.o1 = o1;
        this.o2 = o2;
    }
    public void run(){
        synchronized (o1){
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            synchronized (o2){

            }
        }
    }
}

class MyThread2 extends Thread {
    Object o1;
    Object o2;
    public MyThread2(Object o1,Object o2){
        this.o1 = o1;
        this.o2 = o2;
    }
    public void run(){
        synchronized (o2){
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            synchronized (o1){

            }
        }
    }
}

```

```java
Timer 
package com.yxj.java.thread;

import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Timer;
import java.util.TimerTask;

/*
使用定时器指定定时任务。
 */
public class TimerTest {
    public static void main(String[] args) throws Exception {

        // 创建定时器对象
        Timer timer = new Timer();
        //Timer timer = new Timer(true); //守护线程的方式

        // 指定定时任务
        //timer.schedule(定时任务, 第一次执行时间, 间隔多久执行一次);
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        Date firstTime = sdf.parse("2020-03-14 09:34:30");
        //timer.schedule(new LogTimerTask() , firstTime, 1000 * 10);
        // 每年执行一次。
        //timer.schedule(new LogTimerTask() , firstTime, 1000 * 60 * 60 * 24 * 365);

        //匿名内部类方式
        timer.schedule(new TimerTask(){
            @Override
            public void run() {
                // code....
            }
        } , firstTime, 1000 * 10);

    }
}

// 编写一个定时任务类
// 假设这是一个记录日志的定时任务
class LogTimerTask extends TimerTask {

    @Override
    public void run() {
        // 编写你需要执行的任务就行了。
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String strTime = sdf.format(new Date());
        System.out.println(strTime + ":成功完成了一次数据备份！");
    }
}

```
### 4.1.7 实现线程的第三种方式
```java
package com.yxj.java.thread;

import java.util.concurrent.Callable;
import java.util.concurrent.FutureTask; // JUC包下的，属于java的并发包，老JDK中没有这个包。新特性。

/*
实现线程的第三种方式：
    实现Callable接口
    这种方式的优点：可以获取到线程的执行结果。
    这种方式的缺点：效率比较低，在获取t线程执行结果的时候，当前线程受阻塞，效率较低。
 */
public class ThreadTest15 {
    public static void main(String[] args) throws Exception {

        // 第一步：创建一个“未来任务类”对象。
        // 参数非常重要，需要给一个Callable接口实现类对象。
        FutureTask task = new FutureTask(new Callable() {
            @Override
            public Object call() throws Exception { // call()方法就相当于run方法。只不过这个有返回值
                // 线程执行一个任务，执行之后可能会有一个执行结果
                // 模拟执行
                System.out.println("call method begin");
                Thread.sleep(1000 * 10);
                System.out.println("call method end!");
                int a = 100;
                int b = 200;
                return a + b; //自动装箱(300结果变成Integer)
            }
        });

        // 创建线程对象
        Thread t = new Thread(task);

        // 启动线程
        t.start();

        // 这里是main方法，这是在主线程中。
        // 在主线程中，怎么获取t线程的返回结果？
        // get()方法的执行会导致“当前线程阻塞”
        Object obj = task.get();
        System.out.println("线程执行结果:" + obj);

        // main方法这里的程序要想执行必须等待get()方法的结束
        // 而get()方法可能需要很久。因为get()方法是为了拿另一个线程的执行结果
        // 另一个线程执行是需要时间的。
        System.out.println("hello world!");
    }
}

```
### 4.1.8 使用wait方法和notify方法实现“生产者和消费者模式
```java
package com.yxj.java.thread;

import java.util.ArrayList;
import java.util.List;

/*
1、使用wait方法和notify方法实现“生产者和消费者模式”

2、什么是“生产者和消费者模式”？
   生产线程负责生产，消费线程负责消费。
   生产线程和消费线程要达到均衡。
   这是一种特殊的业务需求，在这种特殊的情况下需要使用wait方法和notify方法。

3、wait和notify方法不是线程对象的方法，是普通java对象都有的方法。

4、wait方法和notify方法建立在线程同步的基础之上。因为多线程要同时操作一个仓库。有线程安全问题。

5、wait方法作用：o.wait()让正在o对象上活动的线程t进入等待状态，并且释放掉t线程之前占有的o对象的锁。

6、notify方法作用：o.notify()让正在o对象上等待的线程唤醒，只是通知，不会释放o对象上之前占有的锁。

7、模拟这样一个需求：
   仓库我们采用List集合。
   List集合中假设只能存储1个元素。
   1个元素就表示仓库满了。
   如果List集合中元素个数是0，就表示仓库空了。
   保证List集合中永远都是最多存储1个元素。

   必须做到这种效果：生产1个消费1个。
*/
public class ThreadTest16 {
    public static void main(String[] args) {
        // 创建1个仓库对象，共享的。
        List list = new ArrayList();
        // 创建两个线程对象
        // 生产者线程
        Thread t1 = new Thread(new Producer(list));
        // 消费者线程
        Thread t2 = new Thread(new Consumer(list));

        t1.setName("生产者线程");
        t2.setName("消费者线程");

        t1.start();
        t2.start();
    }
}

// 生产线程
class Producer implements Runnable {
    // 仓库
    private List list;

    public Producer(List list) {
        this.list = list;
    }
    @Override
    public void run() {
        // 一直生产（使用死循环来模拟一直生产）
        while(true){
            // 给仓库对象list加锁。
            synchronized (list){
                if(list.size() > 0){ // 大于0，说明仓库中已经有1个元素了。
                    try {
                        // 当前线程进入等待状态，并且释放Producer之前占有的list集合的锁。
                        list.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                // 程序能够执行到这里说明仓库是空的，可以生产
                Object obj = new Object();
                list.add(obj);
                System.out.println(Thread.currentThread().getName() + "--->" + obj);
                // 唤醒消费者进行消费
                list.notifyAll();
            }
        }
    }
}

// 消费线程
class Consumer implements Runnable {
    // 仓库
    private List list;

    public Consumer(List list) {
        this.list = list;
    }

    @Override
    public void run() {
        // 一直消费
        while (true) {
            synchronized (list) {
                if (list.size() == 0) {
                    try {
                        // 仓库已经空了。
                        // 消费者线程等待，释放掉list集合的锁
                        list.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                // 程序能够执行到此处说明仓库中有数据，进行消费。
                Object obj = list.remove(0);
                System.out.println(Thread.currentThread().getName() + "--->" + obj);
                // 唤醒生产者生产。
                list.notifyAll();
            }
        }
    }
}
```
## 5.1 反射
```markdown
反射机制相关的重要的类有哪些？
	java.lang.Class：代表整个字节码，代表一个类型，代表整个类。

	java.lang.reflect.Method：代表字节码中的方法字节码。代表类中的方法。

	java.lang.reflect.Constructor：代表字节码中的构造方法字节码。代表类中的构造方法

	java.lang.reflect.Field：代表字节码中的属性字节码。代表类中的成员变量（静态变量+实例变量）


1、反射机制

1.1、什么是反射机制？反射机制有什么用？
	反射机制：可以操作字节码文件
	作用：可以让程序更加灵活。

1.2、反射机制相关的类在哪个包下？
	java.lang.reflect.*;

1.3、反射机制相关的主要的类？
	java.lang.Class
	java.lang.reflect.Method;
	java.lang.reflect.Constructor;
	java.lang.reflect.Field;

1.4、在java中获取Class的三种方式？
	第一种：	 
		Class c = Class.forName("完整类名");
	第二种：
		Class c = 对象.getClass();
	第三种：
		Class c = int.class;
		Class c = String.class;

1.5、获取了Class之后，可以调用无参数构造方法来实例化对象

	//c代表的就是日期Date类型
	Class c = Class.forName("java.util.Date");

	//实例化一个Date日期类型的对象
	Object obj = c.newInstance();
 
	一定要注意：
		newInstance()底层调用的是该类型的无参数构造方法。
		如果没有这个无参数构造方法会出现"实例化"异常。

1.6、如果你只想让一个类的“静态代码块”执行的话，你可以怎么做？
	Class.forName("该类的类名");
	这样类就加载，类加载的时候，静态代码块执行！！！！
	在这里，对该方法的返回值不感兴趣，主要是为了使用“类加载”这个动作。

1.7、关于路径问题？

	String path = Thread.currentThread().getContextClassLoader()
					  .getResource("写相对路径，但是这个相对路径从src出发开始找").getPath();	

	String path = Thread.currentThread().getContextClassLoader()
					  .getResource("abc").getPath();	//必须保证src下有abc文件。

	String path = Thread.currentThread().getContextClassLoader()
					  .getResource("a/db").getPath();	//必须保证src下有a目录，a目录下有db文件。
	
	String path = Thread.currentThread().getContextClassLoader()
					  .getResource("com/yxj/test.properties").getPath();	
					  //必须保证src下有com目录，com目录下有yxj目录。
					  //yxj目录下有test.properties文件。

	这种方式是为了获取一个文件的绝对路径。（通用方式，不会受到环境移植的影响。）
	但是该文件要求放在类路径下，换句话说：也就是放到src下面。
	src下是类的根路径。

	直接以流的形式返回：
	InputStream in = Thread.currentThread().getContextClassLoader()
							.getResourceAsStream("com/yxj/test.properties");

1.8、IO + Properties，怎么快速绑定属性资源文件？

	//要求：第一这个文件必须在类路径下
	//第二这个文件必须是以.properties结尾。
	ResourceBundle bundle = ResourceBundle.getBundle("com/yxj/test");
	String value = bundle.getString(key);


2、今日反射机制的重点内容
2.1、通过反射机制访问对象的某个属性。
2.2、通过反射机制调用对象的某个方法。
2.3、通过反射机制调用某个构造方法实例化对象。
2.4、通过反射机制获取父类以及父类型接口。
```
```java
package com.yxj.java.reflect;

import java.util.Date;

/*
要操作一个类的字节码，需要首先获取到这个类的字节码，怎么获取java.lang.Class实例？
   三种方式
       第一种：Class c = Class.forName("完整类名带包名");
       第二种：Class c = 对象.getClass();
       第三种：Class c = 任何类型.class;

*/
public class ReflectTest01 {
   public static void main(String[] args) {
       /*
       Class.forName()
           1、静态方法
           2、方法的参数是一个字符串。
           3、字符串需要的是一个完整类名。
           4、完整类名必须带有包名。java.lang包也不能省略。
        */
       Class c1 = null;
       Class c2 = null;
       try {
           c1 = Class.forName("java.lang.String"); // c1代表String.class文件，或者说c1代表String类型。
           c2 = Class.forName("java.util.Date"); // c2代表Date类型
           Class c3 = Class.forName("java.lang.Integer"); // c3代表Integer类型
           Class c4 = Class.forName("java.lang.System"); // c4代表System类型
       } catch (ClassNotFoundException e) {
           e.printStackTrace();
       }

       // java中任何一个对象都有一个方法：getClass()
       String s = "abc";
       Class x = s.getClass(); // x代表String.class字节码文件，x代表String类型。
       System.out.println(c1 == x); // true（==判断的是对象的内存地址。）

       Date time = new Date();
       Class y = time.getClass();
       System.out.println(c2 == y); // true (c2和y两个变量中保存的内存地址都是一样的，都指向方法区中的字节码文件。)

       // 第三种方式，java语言中任何一种类型，包括基本数据类型，它都有.class属性。
       Class z = String.class; // z代表String类型
       Class k = Date.class; // k代表Date类型
       Class f = int.class; // f代表int类型
       Class e = double.class; // e代表double类型

       System.out.println(x == z); // true

   }
}



------------------------------------------------------------------------------------------------------------

package com.yxj.java.reflect;

import com.yxj.java.bean.User;

/*
获取到Class，能干什么？
   通过Class的newInstance()方法来实例化对象。
   注意：newInstance()方法内部实际上调用了无参数构造方法，必须保证无参构造存在才可以。
*/
public class ReflectTest02 {
   public static void main(String[] args) {

       // 这是不使用反射机制，创建对象
       User user = new User();
       System.out.println(user);

       // 下面这段代码是以反射机制的方式创建对象。
       try {
           // 通过反射机制，获取Class，通过Class来实例化对象
           Class c = Class.forName("com.yxj.java.bean.User"); // c代表User类型。

           // newInstance() 这个方法会调用User这个类的无参数构造方法，完成对象的创建。
           // 重点是：newInstance()调用的是无参构造，必须保证无参构造是存在的！
           Object obj = c.newInstance();

           System.out.println(obj); // com.yxj.java.bean.User@10f87f48
       } catch (ClassNotFoundException e) {
           e.printStackTrace();
       } catch (IllegalAccessException e) {
           e.printStackTrace();
       } catch (InstantiationException e) {
           e.printStackTrace();
       }
   }
}



------------------------------------------------------------------------------------------------------------

package com.yxj.java.reflect;

import com.yxj.java.bean.User;

import java.io.FileReader;
import java.util.Properties;

/*
验证反射机制的灵活性。
   java代码写一遍，再不改变java源代码的基础之上，可以做到不同对象的实例化。
   非常之灵活。（符合OCP开闭原则：对扩展开放，对修改关闭。）

后期你们要学习的是高级框架，而工作过程中，也都是使用高级框架，
包括： ssh ssm
   Spring SpringMVC MyBatis
   Spring Struts Hibernate
   ...
   这些高级框架底层实现原理：都采用了反射机制。所以反射机制还是重要的。
   学会了反射机制有利于你理解剖析框架底层的源代码。
*/
public class ReflectTest03 {
   public static void main(String[] args) throws Exception{

       // 这种方式代码就写死了。只能创建一个User类型的对象
       //User user = new User();

       // 以下代码是灵活的，代码不需要改动，可以修改配置文件，配置文件修改之后，可以创建出不同的实例对象。
       // 通过IO流读取classinfo.properties文件
       FileReader reader = new FileReader("chapter25/classinfo2.properties");
       // 创建属性类对象Map
       Properties pro = new Properties(); // key value都是String
       // 加载
       pro.load(reader);
       // 关闭流
       reader.close();

       // 通过key获取value
       String className = pro.getProperty("className");
       //System.out.println(className);

       // 通过反射机制实例化对象
       Class c = Class.forName(className);
       Object obj = c.newInstance();
       System.out.println(obj);
   }
}


package com.yxj.java.reflect;
/*
研究一下：Class.forName()发生了什么？
   记住，重点：
       如果你只是希望一个类的静态代码块执行，其它代码一律不执行，
       你可以使用：
           Class.forName("完整类名");
       这个方法的执行会导致类加载，类加载时，静态代码块执行。

提示：
   后面JDBC技术的时候我们还需要。
*/
public class ReflectTest04 {
   public static void main(String[] args) {
       try {
           // Class.forName()这个方法的执行会导致：类加载。
           Class.forName("com.yxj.java.reflect.MyClass");
       } catch (ClassNotFoundException e) {
           e.printStackTrace();
       }
   }
}

-------------------------------------------------------------------------------------------------------------
package com.yxj.java.reflect;
/*
研究一下：Class.forName()发生了什么？
   记住，重点：
       如果你只是希望一个类的静态代码块执行，其它代码一律不执行，
       你可以使用：
           Class.forName("完整类名");
       这个方法的执行会导致类加载，类加载时，静态代码块执行。

提示：
   后面JDBC技术的时候我们还需要。
*/
public class ReflectTest04 {
   public static void main(String[] args) {
       try {
           // Class.forName()这个方法的执行会导致：类加载。
           Class.forName("com.yxj.java.reflect.MyClass");
       } catch (ClassNotFoundException e) {
           e.printStackTrace();
       }
   }
}



package com.yxj.java.reflect;

public class MyClass {

   // 静态代码块在类加载时执行，并且只执行一次。
   static {
       System.out.println("MyClass类的静态代码块执行了！");
   }
}

-------------------------------------------------------------------------------------------------------------
package com.yxj.java.reflect;

import java.lang.reflect.Field;
import java.lang.reflect.Modifier;

/*
反射Student类当中所有的Field（了解一下）
*/
public class ReflectTest05 {
   public static void main(String[] args) throws Exception{

       // 获取整个类
       Class studentClass = Class.forName("com.yxj.java.bean.Student");

       //com.yxj.java.bean.Student
       String className = studentClass.getName();
       System.out.println("完整类名：" + className);

       String simpleName = studentClass.getSimpleName();
       System.out.println("简类名：" + simpleName);

       // 获取类中所有的public修饰的Field
       Field[] fields = studentClass.getFields();
       System.out.println(fields.length); // 测试数组中只有1个元素
       // 取出这个Field
       Field f = fields[0];
       // 取出这个Field它的名字
       String fieldName = f.getName();
       System.out.println(fieldName);

       // 获取所有的Field
       Field[] fs = studentClass.getDeclaredFields();
       System.out.println(fs.length); // 4

       System.out.println("==================================");
       // 遍历
       for(Field field : fs){
           // 获取属性的修饰符列表
           int i = field.getModifiers(); // 返回的修饰符是一个数字，每个数字是修饰符的代号！！！
           System.out.println(i);
           // 可以将这个“代号”数字转换成“字符串”吗？
           String modifierString = Modifier.toString(i);
           System.out.println(modifierString);
           // 获取属性的类型
           Class fieldType = field.getType();
           //String fName = fieldType.getName();
           String fName = fieldType.getSimpleName();
           System.out.println(fName);
           // 获取属性的名字
           System.out.println(field.getName());
       }
   }
}


-------------------------------------------------------------------------------------------------------------
package com.yxj.java.bean;

// 反射属性Field
public class Student {

   // Field翻译为字段，其实就是属性/成员
   // 4个Field，分别采用了不同的访问控制权限修饰符
   private String name; // Field对象
   protected int age; // Field对象
   boolean sex;
   public int no;
   public static final double MATH_PI = 3.1415926;
}

package com.yxj.java.reflect;

//通过反射机制，反编译一个类的属性Field（了解一下）

import java.lang.reflect.Field;
import java.lang.reflect.Modifier;

public class ReflectTest06 {
   public static void main(String[] args) throws Exception{

       // 创建这个是为了拼接字符串。
       StringBuilder s = new StringBuilder();

       //Class studentClass = Class.forName("com.yxj.java.bean.Student");
       Class studentClass = Class.forName("java.lang.Thread");

       s.append(Modifier.toString(studentClass.getModifiers()) + " class " + studentClass.getSimpleName() + " {\n");

       Field[] fields = studentClass.getDeclaredFields();
       for(Field field : fields){
           s.append("\t");
           s.append(Modifier.toString(field.getModifiers()));
           s.append(" ");
           s.append(field.getType().getSimpleName());
           s.append(" ");
           s.append(field.getName());
           s.append(";\n");
       }

       s.append("}");
       System.out.println(s);

   }
}

-------------------------------------------------------------------------------------------------------------
package com.yxj.java.reflect;

import com.yxj.java.bean.Student;

import java.lang.reflect.Field;

/*
重点
必须掌握：
   怎么通过反射机制访问一个java对象的属性？
       给属性赋值set
       获取属性的值get
*/
public class ReflectTest07 {
   public static void main(String[] args) throws Exception{

       // 我们不使用反射机制，怎么去访问一个对象的属性呢？
       Student s = new Student();

       // 给属性赋值
       s.no = 1111; //三要素：给s对象的no属性赋值1111
                   //要素1：对象s
                   //要素2：no属性
                   //要素3：1111

       // 读属性值
       // 两个要素：获取s对象的no属性的值。
       System.out.println(s.no);

       // 使用反射机制，怎么去访问一个对象的属性。（set get）
       Class studentClass = Class.forName("com.yxj.java.bean.Student");
       Object obj = studentClass.newInstance(); // obj就是Student对象。（底层调用无参数构造方法）

       // 获取no属性（根据属性的名称来获取Field）
       Field noFiled = studentClass.getDeclaredField("no");

       // 给obj对象(Student对象)的no属性赋值
       /*
       虽然使用了反射机制，但是三要素还是缺一不可：
           要素1：obj对象
           要素2：no属性
           要素3：2222值
       注意：反射机制让代码复杂了，但是为了一个“灵活”，这也是值得的。
        */
       noFiled.set(obj, 22222); // 给obj对象的no属性赋值2222

       // 读取属性的值
       // 两个要素：获取obj对象的no属性的值。
       System.out.println(noFiled.get(obj));

       // 可以访问私有的属性吗？
       Field nameField = studentClass.getDeclaredField("name");

       // 打破封装（反射机制的缺点：打破封装，可能会给不法分子留下机会！！！）
       // 这样设置完之后，在外部也是可以访问private的。
       nameField.setAccessible(true);

       // 给name属性赋值
       nameField.set(obj, "jackson");
       // 获取name属性的值
       System.out.println(nameField.get(obj));
   }
}

-------------------------------------------------------------------------------------------------------------
package com.yxj.java.reflect;

import java.lang.reflect.Method;
import java.lang.reflect.Modifier;

/*
作为了解内容（不需要掌握）：
   反射Method
*/
public class ReflectTest08 {
   public static void main(String[] args) throws Exception{

       // 获取类了
       Class userServiceClass = Class.forName("com.yxj.java.service.UserService");

       // 获取所有的Method（包括私有的！）
       Method[] methods = userServiceClass.getDeclaredMethods();
       //System.out.println(methods.length); // 2

       // 遍历Method
       for(Method method : methods){
           // 获取修饰符列表
           System.out.println(Modifier.toString(method.getModifiers()));
           // 获取方法的返回值类型
           System.out.println(method.getReturnType().getSimpleName());
           // 获取方法名
           System.out.println(method.getName());
           // 方法的修饰符列表（一个方法的参数可能会有多个。）
           Class[] parameterTypes = method.getParameterTypes();
           for(Class parameterType : parameterTypes){
               System.out.println(parameterType.getSimpleName());
           }
       }
   }
}

-------------------------------------------------------------------------------------------------------------
package com.yxj.java.reflect;

import java.lang.reflect.Method;
import java.lang.reflect.Modifier;

/*
了解一下，不需要掌握（反编译一个类的方法。）
*/
public class ReflectTest09 {
   public static void main(String[] args) throws Exception{
       StringBuilder s = new StringBuilder();
       //Class userServiceClass = Class.forName("com.yxj.java.service.UserService");
       Class userServiceClass = Class.forName("java.lang.String");
       s.append(Modifier.toString(userServiceClass.getModifiers()) + " class "+userServiceClass.getSimpleName()+" {\n");

       Method[] methods = userServiceClass.getDeclaredMethods();
       for(Method method : methods){
           //public boolean login(String name,String password){}
           s.append("\t");
           s.append(Modifier.toString(method.getModifiers()));
           s.append(" ");
           s.append(method.getReturnType().getSimpleName());
           s.append(" ");
           s.append(method.getName());
           s.append("(");
           // 参数列表
           Class[] parameterTypes = method.getParameterTypes();
           for(Class parameterType : parameterTypes){
               s.append(parameterType.getSimpleName());
               s.append(",");
           }
           // 删除指定下标位置上的字符
           s.deleteCharAt(s.length() - 1);
           s.append("){}\n");
       }

       s.append("}");
       System.out.println(s);
   }
}

-------------------------------------------------------------------------------------------------------------
package com.yxj.java.reflect;

import com.yxj.java.service.UserService;

import java.lang.reflect.Method;

/*
重点：必须掌握，通过反射机制怎么调用一个对象的方法？
   五颗星*****

   反射机制，让代码很具有通用性，可变化的内容都是写到配置文件当中，
   将来修改配置文件之后，创建的对象不一样了，调用的方法也不同了，
   但是java代码不需要做任何改动。这就是反射机制的魅力。
*/
public class ReflectTest10 {
   public static void main(String[] args) throws Exception{
       // 不使用反射机制，怎么调用方法
       // 创建对象
       UserService userService = new UserService();
       // 调用方法
       /*
       要素分析：
           要素1：对象userService
           要素2：login方法名
           要素3：实参列表
           要素4：返回值
        */
       boolean loginSuccess = userService.login("admin","123");
       //System.out.println(loginSuccess);
       System.out.println(loginSuccess ? "登录成功" : "登录失败");

       // 使用反射机制来调用一个对象的方法该怎么做？
       Class userServiceClass = Class.forName("com.yxj.java.service.UserService");
       // 创建对象
       Object obj = userServiceClass.newInstance();
       // 获取Method
       Method loginMethod = userServiceClass.getDeclaredMethod("login", String.class, String.class);
       //Method loginMethod = userServiceClass.getDeclaredMethod("login", int.class);
       // 调用方法
       // 调用方法有几个要素？ 也需要4要素。
       // 反射机制中最最最最最重要的一个方法，必须记住。
       /*
       四要素：
       loginMethod方法
       obj对象
       "admin","123" 实参
       retValue 返回值
        */
       Object retValue = loginMethod.invoke(obj, "admin","123123");
       System.out.println(retValue);
   }
}

-------------------------------------------------------------------------------------------------------------
package com.yxj.java.bean;

public class Vip {
   int no;
   String name;
   String birth;
   boolean sex;

   public Vip() {
   }

   public Vip(int no) {
       this.no = no;
   }

   public Vip(int no, String name) {
       this.no = no;
       this.name = name;
   }

   public Vip(int no, String name, String birth) {
       this.no = no;
       this.name = name;
       this.birth = birth;
   }

   public Vip(int no, String name, String birth, boolean sex) {
       this.no = no;
       this.name = name;
       this.birth = birth;
       this.sex = sex;
   }

   @Override
   public String toString() {
       return "Vip{" +
               "no=" + no +
               ", name='" + name + '\'' +
               ", birth='" + birth + '\'' +
               ", sex=" + sex +
               '}';
   }
}


package com.yxj.java.reflect;

import java.lang.reflect.Constructor;
import java.lang.reflect.Modifier;

/*
反编译一个类的Constructor构造方法。
*/
public class ReflectTest11 {
   public static void main(String[] args) throws Exception{
       StringBuilder s = new StringBuilder();
       Class vipClass = Class.forName("java.lang.String");
       s.append(Modifier.toString(vipClass.getModifiers()));
       s.append(" class ");
       s.append(vipClass.getSimpleName());
       s.append("{\n");

       // 拼接构造方法
       Constructor[] constructors = vipClass.getDeclaredConstructors();
       for(Constructor constructor : constructors){
           //public Vip(int no, String name, String birth, boolean sex) {
           s.append("\t");
           s.append(Modifier.toString(constructor.getModifiers()));
           s.append(" ");
           s.append(vipClass.getSimpleName());
           s.append("(");
           // 拼接参数
           Class[] parameterTypes = constructor.getParameterTypes();
           for(Class parameterType : parameterTypes){
               s.append(parameterType.getSimpleName());
               s.append(",");
           }
           // 删除最后下标位置上的字符
           if(parameterTypes.length > 0){
               s.deleteCharAt(s.length() - 1);
           }
           s.append("){}\n");
       }

       s.append("}");
       System.out.println(s);
   }
}

-------------------------------------------------------------------------------------------------------------
package com.yxj.java.reflect;

import com.yxj.java.bean.Vip;

import java.lang.reflect.Constructor;

/*
比上一个例子(ReflectTest11)重要一些！！！

通过反射机制调用构造方法实例化java对象。（这个不是重点）
*/
public class ReflectTest12 {
   public static void main(String[] args) throws Exception{
       // 不使用反射机制怎么创建对象
       Vip v1 = new Vip();
       Vip v2 = new Vip(110, "zhangsan", "2001-10-11", true);

       // 使用反射机制怎么创建对象呢？
       Class c = Class.forName("com.yxj.java.bean.Vip");
       // 调用无参数构造方法
       Object obj = c.newInstance();
       System.out.println(obj);

       // 调用有参数的构造方法怎么办？
       // 第一步：先获取到这个有参数的构造方法
       Constructor con = c.getDeclaredConstructor(int.class, String.class, String.class,boolean.class);
       // 第二步：调用构造方法new对象
       Object newObj = con.newInstance(110, "jackson", "1990-10-11", true);
       System.out.println(newObj);

       // 获取无参数构造方法
       Constructor con2 = c.getDeclaredConstructor();
       Object newObj2 = con2.newInstance();
       System.out.println(newObj2);
   }
}

-------------------------------------------------------------------------------------------------------------
package com.yxj.java.reflect;

/*
重点：给你一个类，怎么获取这个类的父类，已经实现了哪些接口？
*/
public class ReflectTest13 {
   public static void main(String[] args) throws Exception{

       // String举例
       Class stringClass = Class.forName("java.lang.String");

       // 获取String的父类
       Class superClass = stringClass.getSuperclass();
       System.out.println(superClass.getName());

       // 获取String类实现的所有接口（一个类可以实现多个接口。）
       Class[] interfaces = stringClass.getInterfaces();
       for(Class in : interfaces){
           System.out.println(in.getName());
       }
   }
}

-------------------------------------------------------------------------------------------------------------


```
## 6.1 路径问题
```java
package com.yxj.java.reflect;

import java.io.FileReader;

/*
研究一下文件路径的问题。
怎么获取一个文件的绝对路径。以下讲解的这种方式是通用的。但前提是：文件需要在类路径下。才能用这种方式。
*/
public class AboutPath {
   public static void main(String[] args) throws Exception{
       // 这种方式的路径缺点是：移植性差，在IDEA中默认的当前路径是project的根。
       // 这个代码假设离开了IDEA，换到了其它位置，可能当前路径就不是project的根了，这时这个路径就无效了。
       //FileReader reader = new FileReader("chapter25/classinfo2.properties");

       // 接下来说一种比较通用的一种路径。即使代码换位置了，这样编写仍然是通用的。
       // 注意：使用以下通用方式的前提是：这个文件必须在类路径下。
       // 什么类路径下？方式在src下的都是类路径下。【记住它】
       // src是类的根路径。
       /*
       解释：
           Thread.currentThread() 当前线程对象
           getContextClassLoader() 是线程对象的方法，可以获取到当前线程的类加载器对象。
           getResource() 【获取资源】这是类加载器对象的方法，当前线程的类加载器默认从类的根路径下加载资源。
        */
       String path = Thread.currentThread().getContextClassLoader()
               .getResource("classinfo2.properties").getPath(); // 这种方式获取文件绝对路径是通用的。

       // 采用以上的代码可以拿到一个文件的绝对路径。
       // /C:/Users/Administrator/IdeaProjects/javase/out/production/chapter25/classinfo2.properties
       System.out.println(path);

       // 获取db.properties文件的绝对路径（从类的根路径下作为起点开始）
       String path2 = Thread.currentThread().getContextClassLoader()
               .getResource("com/yxj/java/bean/db.properties").getPath();
       System.out.println(path2);

   }
}

-------------------------------------------------------------------------------------------------------------
package com.yxj.java.reflect;

import java.io.FileReader;
import java.io.InputStream;
import java.util.Properties;

public class IoPropertiesTest {
   public static void main(String[] args) throws Exception{

       // 获取一个文件的绝对路径了！！！！！
       /*String path = Thread.currentThread().getContextClassLoader()
               .getResource("classinfo2.properties").getPath();
       FileReader reader = new FileReader(path);*/

       // 直接以流的形式返回。
       InputStream reader = Thread.currentThread().getContextClassLoader()
               .getResourceAsStream("classinfo2.properties");

       Properties pro = new Properties();
       pro.load(reader);
       reader.close();
       // 通过key获取value
       String className = pro.getProperty("className");
       System.out.println(className);
   }
}


-------------------------------------------------------------------------------------------------------------

package com.yxj.java.reflect;

import java.util.ResourceBundle;

/*
java.util包下提供了一个资源绑定器，便于获取属性配置文件中的内容。
使用以下这种方式的时候，属性配置文件xxx.properties必须放到类路径下。
*/
public class ResourceBundleTest {
   public static void main(String[] args) {

       // 资源绑定器，只能绑定xxx.properties文件。并且这个文件必须在类路径下。文件扩展名也必须是properties
       // 并且在写路径的时候，路径后面的扩展名不能写。
       //ResourceBundle bundle = ResourceBundle.getBundle("classinfo2");

       ResourceBundle bundle = ResourceBundle.getBundle("com/yxj/java/bean/db");

       String className = bundle.getString("className");
       System.out.println(className);

   }
}

```
## 7.1 注解
```js
注解Annotation是一种引用数据类型。编译之后也是生
成xxx.class文件。

怎么自定义注解呢？语法格式？

	 [修饰符列表] @interface 注解类型名{

	 }

注解怎么使用，用在什么地方？

	第一：注解使用时的语法格式是：
		@注解类型名
	
	第二：注解可以出现在类上、属性上、方法上、变量上等....
	注解还可以出现在注解类型上。

JDK内置了哪些注解呢？

	java.lang包下的注释类型：

		掌握：
		Deprecated 用 @Deprecated 注释的程序元素，
		不鼓励程序员使用这样的元素，通常是因为它很危险或存在更好的选择。 

		掌握：
		Override 表示一个方法声明打算重写超类中的另一个方法声明。 

		不用掌握：
		SuppressWarnings 指示应该在注释元素（以及包含在该注释元素中的
		所有程序元素）中取消显示指定的编译器警告。 

元注解
	什么是元注解？
		用来标注“注解类型”的“注解”，称为元注解。

	常见的元注解有哪些？
		Target
		Retention
	
	关于Target注解：
		这是一个元注解，用来标注“注解类型”的“注解”
		这个Target注解用来标注“被标注的注解”可以出现在哪些位置上。

		@Target(ElementType.METHOD)：表示“被标注的注解”只能出现在方法上。
		@Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, MODULE, PARAMETER, TYPE})
			表示该注解可以出现在：
				构造方法上
				字段上
				局部变量上
				方法上
				....
				类上...
	
	关于Retention注解：
		这是一个元注解，用来标注“注解类型”的“注解”
		这个Retention注解用来标注“被标注的注解”最终保存在哪里。

		@Retention(RetentionPolicy.SOURCE)：表示该注解只被保留在java源文件中。注解只在源码阶段保留，在编译器完整编译之后，它将被丢弃忽视；比如@Override, @SuppressWarnings
		@Retention(RetentionPolicy.CLASS)：表示该注解被保存在class文件中。注解只被保留到编译进行的时候，它并不会被加载到 JVM 中；
		@Retention(RetentionPolicy.RUNTIME)：表示该注解被保存在class文件中，并且可以被反射机制所读取。 注解可以保留到程序运行的时候，它会被加载进入到 JVM 中，所以在程序运行时可以获取到它们；
	
Retention的源代码

		//元注解	
		public @interface Retention {
			//属性
			RetentionPolicy value();
		}
		
	RetentionPolicy的源代码：
		public enum RetentionPolicy {
			 SOURCE,
			 CLASS,
			 RUNTIME
		}

		//@Retention(value=RetentionPolicy.RUNTIME)
		@Retention(RetentionPolicy.RUNTIME)
		public @interface MyAnnotation{}



Target的源代码
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Target {

   ElementType[] value();
}
	

public enum ElementType {
   /** Class, interface (including annotation type), or enum declaration */
   TYPE,

   /** Field declaration (includes enum constants) */
   FIELD,

   /** Method declaration */
   METHOD,

   /** Formal parameter declaration */
   PARAMETER,

   /** Constructor declaration */
   CONSTRUCTOR,

   /** Local variable declaration */
   LOCAL_VARIABLE,

   /** Annotation type declaration */
   ANNOTATION_TYPE,

   /** Package declaration */
   PACKAGE,

   /**
    * Type parameter declaration
    *
    * @since 1.8
    */
   TYPE_PARAMETER,

   /**
    * Use of a type
    *
    * @since 1.8
    */
   TYPE_USE
}


ElementType.CONSTRUCTOR: 对构造方法进行注解；
ElementType.ANNOTATION_TYPE: 对注解进行注解；
ElementType.FIELD: 对属性、成员变量、成员对象（包括 enum 实例）进行注解；
ElementType.LOCAL_VARIABLE: 对局部变量进行注解；
ElementType.METHOD: 对方法进行注解；
ElementType.PACKAGE: 对包进行注解；
ElementType.PARAMETER: 对描述参数进行注解；
ElementType.TYPE: 对类、接口、枚举进行注解；


@Documented
@Documented 是一个简单的标记注解，表示是否将注解信
息添加在 Java 文档，即 Javadoc 中。


@Inherited
Inherited 是指继承，@Inherited 定义了一个注释
与子类的关系。如果一个超类带有 @Inherited 注
解，那么对于该超类，它的子类如果没有被任何注
解应用的话，那么这个子类就继承了超类的注解。
@Inherited
@Retention(RetentionPolicy.RUNTIME)
@interface Test {}


@Test
public class A {}


public class B extends A {}
注解 Test 被 @Inherited 修饰，之后类 A 被 Test 注解，类 B 继承 A,类 B 也拥有 Test 这个注解。

@Repeatable
@Repeatable 是 Java 8 中加入的，是指可重复的意思。通常使用 @Repeatable 的时候指注解的值可以同时取多个
@interface Persons {
   Person[] value();
}

@Repeatable(Persons.class)
@interface Person {
   String role default "";
}

@Person(role="artist")
@Person(role="coder")
@Person(role="PM")
public class SuperMan {
   ...
}

上面的代码通过 @Repeatable 定义了 Person，而 @Repeatable 后面括号的类相当于一个容器注解。容器注解就是用来存放其它注解的地方，它本身也是一个注解。

@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Repeatable {
   Class<? extends Annotation> value();
}
注解在开发中有什么用呢？

	需求：
		假设有这样一个注解，叫做：@Id
		这个注解只能出现在类上面，当这个类上有这个注解的时候，
		要求这个类中必须有一个int类型的id属性。如果没有这个属性
		就报异常。如果有这个属性则正常执行！


```
```java
package com.yxj.java.annotation;

/*
自定义注解：MyAnnotation
*/
public @interface MyAnnotation {
   // ??????
}

-------------------------------------------------------------------------------------------------------------

package com.yxj.java.annotation;

// 注解修饰注解。
@MyAnnotation
public @interface OtherAnnotation {
}

-------------------------------------------------------------------------------------------------------------
package com.yxj.java.annotation;

// 默认情况下，注解可以出现在任意位置。

@MyAnnotation
public class AnnotationTest01 {

   @MyAnnotation
   private int no;

   @MyAnnotation
   public AnnotationTest01(){}

   @MyAnnotation
   public static void m1(){
       @MyAnnotation
       int i = 100;
   }

   @MyAnnotation
   public void m2(@MyAnnotation
                  String name,
                  @MyAnnotation
                  int k){

   }
}

@MyAnnotation
interface MyInterface {

}

@MyAnnotation
enum Season {
   SPRING,SUMMER,AUTUMN,WINTER
}

-------------------------------------------------------------------------------------------------------------
package com.yxj.java.annotation;

/*
关于JDK lang包下的Override注解
源代码：
public @interface Override {
}

标识性注解，给编译器做参考的。
编译器看到方法上有这个注解的时候，编译器会自动检查该方法是否重写了父类的方法。
如果没有重写，报错。

这个注解只是在编译阶段起作用，和运行期无关！

*/

// @Override这个注解只能注解方法。
// @Override这个注解是给编译器参考的，和运行阶段没有关系。
// 凡是java中的方法带有这个注解的，编译器都会进行编译检查，如果这个方法不是重写父类的方法，编译器报错。

//@Override 
public class AnnotationTest02 {

   //@Override
   private int no;

   @Override
   public String toString() {
       return "toString";
   }

}

-------------------------------------------------------------------------------------------------------------

package com.yxj.java.annotation;

// 表示这个类已过时。
@Deprecated
public class AnnotationTest03 {

   @Deprecated
   private String s;

   public static void main(String[] args) {
       AnnotationTest03 at = new AnnotationTest03();
       at.doSome();
   }

   @Deprecated
   public void doSome(){
       System.out.println("do something!");
   }

   // Deprecated这个注解标注的元素已过时。
   // 这个注解主要是向其它程序员传达一个信息，告知已过时，有更好的解决方案存在。
   @Deprecated
   public static void doOther(){
       System.out.println("do other...");
   }
}

class T {
   public static void main(String[] args) {
       AnnotationTest03 at = new AnnotationTest03();
       at.doSome();

       AnnotationTest03.doOther();

       try {
           Class c = Class.forName("java.util.Date");
           Object obj = c.newInstance();
       } catch (Exception e) {
           e.printStackTrace();
       }
   }
}

-------------------------------------------------------------------------------------------------------------
package com.yxj.java.annotation2;

public @interface MyAnnotation {

   /**
    * 我们通常在注解当中可以定义属性，以下这个是MyAnnotation的name属性。
    * 看着像1个方法，但实际上我们称之为属性name。
    * @return
    */
   String name();

   /*
   颜色属性
    */
   String color();

   /*
   年龄属性
    */
   int age() default 25; //属性指定默认值

}


package com.yxj.java.annotation2;

public class MyAnnotationTest {

   // 报错的原因：如果一个注解当中有属性，那么必须给属性赋值。（除非该属性使用default指定了默认值。）
   /*@MyAnnotation
   public void doSome(){

   }*/

   //@MyAnnotation(属性名=属性值,属性名=属性值,属性名=属性值)
   //指定name属性的值就好了。
   @MyAnnotation(name = "zhangsan", color = "红色")
   public void doSome(){

   }

}

-------------------------------------------------------------------------------------------------------------
package com.yxj.java.annotation4;

public @interface MyAnnotation {
   /*
   注解当中的属性可以是哪一种类型？
       属性的类型可以是：
           byte short int long float double boolean char String Class 枚举类型
           以及以上每一种的数组形式。
    */
   int value1();

   String value2();

   int[] value3();

   String[] value4();

   Season value5();

   Season[] value6();

   Class parameterType();

   Class[] parameterTypes();
}

package com.yxj.java.annotation4;

public @interface OtherAnnotation {
   /*
   年龄属性
    */
   int age();

   /*
   邮箱地址属性，支持多个
    */
   String[] email();

   /**
    * 季节数组，Season是枚举类型
    * @return
    */
   Season[] seasonArray();
}

package com.yxj.java.annotation4;

public enum Season {
   SPRING,SUMMER,AUTUMN,WINTER
}


package com.yxj.java.annotation4;


public class OtherAnnotationTest {

   // 数组是大括号
   @OtherAnnotation(age = 25, email = {"zhangsan@123.com", "zhangsan@sohu.com"}, seasonArray = Season.WINTER)
   public void doSome(){

   }

   // 如果数组中只有1个元素：大括号可以省略。
   @OtherAnnotation(age = 25, email = "zhangsan@123.com", seasonArray = {Season.SPRING, Season.SUMMER})
   public void doOther(){

   }

}

-------------------------------------------------------------------------------------------------------------
package com.yxj.java.annotation5;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

//只允许该注解可以标注类、方法
@Target({ElementType.TYPE, ElementType.METHOD})
// 希望这个注解可以被反射
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {

   /*
   value属性。
    */
   String value() default "北京大兴区";
}


package com.yxj.java.annotation5;

@MyAnnotation("上海浦东区")
public class MyAnnotationTest {

   //@MyAnnotation
   int i;

   //@MyAnnotation
   public MyAnnotationTest(){

   }

   @MyAnnotation
   public void doSome(){

       //@MyAnnotation
       int i;
   }

}


package com.yxj.java.annotation5;

public class ReflectAnnotationTest {
   public static void main(String[] args) throws Exception{
       // 获取这个类
       Class c = Class.forName("com.yxj.java.annotation5.MyAnnotationTest");
       // 判断类上面是否有@MyAnnotation
       //System.out.println(c.isAnnotationPresent(MyAnnotation.class)); // true
       if(c.isAnnotationPresent(MyAnnotation.class)){
           // 获取该注解对象
           MyAnnotation myAnnotation = (MyAnnotation)c.getAnnotation(MyAnnotation.class);
           //System.out.println("类上面的注解对象" + myAnnotation); // @com.yxj.java.annotation5.MyAnnotation()
           // 获取注解对象的属性怎么办？和调接口没区别。
           String value = myAnnotation.value();
           System.out.println(value);
       }

       // 判断String类上面是否存在这个注解
       Class stringClass = Class.forName("java.lang.String");
       System.out.println(stringClass.isAnnotationPresent(MyAnnotation.class)); // false
   }
}

-------------------------------------------------------------------------------------------------------------
package com.yxj.java.annotation6;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface MyAnnotation {

   /*
   username属性
    */
   String username();

   /*
   password属性
    */
   String password();
}


package com.yxj.java.annotation6;

import java.lang.reflect.Method;

public class MyAnnotationTest {

   @MyAnnotation(username = "admin", password = "456456")
   public void doSome(){

   }

   public static void main(String[] args) throws Exception{
       // 获取MyAnnotationTest的doSome()方法上面的注解信息。
       Class c = Class.forName("com.yxj.java.annotation6.MyAnnotationTest");
       // 获取doSome()方法
       Method doSomeMethod = c.getDeclaredMethod("doSome");
       // 判断该方法上是否存在这个注解
       if(doSomeMethod.isAnnotationPresent(MyAnnotation.class)) {
           MyAnnotation myAnnotation = doSomeMethod.getAnnotation(MyAnnotation.class);
           System.out.println(myAnnotation.username());
           System.out.println(myAnnotation.password());
       }
   }

}

-------------------------------------------------------------------------------------------------------------
package com.yxj.java.annotation7;

@MustHasIdPropertyAnnotation
public class User {
   int id;
   String name;
   String password;
}

package com.yxj.java.annotation7;

/*
自定义异常
*/
public class HasNotIdPropertyException extends RuntimeException {
   public HasNotIdPropertyException(){

   }
   public HasNotIdPropertyException(String s){
       super(s);
   }
}


package com.yxj.java.annotation7;

import java.lang.reflect.Field;

public class Test {
   public static void main(String[] args) throws Exception{
       // 获取类
       Class userClass = Class.forName("com.yxj.java.annotation7.User");
       // 判断类上是否存在Id注解
       if(userClass.isAnnotationPresent(MustHasIdPropertyAnnotation.class)){
           // 当一个类上面有@MustHasIdPropertyAnnotation注解的时候，要求类中必须存在int类型的id属性
           // 如果没有int类型的id属性则报异常。
           // 获取类的属性
           Field[] fields = userClass.getDeclaredFields();
           boolean isOk = false; // 给一个默认的标记
           for(Field field : fields){
               if("id".equals(field.getName()) && "int".equals(field.getType().getSimpleName())){
                   // 表示这个类是合法的类。有@Id注解，则这个类中必须有int类型的id
                   isOk = true; // 表示合法
                   break;
               }
           }

           // 判断是否合法
           if(!isOk){
               throw new HasNotIdPropertyException("被@MustHasIdPropertyAnnotation注解标注的类中必须要有一个int类型的id属性！");
           }

       }
   }
}
```
### 7.1.1 自定义注解
```markdown
自定义注解类编写的规则：

注解类型定义为 @interface，所有的注解会自动继承 java.lang.Annotation 这一接口，而且不能再去继承其他的类或接口；
参数成员只能用 public 或 default 两个关键字修饰；
参数成员只能用基本类型：byte, short, char, int, long, float, double, boolean，以及 String, Enum, Class, Annotations 等数据类型，以及这些类型的数组；
要获取类方法和字段的注解信息，必须通过 Java 的反射技术；
注解也可以不定义成员变量，但这样的注解没有什么卵用；
自定义注解需要使用元注解进行编写；
```
```java
以水果与水果供应商为例：
水果名称注解 FruitName.java:
package com.yxj.FruitAnnotation;

import java.lang.annotation.Documented;
import java.lang.annotation.Retention;
import java.lang.annotation.Target;

import static java.lang.annotation.ElementType.FIELD;
import static java.lang.annotation.RetentionPolicy.RUNTIME;

/**
 * 水果名称注解
 */
@Target(FIELD)
@Retention(RUNTIME)
@Documented
public @interface FruitName {
    String value() default "";
}


/**
 * 水果名称注解
 */
@Target(FIELD)
@Retention(RUNTIME)
@Documented
public @interface FruitName {
    String value() default "";
}


水果颜色注解 FruitColor.java
package com.yxj.FruitAnnotation;

import java.lang.annotation.Documented;
import java.lang.annotation.Retention;
import java.lang.annotation.Target;

import static java.lang.annotation.ElementType.FIELD;
import static java.lang.annotation.RetentionPolicy.RUNTIME;

/**
 * 水果颜色注解
 */
@Target(FIELD)
@Retention(RUNTIME)
@Documented
public @interface FruitColor {
    /**
     * 颜色枚举
     */
    public enum Color{ BLUE,RED,GREEN};

    /**
     * 颜色属性
     */
    Color fruitColor() default Color.GREEN;

}


水果供应者注解 FruitProvider.java:
package com.yxj.FruitAnnotation;

import java.lang.annotation.Documented;
import java.lang.annotation.Retention;
import java.lang.annotation.Target;

import static java.lang.annotation.ElementType.FIELD;
import static java.lang.annotation.RetentionPolicy.RUNTIME;

/**
 * 水果供应者注解
 */
@Target(FIELD)
@Retention(RUNTIME)
@Documented
public @interface FruitProvider {
    /**
     * 供应商编号
     */
    public int id() default -1;

    /**
     * 供应商名称
     */
    public String name() default "";

    /**
     * 供应商地址
     */
    public String address() default "";
}


注解处理器 FruitInfoUtil.java:
package com.grq.FruitAnnotation;

import java.lang.reflect.Field;

/**
 * 注解处理器
 */
public class FruitInfoUtil {
    public static void getFruitInfo(Class<?> clazz){

        String strFruitName=" 水果名称：";
        String strFruitColor=" 水果颜色：";
        String strFruitProvicer="供应商信息：";

        Field[] fields = clazz.getDeclaredFields();

        for(Field field :fields){
            if(field.isAnnotationPresent(FruitName.class)){
                FruitName fruitName = (FruitName) field.getAnnotation(FruitName.class);
                strFruitName=strFruitName+fruitName.value();
                System.out.println(strFruitName);
            }
            else if(field.isAnnotationPresent(FruitColor.class)){
                FruitColor fruitColor= (FruitColor) field.getAnnotation(FruitColor.class);
                strFruitColor=strFruitColor+fruitColor.fruitColor().toString();
                System.out.println(strFruitColor);
            }
            else if(field.isAnnotationPresent(FruitProvider.class)){
                FruitProvider fruitProvider= (FruitProvider) field.getAnnotation(FruitProvider.class);
                strFruitProvicer=" 供应商编号："+fruitProvider.id()+" 供应商名称："+fruitProvider.name()+" 供应商地址："+fruitProvider.address();
                System.out.println(strFruitProvicer);
            }
        }
    }
}

苹果 Apple.java:
package com.grq.FruitAnnotation;

/**
 * 注解使用
 */
public class Apple {

    @FruitName("Apple")
    private String appleName;

    @FruitColor(fruitColor = FruitColor.Color.RED)
    private String appleColor;

    @FruitProvider(id=1,name="从前慢有限集团",address="从前慢有限集团大厦")
    private String appleProvider;

    public void setAppleColor(String appleColor) {
        this.appleColor = appleColor;
    }
    public String getAppleColor() {
        return appleColor;
    }

    public void setAppleName(String appleName) {
        this.appleName = appleName;
    }
    public String getAppleName() {
        return appleName;
    }

    public void setAppleProvider(String appleProvider) {
        this.appleProvider = appleProvider;
    }
    public String getAppleProvider() {
        return appleProvider;
    }

    public void displayName(){
        System.out.println("水果的名字是：苹果");
    }
}

测试输出水果信息 FruitTestAnnotation:
package com.yxj.FruitAnnotation;

public class TestFruitAnnotation {
    public static void main(String[] args) {
        FruitInfoUtil.getFruitInfo(Apple.class);
    }
}

运行后的测试结果为：
水果名称：Apple
水果颜色：RED
供应商编号：1 供应商名称：从前慢有限集团 供应商地址：从前慢有限集团大厦
```
