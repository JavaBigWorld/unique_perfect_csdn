# Spring
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210717130548183.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1 Spring框架的引言
```markdown
spring(春天),生于在2002年,由Rod Johnson创作。Spring框架
是一个集众多设计模式于一身的开源的、轻量级的项目管理框
架。致力于JAVAEE轻量级解决方案。相对于原来学过的框架而
言,spring框架和之前学习的struts2 、 mybatis 框架有了
本质的区别,不是替换原来的某个框架,而是对其进行整合管理。
轻量级解决方案:提供一个以简单的、统一的、高效的方式构造
整个应用，并且可以将单层框架以最佳的组合揉和在一起建立一个
连贯的体系。
```

## 2 Spring框架的核心作用
```markdown
Spring 框架用来管理[创建|使用|销毁]项目中的组件,由于spring 
框架可以帮我们生产项目中组件对象,因此也习惯称spring是一
个工厂|容器。

组件: 项目中的service,dao,action,都是项目中的组件 

注意:spring框架通常不管理对实体类对象创建
```


## 3 第一个环境搭建
```markdown
引入依赖
```

```xml
	<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>4.3.2.RELEASE</version>
  </dependency>
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-beans</artifactId>
      <version>4.3.2.RELEASE</version>
  </dependency>
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>4.3.2.RELEASE</version>
  </dependency>
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-expression</artifactId>
      <version>4.3.2.RELEASE</version>
  </dependency>
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aop</artifactId>
      <version>4.3.2.RELEASE</version>
  </dependency>
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>4.3.2.RELEASE</version>
  </dependency>
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context-support</artifactId>
      <version>4.3.2.RELEASE</version>
  </dependency>
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aspects</artifactId>
      <version>4.3.2.RELEASE</version>
  </dependency>
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>4.3.2.RELEASE</version>
  </dependency>
```
```markdown
引入配置文件
```

```xml
配置文件名称: 任意名称 
配置文件位置: 项目中根下任意位置
配置文件的内容:
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
       http://www.springframework.org/schema/beans/spring-beans-3.2.xsd ">
</beans>
```
```markdown
创建组件
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022191750208.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022191812481.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
工厂管理
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

  	<!--通过bean标签管理组件对象-->
    <bean id="userDAO" class="com.yxj.dao.UserDAOImpl"></bean>

</beans>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022191841295.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
启动工厂测试
```

```java
 public static void main(String[] args) {
			 //启动工厂
        ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
				//获取对象
        UserDAO userDAO = (UserDAO) context.getBean("userDAO");

        userDAO.save("小黑");
    }
```
### 3.1 代码例子
```markdown
pom.xml
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.yxj</groupId>
    <artifactId>day01_yxj_bean</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
    </dependencies>
</project>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200604100037354.png)
```markdown
IAccountService.java
```

```java
package com.yxj.service;

/**
 * 账户业务层的接口
 */
public interface IAccountService {

    /**
     * 模拟保存账户
     */
    void saveAccount();
}

```
```markdown
AccountServiceImpl.java
```
```java
package com.yxj.service.impl;

import com.yxj.dao.IAccountDao;
import com.yxj.service.IAccountService;

/**
 * 账户的业务层实现类
 */
public class AccountServiceImpl implements IAccountService {

    private IAccountDao accountDao ;

    public AccountServiceImpl(){
        System.out.println("对象创建了");
    }

    public void  saveAccount(){
        accountDao.saveAccount();
    }
}

```
```markdown
IAccountDao.java
```
```java
package com.yxj.dao;

/**
 * 账户的持久层接口
 */
public interface IAccountDao {

    /**
     * 模拟保存账户
     */
    void saveAccount();
}

```
```markdown
AccountDaoImpl.java
```
```java
package com.yxj.dao.impl;

import com.yxj.dao.IAccountDao;

/**
 * 账户的持久层实现类
 */
public class AccountDaoImpl implements IAccountDao {

    public  void saveAccount(){

        System.out.println("保存了账户");
    }
}

```
```markdown
bean.xml
```

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--把对象的创建交给spring来管理-->
    <bean id="accountService" class="com.yxj.service.impl.AccountServiceImpl"></bean>

    <bean id="accountDao" class="com.yxj.dao.impl.AccountDaoImpl"></bean>
</beans>
```
```markdown
Client.java
```
```java
package com.yxj.ui;

import com.yxj.dao.IAccountDao;
import com.yxj.service.IAccountService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * 模拟一个表现层，用于调用业务层
 */
public class Client {

    /**
     * 获取spring的Ioc核心容器，并根据id获取对象
     *
     * ApplicationContext的三个常用实现类：
     *      ClassPathXmlApplicationContext：它可以加载类路径下的配置文件，要求配置文件必须在类路径下。不在的话，加载不了。(更常用)
     *      FileSystemXmlApplicationContext：它可以加载磁盘任意路径下的配置文件(必须有访问权限）
     *
     *      AnnotationConfigApplicationContext：它是用于读取注解创建容器的，是明天的内容。
     *
     * 核心容器的两个接口引发出的问题：
     *  ApplicationContext:     单例对象适用              采用此接口
     *      它在构建核心容器时，创建对象采取的策略是采用立即加载的方式。也就是说，只要一读取完配置文件马上就创建配置文件中配置的对象。
     *
     *  BeanFactory:            多例对象使用
     *      它在构建核心容器时，创建对象采取的策略是采用延迟加载的方式。也就是说，什么时候根据id获取对象了，什么时候才真正的创建对象。
     * @param args
     */
    public static void main(String[] args) {
        //1.获取核心容器对象
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
//        ApplicationContext ac = new FileSystemXmlApplicationContext("C:\\Users\\zhy\\Desktop\\bean.xml");
        //2.根据id获取Bean对象
        IAccountService as  = (IAccountService)ac.getBean("accountService");
        IAccountDao adao = ac.getBean("accountDao",IAccountDao.class);

        System.out.println(as);
        System.out.println(adao);
        as.saveAccount();


        //--------BeanFactory----------
//        Resource resource = new ClassPathResource("bean.xml");
//        BeanFactory factory = new XmlBeanFactory(resource);
//        IAccountService as  = (IAccountService)factory.getBean("accountService");
//        System.out.println(as);
    }
}

```

## 4 Spring框架中的核心思想
### 4.1 IOC[控制反转]
```markdown
IOC(inversion of controll)控制反转
定义
将对象的创建由原来(new)的方式转移到配置文件中,交给spring
工厂来创建对象
```
```markdown
DI(dependcy Injection)依赖注入 
定义
Spring不仅要创建对象,还要建立类与类之间的关系,因此
在控制反转的基础上又提出了依赖注入的概念。
```

### 4.2 AOP[面向切面编程]
```markdown
AOP( Aspect Oriental Programing ) 面向切面的编程
```
```markdown
AOP 相关术语
Joinpoint(连接点):
所谓连接点是指那些被拦截到的点。在 spring 中,这些点指的
是方法,因为 spring 只支持方法类型的连接点。大白话就
是类中的方法
Pointcut(切入点):
所谓切入点是指我们要对哪些 Joinpoint 进行拦截的定义。大白
话就是类中要增强的方法。也就是说所有的切入点是连接点，但
连接点并不一定是切入点
Advice(通知/增强):
所谓通知是指拦截到 Joinpoint 之后所要做的事情就是通知。
通知的类型：前置通知,后置通知,异常通知,最终通知,环绕通
知。大白话就是说切入点要干的事情。
Introduction(引介):
引介是一种特殊的通知在不修改类代码的前提下, Introduction 
可以在运行期为类动态地添加一些方法或 Field。
Target(目标对象):代理的目标对象。
Weaving(织入):是指把增强应用到目标对象来创建新的代理
对象的过程。
spring采用动态代理织入，而AspectJ采用编译期织入和
类装载期织入。
Proxy（代理）:一个类被 AOP 织入增强后，就产生一个结果代理类。
Aspect(切面):是切入点和通知（引介）的结合。
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200608220345429.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
####  4.2.1 代码例子
```markdown
pom.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.yxj</groupId>
    <artifactId>day03_yxj_03springAOP</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.7</version>
        </dependency>
    </dependencies>
</project>
```
```markdown
Logger.java
```
```java
package com.yxj.utils;

/**
 * 用于记录日志的工具类，它里面提供了公共的代码
 */
public class Logger {

    /**
     * 用于打印日志：计划让其在切入点方法执行之前执行（切入点方法就是业务层方法）
     */
    public  void printLog(){
        System.out.println("Logger类中的pringLog方法开始记录日志了。。。");
    }
}

```
```markdown
IAccountService.java
```

```java
package com.yxj.service;

/**
 * 账户的业务层接口
 */
public interface IAccountService {

    /**
     * 模拟保存账户
     */
   void saveAccount();

    /**
     * 模拟更新账户
     * @param i
     */
   void updateAccount(int i);

    /**
     * 删除账户
     * @return
     */
   int  deleteAccount();
}

```
```markdown
AccountServiceImpl.java
```

```java
package com.yxj.service.impl;

import com.yxj.service.IAccountService;

/**
 * 账户的业务层实现类
 */
public class AccountServiceImpl implements IAccountService{

    @Override
    public void saveAccount() {
        System.out.println("执行了保存");
    }

    @Override
    public void updateAccount(int i) {
        System.out.println("执行了更新"+i);

    }

    @Override
    public int deleteAccount() {
        System.out.println("执行了删除");
        return 0;
    }
}

```
```markdown
bean.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 配置srping的Ioc,把service对象配置进来-->
    <bean id="accountService" class="com.yxj.service.impl.AccountServiceImpl"></bean>

    <!--spring中基于XML的AOP配置步骤
        1、把通知Bean也交给spring来管理
        2、使用aop:config标签表明开始AOP的配置
        3、使用aop:aspect标签表明配置切面
                id属性：是给切面提供一个唯一标识
                ref属性：是指定通知类bean的Id。
        4、在aop:aspect标签的内部使用对应标签来配置通知的类型
               我们现在示例是让printLog方法在切入点方法执行之前之前：所以是前置通知
               aop:before：表示配置前置通知
                    method属性：用于指定Logger类中哪个方法是前置通知
                    pointcut属性：用于指定切入点表达式，该表达式的含义指的是对业务层中哪些方法增强

            切入点表达式的写法：
                关键字：execution(表达式)
                表达式：
                    访问修饰符  返回值  包名.包名.包名...类名.方法名(参数列表)
                标准的表达式写法：
                    public void com.yxj.service.impl.AccountServiceImpl.saveAccount()
                访问修饰符可以省略
                    void com.yxj.service.impl.AccountServiceImpl.saveAccount()
                返回值可以使用通配符，表示任意返回值
                    * com.yxj.service.impl.AccountServiceImpl.saveAccount()
                包名可以使用通配符，表示任意包。但是有几级包，就需要写几个*.
                    * *.*.*.*.AccountServiceImpl.saveAccount())
                包名可以使用..表示当前包及其子包
                    * *..AccountServiceImpl.saveAccount()
                类名和方法名都可以使用*来实现通配
                    * *..*.*()
                参数列表：
                    可以直接写数据类型：
                        基本类型直接写名称           int
                        引用类型写包名.类名的方式   java.lang.String
                    可以使用通配符表示任意类型，但是必须有参数
                    可以使用..表示有无参数均可，有参数可以是任意类型
                全通配写法：
                    * *..*.*(..)

                实际开发中切入点表达式的通常写法：
                    切到业务层实现类下的所有方法
                        * com.yxj.service.impl.*.*(..)
    -->

    <!-- 配置Logger类 -->
    <bean id="logger" class="com.yxj.utils.Logger"></bean>

    <!--配置AOP-->
    <aop:config>
        <!--配置切面 -->
        <aop:aspect id="logAdvice" ref="logger">
            <!-- 配置通知的类型，并且建立通知方法和切入点方法的关联-->
            <aop:before method="printLog" pointcut="execution(* com.yxj.service.impl.*.*(..))"></aop:before>
        </aop:aspect>
    </aop:config>

