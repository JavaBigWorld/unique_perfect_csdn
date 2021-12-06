# JUC并发编程
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210715004139763.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1.1 Java JUC 简介
```markdown
在Java 5.0提供了java.util.concurrent （简称JUC ）
包，在此包中增加了在并发编程中很常用的实用工具
类，用于定义类似于线程的自定义子
系统，包括线程池、异步IO和轻量级任务框架。
提供可调的、灵活的线程池。还提供了设计用于
多线程上下文中的Collection实现等。
```
## 1.2 Lock锁
```markdown
在Java5.0之前，协调共享对象的访问时可以使用的机
制只有synchronized 和volatile 。Java 5.0后增加了一些
新的机制，但并不是一种替代内置锁的方法，而是当内
置锁不适用时，作为一种可选择的高级功能。

ReentrantLock 实现了Lock接口，并提供了与
synchronized 相同的互斥性和内存可见性。但相较于
synchronized 提供了更高的处理锁的灵活性。
```
```markdown
传统 Synchronized
```
```java
// 基本的卖票例子

import java.time.OffsetDateTime;
//Lock 接口

/**
 * 真正的多线程开发，公司中的开发，降低耦合性
 * 线程就是一个单独的资源类，没有任何附属的操作！
 * 1、 属性、方法
 */
public class SaleTicketDemo01 {
    public static void main(String[] args) {
// 并发：多线程操作同一个资源类, 把资源类丢入线程
        Ticket ticket = new Ticket();
// @FunctionalInterface 函数式接口，jdk1.8 lambda表达式 (参数)->{ 代码 }
        new Thread(() -> {
            for (int i = 1; i < 40; i++) {
                ticket.sale();
            }
        }, "A").start();
        new Thread(() -> {
            for (int i = 1; i < 40; i++) {
                ticket.sale();
            }
        }, "B").start();
        new Thread(() -> {
            for (int i = 1; i < 40; i++) {
                ticket.sale();
            }
        }, "C").start();
    }
}

// 资源类 OOP
class Ticket {
    // 属性、方法
    private int number = 30;

    // 卖票的方式
// synchronized 本质: 队列，锁
    public synchronized void sale() {
        if (number > 0) {
            System.out.println(Thread.currentThread().getName() + "卖出了" + (number--) + "票,剩余：" + number);
        }
    }
}
```
```markdown
Lock 接口
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322201158987.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
公平锁：十分公平：可以先来后到
非公平锁：十分不公平：可以插队(默认) 
```
```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
public class SaleTicketDemo02 {
    public static void main(String[] args) {
// 并发：多线程操作同一个资源类, 把资源类丢入线程
        Ticket2 ticket = new Ticket2();
// @FunctionalInterface 函数式接口，jdk1.8 lambda表达式 (参数)->{ 代码 }
        new Thread(()->{for (int i = 1; i < 40 ; i++)
            ticket.sale();},"A").start();
        new Thread(()->{for (int i = 1; i < 40 ; i++)
            ticket.sale();},"B").start();
        new Thread(()->{for (int i = 1; i < 40 ; i++)
            ticket.sale();},"C").start();
    }
}

// Lock三部曲
// 1、 new ReentrantLock();
// 2、 lock.lock(); // 加锁
// 3、 finally=> lock.unlock(); // 解锁
class Ticket2 {
    // 属性、方法
    private int number = 30;
    Lock lock = new ReentrantLock();
    public void sale(){
        lock.lock(); // 加锁
        try {
// 业务代码
            if (number>0){
                System.out.println(Thread.currentThread().getName()+"卖出了"+
                        (number--)+"票,剩余："+number);
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock(); // 解锁
        }
    }
}
```
```markdown
Synchronized 和 Lock 区别
1 Synchronized 内置的Java关键字， Lock 是一个Java类
2 Synchronized 无法判断获取锁的状态，Lock 可以判
断是否获取到了锁
3 Synchronized 会自动释放锁，lock 必须要手动释放锁！如
果不释放锁，死锁
4 Synchronized 线程 1（获得锁，阻塞）、线程2（等待，傻
傻的等）；Lock锁就不一定会等待下去；
5、Synchronized 可重入锁，不可以中断的，非公
平；Lock ，可重入锁，可以 判断锁，非公
平（可以自己设置）；
6、Synchronized 适合锁少量的代码同步问题，Lock 
适合锁大量的同步代码！
```
## 1.3 生产者和消费者问题
```markdown
生产者和消费者问题Synchronized版
```
```java
/**
 * 线程之间的通信问题：生产者和消费者问题！ 等待唤醒，通知唤醒
 * 线程交替执行 A B 操作同一个变量 num = 0
 * A num+1
 * B num-1
 */
public class A {
    public static void main(String[] args) {
        Data data = new Data();
        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"A").start();
        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"B").start();
    }
}
// 判断等待，业务，通知
class Data{ // 数字 资源类
    private int number = 0;
    //+1
    public synchronized void increment() throws InterruptedException {
        if (number!=0){ //0
// 等待
            this.wait();
        }
        number++;
        System.out.println(Thread.currentThread().getName()+"=>"+number);
// 通知其他线程，我+1完毕了
        this.notifyAll();
    }
    //-1
    public synchronized void decrement() throws InterruptedException {
        if (number==0){ // 1
//            问题存在，A B C D 4 个线程！ 虚假唤醒
//            if 改为 while 判断
// 等待
            this.wait();
        }
        number--;
        System.out.println(Thread.currentThread().getName()+"=>"+number);
// 通知其他线程，我-1完毕了
        this.notifyAll();
    }
}
```
```markdown
问题存在，A B C D 4 个线程！ 虚假唤醒
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322202522358.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
if 改为 while 判断
```

