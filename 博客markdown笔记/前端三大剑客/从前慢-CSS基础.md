# CSS
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210713231416486.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1.1 CSS初识
```markdown
CSS(Cascading Style Sheets)  ，通常称为CSS样式表或层
叠样式表（级联样式表）
```
## 1.2 引入CSS样式表
```markdown
行内式（内联样式）
<标签名 style="属性1:属性值1; 属性2:属性值2; 属性3:属性值3;"> 内容 </标签名>

内部样式表（内嵌样式表）
<head>
<style type="text/CSS">
    选择器（选择的标签） { 
      属性1: 属性值1;
      属性2: 属性值2; 
      属性3: 属性值3;
    }
</style>
</head>

外部样式表（外链式）
<head>
  <link rel="stylesheet" type="text/css" href="css文件路径">
</head>
```
```html
行内样式表
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<h1 style="color: pink; font-size: 18px;">世纪佳缘，我也在这里等你思密达</h1>
	<p> 一行 </p>
</body>
</html>

内部样式表
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		h2 {
			color: green;
			font-size: 20px;
		}
		h4 {
			color: purple;
		}
		p {
			color: blue;
		}
	</style>
</head>
<body>

<h2>忆江南 唐.白居易</h2>
<p>
  江南好,风景旧曾谙。<br />
  日出江花红胜火，<br />
  春来江水绿如蓝，<br />
  能不忆江南。</p>

<h4>作者介绍</h4>
<p>白居易(772－846) ，字乐天,白居易(772－846) ，字乐天，
太原(今属山西)人。唐德宗朝进士，元和三年(808)拜左拾遗，后贬江州(今属江西)司马，移忠州(今属四川)刺史，又为苏州(今属江苏)、同州(今属陕西大荔)刺史。晚居洛阳，自号醉吟先生、香山居士。其诗政治倾向鲜明，重讽喻，尚坦易，为中唐大家。也是早期词人中的佼佼者，所作对后世影响甚大。
</p>

<h4>注释</h4>
<p>(1)据《乐府杂录》，此词又名《谢秋娘》，系唐李德裕为亡姬谢秋娘作。又名《望江南》、
《梦江南》等。分单调、双调两体。单调二十七字，双凋五十四字，皆平韵。(2)谙(音安)：熟悉。(3)蓝：蓝草，其叶可制青绿染料。</p>

<h4>品评</h4>
<p>此词写江南春色，首句“江南好”，以一个既浅切又圆活的“好”字，摄尽江南春色的种种佳处，而作者的赞颂之意与向往之情也尽寓其中。同时，唯因“好”之已甚，方能“忆”之不休，因此，此句又已暗逗结句“能不忆江南”，并与之相关阖。次句“风景旧曾谙”，点明江南风景之“好”，并非得之传闻，而是作者出牧杭州时的亲身体验与亲身感受。这就既落实了“好”字，又照应了“忆”字，不失为勾通一篇意脉的精彩笔墨。三、四两句对江南之“好”进 　行形象化的演绎，突出渲染江花、江水红绿相映的明艳色彩，给人以光彩夺目的强烈印象。其中，既有同色间的相互烘托，又有异色间的相互映衬，充分显示了作者善于著色的技巧。篇末，以“能不忆江南”收束全词，既托出身在洛阳的作者对江南春色的无限赞叹与怀念，又造成一种悠远而又深长的韵味，把读者带入余情摇漾的境界中。</p>

</body>
</html>


```
### 1.2.1 三种样式表总结

| 样式表     | 优点                     | 缺点                     | 使用情况       | 控制范围           |
| ---------- | ------------------------ | ------------------------ | -------------- | ------------------ |
| 行内样式表 | 书写方便，权重高         | 没有实现样式和结构相分离 | 较少           | 控制一个标签（少） |
| 内部样式表 | 部分结构和样式相分离     | 没有彻底分离             | 较多           | 控制一个页面（中） |
| 外部样式表 | 完全实现结构和样式相分离 | 需要引入                 | 最多，强烈推荐 | 控制整个站点（多） |

## 1.3 CSS基础选择器
```markdown
标签选择器
标签名{属性1:属性值1; 属性2:属性值2; 属性3:属性值3; } 

类选择器
.类名  {   
  属性1:属性值1; 
  属性2:属性值2; 
  属性3:属性值3;     
}

通配符选择器
* { 属性1:属性值1; 属性2:属性值2; 属性3:属性值3; }


id选择器
#id名 {属性1:属性值1; 属性2:属性值2; 属性3:属性值3; }	
```
```html
标签选择器
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			color: red;
		}
		span {
			color: green;
		}
		
	</style>
</head>
<body>
   标签选择器口诀：
	<div>标签选择器，</div>
	<div>页面同选起。</div>
	<div>直接写标签，</div>
	<div>全部不放弃。</div>
	<span>标签选择器，</span>
	<span>页面同选起。</span>
	<span>直接写标签，</span>
	<span>全部不放弃</span>
</body>
</html>

类选择器
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>类选择器</title>
	<style>
		.red {
			color: red;
		}
	</style>
</head>
<body>
   类选择器口诀：
	<div>差异化选择，</div>
	<div class="red">一个或多个。</div>
	<div>上面点定义，</div>
	<div>类名别写错。</div>
	<div>谁用谁调用，</div>
	<div>class来做。</div>
	<div class="red">嘿嘿，工作类最多。</div>
</body>
</html>

多类名
<span class="blue font100">G</span>

id选择器
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		#pink {
			color: pink;
		}
	</style>
</head>
<body>
	<p>愿你出走半生，</p>
	<p>归来仍是少年。</p>
	<p>愿我洗尽铅华，</p>
	<p id="pink">依旧笑魇如花。</p>
</body>
</html>


通配符选择器
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		* {
			color: pink;
		}
	</style>
</head>
<body>
	<div>所有的，我都要</div>
	<div>所有的，我都要</div>
	<p>所有的，我都要</p>
	<span>所有的，我都要</span>
</body>
</html>
```
```markdown
基础选择器总结
```


| 选择器       | 作用                          | 缺点                     | 使用情况   | 用法                 |
| ------------ | ----------------------------- | ------------------------ | ---------- | -------------------- |
| 标签选择器   | 可以选出所有相同的标签，比如p | 不能差异化选择           | 较多       | p { color：red;}     |
| 类选择器     | 可以选出1个或者多个标签       | 可以根据需求选择         | 非常多     | .nav { color: red; } |
| id选择器     | 一次只能选择器1个标签         | 只能使用一次             | 不推荐使用 | #nav {color: red;}   |
| 通配符选择器 | 选择所有的标签                | 选择的太多，有部分不需要 | 不推荐使用 | * {color: red;}      |

## 1.4 font字体
```markdown
font-size:大小
p {  
    font-size:20px; 
}

font-family:字体
p { 
	font-family:"微软雅黑";
	}

font-weight:字体粗细
font-style:字体风格
平时我们很少给文字加斜体，反而喜欢给斜体标签（em，i）改
为普通模式。
font-style: normal;
```
| 属性值  | 描述                                                      |
| ------- | :-------------------------------------------------------- |
| normal  | 默认值（不加粗的）                                        |
| bold    | 定义粗体（加粗的）                                        |
| 100~900 | 400 等同于 normal，而 700 等同于 bold  我们重点记住这句话 |	

| 属性   | 作用                                                    |
| ------ | :------------------------------------------------------ |
| normal | 默认值，浏览器会显示标准的字体样式  font-style: normal; |
| italic | 浏览器会显示斜体的字体样式。                            |


```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		body {
			font-size: 16px;
		}
		.title {
			/* 字体大小 */
			font-size: 20px;
			/*设置字体		*/
			font-family: Arial, "Microsoft YaHei", "微软雅黑" , "黑体";	
			/*字体加粗	*/
			/*font-weight: bold; */
			/*这个 700 一定不要跟单位  700 等价于 bold*/
			font-weight: 700; 
			/*字体倾斜*/
			font-style: italic;
		}
		h1 {
			/*让粗体的不加粗*/
			/*font-weight: normal;  400 等价于 normal*/
			font-weight: 400;
		}
		em {
			/* 让倾斜的字体 不倾斜 */
			font-style: normal;
		}
	</style>
</head>
<body>
	<p class="title">粉红色的回忆</p>
	<p>夏天夏天悄悄过去留下小秘密</p>
	<p>压心底 压心底 不能告诉你</p>
	<p>晚风春过温暖我心底 我又想起你</p>
	<p>多甜蜜 多甜蜜 怎能忘记</p>
	<h1> 韩宝仪 </h1>
	<em>韩宝仪音乐专辑</em>
</body>
</html>
```
### 1.4.1 font:综合设置字体样式
```markdown
选择器 { font: font-style  font-weight  font-size/line-height  font-family;}
使用font属性时，必须按上面语法格式中的顺序书写，不能更换顺序，
各个属性以空格隔开。
其中不需要设置的属性可以省略（取默认值），但必须
保留font-size和font-family属性，否则font属性将不起作用。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		body {
			font-size: 16px;
		}
		.title {
			/* font: font-style  font-weight  font-size/line-height  font-family;*/
			/*综合性写法*/
			font: italic 700 20px "微软雅黑";
		}
		h1 {
			/*让粗体的不加粗*/
			/*font-weight: normal;  400 等价于 normal*/
			font-weight: 400;
		}
		em {
			/* 让倾斜的字体 不倾斜 */
			font-style: normal;
		}
	</style>
</head>
<body>
	<p class="title">粉红色的回忆</p>
	<p>夏天夏天悄悄过去留下小秘密</p>
	<p>压心底 压心底 不能告诉你</p>
	<p>晚风春过温暖我心底 我又想起你</p>
	<p>多甜蜜 多甜蜜 怎能忘记</p>
	<h1> 韩宝仪 </h1>
	<em>韩宝仪音乐专辑</em>
</body>
</html>
```
### 1.4.2 font总结

| 属性        | 表示     | 注意点                                                       |
| :---------- | :------- | :----------------------------------------------------------- |
| font-size   | 字号     | 我们通常用的单位是px 像素，一定要跟上单位                    |
| font-family | 字体     | 实际工作中按照团队约定来写字体                               |
| font-weight | 字体粗细 | 记住加粗是 700 或者 bold  不加粗 是 normal 或者  400  记住数字不要跟单位 |
| font-style  | 字体样式 | 记住倾斜是 italic     不倾斜 是 normal  工作中我们最常用 normal |
| font        | 字体连写 | 1. 字体连写是有顺序的  不能随意换位置 2. 其中字号 和 字体 必须同时出现 |

## 1.5 color:文本颜色

| 表示表示       | 属性值                                  |
| :------------- | :-------------------------------------- |
| 预定义的颜色值 | red，green，blue，还有我们的御用色 pink |
| 十六进制       | #FF0000，#FF6600，#29D794               |
| RGB代码        | rgb(255,0,0)或rgb(100%,0%,0%)           |

## 1.6 text-align:文本水平对齐方式
```markdown
里面的文字内容 行内块 行内元素都可以居中对齐
```

| 属性   |       解释       |
| ------ | :--------------: |
| left   | 左对齐（默认值） |
| right  |      右对齐      |
| center |     居中对齐     |

```html
<html>
<head>
<style type="text/css">
h1 {text-align: center}
h2 {text-align: left}
h3 {text-align: right}
</style>
</head>

<body>
<h1>这是标题 1</h1>
<h2>这是标题 2</h2>
<h3>这是标题 3</h3>
</body>

</html>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/c73440a9b4504d59b4507afc2b046db2.png)

## 1.7 line-height:行间距
```markdown
一般情况下，行距比字号大7.8像素左右就可以了。
line-height: 24px;
```
## 1.8 text-indent:首行缩进
```markdown
p {
      /*行间距*/
      line-height: 25px;
      /*首行缩进2个字  em  1个em 就是1个字的大小*/
      text-indent: 2em;  
 }
