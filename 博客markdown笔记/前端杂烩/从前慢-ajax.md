# Ajax
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714010239415.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1 Ajax 基础
### 1.1 什么是Ajax
```markdown
Ajax 即Asynchronous Javascript And XML
(异步 JavaScript 和 XML)。他不是一种新的编程语言，
而是一种通过异步实现网页的局部更新技术。
```
### 1.2 传统请求和异步请求
```markdown
传统请求:   基于超级链接 地址栏 form表单 地
址栏 location.href 发起的请求全部是传统请求
  特点: 请求之后,刷新整张页面	
  缺点: 由于刷新了整张页面,用户操作被中断,造成大量网络
  流量的极大浪费。
异步请求:  基于ajax发起的请求都是异步请求
  特点: 多个请求并行发生,请求之间互不影响,请求之后页面不
  动,刷新页面的局部
```


### 1.3  传统网站中存在的问题
```markdown
网速慢的情况下，页面加载时间长，用户只能等待
表单提交后，如果一项内容不合格，需要重新填写所有表单内容
页面跳转，重新加载页面，造成资源浪费，增加用户等待时间
```
### 1.4  Ajax 的应用场景
```markdown
页面上拉加载更多数据
列表数据无刷新分页
表单项离开焦点数据验证
搜索框提示文字下拉列表
```
### 1.5 Ajax 运行原理
```markdown
Ajax 相当于浏览器发送请求与接收响应的代理人，以实现
在不影响用户浏览页面的情况下，局部更新页面数据，从
而提高用户体验。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200806084345314.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 1.6 Ajax的核心对象
```markdown
XMLHttpRequest 对象是一个javascript对象，存在
着浏览器差异。简称xhr对象
```
### 1.7 Ajax入门
```markdown
1.  创建 Ajax 对象
var xhr = new XMLHttpRequest();
2.  告诉 Ajax 请求地址以及请求方式
xhr.open('get', 'http://www.example.com');
3.  发送请求
xhr.send();
4.  获取服务器端给与客户端的响应数据
xhr.onload = function () {
     console.log(xhr.responseText);
 }
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script type="text/javascript">
		// 1.创建ajax对象
		var xhr = new XMLHttpRequest();
		// 2.告诉Ajax对象要向哪发送请求，以什么方式发送请求
		// 1)请求方式 2)请求地址
		xhr.open('get', 'http://localhost:3000/first');
		// 3.发送请求
		xhr.send();
		// 4.获取服务器端响应到客户端的数据
		xhr.onload = function (){
			console.log(xhr.responseText)
		}
	</script>
</body>
</html>
```
### 1.8  服务器端响应的数据格式
```markdown
在真实的项目中，服务器端大多数情况下会以 JSON 对
象作为响应数据的格式。当客户端拿到响应数据时，要
将 JSON 数据和 HTML 字符串进行拼接，然后将拼接的结
果展示在页面中。
在 http 请求与响应的过程中，无论是请求参数还是
响应内容，如果是对象类型，最终都会被转换为对象字
符串进行传输。
JSON.parse() // 将 json 字符串转换为json对象
```
### 1.9 处理服务器端返回的JSON数据
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script type="text/javascript">
		// 1.创建ajax对象
		var xhr = new XMLHttpRequest();
		// 2.告诉Ajax对象要向哪发送请求，以什么方式发送请求
		// 1)请求方式 2)请求地址
		xhr.open('get', 'http://localhost:3000/responseData');
		// 3.发送请求
		xhr.send();
		// 4.获取服务器端响应到客户端的数据
		xhr.onload = function (){
			// console.log(typeof xhr.responseText)
			// 将JSON字符串转换为JSON对象
			var responseText = JSON.parse(xhr.responseText);
			// 测试：在控制台输出处理结果
			console.log(responseText)
			// 将数据和html字符串进行拼接
			var str = '<h2>'+ responseText.name +'</h2>';
			// 将拼接的结果追加到页面中
			document.body.innerHTML = str;
		}
	</script>
</body>
</html>
```
### 1.10 请求参数传递
```markdown
传统网站表单提交
<form method="get" action="http://www.example.com">
     <input type="text" name="username"/>
     <input type="password" name="password">
 </form>
 <!– http://www.example.com?username=zhangsan&password=123456 -->

GET 请求方式
xhr.open('get', 'http://www.example.com?name=zhangsan&age=20');


POST 请求方式
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded') xhr.send('name=zhangsan&age=20');


```
#### 1.10.1 传递get请求参数
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<p>
		<input type="text" id="username">
	</p>
	<p>
		<input type="text" id="age">
	</p>
	<p>
		<input type="button" value="提交" id="btn">
	</p>
	<script type="text/javascript">
		// 获取按钮元素
		var btn = document.getElementById('btn');
		// 获取姓名文本框
		var username = document.getElementById('username');
		// 获取年龄文本框
		var age = document.getElementById('age');
		// 为按钮添加点击事件
		btn.onclick = function () {
			// 创建ajax对象
			var xhr = new XMLHttpRequest();
			// 获取用户在文本框中输入的值
			var nameValue = username.value;
			var ageValue = age.value;
			// 拼接请求参数
			var params = 'username='+ nameValue +'&age=' + ageValue;
			// 配置ajax对象
			xhr.open('get', 'http://localhost:3000/get?'+params);
			// 发送请求
			xhr.send();
			// 获取服务器端响应的数据
			xhr.onload = function () {
				console.log(xhr.responseText)
			}
		}
	</script>
</body>
</html>
```
#### 1.10.2 传递post请求参数
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<p>
		<input type="text" id="username">
	</p>
	<p>
		<input type="text" id="age">
	</p>
	<p>
		<input type="button" value="提交" id="btn">
	</p>
	<script type="text/javascript">
		// 获取按钮元素
		var btn = document.getElementById('btn');
		// 获取姓名文本框
		var username = document.getElementById('username');
		// 获取年龄文本框
		var age = document.getElementById('age');
		// 为按钮添加点击事件
		btn.onclick = function () {
			// 创建ajax对象
			var xhr = new XMLHttpRequest();
			// 获取用户在文本框中输入的值
			var nameValue = username.value;
			var ageValue = age.value;
			// 拼接请求参数
			var params = 'username='+ nameValue +'&age=' + ageValue;
			// 配置ajax对象
			xhr.open('post', 'http://localhost:3000/post');
			// 设置请求参数格式的类型（post请求必须要设置）
			xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
			// 发送请求
			xhr.send(params);
			// 获取服务器端响应的数据
			xhr.onload = function () {
				console.log(xhr.responseText)
			}
		}
	</script>
</body>
</html>
```
#### 1.10.3 向服务器端传递JSON格式的请求参数
```markdown
请求参数的格式
1.   application/x-www-form-urlencoded
name=zhangsan&age=20&sex=男
2.   application/json
{name: 'zhangsan', age: '20', sex: '男'}
在请求头中指定 Content-Type 属性的值是 application/json，
告诉服务器端当前请求参数的格式是 json。
JSON.stringify() // 将json对象转换为json字符串
注意：get 请求是不能提交 json 对象数据格式的，传统网站的
表单提交也是不支持 json 对象数据格式的。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script type="text/javascript">
		// 1.创建ajax对象
		var xhr = new XMLHttpRequest();
		// 2.告诉Ajax对象要向哪发送请求，以什么方式发送请求
		// 1)请求方式 2)请求地址
		xhr.open('post', 'http://localhost:3000/json');
		// 通过请求头告诉服务器端客户端向服务器端传递的请求参数的格式是什么
		xhr.setRequestHeader('Content-Type', 'application/json');
		// JSON.stringify() 将json对象转换为json字符串
		// 3.发送请求
		xhr.send(JSON.stringify({name: 'lisi', age:50}));
		// 4.获取服务器端响应到客户端的数据
		xhr.onload = function (){
			console.log(xhr.responseText)
		}
	</script>
</body>
</html>
```
### 1.11 Ajax 错误处理
```markdown
1.   网络畅通，服务器端能接收到请求，服务器端返回的结果不是预期结果。
可以判断服务器端返回的状态码，分别进行处理。xhr.status 获取http状态码
2.   网络畅通，服务器端没有接收到请求，返回404状态码。
检查请求地址是否错误。
3.   网络畅通，服务器端能接收到请求，服务器端返回500状态码。
服务器端错误，找后端程序员进行沟通。
4.   网络中断，请求无法发送到服务器端。
会触发xhr对象下面的onerror事件，在onerror事件处理函数中对错误进行处理。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<button id="btn">发送Ajax请求</button>
	<script type="text/javascript">
		var btn = document.getElementById('btn');

		btn.onclick = function () {
			// 1.创建ajax对象
			var xhr = new XMLHttpRequest();
			// 2.告诉Ajax对象要向哪发送请求，以什么方式发送请求
			// 1)请求方式 2)请求地址
			xhr.open('get', 'http://localhost:3000/error');
			// 3.发送请求
			xhr.send();
			// 4.获取服务器端响应到客户端的数据
			xhr.onload = function (){
				// xhr.status 获取http状态码
				console.log(xhr.responseText);

				if (xhr.status == 400) {
					alert('请求出错')
				}
			}
			// 当网络中断时会触发onerrr事件
			xhr.onerror = function () {
				alert('网络中断, 无法发送Ajax请求')
			}
		}

		// Ajax状态码: 表示Ajax请求的过程状态 ajax对象返回的
		// Http状态码: 表示请求的处理结果 是服务器端返回的
	</script>
</body>
</html>
```
### 1.12  Ajax缓存
```markdown
低版本 IE 浏览器的缓存问题
问题：在低版本的 IE 浏览器中，Ajax 请求有严重的缓存问题，即
在请求地址不发生变化的情况下，只有第一次请求会真正发送
到服务器端，后续的请求都会从浏览器的缓存中获取结果。
即使服务器端的数据更新了，客户端依然拿到的是缓存中的
旧数据。
解决方案：在请求地址的后面加请求参数，保证
每一次请求中的请求参数的值不相同。 
xhr.open('get', 'http://www.example.com?t=' + Math.random());

```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<button id="btn">发送Ajax请求</button>
	<script type="text/javascript">
		var btn = document.getElementById('btn');

		btn.onclick = function () {
			// 1.创建ajax对象
			var xhr = new XMLHttpRequest();
			// 2.告诉Ajax对象要向哪发送请求，以什么方式发送请求
			// 1)请求方式 2)请求地址
			xhr.open('get', 'http://localhost:3000/cache?t=' + Math.random());
			// 3.发送请求
			xhr.send();
			// 4.获取服务器端响应到客户端的数据
			xhr.onreadystatechange = function (){
				if (xhr.readyState == 4 && xhr.status == 200) {
					alert(xhr.responseText);
				}
			}
			
		}
	</script>
