#  HTML5+CSS3
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714013829146.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1.1 新增了那些语义化标签
```markdown
header   ---  头部标签
nav        ---  导航标签
article---   内容标签
section ---   块级标签
aside     ---   侧边栏标签
footer   ---   尾部标签
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200506213352736.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        header,
        nav,
        section {
            display: block;
            height: 120px;
            background-color: pink;
            margin: 10px;
        }
    </style>

</head>

<body>
    <header> header</header>
    <header> header</header>
    <nav> nav </nav>
    <section></section>
</body>

</html>
```
## 1.2 多媒体音频标签
```markdown
1. 多媒体标签有两个，分别是

   音频  -- audio
   视频  -- video

2. audio标签说明

   - 可以在不使用标签的情况下，也能够原生的支持音频
格式文件的播放，
   - 但是：播放格式是有限的

3. audio 支持的音频格式

   - audio 目前支持三种格式
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200506214334444.png)
```markdown
audio 的参数
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200506214500720.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
audio 代码演示
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <!-- <audio src="media/snow.mp3" controls="controls"></audio> -->
    <audio controls="controls">
        <source src="media/snow.mp3" type="audio/mpeg" />
        <source src="media/snow.ogg" type="audio/ogg" />
            您的浏览器不支持播放声音
    </audio>
</body>

</html>
```
## 1.3 多媒体视频标签
```markdown
video 视频标签
目前支持三种格式
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200506215211206.png)
```markdown
语法格式
```
```html
<video src="./media/video.mp4" controls="controls"></video>
```

```markdown
video 参数
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200506215351934.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
video 代码演示   
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
        video {
            width: 300px;
        }
    </style>
</head>

<body>
    <!-- <video src="media/video.mp4" controls="controls"></video> -->
    <!-- 谷歌浏览器把自动播放功能给禁用了 有解决方案： 给视频添加静音属性 -->
    <video muted="muted" loop="loop" poster="media/pig.jpg" controls>
        <source src="media/video.mp4" type="video/mp4" />
        <source src="media/video.ogg" type="video/ogg" />
        您的浏览器太low了，不支持播放此视频
    </video>
</body>

</html>
```
   

## 1.4 多媒体标签总结
```markdown
音频标签与视频标签使用基本一致
多媒体标签在不同浏览器下情况不同，存在兼容性问题
谷歌浏览器把音频和视频标签的自动播放都禁止了
谷歌浏览器中视频添加 muted 标签可以自己播放
注意：重点记住使用方法以及自动播放即可，其他属性
可以在使用时查找对应的手册
```

## 1.5 新增 input 标签
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200506215907706.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <form action="">
        <ul>
            <li>邮箱: <input type="email" /></li>
            <li>网址: <input type="url" /></li>
            <li>日期: <input type="date" /></li>
            <li>日期: <input type="time" /></li>
            <li>数量: <input type="number" /></li>
            <li>手机号码: <input type="tel" /></li>
            <li>搜索: <input type="search" /></li>
            <li>颜色: <input type="color" /></li>

            <li> <input type="submit" value="提交"></li>
        </ul>
    </form>
</body>

</html>
```

## 1.6 新增表单属性
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200506220121617.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <form action="">
        用户名: <input type="text" required="required" placeholder="请输入用户名" autofocus="autofocus" name="username" autocomplete="off"> <input type="submit" value="提交"> 上传头像: <input type="file" name="" id="" multiple="multiple">
    </form>
</body>

</html>
```

