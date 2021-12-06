# Dubbo
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210715233909504.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1 什么是Dubbo
```markdown
dubbo:是一个基于soa思想的rpc框架
soa思想:面向服务的架构
给每一个模块暴露对应的ip和端口,当做一个服务进行运行
重点在于服务的管理(负载均衡,容灾模式,服务的横向扩展)

dubbo与Spring无缝整合
```
## 2 架构的演变
```markdown
随看互联网的发展，网站应用的规模不断扩大，常规的垂直
应用架构已无法应对，分布式服务架构以及流动计算架构勢
在必行，亟需一个治理系统确保架构有条不系的演进。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201216081827811.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
单一应用架构
当网站流量很小时，只需一个应用，将所有功能都部署在一起，
以减少部署节点和成本。
此时，用于简化增删改查工作量的数据访问框架（ORM）是关键。

垂直应用架构
当访问量逐渐增大，单一应用增加机器带来的加速度越来越小，
将应用拆成互不相干的几个应用，以提升效率。
此时，用于加速前端页面开发的Web框架（MVC)是关键

分布式服务架构
当垂直应用越来越多，应用之间交互不可避免，将核心业务抽取
出来，作为独立的服务，逐渐形成稳定的服务中心，使前端应
用能更快速的响应多变的市场需求。
此时，用于提高业务复用及整合的分布式服务框架（RPC)是关键

流动计算架构
当服务越来越多，容量的评估，小服务资源的浪费等同題逐渐
显现，此时需增加一个调度中心基于访问压力实时管理集群
容量，提高集群利用率。
此时，用于提高机器利用率的资源调度和治理中心（SOA)是关键。
```
```markdown
单一应用架构
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210126190221611.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
垂直应用架构
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021012619341357.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
分布式服务架构
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210126193538597.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
注册中心
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210126193719657.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
流动计算架构
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021012619380250.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

## 3 dubbo的架构图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201028224800867.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201028224808777.png#pic_center)

```markdown
调用关系说明:
1 服务容器负责启动，加载，运行服务提供者。 
2 服务提供者在启动时，向注册中心注册自己提供的服务。 
3 服务消费者在启动时，向注册中⼼订阅自己所需的服务。 
4 注册中心返回服务提供者地址列表给消费者，如果有变更，
注册中心将基于长连接推送变更数据给消费者。 
5 服务消费者，从提供者地址列表中，基于软负载均衡算法，
选一台提供者进行调用，如果调用失败，再选另一台调用。 
6 服务消费者和提供者，在内存中累计调用次数和调用时间，
定时每分钟发送一次统计数据到监控中心。 
```



## 4 Dubbo的入门案例

### 4.1 开发dubbo的服务端
```xml
<dependencies>
        <!-- 引入dubbo -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>dubbo</artifactId>
            <version>2.6.2</version>
        </dependency>
        <!-- 由于我们使用zookeeper作为注册中心，所以需要操作zookeeper
        dubbo 2.6以前的版本引入zkclient操作zookeeper
        dubbo 2.6及以后的版本引入curator操作zookeeper
        下面两个zk客户端根据dubbo版本2选1即可
        -->
       <!-- <dependency>
            <groupId>com.101tec</groupId>
            <artifactId>zkclient</artifactId>
            <version>0.10</version>
        </dependency>-->
        <!-- curator-framework -->
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>2.12.0</version>
        </dependency>
</dependencies>
```
	
### 4.2 开发服务提供者接口
```java
package org.apache.dubbo.demo;

public interface DemoService {
    String sayHello(String name);
}
```
### 4.3 开发服务提供者实现类
```java
package org.apache.dubbo.demo.provider;
 
import org.apache.dubbo.demo.DemoService;
 