</beans>
```
```markdown
AOPTest.java
```
```java
package com.yxj.test;

import com.yxj.service.IAccountService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * 测试AOP的配置
 */
public class AOPTest {

    public static void main(String[] args) {
        //1.获取容器
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        //2.获取对象
        IAccountService as = (IAccountService)ac.getBean("accountService");
        //3.执行方法
        as.saveAccount();
        as.updateAccount(1);
        as.deleteAccount();
    }
}

```
#### 4.2.2 通知类型
```markdown
pom.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.yxj</groupId>
    <artifactId>day03_yxj_04adviceType</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.7</version>
        </dependency>
    </dependencies>

</project>
```
```markdown
Logger.java
```
```java
package com.yxj.utils;

import org.aspectj.lang.ProceedingJoinPoint;

/**
 * 用于记录日志的工具类，它里面提供了公共的代码
 */
public class Logger {

    /**
     * 前置通知
     */
    public  void beforePrintLog(){
        System.out.println("前置通知Logger类中的beforePrintLog方法开始记录日志了。。。");
    }

    /**
     * 后置通知
     */
    public  void afterReturningPrintLog(){
        System.out.println("后置通知Logger类中的afterReturningPrintLog方法开始记录日志了。。。");
    }
    /**
     * 异常通知
     */
    public  void afterThrowingPrintLog(){
        System.out.println("异常通知Logger类中的afterThrowingPrintLog方法开始记录日志了。。。");
    }

    /**
     * 最终通知
     */
    public  void afterPrintLog(){
        System.out.println("最终通知Logger类中的afterPrintLog方法开始记录日志了。。。");
    }

    /**
     * 环绕通知
     * 问题：
     *      当我们配置了环绕通知之后，切入点方法没有执行，而通知方法执行了。
     * 分析：
     *      通过对比动态代理中的环绕通知代码，发现动态代理的环绕通知有明确的切入点方法调用，而我们的代码中没有。
     * 解决：
     *      Spring框架为我们提供了一个接口：ProceedingJoinPoint。该接口有一个方法proceed()，此方法就相当于明确调用切入点方法。
     *      该接口可以作为环绕通知的方法参数，在程序执行时，spring框架会为我们提供该接口的实现类供我们使用。
     *
     * spring中的环绕通知：
     *      它是spring框架为我们提供的一种可以在代码中手动控制增强方法何时执行的方式。
     */
    public Object aroundPringLog(ProceedingJoinPoint pjp){
        Object rtValue = null;
        try{
            Object[] args = pjp.getArgs();//得到方法执行所需的参数

            System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。前置");

            rtValue = pjp.proceed(args);//明确调用业务层方法（切入点方法）

            System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。后置");

            return rtValue;
        }catch (Throwable t){
            System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。异常");
            throw new RuntimeException(t);
        }finally {
            System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。最终");
        }
    }
}

```
```markdown
IAccountService.java
```

```java
package com.yxj.service;

/**
 * 账户的业务层接口
 */
public interface IAccountService {

    /**
     * 模拟保存账户
     */
   void saveAccount();

    /**
     * 模拟更新账户
     * @param i
     */
   void updateAccount(int i);

    /**
     * 删除账户
     * @return
     */
   int  deleteAccount();
}

```
```markdown
AccountServiceImpl.java
```
```java
package com.yxj.service.impl;

import com.yxj.service.IAccountService;

/**
 * 账户的业务层实现类
 */
public class AccountServiceImpl implements IAccountService{

    @Override
    public void saveAccount() {
        System.out.println("执行了保存");
//        int i=1/0;
    }

    @Override
    public void updateAccount(int i) {
        System.out.println("执行了更新"+i);

    }

    @Override
    public int deleteAccount() {
        System.out.println("执行了删除");
        return 0;
    }
}

```
```markdown
bean.xml
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 配置srping的Ioc,把service对象配置进来-->
    <bean id="accountService" class="com.yxj.service.impl.AccountServiceImpl"></bean>


    <!-- 配置Logger类 -->
    <bean id="logger" class="com.yxj.utils.Logger"></bean>

    <!--配置AOP-->
    <aop:config>
        <!-- 配置切入点表达式 id属性用于指定表达式的唯一标识。expression属性用于指定表达式内容
              此标签写在aop:aspect标签内部只能当前切面使用。
              它还可以写在aop:aspect外面，此时就变成了所有切面可用
          -->
        <aop:pointcut id="pt1" expression="execution(* com.yxj.service.impl.*.*(..))"></aop:pointcut>
        <!--配置切面 -->
        <aop:aspect id="logAdvice" ref="logger">
            <!-- 配置前置通知：在切入点方法执行之前执行
            <aop:before method="beforePrintLog" pointcut-ref="pt1" ></aop:before>-->

            <!-- 配置后置通知：在切入点方法正常执行之后值。它和异常通知永远只能执行一个
            <aop:after-returning method="afterReturningPrintLog" pointcut-ref="pt1"></aop:after-returning>-->

            <!-- 配置异常通知：在切入点方法执行产生异常之后执行。它和后置通知永远只能执行一个
            <aop:after-throwing method="afterThrowingPrintLog" pointcut-ref="pt1"></aop:after-throwing>-->

            <!-- 配置最终通知：无论切入点方法是否正常执行它都会在其后面执行
            <aop:after method="afterPrintLog" pointcut-ref="pt1"></aop:after>-->

            <!-- 配置环绕通知 详细的注释请看Logger类中-->
            <aop:around method="aroundPringLog" pointcut-ref="pt1"></aop:around>
        </aop:aspect>
    </aop:config>

</beans>
```
```markdown
AOPTest.java
```
```java
package com.yxj.test;

import com.yxj.service.IAccountService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * 测试AOP的配置
 */
public class AOPTest {

    public static void main(String[] args) {
        //1.获取容器
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        //2.获取对象
        IAccountService as = (IAccountService)ac.getBean("accountService");
        //3.执行方法
        as.saveAccount();
    }
}
```
#### 4.2.3 注解开发
```markdown
pom.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.yxj</groupId>
    <artifactId>day03_yxj_05annotationAOP</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.7</version>
        </dependency>
    </dependencies>

</project>
```
```markdown
Logger.java
```
```java
package com.yxj.utils;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;

/**
 * 用于记录日志的工具类，它里面提供了公共的代码
 */
@Component("logger")
@Aspect//表示当前类是一个切面类
public class Logger {

    @Pointcut("execution(* com.yxj.service.impl.*.*(..))")
    private void pt1(){}

    /**
     * 前置通知
     */
//    @Before("pt1()")
    public  void beforePrintLog(){
        System.out.println("前置通知Logger类中的beforePrintLog方法开始记录日志了。。。");
    }

    /**
     * 后置通知
     */
//    @AfterReturning("pt1()")
    public  void afterReturningPrintLog(){
        System.out.println("后置通知Logger类中的afterReturningPrintLog方法开始记录日志了。。。");
    }
    /**
     * 异常通知
     */
//    @AfterThrowing("pt1()")
    public  void afterThrowingPrintLog(){
        System.out.println("异常通知Logger类中的afterThrowingPrintLog方法开始记录日志了。。。");
    }

    /**
     * 最终通知
     */
//    @After("pt1()")
    public  void afterPrintLog(){
        System.out.println("最终通知Logger类中的afterPrintLog方法开始记录日志了。。。");
    }

    /**
     * 环绕通知
     * 问题：
     *      当我们配置了环绕通知之后，切入点方法没有执行，而通知方法执行了。
     * 分析：
     *      通过对比动态代理中的环绕通知代码，发现动态代理的环绕通知有明确的切入点方法调用，而我们的代码中没有。
     * 解决：
     *      Spring框架为我们提供了一个接口：ProceedingJoinPoint。该接口有一个方法proceed()，此方法就相当于明确调用切入点方法。
     *      该接口可以作为环绕通知的方法参数，在程序执行时，spring框架会为我们提供该接口的实现类供我们使用。
     *
     * spring中的环绕通知：
     *      它是spring框架为我们提供的一种可以在代码中手动控制增强方法何时执行的方式。
     */
    @Around("pt1()")
    public Object aroundPringLog(ProceedingJoinPoint pjp){
        Object rtValue = null;
        try{
            Object[] args = pjp.getArgs();//得到方法执行所需的参数

            System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。前置");

            rtValue = pjp.proceed(args);//明确调用业务层方法（切入点方法）

            System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。后置");

            return rtValue;
        }catch (Throwable t){
            System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。异常");
            throw new RuntimeException(t);
        }finally {
            System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。最终");
        }
    }
}

```
```markdown
IAccountService.java
```
```java
package com.yxj.service;

/**
 * 账户的业务层接口
 */
public interface IAccountService {

    /**
     * 模拟保存账户
     */
   void saveAccount();

    /**
     * 模拟更新账户
     * @param i
     */
   void updateAccount(int i);

    /**
     * 删除账户
     * @return
     */
   int  deleteAccount();
}

```
```markdown
AccountServiceImpl.java
```
```java
package com.yxj.service.impl;

import com.yxj.service.IAccountService;
import org.springframework.stereotype.Service;

/**
 * 账户的业务层实现类
 */
@Service("accountService")
public class AccountServiceImpl implements IAccountService{

    @Override
    public void saveAccount() {
        System.out.println("执行了保存");
        int i=1/0;
    }

    @Override
    public void updateAccount(int i) {
        System.out.println("执行了更新"+i);

    }

    @Override
    public int deleteAccount() {
        System.out.println("执行了删除");
        return 0;
    }
}

```
```markdown
bean.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 配置spring创建容器时要扫描的包-->
    <context:component-scan base-package="com.yxj"></context:component-scan>

    <!-- 配置spring开启注解AOP的支持 -->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
</beans>
```
```markdown
AOPTest.java
```
```java
package com.yxj.test;

import com.yxj.service.IAccountService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * 测试AOP的配置
 */
public class AOPTest {

    public static void main(String[] args) {
        //1.获取容器
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        //2.获取对象
        IAccountService as = (IAccountService)ac.getBean("accountService");
        //3.执行方法
        as.saveAccount();
    }
}

```


## 5 SET方式注入

### 5.1 八种基本类型+String类型 +日期类型的注入

```xml
<property name="name" value="zhagnsan"/>
<property name="age" value="21"/>
<property name="id" value="100063"/>
<property name="bir" value="2012/12/12"/>
<property name="price" value="23.23"/>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022191942381.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
数组类型注入
```
```xml
<!--注入数组类型数据-->
  <property name="qqs">
     <array>
         <value>xxx</value>
         <value>qqq</value>
         <value>vvvv</value>
      </array>
  </property>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022192004117.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