## 1.7 CSS3  属性选择器
```markdown
1. 什么是 CSS3

在CSS2 的基础上拓展、新增的样式

2. CSS3发展现状

移动端支持优于 PC 端
CSS3 目前还草案，在不断改进中
CSS3 相对 H5，应用非常广泛
```
```markdown
属性选择器列表
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020050622074214.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
属性选择器代码演示
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
        /* 属性选择器使用方法 */
        /* 选择的是：  既是button 又有 disabled 这个属性的元素 */
        /* 属性选择器的权重是 10 */
        /* 1.直接写属性 */
        
        button[disabled] {
            cursor: default;
        }
        
        button {
            cursor: pointer;
        }
        /* 2. 属性等于值 */
        
        input[type="search"] {
            color: pink;
        }
        /* 3. 以某个值开头的 属性值 */
        
        div[class^="icon"] {
            color: red;
        }
        /* 4. 以某个值结尾的 */
        
        div[class$="icon"] {
            color: green;
        }
        /* 5. 可以在任意位置的 */
        
        div[class*="icon"] {
            color: blue;
        }
    </style>
</head>

<body>
    <!-- disabled 是禁用我们的按钮 -->
    <button>按钮</button>
    <button>按钮</button>
    <button disabled="disabled">按钮</button>
    <button disabled="disabled">按钮</button>

    <input type="text" name="" id="" value="文本框">
    <input type="text" name="" id="" value="文本框">
    <input type="text" name="" id="" value="文本框">
    <input type="search" name="" id="" value="搜索框">
    <input type="search" name="" id="" value="搜索框">
    <input type="search" name="" id="" value="搜索框">
    <div class="icon1">图标1</div>
    <div class="icon2">图标2</div>
    <div class="icon3">图标3</div>
    <div class="iicon3">图标4</div>
    <div class="absicon">图标5</div>
</body>

</html>
```
## 1.8 结构伪类选择器
```markdown
属性列表
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200506223304997.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        ul li:first-child {
            background-color: pink;
        }
        
        ul li:last-child {
            background-color: deeppink;
        }
        /* nth-child(n)  我们要第几个，n就是几  比如我们选第8个， 我们直接写 8就可以了 */
        
        ul li:nth-child(8) {
            background-color: lightpink;
        }
    </style>
</head>

<body>
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
        <li>6</li>
        <li>7</li>
        <li>8</li>
        <li>9</li>
        <li>10</li>
    </ul>
</body>

</html>
```
```markdown
nth-child 参数详解

1. nth-child 详解
注意：本质上就是选中第几个子元素

n 可以是数字、关键字、公式

n 如果是数字，就是选中第几个

常见的关键字有 `even` 偶数、`odd` 奇数

常见的公式如下(如果 n 是公式，则从 0 开始计算)

但是第 0 个元素或者超出了元素的个数会被忽略
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200506223501672.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```html
nth-child
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        /* n 可是关键词  even 是偶数  odd 是奇数 */
        /* ul li:nth-child(even) {
            background-color: pink;
        }
        
        ul li:nth-child(odd) {
            background-color: hotpink;
        } */
        /* n 是公式  但是 n 从0开始计算 */
        /* ul li:nth-child(n) {
            background-color: pink;
        } */
        /* 2n 偶数  类似于 even */
        /* ul li:nth-child(2n) {
            background-color: pink;
        } */
        /* 2n + 1  类似于 odd */
        /* ul li:nth-child(2n+1) {
            background-color: skyblue;
        } */
        /* 5n  选择第  0  5  10  15 ... */
        /* ul li:nth-child(5n) {
            background-color: pink;
        } */
        /* n+5 就是从第5个开始往后面选择 包含第5个 */
        /* ul li:nth-child(n+5) {
            background-color: pink;
        } */
        /* -n + 5 就是选择前面5个  */
        
        ul li:nth-child(-n+5) {
            background-color: pink;
        }
    </style>
</head>

<body>
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
        <li>6</li>
        <li>7</li>
        <li>8</li>
        <li>9</li>
        <li>10</li>
    </ul>
</body>


nth-of-type
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        /* div :nth-child(1) {
            background-color: pink;
        }
        
        div :nth-child(2) {
            background-color: purple;
        } */
        /* div span:nth-child(1) {  这个选不到
            background-color: pink;
        } */
        
        div span:nth-child(2) {
            background-color: pink;
        }
        /* 总结： :nth-child(n)  选择 父元素里面的 第 n个孩子， 它不管里面的孩子是否同一种类型 */
        /* of-type 选择指定类型的元素 */
        
        div span:first-of-type {
            background-color: purple;
        }
        
        div span:last-of-type {
            background-color: skyblue;
        }
        
        div span:nth-of-type(2) {
            background-color: red;
        }
    </style>
</head>

<body>
    <div>
        <p>我是一个屁</p>
        <span>我是span</span>
        <span>我是span</span>
        <span>我是span</span>
    </div>
    <!-- ul 里面我们只允许放li  所以 nth-child 和 nth-of-type 就一样了 -->
    <ul>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
    </ul>
</body>

</html>
```

```markdown
nth-child 和  nth-of-type的区别
```
```html
<style>
  div :nth-child(1) {
    background-color: lightblue;
  }

  div :nth-child(2) {
    background-color: lightpink;
  }

  div span:nth-of-type(2) {
    background-color: lightseagreen;
  }

  div span:nth-of-type(3) {
    background-color: #fff;
  }
