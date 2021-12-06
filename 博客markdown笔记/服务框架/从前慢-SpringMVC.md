# SpringMVC
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210717132552143.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1 SpringMVC的引言
```markdown
为了使Spring可插入的MVC架构,SpringFrameWork在Spring基
础上开发SpringMVC框架,从而在使用Spring进行WEB开发时
可以选择使用Spring的SpringMVC框架作为web开发的控制器框架。
```

## 2 为什么是SpringMVC
```markdown
可以和spring框架无缝整合
运行效率高于struts2框架
注解式开发更高效
```

## 3 SpringMVC的特点
```markdown
SpringMVC轻量级，典型MVC框架，在整个MVC架构中充
当控制器框架,相对于之前学习的struts2框架,SpringMVC运
行更快,其注解式开发更高效灵活。
```

## 4 SpringMVC与Struts2运行流程对比
![在这里插入图片描述](https://img-blog.csdnimg.cn/202010221828265.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


## 5 第一个环境搭建

###  5.1 引入相关依赖

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
<!--springmvc核心依赖-->
<dependency>
	 <groupId>org.springframework</groupId>
	 <artifactId>spring-webmvc</artifactId>
	 <version>4.3.2.RELEASE</version>
</dependency>
<!--servlet-api-->
<dependency>
	 <groupId>javax.servlet</groupId>
	 <artifactId>servlet-api</artifactId>
	 <version>2.5</version>
	 <scope>provided</scope>
</dependency>
```

### 5.2 编写springmvc配置文件

```xml
<!--开启注解扫描-->
 <context:component-scan base-package="com"/>

 <!--注册处理器映射器-->
 <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>
 <!--注册处理器适配器-->
 <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"/>
 <!--注册视图解析器-->
 <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
     <property name="prefix" value="/"/>
     <property name="suffix" value=".jsp"/>
 </bean>
```

### 5.3 配置springmvc的核心Servlet

```xml
<servlet>
	<servlet-name>springmvc</servlet-name>
	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
	<!--指定springmvc配置文件位置-->
	<init-param>
	<param-name>contextConfigLocation</param-name>
	<param-value>classpath:springmvc.xml</param-value>
	</init-param>
</servlet>

<servlet-mapping>
	<servlet-name>springmvc</servlet-name>
	<url-pattern>/</url-pattern>
</servlet-mapping>
```
```markdown
注意: 这里还要加载spring配置,通过在servlet写init­param标
签,还是contextConfigLocation属性,value用来加载springmvc
配置文件
```


### 5.4 创建控制器

```java
@Controller
@RequestMapping("/hello")
public class HelloController {
    @RequestMapping("/hello")
    public String hello(){
        System.out.println("hello springmvc");
        return "index";//解析结果:前缀+返回值+后缀
    }
}
```
```java
@Controller: 该注解用来类上标识这是一个控制器组件类并创建这个类实例

@RequestMapping: 

修饰范围: 用在方法或者类上
注解作用: 用来指定类以及类中方法的请求路径
请求 URL 的第一级访问目录。此处不写的话，就相当于应用的根目录。写的话需要以/开头。
它出现的目的是为了使我们的 URL 可以按照模块化管理:
例如：
账户模块：
/account/add
/account/update
/account/delete
...
订单模块：
/order/add
/order/update
/order/delete
红色的部分就是把RequsetMappding写在类上，使我们的URL更加精细。
方法上：
请求 URL 的第二级访问目录
属性：
value：用于指定请求的 URL。它和 path 属性的作用是一样的。
method：用于指定请求的方式。
例如：@RequestMapping(value="/saveAccount",method=RequestMethod.POST)
params：用于指定限制请求参数的条件。它支持简单的表达式。要求请求参数的 key 和 value 必须和
配置的一模一样。
例如：
params = {"accountName"}，表示请求参数必须有 accountName
params = {"moeny!100"}，表示请求参数中 money 不能是 100。

例如：@RequestMapping(value="/removeAccount",params= {"accountName","money>100"})
headers：用于指定限制请求消息头的条件。
注意：
以上四个属性只要出现 2 个或以上时，他们的关系是与的关系。
```
### 5.5 部署项目,启动项目测试

```http
访问路径: http://localhost:8989/springmvc_day1/hello/hello
```


### 5.6 Springmvc说明
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200609164528833.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200609165801216.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/202006091700427.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

## 6 SpringMVC中参数接收
```markdown
接收参数语法说明:springmvc中使用控制器方法参数来收
集客户端的请求参数,因此在接收请求参数时直接在需要控
制器方法声明即可,springmvc可以自动根据指定类型完
成类型的转换操作
```


### 6.1 基本类型或者 String 类型
```markdown
如: 八种基本类型 + String + 日期类型
要求我们的参数名称必须和控制器中方法的形参名称保持一致。(严格区分大小写)
注意:springmvc 默认日期格式  spring日期格式   yyyy/MM/dd HH:mm:ss 
struts2日期格式:yyyy-MM-dd
修改默认日期格式: @DateTimeFormat 必须使用mvc标签配置
注意:在springmvc中出现400错误说明传递参数的格式存在问题 
```
#### 6.1.1 代码示例
```java
jsp 代码：
<a href="account/findAccount?accountId=10&accountName=zhangsan">查询账户</a>


控制器代码：
@RequestMapping("/findAccount")
public String findAccount(Integer accountId,String accountName) {
	System.out.println("查询了账户。。。。"+accountId+","+accountName);
	return "success";
}
```


### 6.2 接收对象类型参数
```markdown
包括实体类，以及关联的实体类
如果是 POJO 类型，或者它的关联对象：
要求表单中参数名称和 POJO 类的属性名称保持一致。
并且控制器方法的参数类型是 POJO 类型。
```
#### 6.2.1 代码示例
```java
实体类代码：

public class Account implements Serializable {
private Integer id;
private String name;
private Float money;
private Address address;
//getters and setters
}

public class Address implements Serializable {
private String provinceName;
private String cityName;
//getters and setters
}

jsp 代码：
<!-- pojo 类型演示 -->
<form action="account/saveAccount" method="post">
	账户名称：<input type="text" name="name" ><br/>
	账户金额：<input type="text" name="money" ><br/>
	账户省份：<input type="text" name="address.provinceName" ><br/>
	账户城市：<input type="text" name="address.cityName" ><br/>
	<input type="submit" value="保存">
</form>

控制器代码：

@RequestMapping("/saveAccount")
public String saveAccount(Account account) {
System.out.println("保存了账户。。。。"+account);
return "success";
}
```




### 6.3 接收数组类型参数
```markdown
/**
* 接收数组类型参数
* 语法: 将要接收数组作为控制器方法的参数声明即可
* 前台语法: 要求前台传递的多个参数的参数变量名都要
* 与接受数组变量名一致,
*          springmvc自动将多个变量名放入一个数组中
* url?names=zhangsan&names=李四...
*/
```
#### 6.3.1 代码示例
```java
# GET 方式请求参数传递
http://localhost:8080/springmvc_day1/param/test2?names=zhangsan&names=lisi&names=wangwu

# POST 方式请求参数传递
<h1>测试对象类型参数接收</h1>
<form action="${pageContext.request.contextPath}/param/test2" method="post">
  	爱好: <br>
  	看书:  <input type="checkbox" name="names"/> 
  	看电视:<input type="checkbox" name="names"/>
  	吃饭:  <input type="checkbox" name="names"/>
  	玩游戏: <input type="checkbox" name="names"/>
    <input type="submit" value="提交"/>
</form>

@RequestMapping("/test2")
public String test2(String[] names){
  for (String name : names) {
    System.out.println(name);
  }
  return "index";
}
```
```markdown
注意:接收数组类型数据时前台传递多个key一致
自动放入同一个数组中
```

### 6.4 接收集合类型参数
```java
包括 List 结构和 Map 结构的集合（包括数组）
如果是集合类型,有两种方式：
第一种：
要求集合类型的请求参数必须在 POJO 中。在表单中请
求参数名称要和 POJO 中集合属性名称相同。
给 List 集合中的元素赋值，使用下标。
给 Map 集合中的元素赋值，使用键值对。
第二种：
接收的请求参数是 json 格式数据。需要借助一个注解实现
注意:
它还可以实现一些数据类型自动转换。内置转换器全都在：
org.springframework.core.convert.support 包下。有：
java.lang.Boolean -> java.lang.String : ObjectToStringConverter
java.lang.Character -> java.lang.Number : CharacterToNumberFactory
java.lang.Character -> java.lang.String : ObjectToStringConverter
java.lang.Enum -> java.lang.String : EnumToStringConverter
java.lang.Number -> java.lang.Character : NumberToCharacterConverter
java.lang.Number -> java.lang.Number : NumberToNumberConverterFactory
java.lang.Number -> java.lang.String : ObjectToStringConverter
java.lang.String -> java.lang.Boolean : StringToBooleanConverter
java.lang.String -> java.lang.Character : StringToCharacterConverter
java.lang.String -> java.lang.Enum : StringToEnumConverterFactory
java.lang.String -> java.lang.Number : StringToNumberConverterFactory
java.lang.String -> java.util.Locale : StringToLocaleConverter
java.lang.String -> java.util.Properties : StringToPropertiesConverter
java.lang.String -> java.util.UUID : StringToUUIDConverter
java.util.Locale -> java.lang.String : ObjectToStringConverter
java.util.Properties -> java.lang.String : PropertiesToStringConverter
java.util.UUID -> java.lang.String : ObjectToStringConverter
......
如遇特殊类型转换要求，需要我们自己编写自定义类型转换器。
```

#### 6.4.1 代码示例

```java
实体类代码：
public class User implements Serializable {
private String username;
private String password;
private Integer age;
private List<Account> accounts;
private Map<String,Account> accountMap;
//getters and setters
@Override
public String toString() {
return "User [username=" + username + ", password=" + password + ", age="
+ age + ",\n accounts=" + accounts
+ ",\n accountMap=" + accountMap + "]";
}
}
jsp 代码：
<!-- POJO 类包含集合类型演示 -->
<form action="account/updateAccount" method="post">
用户名称：<input type="text" name="username" ><br/>
用户密码：<input type="password" name="password" ><br/>
用户年龄：<input type="text" name="age" ><br/>
账户 1 名称：<input type="text" name="accounts[0].name" ><br/>
账户 1 金额：<input type="text" name="accounts[0].money" ><br/>
账户 2 名称：<input type="text" name="accounts[1].name" ><br/>
账户 2 金额：<input type="text" name="accounts[1].money" ><br/>
账户 3 名称：<input type="text" name="accountMap['one'].name" ><br/>
账户 3 金额：<input type="text" name="accountMap['one'].money" ><br/>
账户 4 名称：<input type="text" name="accountMap['two'].name" ><br/>
账户 4 金额：<input type="text" name="accountMap['two'].money" ><br/>
<input type="submit" value="保存">
</form>

控制器代码：
/**
* 更新账户
* @return
*/
@RequestMapping("/updateAccount")
public String updateAccount(User user) {
System.out.println("更新了账户。。。。"+user);
return "success";
}
```

### 6.5 自定义类型转换器

```java
第一步：定义一个类，实现 Converter 接口，该接口有两个泛型。
public interface Converter<S, T> {//S:表示接受的类型，T：表示目标类型
/**
* 实现类型转换的方法
*/
@Nullable
T convert(S source);
}
/**
* 自定义类型转换器
*/
public class StringToDateConverter implements Converter<String, Date> {
	/**
	* 用于把 String 类型转成日期类型
	*/
	@Override
	public Date convert(String source) {
		DateFormat format = null;
		try {
			if(StringUtils.isEmpty(source)) {
			throw new NullPointerException("请输入要转换的日期");
			}
			format = new SimpleDateFormat("yyyy-MM-dd");
			Date date = format.parse(source);
			return date;
		} catch (Exception e) {
			throw new RuntimeException("输入日期有误");
		}
	}
}
第二步：在 spring 配置文件中配置类型转换器。
spring 配置类型转换器的机制是，将自定义的转换器注册到类型转换服务中去。
<!-- 配置类型转换器工厂 -->
<bean id="converterService" class="org.springframework.context.support.ConversionServiceFactoryBean">
	<!-- 给工厂注入一个新的类型转换器 -->
	<property name="converters">
		<array>
		<!-- 配置自定义类型转换器 -->
			<bean class="com.yxj.web.converter.StringToDateConverter"></bean>
		</array>
	</property>
</bean>
第三步：在 annotation-driven 标签中引用配置的类型转换服务
<!-- 引用自定义类型转换器 -->
<mvc:annotation-driven	conversion-service="converterService"></mvc:annotation-driven>


补充：
在 SpringMVC 的各个组件中，处理器映射器、处理器适配器、视图解析器称为SpringMVC的三大组件。
使 用 <mvc:annotation-driven> 自动加载 RequestMappingHandlerMapping （处理映射器） 和
RequestMappingHandlerAdapter （处理适配器） ， 可 用 在 SpringMVC.xml 配 置 文 件 中 使 用
<mvc:annotation-driven>替代注解处理器和适配器的配置。
它就相当于在 xml 中配置了：
<!-- 上面的标签相当于 如下配置-->
<!-- Begin -->
<!-- HandlerMapping -->
<bean
class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerM
apping"></bean>
<bean
class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"></bean>
<!-- HandlerAdapter -->
<bean
class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerA
dapter"></bean>
<bean
class="org.springframework.web.servlet.mvc.HttpRequestHandlerAdapter"></bean>
<bean
class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"></bean>
<!-- HadnlerExceptionResolvers -->
<bean
class="org.springframework.web.servlet.mvc.method.annotation.ExceptionHandlerExcept
ionResolver"></bean>
<bean
class="org.springframework.web.servlet.mvc.annotation.ResponseStatusExceptionResolv
er"></bean>
<bean
class="org.springframework.web.servlet.mvc.support.DefaultHandlerExceptionResolver"
></bean>
<!-- End -->
注意：
一般开发中，我们都需要写上此标签（虽然从入门案例中看，我们不写也行，随着课程的深入，该标签还
有具体的使用场景）。
明确：
我们只需要编写处理具体业务的控制器以及视图。
```
### 6.6 使用 ServletAPI 对象作为方法参数

```java
SpringMVC 还支持使用原始 ServletAPI 对象作为控制器方法的参数。支持原始 ServletAPI 对象有：
HttpServletRequest
HttpServletResponse
HttpSession
java.security.Principal
Locale
InputStream
OutputStream
Reader
Writer
我们可以把上述对象，直接写在控制的方法参数中使用。
部分示例代码：
jsp 代码：
<!-- 原始 ServletAPI 作为控制器参数 -->
<a href="account/testServletAPI">测试访问 ServletAPI</a>
控制器中的代码：
/**
* 测试访问 testServletAPI
* @return
*/
@RequestMapping("/testServletAPI")
public String testServletAPI(HttpServletRequest request,HttpServletResponse response,HttpSession session) {
	System.out.println(request);
	System.out.println(response);
	System.out.println(session);
	return "success";
}

```
## 7 接收参数中文乱码解决方案
```markdown
注意:在使用springmvc过程中接收客户端的请求参数时有
时会出现中文乱码,这是因为springmvc并没有对象请求参数进
行编码控制,如果需要控制需要自行指定
```
```markdown
get 请求方式：
tomacat对GET和POST请求处理方式是不同的，GET 请求的编码问题，要改 tomcat 的 server.xml
配置文件，如下：
<Connector connectionTimeout="20000" port="8080"
protocol="HTTP/1.1" redirectPort="8443"/>
改为：
<Connector connectionTimeout="20000" port="8080"
protocol="HTTP/1.1" redirectPort="8443"
useBodyEncodingForURI="true"/>
如果遇到 ajax 请求仍然乱码，请把：
useBodyEncodingForURI="true"改为 URIEncoding="UTF-8"
即可。

 1.针对于post方式中文乱码解决方案:
 <!--配置filter-->
<filter>
	<filter-name>charset</filter-name>
	<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
	<init-param>
		<param-name>encoding</param-name>
		<param-value>UTF-8</param-value>
	</init-param>
</filter>
<filter-mapping>
	<filter-name>charset</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>

在 springmvc 的配置文件中可以配置，静态资源不过滤：
<!-- location 表示路径，mapping 表示文件，**表示该目录下的文件以及子目录的文件 -->
<mvc:resources location="/css/" mapping="/css/**"/>
<mvc:resources location="/images/" mapping="/images/**"/>
<mvc:resources location="/scripts/" mapping="/javascript/**"/>
```
##  8 常用注解
```java
@RequestMapping
作用：
把请求中指定名称的参数给控制器中的形参赋值。
属性：
value：请求参数中的名称。
required：请求参数中是否必须提供此参数。默认值：true。表示必须提供，如果不提供将报错。
jsp 中的代码：
<!-- requestParams 注解的使用 -->
<a href="springmvc/useRequestParam?name=test">requestParam 注解</a>
控制器中的代码：
@RequestMapping("/useRequestParam")
public String useRequestParam(@RequestParam("name")String username,@RequestParam(value="age",required=false)Integer age){
	System.out.println(username+","+age);
	return "success";
}
 
@RequestBody
作用：
用于获取请求体内容。直接使用得到是 key=value&key=value...结构的数据。
get 请求方式不适用。
属性：
required：是否必须有请求体。默认值是:true。当取值为 true 时,get 请求方式会报错。如果取值
为 false，get 请求得到是 null。
post 请求 jsp 代码：
<!-- request body 注解 -->
<form action="springmvc/useRequestBody" method="post">
用户名称：<input type="text" name="username" ><br/>
用户密码：<input type="password" name="password" ><br/>
用户年龄：<input type="text" name="age" ><br/>
<input type="submit" value="保存">
</form>
get 请求 jsp 代码：
<a href="springmvc/useRequestBody?body=test">requestBody 注解 get 请求</a>
控制器代码：
@RequestMapping("/useRequestBody")
public String useRequestBody(@RequestBody(required=false) String body){
	System.out.println(body);
	return "success";
}
```
```markdown
post 请求运行结果：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210124130432496.png)
```markdown
get 请求运行结果：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210124130458383.png)

```java
@PathVariable
作用：
用于绑定 url 中的占位符。例如：请求url 中 /delete/{id}，这个{id}就是 url 占位符。
url 支持占位符是 spring3.0 之后加入的。是springmvc支持rest 风格URL的一个重要标志。
属性：
value：用于指定 url 中占位符名称。
required：是否必须提供占位符。

