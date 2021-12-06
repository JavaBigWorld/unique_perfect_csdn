# Mybatis-Plus
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210711221724131.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

## 1 什么是Mybatis-Plus

### 1.1 什么是mybatis-plus
```markdown
MyBatis-Plus (opens new window)（简称 MP）是一个
MyBatis (opens new window)的增强工具，在 MyBatis
的基础上只做增强不做改变，为简化开发、提高效率而生。
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102221191149.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


### 1.2 官方愿景
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022212038474.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


### 1.3 特性
```markdown
无侵入：只做增强不做改变，引入它不会对现有工程产生影响，
如丝般顺滑
损耗小：启动即会自动注入基本 CURD，性能基本无损耗，
直接面向对象操作
强大的 CRUD 操作：内置通用 Mapper、通用 Service，
仅仅通过少量配置即可实现单表大部分 CRUD 操作，更
有强大的条件构造器，满足各类使用需求
支持 Lambda 形式调用：通过 Lambda 表达式，方便的
编写各类查询条件，无需再担心字段写错
支持主键自动生成：支持多达 4 种主键策
略（内含分布式唯一 ID 生成器 - Sequence），可自由
配置，完美解决主键问题
支持 ActiveRecord 模式：支持 ActiveRecord 形式调
用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
支持自定义全局通用操作：支持全局通用方法注入
（ Write once, use anywhere ）
内置代码生成器：采用代码或者 Maven 插件可快速生成 
Mapper 、 Model 、 Service 、 Controller 层代
码，支持模板引擎，更有超多自定义配置等您来使用
内置分页插件：基于 MyBatis 物理分页，开发者无需关心
具体操作，配置好插件之后，写分页等同于普通 List 查询
分页插件支持多种数据库：支持 MySQL、MariaDB、
Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 
等多种数据库
内置性能分析插件：可输出 Sql 语句以及其执行时间，
建议开发测试时启用该功能，能快速揪出慢查询
内置全局拦截插件：提供全表 delete 、 update 操作智能分析阻
断，也可自定义拦截规则，预防误操作
```

### 1.4 支持数据库
```markdown
mysql 、 mariadb 、 oracle 、 db2 、 h2 、
hsql 、 sqlite 、 postgresql 、 sqlserver
```
### 1.5 框架结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022212058841.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


## 2 快速入门

### 2.1 创建springboot项目
#### 2.1.1 并引入依赖

```xml
 <dependency>
   <groupId>com.baomidou</groupId>
   <artifactId>mybatis-plus-boot-starter</artifactId>
   <version>3.2.0</version>
</dependency>
```
```markdown
注意:不需要在引入mybatis的相关依赖,只引入这一个即可,
当然数据库相关的驱动还的显式引入
```
#### 2.1.2 在入口类加入注解

```java
@SpringBootApplication
@MapperScan("com.yxj.dao")
public class MybatisApplication {
    public static void main(String[] args) {
        SpringApplication.run(MybatisApplication.class, args);
    }
}
```

#### 2.1.3 编写配置文件

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/db?useUnicode=true&characterEncoding=UTF-8
    username: root
    password: root
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```
### 2.2 创建数据库以及表结构

```sql
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `age` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

SET FOREIGN_KEY_CHECKS = 1;
```

### 2.3 开发实体类

```java
@Data //lombok的注解用来生成get set 等相关方法
public class User {
    private String id;
    private String name;
    private Integer age;
}
```

### 2.4 开发mapper通用实现

```java
import com.baomidou.mybatisplus.core.mapper.BaseMapper; //提供了各种CRUD方法
public interface UserDAO extends BaseMapper<User> {  
}
```

### 2.5 测试

```java
@Autowired
private UserDAO userDAO;
@Test
void contextLoads() {
  List<User> users = userDAO.selectList(null);
  System.out.println(users);
}
```

## 3 常用注解说明
### 3.1 @TableName注解
```java
描述:用来将实体对象与数据库表名完成映射
修饰范围:用在类上
常见属性:
value:String类型,指定映射的表名
resultMap:String类型,用来指定XML配置中resultMap的id值


@Data //lombok的注解用来生成get set 等相关方法
@TableName(value = "user")
public class User {
    private String id;
    private String name;
    private Integer age;
}
```

