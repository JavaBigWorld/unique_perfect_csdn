# Spring Boot
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210717140219850.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1 springboot的引言
```markdown
Spring Boot是由Pivotal团队提供的全新框架，其设计目的是用来
简化Spring应用的初始搭建以及开发过程。该框架使用了特定
的方式来进行配置，从而使开发人员不 再需要定义样板化的配置。
通过这种方式，Spring Boot致力于在蓬勃发展的快速应用
开发领域(rapid application development)成为领导者。

springboot(微框架) = springmvc(控制器) + spring core(项目管理)

SSM Spring springmvc  mybatis    <----   SSM Spring  stauts2|struts1  mybatis  <--- SSH Spring Struts  Hibernate 
```

## 2 springboot的特点
```markdown
1 创建独立的Spring应用程序  
2 嵌入的Tomcat，无需部署WAR文件
3 简化Maven配置
4 自动配置Spring,没有XML配置
```
## 3 springboot 的约定大于配置

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022231302312.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
springboot 项目中必须在src/main/resources中放入application.yml(.properties)
核心配置文件  名字必须为:application

springboot 项目中必须在src/main/java中所有子包之外构建
全局入口类型,xxApplication,入口类一个springboot项目只能
有一个
```

## 4 springboot的环境搭建
```markdown
环境要求:
# 1.System Requirements
		JDK18.+
		MAVEN3.3  or  Gradle 5.x and 6.x (4.10 is also supported but in a deprecated form)
		Spring Framework 5.2.4.RELEASE 

# 2.ServletContainers: 
		Tomcat 9.0 
    Jetty 9.4 
    Undertow 2.0

# 3.开发工具
		IDEA 2018版本 
		Eclipse 版本  17版本之后
```

### 4.1 项目中引入依赖

```xml
    <!--继承springboot的父项目-->
   <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.5.RELEASE</version>
    </parent>

    <dependencies>
        <!--引入springboot的web支持-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```

### 4.2 引入配置文件
```markdown
项目中src/main/resources/application.yml
```

### 4.3 建包并创建控制器

```java
//在项目中创建指定的包结构
/*
	 com
	    +| yxj
	    		+| controller */ 
                	@Controller
                    @RequestMapping("/hello")
                    public class HelloController {
                        @RequestMapping("/hello")
                        @ResponseBody
                        public String hello(){
                            System.out.println("======hello world=======");
                            return "hello";
                        }
                    }
               	    		  		
```

### 4.4 编写入口类

```java
//在项目中如下的包结构中创建入口类 Application
/*
	com
		+| yxj                  */
            @SpringBootApplication
            public class Application {
                public static void main(String[] args) {
                    SpringApplication.run(Application.class,args);
                }
            }
```

### 4.5 运行main启动项目

```java
o.s.j.e.a.AnnotationMBeanExporter        : Registering beans for JMX exposure on startup
s.b.c.e.t.TomcatEmbeddedServletContainer : Tomcat started on port(s): 8989 (http)
com.yxj.Application : Started Application in 2.152 seconds (JVM running for 2.611)

//说明:  出现以上日志说明启动成功
```

### 4.6 访问项目 

```java
//注意: springboot的项目默认没有项目名
//访问路径:  http://localhost:8080/hello/hello
```

## 5 启动tomcat端口占用问题

```yml
server:
  port: 8989                 #用来指定内嵌服务器端口号
  context-path: /springboot  #用来指定项目的访问路径
```

## 6 springboot相关注解说明

```markdown
# Spring boot通常有一个名为 xxxApplication的类,入口类中有一
个main方法, 在main方法中使用SpringApplication.run(xxxApplication.class,args)
启动springboot应用的项目。

# @RestController: 就是@Controller+@ResponseBody组合，
支持RESTful访问方 式，返回结果都是json字符串。

# @SpringBootApplication 注解等价于: 
@SpringBootConfiguration    标识注解,标识这是一个springboot的配置类
@EnableAutoConfiguration 自动与项目中集成的第三方技术进行集成
@ComponentScan			扫描入口类所在子包以及子包后代包中注解
```


## 7 springboot中配置文件的拆分

```yml
#说明: 在实际开发过程中生产环境和测试环境有可能是不一样的 因此将生产中的配置和测试中的配置拆分开,是非常必要的在springboot中也提供了配置文件拆分的方式. 这里以生产中项名名称不一致为例:
	
	生产中项目名为: cmfz
	测试中项目名为: springboot
	端口同时为:   8080

