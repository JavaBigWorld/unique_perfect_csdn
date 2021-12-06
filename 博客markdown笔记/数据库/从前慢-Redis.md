# Redis
![在这里插入图片描述](https://img-blog.csdnimg.cn/7d14fb29c8944b829eafb2d57eadd7e3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAdW5pcXVlX3BlcmZlY3Q=,size_16,color_FFFFFF,t_70,g_se,x_16#pic_center)

## 1 redis入门
### 1.1 NoSQL的引言
```markdown
NoSQL(Not Only SQL)，意即不仅仅是SQL, 泛指非关系型的数据
库。Nosql这个技术门类,早期就有人提出,发展至2009年趋势越
发高涨。
```
### 1.2 为什么是NoSQL
```markdown
随着互联网网站的兴起，传统的关系数据库在应付动态网站，
特别是超大规模和高并发的纯动态网站已经显得力不从心，暴
露了很多难以克服的问题。如商城网站中对商品数据频繁查
询、对热搜商品的排行统计、订单超时问题、以及微信朋
友圈（音频，视频）存储等相关使用传统的关系型数据库实
现就显得非常复杂，虽然能实现相应功能但是在性能上却不
是那么乐观。nosql这个技术门类的出现，更好的解决了这些
问题，它告诉了世界不仅仅是sql。
```

### 1.3 NoSQL的四大分类

#### 1.3.1 键值(Key-Value)存储数据库

```markdown
1.说明: 
这一类数据库主要会使用到一个哈希表，这个表中有一个特定的
键和一个指针指向特定的数据。

2.特点
Key/value模型对于IT系统来说的优势在于简单、易部署。  
但是如果DBA只对部分值进行查询或更新的时候，Key/value就
显得效率低下了。

3.相关产品
Tokyo Cabinet/Tyrant,
Redis  内存
SSDB   硬盘
Voldemort 
Oracle BDB
```

#### 1.3.2 列存储数据库

```markdown
1.说明
这部分数据库通常是用来应对分布式存储的海量数据。

2.特点
键仍然存在，但是它们的特点是指向了多个列。这些列是由列家族来安排的。

3.相关产品
Cassandra、HBase、Riak.
```

#### 1.3.3 文档型数据库
```markdown
# 1.说明
- 文档型数据库的灵感是来自于Lotus Notes办公软件的，而且
它同第一种键值存储相类似该类型的数据模型是版本化的文档，
半结构化的文档以特定的格式存储，比如JSON。文档型数据库
可以看作是键值数据库的升级版，允许之间嵌套键值。而且文
档型数据库比键值数据库的查询效率更高

# 2.特点
- 以文档形式存储

# 3.相关产品
- MongoDB、CouchDB、 MongoDb(4.x). 国内也有文档型数据
库SequoiaDB，已经开源。
```

#### 1.3.4 图形(Graph)数据库
```markdown
1.说明
图形结构的数据库同其他行列以及刚性结构的SQL数据库不同，它
是使用灵活的图形模型，并且能够扩展到多个服务器上。
NoSQL数据库没有标准的查询语言(SQL)，因此进行数据库查询
需要制定数据模型。许多NoSQL数据库都有REST式的数据接口
或者查询API。

2.相关产品
Neo4J、InfoGrid、 Infinite Graph、
```

#### 1.3.5 NoSQL应用场景
```markdown
数据模型比较简单

需要灵活性更强的IT系统

对数据库性能要求较高

不需要高度的数据一致性
```
### 1.4 Redis 简介
```markdown
问题现象
海量用户
高并发

罪魁祸首——关系型数据库
性能瓶颈：磁盘IO性能低下
扩展瓶颈：数据关系复杂，扩展性差，不便于大规模集群

解决思路
降低磁盘IO次数，越低越好 —— 内存存储
去除数据间关系，越简单越好 —— 不存储关系，仅存储数据
这里Nosql可以解决

NoSQL：即 Not-Only SQL（ 泛指非关系型的数据库），作为关系
型数据库的补充
作用：应对基于海量用户和海量数据前提下的数据处理问题。

特征：
可扩容，可伸缩
大数据量下高性能
灵活的数据模型
高可用

常见 Nosql 数据库：
Redis
memcache
HBase
MongoDB
```
### 1.5 电商场景
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200807021038179.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

### 1.6 认识Redis
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022134608181.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
概念：Redis (REmote DIctionary Server) 是用 C 语言开发
的一个开源的高性能键值对（key-value）数据库

Redis is an open source (BSD licensed),
in-memory data structure store, used as a database, 
cache and message broker.
Redis开源遵循BSD  基于内存数据存储 被用于作为数据库
缓存消息中间件
总结: redis是一个内存型的数据库

特征：
1. 数据间没有必然的关联关系
2. 内部采用单线程机制进行工作
3. 高性能。官方提供测试数据，50个并发执行100000 个请求,
4. 读的速度是110000 次/s,写的速度是81000次/s。
5. 多数据类型支持
字符串类型 string
列表类型 list
散列类型 hash
集合类型 set
有序集合类型 sorted_set
6. 持久化支持。可以进行数据灾难恢复
```
### 1.7 Redis 的应用
```markdown
为热点数据加速查询（主要场景），如热点商品、热点新闻、
热点资讯、推广类等高访问量信息等
任务队列，如秒杀、抢购、购票排队等
即时信息查询，如各位排行榜、各类网站访问统计、公交到站信息、
在线人数信息（聊天室、网站）、设
备信号等
时效性信息控制，如验证码控制、投票控制等
分布式数据共享，如分布式集群架构中的 session 分离
消息队列
分布式锁
```
### 1.8 Redis 的下载与安装
```markdown
0.准备环境
vmware15.x+
centos7.x+

1.下载redis源码包
https://redis.io/
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020122520243561.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)


```markdown
2.下载完整源码包
redis-6.0.9.tar.gz
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022134702164.png#pic_center)

```markdown
3.将下载redis资料包上传到Linux中

4.解压缩文件

[root@localhost opt]# tar -zxvf redis-6.0.9.tar.gz
[root@localhost opt]# ls
containerd  redis  redis-6.0.9.tar.gz  rh

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201225202919149.png)



```markdown
5.安装gcc  
yum install -y gcc

6.进入解压缩目录执行如下命令
make MALLOC=libc
# 这一步可能会报错，升级gcc版本即可
升级gcc版本
yum -y install centos-release-scl
yum -y install devtoolset-9-gcc devtoolset-9-gcc-c++ devtoolset-9-binutils 
scl enable devtoolset-9 bash  # scl命令启用只是临时的，
退出xshell或者重启就会恢复到原来的gcc版本。

# 如果要长期生效的话，执行如下：
echo "source /opt/rh/devtoolset-9/enable" >>/etc/profile

注意：如果用客户端工具连接服务器的话，有可能使用gcc -v还是
原来的版本，只需要断开重新连接即可 
7.编译完成后执行如下命令
make install PREFIX=/usr/redis  # 将编译好的文件放到指定目录下

8.进入/usr/local/redis/bin目录启动redis服务 
./redis-server
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201225204421809.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)


```markdown
9.Redis服务端口默认是 6379

10.进入bin目录执行客户端连接操作
./redis-cli –p 6379

补充：
默认配置启动
redis-server
redis-server -p 6379
redis-server  -p 6380
 
指定配置文件启动
redis-server redis.conf
redis-server redis-6379.conf
redis-server redis-6380.conf ……
redis-server conf/redis-6379.conf
redis-server config/redis-6380.conf ……

默认连接
redis-cli
连接指定服务器
redis-cli -h 127.0.0.1
redis-cli -p 6379
redis-cli -h 127.0.0.1 -p 6379

基本配置
daemonize yes
以守护进程方式启动，使用本启动方式，redis将以服务的形式存在，
日志将不再打印到命令窗口中
port 6***
设定当前服务启动端口号
dir “/自定义目录/redis/data“
设定当前服务文件保存位置，包含日志文件、持久化文件
（后面详细讲解）等
logfile "6***.log“
设定日志文件名，便于查阅
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201225204621661.png)
### 1.9 核心文件
```markdown
redis-serve 服务器启动命令
redis-cli 命令行客户端
redis.conf    redis核心配置文件
redis-benchmark 性能测试工具
redis-check-aof AOF文件修复工具
redis-check-dump  RDB文件检查工具（快照持久化文件）
```

### 1.10 Redis 的基本操作
```bash
信息添加
set key value
set name itheima

信息查询
功能：根据 key 查询对应的 value，如果不存在，返回空（nil）
get key
get name

清除屏幕信息
功能：清除屏幕中的信息
clear

退出客户端命令行模式
功能：退出客户端
quit
exit
<ESC>

帮助
功能：获取命令帮助文档，获取组中所有命令信息名称
命令
help 命令名称
help set

127.0.0.1:6379> help get

GET key
summary: Get the value of a key
since: 1.0.0
group: string

help @组名
127.0.0.1:6379> help @string

APPEND key value
summary: Append a value to a key
since: 2.0.0

BITCOUNT key [start end]
summary: Count set bits in a string
since: 2.6.0

BITFIELD key [GET type offset] [SET type offset value] [INCRBY type offset increment] [OVERFLOW WRAP|SAT|FAIL]
summary: Perform arbitrary bitfield integer operations on strings
since: 3.2.0
....
```
## 2 Redis 数据类型
```markdown
string String
hash HashMap
list LinkedList
set  HashSet
sorted_set TreeSet
```
### 2.1 redis 数据存储格式
```markdown
redis 自身是一个 Map，其中所有的数据都是采用 key : value的
形式存储
数据类型指的是存储的数据的类型，也就是 value 部分的类型，
key 部分永远都是字符串
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200807090128252.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 2.2 string 类型
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200807090308112.png)
#### 2.2.1 string操作
```bash
存储的数据：单个数据，最简单的数据存储类型，也是最常用的
数据存储类型
存储数据的格式：一个存储空间保存一个数据
存储内容：通常使用字符串，如果字符串以整数的形式展示，
可以作为数字操作使用

添加/修改数据
set key value
获取数据
get key
删除数据
del key

127.0.0.1:6379> set age 100
OK
127.0.0.1:6379> get age
"100"
127.0.0.1:6379> del age
(integer) 1
127.0.0.1:6379> del age
(integer) 0


添加/修改多个数据
mset key1 value1 key2 value2 …
获取多个数据
mget key1 key2 …
获取数据字符个数（字符串长度）
strlen key
追加信息到原始信息后部（如果原始信息存在就追加，否则新建）
append key value
127.0.0.1:6379> mset a 1 b 2 c 3
OK
127.0.0.1:6379> mget a b c
1) "1"
2) "2"
3) "3"
127.0.0.1:6379> strlen a
(integer) 1
127.0.0.1:6379> append a 23
(integer) 3
127.0.0.1:6379> get a
"123"


设置数值数据增加指定范围的值
incr key  # 增加1
incrby key increment
incrbyfloat key increment

127.0.0.1:6379> get a
"123"
127.0.0.1:6379> incr a
(integer) 124
127.0.0.1:6379> incrby a 26
(integer) 150
127.0.0.1:6379> incrbyfloat a 50
"200"


设置数值数据减少指定范围的值
decr key # 减少1
decrby key increment

127.0.0.1:6379> get a
"174"
127.0.0.1:6379> decr a
(integer) 173
127.0.0.1:6379> decrby a 25
(integer) 148



string 作为数值操作
string在redis内部存储默认就是一个字符串，当遇到增减
类操作incr，decr时会转成数值型进行计算。
redis所有的操作都是原子性的，采用单线程处理所有业务，
命令是一个一个执行的，因此无需考虑并发
带来的数据影响。
注意：按数值进行操作的数据，如果原始数据不能转成数值，
或超越了redis 数值上限范围，将报错。

9223372036854775807（java中long型数据最大值，
Long.MAX_VALUE）

Tips 1：
redis用于控制数据库表主键id，为数据库表主键提供生成策略，
保障数据库表的主键唯一性
此方案适用于所有数据库，且支持数据库集群

设置数据具有指定的生命周期
setex key seconds value  # 秒为单位
psetex key milliseconds value   # 毫秒为单位

Tips 2：
redis 控制数据的生命周期，通过数据是否失效控制业务行为，
适用于所有具有时效性限定控制的操作


127.0.0.1:6379> setex a 5 1
OK
127.0.0.1:6379> get a
"1"
127.0.0.1:6379> get a
(nil)

127.0.0.1:6379> psetex a 5000 2
OK
127.0.0.1:6379> get a
"2"
127.0.0.1:6379> get a
"2"
127.0.0.1:6379> get a
(nil)



string 类型数据操作的注意事项
数据操作不成功的反馈与数据正常操作之间的差异
① 表示运行结果是否成功
(integer) 0 → false 失败
(integer) 1 → true 成功
② 表示运行结果值
(integer) 3 → 3 3个
(integer) 1 → 1 1个
数据未获取到
（nil）等同于null
数据最大存储量
512MB
数值计算最大范围（java中的long的最大值）
9223372036854775807
```

#### 2.2.2 单数据操作与多数据操作的选择之惑
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020080709110093.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
set小闹钟表示发送时间，result小闹钟表示返回时间。大闹钟
表示执行命令的时间
```
#### 2.2.3 string 类型应用场景
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200809085249571.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
在redis中为大V用户设定用户信息，以用户主键和属性值作为key，
后台设定定时刷新策略即可
eg: user:id:3506728370:fans → 12210947
eg: user:id:3506728370:blogs → 6164
eg: user:id:3506728370:focuss → 83