```
## 1.9 text-decoration 文本的装饰

| 值           | 描述                                                  |
| ------------ | ----------------------------------------------------- |
| none         | 默认。定义标准的文本。 取消下划线（最常用）           |
| underline    | 定义文本下的一条线。下划线 也是我们链接自带的（常用） |
| overline     | 定义文本上的一条线。（不用）                          |
| line-through | 定义穿过文本下的一条线。（不常用）                    |

```html
<html>
<head>
<style type="text/css">
h1 {text-decoration: overline}
h2 {text-decoration: line-through}
h3 {text-decoration: underline}
h4 {text-decoration:blink}
a {text-decoration: none}
</style>
</head>

<body>
<h1>这是标题 1</h1>
<h2>这是标题 2</h2>
<h3>这是标题 3</h3>
<h4>这是标题 4</h4>
<p><a href="http://www.baidu.com">这是一个链接</a></p>
</body>

</html>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/ee0cf9824d21490b9dd4f49291765ef6.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

## 1.10 CSS外观属性总结

| 属性            | 表示     | 注意点                                                  |
| :-------------- | :------- | :------------------------------------------------------ |
| color           | 颜色     | 我们通常用  十六进制   比如 而且是简写形式 #fff         |
| line-height     | 行高     | 控制行与行之间的距离                                    |
| text-align      | 水平对齐 | 可以设定文字水平的对齐方式                              |
| text-indent     | 首行缩进 | 通常我们用于段落首行缩进2个字的距离   text-indent: 2em; |
| text-decoration | 文本修饰 | 记住 添加 下划线  underline  取消下划线  none           |

## 1.11 背景简写
```markdown
background: 背景颜色 背景图片地址 背景平铺 背景滚动 背景位置;
```

## 2.1 CSS复合选择器
```markdown
CSS选择器分为 基础选择器 和 复合选择器 ，但是基础选择器
不能满足我们实际开发中，快速高效的选择标签。
复合选择器是由两个或多个基础选择器，通过不同的方式组合而成的
目的是为了可以选择更准确更精细的目标元素标签。
```

### 2.1.1 后代选择器
```markdown
用来选择元素或元素组的子孙后代
```
```css
.class h3 {
	color:red;
	font-size:16px;
}
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>后代选择器</title>
	<style>
		/*常山 赵子龙*/
		/*山东 济南*/
		.nav a {
			color: pink;
		}
		.wangsicong ul li {
			color: red;
		}
	</style>
</head>
<body>
	<div class="nav">
		<a href="#">内部链接</a>
		<a href="#">内部链接</a>
		<a href="#">内部链接</a>
	</div>
	<a href="#">外部链接</a>
	<a href="#">外部链接</a>
	<a href="#">外部链接</a>
	<ul>
		<li>一条狗</li>
		<li>一条狗</li>
		<li>一条狗</li>
	</ul>
	<div class="wangsicong">
		<ul>
			<li>王可可是一条狗</li>
			<li>王可可是一条狗</li>
			<li>王可可是一条狗</li>
		</ul>
	</div>
</body>
</html>
```

### 2.1.2 子元素选择器
```markdown
子元素选择器只能选择作为某元素子元素(亲儿子)的元素。
```

```css
.class>h3 {
	color:red;
	font-size:14px;
}
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
	/*后代选择器 选择 子孙后代*/
	/*div strong {
		color: red;
	}*/
	/*子元素选择器  符号  > 只选亲儿子 这些元素 */
	div>strong {
		color: pink;
	}
	</style>
</head>
<body>
	<div>
		<strong>儿子</strong>
		<strong>儿子</strong>
		<strong>儿子</strong>
	</div>
	<div>
		<p>
			<strong>孙子</strong>
			<strong>孙子</strong>
			<strong>孙子</strong>
		</p>
	</div>
</body>
</html>
```
### 2.1.3  交集选择器
```css
交集选择器 是 并且的意思。  即...又...的意思
两个选择器之间不能有空格
比如：   p.one   选择的是： 类名为 .one  的 段落标签。  
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>交集选择器</title>
	<style>
		/*需求，就是让 p 这个 变成红色*/
		/*交集选择器  既是p标签  又是 .red 类选择器的关系*/
		p.red {
			color: red;
		}
	</style>
</head>
<body>
	<p class="red">红色</p>
	<p class="red">红色</p>
	<p class="red">红色</p>
	<div class="red">红色</div>
	<div class="red">红色</div>
	<div class="red">红色</div>
	<p>蓝色</p>
	<p>蓝色</p>
	<p>蓝色</p>
</body>
</html>
```
### 2.1.4 并集选择器
```markdown
比如  .one, p , #test {color: #F00;}  
表示   .one 和 p  和 #test 这三个选择器都会执
行颜色为红色。 
通常用于集体声明。  
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		/*客户需求： p 和 span 和 第一个div 里面的颜色都是红色*/
		/*分开写的*/
		/*p {
			color: red;
		}
		span {
			color: red;
		}*/
		/*.red {
			color: red;
		}*/
		/*并集选择器 用逗号隔开， 逗号理解为 和的意思  通常用于集体声明 就是因为这些选择器 里面的样式 相同*/
		p, 
		span,
		.red {
			color: red;
		}
		
	</style>
</head>
<body>
	<p>我和你</p>
	<p>我和你</p>
	<p>我和你</p>
	<span>我和你</span>
	<span>我和你</span>
	<span>我和你</span>
	<div class="red">我和你</div>
	<div>我和你</div>
	<h1>你和我</h1>
	<h1>你和我</h1>
</body>
</html>
```
### 2.1.5 链接伪类选择器
```markdown
a:link      /* 未访问的链接 */
a:visited   /* 已访问的链接 */
a:hover     /* 鼠标移动到链接上 */
a:active    /* 选定的链接 */
记忆法
love   hate     爱上了讨厌  
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>链接伪类选择器</title>
	<style>
		/*未访问过链接的状态 正常状态*/
		/*p.one 交集选择器*/
		a:link {
			color: #333;
			/*取消下划线*/
			text-decoration: none;
		}
		/*已经访问的链接  我们点击过*/
		a:visited {
			color: orange;
		}
		/*鼠标经过链接时候的状态*/
		a:hover {
			color: red;
		}
		/*当我们点击的时候（按下鼠标，别松开的时候）*/
		a:active {
			color: green;
		}
	</style>
</head>
<body>
	<a href="http://www.xiaomi.com">小米手机</a>
	<a href="http://www.dami.com">大米手机</a>
	<!-- p.one -->
</body>
</html>
```
 
### 2.1.6 复合选择器总结
 
| 选择器         | 作用                     | 特征                 | 使用情况 | 隔开符号及用法                          |
| -------------- | ------------------------ | -------------------- | -------- | --------------------------------------- |
| 后代选择器     | 用来选择元素后代         | 是选择所有的子孙后代 | 较多     | 符号是**空格** .nav a                   |
| 子代选择器     | 选择 最近一级元素        | 只选亲儿子           | 较少     | 符号是**>**   .nav>p                    |
| 交集选择器     | 选择两个标签交集的部分   | 既是 又是            | 较少     | **没有符号**  p.one                     |
| 并集选择器     | 选择某些相同样式的选择器 | 可以用于集体声明     | 较多     | 符号是**逗号** .nav, .header            |
| 链接伪类选择器 | 给链接更改状态           |                      | 较多     | 重点记住 a{} 和 a:hover  实际开发的写法 |


### 2.1.7 权重计算公式

| 标签选择器             | 计算权重公式 |
| ---------------------- | ------------ |
| 继承或者 *             | 0,0,0,0      |
| 每个元素（标签选择器） | 0,0,0,1      |
| 每个类，伪类           | 0,0,1,0      |
| 每个ID                 | 0,1,0,0      |
| 每个行内样式 style=""  | 1,0,0,0      |
| 每个!important  重要的 | ∞ 无穷大     |



## 2.2 标签显示模式（display）重点
```markdown
块级元素(block-level)
常见的块元素有<h1>~<h6>、<p>、<div>、<ul>、<ol>、<li>等，其中<div>标签是最典型的块元素。
（1）比较霸道，自己独占一行

（2）高度，宽度、外边距以及内边距都可以控制。

（3）宽度默认是容器（父级宽度）的100%

（4）是一个容器及盒子，里面可以放行内或者块级元素。
注意：
  只有 文字才 能组成段落  因此 p  里面不能放块级元素，特别是 p 不能放div 
  同理还有这些标签h1,h2,h3,h4,h5,h6,dt，他们都是文字类块级标签，里面不能放其他块级元素。
  
行内元素(inline-level)
常见的行内元素有<a>、<strong>、<b>、<em>、<i>、<del>、<s>、<ins>、<u>、<span>等，其中<span>标签最典型的行内元素。有的地方也成内联元素
（1）相邻行内元素在一行上，一行可以显示多个。

（2）高、宽直接设置是无效的。

（3）默认宽度就是它本身内容的宽度。

（4）行内元素只能容纳文本或则其他行内元素。

行内块元素（inline-block）
在行内元素中有几个特殊的标签——<img />、<input />、<td>，可以对它们设置宽高和对齐属性，有些资料可能会称它们为行内块元素。
（1）和相邻行内元素（行内块）在一行上,但是之间会有空白缝隙。一行可以显示多个
（2）默认宽度就是它本身内容的宽度。
（3）高度，行高、外边距以及内边距都可以控制。


行内元素/行内块元素（inline-block）的共同点:
他们之间如果没有缝隙的话，就紧挨在一起，如果有一点缝隙的
话，就不能紧挨在一起。
```
```html
块级显示模式
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			width: 200px;
			height: 200px;
			/*背景颜色 不要和 文字颜色混淆了 color*/
			background-color: pink;
		}
	</style>
</head>
<body>
	<div>我是块级元素</div>
	<div>我是块级元素</div>
	<div>
		<strong>文字</strong>
		<h1>标题</h1>
	</div>
	<!-- p里面不能包含 div  段落p h  dt 里面尽量不要放块级元素 -->
	<!-- <p>
		<div>123</div>
	</p> -->
</body>
</html>

行内显示模式
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		span {
			width: 200px;
			height: 200px;
			background-color: pink;
		}
	</style>
</head>
<body>
	<span>我是行内元素</span>
	<span>我是行内元素</span>
	<span>我是行内元素哒哒哒</span>
	<span><strong></strong></span>
<!-- 	<span>
		<div></div>
	</span> -->
	<!-- <a href="#">
		<a href="#"></a>
	</a> -->
</body>
</html>

行内块显示模式
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		img {
			width: 200px;
		}
	</style>
</head>
<body>
	<img src="images/3.jpg" alt="">
	<img src="images/3.jpg" alt="">
	<img src="images/3.jpg" alt="">
</body>
</html>
```
### 2.2.1  三种模式总结区别

| 元素模式   | 元素排列               | 设置样式               | 默认宽度         | 包含                     |
| ---------- | ---------------------- | ---------------------- | ---------------- | ------------------------ |
| 块级元素   | 一行只能放一个块级元素 | 可以设置宽度高度       | 容器的100%       | 容器级可以包含任何标签   |
| 行内元素   | 一行可以放多个行内元素 | 不可以直接设置宽度高度 | 它本身内容的宽度 | 容纳文本或则其他行内元素 |
| 行内块元素 | 一行放多个行内块元素   | 可以设置宽度和高度     | 它本身内容的宽度 |                          |