拆分如下:
	#主配置文件:
			application.yml	#用来书写相同的的配置
				server:
					port: 8080 #生产和测试为同一个端口
                   
    #生产配置文件:
    		application-pord.yml
    			server:
    				context-path: /cmfz
    #测试配置文件:
    		application-dev.yml
    			server:
    				context-path: /springboot


spring:
  profiles:
    active:  prod # 激活测试配置
```


## 8 springboot中创建自定义简单对象

### 8.1 管理单个对象
```markdown
在springboot中可以管理自定义的简单组件对象的创建可以直
接使用注解形式创建。
# 1.使用 @Repository  @Service @Controller 以及@Component管理
不同简单对象
	如: 比如要通过工厂创建自定义User对象:
```

```java
@Component
@Data
public class User {
  private String id;
  private String name;
  ......
}	
```

```markdown
# 2.通过工厂创建之后可以在使用处任意注入该对象
	如:在控制器中使用自定义简单对象创建
```

```java
@Controller
@RequestMapping("hello")
public class HelloController {
    @Autowired
    private User user;
  	......
}
```

### 8.2 管理多个对象
```markdown
在springboot中如果要管理复杂对象必须使
用@Configuration + @Bean注解进行管理
```

```markdown
# 1.管理复杂对象的创建
```

```java
@Configuration(推荐)|@Component(不推荐)
public class Beans {
    @Bean
    public Calendar getCalendar(){
        return Calendar.getInstance();
    }
}
```

```markdown
# 2.使用复杂对象
```

```java
@Controller
@RequestMapping("hello")
public class HelloController {
    @Autowired
    private Calendar calendar;
    ......
}
```


```markdown
# 注意: 
1.@Configuration 配置注解主要用来生产多个组件交给工厂管理  (注册形式)
2.@Component     用来管理单个组件                      (包扫描形式)
```


```java
package com.yxj.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.Calendar;

//一次性在工厂中批量创建多个对象
@Configuration //工厂配置文件 配置注解
public class BeansConfig {


    //工厂中每一个方法代表一个对象的创建 Calendar
    @Bean //注解 修饰范围用在方法上相当于 spring.xml bean标签作用   用来创建这个对象在工厂一个实例
    public Calendar getCalendar(){
        return Calendar.getInstance();
    }


    @Bean
    @Scope("prototype")
    public Connection getConnection() throws ClassNotFoundException, SQLException {
        Class.forName("com.mysql.jdbc.Driver");
        Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/test","root","root");
        return connection;
    }



}

```


## 9 springboot中注入
```markdown
springboot中提供了三种注入方式: 注入基本属性,对象注入
```

### 9.1 基本属性注入

```markdown
# 1.@Value 属性注入   [重点]
```

```java
@Controller
@RequestMapping("hello")
public class HelloController {
  
    @Value("${name}")
    private String name;
}
```

```markdown
# 2.在配置文件中注入
```

```yml
name: xiaohei
```

### 9.2 对象方式注入

```markdown
# 1. @ConfigurationProperties(prefix="前缀")
```

```java
@Component
@Data
@ConfigurationProperties(prefix = "user")
public class User {
    private String id;
    private String name;
    private Integer age;
    private String  bir;
    .....
}
```

```markdown
# 2. 编写配置文件
```

```yml
user:
  id: 24
  name: xiaohei
  age: 23
  bir: 2012/12/12
```

```markdown
# 3. 引入依赖构建自定义注入元数据(这样对象方式注入就会有提示)
```

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-configuration-processor</artifactId>
  <optional>true</optional>
</dependency>
```


## 10 springboot中两种模板配置

### 10.1 集成jsp模板

#### 10.1.1 引入jsp的集成jar包

```xml
<dependency>
    <groupId>jstl</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
</dependency>

<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-jasper</artifactId>
</dependency>
```

#### 10.1.2 引入jsp运行插件

```xml
<build>
    <finalName>springboot_day1</finalName>
    <!--引入jsp运行插件-->
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

#### 10.1.3 配置视图解析器

```yml
#在配置文件中引入视图解析器
spring:
  mvc:
    view:
      prefix: /   	# /代表访问项目中webapp中页面
      suffix: .jsp 