127.0.0.1:6379> set user:id:3506728370:fans 12210947
OK
127.0.0.1:6379> get user:id:3506728370:fans 
"12210947"
127.0.0.1:6379> set user:id:3506728370:blogs 6164
OK
get user:id:3506728370:blogs 
"6164"
127.0.0.1:6379> set user:id:3506728370:focuss 83
OK
127.0.0.1:6379> get user:id:3506728370:focuss 
"83"


在redis中以json格式存储大V用户信息，定时刷新
（也可以使用hash类型）
eg: user:id:3506728370 →
{"id":3506728370,"name":"春晚","fans":12210862,"blogs":6164, "focus":83}

Tips 3：
redis应用于各种结构型和非结构型高热度数据访问加速
```
#### 2.2.4 key 的设置约定
```bash
set user:id:00789:fans 123
set user:id:00789:blogs 789
set userid:00789 {id:00789,blogs:789,fans:123}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200809085605386.png)
### 2.3  hash
#### 2.3.1 hash操作
```bash
添加/修改数据
hset key field value
获取数据
hget key field
hgetall key
删除数据
hdel key field1 [field2]

127.0.0.1:6379> hset a name xiaoming
(integer) 1
127.0.0.1:6379> hset a age 18
(integer) 1
127.0.0.1:6379> hget a name
"xiaoming"
127.0.0.1:6379> hgetall a
1) "name"
2) "xiaoming"
3) "age"
4) "18"
127.0.0.1:6379> hdel a name
(integer) 1
127.0.0.1:6379> hgetall a
1) "age"
2) "18"


添加/修改多个数据
hmset key field1 valuel field2 value2
获取多个数据
hmget key fieldl field2
获取哈希表中字段的数量
hlen key
获取哈希表中是否存在指定的字段
exists key field

127.0.0.1:6379> hmset a name xiaohong age 20
OK
127.0.0.1:6379> hmget a name age
1) "xiaohong"
2) "20"
127.0.0.1:6379> hlen a
(integer) 2
127.0.0.1:6379> exists a name
(integer) 1


获取哈希表中所有的字段名或字段值
keys key
hals key

127.0.0.1:6379> hkeys a
1) "age"
2) "name"
127.0.0.1:6379> hvals a
1) "20"
2) "xiaohong"


设置指定字段的数值数据増加指定范围的值
hincrby key field increment
hincrbyfloat key field increment


127.0.0.1:6379> hincrby a age 10
(integer) 30
127.0.0.1:6379> hgetall a
1) "age"
2) "30"
3) "name"
4) "xiaohong"
127.0.0.1:6379> hincrbyfloat a age 30.1
"60.1"
127.0.0.1:6379> hgetall a
1) "age"
2) "60.1"
3) "name"
4) "xiaohong"


hash类型数据操作的注意事项
hash类型下的 value只能存储字符串，不允许存储其
他数据类型，不存在嵌套现象。如果数据未获取到,对
应的值为（nil）
每个hash可以存储2^32-1个键值对
hash类型十分贴近对象的数据存储形式，并且可以灵活添加
删除对象属性。但hash设计初衷不是为了存储大量对象而
设计的，切记不可滥用，更不可以将hash作为对象列表使用
hgetal操作可以获取全部属性，如果内部 field过多，遍
历整体数据效率就很会低，有可能成为数据访问瓶颈
```
#### 2.3.2 电商网站购物车设计与实现
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200810093624312.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```bash
解决方案
以客户id作为key，每位客户创建一个hash存储结构存
储对应的购物车信息
将商品编号作为field，购买数量作为value进行存储
添加商品：追加全新的field与value
浏览：遍历hash
更改数量：自增/自减，设置value值
删除商品：删除field
清空：删除key
此处仅讨论购物车中的模型设计
购物车与数据库间持久化同步、购物车与订单间关系、未登
录用户购物车信息存储不进行讨论


127.0.0.1:6379> hmset 001 g01 100 g02 200
OK
127.0.0.1:6379> hmset 002 g02 1 g04 7 g05 100
OK
127.0.0.1:6379> hset 001 g03 5
(integer) 1
127.0.0.1:6379> hgetall 001
1) "g01"
2) "100"
3) "g02"
4) "200"
5) "g03"
6) "5"
127.0.0.1:6379> hdel 001 g01
(integer) 1
127.0.0.1:6379> hgetall 001
1) "g02"
2) "200"
3) "g03"
4) "5"
127.0.0.1:6379> hincrby 001 g03 1
(integer) 6
127.0.0.1:6379> hgetall 001
1) "g02"
2) "200"
3) "g03"
4) "6"

```
```bash
当前仅仅是将数据存储到了redis中，并没有起到加速的作用，
商品信息还需要二次查询数据库
每条购物车中的商品记录保存成两条field
field1专用于保存购买数量
命名格式：商品id:nums
保存数据：数值
field2专用于保存购物车中显示的信息，包含文字描述，
图片地址，所属商家信息等
命名格式：商品id:info
保存数据：json 
hsetnx key field value # 如果field存在不覆盖，如果不存在则添加

Tips 4：
redis 应用于购物车数据存储设计

127.0.0.1:6379> hmset 003 g01:nums 100 g01:info {.....}
OK
127.0.0.1:6379> hgetall 003
1) "g01:nums"
2) "100"
3) "g01:info"
4) "{.....}"
127.0.0.1:6379> hmset 004 g01:num 5 g01:info {.....}
OK
127.0.0.1:6379> hgetall 004
1) "g01:num"
2) "5"
3) "g01:info"
4) "{.....}"

127.0.0.1:6379> hgetall 003
1) "g01:nums"
2) "200"
3) "g01:info"
4) "{.....}"
127.0.0.1:6379> hsetnx 003 g01:nums 300
(integer) 0
127.0.0.1:6379> hgetall 003
1) "g01:nums"
2) "200"
3) "g01:info"
4) "{.....}"
127.0.0.1:6379> hsetnx 003 g01:age 18
(integer) 1
127.0.0.1:6379> hgetall 003
1) "g01:nums"
2) "200"
3) "g01:info"
4) "{.....}"
5) "g01:age"
6) "18"


```

#### 2.3.3 双11活动日
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200810103539781.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```bash
解决方案
以商家id作为key
将参与抢购的商品id作为field
将参与抢购的商品数量作为对应的value
抢购时使用降值的方式控制产品数量
实际业务中还有超卖等实际问题，这里不做讨论

Tips 5：
redis 应用于抢购，限购类、限量发放优惠卷、激活
码等业务的数据存储设计

127.0.0.1:6379> hmset p01 c30 1000 c50 1000 c100 1000
OK
127.0.0.1:6379> hincrby p01 c50 -1
(integer) 999
127.0.0.1:6379> hincrby p01 c100 -20
(integer) 980
127.0.0.1:6379> hgetall p01
1) "c30"
2) "1000"
3) "c50"
4) "999"
5) "c100"
6) "980"

```
#### 2.3.4 string跟hash的对比
```markdn
string存储对象（json）与hash存储对象,string讲究整体性，主
要体现读操作，而hash整体性不是那么强，主要体现更新操作
```
### 2.4 list
#### 2.4.1 list操作
```bash
数据存储需求：存储多个数据，并对数据进入存储空间
的顺序进行区分
需要的存储结构：一个存储空间保存多个数据，且通过
数据可以体现进入顺序
list类型：保存多个数据，底层使用双向链表存储结构实现


添加/修改数据
lpush key value1 [value2] ……
rpush key value1 [value2] ……

获取数据
lrange key start stop
lindex key index
llen key 

获取并移除数据
lpop key
rpop key

127.0.0.1:6379> lpush list1 a b c
(integer) 3
127.0.0.1:6379> rpush list2 d e f
(integer) 3
127.0.0.1:6379> lrange list1 0 -1
1) "c"
2) "b"
3) "a"
127.0.0.1:6379> lrange list2 0 -1
1) "d"
2) "e"
3) "f"
127.0.0.1:6379> lindex list1 0
"c"
127.0.0.1:6379> lindex list1 -1
"a"
127.0.0.1:6379> lindex list1 -2
"b"
127.0.0.1:6379> llen list1
(integer) 3
127.0.0.1:6379> llen list2
(integer) 3
127.0.0.1:6379> lpop list1
"c"
127.0.0.1:6379> rpop list1
"a"
127.0.0.1:6379> llen list1
(integer) 1
127.0.0.1:6379> lpop list2 
"d"
127.0.0.1:6379> rpop list2
"f"
127.0.0.1:6379> llen list2
(integer) 1

规定时间内获取并移除数据
blpop key1 [key2] timeout
brpop key1 [key2] timeout

127.0.0.1:6379> keys *
1) "list2"
127.0.0.1:6379> lpush list1 aa
(integer) 1
127.0.0.1:6379> keys *
1) "list2"
127.0.0.1:6379> lpush list3 bb
(integer) 1
127.0.0.1:6379> 

127.0.0.1:6379> blpop list1 10
1) "list1"
2) "aa"
127.0.0.1:6379> blpop list3 10
1) "list3"
2) "bb"
(3.78s)
```
#### 2.4.2 微信朋友圈点赞
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200811085926719.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```bash
list 类型数据扩展操作
移除指定数据
lrem key count value  # count表示移除的个数


Tips 6：
redis 应用于具有操作先后顺序的数据控制


127.0.0.1:6379> lpush list1 a b c d a
(integer) 5
127.0.0.1:6379> lrange list1 0 -1
1) "a"
2) "d"
3) "c"
4) "b"
5) "a"
127.0.0.1:6379> lrem list1 2 a
(integer) 2
127.0.0.1:6379> llen list1
(integer) 3
127.0.0.1:6379> lrange list1 0 -1
1) "d"
2) "c"
3) "b"
```
#### 2.4.3 list 类型数据操作注意事项
```markdown
list中保存的数据都是string类型的，数据总容量是有限的，
最多2^32-1 个元素 (4294967295)。

list具有索引的概念，但是操作数据时通常以队列的形式进行入队
出队操作，或以栈的形式进行入栈出栈操作

获取全部数据操作结束索引设置为-1

list可以对数据进行分页操作，通常第一页的信息来自于list，
第2页及更多的信息通过数据库的形式加载
```


#### 2.4.4 保障多台服务器操作日志的统一顺序输出
```bash
依赖list的数据具有顺序的特征对信息进行管理
使用队列模型解决多路信息汇总合并的问题
使用栈模型解决最新消息的问题


Tips 7：
redis 应用于最新消息展示

127.0.0.1:6379> rpush logs a1..
(integer) 1
127.0.0.1:6379> rpush logs a1...
(integer) 2
127.0.0.1:6379> lrange logs 0 -1
1) "a1.."
2) "a1..."
3) "b1.."
4) "c1.."
5) "b1..."
6) "c1..."

127.0.0.1:6379> rpush logs b1..
(integer) 3
127.0.0.1:6379> rpush logs b1...
(integer) 5

127.0.0.1:6379> rpush logs c1..
(integer) 4
127.0.0.1:6379> rpush logs c1...
(integer) 6
```
### 2.5 set类型
#### 2.5.1 set操作
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200811100300677.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```bash
新的存储需求：存储大量的数据，在查询方面提供更高的效率
需要的存储结构：能够保存大量的数据，高效的内部存储机制，
便于查询
set类型：与hash存储结构完全相同，仅存储键，不存储
值（nil），并且值是不允许重复的

set 类型数据的基本操作
添加数据
sadd key member1 [member2]
获取全部数据
smembers key 
删除数据
srem key member1 [member2]
获取集合数据总量
scard key
判断集合中是否包含指定数据
sismember key member

127.0.0.1:6379> sadd users zs
(integer) 1
127.0.0.1:6379> sadd users ls
(integer) 1
127.0.0.1:6379> sadd users ww
(integer) 1
127.0.0.1:6379> smembers users
1) "zs"
2) "ww"
3) "ls"
127.0.0.1:6379> srem users ww
(integer) 1
127.0.0.1:6379> smembers users
1) "zs"
2) "ls"
127.0.0.1:6379> scard users
(integer) 2
127.0.0.1:6379> sismember users zs
(integer) 1
127.0.0.1:6379> sismember users ww
(integer) 0

set 类型数据操作的注意事项
set 类型不允许数据重复，如果添加的数据在 set 中已经存在，
将只保留一份
set 虽然与hash的存储结构相同，但是无法启用hash中存
储值的空间

127.0.0.1:6379> sadd a a
(integer) 1
127.0.0.1:6379> sadd a a
(integer) 0
127.0.0.1:6379> hset a a 1
(error) WRONGTYPE Operation against a key holding the wrong kind of value

set 类型数据的扩展操作
随机获取集合中指定数量的数据
srandmember key [count]

随机获取集合中的某个数据并将该数据移出集合
spop key [count]

Tips 8：
redis 应用于随机推荐类信息检索，例如热点歌单推荐，
热点新闻推荐，热卖旅游线路，应用APP推荐，
大V推荐等

Tips 9：
redis 应用于同类信息的关联搜索，二度关联搜索，深度关联搜索
显示共同关注（一度）
显示共同好友（一度）
由用户A出发，获取到好友用户B的好友信息列表（一度）
由用户A出发，获取到好友用户B的购物清单列表（二度）
由用户A出发，获取到好友用户B的游戏充值列表（二度）

127.0.0.1:6379> sadd news n1
(integer) 1
127.0.0.1:6379> sadd news n2
(integer) 1
127.0.0.1:6379> sadd news n3
(integer) 1
127.0.0.1:6379> sadd news n4
(integer) 1
127.0.0.1:6379> srandmember news 1
1) "n1"
127.0.0.1:6379> srandmember news 1
1) "n1"
127.0.0.1:6379> srandmember news 1
1) "n1"
127.0.0.1:6379> srandmember news 1
1) "n3"
127.0.0.1:6379> scard news
(integer) 4
127.0.0.1:6379> spop news 1
1) "n1"
127.0.0.1:6379> smembers news
1) "n4"
2) "n2"
3) "n3"
127.0.0.1:6379> spop news 2
1) "n4"
2) "n3"
127.0.0.1:6379> smembers news
1) "n2"

求两个集合的交、并、差集
sinter key1 [key2]
sunion key1 [key2]
sdiff key1 [key2]


求两个集合的交、并、差集并存储到指定集合中
sinterstore destination key1 [key2]
sunionstore destination key1 [key2]
sdiffstore destination key1 [key2] 

将指定数据从原始集合中移动到目标集合中
smove source destination member 

127.0.0.1:6379> sadd u1 a1
(integer) 1
127.0.0.1:6379> sadd u1 s1
(integer) 1
127.0.0.1:6379> sadd u1 b1
(integer) 1
127.0.0.1:6379> sadd u2 s1
(integer) 1
127.0.0.1:6379> sadd u2 w1
(integer) 1
127.0.0.1:6379> sinter u1 u2
1) "s1"
127.0.0.1:6379> sunion u1 u2
1) "a1"
2) "s1"
3) "w1"
4) "b1"
127.0.0.1:6379> sdiff u1 u2
1) "a1"
2) "b1"
127.0.0.1:6379> sdiff u2 u1
1) "w1"
127.0.0.1:6379> sdiffstore u3 u1 u2
(integer) 2
127.0.0.1:6379> smembers u3
1) "a1"
2) "b1"
127.0.0.1:6379> smove u2 u1 w1
(integer) 1
127.0.0.1:6379> smembers u1
1) "a1"
2) "s1"
3) "w1"
4) "b1"
127.0.0.1:6379> smembers u2
1) "s1"
```

