# JavaScript高级篇
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714142259403.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1.1 面向过程和对象
```markdown
面向过程编程POP(Process-oriented programming)
面向过程就是分析出解决问题所需要的步骤，然后用函
数把这些步骤一步一步实现，使用的时候再一个一个的依
次调用就可以了。
面向过程，就是按照我们分析好了的步骤，按照步骤解决问题。
优点：性能比面向对象高，适合跟硬件联系很紧密
的东西，例如单片机就采用的面向过程编程。
缺点：没有面向对象易维护、易复用、易扩展

面向对象
面向对象编程 OOP (Object Oriented Programming)
面向对象是把事务分解成为一个个对象，然后由对象之间分工与合作。
面向对象是以对象功能来划分问题，而不是步骤。
优点：易维护、易复用、易扩展，由于面向对象有
封装、继承、多态性的特性，可以设计出低耦合的
系统，使系统 更加灵活、更加易于维护
缺点：性能比面向过程低
```
## 1.2 ES6中的类和对象
```markdown
对象是由属性和方法组成的：
属性：事物的特征，在对象中用属性来表示（常用名词）
方法：事物的行为，在对象中用方法来表示（常用动词）
```
### 1.2.1 创建类和对象
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
        // 1. 创建类 class  创建一个 明星类
        class Star {
            constructor(uname, age) {
                this.uname = uname;
                this.age = age;
            }
        }

        // 2. 利用类创建对象 new
        var ldh = new Star('刘德华', 18);
        var zxy = new Star('张学友', 20);
        console.log(ldh);
        console.log(zxy);
        //(1) 通过class 关键字创建类, 类名我们还是习惯性定义首字母大写
        //(2) 类里面有个constructor 函数,可以接受传递过来的参数,同时返回实例对象
        //(3) constructor 函数 只要 new 生成实例时,就会自动调用这个函数, 如果我们不写这个函数,类也会自动生成这个函数
        //(4) 生成实例 new 不能省略
        //(5) 最后注意语法规范, 创建类 类名后面不要加小括号,生成实例 类名后面加小括号, 构造函数不需要加function
    </script>
</body>

</html>
```

### 1.2.2 类中添加方法
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
        // 1. 创建类 class  创建一个 明星类
        class Star {
            // 类的共有属性放到 constructor 里面
            constructor(uname, age) {
                this.uname = uname;
                this.age = age;
            }
            sing(song) {
                // console.log('我唱歌');
                console.log(this.uname + song);

            }
        }

        // 2. 利用类创建对象 new
        var ldh = new Star('刘德华', 18);
        var zxy = new Star('张学友', 20);
        console.log(ldh);
        console.log(zxy);
        // (1) 我们类里面所有的函数不需要写function 
        //(2) 多个函数方法之间不需要添加逗号分隔
        ldh.sing('冰雨');
        zxy.sing('李香兰');
    </script>
</body>

</html>
```
### 1.2.3 类的继承
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
        // 1. 类的继承
        // class Father {
        //     constructor() {

        //     }
        //     money() {
        //         console.log(100);

        //     }
        // }
        // class Son extends Father {

        // }
        // var son = new Son();
        // son.money();
        class Father {
            constructor(x, y) {
                this.x = x;
                this.y = y;
            }
            sum() {
                console.log(this.x + this.y);

            }
        }
        class Son extends Father {
            constructor(x, y) {
                super(x, y); //调用了父类中的构造函数
            }
        }
        var son = new Son(1, 2);
        var son1 = new Son(11, 22);
        son.sum();
        son1.sum();
    </script>
</body>

</html>
```

### 1.2.4 super关键字调用父类普通函数
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
        // super 关键字调用父类普通函数
        class Father {
            say() {
                return '我是爸爸';
            }
        }
        class Son extends Father {
            say() {
                // console.log('我是儿子');
                console.log(super.say() + '的儿子');
                // super.say() 就是调用父类中的普通函数 say()
            }
        }
        var son = new Son();
        son.say();
        // 继承中的属性或者方法查找原则: 就近原则
        // 1. 继承中,如果实例化子类输出一个方法,先看子类有没有这个方法,如果有就先执行子类的
        // 2. 继承中,如果子类里面没有,就去查找父类有没有这个方法,如果有,就执行父类的这个方法(就近原则)
    </script>
</body>

</html>
```
### 1.2.5 子类继承父类方法同时扩展自己方法
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
        // 父类有加法方法
        class Father {
            constructor(x, y) {
                this.x = x;
                this.y = y;
            }
            sum() {
                console.log(this.x + this.y);
            }
        }
        // 子类继承父类加法方法 同时 扩展减法方法
        class Son extends Father {
            constructor(x, y) {
                // 利用super 调用父类的构造函数
                // super 必须在子类this之前调用
                super(x, y);
                this.x = x;
                this.y = y;

            }
            subtract() {
                console.log(this.x - this.y);

            }
        }
        var son = new Son(5, 3);
        son.subtract();
        son.sum();
    </script>
</body>

</html>
```
### 1.2.6 使用类注意事项
```markdown
1 在 ES6 中类没有变量提升，所以必须先定义类，才
能通过类实例化对象.
2 类里面的共有属性和方法一定要加this使用.
3 类里面的this指向问题.
4 constructor 里面的this指向实例对象, 方法里面
的this 指向这个方法的调用者
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
    <button>点击</button>
    <script>
        var that;
        var _that;
        class Star {
            constructor(uname, age) {
                // constructor 里面的this 指向的是 创建的实例对象
                that = this;
                console.log(this);

                this.uname = uname;
                this.age = age;
                // this.sing();
                this.btn = document.querySelector('button');
                this.btn.onclick = this.sing;
            }
            sing() {
                // 这个sing方法里面的this 指向的是 btn 这个按钮,因为这个按钮调用了这个函数
                console.log(this);

                console.log(that.uname); // that里面存储的是constructor里面的this
            }
            dance() {
                // 这个dance里面的this 指向的是实例对象 ldh 因为ldh 调用了这个函数
                _that = this;
                console.log(this);

            }
        }

        var ldh = new Star('刘德华');
        console.log(that === ldh);
        ldh.dance();
        console.log(_that === ldh);

        // 1. 在 ES6 中类没有变量提升，所以必须先定义类，才能通过类实例化对象

        // 2. 类里面的共有的属性和方法一定要加this使用.
    </script>
</body>

</html
```
### 1.2.7 面向对象版 tab 栏切换
```markdown
功能需求:
1 点击 tab栏,可以切换效果.
2 点击 + 号, 可以添加 tab 项和内容项.
3 点击 x 号, 可以删除当前的tab项和内容项.
4 双击tab项文字或者内容项文字,可以修改里面的文字内容.

抽象对象: Tab 对象
1 该对象具有切换功能
2 该对象具有添加功能
3 该对象具有删除功能
4 该对象具有修改功能

1 点击 + 可以实现添加新的选项卡和内容
2 第一步: 创建新的选项卡li 和 新的 内容 section
3 第二步: 把创建的两个元素追加到对应的父元素中.
4 以前的做法: 动态创建元素 createElement , 但是元素
里面内容较多, 需要innerHTML赋值,在 appendChild
追加到父元素里面.
5 现在高级做法: 利用 insertAdjacentHTML() 可以
直接把字符串格式元素添加到父元素中
6 appendChild 不支持追加字符串的子元素,
insertAdjacentHTML 支持追加字符串的元素
7 insertAdjacentHTML(追加的位置,‘要追加的字符串元素’)
8 追加的位置有: beforeend 插入元素内部的最后一个子节点之后
9 该方法地址: https://developer.mozilla.org/zh-CN/docs/Web/API/Element/insertAdjacentHTML 

1 点击 × 可以删除当前的li选项卡和当前的section
2 X是没有索引号的, 但是它的父亲li 有索引号, 这个索引
号正是我们想要的索引号
3 所以核心思路是: 点击 x 号可以删除这个索引号对
应的 li 和 section
4 但是,当我们动态删除新的li和索引号时,也需要重新获
取 x 这个元素. 需要调用init 方法

1 双击选项卡li或者 section里面的文字,可以实现修改功能
2 双击事件是: ondblclick
3 如果双击文字,会默认选定文字,此时需要双击禁止选中文字
4 window.getSelection ? window.getSelection().removeAllRanges() : document.selection.empty();
5 核心思路: 双击文字的时候, 在 里面生成一个文本框, 当失去
焦点或者按下回车然后把文本框输入的值给原先元素即可.
```

```js
tab.js
var that;
class Tab {
    constructor(id) {
        // 获取元素
        that = this;
        this.main = document.querySelector(id);
        this.add = this.main.querySelector('.tabadd');
        // li的父元素
        this.ul = this.main.querySelector('.fisrstnav ul:first-child');
        // section 父元素
        this.fsection = this.main.querySelector('.tabscon');
        this.init();
    }
    init() {
            this.updateNode();
            // init 初始化操作让相关的元素绑定事件
            this.add.onclick = this.addTab;
            for (var i = 0; i < this.lis.length; i++) {
                this.lis[i].index = i;
                this.lis[i].onclick = this.toggleTab;
                this.remove[i].onclick = this.removeTab;
                this.spans[i].ondblclick = this.editTab;
                this.sections[i].ondblclick = this.editTab;

            }
        }
        // 因为我们动态添加元素 需要从新获取对应的元素
    updateNode() {
            this.lis = this.main.querySelectorAll('li');
            this.sections = this.main.querySelectorAll('section');
            this.remove = this.main.querySelectorAll('.icon-guanbi');
            this.spans = this.main.querySelectorAll('.fisrstnav li span:first-child');
        }
        // 1. 切换功能
    toggleTab() {
            // console.log(this.index);
            that.clearClass();
            this.className = 'liactive';
            that.sections[this.index].className = 'conactive';
        }
        // 清除所有li 和section 的类
    clearClass() {
            for (var i = 0; i < this.lis.length; i++) {
                this.lis[i].className = '';
                this.sections[i].className = '';
            }
        }
        // 2. 添加功能
    addTab() {
            that.clearClass();
            // (1) 创建li元素和section元素 
            var random = Math.random();
            var li = '<li class="liactive"><span>新选项卡</span><span class="iconfont icon-guanbi"></span></li>';
            var section = '<section class="conactive">测试 ' + random + '</section>';
            // (2) 把这两个元素追加到对应的父元素里面
            that.ul.insertAdjacentHTML('beforeend', li);
            that.fsection.insertAdjacentHTML('beforeend', section);
            that.init();
        }
        // 3. 删除功能
    removeTab(e) {
            e.stopPropagation(); // 阻止冒泡 防止触发li 的切换点击事件
            var index = this.parentNode.index;
            console.log(index);
            // 根据索引号删除对应的li 和section   remove()方法可以直接删除指定的元素
            that.lis[index].remove();
            that.sections[index].remove();
            that.init();
            // 当我们删除的不是选中状态的li 的时候,原来的选中状态li保持不变
            if (document.querySelector('.liactive')) return;
            // 当我们删除了选中状态的这个li 的时候, 让它的前一个li 处于选定状态
            index--;
            // 手动调用我们的点击事件  不需要鼠标触发
            that.lis[index] && that.lis[index].click();
        }
        // 4. 修改功能
    editTab() {
        var str = this.innerHTML;
        // 双击禁止选定文字
        window.getSelection ? window.getSelection().removeAllRanges() : document.selection.empty();
        // alert(11);
        this.innerHTML = '<input type="text" />';
        var input = this.children[0];
        input.value = str;
        input.select(); // 文本框里面的文字处于选定状态
        // 当我们离开文本框就把文本框里面的值给span 
        input.onblur = function() {
            this.parentNode.innerHTML = this.value;
        };
        // 按下回车也可以把文本框里面的值给span
        input.onkeyup = function(e) {
            if (e.keyCode === 13) {
                // 手动调用表单失去焦点事件  不需要鼠标离开操作
                this.blur();
            }
        }
    }

}
new Tab('#tab');