```

#### 10.1.4 第一种方式使用插件启动
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022231420371.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

#### 10.1.5 第二种方式使用idea中指定工作目录启动  [推荐]
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022231443356.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

#### 10.1.6 启动访问jsp页面

```markdown
http://localhost:8989/cmfz/index.jsp
```

#### 10.1.7 修改jsp无须重启应用（页面热部署）

```properties
server.servlet.jsp.init-parameters.development=true
```

### 10.2 集成thymelaf模板
```markdown
Thymeleaf是一个用于web和独立环境的现代服务器端Java模板引擎。  --摘自官网https://www.thymeleaf.org/

Thymeleaf是跟Velocity、FreeMarker类似的模板引擎，它可
以完全替代JSP，相较与其他的模板引擎相比, Thymeleaf在有网
络和无网络的环境下皆可运行，即它可以让美工在浏览器查看页
面的静态效果，也可以让程序员在服务器查看带数据的动态页面效果。
```

#### 10.2.1 引入依赖

```xml
<!--使用thymelaf-->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

#### 10.2.2 编写配置

```properties
spring.thymeleaf.prefix=classpath:/templates/      #使用模板目录
spring.thymeleaf.suffix=.html                      #使用模板后缀
spring.thymeleaf.encoding=UTF-8                    #使用模板编码
spring.thymeleaf.enabled=true                      #开始thymelaf模板
spring.thymeleaf.servlet.content-type=text/html    #使用模板响应类型
```
#### 10.2.3 编写控制器测试

```java
@Controller    //一定要是@Controller 不能再使用@RestController注解
@RequestMapping("hello")
public class HelloController {
    @GetMapping("hello")
    public String hello(){
        System.out.println("测试与 thymeleaf 的集成");
        return "index";
    }
}
```

#### 10.2.4 在templates目录中定义模板
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022231536683.png#pic_center)


#### 10.2.5 测试访问

```http
http://localhost:8989/springboot_day3/hello/hello
```

#### 10.2.6 查看结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022231607210.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


#### 10.2.7 开启直接访问html页面

```properties
spring.resources.static-locations=classpath:/templates/,classpath:/static/
```

#### 10.2.8 测试结果

```http
http://localhost:8989/springboot_day3/index.html
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022231638393.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


### 10.3 Thymeleaf基本使用
```markdown
使用时必须在页面中加入thymeleaf如下命名空间:
```
```html
<html lang="en" xmlns:th="http://www.thymeleaf.org">
```

#### 10.3.1 展示单个数据
```java
a. 设置数据
model.addAttribute("name","张三"); 或 request.setAttribute("name","小黑");
```

```html
b. 获取数据
<span th:text="${name}"/>  --->获取数据
```

```markdown
注意:在springboot2.x版本中必须加入国际化配置文件才能正常展示:
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102223171423.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022231734457.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```java
c. 获取并解析含有html标签数据
model.addAttribute("name","<a href=''>张三</a>");
model.addAttribute("username","小陈");
```

```html
直接获取原样输出
<span th:text="${name}"/>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102223184317.png#pic_center)

```html
获取并解析

 <span th:utext="${name}"/>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022231904972.png#pic_center)


```html
将数据赋值给表单元素
<input type="text" th:value="${username}"/>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102223192742.png#pic_center)


```markdown
# 总结
1.使用 th:text="${属性名}"  获取对应数据,获取数据时会将对应标
签中数据清空,因此最好是空标签
2.使用 th:utext="${属性名}" 获取对应的数据,可以将数据中html先
解析在渲染到页面
3.使用 th:value="${属性名}" 获取数据直接作为表单元素value属性
```

#### 10.3.2 展示对象数据

```java
model.addAttribute("user",new User("21","xiaochen",23,new Date()));
```

```html
id:<span th:text="${user.id}"></span>
name:<span th:text="${user.name}"></span>
age:<span th:text="${user.age}"></span>
bir: <span th:text="${user.bir}"></span>  ====  <span th:text="${#dates.format(user.bir, 'yyyy-MM-dd HH:mm')}"></span> 日期格式化
```

#### 10.3.3 条件展示数据

```java
model.addAttribute("user",new User("21","xiaochen",23,new Date()));
```

```html
<span th:if="${user.age} eq 23">
  青年
</span>
```

```markdown
# 运算符
    gt：great than（大于）>
    ge：great equal（大于等于）>=
    eq：equal（等于）==
    lt：less than（小于）<
    le：less equal（小于等于）<=
    ne：not equal（不等于）!=
