# Java设计模式篇章1
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210715020335759.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1.1 设计模式的目的
```markdown
编写软件过程中，程序员面临着来自 耦合性，内聚性以及可维
护性，可扩展性，重用性，灵活性 等多方面的挑战，设计模
式是为了让程序(软件)，具有更好
1) 代码重用性 (即：相同功能的代码，不用多次编写)
2) 可读性 (即：编程规范性, 便于其他程序员的阅读和理解)
3) 可扩展性 (即：当需要增加新的功能时，非常的方便，称为可维护)
4) 可靠性 (即：当我们增加新的功能后，对原来的功能没有影响)
5) 使程序呈现高内聚，低耦合的特性
```
## 1.2 设计模式七大原则
```markdown
设计模式原则，其实就是程序员在编程时，应当遵守的原则，
也是各种设计模式的基础(即：设计模式为什么这样设计的依据)
设计模式常用的七大原则有:
1) 单一职责原则
2) 接口隔离原则
3) 依赖倒转(倒置)原则
4) 里氏替换原则
5) 开闭原则
6) 迪米特法则
7) 合成复用原则
```
## 1.3 单一职责原则
### 1.3.1 基本介绍
```markdown
对类来说的，即一个类应该只负责一项职责。如类A负责两个不
同职责：职责1，职责2。
当职责1需求变更而改变A时，可能造成职责2执行错误，所以
需要将类A的粒度分解为
A1，A2
```
### 1.3.2 应用实例

```java
1) 以交通工具案例讲解
方案1
package com.yxj.principle.singleresponsibility;

public class SingleResponsibility1 {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Vehicle vehicle = new Vehicle();
		vehicle.run("摩托车");
		vehicle.run("汽车");
		vehicle.run("飞机");
	}

}

// 交通工具类
// 方式1
// 1. 在方式1 的run方法中，违反了单一职责原则
// 2. 解决的方案非常的简单，根据交通工具运行方法不同，分解成不同类即可
class Vehicle {
	public void run(String vehicle) {
		System.out.println(vehicle + " 在公路上运行....");
	}
}
```

```java
方案2 
SingleResponsibility2.java
package com.yxj.principle.singleresponsibility;

public class SingleResponsibility2 {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		RoadVehicle roadVehicle = new RoadVehicle();
		roadVehicle.run("摩托车");
		roadVehicle.run("汽车");
		
		AirVehicle airVehicle = new AirVehicle();
		
		airVehicle.run("飞机");
	}

}

//方案2的分析
//1. 遵守单一职责原则
//2. 但是这样做的改动很大，即将类分解，同时修改客户端
//3. 改进：直接修改Vehicle 类，改动的代码会比较少=>方案3

class RoadVehicle {
	public void run(String vehicle) {
		System.out.println(vehicle + "公路运行");
	}
}

class AirVehicle {
	public void run(String vehicle) {
		System.out.println(vehicle + "天空运行");
	}
}

class WaterVehicle {
	public void run(String vehicle) {
		System.out.println(vehicle + "水中运行");
	}
}
```

```java
方案3 
SingleResponsibility3.java
package com.yxj.principle.singleresponsibility;

public class SingleResponsibility3 {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Vehicle2 vehicle2  = new Vehicle2();
		vehicle2.run("汽车");
		vehicle2.runWater("轮船");
		vehicle2.runAir("飞机");
	}

}


//方式3的分析
//1. 这种修改方法没有对原来的类做大的修改，只是增加方法
//2. 这里虽然没有在类这个级别上遵守单一职责原则，但是在方法级别上，仍然是遵守单一职责
class Vehicle2 {
	public void run(String vehicle) {
		//处理
		
		System.out.println(vehicle + " 在公路上运行....");
		
	}
	
	public void runAir(String vehicle) {
		System.out.println(vehicle + " 在天空上运行....");
	}
	
	public void runWater(String vehicle) {
		System.out.println(vehicle + " 在水中行....");
	}
	
	//方法2.
	//..
	//..
	
	//...
}

```
### 1.3.3 单一职责原则注意事项和细节
```markdown
1) 降低类的复杂度，一个类只负责一项职责。
2) 提高类的可读性，可维护性
3) 降低变更引起的风险
4) 通常情况下，我们应当遵守单一职责原则，只有逻辑足够简
单，才可以在代码级别违反单一职责原则；只有类中方法数
量足够少，可以在方法级别保持单一职责原则
```
## 1.4 接口隔离原则(Interface Segregation Principle)
### 1.4.1 基本介绍
```markdown
1) 客户端不应该依赖它不需要的接
口，即一个类对另一个类的依赖
应该建立在最小的接口上
2) 先看一张图:
3) 类A通过接口Interface1依赖类B，类C通过
接口Interface1依赖类D，如果接口
Interface1对于类A和类C来说不是最小接口，
那么类B和类D必须去实现他们不需要的方
法。
4) 按隔离原则应当这样处理：
将接口Interface1拆分为独立的几个接口，
类A和类C分别与他们需要的接口建立依赖
关系。也就是采用接口隔离原则
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201111151245644.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 1.4.2 应用示例
```java
1) 类A通过接口Interface1依赖类B，类C通过接口Interface1依赖类D，请编写代码完成此应用实例。
没有使用接口隔离代码原则代码
package com.yxj.principle.segregation;

public class Segregation1 {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

	}

}

//接口
interface Interface1 {
	void operation1();
	void operation2();
	void operation3();
	void operation4();
	void operation5();
}

class B implements Interface1 {
	public void operation1() {
		System.out.println("B 实现了 operation1");
	}
	
	public void operation2() {
		System.out.println("B 实现了 operation2");
	}
	public void operation3() {
		System.out.println("B 实现了 operation3");
	}
	public void operation4() {
		System.out.println("B 实现了 operation4");
	}
	public void operation5() {
		System.out.println("B 实现了 operation5");
	}
}

class D implements Interface1 {
	public void operation1() {
		System.out.println("D 实现了 operation1");
	}
	
	public void operation2() {
		System.out.println("D 实现了 operation2");
	}
	public void operation3() {
		System.out.println("D 实现了 operation3");
	}
	public void operation4() {
		System.out.println("D 实现了 operation4");
	}
	public void operation5() {
		System.out.println("D 实现了 operation5");
	}
}

class A { //A 类通过接口Interface1 依赖(使用) B类，但是只会用到1,2,3方法
	public void depend1(Interface1 i) {
		i.operation1();
	}
	public void depend2(Interface1 i) {
		i.operation2();
	}
	public void depend3(Interface1 i) {
		i.operation3();
	}
}
  
class C { //C 类通过接口Interface1 依赖(使用) D类，但是只会用到1,4,5方法
	public void depend1(Interface1 i) {
		i.operation1();
	}
	public void depend4(Interface1 i) {
		i.operation4();
	}
	public void depend5(Interface1 i) {
		i.operation5();
	}
}
```
### 1.4.3 传统方法的问题和使用接口隔离原则改进
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201111153034446.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
1) 类A通过接口Interface1依赖类B，类C通过接口Interface1依
赖类D，如果接口
Interface1对于类A和类C来说不是最小接口，那么类B和类D必须
去实现他们不需要的方法
2) 将接口Interface1拆分为独立的几个接口，类A和类C分别与他
们需要的接口建立依赖关系。也就是采用接口隔离原则
3) 接口Interface1中出现的方法，根据实际情况拆分为三个接口
4) 代码实现
```
```java
package com.yxj.principle.segregation.improve;

public class Segregation1 {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		// 使用一把
		A a = new A();
		a.depend1(new B()); // A类通过接口去依赖B类
		a.depend2(new B());
		a.depend3(new B());

		C c = new C();

		c.depend1(new D()); // C类通过接口去依赖(使用)D类
		c.depend4(new D());
		c.depend5(new D());

	}

}

// 接口1
interface Interface1 {
	void operation1();

}

// 接口2
interface Interface2 {
	void operation2();

	void operation3();
}

// 接口3
interface Interface3 {
	void operation4();

	void operation5();
}

class B implements Interface1, Interface2 {
	public void operation1() {
		System.out.println("B 实现了 operation1");
	}

	public void operation2() {
		System.out.println("B 实现了 operation2");
	}

	public void operation3() {
		System.out.println("B 实现了 operation3");
	}

}

class D implements Interface1, Interface3 {
	public void operation1() {
		System.out.println("D 实现了 operation1");
	}

	public void operation4() {
		System.out.println("D 实现了 operation4");
	}

	public void operation5() {
		System.out.println("D 实现了 operation5");
	}
}

class A { // A 类通过接口Interface1,Interface2 依赖(使用) B类，但是只会用到1,2,3方法
	public void depend1(Interface1 i) {
		i.operation1();
	}

	public void depend2(Interface2 i) {
		i.operation2();
	}

	public void depend3(Interface2 i) {
		i.operation3();
	}
}

class C { // C 类通过接口Interface1,Interface3 依赖(使用) D类，但是只会用到1,4,5方法
	public void depend1(Interface1 i) {
		i.operation1();
	}

	public void depend4(Interface3 i) {
		i.operation4();
	}

	public void depend5(Interface3 i) {
		i.operation5();
	}
}
```
## 1.5 依赖倒转原则
### 1.5.1 基本介绍
```markdown
依赖倒转原则(Dependence Inversion Principle)是指：
1) 高层模块不应该依赖低层模块，二者都应该依赖其抽象
2) 抽象不应该依赖细节，细节应该依赖抽象
3) 依赖倒转(倒置)的中心思想是面向接口编程
4) 依赖倒转原则是基于这样的设计理念：相对于细节的多变性，
抽象的东西要稳定的
多。以抽象为基础搭建的架构比以细节为基础的架构要稳定
的多。在java中，抽象
指的是接口或抽象类，细节就是具体的实现类
5) 使用接口或抽象类的目的是制定好规范，而不涉及任何具体的
操作，把展现细节的
任务交给他们的实现类去完成
```

### 1.5.2 应用实例
```java
请编程完成Person 接收消息 的功能
1) 实现方案1 + 分析说明
package com.yxj.principle.inversion;

public class DependecyInversion {

	public static void main(String[] args) {
		Person person = new Person();
		person.receive(new Email());
	}

}


class Email {
	public String getInfo() {
		return "电子邮件信息: hello,world";
	}
}

//完成Person接收消息的功能
//方式1分析
//1. 简单，比较容易想到
//2. 如果我们获取的对象是 微信，短信等等，则新增类，同时Perons也要增加相应的接收方法
//3. 解决思路：引入一个抽象的接口IReceiver, 表示接收者, 这样Person类与接口IReceiver发生依赖
//   因为Email, WeiXin 等等属于接收的范围，他们各自实现IReceiver 接口就ok, 这样我们就符号依赖倒转原则
class Person {
	public void receive(Email email ) {
		System.out.println(email.getInfo());
	}
}

```

