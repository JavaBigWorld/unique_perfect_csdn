# 移动Web开发之响应式布局
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714021122428.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

##  1.1 响应式布局
```markdown
就是使用媒体查询针对不同宽度的设备进行布局和样式的设置，
从而适配不同设备的目的。
```
###  1.1.1 设备的划分情况：
```markdown
小于768的为超小屏幕（手机）
768~992之间的为小屏设备（平板）
992~1200的中等屏幕（桌面显示器）
大于1200的宽屏设备（大桌面显示器）
```
###  1.1.2 响应式开发
```markdown
响应式布局容器
响应式需要一个父级做为布局容器，来配合子级元素来实现变化效果。
原理就是在不同屏幕下，通过媒体查询来改变这个布局容器的大小，
再改变里面子元素的排列方式和大小，从而实现不同屏幕下，看到
不同的页面布局和样式变化。

超小屏幕（手机，小于 768px）：设置宽度为 100%
小屏幕（平板，大于等于 768px）：设置宽度为 750px
中等屏幕（桌面显示器，大于等于 992px）：宽度设置为 970px
大屏幕（大桌面显示器，大于等于 1200px）：宽度设置为 1170px 
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .container {
            height: 150px;
            background-color: pink;
            margin: 0 auto;
        }
        /* 1. 超小屏幕下  小于 768  布局容器的宽度为 100% */
        
        @media screen and (max-width: 767px) {
            .container {
                width: 100%;
            }
        }
        /* 2. 小屏幕下  大于等于768  布局容器改为 750px */
        
        @media screen and (min-width: 768px) {
            .container {
                width: 750px;
            }
        }
        /* 3. 中等屏幕下 大于等于 992px   布局容器修改为 970px */
        
        @media screen and (min-width: 992px) {
            .container {
                width: 970px;
            }
        }
        /* 4. 大屏幕下 大于等于1200 布局容器修改为 1170 */
        
        @media screen and (min-width: 1200px) {
            .container {
                width: 1170px;
            }
        }
    </style>
</head>

<body>
    <!-- 响应式开发里面，首先需要一个布局容器 -->
    <div class="container"></div>
</body>

</html>
```
###  1.1.3 响应式导航
```markdown
1 当我们屏幕大于等于800像素，我们给nav宽度为800px，因
为里面子盒子需要浮动，所以nav需要清除浮动。
2 nav里面包含8个小li 盒子，每个盒子的宽度定为 100px， 高
度为 30px，浮动一行显示。
3 当我们屏幕缩放，宽度小于800像素的时候， nav盒子宽度修
改为 100% 宽度。
nav里面的8个小li，宽度修改为 33.33%，这样一行就只能显示
3个小li ，剩余下行显示。
```
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        
        ul {
            list-style: none;
        }
        
        .container {
            width: 750px;
            margin: 0 auto;
        }
        
        .container ul li {
            float: left;
            width: 93.75px;
            height: 30px;
            background-color: green;
        }
        
        @media screen and (max-width: 767px) {
            .container {
                width: 100%;
            }
            .container ul li {
                width: 33.33%;
            }
        }
    </style>
</head>

<body>
    <div class="container">
        <ul>
            <li>导航栏</li>
            <li>导航栏</li>
            <li>导航栏</li>
            <li>导航栏</li>
            <li>导航栏</li>
            <li>导航栏</li>
            <li>导航栏</li>
            <li>导航栏</li>
        </ul>
    </div>
</body>