index.html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>面向对象 Tab</title>
    <link rel="stylesheet" href="./styles/tab.css">
    <link rel="stylesheet" href="./styles/style.css">
</head>

<body>

    <main>
        <h4>
            Js 面向对象 动态添加标签页
        </h4>
        <div class="tabsbox" id="tab">
            <!-- tab 标签 -->
            <nav class="fisrstnav">
                <ul>
                    <li class="liactive"><span>测试1</span><span class="iconfont icon-guanbi"></span></li>
                    <li><span>测试2</span><span class="iconfont icon-guanbi"></span></li>
                    <li><span>测试3</span><span class="iconfont icon-guanbi"></span></li>
                </ul>
                <div class="tabadd">
                    <span>+</span>
                </div>
            </nav>

            <!-- tab 内容 -->
            <div class="tabscon">
                <section class="conactive">测试1</section>
                <section>测试2</section>
                <section>测试3</section>
            </div>
        </div>
    </main>

    <script src="js/tab.js"></script>
</body>

</html>
```
### 1.2.8  构造函数和原型
```markdown
在典型的 OOP 的语言中（如 Java），都存在类的概念，
类就是对象的模板，对象就是类的实例，但在 ES6之前，
JS 中并没用引入类的概念。
ES6， 全称 ECMAScript 6.0 ，2015.06 发版。但是
目前浏览器的 JavaScript 是 ES5 版本，大多数高版本的浏
览器也支持 ES6，不过只实现了 ES6 的部分特性和功能。
在 ES6之前 ，对象不是基于类创建的，而是用一种称为
构建函数的特殊函数来定义对象和它们的特征。


创建对象可以通过以下三种方式：
1 对象字面量
2 new Object()
3 自定义构造函数
```
#### 1.2.8.1 利用构造函数创建对象
```markdown
构造函数是一种特殊的函数，主要用来初始化对象，即为
对象成员变量赋初始值，它总与 new 一起使用。我
们可以把对象中一些公共的属性和方法抽取出来，然后封
装到这个函数里面。
1.2 构造函数
在 JS 中，使用构造函数时要注意以下两点：
1. 构造函数用于创建某一类对象，其首字母要大写
2. 构造函数要和 new 一起使用才有意义
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
        // 1. 利用 new Object() 创建对象

        var obj1 = new Object();

        // 2. 利用 对象字面量创建对象

        var obj2 = {};

        // 3. 利用构造函数创建对象
        function Star(uname, age) {
            this.uname = uname;
            this.age = age;
            this.sing = function() {
                console.log('我会唱歌');

            }
        }

        var ldh = new Star('刘德华', 18);
        var zxy = new Star('张学友', 19);
        console.log(ldh);
        ldh.sing();
        zxy.sing();
    </script>
</body>

</html>
```
#### 1.2.8.2 静态成员和实例成员
```markdown
JavaScript 的构造函数中可以添加一些成员，可以在构
造函数本身上添加，也可以在构造函数内部的 this 上添
加。通过这两种方式添加的成员，就分别称为静态成员
和实例成员。
静态成员：在构造函数本上添加的成员称为静态成员，只
能由构造函数本身来访问
实例成员：在构造函数内部创建的对象成员称为实例成
员，只能由实例化的对象来访问
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
        // 构造函数中的属性和方法我们称为成员, 成员可以添加
        function Star(uname, age) {
            this.uname = uname;
            this.age = age;
            this.sing = function() {
                console.log('我会唱歌');

            }
        }
        var ldh = new Star('刘德华', 18);
        // 1.实例成员就是构造函数内部通过this添加的成员 uname age sing 就是实例成员
        // 实例成员只能通过实例化的对象来访问
        console.log(ldh.uname);
        ldh.sing();
        // console.log(Star.uname); // 不可以通过构造函数来访问实例成员
        // 2. 静态成员 在构造函数本身上添加的成员  sex 就是静态成员
        Star.sex = '男';
        // 静态成员只能通过构造函数来访问
        console.log(Star.sex);
        console.log(ldh.sex); // 不能通过对象来访问
    </script>
</body>

</html>
```
#### 1.2.8.3 构造函数原型 prototype
```markdown
构造函数通过原型分配的函数是所有对象所共享的。
JavaScript 规定，每一个构造函数都有一个 prototype 
属性，指向另一个对象。注意这个 prototype 就是一
个对象，这个对象的所有属性和方法，都会被构造函数所拥有。
我们可以把那些不变的方法，直接定义在 prototype 对象
上，这样所有对象的实例就可以共享这些方法。
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
        // 1. 构造函数的问题. 
        function Star(uname, age) {
            this.uname = uname;
            this.age = age;
            // this.sing = function() {
            //     console.log('我会唱歌');

            // }
        }
        Star.prototype.sing = function() {
            console.log('我会唱歌');
        }
        var ldh = new Star('刘德华', 18);
        var zxy = new Star('张学友', 19);
        console.log(ldh.sing === zxy.sing);
        // console.dir(Star);
        ldh.sing();
        zxy.sing();
        // 2. 一般情况下,我们的公共属性定义到构造函数里面, 公共的方法我们放到原型对象身上
    </script>
</body>

</html>
```
#### 1.2.8.4 对象原型 proto
```markdown
对象都会有一个属性 __proto__ 指向构造函数的 prototype 
原型对象，之所以我们对象可以使用构造函数
prototype 原型对象的属性和方法，就是因为对象有 __proto__ 
原型的存在。

__proto__对象原型和原型对象 prototype 是等价的
__proto__对象原型的意义就在于为对象的查找机制提供一个方向，
或者说一条路线，但是它是一个非标准属性，
因此实际开发中，不可以使用这个属性，它只是内部指向
原型对象 prototype
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200727105157421.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

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
        function Star(uname, age) {
            this.uname = uname;
            this.age = age;
        }
        Star.prototype.sing = function() {
            console.log('我会唱歌');
        }
        var ldh = new Star('刘德华', 18);
        var zxy = new Star('张学友', 19);
        ldh.sing();
        console.log(ldh); // 对象身上系统自己添加一个 __proto__ 指向我们构造函数的原型对象 prototype
        console.log(ldh.__proto__ === Star.prototype);
        // 方法的查找规则: 首先先看ldh 对象身上是否有 sing 方法,如果有就执行这个对象上的sing
        // 如果没有sing 这个方法,因为有__proto__ 的存在,就去构造函数原型对象prototype身上去查找sing这个方法
    </script>
</body>

</html>
```
#### 1.2.8.5 原型constructor
```markdown
对象原型（ __proto__）和构造函数（prototype）原
型对象里面都有一个属性 constructor 属性 ，constructor 
我们称为构造函数，因为它指回构造函数本身。
constructor 主要用于记录该对象引用于哪个构造函数，它
可以让原型对象重新指向原来的构造函数。
一般情况下，对象的方法都在构造函数的原型对象中设置。如
果有多个对象的方法，我们可以给原型对象采取对象形式赋
值，但是这样就会覆盖构造函数原型对象原来的内容，这
样修改后的原型对象 constructor 就不再指向当前构造函数了。
此时，我们可以在修改后的原型对象中，添加一个 constructor 
指向原来的构造函数。
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
        function Star(uname, age) {
            this.uname = uname;
            this.age = age;
        }
        // 很多情况下,我们需要手动的利用constructor 这个属性指回 原来的构造函数
        // Star.prototype.sing = function() {
        //     console.log('我会唱歌');
        // };
        // Star.prototype.movie = function() {
        //     console.log('我会演电影');
        // }
        Star.prototype = {
            // 如果我们修改了原来的原型对象,给原型对象赋值的是一个对象,则必须手动的利用constructor指回原来的构造函数
            constructor: Star,
            sing: function() {
                console.log('我会唱歌');
            },
            movie: function() {
                console.log('我会演电影');
            }
        }
        var ldh = new Star('刘德华', 18);
        var zxy = new Star('张学友', 19);
        console.log(Star.prototype);
        console.log(ldh.__proto__);
        console.log(Star.prototype.constructor);
        console.log(ldh.__proto__.constructor);
    </script>
</body>

