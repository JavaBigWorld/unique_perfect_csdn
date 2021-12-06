# SpringSecurity和JWT
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210716232937929.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210716234609405.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
# SpringSecurity
## 1 案例介绍
### 1.1 案例效果图
#### 1.1.1 启动项目进入首页
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201120200859267.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
#### 1.1.2 系统管理界面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201120201405264.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
#### 1.1.3 基础数据界面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201120201454792.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
#### 1.1.4 项目最终目录结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201120201533469.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 1.2 建表语句
```sql
/*
SQLyog Ultimate v12.08 (64 bit)
MySQL - 8.0.16 : Database - security_authority
*********************************************************************
*/


/*!40101 SET NAMES utf8 */;

/*!40101 SET SQL_MODE=''*/;

/*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;
/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
/*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;
/*Table structure for table `sys_permission` */

DROP TABLE IF EXISTS `sys_permission`;

CREATE TABLE `sys_permission` (
  `ID` int(11) NOT NULL AUTO_INCREMENT COMMENT '编号',
  `permission_NAME` varchar(30) DEFAULT NULL COMMENT '菜单名称',
  `permission_url` varchar(100) DEFAULT NULL COMMENT '菜单地址',
  `parent_id` int(11) NOT NULL DEFAULT '0' COMMENT '父菜单id',
  PRIMARY KEY (`ID`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

/*Data for the table `sys_permission` */

/*Table structure for table `sys_role` */

DROP TABLE IF EXISTS `sys_role`;

CREATE TABLE `sys_role` (
  `ID` int(11) NOT NULL AUTO_INCREMENT COMMENT '编号',
  `ROLE_NAME` varchar(30) DEFAULT NULL COMMENT '角色名称',
  `ROLE_DESC` varchar(60) DEFAULT NULL COMMENT '角色描述',
  PRIMARY KEY (`ID`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;

/*Data for the table `sys_role` */

/*Table structure for table `sys_role_permission` */

DROP TABLE IF EXISTS `sys_role_permission`;

CREATE TABLE `sys_role_permission` (
  `RID` int(11) NOT NULL COMMENT '角色编号',
  `PID` int(11) NOT NULL COMMENT '权限编号',
  PRIMARY KEY (`RID`,`PID`),
  KEY `FK_Reference_12` (`PID`),
  CONSTRAINT `FK_Reference_11` FOREIGN KEY (`RID`) REFERENCES `sys_role` (`ID`),
  CONSTRAINT `FK_Reference_12` FOREIGN KEY (`PID`) REFERENCES `sys_permission` (`ID`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

/*Data for the table `sys_role_permission` */

/*Table structure for table `sys_user` */

DROP TABLE IF EXISTS `sys_user`;

CREATE TABLE `sys_user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(32) NOT NULL COMMENT '用户名称',
  `password` varchar(120) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '密码',
  `status` int(1) DEFAULT '1' COMMENT '1开启0关闭',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;

/*Data for the table `sys_user` */

/*Table structure for table `sys_user_role` */

DROP TABLE IF EXISTS `sys_user_role`;

