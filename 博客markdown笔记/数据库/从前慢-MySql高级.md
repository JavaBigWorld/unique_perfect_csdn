# MySql高级
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210717155926667.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
## MySql高级
### 1 MYSQL用户管理
~~~markdown
修改密码
方法1： 用SET PASSWORD命令 
首先登录MySQL。 
格式：mysql> set password for 用户名@localhost = password('新密码'); 
例子：mysql> set password for root@localhost = password('123'); 

方法2：用mysqladmin 
格式：mysqladmin -u用户名 -p旧密码 password 新密码 
例子：mysqladmin -uroot -p123456 password 123 

方法3：用UPDATE直接编辑user表 
首先登录MySQL。 
mysql> use mysql; 
mysql> update user set password=password('123') where user='root' 
and host='localhost'; 
mysql> flush privileges; 

方法4：在忘记root密码的时候，可以这样 
以windows为例： 
1. 关闭正在运行的MySQL服务。 
2. 打开DOS窗口，转到mysql\bin目录。 
3. 输入mysqld --skip-grant-tables 回车。--skip-grant-tables 的意思是启动MySQL
服务的时候跳过权限表认证。 
4. 再开一个DOS窗口（因为刚才那个DOS窗口已经不能动了），转到mysql\bin目录。 
5. 输入mysql回车，如果成功，将出现MySQL提示符 >。 
6. 连接权限数据库： use mysql; 。 
7. 改密码：update user set password=password("123")
8.  where user="root";（别忘了最后加分号） 。 
9. 刷新权限（必须步骤）：flush privileges;　。 
10. 退出 quit。 
11. 注销系统，再进入，使用用户名root和刚才设置的新密码123登录。

删除MySQL
1、双击安装包，点击下一步，然后点击remove。卸载。
2、手动删除Program Files中的MySQL目录。
3、手动删除ProgramData目录（这个目录是隐藏的。）中的MySQL。
~~~


#### 1.1 创建用户
```markdown
创建用户
create user zhang3 identified by '123123';
表示创建名称为zhang3的用户，密码设为123123；
```
#### 1.2 了解User表
```markdown
查看用户
select host,user,password,select_priv,insert_priv,drop_priv from mysql.user;

select * from user\G;
将 user 中的数据以行的形式显示出来(针对列很长的表可以采用这个方法 )
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130143618944.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
host ：   表示连接类型
      % 表示所有远程通过 TCP方式的连接
      IP 地址 如 (192.168.1.2,127.0.0.1) 通过制定ip地址进行的TCP方式的连接
     机器名   通过制定i网络中的机器名进行的TCP方式的连接
      ::1   IPv6的本地ip地址  等同于IPv4的 127.0.0.1
      localhost 本地方式通过命令行方式的连接 ，比如mysql -u xxx -p 123xxx 方式的连接。
User:表示用户名
     同一用户通过不同方式链接的权限是不一样的。

password ： 密码
     所有密码串通过 password(明文字符串) 生成的密文字符串。加密算法为MYSQLSHA1 ，不可逆 。
      mysql 5.7 的密码保存到 authentication_string 字段中不再使用password 字段。

select_priv , insert_priv等 
      为该用户所拥有的权限。

```
#### 1.3 设置密码
```markdown
修改当前用户的密码:
set password =password('123456')
 
修改某个用户的密码:
update mysql.user set password=password('123456') where user='li4';
flush privileges;   #所有通过user表的修改，必须用该命令才能生效。
```
#### 1.4 修改用户
```markdown
修改用户名：
update mysql.user set user='li4' where user='wang5';
flush privileges;   #所有通过user表的修改，必须用该命令才能生效。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130144104133.png)
#### 1.5 删除用户
```markdown
drop user li4 ;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130144158391.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
###  2 权限管理
#### 2.1 授予权限
```markdown
授权命令： 
grant 权限1,权限2,…权限n on 数据库名称.表名称 to 用户名@用户地址 identified by ‘连接口令’;
该权限如果发现没有该用户，则会直接新建一个用户。
比如  
grant select,insert,delete,drop on cqmdb.* to li4@localhost  ;
#给li4用户用本地命令行方式下，授予cqmdb这个库下的所有表的插删改查的权限。

grant all privileges on *.* to joe@'%'  identified by '123'; 
#授予通过网络方式登录的的joe用户 ，对所有库所有表的全部权限，密码设为123.
就算 all privileges 了所有权限，grant_priv 权限也只有 root 才能拥有。

给 root 赋连接口令 grant all privileges on *.* to root@'%'  ;后新建的连接没有密码，需要设置密码才能远程连接。
update user set password=password('root') where user='root' and host='%';
```
####  2.2 收回权限
```markdown
授权命令： 
revoke  权限1,权限2,…权限n on 数据库名称.表名称  from  用户名@用户地址 ;

REVOKE ALL PRIVILEGES ON mysql.* FROM joe@localhost;
#若赋的全库的表就 收回全库全表的所有权限

REVOKE select,insert,update,delete ON mysql.* FROM joe@localhost;
#收回mysql库下的所有表的插删改查权限
对比赋予权限的方法。
必须用户重新登录后才能生效
```
####  2.3 查看权限  
```markdown
查看当前用户权限
show grants;
 
查看某用户的全局权限
select  * from user ;
 
查看某用户的某库的权限
select * from  db;
 
查看某用户的某个表的权限
select * from tables_priv;

```
####  2.4 通过工具远程访问
```markdown
1、先 ping 一下数据库服务器的ip 地址确认网络畅通。
 
2、关闭数据库服务的防火墙
    service iptables stop
 
3、 确认Mysql中已经有可以通过远程登录的账户
    select  * from mysql.user where user='li4' and host='%';
 
如果没有用户,先执行如下命令：
    grant all privileges on *.*  to li4@'%'  identified by '123123';
 
4、测试连接：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130144732412.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 3 mysql的一些杂项配置
#### 3.1 大小写问题
```markdown
 SHOW VARIABLES LIKE '%lower_case_table_names%' 
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130145104135.png)