### 2.2.2 标签显示模式转换 display
```markdown
块转行内：display:inline;
行内转块：display:block;
块、行内元素转换为行内块： display: inline-block;
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		span {
			/*把行内元素转换为块级元素*/
			display: block;
			width: 100px;
			height: 100px;
			background-color: pink;
		}
		div {
			/*把块级元素转换为行内元素*/
			display: inline;
			width: 200px;
			height: 200px;
			background-color: purple;
		}
		a {
			/*转换为 行内块元素*/
			display: inline-block;
			width: 80px;
			height: 25px;
			background-color: orange;
		}
	</style>
</head>
<body>
	<span>行内</span>
	<span>行内</span>

	<div>div 是块级元素</div>
	<div>div 是块级元素</div>
	<a href="#">百度</a>
	<a href="#">新浪</a>
</body>
</html>
```
## 2.3 单行文本垂直居中
```markdown
如果 行高 等 高度  文字会 垂直居中
如果行高 大于 高度   文字会 偏下 
如果行高小于高度   文字会  偏上 
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			width: 100px;
			height: 50px;
			background-color: pink;
			line-height: 50px;
		}
	</style>
</head>
<body>
	<div> 文字垂直居中 </div>
</body>
</html>
```

## 2.4 背景
### 2.4.1 CSS 背景(background)
```markdown
背景颜色(color)
background-color:颜色值;   默认的值是 transparent  透明的

背景图片(image)
background-image : none | url (url) 
background-image : url(images/demo.png);	
我们提倡 背景图片后面的地址，url不要加引号。
```
| 参数 |              作用              |
| ---- | :----------------------------: |
| none |       无背景图（默认的）       |
| url  | 使用绝对或相对地址指定背景图像 |

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		.bg {
			width: 800px;
			height: 500px;
			background-color: pink;
			/*背景图片 1. 必须加url 2. url 里面的地址不要加 引号*/
			background-image: url(images/l.jpg);
		}
	</style>
</head>
<body>
	<div class="bg">
		12312312312312312312312312312
	</div>
</body>
</html>
```
### 2.4.2 背景平铺（repeat）
```markdown
background-repeat : repeat | no-repeat | repeat-x | repeat-y 
```
| 参数      |                 作用                 |
| --------- | :----------------------------------: |
| repeat    | 背景图像在纵向和横向上平铺（默认的） |
| no-repeat |            背景图像不平铺            |
| repeat-x  |         背景图像在横向上平铺         |
| repeat-y  |          背景图像在纵向平铺          |



```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		.bg {
			width: 800px;
			height: 500px;
			background-color: pink;
			/*背景图片 1. 必须加url 2. url 里面的地址不要加 引号*/
			background-image: url(images/l.jpg);
			/*默认的是平铺图 repeat*/
			/*background-repeat: repeat;*/
			/*背景图片不平铺*/
			/*background-repeat: no-repeat;*/
			/*横向平铺 repeat-x*/
			/*background-repeat: repeat-x;*/
			/*纵向平铺*/
			background-repeat: repeat-y;
		}
	</style>
</head>
<body>
	<div class="bg">
		12312312312312312312312312312
	</div>
</body>
</html>
```
### 2.4.3 背景位置(position)
```markdown
必须先指定background-image属性
position 后面是x坐标和y坐标。 可以使用方位名词或者 精确单位。
如果指定两个值，两个值都是方位名字，则两个值前后顺序无关，比如left  top和top  left效果一致
如果只指定了一个方位名词，另一个值默认居中对齐。
如果position 后面是精确坐标， 那么第一个，肯定是 x  第二的一定是y
如果只指定一个数值,那该数值一定是x坐标，另一个默认垂直居中
如果指定的两个值是 精确单位和方位名字混合使用，则第一个值是x坐标，第二个值是y坐标

background-position: x坐标 y坐标;
background-position: right top; 右上角
background-position: left bottom; 左下角
background-position: center center; 水平居中 垂直居中
background-position:  center left;   两个值前后顺序无关 因为是方位名词
background-position: top; 如果只指定了一个方位名词，另一个值默认居中对齐

background-position: x坐标 y坐标;
background-position: right top; 右上角
background-position: 50px 10px ; 那么第一个，肯定是 x 是 50   第二的一定是y 是 10
background-position: 10px center; 以下说明  x 10像素  y 垂直居中的
background-position: center 10px;
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		.bg {
			width: 800px;
			height: 500px;
			background-color: pink;
			/*背景图片 1. 必须加url 2. url 里面的地址不要加 引号*/
			background-image: url(images/l.jpg);
			/*背景图片不平铺*/
			background-repeat: no-repeat;
			/*背景位置*/
			/*background-position: x坐标 y坐标;*/
			/*background-position: right top; 右上角*/
			/*background-position: left bottom; 左下角*/
			/*background-position: center center; 水平居中 垂直居中*/
			/*则两个值前后顺序无关 因为是方位名词*/
			/*background-position:  center left; */
			/*如果只指定了一个方位名词，另一个值默认居中对齐*/
			background-position: top; 


		}
	</style>
</head>
<body>
	<div class="bg">
		12312312312312312312312312312
	</div>
</body>
</html>


<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		.bg {
			width: 800px;
			height: 500px;
			background-color: pink;
			/*背景图片 1. 必须加url 2. url 里面的地址不要加 引号*/
			background-image: url(images/l.jpg);
			/*背景图片不平铺*/
			background-repeat: no-repeat;
			/*背景位置*/
			/*background-position: x坐标 y坐标;*/
			/*background-position: right top; 右上角*/
			 /*那么第一个，肯定是 x 是 50   第二的一定是y 是 10*/
			/*background-position: 50px 10px ;*/
			/*以下说明  x 10像素  y 垂直居中的*/
			/*background-position: 10px center;*/
			background-position: center 10px;

		}
	</style>
</head>
<body>
	<div class="bg">
		12312312312312312312312312312
	</div>
</body>
</html>
```
### 2.4.4  背景附着
```markdown
背景附着就是解释背景是滚动的还是固定的
background-attachment : scroll | fixed 
```

| 参数   |           作用           |
| ------ | :----------------------: |
| scroll | 背景图像是随对象内容滚动 |
| fixed  |       背景图像固定       |


```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		body {
			height: 3000px;
			background-image: url(images/sms.jpg);
			background-repeat: no-repeat;
			/*这种写法一般是我们以后 超大背景图片的做法 背景定位*/
			background-position: center top;
			/*背景固定的*/
			background-attachment: fixed;
		}
		p {
			font-size: 30px;
			color: #fff;
		}
	</style>
</head>
<body>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
</body>
</html>
```
### 2.4.5 背景简写
```markdown
background: 背景颜色 背景图片地址 背景平铺 背景滚动 背景位置;
background: transparent url(image.jpg) repeat-y  scroll center top ;
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		body {
			height: 3000px;
			/*background-color: #ccc;
			background-image: url(images/sms.jpg);
			background-repeat: no-repeat;
			background-position: center top;
			background-attachment: fixed;*/
			/*background: 背景颜色 背景图片地址 背景平铺 背景滚动 背景位置;*/
			background: #ccc url(images/sms.jpg) no-repeat fixed center top;
		}
		p {
			font-size: 30px;
			color: #fff;
		}
	</style>
</head>
<body>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
	<p>天王盖地虎，小鸡炖蘑菇</p>
</body>
</html>
```
### 2.4.6 背景透明
```markdown
background: rgba(0, 0, 0, 0.3);
最后一个参数是alpha 透明度  取值范围 0~1之间
我们习惯把0.3 的 0 省略掉  这样写  background: rgba(0, 0, 0, .3);
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		.alpha {
			width: 300px;
			height: 300px;
			background: rgba(0, 0, 0, .2);
		}
	</style>
</head>
<body>

	<div class="alpha">
		哒哒哒
	</div>
</body>
</html>
```
### 2.4.7 背景总结

| 属性                  | 作用             | 值                                                           |
| --------------------- | :--------------- | :----------------------------------------------------------- |
| background-color      | 背景颜色         | 预定义的颜色值/十六进制/RGB代码                              |
| background-image      | 背景图片         | url(图片路径)                                                |
| background-repeat     | 是否平铺         | repeat/no-repeat/repeat-x/repeat-y                           |
| background-position   | 背景位置         | length/position    分别是x  和 y坐标， 切记 如果有 精确数值单位，则必须按照先X 后Y 的写法 |
| background-attachment | 背景固定还是滚动 | scroll/fixed                                                 |
| 背景简写              | 更简单           | 背景颜色 背景图片地址 背景平铺 背景滚动 背景位置;  他们没有顺序 |
| 背景透明              | 让盒子半透明     | background: rgba(0,0,0,0.3);   后面必须是 4个值              |


## 2.5 CSS层叠性
```markdown
样式冲突，遵循的原则是就近原则。那个样式离着结构近，就执行那个样式。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			color: red;
			font-size: 30px;
		}

		div {
			color: pink;
		}
	</style>
</head>
<body>
	<div>
		 长江后浪推前浪，前浪死在沙滩上。
	</div>
</body>
</html>
```
### 2.5.1 CSS继承性
```markdown
子元素可以继承父元素的样式（text-，font-，line-这些元素开
头的可以继承，以及color属性）
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			color: red;
		}
	</style>
</head>
<body>
	<div>
		<p>王思聪</p>
	</div>
</body>
</html>
```
### 2.5.2 CSS优先级
```markdown
选择器相同，则执行层叠性
选择器不同，就会出现优先级的问题。选择器不同，就会
出现优先级的问题。	
CSS权重，我们需要一套计算公式来去计算，这个就
是 CSS Specificity（特殊性）

值从左到右，左面的最大，一级大于一级，数位之间没有
进制，级别之间不可超越。 
数位之间没有进制 比如说： 0,0,0,5 + 0,0,0,5 =0,0,0,10 
而不是 0,0, 1, 0， 所以不会存在10个div能赶上一个类选
择器的情况。
```
| 标签选择器             | 计算权重公式 |
| ---------------------- | ------------ |
| 继承或者 *             | 0,0,0,0      |
| 每个元素（标签选择器） | 0,0,0,1      |
| 每个类，伪类           | 0,0,1,0      |
| 每个ID                 | 0,1,0,0      |
| 每个行内样式 style=""  | 1,0,0,0      |
| 每个!important  重要的 | ∞ 无穷大     |


```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		/*div {
			color: red;
		}*/
		/*标签选择器 权重 0,0,0,1  小组长*/
		div {
			color: pink!important;
		}
		/*类选择器 权重 0,0,1,0 班长*/
		.one {
			color: blue;
		}
		/*id 选择器 权重 0,1,0,0  班主任*/
		#two {
			color: green;
		}
		/*style= 行内样式表 权重 1,0,0,0  校长*/
		/*!important 在样式属性的后面添加 权重最高 ∞  教育局局长*/

	</style>
</head>
<body>
	<div class="one" id="two" style="color: yellow;"> 权重还有30秒到达战场 </div>
</body>
</html>
```
### 2.5.3 权重叠加

| 标签选择器             | 计算权重公式 |
| ---------------------- | ------------ |
| 继承或者 *             | 0,0,0,0      |
| 每个元素（标签选择器） | 0,0,0,1      |
| 每个类，伪类           | 0,0,1,0      |
| 每个ID                 | 0,1,0,0      |
| 每个行内样式 style=""  | 1,0,0,0      |
| 每个!important  重要的 | ∞ 无穷大     |


```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		/*出现了权重叠加的现象 */
		/*.nav a 权重 0,0,1,0  + 0,0,0,1  = 0,0,1,1 */
		.nav a {
			color: red;
		}
		/*.first  权重 0,0,1,0*/
		/*.first {
			color: pink;
		}*/
		/*0020*/
		.nav .first {
			color: pink;
		}


		/*0,0,0,5  + 
		0,0,0,5  =
		0,0,0,10*/
	</style>
</head>
<body>
	<div> 人生四大悲 </div>
	<div class="nav">
		<a href="#" class="first">家里没宽带</a>
		<a href="#">网速不够快</a>
		<a href="#">手机没流量</a>
		<a href="#">学校无wifi</a>
	</div>