</style>
```

```markdown
区别
nth-child 选择父元素里面的第几个子元素，不管是第几个类型
nth-of-type 选择指定类型的元素
```
## 1.9 伪元素选择器
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200506223639419.png)
```markdown
伪类选择器注意事项
before 和 after必须有 content 属性
before在内容前面，after 在内容后面
before 和 after 创建的是一个元素，但是属于行内元素
创建出来的元素在 Dom中查找不到，所以称为伪元素
伪元素和标签选择器一样，权重为 1
```
```html
CSS3伪元素选择器
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        div {
            width: 300px;
            height: 300px;
            border: 1px solid #000;
        }
        
        div::before {
            content: "我";
            display: inline-block;
            width: 100px;
            height: 100px;
            background-color: pink;
        }
        
        div::after {
            content: "小猪佩奇";
            display: inline-block;
            width: 100px;
            height: 100px;
            background-color: pink;
        }
    </style>
</head>

<body>
    <div>是</div>
</body>

</html>


伪元素选择器案例
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        @font-face {
            font-family: 'icomoon';
            src: url('fonts/icomoon.eot?cv013x');
            src: url('fonts/icomoon.eot?cv013x#iefix') format('embedded-opentype'), url('fonts/icomoon.ttf?cv013x') format('truetype'), url('fonts/icomoon.woff?cv013x') format('woff'), url('fonts/icomoon.svg?cv013x#icomoon') format('svg');
            font-weight: normal;
            font-style: normal;
        }
        
        span {
            font-family: 'icomoon';
            position: absolute;
            top: 10px;
            right: 10px;
        }
        
        div,
        p {
            position: relative;
            width: 249px;
            height: 35px;
            border: 1px solid red;
        }
        /* p::after {
            content: '';
            position: absolute;
            top: 10px;
            right: 10px;
            font-family: 'icomoon';
        } */
        
        p::after {
            content: '\ea50';
            position: absolute;
            top: 10px;
            right: 10px;
            font-family: 'icomoon';
        }
    </style>
</head>

<body>
    <div>
        <span></span>
    </div>

    <p></p>
</body>

</html>
```
## 2.1 2D
### 2.1.1 2D转换之 translate
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        /* 移动盒子的位置： 定位   盒子的外边距  2d转换移动 */
        
        div {
            width: 200px;
            height: 200px;
            background-color: pink;
            /* x就是x轴上移动位置 y 就是y轴上移动位置 中间用逗号分隔*/
            /* transform: translate(x, y); */
            /* transform: translate(100px, 100px); */
            /* 1. 我们如果只移动x坐标 */
            /* transform: translate(100px, 0); */
            /* transform: translateX(100px); */
            /* 2. 我们如果只移动y坐标 */
            /* transform: translate(0, 100px); */
            /* transform: translateY(100px); */
        }
        
        div:first-child {
            transform: translate(30px, 30px);
        }
        
        div:last-child {
            background-color: purple;
        }
    </style>
</head>

<body>
    <div></div>
    <div></div>
</body>

</html>
```

```markdown
transition: all 0.3s; 这句话的作用就是给变化元素进行过渡
```
```markdown
1. 2D转换
2D 转换是改变标签在二维平面上的位置和形状
移动： translate
旋转： rotate
缩放： scale

2. translate 语法
x 就是 x 轴上水平移动
y 就是 y 轴上水平移动

transform: translate(x, y)
transform: translateX(n)
transfrom: translateY(n)

3. 重点知识点
2D的移动主要是指 水平、垂直方向上的移动
translate最大的优点就是不影响其他元素的位置
translate中的100%单位，是相对于本身的宽度和高度来进行计算的
行内标签没有效果
```
```markdown
让一个盒子水平垂直居中
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
        div {
            position: relative;
            width: 500px;
            height: 500px;
            background-color: pink;
            /* 1. 我们tranlate里面的参数是可以用 % */
            /* 2. 如果里面的参数是 % 移动的距离是 盒子自身的宽度或者高度来对比的 */
            /* 这里的 50% 就是 50px 因为盒子的宽度是 100px */
            /* transform: translateX(50%); */
        }
        
        p {
            position: absolute;
            top: 50%;
            left: 50%;
            width: 200px;
            height: 200px;
            background-color: purple;
            /* margin-top: -100px;
            margin-left: -100px; */
            /* translate(-50%, -50%)  盒子往上走自己高度的一半   */
            transform: translate(-50%, -50%);
        }
        
        span {
            /* translate 对于行内元素是无效的 */
            transform: translate(300px, 300px);
        }
    </style>
</head>

<body>
    <div>
        <p></p>
    </div>
    <span>123</span>
</body>

</html>
```

### 2.1.2 2D 转换 rotate
```markdown
1. rotate 旋转
2D旋转指的是让元素在二维平面内顺时针或者逆时针旋转

2.rotate语法
/* 单位是：deg */
transform: rotate(度数) 

3. 重点知识点
rotate 里面跟度数，单位是 `deg`
角度为正时，顺时针，角度为负时，逆时针
默认旋转的中心点是元素的中心点
```

