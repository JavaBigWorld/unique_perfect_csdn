## ssm企业权限管理系统项目
![在这里插入图片描述](https://img-blog.csdnimg.cn/5aeae59c040c4042ad922be4abee0432.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAdW5pcXVlX3BlcmZlY3Q=,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

### 1.1 开发疑难杂症
```markdown
<jsp:forward page="/pages/main.jsp"></jsp:forward>里面不可以写内容，否则会报错
```
### 1.2 AdminLTE介绍
```markdown
AdminLTE是一款建立在bootstrap和jquery之上的开源的模板主题工具，它提供了
一系列响应的、可重复使用的组件，并内置了多个模板页面；同时自适应多种屏
幕分辨率，兼容PC和移动端。通过AdminLTE，我们可以快速的创建一个响应式
的Html5网站。AdminLTE框架在网页架构与设计上，有很大的辅助作用，尤其是
前端架构设计师，用好AdminLTE不但美观，而且可以免去写很大CSS与JS的工
作量。
```
### 1.3 GitHub获取AdminLTE
```markdown
https://github.com/almasaeed2010/AdminLTE
```
### 1.4 依赖环境
```markdown
AdminLTE依赖于两个框架Bootstrap3与JQuery1.11+
```
### 1.5 AdminLTE布局与皮肤
```markdown
布局
wrapper包住了body下的所有代码
main-header里是网站的logo和导航栏的代码
main-sidebar里是用户面板和侧边栏菜单的代码
content-wrapper里是页面的页面和内容区域的代码
main-footer里是页脚的代码
control-sidebar里是页面右侧侧边栏区域的代码

布局选项
fixed：固定
layout-boxed：盒子布局
layout-top-nav：顶部隐藏
sidebar-collapse：侧边栏隐藏
sidebar-mini：侧边栏隐藏时有小图标

皮肤
skin-blue：蓝色
skin-black：黑色
skin-purple：紫色
skin-yellow：黄色
skin-red：红色
skin-green：绿色
以上项我们可以查看starter.html页面中查看。
```
### 1.6 SSM 环境搭建与产品操作

#### 1.6.1 数据库与表结构
```markdown
数据库我们使用Mysql
```
```sql
create  database  yxjssm;
use yxjssm;
CREATE TABLE product(
	id varchar(36)  PRIMARY KEY,
	productNum VARCHAR(50) NOT NULL,
	productName VARCHAR(50),
	cityName VARCHAR(50),
	DepartureTime date,
	productPrice double,
	productDesc VARCHAR(500),
	productStatus INT,
	CONSTRAINT product UNIQUE (id, productNum)
);
insert into PRODUCT (id, productnum, productname, cityname, departuretime, productprice,productdesc, productstatus)
values (uuid(), 'itcast-002', '北京三日游', '北京', str_to_date('2020-6-11','%Y-%c-%d'), 1200, '不错的旅行', 1);
insert into PRODUCT (id, productnum, productname, cityname, departuretime, productprice,productdesc, productstatus)
values (uuid(), 'itcast-003', '上海五日游', '上海',str_to_date('2020-6-12','%Y-%c-%d'), 1800, '魔都我来了', 0);
insert into PRODUCT (id, productnum, productname, cityname, departuretime, productprice,productdesc, productstatus)
values (uuid(), 'itcast-001', '北京三日游', '北京', str_to_date('2020-6-13','%Y-%c-%d'), 1200, '不错的旅行', 1);
```
#### 1.6.2 maven工程搭建
##### 1.6.2.1 创建父模块
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200614124148954.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200614124159772.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 1.6.2.2 创建子模块
```markdown
itcast-ssm-web itcast-ssm-domain itcast-ssm-service itcast-ssm-dao itcast-ssm-utils 其中创建itcast-ssm-web时注意我们选择一个web工程
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200614124245412.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 1.6.2.3 在父工程添加pom.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.yxj.ssm</groupId>
    <artifactId>yxjssm</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <properties>
        <spring.version>5.0.2.RELEASE</spring.version>
        <slf4j.version>1.6.6</slf4j.version>
        <log4j.version>1.2.12</log4j.version>
        <mysql.version>5.1.6</mysql.version>
        <mybatis.version>3.4.5</mybatis.version>
        <spring.security.version>5.0.1.RELEASE</spring.security.version>
    </properties>

    <dependencies>        <!-- spring -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.6.8</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context-support</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>

        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.0</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>jstl</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>        <!-- log start -->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>${log4j.version}</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>${slf4j.version}</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>${slf4j.version}</version>
        </dependency>        <!-- log end -->

        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>${mybatis.version}</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>1.3.0</version>
        </dependency>
        <dependency>
            <groupId>c3p0</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.1.2</version>
            <type>jar</type>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper</artifactId>
            <version>5.1.2</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-web</artifactId>
            <version>${spring.security.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-config</artifactId>
            <version>${spring.security.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-core</artifactId>
            <version>${spring.security.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-taglibs</artifactId>
            <version>${spring.security.version}</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>${mysql.version}</version>
        </dependency>


        <dependency>
            <groupId>javax.annotation</groupId>
            <artifactId>jsr250-api</artifactId>
            <version>1.0</version>
        </dependency>
    </dependencies>
    <build>
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.2</version>
                    <configuration>
                        <source>1.8</source>
                        <target>1.8</target>
                        <encoding>UTF-8</encoding>
                        <showWarnings>true</showWarnings>
                    </configuration>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
    <modules>
        <module>ssm_dao</module>
        <module>ssm_service</module>
        <module>ssm_domain</module>
        <module>ssm_utils</module>
        <module>ssm_web</module>
    </modules>


</project>
```
#### 1.6.3 编写实体类
```markdown
ssm_service中编写com.yxj.ssm.domain.Product
```

```java
package com.yxj.ssm.domain;

import java.util.Date;

/**
 * @author shkstart
 * @date 2020/6/14 0014 - 下午 12:48
 */
public class Product {
    private String id; // 主键
    private String productNum; // 编号 唯一
    private String productName; // 名称
    private String cityName; // 出发城市
    private Date departureTime; // 出发时间
    private String departureTimeStr;
    private double productPrice; // 产品价格
    private String productDesc; // 产品描述
    private Integer productStatus; // 状态 0 关闭 1 开启
    private String productStatusStr;

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getProductNum() {
        return productNum;
    }

    public void setProductNum(String productNum) {
        this.productNum = productNum;
    }

    public String getProductName() {
        return productName;
    }

    public void setProductName(String productName) {
        this.productName = productName;
    }

    public String getCityName() {
        return cityName;
    }

    public void setCityName(String cityName) {
        this.cityName = cityName;
    }

    public Date getDepartureTime() {
        return departureTime;
    }

    public void setDepartureTime(Date departureTime) {
        this.departureTime = departureTime;
    }

    public String getDepartureTimeStr() {
        return departureTimeStr;
    }

    public void setDepartureTimeStr(String departureTimeStr) {
        this.departureTimeStr = departureTimeStr;
    }

    public double getProductPrice() {
        return productPrice;
    }

    public void setProductPrice(double productPrice) {
        this.productPrice = productPrice;
    }

    public String getProductDesc() {
        return productDesc;
    }

    public void setProductDesc(String productDesc) {
        this.productDesc = productDesc;
    }

    public Integer getProductStatus() {
        return productStatus;
    }

    public void setProductStatus(Integer productStatus) {
        this.productStatus = productStatus;
    }

    public String getProductStatusStr() {

        if (productStatus != null) {
            if (productStatus == 0) {
                productStatusStr = "关闭";
            }
            if (productStatus == 1) {
                productStatusStr = "开启";
            }
        }
        return productStatusStr;
    }


    public void setProductStatusStr(String productStatusStr) {
        this.productStatusStr = productStatusStr;

    }


}

```
#### 1.6.4 编写持久层接口
```markdown
ssm_dao中编写com.yxj.ssm.dao.IProductDao
```

```java
package com.yxj.ssm.dao;

import com.yxj.ssm.domain.Product;
import org.apache.ibatis.annotations.Select;
import org.springframework.stereotype.Repository;

import java.util.List;

/**
 * @author shkstart
 * @date 2020/6/14 0014 - 下午 1:01
 */
@Repository
public interface IProductDao {

    @Select("select * from product")
    public List<Product> findAll() throws Exception;
}

```
#### 1.6.5 在ssm_service中编写业务接口
```markdown
com.yxj.ssm.service.IProductService
```

```java
package com.yxj.ssm.service;

```
在这里插入代码片
```

import com.yxj.ssm.domain.Product;

import java.util.List;

/**
 * @author shkstart
 * @date 2020/6/14 0014 - 下午 1:06
 */
public interface IProductService {

    public List<Product> findAll() throws Exception;
}
```
#### 1.6.6 编写
```markdown
com.yxj.ssm.service.impl.ProductServiceImpl
```
```java
package com.yxj.ssm.service.impl;

import com.yxj.ssm.service.IProductService;
import com.yxj.ssm.dao.IProductDao;
import com.yxj.ssm.domain.Product;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

/**
 * @author shkstart
 * @date 2020/6/14 0014 - 下午 1:11
 */
@Service
@Transactional
public class ProductServiceImpl implements IProductService {

    @Autowired
    private IProductDao productDao;
    @Override
    public List<Product> findAll() throws Exception {
        return productDao.findAll();
    }
}
	
```
### 1.7 SSM整合与产品查询
#### 1.7.1 Spring环境搭建
##### 1.7.1.1 编写Spring配置文件applicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/aop
	http://www.springframework.org/schema/aop/spring-aop.xsd
	http://www.springframework.org/schema/tx
	http://www.springframework.org/schema/tx/spring-tx.xsd">
    <!-- 开启注解扫描，管理service和dao -->
    <context:component-scan base-package="com.yxj.ssm.service">
    </context:component-scan>
    <context:component-scan base-package="com.yxj.ssm.dao">
    </context:component-scan>

    <context:property-placeholder location="classpath:db.properties"/>

    <!-- 配置连接池 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}" />
        <property name="jdbcUrl" value="${jdbc.url}" />
        <property name="user" value="${jdbc.username}" />
        <property name="password" value="${jdbc.password}" />
    </bean>

    <!-- 把交给IOC管理 SqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
    </bean>

    <!-- 扫描dao接口 -->
    <bean id="mapperScanner" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.yxj.ssm.dao"/>
    </bean>

    <!-- 配置Spring的声明式事务管理 -->
    <!-- 配置事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <tx:annotation-driven transaction-manager="transactionManager"/>