```markdown
windows系统默认大小写不敏感，但是linux系统是大小写敏感的
默认为0，大小写敏感。
设置1，大小写不敏感。创建的表，数据库都是以小写形式存放在磁盘上，
对于sql语句都是转换为小写对表和DB进行查找。
设置2，创建的表和DB依据语句上格式存放，凡是查找都是转换为小
写进行。 
 
 
设置变量常采用 set lower_case_table_names = 1； 的方式，
但此变量是只读权限，所以需要在配置文件中改。
当想设置为大小写不敏感时，要在my.cnf这个配置文件 [mysqld] 中加入
lower_case_table_names = 1 ，然后重启服务器。
但是要在重启数据库实例之前就需要将原来的数据库和表转换为小写，
否则更改后将找不到数据库名。
在进行数据库参数设置之前，需要掌握这个参数带来的影响，
切不可盲目设置。

```
#### 3.2 sql_mode
```markdown
MySQL的sql_mode合理设置
sql_mode是个很容易被忽视的变量，默认值是空值，在这种
设置下是可以允许一些非法操作的，比如允许一些非法数据
的插入。在生产环境必须将这个值设置为严格模式，所以开发、
测试环境的数据库也必须要设置，这样在开发测试阶段就可以发现问题。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130145238544.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
使用 set sql_mode=ONLY_FULL_GROUP_BY; 的方式设置会将之前的设置覆盖掉
同时设置多个限制：set sql_mode='ONLY_FULL_GROUP_BY,NO_AUTO_VALUE_ON_ZERO';
 
sql_mode常用值如下： 
ONLY_FULL_GROUP_BY：
对于GROUP BY聚合操作，如果在SELECT中的列，没有在GROUP BY
中出现，那么这个SQL是不合法的，因为列不在GROUP BY从句中
 
NO_AUTO_VALUE_ON_ZERO：
该值影响自增长列的插入。默认设置下，插入0或NULL代表生成下
一个自增长值。如果用户 希望插入的值为0，而该列又是自增长的，
那么这个选项就有用了。
 
STRICT_TRANS_TABLES：
在该模式下，如果一个值不能插入到一个事务表中，则中断当前的
操作，对非事务表不做限制
NO_ZERO_IN_DATE：
在严格模式下，不允许日期和月份为零
 
NO_ZERO_DATE：
设置该值，mysql数据库不允许插入零日期，插入零日期会抛出错误
而不是警告。
 
ERROR_FOR_DIVISION_BY_ZERO：
在INSERT或UPDATE过程中，如果数据被零除，则产生错误而非警告。
如果未给出该模式，那么数据被零除时MySQL返回NULL
 
NO_AUTO_CREATE_USER：
禁止GRANT创建密码为空的用户
 
NO_ENGINE_SUBSTITUTION：
如果需要的存储引擎被禁用或未编译，那么抛出错误。不设置此值时，
用默认的存储引擎替代，并抛出一个异常
 
PIPES_AS_CONCAT：
将"||"视为字符串的连接操作符而非或运算符，这和Oracle数据库是一样的，
也和字符串的拼接函数Concat相类似
 
ANSI_QUOTES：
启用ANSI_QUOTES后，不能用双引号来引用字符串，因为它被解
释为识别符
 
ORACLE：
  设置等同：PIPES_AS_CONCAT, ANSI_QUOTES, IGNORE_SPACE, NO_KEY_OPTIONS, NO_TABLE_OPTIONS, NO_FIELD_OPTIONS, NO_AUTO_CREATE_USER.
```
### 4 Mysql逻辑架构
#### 4.1 总体概览
```markdown
和其它数据库相比，MySQL有点与众不同，它的架构可以
在多种不同场景中应用并发挥良好作用。主要体现在存储引擎的
架构上，
插件式的存储引擎架构将查询处理和其它的系统任务以及数据
的存储提取相分离。这种架构可以根据业务的需求和实际需要选
择合适的存储引擎。
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130145737547.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
1.连接层
最上层是一些客户端和连接服务，包含本地sock通信和大多数基于
客户端/服务端工具实现的类似于tcp/ip的通信。主要完成一些类似
于连接处理、授权认证、及相关的安全方案。在该层上引入了线程池的概念，
为通过认证安全接入的客户端提供线程。同样在该层上可以实现基于SSL
的安全链接。服务器也会为安全接入的每个客户端验证它所具有的操
作权限。
 
2.服务层
 
2.1  Management Serveices & Utilities： 系统管理和控制工具  
2.2  SQL Interface: SQL接口
      接受用户的SQL命令，并且返回用户需要查询的结果。比如select from就
是调用SQL Interface
2.3 Parser: 解析器
       SQL命令传递到解析器的时候会被解析器验证和解析。 
2.4 Optimizer: 查询优化器。
     SQL语句在查询之前会使用查询优化器对查询进行优化。 
     用一个例子就可以理解： select uid,name from user where  gender= 1;
     优化器来决定先投影还是先过滤。
  
2.5 Cache和Buffer： 查询缓存。
      如果查询缓存有命中的查询结果，查询语句就可以直接去查询缓存中取数据。
      这个缓存机制是由一系列小缓存组成的。比如表缓存，记录缓存，
 key缓存，权限缓存等
       缓存是负责读，缓冲负责写。
 
 
3.引擎层
存储引擎层，存储引擎真正的负责了MySQL中数据的存储和提取，服
务器通过API与存储引擎进行通信。不同的存储引擎具有的功能不同，
这样我们可以根据自己的实际需要进行选取。
后面介绍MyISAM和InnoDB
 
4.存储层
数据存储层，主要是将数据存储在运行于裸设备的文件系统之上，
并完成与存储引擎的交互。
```
### 5 Mysql存储引擎
#### 5.1 查看命令
```markdown
1 如何用命令查看
  #看你的mysql现在已提供什么存储引擎:
  mysql> show engines;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130150959338.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
#看你的mysql当前默认的存储引擎:
mysql> show variables like '%storage_engine%';
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130151102553.png)
#### 5.2 各个引擎简介
```markdown
1、InnoDB存储引擎
InnoDB是MySQL的默认事务型引擎，它被设计用来处理大量的
短期(short-lived)事务。除非有非常特别的原因需要使用其他的存储
引擎，否则应该优先考虑InnoDB引擎。行级锁，适合高并发情况
 
2、MyISAM存储引擎
MyISAM提供了大量的特性，包括全文索引、压缩、空间函数(GIS)等，
但MyISAM不支持事务和行级锁(myisam改表时会将整个表全锁住)，
有一个毫无疑问的缺陷就是崩溃后无法安全恢复。
 
3、Archive引擎
Archive存储引擎只支持INSERT和SELECT操作，在MySQL5.1之前不
支持索引。
Archive表适合日志和数据采集类应用。适合低访问量大数据等情况。
根据英文的测试结论来看，Archive表比MyISAM表要小大约75%，比支
持事务处理的InnoDB表小大约83%。
 
4、Blackhole引擎
Blackhole引擎没有实现任何存储机制，它会丢弃所有插入的数据，不
做任何保存。但服务器会记录Blackhole表的日志，所以可以用于复制
数据到备库，或者简单地记录到日志。但这种应用方式会碰到很多
问题，因此并不推荐。
 
5、CSV引擎
CSV引擎可以将普通的CSV文件作为MySQL的表来处理，但不支持索引。
CSV引擎可以作为一种数据交换的机制，非常有用。
CSV存储的数据直接可以在操作系统里，用文本编辑器，或者excel读取。
 