</html>
```
#### 1.2.8.6 构造函数、实例、原型对象三者之间的关系
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200728075842229.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
#### 1.2.8.7 原型链
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200728080701813.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
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
        function Star(uname, age) {
            this.uname = uname;
            this.age = age;
        }
        Star.prototype.sing = function() {
            console.log('我会唱歌');
        }
        var ldh = new Star('刘德华', 18);
        // 1. 只要是对象就有__proto__ 原型, 指向原型对象
        console.log(Star.prototype);
        console.log(Star.prototype.__proto__ === Object.prototype);
        // 2.我们Star原型对象里面的__proto__原型指向的是 Object.prototype
        console.log(Object.prototype.__proto__);
        // 3. 我们Object.prototype原型对象里面的__proto__原型  指向为 null
    </script>
</body>

</html>
```
#### 1.2.8.8 JavaScript 的成员查找机制
```markdown
1 当访问一个对象的属性（包括方法）时，首先查找
这个对象自身有没有该属性。
2 如果没有就查找它的原型（也就是 __proto__指向的
prototype 原型对象）。
3  如果还没有就查找原型对象的原型（Object的原型对象）。
4  依此类推一直找到 Object 为止（null）。
5 __proto__对象原型的意义就在于为对象成员查找机制提供
一个方向，或者说一条路线。
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
        function Star(uname, age) {
            this.uname = uname;
            this.age = age;
        }
        Star.prototype.sing = function() {
            console.log('我会唱歌');

        }
        Star.prototype.sex = '女';
        // Object.prototype.sex = '男';
        var ldh = new Star('刘德华', 18);
        ldh.sex = '男';
        console.log(ldh.sex);
        console.log(Object.prototype);
        console.log(ldh);
        console.log(Star.prototype);
        console.log(ldh.toString());
    </script>
</body>

</html>
```
#### 1.2.8.9 原型对象this指向
```markdown
构造函数中的this 指向我们实例对象.
原型对象里面放的是方法, 这个方法里面的this 指向的是 这个方法的调用者, 也就是这个实例对象.
```
#### 1.2.8.10 扩展内置对象
```markdown
可以通过原型对象，对原来的内置对象进行扩展自定义的
方法。比如给数组增加自定义求偶数和的功能。
注意：数组和字符串内置对象不能给原型对象覆
盖操作 Array.prototype = {} ，只能是 
Array.prototype.xxx = function(){} 的方式。
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
        // 原型对象的应用 扩展内置对象方法

        Array.prototype.sum = function() {
            var sum = 0;
            for (var i = 0; i < this.length; i++) {
                sum += this[i];
            }
            return sum;
        };
        // Array.prototype = {
        //     sum: function() {
        //         var sum = 0;
        //         for (var i = 0; i < this.length; i++) {
        //             sum += this[i];
        //         }
        //         return sum;
        //     }

        // }
        var arr = [1, 2, 3];
        console.log(arr.sum());
        console.log(Array.prototype);
        var arr1 = new Array(11, 22, 33);
        console.log(arr1.sum());
    </script>
</body>

</html>
```
### 1.2.9 继承
```markdown
ES6之前并没有给我们提供 extends 继承。我们可以通
过构造函数+原型对象模拟实现继承，被称为组合继承
```
#### 1.2.9.1  call方法
```markdown
调用这个函数, 并且修改函数运行时的 this 指向
fun.call(thisArg, arg1, arg2, ...)
thisArg ：当前调用函数 this 的指向对象
arg1，arg2：传递的其他参数
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
        // call 方法
        function fn(x, y) {
            console.log('我想喝手磨咖啡');
            console.log(this);
            console.log(x + y);


        }
        var o = {
            name: 'andy'
        };
        // fn();
        // 1. call() 可以调用函数
        // fn.call();
        // 2. call() 可以改变这个函数的this指向 此时这个函数的this 就指向了o这个对象
        fn.call(o, 1, 2);
    </script>
</body>

</html>
```
#### 1.2.9.2 借用父构造函数继承属性
```markdown
核心原理： 通过 call() 把父类型的 this 指向子类型
的 this ，这样就可以实现子类型继承父类型的属性
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
        // 借用父构造函数继承属性
        // 1. 父构造函数
        function Father(uname, age) {
            // this 指向父构造函数的对象实例
            this.uname = uname;
            this.age = age;
        }
        // 2 .子构造函数 
        function Son(uname, age, score) {
            // this 指向子构造函数的对象实例
            Father.call(this, uname, age);
            this.score = score;
        }
        var son = new Son('刘德华', 18, 100);
        console.log(son);
    </script>
</body>

</html>
```
 #### 1.2.9.3 借用原型对象继承方法
```html
一般情况下，对象的方法都在构造函数的原型对象中设置，
通过构造函数无法继承父类方法。
核心原理：
1 将子类所共享的方法提取出来，让子类的 prototype 原
型对象 = new 父类()
2 本质：子类原型对象等于是实例化父类，因为父
类实例化之后另外开辟空间，就不会影响原来父类原型对象
3 将子类的 constructor 从新指向子类的构造函数
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
        // 借用父构造函数继承属性
        // 1. 父构造函数
        function Father(uname, age) {
            // this 指向父构造函数的对象实例
            this.uname = uname;
            this.age = age;
        }
        Father.prototype.money = function() {
            console.log(100000);

        };
        // 2 .子构造函数 
        function Son(uname, age, score) {
            // this 指向子构造函数的对象实例
            Father.call(this, uname, age);
            this.score = score;
        }
        // Son.prototype = Father.prototype;  这样直接赋值会有问题,如果修改了子原型对象,父原型对象也会跟着一起变化
        Son.prototype = new Father();
        // 如果利用对象的形式修改了原型对象,别忘了利用constructor 指回原来的构造函数
        Son.prototype.constructor = Son;
        // 这个是子构造函数专门的方法
        Son.prototype.exam = function() {
            console.log('孩子要考试');

        }
        var son = new Son('刘德华', 18, 100);
        console.log(son);
        console.log(Father.prototype);
        console.log(Son.prototype.constructor);
    </script>
</body>

</html>
```
### 1.2.10 ES5 中的新增方法
```markdown
ES5 中给我们新增了一些方法，可以很方便的操作数
组或者字符串，这些方法主要包括：
数组方法
字符串方法
对象方法
```
#### 1.2.10.1  数组方法
```markdown
迭代(遍历)方法：forEach()、map()、filter()、some()、every()；
```
##### 1.2.10.1.1 forEach方法
```markdown
array.forEach(function(currentValue, index, arr))

currentValue：数组当前项的值
index：数组当前项的索引
arr：数组对象本身
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
        // forEach 迭代(遍历) 数组
        var arr = [1, 2, 3];
        var sum = 0;
        arr.forEach(function(value, index, array) {
            console.log('每个数组元素' + value);
            console.log('每个数组元素的索引号' + index);
            console.log('数组本身' + array);
            sum += value;
        })
        console.log(sum);
    </script>
</body>

</html>
```
##### 1.2.10.1.2 filter筛选数组
```markdown
array.filter(function(currentValue, index, arr))
filter() 方法创建一个新的数组，新数组中的元素是通过
检查指定数组中符合条件的所有元素,主要用于筛选数组
注意它直接返回一个新数组
currentValue: 数组当前项的值
index：数组当前项的索引
arr：数组对象本身
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
        // filter 筛选数组
        var arr = [12, 66, 4, 88, 3, 7];
        var newArr = arr.filter(function(value, index) {
            // return value >= 20;
            return value % 2 === 0;
        });
        console.log(newArr);
    </script>
</body>

</html>
```
##### 1.2.10.1.3 some查找数组中是否有满足条件的元素
```markdown
array.some(function(currentValue, index, arr))
some() 方法用于检测数组中的元素是否满足指定条件. 通俗点
查找数组中是否有满足条件的元素
注意它返回值是布尔值, 如果查找到这个元素, 就返回true , 
如果查找不到就返回false.
如果找到第一个满足条件的元素,则终止循环. 不在继续查找.
currentValue: 数组当前项的值
index：数组当前项的索引
arr：数组对象本身
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
        // some 查找数组中是否有满足条件的元素 
        // var arr = [10, 30, 4];
        // var flag = arr.some(function(value) {
        //     // return value >= 20;
        //     return value < 3;
        // });
        // console.log(flag);
        var arr1 = ['red', 'pink', 'blue'];
        var flag1 = arr1.some(function(value) {
            return value == 'pink';
        });
        console.log(flag1);
        // 1. filter 也是查找满足条件的元素 返回的是一个数组 而且是把所有满足条件的元素返回回来
        // 2. some 也是查找满足条件的元素是否存在  返回的是一个布尔值 如果查找到第一个满足条件的元素就终止循环
    </script>
</body>

</html>
```
##### 1.2.10.1.4 查询商品案例
```markdown
1. 把数据渲染到页面中 (forEach)
2. 根据价格显示数据
3. 根据商品名称显示数据
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
        table {
            width: 400px;
            border: 1px solid #000;
            border-collapse: collapse;
            margin: 0 auto;
        }
        
        td,
        th {
            border: 1px solid #000;
            text-align: center;
        }
        
        input {
            width: 50px;
        }
        
        .search {
            width: 600px;
            margin: 20px auto;
        }
    </style>
</head>

<body>
    <div class="search">
        按照价格查询: <input type="text" class="start"> - <input type="text" class="end"> <button class="search-price">搜索</button> 按照商品名称查询: <input type="text" class="product"> <button class="search-pro">查询</button>
    </div>
    <table>
        <thead>
            <tr>
                <th>id</th>
                <th>产品名称</th>
                <th>价格</th>
            </tr>
        </thead>
        <tbody>


        </tbody>
    </table>
    <script>
        // 利用新增数组方法操作数据
        var data = [{
            id: 1,
            pname: '小米',
            price: 3999
        }, {
            id: 2,
            pname: 'oppo',
            price: 999
        }, {
            id: 3,
            pname: '荣耀',
            price: 1299
        }, {
            id: 4,
            pname: '华为',
            price: 1999
        }, ];
        // 1. 获取相应的元素
        var tbody = document.querySelector('tbody');
        var search_price = document.querySelector('.search-price');
        var start = document.querySelector('.start');
        var end = document.querySelector('.end');
        var product = document.querySelector('.product');
        var search_pro = document.querySelector('.search-pro');
        setDate(data);
        // 2. 把数据渲染到页面中
        function setDate(mydata) {
            // 先清空原来tbody 里面的数据
            tbody.innerHTML = '';
            mydata.forEach(function(value) {
                // console.log(value);
                var tr = document.createElement('tr');
                tr.innerHTML = '<td>' + value.id + '</td><td>' + value.pname + '</td><td>' + value.price + '</td>';
                tbody.appendChild(tr);
            });
        }

        // 3. 根据价格查询商品
        // 当我们点击了按钮,就可以根据我们的商品价格去筛选数组里面的对象
        search_price.addEventListener('click', function() {
            // alert(11);
            var newDate = data.filter(function(value) {
                return value.price >= start.value && value.price <= end.value;
            });
            console.log(newDate);
            // 把筛选完之后的对象渲染到页面中
            setDate(newDate);
        });
        // 4. 根据商品名称查找商品
        // 如果查询数组中唯一的元素, 用some方法更合适,因为它找到这个元素,就不在进行循环,效率更高]
        search_pro.addEventListener('click', function() {
            var arr = [];
            data.some(function(value) {
                if (value.pname === product.value) {
                    // console.log(value);
                    arr.push(value);
                    return true; // return 后面必须写true  
                }
            });
            // 把拿到的数据渲染到页面中
            setDate(arr);
        })
    </script>
</body>

</html>
```
##### 1.2.10.1.5 forEach和some区别
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
        var arr = ['red', 'green', 'blue', 'pink'];
        // 1. forEach迭代 遍历
        // arr.forEach(function(value) {
        //     if (value == 'green') {
        //         console.log('找到了该元素');
        //         return true; // 在forEach 里面 return 不会终止迭代
        //     }
        //     console.log(11);

        // })
        // 如果查询数组中唯一的元素, 用some方法更合适,
        arr.some(function(value) {
            if (value == 'green') {
                console.log('找到了该元素');
                return true; //  在some 里面 遇到 return true 就是终止遍历 迭代效率更高
            }
            console.log(11);

        });
        // arr.filter(function(value) {
        //     if (value == 'green') {
        //         console.log('找到了该元素');
        //         return true; //  // filter 里面 return 不会终止迭代
        //     }
        //     console.log(11);

        // });
    </script>
