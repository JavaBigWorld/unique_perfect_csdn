# Shiro和JWT
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210717003146326.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210716234609405.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
# Shiro 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022202055253.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1 权限的管理

### 1.1 什么是权限管理
```markdown
基本上涉及到用户参与的系统都要进行权限管理，权限管理属于
系统安全的范畴，权限管理实现对用户访问系统的控制，按照安
全规则或者[安全策略]
(http://baike.baidu.com/view/160028.htm)控制用户可以访问而且只能访问自己被授权的资源。

权限管理包括用户身份认证和授权两部分，简称认证授权。对于
需要访问控制的资源用户首先经过身份认证，认证通过后用户
具有该资源的访问权限方可访问。
```


### 1.2 什么是身份认证
```markdown
身份认证，就是判断一个用户是否为合法用户的处理过程。
最常用的简单身份认证方式是系统通过核对用户输入的用
户名和口令，看其是否与系统中存储的该用户的用户名和
口令一致，来判断用户身份是否正确。对于采用指纹等
系统，则出示指纹；对于硬件Key等刷卡系统，则需要刷卡。
```


### 1.3 什么是授权
```markdown
授权，即访问控制，控制谁能访问哪些资源。主体进行身份认
证后需要分配权限方可访问系统的资源，对于某些资源没有
权限是无法访问的
```



## 2 什么是shiro
```markdown
Apache Shiro™ is a powerful and easy-to-use Java 
security framework that performs authentication, 
authorization, cryptography, and session management. 
With Shiro’s easy-to-understand API, you can 
quickly and easily secure any application – from the 
smallest mobile applications to the largest web and 
enterprise applications.  

Shiro 是一个功能强大且易于使用的Java安全框架，它执行
身份验证、授权、加密和会话管理。使用Shiro易于理解的API，
您可以快速轻松地保护任何应用程序—从最小的移动应用程序到最
大的web和企业应用程序。
```
```markdown
Shiro是apache旗下一个开源框架，它将软件系统的安全认证相关
的功能抽取出来，实现用户身份认证，权限授权、加密、会话管
理等功能，组成了一个通用的安全认证框架。
```

## 3 shiro的核心架构
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022202137964.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


### 3.1 Subject
```markdown
Subject即主体，外部应用与subject进行交互，subject记录了当前
操作用户，将用户的概念理解为当前操作的主体，可能是一个通过浏
览器请求的用户，也可能是一个运行的程序。	Subject在shiro中
是一个接口，接口中定义了很多认证授相关的方法，外部程序通
过subject进行认证授，而subject是通过SecurityManager安全
管理器进行认证授权
```


### 3.2 SecurityManager
```markdown
SecurityManager即安全管理器，对全部的subject进行安全管理，
它是shiro的核心，负责对所有的subject进行安全管理。通过
SecurityManager可以完成subject的认证、授权等，实质上
SecurityManager是通过Authenticator进行认证，通过Authorizer进
行授权，通过SessionManager进行会话管理等。
SecurityManager是一个接口，继承了Authenticator, Authorizer, 
SessionManager这三个接口。
```


### 3.3 Authenticator
```markdown
Authenticator即认证器，对用户身份进行认证，Authenticator是
一个接口，shiro提供ModularRealmAuthenticator实现类，通过
ModularRealmAuthenticator基本上可以满足大多数需求，也可以
自定义认证器。
```


### 3.4 Authorizer
```markdown
Authorizer即授权器，用户通过认证器认证通过，在访问功能时
需要通过授权器判断用户是否有此功能的操作权限。
```

###  3.5 Realm
```markdown
Realm即领域，相当于datasource数据源，securityManager进行
安全认证需要通过Realm获取用户权限数据，比如：如果用户
身份数据在数据库那么realm就需要从数据库获取用户身份信息。
```

```markdown
注意：不要把realm理解成只是从数据源取数据，在realm中还
有认证授权校验的相关的代码。
```

### 3.6 SessionManager
```markdown
sessionManager即会话管理，shiro框架定义了一套会话管理，
它不依赖web容器的session，所以shiro可以使用在非web
应用上，也可以将分布式应用的会话集中在一点管理，此
特性可使它实现单点登录。
```


### 3.7 SessionDAO
```markdown
SessionDAO即会话dao，是对session会话操作的一套接
口，比如要将session存储到数据库，可以通过jdbc将会
话存储到数据库。
```


### 3.8 CacheManager
```markdown
CacheManager即缓存管理，将用户权限数据存储在缓存，
这样可以提高性能。
```


### 3.9 Cryptography
```markdown
Cryptography即密码管理，shiro提供了一套加密/解密的组件，
方便开发。比如提供常用的散列、加/解密等功能。
```
## 4 shiro中的认证

### 4.1 认证
```markdown
身份认证，就是判断一个用户是否为合法用户的处理过程。最
常用的简单身份认证方式是系统通过核对用户输入的用户名和口
令，看其是否与系统中存储的该用户的用户名和口令一致，来判
断用户身份是否正确。
```


### 4.2 shiro中认证的关键对象
```markdown
Subject：主体
访问系统的用户，主体可以是用户、程序等，进行认证的都称为主体； 
```
```markdown
Principal：身份信息
是主体（subject）进行身份认证的标识，标识必须具有唯一性，
如用户名、手机号、邮箱地址等，一个主体可以有多个身份，
但是必须有一个主身份（Primary Principal）。
```
```markdown
credential：凭证信息
是只有主体自己知道的安全信息，如密码、证书等。
```

### 4.3 认证流程
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022202256387.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

### 4.4 认证的开发
#### 4.4.1 创建项目并引入依赖

```xml
<dependency>
  <groupId>org.apache.shiro</groupId>
  <artifactId>shiro-core</artifactId>
  <version>1.5.3</version>
</dependency>
```

#### 4.4.2 引入shiro配置文件并加入如下配置

```ini
[users]
xiaochen=123
zhangsan=456
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022202325558.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

#### 4.4.3 开发认证代码

```java
package com.baizhi;

import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.realm.text.IniRealm;
import org.apache.shiro.subject.Subject;

public class TestAuthenticator {
    public static void main(String[] args) {

        //1.创建安全管理器对象
        DefaultSecurityManager securityManager = new DefaultSecurityManager();

        //2.给安全管理器设置realm
        securityManager.setRealm(new IniRealm("classpath:shiro.ini"));

        //3.SecurityUtils 给全局安全工具类设置安全管理器
        SecurityUtils.setSecurityManager(securityManager);

        //4.关键对象 subject 主体
        Subject subject = SecurityUtils.getSubject();


        //5.创建令牌
        UsernamePasswordToken token = new UsernamePasswordToken("xiaochen","123");

        try{
            System.out.println("认证状态: "+ subject.isAuthenticated());
            subject.login(token);//用户认证
            System.out.println("认证状态: "+ subject.isAuthenticated());
        }catch (UnknownAccountException e){
            e.printStackTrace();
            System.out.println("认证失败: 用户名不存在~");
        }catch (IncorrectCredentialsException e){
            e.printStackTrace();
            System.out.println("认证失败: 密码错误~");
        }

    }
}

