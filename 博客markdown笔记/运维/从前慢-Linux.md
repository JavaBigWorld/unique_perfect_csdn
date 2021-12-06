# Linux
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210713002512189.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1 Linux的引言
```markdown
Linux是一套免费使用和自由传播的类Unix操作系统，
是一个基于POSIX和Unix的多用户、多任务、支持
多线程和多CPU的操作系统。伴随着互联网的发展，
Linux得到了来自全世界软件爱好者、组织、公司的
支持。它除了在服务器操作系统方面保持着强劲
的发展势头以外，在个人电脑、嵌入式系统上都有
着长足的进步。目前Linux存在着许多不同的Linux
发行版本，但它们都使用了Linux内核。Linux可安
装在各种计算机硬件设备中，比如手机、平板电脑、
路由器、台式计算机。
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020154251486.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 2 Linux的诞生
```markdown
20世纪80年代，计算机硬件的性能不断提高，PC的市场不断扩大，
当时可供计算机选用的操作系统主要有Unix、DOS和MacOS这几
种。Unix价格昂贵，不能运行于[
PC](https://baike.baidu.com/item/PC/107)；
[DOS](https://baike.baidu.com/item/DOS/32025)显得简陋，
且源代码被软件厂商严格保密；
[MacOS](https://baike.baidu.com/item/MacOS/8654551)
是一种专门用于苹果计算机的操作系统。此时，计算机科
学领域迫切需要一个更加完善、强大、廉价和完全开放的
操作系统。由于供教学使用的典型操作系统很少，因此当
时在荷兰当教授的美国人AndrewS.Tanenbaum编写了一
个操作系统，名为
[MINIX](https://baike.baidu.com/item/MINIX/7106045)，
为了向学生讲述操作系统内部工作原理。MINIX虽然很好，
但只是一个用于教学目的的简单操作系统，而不是一个强
有力的实用操作系统，然而最大的好处就是公开源代码。
全世界学计算机的学生都通过钻研MINIX源代码来了解电
脑里运行的MINIX操作系统，芬兰赫尔辛基大学大学二年
级的学生Linus Torvalds就是其中一个，在吸收了MINIX
精华的基础上，Linus于1991年写出了属于自己的Linux
操作系统，版本为Linux0.01，是Linux时代开始的标志。
他利用Unix的核心，去除繁杂的核心程序，改写成适用
于一般计算机的x86系统，并放在网络上供大家下载，
1994年推出完整的核心Version1.0，至此，Linux逐渐
成为功能完善、稳定的操作系统，并被广泛使用。

总结:Linux出现于1991年，是由芬兰赫尔辛基大学学生
,Linus Torvalds和后来加入的众多爱好者共同开发完成。
```





![image-20191011201019566.png](https://img-blog.csdnimg.cn/20201020154346581.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 3 Linux的特点

### 3.1 完全免费
```markdown
Linux是一款免费的操作系统，用户可以通过网络或其他途
径免费获得，并可以任意修改其[源代码]
(https://baike.baidu.com/item/源代码/3969)。
这是其他的操作系统所做不到的。正是由于这
一点，来自全世界的无数[程序员]
(https://baike.baidu.com/item/程序员/62748)
参与了Linux的修改、编写工作，程序员可以根据自己的
兴趣和灵感对其进行改变，这让Linux吸收了无数程序员的
精华，不断壮大。
```
### 3.2 多用户、多任务
```markdown
Linux支持多用户，各个用户对于自己的文件设备有自己特
殊的权利，保证了各用户之间互不影响。多任务则是现在
电脑最主要的一个特点，Linux可以使多个程序同时并独立
地运行。同时丰富的网络功能，可靠的系统安全，良好的
可移植性，具有标准兼容性，出色的速度性能。
```




## 4 Linux之Cent OS

### 4.1 centos 引言
```markdown
CentOS（Community Enterprise Operating System，中文意
思是社区企业操作系统）是Linux发行版之一，它是来自于
Red Hat Enterprise Linux依照
[开放源代码](https://baike.baidu.com/item/开放源代码/114160)
规定释出的源代码所编译而成。由于出自同样的[源代码](https://baike.baidu.com/item/源代码/3587471)，
因此有些要求高度稳定性的
[服务器](https://baike.baidu.com/item/服务器/100571)
以CentOS替代商业版的[Red Hat](https://baike.baidu.com/item/Red Hat) 
Enterprise Linux使用。两者的不同，在于CentOS完全开源。
```


### 4.2 centos 和 readheat区别
```markdown
目前的Linux操作系统主要应用于生产环境，
主流企业的Linux系统仍旧是RedHat或者
CentOS,他们出自于同样的源代码,但centos
完全免费。其独有的yum命令支持在线升级，
可以即时更新系统，不像RedHat 那样需要花钱购买支持服务！
```



## 5 Linux中目录结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020203612579.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
# 目录结构	
	bin  (binaries)存放二进制可执行文件																									 [重点]
	sbin  (super user binaries)存放二进制可执行文件，只有root才能访问
	etc (etcetera)存放系统配置文件																											[重点]
	usr  (unix shared resources)用于存放共享的系统资源  																	[重点]
	home 存放用户文件的根目录																														 [重点]
	root  超级用户目录																															   [重点]
	dev (devices)用于存放设备文件
	lib  (library)存放跟文件系统中的程序运行所需要的共享库及内核模块
	mnt  (mount)系统管理员安装临时文件系统的安装点
	boot 存放用于系统引导时使用的各种文件
	tmp  (temporary)用于存放各种临时文件																							   [重点]
	var  (variable)用于存放运行时需要改变数据的文件
```
## 6 Linux中常用指令
### 6.1 cd命令
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200919193928504.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 6.2 cp命令
```markdown
-i	覆盖文件前提示
-r复制目录
-p保留文件属性
cp -rp  [原文件或目录]  [目标目录]
cp  文件名    目录   复制文件到指定目录中
cp  -r 目录名    目录   复制指定目录到指定目录中
-r 递归处理，将指定目录下的文件与子目录一并拷贝（recursive）     
```
### 6.3 cat 命令
```markdown
-b	对非空输出行编号
-n	对输出的所有行编号
```
### 6.4 chown/chgrp/chmod
```markdown
Linux文件有三种典型的权限，即r读权限、w写权限和x执行权限。在长格式输出
中在文件类型的后面有9列权限位，实际上这是针对不同用户而设定的。r=4，
w=2，x=1
# chmod 
注意:rwx对目录跟文件的含义不一样,对于目录而
言,r表示可以列出目录内容,w表示可以删除/创建文件,
x表示可以进入目录,对于文件而言,r表示可以查看文件,
w表示可以修改文件,x表示可以执行文件.
all = user + group + other 
chmod  u+rwx,g+rwx,o+rwx   文件名
字母法：chmod u/g/o/a +/-/= rwx 文件

[ u/g/o/a ]	含义
u	user 					表示该文件的所有者
g	group 				表示与该文件的所有者属于同一组( group )者，即用户组
o	other 				表示其他以外的人
a	all 					表示这三者皆是

[ +-= ]	含义
+								增加权限
-								撤销权限
=								设定权限

rwx	含义
r		read 表示可读取，对于一个目录，如果没有r权限，那么就意味着不能通过ls查看这个目录的内容。
w		write 表示可写入，对于一个目录，如果没有w权限，那么就意味着不能在目录下创建新的文件。
x		excute 表示可执行，对于一个目录，如果没有x权限，那么就意味着不能通过cd进入这个目录。

数字法:   4读 2写  1执行
chmod 777 文件名

chown 用户名 文件名|目录名   #  修改文件|目录的拥有者
chgrp -R 组名 文件名|目录名   # 递归修改文件|目录的组
chmod -R 755 文件名|目录名  # 递归修改文件权限
```
### 6.5 crond任务调度
```markdown
minute hour day month week command   # * * * * * 
minute： 表示分钟，可以是从0到59之间的任何整数。
hour：表示小时，可以是从0到23之间的任何整数。
day：表示日期，可以是从1到31之间的任何整数。
month：表示月份，可以是从1到12之间的任何整数。
week：表示星期几，可以是从0到7之间的任何整数，这里的0或7代表星期日。
command：要执行的命令，可以是系统命令，也可以是自己编写的脚本文件。

星号（*）：代表所有可能的值，例如month字段如果是星号，则表示在满足其它字段的制约条件后每月都执行该命令操作。
逗号（,）：可以用逗号隔开的值指定一个列表范围，例如，“1,2,5,7,8,9”
中杠（-）：可以用整数之间的中杠表示一个整数范围，例如“2-6”表示“2,3,4,5,6”
正斜线（/）：可以用正斜线指定时间的间隔频率，例如“0-23/2”表示每两小时执行一次。同时正斜线可以和星号一起使用，例如*/10，如果用在minute字段，表示每十分钟执行一次。

每小时的每分钟执行  ls –l  /etc/  > /tmp/to.txt 命令
1)	crontab -e
2)	*/1  * * * *  ls -l  /etc >> /tmp/to.txt
3)	当保存退出后就程序。
4)	在每一分钟都会自动的调用 ls -l /etc >> /tmp/to.txt
-e  编辑crontab定时任务
-l  查询crontab任务
-r 删除当前用户所有的crontab任务
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201021202526457.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 6.6 chkconfig 指令
```markdown
通过chkconfig 命令可以给每个服务的各个运行级
别设置自启动/关闭
```
```markdown
1)	查看服务 chkconfig	--list|grep	xxx
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201021232012957.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201021232022316.png#pic_center)
```markdown
2)	chkconfig	服务名	--list
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201021232047201.png#pic_center)
```markdown
3)	chkconfig	--level	5	服务名	on/off
请将 sshd 服务在运行级别为	5 的情况下，不要自启动。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201021232132986.png#pic_center)
```markdown
1)	案例 1： 请显示当前系统所有服务的各个运行级别的运行状态
bash> chkconfig --list
2)	案例 2 ：请查看 sshd 服务的运行状态
bash> service sshd status
3)	案例 3： 将 sshd 服务在运行级别 5 下设置为不自动启动，看看有什么效果？
bash> chkconfig --level 5 sshd off
4)	案例 4： 当运行级别为 5 时，关闭防火墙。
bash> chkconfig	--level 5	iptables off
5)	案例 5： 在所有运行级别下，关闭防火墙
bash> chkconfig	iptables off
6)	案例 6： 在所有运行级别下，开启防火墙
bash> chkconfig	iptables	on	
7)	chkconfig 重新设置服务后自启动或关闭，需要重启机器 reboot 才能生效.
```
### 6.7 date/cal
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200919210939300.png#pic_center)
```markdown
基本语法
1)	date	（功能描述：显示当前时间）
2)	date     "+%Y"（功能描述：显示当前年份）
3)	date     "+%m" 	（功能描述：显示当前月份）
4)	date     "+%d"  （功能描述：显示当前是哪一天）
5)	date "+%Y-%m-%d %H:%M:%S"（功能描述：显示年月日时分秒）

date	-s	字符串时间 # 设置日期
date	-s "2018-10-10 11:30:30"

```
### 6.8 df/du
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200919211016236.png#pic_center)
#### 6.8.1 查询系统整体磁盘使用情况
```markdown
df -h # 查询系统整体磁盘使用情况
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201021225442284.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
#### 6.8.2 查询指定目录的磁盘占用情况
```markdown
du -h	/目录  # 查询指定目录的磁盘占用情况，默认为当前目录
-s 指定目录占用大小汇总
-h 带计量单位
-a 含文件
--max-depth=1	子目录深度
 -c 列出明细的同时，增加汇总值

查询 /opt 目录的磁盘占用情况，深度为 1
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201021230118957.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
1)	统计/home 文件夹下文件的个数
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201021230357803.png#pic_center)
```markdown
2)	统计/home 文件夹下目录的个数
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102123043849.png#pic_center)
```markdown
3)	统计/home 文件夹下文件的个数，包括子文件夹里的
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201021230512907.png#pic_center)
```markdown
4)	统计文件夹下目录的个数，包括子文件夹里的
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201021230723960.png#pic_center)
```markdown
5)	以树状显示目录结构
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201021230744131.png#pic_center)


### 6.9 echo/>/>>/wc
```markdown
标准输出重定向
命令 > 文件 以覆盖的方式，把命令的正确输出输出到指定的文件或设备当中
命令>>文件 以追加的方式，把命令的正确输出输出到指定的文件或设备当中。
标准错误输出重定
错误命令2> 以覆盖的方式，把命令的文件错误输出输出到指定的文件或设备当中
错误命令2>文件 以追加的方式，把命令的错误输出输出到指定的文件或设备当中。

正确输出和错误输出同时保存    
命令 &> 文件 以覆盖的方式，把正确输出和错误输出都保存到同个文件当中。

命令 &>> 文件 以追加的方式，把正确输出和错误输出都保存到同个文件当中。

命令 >> 文件1 2>> 文件2  把正确的输出追加到文件1中，把错误的输出追加到文件2中。

输入重定向wc
[root@localhost ~]# wc [选项] [文件名]
选项:
-c 统计字节数
-w 统计单词数
-l 统计行数

多命令顺序执行
多命令执行符      格式             作用
；              命令1；命令2       多个命令顺序执行，命令之间没有任何逻辑联系
&&              命令1&&命令2       逻辑与
                                  当命令1正确执行，则命令2才会执行
                                  当命令1执行不正确，则命令2不会执行
||               命令1||命令2      逻辑或，当命令1执行不正确，则命令2才会执行
                                   当命令1正确执行，则命令2不会执行
```
### 6.10 find命令
```markdown
语法：find  [搜索范围]  [匹配条件]
功能描述：文件搜索
#  find /tmp  -name  init
```


### 6.11 grep 命令
```markdown
Linux 系统中 grep 命令是一种强大的文本搜索工具
grep允许对文本文件进行 模式查找，所谓模式查找，
又被称为正则表达式
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200919200729611.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 6.12 help命令
```markdown
功能描述：获得Shell内置命令的帮助信息
范例： $ help cd
```
### 6.13 head 指令
```markdown
head 用于显示文件的开头部分内容，默认情况下 head 
指令显示文件的前 10 行内容
基本语法
head  文件	(功能描述：查看文件头 10 行内容)
head -n 5 文件	(功能描述：查看文件头 5 行内容，5 可以是任意行数)
```
### 6.14 history 指令
```markdown
history	（功能描述：查看已经执行过历史命令）  # 显示所有的历史命令
history 10 # 显示最近使用过的 10 个指令。 
！5 # 执行历史编号为 5 的指令
```
### 6.15 id/who/whoami
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200919203739974.png#pic_center)
### 6.16  kill和   killall
```markdown
若是某个进程执行一半需要停止时，或是已消了很大的系统资源时，
此时可以考虑停止该进程。
使用 kill命令来完成此项任务。

kill   [选项]进程号（功能描述：通过进程号杀死进程）
killall进程名称（功能描述：通过进程名称杀死进程，也支持通配符，这在系统因负载过大而变
得很慢时很有用）
常用选项：

-9 :表示强迫进程立即停止
最佳实践：
踢掉某个非法登录用户
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200913221716493.png#pic_center)
```markdown
终止远程登录服务  sshd,在适当时候再次重启   sshd服务
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200913221826851.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
终止多个  gedit编辑器【killall,通过进程名称来终止进程】
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200913223320545.png#pic_center)
```markdown
强制杀掉一个终端
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020091322344215.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
# 10.head、tail查看文本中开头或结尾部分的内容
		head  -n  5  a.log 查看a.log文件的前5行
# 11.tail  -f  b.log 循环读取（fellow）
# 12.echo 输出命令
echo   I love baby		说明:用来向屏幕输出一句话
echo I Love baby  >>  aa.txt	说明:将这段内容输入到 文件中
```
### 6.17 ls 命令
```markdown
# ls  显示文件和目录列表	(list)

常用参数:  
-a 显示所有文件，包括隐藏文件
-l 详细信息显示
-d 查看目录属性
-h 人性化显示大小
-i 查看节点
-R   递归显示指定目录下的文件清单，即会显示指定目录分支内各子目录中的文件清单。

ls 通配符的使用
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200919193658368.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```shell
# 0. ls (查看目录下文件和目录)
[root@localhost ~]# ls
aa  aa.txt

# 1. ls -l (长格式展示文件)
[root@localhost ~]# ls -l
总用量 60
drwxrwxr-x      2   user1  user1    4096    Aug 17 09:10 abc
- rw- r-- r--    1   user1  user1    17     Aug 17 09:04 host.conf
- rw- r-- r--    1   user1  user1    38450  Aug 17 09:04 php.ini

长格式含义:
文件类型 文件权限   链接数  属主    属组    大小   日期   时间    文件名
d     rwxrwxr-x     2     user1  user1 4096  Aug 17 09:10  abc

# 2. ls -a (显示所有文件)
[root@localhost ~]# ls -a
.   aa      .bash_history  .bash_profile  .cshrc  .tcshrc
..  aa.txt  .bash_logout   .bashrc        .pki    .viminfo

# 3. ls -R (递归显示文件)
[root@localhost ~]# ls -R
.:
aa  aa.txt

./aa:
```
### 6.18 ln
```markdown
ln
命令英文原意：link
语法：ln-s [原文件] [目标文件 ]# s创建软链接
功能描述：生成链接文件
源文件要使用绝对路径，不能使用相对路径，这样
可以方便移动链接文件后，仍然能够正常使用
范例
$ ln -s /etc/issue  /tmp/issue.soft # 创建文件/etc/issue的软链接/tmp/issue.soft
$ ln /etc/issue /tmp/issue.hard # 创建文件/etc/issue的硬链接tmp/issue.hard
软链接特征：类似Windows快捷方式

```
### 6.19 last、lastlog命令
```markdown
last
语法:last
功能描述:列出目前与过去登入系统的用户信息

lastlog
语法： lastlog
功能描述:检查某特定用户上次登录的时间
```
### 6.20 mkdir命令
```markdown
如果文件 不存在，可以创建一个空白文件
如果文件 已经存在，可以修改文件的末次修改日期
-p	可以递归创建目录
-p 父目录不存在情况下先生成父目录 （parents）       
```


### 6.21 mv命令
```markdown
 -i	覆盖文件前提示
mv 文件名    新文件名      文件改名
1.mv aa.txt bb.txt # 把aa.txt改为bb.txt
mv 文件名    目录名     	文件移动
2.mv aa.txt   bb # 其中那个bb是已存在的目录，则aa.txt被移动到bb目录中
mv 目录名    不存在目录名  目录改名   
3.mv aa bb # 其中bb是不存在的目录，则aa目录被改名为bb
mv 目录名	  已存在目录名  目录移动
4 mv aa bb # 其中bb是已存在的目录，则aa目录被移动到bb目录中.
```
### 6.22 more、less命令
```markdown
-b	对非空输出行编号
-n	对输出的所有行编号
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200919195501327.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 6.23 man命令
```markdown
功能描述：获得帮助信息
范例： $ man ls
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200919213729913.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 6.24 mail命令
```markdown
mail
语法：mai [用户名]
功能描述：查看发送电子邮件
```


### 6.25 pwd命令
```markdown
# 1.pwd 显示当前工作目录（print working directory）
```
### 6.26 ping/ip/netstat
```markdown
# 1.ip addr 查看IP地址
ip addr 查看IP地址
ip a  简化写法
# 2.ping 测试网络连通性
ping 192.168.0.1
ifconfig	  # configure a network interface	 查看/配置计算机当前的网卡配置信息
127.0.0.1 被称为 本地回环/环回地址，一般用来测试本机网卡是否正常

# 3.netstat
语法： netstat [选项]
功能描述：显示网络相关信息
选项
-t：TCP协议
-u:UDP协议
-l:监听
-r:路由
-n:显示IP地址和端口号
范例:
netstat -tlun  查看本机监听的端口
netstat -an 查看本机所有的网络连接
netstat -rn  查看本机路由表
```
### 6.27 ps命令
```markdown
进程管理
1)在 LINUX中，每个执行的程序（代码）都称为一个进程。
每一个进程都分配一个 ID号。
2)每一个进程，都会对应一个父进程，而这个父进程可以
复制多个子进程。例如 www服务器。
3)每个进程都可能以两种方式存在的。前台与后台，所谓前
台进程就是用户目前的屏幕上可以进
行操作的。后台进程则是实际在操作，但由于屏幕上无法
看到的进程，通常使用后台方式执行。
4)一般系统的服务都是以后台进程的方式存在，而且都会常
驻在系统中。直到关机才才结束。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200913220306807.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
ps指令详解
1)指令：ps   –aux|grep  xxx，比如我看看有没有   sshd服务
2)指令说明
System V展示风格
USER：用户名称
PID：进程号
%CPU：进程占用 CPU的百分比
%MEM：进程占用物理内存的百分比
VSZ：进程占用的虚拟内存大小（单位：KB）
RSS：进程占用的物理内存大小（单位：KB）
TT：终端名称,缩写  .
STAT：进程状态，其中 S-睡眠，s-表示该进程是会话的先导进程，N-表示进程拥有比普通优先
级更低的优先级，R-正在运行，D-短期等待，Z-僵死进程，T-被跟踪或者被停止等等
STARTED：进程的启动时间
TIME：CPU时间，即进程使用  CPU的总时间
COMMAND：启动进程所用的命令和参数，如果过长会被截断显示
以全格式显示当前所有的进程，查看进程的父进程。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200913220704796.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
ps -ef是以全格式显示当前所有的进程
-e显示所有进程。-f全格式。
ps -ef|grep xxx
是 BSD风格
UID：用户  ID
PID：进程  ID
PPID：父进程  ID
C：CPU用于计算执行优先级的因子。数值越大，表明进程是  CPU密集型运算，执行优先级会
降低；数值越小，表明进程是 I/O密集型运算，执行优先级会提高
STIME：进程启动的时间
TTY：完整的终端名称
TIME：CPU时间
CMD：启动进程所用的命令和参数
```
```markdown
如果我们希望查看  sshd进程的父进程号是多少，应该怎样查询
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200913221105935.png#pic_center)
### 6.28  pstree命令
```markdown
查看进程树   pstree
pstree [选项] ,可以更加直观的来看进程信息
-p :显示进程的  PID
-u :显示进程的所属用户
```
### 6.29 rm命令
```markdown
 功能描述：删除文件
-r删除目录  # 删除目录是时需要这个选项，不删除目录时不需要
-f强制执行 # 此选项表示不会提醒，直接删除
```
### 6.30 rmdir命令
```markdown
范例：$ rmdir /tmp/yxj
```	
### 6.31 r!
```markdown
导入命令执行结果：r！命令
```
### 6.32 rpm/yum/apt
```markdown
RPM命令
RPM是RedHat Package Manager（RedHat软件包管理工具）
的缩写，这一文件格式名称虽然打上了RedHat的标志，但是
其原始设计理念是开放式的，现在包括RedHat、CentOS、
SUSE等Linux的分发版本都有采用，可以算是公认的行业
标准了。RPM文件在Linux系统中的安装最为简便,
```


```markdown
# rpm 命令  
	常用参数:
      -i：安装应用程序（install）
      -e：卸载应用程序（erase）
      -v :(verbose) 显示详细信息
      -h :(hash) 显示进度
       -q  (query) 查询包是否安装
       -R 查询软件包的依赖性(requires)
       -p查询未安装包信息(package)
       -f查询系统文件属于哪个软件包（file）
       -l 列表 (list)
      U：升级软件包；（update） 
      -qa: 显示所有已安装软件包（query all）
      
[root@localhost ~]#rpm -q 包名 #查询包是否安装
[root@localhost ~]# rpm -qa  # 查询所有已经安装RPM包
[root@localhost ~]#rpm -qR 包名  # 查询软件包的依赖性
[root@localhost ~]#rpm -qi 包名  # 查询软件包详细信息
[root@localhost ~]# rpm -ql 包名 # 查询包中文件安装位置
[root@localhost ~]#rpm -qf 系统文件名  # 查询系统文件属于哪个RPM包

rpm -ivh  xxxx.rpm  # 安装
rpm -evh  xxxx.rpm  # 卸载
rpm -Uvh  xxx.rpm  # 升级

```

```markdown
YUM命令

Yum（全称为 Yellow dog Updater, Modified）是一个
在Fedora和RedHat以及SUSE、CentOS中的Shell前
端软件包管理器。基於RPM包管理，能够从指定的服
务器自动下载RPM包并且安装，可以自动处理依赖性
关系，并且一次安装所有依赖的软件包，无须繁琐地一次次下载、安装。

例子：
    yum  install  gcc-c++
    yum  remove   gcc-c++
    yum  update   gcc-c++
    
    yum install|remove|update  依赖名称
[root@localhost yum.repos.d]# yum list #查询所有可用软件包列表
[root@localhost yum.repos.d]# yum search 关键字 # 搜索服务器上所有和关键字相关的包
```
```markdown
apt
apt 是 Advanced Packaging Tool，是 Linux 下的一款安装包管理工具
可以在终端中方便的 安装／卸载／更新软件包
安装软件
$ sudo apt install 软件包
 卸载软件
$ sudo apt remove 软件名
 更新已安装的包
$ sudo apt upgrade 
```


### 6.33  runlevel
```markdown
查看或者修改默认级别：   vi /etc/inittab
Linux系统有  7种运行级别(runlevel)：常用的是级别 3和   5
运行级别 0：系统停机状态，系统默认运行级别不能设为 0，否则不能正常启动
运行级别 1：单用户工作状态，root权限，用于系统维护，禁止远程登陆
运行级别 2：多用户状态(没有 NFS)，不支持网络
运行级别 3：完全的多用户状态(有 NFS)，登陆后进入控制台命令行模式
运行级别 4：系统未使用，保留
运行级别 5：X11控制台，登陆后进入图形 GUI模式
运行级别 6：系统正常关闭并重启，默认运行级别不能设为 6，否则不能正常启动
```

### 6.34 systemctl 服务命令
```markdown
# service管理指令：
service 服务名     [start | stop | restart | reload | status]
在 CentOS7.0后不再使用   service ,而是  systemctl
# systemctl 服务命令
systemctl status|start|stop|restart 服务名  mysqld firewalld(防火墙) network(网络)
	systemctl status 服务名          说明:查看某个服务的运行状态
	systemctl start 服务名 					说明:启动某个服务
	systemctl restart 服务名 				说明:重启某个服务
	systemctl stop 服务名 						说明:停止某个服务
查看当前防火墙的状况，关闭防火墙和重启防火墙。	
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200913230457381.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 6.35 shutdown/logout
```markdown
shutdown
重新启动操作系统，其中 now 表示现在
shutdown -r now
立刻关机，其中 now 表示现在
shutdown now
系统在今天的 20:25 会关机
shutdown 20:25
系统再过十分钟后自动关机
shutdown +10
取消之前指定的关机计划
shutdown -c
当我们关机或者重启时，都应该先执行sync指令，
把内存的数据写入磁盘，防止数据丢失
其他关机命令
[root@localhost~]# halt
[root@localhost~]# poweroff
[root(@localhost~]# init 0
reboot
[root@localhost~]# reboot
[root@localhost~]# init 6
系统运行级别
0关机
1单用户
2不完全多用户，不含NFS服务
3完全多用户
4未分配
5图形界面
6重启
logout
[root@localhost ~]# logout
查看或配置网卡信息ifconfig/ping

```
### 6.36 ssh/scp
```markdown
SSH 客户端的简单使用
ssh [-p port]  user@remote
user 是在远程机器上的用户名，如果不指定的话默认为当前用户
remote 是远程机器的地址，可以是 IP／域名，或者是 后面会提到的别名
port 是 SSH Server 监听的端口，如果不指定，就为默认值 22
使用 exit 退出当前用户的登录
scp
scp 就是 secure copy，是一个在 Linux 下用来进行 远程拷贝文件 的命令
它的地址格式与 ssh 基本相同，需要注意的是，在指定端口时用的是大写的 -P 而不是小写的
把本地当前目录下的 01.py 文件 复制到 远程 家目录下的 Desktop/01.py
注意：: 后面的路径如果不是绝对路径，则以用户的家目录作为参照路径
scp -P port 01.py user@remote:Desktop/01.py
把远程 家目录下的 Desktop/01.py 文件 复制到 本地当前目录下的 01.py
scp -P port user@remote:Desktop/01.py 01.py
加上 -r 选项可以传送文件夹
把当前目录下的 demo 文件夹 复制到 远程 家目录下的 Desktop
scp -r demo user@remote:Desktop
把远程 家目录下的 Desktop 复制到 当前目录下的 demo 文件夹
scp -r user@remote:Desktop demo
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200919202349962.png#pic_center)


### 6.37 su

```markdown
切换用户身份su
[root@localhos ~]#su [选项] 用户名
选项：
-:选项只使用“-”代表连带用户的环境变量一起切换
-c命令:仅执行一次命令，而不切换用户身份

[lamp@localhost ~]su - root #切换成oot
[lamp@localhost]$ su - root -c "useradd user3" # 不切换成oot，但是执行useradd命令添加uer1用户
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200919210256256.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 6.38 source命令
```markdown
source命令
[root@localhost ~]# source 配置文件  #  source /etc/profile
```
### 6.39 touch命令
```markdown
如果文件 不存在，可以创建一个空白文件
如果文件 已经存在，可以修改文件的末次修改日期
-p	可以递归创建目录
```



### 6.40 tar命令
```markdown
Windows 常用 rar
Mac 常用 zip
Linux 常用 tar.gz
打包 ／ 解包
tar 是 Linux 中最常用的 备份工具，此命令可以 把一系列文件 打包到 一个大文件中，也可以把一个 打包的大文件恢复成一系列文件
tar 的命令格式如下：
 打包文件
tar -cvf 打包文件.    tar 被打包的文件／路径...
解包文件
tar -xvf 打包文件.tar
tar 选项说明
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200919212855883.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
压缩／解压缩
压缩文件
tar -zcvf 打包文件.tar.gz 被压缩的文件／路径...
 解压缩文件
tar -zxvf 打包文件.tar.gz
解压缩到指定路径
tar -zxvf 打包文件.tar.gz -C 目标路径
-C 解压缩到指定目录，注意：要解压缩的目录必须存在
bzip2
压缩文件
tar -jcvf 打包文件.tar.bz2  被压缩的文件／路径...
解压缩文件
tar -jxvf  打包文件.tar.bz2
```

### 6.41 top命令
```markdown
top	命令  动态显示系统进程
top与ps命令很相似。它们都用来显示正在执行的进程。Top与ps最大的不同之处，在于  top在执行一段时间可以更新正在运行的的进程。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200913235827849.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
案例 1.监视特定用户
top：输入此命令，按回车键，查看执行的进程。
u：然后输入“u”回车，再输入用户名，即可
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914000005812.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
案例 2：终止指定的进程。
top：输入此命令，按回车键，查看执行的进程。
k：然后输入“k”回车，再输入要结束的进程 ID号
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914000107315.png#pic_center)
```markdown
案例 3:指定系统状态更新的时间(每隔 10秒自动更新，默认是   3秒)：
bash> top -d 10
```

### 6.42 tree 命令
```markdown
tree 命令可以以树状图列出文件目录结构
-d	只显示目录
```
### 6.43 traceroute命令
 ```markdown
 语法： traceroute 网址
功能描述：显示数据包到主机间的路径
范例：#traceroute www.lampbrother.net
 ```
 
### 6.44	tail指令
```markdown
tail 用于输出文件中尾部的内容，默认情况下 tail 指令显示文件的后 10 行内容。
基本语法
1)	tail	文件	（功能描述：查看文件后 10 行内容）
2)	tail	-n 5 文件	（功能描述：查看文件后 5 行内容，5 可以是任意行数）
3)	tail	-f	文件	（功能描述：实时追踪该文档的所有更新，工作经常使用）

```
### 6.45 useradd/passwd/userdel
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200919203625134.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 6.46 usermod命令
```markdown
# 修改用户的主组（passwd 中的 GID）
usermod -g 组 用户名
# 修改用户的附加组
usermod -G 组 用户名
# 修改用户登录 Shell
usermod -s  /bin/bash  用户名
注意：默认使用 useradd 添加的用户是没有权限使用 sudo 以 root 身份执行命令的，可以使用以下命令，将用户添加到 sudo 附加组中
usermod -G sudo 用户名
```


### 6.47 which命令
```markdown
功能描述：搜索命令所在目录及别名信息
```


### 6.48 whereis命令
```markdown
功能描述：搜索命令所在目录及帮助文档路径
范例：$ whereis ls
```
### 6.49 w命令
```markdown
功能描述：查看登录用户详细信息
```
### 6.50 write命令
```markdown
write
语法：write <用户名>
功能描述：给用户发信息，以Ctrl+D保存结束
```
### 6.51 wall命令
 ```markdown
 语法：wall message
功能描述：发广播信息
 ```
### 6.52 正则表达式与通配符
 ```markdown
正则表达式与通配符
正则表达式用来在文件中匹配符合条件的字符串，正则是包含匹配。
grep/awk/sed等命令可以支持正则表达式。
通配符用来匹配符合条件的文件名，通配符是完全匹配。ls、find、cp这些命令不
支持正则表达式，所以只能使用shell自己的通配符来进行匹配了。
* 前一个字符匹配0次，或任意多次
.匹配除了换行符外任意一个字符
“^”匹配行首，“$”匹配行尾
“[]”匹配中括号中指定的任意一个字符，只匹配一个字符
“[^]”匹配除中括号的字符以外的任意一个字符
“\{n\}”表示其前面的字符恰好出现n次
“{n，\}” 表示其前面的字符出现不小于n次
“{n，m\}”匹配其前面的字符至少出现n次，最多出现m次
 ```


### 6.53 分区
```markdown
硬盘说明
1)	Linux 硬盘分 IDE 硬盘和 SCSI 硬盘，目前基本上是 SCSI 硬盘
2)	对于 IDE 硬盘，驱动器标识符为“hdx~”,其中“hd”表明分
区所在设备的类型，这里是指 IDE 硬盘了。“x”为盘号
（a 为基本盘，b 为基本从属盘，c 为辅助主盘，d 为
辅助从属盘）,“~”代表分区， 前四个分区用数字 1 到 4 
表示，它们是主分区或扩展分区，从 5 开始就是逻辑
分区。例，hda3 表示为第一个IDE 硬盘上的 第三个主
分区或扩展分区,hdb2 表示为第二个IDE 硬盘上的 第
二个主分区或扩展分区。
3)	对于 SCSI 硬盘则标识为“sdx~”，SCSI 硬盘是用“sd”来表
示分区所在设备的类型的，其余则和 IDE 硬盘的表示方法一样。
```
```markdown
使用lsblk 指令查看当前系统的分区情况
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201021223321335.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201021223351603.png#pic_center)
```markdown
挂硬盘
1)	虚拟机添加硬盘
2)	分区 	fdisk /dev/sdb
3)	格式化	mkfs	-t ext4	/dev/sdb1
4)	挂载	先创建一个 /home/newdisk	,  挂载 mount	/dev/sdb1	/home/newdisk
5)	设置可以自动挂载(永久挂载，当你重启系统，仍然可以挂载到 /home/newdisk) 。
vim	/etc/fstab
/dev/sdb1	/home/newdisk	ext4	defaults	0 0
6)加完成后 执行 mount	–a 即刻生效

# 卸载
umount	设备名称 或者	挂载目录
umount	/dev/sdb1 或者 umount	/newdisk
 

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201021224917101.png#pic_center)


### 6.54 脚本的常用执行方式
```markdown
赋予执行权限，直接运行
chmod 755 hello.sh
 ./hello. sh
