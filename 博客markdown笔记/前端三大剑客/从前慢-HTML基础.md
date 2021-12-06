# HTML
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210713192708235.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1.1  前端工程师

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200320113515609.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
## 1.2 认识B/S架构
~~~markdown
B/S架构的系统有什么优点和缺点？
优点：升级方便，只升级服务器端代码即可。维护成本低
缺点：速度慢、体验不好、界面不炫酷
企业内部的解决方案都是采用B/S架构的系统，因为企业内部
办公需要的一些系统不需要炫酷，不需要特别好的用户体验，
只要能做数据的增删改查即可。并且企业内部更注重维护的成本
~~~
## 1.3  认识C/S架构
~~~markdown
优点：速度快，体验好，界面炫酷。（娱乐型的系统
多数是c/s架构的）
缺点：升级麻烦，维护成本较高。
~~~
## 1.4 浏览器内核

| 浏览器  |      内核      | 备注                                                         |
| :------ | :------------: | :----------------------------------------------------------- |
| IE      |    Trident     | IE、猎豹安全、360极速浏览器、百度浏览器                      |
| firefox |     Gecko      | 可惜这几年已经没落了，打开速度慢、升级频繁、猪一样的队友flash、神一样的对手chrome。 |
| Safari  |     webkit     | 现在很多人错误地把 webkit 叫做 chrome内核（即使 chrome内核已经是 blink 了）。苹果感觉像被别人抢了媳妇，都哭晕再厕所里面了。 |
| chrome  | Chromium/Blink | 在 Chromium 项目中研发 Blink 渲染引擎（即浏览器核心），内置于 Chrome 浏览器之中。Blink 其实是 WebKit 的分支。大部分国产浏览器最新版都采用Blink内核。二次开发 |
| Opera   |     blink      | 现在跟随chrome用blink内核。                                  |

## 1.5 Web 标准构成
```markdown
构成：主要包括结构（Structure）、表现（Presentation）和行
为（Behavior）三个方面。
```

| 标准 | 说明                                                         | 
| :--- | :----------------------------------------------------------- | :----------------------------- |
| 结构 | 结构用于对**网页元素**进行整理和分类，咱们主要学的是HTML。   | 
| 表现 | 表现用于设置网页元素的版式、颜色、大小等**外观样式**，主要指的是CSS | 
| 行为 | 行为是指网页模型的定义及**交互**的编写，咱们主要学的是 Javascript | 

## 1.6 HTML 初识
```markdown
HTML 指的是超文本标记语言 (Hyper Text Markup Language)
是
用来描述网页的一种语言。
HTML 不是一种编程语言，而是一种标记语言 (markup language)
标记语言是一套标记标签 (markup tag)

所谓超文本，有2层含义：
1. 因为它可以加入图片、声音、动画、多媒体等内
2. 容（超越文本限制 ）
3. 不仅如此，它还可以从一个文件跳转到另一个文件，与世界
4. 各地主机的文件连接（超级链接文本 ）。
```
## 1.7 HTML的细节
```markdown
<!DOCTYPE html> 
声明位于文档中的最前面的位置，处于  标签之前。此标签
可告知浏览器文档使用哪种 HTML 或 XHTML 规范。​

<html lang="en">  指定html 语言种类
最常见的2个：
1. en定义语言为英语
2.zh-CN定义语言为中文

<meta charset="UTF-8" />
这句话是让 html 文件是以 UTF-8 编码保存的， 浏览器根
据编码去解码对应的html内容。
```

## 1.8  排版标签总结
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200502095852502.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
## 1.9 文本格式化标签
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200502095945766.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
## 2.1 标签属性
```html
<标签名 属性1="属性值1" 属性2="属性值2" …> 内容 </标签名>
<手机 颜色="红色" 大小="5寸">  </手机>
```