```html
2D转换之旋转rotate
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        img {
            width: 150px;
            /* 顺时针旋转45度 */
            /* transform: rotate(45deg); */
            border-radius: 50%;
            border: 5px solid pink;
            /* 过渡写到本身上，谁做动画给谁加 */
            transition: all 0.3s;
        }
        
        img:hover {
            transform: rotate(360deg);
        }
    </style>
</head>

<body>
    <img src="media/pic.jpg" alt="">
</body>

</html>


CSS3书写三角
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        div {
            position: relative;
            width: 249px;
            height: 35px;
            border: 1px solid #000;
        }
        
        div::after {
            content: "";
            position: absolute;
            top: 8px;
            right: 15px;
            width: 10px;
            height: 10px;
            border-right: 1px solid #000;
            border-bottom: 1px solid #000;
            transform: rotate(45deg);
            transition: all 0.2s;
        }
        /* 鼠标经过div  里面的三角旋转 */
        
        div:hover::after {
            transform: rotate(225deg);
        }
    </style>
</head>

<body>
    <div></div>
</body>

</html>

设置元素转换中心点
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        div {
            width: 200px;
            height: 200px;
            background-color: pink;
            margin: 100px auto;
            transition: all 1s;
            /* 1.可以跟方位名词 */
            /* transform-origin: left bottom; */
            /* 2. 默认的是 50%  50%  等价于 center  center */
            /* 3. 可以是px 像素 */
            transform-origin: 50px 50px;
        }
        
        div:hover {
            transform: rotate(360deg);
        }
    </style>
</head>

<body>
    <div></div>
</body>

</html>


旋转中心点案例
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        div {
            overflow: hidden;
            width: 200px;
            height: 200px;
            border: 1px solid pink;
            margin: 10px;
            float: left;
        }
        
        div::before {
            content: "黑马";
            display: block;
            width: 100%;
            height: 100%;
            background-color: hotpink;
            transform: rotate(180deg);
            transform-origin: left bottom;
            transition: all 0.4s;
        }
        /* 鼠标经过div 里面的before 复原 */
        
        div:hover::before {
            transform: rotate(0deg);
        }
    </style>
</head>

<body>
    <div></div>
    <div></div>
    <div></div>
</body>

</html>
```
### 2.1.3 2D之旋转scale
```html
2D转换之旋转rotate
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        div {
            width: 200px;
            height: 200px;
            background-color: pink;
            margin: 100px auto;
            /* transform-origin: left bottom; */
        }
        
        div:hover {
            /* 1. 里面写的数字不跟单位 就是倍数的意思 1 就是1倍  2就是 2倍 */
            /* transform: scale(x, y); */
            /* transform: scale(2, 2); */
            /* 2. 修改了宽度为原来的2倍  高度 不变 */
            /* transform: scale(2, 1); */
            /* 3. 等比例缩放 同时修改宽度和高度，我们有简单的写法  以下是 宽度修改了2倍，高度默认和第一个参数一样*/
            /* transform: scale(2); */
            /* 4. 我们可以进行缩小  小于1 就是缩放 */
            /* transform: scale(0.5, 0.5); */
            /* transform: scale(0.5); */
            /* 5. scale 的优势之处： 不会影响其他的盒子 而且可以设置缩放的中心点*/
            /* width: 300px;
            height: 300px; */
            transform: scale(2);
        }
    </style>
</head>

<body>
    <div></div>
    123123
</body>

</html>

图片放大案例
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        div {
            overflow: hidden;
            float: left;
            margin: 10px;
        }
        
        div img {
            transition: all .4s;
        }
        
        div img:hover {
            transform: scale(1.1);
        }
    </style>
</head>

<body>
    <div>
        <a href="#"><img src="media/scale.jpg" alt=""></a>
    </div>
    <div>
        <a href="#"><img src="media/scale.jpg" alt=""></a>
    </div>
    <div>
        <a href="#"><img src="media/scale.jpg" alt=""></a>
    </div>
</body>

</html>


分页按钮案例
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        li {
            float: left;
            width: 30px;
            height: 30px;
            border: 1px solid pink;
            margin: 10px;
            text-align: center;
            line-height: 30px;
            list-style: none;
            border-radius: 50%;
            cursor: pointer;
            transition: all .4s;
        }
        
        li:hover {
            transform: scale(1.2);
        }
    </style>
</head>

<body>
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
        <li>6</li>
        <li>7</li>
    </ul>
</body>

</html>
```
### 2.1.4 2D转换综合写法
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        div {
            width: 200px;
            height: 200px;
            background-color: pink;
            transition: all .5s;
        }
        
        div:hover {
            /* transform: rotate(180deg) translate(150px, 50px); */
            /* 我们同时有位移和其他属性，我们需要把位移放到最前面 */
            transform: translate(150px, 50px) rotate(180deg) scale(1.2);
        }
    </style>
