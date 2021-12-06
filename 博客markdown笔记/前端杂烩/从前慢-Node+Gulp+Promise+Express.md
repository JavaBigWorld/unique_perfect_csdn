# Node+Gulp+Promise+Express
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714012130624.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

### 1.1 Node的介绍及基础语法
```markdown
Node是一个基于Chrome V8引擎的JavaScript代码运行环境。
浏览器（软件）能够运行JavaScript代码，浏览器就是
JavaScript代码的运行环境
Node（软件）能够运行JavaScript代码，Node就是
JavaScript代码的运行环境

node之所以能够运行JavaScript代码，是因为内部有V8引擎
node -v（查看当前node的版本）
JavaScript 由三部分组成，ECMAScript，DOM，BOM。
Node. js是由 ECMAScript及Node环境提供的一些附加API组
成的，包括文件、网络、路径等等一些更加强大的API

执行文件
node 文件名.js
```
#### 1.1.1 Node全局对象global
```markdown
在浏览器中全局对象是window，在Node中全局对象是global。
Node中全局对象下有以下方法，可以在任何地方使用，
global可以省略。
console.log()     在控制台中输出
setTimeout()     设置超时定时器
clearTimeout()  清除超时时定时器
setInterval()      设置间歇定时器
clearInterval()   清除间歇定时器
```
```js
/*global.console.log('我是global对象下面的console.log方法输出的内容');

global.setTimeout(function (){
	console.log('123');
}, 2000)*/

console.log('我是global对象下面的console.log方法输出的内容');

setTimeout(function (){
	console.log('123');
}, 2000)
```
### 1.2 Node中模块化
```markdown
Node.js规定一个JavaScript文件就是一个模块，模块内部
定义的变量和函数默认情况下在外部无法得到
模块内部可以使用exports对象进行成员导出， 使用
require方法导入其他模块。
```
#### 1.2.1 模块成员导出
```javascript
// a.js
  // 在模块内部定义变量
 let version = 1.0;
 // 在模块内部定义方法
 const sayHi = name => `您好, ${name}`;
 // 向模块外部导出数据 
 exports.version = version;
 exports.sayHi = sayHi;

```
#### 1.2.2  模块成员的导入
```javascript
  // b.js
  // 在b.js模块中导入模块a
 let a = require('./b.js');
  // 输出b模块中的version变量
 console.log(a.version);
  // 调用b模块中的sayHi方法 并输出其返回值
 console.log(a.sayHi('黑马讲师')); 

导入模块时后缀可以省略
```
```markdown
案例导入导出
```
```markdown
module-a.js
```

```javascript
const add = (n1, n2) => n1 + n2;

exports.add = add;
```
```markdown
module-b.js
```

```javascript
//const a = require('./03.module-a.js');
const a = require('./03.module-a');
console.log(a.add(10, 20));
```

#### 1.2.3 模块成员导出的另一种方式
```markdown
module.exports.version = version;
module.exports.sayHi = sayHi;

exports是module.exports的别名(地址引用关系)，导出
对象最终以module.exports为准
```

#### 1.2.4 模块导出两种方式的联系与区别

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200518101414714.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
module.exports
```
```javascript
const greeting = name => `hello ${name}`;
const x = 100;
exports.x = x;
module.exports.greeting = greeting;
// 当exports对象和moudle.exports对象指向的不是同一个对象时 以module.exports为准
module.exports = {
	name: 'zhangsan'
}

exports = {
	age: 20
}
```

#### 1.2.5 系统模块
```markdown
Node运行环境提供的API. 因为这些API都是以模块化
的方式进行开发的, 所以我们又称Node运行环境提供的API为系统模块
```

##### 1.2.5.1 系统模块fs（文件操作系统）文件操作
```markdown
const fs = require('fs');
fs.reaFile('文件路径/文件名称'[,'文件编码'], callback); // 读取文件内容
```
###### 1.2.5.1.1 读取内容
```javascript
// 1.通过模块的名字fs对模块进行引用
const fs = require('fs');

// 2.通过模块内部的readFile读取文件内容
fs.readFile('./01.helloworld.js', 'utf8', (err, doc) => {
	// 如果文件读取出错err 是一个对象 包含错误信息
	// 如果文件读取正确 err是 null
	// doc 是文件读取的结果
	console.log(err);
	console.log(doc);
});
```
###### 1.2.5.1.2  写入文件内容
```markdown
const fs = require('fs');
fs.writeFile('文件路径/文件名称', '数据', callback);
```
```javascript
const fs = require('fs');

