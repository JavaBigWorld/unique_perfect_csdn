# jQuery和Bootstrap
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714003817870.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714003919724.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

# jQuery
## 1.1 认识jQuery
~~~markdown
jQuery是一款优秀的 JavaScript库，从命名可以看出 jQuery
最主要的用途是用来做查询（ jQuery=js+ Query），
正如jQuey官方Logo副标题所说(write less, do more)
使用jQuery能让我们对HTML文档遍历和操作、事件
处理、动画以及Ajax变得更加简单
官网地址： https://jquery.com/
~~~

## 1.2 jQuery 的入口函数
~~~markdown
1.$(document).ready(function(){
           window.alert("nihao");
       });
2.	$(function(){
           window.alert("nihao");
       });
~~~
```markdown
等着 DOM 结构渲染完毕即可执行内部代码，不必等到所有
外部资源加载完成，jQuery 帮我们完成了封装。
相当于原生 js 中的 DOMContentLoaded。
不同于原生 js 中的 load 事件是等页面文档、外部的 js 
文件、css文件、图片加载完毕才执行内部代码。
更推荐使用第一种方式。
```
```html
<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <script src="jquery.min.js"></script>
   <style>
       div {
           width: 200px;
           height: 200px;
           background-color: pink;
       }
   </style>
</head>

<body>
   <script>
       // $('div').hide();
       // 1. 等着页面DOM加载完毕再去执行js 代码
       // $(document).ready(function() {
       //     $('div').hide();
       // })
       // 2.  等着页面DOM加载完毕再去执行js 代码
       $(function() {
           $('div').hide();

       })
   </script>
   <div></div>

</body>

</html>
```
## 1.3  jQuery 对象和 DOM 对象
```markdown
用原生 JS 获取来的对象就是 DOM 对象
jQuery 方法获取的元素就是 jQuery 对象。
jQuery 对象本质是： 利用$对DOM 对象包装后产生的对
象（伪数组形式存储）。

1 $ 是 jQuery 的别称，在代码中可以使用 jQuery 代替 $，但
一般为了方便，通常都直接使用 $ 。
2 $ 是jQuery 的顶级对象， 相当于原生JavaScript中
的 window。把元素利用$包装成jQuery对象，就可以调
用jQuery 的方法。
注意：
只有 jQuery 对象才能使用 jQuery 方法，DOM 对象则使用原
生的 JavaScirpt 方法。
```
```html
jQuery顶级对象$
<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <script src="jquery.min.js"></script>
   <style>
       div {
           width: 200px;
           height: 200px;
           background-color: pink;
       }
   </style>
</head>

<body>
   <div></div>
   <script>
       // 1. $ 是jQuery的别称（另外的名字）
       // $(function() {
       //     alert(11)
       // });
       jQuery(function() {
           // alert(11)
           // $('div').hide();
           jQuery('div').hide();
       });
       // 2. $同时也是jQuery的 顶级对象
   </script>
</body>

</html>

jQuery对象和DOM对象
<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <script src="jquery.min.js"></script>
   <style>
       div {
           width: 100px;
           height: 100px;
           background-color: pink;
       }
   </style>
</head>

<body>
   <div></div>
   <span></span>
   <script>
       // 1. DOM 对象：  用原生js获取过来的对象就是DOM对象
       var myDiv = document.querySelector('div'); // myDiv 是DOM对象
       var mySpan = document.querySelector('span'); // mySpan 是DOM对象
       console.dir(myDiv);
       // 2. jQuery对象： 用jquery方式获取过来的对象是jQuery对象。 本质：通过$把DOM元素进行了包装
       $('div'); // $('div')是一个jQuery 对象
       $('span'); // $('span')是一个jQuery 对象
       console.dir($('div'));
       // 3. jQuery 对象只能使用 jQuery 方法，DOM 对象则使用原生的 JavaScirpt 属性和方法
       // myDiv.style.display = 'none';
       // myDiv.hide(); myDiv是一个dom对象不能使用 jquery里面的hide方法
       // $('div').style.display = 'none'; 这个$('div')是一个jQuery对象不能使用原生js 的属性和方法
   </script>
</body>

</html>
```
## 1.4 DOM 对象与 jQuery 对象相互转换
```markdown
因为原生js 比 jQuery 更大，原生的一些属性和方法 jQuery没
有给我们封装. 要想使用这些属性和方法需要把jQuery对象转
换为DOM对象才能使用。
```
```markdown
DOM 对象转换为 jQuery 对象： $(DOM对象)
$('div') 
jQuery 对象转换为 DOM 对象（两种方式）
$('div') [index] index 是索引号 
$('div') .get(index) index 是索引号   
```
```html
<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <script src="jquery.min.js"></script>
</head>

<body>
   <video src="mov.mp4" muted></video>
   <script>
       // 1. DOM对象转换为 jQuery对象
       // (1) 我们直接获取视频，得到就是jQuery对象
       // $('video');
       // (2) 我们已经使用原生js 获取过来 DOM对象
       var myvideo = document.querySelector('video');
       // $(myvideo).play();  jquery里面没有play 这个方法
       // 2.  jQuery对象转换为DOM对象
       // myvideo.play();
       $('video')[0].play()
       $('video').get(0).play()
   </script>
</body>

</html>
```
## 1.5 jQuery 选择器
### 1.5.1  jQuery 基础选择器
```markdown
原生 JS 获取元素方式很多，很杂，而且兼容性情况不
一致，因此 jQuery 给我们做了封装，使获取元素统一标准。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200515191850112.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

### 1.5.2 jQuery 层级选择器
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200515192031630.png)
```html
<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <script src="jquery.min.js"></script>
</head>

<body>
   <div>我是div</div>
   <div class="nav">我是nav div</div>
   <p>我是p</p>
   <ol>
       <li>我是ol 的</li>
       <li>我是ol 的</li>
       <li>我是ol 的</li>
       <li>我是ol 的</li>
   </ol>
   <ul>
       <li>我是ul 的</li>
       <li>我是ul 的</li>
       <li>我是ul 的</li>
       <li>我是ul 的</li>
   </ul>
   <script>
       $(function() {
           console.log($(".nav"));
           console.log($("ul li"));

       })
   </script>
</body>

</html>
```
### 1.5.3 隐式迭代
```markdown
遍历内部 DOM 元素（伪数组形式存储）的过程就叫做隐式迭代。
简单理解：给匹配到的所有元素进行循环遍历，执行相应
的方法，而不用我们再进行循环，简化我们的操作，
方便我们调用。
```
```html
<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <script src="jquery.min.js"></script>
</head>

<body>
   <div>惊喜不，意外不</div>
   <div>惊喜不，意外不</div>
   <div>惊喜不，意外不</div>
   <div>惊喜不，意外不</div>
   <ul>
       <li>相同的操作</li>
       <li>相同的操作</li>
       <li>相同的操作</li>
   </ul>
   <script>
       // 1. 获取四个div元素 
       console.log($("div"));

       // 2. 给四个div设置背景颜色为粉色 jquery对象不能使用style
       $("div").css("background", "pink");
       // 3. 隐式迭代就是把匹配的所有元素内部进行遍历循环，给每一个元素添加css这个方法
       $("ul li").css("color", "red");
   </script>
</body>

</html>
```
### 1.5.4 jQuery 筛选选择器
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200515192245820.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```html
<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <script src="jquery.min.js"></script>
</head>

<body>
   <ul>
       <li>多个里面筛选几个</li>
       <li>多个里面筛选几个</li>
       <li>多个里面筛选几个</li>
       <li>多个里面筛选几个</li>
       <li>多个里面筛选几个</li>
       <li>多个里面筛选几个</li>
   </ul>
   <ol>
       <li>多个里面筛选几个</li>
       <li>多个里面筛选几个</li>
       <li>多个里面筛选几个</li>
       <li>多个里面筛选几个</li>
       <li>多个里面筛选几个</li>
       <li>多个里面筛选几个</li>
   </ol>
   <script>
       $(function() {
           $("ul li:first").css("color", "red");
           $("ul li:eq(2)").css("color", "blue");
           $("ol li:odd").css("color", "skyblue");
           $("ol li:even").css("color", "pink");
       })
   </script>
</body>

</html>
```
### 1.5.5 jQuery 筛选方法（重点）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200515192417992.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```html
jQuery筛选方法（上）
<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <script src="jquery.min.js"></script>
</head>

<body>
   <div class="yeye">
       <div class="father">
           <div class="son">儿子</div>
       </div>
   </div>

   <div class="nav">
       <p>我是屁</p>
       <div>
           <p>我是p</p>
       </div>
   </div>
   <script>
       // 注意一下都是方法 带括号
       $(function() {
           // 1. 父  parent()  返回的是 最近一级的父级元素 亲爸爸
           console.log($(".son").parent());
           // 2. 子
           // (1) 亲儿子 children()  类似子代选择器  ul>li
           // $(".nav").children("p").css("color", "red");
           // (2) 可以选里面所有的孩子 包括儿子和孙子  find() 类似于后代选择器
           $(".nav").find("p").css("color", "red");
           // 3. 兄
       });
   </script>
</body>

</html>

jQuery筛选方法（下）
<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <script src="jquery.min.js"></script>
</head>

<body>
   <ol>
       <li>我是ol 的li</li>
       <li>我是ol 的li</li>
       <li class="item">我是ol 的li</li>
       <li>我是ol 的li</li>
       <li>我是ol 的li</li>
       <li>我是ol 的li</li>
   </ol>
   <ul>
       <li>我是ol 的li</li>
       <li>我是ol 的li</li>
       <li>我是ol 的li</li>
       <li>我是ol 的li</li>
       <li>我是ol 的li</li>
       <li>我是ol 的li</li>
   </ul>
   <div class="current">俺有current</div>
   <div>俺木有current</div>
   <script>
       // 注意一下都是方法 带括号
       $(function() {
           // 1. 兄弟元素siblings 除了自身元素之外的所有亲兄弟
           $("ol .item").siblings("li").css("color", "red");
           // 2. 第n个元素
           var index = 2;
           // (1) 我们可以利用选择器的方式选择
           // $("ul li:eq(2)").css("color", "blue");
           // $("ul li:eq("+index+")").css("color", "blue");
           // (2) 我们可以利用选择方法的方式选择 更推荐这种写法
           // $("ul li").eq(2).css("color", "blue");
           // $("ul li").eq(index).css("color", "blue");
           // 3. 判断是否有某个类名
           console.log($("div:first").hasClass("current"));
           console.log($("div:last").hasClass("current"));


       });
   </script>
</body>

</html>
```
### 1.5.6 jQuery 里面的排他思想
```markdown
想要多选一的效果，排他思想：当前元素设置样式，其余
的兄弟元素清除样式 
```
```css
<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <script src="jquery.min.js"></script>
</head>

<body>
   <button>快速</button>
   <button>快速</button>
   <button>快速</button>
   <button>快速</button>
   <button>快速</button>
   <button>快速</button>
   <button>快速</button>
   <script>
       $(function() {
           // 1. 隐式迭代 给所有的按钮都绑定了点击事件
           $("button").click(function() {
               // 2. 当前的元素变化背景颜色
               $(this).css("background", "pink");
               // 3. 其余的兄弟去掉背景颜色 隐式迭代
               $(this).siblings("button").css("background", "");
           });
       })
   </script>
</body>

</html>
```
### 1.5.7 淘宝精品服饰案例
```html
<!DOCTYPE html>
<html>

<head lang="en">
   <meta charset="UTF-8">
   <title></title>
   <style type="text/css">
       * {
           margin: 0;
           padding: 0;
           font-size: 12px;
       }
       
       ul {
           list-style: none;
       }
       
       a {
           text-decoration: none;
       }
       
       .wrapper {
           width: 250px;
           height: 248px;
           margin: 100px auto 0;
           border: 1px solid pink;
           border-right: 0;
           overflow: hidden;
       }
       
       #left,
       #content {
           float: left;
       }
       
       #left li {
           background: url(images/lili.jpg) repeat-x;
       }
       
       #left li a {
           display: block;
           width: 48px;
           height: 27px;
           border-bottom: 1px solid pink;
           line-height: 27px;
           text-align: center;
           color: black;
       }
       
       #left li a:hover {
           background-image: url(images/abg.gif);
       }
       
       #content {
           border-left: 1px solid pink;
           border-right: 1px solid pink;
       }
   </style>
   <script src="jquery.min.js"></script>
   <script>
       $(function() {
           // 1. 鼠标经过左侧的小li 
           $("#left li").mouseover(function() {
               // 2. 得到当前小li 的索引号
               var index = $(this).index();
               console.log(index);
               // 3. 让我们右侧的盒子相应索引号的图片显示出来就好了
               // $("#content div").eq(index).show();
               // 4. 让其余的图片（就是其他的兄弟）隐藏起来
               // $("#content div").eq(index).siblings().hide();
               // 链式编程
               $("#content div").eq(index).show().siblings().hide();

           })
       })
   </script>
</head>

<body>
   <div class="wrapper">
       <ul id="left">
           <li><a href="#">女靴</a></li>
           <li><a href="#">雪地靴</a></li>
           <li><a href="#">冬裙</a></li>
           <li><a href="#">呢大衣</a></li>
           <li><a href="#">毛衣</a></li>
           <li><a href="#">棉服</a></li>
           <li><a href="#">女裤</a></li>
           <li><a href="#">羽绒服</a></li>
           <li><a href="#">牛仔裤</a></li>
       </ul>
       <div id="content">
           <div>
               <a href="#"><img src="images/女靴.jpg" width="200" height="250" /></a>
           </div>
           <div>
               <a href="#"><img src="images/雪地靴.jpg" width="200" height="250" /></a>
           </div>
           <div>
               <a href="#"><img src="images/冬裙.jpg" width="200" height="250" /></a>
           </div>
           <div>
               <a href="#"><img src="images/呢大衣.jpg" width="200" height="250" /></a>
           </div>
           <div>
               <a href="#"><img src="images/毛衣.jpg" width="200" height="250" /></a>
           </div>
           <div>
               <a href="#"><img src="images/棉服.jpg" width="200" height="250" /></a>
           </div>
           <div>
               <a href="#"><img src="images/女裤.jpg" width="200" height="250" /></a>
           </div>
           <div>
               <a href="#"><img src="images/羽绒服.jpg" width="200" height="250" /></a>
           </div>
           <div>
               <a href="#"><img src="images/牛仔裤.jpg" width="200" height="250" /></a>
           </div>

       </div>


   </div>
</body>

</html>
```
### 1.5.8 链式编程
```html
<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <script src="jquery.min.js"></script>
</head>

<body>
   <button>快速</button>
   <button>快速</button>
   <button>快速</button>
   <button>快速</button>
   <button>快速</button>
   <button>快速</button>
   <button>快速</button>
   <script>
       $(function() {
           // 1. 隐式迭代 给所有的按钮都绑定了点击事件
           $("button").click(function() {
               // 2. 让当前元素颜色变为红色
               // $(this).css("color", "red");
               // 3. 让其余的姐妹元素不变色 
               // $(this).siblings().css("color", "");
               // 链式编程
               $(this).css("color", "red").siblings().css("color", "");
           });
       })
   </script>
</body>

</html>
```
## 1.6 jQuery 样式操作
```markdown
参数只写属性名，则是返回属性值
$(this).css(''color''); 
参数是属性名，属性值，逗号分隔，是设置一组样式，属性必
须加引号，值如果是数字可以不用跟单位和引号
参数可以是对象形式，方便设置多组样式。属性名和属性值用
冒号隔开， 属性可以不用加引号.
```
### 1.6.1 jQuery操作样式之css方法
```html
<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <script src="jquery.min.js"></script>
   <style>
       div {
           width: 200px;
           height: 200px;
           background-color: pink;
       }
   </style>
</head>

<body>
   <div></div>
   <script>
       // 操作样式之css方法
       $(function() {
           console.log($("div").css("width"));
           // $("div").css("width", "300px");
           // $("div").css("width", 300);
           // $("div").css(height, "300px"); 属性名一定要加引号
           $("div").css({
               width: 400,
               height: 400,
               backgroundColor: "red"
                   // 如果是复合属性则必须采取驼峰命名法，如果值不是数字，则需要加引号
           })
       })
   </script>
</body>

