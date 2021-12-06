# Java设计模式篇章2
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210715104102679.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1.1 建造者模式
```markdown
盖房项目需求
1) 需要建房子：这一过程为打桩、砌墙、封顶
2) 房子有各种各样的，比如普通房，高楼，别墅，各
种房子的过程虽然一样，但是要求不要相同的.
```
### 1.1.1 传统方式解决盖房需求
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201117000612241.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```java
package com.yxj.builder;

public abstract class AbstractHouse {
	
	//打地基
	public abstract void buildBasic();
	//砌墙
	public abstract void buildWalls();
	//封顶
	public abstract void roofed();
	
	public void build() {
		buildBasic();
		buildWalls();
		roofed();
	}
	
}


package com.yxj.builder;

public class CommonHouse extends AbstractHouse {

	@Override
	public void buildBasic() {
		// TODO Auto-generated method stub
		System.out.println(" 普通房子打地基 ");
	}

	@Override
	public void buildWalls() {
		// TODO Auto-generated method stub
		System.out.println(" 普通房子砌墙 ");
	}

	@Override
	public void roofed() {
		// TODO Auto-generated method stub
		System.out.println(" 普通房子封顶 ");
	}

}



package com.yxj.builder;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		CommonHouse commonHouse = new CommonHouse();
		commonHouse.build();
	}

}

```
```markdown
传统方式解决盖房需求问题分析
1) 优点是比较好理解，简单易操作。
2) 设计的程序结构，过于简单，没有设计缓存层对象，
程序的扩展和维护不好. 也就是说，这种设计方案，把
产品(即：房子) 和 创建产品的过程(即：建房子流程) 封
装在一起，耦合性增强了。
3) 解决方案：将产品和产品建造过程解耦 => 建造者模式.
```
### 1.1.2 建造者模式基本介绍
```markdown
基本介绍
1) 建造者模式（Builder Pattern） 又叫生成器模式，
是一种对象构建模式。它可以将复杂对象的建造过
程抽象出来（抽象类别），使这个抽象过程的不同实现方
法可以构造出不同表现（属性）的对象。
2) 建造者模式 是一步一步创建一个复杂的对象，它
允许用户只通过指定复杂对象的类型和内容就可以构
建它们，用户不需要知道内部的具体构建细节。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201117090607399.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 1.1.3 建造者模式的四个角色
```markdown
1) Product（产品角色）： 一个具体的产品对象。
2) Builder（抽象建造者）： 创建一个Product对象的各个
部件指定的 接口/抽象类。
3) ConcreteBuilder（具体建造者）： 实现接口，构建和
装配各个部件。
4) Director（指挥者）： 构建一个使用Builder接口的对象。
它主要是用于创建一个复杂的对象。它主要有两个作用，
一是：隔离了客户与对象的生产过程，二是：
负责控制产品对象的生产过程。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/202011170907371.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 1.1.4 建造者模式解决盖房需求
* 应用实例
```markdown
1) 需要建房子：这一过程为打桩、砌墙、封顶。不管是普通
房子也好，别墅也好都需要经历这些过程，下面我们使用建
造者模式(Builder Pattern)来完成
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201117090914460.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.builder.improve;

//产品->Product
public class House {
	private String baise;
	private String wall;
	private String roofed;
	public String getBaise() {
		return baise;
	}
	public void setBaise(String baise) {
		this.baise = baise;
	}
	public String getWall() {
		return wall;
	}
	public void setWall(String wall) {
		this.wall = wall;
	}
	public String getRoofed() {
		return roofed;
	}
	public void setRoofed(String roofed) {
		this.roofed = roofed;
	}
	
}


package com.yxj.builder.improve;


// 抽象的建造者
public abstract class HouseBuilder {

	protected House house = new House();
	
	//将建造的流程写好, 抽象的方法
	public abstract void buildBasic();
	public abstract void buildWalls();
	public abstract void roofed();
	
	//建造房子好， 将产品(房子) 返回
	public House buildHouse() {
		return house;
	}
	
}


package com.yxj.builder.improve;

public class CommonHouse extends HouseBuilder {

	@Override
	public void buildBasic() {
		// TODO Auto-generated method stub
		System.out.println(" 普通房子打地基5米 ");
	}

	@Override
	public void buildWalls() {
		// TODO Auto-generated method stub
		System.out.println(" 普通房子砌墙10cm ");
	}

	@Override
	public void roofed() {
		// TODO Auto-generated method stub
		System.out.println(" 普通房子屋顶 ");
	}

}


package com.yxj.builder.improve;

public class HighBuilding extends HouseBuilder {

	@Override
	public void buildBasic() {
		// TODO Auto-generated method stub
		System.out.println(" 高楼的打地基100米 ");
	}

	@Override
	public void buildWalls() {
		// TODO Auto-generated method stub
		System.out.println(" 高楼的砌墙20cm ");
	}

	@Override
	public void roofed() {
		// TODO Auto-generated method stub
		System.out.println(" 高楼的透明屋顶 ");
	}

}


package com.yxj.builder.improve;

//指挥者，这里去指定制作流程，返回产品
public class HouseDirector {
	
	HouseBuilder houseBuilder = null;

	//构造器传入 houseBuilder
	public HouseDirector(HouseBuilder houseBuilder) {
		this.houseBuilder = houseBuilder;
	}

	//通过setter 传入 houseBuilder
	public void setHouseBuilder(HouseBuilder houseBuilder) {
		this.houseBuilder = houseBuilder;
	}
	
	//如何处理建造房子的流程，交给指挥者
	public House constructHouse() {
		houseBuilder.buildBasic();
		houseBuilder.buildWalls();
		houseBuilder.roofed();
		return houseBuilder.buildHouse();
	}
	
	
}


package com.yxj.builder.improve;

public class Client {
	public static void main(String[] args) {
		
		//盖普通房子
		CommonHouse commonHouse = new CommonHouse();
		//准备创建房子的指挥者
		HouseDirector houseDirector = new HouseDirector(commonHouse);
		
		//完成盖房子，返回产品(普通房子)
		House house = houseDirector.constructHouse();
		
		//System.out.println("输出流程");
		
		System.out.println("--------------------------");
		//盖高楼
		HighBuilding highBuilding = new HighBuilding();
		//重置建造者
		houseDirector.setHouseBuilder(highBuilding);
		//完成盖房子，返回产品(高楼)
		houseDirector.constructHouse();
		
		
		
	}
}

```
### 1.1.5 建造者模式在JDK的应用和源码分析
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201117091602740.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
源码中建造者模式角色分析
Appendable 接口定义了多个append方法(抽象方法), 即
Appendable 为抽象建造者, 定义了抽象方法
AbstractStringBuilder 实现了 Appendable 接口方法，
这里的AbstractStringBuilder 已经是建造者，只是不能实例化
StringBuilder 即充当了指挥者角色，同时充当了具体的建造者，
建造方法的实现是由 AbstractStringBuilder 完成, 
而StringBuilder 继承了AbstractStringBuilder 
```
### 1.1.6 建造者模式的注意事项和细节
```markdown
1) 客户端(使用程序)不必知道产品内部组成的细节，将产品
本身与产品的创建过程解耦，使得相同的创建过程可以创建
不同的产品对象
2) 每一个具体建造者都相对独立，而与其他的具体建造者无
关，因此可以很方便地替换具体建造者或增加新的具体建
造者， 用户使用不同的具体建造者即可得到不同的产品对象
3) 可以更加精细地控制产品的创建过程 。将复杂产品的
创建步骤分解在不同的方法中，使得创建过程更加清晰，
也更方便使用程序来控制创建过程
4) 增加新的具体建造者无须修改原有类库的代码，
指挥者类针对抽象建造者类编程，系统扩展方便，符合 “开闭原则”
5) 建造者模式所创建的产品一般具有较多的共同点，
其组成部分相似，如果产品之间的差异性很大，则不适
合使用建造者模式，因此其使用范围受到一定的限制。
6) 如果产品的内部变化复杂，可能会导致需要定义很
多具体建造者类来实现这种变化，导致系统变得很庞大，
因此在这种情况下，要考虑是否选择建造者模式.
7) 抽象工厂模式VS建造者模式
抽象工厂模式实现对产品家族的创建，一个产品家族是这
样的一系列产品：具有不同分类维度的产品组合，采用抽
象工厂模式不需要关心构建过程，只关心什么产品
由什么工厂生产即可。而建造者模式则是要求按照指定
的蓝图建造产品，它的主要
目的是通过组装零配件而产生一个新产品
```
## 1.2 适配器模式
### 1.2.1 泰国旅游使用插座问题
```markdown
现实生活中的适配器例子
泰国插座用的是两孔的（欧标），可以买个多功能
转换插头 (适配器) ，这样就可以使用了。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020111715390140.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 1.2.2 基本介绍
```markdown
1) 适配器模式(Adapter Pattern)将某个类的接口转换成客户
端期望的另一个接口表
示，主的目的是兼容性，让原本因接口不匹配不能一起工作
的两个类可以协同
工作。其别名为包装器(Wrapper)
2) 适配器模式属于结构型模式
3) 主要分为三类：类适配器模式、对象适配器模式、接口适配器模式
```
```markdown
工作原理
1) 适配器模式：将一个类的接口转换成另一种接口.让原本接口
不兼容的类可以兼容
2) 从用户的角度看不到被适配者，是解耦的
3) 用户调用适配器转化出来的目标接口方法，适配器再调用
被适配者的相关接口方法
4) 用户收到反馈结果，感觉只是和目标接口交互，如图
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020111715410129.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 1.2.3 类适配器模式
```markdown
基本介绍：Adapter类，通过继承 src类，实现 dst 类接口，
完成src->dst的适配
```

```markdown
应用实例
以生活中充电器的例子来讲解适配器，充电器本身相当于Adapter，
220V交流电相当于src (即被适配者)，我们的目dst(即 目标)是5V直流电
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201117202826928.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.adapter.classadapter;

//被适配的类
public class Voltage220V {
	//输出220V的电压
	public int output220V() {
		int src = 220;
		System.out.println("电压=" + src + "伏");
		return src;
	}
}


package com.yxj.adapter.classadapter;


//适配接口
public interface IVoltage5V {
	public int output5V();
}


package com.yxj.adapter.classadapter;

//适配器类
public class VoltageAdapter extends Voltage220V implements IVoltage5V {

	@Override
	public int output5V() {
		// TODO Auto-generated method stub
		//获取到220V电压
		int srcV = output220V();
		int dstV = srcV / 44 ; //转成 5v
		return dstV;
	}

}


package com.yxj.adapter.classadapter;

public class Phone {

	//充电
	public void charging(IVoltage5V iVoltage5V) {
		if(iVoltage5V.output5V() == 5) {
			System.out.println("电压为5V, 可以充电~~");
		} else if (iVoltage5V.output5V() > 5) {
			System.out.println("电压大于5V, 不能充电~~");
		}
	}
}


package com.yxj.adapter.classadapter;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		System.out.println(" === 类适配器模式 ====");
		Phone phone = new Phone();
		phone.charging(new VoltageAdapter());
	}

}


```
#### 1.2.3.1 类适配器模式注意事项和细节
```markdown
1) Java是单继承机制，所以类适配器需要继承src类这一点算是一
个缺点, 因为这要求dst必须是接口，有一定局限性;
2) src类的方法在Adapter中都会暴露出来，也增加了使用的成本。
3) 由于其继承了src类，所以它可以根据需求重写src类的方法，
使得Adapter的灵活性增强了。
```
###  1.2.4 对象适配器模式
```markdown
1) 基本思路和类的适配器模式相同，只是将Adapter类作修改，
不是继承src类，而是持有src类的实例，以解决兼容性的问题。 
即：持有 src类，实现 dst 类接口，完成src->dst的适配
2) 根据“合成复用原则”，在系统中尽量使用关联关系来替
代继承关系。
3) 对象适配器模式是适配器模式常用的一种
```

```markdown
应用实例说明
以生活中充电器的例子来讲解适配器，充电器本身相当
于Adapter，220V交流电相当于src (即被适配者)，我
们的目dst(即目标)是5V直流电，使用对象适配器模
式完成。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201117205425296.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.adapter.objectadapter;

//被适配的类
public class Voltage220V {
	//输出220V的电压，不变
	public int output220V() {
		int src = 220;
		System.out.println("电压=" + src + "伏");
		return src;
	}
}



package com.yxj.adapter.objectadapter;


//适配接口
public interface IVoltage5V {
	public int output5V();
}



package com.yxj.adapter.objectadapter;

//适配器类
public class VoltageAdapter  implements IVoltage5V {

	private Voltage220V voltage220V; // 关联关系-聚合
	
	
	//通过构造器，传入一个 Voltage220V 实例
	public VoltageAdapter(Voltage220V voltage220v) {
		
		this.voltage220V = voltage220v;
	}



	@Override
	public int output5V() {
		
		int dst = 0;
		if(null != voltage220V) {
			int src = voltage220V.output220V();//获取220V 电压
			System.out.println("使用对象适配器，进行适配~~");
			dst = src / 44;
			System.out.println("适配完成，输出的电压为=" + dst);
		}
		
		return dst;
		
	}

}



package com.yxj.adapter.objectadapter;

public class Phone {

	//充电
	public void charging(IVoltage5V iVoltage5V) {
		if(iVoltage5V.output5V() == 5) {
			System.out.println("电压为5V, 可以充电~~");
		} else if (iVoltage5V.output5V() > 5) {
			System.out.println("电压大于5V, 不能充电~~");
		}
	}
}



package com.yxj.adapter.objectadapter;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		System.out.println(" === 对象适配器模式 ====");
		Phone phone = new Phone();
		phone.charging(new VoltageAdapter(new Voltage220V()));
	}

}

```
####  1.2.4.1 对象适配器模式对象适配器模式注意事项和细节
```markdown
1) 对象适配器和类适配器其实算是同一种思想，只不过
实现方式不同。根据合成复用原则，使用组合替代继
承， 所以它解决了类适配器必须继承src的
局限性问题，也不再要求dst必须是接口。
2) 使用成本更低，更灵活。
```
###  1.2.5 接口适配器模式
####  1.2.5.1 接口适配器模式介绍
```markdown
1) 一些书籍称为：适配器模式(Default Adapter Pattern)或
缺省适配器模式。
2) 当不需要全部实现接口提供的方法时，可先设计一个抽象
类实现接口，并为该接口中每个方法提供一个默认实
现（空方法），那么该抽象类的子类可有选择地覆
盖父类的某些方法来实现需求
3) 适用于一个接口不想使用其所有的方法的情况。
```

```markdown
应用实例
1) Android中的属性动画ValueAnimator类可以通
过addListener(AnimatorListener listener)方
法添加监听器， 那么常规写法如右：
2) 有时候我们不想实现
Animator.AnimatorListener接口的全部方法，我们
只想监听onAnimationStart，我们会如下写
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201117213012612.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
3) AnimatorListenerAdapter类，就是一个接口适配器，
代码如右图:它空实现了
Animator.AnimatorListener类(src)的所有方法.
4) AnimatorListener是一个接口. 
5) 程序里的匿名内部类就是Listener 具体实现类
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201117213410440.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201117213425646.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


![在这里插入图片描述](https://img-blog.csdnimg.cn/20201117211409423.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.adapter.interfaceadapter;

public interface Interface4 {
	public void m1();
	public void m2();
	public void m3();
	public void m4();
}


package com.yxj.adapter.interfaceadapter;

//在AbsAdapter 我们将 Interface4 的方法进行默认实现
public abstract class AbsAdapter implements Interface4 {

	//默认实现
	public void m1() {

	}

	public void m2() {

	}

	public void m3() {

	}

	public void m4() {

	}
}



package com.yxj.adapter.interfaceadapter;

public class Client {
	public static void main(String[] args) {
		
		AbsAdapter absAdapter = new AbsAdapter() {
			//只需要去覆盖我们 需要使用 接口方法
			@Override
			public void m1() {
				// TODO Auto-generated method stub
				System.out.println("使用了m1的方法");
			}
		};
		
		absAdapter.m1();
	}
}

```
####  1.2.5.2 适配器模式在SpringMVC框架应用的源码分析
```markdown
1) SpringMvc中的HandlerAdapter, 就使用了适配器模式
2) SpringMVC处理请求的流程回顾
3) 使用HandlerAdapter 的原因分析:
可以看到处理器的类型不同，有多重实现方式，那么调用方式就
不是确定的，如果需要直接调用Controller方法，需要调用的时候
就得不断是使用if else来进行判断是哪一种子类然后执行。那么
如果后面要扩展Controller，就得修改原来的代码，这样违背
了OCP原则。
4) 代码分析+Debug源码
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201117214318193.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
5) 动手写SpringMVC通过适配器设计模式获取到对
应的Controller的源码
说明：
Spring定义了一个适配接口，使得每一种Controller有
一种对应的适配器实现类
适配器代替controller执行相应的方法
扩展Controller 时，只需要增加一个适配器类就完成
了SpringMVC的扩展了,
这就是设计模式的力量
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020111722092693.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```java
package com.yxj.spring.springmvc;

//多种Controller实现  
public interface Controller {

}

class HttpController implements Controller {
	public void doHttpHandler() {
		System.out.println("http...");
	}
}

class SimpleController implements Controller {
	public void doSimplerHandler() {
		System.out.println("simple...");
	}
}

class AnnotationController implements Controller {
	public void doAnnotationHandler() {
		System.out.println("annotation...");
	}
}



package com.yxj.spring.springmvc;

import java.util.ArrayList;
import java.util.List;

public class DispatchServlet {

	public static List<HandlerAdapter> handlerAdapters = new ArrayList<HandlerAdapter>();

	public DispatchServlet() {
		handlerAdapters.add(new AnnotationHandlerAdapter());
		handlerAdapters.add(new HttpHandlerAdapter());
		handlerAdapters.add(new SimpleHandlerAdapter());
	}

	public void doDispatch() {

		// 此处模拟SpringMVC从request取handler的对象，
		// 适配器可以获取到希望的Controller
		 HttpController controller = new HttpController();
		// AnnotationController controller = new AnnotationController();
		//SimpleController controller = new SimpleController();
		// 得到对应适配器
		HandlerAdapter adapter = getHandler(controller);
		// 通过适配器执行对应的controller对应方法
		adapter.handle(controller);

	}

	public HandlerAdapter getHandler(Controller controller) {
		//遍历：根据得到的controller(handler), 返回对应适配器
		for (HandlerAdapter adapter : this.handlerAdapters) {
			if (adapter.supports(controller)) {
				return adapter;
			}
		}
		return null;
	}

	public static void main(String[] args) {
		new DispatchServlet().doDispatch(); // http...
	}

}



package com.yxj.spring.springmvc;

///定义一个Adapter接口 
public interface HandlerAdapter {
	public boolean supports(Object handler);

	public void handle(Object handler);
}

// 多种适配器类

class SimpleHandlerAdapter implements HandlerAdapter {

	public void handle(Object handler) {
		((SimpleController) handler).doSimplerHandler();
	}

	public boolean supports(Object handler) {
		return (handler instanceof SimpleController);
	}

}

class HttpHandlerAdapter implements HandlerAdapter {

	public void handle(Object handler) {
		((HttpController) handler).doHttpHandler();
	}

	public boolean supports(Object handler) {
		return (handler instanceof HttpController);
	}

}

class AnnotationHandlerAdapter implements HandlerAdapter {

	public void handle(Object handler) {
		((AnnotationController) handler).doAnnotationHandler();
	}

	public boolean supports(Object handler) {

		return (handler instanceof AnnotationController);
	}

}
```
####  1.2.5.3 适配器模式的注意事项和细节
```markdown
1) 三种命名方式，是根据 src是以怎样的形式给到
Adapter（在Adapter里的形式）来命名的。
2) 类适配器：以类给到，在Adapter里，就是将src当做类，继承
对象适配器：以对象给到，在Adapter里，将src作为一个对象，持有
接口适配器：以接口给到，在Adapter里，将src作为一个接口，实现
3) Adapter模式最大的作用还是将原本不兼容的接口融合在一
起工作。
4) 实际开发中，实现起来不拘泥于我们讲解的三种经典形式
```
##  1.3 桥接模式
###  1.3.1 手机操作问题
```markdown
现在对不同手机类型的不同品牌实现操作编
程(比如:开机、关机、上网，打电话等).
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020111807412341.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
###  1.3.2 传统方案解决手机操作问题
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201118074207514.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
传统方案解决手机操作问题分析
1) 扩展性问题(类爆炸)，如果我们再增加手机的样式(旋转式)，就
需要增加各个品牌手机的类，同样如果我们增加一个手机品牌，
也要在各个手机样式类下增加。
2) 违反了单一职责原则，当我们增加手机样式时，要同时增加
所有品牌的手机，这样增加了代码维护成本。
3) 解决方案-使用桥接模式
```
###  1.3.3 桥接模式(Bridge)
```markdown
基本介绍
1) 桥接模式(Bridge模式)是指：将实现与抽象放在两个不
同的类层次中，使两个层次可以独立改变。
2) 是一种结构型设计模式
3) Bridge模式基于类的最小设计原则，通过使用封装、聚
合及继承等行为让不同的类承担不同的职责。它的主要
特点是把抽象(Abstraction)与行为实现(Implementation)分
离开来，从而可以保持各部分的独立性以及应对他们的功能扩展
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201118075050580.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
1) Client类：桥接模式的调用者
2) 抽象类(Abstraction) :维护了 Implementor / 即它的实
现类ConcreteImplementorA.., 二者是聚合关系,
Abstraction 充当桥接类
3) RefinedAbstraction：是 Abstraction抽象类的子类
4) Implementor：行为实现类的接口
5)ConcretelmplementorA/B: 行为的具体实现类
6)从UML图：这里的抽象类和接口是聚合的关系，其
实调用和被调用关系
```
####  1.3.3.1 桥接模式解决手机操作问题
```markdown
使用桥接模式改进传统方式，让程序具有搞好的扩展性，
利用程序维护
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201118075909213.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```java
package com.yxj.bridge;