```markdown
注入引用类型和集合类型
```
```xml
<!--注入引用类型和对象-->
<property name="userDAO" ref="userDAO"/>
<property name="lists">
  <list>
    <value>aaa</value>
    <value>bbb</value>
    <value>ccc</value>
  </list>
</property>
<property name="maps">
  <map>
    <entry key="aa" value="xiaohei"/>
    <entry key="bb" value="xiaoming"/>
    <entry key="cc" value="xiaosan"/>
  </map>
</property>
<property name="props">
  <props>
    <prop key="url">jdbc:mysql://localhost:3306/test</prop>
    <prop key="driver">com.mysql.jdbc.Driver</prop>
    <prop key="username">hr</prop>
    <prop key="password">hr</prop>
  </props>
</property>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022192029501.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
注意: 引用类型使用ref属性注入,基本类型使用value属性注入
```

## 6 构造注入

```xml
	<constructor-arg index="0" name="id" value="1"/>
	<constructor-arg index="1" name="name" value="xiaohei"/>
	<constructor-arg index="2" name="age" value="12"/>
	<constructor-arg index="3" name="qqs">
    <array>
      <value>xxx</value>
      <value>222</value>
      <value>333</value>
    </array>
	</constructor-arg>
```
```markdown
注意:构造注入并不常用,不过在一些框架类中必须使用构造注入,这
里先了解其注入语法即可。
```
### 6.1 代码例子
```markdown
pom.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.yxj</groupId>
    <artifactId>day01_yxj_05DI</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
    </dependencies>
</project>
```
```markdown
IAccountService.java
```
```java
package com.yxj.service;

/**
 * 账户业务层的接口
 */
public interface IAccountService {

    /**
     * 模拟保存账户
     */
    void saveAccount();
}

```
```markdown
AccountServiceImpl.java
```
```java
package com.yxj.service.impl;

import com.yxj.service.IAccountService;

import java.util.Date;

/**
 * 账户的业务层实现类
 */
public class AccountServiceImpl implements IAccountService {

    //如果是经常变化的数据，并不适用于注入的方式
    private String name;
    private Integer age;
    private Date birthday;

    public AccountServiceImpl(String name,Integer age,Date birthday){
        this.name = name;
        this.age = age;
        this.birthday = birthday;
    }

    public void  saveAccount(){
        System.out.println("service中的saveAccount方法执行了。。。"+name+","+age+","+birthday);
    }


}
```
```markdown
AccountServiceImpl2.javaAccountServiceImpl2.java
```
```java
package com.yxj.service.impl;

import com.yxj.service.IAccountService;

import java.util.Date;

/**
 * 账户的业务层实现类
 */
public class AccountServiceImpl2 implements IAccountService {

    //如果是经常变化的数据，并不适用于注入的方式
    private String name;
    private Integer age;
    private Date birthday;

    public void setName(String name) {
        this.name = name;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public void  saveAccount(){
        System.out.println("service中的saveAccount方法执行了。。。"+name+","+age+","+birthday);
    }


}

```
```markdown
AccountServiceImpl3.java
```
```java
package com.yxj.service.impl;

import com.yxj.service.IAccountService;

import java.util.Arrays;
import java.util.List;
import java.util.Properties;
import java.util.Set;
import java.util.Map;

/**
 * 账户的业务层实现类
 */
public class AccountServiceImpl3 implements IAccountService {

    private String[] myStrs;
    private List<String> myList;
    private Set<String> mySet;
    private Map<String,String> myMap;
    private Properties myProps;

    public void setMyStrs(String[] myStrs) {
        this.myStrs = myStrs;
    }

    public void setMyList(List<String> myList) {
        this.myList = myList;
    }

    public void setMySet(Set<String> mySet) {
        this.mySet = mySet;
    }

    public void setMyMap(Map<String, String> myMap) {
        this.myMap = myMap;
    }

    public void setMyProps(Properties myProps) {
        this.myProps = myProps;
    }

    public void  saveAccount(){
        System.out.println(Arrays.toString(myStrs));
        System.out.println(myList);
        System.out.println(mySet);
        System.out.println(myMap);
        System.out.println(myProps);
    }


}
	
```
```markdown
bean.xml
```

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- spring中的依赖注入
        依赖注入：
            Dependency Injection
        IOC的作用：
            降低程序间的耦合（依赖关系）
        依赖关系的管理：
            以后都交给spring来维护
        在当前类需要用到其他类的对象，由spring为我们提供，我们只需要在配置文件中说明
        依赖关系的维护：
            就称之为依赖注入。
         依赖注入：
            能注入的数据：有三类
                基本类型和String
                其他bean类型（在配置文件中或者注解配置过的bean）
                复杂类型/集合类型
             注入的方式：有三种
                第一种：使用构造函数提供
                第二种：使用set方法提供
                第三种：使用注解提供（明天的内容）
     -->


    <!--构造函数注入：
        使用的标签:constructor-arg
        标签出现的位置：bean标签的内部
        标签中的属性
            type：用于指定要注入的数据的数据类型，该数据类型也是构造函数中某个或某些参数的类型
            index：用于指定要注入的数据给构造函数中指定索引位置的参数赋值。索引的位置是从0开始
            name：用于指定给构造函数中指定名称的参数赋值                                        常用的
            =============以上三个用于指定给构造函数中哪个参数赋值===============================
            value：用于提供基本类型和String类型的数据
            ref：用于指定其他的bean类型数据。它指的就是在spring的Ioc核心容器中出现过的bean对象

        优势：
            在获取bean对象时，注入数据是必须的操作，否则对象无法创建成功。
        弊端：
            改变了bean对象的实例化方式，使我们在创建对象时，如果用不到这些数据，也必须提供。
    -->
    <bean id="accountService" class="com.yxj.service.impl.AccountServiceImpl">
        <constructor-arg name="name" value="泰斯特"></constructor-arg>
        <constructor-arg name="age" value="18"></constructor-arg>
        <constructor-arg name="birthday" ref="now"></constructor-arg>
    </bean>

    <!-- 配置一个日期对象 -->
    <bean id="now" class="java.util.Date"></bean>



    <!-- set方法注入                更常用的方式
        涉及的标签：property
        出现的位置：bean标签的内部
        标签的属性
            name：用于指定注入时所调用的set方法名称
            value：用于提供基本类型和String类型的数据
            ref：用于指定其他的bean类型数据。它指的就是在spring的Ioc核心容器中出现过的bean对象
        优势：
            创建对象时没有明确的限制，可以直接使用默认构造函数
        弊端：
            如果有某个成员必须有值，则获取对象是有可能set方法没有执行。
    -->
    <bean id="accountService2" class="com.yxj.service.impl.AccountServiceImpl2">
        <property name="name" value="TEST" ></property>
        <property name="age" value="21"></property>
        <property name="birthday" ref="now"></property>
    </bean>


    <!-- 复杂类型的注入/集合类型的注入
        用于给List结构集合注入的标签：
            list array set
        用于个Map结构集合注入的标签:
            map  props
        结构相同，标签可以互换
    -->
    <bean id="accountService3" class="com.yxj.service.impl.AccountServiceImpl3">
        <property name="myStrs">
            <set>
                <value>AAA</value>
                <value>BBB</value>
                <value>CCC</value>
            </set>
        </property>

        <property name="myList">
            <array>
                <value>AAA</value>
                <value>BBB</value>
                <value>CCC</value>
            </array>
        </property>

        <property name="mySet">
            <list>
                <value>AAA</value>
                <value>BBB</value>
                <value>CCC</value>
            </list>
        </property>

        <property name="myMap">
            <props>
                <prop key="testC">ccc</prop>
                <prop key="testD">ddd</prop>
            </props>
        </property>

        <property name="myProps">
            <map>
                <entry key="testA" value="aaa"></entry>
                <entry key="testB">
                    <value>BBB</value>
                </entry>
            </map>
        </property>
    </bean>

</beans>
```
```markdown
Client.java
```
```java
package com.yxj.ui;

import com.yxj.service.IAccountService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * 模拟一个表现层，用于调用业务层
 */
public class Client {

    /**
     *
     * @param args
     */
    public static void main(String[] args) {
        //1.获取核心容器对象
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        //2.根据id获取Bean对象
//        IAccountService as  = (IAccountService)ac.getBean("accountService");
//        as.saveAccount();

//        IAccountService as  = (IAccountService)ac.getBean("accountService2");
//        as.saveAccount();

        IAccountService as  = (IAccountService)ac.getBean("accountService3");
        as.saveAccount();

    }
}

```

## 7 自动注入
```markdown
autowire=”byName”
根据注入的属性名与配置文件中bean的id匹配,一致
则注入,不一致报错

autowire=”byType”
根据注入的属性类型,与配置文件中的类型匹配,类型一致
注入(在多个实现类时,会产生歧义)
注意: 无论使用以上那种方式注入都需要为属性提供set方法
```
## 8 bean
### 8.1 bean的创建模式
```markdown
singleton:单例 默认
在工厂中全局唯一,只创建一次
```
```markdown
prototype: 多例
全局不唯一,每次使用都会创建一个新的对象
```


```java
<bean id="" class="xxxx.userAction" scope="prototype|singleton">
		service,dao    ----->  singleton
		struts2 action -----> prototype
```
```markdown
注意:在项目开发中service,dao组件单例,struts2的Action
必须为:多例
```

### 8.2 bean的三种创建方式
```markdown
pom.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.yxj</groupId>
    <artifactId>day01_yxj_04bean</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
    </dependencies>
</project>
```
```markdown
IAccountService.java
```
```java
package com.yxj.service;

/**
 * 账户业务层的接口
 */
public interface IAccountService {

    /**
     * 模拟保存账户
     */
    void saveAccount();
}

```
```markdown
AccountServiceImpl.java
```
```java
package com.yxj.service.impl;

import com.yxj.service.IAccountService;

/**
 * 账户的业务层实现类
 */
public class AccountServiceImpl implements IAccountService {



    public AccountServiceImpl(){
        System.out.println("对象创建了");
    }

    public void  saveAccount(){
        System.out.println("service中的saveAccount方法执行了。。。");
    }

    public void  init(){
        System.out.println("对象初始化了。。。");
    }
    public void  destroy(){
        System.out.println("对象销毁了。。。");
    }

}

```
```markdown
InstanceFactory.java
```
```java
package com.yxj.factory;

import com.yxj.service.IAccountService;
import com.yxj.service.impl.AccountServiceImpl;

/**
 * 模拟一个工厂类（该类可能是存在于jar包中的，我们无法通过修改源码的方式来提供默认构造函数）
 */
public class InstanceFactory {

    public IAccountService getAccountService(){
        return new AccountServiceImpl();
    }
}

```
```markdown
StaticFactory.java
```
```java
package com.yxj.factory;

import com.yxj.service.IAccountService;
import com.yxj.service.impl.AccountServiceImpl;

/**
 * 模拟一个工厂类（该类可能是存在于jar包中的，我们无法通过修改源码的方式来提供默认构造函数）
 */