</html>
```
### 1.6.2 设置类样式方法
```markdown
1.添加类
$(“div”).addClass(''current'');
2.移除类
$(“div”).removeClass(''current'');
3.切换类
$(“div”).toggleClass(''current'');
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
           width: 150px;
           height: 150px;
           background-color: pink;
           margin: 100px auto;
           transition: all 0.5s;
       }
       
       .current {
           background-color: red;
           transform: rotate(360deg);
       }
   </style>
   <script src="jquery.min.js"></script>
</head>

<body>
   <div class="current"></div>
   <script>
       $(function() {
           // 1. 添加类 addClass()
           // $("div").click(function() {
           //     // $(this).addClass("current");
           // });
           // 2. 删除类 removeClass()
           // $("div").click(function() {
           //     $(this).removeClass("current");
           // });
           // 3. 切换类 toggleClass()
           $("div").click(function() {
               $(this).toggleClass("current");
           });
       })
   </script>
</body>

</html>
```
### 1.6.3 tab栏切换
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
       
       li {
           list-style-type: none;
       }
       
       .tab {
           width: 978px;
           margin: 100px auto;
       }
       
       .tab_list {
           height: 39px;
           border: 1px solid #ccc;
           background-color: #f1f1f1;
       }
       
       .tab_list li {
           float: left;
           height: 39px;
           line-height: 39px;
           padding: 0 20px;
           text-align: center;
           cursor: pointer;
       }
       
       .tab_list .current {
           background-color: #c81623;
           color: #fff;
       }
       
       .item_info {
           padding: 20px 0 0 20px;
       }
       
       .item {
           display: none;
       }
   </style>
   <script src="jquery.min.js"></script>
</head>

<body>
   <div class="tab">
       <div class="tab_list">
           <ul>
               <li class="current">商品介绍</li>
               <li>规格与包装</li>
               <li>售后保障</li>
               <li>商品评价（50000）</li>
               <li>手机社区</li>
           </ul>
       </div>
       <div class="tab_con">
           <div class="item" style="display: block;">
               商品介绍模块内容
           </div>
           <div class="item">
               规格与包装模块内容
           </div>
           <div class="item">
               售后保障模块内容
           </div>
           <div class="item">
               商品评价（50000）模块内容
           </div>
           <div class="item">
               手机社区模块内容
           </div>

       </div>
   </div>
   <script>
       $(function() {
           // 1.点击上部的li，当前li 添加current类，其余兄弟移除类
           $(".tab_list li").click(function() {
               // 链式编程操作
               $(this).addClass("current").siblings().removeClass("current");
               // 2.点击的同时，得到当前li 的索引号
               var index = $(this).index();
               console.log(index);
               // 3.让下部里面相应索引号的item显示，其余的item隐藏
               $(".tab_con .item").eq(index).show().siblings().hide();
           });
       })
   </script>
</body>

</html>
```
### 1.6.4 类操作与className区别
```markdown
原生 JS 中 className 会覆盖元素原先里面的类名。
jQuery 里面类操作只是对指定类进行操作，不影响原先的类名。
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
       .one {
           width: 200px;
           height: 200px;
           background-color: pink;
           transition: all .3s;
       }
       
       .two {
           transform: rotate(720deg);
       }
   </style>
   <script src="jquery.min.js"></script>
</head>

<body>
   <div class="one two"></div>
   <script>
       // var one = document.querySelector(".one");
       // one.className = "two";
       // $(".one").addClass("two");  这个addClass相当于追加类名 不影响以前的类名
       $(".one").removeClass("two");
   </script>
</body>

</html>
```

## 1.7 jQuery 效果
```markdown
显示隐藏 show()hide() toggle()
滑动slideDown()slideUp()slideToggle()
淡入淡出fadeIn()fadeOut()fadeToggle()fadeTo()
自定义动画animate()
```
### 1.7.1 显示效果 
```markdown
show([speed,[easing],[fn]])
（1）参数都可以省略， 无动画直接显示。
（2）speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
（3）easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
（4）fn: 回调函数，在动画完成时执行的函数，每个元素执行一次
```
### 1.7.2 隐藏效果

```markdown
hide([speed,[easing],[fn]])
（1）参数都可以省略， 无动画直接显示。
（2）speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
（3）easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
（4）fn: 回调函数，在动画完成时执行的函数，每个元素执行一次。
```
### 1.7.3 jQuery效果之显示与隐藏
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
           width: 150px;
           height: 300px;
           background-color: pink;
       }
   </style>
   <script src="jquery.min.js"></script>
</head>

<body>
   <button>显示</button>
   <button>隐藏</button>
   <button>切换</button>
   <div></div>
   <script>
       $(function() {
           $("button").eq(0).click(function() {
               $("div").show(1000, function() {
                   alert(1);
               });
           })
           $("button").eq(1).click(function() {
               $("div").hide(1000, function() {
                   alert(1);
               });
           })
           $("button").eq(2).click(function() {
                   $("div").toggle(1000);
               })
               // 一般情况下，我们都不加参数直接显示隐藏就可以了
       });
   </script>
</body>