```java

/**
 * 线程之间的通信问题：生产者和消费者问题！ 等待唤醒，通知唤醒
 * 线程交替执行 A B 操作同一个变量 num = 0
 * A num+1
 * B num-1
 */
public class A {
    public static void main(String[] args) {
        Data data = new Data();
        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"A").start();
        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"B").start();
        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"C").start();
        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"D").start();
    }
}
// 判断等待，业务，通知
class Data{ // 数字 资源类
    private int number = 0;
    //+1
    public synchronized void increment() throws InterruptedException {
        while (number!=0){ //0
// 等待
            this.wait();
        }
        number++;
        System.out.println(Thread.currentThread().getName()+"=>"+number);
// 通知其他线程，我+1完毕了
        this.notifyAll();
    }
    //-1
    public synchronized void decrement() throws InterruptedException {
        while (number==0){ // 1
// 等待
            this.wait();
        }
        number--;
        System.out.println(Thread.currentThread().getName()+"=>"+number);
// 通知其他线程，我-1完毕了
        this.notifyAll();
    }
}
```
```markdown
JUC版的生产者和消费者问题
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322202706938.png)
```markdown
通过Lock找到Condition
```


```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
public class B {
    public static void main(String[] args) {
        Data2 data = new Data2();
        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"A").start();
        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"B").start();
        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"C").start();
        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"D").start();
    }
}
// 判断等待，业务，通知
class Data2{ // 数字 资源类
    private int number = 0;
    Lock lock = new ReentrantLock();
    Condition condition = lock.newCondition();
    //condition.await(); // 等待
//condition.signalAll(); // 唤醒全部
//+1
    public void increment() throws InterruptedException {
        lock.lock();
        try {
// 业务代码
            while (number!=0){ //0
// 等待
                condition.await();
            }
            number++;
            System.out.println(Thread.currentThread().getName()+"=>"+number);
// 通知其他线程，我+1完毕了
            condition.signalAll();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
    //-1
    public synchronized void decrement() throws InterruptedException {
        lock.lock();
        try {
            while (number==0){ // 1
// 等待
                condition.await();
            }
            number--;
            System.out.println(Thread.currentThread().getName()+"=>"+number);
// 通知其他线程，我-1完毕了
            condition.signalAll();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}
```

## 1.4 Condition 控制线程通信
```markdown
Condition接口描述了可能会与锁有关联的条件变量。这些变量在用
法上与使用 Object.wait 访问的隐式监视器类似，但提供了
更强大的功能。需要特别指出的是，单个 Lock 可能与多
个 Condition 对象关联。为了避免兼容性问题，Condition方
法的名称与对应的Object版
本中的不同。

在 Condition 对象中，与 wait、notify 和 notifyAll 方
法对应的分别是
await、signal 和 signalAll。

Condition 实例实质上被绑定到一个锁上。要为特定Lock实
例获得Condition 实例，请使用其newCondition() 方法。
```
## 1.5 线程按序交替
```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

/**
 * A 执行完调用B，B执行完调用C，C执行完调用A
 */
public class C {
    public static void main(String[] args) {
        Data3 data = new Data3();
        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                data.printA();
            }
        }, "A").start();
        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                data.printB();
            }
        }, "B").start();
        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                data.printC();
            }
        }, "C").start();
    }
}

class Data3 { // 资源类 Lock
    private Lock lock = new ReentrantLock();
    private Condition condition1 = lock.newCondition();
    private Condition condition2 = lock.newCondition();
    private Condition condition3 = lock.newCondition();
    private int number = 1; // 1A 2B 3C