通过Bash调用执行脚本
bash hello.sh

1)	脚本以#!/bin/bash 开头
2)	脚本需要有可执行权限
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201021233018777.png#pic_center)
#### 6.54.1 变量

```markdown
系统变量和用户自定义变量。
系统变量：$HOME、$PWD、$SHELL、$USER
显示当前 shell 中所有变量：set
基本语法
1)定义变量：变量=值 
2)撤销变量：unset 变量
3) 声明静态变量：readonly 变量，注意：不能 unset

定义变量的规则
1)	变量名称可以由字母、数字和下划线组成，但是不能以数字开头。
2)	等号两侧不能有空格
3)	变量名称一般习惯为大写

将命令的返回值赋给变量
1）	A=`ls -la` 反引号，运行里面的命令，并把结果返回给变量 A
2）	A=$(ls -la) 等价于反引号

设置环境变量
1)	export 变量名=变量值 （功能描述：将 shell 变量输出为环境变量）
2)	source  配置文件	（功能描述：让修改后的配置信息立即生效）  # source /etc/profile
3)	echo $变量名	（功能描述：查询环境变量的值）

位置参数变量
基本语法
$n （功能描述：n 为数字，$0 代表命令本身，$1-$9 代表第一到第九个参数，十以上的参数，十以上的参数需要用大括号包含，如${10}）
$* （功能描述：这个变量代表命令行中所有的参数，$*把所有的参数看成一个整体）
$@（功能描述：这个变量也代表命令行中所有的参数，不过$@把每个参数区分对待）
$#（功能描述：这个变量代表命令行中所有参数的个数）
```
```markdown
位置参数变量应用实例
编写一个 shell 脚本 positionPara.sh  ， 在脚本中获取到命令行的各个参数信息
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022100232531.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022100252939.png#pic_center)
```markdown
预定义变量
基本语法
$$ （功能描述：当前进程的进程号（PID））
$! （功能描述：后台运行的最后一个进程的进程号（PID））
$？ （功能描述：最后一次执行的命令的返回状态。如果这个变量的值为 0，证明上一个命令正确执行；如果这个变量的值为非 0（具体是哪个数，由命令自己来决定），则证明上一个命令执行不正确了。）
```
#### 6.54.2 运算符
```markdown
1) “$((运算式))”或“$[运算式]”
2)	expr m + n
注意 expr 运算符间要有空格
3)	expr m - n
4)	expr	\*, /, %	乘，除，取余