```

#### 10.3.4 展示多条数据
```html
直接遍历集合
 <ul th:each="user:${users}">
   <li th:text="${user.id}"></li>
   <li th:text="${user.name}"></li>
   <li th:text="${user.age}"></li>
   <li th:text="${#dates.format(user.bir,'yyyy-MM-dd')}"></li>
</ul>
```
```html
遍历时获取遍历状态
 <ul th:each="user,userStat:${users}">
   <li><span th:text="${userStat.count}"/>-<span th:text="${user.id}"/></li>   获取遍历次数  count 从1开始 index 从0开始
   <li><span th:text="${userStat.odd}"/>-<span th:text="${user.name}"/></li>   获取当前遍历是否是奇数行
   <li><span th:text="${userStat.even}"/>-<span th:text="${user.age}"/></li>   获取当前遍历是否是偶数行
   <li><span th:text="${userStat.size}"/>-<span th:text="${user.bir}"/></li>   获取当前集合的总条数
</ul>
```

#### 10.3.5 引入静态资源
```markdown
使用thymeleaf模板项目中静态资源默认放在resources路径
小static目录中
```
```markdown
项目中放入对应静态资源
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022232121457.png#pic_center)

```markdown
页面中引入
```

```html
<link rel="stylesheet" th:href="@{/css/index.css}">
<script th:src="@{/js/jquery-min.js}"></script>
```

  

## 11 springboot集成mybatis

### 11.1 引入依赖

```xml
<!--整合mybatis-->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.3</version>
</dependency>

<dependency>
	<groupId>com.alibaba</groupId>
	<artifactId>druid</artifactId>
	<version>1.1.12</version>
</dependency>

<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.38</version>
</dependency>

>说明:由于springboot整合mybatis版本中默认依赖mybatis 因此不需要额外引入mybati版本,否则会出现冲突
```

### 11.2 配置配置文件

```yml
spring:
  mvc:
    view:
      prefix: /
      suffix: .jsp
  datasource:
    type: org.apache.commons.dbcp.BasicDataSource   #指定连接池类型
    driver-class-name: com.mysql.jdbc.Driver        #指定驱动
    url: jdbc:mysql://localhost:3306/cmfz           #指定url
    username: root									#指定用户名
    password: root								 	#指定密码
```

### 11.3 加入mybatis配置

```yml
#配置文件中加入如下配置:

mybatis:
  mapper-locations: classpath:com/yxj/mapper/*.xml  #指定mapper配置文件位置
  type-aliases-package: com.yxj.entity              #指定起别名来的类
```

```java
//入口类中加入如下配置:
@SpringBootApplication
@MapperScan("com.yxj.dao")   //必须在入口类中加入这个配置
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class,args);
    }
}
```



### 11.4 建表

```sql
CREATE TABLE `t_clazz` (
  `id` varchar(40) NOT NULL,
  `name` varchar(80) DEFAULT NULL,
  `no` varchar(90) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

### 11.5 开发实体类

```java
public class Clazz {
    private String id;
    private String name;
    private String no;
    //get set 方法省略....
}
```

### 11.6 开发DAO接口以及Mapper

```java
public interface ClazzDAO {
    List<Clazz> findAll();
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.yxj.dao.ClazzDAO">
    <select id="findAll" resultType="Clazz">
        select * from t_clazz 
    </select>
</mapper>
```

### 11.7 开发Service以及实现

```java
//接口
public interface ClazzService {
    List<Clazz> findAll();
}
//实现
@Service
@Transactional
public class ClazzServiceImpl implements  ClazzService {
    @Autowired
    private ClazzDAO clazzDAO;
    