## 2.2 图像标签img 
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020061508172169.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<h4> 正常的图片： </h4>
	<img src="timg.gif" />
	<h4> 带有alt 替换文本：（图片不能正常显示，就显示文字） </h4>
	<img src="timg1.gif" alt="胡歌咋吹笛子" />
	<h4> 带有title 提示文本：（鼠标放到图片上，显示的文字） </h4>
	<img src="timg.gif" title="胡歌还在吹笛子等霍建华" />
	<h4> 修改图片大小 宽度 width  高度 height </h4>
	<img src="timg.gif" title="胡歌还在吹笛子等霍建华" width="600"  />
	<h4> 带有边框的图片 </h4>
	<img src="timg.gif" title="胡歌还在吹笛子等霍建华" width="600" border="10"  />

</body>
</html>
```
## 2.3 链接标签
```html
<a href="跳转目标" target="目标窗口的弹出方式">文本或图像</a>
```

 | 属性   | 作用                                                         |
| ------ | :----------------------------------------------------------- |
| href   | 用于指定链接目标的url地址，（必须属性）当为标签应用href属性时，它就具有了超链接的功能 |
| target | 用于指定链接页面的打开方式，其取值有_self和_blank两种，其中_self为默认值，__blank为在新窗口中打开方式。 |

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<h4>外部链接：</h4>
	<a href="http://www.baidu.com" target="_blank"> 百度一哈 </a>
	<a href="http://www.sohu.com"> 搜狐 </a>
	<h4>内部链接：</h4>
	<a href="demo.html">胡哥哥 </a>
	<h4>空链接：</h4>
	<a href="#"> 霍建华 </a>
	<h4> 图像链接：</h4>
	<a href="http://www.baidu.com" target="_blank"> <img src="timg.jpg" /> </a>
</body>
</html>
```
## 2.4 注释标签
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>注释标签</title>
</head>
<body>
	<!-- 这是一个h1标签 -->
	<!-- 你看不见我 -->
	<h1>刘德华专访</h1>
</body>
</html>
```
## 2.5 路径
```markdown
相对路径，是从代码所在的这个文件出发， 去寻找我
们的目标文件的

绝对路径以Web站点根目录为参考基础的目录路径。之所以
称为绝对，意指当所有网页引用同一个文件时，所使用的路径
都是一样的。
“D:\web\img\logo.gif”，或完整的网络地址，
例如“http://www.itcast.cn/images/logo.gif”。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
    <!-- 我们不提倡使用这种方法 -->
	<img src="C:\Users\itcast\Desktop\前端基础HTML第一天\案例\timg.gif" />

	<!-- 绝对的网络地址 还是可以用的 -->
	<img src="http://www.itcast.cn/2018czgw/images/logo.png" />
</body>
</html>
```
## 2.6 锚点定位
```html
1. 使用相应的id名标注跳转目标的位置。 (找目标)
  <h3 id="two">第2集</h3> 

2. 使用<a href="#id名">链接文本</a> 创建链接文本（被点击的）
 
  <a href="#two">   
```
## 2.7 base 标签
```markdown
<base target="_blank" />
1. base 可以设置整体链接的打开状态   
2. base 写到  <head>  </head>  之间
3. 把所有的连接 都默认添加 target="_blank"
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<base target="_blank">
</head>
<body>
	<a href="http://www.baidu.com">百度</a>
	<a href="http://www.sina.com">新浪</a>
	<a href="http://www.sohu.com">搜狐</a>
	<a href="http://www.163.com">网易</a>
</body>
</html>
```
## 2.8 预格式化文本pre标签
```markdown
所谓的预格式化文本就是 ，按照我们预先写好的文字格式
来显示页面， 保留空格和换行等。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
<pre>
	    班级歌
	清蒸班主任，
	油炸小助教。
	爆火炒导师，
	班长要红烧。
</pre>
</body>
</html>
```
## 2.9 特殊字符 
![*](https://img-blog.csdnimg.cn/20200605050925540.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	楚乔是&nbsp;&nbsp;&nbsp;&nbsp;燕洵&nbsp;&nbsp;&nbsp;&nbsp;的  <br />
	&lt;p&gt; 表示一个段落   &lt;  &gt;  更多 &gt;&gt;
</body>
</html>
```
## 2.10  表格 table
 ```html
 <table>
  <tr>
    <td>单元格内的文字</td>
    ...
  </tr>
  ...
</table>
 ```