//接口
public interface Brand {
	void open();
	void close();
	void call();
}


package com.yxj.bridge;

public class XiaoMi implements Brand {

	@Override
	public void open() {
		// TODO Auto-generated method stub
		System.out.println(" 小米手机开机 ");
	}

	@Override
	public void close() {
		// TODO Auto-generated method stub
		System.out.println(" 小米手机关机 ");
	}

	@Override
	public void call() {
		// TODO Auto-generated method stub
		System.out.println(" 小米手机打电话 ");
	}

}


package com.yxj.bridge;

public class Vivo implements Brand {

	@Override
	public void open() {
		// TODO Auto-generated method stub
		System.out.println(" Vivo手机开机 ");
	}

	@Override
	public void close() {
		// TODO Auto-generated method stub
		System.out.println(" Vivo手机关机 ");
	}

	@Override
	public void call() {
		// TODO Auto-generated method stub
		System.out.println(" Vivo手机打电话 ");
	}

}



package com.yxj.bridge;

public abstract class Phone {
	
	//组合品牌
	private Brand brand;

	//构造器
	public Phone(Brand brand) {
		super();
		this.brand = brand;
	}
	
	protected void open() {
		this.brand.open();
	}
	protected void close() {
		brand.close();
	}
	protected void call() {
		brand.call();
	}
	
}


package com.yxj.bridge;


//折叠式手机类，继承 抽象类 Phone
public class FoldedPhone extends Phone {

	//构造器
	public FoldedPhone(Brand brand) {
		super(brand);
	}
	
	public void open() {
		super.open();
		System.out.println(" 折叠样式手机 ");
	}
	
	public void close() {
		super.close();
		System.out.println(" 折叠样式手机 ");
	}
	
	public void call() {
		super.call();
		System.out.println(" 折叠样式手机 ");
	}
}


package com.yxj.bridge;

public class UpRightPhone extends Phone {
	
		//构造器
		public UpRightPhone(Brand brand) {
			super(brand);
		}
		
		public void open() {
			super.open();
			System.out.println(" 直立样式手机 ");
		}
		
		public void close() {
			super.close();
			System.out.println(" 直立样式手机 ");
		}
		
		public void call() {
			super.call();
			System.out.println(" 直立样式手机 ");
		}
}



package com.yxj.bridge;

public class Client {

	public static void main(String[] args) {
		
		//获取折叠式手机 (样式 + 品牌 )
		
		Phone phone1 = new FoldedPhone(new XiaoMi());
		
		phone1.open();
		phone1.call();
		phone1.close();
		
		System.out.println("=======================");
		
		Phone phone2 = new FoldedPhone(new Vivo());
		
		phone2.open();
		phone2.call();
		phone2.close();
		
		System.out.println("==============");
		
		UpRightPhone phone3 = new UpRightPhone(new XiaoMi());
		
		phone3.open();
		phone3.call();
		phone3.close();
		
		System.out.println("==============");
		
		UpRightPhone phone4 = new UpRightPhone(new Vivo());
		
		phone4.open();
		phone4.call();
		phone4.close();
	}

}


```
####  1.3.3.2 桥接模式在JDBC的源码剖析
```markdown
1) Jdbc 的 Driver接口，如果从桥接模式来看，Driver就是一
个接口，下面可以有
MySQL的Driver，Oracle的Driver，这些就可以当做实现接口类
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201123150815911.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
###  1.3.4 桥接模式的注意事项和细节
```markdown
1) 实现了抽象和实现部分的分离，从而极大的提供了系统的灵活
性，让抽象部分和实现部分独立开来，这有助于系统进行分层设
计，从而产生更好的结构化系统。
2) 对于系统的高层部分，只需要知道抽象部分和实现部分的接口
就可以了，其它的部分由具体业务来完成。
3) 桥接模式替代多层继承方案，可以减少子类的个数，降低
系统的管理和维护成本。
4) 桥接模式的引入增加了系统的理解和设计难度，由于聚
合关联关系建立在抽象层，要求开发者针对抽象进行设计和编程
5) 桥接模式要求正确识别出系统中两个独立变化的维度，因此其
使用范围有一定的局限性，即需要有这样的应用场景。
```
###  1.3.5 桥接模式其它应用场景
```markdown
1) 对于那些不希望使用继承或因为多层次继承导致系统类的
个数急剧增加的系统，桥接模式尤为适用.
2) 常见的应用场景:
-JDBC驱动程序
-银行转账系统
转账分类: 网上转账，柜台转账，AMT转账
转账用户类型：普通用户，银卡用户，金卡用户..
-消息管理
消息类型：即时消息，延时消息
消息分类：手机短信，邮件消息，QQ消息...
```
##  1.4 装饰者模式
### 1.4.1 星巴克咖啡订单项目
```markdown
星巴克咖啡订单项目（咖啡馆）：
1) 咖啡种类/单品咖啡：Espresso(意大利浓咖啡)、ShortBlack、
LongBlack(美式咖啡)、Decaf(无因咖啡)
2) 调料：Milk、Soy(豆浆)、Chocolate
3) 要求在扩展新的咖啡种类时，具有良好的扩展性、
改动方便、维护方便
4) 使用OO的来计算不同种类咖啡的费用: 客户可以点单品
咖啡，也可以单品咖啡+调料组合。
```
#### 1.4.1.1 方案1-解决星巴克咖啡订单项目
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201123170319801.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
方案1-解决星巴克咖啡订单问题分析
1) Drink 是一个抽象类，表示饮料
2) des就是对咖啡的描述, 比如咖啡的名字
3) cost() 方法就是计算费用，Drink 类中做成一个抽象方法.
4) Decaf 就是单品咖啡， 继承Drink, 并实现cost
5) Espress && Milk 就是单品咖啡+调料， 这个组合很多
6) 问题：这样设计，会有很多类，当我们增加一个单品咖啡，
或者一个新的调料，类的数量就会倍增，就会出现类爆炸
```

```markdown
方案2-解决星巴克咖啡订单
前面分析到方案1因为咖啡单品+调料
组合会造成类的倍增，因此可以做改
进，将调料内置到Drink类，这样就不
会造成类数量过多。从而提高项目
的维护性(如图)
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201123170520588.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
说明: milk,soy,chocolate 可以设计为
Boolean,表示是否要添加相应的调料.
```

```markdown
方案2-的问题分析
1) 方案2可以控制类的数量，不至于造成很多的类
2) 在增加或者删除调料种类时，代码的维护量很大
3) 考虑到用户可以添加多份 调料时，可以将hasMilk 
返回一个对应int
4) 考虑使用 装饰者 模式
```
### 1.4.2 装饰者模式定义
```markdown
1) 装饰者模式：动态的将新功能附加到对象上。在对象功能
扩展方面，它比继承更有弹性，装饰者模式也体现了开闭原则(ocp)
```
### 1.4.3 装饰者模式(Decorator)原理
```markdown
1) 装饰者模式就像打包一个快递
 主体：比如：陶瓷、衣服 (Component) // 被装饰者
 包装：比如：报纸填充、塑料泡沫、纸板、木板(Decorator)
2) Component
主体：比如类似前面的Drink
3) ConcreteComponent和Decorator
ConcreteComponent：具体的主体，
比如前面的各个单品咖啡
Decorator: 装饰者，比如各调料.
4) 在如图的Component与ConcreteComponent之间，如果
ConcreteComponent类很多,还可以设计一个缓冲层，将共有的
部分提取出来，抽象层一个类。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201123170954937.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 1.4.4 装饰者模式解决星巴克咖啡订单
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112317110326.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
#### 1.4.4.1 装饰者模式下的订单：2份巧克力+一份牛奶的LongBlack
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201123173802764.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```java
装饰者模式咖啡订单项目应用实例
package com.yxj.decorator;

public abstract class Drink {

	public String des; // 描述
	private float price = 0.0f;
	public String getDes() {
		return des;
	}
	public void setDes(String des) {
		this.des = des;
	}
	public float getPrice() {
		return price;
	}
	public void setPrice(float price) {
		this.price = price;
	}
	
	//计算费用的抽象方法
	//子类来实现
	public abstract float cost();
	
}


package com.yxj.decorator;

public class Coffee  extends Drink {

	@Override
	public float cost() {
		// TODO Auto-generated method stub
		return super.getPrice();
	}

	
}


package com.yxj.decorator;

public class Espresso extends Coffee {
	
	public Espresso() {
		setDes(" 意大利咖啡 ");
		setPrice(6.0f);
	}
}


package com.yxj.decorator;

public class LongBlack extends Coffee {

	public LongBlack() {
		setDes(" longblack ");
		setPrice(5.0f);
	}
}


package com.yxj.decorator;

public class ShortBlack extends Coffee{
	
	public ShortBlack() {
		setDes(" shortblack ");
		setPrice(4.0f);
	}
}


package com.yxj.decorator;

public class Decorator extends Drink {
	private Drink obj;
	
	public Decorator(Drink obj) { //组合
		// TODO Auto-generated constructor stub
		this.obj = obj;
	}
	
	@Override
	public float cost() {
		// TODO Auto-generated method stub
		// getPrice 自己价格
		return super.getPrice() + obj.cost();
	}
	
	@Override
	public String getDes() {
		// TODO Auto-generated method stub
		// obj.getDes() 输出被装饰者的信息
		return des + " " + getPrice() + " && " + obj.getDes();
	}
	
	

}


package com.yxj.decorator;

//具体的Decorator， 这里就是调味品
public class Chocolate extends Decorator {

	public Chocolate(Drink obj) {
		super(obj);
		setDes(" 巧克力 ");
		setPrice(3.0f); // 调味品 的价格
	}

}



package com.yxj.decorator;

public class Milk extends Decorator {

	public Milk(Drink obj) {
		super(obj);
		// TODO Auto-generated constructor stub
		setDes(" 牛奶 ");
		setPrice(2.0f); 
	}

}


package com.yxj.decorator;

public class Soy extends Decorator{

	public Soy(Drink obj) {
		super(obj);
		// TODO Auto-generated constructor stub
		setDes(" 豆浆  ");
		setPrice(1.5f);
	}

}


package com.yxj.decorator;

public class DeCaf extends Coffee {

	public DeCaf() {
		setDes(" 无因咖啡 ");
		setPrice(1.0f);
	}
}



package com.yxj.decorator;

public class CoffeeBar {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		// 装饰者模式下的订单：2份巧克力+一份牛奶的LongBlack

		// 1. 点一份 LongBlack
		Drink order = new LongBlack();
		System.out.println("费用1=" + order.cost());
		System.out.println("描述=" + order.getDes());

		// 2. order 加入一份牛奶
		order = new Milk(order);

		System.out.println("order 加入一份牛奶 费用 =" + order.cost());
		System.out.println("order 加入一份牛奶 描述 = " + order.getDes());

		// 3. order 加入一份巧克力

		order = new Chocolate(order);

		System.out.println("order 加入一份牛奶 加入一份巧克力  费用 =" + order.cost());
		System.out.println("order 加入一份牛奶 加入一份巧克力 描述 = " + order.getDes());

		// 3. order 加入一份巧克力

		order = new Chocolate(order);

		System.out.println("order 加入一份牛奶 加入2份巧克力   费用 =" + order.cost());
		System.out.println("order 加入一份牛奶 加入2份巧克力 描述 = " + order.getDes());
	
		System.out.println("===========================");
		
		Drink order2 = new DeCaf();
		
		System.out.println("order2 无因咖啡  费用 =" + order2.cost());
		System.out.println("order2 无因咖啡 描述 = " + order2.getDes());
		
		order2 = new Milk(order2);
		
		System.out.println("order2 无因咖啡 加入一份牛奶  费用 =" + order2.cost());
		System.out.println("order2 无因咖啡 加入一份牛奶 描述 = " + order2.getDes());

	
	}

}


```

### 1.4.5 装饰者模式在JDK应用的源码分析
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201123174054319.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.jdk;

import java.io.DataInputStream;
import java.io.FileInputStream;
import java.io.InputStream;

public class Decorator {

	public static void main(String[] args) throws Exception{
		// TODO Auto-generated method stub
		
		//说明
		//1. InputStream 是抽象类, 类似我们前面讲的 Drink
		//2. FileInputStream 是  InputStream 子类，类似我们前面的 DeCaf, LongBlack
		//3. FilterInputStream  是  InputStream 子类：类似我们前面 的 Decorator 修饰者
		//4. DataInputStream 是 FilterInputStream 子类，具体的修饰者，类似前面的 Milk, Soy 等
		//5. FilterInputStream 类 有  protected volatile InputStream in; 即含被装饰者
		//6. 分析得出在jdk 的io体系中，就是使用装饰者模式
		
		DataInputStream dis = new DataInputStream(new FileInputStream("d:\\abc.txt"));
		System.out.println(dis.read());
		dis.close();
	}

}


```
##  1.5 组合模式
###  1.5.1 学校院系展示需求
```markdown
看一个学校院系展示需求
编写程序展示一个学校院系结构：需求是这样，要在一个页
面中展示出学校的院系组成，一个学校有多个学院，一个学院
有多个系。如图：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201123195627756.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
####  1.5.1.1 传统方案解决学校院系展示

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201123195705334.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
####  1.5.1.2 传统方案解决学校院系展示存在的问题分析

```markdown
1) 将学院看做是学校的子类，系是学院的子类，这样实际上是
站在组织大小来进行分层次的
2) 实际上我们的要求是 ：在一个页面中展示出学校的院系
组成，一个学校有多个学院，一个学院有多个系， 因此这
种方案，不能很好实现的管理的操作，比如
对学院、系的添加，删除，遍历等
3) 解决方案：把学校、院、系都看做是组织结构，他们之间没
有继承的关系，而是一个树形结构，可以更好的实现管理
操作。 => 组合模式
```
###  1.5.2 组合模式基本介绍

```markdown
1) 组合模式（Composite Pattern），又叫部分整体模式，
它创建了对象组的树形结构，将对象组合成树状结构以表
示“整体-部分”的层次关系。
2) 组合模式依据树形结构来组合对象，用来表示部分以及整体层次。
3) 这种类型的设计模式属于结构型模式。
4) 组合模式使得用户对单个对象和组合对象的访问具有一致性，
即：组合能让客户以一致的方式处理个别对象以及组合对象
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201123200029152.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
1) Component：这是组合中对象声明接口，在适当情况下，
实现所有类共有的接口默认行为，用于访问和管理
Component子部件， Component可以是抽象类或者接口
2)Leaf：在组合中表示叶子节点，叶子节点没有子节点
3) Composite：非叶子节点，用于存储子部件，在 
Component接口中实现子部件的相关操作，比如
增加(add),(remove)删除。
```
####  1.5.2.1 组合模式解决的问题

```markdown
1) 组合模式解决这样的问题，当我们的要处理的对象可以生成
一颗树形结构，而我们要对树上的节点和叶子进行操作时，它
能够提供一致的方式，而不用考虑它是节点还是叶子
2) 对应的示意图
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112320063168.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
####  1.5.2.2 组合模式解决学校院系展示的应用实例
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201123200802766.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
1) 应用实例要求
编写程序展示一个学校院系结构：需求是这样，要在一个页面
中展示出学校的院系组成，一个学校有多个学院，一个学院有多个系。
```
```java
package com.yxj.composite;

public abstract class OrganizationComponent {

	private String name; // 名字
	private String des; // 说明
	
	protected  void add(OrganizationComponent organizationComponent) {
		//默认实现
		throw new UnsupportedOperationException();
	}
	
	protected  void remove(OrganizationComponent organizationComponent) {
		//默认实现
		throw new UnsupportedOperationException();
	}

	//构造器
	public OrganizationComponent(String name, String des) {
		super();
		this.name = name;
		this.des = des;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getDes() {
		return des;
	}

	public void setDes(String des) {
		this.des = des;
	}
	
	//方法print, 做成抽象的, 子类都需要实现
	protected abstract void print();
		
}



package com.yxj.composite;

import java.util.ArrayList;
import java.util.List;

//University 就是 Composite , 可以管理College
public class University extends OrganizationComponent {

	List<OrganizationComponent> organizationComponents = new ArrayList<OrganizationComponent>();

	// 构造器
	public University(String name, String des) {
		super(name, des);
		// TODO Auto-generated constructor stub
	}

	// 重写add
	@Override
	protected void add(OrganizationComponent organizationComponent) {
		// TODO Auto-generated method stub
		organizationComponents.add(organizationComponent);
	}

	// 重写remove
	@Override
	protected void remove(OrganizationComponent organizationComponent) {
		// TODO Auto-generated method stub
		organizationComponents.remove(organizationComponent);
	}

	@Override
	public String getName() {
		// TODO Auto-generated method stub
		return super.getName();
	}

	@Override
	public String getDes() {
		// TODO Auto-generated method stub
		return super.getDes();
	}

	// print方法，就是输出University 包含的学院
	@Override
	protected void print() {
		// TODO Auto-generated method stub
		System.out.println("--------------" + getName() + "--------------");
		//遍历 organizationComponents 
		for (OrganizationComponent organizationComponent : organizationComponents) {
			organizationComponent.print();
		}
	}

}


package com.yxj.composite;

import java.util.ArrayList;
import java.util.List;

public class College extends OrganizationComponent {

	//List 中 存放的Department
	List<OrganizationComponent> organizationComponents = new ArrayList<OrganizationComponent>();

	// 构造器
	public College(String name, String des) {
		super(name, des);
		// TODO Auto-generated constructor stub
	}

	// 重写add
	@Override
	protected void add(OrganizationComponent organizationComponent) {
		// TODO Auto-generated method stub
		//  将来实际业务中，Colleage 的 add 和  University add 不一定完全一样
		organizationComponents.add(organizationComponent);
	}

	// 重写remove
	@Override
	protected void remove(OrganizationComponent organizationComponent) {
		// TODO Auto-generated method stub
		organizationComponents.remove(organizationComponent);
	}

	@Override
	public String getName() {
		// TODO Auto-generated method stub
		return super.getName();
	}

	@Override
	public String getDes() {
		// TODO Auto-generated method stub
		return super.getDes();
	}

	// print方法，就是输出University 包含的学院
	@Override
	protected void print() {
		// TODO Auto-generated method stub
		System.out.println("--------------" + getName() + "--------------");
		//遍历 organizationComponents 
		for (OrganizationComponent organizationComponent : organizationComponents) {
			organizationComponent.print();
		}
	}


}



package com.yxj.composite;

public class Department extends OrganizationComponent {

	//没有集合
	
	public Department(String name, String des) {
		super(name, des);
		// TODO Auto-generated constructor stub
	}

	
	//add , remove 就不用写了，因为他是叶子节点
	
	@Override
	public String getName() {
		// TODO Auto-generated method stub
		return super.getName();
	}
	
	@Override
	public String getDes() {
		// TODO Auto-generated method stub
		return super.getDes();
	}
	
	@Override
	protected void print() {
		// TODO Auto-generated method stub
		System.out.println(getName());
	}

}



package com.yxj.composite;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		//从大到小创建对象 学校
		OrganizationComponent university = new University("清华大学", " 中国顶级大学 ");
		
		//创建 学院
		OrganizationComponent computerCollege = new College("计算机学院", " 计算机学院 ");
		OrganizationComponent infoEngineercollege = new College("信息工程学院", " 信息工程学院 ");
		
		
		//创建各个学院下面的系(专业)
		computerCollege.add(new Department("软件工程", " 软件工程不错 "));
		computerCollege.add(new Department("网络工程", " 网络工程不错 "));
		computerCollege.add(new Department("计算机科学与技术", " 计算机科学与技术是老牌的专业 "));
		
		//
		infoEngineercollege.add(new Department("通信工程", " 通信工程不好学 "));
		infoEngineercollege.add(new Department("信息工程", " 信息工程好学 "));
		
		//将学院加入到 学校
		university.add(computerCollege);
		university.add(infoEngineercollege);
		
		//university.print();
		infoEngineercollege.print();
	}

}

```
###  1.5.3 组合模式在JDK集合的源码分析

```markdown
1) Java的集合类-HashMap就使用了组合模式
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201123201013720.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201123201033944.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```java
package com.yxj.jdk;

import java.util.HashMap;
import java.util.Map;

import jdk.internal.jimage.ImageReader.Node;

public class Composite {

