# Axios
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021071221563926.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1 Axios的引言

```markdown
# axios的引言
	Axios 是一个  异步请求 技术

# 异步请求
	基于XMLHttpRequest对象发起的请求都是异步请求

# 异步请求特点
	请求之后页面不动,响应回来更新的是页面的局部,
	多个请求之间互不影响,并行执行
	ajax确实用来发送异步请求,ajax过气  
	系统架构 前后端分离架构系统   ---- 异步请求技术 ----->   Vue 全家桶系列 前端技术端  Vue  淘汰了jQuery  
```

## 2 Axios基本入门

### 2.1 下载Axios
```markdown
下载地址: https://unpkg.com/axios/dist/axios.min.js
```


### 2.2 Axios的案例
#### 2.2.1 GET方式请求

```html
<!--引入axios的相关依赖-->
 <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
 <script>
     //get方式请求
     axios.get('http://localhost:8888/axios/findAll?username=zhangsan&password=123')//发送请求的url
         .then(function(response){
             console.log(response.data);
         })//响应回来触发的回调函数
         .catch(function(err){ //当请求出现错误时回调函数
             console.log(err);
         });


 </script>
```

#### 2.2.2 POST方式的请求

```html
<!--引入axios的相关依赖-->
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script>
    //post请求
    axios.post('http://localhost:8888/axios/save',{name:"zhangsan",age:23}).then(function (response) {
        console.log(response.data);
    }).catch(function (err) {
        console.log(err);
    });
</script>
```

```markdown
# 总结
1. axios在发送post方式的请求时传递的参数如果为对
象类型,axios会自动将对象转为json格式的字符串使用 
application/json的请求头向后端服务接口传递参数



2. axios的post请求传递参数的两种方式:
第一种使用字符串进行参数传递: "name=zhangsan&age=23" 
这种形式
第二种方式后端接口直接使用@RequestBody注解形式接收参数: 
3. 发送get跟post时，发送参数名称要跟接受参数名称
保持一致（如果是对象的话，则跟对象属性的名称保持一致）
```


## 3 Axios的并发请求
```markdown
并发请求: 在同一时间发送多个不同的请求到后端
服务,最后同一处理不同服务的响应结果
```

```javascript
 <!--引入axios的相关依赖-->
 <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
 <script>
     //并发请求: 在同一时间发送多个不同的请求到后端服务,最后同一处理不同服务的响应结果
     function findAll(){
         return axios.get("http://localhost:8888/axios/findAll?username=xiaochen&password=123");
     }
     function save(){
         return axios.post('h tp://localhost:8888/axios/save',{name:"xiaosun",age:23})
     }
     //并行发送
     axios.all([findAll(),save()]).then(
         axios.spread(function(result1,result2){
             console.log(result1.data);
             console.log(result2.data);
         })//用来统一处理多个并发请求的执行结果
     );//all用来处理并发请求
 </script>
```

```markdown
# 总结
1.针对于并发请求需要用到axios.all()函数来完成并发请求的处理
2.针对于并发请求的结果汇总需要使用axios.spread()函数来统一
汇总请求结果
```
## 4 Axios的Restful风格的API

```js
# Axios的API总结
  axios.request(config)
  axios.get(url[, config])
  axios.delete(url[, config])
  axios.head(url[, config])
  axios.post(url[, data[, config]])
  axios.put(url[, data[, config]])
  axios.patch(url[, data[, config]])
 
  NOTE:
在使用别名方法时， url、method、data 这些属
性都不必在配置中指定。
```

## 5 Axios的高级使用配置对象

### 5.1 配置对象