### 3.2 @TableId注解
```markdown
描述：主键注解

修饰范围:用在属性上

常见属性:
value:String类型,指定实体类中与表中对应的主键列名
type:枚举类型,指定主键生成类型
```
```markdown
AUTO(0),
NONE(1),
INPUT(2),
ASSIGN_ID(3),
ASSIGN_UUID(4),
/** @deprecated */
@Deprecated
ID_WORKER(3),
/** @deprecated */
@Deprecated
ID_WORKER_STR(3),
/** @deprecated */
@Deprecated
UUID(4);
```
| 值          | 描述                              |
| ----------- | --------------------------------- |
| AUTO        | 数据库自增                        |
| NONE        | MP set 主键，雪花算法实现         |
| INPUT       | 需要开发者手动赋值                |
| ASSIGN_ID   | MP 分配 ID，Long、Integer、String |
| ASSIGN_UUID | 分配 UUID，String              |
```markdown
INPUT 如果开发者没有手动赋值，则数据库通过自增的方式
给主键赋值，
如果开发者手动赋值，则存入该值。

AUTO 默认就是数据库自增，开发者无需赋值。

ASSIGN_ID MP 自动赋值，雪花算法。

ASSIGN_UUID 主键的数据类型必须是 String，自动生成 UUID 
进行赋值
```
### 3.3 @TableField
```java
描述：字段注解(非主键)
修饰范围:用在属性上
常用属性:
value:	String类型,用来指定对应的数据库表中的字段名
exist	boolean是否为数据库表字段 true代表是数据库字段,
false代表不是

select 表示是否查询该字段

fill 表示是否自动填充，将对象存入数据库的时候，由 MyBatis 
Plus 自动给某些字段赋值，create_time、update_time

1、给表添加 create_time、update_time 字段

2、实体类中添加成员变量

import com.baomidou.mybatisplus.annotation.FieldFill;
import com.baomidou.mybatisplus.annotation.TableField;
import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableName;
import lombok.Data;

import java.util.Date;

@Data
@TableName(value = "user")
public class User {
    @TableId
    private String id;
    @TableField(value = "name",select = false)
    private String title;
    private Integer age;
    @TableField(exist = false)
    private String gender;
    @TableField(fill = FieldFill.INSERT)
    private Date createTime;
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Date updateTime;
}


3、创建自动填充处理器
import com.baomidou.mybatisplus.core.handlers.MetaObjectHandler;
import org.apache.ibatis.reflection.MetaObject;
import org.springframework.stereotype.Component;

import java.util.Date;

@Component
public class MyMetaObjectHandler implements MetaObjectHandler {
    @Override
    public void insertFill(MetaObject metaObject) {
        this.setFieldValByName("createTime",new Date(),metaObject);
        this.setFieldValByName("updateTime",new Date(),metaObject);
    }

    @Override
    public void updateFill(MetaObject metaObject) {
        this.setFieldValByName("updateTime",new Date(),metaObject);
    }
}
```
### 3.4 @Version
```markdown
标记乐观锁，通过 version 字段来保证数据的安全性，
当修改数据的时候，会以 version 作为条件，当条件
成立的时候才会修改成功。
```
```markdown
version = 2
线程 1:update ... set version = 2  where version = 1

线程2 ：update ... set version = 2 where version = 1

1、数据库表添加 version 字段，默认值为 1

2、实体类添加 version 成员变量，并且添加 @Version 
```
```java
import com.baomidou.mybatisplus.annotation.*;
import lombok.Data;

import java.util.Date;

@Data
@TableName(value = "user")
public class User {
    @TableId
    private String id;
    @TableField(value = "name",select = false)
    private String title;
    private Integer age;
    @TableField(exist = false)
    private String gender;
    @TableField(fill = FieldFill.INSERT)
    private Date createTime;
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Date updateTime;
    @Version
    private Integer version;
}
```
```java
3、注册配置类
import com.baomidou.mybatisplus.extension.plugins.OptimisticLockerInterceptor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MyBatisPlusConfig {
    
    @Bean
    public OptimisticLockerInterceptor optimisticLockerInterceptor(){
        return new OptimisticLockerInterceptor();
    }
    
}
```
### 3.5 @EnumValue
```java
1、通用枚举类注解，将数据库字段映射成实体类的枚举类型成员变量
import com.baomidou.mybatisplus.annotation.EnumValue;

public enum StatusEnum {
    WORK(1,"上班"),
    REST(0,"休息");

    StatusEnum(Integer code, String msg) {
        this.code = code;
        this.msg = msg;
    }

    @EnumValue
    private Integer code;
    private String msg;
}


import com.baomidou.mybatisplus.annotation.*;
import com.yao.mybatisplus.enums.StatusEnum;
import lombok.Data;

import java.util.Date;

@Data
@TableName(value = "user")
public class User {
    @TableId
    private String id;
    @TableField(value = "name",select = false)
    private String title;
    private Integer age;
    @TableField(exist = false)
    private String gender;
    @TableField(fill = FieldFill.INSERT)
    private Date createTime;
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Date updateTime;
    @Version
    private Integer version;
    private StatusEnum status;
}
```