```
#### 6.54.3 条件判断
```markdown
1)	两个整数的比较
= 字符串比较
-lt 小于
-le 小于等于
-eq 等于
-gt 大于
-ge 大于等于
-ne 不等于
2)	按照文件权限进行判断
-r 有读的权限 [ -r 文件 ]
-w 有写的权限
-x 有执行的权限
3)	按照文件类型进行判断
-f 文件存在并且是一个常规的文件
-e 文件存在
-d 文件存在并是一个目录

```
#### 6.54.4 流程控制
```markdown
if

if [ 条件判断式 ] 
then
    代码
elif [条件判断式]
 then
    代码
fi

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022102345150.png#pic_center)
```markdown
case

case $变量名 in 
"值 1"）
如果变量的值等于值 1，则执行程序 1
;;
"值 2"）
如果变量的值等于值 2，则执行程序 2
;;
…省略其他分支…
*）
如果变量的值都不是以上的值，则执行此程序
 ;;
esac

```
```markdown
当命令行参数是 1  时，输出 "周一",  是 2  时，就输出"周二"， 其它情况输出	"other"
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022102639175.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
for 循环

基本语法 1
for 变量 in 值 1  值 2  值 3…
do
    程序
done


基本语法 2
for (( 初始值;循环控制条件;变量变化 )) 
do
程序
done


```
```markdown
打印命令行输入的参数	【会使用到$* $@】
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022102820942.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
从 1 加到 100 的值输出显示
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022103034922.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
while 循环

基本语法 1
while [ 条件判断式 ] 
do
 程序
done
```
```markdown
从命令行输入一个数 n，统计从 1+..+ n  的值是多少
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102210365560.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
#### 6.54.5 read 读取控制台输入
```markdown
read  (选项)   (参数)
选项：
 -p：指定读取值时的提示符；