    @Transactional(propagation = Propagation.SUPPORTS)
    @Override
    public List<Clazz> findAll() {
        return clazzDAO.findAll();
    }
}
```

### 11.8 引入测试依赖

```xml
 <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
     <scope>test</scope>
</dependency>
```

### 11.9 编写测试类

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = Application.class)
public class TestClazzService {

    @Autowired
    private ClazzService clazzService;

    @Test
    public void test(){
        List<Clazz> all = clazzService.findAll();
        for (Clazz clazz : all) {
            System.out.println(clazz);
        }

    }
}
```

## 12 开启jsp页面热部署

### 12.1 引言
```markdown
在springboot中默认对jsp运行为生产模式,不允许修改内容保存后
立即生效,因此在开发过程需要调试jsp页面每次需要重新启动服
务器这样极大影响了我们的效率,为此springboot中提供了可以
将默认的生产模式修改为调试模式,改为调试模式后就可以保
存立即生效,如何配置为测试模式需要在配置文件中加入如下
配置即可修改为开发模式。
```

### 12.2 配置开启测试模式

```yml
server:
  port: 8989
  jsp-servlet:
    init-parameters:
      development: true  #开启jsp页面的调试模式
```

## 13 springboot中devtools热部署
### 13.1  引言
```markdown
为了进一步提高开发效率,springboot为我们提供了全局项目热部
署,日后在开发过程中修改了部分代码以及相关配置文件后,不
需要每次重启使修改生效,在项目中开启了springboot全局热部署
之后只需要在修改之后等待几秒即可使修改生效。
```
### 13.2 开启热部署
#### 13.2.1 项目中引入依赖

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-devtools</artifactId>
   <optional>true</optional>
</dependency>
```

#### 13.2.2 设置idea中支持自动编译

```markdown
# 1.开启自动编译