</body>

</html>

```
### 1.2.11 字符串方法
```markdown
trim() 方法会从一个字符串的两端删除空白字符。
str.trim()
trim() 方法并不影响原字符串本身，它返回的是一个新的字符串。
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
    <input type="text"> <button>点击</button>
    <div></div>
    <script>
        // trim 方法去除字符串两侧空格
        var str = '   an  dy   ';
        console.log(str);
        var str1 = str.trim();
        console.log(str1);
        var input = document.querySelector('input');
        var btn = document.querySelector('button');
        var div = document.querySelector('div');
        btn.onclick = function() {
            var str = input.value.trim();
            if (str === '') {
                alert('请输入内容');
            } else {
                console.log(str);
                console.log(str.length);
                div.innerHTML = str;
            }
        }
    </script>
</body>

</html>
```
#### 1.2.11.1 对象方法
```markdown
Object.keys() 方法返回一个所有元素为字符串的数组。
Object.keys(obj)
效果类似 for…in

Object.defineProperty() 定义新属性或修改原有的属性。
Object.defineProperty(obj, prop, descriptor)
obj：必需。目标对象
prop：必需。需定义或修改的属性的名字
descriptor：必需。目标属性所拥有的特性
Object.defineProperty() 第三个参数 descriptor 说明：
value: 设置属性的值
writable: 值是否可以重写。true | false
enumerable: 目标属性是否可以被枚举。true | false
configurable: 目标属性是否可以被删除或是否可以再次
修改特性 true | false
```
#### 1.2.11.2 Object.keys遍历对象属性
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
        // 用于获取对象自身所有的属性
        var obj = {
            id: 1,
            pname: '小米',
            price: 1999,
            num: 2000
        };
        var arr = Object.keys(obj);
        console.log(arr);
        arr.forEach(function(value) {
            console.log(value);

        })
    </script>
</body>

</html>
```
#### 1.2.11.3 Object.defineProperty方法
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
        // Object.defineProperty() 定义新属性或修改原有的属性
        var obj = {
            id: 1,
            pname: '小米',
            price: 1999
        };
        // 1. 以前的对象添加和修改属性的方式
        // obj.num = 1000;
        // obj.price = 99;
        // console.log(obj);
        // 2. Object.defineProperty() 定义新属性或修改原有的属性
        Object.defineProperty(obj, 'num', {
            value: 1000,
            enumerable: true
        });
        console.log(obj);
        Object.defineProperty(obj, 'price', {
            value: 9.9
        });
        console.log(obj);
        Object.defineProperty(obj, 'id', {
            // 如果值为false 不允许修改这个属性值 默认值也是false
            writable: false,
        });
        obj.id = 2;
        console.log(obj);
        Object.defineProperty(obj, 'address', {
            value: '中国山东蓝翔技校xx单元',
            // 如果只为false 不允许修改这个属性值 默认值也是false
            writable: false,
            // enumerable 如果值为false 则不允许遍历, 默认的值是 false
            enumerable: false,
            // configurable 如果为false 则不允许删除这个属性 不允许在修改第三个参数里面的特性 默认为false
            configurable: false
        });
        console.log(obj);
        console.log(Object.keys(obj));
        delete obj.address;
        console.log(obj);
        delete obj.pname;
        console.log(obj);
        Object.defineProperty(obj, 'address', {
            value: '中国山东蓝翔技校xx单元',
            // 如果只为false 不允许修改这个属性值 默认值也是false
            writable: true,
            // enumerable 如果值为false 则不允许遍历, 默认的值是 false
            enumerable: true,
            // configurable 如果为false 则不允许删除这个属性 默认为false
            configurable: true
        });
        console.log(obj.address);
    </script>
</body>

</html>
```
## 2.1 函数进阶
### 2.1.1  函数的定义
```markdown
1 函数声明方式 function 关键字 (命名函数)
2 函数表达式 (匿名函数)
3 new Function()
var fn = new Function('参数1','参数2'..., '函数体')
Function 里面参数都必须是字符串格式
第三种方式执行效率低，也不方便书写，因此较少使用
所有函数都是 Function 的实例(对象)
函数也属于对象
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
        //  函数的定义方式

        // 1. 自定义函数(命名函数) 

        function fn() {};

        // 2. 函数表达式 (匿名函数)

        var fun = function() {};


        // 3. 利用 new Function('参数1','参数2', '函数体');

        var f = new Function('a', 'b', 'console.log(a + b)');
        f(1, 2);
        // 4. 所有函数都是 Function 的实例(对象)
        console.dir(f);
        // 5. 函数也属于对象
        console.log(f instanceof Object);
    </script>
</body>

</html>

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200730230402132.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 2.1.2 函数的调用
```markdown
1 普通函数
2 对象的方法
3 构造函数
4 绑定事件函数
5 定时器函数
6 立即执行函数
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
        // 函数的调用方式

        // 1. 普通函数
        function fn() {
            console.log('人生的巅峰');

        }
        // fn();   fn.call()
        // 2. 对象的方法
        var o = {
            sayHi: function() {
                console.log('人生的巅峰');

            }
        }
        o.sayHi();
        // 3. 构造函数
        function Star() {};
        new Star();
        // 4. 绑定事件函数
        // btn.onclick = function() {};   // 点击了按钮就可以调用这个函数
        // 5. 定时器函数
        // setInterval(function() {}, 1000);  这个函数是定时器自动1秒钟调用一次
        // 6. 立即执行函数
        (function() {
            console.log('人生的巅峰');
        })();
        // 立即执行函数是自动调用
    </script>
</body>

</html>
```
### 2.1.3 函数内 this 的指向
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200730230838904.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

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
    <button>点击</button>
    <script>
        // 函数的不同调用方式决定了this 的指向不同
        // 1. 普通函数 this 指向window
        function fn() {
            console.log('普通函数的this' + this);
        }
        window.fn();
        // 2. 对象的方法 this指向的是对象 o
        var o = {
            sayHi: function() {
                console.log('对象方法的this:' + this);
            }
        }
        o.sayHi();
        // 3. 构造函数 this 指向 ldh 这个实例对象 原型对象里面的this 指向的也是 ldh这个实例对象
        function Star() {};
        Star.prototype.sing = function() {

        }
        var ldh = new Star();
        // 4. 绑定事件函数 this 指向的是函数的调用者 btn这个按钮对象
        var btn = document.querySelector('button');
        btn.onclick = function() {
            console.log('绑定时间函数的this:' + this);
        };
        // 5. 定时器函数 this 指向的也是window
        window.setTimeout(function() {
            console.log('定时器的this:' + this);

        }, 1000);
        // 6. 立即执行函数 this还是指向window
        (function() {
            console.log('立即执行函数的this' + this);
        })();
    </script>
</body>

</html>
```
### 2.1.4 改变函数内this指向call方法
```markdown
1. call 方法
call() 方法调用一个对象。简单理解为调用函数的方式，但
是它可以改变函数的 this 指向。
fun.call(thisArg, arg1, arg2, ...)
thisArg：在 fun 函数运行时指定的 this 值
arg1，arg2：传递的其他参数
返回值就是函数的返回值，因为它就是调用函数
因此当我们想改变 this 指向，同时想调用这个函数的时
候，可以使用 call，比如继承
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
        // 改变函数内this指向  js提供了三种方法  call()  apply()  bind()

        // 1. call()
        var o = {
            name: 'andy'
        }

        function fn(a, b) {
            console.log(this);
            console.log(a + b);

        };
        fn.call(o, 1, 2);
        // call 第一个可以调用函数 第二个可以改变函数内的this 指向
        // call 的主要作用可以实现继承
        function Father(uname, age, sex) {
            this.uname = uname;
            this.age = age;
            this.sex = sex;
        }

        function Son(uname, age, sex) {
            Father.call(this, uname, age, sex);
        }
        var son = new Son('刘德华', 18, '男');
        console.log(son);
    </script>
</body>

</html>
```
### 2.1.5 改变函数内this指向apply方法
```markdown
apply() 方法调用一个函数。简单理解为调用函数的方式，
但是它可以改变函数的 this 指向。
fun.apply(thisArg, [argsArray])
thisArg：在fun函数运行时指定的 this 值
argsArray：传递的值，必须包含在数组里面
返回值就是函数的返回值，因为它就是调用函数
因此 apply 主要跟数组有关系，比如使用 Math.max() 求
数组的最大值
```
### 2.1.6 改变函数内this指向bind方法
```markdown
bind() 方法不会调用函数。但是能改变函数内部this 指向
fun.bind(thisArg, arg1, arg2, ...)
thisArg：在 fun 函数运行时指定的 this 值
arg1，arg2：传递的其他参数
返回由指定的 this 值和初始化参数改造的原函数拷贝
因此当我们只是想改变 this 指向，并且不想调用这个函数的
时候，可以使用 bind
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
    <button>点击</button>
    <button>点击</button>
    <button>点击</button>
    <script>
        // 改变函数内this指向  js提供了三种方法  call()  apply()  bind()

        // 3. bind()  绑定 捆绑的意思
        var o = {
            name: 'andy'
        };

        function fn(a, b) {
            console.log(this);
            console.log(a + b);


        };
        var f = fn.bind(o, 1, 2);
        f();
        // 1. 不会调用原来的函数   可以改变原来函数内部的this 指向
        // 2. 返回的是原函数改变this之后产生的新函数
        // 3. 如果有的函数我们不需要立即调用,但是又想改变这个函数内部的this指向此时用bind
        // 4. 我们有一个按钮,当我们点击了之后,就禁用这个按钮,3秒钟之后开启这个按钮
        // var btn1 = document.querySelector('button');
        // btn1.onclick = function() {
        //     this.disabled = true; // 这个this 指向的是 btn 这个按钮
        //     // var that = this;
        //     setTimeout(function() {
        //         // that.disabled = false; // 定时器函数里面的this 指向的是window
        //         this.disabled = false; // 此时定时器函数里面的this 指向的是btn
        //     }.bind(this), 3000); // 这个this 指向的是btn 这个对象
        // }
        var btns = document.querySelectorAll('button');
        for (var i = 0; i < btns.length; i++) {
            btns[i].onclick = function() {
                this.disabled = true;
                setTimeout(function() {
                    this.disabled = false;
                }.bind(this), 2000);
            }
        }
    </script>
