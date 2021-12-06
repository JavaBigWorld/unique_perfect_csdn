# java8 新特性
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714192140412.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1 Java 8新特性简介
```markdown
速度更快
代码更少（增加了新的语法 Lambda 表达式）
强大的 Stream API
便于并行
最大化减少空指针异常 Optional
其中最为核心的为 Lambda 表达式与Stream API
```
## 2 Lambda表达式
```markdown
Lambda 是一个匿名函数，我们可以把 Lambda
表达式理解为是一段可以传递的代码（将代码
像数据一样进行传递）。可以写出更简洁、更
灵活的代码。作为一种更紧凑的代码风格，使
Java的语言表达能力得到了提升。
```
### 2.1 从匿名类到 Lambda 的转换
```java
package com.yxj.java8;

public class Employee {

	private int id;
	private String name;
	private int age;
	private double salary;

	public Employee() {
	}

	public Employee(String name) {
		this.name = name;
	}

	public Employee(String name, int age) {
		this.name = name;
		this.age = age;
	}

	public Employee(int id, String name, int age, double salary) {
		this.id = id;
		this.name = name;
		this.age = age;
		this.salary = salary;
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

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

	public double getSalary() {
		return salary;
	}

	public void setSalary(double salary) {
		this.salary = salary;
	}

	public String show() {
		return "测试方法引用！";
	}

	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + age;
		result = prime * result + id;
		result = prime * result + ((name == null) ? 0 : name.hashCode());
		long temp;
		temp = Double.doubleToLongBits(salary);
		result = prime * result + (int) (temp ^ (temp >>> 32));
		return result;
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Employee other = (Employee) obj;
		if (age != other.age)
			return false;
		if (id != other.id)
			return false;
		if (name == null) {
			if (other.name != null)
				return false;
		} else if (!name.equals(other.name))
			return false;
		if (Double.doubleToLongBits(salary) != Double.doubleToLongBits(other.salary))
			return false;
		return true;
	}

	@Override
	public String toString() {
		return "Employee [id=" + id + ", name=" + name + ", age=" + age + ", salary=" + salary + "]";
	}

}
```
```java
package com.yxj.java8;

@FunctionalInterface
public interface MyPredicate<T> {

	public boolean test(T t);
	
}
```
```java
package com.yxj.java8;

public class FilterEmployeeForAge implements MyPredicate<Employee>{

	@Override
	public boolean test(Employee t) {
		return t.getAge() <= 35;
	}

}

```
```java
package com.yxj.java8;

public class FilterEmployeeForSalary implements MyPredicate<Employee> {

	@Override
	public boolean test(Employee t) {
		return t.getSalary() >= 5000;
	}

}
```
```java
package com.yxj.java8;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.TreeSet;
import org.junit.Test;

public class TestLambda1 {
	
	//原来的匿名内部类
	@Test
	public void test1(){
		Comparator<String> com = new Comparator<String>(){
			@Override
			public int compare(String o1, String o2) {
				return Integer.compare(o1.length(), o2.length());
			}
		};
		
		TreeSet<String> ts = new TreeSet<>(com);
		
		TreeSet<String> ts2 = new TreeSet<>(new Comparator<String>(){
			@Override
			public int compare(String o1, String o2) {
				return Integer.compare(o1.length(), o2.length());
			}
			
		});
	}
	
	//现在的 Lambda 表达式
	@Test
	public void test2(){
		Comparator<String> com = (x, y) -> Integer.compare(x.length(), y.length());
		TreeSet<String> ts = new TreeSet<>(com);
	}
	
	List<Employee> emps = Arrays.asList(
			new Employee(101, "张三", 18, 9999.99),
			new Employee(102, "李四", 59, 6666.66),
			new Employee(103, "王五", 28, 3333.33),
			new Employee(104, "赵六", 8, 7777.77),
			new Employee(105, "田七", 38, 5555.55)
	);

	//需求：获取公司中年龄小于 35 的员工信息
	public List<Employee> filterEmployeeAge(List<Employee> emps){
		List<Employee> list = new ArrayList<>();
		
		for (Employee emp : emps) {
			if(emp.getAge() <= 35){
				list.add(emp);
			}
		}
		
		return list;
	}
	
	@Test
	public void test3(){
		List<Employee> list = filterEmployeeAge(emps);
		
		for (Employee employee : list) {
			System.out.println(employee);
		}
	}
	
	//需求：获取公司中工资大于 5000 的员工信息
	public List<Employee> filterEmployeeSalary(List<Employee> emps){
		List<Employee> list = new ArrayList<>();
		
		for (Employee emp : emps) {
			if(emp.getSalary() >= 5000){
				list.add(emp);
			}
		}
		
		return list;
	}
	
	//优化方式一：策略设计模式
	public List<Employee> filterEmployee(List<Employee> emps, MyPredicate<Employee> mp){
		List<Employee> list = new ArrayList<>();
		
		for (Employee employee : emps) {
			if(mp.test(employee)){
				list.add(employee);
			}
		}
		
		return list;
	}
	
	@Test
	public void test4(){
		List<Employee> list = filterEmployee(emps, new FilterEmployeeForAge());
		for (Employee employee : list) {
			System.out.println(employee);
		}
		
		System.out.println("------------------------------------------");
		
		List<Employee> list2 = filterEmployee(emps, new FilterEmployeeForSalary());
		for (Employee employee : list2) {
			System.out.println(employee);
		}
	}
	
	//优化方式二：匿名内部类
	@Test
	public void test5(){
		List<Employee> list = filterEmployee(emps, new MyPredicate<Employee>() {
			@Override
			public boolean test(Employee t) {
				return t.getId() <= 103;
			}
		});
		
		for (Employee employee : list) {
			System.out.println(employee);
		}
	}
	
	//优化方式三：Lambda 表达式
	@Test
	public void test6(){
		List<Employee> list = filterEmployee(emps, (e) -> e.getAge() <= 35);
		list.forEach(System.out::println);
		
		System.out.println("------------------------------------------");
		
		List<Employee> list2 = filterEmployee(emps, (e) -> e.getSalary() >= 5000);
		list2.forEach(System.out::println);
	}
	
	//优化方式四：Stream API
	@Test
	public void test7(){
		emps.stream()
			.filter((e) -> e.getAge() <= 35)
			.forEach(System.out::println);
		
		System.out.println("----------------------------------------------");
		
		emps.stream()
			.map(Employee::getName)
			.limit(3)
			.sorted()
			.forEach(System.out::println);
	}
}
```
### 2.2 Lambda 表达式语法
```markdown
Lambda 表达式在Java 语言中引入了一个新的语法元
素和操作符。这个操作符为 “->” ， 该操作符被称
为 Lambda 操作符或剪头操作符。它将 Lambda 分为
两个部分：
左侧：指定了 Lambda 表达式需要的所有参数
右侧：指定了 Lambda 体，即 Lambda 表达式要执行
的功能。
```
### 2.3 类型推断
```markdown
上述 Lambda 表达式中的参数类型都是由编译器推断
得出的。Lambda 表达式中无需指定类型，程序依然可
以编译，这是因为 javac 根据程序的上下文，在后台
推断出了参数的类型。Lambda 表达式的类型依赖于上
下文环境，是由编译器推断出来的。这就是所谓的
“类型推断”
```
## 3 函数式接口
```markdown
只包含一个抽象方法的接口，称为函数式接口。
你可以通过 Lambda 表达式来创建该接口的对象。（若 Lambda
表达式抛出一个受检异常，那么该异常需要在目标接口的抽象方
法上进行声明）。
我们可以在任意函数式接口上使用 @FunctionalInterface 注解，
这样做可以检查它是否是一个函数式接口，同时 javadoc 也会包
含一条声明，说明这个接口是一个函数式接口。
```
```java
package com.yxj.java8;

import java.util.ArrayList;
import java.util.Comparator;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.function.Consumer;

import org.junit.Test;

/*
 * 一、Lambda 表达式的基础语法：Java8中引入了一个新的操作符 "->" 该操作符称为箭头操作符或 Lambda 操作符
 * 						    箭头操作符将 Lambda 表达式拆分成两部分：
 * 
 * 左侧：Lambda 表达式的参数列表
 * 右侧：Lambda 表达式中所需执行的功能， 即 Lambda 体
 * 
 * 语法格式一：无参数，无返回值
 * 		() -> System.out.println("Hello Lambda!");
 * 
 * 语法格式二：有一个参数，并且无返回值
 * 		(x) -> System.out.println(x)
 * 
 * 语法格式三：若只有一个参数，小括号可以省略不写
 * 		x -> System.out.println(x)
 * 
 * 语法格式四：有两个以上的参数，有返回值，并且 Lambda 体中有多条语句
 *		Comparator<Integer> com = (x, y) -> {
 *			System.out.println("函数式接口");
 *			return Integer.compare(x, y);
 *		};
 *
 * 语法格式五：若 Lambda 体中只有一条语句， return 和 大括号都可以省略不写
 * 		Comparator<Integer> com = (x, y) -> Integer.compare(x, y);
 * 
 * 语法格式六：Lambda 表达式的参数列表的数据类型可以省略不写，因为JVM编译器通过上下文推断出，数据类型，即“类型推断”
 * 		(Integer x, Integer y) -> Integer.compare(x, y);
 * 
 * 上联：左右遇一括号省
 * 下联：左侧推断类型省
 * 横批：能省则省
 * 
 * 二、Lambda 表达式需要“函数式接口”的支持
 * 函数式接口：接口中只有一个抽象方法的接口，称为函数式接口。 可以使用注解 @FunctionalInterface 修饰
 * 			 可以检查是否是函数式接口
 */
public class TestLambda2 {
	
	@Test
	public void test1(){
		int num = 0;//jdk 1.7 前，必须是 final
		
		Runnable r = new Runnable() {
			@Override
			public void run() {
				System.out.println("Hello World!" + num);
			}
		};
		
		r.run();
		
		System.out.println("-------------------------------");
		
		Runnable r1 = () -> System.out.println("Hello Lambda!");
		r1.run();
	}
	
	@Test
	public void test2(){
		Consumer<String> con = x -> System.out.println(x);
		con.accept("威武！");
	}
	
	@Test
	public void test3(){
		Comparator<Integer> com = (x, y) -> {
			System.out.println("函数式接口");
			return Integer.compare(x, y);
		};
	}
	
	@Test
	public void test4(){
		Comparator<Integer> com = (x, y) -> Integer.compare(x, y);
	}
	
	@Test
	public void test5(){
//		String[] strs;
//		strs = {"aaa", "bbb", "ccc"};
		
		List<String> list = new ArrayList<>();
		
		show(new HashMap<>());
	}

	public void show(Map<String, Integer> map){
		
	}
	
	//需求：对一个数进行运算
	@Test
	public void test6(){
		Integer num = operation(100, (x) -> x * x);
		System.out.println(num);
		
		System.out.println(operation(200, (y) -> y + 200));
	}
	
	public Integer operation(Integer num, MyFun mf){
		return mf.getValue(num);
	}
}
```
### 3.1 自定义函数式接口
```java
@FunctionalInterface
public interface MyNumber{
    public double getValue();
}

// 函数式接口使用泛型
@FunctionalInterface
public interface MyFunc<T>{
    public T getValue(T t);
}
```
### 3.2 作为参数传递 Lambda 表达式
```java
String newStr = toUpperString((str -> {
    str.toUpperCase();
}), "abcdef");
System.out.println(newStr);

// 作为参数传递 Lambda 表达式
public String toUpperString(MyFunc<String> mf, String str){
    return mf.getValue(str);
}
```
```markdown
作为参数传递 Lambda 表达式：为了将 Lambda 表达式作
为参数传递，接收Lambda 表达式的参数类型必须是与该
Lambda 表达式兼容的函数式接口的类型。
```
## 4 Java 内置四大核心函数式接口
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201207105958810.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
package com.yxj.java8;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.function.Consumer;
import java.util.function.Function;
import java.util.function.Predicate;
import java.util.function.Supplier;