```yaml
编写配置文件
application.yml
type-enums-package: 
  com.yao.mybatisplus.enums
```
```java
2、实现接口

import com.baomidou.mybatisplus.core.enums.IEnum;

public enum AgeEnum implements IEnum<Integer> {
    ONE(1,"一岁"),
    TWO(2,"两岁"),
    THREE(3,"三岁");

    private Integer code;
    private String msg;

    AgeEnum(Integer code, String msg) {
        this.code = code;
        this.msg = msg;
    }

    @Override
    public Integer getValue() {
        return this.code;
    }
}
```
### 3.6 @TableLogic
```java
映射逻辑删除

1、数据表添加 deleted 字段

2、实体类添加注解

package com.southwind.mybatisplus.entity;

import com.baomidou.mybatisplus.annotation.*;
import com.yao.mybatisplus.enums.AgeEnum;
import com.yao.mybatisplus.enums.StatusEnum;
import lombok.Data;

import java.util.Date;

@Data
@TableName(value = "user")
public class User {
    @TableId
    private String id;
    @TableField(value = "name",select = false)
    private String title;
    private AgeEnum age;
    @TableField(exist = false)
    private String gender;
    @TableField(fill = FieldFill.INSERT)
    private Date createTime;
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Date updateTime;
    @Version
    private Integer version;
    @TableField(value = "status")
    private StatusEnum statusEnum;
    @TableLogic
    private Integer deleted;
}


3、application.yml 添加配置
global-config:
  db-config:
    logic-not-delete-value: 0
    logic-delete-value: 1
```
## 4 常用方法

### 4.1 查询方法
#### 4.1.1 查询所有
```java
@Test
public void testFindAll(){
  List<User> users = userDAO.selectList(null);
  users.forEach(user-> System.out.println("user = " + user));
}
```
#### 4.1.2 查询一个
```java
@Test
public void testFindOne(){
  User user = userDAO.selectById("1");
  System.out.println("user = " + user);
}
```

#### 4.1.3 逻辑查询
```java
//条件查询
@Test
public void testFind(){
  QueryWrapper<User> queryWrapper = new QueryWrapper<>();
  //queryWrapper.eq("age",23);//设置等值查询
  //queryWrapper.lt("age",23);//设置小于查询
  //queryWrapper.ge("age",23);//小于等于查询 gt 大于  ge 大于等于
  List<User> users = userDAO.selectList(queryWrapper);
  users.forEach(user-> System.out.println(user));
}
```
#### 4.1.4 按list查询
```java
@Test 
public void testSelectByBatchId(){ 
	List<User> users = userMapper.selectBatchIds(Arrays.asList(1, 2, 3)); 
	users.forEach(System.out::println); 
}
```
#### 4.1.5 按map查询
```java
Map 只能做等值判断，逻辑判断需要使用 Wrapper 来处理
        Map<String,Object> map = new HashMap<>();
        map.put("id",7);
        map.put("age",3);
        mapper.selectByMap(map).forEach(System.out::println);
```
#### 4.1.6 模糊查询
```java
 @Test
 public void testFindAll(){
   QueryWrapper<User> queryWrapper = new QueryWrapper<>();
   queryWrapper.likeRight("username","小");
   List<User> users = userDAO.selectList(queryWrapper);
   users.forEach(user-> System.out.println("user = " + user));
 }
```
```markdown
like 相当于 %?%
likeLeft 相当于 %?
likeRight 相当于 ?%
```
#### 4.1.7 Mybatis-Plus分页查询

