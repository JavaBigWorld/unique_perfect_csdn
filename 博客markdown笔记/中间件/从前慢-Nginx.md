# Nginx
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210717012536214.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1 Nginx概述

```markdown
Nginx (engine ) 是一个高性能的 HTTP 和 反向代理服务器,特点是占
有内存少，并发能力强，事实上 nginx 的并发能力确实在同类型的网页
服务器中表现较好，中国大陆使用 nginx
网站用户有：百度、京东、新浪、网易、腾讯、淘宝等
```
### 1.1 Nginx 作为 web 服务器

```markdown
Nginx 可以作为静态页面的 web 服务器，同时还支持 CGI 协议
的动态语言，比如 perl、php等。但是不支持 java。Java 程序只
能通过与 tomcat 配合完成。Nginx 专为性能优化而开发，
性能是其最重要的考量,实现上非常注重效率 ，能经受高负载的
考验,有报告表明能支持高达 50,000 个并发连接数。
```
### 1.2 正向代理

```markdown
需要在客户端配置代理服务器进行指定网站访问
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210305130937652.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)


### 1.3  反向代理

```markdown
暴露的是代理服务器地址，隐藏了真实服务器 IP 地址。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127095707958.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 1.4 负载均衡

```markdown
增加服务器的数量，然后将请求分发到各个服务器上，将原先请
求集中到单个服务器上的情况改为将请求分发到多个服务器上，将
负载分发到不同的服务器，也就是我们所说的负载均衡
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112710001694.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 1.5 动静分离
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127100050210.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
## 2 nginx常用的命令和配置文件

```bash
在 windows 系统中访问 linux 中 nginx，
默认不能访问的，因为防火墙问题
（1）关闭防火墙
（2）开放访问的端口号，80 端口
查看开放的端口号
firewall-cmd --list-all
设置开放的端口号
firewall-cmd --add-service=http --permanent
firewall-cmd --add-port=80/tcp --permanent
重启防火墙
firewall-cmd –reload


进入 nginx 目录中
cd /usr/local/nginx/sbin
1、查看 nginx 版本号
./nginx -v
2、启动 nginx
./nginx
3、停止 nginx
./nginx -s stop
4、重新加载 nginx
./nginx -s reload

```
### 2.1 Nginx的配置文件

```markdown
第一部分：全局块 
（1）全局块：配置服务器整体运行的配置指令
比如 worker_processes 1;处理并发数的配置

（2）events 块：影响 Nginx 服务器与用户的网络连接
比如 worker_connections 1024; 支持的最大连接数为 1024


（3）http 块
还包含两部分：http 全局块、server 块
```
### 2.2 Nginx配置实例- 反向代理实例1

```markdown
1、实现效果
（1）打开浏览器，在浏览器地址栏输入地址 www.123.com，跳转到 liunx 系统 tomcat 主页
面中
2、准备工作
（1）在 liunx 系统安装 tomcat，使用默认端口 8080
tomcat 安装文件放到 liunx 系统中，解压
进入 tomcat 的 bin 目录中，./startup.sh 启动 tomcat 服务器
（2）对外开放访问的端口
firewall-cmd --add-port=8080/tcp --permanent
firewall-cmd –reload
查看已经开放的端口号
firewall-cmd --list-all
（3）在 windows 系统中通过浏览器访问 tomcat 服务器

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127102813718.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
访问过程的分析
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127102248517.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
4、具体配置
第一步 在 windows 系统的 host 文件进行域名和 ip 对应关系的配置
（1）添加内容在 host 文件中
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127102940485.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127103016993.png#pic_center)
```markdown
在 nginx 进行请求转发的配置（ 反向代理配置）
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127103102573.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
5、最终测试
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127103136919.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 2.3 Nginx配置实例-反向代理实例2
```markdown
1、实现效果
使用 nginx  反向代理，根据访问的路径跳转到不同端口的服务中
nginx 监听端口为 9001，
访问 http://192.168.17.129:9001/edu/ 直接跳转到 127.0.0.1:8080
访问 http:// 192.168.17.129:9001/vod/ 直接跳转到 127.0.0.1:8081
2、准备工作
（1）准备两个 tomcat 服务器，一个 8080 端口，一个 8081 端口
（2）创建文件夹和测试页面
3、具体配置
（1）找到 nginx 配置文件，进行 反向代理配置
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127103440920.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
（2）开放对外访问的端口号 9001 8080 8081
```
```markdown
4、最终测试
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112710355138.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127103600892.png#pic_center)
## 3 Nginx 配置实例-负载均衡
```markdown
1、实现效果
（1）浏览器地址栏输入地址 http://192.168.17.129/edu/a.html，
负载均衡效果，平均 8080
和 8081 端口中
2、准备工作
（1）准备两台 tomcat 服务器，一台 8080，一台 8081
（2）在两台 tomcat 里面 webapps 目录中，创建名称是 edu 文件夹，
在 edu 文件夹中创建页面 a.html，用于测试
3、在 nginx 的配置文件中进行负载均衡的配置
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127104719438.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
4、nginx 分配服务器策略
1、轮询（默认）
每个请求按时间顺序逐一分配到不同的后端服务器，
如果后端服务器 down 掉，能自动剔除。
2、weight
weight 代表权,重默认为 1,权重越高被分配的客户端越多
指定轮询几率，weight 和访问比率成正比，用于后端服务器性
能不均的情况。 例如：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127104904601.png#pic_center)
```markdown
3、ip_hash
每个请求按访问 ip 的 hash 结果分配，这样每个访客固定访问一个
后端服务器，可以解决 session 的问题。 例如：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127104937470.png#pic_center)
```markdown
4、fair（第三方）
按后端服务器的响应时间来分配请求，响应时间短的优先分配。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127105007348.png#pic_center)
## 4 Nginx 配置实例-动静分离
### 4.1 什么是动静分离
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127105125442.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
通过 location 指定不同的后缀名实现不同的请求转发。通过expires 
参数设置，可以使浏览器缓存过期时间，减少与服务器之前的请求
和流量。具体 Expires 定义：是给一个资源
设定一个过期时间，也就是说无需去服务端验证，直接通过浏览器
自身确认是否过期即可，所以不会产生额外的流量。此种方法非常
适合不经常变动的资源。（如果经常更新的文件，不建议使用 
Expires 来缓存），我这里设置 3d，表示在这 3 天之内访问这
个 URL，发送一个请求，比对服务器该文件最后更新时间没
有变化，则不会从服务器抓取，返回状态码 304，
如果有修改，则直接从服务器重新下载，返回状态码 200。
```
### 4.2 准备工作
```markdown
（1）在 liunx 系统中准备静态资源，用于进行访问
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127105717954.png#pic_center)
```markdown
2、具体配置
（1）在 nginx 配置文件中进行配置
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127105927921.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
3、最终测试
（1）浏览器中输入地址
http://192.168.17.129/image/01.jpg
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127110027863.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
因为配置文件 autoindex on
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127110326171.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
（2）在浏览器地址栏输入地址
http://192.168.17.129/www/a.html
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127110855931.png#pic_center)
## 5 Nginx配置高可用的集群
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127112547527.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
1、什么是 nginx 高可用
（1）需要两台 nginx 服务器
（2）需要 keepalived
（3）需要虚拟 ip