</body>

</html>
```
### 2.1.7  call apply bind 总结
```markdown
相同点:
都可以改变函数内部的this指向.
区别点:
1. call 和 apply 会调用函数, 并且改变函数内部this指向.
2. call 和 apply 传递的参数不一样, call 传递参
数 aru1, aru2..形式 apply 必须数组形式[arg]
3. bind 不会调用函数, 可以改变函数内部this指向.
主要应用场景:
4. call 经常做继承.
5. apply 经常跟数组有关系. 比如借助于数学对象实
现数组最大值最小值
6. bind 不调用函数,但是还想改变this指向. 比如改变
定时器内部的this指向.
```
### 2.1.8 严格模式
```markdown
JavaScript 除了提供正常模式外，还提供了严格
模式（strict mode）。ES5 的严格模式是采用具有限制性
JavaScript 变体的一种方式，即在严格的条件
下运行 JS 代码。
严格模式在 IE10 以上版本的浏览器中才会被
支持，旧版本浏览器中会被忽略。
严格模式对正常的 JavaScript 语义做了一些更改：
1 消除了 Javascript 语法的一些不合理、不严谨
之处，减少了一些怪异行为。
2 消除代码运行的一些不安全之处，保证代码运行的安全。
3 提高编译器效率，增加运行速度。
4 禁用了在 ECMAScript 的未来版本中可能会定义的一
些语法，为未来新版本的 Javascript 做好铺垫。比
如一些保留字如：class, enum, export, extends, import, super 
不能做变量名
```
### 2.1.9 开启严格模式
```markdown
严格模式可以应用到整个脚本或个别函数中。因此
在使用时，我们可以将严格模式分为为脚本开启严格模式和
为函数开启严格模式两种情况。

为整个脚本文件开启严格模式，需要在所有语句之前放
一个特定语句“use strict”;（或‘use strict’;）。

有的 script 基本是严格模式，有的 script 脚本是正常模式，
这样不利于文件合并，所以可以将整个脚本文件
放在一个立即执行的匿名函数之中。这样独立创建
一个作用域而不影响其他 script 脚本文件。
要给某个函数开启严格模式，需要把“use strict”; 
(或 'use strict'; ) 声明放在函数体所有语句之前
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
    <!-- 为整个脚本(script标签)开启严格模式 -->
    <script>
        'use strict';
        //   下面的js 代码就会按照严格模式执行代码
    </script>
    <script>
        (function() {
            'use strict';
        })();
    </script>
    <!-- 为某个函数开启严格模式 -->
    <script>
        // 此时只是给fn函数开启严格模式
        function fn() {
            'use strict';
            // 下面的代码按照严格模式执行
        }

        function fun() {
            // 里面的还是按照普通模式执行
        }
    </script>
</body>

</html>
```
### 2.1.10  严格模式中的变化
```markdown
1 变量规定

1 在正常模式中，如果一个变量没有声明就赋值，默认是
全局变量。严格模式禁止这种用法，变量都必须先用
var 命令声明，然后再使用。
2 严禁删除已经声明变量。例如，delete x; 语法是错误的。

2 严格模式下 this 指向问题

1 以前在全局作用域函数中的 this 指向 window 对象。
2 严格模式下全局作用域中函数中的 this 是 undefined。
3 以前构造函数时不加 new也可以 调用,当普通函数，this 
指向全局对象
4 严格模式下,如果 构造函数不加new调用, this 指向
的是undefined 如果给他赋值则 会报错
5 new 实例化的构造函数指向创建的对象实例。
6 定时器 this 还是指向 window 。
7 事件、对象还是指向调用者。

3 函数变化

1 函数不能有重名的参数。
2 函数必须声明在顶层.新版本的 JavaScript 会引入“块级
作用域”（ ES6 中已引入）。为了与新版本接轨，
不允许在非函数的代码块内声明函数。
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
        'use strict';
        // 1. 我们的变量名必须先声明再使用
        // num = 10;
        // console.log(num);
        var num = 10;
        console.log(num);
        // 2.我们不能随意删除已经声明好的变量
        // delete num;
        // 3. 严格模式下全局作用域中函数中的 this 是 undefined。
        // function fn() {
        //     console.log(this); // undefined。

        // }
        // fn();
        // 4. 严格模式下,如果 构造函数不加new调用, this 指向的是undefined 如果给他赋值则 会报错.
        // function Star() {
        //     this.sex = '男';
        // }
        // // Star();
        // var ldh = new Star();
        // console.log(ldh.sex);
        // 5. 定时器 this 还是指向 window 
        // setTimeout(function() {
        //     console.log(this);

        // }, 2000);
        // a = 1;
        // a = 2;
        // 6. 严格模式下函数里面的参数不允许有重名
        // function fn(a, a) {
        //     console.log(a + a);

        // };
        // fn(1, 2);
        function fn() {}
    </script>
</body>

</html>
```
### 2.1.11 高阶函数
```markdown
高阶函数是对其他函数进行操作的函数，它接收
函数作为参数或将函数作为返回值输出
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
            width: 100px;
            height: 100px;
            background-color: pink;
        }
    </style>
</head>

<body>
    <div></div>
    <script>
        // 高阶函数- 函数可以作为参数传递
        function fn(a, b, callback) {
            console.log(a + b);
            callback && callback();
        }
        fn(1, 2, function() {
            console.log('我是最后调用的');

        });
        $("div").animate({
            left: 500
        }, function() {
            $("div").css("backgroundColor", "purple");
        })
    </script>
</body>

</html>
```
### 2.1.12 闭包
```markdown
变量根据作用域的不同分为两种：全局变量和局部变量。
1 函数内部可以使用全局变量。
2 函数外部不可以使用局部变量。
3 当函数执行完毕，本作用域内的局部变量会销毁。

闭包（closure）指有权访问另一个函数作用域中变量的
函数。 ----- JavaScript 高级程序设计
简单理解就是 ，一个作用域可以访问另外一个函数内部的局部变量。
延伸变量的作用范围
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
        // 闭包（closure）指有权访问另一个函数作用域中变量的函数。
        // 闭包: 我们fun 这个函数作用域 访问了另外一个函数 fn 里面的局部变量 num
        function fn() {
            var num = 10;

            function fun() {
                console.log(num);

            }
            fun();
        }
        fn();
    </script>
</body>

</html>
```
### 2.1.13 闭包的作用
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
        // 闭包（closure）指有权访问另一个函数作用域中变量的函数。
        // 一个作用域可以访问另外一个函数的局部变量 
        // 我们fn 外面的作用域可以访问fn 内部的局部变量
        // 闭包的主要作用: 延伸了变量的作用范围
        function fn() {
            var num = 10;

            // function fun() {
            //     console.log(num);

            // }
            // return fun;
            return function() {
                console.log(num);
            }
        }
        var f = fn();
        f();
        // 类似于
        // var f = function() {
        //         console.log(num);
        //     }
        // var f =  function fun() {
        //         console.log(num);

        //     }
    </script>
</body>

</html>
```
### 2.1.14 闭包应用-点击li输出索引号
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
    <ul class="nav">
        <li>榴莲</li>
        <li>臭豆腐</li>
        <li>鲱鱼罐头</li>
        <li>大猪蹄子</li>
    </ul>
    <script>
        // 闭包应用-点击li输出当前li的索引号
        // 1. 我们可以利用动态添加属性的方式
        var lis = document.querySelector('.nav').querySelectorAll('li');
        for (var i = 0; i < lis.length; i++) {
            lis[i].index = i;
            lis[i].onclick = function() {
                // console.log(i);
                console.log(this.index);

            }
        }
        // 2. 利用闭包的方式得到当前小li 的索引号
        for (var i = 0; i < lis.length; i++) {
            // 利用for循环创建了4个立即执行函数
            // 立即执行函数也成为小闭包因为立即执行函数里面的任何一个函数都可以使用它的i这变量
            (function(i) {
                // console.log(i);
                lis[i].onclick = function() {
                    console.log(i);

                }
            })(i);
        }
    </script>
</body>

</html>
```
### 2.1.15 闭包应用-定时器中的闭包
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
    <ul class="nav">
        <li>榴莲</li>
        <li>臭豆腐</li>
        <li>鲱鱼罐头</li>
        <li>大猪蹄子</li>
    </ul>
    <script>
        // 闭包应用-3秒钟之后,打印所有li元素的内容
        var lis = document.querySelector('.nav').querySelectorAll('li');
        for (var i = 0; i < lis.length; i++) {
            (function(i) {
                setTimeout(function() {
                    console.log(lis[i].innerHTML);
                }, 3000)
            })(i);
        }
    </script>
</body>

</html>
```
### 2.1.16 闭包应用-打车价格
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
        // 闭包应用-计算打车价格 
        // 打车起步价13(3公里内),  之后每多一公里增加 5块钱.  用户输入公里数就可以计算打车价格
        // 如果有拥堵情况,总价格多收取10块钱拥堵费
        // function fn() {};
        // fn();
        var car = (function() {
            var start = 13; // 起步价  局部变量
            var total = 0; // 总价  局部变量
            return {
                // 正常的总价
                price: function(n) {
                    if (n <= 3) {
                        total = start;
                    } else {
                        total = start + (n - 3) * 5
                    }
                    return total;
                },
                // 拥堵之后的费用
                yd: function(flag) {
                    return flag ? total + 10 : total;
                }
            }
        })();
        console.log(car.price(5)); // 23
        console.log(car.yd(true)); // 33

        console.log(car.price(1)); // 13
        console.log(car.yd(false)); // 13
    </script>
</body>

</html>
```
### 2.1.17 递归函数
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
        // 递归函数 : 函数内部自己调用自己, 这个函数就是递归函数
        var num = 1;

        function fn() {
            console.log('我要打印6句话');

            if (num == 6) {
                return; // 递归里面必须加退出条件
            }
            num++;
            fn();
        }
        fn();
    </script>
</body>

</html>
```
### 2.1.18 利用递归求1~n的阶乘
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
        // 利用递归函数求1~n的阶乘 1 * 2 * 3 * 4 * ..n
        function fn(n) {
            if (n == 1) {
                return 1;
            }
            return n * fn(n - 1);
        }
        console.log(fn(3));
        console.log(fn(4));
        // 详细思路 假如用户输入的是3
        //return  3 * fn(2)
        //return  3 * (2 * fn(1))
        //return  3 * (2 * 1)
        //return  3 * (2)
        //return  6
    </script>