##### 4.1.7.1 预先配置
```markdown
注意:使用分页查询必须设置mybatis-plus提供的分页插件,
才能实现分页效果
```

```java
  @EnableTransactionManagement
  @Configuration
  @MapperScan("com.yxj.dao")
  public class MybatisPlusConfig {
    // 最新版
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        return interceptor;
    }
  }
```

```markdown
注意事项:目前分页查询仅仅支持单表查询,不能再表连接时
使用分页插件
```
##### 4.1.7.2 分页查询
```markdown
非条件分页查询
```
```java
  @Test
  public void testFindAll(){
    IPage<User> page = new Page<>(1,2);
    page = userDAO.selectPage(page, null);
    page.getRecords().forEach(user -> System.out.println("user = " + user));
  }
```
```markdown
带条件分页查询
```
```java
  @Test
  public void testFindAll(){
    QueryWrapper<User> queryWrapper = new QueryWrapper<>();
    queryWrapper.eq("age",23);
    IPage<User> page = new Page<>(1,2);
    page = userDAO.selectPage(page, queryWrapper);
    page.getRecords().forEach(user-> System.out.println("user = " + user));
  }
```
#### 4.1.8 自定义 SQL（多表关联查询）
```java
package com.yao.mybatisplus.entity;

import lombok.Data;

@Data
public class ProductVO {
    private Integer category;
    private Integer count;
    private String description;
    private Integer userId;
    private String userName;
}


package com.yao.mybatisplus.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.southwind.mybatisplus.entity.ProductVO;
import com.southwind.mybatisplus.entity.User;
import org.apache.ibatis.annotations.Select;

import java.util.List;

public interface UserMapper extends BaseMapper<User> {
    @Select("select p.*,u.name userName from product p,user u where p.user_id = u.id and u.id = #{id}")
    List<ProductVO> productList(Integer id);
}
```

### 4.2 添加方法
```markdown
添加方法
```
```java
  @Test
  public void testSave(){
    User entity = new User();
    entity.setAge(23).setName("小明明").setBir(new Date());
    userDAO.insert(entity);
  }
```

### 4.3 修改方法
```markdown
基于id修改
```
```java
  @Test
  public void testUpdateById(){
    User user = userDAO.selectById("1");
    user.setAge(24);
    userDAO.updateById(user);
  }
```
```markdown
基于条件修改
```
  ```java
  @Test
  public void testUpdate(){
    User user = userDAO.selectById("1");
    user.setName("小陈陈");
    QueryWrapper<User> updateWrapper = new QueryWrapper<>();
    updateWrapper.eq("age",22);
    userDAO.update(user, updateWrapper);
  }
  ```