</beans>

```
##### 1.7.1.2 编写Spring配置文件spring-mvc.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="
           http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/mvc
           http://www.springframework.org/schema/mvc/spring-mvc.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context.xsd
           http://www.springframework.org/schema/aop
		http://www.springframework.org/schema/aop/spring-aop.xsd
           ">
    <!-- 扫描controller的注解，别的不扫描 -->
    <context:component-scan base-package="com.yxj.ssm.controller">
    </context:component-scan>

    <!-- 配置视图解析器 -->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- JSP文件所在的目录 -->
        <property name="prefix" value="/pages/" />
        <!-- 文件的后缀名 -->
        <property name="suffix" value=".jsp" />
    </bean>

    <!-- 设置静态资源不过滤 -->
    <mvc:resources location="/css/" mapping="/css/**" />
    <mvc:resources location="/img/" mapping="/img/**" />
    <mvc:resources location="/js/" mapping="/js/**" />
    <mvc:resources location="/plugins/" mapping="/plugins/**" />

    <!-- 开启对SpringMVC注解的支持 -->
    <mvc:annotation-driven />

    <!--
        支持AOP的注解支持，AOP底层使用代理技术
        JDK动态代理，要求必须有接口
        cglib代理，生成子类对象，proxy-target-class="true" 默认使用cglib的方式
    -->
    <aop:aspectj-autoproxy proxy-target-class="true"/>
</beans>

```
##### 1.7.1.3 编写web.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="
           http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/mvc
           http://www.springframework.org/schema/mvc/spring-mvc.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context.xsd
           http://www.springframework.org/schema/aop
		http://www.springframework.org/schema/aop/spring-aop.xsd
           ">
    <!-- 扫描controller的注解，别的不扫描 -->
    <context:component-scan base-package="com.yxj.ssm.controller">
    </context:component-scan>

    <!-- 配置视图解析器 -->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- JSP文件所在的目录 -->
        <property name="prefix" value="/pages/" />
        <!-- 文件的后缀名 -->
        <property name="suffix" value=".jsp" />
    </bean>

    <!-- 设置静态资源不过滤 -->
    <mvc:resources location="/css/" mapping="/css/**" />
    <mvc:resources location="/img/" mapping="/img/**" />
    <mvc:resources location="/js/" mapping="/js/**" />
    <mvc:resources location="/plugins/" mapping="/plugins/**" />

    <!-- 开启对SpringMVC注解的支持 -->
    <mvc:annotation-driven />

    <!--
        支持AOP的注解支持，AOP底层使用代理技术
        JDK动态代理，要求必须有接口
        cglib代理，生成子类对象，proxy-target-class="true" 默认使用cglib的方式
    -->
    <aop:aspectj-autoproxy proxy-target-class="true"/>
