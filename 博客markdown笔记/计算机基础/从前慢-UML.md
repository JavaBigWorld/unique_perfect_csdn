# UML
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210703132222887.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1 UML与面向对象
```markdown
在结构化开发方法中：
      系统分析的建模语言是数据流图，系统设计的建模语言
      是模块结构图。

在面向对象开发方法中:
       系统分析与设计的建模语言是UML

UML仅仅是一种语言。它不是一种系统设计的方法，而是系统建模的标准。
```
### 1.1 UML视图
```markdown
用例视图
     用例是系统中的一个功能单元。用例视图是其他视图的核心，
     从系统外部参与者的角度描述系统应该具有的功能。

逻辑视图
     设计人员和开发人员使用逻辑视图来描述用例视图提出的系
     统功能的实现，包括静态结构（类图、对象图）和动态协作
     关系（状态图、时序图、协作图、活动图）。


并发视图
      开发人员和系统集成人员考虑资源的有效利用、代码的并行
      执行和系统环境中的异步事件的处理而使用的图形，包括状
      态图、协作图、活动图。
组件视图
      组件是不同类型的代码模块，是构造应用的软件单元；组件
      视图是开发人员用于描述系统的实现模块以及它们之间的依
      赖关系的图。
部署视图
       开发人员、系统集成人员和测试人员使用的。用来显示系
       统的物理部署，描述了位于节点上的运行实例的部署情况。
```
### 1.2 UML图
```markdown
UML1.x提供了九种不同的图，可以分为两大类：
静态图：用例图、类图、对象图、组件图和部署图；
动态图：序列图、协作图、状态图和活动图。

UML2.0又增加了：静态图-（包图、组成结构图）
、动态图-（交互纵览图、计时图），总共13种图。
```
## 2 用例图
~~~markdown
用例图有参与者、用例、系统、关系组成
用例需要用动宾词语表示，用例图用来描述需求场景。参与者实际上就是一个类
用例通常用普通正文描述，也可用活动图来描述。
~~~
### 2.1 用例的描述
```markdown
用例通常用正文来描述,也可用活动图来描述 。

用例的正文描述应包括以下内容：
1 名称、标识符、参与者、状态

2 用例的目的：用例的最终目的是什么？它试图达到什么？
用例是如何启动的（前置条件）：哪个执行者在什么情况
下启动用例的执行？

3 执行者和用例之间的消息流（基本操作流程）：用例与执
行者之间交换什么消息或事件来通知对方改变或恢复信
息？描述系统与执行者之间的主消息流是什么？以及系
统中哪些实体被使用或修改？

4 用例中可供选择的流（可选操作流程）：用例中的活动可
根据条件或异常（exception）有选择地执行。

5 如何通过给执行者一个值来结束用例（后置条件）：描
述何时可认为用例已结束.


补充：事件流（flow of events）-
事件流是一系列陈述句，它是从执行者的角度 看，列出用例的各个步骤
。用例描述中可以包含条件、分支和循环。


事件流可分为两部分：
基本路径
基本路径是运转正常时的路径，是一系列没有分支和选择的简单陈述句
可选路径
可选路径是指不同于基本路径而允许不同的事件序列的路径。
对于明显有可能随时发生的事情来说，可选路径非常有效。


总结：
描述用例包括:前置条件(前提条件)/事件流(操作步骤:基本路径/可选路径
(非正常情况))/后置条件
```
### 2.2 实  例 一
```markdown
实  例 一
执行者的简要描述
      客户：向公司订购商品的人
      客户代表：公司处理客户请求的雇员
      库存系统：记录公司库存的软件
用例的简要描述
      订购货物：客户创建一个新的请求商品的订单，并为那些商品付费
      取消订单：客户取消一个已经存在的订单


例如：订购货物用例的描述如下所示。
用例名称：订购货物
参与的执行者：客户、客户代表
前置条件：一个合法的客户已经登录到这个系统
事件流：
1.当客户选择订购货物时，用例开始
2.客户输入他的姓名和地址
3.如果客户只输入邮编，系统将给出州和城市名
4.当客户输入产品代码
	a.系统给出产品描述和价格
	b.系统往客户订单中添加该物品的价格
    循环结束
5.客户输入信用卡支付信息
6. 客户选择提交
7.系统检验输入的信息，把该订单作为未完成的交易保存，
同时向记账系统转发支付信息。如果客户提交的信息不正
确，系统将提示客户修改。
8.当支付确认后，订单就被标记上已经确认，同时返回给
客户一个订单ID，用例也就结束了。如果支付没有被确
认，系统将提示客户改正支付信息或者取消。如果客户
选择修改信息，就回到第5步；如果选择取消，用例结束。
后置条件：如果订单没有被取消，它将保存在系统中，
并做上标记；如果订单取消则不保存。

这里也可以将事件流分出来（建议这样做）：
订购货物用例的基本路径和可选路径：
基本路径
7. 当客户选择订购货物时，用例开始
8. 客户输入他的姓名和地址
9. 当客户输入产品代码时
	a. 系统给出产品描述和价格
	b. 系统往客户订单中添加该物品的价格
    循环结束
10. 客户输入信用卡支付信息
11. 客户选择提交
12. 系统检验输入的信息，把该订单作为未完成的交易保存，同时
13. 向记账系统转发支付信息
14. 当支付确认后，订单就被标记上已经确认，同时返回给客户一个
15. 订单ID，用例结束


如果在订购货物用例中，客户可以在提交订单前随时取消订单，其
可选路径如下：
可选路径：
在选择提交前的任何时候，客户都可选择cancel。这次订购没有被
保存，用例结束。
在基本路径第6步，如果有任何不正确的信息，系统提示客户去修
改这些信息。
在基本路径第7步，若支付没有被确认，系统将提示客户改正支付
信息或者取消。若客户选择修改信息，就回到基本路径第4步；若
选择取消，用例结束。
```
### 2.3 实 例 二

