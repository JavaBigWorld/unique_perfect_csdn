# JavaScript基础
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714115147106.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1.1 认识JavaScript
![在这里插入图片描述](https://img-blog.csdnimg.cn/ec4cb8c2d09c40ba888b5faa8bbeb7f0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAdW5pcXVlX3BlcmZlY3Q=,size_17,color_FFFFFF,t_70,g_se,x_16)

## 1.2 JavaScript的特点
```markdown
解释性语言：经过解释器解释的语言
动态型语言：在编写代码的过程中，可以动态
修改属性。静态语言则在编写代码过程中不可以动态修改属性。
```
## 1.3 JavaScript编写位置
![在这里插入图片描述](https://img-blog.csdnimg.cn/0636e6f18a114345a35503e3568d3cae.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAdW5pcXVlX3BlcmZlY3Q=,size_17,color_FFFFFF,t_70,g_se,x_16)


```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style></style>
    <!-- 2.内嵌式的js -->
    <script>
        // alert('沙漠骆驼');
    </script>
    <!-- 3. 外部js script 双标签 -->
    <script src="my.js"></script>
</head>

<body>
    <!-- 1. 行内式的js 直接写到元素的内部 -->
    <!-- <input type="button" value="唐伯虎" onclick="alert('秋香姐')"> -->
</body>

</html>
```
## 1.4 JavaScript注释使用
~~~markdown
单行注释://
多行注释:/* */
~~~

## 1.5 JS输入输出语句
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 这是一个输入框
        prompt('请输入您的年龄');
        // alert 弹出警示框 输出的 展示给用户的
        alert('计算的结果是');
        // console 控制台输出 给程序员测试用的  
        console.log('我是程序员能看到的');
    </script>
</head>

<body>

</body>

</html>
```
## 1.6 如何定义变量

| 情况                           | 说明                    | 结果      |
| ------------------------------ | ----------------------- | --------- |
| var  age ; console.log (age);  | 只声明 不赋值           | undefined |
| console.log(age)               | 不声明 不赋值  直接使用 | 报错      |
| age   = 10; console.log (age); | 不声明   只赋值         | 10        |

~~~markdown
可以用var,也可以用let,但是开发中用let,因为代
码块{}没有块级作用域,只起分组的作用,但是用let则
代码块{}则有块级作用域
~~~
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1. 更新变量
        var myname = 'pink老师';
        console.log(myname);
        myname = '迪丽热巴';
        console.log(myname);
        // 2. 声明多个变量
        // var age = 18;
        // var address = '火影村';
        // var gz = 2000;
        var age = 18,
            address = '火影村',
            gz = 2000;
        // 3. 声明变量的特殊情况
        // 3.1 只声明不赋值 结果是？  程序也不知道里面存的是啥 所以结果是 undefined  未定义的
        var sex;
        console.log(sex); // undefined
        // 3.2  不声明 不赋值 直接使用某个变量会报错滴
        // console.log(tel);
        // 3.3 不声明直接赋值使用
        qq = 110;
        console.log(qq);
    </script>
</head>

<body>

</body>

</html>
```
## 1.7 基础的数据类型
~~~markdown
基础数据类型:Undefined,Boolean,Null,Number,String
引用数据类型:Object
注意:可以使用typeof 变量来检查数据的类型,但是返回
的是相应数据类型的小写形式,但是有例外,就是typeof null 的
数据类型不是null,而是object

注意javaScript不存在char类型,所以单引号跟双引号都
可以表示字符串
~~~
```html
变量的数据类型
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // int num = 10;  java 
        // var num; // 这里的num 我们是不确定属于哪种数据类型的
        var num = 10; // num 属于数字型 
        // js 的变量数据类型是只有程序在运行过程中，根据等号右边的值来确定的
        var str = 'pink'; // str 字符串型
        // js是动态语言 变量的数据类型是可以变化的
        var x = 10; // x 是数字型 
        x = 'pink'; // x 字符串型
    </script>
</head>

<body>

</body>

</html>

数字型Number
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        var num = 10; // num 数字型 
        var PI = 3.14 // PI 数字型
            // 1. 八进制  0 ~ 7  我们程序里面数字前面加0 表示八进制
        var num1 = 010;
        console.log(num1); //  010  八进制 转换为 10进制 就是  8 
        var num2 = 012;
        console.log(num2);
        // 2. 十六进制  0 ~ 9  a ~ f    #ffffff  数字的前面加 0x 表示十六进制
        var num3 = 0x9;
        console.log(num3);
        var num4 = 0xa;
        console.log(num4);
        // 3. 数字型的最大值
        console.log(Number.MAX_VALUE);
        // 4. 数字型的最小值
        console.log(Number.MIN_VALUE);
        // 5. 无穷大
        console.log(Number.MAX_VALUE * 2); // Infinity 无穷大  
        // 6. 无穷小
        console.log(-Number.MAX_VALUE * 2); // -Infinity 无穷大
        // 7. 非数字
        console.log('pink老师' - 100); // NaN
    </script>
</head>

<body>

</body>

</html>

isNaN
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // isNaN() 这个方法用来判断非数字   并且返回一个值 如果是数字返回的是 false 如果不是数字返回的是true
        console.log(isNaN(12)); // false
        console.log(isNaN('pink老师')); // true
    </script>
</head>

<body>

</body>

</html>

字符串型String1
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 'pink'   'pink老师'  '12'   'true'
        var str = '我是一个"高富帅"的程序员';
        console.log(str);
        // 字符串转义字符  都是用 \ 开头 但是这些转义字符写道引号里面
        var str1 = "我是一个'高富帅'的\n程序员";
        console.log(str1);
    </script>
</head>

<body>

</body>

</html>

字符串拼接
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1. 检测获取字符串的长度 length 
        var str = 'my name is andy';
        console.log(str.length); // 15
        // 2. 字符串的拼接 +  只要有字符串和其他类型相拼接 最终的结果是字符串类型
        console.log('沙漠' + '骆驼'); // 字符串的 沙漠骆驼
        console.log('pink老师' + 18); // 'pink老师18'
        console.log('pink' + true); // pinktrue
        console.log(12 + 12); // 24
        console.log('12' + 12); // '1212'
    </script>
</head>

<body>

</body>

</html>

字符串拼接加强
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        console.log('pink老师' + 18); // pink老师18
        console.log('pink老师' + 18 + '岁');
        var age = 19;
        console.log('pink老师age岁');
        // 我们变量不要写到字符串里面，是通过和 字符串相连的方式实现的
        console.log('pink老师' + age + '岁');
        // 变量和字符串相连的口诀：  引引加加
        console.log('pink老师' + age + '岁');
    </script>
</head>

<body>

</body>

</html>

布尔型Boolean
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        var flag = true; // flag 布尔型 
        var flag1 = false; // flag1 布尔型
        console.log(flag + 1); // true 参与加法运算当1来看
        console.log(flag1 + 1); // false 参与加法运算当 0来看
        // 如果一个变量声明未赋值 就是 undefined 未定义数据类型
        var str;
        console.log(str);
        var variable = undefined;
        console.log(variable + 'pink'); // undefinedpink
        console.log(variable + 1); // NaN  undefined 和数字相加 最后的结果是 NaN
        // null 空值
        var space = null;
        console.log(space + 'pink'); // nullpink
        console.log(space + 1); // 1
    </script>
</head>

<body>

</body>

</html>

获取变量数据类型
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        var num = 10;
        console.log(typeof num); // number
        var str = 'pink';
        console.log(typeof str); // string
        var flag = true;
        console.log(typeof flag); // boolean
        var vari = undefined;
        console.log(typeof vari); // undefined
        var timer = null;
        console.log(typeof timer); // object
        // prompt 取过来的值是 字符型的
        var age = prompt('请输入您的年龄');
        console.log(age);
        console.log(typeof age);
    </script>
</head>

<body>

</body>

</html>

字面量
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        console.log(18);
        console.log('18');
        console.log(true);
        console.log(undefined);
        console.log(null);
    </script>
</head>

<body>

</body>

</html>

```
### 1.7.1  数据类型转换
#### 1.7.1.1 转换为字符串
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200714222219122.png)
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1. 把数字型转换为字符串型 变量.toString()
        var num = 10;
        var str = num.toString();
        console.log(str);
        console.log(typeof str);
        // 2. 我们利用 String(变量)   
        console.log(String(num));
        // 3. 利用 + 拼接字符串的方法实现转换效果 隐式转换
        console.log(num + '');
    </script>
</head>

<body>

</body>

</html>
```
#### 1.7.1.2 转换为数字型
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020071422233172.png)
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // var age = prompt('请输入您的年龄');
        // 1. parseInt(变量)  可以把 字符型的转换为数字型 得到是整数
        // console.log(parseInt(age));
        console.log(parseInt('3.14')); // 3 取整
        console.log(parseInt('3.94')); // 3 取整
        console.log(parseInt('120px')); // 120 会去到这个px单位
        console.log(parseInt('rem120px')); // NaN
        // 2. parseFloat(变量) 可以把 字符型的转换为数字型 得到是小数 浮点数
        console.log(parseFloat('3.14')); // 3.14
        console.log(parseFloat('120px')); // 120 会去掉这个px单位
        console.log(parseFloat('rem120px')); // NaN
        // 3. 利用 Number(变量) 
        var str = '123';
        console.log(Number(str));
        console.log(Number('12'));
        // 4. 利用了算数运算 -  *  /  隐式转换
        console.log('12' - 0); // 12
        console.log('123' - '120');
        console.log('123' * 1);
    </script>
</head>

<body>

</body>

</html>

计算年龄案例
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 弹出一个输入框（prompt)，让用户输入出生年份 （用户输入）
        // 把用户输入的值用变量保存起来，然后用今年的年份减去变量值，结果就是现在的年龄  （程序内部处理）
        // 弹出警示框（alert) ， 把计算的结果输出 （输出结果）
        var year = prompt('请您输入您的出生年份');
        var age = 2018 - year; // year 取过来的是字符串型  但是这里用的减法 有隐式转换
        alert('您今年已经' + age + '岁了');
    </script>
</head>

<body>

</body>

</html>

简单加法器案例
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 先弹出第一个输入框，提示用户输入第一个值  保存起来
        // 再弹出第二个框，提示用户输入第二个值  保存起来
        // 把这两个值相加，并将结果赋给新的变量（注意数据类型转换）  
        // 弹出警示框（alert) ， 把计算的结果输出 （输出结果）
        var num1 = prompt('请您输入第一个值：');
        var num2 = prompt('请您输入第二个值：');
        var result = parseFloat(num1) + parseFloat(num2);
        alert('您的结果是:' + result);
    </script>
</head>

<body>

</body>

</html>
```

#### 1.7.1.3 转换为布尔型
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200714222751364.png)
```html
代表空、否定的值会被转换为 false ，
如 ''、0、NaN、null、undefined
其余值都会被转换为 true
console.log(Boolean('')); // false
console.log(Boolean(0)); // false
console.log(Boolean(NaN)); // false
console.log(Boolean(null)); // false
console.log(Boolean(undefined)); // false
console.log(Boolean('小白')); // true
console.log(Boolean(12)); // true
```
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        console.log(Boolean('')); // false
        console.log(Boolean(0)); // false
        console.log(Boolean(NaN)); // false
        console.log(Boolean(null)); // false
        console.log(Boolean(undefined)); // false
        console.log('------------------------------');
        console.log(Boolean('123'));
        console.log(Boolean('你好吗'));
        console.log(Boolean('我很好'));
    </script>
