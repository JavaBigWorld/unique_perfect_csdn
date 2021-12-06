# mongoDB
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210716152628389.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1 MongoDB的引言
```markdown
MongoDB是一个基于分布式文件存储的数据库。由C++语言编写。
旨在为WEB应用提供可扩展的高性能数据存储解决方案。  
Nosql 技术门类 redis 内存型 mongodb 文档型
MongoDB是一个介于关系数据库和非关系数据库之间的产品，
是非关系数据库当中功能最丰富，最像关系数据库的。他支持
的数据结构非常松散，是类似json的bson格式，因此可以存储
比较复杂的数据类型。Mongo最大的特点是他支持的查询语言
非常强大，其语法有点类似于面向对象的查询语言，几乎可
以实现类似关系数据库单表查询的绝大部分功能，而且还支持对数据建立索引。
```


## 2 MongoDB的特点
```markdown
面向集合存储，易存储对象类型的数据
支持查询,以及动态查询
支持RUBY，PYTHON，JAVA，C++，PHP，C#等多种语言
文件存储格式为BSON（一种JSON的扩展）
支持复制和故障恢复和分片
```
### 2.1 mongo相关术语
```markdown
在一个数据库软件中可以包含多个数据仓库，在每个数据仓库
中可以包含多个数据集合 ，每个数据集合中可以包含多条文
档（具体的数据）。

database
数据库，mongoDB数据库软件中可以建立多个数据库
collection
集合，一组数据的集合，可以理解为JavaScript中的数组
document
文档，一条具体的数据，可以理解为JavaScript中的对象
field
字段，文档中的属性名称，可以理解为JavaScript中的对象属性
```
## 3 MongoDB的安装和使用
```markdown
1.	下载mongoDB的安装包(最新版本3.6,只能在64位系统安装)
这里使用的4.0.21版本
```

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201216230445963.png)
```markdown
2.	上传至linux系统中解压当前的linux系统
```


 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201216230620518.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
3.	将解压的文件为了方便目录进行重命名(这步可以跳过)
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201216230715707.png)

 ```markdown
4.	进入mongodb的文件夹中查看目录
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201216230823635.png)
```markdown
5.	在bin目录中存在大量mongodb使用的命令
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201216230850965.png)

```markdown
6. 创建目录跟文件
# cd /usr/local/mongodb
# mkdir db
# mkdir log
7.	mongodb目录下新建一个名为mongodb.conf的配置文件，
写入如下配置内容
port=27017 #端口  
dbpath= /usr/mongodb/db #数据库存文件存放目录  
logpath= /usr/mongodb/mongodb.log #日志文件存放路径  
logappend=true #使用追加的方式写日志  
fork=false #不以守护程序的方式启用，即不在后台运行  
maxConns=100 #最大同时连接数  
noauth=true #不启用验证  
journal=true #每次写入会记录一条操作日志（通过journal可以重新构造出写入的数据）。
#即使宕机，启动时wiredtiger会先将数据恢复到最近一次
的checkpoint点，然后重放后续的journal日志来恢复。
storageEngine=wiredTiger  #存储引擎有mmapv1、wiretiger、mongorocks
bind_ip = 0.0.0.0  #这样就可外部访问了，例如从win10中去连虚拟机中的MongoDB

7. 输入命令启动./mongod --config /usr/local/mongodb/mongodb.conf
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210108123206155.png)

```markdown
注意:
启动时要求存放数据的目录必须存在
默认的端口号是27017 可以通过--port 指定端口启动

授权登录
在日常工作中我们不可能把数据库设置为免认证登录并暴露在公
网下，所以我们需要为数据库添加用户名和密码，具体操作如下
我们把noauth那一行，前面加上#，注释掉。
再在最后一行添加 auth = true


port=27017 #端口
dbpath= /usr/local/mongodb/db #数据库存文件存放目录
logpath= /usr/local/mongodb/log/mongodb.log #日志文件存放路径
logappend=true #使用追加的方式写日志
fork=true #以守护进程的方式运行，创建服务器进程
maxConns=100 #最大同时连接数
#noauth = true #不启用验证
journal=true #每次写入会记录一条操作日志（通过journal可以
重新构造出写入的数据）。
#即使宕机，启动时wiredtiger会先将数据恢复到最近
一次的checkpoint点，然后重放后续的journal日志来恢复。
storageEngine=wiredTiger  #存储引擎有mmapv1、wiretiger、mongorocks
bind_ip = 0.0.0.0  #这样就可外部访问了，例如从win10中去连虚拟机中的MongoDB
auth = true #用户认证

启动数据库,./mongod --config /usr/local/mongodb/mongodb.conf
执行以下命令
//使用admin数据库
use admin
//给admin数据库添加管理员用户名和密码，用户名和密码请自行设置
db.createUser({user:"admin",pwd:"123456",roles:["root"]})
//验证是否成功，返回1则代表成功
db.auth("admin", "123456") # 验证成功后才可以执行show dbs这些命令

//切换到要设置的数据库,以test为例
use test
//为test创建用户,用户名和密码请自行设置。
db.createUser({user: "test", pwd: "123456", roles: [{ role: "dbOwner", db: "test" }]})

```

## 4 MongoDB的shell(客户端)操作
```markdown
1 进入mongo的bin目录中找到如下指令
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/2020121623155314.png)

```markdown
2 使用如下命令连接到mongodb的服务中
```
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201216231640371.png)
```markdown
注意:
a)	连接到mongodb后,mongo和mysql数据库有点像,先是一个一
个库的概念,操作之前需要先选中库
```

```markdown
3 查看系统中默认的所有库 
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201216230320505.png)
```markdown
4 选中一个库
a)	use  数据库名称
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201216231731612.png)
```markdown
注意:
use命名 存在库使用当前库  不存在则创建当前库
```

```markdown
5 删除一个库
db.dropDatabase();
```
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201216231828461.png)
```markdown
注意: 选中那个库,删除的就是当前选中的库
```