import org.junit.Test;

/*
 * Java8 内置的四大核心函数式接口
 * 
 * Consumer<T> : 消费型接口
 * 		void accept(T t);
 * 
 * Supplier<T> : 供给型接口
 * 		T get(); 
 * 
 * Function<T, R> : 函数型接口
 * 		R apply(T t);
 * 
 * Predicate<T> : 断言型接口
 * 		boolean test(T t);
 * 
 */
public class TestLambda {


    public static void main(String[] args) {
        TestLambda testLambda = new TestLambda();
        testLambda.test1();
        testLambda.test2();
        testLambda.test3();
        testLambda.test4();
    }

    //Consumer<T> 消费型接口 :
    public void test1(){
        happy(10000, (m) -> System.out.println("你们刚哥喜欢大宝剑，每次消费：" + m + "元"));
    }

    public void happy(double money, Consumer<Double> con){
        con.accept(money);
    }

    //Supplier<T> 供给型接口 :
    public void test2(){
        List<Integer> numList = getNumList(10, () -> (int)(Math.random() * 100));

        for (Integer num : numList) {
            System.out.println(num);
        }
    }

    //需求：产生指定个数的整数，并放入集合中
    public List<Integer> getNumList(int num, Supplier<Integer> sup){
        List<Integer> list = new ArrayList<>();

        for (int i = 0; i < num; i++) {
            Integer n = sup.get();
            list.add(n);
        }

        return list;
    }


    //Function<T, R> 函数型接口：
    public void test3(){
        String newStr = strHandler("\t\t\t 威武   ", (str) -> str.trim());
        System.out.println(newStr);

        String subStr = strHandler("  威武呀", (str) -> str.substring(2, 5));
        System.out.println(subStr);
    }

    //需求：用于处理字符串
    public String strHandler(String str, Function<String, String> fun){
        return fun.apply(str);
    }


    //Predicate<T> 断言型接口：
    public void test4(){
        List<String> list = Arrays.asList("Hello", "yxj", "Lambda", "www", "ok");
        List<String> strList = filterStr(list, (s) -> s.length() > 3);

        for (String str : strList) {
            System.out.println(str);
        }
    }

    //需求：将满足条件的字符串，放入集合中
    public List<String> filterStr(List<String> list, Predicate<String> pre){
        List<String> strList = new ArrayList<>();

        for (String str : list) {
            if(pre.test(str)){
                strList.add(str);
            }
        }

        return strList;
    }

}
```
### 4.1 其他接口
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201207110028521.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
## 5 方法引用与构造器引用
```java
package com.yxj.java8;
import java.io.PrintStream;
import java.util.Comparator;
import java.util.function.*;