public class DemoServiceImpl implements DemoService {
    public String sayHello(String name) {
        return "Hello " + name;
    }
}
```

### 4.4 编写dubbo配置文件
```xml
provider.xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
    xsi:schemaLocation="http://www.springframework.org/schema/beans        http://www.springframework.org/schema/beans/spring-beans-4.3.xsd        http://dubbo.apache.org/schema/dubbo        http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
 
    <!-- 提供方应用信息，用于计算依赖关系 -->
    <dubbo:application name="hello-world-app"  />
 
    <!-- 使用multicast广播注册中心暴露服务地址 -->
    <dubbo:registry address="multicast://224.5.6.7:1234" />
 
    <!-- 用dubbo协议在20880端口暴露服务 -->
    <dubbo:protocol name="dubbo" port="20880" />
 
    <!-- 声明需要暴露的服务接口 -->
    <dubbo:service interface="org.apache.dubbo.demo.DemoService" ref="demoService" />
 
    <!-- 和本地bean一样实现服务 -->
    <bean id="demoService" class="org.apache.dubbo.demo.provider.DemoServiceImpl" />
</beans>
```
### 4.5 启动服务提供者
```java
import org.springframework.context.support.ClassPathXmlApplicationContext;
 
public class Provider {
    public static void main(String[] args) throws Exception {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("classpath:provider.xml"});
        context.start();
        System.in.read(); // 按任意键退出
    }
}
```

### 4.6 开发dubbo的服务消费方

### 4.7 引入依赖同服务方依赖
```xml
<dependencies>
        <!-- 引入dubbo -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>dubbo</artifactId>
            <version>2.6.2</version>
        </dependency>
        <!-- 由于我们使用zookeeper作为注册中心，所以需要操作zookeeper
        dubbo 2.6以前的版本引入zkclient操作zookeeper
        dubbo 2.6及以后的版本引入curator操作zookeeper
        下面两个zk客户端根据dubbo版本2选1即可
        -->
       <!-- <dependency>
            <groupId>com.101tec</groupId>
            <artifactId>zkclient</artifactId>
            <version>0.10</version>
        </dependency>-->
        <!-- curator-framework -->
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>2.12.0</version>
        </dependency>
</dependencies>
```
### 4.8 将服务方的接口复制到消费方项目中
```java
package org.apache.dubbo.demo;

public interface DemoService {
    String sayHello(String name);
}
注意: 包结构要与服务提供方严格一致
```
### 4.9 配置服务消费方配置文件

```xml
consumer.xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
    xsi:schemaLocation="http://www.springframework.org/schema/beans        http://www.springframework.org/schema/beans/spring-beans-4.3.xsd        http://dubbo.apache.org/schema/dubbo        http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
 
    <!-- 消费方应用名，用于计算依赖关系，不是匹配条件，不要与提供方一样 -->
    <dubbo:application name="consumer-of-helloworld-app"  />
 
    <!-- 使用multicast广播注册中心暴露发现服务地址 -->
    <dubbo:registry address="multicast://224.5.6.7:1234" />
 
    <!-- 生成远程服务代理，可以和本地bean一样使用demoService -->
    <dubbo:reference id="demoService" interface="org.apache.dubbo.demo.DemoService" />
</beans>
```
```markdown
Error creating bean with name 'userService': FactoryBean threw exception on
object creation; nested exception is java.lang.IllegalStateException: Failed to
check the status of the service com.yxj.service.UserService. No provider
available for the service com.yxj.service.UserService from the url
multicast://224.5.6.7:1234/com.alibaba.dubbo.registry.RegistryService?
application=dubboConsummer&dubbo=2.0.2&interface=com.yxj.service.UserSe
rvice&methods=findName,addUser&pid=12984&register.ip=192.168.42.1&side=
consumer&timestamp=1608813788441 to the consumer 192.168.42.1 use
dubbo version 2.6.5
```
```markdown
解决方法： 
scope: 224.0.0.0 - 239.255.255.255
把注册地址改为其中一个，这里我改为改为239.255.255.254:1234,不要为multicast://224.5.6.7:1234
```
### 4.10 调用服务方服务

```java
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.apache.dubbo.demo.DemoService;
 