</body>
</html>
```
### 1.13 Ajax 异步编程
```markdown
同步
一个人同一时间只能做一件事情，只有一件事情做完，才能做
另外一件事情。
落实到代码中，就是上一行代码执行完成后，才能执行下一
行代码，即代码逐行执行。
console.log('before'); 
console.log('after');

异步
一个人一件事情做了一半，转而去做其他事情，当其他事情做
完以后，再回过头来继续做之前未完成的事情。

落实到代码上，就是异步代码虽然需要花费时间去执行，但程
序不会等待异步代码执行完成后再继续执行后续代码，
而是直接执行后续代码，当后续代码执行完成后再回头看异
步代码是否返回结果，
如果已有返回结果，再调用事先准备好的回调函数处理异步
代码执行的结果。

console.log('before');
setTimeout(
   () => { console.log('last');
}, 2000);
console.log('after');

```

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script type="text/javascript">
		// 1.创建ajax对象
		var xhr = new XMLHttpRequest();
		// 2.告诉Ajax对象要向哪发送请求，以什么方式发送请求
		// 1)请求方式 2)请求地址
		xhr.open('get', 'http://localhost:3000/first', false);
		// 3.发送请求
		xhr.send();
		// 4.获取服务器端响应到客户端的数据
		xhr.onreadystatechange = function (){
			if (xhr.readyState == 4) {
				console.log('2')
				console.log(xhr.responseText)
			}
			
		}

		console.log('1');
	</script>
</body>
</html>
```
### 1.14 ajax函数封装
```markdown
问题：发送一次请求代码过多，发送多次请求代码冗余且重复。
解决方案：将请求代码封装到函数中，发请求时调用函数即可。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script type="text/javascript">
		function ajax (options) {
			// 存储的是默认值
			var defaults = {
				type: 'get',
				url: '',
				data: {},
				header: {
					'Content-Type': 'application/x-www-form-urlencoded'
				},
				success: function () {},
				error: function () {}
			};

			// 使用options对象中的属性覆盖defaults对象中的属性
			Object.assign(defaults, options); //options会覆盖defaults原有的属性

			// 创建ajax对象
			var xhr = new XMLHttpRequest();
			// 拼接请求参数的变量
			var params = '';
			// 循环用户传递进来的对象格式参数
			for (var attr in defaults.data) {
				// 将参数转换为字符串格式
				params += attr + '=' + defaults.data[attr] + '&';
			}
			// 将参数最后面的&截取掉 
			// 将截取的结果重新赋值给params变量
			params = params.substr(0, params.length - 1);

			// 判断请求方式
			if (defaults.type == 'get') {
				defaults.url = defaults.url + '?' + params;
			}

			/*
				{
					name: 'zhangsan',
					age: 20
				}

				name=zhangsan&age=20

			 */

			// 配置ajax对象
			xhr.open(defaults.type, defaults.url);
			// 如果请求方式为post
			if (defaults.type == 'post') {
				// 用户希望的向服务器端`		`传递的请求参数的类型
				var contentType = defaults.header['Content-Type']
				// 设置请求参数格式的类型
				xhr.setRequestHeader('Content-Type', contentType);
				// 判断用户希望的请求参数格式的类型
				// 如果类型为json
				if (contentType == 'application/json') {
					// 向服务器端传递json数据格式的参数
					xhr.send(JSON.stringify(defaults.data))
				}else {
					// 向服务器端传递普通类型的请求参数
					xhr.send(params);
				}

			}else {
				// 发送请求
				xhr.send();
			}
			// 监听xhr对象下面的onload事件
			// 当xhr对象接收完响应数据后触发
			xhr.onload = function () {

				// xhr.getResponseHeader()
				// 获取响应头中的数据
				var contentType = xhr.getResponseHeader('Content-Type');
				// 服务器端返回的数据
				var responseText = xhr.responseText;

				// 如果响应类型中包含applicaition/json
				if (contentType.includes('application/json')) {
					// 将json字符串转换为json对象
					responseText = JSON.parse(responseText)
				}

				// 当http状态码等于200的时候
				if (xhr.status == 200) {
					// 请求成功 调用处理成功情况的函数
					defaults.success(responseText, xhr);
				}else {
					// 请求失败 调用处理失败情况的函数
					defaults.error(responseText, xhr);
				}
			}
		}

		ajax({
			type: 'post',
																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																	`// 请求地址
			url: 'http://localhost:3000/responseData',
			success: function (data) {
				console.log('这里是success函数');
				console.log(data)
			}
		})

		/*
			请求参数要考虑的问题

				1.请求参数位置的问题

					将请求参数传递到ajax函数内部, 在函数内部根据请求方式的不同将请求参数放置在不同的位置

					get 放在请求地址的后面

					post 放在send方法中

				2.请求参数格式的问题

					application/x-www-form-urlencoded

						参数名称=参数值&参数名称=参数值

						name=zhangsan&age=20

					application/json

						{name: 'zhangsan', age: 20}

					1.传递对象数据类型对于函数的调用者更加友好
					2.在函数内部对象数据类型转换为字符串数据类型更加方便

		*/
	</script>
</body>
</html>
```
## 2 编程扩展

### 2.1 模板引擎
```markdown
作用：使用模板引擎提供的模板语法，可以将数据和 HTML 拼接起来。
官方地址： https://aui.github.io/art-template/zh-cn/index.html

安装
服务器端:npm install art-template --save
客户端:
官方地址： https://aui.github.io/art-template/zh-cn/index.html


```
#### 2.1.1  客户端模板引擎使用步骤
```markdown
使用步骤
1.下载 art-template 模板引擎库文件并在 HTML 页面中引入库文件
2.准备 art-template 模板
<script id="tpl" type="text/html">
     <div class="box"> {{ username }} </div>
 </script>
3. 告诉模板引擎将哪一个模板和哪个数据进行拼接
var html = template('tpl', {username: 'zhangsan', age: '20'});
document.getElementById('container').innerHTML = html;
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<!-- 1. 将模板引擎的库文件引入到当前页面 -->
	<script src="/js/template-web.js"></script>
</head>
<body>
	<div id="container"></div>
	<!-- 2.准备art-template模板 -->
	<script type="text/html" id="tpl">
		<h1>{{username}} {{age}}</h1>
	</script>
	<script type="text/javascript">
		// 3.告诉模板引擎将那个数据和哪个模板进行拼接
		// 1) 模板id 2)数据 对象类型
		// 方法的返回值就是拼接好的html字符串
		var html = template('tpl', {username: 'zhangsan', age: 30});
		document.getElementById('container').innerHTML = html;
	</script>
</body>
</html>
```
### 2.2 验证邮箱地址唯一性
```markdown
获取文本框并为其添加离开焦点事件
离开焦点时，检测用户输入的邮箱地址是否符合规则
如果不符合规则，阻止程序向下执行并给出提示信息
向服务器端发送请求，检测邮箱地址是否被别人注册
根据服务器端返回值决定客户端显示何种提示信息
```
```markdown
ajax.js
```
```js
function ajax (options) {
	// 默认值
	var defaults = {
		type: 'get',
		url: '',
		async: true,
		data: {},
		header: {
			'Content-Type': 'application/x-www-form-urlencoded'
		},
		success: function () {},
		error: function () {}
	}
	// 使用用户传递的参数替换默认值参数
	Object.assign(defaults, options);
	// 创建ajax对象
	var xhr = new XMLHttpRequest();
	// 参数拼接变量
	var params = '';
	// 循环参数
	for (var attr in defaults.data) {
		// 参数拼接
		params += attr + '=' + defaults.data[attr] + '&';
		// 去掉参数中最后一个&
		params = params.substr(0, params.length-1)
	}
	// 如果请求方式为get
	if (defaults.type == 'get') {
		// 将参数拼接在url地址的后面
		defaults.url += '?' + params;
	}

	// 配置ajax请求
	xhr.open(defaults.type, defaults.url, defaults.async);
	// 如果请求方式为post
	if (defaults.type == 'post') {
		// 设置请求头
		xhr.setRequestHeader('Content-Type', defaults.header['Content-Type']);
		// 如果想服务器端传递的参数类型为json
		if (defaults.header['Content-Type'] == 'application/json') {
			// 将json对象转换为json字符串
			xhr.send(JSON.stringify(defaults.data))
		}else {
			// 发送请求
			xhr.send(params);
		}
	} else {
		xhr.send();
	}
	// 请求加载完成
	xhr.onload = function () {
		// 获取服务器端返回数据的类型
		var contentType = xhr.getResponseHeader('content-type');
		// 获取服务器端返回的响应数据
		var responseText = xhr.responseText;
		// 如果服务器端返回的数据是json数据类型
		if (contentType.includes('application/json')) {
			// 将json字符串转换为json对象
			responseText = JSON.parse(responseText);
		}
		// 如果请求成功
		if (xhr.status == 200) {
			// 调用成功回调函数, 并且将服务器端返回的结果传递给成功回调函数
			defaults.success(responseText, xhr);
		} else {
			// 调用失败回调函数并且将xhr对象传递给回调函数
			defaults.error(responseText, xhr);
		} 
	}
	// 当网络中断时
	xhr.onerror = function () {
		// 调用失败回调函数并且将xhr对象传递给回调函数
		defaults.error(xhr);
	}
}
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>验证邮箱地址是否已经注册</title>
	<link rel="stylesheet" href="/assets/bootstrap/dist/css/bootstrap.min.css">
	<style type="text/css">
		p:not(:empty) {
			padding: 15px;
		}
		.container {
			padding-top: 100px;
		}
	</style>