public class StaticFactory {

    public static IAccountService getAccountService(){

        return new AccountServiceImpl();
    }
}

```
```markdown
bean.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--把对象的创建交给spring来管理-->
    <!--spring对bean的管理细节
        1.创建bean的三种方式
        2.bean对象的作用范围
        3.bean对象的生命周期
    -->

    <!--创建Bean的三种方式 -->
    <!-- 第一种方式：使用默认构造函数创建。
            在spring的配置文件中使用bean标签，配以id和class属性之后，且没有其他属性和标签时。
            采用的就是默认构造函数创建bean对象，此时如果类中没有默认构造函数，则对象无法创建。

    <bean id="accountService" class="com.yxj.service.impl.AccountServiceImpl"></bean>
    -->

    <!-- 第二种方式： 使用普通工厂中的方法创建对象（使用某个类中的方法创建对象，并存入spring容器）
    <bean id="instanceFactory" class="com.yxj.factory.InstanceFactory"></bean>
    <bean id="accountService" factory-bean="instanceFactory" factory-method="getAccountService"></bean>
    -->

    <!-- 第三种方式：使用工厂中的静态方法创建对象（使用某个类中的静态方法创建对象，并存入spring容器)
    <bean id="accountService" class="com.yxj.factory.StaticFactory" factory-method="getAccountService"></bean>
    -->

    <!-- bean的作用范围调整
        bean标签的scope属性：
            作用：用于指定bean的作用范围
            取值： 常用的就是单例的和多例的
                singleton：单例的（默认值）
                prototype：多例的
                request：作用于web应用的请求范围
                session：作用于web应用的会话范围
                global-session：作用于集群环境的会话范围（全局会话范围），当不是集群环境时，它就是session

    <bean id="accountService" class="com.yxj.service.impl.AccountServiceImpl" scope="prototype"></bean>
    -->

    <!-- bean对象的生命周期
            单例对象
                出生：当容器创建时对象出生
                活着：只要容器还在，对象一直活着
                死亡：容器销毁，对象消亡
                总结：单例对象的生命周期和容器相同
            多例对象
                出生：当我们使用对象时spring框架为我们创建
                活着：对象只要是在使用过程中就一直活着。
                死亡：当对象长时间不用，且没有别的对象引用时，由Java的垃圾回收器回收
     -->
    <bean id="accountService" class="com.yxj.service.impl.AccountServiceImpl"
          scope="prototype" init-method="init" destroy-method="destroy"></bean>
</beans>
```
```markdown
Client.java
```
```java
package com.yxj.ui;

import com.yxj.service.IAccountService;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * 模拟一个表现层，用于调用业务层
 */
public class Client {

    /**
     *
     * @param args
     */
    public static void main(String[] args) {
        //1.获取核心容器对象
//        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        ClassPathXmlApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        //2.根据id获取Bean对象
        IAccountService as  = (IAccountService)ac.getBean("accountService");
        as.saveAccount();

        //手动关闭容器
        ac.close();
    }
}

```



## 9 bean的生产原理
```markdown
原理: 反射+构造方法
```


```java
 UserDAOImpl userDAO = (UserDAOImpl) Class.forName("com.yxj.dao.UserDAOImpl").newInstance();
 System.out.println(userDAO);
```
## 10 bean的生命周期
```markdown
何时创建
随着工厂启动,所有单例bean随之创建 非单例的bean,每次
使用时创建

何时销毁
工厂关闭,所有bean随之销毁( 注意: spring对多例bean管理松散,
不会负责多例bean的销毁)
```
## 11 bean工厂创建对象的好处
```markdown
1. 使用配置文件管理java类,再生产环境中更换类的实现时不需
要重新部署,修改文件即可
2. spring默认使用单例的模式创建bean,减少内存的占用
3. 通过依赖注入建立了类与类之间的关系(使java之间关系更为
清晰,方便了维护与管理)
```
## 12 现有业务层开发存在问题
```markdown
定义业务接口
```
```java
public interface UserService {
    void save(String name);
    void delete(String id);
    void update();
    String findAll(String name);
    String findOne(String id);
}
```
```markdown
实现业务接口
```
```java
public class UserServiceImpl implements UserService {
    @Override
    public void save(String name) {
        try {
            System.out.println("开启事务");
            System.out.println("处理业务逻辑,调用DAO~~~");
            System.out.println("提交事务");
        }catch (Exception e){
            System.out.println("回滚事务");
            e.printStackTrace();
        }
    }

    @Override
    public void delete(String id) {
        try {
            System.out.println("开启事务");
            System.out.println("处理业务逻辑,调用DAO~~~");
            System.out.println("提交事务");
        }catch (Exception e){
            System.out.println("回滚事务");
            e.printStackTrace();
        }
    }

    @Override
    public void update() {
        try {
            System.out.println("开启事务");
            System.out.println("处理业务逻辑,调用DAO~~~");
            System.out.println("提交事务");
        }catch (Exception e){
            System.out.println("回滚事务");
            e.printStackTrace();
        }
    }

    @Override
    public String findAll(String name) {
        try {
            System.out.println("开启事务");
            System.out.println("处理业务逻辑,调用DAO~~~");
            System.out.println("提交事务");
        }catch (Exception e){
            System.out.println("回滚事务");
            e.printStackTrace();
        }
        return name;
    }

    @Override
    public String findOne(String id) {
        try {
            System.out.println("开启事务");
            System.out.println("处理业务逻辑,调用DAO~~~");
            System.out.println("提交事务");
        }catch (Exception e){
            System.out.println("回滚事务");
            e.printStackTrace();
        }
        return id;
    }
}
```
```markdown
问题:从上图中可以看出,现有业务层中控制事务代码出现
了大量的冗余,如何解决现有业务层出现的冗余问题?
```
## 13 代理引言

### 13.1 什么是代理
```markdown
代理: 指的是java中的一种设计模式
```

### 13.2 为什么需要代理
```markdown
很多时候除了当前类能够提供的功能外，我们还需要补充一
些额外功能。
```
### 13.3 代理的作用
```markdown
代理对象可以在客户和目标对象之间起到中介作用,从而为目
标对象增添额外的功能。
```

## 14 静态代理的开发
```markdown
目标类|对象(target):被代理类称之为目标类|或者被代理的对象的
称之为目标对象
开发代理的原则: 代理类和目标类功能一致且实现相同的接口,同
时代理类中依赖于目标类对象
```
### 14.1 开发静态代理类

```java
//静态代理类
//开发原则:代理类和目标类实现相同接口,依赖于真正的目标类
public class UserServiceStaticProxy implements UserService {

    //真正的目标类 //target 原始业务逻辑对象
    private UserService userService;
    public void setUserService(UserService userService) {
        this.userService = userService;
    }

    @Override
    public void save(String name) {
        try {
            System.out.println("开启事务");
            userService.save(name);//调用真正业务逻辑方法
            System.out.println("提交事务");
        }catch (Exception e){
            System.out.println("回滚事务");
            e.printStackTrace();
        }
    }

    @Override
    public void delete(String id) {
        try {
            System.out.println("开启事务");
            userService.delete(id);//调用真正业务逻辑方法
            System.out.println("提交事务");
        }catch (Exception e){
            System.out.println("回滚事务");
            e.printStackTrace();
        }
    }

    @Override
    public void update() {
        try {
            System.out.println("开启事务");
            userService.update();//调用真正业务逻辑方法
            System.out.println("提交事务");
        }catch (Exception e){
            System.out.println("回滚事务");
            e.printStackTrace();
        }
    }

    @Override
    public String findAll(String name) {
        try {
            System.out.println("开启事务");
            String result = userService.findAll(name);//调用真正业务逻辑方法
            System.out.println("提交事务");
            return result;
        }catch (Exception e){
            System.out.println("回滚事务");
            e.printStackTrace();
        }
        return null;
    }

    @Override
    public String findOne(String id) {
        try {
            System.out.println("开启事务");
            //调用目标类方法
            String one = userService.findOne(id);//调用真正业务逻辑方法
            System.out.println("提交事务");
            return one;
        }catch (Exception e){
            System.out.println("回滚事务");
            e.printStackTrace();
        }
        return null;
    }
}
```

### 14.2 更改目标实现类

```java
public class UserServiceImpl implements UserService {
    @Override
    public void save(String name) {
        System.out.println("处理业务逻辑,调用DAO~~~");
    }

    @Override
    public void delete(String id) {
        System.out.println("处理业务逻辑,调用DAO~~~");
    }

    @Override
    public void update() {
        System.out.println("处理业务逻辑,调用DAO~~~");
    }

    @Override
    public String findAll(String name) {
        System.out.println("处理业务逻辑,调用DAO~~~");
        return name;
    }

    @Override
    public String findOne(String id) {
        System.out.println("处理业务逻辑,调用DAO~~~");
        return id;
    }
}
```

### 14.3 配置静态代理类

```xml
		<!--配置目标类-->
    <bean id="userService" class="staticproxy.UserServiceImpl"></bean>

    <!--配置代理类-->
    <bean id="userServiceStaticProxy" class="staticproxy.UserServiceStaticProxy">
        <!--注入目标对象-->
        <property name="userService" ref="userService"/>
    </bean>

```

### 14.4 调用代理方法
```java
ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
UserService userServiceStaticProxy = (UserService) context.getBean("userServiceStaticProxy");
userServiceStaticProxy.save("小黑");
```
```markdown
新的问题:往往在开发我们书写的不仅仅是一个业务层,两个
业务层,而我们的业务层会有很多,如果为每一个业务层开发一
个静态代理类,不仅没有减轻工作量,甚至让我们的工作量多了
一倍不止怎么解决以上这个问题呢?

解决方案: 为业务层在运行过程中动态创建代理类,通过动态
代理类去解决我们现有业务层中业务代码冗余的问题.
```
## 15 动态代理的原理
```markdown
通过jdk提供的Proxy这个类,动态为现有的业务生成代理类
参数一:当前线程类加载器
参数二:生成代理类的接口类型
参数三:通过代理类对象调用方法时会优先进入参数三中的invoke方Proxy.newProxyInstance(loader, interfaces, h);//返回值就是动态代理对象
```
```java
public class TestDynamicProxy {
    public static void main(String[] args) {
        final UserService userService =  new UserServiceImpl();
        //参数1:当前线程类加载器
        ClassLoader contextClassLoader = Thread.currentThread().getContextClassLoader();
        //参数2:
        Class[] classes =  new Class[]{UserService.class};
        //参数3:
        UserService userServiceProxy = (UserService) Proxy.newProxyInstance(contextClassLoader, classes, new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                try{
                    System.out.println("开启事务");//附加操作
                    Object invoke = method.invoke(userService, args);
                    System.out.println("提交事务");//附加操作
                    return invoke;
                }catch (Exception e){
                    System.out.println("回滚事务");//附加操作
                }
                return null;
            }
        });
        userServiceProxy.save("小黑");
    }
}
```



## 16 切入点表达式

### 16.1 execution方法级别的切入点表达式
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022192816274.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
注意:方法级别的切入点表达式尽可能精准,否则程序
运行可能出现异常
```

### 16.2 within类级别的切入点表达式