</head>

<body>

</body>

</html>
```
## 1.8 运算符
~~~markdown
基本跟java语法一样.
注意:如果比较相不相等是,用===或者!==来表示,不
要用==或者!=来表示,因为==或者!=会自动进行类型
转换,类型不相同时,但是文本值相同,则会变成true
~~~
### 1.8.1 算术运算符
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200714223652776.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        console.log(1 + 1); // 2
        console.log(1 - 1); // 0
        console.log(1 * 1); // 1
        console.log(1 / 1); // 1
        // 1. % 取余 （取模）  
        console.log(4 % 2); // 0
        console.log(5 % 3); // 2
        console.log(3 % 5); // 3
        // 2. 浮点数 算数运算里面会有问题
        console.log(0.1 + 0.2); // 0.30000000000000004
        console.log(0.07 * 100); // 7.000000000000001
        // 3. 我们不能直接拿着浮点数来进行相比较 是否相等
        var num = 0.1 + 0.2;
        console.log(num == 0.3); // false
    </script>
</head>

<body>

</body>

</html>
```

### 1.8.2 比较运算符
```markdown
概念：比较运算符（关系运算符）是两个数据进行比
较时所使用的运算符，比较运算后，会返回一个布尔
值（true / false）作为比较运算的结果。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200714223920733.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        console.log(3 >= 5); // false
        console.log(2 <= 4); // true
        //1. 我们程序里面的等于符号 是 ==  默认转换数据类型 会把字符串型的数据转换为数字型 只要求值相等就可以
        console.log(3 == 5); // false
        console.log('pink老师' == '刘德华'); // flase
        console.log(18 == 18); // true
        console.log(18 == '18'); // true
        console.log(18 != 18); // false
        // 2. 我们程序里面有全等 一模一样  要求 两侧的值 还有 数据类型完全一致才可以 true
        console.log(18 === 18);
        console.log(18 === '18'); // false
    </script>
</head>

<body>

</body>

</html>
```

### 1.8.3  逻辑运算符
```markdown
逻辑与&& 两边都是 true才返回 true，否则返回 false
逻辑或 || 两边都为 false 才返回 false，否则都为true
逻辑非 ！ 逻辑非（!）也叫作取反符，用来取一个布
尔值相反的值，如 true 的相反值是 false
```


```html
逻辑运算符
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1. 逻辑与 &&  and 两侧都为true  结果才是 true  只要有一侧为false  结果就为false 
        console.log(3 > 5 && 3 > 2); // false
        console.log(3 < 5 && 3 > 2); // true
        // 2. 逻辑或 || or  两侧都为false  结果才是假 false  只要有一侧为true  结果就是true
        console.log(3 > 5 || 3 > 2); // true 
        console.log(3 > 5 || 3 < 2); // false
        // 3. 逻辑非  not  ！ 
        console.log(!true); // false
    </script>
</head>

<body>

</body>

</html>

短路运算（逻辑中断）
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1. 用我们的布尔值参与的逻辑运算  true && false  == false 
        // 2. 123 && 456  是值 或者是 表达式 参与逻辑运算？ 
        // 3. 逻辑与短路运算  如果表达式1 结果为真 则返回表达式2  如果表达式1为假 那么返回表达式1
        console.log(123 && 456); // 456
        console.log(0 && 456); //  0
        console.log(0 && 1 + 2 && 456 * 56789); // 0
        console.log('' && 1 + 2 && 456 * 56789); // ''
        // 如果有空的或者否定的为假 其余是真的  0  ''  null undefined  NaN
        // 4. 逻辑或短路运算  如果表达式1 结果为真 则返回的是表达式1 如果表达式1 结果为假 则返回表达式2
        console.log(123 || 456); // 123
        console.log(123 || 456 || 456 + 123); // 123
        console.log(0 || 456 || 456 + 123); // 456
        // 逻辑中断很重要 它会影响我们程序运行结果思密达
        var num = 0;
        console.log(123 || num++);
        console.log(num); // 0
    </script>
</head>

<body>

</body>

</html>


```
### 1.8.4 赋值运算符
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200714224457838.png)


```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        var num = 10;
        // num = num + 1;   num++
        // num = num + 2; // num += 2;
        // num += 2;
        num += 5;
        console.log(num);
        var age = 2;
        age *= 3;
        console.log(age);
    </script>
</head>

<body>

</body>

</html>
```
### 1.8.5 运算符优先级
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200714224611184.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        //  ++num   !num     2 + 3
        console.log(4 >= 6 || '人' != '阿凡达' && !(12 * 2 == 144) && true)
        var num = 10;
        console.log(5 == num / 2 && (2 + 2 * num).toString() === '22');
        console.log('-------------------');
        var a = 3 > 5 && 2 < 7 && 3 == 4;
        console.log(a);

        var b = 3 <= 4 || 3 > 1 || 3 != 2;
        console.log(b);

        var c = 2 === "2";
        console.log(c);

        var d = !c || b && a;
        console.log(d);
    </script>
</head>

<body>

</body>

</html>
```
## 1.9 流程控制
```markdown
if 语句
// 条件成立执行代码，否则什么也不做
if (条件表达式) {
 // 条件成立执行的代码语句
}

if else语句
// 条件成立 执行 if 里面代码，否则执行else 里面的代码
if (条件表达式) {
 // [如果] 条件成立执行的代码
} else {
 // [否则] 执行的代码
}

if else if 语句
// 适合于检查多重条件。
if (条件表达式1) {
 语句1；
} else if (条件表达式2) {
 语句2；
} else if (条件表达式3) {
 语句3；
....
} else {
 // 上述条件都不成立执行此处代码
}

三元表达式
表达式1 ? 表达式2 : 表达式3;

switch 语句
switch( 表达式 ){
 case value1:
 // 表达式 等于 value1 时要执行的代码
 break;
 case value2:
 // 表达式 等于 value2 时要执行的代码
 break;
 default:
 // 表达式 不等于任何一个 value 时要执行的代码
}
```
```html
if分支语句
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1. if 的语法结构   如果if
        // if (条件表达式) {
        //     // 执行语句
        // }

        // 2. 执行思路  如果 if 里面的条件表达式结果为真 true 则执行大括号里面的 执行语句 
        // 如果if 条件表达式结果为假 则不执行大括号里面的语句 则执行if 语句后面的代码
        // 3. 代码体验
        if (3 < 5) {
            alert('沙漠骆驼');
        }
    </script>
