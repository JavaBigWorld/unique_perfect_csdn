# JavaScript之Web API-篇章4
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714134912356.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1 PC端网页特效
### 1.1 元素偏移量 offset 系列
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513080713248.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
offset 翻译过来就是偏移量， 我们使用 offset 系列相
关属性可以动态的得到该元素的位置（偏移）、大小等。
获得元素距离带有定位父元素的位置
获得元素自身的大小（宽度高度）
注意： 返回的数值都不带单位
```
### 1.2  offset系列属性
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
            /* position: relative; */
            width: 200px;
            height: 200px;
            background-color: pink;
            margin: 150px;
        }
        
        .son {
            width: 100px;
            height: 100px;
            background-color: purple;
            margin-left: 45px;
        }
        
        .w {
            height: 200px;
            background-color: skyblue;
            margin: 0 auto 200px;
            padding: 10px;
            border: 15px solid red;
        }
    </style>
</head>

<body>
    <div class="father">
        <div class="son"></div>
    </div>
    <div class="w"></div>
    <script>
        // offset 系列
        var father = document.querySelector('.father');
        var son = document.querySelector('.son');
        // 1.可以得到元素的偏移 位置 返回的不带单位的数值  
        console.log(father.offsetTop);
        console.log(father.offsetLeft);
        // 它以带有定位的父亲为准  如果么有父亲或者父亲没有定位 则以 body 为准
        console.log(son.offsetLeft);
        var w = document.querySelector('.w');
        // 2.可以得到元素的大小 宽度和高度 是包含padding + border + width 
        console.log(w.offsetWidth);
        console.log(w.offsetHeight);
        // 3. 返回带有定位的父亲 否则返回的是body
        console.log(son.offsetParent); // 返回带有定位的父亲 否则返回的是body
        console.log(son.parentNode); // 返回父亲 是最近一级的父亲 亲爸爸 不管父亲有没有定位
    </script>
</body>

</html>
```
### 1.3  offset 与 style 区别
```markdown
offset 可以得到任意样式表中的样式值
offset 系列获得的数值是没有单位的
 offsetWidth 包含padding+border+width
offsetWidth 等属性是只读属性，只能获取不能赋值
所以，我们想要获取元素大小位置，用offset更合适
style 只能得到行内样式表中的样式值
style.width 获得的是带有单位的字符串
style.width 获得不包含padding和border 的值
style.width 是可读写属性，可以获取也可以赋值
所以，我们想要给元素更改值，则需要用style改变
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
        .box {
            width: 200px;
            height: 200px;
            background-color: pink;
            padding: 10px;
        }
    </style>
</head>

<body>
    <div class="box" style="width: 200px;"></div>
    <script>
        // offset与style的区别
        var box = document.querySelector('.box');
        console.log(box.offsetWidth);
        console.log(box.style.width);
        // box.offsetWidth = '300px';
        box.style.width = '300px';
    </script>
</body>

</html>
```
### 1.4  计算鼠标在盒子内的坐标
```markdown
案例分析
1 我们在盒子内点击，想要得到鼠标距离盒子左右的距离。
2 首先得到鼠标在页面中的坐标（e.pageX, e.pageY）
3 其次得到盒子在页面中的距离 
( box.offsetLeft, box.offsetTop)
4 用鼠标距离页面的坐标减去盒子在页面中的距离，
得到 鼠标在盒子内的坐标
5 如果想要移动一下鼠标，就要获取最新的坐标，
使用鼠标移动事件 mousemove
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
        .box {
            width: 300px;
            height: 300px;
            background-color: pink;
            margin: 200px;
        }
    </style>
</head>

<body>
    <div class="box"></div>
    <script>
        // 我们在盒子内点击， 想要得到鼠标距离盒子左右的距离。
        // 首先得到鼠标在页面中的坐标（ e.pageX, e.pageY）
        // 其次得到盒子在页面中的距离(box.offsetLeft, box.offsetTop)
        // 用鼠标距离页面的坐标减去盒子在页面中的距离， 得到 鼠标在盒子内的坐标
        var box = document.querySelector('.box');
        box.addEventListener('mousemove', function(e) {
            // console.log(e.pageX);
            // console.log(e.pageY);
            // console.log(box.offsetLeft);
            var x = e.pageX - this.offsetLeft;
            var y = e.pageY - this.offsetTop;
            this.innerHTML = 'x坐标是' + x + ' y坐标是' + y;
        })
    </script>
</body>

</html>
```
### 1.5  拖动的模态框
```markdown
1 点击弹出层， 模态框和遮挡层就会显示出来 display:block;
2 点击关闭按钮，模态框和遮挡层就会隐藏起来 display:none;
3 在页面中拖拽的原理： 鼠标按下并且移动， 之后松开鼠标
4 触发事件是鼠标按下 mousedown， 鼠标移动mousemove 
鼠标松开 mouseup
5 拖拽过程: 鼠标移动过程中，获得最新的值赋值给模态
框的left和top值， 这样模态框可以跟着鼠标走了
6 鼠标按下触发的事件源是 最上面一行，就是 id 为 title
7 鼠标的坐标 减去 鼠标在盒子内的坐标， 才是模态框真正的位置。
8 鼠标按下，我们要得到鼠标在盒子的坐标。
9 鼠标移动，就让模态框的坐标 设置为 ： 鼠标坐标 减去
盒子坐标即可，注意移动事件写到按下事件里面。
10 鼠标松开，就停止拖拽，就是可以让鼠标移动事件解除 
```
```html
<!DOCTYPE html>
<html>

<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <style>
        .login-header {
            width: 100%;
            text-align: center;
            height: 30px;
            font-size: 24px;
            line-height: 30px;
        }
        
        ul,
        li,
        ol,
        dl,
        dt,
        dd,
        div,
        p,
        span,
        h1,
        h2,
        h3,
        h4,
        h5,
        h6,
        a {
            padding: 0px;
            margin: 0px;
        }
        
        .login {
            display: none;
            width: 512px;
            height: 280px;
            position: fixed;
            border: #ebebeb solid 1px;
            left: 50%;
            top: 50%;
            background: #ffffff;
            box-shadow: 0px 0px 20px #ddd;
            z-index: 9999;
            transform: translate(-50%, -50%);
        }
        
        .login-title {
            width: 100%;
            margin: 10px 0px 0px 0px;
            text-align: center;
            line-height: 40px;
            height: 40px;
            font-size: 18px;
            position: relative;
            cursor: move;
        }
        
        .login-input-content {
            margin-top: 20px;
        }
        
        .login-button {
            width: 50%;
            margin: 30px auto 0px auto;
            line-height: 40px;
            font-size: 14px;
            border: #ebebeb 1px solid;
            text-align: center;
        }
        
        .login-bg {
            display: none;
            width: 100%;
            height: 100%;
            position: fixed;
            top: 0px;
            left: 0px;
            background: rgba(0, 0, 0, .3);
        }
        
        a {
            text-decoration: none;
            color: #000000;
        }
        
        .login-button a {
            display: block;
        }
        
        .login-input input.list-input {
            float: left;
            line-height: 35px;
            height: 35px;
            width: 350px;
            border: #ebebeb 1px solid;
            text-indent: 5px;
        }
        
        .login-input {
            overflow: hidden;
            margin: 0px 0px 20px 0px;
        }
        
        .login-input label {
            float: left;
            width: 90px;
            padding-right: 10px;
            text-align: right;
            line-height: 35px;
            height: 35px;
            font-size: 14px;
        }
        
        .login-title span {
            position: absolute;
            font-size: 12px;
            right: -20px;
            top: -30px;
            background: #ffffff;
            border: #ebebeb solid 1px;
            width: 40px;
            height: 40px;
            border-radius: 20px;
        }
    </style>
</head>

<body>
    <div class="login-header"><a id="link" href="javascript:;">点击，弹出登录框</a></div>
    <div id="login" class="login">
        <div id="title" class="login-title">登录会员
            <span><a id="closeBtn" href="javascript:void(0);" class="close-login">关闭</a></span>
        </div>
        <div class="login-input-content">
            <div class="login-input">
                <label>用户名：</label>
                <input type="text" placeholder="请输入用户名" name="info[username]" id="username" class="list-input">
            </div>
            <div class="login-input">
                <label>登录密码：</label>
                <input type="password" placeholder="请输入登录密码" name="info[password]" id="password" class="list-input">
            </div>
        </div>
        <div id="loginBtn" class="login-button"><a href="javascript:void(0);" id="login-button-submit">登录会员</a></div>
    </div>
    <!-- 遮盖层 -->
    <div id="bg" class="login-bg"></div>
    <script>
        // 1. 获取元素
        var login = document.querySelector('.login');
        var mask = document.querySelector('.login-bg');
        var link = document.querySelector('#link');
        var closeBtn = document.querySelector('#closeBtn');
        var title = document.querySelector('#title');
        // 2. 点击弹出层这个链接 link  让mask 和login 显示出来
        link.addEventListener('click', function() {
                mask.style.display = 'block';
                login.style.display = 'block';
            })
            // 3. 点击 closeBtn 就隐藏 mask 和 login 
        closeBtn.addEventListener('click', function() {
                mask.style.display = 'none';
                login.style.display = 'none';
            })
            // 4. 开始拖拽
            // (1) 当我们鼠标按下， 就获得鼠标在盒子内的坐标
        title.addEventListener('mousedown', function(e) {
            var x = e.pageX - login.offsetLeft;
            var y = e.pageY - login.offsetTop;
            // (2) 鼠标移动的时候，把鼠标在页面中的坐标，减去 鼠标在盒子内的坐标就是模态框的left和top值
            document.addEventListener('mousemove', move)

            function move(e) {
                login.style.left = e.pageX - x + 'px';
                login.style.top = e.pageY - y + 'px';
            }
            // (3) 鼠标弹起，就让鼠标移动事件移除
            document.addEventListener('mouseup', function() {
                document.removeEventListener('mousemove', move);
            })
        })
    </script>
</body>

</html>
```
### 1.6 京东放大镜效果
```markdown
1 整个案例可以分为三个功能模块
2 鼠标经过小图片盒子， 黄色的遮挡层 和 大图片盒子显示，
离开隐藏2个盒子功能
3 黄色的遮挡层跟随鼠标功能。
4 移动黄色遮挡层，大图片跟随移动功能。
5 鼠标经过小图片盒子， 黄色的遮挡层 和 大图片盒子显示，
离开隐藏2个盒子功能
6 就是显示与隐藏
7 黄色的遮挡层跟随鼠标功能。
8 把鼠标坐标给遮挡层不合适。因为遮挡层坐标以父盒子为准。
9 首先是获得鼠标在盒子的坐标。
10 之后把数值给遮挡层做为left 和top值。
11 此时用到鼠标移动事件，但是还是在小图片盒子内移动。
12 发现，遮挡层位置不对，需要再减去盒子自身高度和宽度的一半。
13 遮挡层不能超出小图片盒子范围。
14 如果小于零，就把坐标设置为0
15 如果大于遮挡层最大的移动距离，就把坐标设置为最大的移动距离
16 遮挡层的最大移动距离： 小图片盒子宽度 减去 遮挡层盒子宽度

求大图片的移动距离公式 
大图片移动距离 = (遮挡层移动距离*大图片最大移动距离) / 遮挡层
最大移动距离
```
```javascript
window.addEventListener('load', function() {
    var preview_img = document.querySelector('.preview_img');
    var mask = document.querySelector('.mask');
    var big = document.querySelector('.big');
    // 1. 当我们鼠标经过 preview_img 就显示和隐藏 mask 遮挡层 和 big 大盒子
    preview_img.addEventListener('mouseover', function() {
        mask.style.display = 'block';
        big.style.display = 'block';
    })
    preview_img.addEventListener('mouseout', function() {
            mask.style.display = 'none';
            big.style.display = 'none';
        })
        // 2. 鼠标移动的时候，让黄色的盒子跟着鼠标来走
    preview_img.addEventListener('mousemove', function(e) {
        // (1). 先计算出鼠标在盒子内的坐标
        var x = e.pageX - this.offsetLeft;
        var y = e.pageY - this.offsetTop;
        // console.log(x, y);
        // (2) 减去盒子高度 300的一半 是 150 就是我们mask 的最终 left 和top值了
        // (3) 我们mask 移动的距离
        var maskX = x - mask.offsetWidth / 2;
        var maskY = y - mask.offsetHeight / 2;
        // (4) 如果x 坐标小于了0 就让他停在0 的位置
        // 遮挡层的最大移动距离
        var maskMax = preview_img.offsetWidth - mask.offsetWidth;
        if (maskX <= 0) {
            maskX = 0;
        } else if (maskX >= maskMax) {
            maskX = maskMax;
        }
        if (maskY <= 0) {
            maskY = 0;
        } else if (maskY >= maskMax) {
            maskY = maskMax;
        }
        mask.style.left = maskX + 'px';
        mask.style.top = maskY + 'px';
        // 3. 大图片的移动距离 = 遮挡层移动距离 * 大图片最大移动距离 / 遮挡层的最大移动距离
        // 大图
        var bigIMg = document.querySelector('.bigImg');
        // 大图片最大移动距离
        var bigMax = bigIMg.offsetWidth - big.offsetWidth;
        // 大图片的移动距离 X Y
        var bigX = maskX * bigMax / maskMax;
        var bigY = maskY * bigMax / maskMax;
        bigIMg.style.left = -bigX + 'px';
        bigIMg.style.top = -bigY + 'px';

    })

})
```
### 1.7  元素可视区 client 系列
```markdown
client 翻译过来就是客户端，我们使用 client 系列的相关属
性来获取元素可视区的相关信息。通过 client 系列
的相关属性可以动态的得到该元素的边框大小、元素大小等。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513123140120.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
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
            border: 10px solid red;
            padding: 10px;
        }
    </style>
</head>

<body>
    <div></div>
    <script>
        // client 宽度 和我们offsetWidth 最大的区别就是 不包含边框
        var div = document.querySelector('div');
        console.log(div.clientWidth);
    </script>
</body>

</html>
```
### 1.8  立即执行函数
```markdown
立即执行函数 (function() {})() 或者 (function(){}())
主要作用： 创建一个独立的作用域。 避免了命名冲突问题
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
    <script>
        // 1.立即执行函数: 不需要调用，立马能够自己执行的函数
        function fn() {
            console.log(1);

        }
        fn();
        // 2. 写法 也可以传递参数进来
        // 1.(function() {})()    或者  2. (function(){}());
        (function(a, b) {
            console.log(a + b);
            var num = 10;
        })(1, 2); // 第二个小括号可以看做是调用函数
        (function sum(a, b) {
            console.log(a + b);
            var num = 10; // 局部变量
        }(2, 3));
        // 3. 立即执行函数最大的作用就是 独立创建了一个作用域, 里面所有的变量都是局部变量 不会有命名冲突的情况
    </script>
</body>

</html>
```
### 1.9  像素比和pageshow事件
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <!-- <script src="flexible.js"></script> -->
</head>

