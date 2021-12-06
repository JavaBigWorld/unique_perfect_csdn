# JavaWeb
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210716180505679.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1 JavaWeb 的概念
```markdown
1 什么是 JavaWeb
JavaWeb 是指，所有通过Java语言编写可以通过浏览器访问的
程序的总称，叫 JavaWeb。
JavaWeb 是基于请求和响应来开发的。
2 什么是请求
请求是指客户端给服务器发送数据，叫请求 Request
3 什么是响应
响应是指服务器给客户端回传数据，叫响应 Response
4 请求和响应的关系
请求和响应是成对出现的，有请求就有响应。
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109223228601.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 1.1 路径问题
```markdown
相对路径跟绝对路径:
http://www.baidu.com/project/aa.html
以http开头的是绝对路径
以/开头分两种：
1 java、配置文件（xml/properties等），这个路径为后台
路径,此时的/表示web应用的根即http://www.baidu.com/project/
2 静态页面、动态页面的静态部分（jsp页面除了java代码），
这个路径称为前台路径，此时的/表示web服务器的根,
即http://www.baidu.com/，重定向的java代码中的/也表
示前台路径。

不带/开头的:
此时表示当前的访问路径，即http://www.baidu.com/project/，
即去掉资源的名称
```

## 2 Web 资源的分类
```markdown
web 资源按实现的技术和呈现的效果的不同，又分为静态
资源和动态资源两种。
静态资源： html、css、js、txt、mp4 视频 , jpg 图片
动态资源： jsp 页面、Servlet 程序
```
## 3 常用的 Web 服务器
```markdown
Tomcat：由 Apache 组织提供的一种 Web 服务器，提供对 jsp 
和 Servlet 的支持。它是一种轻量级的 javaWeb 容器（服务
器），也是当前应用最广的 JavaWeb 服务器（免费）。

Jboss：是一个遵从 JavaEE 规范的、开放源代码的、
纯 Java 的 EJB 服务器，它支持所有的 JavaEE 规范（免费）。

GlassFish： 由 Oracle 公司开发的一款 JavaWeb 服务器，是一
款强健的商业服务器，达到产品级质量（应用很少）。

Resin：是 CAUCHO 公司的产品，是一个非常流行的服务器，
对 servlet 和 JSP 提供了良好的支持，
性能也比较优良，resin 自身采用 JAVA 语言开发（收费，应用比较多）。

WebLogic：是 Oracle 公司的产品，是目前应用最广泛的 Web 服务器，
支持 JavaEE 规范，
而且不断的完善以适应新的开发要求，适合大型项目（收费，用的不多，
适合大公司）。
```
## 4 Tomcat 服务器和 Servlet 版本的对应关系
```markdown
当前企业常用的版本 7.*、8.*
Servlet 程序从 2.5 版本是现在世面使用最多的版本（xml 配置）
到了 Servlet3.0 之后。就是注解版本的 Servlet 使用。
以 2.5 版本为主线讲解 Servlet 程序。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109223538138.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 5 Tomcat 的使用
```markdown
1 找到你需要用的 Tomcat 版本对应的 zip 压缩包，解压到需要
安装的目录即可。 b)目录介绍
bin 专门用来存放 Tomcat 服务器的可执行程序
conf 专门用来存放 Tocmat 服务器的配置文件
lib 专门用来存放 Tomcat 服务器的 jar 包
logs 专门用来存放 Tomcat 服务器运行时输出的日记信息
temp 专门用来存放 Tomcdat 运行时产生的临时数据
webapps 专门用来存放部署的 Web 工程。
work 是 Tomcat 工作时的目录，用来存放Tomcat运行时 jsp 
翻译为 Servlet 的源码，和 Session 钝化的目录。

2 如何启动 Tomcat 服务器
找到 Tomcat 目录下的 bin 目录下的 startup.bat 文件，
双击，就可以启动 Tomcat 服务器。
如何测试 Tomcat 服务器启动成功？？？
打开浏览器，在浏览器地址栏中输入以下地址测试：
1、http://localhost:8080
2、http://127.0.0.1:8080
3、http://真实 ip:8080
当出现如下界面，说明 Tomcat 服务器启动成功！！！
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109224323102.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
常见的启动失败的情况有，双击 startup.bat 文件，就会出
现一个小黑窗口一闪而来。
这个时候，失败的原因基本上都是因为没有配置好JAVA_HOME环境变量。
配置 JAVA_HOME 环境变量：
常见的 JAVA_HOME 配置错误有以下几种情况：
一：JAVA_HOME 必须全大写。
二：JAVA_HOME 中间必须是下划线，不是减号- 
三：JAVA_HOME 配置的路径只需要配置到 jdk 的安装目录即可。
不需要带上 bin 目录。
```

### 5.1 另一种启动 tomcat 服务器的方式
```markdown
1 打开命令行
2 cd 到 你的 Tomcat 的 bin 目录下
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109225303198.png#pic_center)
```markdown
3 敲入启动命令： catalina run
```

```markdown
Tomcat 的停止
1 点击 tomcat 服务器窗口的 x 关闭按钮
2 把 Tomcat 服务器窗口置为当前窗口，然后按快捷键 Ctrl+C
3 找到 Tomcat 的 bin 目录下的 shutdown.bat 双击，就可以停止 
Tomcat 服务器
```

```markdown
如何修改 Tomcat 的端口号
Mysql 默认的端口号是：3306
Tomcat 默认的端口号是：8080
找到 Tomcat 目录下的 conf 目录，找到 server.xml 配置文件。
```

```markdown
如何部暑 web 工程到 Tomcat 中
第一种部署方法：只需要把 web 工程的目录拷贝到 Tomcat 的 webapps
目录下即可。
```
```markdown
1 在 webapps 目录下创建一个 book 工程：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109225952428.png#pic_center)
```markdown
2 把做的书城第一阶段的内容拷贝到里面：
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109230303643.png#pic_center)
```markdown
3 如何访问 Tomcat 下的 web 工程。
只需要在浏览器中输入访问地址格式如下：
http://ip:port/工程名/目录下/文件名
```
```markdown
手托 html 页面到浏览器和在浏览器中输入 http://ip:端
口号/工程名/访问的区别
```
```markdown
手托 html 页面的原理：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109231452375.png#pic_center)

```markdown
输入访问地址访问的原因：
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109231522441.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


```markdown
ROOT 的工程的访问，以及 默认 index.html 页面的访
问
当我们在浏览器地址栏中输入访问地址如下：
http://ip:port/ ====>>>> 没有工程名的时候，默认访问的是 
ROOT 工程。
当我们在浏览器地址栏中输入的访问地址如下：
http://ip:port/工程名/ ====>>>> 没有资源名，
默认访问 index.html 页面
```
## 6 IDEA 整合 Tomcat 服务器
```markdown
操作的菜单如下：File | Settings | Build, Execution, Deployment | Application Servers
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109231831895.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
配置你的 Tomcat 安装目录：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109231854909.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
就可以通过创建一个 Model 查看是不是配置成功！！！
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109231925131.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
## 7 IDEA 中动态 web 工程的操作
```markdown
IDEA 中如何创建动态 web 工程
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109232049252.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109232117152.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109232157419.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109232220875.png#pic_center)
```markdown
Web 工程的目录介绍
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200611101051506.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
如何给动态 web 工程添加额外 jar 包
1 可以打开项目结构菜单操作界面，添加一个自己的类库：
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109232332572.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
2 添加你你类库需要的 jar 包文件。
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020110923240944.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
3 选择你添加的类库，给哪个模块使用：
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109232426289.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
4 选择 Artifacts 选项，将类库，添加到打包部署中：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109232520291.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
如何在 IDEA 中部署工程到 Tomcat 上运行
1 建议修改 web 工程对应的 Tomcat 运行实例名称
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109232755196.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
2 确认你的 Tomcat 实例中有你要部署运行的 web 工程模块：
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109233441777.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
3 你还可以修改你的 Tomcat 实例启动后默认的访问地址：
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109233507757.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
4 在 IDEA 中如何运行，和停止 Tomcat 实例。
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109233806121.png#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109233825123.png#pic_center)


![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109233845244.png#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109233923501.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
修改工程访问路径
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109233949888.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
修改运行的端口号
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109234305165.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
修改运行使用的浏览器
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109234439380.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
配置资源热部署
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109234508827.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 8 Servlet 技术
### 8.1 Servlet 技术

```markdown
什么是 Servlet
1 Servlet 是 JavaEE 规范之一。规范就是接口
2 Servlet 就 JavaWeb 三大组件之一。
三大组件分别是：Servlet 程序、Filter 过滤器、Listener 监听器。
3 Servlet 是运行在服务器上的一个 java 小程序，它可以接收客户端发
送过来的请求，并响应数据给客户端。 

手动实现 Servlet 程序
1 编写一个类去实现 Servlet 接口
2 实现 service 方法，处理请求，并响应数据
3 到 web.xml 中去配置 servlet 程序的访问地址
```
```markdown
Servlet 程序的示例代码
```

```java
public class HelloServlet implements Servlet {
/**
* service 方法是专门用来处理请求和响应的
* @param servletRequest
* @param servletResponse
* @throws ServletException
* @throws IOException
*/
@Override
public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws
	ServletException, IOException {
	System.out.println("Hello Servlet 被访问了");
}
}

```
```markdown
web.xml
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
version="4.0">
	<!-- servlet 标签给 Tomcat 配置 Servlet 程序 -->
	<servlet>
		<!--servlet-name 标签 Servlet 程序起一个别名（一般是类名） -->
		<servlet-name>HelloServlet</servlet-name>
		<!--servlet-class 是 Servlet 程序的全类名-->
		<servlet-class>com.yxj.servlet.HelloServlet</servlet-class>
	</servlet>
	<!--servlet-mapping 标签给 servlet 程序配置访问地址-->
	<servlet-mapping>
		<!--servlet-name 标签的作用是告诉服务器，我当前配置的地址给哪个 Servlet 程序使用-->
		<servlet-name>HelloServlet</servlet-name>
		<!--url-pattern 标签配置访问地址 <br/>
		/ 斜杠在服务器解析的时候，表示地址为：http://ip:port/工程路径 <br/>
		/hello 表示地址为：http://ip:port/工程路径/hello <br/>
		-->
		<url-pattern>/hello</url-pattern>
	</servlet-mapping>
</web-app>
```
```markdown
常见的错误 1：url-pattern 中配置的路径没有以斜杠打头。
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200611234704977.png)
```markdown
常见错误 2：servlet-name 配置的值不存在：
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200611234716724.png)
```markdown
常见错误 3：servlet-class 标签的全类名配置错误：
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200611234729332.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
url 地址到 Servlet 程序的访问
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200611234918317.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
Servlet 的生命周期
```

```markdown
1 执行 Servlet 构造器方法
2 执行 init 初始化方法
第一、二步，是在第一次访问，的时候创建 Servlet 程序会调用。

3 执行 service 方法
第三步，每次访问都会调用。
4 执行 destroy 销毁方法
第四步，在 web 工程停止的时候调用。
```
```markdown
GET 和 POST 请求的分发处理
```
```java
public class HelloServlet implements Servlet {
	/**
	* service 方法是专门用来处理请求和响应的
	* @param servletRequest
	* @param servletResponse
	* @throws ServletException
	* @throws IOException
	*/
	@Override
	public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws
	ServletException, IOException {
		System.out.println("3 service === Hello Servlet 被访问了");
		// 类型转换（因为它有 getMethod()方法）
		HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
		// 获取请求的方式
		String method = httpServletRequest.getMethod();
		if ("GET".equals(method)) {
			doGet();
		} else if ("POST".equals(method)) {
			doPost();
		}
		}
	/**
	* 做 get 请求的操作
	*/
	public void doGet(){
		System.out.println("get 请求");
		System.out.println("get 请求");
	}
	/**
	* 做 post 请求的操作
	*/
	public void doPost(){
		System.out.println("post 请求");
		System.out.println("post 请求");
	}
}
```
```markdown
通过继承 HttpServlet 实现 Servlet 程序
```
```markdown
一般在实际项目开发中，都是使用继承 HttpServlet 类的方式
去实现 Servlet 程序。
1 编写一个类去继承 HttpServlet 类
2 根据业务需要重写 doGet 或 doPost 方法
3 到 web.xml 中的配置 Servlet 程序的访问地址
```
```markdown
Servlet 类的代码：
```
```java
public class HelloServlet2 extends HttpServlet {
	/**
	* doGet（）在 get 请求的时候调用
	* @param req
	* @param resp
	* @throws ServletException
	* @throws IOException
	*/
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
	IOException {
		System.out.println("HelloServlet2 的 doGet 方法");
	}
	/**
	* doPost（）在 post 请求的时候调用
	* @param req
	* @param resp
	* @throws ServletException
	* @throws IOException
	*/
	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
	IOException {
		System.out.println("HelloServlet2 的 doPost 方法");
	}
}

```
```markdown
web.xml 中的配置：
```
```xml
<servlet>
<servlet-name>HelloServlet2</servlet-name>
<servlet-class>com.yxj.servlet.HelloServlet2</servlet-class>
</servlet>
<servlet-mapping>
<servlet-name>HelloServlet2</servlet-name>
<url-pattern>/hello2</url-pattern>
</servlet-mapping>
```
```markdown
使用 IDEA 创建 Servlet 程序
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612000242680.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612000305716.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
Servlet 类的继承体系
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612000514553.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 8.2.ServletConfig类
```markdown
ServletConfig 类从类名上来看，就知道是 Servlet 程序的配置信息类。
Servlet 程序和 ServletConfig 对象都是由 Tomcat 负责创建，我们负责使用。
Servlet 程序默认是第一次访问的时候创建，ServletConfig 是每个 Servlet 程序创建时，就创建一个对应的 ServletConfig 对象。
```

```markdown
ServletConfig 类的三大作用
1 可以获取 Servlet 程序的别名 servlet-name 的值
2 获取初始化参数 init-param
3 获取 ServletContext 对象
```
```markdown
web.xml 中的配置：
```

```xml
<!-- servlet 标签给 Tomcat 配置 Servlet 程序 -->
<servlet>
<!--servlet-name 标签 Servlet 程序起一个别名（一般是类名） -->
<servlet-name>HelloServlet</servlet-name>
<!--servlet-class 是 Servlet 程序的全类名-->
<servlet-class>com.yxj.servlet.HelloServlet</servlet-class>
<!--init-param 是初始化参数-->
<init-param>
<!--是参数名-->
<param-name>username</param-name>
<!--是参数值-->
<param-value>root</param-value>
</init-param>
<!--init-param 是初始化参数-->
<init-param>
<!--是参数名-->
<param-name>url</param-name>
<!--是参数值-->
<param-value>jdbc:mysql://localhost:3306/test</param-value>
</init-param>
</servlet>
<!--servlet-mapping 标签给 servlet 程序配置访问地址-->
<servlet-mapping>
<!--servlet-name 标签的作用是告诉服务器，我当前配置的地址给哪个 Servlet 程序使用-->
<servlet-name>HelloServlet</servlet-name>
<!--
url-pattern 标签配置访问地址 <br/>
/ 斜杠在服务器解析的时候，表示地址为：http://ip:port/工程路径 <br/>
/hello 表示地址为：http://ip:port/工程路径/hello <br/>
-->
<url-pattern>/hello</url-pattern>
</servlet-mapping>
```
```markdown
Servlet 中的代码：
```

```java
@Override
public void init(ServletConfig servletConfig) throws ServletException {
	System.out.println("2 init 初始化方法");
	// 1、可以获取 Servlet 程序的别名 servlet-name 的值
	System.out.println("HelloServlet 程序的别名是:" + servletConfig.getServletName());
	// 2、获取初始化参数 init-param
	System.out.println("初始化参数 username 的值是;" + servletConfig.getInitParameter("username"));
	System.out.println("初始化参数 url 的值是;" + servletConfig.getInitParameter("url"));
	// 3、获取 ServletContext 对象
	System.out.println(servletConfig.getServletContext());
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020061200413746.png)
## 9 ServletContext 类
```markdown
什么是 ServletContext
1 ServletContext 是一个接口，它表示 Servlet 上下文对象
2 一个 web 工程，只有一个 ServletContext 对象实例。
3 ServletContext 对象是一个域对象。
4 ServletContext 是在 web 工程部署启动的时候创建。
在 web 工程停止的时候销毁。

什么是域对象
域对象，是可以像 Map 一样存取数据的对象，叫域对象。
这里的域指的是存取数据的操作范围，整个 web 工程。
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612004736722.png)

```markdown
ServletContext 类的四个作用
1 获取 web.xml 中配置的上下文参数 context-param
2 获取当前的工程路径，格式: /工程名称
3 获取工程部署后在服务器硬盘上的绝对路径
4 像 Map 一样存取数据
```
```markdown
ServletContext 演示代码：
```

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws
ServletException, IOException {
	// 1、获取 web.xml 中配置的上下文参数 context-param
	ServletContext context = getServletConfig().getServletContext();
	String username = context.getInitParameter("username");
	System.out.println("context-param 参数 username 的值是:" + username);
	System.out.println("context-param 参数 password 的值是:" +
	context.getInitParameter("password"));
	// 2、获取当前的工程路径，格式: /工程路径
	System.out.println( "当前工程路径:" + context.getContextPath() );
	// 3、获取工程部署后在服务器硬盘上的绝对路径
	/**
	* / 斜杠被服务器解析地址为:http://ip:port/工程名/ 映射到 IDEA 代码的 web 目录<br/>
	*/
	System.out.println("工程部署的路径是:" + context.getRealPath("/"));
	System.out.println("工程下 css 目录的绝对路径是:" + context.getRealPath("/css"));
	System.out.println("工程下 imgs 目录 1.jpg 的绝对路径是:" + context.getRealPath("/imgs/1.jpg"));
}
```
```markdown
web.xml 中的配置：
```
```xml
<!--context-param 是上下文参数(它属于整个 web 工程)-->
<context-param>
	<param-name>username</param-name>
	<param-value>context</param-value>
	</context-param>
	<!--context-param 是上下文参数(它属于整个 web 工程)-->
<context-param>
	<param-name>password</param-name>
	<param-value>root</param-value>
</context-param>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612010731297.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
ServletContext 像 Map 一样存取数据：
```
````java
ContextServlet1
public class ContextServlet1 extends HttpServlet {
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throw ServletException, IOException {
		// 获取 ServletContext 对象
		ServletContext context = getServletContext();
		System.out.println(context);
		System.out.println("保存之前: Context1 获取 key1 的值是:"+ context.getAttribute("key1"));
		context.setAttribute("key1", "value1");
		System.out.println("Conte                                                                                                                         xt1 中获取域数据 key1 的值是:"+ context.getAttribute("key1"));
	}
}

ContextServlet2 代码：
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException,IOException {
	ServlwotContext context = getServletContext();
	System.out.println(context);
	System.out.println("Context2 中获取域数据 key1 的值是:"+ context.getAttribute("key1"));
}
````

## 10 HTTP 协议
```markdown
什么是 HTTP 协议
什么是协议?
协议是指双方，或多方，相互约定好，大家都需要遵守的规则，叫协议。
所谓 HTTP 协议，就是指，客户端和服务器之间通信时，发送的数据，需要遵守的规则，叫 HTTP 协议。
HTTP 协议中的数据又叫报文。

请求的 HTTP 协议格式
客户端给服务器发送数据叫请求。
服务器给客户端回传数据叫响应。
请求又分为 GET 请求，和 POST 请求两种
```
```markdown
GET 请求
1 请求行
(1) 请求的方式 GET
(2) 请求的资源路径[+?+请求参数]
(3) 请求的协议的版本号 HTTP/1.1
2 请求头
key : value 组成 不同的键值对，表示不同的含义。
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612084621701.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
POST 请求
1 请求行
(1) 请求的方式 POST
(2) 请求的资源路径[+?+请求参数]
(3) 请求的协议的版本号 HTTP/1.1
2 请求头
1) key : value 不同的请求头，有不同的含义
3 空行
4 请求体 ===>>> 就是发送给服务器的数据
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612084811643.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
常用请求头的说明
Accept: 表示客户端可以接收的数据类型
Accpet-Languege: 表示客户端可以接收的语言类型
User-Agent: 表示客户端浏览器的信息
Host： 表示请求时的服务器 ip 和端口号
```

```markdown
哪些是 GET 请求，哪些是 POST 请求
GET 请求有哪些：
1 form 标签 method=get
2 a 标签
3 link 标签引入 css
4 Script 标签引入 js 文件
5 img 标签引入图片
6 iframe 引入 html 页面
7 在浏览器地址栏中输入地址后敲回车

POST 请求有哪些：
8 form 标签 method=post
```

```markdown
响应的 HTTP 协议格式
1 响应行
(1) 响应的协议和版本号
(2) 响应状态码
(3) 响应状态描述符
2 响应头
(1) key : value 不同的响应头，有其不同含义
空行
3 响应体 ---->>> 就是回传给客户端的数据
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020061208520939.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
常用的响应码说明
200 表示请求成功
302 表示请求重定向
404 表示请求服务器已经收到了，但是你要的数据不存在（请求地址错误）
500 表示服务器已经收到请求，但是服务器内部错误（代码错误）
```

```markdown
MIME 类型说明 
MIME 是 HTTP 协议中数据类型。
MIME 的英文全称是"Multipurpose Internet Mail Extensions" 
多功能 Internet 邮件扩充服务。MIME 类型的格式是“大类型/小
类型”，并与某一种文件的扩展名相对应。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612085458786.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612085520113.png)
```markdown
谷歌浏览器如何查看 HTTP 协议：
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110002048856.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
火狐浏览器如何查看 HTTP 协议：
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110002118785.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
## 11 Servlet
### 11.1 HttpServletRequest类
```markdown
HttpServletRequest 类有什么作用。
每次只要有请求进入 Tomcat 服务器，Tomcat 服务器就会把
请求过来的HTTP 协议信息解析好封装到 Request 对象中。
然后传递到 service 方法（doGet 和 doPost）中给我们使用。
我们可以通过 HttpServletRequest 对象，获取到所有请求的信息。 

HttpServletRequest 类的常用方法
http://www.baidu.com/project/request
1 getRequestURI() 获取请求的资源路径:project/request
2 getRequestURL() 获取请求的统一资源定位符（绝对路径）:http://www.baidu.com/project/request
3 getRemoteHost() 获取客户端的 ip 地址:www.baidu.com的IP地址
4  getHeader() 获取请求头
5 getParameter() 获取请求的参数
6 getParameterValues() 获取请求的参数（多个值的时候使用）
7 getMethod() 获取请求的方式 GET 或 POST
8 setAttribute(key, value); 设置域数据
9 getAttribute(key); 获取域数据
10 getRequestDispatcher() 获取请求转发对象
```

```java
public class RequestAPIServlet extends HttpServlet {
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,IOException {
		// i.getRequestURI() 获取请求的资源路径
		System.out.println("URI => " + req.getRequestURI());
		// ii.getRequestURL() 获取请求的统一资源定位符（绝对路径）
		System.out.println("URL => " + req.getRequestURL());
		// iii.getRemoteHost() 获取客户端的 ip 地址
		/**
		* 在 IDEA 中，使用 localhost 访问时，得到的客户端 ip 地址是 ===>>> 127.0.0.1<br/>
		* 在 IDEA 中，使用 127.0.0.1 访问时，得到的客户端 ip 地址是 ===>>> 127.0.0.1<br/>
		* 在 IDEA 中，使用 真实 ip 访问时，得到的客户端 ip 地址是 ===>>> 真实的客户端 ip 地址<br/>
		*/
		System.out.println("客户端 ip 地址 => " + req.getRemoteHost());
		// iv.getHeader() 获取请求头
		System.out.println("请求头 User-Agent ==>> " + req.getHeader("User-Agent"));
		// vii.getMethod() 获取请求的方式 GET 或 POST
		System.out.println( "请求的方式 ==>> " + req.getMethod() );
	}
}
```
```markdown
如何获取请求参数
表单：
```


```html
<body>
<form action="http://localhost:8080/07_servlet/parameterServlet" method="get">
	用户名：<input type="text" name="username"><br/>
	密码：<input type="password" name="password"><br/>
	兴趣爱好：<input type="checkbox" name="hobby" value="cpp">C++
	<input type="checkbox" name="hobby" value="java">Java
	<input type="checkbox" name="hobby" value="js">JavaScript<br/>
	<input type="submit">
</form>
</body>
```

```java
public class ParameterServlet extends HttpServlet {
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,IOException {
		// 获取请求参数
		String username = req.getParameter("username");
		String password = req.getParameter("password");
		String[] hobby = req.getParameterValues("hobby");
		System.out.println("用户名：" + username);
		System.out.println("密码：" + password);
		System.out.println("兴趣爱好：" + Arrays.asList(hobby));
	}
}
```
```markdown
doGet 请求的中文乱码解决：
```
```java
// 获取请求参数
String username = req.getParameter("username");
//1 先以 iso8859-1 进行编码
//2 再以 utf-8 进行解码
username = new String(username.getBytes("iso-8859-1"), "UTF-8");
```
```markdown
POST 请求的中文乱码解决
```

```java
@Override
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException,IOException {
	// 设置请求体的字符集为 UTF-8，从而解决 post 请求的中文乱码问题
	req.setCharacterEncoding("UTF-8");
	System.out.println("-------------doPost------------");
	// 获取请求参数
	String username = req.getParameter("username");
	String password = req.getParameter("password");
	String[] hobby = req.getParameterValues("hobby");
	System.out.println("用户名：" + username);
	System.out.println("密码：" + password);
	System.out.println("兴趣爱好：" + Arrays.asList(hobby));
}
```