</head>

<body>

</body>

</html>


if else双分支语句
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1. 语法结构  if 如果  else 否则
        // if (条件表达式) {
        //     // 执行语句1
        // } else {
        //     // 执行语句2 
        // }
        // 2. 执行思路 如果表达式结果为真 那么执行语句1  否则  执行语句2
        // 3. 代码验证
        var age = prompt('请输入您的年龄:');
        if (age >= 18) {
            alert('我想带你去网吧偷耳机');
        } else {
            alert('滚， 回家做作业去');
        }
        // 5. if里面的语句1 和 else 里面的语句2 最终只能有一个语句执行  2选1
        // 6.  else 后面直接跟大括号
    </script>
</head>

<body>

</body>

</html>


if else if多分支语句
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1. 多分支语句   就是利用多个条件来选择不同的语句执行 得到不同的结果  多选1 的过程
        // 2. if else if语句是多分支语句
        // 3. 语法规范
        if (条件表达式1) {
            // 语句1;
        } else if (条件表达式2) {
            // 语句2;
        } else if (条件表达式3) {
            // 语句3;
        } else {
            // 最后的语句;
        }
        // 4. 执行思路
        // 如果条件表达式1 满足就执行 语句1 执行完毕后，退出整个if 分支语句  
        // 如果条件表达式1 不满足，则判断条件表达式2  满足的话，执行语句2 以此类推
        // 如果上面的所有条件表达式都不成立，则执行else 里面的语句
        // 5. 注意点
        // (1) 多分支语句还是多选1 最后只能有一个语句执行
        // (2) else if 里面的条件理论上是可以任意多个的
        // (3) else if 中间有个空格了
    </script>
</head>

<body>

</body>

</html>

三元表达式
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1. 有三元运算符组成的式子我们称为三元表达式
        // 2. ++num     3 + 5     ? :
        // 3. 语法结构 
        // 条件表达式 ？ 表达式1 ： 表达式2
        // 4. 执行思路
        // 如果条件表达式结果为真 则 返回 表达式1 的值 如果条件表达式结果为假 则返回 表达式2 的值
        // 5. 代码体验
        var num = 10;
        var result = num > 5 ? '是的' : '不是的'; // 我们知道表达式是有返回值的
        console.log(result);
        // if (num > 5) {
        //     result = '是的';
        // } else {
        //     result = '不是的';
        // }
    </script>
</head>

<body>

</body>

</html>


switch分支语句
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1. switch 语句也是多分支语句 也可以实现多选1
        // 2. 语法结构 switch 转换、开关  case 小例子或者选项的意思
        // switch (表达式) {
        //     case value1:
        //         执行语句1;
        //         break;
        //     case value2:
        //         执行语句2;
        //         break;
        //         ...
        //         default:
        //             执行最后的语句;
        // }
        // 3. 执行思路  利用我们的表达式的值 和 case 后面的选项值相匹配 如果匹配上，就执行该case 里面的语句  如果都没有匹配上，那么执行 default里面的语句
        // 4. 代码验证
        switch (8) {
            case 1:
                console.log('这是1');
                break;
            case 2:
                console.log('这是2');
                break;
            case 3:
                console.log('这是3');
                break;
            default:
                console.log('没有匹配结果');

        }
    </script>
</head>

<body>

</body>

</html>

switch注意事项
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // switch注意事项
        var num = 1;
        switch (num) {
            case 1:
                console.log(1);

            case 2:
                console.log(2);

            case 3:
                console.log(3);
                break;


        }
        // 1. 我们开发里面 表达式我们经常写成变量
        // 2. 我们num 的值 和 case 里面的值相匹配的时候是 全等   必须是值和数据类型一致才可以 num === 1
        // 3. break 如果当前的case里面没有break 则不会退出switch 是继续执行下一个case
    </script>
</head>

<body>

</body>

</html>
```
## 1.10 循环
```markdown
for循环
for(初始化变量; 条件表达式; 操作表达式 ){
    //循环体
}

while循环
while (条件表达式) {
}

do-while循环
do {
    // 循环体代码 - 条件表达式为 true 时重复执行循环体代码
} while(条件表达式);

continue、break
continue 关键字用于立即跳出本次循环，继续下一次
循环（本次循环体中 continue 之后的代码就会少执行一次）。
break 关键字用于立即跳出整个循环（循环结束）
```
```html
for循环
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1. for 重复执行某些代码， 通常跟计数有关系
        // 2. for 语法结构
        // for (初始化变量; 条件表达式; 操作表达式) {
        //     // 循环体
        // }
        // 3. 初始化变量 就是用var 声明的一个普通变量， 通常用于作为计数器使用 
        // 4. 条件表达式 就是用来决定每一次循环是否继续执行 就是终止的条件
        // 5. 操作表达式 是每次循环最后执行的代码 经常用于我们计数器变量进行更新（递增或者递减）
        // 6. 代码体验 我们重复打印100局 你好
        for (var i = 1; i <= 100; i++) {
            console.log('你好吗');
        }
    </script>
</head>

<body>

</body>

</html>


while循环
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1. while 循环语法结构  while 当...的时候
        // while (条件表达式) {
        //     // 循环体
        // }
        // 2. 执行思路  当条件表达式结果为true 则执行循环体 否则 退出循环
        // 3. 代码验证
        var num = 1;
        while (num <= 100) {
            console.log('好啊有');
            num++;
        }
        // 4. 里面应该也有计数器 初始化变量
        // 5. 里面应该也有操作表达式  完成计数器的更新 防止死循环
    </script>
</head>

<body>

</body>

</html>

do whild循环
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1.do while 循环 语法结构
        do {
            // 循环体
        } while (条件表达式)
        // 2.  执行思路 跟while不同的地方在于 do while 先执行一次循环体 在判断条件 如果条件表达式结果为真，则继续执行循环体，否则退出循环
        // 3. 代码验证
        var i = 1;
        do {
            console.log('how are you?');
            i++;
        } while (i <= 100)
        // 4. 我们的do while 循环体至少执行一次
    </script>
</head>

<body>

</body>

</html>

continue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // continue 关键字   退出本次（当前次的循环）  继续执行剩余次数循环
        for (var i = 1; i <= 5; i++) {
            if (i == 3) {
                continue; // 只要遇见 continue就退出本次循环 直接跳到 i++
            }
            console.log('我正在吃第' + i + '个包子');

        }
        // 1. 求1~100 之间， 除了能被7整除之外的整数和 
        var sum = 0;
        for (var i = 1; i <= 100; i++) {
            if (i % 7 == 0) {
                continue;
            }
            sum += i;
        }
        console.log(sum);
    </script>
</head>

<body>

</body>

</html>

break
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // break 退出整个循环
        for (var i = 1; i <= 5; i++) {
            if (i == 3) {
                break;
            }
            console.log('我正在吃第' + i + '个包子');

        }
    </script>
</head>

<body>

</body>

</html>
```
## 2.1 数组
```markdown
数组的创建方式
利用 new 创建数组
var 数组名 = new Array() ；
利用数组字面量创建数组
var 数组名 = []；

获取数组中的元素
// 定义数组
var arrStus = [1,2,3];
// 获取数组中的第2个元素
alert(arrStus[1]); 

遍历数组
var arr = ['red','green', 'blue'];
for(var i = 0; i < arr.length; i++){
 console.log(arrStus[i]);
}

数组的长度
数组名.length

新增元素
通过修改 length 长度新增数组元素
var arr = ['red', 'green', 'blue', 'pink'];
arr.length = 7;
console.log(arr);
console.log(arr[4]);
console.log(arr[5]);
console.log(arr[6]);

通过修改数组索引新增数组元素
不能直接给数组名赋值，否则会覆盖掉以前的数据
var arr = ['red', 'green', 'blue', 'pink'];
arr[4] = 'hotpink';
console.log(arr);
```
```html
数组的使用
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1.数组(Array) ：就是一组数据的集合 存储在单个变量下的优雅方式 
        // 2. 利用new 创建数组
        var arr = new Array(); // 创建了一个空的数组
        // 3. 利用数组字面量创建数组 []
        var arr = []; // 创建了一个空的数组
        var arr1 = [1, 2, 'pink老师', true];
        // 4. 我们数组里面的数据一定用逗号分隔
        // 5. 数组里面的数据 比如1,2， 我们称为数组元素
        // 6. 获取数组元素  格式 数组名[索引号]  索引号从 0开始 
        console.log(arr1);
        console.log(arr1[2]); // pink老师
        console.log(arr1[3]); // true
        var arr2 = ['迪丽热巴', '古丽扎娜', '佟丽丫丫'];
        console.log(arr2[0]);
        console.log(arr2[1]);
        console.log(arr2[2]);
        console.log(arr2[3]); // 因为没有这个数组元素 所以输出的结果是 undefined
    </script>
</head>

<body>

</body>

</html>

