# Mybatis
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210717115759742.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1 Mybatis的引言
```markdown
Mybatis是个基于java语⾔的持久层的框架，主要用来简化数据库
的访问操作,内部封装了原来的jdbc代码,替换了原有项目开发中
的jdbc技术,他可以自动完成对象与关系的映射(ORM),极度简
化了我们的开发,提高开发效率。
```
## 2 Jdbc中存在问题
```markdown
1) 大量的代码冗余 (处理结果集的时候存在大量的冗余)
2) 不能完成数据库和实体的自动转换 (需要手动动封装实体,不
能自定封装实体类)
```

## 3 Mybatis的环境搭建
### 3.1 建表
```sql
DROP TABLE IF EXISTS `user`;

CREATE TABLE `user` (
  `id` int(11) NOT NULL auto_increment,
  `username` varchar(32) NOT NULL COMMENT '用户名称',
  `birthday` datetime default NULL COMMENT '生日',
  `sex` char(1) default NULL COMMENT '性别',
  `address` varchar(256) default NULL COMMENT '地址',
  PRIMARY KEY  (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;



insert  into `user`(`id`,`username`,`birthday`,`sex`,`address`) values (41,'老王','2018-02-27 17:47:08','男','北京'),(42,'小二王','2018-03-02 15:09:37','女','北京金燕龙'),(43,'小二王','2018-03-04 11:34:34','女','北京金燕龙'),(45,'传智播客','2018-03-04 12:04:06','男','北京金燕龙'),(46,'老王','2018-03-07 17:37:26','男','北京'),(48,'小马宝莉','2018-03-08 11:44:00','女','北京修正');
```
### 3.2 开发实体类

```markdown
User.java
```

```java
package com.yxj.domain;

import java.io.Serializable;
import java.util.Date;

/**
 
 * @Company http://www.yxj.com
 */
public class User implements Serializable{

    private Integer id;
    private String username;
    private Date birthday;
    private String sex;
    private String address;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", birthday=" + birthday +
                ", sex='" + sex + '\'' +
                ", address='" + address + '\'' +
                '}';
    }
}

```
### 3.3 开发DAO接口
```markdown
IUserDao.java
```

```java
package com.yxj.dao;

import com.yxj.domain.User;

import java.util.List;

/**
 
 * @Company http://www.yxj.com
 *
 * 用户的持久层接口
 */
public interface IUserDao {

    /**
     * 查询所有操作
     * @return
     */
    List<User> findAll();
}

```
### 3.4 编写mapper配置文件
```markdown
IUserDao.xml
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.yxj.dao.IUserDao">
    <!--配置查询所有-->
    <select id="findAll" resultType="com.yxj.domain.User">
        select * from user
    </select>
</mapper>
```
### 3.5 书写连接相关的配置
```markdown
SqlMapConfig.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!-- mybatis的主配置文件 -->
<configuration>
    <!-- 配置环境 -->
    <environments default="mysql">
        <!-- 配置mysql的环境-->
        <environment id="mysql">
            <!-- 配置事务的类型-->
            <transactionManager type="JDBC"></transactionManager>
            <!-- 配置数据源（连接池） -->
            <dataSource type="POOLED">
                <!-- 配置连接数据库的4个基本信息 -->
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/yxj_mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="1234"/>
            </dataSource>
        </environment>
    </environments>

    <!-- 指定映射配置文件的位置，映射配置文件指的是每个dao独立的配置文件 -->
    <mappers>
        <mapper resource="com/yxj/dao/IUserDao.xml"/>
    </mappers>
</configuration>
```



### 3.6 编写测试类
```markdown
MybatisTest.java
```
```java
package com.yxj.test;

import com.yxj.dao.IUserDao;
import com.yxj.domain.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.InputStream;
import java.util.List;

/**
 * @Company http://www.yxj.com
 * mybatis的入门案例
 */
public class MybatisTest {

    /**
     * 入门案例
     * @param args
     */
    public static void main(String[] args)throws Exception {
        //1.读取配置文件
        InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
        //2.创建SqlSessionFactory工厂
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory factory = builder.build(in);
        //3.使用工厂生产SqlSession对象
        SqlSession session = factory.openSession();
        //4.使用SqlSession创建Dao接口的代理对象
        IUserDao userDao = session.getMapper(IUserDao.class);
        //5.使用代理对象执行方法
        List<User> users = userDao.findAll();
        for(User user : users){
            System.out.println(user);
        }
        //6.释放资源
        session.close();
        in.close();
    }
}

```