```java
2) 实现方案2 + 分析说明
DependecyInversion.java
package com.yxj.principle.inversion.improve;

public class DependecyInversion {

	public static void main(String[] args) {
		//客户端无需改变
		Person person = new Person();
		person.receive(new Email());
		
		person.receive(new WeiXin());
	}

}

//定义接口
interface IReceiver {
	public String getInfo();
}

class Email implements IReceiver {
	public String getInfo() {
		return "电子邮件信息: hello,world";
	}
}

//增加微信
class WeiXin implements IReceiver {
	public String getInfo() {
		return "微信信息: hello,ok";
	}
}

//方式2
class Person {
	//这里我们是对接口的依赖
	public void receive(IReceiver receiver ) {
		System.out.println(receiver.getInfo());
	}
}

```
### 1.5.3 依赖关系传递的三种方式

```markdown
1) 接口传递
2) 构造方法传递
3) setter方式传递
```

```java
package com.yxj.principle.inversion.improve;

public class DependencyPass {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		ChangHong changHong = new ChangHong();
//		OpenAndClose openAndClose = new OpenAndClose();
//		openAndClose.open(changHong);
		
		//通过构造器进行依赖传递
//		OpenAndClose openAndClose = new OpenAndClose(changHong);
//		openAndClose.open();
		//通过setter方法进行依赖传递
		OpenAndClose openAndClose = new OpenAndClose();
		openAndClose.setTv(changHong);
		openAndClose.open();

	}

}

// 方式1： 通过接口传递实现依赖
// 开关的接口
// interface IOpenAndClose {
// public void open(ITV tv); //抽象方法,接收接口
// }
//
// interface ITV { //ITV接口
// public void play();
// }
// 
// class ChangHong implements ITV {
//
//	@Override
//	public void play() {
//		// TODO Auto-generated method stub
//		System.out.println("长虹电视机，打开");
//	}
//	 
// }
//// 实现接口
// class OpenAndClose implements IOpenAndClose{
// public void open(ITV tv){
// tv.play();
// }
// }

// 方式2: 通过构造方法依赖传递
// interface IOpenAndClose {
// public void open(); //抽象方法
// }
// interface ITV { //ITV接口
// public void play();
// }
// class OpenAndClose implements IOpenAndClose{
// public ITV tv; //成员
// public OpenAndClose(ITV tv){ //构造器
// this.tv = tv;
// }
// public void open(){
// this.tv.play();
// }
// }


// 方式3 , 通过setter方法传递
interface IOpenAndClose {
	public void open(); // 抽象方法

	public void setTv(ITV tv);
}

interface ITV { // ITV接口
	public void play();
}

class OpenAndClose implements IOpenAndClose {
	private ITV tv;

	public void setTv(ITV tv) {
		this.tv = tv;
	}

	public void open() {
		this.tv.play();
	}
}

class ChangHong implements ITV {

	@Override
	public void play() {
		// TODO Auto-generated method stub
		System.out.println("长虹电视机，打开");
	}
	 
}
```
### 1.5.4 依赖倒转原则的注意事项和细节
```markdown
1) 低层模块尽量都要有抽象类或接口，或者两者都有，程序稳
定性更好.
2) 变量的声明类型尽量是抽象类或接口, 这样我们的变量引用和
实际对象间，就存在
一个缓冲层，利于程序扩展和优化
3) 继承时遵循里氏替换原则
```
## 1.6 里氏替换原则
```markdown
OO中的继承性的思考和说明
1) 继承包含这样一层含义：父类中凡是已经实现好的方法，实际
上是在设定规范和契约，虽然它不强制要求所有的子类必须遵循这些
契约，但是如果子类对这些已经实现的方法任意修改，就会对整个继承
体系造成破坏。
2) 继承在给程序设计带来便利的同时，也带来了弊端。比如使用
继承会给程序带来侵入性，程序的可移植性降低，增加对象间的耦合性，
如果一个类被其他的类所继承，则当这个类需要修改时，必须考虑到所有
的子类，并且父类修改后，所有涉及到子类的功能都有可能产生故障
3) 问题提出：在编程中，如何正确的使用继承? => 里氏替换原则
```
```markdown
基本介绍
1) 里氏替换原则(Liskov Substitution Principle)在1988
年，由麻省理工学院的以为姓里的女士提出的。
2) 如果对每个类型为T1的对象o1，都有类型为T2的对象o2，
使得以T1定义的所有程序
P在所有的对象o1都代换成o2时，程序P的行为没有发生变化，
那么类型T2是类型T1的子类型。换句话说，所有引用基类的
地方必须能透明地使用其子类的对象。
3) 在使用继承时，遵循里氏替换原则，在子类中尽量不要重
写父类的方法
4) 里氏替换原则告诉我们，继承实际上让两个类耦合性增强了，
在适当的情况下，可以通过聚合，组合，依赖 来解决问题。.
```

```markdown
一个程序引出的问题和思考
```

```java
Liskov.java
package com.yxj.principle.liskov;

public class Liskov {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		A a = new A();
		System.out.println("11-3=" + a.func1(11, 3));
		System.out.println("1-8=" + a.func1(1, 8));

		System.out.println("-----------------------");
		B b = new B();
		System.out.println("11-3=" + b.func1(11, 3));//这里本意是求出11-3
		System.out.println("1-8=" + b.func1(1, 8));// 1-8
		System.out.println("11+3+9=" + b.func2(11, 3));
		
		

	}

}

// A类
class A {
	// 返回两个数的差
	public int func1(int num1, int num2) {
		return num1 - num2;
	}
}

// B类继承了A
// 增加了一个新功能：完成两个数相加,然后和9求和
class B extends A {
	//这里，重写了A类的方法, 可能是无意识
	public int func1(int a, int b) {
		return a + b;
	}

	public int func2(int a, int b) {
		return func1(a, b) + 9;
	}
}

```

```markdown
解决方法
1) 我们发现原来运行正常的相减功能发生了错误。原因就是类B无
意中重写了父类的方法，造成原有功能出现错误。在实际编程
中，我们常常会通过重写父类的方法完成新的功能，这样写起
来虽然简单，但整个继承体系的复用性会比较差。特别是运
行多态比较频繁的时候
2) 通用的做法是：原来的父类和子类都继承一个更通俗的
基类，原有的继承关系去掉，
采用依赖，聚合，组合等关系代替.
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201114100224651.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


```java
Liskov.java
package com.yxj.principle.liskov.improve;

public class Liskov {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		A a = new A();
		System.out.println("11-3=" + a.func1(11, 3));
		System.out.println("1-8=" + a.func1(1, 8));

		System.out.println("-----------------------");
		B b = new B();
		//因为B类不再继承A类，因此调用者，不会再func1是求减法
		//调用完成的功能就会很明确
		System.out.println("11+3=" + b.func1(11, 3));//这里本意是求出11+3
		System.out.println("1+8=" + b.func1(1, 8));// 1+8
		System.out.println("11+3+9=" + b.func2(11, 3));
		
		
		//使用组合仍然可以使用到A类相关方法
		System.out.println("11-3=" + b.func3(11, 3));// 这里本意是求出11-3
		

	}

}

//创建一个更加基础的基类
class Base {
	//把更加基础的方法和成员写到Base类
}

// A类
class A extends Base {
	// 返回两个数的差
	public int func1(int num1, int num2) {
		return num1 - num2;
	}
}

// B类继承了A
// 增加了一个新功能：完成两个数相加,然后和9求和
class B extends Base {
	//如果B需要使用A类的方法,使用组合关系
	private A a = new A();
	
	//这里，重写了A类的方法, 可能是无意识
	public int func1(int a, int b) {
		return a + b;
	}

	public int func2(int a, int b) {
		return func1(a, b) + 9;
	}
	
	//我们仍然想使用A的方法
	public int func3(int a, int b) {
		return this.a.func1(a, b);
	}
}

```
## 1.7 开闭原则
```markdown
1) 开闭原则（Open Closed Principle）是编程中最基础、最
重要的设计原则
2) 一个软件实体如类，模块和函数应该对扩展开放(对提供方)，
对修改关闭(对使用
方)。用抽象构建框架，用实现扩展细节。
3) 当软件需要变化时，尽量通过扩展软件实体的行为来实现
变化，而不是通过修改已有的代码来实现变化。
4) 编程中遵循其它原则，以及使用设计模式的目的就是遵
循开闭原则。
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201114111301651.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


```markdown
方式1的优缺点
1) 优点是比较好理解，简单易操作。
2) 缺点是违反了设计模式的ocp原则，即对扩展开放(提供方)，
对修改关闭(使用方)。
即当我们给类增加新功能的时候，尽量不修改代码，或者尽可
能少修改代码.
3) 比如我们这时要新增加一个图形种类 三角形，我们需
要做如下修改，修改的地方
较多
4) 代码演示
```

```java
package com.yxj.principle.ocp;

public class Ocp {

	public static void main(String[] args) {
		//使用看看存在的问题
		GraphicEditor graphicEditor = new GraphicEditor();
		graphicEditor.drawShape(new Rectangle());
		graphicEditor.drawShape(new Circle());
		graphicEditor.drawShape(new Triangle());
	}

}

//这是一个用于绘图的类 [使用方]
class GraphicEditor {
	//接收Shape对象，然后根据type，来绘制不同的图形
	public void drawShape(Shape s) {
		if (s.m_type == 1)
			drawRectangle(s);
		else if (s.m_type == 2)
			drawCircle(s);
		else if (s.m_type == 3)
			drawTriangle(s);
	}

	//绘制矩形
	public void drawRectangle(Shape r) {
		System.out.println(" 绘制矩形 ");
	}

	//绘制圆形
	public void drawCircle(Shape r) {
		System.out.println(" 绘制圆形 ");
	}
	
	//绘制三角形
	public void drawTriangle(Shape r) {
		System.out.println(" 绘制三角形 ");
	}
}

//Shape类，基类
class Shape {
	int m_type;
}

class Rectangle extends Shape {
	Rectangle() {
		super.m_type = 1;
	}
}

class Circle extends Shape {
	Circle() {
		super.m_type = 2;
	}
}

//新增画三角形
class Triangle extends Shape {
	Triangle() {
		super.m_type = 3;
	}
}

```
```markdown
方式二
改进的思路分析
思路：把创建Shape类做成抽象类，并提供一个抽象的draw方法，
让子类去实现即可，
这样我们有新的图形种类时，只需要让新的图形类继承Shape，
并实现draw方法即可，
使用方的代码就不需要修 -> 满足了开闭原则
```