jsp 代码：
<!-- PathVariable 注解 -->
<a href="springmvc/usePathVariable/100">pathVariable 注解</a>
控制器代码：
@RequestMapping("/usePathVariable/{id}")
public String usePathVariable(@PathVariable("id") Integer id){
	System.out.println(id);
	return "success";
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210124133141233.png)
```java
@RequestHeader
作用：
用于获取请求消息头。
属性：
value：提供消息头名称
required：是否必须有此消息头
注：
在实际开发中一般不怎么用。
jsp 中代码：
<!-- RequestHeader 注解 -->
<a href="springmvc/useRequestHeader">获取请求消息头</a>

控制器中代码：

@RequestMapping("/useRequestHeader")
public String useRequestHeader(@RequestHeader(value="Accept-Language",
	required=false)String requestHeader){
	System.out.println(requestHeader);
	return "success";
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210124134407265.png)
```java
@CookieValue
作用：
用于把指定 cookie 名称的值传入控制器方法参数。
属性：
value：指定 cookie 的名称。
required：是否必须有此 cookie。
jsp 中的代码：
<!-- CookieValue 注解 -->
<a href="springmvc/useCookieValue">绑定 cookie 的值</a>

控制器中的代码：
@RequestMapping("/useCookieValue")
public String useCookieValue(@CookieValue(value="JSESSIONID",required=false) String cookieValue){
	System.out.println(cookieValue);
	return "success";
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210124134556976.png)
```java
@ModelAttribute
作用：
该注解是 SpringMVC4.3 版本以后新加入的。它可以用于
修饰方法和参数。
出现在方法上，表示当前方法会在控制器的方法执行之前，先执行。
它可以修饰没有返回值的方法，也可以修饰有具体返回值的方法。
出现在参数上，获取指定的数据给参数赋值。
属性：
value：用于获取数据的 key。key 可以是 POJO 的属性名称，
也可以是 map 结构的 key。
应用场景：
当表单提交数据不是完整的实体类数据时，保证没有提交数
据的字段使用数据库对象原来的数据。
例如：
我们在编辑一个用户时，用户有一个创建信息字段，该字段
的值是不允许被修改的。在提交表单数据是肯定没有此字段
的内容，一旦更新会把该字段内容置为null，此时就可以使
用此注解解决问题。

基于 POJO 属性的基本使用：
jps 代码：
<a href="springmvc/testModelAttribute?username=test">测试 modelattribute</a>
控制器代码：
/**
* 被 ModelAttribute 修饰的方法
* @param user
*/
@ModelAttribute
public void showModel(User user) {
System.out.println("执行了 showModel 方法"+user.getUsername());
}
/**
* 接收请求的方法
* @param user
* @return
*/
@RequestMapping("/testModelAttribute")
public String testModelAttribute(User user) {
	System.out.println("执行了控制器的方法"+user.getUsername());
	return "success";
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210124140642851.png)
```java
基于 Map 的应用场景示例 1：ModelAttribute 修饰方法带返回值
需求：
修改用户信息，要求用户的密码不能修改
jsp 的代码：
<!-- 修改用户信息 -->
<form action="springmvc/updateUser" method="post">
用户名称：<input type="text" name="username" ><br/>
用户年龄：<input type="text" name="age" ><br/>
<input type="submit" value="保存">
</form>