```markdown
6 Mongodb的数据库中,库中是一个一个集合的概念,选中库后要创建一个一个的集合,集合类似于传统的关系型数据库中的表
a)	显示创建集合 db.createCollection(“t_user”);
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201216232012565.png)
```markdown
7	查看mongo 中当前库 
a)	db 命令显示当前库
```

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201216232348378.png)
```markdown
8	显示当前库中的所有集合
a)	show collections;
```

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201216232418326.png)
```markdown
9	Mongo中插入数据
a)	向集合中插入数据
db.集合名称.insert({name:'xiaohei',age:23,sex:true});
b)	向集合中插入多条数据
db.集合名称.insert([{name:'xiaohei',age:23,sex:true},...]);	
```
```markdown
10	Mongo中的删除数据
a)	db.集合名称.remove({条件}) //删除满足条件的数据
b)	db.集合名称.remove({不加任何条件})//删除所有文档   保留空的集合
```
```markdown
11	Mongo中的修改数据
a)	db.集合名称.update({条件},{更新内容}); 
b)	db.集合名称.update({"name":"zhangsan"},{name:"11",bir:new date()}) 
--这个更新是将符合条件的全部更新成后面的文档,相当于
先删除在更新
c)	db.集合名称.update({"name":"xiaohei"},{$set:{name:"mingming"}})    
--保留原来的值修改,但是只更新符合条件的第一条数据
d)	db.集合名称.update({name:”小黑”},{$set:{name:”小明”}},{multi:true})  
---保留原来数据更新,更新符合条件的所有数据
e)	db.集合名称.update({name:”小黑”},{$set:{name:”小明”}},{multi:true,upsert:true})  
---保留原来数据更新,更新符合条件的所有数据 没有条件符合时
插入数据
f)	db.t_user.update({name:"zhangsan"},{$inc:{age:1}},{upsert:true,multi:true}) 
--在保留原始数据同时给符合条件的所有age这列的值自增指定的大小
```

```markdown
12	删除集合
a)	db.集合名称.drop();
```

```markdown
13	查询集合
a)	db.集合名称.find();
b)	db.集合名称.find({条件})
c)	db.集合名称.find({条件},{显示字段,name:1,age:1}) 1 显示  0  不显示    1和0 不能混合出现
d)	查询结果排序:db.集合名称.find().sort({条件name:1,age:1}), 1   升序     -1  降序
e)	分页查询:db.集合名称.find().sort({条件}).skip(起始条数).limit(显示总记录数);
f)	总条数:db.集合名称.count();|db.t_user.find({"name":"aa"}).count();
g)	模糊查询:使用正则表达式db.集合名称.find({"name":/go/})
h)	等值(==)查询
  db.user.find({name:"张三"});
db.user.find({name:{$eq:"张三"}});
i)	且 ($and) 查询
db.t_user.find({name:"zhangsan",age:12});
db.t_user.find({$and:[{name:"zhangsan"},{id:10}]}) 
j)	$or使用:
i.	
db.集合名称.find({
			$or:[
				{key:value},{age:{$gte:20}}
			]	
		});
k)	$gt大于 $gte大于等于 $lt 小于 $lte 小于等于 $eq 等于:
i.	db.集合名称.find({“age”:{“$lte”:18,”$gte”:30}})
l)	$nor查询使用
i.	db.t_user.find({$nor:[{name:"chenyn"},{age:26}]});
```
```markdown
15.	shell非正常关闭时,下次无法连接问题解决方案:
i.	删除数据目录中的mongo.lock文件即可
```

## 5 mongo跟mysql命令常用命令对比

```markdown
查询
MySQL:  SELECT * FROM user
Mongo:  db.user.find()
MySQL:   SELECT * FROM user WHERE name = "xiaoming"
Mongo:   db.user.find({name : "xiaoming"})

插入
MySQL:  INSERT INTO user (name, age) values ("xiaoming",25)
Mongo:  db.user.insert({name: "xiaoming", age : 25})

修改集合字段
MySQL:  ALTER TABLE user add email varchar(100) comment “邮箱”;
Mongo:  db.user.insert({name: "xiaoming", age: 25, email : "xiaoming@qq.com"})

删除表内容
MySQL:  DELETE * FROM user
Mongo:  db.user.remove({})

根据条件删除
MySQL:  DELETE FROM user WHERE age < 30
Mongo:   db.user.remove({‘age’ : {$lt : 30}})
其他比较符号 gt:>;gte : >= ; lt:<;lte : <= ; $ne : !=

更新
MySQL:  UPDATE user SET age = 36 WHERE name = "xiaoming"
Mongo:  db.user.update({name: "xiaoming"}, {$set : {age: 36}})

根据条件更新
MySQL:  UPDATE user SET age = age + 3 WHERE name = "xiaoming"
Mongo:  db.user.update({name:"xiaoming"}, {$inc : {age : 3}})