</body>
</html>
```

## 3.1 盒子模型
```markdown
盒子边框（border）
border : border-width || border-style || border-color 
边框的样式：
none：没有边框即忽略所有边框的宽度（默认值）
solid：边框为单实线(最为常用的)
dashed：边框为虚线  
dotted：边框为点线
border: 1px solid red;  没有顺序  
```

| 属性         |          作用          |
| ------------ | :--------------------: |
| border-width | 定义边框粗细，单位是px |
| border-style |       边框的样式       |
| border-color |        边框颜色        |


### 3.1.1 盒子边框总结表
```markdown
很多情况下，我们不需要指定4个边框，我们是可以单
独给4个边框分别指定的。
```

| 上边框                     | 下边框                        | 左边框                      | 右边框                       |
| :------------------------- | :---------------------------- | :-------------------------- | :--------------------------- |
| border-top-style:样式;     | border-bottom-style:样式;     | border-left-style:样式;     | border-right-style:样式;     |
| border-top-width:宽度;     | border- bottom-width:宽度;    | border-left-width:宽度;     | border-right-width:宽度;     |
| border-top-color:颜色;     | border- bottom-color:颜色;    | border-left-color:颜色;     | border-right-color:颜色;     |
| border-top:宽度 样式 颜色; | border-bottom:宽度 样式 颜色; | border-left:宽度 样式 颜色; | border-right:宽度 样式 颜色; |


```html
盒子边框
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			width: 200px;
			height: 200px;
			/*边框的宽度 实际开发中都是跟 px 单位*/
			/*border-width: 5px;*/
			/*实线的*/
			/*border-style: solid;*/
			/*虚线的  dashed  大师的 说话都很虚*/
			/*border-style: dashed;*/
			/*点线*/
			/*border-style: dotted;*/
			/*border-color: pink;*/
			/*边框的综合性写法*/
			/*边框粗细 边框 样式  边框颜色*/
			border: 5px dotted  pink;
		}
	</style>
</head>
<body>
	<div> </div>
</body>
</html>

分别指定边框
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			width: 200px;
			height: 400px;
			/*上边框写法*/
			border-top: 2px solid red;
			border-left: 1px solid green;
			border-right: 1px solid blue;
			border-bottom: 1px solid pink;
		}
		input {
			/*border-top: none;
			border-left: none;
			border-right: none;
			border-bottom: 1px dashed red;*/
			/*四个边框都去掉 先写大的，后写小的*/
			border: none;
			border-bottom: 1px dashed red;

		}
	</style>
</head>
<body>
	<div> </div>
	用户名: <input type="text" > <br>
	密码: <input type="text" >
</body>
</html>
```
### 3.1.2 表格的细线边框
```markdown
通过表格的cellspacing="0",将单元格与单元格之间的距离设置为0，
但是两个单元格之间的边框会出现重叠，从而使边框变粗
通过css属性：
table{ border-collapse:collapse; }  

<style>
	table {
		width: 500px;
		height: 300px;
		border: 1px solid red;
	}
	td {
		border: 1px solid red;
		text-align: center;
	}
	table, td {
		border-collapse: collapse;  /*合并相邻边框*/
	}
</style>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>小说排行榜</title>
	<style>
		table,
		td,
		th {
			border: 1px solid deeppink;
			/*让我们的表格 单元格 th 合并相邻的边框*/
			border-collapse: collapse;
		}
		.hotpink {
			background-color: hotpink;
		}
		.pink {
			background-color: pink;
		}
	</style>
</head>
<body>
	<table cellspacing="0" width="500" height="249" align="center">
		<caption> <h3>小说排行榜</h3> </caption>
		<tr class="hotpink">
			<th>排名</th>
			<th>关键词</th>
			<th>趋势</th>
			<th>今日搜索</th>
			<th>最近七日</th>
			<th>相关链接</th>
		</tr>
		<tr>
			<td>1</td>
			<td>鬼吹灯</td>
			<td>
				<img src="images/up.jpg">
			</td>
			<td>356</td>
			<td>3560</td>
			<td>
				<a href="#">贴吧</a>
				<a href="#">图片</a>
				<a href="#">百科</a>
			</td>
		</tr>
		<tr class="pink">
			<td>1</td>
			<td>鬼吹灯</td>
			<td>
				<img src="images/down.jpg">
			</td>
			<td>356</td>
			<td>3560</td>
			<td>
				<a href="#">贴吧</a>
				<a href="#">图片</a>
				<a href="#">百科</a>
			</td>
		</tr>
		<tr>
			<td>1</td>
			<td>鬼吹灯</td>
			<td>
				<img src="images/up.jpg">
			</td>
			<td>356</td>
			<td>3560</td>
			<td>
				<a href="#">贴吧</a>
				<a href="#">图片</a>
				<a href="#">百科</a>
			</td>
		</tr>
		<tr class="pink">
			<td>1</td>
			<td>鬼吹灯</td>
			<td>1</td>
			<td>356</td>
			<td>3560</td>
			<td>
				<a href="#">贴吧</a>
				<a href="#">图片</a>
				<a href="#">百科</a>
			</td>
		</tr>
		<tr>
			<td>1</td>
			<td>鬼吹灯</td>
			<td>1</td>
			<td>356</td>
			<td>3560</td>
			<td>
				<a href="#">贴吧</a>
				<a href="#">图片</a>
				<a href="#">百科</a>
			</td>
		</tr>
		<tr class="pink">
			<td>1</td>
			<td>鬼吹灯</td>
			<td>1</td>
			<td>356</td>
			<td>3560</td>
			<td>
				<a href="#">贴吧</a>
				<a href="#">图片</a>
				<a href="#">百科</a>
			</td>
		</tr>
		<tr>
			<td>1</td>
			<td>鬼吹灯</td>
			<td>1</td>
			<td>356</td>
			<td>3560</td>
			<td>
				<a href="#">贴吧</a>
				<a href="#">图片</a>
				<a href="#">百科</a>
			</td>
		</tr>
	</table>
</body>
</html>
```

### 3.1.3 内边距（padding）
```markdown
padding属性用于设置内边距。 是指 边框与内容之间的距离。
当我们给盒子指定padding值之后， 发生了2件事情：
1. 内容和边框 有了距离，添加了内边距。
2. 盒子会变大了。
```
| 属性           | 作用     |
| -------------- | :------- |
| padding-left   | 左内边距 |
| padding-right  | 右内边距 |
| padding-top    | 上内边距 |
| padding-bottom | 下内边距 |

| 值的个数 | 表达意思                                        |
| -------- | ----------------------------------------------- |
| 1个值    | padding：上下左右内边距;                        |
| 2个值    | padding: 上下内边距    左右内边距 ；            |
| 3个值    | padding：上内边距   左右内边距   下内边距；     |
| 4个值    | padding: 上内边距 右内边距 下内边距 左内边距 ； |


```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>内边距</title>
	<style>
		div {
			width: 200px;
			height: 200px;
			border: 1px solid red;
			/*左内边距*/
			/*padding-left: 10px;
			padding-top: 30px;*/
			/*padding 简写 复合写法*/
			/*padding: 20px; 上下左右 都是 20 内边距*/
			/*padding: 10px 20px; 上下10  左右 20 内边距*/
			/*padding: 10px 20px 30px; 上 10  左右 20  下 30 内边距*/
			/*padding: 10px 20px 30px 40px;  上10 右 20  下 30 左 40 顺时针*/

		}
	</style>
</head>
<body>
	<div> 王者农药 </div>
</body>
</html>
```

### 3.1.4 内盒尺寸计算（元素实际大小）
```markdown
盒子的实际的大小 =   内容的宽度和高度 +  内边距   +  边框   

解决padding跟宽高撑大盒子的问题：先量下盒子的总宽高，然
后减去这两部分，剩下的就是内容的宽高了。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>计算盒子实际的大小</title>
	<style>
		div {
			width: 180px;
			height: 200px;
			background-color: pink;
			/*添加10px 内边距 左右 上下*/
			padding: 10px;
			/*盒子的实际大小 =  内容宽度 高度 +  内边距 +  边框*/
			      /*           =   200  +  20  +  0
			                 =   220  */
             /*解决的方法：
                内边距一定要给的， 我们只能改变 内容宽度 width 让他减去 多出来的内边距就可以了*/
                /*200 - 20  =  180 */
		}
	</style>
</head>
<body>
	<div> </div>
</body>
</html>
```
### 3.1.5 padding不影响盒子大小情况
```markdown
如果没有给一个盒子指定宽度， 此时，如果给这个盒子指
定padding， 则不会撑开盒子。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			width: 200px;
			height: 200px;
			background-color: pink;
		}
		p {	
			/*width: 200px;*/
			height: 30px;
			background-color: purple;
			padding-left: 30px;
			/*特殊情况， 如果这个盒子啊，没有宽度 则padding 不会撑开盒子*/
		}
	</style>
</head>
<body>
	<div> 
		<p>哒哒哒</p>
	</div>
</body>
</html>
```
### 3.1.6 外边距（margin）
```markdown
margin属性用于设置外边距。  margin就是控制盒子和盒
子之间的距离
```

| 属性          | 作用     |
| ------------- | :------- |
| margin-left   | 左外边距 |
| margin-right  | 右外边距 |
| margin-top    | 上外边距 |
| margin-bottom | 下外边距 |



```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			width: 200px;
			height: 200px;
			background-color: pink;
			padding: 20px;
			/*外边距*/
			/*margin-left: 100px;
			margin-top: 50px;*/
			margin: 100px 20px 0 10px;
		}
	</style>
</head>
<body>
	<div></div>
</body>
</html>
```

### 3.1.7 块级盒子水平居中及文本居中
```markdown
margin: 0 auto;  /* 块级盒子水平居中  */
text-align: center; /*  文字、 行内元素 、行内块元素水平居中 */
```
```html
块级盒子居中对齐
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			width: 600px;
			height: 400px;
			background-color: pink;
			/*让块级盒子居中对齐水平 1. 必须有宽度 2. 左右外边距设置为auto*/
			/*1. margin-left: auto;
			margin-right: auto;*/
			/*2. margin: auto;*/
			margin: 0 auto;
		}
	</style>
</head>
<body>
	<div></div>
</body>
</html>

文字居中和盒子居中
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			width: 600px;
			height: 300px;
			background-color: pink;
			/*块级盒子水平居中*/
			margin: 50px auto;
			/*盒子里面的文字 行内元素 、行内块居中对齐水平居中*/
			text-align: center;
		}
	</style>
</head>
<body>
	<div> 稳住 <strong>我们能赢</strong> <input type="text"> </div>
</body>
</html>
```
### 3.1.8 插入图片和背景图片区别
```css
插入图片 我们用的最多 比如产品展示类  移动位置只能靠盒模型 padding margin
背景图片我们一般用于小图标背景 或者 超大背景图片  背景图片 只能通过  background-position

 img {  
		width: 200px;/* 插入图片更改大小 width 和 height */
		height: 210px;
		margin-top: 30px;  /* 插入图片更改位置 可以用margin 或padding  盒模型 */
		margin-left: 50px; /* 插入当图片也是一个盒子 */
	}

 div {
		width: 400px;
		height: 400px;
		border: 1px solid purple;
		background: #fff url(images/sun.jpg) no-repeat;
		background-position: 30px 50px; /* 背景图片更改位置 我用 background-position */
	}
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		.pic,
		.bg {
			width: 500px;
			height: 500px;
			border: 1px solid red;
			/*padding: 30px;*/
		}
		.pic img {
			margin: 30px;
		}
		.bg {
			background: url(images/3.jpg) no-repeat;
			background-position: 30px 30px;
			/*padding: 30px;*/
		}
	</style>
</head>
<body>
	<div class="pic">
		<img src="images/3.jpg" alt="">
	</div>
	<div class="bg">
		
	</div>