```markdown
1.语法
	within(包.类)

2.示例
	within(com.yxj.service.*) 
			包: com.yxj.service
			类: 所有类中所有方法不关心返回值和参数
	
	within(com.yxj.service.UserServiceImpl)
			包: com.yxj.service
			类: UserServiceImpl类中所有方法不关心返回值和参数
```
## 17 创建复杂对象
```markdown
复杂对象: 类中没有构造方法,或者构造方法不能调用如接口
类型或抽象类实例
```
```java
//1.类 implements FactoryBean<创建的类型>

	public class ConnectionFactoryBean implements FactoryBean<Connection> {
    @Override
    public Connection getObject() throws Exception {
        Class.forName("com.mysql.jdbc.Driver");
        return DriverManager.getConnection("jdbc:mysql://localhost:3306/test","root","root");
    }

    @Override
    public Class<?> getObjectType() {
        return Connection.class;
    }

    @Override
    public boolean isSingleton() {
        return false;
    }
	}

// 2.配置工厂管理
	    <bean id="connectionFactoryBean" class="com.yxj.factorybean.ConnectionFactoryBean"></bean>

// 3.获取复杂对象
		    ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
        Connection conn = (Connection) context.getBean("connectionFactoryBean");
        System.out.println(conn);
```
## 18 jdbctemplate
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200609080601635.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
pom.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.yxj</groupId>
    <artifactId>day04_yxj_01jdbctemplate</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>
    </dependencies>
</project>
```
```markdown
Account.java
```
```java
package com.yxj.domain;

import java.io.Serializable;

/**
 * 账户的实体类
 */
public class Account implements Serializable {

    private Integer id;
    private String name;
    private Float money;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Float getMoney() {
        return money;
    }

    public void setMoney(Float money) {
        this.money = money;
    }

    @Override
    public String toString() {
        return "Account{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", money=" + money +
                '}';
    }
}

```
```markdown
IAccountDao.java
```

```java
package com.yxj.dao;

import com.yxj.domain.Account;

/**
 * 账户的持久层接口
 */
public interface IAccountDao {

    /**
     * 根据Id查询账户
     * @param accountId
     * @return
     */
    Account findAccountById(Integer accountId);

    /**
     * 根据名称查询账户
     * @param accountName
     * @return
     */
    Account findAccountByName(String accountName);

    /**
     * 更新账户
     * @param account
     */
    void updateAccount(Account account);
}

```
```markdown
JdbcDaoSupport.java（此类Spring已帮我们写好）
```

```java
package com.yxj.dao.impl;

/**
 * 此类用于抽取dao中的重复代码

public class JdbcDaoSupport {

    private JdbcTemplate jdbcTemplate;

    public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    public JdbcTemplate getJdbcTemplate() {
        return jdbcTemplate;
    }


    public void setDataSource(DataSource dataSource) {
        if(jdbcTemplate == null){
            jdbcTemplate = createJdbcTemplate(dataSource);
        }
    }

    private JdbcTemplate createJdbcTemplate(DataSource dataSource){
        return new JdbcTemplate(dataSource);
    }
}
 */
```
```markdown
AccountDaoImpl.java
```
```java
package com.yxj.dao.impl;

import com.yxj.dao.IAccountDao;
import com.yxj.domain.Account;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.support.JdbcDaoSupport;

import java.util.List;

/**
 * 账户的持久层实现类
 */
public class AccountDaoImpl extends JdbcDaoSupport implements IAccountDao {

    @Override
    public Account findAccountById(Integer accountId) {
        List<Account> accounts = super.getJdbcTemplate().query("select * from account where id = ?",new BeanPropertyRowMapper<Account>(Account.class),accountId);
        return accounts.isEmpty()?null:accounts.get(0);
    }

    @Override
    public Account findAccountByName(String accountName) {
        List<Account> accounts = super.getJdbcTemplate().query("select * from account where name = ?",new BeanPropertyRowMapper<Account>(Account.class),accountName);
        if(accounts.isEmpty()){
            return null;
        }
        if(accounts.size()>1){
            throw new RuntimeException("结果集不唯一");
        }
        return accounts.get(0);
    }

    @Override
    public void updateAccount(Account account) {
        super.getJdbcTemplate().update("update account set name=?,money=? where id=?",account.getName(),account.getMoney(),account.getId());
    }
}

```
```markdown
AccountDaoImpl2.java
```
```java
package com.yxj.dao.impl;

import com.yxj.dao.IAccountDao;
import com.yxj.domain.Account;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

import java.util.List;

/**
 * 账户的持久层实现类
 */
@Repository
public class AccountDaoImpl2 implements IAccountDao {

    @Autowired
    private JdbcTemplate jdbcTemplate;


    @Override
    public Account findAccountById(Integer accountId) {
        List<Account> accounts = jdbcTemplate.query("select * from account where id = ?",new BeanPropertyRowMapper<Account>(Account.class),accountId);
        return accounts.isEmpty()?null:accounts.get(0);
    }

    @Override
    public Account findAccountByName(String accountName) {
        List<Account> accounts = jdbcTemplate.query("select * from account where name = ?",new BeanPropertyRowMapper<Account>(Account.class),accountName);
        if(accounts.isEmpty()){
            return null;
        }
        if(accounts.size()>1){
            throw new RuntimeException("结果集不唯一");
        }
        return accounts.get(0);
    }

    @Override
    public void updateAccount(Account account) {
        jdbcTemplate.update("update account set name=?,money=? where id=?",account.getName(),account.getMoney(),account.getId());
    }
}

```
```markdown
bean.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 配置账户的持久层-->
    <bean id="accountDao" class="com.yxj.dao.impl.AccountDaoImpl">
        <!--<property name="jdbcTemplate" ref="jdbcTemplate"></property>-->
        <property name="dataSource" ref="dataSource"></property>
    </bean>

  
    <!--配置JdbcTemplate
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"></property>
    </bean>-->

    <!-- 配置数据源-->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
        <property name="url" value="jdbc:mysql://localhost:3306/yxj"></property>
        <property name="username" value="root"></property>
        <property name="password" value="1234"></property>
    </bean>
</beans>
```
```markdown
JdbcTemplateDemo1.java
```
```java
package com.yxj.jdbctemplate;

import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.datasource.DriverManagerDataSource;

/**
 * JdbcTemplate的最基本用法
 */
public class JdbcTemplateDemo1 {

    public static void main(String[] args) {
        //准备数据源：spring的内置数据源
        DriverManagerDataSource ds = new DriverManagerDataSource();
        ds.setDriverClassName("com.mysql.jdbc.Driver");
        ds.setUrl("jdbc:mysql://localhost:3306/yxj");
        ds.setUsername("root");
        ds.setPassword("1234");

        //1.创建JdbcTemplate对象
        JdbcTemplate jt = new JdbcTemplate();
        //给jt设置数据源
        jt.setDataSource(ds);
        //2.执行操作
        jt.execute("insert into account(name,money)values('ccc',1000)");
    }
}

```
```markdown
JdbcTemplateDemo2.java
```

```java
package com.yxj.jdbctemplate;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.jdbc.core.JdbcTemplate;

/**
 * JdbcTemplate的最基本用法
 */
public class JdbcTemplateDemo2 {

    public static void main(String[] args) {
        //1.获取容器
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        //2.获取对象
        JdbcTemplate jt = ac.getBean("jdbcTemplate",JdbcTemplate.class);
        //3.执行操作
        jt.execute("insert into account(name,money)values('ddd',2222)");


     /*   //准备数据源：spring的内置数据源
        DriverManagerDataSource ds = new DriverManagerDataSource();
        ds.setDriverClassName("com.mysql.jdbc.Driver");
        ds.setUrl("jdbc:mysql://localhost:3306/yxj");
        ds.setUsername("root");
        ds.setPassword("1234");

        //1.创建JdbcTemplate对象
        JdbcTemplate jt = new JdbcTemplate();
        //给jt设置数据源
        jt.setDataSource(ds);
        //2.执行操作
        jt.execute("insert into account(name,money)values('ccc',1000)");*/
    }
}
	
```
```markdown
JdbcTemplateDemo3.java（增删查改）
```
```java
package com.yxj.jdbctemplate;

import com.yxj.domain.Account;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;

import java.sql.ResultSet;
import java.sql.SQLException;

/**
 * JdbcTemplate的CRUD操作
 */
public class JdbcTemplateDemo3 {

    public static void main(String[] args) {
        //1.获取容器
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        //2.获取对象
        JdbcTemplate jt = ac.getBean("jdbcTemplate",JdbcTemplate.class);
        //3.执行操作
        //保存
//        jt.update("insert into account(name,money)values(?,?)","eee",3333f);
        //更新
//        jt.update("update account set name=?,money=? where id=?","test",4567,7);
        //删除
//        jt.update("delete from account where id=?",8);
        //查询所有
//        List<Account> accounts = jt.query("select * from account where money > ?",new AccountRowMapper(),1000f);
//        List<Account> accounts = jt.query("select * from account where money > ?",new BeanPropertyRowMapper<Account>(Account.class),1000f);
//        for(Account account : accounts){
//            System.out.println(account);
//        }
        //查询一个
//        List<Account> accounts = jt.query("select * from account where id = ?",new BeanPropertyRowMapper<Account>(Account.class),1);
//        System.out.println(accounts.isEmpty()?"没有内容":accounts.get(0));

        //查询返回一行一列（使用聚合函数，但不加group by子句）
        Long count = jt.queryForObject("select count(*) from account where money > ?",Long.class,1000f);
        System.out.println(count);


    }
}

/**
 * 定义Account的封装策略
 */
class AccountRowMapper implements RowMapper<Account>{
    /**
     * 把结果集中的数据封装到Account中，然后由spring把每个Account加到集合中
     * @param rs
     * @param rowNum
     * @return
     * @throws SQLException
     */
    @Override
    public Account mapRow(ResultSet rs, int rowNum) throws SQLException {
        Account account = new Account();
        account.setId(rs.getInt("id"));
        account.setName(rs.getString("name"));
        account.setMoney(rs.getFloat("money"));
        return account;
    }
}
```
```markdown
JdbcTemplateDemo4.java
```
```java
package com.yxj.jdbctemplate;

import com.yxj.dao.IAccountDao;
import com.yxj.domain.Account;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * JdbcTemplate的最基本用法
 */
public class JdbcTemplateDemo4 {

    public static void main(String[] args) {
        //1.获取容器
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        //2.获取对象
        IAccountDao accountDao = ac.getBean("accountDao",IAccountDao.class);

        Account account = accountDao.findAccountById(1);
        System.out.println(account);

        account.setMoney(30000f);
        accountDao.updateAccount(account);
    }
}

```

## 19 事务属性

### 19.1 事务传播属性

```markdown
propagation: 传播

REQUIRED:   需要事务,外部存在事务融入当前事务,外部没有事
务,开启新的事务
SUPPORTS:   支持事务,外部存在事务融入当前事务,外部没有
事务,不开启新的事务
REQUIRES_NEW: 每次开启新的事务,如果外部存在事务外部事
务挂起,开启新的事务运行,运行结束后回复外部事务
NOT_SUPPORTED: 不支持事务,如果外部存在事务外部事务挂
起,已非事务方式运行.
NEVER:				 不支持事务,存在事务报错
MANDATORY:		 强制事务没有事务报错
NESTED:				 嵌套事务,数据库不支持
```

### 19.2 事务的隔离级别

```markdown
isolation 隔离级别