CREATE TABLE `sys_user_role` (
  `UID` int(11) NOT NULL COMMENT '用户编号',
  `RID` int(11) NOT NULL COMMENT '角色编号',
  PRIMARY KEY (`UID`,`RID`),
  KEY `FK_Reference_10` (`RID`),
  CONSTRAINT `FK_Reference_10` FOREIGN KEY (`RID`) REFERENCES `sys_role` (`ID`),
  CONSTRAINT `FK_Reference_9` FOREIGN KEY (`UID`) REFERENCES `sys_user` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

/*Data for the table `sys_user_role` */

/*!40101 SET SQL_MODE=@OLD_SQL_MODE */;
/*!40014 SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS */;
/*!40111 SET SQL_NOTES=@OLD_SQL_NOTES */;

```
### 1.3 页面部分所用技术简单说明
#### 1.3.1 adminlTE介绍
```markdown
AdminLTE是一款基于Bootstrap的页面模板，可以快速构建出
一套美观的后台管理页面。
```
### 1.4 后台部分所用技术简单说明
```markdown
后台代码采用springmvc实现web层，spring控制业务层事务，
mybatis操作数据库
```
## 2 初识权限管理
### 2.1 权限管理概念
```markdown
权限管理，一般指根据系统设置的安全规则或者安全策略，用户
可以访问而且只能访问自己被授权的资源。权限管理几乎出
现在任何系统里面，前提是需要有用户和密码认证的系统。
在权限管理的概念中，有两个非常重要的名词：
认证：通过用户名和密码成功登陆系统后，让系统得到当前用
户的角色身份。
授权：系统根据当前用户的角色，给其授予对应可以操作的权限资源。
```
### 2.2 完成权限管理需要三个对象
```markdown
用户：主要包含用户名，密码和当前用户的角色信息，可实现认证操作。
角色：主要包含角色名称，角色描述和当前角色拥有的权限信息，
可实现授权操作。
权限：权限也可以称为菜单，主要包含当前权限名称，url地址等
信息，可实现动态展示菜单。
注：这三个对象中，用户与角色是多对多的关系，角色与权限是
多对多的关系，用户与权限没有直接关系，
二者是通过角色来建立关联关系的。
```
## 3 初识Spring Security
### 3.1 Spring Security概念
```markdown
Spring Security是spring采用AOP思想，基于servlet过滤器实
现的安全框架。它提供了完善的认证机制和方法级的
授权功能。是一款非常优秀的权限管理框架。
```
### 3.2 Spring Security简单入门
```markdown
Spring Security博大精深，设计巧妙，功能繁杂，一言难尽，
咱们还是直接上代码吧！
```
#### 3.2.1 创建web工程并导入jar包
```markdown
Spring Security主要jar包功能介绍
spring-security-core.jar
核心包，任何Spring Security功能都需要此包。
spring-security-web.jar
web工程必备，包含过滤器和相关的Web安全基础结构代码。
spring-security-config.jar
用于解析xml配置文件，用到Spring Security的xml配置文件的就
要用到此包。
spring-security-taglibs.jar
Spring Security提供的动态标签库，jsp页面可以用
```
```xml
 <dependency>
      <groupId>org.springframework.security</groupId>
      <artifactId>spring-security-taglibs</artifactId>
      <version>5.1.5.RELEASE</version>
</dependency>
<dependency>
      <groupId>org.springframework.security</groupId>
      <artifactId>spring-security-config</artifactId>
      <version>5.1.5.RELEASE</version>
</dependency>

```
```markdown
最终依赖树效果
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112023471778.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
#### 3.2.2 配置web.xml
```xml
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
version="3.0">
<display-name>Archetype Created Web Application</display-name>
<!--Spring Security过滤器链，注意过滤器名称必须叫springSecurityFilterChain-->
<filter>
<filter-name>springSecurityFilterChain</filter-name>
<filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
</filter>
<filter-mapping>
<filter-name>springSecurityFilterChain</filter-name>
<url-pattern>/*</url-pattern>
</filter-mapping>
</web-app>
```
#### 3.2.3 配置spring-security.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:context="http://www.springframework.org/schema/context"
xmlns:aop="http://www.springframework.org/schema/aop"
xmlns:tx="http://www.springframework.org/schema/tx"
xmlns:security="http://www.springframework.org/schema/security"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd
http://www.springframework.org/schema/aop
http://www.springframework.org/schema/aop/spring-aop.xsd
http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring-tx.xsd
http://www.springframework.org/schema/security
http://www.springframework.org/schema/security/spring-security.xsd">

<!--配置springSecurity-->
    <!--
    auto-config="true"  表示自动加载springsecurity的配置文件
    use-expressions="true" 表示使用spring的el表达式来配置springsecurity
    -->
<security:http auto-config="true" use-expressions="true">

	<security:intercept-url pattern="/**" access="hasAnyRole('ROLE_USER','ROLE_ADMIN')"/>
</security:http>
<!--设置Spring Security认证用户信息的来源-->
<security:authentication-manager>
	<security:authentication-provider>
		<security:user-service>
		<!--设置Spring Security认证用户信息的来源-->
    <!--
    springsecurity默认的认证必须是加密的，加上{noop}表示不加密认证。
    -->
			<security:user name="user" password="{noop}user"
authorities="ROLE_USER" />
			<security:user name="admin" password="{noop}admin"
authorities="ROLE_ADMIN" />
</security:user-service>
	</security:authentication-provider>
</security:authentication-manager>
</beans>
```
#### 3.2.4 将spring-security.xml配置文件引入到applicationContext.xml中
```xml
<!--引入SpringSecurity主配置文件-->
<import resource="classpath:spring-security.xml"/>
```
#### 3.2.5 运行结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201121072930700.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
页面源代码
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201121073430316.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201121073451188.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
最后，我们在这个登录页面上输入用户名user，密码user，点
击Sign in，好了，总算再次看到首页了！
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201121073805230.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
## 4 Spring Security过滤器链
### 4.1 Spring Security常用过滤器介绍
```markdown
1. org.springframework.security.web.context.SecurityContextPersistenceFilter
首当其冲的一个过滤器，作用之重要，自不必多言。
SecurityContextPersistenceFilter主要是使
用SecurityContextRepository在session中保存或更新一个
SecurityContext，并将SecurityContext给以后的过滤器使用，
来为后续filter建立所需的上下文。
SecurityContext中存储了当前用户的认证以及权限信息。

2.org.springframework.security.web.context.request.async.WebAsyncManagerIntegrationFilter
此过滤器用于集成SecurityContext到Spring异步执行机制中的WebAsyncManager


3. org.springframework.security.web.header.HeaderWriterFilter
向请求的Header中添加相应的信息,可在http标签内部使用
security:headers来控制


4. org.springframework.security.web.csrf.CsrfFilter
csrf又称跨域请求伪造，SpringSecurity会对所有post请
求验证是否包含系统生成的csrf的token信息，
如果不包含，则报错。起到防止csrf攻击的效果。

5. org.springframework.security.web.authentication.logout.LogoutFilter
匹配URL为/logout的请求，实现用户退出,清除认证信息。

6. org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter
认证操作全靠这个过滤器，默认匹配URL为/login且必须为POST请求。
7. org.springframework.security.web.authentication.ui.DefaultLoginPageGeneratingFilter
如果没有在配置文件中指定认证页面，则由该过滤器生成一个默认认证页面。
8. org.springframework.security.web.authentication.ui.DefaultLogoutPageGeneratingFilter
由此过滤器可以生产一个默认的退出登录页面
9. org.springframework.security.web.authentication.www.BasicAuthenticationFilter
此过滤器会自动解析HTTP请求中头部名字为Authentication，且以Basic开头的头信息。
10. org.springframework.security.web.savedrequest.RequestCacheAwareFilter
通过HttpSessionRequestCache内部维护了一个RequestCache，用于缓存HttpServletRequest
11. org.springframework.security.web.servletapi.SecurityContextHolderAwareRequestFilter
针对ServletRequest进行了一次包装，使得request具有更加丰富的API
12. org.springframework.security.web.authentication.AnonymousAuthenticationFilter
当SecurityContextHolder中认证信息为空,则会创建一个匿名用户存入到SecurityContextHolder中。
spring security为了兼容未登录的访问，也走了一套认证流程，只不过是一个匿名的身份。
13. org.springframework.security.web.session.SessionManagementFilter
SecurityContextRepository限制同一用户开启多个会话的数量
14. org.springframework.security.web.access.ExceptionTranslationFilter
异常转换过滤器位于整个springSecurityFilterChain的后方，用来转换整个链路中出现的异常
15. org.springframework.security.web.access.intercept.FilterSecurityInterceptor
获取所配置资源访问的授权信息，根据SecurityContextHolder中存储的用户信息来决定其是否有权
限。
```
```markdown
好了！这一堆排山倒海的过滤器介绍完了。
那么，是不是spring security一共就这么多过滤器呢？
答案是否定的！随着spring-security.xml配置的添加，还
会出现新的过滤器。
那么，是不是spring security每次都会加载这些过滤器呢？
答案也是否定的！随着spring-security.xml配置的修
改，有些过滤器可能会被去掉。
```

### 4.2 spring security过滤器链加载原理
#### 4.2.1 DelegatingFilterProxy

```java
public class DelegatingFilterProxy extends GenericFilterBean {
@Nullable
private String contextAttribute;
@Nullable
private WebApplicationContext webApplicationContext;
@Nullable
private String targetBeanName;
private boolean targetFilterLifecycle;
@Nullable
private volatile Filter delegate;//注：这个过滤器才是真正加载的过滤器
private final Object delegateMonitor;
//注：doFilter才是过滤器的入口，直接从这看！
public void doFilter(ServletRequest request, ServletResponse response, FilterChain
filterChain) throws ServletException, IOException {
Filter delegateToUse = this.delegate;
if (delegateToUse == null) {
synchronized(this.delegateMonitor) {
delegateToUse = this.delegate;
if (delegateToUse == null) {
WebApplicationContext wac = this.findWebApplicationContext();
if (wac == null) {
throw new IllegalStateException("No WebApplicationContext found: no
ContextLoaderListener or DispatcherServlet registered?");
}
//第一步：doFilter中最重要的一步，初始化上面私有过滤器属性delegate
delegateToUse = this.initDelegate(wac);
}
this.delegate = delegateToUse;
}
}
//第三步：执行FilterChainProxy过滤器
this.invokeDelegate(delegateToUse, request, response, filterChain);
}
//第二步：直接看最终加载的过滤器到底是谁
protected Filter initDelegate(WebApplicationContext wac) throws ServletException {
//debug得知targetBeanName为：springSecurityFilterChain
String targetBeanName = this.getTargetBeanName();
Assert.state(targetBeanName != null, "No target bean name set");
//debug得知delegate对象为：FilterChainProxy
Filter delegate = (Filter)wac.getBean(targetBeanName, Filter.class);
if (this.isTargetFilterLifecycle()) {
delegate.init(this.getFilterConfig());
}
return delegate;
}
protected void invokeDelegate(Filter delegate, ServletRequest request, ServletResponse
response, FilterChain filterChain) throws ServletException, IOException {
delegate.doFilter(request, response, filterChain);
}
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112108205167.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
由此可知，DelegatingFilterProxy通过springSecurityFilterChain这个
名称，得到了一个FilterChainProxy过滤器，
最终执行了这个过滤器。
```
#### 4.2.2 FilterChainProxy

```java
public class FilterChainProxy extends GenericFilterBean {
private static final Log logger = LogFactory.getLog(FilterChainProxy.class);
private static final String FILTER_APPLIED =
FilterChainProxy.class.getName().concat(".APPLIED");
private List<SecurityFilterChain> filterChains;
private FilterChainProxy.FilterChainValidator filterChainValidator;
private HttpFirewall firewall;
//咿！？可以通过一个叫SecurityFilterChain的对象实例化出一个FilterChainProxy对象
//这FilterChainProxy又是何方神圣？会不会是真正的过滤器链对象呢？先留着这个疑问！
public FilterChainProxy(SecurityFilterChain chain) {
this(Arrays.asList(chain));
}
//又是SecurityFilterChain这家伙！嫌疑更大了！
public FilterChainProxy(List<SecurityFilterChain> filterChains) {
this.filterChainValidator = new FilterChainProxy.NullFilterChainValidator();
this.firewall = new StrictHttpFirewall();
this.filterChains = filterChains;
}
//注：直接从doFilter看
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
throws IOException, ServletException {
boolean clearContext = request.getAttribute(FILTER_APPLIED) == null;
if (clearContext) {
try {
request.setAttribute(FILTER_APPLIED, Boolean.TRUE);
this.doFilterInternal(request, response, chain);
} finally {
SecurityContextHolder.clearContext();
request.removeAttribute(FILTER_APPLIED);
}
} else {
//第一步：具体操作调用下面的doFilterInternal方法了
this.doFilterInternal(request, response, chain);
}
}
private void doFilterInternal(ServletRequest request, ServletResponse response, FilterChain
chain) throws IOException, ServletException {
FirewalledRequest fwRequest =
this.firewall.getFirewalledRequest((HttpServletRequest)request);
HttpServletResponse fwResponse =
this.firewall.getFirewalledResponse((HttpServletResponse)response);
//第二步：封装要执行的过滤器链，那么多过滤器就在这里被封装进去了！
List<Filter> filters = this.getFilters((HttpServletRequest)fwRequest);
if (filters != null && filters.size() != 0) {
FilterChainProxy.VirtualFilterChain vfc = new
FilterChainProxy.VirtualFilterChain(fwRequest, chain, filters);
//第四步：加载过滤器链
vfc.doFilter(fwRequest, fwResponse);
} else {
if (logger.isDebugEnabled()) {
logger.debug(UrlUtils.buildRequestUrl(fwRequest) + (filters == null ? " has no
matching filters" : " has an empty filter list"));
}
fwRequest.reset();
chain.doFilter(fwRequest, fwResponse);
}
}
private List<Filter> getFilters(HttpServletRequest request) {
Iterator var2 = this.filterChains.iterator();
//第三步：封装过滤器链到SecurityFilterChain中！
SecurityFilterChain chain;
do {
if (!var2.hasNext()) {
return null;
}
chain = (SecurityFilterChain)var2.next();
} while(!chain.matches(request));
return chain.getFilters();
}
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201121082722473.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
原来这些过滤器还真是都被封装进SecurityFilterChain中了
```
#### 4.2.3 SecurityFilterChain

```java
//接口
public interface SecurityFilterChain {
boolean matches(HttpServletRequest var1);
List<Filter> getFilters();
}
//实现类
public final class DefaultSecurityFilterChain implements SecurityFilterChain {
private static final Log logger = LogFactory.getLog(DefaultSecurityFilterChain.class);
private final RequestMatcher requestMatcher;
private final List<Filter> filters;
public DefaultSecurityFilterChain(RequestMatcher requestMatcher, Filter... filters) {
this(requestMatcher, Arrays.asList(filters));
}
public DefaultSecurityFilterChain(RequestMatcher requestMatcher, List<Filter> filters) {
logger.info("Creating filter chain: " + requestMatcher + ", " + filters);
this.requestMatcher = requestMatcher;
this.filters = new ArrayList(filters);
}
public RequestMatcher getRequestMatcher() {
return this.requestMatcher;
}
public List<Filter> getFilters() {
return this.filters;
}
public boolean matches(HttpServletRequest request) {
return this.requestMatcher.matches(request);
}
public String toString() {
return "[ " + this.requestMatcher + ", " + this.filters + "]";
}
}
```
## 5 SpringSecurity使用自定义认证页面
### 5.1 在SpringSecurity主配置文件中指定认证页面配置信息

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:context="http://www.springframework.org/schema/context"
xmlns:aop="http://www.springframework.org/schema/aop"
xmlns:tx="http://www.springframework.org/schema/tx"
xmlns:security="http://www.springframework.org/schema/security"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd
http://www.springframework.org/schema/aop
http://www.springframework.org/schema/aop/spring-aop.xsd
http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring-tx.xsd
http://www.springframework.org/schema/security
http://www.springframework.org/schema/security/spring-security.xsd">
<!--直接释放无需经过SpringSecurity过滤器的静态资源-->
<security:http pattern="/css/**" security="none"/>
<security:http pattern="/img/**" security="none"/>
<security:http pattern="/plugins/**" security="none"/>
<security:http pattern="/failer.jsp" security="none"/>
<security:http pattern="/favicon.ico" security="none"/>
<!--设置可以用spring的el表达式配置Spring Security并自动生成对应配置组件（过滤器）-->
<security:http auto-config="true" use-expressions="true">
<!--指定login.jsp页面可以被匿名访问-->
<security:intercept-url pattern="/login.jsp" access="permitAll()"/>
<!--使用spring的el表达式来指定项目所有资源访问都必须有ROLE_USER或ROLE_ADMIN角色-->
<security:intercept-url pattern="/**" access="hasAnyRole('ROLE_USER','ROLE_ADMIN')"/>
<!--指定自定义的认证页面-->
<security:form-login login-page="/login.jsp"
login-processing-url="/login"
default-target-url="/index.jsp"
authentication-failure-url="/failer.jsp"/>
<!--指定退出登录后跳转的页面-->
<security:logout logout-url="/logout"
logout-success-url="/login.jsp"/>
</security:http>
<!--设置Spring Security认证用户信息的来源-->
<security:authentication-manager>
<security:authentication-provider>
<security:user-service>
<security:user name="user" password="{noop}user"
authorities="ROLE_USER" />
<security:user name="admin" password="{noop}admin"
authorities="ROLE_ADMIN" />
</security:user-service>
</security:authentication-provider>
</security:authentication-manager>
</beans>
```
```markdown
修改认证页面的请求地址
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201121090352228.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201121090535303.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
然后你开开心心的输入了用户名user，密码user，就出现了
如下的界面：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201121090557907.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
403什么异常？这是SpringSecurity中的权限不足！这个异常怎么
来的？还记得上面SpringSecurity内置认证页面源
码中的那个_csrf隐藏input吗？问题就在这了！
```
### 5.2 SpringSecurity的csrf防护机制
```markdown
CSRF（Cross-site request forgery）跨站请求伪造，是一种
难以防范的网络攻击方式。
```
#### 5.2.1 SpringSecurity中CsrfFilter过滤器说明
```java
public final class CsrfFilter extends OncePerRequestFilter {
public static final RequestMatcher DEFAULT_CSRF_MATCHER = new
CsrfFilter.DefaultRequiresCsrfMatcher();
private final Log logger = LogFactory.getLog(this.getClass());
private final CsrfTokenRepository tokenRepository;
private RequestMatcher requireCsrfProtectionMatcher;
private AccessDeniedHandler accessDeniedHandler;
public CsrfFilter(CsrfTokenRepository csrfTokenRepository) {
this.requireCsrfProtectionMatcher = DEFAULT_CSRF_MATCHER;
this.accessDeniedHandler = new AccessDeniedHandlerImpl();
Assert.notNull(csrfTokenRepository, "csrfTokenRepository cannot be null");
this.tokenRepository = csrfTokenRepository;
}
//通过这里可以看出SpringSecurity的csrf机制把请求方式分成两类来处理
protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response,
FilterChain filterChain) throws ServletException, IOException {
request.setAttribute(HttpServletResponse.class.getName(), response);
CsrfToken csrfToken = this.tokenRepository.loadToken(request);
boolean missingToken = csrfToken == null;
if (missingToken) {
csrfToken = this.tokenRepository.generateToken(request);
this.tokenRepository.saveToken(csrfToken, request, response);
}
request.setAttribute(CsrfToken.class.getName(), csrfToken);
request.setAttribute(csrfToken.getParameterName(), csrfToken);
//第一类："GET", "HEAD", "TRACE", "OPTIONS"四类请求可以直接通过
if (!this.requireCsrfProtectionMatcher.matches(request)) {
filterChain.doFilter(request, response);
} else {
//第二类：除去上面四类，包括POST都要被验证携带token才能通过
String actualToken = request.getHeader(csrfToken.getHeaderName());
if (actualToken == null) {
actualToken = request.getParameter(csrfToken.getParameterName());
}
if (!csrfToken.getToken().equals(actualToken)) {
if (this.logger.isDebugEnabled()) {
this.logger.debug("Invalid CSRF token found for " +
UrlUtils.buildFullRequestUrl(request));
}
if (missingToken) {
this.accessDeniedHandler.handle(request, response, new
MissingCsrfTokenException(actualToken));
} else {
this.accessDeniedHandler.handle(request, response, new
InvalidCsrfTokenException(csrfToken, actualToken));
}
} else {
filterChain.doFilter(request, response);
}
}
}
public void setRequireCsrfProtectionMatcher(RequestMatcher requireCsrfProtectionMatcher) {
Assert.notNull(requireCsrfProtectionMatcher, "requireCsrfProtectionMatcher cannot be
null");
this.requireCsrfProtectionMatcher = requireCsrfProtectionMatcher;
}
public void setAccessDeniedHandler(AccessDeniedHandler accessDeniedHandler) {
Assert.notNull(accessDeniedHandler, "accessDeniedHandler cannot be null");
this.accessDeniedHandler = accessDeniedHandler;
}
private static final class DefaultRequiresCsrfMatcher implements RequestMatcher {
private final HashSet<String> allowedMethods;
private DefaultRequiresCsrfMatcher() {
this.allowedMethods = new HashSet(Arrays.asList("GET", "HEAD", "TRACE", "OPTIONS"));
}
public boolean matches(HttpServletRequest request) {
return !this.allowedMethods.contains(request.getMethod());
}
}
}
```
```markdown
通过源码分析，我们明白了，自己的认证页面，请求方式为POST，
但却没有携带token，所以才出现了403权限不
足的异常。那么如何处理这个问题呢？
禁用csrf防护机制或者在认证页面携带token请求
```
#### 5.2.2 禁用csrf防护机制

```xml
<!--设置可以用spring的el表达式配置Spring Security并自动生成对应配置组件（过滤器）-->
<security:http auto-config="true" use-expressions="true">
	<!--指定login.jsp页面可以被匿名访问-->
	<security:intercept-url pattern="/login.jsp" access="permitAll()"/>
	<!--使用spring的el表达式来指定项目所有资源访问都必须有ROLE_USER或ROLE_ADMIN角色-->
	<security:intercept-url pattern="/**" access="hasAnyRole('ROLE_USER','ROLE_ADMIN')"/>
	<!--指定自定义的认证页面-->
	<security:form-login login-page="/login.jsp"
	login-processing-url="/login"
	default-target-url="/index.jsp"
	authentication-failure-url="/failer.jsp"/>
	<!--指定退出登录后跳转的页面-->
	<security:logout logout-url="/logout"
	logout-success-url="/login.jsp"/>
	<!--禁用csrf防护机制-->
	<security:csrf disabled="true"/>
</security:http>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201224102127718.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

#### 5.2.3 在认证页面携带token请求
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201224102941295.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201121092123506.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
注：HttpSessionCsrfTokenRepository对象负责生成token并
放入session域中。
```
```markdown
为了确保能crsf攻击保护，修改数据用put方式，添加用post
```
#### 5.2.4 实现注销功能
```markdown
注销方式为${pageContext.request.contextPath}/logout，
这个logout(SpringSecurity 已经帮我们写好了，但方式必须
为post且通过csrf)
```
```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@taglib uri="http://www.springframework.org/security/tags" prefix="security"%>
<!-- 页面头部 -->
<header class="main-header">
	<!-- Logo -->
	<a href="${pageContext.request.contextPath}/pages/main.jsp" class="logo"> <!-- mini logo for sidebar mini 50x50 pixels -->
		<span class="logo-mini"><b>数据</b></span> <!-- logo for regular state and mobile devices -->
		<span class="logo-lg"><b>数据</b>后台管理</span>
	</a>
	<!-- Header Navbar: style can be found in header.less -->
	<nav class="navbar navbar-static-top">
		<!-- Sidebar toggle button-->
		<a href="#" class="sidebar-toggle" data-toggle="offcanvas"
			role="button"> <span class="sr-only">Toggle navigation</span>
		</a>

		<div class="navbar-custom-menu">
			<ul class="nav navbar-nav">

				<li class="dropdown user user-menu"><a href="#"
					class="dropdown-toggle" data-toggle="dropdown"> <img
						src="${pageContext.request.contextPath}/img/user2-160x160.jpg"
						class="user-image" alt="User Image">
					<span class="hidden-xs">
							<%--<security:authentication property="principal.username" />--%>
							<%--<security:authentication property="name" />--%>
					</span>
				</a>
					<ul class="dropdown-menu">
						<!-- User image -->
						<li class="user-header"><img
							src="${pageContext.request.contextPath}/img/user2-160x160.jpg"
							class="img-circle" alt="User Image"></li>

						<!-- Menu Footer-->
						<li class="user-footer">
							<div class="pull-left">
								<a href="#" class="btn btn-default btn-flat">修改密码</a>
							</div>
							<div class="pull-right">
								<%--<a href="${pageContext.request.contextPath}/logout"
									class="btn btn-default btn-flat">注销</a>--%>
								<form action="${pageContext.request.contextPath}/logout" method="post">
									<security:csrfInput/>
									<input type="submit" value="注销">
								</form>
							</div>
						</li>
					</ul></li>
			</ul>
		</div>
	</nav>
</header>
<!-- 页面头部 /-->
```
## 6 SpringSecurity使用数据库数据完成认证
### 6.1 认证流程分析
```markdown
UsernamePasswordAuthenticationFilter
```
```java
public class UsernamePasswordAuthenticationFilter extends AbstractAuthenticationProcessingFilter{
	public static final String SPRING_SECURITY_FORM_USERNAME_KEY = "username";
	public static final String SPRING_SECURITY_FORM_PASSWORD_KEY = "password";
	private String usernameParameter = "username";
	private String passwordParameter = "password";
	private boolean postOnly = true;
	public UsernamePasswordAuthenticationFilter() {
	super(new AntPathRequestMatcher("/login", "POST"));
}
	public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse
	response) throws AuthenticationException {
	//必须为POST请求
	if (this.postOnly && !request.getMethod().equals("POST")) {
	throw new AuthenticationServiceException("Authentication method not supported: " +
	request.getMethod());
	} else {
	String username = this.obtainUsername(request);
	String password = this.obtainPassword(request);
	if (username == null) {
	username = "";
	}
	if (password == null) {
	password = "";
	}
	username = username.trim();
	//将填写的用户名和密码封装到了UsernamePasswordAuthenticationToken中
	UsernamePasswordAuthenticationToken authRequest = new
	UsernamePasswordAuthenticationToken(username, password);
	this.setDetails(request, authRequest);
	//调用AuthenticationManager对象实现认证
	return this.getAuthenticationManager().authenticate(authRequest);
	}
}
}
```
```markdown
AuthenticationManager
```
```markdown
由上面源码得知，真正认证操作在AuthenticationManager里面！
然后看AuthenticationManager的实现类ProviderManager：
```
```java
public class ProviderManager implements AuthenticationManager, MessageSourceAware,
InitializingBean {
private static final Log logger = LogFactory.getLog(ProviderManager.class);
private AuthenticationEventPublisher eventPublisher;
private List<AuthenticationProvider> providers;
protected MessageSourceAccessor messages;
private AuthenticationManager parent;
private boolean eraseCredentialsAfterAuthentication;
//注意AuthenticationProvider这个对象，SpringSecurity针对每一种认证，什么qq登录啊，
//用户名密码登陆啊，微信登录啊都封装了一个AuthenticationProvider对象。
public ProviderManager(List<AuthenticationProvider> providers) {
this(providers, (AuthenticationManager)null);
}
public Authentication authenticate(Authentication authentication) throws
AuthenticationException {
Class<? extends Authentication> toTest = authentication.getClass();
AuthenticationException lastException = null;
AuthenticationException parentException = null;
Authentication result = null;
Authentication parentResult = null;
boolean debug = logger.isDebugEnabled();
Iterator var8 = this.getProviders().iterator();
//循环所有AuthenticationProvider，匹配当前认证类型。
while(var8.hasNext()) {
AuthenticationProvider provider = (AuthenticationProvider)var8.next();
if (provider.supports(toTest)) {
if (debug) {
logger.debug("Authentication attempt using " +
provider.getClass().getName());
}
try {
//找到了对应认证类型就继续调用AuthenticationProvider对象完成认证业务。
result = provider.authenticate(authentication);
if (result != null) {
this.copyDetails(authentication, result);
break;
}
} catch (AccountStatusException var13) {
this.prepareException(var13, authentication);
throw var13;
} catch (InternalAuthenticationServiceException var14) {
this.prepareException(var14, authentication);
throw var14;
} catch (AuthenticationException var15) {
lastException = var15;
}
}
}
if (result == null && this.parent != null) {
try {
result = parentResult = this.parent.authenticate(authentication);
} catch (ProviderNotFoundException var11) {
} catch (AuthenticationException var12) {
parentException = var12;
lastException = var12;
}
}
if (result != null) {
if (this.eraseCredentialsAfterAuthentication && result instanceof
CredentialsContainer) {
((CredentialsContainer)result).eraseCredentials();
}
if (parentResult == null) {
this.eventPublisher.publishAuthenticationSuccess(result);
}
return result;
} else {
if (lastException == null) {
lastException = new
ProviderNotFoundException(this.messages.getMessage("ProviderManager.providerNotFound", new
Object[]{toTest.getName()}, "No AuthenticationProvider found for {0}"));
}
if (parentException == null) {
this.prepareException((AuthenticationException)lastException, authentication);
}
throw lastException;
}
}
}
```
```markdown
AbstractUserDetailsAuthenticationProvider

```
```markdown
咱们继续再找到AuthenticationProvider的实现类AbstractUserDetailsAuthenticationProvider：
```
```java
public class DaoAuthenticationProvider extends AbstractUserDetailsAuthenticationProvider {
private static final String USER_NOT_FOUND_PASSWORD = "userNotFoundPassword";
private PasswordEncoder passwordEncoder;
private volatile String userNotFoundEncodedPassword;
private UserDetailsService userDetailsService;
private UserDetailsPasswordService userDetailsPasswordService;
protected final UserDetails retrieveUser(String username,
UsernamePasswordAuthenticationToken authentication) throws AuthenticationException {
this.prepareTimingAttackProtection();
try {
//重点来了！主要就在这里了！
//可别忘了，咱们为什么要翻源码，是想用自己数据库中的数据实现认证操作啊！
//UserDetails就是SpringSecurity自己的用户对象。
//this.getUserDetailsService()其实就是得到UserDetailsService的一个实现类
//loadUserByUsername里面就是真正的认证逻辑
//也就是说我们可以直接编写一个UserDetailsService的实现类，告诉SpringSecurity就可以了！
//loadUserByUsername方法中只需要返回一个UserDetails对象即可
UserDetails loadedUser = this.getUserDetailsService().loadUserByUsername(username);
//若返回null，就抛出异常，认证失败。
if (loadedUser == null) {
throw new InternalAuthenticationServiceException("UserDetailsService returned
null, which is an interface contract violation");
} else {
//若有得到了UserDetails对象，返回即可。
return loadedUser;
}
} catch (UsernameNotFoundException var4) {
this.mitigateAgainstTimingAttack(authentication);
throw var4;
} catch (InternalAuthenticationServiceException var5) {
throw var5;
} catch (Exception var6) {
throw new InternalAuthenticationServiceException(var6.getMessage(), var6);
}
}
}
```
```markdown
AbstractUserDetailsAuthenticationProvider中authenticate返回值
```

```markdown
按理说到此已经知道自定义认证方法的怎么写了，但咱们把返
回的流程也大概走一遍，上面不是说到返回了一个
UserDetails对象对象吗？跟着它，
就又回到了AbstractUserDetailsAuthenticationProvider
对象中authenticate方法的最后一行了。
```
```java
public abstract class AbstractUserDetailsAuthenticationProvider implements
AuthenticationProvider, InitializingBean, MessageSourceAware {
public Authentication authenticate(Authentication authentication) throws
AuthenticationException {
//最后一行返回值，调用了createSuccessAuthentication方法，此方法就在下面！
return this.createSuccessAuthentication(principalToReturn, authentication, user);
}
//咿！？怎么又封装了一次UsernamePasswordAuthenticationToken，开局不是已经封装过了吗？
protected Authentication createSuccessAuthentication(Object principal, Authentication
authentication, UserDetails user) {
//那就从构造方法点进去看看，这才干啥了。
UsernamePasswordAuthenticationToken result = new
UsernamePasswordAuthenticationToken(principal, authentication.getCredentials(),
this.authoritiesMapper.mapAuthorities(user.getAuthorities()));
result.setDetails(authentication.getDetails());
return result;
}
}
```
```markdown
UsernamePasswordAuthenticationToken
```
```java
public class UsernamePasswordAuthenticationToken extends AbstractAuthenticationToken {
private static final long serialVersionUID = 510L;
private final Object principal;
private Object credentials;
//认证成功前，调用的是这个带有两个参数的。
public UsernamePasswordAuthenticationToken(Object principal, Object credentials) {
super((Collection)null);
this.principal = principal;
this.credentials = credentials;
this.setAuthenticated(false);
}
//认证成功后，调用的是这个带有三个参数的。
public UsernamePasswordAuthenticationToken(Object principal, Object credentials,Collection<? extends GrantedAuthority> authorities) {
//看看父类干了什么！
super(authorities);
this.principal = principal;
this.credentials = credentials;
super.setAuthenticated(true);
}
}
```
```markdown
AbstractAuthenticationToken
```
```markdown
再点进去super(authorities)看看：
```
```java
public abstract class AbstractAuthenticationToken implements Authentication,
CredentialsContainer {
private final Collection<GrantedAuthority> authorities;
private Object details;
private boolean authenticated = false;
public AbstractAuthenticationToken(Collection<? extends GrantedAuthority> authorities) {
//这时两个参数那个分支！
if (authorities == null) {
this.authorities = AuthorityUtils.NO_AUTHORITIES;
} else {
//三个参数的，看这里！
Iterator var2 = authorities.iterator();
//原来是多个了添加权限信息的步骤
GrantedAuthority a;
do {
if (!var2.hasNext()) {
ArrayList<GrantedAuthority> temp = new ArrayList(authorities.size());
temp.addAll(authorities);
this.authorities = Collections.unmodifiableList(temp);
return;
}
a = (GrantedAuthority)var2.next();
} while(a != null);
//若没有权限信息，是会抛出异常的！
throw new IllegalArgumentException("Authorities collection cannot contain any null
elements");
}
}
}
```
```markdown
由此，咱们需要牢记自定义认证业务逻辑返回的UserDetails
对象中一定要放置权限信息啊！
现在可以结束源码分析了吧？先不要着急！
咱们回到最初的地方UsernamePasswordAuthenticationFilter，
你看好看了，这可是个过滤器，咱们分析这么
久，都没提到doFilter方法，你不觉得心里不踏实？
可是这里面也没有doFilter呀？那就从父类找！
```
```markdown
AbstractAuthenticationProcessingFilter
```
```java
public abstract class AbstractAuthenticationProcessingFilter extends GenericFilterBean
implements ApplicationEventPublisherAware, MessageSourceAware {
//doFilter再次！
public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain) throws
IOException, ServletException {
HttpServletRequest request = (HttpServletRequest)req;
HttpServletResponse response = (HttpServletResponse)res;
if (!this.requiresAuthentication(request, response)) {
chain.doFilter(request, response);
} else {
if (this.logger.isDebugEnabled()) {
this.logger.debug("Request is to process authentication");
}
Authentication authResult;
try {
authResult = this.attemptAuthentication(request, response);
if (authResult == null) {
return;
}
this.sessionStrategy.onAuthentication(authResult, request, response);
} catch (InternalAuthenticationServiceException var8) {
this.logger.error("An internal error occurred while trying to authenticate the
user.", var8);
this.unsuccessfulAuthentication(request, response, var8);
return;
} catch (AuthenticationException var9) {
this.unsuccessfulAuthentication(request, response, var9);
return;
}
if (this.continueChainBeforeSuccessfulAuthentication) {
chain.doFilter(request, response);
}
this.successfulAuthentication(request, response, chain, authResult);
}
}
protected boolean requiresAuthentication(HttpServletRequest request, HttpServletResponse
response) {
return this.requiresAuthenticationRequestMatcher.matches(request);
}
//成功走successfulAuthentication
protected void successfulAuthentication(HttpServletRequest request, HttpServletResponse response, FilterChain chain, Authentication authResult) throws IOException, ServletException {
if (this.logger.isDebugEnabled()) {
this.logger.debug("Authentication success. Updating SecurityContextHolder to
contain: " + authResult);
}
//认证成功，将认证信息存储到SecurityContext中！
SecurityContextHolder.getContext().setAuthentication(authResult);
//登录成功调用rememberMeServices
this.rememberMeServices.loginSuccess(request, response, authResult);
if (this.eventPublisher != null) {
this.eventPublisher.publishEvent(new
InteractiveAuthenticationSuccessEvent(authResult, this.getClass()));
}
this.successHandler.onAuthenticationSuccess(request, response, authResult);
}
//失败走unsuccessfulAuthentication
protected void unsuccessfulAuthentication(HttpServletRequest request, HttpServletResponse
response, AuthenticationException failed) throws IOException, ServletException {
SecurityContextHolder.clearContext();
if (this.logger.isDebugEnabled()) {
this.logger.debug("Authentication request failed: " + failed.toString(), failed);
this.logger.debug("Updated SecurityContextHolder to contain null Authentication");
this.logger.debug("Delegating to authentication failure handler " +
this.failureHandler);
}
this.rememberMeServices.loginFail(request, response);
this.failureHandler.onAuthenticationFailure(request, response, failed);
}
}
```
```markdown
可见AbstractAuthenticationProcessingFilter这个过滤器对于认证成功与否，做了两个分支，成功执行
successfulAuthentication，失败执行unsuccessfulAuthentication。
在successfulAuthentication内部，将认证信息存储到了SecurityContext中。并调用了loginSuccess方法，这就是
常见的“记住我”功能！此功能具体应用，咱们后续再研究！
```
### 6.2 初步实现认证功能 
#### 6.2.1 让我们自己的UserService接口继承UserDetailsService
```java
public interface UserService extends UserDetailsService {
public void save(SysUser user);
public List<SysUs er> findAll();
public Map<String, Object> toAddRolePage(Integer id);
public void addRoleToUser(Integer userId, Integer[] ids);
}
```
#### 6.2.2 编写loadUserByUsername业务
```java
@Override
public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
SysUser sysUser = userDao.findByName(username);
if(sysUser==null){
//若用户名不对，直接返回null，表示认证失败。
return null;
}
List<SimpleGrantedAuthority> authorities = new ArrayList<>();
List<SysRole> roles = sysUser.getRoles();
for (SysRole role : roles) {
authorities.add(new SimpleGrantedAuthority(role.getRoleName()));
}
//最终需要返回一个SpringSecurity的UserDetails对象，{noop}表示不加密认证。
return new User(sysUser.getUsername(), "{noop}"+sysUser.getPassword(), authorities);
}
```
#### 6.2.3 在SpringSecurity主配置文件中指定认证使用的业务对象
```xml
<!--设置Spring Security认证用户信息的来源-->
<security:authentication-manager>
<security:authentication-provider user-service-ref="userServiceImpl">
</security:authentication-provider>
</security:authentication-manager>
```
### 6.3 加密认证
#### 6.3.1 在IOC容器中提供加密对象
```xml
<!--加密对象-->
<bean id="passwordEncoder"
class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder"/>
<!--设置Spring Security认证用户信息的来源-->
<security:authentication-manager>
<security:authentication-provider user-service-ref="userServiceImpl">
<!--指定认证使用的加密对象-->
<security:password-encoder ref="passwordEncoder"/>
</security:authentication-provider>
</security:authentication-manager>
```
#### 6.3.2 修改认证方法

```markdown
去掉{noop}
```
```java
@Override
public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
SysUser sysUser = userDao.findByName(username);
if(sysUser==null){
//若用户名不对，直接返回null，表示认证失败。
return null;
}
List<SimpleGrantedAuthority> authorities = new ArrayList<>();
List<SysRole> roles = sysUser.getRoles();
for (SysRole role : roles) {
authorities.add(new SimpleGrantedAuthority(role.getRoleName()));
}
//最终需要返回一个SpringSecurity的UserDetails对象，{noop}表示不加密认证。
return new User(sysUser.getUsername(), sysUser.getPassword(), authorities);
}
```
#### 6.3.3 修改添加用户的操作
```java

@Service
@Transactional
public class UserServiceImpl implements UserService {
@Autowired
private UserDao userDao;
@Autowired
private RoleService roleService;
@Autowired
private BCryptPasswordEncoder passwordEncoder;
@Override
public void save(SysUser user) {
//对密码进行加密，然后再入库
user.setPassword(passwordEncoder.encode(user.getPassword()));
userDao.save(user);
}
//……
}
```
#### 6.3.4 手动将数据库中用户密码改为加密后的密文
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112122115927.png#pic_center)
### 6.4 设置用户状态
#### 6.4.1 源码分析
```markdown
用户认证业务里，我们封装User对象时，选择了三个构造参
数的构造方法，其实还有另一个构造方法：
```

```java
public User(String username, String password, boolean enabled, boolean accountNonExpired,
boolean credentialsNonExpired, boolean accountNonLocked, Collection<? extends GrantedAuthority>
authorities) {
if (username != null && !"".equals(username) && password != null) {
this.username = username;
this.password = password;
this.enabled = enabled;
this.accountNonExpired = accountNonExpired;
this.credentialsNonExpired = credentialsNonExpired;
this.accountNonLocked = accountNonLocked;
this.authorities = Collections.unmodifiableSet(sortAuthorities(authorities));
} else {
throw new IllegalArgumentException("Cannot pass null or empty values to constructor");
}
}
```
```markdown
可以看到，这个构造方法里多了四个布尔类型的构造参数，其实我们
使用的三个构造参数的构造方法里这四个布尔
值默认都被赋值为了true，那么这四个布尔值到底是何意思呢？
boolean enabled 是否可用
boolean accountNonExpired 账户是否失效
boolean credentialsNonExpired 秘密是否失效
boolean accountNonLocked 账户是否锁定
```
#### 6.4.2  判断认证用户的状态
```markdown
这四个参数必须同时为true认证才可以，为了节省时间，我只
用第一个布尔值做个测试，修改认证业务代码：
```
```java
@Override
public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
SysUser sysUser = userDao.findByName(username);
if(sysUser==null){
//若用户名不对，直接返回null，表示认证失败。
return null;
}
List<SimpleGrantedAuthority> authorities = new ArrayList<>();
List<SysRole> roles = sysUser.getRoles();
for (SysRole role : roles) {
authorities.add(new SimpleGrantedAuthority(role.getRoleName()));
}
//最终需要返回一个SpringSecurity的UserDetails对象，{noop}表示不加密认证。
return new User(sysUser.getUsername(),
sysUser.getPassword(),
sysUser.getStatus()==1,   // 此刻，只有用户状态为1的用户才能成功通过认证！
true,
true,
true, authorities);
}
```
### 6.5 退出登录
```markdown
注意：一旦开启了csrf防护功能，logout处理器便只支持POST请
求方式了！
修改header.jsp中注销请求：
```

```html
<form action="${pageContext.request.contextPath}/logout" method="post">
<security:csrfInput/>
<input type="submit" value="注销">
</form>
```
### 6.6 remember me
#### 6.6.1 记住我功能原理分析
```markdown
还记得前面咱们分析认证流程时，提到的记住我功能吗？
现在继续跟踪找到AbstractRememberMeServices对象的
loginSuccess方法：
```
```java
public abstract class AbstractRememberMeServices implements RememberMeServices,
InitializingBean, LogoutHandler {
public final void loginSuccess(HttpServletRequest request, HttpServletResponse response,
Authentication successfulAuthentication) {
// 判断是否勾选记住我
// 注意：这里this.parameter点进去是上面的private String parameter = "remember-me";
if (!this.rememberMeRequested(request, this.parameter)) {
this.logger.debug("Remember-me login not requested.");
} else {
//若勾选就调用onLoginSuccess方法
this.onLoginSuccess(request, response, successfulAuthentication);
}
}
}

```

```markdown
再点进去上面if判断中的rememberMeRequested方法，还在当
前类中：
```
```java
protected boolean rememberMeRequested(HttpServletRequest request, String parameter) {
if (this.alwaysRemember) {
return true;
} else {
// 从上面的字parameter的值为"remember-me"
// 也就是说，此功能提交的属性名必须为"remember-me"
String paramValue = request.getParameter(parameter);
// 这里我们看到属性值可以为：true，on，yes，1。
if (paramValue != null && (paramValue.equalsIgnoreCase("true") ||
paramValue.equalsIgnoreCase("on") || paramValue.equalsIgnoreCase("yes") ||
paramValue.equals("1"))) {
//满足上面条件才能返回true
return true;
} else {
if (this.logger.isDebugEnabled()) {
this.logger.debug("Did not send remember-me cookie (principal did not set
parameter '" + parameter + "')");
}
return false;
}
}
}
```

```markdown
如果上面方法返回true，就表示页面勾选了记住我选项了。
继续顺着调用的方法找到PersistentTokenBasedRememberMeServices的onLoginSuccess方法：
```
```java
public class PersistentTokenBasedRememberMeServices extends AbstractRememberMeServices {
protected void onLoginSuccess(HttpServletRequest request, HttpServletResponse response,
Authentication successfulAuthentication) {
// 获取用户名
String username = successfulAuthentication.getName();
this.logger.debug("Creating new persistent login for user " + username);
//创建记住我的token
PersistentRememberMeToken persistentToken = new PersistentRememberMeToken(username,
this.generateSeriesData(), this.generateTokenData(), new Date());
try {
//将token持久化到数据库
this.tokenRepository.createNewToken(persistentToken);
//将token写入到浏览器的Cookie中
this.addCookie(persistentToken, request, response);
} catch (Exception var7) {
this.logger.error("Failed to save persistent token ", var7);
}
}
}
```
#### 6.6.2 记住我功能页面代码
```html
<p class="login-box-msg">登录系统</p>
<form action="${pageContext.request.contextPath}/login" method="post">
<security:csrfInput/>
<div class="form-group has-feedback">
<input type="text" name="username" class="form-control"
placeholder="用户名"> <span
class="glyphicon glyphicon-envelope form-controlfeedback"></span>
</div>
<div class="form-group has-feedback">
<input type="password" name="password" class="form-control"
placeholder="密码"> <span
class="glyphicon glyphicon-lock form-control-feedback">
</span>
</div>
<div class="row">
<div class="col-xs-8">
<div class="checkbox icheck">
<!-- 注意name和value属性的值不要写错哦 -->
<label><input type="checkbox" name="remember-me" value="true"> 记住 下次自动登录
</label>
</div>
</div>
<!-- /.col -->
<div class="col-xs-4">
<button type="submit" class="btn btn-primary btn-block btn-flat">登录</button>
</div>
<!-- /.col -->
</div>
</form>
```
```markdown
先测试一下，认证通过后，关掉浏览器，再次打开页面，发现还
要认证！为什么没有起作用呢？
这是因为remember me功能使用的过滤器RememberMeAuthenticationFilter默认是不开启的！
```
#### 6.6.3 开启remember me过滤器
```html
<!--设置可以用spring的el表达式配置Spring Security并自动生成对应配置组件（过滤器）-->
<security:http auto-config="true" use-expressions="true">
<!--省略其余配置-->
<!--开启remember me过滤器，设置token存储时间为60秒-->
<security:remember-me token-validity-seconds="60"/>
</security:http>
```
```markdown
说明：RememberMeAuthenticationFilter中功能非常简单，
会在打开浏览器时，自动判断是否认证，如果没有则
调用autoLogin进行自动认证。
```
#### 6.6.4 remember me安全性分析
```markdown
记住我功能方便是大家看得见的，但是安全性却令人担忧。
因为Cookie毕竟是保存在客户端的，很容易盗取，而且
cookie的值还与用户名、密码这些敏感数据相关，虽然加密了，
但是将敏感信息存在客户端，还是不太安全。那么
这就要提醒喜欢使用此功能的，用完网站要及时手动退出登录，
清空认证信息。
此外，SpringSecurity还提供了remember me的另一种相对更安
全的实现机制 :在客户端的cookie中，仅保存一个
无意义的加密串（与用户名、密码等敏感数据无关），然后
在db中保存该加密串-用户信息的对应关系，自动登录
时，用cookie中的加密串，到db中验证，如果通过，自
动登录才算通过。
```
#### 6.6.5 持久化remember me信息
```markdown
创建一张表，注意这张表的名称和字段都是固定的，不要修改
```
```sql
CREATE TABLE `persistent_logins` (
`username` varchar(64) NOT NULL,
`series` varchar(64) NOT NULL,
`token` varchar(64) NOT NULL,
`last_used` timestamp NOT NULL,
PRIMARY KEY (`series`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

```markdown
然后将spring-security.xml中 改为：
```
```xml
<!--
开启remember me过滤器，
data-source-ref="dataSource" 指定数据库连接池
token-validity-seconds="60" 设置token存储时间为60秒 可省略
remember-me-parameter="remember-me" 指定记住的参数名 可省略
-->
<security:remember-me data-source-ref="dataSource"
token-validity-seconds="60"
remember-me-parameter="remember-me"/>
```
```markdown
最后测试发现数据库中自动多了一条记录：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201121231539495.png#pic_center)
### 6.7 显示当前认证用户名
```markdown
在header.jsp中找到页面头部最右侧图片处添加如下信息：
```
```html
<span class="hidden-xs">
<security:authentication property="principal.username" />
</span>
或者
<span class="hidden-xs">
<security:authentication property="name" />
</span>
```
### 6.8 授权准备工作

```markdown
为了模拟授权操作，咱们临时编写两个业务功能：
```
```java
//ProductController
@Controller
@RequestMapping("/product")
public class ProductController {
@RequestMapping("/findAll")
public String findAll(){
return "product-list";
}
}
//OrderController
@Controller
@RequestMapping("/order")
public class OrderController {
@RequestMapping("/findAll")
public String findAll(){
return "order-list";
}
}
```
```html
<ul class="treeview-menu">
<li id="system-setting"><a href="${pageContext.request.contextPath}/product/findAll">
<i class="fa fa-circle-o"></i> 产品管理</a>
</li>
<li id="system-setting"><a href="${pageContext.request.contextPath}/order/findAll">
<i class="fa fa-circle-o"></i> 订单管理</a>
</li>
</ul>
```
#### 6.8.1 动态展示菜单
```markdown
在aside.jsp对每个菜单通过SpringSecurity标签库指定访问所需
角色
```
```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<%@taglib uri="http://www.springframework.org/security/tags" prefix="security" %>
<aside class="main-sidebar">
<!-- sidebar: style can be found in sidebar.less -->
<section class="sidebar">
<!-- Sidebar user panel -->
<div class="user-panel">
<div class="pull-left image">
<img src="${pageContext.request.contextPath}/img/user2-160x160.jpg"
class="img-circle" alt="User Image">
</div>
<div class="pull-left info">
<p>
<security:authentication property="principal.username"/>
</p>
<a href="#"><i class="fa fa-circle text-success"></i> 在线</a>
</div>
</div>
<!-- sidebar menu: : style can be found in sidebar.less -->
<ul class="sidebar-menu">
<li class="header">菜单</li>
<li id="admin-index"><a
href="${pageContext.request.contextPath}/pages/main.jsp"><i
class="fa fa-dashboard"></i> <span>首页</span></a></li>
<!-- 唯有超级管理员可以操作权限 -->
<security:authorize access="hasAnyRole('ROLE_ADMIN')">
<li class="treeview"><a href="#"> <i class="fa fa-cogs"></i>
<span>系统管理</span> <span class="pull-right-container"> <i
class="fa fa-angle-left pull-right"></i>
</span>
</a>
<ul class="treeview-menu">
<li id="system-setting"><a
href="${pageContext.request.contextPath}/user/findAll"> <i
class="fa fa-circle-o"></i> 用户管理
</a></li>
<li id="system-setting"><a
href="${pageContext.request.contextPath}/role/findAll"> <i
class="fa fa-circle-o"></i> 角色管理
</a></li>
<li id="system-setting"><a
href="${pageContext.request.contextPath}/pages/permissionlist.jsp">
<i class="fa fa-circle-o"></i> 权限管理
</a></li>
</ul>
</li>
</security:authorize>
<!-- 基础模块超级管理员和用户权限都可以操作 -->
<security:authorize access="hasAnyRole('ROLE_ADMIN', 'ROLE_USER')">
<li class="treeview"><a href="#"> <i class="fa fa-cube"></i
<span>基础数据</span> <span class="pull-right-container"> <i
class="fa fa-angle-left pull-right"></i>
</span>
</a>
<ul class="treeview-menu">
<!-- 产品模块还需要产品角色，注意这里还需再次指定超级管理员 -->
<security:authorize access="hasAnyRole('ROLE_ADMIN','ROLE_PRODUCT')">
<li id="system-setting"><a
href="${pageContext.request.contextPath}/product/findAll">
<i class="fa fa-circle-o"></i> 产品管理
</a></li>
</security:authorize>
<!-- 订单模块，有普通用户角色就可以操作了 -->
<li id="system-setting"><a
href="${pageContext.request.contextPath}/order/findAll">
<i class="fa fa-circle-o"></i> 订单管理
</a></li>
</ul>
</li>
</security:authorize>
</ul>
</section>
<!-- /.sidebar -->
</aside>
```

```markdown
我们做个测试，xiaozhi这个用户现在只有普通用户角色ROLE_USER，
用xiaozhi登录后，果然只看到了订单管理：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122000301931.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
那么问题来了，是不是现在已经授权成功了呢？答案是否定的！
你可以试试直接去访问产品的http请求地址：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122000340941.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
我们发现xiaozhi其实是可以操作产品模块的，只是没有把产品功能
展示给xiaozhi而已！
总结一句：页面动态菜单的展示只是为了用户体验，并未真
正控制权限！
```
#### 6.8.2 授权操作
```markdown
说明：SpringSecurity可以通过注解的方式来控制类或者方法的访问
权限。注解需要对应的注解支持，若注解放在
controller类中，对应注解支持应该放在mvc配置文件中，
因为controller类是有mvc配置文件扫描并创建的，同
理，注解放在service类中，对应注解支持应该放在spring配
置文件中。由于我们现在是模拟业务操作，并没有
service业务代码，所以就把注解放在controller类中了。
```
#### 6.8.3 开启授权的注解支持
```markdown
这里给大家演示三类注解，但实际开发中，用一类即可
```
```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:context="http://www.springframework.org/schema/context"
xmlns:aop="http://www.springframework.org/schema/aop"
xmlns:tx="http://www.springframework.org/schema/tx"
xmlns:mvc="http://www.springframework.org/schema/mvc"
xmlns:security="http://www.springframework.org/schema/security"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd
http://www.springframework.org/schema/aop
http://www.springframework.org/schema/aop/spring-aop.xsd
http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring-tx.xsd
http://www.springframework.org/schema/mvc
http://www.springframework.org/schema/mvc/spring-mvc.xsd
http://www.springframework.org/schema/security
http://www.springframework.org/schema/security/spring-security.xsd">
<context:component-scan base-package="com.yxj.controller"/>
<mvc:annotation-driven/>
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
<property name="prefix" value="/pages/"/>
<property name="suffix" value=".jsp"/>
</bean>
<mvc:default-servlet-handler/>
<!--
开启权限控制注解支持
jsr250-annotations="enabled"表示支持jsr250-api的注解，需要jsr250-api的jar包
pre-post-annotations="enabled"表示支持spring表达式注解
secured-annotations="enabled"这才是SpringSecurity提供的注解
-->
<security:global-method-security jsr250-annotations="enabled"
pre-post-annotations="enabled"
secured-annotations="enabled"/>
</beans>
```
#### 6.8.4 在注解支持对应类或者方法上添加注解
```java
//表示当前类中所有方法都需要ROLE_ADMIN或者ROLE_PRODUCT才能访问
@Controller
@RequestMapping("/product")
@RolesAllowed({"ROLE_ADMIN","ROLE_PRODUCT"})//JSR-250注解
public class ProductController {
@RequestMapping("/findAll")
public String findAll(){
return "product-list";
}
}
//表示当前类中findAll方法需要ROLE_ADMIN或者ROLE_PRODUCT才能访问
@Controller
@RequestMapping("/product")
public class ProductController {
@RequestMapping("/findAll")
@PreAuthorize("hasAnyRole('ROLE_ADMIN','ROLE_PRODUCT')")//spring表达式注解
public String findAll(){
return "product-list";
}
}
//表示当前类中所有方法都需要ROLE_ADMIN或者ROLE_PRODUCT才能访问
@Controller
@RequestMapping("/product")
@Secured({"ROLE_ADMIN","ROLE_PRODUCT"})//SpringSecurity注解
public class ProductController {
@RequestMapping("/findAll")
public String findAll(){
return "product-list";
}
}
```
#### 6.8.5 权限不足异常处理
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122075851167.png#pic_center)
```markdown
方式一：在spring-security.xml配置文件中处理
```

```xml
<!--设置可以用spring的el表达式配置Spring Security并自动生成对应配置组件（过滤器）-->
<security:http auto-config="true" use-expressions="true">
<!--省略其它配置-->
<!--403异常处理-->
<security:access-denied-handler error-page="/403.jsp"/>
</security:http>
```
```markdown
方式二：在web.xml中处理
```

```markdown
<error-page>
<error-code>403</error-code>
<location>/403.jsp</location>
</error-page>
```
```markdown
方式三：编写异常处理器
```
```java
@ControllerAdvice
public class ControllerExceptionAdvice {
//只有出现AccessDeniedException异常才调转403.jsp页面
@ExceptionHandler(AccessDeniedException.class)
public String exceptionAdvice(){
return "forward:/403.jsp";
}
}
```
## 7 SpringSecurity整合SpringBoot集中式版
```markdown
技术选型
SpringBoot2.1.3，SpringSecurity，MySQL，mybatis，jsp
```
### 7.1 初步整合认证第一版

```markdown
创建工程并导入jar包
先只导入SpringBoot
```
```xml
<parent>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-parent</artifactId>
<version>2.1.3.RELEASE</version>
<relativePath/>
</parent>
<dependencies>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
</dependency>
</dependencies>
```

```java
提供处理器
@Controller
@RequestMapping("/product")
public class ProductController {
@RequestMapping
@ResponseBody
public String hello(){
return "success";
}
}
```

```java
编写启动类
@SpringBootApplication
public class SecurityApplication {
public static void main(String[] args) {
SpringApplication.run(SecurityApplication.class, args);
}
}
```

```markdown
测试效果  
使用SpringBoot内置tomcat启动项目，即可访问处理器。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122090010387.png#pic_center)

```markdown
加入SpringSecurity的jar包
```
```xml
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

```markdown
重启再次测试
SpringBoot已经为SpringSecurity提供了默认配置，默认所有资源
都必须认证通过才能访问。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122090114592.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
那么问题来了！此刻并没有连接数据库，也并未在内存中指定认证
用户，如何认证呢？
其实SpringBoot已经提供了默认用户名user，密码在项目启动时
随机生成，如图：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122090155999.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
认证通过后可以继续访问处理器资源：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112209030328.png#pic_center)
### 7.2 整合认证第二版【加入jsp使用自定义认证页面】
```markdown
说明
SpringBoot官方是不推荐在SpringBoot中使用jsp的，那么到底可
以使用吗？答案是肯定的！
不过需要导入tomcat插件启动项目，不能再用SpringBoot
默认tomcat了。
```

```xml
导入SpringBoot的tomcat启动插件jar包
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-tomcat</artifactId>
</dependency>
<dependency>
<groupId>org.apache.tomcat.embed</groupId>
<artifactId>tomcat-embed-jasper</artifactId>
</dependency>
```

```markdown
加入jsp页面等静态资源
在src/main目录下创建webapp目录
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122090509224.png#pic_center)

```markdown
这时webapp目录并不能正常使用，因为只有web工程才有webapp目
录，在pom文件中修改项目为web工程
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112209061327.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
这时webapp目录，可以正常使用了！
注意WEB-INF就不用了哈！
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112209062935.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122090749655.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
修改login.jsp中认证的url地址
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122090824751.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
修改header.jsp中退出登录的url地址
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122091112394.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
提供SpringSecurity配置类
```
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
/**
* 这里先不连接数据库了
*/
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
auth.inMemoryAuthentication()
.withUser("user")
.password("{noop}123")
.roles("USER");
}
protected void configure(HttpSecurity http) throws Exception {
http.authorizeRequests()
.antMatchers("/login.jsp", "/failer.jsp", "/css/**", "/img/**",
"/plugins/**").permitAll()
.antMatchers("/**").hasAnyRole("USER")
.anyRequest()
.authenticated()
.and()
.formLogin()
.loginPage("/login.jsp")
.loginProcessingUrl("/login")
.successForwardUrl("/index.jsp")
.failureForwardUrl("/failer.jsp")
.permitAll()
.and()
.logout()
.logoutUrl("/logout")
.invalidateHttpSession(true)
.logoutSuccessUrl("/login.jsp")
.permitAll()
.and()
.csrf()
.disable();
}
}
```

```markdown
修改产品处理器
有页面了，就跳转一个真的吧！
```
```java
@Controller
@RequestMapping("/product")
public class ProductController {
@RequestMapping("/findAll")
public String findAll(){
return "product-list";
}
}
```
```markdown
配置视图解析器
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122094408977.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
使用tomcat插件启动项目
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122094443951.png#pic_center)
```markdown
测试效果
```

```java
自定义的认证页面
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122094510822.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
认证成功页面
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122094534613.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 7.3 整合认证第三版【数据库认证】
```markdown
数据库环境准备
```


```markdown
导入数据库操作相关jar包
```
```xml
<dependency>
<groupId>mysql</groupId>
<artifactId>mysql-connector-java</artifactId>
<version>5.1.47</version>
</dependency>
<dependency>
<groupId>tk.mybatis</groupId>
<artifactId>mapper-spring-boot-starter</artifactId>
<version>2.1.5</version>
</dependency>
```
```markdown
在配置文件中添加数据库操作相关配置
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122094744909.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
在启动类上添加扫描dao接口包注解
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122094806707.png#pic_center)
```markdown
创建角色pojo对象
```

```markdown
这里直接使用SpringSecurity的角色规范
```
```java
public class SysRole implements GrantedAuthority {
private Integer id;
private String roleName;
private String roleDesc;
public Integer getId() {
return id;
}
public void setId(Integer id) {
this.id = id;
}
public String getRoleName() {
return roleName;
}
public void setRoleName(String roleName) {
this.roleName = roleName;
}
public String getRoleDesc() {
return roleDesc;
}
public void setRoleDesc(String roleDesc) {
this.roleDesc = roleDesc;
}
//标记此属性不做json处理
@JsonIgnore
@Override
public String getAuthority() {
return roleName;
}
}
```
```markdown
这里直接实现SpringSecurity的用户对象接口，并添加角色集合
私有属性。
注意接口属性都要标记不参与json的处理。
```
```java
public class SysUser implements UserDetails {
private Integer id;
private String username;
private String password;
private Integer status;
private List<SysRole> roles = new ArrayList<>();
public Integer getId() {
return id;
}
public void setId(Integer id) {
this.id = id;
}
public void setUsername(String username) {
this.username = username;
}
public void setPassword(String password) {
this.password = password;
}
public Integer getStatus() {
return status;
}
public void setStatus(Integer status) {
this.status = status;
}
public List<SysRole> getRoles() {
return roles;
}
public void setRoles(List<SysRole> roles) {
this.roles = roles;
}
@JsonIgnore
@Override
public Collection<? extends GrantedAuthority> getAuthorities() {
return roles;
}
@Override
public String getPassword() {
return password;
}
@Override
public String getUsername() {
return username;
}
@JsonIgnore
@Override
public boolean isAccountNonExpired() {
return true;
}
@JsonIgnore
@Override
public boolean isAccountNonLocked() {
return true;
}
@JsonIgnore
@Override
public boolean isCredentialsNonExpired() {
return true;
}
@JsonIgnore
@Override
public boolean isEnabled() {
return true;
}
}
```
```markdown
这里直接实现SpringSecurity的用户对象接口，并添加角色
集合私有属性。
注意接口属性都要标记不参与json的处理。
```
```java
public class SysUser implements UserDetails {
private Integer id;
private String username;
private String password;
private Integer status;
private List<SysRole> roles = new ArrayList<>();
public Integer getId() {
return id;
}
public void setId(Integer id) {
this.id = id;
}
public void setUsername(String username) {
this.username = username;
}
public void setPassword(String password) {
this.password = password;
}
public Integer getStatus() {
return status;
}
public void setStatus(Integer status) {
this.status = status;
}
public List<SysRole> getRoles() {
return roles;
}
public void setRoles(List<SysRole> roles) {
this.roles = roles;
}
@JsonIgnore
@Override
public Collection<? extends GrantedAuthority> getAuthorities() {
return roles;
}
@Override
public String getPassword() {
return password;
}
@Override
public String getUsername() {
return username;
}
@JsonIgnore
@Override
public boolean isAccountNonExpired() {
return true;
}
@JsonIgnore
@Override
public boolean isAccountNonLocked() {
return true;
}
@JsonIgnore
@Override
public boolean isCredentialsNonExpired() {
return true;
}
@JsonIgnore
@Override
public boolean isEnabled() {
return true;
}
}
```
```markdown
提供角色mapper接口
```
```java
public interface RoleMapper extends Mapper<SysRole> {
@Select("SELECT r.id, r.role_name roleName, r.role_desc roleDesc " +
"FROM sys_role r, sys_user_role ur " +
"WHERE r.id=ur.rid AND ur.uid=#{uid}")
public List<SysRole> findByUid(Integer uid);
}
```
```markdown
提供用户mapper接口
```
```java
public interface UserMapper extends Mapper<SysUser> {
@Select("select * from sys_user where username=#{username}")
@Results({
@Result(id = true, property = "id", column = "id"),
@Result(property = "roles", column = "id", javaType = List.class,
many = @Many(select = "com.yxj.mapper.RoleMapper.findByUid"))
})
public SysUser findByUsername(String username);
}
```
```markdown
提供认证service接口
```
```java
package com.yxj.service;
import org.springframework.security.core.userdetails.UserDetailsService;
public interface UserService extends UserDetailsService {
}
```

```markdown
提供认证service实现类
```
```java
@Service
@Transactional
public class UserServiceImpl implements UserService {
@Autowired
private UserMapper userMapper;
@Override
public UserDetails loadUserByUsername(String s) throws UsernameNotFoundException {
return userMapper.findByUsername(s);
}
}
```
```markdown
在启动类中把加密对象放入IOC容器
```
```java
@SpringBootApplication
@MapperScan("com.yxj.mapper")
public class SecurityApplication {
public static void main(String[] args) {
SpringApplication.run(SecurityApplication.class, args);
}
@Bean
public BCryptPasswordEncoder passwordEncoder(){
return new BCryptPasswordEncoder();
}
}
```
```markdown
修改配置类
```
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
@Autowired
private UserService userService;
@Autowired
private BCryptPasswordEncoder passwordEncoder;
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
auth.userDetailsService(userService).passwordEncoder(passwordEncoder);
}
protected void configure(HttpSecurity http) throws Exception {
http.authorizeRequests()
.antMatchers("/login.jsp", "/failer.jsp", "/css/**", "/img/**",
"/plugins/**").permitAll()
.antMatchers("/**").hasAnyRole("USER")
.anyRequest()
.authenticated()
.and()
.formLogin()
.loginPage("/login.jsp")
.loginProcessingUrl("/login")
.successForwardUrl("/index.jsp")
.failureForwardUrl("/failer.jsp")
.permitAll()
.and()
.logout()
.logoutUrl("/logout")
.invalidateHttpSession(true)
.logoutSuccessUrl("/login.jsp")
.permitAll()
.and()
.csrf()
.disable();
}
}
```
```markdown
注意还是用插件启动项目，使用数据库表中的用户名和密码
```

```markdown
整合实现授权功能
在启动类上添加开启方法级的授权注解
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122150433550.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
在产品处理器类上添加注解\
要求产品列表功能必须具有ROLE_ADMIN角色才能访问
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122150532192.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
重启项目测试
再次访问产品列表发现权限不足
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122150603388.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
指定自定义异常页面
编写异常处理器拦截403异常
```
```java
@ControllerAdvice
public class HandleControllerException {
@ExceptionHandler(RuntimeException.class)
public String exceptionHandler(RuntimeException e){
if(e instanceof AccessDeniedException){
//如果是权限不足异常，则跳转到权限不足页面！
return "redirect:/403.jsp";
}
//其余的异常都到500页面！
return "redirect:/500.jsp";
}
}
```
```markdown
再次测试产品列表就可以到自定义异常页面了
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122150914442.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
## 8 SpringSecurity整合SpringBoot分布式版
```markdown
分布式认证概念说明
分布式认证，即我们常说的单点登录，简称SSO，指的是在多应
用系统的项目中，用户只需要登录一次，就可以访
问所有互相信任的应用系统。
```
```markdown
分布式认证流程图
首先，我们要明确，在分布式项目中，每台服务器都有各自
独立的session，而这些session之间是无法直接共享资
源的，所以，session通常不能被作为单点登录的技术方案。
```
```markdown
最合理的单点登录方案流程如下图所示
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122172953884.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
总结一下，单点登录的实现分两大环节：
用户认证：这一环节主要是用户向认证服务器发起认证请求，认证
服务器给用户返回一个成功的令牌token，
主要在认证服务器中完成，即图中的A系统，注意A系统只能有一个。
身份校验：这一环节是用户携带token去访问其他服务器时，在其他
服务器中要对token的真伪进行检验，主
要在资源服务器中完成，即图中的B系统，这里B系统可以有很多个。
```
## 9 JWT介绍
### 9.1 概念说明
```markdown
从分布式认证流程中，我们不难发现，这中间起最关键作用的就是
token，token的安全与否，直接关系到系统的
健壮性，这里我们选择使用JWT来实现token的生成和校验。
JWT，全称JSON Web Token，官网地址https://jwt.io，是
一款出色的分布式身份校验方案。可以生成token，也可
以解析检验token。
```
### 9.2 JWT生成的token由三部分组成

```markdown
头部：主要设置一些规范信息，签名部分的编码格式就在头部中声明。
载荷：token中存放有效信息的部分，比如用户名，用户角色，
过期时间等，但是不要放密码，会泄露！
签名：将头部与载荷分别采用base64编码后，用“.”相连，再加入盐，
最后使用头部声明的编码类型进行编
码，就得到了签名。
```
### 9.3 JWT生成token的安全性分析

```markdown
从JWT生成的token组成上来看，要想避免token被伪造，主要就得看
签名部分了，而签名部分又有三部分组成，其
中头部和载荷的base64编码，几乎是透明的，毫无安全性可言，
那么最终守护token安全的重担就落在了加入的盐上面了！
试想：如果生成token所用的盐与解析token时加入的盐是一样的。岂
不是类似于中国人民银行把人民币防伪技术
公开了？大家可以用这个盐来解析token，就能用来伪造token。
这时，我们就需要对盐采用非对称加密的方式进行加密，以达到
生成token与校验token方所用的盐不一致的安全效果！
```
### 9.4 非对称加密RSA介绍

```markdown
基本原理：同时生成两把密钥：私钥和公钥，私钥隐秘保存，
公钥可以下发给信任客户端
私钥加密，持有私钥或公钥才可以解密
公钥加密，持有私钥才可解密
优点：安全，难以破解
缺点：算法比较耗时，为了安全，可以接受
历史：三位数学家Rivest、Shamir 和 Adleman 设计了一种算法，
可以实现非对称加密。这种算法用他们三
个人的名字缩写：RSA。
```
```markdown
JWT相关工具类
```

```xml
<dependency>
<groupId>io.jsonwebtoken</groupId>
<artifactId>jjwt-api</artifactId>
<version>0.10.7</version>
</dependency>
<dependency>
<groupId>io.jsonwebtoken</groupId>
<artifactId>jjwt-impl</artifactId>
<version>0.10.7</version>
<scope>runtime</scope>
</dependency>
<dependency>
<groupId>io.jsonwebtoken</groupId>
<artifactId>jjwt-jackson</artifactId>
<version>0.10.7</version>
<scope>runtime</scope>
</dependency>
```
```markdown
载荷对象
```

```java
/**
* @author 黑马程序员
* 为了方便后期获取token中的用户信息，将token中载荷部分单独封装成一个对象
*/
@Data
public class Payload<T> {
private String id;
private T userInfo;
private Date expiration;
}
```
```markdown
JWT工具类
```
```java
/**
* @author: 黑马程序员
* 生成token以及校验token相关方法
*/
public class JwtUtils {
private static final String JWT_PAYLOAD_USER_KEY = "user";
/**
* 私钥加密token
*
* @param userInfo 载荷中的数据
* @param privateKey 私钥
* @param expire 过期时间，单位分钟
* @return JWT
*/
public static String generateTokenExpireInMinutes(Object userInfo, PrivateKey privateKey,
int expire) {
return Jwts.builder()
.claim(JWT_PAYLOAD_USER_KEY, JsonUtils.toString(userInfo))
.setId(createJTI())
.setExpiration(DateTime.now().plusMinutes(expire).toDate())
.signWith(privateKey, SignatureAlgorithm.RS256)
.compact();
}
/**
* 私钥加密token
*
* @param userInfo 载荷中的数据
* @param privateKey 私钥
* @param expire 过期时间，单位秒
* @return JWT
*/
public static String generateTokenExpireInSeconds(Object userInfo, PrivateKey privateKey,
int expire) {
return Jwts.builder()
.claim(JWT_PAYLOAD_USER_KEY, JsonUtils.toString(userInfo))
.setId(createJTI())
.setExpiration(DateTime.now().plusSeconds(expire).toDate())
.signWith(privateKey, SignatureAlgorithm.RS256)
.compact();
}
/**
* 公钥解析token
*
* @param token 用户请求中的token
* @param publicKey 公钥
* @return Jws<Claims>
*/
private static Jws<Claims> parserToken(String token, PublicKey publicKey) {
return Jwts.parser().setSigningKey(publicKey).parseClaimsJws(token);
}
private static String createJTI() {
return new String(Base64.getEncoder().encode(UUID.randomUUID().toString().getBytes()));
}
/**
* 获取token中的用户信息
*
* @param token 用户请求中的令牌
* @param publicKey 公钥
* @return 用户信息
*/
public static <T> Payload<T> getInfoFromToken(String token, PublicKey publicKey, Class<T>
userType) {
Jws<Claims> claimsJws = parserToken(token, publicKey);
Claims body = claimsJws.getBody();
Payload<T> claims = new Payload<>();
claims.setId(body.getId());
claims.setUserInfo(JsonUtils.toBean(body.get(JWT_PAYLOAD_USER_KEY).toString(),
userType));
claims.setExpiration(body.getExpiration());
return claims;
}
/**
* 获取token中的载荷信息
*
* @param token 用户请求中的令牌
* @param publicKey 公钥
* @return 用户信息
*/
public static <T> Payload<T> getInfoFromToken(String token, PublicKey publicKey) {
Jws<Claims> claimsJws = parserToken(token, publicKey);
Claims body = claimsJws.getBody();
Payload<T> claims = new Payload<>();
claims.setId(body.getId());
claims.setExpiration(body.getExpiration());
return claims;
}
}
```
```markdown
RSA工具类
```

```java
/**
* @author 黑马程序员
*/
public class RsaUtils {
private static final int DEFAULT_KEY_SIZE = 2048;
/**
* 从文件中读取公钥
*
* @param filename 公钥保存路径，相对于classpath
* @return 公钥对象
* @throws Exception
*/
public static PublicKey getPublicKey(String filename) throws Exception {
byte[] bytes = readFile(filename);
return getPublicKey(bytes);
}
/**
* 从文件中读取密钥
*
* @param filename 私钥保存路径，相对于classpath
* @return 私钥对象
* @throws Exception
*/
public static PrivateKey getPrivateKey(String filename) throws Exception {
byte[] bytes = readFile(filename);
return getPrivateKey(bytes);
}
/**
* 获取公钥
*
* @param bytes 公钥的字节形式
* @return
* @throws Exception
*/
private static PublicKey getPublicKey(byte[] bytes) throws Exception {
bytes = Base64.getDecoder().decode(bytes);
X509EncodedKeySpec spec = new X509EncodedKeySpec(bytes);
KeyFactory factory = KeyFactory.getInstance("RSA");
return factory.generatePublic(spec);
}
/**
* 获取密钥
*
* @param bytes 私钥的字节形式
* @return
* @throws Exception
*/
private static PrivateKey getPrivateKey(byte[] bytes) throws NoSuchAlgorithmException,
InvalidKeySpecException {
bytes = Base64.getDecoder().decode(bytes);
PKCS8EncodedKeySpec spec = new PKCS8EncodedKeySpec(bytes);
KeyFactory factory = KeyFactory.getInstance("RSA");
return factory.generatePrivate(spec);
}
/**
* 根据密文，生存rsa公钥和私钥,并写入指定文件
*
* @param publicKeyFilename 公钥文件路径
* @param privateKeyFilename 私钥文件路径
* @param secret 生成密钥的密文
*/
public static void generateKey(String publicKeyFilename, String privateKeyFilename, String
secret, int keySize) throws Exception {
KeyPairGenerator keyPairGenerator = KeyPairGenerator.getInstance("RSA");
SecureRandom secureRandom = new SecureRandom(secret.getBytes());
keyPairGenerator.initialize(Math.max(keySize, DEFAULT_KEY_SIZE), secureRandom);
KeyPair keyPair = keyPairGenerator.genKeyPair();
// 获取公钥并写出
byte[] publicKeyBytes = keyPair.getPublic().getEncoded();
publicKeyBytes = Base64.getEncoder().encode(publicKeyBytes);
writeFile(publicKeyFilename, publicKeyBytes);
// 获取私钥并写出
byte[] privateKeyBytes = keyPair.getPrivate().getEncoded();
privateKeyBytes = Base64.getEncoder().encode(privateKeyBytes);
writeFile(privateKeyFilename, privateKeyBytes);
}
private static byte[] readFile(String fileName) throws Exception {
return Files.readAllBytes(new File(fileName).toPath());
}
private static void writeFile(String destPath, byte[] bytes) throws IOException {
File dest = new File(destPath);
if (!dest.exists()) {
dest.createNewFile();
}
Files.write(dest.toPath(), bytes);
}
}
```
### 9.5 SpringSecurity+JWT+RSA分布式认证思路分析
```markdown
SpringSecurity主要是通过过滤器来实现功能的！我们要找
到SpringSecurity实现认证和校验身份的过滤器！
```

```markdown
回顾集中式认证流程
用户认证：
使用UsernamePasswordAuthenticationFilter过滤器
中attemptAuthentication方法实现认证功能，该过滤
器父类中successfulAuthentication方法实现认证成功后的操作。
身份校验：
使用BasicAuthenticationFilter过滤器中doFilterInternal
方法验证是否登录，以决定能否进入后续过滤器
```

```markdown
分析分布式认证流程
用户认证：
由于，分布式项目，多数是前后端分离的架构设计，我们要
满足可以接受异步post的认证请求参数，需要修
改UsernamePasswordAuthenticationFilter过滤器
中attemptAuthentication方法，让其能够接收请求体。
另外，默认successfulAuthentication方法在认证通过后，是
把用户信息直接放入session就完事了，现在我
们需要修改这个方法，在认证通过后生成token并返回给用户。
身份校验：
原来BasicAuthenticationFilter过滤器中doFilterInternal
方法校验用户是否登录，就是看session中是否有用户信息，
我们要修改为，验证用户携带的token是否合法，并解析出用户信息，
交给SpringSecurity，以便于后续的授权功能可以正常使用。
```
### 9.6 SpringSecurity+JWT+RSA分布式认证实现
```markdown
创建父工程并导入jar包
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122181932129.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
通用模块
创建通用子模块并导入JWT相关jar包
```
```xml
<parent>
<artifactId>springboot_security_jwt_rsa</artifactId>
<groupId>com.yxj</groupId>
<version>1.0-SNAPSHOT</version>
</parent>
<modelVersion>4.0.0</modelVersion>
<artifactId>heima_common</artifactId>
<dependencies>
<!--jwt所需jar包-->
<dependency>
<groupId>io.jsonwebtoken</groupId>
<artifactId>jjwt-api</artifactId>
<version>0.10.7</version>
</dependency>
<dependency>
<groupId>io.jsonwebtoken</groupId>
<artifactId>jjwt-impl</artifactId>
<version>0.10.7</version>
<scope>runtime</scope>
</dependency>
<dependency>
<groupId>io.jsonwebtoken</groupId>
<artifactId>jjwt-jackson</artifactId>
<version>0.10.7</version>
<scope>runtime</scope>
</dependency>
<!--lombok插件-->
<dependency>
<groupId>org.projectlombok</groupId>
<artifactId>lombok</artifactId>
</dependency>
<!--处理日期工具包-->
<dependency>
<groupId>joda-time</groupId>
<artifactId>joda-time</artifactId>
</dependency>
<!--处理json工具包-->
<dependency>
<groupId>com.fasterxml.jackson.core</groupId>
<artifactId>jackson-databind</artifactId>
<version>2.9.9</version>
</dependency>
<!--日志包-->
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-logging</artifactId>
</dependency>
<!--测试包-->
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-test</artifactId>
</dependency>
</dependencies>
```
```markdown
导入工具类
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112218310910.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
在通用子模块中编写测试类生成rsa公钥和私钥
```

```java
public class RsaUtilsTest {
private String publicFile = "D:\\auth_key\\rsa_key.pub";
private String privateFile = "D:\\auth_key\\rsa_key";
@Test
public void generateKey() throws Exception {
RsaUtils.generateKey(publicFile, privateFile, "heima", 2048);
}
}
```
```markdown
执行后查看D:\auth_key目录发现私钥和公钥文件生成成功
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122183240664.png#pic_center)

```markdown
认证服务
创建认证服务工程并导入jar包
```
```xml
<parent>
<artifactId>springboot_security_jwt_rsa</artifactId>
<groupId>com.yxj</groupId>
<version>1.0-SNAPSHOT</version>
</parent>
<modelVersion>4.0.0</modelVersion>
<artifactId>heima_auth_server</artifactId>
<dependencies>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
<groupId>com.yxj</groupId>
<artifactId>heima_common</artifactId>
<version>1.0-SNAPSHOT</version>
</dependency>
<dependency>
<groupId>mysql</groupId>
<artifactId>mysql-connector-java</artifactId>
<version>5.1.47</version>
</dependency>
<dependency>
<groupId>org.mybatis.spring.boot</groupId>
<artifactId>mybatis-spring-boot-starter</artifactId>
<version>2.1.0</version>
</dependency>
</dependencies>
```
```markdown
创建认证服务配置文件
```
```yml
server:
port: 9001
datasource:
driver-class-name: com.mysql.jdbc.Driver
url: jdbc:mysql:///security_authority
username: root
password: root
mybatis:
type-aliases-package: com.yxj.domain
configuration:
map-underscore-to-camel-case: true
logging:
level:
com.yxj: debug
heima:
key:
pubKeyPath: D:\\auth_key\\rsa_key.pub
priKeyPath: D:\\auth_key\\rsa_key
```
```markdown
提供解析公钥和私钥的配置类
```
```java
@Data
@ConfigurationProperties(prefix = "heima.key")
public class RsaKeyProperties {
private String pubKeyPath;
private String priKeyPath;
private PublicKey publicKey;
private PrivateKey privateKey;
@PostConstruct
public void loadKey() throws Exception {
publicKey = RsaUtils.getPublicKey(pubKeyPath);
privateKey = RsaUtils.getPrivateKey(priKeyPath);
}
}
```
```markdown
创建认证服务启动类
```
```java
@SpringBootApplication
@MapperScan("com.yxj.mapper")
@EnableConfigurationProperties(RsaKeyProperties.class)
public class AuthApplication {
public static void main(String[] args) {
	SpringApplication.run(AuthApplication.class, args);
}
}
```
```markdown
将上面集中式案例中数据库认证相关代码复制到认证服务中
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122184839169.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
编写认证过滤器
```
```java
public class TokenLoginFilter extends UsernamePasswordAuthenticationFilter {
	private AuthenticationManager authenticationManager;
	private RsaKeyProperties prop;
public TokenLoginFilter(AuthenticationManager authenticationManager, RsaKeyProperties prop)
{
this.authenticationManager = authenticationManager;
this.prop = prop;
}
/**
* 接收并解析用户凭证，出現错误时，返回json数据前端
*/
@Override
public Authentication attemptAuthentication(HttpServletRequest req, HttpServletResponse res)
{
	try {
		//将json格式请求体转成JavaBean对象
		SysUser user = new ObjectMapper().readValue(req.getInputStream(), SysUser.class);
		return authenticationManager.authenticate(
		new UsernamePasswordAuthenticationToken(
		user.getUsername(),
		user.getPassword())
	);
} catch (Exception e) {
try {
	//如果认证失败，提供自定义json格式异常
	res.setContentType("application/json;charset=utf-8");
	res.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
	PrintWriter out = res.getWriter();
	Map<String, Object> map = new HashMap<String, Object>();
	map.put("code", HttpServletResponse.SC_UNAUTHORIZED);
	map.put("message", "账号或密码错误！");
	out.write(new ObjectMapper().writeValueAsString(map));
	out.flush();
	out.close();
} catch (Exception e1) {
e1.printStackTrace();
}
throw new RuntimeException(e);
}
}
/**
* 用户登录成功后，生成token,并且返回json数据给前端
*/
@Override
protected void successfulAuthentication(HttpServletRequest req, HttpServletResponse res,FilterChain chain, Authentication auth) {
//得到当前认证的用户对象
SysUser user = new SysUser();
user.setUsername(auth.getName());
user.setRoles((List<SysRole>) auth.getAuthorities());
//json web token构建
String token = JwtUtils.generateTokenExpireInMinutes(user, prop.getPrivateKey(), 24*60);
//返回token
res.addHeader("Authorization", "Bearer " + token);
try {
//登录成功時，返回json格式进行提示
res.setContentType("application/json;charset=utf-8");
res.setStatus(HttpServletResponse.SC_OK);
PrintWriter out = res.getWriter();
Map<String, Object> map = new HashMap<String, Object>();
map.put("code", HttpServletResponse.SC_OK);
map.put("message", "登陆成功！");
out.write(new ObjectMapper().writeValueAsString(map));
out.flush();
out.close();
} catch (Exception e1) {
e1.printStackTrace();
}
}
}
```
```markdown
编写检验token过滤器
```

```java
public class TokenVerifyFilter extends BasicAuthenticationFilter {
private RsaKeyProperties prop;
public TokenVerifyFilter(AuthenticationManager authenticationManager, RsaKeyProperties prop)
{
super(authenticationManager);
this.prop = prop;
}
/**
* 过滤请求
*/
@Override
protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response,
FilterChain chain) {
try {
//请求体的头中是否包含Authorization
String header = request.getHeader("Authorization");
//Authorization中是否包含Bearer，不包含直接返回
if (header == null || !header.startsWith("Bearer ")) {
chain.doFilter(request, response);
responseJson(response);
return;
}
//获取权限失败，会抛出异常
UsernamePasswordAuthenticationToken authentication = getAuthentication(request);
//获取后，将Authentication写入SecurityContextHolder中供后序使用
SecurityContextHolder.getContext().setAuthentication(authentication);
chain.doFilter(request, response);
} catch (Exception e) {
responseJson(response);
e.printStackTrace();
}
}
/**
* 未登录提示
* @param response
*/
private void responseJson(HttpServletResponse response) {
try {
//未登录提示
response.setContentType("application/json;charset=utf-8");
response.setStatus(HttpServletResponse.SC_FORBIDDEN);
PrintWriter out = response.getWriter();
Map<String, Object> map = new HashMap<String, Object>();
map.put("code", HttpServletResponse.SC_FORBIDDEN);
map.put("message", "请登录！");
out.write(new ObjectMapper().writeValueAsString(map));
out.flush();
out.close();
} catch (Exception e1) {
e1.printStackTrace();
}
}
/**
* 通过token，获取用户信息
*
* @param request
* @return
*/
private UsernamePasswordAuthenticationToken getAuthentication(HttpServletRequest request) {
String token = request.getHeader("Authorization");
if (token != null) {
//通过token解析出载荷信息
Payload<SysUser> payload = JwtUtils.getInfoFromToken(token.replace("Bearer ", ""),
prop.getPublicKey(), SysUser.class);
SysUser user = payload.getUserInfo();
//不为null，返回
if (user != null) {
return new UsernamePasswordAuthenticationToken(user, null, user.getRoles());
}
return null;
}
return null;
}
}
```
```markdown
编写SpringSecurity配置类
```

```java
@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(securedEnabled = true)
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
@Autowired
private UserDetailsService myCustomUserService;
@Autowired
private RsaKeyProperties prop;
@Bean
public BCryptPasswordEncoder myPasswordEncoder(){
return new BCryptPasswordEncoder();
}
@Override
protected void configure(HttpSecurity http) throws Exception {
http
//关闭跨站请求防护
.cors().and().csrf().disable()
//允许不登陆就可以访问的方法，多个用逗号分隔
.authorizeRequests().antMatchers("/product").hasAnyRole("USER")
//其他的需要授权后访问
.anyRequest().authenticated()
.and()
//增加自定义认证过滤器
.addFilter(new TokenLoginFilter(authenticationManager(), prop))
//增加自定义验证认证过滤器
.addFilter(new TokenVerifyFilter(authenticationManager(), prop))
// 前后端分离是无状态的，不用session了，直接禁用。
.sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);
}
@Override
public void configure(AuthenticationManagerBuilder auth) throws Exception {
//UserDetailsService类
auth.userDetailsService(myCustomUserService)
//加密策略
.passwordEncoder(myPasswordEncoder());
}
}
```
```markdown
启动测试认证服务
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122190125520.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
认证通过结果 
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122190200693.png#pic_center)
```markdown
token在Headers中
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122190230982.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
验证认证请求
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112219030374.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
资源服务
```