</body>
</html>
```
### 3.1.9 清除元素的默认内外边距
```markdown
* {
   padding:0;         /* 清除内边距 */
   margin:0;          /* 清除外边距 */
}
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
	  /*这是一个神奇的代码  我们以后写css 第一句话*/
	  * {
	  	margin: 0;
	  	padding: 0;
	  }
	  span {
	  	 margin: 30px;
	  }
	</style>
</head>
<body>
	一个问题
	<p>又是一个问题</p>
	<span>  行内元素 尽量只设置左右内外边距， 不要设置上下内外边距。</span>
</body>
</html>
```
### 3.1.10 盒子塌陷问题
```markdown
1. 相邻块元素垂直外边距的合并（标准流的盒子）
当上下相邻的两个块元素相遇时，如果上面的元素有下
外边距margin-bottom
下面的元素有上外边距margin-top，则他们之间的垂
直间距不是margin-bottom与margin-top之和
取两个值中的较大者这种现象被称为相邻块元素垂直外边
距的合并(也称外边距塌陷)

解决方案:
尽量给只给一个盒子添加margin值。
```
.

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200502214718340.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
2.嵌套块元素垂直外边距的合并（塌陷）（标准流的盒子）
对于两个嵌套关系的块元素，如果父元素没有上内边距或者上边框
父元素的上外边距会与子元素的上外边距发生合并
合并后的外边距为两者中的较大者
解决方案：
1. 可以为父元素定义上边框。
2. 可以为父元素定义上内边距
3. 可以为父元素添加overflow:hidden.
4. 还有其他方法，比如浮动、固定、绝对定位的盒子不会有问
题，后面咱们再总结
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200624202739843.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```html
外边距合并-上下外边距
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		.top,
		.bottom {
			width: 200px;
			height: 200px;
			background-color: pink;
		}
		.top {
			margin-bottom: 100px;
		}
		.bottom {
			background-color: purple;
			margin-top: 50px;
		}
	</style>
</head>
<body>
	<div class="top"></div>
	<div class="bottom"></div>
</body>
</html>

嵌套关系外边距合并
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		.father {
			width: 500px;
			height: 500px;
			background-color: pink;
			/*嵌套关系 垂直外边距合并  解决方案 */
			/*1. 可以为父元素定义上边框  transparent 透明*/
			/*border-top: 1px solid transparent;*/
			/*2. 可以给父级指定一个 上 padding值*/
			/*padding-top: 1px; */
			/*3. 可以为父元素添加overflow:hidden。*/
			overflow: hidden;
		}
		.son {
			width: 200px;
			height: 200px;
			background-color: purple;
			margin-top: 100px;
		}
	</style>
</head>
<body>
	<div class="father">
		<div class="son"></div>
	</div>
</body>
</html>
```
### 3.1.11 盒子模型布局稳定性
```markdown
我们根据稳定性来分，建议如下：
按照 优先使用  宽度 （width）  其次 使用内边距（padding）    再次  外边距（margin）。

原因：
margin 会有外边距合并 还有 ie6下面margin 加倍的bug（讨厌）所以最后使用。
padding  会影响盒子大小， 需要进行加减计算（麻烦） 其次使用。
width   没有问题（嗨皮）我们经常使用宽度剩余法 高度剩余法来做。
```
### 3.1.12 圆角边框
```markdown
让一个正方形  变成圆圈 
border-radius: 50%;
让一个长方形变成圆角边框:border-radius:0.5height.
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			width: 200px;
			height: 200px;
			background-color: pink;

			/*border-radius: 100px;*/
			border-radius: 50%;
		}
		p {
			width: 100px;
			height: 20px;
			background-color: red;
			font-size: 12px;
			color: #fff;
			text-align: center;
			line-height: 20px;
			border-radius: 10px;
		}
	</style>
</head>
<body>
	<div> </div>
	<p> 特价 免费送 </p>
</body>
</html>
```
### 3.1.13 圆角矩形可以为4个角分别设置圆度， 但是是有顺序的
```markdown
border-top-left-radius:20px;
border-top-right-radius:20px;
border-bottom-right-radius:20px;
border-bottom-left-radius:20px;
```
### 3.1.14 盒子阴影
```markdown
box-shadow:水平阴影 垂直阴影 模糊距离（虚实）  阴影尺寸（影子大小）  阴影颜色  内/外阴影；
即x方向的阴影，y方向的阴影，虚实程度，影子的大小，影子的颜色，内外阴影（默认外阴影） 
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200503082758333.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			width: 200px;
			height: 200px;
			background-color: pink;
			margin: 50px auto;
			/*box-shadow:水平阴影 垂直阴影 模糊距离（虚实）  阴影尺寸（影子大小）  阴影颜色  内/外阴影；*/
			box-shadow: 0 15px 30px rgba(0,0,0,.3);
		}
	</style>
</head>
<body>
	<div></div>
</body>
</html>
```

### 3.1.15 注意父盒子跟子盒子的宽度高度问题
```markdown
没有给定宽度的子盒子，给定padding值不会撑开父盒子.
没有设置高度的父盒子，子盒子的高度撑起父盒子的高度。这里
指的子盒子是标准流的盒子。
没有给定宽度的块级子盒子，默认宽度是块级父盒子的宽度。
行内元素跟行内块元素默认由内容撑起宽度跟高度。
```
## 4.1 CSS 布局的三种机制
```markdown
CSS 提供了 3 种机制来设置盒子的摆放位置，分别是普通流 、
浮动和定位，其中：

1. 普通流（标准流）
2. 浮动
   让盒子从普通流中浮起来 —— 让多个盒子(div)水平排列成一行。
3. 定位
   将盒子在某一个位置  自由的漂浮在其他盒子的上面  —— CSS 离不开
   定位，特别是后面的 js 特效。
```
```html
普通标准流
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			height: 100px;
			background-color: pink;
			margin-top: 10px;
		}
	</style>
</head>
<body>
	<div>123</div>
	<div>abc</div>
	<span>行内元素</span>
	<span>行内元素</span>
	<span>行内元素</span>
	<span>行内元素</span>
	<span>行内元素</span>
	<span>行内元素</span>
	<span>行内元素</span>
	<span>行内元素</span>
	<span>行内元素</span>
</body>
</html>


```
### 4.1.1 浮动
```markdown
float的常见取值;none/left/right
浮动有以下规则:
1.元素一旦浮动后,脱离标准流,朝着向左或向右方
向移动，直到自己的边界紧贴着包含块（一般是父元素）或
者其他浮动元素的边界为止
2.浮动压不住文字还有图片，所以可以用定位来解决
3.浮动对行内元素或者行内块元素无效，只对块级元
素有效
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		img {
			/*左浮动*/
			/*float: left;*/
			float: right;
			margin: 10px;
		}
	</style>
</head>
<body>
	1993年，在古装片《战神传说》中扮演一个武功超群的渔民；同年，主演动作喜剧片《至尊三十六计之偷天换日》，在片中饰演赌术高明的千门高手钱文迪；此外，他还主演了爱情片《天长地久》，在片中塑造了一个风流不羁的江湖浪子形象。

1994年，刘德华投资并主演了剧情片《天与地》，在片中饰演面对恶势力却毫不退缩的禁毒专员张一鹏。1995年，主演赛车励志片《烈火战车》，在片中饰演叛逆、倔强的阿祖，并凭借该片获得第15届香港电影金像奖最佳男主角提名；同年在动作片《大冒险家》中演绎了立仁从小时候父母双亡到长大后进入泰国空军的故事。
<img src="images/img.jpg" alt="">
1996年，主演黑帮题材的电影《新上海滩》，在片中饰演对冯程程痴情一片的丁力。1997年，担任剧情片《香港制造》的制作人；同年，主演爱情片《天若有情之烽火佳人》，在片中饰演家世显赫的空军少尉刘天伟；12月，与梁家辉联袂主演警匪动作片《黑金》，在片中饰演精明干练、嫉恶如仇的调查局机动组组长方国辉。1998年，主演动作片《龙在江湖》，饰演重义气的黑帮成员韦吉祥；同年，出演喜剧片《赌侠1999》；此外，他还担任剧情片《去年烟花特别多》的制作人。
1993年，在古装片《战神传说》中扮演一个武功超群的渔民；同年，主演动作喜剧片《至尊三十六计之偷天换日》，在片中饰演赌术高明的千门高手钱文迪；此外，他还主演了爱情片《天长地久》，在片中塑造了一个风流不羁的江湖浪子形象。

1994年，刘德华投资并主演了剧情片《天与地》，在片中饰演面对恶势力却毫不退缩的禁毒专员张一鹏。1995年，主演赛车励志片《烈火战车》，在片中饰演叛逆、倔强的阿祖，并凭借该片获得第15届香港电影金像奖最佳男主角提名；同年在动作片《大冒险家》中演绎了立仁从小时候父母双亡到长大后进入泰国空军的故事。


1996年，主演黑帮题材的电影《新上海滩》，在片中饰演对冯程程痴情一片的丁力。1997年，担任剧情片《香港制造》的制作人；同年，主演爱情片《天若有情之烽火佳人》，在片中饰演家世显赫的空军少尉刘天伟；12月，与梁家辉联袂主演警匪动作片《黑金》，在片中饰演精明干练、嫉恶如仇的调查局机动组组长方国辉。1998年，主演动作片《龙在江湖》，饰演重义气的黑帮成员韦吉祥；同年，出演喜剧片《赌侠1999》；此外，他还担任剧情片《去年烟花特别多》的制作人。
</body>
</html>
```
#### 4.1.1.1 浮动(float)小结

| 特点 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| 浮   | 加了浮动的盒子**是浮起来**的，漂浮在其他标准流盒子的上面。   |
| 漏   | 加了浮动的盒子**是不占位置的**，它原来的位置**漏给了标准流的盒子**。 |
| 特   | **特别注意**：浮动元素会改变display属性， 类似转换为了行内块，但是元素之间没有空白缝隙 |


```html
浮动口诀之浮漏
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>浮动口诀之浮</title>
	<style>
		.one {
			/*不是标准流了， 漂浮起来， 浮在 标准流的上面*/
			float: left;
			width: 200px;
			height: 200px;
			background-color: pink;
		}
		.two {
			width: 300px;
			height: 300px;
			background-color: purple;
		}
	</style>
</head>
<body>
	<div class="one"></div>
	<div class="two"></div>
</body>
</html>

浮动口诀之特
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>浮动口诀之特</title>
	<style>
		/*这个盒子div 添加浮动之后，不再是 块级元素 默认相当于类似转换为了 inline-block*/
		div {
			float: left;
			height: 100px;
			background-color: pink;
		}
	</style>
</head>
<body>
	<div>
		天王盖地虎，小鸡炖蘑菇
	</div>
</body>
</html>
```
#### 4.1.1.2 浮动和标准流的父盒子搭配
```markdown
我们知道，浮动是脱标的，会影响下面的标准流元素，此时，
我们需要给浮动的元素添加一个标准流的父亲，这样，最大
化的减小了对其他标准流的影响。
浮动元素与父盒子的关系：
子盒子的浮动参照父盒子对齐
不会与父盒子的边框重叠，也不会超过父盒子的内边距
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020050308474781.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 4.1.1.3 浮动元素与兄弟盒子的关系
```markdown
在一个父级盒子中，如果前一个兄弟盒子是：
浮动的，那么当前盒子会与前一个盒子的顶部对齐；
普通流的，那么当前盒子会显示在前一个兄弟盒子的下方。 
浮动只会影响当前的或者是后面的标准流盒子，不会影
响前面的标准流。

如果一个盒子里面有多个子盒子，如果其中一个盒子浮动
了，其他兄弟也应该浮动。防止引起问题
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200625012737552.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 4.1.1.4 清除浮动
```markdown
因为父级盒子很多情况下，不方便给高度，但是子盒子浮动就
不占有位置，最后父级盒子高度为0，就影响了下面的标准流盒子。