	public static void main(String[] args) {
		
		
		//说明
		//1. Map 就是一个抽象的构建 (类似我们的Component)
		//2. HashMap是一个中间的构建(Composite), 实现/继承了相关方法
		//   put, putall 
		//3. Node 是 HashMap的静态内部类，类似Leaf叶子节点, 这里就没有put, putall
		//   static class Node<K,V> implements Map.Entry<K,V>
		
		Map<Integer,String> hashMap=new HashMap<Integer,String>();
		hashMap.put(0, "东游记");//直接存放叶子节点(Node)
		
		Map<Integer,String> map=new HashMap<Integer,String>();
		map.put(1, "西游记");
		map.put(2, "红楼梦"); //..
		hashMap.putAll(map);
		System.out.println(hashMap);
	

	}

}

```
###  1.5.4 组合模式的注意事项和细节
```markdown
1) 简化客户端操作。客户端只需要面对一致的对象而不用考虑
整体部分或者节点叶子的问题。
2) 具有较强的扩展性。当我们要更改组合对象时，我们只需
要调整内部的层次关系，客户端不用做出任何改动.
3) 方便创建出复杂的层次结构。客户端不用理会组合里面的组
成细节，容易添加节点或者叶子从而创建出复杂的树形结构
4) 需要遍历组织机构，或者处理的对象具有树形结构时, 非常适
合使用组合模式.
5) 要求较高的抽象性，如果节点和叶子有很多差异性的话，比
如很多方法和属性都不一样，不适合使用组合模式
```
##  1.6 外观模式
###  1.6.1  影院管理项目
```markdown
组建一个家庭影院：
DVD播放器、投影仪、自动屏幕、环绕立体声、爆米花机,要求
完成使用家庭影院的功能，其过程为：
直接用遥控器：统筹各设备开关
开爆米花机
放下屏幕
开投影仪
开音响
开DVD，选dvd
去拿爆米花
调暗灯光
播放
观影结束后，关闭各种设备
```
###  1.6.2  传统方式解决影院管理
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112323304417.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
###  1.6.3 传统方式解决影院管理问题分析
  ```markdown
1) 在ClientTest 的main方法中，创建各个子系统的对象，并直接
去调用子系统(对象)
相关方法，会造成调用过程混乱，没有清晰的过程
2) 不利于在ClientTest 中，去维护对子系统的操作
3) 解决思路：定义一个高层接口，给子系统中的一组接口提供一
个一致的界面(比
如在高层接口提供四个方法 ready, play, pause, end )，用来
访问子系统中的一群接口
4) 也就是说 就是通过定义一个一致的接口(界面类)，用以屏蔽内
部子系统的细节，使得调用端只需跟这个接口发生调用，而无需
关心这个子系统的内部细节 => 外观模式
```
###  1.6.4 基本介绍
```markdown
1) 外观模式（Facade），也叫“过程模式：外观模式为子系统中
的一组接口提供一个一致的界面，此模式定义了一个高层
接口，这个接口使得这一子系统更加容易使用
2) 外观模式通过定义一个一致的接口，用以屏蔽内部子系
统的细节，使得调用端只需跟这个接口发生调用，而无需
关心这个子系统的内部细节
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201123233636587.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
1)外观类(Facade):为调用端提供统一的调用接口，外观类
知道哪些子系统负责处理请求，从而将调用端的请求代
理给适当子系统对象
2)调用者(Client):外观接口的调用者
3)子系统的集合：指模块或者子系统，处理Facade对象
指派的任务，他是功能的实际提供者
```
###  1.6.5 外观模式解决影院管理
```markdown
1) 外观模式可以理解为转换一群接口，客户只
要调用一个接口，而不用调用多个接口才能
达到目的。比如：在pc上安装软件的时候经
常有一键安装选项（省去选择安装目录、安
装的组件等等），还有就是手机的重启功能
（把关机和启动合为一个操作）。
2) 外观模式就是解决多个复杂接口带来的使用
困难，起到简化用户操作的作用
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201123234325142.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
###  1.6.6 外观模式应用实例
```markdown
外观模式应用实例
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201123234458111.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.facade;

public class DVDPlayer {
	
	//使用单例模式, 使用饿汉式
	private static DVDPlayer instance = new DVDPlayer();
	
	public static DVDPlayer getInstanc() {
		return instance;
	}
	
	public void on() {
		System.out.println(" dvd on ");
	}
	public void off() {
		System.out.println(" dvd off ");
	}
	
	public void play() {
		System.out.println(" dvd is playing ");
	}
	
	//....
	public void pause() {
		System.out.println(" dvd pause ..");
	}
}


package com.yxj.facade;

public class Popcorn {
	
	private static Popcorn instance = new Popcorn();
	
	public static Popcorn getInstance() {
		return instance;
	}
	
	public void on() {
		System.out.println(" popcorn on ");
	}
	
	public void off() {
		System.out.println(" popcorn off ");
	}
	
	public void pop() {
		System.out.println(" popcorn is poping  ");
	}
}



package com.yxj.facade;

public class Projector {

	private static Projector instance = new Projector();
	
	public static Projector getInstance() {
		return instance;
	}
	
	public void on() {
		System.out.println(" Projector on ");
	}
	
	public void off() {
		System.out.println(" Projector ff ");
	}
	
	public void focus() {
		System.out.println(" Projector is Projector  ");
	}
	
	//...
}



package com.yxj.facade;

public class Screen {

	private static Screen instance = new Screen();
	
	public static Screen getInstance() {
		return instance;
	}
	
	
	
	
	public void up() {
		System.out.println(" Screen up ");
	}
	
	public void down() {
		System.out.println(" Screen down ");
	}
	

}



package com.yxj.facade;

public class Stereo {

	private static Stereo instance = new Stereo();
	
	public static Stereo getInstance() {
		return instance;
	}
	
	public void on() {
		System.out.println(" Stereo on ");
	}
	
	public void off() {
		System.out.println(" Screen off ");
	}
	
	public void up() {
		System.out.println(" Screen up.. ");
	}
	
	//...
}



package com.yxj.facade;

public class TheaterLight {

	private static TheaterLight instance = new TheaterLight();

	public static TheaterLight getInstance() {
		return instance;
	}

	public void on() {
		System.out.println(" TheaterLight on ");
	}

	public void off() {
		System.out.println(" TheaterLight off ");
	}

	public void dim() {
		System.out.println(" TheaterLight dim.. ");
	}

	public void bright() {
		System.out.println(" TheaterLight bright.. ");
	}
}



package com.yxj.facade;

public class HomeTheaterFacade {
	
	//定义各个子系统对象
	private TheaterLight theaterLight;
	private Popcorn popcorn;
	private Stereo stereo;
	private Projector projector;
	private Screen screen;
	private DVDPlayer dVDPlayer;
	
	
	//构造器
	public HomeTheaterFacade() {
		super();
		this.theaterLight = TheaterLight.getInstance();
		this.popcorn = Popcorn.getInstance();
		this.stereo = Stereo.getInstance();
		this.projector = Projector.getInstance();
		this.screen = Screen.getInstance();
		this.dVDPlayer = DVDPlayer.getInstanc();
	}

	//操作分成 4 步
	
	public void ready() {
		popcorn.on();
		popcorn.pop();
		screen.down();
		projector.on();
		stereo.on();
		dVDPlayer.on();
		theaterLight.dim();
	}
	
	public void play() {
		dVDPlayer.play();
	}
	
	public void pause() {
		dVDPlayer.pause();
	}
	
	public void end() {
		popcorn.off();
		theaterLight.bright();
		screen.up();
		projector.off();
		stereo.off();
		dVDPlayer.off();
	}
	
}


package com.yxj.facade;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//这里直接调用。。 很麻烦
		HomeTheaterFacade homeTheaterFacade = new HomeTheaterFacade();
		homeTheaterFacade.ready();
		homeTheaterFacade.play();
		
		
		homeTheaterFacade.end();
	}

}

```
###  1.6.7 外观模式在MyBatis框架应用的源码分析
```markdown
1) MyBatis 中的Configuration 去创建MetaObject 对象使
用到外观模式
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201123235841196.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201124000649264.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

###  1.6.8 外观模式的注意事项和细节
```markdown
1) 外观模式对外屏蔽了子系统的细节，因此外观模式降低了客
户端对子系统使用的复杂性
2) 外观模式对客户端与子系统的耦合关系，让子系统内部的模
块更易维护和扩展
3) 通过合理的使用外观模式，可以帮我们更好的划分访问的层次
4) 当系统需要进行分层设计时，可以考虑使用Facade模式
5) 在维护一个遗留的大型系统时，可能这个系统已经变得非常难
以维护和扩展，此时可以考虑为新系统开发一个Facade类，来提
供遗留系统的比较清晰简单的接口，让新系统与Facade类交互，
提高复用性
6) 不能过多的或者不合理的使用外观模式，使用外观模式好，
还是直接调用模块好。要以让系统有层次，利于维护为目的。
```
##  1.7 享元模式
###  1.7.1 展示网站项目需求
```markdown
小型的外包项目，给客户A做一个产品展示网站，客户A的朋
友感觉效果不错，也希望做这样的产品展示网站，但是要求都
有些不同：
1) 有客户要求以新闻的形式发布
2) 有客户人要求以博客的形式发布
3) 有客户希望以微信公众号的形式发布
```
###  1.7.2 传统方案解决网站展现项目
```markdown
1) 直接复制粘贴一份，然后根据客户不同要求，进行定制修改
2) 给每个网站租用一个空间
3) 方案设计示意图
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/202011241230291.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
###  1.7.3 传统方案解决网站展现项目-问题分析
```markdown
1) 需要的网站结构相似度很高，而且都不是高访问量网站，如果
分成多个虚拟空间来处理，相当于一个相同网站的实例对象很多，
造成服务器的资源浪费
2) 解决思路：整合到一个网站中，共享其相关的代码和数据，
对于硬盘、内存、CPU、数据库空间等服务器资源都可以达
成共享，减少服务器资源
3) 对于代码来说，由于是一份实例，维护和扩展都更加容易
4) 上面的解决思路就可以使用 享元模式 来解决
```
###  1.7.4 享元模式基本介绍
```markdown
1) 享元模式（Flyweight Pattern） 也叫蝇量模式: 运
用共享技术有效地支持大量细粒度的对象
2) 常用于系统底层开发，解决系统的性能问题。像
数据库连接池，里面都是创建好的连接对象，在
这些连接对象中有我们需要的则直接拿来用，避
免重新创建，如果没有我们需要的，则创建一个
3) 享元模式能够解决重复对象的内存浪费的问题，
当系统中有大量相似对象，需要缓冲池时。不需
总是创建新对象，可以从缓冲池里拿。这样可以
降低系统内存，同时提高效率
4) 享元模式经典的应用场景就是池技术了，String常
量池、数据库连接池、缓冲池等等都是享元模式
的应用，享元模式是池技术的重要实现方式
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201124123227273.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
###  1.7.5 享元模式的原理类图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201124124117825.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
* 对类图的说明
```markdown
对原理图的说明-即（模式的角色及职责)
1） FlyWeight是抽象的享元角色，他是产品的抽象类，同时
定义出对象的外部状态和内部状态（后面介绍）的接口或实现
2) ConcretelyWeight是具体的享元角色，是具体的产品类，实
现抽象角色定义相关业务
3) UnsharedconcreteFlyWeight是不可共享的角色，一般
不会出现在享元工厂
4) FlyWeightfactory享元工厂类，用于构建一个池容器（集合），
同时提供从池中获取对象方法
```
###  1.7.6 对类图的说明内部状态和外部状态
```markdown
比如围棋、五子棋、跳棋，它们都有大量的棋子对象，围棋和
五子棋只有黑白两色，跳棋颜色多一点，所以棋子颜色就是棋
子的内部状态；而各个棋子之间的差别就是位置的不同，当我
们落子后，落子颜色是定的，但位置是变化的，所以棋子坐标
就是棋子的外部状态
1) 享元模式提出了两个要求：细粒度和共享对象。这里就涉
及到内部状态和外部状态了，即将对象的信息分为两个部分：
内部状态和外部状态
2) 内部状态指对象共享出来的信息，存储在享元对象内部且
不会随环境的改变而改变
3) 外部状态指对象得以依赖的一个标记，是随环境改变而
改变的、不可共享的状态。
4) 举个例子：围棋理论上有361个空位可以放棋子，每盘
棋都有可能有两三百个棋子对象产生，因为内存空间有限，
一台服务器很难支持更多的玩家玩围棋游戏，如果用享
元模式来处理棋子，那么棋子对象就可以减少到只有两
个实例，这样就很好的解决了对象的开销问题
```
###  1.7.7 享元模式解决网站展现项目
```markdown
使用享元模式完成，前面提出的网站外包问题
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201124124701245.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.flyweight;

public class User {
	
	private String name;

	
	public User(String name) {
		super();
		this.name = name;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
	
	
}


package com.yxj.flyweight;

public abstract class WebSite {

	public abstract void use(User user);//抽象方法
}



package com.yxj.flyweight;

//具体网站
public class ConcreteWebSite extends WebSite {

	//共享的部分，内部状态
	private String type = ""; //网站发布的形式(类型)

	
	//构造器
	public ConcreteWebSite(String type) {
		
		this.type = type;
	}


	@Override
	public void use(User user) {
		// TODO Auto-generated method stub
		System.out.println("网站的发布形式为:" + type + " 在使用中 .. 使用者是" + user.getName());
	}
	
	
}


package com.yxj.flyweight;

import java.util.HashMap;

// 网站工厂类，根据需要返回压一个网站
public class WebSiteFactory {

	
	//集合， 充当池的作用
	private HashMap<String, ConcreteWebSite> pool = new HashMap<>();
	
	//根据网站的类型，返回一个网站, 如果没有就创建一个网站，并放入到池中,并返回
	public WebSite getWebSiteCategory(String type) {
		if(!pool.containsKey(type)) {
			//就创建一个网站，并放入到池中
			pool.put(type, new ConcreteWebSite(type));
		}
		
		return (WebSite)pool.get(type);
	}
	
	//获取网站分类的总数 (池中有多少个网站类型)
	public int getWebSiteCount() {
		return pool.size();
	}
}



package com.yxj.flyweight;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		// 创建一个工厂类
		WebSiteFactory factory = new WebSiteFactory();

		// 客户要一个以新闻形式发布的网站
		WebSite webSite1 = factory.getWebSiteCategory("新闻");

		
		webSite1.use(new User("tom"));

		// 客户要一个以博客形式发布的网站
		WebSite webSite2 = factory.getWebSiteCategory("博客");

		webSite2.use(new User("jack"));

		// 客户要一个以博客形式发布的网站
		WebSite webSite3 = factory.getWebSiteCategory("博客");

		webSite3.use(new User("smith"));

		// 客户要一个以博客形式发布的网站
		WebSite webSite4 = factory.getWebSiteCategory("博客");

		webSite4.use(new User("king"));
		
		System.out.println("网站的分类共=" + factory.getWebSiteCount());
	}

}

```
###  1.7.8 享元模式在JDK-Interger的应用源码分析
```markdown
1) Integer中的享元模式
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201124130610644.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```java
package com.yxj.jdk;

public class FlyWeight {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//如果 Integer.valueOf(x) x 在  -128 --- 127 直接，就是使用享元模式返回,如果不在
		//范围类，则仍然 new 
		
		//小结:
		//1. 在valueOf 方法中，先判断值是否在 IntegerCache 中，如果不在，就创建新的Integer(new), 否则，就直接从 缓存池返回
		//2. valueOf 方法，就使用到享元模式
		//3. 如果使用valueOf 方法得到一个Integer 实例，范围在 -128 - 127 ，执行速度比 new 快
		
		
		Integer x = Integer.valueOf(127); // 得到 x实例，类型 Integer
		Integer y = new Integer(127); // 得到 y 实例，类型 Integer
		Integer z = Integer.valueOf(127);//..
		Integer w = new Integer(127);
		
		
		
		System.out.println(x.equals(y)); // 大小，true
		System.out.println(x == y ); //  false
		System.out.println(x == z ); // true
		System.out.println(w == x ); // false
		System.out.println(w == y ); // false
		
		
		Integer x1 = Integer.valueOf(200);
		Integer x2 = Integer.valueOf(200);
		System.out.println("x1==x2" + (x1 == x2)); // false
		
		
		


	}

}

```
###  1.7.9 享元模式的注意事项和细节
```markdown
1) 在享元模式这样理解，“享”就表示共享，“元”表示对象
2) 系统中有大量对象，这些对象消耗大量内存，并且对象
的状态大部分可以外部化时，我们就可以考虑选用享元模式
3) 用唯一标识码判断，如果在内存中有，则返回这个唯一标
识码所标识的对象，用HashMap/HashTable存储
4) 享元模式大大减少了对象的创建，降低了程序内存的占用，
提高效率
5) 享元模式提高了系统的复杂度。需要分离出内部状态和外部
状态，而外部状态具有固化特性，不应该随着内部状态的改
变而改变，这是我们使用享元模式需要注意的地方.
6) 使用享元模式时，注意划分内部状态和外部状态，并
且需要有一个工厂类加以控制。
7) 享元模式经典的应用场景是需要缓冲池的场景，
比如 String常量池、数据库连接池
```
##  1.8 代理模式(Proxy)
###  1.8.1 代理模式的基本介绍
```markdown
1) 代理模式：为一个对象提供一个替身，以控制对这个对象的
访问。即通过代理对象访问目标对象.这样做的好处是:可以在目
标对象实现的基础上,增强额外的功能操作,即扩展目标对象的功能。
2) 被代理的对象可以是远程对象、创建开销大的对象或需要安全
控制的对象
3) 代理模式有不同的形式, 主要有三种 静态代理、动态代
理 (JDK代理、接口代理)和 Cglib代理 (可以在内存动态的
创建对象，而不需要实现接口， 他是属于动态代理的范畴) 。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201124140448266.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
###  1.8.2 静态代理
```markdown
静态代理在使用时,需要定义接口或者父类,被代理对象(即目标对象)与
代理对象一起实现相同的接口或者是继承相同父类
```

```markdown
应用实例
1) 定义一个接口:ITeacherDao
2) 目标对象TeacherDAO实现接口ITeacherDAO
3) 使用静态代理方式,就需要在代理对象TeacherDAOProxy中
也实现ITeacherDAO
4) 调用的时候通过调用代理对象的方法来调用目标对象.
5) 特别提醒：代理对象与目标对象要实现相同的接口,然后通
过调用相同的方法来调用目标对象的方法。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201124140623644.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.proxy.staticproxy;

//接口
public interface ITeacherDao {
	
	void teach(); // 授课的方法
}


package com.yxj.proxy.staticproxy;

public class TeacherDao implements ITeacherDao {

	@Override
	public void teach() {
		// TODO Auto-generated method stub
		System.out.println(" 老师授课中  。。。。。");
	}

}


package com.yxj.proxy.staticproxy;

//代理对象，静态代理
public class TeacherDaoProxy implements ITeacherDao{
	
	private ITeacherDao target;  // 目标对象，通过接口来聚合
	
	
	// 构造器
	public TeacherDaoProxy(ITeacherDao target) {
		this.target = target;
	}



	@Override
	public void teach() {
		// TODO Auto-generated method stub
		System.out.println("开始代理 完成某些操作。。。。。。。");
		target.teach();
		System.out.println("提交");//
	}

}

package com.yxj.proxy.staticproxy;

public class Client {

	public static void main(String[] args) {
		
		// 创建目标对象（被代理对象）
		TeacherDao teacherDao = new TeacherDao();
		
		 // 创建代理对象，同时将被代理对象传递给代理对象
		TeacherDaoProxy teacherDaoProxy = new TeacherDaoProxy(teacherDao);
		
		// 通过代理对象，调用到被代理对象的方法
// 即：执行的是代理对象的方法，代理对象再去调用目标对象的方法
		teacherDaoProxy.teach();
	}

}

```
####  1.8.2.1 静态代理优缺点
```markdown
1) 优点：在不修改目标对象的功能前提下, 能通过代理对象对目标功能扩展
2) 缺点：因为代理对象需要与目标对象实现一样的接口,所以会有很多代理类
3) 一旦接口增加方法,目标对象与代理对象都要维护
```
###  1.8.3 动态代理 
####  1.8.3.1 动态代理模式的基本介绍
```markdown
1) 代理对象,不需要实现接口，但是目标对象要实现接口，否则
不能用动态代理
2) 代理对象的生成，是利用JDK的API，动态的在内存中构建
代理对象
3) 动态代理也叫做：JDK代理、接口代理
```
####  1.8.3.2 JDK中生成代理对象的API
```markdown
1) 代理类所在包:java.lang.reflect.Proxy
2) JDK实现代理只需要使用newProxyInstance方法,但是该方法需
要接收三个参数,完整的写法是:
static Object newProxyInstance(ClassLoader loader, Class<?>[]
interfaces,InvocationHandler h )
```
####  1.8.3.3 动态代理应用实例
```markdown
将前面的静态代理改进成动态代理模式(即：JDK代理模式)
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201124143610333.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.proxy.dynamic;

//接口
public interface ITeacherDao {

	void teach(); // 授课方法
	void sayHello(String name);
}


package com.yxj.proxy.dynamic;

public class TeacherDao implements ITeacherDao {

	@Override
	public void teach() {
		// TODO Auto-generated method stub
		System.out.println(" 老师授课中.... ");
	}

	@Override
	public void sayHello(String name) {
		// TODO Auto-generated method stub
		System.out.println("hello " + name);
	}
	
}