</body>

</html>
```
### 2.1.19 利用递归函数求斐波那契数列
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
        // 利用递归函数求斐波那契数列(兔子序列)  1、1、2、3、5、8、13、21...
        // 用户输入一个数字 n 就可以求出 这个数字对应的兔子序列值
        // 我们只需要知道用户输入的n 的前面两项(n-1 n-2)就可以计算出n 对应的序列值
        function fb(n) {
            if (n === 1 || n === 2) {
                return 1;
            }
            return fb(n - 1) + fb(n - 2);
        }
        console.log(fb(3));
        console.log(fb(6));
    </script>
</body>

</html>
```
### 2.1.20 利用递归遍历数据
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
        var data = [{
            id: 1,
            name: '家电',
            goods: [{
                id: 11,
                gname: '冰箱',
                goods: [{
                    id: 111,
                    gname: '海尔'
                }, {
                    id: 112,
                    gname: '美的'
                }, ]
            }, {
                id: 12,
                gname: '洗衣机'
            }]
        }, {
            id: 2,
            name: '服饰'
        }];
        // 我们想要做输入id号,就可以返回的数据对象
        // 1. 利用 forEach 去遍历里面的每一个对象
        function getID(json, id) {
            var o = {};
            json.forEach(function(item) {
                // console.log(item); // 2个数组元素
                if (item.id == id) {
                    // console.log(item);
                    o = item;
                    // 2. 我们想要得里层的数据 11 12 可以利用递归函数
                    // 里面应该有goods这个数组并且数组的长度不为 0 
                } else if (item.goods && item.goods.length > 0) {
                    o = getID(item.goods, id);
                }

            });
            return o;
        }
        console.log(getID(data, 1));
        console.log(getID(data, 2));
        console.log(getID(data, 11));
        console.log(getID(data, 12));
        console.log(getID(data, 111));
    </script>
</body>

</html>
```
### 2.1.21 浅拷贝
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
        // 浅拷贝只是拷贝一层, 更深层次对象级别的只拷贝引用.
        // 深拷贝拷贝多层, 每一级别的数据都会拷贝.
        var obj = {
            id: 1,
            name: 'andy',
            msg: {
                age: 18
            }
        };
        var o = {};
        // for (var k in obj) {
        //     // k 是属性名   obj[k] 属性值
        //     o[k] = obj[k];
        // }
        // console.log(o);
        // o.msg.age = 20;
        // console.log(obj);

        console.log('--------------');
        Object.assign(o, obj);
        console.log(o);
        o.msg.age = 20;
        console.log(obj);
    </script>
</body>

</html>
```
### 2.1.22 深拷贝
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
        // 深拷贝拷贝多层, 每一级别的数据都会拷贝.
        var obj = {
            id: 1,
            name: 'andy',
            msg: {
                age: 18
            },
            color: ['pink', 'red']
        };
        var o = {};
        // 封装函数 
        function deepCopy(newobj, oldobj) {
            for (var k in oldobj) {
                // 判断我们的属性值属于那种数据类型
                // 1. 获取属性值  oldobj[k]
                var item = oldobj[k];
                // 2. 判断这个值是否是数组
                if (item instanceof Array) {
                    newobj[k] = [];
                    deepCopy(newobj[k], item)
                } else if (item instanceof Object) {
                    // 3. 判断这个值是否是对象
                    newobj[k] = {};
                    deepCopy(newobj[k], item)
                } else {
                    // 4. 属于简单数据类型
                    newobj[k] = item;
                }

            }
        }
        deepCopy(o, obj);
        console.log(o);

        var arr = [];
        console.log(arr instanceof Object);
        o.msg.age = 20;
        console.log(obj);
    </script>
</body>

</html>
```
## 3.1 正则表达式
```markdown
正则表达式（ Regular Expression ）是用于匹配字符串中
字符组合的模式。在 JavaScript中，正则表达式也是对象。
正则表通常被用来检索、替换那些符合某个模式（规则）的
文本，例如验证表单：用户名表单只能输入英文字母、数字
或者下划线， 昵称输入框中可以输入中文(匹配)。此外，
正则表达式还常用于过滤掉页面内容中的一些敏感
词(替换)，或从字符串中获取我们想要的特定部分(提取)等 。
```
### 3.1.1 正则表达式在js在使用
```markdown
1 利用 RegExp对象来创建 正则表达式
var regexp = new RegExp(/123/);
2 利用字面量创建 正则表达式
var rg = /123/;
3 test 方法用来检测字符串是否符合正则表达式要求的规范
console.log(rg.test(123));
```
~~~html
 <script>
        // 正则表达式在js中的使用

        // 1. 利用 RegExp对象来创建 正则表达式
        var regexp = new RegExp(/123/);
        console.log(regexp);

        // 2. 利用字面量创建 正则表达式
        var rg = /123/;
        // 3.test 方法用来检测字符串是否符合正则表达式要求的规范
        console.log(rg.test(123));
        console.log(rg.test('abc'));
 </script>
~~~
### 3.1.2 正则表达式中的特殊字符
#### 3.1.2.1 边界符
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200517072151466.png)
```markdown
正则表达式里面不需要加引号 不管是数字型还是字符串型
```
```html
<script>
        // 边界符 ^ $ 
        var rg = /abc/; // 正则表达式里面不需要加引号 不管是数字型还是字符串型
        // /abc/ 只要包含有abc这个字符串返回的都是true
        console.log(rg.test('abc'));
        console.log(rg.test('abcd'));
        console.log(rg.test('aabcd'));
        console.log('---------------------------');
        var reg = /^abc/;
        console.log(reg.test('abc')); // true
        console.log(reg.test('abcd')); // true
        console.log(reg.test('aabcd')); // false
        console.log('---------------------------');
        var reg1 = /^abc$/; // 精确匹配 要求必须是 abc字符串才符合规范
        console.log(reg1.test('abc')); // true
        console.log(reg1.test('abcd')); // false
        console.log(reg1.test('aabcd')); // false
        console.log(reg1.test('abcabc')); // false
    </script>
```
#### 3.1.2.2 字符类
```markdown
[] 表示有一系列字符可供选择，只要匹配其中一个就可以了
[-] 方括号内部加上 - 表示范围
[^] 方括号内部 取反符^ 
```
```html
<script>
        //var rg = /abc/;  只要包含abc就可以 
        // 字符类: [] 表示有一系列字符可供选择，只要匹配其中一个就可以了
        var rg = /[abc]/; // 只要包含有a 或者 包含有b 或者包含有c 都返回为true
        console.log(rg.test('andy'));
        console.log(rg.test('baby'));
        console.log(rg.test('color'));
        console.log(rg.test('red'));
        var rg1 = /^[abc]$/; // 三选一 只有是a 或者是 b  或者是c 这三个字母才返回 true
        console.log(rg1.test('aa'));
        console.log(rg1.test('a'));
        console.log(rg1.test('b'));
        console.log(rg1.test('c'));
        console.log(rg1.test('abc'));
        console.log('------------------');

        var reg = /^[a-z]$/; // 26个英文字母任何一个字母返回 true  - 表示的是a 到z 的范围  
        console.log(reg.test('a'));
        console.log(reg.test('z'));
        console.log(reg.test(1));
        console.log(reg.test('A'));
        // 字符组合
        var reg1 = /^[a-zA-Z0-9_-]$/; // 26个英文字母(大写和小写都可以)任何一个字母返回 true  
        console.log(reg1.test('a'));
        console.log(reg1.test('B'));
        console.log(reg1.test(8));
        console.log(reg1.test('-'));
        console.log(reg1.test('_'));
        console.log(reg1.test('!'));
        console.log('----------------');
        // 如果中括号里面有^ 表示取反的意思 千万和 我们边界符 ^ 别混淆
        var reg2 = /^[^a-zA-Z0-9_-]$/;
        console.log(reg2.test('a'));
        console.log(reg2.test('B'));
        console.log(reg2.test(8));
        console.log(reg2.test('-'));
        console.log(reg2.test('_'));
        console.log(reg2.test('!'));
    </script>
```
#### 3.1.2.3 量词
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200517074326390.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```html
  <script>
        // 量词符: 用来设定某个模式出现的次数
        // var reg = /^a$/;
        // console.log(reg.test('a'));
        // console.log(reg.test('aa'));


        // 1. * 相当于 >= 0 可以出现0次或者很多次 
        // var reg = /^a*$/;
        // console.log(reg.test(''));
        // console.log(reg.test('a'));
        // console.log(reg.test('aa'));
        // console.log(reg.test('aaaaaa'));

        // 2. + 相当于 >= 1 可以出现1次或者很多次
        // var reg = /^a+$/;
        // console.log(reg.test(''));
        // console.log(reg.test('a'));
        // console.log(reg.test('aa'));
        // console.log(reg.test('aaaaaa'));
        // 3. ?  相当于 1 || 0
        // var reg = /^a?$/;
        // console.log(reg.test(''));
        // console.log(reg.test('a'));
        // console.log(reg.test('aa'));
        // console.log(reg.test('aaaaaa'));
        // 4. {3 } 就是重复3次
        // var reg = /^a{3}$/;
        // console.log(reg.test(''));
        // console.log(reg.test('a'));
        // console.log(reg.test('aa'));
        // console.log(reg.test('aaaaaa'));
        // console.log(reg.test('aaa'));
        // 5. {3, }  大于等于3
        var reg = /^a{3,}$/;
        console.log(reg.test(''));
        console.log(reg.test('a'));
        console.log(reg.test('aa'));
        console.log(reg.test('aaaaaa'));
        console.log(reg.test('aaa'));
        // 6. {3, 16}  大于等于3 并且 小于等于16

        var reg = /^a{3,16}$/;
        console.log(reg.test(''));
        console.log(reg.test('a'));
        console.log(reg.test('aa'));
        console.log(reg.test('aaaaaa'));
        console.log(reg.test('aaa'));
        console.log(reg.test('aaaaaaaaaaaaaaaaaaaaa'));
    </script>
```
#### 3.1.2.4 用户名验证
```markdown
功能需求:
1. 如果用户名输入合法, 则后面提示信息为 : 用户名合
法,并且颜色为绿色
2. 如果用户名输入不合法, 则后面提示信息为: 用户
名不符合规范, 并且颜色为绿色