public class Consumer {
    public static void main(String[] args) throws Exception {
       ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("classpath:consumer.xml"});
        context.start();
        DemoService demoService = (DemoService)context.getBean("demoService"); // 获取远程服务代理
        String hello = demoService.sayHello("world"); // 执行远程方法
        System.out.println( hello ); // 显示调用结果
    }
}
```

##  5 基于zk的注册中心
```markdown
默认dubbo使用的是其默认组件multicast组件作为dubbo的
注册中心,在dubbo中可以选择很多的组件作为注册中心,通
常在发布服务提供者时强烈建议使用zookeeper作为服
务的注册中心,具体配置如下:
```
```xml
<!--提供方的应用信息-->
<dubbo:application name="dubbo_001_p"/>
<!--注册中心-->
<dubbo:registry address="zookeeper://192.168.28.136:2181" />
<!--暴露dubbo的协议-->
<dubbo:protocol name="dubbo" port="20880"/>
......
使用以上配置dubbo的服务就是基于zk的注册中心了
```
## 6 SpringBoot整合dubbo
```markdown
引入依赖
```
```xml
<properties>
    <spring-boot.version>2.3.0.RELEASE</spring-boot.version>
    <dubbo.version>2.7.8</dubbo.version>
</properties>
    
<dependencyManagement>
    <dependencies>
        <!-- Spring Boot -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>${spring-boot.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>

        <!-- Apache Dubbo  -->
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-dependencies-bom</artifactId>
            <version>${dubbo.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
      
    </dependencies>
</dependencyManagement>

<dependencies>
    <!-- Dubbo Spring Boot Starter -->
    <dependency>
        <groupId>org.apache.dubbo</groupId>
        <artifactId>dubbo-spring-boot-starter</artifactId>
        <version>2.7.8</version>
    </dependency>    
</dependencies>
```
```markdown
开发服务提供者
```
```java
public interface DemoService {

    String sayHello(String name);

}


@DubboService
public class DefaultDemoService implements DemoService {
   

    public String sayHello(String name) {
    	return "hello";
    }
}

@EnableAutoConfiguration
public class DubboProviderDemo {

    public static void main(String[] args) {
        SpringApplication.run(DubboProviderDemo.class,args);
    }
}
```

```markdown
application.properties
```
```properties
提供者配置：
dubbo.application.name=gmall-user # application.name就是服务名，不能跟别的dubbo提供端重复
dubbo.registry.address=192.168.67.159:2181 # registry.address 是注册中心的地址加端口号
dubbo.registry.protocol=zookeeper # registry.protocol 是指定注册中心协议

dubbo.protocol.name=dubbo # protocol.name 是分布式固定是dubbo,不要改。
dubbo.protocol.port=20880

dubbo.scan.base-package=com.yxj.gmall # base-package  注解方式要扫描的包
```
```markdown
开发消费者
```
```markdown
application.properties
```
```markdown
dubbo.application.name=gmall-order-web
dubbo.registry.protocol=zookeeper
dubbo.registry.address=192.168.67.159:2181
dubbo.scan.base-package=com.yxj.gmall  # 如果没有在配置中写dubbo.scan.base-package,还需要使用@EnableDubbo注解
```
```java
@EnableAutoConfiguration
public class DubboAutoConfigurationConsumerBootstrap {


    @DubboReference
    private DemoService demoService;

    public static void main(String[] args) {
        SpringApplication.run(DubboAutoConfigurationConsumerBootstrap.class).close();
        System.out.println(demoService.sayHello("hello")); // 显示调用结果
    }

 
}
```
### 6.1 SpringBoot与dubbo整合的三种方式
```java
1）导入dubbo-starter，在application.properties配置属性，使用@DubboService【暴露服务】使用@DubboReference【引用服务】（上面就是这种方式）
2）保留dubbo xml配置文件;
导入dubbo-starter，在启动入口类使用@ImportResource导入dubbo的配置文件即可
@ImportResource(locations="classpath:provider.xml")
@SpringBootApplication
public class BootUserServiceProviderApplication {

	public static void main(String[] args) {
		SpringApplication.run(BootUserServiceProviderApplication.class, args);
	}
}
3）使用注解API的方式：
将每一个组件手动创建到容器中,让dubbo来扫描其他的组件
package com.yxj.gmall.config;

import java.util.ArrayList;
import java.util.List;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.alibaba.dubbo.config.ApplicationConfig;
import com.alibaba.dubbo.config.MethodConfig;
import com.alibaba.dubbo.config.MonitorConfig;
import com.alibaba.dubbo.config.ProtocolConfig;
import com.alibaba.dubbo.config.ProviderConfig;
import com.alibaba.dubbo.config.RegistryConfig;
import com.alibaba.dubbo.config.ServiceConfig;
import com.yxj.gmall.service.UserService;

@Configuration
public class MyDubboConfig {
	
	@Bean
	public ApplicationConfig applicationConfig() {
		ApplicationConfig applicationConfig = new ApplicationConfig();
		applicationConfig.setName("boot-user-service-provider");
		return applicationConfig;
	}
	
	//<dubbo:registry protocol="zookeeper" address="127.0.0.1:2181"></dubbo:registry>
	@Bean
	public RegistryConfig registryConfig() {
		RegistryConfig registryConfig = new RegistryConfig();
		registryConfig.setProtocol("zookeeper");
		registryConfig.setAddress("127.0.0.1:2181");
		return registryConfig;
	}
	
	//<dubbo:protocol name="dubbo" port="20882"></dubbo:protocol>
	@Bean
	public ProtocolConfig protocolConfig() {
		ProtocolConfig protocolConfig = new ProtocolConfig();
		protocolConfig.setName("dubbo");
		protocolConfig.setPort(20882);
		return protocolConfig;
	}
	
	/**
	 *<dubbo:service interface="com.yxj.gmall.service.UserService" 
		ref="userServiceImpl01" timeout="1000" version="1.0.0">
		<dubbo:method name="getUserAddressList" timeout="1000"></dubbo:method>
	</dubbo:service>
	 */
	@Bean
	public ServiceConfig<UserService> userServiceConfig(UserService userService){
		ServiceConfig<UserService> serviceConfig = new ServiceConfig<>();
		serviceConfig.setInterface(UserService.class);
		serviceConfig.setRef(userService);
		serviceConfig.setVersion("1.0.0");
		
		//配置每一个method的信息
		MethodConfig methodConfig = new MethodConfig();
		methodConfig.setName("getUserAddressList");
		methodConfig.setTimeout(1000);
		
		//将method的设置关联到service配置中
		List<MethodConfig> methods = new ArrayList<>();
		methods.add(methodConfig);
		serviceConfig.setMethods(methods);
		
		//ProviderConfig
		//MonitorConfig
		
		return serviceConfig;
	}

}


@EnableDubbo(scanBasePackages="com.yxj.gmall")
@SpringBootApplication
public class BootUserServiceProviderApplication {

	public static void main(String[] args) {
		SpringApplication.run(BootUserServiceProviderApplication.class, args);
	}
}
```
## 7 dubbo配置
```markdown
配置原则
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210127105518689.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
VM 启动 -D 参数优先，这样可以使用户在部署和启动时进行参数
重写，比如在启动时需改变协议的端口。
XML 次之，如果在 XML 中有配置，则 dubbo.properties 中
的相应配置项无效。
Properties 最后，相当于缺省值，只有 XML 没有配置时，
dubbo.properties 的相应配置项才会生效，通常用于共享公
共配置，比如应用名。
```
```markdown
启动时检查
```
```markdown
启动时检查
在启动时检查依赖的服务是否可用
Dubbo 缺省会在启动时检查依赖的服务是否可用，不可用时会
抛出异常，阻止 Spring 初始化完成，以便上线时，能及早发
现问题，默认 check="true"。

可以通过 check="false" 关闭检查，比如，测试时，有些服务
不关心，或者出现了循环依赖，必须有一方先启动。

另外，如果你的 Spring 容器是懒加载的，或者通过 API
 编程延迟引用服务，请关闭 check，否则服务临时不可
 用时，会抛出异常，拿到 null 引用，如果 check="false"，
 总是会返回引用，当服务恢复时，能自动连上。

通过 spring 配置文件 
关闭某个服务的启动时检查 (没有提供者时报错)：
<dubbo:reference interface="com.foo.BarService" check="false" />

关闭所有服务的启动时检查 (没有提供者时报错)：
<dubbo:consumer check="false" />

关闭注册中心启动时检查 (注册订阅失败时报错)：
<dubbo:registry check="false" />

通过 dubbo.properties 
dubbo.reference.com.foo.BarService.check=false
dubbo.reference.check=false
dubbo.consumer.check=false
dubbo.registry.check=false

通过 -D 参数
java -Ddubbo.reference.com.foo.BarService.check=false
java -Ddubbo.reference.check=false
java -Ddubbo.consumer.check=false 
java -Ddubbo.registry.check=false

配置的含义
dubbo.reference.check=false，强制改变所有 reference 的 check 值，就算配置中有声明，也会被覆盖。

dubbo.consumer.check=false，是设置 check 的缺省值，如果配置中有显式的声明，如：<dubbo:reference check="true"/>，不会受影响。

dubbo.registry.check=false，前面两个都是指订阅成功，但提供者列表是否为空是否报错，如果注册订阅失败时，也允许启动，需使用此选项，将在后台定时重试。
```
```markdown
重试次数
```
```markdown
失败自动切换，当出现失败，重试其它服务器，但重试会
带来更长延迟。可通过 retries="2" 来设置重试次
数(不含第一次)。
```
```xml
重试次数配置如下：
<dubbo:service retries="2" />
或
<dubbo:reference retries="2" />
或
<dubbo:reference>
    <dubbo:method name="findFoo" retries="2" />
</dubbo:reference>
```
```markdown
超时时间
```
```markdown
由于网络或服务端不可靠，会导致调用出现一种不确定的中间
状态（超时）。为了避免超时导致客户端资源（线程）挂起耗尽，
必须设置超时时间。
```
```xml
Dubbo消费端 
全局超时配置
<dubbo:consumer timeout="5000" />

指定接口以及特定方法超时配置
<dubbo:reference interface="com.foo.BarService" timeout="2000">
    <dubbo:method name="sayHello" timeout="3000" />
</dubbo:reference>

Dubbo服务端 
全局超时配置
<dubbo:provider timeout="5000" />

指定接口以及特定方法超时配置
<dubbo:provider interface="com.foo.BarService" timeout="2000">
    <dubbo:method name="sayHello" timeout="3000" />
</dubbo:provider>


```
```markdown
配置原则
```
```markdown
dubbo推荐在Provider上尽量多配置Consumer端属性：
1 作服务的提供者，比服务使用方更清楚服务性能参数，如调用的
超时时间，合理的重试次数，等等
2 在Provider配置后，Consumer不配置则会使用Provider的
配置值，即Provider配置可以作为Consumer的缺省值。否
则，Consumer会使用Consumer端的全局设置，这
对于Provider不可控的，并且往往是不合理的

配置的覆盖规则：
1) 方法级配置别优于接口级别，即小Scope优先 
2) Consumer端配置 优于 Provider配置 优于 全局配置，
3) 最后是Dubbo Hard Code的配置值（见配置文档）
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210127110643979.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
版本号
```
```markdown
当一个接口实现，出现不兼容升级时，可以用版本号过渡，
版本号不同的服务相互间不引用。
可以按照以下的步骤进行版本迁移：
在低压力时间段，先升级一半提供者为新版本
再将所有消费者升级为新版本
然后将剩下的一半提供者升级为新版本
```
```xml
老版本服务提供者配置：
<dubbo:service interface="com.foo.BarService" version="1.0.0" />