遍历数组
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 遍历数组：就是把数组的元素从头到尾访问一次
        var arr = ['red', 'green', 'blue'];
        for (var i = 0; i < 3; i++) {
            console.log(arr[i]);
        }
        // 1. 因为我们的数组索引号从0开始 ，所以 i 必须从 0开始  i < 3
        // 2. 输出的时候 arr[i]  i 计数器当索引号来用
    </script>
</head>

<body>

</body>

</html>

数组长度
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 数组长度 数组名.length
        var arr = ['关羽', '张飞', '马超', '赵云', '黄忠', '刘备', '姜维', 'pink'];
        for (var i = 0; i < 7; i++) {
            console.log(arr[i]);
        }
        console.log(arr.length);
        for (var i = 0; i < arr.length; i++) {
            console.log(arr[i]);
        }
        // 1. 数组的长度是元素个数  不要跟索引号混淆
        // 2. arr.length 动态监测数组元素的个数
    </script>
</head>

<body>

</body>

</html>

新增数组元素
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1. 新增数组元素 修改length长度 
        var arr = ['red', 'green', 'blue'];
        console.log(arr.length);
        arr.length = 5; // 把我们数组的长度修改为了 5  里面应该有5个元素 
        console.log(arr);
        console.log(arr[3]); // undefined
        console.log(arr[4]); // undefined

        // 2. 新增数组元素 修改索引号 追加数组元素
        var arr1 = ['red', 'green', 'blue'];
        arr1[3] = 'pink';
        console.log(arr1);
        arr1[4] = 'hotpink';
        console.log(arr1);
        arr1[0] = 'yellow'; // 这里是替换原来的数组元素
        console.log(arr1);
        arr1 = '有点意思';
        console.log(arr1); // 不要直接给 数组名赋值 否则里面的数组元素都没有了
    </script>
</head>

<body>

</body>

</html>
```
## 2.2 函数
```markdown
声明函数
// 声明函数
function 函数名() {
 //函数体代码
}

调用函数
// 调用函数
函数名(); // 通过调用函数名来执行函数体代码

函数参数
function sum(num1, num2) {
 console.log(num1 + num2);
}
sum(100, 200); // 形参和实参个数相等，输出正确结果
sum(100, 400, 500, 700); // 实参个数多于形参，只取到形参的个数
sum(200); // 实参个数少于形参，多的形参定义为undefined，结果为NaN

返回值
在使用 return 语句时，函数会停止执行，并返回指定的值
如果函数没有 return ，返回的值是 undefined

arguments的使用
当我们不确定有多少个参数传递的时候，可以用 arguments 来获取
arguments展示形式是一个伪数组，因此可以进行遍历。伪数组具有以下特点：
 具有 length 属性
 按索引方式储存数据
不具有数组的 push , pop 等方法

函数的两种声明方式
利用函数关键字 function 自定义函数方式。
// 声明定义方式
function fn() {...}
// 调用
fn(); 