6、Memory引擎
如果需要快速地访问数据，并且这些数据不会被修改，重启
以后丢失也没有关系，那么使用Memory表是非常有用。Memory
表至少比MyISAM表要快一个数量级。(使用专业的内存数据库更
快，如redis)
 
7、Federated引擎
Federated引擎是访问其他MySQL服务器的一个代理，尽管该引
擎看起来提供了一种很好的跨服务器的灵活性，但也经常带来问
题，因此默认是禁用的。
```
##### 5.2.1 MyISAM和InnoDB
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202083627226.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202083540526.png)

```markdown
innodb 索引 使用 B+TREE myisam 索引使用 b-tree
innodb 主键为聚簇索引，基于聚簇索引的增删改查效率非常高。
```
### 6 索引优化分析
```markdown
性能下降SQL慢，执行时间长，等待时间长

查询数据过多
能不能拆，条件过滤尽量少

关联了太多的表，太多 Join
join 原理。用  A 表的每一条数据 扫描 B表的所有数据。所以尽量先过滤。

没有利用到索引
单值
复合
条件多时，可以建共同索引(混合索引)。混合索引一般会偶先使用。
有些情况下，就算有索引具体执行时也不会被使 用。

服务器调优及各个参数设置（缓冲、线程数等）
```
###  7 手写跟机读
#### 7.1 手写
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130104201110.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 7.2 机读
```markdown
随着Mysql版本的更新换代，其优化器也在不断的升级，优化
器会分析不同执行顺序产生的性能消耗不同而动态调整执行顺序。
下面是经常出现的查询顺序：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202084729474.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130104437465.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 8 七种join
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200730102829640.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200730103243988.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
1 A、B两表共有
 select * from t_emp a inner join t_dept b on a.deptId = b.id;
 
2 A、B两表共有+A的独有
 select * from t_emp a left join t_dept b on a.deptId = b.id;
 
3 A、B两表共有+B的独有
 select * from t_emp a right join t_dept b on a.deptId = b.id;
 
4 A的独有 
select * from t_emp a left join t_dept b on a.deptId = b.id where b.id is null; 
 
5 B的独有
 select * from t_emp a right join t_dept b on a.deptId = b.id where a.deptId is null;  
 
6 AB全有
#MySQL Full Join的实现 因为MySQL不支持FULL JOIN,下面是替代方法
 #left join + union(可去除重复数据)+ right join
SELECT * FROM t_emp A LEFT JOIN t_dept B ON A.deptId = B.id
UNION
SELECT * FROM t_emp A RIGHT JOIN t_dept B ON A.deptId = B.id
 这里因为要联合的缘故，不能考虑到小表驱动大表的情况。只能用right join。要保证查询出来的数字要一致。
7 A的独有+B的独有
        * FROM t_emp A LEFT JOIN t_dept B ON A.deptId = B.id WHERE B.`id` IS NULL
UNION
SELECT * FROM t_emp A RIGHT JOIN t_dept B ON A.deptId = B.id WHERE A.`deptId` IS NULL;

```
### 9 索引
#### 9.1 是什么
```markdown
MYSQL官方对索引的定义为：索引（ Index）是帮助mysql高效获取数
据的数据结构。
可以得到索引的本质：索引是数据结构。
```
```markdown
索引的目的在于提高查询效率，可以类比字典，
如果要查“mysql”这个单词，我们肯定需要定位到m字母，
然后从下往下找到y字母，再找到剩下的sql。
如果没有索引，那么你可能需要a----z
你可以简单理解为“排好序的快速查找数据结构
```
```markdown
在数据之外，数据库系统还维护着满足特定查找算法的数据结构，
这些数据结构以某种方式引用（指向）数据，
这样就可以在这些数据结构上实现高级查找算法。这种数据
结构，就是索引。下图就是一种可能的索引方式示例：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130153740652.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
左边是数据表，一共有两列七条记录，最左边的是数据记录的物理地址
为了加快Col2的查找，可以维护一个右边所示的二叉查找树，每个
节点分别包含索引键值和一个指向对应数据记录物理地址的指针，这
样就可以运用二叉查找在一定的复杂度内获取到相应数据，从而快速
的检索出符合条件的记录。
二叉树弊端之一：二叉树很可能会发生两边不平衡的情况。
B-TREE: (B:balance)  会自动根据两边的情况自动调节，使两端无
限趋近于平衡状态。可以使性能最稳定。(myisam使用的方式)
 B-TREE弊端：(插入/修改操作多时，B-TREE会不断调整平衡，
 消耗性能)从侧面说明了索引不是越多越好。
B+TREE:Innodb 所使用的索引

```
```markdown
一般来说索引本身也很大，不可能全部存储在内存中，因此索引往往
以索引文件的形式存储的磁盘上

我们平常所说的索引，如果没有特别指明，都是指B树（多路搜索树，
并不一定是二叉的）结构组织的索引。
其中聚集索引，次要索引，覆盖索引，复合索引，前缀索引，唯一
索引默认都是使用B+树索引，统称索引。
当然，除了B+树这种类型的索引之外，还有哈稀索引（ hash index）等。
```
#### 9.2 优势 && 劣势
```markdown
优势
类似大学图书馆建书目索引，提高数据检索的效率，降低数据库的IO成本；
通过索引列对数据进行排序，降低数据排序的成本，降低了CPU的消耗

劣势
实际上素引也是一张表，该表保存了主键与素引字段，并指向实
体表的记录，所以索引列也是要占用空间的

虽然索引大大提高了査询速度，同时却会降低更新表的速度，
如对表进行 INSERT、 UPDATE和 DELETE。
因为更新表时， MYSQL不仅要保存数据，还要保存一下索引文
件每次更新添加了索引列的字段，都会调整因为更新所带来的键值变
化后的素引信息