/*
 * 一、方法引用：若 Lambda 体中的功能，已经有方法提供了实现，可以使用方法引用
 * 			  （可以将方法引用理解为 Lambda 表达式的另外一种表现形式）
 * 
 * 1. 对象的引用 :: 实例方法名
 * 
 * 2. 类名 :: 静态方法名
 * 
 * 3. 类名 :: 实例方法名
 * 
 * 注意：
 * 	 1 方法引用所引用的方法的参数列表与返回值类型，需要与函数式接口中抽象方法的参数列表和返回值类型保持一致！
 * 	 2 若Lambda 的参数列表的第一个参数，是实例方法的调用者，第二个参数(或无参)是实例方法的参数时，格式： ClassName::MethodName
 * 
 * 二、构造器引用 :构造器的参数列表，需要与函数式接口中参数列表保持一致！
 * 
 * 1. 类名 :: new
 * 
 * 三、数组引用
 * 
 * 	类型[] :: new;
 * 
 * 
 */

public class TestMethodRef {
    public static void main(String[] args) {
        TestMethodRef testMethodRef = new TestMethodRef();
        testMethodRef.test1();
        testMethodRef.test2();
        testMethodRef.test3();
        testMethodRef.test4();
        testMethodRef.test5();
        testMethodRef.test6();
        testMethodRef.test7();
        testMethodRef.test8();
    }


    public void test1(){
        PrintStream ps = System.out;
        Consumer<String> con = (str) -> ps.println(str);
        con.accept("Hello World！");

        System.out.println("--------------------------------");

        Consumer<String> con2 = ps::println;
        con2.accept("Hello Java8！");
        System.out.println("--------------------------------");
        Consumer<String> con3 = System.out::println;
        con3.accept("Hello Javala");
    }

    //对象的引用 :: 实例方法名
    public void test2(){
        Employee emp = new Employee(101, "张三", 18, 9999.99);

        Supplier<String> sup = () -> emp.getName();
        System.out.println(sup.get());

        System.out.println("----------------------------------");

        Supplier<String> sup2 = emp::getName;
        System.out.println(sup2.get());
    }
    public void test3(){
        BiFunction<Double, Double, Double> fun = (x, y) -> Math.max(x, y);
        System.out.println(fun.apply(1.5, 22.2));

        System.out.println("--------------------------------------------------");

        BiFunction<Double, Double, Double> fun2 = Math::max;
        System.out.println(fun2.apply(1.2, 1.5));
    }
    

    //类名 :: 静态方法名
    public void test4(){
        Comparator<Integer> com = (x, y) -> Integer.compare(x, y);

        System.out.println("-------------------------------------");

        Comparator<Integer> com2 = Integer::compare;
    }


    //类名 :: 实例方法名
    public void test5(){
        BiPredicate<String, String> bp = (x, y) -> x.equals(y);
        System.out.println(bp.test("abcde", "abcde"));

        System.out.println("-----------------------------------------");

        BiPredicate<String, String> bp2 = String::equals;
        System.out.println(bp2.test("abc", "abc"));

        System.out.println("-----------------------------------------");


        Function<Employee, String> fun = (e) -> e.show();
        System.out.println(fun.apply(new Employee()));

        System.out.println("-----------------------------------------");

        Function<Employee, String> fun2 = Employee::show;
        System.out.println(fun2.apply(new Employee()));

    }


    public void test6(){
        Supplier<Employee> sup = () -> new Employee();
        System.out.println(sup.get());

        System.out.println("------------------------------------");

        Supplier<Employee> sup2 = Employee::new;
        System.out.println(sup2.get());
    }


    //构造器引用
    public void test7(){
        Function<String, Employee> fun = Employee::new;

        BiFunction<String, Integer, Employee> fun2 = Employee::new;
    }


    //数组引用
    public void test8(){
        Function<Integer, String[]> fun = (args) -> new String[args];
        String[] strs = fun.apply(10);
        System.out.println(strs.length);

        System.out.println("--------------------------");

        Function<Integer, Employee[]> fun2 = Employee[] :: new;
        Employee[] emps = fun2.apply(20);
        System.out.println(emps.length);
    }
}
```
### 5.1 Lambda 表达式中的闭包问题
```java
这个问题我们在匿名内部类中也会存在，如果我们把注释
放开会报错，告诉我 num 值是 final 不能被改变。这
里我们虽然没有标识 num 类型为 final，但是在编
译期间虚拟机会帮我们加上 final 修饰关键字。

import java.util.function.Consumer;
public class Main {
    public static void main(String[] args) {

        int num = 10;

        Consumer<String> consumer = ele -> {
            System.out.println(num);
        };

        //num = num + 2;
        consumer.accept("hello");
    }
}
```

### 5.2 强大的 Stream API
```markdown
Java8中有两大最为重要的改变。第一个是 Lambda 表
达式；另外一个则是 Stream API(java.util.stream.*)。
Stream 是 Java8 中处理集合的关键抽象概念，它可以指
定你希望对集合进行的操作，可以执行非常复杂的查找、
过滤和映射数据等操作。使用Stream API 对集合数据
进行操作，就类似于使用 SQL 执行的数据库查询。也
可以使用 Stream API 来并行执行操作。简而言之，
Stream API 提供了一种高效且易于使用的处理数据的方式。
```
```markdown
流(Stream) 到底是什么呢？
是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列。
“集合讲的是数据，流讲的是计算！”
注意：
Stream 自己不会存储元素。
Stream 不会改变源对象。相反，他们会返回一个持有结果
的新Stream。
Stream 操作是延迟执行的。这意味着他们会等到需要
结果的时候才执行。
```
```markdown
Stream 的操作三个步骤
创建 Stream
一个数据源（如：集合、数组），获取一个流
中间操作
一个中间操作链，对数据源的数据进行处理
终止操作(终端操作)
一个终止操作，执行中间操作链，并产生结果
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201207110728432.png)
#### 5.2.1 创建 Stream
```markdown
Java8 中的 Collection 接口被扩展，提供了
两个获取流的方法：
default Stream<E> stream() : 返回一个顺序流
default Stream<E> parallelStream() : 返回一个并行流
```
#### 5.2.2 由数组创建流
```markdown
Java8 中的 Arrays 的静态方法 stream() 可
以获取数组流：
static <T> Stream<T> stream(T[] array): 返回一个流
重载形式，能够处理对应基本类型的数组：
public static IntStream stream(int[] array)
public static LongStream stream(long[] array)
public static DoubleStream stream(double[] array)
```
#### 5.2.3 由值创建流
```markdown
可以使用静态方法 Stream.of(), 通过显示值
创建一个流。它可以接收任意数量的参数。
public static<T> Stream<T> of(T... values) : 返回一个流
```
#### 5.2.4 由函数创建流：创建无限流
```markdown
可以使用静态方法 Stream.iterate() 和
Stream.generate(), 创建无限流。
```
```markdown
迭代
public static<T> Stream<T> iterate(final T seed, final
UnaryOperator<T> f)
生成
public static<T> Stream<T> generate(Supplier<T> s) : 
```

#### 5.2.5 Stream 的中间操作
```markdown
多个中间操作可以连接起来形成一个流水线，除非流水
线上触发终止操作，否则中间操作不会执行任何的处理！
而在终止操作时一次性全部处理，称为“惰性求值”。
```