</head>

<body>
    <div></div>
</body>

</html>
```
### 2.1.5 动画名称的基本使用
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        /* 我们想页面一打开，一个盒子就从左边走到右边 */
        /* 1. 定义动画 */
        
        @keyframes move {
            /* 开始状态 */
            0% {
                transform: translateX(0px);
            }
            /* 结束状态 */
            100% {
                transform: translateX(1000px);
            }
        }
        
        div {
            width: 200px;
            height: 200px;
            background-color: pink;
            /* 2. 调用动画 */
            /* 动画名称 */
            animation-name: move;
            /* 持续时间 */
            animation-duration: 2s;
        }
    </style>
</head>

<body>
    <div></div>
</body>

</html>
```
### 2.1.6 动画序列
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        /* from to 等价于  0% 和  100% */
        /* @keyframes move {
            from {
                transform: translate(0, 0);
            }
            to {
                transform: translate(1000px, 0);
            }
        } */
        /* 动画序列 */
        /* 1. 可以做多个状态的变化 keyframe 关键帧 */
        /* 2. 里面的百分比要是整数 */
        /* 3. 里面的百分比就是 总的时间（我们这个案例10s）的划分 25% * 10  =  2.5s */
        
        @keyframes move {
            0% {
                transform: translate(0, 0);
            }
            25% {
                transform: translate(1000px, 0)
            }
            50% {
                transform: translate(1000px, 500px);
            }
            75% {
                transform: translate(0, 500px);
            }
            100% {
                transform: translate(0, 0);
            }
        }
        
        div {
            width: 100px;
            height: 100px;
            background-color: pink;
            animation-name: move;
            animation-duration: 10s;
        }
    </style>
</head>

<body>
    <div>

    </div>
</body>

</html>
```
### 2.1.7 动画属性
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200507090201671.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        @keyframes move {
            0% {
                transform: translate(0, 0);
            }
            100% {
                transform: translate(1000px, 0);
            }
        }
        
        div {
            width: 100px;
            height: 100px;
            background-color: pink;
            /* 动画名称 */
            animation-name: move;
            /* 持续时间 */
            /* animation-duration: 2s; */
            /* 运动曲线 */
            /* animation-timing-function: ease; */
            /* 何时开始 */
            animation-delay: 1s;
            /* 重复次数  iteration 重复的 conut 次数  infinite  无限 */
            /* animation-iteration-count: infinite; */
            /* 是否反方向播放 默认的是 normal  如果想要反方向 就写 alternate */
            /* animation-direction: alternate; */
            /* 动画结束后的状态 默认的是 backwards  回到起始状态 我们可以让他停留在结束状态 forwards */
            /* animation-fill-mode: forwards; */
            /* animation: name duration timing-function delay iteration-count direction fill-mode; */
            /* animation: move 2s linear 0s 1 alternate forwards; */
            /* 前面2个属性 name  duration 一定要写 */
            /* animation: move 2s linear  alternate forwards; */
        }
        
        div:hover {
            /* 鼠标经过div 让这个div 停止动画，鼠标离开就继续动画 */
            animation-play-state: paused;
        }
    </style>
</head>

<body>
    <div>

    </div>
</body>

</html>
```
### 2.1.8 大数据热点图
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        body {
            background-color: #333;
        }
        
        .map {
            position: relative;
            width: 747px;
            height: 616px;
            background: url(media/map.png) no-repeat;
            margin: 0 auto;
        }
        
        .city {
            position: absolute;
            top: 227px;
            right: 193px;
            color: #fff;
        }
        
        .tb {
            top: 500px;
            right: 80px;
        }
        
        .dotted {
            width: 8px;
            height: 8px;
            background-color: #09f;
            border-radius: 50%;
        }
        
        .city div[class^="pulse"] {
            /* 保证我们小波纹在父盒子里面水平垂直居中 放大之后就会中心向四周发散 */
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 8px;
            height: 8px;
            box-shadow: 0 0 12px #009dfd;
            border-radius: 50%;
            animation: pulse 1.2s linear infinite;
        }
        
        .city div.pulse2 {
            animation-delay: 0.4s;
        }
        
        .city div.pulse3 {
            animation-delay: 0.8s;
        }
        
        @keyframes pulse {
            0% {}
            70% {
                /* transform: scale(5);  我们不要用scale 因为他会让 阴影变大*/
                width: 40px;
                height: 40px;
                opacity: 1;
            }
            100% {
                width: 70px;
                height: 70px;
                opacity: 0;
            }
        }
    </style>