-t：指定读取值时等待的时间（秒），如果没有在指定的时间内输入，就不再等待了。。

```
```markdown
案例 1：读取控制台输入一个 num 值
案例 2：读取控制台输入一个 num 值，在 10 秒内输入。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022104035449.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

### 6.55 vim
```markdown
Vim是一个功能强大的全屏幕文本编辑器,
是Linux/UNIX上最常用的文本编辑器,
它的作用是建立、编辑、显示文本文件.
Vim没有菜单，只有命令
Vim工作模式
vi有三种基本工作模式：
命令模式
打开文件首先进入命令模式，是使用 vi的 入口
通过 命令 对文件进行常规的编辑操作，例如：定位、
翻页、复制、粘贴、删除……
在其他图形编辑器下，通过 快捷键*或者 鼠标实现的操作，
都在 命令模式下实现
末行模式—— 执行 保存、退出 等操作 
要退出 vi返回到控制台，需要在末行模式下输入命令
末行模式是 vi的 出口
编辑模式 —— 正常的编辑文字
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200923222836890.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
在日常工作中，有可能会遇到 打开一个文件，并定位到指定行的情况
例如：在开发时，知道某一行代码有错误，可以 快速定位 到出错代
码的位置
这个时候，可以使用以下命令打开文件
vi 文件名 +行数
提示：如果只带上 +而不指定行号，会直接定位到文件末尾
```

