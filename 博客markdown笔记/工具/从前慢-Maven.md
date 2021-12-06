# maven
![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/4c360d16a01a6071c1d32778d9a671dc.png#pic_center)

## 1 maven的配置
~~~markdown
添加环境变量 MAVEN_HOME:E:\Maven\apache-maven-3.3.9
编辑系统变量 Path，添加变量值:%MAVEN_HOME%\bin
~~~
## 2 maven作用
```markdown
maven是一个项目管理工具
依赖管理：传统工程我们直按把jar包放置在项目中，
maven工程真正的jar包放置在仓库中，项目中只用放置jar包的坐标
仓库的种类：本地仓库，远程仓库【私服】，中央仓库。
```
## 3 maven生命周期
~~~markdown
清理，编译，测试，报告，打包，安装，部署
~~~
```markdown
mvn clean清理（会删除原来编译和测试的目录，即 target目录，
但是已经install到仓库里的包不会删除)
mvn compile编译主程序(会在当前目录下生成一个target里
边存放编译主程序之后生成的字节码文件）
mvn test-compile
编译测试程序（会在当前目录下生成一个 target，里边存放编
译测试程序之后生成的字节码文件）
mvn test测试（会生成一个目录 surefire-reports，保存测试结果）
mvn package打包主程序（会编译、编译测试、测试、并
且按照pom.xml配置把主程序打包生成jar包或者war包
mvn insta‖l安装主程序（会把本工程打包，并且按照本工程的坐
标保存到本地仓库中）
mvn deploy部署主程序（会把本工程打包，按照本工程的坐标保
存到本地库中，并且还会保存到私服仓库中。还会自动把项目部
署到web容器中）.
```
## 4 maven如何管理jar包
~~~markdown
先从本地仓库拿,如果没有的话,就从局域网中的私服仓库拿,如
果还没有分为两种情况:如果有镜像的话,就可以从镜像拿;如
果没有的话,就从中央仓库里拿;
~~~
## 5 maven目录结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200616235743421.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
## 6 maven坐标的配置说明
```xml
<groupId>公司域名的倒写</groupId>
<artifact>自定义项目名称</artifact>
<version>自定版本号</version>
```
## 7 单元测试注意事项
```markdown
1 方法是 public的，必须的
2 方法没有返回值，必须的
3 方法名称是自定义的，推荐是test+方法名称
4 在方法的上面加入@Test
```

## 8 Maven依赖的范围
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200617080422185.png)
## 9 maven常用操作
```markdown
1 maven的属性设置
<properties>设置 maven的常用属性
2 maven的全局变量
自定义的属性，1.在<properties>通过自定义标签声明变
量（标签名就是变量名）
3 在pom.xm文件中的其它位置，使用${标签名}使用变量的值
自定义全局变量一般是定义依赖的版本号，当你的项目中要使
用多个相同的版本号，先使用全局变量定义，在使用$（变量名}
```
## 10 依赖传递
```markdown
会发现出现除spring-webmvc 以外还有其他 jar。因为我
们的项目依赖 spring-webmvc.jar，而 spring-webmvc.jar 会
依赖spring-beans.jar 等等，所以 spring-beans.jar 这些
jar包也出现在了我们的maven工程中，这种现象我们称
为依赖传递
```
```xml
<dependencies>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-webmvc</artifactId>
		<version>4.2.4.RELEASE</version>
	</dependency>
</dependencies>

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200617092937375.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
## 11 Maven导入jar包时冲突的解决
```markdown
maven工程要导入jar包的坐标，就必须要考虑解决jar包冲突。
      解决jar包冲突的方式一：
      第一声明优先原则：哪个jar包的坐标在靠上的位置，
      这个jar包 就是先声明的。
      先声明的jar包坐标下的依赖包，可以优先进入项目中。

      maven导入jar包中的一些概念：
      直接依赖：项目中直接导入的jar包，就是该项目的直接依赖包。
      传递依赖：项目中没有直接导入的jar包，可以通过直接依
      赖jar包相关联的jar包传递到项目中去。

      解决jar包冲突的方式二：
      路径近者优先原则。直接依赖路径比传递依赖路径近，那么最
      终项目进入的jar包会是路径近的直接依赖包。

      解决jar包冲突的方式三【推荐使用】：
      直接排除法。
      当我们要排除某个jar包下依赖包，在配置exclusions标签的时候，
      内部可以不写版本号。
      因为此时依赖包使用的版本和默认和本jar包一样。
      -->
  
    <!--
    maven工程是可以分父子依赖关系的。
    凡是依赖别的项目后，拿到的别的项目的依赖包，都属于传递依赖。
    比如：当前A项目，被B项目依赖。那么我们A项目中所有jar包都
    会传递到B项目中。
    B项目开发者，如果再在B项目中导入一套ssm框架的jar包，对于B
    项目是直接依赖。
    那么直接依赖的jar包就会把我们A项目传递过去的jar包覆盖掉。
    为了防止以上情况的出现。我们可以把A项目中主要jar包的坐标锁
    住，那么其他依赖该项目的项目中，
    即便是有同名jar包直接依赖，也无法覆盖。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200617093143245.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