### 2.10.1  表格属性
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200605052934652.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200605053058397.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```html
<table width="500" height="300" border="1" cellpadding="20" cellspacing="0" align="center">
        <tr>
            <th>姓名</th>
            <th>性别</th>
            <th>年龄</th>
        </tr>
        <tr>
            <td>刘德华</td>
            <td>男</td>
            <td>55</td>
        </tr>
        <tr>
            <td>郭富城</td>
            <td>男</td>
            <td>52</td>
        </tr>
        <tr>
            <td>张学友</td>
            <td>男</td>
            <td>58</td>
        </tr>
        <tr>
            <td>黎明</td>
            <td>男</td>
            <td>18</td>
        </tr>
        <tr>
            <td>刘晓庆</td>
            <td>女</td>
            <td>63</td>
        </tr>
    </table>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200615090054789.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 2.10.2 表头单元格标签th
```markdown
 th 也是一个单元格   只不过和普通的 td单元格不一样，它会
 让自己里面的文字居中且加粗
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<table border="1" width="500" height="200" align="center" cellspacing="0" cellpadding="20">
		<tr> 
			<th>姓名</th>  
			<th>年龄</th>  
			<th>性别</th>  
		</tr>
		<tr> 
			<td>张三</td>  
			<td>18</td>  
			<td>男</td>  
		</tr>
		<tr> 
			<td>张三丰</td>  
			<td>99</td>  
			<td>女</td>  
		</tr>
		<tr> 
			<td>张三疯子</td>  
			<td>199</td>  
			<td>未知</td>  
		</tr>
	</table>
</body>
</html>
```
### 2.10.3 表格标题caption
```markdown
<table>
  <caption>我是表格标题</caption>
</table>
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<table border="1" width="500" height="200" align="center" cellspacing="0" cellpadding="20">
		<caption>个人信息表</caption>
		<tr> 
			<th>姓名</th>  
			<th>年龄</th>  
			<th>性别</th>  
		</tr>
		<tr> 
			<td>张三</td>  
			<td>18</td>  
			<td>男</td>  
		</tr>
		<tr> 
			<td>张三丰</td>  
			<td>99</td>  
			<td>女</td>  
		</tr>
		<tr> 
			<td>张三疯子</td>  
			<td>199</td>  
			<td>未知</td>  
		</tr>
	</table>
</body>
</html>
```

### 2.10.4 合并单元格
```markdown
合并单元格2种方式

跨行合并：rowspan="合并单元格的个数"      
跨列合并：colspan="合并单元格的个数"
合并的顺序我们按照   先上 后下     先左  后右 的顺序 

合并单元格三步曲
1. 先确定是跨行还是跨列合并
2. 根据 先上 后下   先左  后右的原则找到目标单元格    然后写上 合并方式 还有 要合并的单元格数量  比如 ： <td colspan="3">   </td>
3. 删除多余的单元格 单元格 
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<table border="1" width="500" height="240" align="center" cellspacing="0">
		<caption> 个人简历 </caption>
		<tr>
			<td>刘德华</td>
			<td>男</td>
			<td>18</td>
			<!-- 目标单元格  先上后下 -->
			<td rowspan="2">照片</td>
		</tr>
		<tr>
			<td>身高 180</td>
			<td>汉族</td>
			<td>已婚</td>
			<!-- <td>照片</td> 这个单元格是多余的 -->
		</tr>
		<tr>
			<td>个人作品</td>
			<!-- 第二个单元格是目标单元格 -->
			<td colspan="3">个人作品</td>
			
		</tr>
		<tr>
			<td>个人简历</td>
			<td colspan="3">个人简历</td>
		</tr>
	</table>
