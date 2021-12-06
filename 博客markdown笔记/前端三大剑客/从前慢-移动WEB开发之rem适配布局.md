# 移动WEB开发之rem适配布局
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714020211196.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1.1  rem 基础
```markdown
rem (root em)是一个相对单位，类似于em，em是父元素字体大小。

不同的是rem的基准是相对于html元素的字体大
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
        html {
            font-size: 12px;
        }
        
        div {
            font-size: 12px;
            width: 15rem;
            height: 15rem;
            background-color: purple;
        }
        
        p {
            /* 1. em相对于父元素 的字体大小来说的 */
            /* width: 10em;
            height: 10em; */
            /* 2. rem 相对于 html元素 字体大小来说的 */
            width: 10rem;
            height: 10rem;
            background-color: pink;
            /* 3.rem的优点就是可以通过修改html里面的文字大小来改变页面中元素的大小可以整体控制 */
        }
    </style>
</head>

<body>
    <div>
        <p></p>
    </div>

</body>

</html>
```
## 1.2  媒体查询
```markdown
媒体查询（Media Query）是CSS3新语法。
使用 @media查询，可以针对不同的媒体类型定义不同的样式
@media 可以针对不同的屏幕尺寸设置不同的样式
当你重置浏览器大小的过程中，页面也会根据浏览器的宽度和
高度重新渲染页面 
目前针对很多苹果手机、Android手机，平板等设备都用得到
多媒体查询
```
```css
@media mediatype and|not|only (media feature) {
    CSS-Code;
}
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
        /* 这句话的意思就是： 在我们屏幕上 并且 最大的宽度是 800像素 设置我们想要的样式 */
        /* max-width 小于等于800 */
        /* 媒体查询可以根据不同的屏幕尺寸在改变不同的样式 */
        
        @media screen and (max-width: 800px) {
            body {
                background-color: pink;
            }
        }
        
        @media screen and (max-width: 500px) {
            body {
                background-color: purple;
            }
        }
    </style>
</head>

<body>

</body>

</html>
```
## 1.3 mediatype 查询类型
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509134258592.png)
## 1.4 关键字
```markdown
关键字将媒体类型或多个媒体特性连接到一起做为媒体查询的条件。

and：可以将多个媒体特性连接到一起，相当于“且”的意思。
not：排除某个媒体类型，相当于“非”的意思，可以省略。
only：指定某个特定的媒体类型，可以省略.

```
## 1.5 媒体特性
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509134644344.png)
## 1.6 媒体查询案例修改背景颜色
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        /* 1. 媒体查询一般按照从大到小或者 从小到大的顺序来 */
        /* 2. 小于540px 页面的背景颜色变为蓝色 */
        
        @media screen and (max-width: 539px) {
            body {
                background-color: blue;
            }
        }
        /* 3. 540 ~ 970 我们的页面颜色改为 绿色 */
        /* @media screen and (min-width: 540px) and (max-width: 969px) {
            body {
                background-color: green;
            }
        } */
        
        @media screen and (min-width: 540px) {
            body {
                background-color: green;
            }
        }
        /* 4. 大于等于970 我们页面的颜色改为 红色 */
        
        @media screen and (min-width: 970px) {
            body {
                background-color: red;
            }
        }
        /* 5. screen 还有 and 必须带上不能省略的 */
        /* 6. 我们的数字后面必须跟单位  970px   这个 px 不能省略的 */
    </style>
</head>

<body>

</body>

</html>
```
## 1.7 媒体查询+rem实现元素动态变化
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
        /* html {
            font-size: 100px;
        } */
        /* 从小到大的顺序 */
        
        @media screen and (min-width: 320px) {
            html {
                font-size: 50px;
            }
        }
        
        @media screen and (min-width: 640px) {
            html {
                font-size: 100px;
            }
        }
        
        .top {
            height: 1rem;
            font-size: .5rem;
            background-color: green;
            color: #fff;
            text-align: center;
            line-height: 1rem;
        }
    </style>
</head>

<body>
    <div class="top">购物车</div>
</body>

</html>
```
## 1.8 引入资源
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        /* 当我们屏幕大于等于 640px以上的，我们让div 一行显示2个 */
        /* 当我们屏幕小于640 我们让div一行显示一个 */
        /* 一个建议： 我们媒体查询最好的方法是从小到大 */
        /* 引入资源就是 针对于不同的屏幕尺寸 调用不同的css文件 */
    </style>
    <link rel="stylesheet" href="style320.css" media="screen and (min-width: 320px)">
    <link rel="stylesheet" href="style640.css" media="screen and (min-width: 640px)">