由于浮动元素不再占用原文档流的位置，所以它会对后面
的元素排版产生影响
准确地说，并不是清除浮动，而是**清除浮动后造成的影响

清除浮动本质
清除浮动主要为了解决父级元素因为子级浮动引起内部高
度为0 的问题。清除浮动之后， 父级就会根据浮动的子盒
子自动检测高度。父级有了高度，就不会影响下面的标准流了
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/202006250130127.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200625013024970.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```html
标准流不会影响盒子布局
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		/*one是父级，但是很多情况下，不方便给高度 子盒子有多少内容，就显示多少内容，自动撑开父亲最为合理*/
		.one {
			width: 500px;
			background-color: pink;
		}
		.damao {
			width: 200px;
			height: 200px;
			background-color: purple;
		}
		.ermao {
			width: 250px;
			height: 250px;
			background-color: skyblue;
		}
		.two {
			width: 700px;
			height: 150px;
			background-color: #000;
		}
	</style>
</head>
<body>
	<div class="one">
		<div class="damao"></div>
		<div class="ermao"></div>
	</div>
	<div class="two"></div>
</body>
</html>

浮动会影响盒子布局
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		/*很多情况下，我们的父盒子，不方便给高度  根据内容撑开，有多少内容，我的父盒子就有多高*/
		.one {
			width: 500px;
			background-color: pink;
		}
		/*因为damao 二毛 浮动了，不占有位置， 而父级又没有高度， 所以two 就到底下去了*/
		.damao {
			float: left;
			width: 200px;
			height: 200px;
			background-color: purple;
		}
		.ermao {
			float: left;
			width: 250px;
			height: 250px;
			background-color: skyblue;
		}
		.two {
			width: 700px;
			height: 150px;
			background-color: #000;
		}
	</style>
</head>
<body>
	<div class="one">
		<div class="damao"></div>
		<div class="ermao"></div>
	</div>
	<div class="two"></div>
</body>
</html>
```
#### 4.1.1.5 清除浮动的方法
```markdown
在CSS中，clear属性用于清除浮动，在这里，我们先
记住清除浮动的方法，具体的原理，等我们学完cs会再回头分析。
```

```markdown
选择器{clear:属性值;}   clear 清除  
```
```markdown
1.额外标签法(隔墙法)
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		/*很多情况下，我们的父盒子，不方便给高度  根据内容撑开，有多少内容，我的父盒子就有多高*/
		.one {
			width: 500px;
			/*background-color: pink;*/
			border: 1px  solid red;
		}
		/*因为damao 二毛 浮动了，不占有位置， 而父级又没有高度， 所以two 就到底下去了*/
		.damao {
			float: left;
			width: 200px;
			height: 200px;
			background-color: purple;
		}
		.ermao {
			float: left;
			width: 250px;
			height: 250px;
			background-color: skyblue;
		}
		.two {
			width: 700px;
			height: 150px;
			background-color: #000;
		}
		/*清除浮动*/
		.clear {
			clear: both;
		}
	</style>
</head>
<body>
	<div class="one">
		<div class="damao"></div>
		<div class="ermao"></div>
		<div class="clear"></div>
	</div>
	<div class="two"></div>
</body>
</html>
```
```markdown
优点： 通俗易懂，书写方便	
缺点： 添加许多无意义的标签，结构化较差。
```
```markdown
2. 父级添加overflow属性方法
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		/*很多情况下，我们的父盒子，不方便给高度  根据内容撑开，有多少内容，我的父盒子就有多高*/
		.one {
			width: 500px;
			/*background-color: pink;*/
			border: 1px  solid red;
			/*给父级添加 overflow 就可以清除浮动*/
			overflow: hidden;
		}
		/*因为damao 二毛 浮动了，不占有位置， 而父级又没有高度， 所以two 就到底下去了*/
		.damao {
			float: left;
			width: 200px;
			height: 200px;
			background-color: purple;
		}
		.ermao {
			float: left;
			width: 250px;
			height: 250px;
			background-color: skyblue;
		}
		.two {
			width: 700px;
			height: 150px;
			background-color: #000;
		}

	</style>
</head>
<body>
	<div class="one">
		<div class="damao"></div>
		<div class="ermao"></div>
	</div>
	<div class="two"></div>
</body>
</html>
```
```markdown
优点：  代码简洁

缺点：  内容增多时候容易造成不会自动换行导致内容被
隐藏掉，无法显示需要溢出的元素。
```
```markdown
3. 使用after伪元素清除浮动
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		/*声明清除浮动的样式*/
		.clearfix:after {
			content: "";
			display: block;
			height: 0;
			visibility: hidden;
			clear: both;
		}
		.clearfix {
			*zoom: 1;  /*ie6,7 专门清除浮动的样式*/
		}
		/*很多情况下，我们的父盒子，不方便给高度  根据内容撑开，有多少内容，我的父盒子就有多高*/
		.one {
			width: 500px;
			/*background-color: pink;*/
			border: 1px  solid red;
			
		}
		/*因为damao 二毛 浮动了，不占有位置， 而父级又没有高度， 所以two 就到底下去了*/
		.damao {
			float: left;
			width: 200px;
			height: 200px;
			background-color: purple;
		}
		.ermao {
			float: left;
			width: 250px;
			height: 250px;
			background-color: skyblue;
		}
		.two {
			width: 700px;
			height: 150px;
			background-color: #000;
		}

	</style>
</head>
<body>
	<div class="one clearfix">
		<div class="damao"></div>
		<div class="ermao"></div>
	</div>
	<div class="two"></div>
</body>
</html>
```
```markdown
优点： 符合闭合浮动思想  结构语义化正确
缺点： 由于IE6-7不支持:after，使用 zoom:1触发 hasLayout。
```
```markdown
4..使用双伪元素清除浮动
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		/*声明清除浮动的样式*/
		.clearfix:before,
		.clearfix:after {
			content: "";
			display: table;
		}
		.clearfix:after {
			clear: both;
		}
		.clearfix {
			*zoom: 1;
		}
		/*很多情况下，我们的父盒子，不方便给高度  根据内容撑开，有多少内容，我的父盒子就有多高*/
		.one {
			width: 500px;
			/*background-color: pink;*/
			border: 1px  solid red;
			
		}
		/*因为damao 二毛 浮动了，不占有位置， 而父级又没有高度， 所以two 就到底下去了*/
		.damao {
			float: left;
			width: 200px;
			height: 200px;
			background-color: purple;
		}
		.ermao {
			float: left;
			width: 250px;
			height: 250px;
			background-color: skyblue;
		}
		.two {
			width: 700px;
			height: 150px;
			background-color: #000;
		}

	</style>
</head>
<body>
	<div class="one clearfix">
		<div class="damao"></div>
		<div class="ermao"></div>
	</div>
	<div class="two"></div>
</body>
</html>
```
```markdown
优点：  代码更简洁

缺点：  由于IE6-7不支持:after，使用 zoom:1触发 hasLayout。

代表网站： 小米、腾讯等
```
#### 4.1.1.6 清除浮动总结
```markdown
1. 父级没高度
2. 子盒子浮动了
3. 影响下面布局了，我们就应该清除浮动了。
```
| 清除浮动的方式       | 优点               | 缺点                               |
| -------------------- | :----------------- | :--------------------------------- |
| 额外标签法（隔墙法） | 通俗易懂，书写方便 | 添加许多无意义的标签，结构化较差。 |
| 父级overflow:hidden; | 书写简单           | 溢出隐藏                           |
| 父级after伪元素      | 结构语义化正确     | 由于IE6-7不支持:after，兼容性问题  |
| 父级双伪元素         | 结构语义化正确     | 由于IE6-7不支持:after，兼容性问题  |

#### 4.1.1.7 常见的图片格式
```markdown
1. jpg图像格式： 
JPEG（.JPG）对色彩的信息保留较好，高清，颜色较多，我
们产品类的图片经常用jpg格式的
2. gif图像格式：
GIF格式最多只能储存256色，所以通常用来显示简单图形及
字体，但是可以保存透明背景和动画效果
3. png图像格式
是一种新兴的网络图形格式，结合了GIF和JPEG的优点，具
有存储形式丰富的特点，能够保持透明背景
4. PSD图像格式
PSD格式是Photoshop的专用格式，里面可以存放图层、通
道、遮罩等多种设计草稿。 `
```
### 4.2.1 定位

#### 4.2.1.1 标准流
~~~markdown
标准流就是从左到右,上到下的方式排版的流,默认情况下不
存在层叠现象.
脱离标准流的特点:
可以随意设置完高
宽高默认由内容决定
不再受标准流的约束
不再给父元素汇报宽高数据

标准流在最底层 (海底)  -------    浮动 的盒子 在 中间层  (海面)  -------   定位的盒子 在 最上层  （天空）
~~~

#### 4.2.1.2  定位详解
```markdown
定位 = 定位模式 + 边偏移
边偏移需要和定位模式联合使用，单独使用无；
top和 bottom 不要同时使用；
left 和 right不要同时使用。
```

#### 4.2.1.3 边偏移

| 边偏移属性 | 示例           | 描述                                                     |
| ---------- | :------------- | -------------------------------------------------------- |
| `top`      | `top: 80px`    | **顶端**偏移量，定义元素相对于其父元素**上边线的距离**。 |
| `bottom`   | `bottom: 80px` | **底部**偏移量，定义元素相对于其父元素**下边线的距离**。 |
| `left`     | `left: 80px`   | **左侧**偏移量，定义元素相对于其父元素**左边线的距离**。 |
| `right`    | `right: 80px`  | **右侧**偏移量，定义元素相对于其父元素**右边线的距离**   |


#### 4.2.1.4 定位模式
 
| 值         |     语义     |
| ---------- | :----------: |
| `static`   | **静态**定位 |
| `relative` | **相对**定位 |
| `absolute` | **绝对**定位 |
| `fixed`    | **固定**定位 |

#### 4.2.1.5 静态定位(static) 
```markdown
静态定位是元素的默认定位方式，无定位的意思。它
相当于 border 里面的none， 不要定位的时候用。
静态定位 按照标准流特性摆放位置，它没有边偏移。
静态定位在布局时我们几乎不用的 
```
#### 4.2.1.6 相对定位(relative)
```markdown
相对定位是元素相对于它原来在标准流中的位置来说的。（自恋型）
原来在标准流的区域继续占有，后面的盒子仍然以标准流
的方式对待它。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200630110801285.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```html
相对定位初识
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			width: 200px;
			height: 200px;
			background-color: pink;
			margin: 100px;
			/*定位 = 定位模式 +  边偏移*/
			position: relative;
			top: 100px;
			left: 100px;
		}
	</style>
</head>
<body>
	<div></div>
</body>
</html>

相对定位2个特性
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			width: 200px;
			height: 200px;
			background-color: pink;
		}
		.two {
			background-color: purple;
			position: relative;
			top: 100px;
			left: 100px;
		}
	</style>
</head>
<body>
	<div>1</div>
	<div class="two">2</div>
	<div>3</div>
	
</body>
</html>
```
#### 4.2.1.7 绝对定位(absolute)
```markdown

绝对定位是元素以带有定位的父级元素来移动位置 （拼爹型）
将元素依据最近的已经定位（绝对、固定或相对定位）的父元
素（祖先）进行定位.
定位口诀 —— 子绝父相

1. 完全脱标 —— 完全不占位置；  

2. 父元素没有定位，则以浏览器为准定位（Document 文档）。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200630110902897.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200630110944289.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```html
绝对定位以带有定位的父级来移动位置
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>绝对定位以带有定位的父级元素来移动位置</title>
	<style>
		.yeye {
			width: 500px;
			height: 500px;
			background-color: skyblue;
			position: absolute;
		}
		.father {
			width: 350px;
			height: 350px;
			background-color: pink;
			margin: 100px;
			/*标准流的子盒子总是以父级为准移动位置*/
			  /*如果 父级 没有定位 绝对定位子盒子 以我们文档为准浏览器移动位置*/
			  /*如果 父级 有定位 绝对定位子盒子 以父级为准移动位置*/   
			/*position: relative;*/
		}
		.son {
			width: 200px;
			height: 200px;
			background-color: purple;
			/*margin-left: 100px;*/
			position: absolute;
			top: 50px;
			left: 50px;
		}
	</style>