```

## 6 Java操作mongoDB
```markdown
1. 开启远程连接   
上面那个mongodb.conf已经设置可以远程连接了
bind_ip = 0.0.0.0  #这样就可外部访问了
```
```markdown
2.	项目中引入mongo的坐标
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201023121229281.png#pic_center)
```markdown
3.	使用java操作mongo
```
```java
package com.yxj.test;

import com.mongodb.MongoClient;
import com.mongodb.client.FindIterable;
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoDatabase;
import com.mongodb.client.MongoIterable;
import org.bson.Document;
import org.junit.After;
import org.junit.Before;
import org.junit.Test;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;

public class TestMongo001 {

    private MongoClient mongoClient;


    @Before
    public void before(){
        mongoClient = new MongoClient("192.168.28.136",27017);
    }


    /**
     * 获取所有集合名称
     */
    @Test
    public void testGetCollectionNames(){
        MongoDatabase database = mongoClient.getDatabase("test");
        MongoIterable<String> strings = database.listCollectionNames();
        for (String name : strings) {
            System.out.println(name);
        }
    }

    /**
     * 获取指定的集合
     */

    @Test
    public void testGetCollection(){
        MongoDatabase database = mongoClient.getDatabase("test");
        MongoCollection<Document> collection = database.getCollection("t_user");
    }

    /**
     * 获取指定集合中的所有数据
     */

    @Test
    public void testGetCollectionFindAll(){
        MongoDatabase database = mongoClient.getDatabase("test");
        MongoCollection<Document> collection = database.getCollection("t_user");
        FindIterable<Document> documents = collection.find();
        for (Document document : documents) {
            System.out.println(document);
            System.out.print(document.get("name")+"===");
            System.out.print(document.get("age")+"===");
            System.out.println();
        }
    }

    /**
     * 保存一条数据
     */
    @Test
    public void testSave(){
        MongoDatabase database = mongoClient.getDatabase("test");
        MongoCollection<Document> t_user = database.getCollection("t_user");
        Document document = new Document();
        document.put("_id","1");
        document.put("name","xiaochen");
        document.put("age",23);
        document.put("bir",new Date());
        t_user.insertOne(document);
    }

    /**
     * 保存多条数据
     */
    @Test
    public void testSaveMany(){
        MongoDatabase database = mongoClient.getDatabase("test");
        MongoCollection<Document> t_user = database.getCollection("t_user");
        List<Document > list = new ArrayList<Document>();
        Document document = new Document();
        document.put("_id","3");
        document.put("name","xiaohei");
        document.put("age",23);
        document.put("bir",new Date());
        Document document1 = new Document();
        document1.put("_id","2");
        document1.put("name","xiaohei");
        document1.put("age",23);
        document1.put("bir",new Date());
        list.add(document);
        list.add(document1);
        t_user.insertMany(list);
    }


    /**
     * 删除一条数据
     */
    @Test
    public void testDelete(){
        MongoDatabase database = mongoClient.getDatabase("test");
        MongoCollection<Document> t_user = database.getCollection("t_user");
        Document document = new Document();
        document.put("_id",1);
        t_user.deleteOne(document);

    }

    /**
     * 删除多个数据
     */
    @Test
    public void testDeleteMany(){
        MongoDatabase database = mongoClient.getDatabase("test");
        MongoCollection<Document> t_user = database.getCollection("t_user");
        Document document = new Document();
        document.put("name","xiaochen");
        t_user.deleteMany(document);
    }


    /**
     * 查询总条数
     *
     */
    @Test
    public void tesCount(){
        MongoDatabase database = mongoClient.getDatabase("test");
        MongoCollection<Document> t_user = database.getCollection("t_user");
        System.out.println(t_user.count());
    }

    @After
    public void after(){
        mongoClient.close();
    }

}
```
```java
package com.yxj.test;

import com.mongodb.MongoClient;
import com.mongodb.client.FindIterable;
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoDatabase;
import com.mongodb.client.model.Filters;
import org.bson.Document;
import org.junit.After;
import org.junit.Before;
import org.junit.Test;

import java.util.regex.Pattern;

public class TestMongo002 {

    private MongoClient mongoClient;
    private MongoDatabase database;
    private MongoCollection<Document> t_user;

    @Before
    public void before(){
        mongoClient = new MongoClient("192.168.28.136",27017);
        database = mongoClient.getDatabase("test");
        t_user = database.getCollection("t_user");
    }


    /**
     * 有条件的查询数据
     */
    @Test
    public void testFind(){
        Document document = new Document();
        document.put("name", "xiaohei");
        FindIterable<Document> documents = t_user.find(document);
        for (Document doc : documents) {
            System.out.println(doc);
        }
    }

    /**
     * 有条件查询展示指定的字段
     */
    @Test
    public void  testFindField(){
        Document document = new Document();
        document.put("name","xiaohei");
        Document bson = new Document();
        bson.put("_id",1);
        bson.put("name",1);
        FindIterable<Document> documents = t_user.find(document)
                .projection(bson);//
        for (Document document1 : documents) {
            System.out.println(document1);
        }
    }


    /**
     * 查询数据取值范围
     * 条件搜索$lt/ $lte / $gt / $gte /  $ne / $eq <====> < / <= / >  /  >=  /  != /==
     */
    @Test
    public void tesRange(){
        FindIterable<Document> documents = t_user.find()
                .filter(Filters.gt("age", 54))
                .filter(Filters.lte("age", 24));
        for (Document document : documents) {
            System.out.println(document);
        }
    }

    /**
     * 条件搜索OR查询$in
     * 查询id的值是2或者是3的docuemnt
     */
    @Test
    public void testIn(){
        FindIterable<Document> documents = t_user.find().filter(Filters.in("_id", new String[]{"2", "3"}));
        for (Document document : documents) {
            System.out.println(document);
        }

    }


    /**
     * 条件搜索OR查询$or
     * 查询id是1或者是name等于jiangzz的所有文档
     */
    @Test
    public void testOr(){
        Document document1 = new Document();
        document1.put("name","chenyn");
        Document document2 = new Document();
        document2.put("age",52);

        FindIterable<Document> documents = t_user.find().filter(Filters.or(document1,document2));
        for (Document document : documents) {
            System.out.println(document);
        }
    }

    /**
     * 模糊查询
     */
    @Test
    public void testQueryLike(){
        Document document =  new Document();
        document.put("name", Pattern.compile("n",Pattern.CASE_INSENSITIVE));
        FindIterable<Document> documents = t_user.find(document);
        for (Document document1 : documents) {
            System.out.println(document1);
        }

    }


    /**
     * null值得处理
     */

    @Test
    public void testNull(){
        Document document = new Document();
        document.put("name",null);
        FindIterable<Document> documents = t_user.find(document);
        for (Document document1 : documents) {
            System.out.println(document1);
        }
    }

    /**
     *查询数组 查询地址中是北京 上海  广州的记录
     */
    @Test
    public void testAll(){
        FindIterable<Document> address = t_user.find().filter(Filters.all("address", new String[]{"beijing"}));
        for (Document document : address) {
            System.out.println(document);
        }
    }
    /**
     *查询数组 查询地址中包含 河北 的document
     */
    @Test
    public void testArray(){
        Document document1 = new Document();
        document1.put("address","hebei");
        FindIterable<Document> address = t_user.find(document1);
        for (Document document : address) {
            System.out.println(document);
        }
    }


    /**
     * 对查询结果进行排序
     */
    @Test
    public void testSort(){
        FindIterable<Document> documents = t_user.find().sort(new Document("age", -1));
        for (Document document : documents) {
            System.out.println(document);
        }
    }


    /**
     * 对查询结果进行排序 分页查询
     */
    @Test
    public void testPage(){
        FindIterable<Document> documents = t_user.find().sort(new Document("age", -1)).skip(0).limit(2);
        for (Document document : documents) {
            System.out.println(document);
        }
    }






    @After
    public void after(){
        mongoClient.close();
    }






}
```
```java
package com.yxj.test;

import com.mongodb.MongoClient;
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoDatabase;
import com.mongodb.client.model.Filters;
import org.bson.Document;
import org.bson.conversions.Bson;
import org.junit.After;
import org.junit.Before;
import org.junit.Test;

import java.util.UUID;

public class TestMongo003 {

    private MongoClient mongoClient;
    private MongoDatabase database;
    private MongoCollection<Document> t_user;

    @Before
    public void before(){
        mongoClient = new MongoClient("192.168.28.136",27017);
        database = mongoClient.getDatabase("test");
        t_user = database.getCollection("t_user");
    }

    /**
     * 批量插入
     */
    @Test
    public void testInstert(){
        for (int i = 0; i < 100; i++) {
            Document document = new Document();
            document.put("_id", UUID.randomUUID().toString());
            document.put("name", "chenyn" + i);
            document.put("age",i);
            t_user.insertOne(document);
        }
    }


    /**
     * 删除
     */
    @Test
    public void testDelete(){
        t_user.deleteMany(Filters.gt("age",23));
    }



    /**
     * 更新
     */
    @Test
    public void testUpdate(){
        Bson filters = Filters.eq("age", 12);
        t_user.updateOne(filters,new Document("$set",new Document("name","aa")));
    }

    @After
    public void after(){
        mongoClient.close();
    }






}
```
## 7 Spring Boot整合mongoDB
```markdown
1.pom配置
pom包里面添加spring-boot-starter-data-mongodb包引用
2.在application.properties中添加配置
spring.data.mongodb.uri=mongodb://192.168.42.136:27017/test
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102312125711.png#pic_center)


```java
package com.entity;