<body>

    <script>
        // console.log(window.devicePixelRatio);
        window.addEventListener('pageshow', function() {
            alert(11);
        })
    </script>
    <a href="http://www.itcast.cn">链接</a>
</body>

</html>
```
### 1.10 元素滚动 scroll 系列
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513134934951.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513135111614.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

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
            border: 10px solid red;
            padding: 10px;
            overflow: auto;
        }
    </style>
</head>

<body>
    <div>
        我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容
    </div>
    <script>
        // scroll 系列
        var div = document.querySelector('div');
        console.log(div.scrollHeight);
        console.log(div.clientHeight);
        // scroll滚动事件当我们滚动条发生变化会触发的事件
        div.addEventListener('scroll', function() {
            console.log(div.scrollTop);

        })
    </script>
</body>

</html>
```
### 1.11 仿淘宝固定右侧侧边栏
```markdown
1 原先侧边栏是绝对定位
2 当页面滚动到一定位置，侧边栏改为固定定位
3 页面继续滚动，会让 返回顶部显示出来


1 需要用到页面滚动事件 scroll 因为是页面滚动，所以事件
源是 document
2 滚动到某个位置，就是判断页面被卷去的上部值。
3 页面被卷去的头部：可以通过window.pageYOffset 获得
如果是被卷去的左侧 window.pageXOffset
4 注意，元素被卷去的头部是 element.scrollTop , 如果是
页面被卷去的头部 则是 window.pageYOffset
5 其实这个值 可以通过盒子的 offsetTop 可以得到，如果大
于等于这个值，就可以让盒子固定定位了
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
        .slider-bar {
            position: absolute;
            left: 50%;
            top: 300px;
            margin-left: 600px;
            width: 45px;
            height: 130px;
            background-color: pink;
        }
        
        .w {
            width: 1200px;
            margin: 10px auto;
        }
        
        .header {
            height: 150px;
            background-color: purple;
        }
        
        .banner {
            height: 250px;
            background-color: skyblue;
        }
        
        .main {
            height: 1000px;
            background-color: yellowgreen;
        }
        
        span {
            display: none;
            position: absolute;
            bottom: 0;
        }
    </style>
</head>

<body>
    <div class="slider-bar">
        <span class="goBack">返回顶部</span>
    </div>
    <div class="header w">头部区域</div>
    <div class="banner w">banner区域</div>
    <div class="main w">主体部分</div>
    <script>
        //1. 获取元素
        var sliderbar = document.querySelector('.slider-bar');
        var banner = document.querySelector('.banner');
        // banner.offestTop 就是被卷去头部的大小 一定要写到滚动的外面
        var bannerTop = banner.offsetTop
            // 当我们侧边栏固定定位之后应该变化的数值
        var sliderbarTop = sliderbar.offsetTop - bannerTop;
        // 获取main 主体元素
        var main = document.querySelector('.main');
        var goBack = document.querySelector('.goBack');
        var mainTop = main.offsetTop;
        // 2. 页面滚动事件 scroll
        document.addEventListener('scroll', function() {
            // console.log(11);
            // window.pageYOffset 页面被卷去的头部
            // console.log(window.pageYOffset);
            // 3 .当我们页面被卷去的头部大于等于了 172 此时 侧边栏就要改为固定定位
            if (window.pageYOffset >= bannerTop) {
                sliderbar.style.position = 'fixed';
                sliderbar.style.top = sliderbarTop + 'px';
            } else {
                sliderbar.style.position = 'absolute';
                sliderbar.style.top = '300px';
            }
            // 4. 当我们页面滚动到main盒子，就显示 goback模块
            if (window.pageYOffset >= mainTop) {
                goBack.style.display = 'block';
            } else {
                goBack.style.display = 'none';
            }

        })
    </script>
</body>

</html>
```
### 1.12 三大系列总结
```markdown
元素偏移量 offset 系列
元素可视区 client 系列
元素滚动 scroll 系列
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513135518287.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
他们主要用法：
1 offset系列 经常用于获得元素位置 offsetLeft offsetTop
2 client 经常用于获取元素大小 clientWidth clientHeight
3 scroll 经常用于获取滚动距离 scrollTop scrollLeft
4  注意页面滚动的距离通过 window.pageXOffset 获得
```
### 1.13 mouseenter 和mouseover的区别
```markdown
当鼠标移动到元素上时就会触发 mouseenter 事件
 类似 mouseover，它们两者之间的差别是
 mouseover 鼠标经过自身盒子会触发，经过子盒子还会触发。 
 mouseenter 只会经过自身盒子触发
 之所以这样，就是因为mouseenter不会冒泡
 跟mouseenter搭配 鼠标离开 mouseleave 同样不会冒泡
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
        .father {
            width: 300px;
            height: 300px;
            background-color: pink;
            margin: 100px auto;
        }
        
        .son {
            width: 200px;
            height: 200px;
            background-color: purple;
        }
    </style>
</head>

<body>
    <div class="father">
        <div class="son"></div>
    </div>
    <script>
        var father = document.querySelector('.father');
        var son = document.querySelector('.son');
        father.addEventListener('mouseenter', function() {
            console.log(11);

        })
    </script>
</body>

</html>
```
### 1.14 动画函数封装
```markdown
核心原理：通过定时器 setInterval() 不断移动盒子位置。
实现步骤：
1 获得盒子当前位置
2 让盒子在当前位置加上1个移动距离
3 利用定时器不断重复这个操作
4 加一个结束定时器的条件
5 注意此元素需要添加定位，才能使用element.style.left
 
如果多个元素都使用这个动画函数，每次都要var 声明定时器。
我们可以给不同的元素使用不同的定时器（自己专门
用自己的定时器）。
核心原理：利用 JS 是一门动态语言，可以很方便的给当前
对象添加属性。
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
           position: absolute;
           left: 0;
           width: 100px;
           height: 100px;
           background-color: pink;
       }
       
       span {
           position: absolute;
           left: 0;
           top: 200px;
           display: block;
           width: 150px;
           height: 150px;
           background-color: purple;
       }
   </style>
</head>

<body>
   <button>点击夏雨荷才走</button>
   <div></div>
   <span>夏雨荷</span>
   <script>
       // var obj = {};
       // obj.name = 'andy';
       // 简单动画函数封装obj目标对象 target 目标位置
       // 给不同的元素指定了不同的定时器
       function animate(obj, target) {
           // 当我们不断的点击按钮，这个元素的速度会越来越快，因为开启了太多的定时器
           // 解决方案就是 让我们元素只有一个定时器执行
           // 先清除以前的定时器，只保留当前的一个定时器执行
           clearInterval(obj.timer);
           obj.timer = setInterval(function() {
               if (obj.offsetLeft >= target) {
                   // 停止动画 本质是停止定时器
                   clearInterval(obj.timer);
               }
               obj.style.left = obj.offsetLeft + 1 + 'px';

           }, 30);
       }

       var div = document.querySelector('div');
       var span = document.querySelector('span');
       var btn = document.querySelector('button');
       // 调用函数
       animate(div, 300);
       btn.addEventListener('click', function() {
           animate(span, 200);
       })
   </script>
</body>

</html>
```
### 1.15 缓动效果原理
 ```markdown
 缓动动画就是让元素运动速度有所变化，最常见的是让速度慢慢停下来
思路：
1  让盒子每次移动的距离慢慢变小，速度就会慢慢落下来。
2  核心算法： (目标值 - 现在的位置 ) / 10 做为每
次移动的距离 步长
3 停止的条件是： 让当前盒子位置等于目标位置就停止定时器
4  注意步长值需要取整 
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
           position: absolute;
           left: 0;
           width: 100px;
           height: 100px;
           background-color: pink;
       }
       
       span {
           position: absolute;
           left: 0;
           top: 200px;
           display: block;
           width: 150px;
           height: 150px;
           background-color: purple;
       }
   </style>
</head>

<body>
   <button class="btn500">点击夏雨荷到500</button>
   <button class="btn800">点击夏雨荷到800</button>
   <span>夏雨荷</span>
   <script>
       // 缓动动画函数封装obj目标对象 target 目标位置
       // 思路：
       // 1. 让盒子每次移动的距离慢慢变小， 速度就会慢慢落下来。
       // 2. 核心算法：(目标值 - 现在的位置) / 10 做为每次移动的距离 步长
       // 3. 停止的条件是： 让当前盒子位置等于目标位置就停止定时器
       function animate(obj, target) {
           // 先清除以前的定时器，只保留当前的一个定时器执行
           clearInterval(obj.timer);
           obj.timer = setInterval(function() {
               // 步长值写到定时器的里面
               // 把我们步长值改为整数 不要出现小数的问题
               // var step = Math.ceil((target - obj.offsetLeft) / 10);
               var step = (target - obj.offsetLeft) / 10;
               step = step > 0 ? Math.ceil(step) : Math.floor(step);
               if (obj.offsetLeft == target) {
                   // 停止动画 本质是停止定时器
                   clearInterval(obj.timer);
               }
               // 把每次加1 这个步长值改为一个慢慢变小的值  步长公式：(目标值 - 现在的位置) / 10
               obj.style.left = obj.offsetLeft + step + 'px';

           }, 15);
       }
       var span = document.querySelector('span');
       var btn500 = document.querySelector('.btn500');
       var btn800 = document.querySelector('.btn800');

       btn500.addEventListener('click', function() {
           // 调用函数
           animate(span, 500);
       })
       btn800.addEventListener('click', function() {
               // 调用函数
               animate(span, 800);
           })
           // 匀速动画 就是 盒子是当前的位置 +  固定的值 10 
           // 缓动动画就是  盒子当前的位置 + 变化的值(目标值 - 现在的位置) / 10）
   </script>
</body>

</html>
```
### 1.16 缓动动画添加回调函数
```markdown
回调函数原理：函数可以作为一个参数。将这个函
数作为参数传到另一个函数里面，当那个函数执行完之后
，再执行传进去的这个函数，这个过程就叫做回调。
回调函数写的位置：定时器结束的位置。
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
            position: absolute;
            left: 0;
            width: 100px;
            height: 100px;
            background-color: pink;
        }
        
        span {
            position: absolute;
            left: 0;
            top: 200px;
            display: block;
            width: 150px;
            height: 150px;
            background-color: purple;
        }
    </style>
</head>

<body>
    <button class="btn500">点击夏雨荷到500</button>
    <button class="btn800">点击夏雨荷到800</button>
    <span>夏雨荷</span>
    <script>
        // 缓动动画函数封装obj目标对象 target 目标位置
        // 思路：
        // 1. 让盒子每次移动的距离慢慢变小， 速度就会慢慢落下来。
        // 2. 核心算法：(目标值 - 现在的位置) / 10 做为每次移动的距离 步长
        // 3. 停止的条件是： 让当前盒子位置等于目标位置就停止定时器
        function animate(obj, target, callback) {
            // console.log(callback);  callback = function() {}  调用的时候 callback()

            // 先清除以前的定时器，只保留当前的一个定时器执行
            clearInterval(obj.timer);
            obj.timer = setInterval(function() {
                // 步长值写到定时器的里面
                // 把我们步长值改为整数 不要出现小数的问题
                // var step = Math.ceil((target - obj.offsetLeft) / 10);
                var step = (target - obj.offsetLeft) / 10;
                step = step > 0 ? Math.ceil(step) : Math.floor(step);
                if (obj.offsetLeft == target) {
                    // 停止动画 本质是停止定时器
                    clearInterval(obj.timer);
                    // 回调函数写到定时器结束里面
                    if (callback) {
                        // 调用函数
                        callback();
                    }
                }
                // 把每次加1 这个步长值改为一个慢慢变小的值  步长公式：(目标值 - 现在的位置) / 10
                obj.style.left = obj.offsetLeft + step + 'px';

            }, 15);
        }
        var span = document.querySelector('span');
        var btn500 = document.querySelector('.btn500');
        var btn800 = document.querySelector('.btn800');

        btn500.addEventListener('click', function() {
            // 调用函数
            animate(span, 500);
        })
        btn800.addEventListener('click', function() {
                // 调用函数
                animate(span, 800, function() {
                    // alert('你好吗');
                    span.style.backgroundColor = 'red';
                });
            })
            // 匀速动画 就是 盒子是当前的位置 +  固定的值 10 
            // 缓动动画就是  盒子当前的位置 + 变化的值(目标值 - 现在的位置) / 10）
    </script>
</body>

</html>
```
### 1.17 网页轮播图
```markdown
轮播图也称为焦点图，是网页中比较常见的网页特效。
功能需求：
1 鼠标经过轮播图模块，左右按钮显示，离开隐藏左右按钮。
2 点击右侧按钮一次，图片往左播放一张，以此类推， 左侧按
钮同理。
3 图片播放的同时，下面小圆圈模块跟随一起变化。
4 点击小圆圈，可以播放相应图片。
5 鼠标不经过轮播图， 轮播图也会自动播放图片。
6 鼠标经过，轮播图模块， 自动播放停止。

1 因为js较多，我们单独新建js文件夹，再新建js文件， 
引入页面中。
2 此时需要添加 load 事件。
3 鼠标经过轮播图模块，左右按钮显示，离开隐藏左右按钮。
4 显示隐藏 display 按钮。

1 动态生成小圆圈
2 核心思路：小圆圈的个数要跟图片张数一致
3 所以首先先得到ul里面图片的张数（图片放入li里面，
所以就是li的个数）
4 利用循环动态生成小圆圈（这个小圆圈要放入ol里面）
5 创建节点 createElement(‘li’)
6 插入节点 ol. appendChild(li)
7 第一个小圆圈需要添加 current 类

1 小圆圈的排他思想
2 点击当前小圆圈，就添加current类
3 其余的小圆圈就移除这个current类
4 注意： 我们在刚才生成小圆圈的同时，就可以直
接绑定这个点击事件了。

1 点击小圆圈滚动图片
2 此时用到animate动画函数，将js文件引入（注意，
因为index.js 依赖 animate.js 所以，animate.js 要
写到 index.js 上面）
3 使用动画函数的前提，该元素必须有定位
4 注意是ul 移动 而不是小li
5 滚动图片的核心算法： 点击某个小圆圈 ， 就让图片滚动 小
圆圈的索引号乘以图片的宽度做为ul移动距离
6 此时需要知道小圆圈的索引号， 我们可以在生成小圆圈
的时候，给它设置一个自定义属性，点击的时候获取这个自定
义属性即可。

1 点击右侧按钮一次，就让图片滚动一张。
2 声明一个变量num， 点击一次，自增1， 让这个变量乘以图
片宽度，就是 ul 的滚动距离。
3 图片无缝滚动原理
4 把ul 第一个li 复制一份，放到ul 的最后面
5 当图片滚动到克隆的最后一张图片时， 让ul 快速的、不做动画
的跳到最左侧： left 为0
6 同时num 赋值为0，可以从新开始滚动图片了

1 克隆第一张图片
2 克隆ul 第一个li cloneNode() 加true 深克隆 复制里
面的子节点 false 浅克隆
3 添加到 ul 最后面 appendChild

1 点击右侧按钮， 小圆圈跟随变化
2 最简单的做法是再声明一个变量circle，每次点击自增1，注意，
左侧按钮也需要这个变量，因此要声明全局变量。
3 但是图片有5张，我们小圆圈只有4个少一个，必须
加一个判断条件
4 如果circle == 4 就 从新复原为 0

1 自动播放功能
2 添加一个定时器
3 自动播放轮播图，实际就类似于点击了右侧按钮
4 此时我们使用手动调用右侧按钮点击事件 arrow_r.click()
5 鼠标经过focus 就停止定时器
6 鼠标离开focus 就开启定时器

节流阀
防止轮播图按钮连续点击造成播放过快。
节流阀目的：当上一个函数动画内容执行完毕，再去执行
下一个函数动画，让事件无法连续触发。
核心实现思路：利用回调函数，添加一个变量来控制，锁
住函数和解锁函数。
开始设置一个变量 var flag = true;
If(flag) {flag = false; do something} 关闭水龙头
利用回调函数 动画执行完毕， flag = true 打开水龙头
```
```javascript
animate.js
function animate(obj, target, callback) {
    // console.log(callback);  callback = function() {}  调用的时候 callback()

    // 先清除以前的定时器，只保留当前的一个定时器执行
    clearInterval(obj.timer);
    obj.timer = setInterval(function() {
        // 步长值写到定时器的里面
        // 把我们步长值改为整数 不要出现小数的问题
        // var step = Math.ceil((target - obj.offsetLeft) / 10);
        var step = (target - obj.offsetLeft) / 10;
        step = step > 0 ? Math.ceil(step) : Math.floor(step);
        if (obj.offsetLeft == target) {
            // 停止动画 本质是停止定时器
            clearInterval(obj.timer);
            // 回调函数写到定时器结束里面
            // if (callback) {
            //     // 调用函数
            //     callback();
            // }
            callback && callback();
        }
        // 把每次加1 这个步长值改为一个慢慢变小的值  步长公式：(目标值 - 现在的位置) / 10
        obj.style.left = obj.offsetLeft + step + 'px';

    }, 15);
}

index.js
window.addEventListener('load', function() {
    // 1. 获取元素
    var arrow_l = document.querySelector('.arrow-l');
    var arrow_r = document.querySelector('.arrow-r');
    var focus = document.querySelector('.focus');
    var focusWidth = focus.offsetWidth;
    // 2. 鼠标经过focus 就显示隐藏左右按钮
    focus.addEventListener('mouseenter', function() {
        arrow_l.style.display = 'block';
        arrow_r.style.display = 'block';
        clearInterval(timer);
        timer = null; // 清除定时器变量
    });
    focus.addEventListener('mouseleave', function() {
        arrow_l.style.display = 'none';
        arrow_r.style.display = 'none';
        timer = setInterval(function() {
            //手动调用点击事件
            arrow_r.click();
        }, 2000);
    });
    // 3. 动态生成小圆圈  有几张图片，我就生成几个小圆圈
    var ul = focus.querySelector('ul');
    var ol = focus.querySelector('.circle');
    // console.log(ul.children.length);
    for (var i = 0; i < ul.children.length; i++) {
        // 创建一个小li 
        var li = document.createElement('li');
        // 记录当前小圆圈的索引号 通过自定义属性来做 
        li.setAttribute('index', i);
        // 把小li插入到ol 里面
        ol.appendChild(li);
        // 4. 小圆圈的排他思想 我们可以直接在生成小圆圈的同时直接绑定点击事件
        li.addEventListener('click', function() {
            // 干掉所有人 把所有的小li 清除 current 类名
            for (var i = 0; i < ol.children.length; i++) {
                ol.children[i].className = '';
            }
            // 留下我自己  当前的小li 设置current 类名
            this.className = 'current';
            // 5. 点击小圆圈，移动图片 当然移动的是 ul 
            // ul 的移动距离 小圆圈的索引号 乘以 图片的宽度 注意是负值
            // 当我们点击了某个小li 就拿到当前小li 的索引号
            var index = this.getAttribute('index');
            // 当我们点击了某个小li 就要把这个li 的索引号给 num  
            num = index;
            // 当我们点击了某个小li 就要把这个li 的索引号给 circle  
            circle = index;
            // num = circle = index;
            console.log(focusWidth);
            console.log(index);

            animate(ul, -index * focusWidth);
        })
    }
    // 把ol里面的第一个小li设置类名为 current
    ol.children[0].className = 'current';
    // 6. 克隆第一张图片(li)放到ul 最后面
    var first = ul.children[0].cloneNode(true);
    ul.appendChild(first);
    // 7. 点击右侧按钮， 图片滚动一张
    var num = 0;
    // circle 控制小圆圈的播放
    var circle = 0;
    // flag 节流阀
    var flag = true;
    arrow_r.addEventListener('click', function() {
        if (flag) {
            flag = false; // 关闭节流阀
            // 如果走到了最后复制的一张图片，此时 我们的ul 要快速复原 left 改为 0
            if (num == ul.children.length - 1) {
                ul.style.left = 0;
                num = 0;
            }
            num++;
            animate(ul, -num * focusWidth, function() {
                flag = true; // 打开节流阀
            });
            // 8. 点击右侧按钮，小圆圈跟随一起变化 可以再声明一个变量控制小圆圈的播放
            circle++;
            // 如果circle == 4 说明走到最后我们克隆的这张图片了 我们就复原
            if (circle == ol.children.length) {
                circle = 0;
            }
            // 调用函数
            circleChange();
        }
    });

    // 9. 左侧按钮做法
    arrow_l.addEventListener('click', function() {
        if (flag) {
            flag = false;
            if (num == 0) {
                num = ul.children.length - 1;
                ul.style.left = -num * focusWidth + 'px';

            }
            num--;
            animate(ul, -num * focusWidth, function() {
                flag = true;
            });
            // 点击左侧按钮，小圆圈跟随一起变化 可以再声明一个变量控制小圆圈的播放
            circle--;
            // 如果circle < 0  说明第一张图片，则小圆圈要改为第4个小圆圈（3）
            // if (circle < 0) {
            //     circle = ol.children.length - 1;
            // }
            circle = circle < 0 ? ol.children.length - 1 : circle;
            // 调用函数
            circleChange();
        }
    });

    function circleChange() {
        // 先清除其余小圆圈的current类名
        for (var i = 0; i < ol.children.length; i++) {
            ol.children[i].className = '';
        }
        // 留下当前的小圆圈的current类名
        ol.children[circle].className = 'current';
    }
    // 10. 自动播放轮播图
    var timer = setInterval(function() {
        //手动调用点击事件
        arrow_r.click();
    }, 2000);

})

index.html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>品优购-综合网购首选-正品低价、品质保障、配送及时、轻松购物！</title>
    <meta name="description" content="品优购JD.COM-专业的综合网上购物商城,销售家电、数码通讯、电脑、家居百货、服装服饰、母婴、图书、食品等数万个品牌优质商品.便捷、诚信的服务，为您提供愉悦的网上购物体验!" />
    <meta name="Keywords" content="网上购物,网上商城,手机,笔记本,电脑,MP3,CD,VCD,DV,相机,数码,配件,手表,存储卡,品优购" />
    <!-- 引入facicon.ico网页图标 -->
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
    <!-- 引入css 初始化的css 文件 -->
    <link rel="stylesheet" href="css/base.css">
    <!-- 引入公共样式的css 文件 -->
    <link rel="stylesheet" href="css/common.css">
    <!-- 引入 首页的css文件 -->
    <link rel="stylesheet" href="css/index.css">
    <!-- 这个animate.js 必须写到 index.js的上面引入 -->
    <script src="js/animate.js"></script>
    <!-- 引入我们首页的js文件 -->
    <script src="js/index.js"></script>
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
    <!-- header制作 -->
    <div class="header w">
        <!-- logo -->
        <div class="logo">
            <h1>
                <a href="index.html" title="品优购">品优购</a>
            </h1>
        </div>
        <!-- search -->
        <div class="search">
            <input type="text" class="text" value="请搜索内容...">
            <button class="btn">搜索</button>
        </div>
        <!-- hotwrods -->
        <div class="hotwrods">
            <a href="#" class="style-red">优惠购首发</a>
            <a href="#">亿元优惠</a>
            <a href="#">9.9元团购</a>
            <a href="#">美满99减30</a>
            <a href="#">办公用品</a>
            <a href="#">电脑</a>
            <a href="#">通信</a>
        </div>
        <div class="shopcar">
            <i class="car"> </i>我的购物车 <i class="arrow">  </i>
            <i class="count">80</i>
        </div>
    </div>
    <!-- header 结束 -->
    <!-- nav start -->
    <div class="nav">
        <div class="w">
            <div class="dropdown fl">
                <div class="dt"> 全部商品分类 </div>
                <div class="dd">
                    <ul>
                        <li class="menu_item"><a href="#">家用电器</a> <i>  </i> </li>
                        <li class="menu_item">
                            <a href="list.html">手机</a> 、
                            <a href="#">数码</a> 、
                            <a href="#">通信</a>
                            <i>  </i>
                        </li>
                        <li class="menu_item"><a href="#">电脑、办公</a> <i>  </i> </li>
                        <li class="menu_item"><a href="#">家居、家具、家装、厨具</a> <i>  </i> </li>
                        <li class="menu_item"><a href="#">男装、女装、童装、内衣</a> <i>  </i> </li>
                        <li class="menu_item"><a href="#">个户化妆、清洁用品、宠物</a> <i>  </i> </li>
                        <li class="menu_item"><a href="#">鞋靴、箱包、珠宝、奢侈品</a> <i>  </i> </li>
                        <li class="menu_item"><a href="#">运动户外、钟表</a> <i>  </i> </li>
                        <li class="menu_item"><a href="#">汽车、汽车用品</a> <i>  </i> </li>
                        <li class="menu_item"><a href="#">母婴、玩具乐器</a> <i>  </i> </li>
                        <li class="menu_item"><a href="#">食品、酒类、生鲜、特产</a> <i>  </i> </li>
                        <li class="menu_item"><a href="#">医药保健</a> <i>  </i> </li>
                        <li class="menu_item"><a href="#">图书、音像、电子书</a> <i> </i> </li>
                        <li class="menu_item"><a href="#">彩票、旅行、充值、票务</a> <i> </i> </li>
                        <li class="menu_item"><a href="#">理财、众筹、白条、保险</a> <i>  </i> </li>
                    </ul>
                </div>
            </div>
            <!-- 右侧导航 -->
            <div class="navitems fl">
                <ul>
                    <li><a href="#">服装城</a></li>
                    <li><a href="#">美妆馆</a></li>
                    <li><a href="#">传智超市</a></li>
                    <li><a href="#">全球购</a></li>
                    <li><a href="#">闪购</a></li>
                    <li><a href="#">团购</a></li>
                    <li><a href="#">拍卖</a></li>
                    <li><a href="#">有趣</a></li>
                </ul>
            </div>
        </div>
    </div>
    <!-- nav end  -->
    <!-- main 模块 -->
    <div class="w">
        <div class="main">
            <div class="focus fl">
                <!-- 左侧按钮 -->
                <a href="javascript:;" class="arrow-l">
                    &lt;
                 </a>
                <!-- 右侧按钮 -->
                <a href="javascript:;" class="arrow-r">  </a>
                <!-- 核心的滚动区域 -->
                <ul>
                    <li>
                        <a href="#"><img src="upload/focus.jpg" alt=""></a>
                    </li>
                    <li>
                        <a href="#"><img src="upload/focus1.jpg" alt=""></a>
                    </li>
                    <li>
                        <a href="#"><img src="upload/focus2.jpg" alt=""></a>
                    </li>
                    <li>
                        <a href="#"><img src="upload/focus3.jpg" alt=""></a>
                    </li>
                </ul>
                <!-- 小圆圈 -->
                <ol class="circle">

                </ol>
            </div>
            <div class="newsflash fr">
                <div class="news">
                    <div class="news-hd">
                        品优购快报
                        <a href="#">更多</a>
                    </div>
                    <div class="news-bd">
                        <ul>
                            <li><a href="#">【特惠】爆款耳机5折秒！</a></li>
                            <li><a href="#">【特惠】母亲节，健康好礼低至5折！</a></li>
                            <li><a href="#">【特惠】爆款耳机5折秒！</a></li>
                            <li><a href="#">【特惠】9.9元洗100张照片！</a></li>
                            <li><a href="#">【特惠】长虹智能空调立省1000</a></li>
                        </ul>
                    </div>
                </div>
                <div class="lifeservice">
                    <ul>
                        <li>
                            <a href="#">
                                <i class="service_ico service_ico_huafei"></i>
                                <p>话费</p>
                            </a>
                        </li>
                        <li>
                            <a href="#">
                                <i class="service_ico service_ico_feiji"></i>
                                <p>机票</p>
                            </a>
                            <span class="hot"></span>
                        </li>
                        <li>
                            <a href="#">
                                <i class="service_ico service_ico_feiji"></i>
                                <p>机票</p>
                            </a>
                        </li>
                        <li>
                            <a href="#">
                                <i class="service_ico service_ico_feiji"></i>
                                <p>机票</p>
                            </a>
                        </li>
                        <li>
                            <a href="#">
                                <i class="service_ico service_ico_feiji"></i>
                                <p>机票</p>
                            </a>
                        </li>
                        <li>
                            <a href="#">
                                <i class="service_ico service_ico_feiji"></i>
                                <p>机票</p>
                            </a>
                        </li>
                        <li>
                            <a href="#">
                                <i class="service_ico service_ico_feiji"></i>
                                <p>机票</p>
                            </a>
                        </li>
                        <li>
                            <a href="#">
                                <i class="service_ico service_ico_feiji"></i>
                                <p>机票</p>
                            </a>
                        </li>
                        <li>
                            <a href="#">
                                <i class="service_ico service_ico_feiji"></i>
                                <p>机票</p>
                            </a>
                        </li>
                        <li>
                            <a href="#">
                                <i class="service_ico service_ico_feiji"></i>
                                <p>机票</p>
                            </a>
                        </li>
                        <li>
                            <a href="#">
                                <i class="service_ico service_ico_feiji"></i>
                                <p>机票</p>
                            </a>
                        </li>
                        <li>
                            <a href="#">
                                <i class="service_ico service_ico_feiji"></i>
                                <p>机票</p>
                            </a>
                        </li>
                    </ul>
                </div>
                <div class="bargain">
                    <img src="upload/bargain.jpg" alt="">
                </div>
            </div>
        </div>
    </div>

    <!-- 推荐服务模块 start -->
    <div class="recommend w">
        <div class="recom-hd fl">
            <img src="img/clock.png" alt="">
            <h3>今日推荐</h3>
        </div>
        <div class="recom-bd fl">
            <ul>
                <li>
                    <a href="#">
                        <img src="upload/pic.jpg" alt="">
                    </a>
                </li>
                <li>
                    <a href="#">
                        <img src="upload/pic.jpg" alt="">
                    </a>
                </li>
                <li>
                    <a href="#">
                        <img src="upload/pic.jpg" alt="">
                    </a>
                </li>
                <li class="last">
                    <a href="#">
                        <img src="upload/pic.jpg" alt="">
                    </a>
                </li>

            </ul>
        </div>
    </div>
    <!-- 推荐服务模块 end -->

    <!-- 楼层区 start -->
    <div class="floor">
        <div class="jiadian w">
            <div class="box-hd">
                <h3>家用电器</h3>
                <div class="tab-list">
                    <ul>
                        <li><a href="#" class="style-red">热门</a>|</li>
                        <li><a href="#">大家电</a>|</li>
                        <li><a href="#">生活电器</a>|</li>
                        <li><a href="#">厨房电器</a>|</li>
                        <li><a href="#">个护健康</a>|</li>
                        <li><a href="#">应季电器</a>|</li>
                        <li><a href="#">空气/净水</a>|</li>
                        <li><a href="#">新奇特</a>|</li>
                        <li><a href="#">高端电器</a></li>
                    </ul>
                </div>
            </div>
            <div class="box-bd">
                <ul class="tab-con">
                    <li class="w209">
                        <ul class="tab-con-list">
                            <li>
                                <a href="#">节能补贴</a>
                            </li>
                            <li>
                                <a href="#">4K电视</a>
                            </li>
                            <li>
                                <a href="#">空气净化器</a>
                            </li>
                            <li>
                                <a href="#">IH电饭煲</a>
                            </li>
                            <li>
                                <a href="#">滚筒洗衣机</a>
                            </li>
                            <li>
                                <a href="#">电热水器</a>
                            </li>
                        </ul>
                        <img src="upload/floor-1-1.png" alt="">
                    </li>
                    <li class="w329">
                        <img src="upload/pic1.jpg" alt="">
                    </li>
                    <li class="w219">
                        <div class="tab-con-item">
                            <a href="#">
                                <img src="upload/floor-1-2.png" alt="">
                            </a>
                        </div>
                        <div class="tab-con-item">
                            <a href="#">
                                <img src="upload/floor-1-3.png" alt="">
                            </a>
                        </div>
                    </li>
                    <li class="w220">
                        <div class="tab-con-item">
                            <a href="#">
                                <img src="upload/floor-1-4.png" alt="">
                            </a>
                        </div>
                    </li>
                    <li class="w220">
                        <div class="tab-con-item">
                            <a href="#">
                                <img src="upload/floor-1-5.png" alt="">
                            </a>
                        </div>
                        <div class="tab-con-item">
                            <a href="#">
                                <img src="upload/floor-1-6.png" alt="">
                            </a>
                        </div>
                    </li>
                </ul>
                <!-- <ul class="tab-con">
					<li>1</li>
					<li>2</li>
					<li>3</li>
					<li>4</li>
					<li>5</li>
				</ul> -->
            </div>
        </div>
        <div class="shouji w">
            <div class="box-hd">
                <h3>手机通讯</h3>
                <div class="tab-list">
                    <ul>
                        <li><a href="#" class="style-red">热门</a>|</li>
                        <li><a href="#">大家电</a>|</li>
                        <li><a href="#">生活电器</a>|</li>
                        <li><a href="#">厨房电器</a>|</li>
                        <li><a href="#">个护健康</a>|</li>
                        <li><a href="#">应季电器</a>|</li>
                        <li><a href="#">空气/净水</a>|</li>
                        <li><a href="#">新奇特</a>|</li>
                        <li><a href="#">高端电器</a></li>
                    </ul>
                </div>
            </div>
            <div class="box-bd">
                <ul class="tab-con">
                    <li class="w209">
                        <ul class="tab-con-list">
                            <li>
                                <a href="#">节能补贴</a>
                            </li>
                            <li>
                                <a href="#">4K电视</a>
                            </li>
                            <li>
                                <a href="#">空气净化器</a>
                            </li>
                            <li>
                                <a href="#">IH电饭煲</a>
                            </li>
                            <li>
                                <a href="#">滚筒洗衣机</a>
                            </li>
                            <li>
                                <a href="#">电热水器</a>
                            </li>
                        </ul>
                        <img src="upload/floor-1-1.png" alt="">
                    </li>
                    <li class="w329">
                        <img src="upload/pic1.jpg" alt="">
                    </li>
                    <li class="w219">
                        <div class="tab-con-item">
                            <a href="#">
                                <img src="upload/floor-1-2.png" alt="">
                            </a>
                        </div>
                        <div class="tab-con-item">
                            <a href="#">
                                <img src="upload/floor-1-3.png" alt="">
                            </a>
                        </div>
                    </li>
                    <li class="w220">
                        <div class="tab-con-item">
                            <a href="#">
                                <img src="upload/floor-1-4.png" alt="">
                            </a>
                        </div>
                    </li>
                    <li class="w220">
                        <div class="tab-con-item">
                            <a href="#">
                                <img src="upload/floor-1-5.png" alt="">
                            </a>
                        </div>
                        <div class="tab-con-item">
                            <a href="#">
                                <img src="upload/floor-1-6.png" alt="">
                            </a>
                        </div>
                    </li>
                </ul>
                <!-- <ul class="tab-con">
					<li>1</li>
					<li>2</li>
					<li>3</li>
					<li>4</li>
					<li>5</li>
				</ul> -->
            </div>
        </div>
    </div>
    <!-- 楼层区 end -->
    <!-- 固定电梯导航 -->
    <div class="fixedtool">
        <ul>
            <li class="current">家用电器</li>
            <li>手机通讯</li>
            <li>家用电器</li>
            <li>家用电器</li>
            <li>家用电器</li>
            <li>家用电器</li>
        </ul>
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
```

