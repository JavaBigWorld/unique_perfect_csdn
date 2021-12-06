# Sharding-JDBC
![在这里插入图片描述](https://img-blog.csdnimg.cn/e2bfda84d8534ccd836df41de9e34a4e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAdW5pcXVlX3BlcmZlY3Q=,size_18,color_FFFFFF,t_70,g_se,x_16#pic_center)

## 1 概述
### 1.1 分库分表是什么
```markdown
小明是一家初创电商平台的开发人员，他负责卖家模块的功能开发，
其中涉及了店铺、商品的相关业务，设计如下
数据库：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210425165737294.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```sql
通过以下SQL能够获取到商品相关的店铺信息、地理区域信息：
SELECT p.*,r.[地理区域名称],s.[店铺名称],s.[信誉]
FROM [商品信息] p
LEFT JOIN [地理区域] r ON p.[产地] = r.[地理区域编码]
LEFT JOIN [店铺信息] s ON p.id = s.[所属店铺]
WHERE p.id = ?

形成类似以下列表展示：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021042517413673.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
随着公司业务快速发展，数据库中的数据量猛增，访问性能也变慢了，
优化迫在眉睫。分析一下问题出现在哪儿
呢？ 关系型数据库本身比较容易成为系统瓶颈，单机存储容量、
连接数、处理能力都有限。当单表的数据量达到
1000W或100G以后，由于查询维度较多，即使添加从库、优化索引，
做很多操作时性能仍下降严重。
方案1：
通过提升服务器硬件能力来提高数据处理能力，比如增加存储容量 、
CPU等，这种方案成本很高，并且如果瓶颈在
MySQL本身那么提高硬件也是有很的。
方案2：
把数据分散在不同的数据库中，使得单一数据库的数据量变小来缓
解单一数据库的性能问题，从而达到提升数据库
性能的目的，如下图：将电商数据库拆分为若干独立的数据库，
并且对于大表也拆分为若干小表，通过这种数据库
拆分的方法来解决数据库的性能问题。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210425174207841.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
分库分表就是为了解决由于数据量过大而导致数据库性能降低的
问题，将原来独立的数据库拆分成若干数据库组成
，将数据大表拆分成若干数据表组成，使得单一数据库、单一
数据表的数据量变小，从而达到提升数据库性能的目
的。
```
### 1.2 分库分表的方式
```markdown
分库分表包括分库和分表两个部分，在生产中通常包括：垂直分库、
水平分库、垂直分表、水平分表四种方式。
```
#### 1.2.1 垂直分表
```markdown
下边通过一个商品查询的案例讲解垂直分表：
通常在商品列表中是不显示商品详情信息的，如下图：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210425174351479.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
用户在浏览商品列表时，只有对某商品感兴趣时才会查看该商
品的详细描述。因此，商品信息中商品描述字段访问
频次较低，且该字段存储占用空间较大，访问单个数据IO时间较长；
商品信息中商品名称、商品图片、商品价格等
其他字段数据访问频次较高。
由于这两种数据的特性不一样，因此他考虑将商品信息表拆分如下：
将访问频次低的商品描述信息单独存放在一张表中，访问频次较高
的商品基本信息单独放在一张表中。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210425174418267.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```sql
商品列表可采用以下sql：
SELECT p.*,r.[地理区域名称],s.[店铺名称],s.[信誉]
FROM [商品信息] p
LEFT JOIN [地理区域] r ON p.[产地] = r.[地理区域编码]
LEFT JOIN [店铺信息] s ON p.id = s.[所属店铺]
WHERE...ORDER BY...LIMIT...

需要获取商品描述时，再通过以下sql获取：
SELECT *
FROM [商品描述]
WHERE [商品ID] = ?

小明进行的这一步优化，就叫垂直分表。
垂直分表定义：将一个表按照字段分成多表，每个表存储其中一
部分字段。
它带来的提升是：
1.为了避免IO争抢并减少锁表的几率，查看详情的用户与商品信息
浏览互不影响
2.充分发挥热门数据的操作效率，商品信息的操作的高效率不会被商
品描述的低效率所拖累。
一般来说，某业务实体中的各个数据项的访问频次是不一样的，
部分数据项可能是占用存储空间比较大的BLOB或
是TEXT。例如上例中的商品描述。所以，当表数据量很大时，可以将表
按字段切开，将热门字段、冷门字段分开放
置在不同库中，这些库可以放在不同的存储设备上，避免IO争抢。
垂直切分带来的性能提升主要集中在热门数据的
操作效率上，而且磁盘争用情况减少。
通常我们按以下原则进行垂直拆分:
1. 把不常用的字段单独放在一张表;
2. 把text，blob等大字段拆分出来放在附表中;
3. 经常组合查询的列放在一张表中;
```
#### 1.2.2 垂直分库
```markdown
通过垂直分表性能得到了一定程度的提升，但是还没有达到要求，
并且磁盘空间也快不够了，因为数据还是始终限
制在一台服务器，库内垂直分表只解决了单一表数据量过大的问题，
但没有将表分布到不同的服务器上，因此每个
表还是竞争同一个物理机的CPU、内存、网络IO、磁盘。
经过思考，他把原有的SELLER_DB(卖家库)，分为了PRODUCT_DB
(商品库)和STORE_DB(店铺库)，并把这两个库分
散到不同服务器，如下图：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210425174558315.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
由于商品信息与商品描述业务耦合度较高，因此一起被存放在
PRODUCT_DB(商品库)；而店铺信息相对独立，因此
单独被存放在STORE_DB(店铺库)。
小明进行的这一步优化，就叫垂直分库。
垂直分库是指按照业务将表进行分类，分布到不同的数据库上面，
每个库可以放在不同的服务器上，它的核心理念
是专库专用。
它带来的提升是：
解决业务层面的耦合，业务清晰
能对不同业务的数据进行分级管理、维护、监控、扩展等
高并发场景下，垂直分库一定程度的提升IO、数据库连接数、降低单
机硬件资源的瓶颈
垂直分库通过将表按业务分类，然后分布在不同数据库，并且可以将这
些数据库部署在不同服务器上，从而达到多
个服务器共同分摊压力的效果，但是依然没有解决单表数据量
过大的问题。
```
#### 1.2.3 水平分库
```markdown
经过垂直分库后，数据库性能问题得到一定程度的解决，但是随着
业务量的增长，PRODUCT_DB(商品库)单库存储
数据已经超出预估。粗略估计，目前有8w店铺，每个店铺平均150个
不同规格的商品，再算上增长，那商品数量得
往1500w+上预估，并且PRODUCT_DB(商品库)属于访问非常频繁的
资源，单台服务器已经无法支撑。此时该如何
优化？
再次分库？但是从业务角度分析，目前情况已经无法再次垂直分库。
尝试水平分库，将店铺ID为单数的和店铺ID为双数的商品信息分别
放在两个库中。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210425174704674.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
也就是说，要操作某条数据，先分析这条数据所属的店铺ID。如果店
铺ID为双数，将此操作映射至
RRODUCT_DB1(商品库1)；如果店铺ID为单数，将操作映射至
RRODUCT_DB2(商品库2)。此操作要访问数据库名
称的表达式为RRODUCT_DB[店铺ID%2 + 1] 。
小明进行的这一步优化，就叫水平分库。
水平分库是把同一个表的数据按一定规则拆到不同的数据库中，
每个库可以放在不同的服务器上。
它带来的提升是：
解决了单库大数据，高并发的性能瓶颈。
提高了系统的稳定性及可用性。
当一个应用难以再细粒度的垂直切分，或切分后数据量行数巨大，
存在单库读写、存储性能瓶颈，这时候就需要进
行水平分库了，经过水平切分的优化，往往能解决单库存储量及性能
瓶颈。但由于同一个表被分配在不同的数据
库，需要额外进行数据操作的路由工作，因此大大提升了系统复杂度。
```
#### 1.2.4 水平分表
```markdown
按照水平分库的思路对他把PRODUCT_DB_X(商品库)内的表
也可以进行水平拆分，其目的也是为解决单表数据量大
的问题，如下图：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021042517481495.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
与水平分库的思路类似，不过这次操作的目标是表，商品信息及商品描
述被分成了两套表。如果商品ID为双数，将
此操作映射至商品信息1表；如果商品ID为单数，将操作映射至商品
信息2表。此操作要访问表名称的表达式为商品
信息[商品ID%2 + 1] 。
小明进行的这一步优化，就叫水平分表。
水平分表是在同一个数据库内，把同一个表的数据按一定规则拆
到多个表中。
它带来的提升是：
优化单一表数据量过大而产生的性能问题
避免IO争抢并减少锁表的几率
库内的水平分表，解决了单一表数据量过大的问题，分出来的小表
中只包含一部分数据，从而使得单个表的数据量
变小，提高检索性能。
```
#### 1.2.5 小结
```markdown
本章介绍了分库分表的各种方式，它们分别是垂直分表、垂直分库、
水平分库和水平分表：
垂直分表：可以把一个宽表的字段按访问频次、是否是大字段的
原则拆分为多个表，这样既能使业务清晰，还能提
升部分性能。拆分后，尽量从业务角度避免联查，否则性能方面
将得不偿失。
垂直分库：可以把多个表按业务耦合松紧归类，分别存放在不同
的库，这些库可以分布在不同服务器，从而使访问
压力被多服务器负载，大大提升性能，同时能提高整体架构的业务清
晰度，不同的业务库可根据自身情况定制优化
方案。但是它需要解决跨库带来的所有复杂问题。
水平分库：可以把一个表的数据(按数据行)分到多个不同的库，
每个库只有这个表的部分数据，这些库可以分布在
不同服务器，从而使访问压力被多服务器负载，大大提升性能。
它不仅需要解决跨库带来的所有复杂问题，还要解
决数据路由的问题(数据路由问题后边介绍)。
水平分表：可以把一个表的数据(按数据行)分到多个同一个数
据库的多张表中，每个表只有这个表的部分数据，这
样做能小幅提升性能，它仅仅作为水平分库的一个补充优化。
一般来说，在系统设计阶段就应该根据业务耦合松紧来确定垂直
分库，垂直分表方案，在数据量及访问压力不是特
别大的情况，首先考虑缓存、读写分离、索引技术等方案。若数
据量极大，且持续增长，再考虑水平分库水平分表
方案。
```
### 1.3 分库分表带来的问题
```markdown
分库分表能有效的缓解了单机和单库带来的性能瓶颈和压力，
突破网络IO、硬件资源、连接数的瓶颈，同时也带来了一些问题。
```
#### 1.3.1 事务一致性问题
```markdown
由于分库分表把数据分布在不同库甚至不同服务器，不可避
免会带来分布式事务问题。
```
#### 1.3.2 跨节点关联查询
```sql
在没有分库前，我们检索商品时可以通过以下SQL对店铺信息进
行关联查询：
SELECT p.*,r.[地理区域名称],s.[店铺名称],s.[信誉]
FROM [商品信息] p
LEFT JOIN [地理区域] r ON p.[产地] = r.[地理区域编码]
LEFT JOIN [店铺信息] s ON p.id = s.[所属店铺]
WHERE...ORDER BY...LIMIT...

但垂直分库后[商品信息]和[店铺信息]不在一个数据库，甚至不在一
台服务器，无法进行关联查询。
可将原关联查询分为两次查询，第一次查询的结果集中找出关联
数据id，然后根据id发起第二次请求得到关联数
据，最后将获得到的数据进行拼装。
```
#### 1.3.3 跨节点分页、排序函数
```markdown
跨节点多库进行查询时，limit分页、order by排序等问题，就变得
比较复杂了。需要先在不同的分片节点中将数据
进行排序并返回，然后将不同分片返回的结果集进行汇总和再
次排序。
如，进行水平分库后的商品库，按ID倒序排序分页，取第一页：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210425175435601.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021042517545029.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
以上流程是取第一页的数据，性能影响不大，但由于商品信息的分布
在各数据库的数据可能是随机的，如果是取第
N页，需要将所有节点前N页数据都取出来合并，再进行整体的排序，
操作效率可想而知。所以请求页数越大，系
统的性能也会越差。
在使用Max、Min、Sum、Count之类的函数进行计算的时候，与排
序分页同理，也需要先在每个分片上执行相应
的函数，然后将各个分片的结果集进行汇总和再次计算，最终将
结果返回。
```
#### 1.3.4 主键避重
```markdown
在分库分表环境中，由于表中数据同时存在不同数据库中，
主键值平时使用的自增长将无用武之地，某个分区数据
库生成的ID无法保证全局唯一。因此需要单独设计全局主键，
以避免跨库主键重复问题。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210425175534428.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 1.3.5 公共表
```markdown
实际的应用场景中，参数表、数据字典表等都是数据量较小，
变动少，而且属于高频联合查询的依赖表。例子中地
理区域表也属于此类型。
可以将这类表在每个数据库都保存一份，所有对公共表的更
新操作都同时发送到所有分库执行。
由于分库分表之后，数据被分散在不同的数据库、服务器。
因此，对数据的操作也就无法通过常规方式完成，并且
它还带来了一系列的问题。好在，这些问题不是所有都需
要我们在应用层面上解决，市面上有很多中间件可供我们
选择，其中Sharding-JDBC使用流行度较高，我们来了解一下它。
```
### 1.4 Sharding-JDBC介绍
#### 1.4.1 Sharding-JDBC介绍
```markdown
Sharding-JDBC是当当网研发的开源分布式数据库中间件，
从 3.0 开始Sharding-JDBC被包含在 Sharding-Sphere
中，之后该项目进入进入Apache孵化器，4.0版本之后的版
本为Apache版本。
ShardingSphere是一套开源的分布式数据库中间件解决方
案组成的生态圈，它由Sharding-JDBC、ShardingProxy
和Sharding-Sidecar（计划中）这3款相互独立的产品组成。 
他们均提供标准化的数据分片、分布式事务和
数据库治理功能，可适用于如Java同构、异构语言、容器、
云原生等各种多样化的应用场景。
官方地址：https://shardingsphere.apache.org/document/current/cn/overview/
咱们目前只需关注Sharding-JDBC，它定位为轻量级Java框架，
在Java的JDBC层提供的额外服务。 它使用客户端
直连数据库，以jar包形式提供服务，无需额外部署和
依赖，可理解为增强版的JDBC驱动，完全兼容JDBC和各种
ORM框架。
Sharding-JDBC的核心功能为数据分片和读写分离，
通过Sharding-JDBC，应用可以透明的使用jdbc访问已经分库
分表、读写分离的多个数据源，而不用关心数据源的数量
以及数据如何分布。
适用于任何基于Java的ORM框架，
如： Hibernate, Mybatis, Spring JDBC Template或直接使用JDBC。

基于任何第三方的数据库连接池，
如：DBCP, C3P0, BoneCP, Druid, HikariCP等。
支持任意实现JDBC规范的数据库。
目前支持MySQL，Oracle，SQLServer和PostgreSQL。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210425175916134.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
上图展示了Sharding-Jdbc的工作方式，使用Sharding-Jdbc前需
要人工对数据库进行分库分表，在应用程序中加入
Sharding-Jdbc的Jar包，应用程序通过Sharding-Jdbc操作分库
分表后的数据库和数据表，由于Sharding-Jdbc是对
Jdbc驱动的增强，使用Sharding-Jdbc就像使用Jdbc驱动一样，在
应用程序中是无需指定具体要操作的分库和分表的。
```
#### 1.4.2 与jdbc性能对比 
```markdown
1. 性能损耗测试：服务器资源充足、并发数相同，比
较JDBC和Sharding-JDBC性能损耗，Sharding-JDBC
相对JDBC损耗不超过7%。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210425180022499.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
2. 性能对比测试：服务器资源使用到极限，相同的场景JDBC
与Sharding-JDBC的吞吐量相当。
3. 性能对比测试：服务器资源使用到极限，Sharding-JDBC采
用分库分表后，Sharding-JDBC吞吐量较JDBC不分表有接
近2倍的提升。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210425180050970.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
## 2 Sharding-JDBC快速入门
### 2.1 需求说明
```markdown
本章节使用Sharding-JDBC完成对订单表的水平分表，通过快
速入门程序的开发，快速体验Sharding-JDBC的使用
方法。
人工创建两张表，t_order_1和t_order_2，这两张表是订单表
拆分后的表，通过Sharding-Jdbc向订单表插入数据，
按照一定的分片规则，主键为偶数的进入t_order_1，另一部
分数据进入t_order_2，通过Sharding-Jdbc 查询数
据，根据 SQL语句的内容从t_order_1或t_order_2查询数据。
```
### 2.2 环境搭建
#### 2.2.1 环境说明
```markdown
操作系统：Win10
数据库：MySQL-5.7.25
JDK：64位 jdk1.8.0_201
应用框架：spring-boot-2.1.3.RELEASE，Mybatis3.5.0
Sharding-JDBC：sharding-jdbc-spring-boot-starter-4.0.0-RC1
```
#### 2.2.2 创建数据库
```sql
创建订单库order_db
CREATE DATABASE `order_db` CHARACTER SET 'utf8' COLLATE 'utf8_general_ci';

在order_db中创建t_order_1、t_order_2表
DROP TABLE IF EXISTS `t_order_1`;
CREATE TABLE `t_order_1` (
`order_id` bigint(20) NOT NULL COMMENT '订单id',
`price` decimal(10, 2) NOT NULL COMMENT '订单价格',
`user_id` bigint(20) NOT NULL COMMENT '下单用户id',
`status` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '订单状态',
PRIMARY KEY (`order_id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;
DROP TABLE IF EXISTS `t_order_2`;
CREATE TABLE `t_order_2` (
`order_id` bigint(20) NOT NULL COMMENT '订单id',
`price` decimal(10, 2) NOT NULL COMMENT '订单价格',
`user_id` bigint(20) NOT NULL COMMENT '下单用户id',
`status` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '订单状态',
PRIMARY KEY (`order_id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;
```
#### 2.2.3 引入maven依赖
```xml
引入 sharding-jdbc和SpringBoot整合的Jar包：
<dependency>
<groupId>org.apache.shardingsphere</groupId>
<artifactId>sharding‐jdbc‐spring‐boot‐starter</artifactId>
<version>4.0.0‐RC1</version>
</dependency>

具体spring boot相关依赖及配置请参考资料
中dbsharding/sharding-jdbc-simple工程，
本指引只说明与ShardingJDBC相关的内容。
```
### 2.3 编写程序
#### 2.3.1 分片规则配置
```markdown
分片规则配置是sharding-jdbc进行对分库分表操作的重要依据，
配置内容包括：数据源、主键生成策略、分片策略等。
在application.properties中配置
server.port=56081
spring.application.name = sharding‐jdbc‐simple‐demo
server.servlet.context‐path = /sharding‐jdbc‐simple‐demo
spring.http.encoding.enabled = true
spring.http.encoding.charset = UTF‐8
spring.http.encoding.force = true
spring.main.allow‐bean‐definition‐overriding = true
mybatis.configuration.map‐underscore‐to‐camel‐case = true
# 以下是分片规则配置
# 定义数据源
spring.shardingsphere.datasource.names = m1
spring.shardingsphere.datasource.m1.type = com.alibaba.druid.pool.DruidDataSource
spring.shardingsphere.datasource.m1.driver‐class‐name = com.mysql.jdbc.Driver
spring.shardingsphere.datasource.m1.url = jdbc:mysql://localhost:3306/order_db?useUnicode=true
spring.shardingsphere.datasource.m1.username = root
spring.shardingsphere.datasource.m1.password = root
# 指定t_order表的数据分布情况，配置数据节点
spring.shardingsphere.sharding.tables.t_order.actual‐data‐nodes = m1.t_order_$‐>{1..2}
# 指定t_order表的主键生成策略为SNOWFLAKE
spring.shardingsphere.sharding.tables.t_order.key‐generator.column=order_id
spring.shardingsphere.sharding.tables.t_order.key‐generator.type=SNOWFLAKE
# 指定t_order表的分片策略，分片策略包括分片键和分片算法
spring.shardingsphere.sharding.tables.t_order.table‐strategy.inline.sharding‐column = order_id
spring.shardingsphere.sharding.tables.t_order.table‐strategy.inline.algorithm‐expression =
t_order_$‐>{order_id % 2 + 1}
# 打开sql输出日志
spring.shardingsphere.props.sql.show = true
swagger.enable = true
logging.level.root = info
logging.level.org.springframework.web = info
logging.level.com.itheima.dbsharding = debug
logging.level.druid.sql = debug
```
```markdown
1.首先定义数据源m1，并对m1进行实际的参数配置。
2.指定t_order表的数据分布情况，他分布在m1.t_order_1，m1.t_order_2
3.指定t_order表的主键生成策略为SNOWFLAKE，SNOWFLAKE是一
种分布式自增算法，保证id全局唯一
4.定义t_order分片策略，order_id为偶数的数据落在t_order_1，为
奇数的落在t_order_2，分表策略的表达式为
t_order_$->{order_id % 2 + 1}
```
#### 2.3.2 数据操作 
```java
@Mapper
@Component
public interface OrderDao {
    /**
    * 新增订单
    * @param price 订单价格
    * @param userId 用户id
    * @param status 订单状态
    * @return
    */
    @Insert("insert into t_order(price,user_id,status) value(#{price},#{userId},#{status})")
    int insertOrder(@Param("price") BigDecimal price, @Param("userId")Long userId,
    @Param("status")String status);
    /**
    * 根据id列表查询多个订单
    * @param orderIds 订单id列表
    * @return
    */
    @Select({"<script>" +
    "select " +
    " * " +
    " from t_order t" +
    " where t.order_id in " +
    "<foreach collection='orderIds' item='id' open='(' separator=',' close=')'>" +
    " #{id} " +
    "</foreach>"+
    "</script>"})
    List<Map> selectOrderbyIds(@Param("orderIds")List<Long> orderIds);
}
```
#### 2.3.3 测试
```java
编写单元测试：
@RunWith(SpringRunner.class)
@SpringBootTest(classes = {ShardingJdbcSimpleDemoBootstrap.class})
public class OrderDaoTest {
    @Autowired
    private OrderDao orderDao;
    @Test
    public void testInsertOrder(){
        for (int i = 0 ; i<10; i++){
            orderDao.insertOrder(new BigDecimal((i+1)*5),1L,"WAIT_PAY");
        }
    }


    @Test
    public void testSelectOrderbyIds(){
        List<Long> ids = new ArrayList<>();
        ids.add(373771636085620736L);
        ids.add(373771635804602369L);
        List<Map> maps = orderDao.selectOrderbyIds(ids);
        System.out.println(maps);
    }
}

```
```markdown
执行testInsertOrder：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021042603041477.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
通过日志可以发现order_id为奇数的被插入到t_order_2表，
为偶数的被插入到t_order_1表，达到预期目标。
执行testSelectOrderbyIds：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426030449387.png)

```markdown
通过日志可以发现，根据传入order_id的奇偶不同，
sharding-jdbc分别去不同的表检索数据，达到预期目标。
```
### 2.4 流程分析
```markdown
通过日志分析，Sharding-JDBC在拿到用户要执行的sql之后干了哪些事儿：
（1）解析sql，获取片键值，在本例中是order_id
（2）Sharding-JDBC通过规则配置 t_order_$->{order_id % 2 + 1}，
知道了当order_id为偶数时，应该往t_order_1表插数据，
为奇数时，往t_order_2插数据。
（3）于是Sharding-JDBC根据order_id的值改写sql语句，
改写后的SQL语句是真实所要执行的SQL语句。
（4）执行改写后的真实sql语句
（5）将所有真正执行sql的结果进行汇总合并，返回。
```
### 2.5 其他集成方式
```markdown
Sharding-JDBC不仅可以与spring boot良好集成，它还支持
其他配置方式，共支持以下四种集成方式。
```
```yml
Spring Boot Yaml 配置
server:
  port: 56081
  servlet:
    context-path: /sharding-jdbc-simple-demo
spring:
  application:
    name: sharding-jdbc-simple-demo
  http:
    encoding:
      enabled: true
      charset: utf-8
      force: true
  main:
    allow-bean-definition-overriding: true
  shardingsphere:
    datasource:
      names: m1
      m1:
        type: com.alibaba.druid.pool.DruidDataSource
        driverClassName: com.mysql.jdbc.Driver
        url: jdbc:mysql://localhost:3306/order_db?useUnicode=true
        username: root
        password: mysql
    sharding:
      tables:
        t_order:
          actualDataNodes: m1.t_order_$->{1..2}
          tableStrategy:
            inline:
              shardingColumn: order_id
              algorithmExpression: t_order_$->{order_id % 2 + 1}
          keyGenerator:
            type: SNOWFLAKE
            column: order_id
    props:
      sql:
        show: true
mybatis:
  configuration:
    map-underscore-to-camel-case: true
swagger:
  enable: true
logging:
  level:
    root: info
    org.springframework.web: info
    com.itheima.dbsharding: debug
    druid.sql: debug

```
```markdown
如果使用application.yml则需要屏蔽原来的application.properties文件。
```
```java
Java 配置
添加配置类：
package com.itheima.dbsharding.simple.config;

import com.alibaba.druid.pool.DruidDataSource;
import org.apache.shardingsphere.api.config.sharding.KeyGeneratorConfiguration;
import org.apache.shardingsphere.api.config.sharding.ShardingRuleConfiguration;
import org.apache.shardingsphere.api.config.sharding.TableRuleConfiguration;
import org.apache.shardingsphere.api.config.sharding.strategy.InlineShardingStrategyConfiguration;
import org.apache.shardingsphere.shardingjdbc.api.ShardingDataSourceFactory;
import org.springframework.context.annotation.Bean;

import javax.sql.DataSource;
import java.sql.SQLException;
import java.util.HashMap;
import java.util.Map;
import java.util.Properties;

/**
 * @author Administrator
 * @version 1.0
 **/
//@Configuration
public class ShardingJdbcConfig {

    //配置分片规则
    // 定义数据源
    Map<String, DataSource> createDataSourceMap() {
        DruidDataSource dataSource1 = new DruidDataSource();
        dataSource1.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource1.setUrl("jdbc:mysql://localhost:3306/order_db?useUnicode=true");
        dataSource1.setUsername("root");
        dataSource1.setPassword("mysql");
        Map<String, DataSource> result = new HashMap<>();
        result.put("m1", dataSource1);
        return result;
    }
    // 定义主键生成策略
    private static KeyGeneratorConfiguration getKeyGeneratorConfiguration() {
        KeyGeneratorConfiguration result = new KeyGeneratorConfiguration("SNOWFLAKE","order_id");
        return result;
    }

    // 定义t_order表的分片策略
    TableRuleConfiguration getOrderTableRuleConfiguration() {
        TableRuleConfiguration result = new TableRuleConfiguration("t_order","m1.t_order_$->{1..2}");
        result.setTableShardingStrategyConfig(new InlineShardingStrategyConfiguration("order_id", "t_order_$->{order_id % 2 + 1}"));
        result.setKeyGeneratorConfig(getKeyGeneratorConfiguration());

        return result;
    }
    // 定义sharding-Jdbc数据源
    @Bean
    DataSource getShardingDataSource() throws SQLException {
        ShardingRuleConfiguration shardingRuleConfig = new ShardingRuleConfiguration();
        shardingRuleConfig.getTableRuleConfigs().add(getOrderTableRuleConfiguration());
        //spring.shardingsphere.props.sql.show = true
        Properties properties = new Properties();
        properties.put("sql.show","true");
        return ShardingDataSourceFactory.createDataSource(createDataSourceMap(), shardingRuleConfig,properties);
    }

}

```
```markdown
由于采用了配置类所以需要屏蔽原来application.properties文
件中spring.shardingsphere开头的配置信息。
还需要在SpringBoot启动类中屏蔽使用spring.shardingsphere配
置项的类：
@SpringBootApplication(exclude = {SpringBootConfiguration.class})
public class ShardingJdbcSimpleDemoBootstrap {....}
```
```markdown
Spring Boot properties配置
此方式同快速入门程序。
# 定义数据源
spring.shardingsphere.datasource.names = m1
spring.shardingsphere.datasource.m1.type = com.alibaba.druid.pool.DruidDataSource
spring.shardingsphere.datasource.m1.driver‐class‐name = com.mysql.jdbc.Driver
spring.shardingsphere.datasource.m1.url = jdbc:mysql://localhost:3306/order_db?useUnicode=true
spring.shardingsphere.datasource.m1.username = root
spring.shardingsphere.datasource.m1.password = root
# 指定t_order表的主键生成策略为SNOWFLAKE
spring.shardingsphere.sharding.tables.t_order.key‐generator.column=order_id
spring.shardingsphere.sharding.tables.t_order.key‐generator.type=SNOWFLAKE
# 指定t_order表的数据分布情况
spring.shardingsphere.sharding.tables.t_order.actual‐data‐nodes = m1.t_order_$‐>{1..2}
# 指定t_order表的分表策略
spring.shardingsphere.sharding.tables.t_order.table‐strategy.inline.sharding‐column = order_id
spring.shardingsphere.sharding.tables.t_order.table‐strategy.inline.algorithm‐expression =t_order_$‐>{order_id % 2 + 1}
```
```xml
Spring命名空间配置
此方式使用xml方式配置，不推荐使用。
<?xml version="1.0" encoding="UTF‐8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema‐instance"
xmlns:p="http://www.springframework.org/schema/p"
xmlns:context="http://www.springframework.org/schema/context"
xmlns:tx="http://www.springframework.org/schema/tx"
xmlns:sharding="http://shardingsphere.apache.org/schema/shardingsphere/sharding"

xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring‐beans.xsd
http://shardingsphere.apache.org/schema/shardingsphere/sharding
http://shardingsphere.apache.org/schema/shardingsphere/sharding/sharding.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring‐context.xsd
http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring‐tx.xsd">
    <context:annotation‐config />
    <!‐‐定义多个数据源‐‐>
    <bean id="m1" class="com.alibaba.druid.pool.DruidDataSource" destroy‐method="close">
        <property name="driverClassName" value="com.mysql.jdbc.Driver" />
        <property name="url" value="jdbc:mysql://localhost:3306/order_db_1?useUnicode=true" />
        <property name="username" value="root" />
        <property name="password" value="root" />
    </bean>
    <!‐‐定义分库策略‐‐>
    <sharding:inline‐strategy id="tableShardingStrategy" sharding‐column="order_id" algorithm‐
    expression="t_order_$‐>{order_id % 2 + 1}" />
    <!‐‐定义主键生成策略‐‐>
    <sharding:key‐generator id="orderKeyGenerator" type="SNOWFLAKE" column="order_id" />
    <!‐‐定义sharding‐Jdbc数据源‐‐>
    <sharding:data‐source id="shardingDataSource">
        <sharding:sharding‐rule data‐source‐names="m1">
            <sharding:table‐rules>
                <sharding:table‐rule logic‐table="t_order" table‐strategy‐ref="tableShardingStrategy" key‐generator‐ref="orderKeyGenerator" />
            </sharding:table‐rules>
        </sharding:sharding‐rule>
    </sharding:data‐source>
</beans>
```
## 3 Sharding-JDBC执行原理
### 3.1 基本概念
```sql
在了解Sharding-JDBC的执行原理前，需要了解以下概念：
逻辑表
水平拆分的数据表的总称。例：订单数据表根据主键尾数拆分
为10张表，分别是 t_order_0 、 t_order_1 到
t_order_9 ，他们的逻辑表名为 t_order 。
真实表
在分片的数据库中真实存在的物理表。即上个示例中的 t_order_0 到 t_order_9。
数据节点
数据分片的最小物理单元。由数据源名称和数据表组成，例： ds_0.t_order_0 。

绑定表
指分片规则一致的主表和子表。
例如： t_order 表和 t_order_item 表，均按照 order_id 分片,绑定表之间的分区
键完全相同，则此两张表互为绑定表关系。绑定表之间
的多表关联查询不会出现笛卡尔积关联，关联查询效率将大
大提升。举例说明，如果SQL为：
SELECT i.* FROM t_order o JOIN t_order_item i ON o.order_id=i.order_id WHERE o.order_id in (10,
11);


在不配置绑定表关系时，假设分片键 order_id 将数值10路由至第0片，将数值11路由至第1片，那么路由后的SQL
应该为4条，它们呈现为笛卡尔积：
SELECT i.* FROM t_order_0 o JOIN t_order_item_0 i ON o.order_id=i.order_id WHERE o.order_id in
(10, 11);
SELECT i.* FROM t_order_0 o JOIN t_order_item_1 i ON o.order_id=i.order_id WHERE o.order_id in
(10, 11);
SELECT i.* FROM t_order_1 o JOIN t_order_item_0 i ON o.order_id=i.order_id WHERE o.order_id in
(10, 11);
SELECT i.* FROM t_order_1 o JOIN t_order_item_1 i ON o.order_id=i.order_id WHERE o.order_id in
(10, 11);


在配置绑定表关系后，路由的SQL应该为2条：
SELECT i.* FROM t_order_0 o JOIN t_order_item_0 i ON o.order_id=i.order_id WHERE o.order_id in
(10, 11);
SELECT i.* FROM t_order_1 o JOIN t_order_item_1 i ON o.order_id=i.order_id WHERE o.order_id in
(10, 11);

广播表
指所有的分片数据源中都存在的表，表结构和表中的数据在
每个数据库中均完全一致。适用于数据量不大且需要与
海量数据的表进行关联查询的场景，例如：字典表。
分片键
用于分片的数据库字段，是将数据库(表)水平拆分的关键字段。
例：将订单表中的订单主键的尾数取模分片，则订
单主键为分片字段。 SQL中如果无分片字段，将执行全路由，
性能较差。 除了对单分片字段的支持，ShardingJdbc也支持
根据多个字段进行分片。
分片算法
包含分片键和分片算法，由于分片算法的独立性，将其独立抽离。
真正可用于分片操作的是分片键 + 分片算法，也
就是分片策略。内置的分片策略大致可分为尾数取模、哈希、
范围、标签、时间等。由用户方配置的分片策略则更
加灵活，常用的使用行表达式配置分片策略，它采用Groovy表
达式表示，如: t_user_$->{u_id % 8} 表示t_user
表根据u_id模8，而分成8张表，表名称为 t_user_0 到 t_user_7 。
自增主键生成策略
通过在客户端生成自增主键替换以数据库原生自增主键的方式，
做到分布式主键无重复。
```
### 3.2 SQL解析
```markdown
当Sharding-JDBC接受到一条SQL语句时，会陆续执行 
SQL解析 => 查询优化 => SQL路由 => SQL改写 => SQL执行 =>
结果归并 ，最终返回执行结果。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426112242142.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
SQL解析过程分为词法解析和语法解析。 词法解析器用于将SQL拆解
为不可再分的原子符号，称为Token。并根据
不同数据库方言所提供的字典，将其归类为关键字，表达式，字面量和
操作符。 再使用语法解析器将SQL转换为抽
象语法树。
例如，以下SQL：
SELECT id, name FROM t_user WHERE status = 'ACTIVE' AND age > 18
解析之后的为抽象语法树见下图：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021042611243418.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
为了便于理解，抽象语法树中的关键字的Token用绿色表示，变
量的Token用红色表示，灰色表示需要进一步拆
分。
最后，通过对抽象语法树的遍历去提炼分片所需的上下文，并标记
有可能需要SQL改写(后边介绍)的位置。 供分片
使用的解析上下文包含查询选择项（Select Items）、
表信息（Table）、分片条件（Sharding Condition）、自增
主键信息（Auto increment Primary Key）、
排序信息（Order By）、分组信息（Group By）
以及分页信息（Limit、Rownum、Top）。
```
### 3.3 SQL路由
```markdown
SQL路由就是把针对逻辑表的数据操作映射到对数据结点操作的过程。
根据解析上下文匹配数据库和表的分片策略，并生成路由路径。 
对于携带分片键的SQL，根据分片键操作符不同可
以划分为单片路由(分片键的操作符是等号)、多片路由(分片键的
操作符是IN)和范围路由(分片键的操作符是
BETWEEN)，不携带分片键的SQL则采用广播路由。根据分片键进行
路由的场景可分为直接路由、标准路由、笛卡
尔路由等。
```
```sql
标准路由
标准路由是Sharding-Jdbc最为推荐使用的分片方式，它的适用范围
是不包含关联查询或仅包含绑定表之间关联查
询的SQL。 当分片运算符是等于号时，路由结果将落入单库（表），
当分片运算符是BETWEEN或IN时，则路由结
果不一定落入唯一的库（表），因此一条逻辑SQL最终可能被
拆分为多条用于执行的真实SQL。 举例说明，如果按
照 order_id 的奇数和偶数进行数据分片，一个单表查询的SQL如下：
SELECT * FROM t_order WHERE order_id IN (1, 2);
那么路由的结果应为：
SELECT * FROM t_order_0 WHERE order_id IN (1, 2);
SELECT * FROM t_order_1 WHERE order_id IN (1, 2);
绑定表的关联查询与单表查询复杂度和性能相当。举例说明，如果一个包含绑定表的关联查询的SQL如下：
SELECT * FROM t_order o JOIN t_order_item i ON o.order_id=i.order_id WHERE order_id IN (1, 2);

那么路由的结果应为：
SELECT * FROM t_order_0 o JOIN t_order_item_0 i ON o.order_id=i.order_id WHERE order_id IN (1,
2);
SELECT * FROM t_order_1 o JOIN t_order_item_1 i ON o.order_id=i.order_id WHERE order_id IN (1,
2);

可以看到，SQL拆分的数目与单表是一致的。
笛卡尔路由
笛卡尔路由是最复杂的情况，它无法根据绑定表的关系定位
分片规则，因此非绑定表之间的关联查询需要拆解为笛
卡尔积组合执行。 如果上个示例中的SQL并未配置绑定表关系，
那么路由的结果应为：
SELECT * FROM t_order_0 o JOIN t_order_item_0 i ON o.order_id=i.order_id WHERE order_id IN (1,
2);
SELECT * FROM t_order_0 o JOIN t_order_item_1 i ON o.order_id=i.order_id WHERE order_id IN (1,
2);
SELECT * FROM t_order_1 o JOIN t_order_item_0 i ON o.order_id=i.order_id WHERE order_id IN (1,
2);
SELECT * FROM t_order_1 o JOIN t_order_item_1 i ON o.order_id=i.order_id WHERE order_id IN (1,
2);


笛卡尔路由查询性能较低，需谨慎使用。
全库表路由
对于不携带分片键的SQL，则采取广播路由的方式。根据SQL类型
又可以划分为全库表路由、全库路由、全实例路
由、单播路由和阻断路由这5种类型。其中全库表路由用于处理对
数据库中与其逻辑表相关的所有真实表的操作，
主要包括不带分片键的DQL(数据查询)和DML（数据操纵），
以及DDL（数据定义）等。例如：
SELECT * FROM t_order WHERE good_prority IN (1, 10);

则会遍历所有数据库中的所有表，逐一匹配逻辑表和真实表名，能够匹配得上则执行。路由后成为
SELECT * FROM t_order_0 WHERE good_prority IN (1, 10);
SELECT * FROM t_order_1 WHERE good_prority IN (1, 10);
SELECT * FROM t_order_2 WHERE good_prority IN (1, 10);
SELECT * FROM t_order_3 WHERE good_prority IN (1, 10);
```
### 3.4 SQL改写

```markdown
工程师面向逻辑表书写的SQL，并不能够直接在真实的数据库中
执行，SQL改写用于将逻辑SQL改写为在真实数据
库中可以正确执行的SQL。
如一个简单的例子，若逻辑SQL为：
SELECT order_id FROM t_order WHERE order_id=1;
假设该SQL配置分片键order_id，并且order_id=1的情况，将路由至
分片表1。那么改写之后的SQL应该为：
SELECT order_id FROM t_order_1 WHERE order_id=1;

再比如，Sharding-JDBC需要在结果归并时获取相应数据，但该数据
并未能通过查询的SQL返回。 这种情况主要是
针对GROUP BY和ORDER BY。
结果归并时，需要根据 GROUP BY 和 ORDER BY 的字段项进
行分组和排序，但如果原始SQL的选择项中若并未包含分组项或排
序项，则需要对原始SQL进行改写。 先看一下原始SQL中带有结果
归并
所需信息的场景：
SELECT order_id, user_id FROM t_order ORDER BY user_id;

由于使用user_id进行排序，在结果归并中需要能够获取到user_id的
数据，而上面的SQL是能够获取到user_id数据
的，因此无需补列。
如果选择项中不包含结果归并时所需的列，则需要进行补列，如以下SQL：
SELECT order_id FROM t_order ORDER BY user_id;

由于原始SQL中并不包含需要在结果归并中需要获取的user_id，
因此需要对SQL进行补列改写。补列之后的SQL是：
SELECT order_id, user_id AS ORDER_BY_DERIVED_0 FROM t_order ORDER BY user_id;
```
### 3.5 SQL执行
```markdown
Sharding-JDBC采用一套自动化的执行引擎，负责将路由和改写完
成之后的真实SQL安全且高效发送到底层数据源
执行。 它不是简单地将SQL通过JDBC直接发送至数据源执行；也并非
直接将执行请求放入线程池去并发执行。它
更关注平衡数据源连接创建以及内存占用所产生的消耗，以及最大限度
地合理利用并发等问题。 执行引擎的目标是
自动化的平衡资源控制与执行效率，他能在以下两种模式自适应切换：

内存限制模式
使用此模式的前提是，Sharding-JDBC对一次操作所耗费的数
据库连接数量不做限制。 如果实际执行的SQL需要对
某数据库实例中的200张表做操作，则对每张表创建一个新的数据库
连接，并通过多线程的方式并发处理，以达成
执行效率最大化。
连接限制模式
使用此模式的前提是，Sharding-JDBC严格控制对一次操作所耗费
的数据库连接数量。 如果实际执行的SQL需要对
某数据库实例中的200张表做操作，那么只会创建唯一的数据库连接，
并对其200张表串行处理。 如果一次操作中
的分片散落在不同的数据库，仍然采用多线程处理对不同库的
操作，但每个库的每次操作仍然只创建一个唯一的数
据库连接。
内存限制模式适用于OLAP操作，可以通过放宽对数据库连接
的限制提升系统吞吐量； 连接限制模式适用于OLTP操
作，OLTP通常带有分片键，会路由到单一的分片，因此严格控
制数据库连接，以保证在线系统数据库资源能够被更多的应用
所使用，是明智的选择。
```
### 3.6 结果归并
```markdown
将从各个数据节点获取的多数据结果集，组合成为一个结果集并
正确的返回至请求客户端，称为结果归并。
Sharding-JDBC支持的结果归并从功能上可分为遍历、排序、分组、
分页和聚合5种类型，它们是组合而非互斥的关系。归并引擎的
整体结构划分如下图。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021042615215289.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
结果归并从结构划分可分为流式归并、内存归并和装饰者归并。
流式归并和内存归并是互斥的，装饰者归并可以在
流式归并和内存归并之上做进一步的处理。
内存归并很容易理解，他是将所有分片结果集的数据都遍历并存储
在内存中，再通过统一的分组、排序以及聚合等
计算之后，再将其封装成为逐条访问的数据结果集返回。

流式归并是指每一次从数据库结果集中获取到的数据，都能够
通过游标逐条获取的方式返回正确的单条数据，它与
数据库原生的返回结果集的方式最为契合。
下边举例说明排序归并的过程，如下图是一个通过分数进行排序
的示例图，它采用流式归并方式。 图中展示了3张
表返回的数据结果集，每个数据结果集已经根据分数排序完毕，
但是3个数据结果集之间是无序的。 将3个数据结
果集的当前游标指向的数据值进行排序，并放入优先级队列，
t_score_0的第一个数据值最大，t_score_2的第一个
数据值次之，t_score_1的第一个数据值最小，因此优先级队
列根据t_score_0，t_score_2和t_score_1的方式排序队列。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426152248607.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
下图则展现了进行next调用的时候，排序归并是如何进行的。 通过
图中我们可以看到，当进行第一次next调用
时，排在队列首位的t_score_0将会被弹出队列，并且将当前游标指
向的数据值（也就是100）返回至查询客户端，
并且将游标下移一位之后，重新放入优先级队列。 而优先级队列也
会根据t_score_0的当前数据结果集指向游标的
数据值（这里是90）进行排序，根据当前数值，t_score_0排列在队
列的最后一位。 之前队列中排名第二的
t_score_2的数据结果集则自动排在了队列首位。
在进行第二次next时，只需要将目前排列在队列首位的t_score_2弹出
队列，并且将其数据结果集游标指向的值返回至客户端，并下移游标，
继续加入队列排队，以此类推。当一个结果集中已经没有数据了，
则无需再次加入队列
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426152309856.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
可以看到，对于每个数据结果集中的数据有序，而多数据结果集整体
无序的情况下，Sharding-JDBC无需将所有的
数据都加载至内存即可排序。 它使用的是流式归并的方式，
每次next仅获取唯一正确的一条数据，极大的节省了
内存的消耗。
装饰者归并是对所有的结果集归并进行统一的功能增强，比如归并
时需要聚合SUM前，在进行聚合计算前，都会通
过内存归并或流式归并查询出结果集。因此，聚合归并是在之前介
绍的归并类型之上追加的归并能力，即装饰者模式。
```
### 3.7 总结
```markdown
通过以上内容介绍，相信大家已经了解到Sharding-JDBC基础概念、
核心功能以及执行原理。
基础概念：逻辑表，真实表，数据节点，绑定表，广播表，分片键，
分片算法，分片策略，主键生成策略
核心功能：数据分片，读写分离
执行流程： SQL解析 => 查询优化 => SQL路由 => SQL改写 =>
 SQL执行 => 结果归并
接下来我们将通过一个个demo，来演示Sharding-JDBC实际
使用方法。
```
## 4 水平分表
```markdown
前面已经介绍过，水平分表是在同一个数据库内，把同一个表的
数据按一定规则拆到多个表中。在快速入门里，我们已经对水平
分库进行实现，这里不再重复介绍。
```
## 5 水平分库
```markdown
前面已经介绍过，水平分库是把同一个表的数据按一定规则拆到不
同的数据库中，每个库可以放在不同的服务器上。接下来看一下
如何使用Sharding-JDBC实现水平分库，咱们继续对快速入门中的
例子进行完善。
```

```markdown
(1)将原有order_db库拆分为order_db_1、order_db_2
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426152702376.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
(2)分片规则修改
由于数据库拆分了两个，这里需要配置两个数据源。
分库需要配置分库的策略，和分表策略的意义类似，通过分库策略实
现数据操作针对分库的数据库进行操作。

# 定义多个数据源
spring.shardingsphere.datasource.names = m1,m2
spring.shardingsphere.datasource.m1.type = com.alibaba.druid.pool.DruidDataSource
spring.shardingsphere.datasource.m1.driver‐class‐name = com.mysql.jdbc.Driver
spring.shardingsphere.datasource.m1.url = jdbc:mysql://localhost:3306/order_db_1?useUnicode=true
spring.shardingsphere.datasource.m1.username = root
spring.shardingsphere.datasource.m1.password = root
spring.shardingsphere.datasource.m2.type = com.alibaba.druid.pool.DruidDataSource
spring.shardingsphere.datasource.m2.driver‐class‐name = com.mysql.jdbc.Driver
spring.shardingsphere.datasource.m2.url = jdbc:mysql://localhost:3306/order_db_2?useUnicode=true
spring.shardingsphere.datasource.m2.username = root
spring.shardingsphere.datasource.m2.password = root
...
# 分库策略，以user_id为分片键，分片策略为user_id % 2 + 1，user_id为偶数操作m1数据源，否则操作m2。
spring.shardingsphere.sharding.tables.t_order.database‐strategy.inline.sharding‐column = user_id
spring.shardingsphere.sharding.tables.t_order.database‐strategy.inline.algorithm‐expression =
m$‐>{user_id % 2 + 1}
```
```markdown
分库策略定义方式如下：
#分库策略，如何将一个逻辑表映射到多个数据源
spring.shardingsphere.sharding.tables.<逻辑表名称>.database‐strategy.<分片策略>.<分片策略属性名>= #
分片策略属性值
#分表策略，如何将一个逻辑表映射为多个实际表
spring.shardingsphere.sharding.tables.<逻辑表名称>.table‐strategy.<分片策略>.<分片策略属性名>= #分
片策略属性值
```

```markdown
Sharding-JDBC支持以下几种分片策略：
不管理分库还是分表，策略基本一样。
standard：标准分片策略，对应StandardShardingStrategy。提供对SQL语句中的=, IN和BETWEEN AND的
分片操作支持。StandardShardingStrategy只支持单分片键，提供PreciseShardingAlgorithm和
RangeShardingAlgorithm两个分片算法。PreciseShardingAlgorithm是必选的，用于处理=和IN的分片。
RangeShardingAlgorithm是可选的，用于处理BETWEEN AND分片，如果不配置
RangeShardingAlgorithm，SQL中的BETWEEN AND将按照全库路由处理。
complex：符合分片策略，对应ComplexShardingStrategy。复合分片策略。提供对SQL语句中的=, IN和
BETWEEN AND的分片操作支持。ComplexShardingStrategy支持多分片键，由于多分片键之间的关系复
杂，因此并未进行过多的封装，而是直接将分片键值组合以及分片操作符透传至分片算法，完全由应用开发
者实现，提供最大的灵活度。
inline：行表达式分片策略，对应InlineShardingStrategy。使用Groovy的表达式，提供对SQL语句中的=和
IN的分片操作支持，只支持单分片键。对于简单的分片算法，可以通过简单的配置使用，从而避免繁琐的Java
代码开发，如: t_user_$->{u_id % 8} 表示t_user表根据u_id模8，而分成8张表，表名称为 t_user_0 到
t_user_7 。
hint：Hint分片策略，对应HintShardingStrategy。通过Hint而非SQL解析的方式分片的策略。对于分片字段
非SQL决定，而由其他外置条件决定的场景，可使用SQL Hint灵活的注入分片字段。例：内部系统，按照员工
登录主键分库，而数据库中并无此字段。SQL Hint支持通过Java API和SQL注释(待实现)两种方式使用。
none：不分片策略，对应NoneShardingStrategy。不分片的策略。
目前例子中都使用inline分片策略，若对其他分片策略细节若感兴趣，请查阅官方文档：
https://shardingsphere.apache.org
```
```java
(3)插入测试
修改testInsertOrder方法，插入数据中包含不同的user_id
@Test
public void testInsertOrder(){
	for (int i = 0 ; i<10; i++){
		orderDao.insertOrder(new BigDecimal((i+1)*5),1L,"WAIT_PAY");
	}
	for (int i = 0 ; i<10; i++){
		orderDao.insertOrder(new BigDecimal((i+1)*10),2L,"WAIT_PAY");
	}
}
```
```markdown
执行testInsertOrder:
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021042615320249.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
通过日志可以看出，根据user_id的奇偶不同，数据分别落在了
不同数据源，达到目标。
```

```java
（4）查询测试
调用快速入门的查询接口进行测试：
List<Map> selectOrderbyIds(@Param("orderIds")List<Long> orderIds);
通过日志发现，sharding-jdbc将sql路由到m1和m2：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426153308100.png)

```java
问题分析：
由于查询语句中并没有使用分片键user_id，所以sharding-jdbc将广播路由到每个数据结点。
下边我们在sql中添加分片键进行查询。
在OrderDao中定义接口：
@Select({"<script>",
" select",
" * ",
" from t_order t ",
"where t.order_id in",
"<foreach collection='orderIds' item='id' open='(' separator=',' close=')'>",
"#{id}",
"</foreach>",
" and t.user_id = #{userId} ",
"</script>"
})
List<Map> selectOrderbyUserAndIds(@Param("userId") Integer userId,@Param("orderIds")List<Long>
orderIds);
```
```java
编写测试方法：
@Test
public void testSelectOrderbyUserAndIds(){
	List<Long> orderIds = new ArrayList<>();
	orderIds.add(373422416644276224L);
	orderIds.add(373422415830581248L);
	//查询条件中包括分库的键user_id
	int user_id = 1;
	List<Map> orders = orderDao.selectOrderbyUserAndIds(user_id,orderIds);
	JSONArray jsonOrders = new JSONArray(orders);
	System.out.println(jsonOrders);
}
```
```markdown
执行testSelectOrderbyUserAndIds:
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426153440568.png)

```markdown
查询条件user_id为1，根据分片策略m$->{user_id % 2 + 1}
计算得出m2，此sharding-jdbc将sql路由到m2，见上图日志。
```
## 6 垂直分库
```sql
前面已经介绍过，垂直分库是指按照业务将表进行分类，分布
到不同的数据库上面，每个库可以放在不同的服务器
上，它的核心理念是专库专用。接下来看一下如何使用
Sharding-JDBC实现垂直分库。
(1)创建数据库
创建数据库user_db
CREATE DATABASE `user_db` CHARACTER SET 'utf8' COLLATE 'utf8_general_ci';
在user_db中创建t_user表
DROP TABLE IF EXISTS `t_user`;
CREATE TABLE `t_user` (
`user_id` bigint(20) NOT NULL COMMENT '用户id',
`fullname` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '用户姓名',
`user_type` char(1) DEFAULT NULL COMMENT '用户类型',
PRIMARY KEY (`user_id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

2)在Sharding-JDBC规则中修改
# 新增m0数据源，对应user_db
spring.shardingsphere.datasource.names = m0,m1,m2
...
spring.shardingsphere.datasource.m0.type = com.alibaba.druid.pool.DruidDataSource
spring.shardingsphere.datasource.m0.driver‐class‐name = com.mysql.jdbc.Driver
spring.shardingsphere.datasource.m0.url = jdbc:mysql://localhost:3306/user_db?useUnicode=true
spring.shardingsphere.datasource.m0.username = root
spring.shardingsphere.datasource.m0.password = root
....
# t_user分表策略，固定分配至m0的t_user真实表
spring.shardingsphere.sharding.tables.t_user.actual‐data‐nodes = m$‐>{0}.t_user
spring.shardingsphere.sharding.tables.t_user.table‐strategy.inline.sharding‐column = user_id
spring.shardingsphere.sharding.tables.t_user.table‐strategy.inline.algorithm‐expression = t_user

(3)数据操作
新增UserDao:
@Mapper
@Component
public interface UserDao {
/**
* 新增用户
* @param userId 用户id
* @param fullname 用户姓名
* @return
*/
@Insert("insert into t_user(user_id, fullname) value(#{userId},#{fullname})")
int insertUser(@Param("userId")Long userId,@Param("fullname")String fullname);
/**
* 根据id列表查询多个用户
* @param userIds 用户id列表
* @return
*/
@Select({"<script>",
" select",
" * ",
" from t_user t ",
" where t.user_id in",
"<foreach collection='userIds' item='id' open='(' separator=',' close=')'>",
"#{id}",
"</foreach>",
"</script>"
})
List<Map> selectUserbyIds(@Param("userIds")List<Long> userIds);
}
```
```java
(4)测试
新增单元测试方法：
@Test
public void testInsertUser(){
	for (int i = 0 ; i<10; i++){
	Long id = i + 1L;
	userDao.insertUser(id,"姓名"+ id );
	}
}
@Test
public void testSelectUserbyIds(){
	List<Long> userIds = new ArrayList<>();
	userIds.add(1L);
	userIds.add(2L);
	List<Map> users = userDao.selectUserbyIds(userIds);
	System.out.println(users);
}


```
```markdown
执行testInsertUser:
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426154053592.png)

```markdown
通过日志可以看出t_user表的数据被落在了m0数据源，达到目标。
执行testSelectUserbyIds:
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426154114655.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
通过日志可以看出t_user表的查询操作被落在了m0数据源，达到目标。
```
## 7 公共表
```sql
公共表属于系统中数据量较小，变动少，而且属于高频联合查询的依
赖表。参数表、数据字典表等属于此类型。可
以将这类表在每个数据库都保存一份，所有更新操作都同时发
送到所有分库执行。接下来看一下如何使用
Sharding-JDBC实现公共表。
(1)创建数据库
分别在user_db、order_db_1、order_db_2中创建t_dict表：
CREATE TABLE `t_dict` (
`dict_id` bigint(20) NOT NULL COMMENT '字典id',
`type` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '字典类型',
`code` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '字典编码',
`value` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '字典值',
PRIMARY KEY (`dict_id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

(2)在Sharding-JDBC规则中修改
# 指定t_dict为公共表
spring.shardingsphere.sharding.broadcast‐tables=t_dict

(3)数据操作
新增DictDao：
@Mapper
@Component
public interface DictDao {
/**
* 新增字典
* @param type 字典类型
* @param code 字典编码
* @param value 字典值
* @return
*/
@Insert("insert into t_dict(dict_id,type,code,value) value(#{dictId},#{type},#{code},#
{value})")
int insertDict(@Param("dictId") Long dictId,@Param("type") String type, @Param("code")String
code, @Param("value")String value);
/**
* 删除字典
* @param dictId 字典id
* @return
*/
@Delete("delete from t_dict where dict_id = #{dictId}")
int deleteDict(@Param("dictId") Long dictId);
}


(4)字典操作测试
新增单元测试方法：
@Test
public void testInsertDict(){
	dictDao.insertDict(1L,"user_type","0","管理员");
	dictDao.insertDict(2L,"user_type","1","操作员");
}
@Test
public void testDeleteDict(){
	dictDao.deleteDict(1L);
	dictDao.deleteDict(2L);
}
```

```markdown
执行testInsertDict：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426154429456.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```java
通过日志可以看出，对t_dict的表的操作被广播至所有数据源。
测试删除字典，观察是否把所有数据源中该 公共表的记录删除。
（5）字典关联查询测试
字典表已在各各分库存在，各业务表即可和字典表关联查询。
定义用户关联查询dao：
在UserDao中定义：
/**
* 根据id列表查询多个用户，关联查询字典表
* @param userIds 用户id列表
* @return
*/
@Select({"<script>",
" select",
" * ",
" from t_user t ,t_dict b",
" where t.user_type = b.code and t.user_id in",
"<foreach collection='userIds' item='id' open='(' separator=',' close=')'>",
"#{id}",
"</foreach>",
"</script>"
})
List<Map> selectUserInfobyIds(@Param("userIds")List<Long> userIds);
```
```java
定义测试方法：
@Test
public void testSelectUserInfobyIds(){
	List<Long> userIds = new ArrayList<>();
	userIds.add(1L);
	userIds.add(2L);
	List<Map> users = userDao.selectUserInfobyIds(userIds);
	JSONArray jsonUsers = new JSONArray(users);
	System.out.println(jsonUsers);
}
```
```markdown
执行测试方法，查看日志，成功关联查询字典表：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426154641603.png)
## 8 读写分离
### 8.1 理解读写分离
```markdown
面对日益增加的系统访问量，数据库的吞吐量面临着巨大瓶颈。 对
于同一时刻有大量并发读操作和较少写操作类
型的应用系统来说，将数据库拆分为主库和从库，主库负责处
理事务性的增删改操作，从库负责处理查询操作，能
够有效的避免由数据更新导致的行锁，使得整个系统的查询性
能得到极大的改善。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426154754729.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
通过一主多从的配置方式，可以将查询请求均匀的分散到多个数据
副本，能够进一步的提升系统的处理能力。 使用
多主多从的方式，不但能够提升系统的吞吐量，还能够提升系统的
可用性，可以达到在任何一个数据库宕机，甚至
磁盘物理损坏的情况下仍然不影响系统的正常运行。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426154813864.png)

```markdown
读写分离的数据节点中的数据内容是一致的，而水平分片的每个数
据节点的数据内容却并不相同。将水平分片和读
写分离联合使用，能够更加有效的提升系统的性能。
Sharding-JDBC读写分离则是根据SQL语义的分析，将读操作和
写操作分别路由至主库与从库。它提供透明化读写
分离，让使用方尽量像使用一个数据库一样使用主从数据库集群。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426154845869.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
Sharding-JDBC提供一主多从的读写分离配置，可独立使用，也
可配合分库分表使用，同一线程且同一数据库连接
内，如有写入操作，以后的读操作均从主库读取，用于保证数据一致性。
Sharding-JDBC不提供主从数据库的数据同步功能，需要采用其他机
制支持。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426154915471.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
接下来，咱们对上面例子中user_db进行读写分离实现。为了实
现Sharding-JDBC的读写分离，首先，要进行mysql的主从同步配置。
```
### 8.2 mysql主从同步(windows)
```markdown
一，新增mysql实例
复制原有mysql如：D:\mysql-5.7.25(作为主库) -> D:\mysql-5.7.25-s1(作为从库)，并修改以下从库的my.ini：
[mysqld]
#设置3307端口
port = 3307
# 设置mysql的安装目录
basedir=D:\mysql‐5.7.25‐s1
# 设置mysql数据库的数据的存放目录
datadir=D:\mysql‐5.7.25‐s1\data
然后将从库安装为windows服务，注意配置文件位置：
D:\mysql‐5.7.25‐s1\bin>mysqld install mysqls1 ‐‐defaults‐file="D:\mysql‐5.7.25‐s1\my.ini"
由于从库是从主库复制过来的，因此里面的数据完全一致，可使用原来的账号、密码登录。

二，修改主、从库的配置文件(my.ini)，新增内容如下：
主库：
[mysqld]
#开启日志
log‐bin = mysql‐bin
#设置服务id，主从不能一致
server‐id = 1
#设置需要同步的数据库
binlog‐do‐db=user_db
#屏蔽系统库同步
binlog‐ignore‐db=mysql
binlog‐ignore‐db=information_schema
binlog‐ignore‐db=performance_schema

从库：
[mysqld]
#开启日志
log‐bin = mysql‐bin
#设置服务id，主从不能一致
server‐id = 2
#设置需要同步的数据库
replicate_wild_do_table=user_db.%
#屏蔽系统库同步
replicate_wild_ignore_table=mysql.%
replicate_wild_ignore_table=information_schema.%
replicate_wild_ignore_table=performance_schema.%

重启主库和从库：
net start [主库服务名]
net start [从库服务名mysqls1]
请注意，主从MySQL下的数据(data)目录下有个文件auto.cnf，文件中定义了uuid，要保证主从数据库实例的
uuid不一样，建议直接删除掉，重启服务后将会重新生成。

三 授权主从复制专用账号
#切换至主库bin目录，登录主库
mysql ‐h localhost ‐uroot ‐p
#授权主备复制专用账号
GRANT REPLICATION SLAVE ON *.* TO 'db_sync'@'%' IDENTIFIED BY 'db_sync';
#刷新权限
FLUSH PRIVILEGES;
#确认位点 记录下文件名以及位点
show master status;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426155237315.png)

```markdown
四 设置从库向主库同步数据、并检查链路
#切换至从库bin目录，登录从库
mysql ‐h localhost ‐P3307 ‐uroot ‐p
#先停止同步
STOP SLAVE;
#修改从库指向到主库，使用上一步记录的文件名以及位点
CHANGE MASTER TO
master_host = 'localhost',
master_user = 'db_sync',
master_password = 'db_sync',
master_log_file = 'mysql‐bin.000002',
master_log_pos = 154;
#启动同步
START SLAVE;
#查看从库状态Slave_IO_Runing和Slave_SQL_Runing都为Yes说明同步成功，如果不为Yes，请检查error_log，然后
排查相关异常。
show slave status\G
#注意 如果之前此备库已有主库指向 需要先执行以下命令清空
STOP SLAVE IO_THREAD FOR CHANNEL '';
reset slave all;
```

```markdown
最后测试在主库修改数据库，看从库是否能够同步成功。
```
### 8.3 实现sharding-jdbc读写分离
```markdown
(1)在Sharding-JDBC规则中修改
# 增加数据源s0，使用上面主从同步配置的从库。
spring.shardingsphere.datasource.names = m0,m1,m2,s0
...
spring.shardingsphere.datasource.s0.type = com.alibaba.druid.pool.DruidDataSource
spring.shardingsphere.datasource.s0.driver‐class‐name = com.mysql.jdbc.Driver
spring.shardingsphere.datasource.s0.url = jdbc:mysql://localhost:3307/user_db?useUnicode=true
spring.shardingsphere.datasource.s0.username = root
spring.shardingsphere.datasource.s0.password = root
....
# 主库从库逻辑数据源定义 ds0为user_db
spring.shardingsphere.sharding.master‐slave‐rules.ds0.master‐data‐source‐name=m0
spring.shardingsphere.sharding.master‐slave‐rules.ds0.slave‐data‐source‐names=s0
# t_user分表策略，固定分配至ds0的t_user真实表
spring.shardingsphere.sharding.tables.t_user.actual‐data‐nodes = ds0.t_user
....

(2)测试
执行testInsertUser单元测试：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021042615544847.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
通过日志可以看出，所有写操作落入m0数据源。
执行testSelectUserbyIds单元测试：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426155509375.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
通过日志可以看出，所有写操作落入s0数据源，达到目标。
```
## 9 案例
### 9.1 需求描述
```markdown
电商平台商品列表展示，每个列表项中除了包含商品基本信息、
商品描述信息之外，还包括了商品所属的店铺信息，如下：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426155610658.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/202104261557267.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
本案例实现功能如下：
1、添加商品
2、商品分页查询
4、商品统计
```
### 9.2 数据库设计
```markdown
数据库设计如下，其中商品与店铺信息之间进行了垂直分库，
分为了PRODUCT_DB(商品库)和STORE_DB(店铺
库)；商品信息还进行了垂直分表，分为了商品基
本信息(product_info)和商品描述信息(product_descript)，
地理区域信息(region)作为公共表，冗余在两库中：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426155829153.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
考虑到商品信息的数据增长性，对PRODUCT_DB(商品库)进行了水平
分库，分片键使用店铺id，分片策略为店铺
ID%2 + 1，因此商品描述信息对所属店铺ID进行了冗余；
对商品基本信息(product_info)和商品描述信
息(product_descript)进行水平分表，分片键使用商品id，分片策
略为商品ID%2 + 1,并将为这两个表设置为绑定表，避免笛卡
尔积join；为避免主键冲突，ID生成策略采用雪花算法来生成全
局唯一ID，最终数据库设计为下图：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426155914161.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
要求使用读写分离来提升性能，可用性。
```
### 9.3 环境说明
```markdown
操作系统：Win10
数据库：MySQL-5.7.25
JDK：64位 jdk1.8.0_201
应用框架：spring-boot-2.1.3.RELEASE，Mybatis3.5.0
Sharding-JDBC：sharding-jdbc-spring-boot-starter-4.0.0-RC1
```
### 9.4 环境准备
#### 9.4.1 mysql主从同步(windows)
```markdown
参考读写分离章节，对以下库进行主从同步配置：
# 设置需要同步的数据库
binlog‐do‐db=store_db
binlog‐do‐db=product_db_1
binlog‐do‐db=product_db_2
```
#### 9.4.2 初始化数据库 
```sql
创建store_db数据库，并执行以下脚本创建表：
DROP TABLE IF EXISTS `region`;
CREATE TABLE `region` (
`id` bigint(20) NOT NULL COMMENT 'id',
`region_code` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT
'地理区域编码',
`region_name` varchar(100) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL
COMMENT '地理区域名称',
`level` tinyint(1) NULL DEFAULT NULL COMMENT '地理区域级别(省、市、县)',
`parent_region_code` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL
COMMENT '上级地理区域编码',
PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;
INSERT INTO `region` VALUES (1, '110000', '北京', 0, NULL);
INSERT INTO `region` VALUES (2, '410000', '河南省', 0, NULL);
INSERT INTO `region` VALUES (3, '110100', '北京市', 1, '110000');
INSERT INTO `region` VALUES (4, '410100', '郑州市', 1, '410000');
DROP TABLE IF EXISTS `store_info`;
CREATE TABLE `store_info` (
`id` bigint(20) NOT NULL COMMENT 'id',
`store_name` varchar(100) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT
'店铺名称',
`reputation` int(11) NULL DEFAULT NULL COMMENT '信誉等级',
`region_code` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT
'店铺所在地',
PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;
INSERT INTO `store_info` VALUES (1, 'XX零食店', 4, '110100');
INSERT INTO `store_info` VALUES (2, 'XX饮品店', 3, '410100');


创建product_db_1、product_db_2数据库，并分别对两库执行以下脚本创建表：
DROP TABLE IF EXISTS `product_descript_1`;
CREATE TABLE `product_descript_1` (
`id` bigint(20) NOT NULL COMMENT 'id',
`product_info_id` bigint(20) NULL DEFAULT NULL COMMENT '所属商品id',
`descript` longtext CHARACTER SET utf8 COLLATE utf8_general_ci NULL COMMENT '商品描述',
`store_info_id` bigint(20) NULL DEFAULT NULL COMMENT '所属店铺id',
PRIMARY KEY (`id`) USING BTREE,
INDEX `FK_Reference_2`(`product_info_id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;
DROP TABLE IF EXISTS `product_descript_2`;
CREATE TABLE `product_descript_2` (
`id` bigint(20) NOT NULL COMMENT 'id',
`product_info_id` bigint(20) NULL DEFAULT NULL COMMENT '所属商品id',
`descript` longtext CHARACTER SET utf8 COLLATE utf8_general_ci NULL COMMENT '商品描述',
`store_info_id` bigint(20) NULL DEFAULT NULL COMMENT '所属店铺id',
PRIMARY KEY (`id`) USING BTREE,
INDEX `FK_Reference_2`(`product_info_id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;
DROP TABLE IF EXISTS `product_info_1`;
CREATE TABLE `product_info_1` (
`product_info_id` bigint(20) NOT NULL COMMENT 'id',
`store_info_id` bigint(20) NULL DEFAULT NULL COMMENT '所属店铺id',
`product_name` varchar(100) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL
COMMENT '商品名称',
`spec` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '规
格',
`region_code` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT
'产地',
`price` decimal(10, 0) NULL DEFAULT NULL COMMENT '商品价格',
`image_url` varchar(100) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT
'商品图片',
PRIMARY KEY (`product_info_id`) USING BTREE,
INDEX `FK_Reference_1`(`store_info_id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;
DROP TABLE IF EXISTS `product_info_2`;
CREATE TABLE `product_info_2` (
`product_info_id` bigint(20) NOT NULL COMMENT 'id',
`store_info_id` bigint(20) NULL DEFAULT NULL COMMENT '所属店铺id',
`product_name` varchar(100) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL
COMMENT '商品名称',
`spec` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '规
格',
`region_code` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT
'产地',
`price` decimal(10, 0) NULL DEFAULT NULL COMMENT '商品价格',
`image_url` varchar(100) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT
'商品图片',
PRIMARY KEY (`product_info_id`) USING BTREE,
INDEX `FK_Reference_1`(`store_info_id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;
DROP TABLE IF EXISTS `region`;
CREATE TABLE `region` (
`id` bigint(20) NOT NULL COMMENT 'id',
`region_code` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT
'地理区域编码',
`region_name` varchar(100) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL
COMMENT '地理区域名称',
`level` tinyint(1) NULL DEFAULT NULL COMMENT '地理区域级别(省、市、县)',
`parent_region_code` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL
COMMENT '上级地理区域编码',
PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;
INSERT INTO `region` VALUES (1, '110000', '北京', 0, NULL);
INSERT INTO `region` VALUES (2, '410000', '河南省', 0, NULL);
INSERT INTO `region` VALUES (3, '110100', '北京市', 1, '110000');
INSERT INTO `region` VALUES (4, '410100', '郑州市', 1, '410000');
```
### 9.5 实现步骤
#### 9.5.1 搭建maven工程
```xml
（1）搭建工程maven工程shopping，导入资料中基础代码shopping，以dbsharding为总体父工程，并做好
spring boot相关配置。
（2）引入maven依赖
<dependency>
	<groupId>org.apache.shardingsphere</groupId>
	<artifactId>sharding‐jdbc‐spring‐boot‐starter</artifactId>
	<version>4.0.0‐RC1</version>
</dependency>
```
#### 9.5.2 分片配置
```markdown
既然是分库分表，那么就需要定义多个真实数据源，每一个数据库链接信息就是一个数据源定义，如：
spring.shardingsphere.datasource.m0.type = com.alibaba.druid.pool.DruidDataSource
spring.shardingsphere.datasource.m0.driver‐class‐name = com.mysql.jdbc.Driver
spring.shardingsphere.datasource.m0.url = jdbc:mysql://localhost:3306/store_db?useUnicode=true
spring.shardingsphere.datasource.m0.username = root
spring.shardingsphere.datasource.m0.password = root

m0，就是这个真实数据源的名称，然后需要告诉Sharding-JDBC，咱们有哪些真实数据源，如：
spring.shardingsphere.datasource.names = m0,m1,m2,s0,s1,s2

如果需要配置读写分离，还需要告诉Sharding-JDBC，这么多真实数据源，那几个是一套读写分离？也就是定义主从逻辑数据源：

spring.shardingsphere.sharding.master‐slave‐rules.ds0.master‐data‐source‐name=m0
spring.shardingsphere.sharding.master‐slave‐rules.ds0.slave‐data‐source‐names=s0

spring.shardingsphere.sharding.master‐slave‐rules.ds0.master‐data‐source‐name=m0
spring.shardingsphere.sharding.master‐slave‐rules.ds0.slave‐data‐source‐names=s0

# 真实数据源定义 m为主库 s为从库
spring.shardingsphere.datasource.names = m0,m1,m2,s0,s1,s2
spring.shardingsphere.datasource.m0.type = com.alibaba.druid.pool.DruidDataSource
spring.shardingsphere.datasource.m0.driver‐class‐name = com.mysql.jdbc.Driver
spring.shardingsphere.datasource.m0.url = jdbc:mysql://localhost:3306/store_db?useUnicode=true
spring.shardingsphere.datasource.m0.username = root
spring.shardingsphere.datasource.m0.password = root
spring.shardingsphere.datasource.m1.type = com.alibaba.druid.pool.DruidDataSource
spring.shardingsphere.datasource.m1.driver‐class‐name = com.mysql.jdbc.Driver
spring.shardingsphere.datasource.m1.url = jdbc:mysql://localhost:3306/product_db_1?
useUnicode=true
spring.shardingsphere.datasource.m1.username = root
spring.shardingsphere.datasource.m1.password = root
spring.shardingsphere.datasource.m2.type = com.alibaba.druid.pool.DruidDataSource
spring.shardingsphere.datasource.m2.driver‐class‐name = com.mysql.jdbc.Driver
spring.shardingsphere.datasource.m2.url = jdbc:mysql://localhost:3306/product_db_2?
useUnicode=true
spring.shardingsphere.datasource.m2.username = root
spring.shardingsphere.datasource.m2.password = root
spring.shardingsphere.datasource.s0.type = com.alibaba.druid.pool.DruidDataSource
spring.shardingsphere.datasource.s0.driver‐class‐name = com.mysql.jdbc.Driver
spring.shardingsphere.datasource.s0.url = jdbc:mysql://localhost:3307/store_db?useUnicode=true
spring.shardingsphere.datasource.s0.username = root
spring.shardingsphere.datasource.s0.password = root
spring.shardingsphere.datasource.s1.type = com.alibaba.druid.pool.DruidDataSource
spring.shardingsphere.datasource.s1.driver‐class‐name = com.mysql.jdbc.Driver
spring.shardingsphere.datasource.s1.url = jdbc:mysql://localhost:3307/product_db_1?
useUnicode=true
spring.shardingsphere.datasource.s1.username = root
spring.shardingsphere.datasource.s1.password = root
spring.shardingsphere.datasource.s2.type = com.alibaba.druid.pool.DruidDataSource
spring.shardingsphere.datasource.s2.driver‐class‐name = com.mysql.jdbc.Driver
spring.shardingsphere.datasource.s2.url = jdbc:mysql://localhost:3307/product_db_2?
useUnicode=true
spring.shardingsphere.datasource.s2.username = root
spring.shardingsphere.datasource.s2.password = root
# 主库从库逻辑数据源定义 ds0为store_db ds1为product_db_1 ds2为product_db_2
spring.shardingsphere.sharding.master‐slave‐rules.ds0.master‐data‐source‐name=m0
spring.shardingsphere.sharding.master‐slave‐rules.ds0.slave‐data‐source‐names=s0
spring.shardingsphere.sharding.master‐slave‐rules.ds1.master‐data‐source‐name=m1
spring.shardingsphere.sharding.master‐slave‐rules.ds1.slave‐data‐source‐names=s1
spring.shardingsphere.sharding.master‐slave‐rules.ds2.master‐data‐source‐name=m2
spring.shardingsphere.sharding.master‐slave‐rules.ds2.slave‐data‐source‐names=s2
# 默认分库策略，以store_info_id为分片键，分片策略为store_info_id % 2 + 1，也就是store_info_id为双数的
数据进入ds1，为单数的进入ds2
spring.shardingsphere.sharding.default‐database‐strategy.inline.sharding‐column = store_info_id
spring.shardingsphere.sharding.default‐database‐strategy.inline.algorithm‐expression = ds$‐>
{store_info_id % 2 + 1}
# store_info分表策略，固定分配至ds0的store_info真实表，
spring.shardingsphere.sharding.tables.store_info.actual‐data‐nodes = ds$‐>{0}.store_info
spring.shardingsphere.sharding.tables.store_info.table‐strategy.inline.sharding‐column = id
spring.shardingsphere.sharding.tables.store_info.table‐strategy.inline.algorithm‐expression =
store_info
# product_info分表策略，分布在ds1,ds2的product_info_1 product_info_2表 ，分片策略为product_info_id
% 2 + 1，product_info_id生成为雪花算法，为双数的数据进入product_info_1表，为单数的进入product_info_2
表
spring.shardingsphere.sharding.tables.product_info.actual‐data‐nodes = ds$‐>
{1..2}.product_info_$‐>{1..2}
spring.shardingsphere.sharding.tables.product_info.table‐strategy.inline.sharding‐column =
product_info_id
spring.shardingsphere.sharding.tables.product_info.table‐strategy.inline.algorithm‐expression =
product_info_$‐>{product_info_id % 2 + 1}
spring.shardingsphere.sharding.tables.product_info.key‐generator.column=product_info_id
spring.shardingsphere.sharding.tables.product_info.key‐generator.type=SNOWFLAKE
# product_descript分表策略，分布在ds1,ds2的product_descript_1 product_descript_2表 ，分片策略为
product_info_id % 2 + 1，id生成为雪花算法，product_info_id为双数的数据进入product_descript_1表，为单
数的进入product_descript_2表
spring.shardingsphere.sharding.tables.product_descript.actual‐data‐nodes = ds$‐>
{1..2}.product_descript_$‐>{1..2}
spring.shardingsphere.sharding.tables.product_descript.table‐strategy.inline.sharding‐column =
product_info_id
spring.shardingsphere.sharding.tables.product_descript.table‐strategy.inline.algorithm‐
expression = product_descript_$‐>{product_info_id % 2 + 1}
spring.shardingsphere.sharding.tables.product_descript.key‐generator.column=id
spring.shardingsphere.sharding.tables.product_descript.key‐generator.type=SNOWFLAKE
# 设置product_info,product_descript为绑定表
spring.shardingsphere.sharding.binding‐tables[0] = product_info,product_descript
# 设置region为广播表(公共表)，每次更新操作会发送至所有数据源
spring.shardingsphere.sharding.broadcast‐tables=region
# 打开sql输出日志
spring.shardingsphere.props.sql.show = true
```
#### 9.5.3 添加商品
```markdown
实体类，参考基础工程：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426160909274.png)

```java
DAO实现
@Mapper
@Component
public interface ProductDao {
	//添加商品基本信息
	@Insert("insert into product_info(store_info_id,product_name,spec,region_code,price)
	value(#{storeInfoId},#{productName},#{spec},#{regionCode},#{price})")
	@Options(useGeneratedKeys = true,keyProperty = "productInfoId",keyColumn = "id")
	int insertProductInfo(ProductInfo productInfo);
	//添加商品描述信息
	@Insert("insert into product_descript(product_info_id,descript,store_info_id) value(#
	{productInfoId},#{descript},#{storeInfoId})")
	@Options(useGeneratedKeys = true,keyProperty = "id",keyColumn = "id")
	int insertProductDescript(ProductDescript productDescript);
}

service实现，针对垂直分库的两个库，分别实现店铺服务、商品服务
@Service
public class ProductServiceImpl implements ProductService {
@Autowired
private ProductDao productDao;
@Override
@Transactional
public void createProduct(ProductInfo product) {
ProductDescript productDescript = new ProductDescript();
productDescript.setDescript(product.getDescript());
productDao.insertProductInfo(product);//新增商品基本信息
productDescript.setProductInfoId(product.getProductInfoId());
productDescript.setStoreInfoId(product.getStoreInfoId()); //冗余店铺信息
productDao.insertProductDescript(productDescript);//新增商品描述信息
}
}
```
```java
controller实现：
/**
* 卖家商品展示
*/
@RestController
public class SellerController {
	@Autowired
	private ProductService productService;
	@PostMapping("/products")
	public String createProject(@RequestBody ProductInfo productInfo) {
	productService.createProduct(productInfo);
	return "创建成功!";
}

单元测试：
@RunWith(SpringRunner.class)
@SpringBootTest(classes = ShardingJdbcDemoBootstrap.class)
public class ShardingTest {
	@Autowired
	ProductService productService;
	@Test
	public void testCreateProduct(){
	for(long i=1;i<10;i++){
		//store_info_id,product_name,spec,region_code,price,image_url
		ProductInfo productInfo = new ProductInfo();
		productInfo.setProductName("Java编程思想"+i);
		productInfo.setDescript("Java编程思想是一本非常好的Java教程"+i);
		productInfo.setRegionCode("110000");
		productInfo.setStoreInfoId(1);
		productInfo.setPrice(new BigDecimal(i));
		productService.createProduct(productInfo);
	}
}
...


这里使用了sharding-jdbc所提供的全局主键生成方式之一雪
花算法，来生成全局业务唯一主键。
通过添加商品接口新增商品进行分库验证，store_info_id为
偶数的数据在product_db_1,为奇数的数据在
product_db_2。
通过添加商品接口新增商品进行分表验证，product_id为偶
数的数据在product_info_1、product_descript_1，为奇数
的数据在product_info_2、product_descript_2。
```
#### 9.5.4 查询商品
```java
Dao实现：
在ProductDao中定义商品查询方法：
@Select("select i.*, d.descript, r.region_name placeOfOrigin " +
"from product_info i join product_descript d on i.id = d.product_info_id " +
"join region r on r.region_code = i.region_code order by i.id desc limit #{start},#
{pageSize}")
List<ProductInfo> selectProductList(@Param("start")int start,@Param("pageSize") int pageSize);

Service实现：
在ProductServiceImpl定义商品查询方法：
@Override
public List<ProductInfo> queryProduct(int page,int pageSize) {
int start = (page‐1)*pageSize;
return productDao.selectProductList(start,pageSize);
}

Controller实现：
@GetMapping(value = "/products/{page}/{pageSize}")
public List<ProductInfo> queryProduct(@PathVariable("page")int page,@PathVariable("pageSize")int
pageSize){
return productService.queryProduct(page,pageSize);
}

单元测试：
@Test
public void testSelectProductList(){
List<ProductInfo> productInfos = productService.queryProduct(1,10);
System.out.println(productInfos);
}

通过查询商品列表接口，能够查询到所有分片的商品信息，
关联的地理区域，店铺信息正确。
总结：
分页查询是业务中最常见的场景，Sharding-jdbc支持常
用关系数据库的分页查询，不过Sharding-jdbc的分页功能
比较容易让使用者误解，用户通常认为分页归并会占用大
量内存。 在分布式的场景中，将 LIMIT 10000000 , 10改
写为 LIMIT 0, 10000010 ，才能保证其数据的正确性。 
用户非常容易产生ShardingSphere会将大量无意义的数据
加载至内存中，造成内存溢出风险的错觉。 其实大部分
情况都通过流式归并获取数据结果集，因此ShardingSphere
会通过结果集的next方法将无需取出的数据全部跳过，并
不会将其存入内存。
但同时需要注意的是，由于排序的需要，大量的数据仍
然需要传输到Sharding-Jdbc的内存空间。 因此，采用LIMIT
这种方式分页，并非最佳实践。 由于LIMIT并不能通
过索引查询数据，因此如果可以保证ID的连续性，通过ID进行
分页是比较好的解决方案，例如：
SELECT * FROM t_order WHERE id > 100000 AND id <= 100010 ORDER BY id;

或通过记录上次查询结果的最后一条记录的ID进行下一页的查询，例如：
SELECT * FROM t_order WHERE id > 10000000 LIMIT 10;
排序功能是由Sharding-jdbc的排序归并来完成，由于在SQL中
存在 ORDER BY 语句，因此每个数据结果集自身是有序的，因
此只需要将数据结果集当前游标指向的数据值进行排序即可。 
这相当于对多个有序的数组进行排序，归并排序是最适合此
场景的排序算法。
```
#### 9.5.5 统计商品
```java
本小节实现商品总数统计，商品分组统计
Dao实现，在ProductDao中定义：
//总数统计
@Select("select count(1) from product_info")
int selectCount();
//分组统计
@Select("select count(1) as num from product_info group by region_code having num>1 ORDER BY
region_code ASC")
List<Map> selectProductGroupList();

单元测试：
@Test
public void testSelectCount(){
int i = productDao.selectCount();
System.out.println(i);
}
@Test
public void testSelectGroupList(){
List<Map> maps = productDao.selectProductGroupList();
System.out.println(maps);
}

总结：
分组统计
分组统计也是业务中常见的场景，分组功能的实现由Sharding-jdbc
分组归并完成。分组归并的情况最为复杂，它
分为流式分组归并和内存分组归并。 流式分组归并要求SQL的
排序项与分组项的字段必须保持一致，否则只能通过
内存归并才能保证其数据的正确性。
举例说明，假设根据科目分片，表结构中包含考生的姓名（为了
简单起见，不考虑重名的情况）和分数。通过SQL获取每位
考生的总分，可通过如下SQL：
SELECT name, SUM(score) FROM t_score GROUP BY name ORDER BY name;

在分组项与排序项完全一致的情况下，取得的数据是连续的，
分组所需的数据全数存在于各个数据结果集的当前游标所指
向的数据值，因此可以采用流式归并。如下图所示。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426161542187.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
进行归并时，逻辑与排序归并类似。 下图展现了进行next调用
的时候，流式分组归并是如何进行的。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426161605981.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
通过图中我们可以看到，当进行第一次next调用时，排在队列
首位的t_score_java将会被弹出队列，并且将分组值
同为“Jetty”的其他结果集中的数据一同弹出队列。 在获取了所有的姓
名为“Jetty”的同学的分数之后，进行累加操
作，那么，在第一次next调用结束后，取出的结果集是“Jetty”的分数
总和。 与此同时，所有的数据结果集中的游标
都将下移至数据值“Jetty”的下一个不同的数据值，并且根据数据结果
集当前游标指向的值进行重排序。 因此，包含
名字顺着第二位的“John”的相关数据结果集则排在的队列的前列。
```
## 10 Sharding-Proxy 简介
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426163530375.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426163547923.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426163603174.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
Sharding-Proxy 配置（分表）
1、进入 conf 目录，修改文件 server.yaml，打开两段内容注释
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426163643405.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
2、进入 conf 目录，修改 config-sharding.yaml
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426163737232.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426163750972.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
（2）配置分库分表规则
schemaName: sharding_db
dataSources:
 ds_0:
 url: jdbc:mysql://127.0.0.1:3306/edu_1?serverTimezone=UTC&useSSL=false
 username: root
 password: root
 connectionTimeoutMilliseconds: 30000
 idleTimeoutMilliseconds: 60000
 maxLifetimeMilliseconds: 1800000
 maxPoolSize: 50
shardingRule:
 tables:
 t_order:
 actualDataNodes: ds_${0}.t_order_${0..1}
 tableStrategy:
 inline:
 shardingColumn: order_id
 algorithmExpression: t_order_${order_id % 2}
 keyGenerator:
 type: SNOWFLAKE
 column: order_id
 bindingTables:
 - t_order
 defaultDatabaseStrategy:
 inline:
 shardingColumn: user_id
 algorithmExpression: ds_${0}
 defaultTableStrategy:
 none:
```

```markdown
3、启动 Sharding-Proxy 服务
（1）Sharding-Proxy 默认端口号 3307
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021042616383043.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
4、通过 Sharding-Proxy 启动端口进行连接
（1）打开 cmd 窗口连接 Sharding-Proxy，连接方式和连接mysql 一样的
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426163853596.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
（2）进行 sql 命令操作看到只有一个库
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021042616391589.png)

```markdown
（3）在 sharding_db 数据库创建表
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426163944156.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
（4）向表添加一条记录
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426164005954.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
5、回到本地 3306 端口实际数据库中，看到已经创建好了表和
添加数据
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426164037584.png)

```markdown
Sharding-Proxy 配置（分库）
1、创建两个数据库
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426164103621.png)

```markdown
2、找到 conf 目录，config-sharding.yaml
schemaName: sharding_db
dataSources:
 ds_0:
 url: jdbc:mysql://127.0.0.1:3306/edu_db_1?serverTimezone=UTC&useSSL=false
 username: root
 password: root
 connectionTimeoutMilliseconds: 30000
 idleTimeoutMilliseconds: 60000
 maxLifetimeMilliseconds: 1800000
 maxPoolSize: 50
 ds_1:
 url: jdbc:mysql://127.0.0.1:3306/edu_db_2?serverTimezone=UTC&useSSL=false
 username: root
  password: root
 connectionTimeoutMilliseconds: 30000
 idleTimeoutMilliseconds: 60000
 maxLifetimeMilliseconds: 1800000
 maxPoolSize: 50
shardingRule:
 tables:
 t_order:
 actualDataNodes: ds_${0..1}.t_order_${1..2}
 tableStrategy:
 inline:
 shardingColumn: order_id
 algorithmExpression: t_order_${order_id % 2 + 1}
 keyGenerator:
 type: SNOWFLAKE
 column: order_id
 bindingTables:
 - t_order
 defaultDatabaseStrategy:
 inline:
 shardingColumn: user_id
 algorithmExpression: ds_${user_id % 2}
 defaultTableStrategy:
 none:
```
```markdown
3、启动 Sharding-Proxy 服务
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426164206247.png)

```markdown
4、打开 cmd 仓库，连接 Sharding-Proxy 服务
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426164225231.png)

```markdown
（1）创建数据库表，向表添加记录
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021042616424877.png)

```markdown
（2）连接本地 3306 的 MySql 数据库服务器，表已经创建出来，
表里面有数据
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426164308239.png)
```markdown
Sharding-Proxy 配置（读写分离）
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426164355263.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426164407852.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
schemaName: master_slave_db
dataSources:
 master_ds:
 url: jdbc:mysql://127.0.0.1:3306/demo_ds_master?serverTimezone=UTC&useSSL=false
 username: root
 password: root
 connectionTimeoutMilliseconds: 30000
 idleTimeoutMilliseconds: 60000
 maxLifetimeMilliseconds: 1800000
 maxPoolSize: 50
 slave_ds_0:
  url: jdbc:mysql://127.0.0.1:3306/demo_ds_slave_0?serverTimezone=UTC&useSSL=false
 username: root
 password: root
 connectionTimeoutMilliseconds: 30000
 idleTimeoutMilliseconds: 60000
 maxLifetimeMilliseconds: 1800000
 maxPoolSize: 50
 slave_ds_1:
 url: jdbc:mysql://127.0.0.1:3306/demo_ds_slave_1?serverTimezone=UTC&useSSL=false
 username: root
 password: root
 connectionTimeoutMilliseconds: 30000
 idleTimeoutMilliseconds: 60000
 maxLifetimeMilliseconds: 1800000
 maxPoolSize: 50
masterSlaveRule:
 name: ms_ds
 masterDataSourceName: master_ds
 slaveDataSourceNames:
 - slave_ds_0
 - slave_ds_1
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426164459818.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
（2）向表添加记录，不指定向哪个库添加
把添加数据添加到主数据库里面
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210426164534794.png)


## 11 总结
```markdown
为什么分库分表？分库分表就是为了解决由于数据量过大而导
致数据库性能降低的问题，将原来独立的数据库拆分
成若干数据库组成 ，将数据大表拆分成若干数据表组成，使得单一
数据库、单一数据表的数据量变小，从而达到提
升数据库性能的目的。
分库分表方式：垂直分表、垂直分库、水平分库、水平分表
分库分表带来问题：由于数据分散在多个数据库，服务器导致了
事务一致性问题、跨节点join问题、跨节点分页、
排序、函数，主键需要全局唯一，公共表。
Sharding-JDBC基础概念：逻辑表，真实表，数据节点，绑定表，
广播表，分片键，分片算法，分片策略，主键生
成策略
Sharding-JDBC核心功能：数据分片，读写分离
Sharding-JDBC执行流程： SQL解析 => 查询优化 => 
SQL路由 => SQL改写 => SQL执行 => 结果归并
最佳实践：
系统在设计之初就应该对业务数据的耦合松紧进行考量，从
而进行垂直分库、垂直分表，使数据层架构清晰明了。
若非必要，无需进行水平切分，应先从缓存技术着手降低
对数据库的访问压力。如果缓存使用过后，数据库访问量
还是非常大，可以考虑数据库读、写分离原则。若当前数
据库压力依然大，且业务数据持续增长无法估量，最后可
考虑水平分库、分表，单表拆分数据控制在1000万以内。
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复sj
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