import java.util.Date;

import org.bson.types.ObjectId;
import org.springframework.data.annotation.Id;
import org.springframework.data.annotation.Transient;
import org.springframework.data.mongodb.core.index.Indexed;
import org.springframework.data.mongodb.core.mapping.Document;
import org.springframework.data.mongodb.core.mapping.Field;

@Document(collection="t_order")
public class Order {
	@Id
	@Indexed
	private ObjectId id;
	@Field("order_num")
	private String orderNum;
	private Date createDate;
	// 映射忽略的字段，该字段不会保存到mongodb。
	@Transient
	private String cc;
	
	public ObjectId getId() {
		return id;
	}
	public void setId(ObjectId id) {
		this.id = id;
	}
	public String getOrderNum() {
		return orderNum;
	}
	public void setOrderNum(String orderNum) {
		this.orderNum = orderNum;
	}
	public Date getCreateDate() {
		return createDate;
	}
	public void setCreateDate(Date createDate) {
		this.createDate = createDate;
	}
	public Order(ObjectId id, String orderNum, Date createDate) {
		super();
		this.id = id;
		this.orderNum = orderNum;
		this.createDate = createDate;
	}
	public Order() {
		super();
	}
	@Override
	public String toString() {
		return "Order [id=" + id + ", orderNum=" + orderNum + ", createDate="
				+ createDate + "]";
	}
	
}

```
```java
package com.entity;

import java.util.List;

import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.mapping.DBRef;
import org.springframework.data.mongodb.core.mapping.Document;
@Document(collection="t_person")
public class Person {
	@Id // 主键类型只能为：String，ObjectId,BigInteger
	private String id;
	private String name;
	private int age;
	private double salary;
	@DBRef
	private List<Order> orders = null;
	
	public List<Order> getOrders() {
		return orders;
	}
	public void setOrders(List<Order> orders) {
		this.orders = orders;
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
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public double getSalary() {
		return salary;
	}
	public void setSalary(double salary) {
		this.salary = salary;
	}
	public Person(String id, String name, int age, double salary) {
		
		this.id = id;
		this.name = name;
		this.age = age;
		this.salary = salary;
	}
	public Person() {
		
	}
	@Override
	public String toString() {
		return "Student [id=" + id + ", name=" + name + ", age=" + age
				+ ", salary=" + salary + "]";
	}
	
}

```
```java
package mongotest;

import com.baizhi.entity.Order;
import com.baizhi.entity.Person;
import com.mongodb.Mongo;
import org.bson.types.ObjectId;
import org.junit.Before;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.data.mongodb.core.MongoTemplate;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;

public class TestMain01 {
	