```markdown
说明
资源服务可以有很多个，这里只拿产品服务为例，记住，
资源服务中只能通过公钥验证认证。不能签发token！
```
```markdown
创建产品服务并导入jar包
根据实际业务导包即可，咱们就暂时和认证服务一样了。
```
```xml
<dependencies>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
<groupId>com.yxj</groupId>
<artifactId>heima_common</artifactId>
<version>1.0-SNAPSHOT</version>
</dependency>
<dependency>
<groupId>mysql</groupId>
<artifactId>mysql-connector-java</artifactId>
<version>5.1.47</version>
</dependency>
<dependency>
<groupId>org.mybatis.spring.boot</groupId>
<artifactId>mybatis-spring-boot-starter</artifactId>
<version>2.1.0</version>
</dependency>
</dependencies>
```
```markdown
编写产品服务配置文件
切记这里只能有公钥地址！
```
```yml
server:
port: 9002
spring:
datasource:
driver-class-name: com.mysql.jdbc.Driver
url: jdbc:mysql:///security_authority
username: root
password: root
mybatis:
type-aliases-package: com.yxj.domain
configuration:
map-underscore-to-camel-case: true
logging:
level:
com.yxj: debug
heima:
key:
pubKeyPath: D:\\auth_key\\rsa_key.pub
```
```markdown
编写读取公钥的配置类
```