DEFAULT: 采用数据库默认隔离级别
READ_UNCOMMITTED: 读未提交 
READ_COMMITTED: 读提交 用来避免脏读现象出现的   
oracle默认隔离级别
REPEATABLE_READ: 可重复读主要是用来避免不可重复读现象
出现的 (在一次事务中一方更新,导致两次查询结果不一致这种
情况叫不可重复读)  mysql默认隔离级别
SERIALIZABLE   : 序列化读 用来避免幻影读现象出现          (在一次事务中一方插入,导致两次查询结果不一致这种情况叫幻影读)
```
### 19.3 读写和异常性

```xml
readonly 
   true:    本次事务只读
   false:   本次事务非只读
	<tx:method name="save*" propagation="REQUIRES_NEW" read-only="true|false" isolation="SERIALIZABLE"/>
	
rollback-for && no-rollback-for=""
		rollback-for: 遇到什么类异常回滚
		no-rollback-for: 遇到什么类异常不回滚
   	<tx:method name="save*" rollback-for="" no-rollback-for="" propagation="REQUIRES_NEW" read-only="true" isolation="SERIALIZABLE"/>

timeout 超时性
		timeout: -1 永不超时
```
### 19.4 代码
#### 19.4.1 spring中基于XML的声明式事务控制配置步骤

```markdown
pom.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.yxj</groupId>
    <artifactId>day04_yxj_05tx_xml</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>

        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.7</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>

</project>
```
```markdown
IAccountService.java
```
```java
package com.yxj.service;

import com.yxj.domain.Account;

/**
 * 账户的业务层接口
 */
public interface IAccountService {
    /**
     * 根据id查询账户信息
     * @param accountId
     * @return
     */
    Account findAccountById(Integer accountId);

    /**
     * 转账
     * @param sourceName    转成账户名称
     * @param targetName    转入账户名称
     * @param money         转账金额
     */
    void transfer(String sourceName,String targetName,Float money);
}

```
```markdown
AccountServiceImpl.java
```
```java
package com.yxj.service.impl;

import com.yxj.dao.IAccountDao;
import com.yxj.domain.Account;
import com.yxj.service.IAccountService;

/**
 * 账户的业务层实现类
 *
 * 事务控制应该都是在业务层
 */
public class AccountServiceImpl implements IAccountService{

    private IAccountDao accountDao;

    public void setAccountDao(IAccountDao accountDao) {
        this.accountDao = accountDao;
    }

    @Override
    public Account findAccountById(Integer accountId) {
        return accountDao.findAccountById(accountId);

    }



    @Override
    public void transfer(String sourceName, String targetName, Float money) {
        System.out.println("transfer....");
            //2.1根据名称查询转出账户
            Account source = accountDao.findAccountByName(sourceName);
            //2.2根据名称查询转入账户
            Account target = accountDao.findAccountByName(targetName);
            //2.3转出账户减钱
            source.setMoney(source.getMoney()-money);
            //2.4转入账户加钱
            target.setMoney(target.getMoney()+money);
            //2.5更新转出账户
            accountDao.updateAccount(source);

            int i=1/0;

            //2.6更新转入账户
            accountDao.updateAccount(target);
    }
}

```
```markdown
IAccountDao.java
```
```java
package com.yxj.dao;

import com.yxj.domain.Account;

/**
 * 账户的持久层接口
 */
public interface IAccountDao {

    /**
     * 根据Id查询账户
     * @param accountId
     * @return
     */
    Account findAccountById(Integer accountId);

    /**
     * 根据名称查询账户
     * @param accountName
     * @return
     */
    Account findAccountByName(String accountName);

    /**
     * 更新账户
     * @param account
     */
    void updateAccount(Account account);
}

```
```markdown
AccountDaoImpl.java
```
```java
package com.yxj.dao.impl;

import com.yxj.dao.IAccountDao;
import com.yxj.domain.Account;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.support.JdbcDaoSupport;

import java.util.List;

/**
 * 账户的持久层实现类
 */
public class AccountDaoImpl extends JdbcDaoSupport implements IAccountDao {

    @Override
    public Account findAccountById(Integer accountId) {
        List<Account> accounts = super.getJdbcTemplate().query("select * from account where id = ?",new BeanPropertyRowMapper<Account>(Account.class),accountId);
        return accounts.isEmpty()?null:accounts.get(0);
    }

    @Override
    public Account findAccountByName(String accountName) {
        List<Account> accounts = super.getJdbcTemplate().query("select * from account where name = ?",new BeanPropertyRowMapper<Account>(Account.class),accountName);
        if(accounts.isEmpty()){
            return null;
        }
        if(accounts.size()>1){
            throw new RuntimeException("结果集不唯一");
        }
        return accounts.get(0);
    }

    @Override
    public void updateAccount(Account account) {
        super.getJdbcTemplate().update("update account set name=?,money=? where id=?",account.getName(),account.getMoney(),account.getId());
    }
}

```
```markdown
bean.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 配置业务层-->
    <bean id="accountService" class="com.yxj.service.impl.AccountServiceImpl">
        <property name="accountDao" ref="accountDao"></property>
    </bean>

    <!-- 配置账户的持久层-->
    <bean id="accountDao" class="com.yxj.dao.impl.AccountDaoImpl">
        <property name="dataSource" ref="dataSource"></property>
    </bean>


    <!-- 配置数据源-->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
        <property name="url" value="jdbc:mysql://localhost:3306/yxj"></property>
        <property name="username" value="root"></property>
        <property name="password" value="1234"></property>
    </bean>

    <!-- spring中基于XML的声明式事务控制配置步骤
        1、配置事务管理器
        2、配置事务的通知
                此时我们需要导入事务的约束 tx名称空间和约束，同时也需要aop的
                使用tx:advice标签配置事务通知
                    属性：
                        id：给事务通知起一个唯一标识
                        transaction-manager：给事务通知提供一个事务管理器引用
        3、配置AOP中的通用切入点表达式
        4、建立事务通知和切入点表达式的对应关系
        5、配置事务的属性
               是在事务的通知tx:advice标签的内部

     -->
    <!-- 配置事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>

    <!-- 配置事务的通知-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <!-- 配置事务的属性
                isolation：用于指定事务的隔离级别。默认值是DEFAULT，表示使用数据库的默认隔离级别。
                propagation：用于指定事务的传播行为。默认值是REQUIRED，表示一定会有事务，增删改的选择。查询方法可以选择SUPPORTS。
                read-only：用于指定事务是否只读。只有查询方法才能设置为true。默认值是false，表示读写。
                timeout：用于指定事务的超时时间，默认值是-1，表示永不超时。如果指定了数值，以秒为单位。
                rollback-for：用于指定一个异常，当产生该异常时，事务回滚，产生其他异常时，事务不回滚。没有默认值。表示任何异常都回滚。
                no-rollback-for：用于指定一个异常，当产生该异常时，事务不回滚，产生其他异常时事务回滚。没有默认值。表示任何异常都回滚。
        -->
        <tx:attributes>
            <tx:method name="*" propagation="REQUIRED" read-only="false"/>
            <tx:method name="find*" propagation="SUPPORTS" read-only="true"></tx:method>
        </tx:attributes>
    </tx:advice>

    <!-- 配置aop-->
    <aop:config>
        <!-- 配置切入点表达式-->
        <aop:pointcut id="pt1" expression="execution(* com.yxj.service.impl.*.*(..))"></aop:pointcut>
        <!--建立切入点表达式和事务通知的对应关系 -->
        <aop:advisor advice-ref="txAdvice" pointcut-ref="pt1"></aop:advisor>
    </aop:config> 

</beans>
```
```markdown
AccountServiceTest.java
```
```java
package com.yxj.test;

import com.yxj.service.IAccountService;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

/**
 * 使用Junit单元测试：测试我们的配置
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:bean.xml")
public class AccountServiceTest {

    @Autowired
    private  IAccountService as;

    @Test
    public  void testTransfer(){
        as.transfer("aaa","bbb",100f);

    }

}
```
#### 19.4.2 spring中基于注解的声明式事务控制配置步骤
```markdown
pom.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.yxj</groupId>
    <artifactId>day04_yxj_06tx_anno</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>

        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.7</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>

</project>
```
```markdown
Account.java
```

```java
package com.yxj.domain;

import java.io.Serializable;

/**
 * 账户的实体类
 */
public class Account implements Serializable {

    private Integer id;
    private String name;
    private Float money;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Float getMoney() {
        return money;
    }

    public void setMoney(Float money) {
        this.money = money;
    }

    @Override
    public String toString() {
        return "Account{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", money=" + money +
                '}';
    }
}

```
```markdown
IAccountService.java
```
```java
package com.yxj.service;

import com.yxj.domain.Account;

/**
 * 账户的业务层接口
 */
public interface IAccountService {
    /**
     * 根据id查询账户信息
     * @param accountId
     * @return
     */
    Account findAccountById(Integer accountId);

    /**
     * 转账
     * @param sourceName    转成账户名称
     * @param targetName    转入账户名称
     * @param money         转账金额
     */
    void transfer(String sourceName,String targetName,Float money);
}

```
```markdown
AccountServiceImpl.java
```
```java
package com.yxj.service.impl;

import com.yxj.dao.IAccountDao;
import com.yxj.domain.Account;
import com.yxj.service.IAccountService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Propagation;
import org.springframework.transaction.annotation.Transactional;

/**
 * 账户的业务层实现类
 *
 * 事务控制应该都是在业务层
 */
@Service("accountService")
@Transactional(propagation= Propagation.SUPPORTS,readOnly=true)//只读型事务的配置
public class AccountServiceImpl implements IAccountService{

    @Autowired
    private IAccountDao accountDao;


    @Override
    public Account findAccountById(Integer accountId) {
        return accountDao.findAccountById(accountId);

    }


    //需要的是读写型事务配置
    @Transactional(propagation= Propagation.REQUIRED,readOnly=false)
    @Override
    public void transfer(String sourceName, String targetName, Float money) {
        System.out.println("transfer....");
            //2.1根据名称查询转出账户
            Account source = accountDao.findAccountByName(sourceName);
            //2.2根据名称查询转入账户
            Account target = accountDao.findAccountByName(targetName);
            //2.3转出账户减钱
            source.setMoney(source.getMoney()-money);
            //2.4转入账户加钱
            target.setMoney(target.getMoney()+money);
            //2.5更新转出账户
            accountDao.updateAccount(source);

            int i=1/0;

            //2.6更新转入账户
            accountDao.updateAccount(target);
    }
}

```
```markdown
IAccountDao.java
```

```java
package com.yxj.dao;

import com.yxj.domain.Account;

/**
 * 账户的持久层接口
 */
public interface IAccountDao {

    /**
     * 根据Id查询账户
     * @param accountId
     * @return
     */
    Account findAccountById(Integer accountId);

    /**
     * 根据名称查询账户
     * @param accountName
     * @return
     */
    Account findAccountByName(String accountName);

    /**
     * 更新账户
     * @param account
     */
    void updateAccount(Account account);
}