控制的代码：
@ModelAttribute
public User showModel(String username) {
//模拟去数据库查询
User abc = findUserByName(username);
System.out.println("执行了 showModel 方法"+abc);
return abc;
}
/**
* 模拟修改用户方法
* @param user
* @return
*/
@RequestMapping("/updateUser")
public String testModelAttribute(User user) {
System.out.println("控制器中处理请求的方法：修改用户："+user);
return "success";
}
/**
* 模拟去数据库查询
* @param username
* @return
*/
private User findUserByName(String username) {
	User user = new User();
	user.setUsername(username);
	user.setAge(19);
	user.setPassword("123456");
	return user;
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210124141125313.png)
```java
基于 Map 的应用场景示例 1：ModelAttribute 修饰方法不带返回值
需求：
修改用户信息，要求用户的密码不能修改
jsp 中的代码：
<!-- 修改用户信息 -->
<form action="springmvc/updateUser" method="post">
用户名称：<input type="text" name="username" ><br/>
用户年龄：<input type="text" name="age" ><br/>
<input type="submit" value="保存">
</form>
控制器中的代码：
/**
* 查询数据库中用户信息
* @param user
*/
@ModelAttribute
public void showModel(String username,Map<String,User> map) {
//模拟去数据库查询
User user = findUserByName(username);
System.out.println("执行了 showModel 方法"+user);
map.put("abc",user);
}
/**
* 模拟修改用户方法
* @param user
* @return
*/
@RequestMapping("/updateUser")
public String testModelAttribute(@ModelAttribute("abc")User user) {
System.out.println("控制器中处理请求的方法：修改用户："+user);
return "success";
}
/**
* 模拟去数据库查询
* @param username
* @return
*/
private User findUserByName(String username) {
	User user = new User();
	user.setUsername(username);
	user.setAge(19);
	user.setPassword("123456");
	return user;
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210124141356704.png)
```java
@SessionAttributes
作用：
用于多次执行控制器方法间的参数共享。
属性：
value：用于指定存入的属性名称
type：用于指定存入的数据类型。
jsp 中的代码：
<!-- SessionAttribute 注解的使用 -->
<a href="springmvc/testPut">存入 SessionAttribute</a>
<hr/>
<a href="springmvc/testGet">取出 SessionAttribute</a>
<hr/>
<a href="springmvc/testClean">清除 SessionAttribute</a>