新版本服务提供者配置：
<dubbo:service interface="com.foo.BarService" version="2.0.0" />

老版本服务消费者配置：
dubbo:reference id="barService" interface="com.foo.BarService" version="1.0.0" />

新版本服务消费者配置：
<dubbo:reference id="barService" interface="com.foo.BarService" version="2.0.0" />

如果不需要区分版本，可以按照以下的方式配置：
<dubbo:reference id="barService" interface="com.foo.BarService" version="*" />

```
```markdown
本地存根
```
```xml
在 消费者spring 配置文件中按以下方式配置：
<!-- 生成远程服务代理，可以和本地bean一样使用demoService -->
    <dubbo:reference id="userService" interface="com.yxj.service.BarService" stub="com.yxj.service.impl.BarServiceStub"/>

<dubbo:reference id="userService" interface="com.yxj.service.BarService" stub="true"/> # 则默认去找com.yxj.service.BarServiceStub
```
```java
消费方实现接口
public class BarServiceStub implements BarService {
    private final BarService barService;
    
    // 构造函数传入真正的远程代理对象
    public BarServiceStub(BarService barService){
        this.barService = barService;
    }
 
    public String sayHello(String name) {
        // 此代码在客户端执行, 你可以在客户端做ThreadLocal本地缓存，或预先验证参数是否合法，等等
        try {
            return barService.sayHello(name);
        } catch (Exception e) {
            // 你可以容错，可以做任何AOP拦截事项
            return "容错数据";
        }
    }
}
```
## 8 高可用
```markdown
zookeeper宕机与dubbo直连
现象：zookeeper注册中心宕机，还可以消费dubbo暴露的服务。
原因：
健壮性
监控中心宕掉不影响使用，只是丢失部分采样数据
数据库宕掉后，注册中心仍能通过缓存提供服务列表查询，
但不能注册新服务
注册中心对等集群，任意一台宕掉后，将自动切换到另一台
注册中心全部宕掉后，服务提供者和服务消费者仍能通过本
地缓存通讯
服务提供者无状态，任意一台宕掉后，不影响使用
服务提供者全部宕掉后，服务消费者应用将无法使用，并无限次
重连等待服务提供者恢复
高可用：通过设计，减少系统不能提供服务的时间；