## 4 Mybatis中查询
### 4.1 查询所有数据
![ 	](https://img-blog.csdnimg.cn/20201029161742424.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

### 4.2 多个参数的查询
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029161719596.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
## 5 Mybatis中的增删改
### 5.1 添加
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029161930122.png#pic_center)
### 5.2 修改
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029161849929.png#pic_center)
### 5.3 删除
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029161908719.png#pic_center)

### 5.4 代码例子
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
    <artifactId>day02_yxj_mybatisCRUD</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.5</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>

        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.12</version>
        </dependency>

        <dependency>
            <groupId>junyxj</groupId>
            <artifactId>junyxj</artifactId>
            <version>4.10</version>
        </dependency>
    </dependencies>
</project>
```
```markdown
log4j.properties
```

```properties
# Set root category prioryxjy to INFO and yxjs only appender to CONSOLE.
#log4j.rootCategory=INFO, CONSOLE            debug   info   warn error fatal
log4j.rootCategory=debug, CONSOLE, LOGFILE

# Set the enterprise logger category to FATAL and yxjs only appender to CONSOLE.
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
jdbcConfig.properties
```

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/yxj_mybatis
jdbc.username=root
jdbc.password=1234
```
```markdown
SqlMapConfig.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 配置properties
        可以在标签内部配置连接数据库的信息。也可以通过属性引用外部配置文件信息
        resource属性： 常用的
            用于指定配置文件的位置，是按照类路径的写法来写，并且必须存在于类路径下。
        url属性：
            是要求按照Url的写法来写地址
            URL：Uniform Resource Locator 统一资源定位符。它是可以唯一标识一个资源的位置。
            它的写法：
                http://localhost:8080/mybatisserver/demo1Servlet
                协议      主机     端口       URI

            URI:Uniform Resource Identifier 统一资源标识符。它是在应用中可以唯一定位一个资源的。
    -->
    <!-- <properties resource="jdbcConfig.properties">  </properties>-->
    <properties url="file:///D:/IdeaProjects/day02_yxj_01mybatisCRUD/src/main/resources/jdbcConfig.properties">
       <!-- <property name="driver" value="com.mysql.jdbc.Driver"></property>
        <property name="url" value="jdbc:mysql://localhost:3306/yxj_mybatis"></property>
        <property name="username" value="root"></property>
        <property name="password" value="1234"></property>-->
    </properties>

    <!--使用typeAliases配置别名，它只能配置domain中类的别名 -->
    <typeAliases>
        <!--typeAlias用于配置别名。type属性指定的是实体类全限定类名。alias属性指定别名，当指定了别名就再区分大小写 
        <typeAlias type="com.yxj.domain.User" alias="user"></typeAlias>-->

        <!-- 用于指定要配置别名的包，当指定之后，该包下的实体类都会注册别名，并且类名就是别名，不再区分大小写-->
        <package name="com.yxj.domain"></package>
    </typeAliases>

    <!--配置环境-->
    <environments default="mysql">
        <!-- 配置mysql的环境-->
        <environment id="mysql">
            <!-- 配置事务 -->
            <transactionManager type="JDBC"></transactionManager>

            <!--配置连接池-->
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"></property>
                <property name="url" value="${jdbc.url}"></property>
                <property name="username" value="${jdbc.username}"></property>
                <property name="password" value="${jdbc.password}"></property>
            </dataSource>
        </environment>
    </environments>
    <!-- 配置映射文件的位置 -->
    <mappers>
        <!--<mapper resource="com/yxj/dao/IUserDao.xml"></mapper>-->
        <!-- package标签是用于指定dao接口所在的包,当指定了之后就不需要在写mapper以及resource或者class了 -->
        <package name="com.yxj.dao"></package>
    </mappers>
</configuration>
```
```markdown
User.java
```

```java
package com.yxj.domain;

import java.io.Serializable;
import java.util.Date;

/**
 
 * @Company http://www.yxj.com
 */
public class User implements Serializable {

    private Integer userId;
    private String userName;
    private String userAddress;
    private String userSex;
    private Date userBirthday;

    public Integer getUserId() {
        return userId;
    }

    public void setUserId(Integer userId) {
        this.userId = userId;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getUserAddress() {
        return userAddress;
    }

    public void setUserAddress(String userAddress) {
        this.userAddress = userAddress;
    }

    public String getUserSex() {
        return userSex;
    }

    public void setUserSex(String userSex) {
        this.userSex = userSex;
    }

    public Date getUserBirthday() {
        return userBirthday;
    }

    public void setUserBirthday(Date userBirthday) {
        this.userBirthday = userBirthday;
    }

    @Override
    public String toString() {
        return "User{" +
                "userId=" + userId +
                ", userName='" + userName + '\'' +
                ", userAddress='" + userAddress + '\'' +
                ", userSex='" + userSex + '\'' +
                ", userBirthday=" + userBirthday +
                '}';
    }
}
```
```markdown
QueryVo.java
```
```java
package com.yxj.domain;

/**
 
 * @Company http://www.yxj.com
 */
public class QueryVo {

    private User user;

    public User getUser() {
        return user;
    }

    public void setUser(User user) {
        this.user = user;
    }
}
```
```markdown
IUserDao.java
```
```java
package com.yxj.dao;

import com.yxj.domain.QueryVo;
import com.yxj.domain.User;

import java.util.List;

/**
 
 * @Company http://www.yxj.com
 *
 * 用户的持久层接口
 */
public interface IUserDao {

    /**
     * 查询所有用户
     * @return
     */
    List<User> findAll();

    /**
     * 保存用户
     * @param user
     */
    void saveUser(User user);

    /**
     * 更新用户
     * @param user
     */
    void updateUser(User user);

    /**
     * 根据Id删除用户
     * @param userId
     */
    void deleteUser(Integer userId);

    /**
     * 根据id查询用户信息
     * @param userId
     * @return
     */
    User findById(Integer userId);

    /**
     * 根据名称模糊查询用户信息
     * @param username
     * @return
     */
    List<User> findByName(String username);

    /**
     * 查询总用户数
     * @return
     */
    int findTotal();

    /**
     * 根据queryVo中的条件查询用户
     * @param vo
     * @return
     */
    List<User> findUserByVo(QueryVo vo);
}

```
```markdown
IUserDao.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.yxj.dao.IUserDao">

    <!-- 配置 查询结果的列名和实体类的属性名的对应关系 -->
    <resultMap id="userMap" type="uSeR">
        <!-- 主键字段的对应 -->
        <id property="userId" column="id"></id>
        <!--非主键字段的对应-->
        <result property="userName" column="username"></result>
        <result property="userAddress" column="address"></result>
        <result property="userSex" column="sex"></result>
        <result property="userBirthday" column="birthday"></result>
    </resultMap>


    <!-- 查询所有 -->
    <select id="findAll" resultMap="userMap">
        <!--select id as userId,username as userName,address as userAddress,sex as userSex,birthday as userBirthday from user;-->
        select * from user;
    </select>

    <!-- 保存用户 -->
    <insert id="saveUser" parameterType="user">
        <!-- 配置插入操作后，获取插入数据的id -->
        <selectKey keyProperty="userId" keyColumn="id" resultType="int" order="AFTER">
            select last_insert_id();
        </selectKey>
        insert into user(username,address,sex,birthday)values(#{userName},#{userAddress},#{userSex},#{userBirthday});
    </insert>

    <!-- 更新用户 -->
    <update id="updateUser" parameterType="USER">
        update user set username=#{userName},address=#{userAddress},sex=#{userAex},birthday=#{userBirthday} where id=#{userId}
    </update>

    <!-- 删除用户-->
    <delete id="deleteUser" parameterType="java.lang.Integer">
        delete from user where id = #{uid}
    </delete>
    
    <!-- 根据id查询用户 -->
    <select id="findById" parameterType="INT" resultMap="userMap">
        select * from user where id = #{uid}
    </select>

    <!-- 根据名称模糊查询 -->
    <select id="findByName" parameterType="string" resultMap="userMap">
          select * from user where username like #{name} # 开发中用这一种，因为可以防止sql注入问题
        <!-- select * from user where username like '%${value}%'  # 如果是这一种。那么value固定不可以变--> 
  <!-- select * from user where username like concat('%',#{value},'%')-->
   </select>

    <!-- 获取用户的总记录条数 -->
    <select id="findTotal" resultType="int">
        select count(id) from user;
    </select>

    <!-- 根据q ueryVo的条件查询用户 -->
    <select id="findUserByVo" parameterType="com.yxj.domain.QueryVo" resultMap="userMap">
        select * from user where username like #{user.username}
    </select>
</mapper>
```
```markdown
MybatisTest.java
```
```java
package com.yxj.test;

import com.yxj.dao.IUserDao;
import com.yxj.domain.QueryVo;
import com.yxj.domain.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junyxj.After;
import org.junyxj.Before;
import org.junyxj.Test;

import java.io.InputStream;
import java.util.Date;
import java.util.List;

/**
 
 * @Company http://www.yxj.com
 *
 * 测试mybatis的crud操作
 */
public class MybatisTest {

    private InputStream in;
    private SqlSession sqlSession;
    private IUserDao userDao;

    @Before//用于在测试方法执行之前执行
    public void init()throws Exception{
        //1.读取配置文件，生成字节输入流
        in = Resources.getResourceAsStream("SqlMapConfig.xml");
        //2.获取SqlSessionFactory
        SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
        //3.获取SqlSession对象
        sqlSession = factory.openSession();
        //4.获取dao的代理对象
        userDao = sqlSession.getMapper(IUserDao.class);
    }

    @After//用于在测试方法执行之后执行
    public void destroy()throws Exception{
        //提交事务
        sqlSession.commit();
        //6.释放资源
        sqlSession.close();
        in.close();
    }

    /**
     * 测试查询所有
     */
    @Test
    public void testFindAll(){
        //5.执行查询所有方法
        List<User> users = userDao.findAll();
        for(User user : users){
            System.out.println(user);
        }

    }

    /**
     * 测试保存操作
     */
    @Test
    public void testSave(){
        User user = new User();
        user.setUserName("modify User property");
        user.setUserAddress("北京市顺义区");
        user.setUserSex("男");
        user.setUserBirthday(new Date());
        System.out.println("保存操作之前："+user);
        //5.执行保存方法
        userDao.saveUser(user);

        System.out.println("保存操作之后："+user);
    }

    /**
     * 测试更新操作
     */
    @Test
    public void testUpdate(){
        User user = new User();
        user.setUserId(50);
        user.setUserName("mybastis update user");
        user.setUserAddress("北京市顺义区");
        user.setUserSex("女");
        user.setUserBirthday(new Date());

        //5.执行保存方法
        userDao.updateUser(user);
    }

    /**
     * 测试删除操作
     */
    @Test
    public void testDelete(){
        //5.执行删除方法
        userDao.deleteUser(48);
    }

    /**
     * 测试删除操作
     */
    @Test
    public void testFindOne(){
        //5.执行查询一个方法
        User  user = userDao.findById(50);
        System.out.println(user);
    }

    /**
     * 测试模糊查询操作
     */
    @Test
    public void testFindByName(){
        //5.执行查询一个方法
        List<User> users = userDao.findByName("%王%");
//        List<User> users = userDao.findByName("王");
        for(User user : users){
            System.out.println(user);
        }
    }

    /**
     * 测试查询总记录条数
     */
    @Test
    public void testFindTotal(){
        //5.执行查询一个方法
        int count = userDao.findTotal();
        System.out.println(count);
    }


    /**
     * 测试使用QueryVo作为查询条件
     */
    @Test
    public void testFindByVo(){
        QueryVo vo = new QueryVo();
        User user = new User();
        user.setUserName("%王%");
        vo.setUser(user);
        //5.执行查询一个方法
        List<User> users = userDao.findUserByVo(vo);
        for(User u : users){
            System.out.println(u);
        }
    }
}

```
### 5.5 注解开发（增删查改）
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
    <artifactId>day04_yxj_03annotation_mybatis</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.5</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.12</version>
        </dependency>
        <dependency>
            <groupId>junyxj</groupId>
            <artifactId>junyxj</artifactId>
            <version>4.10</version>
        </dependency>
    </dependencies>
</project>
```
```markdown
jdbcConfig.properties
```
```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/yxj_mybatis
jdbc.username=root
jdbc.password=1234
```
```markdown
log4j.properties
```

```properties
# Set root category prioryxjy to INFO and yxjs only appender to CONSOLE.
#log4j.rootCategory=INFO, CONSOLE            debug   info   warn error fatal
log4j.rootCategory=debug, CONSOLE, LOGFILE

# Set the enterprise logger category to FATAL and yxjs only appender to CONSOLE.
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
SqlMapConfig.xml
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 引入外部配置文件-->
    <properties resource="jdbcConfig.properties"></properties>
    <!--配置别名-->
    <typeAliases>
        <package name="com.yxj.domain"></package>
    </typeAliases>
    <!-- 配置环境-->
    <environments default="mysql">
        <environment id="mysql">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"></property>
                <property name="url" value="${jdbc.url}"></property>
                <property name="username" value="${jdbc.username}"></property>
                <property name="password" value="${jdbc.password}"></property>
            </dataSource>
        </environment>
    </environments>
    <!-- 指定带有注解的dao接口所在位置 -->
    <mappers>
        <mapper class="com.yxj.dao.IUserDao"></mapper>
    </mappers>
</configuration>
```
```markdown
User.java
```
```java
package com.yxj.domain;

import java.io.Serializable;
import java.util.Date;

/**
 
 * @Company http://www.yxj.com
 */
public class User implements Serializable{

    private Integer id;
    private String username;
    private String address;
    private String sex;
    private Date birthday;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", address='" + address + '\'' +
                ", sex='" + sex + '\'' +
                ", birthday=" + birthday +
                '}';
    }
}