## 12 简单版maven下的ssm整合
```xml

pom.xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.yxj</groupId>
  <artifactId>maven_day02_1</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>


  <!--maven工程要导入jar包的坐标，就必须要考虑解决jar包冲突。
      解决jar包冲突的方式一：
      第一声明优先原则：哪个jar包的坐标在靠上的位置，这个jar包就是先声明的。
      先声明的jar包坐标下的依赖包，可以优先进入项目中。

      maven导入jar包中的一些概念：
      直接依赖：项目中直接导入的jar包，就是该项目的直接依赖包。
      传递依赖：项目中没有直接导入的jar包，可以通过项目直接依赖jar包传递到项目中去。

      解决jar包冲突的方式二：
      路径近者优先原则。直接依赖路径比传递依赖路径近，那么最终项目进入的jar包会是路径近的直接依赖包。

      解决jar包冲突的方式三【推荐使用】：
      直接排除法。
      当我们要排除某个jar包下依赖包，在配置exclusions标签的时候，内部可以不写版本号。
      因为此时依赖包使用的版本和默认和本jar包一样。
      -->
    <!-- 统一管理jar包版本 -->
    <properties>
      <spring.version>5.0.2.RELEASE</spring.version>
      <slf4j.version>1.6.6</slf4j.version>
      <log4j.version>1.2.12</log4j.version>
      <shiro.version>1.2.3</shiro.version>
      <mysql.version>5.1.6</mysql.version>
      <mybatis.version>3.4.5</mybatis.version>
      <spring.security.version>5.0.1.RELEASE</spring.security.version>
    </properties>

    <!--
    maven工程是可以分父子依赖关系的。
    凡是依赖别的项目后，拿到的别的项目的依赖包，都属于传递依赖。
    比如：当前A项目，被B项目依赖。那么我们A项目中所有jar包都会传递到B项目中。
    B项目开发者，如果再在B项目中导入一套ssm框架的jar包，对于B项目是直接依赖。
    那么直接依赖的jar包就会把我们A项目传递过去的jar包覆盖掉。
    为了防止以上情况的出现。我们可以把A项目中主要jar包的坐标锁住，那么其他依赖该项目的项目中，
    即便是有同名jar包直接依赖，也无法覆盖。
    -->
    <!-- 锁定jar包版本 -->
    <dependencyManagement>
      <dependencies>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>${spring.version}</version>
        </dependency>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-web</artifactId>
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
          <groupId>org.springframework</groupId>
          <artifactId>spring-test</artifactId>
          <version>${spring.version}</version>
        </dependency>
        <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis</artifactId>
          <version>${mybatis.version}</version>
        </dependency>
      </dependencies>
    </dependencyManagement>

    <!-- 项目依赖jar包 -->
    <dependencies>
      <!-- spring -->
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
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>${mysql.version}</version>
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
      </dependency>
      <!-- log start -->
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
      </dependency>
      <!-- log end -->
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
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.0.9</version>
      </dependency>
      <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
      </dependency>
    </dependencies>
    <!-- 添加tomcat7插件 -->
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.tomcat.maven</groupId>
          <artifactId>tomcat7-maven-plugin</artifactId>
          <version>2.2</version>
        </plugin>
      </plugins>
    </build>
</project>

```
```java
Items.java

package com.yxj.domain;

import java.util.Date;

public class Items {
    private Integer id;
    private String name;
    private Double price;
    private String pic;
    private Date createtime;
    private String detail;

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

    public Double getPrice() {
        return price;
    }

    public void setPrice(Double price) {
        this.price = price;
    }

    public String getPic() {
        return pic;
    }

    public void setPic(String pic) {
        this.pic = pic;
    }

    public Date getCreatetime() {
        return createtime;
    }

    public void setCreatetime(Date createtime) {
        this.createtime = createtime;
    }

    public String getDetail() {
        return detail;
    }

    public void setDetail(String detail) {
        this.detail = detail;
    }
}

```
```java
ItemsDao.java

package com.yxj.dao;

import com.yxj.domain.Items;

public interface ItemsDao {
    public Items findById(Integer id);
}
```
```xml
ItemsDao.xml

<?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper
                PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.yxj.dao.ItemsDao">
    <select id="findById" parameterType="int" resultType="items">
        select * from items where id = #{id}
    </select>
</mapper>
```
```xml
applicationContext.xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
			    http://www.springframework.org/schema/beans/spring-beans.xsd
			    http://www.springframework.org/schema/context
			    http://www.springframework.org/schema/context/spring-context.xsd
			    http://www.springframework.org/schema/aop
			    http://www.springframework.org/schema/aop/spring-aop.xsd
			    http://www.springframework.org/schema/tx
			    http://www.springframework.org/schema/tx/spring-tx.xsd
			    http://www.springframework.org/schema/mvc
			    http://www.springframework.org/schema/mvc/spring-mvc.xsd">
   
    <!--dao层配置文件开始-->
    
    <!--配置连接池-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql:///maven"/>
        <property name="username" value="root"/>
        <property name="password" value="root"/>
    </bean>

    <!--配置生产SqlSession对象的工厂-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--扫描pojo包，给包下所有pojo对象起别名-->
        <property name="typeAliasesPackage" value="com.yxj.domain"/>
    </bean>

    <!--扫描接口包路径，生成包下所有接口的代理对象，并且放入spring容器中-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.yxj.dao"/>
    </bean>
    <!--dao层配置文件结束-->

</beans>
```
```properties
log4j.properties

# Set root category priority to INFO and its only appender to CONSOLE.
#log4j.rootCategory=INFO, CONSOLE            debug   info   warn error fatal
log4j.rootCategory=debug, CONSOLE, LOGFILE

# Set the enterprise logger category to FATAL and its only appender to CONSOLE.
log4j.logger.org.apache.axis.enterprise=FATAL, CONSOLE

# CONSOLE is set to be a ConsoleAppender using a PatternLayout.
log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
log4j.appender.CONSOLE.layout.ConversionPattern=%d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m\n

# LOGFILE is set to be a File appender using a PatternLayout.
log4j.appender.LOGFILE=org.apache.log4j.FileAppender
log4j.appender.LOGFILE.File=d:\axis.log
log4j.appender.LOGFILE.Append=true
log4j.appender.LOGFILE.layout=org.apache.log4j.PatternLayout
log4j.appender.LOGFILE.layout.ConversionPattern=%d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m\n


```
```java
ItemsTest.java

package com.yxj.test;

import com.yxj.domain.Items;
import com.yxj.service.ItemsService;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class ItemsTest {
    @Test
    public void findById(){
        //获取spring容器
        ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
       // dao测试
       // 从容器中拿到所需的dao的代理对象
       ItemsDao itemsDao = ac.getBean(ItemsDao.class);
      //  调用方法
       Items items = itemsDao.findById(1);
       System.out.println(items.getName());
    
    }
}

```