package com.yxj.proxy.dynamic;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class ProxyFactory {

	//维护一个目标对象 , Object
	private Object target;

	//构造器 ， 对target 进行初始化
	public ProxyFactory(Object target) {
		
		this.target = target;
	} 
	
	//给目标对象 生成一个代理对象
	public Object getProxyInstance() {
		
		//说明
		/*
		 *  public static Object newProxyInstance(ClassLoader loader,
                                          Class<?>[] interfaces,
                                          InvocationHandler h)
                                          
            //1. ClassLoader loader ： 指定当前目标对象使用的类加载器, 获取加载器的方法固定
            //2. Class<?>[] interfaces: 目标对象实现的接口类型，使用泛型方法确认类型
            //3. InvocationHandler h : 事情处理，执行目标对象的方法时，会触发事情处理器方法, 会把当前执行的目标对象方法作为参数传入
		 */
		return Proxy.newProxyInstance(target.getClass().getClassLoader(), 
				target.getClass().getInterfaces(), 
				new InvocationHandler() {
					
					@Override
					public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
						// TODO Auto-generated method stub
						System.out.println("JDK代理开始~~");
						//反射机制调用目标对象的方法
						Object returnVal = method.invoke(target, args);
						System.out.println("JDK代理提交");
						return returnVal;
					}
				}); 
	}
	
	
}


package com.yxj.proxy.dynamic;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//创建目标对象
		ITeacherDao target = new TeacherDao();
		
		//给目标对象，创建代理对象, 可以转成 ITeacherDao
		ITeacherDao proxyInstance = (ITeacherDao)new ProxyFactory(target).getProxyInstance();
	
		// proxyInstance=class com.sun.proxy.$Proxy0 内存中动态生成了代理对象
		System.out.println("proxyInstance=" + proxyInstance.getClass());
		
		//通过代理对象，调用目标对象的方法
		//proxyInstance.teach();
		
		proxyInstance.sayHello(" tom ");
	}

}

```
####  1.8.3.4 Cglib代理模式的基本介绍
```markdown
1) 静态代理和JDK代理模式都要求目标对象是实现一个接口,但是
有时候目标对象只是一个单独的对象,并没有实现任何的接口,这个
时候可使用目标对象子类来实现代理-这就是Cglib代理
2) Cglib代理也叫作子类代理,它是在内存中构建一个子类对象
从而实现对目标对象功能扩展, 有些书也将Cglib代理归属到动
态代理。
3) Cglib是一个强大的高性能的代码生成包,它可以在运行期
扩展java类与实现java接口.它广泛的被许多AOP的框架
使用,例如Spring AOP，实现方法拦截
4) 在AOP编程中如何选择代理模式：
1. 目标对象需要实现接口，用JDK代理
2. 目标对象不需要实现接口，用Cglib代理
5) Cglib包的底层是通过使用字节码处理框架ASM来
转换字节码并生成新的类
```
####  1.8.3.5 Cglib代理模式实现步骤
```markdown
1) 需要引入cglib的jar文件
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201124151219768.png#pic_center)

```markdown
2) 在内存中动态构建子类，注意代理的类不能为final，否则报错
java.lang.IllegalArgumentException:
```
```markdown
3) 目标对象的方法如果为final/static,那么就不会被拦截,即不
会执行目标对象额外的业务方法.
```
####  1.8.3.6 Cglib代理模式应用实例
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201124151355219.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.proxy.cglib;

public class TeacherDao {

	public String teach() {
		System.out.println(" 老师授课中  ， 我是cglib代理，不需要实现接口 ");
		return "hello";
	}
}



package com.yxj.proxy.cglib;

import java.lang.reflect.Method;

import net.sf.cglib.proxy.Enhancer;
import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;

public class ProxyFactory implements MethodInterceptor {

	//维护一个目标对象
	private Object target;
	
	//构造器，传入一个被代理的对象
	public ProxyFactory(Object target) {
		this.target = target;
	}

	//返回一个代理对象:  是 target 对象的代理对象
	public Object getProxyInstance() {
		//1. 创建一个工具类
		Enhancer enhancer = new Enhancer();
		//2. 设置父类
		enhancer.setSuperclass(target.getClass());
		//3. 设置回调函数
		enhancer.setCallback(this);
		//4. 创建子类对象，即代理对象
		return enhancer.create();
		
	}
	

	//重写  intercept 方法，会调用目标对象的方法
	@Override
	public Object intercept(Object arg0, Method method, Object[] args, MethodProxy arg3) throws Throwable {
		// TODO Auto-generated method stub
		System.out.println("Cglib代理模式 ~~ 开始");
		Object returnVal = method.invoke(target, args);
		System.out.println("Cglib代理模式 ~~ 提交");
		return returnVal;
	}

}


package com.yxj.proxy.cglib;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//创建目标对象
		TeacherDao target = new TeacherDao();
		//获取到代理对象，并且将目标对象传递给代理对象
		TeacherDao proxyInstance = (TeacherDao)new ProxyFactory(target).getProxyInstance();

		//执行代理对象的方法，触发intecept 方法，从而实现 对目标对象的调用
		String res = proxyInstance.teach();
		System.out.println("res=" + res);
	}

}

```

####  1.8.3.7 代理模式(Proxy)的变体
```markdown
几种常见的代理模式介绍— 几种变体
1) 防火墙代理
内网通过代理穿透防火墙，实现对公网的访问。
2) 缓存代理
比如：当请求图片文件等资源时，先到缓存代理取，如果取
到资源则ok,如果取不到资源，再到公网或者数据库取，然后缓存。
3) 远程代理
远程对象的本地代表，通过它可以把远程对象当本地对象来
调用。远程代理通过网络和真正的远程对象沟通信息。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201124153205136.png#pic_center)

```markdown
4) 同步代理：主要使用在多线程编程中，完成多线程间同步工作
```
##  1.9 模板方法模式
###  1.9.1 豆浆制作问题
```markdown
编写制作豆浆的程序，说明如下:
1) 制作豆浆的流程 选材--->添加配料--->浸泡--->放到豆浆机打碎
2) 通过添加不同的配料，可以制作出不同口味的豆浆
3) 选材、浸泡和放到豆浆机打碎这几个步骤对于制作每种口味的豆浆
都是一样的
4) 请使用 模板方法模式 完成 (说明：因为模板方法模式，比较简
单，很容易就想到这个方案，因此就直接使用，不再使用传统的
方案来引出模板方法模式 )
```
###  1.9.2 模板方法模式基本介绍
```markdown
1) 模板方法模式（Template Method Pattern），又叫模板
模式(Template Pattern)，在一个抽象类公开定义了执行它的方
法的模板。它的子类可以按需要重写方法实现，但调用将以抽象
类中定义的方式进行。
2) 简单说，模板方法模式 定义一个操作中的算法的骨架，而
将一些步骤延迟到子类中，使得子类可以不改变一个算法
的结构，就可以重定义该算法的某些特定步骤
3) 这种类型的设计模式属于行为型模式。
```
###  1.9.3 模板方法模式的原理类图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201124214403749.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
1) AbstractClass 抽象类， 类中实现了模板方法(template)，
定义了算法的骨架，具体子类需要去实现 其它的抽象方
法operationr2,3,4
2) ConcreteClass 实现抽象方法operationr2,3,4, 以完成
算法中特点子类的步骤
```
###  1.9.4 模板方法模式解决豆浆制作问题
```markdown
1) 应用实例要求
编写制作豆浆的程序，说明如下:
制作豆浆的流程 选材--->添加配料--->浸泡--->放到豆浆机打碎
通过添加不同的配料，可以制作出不同口味的豆浆
选材、浸泡和放到豆浆机打碎这几个步骤对于制作每种口味的豆
浆都是一样的(红豆、花生豆浆。。。)
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112421460159.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.template;

//抽象类，表示豆浆
public abstract class SoyaMilk {

	//模板方法, make , 模板方法可以做成final , 不让子类去覆盖.
	final void make() {
		
		select(); 
		addCondiments();
		soak();
		beat();
		
	}
	
	//选材料
	void select() {
		System.out.println("第一步：选择好的新鲜黄豆  ");
	}
	
	//添加不同的配料， 抽象方法, 子类具体实现
	abstract void addCondiments();
	
	//浸泡
	void soak() {
		System.out.println("第三步， 黄豆和配料开始浸泡， 需要3小时 ");
	}
	 
	void beat() {
		System.out.println("第四步：黄豆和配料放到豆浆机去打碎  ");
	}
}


package com.yxj.template;

public class RedBeanSoyaMilk extends SoyaMilk {

	@Override
	void addCondiments() {
		// TODO Auto-generated method stub
		System.out.println(" 加入上好的红豆 ");
	}

}


package com.yxj.template;

public class PeanutSoyaMilk extends SoyaMilk {

	@Override
	void addCondiments() {
		// TODO Auto-generated method stub
		System.out.println(" 加入上好的花生 ");
	}

}


package com.yxj.template;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//制作红豆豆浆
		
		System.out.println("----制作红豆豆浆----");
		SoyaMilk redBeanSoyaMilk = new RedBeanSoyaMilk();
		redBeanSoyaMilk.make();
		
		System.out.println("----制作花生豆浆----");
		SoyaMilk peanutSoyaMilk = new PeanutSoyaMilk();
		peanutSoyaMilk.make();
	}

}

```
### 1.9.5 模板方法模式的钩子方法
```markdown
1) 在模板方法模式的父类中，我们可以定义一个方法，它默认
不做任何事，子类可以视情况要不要覆盖它，该方法称为“钩子”。
2) 还是用上面做豆浆的例子来讲解，比如，我们还希望制作纯
豆浆，不添加任何的配料，请使用钩子方法对前面的模板方法进行改造
```
```java
package com.yxj.template.improve;

//抽象类，表示豆浆
public abstract class SoyaMilk {

	//模板方法, make , 模板方法可以做成final , 不让子类去覆盖.
	final void make() {
		
		select(); 
		if(customerWantCondiments()) {
			addCondiments();
		}
		soak();
		beat();
		
	}
	
	//选材料
	void select() {
		System.out.println("第一步：选择好的新鲜黄豆  ");
	}
	
	//添加不同的配料， 抽象方法, 子类具体实现
	abstract void addCondiments();
	
	//浸泡
	void soak() {
		System.out.println("第三步， 黄豆和配料开始浸泡， 需要3小时 ");
	}
	 
	void beat() {
		System.out.println("第四步：黄豆和配料放到豆浆机去打碎  ");
	}
	
	//钩子方法，决定是否需要添加配料
	boolean customerWantCondiments() {
		return true;
	}
}


package com.yxj.template.improve;

public class PeanutSoyaMilk extends SoyaMilk {

	@Override
	void addCondiments() {
		// TODO Auto-generated method stub
		System.out.println(" 加入上好的花生 ");
	}

}


package com.yxj.template.improve;

public class RedBeanSoyaMilk extends SoyaMilk {

	@Override
	void addCondiments() {
		// TODO Auto-generated method stub
		System.out.println(" 加入上好的红豆 ");
	}

}


package com.yxj.template.improve;

public class PureSoyaMilk extends SoyaMilk{

	@Override
	void addCondiments() {
		// TODO Auto-generated method stub
		//空实现
	}
	
	@Override
	boolean customerWantCondiments() {
		// TODO Auto-generated method stub
		return false;
	}
 
}


package com.yxj.template.improve;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//制作红豆豆浆
		
		System.out.println("----制作红豆豆浆----");
		SoyaMilk redBeanSoyaMilk = new RedBeanSoyaMilk();
		redBeanSoyaMilk.make();
		
		System.out.println("----制作花生豆浆----");
		SoyaMilk peanutSoyaMilk = new PeanutSoyaMilk();
		peanutSoyaMilk.make();
		
		System.out.println("----制作纯豆浆----");
		SoyaMilk pureSoyaMilk = new PureSoyaMilk();
		pureSoyaMilk.make();
	}

}

```
###  1.9.6  模板方法模式在Spring框架应用的源码分析
```markdown
1) Spring IOC容器初始化时运用到的模板方法模式
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201124222747898.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
###  1.9.7  模板方法模式的注意事项和细节
```markdown
1) 基本思想是：算法只存在于一个地方，也就是在父类中，容
易修改。需要修改算法时，只要修改父类的模板方法或者已经
实现的某些步骤，子类就会继承这些修改
2) 实现了最大化代码复用。父类的模板方法和已实现的某
些步骤会被子类继承而直接使用。
3) 既统一了算法，也提供了很大的灵活性。父类的模板方法
确保了算法的结构保持不变，同时由子类提供部分步骤的实现。
4) 该模式的不足之处：每一个不同的实现都需要一个子类实
现，导致类的个数增加，使得系统更加庞大
5) 一般模板方法都加上final关键字， 防止子类重写模板方法.
6) 模板方法模式使用场景：当要完成在某个过程，该过程
要执行一系列步骤 ，这一系列的步骤基本相同，但其个别
步骤在实现时 可能不同，通常考虑用模板方法模式来处理
```
##  1.10 命令模式
###  1.10.1 智能生活项目需求
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125000320145.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
1) 我们买了一套智能家电，有照明灯、风扇、冰箱、洗衣机，我
们只要在手机上安装app就可以控制对这些家电工作。
2) 这些智能家电来自不同的厂家，我们不想针对每一种家电
都安装一个App，分别控制，我们希望只要一个app就可以控
制全部智能家电。
3) 要实现一个app控制所有智能家电的需要，则每个智能
家电厂家都要提供一个统一的接口给app调用，这时 就
可以考虑使用命令模式。
4) 命令模式可将“动作的请求者”从“动作的执行者”对象中解耦出来.
5) 在我们的例子中，动作的请求者是手机app，动作的执行
者是每个厂商的一个家电产品
```
###  1.10.2 命令模式基本介绍
```markdown
1) 命令模式（Command Pattern）：在软件设计中，我们经常需要
向某些对象发送请求，但是并不知道请求的接收者是谁，也不知
道被请求的操作是哪个，
我们只需在程序运行时指定具体的请求接收者即可，此时，可以
使用命令模式来进行设计
2) 命名模式使得请求发送者与请求接收者消除彼此之间的耦合，让
对象之间的调用关系更加灵活，实现解耦。
3) 在命名模式中，会将一个请求封装为一个对象，以便使用不同参
数来表示不同的请求(即命名)，同时命令模式也支持可撤销的操作。
4) 通俗易懂的理解：将军发布命令，士兵去执行。其中有几个角色：
将军（命令发布者）、士兵（命令的具体执行者）、命令(连接将
军和士兵)。
Invoker是调用者（将军），Receiver是被调用者（士兵），
MyCommand是命令，实现了Command接口，持有接收对象
```
###  1.10.3 命令模式的原理类图
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112500070394.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
1) Invoker 是调用者角色
2) Command: 是命令角色，需要执行的所有命令都在这里，
可以是接口或抽象类
3) Receiver: 接受者角色，知道如何实施和执行一个请求
相关的操作
4) ConcreteCommand: 将一个接受者对象与一个动作绑定，
调用接受者相应的操作，实现execute
```
###  1.10.4 命令模式解决智能生活项目
```markdown
编写程序，使用命令模式 完成前面的智能家电项目
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125000839411.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```java
package com.yxj.command;


//创建命令接口
public interface Command {

	//执行动作(操作)
	public void execute();
	//撤销动作(操作)
	public void undo();
}



package com.yxj.command;

public class LightOnCommand implements Command {

	//聚合LightReceiver
	
	LightReceiver light;
	
	//构造器
	public LightOnCommand(LightReceiver light) {
		super();
		this.light = light;
	}
	
	@Override
	public void execute() {
		// TODO Auto-generated method stub
		//调用接收者的方法
		light.on();
	}

	

	@Override
	public void undo() {
		// TODO Auto-generated method stub
		//调用接收者的方法
		light.off();
	}

}



package com.yxj.command;

public class LightReceiver {

	public void on() {
		System.out.println(" 电灯打开了.. ");
	}
	
	public void off() {
		System.out.println(" 电灯关闭了.. ");
	}
}



package com.yxj.command;

public class LightOffCommand implements Command {

	// 聚合LightReceiver

	LightReceiver light;

	// 构造器
	public LightOffCommand(LightReceiver light) {
			super();
			this.light = light;
		}

	@Override
	public void execute() {
		// TODO Auto-generated method stub
		// 调用接收者的方法
		light.off();
	}

	@Override
	public void undo() {
		// TODO Auto-generated method stub
		// 调用接收者的方法
		light.on();
	}
}


package com.yxj.command;

/**
 * 没有任何命令，即空执行: 用于初始化每个按钮, 当调用空命令时，对象什么都不做
 * 其实，这样是一种设计模式, 可以省掉对空判断
 * @author Administrator
 *
 */
public class NoCommand implements Command {

	@Override
	public void execute() {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void undo() {
		// TODO Auto-generated method stub
		
	}

}



package com.yxj.command;

public class RemoteController {

	// 开 按钮的命令数组
	Command[] onCommands;
	Command[] offCommands;

	// 执行撤销的命令
	Command undoCommand;

	// 构造器，完成对按钮初始化

	public RemoteController() {

		onCommands = new Command[5];
		offCommands = new Command[5];

		for (int i = 0; i < 5; i++) {
			onCommands[i] = new NoCommand();
			offCommands[i] = new NoCommand();
		}
	}

	// 给我们的按钮设置你需要的命令
	public void setCommand(int no, Command onCommand, Command offCommand) {
		onCommands[no] = onCommand;
		offCommands[no] = offCommand;
	}

	// 按下开按钮
	public void onButtonWasPushed(int no) { // no 0
		// 找到你按下的开的按钮， 并调用对应方法
		onCommands[no].execute();
		// 记录这次的操作，用于撤销
		undoCommand = onCommands[no];

	}

	// 按下开按钮
	public void offButtonWasPushed(int no) { // no 0
		// 找到你按下的关的按钮， 并调用对应方法
		offCommands[no].execute();
		// 记录这次的操作，用于撤销
		undoCommand = offCommands[no];

	}
	
	// 按下撤销按钮
	public void undoButtonWasPushed() {
		undoCommand.undo();
	}

}


package com.yxj.command;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		//使用命令设计模式，完成通过遥控器，对电灯的操作
		
		//创建电灯的对象(接受者)
		LightReceiver lightReceiver = new LightReceiver();
		
		//创建电灯相关的开关命令
		LightOnCommand lightOnCommand = new LightOnCommand(lightReceiver);
		LightOffCommand lightOffCommand = new LightOffCommand(lightReceiver);
		
		//需要一个遥控器
		RemoteController remoteController = new RemoteController();
		
		//给我们的遥控器设置命令, 比如 no = 0 是电灯的开和关的操作
		remoteController.setCommand(0, lightOnCommand, lightOffCommand);
		
		System.out.println("--------按下灯的开按钮-----------");
		remoteController.onButtonWasPushed(0);
		System.out.println("--------按下灯的关按钮-----------");
		remoteController.offButtonWasPushed(0);
		System.out.println("--------按下撤销按钮-----------");
		remoteController.undoButtonWasPushed();
		
		
		System.out.println("=========使用遥控器操作电视机==========");
		
		TVReceiver tvReceiver = new TVReceiver();
		
		TVOffCommand tvOffCommand = new TVOffCommand(tvReceiver);
		TVOnCommand tvOnCommand = new TVOnCommand(tvReceiver);
		
		//给我们的遥控器设置命令, 比如 no = 1 是电视机的开和关的操作
		remoteController.setCommand(1, tvOnCommand, tvOffCommand);
		
		System.out.println("--------按下电视机的开按钮-----------");
		remoteController.onButtonWasPushed(1);
		System.out.println("--------按下电视机的关按钮-----------");
		remoteController.offButtonWasPushed(1);
		System.out.println("--------按下撤销按钮-----------");
		remoteController.undoButtonWasPushed();

	}

}


package com.yxj.command;

public class TVOffCommand implements Command {

	// 聚合TVReceiver

	TVReceiver tv;

	// 构造器
	public TVOffCommand(TVReceiver tv) {
		super();
		this.tv = tv;
	}

	@Override
	public void execute() {
		// TODO Auto-generated method stub
		// 调用接收者的方法
		tv.off();
	}

	@Override
	public void undo() {
		// TODO Auto-generated method stub
		// 调用接收者的方法
		tv.on();
	}
}


package com.yxj.command;

public class TVReceiver {
	
	public void on() {
		System.out.println(" 电视机打开了.. ");
	}
	
	public void off() {
		System.out.println(" 电视机关闭了.. ");
	}
}



package com.yxj.command;

public class TVOnCommand implements Command {

	// 聚合TVReceiver

	TVReceiver tv;

	// 构造器
	public TVOnCommand(TVReceiver tv) {
		super();
		this.tv = tv;
	}

	@Override
	public void execute() {
		// TODO Auto-generated method stub
		// 调用接收者的方法
		tv.on();
	}

	@Override
	public void undo() {
		// TODO Auto-generated method stub
		// 调用接收者的方法
		tv.off();
	}
}