## 2 移动端网页特效
### 2.1  触屏事件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513212906195.png)
```html
触摸事件对象重点我们看三个常见对象列表
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513213203916.png)
```markdown
因为平时我们都是给元素注册触摸事件，所以
重点记住 targetTouches
```

### 2.2 classList 属性
```markdown
添加类：element.classList.add（’类名’）；
focus.classList.add(‘current’);
移除类：element.classList.remove（’类名’）;
focus.classList.remove(‘current’);
切换类：
element.classList.toggle（’类名’）；
focus.classList.toggle(‘current’);
注意以上方法里面，所有类名都不带点
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
        .bg {
            background-color: black;
        }
    </style>
</head>

<body>
    <div class="one two"></div>
    <button> 开关灯</button>
    <script>
        // classList 返回元素的类名
        var div = document.querySelector('div');
        // console.log(div.classList[1]);
        // 1. 添加类名  是在后面追加类名不会覆盖以前的类名 注意前面不需要加.
        div.classList.add('three');
        // 2. 删除类名
        div.classList.remove('one');
        // 3. 切换类
        var btn = document.querySelector('button');
        btn.addEventListener('click', function() {
            document.body.classList.toggle('bg');
        })
    </script>
</body>

</html>
```
### 2.3 移动端轮播图
```markdown
移动端轮播图功能和基本PC端一致。
1 可以自动播放图片
2 手指可以拖动播放轮播图
	1 自动播放功能
	2 开启定时器
	3 移动端移动，可以使用translate 移动
	4 想要图片优雅的移动，请添加过渡效果