```java
Service.java

package com.yxj.service;

import com.yxj.domain.Items;

public interface ItemsService {
    public Items findById(Integer id);
}

```
```java
ItemsServiceImpl.java

package com.yxj.service.impl;

import com.yxj.dao.ItemsDao;
import com.yxj.domain.Items;
import com.yxj.service.ItemsService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class ItemsServiceImpl implements ItemsService {

    @Autowired
    private ItemsDao itemsDao;

    public Items findById(Integer id) {
        return itemsDao.findById(id);
    }
}

```

```xml
applicationContext.xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
			    http://www.springframework.org/schema/beans/spring-beans.xsd
			    http://www.springframework.org/schema/context
			    http://www.springframework.org/schema/context/spring-context.xsd
			    http://www.springframework.org/schema/aop
			    http://www.springframework.org/schema/aop/spring-aop.xsd
			    http://www.springframework.org/schema/tx
			    http://www.springframework.org/schema/tx/spring-tx.xsd
			    http://www.springframework.org/schema/mvc
			    http://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <!--dao层配置文件开始-->
    <!--配置连接池-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql:///maven"/>
        <property name="username" value="root"/>
        <property name="password" value="root"/>
    </bean>

    <!--配置生产SqlSession对象的工厂-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--扫描pojo包，给包下所有pojo对象起别名-->
        <property name="typeAliasesPackage" value="com.yxj.domain"/>
    </bean>

    <!--扫描接口包路径，生成包下所有接口的代理对象，并且放入spring容器中-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.yxj.dao"/>
    </bean>
    <!--dao层配置文件结束-->


    <!--service层配置文件开始-->

    <!--组件扫描配置-->
    <context:component-scan base-package="com.yxj.service"/>

    <!--aop面向切面编程，切面就是切入点和通知的组合-->
    <!--配置事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <!--配置事务的通知-->
    <tx:advice id="advice">
        <tx:attributes>
            <tx:method name="save*" propagation="REQUIRED"/>
            <tx:method name="update*" propagation="REQUIRED"/>
            <tx:method name="delete*" propagation="REQUIRED"/>
            <tx:method name="find*" read-only="true"/>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>

    <!--配置切面-->
    <aop:config>
        <aop:pointcut id="pointcut" expression="execution(* com.yxj.service.impl.*.*(..))"/>
        <aop:advisor advice-ref="advice" pointcut-ref="pointcut"/>
    </aop:config>
    <!--service层配置文件结束-->

</beans>
```