```
###  1.10.5  命令模式在Spring框架JdbcTemplate应用的源码分析
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125075108272.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
3) 模式角色分析说明
StatementCallback 接口 ,类似命令接口(Command)
class QueryStatementCallback implements StatementCallback<T>, SqlProvider , 匿名内
部类， 实现了命令接口， 同时也充当命令接收者
命令调用者 是 JdbcTemplate , 其中execute(StatementCallback<T> action) 方法中，调
用action.doInStatement 方法. 不同的 实现 StatementCallback 接口的对象，对应不同
的doInStatemnt 实现逻辑
另外实现 StatementCallback 命令接口的子类还有 QueryStatementCallback、
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125075214804.png#pic_center)
###  1.10.6  命令模式的注意事项和细节
```markdown
1) 将发起请求的对象与执行请求的对象解耦。发起请求的对象是调
用者，调用者只要调用命令对象的execute()方法就可以让接收者
工作，而不必知道具体的接收者对象是谁、是如何实现的，命令
对象会负责让接收者执行请求的动作，也就是说：”请求发起
者”和“请求执行者”之间的解耦是通过命令对象实现的，命令对象起到
了纽带桥梁的作用。
2) 容易设计一个命令队列。只要把命令对象放到列队，就可以
多线程的执行命令
3) 容易实现对请求的撤销和重做
4) 命令模式不足：可能导致某些系统有过多的具体命令类，增
加了系统的复杂度，这点在在使用的时候要注意
5) 空命令也是一种设计模式，它为我们省去了判空的操作。在
上面的实例中，如果没有用空命令，我们每按下一个按键
都要判空，这给我们编码带来一定的麻烦。
6) 命令模式经典的应用场景：界面的一个按钮都是一
条命令、模拟CMD（DOS命令）
订单的撤销/恢复、触发-反馈机制
```
## 1.11 访问者模式
###  1.11.1 测评系统的需求
```markdown
完成测评系统需求
1) 将观众分为男人和女人，对歌手进行测评，当看完某个歌
手表演后，得到他们对该歌手不同的评价(评价 有不同的种
类，比如 成功、失败 等)
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125075705347.png#pic_center)
###  1.11.2 传统方式的问题分析
```markdown
1) 如果系统比较小，还是ok的，但是考虑系统增加越来越多新的功
能时，对代码改动较大，违反了ocp原则， 不利于维护
2) 扩展性不好，比如 增加了 新的人员类型，或者管理方法，都不
好做
3) 引出我们会使用新的设计模式 – 访问者模式
```
###  1.11.3 访问者模式基本介绍
```markdown
1) 访问者模式（Visitor Pattern），封装一些作用于某种数
据结构的各元素的操作，
它可以在不改变数据结构的前提下定义作用于这些元素的新的操作。
2) 主要将数据结构与数据操作分离，解决 数据结构和操作耦合性问题
3) 访问者模式的基本工作原理是：在被访问的类里面加一个对
外提供接待访问者的接口
4) 访问者模式主要应用场景是：需要对一个对象结构中的对象进行很
多不同操作(这些操作彼此没有关联)，同时需要避免让这些操
作"污染"这些对象的类，可以选用访问者模式解决
```
###  1.11.4 访问者模式的原理类图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125075952971.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
1) Visitor 是抽象访问者，为该对象结构中的ConcreteElement
的每一个类声明一个visit操作
2) ConcreteVisitor ：是一个具体的访问值 实现每个
有Visitor 声明的操作，是每个操作实现的部分.
3) ObjectStructure 能枚举它的元素， 可以提供一个高层的接
口，用来允许访问者访问元素
4) Element 定义一个accept 方法，接收一个访问者对象
5) ConcreteElement 为具体元素，实现了accept 方法
```
###  1.11.5 访问者模式应用实例
```markdown
将人分为男人和女人，对歌手进行测评，当看完某个歌手表演后，得
到他们对该歌手不同的评价(评价有不同的种类，比如成功、失败等)，
请使用访问者模式来说实现
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125080153128.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.visitor;

public abstract class Action {
	
	//得到男性 的测评
	public abstract void getManResult(Man man);
	
	//得到女的 测评
	public abstract void getWomanResult(Woman woman);
}


package com.yxj.visitor;

public class Success extends Action {

	@Override
	public void getManResult(Man man) {
		// TODO Auto-generated method stub
		System.out.println(" 男人给的评价该歌手很成功 !");
	}

	@Override
	public void getWomanResult(Woman woman) {
		// TODO Auto-generated method stub
		System.out.println(" 女人给的评价该歌手很成功 !");
	}

}


package com.yxj.visitor;

public class Fail extends Action {

	@Override
	public void getManResult(Man man) {
		// TODO Auto-generated method stub
		System.out.println(" 男人给的评价该歌手失败 !");
	}

	@Override
	public void getWomanResult(Woman woman) {
		// TODO Auto-generated method stub
		System.out.println(" 女人给的评价该歌手失败 !");
	}

}


package com.yxj.visitor;

public abstract class Person {
	
	//提供一个方法，让访问者可以访问
	public abstract void accept(Action action);
}


package com.yxj.visitor;

public class Man extends Person {

	@Override
	public void accept(Action action) {
		// TODO Auto-generated method stub
		action.getManResult(this);
	}

}



package com.yxj.visitor;

//说明
//1. 这里我们使用到了双分派, 即首先在客户端程序中，将具体状态作为参数传递Woman中(第一次分派)
//2. 然后Woman 类调用作为参数的 "具体方法" 中方法getWomanResult, 同时将自己(this)作为参数
//   传入，完成第二次的分派
public class Woman extends Person{

	@Override
	public void accept(Action action) {
		// TODO Auto-generated method stub
		action.getWomanResult(this);
	}

}


package com.yxj.visitor;

import java.util.LinkedList;
import java.util.List;

//数据结构，管理很多人（Man , Woman）
public class ObjectStructure {

	//维护了一个集合
	private List<Person> persons = new LinkedList<>();
	
	//增加到list
	public void attach(Person p) {
		persons.add(p);
	}
	//移除
	public void detach(Person p) {
		persons.remove(p);
	}
	
	//显示测评情况
	public void display(Action action) {
		for(Person p: persons) {
			p.accept(action);
		}
	}
}


package com.yxj.visitor;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//创建ObjectStructure
		ObjectStructure objectStructure = new ObjectStructure();
		
		objectStructure.attach(new Man());
		objectStructure.attach(new Woman());
		
		
		//成功
		Success success = new Success();
		objectStructure.display(success);
		
		System.out.println("===============");
		Fail fail = new Fail();
		objectStructure.display(fail);
		
		System.out.println("=======给的是待定的测评========");
		
		Wait wait = new Wait();
		objectStructure.display(wait);
	}

}


package com.yxj.visitor;

public class Wait extends Action {

	@Override
	public void getManResult(Man man) {
		// TODO Auto-generated method stub
		System.out.println(" 男人给的评价是该歌手待定 ..");
	}

	@Override
	public void getWomanResult(Woman woman) {
		// TODO Auto-generated method stub
		System.out.println(" 女人给的评价是该歌手待定 ..");
	}

}

```

```markdown
上面提到了双分派，所谓双分派是指不管类怎么变化，我们都能找到
期望的方法运行。双分派意味着得到执行的操作取决于请求的种类
和两个接收者的类型
以上述实例为例，假设我们要添加一个Wait的状态类，考察
Man类和Woman类的反应，由于使用了双分派，只需增加一
个Action子类即可在客户端调用即可，不
需要改动任何其他类的代码。
```
###  1.11.6 访问者模式的注意事项和细节
```markdown
 优点
1) 访问者模式符合单一职责原则、让程序具有优秀的扩展性、灵活
性非常高
2) 访问者模式可以对功能进行统一，可以做报表、UI、拦截器与
过滤器，适用于数据结构相对稳定的系统
 缺点
1) 具体元素对访问者公布细节，也就是说访问者关注了其他类
的内部细节，这是迪米
特法则所不建议的, 这样造成了具体元素变更比较困难
2) 违背了依赖倒转原则。访问者依赖的是具体元素，而不
是抽象元素
3) 因此，如果一个系统有比较稳定的数据结构，又有经常变
化的功能需求，那么访问者模式就是比较合适的.
```
##  1.12 迭代器模式
```markdown
编写程序展示一个学校院系结构：需求是这样，要在一个页面中
展示出学校的院系组成，一个学校有多个学院，一个学院有多
个系。如图：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125083551224.png#pic_center)
###  1.12.1 传统的设计方案
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125083655734.png#pic_center)
###  1.12.2 传统的方式的问题分析
```markdown
1) 将学院看做是学校的子类，系是学院的子类，这样实际上是站在
组织大小来进行分层次的
2) 实际上我们的要求是 ：在一个页面中展示出学校的院系组成，一
个学校有多个学院，一个学院有多个系， 因此这种方案，不能很好实
现的遍历的操作
3) 解决方案：=> 迭代器模式
```
###  1.12.3 迭代器模式基本介绍
```markdown
1) 迭代器模式（Iterator Pattern）是常用的设计模式，属于
行为型模式
2) 如果我们的集合元素是用不同的方式实现的，有数组，还有
java的集合类，或者还有其他方式，当客户端要遍历这些集合
元素的时候就要使用多种遍历方式，而且还会暴露元素的内
部结构，可以考虑使用迭代器模式解决。
3) 迭代器模式，提供一种遍历集合元素的统一接口，用
一致的方法遍历集合元素，不需要知道集合对象的底层
表示，即：不暴露其内部的结构。
```
###  1.12.4 迭代器模式的原理类图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125083923601.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
1) Iterator：迭代器接口，是系统提供，含义 
hasnext,next, remove
2) Concretelterator：具体的迭代器类，管理迭代
3) Aggregate：一个统一的聚合接口，将客户端和具体聚合解耦
4) Concreteaggreage：具体的聚合持有对象集合，并提供一个
方法，返回一个迭代器，该迭代器可以正确遍历集合
5) Client：客户端，通过 Iterator和 Aggregate依赖子类
```
###  1.12.5 迭代器模式应用实例
```markdown
编写程序展示一个学校院系结构：需求是这样，要在一个页面中
展示出学校的院系组成，一个学校有多个学院，一个学院有多个系。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125084647970.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.iterator;

//系
public class Department {

	private String name;
	private String desc;
	public Department(String name, String desc) {
		super();
		this.name = name;
		this.desc = desc;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getDesc() {
		return desc;
	}
	public void setDesc(String desc) {
		this.desc = desc;
	}
	
	
	
}



package com.yxj.iterator;

import java.util.Iterator;


public class ComputerCollegeIterator implements Iterator {

	//这里我们需要Department 是以怎样的方式存放=>数组
	Department[] departments;
	int position = 0; //遍历的位置
	
	
	
	
	public ComputerCollegeIterator(Department[] departments) {
		this.departments = departments;
	}

	//判断是否还有下一个元素
	@Override
	public boolean hasNext() {
		// TODO Auto-generated method stub
		if(position >= departments.length || departments[position] == null) {
			return false;
		}else {
		
			return true;
		}
	}

	@Override
	public Object next() {
		// TODO Auto-generated method stub
		Department department = departments[position];
		position += 1;
		return department;
	}
	
	//删除的方法，默认空实现
	public void remove() {
		
	}

}


package com.yxj.iterator;

import java.util.Iterator;
import java.util.List;

public class InfoColleageIterator implements Iterator {

	
	List<Department> departmentList; // 信息工程学院是以List方式存放系
	int index = -1;//索引
	

	public InfoColleageIterator(List<Department> departmentList) {
		this.departmentList = departmentList;
	}

	//判断list中还有没有下一个元素
	@Override
	public boolean hasNext() {
		// TODO Auto-generated method stub
		if(index >= departmentList.size() - 1) {
			return false;
		} else {
			index += 1;
			return true;
		}
	}

	@Override
	public Object next() {
		// TODO Auto-generated method stub
		return departmentList.get(index);
	}
	
	//空实现remove
	public void remove() {
		
	}

}


package com.yxj.iterator;

import java.util.Iterator;

public interface College {
	
	public String getName();
	
	//增加系的方法
	public void addDepartment(String name, String desc);
	
	//返回一个迭代器,遍历
	public Iterator  createIterator();
}



package com.yxj.iterator;

import java.util.Iterator;

public class ComputerCollege implements College {

	Department[] departments;
	int numOfDepartment = 0 ;// 保存当前数组的对象个数
	
	
	public ComputerCollege() {
		departments = new Department[5];
		addDepartment("Java专业", " Java专业 ");
		addDepartment("PHP专业", " PHP专业 ");
		addDepartment("大数据专业", " 大数据专业 ");
		
	}
	
	
	@Override
	public String getName() {
		// TODO Auto-generated method stub
		return "计算机学院";
	}

	@Override
	public void addDepartment(String name, String desc) {
		// TODO Auto-generated method stub
		Department department = new Department(name, desc);
		departments[numOfDepartment] = department;
		numOfDepartment += 1;
	}

	@Override
	public Iterator createIterator() {
		// TODO Auto-generated method stub
		return new ComputerCollegeIterator(departments);
	}

}


package com.yxj.iterator;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class InfoCollege implements College {

	List<Department> departmentList;
	
	
	public InfoCollege() {
		departmentList = new ArrayList<Department>();
		addDepartment("信息安全专业", " 信息安全专业 ");
		addDepartment("网络安全专业", " 网络安全专业 ");
		addDepartment("服务器安全专业", " 服务器安全专业 ");
	}
	
	@Override
	public String getName() {
		// TODO Auto-generated method stub
		return "信息工程学院";
	}

	@Override
	public void addDepartment(String name, String desc) {
		// TODO Auto-generated method stub
		Department department = new Department(name, desc);
		departmentList.add(department);
	}

	@Override
	public Iterator createIterator() {
		// TODO Auto-generated method stub
		return new InfoColleageIterator(departmentList);
	}

}



package com.yxj.iterator;

import java.util.Iterator;
import java.util.List;

public class OutPutImpl {
	
	//学院集合
	List<College> collegeList;

	public OutPutImpl(List<College> collegeList) {
		
		this.collegeList = collegeList;
	}
	//遍历所有学院,然后调用printDepartment 输出各个学院的系
	public void printCollege() {
		
		//从collegeList 取出所有学院, Java 中的 List 已经实现Iterator
		Iterator<College> iterator = collegeList.iterator();
		
		while(iterator.hasNext()) {
			//取出一个学院
			College college = iterator.next();
			System.out.println("=== "+college.getName() +"=====" );
			printDepartment(college.createIterator()); //得到对应迭代器
		}
	}
	
	
	//输出 学院输出 系
	
	public void printDepartment(Iterator iterator) {
		while(iterator.hasNext()) {
			Department d = (Department)iterator.next();
			System.out.println(d.getName());
		}
	}
	
}


package com.yxj.iterator;

import java.util.ArrayList;
import java.util.List;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//创建学院
		List<College> collegeList = new ArrayList<College>();
		
		ComputerCollege computerCollege = new ComputerCollege();
		InfoCollege infoCollege = new InfoCollege();
		
		collegeList.add(computerCollege);
		//collegeList.add(infoCollege);
		
		OutPutImpl outPutImpl = new OutPutImpl(collegeList);
		outPutImpl.printCollege();
	}

}

```
###  1.12.6 迭代器模式在JDK-ArrayList集合应用的源码分析
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125092302413.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125092319735.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
内部类Itr 充当具体实现迭代器Iterator 的类， 作为ArrayList 
内部类
List 就是充当了聚合接口，含有一个iterator() 方法，返回
一个迭代器对象
ArrayList 是实现聚合接口List 的子类，实现了iterator()
Iterator 接口系统提供
迭代器模式解决了 不同集合(ArrayList ,LinkedList) 统一
遍历问题
```
###  1.12.7 迭代器模式的注意事项和细节
```markdown
优点
1) 提供一个统一的方法遍历对象，客户不用再考虑聚合的类型，使
用一种方法就可以遍历对象了。
2) 隐藏了聚合的内部结构，客户端要遍历聚合的时候只能取到迭代
器，而不会知道聚合的具体组成。
3) 提供了一种设计思想，就是一个类应该只有一个引起变化的原
因（叫做单一责任原则）。在聚合类中，我们把迭代器分开，就
是要把管理对象集合和遍历对象集合的责任分开，这样一来集合改
变的话，只影响到聚合对象。而如果遍历方式改变的话，只影响到了
迭代器。
4) 当要展示一组相似对象，或者遍历一组相同对象时使用,
适合使用迭代器模式

缺点
每个聚合对象都要一个迭代器，会生成多个迭代器不好管理类
```
##  1.13 观察者模式
###  1.13.1 天气预报项目需求
```markdown
1) 气象站可以将每天测量到的温度，湿度，气压等等以公告的
形式发布出去(比如发布到自己的网站或第三方)。
2) 需要设计开放型API，便于其他第三方也能接入气象站获取数据。
3) 提供温度、气压和湿度的接口
4) 测量数据更新时，要能实时的通知给第三方
```
###  1.13.2 天气预报设计方案1-普通方案
```markdown
通过对气象站项目的分析，我们可以初步设计出一个WeatherData类
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125123552355.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
1) 通过getXxx方法，可以让第三方接入，并得到相关信息.
2) 当数据有更新时，气象站通过调用dataChange() 去更新数据，
当第三方再次获取时，就能得到最新数据，当然也可以推送。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125123627671.png#pic_center)

```markdown
CurrentConditions(当前的天气情况)
可以理解成是我们气象局的网站 //推送
```

```java
package com.yxj.observer;

/**
 * 显示当前天气情况（可以理解成是气象站自己的网站）
 * @author Administrator
 *
 */
public class CurrentConditions {
	// 温度，气压，湿度
	private float temperature;
	private float pressure;
	private float humidity;

	//更新 天气情况，是由 WeatherData 来调用，我使用推送模式
	public void update(float temperature, float pressure, float humidity) {
		this.temperature = temperature;
		this.pressure = pressure;
		this.humidity = humidity;
		display();
	}

	//显示
	public void display() {
		System.out.println("***Today mTemperature: " + temperature + "***");
		System.out.println("***Today mPressure: " + pressure + "***");
		System.out.println("***Today mHumidity: " + humidity + "***");
	}
}




package com.yxj.observer;

/**
 * 类是核心
 * 1. 包含最新的天气情况信息 
 * 2. 含有 CurrentConditions 对象
 * 3. 当数据有更新时，就主动的调用   CurrentConditions对象update方法(含 display), 这样他们（接入方）就看到最新的信息
 * @author Administrator
 *
 */
public class WeatherData {
	private float temperatrue;
	private float pressure;
	private float humidity;
	private CurrentConditions currentConditions;
	//加入新的第三方

	public WeatherData(CurrentConditions currentConditions) {
		this.currentConditions = currentConditions;
	}

	public float getTemperature() {
		return temperatrue;
	}

	public float getPressure() {
		return pressure;
	}

	public float getHumidity() {
		return humidity;
	}

	public void dataChange() {
		//调用 接入方的 update
		currentConditions.update(getTemperature(), getPressure(), getHumidity());
	}

	//当数据有更新时，就调用 setData
	public void setData(float temperature, float pressure, float humidity) {
		this.temperatrue = temperature;
		this.pressure = pressure;
		this.humidity = humidity;
		//调用dataChange， 将最新的信息 推送给 接入方 currentConditions
		dataChange();
	}
}


package com.yxj.observer;

public class Client {
	public static void main(String[] args) {
		//创建接入方 currentConditions
		CurrentConditions currentConditions = new CurrentConditions();
		//创建 WeatherData 并将 接入方 currentConditions 传递到 WeatherData中
		WeatherData weatherData = new WeatherData(currentConditions);
		
		//更新天气情况
		weatherData.setData(30, 150, 40);
		
		//天气情况变化
		System.out.println("============天气情况变化=============");
		weatherData.setData(40, 160, 20);
		

	}
}

```
```markdown
1) 其他第三方接入气象站获取数据的问题
2) 无法在运行时动态的添加第三方 (新浪网站)
3) 违反ocp原则=>观察者模式
//在WeatherData中，当增加一个第三方，都需要创建一个对应
的第三方的公告板对象，并加入到dataChange, 不利于维护，也
不是动态加入
public void dataChange() {
currentConditions.update(getTemperature(), getPressure(), getHumidity());
}
```
###  1.13.3 观察者模式(Observer)原理

```markdown
 观察者模式类似订牛奶业务
1) 奶站/气象局：Subject
2) 用户/第三方网站：Observer
Subject：登记注册、移除和通知
1) registerObserver 注册
2) removeObserver 移除
3) notifyObservers() 通知所有的注册的用户，根据
不同需求，可以是更新数据，让用户来取，也可能是实施推送，
看具体需求定