##### 5.2.5.1 筛选与切片 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201207111236515.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 5.2.5.2 映射
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201207111304919.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 5.2.5.3 排序
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201207111334290.png)
#### 5.2.6 Stream 的终止操作
```markdown
终端操作会从流的流水线生成结果。其结果可以是任何不是流的
值，例如：List、Integer，甚至是 void 。
```
```java
package com.yxj.java8;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Iterator;
import java.util.List;
import java.util.stream.Stream;

import org.junit.Test;

/*
 * 一、Stream API 的操作步骤：
 * 
 * 1. 创建 Stream
 * 
 * 2. 中间操作
 * 
 * 3. 终止操作(终端操作)
 */
public class TestStreamaAPI {
	
	//1. 创建 Stream
	@Test
	public void test1(){
		//1. Collection 提供了两个方法  stream() 与 parallelStream()
		List<String> list = new ArrayList<>();
		Stream<String> stream = list.stream(); //获取一个顺序流
		Stream<String> parallelStream = list.parallelStream(); //获取一个并行流
		
		//2. 通过 Arrays 中的 stream() 获取一个数组流
		Integer[] nums = new Integer[10];
		Stream<Integer> stream1 = Arrays.stream(nums);
		
		//3. 通过 Stream 类中静态方法 of()
		Stream<Integer> stream2 = Stream.of(1,2,3,4,5,6);
		
		//4. 创建无限流
		//迭代
		Stream<Integer> stream3 = Stream.iterate(0, (x) -> x + 2).limit(10);
		stream3.forEach(System.out::println);
		
		//生成
		Stream<Double> stream4 = Stream.generate(Math::random).limit(2);
		stream4.forEach(System.out::println);
		
	}
	
	//2. 中间操作
	List<Employee> emps = Arrays.asList(
			new Employee(102, "李四", 59, 6666.66),
			new Employee(101, "张三", 18, 9999.99),
			new Employee(103, "王五", 28, 3333.33),
			new Employee(104, "赵六", 8, 7777.77),
			new Employee(104, "赵六", 8, 7777.77),
			new Employee(104, "赵六", 8, 7777.77),
			new Employee(105, "田七", 38, 5555.55)
	);
	
	/*
	  筛选与切片
		filter——接收 Lambda ， 从流中排除某些元素。
		limit——截断流，使其元素不超过给定数量。
		skip(n) —— 跳过元素，返回一个扔掉了前 n 个元素的流。若流中元素不足 n 个，则返回一个空流。与 limit(n) 互补
		distinct——筛选，通过流所生成元素的 hashCode() 和 equals() 去除重复元素
	 */
	
	//内部迭代：迭代操作 Stream API 内部完成
	@Test
	public void test2(){
		//所有的中间操作不会做任何的处理
		Stream<Employee> stream = emps.stream()
			.filter((e) -> {
				System.out.println("测试中间操作");
				return e.getAge() <= 35;
			});
		
		//只有当做终止操作时，所有的中间操作会一次性的全部执行，称为“惰性求值”
		stream.forEach(System.out::println);
	}
	
	//外部迭代
	@Test
	public void test3(){
		Iterator<Employee> it = emps.iterator();
		
		while(it.hasNext()){
			System.out.println(it.next());
		}
	}
	
	@Test
	public void test4(){
		emps.stream()
			.filter((e) -> {
				System.out.println("短路！"); // &&  ||
				return e.getSalary() >= 5000;
			}).limit(3)
			.forEach(System.out::println);
	}
	
	@Test
	public void test5(){
		emps.parallelStream()
			.filter((e) -> e.getSalary() >= 5000)
			.skip(2)
			.forEach(System.out::println);
	}
	
	@Test
	public void test6(){
		emps.stream()
			.distinct()
			.forEach(System.out::println);
	}
}

```

```java
package com.yxj.java8;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.stream.Stream;

import org.junit.Test;

/*
 * 一、 Stream 的操作步骤
 * 
 * 1. 创建 Stream
 * 
 * 2. 中间操作
 * 
 * 3. 终止操作
 */
public class TestStreamAPI1 {
	
	List<Employee> emps = Arrays.asList(
			new Employee(102, "李四", 59, 6666.66),
			new Employee(101, "张三", 18, 9999.99),
			new Employee(103, "王五", 28, 3333.33),
			new Employee(104, "赵六", 8, 7777.77),
			new Employee(104, "赵六", 8, 7777.77),
			new Employee(104, "赵六", 8, 7777.77),
			new Employee(105, "田七", 38, 5555.55)
	);
	
	//2. 中间操作
	/*
		映射
		map——接收 Lambda ， 将元素转换成其他形式或提取信息。接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素。
		flatMap——接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流
	 */
	@Test
	public void test1(){
		Stream<String> str = emps.stream()
			.map((e) -> e.getName());
		
		System.out.println("-------------------------------------------");
		
		List<String> strList = Arrays.asList("aaa", "bbb", "ccc", "ddd", "eee");
		
		Stream<String> stream = strList.stream()
			   .map(String::toUpperCase);
		
		stream.forEach(System.out::println);
		
		Stream<Stream<Character>> stream2 = strList.stream()
			   .map(TestStreamAPI1::filterCharacter);
		
		stream2.forEach((sm) -> {
			sm.forEach(System.out::println);
		});
		
		System.out.println("---------------------------------------------");
		
		Stream<Character> stream3 = strList.stream()
			   .flatMap(TestStreamAPI1::filterCharacter);
		
		stream3.forEach(System.out::println);
	}

	public static Stream<Character> filterCharacter(String str){
		List<Character> list = new ArrayList<>();
		
		for (Character ch : str.toCharArray()) {
			list.add(ch);
		}
		
		return list.stream();
	}
	
	/*
		sorted()——自然排序
		sorted(Comparator com)——定制排序
	 */
	@Test
	public void test2(){
		emps.stream()
			.map(Employee::getName)
			.sorted()
			.forEach(System.out::println);
		
		System.out.println("------------------------------------");
		
		emps.stream()
			.sorted((x, y) -> {
				if(x.getAge() == y.getAge()){
					return x.getName().compareTo(y.getName());
				}else{
					return Integer.compare(x.getAge(), y.getAge());
				}
			}).forEach(System.out::println);
	}
}
```
##### 5.2.6.1 查找与匹配
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201207111447916.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201207111503597.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
package com.yxj.java8;

import java.util.Arrays;
import java.util.List;
import java.util.Optional;
import java.util.stream.Stream;

import org.junit.Test;

import com.yxj.java8.Employee.Status;

/*
 * 一、 Stream 的操作步骤
 * 
 * 1. 创建 Stream
 * 
 * 2. 中间操作
 * 
 * 3. 终止操作
 */
public class TestStreamAPI2 {
	
	List<Employee> emps = Arrays.asList(
			new Employee(102, "李四", 59, 6666.66, Status.BUSY),
			new Employee(101, "张三", 18, 9999.99, Status.FREE),
			new Employee(103, "王五", 28, 3333.33, Status.VOCATION),
			new Employee(104, "赵六", 8, 7777.77, Status.BUSY),
			new Employee(104, "赵六", 8, 7777.77, Status.FREE),
			new Employee(104, "赵六", 8, 7777.77, Status.FREE),
			new Employee(105, "田七", 38, 5555.55, Status.BUSY)
	);
	