```java
ItemsTest.java

package com.yxj.test;

import com.yxj.domain.Items;
import com.yxj.service.ItemsService;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class ItemsTest {
    @Test
    public void findById(){
        //获取spring容器
        ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
    
        //service测试
        ItemsService itemsService = ac.getBean(ItemsService.class);
        //调用方法
        Items items = itemsService.findById(1);
        System.out.println(items.getName());
    }
}

```
```java
ItemsController.java	

package com.yxj.controller;

import com.yxj.domain.Items;
import com.yxj.service.ItemsService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/items")
public class ItemsController {

    @Autowired
    private ItemsService itemsService;

    @RequestMapping("/findDetail")
    public String findDetail(Model model){
        Items items = itemsService.findById(1);
        model.addAttribute("item", items);
        return "itemDetail";
    }
}

```
```xml
springmvc.xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
			    http://www.springframework.org/schema/beans/spring-beans.xsd
			    http://www.springframework.org/schema/context
			    http://www.springframework.org/schema/context/spring-context.xsd
			    http://www.springframework.org/schema/aop
			    http://www.springframework.org/schema/aop/spring-aop.xsd
			    http://www.springframework.org/schema/tx
			    http://www.springframework.org/schema/tx/spring-tx.xsd
			    http://www.springframework.org/schema/mvc
			    http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--组件扫描-->
    <context:component-scan base-package="com.yxj.controller"/>

    <!--处理器映射器，处理器适配器-->
    <mvc:annotation-driven/>

    <!--视图解析器-->
    <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/pages/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

    <!--释放静态资源-->
    <mvc:default-servlet-handler/>


</beans>
```
```html
itemDetail.jsp

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt"  prefix="fmt"%>    
 
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body> 
	<form>
		<table width="100%" border=1>
			<tr>
				<td>商品名称</td>
				<td> ${item.name } </td>
			</tr>
			<tr>
				<td>商品价格</td>
				<td> ${item.price } </td>
			</tr>
			<tr>
				<td>生成日期</td>
				<td> <fmt:formatDate value="${item.createtime}" pattern="yyyy-MM-dd HH:mm:ss"/> </td>
			</tr>
			<tr>
				<td>商品简介</td>
				<td>${item.detail} </td>
			</tr>
		</table>
	</form>
</body>
</html>
```

```xml
web.xml

<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
          http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         version="3.0">

    <!--编码过滤器-->
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    
    <!--配置spring核心监听器-->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <!--重新指定spring配置文件的路径-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>

    <!--springmvc的核心servlet-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>

```
## 13 拆分跟聚合思想maven下的ssm整合
```markdown
理解继承和聚合
通常继承和聚合同时使用。
何为继承？
继承是为了消除重复，如果将 dao、service、web 分开创建独
立的工程则每个工程的 pom.xml 文件中的内容存在重复，比
如：设置编译版本、锁定 spring 的版本的等，可以将这些重复
的 配置提取出来在父工程的 pom.xml  中定义。

何为聚合？ 项目开发通常是分组分模块开发，每个模块开发完
成要运行整个工程需要将每个模块聚合在 一起运行，
比如：dao、service、web   三个工程最终会打一个
独立的war运行。
```
```markdown
使用拆分跟聚合思想maven下的ssm整合
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200617170829997.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
创建父工程
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200617140719509.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200617140742189.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200617140801181.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200617140820977.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200617141018819.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
创建dao子模块
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200617141217550.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210125110458587.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200617141525298.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
创建service模块
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200617142003618.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200617142023977.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200617142100908.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/202006171421246.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
创建web模块
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200617142419403.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
创建完后的变化
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210120213825397.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210125110855350.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
目录
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623132458684.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623132545384.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623132619882.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```properties
log4j.properties

# Set root category priority to INFO and its only appender to CONSOLE.
#log4j.rootCategory=INFO, CONSOLE            debug   info   warn error fatal
log4j.rootCategory=debug, CONSOLE, LOGFILE

# Set the enterprise logger category to FATAL and its only appender to CONSOLE.
log4j.logger.org.apache.axis.enterprise=FATAL, CONSOLE