</head>
<body>
	<div class="container">
		<div class="form-group">
			<label>邮箱地址</label>
			<input type="email" class="form-control" placeholder="请输入邮箱地址" id="email">
		</div>
		<!-- 错误 bg-danger 正确 bg-success -->
		<p id="info"></p>
	</div>
	<script src="/js/ajax.js"></script>
	<script>
		// 获取页面中的元素
		var emailInp = document.getElementById('email');
		var info = document.getElementById('info');

		// 当文本框离开焦点以后
		emailInp.onblur = function () {
			// 获取用户输入的邮箱地址
			var email = this.value;
			// 验证邮箱地址的正则表达式
			var reg = /^[A-Za-z\d]+([-_.][A-Za-z\d]+)*@([A-Za-z\d]+[-.])+[A-Za-z\d]{2,4}$/;
			// 如果用户输入的邮箱地址不符合规则
			if (!reg.test(email)) {
				// 给出用户提示
				info.innerHTML = '请输入符合规则的邮箱地址';
				// 让提示信息显示为错误提示信息的样式
				info.className = 'bg-danger';
				// 阻止程序向下执行
				return;
			}

			// 向服务器端发送请求
			ajax({
				type: 'get',
				url: 'http://localhost:3000/verifyEmailAdress',
				data: {
					email: email
				},
				success: function (result) {
					console.log(result);
					info.innerHTML = result.message;
					info.className = 'bg-success';
				},
				error: function (result) {
					console.log(result)
					info.innerHTML = result.message;
					info.className = 'bg-danger';
				}
			});

		}
	</script>
</body>
</html>
```

### 2.3 搜索框内容自动提示
```markdown
获取搜索框并为其添加用户输入事件
获取用户输入的关键字
向服务器端发送请求并携带关键字作为请求参数
将响应数据显示在搜索框底部
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>搜索框输入文字自动提示</title>
	<link rel="stylesheet" href="/assets/bootstrap/dist/css/bootstrap.min.css">
	<style type="text/css">
		.container {
			padding-top: 150px;
		}
		.list-group {
			display: none;
		}
	</style>
</head>
<body>
	<div class="container">
		<div class="form-group">
			<input type="text" class="form-control" placeholder="请输入搜索关键字" id="search">
			<ul class="list-group" id="list-box">
				
			</ul>
		</div>
	</div>
	<script src="/js/ajax.js"></script>
	<script src="/js/template-web.js"></script>
	<script type="text/html" id="tpl">
		{{each result}}
			<li class="list-group-item">{{$value}}</li>
		{{/each}}
	</script>
	<script>
		// 获取搜索框
		var searchInp = document.getElementById('search');
		// 获取提示文字的存放容器
		var listBox = document.getElementById('list-box');
		// 存储定时器的变量
		var timer = null;
		// 当用户在文本框中输入的时候触发
		searchInp.oninput = function () {
			// 清除上一次开启的定时器
			clearTimeout(timer);
			// 获取用户输入的内容
			var key = this.value;
			// 如果用户没有在搜索框中输入内容
			if (key.trim().length == 0) {
				// 将提示下拉框隐藏掉
				listBox.style.display = 'none';
				// 阻止程序向下执行
				return;
			}

			// 开启定时器 让请求延迟发送
			timer = setTimeout(function () {
				// 向服务器端发送请求
				// 向服务器端索取和用户输入关键字相关的内容
				ajax({
					type: 'get',
					url: 'http://localhost:3000/searchAutoPrompt',
					data: {
						key: key
					},
					success: function (result) {
						// 使用模板引擎拼接字符串
						var html = template('tpl', {result: result});
						// 将拼接好的字符串显示在页面中
						listBox.innerHTML = html;
						// 显示ul容器
						listBox.style.display = 'block';
					}
				})
			}, 800)
			
		}
	</script>
</body>
</html>
```
### 2.4 省市区联动
```markdown
通过接口获取省份信息
使用JavaScript获取到省市区下拉框元素
将服务器端返回的省份信息显示在下拉框中
为下拉框元素添加表单值改变事件（onchange）
当用户选择省份时，根据省份id获取城市信息
当用户选择城市时，根据城市id获取县城信息
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>搜索框输入文字自动提示</title>
	<link rel="stylesheet" href="/assets/bootstrap/dist/css/bootstrap.min.css">
	<style type="text/css">
		.container {
			padding-top: 150px;
		}
	</style>
</head>
<body>
	<div class="container">
		<div class="form-inline">
			<div class="form-group">
				<select class="form-control" id="province"></select>
			</div>
			<div class="form-group">
				<select class="form-control" id="city">
					<option>请选择城市</option>
				</select>
			</div>
			<div class="form-group">
				<select class="form-control" id="area">
					<option>请选择县城</option>
				</select>
			</div>
		</div>
	</div>
	<script src="/js/ajax.js"></script>
	<script src="/js/template-web.js"></script>
	<!-- 省份模板 -->
	<script type="text/html" id="provinceTpl">
		<option>请选择省份</option>
		{{each province}}
			<option value="{{$value.id}}">{{$value.name}}</option>
		{{/each}}
	</script>
	<!-- 城市模板 -->
	<script type="text/html" id="cityTpl">
		<option>请选择城市</option>
		{{each city}}
			<option value="{{$value.id}}">{{$value.name}}</option>
		{{/each}}
	</script>
	<!-- 县城模板 -->
	<script type="text/html" id="areaTpl">
		<option>请选择县城</option>
		{{each area}}
			<option value="{{$value.id}}">{{$value.name}}</option>
		{{/each}}
	</script>
	<script>
		// 获取省市区下拉框元素
		var province = document.getElementById('province');
		var city = document.getElementById('city');
		var area = document.getElementById('area');
		// 获取省份信息
		ajax({
			type: 'get',
			url: 'http://localhost:3000/province',
			success: function (data) {
				// 将服务器端返回的数据和html进行拼接
				var html = template('provinceTpl', {province: data});
				// 将拼接好的html字符串显示在页面中
				province.innerHTML = html;
			}
		});

		// 为省份的下拉框添加值改变事件
		province.onchange = function () {
			// 获取省份id
			var pid = this.value;

			// 清空县城下拉框中的数据
			var html = template('areaTpl', {area: []});
			area.innerHTML = html;

			// 根据省份id获取城市信息
			ajax({
				type: 'get',
				url: '/cities',
				data: {
					id: pid
				},
				success: function (data) {
					var html = template('cityTpl', {city: data});
					city.innerHTML = html;
				}
			})
		};

		// 当用户选择城市的时候
		city.onchange = function () {
			// 获取城市id
			var cid = this.value;
			// 根据城市id获取县城信息
			ajax({
				type: 'get',
				url: 'http://localhost:3000/areas',
				data: {
					id: cid
				},
				success: function(data) {
					var html = template('areaTpl', {area: data});
					area.innerHTML = html;
				}
			})
		}
	</script>
</body>
</html>
```
### 2.5 FormData
```markdown
模拟HTML表单，相当于将HTML表单映射成表单对象，自
动将表单对象中的数据拼接成请求参数的格式。
异步上传二进制文件
```
#### 2.5.1 FormData对象的使用方法
```markdown
1.准备 HTML 表单
<form id="form">
     <input type="text" name="username" />
     <input type="password" name="password" />
     <input type="button"/>
</form>
2.将 HTML 表单转化为 formData 对象
var form = document.getElementById('form'); 
var formData = new FormData(form);
3.提交表单对象
xhr.send(formData);
注意：
Formdata 对象不能用于 get 请求，因为对象需要被传递
到 send 方法中，而 get 请求方式的请求参数只能放在请
求地址的后面。
服务器端 bodyParser 模块不能解析 formData 对象表单
数据，我们需要使用 formidable 模块进行解析。
```
```markdown
app.js
```
```js
// 引入express框架
const express = require('express');
// 路径处理模块
const path = require('path');
const formidable = require('formidable');
// 创建web服务器
const app = express();

// 静态资源访问服务功能
app.use(express.static(path.join(__dirname, 'public')));

// 邮箱地址验证
app.get('/verifyEmailAdress', (req, res) => {
	// 接收客户端传递过来的邮箱地址
	const email = req.query.email;
	// 判断邮箱地址注册过的情况
	if (email == 'ityxj@itcast.cn') {
		// 设置http状态码并对客户端做出响应
		res.status(400).send({message: '邮箱地址已经注册过了, 请更换其他邮箱地址'});
	} else {
		// 邮箱地址可用的情况
		// 对客户端做出响应
		res.send({message: '恭喜, 邮箱地址可用'});
	} 
});

// 输入框文字提示
app.get('/searchAutoPrompt', (req, res) => {
	// 搜索关键字
	const key = req.query.key;
	// 提示文字列表
	const list = [
		'黑马程序员',
		'黑马程序员官网',
		'黑马程序员顺义校区',
		'黑马程序员学院报名系统',
		'传智播客',
		'传智博客前端与移动端开发',
		'传智播客大数据',
		'传智播客python',
		'传智播客java',
		'传智播客c++',
		'传智播客怎么样'
	];
	// 搜索结果
	let result = list.filter(item => item.includes(key));
	// 将查询结果返回给客户端
	res.send(result);
});

// 获取省份
app.get('/province', (req, res) => {
	res.json([{
		id: '001',
		name: '黑龙江省'
	},{
		id: '002',
		name: '四川省'
	},{
		id: '003',
		name: '河北省'
	},{
		id: '004',
		name: '江苏省'
	}]);
});

// 根据省份id获取城市
app.get('/cities', (req, res) => {
	// 获取省份id
	const id = req.query.id;
	// 城市信息
	const cities = {
		'001': [{
			id: '300',
			name: '哈尔滨市'
		}, {
			id: '301',
			name: '齐齐哈尔市'
		}, {
			id: '302',
			name: '牡丹江市'
		}, {
			id: '303',
			name: '佳木斯市'
		}],
		'002': [{
			id: '400',
			name: '成都市'
		}, {
			id: '401',
			name: '绵阳市'
		}, {
			id: '402',
			name: '德阳市'
		}, {
			id: '403',
			name: '攀枝花市'
		}],
		'003': [{
			id: '500',
			name: '石家庄市'
		}, {
			id: '501',
			name: '唐山市'
		}, {
			id: '502',
			name: '秦皇岛市'
		}, {
			id: '503',
			name: '邯郸市'
		}],
		'004': [{
			id: '600',
			name: '常州市'
		}, {
			id: '601',
			name: '徐州市'
		}, {
			id: '602',
			name: '南京市'
		}, {
			id: '603',
			name: '淮安市'
		}]
	}
	// 响应
	res.send(cities[id]);
});