</head>

<body>
    <div>1</div>
    <div>2</div>
</body>

</html>
```
## 1.9 less 基础
```markdown
Less中文网址：http://lesscss.cn/
需要安装node.js再安装less
npm install -g less
```
### 1.9.1 变量
```markdown
必须有@为前缀
不能包含特殊字符
不能以数字开头
大小写敏感
```
```css
// 定义一个粉色的变量
@color: pink;  
// 错误的变量名  @1color   @color~@#
// 变量名区分大小写  @color  和  @Color 是两个不同的变量
// 定义了一个 字体为14像素的变量
@font14: 14px;
body {
    background-color: @color;
}
div {
    color: @color;
    font-size: @font14;
}
a {
    font-size: @font14;
}

```
### 1.9.2 Less 嵌套
```css
.header {
   width: 200px;
   height: 200px;
   background-color: pink;
   // 1. less嵌套 子元素的样式直接写到父元素里面就好了
   a {
       color: red;
       // 2. 如果有伪类、交集选择器、 伪元素选择器 我们内层选择器的前面需要加&
       &:hover {
           color: blue;
       }
   }
}
.nav {
   .logo {
       color: green;
   }
   &::before {
       content: "";
   }
}
```
### 1.9.3 Less 运算
```markdown
任何数字、颜色或者变量都可以参与运算。就是Less提供
了加（+）、减（-）、乘（*）、除（/）算术运算。
```
```css
@baseFont: 50px;
html {
    font-size: @baseFont;
}
@border: 5px + 5;
div {
    width: 200px - 50;
    height: (200px + 50px ) * 2;
    border: @border solid red;
    background-color: #666 - #222;
}
img {
    width: 82rem / @baseFont;
    height: 82rem / @baseFont;
}
// 1. 我们运算符的左右两侧必须敲一个空格隔开
// 2. 两个数参与运算  如果只有一个数有单位，则最后的结果就以这个单位为准
// 3. 两个数参与运算，如果2个数都有单位，而且不一样的单位 最后的结果以第一个单位为准

```
## 1.10  rem适配方案
```markdown
1.让一些不能等比自适应的元素，达到当设备尺寸发生改变的时
候，等比例适配当前设备。

2.使用媒体查询根据不同设备按比例设置html的字体大小，然后页
面元素使用rem做尺寸单位，当html字体大小变化元素尺寸也会发
生变化，从而达到等比缩放的适配。
```
### 1.10.1 rem 适配方案1
```markdown
1.less+rem+媒体查询

2.flexible.js+rem
github地址：[https://github.com/amfe/lib-flexible]
```
### 1.10.2 rem实际开发适配方案
```markdown
1 假设设计稿是750px

2 假设我们把整个屏幕划分为15等份（划分标准不一可以是20份也
可以是10等份）

3 每一份作为html字体大小，这里就是50px

4 那么在320px设备的时候，字体大小为320/15就是  21.33px

5 用我们页面元素的大小除以不同的 html字体大小会发现他们比例
还是相同的

6 比如我们以750为标准设计稿

7 一个100*100像素的页面元素在  750屏幕下，  
就是 100/ 50  转换为rem  是  2rem*2rem  比例是1比1

8 320屏幕下，  html字体大小为21.33   
则 2rem=  42.66px  此时宽和高都是 42.66  
但是宽和高的比例还是 1比1

9 但是已经能实现不同屏幕下  页面元素盒子等比例缩放的效果
```
### 1.10.3 总结
```markdown
1 最后的公式：页面元素的rem值 =  页面元素值（px） /  （屏幕宽度  /  划分的份数）

2 屏幕宽度/划分的份数就是 html:font-size 的大小

3 或者：页面元素的rem值 =  页面元素值（px） /  html: font-size 字体大小
```
### 1.10.4 rem 适配方案2
```markdown
手机淘宝团队出的简洁高效 移动端适配库

我们再也不需要在写不同屏幕的媒体查询，因为里面js做了处理

它的原理是把当前设备划分为10等份，但是不同设备下，比
例还是一致的。

我们要做的，就是确定好我们当前设备的html 文字大小就可以了
比如当前设计稿是 750px， 那么我们只需要把 html 文字大小设置
为 75px(750px / 10) 就可以
里面页面元素rem值： 页面元素的px 值 /  75  
剩余的，让flexible.js来去算
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复
rem即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