</body>
</html>
```
### 2.10.5 总结表格
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200605054203574.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 2.10.6  表格划分结构
```markdown
1. <thead></thead>：用于定义表格的头部。用来放标题之类
2. 的东西。<thead> 内部必须拥有 <tr> 标签！
3. <tbody></tbody>：用于定义表格的主体。放数据本体 。
4. <tfoot></tfoot>放表格的脚注之类。
5. 以上标签都是放到table标签中。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<table border="1" cellspacing="0" align="center" width="500">
		<thead>
			<tr>
				<th>姓名</th>
				<th>性别</th>
				<th>年龄</th>
			</tr>
		</thead>
		<tbody>
			<tr>
				<td>刘德华</td>
				<td>男</td>
				<td>55</td>
			</tr>
			<tr>
				<td>刘若英</td>
				<td>女</td>
				<td>35</td>
			</tr>
			<tr>
				<td>刘晓庆</td>
				<td>女</td>
				<td>65</td>
			</tr>
			<tr>
				<td>刘三姐</td>
				<td>女</td>
				<td>15</td>
			</tr>
		</tbody>
		<tfoot>
			<tr>
				<td>信息地址</td>
				<td colspan="2"> 北京市金燕龙校区举办演唱会</td>		
			</tr>
		</tfoot>
	</table>
</body>
</html>
```
## 3.1 无序列表 ul 
```markdown
无序列表的各个列表项之间没有顺序级别之分，是并列的。
1. <ul></ul>中只能嵌套<li></li>，直接在<ul></ul>标签中
输入其他标签或者文字的做法是不被允许的。
2. <li>与</li>之间相当于一个容器，可以容纳所有元素。
3. 无序列表会带有自己样式属性，放下那个样式，一会让CSS来！
```
```html
<ul>
  <li>列表项1</li>
  <li>列表项2</li>
  <li>列表项3</li>
  ......
</ul>
```
## 3.2 有序列表 ol
```markdown
有序列表即为有排列顺序的列表，其各个列表项按照一定的
顺序排列定义，有序列表的基本语法格式如下：
```
```html
<ol>
  <li>列表项1</li>
  <li>列表项2</li>
  <li>列表项3</li>
  ......
</ol>
```
## 3.3 自定义列表
```markdown
定义列表常用于对术语或名词进行解释和描述，定义列
表的列表项前没有任何项目符号。其基本语法如下：
```
```html
<dl>
  <dt>名词1</dt>
  <dd>名词1解释1</dd>
  <dd>名词1解释2</dd>
  ...
  <dt>名词2</dt>
  <dd>名词2解释1</dd>
  <dd>名词2解释2</dd>
  ...
</dl>
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
  <h2>1. 无序列表</h2>
  您喜欢的水果有哪些？
  <ul>
  	<li>榴莲</li>
  	<li>香蕉</li>
  	<li>苹果</li>	
  	<li>大白菜</li>
  </ul>
  <h2>2. 有序列表 </h2>
  奥运金牌榜
  <ol>
  	<li>中国</li>
  	<li>英国</li>
  	<li>俄罗斯</li>
  	<li>美国</li>
  </ol>

   <h2>3. 自定义列表 </h2>
   地区：
   <dl>
   		<dt>北京</dt>
   		<dd>昌平区</dd>
   		<dd>海淀区</dd>
   		<dd>大兴区</dd>
   		<dd>东城区</dd>
   		<dt>山东</dt>
   		<dd>威海</dd>
   		<dd>潍坊</dd>
   		<dd>济南</dd>
   		<dd>青岛</dd>
   </dl>
</body>
</html>
```

## 3.4 表单标签
```markdown
在HTML中，一个完整的表单通常由表单控件（也称为表单
元素）、提示信息和表单域3个部分构成。

表单控件： 

​       包含了具体的表单功能项，如单行文本输入框、密码输
入框、复选框、提交按钮、重置按钮等。

提示信息：

​        一个表单中通常还需要包含一些说明性的文字，提示
用户进行填写和操作。

表单域：

​      他相当于一个容器，用来容纳所有的表单控件和提示信息，可
以通过他定义处理表单数据所用程序的url地址，以及数据提交到
服务器的方法。如果不定义表单域，表单中的数据就无法传送
到后台服务器。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020061509432983.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 3.4.1 input 控件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200615094537767.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
1. type 属性

这个属性通过改变值，可以决定了你属于那种input表单。
比如 type = 'text'  就表示 文本框 可以做 用户名， 昵称等。
比如 type = 'password'  就是表示密码框   用户输入的内容 是不可见的。

2. value属性值
value 默认的文本值。 有些表单想刚打开页面就默认显示几个文字，就可以通过这个value 来设置。