```java
package com.yxj.principle.ocp.improve;

public class Ocp {

	public static void main(String[] args) {
		//使用看看存在的问题
		GraphicEditor graphicEditor = new GraphicEditor();
		graphicEditor.drawShape(new Rectangle());
		graphicEditor.drawShape(new Circle());
		graphicEditor.drawShape(new Triangle());
		graphicEditor.drawShape(new OtherGraphic());
	}

}

//这是一个用于绘图的类 [使用方]
class GraphicEditor {
	//接收Shape对象，调用draw方法
	public void drawShape(Shape s) {
		s.draw();
	}

	
}

//Shape类，基类
abstract class Shape {
	int m_type;
	
	public abstract void draw();//抽象方法
}

class Rectangle extends Shape {
	Rectangle() {
		super.m_type = 1;
	}

	@Override
	public void draw() {
		// TODO Auto-generated method stub
		System.out.println(" 绘制矩形 ");
	}
}

class Circle extends Shape {
	Circle() {
		super.m_type = 2;
	}
	@Override
	public void draw() {
		// TODO Auto-generated method stub
		System.out.println(" 绘制圆形 ");
	}
}

//新增画三角形
class Triangle extends Shape {
	Triangle() {
		super.m_type = 3;
	}
	@Override
	public void draw() {
		// TODO Auto-generated method stub
		System.out.println(" 绘制三角形 ");
	}
}

//新增一个图形
class OtherGraphic extends Shape {
	OtherGraphic() {
		super.m_type = 4;
	}

	@Override
	public void draw() {
		// TODO Auto-generated method stub
		System.out.println(" 绘制其它图形 ");
	}
}

```
## 1.8 迪米特法则
### 1.8.1  基本介绍
```markdown
1) 一个对象应该对其他对象保持最少的了解
2) 类与类关系越密切，耦合度越大
3) 迪米特法则(Demeter Principle)又叫最少知道原则，即
一个类对自己依赖的类知道的
越少越好。也就是说，对于被依赖的类不管多么复杂，都尽
量将逻辑封装在类的内
部。对外除了提供的public 方法，不对外泄露任何信息
4) 迪米特法则还有个更简单的定义：只与直接的朋友通信
5) 直接的朋友：每个对象都会与其他对象有耦合关系，只
要两个对象之间有耦合关系，
我们就说这两个对象之间是朋友关系。耦合的方式很多，
依赖，关联，组合，聚合等。其中，我们称出现成员变
量，方法参数，方法返回值中的类为直接的朋友，而
出现在局部变量中的类不是直接的朋友。也就是说，
陌生的类最好不要以局部变量的形式出现在类的
内部。
```
### 1.8.2  应用实例
```markdown
1) 有一个学校，下属有各个学院和总部，现要求打印出学
校总部员工ID和学院员工的id
```
```java
package com.yxj.principle.demeter;

import java.util.ArrayList;
import java.util.List;

//客户端
public class Demeter1 {

	public static void main(String[] args) {
		//创建了一个 SchoolManager 对象
		SchoolManager schoolManager = new SchoolManager();
		//输出学院的员工id 和  学校总部的员工信息
		schoolManager.printAllEmployee(new CollegeManager());

	}

}


//学校总部员工类
class Employee {
	private String id;

	public void setId(String id) {
		this.id = id;
	}

	public String getId() {
		return id;
	}
}


//学院的员工类
class CollegeEmployee {
	private String id;

	public void setId(String id) {
		this.id = id;
	}

	public String getId() {
		return id;
	}
}


//管理学院员工的管理类
class CollegeManager {
	//返回学院的所有员工
	public List<CollegeEmployee> getAllEmployee() {
		List<CollegeEmployee> list = new ArrayList<CollegeEmployee>();
		for (int i = 0; i < 10; i++) { //这里我们增加了10个员工到 list
			CollegeEmployee emp = new CollegeEmployee();
			emp.setId("学院员工id= " + i);
			list.add(emp);
		}
		return list;
	}
}

//学校管理类

//分析 SchoolManager 类的直接朋友类有哪些 Employee、CollegeManager
//CollegeEmployee 不是 直接朋友 而是一个陌生类，这样违背了 迪米特法则 
class SchoolManager {
	//返回学校总部的员工
	public List<Employee> getAllEmployee() {
		List<Employee> list = new ArrayList<Employee>();
		
		for (int i = 0; i < 5; i++) { //这里我们增加了5个员工到 list
			Employee emp = new Employee();
			emp.setId("学校总部员工id= " + i);
			list.add(emp);
		}
		return list;
	}

	//该方法完成输出学校总部和学院员工信息(id)
	void printAllEmployee(CollegeManager sub) {
		
		//分析问题
		//1. 这里的 CollegeEmployee 不是  SchoolManager的直接朋友
		//2. CollegeEmployee 是以局部变量方式出现在 SchoolManager
		//3. 违反了 迪米特法则 
		
		//获取到学院员工
		List<CollegeEmployee> list1 = sub.getAllEmployee();
		System.out.println("------------学院员工------------");
		for (CollegeEmployee e : list1) {
			System.out.println(e.getId());
		}
		//获取到学校总部员工
		List<Employee> list2 = this.getAllEmployee();
		System.out.println("------------学校总部员工------------");
		for (Employee e : list2) {
			System.out.println(e.getId());
		}
	}
}

```
### 1.8.3  应用实例改进
```markdown
1) 前面设计的问题在于SchoolManager中，CollegeEmployee类
并不是SchoolManager类的直接朋友 (分析)
2) 按照迪米特法则，应该避免类中出现这样非直接朋友关系的耦合
3) 对代码按照迪米特法则 进行改进.
```
```java
package com.yxj.principle.demeter.improve;

import java.util.ArrayList;
import java.util.List;

//客户端
public class Demeter1 {

	public static void main(String[] args) {
		System.out.println("~~~使用迪米特法则的改进~~~");
		//创建了一个 SchoolManager 对象
		SchoolManager schoolManager = new SchoolManager();
		//输出学院的员工id 和  学校总部的员工信息
		schoolManager.printAllEmployee(new CollegeManager());

	}

}


//学校总部员工类
class Employee {
	private String id;

	public void setId(String id) {
		this.id = id;
	}

	public String getId() {
		return id;
	}
}


//学院的员工类
class CollegeEmployee {
	private String id;

	public void setId(String id) {
		this.id = id;
	}

	public String getId() {
		return id;
	}
}


//管理学院员工的管理类
class CollegeManager {
	//返回学院的所有员工
	public List<CollegeEmployee> getAllEmployee() {
		List<CollegeEmployee> list = new ArrayList<CollegeEmployee>();
		for (int i = 0; i < 10; i++) { //这里我们增加了10个员工到 list
			CollegeEmployee emp = new CollegeEmployee();
			emp.setId("学院员工id= " + i);
			list.add(emp);
		}
		return list;
	}
	
	//输出学院员工的信息
	public void printEmployee() {
		//获取到学院员工
		List<CollegeEmployee> list1 = getAllEmployee();
		System.out.println("------------学院员工------------");
		for (CollegeEmployee e : list1) {
			System.out.println(e.getId());
		}
	}
}

//学校管理类

//分析 SchoolManager 类的直接朋友类有哪些 Employee、CollegeManager
//CollegeEmployee 不是 直接朋友 而是一个陌生类，这样违背了 迪米特法则 
class SchoolManager {
	//返回学校总部的员工
	public List<Employee> getAllEmployee() {
		List<Employee> list = new ArrayList<Employee>();
		
		for (int i = 0; i < 5; i++) { //这里我们增加了5个员工到 list
			Employee emp = new Employee();
			emp.setId("学校总部员工id= " + i);
			list.add(emp);
		}
		return list;
	}

	//该方法完成输出学校总部和学院员工信息(id)
	void printAllEmployee(CollegeManager sub) {
		
		//分析问题
		//1. 将输出学院的员工方法，封装到CollegeManager
		sub.printEmployee();
	
		//获取到学校总部员工
		List<Employee> list2 = this.getAllEmployee();
		System.out.println("------------学校总部员工------------");
		for (Employee e : list2) {
			System.out.println(e.getId());
		}
	}
}

```
### 1.8.4 迪米特法则注意事项和细节
```markdown
1) 迪米特法则的核心是降低类之间的耦合
2) 但是注意：由于每个类都减少了不必要的依赖，
因此迪米特法则只是要求降低类间(对象间)耦合关
系， 并不是要求完全没有依赖关系
```
## 1.9 合成复用原则（Composite Reuse Principle）
### 1.9.1 基本介绍
```markdown
原则是尽量使用合成/聚合的方式，而不是使用继承
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201115145119384.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 1.9.2 设计原则核心思想
```markdown
1) 找出应用中可能需要变化之处，把它们独立出来，
不要和那些不需要变化的代码混在一起。
2) 针对接口编程，而不是针对实现编程。
3) 为了交互对象之间的松耦合设计而努力
```
## 2.1 UML基本介绍
```markdown
1) UML——Unified modeling language UML
(统一建模语言)，是一种用于软件系统
分析和设计的语言工具，它用于帮助软
件开发人员进行思考和记录思路的结果
2) UML本身是一套符号的规定，就像数学
符号和化学符号一样，这些符号用于描
述软件模型中的各个元素和他们之间的
关系，比如类、接口、实现、泛化、依
赖、组合、聚合等，如右图:
3) 使用UML来建模，常用的工具有 Rational Rose , 也可以使用一些
插件来建模
```
### 2.1.1 UML类图
```markdown
1) 用于描述系统中的类(对象)本身的组成和类(对象)之
间的各种静态关系。
2) 类之间的关系：依赖、泛化（继承）、实现、关联、聚合与组合
```
#### 2.1.1.1 依赖关系（Dependence）
```markdown
只要是在类中用到了对方，那么他们之间就存在依赖关系。
如果没有对方，连编绎都通过不了。
```
```java
package com.yxj.uml.dependence;

public class Department {

}

package com.yxj.uml.dependence;

public class IDCard {

}

package com.yxj.uml.dependence;

public class Person {

}


package com.yxj.uml.dependence;

public class PersonDao {

}

package com.yxj.uml.dependence;

public class PersonServiceBean {
	private PersonDao personDao;

	public void save(Person person) {
	}

	public IDCard getIDCard(Integer personid) {
		return null;
	}

	public void modify() {
		Department department = new Department();
	}

}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201115154916204.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
小结
1) 类中用到了对方
2) 如果是类的成员属性
3) 如果是方法的返回类型
4) 是方法接收的参数类型
5) 方法中使用到
```
#### 2.1.1.2 泛化关系(generalization）
```java
// 实现关系实际上就是A类实现B接口，他是依赖关系的特例
package com.yxj.uml.generalization;