</html>
```
##  1.2 bootstrap的介绍
###  1.2.1 创建html骨架结构
```html
<!--要求当前网页使用IE浏览器最高版本的内核来渲染-->
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<!--视口的设置：视口的宽度和设备一致，默认的缩放比例和PC端一致，用户不能自行缩放-->
<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=0">
<!--[if lt IE 9]>
<!--解决ie9以下浏览器对html5新增标签的不识别，并导致CSS不起作用的问题-->
<script src="https://oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js"></script>
<!--解决ie9以下浏览器对 css3 Media Query 的不识别 -->
<script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
<![endif]-->
```
###  1.2.2  引入相关样式文件
```css
<!-- Bootstrap 核心样式-->
<link rel="stylesheet" href="bootstrap/css/bootstrap.min.css">
```
###  1.2.3 栅格系统使用
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200714085343230.png)
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <!--[if lt IE 9]>
      <script src="https://oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js"></script>
      <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
    <![endif]-->
    <!-- 一定不要忘记引入bootstrap 的样式文件 -->
    <link rel="stylesheet" href="bootstrap/css/bootstrap.min.css">
    <title>Document</title>
    <style>
        [class^="col"] {
            border: 1px solid #ccc;
        }
        
        .row:nth-child(1) {
            background-color: pink;
        }
    </style>
</head>

<body>
    <div class="container">
        <div class="row">
            <div class="col-lg-3 col-md-4 col-sm-6 col-xs-12">1</div>
            <div class="col-lg-3 col-md-4 col-sm-6 col-xs-12">2</div>
            <div class="col-lg-3 col-md-4 col-sm-6 col-xs-12">3</div>
            <div class="col-lg-3 col-md-4 col-sm-6 col-xs-12">4</div>
        </div>
        <!-- 如果孩子的份数相加等于12 则孩子能占满整个 的container 的宽度 -->
        <div class="row">
            <div class="col-lg-6">1</div>
            <div class="col-lg-2">2</div>
            <div class="col-lg-2">3</div>
            <div class="col-lg-2">4</div>
        </div>
        <!-- 如果孩子的份数相加 小于 12 则会？ 则占不满整个container 的宽度 会有空白 -->
        <div class="row">
            <div class="col-lg-6">1</div>
            <div class="col-lg-2">2</div>
            <div class="col-lg-2">3</div>
            <div class="col-lg-1">4</div>
        </div>
        <!-- 如果孩子的份数相加 大于 12 则会？多于的那一列会 另起一行显示  -->

        <div class="row">
            <div class="col-lg-6">1</div>
            <div class="col-lg-2">2</div>
            <div class="col-lg-2">3</div>
            <div class="col-lg-3">4</div>
        </div>


    </div>
</body>

</html>
```
###  1.2.4  布局容器
```markdown
Bootstrap 需要为页面内容和栅格系统包裹一个 .container 容
器，它提供了两个作此用处的类
```
```markdown
1. container 类
响应式布局的容器 固定宽度
大屏 ( >=1200px) 宽度定为 1170px
中屏 ( >=992px) 宽度定为 970px
小屏 ( >=768px) 宽度定为 750px
超小屏 (100%)
2. container-fluid 类
流式布局容器 百分百宽度
占据全部视口（viewport）的容器。
```
###  1.2.5  bootstrap栅格系统
```markdown
Bootstrap提供了一套响应式、移动设备优先的流式栅格系统，随着
屏幕或视口（viewport）尺寸的增加，系统会自动分为最多12列。

栅格系统用于通过一系列的行（row）与列（column）的组合来创
建页面布局，你的内容就可以放入这些创建好的布局中。

按照不同屏幕划分为1~12 等份
行（row） 可以去除父容器作用15px的边距
xs-extra small：超小； sm-small：小；  md-medium：中等； lg-large：大；
列（column）大于 12，多余的“列（column）”所在的元素将被作为一个整体另起一行排列
每一列默认有左右15像素的 padding
可以同时为一列指定多个设备的类名，以便划分不同份数  例如 class="col-md-4 col-sm-6"

栅格嵌套
栅格系统内置的栅格系统将内容再次嵌套。简单理解就是一个列内再分成若干份小列。我们可以通过添加一个新的 .row 元素和一系列 .col-sm-* 元素到已经存在的 .col-sm-*
元素内。

列嵌套 

<div class="col-sm-4">
    <div class="row">
         <div class="col-sm-6">小列</div>
         <div class="col-sm-6">小列</div>
    </div>
</div>

列偏移

使用 .col-md-offset-* 类可以将列向右侧偏移。这些类实际是通过使用 *选择器为当前元素增加了左侧的边距（margin）。<!-- 列偏移 -->
<div class="row">
    <div class="col-lg-4">1</div>
    <div class="col-lg-4 col-lg-offset-4">2</div>
</div>


列排序
通过使用 .col-md-push-* 和 .col-md-pull-* 类就可以很容易的改变列（column）的顺序。

<!-- 列排序 -->
<div class="row">
   <div class="col-lg-4 col-lg-push-8">左侧</div>
   <div class="col-lg-8 col-lg-pull-4">右侧</div>
</div>

```
###  1.2.6 栅格系统列嵌套
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <!--[if lt IE 9]>
      <script src="https://oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js"></script>
      <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
    <![endif]-->
    <!-- 一定不要忘记引入bootstrap 的样式文件 -->
    <link rel="stylesheet" href="bootstrap/css/bootstrap.min.css">
    <title>Document</title>
    <style>
        .row>div {
            height: 50px;
            background-color: pink;
            /* margin: 0 10px; */
        }
    </style>