Observer：接收输入
观察者模式：对象之间多对一依赖的一种设计方案，被依
赖的对象为Subject，依赖的对象为Observer，Subject通
知Observer变化,比如这里的奶站是
Subject，是1的一方。用户时Observer，是多的一方。
```
###  1.13.4 观察者模式解决天气预报需求
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125141313300.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.observer.improve;

//接口, 让WeatherData 来实现 
public interface Subject {
	
	public void registerObserver(Observer o);
	public void removeObserver(Observer o);
	public void notifyObservers();
}


package com.yxj.observer.improve;

//观察者接口，有观察者来实现
public interface Observer {

	public void update(float temperature, float pressure, float humidity);
}


package com.yxj.observer.improve;

public class CurrentConditions implements Observer {

	// 温度，气压，湿度
	private float temperature;
	private float pressure;
	private float humidity;

	// 更新 天气情况，是由 WeatherData 来调用，我使用推送模式
	public void update(float temperature, float pressure, float humidity) {
		this.temperature = temperature;
		this.pressure = pressure;
		this.humidity = humidity;
		display();
	}

	// 显示
	public void display() {
		System.out.println("***Today mTemperature: " + temperature + "***");
		System.out.println("***Today mPressure: " + pressure + "***");
		System.out.println("***Today mHumidity: " + humidity + "***");
	}
}


package com.yxj.observer.improve;

import java.util.ArrayList;

/**
 * 类是核心
 * 1. 包含最新的天气情况信息 
 * 2. 含有 观察者集合，使用ArrayList管理
 * 3. 当数据有更新时，就主动的调用   ArrayList, 通知所有的（接入方）就看到最新的信息
 * @author Administrator
 *
 */
public class WeatherData implements Subject {
	private float temperatrue;
	private float pressure;
	private float humidity;
	//观察者集合
	private ArrayList<Observer> observers;
	
	//加入新的第三方

	public WeatherData() {
		observers = new ArrayList<Observer>();
	}

	public float getTemperature() {
		return temperatrue;
	}

	public float getPressure() {
		return pressure;
	}

	public float getHumidity() {
		return humidity;
	}

	public void dataChange() {
		//调用 接入方的 update
		
		notifyObservers();
	}

	//当数据有更新时，就调用 setData
	public void setData(float temperature, float pressure, float humidity) {
		this.temperatrue = temperature;
		this.pressure = pressure;
		this.humidity = humidity;
		//调用dataChange， 将最新的信息 推送给 接入方 currentConditions
		dataChange();
	}

	//注册一个观察者
	@Override
	public void registerObserver(Observer o) {
		// TODO Auto-generated method stub
		observers.add(o);
	}

	//移除一个观察者
	@Override
	public void removeObserver(Observer o) {
		// TODO Auto-generated method stub
		if(observers.contains(o)) {
			observers.remove(o);
		}
	}

	//遍历所有的观察者，并通知
	@Override
	public void notifyObservers() {
		// TODO Auto-generated method stub
		for(int i = 0; i < observers.size(); i++) {
			observers.get(i).update(this.temperatrue, this.pressure, this.humidity);
		}
	}
}



package com.yxj.observer.improve;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//创建一个WeatherData
		WeatherData weatherData = new WeatherData();
		
		//创建观察者
		CurrentConditions currentConditions = new CurrentConditions();
		BaiduSite baiduSite = new BaiduSite();
		
		//注册到weatherData
		weatherData.registerObserver(currentConditions);
		weatherData.registerObserver(baiduSite);
		
		//测试
		System.out.println("通知各个注册的观察者, 看看信息");
		weatherData.setData(10f, 100f, 30.3f);
		
		
		weatherData.removeObserver(currentConditions);
		//测试
		System.out.println();
		System.out.println("通知各个注册的观察者, 看看信息");
		weatherData.setData(10f, 100f, 30.3f);
	}

}



package com.yxj.observer.improve;

public class BaiduSite implements Observer {

	// 温度，气压，湿度
	private float temperature;
	private float pressure;
	private float humidity;

	// 更新 天气情况，是由 WeatherData 来调用，我使用推送模式
	public void update(float temperature, float pressure, float humidity) {
		this.temperature = temperature;
		this.pressure = pressure;
		this.humidity = humidity;
		display();
	}

	// 显示
	public void display() {
		System.out.println("===百度网站====");
		System.out.println("***百度网站 气温 : " + temperature + "***");
		System.out.println("***百度网站 气压: " + pressure + "***");
		System.out.println("***百度网站 湿度: " + humidity + "***");
	}

}

```
###  1.13.5 观察者模式的好处
```markdown
1) 观察者模式设计后，会以集合的方式来管理用户(Observer)，
包括注册，移除和通知。
2) 这样，我们增加观察者(这里可以理解成一个新的公告板)，就
不需要去修改核心类WeatherData不会修改代码，遵守了ocp原则。
```
###  1.13.6 观察者模式在Jdk应用的源码分析
```markdown
1) Jdk的Observable类就使用了观察者模式
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125142412481.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
Observable 的作用和地位等价于 我们前面讲过Subject
Observable 是类，不是接口，类中已经实现了核心的方法 ,
即管理Observer的方法 add.. delete .. notify...
Observer 的作用和地位等价于我们前面讲过的 Observer, 
有update
Observable 和 Observer 的使用方法和前面讲过的一样，
只是Observable 是类，通过继承来实现观察者模式
```
##  1.14 中介者模式
###  1.14.1 智能家庭项目
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125142740201.png#pic_center)

```markdown
智能家庭项目：
1) 智能家庭包括各种设备，闹钟、咖啡机、电视机、窗帘 等
2) 主人要看电视时，各个设备可以协同工作，自动完成看电
视的准备工作，比如流程为：
闹铃响起->咖啡机开始做咖啡->窗帘自动落下->电视机开始播放
```
###  1.14.2 传统方案解决智能家庭管理问题
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125143016582.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
###  1.14.3  传统的方式的问题分析
```markdown
1) 当各电器对象有多种状态改变时，相互之间的调用关系会比较复杂
2) 各个电器对象彼此联系，你中有我，我中有你，不利于松耦合.
3) 各个电器对象之间所传递的消息(参数)，容易混乱
4) 当系统增加一个新的电器对象时，或者执行流程改变时，代码的
可维护性、扩展性都不理想  考虑中介者模式
```
###  1.14.4 中介者模式基本介绍
```markdown
1) 中介者模式（Mediator Pattern），用一个中介对象来封装一
系列的对象交互。中介者使各个对象不需要显式地相互引用，
从而使其耦合松散，而且可以独立地改变它们之间的交互
2) 中介者模式属于行为型模式，使代码易于维护
3) 比如MVC模式，C（Controller控制器）是M（Model模型）和
V（View视图）的中介者，在前后端交互时起到了中间人的作用
```
###  1.14.5 中介者模式原理类图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125143411581.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
1) Mediator就是抽象中介者，定义了同事对象到中介者对象的接口
2) Colleague是抽象同事类
3) Concretemediator具体的中介者对象，实现抽象方法，他需要
知道所有的具体的同事类，即以一个集合来管理Hashmap，并接受
某个同事对象消息，完成相应的任务
4) ConcreteColleague具体的同事类，会有很多，每个同事只
知道自己的行为，而不了解其他同事类的行为（方法）,但是他
们都依赖中介者对象
```
###  1.14.6 中介者模式应用实例-智能家庭管理
```markdown
完成前面的智能家庭的项目，使用中介者模式
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125143923517.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.mediator.smarthouse;

//同事抽象类
public abstract class Colleague {
	private Mediator mediator;
	public String name;

	public Colleague(Mediator mediator, String name) {

		this.mediator = mediator;
		this.name = name;

	}

	public Mediator GetMediator() {
		return this.mediator;
	}

	public abstract void SendMessage(int stateChange);
}



package com.yxj.mediator.smarthouse;

//具体的同事类
public class Alarm extends Colleague {

	//构造器
	public Alarm(Mediator mediator, String name) {
		super(mediator, name);
		// TODO Auto-generated constructor stub
		//在创建Alarm 同事对象时，将自己放入到ConcreteMediator 对象中[集合]
		mediator.Register(name, this);
	}

	public void SendAlarm(int stateChange) {
		SendMessage(stateChange);
	}

	@Override
	public void SendMessage(int stateChange) {
		// TODO Auto-generated method stub
		//调用的中介者对象的getMessage
		this.GetMediator().GetMessage(stateChange, this.name);
	}

}


package com.yxj.mediator.smarthouse;

public abstract class Mediator {
	//将给中介者对象，加入到集合中
	public abstract void Register(String colleagueName, Colleague colleague);

	//接收消息, 具体的同事对象发出
	public abstract void GetMessage(int stateChange, String colleagueName);

	public abstract void SendMessage();
}



package com.yxj.mediator.smarthouse;

import java.util.HashMap;

//具体的中介者类
public class ConcreteMediator extends Mediator {
	//集合，放入所有的同事对象
	private HashMap<String, Colleague> colleagueMap;
	private HashMap<String, String> interMap;

	public ConcreteMediator() {
		colleagueMap = new HashMap<String, Colleague>();
		interMap = new HashMap<String, String>();
	}

	@Override
	public void Register(String colleagueName, Colleague colleague) {
		// TODO Auto-generated method stub
		colleagueMap.put(colleagueName, colleague);

		// TODO Auto-generated method stub

		if (colleague instanceof Alarm) {
			interMap.put("Alarm", colleagueName);
		} else if (colleague instanceof CoffeeMachine) {
			interMap.put("CoffeeMachine", colleagueName);
		} else if (colleague instanceof TV) {
			interMap.put("TV", colleagueName);
		} else if (colleague instanceof Curtains) {
			interMap.put("Curtains", colleagueName);
		}

	}

	//具体中介者的核心方法
	//1. 根据得到消息，完成对应任务
	//2. 中介者在这个方法，协调各个具体的同事对象，完成任务
	@Override
	public void GetMessage(int stateChange, String colleagueName) {
		// TODO Auto-generated method stub

		//处理闹钟发出的消息
		if (colleagueMap.get(colleagueName) instanceof Alarm) {
			if (stateChange == 0) {
				((CoffeeMachine) (colleagueMap.get(interMap
						.get("CoffeeMachine")))).StartCoffee();
				((TV) (colleagueMap.get(interMap.get("TV")))).StartTv();
			} else if (stateChange == 1) {
				((TV) (colleagueMap.get(interMap.get("TV")))).StopTv();
			}

		} else if (colleagueMap.get(colleagueName) instanceof CoffeeMachine) {
			((Curtains) (colleagueMap.get(interMap.get("Curtains"))))
					.UpCurtains();

		} else if (colleagueMap.get(colleagueName) instanceof TV) {//如果TV发现消息

		} else if (colleagueMap.get(colleagueName) instanceof Curtains) {
			//如果是以窗帘发出的消息，这里处理...
		}

	}

	@Override
	public void SendMessage() {
		// TODO Auto-generated method stub

	}

}

package com.yxj.mediator.smarthouse;

public class Curtains extends Colleague {

	public Curtains(Mediator mediator, String name) {
		super(mediator, name);
		// TODO Auto-generated constructor stub
		mediator.Register(name, this);
	}

	@Override
	public void SendMessage(int stateChange) {
		// TODO Auto-generated method stub
		this.GetMediator().GetMessage(stateChange, this.name);
	}

	public void UpCurtains() {
		System.out.println("I am holding Up Curtains!");
	}

}


package com.yxj.mediator.smarthouse;

public class TV extends Colleague {

	public TV(Mediator mediator, String name) {
		super(mediator, name);
		// TODO Auto-generated constructor stub
		mediator.Register(name, this);
	}

	@Override
	public void SendMessage(int stateChange) {
		// TODO Auto-generated method stub
		this.GetMediator().GetMessage(stateChange, this.name);
	}

	public void StartTv() {
		// TODO Auto-generated method stub
		System.out.println("It's time to StartTv!");
	}

	public void StopTv() {
		// TODO Auto-generated method stub
		System.out.println("StopTv!");
	}
}



package com.yxj.mediator.smarthouse;

public class CoffeeMachine extends Colleague {

	public CoffeeMachine(Mediator mediator, String name) {
		super(mediator, name);
		// TODO Auto-generated constructor stub
		mediator.Register(name, this);
	}

	@Override
	public void SendMessage(int stateChange) {
		// TODO Auto-generated method stub
		this.GetMediator().GetMessage(stateChange, this.name);
	}

	public void StartCoffee() {
		System.out.println("It's time to startcoffee!");
	}

	public void FinishCoffee() {

		System.out.println("After 5 minutes!");
		System.out.println("Coffee is ok!");
		SendMessage(0);
	}
}




package com.yxj.mediator.smarthouse;

public class ClientTest {

	public static void main(String[] args) {
		//创建一个中介者对象
		Mediator mediator = new ConcreteMediator();
		
		//创建Alarm 并且加入到  ConcreteMediator 对象的HashMap
		Alarm alarm = new Alarm(mediator, "alarm");
		
		//创建了CoffeeMachine 对象，并  且加入到  ConcreteMediator 对象的HashMap
		CoffeeMachine coffeeMachine = new CoffeeMachine(mediator,
				"coffeeMachine");
		
		//创建 Curtains , 并  且加入到  ConcreteMediator 对象的HashMap
		Curtains curtains = new Curtains(mediator, "curtains");
		TV tV = new TV(mediator, "TV");
		
		//让闹钟发出消息
		alarm.SendAlarm(0);
		coffeeMachine.FinishCoffee();
		alarm.SendAlarm(1);
	}

}


```
###  1.14.7 中介者模式的注意事项和细节
```markdown
1) 多个类相互耦合，会形成网状结构, 使用中介者模式将网状
结构分离为星型结构，进行解耦
2) 减少类间依赖，降低了耦合，符合迪米特原则
3) 中介者承担了较多的责任，一旦中介者出现了问题，整个系
统就会受到影响
4) 如果设计不当，中介者对象本身变得过于复杂，这点在实
际使用时，要特别注意
```
##  1.15 备忘录模式
###  1.15.1 游戏角色状态恢复问题
```markdown
游戏角色有攻击力和防御力，在大战Boss前保存自身的
状态(攻击力和防御力)，当大战Boss后攻击力和防御力
下降，从备忘录对象恢复到大战前的状态
```
###  1.15.2 传统方案解决游戏角色恢复
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125145530422.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
###  1.15.3 传统的方式的问题分析
```markdown
1) 一个对象，就对应一个保存对象状态的对象， 这样当我们
游戏的对象很多时，不利于管理，开销也很大.
2) 传统的方式是简单地做备份，new出另外一个对象出来，再
把需要备份的数据放到这个新对象，但这就暴露了对象内部的细节
3) 解决方案： => 备忘录模式
```
###  1.15.4 备忘录模式基本介绍
```markdown
1) 备忘录模式（Memento Pattern）在不破坏封装性的前提下，捕
获一个对象的内部状态，并在该对象之外保存这个状态。这样以后
就可将该对象恢复到原先保存的状态
2) 可以这里理解备忘录模式：现实生活中的备忘录是用来记录
某些要去做的事情，或者是记录已经达成的共同意见的事情，
以防忘记了。而在软件层面，备忘录模式有着相同的含义，
备忘录对象主要用来记录一个对象的某种状态，或者某些
数据，当要做回退时，可以从备忘录对象里获取原来的数
据进行恢复操作
3) 备忘录模式属于行为型模式
```
###  1.15.5 备忘录模式的原理类图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125145722340.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
1) originator : 对象(需要保存状态的对象)
2) Memento ： 备忘录对象,负责
保存好记录，即Originator内部状态
3) Caretaker: 守护者对象,负责保存多个备忘录对象，
使用集合管理，提高效率
4) 说明：如果希望保存多个originator对象的不同时间的状态，
也可以，只需要要 HashMap <String, 集合> 
```
```java
package com.yxj.memento.theory;

public class Originator {

	private String state;//状态信息

	public String getState() {
		return state;
	}

	public void setState(String state) {
		this.state = state;
	}
	
	//编写一个方法，可以保存一个状态对象 Memento
	//因此编写一个方法，返回 Memento
	public Memento saveStateMemento() {
		return new Memento(state);
	}
	
	//通过备忘录对象，恢复状态
	public void getStateFromMemento(Memento memento) {
		state = memento.getState();
	}
}



package com.yxj.memento.theory;

public class Memento {
	private String state;

	//构造器
	public Memento(String state) {
		super();
		this.state = state;
	}

	public String getState() {
		return state;
	}
	
	
	
}



package com.yxj.memento.theory;

import java.util.ArrayList;
import java.util.List;

public class Caretaker {
	
	//在List 集合中会有很多的备忘录对象
	private List<Memento> mementoList = new ArrayList<Memento>();
	
	public void add(Memento memento) {
		mementoList.add(memento);
	}
	
	//获取到第index个Originator 的 备忘录对象(即保存状态)
	public Memento get(int index) {
		return mementoList.get(index);
	}
}


package com.yxj.memento.theory;

import java.util.ArrayList;
import java.util.HashMap;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		Originator originator = new Originator();
		Caretaker caretaker = new Caretaker();
		
		originator.setState(" 状态#1 攻击力 100 ");
		
		//保存了当前的状态
		caretaker.add(originator.saveStateMemento());
		
		originator.setState(" 状态#2 攻击力 80 ");
		
		caretaker.add(originator.saveStateMemento());
		
		originator.setState(" 状态#3 攻击力 50 ");
		caretaker.add(originator.saveStateMemento());
		
		
		
		System.out.println("当前的状态是 =" + originator.getState());
		
		//希望得到状态 1, 将 originator 恢复到状态1
		
		originator.getStateFromMemento(caretaker.get(0));
		System.out.println("恢复到状态1 , 当前的状态是");
		System.out.println("当前的状态是 =" + originator.getState());
		
		
		
	}

}

```
###  1.15.6 游戏角色恢复状态实例
```markdown
游戏角色有攻击力和防御力，在大战Boss前保存自身的状
态(攻击力和防御力)，当大战Boss后攻击力和防御力下
降，从备忘录对象恢复到大战前的状态
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125150921318.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.memento.game;

public class Memento {

	//攻击力
	private int vit;
	//防御力
	private int def;
	public Memento(int vit, int def) {
		super();
		this.vit = vit;
		this.def = def;
	}
	public int getVit() {
		return vit;
	}
	public void setVit(int vit) {
		this.vit = vit;
	}
	public int getDef() {
		return def;
	}
	public void setDef(int def) {
		this.def = def;
	}
			
}



package com.yxj.memento.game;

import java.util.ArrayList;
import java.util.HashMap;

//守护者对象, 保存游戏角色的状态
public class Caretaker {

	//如果只保存一次状态
	private Memento  memento;
	//对GameRole 保存多次状态
	//private ArrayList<Memento> mementos;
	//对多个游戏角色保存多个状态
	//private HashMap<String, ArrayList<Memento>> rolesMementos;

	public Memento getMemento() {
		return memento;
	}

	public void setMemento(Memento memento) {
		this.memento = memento;
	}
	
	
}



package com.yxj.memento.game;

public class GameRole {

	private int vit;
	private int def;
	
	//创建Memento ,即根据当前的状态得到Memento
	public Memento createMemento() {
		return new Memento(vit, def);
	}
	
	//从备忘录对象，恢复GameRole的状态
	public void recoverGameRoleFromMemento(Memento memento) {
		this.vit = memento.getVit();
		this.def = memento.getDef();
	}
	
	//显示当前游戏角色的状态
	public void display() {
		System.out.println("游戏角色当前的攻击力：" + this.vit + " 防御力: " + this.def);
	}

	public int getVit() {
		return vit;
	}

	public void setVit(int vit) {
		this.vit = vit;
	}

	public int getDef() {
		return def;
	}

	public void setDef(int def) {
		this.def = def;
	}
	
	
}


package com.yxj.memento.game;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//创建游戏角色
		GameRole gameRole = new GameRole();
		gameRole.setVit(100);
		gameRole.setDef(100);
		
		System.out.println("和boss大战前的状态");
		gameRole.display();
		
		//把当前状态保存caretaker
		Caretaker caretaker = new Caretaker();
		caretaker.setMemento(gameRole.createMemento());
		
		System.out.println("和boss大战~~~");
		gameRole.setDef(30);
		gameRole.setVit(30);
		
		gameRole.display();
		
		System.out.println("大战后，使用备忘录对象恢复到站前");
		
		gameRole.recoverGameRoleFromMemento(caretaker.getMemento());
		System.out.println("恢复后的状态");
		gameRole.display();
	}

}


```
###  1.15.7 备忘录模式的注意事项和细节
```markdown
1) 给用户提供了一种可以恢复状态的机制，可以使用户能够比
较方便地回到某个历史的状态
2) 实现了信息的封装，使得用户不需要关心状态的保存细节
3) 如果类的成员变量过多，势必会占用比较大的资源，而且
每一次保存都会消耗一定的内存, 这个需要注意
4) 适用的应用场景：
1、后悔药。 2、打游戏时的存档。 3、Windows 里的 ctri + z。
4、IE 中的后退。 4、数据库的事务管理
5) 为了节约内存，备忘录模式可以和原型模式配合使用
```
##  1.16 解释器模式
###  1.16.1 四则运算问题
```markdown
通过解释器模式来实现四则运算，如计算a+b-c的值，具体要求
1) 先输入表达式的形式，比如 a+b+c-d+e, 要求表达式的字母
不能重复
2) 在分别输入a ,b, c, d, e 的值
3) 最后求出结果：如图
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125152049823.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
###  1.16.2 传统方案解决四则运算问题分析
```markdown
1) 编写一个方法，接收表达式的形式，然后根据用户输入的数值
进行解析，得到结果
2) 问题分析：如果加入新的运算符，比如 * / ( 等等，不利于
扩展，另外让一个方法来
解析会造成程序结构混乱，不够清晰.
3) 解决方案：可以考虑使用解释器模式， 即： 
表达式 -> 解释器(可以有多种) -> 结果
```
###  1.16.3 解释器模式基本介绍
```markdown
1) 在编译原理中，一个算术表达式通过词法分析器形成词法单元，
而后这些词法单元再通过语法分析器构建语法分析树，最终形
成一颗抽象的语法分析树。这里的词法分析器和语法分析器都
可以看做是解释器
2) 解释器模式（Interpreter Pattern）：是指给定一个语
言(表达式)，定义它的文法的一种表示，并定义一个解释
器，使用该解释器来解释语言中的句子(表达式)
3) 应用场景
应用可以将一个需要解释执行的语言中的句子表示为一个抽象语法树
一些重复出现的问题可以用一种简单的语言来表达
一个简单语法需要解释的场景
4) 这样的例子还有，比如编译器、运算表达式计算、正则
表达式、机器人等
```
###  1.16.4 解释器模式的原理类图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125181300783.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
1) Context: 是环境角色,含有解释器之外的全局信息.
2) AbstractExpression: 抽象表达式， 声明一个抽象的解释
操作,这个方法为抽象语法树中所有的节点所
共享
3) TerminalExpression: 为终结符表达式, 实现与文法中的终
结符相关的解释操作
4) NonTermialExpression: 为非终结符表达式，为文法中的非
终结符实现解释操作.
5) 说明： 输入Context he TerminalExpression 信息
通过Client 输入即可
```
###  1.16.5 解释器模式来实现四则
```markdown
通过解释器模式来实现四则运算，如计算a+b-c的值
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125181504370.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.interpreter;

import java.util.HashMap;

/**
 * 抽象类表达式，通过HashMap 键值对, 可以获取到变量的值
 * 
 * @author Administrator
 *
 */
public abstract class Expression {
	// a + b - c
	// 解释公式和数值, key 就是公式(表达式) 参数[a,b,c], value就是就是具体值
	// HashMap {a=10, b=20}
	public abstract int interpreter(HashMap<String, Integer> var);
}



package com.yxj.interpreter;

import java.util.HashMap;


/**
 * 变量的解释器
 * @author Administrator
 *
 */
public class VarExpression extends Expression {

	private String key; // key=a,key=b,key=c

	public VarExpression(String key) {
		this.key = key;
	}

	// var 就是{a=10, b=20}
	// interpreter 根据 变量名称，返回对应值
	@Override
	public int interpreter(HashMap<String, Integer> var) {
		return var.get(this.key);
	}
}



package com.yxj.interpreter;

import java.util.HashMap;

/**
 * 抽象运算符号解析器 这里，每个运算符号，都只和自己左右两个数字有关系，
 * 但左右两个数字有可能也是一个解析的结果，无论何种类型，都是Expression类的实现类
 * 
 * @author Administrator
 *
 */
public class SymbolExpression extends Expression {

	protected Expression left;
	protected Expression right;

	public SymbolExpression(Expression left, Expression right) {
		this.left = left;
		this.right = right;
	}

	//因为 SymbolExpression 是让其子类来实现，因此 interpreter 是一个默认实现
	@Override
	public int interpreter(HashMap<String, Integer> var) {
		// TODO Auto-generated method stub
		return 0;
	}
}




package com.yxj.interpreter;

import java.util.HashMap;

/**
 * 加法解释器
 * @author Administrator
 *
 */
public class AddExpression extends SymbolExpression  {

	public AddExpression(Expression left, Expression right) {
		super(left, right);
	}

	//处理相加
	//var 仍然是 {a=10,b=20}..
	//一会我们debug 源码,就ok
	public int interpreter(HashMap<String, Integer> var) {
		//super.left.interpreter(var) ： 返回 left 表达式对应的值 a = 10
		//super.right.interpreter(var): 返回right 表达式对应值 b = 20
		return super.left.interpreter(var) + super.right.interpreter(var);
	}
}



package com.yxj.interpreter;

import java.util.HashMap;

public class SubExpression extends SymbolExpression {

	public SubExpression(Expression left, Expression right) {
		super(left, right);
	}

	//求出left 和 right 表达式相减后的结果
	public int interpreter(HashMap<String, Integer> var) {
		return super.left.interpreter(var) - super.right.interpreter(var);
	}
}




package com.yxj.interpreter;

import java.util.HashMap;
import java.util.Stack;

public class Calculator {

	// 定义表达式
	private Expression expression;

	// 构造函数传参，并解析
	public Calculator(String expStr) { // expStr = a+b
		// 安排运算先后顺序
		Stack<Expression> stack = new Stack<>();
		// 表达式拆分成字符数组 
		char[] charArray = expStr.toCharArray();// [a, +, b]

		Expression left = null;
		Expression right = null;
		//遍历我们的字符数组， 即遍历  [a, +, b]
		//针对不同的情况，做处理
		for (int i = 0; i < charArray.length; i++) {
			switch (charArray[i]) {
			case '+': //
				left = stack.pop();// 从stack取出left => "a"
				right = new VarExpression(String.valueOf(charArray[++i]));// 取出右表达式 "b"
				stack.push(new AddExpression(left, right));// 然后根据得到left 和 right 构建 AddExpresson加入stack
				break;
			case '-': // 
				left = stack.pop();
				right = new VarExpression(String.valueOf(charArray[++i]));
				stack.push(new SubExpression(left, right));
				break;
			default: 
				//如果是一个 Var 就创建要给 VarExpression 对象，并push到 stack
				stack.push(new VarExpression(String.valueOf(charArray[i])));
				break;
			}
		}
		//当遍历完整个 charArray 数组后，stack 就得到最后Expression
		this.expression = stack.pop();
	}

	public int run(HashMap<String, Integer> var) {
		//最后将表达式a+b和 var = {a=10,b=20}
		//然后传递给expression的interpreter进行解释执行
		return this.expression.interpreter(var);
	}
}



package com.yxj.interpreter;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.HashMap;

public class ClientTest {

	public static void main(String[] args) throws IOException {
		// TODO Auto-generated method stub
		String expStr = getExpStr(); // a+b
		HashMap<String, Integer> var = getValue(expStr);// var {a=10, b=20}
		Calculator calculator = new Calculator(expStr);
		System.out.println("运算结果：" + expStr + "=" + calculator.run(var));
	}

	// 获得表达式
	public static String getExpStr() throws IOException {
		System.out.print("请输入表达式：");
		return (new BufferedReader(new InputStreamReader(System.in))).readLine();
	}

	// 获得值映射
	public static HashMap<String, Integer> getValue(String expStr) throws IOException {
		HashMap<String, Integer> map = new HashMap<>();

		for (char ch : expStr.toCharArray()) {
			if (ch != '+' && ch != '-') {
				if (!map.containsKey(String.valueOf(ch))) {
					System.out.print("请输入" + String.valueOf(ch) + "的值：");
					String in = (new BufferedReader(new InputStreamReader(System.in))).readLine();
					map.put(String.valueOf(ch), Integer.valueOf(in));
				}
			}
		}

		return map;
	}
}

```
###  1.16.6 解释器模式在Spring框架应用的源码剖析 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125222201797.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```java
说明
 Expression 接口 表达式接口
