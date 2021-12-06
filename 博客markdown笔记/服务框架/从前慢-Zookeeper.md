# Zookeeper
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210717005844525.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

##  1 Zookeeper简介
```markdown
ZooKeeper(动物园管理者)一个分布式的，开放源码的分布式
应用程序协调服务，是Google的Chubby一个开源的实现，
是Hadoop和Hbase的重要组件
ZooKeeper使用Java所编写，但是支持 Java和C 两种编
程语言。

Zk 提供的功能:
1. 分布式锁: 解决分布式环境中数据一致性问题
2. 集群管理: 负载均衡容灾处理
```

## 2 Zookeeper的数据模型
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029153841423.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
## 3 Zookeeper中数据模型的特点
```markdown
每个子目录如/node1都被称作一个znode(节点)。这个znode 
是被它所在的路径唯一标识

znode可以有子节点目录，并且每个znode可以存储数据

znode是有版本的，每个znode中存储的数据可以有多个版本，
也就是一个访问路径中可以存储多份数据

znode可以被监控，包括这个目录节点中存储的数据的修改，
子节点目录的变化等，一旦变化可以通知设置监控的客户端
```

## 4 Zookeeper中节点的分类
```markdown
持久节点（PERSISTENT）
 是指在节点创建后，就一直存在，直到有删除操作来主动删除这个
 节点——不会因为创建该节点的客户端会话失效而消失

持久顺序节点（PERSISTENT_SEQUENTIAL）
这类节点的基本特性和上面的节点类型是一致的。额外的特性是，
在ZK中，每个父节点会为他的第一级子节点维护一份时序，会
记录每个子节点创建的先后顺序。基于这个特性，在创建子节
点的时候，可以设置这个属性，那么在创建节点过程中，ZK会
自动为给定节点名加上一个数字后缀，作为新的节点名。这个
数字后缀的范围是整型的最大值。

临时节点（EPHEMERAL）
和持久节点不同的是，临时节点的生命周期和客户端会话绑定。
也就是说，如果客户端会话失效，那么这个节点就会自动被清
除掉。注意，这里提到的是会话失效，而非连接断开。另外，
在临时节点下面不能创建子节点。 

临时顺序节点（EPHEMERAL_SEQUENTIAL）
```

## 5 Zookeeper中节点的监听
```markdown
客户端可以监测znode节点的变化。Zonode节点的变化触发相
应的事件，然后清除对该节点的监测。当监测一个znode节点时
候，Zookeeper会发送通知给监测节点。

watch机制说明：一个Watch事件是一个一次性的触发器，当被
设置了Watch的数据发生了改变的时候，则服务器将这个改变
发送给设置了Watch的客户端，以便通知它们。

通过这个特性可以实现的功能包括配置的集中管理，集群管理，
分布式锁等等。
```

## 6 zookeeper的安装
```markdown
a)	下载zookeeper的安装包
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020122607485331.png)
```markdown
b)	上传至linux系统中 并使用 tar -zxvf apache-zookeeper-3.6.2-bin.tar.gz 解压缩
tar -zxvf apache-zookeeper-3.6.2-bin.tar.gz
c)	解压缩zookeeper的tar包 重命名为zk便于操作,(可省略)
mv apache-zookeeper-3.6.2-bin zk
d)	修改zk的conf目录下的zoo_simple.cfg，修改完后，重命名为zoo.cfg
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/usr/local/zkData  ----保存snapshot文件的路径
clientPort=2181

各项参数说明：
tickTime：配置单元时间。单元时间是ZooKeeper的时间计算单元，
其他的时间间隔都是使用tickTime的倍数来表示的。


initLimit：节点的初始化时间。该参数用于Follower（从节点）的启动，
并完成与Leader（主节点）进行数据同步的时间。Follower节点在启
动过程中，会与Leader节点建立连接并完成对数据的同步，从而确
定自己的起始状态。Leader节点允许Follower节点在initLimit时
间内完成这项工作。该参数默认值为10，表示是参数tickTime值的10倍。


syncLimit：心跳最大延迟周期。该参数用于配置Leader节点和
Follower节点之间进行心跳检测的最大延时时间。在ZK集群运
行的过程中，Leader节点会通过心跳检测来确定Follower节点是
否存活。如果Leader节点在syncLimit时间内无法获取到Follower
节点的心跳检测响应，那么Leader节点就会认为该Follower节点已
经脱离了和自己的同步。该参数默认值为5，表示是参数tickTime值
的5倍。