    public void printA() {
        lock.lock();
        try {
// 业务，判断-> 执行-> 通知
            while (number != 1) {
// 等待
                condition1.await();
            }
            System.out.println(Thread.currentThread().getName() + "=>AAAAAAA");
// 唤醒，唤醒指定的人，B
            number = 2;
            condition2.signal();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

    public void printB() {
        lock.lock();
        try {
// 业务，判断-> 执行-> 通知
            while (number != 2) {
                condition2.await();
            }
            System.out.println(Thread.currentThread().getName() + "=>BBBBBBBBB");
// 唤醒，唤醒指定的人，c
            number = 3;
            condition3.signal();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

    public void printC() {
        lock.lock();
        try {
// 业务，判断-> 执行-> 通知
// 业务，判断-> 执行-> 通知
            while (number != 3) {
                condition3.await();
            }
            System.out.println(Thread.currentThread().getName() + "=>BBBBBBBBB");
// 唤醒，唤醒指定的人，c
            number = 1;
            condition1.signal();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}
```
## 1.6 线程八锁
```markdown
一个对象里面如果有多个synchronized方法，某一个时刻内，
只要一个线程去调用
其中的一个synchronized方法了，其它的线程都只能等待，
换句话说，某一个时刻内，只能有唯一一个线程去访问这些synchronized方法
锁的是当前对象this，被锁定后，其它的线程都不
能进入到当前对象的其它的
synchronized方法
加个普通方法后发现和同步锁无关
换成两个对象后，不是同一把锁了，情况立刻变化。
都换成静态同步方法后，情况又变化
所有的非静态同步方法用的都是同一把锁——实例对象
本身，也就是说如果一个实例对象的非静态同步方法获
取锁后，该实例对象的其他非静态同步方法必须等待获
取锁的方法释放锁后才能获取锁，可是别的实例对象的
非静态同步方法因为跟该实例对象的非静态同步方法用
的是不同的锁，所以毋须等待该实例对象已获取锁的非
静态同步方法释放锁就可以获取他们自己的锁。
所有的静态同步方法用的也是同一把锁——类对象本身，
这两把锁是两个不同的对象，所以静态同步方法与非
静态同步方法之间是不会有竞态条件的。但是一旦一个
静态同步方法获取锁后，其他的静态同步方法都必须等
待该方法释放锁后才能获取锁，而不管是同一个实例
对象的静态同步方法之间，还是不同的实例对象的静态同
步方法之间，只要它们同一个类的实例对象！
```
```java
/*
 * 题目：判断打印的 "one" or "two" ？
 * 
 * 1. 两个普通同步方法，两个线程，标准打印， 打印? //one  two
 * 2. 新增 Thread.sleep() 给 getOne() ,打印? //one  two
 * 3. 新增普通方法 getThree() , 打印? //three  one   two
 * 4. 两个普通同步方法，两个 Number 对象，打印?  //two  one
 * 5. 修改 getOne() 为静态同步方法，打印?  //two   one
 * 6. 修改两个方法均为静态同步方法，一个 Number 对象?  //one   two
 * 7. 一个静态同步方法，一个非静态同步方法，两个 Number 对象?  //two  one
 * 8. 两个静态同步方法，两个 Number 对象?   //one  two
 * 
 * 线程八锁的关键：
 * ①非静态方法的锁默认为  this,  静态方法的锁为 对应的 Class 实例
 * ②某一个时刻内，只能有一个线程持有锁，无论几个方法。
 */
public class TestThread8Monitor {
	
	public static void main(String[] args) {
		Number number = new Number();
		Number number2 = new Number();
		
		new Thread(new Runnable() {
			@Override
			public void run() {
				number.getOne();
			} 
		}).start();
		
		new Thread(new Runnable() {
			@Override
			public void run() {
//				number.getTwo();
				number2.getTwo();
			}
		}).start();
		
		/*new Thread(new Runnable() {
			@Override
			public void run() {
				number.getThree();
			}
		}).start();*/
		
	}

}

class Number{
	
	public static synchronized void getOne(){//Number.class
		try {
			Thread.sleep(3000);
		} catch (InterruptedException e) {
		}
		
		System.out.println("one");
	}
	
	public synchronized void getTwo(){//this
		System.out.println("two");
	}
	
	public void getThree(){
		System.out.println("three");
	}
	
}
```
## 1.7 集合类不安全

```markdown
List 不安全
```

```java
import java.util.*;
import java.util.concurrent.CopyOnWriteArrayList;
// java.util.ConcurrentModificationException 并发修改异常！
public class ListTest {
	public static void main(String[] args) {
// 并发下 ArrayList 不安全的吗，Synchronized；
/**
 * 解决方案；
 * 1、List<String> list = new Vector<>();
 * 2、List<String> list = Collections.synchronizedList(new ArrayList<>
 ());
 * 3、List<String> list = new CopyOnWriteArrayList<>()；
 */
// CopyOnWrite 写入时复制 COW 计算机程序设计领域的一种优化策略；
// 多个线程调用的时候，list，读取的时候，固定的，写入（覆盖）
// 在写入的时候避免覆盖，造成数据问题！
// 读写分离
// CopyOnWriteArrayList 比 Vector Nb 在哪里？
		List<String> list = new CopyOnWriteArrayList<>();
		for (int i = 1; i <= 10; i++) {
			new Thread(()->{
				list.add(UUID.randomUUID().toString().substring(0,5));
				System.out.println(list);
			},String.valueOf(i)).start();
		}
	
	}
	
}
```
```markdown
Set 不安全
```
```java
import java.util.Collections;
import java.util.HashSet;
import java.util.Set;
import java.util.UUID;
import java.util.concurrent.CopyOnWriteArraySet;
/**
 * 同理可证 ： ConcurrentModificationException
 * //1、Set<String> set = Collections.synchronizedSet(new HashSet<>());
 * //2、
 */
public class SetTest {
	public static void main(String[] args) {
// Set<String> set = new HashSet<>();
// Set<String> set = Collections.synchronizedSet(new HashSet<>());
		Set<String> set = new CopyOnWriteArraySet<>();
		for (int i = 1; i <=30 ; i++) {
			new Thread(()->{
				set.add(UUID.randomUUID().toString().substring(0,5));
				System.out.println(set);
			},String.valueOf(i)).start();
		}
	}
}

```
```markdown
hashSet 底层是什么？
```
```java
public HashSet(){
        map=new HashMap<>();
        }
// add set 本质就是 map key是无法重复的！
public boolean add(E e){
        return map.put(e,PRESENT)==null;
}
private static final Object PRESENT=new Object(); // 不变得值！
```
```markdown
Map 不安全
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322210611668.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
import java.util.Collections;
import java.util.HashMap;
import java.util.Map;
import java.util.UUID;
import java.util.concurrent.ConcurrentHashMap;

// ConcurrentModificationException
public class MapTest {
    public static void main(String[] args) {
// map 是这样用的吗？ 不是，工作中不用 HashMap
// 默认等价于什么？ new HashMap<>(16,0.75);
// Map<String, String> map = new HashMap<>();
        Map<String, String> map = new ConcurrentHashMap<>();
        for (int i = 1; i <= 30; i++) {
            new Thread(() -> {
                map.put(Thread.currentThread().getName(), UUID.randomUUID().toString().substring(
                        0, 5));
                System.out.println(map);
            }, String.valueOf(i)).start();
        }
    }
}
```
## 1.8 Callable
```markdown
Java 5.0 在 java.util.concurrent 提供了一个新的创建执行
线程的方式：Callable 接口

Callable 接口类似于Runnable，两者都是为那些其实例可
能被另一个线程执行的类设计的。但是 Runnable 不会返
回结果，并且无法抛出经过检查的异常。

Callable需要依赖FutureTask ，FutureTask也可以用作闭
锁。
```
```java
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;
import java.util.concurrent.locks.ReentrantLock;


public class CallableTest {
    public static void main(String[] args) throws ExecutionException,
            InterruptedException {
// new Thread(new Runnable()).start();
// new Thread(new FutureTask<V>()).start();
// new Thread(new FutureTask<V>( Callable )).start();
        new Thread().start(); // 怎么启动Callable
        MyThread thread = new MyThread();
        FutureTask futureTask = new FutureTask(thread); // 适配类
        new Thread(futureTask, "A").start();
        new Thread(futureTask, "B").start(); // 结果会被缓存，效率高
        Integer o = (Integer) futureTask.get(); //这个get 方法可能会产生阻塞！把他放到

// 或者使用异步通信来处理！
        System.out.println(o);
    }
}

class MyThread implements Callable<Integer> {
    @Override
    public Integer call() {
        System.out.println("call()"); // 会打印几个call
// 耗时的操作
        return 1024;
    }


}
```
## 1.9 常用的辅助类
### 1.9.1 CountDownLatch
```markdown
Java 5.0 在 java.util.concurrent 包中提供了多种并
发容器类来改进同步容器
的性能。
CountDownLatch 一个同步辅助类，在完成一组正在其
他线程中执行的操作
之前，它允许一个或多个线程一直等待。
闭锁可以延迟线程的进度直到其到达终止状态，闭锁
可以用来确保某些活动直到其他活动都完成才继续执行：
确保某个计算在其需要的所有资源都被初始化之后才继续执行;
确保某个服务在其依赖的所有其他服务都已经启动之后才启动;
等待直到某个操作所有参与者都准备就绪再继续执行。

原理：
countDownLatch.countDown(); // 数量-1

countDownLatch.await(); // 等待计数器归零，然后再向下执行
每次有线程调用 countDown() 数量-1，假设计数器变为0，countDownLatch.await() 就会被唤醒，继续执行！
```
```java
import java.util.concurrent.CountDownLatch;

/*
 * CountDownLatch ：闭锁，在完成某些运算时，只有其他所有线程的运算全部完成，当前运算才继续执行
 */
public class TestCountDownLatch {

	public static void main(String[] args) {
		final CountDownLatch latch = new CountDownLatch(50);
		LatchDemo ld = new LatchDemo(latch);

		long start = System.currentTimeMillis();

		for (int i = 0; i < 50; i++) {
			new Thread(ld).start();
		}

		try {
			latch.await();
		} catch (InterruptedException e) {
		}

		long end = System.currentTimeMillis();

		System.out.println("耗费时间为：" + (end - start));
	}

}

class LatchDemo implements Runnable {

	private CountDownLatch latch;

	public LatchDemo(CountDownLatch latch) {
		this.latch = latch;
	}

	@Override
	public void run() {

		try {
			for (int i = 0; i < 50000; i++) {
				if (i % 2 == 0) {
					System.out.println(i);
				}
			}
		} finally {
			latch.countDown();
		}

	}

}
```
### 1.9.2 CyclicBarrier
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322211743173.png)
```java
import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;
public class CyclicBarrierDemo {
    public static void main(String[] args) {
/**
 * 集齐7颗龙珠召唤神龙
 */
// 召唤龙珠的线程
        CyclicBarrier cyclicBarrier = new CyclicBarrier(7,()->{
            System.out.println("召唤神龙成功！");
        });
        for (int i = 1; i <=7 ; i++) {
            final int temp = i;
// lambda能操作到 i 吗
            new Thread(()->{
                System.out.println(Thread.currentThread().getName()+"收集"+temp+"个龙珠");
                try {
                    cyclicBarrier.await(); // 等待
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } catch (BrokenBarrierException e) {
                    e.printStackTrace();
                }
            }).start();
        }
    }
}
```
### 1.9.3 Semaphore
```markdown
Semaphore：信号量

原理：
semaphore.acquire() 获得，假设如果已经满了，等待，
等待被释放为止！

semaphore.release(); 释放，会将当前的信号量释放 + 1，
然后唤醒等待的线程！
作用： 多个共享资源互斥的使用！并发限流，控制最大的线程数！
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322211955694.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
抢车位！
6车---3个停车位置
```
```java
import java.util.concurrent.Semaphore;
import java.util.concurrent.TimeUnit;

public class SemaphoreDemo {
    public static void main(String[] args) {
// 线程数量：停车位! 限流！
        Semaphore semaphore = new Semaphore(3);
        for (int i = 1; i <= 6; i++) {
            new Thread(() -> {
// acquire() 得到
                try {
                    semaphore.acquire();
                    System.out.println(Thread.currentThread().getName() + "抢到车位");
                    TimeUnit.SECONDS.sleep(2);
                    System.out.println(Thread.currentThread().getName() + "离开车位");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } finally {
                    semaphore.release(); // release() 释放
                }
            }, String.valueOf(i)).start();
        }
    }
}
```
## 1.10 ReadWriteLock 读写锁
```markdown
ReadWriteLock 维护了一对相关的锁，一个用于只读操作，
另一个用于写入操作。只要没有writer，读取锁可以由
多个reader线程同时保持。写入锁是独占的。

ReadWriteLock 读取操作通常不会改变共享资源，但执行
写入操作时，必须独占方式来获取锁。对于读取操作占
多数的数据结构。 ReadWriteLock能提供比独占锁更高
的并发性。而对于只读的数据结构，其中包含的不变性
可以完全不需要考虑加锁操作。
```
```java
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

/**
 * 独占锁（写锁） 一次只能被一个线程占有
 * 共享锁（读锁） 多个线程可以同时占有
 * ReadWriteLock
 * 读-读 可以共存！
 * 读-写 不能共存！
 * 写-写 不能共存！
 */
public class ReadWriteLockDemo {
    public static void main(String[] args) {
        MyCache myCache = new MyCache();
// 写入
        for (int i = 1; i <= 5; i++) {
            final int temp = i;
            new Thread(() -> {
                myCache.put(temp + "", temp + "");
            }, String.valueOf(i)).start();
        }
// 读取
        for (int i = 1; i <= 5; i++) {
            final int temp = i;
            new Thread(() -> {
                myCache.get(temp + "");
            }, String.valueOf(i)).start();
        }
    }
}

// 加锁的
class MyCacheLock {
    private volatile Map<String, Object> map = new HashMap<>();
    // 读写锁： 更加细粒度的控制
    private ReadWriteLock readWriteLock = new ReentrantReadWriteLock();
    private Lock lock = new ReentrantLock();

    // 存，写入的时候，只希望同时只有一个线程写
    public void put(String key, Object value) {
        readWriteLock.writeLock().lock();
        try {
            System.out.println(Thread.currentThread().getName() + "写入" + key);
            map.put(key, value);
            System.out.println(Thread.currentThread().getName() + "写入OK");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            readWriteLock.writeLock().unlock();
        }
    }

    // 取，读，所有人都可以读！
    public void get(String key) {
        readWriteLock.readLock().lock();
        try {
            System.out.println(Thread.currentThread().getName() + "读取" + key);
            Object o = map.get(key);
            System.out.println(Thread.currentThread().getName() + "读取OK");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            readWriteLock.readLock().unlock();
        }
    }
}

/**
 * 自定义缓存
 */
class MyCache {
    private volatile Map<String, Object> map = new HashMap<>();

    // 存，写
    public void put(String key, Object value) {
        System.out.println(Thread.currentThread().getName() + "写入" + key);
        map.put(key, value);
        System.out.println(Thread.currentThread().getName() + "写入OK");
    }

    // 取，读
    public void get(String key) {
        System.out.println(Thread.currentThread().getName() + "读取" + key);
        Object o = map.get(key);
        System.out.println(Thread.currentThread().getName() + "读取OK");
    }
}
```
## 1.11 阻塞队列
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322212729271.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322212803107.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322212816805.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322212913239.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
什么情况下我们会使用 阻塞队列：多线程并发处理，线程池！
学会使用队列
添加、移除
四组API
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322213021630.png)
```java
    /**
     * 抛出异常
     */
    public static void test1(){
// 队列的大小
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);
        System.out.println(blockingQueue.add("a"));
        System.out.println(blockingQueue.add("b"));
        System.out.println(blockingQueue.add("c"));
// IllegalStateException: Queue full 抛出异常！
// System.out.println(blockingQueue.add("d"));
        System.out.println("=-===========");
        System.out.println(blockingQueue.remove());
        System.out.println(blockingQueue.remove());
        System.out.println(blockingQueue.remove());
// java.util.NoSuchElementException 抛出异常！
// System.out.println(blockingQueue.remove());
    }
```
```java
    /**
     * 有返回值，没有异常
     */
    public static void test2(){
// 队列的大小
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);
        System.out.println(blockingQueue.offer("a"));
        System.out.println(blockingQueue.offer("b"));
        System.out.println(blockingQueue.offer("c"));
// System.out.println(blockingQueue.offer("d")); // false 不抛出异常！
        System.out.println("============================");
        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll()); // null 不抛出异常！
    }
```
```java
    /**
     * 等待，阻塞（一直阻塞）
     */
    public static void test3() throws InterruptedException {
// 队列的大小
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);
      
// 一直阻塞
        blockingQueue.put("a");
        blockingQueue.put("b");
        blockingQueue.put("c");
// blockingQueue.put("d"); // 队列没有位置了，一直阻塞
        System.out.println(blockingQueue.take());
        System.out.println(blockingQueue.take());
        System.out.println(blockingQueue.take());
        System.out.println(blockingQueue.take()); // 没有这个元素，一直阻塞
    }

```
```java
    /**
     * 等待，阻塞（等待超时）
     */
    public static void test4() throws InterruptedException {
// 队列的大小
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);
        blockingQueue.offer("a");
        blockingQueue.offer("b");
        blockingQueue.offer("c");
// blockingQueue.offer("d",2,TimeUnit.SECONDS); // 等待超过2秒就退出
        System.out.println("===============");
        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        blockingQueue.poll(2,TimeUnit.SECONDS); // 等待超过2秒就退出
    }
```
```markdown
SynchronousQueue 同步队列
```
```java
没有容量，
进去一个元素，必须等待取出来之后，才能再往里面放一个元素！
put、take
```
```java
import java.sql.Time;
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.SynchronousQueue;
import java.util.concurrent.TimeUnit;
/**
 * 同步队列
 * 和其他的BlockingQueue 不一样， SynchronousQueue 不存储元素
 * put了一个元素，必须从里面先take取出来，否则不能在put进去值！
 */
public class SynchronousQueueDemo {
    public static void main(String[] args) {
        BlockingQueue<String> blockingQueue = new SynchronousQueue<>(); // 同步队列
        new Thread(()->{
            try {
                System.out.println(Thread.currentThread().getName()+" put 1");
                blockingQueue.put("1");
                System.out.println(Thread.currentThread().getName()+" put 2");
                blockingQueue.put("2");
                System.out.println(Thread.currentThread().getName()+" put 3");
                blockingQueue.put("3");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        },"T1").start();
        new Thread(()->{
            try {
                TimeUnit.SECONDS.sleep(3);
                System.out.println(Thread.currentThread().getName()+"=>"+blockingQueue.take());
                TimeUnit.SECONDS.sleep(3);
                System.out.println(Thread.currentThread().getName()+"=>"+blockingQueue.take());
                TimeUnit.SECONDS.sleep(3);
                System.out.println(Thread.currentThread().getName()+"=>"+blockingQueue.take());
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        },"T2").start();
    }
}
```
## 1.12 线程池
```java
线程池：三大方法、7大参数、4种拒绝策略

池化技术：事先准备好一些资源，有人要用，就来我
这里拿，用完之后还给我

线程池的好处:
1 降低资源的消耗
2 提高响应的速度
3 方便管理。

线程池：三大方法
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322215043881.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

// Executors 工具类、3大方法
public class Demo01 {
    public static void main(String[] args) {
        ExecutorService threadPool = Executors.newSingleThreadExecutor();// 单个线程
// ExecutorService threadPool = Executors.newFixedThreadPool(5); // 创建一个固定的线程池的大小
// ExecutorService threadPool = Executors.newCachedThreadPool(); // 可伸缩的，遇强则强，遇弱则弱
        try {
            for (int i = 0; i < 100; i++) {
// 使用了线程池之后，使用线程池来创建线程
                threadPool.execute(() -> {
                    System.out.println(Thread.currentThread().getName() + " ok");
                });
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
// 线程池用完，程序结束，关闭线程池
            threadPool.shutdown();
        }
    }
}

```
```markdown
7大参数
```
```java
    public static ExecutorService newSingleThreadExecutor() {
            return new FinalizableDelegatedExecutorService
            (new ThreadPoolExecutor(1, 1,
            0L, TimeUnit.MILLISECONDS,
            new LinkedBlockingQueue<Runnable>()));
    }
    public static ExecutorService newFixedThreadPool(int nThreads) {
            return new ThreadPoolExecutor(5, 5,
            0L, TimeUnit.MILLISECONDS,
            new LinkedBlockingQueue<Runnable>());
     }
    public static ExecutorService newCachedThreadPool() {
            return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
            60L, TimeUnit.SECONDS,
            new SynchronousQueue<Runnable>());
    }
    // 本质ThreadPoolExecutor（）
    public ThreadPoolExecutor(int corePoolSize, // 核心线程池大小
            int maximumPoolSize, // 最大核心线程池大小
            long keepAliveTime, // 超时了没有人调用就会释放
            TimeUnit unit, // 超时单位
            BlockingQueue<Runnable> workQueue, // 阻塞队列
            ThreadFactory threadFactory, // 线程工厂：创建线程的，一般不用动
            RejectedExecutionHandler handle // 拒绝策略) {
            if (corePoolSize < 0 ||
            maximumPoolSize <= 0 ||
            maximumPoolSize < corePoolSize ||
            keepAliveTime < 0)
            throw new IllegalArgumentException();
            if (workQueue == null || threadFactory == null || handler == null)
            throw new NullPointerException();
            this.acc = System.getSecurityManager() == null ?
            null :
            AccessController.getContext();
            this.corePoolSize = corePoolSize;
            this.maximumPoolSize = maximumPoolSize;
            this.workQueue = workQueue;
            this.keepAliveTime = unit.toNanos(keepAliveTime);
            this.threadFactory = threadFactory;
            this.handler = handler;

```
```markdown
手动创建一个线程池
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021032222060418.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
import java.util.concurrent.*;
// Executors 工具类、3大方法

/**
 * new ThreadPoolExecutor.AbortPolicy() // 银行满了，还有人进来，不处理这个人的，抛出异
 * 常
 * new ThreadPoolExecutor.CallerRunsPolicy() // 哪来的去哪里！
 * new ThreadPoolExecutor.DiscardPolicy() //队列满了，丢掉任务，不会抛出异常！
 * new ThreadPoolExecutor.DiscardOldestPolicy() //队列满了，尝试去和最早的竞争，也不会
 * 抛出异常！
 */
public class Demo01 {
    public static void main(String[] args) {
// 自定义线程池！工作 ThreadPoolExecutor
        ExecutorService threadPool = new ThreadPoolExecutor(
                2,
                5,
                3,
                TimeUnit.SECONDS,
                new LinkedBlockingDeque<>(3),
                Executors.defaultThreadFactory(),
                new ThreadPoolExecutor.DiscardOldestPolicy()); //队列满了，尝试去和最早的竞争，也不会抛出异常！
        try {
// 最大承载：Deque + max
// 超过 RejectedExecutionException
            for (int i = 1; i <= 9; i++) {
// 使用了线程池之后，使用线程池来创建线程
                threadPool.execute(() -> {
                    System.out.println(Thread.currentThread().getName() + " ok");
                });

            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
// 线程池用完，程序结束，关闭线程池
            threadPool.shutdown();
        }
    }
}

```
```markdown
4种拒绝策略
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322220852682.png)
```markdown
/**
* new ThreadPoolExecutor.AbortPolicy() // 银行满了，还有人进来，不处理这个人的，抛出异
常
* new ThreadPoolExecutor.CallerRunsPolicy() // 哪来的去哪里！
* new ThreadPoolExecutor.DiscardPolicy() //队列满了，丢掉任务，不会抛出异常！
* new ThreadPoolExecutor.DiscardOldestPolicy() //队列满了，尝试去和最早的竞争，也不会
抛出异常！
*/
```
```java
import java.util.concurrent.*;
public class Demo01 {
    public static void main(String[] args) {
// 自定义线程池！工作 ThreadPoolExecutor
// 最大线程到底该如何定义
// 1、CPU 密集型，几核，就是几，可以保持CPu的效率最高！
// 2、IO 密集型 > 判断你程序中十分耗IO的线程，
// 程序 15个大型任务 io十分占用资源！
// 获取CPU的核数
        System.out.println(Runtime.getRuntime().availableProcessors());

        ExecutorService threadPool = new ThreadPoolExecutor(
                2,
                Runtime.getRuntime().availableProcessors(),
                3,
                TimeUnit.SECONDS,
                new LinkedBlockingDeque<>(3),
                Executors.defaultThreadFactory(),
                new ThreadPoolExecutor.DiscardOldestPolicy()); //队列满了，尝试去和最早的竞争，也不会抛出异常！
        try {
// 最大承载：Deque + max
// 超过 RejectedExecutionException
            for (int i = 1; i <= 9; i++) {
// 使用了线程池之后，使用线程池来创建线程
                threadPool.execute(()->{
                    System.out.println(Thread.currentThread().getName()+" ok");
                });
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
// 线程池用完，程序结束，关闭线程池
            threadPool.shutdown();
        }
    }
}
```
## 1.13 ForkJoin
```markdown
Fork/Join 框架：就是在必要的情况下，将一个大
任务，进行拆分(fork)成
若干个小任务（拆到不可再拆时），再将一个个的小
任务运算的结果进
行 join 汇总。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322224642999.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
ForkJoin 特点：工作窃取

采用 “工作窃取”模式（work-stealing）：
当执行新的任务时它可以将其拆分分成更小的任务执行，
并将小任务加到线程队列中，然后再从一个随机线程的队列
中偷一个并把它放在自己的队列中。

相对于一般的线程池实现，fork/join框架的优势体现在
对其中包含的任务的处理方式上.在一般的线程池中，
如果一个线程正在执行的任务由于某些原因无法继续运
行，那么该线程会处于等待状态。而在fork/join框架实现中，
如果某个子问题由于等待另外一个子问题的完成而无法
继续运行。那么处理该子问题的线程会主动寻找其他尚
未运行的子问题来执行.这种方式减少了线程的等
待时间，提高了性能。
```
```java
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ForkJoinPool;
import java.util.concurrent.ForkJoinTask;
import java.util.stream.DoubleStream;
import java.util.stream.IntStream;
import java.util.stream.LongStream;

/**
 * 同一个任务，别人效率高你几十倍！
 */
public class Test {
    public static void main(String[] args) throws ExecutionException,
            InterruptedException {
// test1(); // 12224
// test2(); // 10038
// test3(); // 153
    }

    // 普通程序员
    public static void test1() {
        Long sum = 0L;
        long start = System.currentTimeMillis();
        for (Long i = 1L; i <= 10_0000_0000; i++) {
            sum += i;
        }
        long end = System.currentTimeMillis();
        System.out.println("sum=" + sum + " 时间：" + (end - start));
    }

    // 会使用ForkJoin
    public static void test2() throws ExecutionException, InterruptedException {
        long start = System.currentTimeMillis();
        ForkJoinPool forkJoinPool = new ForkJoinPool();
        ForkJoinTask<Long> task = new ForkJoinDemo(0L, 10_0000_0000L);
        ForkJoinTask<Long> submit = forkJoinPool.submit(task);// 提交任务
        Long sum = submit.get();
        long end = System.currentTimeMillis();
        System.out.println("sum=" + sum + " 时间：" + (end - start));
    }

    public static void test3() {
        long start = System.currentTimeMillis();
// Stream并行流 () (]
        long sum = LongStream.rangeClosed(0L,
                10_0000_0000L).parallel().reduce(0, Long::sum);
        long end = System.currentTimeMillis();

				.
        System.out.println("sum=" + "时间：" + (end - start));
    }
}
```
## 1.14 异步回调
```markdown
Future 设计的初衷： 对将来的某个事件的结果进行建模
```
```java
import java.util.concurrent.CompletableFuture;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Future;
import java.util.concurrent.TimeUnit;

/**
 * 异步调用： CompletableFuture
 * // 异步执行
 * // 成功回调
 * // 失败回调
 */
public class Demo01 {
    public static void main(String[] args) throws ExecutionException,
            InterruptedException {
// 没有返回值的 runAsync 异步回调
// CompletableFuture<Void> completableFuture =
        CompletableFuture.runAsync(() -> {
// try {
// TimeUnit.SECONDS.sleep(2);
// } catch (InterruptedException e) {
// e.printStackTrace();
// }
//
            System.out.println(Thread.currentThread().getName() + "runAsync=>Void");
// });
//
// System.out.println("1111");
//
// completableFuture.get(); // 获取阻塞执行结果
// 有返回值的 supplyAsync 异步回调
// ajax，成功和失败的回调

// 返回的是错误信息；
            CompletableFuture<Integer> completableFuture =
                    CompletableFuture.supplyAsync(() -> {
                        System.out.println(Thread.currentThread().getName() + "supplyAsync=>Integer");
                        int i = 10 / 0;
                        return 1024;
                    });
            System.out.println(completableFuture.whenComplete((t, u) -> {
                System.out.println("t=>" + t); // 正常的返回结果
                System.out.println("u=>" + u); // 错误信息：
                java.util.concurrent.CompletionException:java.lang.ArithmeticException: /by
                        zero
            }).exceptionally((e) -> {
                System.out.println(e.getMessage());
                return 233; // 可以获取到错误的返回结果
            }).get());
/**
 * succee Code 200
 * error Code 404 500
 */
        }
    }
```
## 1.15 浅谈JMM
```markdown
JMM ： Java内存模型，不存在的东西，概念！约定！
关于JMM的一些同步的约定：
1 线程解锁前，必须把共享变量立刻刷回主存。
2 线程加锁前，必须读取主存中的最新值到工作内存中！
3 加锁和解锁是同一把锁
```
```markdown
线程 工作内存 、主内存
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322144834329.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
内存交互操作有8种，虚拟机实现必须保证每一个操作都是原子
的，不可在分的（对于double和long类型的变量来
说，load、store、read和write操作在某些平台上允许例外）

lock （锁定）：作用于主内存的变量，把一个变量标识
为线程独占状态
unlock （解锁）：作用于主内存的变量，它把一个处于锁定
状态的变量释放出来，释放后的变量才可以被其他线程锁定
read （读取）：作用于主内存变量，它把一个变量的值从主
内存传输到线程的工作内存中，以便随后的load动作使用
load （载入）：作用于工作内存的变量，它把read操作从主存
中变量放入工作内存中
use （使用）：作用于工作内存中的变量，它把工作内存中的
变量传输给执行引擎，每当虚拟机遇到一个需要使用到变量的
值，就会使用到这个指令
assign （赋值）：作用于工作内存中的变量，它把一个从执
行引擎中接受到的值放入工作内存的变量副本中
store （存储）：作用于主内存中的变量，它把一个从工作
内存中一个变量的值传送到主内存中，以便后续的write使用
write （写入）：作用于主内存中的变量，它把store操作从
工作内存中得到的变量的值放入主内存的变量中

JMM对这八种指令的使用，制定了如下规则：
不允许read和load、store和write操作之一单独出现。即
使用了read必须load，使用了store必须write，不允许
线程丢弃他最近的assign操作，即工作变量的数据改变了
之后，必须告知主存，不允许一个线程将没有assign的数
据从工作内存同步回主内存。
一个新的变量必须在主内存中诞生，不允许工作内存直接
使用一个未被初始化的变量。就是对变量实施use、store操
作之前，必须经过assign和load操作
一个变量同一时间只有一个线程能对其进行lock。多
次lock后，必须执行相同次数的unlock才能解锁
如果对一个变量进行lock操作，会清空所有工作内存中此
变量的值，在执行引擎使用这个变量前，必须重新load或
assign操作初始化变量的值。如果一个变量没有被lock，就
不能对其进行unlock操作。也不能unlock一个被其他线程
锁住的变量。对一个变量进行unlock操作之前，必须把此
变量同步回主内存
问题： 程序不知道主内存的值已经被修改过了
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322145133928.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
使用volatile解决上述问题
```
## 1.16 volatile 关键字与内存可见性
```markdown
内存可见性（Memory Visibility）是指当某个线程正在使用对象状态
而另一个线程在同时修改该状态，需要确保当一个线程修改了对象
状态后，其他线程能够看到发生的状态变化。

可见性错误是指当读操作与写操作在不同的线程中执行时，我们无
法确保执行读操作的线程能适时地看到其他线程写入的值，有时甚
至是根本不可能的事情。

我们可以通过同步来保证对象被安全地发布。除此之外我们也可以
使用一种更加轻量级的 volatile 变量。

volatile 关键字
Java 提供了一种稍弱的同步机制，即 volatile 变
量，用来确保将变量的更新操作通知到其他线程。
可以将 volatile 看做一个轻量级的锁，但是又与
锁有些不同：
对于多线程，不是一种互斥关系
不能保证变量状态的“原子性操作”

Volatile 是 Java 虚拟机提供轻量级的同步机制
1 保证可见性
2 不保证原子性
3 禁止指令重排
```
```java
/*
 * 一、volatile 关键字：当多个线程进行操作共享数据时，可以保证内存中的数据可见。
 * 					  相较于 synchronized 是一种较为轻量级的同步策略。
 * 					synchronized可以解决内存可见性。因为它可以刷新缓存
 * 
 * 注意：
 * 1. volatile 不具备“互斥性”
 * 2. volatile 不能保证变量的“原子性”
 */
public class TestVolatile {
	
	public static void main(String[] args) {
		ThreadDemo td = new ThreadDemo();
		new Thread(td).start();
		
		while(true){
			if(td.isFlag()){
				System.out.println("------------------");
				break;
			}
		}
		
	}

}

class ThreadDemo implements Runnable {

	private volatile boolean flag = false;

	@Override
	public void run() {
		
		try {
			Thread.sleep(200);
		} catch (InterruptedException e) {
		}

		flag = true;
		
		System.out.println("flag=" + isFlag());

	}

	public boolean isFlag() {
		return flag;
	}

	public void setFlag(boolean flag) {
		this.flag = flag;
	}

}
```
## 1.17 原子变量与CAS算法
### 1.17.1 CAS算法
```markdown
CAS (Compare-And-Swap) 是一种硬件对并发的支持，针
对多处理器操作而设计的处理器中的一种特殊指令，用于
管理对共享数据的并
发访问。
CAS 是一种无锁的非阻塞算法的实现。
CAS 包含了 3 个操作数：
需要读写的内存值 V
进行比较的值 A
拟写入的新值 B
当且仅当 V 的值等于 A 时，CAS 通过原子方式用新
值 B 来更新 V 的值，否则不会执行任何操作。
```
### 1.17.2 原子变量
```markdown
类的小工具包，支持在单个变量上解除锁的线程安全编程。事
实上，此包中的类可将 volatile 值、字段和数组元素的概念扩
展到那些也提供原子条件更新操作的类。

类 AtomicBoolean、AtomicInteger、AtomicLong 
和 AtomicReference 的实例各自提供对
相应类型单个变量的访问和更新。每个类也为该类型提
供适当的实用工具方法。

AtomicIntegerArray、AtomicLongArray 和 AtomicReferenceArray 类进一步扩展了原子操作，对这些类型的数组提供了支持。这些类在为其数组元素提供 volatile 访问语义方
面也引人注目，这对于普通数组来说是不受支持的。
核心方法：boolean compareAndSet(expectedValue, updateValue)
java.util.concurrent.atomic 包下提供了一些原子操作的常用类:
AtomicBoolean 、AtomicInteger 、AtomicLong 、 AtomicReference
AtomicIntegerArray 、AtomicLongArray
AtomicMarkableReference
AtomicReferenceArray
AtomicStampedReference

使用原子类保证原子性
```
### 1.17.3 模拟CAS算法
```java
/*
 * 模拟 CAS 算法
 */
public class TestCompareAndSwap {

	public static void main(String[] args) {
		final CompareAndSwap cas = new CompareAndSwap();
		
		for (int i = 0; i < 10; i++) {
			new Thread(new Runnable() {
				
				@Override
				public void run() {
					int expectedValue = cas.get();
					boolean b = cas.compareAndSet(expectedValue, (int)(Math.random() * 101));
					System.out.println(b);
				}
			}).start();
		}
		
	}
	
}

class CompareAndSwap{
	private int value;
	
	//获取内存值
	public synchronized int get(){
		return value;
	}
	
	//比较
	public synchronized int compareAndSwap(int expectedValue, int newValue){
		int oldValue = value;
		
		if(oldValue == expectedValue){
			this.value = newValue;
		}
		
		return oldValue;
	}
	
	//设置
	public synchronized boolean compareAndSet(int expectedValue, int newValue){
		return expectedValue == compareAndSwap(expectedValue, newValue);
	}
}

```
## 1.18 各种锁的理解
### 1.18.1 公平锁、非公平锁
```markdown
公平锁： 非常公平， 不能够插队，必须先来后到！
非公平锁：非常不公平，可以插队 （默认都是非公平）
public ReentrantLock() {
sync = new NonfairSync();
}
public ReentrantLock(boolean fair) {
sync = fair ? new FairSync() : new NonfairSync();
}
```
### 1.18.2 可重入锁（递归锁)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322232314404.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
Synchronized
```
```java
import javax.sound.midi.Soundbank;
// Synchronized
public class Demo01 {
    public static void main(String[] args) {
        Phone phone = new Phone();
        new Thread(()->{
            phone.sms();
        },"A").start();
        new Thread(()->{
            phone.sms();
        },"B").start();
    }
}
class Phone{
    public synchronized void sms(){
        System.out.println(Thread.currentThread().getName() + "sms");
        call(); // 这里也有锁
    }
    public synchronized void call(){
        System.out.println(Thread.currentThread().getName() + "call");
    }
}
```
```markdown
Lock 版
```
```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Demo02 {
    public static void main(String[] args) {
        Phone2 phone = new Phone2();
        new Thread(() -> {
            phone.sms();
        }, "A").start();
        new Thread(() -> {
            phone.sms();
        }, "B").start();
    }
}

class Phone2 {
    Lock lock = new ReentrantLock();

    public void sms() {
        lock.lock(); // 细节问题：lock.lock(); lock.unlock(); // lock 锁必须配对，否则就会死在里面
        lock.lock();
        try {
            System.out.println(Thread.currentThread().getName() + "sms");
            call(); // 这里也有锁
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
            lock.unlock();
        }
    }

    public void call() {
        lock.lock();
        try {
            System.out.println(Thread.currentThread().getName() + "call");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}
```
### 1.18.3 自旋锁
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322235220210.png)
```java
import java.util.concurrent.atomic.AtomicReference;
/**
 * 自旋锁
 */
public class SpinlockDemo {
    // int 0
// Thread null
    AtomicReference<Thread> atomicReference = new AtomicReference<>();
    // 加锁
    public void myLock(){
        Thread thread = Thread.currentThread();
        System.out.println(Thread.currentThread().getName() + "==> mylock");
// 自旋锁
        while (!atomicReference.compareAndSet(null,thread)){
        }
    }
    // 解锁
// 加锁
    public void myUnLock(){
        Thread thread = Thread.currentThread();
        System.out.println(Thread.currentThread().getName() + "==> myUnlock");
        atomicReference.compareAndSet(thread,null);
    }
}

```
```markdown
测试
```
```java
import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.ReentrantLock;
public class TestSpinLock {
    public static void main(String[] args) throws InterruptedException {
// ReentrantLock reentrantLock = new ReentrantLock();
// reentrantLock.lock();
// reentrantLock.unlock();
// 底层使用的自旋锁CAS
        SpinlockDemo lock = new SpinlockDemo();
        new Thread(()-> {
            lock.myLock();
            try {
                TimeUnit.SECONDS.sleep(5);
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                lock.myUnLock();
            }
        },"T1").start();
        TimeUnit.SECONDS.sleep(1);
        new Thread(()-> {
            lock.myLock();
            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                lock.myUnLock();
            }
        },"T2").start();
    }
}
```
### 1.18.4 死锁
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322235406536.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
import com.sun.org.apache.xpath.internal.SourceTree;
import java.util.concurrent.TimeUnit;
public class DeadLockDemo {
    public static void main(String[] args) {
        String lockA = "lockA";
        String lockB = "lockB";
        new Thread(new MyThread(lockA, lockB), "T1").start();
        new Thread(new MyThread(lockB, lockA), "T2").start();
    }
}
class MyThread implements Runnable{
    private String lockA;
    private String lockB;
    public MyThread(String lockA, String lockB) {
        this.lockA = lockA;
        this.lockB = lockB;
    }
    @Override
    public void run() {
        
            System.out.println(Thread.currentThread().getName() +
                    "lock:"+lockA+"=>get"+lockB);
            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            synchronized (lockB){
                System.out.println(Thread.currentThread().getName() +
                        "lock:"+lockB+"=>get"+lockA);
            }
        }
    }
}
```
```markdown
解决问题
1 使用 jps -l 定位进程号
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322235547707.png)

```markdown
2 使用 jstack 进程号 找到死锁问题
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322235620806.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复juc
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