</beans>

```
##### 1.7.1.4 编写db.properties
```xml
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/yxjssm?useUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC
jdbc.username=root
jdbc.password=348697
```
##### 1.7.1.5 在ssm_web编写com.yxj.ssm.controller.ProductController
```java
package com.yxj.ssm.controller;

import com.yxj.ssm.domain.Product;
import com.yxj.ssm.service.IProductService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

import java.util.List;

/**
 * @author shkstart
 * @date 2020/6/14 0014 - 下午 8:22
 */
@Controller
@RequestMapping("/product")
public class ProductController {

    @Autowired
    private IProductService productService;

    @RequestMapping("/findAll.do")
    public ModelAndView findAll() throws Exception {
        ModelAndView mv = new ModelAndView();
        List<Product> ps = productService.findAll();
        mv.addObject("productList",ps);
        mv.setViewName("product-list");
        return mv;
    }
}
```
##### 1.7.1.6 在ssm_web编写index.jsp
```html
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2020/6/14 0014
  Time: 下午 8:47
  To change this template use File | Settings | File Templates.
--%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
         pageEncoding="UTF-8"%>
<head>
    <title>Title</title>
</head>
<body>
    <a href="${pageContext.request.contextPath}/product/findAll.do">查询所有的产品信息</a>
</body>
</html>