```
```markdown
DisabledAccountException（帐号被禁用）

LockedAccountException（帐号被锁定）

ExcessiveAttemptsException（登录失败次数过多）

ExpiredCredentialsException（凭证过期）等

```


### 4.5 自定义Realm
```markdown
上边的程序使用的是Shiro自带的IniRealm，IniRealm从ini配置
文件中读取用户的信息，大部分情况下需要从系统的数据库中读
取用户信息，所以需要自定义realm。
```
#### 4.5.1 shiro提供的Realm
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022202353631.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


#### 4.5.2 根据认证源码认证使用的是SimpleAccountRealm
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022202412828.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
SimpleAccountRealm的部分源码中有两个方法一个是认证 一
个是授权
```

```java
public class SimpleAccountRealm extends AuthorizingRealm {
		//.......省略
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        UsernamePasswordToken upToken = (UsernamePasswordToken) token;
        SimpleAccount account = getUser(upToken.getUsername());

        if (account != null) {

            if (account.isLocked()) {
                throw new LockedAccountException("Account [" + account + "] is locked.");
            }
            if (account.isCredentialsExpired()) {
                String msg = "The credentials for account [" + account + "] are expired";
                throw new ExpiredCredentialsException(msg);
            }

        }

        return account;
    }

    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        String username = getUsername(principals);
        USERS_LOCK.readLock().lock();
        try {
            return this.users.get(username);
        } finally {
            USERS_LOCK.readLock().unlock();
        }
    }
}
```

#### 4.5.3 自定义realm

```java
package com.baizhi.realm;

import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;

/**
 * 自定义realm实现 将认证|授权数据的来源转为数据库的实现
 */
public class CustomerRealm extends AuthorizingRealm {
    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        System.out.println("==================");
        return null;
    }

    //认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        //在token中获取用户名
        String principal = (String) token.getPrincipal();
        System.out.println(principal);
        //根据身份信息使用jdbc mybatis查询相关数据库
        if("xiaochen".equals(principal)){
            //参数1:返回数据库中正确的用户名   //参数2:返回数据库中正确密码  //参数3:提供当前realm的名字 this.getName();
            SimpleAuthenticationInfo simpleAuthenticationInfo = new SimpleAuthenticationInfo(principal,"123",this.getName());
            return simpleAuthenticationInfo;
        }
        return null;
    }
}

```
#### 4.5.4 测试

```java
package com.baizhi;

import com.baizhi.realm.CustomerRealm;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.subject.Subject;

/**
 * 使用自定义realm
 */
public class TestCustomerRealmAuthenticator {
    public static void main(String[] args) {

        //创建securityManager
        DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager();
        //设置自定义realm
        defaultSecurityManager.setRealm(new CustomerRealm());
        //将安全工具类设置安全工具类
        SecurityUtils.setSecurityManager(defaultSecurityManager);
        //通过安全工具类获取subject
        Subject subject = SecurityUtils.getSubject();
        //创建token
        UsernamePasswordToken token = new UsernamePasswordToken("xiaochen", "123");

        try {
            subject.login(token);
            System.out.println(subject.isAuthenticated());
        } catch (UnknownAccountException e) {
            e.printStackTrace();
            System.out.println("用户名错误");
        } catch (IncorrectCredentialsException e) {
            e.printStackTrace();
            System.out.println("密码错误");
        }


        //认证用户进行授权
        if(subject.isAuthenticated()){

            //1.基于角色权限控制
            System.out.println(subject.hasRole("admin"));


        }


    }
}

```

### 4.6 使用MD5和Salt
```markdown
实际应用是将盐和散列后的值存在数据库中，自动realm从数据库取
出盐和加密后的值由shiro完成密码校验。
```

```java
package com.baizhi;

import org.apache.shiro.crypto.hash.Md5Hash;

public class TestShiroMD5 {
    public static void main(String[] args) {

        //创建一个md5算法
//        Md5Hash md5Hash = new Md5Hash();
//        md5Hash.setBytes("123".getBytes());
//        String s = md5Hash.toHex();
//        System.out.println(s);

        //使用md5
        Md5Hash md5Hash = new Md5Hash("123");

        System.out.println(md5Hash.toHex());

        //使用MD5 + salt处理
        Md5Hash md5Hash1 = new Md5Hash("123", "X0*7ps");

        System.out.println(md5Hash1.toHex());

        //使用md5 + salt + hash散列
        Md5Hash md5Hash2 = new Md5Hash("123", "X0*7ps", 1024);
        System.out.println(md5Hash2.toHex());


    }
}

```
#### 4.6.1 自定义md5+salt的realm

```java
package com.baizhi.realm;

import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.authz.SimpleAuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.apache.shiro.util.ByteSource;

/**
 * 使用自定义realm 加入md5 + salt +hash
 */
public class CustomerMd5Realm extends AuthorizingRealm {

    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        String primaryPrincipal = (String) principals.getPrimaryPrincipal();
        System.out.println("身份信息: "+primaryPrincipal);

        //根据身份信息 用户名 获取当前用户的角色信息,以及权限信息  xiaochen  admin user
        SimpleAuthorizationInfo simpleAuthorizationInfo = new SimpleAuthorizationInfo();

        //将数据库中查询角色信息赋值给权限对象
        simpleAuthorizationInfo.addRole("admin");
        simpleAuthorizationInfo.addRole("user");

        //将数据库中查询权限信息赋值个权限对象
        simpleAuthorizationInfo.addStringPermission("user:*:01");
        simpleAuthorizationInfo.addStringPermission("product:create");


        return simpleAuthorizationInfo;
    }

    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        //获取身份信息
        String principal = (String) token.getPrincipal();

        //根据用户名查询数据库
        if ("xiaochen".equals(principal)) {
            //参数1: 数据库用户名  参数2:数据库md5+salt之后的密码  参数3:注册时的随机盐  参数4:realm的名字
            return new SimpleAuthenticationInfo(principal,
                    "e4f9bf3e0c58f045e62c23c533fcf633",
                    ByteSource.Util.bytes("X0*7ps"),
                    this.getName());
        }
        return null;
    }
}

```

#### 4.6.2 测试