public abstract class DaoSupport{
	public void save(Object entity){
	}
	public void delete(Object id){
	}
}

package com.yxj.uml.generalization;

public class PersonServiceBean extends DaoSupport {

}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201115155307101.png#pic_center)
#### 2.1.1.3 实现关系
```markdown
实现关系实际上就是A类实现B接口，他是依赖关系的特例
```
```java
package com.yxj.uml.implementation;

public interface PersonService {
	public void delete(Integer id);

}


package com.yxj.uml.implementation;

public class PersonServiceBean implements PersonService{

	@Override
	public void delete(Integer id) {
		// TODO Auto-generated method stub
		System.out.println("delete..");
	}

}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201115155625738.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
#### 2.1.1.4 关联关系（Association）
```markdown
关联关系实际上就是类与类之间的联系，他是依赖关系的特例
关联具有导航性：即双向关系或单向关系
关系具有多重性：如“1”（表示有且仅有一个），“0...”（表示0个或者多个），
“0，1”（表示0个或者一个），“n...m”(表示n到 m个都可以),“m...*”（表示至少m
个）。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020111515580295.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201115155814946.png#pic_center)
#### 2.1.1.5  聚合关系（Aggregation）
```markdown
聚合关系（Aggregation）表示的是整体和部分的关系，整
体与部分可以分开。聚合关系是关联关系的特例，所以他
具有关联的导航性与多重性。
如：一台电脑由键盘(keyboard)、显示器(monitor)，
鼠标等组成；组成电脑的各个
配件是可以从电脑上分离出来的，使用带空心菱形的实线来表示：
```
```java
package com.yxj.uml.aggregation;

public class Moniter {

}


package com.yxj.uml.aggregation;

public class Mouse {

}


package com.yxj.uml.aggregation;

public class Computer {
	private Mouse mouse; //鼠标可以和computer分离
	private Moniter moniter;//显示器可以和Computer分离
	public void setMouse(Mouse mouse) {
		this.mouse = mouse;
	}
	public void setMoniter(Moniter moniter) {
		this.moniter = moniter;
	}
	
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201115160038850.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
如果我们人Mouse,Monitor和Computer是不可分
离的，则升级为组合关系
```
```java
package com.yxj.uml.composition;

public class Moniter {

}

package com.yxj.uml.composition;

public class Mouse {

}

package com.yxj.uml.composition;

public class Computer {
	private Mouse mouse = new Mouse(); //鼠标可以和computer不能分离
	private Moniter moniter = new Moniter();//显示器可以和Computer不能分离
	public void setMouse(Mouse mouse) {
		this.mouse = mouse;
	}
	public void setMoniter(Moniter moniter) {
		this.moniter = moniter;
	}
	
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201115160948952.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

#### 2.1.1.6  组合关系（Composition）
```markdown
组合关系：也是整体与部分的关系，但是整体与部分不可以分开。
再看一个案例：在程序中我们定义实体：Person与IDCard、Head, 
那么 Head 和Person 就是 组合，IDCard 和 Person 就是聚合。
但是如果在程序中Person实体中定义了对IDCard进行级联删除，即
删除Person时，连同IDCard一起删除，那么IDCard 和 Person 
就是组合了.
```
```java
package com.yxj.uml.composition;

public class Head {

}

package com.yxj.uml.composition;

public class IDCard {

}


package com.yxj.uml.composition;

public class Person {
    private IDCard card; //聚合关系
    private Head head = new Head(); //组合关系

}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201115160915447.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 3.1 设计模式介绍
```markdown
1) 设计模式是程序员在面对同类软件工程设计问题所总结出来的
有用的经验，
模式不是代码，而是某类问题的通用解决方案，设计
模式（Design pattern）代表了最佳的实践。这些解决方
案是众多软件开发人员经过相当长的一段时间的试验和
错误总结出来的。
2) 设计模式的本质提高 软件的维护性，通用性和扩展性，
并降低软件的复杂
度。
3) <<设计模式>> 是经典的书，作者
是 Erich Gamma、Richard Helm、RalphJohnson 
和 John Vlissides Design（俗称 “四人组 GOF”）
4) 设计模式并不局限于某种语言，java，php，c++ 都有设计模式.
```
## 3.2 设计模式类型
```markdown
设计模式分为三种类型，共23种
1) 创建型模式：单例模式、抽象工厂模式、原型模式、建造者模式、工厂模式。
2) 结构型模式：适配器模式、桥接模式、装饰模式、组合模式、外观模式、享
元模式、代理模式。
3) 行为型模式：模版方法模式、命令模式、访问者模式、迭代器模式、观察者
模式、中介者模式、备忘录模式、解释器模式（Interpreter模式）、状态模
式、策略模式、职责链模式(责任链模式)。
```
### 3.2.1 单例设计模式
```markdown
所谓类的单例设计模式，就是采取一定的方法保证在整
个的软件系统中，对某个类
只能存在一个对象实例，并且该类只提供一个取得其对
象实例的方法(静态方法)。
比如Hibernate的SessionFactory，它充当数据存储源的
代理，并负责创建Session
对象。SessionFactory并不是轻量级的，一般情况下，一
个项目通常只需要一个
SessionFactory就够，这是就会使用到单例模式。
```
```markdown
单例模式有八种方式：
1) 饿汉式(静态常量)
2) 饿汉式（静态代码块）
3) 懒汉式(线程不安全)
4) 懒汉式(线程安全，同步方法)
5) 懒汉式(线程安全，同步代码块)
6) 双重检查
7) 静态内部类
8) 枚举
```
#### 3.2.1.1 饿汉式（静态常量） 
```markdown
步骤如下：
1) 构造器私有化 (防止 new )
2) 类的内部创建对象
3) 向外暴露一个静态的公共方法。getInstance
```
```java
package com.yxj.singleton.type1;

public class SingletonTest01 {

	public static void main(String[] args) {
		//测试
		Singleton instance = Singleton.getInstance();
		Singleton instance2 = Singleton.getInstance();
		System.out.println(instance == instance2); // true
		System.out.println("instance.hashCode=" + instance.hashCode());
		System.out.println("instance2.hashCode=" + instance2.hashCode());
	}

}

//饿汉式(静态变量)

class Singleton {
	
	//1. 构造器私有化, 外部不能new
	private Singleton() {
		
	}
	
	//2.本类内部创建对象实例
	private final static Singleton instance = new Singleton();
	
	//3. 提供一个公有的静态方法，返回实例对象
	public static Singleton getInstance() {
		return instance;
	}
	
}
```

```markdown
优缺点说明：
1) 优点：这种写法比较简单，就是在类装载的时候就完成实例化。
避免了线程同步问题。
2) 缺点：在类装载的时候就完成实例化，没有达到Lazy Loading的
效果。如果从始至终从未使用过这个实例，则会造成内存的浪费
3) 这种方式基于classloder机制避免了多线程的同步问
题，不过，instance在类装载时就实例化，在单例模
式中大多数都是调用getInstance方法， 但是导致类装载
的原因有很多种，因此不能确定有其他的方式（或者其
他的静态方法）导致类装载，这时候初始化instance就没有
达到lazy loading的效果
4) 结论：这种单例模式可用，可能造成内存浪费
```
#### 3.2.1.2 饿汉式（静态代码块）
```java
package com.yxj.singleton.type2;

public class SingletonTest02 {

	public static void main(String[] args) {
		//测试
		Singleton instance = Singleton.getInstance();
		Singleton instance2 = Singleton.getInstance();
		System.out.println(instance == instance2); // true
		System.out.println("instance.hashCode=" + instance.hashCode());
		System.out.println("instance2.hashCode=" + instance2.hashCode());
	}

}

//饿汉式(静态变量)

class Singleton {
	
	//1. 构造器私有化, 外部能new
	private Singleton() {
		
	}
	

	//2.本类内部创建对象实例
	private  static Singleton instance;
	
	static { // 在静态代码块中，创建单例对象
		instance = new Singleton();
	}
	
	//3. 提供一个公有的静态方法，返回实例对象
	public static Singleton getInstance() {
		return instance;
	}
	
}
```
```markdown
优缺点说明：
1) 这种方式和上面的方式其实类似，只不过将类实例化的过程
放在了静态代码块中，也是在类装载的时候，就执行静态代码块中的代码，
初始化类的实例。优缺点和上面是一样的。
2) 结论：这种单例模式可用，但是可能造成内存浪费
```
#### 3.2.1.3 懒汉式(线程不安全)
```java
package com.yxj.singleton.type3;


public class SingletonTest03 {

	public static void main(String[] args) {
		System.out.println("懒汉式1 ， 线程不安全~");
		Singleton instance = Singleton.getInstance();
		Singleton instance2 = Singleton.getInstance();
		System.out.println(instance == instance2); // true
		System.out.println("instance.hashCode=" + instance.hashCode());
		System.out.println("instance2.hashCode=" + instance2.hashCode());
	}

}

class Singleton {
	private static Singleton instance;
	
	private Singleton() {}
	
	//提供一个静态的公有方法，当使用到该方法时，才去创建 instance
	//即懒汉式
	public static Singleton getInstance() {
		if(instance == null) {
			instance = new Singleton();
		}
		return instance;
	}
}
```

```markdown
优缺点说明：
1) 起到了Lazy Loading的效果，但是只能在单线程下使用。
2) 如果在多线程下，一个线程进入了if (singleton == null)判断语
句块，还未来得及往下执行，另一个线程也通过了这个判断语
句，这时便会产生多个实例。所以在多线程环境下不可使用这种方式
3) 结论：在实际开发中，不要使用这种方式.
```
#### 3.2.1.4 懒汉式(线程安全，同步方法)
```java
package com.yxj.singleton.type4;


public class SingletonTest04 {

	public static void main(String[] args) {
		System.out.println("懒汉式2 ， 线程安全~");
		Singleton instance = Singleton.getInstance();
		Singleton instance2 = Singleton.getInstance();
		System.out.println(instance == instance2); // true
		System.out.println("instance.hashCode=" + instance.hashCode());
		System.out.println("instance2.hashCode=" + instance2.hashCode());
	}

}

// 懒汉式(线程安全，同步方法)
class Singleton {
	private static Singleton instance;
	
	private Singleton() {}
	
	//提供一个静态的公有方法，加入同步处理的代码，解决线程安全问题
	//即懒汉式
	public static synchronized Singleton getInstance() {
		if(instance == null) {
			instance = new Singleton();
		}
		return instance;
	}
}
```
```markdown
优缺点说明：
1) 解决了线程不安全问题
2) 效率太低了，每个线程在想获得类的实例时候，
执行getInstance()方法都要进行同步。而其实这个方法只执行一次实例
化代码就够了，后面的想获得该类实例，直接return就行了。方法
进行同步效率太低
3) 结论：在实际开发中，不推荐使用这种方式
```
#### 3.2.1.5 懒汉式(线程安全，同步代码块)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201115181925135.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
优缺点说明：
1) 这种方式，本意是想对第四种实现方式的改进，因为前
面同步方法效率太低，改为同步产生实例化的的代码块
2) 但是这种同步并不能起到线程同步的作用。跟第3种实现
方式遇到的情形一致，假如一个线程进入了if (singleton == null)
判断语句块，还未来得及往下执行，
另一个线程也通过了这个判断语句，这时便会产生多个实例
3) 结论：在实际开发中，不能使用这种方式
```
#### 3.2.1.6 双重检查
```java
package com.yxj.singleton.type6;