```markdown
本案例实现一个简化了的银行储蓄账户管理系统，该系统是在银行的柜
台上对客户办理活期储蓄业务。系统的需求陈述如下：
一个客户可以在多个银行中开设账户，一个客户也可在同一银行中开
设多个不同的账户。客户可以通过银行职员进行开户、存款、取款、
转账、注销账户等活动。其中转账指客户将自己的某个账户上的钱款
转入同一银行的不同账户（称为银行内转账）或转入不同银行
的账户（称为银行间转账）。系统管理员负责系统的账户管理
及业务报表的生成。


识别执行者
客户：到银行办理储蓄业务的人，负责输入密码
银行职员（客户代理）：银行工作人员，代表客户进行储蓄业务的操作
银行职员（管理人员）：银行工作人员，根据客户的储蓄业务更新账户
管理员：银行计算机的管理人员，负责账户的管理和业务报表的生成


识别用例
从系统的需求陈述可知，银行职员(客户代理)需要系统提
供开户、存款、取款、转账、注销账户等功能，这些功能都
包含了校验密码的功能。系统管理员需要系统提供账户管理和
报表生成功能。银行职员(管理人员)则参与了账户管理中的更新
账户的功能。此外，转账功能可分为银行内转账和银行间转账，
可将它们设计成三个用例，其中银行内转账用例和银行间转账
用例都继承了基本转账用例。据此分析，得到该系统的
用例图如下图所示。

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210629092622853.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
开户用例描述
用例名称：开户
参与的执行者：银行职员（客户代理），客户
前置条件：一合法的银行职员（客户代理）已登录到该系统
事件流：
1.当选择开户功能时用例开始
2.输入客户信息（姓名、地址、身份证号等）
3.从账户管理系统获取新的账号
4.请客户输入密码
5.请客户再次输入密码
6.如果两次密码不一致则回到第4步，否则继续
7.在账户库中添加新账户
8.打印存折，用例结束
后置条件：在账户库中增加了一个新账户，得到一张新存折


取款用例描述
用例名称：取款
参与的执行者：银行职员（客户代理）
前置条件：一合法的银行职员(客户代理)已登录到该系统
事件流：
基本路径：
1.当选择取款功能时用例开始
2.当输入客户信息（姓名、账号等）后
  a）如果客户信息与账户不一致，显示错误信息，可以重新
  输入或结束用例
  b）如果该账户被冻结（如因挂失而冻结），显示冻结信息
  并结束用例
3.输入并校验密码
4.输入取款金额，若该账户的余款小于取款金额，显示错误信
息，要求重新输入
5.打印取款单，交客户签字
6.建立取款事件记录，更新账户信息
7. 打印存折，用例结束
可选路径：
1.在第5步客户签字之前的任何时刻，客户可以取消本次取款，
用例结束
2.第3步校验密码时，如发现密码不一致，则重新输入密码，
或用例结束
后置条件：如果取款成功，客户账户中的余额被更新（减少），
否则余额不变。
```
### 2.4 用例之间的关系
~~~markdown     
关联关系： 执行者与用例之间的关系，用 —————表示

                                          
包含关系：表示A需要用到B，需要把B给用进去（一般是把公共部分提
              《include》