```
```markdown
IUserDao.java
```
```java
package com.yxj.dao;

import com.yxj.domain.User;
import org.apache.ibatis.annotations.Delete;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Select;
import org.apache.ibatis.annotations.Update;

import java.util.List;

/**
 
 * @Company http://www.yxj.com
 * 在mybatis中针对,CRUD一共有四个注解
 *  @Select @Insert @Update @Delete
 */
public interface IUserDao {

    /**
     * 查询所有用户
     * @return
     */
    @Select("select * from user")
    List<User> findAll();

    /**
     * 保存用户
     * @param user
     */
    @Insert("insert into user(username,address,sex,birthday)values(#{username},#{address},#{sex},#{birthday})")
    void saveUser(User user);

    /**
     * 更新用户
     * @param user
     */
    @Update("update user set username=#{username},sex=#{sex},birthday=#{birthday},address=#{address} where id=#{id}")
    void updateUser(User user);

    /**
     * 删除用户
     * @param userId
     */
    @Delete("delete from user where id=#{id} ")
    void deleteUser(Integer userId);

    /**
     * 根据id查询用户
     * @param userId
     * @return
     */
    @Select("select * from user  where id=#{id} ")
    User findById(Integer userId);

    /**
     * 根据用户名称模糊查询
     * @param username
     * @return
     */
//    @Select("select * from user where username like #{username} ")
    @Select("select * from user where username like '%${value}%' ")
    List<User> findUserByName(String username);

    /**
     * 查询总用户数量
     * @return
     */
    @Select("select count(*) from user ")
    int findTotalUser();
}

```
```markdown
AnnotationCRUDTest.java
```

```java
package com.yxj.test;

import com.yxj.dao.IUserDao;
import com.yxj.domain.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junyxj.After;
import org.junyxj.Before;
import org.junyxj.Test;

import java.io.InputStream;
import java.util.Date;
import java.util.List;

/**
 
 * @Company http://www.yxj.com
 */
public class AnnotationCRUDTest {
    private InputStream in;
    private SqlSessionFactory factory;
    private SqlSession session;
    private IUserDao userDao;

    @Before
    public  void init()throws Exception{
        in = Resources.getResourceAsStream("SqlMapConfig.xml");
        factory = new SqlSessionFactoryBuilder().build(in);
        session = factory.openSession();
        userDao = session.getMapper(IUserDao.class);
    }

    @After
    public  void destroy()throws  Exception{
        session.commit();
        session.close();
        in.close();
    }


    @Test
    public void testSave(){
        User user = new User();
        user.setUsername("mybatis annotation");
        user.setAddress("北京市昌平区");

        userDao.saveUser(user);
    }

    @Test
    public void testUpdate(){
        User user = new User();
        user.setId(57);
        user.setUsername("mybatis annotation update");
        user.setAddress("北京市海淀区");
        user.setSex("男");
        user.setBirthday(new Date());

        userDao.updateUser(user);
    }


    @Test
    public void testDelete(){
        userDao.deleteUser(51);
    }

    @Test
    public void testFindOne(){
        User user = userDao.findById(57);
        System.out.println(user);
    }


    @Test
    public  void testFindByName(){
//        List<User> users = userDao.findUserByName("%mybatis%");
        List<User> users = userDao.findUserByName("mybatis");
        for(User user : users){
            System.out.println(user);
        }
    }

    @Test
    public  void testFindTotal(){
        int total = userDao.findTotalUser();
        System.out.println(total);
    }
}

```



## 6 添加修改细节
```markdown
注意: Mybatis执行插入,修改时不允许出现空值,因此在插入修改
时,想要使某个列的值可以为空,必须加jdbcType属性
```
### 6.1 添加修改细节
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029163643137.png#pic_center)

### 6.2 执行添加时
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029163737712.png#pic_center)
## 7 全部的jdbcType属性
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029163836565.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
## 8 显示执行sql语句
```markdown
引入log4j.properties
```
```properties
log4j.rootLogger=DEBUG, Console //根⽇志
log4j.appender.Console=org.apache.log4j.ConsoleAppender //控制台展示
log4j.appender.Console.layout=org.apache.log4j.PatternLayout //打印⽅式
log4j.appender.Console.layout.ConversionPattern=%d [%t] %-5p [%c] - %m%n //⽇
志格式
log4j.logger.com.yxj.dao=DEBUG
注意:
1.log4j中存在两种日志。一种是项目中的根日志,二种种是项目中子日志
2.一般在项目中开启子日志即可
3. 日志级别: ERROR、WARN、INFO、DEBUG、TRACE
```

## 9 Mybatis中的别名     
```markdown
声明别名  
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029174652705.png#pic_center)
```markdown
使用别名 
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029174718969.png#pic_center)
## 10 MyBatis中动态Sql
### 10.1 sql复用标签
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029175308460.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 10.2 if标签动态拼接sql
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029175351486.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 10.3 where标签
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029175452413.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 10.4 choose when otherwise标签
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029175614937.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

### 10.5 set标签
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029175718983.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 10.6 trim标签
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029180609798.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

### 10.7 foreach遍历标签
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029180704242.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 10.8 代码例子
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
    <artifactId>day03_yxj_02dynamicSQL</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.5</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>

        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.12</version>
        </dependency>

        <dependency>
            <groupId>junyxj</groupId>
            <artifactId>junyxj</artifactId>
            <version>4.10</version>
        </dependency>
    </dependencies>

</project>
```
```markdown
SqlMapConfig.xml
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 配置properties-->
    <properties resource="jdbcConfig.properties"></properties>

    <!--使用typeAliases配置别名，它只能配置domain中类的别名 -->
    <typeAliases>
        <package name="com.yxj.domain"></package>
    </typeAliases>

    <!--配置环境-->
    <environments default="mysql">
        <!-- 配置mysql的环境-->
        <environment id="mysql">
            <!-- 配置事务 -->
            <transactionManager type="JDBC"></transactionManager>

            <!--配置连接池-->
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"></property>
                <property name="url" value="${jdbc.url}"></property>
                <property name="username" value="${jdbc.username}"></property>
                <property name="password" value="${jdbc.password}"></property>
            </dataSource>
        </environment>
    </environments>
    <!-- 配置映射文件的位置 -->
    <mappers>
        <package name="com.yxj.dao"></package>
    </mappers>
</configuration>
```
```markdown
log4j.properties
```
```java
# Set root category prioryxjy to INFO and yxjs only appender to CONSOLE.
#log4j.rootCategory=INFO, CONSOLE            debug   info   warn error fatal
log4j.rootCategory=debug, CONSOLE, LOGFILE

# Set the enterprise logger category to FATAL and yxjs only appender to CONSOLE.
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
jdbcConfig.properties
```

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/yxj_mybatis
jdbc.username=root
jdbc.password=1234
```
```markdown
User.java
```

```java
package com.yxj.domain;

import java.io.Serializable;
import java.util.Date;

/**
 
 * @Company http://www.yxj.com
 */
public class User implements Serializable {

    private Integer userId;
    private String userName;
    private String userAddress;
    private String userSex;
    private Date userBirthday;

    public Integer getUserId() {
        return userId;
    }

    public void setUserId(Integer userId) {
        this.userId = userId;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getUserAddress() {
        return userAddress;
    }

    public void setUserAddress(String userAddress) {
        this.userAddress = userAddress;
    }

    public String getUserSex() {
        return userSex;
    }

    public void setUserSex(String userSex) {
        this.userSex = userSex;
    }

    public Date getUserBirthday() {
        return userBirthday;
    }

    public void setUserBirthday(Date userBirthday) {
        this.userBirthday = userBirthday;
    }

    @Override
    public String toString() {
        return "User{" +
                "userId=" + userId +
                ", userName='" + userName + '\'' +
                ", userAddress='" + userAddress + '\'' +
                ", userSex='" + userSex + '\'' +
                ", userBirthday=" + userBirthday +
                '}';
    }
}

```
```markdown
QueryVo.java
```

```java
package com.yxj.domain;

import java.util.List;

/**
 
 * @Company http://www.yxj.com
 */
public class QueryVo {

    private User user;

    private List<Integer> ids;

    public User getUser() {
        return user;
    }

    public void setUser(User user) {
        this.user = user;
    }

    public List<Integer> getIds() {
        return ids;
    }

    public void setIds(List<Integer> ids) {
        this.ids = ids;
    }
}

```
```markdown
IUserDao.java
```

```java
package com.yxj.dao;

import com.yxj.domain.QueryVo;
import com.yxj.domain.User;

import java.util.List;

/**
 
 * @Company http://www.yxj.com
 *
 * 用户的持久层接口
 */
public interface IUserDao {

    /**
     * 查询所有用户
     * @return
     */
    List<User> findAll();


    /**
     * 根据id查询用户信息
     * @param userId
     * @return
     */
    User findById(Integer userId);

    /**
     * 根据名称模糊查询用户信息
     * @param username
     * @return
     */
    List<User> findByName(String username);


    /**
     * 根据queryVo中的条件查询用户
     * @param vo
     * @return
     */
    List<User> findUserByVo(QueryVo vo);