```java
@Data
@ConfigurationProperties(prefix = "heima.key")
public class RsaKeyProperties {
private String pubKeyPath;
private PublicKey publicKey;
@PostConstruct
public void loadKey() throws Exception {
publicKey = RsaUtils.getPublicKey(pubKeyPath);
}
}
```
```markdown
编写启动类
```

```java
@SpringBootApplication
@MapperScan("com.yxj.mapper")
@EnableConfigurationProperties(RsaKeyProperties.class)
public class ProductApplication {
public static void main(String[] args) {
SpringApplication.run(ProductApplication.class, args);
}
}
```

```markdown
复制认证服务中，用户对象，角色对象和校验认证的接口
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122193400768.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
复制认证服务中SpringSecurity配置类做修改
去掉“增加自定义认证过滤器”即可！
```
```java
@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(securedEnabled = true)
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
@Autowired
private UserDetailsService myCustomUserService;
@Autowired
private RsaKeyProperties prop;
@Bean
public BCryptPasswordEncoder myPasswordEncoder(){
return new BCryptPasswordEncoder();
}
@Override
protected void configure(HttpSecurity http) throws Exception {
http
//关闭跨站请求防护
.cors().and().csrf().disable()
//允许不登陆就可以访问的方法，多个用逗号分隔
.authorizeRequests().antMatchers("/product").hasAnyRole("USER")
//其他的需要授权后访问
.anyRequest().authenticated()
.and()
//增加自定义验证认证过滤器
.addFilter(new TokenVerifyFilter(authenticationManager(), prop))
// 前后端分离是无状态的，不用session了，直接禁用。
.sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);
}
@Override
public void configure(AuthenticationManagerBuilder auth) throws Exception {
//UserDetailsService类
auth.userDetailsService(myCustomUserService)
//加密策略
.passwordEncoder(myPasswordEncoder());
}
}
```
```markdown
编写产品处理器
```