# CONSOLE is set to be a ConsoleAppender using a PatternLayout.
log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
log4j.appender.CONSOLE.layout.ConversionPattern=%d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m\n

# LOGFILE is set to be a File appender using a PatternLayout.
log4j.appender.LOGFILE=org.apache.log4j.FileAppender
log4j.appender.LOGFILE.File=d:\axis.log
log4j.appender.LOGFILE.Append=true
log4j.appender.LOGFILE.layout=org.apache.log4j.PatternLayout
log4j.appender.LOGFILE.layout.ConversionPattern=%d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m\n


```
```markdown
maven_day02_parent
```

```xml
pom.xml

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!--
    工程和模块的区别：
    工程不等于完整的项目，模块也不等于完整的项目，一个完整的项目看的是代码，代码完整，就可以说这是一个完整的项目
    和此项目是工程和模块没有关系。

    工程天生只能使用自己内部资源，工程天生是独立的。后天可以和其他工程或模块建立关联关系。
    模块天生不是独立的，模块天生是属于父工程的，模块一旦创建，所有父工程的资源都可以使用。

    父子工程直接，子模块天生集成父工程，可以使用父工程所有资源。
    子模块之间天生是没有任何关系的。

    父子工程直接不用建立关系，继承关系是先天的，不需要手动建立。

    平级直接的引用叫依赖，依赖不是先天的，依赖是需要后天建立的。
    -->
    <groupId>com.yxj</groupId>
    <artifactId>maven_day02_parent</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>maven_day02_dao</module>
        <module>maven_day02_service</module>
        <module>maven_day02_web</module>
    </modules>

    <!-- 统一管理jar包版本 -->
    <properties>
        <spring.version>5.0.2.RELEASE</spring.version>
        <slf4j.version>1.6.6</slf4j.version>
        <log4j.version>1.2.12</log4j.version>
        <shiro.version>1.2.3</shiro.version>
        <mysql.version>5.1.6</mysql.version>
        <mybatis.version>3.4.5</mybatis.version>
        <spring.security.version>5.0.1.RELEASE</spring.security.version>
    </properties>

    <!-- 锁定jar包版本 -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-context</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-web</artifactId>
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
                <groupId>org.springframework</groupId>
                <artifactId>spring-test</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.mybatis</groupId>
                <artifactId>mybatis</artifactId>
                <version>${mybatis.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <!-- 项目依赖jar包 -->
    <dependencies>
        <!-- spring -->
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
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>${mysql.version}</version>
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
        </dependency>
        <!-- log start -->
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
        </dependency>
        <!-- log end -->
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
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.0.9</version>
        </dependency>
    </dependencies>
    <!-- 添加tomcat7插件 -->
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <version>2.2</version>
            </plugin>
        </plugins>
    </build>

</project>

```
```markdown
maven_day02_dao
```
```xml
pom.xml

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>maven_day02_parent</artifactId>
        <groupId>com.yxj</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>maven_day02_dao</artifactId>


</project>
```

```java
Items

package com.yxj.domain;

import java.util.Date;

public class Items {
    private Integer id;
    private String name;
    private Double price;
    private String pic;
    private Date createtime;
    private String detail;

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

    public Double getPrice() {
        return price;
    }

    public void setPrice(Double price) {
        this.price = price;
    }

    public String getPic() {
        return pic;
    }

    public void setPic(String pic) {
        this.pic = pic;
    }

    public Date getCreatetime() {
        return createtime;
    }

    public void setCreatetime(Date createtime) {
        this.createtime = createtime;
    }

    public String getDetail() {
        return detail;
    }

    public void setDetail(String detail) {
        this.detail = detail;
    }
}
```

```java
ItemsDao

package com.yxj.dao;

import com.yxj.domain.Items;

public interface ItemsDao {
    public Items findById(Integer id);
}

```

```xml
ItemsDao.xml

<?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper
                PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.yxj.dao.ItemsDao">
    <select id="findById" parameterType="int" resultType="items">
        select * from items where id = #{id}
    </select>
</mapper>
```

```xml
applicationContext-dao.xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
			    http://www.springframework.org/schema/beans/spring-beans.xsd
			    http://www.springframework.org/schema/context
			    http://www.springframework.org/schema/context/spring-context.xsd
			    http://www.springframework.org/schema/aop
			    http://www.springframework.org/schema/aop/spring-aop.xsd
			    http://www.springframework.org/schema/tx
			    http://www.springframework.org/schema/tx/spring-tx.xsd
			    http://www.springframework.org/schema/mvc
			    http://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <!--dao层配置文件开始-->
    <!--配置连接池-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql:///maven"/>
        <property name="username" value="root"/>
        <property name="password" value="348697"/>
    </bean>

    <!--配置生产SqlSession对象的工厂-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--扫描pojo包，给包下所有pojo对象起别名-->
        <property name="typeAliasesPackage" value="com.yxj.domain"/>
    </bean>

    <!--扫描接口包路径，生成包下所有接口的代理对象，并且放入spring容器中-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.yxj.dao"/>
    </bean>
    <!--dao层配置文件结束-->

</beans>

```
```markdown
maven_day02_service
```

```xml
pom.xml

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>maven_day02_parent</artifactId>
        <groupId>com.yxj</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>maven_day02_service</artifactId>

    <dependencies>
        <dependency>
            <groupId>com.yxj</groupId>
            <artifactId>maven_day02_dao</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>
</project>
```
```java
ItemsService

package com.yxj.service;

import com.yxj.domain.Items;

public interface ItemsService {
    public Items findById(Integer id);
}

```
```java
ItemsServiceImpl

package com.yxj.service.impl;

import com.yxj.dao.ItemsDao;
import com.yxj.domain.Items;
import com.yxj.service.ItemsService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class ItemsServiceImpl implements ItemsService {

    @Autowired
    private ItemsDao itemsDao;

    public Items findById(Integer id) {
        return itemsDao.findById(id);
    }
}

```

```xml
applicationContext-service.xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
			    http://www.springframework.org/schema/beans/spring-beans.xsd
			    http://www.springframework.org/schema/context
			    http://www.springframework.org/schema/context/spring-context.xsd
			    http://www.springframework.org/schema/aop
			    http://www.springframework.org/schema/aop/spring-aop.xsd
			    http://www.springframework.org/schema/tx
			    http://www.springframework.org/schema/tx/spring-tx.xsd
			    http://www.springframework.org/schema/mvc
			    http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--service层配置文件开始-->

    <!--组件扫描配置-->
    <context:component-scan base-package="com.yxj.service"/>

    <!--aop面向切面编程，切面就是切入点和通知的组合-->
    <!--配置事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <!--配置事务的通知-->
    <tx:advice id="advice">
        <tx:attributes>
            <tx:method name="save*" propagation="REQUIRED"/>
            <tx:method name="update*" propagation="REQUIRED"/>
            <tx:method name="delete*" propagation="REQUIRED"/>
            <tx:method name="find*" read-only="true"/>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>

    <!--配置切面-->
    <aop:config>
        <aop:pointcut id="pointcut" expression="execution(* com.yxj.service.impl.*.*(..))"/>
        <aop:advisor advice-ref="advice" pointcut-ref="pointcut"/>
    </aop:config>
    <!--service层配置文件结束-->


</beans>
```
```markdown
maven_day02_web
```
```xml
pom.xml

<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>maven_day02_parent</artifactId>
        <groupId>com.yxj</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>maven_day02_web</artifactId>
    <packaging>war</packaging>

    <dependencies>
        <dependency>
            <groupId>com.yxj</groupId>
            <artifactId>maven_day02_service</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>
</project>

```

```markdown
web.xml
```
```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
          http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         version="3.0">

  <!--编码过滤器-->
  <filter>
    <filter-name>encoding</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
      <param-name>forceEncoding</param-name>
      <param-value>true</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>

  <!--配置spring核心监听器-->
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  <!--重新指定spring配置文件的路径-->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
  </context-param>

  <!--springmvc的核心servlet-->
  <servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:springmvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>
```

```xml
applicationContext.xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
			    http://www.springframework.org/schema/beans/spring-beans.xsd
			    http://www.springframework.org/schema/context
			    http://www.springframework.org/schema/context/spring-context.xsd
			    http://www.springframework.org/schema/aop
			    http://www.springframework.org/schema/aop/spring-aop.xsd
			    http://www.springframework.org/schema/tx
			    http://www.springframework.org/schema/tx/spring-tx.xsd
			    http://www.springframework.org/schema/mvc
			    http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <import resource="classpath:spring/applicationContext-dao.xml"/>
    <import resource="classpath:spring/applicationContext-service.xml"/>
</beans>
```

```xml
springmvc.xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
			    http://www.springframework.org/schema/beans/spring-beans.xsd
			    http://www.springframework.org/schema/context
			    http://www.springframework.org/schema/context/spring-context.xsd
			    http://www.springframework.org/schema/aop
			    http://www.springframework.org/schema/aop/spring-aop.xsd
			    http://www.springframework.org/schema/tx
			    http://www.springframework.org/schema/tx/spring-tx.xsd
			    http://www.springframework.org/schema/mvc
			    http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--组件扫描-->
    <context:component-scan base-package="com.yxj.controller"/>

    <!--处理器映射器，处理器适配器-->
    <mvc:annotation-driven/>

    <!--视图解析器-->
    <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/pages/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

    <!--释放静态资源-->
    <mvc:default-servlet-handler/>


</beans>
```
```html
itemDetail.jsp

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt"  prefix="fmt"%>    
 
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body> 
	<form>
		<table width="100%" border=1>
			<tr>
				<td>商品名称</td>
				<td> ${item.name } </td>
			</tr>
			<tr>
				<td>商品价格</td>
				<td> ${item.price } </td>
			</tr>
			<tr>
				<td>生成日期</td>
				<td> <fmt:formatDate value="${item.createtime}" pattern="yyyy-MM-dd HH:mm:ss"/> </td>
			</tr>
			<tr>
				<td>商品简介</td>
				<td>${item.detail} </td>
			</tr>
		</table>
	</form>
</body>
</html>
```

```html
index.jsp

<html>
<body>
<h2>Hello World!</h2>
</body>
</html>

```

```java
ItemsController

package com.yxj.controller;

import com.yxj.domain.Items;
import com.yxj.service.ItemsService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/items")
public class ItemsController {

    @Autowired
    private ItemsService itemsService;

    @RequestMapping("/findDetail")
    public String findDetail(Model model){
        Items items = itemsService.findById(1);
        model.addAttribute("item", items);
        return "itemDetail";
    }
}

```
```markdown
运行调试

方法 1：在 ssm_web 工程的 pom.xml 中配置 tomcat 插件运行
运行 ssm_web 工程它会从本地仓库下载依赖的 jar 包，所
以当 ssm_web 依赖的 jar包内容修改了必须及时发布到本地仓库，
比如：ssm_web 依赖的 ssm_service 修改了，
需要及时将ssm_service 发布到本地仓库。

方法 2：在父工程的 pom.xml 中配置 tomcat 插件运行，自动聚合
并执行
推荐方法 2，如果子工程都在本地，采用方法 2 则不需要子工程修改
就立即发布到本地仓库， 父工程会自动聚合并使用最新代码执行。

注意：如果子工程和父工程中都配置了tomcat插件，运行的端口
和路径以子工程为准。

方法三：直接配置本地Tomcat运行。
```
	

## 14 maven私服
```markdown
私服是指私有服务器，是架设在局域网的一种特殊的远程仓库，
目的是代理远程仓库及部署第三方构建。有了私服之后，当 
Maven 需要下载构件时，直接请求私服，私服上存在则下载
到本地仓库；否则，私服请求外部的远程仓库，将构件下载到
私服，再提供给本地仓库下载。Nexus是一个强大的Maven仓
库管理器，它极大地简化了本地内部仓库的维护和外部仓库的访问。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210126145518218.png)