dubbo直连：消费方可以直连服务方的ip+端口。这样就可以
绕过注册中心同样可以消费
<dubbo:reference id="userService" interface="com.yxj.service.UserService"  url="192.168.1.101:20880"/> # 192.168.1.101:20880代表服务方的ip+端口
```
## 9 dubbo中集群的负载均衡
```markdown
搭建服务提供者的集群
```
```xml
只需要将服务提供者代码,复制几份,修改一下的端口,全部启动即可:
server1: <dubbo:protocol name="dubbo" port="20880"/>
server2: <dubbo:protocol name="dubbo" port="20880"/>
server3: <dubbo:protocol name="dubbo" port="20880"/>
```
```markdown
启动所有的服务提供者集群
使用服务消费者进行调用
注意:默认dubbo使用负载均衡算法是random 随机的
```

```markdown
常见的负载均衡算法
Random LoadBalance 随机，按权重设置随机概率。

RoundRobin LoadBalance 轮循，按公约后的权重设置轮循比率

LeastActive LoadBalance  使慢的提供者收到更少请求，因为越慢的提供者的调用前后计数差会越大。

ConsistentHash LoadBalance 一致性Hash，相同参数的请求总是发到同一提供者。

```
```markdown
Random LoadBalance
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210127161551175.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
RoundRobin LoadBalance
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021012716162217.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
LeastActive LoadBalance
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210127161647332.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
ConsistentHash LoadBalance
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021012716181934.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