</html>
```

### 1.7.4  切换效果
```markdown
toggle([speed,[easing],[fn]])
（1）参数都可以省略， 无动画直接显示。
（2）speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
（3）easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
（4）fn: 回调函数，在动画完成时执行的函数，每个元素执行一次。
```
### 1.7.5 下滑效果
```markdown
slideDown([speed,[easing],[fn]])
```
```markdown
（1）参数都可以省略。
（2）speed:三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
（3）easing:(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
（4）fn: 回调函数，在动画完成时执行的函数，每个元素执行一次。
```
### 1.7.6 上滑效果

```markdown
slideUp([speed,[easing],[fn]])
（1）参数都可以省略。
（2）speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
（3）easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
（4）fn: 回调函数，在动画完成时执行的函数，每个元素执行一次。
```
### 1.7.7 滑动切换效果
```markdown
slideToggle([speed,[easing],[fn]])
（1）参数都可以省略。
（2）speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
（3）easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
（4）fn: 回调函数，在动画完成时执行的函数，每个元素执行一次。
```
### 1.7.8 jQuery效果之滑动
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
           width: 150px;
           height: 300px;
           background-color: pink;
           display: none;
       }
   </style>
   <script src="jquery.min.js"></script>
</head>

<body>
   <button>下拉滑动</button>
   <button>上拉滑动</button>
   <button>切换滑动</button>
   <div></div>
   <script>
       $(function() {
           $("button").eq(0).click(function() {
               // 下滑动 slideDown()
               $("div").slideDown();
           })
           $("button").eq(1).click(function() {
               // 上滑动 slideUp()
               $("div").slideUp(500);


           })
           $("button").eq(2).click(function() {
               // 滑动切换 slideToggle()

               $("div").slideToggle(500);

           });

       });
   </script>
</body>

</html>
```
### 1.7.9 简洁版滑动下拉菜单
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
       
       li {
           list-style-type: none;
       }
       
       a {
           text-decoration: none;
           font-size: 14px;
       }
       
       .nav {
           margin: 100px;
       }
       
       .nav>li {
           position: relative;
           float: left;
           width: 80px;
           height: 41px;
           text-align: center;
       }
       
       .nav li a {
           display: block;
           width: 100%;
           height: 100%;
           line-height: 41px;
           color: #333;
       }
       
       .nav>li>a:hover {
           background-color: #eee;
       }
       
       .nav ul {
           display: none;
           position: absolute;
           top: 41px;
           left: 0;
           width: 100%;
           border-left: 1px solid #FECC5B;
           border-right: 1px solid #FECC5B;
       }
       
       .nav ul li {
           border-bottom: 1px solid #FECC5B;
       }
       
       .nav ul li a:hover {
           background-color: #FFF5DA;
       }
   </style>
   <script src="jquery.min.js"></script>
</head>

<body>
   <ul class="nav">
       <li>
           <a href="#">微博</a>
           <ul>
               <li>
                   <a href="">私信</a>
               </li>
               <li>
                   <a href="">评论</a>
               </li>
               <li>
                   <a href="">@我</a>
               </li>
           </ul>
       </li>
       <li>
           <a href="#">微博</a>
           <ul>
               <li>
                   <a href="">私信</a>
               </li>
               <li>
                   <a href="">评论</a>
               </li>
               <li>
                   <a href="">@我</a>
               </li>
           </ul>
       </li>
       <li>
           <a href="#">微博</a>
           <ul>
               <li>
                   <a href="">私信</a>
               </li>
               <li>
                   <a href="">评论</a>
               </li>
               <li>
                   <a href="">@我</a>
               </li>
           </ul>
       </li>
       <li>
           <a href="#">微博</a>
           <ul>
               <li>
                   <a href="">私信</a>
               </li>
               <li>
                   <a href="">评论</a>
               </li>
               <li>
                   <a href="">@我</a>
               </li>
           </ul>
       </li>
   </ul>
   <script>
       $(function() {
           // 鼠标经过
           // $(".nav>li").mouseover(function() {
           //     // $(this) jQuery 当前元素  this不要加引号
           //     // show() 显示元素  hide() 隐藏元素
           //     $(this).children("ul").slideDown(200);
           // });
           // // 鼠标离开
           // $(".nav>li").mouseout(function() {
           //     $(this).children("ul").slideUp(200);
           // });
           // 1. 事件切换 hover 就是鼠标经过和离开的复合写法
           // $(".nav>li").hover(function() {
           //     $(this).children("ul").slideDown(200);
           // }, function() {
           //     $(this).children("ul").slideUp(200);
           // });
           // 2. 事件切换 hover  如果只写一个函数，那么鼠标经过和鼠标离开都会触发这个函数
           $(".nav>li").hover(function() {
               $(this).children("ul").slideToggle();
           });
       })
   </script>
</body>

</html>
```
### 1.7.10 事件切换
```markdown
（1）over:鼠标移到元素上要触发的函数（相当于mouseenter）
（2）out:鼠标移出元素要触发的函数（相当于mouseleave）
（3）如果只写一个函数，则鼠标经过和离开都会触发它
```
#### 1.7.10.1 简洁版滑动下拉菜单
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
       
       li {
           list-style-type: none;
       }
       
       a {
           text-decoration: none;
           font-size: 14px;
       }
       
       .nav {
           margin: 100px;
       }
       
       .nav>li {
           position: relative;
           float: left;
           width: 80px;
           height: 41px;
           text-align: center;
       }
       
       .nav li a {
           display: block;
           width: 100%;
           height: 100%;
           line-height: 41px;
           color: #333;
       }
       
       .nav>li>a:hover {
           background-color: #eee;
       }
       
       .nav ul {
           display: none;
           position: absolute;
           top: 41px;
           left: 0;
           width: 100%;
           border-left: 1px solid #FECC5B;
           border-right: 1px solid #FECC5B;
       }
       
       .nav ul li {
           border-bottom: 1px solid #FECC5B;
       }
       
       .nav ul li a:hover {
           background-color: #FFF5DA;
       }
   </style>
   <script src="jquery.min.js"></script>
</head>

<body>
   <ul class="nav">
       <li>
           <a href="#">微博</a>
           <ul>
               <li>
                   <a href="">私信</a>
               </li>
               <li>
                   <a href="">评论</a>
               </li>
               <li>
                   <a href="">@我</a>
               </li>
           </ul>
       </li>
       <li>
           <a href="#">微博</a>
           <ul>
               <li>
                   <a href="">私信</a>
               </li>
               <li>
                   <a href="">评论</a>
               </li>
               <li>
                   <a href="">@我</a>
               </li>
           </ul>
       </li>
       <li>
           <a href="#">微博</a>
           <ul>
               <li>
                   <a href="">私信</a>
               </li>
               <li>
                   <a href="">评论</a>
               </li>
               <li>
                   <a href="">@我</a>
               </li>
           </ul>
       </li>
       <li>
           <a href="#">微博</a>
           <ul>
               <li>
                   <a href="">私信</a>
               </li>
               <li>
                   <a href="">评论</a>
               </li>
               <li>
                   <a href="">@我</a>
               </li>
           </ul>
       </li>
   </ul>
   <script>
       $(function() {
           // 鼠标经过
           // $(".nav>li").mouseover(function() {
           //     // $(this) jQuery 当前元素  this不要加引号
           //     // show() 显示元素  hide() 隐藏元素
           //     $(this).children("ul").slideDown(200);
           // });
           // // 鼠标离开
           // $(".nav>li").mouseout(function() {
           //     $(this).children("ul").slideUp(200);
           // });
           // 1. 事件切换 hover 就是鼠标经过和离开的复合写法
           // $(".nav>li").hover(function() {
           //     $(this).children("ul").slideDown(200);
           // }, function() {
           //     $(this).children("ul").slideUp(200);
           // });
           // 2. 事件切换 hover  如果只写一个函数，那么鼠标经过和鼠标离开都会触发这个函数
           $(".nav>li").hover(function() {
               $(this).children("ul").slideToggle();
           });
       })
   </script>
</body>

</html>
```
#### 1.7.10.2 动画队列及其停止排队方法
```markdown
1 动画或效果队列
动画或者效果一旦触发就会执行，如果多次触发，就造成多
个动画或者效果排队执行。

2 停止排队
stop()
(1）stop() 方法用于停止动画或效果。
(2) 注意： stop() 写到动画或者效果的前面， 相当于停
止结束上一次的动画。
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
       
       li {
           list-style-type: none;
       }
       
       a {
           text-decoration: none;
           font-size: 14px;
       }
       
       .nav {
           margin: 100px;
       }
       
       .nav>li {
           position: relative;
           float: left;
           width: 80px;
           height: 41px;
           text-align: center;
       }
       
       .nav li a {
           display: block;
           width: 100%;
           height: 100%;
           line-height: 41px;
           color: #333;
       }
       
       .nav>li>a:hover {
           background-color: #eee;
       }
       
       .nav ul {
           display: none;
           position: absolute;
           top: 41px;
           left: 0;
           width: 100%;
           border-left: 1px solid #FECC5B;
           border-right: 1px solid #FECC5B;
       }
       
       .nav ul li {
           border-bottom: 1px solid #FECC5B;
       }
       
       .nav ul li a:hover {
           background-color: #FFF5DA;
       }
   </style>
   <script src="jquery.min.js"></script>
</head>

<body>
   <ul class="nav">
       <li>
           <a href="#">微博</a>
           <ul>
               <li>
                   <a href="">私信</a>
               </li>
               <li>
                   <a href="">评论</a>
               </li>
               <li>
                   <a href="">@我</a>
               </li>
           </ul>
       </li>
       <li>
           <a href="#">微博</a>
           <ul>
               <li>
                   <a href="">私信</a>
               </li>
               <li>
                   <a href="">评论</a>
               </li>
               <li>
                   <a href="">@我</a>
               </li>
           </ul>
       </li>
       <li>
           <a href="#">微博</a>
           <ul>
               <li>
                   <a href="">私信</a>
               </li>
               <li>
                   <a href="">评论</a>
               </li>
               <li>
                   <a href="">@我</a>
               </li>
           </ul>
       </li>
       <li>
           <a href="#">微博</a>
           <ul>
               <li>
                   <a href="">私信</a>
               </li>
               <li>
                   <a href="">评论</a>
               </li>
               <li>
                   <a href="">@我</a>
               </li>
           </ul>
       </li>
   </ul>
   <script>
       $(function() {
           // 鼠标经过
           // $(".nav>li").mouseover(function() {
           //     // $(this) jQuery 当前元素  this不要加引号
           //     // show() 显示元素  hide() 隐藏元素
           //     $(this).children("ul").slideDown(200);
           // });
           // // 鼠标离开
           // $(".nav>li").mouseout(function() {
           //     $(this).children("ul").slideUp(200);
           // });
           // 1. 事件切换 hover 就是鼠标经过和离开的复合写法
           // $(".nav>li").hover(function() {
           //     $(this).children("ul").slideDown(200);
           // }, function() {
           //     $(this).children("ul").slideUp(200);
           // });
           // 2. 事件切换 hover  如果只写一个函数，那么鼠标经过和鼠标离开都会触发这个函数
           $(".nav>li").hover(function() {
               // stop 方法必须写到动画的前面
               $(this).children("ul").stop().slideToggle();
           });
       })
   </script>
</body>

</html>
```
### 1.7.11 淡入效果
```markdown
fadeIn([speed,[easing],[fn]])
（1）参数都可以省略。
（2）speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
（3）easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
（4）fn: 回调函数，在动画完成时执行的函数，每个元素执行一次。
```

```markdown
淡出效果
fadeOut([speed,[easing],[fn]])
（1）参数都可以省略。
（2）speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或
表示动画时长的毫秒数值(如：1000)。
（3）easing：(Optional) 用来指定切换效果，默认是“swing”，可
用参数“linear”。
（4）fn: 回调函数，在动画完成时执行的函数，每个元素执行一次。

淡入淡出切换效果
fadeToggle([speed,[easing],[fn]])
（1）参数都可以省略。
（2）speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
（3）easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
（4）fn: 回调函数，在动画完成时执行的函数，每个元素执行一次。

渐进方式调整到指定的不透明度
fadeTo([[speed],opacity,[easing],[fn]])
（1）opacity 透明度必须写，取值 0~1 之间。
（2）speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。必须写
（3）easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
（4）fn: 回调函数，在动画完成时执行的函数，每个元素执行一次。
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
           width: 150px;
           height: 300px;
           background-color: pink;
           display: none;
       }
   </style>
   <script src="jquery.min.js"></script>
</head>

<body>
   <button>淡入效果</button>
   <button>淡出效果</button>
   <button>淡入淡出切换</button>
   <button>修改透明度</button>
   <div></div>
   <script>
       $(function() {
           $("button").eq(0).click(function() {
               // 淡入 fadeIn()
               $("div").fadeIn(1000);
           })
           $("button").eq(1).click(function() {
               // 淡出 fadeOut()
               $("div").fadeOut(1000);

           })
           $("button").eq(2).click(function() {
               // 淡入淡出切换 fadeToggle()
               $("div").fadeToggle(1000);

           });
           $("button").eq(3).click(function() {
               //  修改透明度 fadeTo() 这个速度和透明度要必须写
               $("div").fadeTo(1000, 0.5);

           });


       });
   </script>
</body>

</html>
```
### 1.7.12 自定义动画 animate
```markdown
animate(params,[speed],[easing],[fn])
（1）params: 想要更改的样式属性，以对象形式传递，必须写。 
属性名可以不用带引号， 如果是复合属性则需要采
取驼峰命名法 borderLeft。其余参数都可以省略。
（2）speed：三种预定速度之一的字符
串(“slow”,“normal”, or “fast”)或表
示动画时长的毫秒数值(如：1000)。
（3）easing：(Optional) 用来指定切换效果，
默认是“swing”，可用参数“linear”。
（4）fn: 回调函数，在动画完成时执行的函数，每个元素执行一次。
```
```html
<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <script src="jquery.min.js"></script>
   <style>
       div {
           position: absolute;
           width: 200px;
           height: 200px;
           background-color: pink;
       }
   </style>
</head>

<body>
   <button>动起来</button>
   <div></div>
   <script>
       $(function() {
           $("button").click(function() {
               $("div").animate({
                   left: 500,
                   top: 300,
                   opacity: .4,
                   width: 500
               }, 500);
           })
       })
   </script>
</body>

</html>
```
### 1.7.13  jQuery 属性操作
```markdown
设置或获取元素固有属性值 prop()
获取属性语法prop(''属性'')
设置属性语法prop(''属性'', ''属性值'')

设置或获取元素自定义属性值 attr()
1 获取属性语法
attr(''属性'') // 类似原生 getAttribute()
2 设置属性语法
attr(''属性'', ''属性值'') // 类似原生 setAttribute()

数据缓存 data()
data() 方法可以在指定的元素上存取数据，并不会修改 DOM
元素结构。一旦页面刷新，之前存放的数据都将被移除。
1 附加数据语法
data(''name'',''value'') // 向被选元素附加数据 
2 获取数据语法
date(''name'') // 向被选元素获取数据 
```
```html
<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <script src="jquery.min.js"></script>
</head>

<body>
   <a href="http://www.itcast.cn" title="都挺好">都挺好</a>
   <input type="checkbox" name="" id="" checked>
   <div index="1" data-index="2">我是div</div>
   <span>123</span>
   <script>
       $(function() {
           //1. element.prop("属性名") 获取元素固有的属性值
           console.log($("a").prop("href"));
           $("a").prop("title", "我们都挺好");
           $("input").change(function() {
               console.log($(this).prop("checked"));

           });
           // console.log($("div").prop("index"));
           // 2. 元素的自定义属性 我们通过 attr()
           console.log($("div").attr("index"));
           $("div").attr("index", 4);
           console.log($("div").attr("data-index"));
           // 3. 数据缓存 data() 这个里面的数据是存放在元素的内存里面
           $("span").data("uname", "andy");
           console.log($("span").data("uname"));
           // 这个方法获取data-index h5自定义属性 第一个 不用写data-  而且返回的是数字型
           console.log($("div").data("index"));





       })
   </script>
</body>

</html>
```
### 1.7.14 jQuery 内容文本值
```markdown
1 普通元素内容 html()（ 相当于原生inner HTML)
html() // 获取元素的内容
html(''内容'') // 设置元素的内容
2 普通元素文本内容 text() (相当与原生 innerText)
text() // 获取元素的文本内容
text(''文本内容'') // 设置元素的文本内容
3 表单的值 val()（ 相当于原生value)
val() // 获取表单的值
val(''内容'') // 设置表单的值
```
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script src="jquery.min.js"></script>
</head>

<body>
  <div>
      <span>我是内容</span>
  </div>
  <input type="text" value="请输入内容">
  <script>
      // 1. 获取设置元素内容 html()
      console.log($("div").html());
      // $("div").html("123");
      // 2. 获取设置元素文本内容 text()
      console.log($("div").text());
      $("div").text("123");

      // 3. 获取设置表单值 val()
      console.log($("input").val());
      $("input").val("123");
  </script>
</body>

</html>
```
### 1.7.15 返回指定祖先元素
```html
<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <script src="jquery.min.js"></script>
</head>

<body>
   <div class="one">
       <div class="two">
           <div class="three">
               <div class="four">我不容易</div>
           </div>
       </div>
   </div>
   <script>
       console.log($(".four").parent().parent().parent());
       console.log($(".four").parents());
       console.log($(".four").parents(".one"));
   </script>
</body>

</html>
```

## 1.8 jQuery 元素操作
### 1.8.1 遍历元素
```markdown
jQuery 隐式迭代是对同一类元素做了同样的操作。 如
果想要给同一类元素做不同操作，就需要用到遍历。
$("div").each(function (index, domEle) { xxx; }） 
1 each() 方法遍历匹配的每一个元素。主要用DOM处理。 
each 每一个
2 里面的回调函数有2个参数： index 是每个元素的索引号; 
demEle 是每个DOM元素对象，不是jquery对象
3 所以要想使用jquery方法，需要给这个dom元素转换
为jquery对象 $(domEle)或者
$.each(object，function (index, element) { xxx; }） 
$.each()方法可用于遍历任何对象。主要用于数据处理，比如数组，
对象里面的函数有2个参数： index 是每个元素的索引号; 
element 遍历内容
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

   </style>
   <script src="jquery.min.js"></script>
</head>

<body>
   <div>1</div>
   <div>2</div>
   <div>3</div>
   <script>
       $(function() {
           // $("div").css("color", "red");
           // 如果针对于同一类元素做不同操作，需要用到遍历元素（类似for，但是比for强大）
           var sum = 0;
           // 1. each() 方法遍历元素 
           var arr = ["red", "green", "blue"];
           $("div").each(function(i, domEle) {
               // 回调函数第一个参数一定是索引号  可以自己指定索引号号名称
               // console.log(index);
               // console.log(i);
               // 回调函数第二个参数一定是 dom元素对象 也是自己命名
               // console.log(domEle);
               // domEle.css("color"); dom对象没有css方法
               $(domEle).css("color", arr[i]);
               sum += parseInt($(domEle).text());
           })
           console.log(sum);
           // 2. $.each() 方法遍历元素 主要用于遍历数据，处理数据
           // $.each($("div"), function(i, ele) {
           //     console.log(i);
           //     console.log(ele);

           // });
           // $.each(arr, function(i, ele) {
           //     console.log(i);
           //     console.log(ele);


           // })
           $.each({
               name: "andy",
               age: 18
           }, function(i, ele) {
               console.log(i); // 输出的是 name age 属性名
               console.log(ele); // 输出的是 andy  18 属性值


           })
       })
   </script>
</body>

</html>
```
### 1.8.2 创建添加删除元素
```markdown
创建元素
$(''<li></li>''); 
添加元素
内部添加
element.append(''内容'') 把内容放入匹配元素内部最后面，
类似原生 appendChild
element.prepend(''内容'') 把内容放入匹配元素内部最前面。
外部添加
element.after(''内容'') // 把内容放入目标元素后面
element.before(''内容'') // 把内容放入目标元素前面
1 内部添加元素，生成之后，它们是父子关系。
2 外部添加元素，生成之后，他们是兄弟关系。

删除元素
element.remove() // 删除匹配的元素（本身）
element.empty() // 删除匹配的元素集合中所有的子节点
element.html('''') // 清空匹配的元素内容
1 remove 删除元素本身。
2 empt() 和 html('''') 作用等价，都可以删除元素里
面的内容，只不过 html 还可以设置内容。
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <script src="jquery.min.js"></script>
</head>

<body>
   <ul>
       <li>原先的li</li>
   </ul>
   <div class="test">我是原先的div</div>
   <script>
       $(function() {
           // 1. 创建元素
           var li = $("<li>我是后来创建的li</li>");
           // 2. 添加元素

           // (1) 内部添加
           // $("ul").append(li);  内部添加并且放到内容的最后面 
           $("ul").prepend(li); // 内部添加并且放到内容的最前面

           // (2) 外部添加
           var div = $("<div>我是后妈生的</div>");
           // $(".test").after(div);
           $(".test").before(div);
           // 3. 删除元素
           // $("ul").remove(); 可以删除匹配的元素 自杀
           // $("ul").empty(); // 可以删除匹配的元素里面的子节点 孩子
           $("ul").html(""); // 可以删除匹配的元素里面的子节点 孩子

       })
   </script>
</body>

</html>
```

### 1.8.3 购物车
```html
购物车案例模块-全选分析
1 全选思路：里面3个小的复选框按钮（j-checkbox）
选中状态（checked）跟着全选按钮（checkall）走。
2 因为checked 是复选框的固有属性，此时我们需要利
用prop()方法获取和设置该属性。
3 把全选按钮状态赋值给3小复选框就可以了。
4 当我们每次点击小的复选框按钮，就来判断：
5 如果小复选框被选中的个数等于3 就应该把全选按钮
选上，否则全选按钮不选。
6 :checked 选择器 :checked 查找被选中的表单元素。

购物车案例模块-增减商品数量
1 核心思路：首先声明一个变量，当我们点
击+号（increment），就让这个值++，然后
赋值给文本框。
2 注意1： 只能增加本商品的数量， 就是当
前+号的兄弟文本框（itxt）的值。
3 修改表单的值是val() 方法
4 注意2： 这个变量初始值应该是这个文本
框的值，在这个值的基础上++。要获取表单的值
5 减号（decrement）思路同理，但是如果文本
框的值是1，就不能再减了。

购物车案例模块-修改商品小计
1 核心思路：每次点击+号或者-号，根据文本框
的值 乘以 当前商品的价格 就是 商品的小计
2 注意1： 只能增加本商品的小计， 就是当前
商品的小计模块（p-sum）
3 修改普通元素的内容是text() 方法
4 注意2： 当前商品的价格，要把￥符号去掉
再相乘 截取字符串 substr(1)
5 parents(‘选择器’) 可以返回指定祖先元素
6 最后计算的结果如果想要保留2位小数 通
过 toFixed(2) 方法
7 用户也可以直接修改表单里面的值，同样要
计算小计。 用表单change事件
8 用最新的表单内的值 乘以 单价即可 但
是还是当前商品小计

购物车案例模块-计算总计和总额
1 核心思路：把所有文本框里面的值相加就
是总计数量。总额同理
2 文本框里面的值不相同，如果想要相加需要
用到each遍历。声明一个变量，相加即可
3 点击+号-号，会改变总计和总额，如果用
户修改了文本框里面的值同样会改变总计和总额
4 因此可以封装一个函数求总计和总额的， 以
上2个操作调用这个函数即可。
5 注意1： 总计是文本框里面的值相加用 val() 总
额是普通元素的内容用text()
6 要注意普通元素里面的内容要去掉￥并且转
换为数字型才能相加

购物车案例模块-删除商品模块
1 核心思路：把商品remove() 删除元素即可
2 有三个地方需要删除： 1. 商品后面的删除
按钮 2. 删除选中的商品 3. 清理购物车
3 商品后面的删除按钮： 一定是删除当前的
商品，所以从 $(this) 出发
4 删除选中的商品： 先判断小的复选框按钮
是否选中状态，如果是选中，则删除对应的商品
5 清理购物车： 则是把所有的商品全部删掉

购物车案例模块-选中商品添加背景
1 核心思路：选中的商品添加背景，不选中移除背景即可
2 全选按钮点击：如果全选是选中的，则所有
的商品添加背景，否则移除背景
3 小的复选框点击： 如果是选中状态，则当
前商品添加背景，否则移除背景
4 这个背景，可以通过类名修改，添加类和删除类
```
```html
index.html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
   <meta charset="UTF-8">
   <title>我的购物车-品优购</title>
   <meta name="description" content="品优购JD.COM-专业的综合网上购物商城,销售家电、数码通讯、电脑、家居百货、服装服饰、母婴、图书、食品等数万个品牌优质商品.便捷、诚信的服务，为您提供愉悦的网上购物体验!" />
   <meta name="Keywords" content="网上购物,网上商城,手机,笔记本,电脑,MP3,CD,VCD,DV,相机,数码,配件,手表,存储卡,品优购" />
   <!-- 引入facicon.ico网页图标 -->
   <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
   <!-- 引入css 初始化的css 文件 -->
   <link rel="stylesheet" href="css/base.css">
   <!-- 引入公共样式的css 文件 -->
   <link rel="stylesheet" href="css/common.css">
   <!-- 引入car css -->
   <link rel="stylesheet" href="css/car.css">
   <!-- 先引入jquery  -->
   <script src="js/jquery.min.js"></script>
   <!-- 在引入我们自己的js文件 -->
   <script src="js/car.js"></script>
</head>

<body>
   <!-- 顶部快捷导航start -->
   <div class="shortcut">
       <div class="w">
           <div class="fl">
               <ul>
                   <li>品优购欢迎您！ </li>
                   <li>
                       <a href="#">请登录</a>
                       <a href="#" class="style-red">免费注册</a>
                   </li>
               </ul>
           </div>
           <div class="fr">
               <ul>
                   <li><a href="#">我的订单</a></li>
                   <li class="spacer"></li>
                   <li>
                       <a href="#">我的品优购</a>
                       <i class="icomoon"></i>
                   </li>
                   <li class="spacer"></li>
                   <li><a href="#">品优购会员</a></li>
                   <li class="spacer"></li>
                   <li><a href="#">企业采购</a></li>
                   <li class="spacer"></li>
                   <li><a href="#">关注品优购</a> <i class="icomoon"></i></li>
                   <li class="spacer"></li>
                   <li><a href="#">客户服务</a> <i class="icomoon"></i></li>
                   <li class="spacer"></li>
                   <li><a href="#">网站导航</a> <i class="icomoon"></i></li>
               </ul>
           </div>
       </div>
   </div>
   <!-- 顶部快捷导航end  -->


   <div class="car-header">
       <div class="w">
           <div class="car-logo">
               <img src="img/logo.png" alt=""> <b>购物车</b>
           </div>
       </div>
   </div>

   </div>
   <div class="c-container">
       <div class="w">
           <div class="cart-filter-bar">
               <em>全部商品</em>
           </div>
           <!-- 购物车主要核心区域 -->
           <div class="cart-warp">
               <!-- 头部全选模块 -->
               <div class="cart-thead">
                   <div class="t-checkbox">
                       <input type="checkbox" name="" id="" class="checkall"> 全选
                   </div>
                   <div class="t-goods">商品</div>
                   <div class="t-price">单价</div>
                   <div class="t-num">数量</div>
                   <div class="t-sum">小计</div>
                   <div class="t-action">操作</div>
               </div>
               <!-- 商品详细模块 -->
               <div class="cart-item-list">
                   <div class="cart-item check-cart-item">
                       <div class="p-checkbox">
                           <input type="checkbox" name="" id="" checked class="j-checkbox">
                       </div>
                       <div class="p-goods">
                           <div class="p-img">
                               <img src="upload/p1.jpg" alt="">
                           </div>
                           <div class="p-msg">【5本26.8元】经典儿童文学彩图青少版八十天环游地球中学生语文教学大纲</div>
                       </div>
                       <div class="p-price">￥12.60</div>
                       <div class="p-num">
                           <div class="quantity-form">
                               <a href="javascript:;" class="decrement">-</a>
                               <input type="text" class="itxt" value="1">
                               <a href="javascript:;" class="increment">+</a>
                           </div>
                       </div>
                       <div class="p-sum">￥12.60</div>
                       <div class="p-action"><a href="javascript:;">删除</a></div>
                   </div>
                   <div class="cart-item">
                       <div class="p-checkbox">
                           <input type="checkbox" name="" id="" class="j-checkbox">
                       </div>
                       <div class="p-goods">
                           <div class="p-img">
                               <img src="upload/p2.jpg" alt="">
                           </div>
                           <div class="p-msg">【2000张贴纸】贴纸书 3-6岁 贴画儿童 贴画书全套12册 贴画 贴纸儿童 汽</div>
                       </div>
                       <div class="p-price">￥24.80</div>
                       <div class="p-num">
                           <div class="quantity-form">
                               <a href="javascript:;" class="decrement">-</a>
                               <input type="text" class="itxt" value="1">
                               <a href="javascript:;" class="increment">+</a>
                           </div>
                       </div>
                       <div class="p-sum">￥24.80</div>
                       <div class="p-action"><a href="javascript:;">删除</a></div>
                   </div>
                   <div class="cart-item">
                       <div class="p-checkbox">
                           <input type="checkbox" name="" id="" class="j-checkbox">
                       </div>
                       <div class="p-goods">
                           <div class="p-img">
                               <img src="upload/p3.jpg" alt="">
                           </div>
                           <div class="p-msg">唐诗三百首+成语故事全2册 一年级课外书 精装注音儿童版 小学生二三年级课外阅读书籍</div>
                       </div>
                       <div class="p-price">￥29.80</div>
                       <div class="p-num">
                           <div class="quantity-form">
                               <a href="javascript:;" class="decrement">-</a>
                               <input type="text" class="itxt" value="1">
                               <a href="javascript:;" class="increment">+</a>
                           </div>
                       </div>
                       <div class="p-sum">￥29.80</div>
                       <div class="p-action"><a href="javascript:;">删除</a></div>
                   </div>
               </div>

               <!-- 结算模块 -->
               <div class="cart-floatbar">
                   <div class="select-all">
                       <input type="checkbox" name="" id="" class="checkall">全选
                   </div>
                   <div class="operation">
                       <a href="javascript:;" class="remove-batch"> 删除选中的商品</a>
                       <a href="javascript:;" class="clear-all">清理购物车</a>
                   </div>
                   <div class="toolbar-right">
                       <div class="amount-sum">已经选<em>1</em>件商品</div>
                       <div class="price-sum">总价： <em>￥12.60</em></div>
                       <div class="btn-area">去结算</div>
                   </div>
               </div>
           </div>
       </div>

   </div>

   <!-- footer start -->
   <div class="footer">
       <div class="w">
           <!-- mod_service -->
           <div class="mod_service">
               <ul>
                   <li>
                       <i class="mod-service-icon mod_service_zheng"></i>
                       <div class="mod_service_tit">
                           <h5>正品保障</h5>
                           <p>正品保障，提供发票</p>
                       </div>
                   </li>
                   <li>
                       <i class="mod-service-icon mod_service_kuai"></i>
                       <div class="mod_service_tit">
                           <h5>正品保障</h5>
                           <p>正品保障，提供发票</p>
                       </div>
                   </li>
                   <li>
                       <i class="mod-service-icon mod_service_bao"></i>
                       <div class="mod_service_tit">
                           <h5>正品保障</h5>
                           <p>正品保障，提供发票</p>
                       </div>
                   </li>
                   <li>
                       <i class="mod-service-icon mod_service_bao"></i>
                       <div class="mod_service_tit">
                           <h5>正品保障</h5>
                           <p>正品保障，提供发票</p>
                       </div>
                   </li>
                   <li>
                       <i class="mod-service-icon mod_service_bao"></i>
                       <div class="mod_service_tit">
                           <h5>正品保障</h5>
                           <p>正品保障，提供发票</p>
                       </div>
                   </li>
               </ul>
           </div>
           <!-- mod_help -->
           <div class="mod_help">
               <dl class="mod_help_item">
                   <dt>购物指南</dt>
                   <dd> <a href="#">购物流程 </a></dd>
                   <dd> <a href="#">会员介绍 </a></dd>
                   <dd> <a href="#">生活旅行/团购 </a></dd>
                   <dd> <a href="#">常见问题 </a></dd>
                   <dd> <a href="#">大家电 </a></dd>
                   <dd> <a href="#">联系客服 </a></dd>
               </dl>
               <dl class="mod_help_item">
                   <dt>购物指南</dt>
                   <dd> <a href="#">购物流程 </a></dd>
                   <dd> <a href="#">会员介绍 </a></dd>
                   <dd> <a href="#">生活旅行/团购 </a></dd>
                   <dd> <a href="#">常见问题 </a></dd>
                   <dd> <a href="#">大家电 </a></dd>
                   <dd> <a href="#">联系客服 </a></dd>
               </dl>
               <dl class="mod_help_item">
                   <dt>购物指南</dt>
                   <dd> <a href="#">购物流程 </a></dd>
                   <dd> <a href="#">会员介绍 </a></dd>
                   <dd> <a href="#">生活旅行/团购 </a></dd>
                   <dd> <a href="#">常见问题 </a></dd>
                   <dd> <a href="#">大家电 </a></dd>
                   <dd> <a href="#">联系客服 </a></dd>
               </dl>
               <dl class="mod_help_item">
                   <dt>购物指南</dt>
                   <dd> <a href="#">购物流程 </a></dd>
                   <dd> <a href="#">会员介绍 </a></dd>
                   <dd> <a href="#">生活旅行/团购 </a></dd>
                   <dd> <a href="#">常见问题 </a></dd>
                   <dd> <a href="#">大家电 </a></dd>
                   <dd> <a href="#">联系客服 </a></dd>
               </dl>
               <dl class="mod_help_item">
                   <dt>购物指南</dt>
                   <dd> <a href="#">购物流程 </a></dd>
                   <dd> <a href="#">会员介绍 </a></dd>
                   <dd> <a href="#">生活旅行/团购 </a></dd>
                   <dd> <a href="#">常见问题 </a></dd>
                   <dd> <a href="#">大家电 </a></dd>
                   <dd> <a href="#">联系客服 </a></dd>
               </dl>
               <dl class="mod_help_item mod_help_app">
                   <dt>帮助中心</dt>
                   <dd>
                       <img src="upload/erweima.png" alt="">
                       <p>品优购客户端</p>
                   </dd>
               </dl>
           </div>

           <!-- mod_copyright  -->
           <div class="mod_copyright">
               <p class="mod_copyright_links">
                   关于我们 | 联系我们 | 联系客服 | 商家入驻 | 营销中心 | 手机品优购 | 友情链接 | 销售联盟 | 品优购社区 | 品优购公益 | English Site | Contact U
               </p>
               <p class="mod_copyright_info">
                   地址：北京市昌平区建材城西路金燕龙办公楼一层 邮编：100096 电话：400-618-4000 传真：010-82935100 邮箱: zhanghj+itcast.cn <br> 京ICP备08001421号京公网安备110108007702
               </p>
           </div>
       </div>
   </div>
   <!-- footer end -->
</body>

</html>

car.html
$(function() {
   // 1. 全选 全不选功能模块
   // 就是把全选按钮（checkall）的状态赋值给 三个小的按钮（j-checkbox）就可以了
   // 事件可以使用change
   $(".checkall").change(function() {
       // console.log($(this).prop("checked"));
       $(".j-checkbox, .checkall").prop("checked", $(this).prop("checked"));
       if ($(this).prop("checked")) {
           // 让所有的商品添加 check-cart-item 类名
           $(".cart-item").addClass("check-cart-item");
       } else {
           // check-cart-item 移除
           $(".cart-item").removeClass("check-cart-item");
       }
   });
   // 2. 如果小复选框被选中的个数等于3 就应该把全选按钮选上，否则全选按钮不选。
   $(".j-checkbox").change(function() {
       // if(被选中的小的复选框的个数 === 3) {
       //     就要选中全选按钮
       // } else {
       //     不要选中全选按钮
       // }
       // console.log($(".j-checkbox:checked").length);
       // $(".j-checkbox").length 这个是所有的小复选框的个数
       if ($(".j-checkbox:checked").length === $(".j-checkbox").length) {
           $(".checkall").prop("checked", true);
       } else {
           $(".checkall").prop("checked", false);
       }
       if ($(this).prop("checked")) {
           // 让当前的商品添加 check-cart-item 类名
           $(this).parents(".cart-item").addClass("check-cart-item");
       } else {
           // check-cart-item 移除
           $(this).parents(".cart-item").removeClass("check-cart-item");
       }
   });
   // 3. 增减商品数量模块 首先声明一个变量，当我们点击+号（increment），就让这个值++，然后赋值给文本框。
   $(".increment").click(function() {
       // 得到当前兄弟文本框的值
       var n = $(this).siblings(".itxt").val();
       // console.log(n);
       n++;
       $(this).siblings(".itxt").val(n);
       // 3. 计算小计模块 根据文本框的值 乘以 当前商品的价格  就是 商品的小计
       // 当前商品的价格 p  
       var p = $(this).parents(".p-num").siblings(".p-price").html();
       // console.log(p);
       p = p.substr(1);
       console.log(p);
       var price = (p * n).toFixed(2);
       // 小计模块 
       // toFixed(2) 可以让我们保留2位小数
       $(this).parents(".p-num").siblings(".p-sum").html("￥" + price);
       getSum();
   });
   $(".decrement").click(function() {
       // 得到当前兄弟文本框的值
       var n = $(this).siblings(".itxt").val();
       if (n == 1) {
           return false;
       }
       // console.log(n);
       n--;
       $(this).siblings(".itxt").val(n);
       // var p = $(this).parent().parent().siblings(".p-price").html();
       // parents(".p-num") 返回指定的祖先元素
       var p = $(this).parents(".p-num").siblings(".p-price").html();
       // console.log(p);
       p = p.substr(1);
       console.log(p);
       // 小计模块 
       $(this).parents(".p-num").siblings(".p-sum").html("￥" + (p * n).toFixed(2));
       getSum();
   });
   //  4. 用户修改文本框的值 计算 小计模块  
   $(".itxt").change(function() {
       // 先得到文本框的里面的值 乘以 当前商品的单价 
       var n = $(this).val();
       // 当前商品的单价
       var p = $(this).parents(".p-num").siblings(".p-price").html();
       // console.log(p);
       p = p.substr(1);
       $(this).parents(".p-num").siblings(".p-sum").html("￥" + (p * n).toFixed(2));
       getSum();
   });
   // 5. 计算总计和总额模块
   getSum();

   function getSum() {
       var count = 0; // 计算总件数 
       var money = 0; // 计算总价钱
       $(".itxt").each(function(i, ele) {
           count += parseInt($(ele).val());
       });
       $(".amount-sum em").text(count);
       $(".p-sum").each(function(i, ele) {
           money += parseFloat($(ele).text().substr(1));
       });
       $(".price-sum em").text("￥" + money.toFixed(2));
   }
   // 6. 删除商品模块
   // (1) 商品后面的删除按钮
   $(".p-action a").click(function() {
       // 删除的是当前的商品 
       $(this).parents(".cart-item").remove();
       getSum();
   });
   // (2) 删除选中的商品
   $(".remove-batch").click(function() {
       // 删除的是小的复选框选中的商品
       $(".j-checkbox:checked").parents(".cart-item").remove();
       getSum();
   });
   // (3) 清空购物车 删除全部商品
   $(".clear-all").click(function() {
       $(".cart-item").remove();
       getSum();
   })
   
})
```

## 1.9 jQuery 尺寸
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200515232931324.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 1.9.1 jQuery元素大小
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
           padding: 10px;
           border: 15px solid red;
           margin: 20px;
       }
   </style>
   <script src="jquery.min.js"></script>
</head>

<body>
   <div></div>
   <script>
       $(function() {
           // 1. width() / height() 获取设置元素 width和height大小 
           console.log($("div").width());
           // $("div").width(300);

           // 2. innerWidth() / innerHeight()  获取设置元素 width和height + padding 大小 
           console.log($("div").innerWidth());

           // 3. outerWidth()  / outerHeight()  获取设置元素 width和height + padding + border 大小 
           console.log($("div").outerWidth());

           // 4. outerWidth(true) / outerHeight(true) 获取设置 width和height + padding + border + margin
           console.log($("div").outerWidth(true));


       })
   </script>
</body>

</html>
```
### 1.9.2 jQuery 位置
```markdown
1  offset() 设置或获取元素偏移

1  offset() 方法设置或返回被选元素相对于文档的偏
移坐标，跟父级没有关系。
2 该方法有2个属性 left、top 。offset().top 用于
获取距离文档顶部的距离，offset().left 用于获取距离文
档左侧的距离。
3 可以设置元素的偏移：offset({ top: 10, left: 30 });

2 position() 获取元素偏移
1  position() 方法用于返回被选元素相对于带有定位的
父级偏移坐标，如果父级都没有定位，则以文档为准。
2 该方法有2个属性 left、top、position().top 用于获取距离
定位父级顶部的距离，position().left 用于获取距
离定位父级左侧的距离。

3 scrollTop()/scrollLeft() 设置或获取元素被卷去的头
部和左侧
1 scrollTop() 方法设置或返回被选元素被卷去的头部。
2 不跟参数是获取，参数为不带单位的数字则是设置被卷去的头部。
```
### 1.9.3 jQuery元素位置
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
       
       .father {
           width: 400px;
           height: 400px;
           background-color: pink;
           margin: 100px;
           overflow: hidden;
           position: relative;
       }
       
       .son {
           width: 150px;
           height: 150px;
           background-color: purple;
           position: absolute;
           left: 10px;
           top: 10px;
       }
   </style>
   <script src="jquery.min.js"></script>
</head>

<body>
   <div class="father">
       <div class="son"></div>
   </div>
   <script>
       $(function() {
           // 1. 获取设置距离文档的位置（偏移） offset
           console.log($(".son").offset());
           console.log($(".son").offset().top);
           // $(".son").offset({
           //     top: 200,
           //     left: 200
           // });
           // 2. 获取距离带有定位父级位置（偏移） position   如果没有带有定位的父级，则以文档为准
           // 这个方法只能获取不能设置偏移
           console.log($(".son").position());
           // $(".son").position({
           //     top: 200,
           //     left: 200
           // });
       })
   </script>
</body>

</html>
```
### 1.9.4 jQuery被卷去的头部
```markdown
1 核心原理： 使用animate动画返回顶部。
2 animate动画函数里面有个scrollTop 属性，可以设置位置
3 但是是元素做动画，因此 $(“body,html”).animate({scrollTop: 0})
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
           height: 2000px;
       }
       
       .back {
           position: fixed;
           width: 50px;
           height: 50px;
           background-color: pink;
           right: 30px;
           bottom: 100px;
           display: none;
       }
       
       .container {
           width: 900px;
           height: 500px;
           background-color: skyblue;
           margin: 400px auto;
       }
   </style>
   <script src="jquery.min.js"></script>
</head>

<body>
   <div class="back">返回顶部</div>
   <div class="container">
   </div>
   <script>
       $(function() {
           $(document).scrollTop(100);
           // 被卷去的头部 scrollTop()  / 被卷去的左侧 scrollLeft()
           // 页面滚动事件
           var boxTop = $(".container").offset().top;
           $(window).scroll(function() {
               // console.log(11);
               console.log($(document).scrollTop());
               if ($(document).scrollTop() >= boxTop) {
                   $(".back").fadeIn();
               } else {
                   $(".back").fadeOut();
               }
           });
           // 返回顶部
           $(".back").click(function() {
               // $(document).scrollTop(0);
               $("body, html").stop().animate({
                   scrollTop: 0
               });
               // $(document).stop().animate({
               //     scrollTop: 0
               // }); 不能是文档而是 html和body元素做动画
           })
       })
   </script>
</body>

</html>
```


## 1.10 jQuery 事件注册
```markdown
单个事件注册
语法：
element.事件(function(){}) 
$(“div”).click(function(){ 事件处理程序 }) 

其他事件和原生基本一致。
比如mouseover、mouseout、blur、focus、change、
keydown、keyup、resize、scroll 等
```
###  1.10.1 事件处理on
```markdown
on() 方法在匹配元素上绑定一个或多个事件的事件处理函数
语法：
element.on(events,[selector],fn) 

1 events:一个或多个用空格分隔的事件类型，
如"click"或"keydown" 。
2 selector: 元素的子元素选择器 。
3 fn:回调函数 即绑定在元素身上的侦听函数。

on() 方法优势1：
可以绑定多个事件，多个处理事件处理程序。
$(“div”).on({
mouseover: function(){},
mouseout: function(){},
click: function(){}
}); 

如果事件处理程序相同
$(“div”).on(“mouseover mouseout”, function() {
$(this).toggleClass(“current”);
}); 

可以事件委派操作 。事件委派的定义就是，把原来加给子元素
身上的事件绑定在父元素身上，就是把事件委派给父元素。
$('ul').on('click', 'li', function() {
alert('hello world!');
}); 

动态创建的元素，click() 没有办法绑定事件， on() 可以
给动态生成的元素绑定事件
$(“div").on("click",”p”, function(){
alert("俺可以给动态生成的元素绑定事件")
});

$("div").append($("<p>我是动态创建的p</p>"));
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
           width: 100px;
           height: 100px;
           background-color: pink;
       }
       
       .current {
           background-color: purple;
       }
   </style>
   <script src="jquery.min.js"></script>
</head>

<body>
   <div></div>
   <ul>
       <li>我们都是好孩子</li>
       <li>我们都是好孩子</li>
       <li>我们都是好孩子</li>
       <li>我们都是好孩子</li>
       <li>我们都是好孩子</li>
   </ul>
   <ol>

   </ol>
   <script>
       $(function() {
           // 1. 单个事件注册
           // $("div").click(function() {
           //     $(this).css("background", "purple");
           // });
           // $("div").mouseenter(function() {
           //     $(this).css("background", "skyblue");
           // });

           // 2. 事件处理on
           // (1) on可以绑定1个或者多个事件处理程序
           // $("div").on({
           //     mouseenter: function() {
           //         $(this).css("background", "skyblue");
           //     },
           //     click: function() {
           //         $(this).css("background", "purple");
           //     },
           //     mouseleave: function() {
           //         $(this).css("background", "blue");
           //     }
           // });
           $("div").on("mouseenter mouseleave", function() {
               $(this).toggleClass("current");
           });
           // (2) on可以实现事件委托（委派）
           // $("ul li").click();
           $("ul").on("click", "li", function() {
               alert(11);
           });
           // click 是绑定在ul 身上的，但是 触发的对象是 ul 里面的小li
           // (3) on可以给未来动态创建的元素绑定事件
           // $("ol li").click(function() {
           //     alert(11);
           // })
           $("ol").on("click", "li", function() {
               alert(11);
           })
           var li = $("<li>我是后来创建的</li>");
           $("ol").append(li);
       })
   </script>
</body>

</html>
```
###  1.10.2 微博发布效果
```markdown
1 点击发布按钮， 动态创建一个小li，放入文本
框的内容和删除按钮， 并且添加到ul 中。
2 点击的删除按钮，可以删除当前的微博留言。
```
```html
<!DOCTYPE html>
<html>

<head lang="en">
   <meta charset="UTF-8">
   <title></title>
   <style>
       * {
           margin: 0;
           padding: 0
       }
       
       ul {
           list-style: none
       }
       
       .box {
           width: 600px;
           margin: 100px auto;
           border: 1px solid #000;
           padding: 20px;
       }
       
       textarea {
           width: 450px;
           height: 160px;
           outline: none;
           resize: none;
       }
       
       ul {
           width: 450px;
           padding-left: 80px;
       }
       
       ul li {
           line-height: 25px;
           border-bottom: 1px dashed #cccccc;
           display: none;
       }
       
       input {
           float: right;
       }
       
       ul li a {
           float: right;
       }
   </style>
   <script src="jquery.min.js"></script>
   <script>
       $(function() {
           // 1.点击发布按钮， 动态创建一个小li，放入文本框的内容和删除按钮， 并且添加到ul 中
           $(".btn").on("click", function() {
               var li = $("<li></li>");
               li.html($(".txt").val() + "<a href='javascript:;'> 删除</a>");
               $("ul").prepend(li);
               li.slideDown();
               $(".txt").val("");
           })

           // 2.点击的删除按钮，可以删除当前的微博留言li
           // $("ul a").click(function() {  // 此时的click不能给动态创建的a添加事件
           //     alert(11);
           // })
           // on可以给动态创建的元素绑定事件
           $("ul").on("click", "a", function() {
               $(this).parent().slideUp(function() {
                   $(this).remove();
               });
           })

       })
   </script>
</head>

<body>
   <div class="box" id="weibo">
       <span>微博发布</span>
       <textarea name="" class="txt" cols="30" rows="10"></textarea>
       <button class="btn">发布</button>
       <ul>
       </ul>
   </div>
</body>

</html>
```
###  1.10.3 事件解绑off
```markdown
off() 方法可以移除通过 on() 方法添加的事件处理程序。
$("p").off() // 解绑p元素所有事件处理程序
$("p").off( "click") // 解绑p元素上面的点击事件 后面
的 foo 是侦听函数名
$("ul").off("click", "li"); // 解绑事件委托
如果有的事件只想触发一次， 可以使用 one() 来绑定事件。
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
           width: 100px;
           height: 100px;
           background-color: pink;
       }
   </style>
   <script src="jquery.min.js"></script>
   <script>
       $(function() {
           $("div").on({
               click: function() {
                   console.log("我点击了");
               },
               mouseover: function() {
                   console.log('我鼠标经过了');
               }
           });
           $("ul").on("click", "li", function() {
               alert(11);
           });
           // 1. 事件解绑 off 
           // $("div").off();  // 这个是解除了div身上的所有事件
           $("div").off("click"); // 这个是解除了div身上的点击事件
           $("ul").off("click", "li");
           // 2. one() 但是它只能触发事件一次
           $("p").one("click", function() {
               alert(11);
           })
       })
   </script>