```
```markdown
AccountDaoImpl.java
```
```java
package com.yxj.dao.impl;

import com.yxj.dao.IAccountDao;
import com.yxj.domain.Account;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

import java.util.List;

/**
 * 账户的持久层实现类
 */
@Repository("accountDao")
public class AccountDaoImpl implements IAccountDao {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Override
    public Account findAccountById(Integer accountId) {
        List<Account> accounts = jdbcTemplate.query("select * from account where id = ?",new BeanPropertyRowMapper<Account>(Account.class),accountId);
        return accounts.isEmpty()?null:accounts.get(0);
    }

    @Override
    public Account findAccountByName(String accountName) {
        List<Account> accounts = jdbcTemplate.query("select * from account where name = ?",new BeanPropertyRowMapper<Account>(Account.class),accountName);
        if(accounts.isEmpty()){
            return null;
        }
        if(accounts.size()>1){
            throw new RuntimeException("结果集不唯一");
        }
        return accounts.get(0);
    }

    @Override
    public void updateAccount(Account account) {
        jdbcTemplate.update("update account set name=?,money=? where id=?",account.getName(),account.getMoney(),account.getId());
    }
}

```
```markdown
bean.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 配置spring创建容器时要扫描的包-->
    <context:component-scan base-package="com.yxj"></context:component-scan>

    <!-- 配置JdbcTemplate-->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"></property>
    </bean>



    <!-- 配置数据源-->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
        <property name="url" value="jdbc:mysql://localhost:3306/yxj"></property>
        <property name="username" value="root"></property>
        <property name="password" value="1234"></property>
    </bean>

    <!-- spring中基于注解 的声明式事务控制配置步骤
        1、配置事务管理器
        2、开启spring对注解事务的支持
        3、在需要事务支持的地方使用@Transactional注解


     -->
    <!-- 配置事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>



    <!-- 开启spring对注解事务的支持-->
    <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>

</beans>
```

```markdown
AccountServiceTest.java
```
```java
package com.yxj.test;

import com.yxj.service.IAccountService;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

/**
 * 使用Junit单元测试：测试我们的配置
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:bean.xml")
public class AccountServiceTest {

    @Autowired
    private  IAccountService as;

    @Test
    public  void testTransfer(){
        as.transfer("aaa","bbb",100f);

    }

}

```

## 20 Spring的注解式开发

### 20.1 开启注解扫描配置

```xml
	<!--开启注解扫描-->
  <context:component-scan base-package="com"/>
```

### 20.2 实例化相关注解
```markdown
Spring自2.5版本以后提供了一些注解用来在使用spring时进行快速开发
```
```java
1. @Component(value="beanid")
修饰范围:    用在类上
注解作用:    通用的创建实例的注解,用来创建当前这个类的实例
value属性:	用来指定创建的对象在工厂中的唯一标识   
如果不指定默认创建对象在工厂中的标识为类名首字母小写
				
2. @Repository 
修饰范围:    用在类上
注解作用:    @component的子类注解专用于DAO组件的创建,
通常加在DAO组件上
value属性:	用来指定创建的对象在工厂中的唯一标识   
如果不指定默认创建对象在工厂中的标识为类名首字母小写

3. @Service
修饰范围:    用在类上
注解作用:    @component的子类注解专用于Service组件的创建,
通常加在Service组件上
value属性:	用来指定创建的对象在工厂中的唯一标识   
如果不指定默认创建对象在工厂中的标识为类名首字母小写

4. @Controller
修饰范围:    用在类上
注解作用:    @component的子类注解专用于Action组件的创建,
通常加在Action组件上
value属性:	用来指定创建的对象在工厂中的唯一标识   
如果不指定默认创建对象在工厂中的标识为类名首字母小写
```


### 20.3 控制对象的创建次数的注解

```markdown
1. @Scope(value="singleton|prototype")
修饰范围:    用在类上
注解作用:    用来控制这个实例在工厂中的创建次数
value属性:	singleton为单例,prototype为多例   默认单例
```

### 20.4 注入相关的注解

```markdown
1. @Autowired(Spring提供)
修饰范围:    用在成员变量或成员变量的GET/SET方法上
注解作用:		 用来给类中成员变量赋值
注入原则:    默认根据类型自动注入

2. @Resource(JAVAEE提供)
修饰范围:    用在成员变量或成员变量的GET/SET方法上
注解作用:		 用来给类中成员变量赋值
注入原则:    默认根据名称自动注入名称找不到根据类型自动注入
```

### 20.5 控制事务的相关注解

```markdown
1. @Transactional
修饰范围:    用在类上主要用在业务层组件类上或者是方法上
注解作用:	 用来给类中方法加入事务,当类上和方法上同
时存在该注解时局部优先
注解属性:    
propagation  用来控制传播属性
Isolation    用来控制隔离级别
timeout      用来设置超时性
rollback-for 用来设置什么异常回滚
norollback-for 用来设置什么异常不会滚
readonly     用来设置事务读写性
```
```markdown
注意:如果要使用事务注解在配置文件中必须开启事务注解生
效加入如下配置:
```


```xml
<tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
```

### 20.6 代码示例1
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200604165418787.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
pom.xml
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.yxj</groupId>
    <artifactId>day02_yxj_01anno_ioc</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
    </dependencies>
</project>
```
```markdown
bean.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <!--告知spring在创建容器时要扫描的包，配置所需要的标签不是在beans的约束中，而是一个名称为
    context名称空间和约束中,作用就是扫描指定包下的注解-->
    <context:component-scan base-package="com.yxj"></context:component-scan>
</beans>
```



```markdown
IAccountService.java
```
```java
package com.yxj.service;

/**
 * 账户业务层的接口
 */
public interface IAccountService {

    /**
     * 模拟保存账户
     */
    void saveAccount();
}

```
```markdown
AccountServiceImpl.java
```
```java
package com.yxj.service.impl;

import com.yxj.dao.IAccountDao;
import com.yxj.service.IAccountService;
import org.springframework.stereotype.Service;

import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;
import javax.annotation.Resource;

/**
 * 账户的业务层实现类
 *
 * 曾经XML的配置：
 *  <bean id="accountService" class="com.yxj.service.impl.AccountServiceImpl"
 *        scope=""  init-method="" destroy-method="">
 *      <property name=""  value="" | ref=""></property>
 *  </bean>
 *
 * 用于创建对象的
 *      他们的作用就和在XML配置文件中编写一个<bean>标签实现的功能是一样的
 *      Component:
 *          作用：用于把当前类对象存入spring容器中
 *          属性：
 *              value：用于指定bean的id。当我们不写时，它的默认值是当前类名，且首字母改小写。
 *      Controller：一般用在表现层
 *      Service：一般用在业务层
 *      Repository：一般用在持久层
 *      以上三个注解他们的作用和属性与Component是一模一样。
 *      他们三个是spring框架为我们提供明确的三层使用的注解，使我们的三层对象更加清晰
 *
 *
 * 用于注入数据的
 *      他们的作用就和在xml配置文件中的bean标签中写一个<property>标签的作用是一样的
 *      Autowired:
 *          作用：自动按照类型注入。只要容器中有唯一的一个bean对象类型和要注入的变量类型匹配，就可以注入成功
 *                如果ioc容器中没有任何bean的类型和要注入的变量类型匹配，则报错。
 *                如果Ioc容器中有多个类型匹配时,再按变量名称匹配：
 *          出现位置：
 *              可以是变量上，也可以是方法上
 *          细节：
 *              在使用注解注入时，set方法就不是必须的了。
 *      Qualifier:
 *          作用：在按照类中注入的基础之上再按照名称注入。它在给类成员注入时不能单独使用。但是在给方法参数注入时可以（用在方法上时表示注入指定名称的bean）
 *          属性：
 *              value：用于指定注入bean的id。
 *      Resource
 *          作用：直接按照bean的id注入。它可以独立使用
 *          属性：
 *              name：用于指定bean的id。
 *      以上三个注入都只能注入其他bean类型的数据，而基本类型和String类型无法使用上述注解实现。
 *      另外，集合类型的注入只能通过XML来实现。
 *
 *      Value
 *          作用：用于注入基本类型和String类型的数据
 *          属性：
 *              value：用于指定数据的值。它可以使用spring中SpEL(也就是spring的el表达式）
 *                      SpEL的写法：${表达式}
 *
 * 用于改变作用范围的
 *      他们的作用就和在bean标签中使用scope属性实现的功能是一样的
 *      Scope
 *          作用：用于指定bean的作用范围
 *          属性：
 *              value：指定范围的取值。常用取值：singleton prototype
 *
 * 和生命周期相关 了解
 *      他们的作用就和在bean标签中使用init-method和destroy-methode的作用是一样的
 *      PreDestroy
 *          作用：用于指定销毁方法
 *      PostConstruct
 *          作用：用于指定初始化方法
 */
@Service("accountService")
//@Scope("prototype")
public class AccountServiceImpl implements IAccountService {

//    @Autowired
//    @Qualifier("accountDao1")
    @Resource(name = "accountDao2")
    private IAccountDao accountDao = null;

    @PostConstruct
    public void  init(){
        System.out.println("初始化方法执行了");
    }

    @PreDestroy
    public void  destroy(){
        System.out.println("销毁方法执行了");
    }

    public void  saveAccount(){
        accountDao.saveAccount();
    }
}

```
```markdown
IAccountDao.java
```
```java
package com.yxj.dao;

/**
 * 账户的持久层接口
 */
public interface IAccountDao {

    /**
     * 模拟保存账户
     */
    void saveAccount();
}

```
```markdown
AccountDaoImpl.java
```
```java
package com.yxj.dao.impl;

import com.yxj.dao.IAccountDao;
import org.springframework.stereotype.Repository;

/**
 * 账户的持久层实现类
 */
@Repository("accountDao1")
public class AccountDaoImpl implements IAccountDao

    {

    public  void saveAccount(){

        System.out.println("保存了账户1111111111111");
    }
}

```
```markdown
AccountDaoImpl2.java
```
```java
package com.yxj.dao.impl;

import com.yxj.dao.IAccountDao;
import org.springframework.stereotype.Repository;

/**
 * 账户的持久层实现类
 */
@Repository("accountDao2")
public class AccountDaoImpl2  implements IAccountDao {

    public  void saveAccount(){

        System.out.println("保存了账户2222222222222");
    }
}

```
```markdown
Client.java
```
```java
package com.yxj.ui;

import com.yxj.service.IAccountService;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * 模拟一个表现层，用于调用业务层
 */
public class Client {

    /**
     * @param args
     */
    public static void main(String[] args) {
        //1.获取核心容器对象
//        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        ClassPathXmlApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        //2.根据id获取Bean对象
        IAccountService as  = (IAccountService)ac.getBean("accountService");
//        IAccountService as2  = (IAccountService)ac.getBean("accountService");
//        System.out.println(as);
//        IAccountDao adao = ac.getBean("accountDao",IAccountDao.class);
//        System.out.println(adao);
//        System.out.println(as == as2);
        as.saveAccount();
        ac.close();
    }
}