// 根据城市id获取县城
app.get('/areas', (req, res) => {
	// 获取城市id
	const id = req.query.id;
	// 县城信息
	const areas = {
		'300': [{
			id: '20',
			name: '道里区',
		}, {
			id: '21',
			name: '南岗区'
		}, {
			id: '22',
			name: '平房区',
		}, {
			id: '23',
			name: '松北区'
		}],
		'301': [{
			id: '30',
			name: '龙沙区'
		}, {
			id: '31',
			name: '铁锋区'
		}, {
			id: '32',
			name: '富拉尔基区'
		}]
	};
	// 响应
	res.send(areas[id] || []);
});

app.post('/formData', (req, res) => {
	// 创建formidable表单解析对象
	const form = new formidable.IncomingForm();
	// 解析客户端传递过来的FormData对象
	form.parse(req, (err, fields, files) => {
		res.send(fields);
	});
});

// 实现文件上传的路由
app.post('/upload', (req, res) => {
	// 创建formidable表单解析对象
	const form = new formidable.IncomingForm();
	// 设置客户端上传文件的存储路径
	form.uploadDir = path.join(__dirname, 'public', 'uploads');
	// 保留上传文件的后缀名字
	form.keepExtensions = true;
	// 解析客户端传递过来的FormData对象
	form.parse(req, (err, fields, files) => {
		// 将客户端传递过来的文件地址响应到客户端
		res.send({
			path: files.attrName.path.split('public')[1]
		});
	});
});

// 监听端口
app.listen(3000);
// 控制台提示输出
console.log('服务器启动成功');
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<!-- 创建普通的html表单 -->
	<form id="form">
		<input type="text" name="username">
		<input type="password" name="password">
		<input type="button" id="btn" value="提交">
	</form>
	<script type="text/javascript">
		// 获取按钮
		var btn = document.getElementById('btn');
		// 获取表单
		var form = document.getElementById('form');
		// 为按钮添加点击事件
		btn.onclick = function () {
			// 将普通的html表单转换为表单对象
			var formData = new FormData(form);
			// 创建ajax对象
			var xhr = new XMLHttpRequest();
			// 对ajax对象进行配置
			xhr.open('post', 'http://localhost:3000/formData');
			// 发送ajax请求
			xhr.send(formData);
			// 监听xhr对象下面的onload事件
			xhr.onload = function () {
				// 对象http状态码进行判断
				if (xhr.status == 200) {
					console.log(xhr.responseText);
				}
			}
		}
	</script>
</body>
</html>
```
#### 2.5.2 FormData对象下的实例方法
```markdown
获取表单对象中属性的值
formData.get('key');
设置表单对象中属性的值
formData.set('key', 'value');
删除表单对象中属性的值
formData.delete('key');
向表单对象中追加属性值
formData.append('key', 'value');
注意：set 方法与 append 方法的区别是，在属性名已存
在的情况下，set 会覆盖已有键名的值，append会保留两个值。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<!-- 创建普通的html表单 -->
	<form id="form">
		<input type="text" name="username">
		<input type="password" name="password">
		<input type="button" id="btn" value="提交">
	</form>
	<script type="text/javascript">
		// 获取按钮
		var btn = document.getElementById('btn');
		// 获取表单
		var form = document.getElementById('form');
		// 为按钮添加点击事件
		btn.onclick = function () {
			// 将普通的html表单转换为表单对象
			var formData = new FormData(form);

			/*
				get('key') 获取表单对象属性的值
				set('key', 'value') 设置表单对象属性的值
				delete('key') 删除表单对象属性中的值
			*/
		
			console.log(formData.get('username'));

			// 如果设置的表单属性存在 将会覆盖属性原有的值
			formData.set('username', 'itcast');
			formData.append('username', 'ityxj');
			// 如果设置的表单属性不存在 将会创建这个表单属性
			formData.set('age', 100);
			// 删除用户输入的密码
			formData.delete('password');

			// 创建ajax对象
			var xhr = new XMLHttpRequest();
			// 对ajax对象进行配置
			xhr.open('post', 'http://localhost:3000/formData');
			// 发送ajax请求
			xhr.send(formData);
			// 监听xhr对象下面的onload事件
			xhr.onload = function () {
				// 对象http状态码进行判断
				if (xhr.status == 200) {
					console.log(xhr.responseText);
				}
			}

			// 创建空的表单对象
			var f = new FormData();
			f.append('sex', '男');
			console.log(f.get('sex'));
		}
	</script>
</body>
</html>
```
#### 2.5.3 FormData对象实现二进制文件上传
```markdown
<input type="file" id="file"/>
var file = document.getElementById('file')
// 当用户选择文件的时候
 file.onchange = function () {
     // 创建空表单对象
     var formData = new FormData();
     // 将用户选择的二进制文件追加到表单对象中
     formData.append('attrName', this.files[0]);
     // 配置ajax对象，请求方式必须为post
     xhr.open('post', 'www.example.com');
     xhr.send(formData);
 }

FormData 文件上传进度展示
// 当用户选择文件的时候
 file.onchange = function () {
     // 文件上传过程中持续触发onprogress事件
     xhr.upload.onprogress = function (ev) {
         // 当前上传文件大小/文件总大小 再将结果转换为百分数
         // 将结果赋值给进度条的宽度属性 
         bar.style.width = (ev.loaded / ev.total) * 100 + '%';
     }
 }


FormData 文件上传图片即时预览
在我们将图片上传到服务器端以后，服务器端通常都会将图片地址做为响应数据传递到客户端，客户端可以从响应数据中获取图片地址，然后将图片再显示在页面中。
xhr.onload = function () {
     var result = JSON.parse(xhr.responseText);
     var img = document.createElement('img');
     img.src = result.src;
     img.onload = function () {
         document.body.appendChild(this);
     }
 }


```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<link rel="stylesheet" href="/assets/bootstrap/dist/css/bootstrap.min.css">
	<style type="text/css">
		.container {
			padding-top: 60px;
		}
		.padding {
			padding: 5px 0 20px 0;
		}
	</style>
</head>
<body>
	<div class="container">
		<div class="form-group">
			<label>请选择文件</label>
			<input type="file" id="file">
			<div class="padding" id="box">
				<!--<img src="" class="img-rounded img-responsive">-->
			</div>
			<div class="progress">
				<div class="progress-bar" style="width: 0%;" id="bar">0%</div>
			</div>
		</div>
	</div>
	<script type="text/javascript">
		// 获取文件选择控件
		var file = document.getElementById('file');
		// 获取进度条元素
		var bar = document.getElementById('bar');
		// 获取图片容器
		var box = document.getElementById('box');
		// 为文件选择控件添加onchanges事件
		// 在用户选择文件时触发
		file.onchange = function () {
			// 创建空的formData表单对象
			var formData = new FormData();
			// 将用户选择的文件追加到formData表单对象中
			formData.append('attrName', this.files[0]);
			// 创建ajax对象
			var xhr = new XMLHttpRequest();
			// 对ajax对象进行配置
			xhr.open('post', 'http://localhost:3000/upload');
			// 在文件上传的过程中持续触发
			xhr.upload.onprogress = function (ev) {
				// ev.loaded 文件已经上传了多少
				// ev.total  上传文件的总大小
				var result = (ev.loaded / ev.total) * 100 + '%';
				// 设置进度条的宽度
				bar.style.width = result;
				// 将百分比显示在进度条中
				bar.innerHTML = result;
			}
			// 发送ajax请求
			xhr.send(formData);
			// 监听服务器端响应给客户端的数据
			xhr.onload = function () {
				// 如果服务器端返回的http状态码为200
				// 说明请求是成功的
				if (xhr.status == 200) {
					// 将服务器端返回的数据显示在控制台中
					var result = JSON.parse(xhr.responseText);
					// 动态创建img标签
					var img = document.createElement('img');
					// 给图片标签设置src属性
					img.src = result.path;
					// 当图片加载完成以后
					img.onload = function () {
						// 将图片显示在页面中
						box.appendChild(img);
					}
				}
			}
			
		}
	</script>
</body>
</html>

```
### 2.6 同源政策
```markdown
Ajax 只能向自己的服务器发送请求。比如现在有一个A网站、有
一个B网站，A网站中的 HTML 文件只能向A网站服务器中发
送 Ajax 请求，B网站中的 HTML 文件只能向 B 网站中发
送 Ajax 请求，但是 A 网站是不能向 B 网站发送 Ajax请求
的，同理，B 网站也不能向 A 网站发送 Ajax请求。
如果两个页面拥有相同的协议、域名和端口，那么这两个页
面就属于同一个源，其中只要有一个不相同，就是不同源。
http://www.example.com/dir/page.html
http://www.example.com/dir2/other.html：同源
http://example.com/dir/other.html：不同源（域名不同）
http://v2.www.example.com/dir/other.html：不同源（域名不同）
http://www.example.com:81/dir/other.html：不同源（端口不同）
https://www.example.com/dir/page.html：不同源（协议不同）
同源政策是为了保证用户信息的安全，防止恶意的网站
窃取数据。最初的同源政策是指 A 网站在客户端设置
的 Cookie，B网站是不能访问的。
随着互联网的发展，同源政策也越来越严格，在不同
源的情况下，其中有一项规定就是无法向非同源地址发
送Ajax 请求，如果请求，浏览器就会报错。
```
#### 2.6.1 测试非同源Ajax请求
```markdown
s1/app.js
```

```js
// 引入express框架
const express = require('express');
// 路径处理模块
const path = require('path');
// 向其他服务器端请求数据的模块
const request = require('request');
// 创建web服务器
const app = express();
// 静态资源访问服务功能
app.use(express.static(path.join(__dirname, 'public')));

app.get('/server', (req, res) => {
	request('http://localhost:3001/cross', (err, response, body) => {
		res.send(body);
	})
});

// 监听端口
app.listen(3000);
// 控制台提示输出
console.log('服务器启动成功');
```
```markdown
s2/app.js
```