</head>

<body>
   <div></div>
   <ul>
       <li>我们都是好孩子</li>
       <li>我们都是好孩子</li>
       <li>我们都是好孩子</li>
   </ul>
   <p>我是屁</p>
</body>

</html>
```
###  1.10.4 自动触发事件 trigger() 
```html
有些事件希望自动触发, 比如轮播图自动播放功能跟点
击右侧按钮一致。可以利用定时器自动触发右侧按
钮点击事件，不必鼠标点击触发。

element.click() // 第一种简写形式
element.trigger("type") // 第二种自动触发模式

$("p").on("click", function () {
alert("hi~");
});
$("p").trigger("click"); // 此时自动触发点击事件，不需要鼠标点击

element.triggerHandler(type) // 第三种自动触发模式
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
           width: 100px;
           height: 100px;
           background-color: pink;
       }
   </style>
   <script src="jquery.min.js"></script>
   <script>
       $(function() {
           $("div").on("click", function() {
               alert(11);
           });

           // 自动触发事件
           // 1. 元素.事件()
           // $("div").click();会触发元素的默认行为
           // 2. 元素.trigger("事件")
           // $("div").trigger("click");会触发元素的默认行为
           $("input").trigger("focus");
           // 3. 元素.triggerHandler("事件") 就是不会触发元素的默认行为
           $("div").triggerHandler("click");
           $("input").on("focus", function() {
               $(this).val("你好吗");
           });
           // $("input").triggerHandler("focus");

       });
   </script>