#### 6.55.1 末行模式命令

| 命令 | 英文 | 功能 |
| :---: | :---: | --- |
| w | write | 保存 |
| q | quit | 退出，如果没有保存，不允许退出 |
| q! | quit | 强行退出，不保存退出 |
| wq | write & quit | 保存并退出 |
| x |  | 保存并退出 |

#### 6.55.2 移动
| 命令 | 功能 | 手指 |
| :---: | --- | :---: |
| h | 向左 | 食指 |
| j | 向下 | 食指 |
| k | 向上 | 中指 |
| l | 向右 | 无名指 |

##### 6.55.2.1 行内移动

| 命令 | 英文 | 功能 |
| :---: | :---: | --- |
| w | word | 向后移动一个单词 |
| b | back | 向前移动一个单词 |
| 0 | | 行首 |
| ^ | | 行首，第一个不是空白字符的位置 |
| $ | | 行尾 |

```markdown
定位命令
set nu设置行号
set nonu 取消行号
```


##### 6.55.2.2 行数移动

| 命令 | 英文 | 功能 |
| :---: | :---: | --- |
| $  |$|移至行尾|
|0|0|移至行首|
| gg | go | 文件顶部 |
| G | go | 文件末尾 |
| 数字gg | go | 移动到 数字 对应行数 |
| 数字G | go | 移动到 数字 对应行数 |
| :数字 |  | 移动到 数字 对应行数 |
|:n|:n| 到第n行|

##### 6.55.2.3 屏幕移动

| 命令 | 英文 | 功能 |
| :---: | :---: | --- |
| Ctrl + b | back | 向上翻页 |
| Ctrl + f | forward | 向下翻页 |
| H | Head | 屏幕顶部 |
| M | Middle | 屏幕中间 |
| L | Low | 屏幕底部 |


##### 6.55.2.4 段落移动
```markdown
vi中使用 空行 来区分段落
在程序开发时，通常 一段功能相关的代码会写在一起 —— 之间
没有空行
```
| 命令 | 功能 |
| :---: | --- |
| { | 上一段 |
| } | 下一段 |

```markdown
括号切换
在程序世界中，()、[]、{} 使用频率很高，而且 都是成对出现的
```



| 命令 | 功能 |
| :---: | --- |
| % | 括号匹配及切换 |

#### 6.55.3 标记
```markdown
在开发时，某一块代码可能需要稍后处理，例如：编辑、查看
此时先使用 m增加一个标记，这样可以 在需要时快速地跳转
回来 或者 执行其他编辑操作
标记名称 可以是 a~z 或者A~Z之间的任意 一个 字母
添加了标记的 行如果被删除，标记同时被删除
如果 在其他行添加了相同名称的标记，之前添加的标记也会被替换掉
```


| 命令 | 英文 | 功能 |
| :---: | :---: | --- |
| mx | mark | 添加标记 x，x 是 a~z 或者 A~Z 之间的任意一个字母 |
| 'x |  | 直接定位到标记 x 所在位置 |

#### 6.55.4 选中文本（可视模式）
```markdown
按 ESC 可以放弃选中，返回到 命令模式
可视模式下，可以和 移动命令 连用，例如：ggVG
能够选中所有内容
```

| 命令 | 模式 | 功能 |
| :---: | --- | --- |
| v | 可视模式 | 从光标位置开始按照正常模式选择文本 |
| V | 可视行模式 | 选中光标经过的完整行 |
| Ctrl + v | 可视块模式 | 垂直方向选中文本 |

#### 6.55.5 撤销和恢复撤销

| 命令 | 英文 | 功能 |
| :---: | :---: | --- |
| u | undo | 撤销上次命令 |
| CTRL + r | redo | 恢复撤销的命令 |

#### 6.55.6 删除文本

| 命令 | 英文 | 功能 |
| :---: | :---: | --- |
| x | cut | 删除光标所在字符，或者选中文字 ,nX 删除光标所在处后n个字符|
| d(移动命令) | delete | 删除移动命令对应的内容 |
| dd | delete | 删除光标所在行ndd删除n行 |
| D | delete | 删除至行尾 |


#### 6.55.7 复制、粘贴

| 命令 | 英文 | 功能 |
| :---: | :---: | --- |
| y(移动命令) | copy | 复制 |
| yy | copy | 复制一行，可以 nyy 复制多行 |
| p | paste | 粘贴 |
|dd|dd|剪切当前行,ndd 剪切当前行以下n行|


#### 6.55.8 替换

| 命令 | 英文 | 功能 | 工作模式 |
| :---: | :---: | --- | --- |
| r | replace | 替换当前字符 | 命令模式 |
| R | replace | 替换当前行光标后的字符 | 替换模式 |

#### 6.55.9 缩排和重复执行