    /**
     * 根据传入参数条件
     * @param user 查询的条件：有可能有用户名，有可能有性别，也有可能有地址，还有可能是都有
     * @return
     */
    List<User> findUserByCondition(User user);

    /**
     * 根据queryvo中提供的id集合，查询用户信息
     * @param vo
     * @return
     */
    List<User> findUserInIds(QueryVo vo);
}

```
```markdown
IUserDao.xml
```

```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.yxj.dao.IUserDao">

    <!-- 配置 查询结果的列名和实体类的属性名的对应关系 -->
    <resultMap id="userMap" type="uSeR">
        <!-- 主键字段的对应 -->
        <id property="userId" column="id"></id>
        <!--非主键字段的对应-->
        <result property="userName" column="username"></result>
        <result property="userAddress" column="address"></result>
        <result property="userSex" column="sex"></result>
        <result property="userBirthday" column="birthday"></result>
    </resultMap>

    <!-- 了解的内容：抽取重复的sql语句-->
    <sql id="defaultUser">
        select * from user
    </sql>

    <!-- 查询所有 -->
    <select id="findAll" resultMap="userMap">
        <include refid="defaultUser"></include>
    </select>

    <!-- 根据id查询用户 -->
    <select id="findById" parameterType="INT" resultMap="userMap">
        select * from user where id = #{uid}
    </select>

    <!-- 根据名称模糊查询 -->
    <select id="findByName" parameterType="string" resultMap="userMap">
          select * from user where username like #{name}
   </select>

    <!-- 根据queryVo的条件查询用户 -->
    <select id="findUserByVo" parameterType="com.yxj.domain.QueryVo" resultMap="userMap">
        select * from user where username like #{user.userName}
    </select>

    <!-- 根据条件查询
    <select id="findUserByCondition" resultMap="userMap" parameterType="user">
        select * from user where 1=1
        <if test="userName != null">
          and username = #{userName}
        </if>
        <if test="userSex != null">
            and sex = #{userSex}
        </if>
    </select>-->

    <select id="findUserByCondition" resultMap="userMap" parameterType="user">
        select * from user
        <where>
            <if test="userName != null">
                and username = #{userName}
            </if>
            <if test="userSex != null">
                and sex = #{userSex}
            </if>
        </where>
    </select>

    <!-- 根据queryvo中的Id集合实现查询用户列表 -->
    <select id="findUserInIds" resultMap="userMap" parameterType="queryvo">
        <include refid="defaultUser"></include>
        <where>
            <if test="ids != null and ids.size()>0">
                <foreach collection="ids" open="and id in (" close=")" item="uid" separator=",">
                    #{uid}
                </foreach>
            </if>
        </where>
    </select>
</mapper>
```
## 11 Mybatis处理一对一关联关系

### 11.1 数据库
```sql
--一对一
--用户表
create table t_users(
	id varchar2(36) primary key,
	name varchar2(40),
	age Integer,
	bir date,
	cardId varchar2(36) references t_card(id)
);
--身份信息表
create table t_card(
	id  varchar2(36) primary key,
	no varchar2(18),
	address varchar2(100),
	fork varchar2(30)
);
------------------------------------------------
--测试数据
insert into t_card values('1','1','北京市海淀区','汉族');
insert into t_card values('2','2','北京市昌平区','满族');

insert into t_users values('1','小黑',23,sysdate,'1');
insert into t_users values('2','小三',23,sysdate,'2');

--连接查询语句
select * from t_users u left join t_card c on u.cardId = c.id
```
### 11.2 实体类
```java
package com.yxj.entity;

import java.util.Date;

public class User {
	
	private String id;
	private String name;
	private Integer age;
	private Date bir;
	
	private Card card;//关系属性  不要出现在toString方法里面  
	
	
	public Card getCard() {
		return card;
	}
	public void setCard(Card card) {
		this.card = card;
	}
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public Integer getAge() {
		return age;
	}
	public void setAge(Integer age) {
		this.age = age;
	}
	public Date getBir() {
		return bir;
	}
	public void setBir(Date bir) {
		this.bir = bir;
	}
	public User(String id, String name, Integer age, Date bir) {
		super();
		this.id = id;
		this.name = name;
		this.age = age;
		this.bir = bir;
	}
	public User() {
		super();
		// TODO Auto-generated constructor stub
	}
	@Override
	public String toString() {
		return "User [id=" + id + ", name=" + name + ", age=" + age + ", bir="
				+ bir + "]";
	}
	
	

}
```
```java
package com.yxj.entity;

public class Card {
	private String id;
	private String no;
	private String address;
	private String fork;
	
	
	private User user;//关系属性
	
	public User getUser() {
		return user;
	}
	public void setUser(User user) {
		this.user = user;
	}
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	public String getNo() {
		return no;
	}
	public void setNo(String no) {
		this.no = no;
	}
	public String getAddress() {
		return address;
	}
	public void setAddress(String address) {
		this.address = address;
	}
	public String getFork() {
		return fork;
	}
	public void setFork(String fork) {
		this.fork = fork;
	}
	public Card() {
		super();
		// TODO Auto-generated constructor stub
	}
	public Card(String id, String no, String address, String fork) {
		super();
		this.id = id;
		this.no = no;
		this.address = address;
		this.fork = fork;
	}
	@Override
	public String toString() {
		return "Card [id=" + id + ", no=" + no + ", address=" + address
				+ ", fork=" + fork + "]";
	}
	
}
```
### 11.3 根据用户查找用户的身份信息
```java
package com.yxj.dao;

import java.util.List;

import com.yxj.entity.User;

public interface UserDAO {
	
	//查询用户  并且查询用户的身份信息
	public List<User> queryAll();
	
}
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.yxj.dao.UserDAO">
	<!-- 
		处理关联关系时  resultType无法将关系属性进行自动封装     只能处理单表简单类型(对象)
		
		处理关系属性赋值 :使用resultMap
			type:封装对象类型  如果是一个 直接写对象的全名(别名)  如果是多个 同样写泛型的类型
			id : resultMap的名字
	 -->
	 
	 
	 <resultMap type="com.yxj.entity.User" id="aaa">
	 	<!-- 用来封装user中的主键的值 column 数据库返回的列名   property 类中属性名 -->
	 	<id column="userId" property="id"/>
	 	<!-- 用来封装user中的普通属性值 -->
	 	<result column="name" property="name"/>
	 	<result column="age" property="age"/>
	 	<result column="bir" property="bir"/>
	 	<!-- 关系属性封装 一对一  对象类型
	 		association: 用来处理一对一关系属性封装
	 		property : 关系属性名
	 		javaType: 关系属性的类型
	 	 -->
	 	<association property="card" javaType="com.yxj.entity.Card" >
	 		<id column="cardId" property="id" />
	 		<result column="no" property="no"/>
	 		<result column="address" property="address"/>
	 		<result column="fork" property="fork"/>
	 	</association>
	 </resultMap>

	<!-- 
		resultMap: 封装对象时使用外部的哪个resultMap进行封装  书写resultMap标签的id属性
	 -->
	<select id="queryAll" resultMap="aaa">
		select u.id userId,u.name,u.age,u.bir,
		       c.id cardId,c.no,c.fork,c.address 
		from t_users u 
		left join t_card c 
		on  u.cardId = c.id
	</select>
	
	
	
  
</mapper>
```
```java
package com.yxj.dao;

import java.util.List;

import com.yxj.entity.Card;

public interface CardDAO {
	
	public List<Card> queryAll();
	
}
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.yxj.dao.CardDAO">
	
	
	
	<resultMap type="com.yxj.entity.Card" id="cardMap">
		<id column="cardId" property="id"/>
		<result column="no" property="no"/>
		<result column="address" property="address"/>
		<result column="fork" property="fork"/>
		<!-- 处理一对一 -->
		<association property="user" javaType="com.yxj.entity.User">
			<id column="userId" property="id"/>
			<result column="name" property="name"/>
			<result column="age" property="age"/>
			<result column="bir" property="bir"/>
		</association>
	</resultMap>
	
	
	<select id="queryAll" resultMap="cardMap">
		select u.id userId,u.name,u.age,u.bir,
	       c.id cardId,c.no,c.fork,c.address 
		from t_users u 
		left join t_card c 
		on  u.cardId = c.id
	</select> 
</mapper>
```
## 12 Mybatis处理一对多关联关系
### 12.1 数据库模型
```sql
--处理一对多
--部门表
create table t_dept(
	id varchar2(36) primary key,
	name varchar2(40)
);
--员工表
create table t_emps(
	id varchar2(36) primary key,
	name varchar2(40),
	age Integer ,
	bir date,
	salary number(7,2),
	deptId varchar2(36) references t_dept(id)
);
--部门测试数据
insert into t_dept values('1','教学部');
insert into t_dept values('2','研发部');
--员工的测试数据
insert into t_emps values('1','小黑',12,sysdate,23000.12,'1');
insert into t_emps values('2','小三',12,sysdate,23000.12,'2');
insert into t_emps values('3','小黄',12,sysdate,23000.12,'1');
insert into t_emps values('4','小牛',12,sysdate,23000.12,'1');
insert into t_emps values('5','小金',12,sysdate,23000.12,'1');
insert into t_emps values('6','小陈',12,sysdate,23000.12,'2');

--根据部门找员工信息
select * from t_dept d left join t_emps e on d.id  = e.deptId where d.id = '1'
--
```
### 12.2 实体类实现
```java
package com.yxj.entity;

import java.util.Date;

public class Emp {
	private String id;
	private String name;
	private Integer age;
	private Double salary;
	private Date  bir;
	