### 4.4 删除方法
#### 4.4.1 基于id删除
```java
@Test
public void testDeleteById(){
  userDAO.deleteById("3");
}

mapper.deleteBatchIds(Arrays.asList(7,8)); // // 通过id批量删除
```
#### 4.4.2 基于map删除
```java
//        QueryWrapper wrapper = new QueryWrapper();
//        wrapper.eq("age",14);
//        mapper.delete(wrapper);

Map<String,Object> map = new HashMap<>();
map.put("id",10);
mapper.deleteByMap(map);
```
## 5 执行 SQL 分析打印
```xml
添加依赖
p6spy 依赖引入
<dependency>
  <groupId>p6spy</groupId>
  <artifactId>p6spy</artifactId>
  <version>最新版本</version>
</dependency>

application.yml 配置：
spring:
  datasource:
    driver-class-name: com.p6spy.engine.spy.P6SpyDriver  # com.mysql.cj.jdbc.Driver
    url: jdbc:p6spy:h2:mem:test # 把jdbc改为jdbc:p6spy
    ...

spy.properties 配置：
#3.2.1以上使用
modulelist=com.baomidou.mybatisplus.extension.p6spy.MybatisPlusLogFactory,com.p6spy.engine.outage.P6OutageFactory
#3.2.1以下使用或者不配置
#modulelist=com.p6spy.engine.logging.P6LogFactory,com.p6spy.engine.outage.P6OutageFactory
# 自定义日志打印
logMessageFormat=com.baomidou.mybatisplus.extension.p6spy.P6SpyLogger
#日志输出到控制台
appender=com.baomidou.mybatisplus.extension.p6spy.StdoutLogger
# 使用日志系统记录 sql
#appender=com.p6spy.engine.spy.appender.Slf4JLogger
# 设置 p6spy driver 代理
deregisterdrivers=true
# 取消JDBC URL前缀
useprefix=true
# 配置记录 Log 例外,可去掉的结果集有error,info,batch,debug,statement,commit,rollback,result,resultset.
excludecategories=info,debug,result,commit,resultset
# 日期格式
dateformat=yyyy-MM-dd HH:mm:ss
# 实际驱动可多个
#driverlist=org.h2.Driver
# 是否开启慢SQL记录
outagedetection=true
# 慢SQL记录标准 2 秒
outagedetectioninterval=2


```
## 6 MyBatisPlus 自动生成
### 6.1 添加代码生成器依赖
```markdown
根据数据表自动生成实体类、Mapper、Service、ServiceImpl、Controller

MyBatis-Plus 从 3.0.3 之后移除了代码生成器与模板引擎的默认依赖，需要手动添加相关依赖：

<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-generator</artifactId>
    <version>3.4.1</version>
</dependency>

<dependency>
    <groupId>org.apache.velocity</groupId>
    <artifactId>velocity-engine-core</artifactId>
    <version>latest-velocity-version</version>
</dependency>
```
### 6.2 启动类
```java
import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
import com.baomidou.mybatisplus.generator.config.GlobalConfig;
import com.baomidou.mybatisplus.generator.config.PackageConfig;
import com.baomidou.mybatisplus.generator.config.StrategyConfig;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;

public class Main {
    public static void main(String[] args) {
        //创建generator对象
        AutoGenerator autoGenerator = new AutoGenerator();
        //数据源
        DataSourceConfig dataSourceConfig = new DataSourceConfig();
        dataSourceConfig.setDbType(DbType.MYSQL);
        dataSourceConfig.setUrl("jdbc:mysql://ip:3306/db?useUnicode=true&characterEncoding=UTF-8");
        dataSourceConfig.setUsername("root");
        dataSourceConfig.setPassword("root");
        dataSourceConfig.setDriverName("com.mysql.cj.jdbc.Driver");
        autoGenerator.setDataSource(dataSourceConfig);
        
        //全局配置
        GlobalConfig globalConfig = new GlobalConfig();
        globalConfig.setOutputDir(System.getProperty("user.dir")+"/src/main/java");
        globalConfig.setOpen(false);
        globalConfig.setAuthor("southwind");  // 作者
        globalConfig.setServiceName("%sService");  
        autoGenerator.setGlobalConfig(globalConfig);
        
        //包信息
        PackageConfig packageConfig = new PackageConfig();
        packageConfig.setParent("com.yao.mybatisplus");
        packageConfig.setModuleName("generator");
        packageConfig.setController("controller");
        packageConfig.setService("service");
        packageConfig.setServiceImpl("service.impl");
        packageConfig.setMapper("mapper");
        packageConfig.setEntity("entity");
        autoGenerator.setPackageInfo(packageConfig);
        
        //配置策略
        StrategyConfig strategyConfig = new StrategyConfig();
        strategyConfig.setEntityLombokModel(true);
        strategyConfig.setNaming(NamingStrategy.underline_to_camel);
        strategyConfig.setColumnNaming(NamingStrategy.underline_to_camel);
        autoGenerator.setStrategy(strategyConfig);

        autoGenerator.execute();
    }
}
```
## 7 数据安全保护
```markdown
密钥加密：
// 生成 16 位随机 AES 密钥
String randomKey = AES.generateRandomKey();

// 随机密钥加密
String url = AES.encrypt("jdbc:mysql://ip:3306/db?useUnicode=true&characterEncoding=UTF-8", randomKey);
String username = AES.encrypt("root", randomKey);
String password = AES.encrypt("root", randomKey);

url: mpw:+url
username: mpw:+username
password: mpw:+password

// 加密配置 mpw: 开头紧接加密内容（ 非数据库配置专用 YML 中其它配置也是可以使用的 ）
spring:
  datasource:
    url: mpw:qRhvCwF4GOqjessEB3G+a5okP+uXXr96wcucn2Pev6Bf1oEMZ1gVpPPhdDmjQqoM
    password: mpw:Hzy5iliJbwDHhjLs1L0j6w==
    username: mpw:Xb+EgsyuYRXw7U7sBJjBpA==


// Jar 启动参数（ idea 设置 Program arguments , 服务器可以设置为启动环境变量 ）
java -jar xxxx.jar --mpw.key=randomKey
```
## 8 Mybatis-Plus多数据源配置