</head>
<body>
	<div class="yeye">
		<div class="father">
				<div class="son"></div>
		</div>
	</div>
</body>
</html>


绝对定位的盒子不占有位置
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>绝对定位的盒子不占有位置</title>
	<style>
		.father {
			width: 500px;
			height: 500px;
			background-color: pink;
			position: relative;
		}
		.damao {
			position: absolute;
			top: 0;
			left: 0;
			width: 200px;
			height: 200px;
			background-color: purple;
		}
		.ermao {
			width: 250px;
			height: 250px;
			background-color: skyblue;
		}
	</style>
</head>
<body>
	<div class="father">
		<div class="damao"></div>
		<div class="ermao"></div>
	</div>
</body>
</html>

子绝父相代码验证
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		.up {
			position: relative;
			width: 1000px;
			height: 90px;
			background-color: pink;
		}
		.down {
			width: 1000px;
			height: 150px;
			background-color: #000;
		}
		.arr-l {
			position: absolute;
			top: 25px;
			left: 0;
			width: 40px;
			height: 40px;
			background-color: purple;
		}
		.arr-r {
			position: absolute;
			top: 25px;
			right: 0;
			width: 40px;
			height: 40px;
			background-color: purple;
		}
	</style>
</head>
<body>
	<div class="up">
		<img src="images/img.jpg" alt="">
		<div class="arr-l"></div>
		<div class="arr-r"></div>
	</div>
	<div class="down"></div>
</body>
</html>
```
#### 4.2.1.8 固定定位(fixed)
```markdown
1. 完全脱标 —— 完全不占位置；
2. 只认浏览器的可视窗口 —— 浏览器可视窗口 + 边偏移属性 来设置元素的位置；
   * 跟父元素没有任何关系；单独使用的
   * 不随滚动条滚动。
 3.在使用固定定位时，如果盒子中没有内容，需要指定宽度
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200630111315141.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>固定定位</title>
	<style>
		body {
			height: 1500px;
		}
		.box {
			position: relative;
			width: 300px;
			height: 300px;
			background-color: pink;
			margin: 100px;
		}
		.box img {
			/* 跟父级没有任何关系 */
			position: fixed;
			right: 0;
			bottom: 0;
		}
	</style>
</head>
<body>
	<div class="box">
		<img src="images/sun.jpg" alt="" width="150">
	</div>
</body>
</html>
```
#### 4.2.1.9 定位(position)的扩展
```markdown
绝对定位/固定定位的盒子不能通过设置 margin: auto设置
水平居中。相对定位可以

1. left: 50%;：让盒子的左侧移动到父级元素的水平中心位置；
2. margin-left: -100px;：让盒子向左移动自身宽度的一半。
```
```markdown
解决方案:
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200504135832139.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200504140345980.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)


```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>绝对定位的盒子水平居中对齐</title>
	<style>
		div {
			/*绝对定位 margin 左右auto 不能让盒子水平居中*/
			position: absolute;
			/*1.left 50% 走父亲宽度的一半*/	
			left: 50%;
			/*2.margin-left 左走自己宽度的一半  一定注意是 负值*/
			margin-left: -100px;
			width: 200px;
			height: 200px;
			background-color: pink;
			/*标准流 margin 左右auto 就可以让盒子水平居中*/
			/*margin: auto;*/
		}
	</style>
</head>
<body>
	<div></div>
</body>
</html>
```
#### 4.2.1.10 堆叠顺序
```markdown
在使用定位布局时，可能会出现盒子重叠的情况。
加了定位的盒子，默认后来者居上， 后面的盒子会压住前面的盒子。
应用 `z-index` 层叠等级属性可以调整盒子的堆叠顺序
z-index的特性如下：
属性值：正整数、负整数或 、0，默认值是 0，数值越大，盒子
越靠上；
如果属性值相同，则按照书写顺序，后来居上；
数字后面不能加单位。
注意：`z-index` 只能应用于相对定位、绝对定位和固定定位
的元素，其他标准流、浮动和静态定位无效。
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		.damao,
		.ermao,
		.sanmao {
			/*绝对定位*/
			position: absolute;
			width: 200px;
			height: 200px;
			background-color: red;
		}
		.damao {
			z-index: 2;
			/*数值越大越靠上
			后面不跟单位
			只能是整数*/
		}
		.ermao {
			top: 50px;
			left: 50px;
			z-index: 1;
			background-color: green;
		}
		.sanmao {
			top: 100px;
			left: 100px;
			background-color: blue;
		}
	</style>
</head>
<body>
	<div class="damao"></div>
	<div class="ermao"></div>
	<div class="sanmao"></div>
</body>
</html>
```
#### 4.2.1.11 定位改变display属性
```markdown
前面我们讲过， display 是 显示模式， 可以改变显示模式有
以下方式:

可以用inline-block  转换为行内块
可以用浮动 float 默认转换为行内块（类似，并不完全一样，因为
浮动是脱标的）
绝对定位和固定定位也和浮动类似， 默认转换的特性 转换为行内块。
所以说， 一个行内的盒子，如果加了浮动、固定定位和绝对定
位，不用转换，就可以给这个盒子直接设置宽度和高度等。
同时注意：
浮动元素、绝对定位(固定定位）元素的都不会触发外边距合并的问题。 （我们以前是用padding border overflow解决的）
也就是说，我们给盒子改为了浮动或者定位，就不会有垂直外边
距合并的问题了。
```

| 定位模式         | 是否脱标占有位置     | 移动位置基准           | 模式转换（行内块） | 使用情况                 |
| ---------------- | -------------------- | :--------------------- | ------------------ | ------------------------ |
| 静态static       | 不脱标，正常模式     | 正常模式               | 不能               | 几乎不用                 |
| 相对定位relative | 不脱标，占有位置     | 相对自身位置移动       | 不能               | 基本单独使用             |
| 绝对定位absolute | 完全脱标，不占有位置 | 相对于定位父级移动位置 | 能                 | 要和定位父级元素搭配使用 |
| 固定定位fixed    | 完全脱标，不占有位置 | 相对于浏览器移动位置   | 能                 | 单独使用，不需要父级     |


```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			/*块级元素 不给width 默认通栏显示*/
			/*1. 利用 display inline-block*/
			/*display: inline-block;*/
			/*行内块不给width 默认的宽度就是内容的宽度*/
			/*2. 浮动 也能转换*/
			/*float: left;*/
			/*3. 绝对定位（固定定位） 也能转换*/
			position: absolute;
			height: 100px;
			background-color: pink;
		}
		span {
			position: absolute;
			top: 200px;
			left: 200px;
			width: 300px;
			height: 300px;
			background-color: purple;
		}
	</style>
</head>
<body>
	<div>天王盖地虎， 导师一米五</div>
	<span></span>
</body>
</html>

绝对定位和浮动不会触发外边距合并
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		.father {
			/*float: left;*/
			position: absolute;
			width: 500px;
			height: 500px;
			background-color: pink;
		}
		.son {
			/*float: left;*/
			/*position: absolute;*/
			width: 200px;
			height: 200px;
			background-color: purple;
			/*外边距合并*/
			margin-top: 100px;
		}
	</style>
</head>
<body>
	<div class="father">
		<div class="son"></div>
	</div>
</body>
</html>
```
#### 4.2.1.12 固定定位的实际应用技巧
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020050415580295.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 4.2.1.13 圆角矩形设置4个角
```markdown
圆角矩形可以为4个角分别设置圆度， 但是是有顺序的
border-top-left-radius:20px;
border-top-right-radius:20px;
border-bottom-right-radius:20px;
border-bottom-left-radius:20px;

如果4个角，数值相同
border-radius: 15px;

里面数值不同，我们也可以按照简写的形式，具体格式如下:
border-radius: 左上角 右上角  右下角  左下角;
```

#### 4.2.1.14  网页布局总结
```markdown
一个完整的网页，有标准流 、 浮动 、 定位 一起完成
布局的。每个都有自己的专门用法。

1). 标准流

可以让盒子上下排列 或者 左右排列的

2). 浮动

可以让多个块级元素一行显示  或者 左右对齐盒子   浮动的盒子就是按照顺序左右排列 

3). 定位

定位最大的特点是有层叠的概念，就是可以让多个盒子 前后 叠压来显示。 但是每个盒子需要测量数值。
```
## 5.1 高级技巧
###  5.1.1 元素的显示与隐藏
```markdown
display: none 隐藏对象

display：block 除了转换为块级元素之外，同
时还有显示元素的意思。

特点： 隐藏之后，不再保留位置
配合后面js做特效，比如下拉菜单，原先没有，鼠标经过，
显示下拉菜单， 应用极为广泛
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020063016103233.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		.damao {
			/*隐藏元素 damao*/
			/*display: none;*/
			/*1. 先隐藏元素 
			  2. 不保留位置 */
			display: block;
			/*这里除了转换为块级元素以外，还可以 显示元素*/
			width: 200px;
			height: 200px;
			background-color: pink;
		}
		.ermao {
			width: 250px;
			height: 250px;
			background-color: purple;
		}
	</style>
</head>
<body>
	<div class="damao"></div>
	<div class="ermao"></div>
</body>
</html>
```

###  5.1.2 visibility 可见性
```markdown
visibility：visible ; 　对象可视

visibility：hidden; 　  对象隐藏

特点： 隐藏之后，继续保留原有位置。（停职留薪）
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020063016112235.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		.damao {
			/*1. hidden 隐藏元素
			2. 保留位置*/
			/*visibility: hidden;*/
			visibility: visible;
			width: 200px;
			height: 200px;
			background-color: pink;
		}
		.ermao {
			width: 250px;
			height: 250px;
			background-color: purple;
		}
	</style>
</head>
<body>
	<div class="damao"></div>
	<div class="ermao"></div>
</body>
</html>
```
###  5.1.3 overflow 溢出
```markdown
实际开发场景：
1. 清除浮动
2. 隐藏超出内容，隐藏掉,  不允许内容超过父盒子。
```

| 属性值      | 描述                                       |
| ----------- | ------------------------------------------ |
| **visible** | 不剪切内容也不添加滚动条                   |
| **hidden**  | 不显示超过对象尺寸的内容，超出的部分隐藏掉 |
| **scroll**  | 不管超出内容否，总是显示滚动条             |
| **auto**    | 超出自动显示滚动条，不超出不显示滚动条     |

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			/*overflow: hidden;*/
			/*溢出隐藏 -- 超出盒子大小的部分 隐藏起来*/
			/*overflow: scroll;*/
			/*显示滚动条总是会显示 - 不会超出盒子大小  内容能显示全但是  太丑了我们不常用*/
			overflow: auto;
			/*1.如果超出，就显示滚动条
			2.如果不超出，不显示滚动条 
			3.然并卵，我们还是不用*/
			width: 150px;
			height: 150px;
			border: 1px solid red;
		}
	</style>
</head>
<body>
	<div>
		我要带你去浪漫的土耳其。
		我要带你去网吧偷耳机。
		我要带你去浪漫的土耳其。
		我要带你去网吧偷耳机。
		我要带你去浪漫的土耳其。
		我要带你去网吧偷耳机。
		我要带你去网吧偷耳机。
		我要带你去网吧偷耳机。
	</div>