```java
@RestController
@RequestMapping("/product")
public class ProductController {
@GetMapping
public String findAll(){
return "产品测试成功！";
}
}
```
```markdown
启动产品服务做测试
```

```markdown
携带token
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122193614740.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
在产品处理器上添加访问需要ADMIN角色
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122193649437.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
在产品处理器上添加访问需要ADMIN角色
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122193714579.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
重启测试权限不足
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122193742194.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
在数据库中手动给用户添加ADMIN角色
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122193814789.png#pic_center)

```markdown
重新认证获取新token再测试OK了！
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122193856261.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
## 10 OAuth2.0介绍
* 概念说明
```markdown
先说OAuth，OAuth是Open Authorization的简写。
OAuth协议为用户资源的授权提供了一个安全的、开放而又简易的标
准。与以往的授权方式不同之处是
OAuth的授权不会使第三方触及到用户的帐号信息（如用户名与密码），
即第三方无需使用用户的用户名与密码就可以申请获得该用户资源
的授权，因此OAuth是安全的。
OAuth2.0是OAuth协议的延续版本，但不向前兼容(即完全废
止了OAuth1.0)。
```

```markdown
使用场景
假设，A网站是一个打印照片的网站，B网站是一个存储照片的网站，
二者原本毫无关联。
如果一个用户想使用A网站打印自己存储在B网站的照片，那么A
网站就需要使用B网站的照片资源才行。
按照传统的思考模式，我们需要A网站具有登录B网站的用户名和密
码才行，但是，现在有了OAuth2，只需要A网站获取到使用B网
站照片资源的一个通行令牌即可！这个令牌无需具备操作B网站
所有资源的权限，也无需永久有
效，只要满足A网站打印照片需求即可。这么听来，是不是有
点像单点登录？NONONO！千万不要混淆概念！单点登录是用
户一次登录，自己可以操作其他关联的服务资源。OAuth2则是
用户给一个系统授权，可以直接操作其他系统资源的一种方式。
但SpringSecurity的OAuth2也是可以实现单点登录的！
总结一句：SpringSecurity的OAuth2可以做服务之间资源共
享，也可以实现单点登录！
```
### 10.1 OAuth2.0中四种授权方式
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122204005227.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
#### 10.1.1 授权码模式（authorization code）
```markdown
流程
说明：【A服务客户端】需要用到【B服务资源服务】中的资源
第一步：【A服务客户端】将用户自动导航到【B服务认证服务】，
这一步用户需要提供一个回调地址，以备【B服务认证服务】返回
授权码使用。
第二步：用户点击授权按钮表示让【A服务客户端】使用【B服务资
源服务】，这一步需要用户登录B服务，也就是说用户要事先具有B
服务的使用权限。
第三步：【B服务认证服务】生成授权码，授权码将通过第一步提
供的回调地址，返回给【A服务客户端】。注意这个授权码并非通
行【B服务资源服务】的通行凭证。
第四步：【A服务认证服务】携带上一步得到的授权码向【B服务
认证服务】发送请求，获取通行凭证token。
第五步：【B服务认证服务】给【A服务认证服务】返回令牌token
和更新令牌refresh token。
使用场景
授权码模式是OAuth2中最安全最完善的一种模式，应用场景最
广泛，可以实现服务之间的调用，常见的微信，QQ等第三方登
录也可采用这种方式实现。
```
#### 10.1.2 简化模式（implicit）
```markdown
流程
说明：简化模式中没有【A服务认证服务】这一部分，全部有
【A服务客户端】与B服务交互，整个过程不再有授权码，token直接暴
露在浏览器。
第一步：【A服务客户端】将用户自动导航到【B服务认证服务】，这一
步用户需要提供一个回调地址，以备
【B服务认证服务】返回token使用，还会携带一个【A服务客户端】
的状态标识state。
第二步：用户点击授权按钮表示让【A服务客户端】使用【B服务资源服务】，
这一步需要用户登录B服务，也就是说用户要事先具有B服务的使用权限。
第三步：【B服务认证服务】生成通行令牌token，token将通过第一步提
的回调地址，返回给【A服务客户端】。
使用场景
适用于A服务没有服务器的情况。
比如：纯手机小程序，JavaScript语言实现的网页插件等。
```
#### 10.1.3 密码模式（resource owner password credentials）