```js
// 引入express框架
const express = require('express');
// 路径处理模块
const path = require('path');
// 接收post请求参数
const formidable = require('formidable');
// 实现session功能
var session = require('express-session');
// 创建web服务器
const app = express();
// 接收post请求参数
// 实现session功能
app.use(session({
  secret: 'keyboard cat',
  resave: false,
  saveUninitialized: false
}));

// 静态资源访问服务功能
app.use(express.static(path.join(__dirname, 'public')));

// 拦截所有请求
// app.use((req, res, next) => {
// 	// 1.允许哪些客户端访问我
// 	// * 代表允许所有的客户端访问我
// 	res.header('Access-Control-Allow-Origin', '*')
// 	// 2.允许客户端使用哪些请求方法访问我
// 	res.header('Access-Control-Allow-Methods', 'get,post')
// 	next();
// });

app.get('/test', (req, res) => {
	const result = 'fn({name: "张三"})';
	res.send(result);
});

app.get('/better', (req, res) => {
	// 接收客户端传递过来的函数的名称
	//const fnName = req.query.callback;
	// 将函数名称对应的函数调用代码返回给客户端
	//const data = JSON.stringify({name: "张三"});
	//const result = fnName + '('+ data +')';
	// setTimeout(() => {
	// 	res.send(result);
	// }, 1000)
	res.jsonp({name: 'lisi', age: 20});
});

app.get('/cross', (req, res) => {
	res.send('ok')
});

// 拦截所有请求
app.use((req, res, next) => {
	// 1.允许哪些客户端访问我
	// * 代表允许所有的客户端访问我
	// 注意：如果跨域请求中涉及到cookie信息传递，值不可以为*号 比如是具体的域名信息
	res.header('Access-Control-Allow-Origin', 'http://localhost:3000')
	// 2.允许客户端使用哪些请求方法访问我
	res.header('Access-Control-Allow-Methods', 'get,post')
	// 允许客户端发送跨域请求时携带cookie信息
	res.header('Access-Control-Allow-Credentials', true);
	next();
});

app.post('/login', (req, res) => {
	// 创建表单解析对象
	var form = formidable.IncomingForm();
	// 解析表单
	form.parse(req, (err, fields, file) => {
		// 接收客户端传递过来的用户名和密码
		const { username, password } = fields;
		// 用户名密码比对
		if (username == 'ityxj' && password == '123456') {
			// 设置session
			req.session.isLogin = true;
			res.send({message: '登录成功'});
		} else {
			res.send({message: '登录失败, 用户名或密码错误'});
		}
	})
});

app.get('/checkLogin', (req, res) => {
	// 判断用户是否处于登录状态
	if (req.session.isLogin) {
		res.send({message: '处于登录状态'})
	} else {
		res.send({message: '处于未登录状态'})
	}
});




// 监听端口
app.listen(3001);
// 控制台提示输出
console.log('服务器启动成功');
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script type="text/javascript" src="/js/ajax.js"></script>
	<script type="text/javascript">
		ajax({
			url: 'http://localhost:3001/test',
			type: 'get',
			success: function (result) {
				console.log(result);
			}
		})
	</script>
</body>
</html>
```
####  2.6.2 使用 JSONP 解决同源限制问题
```markdown
jsonp 是 json with padding 的缩写，它不属于 Ajax 请求，但它可以模拟 Ajax 请求。
1.   将不同源的客户端端请求地址写在 script 标签的 src 属性中
<script src="www.example.com"></script>
<script src=“https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>
2.   服务器端响应数据必须是一个函数的调用，真正要发送给客户端的数据需要作为函数调用的参数。
const data = 'fn({name: "张三", age: "20"})';
 res.send(data);
3.   在客户端全局作用域下定义函数 fn
function fn (data) { }
4.   在 fn 函数内部对服务器端返回的数据进行处理
function fn (data) { console.log(data); }

```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script>
		function fn (data) {
			console.log('客户端的fn函数被调用了')
			console.log(data);
		}
	</script>
	<!-- 1.将非同源服务器端的请求地址写在script标签的src属性中 -->
	<script src="http://localhost:3001/test"></script>
</body>
</html>
```
#### 2.6.3 封装jsonp方法
```markdown
客户端需要将函数名称传递到服务器端。
将 script 请求的发送变成动态请求。
封装 jsonp 函数，方便请求发送。
服务器端代码优化之 res.jsonp 方法。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<button id="btn1">点我发送请求</button>
	<button id="btn2">点我发送请求</button>
	<script type="text/javascript">
		// 获取按钮
		var btn1 = document.getElementById('btn1');
		var btn2 = document.getElementById('btn2');
		// 为按钮添加点击事件
		btn1.onclick = function () {
			jsonp({
				// 请求地址
				url: 'http://localhost:3001/better',
				data: {
					name: 'lisi',
					age: 30
				},
				success: function (data) {
					console.log(123)
					console.log(data)
				}
			})
		}

		btn2.onclick = function () {
			jsonp({
				// 请求地址
				url: 'http://localhost:3001/better',
				success: function (data) {
					console.log(456789)
					console.log(data)
				}
			})
		}

		function jsonp (options) {
			// 动态创建script标签
			var script = document.createElement('script');
			// 拼接字符串的变量
			var params = '';

			for (var attr in options.data) {
				params += '&' + attr + '=' + options.data[attr];
			}
			
			// myJsonp0124741
			var fnName = 'myJsonp' + Math.random().toString().replace('.', '');
			// 它已经不是一个全局函数了
			// 我们要想办法将它变成全局函数
			window[fnName] = options.success;
			// 为script标签添加src属性
			script.src = options.url + '?callback=' + fnName + params;
			// 将script标签追加到页面中
			document.body.appendChild(script);
			// 为script标签添加onload事件
			script.onload = function () {
				document.body.removeChild(script);
			}
		}

		
	</script>
</body>
</html>
```
#### 2.6.4 使用jsonp获取腾讯天气信息
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>使用jsonp获取腾讯天气信息</title>
	<link rel="stylesheet" href="/assets/bootstrap/dist/css/bootstrap.min.css">
	<style type="text/css">
		.container {
			padding-top: 60px;
		}
	</style>
</head>
<body>
	<div class="container">
		<table class="table table-striped table-hover" align="center" id="box"></table>
	</div>
	<script src="/js/jsonp.js"></script>
	<script src="/js/template-web.js"></script>
	<script type="text/html" id="tpl">
		<tr>
			<th>时间</th>
			<th>温度</th>
			<th>天气</th>
			<th>风向</th>
			<th>风力</th>
		</tr>
		{{each info}}
		<tr>
			<td>{{dateFormat($value.update_time)}}</td>
			<td>{{$value.degree}}</td>
			<td>{{$value.weather}}</td>
			<td>{{$value.wind_direction}}</td>
			<td>{{$value.wind_power}}</td>
		</tr>
		{{/each}}
	</script>
	<script>
		// 获取table标签
		var box = document.getElementById('box');

		function dateFormat(date) {
			var year = date.substr(0, 4);
			var month = date.substr(4, 2);
			var day = date.substr(6, 2);
			var hour = date.substr(8, 2);
			var minute = date.substr(10, 2);
			var seconds = date.substr(12, 2);

			return year + '年' + month + '月' + day + '日' + hour + '时' + minute + '分' + seconds + '秒';
		}

		// 向模板中开放外部变量
		template.defaults.imports.dateFormat = dateFormat;

		// 向服务器端获取天气信息
		jsonp({
			url: 'https://wis.qq.com/weather/common',
			data: {
				source: 'pc',
				weather_type: 'forecast_1h',
				// weather_type: 'forecast_1h|forecast_24h',
				province: '黑龙江省',
				city: '哈尔滨市'
			},
			success: function (data) {
				var html = template('tpl', {info: data.data.forecast_1h});
				box.innerHTML = html;
				
			}
		})
	</script>
</body>
</html>
```
#### 2.6.5 访问非同源数据的服务器端解决方法-CORS 跨域资源共享
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020081508491123.png#pic_center)
```markdown
CORS：全称为 Cross-origin resource sharing，即跨域资源共享，它允许浏览器向跨域服务器发送 Ajax 请求，克服了 Ajax 只能同源使用的限制。
origin: http://localhost:3000
Access-Control-Allow-Origin: 'http://localhost:3000'
Access-Control-Allow-Origin: '*'
Node 服务器端设置响应头示例代码：
app.use((req, res, next) => {
     res.header('Access-Control-Allow-Origin', '*');  // 允许所有客户端访问
     res.header('Access-Control-Allow-Methods', 'GET, POST');  // 允许哪些方法访问
     next();
 })

```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<button id="btn">点我发送请求</button>
	<script src="/js/ajax.js"></script>
	<script>
		// 获取按钮
		var btn = document.getElementById('btn');
		// 为按钮添加点击事件
		btn.onclick = function () {
			ajax({
				type: 'get',
				url: 'http://localhost:3001/cross',
				success: function (data) {
					console.log(data)
				}
			})
		};
	</script>
</body>
</html>
```
#### 2.6.6 访问非同源数据的服务器端另一种解决方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200815091127182.png#pic_center)
```markdown
服务器端没有同源政策,只存在于浏览器端。可以通过服务
器实现跨域访问。再把请求结果返回给浏览器端
```
```js
// 引入express框架
const express = require('express');
// 路径处理模块
const path = require('path');
// 向其他服务器端请求数据的模块
const request = require('request');
// 创建web服务器
const app = express();
// 静态资源访问服务功能
app.use(express.static(path.join(__dirname, 'public')));

app.get('/server', (req, res) => {
	request('http://localhost:3001/cross', (err, response, body) => {
		res.send(body);  // body为收到的数据内容
	})
});

// 监听端口
app.listen(3000);
// 控制台提示输出
console.log('服务器启动成功');
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<button id="btn">点我发送请求</button>
	<script src="/js/ajax.js"></script>
	<script>
		// 获取按钮
		var btn = document.getElementById('btn');
		// 为按钮添加点击事件
		btn.onclick = function () {
			ajax({
				type: 'get',
				url: 'http://localhost:3000/server',
				success: function (data) {
					console.log(data);
				}
			})
		};
	</script>