public class SingletonTest06 {

	public static void main(String[] args) {
		System.out.println("双重检查");
		Singleton instance = Singleton.getInstance();
		Singleton instance2 = Singleton.getInstance();
		System.out.println(instance == instance2); // true
		System.out.println("instance.hashCode=" + instance.hashCode());
		System.out.println("instance2.hashCode=" + instance2.hashCode());
		
	}

}

// 懒汉式(线程安全，同步方法)
class Singleton {
	private static volatile Singleton instance;
	
	private Singleton() {}
	
	//提供一个静态的公有方法，加入双重检查代码，解决线程安全问题, 同时解决懒加载问题
	//同时保证了效率, 推荐使用
	
	public static  Singleton getInstance() {
		if(instance == null) {
			synchronized (Singleton.class) {
				if(instance == null) {
					instance = new Singleton();
				}
			}
			
		}
		return instance;
	}
}
```
```markdown
优缺点说明：
1) Double-Check概念是多线程开发中常使用到的，如代码中所示，
我们进行了两次if (singleton == null)检查，这样就可以保证
程安全了。
2) 这样，实例化代码只用执行一次，后面再次访问时，判断
if (singleton == null)，
直接return实例化对象，也避免的反复进行方法同步.
3) 线程安全；延迟加载；效率较高
4) 结论：在实际开发中，推荐使用这种单例设计模式
```
#### 3.2.1.7 静态内部类
```java
package com.yxj.singleton.type7;


public class SingletonTest07 {

	public static void main(String[] args) {
		System.out.println("使用静态内部类完成单例模式");
		Singleton instance = Singleton.getInstance();
		Singleton instance2 = Singleton.getInstance();
		System.out.println(instance == instance2); // true
		System.out.println("instance.hashCode=" + instance.hashCode());
		System.out.println("instance2.hashCode=" + instance2.hashCode());
		
	}

}

// 静态内部类完成， 推荐使用
class Singleton {
	private static volatile Singleton instance;
	
	//构造器私有化
	private Singleton() {}
	
	//写一个静态内部类,该类中有一个静态属性 Singleton
	private static class SingletonInstance {
		private static final Singleton INSTANCE = new Singleton(); 
	}
	
	//提供一个静态的公有方法，直接返回SingletonInstance.INSTANCE
	
	public static synchronized Singleton getInstance() {
		
		return SingletonInstance.INSTANCE;
	}
}
```
```markdown
优缺点说明：
1) 这种方式采用了类装载的机制来保证初始化实例时只有一个线程。
2) 静态内部类方式在Singleton类被装载时并不会立即实例化，
而是在需要实例化
时，调用getInstance方法，才会装载SingletonInstance类，
从而完成Singleton的实例化。
3) 类的静态属性只会在第一次加载类的时候初始化，所以在这
里，JVM帮助我们保证了线程的安全性，在类进行初始化时，
别的线程是无法进入的。
4) 优点：避免了线程不安全，利用静态内部类特点实现延迟加载，
效率高
5) 结论：推荐使用.
```
#### 3.2.1.8 枚举
```java
package com.yxj.singleton.type8;

public class SingletonTest08 {
	public static void main(String[] args) {
		Singleton instance = Singleton.INSTANCE;
		Singleton instance2 = Singleton.INSTANCE;
		System.out.println(instance == instance2);
		
		System.out.println(instance.hashCode());
		System.out.println(instance2.hashCode());
		
		instance.sayOK();
	}
}

//使用枚举，可以实现单例, 推荐
enum Singleton {
	INSTANCE; //属性
	public void sayOK() {
		System.out.println("ok~");
	}
}
```
```markdown
优缺点说明：
1) 这借助JDK1.5中添加的枚举来实现单例模式。
不仅能避免多线程同步问题，而且还能防止反序列化重新创建新的对象。
2) 这种方式是Effective Java作者Josh Bloch 提倡的方式
3) 结论：推荐使用
```
#### 3.2.1.9 单例模式在JDK 应用的源码分析
```markdown
单例模式在JDK 应用的源码分析
1) 我们JDK中，java.lang.Runtime就是经典的单例
模式(饿汉式)
2) 代码分析+Debug源码+代码说明
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201115184918438.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
#### 3.2.1.10 单例模式注意事项和细节说明
```markdown
1) 单例模式保证了 系统内存中该类只存在一个对象，节省了系
统资源，对于一些需
要频繁创建销毁的对象，使用单例模式可以提高系统性能
2) 当想实例化一个单例类的时候，必须要记住使用相应的获取对象
的方法，而不是使
用new
3) 单例模式使用的场景：需要频繁的进行创建和销毁的对象、
创建对象时耗时过多或
耗费资源过多(即：重量级对象)，但又经常用到的对象、工具类
对象、频繁访问数
据库或文件的对象(比如数据源、session工厂等)
```
### 3.2.2 工厂模式
```markdown
看一个具体的需求
看一个披萨的项目：要便于披萨种类的扩展，要便于维护
1) 披萨的种类很多(比如 GreekPizz、CheesePizz 等)
2) 披萨的制作有 prepare，bake, cut, box
3) 完成披萨店订购功能。
```
#### 3.2.2.1 使用传统的方式来完成
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201115220022452.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```java
package com.yxj.factory.simplefactory.pizzastore.pizza;

//将Pizza 类做成抽象
public abstract class Pizza {
	protected String name; //名字

	//准备原材料, 不同的披萨不一样，因此，我们做成抽象方法
	public abstract void prepare();

	
	public void bake() {
		System.out.println(name + " baking;");
	}

	public void cut() {
		System.out.println(name + " cutting;");
	}

	//打包
	public void box() {
		System.out.println(name + " boxing;");
	}

	public void setName(String name) {
		this.name = name;
	}
}


package com.yxj.factory.simplefactory.pizzastore.pizza;

public class CheesePizza extends Pizza {

	@Override
	public void prepare() {
		// TODO Auto-generated method stub
		System.out.println(" 给制作奶酪披萨 准备原材料 ");
	}

}


package com.yxj.factory.simplefactory.pizzastore.pizza;

public class GreekPizza extends Pizza {

	@Override
	public void prepare() {
		// TODO Auto-generated method stub
		System.out.println(" 给希腊披萨 准备原材料 ");
	}

}

package com.yxj.factory.simplefactory.pizzastore.order;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;


import com.yxj.factory.simplefactory.pizzastore.pizza.Pizza;

public class OrderPizza {

	//构造器
	public OrderPizza() {
		Pizza pizza = null;
		String orderType; // 订购披萨的类型
		do {
			orderType = getType();
			if (orderType.equals("greek")) {
				pizza = new GreekPizza();
				pizza.setName(" 希腊披萨 ");
			} else if (orderType.equals("cheese")) {
				pizza = new CheesePizza();
				pizza.setName(" 奶酪披萨 ");
			} else if (orderType.equals("pepper")) {
				pizza = new PepperPizza();
				pizza.setName("胡椒披萨");
			} else {
				break;
			}
			//输出pizza 制作过程
			pizza.prepare();
			pizza.bake();
			pizza.cut();
			pizza.box();
			
		} while (true);
	}
	// 写一个方法，可以获取客户希望订购的披萨种类
	private String getType() {
		try {
			BufferedReader strin = new BufferedReader(new InputStreamReader(System.in));
			System.out.println("input pizza 种类:");
			String str = strin.readLine();
			return str;
		} catch (IOException e) {
			e.printStackTrace();
			return "";
		}
	}

}

package com.yxj.factory.simplefactory.pizzastore.pizza;

public class PepperPizza extends Pizza {

	@Override
	public void prepare() {
		// TODO Auto-generated method stub
		System.out.println(" 给胡椒披萨准备原材料 ");
	}

}


package com.yxj.factory.simplefactory.pizzastore.order;

//相当于一个客户端，发出订购
public class PizzaStore {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		new OrderPizza();
		
	}

}

```
```java
传统的方式的优缺点
1) 优点是比较好理解，简单易操作。
2) 缺点是违反了设计模式的ocp原则，即对扩展开放，
对修改关闭。即当我们给类增
加新功能的时候，尽量不修改代码，或者尽可能少修改代码.
3) 比如我们这时要新增加一个Pizza的种类(Pepper披萨)，
我们需要做如下修改.

//新增 写
public class CheesePizza extends Pizza {
@Override
public void prepare() {
// TODO Auto-generated method stub
setName("奶酪pizza");
System.out.println(name + " preparing;");
}}

//增加一段代码 OrderPizza.java //写
if (ordertype.equals("greek")) {
pizza = new GreekPizza();
} else if (ordertype.equals("pepper")) {
pizza = new PepperPizza();
} else if (ordertype.equals("cheese")) {
pizza = new CheesePizza();
} else {
break;
}}


4) 改进的思路分析
分析：修改代码可以接受，但是如果我们在其它的地方
也有创建Pizza的代码，就意味着，也需要修改，而创
建Pizza的代码，往往有多处。
思路：把创建Pizza对象封装到一个类中，这样我们有
新的Pizza种类时，只需要修改该类就可，其它有创
建到Pizza对象的代码就不需要修改了.-> 简单工厂模式
```
#### 3.2.2.2 简单工厂模式
```markdown
基本介绍
1) 简单工厂模式是属于创建型模式，是工厂模式的一种。
简单工厂模式是由一个工厂对象决定创建出哪一种产品类
的实例。简单工厂模式是工厂模式家族中最简单实用的模式
2) 简单工厂模式：定义了一个创建对象的类，由这个类
来封装实例化对象的行为(代码)
3) 在软件开发中，当我们会用到大量的创建某种、某类
或者某批对象时，就会使用到工厂模式
```
```markdown
使用简单工厂模式
1) 简单工厂模式的设计方案: 定义一个可以实例化Pizaa对
象的类，封装创建对象的代码。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201115220504295.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```java
package com.yxj.factory.simplefactory.pizzastore.order;