```markdown
流程
第一步：直接告诉【A服务客户端】自己的【B服务认证服务】的用户
名和密码
第二步：【A服务客户端】携带【B服务认证服务】的用户名和密
码向【B服务认证服务】发起请求获取
token。
第三步：【B服务认证服务】给【A服务客户端】颁发token。
使用场景
此种模式虽然简单，但是用户将B服务的用户名和密码暴露给了A服务，
需要两个服务信任度非常高才能使用。
```
#### 10.1.4 客户端模式（client credentials）

```markdown
流程
说明：这种模式其实已经不太属于OAuth2的范畴了。A服务
完全脱离用户，以自己的身份去向B服务索取
token。换言之，用户无需具备B服务的使用权也可以。完全
是A服务与B服务内部的交互，与用户无关了。
第一步：A服务向B服务索取token。
第二步：B服务返回token给A服务。
使用场景
A服务本身需要B服务资源，与用户无关。
```
### 10.2 OAuth2.0中表结构说明

```markdown
说明
如果只是写个测试案例，完全可以不用连接数据库，直接将用户等信
息写在项目中就行。
但是，我们应该把眼光放在企业开发中。试想，我们自己做的
一个软件，想使用微信第三方登录。难道你还指望微
信去修改他们的代码，让我们去访问？想都别想！
那么微信会怎么做呢？微信会提供好一个接入的入口，让我们
自己去申请访问权限。这些数据自然而然需要保存在
数据库中！
所以，我们将直-接讲解数据库版实现方式！
```

```markdown
建表语句
官方SQL地址：
https://github.com/spring-projects/spring-security-oauth/blob/master/spring-securityoauth2/src/test/resources/schema.sql
```
```markdown
表字段说明
oauth_client_details【核心表】
```