	Preferences | Build, Execution, Deployment | Compiler -> 勾选上 Build project automatically 这个选项

# 2.开启允许在运行过程中修改文件
	ctrl + alt + shift + / ---->选择1.Registry ---> 勾选 compiler.automake.allow.when.app.running 这个选项
```

#### 13.2.3 启动项目检测热部署是否生效

```markdown
# 1.启动出现如下日志代表生效
```

```verilog
2019-07-17 21:23:17.566  INFO 4496 --- [  restartedMain] com.yxj.InitApplication               : Starting InitApplication on chenyannandeMacBook-Pro.local with PID 4496 (/Users/chenyannan/IdeaProjects/ideacode/springboot_day1/target/classes started by chenyannan in /Users/chenyannan/IdeaProjects/ideacode/springboot_day1)
2019-07-17 21:23:17.567  INFO 4496 --- [  restartedMain] com.yxj.InitApplication               : The following profiles are active: dev
2019-07-17 21:23:17.612  INFO 4496 --- [  restartedMain] ationConfigEmbeddedWebApplicationContext : Refreshing org.springframework.boot.context.embedded.AnnotationConfigEmbeddedWebApplicationContext@66d799c5: startup date [Wed Jul 17 21:23:17 CST 2019]; root of context hierarchy
2019-07-17 21:23:18.782  INFO 4496 --- [  restartedMain] s.b.c.e.t.TomcatEmbeddedServletContainer : Tomcat initialized with port(s): 8989 (http)
2019-07-17 21:23:18.796  INFO 4496 --- [  restartedMain] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2019-07-17 21:23:18.797  INFO 4496 --- [  restartedMain] org.apache.catalina.core.StandardEngine  : Starting Servlet Engine: Apache Tomcat/8.5.20
```
```markdown
注意:日志出现restartedMain代表已经生效,在使用热部署时如果
遇到修改之后不能生效,请重试重启项目在试
```

##  14 logback日志的集成
### 	14.1 logback简介
```markdown
Logback是由[log4j](https://baike.baidu.com/item/log4j/480673)创始人设计的又一个开源日志组件。目前，logback分为三个模块：logback-core，logback-classic和logback-access。是对log4j日志展示进一步改进
```
### 	14.2 日志的级别
```markdown
DEBUG < INFO < WARN < ERROR < OFF

日志级别由低到高:  日志级别越高输出的日志信息越少
```
### 14.3 项目中日志分类
```markdown
日志分为两类

一种是rootLogger :     用来监听项目中所有的运行日志 包括引入依赖jar中的日志 

一种是logger :         用来监听项目中指定包中的日志信息
```
### 	14.4 java项目中使用
#### 14.4.1 logback配置文件
```xml
logback的配置文件必须放在项目根目录中 且名字必须为logback.xml

<?xml version="1.0" encoding="UTF-8" ?>
<configuration>
    <!--定义项目中日志输出位置-->
    <appender name="stdout" class="ch.qos.logback.core.ConsoleAppender">
        <!--定义项目的日志输出格式-->
        <!--定义项目的日志输出格式-->
        <layout class="ch.qos.logback.classic.PatternLayout">
            <pattern> [%p] %d{yyyy-MM-dd HH:mm:ss} %m %n</pattern>
        </layout>
    </appender>

    <!--项目中跟日志控制-->
    <root level="INFO">
        <appender-ref ref="stdout"/>
    </root>
    <!--项目中指定包日志控制-->
    <logger name="com.yxj.dao" level="DEBUG"/>

</configuration>
```

#### 14.4.2 具体类中使用日志

```java
@Controller
@RequestMapping("/hello")
public class HelloController {
    //声明日志成员
    private static org.apache.log4j.Logger logger = org.apache.log4j.Logger.getLogger(HelloController.class);
    @RequestMapping("/hello")
    @ResponseBody
    public String hello(){
        System.out.println("======hello world=======");
        logger.debug("DEBUG");
        logger.info("INFO");
        logger.warn("WARN");
        logger.error("ERROR");
        return "hello";
    }
}
```

#### 14.4.3 使用默认日志配置

```yml
logging:
  level:
    root: debug
    com.yxj.dao: debug
  path: /Users/chenyannan/aa.log
  file: bbb.log
```


## 15 切面编程
### 	15.1 引言
```markdown
springboot是对原有项目中spring框架和springmvc的进一步
封装,因此在springboot中同样支持spring框架中AOP切面编
程,不过在springboot中为了快速开发仅仅提供了注解方式的
切面编程.
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022232207118.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

### 	15.2 使用
#### 15.2.1 引入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

#### 15.2.2 相关注解

```java
/**
    @Aspect 用来类上,代表这个类是一个切面
    @Before 用在方法上代表这个方法是一个前置通知方法 
    @After 用在方法上代表这个方法是一个后置通知方法 @Around 用在方法上代表这个方法是一个环绕的方法
    @Around 用在方法上代表这个方法是一个环绕的方法
**/
```

#### 15.2.3 前置切面

```java
@Aspect
@Component
public class MyAspect {
    @Before("execution(* com.yxj.service.*.*(..))")
    public void before(JoinPoint joinPoint){
        System.out.println("前置通知");
        joinPoint.getTarget();//目标对象
        joinPoint.getSignature();//方法签名
        joinPoint.getArgs();//方法参数
    }
}
```

#### 15.2.4 后置切面

```java
@Aspect
@Component
public class MyAspect {
    @After("execution(* com.yxj.service.*.*(..))")
    public void before(JoinPoint joinPoint){
        System.out.println("后置通知");
        joinPoint.getTarget();//目标对象
        joinPoint.getSignature();//方法签名
        joinPoint.getArgs();//方法参数
    }
}
```
```markdown
注意: 前置通知和后置通知都没有返回值,方法参数都为joinpoint
```

#### 15.2.5 环绕切面
```java
@Aspect
@Component
public class MyAspect {
    @Around("execution(* com.yxj.service.*.*(..))")
    public Object before(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        System.out.println("进入环绕通知");
        proceedingJoinPoint.getTarget();//目标对象
        proceedingJoinPoint.getSignature();//方法签名
        proceedingJoinPoint.getArgs();//方法参数
        Object proceed = proceedingJoinPoint.proceed();//放行执行目标方法
        System.out.println("目标方法执行之后回到环绕通知");
        return proceed;//返回目标方法返回值
    }
}
```
```markdown
注意: 环绕通知存在返回值,参数为ProceedingJoinPoint,如果执行
放行,不会执行目标方法,一旦放行必须将目标方法的返回值返回,否
则调用者无法接受返回数据
```


## 16 文件上传下载
### 16.1 文件上传
```markdown
定义:用户访问当前系统,将自己本地计算机中文件通过浏览器上传
到当前系统所在的服务器过程中称之为文件的上传
```
#### 16.1.1 准备上传页面

```html
<form action="路径...." method="post" enctype="multipart/form-data">
        <input type="file" name="aaa">
        <input type="submit" value="上传">
</form>
<!--
	1. 表单提交方式必须是post
	2. 表单的enctype属性必须为multipart/form-data
	3. 后台接受变量名字要与文件选择name属性一致
-->
```
#### 16.1.2 编写控制器

```java
@Controller
@RequestMapping("/file")
public class FileController {
 //处理文件上传操作
    @RequestMapping("upload")
    public String  upload(MultipartFile aaa, HttpServletRequest request) throws IOException {
        System.out.println("文件名: "+aaa.getOriginalFilename());

        System.out.println("文件类型: "+aaa.getContentType());

        System.out.println("文件大小: "+aaa.getSize());
        //根据相对获取绝对路径
        // String realPath = ResourceUtils.getURL("classpath:").getPath()+"static/files";
        String realPath = request.getSession().getServletContext().getRealPath("/files");
        //创建时间文件夹
        String format = new SimpleDateFormat("yyyy-MM-dd").format(new Date());
        File file = new File(realPath, format);
        if(!file.exists()) file.mkdirs();
        //获取文件后缀
        String extension = FilenameUtils.getExtension(aaa.getOriginalFilename());

        //根据文件后缀动态获取文件类型

        String mimeType = request.getSession().getServletContext().getMimeType("."+extension);
        System.out.println("动态根据后缀获取文件类型: "+mimeType);

        String newFileNamePrefix = UUID.randomUUID().toString().replace("-","")+
                  new SimpleDateFormat("yyyyMMddHHmmssSSS").format(new Date());
        String newFileName = newFileNamePrefix+"."+extension;
        //处理上传操作
        aaa.transferTo(new File(file,newFileName));
        return "redirect:/upload.jsp";
    }
}


```
#### 16.1.3 修改文件上传大小

```yml
#上传时出现如下异常:  上传文件的大小超出默认配置  默认10M
nested exception is java.lang.IllegalStateException: org.apache.tomcat.util.http.fileupload.FileUploadBase$SizeLimitExceededException: the request was rejected because its size (38443713) exceeds the configured maximum (10485760)
#修改上传文件大小:
spring:
  http:
    multipart:
       max-request-size: 209715200  #用来控制文件上传大小的限制
       max-file-size: 209715200 #用来指定服务端最大文件大小   

新版本
spring.servlet.multipart.max-file-size=500MB
spring.servlet.multipart.max-request-size=500MB
```

### 	16.2 文件下载
#### 16.2.1 提供下载文件链接

```html
<a href="../file/down?fileName=corejava.txt">corejava.txt</a>
```
#### 16.2.2 开发控制器
```java
 //处理文件下载操作
    @RequestMapping("down")
    public void download(String fileName, HttpServletRequest request, HttpServletResponse response) throws IOException {
        System.out.println(user);
        //根据相当路径获取绝对路径
        //ServletContext servletContext = request.getSession().getServletContext();
        String realPath = request.getServletContext().getRealPath("/files/download");//2.5规范中不能这么使用
        //获取文件名,根据文件名去指定的目录读取文件
        FileInputStream is = new FileInputStream(new File(realPath, fileName));
        //设置下载时响应头
        //attachment; 附件下载   inline 在线打开(图片 pdf )
        response.setHeader("content-disposition","inline;fileName="+ URLEncoder.encode(fileName,"UTF-8"));
        //通过响应流响应即可
        ServletOutputStream outputStream = response.getOutputStream();
        //流的复制
        IOUtils.copy(is,outputStream);
        IOUtils.closeQuietly(is);
        IOUtils.closeQuietly(outputStream);

    }
```

## 17 拦截器
### 	17.1 开发拦截器

```java
public class MyInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object o) throws Exception {
        System.out.println("======1=====");
        return true;//返回true 放行  返回false阻止
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object o, ModelAndView modelAndView) throws Exception {
        System.out.println("=====2=====");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object o, Exception e) throws Exception {
        System.out.println("=====3=====");
    }
}
```
### 	17.2 配置拦截器

```java
//1.x中拦截器配置
@Component
public class InterceptorConfig extends WebMvcConfigurerAdapter {

	@Autowired
    private MyInterceptor myInterceptor;
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //添加拦截器
        registry.addInterceptor(myInterceptor)
            .addPathPatterns("/**")//定义拦截路径
            .excludePathPatterns("/hello/**"); //排除拦截路径
    }
}

//2.x中拦截器配置
	@Configuration
public class InterceptorConfig extends WebMvcConfigurationSupport {
	@Autowired
    private MyInterceptor myInterceptor;
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(myInterceptor)//添加拦截器
                .addPathPatterns("/hello/**")//添加拦截的请求路径
                .excludePathPatterns("/hello/world");//添加排除那些请求路径不经过拦截器
    }
}
```
```markdown
注意:在springboot2.x版本中自定义拦截器之后出现项目中静态
资源 404情况,需要在自定义拦截器的配置中加入如下配置即可:
```
```java
@Configuration
public class InterceptorConfig extends WebMvcConfigurationSupport {

    @Override
    protected void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginInterceptor())
                .addPathPatterns("/file/**")
                .excludePathPatterns("/js/**", "/css/**", "/files/**");//放行静态资源 静态资源被认为是一个控制器请求
    }
		//springboot2.x 静态资源在自定义拦截器之后无法访问的解决方案
    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/**") //代表以什么样的请求路径访问静态资源
                .addResourceLocations("classpath:/static/")
                .addResourceLocations("classpath:/templates/");

    }
}
```

## 18 war包部署
### 	18.1 设置打包方式为war
```markdown
<packaging>war</packaging>
```
#### 18.2 在插件中指定入口类
```xml
<build>
	<plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
        <!--使用热部署出现中文乱码解决方案-->
        <configuration>
          <fork>true</fork>
          <!--增加jvm参数-->
          <jvmArguments>-Dfile.encoding=UTF-8</jvmArguments>
          <!--指定入口类-->
          <mainClass>com.yxj.Application</mainClass>
        </configuration>
      </plugin>
    </plugins>
</build>	
```

#### 18.3 排除内嵌的tomcat

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-tomcat</artifactId>
  <scope>provided</scope>   <!--去掉内嵌tomcat-->
</dependency>

<dependency>
  <groupId>org.apache.tomcat.embed</groupId>
  <artifactId>tomcat-embed-jasper</artifactId>
  <scope>provided</scope>  <!--去掉使用内嵌tomcat解析jsp-->
</dependency>
```

#### 18.4 配置入口类

```java
//1.继承SpringBootServletInitializer
//2.覆盖configure方法
public class Application extends SpringBootServletInitializer{
    public static void main(String[] args) {
        SpringApplication.run(Application.class,args);
    }
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        return builder.sources(Application.class);
    }
}
```

#### 18.5 打包测试

```java
/* 一旦使用war包部署注意:
	1. application.yml 中配置port context-path 失效
	2. 访问时使用打成war包的名字和外部tomcat端口号进行访问项目
*/
```

## 19 注解
```java
springboot 的应用默认没有项目名
@RestController //将类中所有控制器方法返回值转为json格式并响应前端浏览器


@Configuration //一次性在工厂中批量创建多个对象,工厂配置文件 配置注解
 @Bean //注解 修饰范围用在方法上相当于 spring.xml bean标签作用   用来创建这个对象在工厂一个实例
```
## 20 常用配置
### 20.1 yml
```yml
server:
  port: 8989
  context-path: /springboot_day2   # 配置应用名
  jsp-servlet:
    init-parameters:
      development: true
spring:
  mvc:
    view:
      prefix: /
      suffix: .jsp
  http:
    multipart:
      max-request-size: 2MB  #控制请求可以上传文件大小  默认值10M  单位MB
      max-file-size:  4MB  #用来控制实际文件上传大小限制  默认值 1M 单位MB

spring:
  profiles:
    active:  prod # 激活测试配置
  mvc:
    view:
      prefix:  /  # 配置前缀
      suffix: .jsp #配置后缀

```
### 20.2 properties
```java
#修改上传文件大小:
spring.servlet.multipart.max-file-size=500MB
spring.servlet.multipart.max-request-size=500MB
```

## 21 常用依赖
```xml
<dependencies>