dataDir：是zookeeper持久化数据存放的目录。
myid文件处于此目录下。


dataLogDir：日志目录选项，如果没有设置该参数，
默认将使用和dataDir相同的设置。


clientPort：zookeeper监听客户端连接的端口，默认是2181。


e)	启动zk，在zk的bin目录下，运行zkServer.sh
./zkServer.sh start /usr/local/zookeeper/conf/zoo.cfg
ps: ./zkServer.sh help 可以查看服务器端所有可以执行的指令

补充： 
# 启动服务端
$ bin/zkServer.sh start

# 查看状态
$ bin/zkServer.sh status

# 停止
$ bin/zkServer.sh stop

# 重启
$ bin/zkServer.sh restart
f)	使用jps查看启动是否成功
46193 QuorumPeerMain # 表示成功
g)	启动客户端连接到zk
./zkCli.sh -server 192.168.0.220:2181
ps:  可以通过 ./zkCli.sh help 查看客户端所有可以执行的指令
```


## 7 客户端基本指令
### 7.1  对 znode 进行增删改查等命令
```markdown
1.创建节点 create  注意： 默认是持久性节点
create [-s] [-e] [-c] [-t ttl] path [data] [acl]
-s 创建有序节点
如果在创建znode时，我们使用排序标志的话，ZooKeeper会在我们
指定的 znode 名字后面增加一个数字。我们继续加入相同名字
的znode时，这个数字会不断增加。这个序号的计数器是由这些排
序znode的父节点来维护的。

-e 创建临时节点
znode有两种类型：ephemeral 和 persistent。当创建znode的
客户端的session结束后，ephemeral类型的znode将被删除。
persistent类型的znode在创建以后，就与客户端没什么联系了，
除非主动去删除它，否则他会一直存在。Ephemeral znode没有任
何子节点。

acl 在下面的《ACL操作》中详细介绍。

普通节点
[zk: localhost:2181(CONNECTED) 3] create /mynode hello
Created /mynode
[zk: localhost:2181(CONNECTED) 4] create /mynode/subnode world
Created /mynode/subnode

[zk: localhost:2181(CONNECTED) 9] get /mynode
hello
[zk: localhost:2181(CONNECTED) 10] get /mynode/subnode
world

有序节点
[zk: localhost:2181(CONNECTED) 4] create -s /mynode hello
Created /mynode0000000004
[zk: localhost:2181(CONNECTED) 6] create -s /mynode world
Created /mynode0000000005

临时节点
[zk: localhost:2181(CONNECTED) 7] create -e /temp hello
Created /temp
退出 zkCli（quit或者直接退出连接终端工具），然后再重新打开它，/temp 节点已经被删除了(如果打开多个客户端，则所有客户端都退出节点才被删除)。

2.列出节点 ls
ls [-s] [-w] [-R] path
-w 添加一个 watch（监视器），如果该节点发生变化，watch 可以使客
户端得到通知。watch只能被触发一次。如果要一直获得 znode 的创
建和删除的通知，那么就需要不断的在znode上开启观察模式。如果
在该path下节点发生变化，会产生NodeChildrenChanged事件，
删除节点，会产生 NodeDeleted 事件。

[zk: localhost:2181(CONNECTED) 12] ls /
[mynode, mynode0000000003, mynode0000000004, test, zookeeper]
[zk: localhost:2181(CONNECTED) 13] ls -s /
[mynode, mynode0000000003, mynode0000000004, test, zookeeper]
cZxid = 0x0
ctime = Thu Jan 01 08:00:00 CST 1970
mZxid = 0x0
mtime = Thu Jan 01 08:00:00 CST 1970
pZxid = 0x300000053
cversion = 7
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 0
numChildren = 5

[zk: localhost:2181(CONNECTED) 2] ls /mynode
[subnode]

使用 -w 查看 /mynode 节点，然后在它下面添加（删除）子节点，
就会触发该 watch。在其他节点下创建子节点，不会触发该 watch。

[zk: localhost:2181(CONNECTED) 20] ls -w /mynode
[subnode]
[zk: localhost:2181(CONNECTED) 21] create /mynode/subnode2

WATCHER::

WatchedEvent state:SyncConnected type:NodeChildrenChanged path:/mynode
Created /mynode/subnode2