fs.writeFile('./demo.txt', '即将要写入的内容', err => {
	if (err != null) {
		console.log(err);
		return;
	}

	console.log('文件内容写入成功');
})
```
###### 1.2.5.1.3  路径拼接语法
```markdown
为什么要进行路径拼接 
不同操作系统的路径分隔符不统一
/public/uploads/avatar
Windows 上是 \   /
Linux 上是 /
```
```js
const path = require('path');
 // 路径拼接
let finialPath = path.join('itcast', 'a', 'b', 'c.css');
 // 输出结果 itcast\a\b\c.css
console.log(finialPath);
```
###### 1.2.5.1.4   相对路径VS绝对路径 
```markdown
大多数情况下使用绝对路径，因为相对路径有时候相
对的是命令行工具的当前工作目录
在读取文件或者设置文件路径时都会选择绝对路径
使用__dirname获取当前文件所在的绝对路径
```
```javascript
const fs = require('fs');
const path = require('path');

console.log(__dirname);
console.log(path.join(__dirname, '01.helloworld.js'))

fs.readFile(path.join(__dirname, '01.helloworld.js'), 'utf8', (err, doc) => {
	console.log(err)
	console.log(doc)
});
```
###### 1.2.5.1.5  第三方模块
```markdown
别人写好的、具有特定功能的、我们能直接使用的模块即第三方模块，
由于第三方模块通常都是由多个文件组成并且被放置在一个文件夹中，
所以又名包。

第三方模块有两种存在形式：
以js文件的形式存在，提供实现项目具体功能的API接口。
以命令行工具形式存在，辅助项目开发

获取第三方模块
npmjs.com：第三方模块的存储和分发仓库
```
#### 1.2.6  npm
```markdown
npm (node package manager) ： node的第三方模块管理工具
下载：npm install 模块名称 // 默认在下载的目录下生成node_modules。
卸载：npm unintall package 模块名称

npm默认全局安装路径
C:\Users\Administrator\AppData\Roaming\npm
```
```markdown
npm install --save 、--save-dev 、-D、-S 的区别
```
```markdown
1、npm install <=> npm i

   --save   <=> -S     

   --save-dev  <=> -D 

npm run start <=> npm start  // 对应"scripts"里的"start"命令

2、npm i --save-dev  <packname>  

工程构建（开发时、“打包”时）依赖 ；例：xxx-cli , less-loader , babel-loader...

3、npm i --save <packname> 

项目（运行时、发布到生产环境时）依赖；例：antd , element,react...

4、对应关系如下
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200523074406676.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
5、使用 npm i 安装package.json里的依赖时，两部分的
包都会pull下来
```
```markdown
使用 --prod

npm i --prod <=> npm i --production  // 仅会拉取dependencies中的依赖
```
```markdown
全局安装与本地安装
命令行工具：全局安装
库文件：本地安装
```
##### 1.2.6.1 npm 下载慢解决办法
```markdown
有两个办法：

1、安装cnpm

命令：npm install cnpm -g --registry=https://registry.npm.taobao.org

以后就用 cnpm 代替 npm 做操作。从地址看出这是淘宝的国内镜像，比较快。据说每10分钟更新一次仓库，同步性也好


2、更改npm的绑定地址
命令：npm config set registry https://registry.npm.taobao.org
这样以后还是用npm，但已切换到淘宝镜像
更改后可通过下面命令来验证是否成功

npm config get registry