控制器中的代码：
@Controller("sessionAttributeController")
@RequestMapping("/springmvc")
@SessionAttributes(value ={"username","password"},types={Integer.class})
public class SessionAttributeController {
/**
* 把数据存入 SessionAttribute
* @param model
* @return
* Model 是 spring 提供的一个接口，该接口有一个实现类 ExtendedModelMap
* 该类继承了 ModelMap，而 ModelMap 就是 LinkedHashMap 子类
*/
	@RequestMapping("/testPut")
	public String testPut(Model model){
		// 底层会存储到request作用域中
		 model.addAttribute("username", "泰斯特");
		 model.addAttribute("password","123456");
		 model.addAttribute("age", 31);
	 //跳转之前将数据保存到 username、password 和 age 中，因为注解@SessionAttribute 中有这几个参数
	 return "success";
	 }
	
	 @RequestMapping("/testGet")
	 public String testGet(ModelMap model){
	
		System.out.println(model.get("username")+";"+model.get("password")+";"+model.get("age"));
		return "success";
	 }
	
	 @RequestMapping("/testClean")
	 public String complete(SessionStatus sessionStatus){
	 	sessionStatus.setComplete();
	 	return "success";
	 }
}
```
## 9 响应数据和结果视图
### 9.1 返回值分类
#### 9.1.1 字符串
```java
//controller 方法返回字符串可以指定逻辑视图名，通过视图解析器解析为物理视图地址。
//指定逻辑视图名，经过视图解析器解析为 jsp 物理路径：/WEB-INF/pages/success.jsp
@RequestMapping("/testReturnString")
		public String testReturnString() {
			System.out.println("AccountController 的 testReturnString 方法执行了。。。。");
			return "success";
		}