索引只是提高效率的一个因素，如果你的 MYSQL有大数据量的表，
就需要花时间研究建立最优秀的索引，或优化査询语句
```
### 10 mysq索引结构
#### 10.1  Myisam普通素引
##### 10.1.1  Btree索引
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202092642918.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
【初始化介绍】 
一颗b树，浅蓝色的块我们称之为一个磁盘块，可以看到每个磁盘块
包含几个数据项（深蓝色所示）和指针（黄色所示），
如磁盘块1包含数据项17和35，包含指针P1、P2、P3，
P1表示小于17的磁盘块，P2表示在17和35之间的磁盘块，P3表示
大于35的磁盘块。
真实的数据存在于叶子节点即3、5、9、10、13、15、28、29、36、60、75、79、90、99。
非叶子节点不存储真实的数据，只存储指引搜索方向的数据项，如17、35并
不真实存在于数据表中。
 
【查找过程】
如果要查找数据项29，那么首先会把磁盘块1由磁盘加载到内存，此
时发生一次IO，在内存中用二分查找确定29在17和35之间，锁定磁
盘块1的P2指针，内存时间因为非常短（相比磁盘的IO）可以忽略
不计，通过磁盘块1的P2指针的磁盘地址把磁盘块3由磁盘加载
到内存，发生第二次IO，29在26和30之间，锁定磁盘块3的P2指针，
通过指针加载磁盘块8到内存，发生第三次IO，同时内存中做二
分查找找到29，结束查询，总计三次IO。
 
真实的情况是，3层的b+树可以表示上百万的数据，如果上百万的
数据查找只需要三次IO，性能提高将是巨大的，如果没有索引，每
个数据项都要发生一次IO，那么总共需要百万次的IO，显然成
本非常非常高。
```
##### 10.1.2 关于时间复杂度  
```markdown
同一问题可用不同算法解决，而一个算法的质量优劣将影响到
算法乃至程序的效率。算法分析的目的在于选择合适算法和改进算法。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202092945674.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202092957555.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 10.2 innodb的普通素引
##### 	10.2.1  B+Tree索引
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202093428268.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
B+TREE 第二级的 数据并不能直接取出来，只作索引使用。在内
存有限的情况下，查询效率高于 B-TREE
B-TREE 第二级可以直接取出来，树形结构比较重，在内存无
限大的时候有优势。
```
##### 10.2.2  B树和B+树的区别
```markdown
B+Tree与B-Tree 的区别：结论在内存有限的情况下，B+TREE永远比
 B-TREE好。无限内存则后者方便
 
　1）B-树的关键字和记录是放在一起的，叶子节点可以看作外部节点，
不包含任何信息；B+树叶子节点中只有关键字和指向下一个节点的索引，
记录只放在叶子节点中。(一次查询可能进行两次i/o操作)
　 2）在B-树中，越靠近根节点的记录查找时间越快，只要找到关键字
即可确定记录的存在；而B+树中每个记录的查找时间基本是一样的，
都需要从根节点走到叶子节点，而且在叶子节点中还要再比较关键字。
从这个角度看B-树的性能好像要比B+树好，而在实际应用中却是B+
树的性能要好些。因为B+树的非叶子节点不存放实际的数据，这样每
个节点可容纳的元素个数比B-树多，树高比B-树小，这样带来的好处
是减少磁盘访问次数。尽管B+树找到一个记录所需的比较次数要比B-
树多，但是一次磁盘访问的时间相当于成百上千次内存比较的时间，
因此实际中B+树的性能可能还会好些，而且B+树的叶子节点使用指
针连接在一起，方便顺序遍历（例如查看一个目录下的所有文件，
一个表中的所有记录等），这也是很多数据库和文件系统使用B+
树的缘故。 
　
思考：为什么说B+树比B-树更适合实际应用中操作系统的文件索
引和数据库索引？ 
1) B+树的磁盘读写代价更低 
　　B+树的内部结点并没有指向关键字具体信息的指针。因此其
内部结点相对B 树更小。如果把所有同一内部结点的关键字存放在
同一盘块中，那么盘块所能容纳的关键字数量也越多。一次性读入
内存中的需要查找的关键字也就越多。相对来说IO读写次数也就降低了。 
2) B+树的查询效率更加稳定 
　　由于非终结点并不是最终指向文件内容的结点，而只是叶子结点
中关键字的索引。所以任何关键字的查找必须走一条从根结点到叶子
结点的路。所有关键字查询的路径长度相同，导致每一个数据的查询效率相当。
```
### 11 mysq素引分类
#### 11.1 主键索引
```markdown
设定为主键后数据库会自动建立索引， innodb为聚簇索引
```
```sql
随表一起建索引：
CREATE TABLE customer (id INT(10) UNSIGNED  AUTO_INCREMENT ,customer_no VARCHAR(200),customer_name VARCHAR(200),
  PRIMARY KEY(id) 
);
unsigned (无符号的)
使用  AUTO_INCREMENT 关键字的列必须有索引(只要有索引就行)。
 
CREATE TABLE customer2 (id INT(10) UNSIGNED   ,customer_no VARCHAR(200),customer_name VARCHAR(200),
  PRIMARY KEY(id) 
);
 
 单独建主键索引：
ALTER TABLE customer 
 add PRIMARY KEY customer(customer_no);  
 
删除建主键索引：
ALTER TABLE customer 
 drop PRIMARY KEY ;  
 
修改建主键索引：
必须先删除掉(drop)原索引，再新建(add)索引
```
#### 11.2 单值索引
```markdown
即一个素引只包含单个列，一个表可以有多个单列索引
```
```markdown
索引建立成哪种索引类型？
根据数据引擎类型自动选择的索引类型
除了innodb 引擎主键默认为聚簇索引 外。 innodb 的索引都采用的 B+TREE
myisam 则都采用的 B-TREE索引
```
```sql
随表一起建索引：
CREATE TABLE customer (id INT(10) UNSIGNED  AUTO_INCREMENT ,customer_no VARCHAR(200),customer_name VARCHAR(200),
  PRIMARY KEY(id),
  KEY (customer_name)  
);
 随表一起建立的索引 索引名同 列名(customer_name)
单独建单值索引：
CREATE  INDEX idx_customer_name ON customer(customer_name); 
 
删除索引：
DROP INDEX idx_customer_name ;
```
#### 11.3 唯一索引
```markdown
索引列的值必须唯一，但允许有空值
```
```sql
随表一起建索引：
CREATE TABLE customer (id INT(10) UNSIGNED  AUTO_INCREMENT ,customer_no VARCHAR(200),customer_name VARCHAR(200),
  PRIMARY KEY(id),
  KEY (customer_name),
  UNIQUE (customer_no)
);
建立 唯一索引时必须保证所有的值是唯一的（除了null），若有重复数据，会报错。  
 
单独建唯一索引：
CREATE UNIQUE INDEX idx_customer_no ON customer(customer_no); 
 
删除索引：
DROP INDEX idx_customer_no on customer ;
```
#### 11.4 复合索引
```markdown
即一个索引包含多个列
在数据库操作期间，复合索引比单值索引所需要的开销更小（对于相同的多个列建索引）
当表的行数远大于索引列的数目时可以使用复合索弓
```
```sql
 随表一起建索引：
CREATE TABLE customer (id INT(10) UNSIGNED  AUTO_INCREMENT ,customer_no VARCHAR(200),customer_name VARCHAR(200),
  PRIMARY KEY(id),
  KEY (customer_name),
  UNIQUE (customer_name),
  KEY (customer_no,customer_name)
);
 
单独建索引：
CREATE  INDEX idx_no_name ON customer(customer_no,customer_name); 
 
删除索引：
DROP INDEX idx_no_name  on customer ;
```
#### 11.5 基本语法
```sql
创建 ALTER mytable ADD  [UNIQUE] INDEX [indexname]  ON  (columnname(length))
删除 DROP INDEX [indexname] ON mytable;
查看 SHOW INDEX FROM table_name\G
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202095755996.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
non_unique: 是否是唯一索引  1：是   0：不是
seq_in_index:列 在索引中的 序列。针对符合索引(一个索引对应多个列)。
针对同一个复合索引 按照创建复合索引时的顺序进行排序
collation:
cardinality:
sub_part:
packed:
Null:是否允许 null 值

```
```sql
使用 ALTERI命令
有四种方式来添加数据表的索引：
ALTER TABLE tbl_name ADD PRIMARY KEY (column_list): 该语句添加一个主键，这意味着索引值必须是唯一的，且不能为NULL。
 
ALTER TABLE tbl_name ADD UNIQUE index_name (column_list): 这条语句创建索引的值必须是唯一的（除了NULL外，NULL可能会出现多次）。
 
ALTER TABLE tbl_name ADD INDEX index_name (column_list): 添加普通索引，索引值可出现多次。
 
ALTER TABLE tbl_name ADD FULLTEXT index_name (column_list):该语句指定了索引为 FULLTEXT ，用于全文索引。
 
```
#### 11.6 哪些情况需要创建索引
```markdown
1.主键自动建立唯一索引
2.频繁作为查询条件的字段应该创建索引（where后面的语句）
```
```markdown
3.查询中与其它表关联的字段，外键关系建立索引

A 表关联 B 表：A join B  。  on 后面的连接条件 既 A 表查询 B 表的条
件。所以 B 表被关联的字段建立索引能大大提高查询效率
因为在 join 中，join 左边的表会用每一个字段去遍历 B 表的所有
的关联数据，相当于一个查询操作

```
```markdown
4.单键/组合索引的选择问题，Who？(在高并发下倾向创建组合索引)
```
```markdown
5.查询中排序的字段，排序字段若通过索引去访问将大大提高排序速度
group by 和 order by 后面的字段有索引大大提高效率
```
```markdown
6.查询中统计或者分组字段
```
#### 11.7 哪些情况不要创建索引
```markdown
1.表记录太少
```
```markdown
2.经常増删改的表
Why：提高了查询速度，同时却会降低更新表的速度，
如对表进行INSERT、 UPDATE和 DELETE.
因为更新表时， MYSQL不仅要保存数据，还要保存一下素引文件
```
```markdown
3. Where条件里用不到的字段不创建索引
索引建多了影响 增删改 的效率
```
```markdown
4.数据重复且分布平均的表字段,因此应该只为最经常查询和最经
常排序的数据列建立索引。
注意，如果某个数据列包含许多重复的内容，为它建立索引就没
有太大的实际效果。
```
### 12 性能分析
#### 12.1 MYSQL常见瓶颈