当然，想换回来也行，npm的原始仓库地址是 https://registry.npmjs.org/
```
#### 1.2.7 nodemon
```markdown
nodemon是一个命令行工具，用以辅助项目开发。
在Node.js中，每次修改文件都要在命令行工具中重新
执行该文件，非常繁琐。
使用npm install -g  nodemon下载它
在命令行工具中用nodemon命令替代node命令执行文件
```
#### 1.2.8  nrm
```markdown
nrm ( npm registry manager )：npm下载地址切换工具
npm默认的下载地址在国外，国内下载速度慢
使用步骤
使用npm install nrm –g 下载它
查询可用下载地址列表 nrm ls
切换npm下载地址 nrm use 下载地址名称
```
#### 1.2.9 Gulp
```markdown
npm install gulp
基于node平台开发的前端构建工具
将机械化操作编写成任务, 想要执行机械化操作时执行一个命
令行命令任务就能自动执行了.
用机器代替手工，提高开发效率。
```
```markdown
Gulp能做什么
```
```markdown
项目上线，HTML、CSS、JS文件压缩合并
语法转换（es6、less ...）
公共文件抽离
修改文件浏览器自动刷新
```
```markdown
Gulp的使用
```
```markdown
在项目根目录下使用npm install gulp下载gulp库文件
在项目根目录下建立gulpfile.js文件
重构项目的文件夹结构 src目录放置源代码文件 dist目录
放置构建后文件
在gulpfile.js文件中编写任务.
在命令行工具中执行gulp任务（因为node命令是执行某个文
件，所以我们要
使用npm install gulp-cli -g来使用gulp来执行某个文件的具
体任务。安装好后
使用gulp 任务名字就可以了，gulp会去gulpfile.js寻找指
定的任务。
```
```markdown
Gulp中提供的方法
```
```markdown
gulp.src()：获取任务要处理的文件
gulp.dest()：输出文件
gulp.task()：建立gulp任务
gulp.watch()：监控文件的变化
```
```markdown
Gulp插件
```
```markdown
gulp-htmlmin ：html文件压缩
gulp-csso ：压缩css
gulp-babel ：JavaScriptES6语法转化
gulp-less: less语法转化为CSS
gulp-uglify ：压缩混淆JavaScript
gulp-file-include 公共文件包含
browsersync 浏览器实时同步
```
```markdown
使用例子
```
```js
// 引用gulp模块
const gulp = require('gulp');
const htmlmin = require('gulp-htmlmin');
const fileinclude = require('gulp-file-include');
const less = require('gulp-less');
const csso = require('gulp-csso');
const babel = require('gulp-babel');
const uglify = require('gulp-uglify');
// 使用gulp.task建立任务
// 1.任务的名称
// 2.任务的回调函数
gulp.task('first', () => {
	console.log('我们人生中的第一个gulp任务执行了');
	// 1.使用gulp.src获取要处理的文件
	gulp.src('./src/css/base.css')
		.pipe(gulp.dest('dist/css'));
});

// html任务
// 1.html文件中代码的压缩操作
// 2.抽取html文件中的公共代码
gulp.task('htmlmin', () => {
	gulp.src('./src/*.html')
		.pipe(fileinclude())
		// 压缩html文件中的代码
		.pipe(htmlmin({ collapseWhitespace: true }))
		.pipe(gulp.dest('dist'));
});

// css任务
// 1.less语法转换
// 2.css代码压缩
gulp.task('cssmin', () => {
	// 选择css目录下的所有less文件以及css文件
	gulp.src(['./src/css/*.less', './src/css/*.css'])
		// 将less语法转换为css语法
		.pipe(less())
		// 将css代码进行压缩
		.pipe(csso())
		// 将处理的结果进行输出
		.pipe(gulp.dest('dist/css'))
});

// js任务
// 1.es6代码转换
// 2.代码压缩
gulp.task('jsmin', () => {
	gulp.src('./src/js/*.js')
		.pipe(babel({
			// 它可以判断当前代码的运行环境 将代码转换为当前运行环境所支持的代码
            presets: ['@babel/env']
        }))
        .pipe(uglify())
        .pipe(gulp.dest('dist/js'))
});

// 复制文件夹
gulp.task('copy', () => {

	gulp.src('./src/images/*')
		.pipe(gulp.dest('dist/images'));

	gulp.src('./src/lib/*')
		.pipe(gulp.dest('dist/lib'))
});

// 构建任务
gulp.task('default', ['htmlmin', 'cssmin', 'jsmin', 'copy']);
```
#### 1.2.10 node_modules文件夹的问题
```markdown
文件夹以及文件过多过碎，当我们将项目整体拷贝给别人的时
候,，传输速度会很慢很慢. 
复杂的模块依赖关系需要被记录，确保模块的版本和当前保持
一致，否则会导致当前项目运行报错
```
```markdown
package.json文件的作用
```
```markdown
项目描述文件，记录了当前项目信息，例如项目名称、版本、作
者、github地址、当前项目依赖了哪些第三方模块等。
使用npm init -y(这个选项就是yes的意思,没有这个选项,就要手
动设置)命令生成。
```
```markdown
当我们下载别人的文件时,一般没有 node_modules这个文件
夹,项目就运行不起来,所以,我们需要使用npm install命令,
这个命令会自动找到package.json中,然后下载相应的依赖
到node_modules.
```
```markdown
项目依赖
```
```markdown
在项目的开发阶段和线上运营阶段，都需要依赖的第三方
包，称为项目依赖
使用npm install 包名命令下载的文件会默认被添加
到 package.json 文件的 dependencies 字段中
```
```markdown
开发依赖
```
```markdown
在项目的开发阶段需要依赖，线上运营阶段不需要依赖的
第三方包，称为开发依赖
使用npm install 包名 --save-dev命令将包添加
到package.json文件的devDependencies字段中
```
```markdown
package-lock.json文件的作用
```
```markdown
锁定包的版本，确保再次下载时不会因为包版本不
同而产生问题
加快下载速度，因为该文件中已经记录了项目所依
赖第三方包的树状结构和包的下载地址，重新安装时
只需下载即可，不需要做额外的工作
```
```markdown
Node.js中模块加载机制
require('./find.js');
标准式,找到就有,没有就报错.