```
#### 9.1.2 void
```java
在昨天的学习中，我们知道 Servlet 原始 API 可以作为
控制器中方法的参数：
@RequestMapping("/testReturnVoid")
public void testReturnVoid(HttpServletRequest request,HttpServletResponse response) throws Exception {
}
在 controller 方法形参上可以定义 request 和 response，使用 request 或 response 指定响应结果：
1、使用 request 转向页面，如下：
request.getRequestDispatcher("/WEB-INF/pages/success.jsp").forward(request,
response);
2、也可以通过 response 页面重定向：
response.sendRedirect("testRetrunString")
3、也可以通过 response 指定响应结果，例如响应 json 数据：
response.setCharacterEncoding("utf-8");
response.setContentType("application/json;charset=utf-8");
response.getWriter().write("json 串");
```
#### 9.1.3  ModelAndView
```java
ModelAndView 是 SpringMVC 为我们提供的一个对象，该对
象也可以用作控制器方法的返回值。
@RequestMapping("/testReturnModelAndView")
public ModelAndView testReturnModelAndView() {
	ModelAndView mv = new ModelAndView();
	mv.addObject("username", "张三");
	mv.setViewName("success");
	return mv;
}
响应的 jsp 代码：
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>执行成功</title>
</head>
<body>
执行成功！
${requestScope.username}
</body>
</html>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210124152414594.png)

###  9.2 SpringMVC中跳转方式

#### 9.2.1 跳转方式
```markdown
说明 : 跳转有两种,一种forward,一种是redirect。forward跳转,一
次请求,地址栏不变,redirect跳转多次请求,地址栏改变。
```