```markdown
CPU 
SQL中对大量数据进行比较、关联、排序、分组
最大的压力在于 比较

IO
实际内存满足不了缓存数据或排序等需要，导致产生大量物理IO
查询执行效率低，扫描过多数据行。

锁
不适宜的锁的设置，导致线程阻塞，性能下降。
死锁，线程之间交叉调用资源，导致死锁，程序卡住。

服务器硬件的性能瓶颈：top,free, iostat和 vmstat来查看系统
的性能状态
```
#### 12.2 Explain
```markdown
使用 EXPLAIN关键字可以模拟优化器执行SQL查询语句，从
而知道MySQL是
如何处理你的SQL语句的。分析你的查询语句或是表结构的
性能瓶颈
```
```markdown
表的读取顺序
哪些索引可以使用
数据读取操作的操作类型
哪些索引被实际使用
表之间的引用
每张表有多少行被优化器查询
```
#### 12.3 执行计划包含的信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020120210534494.png)
##### 12.3.1 id
```markdown
select查询的序列号，包含一组数字表示查询中执行 select子句
或操作表的顺序
```
```markdown
三种情況
id相同，执行顺序由上至下  
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202142409303.png)

```markdown
此例中 先执行where 后的第一条语句 t1.id = t2.id 通过 t1.id 关联 t2.id 。 
而  t2.id 的结果建立在 t2.id=t3.id 的基础之上。
```
```markdown
id不同，如果是子查询，id的序号会增，id值越大优先级越高，
越先被执行
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202142536548.png)

```markdown
id相同不同，同时存在
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202140414660.png)

```markdown
id如果相同，可以认为是一组，从上往下顺序执行；
在所有组中，id值越大，优先级越高，越先执行
 
衍生表 = derived2 --> derived + 2 （2 表示由 id =2 的查询衍生出来的表。
type 肯定是 all ，因为衍生的表没有建立索引）
```
##### 12.3.2 select_type
```markdown
查询的类型，主要是用于区别普通查询、联合查询、子查询
等的复杂查询表示 SELECT 的类型，常见的取值，如下表所示：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020120214145716.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
SIMPLE
简单的 select查询，查询中不包含子查询或者 UNION
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202143803751.png)

```markdown
PRIMARY
查询中若包含任何复杂的子部分，最外层查询则被标记为 Primary
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202143926349.png)

```markdown
DERIVED
在FROM列表中包含的子查询被标记为 DERIVED(衍生)
MYSQL会递归执行这些子查询，把结果放在临时表里。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202144251856.png)

```markdown
SUBQUERY
在 SELECTI或 WHERE列表中包含了子查询
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202144441536.png)
```markdown
DEPENDENT SUBQUERY
在 SELECT或WHERE列表中包含了子査询，子査询基于外层
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202144652540.png)

```markdown
dependent subquery 与 subquery 的区别
依赖子查询 ： 子查询结果为 多值
子查询：查询结果为 单值 

```

```markdown
UNCACHEABLE SUBQUREY
无法被缓存的子查询
图中的 @@ 表示查的环境参数 。没办法缓存
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202144928700.png)
```markdown
UNION
若第二个 SELECT出现在 UNION之后，则被标记为 UNION
若 UNION包含在FROM子句的子查询中，外层 SELECT将被标记为： 
DERIVED
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202145225866.png)

```markdown
UNION RESULT 两个语句执行完后的结果
```

```markdown
UNION RESULT 
从 UNION表获取结果的 SELECT
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202150004902.png)
##### 12.3.3 table
```markdown
显示这一行的数据是关于哪张表的
```

##### 12.3.4 访问类型排列
```markdown 
type显示的是访问类型，是较为重要的一个指标，结果值从最好到最
坏依次是： 

system > const > eq_ref > ref > fulltext 
> ref_or_null > index_merge > unique_subquery
> index_subquery > range(尽量保证) > index > ALL 