分析:
3. 用户名只能为英文字母,数字,下划线或者短横线组
成, 并且用户名长度为 6~16位.
4. 首先准备好这种正则表达式模式 /$[a-zA-Z0-9-_]{6,16}^/
5. 当表单失去焦点就开始验证.
6. 如果符合正则规范, 则让后面的span标签添加 right 类.
7. 如果不符合正则规范, 则让后面的span标签添加 wrong 类.
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
        span {
            color: #aaa;
            font-size: 14px;
        }
        
        .right {
            color: green;
        }
        
        .wrong {
            color: red;
        }
    </style>
</head>

<body>
    <input type="text" class="uname"> <span>请输入用户名</span>
    <script>
        //  量词是设定某个模式出现的次数
        var reg = /^[a-zA-Z0-9_-]{6,16}$/; // 这个模式用户只能输入英文字母 数字 下划线 短横线但是有边界符和[] 这就限定了只能多选1
        // {6,16}  中间不要有空格
        // console.log(reg.test('a'));
        // console.log(reg.test('8'));
        // console.log(reg.test('18'));
        // console.log(reg.test('aa'));
        // console.log('-------------');
        // console.log(reg.test('andy-red'));
        // console.log(reg.test('andy_red'));
        // console.log(reg.test('andy007'));
        // console.log(reg.test('andy!007'));
        var uname = document.querySelector('.uname');
        var span = document.querySelector('span');
        uname.onblur = function() {
            if (reg.test(this.value)) {
                console.log('正确的');
                span.className = 'right';
                span.innerHTML = '用户名格式输入正确';
            } else {
                console.log('错误的');
                span.className = 'wrong';
                span.innerHTML = '用户名格式输入不正确';
            }
        }
    </script>
</body>

</html>
```
#### 3.1.2.5 括号总结
```markdown
1. 大括号{} 量词符. 里面表示重复次数
2. 中括号[] 字符集合。匹配方括号中的任意字符.
3. 小括号 ()表示优先级
```
##### 3.1.2.5.1 预定义类
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200517082006769.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 3.1.2.5.2 表单验证
```js
reg.js

window.onload = function() {
    var regtel = /^1[3|4|5|7|8]\d{9}$/; // 手机号码的正则表达式
    var regqq = /^[1-9]\d{4,}$/; // 10000
    var regnc = /^[\u4e00-\u9fa5]{2,8}$/;
    var regmsg = /^\d{6}$/;
    var regpwd = /^[a-zA-Z0-9_-]{6,16}$/;
    var tel = document.querySelector('#tel');
    var qq = document.querySelector('#qq');
    var nc = document.querySelector('#nc');
    var msg = document.querySelector('#msg');
    var pwd = document.querySelector('#pwd');
    var surepwd = document.querySelector('#surepwd');
    regexp(tel, regtel); // 手机号码
    regexp(qq, regqq); // qq号码
    regexp(nc, regnc); // 昵称
    regexp(msg, regmsg); // 短信验证
    regexp(pwd, regpwd); // 密码框
    // 表单验证的函数
    function regexp(ele, reg) {
        ele.onblur = function() {
            if (reg.test(this.value)) {
                // console.log('正确的');
                this.nextElementSibling.className = 'success';
                this.nextElementSibling.innerHTML = '<i class="success_icon"></i> 恭喜您输入正确';
            } else {
                // console.log('不正确');
                this.nextElementSibling.className = 'error';
                this.nextElementSibling.innerHTML = '<i class="error_icon"></i> 格式不正确，请从新输入 ';
            }
        }
    };

    surepwd.onblur = function() {
        if (this.value == pwd.value) {
            this.nextElementSibling.className = 'success';
            this.nextElementSibling.innerHTML = '<i class="success_icon"></i> 恭喜您输入正确';
        } else {
            this.nextElementSibling.className = 'error';
            this.nextElementSibling.innerHTML = '<i class="error_icon"></i> 两次密码输入不一致';

        }
    }

}

register.html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>注册页面</title>
    <!-- 初始化css -->
    <link rel="stylesheet" href="css/base.css">
    <!-- register css文件 -->
    <link rel="stylesheet" href="css/register.css">
    <script src="js/reg.js"></script>
</head>

<body>
    <div class="w">
        <!-- header -->
        <div class="header">
            <div class="logo">
                <a href="index.html">
                    <img src="img/logo.png" alt="">
                </a>
            </div>
        </div>
        <!-- registerarea -->
        <div class="registerarea">
            <h3>
                注册新用户
                <em>
					我有账号，去<a href="login.html">登陆</a>
				</em>
            </h3>
            <div class="reg_form">

                <form action="demo.php">
                    <ul>
                        <li>
                            <label for="tel">手机号:</label>
                            <input type="text" class="inp" id="tel">
                            <span class="">		
						
						</span>
                        </li>
                        <li>
                            <label for="">QQ:</label>
                            <input type="text" class="inp" id="qq">
                            <span></span>
                        </li>
                        <li>
                            <label for="">昵称:</label>
                            <input type="text" class="inp" id="nc">
                            <span></span>
                        </li>
                        <li>
                            <label for="">短信验证码:</label>
                            <input type="text" class="inp" id="msg">
                            <span></span>
                        </li>
                        <li>
                            <label for="">登陆密码:</label>
                            <input type="text" class="inp" id="pwd">
                            <span>
                            </span>
                        </li>
                        <li class="safe">
                            安全程度
                            <em class="ruo">弱</em>
                            <em class="zhong">中</em>
                            <em class="qiang">强</em>
                        </li>

                        <li>
                            <label for="">确认密码:</label>
                            <input type="text" class="inp" id="surepwd">
                            <span></span>
                        </li>
                        <li class="agree">

                            <input type="checkbox">同意协议并注册
                            <a href="#">《知果果用户协议》</a>
                        </li>
                        <li>
                            <input type="submit" value="完成注册" class="over">
                        </li>
                    </ul>
                </form>

            </div>
        </div>
        <div class="footer">
            <p class="links">
                关于我们 | 联系我们 | 联系客服 | 商家入驻 | 营销中心 | 手机品优购 | 友情链接 | 销售联盟 | 品优购社区 | 品优购公益 | English Site | Contact U
            </p>

            <p class="copyright">
                地址：北京市昌平区建材城西路金燕龙办公楼一层 邮编：100096 电话：400-618-4000 传真：010-82935100 邮箱: zhanghj+itcast.cn <br> 京ICP备08001421号京公网安备110108007702
            </p>
        </div>
    </div>
</body>

</html>
```

#### 3.1.2.6 替换
```markdown
stringObject.replace(regexp/substr,replacement)
1 第一个参数: 被替换的字符串 或者 正则表达式
2 第二个参数: 替换为的字符串
3 返回值是一个替换完毕的新字符串
正则表达式参数
switch(也称为修饰符) 按照什么样的模式来匹配. 有三种值：
g：全局匹配
i：忽略大小写
gi：全局匹配 + 忽略大小写
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
        textarea {
            width: 300px;
            height: 100px;
            border: 1px solid #ccc;
        }
    </style>
</head>

<body>
    <textarea name="" id="message"></textarea> <button>提交</button>
    <div></div>
    <script>
        // 替换 replace
        // var str = 'andy和red';
        // // var newStr = str.replace('andy', 'baby');
        // var newStr = str.replace(/andy/, 'baby');
        // console.log(newStr);
        var text = document.querySelector('textarea');
        var btn = document.querySelector('button');
        var div = document.querySelector('div');
        btn.onclick = function() {
            div.innerHTML = text.value.replace(/激情|gay/g, '**');
        }
    </script>
</body>

</html>
```
## 4.1 ES6
```markdown
ES 的全称是 ECMAScript , 它是由 ECMA 国际标准化
组织,制定的一项脚本语言的标准化规范。
```
### 4.1.1 使用let关键字声明变量
```markdown
使用let关键字声明的变量才具有块级作用域，不存在
变量提升,暂时性死区,使用var声明的变量不具备块级作用域特性.
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>使用let关键字声明变量</title>
</head>
<body>
	<script type="text/javascript">
		/*
			let关键字就是用来声明变量的

			使用let关键字声明的变量具有块级作用域

			在一个大括号中 使用let关键字声明的变量才具有块级作用域 var关键字是不具备这个特点的

			防止循环变量变成全局变量

			使用let关键字声明的变量没有变量提升

			使用let关键字声明的变量具有暂时性死区特性

		*/
		
		/* --------let关键字就是用来声明变量的-------- */
		// let a = 10;
		// console.log(a);
		
		/* --------使用let关键字声明的变量具有块级作用域-------- */
		// if (true) {
		// 	let b = 20;
		// 	console.log(b)
		// 	if (true) {
		// 		let c = 30;
		// 	}
		// 	console.log(c);
		// }
		// console.log(b) //报错
		
		/* -------在一个大括号中 使用let关键字声明的变量才具有块级作用域 var关键字是不具备这个特点的--------- */

		// if (true) {
		// 	let num = 100;
		// 	var abc = 200;
		// }
		// console.log(abc); //报错
		// console.log(num)


		/* -------防止循环变量变成全局变量--------- */
		// for (let i = 0; i < 2; i++) {}
		// console.log(i);//报错
		

		/*-----使用let关键字声明的变量没有变量提升------*/
		// console.log(a); //报错
		// let a = 100; 
		

		/* -------使用let关键字声明的变量具有暂时性死区特性------- */
		var num = 10
		if (true) {
			console.log(num); //报错
			let num = 20;
		}



	</script>
</body>
</html>
```
```html
经典面试题
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>经典面试题</title>
</head>
<body>
	<script type="text/javascript">
		let arr = [];

		for (let i = 0; i < 2; i++) {
			arr[i] = function () {
				console.log(i);
			}
		}

		arr[0](); 0 
		arr[1](); 1
//如果把前面的let i 改为var i，则结果全为2.因为var i 此时为全局变量
	
	</script>
</body>
</html>
```
### 4.1.2 使用const关键字声明常量
```markdown
具有块级作用域
声明常量时必须赋值
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>使用const关键字声明常量</title>
</head>
<body>
	<script type="text/javascript">
		// 使用const关键字声明的常量具有块级作用域
		// if (true) {
		// 	const a = 10;
		// 	if (true) {
		// 		const a = 20;
		// 		console.log(a); //20
		// 	}
		// 	console.log(a); //10
		// }
		// console.log(a); //报错
		
		// 使用const关键字声明的常量必须赋初始值
		// const PI = 3.14;
		
		// 常量声明后值不可更改 
		const PI = 3.14;
		// PI = 100;
		const ary = [100, 200];
		ary[0] = 123; //可以为数组的成员赋值,但是不可以为数组赋值
		ary = [1, 2] 
		console.log(ary); //报错
	</script>