1 自动播放功能-无缝滚动
2 注意，我们判断条件是要等到图片滚动完毕再去判断，
就是过渡完成后判断
3 此时需要添加检测过渡完成事件 transitionend
4 判断条件： 如果索引号等于 3 说明走到最后一张图片，
此时 索引号要复原为 0
5 此时图片，去掉过渡效果，然后移动
6 如果索引号小于0， 说明是倒着走， 索引号等于2
7 此时图片，去掉过渡效果，然后移动

1 小圆点跟随变化效果
2 把ol里面li带有current类名的选出来去掉类名 remove
3 让当前索引号 的小li 加上 current add
4 但是，是等着过渡结束之后变化，所以这个写
到 transitionend 事件里面

1 手指滑动轮播图
2 本质就是ul跟随手指移动，简单说就是移动端拖动元素
3 触摸元素 touchstart： 获取手指初始坐标
4 移动手指 touchmove： 计算手指的滑动距离，并且移动盒子
5 离开手指 touchend: 根据滑动的距离分不同的情况
6 如果移动距离小于 某个像素 就回弹原来位置
7 如果移动距离大于某个像素就上一张下一张滑动。
8 滑动也分为左滑动和右滑动 判断的标准是 移动距离正负 
如果是负值就是左滑 反之右滑
9 如果是左滑 就播放下一张 （index++）
10 如果是右滑 就播放上一张 (index--)