取出来），用A------------>B表示；（缺少了B，则A则不完整）

                                                《extend》
扩展关系：表示A的功能可以再扩展，可以用B来补充，用B---------->A
表示（缺少了B，A仍然完整）

泛化关系:表示所谓的继承关系,比如A是子类,B是父类,则A————▷B表示


总结：
泛化：同一业务目的的不同技术实现

包含：提取公共交互，提高复用

扩展：“冻结”基用例以保持稳定，并且动态扩展基用例功能
~~~
#### 2.4.1 包含举例
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210702230324412.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210702230330796.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

#### 2.4.2 扩展举例
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210702230440476.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210702230445616.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 2.4.3 包含用例与扩展用例的区别
```markdown
1 相对于基础用例，扩展用例是可选的，而包含用例则不是。
2 如果缺少扩展用例，基础用例还是完整的，而缺少包含用例，
则基础用例就不完整了。
3 扩展用例的执行需要满足某种条件，而包含用例不需要。
4 扩展用例的执行会改变基础用例的行为，而包含用例不会.
```
## 3 类图、对象图和包图

### 3.1 类图
```markdown
在面向对象的处理中，类图处于核心地位，它提供了用于定义
和使用对象的主要规则。
类图是正向工程（将模型转化为代码）的主要资源，是逆向
工程（将代码转化为模型）的生成物。

类名必须大写
属性跟操作名必须小写。
UML规范采用3个预定义分栏的图标表示类，分栏中包含的信
息有：名称、属性和操作表示。
```
### 3.2 类和对象的表示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210702231427317.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210702221929706.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200408151033508.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020040815110279.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200408151126686.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200408151220314.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020040815125670.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200408151346833.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 3.3 类之间的关系
![ ](https://img-blog.csdnimg.cn/20200626162114718.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 3.3.1 关联关系
```markdown
用一条无向线段表示，是一种双向关系。例如客户和订
单的关联:从客户看,订单是他提交的;从订单看,它
有一个客户。
用一条有向线段表示,是一种单向关系.

关联的命名：可以用动词词组或名词命名。但只要这个关联的含义
明确，则可省略这个名字。
```
##### 3.3.1.1 角色的多元性
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210702232537569.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 3.3.1.2 关联关系实例
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200402140749867.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
3![在这里插入图片描述](https://img-blog.csdnimg.cn/20200402141659523.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

##### 3.3.1.3 聚合关系
~~~markdown
聚合关系描述的是整体和部分的关系.聚合关系是比较特殊的关联关
系，比如：一个教室当中有多个学生，教室和学生之间的关系就是整
体和部分的关系，在聚合关系中，整体的生命周期不会决定部分的生
命周期,例如：教室没了,学生还在,或者说学生走了,教室还在.
~~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200402142927327.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 3.3.1.4 组合关系
~~~markdown
组合关系可以看做是一种特殊的聚合关系,整体的生命周期决定部分
的生命周期,部分是依附在整体上面的,部分离开了整体是无法“存活的”。
~~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210702233704506.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 3.3.2 泛化关系
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200402135955989.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 3.3.3 实现关系
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200402140222441.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 3.3.4 依赖关系
~~~markdown
依赖关系是所有关系中最弱的一种这种关系。通常体现在类和局部变
量之间的关系.
~~~

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200402143928808.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

### 3.4 对象图、包图
#### 3.4.1 对象图
```markdown
对象图中的建模元素有对象和链.对象是类的实例,对象之间的链是
类之间的关联的实例,对象图实质上是类图的实例

UML中对象图的图标也是一个矩形，和类的图标一样，但是对象名下面
要带下划线。在左边的这个图标中，具体实例的名字位于冒号的左边，
而该实例所属的类名位于冒号的右边。实例的名字以一个小写字母开头。
也可以是一个匿名的对象，如图右边所示。这仅仅意味着指明了对象所属
的类，但并没有提供一个具体的对象名。

对象之间的关系，被称为链

对象是类的实例，链是关联的实例

对象的基本特征可以归纳为对象的属性和行为两类，对象名必须小写
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210702234832224.png)
#### 3.4.2 包图
```markdown
可类比java中的包机制
包图只有依赖关系

当一个包将另一个包导入时，该包里的元素能够使用被导入包里的元
素，而不必在使用时通过包名指定其中的元素。
例如，当使用某个包中的类时，如果未将包导入，则需要使用包名加
类名的形式引用指定的类。在导入关系中，被导入的包称为目标包。
要在UML中显示导入关系，需要画一条从包连接到目标包的依赖性箭
头，再加上字符import，如图所示。

A包导入B包，则可以直接使用B包中的类。如果没有的话，则是B.类表示
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210702235237173.png)
## 4 活动图
```markdown
活动图本质是上是一种流程图，但是活动图是面向对象的，而流程图是面
向过程的。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070300061596.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 4.1 活动图与流程图的区别
```markdown
流程图着重描述处理过程，它的主要控制结构是顺序、分支和循环，
各个处理之间有严格的顺序和时间关系；
活动图描述的则是对象活动的顺序关系所遵循的规则，它着重表现的
是系统的行为，而非系统的处理过程。
活动图能够表示并发活动的情形；
流程图做不到。
活动图是面向对象的；
流程图是面向过程的。
```
### 4.2 活动图实例
```markdown
用户登录系统的活动图
输入网址、显示主页、输入登录信息、验证
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210703003346374.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
教师上传课件的活动图
上传文件、验证容量、存储文件、管理员授权、更新网页
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070300335761.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
系统管理员维护网站的活动图
登录、更新信息、处理CAI、保存
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070300340677.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
## 5 顺序图(跟协作图，也叫做通信图等价)
```markdown
顺序图用来建模以时间顺序安排的对象交互，并且把用例行为分配给类。
顺序图与活动图具有类似的作用。其中重要的理由就是实现用例。任何用
例都可以使用顺序图进一步阐明和实现。

顺序图主要有：对象、生命线、消息和激活。
顺序图有两个主要的标记符：活动对象和这些活动对象之间的通信消息。
活动对象可以是任何在系统中扮演角色的对象，不管它是对象实例还是
参与者，如下图所示。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200402152532645.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200402152635676.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200402152749245.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200402152934645.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 5.1 顺序图和协作图的区别
```markdown
顺序图
    它描述对象按时间顺序的消息交换过程，它体现出系统用例的行为。
协作图
    它描述对象间的组织协作关系，它也可体现出系统用例的行为。


顺序图和协作图都可以表示对象间的交互关系，但它们的侧重点不同。
顺序图用消息的几何排列关系来表达对象间交互消息的先后时间顺序。
而协作图用建模对象（或角色）间的通信关系。
```
### 5.2 用例图、类图、顺序图对比
```markdown
用例图描述了系统必须做什么（有什么功能）；类图描述了组成系统结
构各部分的各种类型。顺序图描述了对象之间传递消息的时间顺序，
它用来表示用例中的行为顺序。
顺序图是来描述对象的，所以所有顺序图中所有活动对象都要在类图
能找到。
用例跟用例之间不能直接通信。必须通过对象作为中间介进行通信。
```
## 6 协作图
```markdown
与顺序图等价
```
## 7 时序图
```markdown
时序图侧重于描述时间对系统交互的影响，因此时序图的一个重要的特
征是加入了时间元素。 时序图上的时间由左到右横跨页面。
```
## 8 状态图
```markdown
一般不去描述参与者类对象的状态。要描述的是系统内部的
核心对象的状态。

需求过程,需要描述一个对象的状态跟踪的时候,比如一个表
单在不同环节审批的状态;
状态图描述的是一个对象
状态图就是一个开关,是描述状态变化的图形;
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200625110633539.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 8.1 活动图与状态图
```markdown
活动图描述的是用例的行为，而状态图描述的是对象。活动图可
以包含多个对象。而状态图只可以包含一个对象。
```
## 9 构造实现方式图
```markdown
包括有组件图跟部署图
```
### 9.1 组件图
```markdown
组件图显示软件组件的组织以及组件之间的依赖关系，包
括源代码组件、二进制代码组件以及可执行组件。
包括组件、接口、依赖关系
```
#### 9.1.1 组件图实例
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210703012904810.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210703012921670.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 9.1.2 组件与类的比较
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210703013352816.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

### 9.2 部署图
```markdown
组件图用来建模软件组件，而部署图用来对部署系统时涉及到
的硬件进行建模。
节点表示一种硬件。组件表示逻辑元素的物理包装，即类的
物理包装，而节点表示组件的物理配置。
节点的种类：处理器跟设备。
处理器是能够执行软件、具有计算能力的节点。设备是没有计
算能力的节点，通常情况下都是通过其接口为外部提供某种服务。
```
#### 9.2.1 部署图实例
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210703014700742.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)