require('./find');
先找当前目录下有没有find.js,没有就找find文件夹中
的index.js,没有就会去find文件夹找package.json,
查找main选项中的入口文件,如果找指定的入口文
件不存在或者没有指定入口文件就会报错，模块没有被找到

require('find');
Node.js会假设它是系统模块,Node.js会去node_modules文件夹中
首先看是否有该名字的JS文件,再看是否有该名字的文件
夹,如果是文件夹看里面是否有index.js,
如果没有index.js查看该文件夹中的package.json中的main
选项确定模块入口文件,如果没有入口文件,
就报错.
```

#### 1.2.11 创建web服务器
```markdown
get
```
```js
// 用于创建网站服务器的模块
const http = require('http');
// 用于处理url地址
const url = require('url');
// app对象就是网站服务器对象
const app = http.createServer();
// 当客户端有请求来的时候
app.on('request', (req, res) => {
	// 获取请求方式
	// req.method
	// console.log(req.method);
	
	// 获取请求地址
	// req.url
	// console.log(req.url);
	
	// 获取请求报文信息
	// req.headers
	// console.log(req.headers['accept']);
	
	res.writeHead(200, {
		'content-type': 'text/html;charset=utf8'
	});

	console.log(req.url);
	// 1) 要解析的url地址
	// 2) 将查询参数解析成对象形式
	let { query, pathname } = url.parse(req.url, true);
	console.log(query.name)
	console.log(query.age)

	if (pathname == '/index' || pathname == '/') {
		res.end('<h2>欢迎来到首页</h2>');
	}else if (pathname == '/list') {
		res.end('welcome to listpage');
	}else {
		res.end('not found');
	}
	
	if (req.method == 'POST') {
		res.end('post')
	} else if (req.method == 'GET') {
		res.end('get')
	}

	// res.end('<h2>hello user</h2>');
});
// 监听端口
app.listen(3000);
console.log('网站服务器启动成功');
```
```markdown
post
```
```js
// 用于创建网站服务器的模块
const http = require('http');
// app对象就是网站服务器对象
const app = http.createServer();
// 处理请求参数模块,导入系统模块querystring 用于将HTTP参数转换为对象格式

const querystring = require('querystring');
// 当客户端有请求来的时候
app.on('request', (req, res) => {
	// post参数是通过事件的方式接受的
	// data 当请求参数传递的时候出发data事件
	// end 当参数传递完成的时候出发end事件
	
	let postParams = '';

	req.on('data', params => {
		postParams += params;
	});

	req.on('end', () => {
		console.log(querystring.parse(postParams));
	});

	res.end('ok');

});
// 监听端口
app.listen(3000);
console.log('网站服务器启动成功');
```
```markdown
路由
路由是指客户端请求地址与服务器端程序代码
的对应关系。简单的说，就是请求什么响应什么。
```
```js
// 1.引入系统模块http
// 2.创建网站服务器
// 3.为网站服务器对象添加请求事件
// 4.实现路由功能
// 	1.获取客户端的请求方式
// 	2.获取客户端的请求地址
const http = require('http');
const url = require('url');

const app = http.createServer();

app.on('request', (req, res) => {
	// 获取请求方式
	const method = req.method.toLowerCase();
	// 获取请求地址
	const pathname = url.parse(req.url).pathname;

	res.writeHead(200, {
		'content-type': 'text/html;charset=utf8'
	});

	if (method == 'get') {

		if (pathname == '/' || pathname == '/index') {
			res.end('欢迎来到首页')
		}else if (pathname == '/list') {
			res.end('欢迎来到列表页')
		}else {
			res.end('您访问的页面不存在')
		}

	}else if (method == 'post') {

	}

});