</head>

<body>
    <div class="map">
        <div class="city">
            <div class="dotted"></div>
            <div class="pulse1"></div>
            <div class="pulse2"></div>
            <div class="pulse3"></div>
        </div>
        <div class="city tb">
            <div class="dotted"></div>
            <div class="pulse1"></div>
            <div class="pulse2"></div>
            <div class="pulse3"></div>
        </div>
    </div>
</body>

</html>
```
### 2.1.9 速度曲线步长
```markdown
速度曲线细节
animation-timing-function: 规定动画的速度曲线，默认是ease
```

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200507110415478.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        div {
            overflow: hidden;
            font-size: 20px;
            width: 0;
            height: 30px;
            background-color: pink;
            /* 让我们的文字强制一行内显示 */
            white-space: nowrap;
            /* steps 就是分几步来完成我们的动画 有了steps 就不要在写 ease 或者linear 了 */
            animation: w 4s steps(10) forwards;
        }
        
        @keyframes w {
            0% {
                width: 0;
            }
            100% {
                width: 200px;
            }
        }
    </style>
</head>

<body>
    <div>世纪佳缘我在这里等你</div>
</body>

</html>
```
### 2.1.10 奔跑的熊大
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        body {
            background-color: #ccc;
        }
        
        div {
            position: absolute;
            width: 200px;
            height: 100px;
            background: url(media/bear.png) no-repeat;
            /* 我们元素可以添加多个动画， 用逗号分隔 */
            animation: bear .4s steps(8) infinite, move 3s forwards;
        }
        
        @keyframes bear {
            0% {
                background-position: 0 0;
            }
            100% {
                background-position: -1600px 0;
            }
        }
        
        @keyframes move {
            0% {
                left: 0;
            }
            100% {
                left: 50%;
                /* margin-left: -100px; */
                transform: translateX(-50%);
            }
        }
    </style>
</head>

<body>
    <div></div>
</body>

</html>
```
## 3.1 3D
### 3.1.1 认识3D转换
```markdown
transition实现过渡的效果。
transform实现转化的效果。比如旋转、平移、缩放
perspective实现近大远小的效果。给父盒子添加
transform-style: preserve-3d;开启3D效果，给父盒子添加，父盒子跟子盒子都有3D效果
```
### 3.1.2 3D移动
```markdown
3D 转换知识要点
3D位移：translate3d(x, y, z)
3D旋转：rotate3d(x, y, z)
透视：perspctive
3D呈现 transfrom-style

3D 移动 translate3d
transform: translateX(100px)：仅仅是在 x 轴上移动
transform: translateY(100px)：仅仅是在 y 轴上移动
transform: translateZ(100px)：仅仅是在 z 轴上移动
transform: translate3d(x, y, z)：其中x、y、z 分别指要移动的轴的方向的距离
注意：x, y, z 对应的值不能省略，不需要填写用 0 进行填充
语法
transform: translate3d(x, y, z)

代码演示
transform: translate3d(100px, 100px, 100px)
/* 注意：x, y, z 对应的值不能省略，不需要填写用 0 进行填充 */
transform: translate3d(100px, 100px, 0)

透视 perspective
透视需要写在被视察元素的父盒子上面
注意下方图片
d：就是视距，视距就是指人的眼睛到屏幕的距离

z：就是 z 轴，z 轴越大(正值)，我们看到的物体就越大
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200507124936853.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        body {
            /* 透视写到被观察元素的父盒子上面 */
            perspective: 200px;
        }
        
        div {
            width: 200px;
            height: 200px;
            background-color: pink;
            /* transform: translateX(100px);
            transform: translateY(100px); */
            /* transform: translateX(100px) translateY(100px) translateZ(100px); */
            /* 1. translateZ 沿着Z轴移动 */
            /* 2. translateZ 后面的单位我们一般跟px */
            /* 3. translateZ(100px) 向外移动100px （向我们的眼睛来移动的） */
            /* 4. 3D移动有简写的方法 */
            /* transform: translate3d(x,y,z); */
            /* transform: translate3d(100px, 100px, 100px); */
            /* 5. xyz是不能省略的，如果没有就写0 */
            transform: translate3d(400px, 100px, 100px);
        }
    </style>
</head>

<body>
    <div></div>
</body>

</html>
```
```markdown
translateZ
```
```markdown
translateZ 与 perspecitve 的区别
perspecitve给父级进行设置，translateZ给 子元素进行设
置不同的大小
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
        body {
            perspective: 500px;
        }
        
        div {
            width: 200px;
            height: 200px;
            background-color: pink;
            margin: 100px auto;
            transform: translateZ(0);
        }
    </style>
</head>