## 10  UML与数据库设计
```markdown
依赖关系强调的是类操作间的使用关系，类图到表结构的映射中并不涉及这种关系，所以下面只讨论泛化关系、关联关系到表的映射规范。

不需要画ER图，因为已经有类图了
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210703015747626.png)

### 10.1 泛化关系的映射
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210703015920422.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210703015938194.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210703015949228.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210703020001214.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210703020018442.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210703020031298.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 10.2 关联关系的映射
```markdown
关联关系：一对一关联、一对多关联和多对多关联。
```
#### 10.2.1 一对一映射
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210703020227869.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 10.2.2 一对多映射
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210703020319451.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 10.2.3 多对多映射
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210703020349288.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210703020433900.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 10.3 数据库设计原则
```markdown
每一个类成为一个数据库表。
关系映射：
一对多的关系映射为数据库表的主外键关联（1方的主键加入n方成为外键）
一对一的关系映射为数据库表的主外键关联（1方的 主键加入另一方成为外键）
多对多的关系映射：产生第三张表，将两个多方的主键加入其中成为外键，两
个外键的组合成为主键。
利用数据库三范式检查表，从而考察类图的分析是否合理，消除冗余数据。
检查数据是否能够反映用例视图的需要；进一步与用户再次确认使用的数据。
```
## 11 毕设
```markdown
必须：毕业设计是只要是面向对象，就一定要有用例图、类图、顺序图、组件图、部署图。
灵活采用：活动图、状态图
选用，可有可无：计时图、对象图、包图、组成结构图、交互概览图。
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复UML
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