	private Dept dept;//关系属性
	
	
	public Dept getDept() {
		return dept;
	}
	public void setDept(Dept dept) {
		this.dept = dept;
	}
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public Integer getAge() {
		return age;
	}
	public void setAge(Integer age) {
		this.age = age;
	}
	public Double getSalary() {
		return salary;
	}
	public void setSalary(Double salary) {
		this.salary = salary;
	}
	public Date getBir() {
		return bir;
	}
	public void setBir(Date bir) {
		this.bir = bir;
	}
	public Emp() {
		super();
		// TODO Auto-generated constructor stub
	}
	public Emp(String id, String name, Integer age, Double salary, Date bir) {
		super();
		this.id = id;
		this.name = name;
		this.age = age;
		this.salary = salary;
		this.bir = bir;
	}
	@Override
	public String toString() {
		return "Emp [id=" + id + ", name=" + name + ", age=" + age
				+ ", salary=" + salary + ", bir=" + bir + "]";
	}
	
}

```
```java
package com.yxj.entity;

import java.util.List;

public class Dept {
	private String id;
	private String name;
	
	//关系属性
	private List<Emp> emps;
	
	public List<Emp> getEmps() {
		return emps;
	}
	public void setEmps(List<Emp> emps) {
		this.emps = emps;
	}
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public Dept() {
		super();
		// TODO Auto-generated constructor stub
	}
	public Dept(String id, String name) {
		super();
		this.id = id;
		this.name = name;
	}
	@Override
	public String toString() {
		return "Dept [id=" + id + ", name=" + name + "]";
	}
	
}

```


### 12.3 通过部门找员工
```java
package com.yxj.dao;

import java.util.List;

import com.yxj.entity.Dept;

public interface DeptDAO {
	
	//查询部门的同时将部门中所有的员工信息一并查到
	public List<Dept> queryAll();
	
}

```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.yxj.dao.DeptDAO">


	<resultMap type="com.yxj.entity.Dept" id="deptMap">
		<id column="id" property="id"/>
		<result column="name" property="name"/>
		<!-- 处理关系属性的封装
			collection 用来处理封装集合类型的管理属性   用来处理一对多这种情况
			property:  关系属性名 
			javaType:  关系属性类型
			ofType  :  集合中泛型类型  全名
		 -->
		<collection property="emps" javaType="list" ofType="com.yxj.entity.Emp">
			<id column="eid" property="id"/>
			<result  column="ename" property="name"/>
			<result column="age" property="age"/>
			<result column="salary" property="salary"/>
			<result column="bir" property="bir"/>
		</collection>
	</resultMap>

	<select id="queryAll" resultMap="deptMap">
		select d.id,d.name,e.id eid,e.name ename,e.age,e.salary,e.bir from t_dept d left join t_emps e on d.id  = e.deptId
	</select>
  
</mapper>

```



### 12.4 通过员工找部门
```java
package com.yxj.dao;

import java.util.List;

import com.yxj.entity.Emp;

public interface EmpDAO {
	
	//查询员工信息的同时将员工的部门信息获取
	public List<Emp> queryAll();
	
}

```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.yxj.dao.EmpDAO">


	<resultMap type="com.yxj.entity.Emp" id="empMap">
		<id column="eid" property="id"/>
		<result  column="ename" property="name"/>
		<result column="age" property="age"/>
		<result column="salary" property="salary"/>
		<result column="bir" property="bir"/>
		<!-- 封装关系属性  一对一 -->
		<association property="dept" javaType="com.yxj.entity.Dept">
			<id column="id" property="id"/>
			<result column="name" property="name"/>
		</association>
	</resultMap>