</body>
</html>
```
### 4.1.3 let、const、var 的区别
```markdown
1. 使用 var 声明的变量，其作用域为该语句所在的函数
内，且存在变量提升现象。
2. 使用 let 声明的变量，其作用域为该语句所在的代码
块内，不存在变量提升。
3. 使用 const 声明的是常量，在后面出现的代码中不
能再修改该常量的值
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200517151020373.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 4.1.4 解构赋值
```markdown
按照一定模式，从数组中或对象中提取值，将提取出来的
值赋值给另外的变量。
```
#### 4.1.4.1 数组解构
```markdown
ES6中允许从数组中提取值，按照对应位置，对变量赋值。对
象也可以实现解构。
如果解构不成功，变量的值为undefined。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>数组解构</title>
</head>
<body>
	<script type="text/javascript">
		// 数组解构允许我们按照一一对应的关系从数组中提取值 然后将值赋值给变量
		let ary = [1,2,3];
		let [a, b, c, d, e] = ary;
		console.log(a) //1
		console.log(b) //2
		console.log(c) //3
		console.log(d) //undefined
		console.log(e) //undefined
	</script>
</body>
</html>
```
#### 4.1.4.2 对象解构
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>对象解构</title>
</head>
<body>
	<script type="text/javascript">
		// 对象解构允许我们使用变量的名字匹配对象的属性 匹配成功 将对象属性的值赋值给变量
		
		let person = {name: 'lisi', age: 30, sex: '男'};
		// let { name, age, sex } = person;
		// console.log(name) //lisi
		// console.log(age) //30
		// console.log(sex) //男
		
		let {name: myName} = person; // myName 属于别名
		console.log(myName) //'lisi'

	</script>
</body>
</html>
```
### 4.1.5 箭头函数
```markdown
函数体中只有一句代码，且代码的执行结果就是返回值，可
以省略大括号
如果形参只有一个，可以省略小括号
箭头函数不绑定this关键字，箭头函数中的this，指向的是
函数定义位置的上下文this。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>箭头函数</title>
</head>
<body>
	<script type="text/javascript">
		// 箭头函数是用来简化函数定义语法的
		// const fn = () => {
		// 	console.log(123)
		// }
		// fn();
		
		// 在箭头函数中 如果函数体中只有一句代码 并且代码的执行结果就是函数的返回值 函数体大括号可以省略
		// const sum = (n1, n2) => n1 + n2;	 
		// const result = sum(10, 20);
		// console.log(result)
		
		// 在箭头函数中 如果形参只有一个 形参外侧的小括号也是可以省略的
		// const fn = v => {
		// 	alert(v);
		// }
		// fn(20)
		
		// 箭头函数不绑定this 箭头函数没有自己的this关键字 如果在箭头函数中使用this this关键字将指向箭头函数定义位置中的this
		
		function fn () {
			console.log(this);
			return () => {
				console.log(this)
			}
		}

		const obj = {name: 'zhangsan'};

		const resFn = fn.call(obj);

		resFn();
	</script>
</body>
</html>
```
```html
箭头函数面试题
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>箭头函数面试题</title>
</head>
<body>
	<script type="text/javascript">
	
		var age = 100;

		var obj = {
			age: 20,
			say: () => {
				alert(this.age) //100,这里的this指的是window
			}
			}
		}

		obj.say();
	</script>
</body>
</html>
```
### 4.1.6 剩余参数
```markdown
剩余参数语法允许我们将一个不定数量的参数表示为一个数组。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>剩余参数</title>
</head>
<body>
	<script type="text/javascript">
		// const sum = (...args) => {
		// 	let total = 0;
		// 	args.forEach(item => total += item);
		// 	return total;
		// };

		// console.log(sum(10, 20));
		// console.log(sum(10, 20, 30));
		

		let ary1 = ['张三' , '李四', '王五'];
		let [s1, ...s2] = ary1;
		console.log(s1)
		console.log(s2) //剩余参数语法允许我们将一个不定数量的参数表示为一个数组。

	</script>
</body>
</html>
```
### 4.1.7 扩展运算符
```markdown
扩展运算符可以将数组或者对象转为用逗号分隔的参数序列。
扩展运算符可以应用于合并数组。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>扩展运算符</title>
</head>
<body>
	<div>1</div>
	<div>4</div>
	<div>3</div>
	<div>6</div>
	<div>2</div>
	<div>5</div>
	<script type="text/javascript">
		// 扩展运算符可以将数组拆分成以逗号分隔的参数序列
		// let ary = ["a", "b", "c"];
		// ...ary // "a", "b", "c"
		// console.log(...ary)
		// console.log("a", "b", "c")
		
		// 扩展运算符应用于数组合并
		// let ary1 = [1, 2, 3];
		// let ary2 = [4, 5, 6];
		// // ...ary1 // 1, 2, 3
		// // ...ary1 // 4, 5, 6
		// let ary3 = [...ary1, ...ary2];
		// console.log(ary3)

		// 合并数组的第二种方法
		// let ary1 = [1, 2, 3];
		// let ary2 = [4, 5, 6];

		// ary1.push(...ary2);
		// console.log(ary1)
		
		// 利用扩展运算符将伪数组转换为真正的数组
		var oDivs = document.getElementsByTagName('div');
		console.log(oDivs)
		var ary = [...oDivs];
		ary.push('a');
		console.log(ary);
	</script>
</body>
</html>
```
### 4.1.8 Array
#### 4.1.8.1 Array.from方法
```js
伪数组或可遍历对象转换为真正的数组
let oDivs = document.getElementsByTagName('div');
oDivs = [...oDivs];

Array.from()
将类数组或可遍历对象转换为真正的数组
let arrayLike = {
 '0': 'a',
 '1': 'b',
 '2': 'c',
 length: 3
};
let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']

方法还可以接受第二个参数，作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组。
let arrayLike = {
 "0": 1,
 "1": 2,
 "length": 2
}
let newAry = Array.from(aryLike, item => item *2)
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Array.from方法</title>
</head>
<body>
	<script type="text/javascript">
		// var arrayLike = {
		// 	"0": "张三",
		// 	"1": "李四",
		// 	"2": "王五",
		// 	"length": 3
		// }

		// var ary = Array.from(arrayLike);
		// console.log(ary)
		
		var arrayLike = {
			"0": "1",
			"1": "2",
			"length": 2
		}

		var ary = Array.from(arrayLike, item => item * 2)
		console.log(ary)
	</script>
</body>
</html>
```
#### 4.1.8.2 Array.find方法介绍
```markdown
用于找出第一个符合条件的数组成员，如果没有找到返回undefined
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>find方法</title>
</head>
<body>
	<script type="text/javascript">
		var ary = [{
			id: 1,
			name: '张三'
		}, {
			id: 2,
			name: '李四'
		}];
		let target = ary.find(item => item.id == 3);
		console.log(target)
	</script>
</body>
</html>
```
#### 4.1.8.3 Array.findIndex方法
```markdown
用于找出第一个符合条件的数组成员的位置，如果没有找到返回-1
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>findIndex方法</title>
</head>
<body>
	<script type="text/javascript">
		let ary = [10, 20, 50];
		let index = ary.findIndex(item => item > 15);
		console.log(index)
	</script>
</body>
</html>
```
#### 4.1.8.4 Array.includes方法介绍
```markdown
表示某个数组是否包含给定的值，返回布尔值。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>includes方法</title>
</head>
<body>
	<script type="text/javascript">
		let ary = ["a", "b", "c"];

		let result = ary.includes('a')
		console.log(result)
		result = ary.includes('e')
		console.log(result)
	</script>
</body>
</html>
```
### 4.1.9 模板字符串
```markdown
ES6新增的创建字符串的方式，使用反引号定义。
模板字符串中可以解析变量。
模板字符串中可以换行
在模板字符串中可以调用函数。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>模板字符串</title>
</head>
<body>
	<script type="text/javascript">
		// let name = `张三`;
		// let sayHello = `Hello, 我的名字叫${name}`;
		// console.log(sayHello);
		
		// let result = {
		// 	name: "zhangsan",
		// 	age: 20
		// };
		// let html = `
		// 	<div>
		// 		<span>${result.name}</span>
		// 		<span>${result.age}</span>
		// 	</div>
		// `;
		// console.log(html);
		
		const fn = () => {
			return '我是fn函数'
		}

		let html = `我是模板字符串 ${fn()}`;
		console.log(html)

	</script>
</body>
</html>
```
### 4.1.10 startsWith方法和endsWith方法
```markdown
startsWith()：表示参数字符串是否在原字符串的头部，返回布尔值
endsWith()：表示参数字符串是否在原字符串的尾部，返回布尔值
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>startsWith方法和endsWith方法</title>
</head>
<body>
	<script type="text/javascript">
		let str = 'Hello ECMAScript 2015';
		let r1 = str.startsWith('Hello');
		console.log(r1);
		let r2 = str.endsWith('2016');
		console.log(r2)
	</script>
</body>
</html>
```
### 4.1.11 repeat方法介绍
```markdown
repeat方法表示将原字符串重复n次，返回一个新字符串。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>repeat方法</title>
</head>
<body>
	<script type="text/javascript">
		console.log("y".repeat(5))
	</script>
</body>
</html>
```
### 4.1.12 Set
```markdown
Set本身是一个构造函数，用来生成 Set 数据结构。
Set函数可以接受一个数组作为参数，用来初始化。
add(value)：添加某个值，返回 Set 结构本身
delete(value)：删除某个值，返回一个布尔值，表示删除是否成功
has(value)：返回一个布尔值，表示该值是否为 Set 的成员
clear()：清除所有成员，没有返回值
Set 结构的实例与数组一样，也拥有forEach方法，用
于对每个成员执行某种操作，没有返回值。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Set</title>
</head>
<body>
	<script type="text/javascript">
		// const s1 = new Set();
		// console.log(s1.size)

		// const s2 = new Set(["a", "b"]);
		// console.log(s2.size)

		// const s3 = new Set(["a","a","b","b"]);
		// console.log(s3.size)
		// const ary = [...s3];
		// console.log(ary)
		
		// const s4 = new Set();
		// 向set结构中添加值 使用add方法
		// s4.add('a').add('b');
		// console.log(s4.size)

		// 从set结构中删除值 用到的方法是delete
		// const r1 = s4.delete('c');
		// console.log(s4.size)
		// console.log(r1);

		// 判断某一个值是否是set数据结构中的成员 使用has
		// const r2 = s4.has('d');
		// console.log(r2)

		// 清空set数据结构中的值 使用clear方法
		// s4.clear();
		// console.log(s4.size);
		
		// 遍历set数据结构 从中取值
		const s5 = new Set(['a', 'b', 'c']);
		s5.forEach(value => {
			console.log(value)
		})

	</script>
</body>
</html>
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复
js高即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