```java
package com.baizhi;

import com.baizhi.realm.CustomerMd5Realm;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.authc.credential.HashedCredentialsMatcher;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.subject.Subject;

import java.util.Arrays;

public class TestCustomerMd5RealmAuthenicator {

    public static void main(String[] args) {

        //创建安全管理器
        DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager();
        //注入realm
        CustomerMd5Realm realm = new CustomerMd5Realm();
        //设置realm使用hash凭证匹配器

        HashedCredentialsMatcher credentialsMatcher = new HashedCredentialsMatcher();
        //使用算法
        credentialsMatcher.setHashAlgorithmName("md5");
        //散列次数
        credentialsMatcher.setHashIterations(1024);
        realm.setCredentialsMatcher(credentialsMatcher);

        defaultSecurityManager.setRealm(realm);
        //将安全管理器注入安全工具
        SecurityUtils.setSecurityManager(defaultSecurityManager);

        //通过安全工具类获取subject
        Subject subject = SecurityUtils.getSubject();

        //认证
        UsernamePasswordToken token = new UsernamePasswordToken("xiaochen", "123");

        try {
            subject.login(token);
            System.out.println("登录成功");
        } catch (UnknownAccountException e) {
            e.printStackTrace();
            System.out.println("用户名错误");
        }catch (IncorrectCredentialsException e){
            e.printStackTrace();
            System.out.println("密码错误");
        }


        //授权
        if(subject.isAuthenticated()){

            //基于角色权限控制
            System.out.println(subject.hasRole("super"));

            //基于多角色权限控制
            System.out.println(subject.hasAllRoles(Arrays.asList("admin", "super")));

            //是否具有其中一个角色
            boolean[] booleans = subject.hasRoles(Arrays.asList("admin", "super", "user"));
            for (boolean aBoolean : booleans) {
                System.out.println(aBoolean);
            }
            System.out.println("==============================================");

            //基于权限字符串的访问控制  资源标识符:操作:资源类型
            System.out.println("权限:"+subject.isPermitted("user:update:01"));
            System.out.println("权限:"+subject.isPermitted("product:create:02"));

            //分别具有那些权限
            boolean[] permitted = subject.isPermitted("user:*:01", "order:*:10");
            for (boolean b : permitted) {
                System.out.println(b);
            }

            //同时具有哪些权限
            boolean permittedAll = subject.isPermittedAll("user:*:01", "product:create:01");
            System.out.println(permittedAll);
        }

    }
}

```

## 5 shiro中的授权

### 5.1 授权
```markdown
授权，即访问控制，控制谁能访问哪些资源。主体进行身份认
证后需要分配权限方可访问系统的资源，对于某些资源没有权
限是无法访问的。
```

### 5.2 关键对象
```markdown
授权可简 单理解为who对what(which)进行How操作：
```
```markdown
Who，即主体（Subject），主体需要访问系统中的资源。

What，即资源（Resource)，如系统菜单、页面、按钮、类方法、
系统商品信息等。资源包括资源类型和资源实例，比如商品信息
为资源类型，类型为t01的商品为资源实例，编号为001的商品信息
也属于资源实例。

How，权限/许可（Permission)，规定了主体对资源的操作许可，
权限离开资源没有意义，如用户查询权限、用户添加权限、某个
类方法的调用权限、编号为001用户的修改权限等，通过权限可
知主体对哪些资源都有哪些操作许可。
```

### 5.3 授权流程
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022202458376.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

### 5.4 授权方式
```markdown
基于角色的访问控制
RBAC基于角色的访问控制（Role-Based Access Control）是以
角色为中心进行访问控制
```
```java
if(subject.hasRole("admin")){
   //操作什么资源
}
```
```markdown
基于资源的访问控制
RBAC基于资源的访问控制（Resource-Based Access Control）
是以资源为中心进行访问控制
```

```java
if(subject.isPermission("user:update:01")){ //资源实例
//对01用户进行修改
}
if(subject.isPermission("user:update:*")){  //资源类型
//对01用户进行修改
}
```

### 5.5 权限字符串 
```markdown
权限字符串的规则是：资源标识符：操作：资源实例标识符，
意思是对哪个资源的哪个实例具有什么操作，“:”是资源/操作/实
例的分割符，权限字符串也可以使用*通配符。
```
```markdown
例子：
用户创建权限：user:create，或user:create:*
用户修改实例001的权限：user:update:001
用户实例001的所有权限：user:*：001
```
### 5.6 shiro中授权编程实现方式
```markdown
编程式
```

```java
Subject subject = SecurityUtils.getSubject();
if(subject.hasRole(“admin”)) {
	//有权限
} else {
	//无权限
}
```
```markdown
注解式
```
  ```java
  @RequiresRoles("admin")
  public void hello() {
  	//有权限
  }
  ```
```markdown
标签式
```
```jsp
JSP/GSP 标签：在JSP/GSP 页面通过相应的标签完成：
<shiro:hasRole name="admin">
<!— 有权限—>
</shiro:hasRole>
注意: Thymeleaf 中使用shiro需要额外集成!
```

### 5.7 开发授权
#### 5.7.1 realm的实现
```java
package com.baizhi.realm;

import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.authz.SimpleAuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.apache.shiro.util.ByteSource;

/**
 * 使用自定义realm 加入md5 + salt +hash
 */
public class CustomerMd5Realm extends AuthorizingRealm {

    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        String primaryPrincipal = (String) principals.getPrimaryPrincipal();
        System.out.println("身份信息: "+primaryPrincipal);

        //根据身份信息 用户名 获取当前用户的角色信息,以及权限信息  xiaochen  admin user
        SimpleAuthorizationInfo simpleAuthorizationInfo = new SimpleAuthorizationInfo();

        //将数据库中查询角色信息赋值给权限对象
        simpleAuthorizationInfo.addRole("admin");
        simpleAuthorizationInfo.addRole("user");

        //将数据库中查询权限信息赋值个权限对象
        simpleAuthorizationInfo.addStringPermission("user:*:01");
        simpleAuthorizationInfo.addStringPermission("product:create");


        return simpleAuthorizationInfo;
    }

    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        //获取身份信息
        String principal = (String) token.getPrincipal();

        //根据用户名查询数据库
        if ("xiaochen".equals(principal)) {
            //参数1: 数据库用户名  参数2:数据库md5+salt之后的密码  参数3:注册时的随机盐  参数4:realm的名字
            return new SimpleAuthenticationInfo(principal,
                    "e4f9bf3e0c58f045e62c23c533fcf633",
                    ByteSource.Util.bytes("X0*7ps"),
                    this.getName());
        }
        return null;
    }
}

```
#### 5.7.2 测试