	@Autowired
	private MongoTemplate temp;
	
	
	@Test
	public void test09(){
		Person person = temp.findById("1111111111",Person.class);
		System.out.println(person.getOrders());
	}
	// 根据主键id查询
	@Test
	public void test08(){
		Person person = temp.findById("1111111111",Person.class);
		System.out.println(person.getOrders());
	}
	// 测试有级联关系的两个对象
	@Test
	public void test06(){
		Person p = new Person("1111111111","haha",23,1111);
		List<Order> list =new ArrayList<Order>();
		for(int i = 0 ; i < 3; i++){
			ObjectId objectId  = new ObjectId(new Date());
			Order o = new Order(objectId, 111111+"_"+i, new Date());
			temp.save(o);
			list.add(o);
		}
		p.setOrders(list);
		temp.save(p);
	}
	// 删除数据
	@Test
	public void test05(){
		Person s = new Person();
		s.setId(1+"");
		temp.remove(s);
	}
	// 查询person表中的所有数据
	@Test
	public void test03(){
		List<Person> list = temp.findAll(Person.class);
		for (Person s : list) {
			System.out.println(s);
		}
	}
	// 添加数据
	@Test
	public void test02() {
		/*for(int i = 0 ;  i< 5;i++){
			Student s = new Student(10+i+"", "zhangsan"+i, 12*i, 2000+i);
			temp.save(s);
		}*/
		// 主键如果存在，进行更新
		/*Student s = new Student("2", "zhangsasssssn", 111, 2000);
		temp.save(s);*/
		/**
		 * insert方法：如果key存在，报错
		 */
		Person s = new Person("2", "aa", 111, 2000);
		temp.insert(s);
	}
	// 查询所有的库
	@Test
	public void test01() {
		Mongo mongo = ac.getBean("mongo1",Mongo.class);
		List<String> list = mongo.getDatabaseNames();
		for (String dbName : list) {
			System.out.println(dbName);
		}
		
	}
}

```
```java
package mongotest;

import com.baizhi.entity.Person;
import com.mongodb.BasicDBObject;
import com.mongodb.DBObject;
import org.junit.Before;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.data.domain.Sort;
import org.springframework.data.domain.Sort.Direction;
import org.springframework.data.mongodb.core.MongoTemplate;
import org.springframework.data.mongodb.core.query.BasicUpdate;
import org.springframework.data.mongodb.core.query.Criteria;
import org.springframework.data.mongodb.core.query.Query;
import org.springframework.data.mongodb.core.query.Update;
import org.springframework.data.mongodb.core.query.Update.Position;

import java.util.List;
import java.util.UUID;
import java.util.regex.Pattern;

public class TestMain02 {
	
	@Autowired
	private MongoTemplate temp;
	
	
	@Test
	public void testAddBatch(){
		for(int i = 0 ; i<10;i++){
			String id = UUID.randomUUID().toString().replace("-", "");
			Person p = new Person(id, "zhangsan"+i, 10+i*2, 10000+i*2);
			temp.save(p);
		}
	}

	@Test  //   排序
	public void testOrder(){
		Query query = new 	Query();
		query.addCriteria(new Criteria().where("age").gt(10));
		query.limit(10);
		query.skip(2);
		// 按照salary的升序排列
//		query.with(new Sort(Direction.ASC,"salary"));
		// 按照salary的降序排列
		query.with(new Sort(Direction.DESC, "salary"));
		List<Person> list = temp.find(query, Person.class);
		
		bianli(list);
	}
	@Test  
	public void testLimitSkip(){
		Query query = new 	Query();
		query.addCriteria(new Criteria().where("age").gt(10));
		query.limit(10);
		query.skip(2);
		List<Person> list = temp.find(query, Person.class);
		bianli(list);
	}
	@Test  // 查询与数组相关的信息
	public void testQuery03(){
		Query query = new 	Query();
		Pattern pattern = Pattern.compile("^.*1.*$", Pattern.CASE_INSENSITIVE);

		query.addCriteria(Criteria.where("name").regex(pattern));
		List<Person> list = temp.find(query, Person.class);
		bianli(list);
	}
	@Test  // 查询与数组相关的信息
	public void testQuery02(){
		Query query = new 	Query();
		// 查询  age数组长度为4的数据
//		query.addCriteria(Criteria.where("age").size(4));
		// 查询 age数组第三位 为 "aa"的记录
//		query.addCriteria(Criteria.where("age.2").is("aa"));
		List<Person> list = temp.find(query, Person.class);
		bianli(list);
	}
	@Test
	public void testQuery01(){
		Query query = new 	Query();
		// 查询  age =18
		//query.addCriteria(new Criteria().where("age").is(18));
		//查询 age > 12 并且  age< 20
		//query.addCriteria(new Criteria().andOperator(new Criteria("age").gt(12),new Criteria("age").lt(20)));
		//查询  age =1  或者 age = 100
//		query.addCriteria(new Criteria().orOperator(new Criteria("age").is(1),new Criteria("age").is(1000)));
		// 查询  age  in  (1,2)
//		query.addCriteria(new Criteria().where("age").in(1,2));
		
		// 查询  age值  除以5  余数为2的数据
		query.addCriteria(new Criteria("age").mod(5,2));
		List<Person> list = temp.find(query, Person.class);
		bianli(list);
	}
	public void bianli(List<Person> list){
		for (Object o : list) {
			System.out.println(o);
		}
	}
	@Test
	public void testUpdate04(){
		Query query = new 	Query();
		query.addCriteria(new Criteria().where("key1").is("hello 1"));
		Update update = new Update();
		update.set("key1", "value1");
		update.pushAll("age",new Object[]{12,"aa",13});
		temp.updateFirst(query,update, Person.class);
	}
	@Test
	public void testUpdate03(){
		Query query = new 	Query();
		query.addCriteria(new Criteria("age").gt(26).lt(30));
		// 用新元素替换旧元素
		DBObject obj = new BasicDBObject("key1", "value1");
		Update update = new BasicUpdate(obj);
		temp.updateFirst(query,update, Person.class);
	}
	@Test
	public void testUpdate02(){
		Query query = new 	Query();
		query.addCriteria(new Criteria("age").gt(26).lt(30));
		// 用新元素替换旧元素
		DBObject obj = new BasicDBObject("key1", "value1");
		Update update = new BasicUpdate(obj);
		temp.updateFirst(query,update, Person.class);
	}
	@Test
	public void testUpdate01(){
		Query query = new 	Query();
		query.addCriteria(new Criteria("age").gt(26).lt(30));
		// 在原始旧数据的基础上进行修改
		Update update = new Update();
		update.set("name","helloe zhangsan"); //根据key，对对应的值进行更新
		update.inc("salary", 3);  // 将该key对于的值进行递增
		update.pull("optKey",new String[]{"optionValue1","optionValue2"});//只对数组有效。 将新数据压入到key对应的数组中
		update.pop("optKey", Position.FIRST); // 只能对数组有用。将数组特定位置的元素移除
		
		temp.updateFirst(query,update, Person.class);
	}
	
}

```
## 8 MongoDB中的索引
```markdown
索引就是为了加速查询的,MongoDB的索引几乎与传统的关
系型数据库一模一样，这其中也包括一些基本的优化技巧。
下面是创建索引的命令：
```


```markdown
1.	创建索引:db.集合名称.ensureIndex({"name":1}) 
a)	1 代表索引升序存储  -1 代表索引降序存储
b)	_id 默认自动创建索引
2.	创建索引指定索引名称:db.集合名称.ensureIndex({"name":1},{name:"name_index"}) 
3.	查看索引是否创建成功:db.集合名称.getIndexes()
4.	删除索引的命令:db.集合名称.dropIndex({"name":1});
5.	创建复合索引:db.集合名称.ensureIndex({"name":1, "age":-1,bir:1})
a)	注意: 
i.	该索引被创建后，基于name和age的查询将会用到该索引
ii.	或者是基于name的查询也会用到该索引
iii.	但是只是基于age的查询将不会用到该复合索引。
b)	总结: 
i.	如果想用到复合索引，必须在查询条件中包含复合索引中的前N个
索引列。然而如果查询条件中的键值顺序和复合索引中的创建顺序
不一致的话，MongoDB可以智能的帮助我们调整该顺序，以便使
复合索引可以为查询所用。