2、配置高可用的准备工作
（1）需要两台服务器 192.168.17.129 和 192.168.17.131
（2）在两台服务器安装 nginx
（3）在两台服务器安装 keepalived


3、在两台服务器安装 keepalived
（1）使用 yum 命令进行安装
yum install keepalived –y
（2）安装之后，在 etc 里面生成目录 keepalived，
有文件 keepalived.conf
```
```markdown
4、完成高可用配置（主从配置）
（1）修改/etc/keepalived/keepalivec.conf 配置文件
global_defs {
 notification_email {
 acassen@firewall.loc
 failover@firewall.loc
 sysadmin@firewall.loc
 }
 notification_email_from Alexandre.Cassen@firewall.loc
 smtp_server 192.168.17.129
 smtp_connect_timeout 30
 router_id LVS_DEVEL
}
vrrp_script chk_http_port {
 script "/usr/local/src/nginx_check.sh"
 interval 2 #（检测脚本执行的间隔）
 weight 2
}
vrrp_instance VI_1 {
 state BACKUP # 备份服务器上将 MASTER 改为 BACKUP
 interface ens33 //网卡
 virtual_router_id 51 # 主、备机的 virtual_router_id 必须相同
 priority 90 # 主、备机取不同的优先级，主机值较大，备份机值较小
 advert_int 1
  authentication {
 auth_type PASS
 auth_pass 1111
 }
 virtual_ipaddress {
 192.168.17.50 // VRRP H 虚拟地址
 }
}
（2）在/usr/local/src 添加检测脚本
#!/bin/bash
A=`ps -C nginx –no-header |wc -l`
if [ $A -eq 0 ];then
 /usr/local/nginx/sbin/nginx
 sleep 2
 if [ `ps -C nginx --no-header |wc -l` -eq 0 ];then
 killall keepalived
 fi
fi

（3）把两台服务器上 nginx 和 keepalived 启动
启动 nginx：./nginx
启动 keepalived：systemctl start keepalived.service
5、最终测试
（1）在浏览器地址栏输入 虚拟 ip 地址 192.168.17.50
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127114234623.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127114252211.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
（2）把主服务器（192.168.17.129）nginx 和 keepalived 停止，
再输入 192.168.17.50
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127114351483.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127114400582.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
## 6 Nginx 的原理
```markdown
1、mater 和 worker
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112711495190.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
2、worker 如何进行工作的
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127115017298.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
3、一个 master 和多个 woker 有好处
（1）可以使用 nginx –s reload 热部署，利用 nginx 进行热部署操作
（2）每个 woker 是独立的进程，如果有其中的一个woker出现问题，
其他 woker 独立的，继续进行争抢，实现请求过程，不会造成服
务中断
4、设置多少个 woker 合适
worker 数和服务器的 cpu 数相等是最为适宜的
5、连接数 worker_connection
第一个：发送请求，占用了 woker 的几个连接数？
答案：2 或者 4 个
第二个：nginx 有一个 master，有四个woker，每个woker支持最大
的连接数 1024，支持的
最大并发数是多少？
普通的静态访问最大并发数是： worker_connections * worker_processes /2，
而如果是HTTP作为反向代理来说最大并发数量应该是 
worker_connections *worker_processes/4。
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复nginx
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