	//3. 终止操作
	/*
		allMatch——检查是否匹配所有元素
		anyMatch——检查是否至少匹配一个元素
		noneMatch——检查是否没有匹配的元素
		findFirst——返回第一个元素
		findAny——返回当前流中的任意元素
		count——返回流中元素的总个数
		max——返回流中最大值
		min——返回流中最小值
	 */
	@Test
	public void test1(){
			boolean bl = emps.stream()
				.allMatch((e) -> e.getStatus().equals(Status.BUSY));
			
			System.out.println(bl);
			
			boolean bl1 = emps.stream()
				.anyMatch((e) -> e.getStatus().equals(Status.BUSY));
			
			System.out.println(bl1);
			
			boolean bl2 = emps.stream()
				.noneMatch((e) -> e.getStatus().equals(Status.BUSY));
			
			System.out.println(bl2);
	}
	
	@Test
	public void test2(){
		Optional<Employee> op = emps.stream()
			.sorted((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary()))
			.findFirst();
		
		System.out.println(op.get());
		
		System.out.println("--------------------------------");
		
		Optional<Employee> op2 = emps.parallelStream()
			.filter((e) -> e.getStatus().equals(Status.FREE))
			.findAny();
		
		System.out.println(op2.get());
	}
	
	@Test
	public void test3(){
		long count = emps.stream()
						 .filter((e) -> e.getStatus().equals(Status.FREE))
						 .count();
		
		System.out.println(count);
		
		Optional<Double> op = emps.stream()
			.map(Employee::getSalary)
			.max(Double::compare);
		
		System.out.println(op.get());
		
		Optional<Employee> op2 = emps.stream()
			.min((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary()));
		
		System.out.println(op2.get());
	}
	
	//注意：流进行了终止操作后，不能再次使用
	@Test
	public void test4(){
		Stream<Employee> stream = emps.stream()
		 .filter((e) -> e.getStatus().equals(Status.FREE));
		
		long count = stream.count();
		
		stream.map(Employee::getSalary)
			.max(Double::compare);
	}
}
```
##### 5.2.6.2 归约
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201207111532931.png)

```markdown
备注：map 和 reduce 的连接通常称为 map-reduce 模式，
因 Google 用它
来进行网络搜索而出名。
```
##### 5.2.6.3 收集
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201207111624884.png)

```markdown
Collector 接口中方法的实现决定了如何对流执行收
集操作(如收集到 List、Set、Map)。但是 Collectors 实
用类提供了很多静态
方法，可以方便地创建常见收集器实例，具体方法与实例如下表：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201207111653490.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201207111709814.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
package com.yxj.java8;

import java.util.Arrays;
import java.util.DoubleSummaryStatistics;
import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.Optional;
import java.util.Set;
import java.util.stream.Collectors;

import org.junit.Test;

import com.yxj.java8.Employee.Status;

public class TestStreamAPI3 {
	
	List<Employee> emps = Arrays.asList(
			new Employee(102, "李四", 79, 6666.66, Status.BUSY),
			new Employee(101, "张三", 18, 9999.99, Status.FREE),
			new Employee(103, "王五", 28, 3333.33, Status.VOCATION),
			new Employee(104, "赵六", 8, 7777.77, Status.BUSY),
			new Employee(104, "赵六", 8, 7777.77, Status.FREE),
			new Employee(104, "赵六", 8, 7777.77, Status.FREE),
			new Employee(105, "田七", 38, 5555.55, Status.BUSY)
	);
	
	//3. 终止操作
	/*
		归约
		reduce(T identity, BinaryOperator) / reduce(BinaryOperator) ——可以将流中元素反复结合起来，得到一个值。
	 */
	@Test
	public void test1(){
		List<Integer> list = Arrays.asList(1,2,3,4,5,6,7,8,9,10);
		
		Integer sum = list.stream()
			.reduce(0, (x, y) -> x + y);
		
		System.out.println(sum);
		
		System.out.println("----------------------------------------");
		
		Optional<Double> op = emps.stream()
			.map(Employee::getSalary)
			.reduce(Double::sum);
		
		System.out.println(op.get());
	}
	
	//需求：搜索名字中 “六” 出现的次数
	@Test
	public void test2(){
		Optional<Integer> sum = emps.stream()
			.map(Employee::getName)
			.flatMap(TestStreamAPI1::filterCharacter)
			.map((ch) -> {
				if(ch.equals('六'))
					return 1;
				else 
					return 0;
			}).reduce(Integer::sum);
		
		System.out.println(sum.get());
	}
	
	//collect——将流转换为其他形式。接收一个 Collector接口的实现，用于给Stream中元素做汇总的方法
	@Test
	public void test3(){
		List<String> list = emps.stream()
			.map(Employee::getName)
			.collect(Collectors.toList());
		
		list.forEach(System.out::println);
		
		System.out.println("----------------------------------");
		
		Set<String> set = emps.stream()
			.map(Employee::getName)
			.collect(Collectors.toSet());
		
		set.forEach(System.out::println);

		System.out.println("----------------------------------");
		
		HashSet<String> hs = emps.stream()
			.map(Employee::getName)
			.collect(Collectors.toCollection(HashSet::new));
		
		hs.forEach(System.out::println);
	}
	
	@Test
	public void test4(){
		Optional<Double> max = emps.stream()
			.map(Employee::getSalary)
			.collect(Collectors.maxBy(Double::compare));
		
		System.out.println(max.get());
		
		Optional<Employee> op = emps.stream()
			.collect(Collectors.minBy((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary())));
		
		System.out.println(op.get());
		
		Double sum = emps.stream()
			.collect(Collectors.summingDouble(Employee::getSalary));
		
		System.out.println(sum);
		
		Double avg = emps.stream()
			.collect(Collectors.averagingDouble(Employee::getSalary));
		
		System.out.println(avg);
		
		Long count = emps.stream()
			.collect(Collectors.counting());
		
		System.out.println(count);
		
		System.out.println("--------------------------------------------");
		
		DoubleSummaryStatistics dss = emps.stream()
			.collect(Collectors.summarizingDouble(Employee::getSalary));
		
		System.out.println(dss.getMax());
	}
	
	//分组
	@Test
	public void test5(){
		Map<Status, List<Employee>> map = emps.stream()
			.collect(Collectors.groupingBy(Employee::getStatus));
		
		System.out.println(map);
	}
	
	//多级分组
	@Test
	public void test6(){
		Map<Status, Map<String, List<Employee>>> map = emps.stream()
			.collect(Collectors.groupingBy(Employee::getStatus, Collectors.groupingBy((e) -> {
				if(e.getAge() >= 60)
					return "老年";
				else if(e.getAge() >= 35)
					return "中年";
				else
					return "成年";
			})));
		
		System.out.println(map);
	}
	
	//分区
	@Test
	public void test7(){
		Map<Boolean, List<Employee>> map = emps.stream()
			.collect(Collectors.partitioningBy((e) -> e.getSalary() >= 5000));
		
		System.out.println(map);
	}
	
	//
	@Test
	public void test8(){
		String str = emps.stream()
			.map(Employee::getName)
			.collect(Collectors.joining("," , "----", "----"));
		
		System.out.println(str);
	}
	