```
##### 1.7.1.7 编写pages页面
```markdown
aside.jsp
header.jsp
product-list.jsp
```
##### 1.7.1.8 copy pages页面依赖文件
```markdown
css
img
plugins
```

```sql
orders.sql

-- 会员
CREATE TABLE member(
  id varchar(36)  PRIMARY KEY,
  NAME VARCHAR(20),
  nickname VARCHAR(20),
  phoneNum VARCHAR(20),
  email VARCHAR(20)
);
insert into MEMBER (id, name, nickname, phonenum, email)
values (uuid(), '张三', '小三', '18888888888', 'zs@163.com');

-- 旅客
CREATE TABLE traveller(
  id varchar(36) PRIMARY KEY,
  NAME VARCHAR(20),
  sex VARCHAR(20),
  phoneNum VARCHAR(20),
  credentialsType INT,
  credentialsNum VARCHAR(50),
  travellerType INT
);
insert into TRAVELLER (id, name, sex, phonenum, credentialstype, credentialsnum, travellertype)
values (uuid(), '张龙', '男', '13333333333', 0, '123456789009876543', 0);
insert into TRAVELLER (id, name, sex, phonenum, credentialstype, credentialsnum, travellertype)
values (uuid(), '张小龙', '男', '15555555555', 0, '987654321123456789', 1);