      <!--引入web支持-->
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
      </dependency>
  
  
      <!--引入jsp支持-->
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-tomcat</artifactId>
      </dependency>
  
      <dependency>
        <groupId>org.apache.tomcat.embed</groupId>
        <artifactId>tomcat-embed-jasper</artifactId>
      </dependency>
  
      <dependency>
        <groupId>jstl</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
      </dependency>
  
      <!--引入commons-fileUpload-->
      <dependency>
        <groupId>commons-fileupload</groupId>
        <artifactId>commons-fileupload</artifactId>
        <version>1.3.2</version>
      </dependency>
  
  


  </dependencies>

  <build>

    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
        <!--使用热部署出现中文乱码解决方案-->
        <configuration>
          <fork>true</fork>
          <!--增加jvm参数-->
          <jvmArguments>-Dfile.encoding=UTF-8</jvmArguments>
          <!--指定入口类-->
          <mainClass>com.yxj.Application</mainClass>
        </configuration>
      </plugin>
    </plugins>

  </build>
```
## 22 图片

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201005081023208.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201005081041682.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201005090439682.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201005090654915.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201005094255842.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201005095215658.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201005100205725.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201005100243537.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201005102348534.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201005103958244.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201005105711519.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201005132244494.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201005133245914.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201005133338636.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201005145050975.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201005152321525.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201005152623754.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201005152704906.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201005181548773.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201005181954595.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/202010051822113.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201005192643413.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复
boot即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)










































