	<select id="queryAll" resultMap="empMap">
		select d.id,d.name,e.id eid,e.name ename,e.age,e.salary,e.bir from t_dept d left join t_emps e on d.id  = e.deptId
	</select>


  
</mapper>
```
### 12.5 代码例子
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
    <artifactId>day03_yxj_03one2many</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.5</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>

        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.12</version>
        </dependency>

        <dependency>
            <groupId>junyxj</groupId>
            <artifactId>junyxj</artifactId>
            <version>4.10</version>
        </dependency>
    </dependencies>


</project>
```
```markdown
log4j.properties
```

```properties
# Set root category prioryxjy to INFO and yxjs only appender to CONSOLE.
#log4j.rootCategory=INFO, CONSOLE            debug   info   warn error fatal
log4j.rootCategory=debug, CONSOLE, LOGFILE

# Set the enterprise logger category to FATAL and yxjs only appender to CONSOLE.
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
jdbcConfig.properties
```

```.properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/yxj_mybatis
jdbc.username=root
jdbc.password=1234
```
```markdown
SqlMapConfig.xml
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 配置properties-->
    <properties resource="jdbcConfig.properties"></properties>

    <!--使用typeAliases配置别名，它只能配置domain中类的别名 -->
    <typeAliases>
        <package name="com.yxj.domain"></package>
    </typeAliases>

    <!--配置环境-->
    <environments default="mysql">
        <!-- 配置mysql的环境-->
        <environment id="mysql">
            <!-- 配置事务 -->
            <transactionManager type="JDBC"></transactionManager>

            <!--配置连接池-->
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"></property>
                <property name="url" value="${jdbc.url}"></property>
                <property name="username" value="${jdbc.username}"></property>
                <property name="password" value="${jdbc.password}"></property>
            </dataSource>
        </environment>
    </environments>
    <!-- 配置映射文件的位置 -->
    <mappers>
        <package name="com.yxj.dao"></package>
    </mappers>
</configuration>
```
```markdown
User.java
```

```java
package com.yxj.domain;

import java.io.Serializable;
import java.util.Date;
import java.util.List;

/**
 
 * @Company http://www.yxj.com
 */
public class User implements Serializable {

    private Integer id;
    private String username;
    private String address;
    private String sex;
    private Date birthday;

    //一对多关系映射：主表实体应该包含从表实体的集合引用
    private List<Account> accounts;

    public List<Account> getAccounts() {
        return accounts;
    }

    public void setAccounts(List<Account> accounts) {
        this.accounts = accounts;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", address='" + address + '\'' +
                ", sex='" + sex + '\'' +
                ", birthday=" + birthday +
                '}';
    }
}

```
```markdown
Account.java
```

```java
package com.yxj.domain;

import java.io.Serializable;

/**
 
 * @Company http://www.yxj.com
 */
public class Account implements Serializable {

    private Integer id;
    private Integer uid;
    private Double money;

    //从表实体应该包含一个主表实体的对象引用
    private User user;

    public User getUser() {
        return user;
    }

    public void setUser(User user) {
        this.user = user;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public Integer getUid() {
        return uid;
    }

    public void setUid(Integer uid) {
        this.uid = uid;
    }

    public Double getMoney() {
        return money;
    }

    public void setMoney(Double money) {
        this.money = money;
    }

    @Override
    public String toString() {
        return "Account{" +
                "id=" + id +
                ", uid=" + uid +
                ", money=" + money +
                '}';
    }
}

```
```markdown
AccountUser.java
```

```java
package com.yxj.domain;

/**
 
 * @Company http://www.yxj.com
 */
public class AccountUser extends Account {

    private String username;
    private String address;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return super.toString()+"        AccountUser{" +
                "username='" + username + '\'' +
                ", address='" + address + '\'' +
                '}';
    }
}

```
```markdown
IUserDao.java
```

```java
package com.yxj.dao;

import com.yxj.domain.User;

import java.util.List;

/**
 
 * @Company http://www.yxj.com
 *
 * 用户的持久层接口
 */
public interface IUserDao {

    /**
     * 查询所有用户，同时获取到用户下所有账户的信息
     * @return
     */
    List<User> findAll();


    /**
     * 根据id查询用户信息
     * @param userId
     * @return
     */
    User findById(Integer userId);


}

```
```markdown
IUserDao.xml
```

```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.yxj.dao.IUserDao">

    <!-- 定义User的resultMap-->
    <resultMap id="userAccountMap" type="user">
        <id property="id" column="id"></id>
        <result property="username" column="username"></result>
        <result property="address" column="address"></result>
        <result property="sex" column="sex"></result>
        <result property="birthday" column="birthday"></result>
        <!-- 配置user对象中accounts集合的映射 -->
        <collection property="accounts" ofType="account">
            <id column="aid" property="id"></id>
            <result column="uid" property="uid"></result>
            <result column="money" property="money"></result>
        </collection>
    </resultMap>

    <!-- 查询所有 -->
    <select id="findAll" resultMap="userAccountMap">
        select * from user u left outer join account a on u.id = a.uid
    </select>

    <!-- 根据id查询用户 -->
    <select id="findById" parameterType="INT" resultType="user">
        select * from user where id = #{uid}
    </select>

</mapper>
```
```markdown
IAccountDao.java
```

```java
package com.yxj.dao;

import com.yxj.domain.Account;
import com.yxj.domain.AccountUser;

import java.util.List;

/**
 
 * @Company http://www.yxj.com
 */
public interface IAccountDao {

    /**
     * 查询所有账户，同时还要获取到当前账户的所属用户信息
     * @return
     */
    List<Account> findAll();

    /**
     * 查询所有账户，并且带有用户名称和地址信息
     * @return
     */
    List<AccountUser> findAllAccount();
}

```
```markdown
IAccountDao.xml
```

```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.yxj.dao.IAccountDao">

    <!-- 定义封装account和user的resultMap -->
    <resultMap id="accountUserMap" type="account">
        <id property="id" column="aid"></id>
        <result property="uid" column="uid"></result>
        <result property="money" column="money"></result>
        <!-- 一对一的关系映射：配置封装user的内容-->
        <association property="user" column="uid" javaType="user">
            <id property="id" column="id"></id>
            <result column="username" property="username"></result>
            <result column="address" property="address"></result>
            <result column="sex" property="sex"></result>
            <result column="birthday" property="birthday"></result>
        </association>
    </resultMap>

    <!-- 查询所有 -->
    <select id="findAll" resultMap="accountUserMap">
        select u.*,a.id as aid,a.uid,a.money from account a , user u where u.id = a.uid;
    </select>

    <!--查询所有账户同时包含用户名和地址信息-->
    <select id="findAllAccount" resultType="accountuser">
        select a.*,u.username,u.address from account a , user u where u.id = a.uid;
    </select>

</mapper>
```
```markdown
UserTest.java
```

```java
package com.yxj.test;

import com.yxj.dao.IUserDao;
import com.yxj.domain.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junyxj.After;
import org.junyxj.Before;
import org.junyxj.Test;

import java.io.InputStream;
import java.util.List;

/**
 
 * @Company http://www.yxj.com
 */
public class UserTest {

    private InputStream in;
    private SqlSession sqlSession;
    private IUserDao userDao;

    @Before//用于在测试方法执行之前执行
    public void init()throws Exception{
        //1.读取配置文件，生成字节输入流
        in = Resources.getResourceAsStream("SqlMapConfig.xml");
        //2.获取SqlSessionFactory
        SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
        //3.获取SqlSession对象
        sqlSession = factory.openSession(true);
        //4.获取dao的代理对象
        userDao = sqlSession.getMapper(IUserDao.class);
    }

    @After//用于在测试方法执行之后执行
    public void destroy()throws Exception{
        //提交事务
        // sqlSession.commit();
        //6.释放资源
        sqlSession.close();
        in.close();
    }

    /**
     * 测试查询所有
     */
    @Test
    public void testFindAll(){
        List<User> users = userDao.findAll();
        for(User user : users){
            System.out.println("-----每个用户的信息------");
            System.out.println(user);
            System.out.println(user.getAccounts());
        }
    }



}

```
```markdown
AccountTest.java
```

```java
package com.yxj.test;

import com.yxj.dao.IAccountDao;
import com.yxj.domain.Account;
import com.yxj.domain.AccountUser;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junyxj.After;
import org.junyxj.Before;
import org.junyxj.Test;

import java.io.InputStream;
import java.util.List;

/**
 
 * @Company http://www.yxj.com
 */
public class AccountTest {

    private InputStream in;
    private SqlSession sqlSession;
    private IAccountDao accountDao;

    @Before//用于在测试方法执行之前执行
    public void init()throws Exception{
        //1.读取配置文件，生成字节输入流
        in = Resources.getResourceAsStream("SqlMapConfig.xml");
        //2.获取SqlSessionFactory
        SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
        //3.获取SqlSession对象
        sqlSession = factory.openSession(true);
        //4.获取dao的代理对象
        accountDao = sqlSession.getMapper(IAccountDao.class);
    }

    @After//用于在测试方法执行之后执行
    public void destroy()throws Exception{
        //提交事务
        // sqlSession.commit();
        //6.释放资源
        sqlSession.close();
        in.close();
    }

    /**
     * 测试查询所有
     */
    @Test
    public void testFindAll(){
        List<Account> accounts = accountDao.findAll();
        for(Account account : accounts){
            System.out.println("--------每个account的信息------------");
            System.out.println(account);
            System.out.println(account.getUser());
        }
    }


    /**
     * 测试查询所有账户，同时包含用户名称和地址
     */
    @Test
    public void testFindAllAccountUser(){
        List<AccountUser> aus = accountDao.findAllAccount();
        for(AccountUser au : aus){
            System.out.println(au);
        }
    }
}

```
### 12.6 注解开发
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
    <artifactId>day04_yxj_04annoOne2Many</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.5</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.12</version>
        </dependency>
        <dependency>
            <groupId>junyxj</groupId>
            <artifactId>junyxj</artifactId>
            <version>4.10</version>
        </dependency>
    </dependencies>

</project>
```
```markdown
jdbcConfig.properties
```

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/yxj_mybatis
jdbc.username=root
jdbc.password=1234
```
```markdown
SqlMapConfig.xml
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 引入外部配置文件-->
    <properties resource="jdbcConfig.properties"></properties>
    <!--配置开启二级缓存-->
    <settings>
        <setting name="cacheEnabled" value="true"/>
    </settings>

    <!--配置别名-->
    <typeAliases>
        <package name="com.yxj.domain"></package>
    </typeAliases>
    <!-- 配置环境-->
    <environments default="mysql">
        <environment id="mysql">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"></property>
                <property name="url" value="${jdbc.url}"></property>
                <property name="username" value="${jdbc.username}"></property>
                <property name="password" value="${jdbc.password}"></property>
            </dataSource>
        </environment>
    </environments>
    <!-- 指定带有注解的dao接口所在位置 -->
    <mappers>
       <package name="com.yxj.dao"></package>
    </mappers>
</configuration>
```
```markdown
log4j.properties
```

```properties
# Set root category prioryxjy to INFO and yxjs only appender to CONSOLE.
#log4j.rootCategory=INFO, CONSOLE            debug   info   warn error fatal
log4j.rootCategory=debug, CONSOLE, LOGFILE

# Set the enterprise logger category to FATAL and yxjs only appender to CONSOLE.
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
Account.java
```

```java
package com.yxj.domain;

import java.io.Serializable;

/**
 
 * @Company http://www.yxj.com
 */
public class Account implements Serializable {

    private Integer id;
    private Integer uid;
    private Double money;

    //多对一（mybatis中称之为一对一）的映射：一个账户只能属于一个用户
    private User user;

    public User getUser() {
        return user;
    }

    public void setUser(User user) {
        this.user = user;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public Integer getUid() {
        return uid;
    }

    public void setUid(Integer uid) {
        this.uid = uid;
    }

    public Double getMoney() {
        return money;
    }

    public void setMoney(Double money) {
        this.money = money;
    }

    @Override
    public String toString() {
        return "Account{" +
                "id=" + id +
                ", uid=" + uid +
                ", money=" + money +
                '}';
    }
}

```
```markdown
User.java
```
```java
package com.yxj.domain;

import java.io.Serializable;
import java.util.Date;
import java.util.List;

/**
 
 * @Company http://www.yxj.com
 */
public class User implements Serializable{

    private Integer userId;
    private String userName;
    private String userAddress;
    private String userSex;
    private Date userBirthday;

    //一对多关系映射：一个用户对应多个账户
    private List<Account> accounts;

    public List<Account> getAccounts() {
        return accounts;
    }

    public void setAccounts(List<Account> accounts) {
        this.accounts = accounts;
    }

    public Integer getUserId() {
        return userId;
    }

    public void setUserId(Integer userId) {
        this.userId = userId;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getUserAddress() {
        return userAddress;
    }

    public void setUserAddress(String userAddress) {
        this.userAddress = userAddress;
    }

    public String getUserSex() {
        return userSex;
    }

    public void setUserSex(String userSex) {
        this.userSex = userSex;
    }

    public Date getUserBirthday() {
        return userBirthday;
    }

    public void setUserBirthday(Date userBirthday) {
        this.userBirthday = userBirthday;
    }

    @Override
    public String toString() {
        return "User{" +
                "userId=" + userId +
                ", userName='" + userName + '\'' +
                ", userAddress='" + userAddress + '\'' +
                ", userSex='" + userSex + '\'' +
                ", userBirthday=" + userBirthday +
                '}';
    }
}

```
```markdown
IUserDao.java
```
```java
package com.yxj.dao;

import com.yxj.domain.User;
import org.apache.ibatis.annotations.*;
import org.apache.ibatis.mapping.FetchType;

import java.util.List;

/**
 
 * @Company http://www.yxj.com
 * 在mybatis中针对,CRUD一共有四个注解
 *  @Select @Insert @Update @Delete
 */
@CacheNamespace(blocking = true)
public interface IUserDao {

    /**
     * 查询所有用户
     * @return
     */
    @Select("select * from user")
    @Results(id="userMap",value={
            @Result(id=true,column = "id",property = "userId"),
            @Result(column = "username",property = "userName"),
            @Result(column = "address",property = "userAddress"),
            @Result(column = "sex",property = "userSex"),
            @Result(column = "birthday",property = "userBirthday"),
            @Result(property = "accounts",column = "id",
                    many = @Many(select = "com.yxj.dao.IAccountDao.findAccountByUid",
                                fetchType = FetchType.LAZY))
    })
    List<User> findAll();

    /**
     * 根据id查询用户
     * @param userId
     * @return
     */
    @Select("select * from user  where id=#{id} ")
    @ResultMap("userMap")
    User findById(Integer userId);

    /**
     * 根据用户名称模糊查询
     * @param username
     * @return
     */
    @Select("select * from user where username like #{username} ")
    @ResultMap("userMap")
    List<User> findUserByName(String username);


}

```
```markdown
IAccountDao.java
```

```java
package com.yxj.dao;

import com.yxj.domain.Account;
import org.apache.ibatis.annotations.One;
import org.apache.ibatis.annotations.Result;
import org.apache.ibatis.annotations.Results;
import org.apache.ibatis.annotations.Select;
import org.apache.ibatis.mapping.FetchType;

import java.util.List;

/**
 
 * @Company http://www.yxj.com
 */
public interface IAccountDao {

    /**
     * 查询所有账户，并且获取每个账户所属的用户信息
     * @return
     */
    @Select("select * from account")
    @Results(id="accountMap",value = {
            @Result(id=true,column = "id",property = "id"),
            @Result(column = "uid",property = "uid"),
            @Result(column = "money",property = "money"),
            @Result(property = "user",column = "uid",one=@One(select="com.yxj.dao.IUserDao.findById",fetchType= FetchType.EAGER))
    })
    List<Account> findAll();

    /**
     * 根据用户id查询账户信息
     * @param userId
     * @return
     */
    @Select("select * from account where uid = #{userId}")
    List<Account> findAccountByUid(Integer userId);
}
```

```markdown
AccountTest.java
```
```java
package com.yxj.test;

import com.yxj.dao.IAccountDao;
import com.yxj.domain.Account;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junyxj.After;
import org.junyxj.Before;
import org.junyxj.Test;

import java.io.InputStream;
import java.util.List;

/**
 
 * @Company http://www.yxj.com
 */
public class AccountTest {
    private InputStream in;
    private SqlSessionFactory factory;
    private SqlSession session;
    private IAccountDao accountDao;

    @Before
    public  void init()throws Exception{
        in = Resources.getResourceAsStream("SqlMapConfig.xml");
        factory = new SqlSessionFactoryBuilder().build(in);
        session = factory.openSession();
        accountDao = session.getMapper(IAccountDao.class);
    }

    @After
    public  void destroy()throws  Exception{
        session.commit();
        session.close();
        in.close();
    }

    @Test
    public  void  testFindAll(){
        List<Account> accounts = accountDao.findAll();
        for(Account account : accounts){
            System.out.println("----每个账户的信息-----");
            System.out.println(account);
            System.out.println(account.getUser());
        }
    }



}


```
```markdown
AnnotationCRUDTest.java
```
```java
package com.yxj.test;

import com.yxj.dao.IUserDao;
import com.yxj.domain.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junyxj.After;
import org.junyxj.Before;
import org.junyxj.Test;

import java.io.InputStream;
import java.util.List;

/**
 
 * @Company http://www.yxj.com
 */
public class AnnotationCRUDTest {
    private InputStream in;
    private SqlSessionFactory factory;
    private SqlSession session;
    private IUserDao userDao;

    @Before
    public  void init()throws Exception{
        in = Resources.getResourceAsStream("SqlMapConfig.xml");
        factory = new SqlSessionFactoryBuilder().build(in);
        session = factory.openSession();
        userDao = session.getMapper(IUserDao.class);
    }

    @After
    public  void destroy()throws  Exception{
        session.commit();
        session.close();
        in.close();
    }

    @Test
    public  void  testFindAll(){
        List<User> users = userDao.findAll();
//        for(User user : users){
//            System.out.println("---每个用户的信息----");
//            System.out.println(user);
//            System.out.println(user.getAccounts());
//        }
    }

    @Test
    public void testFindOne(){
        User user = userDao.findById(57);
        System.out.println(user);
    }


    @Test
    public  void testFindByName(){
        List<User> users = userDao.findUserByName("%mybatis%");
        for(User user : users){
            System.out.println(user);
        }
    }


}

```
```markdown
SecondLevelCatchTest.java
```
```java
package com.yxj.test;

import com.yxj.dao.IUserDao;
import com.yxj.domain.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junyxj.After;
import org.junyxj.Before;
import org.junyxj.Test;

import java.io.InputStream;

/**
 
 * @Company http://www.yxj.com
 */
public class SecondLevelCatchTest {

    private InputStream in;
    private SqlSessionFactory factory;


    @Before
    public  void init()throws Exception{
        in = Resources.getResourceAsStream("SqlMapConfig.xml");
        factory = new SqlSessionFactoryBuilder().build(in);

    }

    @After
    public  void destroy()throws  Exception{

        in.close();
    }

    @Test
    public void testFindOne(){
        SqlSession session = factory.openSession();
        IUserDao userDao = session.getMapper(IUserDao.class);
        User user = userDao.findById(57);
        System.out.println(user);

        session.close();//释放一级缓存

        SqlSession session1 = factory.openSession();//再次打开session
        IUserDao userDao1 = session1.getMapper(IUserDao.class);
        User user1 = userDao1.findById(57);
        System.out.println(user1);


        session1.close();

    }
}
```


## 13 Mybatis处理多对多关联关系
```markdown
多对多在数据库的设计里也是使用两个一对多实现的
实体类在开发时两边都是集合类型的关系属性
mapper文件在维护时都使用 <collection 标签>
```
### 13.1 数据库模型
```sql
--多对多
--学生表
create table t_student(
	id varchar2(36) primary key,
	name varchar2(40),
	age Integer 
);

--课程表
create table t_course(
	id varchar2(36) primary key,
	name varchar2(40)
);

--学生选课表  关系表   多的一方
create table t_student_course(
	sid varchar2(36) references t_student(id),--学生主键
	cid varchar2(36) references t_course(id), --课程主键
	primary key (sid,cid)
);
drop table t_course;
--测试数据
insert into t_student values ('11','小明',23);
insert into t_student values ('22','小王',23);

insert into t_course  values ('1','语文');
insert into t_course  values ('2','数学');
insert into t_course  values ('3','政治');


insert into t_student_course values('11','1');
insert into t_student_course values('11','2');
insert into t_student_course values('11','3');

insert into t_student_course values('22','1');
insert into t_student_course values('22','3');


select s.id,s.name,s.age,c.id cid,c.name cname from t_student s 
         left join t_student_course sc 
         on s.id = sc.sid
         left join t_course c
         on sc.cid = c.id
```
### 13.2 实体类实现
```java
package com.yxj.entity;

import java.util.List;

public class Student {
	
	private String id;
	private String name;
	private Integer age;
	
	private List<Course> courses;//关系属性   用来表示一个学生选了多个课程
	
	
	
	
	public List<Course> getCourses() {
		return courses;
	}
	public void setCourses(List<Course> courses) {
		this.courses = courses;
	}
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public Integer getAge() {
		return age;
	}
	public void setAge(Integer age) {
		this.age = age;
	}
	public Student() {
		super();
		// TODO Auto-generated constructor stub
	}
	public Student(String id, String name, Integer age) {
		super();
		this.id = id;
		this.name = name;
		this.age = age;
	}
	@Override
	public String toString() {
		return "Student [id=" + id + ", name=" + name + ", age=" + age + "]";
	}
	
	

}
```
```java
package com.yxj.entity;

public class Course {
	
	private String id;
	private String name;
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public Course() {
		super();
		// TODO Auto-generated constructor stub
	}
	public Course(String id, String name) {
		super();
		this.id = id;
		this.name = name;
	}
	@Override
	public String toString() {
		return "Course [id=" + id + ", name=" + name + "]";
	}
	

}

```
### 13.3 代码实现
```java
package com.yxj.dao;

import java.util.List;

import com.yxj.entity.Student;

public interface StudentDAO {
	
	public List<Student> queryAll();
	
}

```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.yxj.dao.StudentDAO">