-- 订单
CREATE TABLE orders(
  id varchar(36) PRIMARY KEY,
  orderNum VARCHAR(20) NOT NULL UNIQUE,
  orderTime date,
  peopleCount INT,
  orderDesc VARCHAR(500),
  payType INT,
  orderStatus INT,
  productId varchar(36),
  memberId varchar(36),
  FOREIGN KEY (productId) REFERENCES product(id),
  FOREIGN KEY (memberId) REFERENCES member(id)
);
insert into ORDERS (id, orderNum, ordertime, peoplecount, orderdesc, paytype, orderstatus, productid, memberid)
values (uuid(), '12345', str_to_date('2020-6-11','%Y-%c-%d'), 2, '没什么', 0, 1, '89fd15b5-b101-11ea-824a-e8039aa6b14e', '6d0b50e9-b12a-11ea-824a-e8039aa6b14e');
insert into ORDERS (id, ordernum, ordertime, peoplecount, orderdesc, paytype, orderstatus, productid, memberid)
values (uuid(), '54321', str_to_date('2020-6-11','%Y-%c-%d'), 2, '没什么', 0, 1, '89fd15b5-b101-11ea-824a-e8039aa6b14e', '6d0b50e9-b12a-11ea-824a-e8039aa6b14e');
insert into ORDERS (id, ordernum, ordertime, peoplecount, orderdesc, paytype, orderstatus, productid, memberid)
values (uuid(), '67890', str_to_date('2020-6-11','%Y-%c-%d'), 2, '没什么', 0, 1, '89fd15b5-b101-11ea-824a-e8039aa6b14e', '6d0b50e9-b12a-11ea-824a-e8039aa6b14e');
insert into ORDERS (id, ordernum, ordertime, peoplecount, orderdesc, paytype, orderstatus, productid, memberid)
values (uuid(), '98765',str_to_date('2020-6-11','%Y-%c-%d'), 2, '没什么', 0, 1, '8a06bbf8-b101-11ea-824a-e8039aa6b14e', '6d0b50e9-b12a-11ea-824a-e8039aa6b14e');
insert into ORDERS (id, ordernum, ordertime, peoplecount, orderdesc, paytype, orderstatus, productid, memberid)
values (uuid(), '11111', str_to_date('2020-6-11','%Y-%c-%d'), 2, '没什么', 0, 1, '8a06bbf8-b101-11ea-824a-e8039aa6b14e', '6d0b50e9-b12a-11ea-824a-e8039aa6b14e');
insert into ORDERS (id, ordernum, ordertime, peoplecount, orderdesc, paytype, orderstatus, productid, memberid)
values (uuid(), '22222', str_to_date('2020-6-11','%Y-%c-%d'), 2, '没什么', 0, 1, '8a0cebe1-b101-11ea-824a-e8039aa6b14e', '6d0b50e9-b12a-11ea-824a-e8039aa6b14e');
insert into ORDERS (id, ordernum, ordertime, peoplecount, orderdesc, paytype, orderstatus, productid, memberid)
values (uuid(), '33333', str_to_date('2020-6-11','%Y-%c-%d'), 2, '没什么', 0, 1, '8a0cebe1-b101-11ea-824a-e8039aa6b14e', '6d0b50e9-b12a-11ea-824a-e8039aa6b14e');
insert into ORDERS (id, ordernum, ordertime, peoplecount, orderdesc, paytype, orderstatus, productid, memberid)
values (uuid(), '44444', str_to_date('2020-6-11','%Y-%c-%d'), 2, '没什么', 0, 1, '8a0cebe1-b101-11ea-824a-e8039aa6b14e', '6d0b50e9-b12a-11ea-824a-e8039aa6b14e');
insert into ORDERS (id, ordernum, ordertime, peoplecount, orderdesc, paytype, orderstatus, productid, memberid)
values (uuid(), '55555', str_to_date('2020-6-11','%Y-%c-%d'), 2, '没什么', 0, 1, '8d49afff-b123-11ea-824a-e8039aa6b14e', '6d0b50e9-b12a-11ea-824a-e8039aa6b14e');



-- 订单与旅客中间表
CREATE TABLE order_traveller(
  orderId varchar(36),
  travellerId varchar(36),
  PRIMARY KEY (orderId,travellerId),
  FOREIGN KEY (orderId) REFERENCES orders(id),
  FOREIGN KEY (travellerId) REFERENCES traveller(id)
);

insert into ORDER_TRAVELLER (orderid, travellerid)
values ('e4ab628d-b12b-11ea-824a-e8039aa6b14e', '7ee65ffc-b12a-11ea-824a-e8039aa6b14e');
insert into ORDER_TRAVELLER (orderid, travellerid)
values ('f10a30ae-b12b-11ea-824a-e8039aa6b14e', '7ee9ddf3-b12a-11ea-824a-e8039aa6b14e');
insert into ORDER_TRAVELLER (orderid, travellerid)
values ('f10d4169-b12b-11ea-824a-e8039aa6b14e', '7ee65ffc-b12a-11ea-824a-e8039aa6b14e');
insert into ORDER_TRAVELLER (orderid, travellerid)
values ('f1106881-b12b-11ea-824a-e8039aa6b14e', '7ee9ddf3-b12a-11ea-824a-e8039aa6b14e');
insert into ORDER_TRAVELLER (orderid, travellerid)
values ('f114504b-b12b-11ea-824a-e8039aa6b14e', '7ee65ffc-b12a-11ea-824a-e8039aa6b14e');
insert into ORDER_TRAVELLER (orderid, travellerid)
values ('f11734a5-b12b-11ea-824a-e8039aa6b14e', '7ee9ddf3-b12a-11ea-824a-e8039aa6b14e');
insert into ORDER_TRAVELLER (orderid, travellerid)
values ('f119fa9e-b12b-11ea-824a-e8039aa6b14e', '7ee65ffc-b12a-11ea-824a-e8039aa6b14e');
insert into ORDER_TRAVELLER (orderid, travellerid)
values ('f11cfe61-b12b-11ea-824a-e8039aa6b14e', '7ee9ddf3-b12a-11ea-824a-e8039aa6b14e');
insert into ORDER_TRAVELLER (orderid, travellerid)
values ('f1202e9c-b12b-11ea-824a-e8039aa6b14e', '7ee65ffc-b12a-11ea-824a-e8039aa6b14e');
```