</head>

<body>
   <div></div>
   <input type="text">
</body>

</html>
```
### 1.10.5 jQuery 事件对象
```markdown
事件被触发，就会有事件对象的产生。
element.on(events,[selector],function(event) {}) 
阻止默认行为：event.preventDefault() 或者 return false
阻止冒泡： event.stopPropagation() 
```
### 1.10.6 jQuery 拷贝对象
```markdown
$.extend([deep], target, object1, [objectN])
1 deep: 如果设为true 为深拷贝， 默认为false 浅拷贝
2 target: 要拷贝的目标对象
3 object1:待拷贝到第一个对象的对象。
4 objectN:待拷贝到第N个对象的对象。
5 浅拷贝目标对象引用的被拷贝的对象地址，修改目
标对象会影响被拷贝对象。
6 深拷贝，前面加true， 完全克隆，修改目标对象不会
影响被拷贝对象。
```
```html
<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <script src="jquery.min.js"></script>
   <script>
       $(function() {
           // var targetObj = {};
           // var obj = {
           //     id: 1,
           //     name: "andy"
           // };
           // // $.extend(target, obj);
           // $.extend(targetObj, obj);
           // console.log(targetObj);
           // var targetObj = {
           //     id: 0
           // };
           // var obj = {
           //     id: 1,
           //     name: "andy"
           // };
           // // $.extend(target, obj);
           // $.extend(targetObj, obj);
           // console.log(targetObj); // 会覆盖targetObj 里面原来的数据
           var targetObj = {
               id: 0,
               msg: {
                   sex: '男'
               }
           };
           var obj = {
               id: 1,
               name: "andy",
               msg: {
                   age: 18
               }
           };
           // // $.extend(target, obj);
           // $.extend(targetObj, obj);
           // console.log(targetObj); // 会覆盖targetObj 里面原来的数据
           // // 1. 浅拷贝把原来对象里面的复杂数据类型地址拷贝给目标对象
           // targetObj.msg.age = 20;
           // console.log(targetObj);
           // console.log(obj);
           // 2. 深拷贝把里面的数据完全复制一份给目标对象 如果里面有不冲突的属性,会合并到一起 
           $.extend(true, targetObj, obj);
           // console.log(targetObj); // 会覆盖targetObj 里面原来的数据
           targetObj.msg.age = 20;
           console.log(targetObj); // msg :{sex: "男", age: 20}
           console.log(obj);




       })
   </script>
</head>

<body>

</body>

</html>
```
### 1.10.7 jQuery 多库共存
```markdown
jQuery使用$作为标示符，随着jQuery的流行,其他 js 库
也会用这$作为标识符， 这样一起使用会引起冲突。
需要一个解决方案，让jQuery 和其他的js库不存在
冲突，可以同时存在，这就叫做多库共存。
1 把里面的 $ 符号 统一改为 jQuery。 比如 jQuery(''div'')
2 jQuery 变量规定新的名称：$.noConflict() var xx = $.noConflict();
```
```html
<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <script src="jquery.min.js"></script>
   <script>
       $(function() {
           function $(ele) {
               return document.querySelector(ele);
           }
           console.log($("div"));
           // 1. 如果$ 符号冲突 我们就使用 jQuery
           jQuery.each();
           // 2. 让jquery 释放对$ 控制权 让用自己决定
           var suibian = jQuery.noConflict();
           console.log(suibian("span"));
           suibian.each();
       })
   </script>
</head>

<body>
   <div></div>
   <span></span>
</body>

</html>
```

# Bootstrap
## 1 BootStrap 的引言

```markdown
Bootstrap是美国Twitter公司的设计
师Mark Otto和Jacob Thornton合作基
于HTML、CSS、JavaScript 开发的简
洁、直观、强悍的前端开发框架，使得 Web 
开发更加快捷

Bootstrap一经推出后颇受欢迎，一直是GitHub上
的热门开源项目，包括NASA的MSNBC（微软全国
广播公司）的Breaking News都使用了该项目

Bootstrap 是最受欢迎的 HTML、CSS 和 JS 框架，
用于开发响应式布局、移动设备优先的 WEB 项目。
```


## 2 BootStrap 环境搭建

```html
1.下载bootStrap 相关资料  说明:bootstrap第3.3.7版本
  https://v3.bootcss.com/getting-started/#download
  
2.下载完成之后解压bootstrap压缩包
   css 	 bootstrap核心css文件   bootstrap.css 核心css bootstarp.min.css(压缩css)
   fonts 用来存放bootstrap字体图标的文件夹
   js    bootstrap.js 是bootstrap核心js   bootstrap.min.js(压缩js)
    
3.将bootstrap文件夹全部放入项目中

4.页面使用bootstrap
   <meta name="viewport" content="width=device-width, initial-scale=1"> 移动设备优先
   <link rel="stylesheet" href="../boot/css/bootstrap.min.css">
```


## 3 BootStrap 容器

```html
<div class="container" style="border: 1px red solid;"></div>
注意: 两边留有一定宽度 padding

<div class="container-fluid" style="border: 1px blue solid;"></div>
注意: 占用页面的100宽度

注意: 两种容器类不能互相嵌套
```


## 4 BootStrap 栅格系统

```html
栅格系统用于通过一系列的行（row）与列（column）的
组合来创建页面布局，你的内容就可以放入这些创建好
的布局中。下面就介绍一下 Bootstrap 栅格系统的工作原理：

“行（row）”必须包含在 .container （固定宽度）
或 .container-fluid （100% 宽度）中，以便
为其赋予合适的排列（aligment）和内补（padding）。
通过“行（row）”在水平方向创建一组“列（column）”。
你的内容应当放置于“列（column）”内，并且，只
有“列（column）”可以作为行（row）”的直接子元素。
类似 .row 和 .col-xs-4 这种预定义的类，可以
用来快速创建栅格布局。Bootstrap 源码中定义
的 mixin 也可以用来创建语义化的布局。
通过为“列（column）”设置 padding 属性，从而
创建列与列之间的间隔（gutter）。通过为 .row 元
素设置负值 margin 从而抵消掉为 .container 元
素设置的 padding，也就间接为“行（row）”所包
含的“列（column）”抵消掉了padding。
负值的 margin就是下面的示例为什么是向外突出
的原因。在栅格列中的内容排成一行。
栅格系统中的列是通过指定1到12的值来表示其
跨越的范围。例如，三个等宽的列可以使用三
个 .col-xs-4 来创建。
如果一“行（row）”中包含了的“列（column）”大
于 12，多余的“列（column）”所在的元素将被作为
一个整体另起一行排列。
栅格类适用于与屏幕宽度大于或等于分界点大小的设
备，并且针对小屏幕设备覆盖栅格类。 因此，在
元素上应用任何 .col-md-* 栅格类适用于与屏幕
宽度大于或等于分界点大小的设备 ， 并且针对
小屏幕设备覆盖栅格类。 因此，在元素上应用任
何 .col-lg-* 不存在， 也影响大屏幕设备。

1.栅格系统分配
   注意: 无论屏幕尺寸多大在bootsrap中将其分为12份

2.栅格系统基本使用

  <div class="container-fulid">
       <div class="row">
           12column (.col-xs .col-sm(推荐) .col-md .col-lg)
      </div>
  </div>

3.栅格系统的分配(一个行12等份)
  <div class="row">
            <!--行最多12个列-->
            <div class="col-sm-1 aa">1</div>
            <div class="col-sm-1 aa">1</div>
            <div class="col-sm-1 aa">1</div>
            <div class="col-sm-1 aa">1</div>
            <div class="col-sm-1 aa">1</div>
            <div class="col-sm-1 aa">1</div>
            <div class="col-sm-1 aa">1</div>
            <div class="col-sm-1 aa">1</div>
            <div class="col-sm-1 aa">1</div>
            <div class="col-sm-1 aa">1</div>
            <div class="col-sm-1 aa">1</div>
            <div class="col-sm-1 aa">1</div>
   </div>