# 监听父节点，删除子节点，产生 NodeChildrenChanged事件
[zk: localhost:2181(CONNECTED) 22] ls -w /mynode
[subnode, subnode2]
[zk: localhost:2181(CONNECTED) 23] delete /mynode/subnode2

WATCHER::

WatchedEvent state:SyncConnected type:NodeChildrenChanged path:/mynode

# 监听子节点，删除子节点，产生 NodeDeleted 事件
[zk: localhost:2181(CONNECTED) 51] create /mynode/subnode2
Created /mynode/subnode2
[zk: localhost:2181(CONNECTED) 52] ls -w /mynode/subnode2
[]
[zk: localhost:2181(CONNECTED) 53] delete /mynode/subnode2

WATCHER::

WatchedEvent state:SyncConnected type:NodeDeleted path:/mynode/subnode2

从上面的操作可以看到，在 /mynode 下添加了subnode2节点之后，
触发了watch，WatchedEvent的类型是NodeChildrenChanged。
之后再删除 subnode2 节点，也出发了 watch。

3. 获取节点信息 get
get [-s] [-w] path

-w 添加一个 watch（监视器），如果节点内容发生改变，会产生 NodeDataChanged 事件；如果删除节点，会产生 NodeDeleted 事件。

[zk: localhost:2181(CONNECTED) 33] get /mynode
helloo
[zk: localhost:2181(CONNECTED) 34] get -s /mynode
helloo
cZxid = 0x30000004c
ctime = Sun Apr 05 15:48:14 CST 2020
mZxid = 0x30000005d
mtime = Sun Apr 05 16:05:56 CST 2020
pZxid = 0x30000005c
cversion = 7
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 6
numChildren = 1

# 监听节点内容，发生变化后，产生 NodeDataChanged 事件
[zk: localhost:2181(CONNECTED) 35] get -w /mynode
helloo
[zk: localhost:2181(CONNECTED) 36] set /mynode hello

WATCHER::

WatchedEvent state:SyncConnected type:NodeDataChanged path:/mynode

# 删除节点，产生 NodeDeleted 事件
[zk: localhost:2181(CONNECTED) 43] get -w /mynode/subnode
world
[zk: localhost:2181(CONNECTED) 44] delete /mynode/subnode

WATCHER::

WatchedEvent state:SyncConnected type:NodeDeleted path:/mynode/subnode

每一个对znode树的更新操作，都会被赋予一个全局唯一的ID，我们称之为zxid
（ZooKeeper Transaction ID）。更新操作的ID按照发生的时间顺序升序排序。
例如，z1大于z2，那么z1的操作就早于z2操作。


每个 znode 的状态信息包含以下内容：
czxid，创建（create）该 znode 的 zxid
mzxid，最后一次修改（modify）该znode的zxid
pzxid，最后一次修改该znode子节点的 zxid
ctime，创建该znode的时间
mtime，最后一次修改该znode的时间
dataVersion，该节点内容的版本，每次修改内容，版本都会增加
cversion，该节点子节点的版本
aclVersion，该节点的 ACL 版本
ephemeralOwner，如果该节点是临时节点（ephemeral node），会列出该节点所在客户端的 session id；如果不是临时节点，该值为 0
dataLength，该节点存储的数据长度
numChildren，该节点子节点的个数

4.检查状态 stat
stat [-w] path

-w 添加一个 watch（监视器），如果节点内容发生改变，会产生 NodeDataChanged 事件；如果删除节点，会产生 NodeDeleted 事件。
与 get 的区别是，不会列出 znode 的值。

[zk: localhost:2181(CONNECTED) 56] stat /mynode
cZxid = 0x30000004c
ctime = Sun Apr 05 15:48:14 CST 2020
mZxid = 0x30000005e
mtime = Sun Apr 05 16:09:32 CST 2020
pZxid = 0x300000067
cversion = 16
dataVersion = 2
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 5
numChildren = 0

5. 修改节点 set
set [-s] [-v version] path data
修改已经存在的节点的值

[zk: localhost:2181(CONNECTED) 57] set /mynode hello
[zk: localhost:2181(CONNECTED) 58] ls /mynode
[]
[zk: localhost:2181(CONNECTED) 59] stat /mynode
cZxid = 0x30000004c
ctime = Sun Apr 05 15:48:14 CST 2020
mZxid = 0x300000068
mtime = Sun Apr 05 16:20:34 CST 2020
pZxid = 0x300000067
cversion = 16
dataVersion = 3
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 5
numChildren = 0