| 命令 | 功能 |
| :---: | --- |
| >> | 向右增加缩进 |
| << | 向左减少缩进 |
| . | 重复上次命令 |

```markdown
缩排命令 在开发程序时，统一增加代码的缩进 比较有用！
     一次性 在选中代码前增加 4 个空格，就叫做 增加缩进
    一次性在选中代码前删除 4 个空格，就叫做 减少缩进*
在 可视模式下，缩排命令只需要使用 一个>或者 <
```

#### 6.55.10常规查找

| 命令 | 功能 |
| :---: | --- |
| /str | 查找 str |

```markdown
查找到指定内容之后，使用 Next查找下一个出现的位置：
n: 查找下一个
N: 查找上一个
单词快速匹配
```

| 命令 | 功能 |
| :---: | --- |
| * | 向后查找当前光标所在单词 |
| # | 向前查找当前光标所在单词 |

```markdown
查找并替换
在 vi 中查找和替换命令需要在 末行模式 下执行
记忆命令格式：
:%s///g
全局替换
一次性替换文件中的 所有出现的旧文本
命令格式如下：
:%s/旧文本/新文本/g
可视区域替换
先选中 要替换文字的 范围
命令格式如下：
:s/旧文本/新文本/g
确认替换
如果把末尾的 g 改成 gc 在替换的时候，会有提示！推荐使用！
:%s/旧文本/新文本/gc
y- yes 替换
n - no 不替换
a - all替换所有
q - quit退出替换
l - last最后一个，并把光标移动到行首
^E 向下滚屏
^Y 向上滚屏
```

#### 6.55.11 插入命令

| 命令 | 英文 | 功能 | 常用 |
| :---: | :---: | --- | :---: |
| i | insert | 在当前字符前插入文本 | 常用 |
| I | insert | 在行首插入文本 | 较常用 |
| a | append | 在当前字符后添加文本 | |
| A | append | 在行末添加文本 | 较常用 |
| o |  | 在当前行后面插入一空行 | 常用 |
| O |  | 在当前行前面插入一空行 | 常用 |

#### 6.55.12 编辑命令和数字连用
```markdown
********** 连续 10 个星号
要实现这个效果可以在 命令模式 下
输入 10，表示要重复 10 次
输入i 进入 编辑模式
输入 *也就是重复的文字
按下 ESC 返回到 命令模式，返回之后 vi就会把第 2、3 两步的操作重复 10次
 利用 可视块 给多行代码增加注释
  移动到要添加注释的 第 1 行代码，按 ^ 来到行首
 按 CTRL + v 进入 可视块 模式
 使用 j向下连续选中要添加的代码行
输入 I进入 编辑模式，并在 行首插入，注意：一定要使用 I
 输入 # 也就是注释符号
按下 ESC返回到 命令模式，返回之后 vi会在之前选中的每一行代码 前 插入 # 
```


#### 6.55.13 末行命令扩展

| 命令 | 英文 | 功能 |
| :---: | :---: | --- |
| :e . | edit | 会打开内置的文件浏览器，浏览要当前目录下的文件 |
| :n 文件名 | new | 新建文件 |
| :w 文件名 | write | 另存为，但是仍然编辑当前文件，并不会切换文件 |



## 7 软件安装
### 7.1 安装软件位置问题
```markdown
linux安装包放在linux/opt,解压缩包放在/usr/local
```
### 7.2 安装JDK