	@Test
	public void test9(){
		Optional<Double> sum = emps.stream()
			.map(Employee::getSalary)
			.collect(Collectors.reducing(Double::sum));
		
		System.out.println(sum.get());
	}
}
```
```java
package com.yxj.exer;
//交易员类
public class Trader {

	private String name;
	private String city;

	public Trader() {
	}

	public Trader(String name, String city) {
		this.name = name;
		this.city = city;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getCity() {
		return city;
	}

	public void setCity(String city) {
		this.city = city;
	}

	@Override
	public String toString() {
		return "Trader [name=" + name + ", city=" + city + "]";
	}

}



package com.yxj.exer;
//交易类
public class Transaction {

	private Trader trader;
	private int year;
	private int value;

	public Transaction() {
	}

	public Transaction(Trader trader, int year, int value) {
		this.trader = trader;
		this.year = year;
		this.value = value;
	}

	public Trader getTrader() {
		return trader;
	}

	public void setTrader(Trader trader) {
		this.trader = trader;
	}

	public int getYear() {
		return year;
	}

	public void setYear(int year) {
		this.year = year;
	}

	public int getValue() {
		return value;
	}

	public void setValue(int value) {
		this.value = value;
	}

	@Override
	public String toString() {
		return "Transaction [trader=" + trader + ", year=" + year + ", value="
				+ value + "]";
	}

}


package com.yxj.exer;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.Optional;
import java.util.stream.Stream;

import org.junit.Before;
import org.junit.Test;

public class TestTransaction {
	
	List<Transaction> transactions = null;
	
	@Before
	public void before(){
		Trader raoul = new Trader("Raoul", "Cambridge");
		Trader mario = new Trader("Mario", "Milan");
		Trader alan = new Trader("Alan", "Cambridge");
		Trader brian = new Trader("Brian", "Cambridge");
		
		transactions = Arrays.asList(
				new Transaction(brian, 2011, 300),
				new Transaction(raoul, 2012, 1000),
				new Transaction(raoul, 2011, 400),
				new Transaction(mario, 2012, 710),
				new Transaction(mario, 2012, 700),
				new Transaction(alan, 2012, 950)
		);
	}
	
	//1. 找出2011年发生的所有交易， 并按交易额排序（从低到高）
	@Test
	public void test1(){
		transactions.stream()
					.filter((t) -> t.getYear() == 2011)
					.sorted((t1, t2) -> Integer.compare(t1.getValue(), t2.getValue()))
					.forEach(System.out::println);
	}
	
	//2. 交易员都在哪些不同的城市工作过？
	@Test
	public void test2(){
		transactions.stream()
					.map((t) -> t.getTrader().getCity())
					.distinct()
					.forEach(System.out::println);
	}
	
	//3. 查找所有来自剑桥的交易员，并按姓名排序
	@Test
	public void test3(){
		transactions.stream()
					.filter((t) -> t.getTrader().getCity().equals("Cambridge"))
					.map(Transaction::getTrader)
					.sorted((t1, t2) -> t1.getName().compareTo(t2.getName()))
					.distinct()
					.forEach(System.out::println);
	}
	
	//4. 返回所有交易员的姓名字符串，按字母顺序排序
	@Test
	public void test4(){
		transactions.stream()
					.map((t) -> t.getTrader().getName())
					.sorted()
					.forEach(System.out::println);
		
		System.out.println("-----------------------------------");
		
		String str = transactions.stream()
					.map((t) -> t.getTrader().getName())
					.sorted()
					.reduce("", String::concat);
		
		System.out.println(str);
		
		System.out.println("------------------------------------");
		
		transactions.stream()
					.map((t) -> t.getTrader().getName())
					.flatMap(TestTransaction::filterCharacter)
					.sorted((s1, s2) -> s1.compareToIgnoreCase(s2))
					.forEach(System.out::print);
	}
	
	public static Stream<String> filterCharacter(String str){
		List<String> list = new ArrayList<>();
		
		for (Character ch : str.toCharArray()) {
			list.add(ch.toString());
		}
		
		return list.stream();
	}
	
	//5. 有没有交易员是在米兰工作的？
	@Test
	public void test5(){
		boolean bl = transactions.stream()
					.anyMatch((t) -> t.getTrader().getCity().equals("Milan"));
		
		System.out.println(bl);
	}
	
	
	//6. 打印生活在剑桥的交易员的所有交易额
	@Test
	public void test6(){
		Optional<Integer> sum = transactions.stream()
					.filter((e) -> e.getTrader().getCity().equals("Cambridge"))
					.map(Transaction::getValue)
					.reduce(Integer::sum);
		
		System.out.println(sum.get());
	}
	
	
	//7. 所有交易中，最高的交易额是多少
	@Test
	public void test7(){
		Optional<Integer> max = transactions.stream()
					.map((t) -> t.getValue())
					.max(Integer::compare);
		
		System.out.println(max.get());
	}
	
	//8. 找到交易额最小的交易
	@Test
	public void test8(){
		Optional<Transaction> op = transactions.stream()
					.min((t1, t2) -> Integer.compare(t1.getValue(), t2.getValue()));
		
		System.out.println(op.get());
	}

}
```
```java
package com.yxj.exer;

import java.util.Arrays;
import java.util.List;
import java.util.Optional;

import org.junit.Test;

import com.yxj.java8.Employee;
import com.yxj.java8.Employee.Status;

public class TestStreamAPI {
	
	/*
	  	1.	给定一个数字列表，如何返回一个由每个数的平方构成的列表呢？
		，给定【1，2，3，4，5】， 应该返回【1，4，9，16，25】。
	 */
	@Test
	public void test1(){
		Integer[] nums = new Integer[]{1,2,3,4,5};
		
		Arrays.stream(nums)
			  .map((x) -> x * x)
			  .forEach(System.out::println);
	}

	/*
	 2.	怎样用 map 和 reduce 方法数一数流中有多少个Employee呢？
	 */
	List<Employee> emps = Arrays.asList(
			new Employee(102, "李四", 59, 6666.66, Status.BUSY),
			new Employee(101, "张三", 18, 9999.99, Status.FREE),
			new Employee(103, "王五", 28, 3333.33, Status.VOCATION),
			new Employee(104, "赵六", 8, 7777.77, Status.BUSY),
			new Employee(104, "赵六", 8, 7777.77, Status.FREE),
			new Employee(104, "赵六", 8, 7777.77, Status.FREE),
			new Employee(105, "田七", 38, 5555.55, Status.BUSY)
	);
	