#### 2.5.2  权限校验
```markdown
集团公司共具有12000名员工，内部OA系统中具有700多个角色，
3000多个业务操作，23000多种数据，每位员工具有一个或多
个角色，可以快速进行业务操作的权限校验
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200821081742963.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```bash
127.0.0.1:6379> sadd rid:001 getall
(integer) 1
127.0.0.1:6379> sadd rid:001 getById
(integer) 1
127.0.0.1:6379> sadd rid:002 getall
(integer) 1
127.0.0.1:6379> sadd rid:002 getCount 
(integer) 1
127.0.0.1:6379> sadd rid:002 insert
(integer) 1
127.0.0.1:6379> sunionstore uid:007  rid:001 rid:002
(integer) 4
127.0.0.1:6379> smembers uid:007
1) "insert"
2) "getById"
3) "getCount"
4) "getall"
127.0.0.1:6379> sismember uid:007 insert
(integer) 1
```
#### 2.5.3  快速去重
```markdown
Tips 10：
redis 应用于同类型数据的快速去重
```
```bash
127.0.0.1:6379> sadd ips 1.2.3.4
(integer) 1
127.0.0.1:6379> sadd ips 2.3.4.5
(integer) 1
127.0.0.1:6379> sadd ips 2.3.4.5
(integer) 0
127.0.0.1:6379> scard ips
(integer) 2
```
### 2.6 sorted_set
#### 2.6.1 sorted_set操作
```markdown
新的存储需求：数据排序有利于数据的有效展示，需要提
供一种可以根据自身特征进行排序的方式
需要的存储结构：新的存储模型，可以保存可排序的数据
sorted_set类型：在set的存储结构基础上添加可排序字段
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200821090414885.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


```bash
sorted_set 类型数据的基本操作
添加数据
zadd key score1 member1 [score2 member2]
获取全部数据
zrange key start stop [WITHSCORES]
zrevrange key start stop [WITHSCORES]
删除数据
zrem key member [member ...]
按条件获取数据
zrangebyscore key min max [WITHSCORES] [LIMIT]
zrevrangebyscore key max min [WITHSCORES]
条件删除数据
zremrangebyrank key start stop
zremrangebyscore key min max
注意：
min与max用于限定搜索查询的条件
start与stop用于限定查询范围，作用于索引，表示开始和结束索引
offset与count用于限定查询范围，作用于查询结果，
表示开始位置和数据总量
获取集合数据总量
 zcard key
zcount key min max
集合交、并操作
zinterstore destination numkeys key [key ...]
zunionstore destination numkeys key [key ...]

127.0.0.1:6379> zadd scores 94 zs
(integer) 1
127.0.0.1:6379> zadd scores 100 ls
(integer) 1
127.0.0.1:6379> zadd scores 60 ww
(integer) 1
127.0.0.1:6379> zadd scores 47 zl
(integer) 1
127.0.0.1:6379> zrange scores 0 -1
1) "zl"
2) "ww"
3) "zs"
4) "ls"
127.0.0.1:6379> zrange scores 0 -1 withscores
1) "zl"
2) "47"
3) "ww"
4) "60"
5) "zs"
6) "94"
7) "ls"
8) "100"
127.0.0.1:6379> zrevrange scores 0 -1 withscores
1) "ls"
2) "100"
3) "zs"
4) "94"
5) "ww"
6) "60"
7) "zl"
8) "47"
127.0.0.1:6379> zrem scores ls
(integer) 1
127.0.0.1:6379> zrevrange scores 0 -1 withscores
1) "zs"
2) "94"
3) "ww"
4) "60"
5) "zl"
6) "47"

127.0.0.1:6379> zrange scores 0 -1 withscores
 1) "wangwu"
 2) "45"
 3) "zhangsan"
 4) "67"
 5) "zhouqi"
 6) "71"
 7) "qianba"
 8) "92"
 9) "lisi"
10) "99"
11) "zhangliu"
12) "100"
127.0.0.1:6379> zrangebyscore scores 50 80 withscores
1) "zhangsan"
2) "67"
3) "zhouqi"
4) "71"
127.0.0.1:6379> zrevrangebyscore scores 80 50 withscores
1) "zhouqi"
2) "71"
3) "zhangsan"
4) "67"
127.0.0.1:6379> zrangebyscore scores 50 99 withscores
1) "zhangsan"
2) "67"
3) "zhouqi"
4) "71"
5) "qianba"
6) "92"
7) "lisi"
8) "99"
127.0.0.1:6379> zrangebyscore scores 50 99 withscores limit 0 3
1) "zhangsan"
2) "67"
3) "zhouqi"
4) "71"
5) "qianba"
6) "92"
127.0.0.1:6379> zrangebyscore scores 50 99 withscores limit 0 1
1) "zhangsan"
2) "67"
127.0.0.1:6379> zremrangebyscore scores 50 70
(integer) 1
127.0.0.1:6379> zrange scores 0 -1 withscores
 1) "wangwu"
 2) "45"
 3) "zhouqi"
 4) "71"
 5) "qianba"
 6) "92"
 7) "lisi"
 8) "99"
 9) "zhangliu"
10) "100"
127.0.0.1:6379> zremrangebyrank scores 0 1
(integer) 2
127.0.0.1:6379> zrange scores 0 -1 withscores
1) "qianba"
2) "92"
3) "lisi"
4) "99"
5) "zhangliu"
6) "100"
127.0.0.1:6379> zcard scores
(integer) 3
127.0.0.1:6379> zcount scores 99 200
(integer) 2
127.0.0.1:6379> zadd s1 50 aa 60 bb 70 cc
(integer) 3
127.0.0.1:6379> zadd s2 60 aa 40 bb 90 aa
(integer) 2
127.0.0.1:6379> zadd s3 70 aa 20 bb 100 dd
(integer) 3
127.0.0.1:6379> zinterstore sss 3 s1 s2 s3   # 求和
(integer) 2
127.0.0.1:6379> zrange sss 0 -1 withscores
1) "bb"
2) "120"
3) "aa"
4) "210"
127.0.0.1:6379> zinterstore ssss 3 s1 s2 s3 aggregate max  # 求最大值
(integer) 2
127.0.0.1:6379> zrange ssss 0 -1 withscores
1) "bb"
2) "60"
3) "aa"
4) "90"
127.0.0.1:6379> help zunionstore

  ZUNIONSTORE destination numkeys key [key ...] [WEIGHTS weight] [AGGREGATE SUM|MIN|MAX]
  summary: Add multiple sorted sets and store the resulting sorted set in a new key
  since: 2.0.0
  group: sorted_set

127.0.0.1:6379> help zinterstore

  ZINTERSTORE destination numkeys key [key ...] [WEIGHTS weight] [AGGREGATE SUM|MIN|MAX]
  summary: Intersect multiple sorted sets and store the resulting sorted set in a new key
  since: 2.0.0
  group: sorted_set


sorted_set 类型数据的扩展操作
获取数据对应的索引（排名）
zrank key member
zrevrank key member
score值获取与修改
zscore key member
zincrby key increment member
Tips 11：
redis 应用于计数器组合排序功能对应的排名

127.0.0.1:6379> zadd movies 143 aa 97 bb 201 cc
(integer) 3
127.0.0.1:6379> zrank movies bb
(integer) 0
127.0.0.1:6379> zrevrank movies bb
(integer) 2
127.0.0.1:6379> zscore movies aa
"143"
127.0.0.1:6379> zincrby movies 1 aa
"144"
127.0.0.1:6379> zscore movies aa
"144"

sorted_set 类型数据操作的注意事项
score保存的数据存储空间是64位，如果是
整数范围是-9007199254740992~9007199254740992
score保存的数据也可以是一个双精度的double值，基
于双精度浮点数的特征，可能会丢失精度，使用时
候要慎重
sorted_set 底层存储还是基于set结构的，因此数据不
能重复，如果重复添加相同的数据，score值将被反
复覆盖，保留最后一次修改的结果
```
#### 2.6.2 时效性任务管理
```markdown
对于基于时间线限定的任务处理，将处理时间记录为score值，
利用排序功能区分处理的先后顺序
记录下一个要处理的时间，当到期后处理对应任务，移除redis中的
记录，并记录下一个要处理的时间
当新任务加入时，判定并更新当前下一个要处理的任务时间
为提升sorted_set的性能，通常将任务根据特征存储成若
干个sorted_set。例如1小时内，1天内，周内，
月内，季内，年度等，操作时逐级提升，将即将操作的若干
个任务纳入到1小时内处理的队列中

获取当前系统时间
time

Tips 12：
redis 应用于定时任务执行顺序管理或任务过期管理
```
```bash
127.0.0.1:6379> zadd ts 10000000000 uid:001
(integer) 1
127.0.0.1:6379> zadd ts 20000000000 uid:007
(integer) 1
127.0.0.1:6379> zadd ts 30000000000 uid:008
(integer) 1
127.0.0.1:6379> zrange ts 0 -1 withscores
1) "uid:001"
2) "10000000000"
3) "uid:007"
4) "20000000000"
5) "uid:008"
6) "30000000000"
127.0.0.1:6379> time
1) "1609590089"
2) "720158"
127.0.0.1:6379> time
1) "1609590096"
2) "577421"
127.0.0.1:6379> time
1) "1609590103"
2) "21331"
```
#### 2.6.3 带有权重的任务管理
```markdown
任务/消息权重设定应用
当任务或者消息待处理，形成了任务队列或消息队列时，对
于高优先级的任务要保障对其优先处理，如何实现任务权重管理。

对于带有权重的任务，优先处理权重高的任务，采用score记
录权重即可
多条件任务权重设定
如果权重条件过多时，需要对排序score值进行处理，保障score值能
够兼容2条件或者多条件，例如外贸
订单优先于国内订单，总裁订单优先于员工订单，经理订单优
先于员工订单
因score长度受限，需要对数据进行截断处理，尤其是时间设置
为小时或分钟级即可（折算后）
先设定订单类别，后设定订单发起角色类别，整体score长度必须
是统一的，不足位补0。第一排序规则首
位不得是0
例如外贸101，国内102，经理004，员工008。
员工下的外贸单score值为101008（优先）
经理下的国内单score值为102004

Tips 13：
redis 应用于即时任务/消息队列执行管理
```
#### 2.6.4 按次结算的服务控制
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210102205720919.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210102205737668.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```bash
业务场景
人工智能领域的语义识别与自动对话将是未来服务业机器人应答
呼叫体系中的重要技术，百度自研用户评价
语义识别服务，免费开放给企业试用，同时训练百度自己
的模型。现对试用用户的使用行为进行限速，限制
每个用户每分钟最多发起10次调用

解决方案
设计计数器，记录调用次数，用于控制业务执行次数。
以用户id作为key，使用次数作为value
在调用前获取次数，判断是否超过限定次数
不超过次数的情况下，每次调用计数+1
业务调用失败，计数-1
为计数器设置生命周期为指定周期，例如1秒/分钟，自动
清空周期内使用次数

127.0.0.1:6379> get 415
(nil)
127.0.0.1:6379> setex 415 60 1
OK
127.0.0.1:6379> get 415
"1"
127.0.0.1:6379> incr 415
(integer) 2
127.0.0.1:6379> incrby 415 7
(integer) 9
127.0.0.1:6379> incr 415
(integer) 10
127.0.0.1:6379> get 415
"10"
127.0.0.1:6379> get 415
"10"
127.0.0.1:6379> get 415
(nil)
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210102205957431.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
取消最大值的判定，利用incr操作超过最大值抛出异常的形式替
代每次判断是否大于最大值
判断是否为nil，
如果是，设置为Max-次数
如果不是，计数+1
业务调用失败，计数-1
遇到异常即+操作超过上限，视为使用达到上限

127.0.0.1:6379> 
127.0.0.1:6379> get 415
(nil)
127.0.0.1:6379> setex 415 60 9223372036854775797
OK
127.0.0.1:6379> get 415
"9223372036854775797"
127.0.0.1:6379> incr 415
(integer) 9223372036854775798
127.0.0.1:6379> get 415
"9223372036854775798"
127.0.0.1:6379> incr 415
(integer) 9223372036854775799
127.0.0.1:6379> incr 415
(integer) 9223372036854775800
127.0.0.1:6379> incr 415
(integer) 9223372036854775801
127.0.0.1:6379> incr 415
(integer) 9223372036854775802
127.0.0.1:6379> incr 415
(integer) 9223372036854775803
127.0.0.1:6379> incr 415
(integer) 9223372036854775804
127.0.0.1:6379> incr 415
(integer) 9223372036854775805
127.0.0.1:6379> incr 415
(integer) 9223372036854775806
127.0.0.1:6379> incr 415
(integer) 9223372036854775807
127.0.0.1:6379> incr 415
(error) ERR increment or decrement would overflow
127.0.0.1:6379> incr 415
(error) ERR increment or decrement would overflow
127.0.0.1:6379> incr 415
(integer) 1
127.0.0.1:6379> incr 415
(integer) 2

Tips 14：
redis 应用于限时按次结算的服务控制
```
#### 2.6.5 微信接受消息顺序控制
```markdown
使用微信的过程中，当微信接收消息后，会默认将最近接收的消
息置顶，当多个好友及关注的订阅号同时发
送消息时，该排序会不停的进行交替。同时还可以将重要的会话设
置为置顶。一旦用户离线后，再次打开微
信时，消息该按照什么样的顺序显示？

依赖list的数据具有顺序的特征对消息进行管理，将list结构作为
栈使用
对置顶与普通会话分别创建独立的list分别管理
当某个list中接收到用户消息后，将消息发送方的id从list的一侧
加入list（此处设定左侧）
多个相同id发出的消息反复入栈会出现问题，在入栈之前无论是否具
有当前id对应的消息，先删除对应id
推送消息时先推送置顶会话list，再推送普通会话list，推送完成
的list清除所有数据
消息的数量，也就是微信用户对话数量采用计数器的思想另行记录，伴
随list操作同步更新

Tips 15：
redis 应用于基于时间顺序的数据操作，而不关注具体时间
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210102211814706.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```bash
127.0.0.1:6379> lrem 100 1 200
(integer) 0
127.0.0.1:6379> lpush 100 200
(integer) 1
127.0.0.1:6379> lrem 100 1 300
(integer) 0
127.0.0.1:6379> lpush 100 300
(integer) 2
127.0.0.1:6379> lrem 100 1 400
(integer) 0
127.0.0.1:6379> lpush 100 400
(integer) 3
127.0.0.1:6379> lrem 100 1 200
(integer) 1
127.0.0.1:6379> lpush 100 200
(integer) 3
127.0.0.1:6379> lrem 100 1 300
(integer) 1
127.0.0.1:6379> lpush 100 300
(integer) 3
127.0.0.1:6379> lrange 100 0 -1
1) "300"
2) "200"
3) "400"