### 9.1 设置负载均衡策略
```xml
a. 服务端配置:
<dubbo:service interface="..." loadbalance="roundrobin" />
b . 客户端配置:
<dubbo:reference id="userService" loadbalance="consistenthash" interface="com.baizhi.service.UserService"/>
c. 服务端方法级别:
<dubbo:service interface="...">
    <dubbo:method name="..." loadbalance="roundrobin"/>
</dubbo:service>
配置优先级: 消费者优先级最高,服务端优先级就近原则,一般优先选择在服务提供方配置
```

## 10 dubbo中集群的容错
```xml
在集群调用失败时，Dubbo 提供了多种容错方案，缺
省为 failover 重试。

集群容错模式配置:
<dubbo:service cluster="failsafe" />
或
<dubbo:reference cluster="failsafe" />
```
	
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201028225201429.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
各节点关系：

a 这里的 Invoker 是 Provider 的一个可调用 Service 的抽象，
Invoker 封装了 Provider 地址及 Service 接口信息
Directory 代表多个 Invoker，可以把它看成 List<Invoker> ，
但与 List 不同的是，它的值可能是动态变化的，比如注册中心
推送变更
b Cluster 将 Directory 中的多个 Invoker 伪装成一个Invoker，
对上层透明，伪装过程包含了容错逻辑，调用失败后，重
试另一个
c Router 负责从多个 Invoker 中按路由规则选出子集，比如读写
分离，应用隔离等
d LoadBalance 负责从多个 Invoker 中选出具体的一个用
于本次调用，选的过程包含了负载均衡算法，调用失败
后，需要重选