```java
package com.baizhi;

import com.baizhi.realm.CustomerMd5Realm;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.authc.credential.HashedCredentialsMatcher;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.subject.Subject;

import java.util.Arrays;

public class TestCustomerMd5RealmAuthenicator {

    public static void main(String[] args) {

        //创建安全管理器
        DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager();
        //注入realm
        CustomerMd5Realm realm = new CustomerMd5Realm();
        //设置realm使用hash凭证匹配器

        HashedCredentialsMatcher credentialsMatcher = new HashedCredentialsMatcher();
        //使用算法
        credentialsMatcher.setHashAlgorithmName("md5");
        //散列次数
        credentialsMatcher.setHashIterations(1024);
        realm.setCredentialsMatcher(credentialsMatcher);

        defaultSecurityManager.setRealm(realm);
        //将安全管理器注入安全工具
        SecurityUtils.setSecurityManager(defaultSecurityManager);

        //通过安全工具类获取subject
        Subject subject = SecurityUtils.getSubject();

        //认证
        UsernamePasswordToken token = new UsernamePasswordToken("xiaochen", "123");

        try {
            subject.login(token);
            System.out.println("登录成功");
        } catch (UnknownAccountException e) {
            e.printStackTrace();
            System.out.println("用户名错误");
        }catch (IncorrectCredentialsException e){
            e.printStackTrace();
            System.out.println("密码错误");
        }


        //授权
        if(subject.isAuthenticated()){

            //基于角色权限控制
            System.out.println(subject.hasRole("super"));

            //基于多角色权限控制
            System.out.println(subject.hasAllRoles(Arrays.asList("admin", "super")));

            //是否具有其中一个角色
            boolean[] booleans = subject.hasRoles(Arrays.asList("admin", "super", "user"));
            for (boolean aBoolean : booleans) {
                System.out.println(aBoolean);
            }
            System.out.println("==============================================");

            //基于权限字符串的访问控制  资源标识符:操作:资源类型
            System.out.println("权限:"+subject.isPermitted("user:update:01"));
            System.out.println("权限:"+subject.isPermitted("product:create:02"));

            //分别具有那些权限
            boolean[] permitted = subject.isPermitted("user:*:01", "order:*:10");
            for (boolean b : permitted) {
                System.out.println(b);
            }

            //同时具有哪些权限
            boolean permittedAll = subject.isPermittedAll("user:*:01", "product:create:01");
            System.out.println(permittedAll);
        }

    }
}

```

## 6 整合SpringBoot项目实战

### 6.1 整合思路
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022202738807.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


### 6.2 引入shiro依赖

```xml
<dependency>
  <groupId>org.apache.shiro</groupId>
  <artifactId>shiro-spring-boot-starter</artifactId>
  <version>1.5.3</version>
</dependency>
```

### 6.3 配置shiro环境

#### 6.3.1 创建配置类
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102220281641.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
配置shiroFilterFactoryBean
```
```java
@Bean
public ShiroFilterFactoryBean getShiroFilterFactoryBean(SecurityManager securityManager){
  //创建shiro的filter
  ShiroFilterFactoryBean shiroFilterFactoryBean = new ShiroFilterFactoryBean();
  //注入安全管理器
  shiroFilterFactoryBean.setSecurityManager(securityManager);
 	
  return shiroFilterFactoryBean;
}
```
```markdown
配置WebSecurityManager
```
```java
@Bean
public DefaultWebSecurityManager getSecurityManager(Realm realm){
  DefaultWebSecurityManager defaultWebSecurityManager = new DefaultWebSecurityManager();
  defaultWebSecurityManager.setRealm(realm);
  return defaultWebSecurityManager;
}
```
```markdown
创建自定义realm
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022203001696.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
public class CustomerRealm extends AuthorizingRealm {
    //处理授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        return null;
    }
		//处理认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws 
      																																		AuthenticationException {
        return null;
    }
}
```
```markdown
配置自定义realm
```
```java
//创建自定义realm
@Bean
public Realm getRealm(){
  return new CustomerRealm();
}
```
```markdown
编写控制器跳转至index.html
```
```java
@Controller
public class IndexController {
    @RequestMapping("index")
    public String index(){
        System.out.println("跳转至主页");
        return "index";
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022203027889.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022203044183.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
启动springboot应用访问index
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022203103246.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
注意:
默认在配置好shiro环境后默认环境中没有对项目中任何资源进
行权限控制,所有现在项目中所有资源都可以通过路径访问
```

```markdown
加入权限控制
```
```markdown
修改ShiroFilterFactoryBean配置
```

```java
  //注入安全管理器
  shiroFilterFactoryBean.setSecurityManager(securityManager);
  Map<String,String> map =  new LinkedHashMap<>();
  map.put("/index.jsp","authc");
  //配置认证和授权规则
  shiroFilterFactoryBean.setFilterChainDefinitionMap(map);
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022203143281.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
/**代表拦截项目中一切资源 ，authc代表shiro中的一个filter的
别名,详细内容看文档的shirofilter列表
注意：shiro认证页面路径为/login.jsp.如果认证没通过，会跳
到这个页面。
```
### 6.4 常见过滤器
```markdown
注意: shiro提供和多个默认的过滤器，我们可以用这些过滤器来配
置控制指定url的权限：
```


| 配置缩写          | 对应的过滤器                   | 功能                                                         |
| ----------------- | ------------------------------ | ------------------------------------------------------------ |
| anon              | AnonymousFilter                | 指定url可以匿名访问                                          |
| authc             | FormAuthenticationFilter       | 指定url需要form表单登录，默认会从请求中获取`username`、`password`,`rememberMe`等参数并尝试登录，如果登录不了就会跳转到loginUrl配置的路径。我们也可以用这个过滤器做默认的登录逻辑，但是一般都是我们自己在控制器写登录逻辑的，自己写的话出错返回的信息都可以定制嘛。 |
| authcBasic        | BasicHttpAuthenticationFilter  | 指定url需要basic登录                                         |
| logout            | LogoutFilter                   | 登出过滤器，配置指定url就可以实现退出功能，非常方便          |
| noSessionCreation | NoSessionCreationFilter        | 禁止创建会话                                                 |
| perms             | PermissionsAuthorizationFilter | 需要指定权限才能访问                                         |
| port              | PortFilter                     | 需要指定端口才能访问                                         |
| rest              | HttpMethodPermissionFilter     | 将http请求方法转化成相应的动词来构造一个权限字符串，这个感觉意义不大，有兴趣自己看源码的注释 |
| roles             | RolesAuthorizationFilter       | 需要指定角色才能访问                                         |
| ssl               | SslFilter                      | 需要https请求才能访问                                        |
| user              | UserFilter                     | 需要已登录或“记住我”的用户才能访问                           |

### 6.5 认证实现
```markdown
在login.jsp中开发认证界面
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022203305241.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```html
<form action="${pageContext.request.contextPath}/user/login" method="post">
  用户名:<input type="text" name="username" > <br/>
  密码  : <input type="text" name="password"> <br>
  <input type="submit" value="登录">
</form>
```
```markdown
开发controller
```
```java
@Controller
@RequestMapping("user")
public class UserController {
  /**
    * 用来处理身份认证
    * @param username
    * @param password
    * @return
    */
  @RequestMapping("login")
  public String login(String username,String password){
    //获取主体对象
    Subject subject = SecurityUtils.getSubject();
    try {
      subject.login(new UsernamePasswordToken(username,password));
      return  "redirect:/index.jsp";
    } catch (UnknownAccountException e) {
      e.printStackTrace();
      System.out.println("用户名错误!");
    }catch (IncorrectCredentialsException e){
      e.printStackTrace();
      System.out.println("密码错误!");
    }
    return "redirect:/login.jsp";
  }
}
```
```markdown
## 注意：当注册WebSecurityManager时候，我们不需要利用
SecurityUtils注册
WebSecurityManager。因为当配置WebSecurityManager的时候。
会自动注册到SecurityUtils
```
```markdown
开发realm中返回静态数据(未连接数据库)
```
```java
@Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        System.out.println("==========================");
        String principal = (String) token.getPrincipal();
        if("xiaochen".equals(principal)){
            return new SimpleAuthenticationInfo(principal,"123",this.getName());
        }
        return null;
    }
}
```
```markdown
启动项目以realm中定义静态数据进行认证
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022203332676.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022203352349.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022203414921.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
认证功能没有md5和随机盐的认证就实现啦
```
### 6.6 退出认证
```markdown
开发页面退出连接
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102220343686.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
开发controller
```
```java
@Controller
@RequestMapping("user")
public class UserController {
  /**
    * 退出登录
    *
    */
  @RequestMapping("logout")
  public String logout(){
    Subject subject = SecurityUtils.getSubject();
    subject.logout();//退出用户
    return "redirect:/login.jsp";
  }
}
```
```markdown
修改退出连接访问退出路径
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022203502981.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
退出之后访问受限资源立即返回认证界面
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022203522635.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