	<resultMap type="com.yxj.entity.Student" id="studentMap">
		<id column="id" property="id"/>
		<result column="name" property="name"/>
		<result column="age" property="age"/>
		<!-- 封装课程信息 一对多   collection类型 -->
		<collection property="courses" javaType="list" ofType="com.yxj.entity.Course">
			<id column="cid" property="id"/>
			<result column="cname" property="name"/>
		</collection>
	</resultMap>


	<select id="queryAll" resultMap="studentMap">
	  select s.id,s.name,s.age,
			 c.id cid,c.name cname 
			 from t_student s 
	         left join t_student_course sc 
	         on s.id = sc.sid
	         left join t_course c
	         on sc.cid = c.id
	</select>
  
</mapper>
```
### 13.4 代码例子
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
    <artifactId>day03_yxj_04many2many</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.5</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>

        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.12</version>
        </dependency>

        <dependency>
            <groupId>junyxj</groupId>
            <artifactId>junyxj</artifactId>
            <version>4.10</version>
        </dependency>
    </dependencies>

</project>
```
```markdown
log4j.properties
```

```java
# Set root category prioryxjy to INFO and yxjs only appender to CONSOLE.
#log4j.rootCategory=INFO, CONSOLE            debug   info   warn error fatal
log4j.rootCategory=debug, CONSOLE, LOGFILE

# Set the enterprise logger category to FATAL and yxjs only appender to CONSOLE.
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
jdbcConfig.properties
```

```java
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/yxj_mybatis
jdbc.username=root
jdbc.password=1234
```
```markdown
SqlMapConfig.xml
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 配置properties-->
    <properties resource="jdbcConfig.properties"></properties>

    <!--使用typeAliases配置别名，它只能配置domain中类的别名 -->
    <typeAliases>
        <package name="com.yxj.domain"></package>
    </typeAliases>

    <!--配置环境-->
    <environments default="mysql">
        <!-- 配置mysql的环境-->
        <environment id="mysql">
            <!-- 配置事务 -->
            <transactionManager type="JDBC"></transactionManager>

            <!--配置连接池-->
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"></property>
                <property name="url" value="${jdbc.url}"></property>
                <property name="username" value="${jdbc.username}"></property>
                <property name="password" value="${jdbc.password}"></property>
            </dataSource>
        </environment>
    </environments>
    <!-- 配置映射文件的位置 -->
    <mappers>
        <package name="com.yxj.dao"></package>
    </mappers>
</configuration>
```
```markdown
User.java
```

```java
package com.yxj.domain;

import java.io.Serializable;
import java.util.Date;
import java.util.List;

/**
 
 * @Company http://www.yxj.com
 */
public class User implements Serializable {

    private Integer id;
    private String username;
    private String address;
    private String sex;
    private Date birthday;

    //多对多的关系映射：一个用户可以具备多个角色
    private List<Role> roles;

    public List<Role> getRoles() {
        return roles;
    }

    public void setRoles(List<Role> roles) {
        this.roles = roles;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", address='" + address + '\'' +
                ", sex='" + sex + '\'' +
                ", birthday=" + birthday +
                '}';
    }
}

```
```markdown
Role.java
```

```java
package com.yxj.domain;

import java.io.Serializable;
import java.util.List;

/**
 
 * @Company http://www.yxj.com
 */
public class Role implements Serializable {

    private Integer roleId;
    private String roleName;
    private String roleDesc;

    //多对多的关系映射：一个角色可以赋予多个用户
    private List<User> users;

    public List<User> getUsers() {
        return users;
    }

    public void setUsers(List<User> users) {
        this.users = users;
    }

    public Integer getRoleId() {
        return roleId;
    }

    public void setRoleId(Integer roleId) {
        this.roleId = roleId;
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

    @Override
    public String toString() {
        return "Role{" +
                "roleId=" + roleId +
                ", roleName='" + roleName + '\'' +
                ", roleDesc='" + roleDesc + '\'' +
                '}';
    }
}

```
```markdown
IUserDao.java
```
```java
package com.yxj.dao;

import com.yxj.domain.User;

import java.util.List;

/**
 
 * @Company http://www.yxj.com
 *
 * 用户的持久层接口
 */
public interface IUserDao {

    /**
     * 查询所有用户，同时获取到用户下所有账户的信息
     * @return
     */
    List<User> findAll();


    /**
     * 根据id查询用户信息
     * @param userId
     * @return
     */
    User findById(Integer userId);


}

```
```markdown
IUserDao.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.yxj.dao.IUserDao">

    <!-- 定义User的resultMap-->
    <resultMap id="userMap" type="user">
        <id property="id" column="id"></id>
        <result property="username" column="username"></result>
        <result property="address" column="address"></result>
        <result property="sex" column="sex"></result>
        <result property="birthday" column="birthday"></result>
        <!-- 配置角色集合的映射 -->
        <collection property="roles" ofType="role">
            <id property="roleId" column="rid"></id>
            <result property="roleName" column="role_name"></result>
            <result property="roleDesc" column="role_desc"></result>
        </collection>
    </resultMap>

    <!-- 查询所有 -->
    <select id="findAll" resultMap="userMap">
        select u.*,r.id as rid,r.role_name,r.role_desc from user u
         left outer join user_role ur  on u.id = ur.uid
         left outer join role r on r.id = ur.rid
    </select>

    <!-- 根据id查询用户 -->
    <select id="findById" parameterType="INT" resultType="user">
        select * from user where id = #{uid}
    </select>

</mapper>
```
```markdown
IRoleDao.java
```

```java
package com.yxj.dao;

import com.yxj.domain.Role;

import java.util.List;

/**
 
 * @Company http://www.yxj.com
 */
public interface IRoleDao {

    /**
     * 查询所有角色
     * @return
     */
    List<Role> findAll();
}

```
```markdown
IRoleDao.xml
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.yxj.dao.IRoleDao">

    <!--定义role表的ResultMap-->
    <resultMap id="roleMap" type="role">
        <id property="roleId" column="rid"></id>
        <result property="roleName" column="role_name"></result>
        <result property="roleDesc" column="role_desc"></result>
        <collection property="users" ofType="user">
            <id column="id" property="id"></id>
            <result column="username" property="username"></result>
            <result column="address" property="address"></result>
            <result column="sex" property="sex"></result>
            <result column="birthday" property="birthday"></result>
        </collection>
    </resultMap>

    <!--查询所有-->
    <select id="findAll" resultMap="roleMap">
       select u.*,r.id as rid,r.role_name,r.role_desc from role r
        left outer join user_role ur  on r.id = ur.rid
        left outer join user u on u.id = ur.uid
    </select>
</mapper>
```
```markdown
UserTest.java
```
```java
package com.yxj.test;

import com.yxj.dao.IUserDao;
import com.yxj.domain.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junyxj.After;
import org.junyxj.Before;
import org.junyxj.Test;

import java.io.InputStream;
import java.util.List;

/**
 
 * @Company http://www.yxj.com
 */
public class UserTest {

    private InputStream in;
    private SqlSession sqlSession;
    private IUserDao userDao;

    @Before//用于在测试方法执行之前执行
    public void init()throws Exception{
        //1.读取配置文件，生成字节输入流
        in = Resources.getResourceAsStream("SqlMapConfig.xml");
        //2.获取SqlSessionFactory
        SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
        //3.获取SqlSession对象
        sqlSession = factory.openSession(true);
        //4.获取dao的代理对象
        userDao = sqlSession.getMapper(IUserDao.class);
    }

    @After//用于在测试方法执行之后执行
    public void destroy()throws Exception{
        //提交事务
        // sqlSession.commit();
        //6.释放资源
        sqlSession.close();
        in.close();
    }

    /**
     * 测试查询所有
     */
    @Test
    public void testFindAll(){
        List<User> users = userDao.findAll();
        for(User user : users){
            System.out.println("-----每个用户的信息------");
            System.out.println(user);
            System.out.println(user.getRoles());
        }
    }



}

```
```markdown
RoleTest.java
```
```java
package com.yxj.test;

import com.yxj.dao.IRoleDao;
import com.yxj.domain.Role;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junyxj.After;
import org.junyxj.Before;
import org.junyxj.Test;

import java.io.InputStream;
import java.util.List;

/**
 
 * @Company http://www.yxj.com
 */
public class RoleTest {

    private InputStream in;
    private SqlSession sqlSession;
    private IRoleDao roleDao;

    @Before//用于在测试方法执行之前执行
    public void init()throws Exception{
        //1.读取配置文件，生成字节输入流
        in = Resources.getResourceAsStream("SqlMapConfig.xml");
        //2.获取SqlSessionFactory
        SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
        //3.获取SqlSession对象
        sqlSession = factory.openSession(true);
        //4.获取dao的代理对象
        roleDao = sqlSession.getMapper(IRoleDao.class);
    }

    @After//用于在测试方法执行之后执行
    public void destroy()throws Exception{
        //提交事务
        // sqlSession.commit();
        //6.释放资源
        sqlSession.close();
        in.close();
    }

    /**
     * 测试查询所有
     */
    @Test
    public void testFindAll(){
        List<Role> roles = roleDao.findAll();
        for(Role role : roles){
            System.out.println("---每个角色的信息----");
            System.out.println(role);
            System.out.println(role.getUsers());
        }
    }

}

```
## 14 关联关系总结
```markdown
一对一,在任意一方引入对方主键作为外键;
一对多,在"多"的一方,添加"一"的一方的主键外键
多对多,产生中间关系表,引入两张表的主键作为外键,两
个主键成为联合主键.
```
## 15 常用配置
```markdown
db.properties
```
```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/yxjmydatabase?useUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC
jdbc.username=root
jdbc.password=123456
```
```markdown
log4j.properties
```
```properties
# Global logging configuration
log4j.rootLogger=ERROR,stdout
# MyBatis logging configuration.
log4j.logger.yxj.com.yxj=DEBUG
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

## 16 图片
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029162511584.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029162526734.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029162537735.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029162549517.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029162605679.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029162616473.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102916262979.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029162751643.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102916281298.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029162832158.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/202010291628467.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029162900504.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029162913770.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029162929660.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029162945320.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029162959864.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029163013805.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029163030531.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复
mybatis即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