```markdown
client_id
主键,必须唯一,不能为空. 用于唯一标识每一个客户端(client); 在
注册时必须填写(也可由服务端自动生成). 对于不同的grant_type,该
字段都是必须的. 在实际应用中的另一个名称叫
appKey,与client_id是同一个概念

resource_ids
客户端所能访问的资源id集合,多个资源时用逗号(,)分隔,
如: “unity-resource,mobileresource”. 该字段的值必须来
源于与security.xml中标签‹oauth2:resource-server的属性
resource-id值一致. 在security.xml配置有几个‹oauth2:resource-server标签, 
则该字段可以使用几个该值. 在实际应用中, 我们一般将资源进行分类,
并分别配置对应的‹oauth2:resource-server,
如订单资源配置一个‹oauth2:resource-server, 用户资源又配置
一个‹oauth2:resource-server. 当注册客户端时,根据实际需要可
选择资源id,也可根据不同的注册流程,赋予对应的资源id.


client_secret
用于指定客户端(client)的访问密匙; 在注册时必须填
写(也可由服务端自动生成). 对于不同的
grant_type,该字段都是必须的. 在实际应用中的另一个名称
叫appSecret,与client_secret是同一个概念.

scope
指定客户端申请的权限范围,可选值包括read,write,trust;若有多
个权限范围用逗号(,)分隔,如:
“read,write”. scope的值与security.xml中配置的
‹intercept-url的access属性有关系.
如‹intercept-url的配置为‹intercept-url pattern="/m/**"
access=“ROLE_MOBILE,SCOPE_READ”/>则说明访问该URL时的客户端必须有read权限范
围. write的配置值为SCOPE_WRITE, trust的配置值
为SCOPE_TRUST. 在实际应该中, 该值一般由服务端指定, 常用的值
为read,write.

authorized_grant_types
指定客户端支持的grant_type,可选值包括
authorization_code,password,refresh_token,implicit,client_credentials, 若支持多个
grant_type用逗号(,)分隔,如: “authorization_code,password”. 在实际应用中,当注册时,该字
段是一般由服务器端指定的,而不是由申请者去选择的,最常用的grant_type组合有:
“authorization_code,refresh_token”(针对通过浏览器访问的客户端);
“password,refresh_token”(针对移动设备的客户端). implicit与client_credentials在实际中很少使用

web_server_redirect_uri
客户端的重定向URI,可为空, 当grant_type为authorization_code或implicit时, 在Oauth的流
程中会使用并检查与注册时填写的redirect_uri是否一致. 下面分别说明:当
grant_type=authorization_code时, 第一步 从 spring-oauth-server获取 'code’时客户端发
起请求时必须有redirect_uri参数, 该参数的值必须与 web_server_redirect_uri的值一致. 第
二步 用 ‘code’ 换取 ‘access_token’ 时客户也必须传递相同的redirect_uri. 在实际应用中,
web_server_redirect_uri在注册时是必须填写的, 一般用来处理服务器返回的code, 验证
state是否合法与通过code去换取access_token值.在spring-oauth-client项目中, 可具体参考
AuthorizationCodeController.java中的authorizationCodeCallback方法.当
grant_type=implicit时通过redirect_uri的hash值来传递access_token值.
如:http://localhost:7777/spring-oauth-client/implicit#access_token=dc891f4a-ac88-
4ba6-8224-a2497e013865&token_type=bearer&expires_in=43199然后客户端通过JS等从
hash值中取到access_token值.

authorities
指定客户端所拥有的Spring Security的权限值,可选, 若有多个权限值,用逗号(,)分隔, 如:"ROLE_"

access_token_validity
设定客户端的access_token的有效时间值(单位:秒),可选, 若不设定值则使用默认的有效时间
值(60 * 60 * 12, 12小时). 在服务端获取的access_token JSON数据中的expires_in字段的值
即为当前access_token的有效时间值. 在项目中, 可具体参考DefaultTokenServices.java中属
性accessTokenValiditySeconds. 在实际应用中, 该值一般是由服务端处理的, 不需要客户端
自定义.refresh_token_validity 设定客户端的refresh_token的有效时间值(单位:秒),可选,
若不设定值则使用默认的有效时间值(60 * 60 * 24 * 30, 30天). 若客户端的grant_type不包
括refresh_token,则不用关心该字段 在项目中, 可具体参考DefaultTokenServices.java中属
性refreshTokenValiditySeconds. 在实际应用中, 该值一般是由服务端处理的, 不需要客户端自定义.


additional_information
这是一个预留的字段,在Oauth的流程中没有实际的使用,可选,但若设置值,必须是JSON格式的
数据,如:{“country”:“CN”,“country_code”:“086”}按照spring-security-oauth项目中对该字段
的描述 Additional information for this client, not need by the vanilla OAuth protocol
but might be useful, for example,for storing descriptive information. (详见
ClientDetails.java的getAdditionalInformation()方法的注释)在实际应用中, 可以用该字段来
存储关于客户端的一些其他信息,如客户端的国家,地区,注册时的IP地址等等.create_time
数据的创建时间,精确到秒,由数据库在插入数据时取当前系统时间自动生成(扩展字段)

archived
用于标识客户端是否已存档(即实现逻辑删除),默认值为’0’(即未存档). 对
该字段的具体使用请参考CustomJdbcClientDetailsService.java,在该
类中,扩展了在查询client_details的SQL加上archived = 0条件 (扩展字段)

trusted
设置客户端是否为受信任的,默认为’0’(即不受信任的,1为受信任的). 
该字段只适用于
grant_type="authorization_code"的情况,当用户登录成功后,若该值
为0,则会跳转到让用户
Approve的页面让用户同意授权, 若该字段为1,则在登录后不需要再
让用户Approve同意授权
(因为是受信任的). 对该字段的具体使用请参考
OauthUserApprovalHandler.java. (扩展字段)

autoapprove
设置用户是否自动Approval操作, 默认值为 ‘false’, 
可选值包括 ‘true’,‘false’, ‘read’,‘write’. 该
字段只适用于grant_type="authorization_code"的情况,
当用户登录成功后,若该值为’true’或支持的scope值,则
会跳过用户Approve的页面, 直接授权. 该字段与 trusted 
有类似的功能, 是spring-security-oauth2 的 2.0 版本后添加
的新属性. 在项目中,主要操作oauth_client_details表的类是
JdbcClientDetailsService.java, 更多的细节请参考该类. 
也可以根据实际的需要,去扩展或修改该类的实现.
```
```markdown
oauth_client_token
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112221521534.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
该表用于在客户端系统中存储从服务端获取的token数据, 
在spring-oauth-server项目中未使用到. 对
oauth_client_token表的主要操作在JdbcClientTokenServices.java类中, 
更多的细节请参考该类.
```
```markdown
oauth_access_token
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122215308542.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
oauth_refresh_token
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122220426105.png#pic_center)

```markdown
在项目中,主要操作oauth_refresh_token表的对象是JdbcTokenStore.java. 
(与操作oauth_access_token表的对象一样);更多的细节请参考该类. 如果
客户端的grant_type不支持refresh_token,则不会使用该表
```
```markdown
oauth_code
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122220539430.png#pic_center)

```markdown
在项目中,主要操作oauth_code表的对象是
JdbcAuthorizationCodeServices.java. 更多的细节请参考该类。 只有当
grant_type为"authorization_code"时,该表中才会有数据产生; 其
他的grant_type没有使用该表。
```
```markdown
OAuth2.0实战案例
```

```xml
创建父工程并导入jar包
<groupId>com.yxj</groupId>
<artifactId>springboot_security_oauth</artifactId>
<version>1.0-SNAPSHOT</version>
<parent>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-parent</artifactId>
<version>2.1.3.RELEASE</version>
<relativePath/>
</parent>
<properties>
<spring-cloud.version>Greenwich.RELEASE</spring-cloud.version>
</properties>
<dependencyManagement>
<dependencies>
<dependency>
<groupId>org.springframework.cloud</groupId>
<artifactId>spring-cloud-dependencies</artifactId>
<version>${spring-cloud.version}</version>
<type>pom</type>
<scope>import</scope>
</dependency>
</dependencies>
</dependencyManagement>
<repositories>
<repository>
<id>spring-snapshots</id>
<name>Spring Snapshots</name>
<url>https://repo.spring.io/snapshot</url>
<snapshots>
<enabled>true</enabled>
</snapshots>
</repository>
<repository>
<id>spring-milestones</id>
<name>Spring Milestones</name>
<url>https://repo.spring.io/milestone</url>
<snapshots>
<enabled>false</enabled>
</snapshots>
</repository>
</repositories>
```
```markdown
创建资源模块
```

```xml
<parent>
	<artifactId>springboot_security_oauth</artifactId>
	<groupId>com.yxj</groupId>
	<version>1.0-SNAPSHOT</version>
</parent>
<modelVersion>4.0.0</modelVersion>
<artifactId>oauth_source</artifactId>
<dependencies>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-web</artifactId>
	</dependency>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-security</artifactId>
	</dependency>
	<dependency>
		<groupId>org.springframework.cloud</groupId>
		<artifactId>spring-cloud-starter-oauth2</artifactId>
	</dependency>
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>5.1.47</version>
	</dependency>
	<dependency>
		<groupId>org.mybatis.spring.boot</groupId>
		<artifactId>mybatis-spring-boot-starter</artifactId>
		<version>2.1.0</version>
	</dependency>
</dependencies>
```
```markdown
提供配置文件
```
```yml
server:
	port: 9002
	spring:
	datasource:
	driver-class-name: com.mysql.jdbc.Driver
	url: jdbc:mysql:///security_authority
	username: root
	password: root
main:
	allow-bean-definition-overriding: true
mybatis:
	type-aliases-package: com.yxj.domain
	configuration:
		map-underscore-to-camel-case: true
logging:
	level:
		com.yxj: debug
```
```markdown
提供启动类
```

```java
@SpringBootApplication
@MapperScan("com.yxj.mapper")
public class OAuthSourceApplication {
public static void main(String[] args) {
SpringApplication.run(OAuthSourceApplication.class, args);
}
}
```
```markdown
提供处理器
```

```java
@RestController
@RequestMapping("/product")
public class ProductController {
	@GetMapping
	public String findAll(){
		return "查询产品列表成功！";
	}
}
```
```markdown
启动项目测试
```
```markdown
由于此刻，项目中添加的有SpringBoot的Security包，默认不通过
认证是无法访问处理器的，这个结果咱们在第三天都已经知道了！
那么如何解决呢？第三天我们是采用单点登录的方式解决了这个问题，
那么今天我们把这个资源交给OAuth2来管理，使用通行的token来
访问资源!
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122230155470.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
将访问资源作为OAuth2的资源来管理
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112223111885.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
编写资源管理配置类
```

```java
@Configuration
@EnableResourceServer
@EnableGlobalMethodSecurity(securedEnabled = true)
public class OauthSourceConfig extends ResourceServerConfigurerAdapter {
@Autowired
private DataSource dataSource;
/**
* TokenStore是OAuth2保存token的接口
* 其下有RedisTokenStore保存到redis中，
* JdbcTokenStore保存到数据库中，
* InMemoryTokenStore保存到内存中等实现类，
* 这里我们选择保存在数据库中
* @return
*/
@Bean
public TokenStore tokenStore() {
return new JdbcTokenStore(dataSource);
}
@Override
public void configure(ResourceServerSecurityConfigurer resources)throws Exception{
TokenStore tokenStore = new JdbcTokenStore(dataSource);
resources.resourceId("product_api")//指定当前资源的id，非常重要！必须写！
.tokenStore(tokenStore);//指定保存token的方式
}
@Override
public void configure(HttpSecurity http) throws Exception{
http.authorizeRequests()
//指定不同请求方式访问资源所需要的权限，一般查询是read，其余是write。
.antMatchers(HttpMethod.GET, "/**").access("#oauth2.hasScope('read')")
.antMatchers(HttpMethod.POST, "/**").access("#oauth2.hasScope('write')")
.antMatchers(HttpMethod.PATCH, "/**").access("#oauth2.hasScope('write')")
.antMatchers(HttpMethod.PUT, "/**").access("#oauth2.hasScope('write')")
.antMatchers(HttpMethod.DELETE, "/**").access("#oauth2.hasScope('write')")
.and()
.headers().addHeaderWriter((request, response) -> {
response.addHeader("Access-Control-Allow-Origin", "*");//允许跨域
if (request.getMethod().equals("OPTIONS")) {//如果是跨域的预检请求，则原封不动向下传达请
求头信息
response.setHeader("Access-Control-Allow-Methods", request.getHeader("AccessControl-Request-Method"));
response.setHeader("Access-Control-Allow-Headers", request.getHeader("AccessControl-Request-Headers"));
}
});
}
}
```
```markdown
创建授权模块
```


```markdown
创建工程并导入jar包
```
```xml
<parent>
<artifactId>springboot_security_oauth</artifactId>
<groupId>com.yxj</groupId>
<version>1.0-SNAPSHOT</version>
</parent>
<modelVersion>4.0.0</modelVersion>
<artifactId>oauth_server</artifactId>
<dependencies>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
<groupId>org.springframework.cloud</groupId>
<artifactId>spring-cloud-starter-oauth2</artifactId>
</dependency>
<dependency>
<groupId>mysql</groupId>
<artifactId>mysql-connector-java</artifactId>
<version>5.1.47</version>
</dependency>
<dependency>
<groupId>org.mybatis.spring.boot</groupId>
<artifactId>mybatis-spring-boot-starter</artifactId>
<version>2.1.0</version>
</dependency>
</dependencies
```
```markdown
提供配置文件
```

```yml
server:
port: 9001
spring:
datasource:
driver-class-name: com.mysql.jdbc.Driver
url: jdbc:mysql:///security_authority
username: root
password: root
main:
allow-bean-definition-overriding: true # 这个表示允许我们覆盖OAuth2放在容器中的bean对象，一定要
配置
mybatis:
type-aliases-package: com.yxj.domain
configuration:
map-underscore-to-camel-case: true
logging:
level:
com.yxj: debug
```
```markdown
提供启动类
```

```java
@SpringBootApplication
@MapperScan("com.yxj.mapper")
public class OauthServerApplication {
public static void main(String[] args) {
SpringApplication.run(OauthServerApplication.class, args);
}
}
```
```markdown
将之前所有认证的代码复制进来 
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122233116268.png#pic_center)
```markdown
提供SpringSecurity配置类
```

```java
@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(securedEnabled = true)
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
@Autowired
private UserDetailsService myCustomUserService;
@Bean
public BCryptPasswordEncoder myPasswordEncoder(){
return new BCryptPasswordEncoder();
}
@Override
protected void configure(HttpSecurity http) throws Exception {
http.authorizeRequests()
//所有资源必须授权后访问
.anyRequest().authenticated()
.and()
.formLogin()
.loginProcessingUrl("/login")
.permitAll()//指定认证页面可以匿名访问
//关闭跨站请求防护
.and().csrf().disable();
}
@Override
public void configure(AuthenticationManagerBuilder auth) throws Exception {
//UserDetailsService类
auth.userDetailsService(myCustomUserService)
//加密策略
.passwordEncoder(myPasswordEncoder());
}
//AuthenticationManager对象在OAuth2认证服务中要使用，提取放入IOC容器中
@Override
@Bean
public AuthenticationManager authenticationManagerBean() throws Exception {
return super.authenticationManagerBean();
}
}
```
```markdown
提供OAuth2授权配置类
```

```java
@Configuration
@EnableAuthorizationServer
public class OauthServerConfig extends AuthorizationServerConfigurerAdapter {
@Autowired
private DataSource dataSource;
@Autowired
private AuthenticationManager authenticationManager;
@Autowired
private UserDetailsService userDetailsService;
//从数据库中查询出客户端信息
@Bean
public JdbcClientDetailsService clientDetailsService() {
return new JdbcClientDetailsService(dataSource);
}
//token保存策略
@Bean
public TokenStore tokenStore() {
return new JdbcTokenStore(dataSource);
}
//授权信息保存策略
@Bean
public ApprovalStore approvalStore() {
return new JdbcApprovalStore(dataSource);
}
//授权码模式专用对象
@Bean
public AuthorizationCodeServices authorizationCodeServices() {
return new JdbcAuthorizationCodeServices(dataSource);
}
//指定客户端登录信息来源
@Override
public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
clients.withClientDetails(clientDetailsService());
}
@Override
public void configure(AuthorizationServerSecurityConfigurer oauthServer) throws Exception {
oauthServer.allowFormAuthenticationForClients();
oauthServer.checkTokenAccess("isAuthenticated()");
}
@Override
public void configure(AuthorizationServerEndpointsConfigurer endpoints) throws Exception {
endpoints
.approvalStore(approvalStore())
.authenticationManager(authenticationManager)
.authorizationCodeServices(authorizationCodeServices())
.tokenStore(tokenStore());
}
}