import com.yxj.factory.simplefactory.pizzastore.pizza.CheesePizza;
import com.yxj.factory.simplefactory.pizzastore.pizza.GreekPizza;
import com.yxj.factory.simplefactory.pizzastore.pizza.PepperPizza;
import com.yxj.factory.simplefactory.pizzastore.pizza.Pizza;

//简单工厂类
public class SimpleFactory {

	//更加orderType 返回对应的Pizza 对象
	public Pizza createPizza(String orderType) {

		Pizza pizza = null;

		System.out.println("使用简单工厂模式");
		if (orderType.equals("greek")) {
			pizza = new GreekPizza();
			pizza.setName(" 希腊披萨 ");
		} else if (orderType.equals("cheese")) {
			pizza = new CheesePizza();
			pizza.setName(" 奶酪披萨 ");
		} else if (orderType.equals("pepper")) {
			pizza = new PepperPizza();
			pizza.setName("胡椒披萨");
		}
		
		return pizza;
	}
	
	//简单工厂模式 也叫 静态工厂模式 
	
	public static Pizza createPizza2(String orderType) {

		Pizza pizza = null;

		System.out.println("使用简单工厂模式2");
		if (orderType.equals("greek")) {
			pizza = new GreekPizza();
			pizza.setName(" 希腊披萨 ");
		} else if (orderType.equals("cheese")) {
			pizza = new CheesePizza();
			pizza.setName(" 奶酪披萨 ");
		} else if (orderType.equals("pepper")) {
			pizza = new PepperPizza();
			pizza.setName("胡椒披萨");
		}
		
		return pizza;
	}

}


package com.yxj.factory.simplefactory.pizzastore.order;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;


import com.yxj.factory.simplefactory.pizzastore.pizza.Pizza;

public class OrderPizza {



	//定义一个简单工厂对象
	SimpleFactory simpleFactory;
	Pizza pizza = null;
	
	//构造器
	public OrderPizza(SimpleFactory simpleFactory) {
		setFactory(simpleFactory);
	}
	
	public void setFactory(SimpleFactory simpleFactory) {
		String orderType = ""; //用户输入的
		
		this.simpleFactory = simpleFactory; //设置简单工厂对象
		
		do {
			orderType = getType(); 
			pizza = this.simpleFactory.createPizza(orderType);
			
			//输出pizza
			if(pizza != null) { //订购成功
				pizza.prepare();
				pizza.bake();
				pizza.cut();
				pizza.box();
			} else {
				System.out.println(" 订购披萨失败 ");
				break;
			}
		}while(true);
	}
	
	// 写一个方法，可以获取客户希望订购的披萨种类
	private String getType() {
		try {
			BufferedReader strin = new BufferedReader(new InputStreamReader(System.in));
			System.out.println("input pizza 种类:");
			String str = strin.readLine();
			return str;
		} catch (IOException e) {
			e.printStackTrace();
			return "";
		}
	}

}


package com.yxj.factory.simplefactory.pizzastore.order;

//相当于一个客户端，发出订购
public class PizzaStore {

	public static void main(String[] args) {
	
		// 使用简单工厂模式
		new OrderPizza(new SimpleFactory());
		System.out.println("~~退出程序~~");
		

	}

}

```

```markdown
简单工厂模式 也叫 静态工厂模式 
```

```java
package com.yxj.factory.simplefactory.pizzastore.order;

import com.yxj.factory.simplefactory.pizzastore.pizza.CheesePizza;
import com.yxj.factory.simplefactory.pizzastore.pizza.GreekPizza;
import com.yxj.factory.simplefactory.pizzastore.pizza.PepperPizza;
import com.yxj.factory.simplefactory.pizzastore.pizza.Pizza;

//简单工厂类
public class SimpleFactory {

	
	
	//简单工厂模式 也叫 静态工厂模式 
	
	public static Pizza createPizza2(String orderType) {

		Pizza pizza = null;

		System.out.println("使用简单工厂模式2");
		if (orderType.equals("greek")) {
			pizza = new GreekPizza();
			pizza.setName(" 希腊披萨 ");
		} else if (orderType.equals("cheese")) {
			pizza = new CheesePizza();
			pizza.setName(" 奶酪披萨 ");
		} else if (orderType.equals("pepper")) {
			pizza = new PepperPizza();
			pizza.setName("胡椒披萨");
		}
		
		return pizza;
	}

}


package com.yxj.factory.simplefactory.pizzastore.order;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

import com.yxj.factory.simplefactory.pizzastore.pizza.Pizza;

public class OrderPizza2 {

	Pizza pizza = null;
	String orderType = "";
	// 构造器
	public OrderPizza2() {
		
		do {
			orderType = getType();
			pizza = SimpleFactory.createPizza2(orderType);

			// 输出pizza
			if (pizza != null) { // 订购成功
				pizza.prepare();
				pizza.bake();
				pizza.cut();
				pizza.box();
			} else {
				System.out.println(" 订购披萨失败 ");
				break;
			}
		} while (true);
	}

	// 写一个方法，可以获取客户希望订购的披萨种类
	private String getType() {
		try {
			BufferedReader strin = new BufferedReader(new InputStreamReader(System.in));
			System.out.println("input pizza 种类:");
			String str = strin.readLine();
			return str;
		} catch (IOException e) {
			e.printStackTrace();
			return "";
		}
	}
}


package com.yxj.factory.simplefactory.pizzastore.order;

//相当于一个客户端，发出订购
public class PizzaStore {

	public static void main(String[] args) {
	
		new OrderPizza2();
	}

}

```
#### 3.2.2.3 工厂方法模式
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020111610470340.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
思路1
使用简单工厂模式，创建不同的简单工厂类，比如
BJPizzaSimpleFactory、LDPizzaSimpleFactory 等等.从当前
这个案例来说，也是可以的，但是考虑到项目的规模，以及软
件的可维护性、可扩展性并不是特别好

思路2
使用工厂方法模式

工厂方法模式
工厂方法模式介绍
工厂方法模式设计方案：将披萨项目的实例化功能抽象成抽象方
法，在不同的口味点餐子类中具体实现。
工厂方法模式：定义了一个创建对象的抽象方法，由子
类决定要实例化的类。工厂方法模式将对象的实例化推迟到子类。
```
```java

package com.yxj.factory.factorymethod.pizzastore.pizza;

//将Pizza 类做成抽象
public abstract class Pizza {
	protected String name; //名字

	//准备原材料, 不同的披萨不一样，因此，我们做成抽象方法
	public abstract void prepare();

	
	public void bake() {
		System.out.println(name + " baking;");
	}

	public void cut() {
		System.out.println(name + " cutting;");
	}

	//打包
	public void box() {
		System.out.println(name + " boxing;");
	}

	public void setName(String name) {
		this.name = name;
	}
}

package com.yxj.factory.factorymethod.pizzastore.pizza;

public class BJCheesePizza extends Pizza {

	@Override
	public void prepare() {
		// TODO Auto-generated method stub
		setName("北京的奶酪pizza");
		System.out.println(" 北京的奶酪pizza 准备原材料");
	}

}

package com.yxj.factory.factorymethod.pizzastore.pizza;

public class LDCheesePizza extends Pizza{

	@Override
	public void prepare() {
		// TODO Auto-generated method stub
		setName("伦敦的奶酪pizza");
		System.out.println(" 伦敦的奶酪pizza 准备原材料");
	}
}

package com.yxj.factory.factorymethod.pizzastore.pizza;

public class BJPepperPizza extends Pizza {
	@Override
	public void prepare() {
		// TODO Auto-generated method stub
		setName("北京的胡椒pizza");
		System.out.println(" 北京的胡椒pizza 准备原材料");
	}
}

package com.yxj.factory.factorymethod.pizzastore.pizza;

public class LDPepperPizza extends Pizza{
	@Override
	public void prepare() {
		// TODO Auto-generated method stub
		setName("伦敦的胡椒pizza");
		System.out.println(" 伦敦的胡椒pizza 准备原材料");
	}
}


package com.yxj.factory.factorymethod.pizzastore.order;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

import com.yxj.factory.factorymethod.pizzastore.pizza.Pizza;




public abstract class OrderPizza {

	//定义一个抽象方法，createPizza , 让各个工厂子类自己实现
	abstract Pizza createPizza(String orderType);
	
	// 构造器
	public OrderPizza() {
		Pizza pizza = null;
		String orderType; // 订购披萨的类型
		do {
			orderType = getType();
			pizza = createPizza(orderType); //抽象方法，由工厂子类完成
			//输出pizza 制作过程
			pizza.prepare();
			pizza.bake();
			pizza.cut();
			pizza.box();
			
		} while (true);
	}

	

	// 写一个方法，可以获取客户希望订购的披萨种类
	private String getType() {
		try {
			BufferedReader strin = new BufferedReader(new InputStreamReader(System.in));
			System.out.println("input pizza 种类:");
			String str = strin.readLine();
			return str;
		} catch (IOException e) {
			e.printStackTrace();
			return "";
		}
	}

}


package com.yxj.factory.factorymethod.pizzastore.order;

import com.yxj.factory.factorymethod.pizzastore.pizza.BJCheesePizza;
import com.yxj.factory.factorymethod.pizzastore.pizza.BJPepperPizza;
import com.yxj.factory.factorymethod.pizzastore.pizza.Pizza;


public class BJOrderPizza extends OrderPizza {

	
	@Override
	Pizza createPizza(String orderType) {
	
		Pizza pizza = null;
		if(orderType.equals("cheese")) {
			pizza = new BJCheesePizza();
		} else if (orderType.equals("pepper")) {
			pizza = new BJPepperPizza();
		}
		// TODO Auto-generated method stub
		return pizza;
	}

}


package com.yxj.factory.factorymethod.pizzastore.order;

import com.yxj.factory.factorymethod.pizzastore.pizza.LDCheesePizza;
import com.yxj.factory.factorymethod.pizzastore.pizza.LDPepperPizza;
import com.yxj.factory.factorymethod.pizzastore.pizza.Pizza;


public class LDOrderPizza extends OrderPizza {

	
	@Override
	Pizza createPizza(String orderType) {
	
		Pizza pizza = null;
		if(orderType.equals("cheese")) {
			pizza = new LDCheesePizza();
		} else if (orderType.equals("pepper")) {
			pizza = new LDPepperPizza();
		}
		// TODO Auto-generated method stub
		return pizza;
	}

}


package com.yxj.factory.factorymethod.pizzastore.order;

public class PizzaStore {

	public static void main(String[] args) {
		String loc = "bj";
		if (loc.equals("bj")) {
			//创建北京口味的各种Pizza
			new BJOrderPizza();
		} else {
			//创建伦敦口味的各种Pizza
			new LDOrderPizza();
		}
		// TODO Auto-generated method stub
	}

}


```
#### 3.2.2.4 抽象工厂模式
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201116105350915.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
1) 抽象工厂模式：定义了一个interface用于创建相关或有
依赖关系的对象簇，而无需指明具体的类
2) 抽象工厂模式可以将简单工厂模式和工厂方法模式进行整合。
3) 从设计层面看，抽象工厂模式就是对简单工厂模式
的改进(或者称为进一步的抽象)。
4) 将工厂抽象成两层，AbsFactory(抽象工厂) 和具体实现的工
厂子类。程序员可以根据创建对象类型使用对应的工厂子类。
这样将单个的简单工厂类变成了工厂簇，更利于代码的维护和扩展。
```
```java
package com.yxj.factory.absfactory.pizzastore.pizza;