</body>
</html>
```
#### 2.6.7 实现跨域登录功能
```markdown
实现跨域发送ajax请求是默认不带cookie信息
withCredentials属性
在使用Ajax技术发送跨域请求时，默认情况下不会在
请求中携带cookie信息。
withCredentials：指定在涉及到跨域请求时，是否携
带cookie信息，默认值为false
Access-Control-Allow-Credentials：true 允许客户端
发送请求时携带cookie
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>实现跨域功能</title>
	<link rel="stylesheet" href="/assets/bootstrap/dist/css/bootstrap.min.css">
	<style type="text/css">
		.container {
			padding-top: 60px;
		}
	</style>
</head>
<body>
	<div class="container">
		<form id="loginForm">
			<div class="form-group">
				<label>用户名</label>
				<input type="text" name="username" class="form-control" placeholder="请输入用户名">
			</div>
			<div class="form-group">
				<label>密码</label>
				<input type="password" name="password" class="form-control" placeholder="请输入用密码">
			</div>
			<input type="button" class="btn btn-default" value="登录" id="loginBtn">
			<input type="button" class="btn btn-default" value="检测用户登录状态" id="checkLogin">
		</form>
	</div>
	<script type="text/javascript">
		// 获取登录按钮
		var loginBtn = document.getElementById('loginBtn');
		// 获取检测登录状态按钮
		var checkLogin = document.getElementById('checkLogin');
		// 获取登录表单
		var loginForm = document.getElementById('loginForm');
		// 为登录按钮添加点击事件
		loginBtn.onclick = function () {
			// 将html表单转换为formData表单对象
			var formData = new FormData(loginForm);
			// 创建ajax对象
			var xhr = new XMLHttpRequest();
			// 对ajax对象进行配置
			xhr.open('post', 'http://localhost:3001/login');
			// 当发送跨域请求时，携带cookie信息
			xhr.withCredentials = true;
			// 发送请求并传递请求参数
			xhr.send(formData);
			// 监听服务器端给予的响应内容
			xhr.onload = function () {
				console.log(xhr.responseText);
			}
		}

		// 当检测用户状态按钮被点击时
		checkLogin.onclick = function () {
			// 创建ajax对象
			var xhr = new XMLHttpRequest();
			// 对ajax对象进行配置
			xhr.open('get', 'http://localhost:3001/checkLogin');
			// 当发送跨域请求时，携带cookie信息
			xhr.withCredentials = true;
			// 发送请求并传递请求参数
			xhr.send();
			// 监听服务器端给予的响应内容
			xhr.onload = function () {
				console.log(xhr.responseText);
			}
		}
	</script>
</body>
</html>
```
## 3 jQuery中Ajax
### 3.1  $.ajax方法基本使用
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>$.ajax方法基本使用</title>
</head>
<body>
	<button id="btn">发送请求</button>
	<script src="/js/jquery.min.js"></script>
	<script>
		$('#btn').on('click', function () {
			$.ajax({
				// 请求方式
				type: 'post',
				// 请求地址
				url: '/base',
				// 请求成功以后函数被调用
				success: function (response) {
					// response为服务器端返回的数据
					// 方法内部会自动将json字符串转换为json对象
					console.log(response);
				},
				// 请求失败以后函数被调用
				error: function (xhr) {
					console.log(xhr)
				}
			})
		});
	</script>
</body>
</html>
```


### 3.2 $.ajax方法传递请求参数
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>$.ajax方法基本使用</title>
</head>
<body>
	<button id="btn">发送请求</button>
	<script src="/js/jquery.min.js"></script>
	<script>
		var params = {name: 'wangwu', age: 300}
		$('#btn').on('click', function () {
			$.ajax({
				// 请求方式
				type: 'post',
				// 请求地址
				url: '/user',
				// 向服务器端发送的请求参数
				// data: 'name=zhangsan&age=100'
				// data: {
				// 	name: 'zhangsan',
				// 	age: 100
				// },
				data: JSON.stringify(params),
				// 指定参数的格式类型
				contentType: 'application/json',
				// 请求成功以后函数被调用
				success: function (response) {
					// response为服务器端返回的数据
					// 方法内部会自动将json字符串转换为json对象
					console.log(response);
				}
			})
		});
	</script>