app.listen(3000);
console.log('服务器启动成功')
```
```markdown
静态资源
```
```markdown
服务器端不需要处理，可以直接响应给客户端
的资源就是静态资源，例如CSS、JavaScript、image文件。
http://www.itcast.cn/images/logo.png
```
```markdown
动态资源
```
```markdown
相同的请求地址不同的响应资源，这种资源就是动态资源。
http://www.itcast.cn/article?id=1
http://www.itcast.cn/article?id=2
```
```js
const http = require('http');
const url = require('url');
const path = require('path');
const fs = require('fs');
const mime = require('mime');

const app = http.createServer();

app.on('request', (req, res) => {
	// 获取用户的请求路径
	let pathname = url.parse(req.url).pathname;

	pathname = pathname == '/' ? '/default.html' : pathname;

	// 将用户的请求路径转换为实际的服务器硬盘路径
	let realPath = path.join(__dirname, 'public' + pathname);

	let type = mime.getType(realPath)

	// 读取文件
	fs.readFile(realPath, (error, result) => {
		// 如果文件读取失败
		if (error != null) {
			res.writeHead(404, {
				'content-type': 'text/html;charset=utf8'
			})
			res.end('文件读取失败');
			return;
		}

		res.writeHead(200, {
			'content-type': type
		})

		res.end(result);
	});
});

app.listen(3000);
console.log('服务器启动成功')
```
#### 1.2.12 Node.js异步编程
##### 1.2.12.1 同步API, 异步API
```markdown
同步API：只有当前API执行完成后，才能继续执行下一个API
同步API可以从返回值中拿到API执行的结果, 但是
异步API是不可以的
```

```js
console.log('before'); 
console.log('after');
异步API：当前API的执行不会阻塞后续代码的执行
console.log('before');
setTimeout(
   () => { console.log('last');
}, 2000);
console.log('after');
```
##### 1.2.12.2 回调函数
```js
function getData (callback) {
	callback('123')
}

getData(function (n) {
	console.log('callback函数被调用了')
	console.log(n)
});
```
##### 1.2.12.3 异步编程
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200520094057930.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
##### 1.2.12.4 回调地狱
 ```js
 const fs = require('fs');

fs.readFile('./1.txt', 'utf8', (err, result1) => {
	console.log(result1)
	fs.readFile('./2.txt', 'utf8', (err, result2) => {
		console.log(result2)
		fs.readFile('./3.txt', 'utf8', (err, result3) => {
			console.log(result3)
		})
	})
});

 ```
##### 1.2.12.5 Promise
```markdown
Promise出现的目的是解决Node.js异步编程中回调地狱的问题。
```
```markdown
promise1.js
```
```javascript
const fs = require('fs');

let promise = new Promise((resolve, reject) => {

fs.readFile('./100.txt', 'utf8', (err, result) => {

	if (err != null) {
		reject(err);
	}else {
		resolve(result);
	}

});

});

promise.then((result) => {
 console.log(result);
})
.catch((err)=> {
console.log(err);
})
```
```markdown
promise2
```
```javascript
 const fs = require('fs');

// fs.readFile('./1.txt', 'utf8', (err, result1) => {
// 	console.log(result1)
// 	fs.readFile('./2.txt', 'utf8', (err, result2) => {
// 		console.log(result2)
// 		fs.readFile('./3.txt', 'utf8', (err, result3) => {
// 			console.log(result3)
// 		})
// 	})
// });

function p1 () {
	return new Promise ((resolve, reject) => {
		fs.readFile('./1.txt', 'utf8', (err, result) => {
			resolve(result)
		})
	});
}

function p2 () {
	return new Promise ((resolve, reject) => {
		fs.readFile('./2.txt', 'utf8', (err, result) => {
			resolve(result)
		})
	});
}

function p3 () {
	return new Promise ((resolve, reject) => {
		fs.readFile('./3.txt', 'utf8', (err, result) => {
			resolve(result)
		})
	});
}