</body>
</html>
```

###  5.1.4 显示与隐藏总结

| 属性           | 区别                   | 用途                                                         |
| -------------- | ---------------------- | ------------------------------------------------------------ |
| **display**    | 隐藏对象，不保留位置   | 配合后面js做特效，比如下拉菜单，原先没有，鼠标经过，显示下拉菜单， 应用极为广泛 |
| **visibility** | 隐藏对象，保留位置     | 使用较少                                                     |
| **overflow**   | 只是隐藏超出大小的部分 | 1. 可以清除浮动 2. 保证盒子里面的内容不会超出该盒子范围      |

###  5.1.5  CSS用户界面样式
####  5.1.5.1 鼠标样式cursor

| 属性值          | 描述       |
| --------------- | ---------- |
| **default**     | 小白  默认 |
| **pointer**     | 小手       |
| **move**        | 移动       |
| **text**        | 文本       |
| **not-allowed** | 禁止       |

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<ul>
		<li style="cursor: default;">默认的</li>
		<li style="cursor: pointer;">小手</li>
		<li style="cursor: move;">移动</li>
		<li style="cursor: text;">文本</li>
		<li style="cursor: not-allowed;">禁止</li>
	</ul>
</body>
</html>
```

####  5.1.5.2 轮廓线 outline
```markdown
是绘制于元素周围的一条线，位于边框边缘的外围，可
起到突出元素的作用。 
```
```css
outline : outline-color ||outline-style || outline-width 
```
```markdown
但是我们都不关心可以设置多少，我们平时都是去掉的。
最直接的写法是 ： outline: 0;   或者  outline: none;
```
```html
 <input  type="text"  style="outline: 0;"/>
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		input {
			/*取消轮廓线*/
			outline: none;
		}
	</style>
</head>
<body>
	<input type="text">
</body>
</html>
```

####  5.1.5.3  防止拖拽文本域resize
```markdown
实际开发中，我们文本域右下角是不可以拖拽： 
```
```html
<textarea  style="resize: none;"></textarea>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>防止拖拽文本域</title>
	<style>
		textarea {
			width: 500px;
			height: 249px;
			/*取消轮廓线*/
			outline: none;
			/*边框改变颜色*/
			border: 1px solid #036;
			/*防止用户拖拽文本域*/
			resize: none;
		}
	</style>
</head>
<body>
	<textarea>请留了言</textarea>
	1231231
</body>
</html>
```
####  5.1.5.4 用户界面样式总结

| 属性         | 用途                 | 用途                                                         |
| ------------ | -------------------- | ------------------------------------------------------------ |
| **鼠标样式** | 更改鼠标样式cursor   | 样式很多，重点记住 pointer                                   |
| **轮廓线**   | 表单默认outline      | outline 轮廓线，我们一般直接去掉，border是边框，我们会经常用 |
| 防止拖拽     | 主要针对文本域resize | 防止用户随意拖拽文本域，造成页面布局混乱，我们resize:none    |

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			border: 1px solid red;
		}
		div img {
			/*因为默认的是基线对齐，所有底册有空白缝隙*/
			/*1. 只要不是 基线对齐就好了*/
			/*vertical-align: baseline;*/
			/*vertical-align: bottom;*/
			/*vertical-align: middle;*/
			/*2. 块级元素来说 vertical-align:  是无效的  就不会有基线对齐的问题了*/
			display: block;
		}
	</style>
</head>
<body>
	<div>
		<img src="images/3.jpg" alt="">
	</div>
</body>
</html> 
```
```markdown
vertical-align 垂直对齐
vertical-align 垂直对齐，它只针对于行内元
素或者行内块元素
一般用来处理图片跟文字的对齐效果
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200504163617232.png)
```markdown
vertical-align : baseline |top |middle |bottom 

原因：

图片或者表单等行内块元素，他的底线会和父级盒子的基线对齐。就是图片底侧会有一个空白缝隙。
解决的方法就是：  
给img vertical-align:middle | top| bottom等等。  让图片不要和基线对齐。
给img 添加 display：block; 转换为块级元素就不会存在问题了。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200504163915809.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```html
vertical-align对于块级无效
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			width: 100px;
			height: 100px;
			background-color: pink;
			margin: auto;
			vertical-align: middle;
		}
	</style>
</head>
<body>
	<div>
		你会失望的
	</div>
</body>
</html>

vertical-align图片对齐文字
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		.one {
			/*默认的是基线对齐*/
			vertical-align: baseline;
		}
		.two {
			/*让图片的中线 对齐 文字的中线  垂直居中*/
			vertical-align: middle;
		}
		.three {
			/*让图片的顶线 对齐 文字的顶线 顶部对齐*/
			vertical-align: top;
		}
	</style>
</head>
<body>
	<div>
		<img src="images/2.jpg" alt="" class="one"> 你瞅啥 
	</div>
	<div>
		<img src="images/2.jpg" alt="" class="two"> 你瞅啥 
	</div>
	<div>
		<img src="images/2.jpg" alt="" class="three"> 你瞅啥 
	</div>
</body>
</html>

去除图片底侧空白缝隙
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			border: 1px solid red;
		}
		div img {
			/*因为默认的是基线对齐，所有底册有空白缝隙*/
			/*1. 只要不是 基线对齐就好了*/
			/*vertical-align: baseline;*/
			/*vertical-align: bottom;*/
			/*vertical-align: middle;*/
			/*2. 块级元素来说 vertical-align:  是无效的  就不会有基线对齐的问题了*/
			display: block;
		}
	</style>
</head>
<body>
	<div>
		<img src="images/3.jpg" alt="">
	</div>
</body>
</html>
```

###  5.1.6 溢出的文字省略号显示
```markdown
white-space:normal ；默认处理方式

white-space:nowrap ；　强制在同一行内显示所有文本，
直到文本结束或者遭遇br标签对象才换行。

text-overflow : clip ；不显示省略标记（...），
而是简单的裁切 

text-overflow：ellipsis ； 当对象内文本溢出时
显示省略标记（...）
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>溢出的文字省略号显示</title>
	<style>
		div {
			width: 150px;
			height: 25px;
			border: 1px solid pink;
			/*当文字显示不开的时候，自动换行*/
			/*white-space: normal;*/
			/*1. 要文字强制一行内显示 除非 遇到 br   */
			white-space: nowrap;
			/*2. 溢出隐藏*/
			overflow: hidden;
			/*3. 文字溢出 用省略号替代  ellipsis 省略号*/
			text-overflow: ellipsis;

		}
	</style>
</head>
<body>
	<div>hi~ 来自猩猩的你-欢迎你</div>
</body>
</html>
```

###  5.1.7 总结三步曲
```markdown
/*1. 先强制一行内显示文本*/
    white-space: nowrap;
/*2. 超出的部分隐藏*/
    overflow: hidden;
/*3. 文字用省略号替代超出的部分*/
    text-overflow: ellipsis;
```

###  5.1.8 CSS精灵技术（sprite)
```markdown
首先我们知道，css精灵技术主要针对于背景图片，插
入的图片img 是不需要这个技术的。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>体会css精灵技术</title>
	<style>
		.icon1 {
			width: 23px;
			height: 23px;
			background: url(images/index.png) no-repeat  0 -107px;
			/*background-position: 0 -107px;*/
		}
		.icon2 {
			width: 23px;
			height: 23px;
			background: url(images/index.png) no-repeat -157px -107px;
			margin: 100px;
		}
	</style>
</head>
<body>
	<div class="icon1"></div>
	<div class="icon2"></div>
</body>
</html>
```
###  5.1.9 滑动门
```markdown
核心技术就是利用CSS精灵（主要是背景位置）和 盒
子padding撑开宽度, 以便能适应不同字数的导航栏。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		/*1. a 是 设置 左侧 背景 （左门）*/
		a {
			 /*因为我们是滑动门，左右推拉 跟文字内容多少有关系，此时需要用文字撑开盒子， 就要用到行内块*/
			display: inline-block;
			height: 33px;
			background: url(images/to.png) no-repeat;
			margin: 100px;
			padding-left: 15px;
			color: #fff;
		}
		/*2. span 是设置 右侧 背景 （右门）*/
		a span {
			display: inline-block;
			height: 33px;
			/*一定注意 span 需要背景图片 右对齐*/
			background: url(images/to.png) no-repeat right top;
			padding-right: 15px;
		}
		/*3 因为整个导航栏都是 链接 所以 a 要包含 span */
		
	</style>
</head>
<body>
	<a href="#">
		<span>首页</span>
	</a>
	<a href="#">
		<span>公司新闻</span>
	</a>
</body>
</html>
```

```markdown
总结： 

1. a 设置 背景左侧，padding撑开合适宽度。    
2. span 设置背景右侧， padding撑开合适宽度 剩下由文字继续撑开宽度。
3. 之所以a包含span就是因为 整个导航都是可以点击的。
```
###  5.1.10 margin负值之美
```markdown
负边距+定位：水平垂直居中
咱们前面讲过， 一个绝对定位的盒子， 利用  父级盒子的 50%，  然后 往左(上) 走 自己宽度的一半 ，可以实现盒子水平垂直居中。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>margin负值之美-突出显示某个盒子</title>
	<style>
		div {
			/*浮动的盒子是紧贴在一起的*/
			float: left;
			width: 200px;
			height: 300px;
			border: 1px solid #ccc;
			margin-left: -1px;
			margin-top: -1px;
		}
		/*鼠标经过div 的意思  p:hover */
		div:hover {
			/*我要让当前鼠标经过的这个div 升到最高处来就好了*/
			/*定位的盒子是最高层的  */
			border: 1px solid #f40;
			/*我们只要保证当前的这个盒子 是定位 就会压住 标准流和浮动盒子*/
			position: relative;
			/*我们只能用相对定位  它是占位置的*/
		}
	</style>
</head>
<body>
	<div></div>
	<div></div>
	<div></div>
	<div></div>
	<div></div>
	<div></div>
	<div></div>
	<div></div>
	<div></div>
</body>
</html>
```
```markdown
或者
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>margin负值之美-突出显示某个盒子</title>
	<style>
		div {
			position: relative;
			/*浮动的盒子是紧贴在一起的*/
			float: left;
			width: 200px;
			height: 300px;
			border: 1px solid #ccc;
			margin-left: -1px;
			margin-top: -1px;
		}
		/*鼠标经过div 的意思  p:hover */
		div:hover {
			/*我要让当前鼠标经过的这个div 升到最高处来就好了*/
			/*定位的盒子是最高层的  */
			border: 1px solid #f40;
			/*都是定位的盒子，我们通过z-index 来实现层级关系*/
			z-index: 1;

		}
	</style>
</head>
<body>
	<div></div>
	<div></div>
	<div></div>
	<div></div>
	<div></div>
	<div></div>
	<div></div>
	<div></div>
	<div></div>
</body>
</html>
```
```markdown
float跟position可以连着一起使用。
```

### 5.1.11 三角形之美
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		div {
			/*我们用css 边框可以模拟三角效果*/
			width: 0;
			height: 0;
			border-top: 10px solid red;
			border-right: 10px solid green;
			border-bottom: 10px solid blue;
			border-left: 10px solid pink;
		}
		p {
			width: 0;
			height: 0;
			border-style: solid;
			border-width: 10px;
			border-color:  transparent transparent transparent red;
			font-size: 0;
			line-height: 0;
		}
	</style>
</head>
<body>
	<div></div>
	<p></p>
</body>
</html>
```
```markdown
1. 我们用css 边框可以模拟三角效果
2. 宽度高度为0
3. 我们4个边框都要写， 只保留需要的边框颜色，其余的不能省略，都改为 transparent 透明就好了
4. 为了照顾兼容性 低版本的浏览器，加上 font-size: 0;  line-height: 0;
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复
css即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