	@Test
	public void test2(){
		Optional<Integer> count = emps.stream()
			.map((e) -> 1)
			.reduce(Integer::sum);
		
		System.out.println(count.get());
	}
}
```
### 5.3 并行流与串行流
```markdown
并行流就是把一个内容分成多个数据块，并用不同的线程分
别处理每个数据块的流。
Java 8 中将并行进行了优化，我们可以很容易的对数据进行并
行操作。Stream API 可以声明性地通过 parallel() 与
sequential() 在并行流与顺序流之间进行切换。
```
#### 5.3.1 了解 Fork/Join 框架
```markdown
Fork/Join 框架：就是在必要的情况下，将一个大任务，进行
拆分(fork)成若干个小任务（拆到不可再拆时），再将一个个
的小任务运算的结果进行 join 汇总
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201207112037449.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 5.3.2 Fork/Join 框架与传统线程池的区别
```markdown
采用 “工作窃取”模式（work-stealing）：
当执行新的任务时它可以将其拆分分成更小的任务执行，
并将小任务加到线程队列中，然后再从一个随机线程的队
列中偷一个并把它放在自己的队列中。相对于一般的线
程池实现,fork/join框架的优势体现在对其中包含的任务的
处理方式上.在一般的线程池中,如果一个线程正在执行的
任务由于某些原因无法继续运行,那么该线程会处于等待
状态.而在fork/join框架实现中,如果某个子问题由于等
待另外一个子问题的完成而无法继续运行.那么处理该子
问题的线程会主动寻找其他尚未运行的子问题来执行.这
种方式减少了线程
的等待时间,提高了性能.
```
```java
package com.yxj.java8;

import java.util.concurrent.RecursiveTask;

public class ForkJoinCalculate extends RecursiveTask<Long>{

	/**
	 * 
	 */
	private static final long serialVersionUID = 13475679780L;
	
	private long start;
	private long end;
	
	private static final long THRESHOLD = 10000L; //临界值
	
	public ForkJoinCalculate(long start, long end) {
		this.start = start;
		this.end = end;
	}
	
	@Override
	protected Long compute() {
		long length = end - start;
		
		if(length <= THRESHOLD){
			long sum = 0;
			
			for (long i = start; i <= end; i++) {
				sum += i;
			}
			
			return sum;
		}else{
			long middle = (start + end) / 2;
			
			ForkJoinCalculate left = new ForkJoinCalculate(start, middle);
			left.fork(); //拆分，并将该子任务压入线程队列
			
			ForkJoinCalculate right = new ForkJoinCalculate(middle+1, end);
			right.fork();
			
			return left.join() + right.join();
		}
		
	}

}
```
```java
package com.yxj.java8;

import java.util.concurrent.ForkJoinPool;
import java.util.concurrent.ForkJoinTask;
import java.util.stream.LongStream;

import org.junit.Test;

public class TestForkJoin {
	
	@Test
	public void test1(){
		long start = System.currentTimeMillis();
		
		ForkJoinPool pool = new ForkJoinPool();
		ForkJoinTask<Long> task = new ForkJoinCalculate(0L, 10000000000L);
		
		long sum = pool.invoke(task);
		System.out.println(sum);
		
		long end = System.currentTimeMillis();
		
		System.out.println("耗费的时间为: " + (end - start)); //112-1953-1988-2654-2647-20663-113808
	}
	
	@Test
	public void test2(){
		long start = System.currentTimeMillis();
		
		long sum = 0L;
		
		for (long i = 0L; i <= 10000000000L; i++) {
			sum += i;
		}
		
		System.out.println(sum);
		
		long end = System.currentTimeMillis();
		
		System.out.println("耗费的时间为: " + (end - start)); //34-3174-3132-4227-4223-31583
	}
	
	@Test
	public void test3(){
		long start = System.currentTimeMillis();
		
		Long sum = LongStream.rangeClosed(0L, 10000000000L)
							 .parallel()
							 .sum();
		
		System.out.println(sum);
		
		long end = System.currentTimeMillis();
		
		System.out.println("耗费的时间为: " + (end - start)); //2061-2053-2086-18926
	}

}
```
### 5.4 新时间日期 API
```markdown
使用LocalDate、LocalTime、LocalDateTime
LocalDate、LocalTime、LocalDateTime 类的实
例是不可变的对象，分别表示使用ISO-8601日
历系统的日期、时间、日期和时间。它们提供
了简单的日期或时间，并不包含当前的时间信
息。也不包含与时区相关的信息。
注：ISO-8601日历系统是国际标准化组织制定的现代
公民的日期和时间的表示法
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201207112231744.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 5.4.1 Instant 时间戳
```markdown
用于“时间戳”的运算。它是以Unix元年(传统
的设定为UTC时区1970年1月1日午夜时分)开始
所经历的描述进行运算
```
#### 5.4.2 Duration 和 Period
```markdown
Duration:用于计算两个“时间”间隔
Period:用于计算两个“日期”间隔
```
#### 5.4.3 日期的操纵
```markdown
TemporalAdjuster : 时间校正器。有时我们可能需要获
取例如：将日期调整到“下个周日”等操作。
TemporalAdjusters : 该类通过静态方法提供了大量的常
用 TemporalAdjuster 的实现。
例如获取下个周日：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201207112642865.png)
#### 5.4.4 解析与格式化
```markdown
java.time.format.DateTimeFormatter 类：该类提供了三种
格式化方法：
预定义的标准格式
语言环境相关的格式
自定义的格式
```
#### 5.4.5 时区的处理
```markdown
Java8 中加入了对时区的支持，带时区的时间为分别为：
ZonedDate、ZonedTime、ZonedDateTime
其中每个时区都对应着 ID，地区ID都为 “{区域}/{城市}”的格式
例如 ：Asia/Shanghai 等
ZoneId：该类中包含了所有的时区信息
getAvailableZoneIds() : 可以获取所有时区时区信息
of(id) : 用指定的时区信息获取 ZoneId 对象
```
#### 5.4.6 与传统日期处理的转换
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020120711292963.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
package com.yxj.java8;

import java.time.DayOfWeek;
import java.time.Duration;
import java.time.Instant;
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.OffsetDateTime;
import java.time.Period;
import java.time.ZoneId;
import java.time.ZoneOffset;
import java.time.ZonedDateTime;
import java.time.format.DateTimeFormatter;
import java.time.temporal.TemporalAdjusters;
import java.util.Set;

import org.junit.Test;

public class TestLocalDateTime {
	
	//6.ZonedDate、ZonedTime、ZonedDateTime ： 带时区的时间或日期
	@Test
	public void test7(){
		LocalDateTime ldt = LocalDateTime.now(ZoneId.of("Asia/Shanghai"));
		System.out.println(ldt);
		
		ZonedDateTime zdt = ZonedDateTime.now(ZoneId.of("US/Pacific"));
		System.out.println(zdt);
	}
	
	@Test
	public void test6(){
		Set<String> set = ZoneId.getAvailableZoneIds();
		set.forEach(System.out::println);
	}

	
	//5. DateTimeFormatter : 解析和格式化日期或时间
	@Test
	public void test5(){
//		DateTimeFormatter dtf = DateTimeFormatter.ISO_LOCAL_DATE;
		
		DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy年MM月dd日 HH:mm:ss E");
		
		LocalDateTime ldt = LocalDateTime.now();
		String strDate = ldt.format(dtf);
		
		System.out.println(strDate);
		
		LocalDateTime newLdt = ldt.parse(strDate, dtf);
		System.out.println(newLdt);
	}
	
	//4. TemporalAdjuster : 时间校正器
	@Test
	public void test4(){
	LocalDateTime ldt = LocalDateTime.now();
		System.out.println(ldt);
		
		LocalDateTime ldt2 = ldt.withDayOfMonth(10);
		System.out.println(ldt2);
		
		LocalDateTime ldt3 = ldt.with(TemporalAdjusters.next(DayOfWeek.SUNDAY));
		System.out.println(ldt3);
		
		//自定义：下一个工作日
		LocalDateTime ldt5 = ldt.with((l) -> {
			LocalDateTime ldt4 = (LocalDateTime) l;
			
			DayOfWeek dow = ldt4.getDayOfWeek();
			
			if(dow.equals(DayOfWeek.FRIDAY)){
				return ldt4.plusDays(3);
			}else if(dow.equals(DayOfWeek.SATURDAY)){
				return ldt4.plusDays(2);
			}else{
				return ldt4.plusDays(1);
			}
		});
		
		System.out.println(ldt5);
		
	}
	