p1().then((r1)=> {
	console.log(r1);
	return p2();
})
.then((r2)=> {
	console.log(r2);
	return p3();
})
.then((r3) => {
	console.log(r3)
})
```
##### 1.2.12.6 异步函数
```javascript
异步函数是异步编程语法的终极解决方案，它可
以让我们将异步代码写成同步的形式，让代码不
再有回调函数嵌套，使代码变得清晰明了。
1.普通函数定义前加async关键字 普通函数变成异步函数
2. 异步函数默认返回promise对象
```
```markdown
async关键字
```
```markdown
3. 普通函数定义前加async关键字 普通函数变成异步函数
4. 异步函数默认返回promise对象
5. 在异步函数内部使用return关键字进行结果返回 结果会
被包裹的promise对象中 return关键字代替了resolve方法
6. 在异步函数内部使用throw关键字抛出程序异常
7. 调用异步函数再链式调用then方法获取异步函数执行结果
8. 调用异步函数再链式调用catch方法获取异步函数执行的错误信息
```
```js
// 1.在普通函数定义的前面加上async关键字 普通函数就变成了异步函数
// 2.异步函数默认的返回值是promise对象
// 3.在异步函数内部使用throw关键字进行错误的抛出
// 
// await关键字
// 1.它只能出现在异步函数中
// 2.await promise 它可以暂停异步函数的执行 等待promise对象返回结果后再向下执行函数

// async function fn () {
// 	throw '发生了一些错误';
// 	return 123;
// }

// // console.log(fn ())
// fn ().then(function (data) {
// 	console.log(data);
// }).catch(function (err){
// 	console.log(err);
// })

async function p1 () {
	return 'p1';
}

async function p2 () {
	return 'p2';
}

async function p3 () {
	return 'p3';
}

async function run () {
	let r1 = await p1()
	let r2 = await p2()
	let r3 = await p3()
	console.log(r1)
	console.log(r2)
	console.log(r3)
}

run();
```
```markdown
await关键字
```
```js
1. await关键字只能出现在异步函数中
2. await promise await后面只能写promise
3. 对象 写其他类型的API是不可以的
4. await关键字可是暂停异步函数向下执行 直到promise返回结果
```
```js
const fs = require('fs');
// 改造现有异步函数api 让其返回promise对象 从而支持异步函数语法
const promisify = require('util').promisify;
// 调用promisify方法改造现有异步API 让其返回promise对象
const readFile = promisify(fs.readFile);

async function run () {
	let r1 = await readFile('./1.txt', 'utf8')
	let r2 = await readFile('./2.txt', 'utf8')
	let r3 = await readFile('./3.txt', 'utf8')
	console.log(r1)
	console.log(r2)
	console.log(r3)
}

run();
```
## 2 Express
```markdown
Express是一个基于Node平台的web应用开发框架，它提
供了一系列的强大特性，帮助你创建各种Web应用。我
们可以使用 npm install express 命令进行下载。
提供了方便简洁的路由定义方式
对获取HTTP请求参数进行了简化处理
对模板引擎支持程度高，方便渲染动态HTML页面
提供了中间件机制有效控制HTTP请求
拥有大量第三方中间件对功能进行扩展
```
```markdown
原生Node.js与Express框架对比之路由
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200803101159295.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
原生Node.js与Express框架对比之获取请求参数
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200803101239964.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
```markdown
中间件
```
```markdown
中间件就是一堆方法，可以接收客户端发来的请求、可以
对请求做出响应，也可以将请求继续交给下一个中间件继续处理。
中间件主要由两部分构成，中间件方法以及请求处理函数。
中间件方法由Express提供，负责拦截请求，请求处理函数
由开发人员提供，负责处理请求。
可以针对同一个请求设置多个中间件，对同一个请求进行多次处理。
默认情况下，请求从上到下依次匹配中间件，一旦匹配
成功，终止匹配。
可以调用next方法将请求的控制权交给下一个中间件，直到遇
到结束请求的中间件。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200519133104969.png)
```markdown
框架入门
```

```js
// 引入express框架
const express = require('express');
// 创建网站服务器
const app = express();

app.get('/' , (req, res) => {
	// send()
	// 1. send方法内部会检测响应内容的类型
	// 2. send方法会自动设置http状态码
	// 3. send方法会帮我们自动设置响应的内容类型及编码
	res.send('Hello. Express');
})

app.get('/list', (req, res) => {
	res.send({name: '张三', age: 20})
})

// 监听端口
app.listen(3000);
console.log('网站服务器启动成功');
```
```markdown
中间件
```
```js
// 引入express框架
const express = require('express');
// 创建网站服务器
const app = express();

app.get('/request', (req, res, next) => {
	req.name = "张三";
	next();
})

app.get('/request', (req, res) => {
	res.send(req.name)
})