```
### 2.7 解决方案列表
```markdown
Tips 1：redis用于控制数据库表主键id，为数据库表主键提供
生成策略，保障数据库表的主键唯一性
Tips 2：redis 控制数据的生命周期，通过数据是否失效控制业务
行为，适用于所有具有时效性限定控制的操作
Tips 3：redis应用于各种结构型和非结构型高热度数据访问加速
Tips 4：redis 应用于购物车数据存储设计
Tips 5：redis 应用于抢购，限购类、限量发放优惠卷、激活码等业
务的数据存储设计
Tips 6：redis 应用于具有操作先后顺序的数据控制
Tips 7：redis 应用于最新消息展示
Tips 8：redis 应用于随机推荐类信息检索，例如热点歌单
推荐，热点新闻推荐，热卖旅游线路，应用APP推荐，大V推荐等
Tips 9：redis 应用于同类信息的关联搜索，二度关联搜索，深
度关联搜索
Tips 10：redis 应用于同类型不重复数据的合并、取交集操作
Tips 11：redis 应用于同类型数据的快速去重
Tips 12：redis 应用于基于黑名单与白名单设定的服务控制
Tips 13：redis 应用于计数器组合排序功能对应的排名
Tips 14：redis 应用于定时任务执行顺序管理或任务过期管理
Tips 15：redis 应用于及时任务/消息队列执行管理
Tips 16：redis 应用于按次结算的服务控制
Tips 17：redis 应用于基于时间顺序的数据操作，而不关
注具体时间
```
## 3 通用指令
### 3.1 key 基本操作
```markdown
删除指定key
del key
获取key是否存在
exists key
获取key的类型
type key
```
### 3.2 key 扩展操作（时效性控制）
```markdown
为指定key设置有效期
expire key seconds
pexpire key milliseconds
expireat key timestamp
pexpireat key milliseconds-timestamp

获取key的有效时间
ttl key
pttl key

切换key从时效性转换为永久性
persist key
```
### 3.3 查询key
```markdown
keys pattern
查询模式规则
 * 匹配任意数量的任意符号  ? 配合一个任意符号  [] 匹配一个指定符号
keys       *               查询所有
keys      it*                查询所有以it开头
keys      *heima        查询所有以heima结尾
keys      ??heima   查询所有前面两个字符任意，后面以heima结尾
keys     user:?         查询所有以user:开头，最后一个字符任意
keys     u[st]er:1     查询所有以u开头，以er:1结尾，中间包含一个字母，s或t
```
### 3.4 key 其他操作
```markdown
为key改名
rename key newkey
renamenx key newkey
对所有key排序
sort
其他key通用操作
help @generic
```
### 3.5 数据库通用操作
```markdown
redis为每个服务提供有16个数据库，编号从0到15
每个数据库之间的数据相互独立
```
```markdown
db 基本操作
切换数据库
select index
其他操作
quit
ping
echo message

db 相关操作
数据移动
move key db
数据清除
dbsize
flushdb  # 清空当前的库
flushall # 清空全部的库
```
## 4 持久化机制
```markdown
client  redis[内存] ----->  内存数据- 数据持久化-->磁盘
Redis官方提供了两种不同的持久化方法来将数据存储到硬
盘里面分别是:
快照(Snapshot)和AOF
dbfilename dump.rdb
说明：设置本地数据库文件名，默认值为 dump.rdb
经验：通常设置为dump-端口号.rdb
dir
说明：设置存储.rdb文件的路径
经验：通常设置成存储空间较大的目录中，目录名称data
rdbcompression yes
说明：设置存储至本地数据库时是否压缩数据，
默认为 yes，采用 LZF 压缩
经验：通常默认为开启状态，如果设置为no，可以节省 CPU 运行时间，
但会使存储的文件变大（巨大）
rdbchecksum yes
说明：设置是否进行RDB文件格式校验，该校验过程在写文件和读文件过
程均进行
经验：通常默认为开启状态，如果设置为no，可以节约读写性
过程约10%时间消耗，但是存储一定的数据损坏风险

AOF (Append Only File) 只追加日志文件
```
### 4.1 快照(Snapshot)

#### 4.1.1 特点
```markdown
这种方式可以将某一时刻的所有数据都写入硬盘中,当然这也
是redis的默认开启持久化方式,保存的文件是以.rdb形式结尾的文
件,因此这种方式也称之为RDB方式。
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022135201789.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

#### 4.1.2 快照生成方式
```markdown
客户端方式: BGSAVE 和 SAVE指令
服务器配置自动触发
```

##### 4.1.2.1 客户端方式之BGSAVE
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022135225570.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
客户端可以使用BGSAVE命令来创建一个快照,当接收到客户端的
BGSAVE命令时,redis会调用fork¹来创建一个子进程,然后子进程
负责将快照写入磁盘中,而父进程则继续处理命令请求。
	
名词解释: fork当一个进程创建子进程的时候,底层的操作系统会创
建该进程的一个副本,在类unix系统中创建子进程的操作会进行优
化:在刚开始的时候,父子进程共享相同内存,直到父进程或子进程
对内存进行了写之后,对被写入的内存的共享才会结束服务


stop-writes-on-bgsave-error yes
说明：后台存储过程中如果出现错误现象，是否停止保存操作
经验：通常默认为开启状态
```

##### 4.1.2.2 客户端方式之SAVE

```markdown
客户端还可以使用SAVE命令来创建一个快照,接收到SAVE命令
的redis服务器在快照创建完毕之前将不再响应任何其他的命令
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022135242624.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
注意: SAVE命令并不常用,使用SAVE命令在快照创建完毕之
前,redis处于阻塞状态,无法对外服务
```
##### 4.1.2.3 服务器配置方式之满足配置自动触发
```markdown
如果用户在redis.conf中设置了save配置选项,redis会在save选项
条件满足之后自动触发一次BGSAVE命令,如果设置多个save配
置选项,当任意一个save配置选项条件满足,redis也会触发一
次BGSAVE命令

save second changes
作用
满足限定时间范围内key的变化数量达到指定数量即进行持久化
参数
second：监控时间范围
changes：监控key的变化量
位置
在conf文件中进行配置
范例
save 900 1
save 300 10
save 60 10000

注意： save配置要根据实际业务情况进行设置，频度过高或过低
都会出现性能问题，结果可能是灾难性的
save配置中对于second与changes设置通常具有互补对应关系，尽
量不要设置成包含性关系
save配置启动后执行的是bgsave操作
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022135310786.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
##### 4.1.2.4 服务器接收客户端shutdown指令
```markdown
当redis通过shutdown指令接收到关闭服务器的请求时,会执行
一个save命令,阻塞所有的客户端,不再执行客户端执行发送的
任何命令,并且在save命令执行完毕之后关闭服务器
```
#### 4.1.3 配置生成快照名称和位置
```markdown
1.修改生成快照名称
dbfilename dump.rdb

2.修改生成位置
dir ./   # 这个表示redis-cli、redis-server这些命令的同级目录
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102213533755.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
#### 4.1.4 RDB优点、缺点
```markdown
RDB优点
RDB是一个紧凑压缩的二进制文件，存储效率较高
RDB内部存储的是redis在某个时间点的数据快照，非常适合
用于数据备份，全量复制等场景
RDB恢复数据的速度要比AOF快很多
应用：服务器中每X小时执行bgsave备份，并将RDB文件拷贝
到远程机器中，用于灾难恢复。

Rdb缺点
RDB方式无论是执行指令还是利用配置，无法做到实时持久化，
具有较大的可能性丢失数据
bgsave指令每次运行要执行fork操作创建子进程，要牺牲掉一些性能
Redis的众多版本中未进行RDB文件格式的版本统一，有可能出现各
版本服务之间数据格式无法兼容现象
```
### 4.2 AOF 只追加日志文件
#### 4.2.1 特点
```markdown
这种方式可以将所有客户端执行的写命令记录到日志文件中,AOF持
久化会将被执行的写命令写到AOF的文件末尾,以此来记录数据发
生的变化,因此只要redis从头到尾执行一次AOF文件所包含的所有
写命令,就可以恢复AOF文件的记录的数据集.

dir
AOF持久化文件保存路径，与RDB持久化文件保持一致即可
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022135402438.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

#### 4.2.2 开启AOF持久化
```markdown
在redis的默认配置中AOF持久化机制是没有开启的，需要在
配置中开启
```
```markdown
1.开启AOF持久化
a.修改 appendonly yes 开启持久化
b.修改 appendfilename "appendonly.aof" 指定生成文件名称
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022135424974.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

#### 4.2.3 日志追加频率

```markdown
1.always 【谨慎使用】
说明: 每个redis写命令都要同步写入硬盘,严重降低redis速度
解释: 如果用户使用了always选项,那么每个redis写命令都会被
写入硬盘,从而将发生系统崩溃时出现的数据丢失减到最少;遗憾
的是,因为这种同步策略需要对硬盘进行大量的写入操作,所以
redis处理命令的速度会受到硬盘性能的限制;
注意: 转盘式硬盘在这种频率下200左右个命令/s ; 
固态硬盘(SSD) 几百万个命令/s;
警告: 使用SSD用户请谨慎使用always选项,这种模式不断写入少量
数据的做法有可能会引发严重的写入放大问题,导致将固态硬盘的
寿命从原来的几年降低为几个月。