函数表达式方式(匿名函数）
// 这是函数表达式写法，匿名函数后面跟分号结束
var fn = function(){...}；
// 调用的方式，函数调用必须写到函数体下面
fn();

全局变量
在全局作用域下声明的变量叫做全局变量（在函数外部定义的变量）。
全局变量在代码的任何位置都可以使用
在全局作用域下 var 声明的变量 是全局变量
特殊情况下，在函数内不使用 var 声明的变量也是全局变量（不建议使用）

局部变量
在局部作用域下声明的变量叫做局部变量（在函数内部定义的变量）
局部变量只能在该函数内部使用
在函数内部 var 声明的变量是局部变量
函数的形参实际上就是局部变量
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 函数使用分为两步： 声明函数 和 调用函数
        // 1. 声明函数
        // function 函数名() {
        //     // 函数体
        // }
        function sayHi() {
            console.log('hi~~');

        }
        // (1) function 声明函数的关键字 全部小写
        // (2) 函数是做某件事情，函数名一般是动词 sayHi 
        // (3) 函数不调用自己不执行
        // 2. 调用函数
        // 函数名();
        sayHi();
        // 调用函数的时候千万不要忘记加小括号
    </script>
</head>

<body>

</body>

</html>


带参数的函数
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1. 函数可以重复相同的代码
        // function cook() {
        //     console.log('酸辣土豆丝');

        // }
        // cook();
        // cook();
        // 2. 我们可以利用函数的参数实现函数重复不同的代码
        // function 函数名(形参1,形参2...) { // 在声明函数的小括号里面是 形参 （形式上的参数）

        // }
        // 函数名(实参1,实参2...); // 在函数调用的小括号里面是实参（实际的参数）
        // 3. 形参和实参的执行过程
        function cook(aru) { // 形参是接受实参的  aru = '酸辣土豆丝' 形参类似于一个变量
            console.log(aru);

        }
        cook('酸辣土豆丝');
        cook('大肘子');
        // 4. 函数的参数可以有，也可以没有个数不限
    </script>
</head>

<body>

</body>

</html>


函数形参实参个数匹配
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 函数形参实参个数匹配
        function getSum(num1, num2) {
            console.log(num1 + num2);

        }
        // 1. 如果实参的个数和形参的个数一致 则正常输出结果
        getSum(1, 2);
        // 2. 如果实参的个数多于形参的个数  会取到形参的个数 
        getSum(1, 2, 3);
        // 3. 如果实参的个数小于形参的个数  多于的形参定义为undefined  最终的结果就是 NaN
        // 形参可以看做是不用声明的变量  num2 是一个变量但是没有接受值  结果就是undefined 
        getSum(1); // NaN
        // 建议 我们尽量让实参的个数和形参相匹配
    </script>
</head>

<body>

</body>

</html>

函数的返回值
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1.函数是做某件事或者实现某种功能
        // function cook(aru) {
        //     console.log(aru);

        // }
        // cook('大肘子');
        // 2. 函数的返回值格式
        // function 函数名() {
        //     return 需要返回的结果;
        // }
        // 函数名();
        // (1) 我们函数只是实现某种功能，最终的结果需要返回给函数的调用者函数名() 通过return 实现的
        // (2) 只要函数遇到return 就把后面的结果 返回给函数的调用者  函数名() = return后面的结果
        // 3. 代码验证
        function getResult() {
            return 666;
        }
        getResult(); // getResult()   = 666
        console.log(getResult());

        // function cook(aru) {
        //     return aru;
        // }
        // console.log(cook('大肘子'));
        // 4. 求任意两个数的和
        function getSum(num1, num2) {
            return num1 + num2;
        }
        console.log(getSum(1, 2));
    </script>
</head>

<body>

</body>

</html>

函数返回值注意事项
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 函数返回值注意事项
        // 1. return 终止函数
        function getSum(num1, num2) {
            return num1 + num2; // return 后面的代码不会被执行
            alert('我是不会被执行的哦！')
        }
        console.log(getSum(1, 2));
        // 2. return 只能返回一个值
        function fn(num1, num2) {
            return num1, num2; // 返回的结果是最后一个值
        }
        console.log(fn(1, 2));

        // 3.  我们求任意两个数的 加减乘数结果
        function getResult(num1, num2) {
            return [num1 + num2, num1 - num2, num1 * num2, num1 / num2];
        }
        var re = getResult(1, 2); // 返回的是一个数组
        console.log(re);
        // 4. 我们的函数如果有return 则返回的是 return 后面的值，如果函数么有 return 则返回undefined
        function fun1() {
            return 666;
        }
        console.log(fun1()); // 返回 666
        function fun2() {

        }
        console.log(fun2()); // 函数返回的结果是 undefined
    </script>
</head>

<body>

</body>

</html>

arguments使用
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // arguments 的使用  只有函数才有 arguments对象  而且是每个函数都内置好了这个arguments
        function fn() {
            // console.log(arguments); // 里面存储了所有传递过来的实参  arguments = [1,2,3]
            // console.log(arguments.length);
            // console.log(arguments[2]);
            // 我们可以按照数组的方式遍历arguments
            for (var i = 0; i < arguments.length; i++) {
                console.log(arguments[i]);

            }
        }
        fn(1, 2, 3);
        fn(1, 2, 3, 4, 5);
        // 伪数组 并不是真正意义上的数组
        // 1. 具有数组的 length 属性
        // 2. 按照索引的方式进行存储的
        // 3. 它没有真正数组的一些方法 pop()  push() 等等
    </script>
</head>

<body>

</body>

</html>
```


## 2.3 for的例外
~~~markdown
基本跟java语言差不多,但是for例外:
~~~
~~~html
<script>
       let a = ["ni","hao","ma"];
        for(let i in a){
            alert(i);//这里将输出0,1,2,而不是值
            alert(a[i]);//这里将输出值
        }
        let a2 = {name:"xiaoming", age:18}
        for(let i in a2){
            alert(i);这里将输出属性名
            alert(a2[i]);//这里将输出属性值
        }
   </script>

函数的两种声明方式
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 函数的2中声明方式
        // 1. 利用函数关键字自定义函数(命名函数)
        function fn() {

        }
        fn();
        // 2. 函数表达式(匿名函数) 
        // var 变量名 = function() {};
        var fun = function(aru) {
            console.log('我是函数表达式');
            console.log(aru);

        }
        fun('pink老师');
        // (1) fun是变量名 不是函数名  
        // (2) 函数表达式声明方式跟声明变量差不多，只不过变量里面存的是值 而 函数表达式里面存的是函数
        // (3) 函数表达式也可以进行传递参数
    </script>
</head>

<body>

</body>

</html>
~~~
## 2.4 作用域
```markdown
全局作用域
作用于所有代码执行的环境(整个 script 标签内部)或者一个独立
的 js 文件。
局部作用域
作用于函数内的代码环境，就是局部作用域。 因为跟函数有关系，
所以也称为函数作用域。

JS 没有块级作用域
Js中没有块级作用域（在ES6之前）。
```
```html
JavaScript作用域
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1.JavaScript作用域 ： 就是代码名字（变量）在某个范围内起作用和效果 目的是为了提高程序的可靠性更重要的是减少命名冲突
        // 2. js的作用域（es6）之前 ： 全局作用域   局部作用域 
        // 3. 全局作用域： 整个script标签 或者是一个单独的js文件
        var num = 10;
        var num = 30;
        console.log(num);

        // 4. 局部作用域（函数作用域） 在函数内部就是局部作用域 这个代码的名字只在函数内部起效果和作用
        function fn() {
            // 局部作用域
            var num = 20;
            console.log(num);

        }
        fn();
    </script>
</head>

<body>

</body>

</html>

变量的作用域
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 变量的作用域： 根据作用域的不同我们变量分为全局变量和局部变量
        // 1. 全局变量： 在全局作用域下的变量 在全局下都可以使用
        // 注意 如果在函数内部 没有声明直接赋值的变量也属于全局变量
        var num = 10; // num就是一个全局变量
        console.log(num);

        function fn() {
            console.log(num);

        }
        fn();
        // console.log(aru);

        // 2. 局部变量   在局部作用域下的变量   后者在函数内部的变量就是 局部变量
        // 注意： 函数的形参也可以看做是局部变量
        function fun(aru) {
            var num1 = 10; // num1就是局部变量 只能在函数内部使用
            num2 = 20;
        }
        fun();
        // console.log(num1);
        // console.log(num2);
        // 3. 从执行效率来看全局变量和局部变量
        // (1) 全局变量只有浏览器关闭的时候才会销毁，比较占内存资源
        // (2) 局部变量 当我们程序执行完毕就会销毁， 比较节约内存资源
    </script>
</head>

<body>

</body>

</html>

JavaScript没有块级作用域
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // js中没有块级作用域  js的作用域： 全局作用域  局部作用域  现阶段我们js 没有 块级作用域
        // 我们js 也是在 es6 的时候新增的块级作用域
        // 块级作用域 {}   if {}  for {}
        // java 
        // if(xx) {
        //     int num = 10;
        // }
        // 外面的是不能调用num的
        if (3 < 5) {
            var num = 10;
        }
        console.log(num);
    </script>
</head>

<body>

</body>

</html>
```
## 2.5 函数作用链
```markdown
只要是代码，就至少有一个作用域
写在函数内部的局部作用域
如果函数中还有函数，那么在这个作用域中就又可以诞生一个作用域
根据在内部函数可以访问外部函数变量的这种机制，用链式查
找决定哪些数据能被内部函数访问，就称作作用域链
```
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 作用域链  ： 内部函数访问外部函数的变量，采取的是链式查找的方式来决定取那个值 这种结构我们称为作用域链   就近原则
        var num = 10;

        function fn() { // 外部函数
            var num = 20;

            function fun() { // 内部函数
                console.log(num);

            }
            fun();
        }
        fn();
    </script>
</head>

<body>

</body>

</html>
```
## 2.6 函数预解析
```markdown
预解析：在当前作用域下, JS 代码执行之前，浏览器会默认把
带有 var 和 function 声明的变量在内存中进行提前声明或
者定义。

代码执行： 从上到下执行JS语句。
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1问  
        console.log(num);



        // 2问
        console.log(num); // undefined  坑 1
        var num = 10;
        // 相当于执行了以下代码
        // var num;
        // console.log(num);
        // num = 10;



        // 3问  
        function fn() {
            console.log(11);
        }
        fn();




        // 4问
        fun(); // 报错  坑2 
        var fun = function() {
                console.log(22);

            }
            // 函数表达式 调用必须写在函数表达式的下面
            // 相当于执行了以下代码
            // var fun;
            // fun();
            // fun = function() {
            //         console.log(22);

        //     }

        // 1. 我们js引擎运行js 分为两步：  预解析  代码执行
        // (1). 预解析 js引擎会把js 里面所有的 var  还有 function 提升到当前作用域的最前面
        // (2). 代码执行  按照代码书写的顺序从上往下执行
        // 2. 预解析分为 变量预解析（变量提升） 和 函数预解析（函数提升）
        // (1) 变量提升 就是把所有的变量声明提升到当前的作用域最前面  不提升赋值操作
        // (2) 函数提升 就是把所有的函数声明提升到当前作用域的最前面  不调用函数
    </script>
</head>

<body>

</body>

</html>
```
## 2.7 对象
~~~markdown
1.内建对象
由ES标准中定义的对象，在任何的ES的实现中都可以使用
比如： Math String Number Boolean Function Object
2.宿主对象
由JS的运行环境提供的对象，目前来讲主要指由浏览器提供的对象
比如 BOM DOM
3.自定义对象
由开发人员自己创建的对象
~~~
### 2.7.1 如何创造对象
```markdown
第一种
var obj = {
    "name":"张三疯",
    "sex":"男",
    "age":128,
    "height":154
}

第二种

var andy = new Obect();
andy.name = 'pink';
andy.age = 18;
andy.sex = '男';
andy.sayHi = function(){
    alert('大家好啊~');
}

第三种 利用构造函数创建对象
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 利用构造函数创建对象
        // 我们需要创建四大天王的对象  相同的属性： 名字 年龄 性别  相同的方法： 唱歌
        // 构造函数的语法格式
        // function 构造函数名() {
        //     this.属性 = 值;
        //     this.方法 = function() {}
        // }
        // new 构造函数名();
        function Star(uname, age, sex) {
            this.name = uname;
            this.age = age;
            this.sex = sex;
            this.sing = function(sang) {
                console.log(sang);

            }
        }
        var ldh = new Star('刘德华', 18, '男'); // 调用函数返回的是一个对象
        // console.log(typeof ldh);
        console.log(ldh.name);
        console.log(ldh['sex']);
        ldh.sing('冰雨');
        var zxy = new Star('张学友', 19, '男');
        console.log(zxy.name);
        console.log(zxy.age);
        zxy.sing('李香兰')



        // 1. 构造函数名字首字母要大写
        // 2. 我们构造函数不需要return 就可以返回结果
        // 3. 我们调用构造函数 必须使用 new
        // 4. 我们只要new Star() 调用函数就创建一个对象 ldh  {}
        // 5. 我们的属性和方法前面必须添加 this
    </script>
</head>

<body>

</body>

</html>

```
```html
利用对象字面量创建对象
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1.利用对象字面量创建对象 {}
        // var obj = {};  // 创建了一个空的对象 
        var obj = {
                uname: '张三疯',
                age: 18,
                sex: '男',
                sayHi: function() {
                    console.log('hi~');

                }
            }
            // (1) 里面的属性或者方法我们采取键值对的形式  键 属性名 ： 值  属性值 
            // (2) 多个属性或者方法中间用逗号隔开的
            // (3) 方法冒号后面跟的是一个匿名函数
            // 2. 使用对象
            // (1). 调用对象的属性 我们采取 对象名.属性名 . 我们理解为 的
        console.log(obj.uname);
        // (2). 调用属性还有一种方法 对象名['属性名']
        console.log(obj['age']);
        // (3) 调用对象的方法 sayHi   对象名.方法名() 千万别忘记添加小括号
        obj.sayHi();
    </script>
</head>

<body>

</body>

</html>

变量属性函数方法区别
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 变量、属性、函数、方法的区别
        // 1.变量和属性的相同点 他们都是用来存储数据的 
        var num = 10;
        var obj = {
            age: 18,
            fn: function() {

            }
        }

        function fn() {

        }
        console.log(obj.age);
        // console.log(age);

        // 变量 单独声明并赋值  使用的时候直接写变量名 单独存在
        // 属性 在对象里面的不需要声明的 使用的时候必须是 对象.属性
        // 2. 函数和方法的相同点 都是实现某种功能  做某件事
        // 函数是单独声明 并且调用的 函数名() 单独存在的
        // 方法 在对象里面 调用的时候 对象.方法()
    </script>
</head>

<body>

</body>

</html>

利用new Object创建对象
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 利用 new Object 创建对象
        var obj = new Object(); // 创建了一个空的对象
        obj.uname = '张三疯';
        obj.age = 18;
        obj.sex = '男';
        obj.sayHi = function() {
                console.log('hi~');

            }
            // (1) 我们是利用 等号 = 赋值的方法 添加对象的属性和方法
            // (2) 每个属性和方法之间用 分号结束
        console.log(obj.uname);
        console.log(obj['sex']);
        obj.sayHi();
    </script>
</head>

<body>

</body>

</html>


利用构造函数创建对象
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 利用构造函数创建对象
        // 我们需要创建四大天王的对象  相同的属性： 名字 年龄 性别  相同的方法： 唱歌
        // 构造函数的语法格式
        // function 构造函数名() {
        //     this.属性 = 值;
        //     this.方法 = function() {}
        // }
        // new 构造函数名();
        function Star(uname, age, sex) {
            this.name = uname;
            this.age = age;
            this.sex = sex;
            this.sing = function(sang) {
                console.log(sang);

            }
        }
        var ldh = new Star('刘德华', 18, '男'); // 调用函数返回的是一个对象
        // console.log(typeof ldh);
        console.log(ldh.name);
        console.log(ldh['sex']);
        ldh.sing('冰雨');
        var zxy = new Star('张学友', 19, '男');
        console.log(zxy.name);
        console.log(zxy.age);
        zxy.sing('李香兰')



        // 1. 构造函数名字首字母要大写
        // 2. 我们构造函数不需要return 就可以返回结果
        // 3. 我们调用构造函数 必须使用 new
        // 4. 我们只要new Star() 调用函数就创建一个对象 ldh  {}
        // 5. 我们的属性和方法前面必须添加 this
    </script>
</head>

<body>

</body>

</html>

构造函数和对象
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 构造函数和对象
        // 1. 构造函数  明星 泛指的某一大类  它类似于 java 语言里面的  类(class)
        function Star(uname, age, sex) {
            this.name = uname;
            this.age = age;
            this.sex = sex;
            this.sing = function(sang) {
                console.log(sang);

            }
        }
        // 2. 对象 特指 是一个具体的事物 刘德华 ==  {name: "刘德华", age: 18, sex: "男", sing: ƒ}
        var ldh = new Star('刘德华', 18, '男'); // 调用函数返回的是一个对象
        console.log(ldh);
        // 3. 我们利用构造函数创建对象的过程我们也称为对象的实例化
    </script>
</head>

<body>

</body>

</html>
```
### 2.7.2 new关键字的作用
```markdown
1. 在构造函数代码开始执行之前，创建一个空对象；
2. 修改this的指向，把this指向创建出来的空对象；
3. 执行函数的代码
4. 在函数完成之后，返回this---即创建出来的对象
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // new关键字执行过程
        // 1. new 构造函数可以在内存中创建了一个空的对象 
        // 2. this 就会指向刚才创建的空对象
        // 3. 执行构造函数里面的代码 给这个空对象添加属性和方法
        // 4. 返回这个对象
        function Star(uname, age, sex) {
            this.name = uname;
            this.age = age;
            this.sex = sex;
            this.sing = function(sang) {
                console.log(sang);

            }
        }
        var ldh = new Star('刘德华', 18, '男');
    </script>
</head>

<body>

</body>

</html>
```
### 2.7.3 对象的操作
```markdown
遍历对象的属性
for (var k in obj) {
    console.log(k);      // 这里的 k 是属性名
    console.log(obj[k]); // 这里的 obj[k] 是属性值
}

增加属性
obj.name = "xiaoming"

删除属性
delete obj.name;

查属性
console.log(obj.name);

改属性
obj.name = 'xiaohong';

如何判断对象是否包含某个属性
console.log(‘name’ in obj);

修改样式
元素.style.样式名 = 样式值
```
```html
遍历对象
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 遍历对象 
        var obj = {
                name: 'pink老师',
                age: 18,
                sex: '男',
                fn: function() {}
            }
            // console.log(obj.name);
            // console.log(obj.age);
            // console.log(obj.sex);
            // for in 遍历我们的对象
            // for (变量 in 对象) {

        // }
        for (var k in obj) {
            console.log(k); // k 变量 输出  得到的是 属性名
            console.log(obj[k]); // obj[k] 得到是 属性值

        }
        // 我们使用 for in 里面的变量 我们喜欢写 k  或者  key
    </script>
</head>

<body>

</body>

</html>
```

### 2.7.4 JavaScript 内置对象
```markdown
Math 对象
Math 对象不是构造函数，它具有数学常数和函数的属性和方法。跟
数学相关的运算（求绝对值，取整、最大值
等）可以使用 Math 中的成员。

Math.PI // 圆周率
Math.floor() // 向下取整
Math.ceil() // 向上取整
Math.round() // 四舍五入版 就近取整 注意 -3.5 结果是 -3
Math.abs() // 绝对值
Math.max()/Math.min() // 求最大和最小值
Math.random() //random() 方法可以随机返回一个小数，其取值范围是 [0，1)，左闭右开 0 <= x < 1 

Date对象
Date 对象和 Math 对象不一样，他是一个构造函数，所以我们需要实例化后才能使用
   // 1. 使用Date  如果没有参数 返回当前系统的当前时间
        var date = new Date();
        
   // 2. 参数常用的写法  数字型  2019, 10, 01  或者是 字符串型 '2019-10-1 8:8:8'
        var date1 = new Date(2019, 10, 1);
        var date2 = new Date('2019-10-1 8:8:8');

获取日期的总的毫秒形式
// 实例化Date对象
var now = new Date();
// 1. 用于获取对象的原始值
console.log(date.valueOf())
console.log(date.getTime())
// 2. 简单写可以这么做
var now = + new Date();
// 3. HTML5中提供的方法，有兼容性问题
var now = Date.now();

数组对象
字面量方式
new Array()
检测是否为数组
instanceof 运算符，可以判断一个对象是否属于某种类型
Array.isArray()用于判断一个对象是否为数组，isArray() 是 HTML5 中提供的方法
var arr = [1, 23];
var obj = {};
console.log(arr instanceof Array); // true
console.log(obj instanceof Array); // false
console.log(Array.isArray(arr)); // true
console.log(Array.isArray(obj)); // false

字符串对象
基本包装类型
为了方便操作基本数据类型，JavaScript 还提供了三个特殊的引用类型：String、Number和 Boolean

var str = 'andy';
console.log(str.length);
这是因为 js 会把基本数据类型包装为复杂数据类型，其执行过程如下 ：
// 1. 生成临时变量，把简单类型包装为复杂数据类型
var temp = new String('andy');
// 2. 赋值给我们声明的字符变量
str = temp;
// 3. 销毁临时变量
temp = null;
```
```html
Math对象最大值
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // Math数学对象 不是一个构造函数 ，所以我们不需要new 来调用 而是直接使用里面的属性和方法即可
        console.log(Math.PI); // 一个属性 圆周率
        console.log(Math.max(1, 99, 3)); // 99
        console.log(Math.max(-1, -10)); // -1
        console.log(Math.max(1, 99, 'pink老师')); // NaN
        console.log(Math.max()); // -Infinity
    </script>
</head>

<body>

</body>

</html>

Math绝对值和三个取整方法
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1.绝对值方法
        console.log(Math.abs(1)); // 1
        console.log(Math.abs(-1)); // 1
        console.log(Math.abs('-1')); // 隐式转换 会把字符串型 -1 转换为数字型
        console.log(Math.abs('pink')); // NaN 

        // 2.三个取整方法
        // (1) Math.floor()   地板 向下取整  往最小了取值
        console.log(Math.floor(1.1)); // 1
        console.log(Math.floor(1.9)); // 1
        // (2) Math.ceil()   ceil 天花板 向上取整  往最大了取值
        console.log(Math.ceil(1.1)); // 2
        console.log(Math.ceil(1.9)); // 2
        // (3) Math.round()   四舍五入  其他数字都是四舍五入，但是 .5 特殊 它往大了取  
        console.log(Math.round(1.1)); // 1
        console.log(Math.round(1.5)); // 2
        console.log(Math.round(1.9)); // 2
        console.log(Math.round(-1.1)); // -1
        console.log(Math.round(-1.5)); // 这个结果是 -1
    </script>
</head>

<body>

</body>

</html>


Math对象随机数方法
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1.Math对象随机数方法   random() 返回一个随机的小数  0 =< x < 1
        // 2. 这个方法里面不跟参数
        // 3. 代码验证 
        console.log(Math.random());
        // 4. 我们想要得到两个数之间的随机整数 并且 包含这2个整数
        // Math.floor(Math.random() * (max - min + 1)) + min;
        function getRandom(min, max) {
            return Math.floor(Math.random() * (max - min + 1)) + min;
        }
        console.log(getRandom(1, 10));
        // 5. 随机点名  
        var arr = ['张三', '张三丰', '张三疯子', '李四', '李思思', 'pink老师'];
        // console.log(arr[0]);
        console.log(arr[getRandom(0, arr.length - 1)]);
    </script>
</head>

<body>

</body>

</html>

Date日期对象
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // Date() 日期对象  是一个构造函数 必须使用new 来调用创建我们的日期对象
        var arr = new Array(); // 创建一个数组对象
        var obj = new Object(); // 创建了一个对象实例
        // 1. 使用Date  如果没有参数 返回当前系统的当前时间
        var date = new Date();
        console.log(date);
        // 2. 参数常用的写法  数字型  2019, 10, 01  或者是 字符串型 '2019-10-1 8:8:8'
        var date1 = new Date(2019, 10, 1);
        console.log(date1); // 返回的是 11月 不是 10月 
        var date2 = new Date('2019-10-1 8:8:8');
        console.log(date2);
    </script>
</head>

<body>

</body>

</html>


```
### 2.7.5  日期格式化
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200715123426717.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```html
Date日期对象
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // Date() 日期对象  是一个构造函数 必须使用new 来调用创建我们的日期对象
        var arr = new Array(); // 创建一个数组对象
        var obj = new Object(); // 创建了一个对象实例
        // 1. 使用Date  如果没有参数 返回当前系统的当前时间
        var date = new Date();
        console.log(date);
        // 2. 参数常用的写法  数字型  2019, 10, 01  或者是 字符串型 '2019-10-1 8:8:8'
        var date1 = new Date(2019, 10, 1);
        console.log(date1); // 返回的是 11月 不是 10月 
        var date2 = new Date('2019-10-1 8:8:8');
        console.log(date2);
    </script>
</head>

<body>

</body>

</html>

格式化日期年月日
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 格式化日期 年月日 
        var date = new Date();
        console.log(date.getFullYear()); // 返回当前日期的年  2019
        console.log(date.getMonth() + 1); // 月份 返回的月份小1个月   记得月份+1 呦
        console.log(date.getDate()); // 返回的是 几号
        console.log(date.getDay()); // 3  周一返回的是 1 周六返回的是 6 但是 周日返回的是 0
        // 我们写一个 2019年 5月 1日 星期三
        var year = date.getFullYear();
        var month = date.getMonth() + 1;
        var dates = date.getDate();
        var arr = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
        var day = date.getDay();
        console.log('今天是：' + year + '年' + month + '月' + dates + '日 ' + arr[day]);
    </script>
</head>

<body>

</body>

</html>

格式化日期时分秒
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 格式化日期 时分秒
        var date = new Date();
        console.log(date.getHours()); // 时
        console.log(date.getMinutes()); // 分
        console.log(date.getSeconds()); // 秒
        // 要求封装一个函数返回当前的时分秒 格式 08:08:08
        function getTimer() {
            var time = new Date();
            var h = time.getHours();
            h = h < 10 ? '0' + h : h;
            var m = time.getMinutes();
            m = m < 10 ? '0' + m : m;
            var s = time.getSeconds();
            s = s < 10 ? '0' + s : s;
            return h + ':' + m + ':' + s;
        }
        console.log(getTimer());
    </script>
</head>

<body>

</body>

</html>

获得Date总的毫秒数
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 获得Date总的毫秒数(时间戳)  不是当前时间的毫秒数 而是距离1970年1月1号过了多少毫秒数
        // 1. 通过 valueOf()  getTime()
        var date = new Date();
        console.log(date.valueOf()); // 就是 我们现在时间 距离1970.1.1 总的毫秒数
        console.log(date.getTime());
        // 2. 简单的写法 (最常用的写法)
        var date1 = +new Date(); // +new Date()  返回的就是总的毫秒数
        console.log(date1);
        // 3. H5 新增的 获得总的毫秒数
        console.log(Date.now());
    </script>
</head>

<body>

</body>

</html>


倒计时效果
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 倒计时效果
        // 1.核心算法：输入的时间减去现在的时间就是剩余的时间，即倒计时 ，但是不能拿着时分秒相减，比如 05 分减去25分，结果会是负数的。
        // 2.用时间戳来做。用户输入时间总的毫秒数减去现在时间的总的毫秒数，得到的就是剩余时间的毫秒数。
        // 3.把剩余时间总的毫秒数转换为天、时、分、秒 （时间戳转换为时分秒）
        // 转换公式如下： 
        //  d = parseInt(总秒数/ 60/60 /24);    //  计算天数
        //  h = parseInt(总秒数/ 60/60 %24)   //   计算小时
        //  m = parseInt(总秒数 /60 %60 );     //   计算分数
        //  s = parseInt(总秒数%60);            //   计算当前秒数
        function countDown(time) {
            var nowTime = +new Date(); // 返回的是当前时间总的毫秒数
            var inputTime = +new Date(time); // 返回的是用户输入时间总的毫秒数
            var times = (inputTime - nowTime) / 1000; // times是剩余时间总的秒数 
            var d = parseInt(times / 60 / 60 / 24); // 天
            d = d < 10 ? '0' + d : d;
            var h = parseInt(times / 60 / 60 % 24); //时
            h = h < 10 ? '0' + h : h;
            var m = parseInt(times / 60 % 60); // 分
            m = m < 10 ? '0' + m : m;
            var s = parseInt(times % 60); // 当前的秒
            s = s < 10 ? '0' + s : s;
            return d + '天' + h + '时' + m + '分' + s + '秒';
        }
        console.log(countDown('2019-5-1 18:00:00'));
        var date = new Date();
        console.log(date);
    </script>
</head>

<body>

</body>

</html>
```

### 2.7.6 数组
```html
创建数组的两种方式
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 创建数组的两种方式
        // 1. 利用数组字面量
        var arr = [1, 2, 3];
        console.log(arr[0]);

        // 2. 利用new Array()
        // var arr1 = new Array();  // 创建了一个空的数组
        // var arr1 = new Array(2);  // 这个2 表示 数组的长度为 2  里面有2个空的数组元素 
        var arr1 = new Array(2, 3); // 等价于 [2,3]  这样写表示 里面有2个数组元素 是 2和3
        console.log(arr1);
    </script>
</head>

<body>

</body>

</html>

检测是否为数组方法
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 翻转数组
        function reverse(arr) {
            // if (arr instanceof Array) {
            if (Array.isArray(arr)) {
                var newArr = [];
                for (var i = arr.length - 1; i >= 0; i--) {
                    newArr[newArr.length] = arr[i];

                }
                return newArr;
            } else {
                return 'error 这个参数要求必须是数组格式 [1,2,3]'
            }
        }
        console.log(reverse([1, 2, 3]));
        console.log(reverse(1, 2, 3));
        // 检测是否为数组
        // (1) instanceof  运算符 它可以用来检测是否为数组
        var arr = [];
        var obj = {};
        console.log(arr instanceof Array);
        console.log(obj instanceof Array);
        // (2) Array.isArray(参数);  H5新增的方法  ie9以上版本支持
        console.log(Array.isArray(arr));
        console.log(Array.isArray(obj));
    </script>
</head>

<body>

</body>

</html>
```
#### 2.7.6.1 添加删除数组元素
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200715131153881.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```html
添加删除数组元素方法
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 添加删除数组元素方法
        // 1. push() 在我们数组的末尾 添加一个或者多个数组元素   push  推
        var arr = [1, 2, 3];
        // arr.push(4, 'pink');
        console.log(arr.push(4, 'pink'));

        console.log(arr);
        // (1) push 是可以给数组追加新的元素
        // (2) push() 参数直接写 数组元素就可以了
        // (3) push完毕之后，返回的结果是 新数组的长度 
        // (4) 原数组也会发生变化
        // 2. unshift 在我们数组的开头 添加一个或者多个数组元素
        console.log(arr.unshift('red', 'purple'));

        console.log(arr);
        // (1) unshift是可以给数组前面追加新的元素
        // (2) unshift() 参数直接写 数组元素就可以了
        // (3) unshift完毕之后，返回的结果是 新数组的长度 
        // (4) 原数组也会发生变化

        // 3. pop() 它可以删除数组的最后一个元素  
        console.log(arr.pop());
        console.log(arr);
        // (1) pop是可以删除数组的最后一个元素 记住一次只能删除一个元素
        // (2) pop() 没有参数
        // (3) pop完毕之后，返回的结果是 删除的那个元素 
        // (4) 原数组也会发生变化
        // 4. shift() 它可以删除数组的第一个元素  
        console.log(arr.shift());
        console.log(arr);
        // (1) shift是可以删除数组的第一个元素 记住一次只能删除一个元素
        // (2) shift() 没有参数
        // (3) shift完毕之后，返回的结果是 删除的那个元素 
        // (4) 原数组也会发生变化
    </script>
</head>

<body>

</body>

</html>


筛选数组
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 有一个包含工资的数组[1500, 1200, 2000, 2100, 1800]，要求把数组中工资超过2000的删除，剩余的放到新数组里面
        var arr = [1500, 1200, 2000, 2100, 1800];
        var newArr = [];
        for (var i = 0; i < arr.length; i++) {
            if (arr[i] < 2000) {
                // newArr[newArr.length] = arr[i];
                newArr.push(arr[i]);
            }
        }
        console.log(newArr);
    </script>
</head>

<body>

</body>

</html>
```


#### 2.7.6.2  数组排序
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200715131245426.png)
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 数组排序
        // 1. 翻转数组
        var arr = ['pink', 'red', 'blue'];
        arr.reverse();
        console.log(arr);

        // 2. 数组排序（冒泡排序）
        var arr1 = [13, 4, 77, 1, 7];
        arr1.sort(function(a, b) {
            //  return a - b; 升序的顺序排列
            return b - a; // 降序的顺序排列
        });
        console.log(arr1);
    </script>