1	Failover Cluster
失败自动切换，当出现失败，重试其它服务器 。通常用于读
操作，但重试会带来更长延迟。可通过 retries="2" 来设置重
试次数(不含第一次)。
重试次数配置如下：
<dubbo:service retries="2" />
或
<dubbo:reference retries="2" />
或
<dubbo:reference>
    <dubbo:method name="findFoo" retries="2" />
</dubbo:reference>
       
2	Failfast Cluster
快速失败，只发起一次调用，失败立即报错。通常用于非幂等性
的写操作，比如新增记录。

3 Failsafe Cluster
失败安全，出现异常时，直接忽略。通常用于写入审计日志等操作。
4 Failback Cluster
失败自动恢复，后台记录失败请求，定时重发。通常用于消息通知操作。
5 Forking Cluster
并行调用多个服务器，只要一个成功即返回。通常用于实时性要
求较高的读操作，但需要浪费更多服务资源。可通过 forks="2" 来
设置最大并行数。
6 Broadcast Cluster
广播调用所有提供者，逐个调用，任意一台报错则报错 [2]。通常
用于通知所有提供者更新缓存或日志等本地资源信息。


集群模式配置
按照以下示例在服务提供方和消费方配置集群模式
<dubbo:service cluster="failsafe" />
或
<dubbo:reference cluster="failsafe" />
```
## 11 dubbo原理
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210127175557898.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
一次完整的RPC调用流程（同步调用，异步另说）如下： 
1）服务消费方（client）调用以本地调用方式调用服务； 
2）client stub接收到调用后负责将方法、参数等组装成能够进
行网络传输的消息体； 
3）client stub找到服务地址，并将消息发送到服务端； 
4）server stub收到消息后进行解码； 
5）server stub根据解码结果调用本地的服务； 
6）本地服务执行并将结果返回给server stub； 
7）server stub将返回结果打包成消息并发送至消费方； 
8）client stub接收到消息，并进行解码； 
9）服务消费方得到最终结果。
RPC框架的目标就是要2~8这些步骤都封装起来，这些细节对
用户来说是透明的，不可见的。
```
## 12 图片总结
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029152121779.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029152132511.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102915214211.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029152150592.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029152159683.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029152208342.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102915221790.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029152226140.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029152238303.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029152247640.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029152259988.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029152311268.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029152322326.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029152332815.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复dubbo
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)