```markdown
安装环境：

1 操作系统：Windows 10 /Windows 7

2 nexus版本：nexus-3.9.0-01-win64（Nexus 专业版是需要付
费的，这里我们下载开源免费版 Nexus OSS）

下载地址：https://www.sonatype.com/nexus-repository-oss
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210126145651315.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
解压过后，进入/nexus-3.9.1-01/bin目录下，以管理员身份打开
命令行：nexus /install ,该命令将会将Nexus Repository注册成为Windows服务。
运行nexus  /start 启动私服
打开浏览器，输出localhost:8081，出现如下界面，安装成功。

补充：
nexus /install # 安装私服
nexus  /start # 启动私服
nexus  /uninstall # 卸载私服
nexus /stop # 停止私服
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210126150114881.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
一些概念：
1.component name的一些说明： 
1）maven-central：maven中央库，该仓库代理Maven中央仓库，
默认从https://repo1.maven.org/maven2/拉取jar ，其策略为Release，
只会下载和缓存中央仓库中的发布版本构件。
2）maven-releases：私库发行版jar 
3）maven-snapshots：私库快照（调试版本）jar 
4）maven-public：仓库分组，把上面三个仓库组合在一起对外提供
服务，在本地maven基础配置settings.xml中使用。

 
2.Nexus默认的仓库类型有以下四种：
1）group(仓库组类型)：又叫组仓库，开发人员自己设定的仓库组；

2）hosted(宿主类型)：内部项目的发布仓库（内部开发人员，
发布上去存放的仓库）；

3）proxy(代理类型)：从远程中央仓库中寻找数据的仓库


3.Nexus仓库分类的概念：

1）Maven可直接从宿主仓库下载构件，也可以从代理仓库下载构件，
而代理仓库间接的从远程仓库下载并缓存构件

2）为了方便Maven可以从仓库组下载构件，仓库组并没有实际的内
容（下图中用虚线表示）,访问它时，它会转向包含的宿主仓库或者
代理仓库获得实际构件的内容).
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021012615052377.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
Nexus自带Jetty容器，默认的端口是8081，若想修改Nexus的服务端口，
可以在nexus-3.9.0-01-win64\sonatype-work\nexus3\etc\nexus.properties
配置中修改application-port=自己设置的端口号。

Nexus的工作目录是sonatype-work（路径一般在nexus同级目录下），
用户数据和设置都在这个目录下面，若要备份，备份这个目录即可；
```