	//3.
	//Duration : 用于计算两个“时间”间隔
	//Period : 用于计算两个“日期”间隔
	@Test
	public void test3(){
		Instant ins1 = Instant.now();
		
		System.out.println("--------------------");
		try {
			Thread.sleep(1000);
		} catch (InterruptedException e) {
		}
		
		Instant ins2 = Instant.now();
		
		System.out.println("所耗费时间为：" + Duration.between(ins1, ins2));
		
		System.out.println("----------------------------------");
		
		LocalDate ld1 = LocalDate.now();
		LocalDate ld2 = LocalDate.of(2011, 1, 1);
		
		Period pe = Period.between(ld2, ld1);
		System.out.println(pe.getYears());
		System.out.println(pe.getMonths());
		System.out.println(pe.getDays());
	}
	
	//2. Instant : 时间戳。 （使用 Unix 元年  1970年1月1日 00:00:00 所经历的毫秒值）
	@Test
	public void test2(){
		Instant ins = Instant.now();  //默认使用 UTC 时区
		System.out.println(ins);
		
		OffsetDateTime odt = ins.atOffset(ZoneOffset.ofHours(8));
		System.out.println(odt);
		
		System.out.println(ins.getNano());
		
		Instant ins2 = Instant.ofEpochSecond(5);
		System.out.println(ins2);
	}
	
	//1. LocalDate、LocalTime、LocalDateTime
	@Test
	public void test1(){
		LocalDateTime ldt = LocalDateTime.now();
		System.out.println(ldt);
		
		LocalDateTime ld2 = LocalDateTime.of(2016, 11, 21, 10, 10, 10);
		System.out.println(ld2);
		
		LocalDateTime ldt3 = ld2.plusYears(20);
		System.out.println(ldt3);
		
		LocalDateTime ldt4 = ld2.minusMonths(2);
		System.out.println(ldt4);
		
		System.out.println(ldt.getYear());
		System.out.println(ldt.getMonthValue());
		System.out.println(ldt.getDayOfMonth());
		System.out.println(ldt.getHour());
		System.out.println(ldt.getMinute());
		System.out.println(ldt.getSecond());
	}

}

```
#### 5.4.7 接口中的默认方法与静态方法
##### 5.4.7.1  接口中的默认方法
```markdown
Java 8中允许接口中包含具有具体实现的方法，该方法称为
“默认方法”，默认方法使用 default 关键字修饰。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201207113036823.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
接口默认方法的”类优先”原则
若一个接口中定义了一个默认方法，而另外一个父类或接口中
又定义了一个同名的方法时
选择父类中的方法。如果一个父类提供了具体的实现，那么
接口中具有相同名称和参数的默认方法会被忽略。
接口冲突。如果一个父接口提供一个默认方法，而另一个接
也提供了一个具有相同名称和参数列表的方法（不管方法
是否是默认方法），那么必须覆盖该方法来解决冲突
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201207113128606.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 5.4.7.2 接口中的静态方法
```markdown
Java8 中，接口中允许添加静态方法。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201207113306674.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 6 其他新特性
####  6.1 Optional 类
```markdown
Optional<T> 类(java.util.Optional) 是一个容器类，代表一个值存
在或不存在，原来用 null 表示一个值不存在，现在 Optional 可
以更好的表达这个概念。并且可以避免空指针异常。
常用方法：
Optional.of(T t) : 创建一个 Optional 实例
Optional.empty() : 创建一个空的 Optional 实例
Optional.ofNullable(T t):若 t 不为 null,创建 Optional 实例,否则创建空实例
isPresent() : 判断是否包含值
orElse(T t) : 如果调用对象包含值，返回该值，否则返回t
orElseGet(Supplier s) :如果调用对象包含值，返回该值，否则返回 s 获取的值
map(Function f): 如果有值对其处理，并返回处理后的Optional，否则返回 Optional.empty()
flatMap(Function mapper):与 map 类似，要求返回值必须是Optional
```
```java
package com.yxj.java8;

import java.util.Optional;

import org.junit.Test;

/*
 * 一、Optional 容器类：用于尽量避免空指针异常
 * 	Optional.of(T t) : 创建一个 Optional 实例
 * 	Optional.empty() : 创建一个空的 Optional 实例
 * 	Optional.ofNullable(T t):若 t 不为 null,创建 Optional 实例,否则创建空实例
 * 	isPresent() : 判断是否包含值
 * 	orElse(T t) :  如果调用对象包含值，返回该值，否则返回t
 * 	orElseGet(Supplier s) :如果调用对象包含值，返回该值，否则返回 s 获取的值
 * 	map(Function f): 如果有值对其处理，并返回处理后的Optional，否则返回 Optional.empty()
 * 	flatMap(Function mapper):与 map 类似，要求返回值必须是Optional
 */
public class TestOptional {
	
	@Test
	public void test4(){
		Optional<Employee> op = Optional.of(new Employee(101, "张三", 18, 9999.99));
		
		Optional<String> op2 = op.map(Employee::getName);
		System.out.println(op2.get());
		
		Optional<String> op3 = op.flatMap((e) -> Optional.of(e.getName()));
		System.out.println(op3.get());
	}
	
	@Test
	public void test3(){
		Optional<Employee> op = Optional.ofNullable(new Employee());
		
		if(op.isPresent()){
			System.out.println(op.get());
		}
		
		Employee emp = op.orElse(new Employee("张三"));
		System.out.println(emp);
		
		Employee emp2 = op.orElseGet(() -> new Employee());
		System.out.println(emp2);
	}
	
	@Test
	public void test2(){
		/*Optional<Employee> op = Optional.ofNullable(null);
		System.out.println(op.get());*/
		
//		Optional<Employee> op = Optional.empty();
//		System.out.println(op.get());
	}

	@Test
	public void test1(){
		Optional<Employee> op = Optional.of(new Employee());
		Employee emp = op.get();
		System.out.println(emp);
	}
	
	@Test
	public void test5(){
		Man man = new Man();
		
		String name = getGodnessName(man);
		System.out.println(name);
	}
	
	//需求：获取一个男人心中女神的名字
	public String getGodnessName(Man man){
		if(man != null){
			Godness g = man.getGod();
			
			if(g != null){
				return g.getName();
			}
		}
		
		return "老师";
	}
	
	//运用 Optional 的实体类
	@Test
	public void test6(){
		Optional<Godness> godness = Optional.ofNullable(new Godness("林志玲"));
		
		Optional<NewMan> op = Optional.ofNullable(new NewMan(godness));
		String name = getGodnessName2(op);
		System.out.println(name);
	}
	
	public String getGodnessName2(Optional<NewMan> man){
		return man.orElse(new NewMan())
				  .getGodness()
				  .orElse(new Godness("老师"))
				  .getName();
	}
}

```
####  6.2 重复注解与类型注解
```markdown
Java 8对注解处理提供了两点改进：可重复的注解及可用于类
型的注解。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201207113748237.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复java8
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