```markdown
# 1. Controller跳转到JSP

forward跳转到页面 :   默认就是forward跳转 		
语法:   return "页面逻辑名"

redirect跳转到页面:   使用springmvc提供redirect:关键字进行重定向页面跳转
语法:   return "redirect:/index.jsp"  
注意:   使用redirect跳转页面不会经过试图解析器

# 2. Controller跳转到Controller
			
forward跳转到Controller  :  使用springmvc提供的关键字forward:
语法:  forward:/跳转类上@requestMapping的值/跳转类上@RequestMapping的值
												   
redirect:跳转到Controller:  
使用springmvc提供关键字redirect:
语法:  redirect:/跳转类上@requestMapping的值/跳转类上@RequestMapping的值
```
```java
forward 转发
controller 方法在提供了 String 类型的返回值之后，默认就是请求转发。我们也可以写成：
@RequestMapping("/testForward")
public String testForward() {
System.out.println("AccountController 的 testForward 方法执行了。。。。");
return "forward:/WEB-INF/pages/success.jsp";
}
需要注意的是，如果用了 formward：则路径必须写成实际视图 url，不能写逻辑视图。
它相当于“request.getRequestDispatcher("url").forward(request,response)”。使用请求
转发，既可以转发到 jsp，也可以转发到其他的控制器方法。

Redirect 重定向
contrller 方法提供了一个 String 类型返回值之后，它需要在返回值里使用:redirect:

@RequestMapping("/testRedirect")
public String testRedirect() {
	System.out.println("AccountController 的 testRedirect 方法执行了。。。。");
	return "redirect:testReturnModelAndView";
}
它相当于“response.sendRedirect(url)”。
需要注意的是，如果是重定向到 jsp 页面，则 jsp 页面不
能写在 WEB-INF 目录中，否则无法找到。
```
#### 9.2.2 跳转方式总结
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022183424327.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

### 9.3 ResponseBody响应json数据
```java
@ResponseBody: 为了进一步方便控制器与ajax集成,springmvc
提供@ResponseBody该注解用在方法的返回值上,代表可以将方法的
返回值转换为json格式字符串并响应到前台,省去了通过第三
方工具转换json的过程
```
```xml
json字符串和JavaBean对象互相转换的过程中，需要使用
jackson的jar包
<dependency>
	<groupId>com.fasterxml.jackson.core</groupId>
	<artifactId>jackson-databind</artifactId>
	<version>2.9.0</version>
</dependency>
<dependency>
	<groupId>com.fasterxml.jackson.core</groupId>
	<artifactId>jackson-core</artifactId>
	<version>2.9.0</version>
</dependency>
<dependency>
	<groupId>com.fasterxml.jackson.core</groupId>
	<artifactId>jackson-annotations</artifactId>
	<version>2.9.0</version>
</dependency>
```
```java
1 DispatcherServlet会拦截到所有的资源，导致一个问题就是静
态资源（img、css、js）也会被拦截到，从而
不能被使用。解决问题就是需要配置静态资源不进行拦截，
在springmvc.xml配置文件添加如下配置
2 mvc:resources标签配置不过滤
location元素表示webapp目录下的包下的所有文件
mapping元素表示以/static开头的所有请求路径，如/static/a 或者/static/a/b
<!-- 设置静态资源不过滤 -->
<mvc:resources location="/css/" mapping="/css/**"/> <!-- 样式 -->
<mvc:resources location="/images/" mapping="/images/**"/> <!-- 图片 -->
<mvc:resources location="/js/" mapping="/js/**"/> <!-- javascript -->


3 使用@RequestBody获取请求体数据
// 页面加载
// 页面加载
$(function(){
	// 绑定点击事件
	$("#btn").click(function(){
	$.ajax({
		url:"user/testJson",
		contentType:"application/json;charset=UTF-8",
		data:'{"addressName":"aa","addressNum":100}',
		dataType:"json",
		type:"post",
		success:function(data){
		alert(data);
		alert(data.addressName);
		}
		});
	});
});

/**
* 获取请求体的数据
* @param body
*/
@RequestMapping("/testJson")
public void testJson(@RequestBody String body) {
	System.out.println(body);
}

4 使用@RequestBody注解把json的字符串转换成JavaBean的对象
// 页面加载
// 页面加载
$(function(){
	// 绑定点击事件
	$("#btn").click(function(){
		$.ajax({
			url:"user/testJson",
			contentType:"application/json;charset=UTF-8",
			data:'{"addressName":"aa","addressNum":100}',
			dataType:"json",
			type:"post",
			success:function(data){
			alert(data);
			alert(data.addressName);
			}
		});
	});
});
/**
* 获取请求体的数据
* @param body
*/
@RequestMapping("/testJson")
public void testJson(@RequestBody Address address) {
System.out.println(address);
}

5 使用@ResponseBody注解把JavaBean对象转换成json字符串，直接响应
1 要求方法需要返回JavaBean的对象
// 页面加载
$(function(){
// 绑定点击事件
	$("#btn").click(function(){
		$.ajax({
			url:"user/testJson",
			contentType:"application/json;charset=UTF-8",
			data:'{"addressName":"哈哈","addressNum":100}',
			dataType:"json",
			type:"post",
			success:function(data){
			alert(data);
			alert(data.addressName);
			}
			});
	});
});
@RequestMapping("/testJson")
public @ResponseBody Address testJson(@RequestBody Address address) {
	System.out.println(address);
	address.setAddressName("上海");
	return address;
}
```


## 10 SpringMVC处理静态资源拦截

```xml
# 1.处理静态资源拦截

# 问题:当web.xml中配置为"/"时,会拦截项目静态资源
<mvc:default-servlet-handler/>
或者
<mvc:resources location="/css/" mapping="/css/**" />
<mvc:resources location="/images/" mapping="/images/**" />
<mvc:resources location="/js/" mapping="/js/**" />
```