```markdown
Nexus的web界面功能介绍
```

```markdown
Search
这个就是类似Maven仓库上的搜索功能，就是从私服上查找是否有哪些包。

在Search这级是支持模糊搜索的，如图所示：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210126151308134.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
Browse
这里查看所有的库以及库里面的组件
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210126151357851.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
Blob Stores
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210126151735985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
Repositories

Proxy就是代理的意思，代理中央Maven仓库，当PC访问中央
库的时候，先通过Proxy下载到Nexus仓库，然后再从Nexus仓库
下载到PC本地。
这样的优势只要其中一个人从中央库下来了，以后大家都是从Nexus私
服上进行下来，私服一般部署在内网，这样大大节约的宽带。
```
```markdown
创建Proxy的具体步骤
1 点击"Create Repositories"按钮
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210126152620262.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
2 选择要创建的类型
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210126152434873.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
3--填写详细信息
Name：就是为代理起个名字
Remote Storage: 代理的地址，Maven的地址为: https://repo1.maven.org/maven2/
Blob Store: 选择代理下载包的存放路径
```

```markdown
创建完成后如下图：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021012615234846.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
将添加的代理仓库加入 Public Repositories 仓库组
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210126152712470.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```xml
上传本地仓库的jar包到私服
上传步骤
修改本地maven配置文件
<servers>
	<server>
      <id>releases</id>
      <username>admin</username>
      <password>admin123</password>
    </server>
    <server>
      <id>snapshots</id>
      <username>admin</username>
      <password>admin123</password>
    </server>
 </servers>