当页面滚动某个地方，就显示，否则隐藏
点击可以返回顶部

1 滚动某个地方显示
2 事件： scroll 页面滚动事件
3 如果被卷去的头部（window.pageYOffset ）大于某个数值
4 点击， window.scroll(0,0) 返回顶部
```
```javascript
index,js
window.addEventListener('load', function() {
    // alert(1);
    // 1. 获取元素 
    var focus = document.querySelector('.focus');
    var ul = focus.children[0];
    // 获得focus 的宽度
    var w = focus.offsetWidth;
    var ol = focus.children[1];
    // 2. 利用定时器自动轮播图图片
    var index = 0;
    var timer = setInterval(function() {
        index++;
        var translatex = -index * w;
        ul.style.transition = 'all .3s';
        ul.style.transform = 'translateX(' + translatex + 'px)';
    }, 2000);
    // 等着我们过渡完成之后，再去判断 监听过渡完成的事件 transitionend 
    ul.addEventListener('transitionend', function() {
        // 无缝滚动
        if (index >= 3) {
            index = 0;
            // console.log(index);
            // 去掉过渡效果 这样让我们的ul 快速的跳到目标位置
            ul.style.transition = 'none';
            // 利用最新的索引号乘以宽度 去滚动图片
            var translatex = -index * w;
            ul.style.transform = 'translateX(' + translatex + 'px)';
        } else if (index < 0) {
            index = 2;
            ul.style.transition = 'none';
            // 利用最新的索引号乘以宽度 去滚动图片
            var translatex = -index * w;
            ul.style.transform = 'translateX(' + translatex + 'px)';
        }
        // 3. 小圆点跟随变化
        // 把ol里面li带有current类名的选出来去掉类名 remove
        ol.querySelector('.current').classList.remove('current');
        // 让当前索引号 的小li 加上 current   add
        ol.children[index].classList.add('current');
    });

    // 4. 手指滑动轮播图 
    // 触摸元素 touchstart： 获取手指初始坐标
    var startX = 0;
    var moveX = 0; // 后面我们会使用这个移动距离所以要定义一个全局变量
    var flag = false;
    ul.addEventListener('touchstart', function(e) {
        startX = e.targetTouches[0].pageX;
        // 手指触摸的时候就停止定时器
        clearInterval(timer);
    });
    // 移动手指 touchmove： 计算手指的滑动距离， 并且移动盒子
    ul.addEventListener('touchmove', function(e) {
        // 计算移动距离
        moveX = e.targetTouches[0].pageX - startX;
        // 移动盒子：  盒子原来的位置 + 手指移动的距离 
        var translatex = -index * w + moveX;
        // 手指拖动的时候，不需要动画效果所以要取消过渡效果
        ul.style.transition = 'none';
        ul.style.transform = 'translateX(' + translatex + 'px)';
        flag = true; // 如果用户手指移动过我们再去判断否则不做判断效果
        e.preventDefault(); // 阻止滚动屏幕的行为
    });
    // 手指离开 根据移动距离去判断是回弹还是播放上一张下一张
    ul.addEventListener('touchend', function(e) {
        if (flag) {
            // (1) 如果移动距离大于50像素我们就播放上一张或者下一张
            if (Math.abs(moveX) > 50) {
                // 如果是右滑就是 播放上一张 moveX 是正值
                if (moveX > 0) {
                    index--;
                } else {
                    // 如果是左滑就是 播放下一张 moveX 是负值
                    index++;
                }
                var translatex = -index * w;
                ul.style.transition = 'all .3s';
                ul.style.transform = 'translateX(' + translatex + 'px)';
            } else {
                // (2) 如果移动距离小于50像素我们就回弹
                var translatex = -index * w;
                ul.style.transition = 'all .1s';
                ul.style.transform = 'translateX(' + translatex + 'px)';
            }
        }
        // 手指离开的时候就重新开启定时器
        clearInterval(timer);
        timer = setInterval(function() {
            index++;
            var translatex = -index * w;
            ul.style.transition = 'all .3s';
            ul.style.transform = 'translateX(' + translatex + 'px)';
        }, 2000);
    });


    // 返回顶部模块制作
    var goBack = document.querySelector('.goBack');
    var nav = document.querySelector('nav');
    window.addEventListener('scroll', function() {
        if (window.pageYOffset >= nav.offsetTop) {
            goBack.style.display = 'block';
        } else {
            goBack.style.display = 'none';
        }
    });
    goBack.addEventListener('click', function() {
        window.scroll(0, 0);
    })
})