```
### 20.7 代码事例2
```markdown
pom.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.yxj</groupId>
    <artifactId>day02_yxj_04account_annoioc_withoutxml</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>commons-dbutils</groupId>
            <artifactId>commons-dbutils</artifactId>
            <version>1.4</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>

        <dependency>
            <groupId>c3p0</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.1.2</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>

</project>
```
```markdown
jdbcConfig.properties
```
```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/yxj
jdbc.username=root
jdbc.password=1234
```
```markdown
JdbcConfig.java
```
```java
package config;

import com.mchange.v2.c3p0.ComboPooledDataSource;
import org.apache.commons.dbutils.QueryRunner;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Scope;

import javax.sql.DataSource;

/**
 * 和spring连接数据库相关的配置类
 */
public class JdbcConfig {

    @Value("${jdbc.driver}")
    private String driver;

    @Value("${jdbc.url}")
    private String url;

    @Value("${jdbc.username}")
    private String username;

    @Value("${jdbc.password}")
    private String password;

    /**
     * 用于创建一个QueryRunner对象
     * @param dataSource
     * @return
     */
    @Bean(name="runner")
    @Scope("prototype")
    public QueryRunner createQueryRunner(@Qualifier("ds2") DataSource dataSource){
        return new QueryRunner(dataSource);
    }

    /**
     * 创建数据源对象
     * @return
     */
    @Bean(name="ds2")
    public DataSource createDataSource(){
        try {
            ComboPooledDataSource ds = new ComboPooledDataSource();
            ds.setDriverClass(driver);
            ds.setJdbcUrl(url);
            ds.setUser(username);
            ds.setPassword(password);
            return ds;
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }

    @Bean(name="ds1")
    public DataSource createDataSource1(){
        try {
            ComboPooledDataSource ds = new ComboPooledDataSource();
            ds.setDriverClass(driver);
            ds.setJdbcUrl("jdbc:mysql://localhost:3306/yxj02");
            ds.setUser(username);
            ds.setPassword(password);
            return ds;
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }
}

```
```markdown
SpringConfiguration.java
```
```java
package config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Import;
import org.springframework.context.annotation.PropertySource;

/**
 * 该类是一个配置类，它的作用和bean.xml是一样的
 * spring中的新注解
 * Configuration
 *     作用：指定当前类是一个配置类
 *     细节：当配置类作为AnnotationConfigApplicationContext对象创建的参数时，该注解可以不写。
 * ComponentScan
 *      作用：用于通过注解指定spring在创建容器时要扫描的包
 *      属性：
 *          value：它和basePackages的作用是一样的，都是用于指定创建容器时要扫描的包。
 *                 我们使用此注解就等同于在xml中配置了:
 *                      <context:component-scan base-package="com.yxj"></context:component-scan>
 *  Bean
 *      作用：用于把当前方法的返回值作为bean对象存入spring的ioc容器中
 *      属性:
 *          name:用于指定bean的id。当不写时，默认值是当前方法的名称
 *      细节：
 *          当我们使用注解配置方法时，如果方法有参数，spring框架会去容器中查找有没有可用的bean对象。
 *          查找的方式和Autowired注解的作用是一样的
 *  Import
 *      作用：用于导入其他的配置类
 *      属性：
 *          value：用于指定其他配置类的字节码。
 *                  当我们使用Import的注解之后，有Import注解的类就父配置类，而导入的都是子配置类
 *  PropertySource
 *      作用：用于指定properties文件的位置
 *      属性：
 *          value：指定文件的名称和路径。
 *                  关键字：classpath，表示类路径下
 */
//@Configuration
@ComponentScan("com.yxj")
@Import(JdbcConfig.class)
@PropertySource("classpath:jdbcConfig.properties")
public class SpringConfiguration {

}
```
```markdown
QueryRunnerTest.java
```
```java
package com.yxj.test;

import config.SpringConfiguration;
import org.apache.commons.dbutils.QueryRunner;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

/**
 * 测试queryrunner是否单例
 */
public class QueryRunnerTest {

    @Test
    public  void  testQueryRunner(){
        //1.获取容易
        ApplicationContext ac = new AnnotationConfigApplicationContext(SpringConfiguration.class);
        //2.获取queryRunner对象
        QueryRunner runner = ac.getBean("runner",QueryRunner.class);
        QueryRunner runner1 = ac.getBean("runner",QueryRunner.class);
        System.out.println(runner == runner1);
    }
}

```
```markdown
AccountServiceTest.java
```
```java
package com.yxj.test;

import com.yxj.domain.Account;
import com.yxj.service.IAccountService;
import config.SpringConfiguration;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import java.util.List;

/**
 * 使用Junit单元测试：测试我们的配置
 * Spring整合junit的配置
 *      1、导入spring整合junit的jar(坐标)
 *      2、使用Junit提供的一个注解把原有的main方法替换了，替换成spring提供的
 *             @Runwith
 *      3、告知spring的运行器，spring和ioc创建是基于xml还是注解的，并且说明位置
 *          @ContextConfiguration
 *                  locations：指定xml文件的位置，加上classpath关键字，表示在类路径下
 *                  classes：指定注解类所在地位置
 *
 *   当我们使用spring 5.x版本的时候，要求junit的jar必须是4.12及以上
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfiguration.class)
public class AccountServiceTest {

    @Autowired
    private IAccountService as = null;


    @Test
    public void testFindAll() {
        //3.执行方法
        List<Account> accounts = as.findAllAccount();
        for(Account account : accounts){
            System.out.println(account);
        }
    }

    @Test
    public void testFindOne() {
        //3.执行方法
        Account account = as.findAccountById(1);
        System.out.println(account);
    }

    @Test
    public void testSave() {
        Account account = new Account();
        account.setName("test anno");
        account.setMoney(12345f);
        //3.执行方法
        as.saveAccount(account);

    }

    @Test
    public void testUpdate() {
        //3.执行方法
        Account account = as.findAccountById(4);
        account.setMoney(23456f);
        as.updateAccount(account);
    }

    @Test
    public void testDelete() {
        //3.执行方法
        as.deleteAccount(4);
    }
}

```

## 21 SM整合思路

### 21.1 spring框架的作用
```markdown
spring框架  项目管理框架  主要负责项目中组件对象的创建
```

### 21.2 Mybatis框架的作用
```markdown
Mybatis框架	   持久层框架        主要用来简化数据库访问的操作
```

### 21.3 整合思路
```markdown
整合思路: 两个框架作用不同,貌似没有什么联系,更深入看才能
看出所谓Spring整合Mybatis,其实就是通过spring框架接管
mybatis框架中核心对象的创建
```

### 21.4 mybatis中的核心对象有哪些
```markdown
Mybatis的核心对象为:  SqlSessionFactory 整合就是通
过Spring管理SqlSessionFactory对象的创建
```

### 21.5 整合思路图示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022193232104.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


## 22 SM整合DAO编程步骤

### 22.1 引入mybatis的依赖jar包

```xml
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>3.2.8</version>
</dependency>
```

### 22.2 引入Spring相关jar包

```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-core</artifactId>
  <version>4.3.2.RELEASE</version>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context</artifactId>
  <version>4.3.2.RELEASE</version>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context-support</artifactId>
  <version>4.3.2.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>4.3.2.RELEASE</version>
  </dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-aop</artifactId>
  <version>4.3.2.RELEASE</version>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-beans</artifactId>
  <version>4.3.2.RELEASE</version>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-expression</artifactId>
  <version>4.3.2.RELEASE</version>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-aspects</artifactId>
  <version>4.3.2.RELEASE</version>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-tx</artifactId>
  <version>4.3.2.RELEASE</version>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-web</artifactId>
  <version>4.3.2.RELEASE</version>
</dependency>
```

### 22.3 mybatis-spring整合jar

```xml
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis-spring</artifactId>
  <version>1.3.1</version>
</dependency>
```

### 22.4 引入数据库驱动jar

```xml
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>5.1.40</version>
</dependency>
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.12</version>
</dependency>
```

### 22.5 建表

```sql
CREATE TABLE `t_user` (
  `id` varchar(40) NOT NULL,
  `name` varchar(40) DEFAULT NULL,
  `age` int(3) DEFAULT NULL,
  `bir` timestamp NULL DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

### 22.6 编写实体类

```java
public class User {
    private String id;
    private String name;
    private Integer age;
    private Date bir;
		//......
}
```

### 22.7 书写DAO接口

```java
public interface UserDAO {
    //查询所有
    List<User> findAll();
}
```

### 22.8 编写mapper配置文件

```xml
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.yxj.dao.UserDAO">
    
    <select id="findAll" resultType="com.yxj.entity.User">
        select id,name,age,bir from t_user
    </select>

</mapper>
```

### 22.9 编写Spring-myabtis整合配置文件

```xml
		<!--创建数据源-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/sm"/>
        <property name="username" value="root"/>
        <property name="password" value="root"/>
    </bean>

    <!--创建sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <property name="mapperLocations" >
            <array>
                <value>classpath:com/yxj/mapper/UserDAOMapper.xml</value>
            </array>
        </property>
    </bean>

    <!--创建DAO-->
    <bean id="userDAO" class="org.mybatis.spring.mapper.MapperFactoryBean">
        <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
        <property name="mapperInterface" value="com.yxj.dao.UserDAO"/>
    </bean>
```

### 22.10 启动工厂测试
```java
ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
UserDAO userDAO = (UserDAO) context.getBean("userDAO");
List<User> users = userDAO.findAll();
for (User user : users) {
  System.out.println(user);
}
```

## 23 SM整合Service编程步骤

### 23.1 编写spring-mybatis.xml配置文件

```xml
<!--创建事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!--配置事务属性-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="save*"/>
            <tx:method name="update*"/>
            <tx:method name="delete*"/>
            <tx:method name="find*"/>
        </tx:attributes>
    </tx:advice>

    <!--配置事务切面-->
    <aop:config>
        <aop:pointcut id="pc" expression="within(com.yxj.service.*)"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="pc"/>
    </aop:config>
```

### 23.2 配置图示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022193158762.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


### 23.3 创建service接口

```java
public interface UserService {
    List<User> findAll();
}
```

### 23.4 创建service对象

```java
public class UserServiceImpl implements UserService {
    
    private UserDAO userDAO;
    public void setUserDAO(UserDAO userDAO) {
        this.userDAO = userDAO;
    }

    @Override
    public List<User> findAll() {
        return userDAO.findAll();
    }
}
```

### 23.5 配置业务组件对象

```xml
  <!--配置Service组件-->
    <bean id="userService" class="com.yxj.service.UserServiceImpl">
        <property name="userDAO" ref="userDAO"/>
    </bean>
```
## 24 图片

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123184053579.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123184112185.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123184306302.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123184451774.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123184511164.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123184611104.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123184634382.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021012318470349.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123184750509.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123184809331.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123184838970.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021012318485871.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123184913654.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123184932138.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123185946332.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123190014190.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123190041827.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123190121687.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123190140165.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123190234160.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123190254799.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123190512558.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123190529689.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123190555803.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123190619477.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123190633138.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123190740192.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021012320015738.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123200213527.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123200253338.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123200732791.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123200825565.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123200857886.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123200915672.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123201116889.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123202007689.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123202041995.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123202113111.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123202129100.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123202147530.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/202101232022104.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123202334191.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123202356676.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123202442774.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复
spring即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