system>const>eq_ref>ref>range>index>ALL
一般来说，得保证查询至少达到range级别，最好能达到ref。
```

```markdown
system
表只有一行记录（等于系统表），这是cont类型的特列，平时不会出现，
这个也可以忽略不计
```

```markdown
const
表示通过索引一次就找到了 const用于比较 primary key或者 
unique素引。因为只匹配一行数据，所以很快
如将主键置于 where列表中， MYSQL就能将该查询转换为一个常量
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202153814990.png)


```markdown
eq_ref
唯一性素引扫描，对于每个索引键，表中只有一条记录与之匹配。
常见于主键或唯一索引扫描
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202153731432.png)
```markdown
ref
非唯一性素引扫描，返回匹配某个单独值的所有行
本质上也是一种索引访问，它返回所有匹配某个单独值的行，然而
它可能会找到多个符合条件的行，所以他应该属于查找和扫描的混合体
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202154435169.png)

```markdown
range
只检索给定范围的行，使用一个索引来选择行。
key列显示使用了哪个素引
一般就是在你的 where语句中出现了 between、<、>、in等的查询
这种范围扫描索引扫描比全表扫描要好，因为它只需要开始于索
引的某一点.而结束语另一点，不用扫描全部素引。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202181003539.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
index
Full Index Scan, index与ALL区别为 index类型只遍历素引树.
这通常比ALL快，因为索引文件週常比数据文件小。
也就是说虽然al和 Index都是读全表，但 index是从素引中读取的，
而all是从硬盘中读的）
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202182023698.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
all
Full Table Scan，将遍历全表以找到匹配的行
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202182132346.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 12.3.5 possible_keys
```markdown
显示可能应用在这张表中的素引，一个或多个
查询涉及到的字段上若存在素引，则该索引将被列出，但不一
定被查询实际使用
```
##### 12.3.6 key
```markdown
实际使用的素引。如果为NULL，则没有使用索引
```
```markdown
查询中若使用了覆盖索引，则该索引和查询的 select字段重叠
```

```markdown
对比下图两个 sql 语句。和 key 的值：当查询具体某一字段时，
且那个字段有索引时，key 值会显示为索引。 
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202183656628.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 12.3.7 key_len
```markdown
表示索引中使用的字节数，可通过该列计算查询中使用的索引的长度。
key_len字段能够帮你裣查是否充分的利用上了索引
```
##### 12.3.8 ref

```markdown
显示索引的哪一列被使用了，如果可能的话，是一个常数。
哪些列或常量被用于查找素引列上的值
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202191619911.png)
##### 12.3.9 rows
```markdown
rows列显示MySQL认为它执行查询时必须检查的行数。
越少越好
```
##### 12.3.10  Extra
```markdown
包含不适合在其他列中显示但十分重要的额外信息
```
```markdown
Using filesort
说明mysql会对数据使用一个外部的索引排序,
而不是按照表内的索引顺序进行读取。
MYSQL中无法利用索引完成的排序操作称为"文件排序"
```

```markdown
Using temporary
使了用临时表保存中间结果 ,MYQL在对查询
结果排序时使用临时表。常见于排序 order
by和分组查询 group by。
```
```markdown
优化前存在 using  temporary 和 using  filesort 
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202194416997.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
create index idx_deptno_ename on emp(deptno,ename) 后解决
优化前存在的 using  temporary 和 using  filesort 不在，性能发生明显变化：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202194457546.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
USING index
表示相应的 select操作中使用了覆盖索引(CoveringIndex) ,避兔访问了
表的数据行，效率不错
如果同时出现 using where，表明索引被用来执行索引键值的查找；
如果没有同时出现 using where，表明索引只是用来读取数据而非利
用索引执行查找。
```

```markdown
Using where
表明使用了 where过滤
```
#### 12.4  查询优化
##### 12.4.1  案例（索引失效）
```markdown
1.全值匹配我最爱
```
```markdown
索引  idx_staffs_nameAgePos 建立索引时 以 name ， age ，pos 的顺序建立的。全值匹配表示 按顺序匹配的
EXPLAIN SELECT * FROM staffs WHERE NAME = 'July';
EXPLAIN SELECT * FROM staffs WHERE NAME = 'July' AND age = 25;
EXPLAIN SELECT * FROM staffs WHERE NAME = 'July' AND age = 25 AND pos = 'dev';
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202230540196.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
2.最佳左前缀法则
如果索引了多列，要遵守最左前缀法则。指的是查询从索引的最左
前列开始并且不跳过索引中的列
```
```markdown
EXPLAIN SELECT * FROM staffs WHERE age = 25 AND pos = 'dev'; 
EXPLAIN SELECT * FROM staffs WHERE pos = 'dev';
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202231814488.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202231827845.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202231902847.png)


```markdown
3.不在索引列上做任何操作
(计算、函数、(自动or手动）类型转换）,会导致索引失效而转向全
表扫描
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020120223210288.png)

```markdown
4.存储引擎不能使用索引中范围条件右边的列
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202233238740.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201202233346723.png)


```markdown
5.尽量使用覆盖索引,只访问索引的查询（索引列和查询列一致），
减少 select * 
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203074831799.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203074924515.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
6.mysql在使用不等于（!=或者<>)的时候无法使用索引会导致
全表扫描
索引  idx_nameAgeJob
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203080035748.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
7.is not nu也无法使用索引，但是 is null是可以使用索引的
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203080132985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
8.like以通配符开头（"%abc..)mysql索引失效会变成全表扫描的操作
like ‘%abc%’  type 类型会变成 all
like ‘abc%’ type 类型为 range ，算是范围，可以使用索引
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203080309326.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
9.字符串不加单引号索引失效
 底层进行转换使索引失效，使用了函数造成索引失效
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203080425311.png)

```markdown
10.少用or，用它来连接时会索引失效
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020120308051825.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
小总结
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203081016393.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 12.5  order by关键字优化
```markdown
ORDER BY子句，尽量使用 Index方式排序，避免使用 File Sort，方式排序
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203091940631.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203092000526.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
MYSQL支持二种方式的排序， File Sort和Index, Index效率高,
它指 MYSQL扫描索引本身完成排序。 File Sort，方式效率较低。
```
```markdown
ORDER BY满足两情況，会使用Index方式排序
1ORDER BY语句使用索引最左前列
2.使用 Where子句与 Order BY子句条件列组合满足索引最左前列
3.where子句中如果出现引的范围查询（即explain中出现 range）
会导致 order by索引失效
```
```markdown
尽可能在索引列上完成排序操作，遵照索引建的最佳左前缀
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203093527967.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
第二种中，where a = const and b > const order by b , c 
不会出现 using filesort  b , c 两个衔接上了
但是：where a = const and b > const order by  c 将会出
现 using filesort 。因为 b 用了范围索引，断了。
而上一个  order by 后的b 用到了索引，所以能衔接上 c 
```
#### 12.6  GROUP BY关键字优化
```markdown
1.group  by实质是先排序后进行分组，遵照索引建的最佳左前缀
2.当无法使用索引列，增大 max_length_for_sort_data参数的设置+增
大 sort_buffer_size参数的设置
3.where高于having,能写在 where限定的条件就不要去 having限定了。
4.去重优化
尽量不要使用 distinct
关键字去重：优化
t_mall_sku 表
  id  shp_id      kcdz                
------  ------ --------------------
     3       1    北京市昌平区  
     4       1    北京市昌平区  
     5       5    北京市昌平区  
     6       3       重庆              
     8       8     天津              
例子：select kcdz form t_mall_sku where id in( 3,4,5,6,8 )  
将产生重复数据，
select distinct kcdz form t_mall_sku where id in( 3,4,5,6,8 )   使用 distinct 关键字去重消耗性能
优化： select  kcdz form t_mall_sku where id in( 3,4,5,6,8 )  group by kcdz 能够利用到索引

```
```markdown
5.分页查询的优化-limit
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203095128383.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 12.7 查询截取分析
##### 12.7.1 慢查询日志
```markdown
 