### 6.7 MD5、Salt的认证实现
#### 6.7.1 开发数据库注册
```markdown
开发注册界面
```
```html
<h1>用户注册</h1>
<form action="${pageContext.request.contextPath}/user/register" method="post">
  用户名:<input type="text" name="username" > <br/>
  密码  : <input type="text" name="password"> <br>
  <input type="submit" value="立即注册">
</form>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022203543395.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
创建数据表结构
```
```sql
SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;
-- ----------------------------
-- Table structure for t_user
-- ----------------------------
DROP TABLE IF EXISTS `t_user`;
CREATE TABLE `t_user` (
  `id` int(6) NOT NULL AUTO_INCREMENT,
  `username` varchar(40) DEFAULT NULL,
  `password` varchar(40) DEFAULT NULL,
  `salt` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;

SET FOREIGN_KEY_CHECKS = 1;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022203600965.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
项目引入依赖
```

```xml
<!--mybatis相关依赖-->
<dependency>
  <groupId>org.mybatis.spring.boot</groupId>
  <artifactId>mybatis-spring-boot-starter</artifactId>
  <version>2.1.2</version>
</dependency>

<!--mysql-->
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>5.1.38</version>
</dependency>


<!--druid-->
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>druid</artifactId>
  <version>1.1.19</version>
</dependency>
```
```markdown
配置application.properties配置文件
```
```properties
server.port=8888
server.servlet.context-path=/shiro
spring.application.name=shiro

spring.mvc.view.prefix=/
spring.mvc.view.suffix=.jsp
#新增配置
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/shiro?characterEncoding=UTF-8
spring.datasource.username=root
spring.datasource.password=root


mybatis.type-aliases-package=com.baizhi.springboot_jsp_shiro.entity
mybatis.mapper-locations=classpath:com/baizhi/mapper/*.xml

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022203626541.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
创建entity
```


```java
@Data
@Accessors(chain = true)
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private String  id;
    private String username;
    private String password;
    private String salt;
}
```
```markdown
创建DAO接口
```

```java
@Mapper
public interface UserDAO {
    void save(User user);
}
```
```markdown
开发mapper配置文件
```
```xml
<insert id="save" parameterType="User" useGeneratedKeys="true" keyProperty="id">
  insert into t_user values(#{id},#{username},#{password},#{salt})
</insert>
```
```markdown
开发service接口
```
```java
public interface UserService {
    //注册用户方法
    void register(User user);
}
```
```markdown
创建salt工具类
```
```java
public class SaltUtils {
    /**
     * 生成salt的静态方法
     * @param n
     * @return
     */
    public static String getSalt(int n){
        char[] chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz01234567890!@#$%^&*()".toCharArray();
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n; i++) {
            char aChar = chars[new Random().nextInt(chars.length)];
            sb.append(aChar);
        }
        return sb.toString();
    }
}
```
```markdown
开发service实现类
```
```java
@Service
@Transactional
public class UserServiceImpl implements UserService {

    @Autowired
    private UserDAO userDAO;

    @Override
    public void register(User user) {
        //处理业务调用dao
        //1.生成随机盐
        String salt = SaltUtils.getSalt(8);
        //2.将随机盐保存到数据
        user.setSalt(salt);
        //3.明文密码进行md5 + salt + hash散列
        Md5Hash md5Hash = new Md5Hash(user.getPassword(),salt,1024);
        user.setPassword(md5Hash.toHex());
        userDAO.save(user);
    }
}
```
```markdown
开发Controller
```
```java
@Controller
@RequestMapping("user")
public class UserController {

    @Autowired
    private UserService userService;

    /**
     * 用户注册
     */
    @RequestMapping("register")
    public String register(User user) {
        try {
            userService.register(user);
            return "redirect:/login.jsp";
        }catch (Exception e){
            e.printStackTrace();
            return "redirect:/register.jsp";
        }
    }
}
```
```markdown
启动项目进行注册
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022203652486.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


#### 6.7.2 开发数据库认证
```markdown
开发DAO
```


```java
@Mapper
public interface UserDAO {

    void save(User user);
		//根据身份信息认证的方法
    User findByUserName(String username);
}
```
```markdown
开发mapper配置文件
```


```xml
<select id="findByUserName" parameterType="String" resultType="User">
  select id,username,password,salt from t_user
  where username = #{username}
</select>
```
```markdown
开发Service接口
```

```java
public interface UserService {
    //注册用户方法
    void register(User user);
    //根据用户名查询业务的方法
    User findByUserName(String username);
}
```
```markdown
开发Service实现类
```
```java
@Service("userService")
@Transactional
public class UserServiceImpl implements UserService {
    @Autowired
    private UserDAO userDAO;
    @Override
    public User findByUserName(String username) {
        return userDAO.findByUserName(username);
    }
}
```
```markdown
开发在工厂中获取bean对象的工具类
```
```java
@Component
public class ApplicationContextUtils implements ApplicationContextAware {