</head>

<body>

</body>

</html>
```
#### 2.7.6.3 数组索引方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200715131754587.png)

```html
获取数组元素索引方法
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 返回数组元素索引号方法  indexOf(数组元素)  作用就是返回该数组元素的索引号 从前面开始查找
        // 它只返回第一个满足条件的索引号 
        // 它如果在该数组里面找不到元素，则返回的是 -1  
        // var arr = ['red', 'green', 'blue', 'pink', 'blue'];
        var arr = ['red', 'green', 'pink'];
        console.log(arr.indexOf('blue'));
        // 返回数组元素索引号方法  lastIndexOf(数组元素)  作用就是返回该数组元素的索引号 从后面开始查找
        var arr = ['red', 'green', 'blue', 'pink', 'blue'];

        console.log(arr.lastIndexOf('blue')); // 4
    </script>
</head>

<body>

</body>

</html>


数组去重
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 数组去重 ['c', 'a', 'z', 'a', 'x', 'a', 'x', 'c', 'b'] 要求去除数组中重复的元素。
        // 1.目标： 把旧数组里面不重复的元素选取出来放到新数组中， 重复的元素只保留一个， 放到新数组中去重。
        // 2.核心算法： 我们遍历旧数组， 然后拿着旧数组元素去查询新数组， 如果该元素在新数组里面没有出现过， 我们就添加， 否则不添加。
        // 3.我们怎么知道该元素没有存在？ 利用 新数组.indexOf(数组元素) 如果返回时 - 1 就说明 新数组里面没有改元素
        // 封装一个 去重的函数 unique 独一无二的 
        function unique(arr) {
            var newArr = [];
            for (var i = 0; i < arr.length; i++) {
                if (newArr.indexOf(arr[i]) === -1) {
                    newArr.push(arr[i]);
                }
            }
            return newArr;
        }
        // var demo = unique(['c', 'a', 'z', 'a', 'x', 'a', 'x', 'c', 'b'])
        var demo = unique(['blue', 'green', 'blue'])
        console.log(demo);
    </script>