//将Pizza 类做成抽象
public abstract class Pizza {
	protected String name; //名字

	//准备原材料, 不同的披萨不一样，因此，我们做成抽象方法
	public abstract void prepare();

	
	public void bake() {
		System.out.println(name + " baking;");
	}

	public void cut() {
		System.out.println(name + " cutting;");
	}

	//打包
	public void box() {
		System.out.println(name + " boxing;");
	}

	public void setName(String name) {
		this.name = name;
	}
}

package com.yxj.factory.absfactory.pizzastore.pizza;

public class BJCheesePizza extends Pizza {

	@Override
	public void prepare() {
		// TODO Auto-generated method stub
		setName("北京的奶酪pizza");
		System.out.println(" 北京的奶酪pizza 准备原材料");
	}

}


package com.yxj.factory.absfactory.pizzastore.pizza;

public class LDCheesePizza extends Pizza{

	@Override
	public void prepare() {
		// TODO Auto-generated method stub
		setName("伦敦的奶酪pizza");
		System.out.println(" 伦敦的奶酪pizza 准备原材料");
	}
}


package com.yxj.factory.absfactory.pizzastore.pizza;

public class BJPepperPizza extends Pizza {
	@Override
	public void prepare() {
		// TODO Auto-generated method stub
		setName("北京的胡椒pizza");
		System.out.println(" 北京的胡椒pizza 准备原材料");
	}
}


package com.yxj.factory.absfactory.pizzastore.pizza;

public class LDPepperPizza extends Pizza{
	@Override
	public void prepare() {
		// TODO Auto-generated method stub
		setName("伦敦的胡椒pizza");
		System.out.println(" 伦敦的胡椒pizza 准备原材料");
	}
}


package com.yxj.factory.absfactory.pizzastore.order;

import com.yxj.factory.absfactory.pizzastore.pizza.Pizza;

//一个抽象工厂模式的抽象层(接口)
public interface AbsFactory {
	//让下面的工厂子类来 具体实现
	public Pizza createPizza(String orderType);
}


package com.yxj.factory.absfactory.pizzastore.order;

import com.yxj.factory.absfactory.pizzastore.pizza.BJCheesePizza;
import com.yxj.factory.absfactory.pizzastore.pizza.BJPepperPizza;
import com.yxj.factory.absfactory.pizzastore.pizza.Pizza;

//这是工厂子类
public class BJFactory implements AbsFactory {

	@Override
	public Pizza createPizza(String orderType) {
		System.out.println("~使用的是抽象工厂模式~");
		// TODO Auto-generated method stub
		Pizza pizza = null;
		if(orderType.equals("cheese")) {
			pizza = new BJCheesePizza();
		} else if (orderType.equals("pepper")){
			pizza = new BJPepperPizza();
		}
		return pizza;
	}

}


package com.yxj.factory.absfactory.pizzastore.order;


import com.yxj.factory.absfactory.pizzastore.pizza.LDCheesePizza;
import com.yxj.factory.absfactory.pizzastore.pizza.LDPepperPizza;
import com.yxj.factory.absfactory.pizzastore.pizza.Pizza;


public class LDFactory implements AbsFactory {

	@Override
	public Pizza createPizza(String orderType) {
		System.out.println("~使用的是抽象工厂模式~");
		Pizza pizza = null;
		if (orderType.equals("cheese")) {
			pizza = new LDCheesePizza();
		} else if (orderType.equals("pepper")) {
			pizza = new LDPepperPizza();
		}
		return pizza;
	}

}

package com.yxj.factory.absfactory.pizzastore.order;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

import com.yxj.factory.absfactory.pizzastore.pizza.Pizza;

public class OrderPizza {

	AbsFactory factory;

	// 构造器
	public OrderPizza(AbsFactory factory) {
		setFactory(factory);
	}

	private void setFactory(AbsFactory factory) {
		Pizza pizza = null;
		String orderType = ""; // 用户输入
		this.factory = factory;
		do {
			orderType = getType();
			// factory 可能是北京的工厂子类，也可能是伦敦的工厂子类
			pizza = factory.createPizza(orderType);
			if (pizza != null) { // 订购ok
				pizza.prepare();
				pizza.bake();
				pizza.cut();
				pizza.box();
			} else {
				System.out.println("订购失败");
				break;
			}
		} while (true);
	}

	// 写一个方法，可以获取客户希望订购的披萨种类
	private String getType() {
		try {
			BufferedReader strin = new BufferedReader(new InputStreamReader(System.in));
			System.out.println("input pizza 种类:");
			String str = strin.readLine();
			return str;
		} catch (IOException e) {
			e.printStackTrace();
			return "";
		}
	}
}


package com.yxj.factory.absfactory.pizzastore.order;

public class PizzaStore {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//new OrderPizza(new BJFactory());
		new OrderPizza(new LDFactory());
	}

}

```
#### 3.2.2.5 工厂模式在JDK-Calendar 应用的源码分析
```markdown
1) JDK 中的Calendar类中，就使用了简单工厂模式
2) 源码分析+Debug源码+说明
```
```java
package com.yxj.jdk;

import java.util.Calendar;

public class Factory {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		// getInstance 是 Calendar 静态方法
		Calendar cal = Calendar.getInstance();
	    // 注意月份下标从0开始，所以取月份要+1
	    System.out.println("年:" + cal.get(Calendar.YEAR));
	    System.out.println("月:" + (cal.get(Calendar.MONTH) + 1));       
	    System.out.println("日:" + cal.get(Calendar.DAY_OF_MONTH));
	    System.out.println("时:" + cal.get(Calendar.HOUR_OF_DAY));
	    System.out.println("分:" + cal.get(Calendar.MINUTE));
	    System.out.println("秒:" + cal.get(Calendar.SECOND));
	    
	    

	    
	}

}


Calendar.java
public static Calendar getInstance()
    {
        Locale aLocale = Locale.getDefault(Locale.Category.FORMAT);
        return createCalendar(defaultTimeZone(aLocale), aLocale);
    }



private static Calendar createCalendar(TimeZone zone,Locale aLocale) / 根据TimeZone zone,Locale aLocale创建对应的实例
                            
    {
        CalendarProvider provider =
            LocaleProviderAdapter.getAdapter(CalendarProvider.class, aLocale)
                                 .getCalendarProvider();
        if (provider != null) {
            try {
                return provider.getInstance(zone, aLocale);
            } catch (IllegalArgumentException iae) {
                // fall back to the default instantiation
            }
        }

        Calendar cal = null;

        if (aLocale.hasExtensions()) {
            String caltype = aLocale.getUnicodeLocaleType("ca");
            if (caltype != null) {
                switch (caltype) {
                case "buddhist":
                cal = new BuddhistCalendar(zone, aLocale);
                    break;
                case "japanese":
                    cal = new JapaneseImperialCalendar(zone, aLocale);
                    break;
                case "gregory":
                    cal = new GregorianCalendar(zone, aLocale);
                    break;
                }
            }
        }
        if (cal == null) {
            // If no known calendar type is explicitly specified,
            // perform the traditional way to create a Calendar:
            // create a BuddhistCalendar for th_TH locale,
            // a JapaneseImperialCalendar for ja_JP_JP locale, or
            // a GregorianCalendar for any other locales.
            // NOTE: The language, country and variant strings are interned.
            if (aLocale.getLanguage() == "th" && aLocale.getCountry() == "TH") {
                cal = new BuddhistCalendar(zone, aLocale);
            } else if (aLocale.getVariant() == "JP" && aLocale.getLanguage() == "ja"
                       && aLocale.getCountry() == "JP") {
                cal = new JapaneseImperialCalendar(zone, aLocale);
            } else {
                cal = new GregorianCalendar(zone, aLocale);
            }
        }
        return cal;
    }

```
#### 3.2.2.6工厂模式小结
```markdown
1) 工厂模式的意义
将实例化对象的代码提取出来，放到一个类中统一管理和维护，
达到和主项目的依赖关系的解耦。从而提高项目的扩展
和维护性。
2) 三种工厂模式 (简单工厂模式、工厂方法模式、抽象工厂模式)
3) 设计模式的依赖抽象原则
创建对象实例时，不要直接 new 类, 而是把这个new 类的动
作放在一个工厂的方法
中，并返回。有的书上说，变量不要直接持有具体类的引用。
不要让类继承具体类，而是继承抽象类或者是实现interface(接口)
不要覆盖基类中已经实现的方法。
```
### 3.2.3 克隆羊问题
```markdown
现在有一只羊tom，姓名为: tom, 年龄为：1，颜色为：白色，
请编写程序创建和tom
羊 属性完全相同的10只羊。
```
#### 3.2.3.1  传统方式解决克隆羊问题
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201116142658564.png#pic_center)

```markdown
传统的方式的优缺点
1) 优点是比较好理解，简单易操作。
2) 在创建新的对象时，总是需要重新获取原始对象的属性，
如果创建的对象比较复杂时，效率较低
3) 总是需要重新初始化对象，而不是动态地获得对象运行时的
状态, 不够灵活
4) 改进的思路分析
思路：Java中Object类是所有类的根类，Object类提供了一
个clone()方法，该方法可以将一个Java对象复制一份，但是需要实
现clone的Java类必须要实现一个接口Cloneable，
该接口表示该类能够复制且具有复制的能力 => 原型模式
```
```java
package com.yxj.prototype;

public class Sheep {
	private String name;
	private int age;
	private String color;
	public Sheep(String name, int age, String color) {
		super();
		this.name = name;
		this.age = age;
		this.color = color;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public String getColor() {
		return color;
	}
	public void setColor(String color) {
		this.color = color;
	}
	@Override
	public String toString() {
		return "Sheep [name=" + name + ", age=" + age + ", color=" + color + "]";
	}
	
	
}


package com.yxj.prototype;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//传统的方法
		Sheep sheep = new Sheep("tom", 1, "白色");
		