    private static ApplicationContext context;

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        this.context = applicationContext;
    }


    //根据bean名字获取工厂中指定bean 对象
    public static Object getBean(String beanName){
        return context.getBean(beanName);
    }
}
```
```markdown
修改自定义realm
```
```java
 @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        System.out.println("==========================");

        //根据身份信息
        String principal = (String) token.getPrincipal();
        //在工厂中获取service对象
        UserService userService = (UserService) ApplicationContextUtils.getBean("userService");
				//根据身份信息查询
        User user = userService.findByUserName(principal);

        if(!ObjectUtils.isEmpty(user)){
            //返回数据库信息
            return new SimpleAuthenticationInfo(user.getUsername(),user.getPassword(), 
                                               ByteSource.Util.bytes(user.getSalt()),this.getName());
        }
        return null;
    }
```
```markdown
修改ShiroConfig中realm使用凭证匹配器以及hash散列
```


```java
@Bean
public Realm getRealm(){
  CustomerRealm customerRealm = new CustomerRealm();
  //设置hashed凭证匹配器
  HashedCredentialsMatcher credentialsMatcher = new HashedCredentialsMatcher();
  //设置md5加密
  credentialsMatcher.setHashAlgorithmName("md5");
  //设置散列次数
  credentialsMatcher.setHashIterations(1024);
  customerRealm.setCredentialsMatcher(credentialsMatcher);
  return customerRealm;
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022203727836.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


### 6.8 授权实现
```markdown
页面资源授权
```

```xml
<%@taglib prefix="shiro" uri="http://shiro.apache.org/tags" %>

<shiro:hasAnyRoles name="user,admin">
        <li><a href="">用户管理</a>
            <ul>
                <shiro:hasPermission name="user:add:*">
                <li><a href="">添加</a></li>
                </shiro:hasPermission>
                <shiro:hasPermission name="user:delete:*">
                    <li><a href="">删除</a></li>
                </shiro:hasPermission>
                <shiro:hasPermission name="user:update:*">
                    <li><a href="">修改</a></li>
                </shiro:hasPermission>
                <shiro:hasPermission name="user:find:*">
                    <li><a href="">查询</a></li>
                </shiro:hasPermission>
            </ul>
        </li>
        </shiro:hasAnyRoles>
        <shiro:hasRole name="admin">
            <li><a href="">商品管理</a></li>
            <li><a href="">订单管理</a></li>
            <li><a href="">物流管理</a></li>
        </shiro:hasRole>
```
```markdown
代码方式授权
```
```java
@RequestMapping("save")
public String save(){
  System.out.println("进入方法");
  //获取主体对象
  Subject subject = SecurityUtils.getSubject();
  //代码方式
  if (subject.hasRole("admin")) {
    System.out.println("保存订单!");
  }else{
    System.out.println("无权访问!");
  }
  //基于权限字符串
  //....
  return "redirect:/index.jsp";
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022203837412.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
方法调用授权
@RequiresRoles          用来基于角色进行授权
@RequiresPermissions    用来基于权限进行授权
```

```java
@RequiresRoles(value={"admin","user"})//用来判断角色  同时具有 admin user
@RequiresPermissions("user:update:01") //用来判断权限字符串
@RequestMapping("save")
public String save(){
  System.out.println("进入方法");
  return "redirect:/index.jsp";
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102220390268.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
授权数据持久化
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022203922755.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```sql
SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for t_pers
-- ----------------------------
DROP TABLE IF EXISTS `t_pers`;
CREATE TABLE `t_pers` (
  `id` int(6) NOT NULL AUTO_INCREMENT,
  `name` varchar(80) DEFAULT NULL,
  `url` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- ----------------------------
-- Table structure for t_role
-- ----------------------------
DROP TABLE IF EXISTS `t_role`;
CREATE TABLE `t_role` (
  `id` int(6) NOT NULL AUTO_INCREMENT,
  `name` varchar(60) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- ----------------------------
-- Table structure for t_role_perms
-- ----------------------------
DROP TABLE IF EXISTS `t_role_perms`;
CREATE TABLE `t_role_perms` (
  `id` int(6) NOT NULL,
  `roleid` int(6) DEFAULT NULL,
  `permsid` int(6) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- ----------------------------
-- Table structure for t_user
-- ----------------------------
DROP TABLE IF EXISTS `t_user`;
CREATE TABLE `t_user` (
  `id` int(6) NOT NULL AUTO_INCREMENT,
  `username` varchar(40) DEFAULT NULL,
  `password` varchar(40) DEFAULT NULL,
  `salt` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Table structure for t_user_role
-- ----------------------------
DROP TABLE IF EXISTS `t_user_role`;
CREATE TABLE `t_user_role` (
  `id` int(6) NOT NULL,
  `userid` int(6) DEFAULT NULL,
  `roleid` int(6) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

SET FOREIGN_KEY_CHECKS = 1;

```

```markdown
创建dao方法
```


```java
 //根据用户名查询所有角色
User findRolesByUserName(String username);
//根据角色id查询权限集合
List<Perms> findPermsByRoleId(String id);
```
```markdown
mapper实现
```
```xml
<resultMap id="userMap" type="User">
  <id column="uid" property="id"/>
  <result column="username" property="username"/>
  <!--角色信息-->
  <collection property="roles" javaType="list" ofType="Role">
    <id column="id" property="id"/>
    <result column="rname" property="name"/>
  </collection>
</resultMap>

<select id="findRolesByUserName" parameterType="String" resultMap="userMap">
  SELECT u.id uid,u.username,r.id,r.NAME rname
  FROM t_user u
  LEFT JOIN t_user_role ur
  ON u.id=ur.userid
  LEFT JOIN t_role r
  ON ur.roleid=r.id
  WHERE u.username=#{username}
</select>

<select id="findPermsByRoleId" parameterType="String" resultType="Perms">
  SELECT p.id,p.NAME,p.url,r.NAME
  FROM t_role r
  LEFT JOIN t_role_perms rp
  ON r.id=rp.roleid
  LEFT JOIN t_perms p ON rp.permsid=p.id
  WHERE r.id=#{id}
</select>
```
```markdown
Service接口
```
```java
//根据用户名查询所有角色
User findRolesByUserName(String username);
//根据角色id查询权限集合
List<Perms> findPermsByRoleId(String id);
```

```markdown
Service实现
```
```java
@Override
public List<Perms> findPermsByRoleId(String id) {
  return userDAO.findPermsByRoleId(id);
}

@Override
public User findRolesByUserName(String username) {
  return userDAO.findRolesByUserName(username);
}
```
```markdown
修改自定义realm
```
```java
public class CustomerRealm extends AuthorizingRealm {
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        //获取身份信息
        String primaryPrincipal = (String) principals.getPrimaryPrincipal();
        System.out.println("调用授权验证: "+primaryPrincipal);
        //根据主身份信息获取角色 和 权限信息
        UserService userService = (UserService) ApplicationContextUtils.getBean("userService");
        User user = userService.findRolesByUserName(primaryPrincipal);
        //授权角色信息
        if(!CollectionUtils.isEmpty(user.getRoles())){
            SimpleAuthorizationInfo simpleAuthorizationInfo = new SimpleAuthorizationInfo();
            user.getRoles().forEach(role->{
                simpleAuthorizationInfo.addRole(role.getName());
                //权限信息
                List<Perms> perms = userService.findPermsByRoleId(role.getId());
                if(!CollectionUtils.isEmpty(perms)){
                    perms.forEach(perm->{
                        simpleAuthorizationInfo.addStringPermission(perm.getName());
                    });
                }
            });
            return simpleAuthorizationInfo;
        }
        return null;
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022203956763.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

### 6.9 使用CacheManager
```markdown
Cache 作用
```
```markdown
Cache 缓存: 计算机内存中一段数据
作用: 用来减轻DB的访问压力,从而提高系统的查询效率
流程: 
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022204044475.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

#### 6.9.1 使用shiro中默认EhCache实现缓存
```markdown
引入依赖
```

```xml
<!--引入shiro和ehcache-->
<dependency>
  <groupId>org.apache.shiro</groupId>
  <artifactId>shiro-ehcache</artifactId>
  <version>1.5.3</version>
</dependency>
```
```markdown
开启缓存
```
```java
//3.创建自定义realm
    @Bean
    public Realm getRealm(){
        CustomerRealm customerRealm = new CustomerRealm();
        //修改凭证校验匹配器
        HashedCredentialsMatcher credentialsMatcher = new HashedCredentialsMatcher();
        //设置加密算法为md5
        credentialsMatcher.setHashAlgorithmName("MD5");
        //设置散列次数
        credentialsMatcher.setHashIterations(1024);
        customerRealm.setCredentialsMatcher(credentialsMatcher);

        //开启缓存管理
        customerRealm.setCacheManager(new RedisCacheManager());
        customerRealm.setCachingEnabled(true);//开启全局缓存
        customerRealm.setAuthenticationCachingEnabled(true);//认证认证缓存
        customerRealm.setAuthenticationCacheName("authenticationCache");
        customerRealm.setAuthorizationCachingEnabled(true);//开启授权缓存
        customerRealm.setAuthorizationCacheName("authorizationCache");
        return customerRealm;
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022204113846.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
启动刷新页面进行测试
注意:如果控制台没有任何sql展示说明缓存已经开启
```

#### 6.9.2 shiro中使用Redis作为缓存实现
```markdown
引入redis依赖
```

```xml
<!--redis整合springboot-->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```
```markdown
配置redis连接
```

```properties
spring.redis.port=6379
spring.redis.host=localhost
spring.redis.database=0
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102220432281.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
启动redis服务
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022204342224.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
开发RedisCacheManager
```
```java
public class RedisCacheManager implements CacheManager {
    @Override
    public <K, V> Cache<K, V> getCache(String cacheName) throws CacheException {
        System.out.println("缓存名称: "+cacheName);
        return new RedisCache<K,V>(cacheName);
    }
}
```
```markdown
开RedisCache实现
```
```java
public class RedisCache<K,V> implements Cache<K,V> {

    private String cacheName;

    public RedisCache() {
    }

    public RedisCache(String cacheName) {
        this.cacheName = cacheName;
    }

    @Override
    public V get(K k) throws CacheException {
        System.out.println("获取缓存:"+ k);
        return (V) getRedisTemplate().opsForHash().get(this.cacheName,k.toString());
    }

    @Override
    public V put(K k, V v) throws CacheException {
        System.out.println("设置缓存key: "+k+" value:"+v);
        getRedisTemplate().opsForHash().put(this.cacheName,k.toString(),v);
        return null;
    }

    @Override
    public V remove(K k) throws CacheException {
        return (V) getRedisTemplate().opsForHash().delete(this.cacheName,k.toString());
    }

    @Override
    public v remove(k k) throws CacheException {
        return (v) getRedisTemplate().opsForHash().delete(this.cacheName,k.toString());
    }

    @Override
    public void clear() throws CacheException {
        getRedisTemplate().delete(this.cacheName);
    }

    @Override
    public int size() {
        return getRedisTemplate().opsForHash().size(this.cacheName).intValue();
    }

    @Override
    public Set<k> keys() {
        return getRedisTemplate().opsForHash().keys(this.cacheName);
    }

    @Override
    public Collection<v> values() {
        return getRedisTemplate().opsForHash().values(this.cacheName);
    }

    private RedisTemplate getRedisTemplate(){
        RedisTemplate redisTemplate = (RedisTemplate) ApplicationContextUtils.getBean("redisTemplate");
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        return redisTemplate;
    }


    //封装获取redisTemplate
    private RedisTemplate getRedisTemplate(){
        RedisTemplate redisTemplate = (RedisTemplate) ApplicationContextUtils.getBean("redisTemplate");
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        return redisTemplate;
    }
}
```
```markdown
启动项目测试发现报错
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022204410560.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022204437324.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
错误解释: 由于shiro中提供的simpleByteSource实现没有实现
序列化,所有在认证时出现错误信息
```
```markdown
解决方案: 需要自动salt实现序列化
自定义salt实现序列化
```

```java
 //自定义salt实现  实现序列化接口
 public class MyByteSource extends SimpleByteSource implements Serializable {
     public MyByteSource(String string) {
         super(string);
     }
 }
```
```markdown
在realm中使用自定义salt
```
```java
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
      System.out.println("==========================");
      //根据身份信息
      String principal = (String) token.getPrincipal();
      //在工厂中获取service对象
      UserService userService = (UserService) ApplicationContextUtils.getBean("userService");
      User user = userService.findByUserName(principal);
      if(!ObjectUtils.isEmpty(user)){
        return new SimpleAuthenticationInfo(user.getUsername(),user.getPassword(), 
                                          new MyByteSource(user.getSalt()),this.getName());
      }
      return null;
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022204507918.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```java
再次启动测试,发现可以成功放入redis缓存
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022204605306.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


#### 6.9.3 加入验证码验证
```markdown
开发页面加入验证码
```

```markdown
开发控制器
```
```java
  @RequestMapping("getImage")
  public void getImage(HttpSession session, HttpServletResponse response) throws IOException {
    //生成验证码
    String code = VerifyCodeUtils.generateVerifyCode(4);
    //验证码放入session
    session.setAttribute("code",code);
    //验证码存入图片
    ServletOutputStream os = response.getOutputStream();
    response.setContentType("image/png");
    VerifyCodeUtils.outputImage(220,60,os,code);
  }
```
```markdown
放行验证码
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102220463920.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

- 开发页面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022204705577.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
修改认证流程
```
```java
  @RequestMapping("login")
      public String login(String username, String password,String code,HttpSession session) {
          //比较验证码
          String codes = (String) session.getAttribute("code");
          try {
              if (codes.equalsIgnoreCase(code)){
                  //获取主体对象
                  Subject subject = SecurityUtils.getSubject();
                      subject.login(new UsernamePasswordToken(username, password));
                      return "redirect:/index.jsp";
              }else{
                  throw new RuntimeException("验证码错误!");
              }
          } catch (UnknownAccountException e) {
              e.printStackTrace();
              System.out.println("用户名错误!");
          } catch (IncorrectCredentialsException e) {
              e.printStackTrace();
              System.out.println("密码错误!");
          }catch (Exception e){
              e.printStackTrace();
              System.out.println(e.getMessage());
          }
          return "redirect:/login.jsp";
      }
```
```markdown
修改salt不能序列化的问题
```
```java
  //自定义salt实现  实现序列化接口
  public class MyByteSource implements ByteSource,Serializable {
  
      private  byte[] bytes;
      private String cachedHex;
      private String cachedBase64;
  
      //加入无参数构造方法实现序列化和反序列化
      public MyByteSource(){
  
      }
  
      public MyByteSource(byte[] bytes) {
          this.bytes = bytes;
      }
  
      public MyByteSource(char[] chars) {
          this.bytes = CodecSupport.toBytes(chars);
      }
  
      public MyByteSource(String string) {
          this.bytes = CodecSupport.toBytes(string);
      }
  
      public MyByteSource(ByteSource source) {
          this.bytes = source.getBytes();
      }
  
      public MyByteSource(File file) {
          this.bytes = (new MyByteSource.BytesHelper()).getBytes(file);
      }
  
      public MyByteSource(InputStream stream) {
          this.bytes = (new MyByteSource.BytesHelper()).getBytes(stream);
      }
  
      public static boolean isCompatible(Object o) {
          return o instanceof byte[] || o instanceof char[] || o instanceof String || o instanceof ByteSource || o instanceof File || o instanceof InputStream;
      }
  
      public byte[] getBytes() {
          return this.bytes;
      }
  
      public boolean isEmpty() {
          return this.bytes == null || this.bytes.length == 0;
      }
  
      public String toHex() {
          if (this.cachedHex == null) {
              this.cachedHex = Hex.encodeToString(this.getBytes());
          }
  
          return this.cachedHex;
      }
  
      public String toBase64() {
          if (this.cachedBase64 == null) {
              this.cachedBase64 = Base64.encodeToString(this.getBytes());
          }
  
          return this.cachedBase64;
      }
  
      public String toString() {
          return this.toBase64();
      }
  
      public int hashCode() {
          return this.bytes != null && this.bytes.length != 0 ? Arrays.hashCode(this.bytes) : 0;
      }
  
      public boolean equals(Object o) {
          if (o == this) {
              return true;
          } else if (o instanceof ByteSource) {
              ByteSource bs = (ByteSource)o;
              return Arrays.equals(this.getBytes(), bs.getBytes());
          } else {
              return false;
          }
      }
  
      private static final class BytesHelper extends CodecSupport {
          private BytesHelper() {
          }
  
          public byte[] getBytes(File file) {
              return this.toBytes(file);
          }
  
          public byte[] getBytes(InputStream stream) {
              return this.toBytes(stream);
          }
      }
  }
```


## 7 Shiro整合springboot之thymeleaf权限控制
### 7.1 引入扩展依赖
```xml
<dependency>
    <groupId>com.github.theborakompanioni</groupId>
    <artifactId>thymeleaf-extras-shiro</artifactId>
    <version>2.0.0</version>
</dependency>
```

### 7.2 页面中引入命名空间


```html
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="http://www.thymeleaf.org"
      xmlns:shiro="http://www.pollix.at/thymeleaf/shiro">
......
```
### 7.3 常见权限控制标签使用

```html
<!-- 验证当前用户是否为“访客”，即未认证（包含未记住）的用户。 -->
<p shiro:guest="">Please <a href="login.html">login</a></p>


<!-- 认证通过或已记住的用户。 -->
<p shiro:user="">
    Welcome back John! Not John? Click <a href="login.html">here</a> to login.
</p>

<!-- 已认证通过的用户。不包含已记住的用户，这是与user标签的区别所在。 -->
<p shiro:authenticated="">
    Hello, <span shiro:principal=""></span>, how are you today?
</p>
<a shiro:authenticated="" href="updateAccount.html">Update your contact information</a>

<!-- 输出当前用户信息，通常为登录帐号信息。 -->
<p>Hello, <shiro:principal/>, how are you today?</p>


<!-- 未认证通过用户，与authenticated标签相对应。与guest标签的区别是，该标签包含已记住用户。 -->
<p shiro:notAuthenticated="">
    Please <a href="login.html">login</a> in order to update your credit card information.
</p>

<!-- 验证当前用户是否属于该角色。 -->
<a shiro:hasRole="admin" href="admin.html">Administer the system</a><!-- 拥有该角色 -->

<!-- 与hasRole标签逻辑相反，当用户不属于该角色时验证通过。 -->
<p shiro:lacksRole="developer"><!-- 没有该角色 -->
    Sorry, you are not allowed to developer the system.
</p>

<!-- 验证当前用户是否属于以下所有角色。 -->
<p shiro:hasAllRoles="developer, 2"><!-- 角色与判断 -->
    You are a developer and a admin.
</p>

<!-- 验证当前用户是否属于以下任意一个角色。  -->
<p shiro:hasAnyRoles="admin, vip, developer,1"><!-- 角色或判断 -->
    You are a admin, vip, or developer.
</p>

<!--验证当前用户是否拥有指定权限。  -->
<a shiro:hasPermission="userInfo:add" href="createUser.html">添加用户</a><!-- 拥有权限 -->

<!-- 与hasPermission标签逻辑相反，当前用户没有制定权限时，验证通过。 -->
<p shiro:lacksPermission="userInfo:del"><!-- 没有权限 -->
    Sorry, you are not allowed to delete user accounts.
</p>

<!-- 验证当前用户是否拥有以下所有角色。 -->
<p shiro:hasAllPermissions="userInfo:view, userInfo:add"><!-- 权限与判断 -->
    You can see or add users.
</p>

<!-- 验证当前用户是否拥有以下任意一个权限。  -->
<p shiro:hasAnyPermissions="userInfo:view, userInfo:del"><!-- 权限或判断 -->
    You can see or delete users.
</p>
<a shiro:hasPermission="pp" href="createUser.html">Create a new User</a>

```

### 7.4 加入shiro的方言配置
```markdown
页面标签不起作用一定要记住加入方言处理
```


```java
@Bean(name = "shiroDialect")
public ShiroDialect shiroDialect(){
  return new ShiroDialect();
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102220501515.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
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
微信公众号二维码。或者搜索微信公众号-Java大世界。回复shjwt
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