2.everysec 【推荐】
说明: 每秒执行一次同步显式的将多个写命令同步到磁盘
解释： 为了兼顾数据安全和写入性能,用户可以考虑使
用everysec选项,让redis每秒一次的频率对AOF文件进行
同步;redis每秒同步一次AOF文件时性能和不使用任何持久
化特性时的性能相差无几,而通过每秒同步一次AOF文件,redis可
以保证,即使系统崩溃,用户最多丢失一秒之内产生的数据。

3.no	【不推荐】
说明: 由操作系统决定何时同步 
解释：最后使用no选项,将完全有操作系统决定什么时候同
步AOF日志文件,这个选项不会对redis性能带来影响但是系统
崩溃时,会丢失不定数量的数据,另外如果用户硬盘处理写入操
作不够快的话,当缓冲区被等待写入硬盘数据填满时,redis会
处于阻塞状态,并导致redis的处理命令请求的速度变慢。
```

#### 4.2.4 修改同步频率

```markdown
1.修改日志同步频率
修改appendfsync everysec|always|no 指定
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022135447843.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 4.3 AOF文件的重写
#### 4.3.1 AOF带来的问题
```markdown
AOF的方式也同时带来了另一个问题。持久化文件会变的越来
越大。例如我们调用incr test命令100次，文件中必须保存全部
的100条命令，其实有99条都是多余的。因为要恢复数据库的状
态其实文件中保存一条set test 100就够了。为了压缩aof的持
久化文件Redis提供了AOF重写(ReWriter)机制。
```
#### 4.3.2 AOF重写
```markdown
用来在一定程度上减小AOF文件的体积
```
#### 4.3.3 触发重写方式

```markdown
1.客户端方式触发重写
执行BGREWRITEAOF命令  不会阻塞redis的服务

2.服务器配置方式自动触发
配置redis.conf中的auto-aof-rewrite-percentage选项 参加下图↓↓↓
如果设置auto-aof-rewrite-percentage值为100
和auto-aof-rewrite-min-size 64mb,并且启用的
AOF持久化时,那么当AOF文件体积大于64M,
并且AOF文件的体积比上一次重写之后体积大了
至少一倍(100%)时,会自动触发,如果重写过于频繁,
用户可以考虑将auto-aof-rewrite-percentage设置为更大
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022135524819.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

#### 4.3.4 重写原理
```markdown
注意：重写aof文件的操作，并没有读取旧的aof文件，
而是将整个内存中的数据库内容用命令的方式重写了一个
新的aof文件,替换原有的文件这点和快照有点类似。
```
```markdown
重写流程
1. redis调用fork ，现在有父子两个进程 子进程根据内存中的
数据库快照，往临时文件中写入重建数据库状态的命令
2. 父进程继续处理client请求，除了把写命令写入到原来的aof文
件中。同时把收到的写命令缓存起来。这样就能保证如果子进
程重写失败的话并不会出问题。
3. 当子进程把快照内容写入已命令方式写到临时文件中后，子
进程发信号通知父进程。然后父进程把缓存的写命令也写入
到临时文件。
4. 现在父进程可以使用临时文件替换老的aof文件，并重命名，
后面收到的写命令也开始往新的aof文件中追加。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022135548750.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

### 4.4 持久化总结
```markdown
两种持久化方案既可以同时使用(aof),又可以单独使用,在某种情
况下也可以都不使用,具体使用那种持久化方案取决于用户的
数据和应用决定。
无论使用AOF还是快照机制持久化,将数据持久化到硬盘都是有
必要的,除了持久化外,用户还应该对持久化的文件进行备份
(最好备份在多个不同地方)。
```
## 5 java操作Redis
### 5.1 环境准备
#### 5.1.1 引入依赖
```xml
<!--引入jedis连接依赖-->
<dependency>
  <groupId>redis.clients</groupId>
  <artifactId>jedis</artifactId>
  <version>2.9.0</version>
</dependency>
```

#### 5.1.2 创建jedis对象
```java
package com.package com.yxj.test;

import redis.clients.jedis.Jedis;

import java.util.Set;

//测试redis连接
public class TestRedis {

    public static void main(String[] args) {

        //创建jedis客户端对象
        Jedis jedis = new Jedis("192.168.202.205",7000);
        //选择使用一个库  默认:使用 0号库
        jedis.select(0);


        //获取redis中所有key信息
        Set<String> keys = jedis.keys("*");
        keys.forEach(key-> System.out.println("key = " + key));

        //操作库相关
        //jedis.flushDB();//清空当前库
        jedis.flushAll();//清空所有库


        //释放资源
        jedis.close();
    }


}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022135628340.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

### 5.2 操作key相关API
```java
package com.yxj.test;

import org.junit.After;
import org.junit.Before;
import org.junit.Test;
import redis.clients.jedis.Jedis;

public class TestKey {
    private Jedis jedis;

    @Before
    public void before(){
        this.jedis = new Jedis("192.168.202.205", 7000);
    }

    @After
    public void after(){
        jedis.close();
    }


    //测试key相关
    @Test
    public void testKeys(){
        //删除一个key
        jedis.del("name");
        //删除多个key
        //jedis.del("name","age");

        //判断一个key是否存在exits
        Boolean name = jedis.exists("name");
        System.out.println(name);

        //设置一个key超时时间 expire pexpire
        //Long age = jedis.expire("age", 100);
        //System.out.println(age);

        //获取一个key超时时间 ttl
        Long age1 = jedis.ttl("newage");
        System.out.println(age1);

        //随机获取一个key
        String s = jedis.randomKey();

        //修改key名称
       // jedis.rename("age","newage");

        //查看可以对应值的类型
        String name1 = jedis.type("name");
        System.out.println(name1);
        String maps = jedis.type("maps");
        System.out.println(maps);
    }

}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022135649322.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 5.3 操作String相关API

```java
package com.yxj.test;

import org.junit.After;
import org.junit.Before;
import org.junit.Test;
import redis.clients.jedis.Jedis;

import java.util.List;

public class TestString {
    private Jedis jedis;

    @Before
    public void before(){
        this.jedis = new Jedis("192.168.202.205", 7000);
    }

    @After
    public void after(){
        jedis.close();
    }


    //测试String相关
    @Test
    public void testString(){
        //set
        jedis.set("name","小陈");
        //get
        String s = jedis.get("name");
        System.out.println(s);
        //mset
        jedis.mset("content","好人","address","海淀区");
        //mget
        List<String> mget = jedis.mget("name", "content", "address");
        mget.forEach(v-> System.out.println("v = " + v));
        //getset
        String set = jedis.getSet("name", "小明");
        System.out.println(set);

        //............
    }

}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022135711940.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

### 5.4 操作List相关API

```java
package com.yxj.test;

import org.junit.After;
import org.junit.Before;
import org.junit.Test;
import redis.clients.jedis.BinaryClient;
import redis.clients.jedis.Jedis;

import java.util.List;

public class TestList {
    private Jedis jedis;

    @Before
    public void before(){
        this.jedis = new Jedis("192.168.202.205", 7000);
    }

    @After
    public void after(){
        jedis.close();
    }


    //测试List相关
    @Test
    public void testList(){

        //lpush
        jedis.lpush("names1","张三","王五","赵柳","win7");

        //rpush
        jedis.rpush("names1","xiaomingming");

        //lrange

        List<String> names1 = jedis.lrange("names1", 0, -1);
        names1.forEach(name-> System.out.println("name = " + name));

        //lpop rpop
        String names11 = jedis.lpop("names1");
        System.out.println(names11);

        //llen
        jedis.linsert("lists", BinaryClient.LIST_POSITION.BEFORE,"xiaohei","xiaobai");


    }

}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022135736536.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


### 5.5 操作Set的相关API

```java
package com.yxj.test;

import org.junit.After;
import org.junit.Before;
import org.junit.Test;
import redis.clients.jedis.Jedis;

public class TestSet {
    private Jedis jedis;

    @Before
    public void before(){
        this.jedis = new Jedis("192.168.202.205", 7000);
    }

    @After
    public void after(){
        jedis.close();
    }


    //测试SET相关
    @Test
    public void testSet(){

        //sadd
        jedis.sadd("names","zhangsan","lisi");

        //smembers
        jedis.smembers("names");

        //sismember
        jedis.sismember("names","xiaochen");

        //...
    }

}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102213575652.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


### 5.6 操作ZSet相关API

```java
package com.yxj.test;

import org.junit.After;
import org.junit.Before;
import org.junit.Test;
import redis.clients.jedis.BinaryClient;
import redis.clients.jedis.Jedis;

import java.util.List;

public class TestZSet {
    private Jedis jedis;

    @Before
    public void before(){
        this.jedis = new Jedis("192.168.202.205", 7000);
    }

    @After
    public void after(){
        jedis.close();
    }


    //测试ZSET相关
    @Test
    public void testZset(){

        //zadd
        jedis.zadd("names",10,"张三");

        //zrange
        jedis.zrange("names",0,-1);

        //zcard
        jedis.zcard("names");

        //zrangeByScore
        jedis.zrangeByScore("names","0","100",0,5);

        //..

    }

}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022135816534.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 5.7 操作Hash相关API
```java
package com.yxj.test;

import org.junit.After;
import org.junit.Before;
import org.junit.Test;
import redis.clients.jedis.Jedis;

public class TestHash {
    private Jedis jedis;

    @Before
    public void before(){
        this.jedis = new Jedis("192.168.202.205", 7000);
    }

    @After
    public void after(){
        jedis.close();
    }


    //测试HASH相关
    @Test
    public void testHash(){
        //hset
        jedis.hset("maps","name","zhangsan");
        //hget
        jedis.hget("maps","name");
        //hgetall
        jedis.hgetAll("mps");
        //hkeys
        jedis.hkeys("maps");
        //hvals
        jedis.hvals("maps");
        //....
    }

}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022135834638.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 5.8 服务调用次数控制
```markdown
限制每个用户规定时间内最多发起调用的次数
```
```java
package com.itheima.util;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.JedisPoolConfig;

import java.util.ResourceBundle;

public class JedisUtils {
    private static JedisPool jp = null;
    private static String host = null;
    private static int port;
    private static int maxTotal;
    private static int maxIdle;

    static {
        ResourceBundle rb = ResourceBundle.getBundle("redis");
        host = rb.getString("redis.host");
        port = Integer.parseInt(rb.getString("redis.port"));
        maxTotal = Integer.parseInt(rb.getString("redis.maxTotal"));
        maxIdle = Integer.parseInt(rb.getString("redis.maxIdle"));
        JedisPoolConfig jpc = new JedisPoolConfig();
        jpc.setMaxTotal(maxTotal);
        jpc.setMaxIdle(maxIdle);
        jp = new JedisPool(jpc,host,port);
    }

    public static Jedis getJedis(){
        return jp.getResource();
    }
    public static void main(String[] args){
        JedisUtils.getJedis();
    }
}

```
```properties
redis.host=127.0.0.1
redis.port=6379
redis.maxTotal=30
redis.maxIdle=10
```
```java
package com.itheima;

import com.itheima.util.JedisUtils;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.exceptions.JedisDataException;

public class Service {
    private String id;
    private int num;

    public Service(String id,int num){
        this.id = id;
        this.num = num;
    }
    //控制单元
    public void service(){
//        Jedis jedis = new Jedis("127.0.0.1",6379);
        Jedis jedis = JedisUtils.getJedis();
        String value = jedis.get("compid:"+id);
        //判断该值是否存在
        try{
            if(value == null){
                //不存在，创建该值
                jedis.setex("compid:"+id,5,Long.MAX_VALUE-num+"");
            }else{
                //存在，自增，调用业务
                Long val = jedis.incr("compid:"+id);
                business(id,num-(Long.MAX_VALUE-val));
            }
        }catch (JedisDataException e){
            System.out.println("使用已经到达次数上限，请升级会员级别");
            return;
        }finally{
            jedis.close();
        }
    }
    //业务操作
    public void business(String id,Long val){
        System.out.println("用户:"+id+" 业务操作执行第"+val+"次");
    }
}