![在这里插入图片描述](https://img-blog.csdnimg.cn/20201021092751279.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


```markdown
# 1.将JDK解压缩到指定目录
	tar -zxvf jdk-8u171-linux-x64.tar.gz -C /usr/
	注意:-C参数是将JDK解压之后文件放入usr目录中

# 2.进入jdk解压缩目录查看
	cd /usr/jdk1.8.0_171/

# 3.查看详细信息
	[root@localhost jdk1.8.0_171]# ls
		bin        db       javafx-src.zip  lib      man          release  THIRDPARTYLICENSEREADME-JAVAFX.txt
		COPYRIGHT  include  jre             LICENSE  README.html  src.zip  THIRDPARTYLICENSEREADME.txt

# 4.配置环境变量
	 vi /etc/profile
	 
# 5.在文件末尾加入如下配置
	export JAVA_HOME=/usr/jdk1.8.0_171
	export PATH=$PATH:$JAVA_HOME/bin

# 6.加载配置生效
	source /etc/profile    加载配置生效
	reboot                 重启系统
	注意: 以上两个选项选择任意一个即可source可以不用重启立即生效,某些情况下source无法生效时,可以使用重启试试

# 7.测试环境变量
	java
	javac
	java -version
```



### 7.3 安装Tomcat
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201021093136236.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
# 0.下载tomcat
	http://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-8/v8.5.46/bin/apache-tomcat-8.5.46.tar.gz

# 1.通过工具上传到Linux系统中

# 2.解压缩到/usr目录中
	[root@localhost ~]# tar -zxvf apache-tomcat-8.5.46.tar.gz -C /usr/
	-C 用来指定解压缩的位置

# 3.查看解压内容
	[root@localhost apache-tomcat-8.5.46]# ls -l
    总用量 124
    drwxr-x---. 2 root root  4096 10月 13 12:27 bin
    -rw-r-----. 1 root root 19318 9月  17 02:19 BUILDING.txt
    drwx------. 2 root root   238 9月  17 02:19 conf
    -rw-r-----. 1 root root  5407 9月  17 02:19 CONTRIBUTING.md
    drwxr-x---. 2 root root  4096 10月 13 12:27 lib
    -rw-r-----. 1 root root 57011 9月  17 02:19 LICENSE
    drwxr-x---. 2 root root     6 9月  17 02:17 logs
    -rw-r-----. 1 root root  1726 9月  17 02:19 NOTICE
    -rw-r-----. 1 root root  3255 9月  17 02:19 README.md
    -rw-r-----. 1 root root  7139 9月  17 02:19 RELEASE-NOTES
    -rw-r-----. 1 root root 16262 9月  17 02:19 RUNNING.txt
    drwxr-x---. 2 root root    30 10月 13 12:27 temp
    drwxr-x---. 7 root root    81 9月  17 02:17 webapps
    drwxr-x---. 2 root root     6 9月  17 02:17 work

# 4.启动tomcat
	[root@localhost apache-tomcat-8.5.46]# ./bin/startup.sh 

# 5.关闭网络防火墙
	systemctl stop firewalld	   关闭网络防火墙
	systemctl disable firewalld  关闭开机自启动(永久关闭)

# 6.在windows中访问tomcat
	http://10.15.0.8:8080/

# 7.显示tomcat实时控制台信息
	进入tomcat的logs目录中使用tail -f catalina.out 命令实时查看控制台信息 

# 8.关闭tomcat
	在tomcat的bin目录下面使用 ./shutdown.sh
```
### 7.4 安装MySQL

#### 7.4.1 环境准备
```markdown
# 1.卸载mariadb，否则安装mysql会出现冲突
# 2.执行命令rpm -qa | grep mariadb
# 3.列出所有被安装的mariadb rpm 包；
# 4.执行命令rpm -e --nodeps mariadb-libs-5.5.56-2.el7.x86_64
```

#### 7.4.2 本地安装(5.6版本默认root没有密码)

```markdown
# 0.上传下载好的软件包到系统中
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102021354660.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```shell
# 0.执行本地安装包之前必须先安装vim
	yum install -y vim
# 1.安装步骤
	rpm -ivh perl-*
	rpm -ivh net-tools-2.0-0.22.20131004git.el7.x86_64.rpm
	rpm -ivh mysql-community-common-5.6.42-2.el7.x86_64.rpm
	rpm -ivh mysql-community-libs-5.6.42-2.el7.x86_64.rpm
	rpm -ivh mysql-community-client-5.6.42-2.el7.x86_64.rpm
	rpm -ivh mysql-community-server-5.6.42-2.el7.x86_64.rpm
```

#### 7.4.3 在线安装

```markdown
# 1.添加官方的yum源创建并编辑mysql-community.repo文件
		vi /etc/yum.repos.d/mysql-community.repo
# 2.粘贴以下内容到源文件中
		[mysql56-community]
    name=MySQL 5.6 Community Server
    baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/7/$basearch/
    enabled=1
    gpgcheck=0
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
    
    注意:如果需要安装mysql5.7只需要将baseurl修改即可 
    	baseurl=http://repo.mysql.com/yum/mysql-5.7-community/el/7/$basearch/

# 3.安装mysql
	sudo yum install -y mysql-community-server
```

#### 7.4.4 设置root用户密码

```markdown
# 1.启动mysql数据库
	[root@localhost mysql]# systemctl start mysqld

# 2.修改mysql数据库密码
	mysqladmin -u root -p password 回车 输入原始密码 在输入新的密码
	
	注意:5.7之前版本安装完成之后没有密码,mysql5.7之后的版本的初始密码是随机生成的，放在了 /var/log/mysqld.log
			使用命令 grep ‘temporary password’ /var/log/mysqld.log 读出来即可
			ROOT!Q2w
# 3.登录mysql
	[root@localhost mysql]# mysql -u root -p  
```

#### 7.4.5 开启远程访问

```markdown
# 1.安装完成mysql时,发现mysql数据库,不允许我们远程
连接需要修改设置
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020213621173.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


```markdown
# 2.登录mysql,并选择使用mysql数据库
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020213657911.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


```markdown
# 3.查看mysql库中的所有表
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020213733474.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
# 4.查询user表
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020213818421.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
# 5.执行如下命令
	grant all privileges on *.* to 'root'@'%' identified by 'ROOT!Q2w' with grant option;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020213841610.png#pic_center)

```markdown
# 6.刷新权限
	flush privileges;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020213904712.png#pic_center)


```markdown
# 7.重启服务
	systemctl restart mysqld
# 8.测试连接
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020213944767.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


## 8 MySQL主从复制
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201021132213288.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
# 0.架构规划
	192.168.202.201    master  主节点
	192.168.202.202    slave   从节点

# 1.修改mysql的配置文件
	[root@localhost mysql]# vim /etc/my.cnf

# 2.分别在配置文件中加入如下配置

	mysql(master):
		server-id=1
		log-bin=mysql-bin
		log-slave-updates
		slave-skip-errors=all
	
	msyql(slave):
		server-id=2
		log-bin=mysql-bin
		log-slave-updates
		slave-skip-errors=all
		
		注意:两个机器的server-id不能一致
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020214037303.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
# 3.重启mysql服务
	systemctl restart mysqld

# 4.登录mysql执行如下命令检测配置是否生效
	SHOW VARIABLES like 'server_id';
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020214132662.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
# 5.登录master节点执行如下命令
		show master status;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020214156378.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
# 6.登录从节点执行如下命令:
		change master to 
		master_host='10.15.0.9',
		master_user='root',
		master_password='root',
		master_log_file='mysql-bin.000001',
		master_log_pos=120;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020214231928.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
# 7.开启从节点
		start slave; 
		stop  slave;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020214251191.png#pic_center)


```markdown
# 8.查看从节点状态
		show slave status\G;

		************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 10.15.0.9
                  Master_User: root
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000001
          Read_Master_Log_Pos: 120
               Relay_Log_File: mysqld-relay-bin.000002
                Relay_Log_Pos: 283
        Relay_Master_Log_File: mysql-bin.000001
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
   	
    注意:
    		1.出现 Slave_IO_Running: Yes 和 Slave_SQL_Running: Yes 说名成功,
    		2.如果在搭建过程出现错误,可以查看查看错误日志文件 cat /var/log/mysqld.log
 
# 9.通过客户端工具进行测试
	
# 10.关闭主从复制(在从节点执行)
	stop slave;
	
注意:如果出现Slave I/O: Fatal error: The slave I/O thread stops because master
and slave have equal MySQL server UUIDs; these UUIDs must be different for
replication to work. Error_code: 1593错误,请执行如下命令,rm -rf 
/var/lib/mysql/auto.cnf删除这个文件,之所以出现会出现这样的问题，是因为我的
从库主机是克隆的主库所在的主机，所以auto.cnf文件中保存的UUID会出现重复.
```

## 9 读写分离
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102113240375.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

### 9.1 MyCat引言
```markdown
基于阿里开源的Cobar产品而研发，Cobar的稳定性、可靠性、
优秀的架构和性能以及众多成熟的使用案例使得MYCAT一开
始就拥有一个很好的起点，站在巨人的肩膀上，我们能看到
更远。业界优秀的开源项目和创新思路被广泛融入到MYCAT
的基因中，使得MYCAT在很多方面都领先于目前其他一些
同类的开源项目，甚至超越某些商业产品。
MYCAT背后有一支强大的技术团队，其参与者都是5年以
上资深软件工程师、架构师、DBA等，优秀的技术团队保
证了MYCAT的产品质量。MYCAT并不依托于任何一个
商业公司，因此不像某些开源项目，将一些重要的特性封闭
在其商业产品中，使得开源项目成了一个摆设. 
```


### 9.2 安装Mycat

```markdown
# 1.下载mycat
	http://dl.mycat.io/1.6-RELEASE/Mycat-server-1.6-RELEASE-20161028204710-linux.tar.gz

# 2.解压mycat
	tar -zxvf Mycat-server-1.6-RELEASE-20161028204710-linux.tar.gz
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020214344840.png#pic_center)

```markdown
# 3.查看解压之后目录]
	[root@localhost mycat]# ls
	总用量 12
	drwxr-xr-x. 2 root root  190 10月 14 22:58 bin
	drwxrwxrwx. 2 root root    6 3月   1 2016 catlet
	drwxrwxrwx. 4 root root 4096 10月 14 22:58 conf
	drwxr-xr-x. 2 root root 4096 10月 14 22:58 lib
	drwxrwxrwx. 2 root root    6 10月 28 2016 logs
	-rwxrwxrwx. 1 root root  217 10月 28 2016 version.txt
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020214404104.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
# 4.移动到/usr目录
	mv mycat/ /usr/

# 5.配置mycat中conf下的配置schema.xml

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201021132625406.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```xml
	<!-- 定义MyCat的逻辑库  -->   
    <schema name="test_schema" checkSQLschema="false" sqlMaxLimit="100" dataNode="testNode"></schema>
    <!-- 定义MyCat的数据节点 -->
    <dataNode name="testNode" dataHost="dtHost" database="test" />
   <dataHost name="dtHost" maxCon="1000" minCon="10" balance="1"
                writeType="0" dbType="mysql" dbDriver="native" switchType="-1"  slaveThreshold="100">
                <heartbeat>select user()</heartbeat>
                <!--写节点-->
                <writeHost host="hostM1" url="192.168.28.128:3306" user="root"
                        password="root">
                  		<!--从节点-->
                			<readHost host="hostS1" url="192.168.28.129:3306" user="root" password="root" />
                </writeHost>
   </dataHost>
```

```markdown
# 6.配置登陆mycat的权限server.xml
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201021132646754.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```xml
<system>
  <!-- 这里配置的都是一些系统属性，可以自己查看mycat文-->
  <property name="defaultSqlParser">druidparser</property>
  <property name="charset">utf8mb4</property>
</system>

<user name="root">
  <property name="password">root</property>
  <property name="schemas">test_schema</property>
</user>
```

```markdown
# 7.启动mycat
	 ./mycat console

# 8.查看日志
	tail -f ../logs/mycat.log

# 9.数据库连接配置,测试
```

## 10 安装Nginx

### 10.1 Nginx的引言
```markdown
Nginx是一款轻量级的Web服务器/反向代理服务器及[电子邮件]
（IMAP/POP3）代理服务器，并在一个BSD-like 协议下发行。
由俄罗斯的程序设计师Igor Sysoev所开发，供俄国大型的入口
网站及搜索引擎Rambler（俄文：Рамблер）使用。其特点是
占有内存少，[并发]能力强，事实上nginx的并发能力确实在同
类型的网页服务器中表现较好，中国大陆使用nginx网站用户有：
[京东]、[新浪]、[网易]、[腾讯]、[淘宝]等。
```

### 10.2 Nginx的安装
```markdown
# 0.安装必要依赖
	yum install -y gcc pcre-devel zlib-devel

# 1.下载Nginx
	http://nginx.org/en/download.html

# 2.将Nginx上传到linux中,并解压缩
	 tar -zxvf nginx-1.11.1.tar.gz


# 3.查看Nginx安装目录
	[root@localhost nginx-1.11.1]# ls
	auto  CHANGES  CHANGES.ru  conf  configure  contrib  html  LICENSE  man  README  src

# 4.在Nginx安装目录中执行如下命令:(指定安装位置)
	./configure --prefix=/usr/nginx

# 5.执行上述命令后,执行如下命令:
	make && make install

# 6.编译完成后进入编译安装目录/usr/nginx目录中查看:
	[root@localhost nginx]# ls -l
	总用量 4
	drwxr-xr-x. 2 root root 4096 10月 14 21:17 conf
	drwxr-xr-x. 2 root root   40 10月 14 21:17 html
	drwxr-xr-x. 2 root root    6 10月 14 21:17 logs
	drwxr-xr-x. 2 root root   19 10月 14 21:17 sbin

# 7.启动nginx,进入nginx安装目录的sbin目录中执行:
	./nginx   

# 8.在windows中浏览器访问,可以看到nginx欢迎页面:
	http://10.15.0.8:80/
	
		注意:关闭网络防火墙

# 9.关闭nginx,进入nginx安装目录的sbin目录中执行:
	./nginx -s stop

# 10.nginx配置文件在nginx安装目录的conf目录中:
	[root@localhost conf]# ls -l
	总用量 60
	-rw-r--r--. 1 root root 2656 10月 14 21:17 nginx.conf
	.......
	注意:nginx.conf为nginx的配置文件,可以在nginx.conf修改nginx默认配置
```
## 11 Tomcat负载均衡集群
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201021132948579.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
# 0.准备多个tomcat
	 tar -zxvf apache-tomcat-8.5.46.tar.gz #解压缩一个新的tomcat安装包
	 mv apache-tomcat-8.5.46 tomcat1 			 #将名称改为tomcat1
	 cp -r tomcat1/ tomcat2								 #复制一份
	 cp -r tomcat1/ tomcat3                #复制一份

# 1.此时当前目录中有三个服务器,如下:
	[root@localhost ~]# ls -l
	总用量 12248
	-rwxrwxrwx. 1 root root  11623939 10月 13 12:25 apache-tomcat-8.5.46.tar.gz
	drwxr-xr-x. 9 root root       220 10月 14 21:28 tomcat1
	drwxr-xr-x. 9 root root       220 10月 14 21:38 tomcat2
	drwxr-xr-x. 9 root root       220 10月 14 21:38 tomcat3
```

```markdown
# 2.修改tomcat1端口号:(伪分布式)
vim tomcat1/conf/server.xml,命令修改如下内容:
a.<Server port="8001" shutdown="SHUTDOWN">   ---关闭端口
b.<Connector port="8888" protocol="HTTP/1.1" ---http协议端口
               connectionTimeout="20000"
               redirectPort="8443" />
c.<Connector port="10010" protocol="AJP/1.3" redirectPort="8443" /> ---AJP协议端口
```

```markdown
# 3.修改tomcat2端口号:(伪分布式)
		vim tomcat2/conf/server.xml,命令修改如下内容:
a.<Server port="8002" shutdown="SHUTDOWN">
b.<Connector port="8889" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
c.<Connector port="10011" protocol="AJP/1.3" redirectPort="8443" />
```

```markdown
# 4.修改tomcat3端口号:(伪分布式)
vim tomcat2/conf/server.xml,命令修改如下内容:
a.<Server port="8003" shutdown="SHUTDOWN">
b.<Connector port="8890" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
c.<Connector port="10012" protocol="AJP/1.3" redirectPort="8443" />
```

```markdown
# 5.将多个tomcat启动:
		tomcat1/bin/startup.sh 
		tomcat2/bin/startup.sh 
		tomcat3/bin/startup.sh
    
# 6.查看tomcat是否启动成功
		ps -aux|grep tomcat
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020214518221.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
# 7.在windows中分别访问tomcat,都看到主页代表启动成功:
	
	http://10.15.0.8:8888/
	http://10.15.0.8:8889/
	http://10.15.0.8:8890/
	
注意:这步一定要关闭网路防火墙
```

```markdown
# 8.将多个tomcat配置到nginx的配置文件中:
	1).在server标签上加入如下配置:
    upstream tomcat-servers {
      server 10.15.0.8:8888;
      server 10.15.0.8:8889;
      server 10.15.0.8:8890;
    }
	2).将配置文件中 location /替换为如下配置:
		location / {
			 proxy_pass http://tomcat-servers;
			 proxy_redirect    off;
			 proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			 proxy_set_header X-Real-IP $remote_addr;
			 proxy_set_header Host $http_host;
			 proxy_next_upstream http_502 http_504 error timeout invalid_header;
		   }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020214548428.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