可以看到，在修改节点值之后，mZxid、mtime、dataVersion 都发生了变化。

6.删除节点 delete、deleteall
 delete [-v version] path
 如果要删除的节点有子节点，不能删除
[zk: localhost:2181(CONNECTED) 33] create /mynode/sub sub
Created /mynode/sub
[zk: localhost:2181(CONNECTED) 34] delete /mynode
Node not empty: /mynode

deleteall path [-b batch size]
[zk: localhost:2181(CONNECTED) 34] delete /mynode
删除 /mynode，不会返回任何内容。如果有子节点的时候，都会删除。

调用delete和set操作时，如果指定znode版本号，需要与当前的版本号匹配。如
果版本号不匹配，操作将会失败。失败的原因可能是在我们提交之前，该znode
已经被修改过了，版本号发生了增量变化。如果不指定版本号，就是直接操作最
新版本的 znode。


7.历史记录 history
history 列出最近的10条历史记录
[zk: localhost:2181(CONNECTED) 7] history
0 - history
1 - create /mynode hello
2 - ls /
3 - set /mynode worold
4 - get /mynode
5 - stat /mynode
6 - rmr /mynode
7 - history
```
## 8 ZK的集群架构图

![在这里插入图片描述](https://img-blog.csdnimg.cn/202010291539516.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
## 9 ZK集群的搭建
```markdown
1、在每台机器zookeeper/conf目录下创建三个zk配置文件，
为 zoo.cfg(可以将zookeeper里面的zoo_sample.cfg改为zoo.cfg,zookeeper
会按这个文件进行启动)
各个内容为（每个文件都是这个内容）：
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/usr/local/zookeeper/zkdata
clientPort=2181
server.1=192.168.42.129:2888:3888
server.2=192.168.42.130:2888:3888
server.3=192.168.42.131:2888:3888
server.4=192.168.42.132:2888:3888
server.5=192.168.42.133:2888:3888


2、在每台机器上创建数据存放的目录，存放的路径为
dataDir=/usr/local/zookeeper/zkdata（跟你上面写的内容一样）

3、分别在每台机器上（dataDir=/usr/local/zookeeper/zkdata）
创建myid文件myid的内容分别是
server.1=192.168.42.129:2888:3888中的数字1，
server.2=192.168.42.130:2888:3888中的数字2，
server.3=192.168.42.131:2888:3888中的数字3，
server.4=192.168.42.132:2888:3888中的数字4，
server.5=192.168.42.133:2888:3888中的数字5


ps： server.X :x为服务器的唯一标识。
192.168.42.129：服务器所在的ip地址
2888：数据同步使用的端口号
3888：选举使用的端口号

4、分别启动各个zk服务器(进入zookeeper的bin目录)
./zkServer.sh  start  /usr/local/zookeeper/conf/zoo.cfg （这里可以省略 /usr/local/zookeeper/conf/zoo.cfg）

5、查看各个zk服务器的角色信息(进入zookeeper的bin目录)
./zkServer.sh  status /usr/local/zookeeper/conf/zoo.cfg  （这里可以省略 /usr/local/zookeeper/conf/zoo.cfg）

6、客户端连接任意zk服务器进行节点操作(进入zookeeper的bin目录)
./zkCli.sh  -server  192.168.42.129:2181(ip地址加端口)（连接本机则./zkCli.sh也行）

7.停止zk服务器(进入zookeeper的bin目录)
./zkServer.sh stop  ./usr/local/zookeeper/conf/zoo.cfg  （这里可以省略 /usr/local/zookeeper/conf/zoo.cfg）
```

## 10 java操作ZK
```xml
项目中引入如下依赖:
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>


<dependency>
    <groupId>com.101tec</groupId>
    <artifactId>zkclient</artifactId>
    <version>0.10</version>
</dependency>
```
### 10.1 通过zkClient创建节点
```java
private ZkClient zkClient;

/**
 * 获取zk客户端连接
 */
@Before
public void Before(){
    //参数1:服务器的ip和端口，多台机器之间使用“，”，隔开
    //参数2:会话的超时时间，sessionTimeout  毫秒
    //参数3:回话的连接时间 connectionTimeout 毫秒
    //参数4:序列化方式   
    zkClient = new ZkClient("192.168.28.132:2181,192.168.28.132:2181",30000,60000,new SerializableSerializer());
}