4.栅格系统的拆分
	<div class="row">
            <!--行最多12个列-->
            <div class="col-sm-4 aa">1</div>
            <div class="col-sm-4 aa">1</div>
            <div class="col-sm-4 aa">1</div>
    </div>

5.栅格系统嵌套
	<div class="row">
        <!--行最多12个列-->
        <div class="col-sm-2 aa">1</div>
        <div class="col-sm-8 aa">
            <div class="col-sm-10 bb">
                aaa
            </div>
            <div class="col-sm-2 bb">
                bb
            </div>
        </div>
        <div class="col-sm-2 aa">1</div>
    </div>

6.栅格系统列偏移 (.col-sm-offset)
 	<div class="row">
        <div class="col-sm-4 aa col-sm-offset-8">aaa</div>	
	</div>

7.栅格系统多余列
 	<div class="row">
        <!--占4份-->
        <div class="col-sm-4 aa">aaa</div>
        <!--占4份-->
        <div class="col-sm-4 aa">bbb</div>
        <!--超过另起一行-->
        <div class="col-sm-8 aa">bbb</div>
        <!--没有超过12等份 在同一行-->
        <div class="col-sm-4 aa">ccc</div>
	</div>

```

## 5 BootStrap 排版

### 5.1 标题

```html
	<h1>h1. Bootstrap heading</h1>
    <h2>h2. Bootstrap heading</h2>
    <h3>h3. Bootstrap heading</h3>
    <h4>h4. Bootstrap heading</h4>
    <h5>h5. Bootstrap heading</h5>
    <h6>h6. Bootstrap heading</h6>

    注意: 在bootstrap中默认h1标题的大小为36px   
    最小标题的字体12px
```

### 5.2  页面主体

```html
Bootstrap 将全局 font-size 设置
为 14px，line-height 设置为 1.428。这些属性直接
赋予 <body> 元素和所有段落元素
    
1.使用可以直接在body标签中书写文字
    
2.可以在<p>标签中书写文字和直接在body标签中书写文字样式一致
<p>这是一个段落</p>
    
3.突出显示
<p class="lead">这是一个段落突出显示</p>
```

### 5.3 内联文本元素

```html
1.使用<mark>标签高亮元素
    这是一个段落<mark>突出</mark>显示
```

### 5.4 被删除的文本

```html
1.使用<del>标签让元素显示删除线
	小<del>黑小</del>明
    小<s>黑小</s>明
```

### 5.5 插入文本和带有下划线的文本

```html
1.使用<ins>标签
     <ins>这是bootstrap相关的内容</ins>
2.使用<u>标签
     <u>这是bootstrap相关的内容</u>
```

### 5.6 小号文本

```html
1.使用<small>标签完成字体小号展示 
     这是一个好<small>的课程</small>
     注意:使用 <small> 标签包裹，其内的文本将被设置为父容器字体大小的 85%
    
2.副标题
    <h1>h1. Bootstrap heading<small>V2.0</small></h1>
    <h2>h2. Bootstrap heading<small>副标题</small></h2>
    <h3>h3. Bootstrap heading<small>副标题</small></h3>
    <h4>h4. Bootstrap heading<small>副标题</small></h4>
    <h5>h5. Bootstrap heading<small>副标题</small></h5>
    <h6>h6. Bootstrap heading<small>副标题</small></h6>
```

### 5.7 着重和斜体

```html
1.使用<strong>标签着重展示
    小明是一个<strong>好人</strong>
    
2.使用<em>标签可以让包裹的文本变成斜体
    小明是一个<strong><em>好人</em></strong>
```

### 5.8 文本对齐方式

```html
1.设置文本对齐方式
	<!--左对齐-->
    <p class="text-left">小黑</p>
    <!--右对齐-->
    <p class="text-right">小黑</p>
    <!--居中对齐-->
    <p class="text-center">小黑</p>
    <!--不换行-->
    <p class="text-nowrap">小黑</p>
```

### 5.9 改变大小写

```html
1.设置转换大小写
	<p class="text-lowercase">LOwercased text.</p>
    <p class="text-uppercase">Uppercased text.</p>
    <p class="text-capitalize">Capitalized text boot.</p>   注意:这里是首字母大写
```

### 5.10 列表

```html
1.无序列表
	<ul>
        <li>小明</li>
        <li>小黑</li>
    </ul>

2.有序列表
	<ol>
        <li>小黑</li>
        <li>小张</li>
    </ol>

3.无样式列表
	<ul class="list-unstyled">
        <li>小明
            <ul>  
                <li>二级</li>
                <li>二级</li>
            </ul>
        </li>
        <li>小黑</li>
    </ul>
	注意: list-unstyled 只能解决ul中一级列表没有样式 但是内部嵌套列表依然存在样式

4.内联列表(列表项一行展示)
	<ul class="list-inline">
        <li>看电视</li>
        <li>看书</li>
        <li>看报</li>
    </ul>
5.水平列表
	<dl class="dl-horizontal">
        <dt>小明明</dt>
        <dd>是一个非常好的人</dd>
        <dd>是一个非常好的人1</dd>
        <dd>是一个非常好的人2</dd>
        <dt>小嘿嘿</dt>
        <dd>是一个非常好的人</dd>
        <dd>是一个非常好的人1</dd>
        <dd>是一个非常好的人2</dd>
    </dl>
```
## 6 BootStrap 表格

### 6.1 基本表格

```html
基本样式表格.table 用来美化table控件
<table class="table">
  ...
</table>
```

### 6.2 条纹状表格(斑马线表格)

```html
<table class="table table-striped">
  ...
</table>
```

### 6.3 带边框的表格

```html
<table class="table table-bordered">
  ...
</table>
```

### 6.4 鼠标悬停表格(鼠标引入变色)

```html
<table class="table table-hover">
  ...
</table>
```

### 6.5 紧缩表格

```html
<table class="table table-condensed">
  ...
</table>
```

### 6.6 表格的状态类

```html
1.提供的状态类

.active	    鼠标悬停在行或单元格上时所设置的颜色  灰色着重
.success	标识成功或积极的动作               浅绿色
.info	    标识普通的提示信息或动作            浅蓝色     
.warning	标识警告或需要用户注意             
.danger	    标识危险或潜在的带来负面影响的动作

2.在tr中使用
	<tr class="active">...</tr>
	<tr class="success">...</tr>
	<tr class="warning">...</tr>
	<tr class="danger">...</tr>
	<tr class="info">...</tr>
3.在td中使用  注意: 只有当前td的背景颜色发生变化
    <tr>
      <td class="active">...</td>
      <td class="success">...</td>
      <td class="warning">...</td>
      <td class="danger">...</td>
      <td class="info">...</td>
    </tr>
```

## 7 BootStrap 表单

### 7.1 基本实例

```html
使用方式:单独的表单控件会被自动赋予一些全局样
式。所有设置了 .form-control 类
的 <input>、<textarea> 和 <select> 元素都将
被默认设置宽度属性为 width: 100%;。 将 label 元
素和前面提到的控件包裹在 .form-group 中可以获得最好的排列。
    
1.基本实例
   <form action="">
         <div class="form-group">
             <label for="aa">用户名:</label>
             <input type="text" id="aa" name="name" class="form-control"/>
         </div>
         <div class="form-group">
             <label for="pwd">密码:</label>
             <input type="text" id="pwd" name="name" class="form-control"/>
         </div>

         <div class="form-group">
             <label for="exampleInputFile">请选择文件:</label>
             <input type="file" id="exampleInputFile">
             <p class="help-block">上传文件的大小不能超过2M!</p>
         </div>

         <div class="checkbox">
             <label>
                 <input type="checkbox"> 电视
             </label>
             <label>
                 <input type="checkbox"> 睡觉
             </label>
             <label>
                 <input type="checkbox"> 吃饭
             </label>
         </div>

         <select class="form-control">
             <option value="1">北京</option>
             <option value="2">南京</option>
             <option value="3">东京</option>
         </select>
    </form>
```

### 7.2 内联表单(一行展示表单控件)

```html
<form action="" class="form-inline">
    <div class="form-group">
       <label for="aa">用户名:</label>
       <input type="text" id="aa" name="name" placeholder="输名." class="form-control"/>
    </div>
    <div class="form-group">
        <label for="pwd">密码:</label>
        <input type="text" id="pwd" name="name" class="form-control"/>
    </div>

    <input type="submit" value="登录"></input>
</form>
```

### 7.3 输入框组(定制input框)

```html
<form action="" class="form-inline">
    <div class="form-group">
        <label for="name">请输入金额:</label>
        <div class="input-group">
            <div class="input-group-addon">$</div>
            <input type="text" class="form-control" id="name">
            <div class="input-group-addon">.00</div>
        </div>
    </div>
</form>
```

### 7.4 水平排列表单 (必须和栅格系统连用)

```html
<div class="container">
        <div class="row">
            <div class="col-sm-1"></div>
            <div class="col-sm-10">
                <form action="" class="form-horizontal">
                    <div class="form-group">
                        <label for="name" class="col-sm-2 control-label">用户名</label>
                        <div class="col-sm-10">
                            <input type="text" id="name" class="form-control"/>
                        </div>
                    </div>
                </form>
            </div>
            <div class="col-sm-1"></div>
        </div>
</div>

```


### 7.5 文本域

```html
<textarea class="form-control" rows="10"></textarea>
```

### 7.6 复选框 和 单选扭

```html
1.复选框|单选按钮水平排列
 	<div class="checkbox">
        <label>
            <input type="checkbox"> 看书
        </label>
        <label>
            <input type="checkbox"> 看书
        </label>
	</div>

2.复选框单选按钮垂直排列
	<div class="checkbox">
        <label>
            <input type="checkbox"> 看书
        </label>
    </div>
    <div class="checkbox">
        <label>
            <input type="checkbox" disabled> 看书
        </label>
    </div>

3.使用bootstrap提供样式水平展示(.checkbox-inline .radio-inline)
<label class="checkbox-inline">
    <input type="checkbox" id="aa0" value="option1"> 1
</label>
<label class="checkbox-inline">
    <input type="checkbox" id="aa2" value="option2"> 2
</label>
<label class="checkbox-inline">
    <input type="checkbox" id="aa3" value="option3"> 3
</label>

<hr>
<label class="radio-inline">
    <input type="radio" name="rad" id="aa4" value="option1"> 1
</label>
<label class="radio-inline">
    <input type="radio" name="rad" id="aa5" value="option2"> 2
</label>
<label class="radio-inline">
    <input type="radio" name="rad" id="aa6" value="option3"> 3
</label>
```

### 7.7 下拉列表

```html
1.下拉列表(单选) 加入form-control可以更好的适应bootstrap	
	<select name="" id="" class="form-control">
        <option value="1">北京</option>
        <option value="1">东京</option>
        <option value="1">天津</option>
    </select>

2.下拉列表(多选)  实现多选必须在select标签上加入multiple属性
    <select name="citys" multiple class="form-control">
        <option value="1">北京</option>
        <option value="1">东京</option>
        <option value="1">天津</option>
    </select>
```

### 7.8 静态控件

```html
<div class="contaier">
    <div class="row">
        <div class="col-sm-12">
            <form action="" class="form-horizontal">
                <div class="form-group">
                    <label class="col-sm-2 control-label">Emial</label>
                    <div class="col-sm-10">
                       <p class="form-control-static"><strong><u>60992323@qq.com</u></strong></p>
                    </div>
                </div>
            </form>
        </div>
    </div>
</div>
```

### 7.9 校验状态

```html
Bootstrap 对表单控件的校验状态，如 error、warning 
和 success 状态，都定义了样式。使用时，添
加 .has-warning、.has-error 或 .has-success 类到
这些控件的父元素即可。任何包含在此元素之内
的 .control-label、.form-control 和 .help-block 元
素都将接受这些校验状态的样式。

1.使用校验状态
<div class="form-group has-warning">
    <label class="col-sm-2 control-label">Emial</label>
    <div class="col-sm-10">
        <input type="text" name="email" class="form-control">
        <p class="help-block">必须是邮箱格式</p>
    </div>
</div>

2.为校验状态设置图标 
   注意:反馈图标（feedback icon）只能使用在文本输入框 <input class="form-control"> 元素上
   <div class="form-group has-warning  has-feedback">
       <label class="col-sm-2 control-label">Emial</label>
       <div class="col-sm-10">
           <input type="text" name="email" class="form-control ">
           <span class="glyphicon glyphicon-ok  form-control-feedback"></span>
           <p class="help-block">必须是邮箱格式</p>
       </div>
	</div>

```



```html
3.为校验设置前后图标
<div class="form-group has-warning  has-feedback">
    <label class="col-sm-2 control-label">Emial</label>
    <div class="col-sm-10">
        <div class="input-group">
            <span class="input-group-addon">@</span>
            <input type="text" name="email" class="form-control ">
        </div>
        <span class="glyphicon glyphicon-ok  form-control-feedback"></span>
        <p class="help-block">必须是邮箱格式</p>
    </div>
</div>

```


### 7.10 控件尺寸

```html
1.控制form基本实例控件的尺寸(.input-lg大 中form-control默认 .input-sm小 )
	<input class="form-control input-lg" type="text" placeholder=".input-lg">
    <input class="form-control" type="text" placeholder="Default input">
    <input class="form-control input-sm" type="text" placeholder=".input-sm">

    <select class="form-control input-lg">...</select>
    <select class="form-control">...</select>
    <select class="form-control input-sm">...</select>

2.控制form表单水平排列的尺寸(.form-group-lg)

<div class="form-group form-group-lg">
    <label class="col-sm-2 control-label" for="formGroupInputLarge">Large label</label>
    <div class="col-sm-10">
        <input class="form-control" type="text" id="formGroupInputLarge" placeholder="Large input">
    </div>
</div>
```


## 8 BootStrap 按钮

### 8.1 可以作为按钮标签

```html
为 <a>、<button> 或 <input> 元素添加按钮
类（button class）即可使用 Bootstrap 提供的样式。
```

### 8.2 创建按钮(.btn)

```html
<a href="" class="btn btn-default">link</a>
<button class="btn btn-default">button</button>
<input type="button" value="input" class="btn btn-default">
```

### 8.3 内置按钮样式

```html
<button class="btn btn-default">link</button>
<button class="btn btn-primary">link</button>
<button class="btn btn-warning">link</button>
<button class="btn btn-info">link</button>
<button class="btn btn-success">link</button>
<button class="btn btn-danger">link</button>
<button class="btn btn-link">link</button>
```

### 8.4 调整按钮尺寸

```html
使用 .btn-lg、.btn-sm 或 .btn-xs 就可以获
得不同尺寸的按钮
<button class="btn btn-success btn-lg">link(默认样式)</button>
<button class="btn btn-default">link(默认样式)</button>
<button class="btn btn-default btn-sm">link(默认样式)</button>
<button class="btn btn-default btn-xs">link(默认样式)</button>
```

### 8.5 按钮适应父元素

```html
通过给按钮添加 .btn-block 类可以将其拉伸至父
元素100%的宽度，而且按钮也变为了块级（block）元素。
 <button class="btn btn-success btn-lg btn-block">link(默认样式)</button>
```

### 8.6 按钮激活状态

```html
1.但是在需要让其表现出同样外观的时候可以添加 .active 类。
<button class="btn btn-success btn-lg btn-block active">link(默认样式)</button>
2.为<button> 元素添加 disabled 属性，使其表现出禁用状态。
<a href="" disabled="disabled" class="btn btn-success btn-lg btn-block active">xxxxx </a>
```


## 9 BootStrap图片

### 9.1 图片的生成

```html
通过为 <img> 元素添加以下相应的类.img-rounded (圆角) 
.img-circle(圆形) .img-thumbnail(边框)
 注意:跨浏览器兼容性
	请时刻牢记：Internet Explorer 8 不支持 CSS3 
	中的圆角属性。
   <img src="../boot/img/aa.jpg" class="img-rounded" alt="这是提示信息" title="嘻嘻嘻嘻嘻">
   <img src="../boot/img/aa.jpg" class="img-circle" alt="这是提示信息" title="嘻嘻嘻嘻嘻">
   <img src="../boot/img/aa.jpg" class="img-thumbnail" alt="这是提示信息" title="嘻嘻嘻嘻嘻">