</head>

<body>

</body>

</html>
```
#### 2.7.6.4 数组转换为字符串
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200715132122620.png)
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 数组转换为字符串 
        // 1. toString() 将我们的数组转换为字符串
        var arr = [1, 2, 3];
        console.log(arr.toString()); // 1,2,3
        // 2. join(分隔符) 
        var arr1 = ['green', 'blue', 'pink'];
        console.log(arr1.join()); // green,blue,pink
        console.log(arr1.join('-')); // green-blue-pink
        console.log(arr1.join('&')); // green&blue&pink
    </script>
</head>

<body>

</body>

</html>

基本包装类型
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 基本包装类型
        var str = 'andy';
        console.log(str.length);
        // 对象 才有 属性和方法   复杂数据类型才有 属性和方法 
        // 简单数据类型为什么会有length 属性呢？ 
        // 基本包装类型：  就是把简单数据类型 包装成为了 复杂数据类型 
        // (1) 把简单数据类型包装为复杂数据类型 
        var temp = new String('andy');
        // (2) 把临时变量的值 给 str
        str = temp;
        // (3) 销毁这个临时变量
        temp = null;
    </script>
</head>

<body>

</body>

</html>

字符串的不可变性
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 字符串的不可变性
        var str = 'andy';
        console.log(str);
        str = 'red';
        console.log(str);
        // 因为我们字符串的不可变所以不要大量的拼接字符串
        var str = '';
        for (var i = 1; i <= 1000000000; i++) {
            str += i;
        }
        console.log(str);
    </script>
</head>

<body>

</body>

</html>

```
### 2.7.7 字符串
#### 2.7.7.1 根据位置返回字符
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200715133230424.png)
```html
根据字符返回位置
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 字符串对象  根据字符返回位置  str.indexOf('要查找的字符', [起始的位置])
        var str = '改革春风吹满地，春天来了';
        console.log(str.indexOf('春'));
        console.log(str.indexOf('春', 3)); // 从索引号是 3的位置开始往后查找
    </script>
</head>

<body>

</body>

</html>

根据位置返回字符
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 根据位置返回字符
        // 1. charAt(index) 根据位置返回字符
        var str = 'andy';
        console.log(str.charAt(3));
        // 遍历所有的字符
        for (var i = 0; i < str.length; i++) {
            console.log(str.charAt(i));
        }
        // 2. charCodeAt(index)  返回相应索引号的字符ASCII值 目的： 判断用户按下了那个键 
        console.log(str.charCodeAt(0)); // 97
        // 3. str[index] H5 新增的
        console.log(str[0]); // a
    </script>
</head>

<body>

</body>

</html>
```