class MyThread extends Thread{
    Service sc ;
    public MyThread(String id,int num){
        sc = new Service(id,num);
    }
    public void run(){
        while(true){
            sc.service();
            try {
                Thread.sleep(300L);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

class Main{
    public static void main(String[] args) {
        MyThread mt1 = new MyThread("初级用户",10);
        MyThread mt2 = new MyThread("高级用户",30);
        mt1.start();
        mt2.start();
    }
}
```
## 6 SpringBoot整合Redis
```markdown
Spring Boot Data(数据) Redis 中提供了
RedisTemplate和StringRedisTemplate，
其中StringRedisTemplate是RedisTemplate的子类，
两个方法基本一致，不同之处主要体现在操作的数据类型
不同，RedisTemplate中的两个泛型都是Object，意味着存
储的key和value都可以是一个对象，而StringRedisTemplate的
两个泛型都是String，意味着StringRedisTemplate的key
和value都只能是字符串。

注意: 使用RedisTemplate默认是将对象序列
化到Redis中,所以放入的对象必须实现对象序列化接口
```
### 6.1 环境准备
#### 6.1.1 引入依赖
```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

#### 6.1.2 配置application.propertie

```properties
spring.redis.host=localhost
spring.redis.port=6379
spring.redis.database=0
```

### 6.2 使用StringRedisTemplate、RedisTemplate和BoundAPI
#### 6.2.1 StringRedisTemplate
```java
package com.yxj;


import com.yxj.entity.User;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.redis.connection.DataType;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.data.redis.core.ZSetOperations;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.*;
import java.util.concurrent.TimeUnit;


//启动springboot应用
@SpringBootTest(classes = RedisDay2Application.class)
@RunWith(SpringRunner.class)
public class TestStringRedisTemplate {


    //注入StringRedisTemplate
    @Autowired
    private StringRedisTemplate stringRedisTemplate;  //key  value 都是字符串


    //操作redis中key相关
    @Test
    public void testKey(){
        //stringRedisTemplate.delete("name");//删除一个key
        Boolean hasKey = stringRedisTemplate.hasKey("name");//判断某个key是否存在
        System.out.println(hasKey);


        DataType name = stringRedisTemplate.type("name");//判断key所对应值的类型
        System.out.println(name);


        Set<String> keys = stringRedisTemplate.keys("*");//获取redis中所有key
        keys.forEach(key -> System.out.println("key = " + key));

        Long expire = stringRedisTemplate.getExpire("age");//获取key超时时间 -1 永不超时  -2  key不存在 >=0 过期时间
        System.out.println(expire);

        stringRedisTemplate.randomKey();//在redis中随机获取一个key

        //stringRedisTemplate.rename("age","age1");//修改key名字 要求key必须存在 不存在 报错

        //stringRedisTemplate.renameIfAbsent("name","name1");//修改key名字  判断key是否存在

        stringRedisTemplate.move("name1",1);//移动key到指定库

    }

    //操作redis中字符串 opsForValue 实际操作就是redis中String类型
    @Test
    public void testString(){
        stringRedisTemplate.opsForValue().set("name","小陈"); //set 用来设置一个key value

        String value= stringRedisTemplate.opsForValue().get("name"); //用来获取一个key对应value
        System.out.println("value = " + value);

        stringRedisTemplate.opsForValue().set("code","2357",120, TimeUnit.SECONDS);//设置一个key 超时时间

        stringRedisTemplate.opsForValue().append("name","他是是一个好人,单纯少年!");//追加

    }

    //操作redis中list类型   opsForList 实际操作就是redis中list类型
    @Test
    public void testList(){
        //stringRedisTemplate.opsForList().leftPush("names","小陈");//创建一个列表  并放入一个元素
        //stringRedisTemplate.opsForList().leftPushAll("names","小陈","小张","小王");//创建一个列表 放入多个元素
        List<String> names = new ArrayList<>();
        names.add("xiaoming");
        names.add("xiaosan");
        //stringRedisTemplate.opsForList().leftPushAll("names",names);//创建一个列表 放入多个元素

        List<String> stringList = stringRedisTemplate.opsForList().range("names", 0, -1); //遍历list
        stringList.forEach(value-> System.out.println("value = " + value));

        stringRedisTemplate.opsForList().trim("names",1,3); //截取指定区间的list
    }

    //操作redis中set类型   opsForSet 实际操作就是redis中set类型
    @Test
    public void testSet(){
        stringRedisTemplate.opsForSet().add("sets","张三","张三","小陈","xiaoming");//创建set 并放入多个元素


        Set<String> sets = stringRedisTemplate.opsForSet().members("sets");//查看set中成员
        sets.forEach(value-> System.out.println("value = " + value));

        Long size = stringRedisTemplate.opsForSet().size("sets");//获取set集合元素个数
        System.out.println("size = " + size);
    }

    //操作redis中Zset类型   opsForZSet 实际操作就是redis中Zset类型
    @Test
    public void testZset(){
        stringRedisTemplate.opsForZSet().add("zsets","小黑",20);//创建并放入元素

        Set<String> zsets = stringRedisTemplate.opsForZSet().range("zsets", 0, -1);//指定范围查询

        zsets.forEach(value-> System.out.println(value));
        System.out.println("=====================================");
        Set<ZSetOperations.TypedTuple<String>> zsets1 = stringRedisTemplate.opsForZSet().rangeByScoreWithScores("zsets", 0, 1000);//获取指定元素以及分数

        zsets1.forEach(typedTuple ->{
            System.out.println(typedTuple.getValue());
            System.out.println(typedTuple.getScore());
        });

    }

    //操作redis中Hash类型   opsForHash 实际操作就是redis中Hash类型

    @Test
    public void testHash(){

        stringRedisTemplate.opsForHash().put("maps","name","张三");//创建一个hash类型 并放入key value

        Map<String,String> map =  new HashMap<String,String>();
        map.put("age","12");
        map.put("bir","2012-12-12");
        stringRedisTemplate.opsForHash().putAll("maps",map);  //放入多个key value


        List<Object> values = stringRedisTemplate.opsForHash().multiGet("maps", Arrays.asList("name", "age"));//获取多个key的value
        values.forEach(value-> System.out.println(value));

        String value  = (String) stringRedisTemplate.opsForHash().get("maps", "name");//获取hash中某个key的值

        List<Object> vals = stringRedisTemplate.opsForHash().values("maps");//获取所有values

        Set<Object> keys = stringRedisTemplate.opsForHash().keys("maps");//获取所有keys


    }



}

```
#### 6.2.2 RedisTemplate
```java
package com.yxj.entity;

import lombok.Data;
import lombok.experimental.Accessors;

import java.io.Serializable;
import java.util.Date;

@Data
@Accessors(chain = true)
public class User implements Serializable {

    private String id;
    private String name;
    private Integer age;
    private Date bir;

}

```
```java
package com.yxj;


import com.yxj.entity.User;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.cache.CacheProperties;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.redis.connection.DataType;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.data.redis.core.ZSetOperations;
import org.springframework.data.redis.serializer.JdkSerializationRedisSerializer;
import org.springframework.data.redis.serializer.RedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.*;
import java.util.concurrent.TimeUnit;


//启动springboot应用
@SpringBootTest(classes = RedisDay2Application.class)
@RunWith(SpringRunner.class)
public class TestRedisTemplate {


    //注入RedisTemplate key Object  Value Object  ===>   对象序列化   name  new User() ====>   name序列化  对象序列化结果
    @Autowired
    private RedisTemplate redisTemplate;


    //opsForxxx  Value String  List  Set  Zset  hash

    @Test
    public void testRedisTemplate(){

        /**
         * redisTemplate对象中 key 和 value 的序列化都是 JdkSerializationRedisSerializer
         *      key: string
         *      value: object
         *      修改默认key序列化方案 :  key  StringRedisSerializer
         */

        //修改key序列化方案   String类型序列
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        //修改hash key 序列化方案
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());

        User user = new User();
        user.setId(UUID.randomUUID().toString()).setName("小陈").setAge(23).setBir(new Date());
        redisTemplate.opsForValue().set("user", user);//redis进行设置 对象需要经过序列化

        User user1 = (User) redisTemplate.opsForValue().get("user");
        System.out.println(user1);


        redisTemplate.opsForList().leftPush("list",user);

        redisTemplate.opsForSet().add("set",user);

        redisTemplate.opsForZSet().add("zset",user,10);

        redisTemplate.opsForHash().put("map","name",user);


    }


}

```
#### 6.2.3 BoundAPI(简化操作)

```java
package com.yxj;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.redis.core.BoundListOperations;
import org.springframework.data.redis.core.BoundValueOperations;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.data.redis.serializer.StringRedisSerializer;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.List;

@SpringBootTest(classes = RedisDay2Application.class)
@RunWith(SpringRunner.class)
public class TestBoundAPI {

    @Autowired
    private RedisTemplate redisTemplate;

    @Autowired
    private StringRedisTemplate stringRedisTemplate;


    //spring data 为了方便我们对redis进行更友好的操作 因此有提供了bound api 简化操作
    @Test
    public void testBound(){

        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());


        //redisTemplate   stringRedisTemplate  将一个key多次操作进行绑定 对key绑定
        //stringRedisTemplate.opsForValue().set("name","zhangsan");
        //stringRedisTemplate.opsForValue().append("name","是一个好人");
        //String s = stringRedisTemplate.opsForValue().get("name");
        //System.out.println(s);


        //对字符串类型key进行绑定 后续所有操作都是基于这个key的操作

        BoundValueOperations<String, String> nameValueOperations = stringRedisTemplate.boundValueOps("name");
        nameValueOperations.set("zhangsan");
        nameValueOperations.append("是一个好人");
        String s1 = nameValueOperations.get();
        System.out.println(s1);

        //对list set zset hash
        BoundListOperations<String, String> listsOperations = stringRedisTemplate.boundListOps("lists");
        listsOperations.leftPushAll("张三","李四","小陈");
        List<String> lists = listsOperations.range(0, -1);
        lists.forEach(list-> System.out.println(list));

        //set
        //redisTemplate.boundSetOps();
        //stringRedisTemplate.boundSetOps()
        //zset
        //stringRedisTemplate.boundZSetOps();
        //redisTemplate.boundZSetOps();
        //hash
        //stringRedisTemplate.boundHashOps();
        //redisTemplate.boundHashOps()


        /**
         * 1.针对于日后处理key value 都是 String 使用 StringRedisTemplate
         * 2.针对于日后处理的key value 存在对象 使用 RedisTemplate
         * 3.针对于同一个key多次操作可以使用boundXXxOps() Value List Set Zset Hash的api 简化书写
         */

        /**
         * redis应用场景
         *
         *  1.利用redis 中字符串类型完成 项目中手机验证码存储的实现
         *  2.利用redis 中字符串类型完成 具有失效性业务功能  12306  淘宝  订单还有:40分钟
         *  3.利用redis 分布式集群系统中 Session共享  memcache 内存 数据存储上限 数据类型比较简单  redis 内存  数据上限  数据类型丰富
         *  4.利用redis zset类型 可排序set类型  元素 分数  排行榜之类功能   dangdang 销量排行  sales(zset) [商品id,商品销量] ......
         *  5.利用redis 分布式缓存  实现
         *  6.利用redis 存储认证之后token信息   微信小程序 微信公众号 |用户 openid   ---> 令牌(token) redis 超时
         *  7.利用redis 解决分布式集群系统中分布式锁问题       redis 单进程 单线程   n 20 定义
         *      jvm  1进程开启多个线程 synchronize int n=20
         *      jvm  1进程开启多个线程 synchronize int n=20
         *      .....  LRA脚本
         *
         */

    }


}

```
## 7 分布式缓存
```xml
mybatis是里面有个<cache></cache>便签，可以实现分布式
缓存，只要把里面的实现替换成redis的实现，就可以实现分
布式缓存。注意，增删改都会清除缓存。
```
```markdown
1 pom.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.5.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.cqm</groupId>
    <artifactId>redis_day2</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>redis_day2</name>
    <description>SpringBoot Data Redis Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>



        <!--引入测试-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>



        <!--引入依赖 spring data redis依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>


        <!--mybatis-->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.3</version>
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

    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

```
```markdown
2 application.properties
```
```properties
server.port=8989

redis 单节点(这个是单节点的配置，即主从复制或者没有主从配置)
spring.redis.host=192.168.42.137
spring.redis.port=6377
spring.redis.database=0

spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/2001?characterEncoding=UTF-8
spring.datasource.username=root
spring.datasource.password=root

mybatis.mapper-locations=classpath:com/cqm/mapper/*.xml
mybatis.type-aliases-package=com.cqm.entity


logging.level.com.cqm.dao=debug
```
```markdown
3 启动类
```
```java
package com.cqm;

import org.apache.ibatis.annotations.Mapper;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
@MapperScan("com.cqm.dao")
public class RedisDay2Application {

    public static void main(String[] args) {
        SpringApplication.run(RedisDay2Application.class, args);
    }

}

```
```markdown
4 cache、dao、entity、service、utilc层的编写
```
```markdown
service
```
```java
package com.cqm.service;

import com.cqm.entity.Emp;

import java.util.List;

public interface EmpService {

    List<Emp> findAll();
}


package com.cqm.service;

import com.cqm.dao.EmpDAO;
import com.cqm.entity.Emp;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Propagation;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

@Service
@Transactional
public class EmpServiceImpl implements EmpService {

    @Autowired
    private EmpDAO empDAO;


    @Override
    @Transactional(propagation = Propagation.SUPPORTS)
    public List<Emp> findAll() {
        return empDAO.findAll();
    }
}


package com.cqm.service;

import com.cqm.entity.User;

import java.util.List;

public interface UserService {

    List<User> findAll();

    User findById(String id);

    void delete(String id);

    void save(User user);

    void update(User user);
}


package com.cqm.service;

import com.cqm.dao.UserDAO;
import com.cqm.entity.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Propagation;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;
import java.util.UUID;

@Service
@Transactional
public class UserServiceImpl implements UserService {

    @Autowired
    private UserDAO userDAO;


    @Override
    @Transactional(propagation = Propagation.SUPPORTS)
    public User findById(String id) {
        return userDAO.findById(id);
    }

    @Override
    @Transactional(propagation = Propagation.SUPPORTS)
    public List<User> findAll() {
        return userDAO.findAll();
    }

    @Override
    public void update(User user) {
        userDAO.update(user);
    }

    @Override
    public void save(User user) {
        user.setId(UUID.randomUUID().toString());
        userDAO.save(user);
    }

    @Override
    public void delete(String id) {
        userDAO.delete(id);
    }
}

```
```markdown
dao
```
```java
package com.cqm.dao;

import com.cqm.entity.Emp;

import java.util.List;

public interface EmpDAO {


    List<Emp> findAll();

}


package com.cqm.dao;

import com.cqm.entity.User;

import java.util.List;

public interface UserDAO {

    List<User> findAll();

    User findById(String id);

    void delete(String id);

    void save(User user);

    void update(User user);
}

```
```markdown
entity
```

```java
package com.cqm.entity;

import lombok.Data;
import lombok.experimental.Accessors;

import java.io.Serializable;

@Data
@Accessors(chain = true)
public class Emp implements Serializable {
    private String id;
    private String name;
}


package com.cqm.entity;

import lombok.Data;
import lombok.experimental.Accessors;

import java.io.Serializable;
import java.util.Date;

@Data
@Accessors(chain = true)
public class User implements Serializable {

    private String id;
    private String name;
    private Integer age;
    private Date bir;

}

```
```markdown
util
```
```java
package com.cqm.util;

import org.springframework.beans.BeansException;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;
import org.springframework.context.annotation.Configuration;
import org.springframework.stereotype.Component;

//用来获取springboot创建好的工厂
@Component
public class ApplicationContextUtils implements ApplicationContextAware {

    //保留下来工厂
    private static ApplicationContext applicationContext;

    //将创建好工厂以参数形式传递给这个类
    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        this.applicationContext = applicationContext;
    }



    //提供在工厂中获取对象的方法 //RedisTemplate  redisTemplate
    public static Object getBean(String beanName){
        return applicationContext.getBean(beanName);
    }

}

```
```markdown
cache
```

```java
package com.cqm.cache;

import com.cqm.util.ApplicationContextUtils;
import org.apache.ibatis.cache.Cache;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.StringRedisSerializer;
import org.springframework.util.DigestUtils;

import java.util.concurrent.TimeUnit;

//自定义Redis缓存实现
public class RedisCache implements Cache {


    //当前放入缓存的mapper的namespace
    private final String id;

    //必须存在构造方法
    public RedisCache(String id) {
        System.out.println("id:=====================> " + id);
        this.id = id;
    }

    //返回cache唯一标识
    @Override
    public String getId() {
        return this.id;
    }


    //缓存放入值  redis RedisTemplate   StringRedisTemplate
    @Override
    public void putObject(Object key, Object value) {
        System.out.println("key:" + key.toString());
        System.out.println("value:" + value);
//        //通过application工具类获取redisTemplate
//        RedisTemplate redisTemplate = (RedisTemplate) ApplicationContextUtils.getBean("redisTemplate");
//        redisTemplate.setKeySerializer(new StringRedisSerializer());
//        redisTemplate.setHashKeySerializer(new StringRedisSerializer());

        //使用redishash类型作为缓存存储模型  key   hashkey  value
        getRedisTemplate().opsForHash().put(id.toString(),getKeyToMD5(key.toString()),value);



        if(id.equals("com.cqm.dao.UserDAO")){
            //缓存超时  client  用户   client  员工
            getRedisTemplate().expire(id.toString(),1, TimeUnit.HOURS);
        }


        if(id.equals("com.cqm.dao.CityDAO")){
            //缓存超时  client  用户   client  员工
            getRedisTemplate().expire(id.toString(),30, TimeUnit.MINUTES);
        }

        //.....指定不同业务模块设置不同缓存超时时间




    }

    //获取中获取数据
    @Override
    public Object getObject(Object key) {
        System.out.println("key:" + key.toString());
//        //通过application工具类获取redisTemplate
//        RedisTemplate redisTemplate = (RedisTemplate) ApplicationContextUtils.getBean("redisTemplate");
//        redisTemplate.setKeySerializer(new StringRedisSerializer());
//        redisTemplate.setHashKeySerializer(new StringRedisSerializer());

        //根据key 从redis的hash类型中获取数据
        return getRedisTemplate().opsForHash().get(id.toString(), getKeyToMD5(key.toString()));
    }


    //注意:这个方法为mybatis保留方法 默认没有实现 后续版本可能会实现
    @Override
    public Object removeObject(Object key) {
        System.out.println("根据指定key删除缓存");
        return null;
    }

    @Override
    public void clear() {
        System.out.println("清空缓存~~~");
        //清空namespace
        getRedisTemplate().delete(id.toString());//清空缓存
    }

    //用来计算缓存数量
    @Override
    public int getSize() {
        //获取hash中key value数量
        return getRedisTemplate().opsForHash().size(id.toString()).intValue();
    }


    //封装redisTemplate
    private RedisTemplate getRedisTemplate(){
        //通过application工具类获取redisTemplate
        RedisTemplate redisTemplate = (RedisTemplate) ApplicationContextUtils.getBean("redisTemplate");
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        return redisTemplate;
    }


    //封装一个对key进行md5处理方法
    private String getKeyToMD5(String key){
        return DigestUtils.md5DigestAsHex(key.getBytes());
    }

}

```
### 7.1 缓存雪崩
```markdown
缓存雪崩就是瞬间过期数据量太大，导致对数据库服务器造
成压力。如能够有效避免过期时间集中，可以有效解决雪崩
现象的出现（约40%），配合其他策略一起使用，并监控
服务器的运行数据，根据运行记录做快速调整。
```
```markdown
数据库服务器崩溃（1）
1. 系统平稳运行过程中，忽然数据库连接量激增
2. 应用服务器无法及时处理请求
3. 大量408，500错误页面出现
4. 客户反复刷新页面获取数据
5. 数据库崩溃
6. 应用服务器崩溃
7. 重启应用服务器无效
8. Redis服务器崩溃
9. Redis集群崩溃
10. 重启数据库后再次被瞬间流量放倒

问题排查
11. 在一个较短的时间内，缓存中较多的key集中过期
12. 此周期内请求访问过期的数据，redis未命中，
redis向数据库获取数据
13. 数据库同时接收到大量的请求无法及时处理
14. Redis大量请求被积压，开始出现超时现象
15. 数据库流量激增，数据库崩溃
16. 重启后仍然面对缓存中无数据可用
17. Redis服务器资源被严重占用，Redis服务器崩溃
18. Redis集群呈现崩塌，集群瓦解
19. 应用服务器无法及时得到数据响应请求，来自客户端的请求数
量越来越多，应用服务器崩溃
20. 应用服务器，redis，数据库全部重启，效果不理想
问题分析
短时间范围内
大量key集中过期

解决方案（道）
21. 更多的页面静态化处理
22. 构建多级缓存架构
Nginx缓存+redis缓存+ehcache缓存
23. 检测Mysql严重耗时业务进行优化
对数据库的瓶颈排查：例如超时查询、耗时较高事务等
24. 灾难预警机制
监控redis服务器性能指标
CPU占用、CPU使用率
内存容量
查询平均响应时间
线程数
25. 限流、降级
短时间范围内牺牲一些客户体验，限制一部分请求访问，降低
应用服务器压力，待业务低速运转后再逐步放开访问

解决方案（术）
26. LRU与LFU切换
27. 数据有效期策略调整
根据业务数据有效期进行分类错峰，A类90分钟，B类80分钟，
C类70分钟
过期时间使用固定时间+随机值的形式，稀释集中到期的key的数量
28. 超热数据使用永久key
29. 定期维护（自动+人工）
对即将过期数据做访问量分析，确认是否延时，配合访问量统计，
做热点数据的延时
30. 加锁
慎用！
```

### 7.2 缓存击穿
```markdown
缓存击穿就是单个高热数据过期的瞬间，数据访问量较大，
未命中redis后，发起了大量对同一数据的数据库访问，导致对
数据库服务器造成压力。应对策略应该在业务数据分析与预防
方面进行，配合运行监控测试与即时调整策略，毕竟单个key的
过期监控难度较高，配合雪崩处理策略即可。
```
```markdown
数据库服务器崩溃（2）
1. 系统平稳运行过程中
2. 数据库连接量瞬间激增
3. Redis服务器无大量key过期
4. Redis内存平稳，无波动
5. Redis服务器CPU正常
6. 数据库崩溃

问题排查
7. Redis中某个key过期，该key访问量巨大
8. 多个数据请求从服务器直接压到Redis后，均未命中
9. Redis在短时间内发起了大量对数据库中同一数据的访问

问题分析
单个key高热数据
key过期

解决方案（术）
1. 预先设定
以电商为例，每个商家根据店铺等级，指定若干款主打商品，
在购物节期间，加大此类信息key的过期时长
注意：购物节不仅仅指当天，以及后续若干天，访问峰值呈现
逐渐降低的趋势
2. 现场调整
监控访问量，对自然流量激增的数据延长过期时间或设置为永久性key
3. 后台刷新数据
启动定时任务，高峰期来临之前，刷新数据有效期，确保不丢失
4. 二级缓存
设置不同的失效时间，保障不会被同时淘汰就行
5. 加锁
分布式锁，防止被击穿，但是要注意也是性能瓶颈，慎重！
```
### 7.3 缓存穿透
```markdown
缓存击穿访问了不存在的数据，跳过了合法数据的redis数据缓
存阶段，每次访问数据库，导致对数据库服务器造成压力。通常此类
数据的出现量是一个较低的值，当出现此类情况以毒攻毒，并及
时报警。应对策略应该在临时预案防范方面多做文章。
无论是黑名单还是白名单，都是对整体系统的压力，警报解
除后尽快移除。
```
```markdown
数据库服务器崩溃（3）
1. 系统平稳运行过程中
2. 应用服务器流量随时间增量较大
3. Redis服务器命中率随时间逐步降低
4. Redis内存平稳，内存无压力
5. Redis服务器CPU占用激增
6. 数据库服务器压力激增
7. 数据库崩溃

问题排查
1. Redis中大面积出现未命中
2. 出现非正常URL访问

问题分析
获取的数据在数据库中也不存在，数据库查询未得到对应数据
Redis获取到null数据未进行持久化，直接返回
下次此类数据到达重复上述过程
出现黑客攻击服务器


解决方案（术）
1. 缓存null
对查询结果为null的数据进行缓存（长期使用，定期清理），
设定短时限，例如30-60秒，最高5分钟
2. 白名单策略
提前预热各种分类数据id对应的bitmaps，id作为bitmaps
的offset，相当于设置了数据白名单。当加载正常数据时，放
行，加载异常数据时直接拦截（效率偏低）
使用布隆过滤器（有关布隆过滤器的命中问题对当前状况可以忽略）
3. 实施监控
实时监控redis命中率（业务正常范围时，通常会有一个波动值）
与null数据的占比
非活动时段波动：通常检测3-5倍，超过5倍纳入重点排查对象
活动时段波动：通常检测10-50倍，超过50倍纳入重点排查对象
根据倍数不同，启动不同的排查流程。然后使用黑名单进行防控（运营）
4. key加密
问题出现后，临时启动防灾业务key，对key进行业务层传输加密服务，
设定校验程序，过来的key校验
例如每天随机分配60个加密串，挑选2到3个，混淆到页面数据id中，
发现访问key不满足规则，驳回数据访问
```
## 8 Redis 主从复制

### 8.1 主从复制
```markdown
主从复制架构仅仅用来解决数据的冗余备份,从节点仅仅用来同步数据

无法解决: 1.master节点出现故障的自动故障转移
```
### 8.2 主从复制架构图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022135929465.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

### 8.3 搭建主从复制

```markdown
1.准备3台机器并修改配置redis.conf
master
port 6377
bind 0.0.0.0
如果masterip 为 192.168.42.137,则slaveof masterip masterport 为 slaveof 192.168.42.137 6377
slave1
port 6378
bind 0.0.0.0
slaveof masterip masterport  (高版本 replicaof  masterip masterport )

slave2
port 6379
bind 0.0.0.0
slaveof masterip masterport (高版本 replicaof  masterip masterport )
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022135951237.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
2.启动3台机器进行测试
# 这里我把创建了data文件夹并把redis.conf放进了data文件夹中
./redis-server data/redis6377.conf
./redis-server data/redis6378.conf
./redis-server data/redis6379.conf
```
## 9 Redis哨兵机制

### 9.1 哨兵Sentinel机制
```markdown
Sentinel（哨兵）是Redis 的高可用性解决方案：由一个或
多个Sentinel 实例 组成的Sentinel 系统可以监视任意多个主服
务器，以及这些主服务器属下的所有从服务器，并在被监视的
主服务器进入下线状态时，自动将下线主服务器属下的某个从
服务器升级为新的主服务器。简单的说哨兵就是带有自动故障转
移功能的主从架构。

无法解决: 1.单节点并发压力问题   2.单节点内存和磁盘物理上限
```

### 9.2 哨兵架构原理
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022140020714.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


### 9.3 搭建哨兵架构

```markdown
这里我搭配了1主2从，3个哨兵
我把配置文件放在了conf文件夹下
redis6377.conf  redis6379.conf       sentinel-26378.conf  
redis6378.conf  sentinel-26377.conf  sentinel-26379.conf

下面列出的就是要改的地方
redis6377.conf  (主的配置文件)

bind 0.0.0.0  # 表示任何ip都可以连接，这个要开启来，否则远程连接不起作用
port 6377  # redis-server端口号
dir ./data  # 这个文件要自己创建，./表示命令所在的目录，比如redis-server这个命令所在目录为/usr/local/redis6377/bin。则./data表示/usr/local/redis6377/bin/data

redis6378.conf

bind 0.0.0.0
port 6378
replicaof 192.168.42.137 6377  # 主的IP地址加端口
dir ./data 

redis6379.conf  

bind 0.0.0.0
port 6379
replicaof 192.168.42.137 6377  # 主的IP地址加端口
dir ./data 


sentinel-26377.conf 
port 26377 # 哨兵启动端口
bind: 0.0.0.0 # 开启远程连接
daemonize no  # 是否以后台启动
dir  ./data
sentinel monitor mymaster 192.168.42.137 6377 2  #  mymaster可以随便命令，192.168.42.137 6377 表示主的主机加端口，2表示哨兵选举谁当master时的数量，一般为哨兵数量的一半加1。我这里哨兵为3.所以算出来的值为2,.当有两个及以上的sentinel服务检测到master宕机，才会去执行主从切换的功能。
sentinel down-after-milliseconds mymaster 30000  # 多长时间没响应算master挂了，这里为30s
sentinel parallel-syncs mymaster 1 # 选取新的master时，一次有多少个开始同步数据，值越小，服务器负担越小
sentinel failover-timeout mymaster 180000 # 同步数据的超时时间，这里为18s

sentinel-26378.conf  

port 26378
bind: 0.0.0.0 # 开启远程连接
daemonize no
pidfile /var/run/redis-sentinel.pid
logfile ""
dir ./data
sentinel monitor mymaster 192.168.42.137 6377 2
sentinel down-after-milliseconds mymaster 30000
sentinel parallel-syncs mymaster 1
sentinel failover-timeout mymaster 180000

sentinel-26379.conf
port 26379

bind: 0.0.0.0 # 开启远程连接
daemonize no
pidfile /var/run/redis-sentinel.pid
logfile ""
dir ./data
sentinel monitor mymaster 192.168.42.137 6377 2
sentinel down-after-milliseconds mymaster 30000
sentinel parallel-syncs mymaster 1
sentinel failover-timeout mymaster 180000
```

### 9.4 通过springboot操作哨兵

```properties
# redis sentinel 配置（这个是启动哨兵机制时的配置）
# master书写是使用哨兵监听的那个名称，比如sentinel monitor mymaster 192.168.42.137 6377 2  
spring.redis.sentinel.master=mymaster
# 连接的不再是一个具体redis主机,书写的是多个哨兵节点
spring.redis.sentinel.nodes=192.168.42.137:26377,192.168.42.137:26378,192.168.42.137:26379
```
```markdown
注意:如果连接过程中出现如下错误:RedisConnectionException: 
DENIED Redis is running in protected mode because 
protected mode is enabled, no bind address was 
specified, no authentication password is requested
to clients. In this mode connections are only 
accepted from the loopback interface. If you want to 
connect from external computers to Redis you may 
adopt one of the following solutions: 1) Just 
disable protected mode sending the command 
'CONFIG SET protected-mode no' from the loopback 
interface by connecting to Redis from the same 
host the server is running, however MAKE SURE Redis 
is not publicly accessible from internet if you do so. 
Use CONFIG REWRITE to make this change permanent. 
```

```markdown
解决方案:在哨兵的配置文件中加入bind 0.0.0.0 开启远程连接权限
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022140109716.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 10 Redis集群

### 10.1 集群
```markdown
Redis在3.0后开始支持Cluster(模式)模式,目前redis的集群支持
节点的自动发现,支持slave-master选举和容错,支持在线分
片(sharding shard )等特性。reshard
```
### 10.2 集群架构图
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102214013737.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

### 10.3 集群细节

```markdown
所有的redis节点彼此互联(PING-PONG机制),内部使用二进制协
议优化传输速度和带宽.
节点的fail是通过集群中超过半数的节点检测失效时才生效. 
客户端与redis节点直连,不需要中间proxy层.客户端不需要连接集
群所有节点,连接集群中任何一个可用节点即可
redis-cluster把所有的物理节点映射到[0-16383]slot上,
cluster 负责维护node<->slot<->value
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022140159563.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

### 10.4 集群搭建
```markdown
判断一个是集群中的节点是否可用,是集群中的所用主节点选举
过程,如果半数以上的节点认为当前节点挂掉,那么当前节点就
是挂掉了,所以搭建redis集群时建议节点数最好为奇数，搭建
集群至少需要三个主节点,三个从节点,至少需要6个节点。
```
```markdown
1.安装ruby环境
yum install ruby
yum install rubygems
2.安装redis gem
gem install redis 
遇到问题：
[root@localhost ~]# gem install redis
Fetching: redis-4.1.2.gem (100%)
ERROR:  Error installing redis:
redis requires Ruby version >= 2.3.0.

解决方案：先安装rvm，再把ruby版本提升至2.3.0
1.安装curl
yum install curl
2.安装RVM
curl -sSL https://get.rvm.io | bash -s stable
3.安装一个ruby版本
rvm install 2.6.3
可能会报错：执行以下命令
curl -sSL https://rvm.io/mpapis.asc | gpg2 --import -
curl -sSL https://rvm.io/pkuczynski.asc | gpg2 --import -
补充：
1.查看rvm库中已知的ruby版本
rvm list known
2.使用一个ruby版本
rvm use 2.6.3
3.卸载一个已知版本
rvm remove 2.0.0

```

```markdown
2.在一台机器创建7个目录
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210105163657729.png)



```markdown
3.每个目录复制一份配置文件
[root@localhost ~]# cp redis/redis.conf 7000/
[root@localhost ~]# cp redis/redis.conf 7001/
[root@localhost ~]# cp redis/redis.conf 7002/
[root@localhost ~]# cp redis/redis.conf 7003/
[root@localhost ~]# cp redis/redis.conf 7004/
[root@localhost ~]# cp redis/redis.conf 7005/
[root@localhost ~]# cp redi/redis.conf 7006/
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022140334132.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
4.修改不同目录配置文件
port 	6379 .....                		 //修改端口
bind  0.0.0.0                   		 //开启远程连接
cluster-enabled  yes 	        			 //开启集群模式
cluster-config-file  nodes-port.conf //集群节点配置文件
cluster-node-timeout  5000      	   //集群节点超时时间
appendonly  yes   		               //开启AOF持久化

5.指定不同目录配置文件启动七个节点
[root@localhost bin]# ./redis-server  /root/7000/redis.conf
[root@localhost bin]# ./redis-server  /root/7001/redis.conf
[root@localhost bin]# ./redis-server  /root/7002/redis.conf
[root@localhost bin]# ./redis-server  /root/7003/redis.conf
[root@localhost bin]# ./redis-server  /root/7004/redis.conf
[root@localhost bin]# ./redis-server  /root/7005/redis.conf
[root@localhost bin]# ./redis-server  /root/7006/redis.conf
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/202010221404013.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


```markdown
6.查看进程
[root@localhost bin]# ps aux|grep redis
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022140427872.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

#### 10.4.1 创建集群

```markdown
1.复制集群操作脚本到bin目录中
[root@localhost bin]# cp /root/redis/src/redis-trib.rb .

2.创建集群
./redis-trib.rb create --replicas 1 192.168.202.205:7000 192.168.202.205:7001 192.168.202.205:7002 192.168.202.205:7003 192.168.202.205:7004 192.168.202.205:7005
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022140455281.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
3.集群创建成功出现如下提示
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022140522707.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


#### 10.4.2 查看集群状态
```markdown
1.查看集群状态 check [原始集群中任意节点]

./redis-trib.rb check 192.168.202.205:7000
2.集群节点状态说明
主节点 
	主节点存在hash slots,且主节点的hash slots 没有交叉
	主节点不能删除
	一个主节点可以有多个从节点
	主节点宕机时多个副本之间自动选举主节点

从节点
	从节点没有hash slots
	从节点可以删除
	从节点不负责数据的写,只负责数据的同步
```

#### 10.4.3 添加主节点

```markdown
1.添加主节点 add-node [新加入节点] [原始集群中任意节点]
 ./redis-trib.rb  add-node 192.168.1.158:7006  192.168.1.158:7005
注意:
	1.该节点必须以集群模式启动
	2.默认情况下该节点就是以master节点形式添加
```

#### 10.4.4 添加从节点

```markdown
1.添加从节点 add-node --slave [新加入节点] [集群中任意节点]
./redis-trib.rb  add-node --slave 192.168.1.158:7006 192.168.1.158:7000
注意:
当添加副本节点时没有指定主节点,redis会随机给副本节点较少的主节点
	
2.为确定的master节点添加主节点 add-node --slave --master-id master节点id [新加入节点] [集群任意节点]
./redis-trib.rb  add-node --slave --master-id 3c3a0c74aae0b56170ccb03a76b60cfe7dc1912e 127.0.0.1:7006  127.0.0.1:7000
```

#### 10.4.5 删除副本节点

```markdown
1.删除节点 del-node [集群中任意节点] [删除节点id]
./redis-trib.rb  del-node 127.0.0.1:7002 0ca3f102ecf0c888fc7a7ce43a13e9be9f6d3dd1
注意:
1.被删除的节点必须是从节点或没有被分配hash slots的节点
```

#### 10.4.6 集群在线分片

```markdown
1.在线分片 reshard [集群中任意节点] 
./redis-trib.rb  reshard  192.168.1.158:7000
```
#### 10.4.7 新版本创建集群的相关命令
```markdown
[root@k8smaster bin]# ./redis-cli --cluster help
Cluster Manager Commands:
  create         host1:port1 ... hostN:portN
                 --cluster-replicas <arg>
  check          host:port
                 --cluster-search-multiple-owners
  info           host:port
  fix            host:port
                 --cluster-search-multiple-owners
                 --cluster-fix-with-unreachable-masters
  reshard        host:port
                 --cluster-from <arg>
                 --cluster-to <arg>
                 --cluster-slots <arg>
                 --cluster-yes
                 --cluster-timeout <arg>
                 --cluster-pipeline <arg>
                 --cluster-replace
  rebalance      host:port
                 --cluster-weight <node1=w1...nodeN=wN>
                 --cluster-use-empty-masters
                 --cluster-timeout <arg>
                 --cluster-simulate
                 --cluster-pipeline <arg>
                 --cluster-threshold <arg>
                 --cluster-replace
  add-node       new_host:new_port existing_host:existing_port
                 --cluster-slave
                 --cluster-master-id <arg>
  del-node       host:port node_id
  call           host:port command arg arg .. arg
                 --cluster-only-masters
                 --cluster-only-replicas
  set-timeout    host:port milliseconds
  import         host:port
                 --cluster-from <arg>
                 --cluster-copy
                 --cluster-replace
  backup         host:port backup_directory
  help           

For check, fix, reshard, del-node, set-timeout you can specify the host and port of any working node in the cluster.




```
#### 10.4.8 springboot操作集群
```properties
# redis cluster 操作 书写集群中所有节点
spring.redis.cluster.nodes=192.168.202.206:7000,192.168.202.206:7001,192.168.202.206:7002,192.168.202.206:7003,192.168.202.206:7004,192.168.202.206:7005,192.168.202.206:7006
```
## 11 Redis实现分布式Session管理

### 11.1 管理机制
```markdown
redis的session管理是利用spring提供的session管理解决方案,
将一个应用session交给Redis存储,整个应用中所有session的请
求都会去redis中获取对应的session数据。
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022140556814.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


### 11.2 开发Session管理

#### 11.2.1 引入依赖

```xml
<dependency>
  <groupId>org.springframework.session</groupId>
  <artifactId>spring-session-data-redis</artifactId>
</dependency>
```
```properties
server.port=8989
server.servlet.context-path=/redissession

# redis cluster
spring.redis.cluster.nodes=192.168.202.206:7000,192.168.202.206:7001,192.168.202.206:7002,192.168.202.206:7003,192.168.202.206:7004,192.168.202.206:7005,192.168.202.206:7006

```

####  11.2.2 开发Session管理配置类
```markdown
config
```
```java
package com.cqm.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.session.data.redis.config.annotation.web.http.EnableRedisHttpSession;

@Configuration
@EnableRedisHttpSession  //将整个应用中使用session的数据全部交给redis处理
public class RedisSessionManager {
}

```
```markdown
controller
```
```java
package com.cqm.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

@Controller
@RequestMapping("test")
public class TestController {




    // redis  session   list    1

    // jvm  list 地址    list.add   list.size  2

    //使用redis 的session管理  注意:当session中数据发生变化时必须将session中变化的数据同步到redis中

    @RequestMapping("test")
    public void test(HttpServletRequest request, HttpServletResponse response) throws IOException {


        List<String> list = (List<String>) request.getSession().getAttribute("list");
        if(list==null){
            list = new ArrayList<>();
        }
        list.add("xxxx");
        request.getSession().setAttribute("list",list);//每次session变化都要同步session

        response.getWriter().println("size: "+list.size());
        response.getWriter().println("sessionid: "+request.getSession().getId());

    }


    @RequestMapping("logout")
    public void logout(HttpServletRequest request){
        //退出登录
        request.getSession().invalidate();//失效
    }
}

```
## 12 图片总结
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210103170326415.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/202101031704121.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210103170434259.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210103170636970.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210103170655374.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210103170717807.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210103170747976.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210103170818312.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210103170839851.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210103170906414.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复redis
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