# 9.进入nginx安装目录sbin目录启动nginx
	./nginx -c /usr/nginx/conf/nginx.conf
```

```markdown
# 10.访问nginx,看到其中一个tomcat画面:
	http://10.15.0.8/ 
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020214619163.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

### 11.1 Nginx负载均衡策略

```markdown
# 1.轮询
	 说明: 默认策略,每个请求会按时间顺序逐一分配到不同的后端服务器

# 2.weight 权重
	说明: weight参数用于指定轮询几率，weight的默认值为1,；weight的数值与访问比率成正比 
    upstream tomcat-servers {
        server localhost:8080   weight=2;  
        server localhost:8081;  
        server localhost:8082   backup;  
    }
    注意：1.权重越高分配到需要处理的请求越多。2.此策略可以与least_conn和ip_hash结合使用主要用于后端服务器性能不均

# 3.ip_hash  4%3=1 
	　说明:指定负载均衡器按照基于客户端IP的分配方式，这个方法确保了相同的客户端的请求一直发送到相同的服务器，以保证session会话。这样每个访客都固定访问一个后端服务器，可以解决session不能跨服务器的问题。
	　upstream tomcat-servers {
        ip_hash;    #保证每个访客固定访问一个后端服务器
        server localhost:8080;
        ......
    }

# 4.least_conn
	说明: 把请求转发给连接数较少的后端服务器。轮询算法是把请求平均的转发给各个后端，使它们的负载大致相同；但是，有些请求占用的时间很长，会导致其所在的后端负载较高。这种情况下，least_conn这种方式就可以达到更好的负载均衡效果。
	upstream tomcat-servers{
        least_conn;    #把请求转发给连接数较少的后端服务器
        server localhost:8080;
    }

```



## 12 MSM配置
```markdown
Memcached Session Manager基于memcache缓存的session共享.即使用cacheDB存取session信息，应用服务器接受新请求将session信息保存在cache DB中，当应用服务器发生故障时，调度器会遍历寻找可用节点，分发请求，当应用服务器发现session不在本机内存时，则去cacheDB中查找，如果找到则复制到本机，这样实现session共享和高可用。
```
```markdown
# 0.准备一个memcache服务

# 1.安装memcached
	 yum install -y memcached

# 2.启动memcached
	memcached -p 11211 -vvv -u root

# 3.tomcat安装的lib目录中放入与memcache整合jar包
		cp *.jar tomcat1/lib
		cp *.jar tomcat2/lib
		cp *.jar tomcat3/lib

# 4.配置tomcat目录中conf目录中context.xml(所有tomcat均需要配置)
	<Context>
    <Manager className="de.javakaffee.web.msm.MemcachedBackupSessionManager"
        memcachedNodes="n1:10.15.0.8:11211"
        sticky="false"  
    		sessionBackupAsync="false"  
        requestUriIgnorePattern=".*\.(ico|png|gif|jpg|css|js)$"
        transcoderFactoryClass="de.javakaffee.web.msm.serializer.kryo.KryoTranscoderFactory"
        />
	</Context>

# 5.放入测试项目进行测试
```

##  13 虚拟机网络模式
```markdown
Bridged（桥接模式）
VMnet0表示的是用于桥接模式下的虚拟交换机；
VMnet1表示的是用于仅主机模式下的虚拟交换机；
VMnet8表示的是用于NAT模式下的虚拟交换机。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200831084842463.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
同时，在主机的“网络连接”中我们可以看到这两块虚拟网卡，
VMware Network Adapter VMnet1      
作用于仅主机模式，用来实现虚拟机和物理机进行通信，
和联网无关系，它的联网需要物理网卡的共享才能上网
VMware Network Adapter VMnet8    
作用于NAT模式，用来实现虚拟机和物理机进行通信，
和联网无关系，它是借助于虚拟nat进行联网
如果将这两块卸载了，可以在vmware的“编辑”下的“虚拟
网络编辑器”中点击“还原默认设置”，可重新将虚拟网卡还原。
看到这里，大家肯定有疑问，为什么在物理机上没有
VMware Network Adapter VMnet0虚拟网卡呢？
因为桥接模式是通过虚拟网桥进行通信和联网的，而不需要
虚拟网卡来使虚拟机和物理机进行通信

桥接模式就是将主机网卡与虚拟机的网卡利用虚拟网桥
进行通信。在桥接的作用下，类似于把物理主机虚拟为
一个交换机，所有桥接设置的虚拟机连接到这个交换机
的一个接口上，物理主机也同样插在这个交换机当中，
所以所有桥接下的网卡与网卡都是交换模式的，相互可
以访问而不干扰。

在桥接模式下，虚拟机ip地址需要与主机在同一个网
段，如果需要联网，则网关与DNS需要与主机网卡一
致。其网络结构如下图所示：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200831085153890.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
host-only(仅主机模式)
Host-Only模式是出于安全考虑，Host-Only模式将虚拟机
与外网隔开，使得虚拟机成为一个独立的系统，只与主
机相互通讯。

如果要使得虚拟机能联网，我们可以将主机网卡共享给
VMware Network Adapter VMnet1网卡，从而达到虚拟
机联网的目的
Host-Only模式其实就是NAT模式去除了虚拟NAT设备，
然后使用VMware Network Adapter VMnet1虚拟网卡
连接VMnet1虚拟交换机来与虚拟机通信的，其网
络结构如下图所示：
```
![*](https://img-blog.csdnimg.cn/20200830192741696.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
只可以进行虚拟机之间的访问，访问不了本机的真实网卡，即主
机网卡。但可以访问VMware Network Adapter VMnet1这块虚
拟网卡。图中红线表示到达不了。
```

```markdown
NAT（地址转换模式）
一般虚拟机上网，使用桥接模式配置简单，但如果你的网络
环境是ip资源很缺少或对ip管理比较严格的话，那桥接模式
就不太适用了，而我们又需要联网。
我们该如何解决呢？

我们需要用到vmware的另一种网络模式：NAT模式。
NAT模式借助虚拟NAT设备和虚拟DHCP服务器，使
得虚拟机可以联网。其网络结构如下图所示：
```
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200831083051319.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
在NAT模式中，主机网卡直接与虚拟NAT设备相连，然后虚
拟NAT设备与虚拟DHCP服务器一起连接在虚拟交换机
VMnet8上，这样就实现了虚拟机联网。

那么我们会觉得很奇怪，为什么需要虚拟网卡
VMware Network Adapter VMnet8呢？
原来我们的VMware Network Adapter VMnet8
虚拟网卡主要是为了实现主机与虚拟机之间的通信。

NAT模式，利用虚拟的NAT设备以及虚拟DHCP服务
器来使虚拟机连接外网，而VMware Network Adapter VMnet8
虚拟网卡是用来与虚拟机通信的。
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复Linux
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