下面有不同的实现类，比如SpelExpression, 或者CompositeStringExpression。
使用时候，根据你创建的不同的Parser 对象，返回不同的 Expression 对象
public Expression parseExpression(String expressionString, ParserContext context)
throws ParseException {
if (context == null) {
context = NON_TEMPLATE_PARSER_CONTEXT;
}
if (context.isTemplate()) {
return parseTemplate(expressionString, context); //返回的就是 CompositeStringExpression
}
else {
return doParseExpression(expressionString, context); //返回的就是 SpelExpression
}
}
使用得当 Expression对象，调用getValue 解释执行 表达式，最后得到结果
```
###  1.16.7 解释器模式的注意事项和细节
```markdown
1) 当有一个语言需要解释执行，可将该语言中的句子表示为一个抽
象语法树，就可以考虑使用解释器模式，让程序具有良好的扩展性
2) 应用场景：编译器、运算表达式计算、正则表达式、机器人等
3) 使用解释器可能带来的问题：解释器模式会引起类膨胀、解释
器模式采用递归调用方法，将会导致调试非常复杂、效率可能降低.
```
##  1.17 状态模式
###  1.17.1 APP抽奖活动问题
```markdown
请编写程序完成APP抽奖活动 具体要求如下:
1) 假如每参加一次这个活动要扣除用户50积分，中奖概率是10%
2) 奖品数量固定，抽完就不能抽奖
3) 活动有四个状态: 可以抽奖、不能抽奖、发放奖品和奖品领完
4) 活动的四个状态转换关系图
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125224024978.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
###  1.17.2 状态模式基本介绍
```markdown
1) 状态模式（State Pattern）：它主要用来解决对象在多种
状态转换时，需要对外输出不同的行为的问题。状态和行为
是一一对应的，状态之间可以相互转换
2) 当一个对象的内在状态改变时，允许改变其行为，这个对象
看起来像是改变了其类
```
###  1.17.3 状态模式原理类图
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112522422430.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
1) Context 类为环境角色, 用于维护State实例,这个实例定义
当前状态
2) State 是抽象状态角色,定义一个接口封装与Context 的一个
特点接口相关行为
3) ConcreteState 具体的状态角色，每个子类实现一个
与Context 的一个状态相关行为
```
###  1.17.4 状态模式解决APP抽奖问题
```markdown
完成APP抽奖活动项目，使用状态模式.
```
```markdown
定义出一个接口叫状态接口，每个状态都实现它。
接口有扣除积分方法、抽奖方法、发放奖品方法
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/202011252247095.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.state;

/**
 * 状态抽象类
 * @author Administrator
 *
 */
public abstract class State {

	
	// 扣除积分 - 50
    public abstract void deductMoney();

    // 是否抽中奖品
    public abstract boolean raffle();

    // 发放奖品
    public abstract  void dispensePrize();

}


package com.yxj.state;

/**
 * 不能抽奖状态
 * @author Administrator
 *
 */
public class NoRaffleState extends State {

	 // 初始化时传入活动引用，扣除积分后改变其状态
    RaffleActivity activity;

    public NoRaffleState(RaffleActivity activity) {
        this.activity = activity;
    }

    // 当前状态可以扣积分 , 扣除后，将状态设置成可以抽奖状态
    @Override
    public void deductMoney() {
        System.out.println("扣除50积分成功，您可以抽奖了");
        activity.setState(activity.getCanRaffleState());
    }

    // 当前状态不能抽奖
    @Override
    public boolean raffle() {
        System.out.println("扣了积分才能抽奖喔！");
        return false;
    }

    // 当前状态不能发奖品
    @Override
    public void dispensePrize() {
        System.out.println("不能发放奖品");
    }
}


package com.yxj.state;

import java.util.Random;

/**
 * 可以抽奖的状态
 * @author Administrator
 *
 */
public class CanRaffleState extends State {

    RaffleActivity activity;

    public CanRaffleState(RaffleActivity activity) {
        this.activity = activity;
    }

    //已经扣除了积分，不能再扣
    @Override
    public void deductMoney() {
        System.out.println("已经扣取过了积分");
    }

    //可以抽奖, 抽完奖后，根据实际情况，改成新的状态
    @Override
    public boolean raffle() {
        System.out.println("正在抽奖，请稍等！");
        Random r = new Random();
        int num = r.nextInt(10);
        // 10%中奖机会
        if(num == 0){
            // 改变活动状态为发放奖品 context
            activity.setState(activity.getDispenseState());
            return true;
        }else{
            System.out.println("很遗憾没有抽中奖品！");
            // 改变状态为不能抽奖
            activity.setState(activity.getNoRafflleState());
            return false;
        }
    }

    // 不能发放奖品
    @Override
    public void dispensePrize() {
        System.out.println("没中奖，不能发放奖品");
    }
}


package com.yxj.state;

/**
 * 发放奖品的状态
 * @author Administrator
 *
 */
public class DispenseState extends State {

	 // 初始化时传入活动引用，发放奖品后改变其状态
    RaffleActivity activity;

    public DispenseState(RaffleActivity activity) {
        this.activity = activity;
    }
    
    //

    @Override
    public void deductMoney() {
        System.out.println("不能扣除积分");
    }

    @Override
    public boolean raffle() {
        System.out.println("不能抽奖");
        return false;
    }

    //发放奖品
    @Override
    public void dispensePrize() {
        if(activity.getCount() > 0){
            System.out.println("恭喜中奖了");
            // 改变状态为不能抽奖
            activity.setState(activity.getNoRafflleState());
        }else{
            System.out.println("很遗憾，奖品发送完了");
            // 改变状态为奖品发送完毕, 后面我们就不可以抽奖
            activity.setState(activity.getDispensOutState());
            //System.out.println("抽奖活动结束");
            //System.exit(0);
        }

    }
}


package com.yxj.state;

/**
 * 奖品发放完毕状态
 * 说明，当我们activity 改变成 DispenseOutState， 抽奖活动结束
 * @author Administrator
 *
 */
public class DispenseOutState extends State {

	// 初始化时传入活动引用
    RaffleActivity activity;

    public DispenseOutState(RaffleActivity activity) {
        this.activity = activity;
    }
    @Override
    public void deductMoney() {
        System.out.println("奖品发送完了，请下次再参加");
    }

    @Override
    public boolean raffle() {
        System.out.println("奖品发送完了，请下次再参加");
        return false;
    }

    @Override
    public void dispensePrize() {
        System.out.println("奖品发送完了，请下次再参加");
    }
}


package com.yxj.state;

/**
 * 抽奖活动 //
 * 
 * @author Administrator
 *
 */
public class RaffleActivity {

	// state 表示活动当前的状态，是变化
    State state = null;
    // 奖品数量
    int count = 0;
    
    // 四个属性，表示四种状态
    State noRafflleState = new NoRaffleState(this);
    State canRaffleState = new CanRaffleState(this);
    
    State dispenseState =   new DispenseState(this);
    State dispensOutState = new DispenseOutState(this);

    //构造器
    //1. 初始化当前的状态为 noRafflleState（即不能抽奖的状态）
    //2. 初始化奖品的数量 
    public RaffleActivity( int count) {
        this.state = getNoRafflleState();
        this.count = count;
    }

    //扣分, 调用当前状态的 deductMoney
    public void debuctMoney(){
        state.deductMoney();
    }

    //抽奖 
    public void raffle(){
    	// 如果当前的状态是抽奖成功
        if(state.raffle()){
        	//领取奖品
            state.dispensePrize();
        }

    }

    public State getState() {
        return state;
    }

    public void setState(State state) {
        this.state = state;
    }

    //这里请大家注意，每领取一次奖品，count--
    public int getCount() {
    	int curCount = count; 
    	count--;
        return curCount;
    }

    public void setCount(int count) {
        this.count = count;
    }

    public State getNoRafflleState() {
        return noRafflleState;
    }

    public void setNoRafflleState(State noRafflleState) {
        this.noRafflleState = noRafflleState;
    }

    public State getCanRaffleState() {
        return canRaffleState;
    }

    public void setCanRaffleState(State canRaffleState) {
        this.canRaffleState = canRaffleState;
    }

    public State getDispenseState() {
        return dispenseState;
    }

    public void setDispenseState(State dispenseState) {
        this.dispenseState = dispenseState;
    }

    public State getDispensOutState() {
        return dispensOutState;
    }

    public void setDispensOutState(State dispensOutState) {
        this.dispensOutState = dispensOutState;
    }
}



package com.yxj.state;

/**
 * 状态模式测试类
 * @author Administrator
 *
 */
public class ClientTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		// 创建活动对象，奖品有1个奖品
        RaffleActivity activity = new RaffleActivity(1);

        // 我们连续抽300次奖
        for (int i = 0; i < 30; i++) {
            System.out.println("--------第" + (i + 1) + "次抽奖----------");
            // 参加抽奖，第一步点击扣除积分
            activity.debuctMoney();

            // 第二步抽奖
            activity.raffle();
        }
	}

}

```
###  1.17.5 状态模式在实际项目-借贷平台
```markdown
1) 借贷平台的订单，有审核-发布-抢单 等等 步骤，随着操
作的不同，会改变订单的状态, 项目中的这个模块实现就会
使用到状态模式
2) 通常通过if/else判断订单的状态，从而实现不同的逻辑，
伪代码如下
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112523403760.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
3) 使用状态模式完成 借贷平台项目的审核模块
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112523524636.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```java
package com.yxj.state.money;

/**
 * 状态接口
 * @author Administrator
 *
 */
public interface State {

	/**
     * 电审
     */
    void checkEvent(Context context);

    /**
     * 电审失败
     */
    void checkFailEvent(Context context);

    /**
     * 定价发布
     */
    void makePriceEvent(Context context);

    /**
     * 接单
     */
    void acceptOrderEvent(Context context);

    /**
     * 无人接单失效
     */
    void notPeopleAcceptEvent(Context context);

    /**
     * 付款
     */
    void payOrderEvent(Context context);

    /**
     * 接单有人支付失效
     */
    void orderFailureEvent(Context context);

    /**
     * 反馈
     */
    void feedBackEvent(Context context);


    String getCurrentState();
}



package com.yxj.state.money;

public abstract class AbstractState implements State {

	protected static final RuntimeException EXCEPTION = new RuntimeException("操作流程不允许");

	//抽象类，默认实现了 State 接口的所有方法
	//该类的所有方法，其子类(具体的状态类)，可以有选择的进行重写
	
    @Override
    public void checkEvent(Context context) {
        throw EXCEPTION;
    }

    @Override
    public void checkFailEvent(Context context) {
        throw EXCEPTION;
    }

    @Override
    public void makePriceEvent(Context context) {
        throw EXCEPTION;
    }

    @Override
    public void acceptOrderEvent(Context context) {
        throw EXCEPTION;
    }

    @Override
    public void notPeopleAcceptEvent(Context context) {
        throw EXCEPTION;
    }

    @Override
    public void payOrderEvent(Context context) {
        throw EXCEPTION;
    }

    @Override
    public void orderFailureEvent(Context context) {
        throw EXCEPTION;
    }

    @Override
    public void feedBackEvent(Context context) {
        throw EXCEPTION;
    }
}



package com.yxj.state.money;

//各种具体状态类
class FeedBackState extends AbstractState {

	@Override
	public String getCurrentState() {
		return StateEnum.FEED_BACKED.getValue();
	}
}

class GenerateState extends AbstractState {

	@Override
	public void checkEvent(Context context) {
		context.setState(new ReviewState());
	}

	@Override
	public void checkFailEvent(Context context) {
		context.setState(new FeedBackState());
	}

	@Override
	public String getCurrentState() {
		return StateEnum.GENERATE.getValue();
	}
}

class NotPayState extends AbstractState {

	@Override
	public void payOrderEvent(Context context) {
		context.setState(new PaidState());
	}

	@Override
	public void feedBackEvent(Context context) {
		context.setState(new FeedBackState());
	}

	@Override
	public String getCurrentState() {
		return StateEnum.NOT_PAY.getValue();
	}
}

class PaidState extends AbstractState {

	@Override
	public void feedBackEvent(Context context) {
		context.setState(new FeedBackState());
	}

	@Override
	public String getCurrentState() {
		return StateEnum.PAID.getValue();
	}
}

class PublishState extends AbstractState {

	@Override
	public void acceptOrderEvent(Context context) {
		//把当前状态设置为  NotPayState。。。
		//至于应该变成哪个状态，有流程图来决定
		context.setState(new NotPayState());
	}

	@Override
	public void notPeopleAcceptEvent(Context context) {
		context.setState(new FeedBackState());
	}

	@Override
	public String getCurrentState() {
		return StateEnum.PUBLISHED.getValue();
	}
}

class ReviewState extends AbstractState {

	@Override
	public void makePriceEvent(Context context) {
		context.setState(new PublishState());
	}

	@Override
	public String getCurrentState() {
		return StateEnum.REVIEWED.getValue();
	}

}



package com.yxj.state.money;

/**
 * 状态枚举类
 * @author Administrator
 *
 */
public enum StateEnum {

	 //订单生成
    GENERATE(1, "GENERATE"),

    //已审核
    REVIEWED(2, "REVIEWED"),

    //已发布
    PUBLISHED(3, "PUBLISHED"),

    //待付款
    NOT_PAY(4, "NOT_PAY"),

    //已付款
    PAID(5, "PAID"),

    //已完结
    FEED_BACKED(6, "FEED_BACKED");

    private int key;
    private String value;

    StateEnum(int key, String value) {
        this.key = key;
        this.value = value;
    }
    public int getKey() {return key;}
    public String getValue() {return value;}
}


package com.yxj.state.money;

//环境上下文
public class Context extends AbstractState{
	//当前的状态 state, 根据我们的业务流程处理，不停的变化
	private State state;

    @Override
    public void checkEvent(Context context) {
        state.checkEvent(this);
        getCurrentState();
    }

    @Override
    public void checkFailEvent(Context context) {
        state.checkFailEvent(this);
        getCurrentState();
    }

    @Override
    public void makePriceEvent(Context context) {
        state.makePriceEvent(this);
        getCurrentState();
    }

    @Override
    public void acceptOrderEvent(Context context) {
        state.acceptOrderEvent(this);
        getCurrentState();
    }

    @Override
    public void notPeopleAcceptEvent(Context context) {
        state.notPeopleAcceptEvent(this);
        getCurrentState();
    }

    @Override
    public void payOrderEvent(Context context) {
        state.payOrderEvent(this);
        getCurrentState();
    }

    @Override
    public void orderFailureEvent(Context context) {
        state.orderFailureEvent(this);
        getCurrentState();
    }

    @Override
    public void feedBackEvent(Context context) {
        state.feedBackEvent(this);
        getCurrentState();
    }

    public State getState() {
        return state;
    }

    public void setState(State state) {
        this.state = state;
    }

    @Override
    public String getCurrentState() {
        System.out.println("当前状态 : " + state.getCurrentState());
        return state.getCurrentState();
    }
}


package com.yxj.state.money;

/**测试类*/
public class ClientTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//创建context 对象
		Context context = new Context();
        //将当前状态设置为 PublishState
		context.setState(new PublishState());
        System.out.println(context.getCurrentState());
        
//        //publish --> not pay
        context.acceptOrderEvent(context);
//        //not pay --> paid
        context.payOrderEvent(context);
//        // 失败, 检测失败时，会抛出异常
//        try {
//        	context.checkFailEvent(context);
//        	System.out.println("流程正常..");
//		} catch (Exception e) {
//			// TODO: handle exception
//			System.out.println(e.getMessage());
//		}
        
	}

}


```
###  1.17.6 状态模式的注意事项和细节
```markdown
1) 代码有很强的可读性。状态模式将每个状态的行为封
装到对应的一个类中
2) 方便维护。将容易产生问题的if-else语句删除了，如果把每个
状态的行为都放到一个类中，每次调用方法时都要判断当前是什
么状态，不但会产出很多if-else语句，而且容易出错
3) 符合“开闭原则”。容易增删状态
4) 会产生很多类。每个状态都要一个对应的类，当状态过多
会产生很多类，加大维护难度
5) 应用场景：当一个事件或者对象有很多种状态，状态之
间会相互转换，对不同的状态要求有不同的行为的时候，可
以考虑使用状态模式
```
##  1.18 策略模式
###  1.18.1 鸭子问题
```markdown
1) 有各种鸭子(比如 野鸭、北京鸭、水鸭等， 鸭子有
各种行为，比如 叫、飞行等)
2) 显示鸭子的信息
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201126001113812.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.strategy;

public abstract class Duck {

	public Duck() {
	
	}

	public abstract void display();//显示鸭子信息
	
	public void quack() {
		System.out.println("鸭子嘎嘎叫~~");
	}
	
	public void swim() {
		System.out.println("鸭子会游泳~~");
	}
	
	public void fly() {
		System.out.println("鸭子会飞翔~~~");
	}
	
}



package com.yxj.strategy;

public class WildDuck extends Duck {

	@Override
	public void display() {
		// TODO Auto-generated method stub
		System.out.println(" 这是野鸭 ");
	}

}


package com.yxj.strategy;

public class PekingDuck extends Duck {

	@Override
	public void display() {
		// TODO Auto-generated method stub
		System.out.println("~~北京鸭~~~");
	}
	
	//因为北京鸭不能飞翔，因此需要重写fly
	@Override
	public void fly() {
		// TODO Auto-generated method stub
		System.out.println("北京鸭不能飞翔");
	}

}



package com.yxj.strategy;

public class ToyDuck extends Duck{

	@Override
	public void display() {
		// TODO Auto-generated method stub
		System.out.println("玩具鸭");
	}

	//需要重写父类的所有方法
	
	public void quack() {
		System.out.println("玩具鸭不能叫~~");
	}
	
	public void swim() {
		System.out.println("玩具鸭不会游泳~~");
	}
	
	public void fly() {
		System.out.println("玩具鸭不会飞翔~~~");
	}
}


package com.yxj.strategy;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//测试
	}

}

```
###  1.18.2 传统的方式实现的问题分析和解决方案
```markdown
1) 其它鸭子，都继承了Duck类，所以fly让所有子类都会飞了，这是不正确的
2) 上面说的1 的问题，其实是继承带来的问题：对类的局部改动，尤其超类的局部改
动，会影响其他部分。会有溢出效应
3) 为了改进1问题，我们可以通过覆盖fly 方法来解决 => 覆盖解决
4) 问题又来了，如果我们有一个玩具鸭子ToyDuck, 这样就需要ToyDuck去覆盖Duck
的所有实现的方法 => 解决思路 策略模式 (strategy pattern)
```