ii.	如：db.t_user.find({"age": 30, "name": "stephen"})
对于上面示例中的查询条件，MongoDB在检索之前将会
动态的调整查询条件文档的顺序，以使该查询可以用到刚
刚创建的复合索引。

6.	创建唯一索引:db.t_user.ensureIndex({"name":1},{"unique":true})
注意: 在缺省情况下创建的索引均不是唯一索引。一旦创建唯一索引, 
如果再次插入name重复的文档时，MongoDB将报错，以提示插入重
复键 (exception: E11000 duplicate key error index: zpark.t_user.$name_1 dup key: { : \"xiaohei\" })
7.	重建索引:db.集合名称.reIndex(); 

```

## 9 mongoDB中的主从复制(4.0版本废弃) 
```markdown
主从复制是mongoDB最常用的复制方式,这种方式非常灵活,可用
于备份,故障恢复和扩展等.因为已经废弃，所以这里不展开
```


## 10 MongoDB中的副本集
```markdown
MongoDB 副本集（Replica Set）是有自动故障恢复功能的
主从集群，有一个Primary节点和一个或多个Secondary节点组
成。主从集群和副本集之间最明显的区别就是副本集没有固
定的”主节点”,
整个集群会选举一个主节点,当主节点不能工作时自动变更到其他节点.
```
```markdown
1 配置三个文件
mongodb27017.conf 

port=27017 #端口  
dbpath=/usr/local/mongodb/data/db27017  #数据库存文件存放目录  
logpath=/usr/local/mongodb/log/mongodb27017.log #日志文件存放路径  
#logappend=true #使用追加的方式写日志  
fork=true #不以守护程序的方式启用，即不在后台运行  
maxConns=100 #最大同时连接数  
noauth=true #不启用验证
#auth=true  
journal=true #每次写入会记录一条操作日志（通过journal可以重新构造出写入的数据）。
#即使宕机，启动时wiredtiger会先将数据恢复到最近一次的checkpoint点，然后重放后续的journal日志来恢复。
storageEngine=wiredTiger  #存储引擎有mmapv1、wiretiger、mongorocks
bind_ip = 0.0.0.0  #这样就可外部访问了，例如从win10中去连虚拟机中的MongoDB
replSet=rs  # 副本集名称

mongodb27018.conf 

port=27018 #端口  
dbpath=/usr/local/mongodb/data/db27018  #数据库存文件存放目录  
logpath=/usr/local/mongodb/log/mongodb27018.log #日志文件存放路径  
#logappend=true #使用追加的方式写日志  
fork=true #不以守护程序的方式启用，即不在后台运行  
maxConns=100 #最大同时连接数  
noauth=true #不启用验证
#auth=true  
journal=true #每次写入会记录一条操作日志（通过journal可以重新构造出写入的数据）。
#即使宕机，启动时wiredtiger会先将数据恢复到最近一次的checkpoint点，然后重放后续的journal日志来恢复。
storageEngine=wiredTiger  #存储引擎有mmapv1、wiretiger、mongorocks
bind_ip = 0.0.0.0  #这样就可外部访问了，例如从win10中去连虚拟机中的MongoDB
replSet=rs

mongodb27019.conf 

port=27019 #端口  
dbpath=/usr/local/mongodb/data/db27019  #数据库存文件存放目录  
logpath=/usr/local/mongodb/log/mongodb27019.log #日志文件存放路径  
#logappend=true #使用追加的方式写日志  
fork=true #不以守护程序的方式启用，即不在后台运行  
maxConns=100 #最大同时连接数  
noauth=true #不启用验证
#auth=true  
journal=true #每次写入会记录一条操作日志（通过journal可以重新构造出写入的数据）。
#即使宕机，启动时wiredtiger会先将数据恢复到最近一次的checkpoint点，然后重放后续的journal日志来恢复。
storageEngine=wiredTiger  #存储引擎有mmapv1、wiretiger、mongorocks
bind_ip = 0.0.0.0  #这样就可外部访问了，例如从win10中去连虚拟机中的MongoDB
replSet=rs


2. 创建目录
dbpath=/usr/local/mongodb/data/db27017 # 没有db27017目录则启动不起来
dbpath=/usr/local/mongodb/data/db27018  # 没有db27018目录则启动不起来
dbpath=/usr/local/mongodb/data/db27019 # 没有db27019目录则启动不起来
logpath=/usr/local/mongodb/log/  # 创建log目录
3. 启动服务
./mongod --config /usr/local/mongodb/mongodb27017.conf
./mongod --config /usr/local/mongodb/mongodb27018.conf
./mongod --config /usr/local/mongodb/mongodb27019.conf

4.  连接任意一台mongo
i.	use admin
ii.	执行如下命令 # 我是在一台机器上进行的，所以ip地址一样。如果不同，修改ip即可
	var config = { 
	 	_id:"rs", 
	 	members:[
	   	{_id:0,host:"192.168.42.136:27017"},
	   	{_id:1,host:"192.168.42.136:27018"},
	   	{_id:2,host:"192.168.42.136:27019"}]
	 }
	 
iii.	rs.initiate(config);//初始化配置

c)	设置客户端临时访问数据:rs.secondaryOk();  # 得执行这个命令，否则从节点不能访问数据
```

## 11 SpringBoot操作MongoDB中的副本集
```markdown
spring.data.mongodb.uri=mongodb://192.168.42.136.1:27017,192.168.42.136:27018,192.168.42.136:27019/ems?replcaSet=rs(副本集名称)
```
## 12 Mongodb中的分片
```markdown
1.	分片(sharding)
分片目的是通过分片能够增加更多机器来应对不断的增加负载和数
据,还不影响应用.
分片(sharding)是指将数据拆分,将其分散存在不同机器的过程,有时
也用分区(partitioning)来表示这个概念,将数据分散在不同的机器
上,不需要功能强大的大型计算机就能存储更多的数据,处理更大的负载.
MongoDB支持自动分片,可以摆脱手动分片的管理困扰,集群自动
切分数据做负载均衡.MongoDB分片的基本思想就是将集合拆分成
多个块,这些快分散在若干个片里,每个片只负责总数据的一部
分,应用程序不必知道哪些片对应哪些数据,甚至不需要知道数
据拆分了,所以在分片之前会运行一个路由进程,mongos进程,这
个路由器知道所有的数据存放位置,应用只需要直接与mongos
交互即可,mongos自动将请求转到相应的片上获取数据.从应
用角度看分不分片没有什么区别.

2.	什么时候分片
a)	机器磁盘不够用了
b)	单个的mongo已经不能满足写数据的性能需要了
```

```markdown
3.mongoDB的分片架构图
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/202010231229087.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201023122923566.png#pic_center)