</head>

<body>
    <div class="container">
        <div class="row">
            <div class="col-md-4">
                <!-- 我们列嵌套最好加1个行 row 这样可以取消父元素的padding值 而且高度自动和父级一样高 -->
                <div class="row">
                    <div class="col-md-4">a</div>
                    <div class="col-md-8">b</div>
                </div>
            </div>
            <div class="col-md-4">2</div>
            <div class="col-md-4">3</div>
        </div>
    </div>
</body>

</html>
```
###  1.2.7 栅格系统列偏移
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <!--[if lt IE 9]>
      <script src="https://oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js"></script>
      <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
    <![endif]-->
    <!-- 一定不要忘记引入bootstrap 的样式文件 -->
    <link rel="stylesheet" href="bootstrap/css/bootstrap.min.css">
    <title>Document</title>
    <style>
        .row div {
            height: 50px;
            background-color: pink;
        }
    </style>
</head>

<body>
    <div class="container">
        <div class="row">
            <div class="col-md-3">左侧</div>
            <!-- 偏移的份数 就是 12 - 两个盒子的份数 = 6 -->
            <div class="col-md-3 col-md-offset-6">右侧</div>
        </div>
        <div class="row">
            <!-- 如果只有一个盒子 那么就偏移 = (12 - 8) /2 -->
            <div class="col-md-8 col-md-offset-2">中间盒子</div>
        </div>

    </div>
</body>

</html>
```
###  1.2.8 栅格系统列排序
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <!--[if lt IE 9]>
      <script src="https://oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js"></script>
      <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
    <![endif]-->
    <!-- 一定不要忘记引入bootstrap 的样式文件 -->
    <link rel="stylesheet" href="bootstrap/css/bootstrap.min.css">
    <title>Document</title>
    <style>
        .row div {
            height: 50px;
            background-color: pink;
        }
    </style>
</head>

<body>
    <div class="container">
        <div class="row">
            <div class="col-md-4 col-md-push-8">左侧</div>
            <div class="col-md-8 col-md-pull-4">右侧</div>
        </div>
    </div>
</body>

</html>
```
###  1.2.9 响应式工具
```markdown
为了加快对移动设备友好的页面开发工作，利用媒体查询功
能，并使用这些工具类可以方便的针对不同设备展示或隐藏页面内容。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509220358653.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <!--[if lt IE 9]>
      <script src="https://oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js"></script>
      <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
    <![endif]-->
    <!-- 一定不要忘记引入bootstrap 的样式文件 -->
    <link rel="stylesheet" href="bootstrap/css/bootstrap.min.css">
    <title>Document</title>
    <style>
        .row div {
            height: 300px;
            background-color: purple;
        }
        
        .row div:nth-child(3) {
            background-color: pink;
        }
        
        span {
            font-size: 50px;
            color: #fff;
        }
    </style>
</head>

<body>
    <div class="container">
        <div class="row">
            <div class="col-xs-3">
                <span class="visible-lg">我会显示哦</span>
            </div>
            <div class="col-xs-3">2</div>
            <div class="col-xs-3 hidden-md hidden-xs">我会变魔术哦</div>
            <div class="col-xs-3">4</div>
        </div>

    </div>
</body>

</html>
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复
响应式即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