// 监听端口
app.listen(3000);
console.log('网站服务器启动成功');
```
```markdown
app.use
```
```markdown
app.use 匹配所有的请求方式
app.use 匹配所有的请求方式，可以直接传入请求处
理函数，代表接收所有的请求。
app.use 第一个参数也可以传入请求地址，代表不论
什么请求方式，只要是这个请求地址就接收这个请求。
```
```js
// 引入express框架
const express = require('express');
// 创建网站服务器
const app = express();

// 接收所有请求的中间件
app.use((req, res, next) => {
	console.log('请求走了app.use中间件');
	next()
})

// 当客户端访问/request请求的时候走当前中间件
app.use('/request', (req, res, next) => {
	console.log('请求走了app.use / request中间件')
	next()
})

app.get('/list', (req, res) => {
	res.send('/list')
})

app.get('/request', (req, res, next) => {
	req.name = "张三";
	next();
})

app.get('/request', (req, res) => {
	res.send(req.name)
})

// 监听端口
app.listen(3000);
console.log('网站服务器启动成功');
```
```markdown
中间件应用
```
```markdown
路由保护，客户端在访问需要登录的页面时，可以先
使用中间件判断用户登录状态，用户如果未登录，则
拦截请求，直接响应，禁止用户进入需要登录的页面。
网站维护公告，在所有路由的最上面定义接收所有请求
的中间件，直接为客户端做出响应，网站正在维护中。
自定义404页面
```
```markdown
中间件应用
```
```js
// 引入express框架
const express = require('express');
// 创建网站服务器
const app = express();

// 网站公告
// app.use((req, res, next) => {
// 	res.send('当前网站正在维护...')
// })


// 路由保护
app.use('/admin', (req, res, next) => {
	// 用户没有登录
	let isLogin = true;
	// 如果用户登录
	if (isLogin) {
		// 让请求继续向下执行
		next()
	}else {
		// 如果用户没有登录 直接对客户端做出响应
		res.send('您还没有登录 不能访问/admin这个页面')
	}
})

app.get('/admin', (req, res) => {
	res.send('您已经登录 可以访问当前页面')
})

//自定义404页面
app.use((req, res, next) => {
	// 为客户端响应404状态码以及提示信息
	res.status(404).send('当前访问的页面是不存在的')
})

// 监听端口
app.listen(3000);
console.log('网站服务器启动成功');
```
```markdown
错误处理中间件
```
```markdown
在程序执行的过程中，不可避免的会出现一些无法预料的
错误，比如文件读取失败，数据库连接失败。
错误处理中间件是一个集中处理错误的地方。
```
```js
// 引入express框架
const express = require('express');
const fs = require('fs');
// 创建网站服务器
const app = express();
//当没有参数next时,使用throw来激发自定义错误处理中间件,当有next时,采用下面这种方式,这个激发内置的错误处理中间件
app.get('/index', (req, res, next) => {
	// throw new Error('程序发生了未知错误') //这个跳到错误处理中间件这里,('程序发生了未知错误')
	这个内容包装到err对象中
	fs.readFile('./01.js', 'utf8', (err, result) => {
		if (err != null) {
			next(err) //触发express内部弄好的错误机制
		}else {
			res.send(result) //返回文件的内容
		}
	})

	// res.send('程序正常执行')
})

// 错误处理中间件
app.use((err, req, res, next) => {
	res.status(500).send(err.message);
})

// 监听端口
app.listen(3000);
console.log('网站服务器启动成功');
```
```markdown
异步函数错误的捕获
```
```markdown
在node.js中，异步API的错误信息都是通过回调函数获
取的，支持Promise对象的异步API发生错误可以通
过catch方法捕获。
异步函数执行如果发生错误要如何捕获错误呢？

try catch 可以捕获异步函数以及其他同步代码在执行过
程中发生的错误，但是不能其他类型的API发生的错误。
```
```js
// 引入express框架
const express = require('express');
const fs = require('fs');
const promisify = require('util').promisify;
const readFile = promisify(fs.readFile);
// 创建网站服务器
const app = express();

app.get('/index', async (req, res, next) => {
	try {
		await readFile('./aaa.js')
	}catch (ex) {
		next(ex);
	}
})

// 错误处理中间件
app.use((err, req, res, next) => {
	res.status(500).send(err.message);
})