```


## 10 Bootstrap辅助类

### 10.1 情境文本颜色

```html
<p class="text-muted">...</p>
<p class="text-primary">...</p>
<p class="text-success">...</p>
<p class="text-info">...</p>
<p class="text-warning">...</p>
<p class="text-danger">...</p>
```

### 10.2 情境背景色

```html
<p class="bg-primary">...</p>
<p class="bg-success">...</p>
<p class="bg-info">...</p>
<p class="bg-warning">...</p>
<p class="bg-danger">...</p>
```

### 10.3 关闭按钮

```html
 <button type="button" class="close" aria-label="Close"><span aria-hidden="true">&times;</span></button>
```

### 10.4 三角按钮

```html
<span class="caret"></span>
```

### 10.5 快速浮动

```html
<div class="pull-left">...</div>
<div class="pull-right">...</div>
```


## 11 BootStrap 字体图标

### 11.1 使用字体图标

```html
1.生成图标注意事项
	a.为了设置正确的内补（padding），务必在图
	标和文本之间添加一个空格。
	b.图标类不能和其它组件直接联合使用。它们不能
	在同一个元素上与其他类共同存在
    c.应该创建一个嵌套的<span> 标签，并将图标类
    应用到这个 <span> 标签上
    d.图标类只能应用在不包含任何文本内容或子元素的元素上
       
2.创建图标
  	<span class="glyphicon glyphicon-king"></span> 皇冠
    <span class="glyphicon glyphicon-flash"></span> 闪电
        
3.应用与按钮图标
    a.文字和图标存在
    <button class="btn btn-primary btn-lg">
        <span class="glyphicon glyphicon-apple"></span> link
    </button>
    b.只有图标存在
    <button class="btn btn-success">
        <span class="glyphicon glyphicon-align-left"></span>
    </button>
```


## 12 BootStrap 下拉菜单

### 12.1 下拉菜单

```html
1.创建下拉菜单注意事项
	a. 要求出发下拉菜单控件 和 下拉菜单必须通过.dropdown类包裹
    b. 触发按钮要加入 .dropdown-toggle 和 data-toggle属性的值必须为 dropdown
	c. 下拉菜单中 ul 必须加入 .dropdown-menu 

2.创建下拉菜单
    <div class="dropdown">
        <button class="btn btn-default dropdown-toggle" data-toggle="dropdown">
            点我实现拉列表 <span class="caret"></span>
        </button>
        <ul class="dropdown-menu">
            <li><a href="">Springmvc</a></li>
            <li><a href="">Springboot</a></li>
            <li><a href="">Springcloud</a></li>
        </ul>
    </div>
```

### 12.2 下拉菜单添加标题

```html
<div class="dropdown">
       <button class="btn btn-default dropdown-toggle" data-toggle="dropdown">
           点我下拉 <span class="caret"></span>
       </button>
       <ul class="dropdown-menu">
           <li class="dropdown-header"><a href="">框架</a></li>
           <li><a href="">Spring MVC</a></li>
           <li><a href="">Spring BOOT</a></li>
           <li><a href="">Spring Cloud</a></li>
           <li class="dropdown-header"><a href="">WEB</a></li>
           <li><a href="">JDBC</a></li>
           <li  class="divider"></li>    //这是分割线
           <li class="disabled"><a href="">Mybatis</a></li>   //disabled 不可用
       </ul>
</div>
```

## 13 BootStrap按钮组

### 13.1 创建按钮组

```html
使用方式:  将多个按钮通过.btn-group包裹
<div class="btn-group">
    <button class="btn btn-default">按钮</button>
    <button class="btn btn-default">按钮</button>
    <button class="btn btn-default">按钮</button>
</div>
```

### 13.2 创建按钮组工具栏(toolbar)

```html
<div class="btn-toolbar" >
  <div class="btn-group" >...</div>
  <div class="btn-group" >...</div>
  <div class="btn-group" >...</div>
</div>
```

### 13.3 按钮组的下拉菜单

```html
<div class="btn-group">
    <button class="btn btn-default">按钮1</button>
    <button class="btn btn-default">按钮2</button>
    <!--下拉菜单-->
    <div class="btn-group">
        <button class="btn btn-default dropdown-toggle" data-toggle="dropdown">
            点我下拉 <span class="caret"></span>
        </button>
        <ul class="dropdown-menu">
            <li><a href="">springmvc</a></li>
            <li><a href="">springmvc</a></li>
        </ul>
    </div>
</div>
```

### 13.4 垂直排列的下拉按钮

```html
1.使用说明如果想要使用按钮组垂直排列只需要
将.btn-group替换为.btn-group-vertical即可
<div class="btn-group-vertical" role="group" aria-label="...">
  ...
</div>
```

### 13.5 按钮组适应父容器

```html
1.如果想要使用按钮实现两端对齐(适应父元素),必须
使用<a> 元素创建按钮,然后将一系列 .btn 元素
包裹到 .btn-group.btn-group-justified 中即可。
 
 <div class="btn-group btn-group-justified" style="width: 100%;">
     <a class="btn btn-primary">按钮</a>
     <a class="btn btn-primary">按钮</a>
     <a class="btn btn-primary">按钮</a>
 </div>
    
2.关于button标签构建按钮组实现两端对齐(自适应父容器)
 <div class="btn-group btn-group-justified">
      <div class="btn-group">
          <button class="btn btn-default">按钮1</button>
      </div>
      <div class="btn-group">
          <button class="btn btn-default">按钮1</button>
      </div>
 </div>
```

## 14 BootStrap导航

### 14.1 标签页导航

```html
1.生成基本导航栏的注意事项:
	a. Bootstrap 中的导航组件都依赖同一个 .nav 类

2.生成标签页导航
<ul class="nav nav-tabs">
    <li class="active"><a href="">Home</a></li>
    <li><a href="">Home</a></li>
    <li><a href="">Home</a></li>
</ul>
```

### 14.2 胶囊式标签页

```html
<ul class="nav nav-pills">
    <li><a href="">Home</a></li>
    <li class="active"><a href="">Spring</a></li>
    <li><a href="">SpringMvc</a></li>
</ul>
```

### 14.3 两端对齐标签页

```html
<ul class="nav nav-tabs nav-justified">
  ...
</ul>
<ul class="nav nav-pills nav-justified">
  ...
</ul>
```

### 14.4 带有下拉菜单的标签页

```html
<ul class="nav nav-tabs">
        <li><a href="">HOME</a></li>
        <li><a href="">SpringMVC</a></li>
        <li class="dropdown">
            <a class="dropdown-toggle" data-toggle="dropdown">
                点我下拉 <span class="caret"></span>
            </a>
            <ul class="dropdown-menu">
                <li><a href="">北京</a></li>
                <li><a href="">上海</a></li>
                <li><a href="">南京</a></li>
            </ul>
        </li>
    </ul>
```


## 15 BootStrap导航条

### 15.1 生成导航条的标题

```html
1.生成导航条的注意事项:
	a.建议使用<nav>标签生成导航条
    b.生成导航条时让<nav>包裹.container .container-fuild

2.创建方式
 <nav class="navbar navbar-default">
    <div class="container-fluid">
        <!--导航条标题-->
        <div class="navbar-header">
            <a href="" class="navbar-brand">后台管理系统V1.0</a>
        </div>
    </div>
</nav>
```

### 15.2 生成导航条中(连接 按钮 下拉菜单)

```html
<div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav">
                <!--生成链接-->
                <li><a href="">Link</a></li>
                <!--生成按钮-->
                <li><button class="btn btn-default navbar-btn">按钮</button></li>
                <!--生成下拉菜单-->
                <li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Dropdown <span class="caret"></span></a>
                    <ul class="dropdown-menu">
                        <li><a href="#">Action</a></li>
                        <li><a href="#">Another action</a></li>
                        <li><a href="#">Something else here</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">Separated link</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">One more separated link</a></li>
                    </ul>
                </li>
            </ul>
        </div>
```

### 15.3 生成导航条中表单

```html
<!--导航条-->
<nav class="navbar navbar-default">
    <div class="container-fluid">
        <!--导航条标题-->
        <div class="navbar-header">
            <a href="" class="navbar-brand">
                <!--放入图标-->
               <!-- <img src="../boot/img/aa.jpg" width="20" height="20" alt="">-->
                <!--放入文本-->
                后台管理系统
            </a>
        </div>
        <!--导航内容-->
        <div class="collapse navbar-collapse" >
            <!--放入表单元素-->
            <form class="navbar-form navbar-left" role="search">
                <div class="form-group">
                    <input type="text" class="form-control" placeholder="Search">
                </div>
                <button type="submit" class="btn btn-default">Submit</button>
            </form>
            <ul class="nav navbar-nav">
                <!--生成链接-->
                <li><a href="">Link</a></li>
                <!--生成按钮-->
                <li><button class="btn btn-default navbar-btn">按钮</button></li>
                <!--生成下拉菜单-->
                <li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Dropdown <span class="caret"></span></a>
                    <ul class="dropdown-menu">
                        <li><a href="#">Action</a></li>
                        <li><a href="#">Another action</a></li>
                        <li><a href="#">Something else here</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">Separated link</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">One more separated link</a></li>
                    </ul>
                </li>
            </ul>
        </div>
    </div>
</nav>

```

### 15.4 生成导航条放入文本 右侧组件

```html
<!--导航条-->
<nav class="navbar navbar-default">
    <div class="container-fluid">
        <!--导航条标题-->
        <div class="navbar-header">
            <a href="" class="navbar-brand">
                <!--放入图标-->
               <!-- <img src="../boot/img/aa.jpg" width="20" height="20" alt="">-->
                <!--放入文本-->
                后台管理系统
            </a>
        </div>
        <!--导航内容-->
        <div class="collapse navbar-collapse" >
            <!--放入表单元素-->
            <form class="navbar-form navbar-left" role="search">
                <div class="form-group">
                    <input type="text" class="form-control" placeholder="Search">
                </div>
                <button type="submit" class="btn btn-default">Submit</button>
            </form>
            <ul class="nav navbar-nav">
                <!--生成链接-->
                <li><a href="">Link</a></li>
                <!--生成按钮-->
                <li><button class="btn btn-default navbar-btn">按钮</button></li>
                <!--生成下拉菜单-->
                <li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Dropdown <span class="caret"></span></a>
                    <ul class="dropdown-menu">
                        <li><a href="#">Action</a></li>
                        <li><a href="#">Another action</a></li>
                        <li><a href="#">Something else here</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">Separated link</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">One more separated link</a></li>
                    </ul>
                </li>
            </ul>
            <!--放入文本元素-->
            <p class="navbar-text">Signed in as Mark Otto</p>
            <!--放入非导航链接-->
            <p class="navbar-text">Signed in as <a href="#" class="navbar-link">Mark Otto</a></p>

            <!--将元素放在右侧-->
            <ul class="nav navbar-nav navbar-right">
                <li><a href="">Link</a></li>
            </ul>
        </div>
    </div>
</nav>

```

### 15.5 导航条位置

```html
1.固定在顶部
<nav class="navbar navbar-default navbar-fixed-top">
  <div class="container">
    ...
  </div>
</nav>

2.固定在底部
<nav class="navbar navbar-default navbar-fixed-bottom">
  <div class="container">
    ...
  </div>
</nav>
```

### 15.6 反色导航条

```html
<nav class="navbar navbar-inverse">
  ...
</nav>
```

### 15.7 简单方式导航条

```html
<!--生成导航条-->
<nav class="navbar navbar-inverse">
    <div class="container-fluid">

        <!--生成导航标题-->
        <div class="navbar-header">
            <a href="" class="navbar-brand">后台管理系统</a>
        </div>
        <!--生成导航内容-->
        <ul class="nav navbar-nav">
            <li class="active"><a href="">Link</a></li>
            <li><button class="btn btn-default navbar-btn">按钮</button></li>
            <li>
                <form class="navbar-form">
                    <div class="form-group">
                        <input type="text" class="form-control">
                    </div>
                    <button class="btn btn-primary">提交</button>
                </form>
            </li>
            <li class="dropdown">
                <a class="dropdown-toggle" data-toggle="dropdown">
                    下拉菜单 <span class="caret"></span>
                </a>
                <ul class="dropdown-menu">
                    <li><a href="">Springmvc</a></li>
                    <li><a href="">Springmvc</a></li>
                </ul>
            </li>
            <li>
                <p class="navbar-text">this is xiaohei </p>
            </li>
        </ul>
    </div>
</nav>
```

## 16 BootStrap分页

### 16.1 基本分页

```html
<nav aria-label="Page navigation" class="pull-right" >
    <ul class="pagination">
        <li>
            <a href="#" aria-label="Previous">
                <span aria-hidden="true">&laquo;</span>
            </a>
        </li>
        <li><a href="#">1</a></li>
        <li><a href="#">2</a></li>
        <li><a href="#">3</a></li>
        <li><a href="#">4</a></li>
        <li><a href="#">5</a></li>
        <li>
            <a href="#" aria-label="Next">
                <span aria-hidden="true">&raquo;</span>
            </a>
        </li>
    </ul>
</nav>
```

### 16.2 分页激活状态

```html
<nav aria-label="...">
  <ul class="pagination">
    <li class="disabled"><a href="#" aria-label="Previous"><span aria-hidden="true">&laquo;</span></a></li>
    <li class="active"><a href="#">1 <span class="sr-only">(current)</span></a></li>
    ...
  </ul>
</nav>
```

### 16.3 简单分页

```html
<!--基本简单分页-->
<nav aria-label="...">
    <ul class="pager">
        <li><a href="#">Previous</a></li>
        <li><a href="#">Next</a></li>
    </ul>
</nav>

<hr>
<!--两端分页-->
<nav aria-label="...">
    <ul class="pager">
        <li class="previous"><a href="#">Previous</a></li>
        <li class="next"><a href="#">Next</a></li>
    </ul>
</nav>

<hr>
<!--禁用分页-->
<nav aria-label="...">
    <ul class="pager">
        <li class="previous disabled"><a href="#">Previous</a></li>
        <li class="next"><a href="#">Next</a></li>
    </ul>
</nav>

```

## 17 标签

### 17.1 简单标签

```html
<h3>Example heading <span class="label label-default">New</span></h3>
```

### 17.2 标签变体

```html
<span class="label label-default">Default</span>
<span class="label label-primary">Primary</span>
<span class="label label-success">Success</span>
<span class="label label-info">Info</span>
<span class="label label-warning">Warning</span>
<span class="label label-danger">Danger</span>

```
## 18 徽章

```html
1.生成徽章使用  <span class="badge">数字</span>

2.创建简单徽章
<a href="">小黑 <span class="badge">1</span></a>

3.创建按钮徽章
<button class="btn btn-primary">
    按钮 <span class="badge">18</span>
</button>
```

## 19 巨幕

```html
<div class="jumbotron">
        <h1>Hello, world!</h1>
        <p>This is a simple hero unit, a simple jum....</p>
        <p><a class="btn btn-primary btn-lg" href="#" role="button">Learn more</a></p>
    </div>
```

## 20 页头

```html
<div class="page-header">
  <h1>Example page header <small>Subtext for header</small></h1>
</div>
```

## 21 警告框

### 21.1 基本实例

```html
1.生成警告框注意事项:
	a.警告框的基类是.alert 使用时必须先引入这个类

2.创建警告框
<div class="alert alert-success" role="alert">...</div>
<div class="alert alert-info" role="alert">...</div>
<div class="alert alert-warning" role="alert">...</div>
<div class="alert alert-danger" role="alert">...</div>

```

### 21.2 关闭警告框

```html
1.在关闭的按钮上加入data-dismiss="alert" 可
以实现关闭警告框功能 注意:依赖jqueryjs 和 boot的js

<div class="alert alert-warning">
    <button type="button" class="close" data-dismiss="alert" aria-label="Close">
        <span aria-hidden="true">&times;</span>
    </button>
    可以关闭的
</div>
```

### 21.3 警告框中连接

```html
<div class="alert alert-warning">
    <button type="button" class="close" data-dismiss="alert" aria-label="Close">
        <span aria-hidden="true">&times;</span>
    </button>
    <a href="" class="alert-link">可以关闭的</a>