<body>
    <div></div>
    <div></div>
    <div></div>
</body>

</html>
```
### 3.1.3 3D旋转rotate
```markdown
语法
transform: rotateX(45deg)  -- 沿着 x 轴正方向旋转 45 度
transform: rotateY(45deg) -- 沿着 y 轴正方向旋转 45 度
transform: rotateZ(45deg) -- 沿着 z 轴正方向旋转 45 度
transform: rotate3d(x, y, z, 45deg)  -- 沿着自定义轴旋转 45 deg 为角度

transform: rotate3d(x, y, z, deg) -- 沿着自定义轴旋转 deg 为角度
x, y, z 表示旋转轴的矢量，是标识你是否希望沿着该轴进行旋转，最后一个标旋转的角度
transform: rotate3d(1, 1, 0, 180deg) -- 沿着对角线旋转 45deg
transform: rotate3d(1, 0, 0, 180deg) -- 沿着 x 轴旋转 45deg
```

```html
rotateX
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        body {
            perspective: 300px;
        }
        
        img {
            display: block;
            margin: 100px auto;
            transition: all 1s;
        }
        
        img:hover {
            transform: rotateX(45deg);
        }
    </style>
</head>

<body>
    <img src="media/pig.jpg" alt="">
</body>

</html>

rotateY
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        body {
            perspective: 500px;
        }
        
        img {
            display: block;
            margin: 100px auto;
            transition: all 1s;
        }
        
        img:hover {
            transform: rotateY(45deg);
        }
    </style>
</head>

<body>
    <img src="media/pig.jpg" alt="">
</body>

</html>


rotateZ
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        body {
            perspective: 500px;
        }
        
        img {
            display: block;
            margin: 100px auto;
            transition: all 1s;
        }
        
        img:hover {
            /* transform: rotateZ(180deg); */
            /* transform: rotate3d(x,y,z,deg); */
            /* transform: rotate3d(1, 0, 0, 45deg); */
            /* transform: rotate3d(0, 1, 0, 45deg); */
            transform: rotate3d(1, 1, 0, 45deg);
        }
    </style>
</head>

<body>
    <img src="media/pig.jpg" alt="">
</body>

</html>
```
### 3.1.4 3D 呈现 transform-style
```markdown
transform-style
控制子元素是否开启三维立体环境
transform-style: flat  代表子元素不开启 3D立体空
间，默认的
transform-style: preserve-3d子元素开启立体空间
代码写给父级，但是影响的是子盒子
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
        body {
            perspective: 500px;
        }
        
        .box {
            position: relative;
            width: 200px;
            height: 200px;
            margin: 100px auto;
            transition: all 2s;
            /* 让子元素保持3d立体空间环境 */
            transform-style: preserve-3d;
        }
        
        .box:hover {
            transform: rotateY(60deg);
        }
        
        .box div {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: pink;
        }
        
        .box div:last-child {
            background-color: purple;
            transform: rotateX(60deg);
        }
    </style>
</head>

<body>
    <div class="box">
        <div></div>
        <div></div>
    </div>
</body>

</html>
```
### 3.1.5  两面翻转的盒子
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        body {
            perspective: 400px;
        }
        
        .box {
            position: relative;
            width: 300px;
            height: 300px;
            margin: 100px auto;
            transition: all .4s;
            /* 让背面的紫色盒子保留立体空间 给父级添加的 */
            transform-style: preserve-3d;
        }
        
        .box:hover {
            transform: rotateY(180deg);
        }
        
        .front,
        .back {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            border-radius: 50%;
            font-size: 30px;
            color: #fff;
            text-align: center;
            line-height: 300px;
        }
        
        .front {
            background-color: pink;
            z-index: 1;
        }
        
        .back {
            background-color: purple;
            /* 像手机一样 背靠背 旋转 */
            transform: rotateY(180deg);
        }
    </style>
</head>

<body>
    <div class="box">
        <div class="front">黑马程序员</div>
        <div class="back">pink老师这里等你</div>
    </div>
</body>

</html>
```
### 3.1.6 3D导航栏案例
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
            margin: 100px;
        }
        
        ul li {
            float: left;
            margin: 0 5px;
            width: 120px;
            height: 35px;
            list-style: none;
            /* 一会我们需要给box 旋转 也需要透视 干脆给li加 里面的子盒子都有透视效果 */
            perspective: 500px;
        }
        
        .box {
            position: relative;
            width: 100%;
            height: 100%;
            transform-style: preserve-3d;
            transition: all .4s;
        }
        
        .box:hover {
            transform: rotateX(90deg);
        }
        
        .front,
        .bottom {
            position: absolute;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
        }
        
        .front {
            background-color: pink;
            z-index: 1;
            transform: translateZ(17.5px);
        }
        
        .bottom {
            background-color: purple;
            /* 这个x轴一定是负值 */
            /* 我们如果有移动 或者其他样式，必须先写我们的移动 */
            transform: translateY(17.5px) rotateX(-90deg);
        }
    </style>