```
```markdown
测试
```

```markdown
在数据库中手动添加客户端信息
所有要使用当前项目资源的项目，都是我们的客户端。比如
我们之前举的例子，A服务打印照片，B服务存储照
片。A服务要使用B服务的资源，那么A服务就是B服务的客户端。
这里要区分用户的信息和客户端信息，用户信息是用户在B服务上
注册的用户信息，在sys_user表中。客户端信息
是A服务在B服务中注册的账号，在OAuth2的oauth_client_details表中。
测试数据sql语句如下：
```
```sql
insert into
`oauth_client_details`(`client_id`,`resource_ids`,`client_secret`,`scope`,`authorized_grant_type
s`,`web_server_redirect_uri`,`authorities`,`access_token_validity`,`refresh_token_validity`,`add
itional_information`,`autoapprove`) values
('heima_one','product_api','$2a$10$CYX9OMv0yO8wR8rE19N2fOaXDJondci5uR68k2eQJm50q8ESsDMlC','read,
write','client_credentials,implicit,authorization_code,refresh_token,password','http://www.baidu
.com',NULL,NULL,NULL,NULL,'false');
```

```markdown
这里注意resource_ids不要写错，回调地址web_server_redirect_uri先写成百度。
```
```markdown
授权码模式测试
```
```markdown
在地址栏访问地址
http://localhost:9001/oauth/authorize?response_type=code&client_id=heima_one
跳转到SpringSecurity默认认证页面，提示用户登录
个人账户【这里是sys_user表中的数据】
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112223420839.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


```markdown
登录成功后询问用户是否给予操作资源的权限，具体给什么权限。
Approve是授权，Deny是拒绝。
这里我们选择read和write都给予Approve。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122234248436.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


```markdown
点击Authorize后跳转到回调地址并获取授权码
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122234813106.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
使用授权码到服务器申请通行令牌token
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122234919996.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122234946480.png#pic_center)

```markdown
重启资源服务器，然后携带通行令牌再次去访问资源服务器，大功告成！
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122235030618.png#pic_center)
```markdown
简化模式测试
```
```markdown
在地址栏访问地址
http://localhost:9001/oauth/authorize?response_type=token&client_id=heima_one
由于上面用户已经登录过了，所以无需再次登录，其
实和上面是有登录步骤的，这时，浏览器直接返回了token
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122235217228.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
直接访问资源服务器
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122235246882.png#pic_center)
```markdown
密码模式测试
```

```markdown
申请token
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122235403196.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
访问资源服务器
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122235428990.png#pic_center)
```markdown
客户端模式测试
```
```markdown
申请token
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201123001346583.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


```markdown
访问资源服务
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201123001455436.png#pic_center)
# JWT


## 1 什么是JWT
![image-20200726102546868.png](https://img-blog.csdnimg.cn/20201022130311773.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
JSON Web Token (JWT) is an open standard ([RFC 7519]
(https://tools.ietf.org/html/rfc7519)) that defines a compact 
and self-contained way for securely transmitting 
information between parties as a JSON object. This 
information can be verified and trusted because it is 
digitally signed. JWTs can be signed using a secret 
(with the HMAC algorithm) or a public/private 
key pair using  RSA or ECDSA
---[摘自官网]


# 1.翻译
  官网地址: https://jwt.io/introduction/
  翻译: jsonwebtoken（JWT）是一个开放标准（rfc7519），
  它定义了一种紧凑的、自包含的方式，用于在各方之间以JSON
  对象安全地传输信息。此信息可以验证和信任，因为它是数字签
  名的。jwt可以使用秘密（使用HMAC算法）或使用RSA或ECDSA的
  公钥/私钥对进行签名

# 2.通俗解释
JWT简称JSON Web Token,也就是通过JSON形式作为Web应
用中的令牌,用于在各方之间安全地将信息作为JSON对象传输。
在数据传输过程中还可以完成数据加密、签名等相关处理。
```

## 2 JWT能做什么

```markdown
# 1.授权
这是使用JWT的最常见方案。一旦用户登录，每个后续请求将
包括JWT，从而允允许的路由，服务和资源。单点登录是当今
广泛使用JWT的一项功能，因为它的开销很小并且可以在不同
的域中轻松使用。

# 2.信息交换
JSON Web Token是在各方之间安全地传输信息的好方法。因为
可以对JWT进行签名（例如，使用公钥/私钥对），所以您可
以确保发件人是他们所说的人。此外，由于签名是使用标头和有
效负载计算的，因此您还可以验证内容是否遭到篡改。


注意：jwt跟session不一样，jwt存储在客户端，session
存储在服务器端，服务器断电后session就没了，而jwt因
为存储在客户端，所以就不会被影响，只要jwt不过期，
就可以继续使用。
```

## 3 为什么是JWT

### 3.1 基于传统的Session认证

```markdown
# 1.认证方式
我们知道，http协议本身是一种无状态的协议，而这
就意味着如果用户向我们的应用提供了用户名和密码来进行用
户认证，那么下一次请求时，用户还要再一次进行用户认证
才行，因为根据http协议，我们并不能知道是哪个用户发出
的请求，所以为了让我们的应用能识别是哪个用户发出的请求，我
们只能在服务器存储一份用户登录的信息，这份登录信息会在响
应时传递给浏览器，告诉其保存为cookie,以便下次请求时发送
给我们的应用，这样我们的应用就能识别请求来自哪个用
户了,这就是传统的基于session认证。
t
# 2.认证流程
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022130410229.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
# 3.暴露问题
1.每个用户经过我们的应用认证之后，我们的应用都要在服务端做
一次记录，以方便用户下次请求的鉴别，通常而言session都是保
存在内存中，而随着认证用户的增多，服务端的开销会明显增大

2.用户认证之后，服务端做认证记录，如果认证的记录被保存在
内存中的话，这意味着用户下次请求还必须要请求在这台服务
器上,这样才能拿到授权的资源，这样在分布式的应用上，相应
的限制了负载均衡器的能力。这也意味着限制了应用的扩展能力。

3.因为是基于cookie来进行用户识别的, cookie如果被截获，用户
就会很容易受到跨站请求伪造的攻击。

4.在前后端分离系统中就更加痛苦:如下图所示
也就是说前后端分离在应用解耦后增加了部署的复杂性。通常用户
一次请求就要转发多次。如果用session 每次携带sessionid 到服务
器，服务器还要查询用户信息。同时如果用户很多。这些信息存储
在服务器内存中，给服务器增加负担。还有就是CSRF（跨站伪造
请求攻	击）攻击，session是基于cookie进行用户识别的, cookie如
果被截获，用户就会很容易受到跨站请求伪造的攻击。还有就是
sessionid就是一个特征值，表达的信息不够丰富。不容易扩展。
而且如果你后端应用是多节点部署。那么就需要实现session共
享机制。不方便集群应用。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022130428505.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


### 3.2 基于JWT认证
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022130449432.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
# 1.认证流程
首先，前端通过Web表单将自己的用户名和密码发送到后端的接口。
这一过程一般是一个HTTP POST请求。建议的方式是通过SSL加
密的传输（https协议），从而避免敏感信息被嗅探。

后端核对用户名和密码成功后，将用户的id等其他信息作为
JWT Payload（负载），将其与头部分别进行Base64编码拼接后
签名，形成一个JWT(Token)。形成的JWT就是一个形同lll.zzz.xxx
的字符串。 token head.payload.singurater

后端将JWT字符串作为登录成功的返回结果返回给前端。前端可
以将返回的结果保存在localStorage或sessionStorage上，退出登录
时前端删除保存的JWT即可。

前端在每次请求时将JWT放入HTTP Header中的Authorization位。
(解决XSS和XSRF问题) HEADER

后端检查是否存在，如存在验证JWT的有效性。例如，检查签名
是否正确；检查Token是否过期；检查Token的接收方是否是自己（可选）。

验证通过后后端使用JWT中包含的用户信息进行其他逻辑操作，
返回相应结果。

# 2.jwt优势

简洁(Compact): 可以通过URL，POST参数或者在HTTP header发送，
因为数据量小，传输速度也很快

自包含(Self-contained)：负载中包含了所有用户所需要的信息，
避免了多次查询数据库

因为Token是以JSON加密的形式保存在客户端的，所以JWT是跨语
言的，原则上任何web形式都支持。

不需要在服务端保存会话信息，特别适用于分布式微服务。
```

## 4 JWT的结构是什么

```markdown
token   string  ====>  header.payload.singnature  token   

# 1.令牌组成
- 1.标头(Header)
- 2.有效载荷(Payload)
- 3.签名(Signature)
- 因此，JWT通常如下所示:xxxxx.yyyyy.zzzzz   Header.Payload.Signature
```

```markdown
# 2.Header
- 标头通常由两部分组成：令牌的类型（即JWT）和所使用的签名算法，例如HMAC SHA256或RSA。它会使用 Base64 编码组成 JWT 结构的第一部分。

- 注意:Base64是一种编码，也就是说，它是可以被翻译回原来的样子来的。它并不是一种加密过程。
```

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

```markdown
# 3.Payload
- 令牌的第二部分是有效负载，其中包含声明。声明是有关实体
- （通常是用户）和其他数据的声明。同样的，它会使用 
Base64 编码组成 JWT 结构的第二部分
```

```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

```markdown
# 4.Signature
- 前面两部分都是使用 Base64 进行编码的，即前端可以解开知
道里面的信息。Signature 需要使用编码后的 header 和 payload
以及我们提供的一个密钥，然后使用 header 中指定的签名算法
（HS256）进行签名。签名的作用是保证 JWT 没有被篡改过
- 如:
	HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload),secret);

# 签名目的
- 最后一步签名的过程，实际上是对头部以及负载内容进行签名，
防止内容被窜改。如果有人对头部以及负载的内容解码之后进行修
改，再进行编码，最后加上之前的签名组合形成新的JWT的话，
那么服务器端会判断出新的头部和负载形成的签名和JWT附带上
的签名是不一样的。如果要对新的头部和负载进行签名，在不知
道服务器加密时用的密钥的话，得出来的签名也是不一样的。

# 信息安全问题
- 在这里大家一定会问一个问题：Base64是一种编码，
是可逆的，那么我的信息不就被暴露了吗？

- 是的。所以，在JWT中，不应该在负载里面加入任何敏感的
数据。在上面的例子中，我们传输的是用户的User ID。这个值
实际上不是什么敏	感内容，一般情况下被知道也是安全的。但
是像密码这样的内容就不能被放在JWT中了。如果将用户的密
码放在了JWT中，那么怀有恶意的第	三方通过Base64解码就能
很快地知道你的密码了。因此JWT适合用于向Web应用传递一些
非敏感信息。JWT还经常用于设计用户认证和授权系	统，甚至
实现Web应用的单点登录。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022130517586.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
# 5.放在一起
- 输出是三个由点分隔的Base64-URL字符串，可以在HTML和
HTTP环境中轻松传递这些字符串，与基于XML的标准（例如SAML）
相比，它更紧凑。
- 简洁(Compact)
	可以通过URL, POST 参数或者在 HTTP header 发送，因为
	数据量小，传输速度快
- 自包含(Self-contained)
	负载中包含了所有用户所需要的信息，避免了多次查询数据库
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022130536633.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


## 5 使用JWT

```markdown
# 1.引入依赖
```

```xml
<!--引入jwt-->
<dependency>
  <groupId>com.auth0</groupId>
  <artifactId>java-jwt</artifactId>
  <version>3.4.0</version>
</dependency>
```

```markdown
# 2.生成token
```

```java
Calendar instance = Calendar.getInstance();
instance.add(Calendar.SECOND, 90);
//生成令牌
String token = JWT.create()
  .withClaim("username", "张三")//设置自定义用户名
  .withExpiresAt(instance.getTime())//设置过期时间
  .sign(Algorithm.HMAC256("token!Q2W#E$RW"));//设置签名 保密 复杂
//输出令牌
System.out.println(token);
```

```markdown
生成结果
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhdWQiOlsicGhvbmUiLCIxNDMyMzIzNDEzNCJdLCJleHAiOjE1OTU3Mzk0NDIsInVzZXJuYW1lIjoi5byg5LiJIn0.aHmE3RNqvAjFr_dvyn_sD2VJ46P7EGiS5OBMO_TI5jg
```

```markdown
# 3.根据令牌和签名解析数据
```

```java
JWTVerifier jwtVerifier = JWT.require(Algorithm.HMAC256("token!Q2W#E$RW")).build();
DecodedJWT decodedJWT = jwtVerifier.verify(token);
System.out.println("用户名: " + decodedJWT.getClaim("username").asString());  、// 存的是时候是什么类型，取得时候就是什么类型，否则取不到值。
System.out.println("过期时间: "+decodedJWT.getExpiresAt());
```

````markdown
# 4.常见异常信息
SignatureVerificationException:		签名不一致异常
TokenExpiredException:    			令牌过期异常
AlgorithmMismatchException:				算法不匹配异常
InvalidClaimException:			失效的payload异常
````
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022130611202.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


## 6 封装工具类

```java
public class JWTUtils {
    private static String TOKEN = "token!Q@W3e4r";
    /**
     * 生成token
     * @param map  //传入payload
     * @return 返回token
     */
    public static String getToken(Map<String,String> map){
        JWTCreator.Builder builder = JWT.create();
        map.forEach((k,v)->{
            builder.withClaim(k,v);
        });
        Calendar instance = Calendar.getInstance();
        instance.add(Calendar.SECOND,7);
        builder.withExpiresAt(instance.getTime());
        return builder.sign(Algorithm.HMAC256(TOKEN));
    }
    /**
     * 验证token
     * @param token
     * @return
     */
    public static void verify(String token){
        JWT.require(Algorithm.HMAC256(TOKEN)).build().verify(token);  // 如果验证通过，则不会把报错，否则会报错
    }
    /**
     * 获取token中payload
     * @param token
     * @return
     */
    public static DecodedJWT getToken(String token){
        return JWT.require(Algorithm.HMAC256(TOKEN)).build().verify(token);
    }
}
```

## 7 整合springboot

```markdown
# 0.搭建springboot+mybatis+jwt环境
- 引入依赖
- 编写配置
```

```xml
<!--引入jwt-->
<dependency>
  <groupId>com.auth0</groupId>
  <artifactId>java-jwt</artifactId>
  <version>3.4.0</version>
</dependency>

<!--引入mybatis-->
<dependency>
  <groupId>org.mybatis.spring.boot</groupId>
  <artifactId>mybatis-spring-boot-starter</artifactId>
  <version>2.1.3</version>
</dependency>

<!--引入lombok-->
<dependency>
  <groupId>org.projectlombok</groupId>
  <artifactId>lombok</artifactId>
  <version>1.18.12</version>
</dependency>

<!--引入druid-->
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>druid</artifactId>
  <version>1.1.19</version>
</dependency>

<!--引入mysql-->
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>5.1.38</version>
</dependency>
```

```properties
server.port=8989
spring.application.name=jwt

spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/jwt?characterEncoding=UTF-8
spring.datasource.username=root
spring.datasource.password=root

mybatis.type-aliases-package=com.baizhi.entity
mybatis.mapper-locations=classpath:com/baizhi/mapper/*.xml

logging.level.com.baizhi.dao=debug
```

```markdown
1. 开发数据库
- 这里采用最简单的表结构验证JWT使用
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022130659269.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```sql
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '主键',
  `name` varchar(80) DEFAULT NULL COMMENT '用户名',
  `password` varchar(40) DEFAULT NULL COMMENT '用户密码',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;
```


```markdown
2. 开发entity
```
```java
@Data
@Accessors(chain=true)
public class User {
    private String id;
    private String name;
    private String password;
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022130719638.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
3.开发DAO接口和mapper.xml
```

```java
@Mapper
public interface UserDAO {
    User login(User user);
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022130741498.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```xml
<mapper namespace="com.baizhi.dao.UserDAO">
    <!--这里就写的简单点了毕竟不是重点-->
    <select id="login" parameterType="User" resultType="User">
        select * from user where name=#{name} and password = #{password}
    </select>
</mapper>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022130800217.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


```markdown
 4.开发Service 接口以及实现类
```

```java
public interface UserService {
    User login(User user);//登录接口
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022130820924.png#pic_center)

````java
@Service
@Transactional
public class UserServiceImpl implements UserService {
    @Autowired
    private UserDAO userDAO;
    @Override
    @Transactional(propagation = Propagation.SUPPORTS)
    public User login(User user) {
        User userDB = userDAO.login(user);
        if(userDB!=null){
            return userDB;
        }
        throw  new RuntimeException("登录失败~~");
    }
}
````
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022130840825.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
5.开发controller
```

```java
@RestController
@Slf4j
public class UserController {
    @Autowired
    private UserService userService;
    @GetMapping("/user/login")
    public Map<String,Object> login(User user) {
        Map<String,Object> result = new HashMap<>();
        log.info("用户名: [{}]", user.getName());
        log.info("密码: [{}]", user.getPassword());
        try {
            User userDB = userService.login(user);
            Map<String, String> map = new HashMap<>();//用来存放payload
            map.put("id",userDB.getId());
            map.put("username", userDB.getName());
            String token = JWTUtils.getToken(map);
            result.put("state",true);
            result.put("msg","登录成功!!!");
            result.put("token",token); //成功返回token信息
        } catch (Exception e) {
            e.printStackTrace();
            result.put("state","false");
            result.put("msg",e.getMessage());
        }
        return result;
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022130904788.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


```markdown
6.数据库添加测试数据启动项目
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022130926195.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022130941387.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
7.通过postman模拟登录失败
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022131001146.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


```markdown
8.通过postman模拟登录成功
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022131022429.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
 9.编写测试接口
```

```java
@PostMapping("/test/test")
public Map<String, Object> test(String token) {
  Map<String, Object> map = new HashMap<>();
  try {
    JWTUtils.verify(token);
    map.put("msg", "验证通过~~~");
    map.put("state", true);
  } catch (TokenExpiredException e) {
    map.put("state", false);
    map.put("msg", "Token已经过期!!!");
  } catch (SignatureVerificationException e){
    map.put("state", false);
    map.put("msg", "签名错误!!!");
  } catch (AlgorithmMismatchException e){
    map.put("state", false);
    map.put("msg", "加密算法不匹配!!!");
  } catch (Exception e) {
    e.printStackTrace();
    map.put("state", false);
    map.put("msg", "无效token~~");
  }
  return map;
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102213104016.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


```markdown
10.通过postman请求接口
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022131058107.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022131123608.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
11.问题?
- 使用上述方式每次都要传递token数据,每个方法都需要
验证token代码冗余,不够灵活? 如何优化
- 使用拦截器进行优化
```

```java

public class JWTInterceptor implements HandlerInterceptor{

	@Override
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
  String token = request.getHeader("token");
  Map<String,Object> map = new HashMap<>();
  try {
    JWTUtils.verify(token);
    return true;
  } catch (TokenExpiredException e) {
    map.put("state", false);
    map.put("msg", "Token已经过期!!!");
  } catch (SignatureVerificationException e){
    map.put("state", false);
    map.put("msg", "签名错误!!!");
  } catch (AlgorithmMismatchException e){
    map.put("state", false);
    map.put("msg", "加密算法不匹配!!!");
  } catch (Exception e) {
    e.printStackTrace();
    map.put("state", false);
    map.put("msg", "无效token~~");
  }
  String json = new ObjectMapper().writeValueAsString(map);
  response.setContentType("application/json;charset=UTF-8");
  response.getWriter().println(json);
  return false;
}

}
```

```java
@Configuration
public class InterceptorConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new JwtTokenInterceptor()).
          excludePathPatterns("/user/**")  // 放行
          .addPathPatterns("/**"); // 拦截除了"/user/**的所有请求路径
    }
}
```

```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复ssjwt
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