3. name属性
 name表单的名字， 这样，后台可以通过这个name属性找到这个表单。  页面中的表单很多，name主要作用就是用于区别不同的表单。
 radio  如果是一组，我们必须给他们命名相同的名字 name   这样就可以多个选其中的一个啦

4.checked属性
表示默认选中状态。  较常见于 单选按钮和复选按钮。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	 <!-- type text  是一个文本框 -->
    用户名： <input type="text" value="请输入用户名" name="username" />  <br />
    昵称： <input type="text" value="请输入昵称" name="nicheng" />  <br />
     <!-- type text  是一个密码框 -->
    密码：  <input type="password" name="pwd" /> <br />
    性别： 
	    男 <input type="radio" name="sex" />  
	    女  <input type="radio" name="sex" checked="checked" /> 
	    未知  <input type="radio" name="sex" />  <br />
	爱好:
	    睡觉 <input type="checkbox" name="hobby" checked="checked" />
	    爬山 <input type="checkbox" name="hobby" />
	    篮球 <input type="checkbox" name="hobby" />
	    足球 <input type="checkbox" name="hobby" /> <br />
	         <!-- 普通按钮需要些value值 -->
	   		<input type="button" value="获取短信验证码" />
	   		<input type="submit" value="提交所填" />
	   		<input type="reset" value="重置所填" />
	   		<!-- 图片提交按钮 里面必须包含 src 属性 -->
	   		<input type="image" src="images/btn.png"   /> <br />
     上传头像:
            <!-- 文件域 上传文件用的-->
            <input type="file" />
</body>
</html>
```
### 3.4.2 input 属性小结

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200615094921251.png)
### 3.4.3 label标签
```markdown
 用于绑定一个表单元素, 当点击label标签的时候, 被绑定
 的表单元素就会获得输入焦点。	
```
```html
第一种用法就是用label直接包括input表单。
<label> 用户名： <input type="radio" name="usename" value="请输入用户名">   </label> 

第二种用法 for 属性规定 label 与哪个表单元素绑定。
<label for="sex">男</label>
<input type="radio" name="sex"  id="sex">


<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
   <h4>第一种用法，label直接包含 表单</h4>
	<label> 用户名： <input type="text" /> </label>
   <h4>第二种方法，通过for 和 id 来控制 </h4>
   <label for="nc"> 昵称： </label>     <input type="text" id="nc" />
</body>
</html>
```

### 3.4.4 textarea控件(文本域)
```markdown
通过textarea控件可以轻松地创建多行文本输入框.

cols="每行中的字符数" rows="显示的行数"  我们实际开发不用
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	用户留言：  <textarea></textarea>
</body>
</html>
```
### 3.4.5 文本框和文本域区别
 
 | 表单              |  名称  |       区别       |                  默认值显示 |             用于场景 |
| :---------------- | :----: | :--------------: | --------------------------: | -------------------: |
| input type="text" | 文本框 | 只能显示一行文本 | 单标签，通过value显示默认值 | 用户名、昵称、密码等 |
| textarea          | 文本域 | 可以显示多行文本 |  双标签，默认值写到标签中间 |               留言板 |

### 3.4.6 select下拉列表
```markdown
1. select 中至少包含一对 option 
2. 在option 中定义selected =" selected "时，当前项即
为默认选中项。
3. 但是我们实际开发会用的比较少
```
```html
<select>
  <option>选项1</option>
  <option>选项2</option>
  <option>选项3</option>
  ...
</select>
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	籍贯：
	<!-- 省份选择的  selected="selected" 表示默认选中 北京 -->
	<select>
		<option>--请选择省份--</option>
		<option selected="selected">北京</option>
		<option>天津</option>
		<option>上海</option>
		<option>山东</option>
	</select>
    <!-- 城市选择 -->
	<select>
		<option>--请选择城市--</option>
		<option>海淀区</option>
		<option>昌平区</option>
		<option>通州区</option>
		<option>雄安区</option>
	</select>