```markdown
4.	片键
设置分片时需要在集合中选一个键,用该键的值作为拆分数据的依
据,这个片键称之为(shard key)
注意: 在真正的生产环境中,片键的选取很重要,片键的选取要一定要
数据散列均匀

5.	搭建分片集群(我采用的是伪分布式，也就是在一台机器上运行)
a)	分片结构的端口如下
s1-Shard Server 1：27017
s2-Shard Server 2：27018
s3-Shard Server 3：27019
s4-Shard Server 4：27020
s5-Config Server5 ：27021
s6-Config Server6 ：27022
s7-Config Server7 ：27023
s8-Route Process8：27024
b)	创建数据目录
/usr/local/mongodb/data/27017         
/usr/local/mongodb/data/27018
/usr/local/mongodb/data/27019
/usr/local/mongodb/data/27020
/usr/local/mongodb/data/27021
/usr/local/mongodb/data/27022
/usr/local/mongodb/data/27023
c) 创建片键服务器配置文件
/usr/local/mongodb/config/s1.conf

port=27017 #端口  
dbpath=/usr/local/mongodb/data/27017  #数据库存文件存放目录  
logpath=/usr/local/mongodb/log/s1.log #日志文件存放路径  
logappend=true #使用追加的方式写日志  
fork=true #不以守护程序的方式启用，即不在后台运行  
maxConns=100 #最大同时连接数  
noauth=true #不启用验证  
journal=true #每次写入会记录一条操作日志（通过journal可以重新构造出写入的数据）。
#即使宕机，启动时wiredtiger会先将数据恢复到最近一次的checkpoint点，然后重放后续的journal日志来恢复。
storageEngine=wiredTiger  #存储引擎有mmapv1、wiretiger、mongorocks
bind_ip = 0.0.0.0  #这样就可外部访问了，例如从win10中去连虚拟机中的
shardsvr=true

/usr/local/mongodb/config/s2.conf

port=27018 #端口  
dbpath=/usr/local/mongodb/data/27018  #数据库存文件存放目录  
logpath=/usr/local/mongodb/log/s2.log #日志文件存放路径  
logappend=true #使用追加的方式写日志  
fork=true #不以守护程序的方式启用，即不在后台运行  
maxConns=100 #最大同时连接数  
noauth=true #不启用验证  
journal=true #每次写入会记录一条操作日志（通过journal可以重新构造出写入的数据）。
#即使宕机，启动时wiredtiger会先将数据恢复到最近一次的checkpoint点，然后重放后续的journal日志来恢复。
storageEngine=wiredTiger  #存储引擎有mmapv1、wiretiger、mongorocks
bind_ip = 0.0.0.0  #这样就可外部访问了，例如从win10中去连虚拟机中的
shardsvr=true

/usr/local/mongodb/config/s3.conf

port=27019 #端口  
dbpath=/usr/local/mongodb/data/27019  #数据库存文件存放目录  
logpath=/usr/local/mongodb/log/s3.log #日志文件存放路径  
logappend=true #使用追加的方式写日志  
fork=true #不以守护程序的方式启用，即不在后台运行  
maxConns=100 #最大同时连接数  
noauth=true #不启用验证  
journal=true #每次写入会记录一条操作日志（通过journal可以重新构造出写入的数据）。
#即使宕机，启动时wiredtiger会先将数据恢复到最近一次的checkpoint点，然后重放后续的journal日志来恢复。
storageEngine=wiredTiger  #存储引擎有mmapv1、wiretiger、mongorocks
bind_ip = 0.0.0.0  #这样就可外部访问了，例如从win10中去连虚拟机中的
shardsvr=true

/usr/local/mongodb/config/s4.conf

port=27020 #端口  
dbpath=/usr/local/mongodb/data/27020  #数据库存文件存放目录  
logpath=/usr/local/mongodb/log/s4.log #日志文件存放路径  
logappend=true #使用追加的方式写日志  
fork=true #不以守护程序的方式启用，即不在后台运行  
maxConns=100 #最大同时连接数  
noauth=true #不启用验证  
journal=true #每次写入会记录一条操作日志（通过journal可以重新构造出写入的数据）。
#即使宕机，启动时wiredtiger会先将数据恢复到最近一次的checkpoint点，然后重放后续的journal日志来恢复。
storageEngine=wiredTiger  #存储引擎有mmapv1、wiretiger、mongorocks
bind_ip = 0.0.0.0  #这样就可外部访问了，例如从win10中去连虚拟机中的
shardsvr=true

d)创建Config Server配置文件
/usr/local/mongodb/config/s5.conf
port=27021 #端口  
dbpath=/usr/local/mongodb/data/27021  #数据库存文件存放目录  
logpath=/usr/local/mongodb/log/s5.log #日志文件存放路径  
logappend=true #使用追加的方式写日志  
fork=true #不以守护程序的方式启用，即不在后台运行  
maxConns=100 #最大同时连接数  
noauth=true #不启用验证  
journal=true #每次写入会记录一条操作日志（通过journal可以重新构造出写入的数据）。
#即使宕机，启动时wiredtiger会先将数据恢复到最近一次的checkpoint点，然后重放后续的journal日志来恢复。
storageEngine=wiredTiger  #存储引擎有mmapv1、wiretiger、mongorocks
bind_ip = 0.0.0.0  #这样就可外部访问了，例如从win10中去连虚拟机中的
configsvr=true
replSet=config #  config为上面的副本集名称

/usr/local/mongodb/config/s6.conf

port=27022 #端口  
dbpath=/usr/local/mongodb/data/27022  #数据库存文件存放目录  
logpath=/usr/local/mongodb/log/s6.log #日志文件存放路径  
logappend=true #使用追加的方式写日志  
fork=true #不以守护程序的方式启用，即不在后台运行  
maxConns=100 #最大同时连接数  
noauth=true #不启用验证  
journal=true #每次写入会记录一条操作日志（通过journal可以重新构造出写入的数据）。
#即使宕机，启动时wiredtiger会先将数据恢复到最近一次的checkpoint点，然后重放后续的journal日志来恢复。
storageEngine=wiredTiger  #存储引擎有mmapv1、wiretiger、mongorocks
bind_ip = 0.0.0.0  #这样就可外部访问了，例如从win10中去连虚拟机中的
configsvr=true
replSet=config

/usr/local/mongodb/config/s7.conf

port=27023 #端口  
dbpath=/usr/local/mongodb/data/27023  #数据库存文件存放目录  
logpath=/usr/local/mongodb/log/s7.log #日志文件存放路径  
logappend=true #使用追加的方式写日志  
fork=true #不以守护程序的方式启用，即不在后台运行  
maxConns=100 #最大同时连接数  
noauth=true #不启用验证  
journal=true #每次写入会记录一条操作日志（通过journal可以重新构造出写入的数据）。
#即使宕机，启动时wiredtiger会先将数据恢复到最近一次的checkpoint点，然后重放后续的journal日志来恢复。
storageEngine=wiredTiger  #存储引擎有mmapv1、wiretiger、mongorocks
bind_ip = 0.0.0.0  #这样就可外部访问了，例如从win10中去连虚拟机中的
configsvr=true
replSet=config

e) 创建路由配置信息
s8.conf

port=27024 #端口  
logpath=/usr/local/mongodb/log/s8.log #日志文件存放路径  
logappend=true #使用追加的方式写日志  
fork=true #不以守护程序的方式启用，即不在后台运行  
maxConns=100 #最大同时连接数  
noauth=true #不启用验证  
bind_ip = 0.0.0.0  #这样就可外部访问了，例如从win10中去连虚拟机中的
configdb=config/192.168.42.136:27021,192.168.42.136:27022,192.168.42.136:27023

f) 创建日志目录
/usr/local/mongodb/log

g) 启动片键服务器
./mongod --config /usr/local/mongodb/config/s1.conf
./mongod --config /usr/local/mongodb/config/s2.conf
./mongod --config /usr/local/mongodb/config/s3.conf
./mongod --config /usr/local/mongodb/config/s4.conf
h) 启动config服务器
/mongod --config /usr/local/mongodb/config/s5.conf
/mongod --config /usr/local/mongodb/config/s6.conf
/mongod --config /usr/local/mongodb/config/s7.conf

i)	初始化config的配置服务器副本集
i.	登录任意config的server节点中使用 use admin 
ii.	在admin中执行
var config = { 
	 	_id:"config",
		configsvr:true, 
	 	members:[
	   	{_id:0,host:"192.168.42.136:27021"},
	   	{_id:1,host:"192.168.42.136:27022"},
	   	{_id:2,host:"192.168.42.136:27023"}]
	 }
iii.初始化副本集配置 rs.initiate(config);

j) 启动路由
./mongos --config /usr/local/mongodb/config/s8.conf 

h)客户端登陆到mongos中 mongo --port 27024
i.	use admin
ii.	添加分片节点: 
db.runCommand({ addshard:"192.168.42.136:27017","allowLocal":true });
db.runCommand({ addshard:"192.168.42.136:27018","allowLocal":true });
db.runCommand({ addshard:"192.168.42.136:27019","allowLocal":true });
db.runCommand({ addshard:"192.168.42.136:27020","allowLocal":true });
iii. 设置分片的库:db.runCommand({ enablesharding:"yxj" });
iiii. 设置那个库中哪个集合以及片键信息:
db.runCommand({ shardcollection: "yxj.users", key: { _id:1}})  # 对yxj库的users表的id字段进行分片。如果这个yxj库不存在。会自动创建。这个没有进行哈希处理。可能分片不均匀
db.runCommand({ shardcollection: "yxj.emps", key: { _id: "hashed"}}) # 对id进行哈希处理

i) 对库的增删查改对操作路由表即可实现
```
## 13 SpringBoot操作分片
```markdown
spring.data.mongodb.uri=mongodb://192.168.42.136:27024
```


## 14 图片总结 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210110202828194.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210110202840484.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210110202854162.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210110202912246.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210110203119135.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210110203142794.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210110203225612.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210110203252479.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021011020333039.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210110203352648.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210110203512605.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210110203551649.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210110203628225.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210110203650242.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210110203706465.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复modb
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