</div>
注意: alert-link 这个类可以保证连接的颜色和警告框的颜色一致
```

## 22 列表组

### 22.1 基本实例

```html
<ul class="list-group">
  <li class="list-group-item">Cras justo odio</li>
  <li class="list-group-item">Dapibus ac facilisis in</li>
  <li class="list-group-item">Morbi leo risus</li>
  <li class="list-group-item">Porta ac consectetur ac</li>
  <li class="list-group-item">Vestibulum at eros</li>
</ul>
```

### 22.2 加入徽章的列表组

```html
<ul class="list-group">
    <li class="list-group-item">小黑 <span class="badge">34</span></li>
    <li class="list-group-item">小白 <span class="badge">14</span></li>
</ul>
```

### 22.3 链接列表组

```html
1.连接列表组使用div替换原来ul 使用a标签替换原来的
li标签,并将a标签href设置指定路径

<div class="list-group">
    <a href="http://www.baidu.com" class="list-group-item active">
        Cras justo odio
    </a>
    <a href="#" class="list-group-item">Dapibus ac facilisis in</a>
    <a href="#" class="list-group-item">Morbi leo risus</a>
    <a href="#" class="list-group-item">Porta ac consectetur ac</a>
    <a href="#" class="list-group-item">Vestibulum at eros</a>
</div>
```

### 22.4 按钮列表组

```html
1.还是使用div作为容器标签  使用button标签作为列表组的项标签
<div class="list-group">
    <button type="button" class="list-group-item">Cras justo odio</button>
    <button type="button" class="list-group-item">Dapibus ac facilisis in</button>
    <button type="button" class="list-group-item">Morbi leo risus</button>
    <button type="button" class="list-group-item">Porta ac consectetur ac</button>
    <button type="button" class="list-group-item">Vestibulum at eros</button>
</div>
```

### 22.5 情景类

```html
<ul class="list-group">
  <li class="list-group-item list-group-item-success">Dapibus ac facilisis in</li>
  <li class="list-group-item list-group-item-info">Cras sit amet nibh libero</li>
  <li class="list-group-item list-group-item-warning">Porta ac consectetur ac</li>
  <li class="list-group-item list-group-item-danger">Vestibulum at eros</li>
</ul>
<div class="list-group">
  <a href="#" class="list-group-item list-group-item-success">Dapibus ac facilisis in</a>
  <a href="#" class="list-group-item list-group-item-info">Cras sit amet nibh libero</a>
  <a href="#" class="list-group-item list-group-item-warning">Porta ac consectetur ac</a>
  <a href="#" class="list-group-item list-group-item-danger">Vestibulum at eros</a>
</div>
```

### 22.6 列表组定制

```html
<div class="list-group">
  <a href="#" class="list-group-item active">
    <h4 class="list-group-item-heading">List group item heading</h4>
    <p class="list-group-item-text">...</p>
  </a>
</div>
```

## 23 面板

### 23.1 基本实例

```html
<div class="panel panel-default">
  <div class="panel-body">
    Basic panel example
  </div>
</div>
```

### 23.2 标题面板

```html
1.直接设置标题
<!--标题的面板-->
<div class="panel panel-default">
    <!--标题-->
    <div class="panel-heading">标题</div>
    <div class="panel-body">
        这是一个面板
    </div>
</div>

2.使用h标签设置标题
 <!--标题的面板-->
    <div class="panel panel-default">
        <!--标题-->
        <div class="panel-heading">
            <h3 class="panel-title">标题</h3>
        </div>
        <div class="panel-body">
            这是一个面板
        </div>
    </div>
```

### 23.3 带脚注的面板

```html
<div class="panel panel-default">
  <div class="panel-body">
    Panel content
  </div>
  <div class="panel-footer">Panel footer</div>
</div>
```

### 23.4 带表格的面板

```html
<div class="panel panel-default">
  <!-- Default panel contents -->
  <div class="panel-heading">Panel heading</div>
  <div class="panel-body">
    <p>...</p>
  </div>

  <!-- Table -->
  <table class="table">
    ...
  </table>
</div>
```


## 24 模态框

### 24.1 静态实例

```html
1.使用模态框注意事项:
	a.千万不要在一个模态框上重叠另一个模态框,要想
	同时支持多个模态框，需要自己写额外的代码来实现
    b.务必将模态框的 HTML 代码放在文档的最高层级
    内（也就是说，尽量作为 body 标签的直接子元素），以
    避免其他组件影响模态框的展现和使用

2.创建模态框
  a.在页面中生成模态框 注意:默认书写模态框在页面中没有展示 需要手动展示
	<div class="modal fade" id="myModal" data-backdrop="false" data-keyboard="false" tabindex="-1" role="dialog">
    <div class="modal-dialog" role="document">
        <div class="modal-content">
            <div class="modal-header">
                <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
                <h4 class="modal-title">Modal title</h4>
            </div>
            <div class="modal-body">
                <p>One fine body&hellip;</p>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
                <button type="button" class="btn btn-primary">Save changes</button>
            </div>
        </div><!-- /.modal-content -->
    </div><!-- /.modal-dialog -->
</div><!-- /.modal -->

	b.使用javascript代码通过参数形式展示
		$("#myModal").modal({
                show:true,//初始化后立即展示
         });
	c.设置其他参数直接在模态框中标签中使用data-参数形式进行设置
	<div class="modal fade" id="myModal" data-backdrop="false" data-keyboard="false" tabindex="-1">
    </div>
```

### 24.2 动态实例

```html
1.模态框在创建时是隐藏不可见的
<div class="modal fade" id="myModal" tabindex="-1">
    <div class="modal-dialog">
        <div class="modal-content">
            <!--模态框标题-->
            <div class="modal-header">
                <!--
                    用来关闭模态框的属性:data-dismiss="modal"
                -->
                <button type="button" class="close" data-dismiss="modal" ><span >&times;</span></button>
                <h4 class="modal-title">编辑用户信息</h4>
            </div>

            <!--模态框内容体-->
            <div class="modal-body">

                <form action="" class="form-horizontal">

                    <div class="form-group">
                        <label class="col-sm-2 control-label">用户名</label>
                        <div class="col-sm-10">
                            <input type="text" name="name" id="name" placeholder="请输入姓名" class="form-control">
                        </div>
                    </div>

                </form>

            </div>

            <!--模态页脚-->
            <div class="modal-footer">
                <button type="button" class="btn btn-primary">保存</button>
                <button type="button" class="btn btn-danger" data-dismiss="modal">取消</button>
            </div>

        </div>
    </div>
</div>

2.展示模态框可以调用模态框的方法
<div class="modal fade" id="myModal" tabindex="-1">
    <div class="modal-dialog">
        <div class="modal-content">
            <!--模态框标题-->
            <div class="modal-header">
                <!--
                    用来关闭模态框的属性:data-dismiss="modal"
                -->
                <button type="button" class="close" data-dismiss="modal" ><span >&times;</span></button>
                <h4 class="modal-title">编辑用户信息</h4>
            </div>

            <!--模态框内容体-->
            <div class="modal-body">

                <form action="" class="form-horizontal">

                    <div class="form-group">
                        <label class="col-sm-2 control-label">用户名</label>
                        <div class="col-sm-10">
                            <input type="text" name="name" id="name" placeholder="请输入姓名" class="form-control">
                        </div>
                    </div>

                </form>

            </div>

            <!--模态页脚-->
            <div class="modal-footer">
                <button type="button" class="btn btn-primary">保存</button>
                <button type="button" class="btn btn-danger" data-dismiss="modal">取消</button>
            </div>

        </div>
    </div>
</div>

3.使用模态框中事件
$("#myModal").on("show.bs.modal",function () {
	console.log("1");
});

$("#myModal").on("shown.bs.modal",function () {
	console.log("2");
})

//监听隐藏事件
$("#myModal").on("hide.bs.modal",function () {
	console.log("3");
})

$("#myModal").on("hidden.bs.modal",function () {
	console.log("4");
})
```

## 25 标签页

### 25.1 标签页动态实例

```html
<!--标签-->
    <ul class="nav nav-tabs">
        <li class="active"><a href="#list" data-toggle="tab">用户列表</a></li>
        <li><a href="#saveInfo" data-toggle="tab">用户添加</a></li>
        <li class="dropdown">
            <!--触发器-->
            <a href="" class="dropdown-toggle" data-toggle="dropdown">
                下拉菜单 <span class="caret"></span>
            </a>
            <!--下拉菜单-->
            <ul class="dropdown-menu">
                <li><a href="#SpringMvc" data-toggle="tab">SpringMVC</a></li>
                <li><a href="#SpringBoot" data-toggle="tab">SpringBOOT</a></li>
            </ul>
        </li>
    </ul>

    <!--标签内容组-->
    <div class="tab-content">
        <!--标签内容面板-->
        <div class="tab-pane active" id="list">A</div>

        <div class="tab-pane" id="saveInfo">B</div>
        <div class="tab-pane" id="SpringMvc">SpringMvc</div>
        <div class="tab-pane" id="SpringBoot">SpringBoot</div>
    </div>
```

### 25.2 胶囊式标签页动态实例

```html
<ul class="nav nav-pills ">
    <li class="active"><a href="#mvc" data-toggle="pill">Springmvc</a></li>
    <li><a href="#boot" data-toggle="pill">SpringBoot</a></li>
</ul>



<!--标签页内容组-->
<div class="tab-content">
    <div class="tab-pane active" id="mvc">MVC</div>
    <div class="tab-pane" id="boot">Boot</div>
</div>
```

### 25.3 胶囊式标签页方法调用

```html
1.使用tab的展示(show)方法时:
	a.应该使用对应标签页链接id调用tab的show方法进行展示
```
### 25.4 标签页事件的使用

```js
 $('a[data-toggle="pill"]').on('show.bs.tab', function (e) {
    console.log(e.target); //切换之后目标对象
    console.log(e.relatedTarget);  //切换之前的对象
    console.log($(e.target).text());
    console.log($(e.target).attr("name"));
    console.log("1")
})
$('a[data-toggle="pill"]').on('shown.bs.tab', function (e) {
    console.log(e.target); //切换之后目标对象
    console.log(e.relatedTarget);  //切换之前的对象
    console.log("2")
})

$('a[data-toggle="pill"]').on('hide.bs.tab', function (e) {
    console.log(e.target); //切换之后目标对象
    console.log(e.relatedTarget);  //切换之前的对象
    console.log("1")
});

$('a[data-toggle="pill"]').on('hidden.bs.tab', function (e) {
    console.log(e.target); //切换之后目标对象
    console.log(e.relatedTarget);  //切换之前的对象
    console.log("2")
})
```

## 26 Accordion组件(手风琴)

### 26.1 基本实例

```html
 <!--创建手风琴实例-->
    <div class="panel-group" id="panelgroup">


        <!--创建面板-->
        <div class="panel panel-default">
            <div class="panel-heading">
                <div class="panel-title">
                    <!--使用连接完成折叠效果-->
                    <a href="#aa" data-toggle="collapse" data-parent="#panelgroup" ><h5>用户管理</h5></a>
                </div>
            </div>

            <div class="panel-collapse collapse" id="aa">
                <div class="panel-body" >
                    <p>标签页(胶囊式)02.html</p>
                    <p>标签页(胶囊式)02.html</p>
                    <p>标签页(胶囊式)02.html</p>
                    <p>标签页(胶囊式)02.html</p>
                    <p>标签页(胶囊式)02.html</p>
                    <p>标签页(胶囊式)02.html</p>
                    <p>标签页(胶囊式)02.html</p>
                    <p>标签页(胶囊式)02.html</p>
                    <p>标签页(胶囊式)02.html</p>
                    <p>标签页(胶囊式)02.html</p>
                    <p>标签页(胶囊式)02.html</p>
                </div>
            </div>
        </div>


        <!--创建另一个面板-->
        <div class="panel panel-default">
            <div class="panel-heading">
                <div class="panel-title">
                    <!--使用连接完成折叠效果-->
                    <a href="#bb" data-toggle="collapse" data-parent="#panelgroup" ><h5>用户管理</h5></a>
                </div>
            </div>

            <div class="panel-collapse collapse" id="bb">
                <div class="panel-body" >
                    <p>标签页(胶囊式)02.html</p>
                    <p>标签页(胶囊式)02.html</p>
                    <p>标签页(胶囊式)02.html</p>
                    <p>标签页(胶囊式)02.html</p>
                    <p>标签页(胶囊式)02.html</p>
                    <p>标签页(胶囊式)02.html</p>
                    <p>标签页(胶囊式)02.html</p>
                    <p>标签页(胶囊式)02.html</p>
                    <p>标签页(胶囊式)02.html</p>
                    <p>标签页(胶囊式)02.html</p>
                    <p>标签页(胶囊式)02.html</p>
                </div>
            </div>
        </div>
    </div>
```

### 26.2 带有列表组的手风琴

```html
<!--创建手风琴实例-->
<div class="panel-group" id="panelgroup">


    <!--创建面板-->
    <div class="panel panel-default">
        <div class="panel-heading">
            <div class="panel-title">
                <!--使用连接完成折叠效果-->
                <a href="#aa" data-toggle="collapse" data-parent="#panelgroup" ><h5>用户管理</h5></a>
            </div>
        </div>

        <div class="panel-collapse collapse" id="aa">
            <ul class="list-group">
                <li class="list-group-item"><a href="">用户列表</a></li>
                <li class="list-group-item"><a href="">用户添加</a></li>
            </ul>
        </div>
    </div>


    <!--创建另一个面板-->
    <div class="panel panel-default">
        <div class="panel-heading">
            <div class="panel-title">
                <!--使用连接完成折叠效果-->
                <a href="#bb" data-toggle="collapse" data-parent="#panelgroup" ><h5>类别管理</h5></a>
            </div>
        </div>

        <div class="panel-collapse collapse" id="bb">
            <ul class="list-group">
                <li class="list-group-item">类别列表</li>
                <li class="list-group-item">添加类别</li>
            </ul>
        </div>
    </div>
  
</div>
```

### 26.3 手风琴方法使用

```html
1.调用手风琴方法:
	a.使用对应面板的内容的id调用collapse的相关方法  (使用隐藏内容的唯一标识调用方法)

2.代码如下
 <!--创建面板-->
        <div class="panel panel-default">
            <div class="panel-heading">
                <div class="panel-title">
                    <!--使用连接完成折叠效果-->
                    <a href="#aa" data-toggle="collapse" data-parent="#panelgroup" ><h5>用户管理</h5></a>
                </div>
            </div>

            <div class="panel-collapse collapse in" id="aa">
                <ul class="list-group">
                    <li class="list-group-item"><a href="">用户列表</a></li>
                    <li class="list-group-item"><a href="">用户添加</a></li>
                </ul>
            </div>

        </div>

//展开指定面板
$("#aa").collapse('show');
$("#aa").collapse('toggle');
$("#aa").collapse('hide');
```

### 26.4 事件的使用

```html
1.监听指定面板的事件
<!--创建手风琴实例-->
    <div class="panel-group" id="panelgroup">

        <!--创建面板-->
        <div class="panel panel-default">
            <div class="panel-heading">
                <div class="panel-title">
                    <!--使用连接完成折叠效果-->
                    <a href="#aa" data-toggle="collapse" data-parent="#panelgroup" ><h5>用户管理</h5></a>
                </div>
            </div>

            <div class="panel-collapse collapse in" id="aa">
                <ul class="list-group">
                    <li class="list-group-item"><a href="">用户列表</a></li>
                    <li class="list-group-item"><a href="">用户添加</a></li>
                </ul>
            </div>

        </div>


        <!--创建另一个面板-->
        <div class="panel panel-default">
            <div class="panel-heading">
                <div class="panel-title">
                    <!--使用连接完成折叠效果-->
                    <a href="#bb" data-toggle="collapse" data-parent="#panelgroup" ><h5>类别管理</h5></a>
                </div>
            </div>

            <div class="panel-collapse collapse" id="bb">
                <ul class="list-group">
                    <li class="list-group-item">类别列表</li>
                    <li class="list-group-item">添加类别</li>
                </ul>
            </div>
        </div>
    </div>


2.监听指定面板事件
	$('#aa').on('show.bs.collapse', function () {
    	console.log("1")
    });

    $('#aa').on('shown.bs.collapse', function () {
    	console.log("2")
    });

3.监听所有面板的事件
	 $('.collapse').on('show.bs.collapse',function () {
    	console.log("1");
     });
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复
jb即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