MySQL的慢查询日志是MySQL提供的一种日志记录，它用来记
录在MySQL中响应时间超过阀值的语句，具体指运行时间超
过long_query_time值的SQL，则会被记录到慢查询日志中。

具体指运行时间超过long_query_time值的SQL，则会被记录到
慢查询日志中。long_query_time的默认值为10，意思是运行10秒
以上的语句。

由他来查看哪些SQL超出了我们的最大忍耐时间值，比如一条
sql执行超过5秒钟，我们就算慢SQL，希望能收集超过5秒的
sql，结合之前explain进行全面分析。


 
 
默认情况下，MySQL数据库没有开启慢查询日志，需要我们手
动来设置这个参数。
当然，如果不是调优需要的话，一般不建议启动该参数，因为开
启慢查询日志会或多或少带来一定的性能影响。慢查询日志支持
将日志记录写入文件
```
```markdown
默认情况下slow_query_log的值为OFF，表示慢查询日志是禁用的，
可以通过设置slow_query_log的值来开启
 
 SHOW VARIABLES LIKE '%slow_query_log%';
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203195527147.png)

```markdown
 
使用set global slow_query_log=1开启了慢查询日志只对当前数
据库生效，如果MySQL重启后则会失效。


 全局变量设置，对当前连接不影响
对当前连接立刻生效

 
如果要永久生效，就必须修改配置文件my.cnf（其它系统变量也是如此）
修改my.cnf文件，[mysqld]下增加或修改参数
slow_query_log 和slow_query_log_file后，然后重启MySQL服务器。
也即将如下两行配置进my.cnf文件
 
slow_query_log =1
slow_query_log_file=/var/lib/mysql/cqm-slow.log
 
关于慢查询的参数slow_query_log_file ，它指定慢查询日志文件
的存放路径，系统默认会给一个缺省的文件host_name-slow.log
（如果没有指定参数slow_query_log_file的话）

```
```markdown
那么开启了慢査询日志后，什么样的SQLオ会记录到慢查询日志里面呢
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203195822630.png)

```markdown
可以使用命令修改，也可以在my.cnf参数里面修改。
 
假如运行时间正好等于long_query_time的情况，并不会被记录下来。
也就是说，在mysql源码里是判断大于long_query_time，而非大于等于。
```
```markdown
査看当前多少秒算慢
SHOW VARIABLES LIKE 'long_query_time%';
```
```markdown
设置慢的阙值时间
set global long_query_time=1
修改为阙值到1秒钟的就是慢sql
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203200158628.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
修改后发现long_query_time并没有改变。
需要重新连接或新开一个会话才能看到修改值。 SHOW VARIABLES LIKE  'long_query_time%';

或者通过 set session
long_ query time=1来改变当前 session变量；
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203200347713.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
记录慢SQL并后续分析
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203200500786.png)

```markdown
跟踪日志信息
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203200535361.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
查询当前系统中有多少条慢查询记录
show global status like '%Slow_queries%';
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203201250864.png)

```markdown
日志分析工具 mysqldumpslow
在生产环境中，如果要手工分析日志，查找、分析SQL，显然是
个体力活，MySQL提供了日志分析工具mysqldumpslow。
```
```markdown
mysqldumpslow --help
```
```markdown
工作常用参考
得到返回记录集最多的10个SQL
mysqldumpslow -s r -t 10 /var/lib/mysql/cqm-slow.log
 
得到访问次数最多的10个SQL
mysqldumpslow -s c -t 10 /var/lib/mysql/cqm-slow.log
 
得到按照时间排序的前10条里面含有左连接的查询语句
mysqldumpslow -s t -t 10 -g "left join" /var/lib/mysql/cqm-slow.log
 
另外建议在使用这些命令时结合 | 和more 使用 ，否则有可能出现爆屏情况
mysqldumpslow -s r -t 10 /var/lib/mysql/cqm-slow.log | more

```
```markdown
Show Profile
是 mysql提供可以用来分析当前会话中语句执行的资源消耗情況。
可以用于SQL的调优的测量
默认情況下，参数处于关闭状态，并保存最近15次的运行结果
```
```markdown
Show  variables like 'profiling';
默认是关闭，使用前需要开启
开启功能，默认是关闭，使用前需要开启
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203202500602.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
查看结果， show profiles
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203202624620.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
诊新5QL, show profile cpu, block io for query n(n为上ー步前面的问题
5QL数字号码）
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203204708908.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 12.8 主从复制
```markdown
slave会从 master 读取 binlog来进行数据同步
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203205044615.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
MySQL复制过程分成三步：
1 master将改变记录到二进制日志（binary log）。这些记录过程叫做二进制日志事件，binary log events；
2 slave将master的binary log events拷贝到它的中继日志（relay log）；
3 slave重做中继日志中的事件，将改变应用到自己的数据库中。 MySQL复制是异步的且串行化的
```
```markdown
复制的基本原则
每个slave有一个 master
每个slave只能有一个唯一的服务器ID
每个master可以有多个 salve
```
```markdown
复制的最大问越
延时
```
#### 12.9 MySql锁机制
```markdown
锁是计算机协调多个进程或线程并发访问某一资源的机制。

在数据库中，除传统的计算资源（如CPU、RAM、I/O等）的争用以外，数据也是一种供许多用户共享的资源。如何保证数据并发访问的一致性、有效性是所有数据库必须解决的一个问题，锁冲突也是影响数据库并发访问性能的一个重要因素。从这个角度来说，锁对数据库而言显得尤其重要，也更加复杂。
```
#### 12.10 锁的分类
```markdown
从对数据操作的类型（读\写）分
读锁（共享锁）：针对同一份数据，多个读操作可以同时进行而不会互相影响。
写锁（排它锁）：当前写操作没有完成前，它会阻断其他写锁和读锁。
从对数据操作的粒度分
表锁
行锁
```
##### 12.10.1 表锁（偏读）
```markdown
偏向 MYISAM存储引擎，开销小，加锁快；无死锁；锁定粒度大，
发生锁冲突的概最高，并发度最低
```
```sql 
create table mylock(
 id int not null primary key auto_increment,
 name varchar(20)
)engine myisam;
 
insert into mylock(name) values('a');
insert into mylock(name) values('b');
insert into mylock(name) values('c');
insert into mylock(name) values('d');
insert into mylock(name) values('e');
 
select * from mylock;
 
【手动增加表锁】
 lock table 表名字1 read(write)，表名字2 read(write)，其它;
【查看表上加过的锁】
  show open tables;
  【释放表锁】
unlock tables;

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203210658919.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
我们为mylock表加read锁(读阻塞写例子)
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203211140320.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203211226876.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203211510677.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203211524262.png)

```markdown
加写锁
mylockwrite(MyISAM)
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203211656234.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203211912471.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203211929625.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
MyISAM在执行查询语句（SELECT）前，会自动给涉及的所有表加读锁，在执行增删改操作前，会自动给涉及的表加写锁。 
MySQL的表级锁有两种模式：
表共享读锁（Table Read Lock）
表独占写锁（Table Write Lock）
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203212133939.png)