## 11 SpringMVC实现文件上传
```markdown
文件上传: 指的就是将用户本地计算机中文件上传到服务器上的过程称之
为文件上传

文件上传的必要前提
form 表单的 enctype 取值必须是：multipart/form-data
(默认值是:application/x-www-form-urlencoded)
enctype:是表单请求正文的类型

method 属性取值必须是 Post
提供一个文件选择域<input type=”file” />

文件上传的原理分析
当 form 表单的 enctype 取值不是默认值后，request.getParameter()将失效。
enctype=”application/x-www-form-urlencoded”时，form 表单的正文内容是：
key=value&key=value&key=value
当 form 表单的 enctype 取值为 Mutilpart/form-data 时，请求正文内容就变成：
每一部分都是 MIME 类型描述的正文
-----------------------------7de1a433602ac 分界符
Content-Disposition: form-data; name="userName" 协议头
aaa 协议的正文
-----------------------------7de1a433602ac
Content-Disposition: form-data; name="file";
filename="C:\Users\zhy\Desktop\fileupload_demofile\b.txt"
Content-Type: text/plain 协议的类型（MIME 类型）
bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
-----------------------------7de1a433602ac--
```
### 11.1 文件上传编程步骤

```markdown
1 项目中引入相关依赖
```

```xml
<dependency>
	<groupId>commons-fileupload</groupId>
	<artifactId>commons-fileupload</artifactId>
	<version>1.3.1</version>
</dependency>
<dependency>
	<groupId>commons-io</groupId>
	<artifactId>commons-io</artifactId>
	<version>2.4</version>
</dependency>
```

```markdown
2 开发页面
```

```html
<h1>文件上传</h1>
<form action="${pageContext.request.contextPath}/file/upload" method="post" enctype="multipart/form-data">
   <input type="file" name="bb"/>
   <input type="submit" value="上传文件"/>
</form>
```

```markdown
3 开发控制器
```

```java
   @RequestMapping("upload")
    public String upload(MultipartFile bb, HttpServletRequest request) throws IOException {
        System.out.println("文件名: "+bb.getOriginalFilename());
        System.out.println("文件类型: "+bb.getContentType());
        System.out.println("文件大小: "+bb.getSize());
        //处理文件上传
        //根据相对upload获取绝对upload路径
        String realPath = request.getSession().getServletContext().getRealPath("/upload");
        //修改文件名  新的文件名生成策略  uuid   时间戳方式
        String newFileNamePrefix = new SimpleDateFormat("yyyyMMddHHmmssSSS").format(new Date())
                                   +UUID.randomUUID().toString().replace("-","");
        //String fileNameSuffix = bb.getOriginalFilename().substring(bb.getOriginalFilename().lastIndexOf("."));
        String fileNameSuffix = "."+FilenameUtils.getExtension(bb.getOriginalFilename());
        System.out.println("动态获取文件类型: "+request.getSession().getServletContext().getMimeType("."+fileNameSuffix));

        //新的文件名
        String newFileName = newFileNamePrefix+fileNameSuffix;
        //创建日期目录
        String dateDirString = new SimpleDateFormat("yyyy-MM-dd").format(new Date());
        File dateDir = new File(realPath, dateDirString);
        if(!dateDir.exists()){
            dateDir.mkdirs();
        }
        //文件上传
        bb.transferTo(new File(dateDir,newFileName));
        return "index";
    }
```

```markdown
4 配置文件上传解析器
```

```xml
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
  <!--控制文件上传大小单位字节 默认没有大小限制 这里是2MB-->
  <property name="maxUploadSize" value="2097152"/>
</bean>
```
```markdown
注意:使用springmvc中multipartfile接收客户端上传的文件必须配置文件上传解析器且解析的id必须为multipartResolver
```


## 12 文件下载
```markdown
文件下载:将服务器上的文件下载到当前用户访问的计算机的过程称之为文件下载
```



### 12.1 文件下载编程思路

```markdown
1 项目中准备下载目录并存在下载的相关文件
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022183858507.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


```markdown
2 开发下载控制器
```

```java
	@RequestMapping("down")
   public void down(String fileName,String openStyle, HttpServletRequest request, HttpServletResponse response) throws IOException {
       openStyle = openStyle==null?"attachment":openStyle;
       //根据相对目录获取绝对路径
       String realPath = request.getSession().getServletContext().getRealPath("/down");
       //读取下载的指定文件
       File file = new File(realPath, fileName);
       //获取文件输入流
       FileInputStream is = new FileInputStream(file);

       //默认下载方式是在线打开 inline   附件形式下载  attachment
       response.setHeader("content-disposition",openStyle+";fileName="+ URLEncoder.encode(fileName,"UTF-8"));
       //响应输出流
       ServletOutputStream os = response.getOutputStream();

       //IO拷贝
       IOUtils.copy(is,os);
       IOUtils.closeQuietly(is);//优雅的关流
       IOUtils.closeQuietly(os);//优雅的关流
}
```
```markdown
注意:下载时必须设置响应的头信息,指定文件以何种方式保存,另
外下载文件的控制器不能存在返回值,代表响应只用来下载文件信息
```

```markdown
3 开发页面
```

```html
<h1>文件下载</h1>
<a href="${pageContext.request.contextPath}/file/download?fileName=init.txt">init.txt</a>
```

## 13 SpringMVC中拦截器

### 13.1 作用
```markdown
作用:类似于javaweb中的Filter,用来对请求进行拦截,可以将多个
Controller中执行的共同代码放入拦截器中执行,减少Controller类中代码的冗余.
```

### 13.2 特点
```markdown
拦截器器只能拦截Controller的请求,不能拦截jsp

拦截器可中断用户的请求轨迹
请求先经过拦截器,之后还会经过拦截器
preHandle方法会在进入controller方法体(这里指@RequestMapping等注释的方法体)之前执行。
postHandle会在controller方法体执行完但是还没处理return 语句之前执行。
afterCompletion会在controller方法体内处理return 语句之后执行。
```



### 13.3 开发拦截器

```java
package com.yxj.interceptors;