/**
 * 创建节点
c */
@Test
public void testCreateNode(){
    //第一中创建方式  返回创建节点的名称
    String nodeName = zkClient.create("/node5","lisi", CreateMode.PERSISTENT);
    zkClient.create("/node6","zhangsan", CreateMode.PERSISTENT_SEQUENTIAL);
    zkClient.create("/node7","王五",CreateMode.EPHEMERAL);
    zkClient.create("/node8","xiaozhang",CreateMode.EPHEMERAL_SEQUENTIAL);
    //第二种创建方式 不会返回创建节点的名称
    zkClient.createPersistent("/node1","持久数据");
    zkClient.createPersistentSequential("/node1/aa","持久数据顺序节点");
    zkClient.createEphemeral("/node2","临时节点");
    zkClient.createEphemeralSequential("/node1/bb","临时顺序节点");
}
/**
 * 关闭资源
 */
@After
public void after(){
    zkClient.close();
}
```
### 10.2 删除节点
```java
/**
 * 删除节点
 */
@Test
public void testDeleteNode(){
    //删除没有子节点的节点  返回值:是否删除成功
    boolean delete = zkClient.delete("/node1");
    //递归删除节点信息 返回值:是否删除成功
    boolean recursive = zkClient.deleteRecursive("/node1");
}

```
### 10.3 查看节点的子节点
```java
/**
 * 查询节点
 */
@Test
public void testFindNodes(){
    //获取指定路径的节点信息  //返回值: 为当前节点的子节点信息
    List<String> children = zkClient.getChildren("/");
    for (String child : children) {
        System.out.println(child);
    }
}
```

### 10.4 查看当前节点的数据
```java
/**
 * 获取节点的数据
 *
 */
@Test
public void testFindNodeData(){
    Object readData = zkClient.readData("/node3");
    System.out.println(readData);
}
注意:
如果出现:org.I0Itec.zkclient.exception.ZkMarshallingError: 
java.io.StreamCorruptedException: invalid stream header: 61616161 
异常的原因是: 在shell中的数据序列化方式 和java代码
中使用的序列化方式不一致导致  因此要解决这个问题只需要
保证序列化一致即可都使用相同端操作即可
```

### 10.5 查看当前节点的数据并获取状态信息
```java
/**
 * 获取数据以及当前节点的状态信息
 */
@Test
public void testFindNodeDataAndStat(){
    Stat stat = new Stat();
    Object readData = zkClient.readData("/node60000000024",stat);
    System.out.println(readData);
    System.out.println(stat);
}
```
### 10.6 修改节点的数据
```java
/**
 * 修改节点数据
 */
@Test
public void testUpdateNodeData(){
    zkClient.writeData("/node60000000024", new User("121","name","xxx"));
}
```

### 10.7 监听节点数据的变化
```java
/**
 * 监听节点数据的变化
 */
@Test
public  void testOnNodeDataChange() throws IOException {
    zkClient.subscribeDataChanges("/node60000000024", new IZkDataListener() {

        //当节点的值在修改时,会自动调用这个方法  将当前修改节点的名字,和节点变化之后的数据传递给方法
        @Override
        public void handleDataChange(String nodeName, Object result) throws Exception {
            System.out.println(nodeName);
            System.out.println(result);
        }

        //当节点的值被删除的时候,会自动调用这个方法,会将节点的名字已参数形式传递给方法
        @Override
        public void handleDataDeleted(String nodename) throws Exception {
            System.out.println("节点的名字:"+nodename);
        }
    });
    //阻塞客户端
    System.in.read();
}

```
### 10.8 监听节点目录变化
```java
/**
 * 监听节点的变化
 */
@Test
public  void testOnNodesChange() throws IOException {
    zkClient.subscribeChildChanges("/node60000000024", new IZkChildListener() {
        //当节点的发生变化时,会自动调用这个方法
        //参数1:父节点名称
        //参数2:父节点中的所有子节点名称
        @Override
        public void handleChildChange(String nodeName, List<String> list) throws Exception {
            System.out.println("父节点名称: "+nodeName);
            System.out.println("发生变更后字节孩子节点名称:");
            for (String name : list) {
                System.out.println(name);
            }
        }
    });
    //阻塞客户端
    System.in.read();
}
```
## 11 图片总结
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029155158274.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102915520625.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029155214805.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复zk
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)