index.html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0,user-scalable=no">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <link rel="stylesheet" href="css/normalize.css">
    <link rel="stylesheet" href="css/index.css">
    <title>携程在手，说走就走</title>
    <script src="js/index.js"></script>
</head>

<body>
    <!-- 返回顶部模块 -->
    <div class="goBack"></div>
    <!-- 顶部搜索 -->
    <div class="search-index">
        <div class="search">搜索:目的地/酒店/景点/航班号</div>
        <a href="#" class="user">我 的</a>
    </div>
    <!-- 焦点图模块 -->
    <div class="focus">
        <ul>
            <li><img src="upload/focus3.jpg" alt=""></li>
            <li><img src="upload/focus1.jpg" alt=""></li>
            <li><img src="upload/focus2.jpg" alt=""></li>
            <li><img src="upload/focus3.jpg" alt=""></li>
            <li><img src="upload/focus1.jpg" alt=""></li>
        </ul>
        <!-- 小圆点 -->
        <ol>
            <li class="current"></li>
            <li></li>
            <li></li>
        </ol>
    </div>

    <!-- 局部导航栏 -->
    <ul class="local-nav">
        <li>
            <a href="#" title="景点·玩乐">
                <span class="local-nav-icon-icon1"></span>
                <span>景点·玩乐</span>
            </a>
        </li>
        <li>
            <a href="#" title="景点·玩乐">
                <span class="local-nav-icon-icon2"></span>
                <span>景点·玩乐</span>
            </a>
        </li>
        <li>
            <a href="#" title="景点·玩乐">
                <span class="local-nav-icon-icon3"></span>
                <span>景点·玩乐</span>
            </a>
        </li>
        <li>
            <a href="#" title="景点·玩乐">
                <span class="local-nav-icon-icon4"></span>
                <span>景点·玩乐</span>
            </a>
        </li>
        <li>
            <a href="#" title="景点·玩乐">
                <span class="local-nav-icon-icon5"></span>
                <span>景点·玩乐</span>
            </a>
        </li>

    </ul>

    <!-- 主导航栏 -->
    <nav>
        <div class="nav-common">
            <div class="nav-items">
                <a href="#">海外酒店</a>
            </div>
            <div class="nav-items">
                <a href="#">海外酒店</a>
                <a href="#">特价酒店</a>
            </div>
            <div class="nav-items">
                <a href="#">海外酒店</a>
                <a href="#">特价酒店</a>
            </div>
        </div>
        <div class="nav-common">
            <div class="nav-items">
                <a href="#">海外酒店</a>
            </div>
            <div class="nav-items">
                <a href="#">海外酒店</a>
                <a href="#">特价酒店</a>
            </div>
            <div class="nav-items">
                <a href="#">海外酒店</a>
                <a href="#">特价酒店</a>
            </div>
        </div>
        <div class="nav-common">
            <div class="nav-items">
                <a href="#">海外酒店</a>
            </div>
            <div class="nav-items">
                <a href="#">海外酒店</a>
                <a href="#">特价酒店</a>
            </div>
            <div class="nav-items">
                <a href="#">海外酒店</a>
                <a href="#">特价酒店</a>
            </div>
        </div>

    </nav>
    <!-- 侧导航栏 -->
    <ul class="subnav-entry">
        <li>
            <a href="#">
                <span class="subnav-entry-icon"></span>
                <span>电话费</span>
            </a>
        </li>
        <li>
            <a href="#">
                <span class="subnav-entry-icon"></span>
                <span>电话费</span>
            </a>
        </li>
        <li>
            <a href="#">
                <span class="subnav-entry-icon"></span>
                <span>电话费</span>
            </a>
        </li>
        <li>
            <a href="#">
                <span class="subnav-entry-icon"></span>
                <span>电话费</span>
            </a>
        </li>
        <li>
            <a href="#">
                <span class="subnav-entry-icon"></span>
                <span>电话费</span>
            </a>
        </li>
        <li>
            <a href="#">
                <span class="subnav-entry-icon"></span>
                <span>电话费</span>
            </a>
        </li>
        <li>
            <a href="#">
                <span class="subnav-entry-icon"></span>
                <span>电话费</span>
            </a>
        </li>
        <li>
            <a href="#">
                <span class="subnav-entry-icon"></span>
                <span>电话费</span>
            </a>
        </li>
        <li>
            <a href="#">
                <span class="subnav-entry-icon"></span>
                <span>电话费</span>
            </a>
        </li>
        <li>
            <a href="#">
                <span class="subnav-entry-icon"></span>
                <span>电话费</span>
            </a>
        </li>

    </ul>

    <!-- 销售模块 -->
    <div class="sales-box">
        <div class="sales-hd">
            <h2>热门活动</h2>
            <a href="#" class="more">获取更多福利</a>
        </div>
        <div class="sales-bd">
            <div class="row">
                <a href="#">
                    <img src="upload/pic1.jpg" alt="">
                </a>
                <a href="#">
                    <img src="upload/pic2.jpg" alt="">

                </a>
            </div>
            <div class="row">
                <a href="#">
                    <img src="upload/pic3.jpg" alt="">
                </a>
                <a href="#">
                    <img src="upload/pic4.jpg" alt="">

                </a>
            </div>
            <div class="row">
                <a href="#">
                    <img src="upload/pic5.jpg" alt="">
                </a>
                <a href="#">
                    <img src="upload/pic6.jpg" alt="">

                </a>
            </div>

        </div>
    </div>