</body>
</html>
```
### 3.3 beforeSend方法的说明
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>$.ajax方法基本使用</title>
</head>
<body>
	<button id="btn">发送请求</button>
	<script src="/js/jquery.min.js"></script>
	<script>
		$('#btn').on('click', function () {
			$.ajax({
				// 请求方式
				type: 'post',
				// 请求地址
				url: '/user',
				// 在请求发送之前调用
				beforeSend: function () {
					alert('请求不会被发送')
					// 请求不会被发送
					return false;
				},
				// 请求成功以后函数被调用
				success: function (response) {
					// response为服务器端返回的数据
					// 方法内部会自动将json字符串转换为json对象
					console.log(response);
				}
			})
		});
	</script>
</body>
</html>
```
### 3.4 serialize方法说明
```markdown
作用：将表单中的数据自动拼接成字符串类型的参数
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>serialize方法说明</title>
</head>
<body>
	<form id="form">
		<input type="text" name="username">
		<input type="password" name="password">
		<input type="submit" value="提交">
	</form>
	<script src="/js/jquery.min.js"></script>
	<script type="text/javascript">
		$('#form').on('submit', function () {
			// 将表单内容拼接成字符串类型的参数
			// var params = $('#form').serialize();
			// console.log(params)
			serializeObject($(this));  // $(this)表示表单这个对象
			return false;  // 阻止表单提交
		});

		// 将表单中用户输入的内容转换为对象类型
		function serializeObject (obj) {
			// 处理结果对象
			var result = {};
			// [{name: 'username', value: '用户输入的内容'}, {name: 'password', value: '123456'}]
			var params = obj.serializeArray();

			// 循环数组 将数组转换为对象类型
			$.each(params, function (index, value) {
				result[value.name] = value.value;
			})
			// 将处理的结果返回到函数外部
			return result;
		}

	</script>
</body>
</html>
```
### 3.5 $.ajax方法发送jsonp请求
```html


<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>$.ajax方法基本使用</title>
</head>
<body>
	<button id="btn">发送请求</button>
	<script src="/js/jquery.min.js"></script>
	<script>

		function fn (response) {
			console.log(response)
		}

		$('#btn').on('click', function () {
			$.ajax({
				url: '/jsonp',
				// 向服务器端传递函数名字的参数名称
				jsonp: 'cb',
				jsonpCallback: 'fn',
				// 代表现在要发送的是jsonp请求
				dataType: 'jsonp'/*,
				success: function (response) {
					console.log(response)
				}*/
			})
		});
	</script>
</body>
</html>
```
### 3.6  $ .get()、$ .post()方法概述
```markdown
1.发送GET方式的异步请求 
    $.get(url,data,function(result){},"JSON");
2.发送POST方式的异步请求
    $.post(url,data,function(result){},"JSON");
    
$.get('http://www.example.com', {name: 'zhangsan', age: 30}, function (response) {}) 
$.post('http://www.example.com', {name: 'lisi', age: 22}, function (response) {})
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>$.ajax方法基本使用</title>
</head>
<body>
	<button id="btn">发送请求</button>
	<script src="/js/jquery.min.js"></script>
	<script>
		$('#btn').on('click', function () {
			// $.get('/base', 'name=zhangsan&age=30', function (response) {
			// 	console.log(response)
			// })
			
			$.post('/base', function (response) {
				console.log(response)
			})
		});
	</script>
</body>
</html>
```
### 3.7 Todo案例
```markdown
使用mongo命令进入mongodb数据库
使用use admin命令进入到admin数据中
使用db.auth(‘root’, ‘root’)命令登录数据库
使用use todo命令切换到todo数据库
使用db.createUser({user: ‘itcast’, pwd: ‘itcast’, roles: [‘readWrite’]})
创建todo数据库账号
使用exit命令退出mongodo数据库

展示任务列表
准备一个放置任务列表的数组
向服务器端发送请求，获取已存在的任务
将已存在的任务存储在任务列表数组中
通过模板引擎将任务列表数组中的任务显示在页面中

添加任务
为文本框绑定键盘抬起事件，在事件处理函数中判断当前用户敲
击的是否是回车键
当用户敲击回车键的时候，判断用户在文本框中是否输入了任务名称
向服务器端发送请求，将用户输入的任务名称添加到数据库中，
同时将任务添加到任务数组中
通过模板引擎将任务列表数组中的任务显示在页面中

删除任务
为删除按钮添加点击事件
在事件处理函数中获取到要删任务的id
向服务器端发送请求，根据ID删除任务，同时将任务数组中的
相同任务删除
通过模板引擎将任务列表数组中的任务重新显示在页面中



更改任务状态
为任务复选框添加onchange事件
在事件处理函数中获取复选框是否选中
向服务器端发送请求，将当前复选框的是否选中状态提交到服务器端
将任务状态同时也更新到任务列表数组中
通过模板引擎将任务列表数组中的任务重新显示在页面中并且根
据任务是否完成为li元素添加completed类名


修改任务名称
为任务名称外层的label标签添加双击事件，同时为当前任务
外层的li标签添加editing类名，开启编辑状态
将任务名称显示在文本框中并让文本框获取焦点
当文本框离开焦点时，将用户在文本框中输入值提交到服务
器端，并且将最新的任务名称更新到任务列表数组中
使用模板引擎重新渲染页面中的任务列表。


计算未完成任务数量
准备一个用于存储未完成任务数量的变量
将未完成任务从任务数组中过滤出来
将过滤结果数组的长度赋值给任务数量变量
将结果更新到页面中


显示未完成任务
为active按钮添加点击事件
从任务列表数组中将未完成任务过滤出来
使用模板引擎将过滤结果显示在页面中


清除已完成任务
为clear completed按钮添加点击事件
向服务器端发送请求将数据库中的已完成任务删除掉
将任务列表中的已完成任务删除调用
使用模板引擎将任务列表中的最后结果显示在页面中

全局事件
.ajaxStart()     // 当请求开始发送时触发
.ajaxComplete()  // 当请求完成时触

NProgress
官宣：纳米级进度条，使用逼真的涓流动画来告诉用户正在发生的事情！
<link rel='stylesheet' href='nprogress.css'/>
<script src='nprogress.js'></script>
NProgress.start();  // 进度条开始运动 
NProgress.done();   // 进度条结束运动
```
```markdown
app.js
```
```js
// 引入express框架
const express = require('express');
// 路径处理模块
const path = require('path');
// 导入mongoose模块
const mongoose = require('mongoose');
// 导入bodyParser模块
const bodyParser = require('body-parser');
// 创建web服务器
const app = express();
// 静态资源访问服务功能
app.use(express.static(path.join(__dirname, 'public')));
// 处理post请求参数
app.use(bodyParser.json()); 
// app.use(bodyParser.urlencoded({extended: false}));

// 数据库连接
mongoose.connect('mongodb://itcast:itcast@localhost:27017/todo', {useNewUrlParser: true })

app.get('/base', (req, res) => {
	res.send({
		name: 'zhangsan',
		age: 30
	})
});

app.post('/base', (req, res) => {
	res.status(400).send({
		name: 'zhaoliu',
		age: 35
	})
});

app.get('/user', (req, res) => {
	res.send(req.query);
});

app.post('/user', (req, res) => {
	res.send(req.body)
});

app.get('/jsonp', (req, res) => {
	const cb = req.query.cb
	const data = cb+"({name: 'zhaoliu'})"
	res.send(data);
	// res.jsonp({
	// 	name: 'lisi',
	// 	age:50
	// })
});

// 导入todo路由案例
const todoRouter = require('./route/todo')
// 当客户端的请求路径以/todo开头时
app.use('/todo', todoRouter);

// 获取用户列表信息
app.get('/users', (req, res) => {
	res.send('当前是获取用户列表信息的路由');
});

// 获取某一个用户具体信息的路由
app.get('/users/:id', (req, res) => {
	// 获取客户端传递过来的用户id
	const id = req.params.id;
	res.send(`当前我们是在获取id为${id}用户信息`);
});

// 删除某一个用户
app.delete('/users/:id', (req, res) => {
	// 获取客户端传递过来的用户id
	const id = req.params.id;
	res.send(`当前我们是在删除id为${id}用户信息`);
});

// 修改某一个用户的信息
app.put('/users/:id', (req, res) => {
	// 获取客户端传递过来的用户id
	const id = req.params.id;
	res.send(`当前我们是在修改id为${id}用户信息`);
});

app.get('/xml', (req, res) => {
	res.header('content-type', 'text/xml');
	res.send('<message><title>消息标题</title><content>消息内容</content></message>')
});


// 监听端口
app.listen(3000);
// 控制台提示输出
console.log('服务器启动成功');
```
```markdown
task.js
```
```js
// 引入mongoose模块
const mongoose = require('mongoose');
// 创建任务集合规则
const taskSchema = new mongoose.Schema({
	title: {
		type: String,
		required: true
	},
	completed: {
		type: Boolean,
		default: false
	}
}, {versionKey: false});
// 创建todo集合
const Task = mongoose.model('task', taskSchema);
// 将集合构造函数作为模块成员进行导出
module.exports = Task;
```
```markdown
todo.js
```
```js
// 引入express框架
const express = require('express');
// 工具库
const _ = require('lodash');
// 对象校验
const Joi = require('joi');
// 创建todo案例路由
const todoRouter = express.Router();
// 导入todo集合构造函数
const Task = require('../model/task');

// 获取任务列表
todoRouter.get('/task', async (req, res) => {
	const task = await Task.find();
	// 响应
	res.send(task);
});

// 添加任务
todoRouter.post('/addTask', async (req, res) => {
	// 接收客户端传递过来的任务名称
	const { title } = req.body;
	// 验证规则
	const schema = {
		title: Joi.string().required().min(2).max(30)
	};
	// 验证客户端传递过来的请求参数 
	const { error } = Joi.validate(req.body, schema);
	// 验证失败
	if (error) {
		// 将错误信息响应给客户端
		return res.status(400).send({message: error.details[0].message})
	}
	// 创建任务实例
	const task = new Task({title: title, completed: false});
	// 执行插入操作
	await task.save();
	// 响应
	setTimeout(() => {
		res.send(task);
	}, 2000)
});

// 删除任务
todoRouter.get('/deleteTask', async (req, res) => {
	// 要删除的任务id
	const { _id } = req.query;
	// 验证规则
	const schema = {
		_id: Joi.string().required().regex(/^[0-9a-fA-F]{24}$/)
	}
	// 验证客户端传递过来的请求参数 
	const { error } = Joi.validate(req.query, schema);
	// 验证失败
	if (error) {
		// 将错误信息响应给客户端
		return res.status(400).send({message: error.details[0].message})
	}
	// 删除任务
	const task = await Task.findOneAndDelete({_id: _id});
	// 响应
	res.send(task);
});

// 清除已完成任务
todoRouter.get('/clearTask', async (req, res) => {
	// 执行清空操作
	const result = await Task.deleteMany({completed: true});
	// 返回清空数据
	res.send(result);
});

// 修改任务
todoRouter.post('/modifyTask', async (req, res) => {
	// 执行修改操作
	const task = await Task.findOneAndUpdate({_id: req.body._id}, _.pick(req.body, ['title', 'completed']),{new: true})
	// 响应
	res.send(task);
});

// 查询未完成任务数量
todoRouter.get('/unCompletedTaskCount', async (req, res) => {
	// 执行查询操作
	const result = await Task.countDocuments({completed: false});
	// 响应
	res.send({num: result})
});

// 更改任务全部状态
todoRouter.get('/changeAllTasksComplete', async (req, res) => {
	// 状态
	const { status } = req.query;
	// 执行更改状态操作
	const result = await Task.updateMany({}, {completed: status});
	// 响应
	res.send(result);
});

// 将todo案例路由作为模块成员进行导出
module.exports = todoRouter;
```
```markdown
index.html
```
```html
<!doctype html>
<html lang="en">
	<head>
		<meta charset="utf-8">
		<meta name="viewport" content="width=device-width, initial-scale=1">
		<title>Todo List</title>
		<link rel="stylesheet" href="assets/css/base.css">
		<link rel="stylesheet" href="assets/css/index.css">
		<link rel="stylesheet" href="/js/nprogress/nprogress.css">
	</head>
	<body>
		<section class="todoapp">
			<header class="header">
				<h1>todos</h1>
				<input type="text" class="new-todo" placeholder="What needs to be done?" autofocus id="task">
			</header>
			<!-- This section should be hidden by default and shown when there are todos -->
			<section class="main">
				<input class="toggle-all" type="checkbox">
				<ul class="todo-list" id="todo-list"></ul>
			</section>
			<!-- This footer should hidden by default and shown when there are todos -->
			<footer class="footer">
				<!-- This should be `0 items left` by default -->
				<span class="todo-count"><strong id="count">0</strong> item left</span>
				<!-- Remove this if you don't implement routing -->
				<ul class="filters">
					<li>
						<a class="selected" href="javascript:;">All</a>
					</li>
					<li>
						<a href="javascript:;">Active</a>
					</li>
					<li>
						<a href="javascript:;">Completed</a>
					</li>
				</ul>
				<!-- Hidden if no completed items are left ↓ -->
				<button class="clear-completed">Clear completed</button>
			</footer>
		</section>
		<script src="/js/jquery.min.js"></script>
		<script src="/js/template-web.js"></script>
		<script src="/js/nprogress/nprogress.js"></script>
		<!-- 任务列表模板 -->
		<script type="text/html" id="taskTpl">
			{{each tasks}}
			<li class="{{$value.completed ? 'completed' : ''}}">
				<div class="view">
					<input class="toggle" type="checkbox" {{$value.completed ? 'checked' : ''}}>
					<label>{{$value.title}}</label>
					<button class="destroy" data-id="{{$value._id}}"></button>
				</div>
				<input class="edit">
			</li>
			{{/each}}
		</script>
		<script type="text/javascript">
			// 用于存放任务列表的数组
			var taskAry = [];
			// 选择任务列表容器
			var taskBox = $('#todo-list');
			// 添加任务的文本框
			var taskInp = $('#task');
			// 用于存储未完成任务数量的strong标签
			var strong = $('#count');

			// 当页面中有ajax请求发送时触发
			$(document).on('ajaxStart', function () {
				 NProgress.start() 
			})

			// 当页面中有ajax请求完成时触发
			$(document).on('ajaxComplete', function () {
				NProgress.done() 
			})

			// 向服务器端发送请求 获取已经存在的任务
			$.ajax({
				url: '/todo/task',
				type: 'get',
				success: function (response) {
					// 将已存在的任务存储在taskAry变量中
					taskAry = response;
					// 拼接字符串 将拼接好的字符串显示在页面中
					render();
					// 计算未完成任务数量
					calcCount ()
				}
			})

			// 获取文本框并且添加键盘抬起事件
			taskInp.on('keyup', function (event) {
				// 如果用户敲击的是回车键
				if (event.keyCode == 13) {
					// 判断用户是否在文本框中输入了任务名称
					var taskName = $(this).val();
					// 如果用户没有在文本框中输入内容
					if (taskName.trim().length == 0) {
						alert('请输入任务名称')
						// 阻止代码向下执行
						return;
					}
					// 向服务器端发送请求 添加任务
					$.ajax({
						type: 'post',
						url: '/todo/addTask',
						contentType: 'application/json',
						data: JSON.stringify({title: taskName}),
						success: function (response) {
							// 将任务添加到任务列表中
							taskAry.push(response);
							// 拼接字符串 将拼接好的字符串显示在页面中
							render();
							// 清空文本框中的内容
							taskInp.val('');
							// 计算未完成任务数量
							calcCount ()
						}
					})
				}
			});

			// 拼接字符串 将拼接好的字符串显示在页面中
			function render() {
				// 字符串拼接
				var html = template('taskTpl', {
					tasks: taskAry
				});
				// 将拼接好的字符串显示在ul标签中
				taskBox.html(html);
			}

			// 当用户点击删除按钮时触发ul标签身上的点击事件
			taskBox.on('click', '.destroy', function () {
				// 要删除的任务的id
				var id = $(this).attr('data-id');
				// 向服务器端发送请求删除 任务
				$.ajax({
					url: '/todo/deleteTask',
					type: 'get',
					data: {
						_id: id
					},
					success: function (response) {
						// 从任务数组中找到已经删除掉的任务的索引
						var index = taskAry.findIndex(item => item._id == id);
						// 将任务从数组中删除
						taskAry.splice(index, 1);
						// 重新将任务数组中的元素显示在页面中
						render();
						// 计算未完成任务数量
						calcCount ()
					}
				})
			});

			// 当用户改变任务名称前面的复选框状态时触发
			taskBox.on('change', '.toggle', function () {
				// 代表复选框是否选中 true 选中 false 未选中的
				const status = $(this).is(':checked');
				// 当前点击任务的id
				const id = $(this).siblings('button').attr('data-id');
				// 向服务器端发送请求 更改任务状态
				$.ajax({
					type: 'post',
					url: '/todo/modifyTask',
					data: JSON.stringify({_id: id, completed: status}),
					contentType: 'application/json',
					success: function (response) {
						// 将任务状态同步到任务数组中
						var task = taskAry.find(item => item._id == id);
						// 更改任务状态
						task.completed = response.completed;
						// 将数组中任务的最新状态更新到页面中
						render();
						// 计算未完成任务数量
						calcCount ()
					}
				})
			});

			// 当双击事件名称的时候触发
			taskBox.on('dblclick', 'label', function () {
				// 让任务处于编辑状态
				$(this).parent().parent().addClass('editing');
				// 将任务名称显示在文本框中
				$(this).parent().siblings('input').val($(this).text())
				// 让文本框获取焦点
				$(this).parent().siblings('input').focus();
			})

			// 当文本框离开焦点的时候
			taskBox.on('blur', '.edit', function () {
				// 最新的任务名称
				var newTaskName = $(this).val();
				// 编辑任务的id
				var id = $(this).siblings().find('button').attr('data-id');
				// 向服务器端发送请求 修改任务名称
				$.ajax({
					url: '/todo/modifyTask',
					type: 'post',
					data: JSON.stringify({_id: id, title: newTaskName}),
					contentType: 'application/json',
					success: function (response) {
						// 将当期任务的最新状态同步到任务数组中
						var task = taskAry.find(item => item._id == id);
						// 修改任务名称
						task.title = response.title;
						// 将任务数组中的任务同步到页面中
						render();
						// 计算未完成任务数量
						calcCount ()
					}
				})
			});

			// 用于计算未完成任务的数量
			function calcCount () {
				// 存储结果的变量
				var count = 0;
				// 将未完成的任务过滤到一个新的数组中
				var newAry = taskAry.filter(item => item.completed == false);
				// 将新数组的长度赋值给count
				count = newAry.length;
				// 将未完成的任务数量显示在页面中
				strong.text(count)
			}

		</script>
	</body>
</html>

```
##  4 RESTful 和 XML
### 4.1 RESTful 风格的 API
```markdown
传统请求地址回顾
GET http://www.example.com/getUsers         // 获取用户列表
GET http://www.example.com/getUser?id=1     // 比如获取某一个用户的信息
POST http://www.example.com/modifyUser      // 修改用户信息
GET http://www.example.com/deleteUser?id=1  // 删除用户信息

GET：      获取数据
POST：    添加数据
PUT：      更新数据
DELETE： 删除数据

GET：        http://www.example.com/users    获取用户列表数据
POST：      http://www.example.com/users   创建(添加)用户数据
GET：        http://www.example.com/users/1   获取用户ID为1的用户信息
PUT：       http://www.example.com/users/1   修改用户ID为1的用户信息
DELETE：http://www.example.com/users/1   删除用户ID为1的用户信息
```