```markdown
请求的转发
什么是请求的转发?
请求转发是指，服务器收到请求后，从一次资源跳转到另一个
资源的操作叫请求转发。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612091344903.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```java
Servlet1 代码：
public class Servlet1 extends HttpServlet {
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,IOException {
		// 获取请求的参数（办事的材料）查看
		String username = req.getParameter("username");
		System.out.println("在 Servlet1（柜台 1）中查看参数（材料）：" + username);
		// 给材料 盖一个章，并传递到 Servlet2（柜台 2）去查看
		req.setAttribute("key1","柜台 1 的章");
		// 问路：Servlet2（柜台 2）怎么走
		/**
		* 请求转发必须要以斜杠打头，/ 斜杠表示地址为：http://ip:port/工程名/ , 映射到 IDEA 代码的 web 目录
		<br/>
		*
		*/
		RequestDispatcher requestDispatcher = req.getRequestDispatcher("/servlet2");
		// RequestDispatcher requestDispatcher = req.getRequestDispatcher("http://www.baidu.com");
		// 走向 Sevlet2（柜台 2）
		requestDispatcher.forward(req,resp);
	}
}


Servlet2 代码：
public class Servlet2 extends HttpServlet {
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,IOException {
		// 获取请求的参数（办事的材料）查看
		String username = req.getParameter("username");
		System.out.println("在 Servlet2（柜台 2）中查看参数（材料）：" + username);
		// 查看 柜台 1 是否有盖章
		Object key1 = req.getAttribute("key1");
		System.out.println("柜台 1 是否有章：" + key1);
		// 处理自己的业务
		System.out.println("Servlet2 处理自己的业务 ");
}
```
```markdown
base 标签的作用
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612093857516.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```java
<!DOCTYPE html>
<html lang="zh_CN">
	<head>
		<meta charset="UTF-8">
		<title>Title</title>
		<!--base 标签设置页面相对路径工作时参照的地址
		href 属性就是参数的地址值
		-->
		<base href="http://localhost:8080/07_servlet/a/b/">
	</head>
	<body>
		这是 a 下的 b 下的 c.html 页面<br/>
		<a href="../../index.html">跳回首页</a><br/>
	</body>
</html>
```
```markdown
Web 中的相对路径和绝对路径
```

```markdown
在 javaWeb 中，路径分为相对路径和绝对路径两种：
相对路径是：
. 表示当前目录
.. 表示上一级目录
资源名 表示当前目录/资源名
绝对路径：
http://ip:port/工程路径/资源路径
在实际开发中，路径都使用绝对路径，而不简单的使用相对路径。
1 绝对路径
2 base+相对

web 中 / 斜杠的不同意义
在 web 中 / 斜杠 是一种绝对路径。
/ 斜杠 如果被浏览器解析，得到的地址是：http://ip:port/
<a href="/">斜杠</a>
/ 斜杠 如果被服务器解析，得到的地址是：http://ip:port/工程路径
1 <url-pattern>/servlet1</url-pattern>
2 servletContext.getRealPath(“/”);
3 request.getRequestDispatcher(“/”);
特殊情况： response.sendRediect(“/”); 把斜杠发送给浏览器解析。得到 http://ip:port/
```

### 11.2 HttpServletResponse类
```markdown
HttpServletResponse 类的作用

HttpServletResponse 类和 HttpServletRequest 类一样。
每次请求进来，Tomcat 服务器都会创建一个 Response 对
象传递给 Servlet 程序去使用。HttpServletRequest表示请求过来的信息，
HttpServletResponse 表示所有响应的信息，
我们如果需要设置返回给客户端的信息，都可以通过 
HttpServletResponse 对象来进行设置

两个输出流的说明。
字节流 getOutputStream(); 常用于下载（传递二进制数据）
字符流 getWriter(); 常用于回传字符串（常用）
两个流同时只能使用一个。
使用了字节流，就不能再使用字符流，反之亦然，否则就会报错
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612094436894.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
如何往客户端回传数据
要求 ： 往客户端回传 字符串 数据。
```
```java
public class ResponseIOServlet extends HttpServlet {
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,IOException {
		// 要求 ： 往客户端回传 字符串 数据。
		PrintWriter writer = resp.getWriter();
		writer.write("response's content!!!");
	}
}
```

```markdown
响应的乱码解决
解决响应中文乱码方案一（不推荐使用）：
```
```java
// 设置服务器字符集为 UTF-8
resp.setCharacterEncoding("UTF-8");
// 通过响应头，设置浏览器也使用 UTF-8 字符集
resp.setHeader("Content-Type", "text/html; charset=UTF-8");
```
```markdown
解决响应中文乱码方案二（推荐）：
```
```java
// 它会同时设置服务器和客户端都使用 UTF-8 字符集，还设置了响应头
// 此方法一定要在获取流对象之前调用才有效
resp.setContentType("text/html; charset=UTF-8");
```

```markdown
请求重定向
请求重定向，是指客户端给服务器发请求，然后服务器告诉客户端说。
我给你一些地址。你去新地址访问。叫请求
重定向（因为之前的地址可能已经被废弃）。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612094923698.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
请求重定向的第一种方案：
// 设置响应状态码 302 ，表示重定向，（已搬迁）
resp.setStatus(302);
// 设置响应头，说明 新的地址在哪里
resp.setHeader("Location", "http://localhost:8080");
请求重定向的第二种方案（推荐使用）：
resp.sendRedirect("http://localhost:8080");
```
## 12 书城
### 12.1书城项目第一阶段，表单验证
```markdown
验证用户名：必须由字母，数字下划线组成，并且长度为 5 到 12 位
验证密码：必须由字母，数字下划线组成，并且长度为 5 到 12 位
验证确认密码：和密码相同
邮箱验证：xxxxx@xxx.com
验证码：现在只需要验证用户已输入。因为还没讲到服务器。
验证码生成。
```
```markdown
1 新建一个模块
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201111092917781.png#pic_center)
```markdown
2 把书城的静态资源拷贝到 05_book_static 工程下：
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201111092941477.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
3 验证实现如下：
```

```javascript
<script type="text/javascript" src="../../static/script/jquery-1.7.2.js"></script>
<script type="text/javascript">
    // 页面加载完成之后
    $(function () {
        // 给注册绑定单击事件
        $("#sub_btn").click(function () {
            // 验证用户名：必须由字母，数字下划线组成，并且长度为 5 到 12 位
            //1 获取用户名输入框里的内容
            var usernameText = $("#username").val();
            //2 创建正则表达式对象
            var usernamePatt = /^\w{5,12}$/;
            //3 使用 test 方法验证
            if (!usernamePatt.test(usernameText)) {
                //4 提示用户结果
                $("span.errorMsg").text("用户名不合法！");
                return false;
            }
            // 验证密码：必须由字母，数字下划线组成，并且长度为 5 到 12 位
            //1 获取用户名输入框里的内容
            var passwordText = $("#password").val();
            //2 创建正则表达式对象
            var passwordPatt = /^\w{5,12}$/;
            //3 使用 test 方法验证
            if (!passwordPatt.test(passwordText)) {
                //4 提示用户结果
                $("span.errorMsg").text("密码不合法！");
                return false;
            }
            // 验证确认密码：和密码相同
            //1 获取确认密码内容
            var repwdText = $("#repwd").val();
            //2 和密码相比较
            if (repwdText != passwordText) {
                //3 提示用户
                $("span.errorMsg").text("确认密码和密码不一致！");
                return false;
            }
            // 邮箱验证：xxxxx@xxx.com
            //1 获取邮箱里的内容
            var emailText = $("#email").val();
            //2 创建正则表达式对象
            var emailPatt = /^[a-z\d]+(\.[a-z\d]+)*@([\da-z](-[\da-z])?)+(\.{1,2}[a-z]+)+$/;
            //3 使用 test 方法验证是否合法
            if (!emailPatt.test(emailText)) {
                //4 提示用户
                $("span.errorMsg").text("邮箱格式不合法！");
                return false;
            }
            // 验证码：现在只需要验证用户已输入。因为还没讲到服务器。验证码生成。
            var codeText = $("#code").val();
            //去掉验证码前后空格
            alert("去空格前：[" + codeText + "]")
            codeText = $.trim(codeText);
            alert("去空格后：[" + codeText + "]")
            if (codeText == null || codeText == "") {
                //4 提示用户
                $("span.errorMsg").text("验证码不能为空！");
                return false;
            }
            $("span.errorMsg").text("");
        });
    });
</script>
```

### 12.2 搭建书城项目开发环境(第二阶段)
#### 12.2.1 JavaEE 项目的三层架构
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020061209565834.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612095724881.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110081522489.png#pic_center)

#### 12.2.2 先创建书城需要的数据库和表
```java
drop database if exists book;
create database book;
use book;
create table t_user(
	`id` int primary key auto_increment,
	`username` varchar(20) not null unique,
	`password` varchar(32) not null,
	`email` varchar(200)
);
insert into t_user(`username`,`password`,`email`) VALUES('admin','admin','admin@yxj.com');
select * from t_user;
```
#### 12.2.3 编写数据库表对应的 JavaBean 对象
```java
public class User {
	private Integer id;
	private String username;
	private String password;
	private String email;
}
```
#### 12.2.4 编写工具类 JdbcUtils
```java
1 导入需要的 jar 包（数据库和连接池需要）：
druid-1.1.9.jar
mysql-connector-java-5.1.7-bin.jar
以下是测试需要：
hamcrest-core-1.3.jar
junit-4.12.jar

2 在 src 源码目录下编写 jdbc.properties 属性配置文件：
username=root
password=root
url=jdbc:mysql://localhost:3306/book
driverClassName=com.mysql.jdbc.Driver
initialSize=5
maxActive=10

3 编写 JdbcUtils 工具类：
public class JdbcUtils {
    private static DruidDataSource dataSource;
    static {
        try {
            Properties properties = new Properties(); // 读取 jdbc.properties 属性配置文件

            InputStream inputStream =
                    JdbcUtils.class.getClassLoader().getResourceAsStream("jdbc.properties"); //  从流中加载数据

            properties.load(inputStream);  // 创建 数据库连接 池

            dataSource = (DruidDataSource) DruidDataSourceFactory.createDataSource(properties);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    /**
     * 获取数据库连接池中的连接
     * @return 如果返回 null,说明获取连接失败<br />有值就是获取连接成功
     */
    public static Connection getConnection(){
        Connection conn = null;
        try {
            conn = dataSource.getConnection();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return conn;
    }
    /**
     * 关闭连接，放回数据库连接池
     * @param conn
     */
    public static void close(Connection conn){
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}

4 JdbcUtils 测试
public class JdbcUtilsTest {
    @Test
    public void testJdbcUtils(){
        for (int i = 0; i < 100; i++){
            Connection connection = JdbcUtils.getConnection();
            System.out.println(connection);
            JdbcUtils.close(connection);
        }
    }
}
```

#### 12.2.5 编写BaseDao
```java
1 导入 DBUtils 的 jar 包
commons-dbutils-1.3.jar

2 编写 BaseDao：
public abstract class BaseDao {
    //使用 DbUtils 操作数据库
    private QueryRunner queryRunner = new QueryRunner();
    /**
     * update() 方法用来执行：Insert\Update\Delete 语句
     *
     * @return 如果返回-1,说明执行失败<br/>返回其他表示影响的行数
     */
    public int update(String sql, Object... args) {
        Connection connection = JdbcUtils.getConnection();
        try {
            return queryRunner.update(connection, sql, args);
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JdbcUtils.close(connection);
        }
        return -1;
    }
    /**
     * 查询返回一个 javaBean 的 sql 语句
     *
     * @param type 返回的对象类型
     * @param sql 执行的 sql 语句
     * @param args sql 对应的参数值
     * @param <T> 返回的类型的泛型
     * @return
     */
    public <T> T queryForOne(Class<T> type, String sql, Object... args) {
        Connection con = JdbcUtils.getConnection();
        try {
            return queryRunner.query(con, sql, new BeanHandler<T>(type), args);
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {

            JdbcUtils.close(con);
        }
        return null;
    }
    /**
     * 查询返回多个 javaBean 的 sql 语句
     *
     * @param type 返回的对象类型
     * @param sql 执行的 sql 语句
     * @param args sql 对应的参数值
     * @param <T> 返回的类型的泛型
     * @return
     */
    public <T> List<T> queryForList(Class<T> type, String sql, Object... args) {
        Connection con = JdbcUtils.getConnection();
        try {
            return queryRunner.query(con, sql, new BeanListHandler<T>(type), args);
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JdbcUtils.close(con);
        }
        return null;
    }
    /**
     * 执行返回一行一列的 sql 语句
     * @param sql 执行的 sql 语句
     * @param args sql 对应的参数值
     * @return
     */
    public Object queryForSingleValue(String sql, Object... args){
        Connection conn = JdbcUtils.getConnection();
        try {
            return queryRunner.query(conn, sql, new ScalarHandler(), args);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            JdbcUtils.close(conn);
        }
        return null;
    }
}

3 编写 UserDao 和测试
UserDao 接口：
public interface UserDao {
    /**
     * 根据用户名查询用户信息
     * @param username 用户名
     * @return 如果返回 null,说明没有这个用户。反之亦然
     */
    public User queryUserByUsername(String username);
    /**
     * 根据 用户名和密码查询用户信息
     * @param username
     * @param password
     * @return 如果返回 null,说明用户名或密码错误,反之亦然
     */
    public User queryUserByUsernameAndPassword(String username,String password);
    /**
     * 保存用户信息
     * @param user
     * @return 返回-1 表示操作失败，其他是 sql 语句影响的行数
     */
    public int saveUser(User user);
}


4 UserDaoImpl 实现类：
public class UserDaoImpl extends BaseDao implements UserDao {
    @Override
    public User queryUserByUsername(String username) {
        String sql = "select `id`,`username`,`password`,`email` from t_user where username = ?";
        return queryForOne(User.class, sql, username);
    }
    @Override
    public User queryUserByUsernameAndPassword(String username, String password) {
        String sql = "select `id`,`username`,`password`,`email` from t_user where username = ? and
        password = ?";
        return queryForOne(User.class, sql, username,password);
    }
    @Override
    public int saveUser(User user) {
        String sql = "insert into t_user(`username`,`password`,`email`) values(?,?,?)";
        return update(sql, user.getUsername(),user.getPassword(),user.getEmail());
    }
}


5 UserDao 测试：
public class UserDaoTest {
    UserDao userDao = new UserDaoImpl();
    @Test
    public void queryUserByUsername() {
        if (userDao.queryUserByUsername("admin1234") == null ){
            System.out.println("用户名可用！");
        } else {
            System.out.println("用户名已存在！");
        }
    }
    @Test
    public void queryUserByUsernameAndPassword() {
        if ( userDao.queryUserByUsernameAndPassword("admin","admin1234") == null) {
            System.out.println("用户名或密码错误，登录失败");
        } else {
            System.out.println("查询成功");
        }
    }
    @Test
    public void saveUser() {
        System.out.println( userDao.saveUser(new User(null,"wzg168", "123456", "wzg168@qq.com")) );
    }
}
```

#### 12.2.6 编写UserService和测试
```java
1 UserService 接口：
public interface UserService {
    /**
     * 注册用户
     * @param user
     */
    public void registUser(User user);
    /**
     * 登录
     * @param user
     * @return 如果返回 null，说明登录失败，返回有值，是登录成功
     */
    public User login(User user);
    /**
     * 检查 用户名是否可用
     * @param username
     * @return 返回 true 表示用户名已存在，返回 false 表示用户名可用
     */
    public boolean existsUsername(String username);
}

2 UserServiceImpl 实现类：
public class UserServiceTest {
    UserService userService = new UserServiceImpl();
    @Test
    public void registUser() {
        userService.registUser(new User(null, "bbj168", "666666", "bbj168@qq.com"));
        userService.registUser(new User(null, "abc168", "666666", "abc168@qq.com"));
    }
    @Test
    public void login() {
        System.out.println( userService.login(new User(null, "wzg168", "123456", null)) );
    }
    @Test
    public void existsUsername() {
        if (userService.existsUsername("wzg16888")) {
            System.out.println("用户名已存在！");
        } else {
            System.out.println("用户名可用！");
        }
    }
}

3 UserService 测试
public class UserServiceTest {
    UserService userService = new UserServiceImpl();
    @Test
    public void registUser() {
        userService.registUser(new User(null, "bbj168", "666666", "bbj168@qq.com"));
        userService.registUser(new User(null, "abc168", "666666", "abc168@qq.com"));
    }
    @Test
    public void login() {
        System.out.println( userService.login(new User(null, "wzg168", "123456", null)) );
    }
    @Test
    public void existsUsername() {
        if (userService.existsUsername("wzg16888")) {
            System.out.println("用户名已存在！");
        } else {
            System.out.println("用户名可用！");
        }
    }
}
```


#### 12.2.7 编写web层
```markdown
1 实现用户注册的功能
 图解用户注册的流程：
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110082753662.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
2 修改 regist.html 和 regist_success.html 页面
1 添加 base 标签
<!--写 base 标签，永远固定相对路径跳转的结果-->
<base href="http://localhost:8080/book/">
2 修改 base 标签对页面中所有相对路径的影响（浏览器 F12，哪个报红，改哪个）
以下是几个修改的示例：
<link type="text/css" rel="stylesheet" href="static/css/style.css" >
<script type="text/javascript" src="static/script/jquery-1.7.2.js"></script>
3 修改注册表单的提交地址和请求方式
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110082848501.png#pic_center)

```java
3 编写 RegistServlet 程序
public class RegistServlet extends HttpServlet {
    private UserService userService = new UserServiceImpl();
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
            IOException {
// 1、获取请求的参数
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String email = req.getParameter("email");
        String code = req.getParameter("code");
// 2、检查 验证码是否正确 === 写死,要求验证码为:abcde
        if ("abcde".equalsIgnoreCase(code)) {
// 3、检查 用户名是否可用
            if (userService.existsUsername(username)) {
                System.out.println("用户名[" + username + "]已存在!");
// 跳回注册页面
                req.getRequestDispatcher("/pages/user/regist.html").forward(req, resp);
            } else {
// 可用
// 调用 Sservice 保存到数据库
                userService.registUser(new User(null, username, password, email));
// 跳到注册成功页面 regist_success.html
                req.getRequestDispatcher("/pages/user/regist_success.html").forward(req, resp);
            }
        } else {
            System.out.println("验证码[" + code + "]错误");
            req.getRequestDispatcher("/pages/user/regist.html").forward(req, resp);
        }
    }
}
```
### 12.3 IDEA中Debug调试的使用
#### 12.3.1 Debug调试代码，首先需要两个元素：断点 + Debug启动服务器
```markdown
1 断点，只需要在代码需要停的行的左边上单击，就可以添加和取消
2 Debug 启动 Tomcat 运行代码：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110083945480.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

#### 12.3.2 测试工具栏
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612113718656.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612113729555.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 12.3.3 变量窗口
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612113910592.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 12.3.4 方法调用栈窗口
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612114122463.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 12.3.5 其他常用调试相关按钮
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612114408914.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 12.4 用户登录功能的实现
#### 12.4.1 图解用户登录
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110084253233.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
#### 12.4.2 修改 login.html页面和 login_success.html页面
```markdown
1 添加 base 标签
<!--写 base 标签，永远固定相对路径跳转的结果-->
<base href="http://localhost:8080/book/">
2 修改 base 标签对页面中所有相对路径的影响（浏览器 F12，哪个报红，改哪个）
以下是几个修改的示例：
<link type="text/css" rel="stylesheet" href="static/css/style.css" >
<script type="text/javascript" src="static/script/jquery-1.7.2.js"></script>
3 修改 login.html 表单的提交地址和请求方式
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/202011100843507.png#pic_center)
#### 12.4.3 LoginServlet程序
```java
public class LoginServlet extends HttpServlet {
    private UserService userService = new UserServiceImpl();
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
            IOException {
// 1、获取请求的参数
        String username = req.getParameter("username");
        String password = req.getParameter("password");
// 调用 userService.login()登录处理业务
        User loginUser = userService.login(new User(null, username, password, null));
// 如果等于 null,说明登录 失败!
        if (loginUser == null) {
// 跳回登录页面
            req.getRequestDispatcher("/pages/user/login.html").forward(req, resp);
        } else {
// 登录 成功
//跳到成功页面 login_success.html
            req.getRequestDispatcher("/pages/user/login_success.html").forward(req, resp);
        }
    }
}
```

## 13 jsp
### 13.1 jsp技术

```markdown
jsp 的全换是 java server pages。Java 的服务器页面。
jsp 的主要作用是代替 Servlet 程序回传 html 页面的数据。
因为 Servlet 程序回传 html 页面数据是一件非常繁锁的事情。
开发成本和维护成本都极高。
```
```markdown
Servlet 回传 html 页面数据的代码
```

```java
public class PringHtml extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
            IOException {
// 通过响应的回传流回传 html 页面数据
        resp.setContentType("text/html; charset=UTF-8");
        PrintWriter writer = resp.getWriter();
        writer.write("<!DOCTYPE html>\r\n");
        writer.write(" <html lang=\"en\">\r\n");
        writer.write(" <head>\r\n");
        writer.write(" <meta charset=\"UTF-8\">\r\n");
        writer.write(" <title>Title</title>\r\n");
        writer.write(" </head>\r\n");
        writer.write(" <body>\r\n");
        writer.write(" 这是 html 页面数据 \r\n");
        writer.write(" </body>\r\n");
        writer.write("</html>\r\n");
        writer.write("\r\n");
    }
}
```
```markdown
jsp 回传一个简单 html 页面的代码
```

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
这是 html 页面数据
</body>
</html>
```
```markdown
jsp 的小结：
1 如何创建 jsp 的页面?
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110085653213.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
输入文件名敲回车即可！！
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020111008570928.png#pic_center)
```markdown
jsp 如何访问：
jsp 页面和 html 页面一样，都是存放在 web 目录下。访问也跟访问 html 页面一样。
比如：
在 web 目录下有如下的文件：
web 目录
a.html 页面 访问地址是 =======>>>>>> http://ip:port/工程路径/a.html
b.jsp 页面 访问地址是 =======>>>>>> http://ip:port/工程路径/b.jsp
```


### 13.2.jsp的本质
```markdown
jsp 页面本质上是一个 Servlet 程序。
当我们第一次访问 jsp 页面的时候。Tomcat 服务器会帮我们把 
jsp 页面翻译成为一个 java 源文件。并且对它进行编译成
为.class 字节码程序。我们打开 java 源文件不难发现其里面的
内容是：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110085818391.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
我们跟踪原代码发现，HttpJspBase 类。它直接地继
承了HttpServlet 类。也就是说。jsp 翻译出来的 java 类，
它间接了继承了 HttpServlet 类。也就是说，翻译出来的是一个 
Servlet 程序
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110085855911.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
总结：通过翻译的 java 源代码我们就可以得到结果：jsp 就是 
Servlet 程序。大家也可以去观察翻译出来的 Servlet 程序的源
代码，不难发现。其底层实现，也是通过输出流。把 html 页面数
据回传
给客户端。
```
```java
public void _jspService(final javax.servlet.http.HttpServletRequest request,final javax.servlet.http.HttpServletResponse response)throws java.io.IOException,javax.servlet.ServletException{

final java.lang.String _jspx_method=request.getMethod();
        if(!"GET".equals(_jspx_method)&&!"POST".equals(_jspx_method)&&!"HEAD".equals(_jspx_method)
        &&!javax.servlet.DispatcherType.ERROR.equals(request.getDispatcherType())){
        response.sendError(HttpServletResponse.SC_METHOD_NOT_ALLOWED,"JSPs only permit GET POST or
        HEAD");
        return;
        }
final javax.servlet.jsp.PageContext pageContext;
        javax.servlet.http.HttpSession session=null;
final javax.servlet.ServletContext application;
final javax.servlet.ServletConfig config;
        javax.servlet.jsp.JspWriter out=null;
final java.lang.Object page=this;
        javax.servlet.jsp.JspWriter _jspx_out=null;
        javax.servlet.jsp.PageContext _jspx_page_context=null;
        try{
        response.setContentType("text/html;charset=UTF-8");
        pageContext=_jspxFactory.getPageContext(this,request,response,
        null,true,8192,true);
        _jspx_page_context=pageContext;
        application=pageContext.getServletContext();
        config=pageContext.getServletConfig();
        session=pageContext.getSession();
        out=pageContext.getOut();
        _jspx_out=out;
        out.write("\r\n");
        out.write("\r\n");
        out.write("<html>\r\n");
        out.write("<head>\r\n");
        out.write(" <title>Title</title>\r\n");
        out.write("</head>\r\n");
        out.write("<body>\r\n");
        out.write(" a.jsp 页面\r\n");
        out.write("</body>\r\n");
        out.write("</html>\r\n");
        }catch(java.lang.Throwable t){
        if(!(t instanceof javax.servlet.jsp.SkipPageException)){
        out=_jspx_out;
        if(out!=null&&out.getBufferSize()!=0)
        try{
        if(response.isCommitted()){
        out.flush();
        }else{
        out.clearBuffer();
        }
        }catch(java.io.IOException e){}
        if(_jspx_page_context!=null)_jspx_page_context.handlePageException(t);
        else throw new ServletException(t);
        }
        }finally{
        _jspxFactory.releasePageContext(_jspx_page_context);
        }
}
```

### 13.3 jsp的三种语法
```markdown
jsp 头部的 page 指令
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612134431801.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612134631960.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612134649221.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
jsp 中的常用脚本
```

```markdown
声明脚本(极少使用)
声明脚本的格式是： <%! 声明 java 代码 %>
作用：可以给 jsp 翻译出来的 java 类定义属性和方法甚至是静态代
码块。内部类等。
练习：
1 声明类属性
2 声明 static 静态代码块
3 声明类方法
4 声明内部类
```

```html
<%--1 声明类属性--%>
<%!
private Integer id;
private String name;
private static Map<String,Object> map;
%>
<%--2 声明 static 静态代码块--%>
<%!
static {
map = new HashMap<String,Object>();
map.put("key1", "value1");
map.put("key2", "value2");
map.put("key3", "value3");
}
%>
<%--3 声明类方法--%>
<%!
public int abc(){
return 12;
}
%>
<%--4 声明内部类--%>
<%!
public static class A {
private Integer id = 12;
private String abc = "abc";
}
%>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612134959547.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
表达式脚本（常用）
表达式脚本的格式是：<%=表达式%>
表达式脚本的作用是：的 jsp 页面上输出数据。
表达式脚本的特点：
1 所有的表达式脚本都会被翻译到_jspService() 方法中
2 表达式脚本都会被翻译成为 out.print()输出到页面上
3 由于表达式脚本翻译的内容都在_jspService() 方法中,所以_jspService()方法中的对象都可以直接使用。
4 表达式脚本中的表达式不能以分号结束。
练习：
1 输出整型
2 输出浮点型
3 输出字符串
4 输出对象
```

```html
<%=12 %> <br>
<%=12.12 %> <br>
<%="我是字符串" %> <br>
<%=map%> <br>
<%=request.getParameter("username")%>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612135231940.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```html
代码脚本
代码脚本的格式是：
<%
java 语句
%>
代码脚本的作用是：可以在 jsp 页面中，编写我们自己需要的功能（写的是 java 语句）。
代码脚本的特点是：
1 代码脚本翻译之后都在_jspService 方法中
2 代码脚本由于翻译到_jspService()方法中，所以在_jspService()方法中的现有对象都可以直接使用。
3 还可以由多个代码脚本块组合完成一个完整的 java 语句。
4 代码脚本还可以和表达式脚本一起组合使用，在 jsp 页面上输出数据
练习：
1 代码脚本----if 语句
2 代码脚本----for 循环语句
3 翻译后 java 文件中_jspService 方法内的代码都可以写
```

```html
<%--练习：--%>
<%--1.代码脚本----if 语句--%>
<%
int i = 13 ;
if (i == 12) {
%>
<h1>国哥好帅</h1>
<%
} else {
%>
<h1>国哥又骗人了！</h1>
<%
}
%>
<br>
<%--2.代码脚本----for 循环语句--%>
<table border="1" cellspacing="0">
<%
for (int j = 0; j < 10; j++) {
%>
<tr>
<td>第 <%=j + 1%>行</td>
</tr>
<%
}
%>
</table>
<%--3.翻译后 java 文件中_jspService 方法内的代码都可以写--%>
<%
String username = request.getParameter("username");
System.out.println("用户名的请求参数值是：" + username);
%>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612140130652.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
jsp 中的三种注释
html 注释
<!-- 这是 html 注释 -->
html 注释会被翻译到 java 源代码中。在_jspService 方法里，以 out.writer 输出到客户端。

java 注释
<%
// 单行 java 注释
/* 多行 java 注释 */
%>
java 注释会被翻译到 java 源代码中。

jsp 注释
<%-- 这是 jsp 注释 --%>
jsp 注释可以注掉，jsp 页面中所有代码。
```
### 13.4 jsp九大内置对象
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612140711244.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 13.5 jsp四大域对象
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612140810352.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
scope.jsp 页面
```

```html
<body>
<h1>scope.jsp 页面</h1>
<%
// 往四个域中都分别保存了数据
pageContext.setAttribute("key", "pageContext");
request.setAttribute("key", "request");
session.setAttribute("key", "session");
application.setAttribute("key", "application");
%>
pageContext 域是否有值：<%=pageContext.getAttribute("key")%> <br>
request 域是否有值：<%=request.getAttribute("key")%> <br>
session 域是否有值：<%=session.getAttribute("key")%> <br>
application 域是否有值：<%=application.getAttribute("key")%> <br>
<%
request.getRequestDispatcher("/scope2.jsp").forward(request,response);
%>
</body>
```
```markdown
scope2.jsp 页面
```

```html
<body>
<h1>scope2.jsp 页面</h1>
pageContext 域是否有值：<%=pageContext.getAttribute("key")%> <br>
request 域是否有值：<%=request.getAttribute("key")%> <br>
session 域是否有值：<%=session.getAttribute("key")%> <br>
application 域是否有值：<%=application.getAttribute("key")%> <br>
</body>
```

### 13.6 jsp中的out输出和 response.getWriter 输出的区别
```markdown
response 中表示响应，我们经常用于设置返回给客户端的内容（输出）
out 也是给用户做输出使用的。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612144234928.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
out.write() 输出字符串没有问题
out.print() 输出任意数据都没有问题（都转换成为字符串后调用的 write 输出）
深入源码，浅出结论：在 jsp 页面中，可以统一使用 out.print()来进行输出
```
### 13.7 jsp的常用标签
#### 13.7.1 jsp静态包含

```html
<%--
<%@ include file=""%> 就是静态包含
file 属性指定你要包含的 jsp 页面的路径
地址中第一个斜杠 / 表示为 http://ip:port/工程路径/ 映射到代码的 web 目录
静态包含的特点：
1、静态包含不会翻译被包含的 jsp 页面。
2、静态包含其实是把被包含的 jsp 页面的代码拷贝到包含的位置执行输出。
--%>
<%@ include file="/include/footer.jsp"%>
```
#### 13.7.2 jsp动态包含
```html
<%--
<jsp:include page=""></jsp:include> 这是动态包含
page 属性是指定你要包含的 jsp 页面的路径
动态包含也可以像静态包含一样。把被包含的内容执行输出到包含位置
动态包含的特点：
1、动态包含会把被包含的 jsp 页面也翻译成为 java 代码
2、动态包含底层代码使用如下代码去调用被包含的 jsp 页面执行输出。
JspRuntimeLibrary.include(request, response, "/include/footer.jsp", out, false);
3、动态包含，还可以传递参数
--%>
<jsp:include page="/include/footer.jsp">
<jsp:param name="username" value="bbj"/>
<jsp:param name="password" value="root"/>
</jsp:include>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612145459118.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 13.7.3 jsp标签-转发
```html
<%--
<jsp:forward page=""></jsp:forward> 是请求转发标签，它的功能就是请求转发
page 属性设置请求转发的路径
--%>
<jsp:forward page="/scope2.jsp"></jsp:forward>
```
### 13.8 jsp的练习题
```markdown
练习一 在 jsp 页面中输出九九乘法口诀表
```

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <style type="text/css">
        table{
            width: 650px;
        }
    </style>
</head>
<body>
<%-- 练习一：在 jsp 页面中输出九九乘法口诀表 --%>
<h1 align="center">九九乘法口诀表</h1>
<table align="center">
    <%-- 外层循环遍历行 --%>
    <% for (int i = 1; i <= 9; i++) { %>
    <tr>
        <%-- 内层循环遍历单元格 --%>
        <% for (int j = 1; j <= i ; j++) { %>
        <td><%=j + "x" + i + "=" + (i*j)%></td>
        <% } %>
    </tr>
    <% } %>
</table>
</body>
</html>
```
```markdown
练习二 jsp 输出一个表格，里面有 10 个学生信息。
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110091002879.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
Student 类：
```

```java
public class Student {
    private Integer id;
    private String name;
    private Integer age;
    private String phone;
}
```
```markdown
SearchStudentServlet 程序
```

```java
public class SearchStudentServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
            IOException {
// 获取请求的参数
// 发 sql 语句查询学生的信息
// 使用 for 循环生成查询到的数据做模拟
        List<Student> studentList = new ArrayList<Student>();
        for (int i = 0; i < 10; i++) {
            int t = i + 1;
            studentList.add(new Student(t,"name"+t, 18+t,"phone"+t));
        }
// 保存查询到的结果（学生信息）到 request 域中
        req.setAttribute("stuList", studentList);
// 请求转发到 showStudent.jsp 页面
        req.getRequestDispatcher("/test/showStudent.jsp").forward(req,resp);
    }
}
```
```markdown
showStudent.jsp 页面
```


```html
<%@ page import="java.util.List" %>
<%@ page import="com.yxj.pojo.Student" %>
<%@ page import="java.util.ArrayList" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <style>
        table{
            border: 1px blue solid;
            width: 600px;
            border-collapse: collapse;
        }
        td,th{
            border: 1px blue solid;
        }
    </style>
</head>
<body>
<%--练习二：jsp 输出一个表格，里面有 10 个学生信息。--%>
<%
List<Student> studentList = (List<Student>) request.getAttribute("stuList");
    %>
    <table>
        <tr>
            <td>编号</td>
            <td>姓名</td>
            <td>年龄</td>
            <td>电话</td>
            <td>操作</td>
        </tr>
        <% for (Student student : studentList) { %>
        <tr>
            <td><%=student.getId()%></td>
            <td><%=student.getName()%></td>
            <td><%=student.getAge()%></td>
            <td><%=student.getPhone()%></td>
            <td>删除、修改</td>
        </tr>
        <% } %>
    </table>
</body>
</html>
```
### 13.9 Listener
#### 13.9.1 Listener监听器
```markdown
1 Listener 监听器它是 JavaWeb 的三大组件之一。JavaWeb 的三
大组件分别是：Servlet 程序、Filter 过滤器、Listener 监
听器。
2 Listener 它是 JavaEE 的规范，就是接口
3 监听器的作用是，监听某种事物的变化。然后通过回调函数，
反馈给客户（程序）去做一些相应的处理。
```
#### 13.9.2 ServletContextListener监听器
```markdown
ServletContextListener 它可以监听 ServletContext 对象的创建和销毁。
ServletContext 对象在 web 工程启动的时候创建，在 web 工程停止的时候销毁。
监听到创建和销毁之后都会分别调用 ServletContextListener 监听器的方法反馈。
```
```markdown
两个方法分别是
```
```java
public interface ServletContextListener extends EventListener {
    /**
     * 在 ServletContext 对象创建之后马上调用，做初始化
     */
    public void contextInitialized(ServletContextEvent sce);
    /**
     * 在 ServletContext 对象销毁之后调用
     */
    public void contextDestroyed(ServletContextEvent sce);
}
```

```markdown
如何使用 ServletContextListener 监听器监听 ServletContext 对象。
使用步骤如下：
1 编写一个类去实现 ServletContextListener
2 实现其两个回调方法
3 到 web.xml 中去配置监听器
```
```markdown
监听器实现类
```

```java
public class MyServletContextListenerImpl implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("ServletContext 对象被创建了");
    }
    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("ServletContext 对象被销毁了");
    }
}
```
```markdown
web.xml 中的配置
```

```xml
<!--配置监听器-->
<listener>
    <listener-class>com.yxj.listener.MyServletContextListenerImpl</listener-class>
</listener>	
```
## 14 EL 表达式
### 14.1 EL表达式
```markdown
EL 表达式的全称是：Expression Language。是表达式语言。
EL 表达式的什么作用：EL 表达式主要是代替 jsp 页面中的表达式
脚本在 jsp 页面中进行数据的输出。
因为 EL 表达式在输出数据的时候，要比jsp的表达式脚本要简洁很多。
```
```html
<body>
<%
request.setAttribute("key","值");
%>
表达式脚本输出 key 的值是：
<%=request.getAttribute("key1")==null?"":request.getAttribute("key1")%><br/>
EL 表达式输出 key 的值是：${key1}
</body>

EL 表达式的格式是：${表达式}
EL 表达式在输出 null 值的时候，输出的是空串。jsp 表达式脚本输出 null 值的时候，输出的是 null 字符串。
```
```markdown
EL 表达式搜索域数据的顺序
```

```markdown
EL 表达式主要是在 jsp 页面中输出数据。
主要是输出域对象中的数据。
当四个域中都有相同的 key 的数据的时候，EL 表达式会按照四个域的从小到大的顺序去进行搜索，找到就输出。
```
```html
<body>
<%
//往四个域中都保存了相同的 key 的数据。
request.setAttribute("key", "request"); 
session.setAttribute("key", "session");
application.setAttribute("key", "application");
pageContext.setAttribute("key", "pageContext");
%>
${ key }

<!--如果把request.setAttribute("key", "request");注释掉,就得到session 
如果把session.setAttribute("key", "session");注释掉,得到session,因为此时浏览器并没有关闭
如果把application.setAttribute("key", "application");注释掉,得到application,因为web程序没有关闭
pageContext.setAttribute("key", "pageContext"); -->
</body>
```
```markdown
EL 表达式输出 Bean 的普通属性，数组属性。List 集
合属性，map 集合属性
```

```markdown
需求——输出 Person 类中普通属性，数组属性。list 集合属性和 
map 集合属性。
```
```markdown
Person 类
```

```java
public class Person {
// i.需求——输出 Person 类中普通属性，数组属性。list 集合属性和 map 集合属性。
private String name;
private String[] phones;
private List<String> cities;
private Map<String,Object> map;
public int getAge() {
return 18;
}
```
```markdown
输出的代码
```

```html
<body>
<%
Person person = new Person();
person.setName("国哥好帅！");
person.setPhones(new String[]{"18610541354","18688886666","18699998888"});
List<String> cities = new ArrayList<String>();
    cities.add("北京");
    cities.add("上海");
    cities.add("深圳");
    person.setCities(cities);
    Map<String,Object>map = new HashMap<>();
    map.put("key1","value1");
    map.put("key2","value2");
    map.put("key3","value3");
    person.setMap(map);
    pageContext.setAttribute("p", person);
    %>
    输出 Person：${ p }<br/>
    输出 Person 的 name 属性：${p.name} <br>
    输出 Person 的 pnones 数组属性值：${p.phones[2]} <br>
    输出 Person 的 cities 集合中的元素值：${p.cities} <br>
    输出 Person 的 List 集合中个别元素值：${p.cities[2]} <br>
    输出 Person 的 Map 集合: ${p.map} <br>
    输出 Person 的 Map 集合中某个 key 的值: ${p.map.key3} <br>
    输出 Person 的 age 属性：${p.age} <br>
</body>
```

```markdown
EL 表达式——运算
语法：${ 运算表达式 } ， EL 表达式支持如下运算符：
```
```markdown
关系运算
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020111009225324.png#pic_center)
```markdown
逻辑运算
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110092314532.png#pic_center)
```markdown
算数运算
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/2020111009235076.png#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110092340751.png#pic_center)
```markdown
empty 运算
```

```markdown
empty 运算可以判断一个数据是否为空，如果为空，则输出 true,
不为空输出 false。
以下几种情况为空：
1 值为 null 值的时候，为空
2 值为空串的时候，为空
3 值是 Object 类型数组，长度为零的时候
4 list 集合，元素个数为零
5 map 集合，元素个数为零
```

```html
<body>
<%
// 1 值为 null 值的时候，为空
request.setAttribute("emptyNull", null);
// 2 值为空串的时候，为空
request.setAttribute("emptyStr", "");
// 3 值是 Object 类型数组，长度为零的时候
request.setAttribute("emptyArr", new Object[]{});
// 4 list 集合，元素个数为零
List<String> list = new ArrayList<>();
    // list.add("abc");
    request.setAttribute("emptyList", list);
    // 5 map 集合，元素个数为零
    Map<String,Object> map = new HashMap<String, Object>();
    // map.put("key1", "value1");
    request.setAttribute("emptyMap", map);
    %>
    ${ empty emptyNull } <br/>
    ${ empty emptyStr } <br/>
    ${ empty emptyArr } <br/>
    ${ empty emptyList } <br/>
    ${ empty emptyMap } <br/>
</body>
```


```markdown
三元运算

表达式 1？表达式 2：表达式 3
如果表达式 1 的值为真，返回表达式 2 的值，如果表达式 1 的值为假，返回表达式 3 的值
示例：
${ 12 != 12 ? "国哥帅呆":"国哥又骗人啦" }
```

```markdown
“.”点运算 和 [] 中括号运算符

.点运算，可以输出 Bean 对象中某个属性的值。
[]中括号运算，可以输出有序集合中某个元素的值。
并且[]中括号运算，还可以输出 map 集合中 key 里含有特殊字符的 key 的值。

```
```html
<body>
<%
Map<String,Object> map = new HashMap<String, Object>();
map.put("a.a.a", "aaaValue");
map.put("b+b+b", "bbbValue");
map.put("c-c-c", "cccValue");
request.setAttribute("map", map);
%>
${ map['a.a.a'] } <br>
${ map["b+b+b"] } <br>
${ map['c-c-c'] } <br>
</body>
```

```html
<body>
<%
// 1、值为 null 值的时候，为空
request.setAttribute("emptyNull", null);
// 2、值为空串的时候，为空
request.setAttribute("emptyStr", "");
// 3、值是 Object 类型数组，长度为零的时候
request.setAttribute("emptyArr", new Object[]{});
// 4、list 集合，元素个数为零
List<String> list = new ArrayList<>();
    // list.add("abc");
    request.setAttribute("emptyList", list);
    // 5、map 集合，元素个数为零
    Map<String,Object> map = new HashMap<String, Object>();
    // map.put("key1", "value1");
    request.setAttribute("emptyMap", map);
    %>
    ${ empty emptyNull } <br/>
    ${ empty emptyStr } <br/>
    ${ empty emptyArr } <br/>
    ${ empty emptyList } <br/>
    ${ empty emptyMap } <br/>
</body>
```

```markdown
EL 表达式的 11 个隐含对象
EL 个达式中 11 个隐含对象，是 EL 表达式中自己定义的，
可以直接使用
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020111009270749.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110092717824.png#pic_center)
```markdown
EL 获取四个特定域中的属性
```


```markdown
pageScope ====== pageContext 域
requestScope ====== Request 域
sessionScope ====== Session 域
applicationScope ====== ServletContext 域
```
```html
<body>
<%
pageContext.setAttribute("key1", "pageContext1");
pageContext.setAttribute("key2", "pageContext2");
request.setAttribute("key2", "request");
session.setAttribute("key2", "session");
application.setAttribute("key2", "application");
%>
${ applicationScope.key2 }
</body>
```
```markdown
pageContext 对象的使用
```

```markdown
1 协议
2 服务器 ip
3 服务器端口
4 获取工程路径
5 获取请求方法
6 获取客户端 ip 地址
7 获取会话的 id 编号
```
```html
<body>
<%--
request.getScheme() 它可以获取请求的协议
request.getServerName() 获取请求的服务器 ip 或域名
request.getServerPort() 获取请求的服务器端口号
getContextPath() 获取当前工程路径
request.getMethod() 获取请求的方式（GET 或 POST）
request.getRemoteHost() 获取客户端的 ip 地址
session.getId() 获取会话的唯一标识
--%>
<%
pageContext.setAttribute("req", request);
%>
<%=request.getScheme() %> <br>

---------------------------------------------------------------------------------------------------
1 协议： ${ req.scheme }<br>
2 服务器 ip：${ pageContext.request.serverName }<br>
3 服务器端口：${ pageContext.request.serverPort }<br>
4 获取工程路径：${ pageContext.request.contextPath }<br>
5 获取请求方法：${ pageContext.request.method }<br>
6 获取客户端 ip 地址：${ pageContext.request.remoteHost }<br>
7.获取会话的 id 编号：${ pageContext.session.id }<br>
</body>
```
```markdown
EL 表达式其他隐含对象的使用
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110092954669.png#pic_center)
```markdown
示例代码
```

```html
输出请求参数 username 的值：${ param.username } <br>
输出请求参数 password 的值：${ param.password } <br>
输出请求参数 username 的值：${ paramValues.username[0] } <br>
输出请求参数 hobby 的值：${ paramValues.hobby[0] } <br>
输出请求参数 hobby 的值：${ paramValues.hobby[1] } <br>
```
### 14.2 JSTL 标签库
```markdown
JSTL 标签库 全称是指 JSP Standard Tag Library JSP 标准标签库。是一个不断完善的开放源代码的 JSP 标
签库。
EL 表达式主要是为了替换 jsp 中的表达式脚本，而标签库则是为了替换代码脚本。这样使得整个 jsp 页面
变得更佳简洁。
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612155143152.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```html
在 jsp 标签库中使用 taglib 指令引入标签库
CORE 标签库
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
XML 标签库
<%@ taglib prefix="x" uri="http://java.sun.com/jsp/jstl/xml" %>
FMT 标签库
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
SQL 标签库
<%@ taglib prefix="sql" uri="http://java.sun.com/jsp/jstl/sql" %>
FUNCTIONS 标签库
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
```
```markdown
JSTL 标签库的使用步骤
```

```markdown
1 先导入 jstl 标签库的 jar 包。
taglibs-standard-impl-1.2.1.jar
taglibs-standard-spec-1.2.1.jar
2 第二步，使用 taglib 指令引入标签库。
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

```markdown
core 核心库使用
<c:set />（使用很少）
作用：set 标签可以往域中保存数据
```

```html
<%--
<c:set />
作用：set 标签可以往域中保存数据
域对象.setAttribute(key,value);
scope 属性设置保存到哪个域
page 表示 PageContext 域（默认值）
request 表示 Request 域
session 表示 Session 域
application 表示 ServletContext 域
var 属性设置 key 是多少
value 属性设置值
--%>
保存之前：${ sessionScope.abc } <br>
<c:set scope="session" var="abc" value="abcValue"/>
保存之后：${ sessionScope.abc } <br>
```

```markdown
<c:if />
if 标签用来做 if 判断。
```

```html
<%--
ii.<c:if />
if 标签用来做 if 判断。
test 属性表示判断的条件（使用 EL 表达式输出）
--%>
<c:if test="${ 12 == 12 }">
<h1>12 等于 12</h1>
</c:if>
<c:if test="${ 12 != 12 }">
<h1>12 不等于 12</h1>
</c:if>
```

```markdown
<c:choose> <c:when> <c:otherwise>标签
作用：多路判断。跟 switch ... case .... default 非常接近
```
```html
<%--
<c:choose> <c:when> <c:otherwise>标签
    作用：多路判断。跟 switch ... case .... default 非常接近
    choose 标签开始选择判断
    when 标签表示每一种判断情况
    test 属性表示当前这种判断情况的值
    otherwise 标签表示剩下的情况
    <c:choose> <c:when> <c:otherwise>标签使用时需要注意的点：
        1 标签里不能使用 html 注释，要使用 jsp 注释
        2 when 标签的父标签一定要是 choose 标签
        --%>
        <%
        request.setAttribute("height", 180);
        %>
        <c:choose>
            <%-- 这是 html 注释 --%>
            <c:when test="${ requestScope.height > 190 }">
                <h2>小巨人</h2>
            </c:when>
            <c:when test="${ requestScope.height > 180 }">
                <h2>很高</h2>
            </c:when>
            <c:when test="${ requestScope.height > 170 }">
                <h2>还可以</h2>
            </c:when>
            <c:otherwise>
                <c:choose>
                    <c:when test="${requestScope.height > 160}">
                        <h3>大于 160</h3>
                    </c:when>
                    <c:when test="${requestScope.height > 150}">
                        <h3>大于 150</h3>
                    </c:when>
                    <c:when test="${requestScope.height > 140}">
                        <h3>大于 140</h3>
                    </c:when>
                    <c:otherwise>
                        其他小于 140
                    </c:otherwise>
                </c:choose>
            </c:otherwise>
</c:choose>
```

```markdown
<c:forEach />
作用：遍历输出使用。
```

```html
遍历 1 到 10，输出
示例代码：
<%--1.遍历 1 到 10，输出
begin 属性设置开始的索引
end 属性设置结束的索引
var 属性表示循环的变量(也是当前正在遍历到的数据)
for (int i = 1; i < 10; i++)
--%>
<table border="1">
    <c:forEach begin="1" end="10" var="i">
    <tr>
    <td>第${i}行</td>
    </tr>
    </c:forEach>
</table>
```

```html
遍历 Object 数组
示例代码
<%-- 2.遍历 Object 数组
for (Object item: arr)
items 表示遍历的数据源（遍历的集合）
var 表示当前遍历到的数据
--%>
<%
request.setAttribute("arr", new String[]{"18610541354","18688886666","18699998888"});
%>
<c:forEach items="${ requestScope.arr }" var="item">
${ item } <br>
</c:forEach>
```

```html
遍历 Map 集合
示例代码
<%
Map<String,Object> map = new HashMap<String, Object>();
map.put("key1", "value1");
map.put("key2", "value2");
map.put("key3", "value3");
// for ( Map.Entry<String,Object> entry : map.entrySet()) {
// }
request.setAttribute("map", map);
%>
<c:forEach items="${ requestScope.map }" var="entry">
<h1>${entry.key} = ${entry.value}</h1>
</c:forEach>
```
```markdown
遍历 List 集合---list 中存放 Student 类，有属性：编号，用户名，密码，年龄，电话信息
Student 类：
```
```java
public class Student {
    //4.编号，用户名，密码，年龄，电话信息
    private Integer id;
    private String username;
    private String password;
    private Integer age;
    private String phone;

}
```

```html
示例代码：
<%--4.遍历 List 集合---list 中存放 Student 类，有属性：编号，用户名，密码，年龄，电话信息--%>
<%
List<Student> studentList = new ArrayList<Student>();
for (int i = 1; i <= 10; i++) {
studentList.add(new Student(i,"username"+i ,"pass"+i,18+i,"phone"+i));
}
request.setAttribute("stus", studentList);
%>
<table>
<tr>
<th>编号</th>
<th>用户名</th>
<th>密码</th>
<th>年龄</th>
<th>电话</th>
<th>操作</th>
</tr>
<%--
items 表示遍历的集合
var 表示遍历到的数据
begin 表示遍历的开始索引值
end 表示结束的索引值
step 属性表示遍历的步长值
varStatus 属性表示当前遍历到的数据的状态
for（int i = 1; i < 10; i+=2）
--%>
<c:forEach begin="2" end="7" step="2" varStatus="status" items="${requestScope.stus}" var="stu">
<tr>
<td>${stu.id}</td>
<td>${stu.username}</td>
<td>${stu.password}</td>
<td>${stu.age}</td>
<td>${stu.phone}</td>
<td>${status.step}</td>
</tr>
</c:forEach>
</table>
```
## 15 文件的上传和下载
### 15.1 文件的上传介绍
```markdown
文件的上传和下载，是非常常见的功能。很多的系统中，
或者软件中都经常使用文件的上传和下载。
比如：QQ 头像，就使用了上传。
邮箱中也有附件的上传和下载功能。
OA 系统中审批有附件材料的上传。

1 要有一个 form 标签，method=post 请求
2 form 标签的 encType 属性值必须为 multipart/form-data 值
3 在 form 标签中使用 input type=file 添加上传的文件
4 编写服务器代码（Servlet 程序）接收，处理上传的数据。
encType=multipart/form-data 表示提交的数据，以多段
（每一个表单项一个数据段）的形式进行拼
接，然后以二进制流的形式发送给服务器
```

### 15.2 文件上传，HTTP 协议的说明。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612165320831.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 15.3 commons-fileupload.jar常用API 介绍说明
```markdown
commons-fileupload.jar 需要依赖 commons-io.jar 这个包，所以两个包我们都要引入。
第一步，就是需要导入两个 jar 包：
commons-fileupload-1.2.1.jar
commons-io-1.4.jar
```
```markdown
commons-fileupload.jar 和 commons-io.jar 包中，我们常用的类有哪些？
ServletFileUpload 类，用于解析上传的数据。
FileItem 类，表示每一个表单项。
boolean ServletFileUpload.isMultipartContent(HttpServletRequest request);
判断当前上传的数据格式是否是多段的格式。
public List<FileItem> parseRequest(HttpServletRequest request)
解析上传的数据
boolean FileItem.isFormField()
判断当前这个表单项，是否是普通的表单项。还是上传的文件类型。
true 表示普通类型的表单项
false 表示上传的文件类型
String FileItem.getFieldName()
获取表单项的 name 属性值

String FileItem.getString()
获取当前表单项的值。
String FileItem.getName();
获取上传的文件名
void FileItem.write( file );
将上传的文件写到 参数 file 所指向抽硬盘位置 。
```
### 15.4 fileupload 类库的使用

```html
上传文件的表单
<form action="http://192.168.31.74:8080/09_EL_JSTL/uploadServlet" method="post"
enctype="multipart/form-data">
用户名：<input type="text" name="username" /> <br>
头像：<input type="file" name="photo" > <br>
<input type="submit" value="上传">
</form>
```

```java
解析上传的数据的代码：
UploadServlet.java
package com.yxj.servlet;


import org.apache.commons.fileupload.FileItem;
import org.apache.commons.fileupload.FileItemFactory;
import org.apache.commons.fileupload.disk.DiskFileItemFactory;
import org.apache.commons.fileupload.servlet.ServletFileUpload;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.File;
import java.io.IOException;
import java.util.List;

public class UploadServlet extends HttpServlet {
    /**
     * 用来处理上传的数据
     * @param req
     * @param resp
     * @throws ServletException
     * @throws IOException
     */
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //1 先判断上传的数据是否多段数据（只有是多段的数据，才是文件上传的）
        if (ServletFileUpload.isMultipartContent(req)) {
//           创建FileItemFactory工厂实现类
            FileItemFactory fileItemFactory = new DiskFileItemFactory();
            // 创建用于解析上传数据的工具类ServletFileUpload类
            ServletFileUpload servletFileUpload = new ServletFileUpload(fileItemFactory);
            try {
                // 解析上传的数据，得到每一个表单项FileItem
                List<FileItem> list = servletFileUpload.parseRequest(req);
                // 循环判断，每一个表单项，是普通类型，还是上传的文件
                for (FileItem fileItem : list) {

                    if (fileItem.isFormField()) {
                        // 普通表单项

                        System.out.println("表单项的name属性值：" + fileItem.getFieldName());
                        // 参数UTF-8.解决乱码问题
                        System.out.println("表单项的value属性值：" + fileItem.getString("UTF-8"));

                    } else {
                        // 上传的文件
                        System.out.println("表单项的name属性值：" + fileItem.getFieldName());
                        System.out.println("上传的文件名：" + fileItem.getName());

                        fileItem.write(new File("e:\\" + fileItem.getName()));
                    }
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }

    }

}

```
### 15.5 文件下载
```markdown
下载的常用 API 说明：
response.getOutputStream();
servletContext.getResourceAsStream();
servletContext.getMimeType();
response.setContentType();
response.setHeader("Content-Disposition", "attachment; fileName=1.jpg");
这个响应头告诉浏览器。这是需要下载的。而 attachment 表示附件，
也就是下载的一个文件。fileName=后面，
表示下载的文件名。
完成上面的两个步骤，下载文件是没问题了。但是如果我们要下载的文件是中文名的话。你会发现，下载无法正确
显示出正确的中文名。
原因是在响应头中，不能包含有中文字符，只能包含 ASCII 码。
```
```java
package com.yxj.servlet;

import org.apache.commons.io.IOUtils;
import sun.misc.BASE64Encoder;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.URLEncoder;

public class Download extends HttpServlet {


    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
//        1、获取要下载的文件名
        String downloadFileName = "2.jpg";
//        2、读取要下载的文件内容 (通过ServletContext对象可以读取)
        ServletContext servletContext = getServletContext();
        // 获取要下载的文件类型
        String mimeType = servletContext.getMimeType("/file/" + downloadFileName);
        System.out.println("下载的文件类型：" + mimeType);
//        4、在回传前，通过响应头告诉客户端返回的数据类型
        resp.setContentType(mimeType);
//        5、还要告诉客户端收到的数据是用于下载使用（还是使用响应头）
        // Content-Disposition响应头，表示收到的数据怎么处理
        // attachment表示附件，表示下载使用
        // filename= 表示指定下载的文件名
        // url编码是把汉字转换成为%xx%xx的格式
        if (req.getHeader("User-Agent").contains("Firefox")) {
            // 如果是火狐浏览器使用Base64编码
            resp.setHeader("Content-Disposition", "attachment; filename==?UTF-8?B?" + new BASE64Encoder().encode("中国.jpg".getBytes("UTF-8")) + "?=");
        } else {
            // 如果不是火狐，是IE或谷歌，使用URL编码操作
            resp.setHeader("Content-Disposition", "attachment; filename=" + URLEncoder.encode("中国.jpg", "UTF-8"));
        }
        /**
         * /斜杠被服务器解析表示地址为http://ip:prot/工程名/  映射 到代码的Web目录
         */
        InputStream resourceAsStream = servletContext.getResourceAsStream("/file/" + downloadFileName);
        // 获取响应的输出流
        OutputStream outputStream = resp.getOutputStream();
        //        3、把下载的文件内容回传给客户端
        // 读取输入流中全部的数据，复制给输出流，输出给客户端
        IOUtils.copy(resourceAsStream, outputStream);
    }
}

```

```markdown
附件中文名乱码问题解决方案
方案一：URLEncoder 解决 IE 和谷歌浏览器的 附件中
文名问题。
如果客户端浏览器是 IE 浏览器 或者 是谷歌浏览器。我们需要使用 
URLEncoder 类先对中文名进行 UTF-8 的编码
操作。
因为 IE 浏览器和谷歌浏览器收到含有编码后的字符串后会以 UTF-8 字符集进行解码显示。
// 把中文名进行 UTF-8 编码操作。
String str = "attachment; fileName=" + URLEncoder.encode("中文.jpg", "UTF-8");
// 然后把编码后的字符串设置到响应头中
response.setHeader("Content-Disposition", str);
```
```java
方案二：BASE64 编解码 解决 火狐浏览器的附件中文名问题

如果客户端浏览器是火狐浏览器。 那么我们需要对中文名进行 BASE64 的编码操作。
这时候需要把请求头 Content-Disposition: attachment; filename=中文名
编码成为：Content-Disposition: attachment; filename==?charset?B?xxxxx?=
=?charset?B?xxxxx?= 现在我们对这段内容进行一下说明。
=?
charset
B
xxxx
?=
BASE64 编解码操作：
public static void main(String[] args) throws Exception {
        String content = "这是需要 Base64 编码的内容";
// 创建一个 Base64 编码器
        BASE64Encoder base64Encoder = new BASE64Encoder();
// 执行 Base64 编码操作
        String encodedString = base64Encoder.encode(content.getBytes("UTF-8"));
        System.out.println( encodedString );
// 创建 Base64 解码器
        BASE64Decoder base64Decoder = new BASE64Decoder();
// 解码操作
        byte[] bytes = base64Decoder.decodeBuffer(encodedString);
        String str = new String(bytes, "UTF-8");
        System.out.println(str);
        }
        因为火狐使用的是 BASE64 的编解码方式还原响应中的汉字。所以需要使用 BASE64Encoder 类进行编码操作。
        // 使用下面的格式进行 BASE64 编码后
        String str = "attachment; fileName=" + "=?utf-8?B?"
        + new BASE64Encoder().encode("中文.jpg".getBytes("utf-8")) + "?=";
// 设置到响应头中
        response.setHeader("Content-Disposition", str);
        那么我们如何解决上面两种不同编解码方式呢。我们只需要通过判断请求头中 User-Agent 这个请求头携带过来的
        浏览器信息即可判断出是什么浏览器。
        如下：
        String ua = request.getHeader("User-Agent");
// 判断是否是火狐浏览器
        if (ua.contains("Firefox")) {
// 使用下面的格式进行 BASE64 编码后
        String str = "attachment; fileName=" + "=?utf-8?B?"
        + new BASE64Encoder().encode("中文.jpg".getBytes("utf-8")) + "?=";
// 设置到响应头中
        response.setHeader("Content-Disposition", str);
        } else {
// 把中文名进行 UTF-8 编码操作。
        String str = "attachment; fileName=" + URLEncoder.encode("中文.jpg", "UTF-8");
// 然后把编码后的字符串设置到响应头中
        response.setHeader("Content-Disposition", str);
        }
```
## 16 书城项目第三阶段
### 16.1 第三阶段

```markdown
1 在 html 页面顶行添加 page 指令。
2 修改文件后缀名为：.jsp
3 使用 IDEA 搜索替换.html 为.jsp(快捷键：Ctrl+Shift+R)
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110143430236.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


```html
抽取页面中相同的内容
head 中 css、jquery、base 标签
<%
	String basePath = request.getScheme()
	+ "://"
	+ request.getServerName()
	+ ":"
	+ request.getServerPort()
	+ request.getContextPath()
	+ "/";
%>
<%=basePath%>
<!--写 base 标签，永远固定相对路径跳转的结果-->
<base href="<%=basePath%>">
<link type="text/css" rel="stylesheet" href="static/css/style.css" >
<script type="text/javascript" src="static/script/jquery-1.7.2.js"></script>
```


```html
每个页面的页脚
<div id="bottom">
<span>
尚硅谷书城.Copyright &copy;2015
</span>
</div>
```

```html
登录成功后的菜单

<div>
    <span>欢迎<span class="um_span">韩总</span>光临尚硅谷书城</span>
    <a href="../order/order.jsp">我的订单</a>
    <a href="../../index.jsp">注销</a>&nbsp;&nbsp;
    <a href="../../index.jsp">返回</a>
</div>
```

```html
manager 模块的菜单
<div>
<a href="book_manager.jsp">图书管理</a>
<a href="order_manager.jsp">订单管理</a>
<a href="../../index.jsp">返回商城</a>
</div>
```

```markdown
登录，注册错误提示，及表单回显
以登录回显为示例：
Servlet 程序端需要添加回显信息到 Request 域中
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110143847984.png#pic_center)
```markdown
jsp 页面，需要输出回显信息
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110143920743.png#pic_center)

```markdown
BaseServlet 的抽取
在实际的项目开发中，一个模块，一般只使用一个 Servlet 程序。
```
```markdown
代码优化一：代码优化：合并 LoginServlet 和 RegistServlet 程序为 UserServlet 程序
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110144033739.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
UserServlet 程序：
```


```java
public class UserServlet extends HttpServlet {
    private UserService userService = new UserServiceImpl();
    /**
     * 处理登录的功能
     * @param req
     * @param resp
     * @throws ServletException
     * @throws IOException
     */
    protected void login(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
            IOException {
// 1、获取请求的参数
        String username = req.getParameter("username");
        String password = req.getParameter("password");
// 调用 userService.login()登录处理业务
        User loginUser = userService.login(new User(null, username, password, null));
// 如果等于 null,说明登录 失败!
        if (loginUser == null) {
// 把错误信息，和回显的表单项信息，保存到 Request 域中
            req.setAttribute("msg","用户或密码错误！");
            req.setAttribute("username", username);
// 跳回登录页面
            req.getRequestDispatcher("/pages/user/login.jsp").forward(req, resp);
        } else {
// 登录 成功
//跳到成功页面 login_success.html
            req.getRequestDispatcher("/pages/user/login_success.jsp").forward(req, resp);
        }
    }
    /**
     * 处理注册的功能
     * @param req
     * @param resp
     * @throws ServletException
     * @throws IOException
     */
    protected void regist(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
            IOException {
// 1、获取请求的参数
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String email = req.getParameter("email");
        String code = req.getParameter("code");
// 2、检查 验证码是否正确 === 写死,要求验证码为:abcde
        if ("abcde".equalsIgnoreCase(code)) {
// 3、检查 用户名是否可用
            if (userService.existsUsername(username)) {
                System.out.println("用户名[" + username + "]已存在!");
// 把回显信息，保存到 Request 域中
                req.setAttribute("msg", "用户名已存在！！");
                req.setAttribute("username", username);
                req.setAttribute("email", email);
// 跳回注册页面
                req.getRequestDispatcher("/pages/user/regist.jsp").forward(req, resp);
            } else {
// 可用
// 调用 Sservice 保存到数据库
                userService.registUser(new User(null, username, password, email));
//
// 跳到注册成功页面 regist_success.jsp
                req.getRequestDispatcher("/pages/user/regist_success.jsp").forward(req, resp);
            }
        } else {
// 把回显信息，保存到 Request 域中
            req.setAttribute("msg", "验证码错误！！");
            req.setAttribute("username", username);
            req.setAttribute("email", email);
            System.out.println("验证码[" + code + "]错误");
            req.getRequestDispatcher("/pages/user/regist.jsp").forward(req, resp);
        }
    }
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
            IOException {
        String action = req.getParameter("action");
        if ("login".equals(action)) {
            login(req, resp);
        } else if ("regist".equals(action)) {
            regist(req, resp);
        }
    }
}
```
```markdown
还要给 login.jsp 添加隐藏域和修改请求地址
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110144221115.png#pic_center)
```markdown
给 tegist.jsp 页面添加隐藏域 action，和修改请求地址
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110144252976.png#pic_center)

```java
优化代码二：使用反射优化大量 else if 代码：

protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
IOException {
String action = req.getParameter("action");
try {
// 获取 action 业务鉴别字符串，获取相应的业务 方法反射对象
Method method = this.getClass().getDeclaredMethod(action, HttpServletRequest.class,
HttpServletResponse.class);
// System.out.println(method);
// 调用目标业务 方法
method.invoke(this, req, resp);
} catch (Exception e) {
e.printStackTrace();
}
}

```
```markdown
代码优化三：抽取 BaseServlet 程序
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110144430263.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
BaseServlet 程序代码：
```

```java
public abstract class BaseServlet extends HttpServlet {
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
            IOException {
        String action = req.getParameter("action");
        try {
// 获取 action 业务鉴别字符串，获取相应的业务 方法反射对象
            Method method = this.getClass().getDeclaredMethod(action, HttpServletRequest.class,
                    HttpServletResponse.class);
// System.out.println(method);
// 调用目标业务 方法
            method.invoke(this, req, resp);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
```markdown
修改 UserServlet 程序继承 BaseServlet 程序。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110144542771.png#pic_center)

```markdown
数据的封装和抽取 BeanUtils 的使用
BeanUtils 工具类，它可以一次性的把所有请求的参数注入到 
JavaBean 中。
BeanUtils 工具类，经常用于把 Map 中的值注入到 JavaBean 中，
或者是对象属性值的拷贝操作。
BeanUtils 它不是 Jdk 的类。而是第三方的工具类。所以需要导包。
1  commons-beanutils-1.8.0.jar
commons-logging-1.1.1.jar
2 编写 WebUtils 工具类使用：
```
```markdown
WebUtils 工具类：
```

```java
public class WebUtils {
    /**
     * 把 Map 中的值注入到对应的 JavaBean 属性中。
     * @param value
     * @param bean
     */
    public static <T> T copyParamToBean( Map value , T bean ){
        try {
            System.out.println("注入之前：" + bean);
/**
 * 把所有请求的参数都注入到 user 对象中
 */
            BeanUtils.populate(bean, value);
            System.out.println("注入之后：" + bean);
        } catch (Exception e) {
            e.printStackTrace();
        }
        return bean;
    }
}
```
## 17 书城-第四阶段  使用 EL 表达式修改表单回显
```markdown
以登录为示例
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110144808397.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
## 18 书城项目第五阶段-图书模块
### 18.1 MVC 概念
```markdown
MVC 全称：Model 模型、 View 视图、 Controller 控制器。
MVC 最早出现在 JavaEE 三层中的 Web 层，它可以有效的指
导 Web 层的代码如何有效分离，单独工作。
View 视图：只负责数据和界面的显示，不接受任何与显示数据无
关的代码，便于程序员和美工的分工合作——
JSP/HTML。
Controller 控制器：只负责接收请求，调用业务层的代码处理请求，
然后派发页面，是一个“调度者”的角色——Servlet。
转到某个页面。或者是重定向到某个页面。
Model 模型：将与业务逻辑相关的数据封装为具体的 JavaBean 类，
其中不掺杂任何与数据处理相关的代码——
JavaBean/domain/entity/pojo。
MVC 是一种思想
MVC 的理念是将软件代码拆分成为组件，单独开发，组合使
用（目的还是为了降低耦合度）。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110145029558.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
MVC 的作用还是为了降低耦合。让代码合理分层。方便后期升级和维护。
```
### 18.2 图书模块
#### 18.2.1 编写图书模块的数据库表
```sql
create table t_book(
`id` int primary key auto_increment,
`name` varchar(100),
`price` decimal(11,2),
`author` varchar(100),
`sales` int,
`stock` int,
`img_path` varchar(200)
);
## 插入初始化测试数据
insert into t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
values(null , 'java 从入门到放弃' , '国哥' , 80 , 9999 , 9 , 'static/img/default.jpg');
insert into t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
values(null , '数据结构与算法' , '严敏君' , 78.5 , 6 , 13 , 'static/img/default.jpg');
insert into t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
values(null , '怎样拐跑别人的媳妇' , '龙伍' , 68, 99999 , 52 , 'static/img/default.jpg');
insert into t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
values(null , '木虚肉盖饭' , '小胖' , 16, 1000 , 50 , 'static/img/default.jpg');
insert into t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
values(null , 'C++编程思想' , '刚哥' , 45.5 , 14 , 95 , 'static/img/default.jpg');
insert into t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
values(null , '蛋炒饭' , '周星星' , 9.9, 12 , 53 , 'static/img/default.jpg');
insert into t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
values(null , '赌神' , '龙伍' , 66.5, 125 , 535 , 'static/img/default.jpg');
insert into t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
values(null , 'Java 编程思想' , '阳哥' , 99.5 , 47 , 36 , 'static/img/default.jpg');
insert into t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
values(null , 'JavaScript 从入门到精通' , '婷姐' , 9.9 , 85 , 95 , 'static/img/default.jpg');
insert into t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
values(null , 'cocos2d-x 游戏编程入门' , '国哥' , 49, 52 , 62 , 'static/img/default.jpg');
insert into t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
values(null , 'C 语言程序设计' , '谭浩强' , 28 , 52 , 74 , 'static/img/default.jpg');
insert into t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
values(null , 'Lua 语言程序设计' , '雷丰阳' , 51.5 , 48 , 82 , 'static/img/default.jpg');
insert into t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
values(null , '西游记' , '罗贯中' , 12, 19 , 9999 , 'static/img/default.jpg');
insert into t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
values(null , '水浒传' , '华仔' , 33.05 , 22 , 88 , 'static/img/default.jpg');
insert into t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
values(null , '操作系统原理' , '刘优' , 133.05 , 122 , 188 , 'static/img/default.jpg');
insert into t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
values(null , '数据结构 java 版' , '封大神' , 173.15 , 21 , 81 , 'static/img/default.jpg');

insert into t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
values(null , 'UNIX 高级环境编程' , '乐天' , 99.15 , 210 , 810 , 'static/img/default.jpg');
insert into t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
values(null , 'javaScript 高级编程' , '国哥' , 69.15 , 210 , 810 , 'static/img/default.jpg');
insert into t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
values(null , '大话设计模式' , '国哥' , 89.15 , 20 , 10 , 'static/img/default.jpg');
insert into t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
values(null , '人月神话' , '刚哥' , 88.15 , 20 , 80 , 'static/img/default.jpg');
## 查看表内容
select id,name,author,price,sales,stock,img_path from t_book;
```
#### 18.2.2 编写图书模块的JavaBean

```java
public class Book {
    private Integer id;
    private String name;
    private String author;
    private BigDecimal price;
    private Integer sales;
    private Integer stock;
    private String imgPath = "static/img/default.jpg";
}
```
#### 18.2.3 编写图书模块的Dao和测试Dao
```markdown
Dao 接口
```

```java
public interface BookDao {
    public int addBook(Book book);
    public int deleteBookById(Integer id);
    public int updateBook(Book book);
    public Book queryBookById(Integer id);
    public List<Book> queryBooks();
}
```
```markdown
BookDaoImpl 实现类
```


```java
public class BookDaoImpl extends BaseDao implements BookDao {
    @Override
    public int addBook(Book book) {
        String sql = "insert into t_book(`name`,`author`,`price`,`sales`,`stock`,`img_path`)
        values(?,?,?,?,?,?)";
        return update(sql,
                book.getName(),book.getAuthor(),book.getPrice(),book.getSales(),book.getStock(),book.getImgPath());
    }
    @Override
    public int deleteBookById(Integer id) {
        String sql = "delete from t_book where id = ?";
        return update(sql, id);
    }
    @Override
    public int updateBook(Book book) {
        String sql = "update t_book set `name`=?,`author`=?,`price`=?,`sales`=?,`stock`=?,`img_path`=?
        where id = ?";
        return
                update(sql,book.getName(),book.getAuthor(),book.getPrice(),book.getSales(),book.getStock(),book.ge
                        tImgPath(),book.getId());
    }
    @Override
    public Book queryBookById(Integer id) {
        String sql = "select `id` , `name` , `author` , `price` , `sales` , `stock` , `img_path` imgPath
        from t_book where id = ?";
        return queryForOne(Book.class, sql,id);
    }
    @Override
    public List<Book> queryBooks() {
        String sql = "select `id` , `name` , `author` , `price` , `sales` , `stock` , `img_path` imgPath
        from t_book";
        return queryForList(Book.class, sql);
    }
}
```
```markdown
BookDao 的测试
```


```java
public class BookDaoTest {
    private BookDao bookDao = new BookDaoImpl();
    @Test
    public void addBook() {
        bookDao.addBook(new Book(null,"国哥为什么这么帅！", "191125", new
                BigDecimal(9999),1100000,0,null
        ));
    }
    @Test
    public void deleteBookById() {
        bookDao.deleteBookById(21);
    }
    @Test
    public void updateBook() {
        bookDao.updateBook(new Book(21,"大家都可以这么帅！", "国哥", new
                BigDecimal(9999),1100000,0,null
        ));
    }
    @Test
    public void queryBookById() {
        System.out.println( bookDao.queryBookById(21) );
    }
    @Test
    public void queryBooks() {
        for (Book queryBook : bookDao.queryBooks()) {
            System.out.println(queryBook);
        }
    }
}
```
#### 18.2.4 编写图书模块的Service和测试 Service
```markdown
BookService 接口
```


```java
public interface BookService {
    public void addBook(Book book);
    public void deleteBookById(Integer id);
    public void updateBook(Book book);
    public Book queryBookById(Integer id);
    public List<Book> queryBooks();
}
```
```markdown
BookServiceImpl 实现类
```


```java
public class BookServiceImpl implements BookService {
    private BookDao bookDao = new BookDaoImpl();
    @Override
    public void addBook(Book book) {
        bookDao.addBook(book);
    }
    @Override
    public void deleteBookById(Integer id) {
        bookDao.deleteBookById(id);
    }
    @Override
    public void updateBook(Book book) {
        bookDao.updateBook(book);
    }
    @Override
    public Book queryBookById(Integer id) {
        return bookDao.queryBookById(id);
    }
    @Override
    public List<Book> queryBooks() {
        return bookDao.queryBooks();
    }
}
```
```markdown
BookService 的测试
```


```java
public class BookServiceTest {
    private BookService bookService = new BookServiceImpl();
    @Test
    public void addBook() {
        bookService.addBook(new Book(null,"国哥在手，天下我有！", "1125", new BigDecimal(1000000),
                100000000, 0, null));
    }
    @Test
    public void deleteBookById() {
        bookService.deleteBookById(22);
    }
    @Test
    public void updateBook() {
        bookService.updateBook(new Book(22,"社会我国哥，人狠话不多！", "1125", new BigDecimal(999999),
                10, 111110, null));
    }
    @Test
    public void queryBookById() {
        System.out.println(bookService.queryBookById(22));
    }
    @Test
    public void queryBooks() {
        for (Book queryBook : bookService.queryBooks()) {
            System.out.println(queryBook);
        }
    }
}
```
#### 18.2.5 编写图书模块的Web层，和页面联调测试
##### 18.2.5.1 图书列表功能的实现
```markdown
1 图解列表功能流程
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110145938673.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
2 BookServlet 程序中添加 list 方法
```


```java
protected void list(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
//1 通过 BookService 查询全部图书
        List<Book> books = bookService.queryBooks();
//2 把全部图书保存到 Request 域中
        req.setAttribute("books", books);
//3、请求转发到/pages/manager/book_manager.jsp 页面
        req.getRequestDispatcher("/pages/manager/book_manager.jsp").forward(req,resp);
}
```
```markdown
3 修改【图书管理】请求地址
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110150021522.png#pic_center)
```markdown
4 修改 pages/manager/book_manager.jsp 页面的数据遍历输出
```


```html
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>图书管理</title>
<%-- 静态包含 base 标签、css 样式、jQuery 文件 --%>
<%@ include file="/pages/common/head.jsp"%>
</head>
<body>
<div id="header">
<img class="logo_img" alt="" src="../../static/img/logo.gif" >
<span class="wel_word">图书管理系统</span>
<%-- 静态包含 manager 管理模块的菜单 --%>
<%@include file="/pages/common/manager_menu.jsp"%>
</div>
<div id="main">
<table>
<tr>
<td>名称</td>
<td>价格</td>
<td>作者</td>
<td>销量</td>
<td>库存</td>
<td colspan="2">操作</td>
</tr>
<c:forEach items="${requestScope.books}" var="book">
<tr>
<td>${book.name}</td>
<td>${book.price}</td>
<td>${book.author}</td>
<td>${book.sales}</td>
<td>${book.stock}</td>
<td><a href="book_edit.jsp">修改</a></td>
<td><a href="#">删除</a></td>
</tr>
</c:forEach>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td><a href="book_edit.jsp">添加图书</a></td>
</tr>
</table>
</div>
<%--静态包含页脚内容--%>
<%@include file="/pages/common/footer.jsp"%>
</body>
</html>
```
##### 18.2.5.2 前后台的简单介绍
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110150150256.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
##### 18.2.5.3 添加图书功能的实现
###### 18.2.5.3.1 添加图书流程细节：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110150255289.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
###### 18.2.5.3.2 问题说明：表单重复提交
```markdown
当用户提交完请求，浏览器会记录下最后一次请求的全部信息。
当用户按下功能键 F5，就会发起浏览器记录的最后一次
请求。
```
###### 18.2.5.3.3 BookServlet 程序中添加add方法

```java
protected void add(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
// 1、获取请求的参数==封装成为 Book 对象
        Book book = WebUtils.copyParamToBean(req.getParameterMap(),new Book());
// 2、调用 BookService.addBook()保存图书
        bookService.addBook(book);
// 3、跳到图书列表页面
// /manager/bookServlet?action=list
// req.getRequestDispatcher("/manager/bookServlet?action=list").forward(req, resp);
        resp.sendRedirect(req.getContextPath() + "/manager/bookServlet?action=list");
}
```
###### 18.2.5.3.4 修改book_edit.jsp页面
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>编辑图书</title>
<%-- 静态包含 base 标签、css 样式、jQuery 文件 --%>
<%@ include file="/pages/common/head.jsp"%>
<style type="text/css">
h1 {
text-align: center;
margin-top: 200px;
}
h1 a {
color:red;
}
input {
text-align: center;
}
</style>
</head>
<body>
<div id="header">
<img class="logo_img" alt="" src="../../static/img/logo.gif" >
<span class="wel_word">编辑图书</span>
<%-- 静态包含 manager 管理模块的菜单 --%>
<%@include file="/pages/common/manager_menu.jsp"%>
</div>
<div id="main">
<form action="manager/bookServlet" method="get">
<input type="hidden" name="action" value="add" />
<table>
<tr>
<td>名称</td>
<td>价格</td>
<td>作者</td>
<td>销量</td>
<td>库存</td>
<td colspan="2">操作</td>
</tr>
<tr>
<td><input name="name" type="text" value="时间简史"/></td>
<td><input name="price" type="text" value="30.00"/></td>
<td><input name="author" type="text" value="霍金"/></td>
<td><input name="sales" type="text" value="200"/></td>
<td><input name="stock" type="text" value="300"/></td>
<td><input type="submit" value="提交"/></td>
</tr>
</table>
</form>
</div>
<%--静态包含页脚内容--%>
<%@include file="/pages/common/footer.jsp"%>
</body>
</html>
```
##### 18.2.5.4 删除图书功能的实现
###### 18.2.5.4.1 图解删除流程
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110150632825.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
###### 18.2.5.4.2 BookServlet 程序中的 delete 方法
```java
protected void delete(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
// 1、获取请求的参数 id，图书编程
        int id = WebUtils.parseInt(req.getParameter("id"), 0);
// 2、调用 bookService.deleteBookById();删除图书
        bookService.deleteBookById(id);
// 3、重定向回图书列表管理页面
// /book/manager/bookServlet?action=list
        resp.sendRedirect(req.getContextPath() + "/manager/bookServlet?action=list");
}
```
###### 18.2.5.4.3 给 WebUtils 工具类添加转换 int 类型的工具方法
```java
/**
 * 将字符串转换成为 int 类型的数据
 * @param strInt
 * @param defaultValue
 * @return
 */
public static int parseInt(String strInt,int defaultValue) {
        try {
        return Integer.parseInt(strInt);
        } catch (Exception e) {
        e.printStackTrace();
        }
        return defaultValue;
        }
```
###### 18.2.5.4.4 修改删除的连接地址
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110150839153.png#pic_center)
###### 18.2.5.4.5 给删除添加确认提示操作
```html
<script type="text/javascript">
    $(function () {
    // 给删除的 a 标签绑定单击事件，用于删除的确认提示操作
    $("a.deleteClass").click(function () {
    // 在事件的 function 函数中，有一个 this 对象。这个 this 对象，是当前正在响应事件的 dom 对象。
    /**
    * confirm 是确认提示框函数
    * 参数是它的提示内容
    * 它有两个按钮，一个确认，一个是取消。
    * 返回 true 表示点击了，确认，返回 false 表示点击取消。
    */
    return confirm("你确定要删除【" + $(this).parent().parent().find("td:first").text() + "】?");
    // return false// 阻止元素的默认行为===不提交请求
    });
    });
    </script>
```
##### 18.2.5.5 修改图书功能的实现
###### 18.2.5.5.1 图解修改图书细节
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110151040907.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
###### 18.2.5.5.2 更新【修改】的请求地址
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110151114996.png#pic_center)
###### 18.2.5.5.3 BookServlet 程序中添加 getBook 方法

```java
protected void getBook(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
//1 获取请求的参数图书编号
        int id = WebUtils.parseInt(req.getParameter("id"), 0);
//2 调用 bookService.queryBookById 查询图书
        Book book = bookService.queryBookById(id);
//3 保存到图书到 Request 域中
        req.setAttribute("book", book) ;
//4 请求转发到。pages/manager/book_edit.jsp 页面
        req.getRequestDispatcher("/pages/manager/book_edit.jsp").forward(req,resp);
}
```
###### 18.2.5.5.4 在book_edit.jsp页面中显示修改的数据
```html
<div id="main">
    <form action="manager/bookServlet" method="get">
    <input type="hidden" name="action" value="add" />
    <table>
    <tr>
    <td>名称</td>
    <td>价格</td>
    <td>作者</td>
    <td>销量</td>
    <td>库存</td>
    <td colspan="2">操作</td>
    </tr>
    <tr>
    <td><input name="name" type="text" value="${requestScope.book.name}"/></td>
    <td><input name="price" type="text" value="${requestScope.book.price}"/></td>
    <td><input name="author" type="text" value="${requestScope.book.author}"/></td>
    <td><input name="sales" type="text" value="${requestScope.book.sales}"/></td>
    <td><input name="stock" type="text" value="${requestScope.book.stock}"/></td>
    <td><input type="submit" value="提交"/></td>
    </tr>
    </table>
    </form>
</div>
```
###### 18.2.5.5.5 在BookServlet程序中添加update方法

```java
protected void update(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
// 1、获取请求的参数==封装成为 Book 对象
        Book book = WebUtils.copyParamToBean(req.getParameterMap(),new Book());
// 2、调用 BookService.updateBook( book );修改图书
        bookService.updateBook(book);
// 3、重定向回图书列表管理页面
// 地址：/工程名/manager/bookServlet?action=list
        resp.sendRedirect(req.getContextPath() + "/manager/bookServlet?action=list");
}
```
###### 18.2.5.5.6 解决book_edit.jsp页面，即要实现添加，又要实现修改操作
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110151411422.png#pic_center)
### 18.3 图书分页
#### 18.3.1 图书分页
```markdown
分页模块的分析
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110151908996.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
分页模型 Page 的抽取（当前页数，总页数，总记录数，
当前页数据，每页记录数）
```


```java
/**
 * Page 是分页的模型对象
 * @param <T> 是具体的模块的 javaBean 类
 */
public class Page<T> {
    public static final Integer PAGE_SIZE = 4;
    // 当前页码
    private Integer pageNo;
    // 总页码
    private Integer pageTotal;
    // 当前页显示数量
    private Integer pageSize = PAGE_SIZE;
    // 总记录数
    private Integer pageTotalCount;
    // 当前页数据
    private List<T> items;
}
```
```markdown
分页的初步实现
```
```markdown
BookDao 代码
```


```java
@Override
public Integer queryForPageTotalCount() {
        String sql = "select count(*) from t_book";
        Number count = (Number) queryForSingleValue(sql);
        return count.intValue();
        }
@Override
public List<Book> queryForPageItems(int begin, int pageSize) {
        String sql = "select `id` , `name` , `author` , `price` , `sales` , `stock` , `img_path` imgPath
        from t_book limit ?,?";
        return queryForList(Book.class,sql,begin,pageSize);
        }
```
```markdown
BookService 代码
```

```java
@Override
public Page<Book> page(int pageNo, int pageSize) {
        Page<Book> page = new Page<Book>();
// 设置当前页码
        page.setPageNo(pageNo);
// 设置每页显示的数量
        page.setPageSize(pageSize);
// 求总记录数
        Integer pageTotalCount = bookDao.queryForPageTotalCount();
// 设置总记录数
        page.setPageTotalCount(pageTotalCount);
// 求总页码
        Integer pageTotal = pageTotalCount / pageSize;
        if (pageTotalCount % pageSize > 0) {
        pageTotal+=1;
        }
// 设置总页码
        page.setPageTotal(pageTotal);
// 求当前页数据的开始索引
        int begin = (page.getPageNo() - 1) * pageSize;
// 求当前页数据
        List<Book> items = bookDao.queryForPageItems(begin,pageSize);
// 设置当前页数据
        page.setItems(items);
        return page;
        }
```
```markdown
BookServlet 程序的代码
```


```java
/**
 * 处理分页功能
 * @param req
 * @param resp
 * @throws ServletException
 * @throws IOException
 */
protected void page(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
//1 获取请求的参数 pageNo 和 pageSize
        int pageNo = WebUtils.parseInt(req.getParameter("pageNo"), 1);
        int pageSize = WebUtils.parseInt(req.getParameter("pageSize"), Page.PAGE_SIZE);
//2 调用 BookService.page(pageNo，pageSize)：Page 对象
        Page<Book> page = bookService.page(pageNo,pageSize);
//3 保存 Page 对象到 Request 域中
        req.setAttribute("page",page);
//4 请求转发到 pages/manager/book_manager.jsp 页面
        req.getRequestDispatcher("/pages/manager/book_manager.jsp").forward(req,resp);
        }
```
```markdown
manager_menu.jsp 中【图书管理】请求地址的修改
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110152239569.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
book_manager.jsp 修改：
```
```html
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>图书管理</title>
<%-- 静态包含 base 标签、css 样式、jQuery 文件 --%>
<%@ include file="/pages/common/head.jsp"%>
<script type="text/javascript">
$(function () {
// 给删除的 a 标签绑定单击事件，用于删除的确认提示操作
$("a.deleteClass").click(function () {
// 在事件的 function 函数中，有一个 this 对象。这个 this 对象，是当前正在响应事件的 dom 对象。
/**
* confirm 是确认提示框函数
* 参数是它的提示内容
* 它有两个按钮，一个确认，一个是取消。
* 返回 true 表示点击了，确认，返回 false 表示点击取消。
*/
return confirm("你确定要删除【" + $(this).parent().parent().find("td:first").text() + "】?");
// return false// 阻止元素的默认行为===不提交请求
});
});
</script>
</head>
<body>
<div id="header">
<img class="logo_img" alt="" src="../../static/img/logo.gif" >
<span class="wel_word">图书管理系统</span>
<%-- 静态包含 manager 管理模块的菜单 --%>
<%@include file="/pages/common/manager_menu.jsp"%>
</div>
<div id="main">
<table>
<tr>
<td>名称</td>
<td>价格</td>
<td>作者</td>
<td>销量</td>
<td>库存</td>
<td colspan="2">操作</td>
</tr>
<c:forEach items="${requestScope.page.items}" var="book">
<tr>
<td>${book.name}</td>
<td>${book.price}</td>
<td>${book.author}</td>
<td>${book.sales}</td>
<td>${book.stock}</td>
<td><a href="manager/bookServlet?action=getBook&id=${book.id}">修改</a></td>
<td><a class="deleteClass" href="manager/bookServlet?action=delete&id=${book.id}">删
除</a></td>
</tr>
</c:forEach>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td><a href="pages/manager/book_edit.jsp">添加图书</a></td>
</tr>
</table>
<div id="page_nav">
<a href="#">首页</a>
<a href="#">上一页</a>
<a href="#">3</a>
【${ requestScope.page.pageNo }】
<a href="#">5</a>
<a href="#">下一页</a>
<a href="#">末页</a>
共${ requestScope.page.pageTotal }页，${ requestScope.page.pageTotalCount }条记录
到第<input value="4" name="pn" id="pn_input"/>页
<input type="button" value="确定">
</div>
</div>
<%--静态包含页脚内容--%>
<%@include file="/pages/common/footer.jsp"%>
</body>
</html>
```
```markdown
首页、上一页、下一页、末页实现
```

```html
<div id="page_nav">
    <%--大于首页，才显示--%>
    <c:if test="${requestScope.page.pageNo > 1}">
    <a href="manager/bookServlet?action=page&pageNo=1">首页</a>
    <a href="manager/bookServlet?action=page&pageNo=${requestScope.page.pageNo-1}">上一页</a>
    </c:if>
    <a href="#">3</a>
    【${ requestScope.page.pageNo }】
    <a href="#">5</a>
    <%-- 如果已经 是最后一页，则不显示下一页，末页 --%>
    <c:if test="${requestScope.page.pageNo < requestScope.page.pageTotal}">
    <a href="manager/bookServlet?action=page&pageNo=${requestScope.page.pageNo+1}">下一页</a>
    <a href="manager/bookServlet?action=page&pageNo=${requestScope.page.pageTotal}">末页</a>
    </c:if>
    共${ requestScope.page.pageTotal }页，${ requestScope.page.pageTotalCount }条记录
    到第<input value="4" name="pn" id="pn_input"/>页
    <input type="button" value="确定">
</div>
```
```markdown
分页模块中跳转到指定页数功能实现
```


```html
<div id="page_nav">
    <%--大于首页，才显示--%>
        <c:if test="${requestScope.page.pageNo > 1}">
            <a href="manager/bookServlet?action=page&pageNo=1">首页</a>
            <a href="manager/bookServlet?action=page&pageNo=${requestScope.page.pageNo-1}">上一页</a>
        </c:if>
        <a href="#">3</a>
        【${ requestScope.page.pageNo }】
        <a href="#">5</a>
        <%-- 如果已经 是最后一页，则不显示下一页，末页 --%>
            <c:if test="${requestScope.page.pageNo < requestScope.page.pageTotal}">
                <a href="manager/bookServlet?action=page&pageNo=${requestScope.page.pageNo+1}">下一页</a>
                <a href="manager/bookServlet?action=page&pageNo=${requestScope.page.pageTotal}">末页</a>
            </c:if>
            共${ requestScope.page.pageTotal }页，${ requestScope.page.pageTotalCount }条记录
            到第<input value=<div id="page_nav">
            <%--大于首页，才显示--%>
                <c:if test="${requestScope.page.pageNo > 1}">
                    <a href="manager/bookServlet?action=page&pageNo=1">首页</a>
                    <a href="manager/bookServlet?action=page&pageNo=${requestScope.page.pageNo-1}">上一页</a>
                </c:if>
                <a href="#">3</a>
                【${ requestScope.page.pageNo }】
                <a href="#">5</a>
                <%-- 如果已经 是最后一页，则不显示下一页，末页 --%>
                    <c:if test="${requestScope.page.pageNo < requestScope.page.pageTotal}">
                        <a href="manager/bookServlet?action=page&pageNo=${requestScope.page.pageNo+1}">下一页</a>
                        <a href="manager/bookServlet?action=page&pageNo=${requestScope.page.pageTotal}">末页</a>
                    </c:if>
                    共${ requestScope.page.pageTotal }页，${ requestScope.page.pageTotalCount }条记录
                    到第<input value="${param.pageNo}" name="pn" id="pn_input" />页
                    <input id="searchPageBtn" type="button" value="确定">
                    <script type="text/javascript">
                        $(function () {
                            // 跳到指定的页码
                            $("#searchPageBtn").click(function () {
                                var pageNo = $("#pn_input").val();
                <% --var pageTotal = ${ requestScope.page.pageTotal }; --%>
                <% --alert(pageTotal); --%>
                                    // javaScript 语言中提供了一个 location 地址栏对象
                                    // 它有一个属性叫 href.它可以获取浏览器地址栏中的地址
                                    // href 属性可读，可写
                                    location.href = "${pageScope.basePath}manager/bookServlet?action=page&pageNo=" +
                                        pageNo;
                            });
                        });
                    </script>
</div>"${param.pageNo}" name="pn" id="pn_input" />页
<input id="searchPageBtn" type="button" value="确定">
<script type="text/javascript">
    $(function () {
        // 跳到指定的页码
        $("#searchPageBtn").click(function () {
            var pageNo = $("#pn_input").val();
    <% --var pageTotal = ${ requestScope.page.pageTotal }; --%>
    <% --alert(pageTotal); --%>
                // javaScript 语言中提供了一个 location 地址栏对象
                // 它有一个属性叫 href.它可以获取浏览器地址栏中的地址
                // href 属性可读，可写
                location.href = "${pageScope.basePath}manager/bookServlet?action=page&pageNo=" +
                    pageNo;
        });
    });
</script>
</div>
```
```markdown
Page 对象中的修改：
```
```java
public void setPageNo(Integer pageNo) {
        /* 数据边界的有效检查 */
        if (pageNo < 1) {
        pageNo = 1;
        }
        if (pageNo > pageTotal) {
        pageNo = pageTotal;
        }
        this.pageNo = pageNo;
}
```
```markdown
BookService 中 page 方法的修改
```

```java
@Override
public Page<Book> page(int pageNo, int pageSize) {
        Page<Book> page = new Page<Book>();
// 设置每页显示的数量
        page.setPageSize(pageSize);
// 求总记录数
        Integer pageTotalCount = bookDao.queryForPageTotalCount();
// 设置总记录数
        page.setPageTotalCount(pageTotalCount);
// 求总页码
        Integer pageTotal = pageTotalCount / pageSize;
        if (pageTotalCount % pageSize > 0) {
        pageTotal+=1;
        }
// 设置总页码
        page.setPageTotal(pageTotal);
// 设置当前页码
        page.setPageNo(pageNo);
// 求当前页数据的开始索引
        int begin = (page.getPageNo() - 1) * pageSize;
// 求当前页数据
        List<Book> items = bookDao.queryForPageItems(begin,pageSize);
// 设置当前页数据
        page.setItems(items);
        return page;
}
```
```markdown
分页模块中，页码 1,2,【3】,4,5 的显示，要显示 5 个页
码，并且页码可以点击跳转。
```

```markdown
需求：显示 5 个连续的页码，而且当前页码在中间。除了当前页码之外，每个页码都可以点击跳到指定页。
情况 1：如果总页码小于等于 5 的情况，页码的范围是：1-总页码
1 页 1
2 页 1，2
3 页 1，2，3
4 页 1，2，3，4
5 页 1，2，3，4，5

情况 2：总页码大于 5 的情况。假设一共 10 页
小情况 1：当前页码为前面 3 个：1，2，3 的情况，页码范围是：1-5. 【1】2，3，4，5
1【2】3，4，5
1，2【3】4，5


小情况 3：当前页码为最后 3 个，8，9，10，页码范围是：总页码减 4 - 总页码
6，7【8】9，10
6，7，8【9】10
6，7，8，9【10】

小情况 4：4，5，6，7，页码范围是：当前页码减 2 - 当前页码加 2
2，3，4，5，6
3，4，5，6，7
4，5，6，7，8
5，6，7，8，9
```

```html
<%--页码输出的开始--%>
    <c:choose>
        <%--情况 1：如果总页码小于等于 5 的情况，页码的范围是：1-总页码--%>
            <c:when test="${ requestScope.page.pageTotal <= 5 }">
                <c:set var="begin" value="1" />
                <c:set var="end" value="${requestScope.page.pageTotal}" />
            </c:when>
            <%--情况 2：总页码大于 5 的情况--%>
                <c:when test="${requestScope.page.pageTotal > 5}">
                    <c:choose>
                        <%--小情况 1：当前页码为前面 3 个：1，2，3 的情况，页码范围是：1-5.--%>
                            <c:when test="${requestScope.page.pageNo <= 3}">
                                <c:set var="begin" value="1" />
                                <c:set var="end" value="5" />
                            </c:when>
                            <%--小情况 2：当前页码为最后 3 个，8，9，10，页码范围是：总页码减 4 - 总页码--%>
                                <c:when test="${requestScope.page.pageNo > requestScope.page.pageTotal-3}">
                                    <c:set var="begin" value="${requestScope.page.pageTotal-4}" />
                                    <c:set var="end" value="${requestScope.page.pageTotal}" />
                                </c:when>
                                <%--小情况 3：4，5，6，7，页码范围是：当前页码减 2 - 当前页码加 2--%>
                                    <c:otherwise>
                                        <c:set var="begin" value="${requestScope.page.pageNo-2}" />
                                        <c:set var="end" value="${requestScope.page.pageNo+2}" />
                                    </c:otherwise>
                    </c:choose>
                </c:when>
    </c:choose>
    <c:forEach begin="${begin}" end="${end}" var="i">
        <c:if test="${i == requestScope.page.pageNo}">
            【${i}】
        </c:if>
        <c:if test="${i != requestScope.page.pageNo}">
            <a href="manager/bookServlet?action=page&pageNo=${i}">${i}</a>
        </c:if>
    </c:forEach>
    <%--页码输出的结束--%>
```
```markdown
修改分页后，增加，删除，修改图书信息的回显页面
```


```html
以修改图书为示例：
1 在修改的请求地址上追加当前页码参数：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110182832722.png#pic_center)

```html
2 在 book_edit.jsp 页面中使用隐藏域记录下 pageNo 参数
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110182911422.png#pic_center)

```html
3 在服务器重定向的时候，获取当前页码追加上进行跳转
```
```java
protected void update(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
// 1、获取请求的参数==封装成为 Book 对象
        Book book = WebUtils.copyParamToBean(req.getParameterMap(),new Book());
// 2、调用 BookService.updateBook( book );修改图书
        bookService.updateBook(book);
// 3、重定向回图书列表管理页面
// 地址：/工程名/manager/bookServlet?action=list
        resp.sendRedirect(req.getContextPath() + "/manager/bookServlet?action=page&pageNo=" +
        req.getParameter("pageNo"));
}
```

#### 18.3.2 首页 index.jsp 的跳转
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110183414507.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 18.4 分页条的抽取
#### 18.4.1 抽取分页条中请求地址为url变量
##### 18.4.1.1 在 page 对象中添加url属性

```java
/**
 * Page 是分页的模型对象
 * @param <T> 是具体的模块的 javaBean 类
 */
public class Page<T> {
    public static final Integer PAGE_SIZE = 4;
    // 当前页码
    private Integer pageNo;
    // 总页码
    private Integer pageTotal;
    // 当前页显示数量
    private Integer pageSize = PAGE_SIZE;
    // 总记录数
    private Integer pageTotalCount;
    // 当前页数据
    private List<T> items;
    // 分页条的请求地址
    private String url;

}
```
#### 18.4.2 在Servlet程序的page分页方法中设置url的分页请求地址
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110183748765.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
#### 18.4.3 修改分页条中请求地址为url变量输出,并抽取一个单独的jsp页面

```html
<%--分页条的开始--%>
    <div id="page_nav">
        <%--大于首页，才显示--%>
            <c:if test="${requestScope.page.pageNo > 1}">
                <a href="${ requestScope.page.url }&pageNo=1">首页</a>
                <a href="${ requestScope.page.url }&pageNo=${requestScope.page.pageNo-1}">上一页</a>
            </c:if>
            <%--页码输出的开始--%>
                <c:choose>
                    <%--情况 1：如果总页码小于等于 5 的情况，页码的范围是：1-总页码--%>
                        <c:when test="${ requestScope.page.pageTotal <= 5 }">
                            <c:set var="begin" value="1" />
                            <c:set var="end" value="${requestScope.page.pageTotal}" />
                        </c:when>
                        <%--情况 2：总页码大于 5 的情况--%>
                            <c:when test="${requestScope.page.pageTotal > 5}">
                                <c:choose>
                                    <%--小情况 1：当前页码为前面 3 个：1，2，3 的情况，页码范围是：1-5.--%>
                                        <c:when test="${requestScope.page.pageNo <= 3}">
                                            <c:set var="begin" value="1" />
                                            <c:set var="end" value="5" />
                                        </c:when>
                                        <%--小情况 2：当前页码为最后 3 个，8，9，10，页码范围是：总页码减 4 - 总页码--%>
                                            <c:when test="${requestScope.page.pageNo > requestScope.page.pageTotal-3}">
                                                <c:set var="begin" value="${requestScope.page.pageTotal-4}" />
                                                <c:set var="end" value="${requestScope.page.pageTotal}" />
                                            </c:when>
                                            <%--小情况 3：4，5，6，7，页码范围是：当前页码减 2 - 当前页码加 2--%>
                                                <c:otherwise>
                                                    <c:set var="begin" value="${requestScope.page.pageNo-2}" />
                                                    <c:set var="end" value="${requestScope.page.pageNo+2}" />
                                                </c:otherwise>
                                </c:choose>
                            </c:when>
                </c:choose>
                <c:forEach begin="${begin}" end="${end}" var="i">
                    <c:if test="${i == requestScope.page.pageNo}">
                        【${i}】
                    </c:if>
                    <c:if test="${i != requestScope.page.pageNo}">
                        <a href="${ requestScope.page.url }&pageNo=${i}">${i}</a>
                    </c:if>
                </c:forEach>
                <%--页码输出的结束--%>
                    <%-- 如果已经 是最后一页，则不显示下一页，末页 --%>
                        <c:if test="${requestScope.page.pageNo < requestScope.page.pageTotal}">
                            <a href="${ requestScope.page.url }&pageNo=${requestScope.page.pageNo+1}">下一页</a>
                            <a href="${ requestScope.page.url }&pageNo=${requestScope.page.pageTotal}">末页</a>
                        </c:if>
                        共${ requestScope.page.pageTotal }页，${ requestScope.page.pageTotalCount }条记录
                        到第<input value="${param.pageNo}" name="pn" id="pn_input" />页
                        <input id="searchPageBtn" type="button" value="确定">
                        <script type="text/javascript">
                            $(function () {
                                // 跳到指定的页码
                                $("#searchPageBtn").click(function () {
                                    var pageNo = $("#pn_input").val();
<% --var pageTotal = ${ requestScope.page.pageTotal }; --%>
<% --alert(pageTotal); --%>
                                        // javaScript 语言中提供了一个 location 地址栏对象
                                        // 它有一个属性叫 href.它可以获取浏览器地址栏中的地址
                                        // href 属性可读，可写
                                        location.href = "${pageScope.basePath}${ requestScope.page.url }&pageNo=" + pageNo;
                                });
                            });
                        </script>
    </div>
    <%--分页条的结束--%>
```
#### 18.4.4 首页价格搜索
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110184033650.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 19 Cookie 和 Session
### 19.1 Cookie饼干
```markdown
1、Cookie 翻译过来是饼干的意思。
2、Cookie 是服务器通知客户端保存键值对的一种技术。
3、客户端有了 Cookie 后，每次请求都发送给服务器。
4、每个 Cookie 的大小不能超过 4kb
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612210851785.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
Servlet 程序中的代码
```

```java
protected void createCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
//1 创建 Cookie 对象
        Cookie cookie = new Cookie("key4", "value4");
//2 通知客户端保存 Cookie
        resp.addCookie(cookie);
//1 创建 Cookie 对象
        Cookie cookie1 = new Cookie("key5", "value5");
//2 通知客户端保存 Cookie
        resp.addCookie(cookie1);
        resp.getWriter().write("Cookie 创建成功");
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612211753501.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
Cookie 的工具类
```

```java
public class CookieUtils {
    /**
     * 查找指定名称的 Cookie 对象
     * @param name
     * @param cookies
     * @return
     */
    public static Cookie findCookie(String name , Cookie[] cookies){
        if (name == null || cookies == null || cookies.length == 0) {
            return null;
        }
        for (Cookie cookie : cookies) {
            if (name.equals(cookie.getName())) {
                return cookie;
            }
        }
        return null;
    }
}
```
```markdown
Servlet 程序中的代码
```

```java
protected void getCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
        Cookie[] cookies = req.getCookies();
        for (Cookie cookie : cookies) {
// getName 方法返回 Cookie 的 key（名）
// getValue 方法返回 Cookie 的 value 值
        resp.getWriter().write("Cookie[" + cookie.getName() + "=" + cookie.getValue() + "] <br/>");
        }
        Cookie iWantCookie = CookieUtils.findCookie("key1", cookies);
// for (Cookie cookie : cookies) {
// if ("key2".equals(cookie.getName())) {
// iWantCookie = cookie;
// break;
// }
// }
// 如果不等于 null，说明赋过值，也就是找到了需要的 Cookie
        if (iWantCookie != null) {
        resp.getWriter().write("找到了需要的 Cookie");
        }
}
```
```markdown
Cookie 值的修改
```

```markdown
方案一：
1 先创建一个要修改的同名（指的就是 key）的 Cookie 对象
2 在构造器，同时赋于新的 Cookie 值。
3 调用 response.addCookie( Cookie );
```
```java
// 方案一：
// 1、先创建一个要修改的同名的 Cookie 对象
// 2、在构造器，同时赋于新的 Cookie 值。
Cookie cookie = new Cookie("key1","newValue1");
// 3、调用 response.addCookie( Cookie ); 通知 客户端 保存修改
resp.addCookie(cookie);
```
```markdown
方案二：
1 先查找到需要修改的 Cookie 对象
2 调用 setValue()方法赋于新的 Cookie 值。
3 调用 response.addCookie()通知客户端保存修改
```
```java
// 方案二：
// 1、先查找到需要修改的 Cookie 对象
Cookie cookie = CookieUtils.findCookie("key2", req.getCookies());
        if (cookie != null) {
// 2、调用 setValue()方法赋于新的 Cookie 值。
        cookie.setValue("newValue2");
// 3、调用 response.addCookie()通知客户端保存修改
        resp.addCookie(cookie);
        }
```
```markdown
浏览器查看 Cookie
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020061221265375.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612212709642.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
Cookie 生命控制
```

```f) Cookie 生命控制
Cookie 的生命控制指的是如何管理 Cookie 什么时候被销毁（删除）
setMaxAge()
正数，表示在指定的秒数后过期
负数，表示浏览器一关，Cookie 就会被删除（默认值是-1）
零，表示马上删除 Cookie
```
```java
/**
 * 设置存活 1 个小时的 Cooie
 * @param req
 * @param resp
 * @throws ServletException
 * @throws IOException
 */
protected void life3600(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
        Cookie cookie = new Cookie("life3600", "life3600");
        cookie.setMaxAge(60 * 60); // 设置 Cookie 一小时之后被删除。无效
        resp.addCookie(cookie);
        resp.getWriter().write("已经创建了一个存活一小时的 Cookie");
        }
/**
 * 马上删除一个 Cookie
 * @param req
 * @param resp
 * @throws ServletException
 * @throws IOException
 */
protected void deleteNow(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
// 先找到你要删除的 Cookie 对象
        Cookie cookie = CookieUtils.findCookie("key4", req.getCookies());
        if (cookie != null) {
// 调用 setMaxAge(0);
        cookie.setMaxAge(0); // 表示马上删除，都不需要等待浏览器关闭
// 调用 response.addCookie(cookie);
        resp.addCookie(cookie);
        resp.getWriter().write("key4 的 Cookie 已经被删除");
        }
        }
/**
 * 默认的会话级别的 Cookie
 * @param req
 * @param resp
 * @throws ServletException
 * @throws IOException
 */
protected void defaultLife(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
        Cookie cookie = new Cookie("defalutLife","defaultLife");
        cookie.setMaxAge(-1);//设置存活时间
        resp.addCookie(cookie);
        }

```
```markdown
Cookie 有效路径 Path 的设置
```

```f) Cookie 生命控制
Cookie 的 path 属性可以有效的过滤哪些 Cookie 可以发送给服务器。哪些不发。
        path 属性是通过请求的地址来进行有效的过滤。
        CookieA path=/工程路径
        CookieB path=/工程路径/abc
        请求地址如下：
        http://ip:port/工程路径/a.html
        CookieA 发送
        CookieB 不发送
        http://ip:port/工程路径/abc/a.html
        CookieA 发送
        CookieB 发送
```
```java
protected void testPath(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
        Cookie cookie = new Cookie("path1", "path1");
// getContextPath() ===>>>> 得到工程路径
        cookie.setPath( req.getContextPath() + "/abc" ); // ===>>>> /工程路径/abc
        resp.addCookie(cookie);
        resp.getWriter().write("创建了一个带有 Path 路径的 Cookie");
        }
```
```markdown
Cookie 练习---免输入用户名登录
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110185159824.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
login.jsp 页面
```

```html
<form action="http://localhost:8080/13_cookie_session/loginServlet" method="get">
    用户名：<input type="text" name="username" value="${cookie.username.value}"> <br>
    密码：<input type="password" name="password"> <br>
    <input type="submit" value="登录">
</form>
```
```markdown
LoginServlet 程序
```


```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        if ("wzg168".equals(username) && "123456".equals(password)) {
//登录 成功
        Cookie cookie = new Cookie("username", username);
        cookie.setMaxAge(60 * 60 * 24 * 7);//当前 Cookie 一周内有效
        resp.addCookie(cookie);
        System.out.println("登录 成功");
        } else {
// 登录 失败
        System.out.println("登录 失败");
        }
}
```
### 19.2 Session会话
```markdown
什么是 Session 会话?
```

```markdown
1 Session 就一个接口（HttpSession）。
2 Session 就是会话。它是用来维护一个客户端和服务器之间关联的一种技术。
3 每个客户端都有自己的一个 Session 会话。
4 Session 会话中，我们经常用来保存用户登录之后的信息。
```
```markdown
如何创建 Session 和获取(id 号,是否为新)
```

```markdown
如何创建和获取 Session。它们的 API 是一样的。
request.getSession()
第一次调用是：创建 Session 会话
之后调用都是：获取前面创建好的 Session 会话对象。
isNew(); 判断到底是不是刚创建出来的（新的）
true 表示刚创建
false 表示获取之前创建
每个会话都有一个身份证号。也就是 ID 值。而且这个 ID 是唯一的。
getId() 得到 Session 的会话 id 值。
```
```markdown
Session 域数据的存取
```

```java
/**
 * 往 Session 中保存数据
 * @param req
 * @param resp
 * @throws ServletException
 * @throws IOException
 */
protected void setAttribute(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
        req.getSession().setAttribute("key1", "value1");
        resp.getWriter().write("已经往 Session 中保存了数据");
        }
/**
 * 获取 Session 域中的数据
 * @param req
 * @param resp
 * @throws ServletException
 * @throws IOException
 */
protected void getAttribute(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
        Object attribute = req.getSession().getAttribute("key1");
        resp.getWriter().write("从 Session 中获取出 key1 的数据是：" + attribute);
        }
```
```markdown
Session 生命周期控制
```

```java
public void setMaxInactiveInterval(int interval) 设置 Session 的超时时间（以秒为单位），超过指定的时长，Session
就会被销毁。
值为正数的时候，设定 Session 的超时时长。
负数表示永不超时（极少使用）
public int getMaxInactiveInterval()获取 Session 的超时时间
public void invalidate() 让当前 Session 会话马上超时无效。
Session 默认的超时时长是多少！
Session 默认的超时时间长为 30 分钟。
因为在 Tomcat 服务器的配置文件 web.xml中默认有以下的配置，它就表示配置了当前 Tomcat 服务器下所有的 Session
超时配置默认时长为：30 分钟。
<session-config>
<session-timeout>30</session-timeout>
</session-config>
```
```markdown
如果说。你希望你的 web 工程，默认的 Session 的超时时长为其他时长。你可以在你自己的 web.xml 配置文件中做
以上相同的配置。就可以修改你的 web 工程所有 Seession 的默认超时时长。
```
```xml
<!--表示当前 web 工程。创建出来 的所有 Session 默认是 20 分钟 超时时长-->
<session-config>
<session-timeout>20</session-timeout>
</session-config>
```
```markdown
如果你想只修改个别 Session 的超时时长。就可以使用上面的 API。setMaxInactiveInterval(int interval)来进行单独的设
置。
session.setMaxInactiveInterval(int interval)单独设置超时时长。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200613013649702.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```java
protected void life3(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
// 先获取 Session 对象
        HttpSession session = req.getSession();
// 设置当前 Session3 秒后超时
        session.setMaxInactiveInterval(3);
        resp.getWriter().write("当前 Session 已经设置为 3 秒后超时");
}
```
```markdown
Session 马上被超时示例
```

```java
protected void deleteNow(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
// 先获取 Session 对象
        HttpSession session = req.getSession();
// 让 Session 会话马上超时
        session.invalidate();
        resp.getWriter().write("Session 已经设置为超时（无效）");
}
```
```markdown
浏览器和 Session 之间关联的技术内幕
```

```markdown
Session 技术，底层其实是基于 Cookie 技术来实现的。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/202011101903434.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 19.3 项目第六阶段
#### 19.3.1 登陆---显示用户名
```markdown
UserServlet 程序中保存用户登录的信息
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110191420146.png#pic_center)
```markdown
修改 login_succuess_menu.jsp
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110191450393.png#pic_center)
```markdown
还要修改首页 index.jsp 页面的菜单 ：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110191530436.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
#### 19.3.2 登出---注销用户
```markdown
1 销毁 Session 中用户登录的信息（或者销毁 Session）
2 重定向到首页（或登录页面）。
```
```markdown
UserServlet 程序中添加 logout 方法
```

```java
/**
 * 注销
 * @param req
 * @param resp
 * @throws ServletException
 * @throws IOException
 */
protected void logout(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
// 1、销毁 Session 中用户登录的信息（或者销毁 Session）
        req.getSession().invalidate();
// 2、重定向到首页（或登录页面）。
        resp.sendRedirect(req.getContextPath());
}
```
```markdown
修改【注销】的菜单地址
```

```html
<a href="userServlet?action=logout">注销</a>
```
#### 19.3.3 表单重复提交之-----验证码
```markdown
表单重复提交有三种常见的情况：
1 提交完表单。服务器使用请求转来进行页面跳转。这个时候，用户按下功能键 F5，就会发起最后一次的请求。
造成表单重复提交问题。解决方法：使用重定向来进行跳转
2 用户正常提交服务器，但是由于网络延迟等原因，迟迟未收到服务器的响应，这个时候，用户以为提交失败，
就会着急，然后多点了几次提交操作，也会造成表单重复提交。
3 用户正常提交服务器。服务器也没有延迟，但是提交完成后，用户回退浏览器。重新提交。也会造成表单重复
提交
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200613014006246.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 19.3.4 谷歌 kaptcha图片验证码的使用
```markdown
谷歌验证码 kaptcha 使用步骤如下：
1 导入谷歌验证码的 jar 包
kaptcha-2.3.2.jar
2 在 web.xml 中去配置用于生成验证码的 Servlet 程序
```
```xml
<servlet>
    <servlet-name>KaptchaServlet</servlet-name>
    <servlet-class>com.google.code.kaptcha.servlet.KaptchaServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>KaptchaServlet</servlet-name>
    <url-pattern>/kaptcha.jpg</url-pattern>
</servlet-mapping>
```
```markdown
3 在表单中使用 img 标签去显示验证码图片并使用它
```

```html
<form action="http://localhost:8080/tmp/registServlet" method="get">
    用户名：<input type="text" name="username"> <br>
    验证码：<input type="text" style="width: 80px;" name="code">
    <img src="http://localhost:8080/tmp/kaptcha.jpg" alt="" style="width: 100px; height: 28px;"> <br>
    <input type="submit" value="登录">
</form>
```
```markdown
4 在服务器获取谷歌生成的验证码和客户端发送过来的验证码比较使用。
```

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
// 获取 Session 中的验证码
        String token = (String) req.getSession().getAttribute(KAPTCHA_SESSION_KEY);
// 删除 Session 中的验证码
        req.getSession().removeAttribute(KAPTCHA_SESSION_KEY);
        String code = req.getParameter("code");
// 获取用户名
        String username = req.getParameter("username");
        if (token != null && token.equalsIgnoreCase(code)) {
        System.out.println("保存到数据库：" + username);
        resp.sendRedirect(req.getContextPath() + "/ok.jsp");
        } else {
        System.out.println("请不要重复提交表单");
        }
}
```
```markdown
切换验证码
```

```java
// 给验证码的图片，绑定单击事件
$("#code_img").click(function () {
// 在事件响应的 function 函数中有一个 this 对象。这个 this 对象，是当前正在响应事件的 dom 对象
// src 属性表示验证码 img 标签的 图片路径。它可读，可写
// alert(this.src);
this.src = "${basePath}kaptcha.jpg?d=" + new Date();
});
```
## 20 书城项目第七、八阶段
### 20.1 项目第七阶段：购物车
#### 20.1.1 购物车模块分析
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110223419243.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
#### 20.1.2 购物车模型编写
##### 20.1.2.1 购物车模型：

```java
/**
 * 购物车的商品项
 */
public class CartItem {
    private Integer id;
    private String name;
    private Integer count;
    private BigDecimal price;
    private BigDecimal totalPrice;
    /**
     * 购物车对象
     */
}
    public class Cart {
// private Integer totalCount;
// private BigDecimal totalPrice;
        /**
         * key 是商品编号，
         * value，是商品信息
         */
        private Map<Integer,CartItem> items = new LinkedHashMap<Integer,CartItem>();
        /**
         * 添加商品项
         *
         * @param cartItem
         */
        public void addItem(CartItem cartItem) {
// 先查看购物车中是否已经添加过此商品，如果已添加，则数量累加，总金额更新，如果没有添加过，直接放到
            集合中即可
            CartItem item = items.get(cartItem.getId());
            if (item == null) {
// 之前没添加过此商品
                items.put(cartItem.getId(), cartItem);
            } else {
// 已经 添加过的情况
                item.setCount( item.getCount() + 1 ); // 数量 累加
                item.setTotalPrice( item.getPrice().multiply(new BigDecimal( item.getCount() )) ); // 更
                新总金额
            }
        }
        /**
         * 删除商品项
         */
        public void deleteItem(Integer id) {
            items.remove(id);
        }
        /**
         * 清空购物车
         */
        public void clear() {
            items.clear();
        }
        /**
         * 修改商品数量
         */
        public void updateCount(Integer id,Integer count) {
// 先查看购物车中是否有此商品。如果有，修改商品数量，更新总金额
            CartItem cartItem = items.get(id);
            if (cartItem != null) {
                cartItem.setCount(count);// 修改商品数量
                cartItem.setTotalPrice( cartItem.getPrice().multiply(new
                        BigDecimal( cartItem.getCount() )) ); // 更新总金额
            }
        }
        public Integer getTotalCount() {
            Integer totalCount = 0;
            for (Map.Entry<Integer,CartItem>entry : items.entrySet()) {
                totalCount += entry.getValue().getCount();
            }
            return totalCount;
        }
        public BigDecimal getTotalPrice() {
            BigDecimal totalPrice = new BigDecimal(0);
            for (Map.Entry<Integer,CartItem>entry : items.entrySet()) {
                totalPrice = totalPrice.add(entry.getValue().getTotalPrice());
            }
            return totalPrice;
        }
        public Map<Integer, CartItem> getItems() {
            return items;
        }
        public void setItems(Map<Integer, CartItem> items) {
            this.items = items;
        }
        @Override
        public String toString() {
            return "Cart{" +
                    "totalCount=" + getTotalCount() +
                    ", totalPrice=" + getTotalPrice() +
                    ", items=" + items +
                    '}';
        }
    }
```
##### 20.1.2.2 购物车的测试

```java
public class CartTest {
    @Test
    public void addItem() {
        Cart cart = new Cart();
        cart.addItem(new CartItem(1, "java从入门到精通", 1, new BigDecimal(1000),new BigDecimal(1000)));
        cart.addItem(new CartItem(1, "java从入门到精通", 1, new BigDecimal(1000),new BigDecimal(1000)));
        cart.addItem(new CartItem(2, "数据结构与算法", 1, new BigDecimal(100),new BigDecimal(100)));
        System.out.println(cart);
    }
    @Test
    public void deleteItem() {
        Cart cart = new Cart();
        cart.addItem(new CartItem(1, "java从入门到精通", 1, new BigDecimal(1000),new BigDecimal(1000)));
        cart.addItem(new CartItem(1, "java从入门到精通", 1, new BigDecimal(1000),new BigDecimal(1000)));
        cart.addItem(new CartItem(2, "数据结构与算法", 1, new BigDecimal(100),new BigDecimal(100)));
        cart.deleteItem(1);
        System.out.println(cart);
    }
    @Test
    public void clear() {
        Cart cart = new Cart();
        cart.addItem(new CartItem(1, "java从入门到精通", 1, new BigDecimal(1000),new BigDecimal(1000)));
        cart.addItem(new CartItem(1, "java从入门到精通", 1, new BigDecimal(1000),new BigDecimal(1000)));
        cart.addItem(new CartItem(2, "数据结构与算法", 1, new BigDecimal(100),new BigDecimal(100)));
        cart.deleteItem(1);
        cart.clear();
        System.out.println(cart);
    }
    @Test
    public void updateCount() {
        Cart cart = new Cart();
        cart.addItem(new CartItem(1, "java从入门到精通", 1, new BigDecimal(1000),new BigDecimal(1000)));
        cart.addItem(new CartItem(1, "java从入门到精通", 1, new BigDecimal(1000),new BigDecimal(1000)));
        cart.addItem(new CartItem(2, "数据结构与算法", 1, new BigDecimal(100),new BigDecimal(100)));
        cart.deleteItem(1);
        cart.clear();
        cart.addItem(new CartItem(1, "java从入门到精通", 1, new BigDecimal(1000),new BigDecimal(1000)));
        cart.updateCount(1, 10);
        System.out.println(cart);
    }
}
```
##### 20.1.2.3 加入购物车功能的实现
```markdown
CartServlet 程序中的代码
```


```java
/**
 * 加入购物车
 * @param req
 * @param resp
 * @throws ServletException
 * @throws IOException
 */
protected void addItem(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
// 获取请求的参数 商品编号
        int id = WebUtils.parseInt(req.getParameter("id"), 0);
// 调用 bookService.queryBookById(id):Book 得到图书的信息
        Book book = bookService.queryBookById(id);
// 把图书信息，转换成为 CartItem 商品项
        CartItem cartItem = new CartItem(book.getId(),book.getName(),1,book.getPrice(),book.getPrice());
// 调用 Cart.addItem(CartItem);添加商品项
        Cart cart = (Cart) req.getSession().getAttribute("cart");
        if (cart == null) {
        cart = new Cart();
        req.getSession().setAttribute("cart",cart);
        }
        cart.addItem(cartItem);
        System.out.println(cart);
        System.out.println("请求头 Referer 的值：" + req.getHeader("Referer"));
// 重定向回原来商品所在的地址页面
        resp.sendRedirect(req.getHeader("Referer"));
        }
```
```markdown
index.jsp 页面 js 的代码
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020111022585411.png#pic_center)

```html
<Script type="text/javascript">
    $(function () {
        // 给加入购物车按钮绑定单击事件
        $("button.addToCart").click(function () {
            /**
            * 在事件响应的 function 函数 中，有一个 this 对象，这个 this 对象，是当前正在响应事件的 dom 对象
            * @type {jQuery}
            */
            var bookId = $(this).attr("bookId");
            location.href = "http://localhost:8080/book/cartServlet?action=addItem&id=" + bookId;
        });
    });
</Script>
```

```markdown
图解说明，如何跳回添加商品的页面
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110224521907.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
##### 20.1.2.4 购物车的展示
```html
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
        <!DOCTYPE html>
        <html>

        <head>
            <meta charset="UTF-8">
            <title>购物车</title>
            <%-- 静态包含 base 标签、css 样式、jQuery 文件 --%>
                <%@ include file="/pages/common/head.jsp" %>
        </head>

        <body>
            <div id="header">
                <img class="logo_img" alt="" src="static/img/logo.gif">
                <span class="wel_word">购物车</span>
                <%--静态包含，登录 成功之后的菜单 --%>
                    <%@ include file="/pages/common/login_success_menu.jsp" %>
            </div>
            <div id="main">
                <table>
                    <tr>
                        <td>商品名称</td>
                        <td>数量</td>
                        <td>单价</td>
                        <td>金额</td>
                        <td>操作</td>
                    </tr>
                    <c:if test="${empty sessionScope.cart.items}">
                        <%--如果购物车空的情况--%>
                            <tr>
                                <td colspan="5"><a href="index.jsp">亲，当前购物车为空！快跟小伙伴们去浏览商品吧！！！</a>
                                </td>
                            </tr>
                    </c:if>
                    <c:if test="${not empty sessionScope.cart.items}">
                        <%--如果购物车非空的情况--%>
                            <c:forEach items="${sessionScope.cart.items}" var="entry">
                                <tr>
                                    <td>${entry.value.name}</td>
                                    <td>${entry.value.count}</td>
                                    <td>${entry.value.price}</td>
                                    <td>${entry.value.totalPrice}</td>
                                    <td><a href="#">删除</a></td>
                                </tr>
                            </c:forEach>
                    </c:if>
                </table>
                <%--如果购物车非空才输出页面的内容--%>
                    <c:if test="${not empty sessionScope.cart.items}">
                        <div class="cart_info">
                            <span class="cart_span">购物车中共有<span
                                    class="b_count">${sessionScope.cart.totalCount}</span>件商品</span>
                            <span class="cart_span">总金额<span
                                    class="b_price">${sessionScope.cart.totalPrice}</span>元</span>
                            <span class="cart_span"><a href="#">清空购物车</a></span>
                            <span class="cart_span"><a href="pages/cart/checkout.jsp">去结账</a></span>
                        </div>
                    </c:if>
            </div>
            <%--静态包含页脚内容--%>
                <%@include file="/pages/common/footer.jsp" %>
        </body>

        </html>
```
##### 20.1.2.5 删除购物车商品项
```markdown
CartServlet 程序
```

```java
/**
 * 删除商品项
 * @param req
 * @param resp
 * @throws ServletException
 * @throws IOException
 */
protected void deleteItem(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException{
// 获取商品编号
        int id = WebUtils.parseInt(req.getParameter("id"), 0);
// 获取购物车对象
        Cart cart = (Cart) req.getSession().getAttribute("cart");
        if (cart != null) {
// 删除 了购物车商品项
        cart.deleteItem(id);
// 重定向回原来购物车展示页面
        resp.sendRedirect(req.getHeader("Referer"));
        }
        }
```
```markdown
购物车/pages/cart/cart.jsp 页面的代码：
删除的请求地址
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110230922344.png#pic_center)
```markdown
删除的确认提示操作
```
```html
<script type="text/javascript">
    $(function () {
        // 给 【删除】绑定单击事件
        $("a.deleteItem").click(function () {
            return confirm("你确定要删除【" + $(this).parent().parent().find("td:first").text() + "】吗?")
        });
    });
</script>
```
##### 20.1.2.6 清空购物车
```markdown
CartServlet 程序
```

```java
/**
 * 清空购物车
 * @param req
 * @param resp
 * @throws ServletException
 * @throws IOException
 */
protected void clear(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException{
// 1 获取购物车对象
        Cart cart = (Cart) req.getSession().getAttribute("cart");
        if (cart != null) {
// 清空购物车
        cart.clear();
// 重定向回原来购物车展示页面
        resp.sendRedirect(req.getHeader("Referer"));
        }
        }
```
```markdown
cart.jsp 页面的内容
```

```markdown
给清空购物车添加请求地址，和添加 id 属性：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110231244482.png#pic_center)
```markdown
清空的确认提示操作：
```
```html
// 给清空购物车绑定单击事件
$("#clearCart").click(function () {
return confirm("你确定要清空购物车吗?");
})
```
##### 20.1.2.7 修改购物车商品数量
```markdown
CartServlet 程序
```

```java
/**
 * 修改商品数量
 * @param req
 * @param resp
 * @throws ServletException
 * @throws IOException
 */
protected void updateCount(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException{
// 获取请求的参数 商品编号 、商品数量
        int id = WebUtils.parseInt(req.getParameter("id"),0);
        int count = WebUtils.parseInt(req.getParameter("count"), 1);
// 获取 Cart 购物车对象
        Cart cart = (Cart) req.getSession().getAttribute("cart");
        if (cart != null) {
// 修改商品数量
        cart.updateCount(id,count);
// 重定向回原来购物车展示页面
        resp.sendRedirect(req.getHeader("Referer"));
        }
        }
```
```markdown
修改 pages/cart/cart.jsp 购物车页面：
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110231845347.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
修改商品数量 js 代码：
```

```java
// 给输入框绑定 onchange 内容发生改变事件
$(".updateCount").change(function () {
// 获取商品名称
var name = $(this).parent().parent().find("td:first").text();
var id = $(this).attr('bookId');
// 获取商品数量
var count = this.value;
if ( confirm("你确定要将【" + name + "】商品修改数量为：" + count + " 吗?") ) {
//发起请求。给服务器保存修改
location.href =
"http://localhost:8080/book/cartServlet?action=updateCount&count="+count+"&id="+id;
} else {
// defaultValue 属性是表单项 Dom 对象的属性。它表示默认的 value 属性值。
this.value = this.defaultValue;
}
});
```
##### 20.1.2.8 首页，购物车数据回显
```markdown
在添加商品到购物车的时候，保存最后一个添加的商品名称
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110232111855.png#pic_center)
```markdown
在 pages/client/index.jsp 页面中输出购物车信息
```
```html
<div style="text-align: center">
    <c:if test="${empty sessionScope.cart.items}">
        <%--购物车为空的输出--%>
            <span> </span>
            <div>
                <span style="color: red">当前购物车为空</span>
            </div>
    </c:if>
    <c:if test="${not empty sessionScope.cart.items}">
        <%--购物车非空的输出--%>
            <span>您的购物车中有 ${sessionScope.cart.totalCount} 件商品</span>
            <div>
                您刚刚将<span style="color: red">${sessionScope.lastName}</span>加入到了购物车中
            </div>
    </c:if>
</div>
```
## 21 项目第八阶段：订单
### 21.1 订单模块的分析
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110233637934.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 21.2 订单模块的实现
#### 21.2.1 创建订单模块的数据库表
```sql
use book;
create table t_order(
`order_id` varchar(50) primary key,
`create_time` datetime,
`price` decimal(11,2),
`status` int,
`user_id` int,
foreign key(`user_id`) references t_user(`id`)
);
create table t_order_item(
`id` int primary key auto_increment,
`name` varchar(100),
`count` int,
`price` decimal(11,2),
`total_price` decimal(11,2),
`order_id` varchar(50),
foreign key(`order_id`) references t_order(`order_id`)
);
```
#### 21.2.2 创建订单模块的数据模型
```java
/**
 * 订单
 */
public class Order {
    private String orderId;
    private Date createTime;
    private BigDecimal price;
    // 0 未发货，1 已发货，2 表示已签收
    private Integer status = 0;
    private Integer userId;


}
    /**
     * 订单项
     */
public class OrderItem {
    private Integer id;
    private String name;
    private Integer count;
    private BigDecimal price;
    private BigDecimal totalPrice;
    private String orderId;
}
```
#### 21.2.3 编写订单模块的Dao程序和测试
```markdown
OrderDao 接口
```

```java
public interface OrderDao {
    public int saveOrder(Order order);
}
```
```markdown
OrderDao 实现
```

```java
public class OrderDaoImpl extends BaseDao implements OrderDao {
    @Override
    public int saveOrder(Order order) {
        String sql = "insert into t_order(`order_id`,`create_time`,`price`,`status`,`user_id`)
        values(?,?,?,?,?)";
        return
                update(sql,order.getOrderId(),order.getCreateTime(),order.getPrice(),order.getStatus(),order.getUs
                        erId());
    }
}
```
```markdown
OrderItemDao 接口
```

```java
public interface OrderItemDao {
    public int saveOrderItem(OrderItem orderItem);
}
```
```markdown
OrderItemDao 实现
```

```java
public class OrderItemDaoImpl extends BaseDao implements OrderItemDao {
    @Override
    public int saveOrderItem(OrderItem orderItem) {
        String sql = "insert into t_order_item(`name`,`count`,`price`,`total_price`,`order_id`)
        values(?,?,?,?,?)";
        return
                update(sql,orderItem.getName(),orderItem.getCount(),orderItem.getPrice(),orderItem.getTotalPrice(),
                        orderItem.getOrderId());
    }
}
```
```markdown
测试
```

```java
public class OrderDaoTest {
    @Test
    public void saveOrder() {
        OrderDao orderDao = new OrderDaoImpl();
        orderDao.saveOrder(new Order("1234567891",new Date(),new BigDecimal(100),0, 1));
    }
}
public class OrderItemDaoTest {
    @Test
    public void saveOrderItem() {
        OrderItemDao orderItemDao = new OrderItemDaoImpl();
        orderItemDao.saveOrderItem(new OrderItem(null,"java 从入门到精通", 1,new BigDecimal(100),new
                BigDecimal(100),"1234567890"));
        orderItemDao.saveOrderItem(new OrderItem(null,"javaScript 从入门到精通", 2,new
                BigDecimal(100),new BigDecimal(200),"1234567890"));
        orderItemDao.saveOrderItem(new OrderItem(null,"Netty 入门", 1,new BigDecimal(100),new
                BigDecimal(100),"1234567890"));
    }
}
```
#### 21.2.4 编写订单模块的Service和测试
```markdown
OrderService 接口
```

```java
public interface OrderService {
    public String createOrder(Cart cart,Integer userId);
}
```
```markdown
OrderService 实现类
```

```java
public class OrderServiceImpl implements OrderService {
    private OrderDao orderDao = new OrderDaoImpl();
    private OrderItemDao orderItemDao = new OrderItemDaoImpl();
    @Override
    public String createOrder(Cart cart, Integer userId) {
// 订单号===唯一性
        String orderId = System.currentTimeMillis()+""+userId;
// 创建一个订单对象
        Order order = new Order(orderId,new Date(),cart.getTotalPrice(), 0,userId);
// 保存订单
        orderDao.saveOrder(order);
// 遍历购物车中每一个商品项转换成为订单项保存到数据库
        for (Map.Entry<Integer, CartItem>entry : cart.getItems().entrySet()){
// 获取每一个购物车中的商品项
            CartItem cartItem = entry.getValue();
// 转换为每一个订单项
            OrderItem orderItem = new
                    OrderItem(null,cartItem.getName(),cartItem.getCount(),cartItem.getPrice(),cartItem.getTotalPrice(),
                    orderId);
// 保存订单项到数据库
            orderItemDao.saveOrderItem(orderItem);
        }
// 清空购物车
        cart.clear();
        return orderId;
    }
}
```
```markdown
测试
```

```java
public class OrderServiceTest {
    @Test
    public void createOrder() {
        Cart cart = new Cart();
        cart.addItem(new CartItem(1, "java从入门到精通", 1, new BigDecimal(1000),new BigDecimal(1000)));
        cart.addItem(new CartItem(1, "java从入门到精通", 1, new BigDecimal(1000),new BigDecimal(1000)));
        cart.addItem(new CartItem(2, "数据结构与算法", 1, new BigDecimal(100),new BigDecimal(100)));
        OrderService orderService = new OrderServiceImpl();
        System.out.println( "订单号是：" + orderService.createOrder(cart, 1) );
    }
}
```
#### 21.2.5 编写订单模块的web层和页面联调
```markdown
修改 OrderService 程序
```

```java
public class OrderServiceImpl implements OrderService {
    private OrderDao orderDao = new OrderDaoImpl();
    private OrderItemDao orderItemDao = new OrderItemDaoImpl();
    private BookDao bookDao = new BookDaoImpl();
    @Override
    public String createOrder(Cart cart, Integer userId) {
// 订单号===唯一性
        String orderId = System.currentTimeMillis()+""+userId;
// 创建一个订单对象
        Order order = new Order(orderId,new Date(),cart.getTotalPrice(), 0,userId);
// 保存订单
        orderDao.saveOrder(order);
// 遍历购物车中每一个商品项转换成为订单项保存到数据库
        for (Map.Entry<Integer, CartItem>entry : cart.getItems().entrySet()){
// 获取每一个购物车中的商品项
            CartItem cartItem = entry.getValue();
// 转换为每一个订单项
            OrderItem orderItem = new
                    OrderItem(null,cartItem.getName(),cartItem.getCount(),cartItem.getPrice(),cartItem.getTotalPrice(),
                    orderId);
// 保存订单项到数据库
            orderItemDao.saveOrderItem(orderItem);
// 更新库存和销量
            Book book = bookDao.queryBookById(cartItem.getId());
            book.setSales( book.getSales() + cartItem.getCount() );
            book.setStock( book.getStock() - cartItem.getCount() );
            bookDao.updateBook(book);
        }
// 清空购物车
        cart.clear();
        return orderId;
    }
}
```
```markdown
OrderServlet 程序
```

```java
public class OrderServlet extends BaseServlet {
    private OrderService orderService = new OrderServiceImpl();
    /**
     * 生成订单
     *
     * @param req
     * @param resp
     * @throws ServletException
     * @throws IOException
     */
    protected void createOrder(HttpServletRequest req, HttpServletResponse resp) throws
            ServletException, IOException {
// 先获取 Cart 购物车对象
        Cart cart = (Cart) req.getSession().getAttribute("cart");
// 获取 Userid
        User loginUser = (User) req.getSession().getAttribute("user");
        if (loginUser == null) {
            req.getRequestDispatcher("/pages/user/login.jsp").forward(req,resp);
            return;
        }
        Integer userId = loginUser.getId();
// 调用 orderService.createOrder(Cart,Userid);生成订单
        String orderId = orderService.createOrder(cart, userId);
// req.setAttribute("orderId", orderId);
// 请求转发到/pages/cart/checkout.jsp
// req.getRequestDispatcher("/pages/cart/checkout.jsp").forward(req, resp);
        req.getSession().setAttribute("orderId",orderId);
        resp.sendRedirect(req.getContextPath()+"/pages/cart/checkout.jsp");
    }
}
```
```markdown
修改 pages/cart/cart.jsp 页面，结账的请求地址
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110235050937.png#pic_center)
```markdown
修改 pages/cart/checkout.jsp 页面，输出订单号
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201110235118528.png#pic_center)

## 22 Filter 过滤器
### 22.1 Filter什么是过滤器

```markdown
1 Filter 过滤器它是 JavaWeb 的三大组件之一。三大组件分别是：Servlet 程序、Listener 监听器、Filter 过滤器
2 Filter 过滤器它是 JavaEE 的规范。也就是接口
3 Filter 过滤器它的作用是：拦截请求，过滤响应。
拦截请求常见的应用场景有：
1 权限检查
2 日记操作
3 事务管理
……等等
```
### 22.2 Filter的初体验
```markdown
要求：在你的 web 工程下，有一个 admin 目录。这个 admin 目录下的所有资源（html 页面、jpg 图片、jsp 文件、等等）都必
须是用户登录之后才允许访问。
思考：根据之前我们学过内容。我们知道，用户登录之后都会把用户登录的信息保存到 Session 域中。所以要检查用户是否
登录，可以判断 Session 中否包含有用户登录的信息即可！！！
```
```html
<%
        Object user = session.getAttribute("user");
// 如果等于 null，说明还没有登录
        if (user == null) {
        request.getRequestDispatcher("/login.jsp").forward(request,response);
        return;
        }
%>
```
```markdown
Filter 的工作流程图
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200613080355429.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
Filter 的代码
```

```java
public class AdminFilter implements Filter {
    /**
     * doFilter 方法，专门用于拦截请求。可以做权限检查
     */
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain
            filterChain) throws IOException, ServletException {
        HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
        HttpSession session = httpServletRequest.getSession();
        Object user = session.getAttribute("user");
// 如果等于 null，说明还没有登录
        if (user == null) {
            servletRequest.getRequestDispatcher("/login.jsp").forward(servletRequest,servletResponse);
            return;
        } else {
// 让程序继续往下访问用户的目标资源
            filterChain.doFilter(servletRequest,servletResponse);
        }
    }
}
```
```markdown
web.xml 中的配置
```

```java
<!--filter 标签用于配置一个 Filter 过滤器-->
<filter>
    <!--给 filter 起一个别名-->
    <filter-name>AdminFilter</filter-name>
    <!--配置 filter 的全类名-->
    <filter-class>com.yxj.filter.AdminFilter</filter-class>
</filter>
<!--filter-mapping 配置 Filter 过滤器的拦截路径-->
<filter-mapping>
    <!--filter-name 表示当前的拦截路径给哪个 filter 使用-->
    <filter-name>AdminFilter</filter-name>
    <!--url-pattern 配置拦截路径
    / 表示请求地址为：http://ip:port/工程路径/ 映射到 IDEA 的 web 目录
    /admin/* 表示请求地址为：http://ip:port/工程路径/admin/*
    -->
    <url-pattern>/admin/*</url-pattern>
</filter-mapping>
```
```markdown
Filter 过滤器的使用步骤
```

```markdown
1 编写一个类去实现 Filter 接口
2 实现过滤方法 doFilter()
3 到 web.xml 中去配置 Filter 的拦截路径
```
```markdown
完整的用户登录
ogin.jsp 页面 == 登录表单
```

```html
这是登录页面。login.jsp 页面 <br>
<form action="http://localhost:8080/15_filter/loginServlet" method="get">
    用户名：<input type="text" name="username" /> <br>
    密 码：<input type="password" name="password" /> <br>
    <input type="submit" />
</form>
```
```markdown
LoginServlet 程序
```

```java
public class LoginServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
            IOException {
        resp.setContentType("text/html; charset=UTF-8");
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        if ("wzg168".equals(username) && "123456".equals(password)) {
            req.getSession().setAttribute("user",username);
            resp.getWriter().write("登录 成功！！！");
        } else {
            req.getRequestDispatcher("/login.jsp").forward(req,resp);
        }
    }
}
```
### 22.3 Filter的生命周期
```markdown
Filter 的生命周期包含几个方法
1 构造器方法
2 init 初始化方法
第 1，2 步，在 web 工程启动的时候执行（Filter 已经创建）
3 doFilter 过滤方法
第 3 步，每次拦截到请求，就会执行
4 destroy 销毁
第 4 步，停止 web 工程的时候，就会执行（停止 web 工程，也会销毁 Filter 过滤器）
```
### 22.4 FilterConfig类
```markdown
FilterConfig 类见名知义，它是 Filter 过滤器的配置文件类。
Tomcat 每次创建 Filter 的时候，也会同时创建一个 FilterConfig 类，这里包含了 Filter 配置文件的配置信息。
FilterConfig 类的作用是获取 filter 过滤器的配置内容
1 获取 Filter 的名称 filter-name 的内容
2 获取在 Filter 中配置的 init-param 初始化参数
3 获取 ServletContext 对象
```

```java
@Override
public void init(FilterConfig filterConfig) throws ServletException {
System.out.println("2.Filter 的 init(FilterConfig filterConfig)初始化");
// 1、获取 Filter 的名称 filter-name 的内容
System.out.println("filter-name 的值是：" + filterConfig.getFilterName());
// 2、获取在 web.xml 中配置的 init-param 初始化参数
System.out.println("初始化参数 username 的值是：" + filterConfig.getInitParameter("username"));
System.out.println("初始化参数 url 的值是：" + filterConfig.getInitParameter("url"));
// 3、获取 ServletContext 对象
System.out.println(filterConfig.getServletContext());
}
```
```markdown
web.xml 配置
```
```xml
<!--filter 标签用于配置一个 Filter 过滤器-->
<filter>
    <!--给 filter 起一个别名-->
    <filter-name>AdminFilter</filter-name>
    <!--配置 filter 的全类名-->
    <filter-class>com.yxj.filter.AdminFilter</filter-class>
    <init-param>
        <param-name>username</param-name>
        <param-value>root</param-value>
    </init-param>
    <init-param>
        <param-name>url</param-name>
        <param-value>jdbc:mysql://localhost3306/test</param-value>
    </init-param>
</filter>
```
### 22.5 FilterChain过滤器链
```markdown
Filter 过滤器
Chain 链，链条
FilterChain 就是过滤器链（多个过滤器如何一起工作）
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200613081536167.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 22.6 Filter的拦截路径
```xml
--精确匹配
<url-pattern>/target.jsp</url-pattern>
以上配置的路径，表示请求地址必须为：http://ip:port/工程路径/target.jsp


--目录匹配
<url-pattern>/admin/*</url-pattern>
以上配置的路径，表示请求地址必须为：http://ip:port/工程路径/admin/* --后缀名匹配
<url-pattern>*.html</url-pattern>
以上配置的路径，表示请求地址必须以.html 结尾才会拦截到
<url-pattern>*.do</url-pattern>
以上配置的路径，表示请求地址必须以.do 结尾才会拦截到
<url-pattern>*.action</url-pattern>
以上配置的路径，表示请求地址必须以.action 结尾才会拦截到
Filter 过滤器它只关心请求的地址是否匹配，不关心请求的资源是否存在！！！
```
## 23 书城第九阶段

###  23.1 使用 Filter 过滤器拦截/pages/manager/所有内容，实现权限检查
```markdown
Filter 代码
```

```java
public class ManagerFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
    }
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain
            filterChain) throws IOException, ServletException {
        HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
        Object user = httpServletRequest.getSession().getAttribute("user");
        if (user == null) {
            httpServletRequest.getRequestDispatcher("/pages/user/login.jsp").forward(servletRequest,servletRes
                    ponse);
        } else {
            filterChain.doFilter(servletRequest,servletResponse);
        }
    }
    @Override
    public void destroy() {
    }
}
```
```markdown
web.xml 中的配置
```

```xml
<filter>
    <filter-name>ManagerFilter</filter-name>
    <filter-class>com.yxj.filter.ManagerFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>ManagerFilter</filter-name>
    <url-pattern>/pages/manager/*</url-pattern>
    <url-pattern>/manager/bookServlet</url-pattern>
</filter-mapping>
```
###  23.2 ThreadLocal的使用
```markdown
ThreadLocal 的作用，它可以解决多线程的数据安全问题。
ThreadLocal 它可以给当前线程关联一个数据（可以是普通变量，可以是对象，也可以是数组，集合）
ThreadLocal 的特点：
1 ThreadLocal 可以为当前线程关联一个数据。（它可以像 Map 一样存取数据，key 为当前线程）
2 每一个 ThreadLocal 对象，只能为当前线程关联一个数据，如果要为当前线程关联多个数据，就需要使用多个
ThreadLocal 对象实例。
3 每个 ThreadLocal 对象实例定义的时候，一般都是 static 类型
4 ThreadLocal 中保存数据，在线程销毁后。会由 JVM 虚拟自动释放。
```
```markdown
测试类
```

```java
public class OrderService {
    public void createOrder(){
        String name = Thread.currentThread().getName();
        System.out.println("OrderService 当前线程[" + name + "]中保存的数据是：" +
                ThreadLocalTest.threadLocal.get());
        new OrderDao().saveOrder();
    }
}
public class OrderDao {
    public void saveOrder(){
        String name = Thread.currentThread().getName();
        System.out.println("OrderDao 当前线程[" + name + "]中保存的数据是：" +
                ThreadLocalTest.threadLocal.get());
    }
}
public class ThreadLocalTest {
    // public static Map<String,Object> data = new Hashtable<String,Object>();
    public static ThreadLocal<Object> threadLocal = new ThreadLocal<Object>();
    private static Random random = new Random();
    public static class Task implements Runnable {
        @Override
        public void run() {
// 在 Run 方法中，随机生成一个变量（线程要关联的数据），然后以当前线程名为 key 保存到 map 中
            Integer i = random.nextInt(1000);
// 获取当前线程名
            String name = Thread.currentThread().getName();
            System.out.println("线程["+name+"]生成的随机数是：" + i);
// data.put(name,i);
            threadLocal.set(i);
            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            new OrderService().createOrder();
// 在 Run 方法结束之前，以当前线程名获取出数据并打印。查看是否可以取出操作
// Object o = data.get(name);
            Object o = threadLocal.get();
            System.out.println("在线程["+name+"]快结束时取出关联的数据是：" + o);
        }
    }
    public static void main(String[] args) {
        for (int i = 0; i < 3; i++){
            new Thread(new Task()).start();
        }
    }
}
```
###  23.3 使用Filter和ThreadLocal组合管理事务
####  23.3.1 使用ThreadLocal来确保所有dao操作都在同一个Connection连接对象中完成
```markdown
原理分析图
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200613083516766.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
JdbcUtils 工具类的修改
```

```java
public class JdbcUtils {
private static DruidDataSource dataSource;
private static ThreadLocal<Connection> conns = new ThreadLocal<Connection>();
static {
try {
Properties properties = new Properties();
// 读取 jdbc.properties 属性配置文件
InputStream inputStream =
JdbcUtils.class.getClassLoader().getResourceAsStream("jdbc.properties");
// 从流中加载数据
properties.load(inputStream);
// 创建 数据库连接 池
dataSource = (DruidDataSource) DruidDataSourceFactory.createDataSource(properties);
} catch (Exception e) {
e.printStackTrace();
}
}
/**
* 获取数据库连接池中的连接
* @return 如果返回 null,说明获取连接失败<br/>有值就是获取连接成功
        */
public static Connection getConnection(){
        Connection conn = conns.get();
        if (conn == null) {
        try {
        conn = dataSource.getConnection();//从数据库连接池中获取连接
        conns.set(conn); // 保存到 ThreadLocal 对象中，供后面的 jdbc 操作使用
        conn.setAutoCommit(false); // 设置为手动管理事务
        } catch (SQLException e) {
        e.printStackTrace();
        }
        }
        return conn;
        }
/**
 * 提交事务，并关闭释放连接
 */
public static void commitAndClose(){
        Connection connection = conns.get();
        if (connection != null) { // 如果不等于 null，说明 之前使用过连接，操作过数据库
        try {
        connection.commit(); // 提交 事务
        } catch (SQLException e) {
        e.printStackTrace();
        } finally {
        try {
        connection.close(); // 关闭连接，资源资源
        } catch (SQLException e) {
        e.printStackTrace();
        }
        }
        }
// 一定要执行 remove 操作，否则就会出错。（因为 Tomcat 服务器底层使用了线程池技术）
        conns.remove();
        }
/**
 * 回滚事务，并关闭释放连接
 */
public static void rollbackAndClose(){
        Connection connection = conns.get();
        if (connection != null) { // 如果不等于 null，说明 之前使用过连接，操作过数据库
        try {
        connection.rollback();//回滚事务
        } catch (SQLException e) {
        e.printStackTrace();
        } finally {
        try {
        connection.close(); // 关闭连接，资源资源
        } catch (SQLException e) {
        e.printStackTrace();
        }
        }
        }
// 一定要执行 remove 操作，否则就会出错。（因为 Tomcat 服务器底层使用了线程池技术）
        conns.remove();
        }
/**
 * 关闭连接，放回数据库连接池
 * @param conn
public static void close(Connection conn){
if (conn != null) {
try {
conn.close();
} catch (SQLException e) {
e.printStackTrace();
}
}
} */
        }
```
```markdown
修改 BaseDao
```

```java
public abstract class BaseDao {
    //使用 DbUtils 操作数据库
    private QueryRunner queryRunner = new QueryRunner();
    /**
     * update() 方法用来执行：Insert\Update\Delete 语句
     *
     * @return 如果返回-1,说明执行失败<br/>返回其他表示影响的行数
     */
    public int update(String sql, Object... args) {
        System.out.println(" BaseDao 程序在[" +Thread.currentThread().getName() + "]中");
        Connection connection = JdbcUtils.getConnection();
        try {
            return queryRunner.update(connection, sql, args);
        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }
    }
    /**
     * 查询返回一个 javaBean 的 sql 语句
     *
     * @param type 返回的对象类型
     * @param sql 执行的 sql 语句
     * @param args sql 对应的参数值
     * @param <T> 返回的类型的泛型
     * @return
     */
    public <T> T queryForOne(Class<T> type, String sql, Object... args) {
        Connection con = JdbcUtils.getConnection();
        try {
            return queryRunner.query(con, sql, new BeanHandler<T>(type), args);
        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }
    }
    /**
     * 查询返回多个 javaBean 的 sql 语句
     *
     * @param type 返回的对象类型
     * @param sql 执行的 sql 语句
     * @param args sql 对应的参数值
     * @param <T> 返回的类型的泛型
     * @return
     */
    public <T> List<T> queryForList(Class<T> type, String sql, Object... args) {
        Connection con = JdbcUtils.getConnection();
        try {
            return queryRunner.query(con, sql, new BeanListHandler<T>(type), args);
        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }
    }
    /**
     * 执行返回一行一列的 sql 语句
     * @param sql 执行的 sql 语句
     * @param args sql 对应的参数值
     * @return
     */
    public Object queryForSingleValue(String sql, Object... args){
        Connection conn = JdbcUtils.getConnection();
        try {
            return queryRunner.query(conn, sql, new ScalarHandler(), args);
        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }
    }
}
```
####  23.3.2 使用Filter过滤器统一给所有的Service方法都加上 try-catch。来进行实现的管理
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020061308433397.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
Filter 类代码
```

```java
public class TransactionFilter implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain
            filterChain) throws IOException, ServletException {
        try {
            filterChain.doFilter(servletRequest,servletResponse);
            JdbcUtils.commitAndClose();// 提交事务
        } catch (Exception e) {
            JdbcUtils.rollbackAndClose();//回滚事务
            e.printStackTrace();
        }
    }
}
```
```markdown
在 web.xml 中的配置
```


```xml
<filter>
    <filter-name>TransactionFilter</filter-name>
    <filter-class>com.yxj.filter.TransactionFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>TransactionFilter</filter-name>
    <!-- /* 表示当前工程下所有请求 -->
    <url-pattern>/*</url-pattern>
</filter-mapping>
```
```markdown
一定要记得把 BaseServlet 中的异常往外抛给 Filter 过滤器
```

```java
public abstract class BaseServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
            IOException {
        doPost(req, resp);
    }
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
            IOException {
// 解决 post 请求中文乱码问题
// 一定要在获取请求参数之前调用才有效
        req.setCharacterEncoding("UTF-8");
        String action = req.getParameter("action");
        try {
// 获取 action 业务鉴别字符串，获取相应的业务 方法反射对象
            Method method = this.getClass().getDeclaredMethod(action, HttpServletRequest.class,
                    HttpServletResponse.class);
// System.out.println(method);
// 调用目标业务 方法
            method.invoke(this, req, resp);
        } catch (Exception e) {
            e.printStackTrace();
            throw new RuntimeException(e);// 把异常抛给 Filter 过滤器
        }
    }
}
```
####  23.3.3 将所有异常都统一交给Tomcat，让Tomcat展示友好的错误信息页面
```markdown
在 web.xml 中我们可以通过错误页面配置来进行管理。
```


```xml
<!--error-page 标签配置，服务器出错之后，自动跳转的页面-->
<error-page>
    <!--error-code 是错误类型-->
    <error-code>500</error-code>
    <!--location 标签表示。要跳转去的页面路径-->
    <location>/pages/error/error500.jsp</location>
</error-page>
<!--error-page 标签配置，服务器出错之后，自动跳转的页面-->
<error-page>
    <!--error-code 是错误类型-->
    <error-code>404</error-code>
    <!--location 标签表示。要跳转去的页面路径-->
    <location>/pages/error/error404.jsp</location>
</error-page>
```
## 24 JSON、AJAX、i18n
### 24.1 什么是 JSON
```markdown
JSON (JavaScript Object Notation) 是一种轻量级的数据交换格式。易于人阅读
和编写。同时也易于机器解析和生成。JSON
采用完全独立于语言的文本格式，而且很多语言都提供了对 json 的支持（包括 C, C++, C#, Java, JavaScript, Perl, Python
等）。 这样就使得 JSON 成为理想的数据交换格式。
json 是一种轻量级的数据交换格式。
轻量级指的是跟 xml 做比较。
数据交换指的是客户端和服务器之间业务数据的传递格式。
```
#### 24.1.1 JSON在JavaScript中的使用
##### 24.1.1.1 json的定义
```markdown
json 是由键值对组成，并且由花括号（大括号）包围。每个键由引号引起来，键和值之间使用冒号进行分隔，
多组键值对之间进行逗号进行分隔。
json 定义示例：
```
```java
var jsonObj = {
    "key1":12,
    "key2":"abc",
    "key3":true,
    "key4":[11,"arr",false],
    "key5":{
    "key5_1" : 551,
    "key5_2" : "key5_2_value"
    },
    "key6":[{
    "key6_1_1":6611,
    "key6_1_2":"key6_1_2_value"
    },{
    "key6_2_1":6621,
    "key6_2_2":"key6_2_2_value"
    }]
    };
```
##### 24.1.1.2 json的访问
```markdown
json 本身是一个对象。
json 中的 key 我们可以理解为是对象中的一个属性。
json 中的 key 访问就跟访问对象的属性一样： json 对象.key
```

```javascript
alert(typeof(jsonObj));// object json 就是一个对象
alert(jsonObj.key1); //12
alert(jsonObj.key2); // abc
alert(jsonObj.key3); // true
alert(jsonObj.key4);// 得到数组[11,"arr",false]
// json 中 数组值的遍历
for(var i = 0; i < jsonObj.key4.length; i++) {
alert(jsonObj.key4[i]);
}
alert(jsonObj.key5.key5_1);//551
alert(jsonObj.key5.key5_2);//key5_2_value
alert( jsonObj.key6 );// 得到 json 数组
// 取出来每一个元素都是 json 对象
var jsonItem = jsonObj.key6[0];
// alert( jsonItem.key6_1_1 ); //6611
alert( jsonItem.key6_1_2 ); //key6_1_2_value
```
##### 24.1.1.3 json的两个常用方法
```markdown
json 的存在有两种形式。
一种是：对象的形式存在，我们叫它 json 对象。
一种是：字符串的形式存在，我们叫它 json 字符串。
一般我们要操作 json 中的数据的时候，需要 json 对象的格式。
一般我们要在客户端和服务器之间进行数据交换的时候，使用 json 字符串。
JSON.stringify() 把 json 对象转换成为 json 字符串
JSON.parse() 把 json 字符串转换成为 json 对象
```

```javascript
// 把 json 对象转换成为 json 字符串
var jsonObjString = JSON.stringify(jsonObj); // 特别像 Java 中对象的 toString
alert(jsonObjString)
// 把 json 字符串。转换成为 json 对象
var jsonObj2 = JSON.parse(jsonObjString);
alert(jsonObj2.key1);// 12
alert(jsonObj2.key2);// abc
```
### 24.2 JSON在java中的使用
#### 24.2.1 javaBean和json的互转

```java
@Test
public void test1(){
        Person person = new Person(1,"国哥好帅!");
// 创建 Gson 对象实例
        Gson gson = new Gson();
// toJson 方法可以把 java 对象转换成为 json 字符串
        String personJsonString = gson.toJson(person);
        System.out.println(personJsonString);
// fromJson 把 json 字符串转换回 Java 对象
// 第一个参数是 json 字符串
// 第二个参数是转换回去的 Java 对象类型
        Person person1 = gson.fromJson(personJsonString, Person.class);
        System.out.println(person1);
}
```
#### 24.2.2 List和json的互转

```java
// 1.2.2、List 和 json 的互转
@Test
public void test2() {
        List<Person> personList = new ArrayList<>();
        personList.add(new Person(1, "国哥"));
        personList.add(new Person(2, "康师傅"));
        Gson gson = new Gson();
// 把 List 转换为 json 字符串
        String personListJsonString = gson.toJson(personList);
        System.out.println(personListJsonString);
        List<Person> list = gson.fromJson(personListJsonString, new PersonListType().getType());
        System.out.println(list);
        Person person = list.get(0);
        System.out.println(person);
}
```
#### 24.2.3 map和json的互转

```java
// 1.2.3、map 和 json 的互转
@Test
public void test3(){
        Map<Integer,Person> personMap = new HashMap<>();
        personMap.put(1, new Person(1, "国哥好帅"));
        personMap.put(2, new Person(2, "康师傅也好帅"));
        Gson gson = new Gson();
// 把 map 集合转换成为 json 字符串
        String personMapJsonString = gson.toJson(personMap);
        System.out.println(personMapJsonString);
// Map<Integer,Person> personMap2 = gson.fromJson(personMapJsonString, new
        PersonMapType().getType());
        Map<Integer,Person> personMap2 = gson.fromJson(personMapJsonString, new
        TypeToken<HashMap<Integer,Person>>(){}.getType());
        System.out.println(personMap2);
        Person p = personMap2.get(1);
        System.out.println(p);
}
```
### 24.3 AJAX请求
#### 24.3.1 什么是AJAX请求
```markdown
AJAX 即“Asynchronous Javascript And XML”（异步 JavaScript 和 XML），
是指一种创建交互式网页应用的网页开发技术。
ajax 是一种浏览器通过 js 异步发起请求，局部更新页面的技术。
Ajax 请求的局部更新，浏览器地址栏不会发生变化
局部更新不会舍弃原来页面的内容
```
#### 24.3.2 原生AJAX请求的示例

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>

<head>
    <meta http-equiv="pragma" content="no-cache" />
    <meta http-equiv="cache-control" content="no-cache" />
    <meta http-equiv="Expires" content="0" />
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>Insert title here</title>
    <script type="text/javascript">
        // 在这里使用 javaScript 语言发起 Ajax 请求，访问服务器 AjaxServlet 中 javaScriptAjax
        function ajaxRequest() {
            // 1、我们首先要创建 XMLHttpRequest
            var xmlhttprequest = new XMLHttpRequest();
            // 2、调用 open 方法设置请求参数
            xmlhttprequest.open("GET", "http://localhost:8080/16_json_ajax_i18n/ajaxServlet?action=javaScriptAjax", true)
            // 4、在 send 方法前绑定 onreadystatechange 事件，处理请求完成后的操作。
            xmlhttprequest.onreadystatechange = function () {
                if (xmlhttprequest.readyState == 4 && xmlhttprequest.status == 200) {
                    var jsonObj = JSON.parse(xmlhttprequest.responseText);
                    // 把响应的数据显示在页面上
                    document.getElementById("div01").innerHTML = "编号：" + jsonObj.id + " , 姓名：" +
                        jsonObj.name;
                }
            }
            // 3、调用 send 方法发送请求
            xmlhttprequest.send();
        }
    </script>
</head>

<body>
    <button onclick="ajaxRequest()">ajax request</button>
    <div id="div01">
    </div>
</body>

</html>
```
#### 24.3.3 jQuery中的AJAX请求
```markdown
$.ajax 方法
url 表示请求的地址
type 表示请求的类型 GET 或 POST 请求
data 表示发送给服务器的数据
格式有两种：
一：name=value&name=value
二：{key:value}
success 请求成功，响应的回调函数
dataType 响应的数据类型
常用的数据类型有：
text 表示纯文本
xml 表示 xml 数据
json 表示 json 对象
```

```javascript
$("#ajaxBtn").click(function(){
$.ajax({
url:"http://localhost:8080/16_json_ajax_i18n/ajaxServlet",
// data:"action=jQueryAjax",
data:{action:"jQueryAjax"},
type:"GET",
success:function (data) {
// alert("服务器返回的数据是：" + data);
// var jsonObj = JSON.parse(data);
$("#msg").html("编号：" + data.id + " , 姓名：" + data.name);
},
dataType : "json"
});
});
```
```markdown
$.get 方法和$.post 方法
url 请求的 url 地址
data 发送的数据
callback 成功的回调函数
type 返回的数据类型
```
```javascript
// ajax--get 请求
$("#getBtn").click(function(){
$.get("http://localhost:8080/16_json_ajax_i18n/ajaxServlet","action=jQueryGet",function (data) {
$("#msg").html(" get 编号：" + data.id + " , 姓名：" + data.name);
},"json");
});
// ajax--post 请求
$("#postBtn").click(function(){
$.post("http://localhost:8080/16_json_ajax_i18n/ajaxServlet","action=jQueryPost",function (data)
{
$("#msg").html(" post 编号：" + data.id + " , 姓名：" + data.name);
},"json");
});
```
```markdown
$.getJSON 方法
url 请求的 url 地址
data 发送给服务器的数据
callback 成功的回调函数
```
```javascript
// ajax--getJson 请求
$("#getJSONBtn").click(function(){
$.getJSON("http://localhost:8080/16_json_ajax_i18n/ajaxServlet","action=jQueryGetJSON",function
(data) {
$("#msg").html(" getJSON 编号：" + data.id + " , 姓名：" + data.name);
});
});
```
```markdown
表单序列化 serialize()
serialize()可以把表单中所有表单项的内容都获取到，并以 name=value&name=value 的形式进行拼接。
```

```javascript
// ajax 请求
$("#submit").click(function(){
// 把参数序列化
$.getJSON("http://localhost:8080/16_json_ajax_i18n/ajaxServlet","action=jQuerySerialize&" +
$("#form01").serialize(),function (data) {
$("#msg").html(" Serialize 编号：" + data.id + " , 姓名：" + data.name);
});
});
```
### 24.4 书城项目第十阶段
#### 24.4.1  使用AJAX验证用户名是否可用
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201111090138700.png#pic_center)
```markdown
UserServlet 程序中 ajaxExistsUsername 方法：
```

```java
protected void ajaxExistsUsername(HttpServletRequest req, HttpServletResponse resp) throws
        ServletException, IOException {
// 获取请求的参数 username
        String username = req.getParameter("username");
// 调用 userService.existsUsername();
        boolean existsUsername = userService.existsUsername(username);
// 把返回的结果封装成为 map 对象
        Map<String,Object> resultMap = new HashMap<>();
        resultMap.put("existsUsername",existsUsername);
        Gson gson = new Gson();
        String json = gson.toJson(resultMap);
        resp.getWriter().write(json);
}
```
```markdown
regist.jsp 页面中的代码
```

```javascript
$("#username").blur(function () {
//1 获取用户名
var username = this.value;
$.getJSON("http://localhost:8080/book/userServlet","action=ajaxExistsUsername&username=" +
username,function (data) {
if (data.existsUsername) {
$("span.errorMsg").text("用户名已存在！");
} else {
$("span.errorMsg").text("用户名可用！");
}
});
});
```
#### 24.4.2 使用AJAX修改把商品添加到购物车
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201111090405578.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
CartServlet 程序
```


```java
protected void ajaxAddItem(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
        IOException {
// 获取请求的参数 商品编号
        int id = WebUtils.parseInt(req.getParameter("id"), 0);
// 调用 bookService.queryBookById(id):Book 得到图书的信息
        Book book = bookService.queryBookById(id);
// 把图书信息，转换成为 CartItem 商品项
        CartItem cartItem = new CartItem(book.getId(),book.getName(),1,book.getPrice(),book.getPrice());
// 调用 Cart.addItem(CartItem);添加商品项
        Cart cart = (Cart) req.getSession().getAttribute("cart");
        if (cart == null) {
        cart = new Cart();
        req.getSession().setAttribute("cart",cart);
        }
        cart.addItem(cartItem);
        System.out.println(cart);
// 最后一个添加的商品名称
        req.getSession().setAttribute("lastName", cartItem.getName());
//6、返回购物车总的商品数量和最后一个添加的商品名称
        Map<String,Object> resultMap = new HashMap<String,Object>();
        resultMap.put("totalCount", cart.getTotalCount());
        resultMap.put("lastName",cartItem.getName());
        Gson gson = new Gson();
        String resultMapJsonString = gson.toJson(resultMap);
        resp.getWriter().write(resultMapJsonString);
}
```
```markdown
pages/client/index.jsp 页面
```
```markdown
html 代码
```
```html
<div style="text-align: center">
    <c:if test="${empty sessionScope.cart.items}">
        <%--购物车为空的输出--%>
            <span id="cartTotalCount"> </span>
            <div>
                <span style="color: red" id="cartLastName">当前购物车为空</span>
            </div>
    </c:if>
    <c:if test="${not empty sessionScope.cart.items}">
        <%--购物车非空的输出--%>
            <span id="cartTotalCount">您的购物车中有 ${sessionScope.cart.totalCount} 件商品</span>
            <div>
                您刚刚将<span style="color: red" id="cartLastName">${sessionScope.lastName}</span>加入到了购
                物车中
            </div>
    </c:if>
</div>
```
```markdown
 javaScript 代码
```


```javascript
<Script type="text/javascript">
    $(function () {
        // 给加入购物车按钮绑定单击事件
        $("button.addToCart").click(function () {
            /**
            * 在事件响应的 function 函数 中，有一个 this 对象，这个 this 对象，是当前正在响应事件的 dom 对象
            * @type {jQuery}
            */
            var bookId = $(this).attr("bookId");
            // location.href = "http://localhost:8080/book/cartServlet?action=addItem&id=" + bookId;
            // 发 ajax 请求，添加商品到购物车
            $.getJSON("http://localhost:8080/book/cartServlet", "action=ajaxAddItem&id=" +
                bookId, function (data) {
                    $("#cartTotalCount").text("您的购物车中有 " + data.totalCount + " 件商品");
                    $("#cartLastName").text(data.lastName);
                })
        });
    });
</Script>
```
#### 24.4.3 i18n国际化
##### 24.4.3.1 什么是i18n国际化
```markdown
国际化（Internationalization）指的是同一个网站可以支持多种不同的语言，以方便不同国家，不同语种的用户访问。
关于国际化我们想到的最简单的方案就是为不同的国家创建不同的网站，比如苹果公司，他的英文官网是：
http://www.apple.com 而中国官网是 http://www.apple.com/cn
苹果公司这种方案并不适合全部公司，而我们希望相同的一个网站，而不同人访问的时候可以根据用户所在的区域显示
不同的语言文字，而网站的布局样式等不发生改变。
于是就有了我们说的国际化，国际化总的来说就是同一个网站不同国家的人来访问可以显示出不同的语言。但实际上这
种需求并不强烈，一般真的有国际化需求的公司，主流采用的依然是苹果公司的那种方案，为不同的国家创建不同的页
面。所以国际化的内容我们了解一下即可。
国际化的英文 Internationalization，但是由于拼写过长，老外想了一个简单的写
法叫做 I18N，代表的是 Internationalization
这个单词，以 I 开头，以 N 结尾，而中间是 18 个字母，所以简写为 I18N。以后我们说 I18N 和国际化是一个意思。
```
##### 24.4.3.2 国际化相关要素介绍
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201111091539864.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
##### 24.4.3.3 国际化资源properties测试
```markdown
配置两个语言的配置文件：
i18n_en_US.properties 英文
```
```markdown
username=username
password=password
sex=sex
age=age
regist=regist
boy=boy
email=email
girl=girl
reset=reset
submit=submit
```
```markdown
i18n_zh_CN.properties 中文
```
```markdown
username=用户名
password=密码
sex=性别
age=年龄
regist=注册
boy=男
girl=女
email=邮箱
reset=重置
submit=提交
```
```markdown
国际化测试代码
```


```java
public class I18nTest {
    @Test
    public void testLocale(){
// 获取你系统默认的语言。国家信息
// Locale locale = Locale.getDefault();
// System.out.println(locale);
// for (Locale availableLocale : Locale.getAvailableLocales()) {
// System.out.println(availableLocale);
// }
// 获取中文，中文的常量的 Locale 对象
        System.out.println(Locale.CHINA);
// 获取英文，美国的常量的 Locale 对象
        System.out.println(Locale.US);
    }
    @Test
    public void testI18n(){
// 得到我们需要的 Locale 对象
        Locale locale = Locale.CHINA;
// 通过指定的 basename 和 Locale 对象，读取 相应的配置文件
        ResourceBundle bundle = ResourceBundle.getBundle("i18n", locale);
        System.out.println("username：" + bundle.getString("username"));
        System.out.println("password：" + bundle.getString("password"));
        System.out.println("Sex：" + bundle.getString("sex"));
        System.out.println("age：" + bundle.getString("age"));
    }
}
```
##### 24.4.3.4 通过请求头国际化页面
```html
<%@ page import="java.util.Locale" %>
    <%@ page import="java.util.ResourceBundle" %>
        <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
            <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
            <html>

            <head>
                <meta http-equiv="pragma" content="no-cache" />
                <meta http-equiv="cache-control" content="no-cache" />
                <meta http-equiv="Expires" content="0" />
                <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
                <title>Insert title here</title>
            </head>

            <body>
                <% // 从请求头中获取 Locale 信息（语言） Locale locale=request.getLocale(); System.out.println(locale); // 获取读取包（根据
                    指定的 baseName 和 Locale 读取 语言信息） ResourceBundle i18n=ResourceBundle.getBundle("i18n", locale); %>
                    <a href="">中文</a>|
                    <a href="">english</a>
                    <center>
                        <h1>
                            <%=i18n.getString("regist")%>
                        </h1>
                        <table>
                            <form>
                                <tr>
                                    <td>
                                        <%=i18n.getString("username")%>
                                    </td>
                                    <td><input name="username" type="text" /></td>
                                </tr>
                                <tr>
                                    <td>
                                        <%=i18n.getString("password")%>
                                    </td>
                                    <td><input type="password" /></td>
                                </tr>
                                <tr>
                                    <td>
                                        <%=i18n.getString("sex")%>
                                    </td>
                                    <td>
                                        <input type="radio" />
                                        <%=i18n.getString("boy")%>
                                            <input type="radio" />
                                            <%=i18n.getString("girl")%>
                                    </td>
                                </tr>
                                <tr>
                                    <td>
                                        <%=i18n.getString("email")%>
                                    </td>
                                    <td><input type="text" /></td>
                                </tr>
                                <tr>
                                    <td colspan="2" align="center">
                                        <input type="reset" value="<%=i18n.getString(" reset")%>" />&nbsp;&nbsp;
                                        <input type="submit" value="<%=i18n.getString(" submit")%>" />
                                    </td>
                                </tr>
                            </form>
                        </table>
                        <br /> <br /> <br /> <br />
                    </center>
                    国际化测试：
                    <br /> 1、访问页面，通过浏览器设置，请求头信息确定国际化语言。
                    <br /> 2、通过左上角，手动切换语言
            </body>

            </html>
```
##### 24.4.3.5 通过显示的选择语言类型进行国际化

```html
<%@ page import="java.util.Locale" %>
    <%@ page import="java.util.ResourceBundle" %>
        <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
            <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
            <html>

            <head>
                <meta http-equiv="pragma" content="no-cache" />
                <meta http-equiv="cache-control" content="no-cache" />
                <meta http-equiv="Expires" content="0" />
                <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
                <title>Insert title here</title>
            </head>

            <body>
                <% // 从请求头中获取 Locale 信息（语言） Locale locale=null; String country=request.getParameter("country"); if
                    ("cn".equals(country)) { locale=Locale.CHINA; } else if ("usa".equals(country)) { locale=Locale.US;
                    } else { locale=request.getLocale(); } System.out.println(locale); // 获取读取包（根据 指定的 baseName 和 Locale
                    读取 语言信息） ResourceBundle i18n=ResourceBundle.getBundle("i18n", locale); %>
                    <a href="i18n.jsp?country=cn">中文</a>|
                    <a href="i18n.jsp?country=usa">english</a>
                    <center>
                        <h1>
                            <%=i18n.getString("regist")%>
                        </h1>
                        <table>
                            <form>
                                <tr>
                                    <td>
                                        <%=i18n.getString("username")%>
                                    </td>
                                    <td><input name="username" type="text" /></td>
                                </tr>
                                <tr>
                                    <td>
                                        <%=i18n.getString("password")%>
                                    </td>
                                    <td><input type="password" /></td>
                                </tr>
                                <tr>
                                    <td>
                                        <%=i18n.getString("sex")%>
                                    </td>
                                    <td>
                                        <input type="radio" />
                                        <%=i18n.getString("boy")%>
                                            <input type="radio" />
                                            <%=i18n.getString("girl")%>
                                    </td>
                                </tr>
                                <tr>
                                    <td>
                                        <%=i18n.getString("email")%>
                                    </td>
                                    <td><input type="text" /></td>
                                </tr>
                                <tr>
                                    <td colspan="2" align="center">
                                        <input type="reset" value="<%=i18n.getString(" reset")%>" />&nbsp;&nbsp;
                                        <input type="submit" value="<%=i18n.getString(" submit")%>" />
                                    </td>
                                </tr>
                            </form>
                        </table>
                        <br /> <br /> <br /> <br />
                    </center>
                    国际化测试：
                    <br /> 1、访问页面，通过浏览器设置，请求头信息确定国际化语言。
                    <br /> 2、通过左上角，手动切换语言
            </body>

            </html>
```
##### 24.4.3.6 JSTL标签库实现国际化
```html
<%--1 使用标签设置 Locale 信息--%>
<fmt:setLocale value="" />
<%--2 使用标签设置 baseName--%>
<fmt:setBundle basename=""/>
<%--3 输出指定 key 的国际化信息--%>
<fmt:message key="" />
```
```html
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
    <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
        <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
        <html>

        <head>
            <meta http-equiv="pragma" content="no-cache" />
            <meta http-equiv="cache-control" content="no-cache" />
            <meta http-equiv="Expires" content="0" />
            <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
            <title>Insert title here</title>
        </head>

        <body>
            <%--1 使用标签设置 Locale 信息--%>
                <fmt:setLocale value="${param.locale}" />
                <%--2 使用标签设置 baseName--%>
                    <fmt:setBundle basename="i18n" />
                    <a href="i18n_fmt.jsp?locale=zh_CN">中文</a>|
                    <a href="i18n_fmt.jsp?locale=en_US">english</a>
                    <center>
                        <h1>
                            <fmt:message key="regist" />
                        </h1>
                        <table>
                            <form>
                                <tr>
                                    <td>
                                        <fmt:message key="username" />
                                    </td>
                                    <td><input name="username" type="text" /></td>
                                </tr>
                                <tr>
                                    <td>
                                        <fmt:message key="password" />
                                    </td>
                                    <td><input type="password" /></td>
                                </tr>
                                <tr>
                                    <td>
                                        <fmt:message key="sex" />
                                    </td>
                                    <td>
                                        <input type="radio" />
                                        <fmt:message key="boy" />
                                        <input type="radio" />
                                        <fmt:message key="girl" />
                                    </td>
                                </tr>
                                <tr>
                                    <td>
                                        <fmt:message key="email" />
                                    </td>
                                    <td><input type="text" /></td>
                                </tr>
                                <tr>
                                    <td colspan="2" align="center">
                                        <input type="reset" value="<fmt:message key=" reset" />" />&nbsp;&nbsp;
                                        <input type="submit" value="<fmt:message key=" submit" />" />
                                    </td>
                                </tr>
                            </form>
                        </table>
                        <br /> <br /> <br /> <br />
                    </center>
        </body>

        </html>
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复jw
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