在pom.xml添加如下配置
<distributionManagement>
        <repository>
            <id>releases</id>

            <url>http://192.168.1.101:8081/repository/maven-releases/</url>
        </repository>
        <snapshotRepository>
            <id>snapshots</id>

            <url>http://192.168.1.101:8081/repository/maven-snapshots/</url>
        </snapshotRepository>
    </distributionManagement>

注意：上面id需保持一致
url从copy按钮获得
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210126153526613.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)


```markdown
运行deploy
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210126153022144.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
从私服下载jar包
修改maven配置文件
<mirror>
      <id>test</id>
      <mirrorOf>central</mirrorOf>  # 固定写死
      <name>Nexus</name>
      <url>http://192.168.1.101:8081/repository/maven-public/</url> # 这个可以从copy按钮中获得。上面图有解释
</mirror>
```

```markdown
安装第三方jar包到本地仓库

进入jar包所在目录运行
mvn install:install-file -DgroupId=com.alibaba -DartifactId=fastjson -Dversion=1.1.37 -Dfile=fastjson-1.1.37.jar -Dpackaging=jar

或者打开cmd直接运行
mvn install:install-file -DgroupId=com.alibaba -DartifactId=fastjson -Dversion=1.1.37 -Dpackaging=jar -Dfile=C:\my_java\fastjson-1.1.37.jar




安装第三方jar包到私服
在settings配置文件中添加登录私服第三方登录信息
<server>
	<id>thirdparty</id>
	<username>admin</username>
	<password>admin123</password>
</server>

进入jar包所在目录运行
mvn deploy:deploy-file -DgroupId=com.alibaba -DartifactId=fastjson -Dversion=1.1.37 -Dpackaging=jar -Dfile=fastjson-1.1.37.jar -Durl=http://192.168.1.101:8081/repository/thridparty/ -DrepositoryId=thirdparty

或者打开cmd直接运行
mvn deploy:deploy-file -DgroupId=com.alibaba -DartifactId=fastjson -Dversion=1.1.37 -Dpackaging=jar -Dfile=C:\my_java\fastjson-1.1.37.jar -Durl=http://192.168.1.101:8081/repository/thridparty/ -DrepositoryId=thirdparty

其中-DgroupId 为上传的jar的groupId

-DartifactId 为上传的jar的artifactId

-Dversion 为上传的jar的需要被依赖的时候的版本号

-Dpackaging为jar，-Dfile为jar包路径

-Durl 为要上传的路径

-DrepositoryId 为repository的唯一标示，跟赋权配置的server相同
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复maven
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