import org.springframework.web.method.HandlerMethod;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.lang.reflect.Method;

/**
 * 自定义拦截器
 */
public class MyInterceptor  implements HandlerInterceptor {

    //1.请求经过拦截器会优先进入拦截器中preHandler方法执行preHandler方法中内容
    //2.如果preHandler 返回为true 代表放行请求  如果返回值为false 中断请求
    //3.如果preHandler返回值为true,会执行当前请求对应的控制器中方法
    //4.当控制器方法执行结束之后,会返回拦截器中执行拦截器中postHandler方法
    //5.posthanlder执行完成之后响应请求,在响应请求完成后会执行afterCompletion方法
    @Override
    //参数1: 当前请求对象 参数2:当前请求对应响应对象  参数3:当前请求的控制器对应的方法对象
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println(((HandlerMethod)handler).getMethod().getName());
        System.out.println("===========1=============");
        //强制用户登录
        /*Object user = request.getSession().getAttribute("user");
        if(user==null){
            //重定向到登录页面
            response.sendRedirect(request.getContextPath()+"/login.jsp");
            return false;
        }*/
        return true;
    }

    @Override
    //参数1: 当前请求对象 参数2:当前请求对应响应对象  参数3:当前请求的控制器对应的方法对象 参数4: 当前请求控制器方法返回值 = 当前请求控制器方法返回的modelandview对象 modelandview 模型和试图
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println(modelAndView);
        System.out.println("===========3=============");
    }


    @Override
    //注意: 无论正确还是失败都会执行
    //参数1: 当前请求对象 参数2:当前请求对应响应对象  参数3:当前请求的控制器对应的方法对象  参数4: 请求过程中出现异常时异常对象
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        if(ex!=null){
            System.out.println(ex.getMessage());
        }
        System.out.println("===========4=============");
    }
}
```

### 13.4 配置拦截器

```xml
# 单个拦截器
 <!--注册拦截器-->
    <bean id="myInterceptor" class="com.yxj.interceptors.MyInterceptor"/>


    <!--配置拦截器-->
    <mvc:interceptors>

        <!--配置一个拦截器-->
        <mvc:interceptor>
            <!--mvc:mapping 代表拦截那个请求路径-->
            <mvc:mapping path="/json/*"/>
            <!--mvc:exclude-mapping 排除拦截那个请求-->
            <mvc:exclude-mapping path="/json/showAll"/>
            <!--使用那个拦截器-->
            <ref bean="myInterceptor"/>
        </mvc:interceptor>

    </mvc:interceptors>

# 多个拦截器
 <!--配置拦截器-->
    <bean id="myInterceptor1" class="com.yxj.interceptors.MyInterceptor1"></bean>
    <bean id="myInterceptor2" class="com.yxj.interceptors.MyInterceptor2"></bean>

    <!--配置拦截器-->
    <mvc:interceptors>
        <mvc:interceptor>
            <!--拦截的请求是谁-->
            <mvc:mapping path="/json/**"/>
            <!--放行某些请求-->
            <!--<mvc:exclude-mapping path="/json/test2"/>-->
            <ref bean="myInterceptor1"/>
            <!--<bean id="handlerInterceptorDemo2"
class="com.yxj.interceptors.MyInterceptor1"></bean> -->
        </mvc:interceptor>

        <mvc:interceptor>
            <mvc:mapping path="/json/test2"/>
            <ref bean="myInterceptor2"></ref>
        </mvc:interceptor>


    </mvc:interceptors>
```
```markdown
/*: 代表拦截所有请求路径
```


## 14 springMVC全局异常处理

### 14.1 作用
```markdown
当控制器中某个方法在运行过程中突然发生运行时异常时,为
了增加用户体验对于用户不能出现500错误代码,应该给用户良
好展示错误界面,全局异常处理就能更好解决这个问题
```

### 14.2 全局异常处理开发

```java
public class GlobalExceptionResolver  implements HandlerExceptionResolver {


    /**
     * 用来处理发生异常时方法
     * @param request   当前请求对象
     * @param response  当前请求对应的响应对象
     * @param handler   当前请求的方法对象
     * @param ex        当前出现异常时的异常对象
     * @return          出现异常时展示视图和数据
     */
    @Override
    public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        System.out.println("进入全局异常处理器获取的异常信息为: "+ex.getMessage());
        ModelAndView modelAndView = new ModelAndView();
        //基于不同业务异常跳转到不同页面
        if(ex instanceof UserNameNotFoundException){
            modelAndView.setViewName("redirect:/login.jsp");
        }else{
            modelAndView.setViewName("redirect:/error.jsp");//return "error" ===>  /error.jsp
        }
        //modelandview中model 默认放入request作用域  如果使用redirect跳转:model中数据会自动拼接到跳转url
        modelAndView.addObject("msg",ex.getMessage());
        return modelAndView;
    }
}


package com.yxj.exceptions;

/**
 * Created by HIAPAD on 2019/10/28.
 */
//自定义用户名不存在异常类
public class UserNameNotFoundException extends RuntimeException {


    public UserNameNotFoundException(String message) {
        super(message);
    }
}
```

### 14.3 配置全局异常处理

```xml
<!--配置全局异常处理 -->
<bean class="com.yxj.handlerxception.GlobalExceptionResolver"/>
```




## 15 图片
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022184658946.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022184711314.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022184728427.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022184742288.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102218475318.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022184807825.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022184829696.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022184845702.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022184953501.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022185009333.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022185024668.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022185038123.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022185051655.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022185105657.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022185119398.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复
mvc即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)



