// 监听端口
app.listen(3000);
console.log('网站服务器启动成功');
```
```markdown
构造模块化路由的基础代码 
```
```js
// 引入express框架
const express = require('express');
// 创建网站服务器
const app = express();
// 创建路由对象
const home = express.Router();
// 为路由对象匹配请求路径
app.use('/home', home);
// 创建二级路由
home.get('/index', (req, res) => {
	res.send('欢迎来到博客首页页面')
})

// 端口监听`
app.listen(3000);
```
```markdown
构建模块化路由
```
```js
// home.js
 const home = express.Router(); 
 home.get('/index', () => {
     res.send('欢迎来到博客展示页面');
 });
 module.exports = home;
// admin.js
 const admin = express.Router();
 admin.get('/index', () => {
     res.send('欢迎来到博客管理页面');
 });
 module.exports = admin;
// app.js
 const home = require('./route/home.js');
 const admin = require('./route/admin.js');
 app.use('/home', home);
 app.use('/admin', admin);
```
```markdown
get参数的获取
```
```markdown
Express框架中使用req.query即可获取GET参数，框架内
部会将GET参数转换为对象并返回。
```
```js
// 引入express框架
const express = require('express');
// 创建网站服务器
const app = express();

app.get('/index', (req, res) => {
	// 获取get请求参数
	res.send(req.query)
})

// 端口监听
app.listen(3000);
```
```markdown
post参数的获取
```
```markdown
Express中接收post请求参数需要借助第三方包 body-parser。
```
```js
// 引入express框架
const express = require('express');
const bodyParser = require('body-parser');
// 创建网站服务器
const app = express();
// 拦截所有请求
// extended: false 方法内部使用querystring模块处理请求参数的格式
// extended: true 方法内部使用第三方模块qs处理请求参数的格式
app.use(bodyParser.urlencoded({extended: false}))

app.post('/add', (req, res) => {
	// 接收post请求参数
	res.send(req.body)
})

// 端口监听
app.listen(3000);
```
```markdown
路由参数
```
```js
// 引入express框架
const express = require('express');
const bodyParser = require('body-parser');
// 创建网站服务器
const app = express();

app.get('/index/:id/:name/:age', (req, res) => {
	// 接收post请求参数
	res.send(req.params)
})

// 端口监听
app.listen(3000);
```
```markdown
静态资源的处理
```
```markdown
通过Express内置的express.static可以方便地托管静态
文件，例如img、CSS、JavaScript 文件等。
```
```js
const express = require('express');
const path = require('path');
const app = express();

// 实现静态资源访问功能
app.use('/static',express.static(path.join(__dirname, 'public')))

// 端口监听
app.listen(3000);
```
```markdown
express-art-template模板引擎
```
```markdown
为了使art-template模板引擎能够更好的和Express框架
配合，模板引擎官方在原art-template模板引擎的基础
上封装了express-art-template。

使用npm install art-template express-art-template命
令进行安装。
```

```js
const express = require('express');
const path = require('path');
const app = express();

// 1.告诉express框架使用什么模板引擎渲染什么后缀的模板文件
//  1.模板后缀
//  2.使用的模板疫情
app.engine('art', require('express-art-template'))
// 2.告诉express框架模板存放的位置是什么
app.set('views', path.join(__dirname, 'views'))
// 3.告诉express框架模板的默认后缀是什么
app.set('view engine', 'art');

app.get('/index', (req, res) => {
	// 1. 拼接模板路径
	// 2. 拼接模板后缀
	// 3. 哪一个模板和哪一个数据进行拼接
	// 4. 将拼接结果响应给了客户端
	res.render('index', {
		msg: 'message'
	})
});

app.get('/list', (req, res) => {
	res.render('list', {
		msg: 'list page'
	})
})


// 端口监听
app.listen(3000);
```
```markdown
app.locals 对象
```
```markdown
将变量设置到app.locals对象下面，这个数据在所有
的模板中都可以获取到。
```
```js
const express = require('express');
const path = require('path');
const app = express();
// 模板配置
app.engine('art', require('express-art-template'))
app.set('views', path.join(__dirname, 'views'))
app.set('view engine', 'art');

app.locals.users = [{
	name: 'zhangsan',
	age: 20
},{
	name: '李四',
	age: 30
}]

app.get('/index', (req, res) => {
	res.render('index', {
		msg: '首页'
	})
});

app.get('/list', (req, res) => {
	res.render('list', {
		msg: '列表页'
	});
})


// 端口监听
app.listen(3000);
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复
ngpe即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