</body>
</html>
```
### 3.4.7 form表单域

| 属性   | 属性值   | 作用                                               |
| ------ | :------- | -------------------------------------------------- |
| action | url地址  | 用于指定接收并处理表单数据的服务器程序的url地址。  |
| method | get/post | 用于设置表单数据的提交方式，其取值为get或post。    |
| name   | 名称     | 用于指定表单的名称，以区分同一个页面中的多个表单。 |

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<form action="" method="post" name="user">
		用户名: <input type="text" name="username" /> <br />
		密码: <input type="password" name="pwd" /><br />
		<input type="submit" />
		<input type="reset" />
	</form>
</body>
</html>
```

### 3.4.8 属性规范
```markdown
元素属性值使用双引号语法
元素属性值可以写上的都写上，不要省略
```
### 3.4.9 综合案例注册页面.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>世纪佳缘-你在我也在</title>
</head>
<body>
	<table width="600" align="center">
		<caption> <h4> 青春不常在，抓紧谈恋爱 </h4></caption>
		<!-- 1 -->
		<tr>
			<td>性别</td>
			<td>
				<input type="radio"  name="sex"  checked="checked" /><img src="images/man.jpg" /> 男
				<input type="radio"  name="sex" /><img src="images/women.jpg" /> 女
			</td>
		</tr>
		<!-- 2 -->
		<tr>
			<td>生日</td>
			<td>
				<!-- 年份的 -->
				<select>
					<option>--请选择年--</option>
					<option>1995</option>
					<option>1996</option>
					<option>1997</option>
					<option>1998</option>
				</select>
				<!-- 月份的 -->
				<select>
					<option>--请选择月--</option>
					<option>1</option>
					<option>2</option>
					<option>3</option>
					<option>4</option>
				</select>
				<!-- 日子 -->
				<select>
					<option>--请选择日--</option>
					<option>1</option>
					<option>2</option>
					<option>3</option>
					<option>4</option>
				</select>
			</td>
		</tr>
		<!-- 3 -->
		<tr>
			<td>所在地区</td>
			<td>
				<input  type="text" value="北京思密达" />
			</td>
		</tr>
		<!-- 4行 -->
		<tr>
			<td>婚姻状况</td>
			<td>
				<input type="radio" name="marry" checked="checked"/> 未婚
				<input type="radio" name="marry" /> 已婚
				<input type="radio" name="marry" /> 离婚
			</td>
		</tr>
		<!-- 5行 -->
		<tr>
			<td>学历</td>
			<td>
				<input type="text" value="幼儿园">
			</td>
		</tr>
		<!-- 6行 -->
		<tr>
			<td>月薪</td>
			<td>
				<input type="text" value="10000-20000">
			</td>
		</tr>
		<!-- 7行 -->
		<tr>
			<td>手机号码</td>
			<td>
				<input type="text">
			</td>
		</tr>
		<!-- 8行 -->
		<tr>
			<td>昵称</td>
			<td>
				<input type="text" >
			</td>
		</tr>
		<!-- 9行 -->
		<tr>
			<td>喜欢的类型</td>
			<td>
				<input type="checkbox" name="love" /> 妩媚的
				<input type="checkbox" name="love" /> 可爱的
				<input type="checkbox" name="love" /> 小鲜肉
				<input type="checkbox" name="love" /> 老腊肉
				<input type="checkbox" name="love" /> 都喜欢
			</td>
		</tr>
		<!-- 10 行 -->
		<tr>
			<td>自我介绍</td>
			<td>
				<textarea> 自我介绍 </textarea>
			</td>
		</tr>
		<!-- 11行 -->
		<tr>
			<td></td>
			<td>
				<input type="image" src="images/btn.png" />
			</td>
		</tr>
		<!-- 12 行 -->
		<tr>
			<td></td>
			<td> <input type="checkbox" name="agree" checked="checked" />我同意注册条款和会员加入标准</td>
		</tr>
		<!-- 13行 -->
		<tr>
			<td></td>
			<td>
				<a href="#">我是会员，立即登录</a>
			</td>
		</tr>
		<!-- 14 -->
		<tr>
			<td></td>
			<td>
				<h3>我承诺</h3>
				<ul>
					<li>年满18岁、单身</li>
					<li>抱着严肃的态度</li>
					<li>真诚寻找另一半</li>
				</ul>
			</td>
		</tr>
	</table>
</body>
</html>

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020060505243016.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复
html即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)