</body>

</html>
```
### 2.4 click 延时解决方案
```markdown
移动端 click 事件会有 300ms 的延时，原因是移动端屏幕
双击会缩放(double tap to zoom) 页面。
解决方案：
1. 禁用缩放。 浏览器禁用默认的双击缩放行为并
且去掉 300ms 的点击延迟。
 <meta name="viewport" content="user-scalable=no">
2. 利用touch事件自己封装这个事件解决 300ms 延迟。
原理就是：
3. 当我们手指触摸屏幕，记录当前触摸时间
4. 当我们手指离开屏幕， 用离开的时间减去触摸的时间
5. 如果时间小于150ms，并且没有滑动过屏幕， 那么我们
就定义为点击
//封装tap，解决click 300ms 延时
function tap (obj, callback) {
 var isMove = false;
 var startTime = 0; // 记录触摸时候的时间变量
 obj.addEventListener('touchstart', function (e) {
 startTime = Date.now(); // 记录触摸时间
 });
 obj.addEventListener('touchmove', function (e) {
 isMove = true; // 看看是否有滑动，有滑动算拖拽，不算点击
 });
 obj.addEventListener('touchend', function (e) {
 if (!isMove && (Date.now() - startTime) < 150) { // 如果手指触摸和离开时间小于150ms 算点击
 callback && callback(); // 执行回调函数
 }
 isMove = false; // 取反 重置
 startTime = 0;
 });
}
//调用
 tap(div, function(){ // 执行代码 });
6. 使用插件。 fastclick 插件解决 300ms 延迟。
```

### 2.5 本地存储
```markdown
本地存储特性
1 数据存储在用户浏览器中
2 设置、读取方便、甚至页面刷新不丢失数据
3 容量较大，sessionStorage约5M、localStorage约20M
4 只能存储字符串，可以将对象JSON.stringify() 编码后存储

window.sessionStorage
1 生命周期为关闭浏览器窗口
2 在同一个窗口(页面)下数据可以共享
3 以键值对的形式存储使用

window.sessionStorage
存储数据：
sessionStorage.setItem(key, value)
获取数据：
sessionStorage.getItem(key)
删除数据：
sessionStorage.removeItem(key)
删除所有数据：
sessionStorage.clear()

window.localStorage
1 声明周期永久生效，除非手动删除 否则关闭页面也会存在
2 可以多窗口（页面）共享（同一浏览器可以共享）
3 以键值对的形式存储使用
存储数据：
localStorage.setItem(key, value)
获取数据：
localStorage.getItem(key)
删除数据：
localStorage.clear()
```
```html
本地存储之sessionStorage
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <input type="text">
    <button class="set">存储数据</button>
    <button class="get">获取数据</button>
    <button class="remove">删除数据</button>
    <button class="del">清空所有数据</button>
    <script>
        console.log(localStorage.getItem('username'));

        var ipt = document.querySelector('input');
        var set = document.querySelector('.set');
        var get = document.querySelector('.get');
        var remove = document.querySelector('.remove');
        var del = document.querySelector('.del');
        set.addEventListener('click', function() {
            // 当我们点击了之后，就可以把表单里面的值存储起来
            var val = ipt.value;
            sessionStorage.setItem('uname', val);
            sessionStorage.setItem('pwd', val);
        });
        get.addEventListener('click', function() {
            // 当我们点击了之后，就可以把表单里面的值获取过来
            console.log(sessionStorage.getItem('uname'));

        });
        remove.addEventListener('click', function() {
            // 
            sessionStorage.removeItem('uname');

        });
        del.addEventListener('click', function() {
            // 当我们点击了之后，清除所有的
            sessionStorage.clear();

        });
    </script>
</body>

</html>

本地存储之localStorage
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <input type="text">
    <button class="set">存储数据</button>
    <button class="get">获取数据</button>
    <button class="remove">删除数据</button>
    <button class="del">清空所有数据</button>
    <script>
        var ipt = document.querySelector('input');
        var set = document.querySelector('.set');
        var get = document.querySelector('.get');
        var remove = document.querySelector('.remove');
        var del = document.querySelector('.del');
        set.addEventListener('click', function() {
            var val = ipt.value;
            localStorage.setItem('username', val);
        })
        get.addEventListener('click', function() {
            console.log(localStorage.getItem('username'));

        })
        remove.addEventListener('click', function() {
            localStorage.removeItem('username');

        })
        del.addEventListener('click', function() {
            localStorage.clear();

        })
    </script>
</body>

</html>
```
### 2.6 记住用户名
```markdown
如果勾选记住用户名， 下次用户打开浏览器，就在文本框里面
自动显示上次登录的用户名
1 把数据存起来，用到本地存储
2 关闭页面，也可以显示用户名，所以用到localStorage
3 打开页面，先判断是否有这个用户名，如果有，就在表单里面
显示用户名，并且勾选复选框
4 当复选框发生改变的时候 change事件
5 如果勾选，就存储，否则就移除
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
    <input type="text" id="username"> <input type="checkbox" name="" id="remember"> 记住用户名
    <script>
        var username = document.querySelector('#username');
        var remember = document.querySelector('#remember');
        if (localStorage.getItem('username')) {
            username.value = localStorage.getItem('username');
            remember.checked = true;
        }
        remember.addEventListener('change', function() {
            if (this.checked) {
                localStorage.setItem('username', username.value)
            } else {
                localStorage.removeItem('username');
            }
        })
    </script>
</body>
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复w4
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