```json
{
  // `url` 是用于请求的服务器 URL
  url: '/user',

  // `method` 是创建请求时使用的方法
  method: 'get', // 默认是 get

  // `baseURL` 将自动加在 `url` 前面，除非 `url` 是一个绝对 URL。
  // 它可以通过设置一个 `baseURL` 便于为 axios 实例的方法传递相对 URL
  baseURL: 'htt ps://some-domain.com/api/',

  // `transformRequest` 允许在向服务器发送前，修改请求数据
  // 只能用在 'PUT', 'POST' 和 'PATCH' 这几个请求方法
  // 后面数组中的函数必须返回一个字符串，或 ArrayBuffer，或 Stream
  transformRequest: [function (data) {
    // 对 data 进行任意转换处理

    return data;
  }],

  // `transformResponse` 在传递给 then/catch 前，允许修改响应数据
  transformResponse: [function (data) {
    // 对 data 进行任意转换处理

    return data;
  }],

  // `headers` 是即将被发送的自定义请求头
  headers: {'X-Requested-With': 'XMLHttpRequest'},

  // `params` 是即将与请求一起发送的 URL 参数
  // 必须是一个无格式对象(plain object)或 URLSearchParams 对象
  params: {
    ID: 12345
  },

  // `paramsSerializer` 是一个负责 `params` 序列化的函数
  // (e.g. https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
  paramsSerializer: function(params) {
    return Qs.stringify(params, {arrayFormat: 'brackets'})
  },

  // `data` 是作为请求主体被发送的数据
  // 只适用于这些请求方法 'PUT', 'POST', 和 'PATCH'
  // 在没有设置 `transformRequest` 时，必须是以下类型之一：
  // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
  // - 浏览器专属：FormData, File, Blob
  // - Node 专属： Stream
  data: {
    firstName: 'Fred'
  },

  // `timeout` 指定请求超时的毫秒数(0 表示无超时时间)
  // 如果请求话费了超过 `timeout` 的时间，请求将被中断
  timeout: 1000,

  // `withCredentials` 表示跨域请求时是否需要使用凭证
  withCredentials: false, // 默认的

  // `adapter` 允许自定义处理请求，以使测试更轻松
  // 返回一个 promise 并应用一个有效的响应 (查阅 [response docs](#response-api)).
  adapter: function (config) {
    /* ... */
  },

  // `auth` 表示应该使用 HTTP 基础验证，并提供凭据
  // 这将设置一个 `Authorization` 头，覆写掉现有的任意使用 `headers` 设置的自定义 `Authorization`头
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },

  // `responseType` 表示服务器响应的数据类型，可以是 'arraybuffer', 'blob', 'document', 'json', 'text', 'stream'
  responseType: 'json', // 默认的

  // `xsrfCookieName` 是用作 xsrf token 的值的cookie的名称
  xsrfCookieName: 'XSRF-TOKEN', // default

  // `xsrfHeaderName` 是承载 xsrf token 的值的 HTTP 头的名称
  xsrfHeaderName: 'X-XSRF-TOKEN', // 默认的

  // `onUploadProgress` 允许为上传处理进度事件
  onUploadProgress: function (progressEvent) {
    // 对原生进度事件的处理
  },

  // `onDownloadProgress` 允许为下载处理进度事件
  onDownloadProgress: function (progressEvent) {
    // 对原生进度事件的处理
  },

  // `maxContentLength` 定义允许的响应内容的最大尺寸
  maxContentLength: 2000,

  // `validateStatus` 定义对于给定的HTTP 响应状态码是 resolve 或 reject  promise 。如果 `validateStatus` 返回 `true` (或者设置为 `null` 或 `undefined`)，promise 将被 resolve; 否则，promise 将被 rejecte
  validateStatus: function (status) {
    return status >= 200 && status < 300; // 默认的
  },

  // `maxRedirects` 定义在 node.js 中 follow 的最大重定向数目
  // 如果设置为0，将不会 follow 任何重定向
  maxRedirects: 5, // 默认的

  // `httpAgent` 和 `httpsAgent` 分别在 node.js 中用于定义在执行 http 和 https 时使用的自定义代理。允许像这样配置选项：
  // `keepAlive` 默认没有启用
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),

  // 'proxy' 定义代理服务器的主机名称和端口
  // `auth` 表示 HTTP 基础验证应当用于连接代理，并提供凭据
  // 这将会设置一个 `Proxy-Authorization` 头，覆写掉已有的通过使用 `header` 设置的自定义 `Proxy-Authorization` 头。
  proxy: {
    host: '127.0.0.1',
    port: 9000,
    auth: : {
      username: 'mikeymike',
      password: 'rapunz3l'
    }
  },

  // `cancelToken` 指定用于取消请求的 cancel token
  // （查看后面的 Cancellation 这节了解更多）
  cancelToken: new CancelToken(function (cancel) {
  })
}
```

### 5.2 使用配置对象形式发送请求

```js
 var instance = axios.create({
   method:"GET",
   baseURL:"http://localhost:8888",
   data:{  //作为请求体发送的数据,只适用于这些请求方法 'PUT', 'POST', 和 'PATCH'

   }
 });

instance.get("/axios/findAll?username=zhangsan");
```
## 6 在脚手架中使用axios

### 6.1 安装axios

```js
# 1.安装axios
	npm install axios --save-dev

# 2.配置main.js中引入axios
	import axios from 'axios';

	Vue.prototype.$http=axios;

# 3.使用axios
	在需要发送异步请求的位置:this.$http.get("url").then((res)=>{}) this.$http.post("url").then((res)=>{})
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复
Axios即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