</head>

<body>
    <ul>
        <li>
            <div class="box">
                <div class="front">黑马程序员</div>
                <div class="bottom">pink老师等你</div>
            </div>
        </li>
        <li>
            <div class="box">
                <div class="front">黑马程序员</div>
                <div class="bottom">pink老师等你</div>
            </div>
        </li>
        <li>
            <div class="box">
                <div class="front">黑马程序员</div>
                <div class="bottom">pink老师等你</div>
            </div>
        </li>
        <li>
            <div class="box">
                <div class="front">黑马程序员</div>
                <div class="bottom">pink老师等你</div>
            </div>
        </li>
        <li>
            <div class="box">
                <div class="front">黑马程序员</div>
                <div class="bottom">pink老师等你</div>
            </div>
        </li>
        <li>
            <div class="box">
                <div class="front">黑马程序员</div>
                <div class="bottom">pink老师等你</div>
            </div>
        </li>
    </ul>
</body>

</html>
```
### 3.1.7  旋转木马
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        body {
            perspective: 1000px;
        }
        
        section {
            position: relative;
            width: 300px;
            height: 200px;
            margin: 150px auto;
            transform-style: preserve-3d;
            /* 添加动画效果 */
            animation: rotate 10s linear infinite;
            background: url(media/pig.jpg) no-repeat;
        }
        
        section:hover {
            /* 鼠标放入section 停止动画 */
            animation-play-state: paused;
        }
        
        @keyframes rotate {
            0% {
                transform: rotateY(0);
            }
            100% {
                transform: rotateY(360deg);
            }
        }
        
        section div {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: url(media/dog.jpg) no-repeat;
        }
        
        section div:nth-child(1) {
            transform: rotateY(0) translateZ(300px);
        }
        
        section div:nth-child(2) {
            /* 先旋转好了再 移动距离 */
            transform: rotateY(60deg) translateZ(300px);
        }
        
        section div:nth-child(3) {
            /* 先旋转好了再 移动距离 */
            transform: rotateY(120deg) translateZ(300px);
        }
        
        section div:nth-child(4) {
            /* 先旋转好了再 移动距离 */
            transform: rotateY(180deg) translateZ(300px);
        }
        
        section div:nth-child(5) {
            /* 先旋转好了再 移动距离 */
            transform: rotateY(240deg) translateZ(300px);
        }
        
        section div:nth-child(6) {
            /* 先旋转好了再 移动距离 */
            transform: rotateY(300deg) translateZ(300px);
        }
    </style>
</head>

<body>
    <section>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
    </section>
</body>

</html>
```
### 3.1.8 3d立方体盒子
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		* {
			padding: 0;
			margin: 0;
		}
		body {
			padding: 200px 300px;
		}
		ul {
			position: relative;
			list-style: none;
			width: 300px;
			height: 300px;
			transform: rotate3d(1, 1, 0, -45deg);
			transform-style: preserve-3d;
			/* perspective: 500;
			-webkit-perspective: 500;
			perspective-origin: 10% 10%; */
		}
		ul > li {
			position: absolute;
			top: 0;
			left: 0;
			width: 300px;
			height: 300px;
			line-height: 300px;
			text-align: center;
			font-size: 40px;
		}
		ul > li:nth-of-type(1) {
			background-color: #ccc;
			transform: translateZ(150px);
		}
		ul > li:nth-of-type(2) {
			background-color: skyblue;
			transform: translateZ(-150px) rotateY(180deg);
		}
		ul > li:nth-of-type(3) {
			background-color: greenyellow;
			transform: translateX(-150px) rotateY(-90deg);
		}
		ul > li:nth-of-type(4) {
			background-color: deeppink;
			transform: translateX(150px) rotateY(90deg);
		}
		ul > li:nth-of-type(5) {
			background-color: rebeccapurple;
			transform: translateY(-150px) rotateX(90deg);
		}
		ul > li:nth-of-type(6) {
			background-color: saddlebrown;
			transform: translateY(150px) rotateX(-90deg);
		}
	</style>
</head>
<body>
	<ul>
		<li>01-前面</li>
		<li>02-后面</li>
		<li>03-左面</li>
		<li>04-右面</li>
		<li>05-上面</li>
		<li>06-下面</li>
	</ul>
</body>
</html>
```
### 3.1.9  浏览器私有前缀

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200507170214209.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复
h5c3即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