###  1.18.3 策略模式基本介绍
```markdown
1) 策略模式（Strategy Pattern）中，定义算法族，分别封
装起来，让他们之间可以互相替换，此模式让算法的变化独
立于使用算法的客户
2) 这算法体现了几个设计原则，第一、把变化的代码从
不变的代码中分离出来；
第二、针对接口编程而不是具体类（定义了策略接口）；
第三、多用组合/聚合，少用继承（客户通过组合方式使用策略）。
```
###  1.18.4 策略模式的原理类图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201126001931533.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


```markdown
说明：从上图可以看到，客户context 有成员变量strategy或
者其他的策略接口,至于需要使用到哪个策略，我们可以在
构造器中指定.
```
###  1.18.5 策略模式解决鸭子问题
```markdown
编写程序完成前面的鸭子项目，要求使用策略模式
```

```markdown
策略模式：分别封装行为接口，实现算法族，超类里放行为接
口对象，在子类里具体设定行为对象。原则就是：分离变
化部分，封装接口，基于接口编程各种功能。此模
式让行为的变化独立于算法的使用者
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201126002150791.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.strategy.improve;

public interface FlyBehavior {
	
	void fly(); // 子类具体实现
}


package com.yxj.strategy.improve;

public class BadFlyBehavior implements FlyBehavior {

	@Override
	public void fly() {
		// TODO Auto-generated method stub
		System.out.println(" 飞翔技术一般 ");
	}

}


package com.yxj.strategy.improve;

public class GoodFlyBehavior implements FlyBehavior {

	@Override
	public void fly() {
		// TODO Auto-generated method stub
		System.out.println(" 飞翔技术高超 ~~~");
	}

}



package com.yxj.strategy.improve;

public class NoFlyBehavior implements FlyBehavior{

	@Override
	public void fly() {
		// TODO Auto-generated method stub
		System.out.println(" 不会飞翔  ");
	}

}


package com.yxj.strategy.improve;

public interface QuackBehavior {
	void quack();//子类实现
}



package com.yxj.strategy.improve;

public abstract class Duck {

	//属性, 策略接口
	FlyBehavior flyBehavior;
	//其它属性<->策略接口
	QuackBehavior quackBehavior;
	
	public Duck() {
	
	}

	public abstract void display();//显示鸭子信息
	
	public void quack() {
		System.out.println("鸭子嘎嘎叫~~");
	}
	
	public void swim() {
		System.out.println("鸭子会游泳~~");
	}
	
	public void fly() {
		
		//改进
		if(flyBehavior != null) {
			flyBehavior.fly();
		}
	}

	public void setFlyBehavior(FlyBehavior flyBehavior) {
		this.flyBehavior = flyBehavior;
	}
	
	
	public void setQuackBehavior(QuackBehavior quackBehavior) {
		this.quackBehavior = quackBehavior;
	}
	
	
	
}


package com.yxj.strategy.improve;

public class PekingDuck extends Duck {

	
	//假如北京鸭可以飞翔，但是飞翔技术一般
	public PekingDuck() {
		// TODO Auto-generated constructor stub
		flyBehavior = new BadFlyBehavior();
		
	}
	
	@Override
	public void display() {
		// TODO Auto-generated method stub
		System.out.println("~~北京鸭~~~");
	}
	
	

}


package com.yxj.strategy.improve;

public class WildDuck extends Duck {

	
	//构造器，传入FlyBehavor 的对象
	public  WildDuck() {
		// TODO Auto-generated constructor stub
		flyBehavior = new GoodFlyBehavior();
	}
	
	
	@Override
	public void display() {
		// TODO Auto-generated method stub
		System.out.println(" 这是野鸭 ");
	}

}


package com.yxj.strategy.improve;

public class ToyDuck extends Duck{

	
	public ToyDuck() {
		// TODO Auto-generated constructor stub
		flyBehavior = new NoFlyBehavior();
	}
	
	@Override
	public void display() {
		// TODO Auto-generated method stub
		System.out.println("玩具鸭");
	}

	//需要重写父类的所有方法
	
	public void quack() {
		System.out.println("玩具鸭不能叫~~");
	}
	
	public void swim() {
		System.out.println("玩具鸭不会游泳~~");
	}
	
	
}



package com.yxj.strategy.improve;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		WildDuck wildDuck = new WildDuck();
		wildDuck.fly();//
		
		ToyDuck toyDuck = new ToyDuck();
		toyDuck.fly();
		
		PekingDuck pekingDuck = new PekingDuck();
		pekingDuck.fly();
		
		//动态改变某个对象的行为, 北京鸭 不能飞
		pekingDuck.setFlyBehavior(new NoFlyBehavior());
		System.out.println("北京鸭的实际飞翔能力");
		pekingDuck.fly();
	}

}

```
###  1.18.6 策略模式在JDK-Arrays 应用的源码分析

```markdown
1) JDK的 Arrays 的Comparator就使用了策略模式
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201126095956272.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.jdk;

import java.util.Arrays;
import java.util.Comparator;


public class Strategy {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//数组
		Integer[] data = { 9, 1, 2, 8, 4, 3 };
		// 实现降序排序，返回-1放左边，1放右边，0保持不变
		
		// 说明
		// 1. 实现了 Comparator 接口（策略接口） , 匿名类 对象 new Comparator<Integer>(){..}
		// 2. 对象 new Comparator<Integer>(){..} 就是实现了 策略接口 的对象
		// 3. public int compare(Integer o1, Integer o2){} 指定具体的处理方式
		Comparator<Integer> comparator = new Comparator<Integer>() {
			public int compare(Integer o1, Integer o2) {
				if (o1 > o2) {
					return -1;
				} else {
					return 1;
				}
			};
		};
		
		// 说明
		/*
		 * public static <T> void sort(T[] a, Comparator<? super T> c) {
		        if (c == null) {
		            sort(a); //默认方法
		        } else { 
		            if (LegacyMergeSort.userRequested)
		                legacyMergeSort(a, c); //使用策略对象c
		            else
		            	// 使用策略对象c
		                TimSort.sort(a, 0, a.length, c, null, 0, 0);
		        }
		    }
		 */
		//方式1 
		Arrays.sort(data, comparator);
		
		System.out.println(Arrays.toString(data)); // 降序排序

		
		//方式2- 同时lambda 表达式实现 策略模式
		Integer[] data2 = { 19, 11, 12, 18, 14, 13 };
		
		Arrays.sort(data2, (var1, var2) -> {
			if(var1.compareTo(var2) > 0) {
				return -1;
			} else {
				return 1;
			}
		});
		
		System.out.println("data2=" + Arrays.toString(data2));
		
	}

}

```
###  1.18.7 策略模式的注意事项和细节
```markdown
1) 策略模式的关键是：分析项目中变化部分与不变部分
2) 策略模式的核心思想是：多用组合/聚合 少用继承；用行为
类组合，而不是行为的继承。更有弹性
3) 体现了“对修改关闭，对扩展开放”原则，客户端增加行为
不用修改原有代码，只要添加一种策略（或者行为）即可，
避免了使用多重转移语句（if..else if..else）
4) 提供了可以替换继承关系的办法： 策略模式将算法封
装在独立的Strategy类中使得你可以独立于其Context改
变它，使它易于切换、易于理解、易于扩展
5) 需要注意的是：每添加一个策略就要增加一个类，当策
略过多是会导致类数目庞大
```
##  1.19 职责链模式
###  1.19.1 OA系统采购审批需求
```markdown
学校OA系统的采购审批项目：需求是
1) 采购员采购教学器材
2) 如果金额 小于等于5000, 由教学主任审批 （0<=x<=5000）
3) 如果金额 小于等于10000, 由院长审批 (5000<x<=10000)
4) 如果金额 小于等于30000, 由副校长审批 (10000<x<=30000)
5) 如果金额 超过30000以上，有校长审批 ( 30000<x)
```

```markdown
传统方案解决OA系统审批，传统的设计方案(类图) 
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201126100531805.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
###  1.19.2 传统方案解决OA系统审批问题分析
```markdown
1) 传统方式是：接收到一个采购请求后，根据采购金额来调用
对应的Approver (审批人)完成审批。
2) 传统方式的问题分析 : 客户端这里会使用到 分支判断
(比如switch) 来对不同的采购请求处理， 这样就存在如下问题 
(1) 如果各个级别的人员审批金额发生变化，在客户端的也需要变化 
(2) 客户端必须明确的知道 有多少个审批级别和访问
3) 这样 对一个采购请求进行处理 和 Approver (审批人) 就存
在强耦合关系，不利于代码的扩展和维护
4) 解决方案 =》 职责链模式
```
###  1.19.3 职责链模式基本介绍
```markdown
1) 职责链模式（Chain of Responsibility Pattern）,
又叫 责任链模式，为请求创建了一个接收者
对象的链(简单示意图)。这种模式对请求的
发送者和接收者进行解耦。
2) 职责链模式通常每个接收者都包含对另一个接
收者的引用。如果一个对象不能处理该请求，
那么它会把相同的请求传给下一个接收者，依
此类推。
3) 这种类型的设计模式属于行为型模式
职责链模式（Chain Of Responsibility），
使多个对象都有机会处理请求，从而避
免请求的发送者和接收者之间的耦合关
系。将这个对象连成一条链，并沿着这
条链传递该请求，直到有一个对象处理
它为止.
```
###  1.19.4 职责链模式的原理类图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201126100801571.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
1) Handler : 抽象的处理者, 定义了一个处理请求的接口, 
同时含义另外Handler
2) ConcreteHandlerA , B 是具体的处理者, 处理它自己负责
的请求， 可以访问它的后继者(即下一个处理者), 如果可以
处理当前请求，则处理，否则就将该请求交个 后继者去处
理，从而形成一个职责链
3) Request ， 含义很多属性，表示一个请求
```
###  1.19.5 职责链模式解决OA系统采购审批
```markdown
编写程序完成学校OA系统的采购审批项目：需求
采购员采购教学器材
如果金额 小于等于5000, 由教学主任审批
如果金额 小于等于10000, 由院长审批
如果金额 小于等于30000, 由副校长审批
如果金额 超过30000以上，有校长审批
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201126101006435.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.responsibilitychain;


//请求类
public class PurchaseRequest {

	private int type = 0; //请求类型
	private float price = 0.0f; //请求金额
	private int id = 0;
	//构造器
	public PurchaseRequest(int type, float price, int id) {
		this.type = type;
		this.price = price;
		this.id = id;
	}
	public int getType() {
		return type;
	}
	public float getPrice() {
		return price;
	}
	public int getId() {
		return id;
	}
	
			
}



package com.yxj.responsibilitychain;

public abstract class Approver {

	Approver approver;  //下一个处理者
	String name; // 名字
	
	public Approver(String name) {
		// TODO Auto-generated constructor stub
		this.name = name;
	}

	//下一个处理者
	public void setApprover(Approver approver) {
		this.approver = approver;
	}
	
	//处理审批请求的方法，得到一个请求, 处理是子类完成，因此该方法做成抽象
	public abstract void processRequest(PurchaseRequest purchaseRequest);
	
}



package com.yxj.responsibilitychain;

public class CollegeApprover extends Approver {

	public CollegeApprover(String name) {
		// TODO Auto-generated constructor stub
		super(name);
	}
	
	@Override
	public void processRequest(PurchaseRequest purchaseRequest) {
		// TODO Auto-generated method stub
		if(purchaseRequest.getPrice() < 5000 && purchaseRequest.getPrice() <= 10000) {
			System.out.println(" 请求编号 id= " + purchaseRequest.getId() + " 被 " + this.name + " 处理");
		}else {
			approver.processRequest(purchaseRequest);
		}
	}
}



package com.yxj.responsibilitychain;

public class DepartmentApprover extends Approver {

	
	public DepartmentApprover(String name) {
		// TODO Auto-generated constructor stub
		super(name);
	}
	
	@Override
	public void processRequest(PurchaseRequest purchaseRequest) {
		// TODO Auto-generated method stub
		if(purchaseRequest.getPrice() <= 5000) {
			System.out.println(" 请求编号 id= " + purchaseRequest.getId() + " 被 " + this.name + " 处理");
		}else {
			approver.processRequest(purchaseRequest);
		}
	}

}


package com.yxj.responsibilitychain;

public class SchoolMasterApprover extends Approver {

	public SchoolMasterApprover(String name) {
		// TODO Auto-generated constructor stub
		super(name);
	}
	
	@Override
	public void processRequest(PurchaseRequest purchaseRequest) {
		// TODO Auto-generated method stub
		if(purchaseRequest.getPrice() > 30000) {
			System.out.println(" 请求编号 id= " + purchaseRequest.getId() + " 被 " + this.name + " 处理");
		}else {
			approver.processRequest(purchaseRequest);
		}
	}
}


package com.yxj.responsibilitychain;

public class ViceSchoolMasterApprover extends Approver {

	public ViceSchoolMasterApprover(String name) {
		// TODO Auto-generated constructor stub
		super(name);
	}
	
	@Override
	public void processRequest(PurchaseRequest purchaseRequest) {
		// TODO Auto-generated method stub
		if(purchaseRequest.getPrice() < 10000 && purchaseRequest.getPrice() <= 30000) {
			System.out.println(" 请求编号 id= " + purchaseRequest.getId() + " 被 " + this.name + " 处理");
		}else {
			approver.processRequest(purchaseRequest);
		}
	}
}


package com.yxj.responsibilitychain;

public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//创建一个请求
		PurchaseRequest purchaseRequest = new PurchaseRequest(1, 31000, 1);
		
		//创建相关的审批人
		DepartmentApprover departmentApprover = new DepartmentApprover("张主任");
		CollegeApprover collegeApprover = new CollegeApprover("李院长");
		ViceSchoolMasterApprover viceSchoolMasterApprover = new ViceSchoolMasterApprover("王副校");
		SchoolMasterApprover schoolMasterApprover = new SchoolMasterApprover("佟校长");
	
	
		//需要将各个审批级别的下一个设置好 (处理人构成环形: )
		departmentApprover.setApprover(collegeApprover);
		collegeApprover.setApprover(viceSchoolMasterApprover);
		viceSchoolMasterApprover.setApprover(schoolMasterApprover);
		schoolMasterApprover.setApprover(departmentApprover);
		
		
		
		departmentApprover.processRequest(purchaseRequest);
		viceSchoolMasterApprover.processRequest(purchaseRequest);
	}

}

```
###  1.19.6 职责链模式在SpringMVC框架应用的源码分析
```markdown
) SpringMVC-HandlerExecutionChain 类就使用到职责链模式
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201126101706710.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
package com.yxj.spring.test;

import org.springframework.web.servlet.HandlerExecutionChain;
import org.springframework.web.servlet.HandlerInterceptor;

public class ResponsibilityChain {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		// DispatcherServlet 
		
		//说明
		/*
		 * 
		 *  protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
		 *   HandlerExecutionChain mappedHandler = null; 
		 *   mappedHandler = getHandler(processedRequest);//获取到HandlerExecutionChain对象
		 *    //在 mappedHandler.applyPreHandle 内部 得到啦 HandlerInterceptor interceptor
		 *    //调用了拦截器的  interceptor.preHandle
		 *   if (!mappedHandler.applyPreHandle(processedRequest, response)) {
					return;
				}
				
			  //说明：mappedHandler.applyPostHandle 方法内部获取到拦截器，并调用 
			  //拦截器的  interceptor.postHandle(request, response, this.handler, mv);
			 mappedHandler.applyPostHandle(processedRequest, response, mv);
		 *  }
		 *  
		 *  
		 *  //说明：在  mappedHandler.applyPreHandle内部中，
		 *  还调用了  triggerAfterCompletion 方法，该方法中调用了  
		 *  HandlerInterceptor interceptor = getInterceptors()[i];
			try {
				interceptor.afterCompletion(request, response, this.handler, ex);
			}
			catch (Throwable ex2) {
				logger.error("HandlerInterceptor.afterCompletion threw exception", ex2);
			}
		 */
	
	}

}

```
```markdown
springmvc 请求的流程图中，执行了 拦截器相关方法 
interceptor.preHandler 等等在处理SpringMvc请求时，
使用到职责链模式还使用到适配器模式
HandlerExecutionChain 主要负责的是请求拦截器的执
行和请求处理,但是他本身不处理请求，只是将请求分
配给链上注册处理器执行，这是职责链实现方式,减少职责
链本身与处理逻辑之间的耦合,规范了处理流程
HandlerExecutionChain 维护了 HandlerInterceptor 
的集合， 可以向其中注册相应的拦截器.
```
###  1.19.7 职责链模式的注意事项和细节
```markdown
1) 将请求和处理分开，实现解耦，提高系统的灵活性
2) 简化了对象，使对象不需要知道链的结构
3) 性能会受到影响，特别是在链比较长的时候，因此需控制链中
最大节点数量，一般通过在Handler中设置一个最大节点数量，
在setNext()方法中判断是否已经超过阀值，超过则不允许该链
建立，避免出现超长链无意识地破坏系统性能
4) 调试不方便。采用了类似递归的方式，调试时逻辑可能比较复杂
5) 最佳应用场景：有多个对象可以处理同一个请求时，比如：
多级请求、请假/加薪
等审批流程、Java Web中Tomcat对Encoding的处理、拦截器
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复
设计模式2即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