### 8.1 引言
```markdown
为了确保数据库产品的稳定性，很多数据库拥有双机热备功能。
也就是，第一台数据库服务器，是对外提供增删改业务的生产服务器；
第二台数据库服务器，主要进行读的操作。
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022212228798.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 8.2 引入dynamic-datasource-spring-boot-starter

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>dynamic-datasource-spring-boot-starter</artifactId>
    <version>3.0.0</version>
</dependency>
```

### 8.3 配置数据源

```properties
spring.datasource.primary=master  #指定默认数据源
spring.datasource.dynamic.datasource.master.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.dynamic.datasource.master.url=jdbc:mysql://localhost:3306/mybatis-plus?characterEncoding=UTF-8
spring.datasource.dynamic.datasource.master.username=root
spring.datasource.dynamic.datasource.master.password=root
spring.datasource.dynamic.datasource.slave_1.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.dynamic.datasource.slave_1.url=jdbc:mysql://localhost:3306/mybatis-plus-1?characterEncoding=UTF-8
spring.datasource.dynamic.datasource.slave_1.username=root
spring.datasource.dynamic.datasource.slave_1.password=root
```

### 8.4 创建多个数据库模拟不同mysql服务 
#### 8.4.1 @DS注解
```markdown
作用:用来切换数据源的注解 
修饰范围:方法上和类上，同时存在方法注解优先于类上注解。
Value属性:切换数据源名称
```
### 8.5 开发业务层
```markdown
业务接口
```
```java
  public interface UserService{
      List<User> findAll();
      void save(User user);
  }
```

  
```markdown
业务实现类
```
```java
  @Service
  @Transactional
  public class UserServiceImpl implements UserService {
  
      @Autowired
      private UserDAO userDAO;
  
      @Override
      @DS("slave_1")
      public List<User> findAll() {
          return userDAO.selectList(null);
      }
  
      @Override
      public void save(User user) {
          userDAO.insert(user);
      }
  }
```

### 8.6 测试结果 

```java
package com.yxj;

import com.yxj.entity.User;
import com.yxj.service.UserService;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.Date;

@SpringBootTest
public class TestUserService {

    
    @Autowired
    private UserService userService;
    
    @Test
    public void testFindAll(){
        userService.findAll().forEach(user-> System.out.println("user = " + user));
    }

    @Test
    public void testSave(){
        User user = new User();
        user.setName("aaa").setAge(23).setBir(new Date());
        userService.save(user);
    }
}
```

```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复
mp即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