```html

<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script src="/js/jquery.min.js"></script>
	<script type="text/javascript">
		// 获取用户列表信息
		$.ajax({
			type: 'get',
			url: '/users',
			success: function (response) {
				console.log(response)
			}
		})

		// 获取id为1的用户信息
		$.ajax({
			type: 'get',
			url: '/users/1',
			success: function (response) {
				console.log(response)
			}
		})

		// 获取id为1的用户信息
		$.ajax({
			type: 'delete',
			url: '/users/10',
			success: function (response) {
				console.log(response)
			}
		})

		// 获取id为1的用户信息
		$.ajax({
			type: 'put',
			url: '/users/10',
			success: function (response) {
				console.log(response)
			}
		})
	</script>
</body>
</html>
```
### 4.2 XML
```markdown
XML 的全称是 extensible markup language，代表可扩展标记语言，它的作用是传输和存储数据。
<students> 
     <student>
         <sid>001</sid>
         <name>张三</name>
         </student>
     <student>
         <sid>002</sid>
         <name>王二丫</name>
         </student>
 </students>

```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<button id="btn">发送请求</button>
	<div id="container"></div>
	<script type="text/javascript">
		var btn = document.getElementById('btn');
		var container = document.getElementById('container');

		btn.onclick = function () {
			var xhr = new XMLHttpRequest();
			xhr.open('get', '/xml');
			xhr.send();
			xhr.onload = function () {
				// xhr.responseXML 获取服务器端返回的xml数据
				var xmlDocument = xhr.responseXML;
				var title = xmlDocument.getElementsByTagName('title')[0].innerHTML;
				container.innerHTML = title;
			}
		}
	</script>
</body>
</html>

```
## 5 获取服务器端响应数据的另一种方式
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200521085435479.png)
```markdown
Ajax 状态码
在创建ajax对象，配置ajax对象，发送请求，以及接收完服务器端响应数据，这个过程中的每一个步骤都会对应一个数值，这个数值就是ajax状态码。
0：请求未初始化(还没有调用open())
1：请求已经建立，但是还没有发送(还没有调用send())
2：请求已经发送
3：请求正在处理中，通常响应中已经有部分数据可以用了
4：响应已经完成，可以获取并使用服务器的响应了
xhr.readyState // 获取Ajax状态码

onreadystatechange 事件
当 Ajax 状态码发生变化时将自动触发该事件。
在事件处理函数中可以获取 Ajax 状态码并对其进行判断，当状态码为 4 时就可以通过 xhr.responseText 获取服务器端的响应数据了。


不推荐,因为这种方式会由于状态码的变化被触发多次,效率低.推荐使用onload这种方式.唯一不足就是不兼容低版本IE浏览器
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script type="text/javascript">
		var xhr = new XMLHttpRequest();
		// 0 已经创建了ajax对象 但是还没有对ajax对象进行配置
		console.log(xhr.readyState);
		xhr.open('get', 'http://localhost:3000/readystate');
		// 1 已经对ajax对象进行配置 但是还没有发送请求
		console.log(xhr.readyState);

		// 当ajax状态码发生变化的时候出发
		xhr.onreadystatechange = function() {
			// 2 请求已经发送了
			// 3 已经接收到服务器端的部分数据了
			// 4 服务器端的响应数据已经接收完成
			console.log(xhr.readyState);
			// 对ajax状态码进行判断 如果状态码的值为4就代表数据已经接收完成了
			if (xhr.readyState == 4) {
				console.log(xhr.responseText);
			}
		} 

		xhr.send();
		
	</script>
</body>
</html>
```

### 5.1 Ajax的编程思路
```javascript
1. 创建xhr对象
var xhr ;
	if(window.XMLHttpRequest){ 
        xhr = new XMLHttpRequest();  # Webkit内核
    }else{
        xhr =  new ActiveXObject("Microsoft.XMLHTTP");
    }

2. 发送请求并传递参数
	xhr.open("GET|POST","url?参数"); xhr.send(null);

3. 处理响应并渲染页面
	xhr.onreadystatechange = function(){ 
    if(xhr.readyState == 4 && xhr.status == 200){
            console.log(xhr.resonseText);
        }
	} 
```



### 5.2 发送GET方式请求

```javascript
//1. 创建xhr对象
var xhr ; 
if(window.XMLHttpRequest){
	xhr = new XMLHttpRequest(); 
}else{
	xhr = new ActiveXObject("Microsoft.XMLHTTP"); 
}
//2.发送请求,并传递参数
xhr.open("GET", "/ajax_day2/test?name=zhangsan");
xhr.send();
//3.处理响应
xhr.onreadystatechange = function(){ 
  if(xhr.readyState==4 && xhr.status==200){
     console.log(xhr.responseText);
 	}
}
```

### 5.3 发送POST方式请求
```javascript
//1. 创建xhr对象
var xhr; if(window.XMLHttpRequest){
xhr = new XMLHttpRequest(); }else{
xhr = new ActiveXObject("Microsoft.XMLHTTP"); }
//2.发送请求,并传递参数
xhr.open("POST","/ajax_day2/test");
xhr.setRequestHeader("content-type", "application/x-www-form-urlencoded"); xhr.send("name=zhangsan");
//3.处理响应
xhr.onreadystatechange = function(){
if(xhr.readyState==4 && xhr.status==200){ console.log(xhr.reponseText);
} }
```



### 5.4 Ajax的数据交换机制
```markdown
JSON (JavaScript Object Notation, JS对象标记) 是一种
轻量级的数据交换格式。
```
```java
1. 如果服务端响应的不再是字符串而是对象或者是集合类型时,无法直接将对象响应给客户端。 
		如: User、List<User>、Map<String,User> 需要将对象转为json格式字符串响应给ajax。
		
		
2.如何将对象转为json格式的字符串
  User user = new User("21","chenyn",23,123.0);
  String userJson = JSONObject.toJSONString(user); // fastjson json
	response.setContentType("application/json;charset=UTf-8");
  PrintWriter writer = response.getWriter();
  writer.out(userJson);

3.前端的ajax函数中应该如何处理json格式的字符串
	xhr.onreadystatechange = function(){ 
		if(xhr.readyState==4 && xhr.status==200){
			var json = xhr.responseText;// json
			var userObj = eval("("+xhr.responseText+")"); //转为js对象" 
			console.log(userObj.id);//
			console.log(userObj.name);
		} 
}

4.处理json字符串中的日期格式问题
    var userJson = JSONObject.toJsonStringWithDateFormat(user,"yyyy-MM-dd");
```

### 5.5 图片总结
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210101100900842.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210101100703432.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210101100959642.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210101101214455.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210101101232644.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210101101247963.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210101101330577.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021010110141659.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210101101446538.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210101101515178.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210101101543186.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复
ajax即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