```markdown
结论：
 结合上表，所以对MyISAM表进行操作，会有以下情况： 
  1、对MyISAM表的读操作（加读锁），不会阻塞其他进程对同一表
的读请求，但会阻塞对同一表的写请求。只有当读锁释放后，才会执行
其它进程的写操作。 
  2、对MyISAM表的写操作（加写锁），会阻塞其他进程对同一表的
读和写操作，只有当写锁释放后，才会执行其它进程的读写操作。
简而言之，就是读锁会阻塞写，但是不会堵塞读。而写锁则会把读和写都堵塞
```
##### 12.10.2 行锁(偏写)
```markdown
偏向 INNODB存储引擎，开销大，
加锁慢；会出现死锁；锁定粒度
最小，发生锁冲突的概率最低，并
发度也最高。

INNODB与 MYISAM的最大不同有两点：一是支持事务（TRANSACTION）;二是采用了行级锁
```
```markdown
由于行锁支持事务
```
```markdown
事务（Transaction）及其ACID属性
事务是由一组SQL语句组成的逻辑处理单元，事务具有以下4个属性，通常简称为事务的ACID属性。 
原子性（Atomicity）：事务是一个原子操作单元，其对数据的修改，要么全都执行，要么全都不执行。 
一致性（Consistent）：在事务开始和完成时，数据都必须保持一致状态。
这意味着所有相关的数据规则都必须应用于事务的修改，以保持数据的完
整性；事务结束时，所有的内部数据结构（如B树索引或双向链表）也都
必须是正确的。 
隔离性（Isolation）：数据库系统提供一定的隔离机制，保证事务在不受
外部并发操作影响的“独立”环境执行。这意味着事务处理过程中的中间状
态对外部是不可见的，反之亦然。 
持久性（Durable）：事务完成之后，它对于数据的修改是永久性的，即
使出现系统故障也能够保持。 

```
```sql
建表SQL
create table test_innodb_lock (a int(11),b varchar(16))engine=innodb;
 
insert into test_innodb_lock values(1,'b2');
insert into test_innodb_lock values(3,'3');
insert into test_innodb_lock values(4,'4000');
insert into test_innodb_lock values(5,'5000');
insert into test_innodb_lock values(6,'6000');
insert into test_innodb_lock values(7,'7000');
insert into test_innodb_lock values(8,'8000');
insert into test_innodb_lock values(9,'9000');
insert into test_innodb_lock values(1,'b1');
 
create index test_innodb_a_ind on test_innodb_lock(a);
 
create index test_innodb_lock_b_ind on test_innodb_lock(b);
 
select * from test_innodb_lock;
```
```markdown
行锁定基本演示
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203213428769.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203213451312.png)

```markdown
无索引行锁升级为表锁
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203213556373.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
###### 12.10.2.1  间隙锁
```markdown
间隙锁带来的插入问题
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203213935913.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
【什么是间隙锁】
当我们用范围条件而不是相等条件检索数据，并请求共享或排他锁时，
InnoDB会给符合条件的已有数据记录的索引项加锁；对于键值在条件
范围内但并不存在的记录，叫做“间隙（GAP)”，
InnoDB也会对这个“间隙”加锁，这种锁机制就是所谓的间隙锁（GAP Lock）。
 
【危害】
因为Query执行过程中通过过范围查找的话，他会锁定整个范围内所
有的索引键值，即使这个键值并不存在。
间隙锁有一个比较致命的弱点，就是当锁定一个范围键值之后，即使某
些不存在的键值也会被无辜的锁定，而造成在锁定的时候无法插入
锁定键值范围内的任何数据。在某些场景下这可能会对性能造成很
大的危害
```
```markdown
行锁分析
通过检查InnoDB_row_lock状态变量来分析系统上的行锁的争夺情况
mysql>show status like 'innodb_row_lock%';
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203214549124.png)

```markdown
对各个状态量的说明如下：

Innodb_row_lock_current_waits：当前正在等待锁定的数量；
Innodb_row_lock_time：从系统启动到现在锁定总时间长度；
Innodb_row_lock_time_avg：每次等待所花平均时间；
Innodb_row_lock_time_max：从系统启动到现在等待最常的一次所花的时间；
Innodb_row_lock_waits：系统启动后到现在总共等待的次数；
对于这5个状态变量，比较重要的主要是
Innodb_row_lock_time_avg（等待平均时长），
Innodb_row_lock_waits（等待总次数）
Innodb_row_lock_time（等待总时长）这三项。
尤其是当等待次数很高，而且每次等待时长也不小的时候，我们就需要
分析系统中为什么会有如此多的等待，然后根据分析结果着手指
定优化计划。

最后可以通过
SELECT * FROM information_schema.INNODB_TRX\G;
来查询正在被锁阻塞的sql语句。

```
```markdown
优化建议
尽可能让所有数据检索都通过索引来完成，避免无索引行锁升级为表锁。

尽可能较少检索条件，避免间隙锁

尽量控制事务大小，减少锁定资源量和时间长度

锁住某行后，尽量不要去调别的行或表，赶紧处理被锁住的行然后
释放掉锁

涉及相同表的事务，对于调用表的顺序尽量保持一致。

在业务环境允许的情况下，尽可能低级别事务隔离
```
##### 12.10.3 页锁
```markdown
开销和加锁时间界于表锁和行锁之间：会出现死锁；锁定粒度界于表锁
利行锁之间，并发度一般
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复
mysql高级即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