		Sheep sheep2 = new Sheep(sheep.getName(), sheep.getAge(), sheep.getColor());
		Sheep sheep3 = new Sheep(sheep.getName(), sheep.getAge(), sheep.getColor());
		Sheep sheep4 = new Sheep(sheep.getName(), sheep.getAge(), sheep.getColor());
		Sheep sheep5 = new Sheep(sheep.getName(), sheep.getAge(), sheep.getColor());
		//....
		
		System.out.println(sheep);
		System.out.println(sheep2);
		System.out.println(sheep3);
		System.out.println(sheep4);
		System.out.println(sheep5);
		//...
	}

}

```
#### 3.2.3.2  原型模式-基本介绍
```markdown
1) 原型模式(Prototype模式)是指：用原型实例指定创建对象的种
类，并且通过拷贝这些原型，创建新的对象
2) 原型模式是一种创建型设计模式，允许一个对象再创建另
外一个可定制的对象，无需知道如何创建的细节
3) 工作原理是:通过将一个原型对象传给那个要发动创建的对
象，这个要发动创建的对象通过请求原型对象拷贝它们自
己来实施创建，即 对象.clone()
4) 形象的理解：孙大圣拔出猴毛， 变出其它孙大圣
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201116144230122.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
使用原型模式改进传统方式，让程序具有更高的效率和扩展性。
```
```java
package com.yxj.prototype.improve;




public class Sheep implements Cloneable {
	private String name;
	private int age;
	private String color;
	private String address = "蒙古羊";
	public Sheep(String name, int age, String color) {
		super();
		this.name = name;
		this.age = age;
		this.color = color;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public String getColor() {
		return color;
	}
	public void setColor(String color) {
		this.color = color;
	}
	
	
	
	@Override
	public String toString() {
		return "Sheep [name=" + name + ", age=" + age + ", color=" + color + ", address=" + address + "]";
	}
	//克隆该实例，使用默认的clone方法来完成
	@Override
	protected Object clone()  {
		
		Sheep sheep = null;
		try {
			sheep = (Sheep)super.clone();
		} catch (Exception e) {
			// TODO: handle exception
			System.out.println(e.getMessage());
		}
		// TODO Auto-generated method stub
		return sheep;
	}
	
	
}


package com.yxj.prototype.improve;



public class Client {

	public static void main(String[] args) {
		System.out.println("原型模式完成对象的创建");
		// TODO Auto-generated method stub
		Sheep sheep = new Sheep("tom", 1, "白色");
		
		
		Sheep sheep2 = (Sheep)sheep.clone(); //克隆
		Sheep sheep3 = (Sheep)sheep.clone(); //克隆
		Sheep sheep4 = (Sheep)sheep.clone(); //克隆
		Sheep sheep5 = (Sheep)sheep.clone(); //克隆
		
		System.out.println("sheep2 =" + sheep2);
		System.out.println("sheep3 =" + sheep3);
		System.out.println("sheep4 =" + sheep4);
		System.out.println("sheep5 =" + sheep5);
	}

}

```

```markdown
1) Prototype : 原型类，声明一个克隆自己的接口
2) ConcretePrototype: 具体的原型类, 实现一个克隆自己的操作
3) Client: 让一个原型对象克隆自己，从而创建一个新
的对象(属性一样)
```
#### 3.2.3.3  原型模式在Spring框架中源码分析
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201116150134288.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
#### 3.2.3.4  浅拷贝
```markdown
1) 对于数据类型是基本数据类型的成员变量，浅拷贝会直接进行
值传递，也就是将该属性值复制一份给新的对象。
2) 对于数据类型是引用数据类型的成员变量，比如说成员变
量是某个数组、某个类的对象等，那么浅拷贝会进行引用传
递，也就是只是将该成员变量的引用值（内存地址）复制一
份给新的对象。因为实际上两个对象的该成员变量都指向
同一个实例。在这种情况下，在一个对象中修改该成员变
量会影响到另一个对象的该成员变量值
3) 前面我们克隆羊就是浅拷贝
4) 浅拷贝是使用默认的 clone()方法来实现
sheep = (Sheep) super.clone();
```
```java
package com.yxj.prototype.improve;




public class Sheep implements Cloneable {
	private String name;
	private int age;
	private String color;
	private String address = "蒙古羊";
	public Sheep friend; //是对象, 克隆是会如何处理
	public Sheep(String name, int age, String color) {
		super();
		this.name = name;
		this.age = age;
		this.color = color;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public String getColor() {
		return color;
	}
	public void setColor(String color) {
		this.color = color;
	}
	
	
	
	@Override
	public String toString() {
		return "Sheep [name=" + name + ", age=" + age + ", color=" + color + ", address=" + address + "]";
	}
	//克隆该实例，使用默认的clone方法来完成
	@Override
	protected Object clone()  {
		
		Sheep sheep = null;
		try {
			sheep = (Sheep)super.clone();
		} catch (Exception e) {
			// TODO: handle exception
			System.out.println(e.getMessage());
		}
		// TODO Auto-generated method stub
		return sheep;
	}
	
	
}


package com.yxj.prototype.improve;



public class Client {

	public static void main(String[] args) {
		System.out.println("原型模式完成对象的创建");
		// TODO Auto-generated method stub
		Sheep sheep = new Sheep("tom", 1, "白色");
		
		sheep.friend = new Sheep("jack", 2, "黑色");
		
		Sheep sheep2 = (Sheep)sheep.clone(); //克隆
		Sheep sheep3 = (Sheep)sheep.clone(); //克隆
		Sheep sheep4 = (Sheep)sheep.clone(); //克隆
		Sheep sheep5 = (Sheep)sheep.clone(); //克隆
		
		System.out.println("sheep2 =" + sheep2 + "sheep2.friend=" + sheep2.friend.hashCode());
		System.out.println("sheep3 =" + sheep3 + "sheep3.friend=" + sheep3.friend.hashCode());
		System.out.println("sheep4 =" + sheep4 + "sheep4.friend=" + sheep4.friend.hashCode());
		System.out.println("sheep5 =" + sheep5 + "sheep5.friend=" + sheep5.friend.hashCode());
	}

}

```
#### 3.2.3.5 深拷贝
```markdown
1) 复制对象的所有基本数据类型的成员变量值
2) 为所有引用数据类型的成员变量申请存储空间，并复制
每个引用数据类型成员变量所引用的对象，直到该对象可
达的所有对象。也就是说，对象进行深拷贝要对整个对象
进行拷贝
3) 深拷贝实现方式1：重写clone方法来实现深拷贝
4) 深拷贝实现方式2：通过对象序列化实现深拷贝(推荐)
```
```java
package com.yxj.prototype.deepclone;

import java.io.Serializable;

public class DeepCloneableTarget implements Serializable, Cloneable {
	
	/**
	 * 
	 */
	private static final long serialVersionUID = 1L;

	private String cloneName;

	private String cloneClass;

	//构造器
	public DeepCloneableTarget(String cloneName, String cloneClass) {
		this.cloneName = cloneName;
		this.cloneClass = cloneClass;
	}

	//因为该类的属性，都是String , 因此我们这里使用默认的clone完成即可
	@Override
	protected Object clone() throws CloneNotSupportedException {
		return super.clone();
	}
}


package com.yxj.prototype.deepclone;

import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;

public class DeepProtoType implements Serializable, Cloneable{
	
	public String name; //String 属性
	public DeepCloneableTarget deepCloneableTarget;// 引用类型
	public DeepProtoType() {
		super();
	}
	
	
	//深拷贝 - 方式 1 使用clone 方法
	@Override
	protected Object clone() throws CloneNotSupportedException {
		
		Object deep = null;
		//这里完成对基本数据类型(属性)和String的克隆
		deep = super.clone(); 
		//对引用类型的属性，进行单独处理
		DeepProtoType deepProtoType = (DeepProtoType)deep;
		deepProtoType.deepCloneableTarget  = (DeepCloneableTarget)deepCloneableTarget.clone();
		
		// TODO Auto-generated method stub
		return deepProtoType;
	}
	
	//深拷贝 - 方式2 通过对象的序列化实现 (推荐)
	
	public Object deepClone() {
		
		//创建流对象
		ByteArrayOutputStream bos = null;
		ObjectOutputStream oos = null;
		ByteArrayInputStream bis = null;
		ObjectInputStream ois = null;
		
		try {
			
			//序列化
			bos = new ByteArrayOutputStream();
			oos = new ObjectOutputStream(bos);
			oos.writeObject(this); //当前这个对象以对象流的方式输出
			
			//反序列化
			bis = new ByteArrayInputStream(bos.toByteArray());
			ois = new ObjectInputStream(bis);
			DeepProtoType copyObj = (DeepProtoType)ois.readObject();
			
			return copyObj;
			
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
			return null;
		} finally {
			//关闭流
			try {
				bos.close();
				oos.close();
				bis.close();
				ois.close();
			} catch (Exception e2) {
				// TODO: handle exception
				System.out.println(e2.getMessage());
			}
		}
		
	}
	
}


package com.yxj.prototype.deepclone;

public class Client {

	public static void main(String[] args) throws Exception {
		// TODO Auto-generated method stub
		DeepProtoType p = new DeepProtoType();
		p.name = "宋江";
		p.deepCloneableTarget = new DeepCloneableTarget("大牛", "小牛");
		
		//方式1 完成深拷贝
		
//		DeepProtoType p2 = (DeepProtoType) p.clone();
//		
//		System.out.println("p.name=" + p.name + "p.deepCloneableTarget=" + p.deepCloneableTarget.hashCode());
//		System.out.println("p2.name=" + p.name + "p2.deepCloneableTarget=" + p2.deepCloneableTarget.hashCode());
	
		//方式2 完成深拷贝
		DeepProtoType p2 = (DeepProtoType) p.deepClone();
		
		System.out.println("p.name=" + p.name + "p.deepCloneableTarget=" + p.deepCloneableTarget.hashCode());
		System.out.println("p2.name=" + p.name + "p2.deepCloneableTarget=" + p2.deepCloneableTarget.hashCode());
	
	}

}

```
#### 3.2.3.6 原型模式的注意事项和细节
```markdown
原型模式的注意事项和细节
1) 创建新的对象比较复杂时，可以利用原型模式简化对象的创建
过程，同时也能够提高效率
2) 不用重新初始化对象，而是动态地获得对象运行时的状态
3) 如果原始对象发生变化(增加或者减少属性)，其它克隆对象的
也会发生相应的变化，无需修改代码
4) 在实现深克隆的时候可能需要比较复杂的代码
5) 缺点：需要为每一个类配备一个克隆方法，这对全新的类来
说不是很难，但对已有的类进行改造时，需要修改其源代码，
违背了ocp原则
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复设计模式1
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