#### 2.7.7.2 字符串操作方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200715133427399.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```html
字符串操作方法
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 字符串操作方法
        // 1. concat('字符串1','字符串2'....)
        var str = 'andy';
        console.log(str.concat('red'));

        // 2. substr('截取的起始位置', '截取几个字符');
        var str1 = '改革春风吹满地';
        console.log(str1.substr(2, 2)); // 第一个2 是索引号的2 从第几个开始  第二个2 是取几个字符
    </script>
</head>

<body>

</body>

</html>
```
#### 2.7.7.3 其他方法
```markdown
replace()方法
replace(被替换的字符串， 要替换为的字符串)；

split()方法
split()方法用于切分字符串，它可以将字符串切分为数组。在切分完毕之后，返回的是一个新数组。
```
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 1. 替换字符 replace('被替换的字符', '替换为的字符')  它只会替换第一个字符
        var str = 'andyandy';
        console.log(str.replace('a', 'b'));
        // 有一个字符串 'abcoefoxyozzopp'  要求把里面所有的 o 替换为 *
        var str1 = 'abcoefoxyozzopp';
        while (str1.indexOf('o') !== -1) {
            str1 = str1.replace('o', '*');
        }
        console.log(str1);

        // 2. 字符转换为数组 split('分隔符')    前面我们学过 join 把数组转换为字符串
        var str2 = 'red, pink, blue';
        console.log(str2.split(','));
        var str3 = 'red&pink&blue';
        console.log(str3.split('&'));
    </script>
</head>

<body>

</body>

</html>
```
### 2.7.8 JavaScript 简单类型与复杂类型
```markdown
简单类型又叫做基本数据类型或者值类型，复杂类型又叫做引用类型。
值类型：简单数据类型/基本数据类型，在存储时变量中存储的是值本
身，因此叫做值类型
string ，number，boolean，undefined，null
引用类型：复杂数据类型，在存储时变量中存储的仅仅是地
址（引用），因此叫做引用数据类型
通过 new 关键字创建的对象（系统对象、自定义对象），
如 Object、Array、Date等


简单类型传参
函数的形参也可以看做是一个变量，当我们把一个值类型变量
作为参数传给函数的形参时，其实是把变量在栈
空间里的值复制了一份给形参，那么在方法内部对形参做任何
修改，都不会影响到的外部变量。

复杂类型传参
函数的形参也可以看做是一个变量，当我们把引用类型变量传给形
参时，其实是把变量在栈空间里保存的堆地
址复制给了形参，形参和实参其实保存的是同一个堆地址，所以操
作的是同一个对象。
```
```html
简单数据类型null
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 简单数据类型 null  返回的是一个空的对象  object 
        var timer = null;
        console.log(typeof timer);
        // 如果有个变量我们以后打算存储为对象，暂时没想好放啥， 这个时候就给 null 
        // 1. 简单数据类型 是存放在栈里面 里面直接开辟一个空间存放的是值
        // 2. 复杂数据类型 首先在栈里面存放地址 十六进制表示  然后这个地址指向堆里面的数据
    </script>
</head>

<body>

</body>

</html>


简单数据类型传参
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 简单数据类型传参
        function fn(a) {
            a++;
            console.log(a);
        }
        var x = 10;
        fn(x);
        console.log(x);
    </script>
</head>

<body>

</body>

</html>

复杂数据类型传参
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        // 复杂数据类型传参
        function Person(name) {
            this.name = name;
        }

        function f1(x) { // x = p
            console.log(x.name); // 2. 这个输出什么 ?  刘德华   
            x.name = "张学友";
            console.log(x.name); // 3. 这个输出什么 ?   张学友
        }
        var p = new Person("刘德华");
        console.log(p.name); // 1. 这个输出什么 ?   刘德华 
        f1(p);
        console.log(p.name); // 4. 这个输出什么 ?   张学友
    </script>
</head>

<body>

</body>

</html>
```

```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复js
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

