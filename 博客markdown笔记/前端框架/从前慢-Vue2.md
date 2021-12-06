# Vue2
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210712214055541.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


## 1 Vue 引言
```markdown
Vue（读音[vju]，类似于view），不要读错
渐进式 JavaScript 框架   --摘自官网
渐进式意味着你可以将Vue作为你应用的一部分嵌入到项目中，
带来更丰富的交互体验。
或者如果你希望将更多的业务逻辑使用vue实现，
那么Vue的核心库以及其生态系统。比如
Core+Vue- router+Vuex，也可以满足你各种各样的需求
Vue有很多特点和Web开发中常见的高级功能
解耦视图和数据
可复用的组件
前端路由技术
状态管理
虚拟Dom

声明式渲染→组件系统→客户端路由→集中式状态管理→项目构建
```





```markdown
# 渐进式

   1. 易用  html css javascript
   2. 高效  开发前端页面 非常高效 
   3. 灵活  开发灵活 多样性

# 总结
		Vue 是一个javascript 框架

# 后端服务端开发人员: 
		Vue 渐进式javascript框架: 让我们通过操作很少的DOM,
		甚至不需要操作页面中任何DOM元素,就很容易的完成
		数据和视图绑定  双向绑定 MVVM  
		
		注意: 日后在使用Vue过程中页面中不要在引入Jquery框架
		
htmlcss--->javascript ----->jquery---->angularjs -----> Vue
 
 # Vue 作者
 		 尤雨溪   国内的    
```

## 2 Vue入门
### 2.1 下载Vuejs

```js
//开发版本:
	<!-- 开发环境版本，包含了有帮助的命令行警告 -->
	<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

//生产版本:
	<!-- 生产环境版本，优化了尺寸和速度 -->
	<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```
### 2.2  传统开发模式对比与Vue之间的对比
```js
传统开发模式对比
原生JS
<div id="msg"></div>
<script type="text/javascript">
var msg = 'Hello World';
var div = document.getElementById('msg');
div.innerHTML = msg;
</script> 

jQuery
<div id="msg"></div>
<script type="text/javascript" src="js/jquery.js"></script> 
<script type="text/javascript">
var msg = 'Hello World';
$('#msg').html(msg);
</script>

Vue

<div id="app">
<div>{{msg}}</div>
</div>
<script type="text/javascript" src="js/vue.js"></script> 
<script type="text/javascript">
new Vue({
el: '#app',
data: {
msg: 'HelloWorld'
}
})
</script>
```
### 2.3 Vue第一个入门应用

```html
<div id="app">
       {{ msg }}  {{username}} {{pwd}}

       <br>
       <span>
           {{ username }}
           <h1>{{ msg }}</h1>
       </span>

  </div>


    <!--引入vue.js-->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        const app = new Vue({
            el:"#app",  //element 用来给Vue实例定义一个作用范围
            data:{      //用来给Vue实例定义一些相关数据
                msg:"百知欢迎你,期待你的加入!",
                username:"hello Vue!",
                pwd :"12345",
            },
        });
    </script>
```

```markdown
# 总结:
1.vue实例(对象)中el属性: 	代表Vue的作用范围  
日后在Vue的作用范围内都可以使用Vue的语法
2.vue实例(对象)中data属性: 用来给Vue实例绑
定一些相关数据, 绑定的数据可以通过{{变量名}}在
Vue作用范围内取出
3.在使用{{}}进行获取data中数据时,可以在{{}}
中书写表达式,运算符,调用相关方法,以及逻辑运算等
4.el属性中可以书写任意的CSS选择器
[jquery选择器],但是在使用Vue开发是推荐使用 id选择器
```

### 2.4  Vue中的MVVM
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200310172105394.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200808005905320.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
## 3 v-text、v-html、v-pre、v-cloak、v-once
```markdown
1、v-text指令用于将数据填充到标签中，作用于插值表达式类似，
但是没有闪动问题
2、v-html指令用于将HTML片段填充到标签中，但是可能有安全问题
3、v-pre用于显示原始信息
显示原始信息，跳过编译过程（分析编译过程）
```


### 3.1 v-text
```markdown
v-text:用来获取data中数据将数据以文本的形式
渲染到指定标签内部             
类似于javascript 中 innerText
```


```html
		<div id="app" class="aa">
        <span >{{ message }}</span>
        <span v-text="message"></span>
    </div>

    <!--引入vue.js-->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        const app = new Vue({
            el:"#app",
            data:{
                message:"百知欢迎您"
            }
        })
    </script>
```

```markdown
# 总结
1.{{}}(插值表达式)和v-text获取数据的区别在于 
a.使用v-text取值会将标签中原有的数据覆盖 
使用插值表达式的形式不会覆盖标签原有的数据
b.使用v-text可以避免在网络环境较差的情况下出现插值闪烁
```
### 3.2 v-html
```markdown
v-html用来获取data中数据将数据中含有的html标签先
解析在渲染到指定标签的内部  类似于javascript中 innerHTML
```


```html
<div id="app" class="aa">
        <span>{{message}}</span>
        <br>
        <span v-text="message"></span>

        <br>
        <span v-html="message">xxxxxx</span>
    </div>

    <!--引入vue.js-->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        const app = new Vue({
            el:"#app",
            data:{
                message:"<a href=''>百知欢迎您</a>"
            }
        })
    </script>
```
### 3.3 v-pre
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <div>{{msg}}</div>
    <div v-text='msg'></div>
    <div v-html='msg1'></div>
    <div v-pre>{{msg}}</div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      1、v-text指令用于将数据填充到标签中，作用于插值表达式类似，但是没有闪动问题
      2、v-html指令用于将HTML片段填充到标签中，但是可能有安全问题
      3、v-pre用于显示原始信息
    */
    var vm = new Vue({
      el: '#app',
      data: {
        msg: 'Hello Vue',
        msg1: '<h1>HTML</h1>'
      }
    });
  </script>
</body>
</html>
```
### 3.4 v-cloak
```markdown
v-cloak的作用：插值表达式存在的问题：“闪动”
如何解决该问题：使用v-cloak指令
解决该问题的原理：先隐藏，替换好值之后再显示最终的值
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <style type="text/css">
  [v-cloak]{
    display: none;
  }
  </style>
</head>
<body>
  <div id="app">
    <div v-cloak>{{msg}}</div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      v-cloak指令的用法
      1、提供样式
        [v-cloak]{
          display: none;
        }
      2、在插值表达式所在的标签中添加v-cloak指令

      背后的原理：先通过样式隐藏内容，然后在内存中进行值的替换，替换好之后再显示最终的结果
    */
    var vm = new Vue({
      el: '#app',
      data: {
        msg: 'Hello Vue'
      }
    });
  </script>
</body>
</html>
```
### 3.5 v-once
```markdown
v-once的应用场景：如果显示的信息后续不需要再修改，
你们可以使用v-once，这样可以提高性能。
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <div>{{msg}}</div>
    <div v-once>{{info}}</div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      v-once的应用场景：如果显示的信息后续不需要再修改，你们可以使用v-once，这样可以提高性能。
    */
    var vm = new Vue({
      el: '#app',
      data: {
        msg: 'Hello Vue',
        info: 'nihao'
      }
    });
  </script>
</body>
</html>
```
 
## 4 vue中事件绑定(v-on)
```markdown
1.Vue如何处理事件？
v-on指令用法
<input type=‘button' v-on:click='num++'/>
v-on简写形式
<input type=‘button' @click='num++'/>

2. 事件函数的调用方式
直接绑定函数名称
<button v-on:click='say'>Hello</button>
调用函数
<button v-on:click='say()'>Say hi</button>

3. 事件函数参数传递
普通参数和事件对象
<button v-on:click='say("hi",$event)'>Say hi</button>

4. 事件修饰符
.stop 阻止冒泡
.prevent 阻止默认行为
<a v-on:click.stop="handle">跳转</a>
<a v-on:click.prevent="handle">跳转</a>
5. 按键修饰符
.enter 回车键
.esc 退出键
<input v-on:keyup.enter='submit'>
<input v-on:keyup.delete='handle'>
6. 自定义按键修饰符
全局 config.keyCodes 对象
Vue.config.keyCodes.f1 = 112
```


### 4.1 绑定事件基本语法

```html
		<div id="app">
        <h2>{{message}}</h2>
        <h2 v-text="message"></h2>
        <h2>年龄:{{ age }}</h2>
        <br>
        <input type="button" value="点我改变年龄" v-on:click="changeage">
    </div>
    <!--引入vue.js-->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        const app = new Vue({
            el:"#app",
            data:{
                message:"hello 欢迎来到我的课堂!",
                age:23,
            },
            methods:{  //methods 用来定义vue中时间
                changeage:function(){
                    alert('点击触发');
                }
            }
        })
    </script>
```

```markdown
# 总结:
事件  事件源:发生事件dom元素  事件: 发生特定的动作  
click....  监听器  发生特定动作之后的事件处理程序 
通常是js中函数
1.在vue中绑定事件是通过v-on指令来完成的
v-on:事件名 如  v-on:click
2.在v-on:事件名的赋值语句中是当前时间触发调用的函数名
3.在vue中事件的函数统一定义在Vue实例的methods属性中
4.在vue定义的事件中this指的就是当前的Vue实例,日后可
以在事件中通过使用this获取Vue实例中相关数据
```

### 4.2 Vue中事件的简化语法

```html
		<div id="app">
        <h2>{{ age }}</h2>
        <input type="button" value="通过v-on事件修改年龄每次+1" v-on:click="changeage">
        <input type="button" value="通过@绑定时间修改年龄每次-1" @click="editage">
    </div>
    <!--引入vue.js-->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        const app = new Vue({
           el:"#app",  //element: 用来指定vue作用范围
           data:{
               age:23,
           },    //data   : 用来定义vue实例中相关数据
           methods:{
               changeage:function(){
                   this.age++;
               },
               editage:function(){
                   this.age--;
               }

           }  //methods: 用来定义事件的处理函数
        });
    </script>
```

```markdown
# 总结:
1.日后在vue中绑定事件时可以通过@符号形式 
简化  v-on 的事件绑定
```

### 4.3 Vue事件函数两种写法

```html
		<div id="app">
        <span>{{count}}</span>
        <input type="button" value="改变count的值" @click="changecount">
    </div>
    <!--引入vue.js-->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        const app = new Vue({
           el:"#app",
           data:{
               count:1,
           },
           methods:{
               /*changecount:function(){
                   this.count++;
               }*/
               changecount(){
                   this.count++;
               }
           }
        });
    </script>
```

```markdown
# 总结:
1.在Vue中事件定义存在两种写法  一种是 函数名:function(){}  
不太推荐        一种是  函数名(){} 推荐
```

### 4.4 Vue事件参数传递

```html
		<div id="app">
        <span>{{count}}</span>
        <input type="button" value="改变count为指定的值" @click="changecount(23,'xiaohei')">
    </div>

    <!--引入vue.js-->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        const app = new Vue({
           el:"#app",
           data:{
               count:1,
           },
           methods:{
               //定义changecount
               changecount(count,name){
                   this.count = count;
                   alert(name);
               }

           }
        });
    </script>
```
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>

<body>
    <div id="app">
        <div>{{num}}</div>
        <div>
            <!-- 如果事件直接绑定函数名称，那么默认会传递事件对象作为事件函数的第一个参数 -->
            <button v-on:click='handle1'>点击1</button>
            <!-- 2、如果事件绑定函数调用，那么事件对象必须作为最后一个参数显示传递，
                 并且事件对象的名称必须是$event 
            -->
            <button v-on:click='handle2(123, 456, $event)'>点击2</button>
        </div>
    </div>
    <script type="text/javascript" src="js/vue.js"></script>
    <script type="text/javascript">
        var vm = new Vue({
            el: '#app',
            data: {
                num: 0
            },
            methods: {
                handle1: function(event) {
                    console.log(event.target.innerHTML)
                },
                handle2: function(p, p1, event) {
                    console.log(p, p1)
                    console.log(event.target.innerHTML)
                    this.num++;
                }
            }
        });
    </script>
</body>

</html>
```
```markdown
# 总结:
1.在使用事件时,可以直接在事件调用出给事件进行参数传递,
在事件定义出通过定义对应变量接收传递的参数
```
### 4.5 自定义事件修饰符
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <input type="text" v-on:keyup.aaa='handle' v-model='info'>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      事件绑定-自定义按键修饰符
      规则：自定义按键修饰符名字是自定义的，但是对应的值必须是按键对应event.keyCode值
    */
    Vue.config.keyCodes.aaa = 65
    var vm = new Vue({
      el: '#app',
      data: {
        info: ''
      },
      methods: {
        handle: function(event){
          console.log(event.keyCode)
        }
      }
    });
  </script>
</body>
</html>

```

 
## 5 v-show v-if v-bind
### 5.1 v-show
```markdown
v-show:用来控制页面中某个标签元素是否展示        
底层使用控制是 display 属性
```

```html
<div id="app">
    <!--
        v-show: 用来控制标签展示还是隐藏的
    -->
    <h2 v-show="false">欢迎你的加入!</h2>
    <h2 v-show="show">欢迎你的加入这是vue中定义变量true!</h2>
    <input type="button" value="展示隐藏标签" @click="showmsg">

</div>
<!--引入vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            show:false,
        },
        methods:{
            //定义时间
            showmsg(){
               this.show =  !this.show;
            }
        }
    })
</script>
```

```markdown
# 总结
1.在使用v-show时可以直接书写boolean值控制元素展示,
也可以通过变量控制标签展示和隐藏
2.在v-show中可以通过boolean表达式控制标签的展示课隐藏
```

### 5.2 v-if 
```markdown
v-if: 用来控制页面元素是否展示          
底层控制是DOM元素    操作DOM
```


```html
<div id="app">
    <h2 v-if="false">教育</h2>
    <h2 v-if="show">教育欢迎你的加入</h2>
</div>
<!--引入vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            show:false
        },
        methods:{

        }
    });
</script>
```
### 5.3 v-if跟v-show的区别
```markdown
v-if跟v-show的区别:v-if会删除标签元素，
而v-show只会添加display:none
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  
</head>
<body>
  <div id="app">
    <div v-if='score>=90'>优秀</div>
    <div v-else-if='score<90&&score>=80'>良好</div>
    <div v-else-if='score<80&&score>60'>一般</div>
    <div v-else>比较差</div>
    <div v-show='flag'>测试v-show</div>
    <button v-on:click='handle'>点击</button>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      分支结构

      v-show的原理：控制元素样式是否显示 display:none
    */
    var vm = new Vue({
      el: '#app',
      data: {
        score: 10,
        flag: false
      },
      methods: {
        handle: function(){
          this.flag = !this.flag;
        }
      }
    });
  </script>
</body>
</html>

```
### 5.4 v-bind
```markdown
v-bind: 用来绑定标签的属性从而通过vue动态修改标签的属性
```


```html
<div id="app">

    <img width="300" v-bind:title="msg" v-bind:class="{aa:showCss}"  src="yxjlogo.jpg" alt="">
    
</div>
<!--引入vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>

    const app = new Vue({
        el:"#app",
        data:{
            msg:"logo!!!!",
            showCss:true,
        },
        methods:{
        }
    })
</script>
```

### 5.5 v-bind 简化写法
```markdown
vue为了方便我们日后绑定标签的属性提供了对属性绑
定的简化写法如 v-bind:属性名 简化之后 :属性名
```

```html
<div id="app">
    <img width="300" :title="msg" :class="{aa:showCss}"  :src="src" alt="">
    <input type="button" value="动态控制加入样式" @click="addCss">
    <input type="button" value="改变图片" @click="changeSrc">
</div>
<!--引入vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>

    const app = new Vue({
        el:"#app",
        data:{
            msg:"教育官方logo!!!!",
            showCss:true,
            src:"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1583490365568&di=52a82bd614cd4030f97ada9441bb2d0e&imgtype=0&src=http%3A%2F%2Fimg.kanzhun.com%2Fimages%2Flogo%2F20160714%2F820a68f65b4e4a3634085055779c000c.jpg"
        },
        methods:{
            addCss(){
                this.showCss= !this.showCss;
            },
            changeSrc(){
                this.src = "https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=1925088662,1336364220&fm=26&gp=0.jpg";
            }
        }
    })
</script>
```



## 6 v-for的使用
### 6.1 循环结构-遍历数组
```markdown
v-for遍历数组
<li v-for='item in list'>{{item}}</li>
key的作用：帮助Vue区分不同的元素，从而提高性能
<li v-for='(item,index) in list'>{{item}} + '---' +{{index}}</li>
<li :key='item.id' v-for='(item,index) in list'>{{item}} + '---' {{index}}</li>
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  
</head>
<body>
  <div id="app">
    <div>水果列表</div>
    <ul>
      <li v-for='item in fruits'>{{item}}</li>
      <li v-for='(item, index) in fruits'>{{item + '---' + index}}</li>
      <li :key='item.id' v-for='(item, index) in myFruits'>
        <span>{{item.ename}}</span>
        <span>-----</span>
        <span>{{item.cname}}</span>
      </li>

    </ul>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      循环结构-遍历数组
    */
    var vm = new Vue({
      el: '#app',
      data: {
        fruits: ['apple', 'orange', 'banana'],
        myFruits: [{
          id: 1,
          ename: 'apple',
          cname: '苹果'
        },{
          id: 2,
          ename: 'orange',
          cname: '橘子'
        },{
          id: 3,
          ename: 'banana',
          cname: '香蕉'
        }]
      }
    });
  </script>
</body>
</html>

```
### 6.2 循环结构-遍历对象
```markdown
<div v-for='(value, key, index) in object'></div>
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  
</head>
<body>
  <div id="app">
    <div v-if='v==13' v-for='(v,k,i) in obj'>{{v + '---' + k + '---' + i}}</div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    // 使用原生js遍历对象
    var obj = {
      uname: 'lisi',
      age: 12,
      gender: 'male'
    }
    for(var key in obj) {
      console.log(key, obj[key])
    }
    /*
      循环结构
    */
    var vm = new Vue({
      el: '#app',
      data: {
        obj: {
          uname: 'zhangsan',
          age: 13,
          gender: 'female'
        }
      }
    });
  </script>
</body>
</html>

```


```markdown
# 总结
1.在使用v-for的时候一定要注意加入:key 
用来给vue内部提供重用和排序的唯一key 
```


## 7 v-model 双向绑定
```markdown
v-model: 作用用来绑定标签元素的值与vue实例对象
中data数据保持一致,从而实现双向的数据绑定机制

指令v-model的本质
<input v-bind:value="msg" v-on:input="msg=$event.target.value">
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <div>{{msg}}</div>
    <input type="text" v-bind:value="msg" v-on:input='handle'>
    <input type="text" v-bind:value="msg" v-on:input='msg=$event.target.value'>
    <input type="text" v-model='msg'>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      v-model指令的本质

    */
    var vm = new Vue({
      el: '#app',
      data: {
        msg: 'hello'
      },
      methods: {
        handle: function(event){
          // 使用输入域中的最新的数据覆盖原来的数据
          this.msg = event.target.value;
        }
      }
    });
  </script>
</body>
</html>
```

```html
<div id="app">
    <input type="text" v-model="message">
    <span>{{message}}</span>
    <hr>
    <input type="button" value="改变Data中值" @click="changeValue">
</div>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    const app = new Vue({
        el: "#app",
        data: {
            message:""
        },
        methods: {
            changeValue(){
                this.message='教育!';
            }
        }
    });
</script>
```

```markdown
# 总结
1.使用v-model指令可以实现数据的双向绑定 
2.所谓双向绑定 表单中数据变化导致vue实
例data数据变化   
vue实例中data数据的变化导致表单中数据变化 称之为双向绑定

# MVVM架构  双向绑定机制
	Model: 数据  Vue实例中绑定数据
	
	VM:   ViewModel  监听器

	View:  页面  页面展示的数据
```

## 8 事件修饰符
```markdown
修饰符: 作用用来和事件连用,用来决定事件触发
条件或者是阻止事件的触发机制
```


```markdown
# 1.常用的事件修饰符
	.stop
	.prevent
	.capture
	.self
	.once
	.passive
```

### 8.1 stop事件修饰符
```markdown
 用来阻止事件冒泡
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <div>{{num}}</div>
    <div v-on:click='handle0'>
      <button v-on:click.stop='handle1'>点击1</button>
    </div>
    <div>
      <a href="http://www.baidu.com" v-on:click.prevent='handle2'>百度</a>
    </div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      事件绑定-事件修饰符
    */
    var vm = new Vue({
      el: '#app',
      data: {
        num: 0
      },
      methods: {
        handle0: function(){
          this.num++;
        },
        handle1: function(event){
          // 阻止冒泡
          // event.stopPropagation();
        },
        handle2: function(event){
          // 阻止默认行为
          // event.preventDefault();
        }
      }
    });
  </script>
</body>
</html>

```
```html
<div id="app">
    <div class="aa" @click="divClick">
        <!--用来阻止事件冒泡-->
        <input type="button" value="按钮" @click.stop="btnClick">
    </div>
</div>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    const app = new Vue({
        el: "#app",
        data: {},
        methods: {
            btnClick(){
                alert('button被点击了');
            },
            divClick(){
                alert('div被点击了');
            }
        }
    });
</script>
```

### 8.2 prevent 事件修饰符
```markdown
用来阻止标签的默认行为
```

```html
		<!--用来阻止事件的默认行为-->
    <a href="http://www.baidu.com/" @click.prevent="aClick">教育</a>
```

### 8.3 self 事件修饰符
```markdown
 用来针对于当前标签的事件触发     ===========> 
 只触发自己标签的上特定动作的事件     
 只关心自己标签上触发的事件 不监听事件冒泡
```


```html
<!--只触发标签自身的事件-->
  <div class="aa" @click.self="divClick">
      <!--用来阻止事件冒泡-->
      <input type="button" value="按钮" @click.stop="btnClick">
      <input type="button" value="按钮1" @click="btnClick1">
  </div>

```

### 8.4 once 事件修饰符
```markdown
 once 一次 作用:  就是让指定事件只触发一次
```


```html
    <!--
    .prevent : 用来阻止事件的默认行为
    .once    : 用来只执行一次特定的事件
    -->
    <a href="http://www.yxjbest.com/" @click.prevent.once="aClick">教育</a>
```

## 9 按键修饰符
```markdown
作用: 用来与键盘中按键事件绑定在一起,
用来修饰特定的按键事件的修饰符
```


```markdown
# 按键修饰符
	.enter
	.tab
	.delete (捕获“删除”和“退格”键)
	.esc
	.space
	.up
	.down
	.left
	.right
```

### 9.1 enter 回车键
```markdown
用来在触发回车按键之后触发的事件
```


```html
 <input type="text" v-model="msg" @keyup.enter="keyups">
```

### 9.2 tab 键
```markdown
用来捕获到tab键执行到当前标签是才会触发
```
```html
<input type="text" @keyup.tab="keytabs">
```
## 10 样式绑定
```markdown
1. class样式处理
 对象语法
<div v-bind:class="{ active: isActive }"></div>
数组语法
<div v-bind:class="[activeClass, errorClass]"></div>
2. style样式处理
对象语法
<div v-bind:style="{ color: activeColor, fontSize: fontSize }"></div>
数组语法
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```
### 10.1 样式绑定之class绑定对象用法
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <style type="text/css">
    .active {
      border: 1px solid red;
      width: 100px;
      height: 100px;
    }
    .error {
      background-color: orange;
    }
  </style>
</head>
<body>
  <div id="app">
    <div v-bind:class="{active: isActive,error: isError}">
      测试样式
    </div>
    <button v-on:click='handle'>切换</button>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      样式绑定

    */
    var vm = new Vue({
      el: '#app',
      data: {
        isActive: true,
        isError: true
      },
      methods: {
        handle: function(){
          // 控制isActive的值在true和false之间进行切换
          this.isActive = !this.isActive;
          this.isError = !this.isError;
        }
      }
    });
  </script>
</body>
</html>
```
### 10.2 样式绑定之class绑定数组用法
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <style type="text/css">
    .active {
      border: 1px solid red;
      width: 100px;
      height: 100px;
    }
    .error {
      background-color: orange;
    }
  </style>
</head>
<body>
  <div id="app">
    <div v-bind:class='[activeClass, errorClass]'>测试样式</div>
    <button v-on:click='handle'>切换</button>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      样式绑定

    */
    var vm = new Vue({
      el: '#app',
      data: {
        activeClass: 'active',
        errorClass: 'error'
      },
      methods: {
        handle: function(){
          this.activeClass = '';
          this.errorClass = '';
        }
      }
    });
  </script>
</body>
</html>

```
### 10.3 样式绑定之class绑定3个细节用法
```markdown
1、对象绑定和数组绑定可以结合使用
2、class绑定的值可以简化操作
3、默认的class如何处理？默认的class会保留
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <style type="text/css">
    .active {
      border: 1px solid red;
      width: 100px;
      height: 100px;
    }
    .error {
      background-color: orange;
    }
    .test {
      color: blue;
    }
    .base {
      font-size: 28px;
    }
  </style>
</head>
<body>
  <div id="app">
    <div v-bind:class='[activeClass, errorClass, {test: isTest}]'>测试样式</div>
    <div v-bind:class='arrClasses'></div>
    <div v-bind:class='objClasses'></div>
    <div class="base" v-bind:class='objClasses'></div>

    <button v-on:click='handle'>切换</button>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      样式绑定相关语法细节：
      1、对象绑定和数组绑定可以结合使用
      2、class绑定的值可以简化操作
      3、默认的class如何处理？默认的class会保留
      
    */
    var vm = new Vue({
      el: '#app',
      data: {
        activeClass: 'active',
        errorClass: 'error',
        isTest: true,
        arrClasses: ['active','error'],
        objClasses: {
          active: true,
          error: true
        }
      },
      methods: {
        handle: function(){
          // this.isTest = false;
          this.objClasses.error = false;
        }
      }
    });
  </script>
</body>
</html>
```
### 10.4  样式绑定之style绑定用法
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  
</head>
<body>
  <div id="app">
    <div v-bind:style='{border: borderStyle, width: widthStyle, height: heightStyle}'></div>
    <div v-bind:style='objStyles'></div>
    <div v-bind:style='[objStyles, overrideStyles]'></div>
    <button v-on:click='handle'>切换</button>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      样式绑定之内联样式Style：
      
    */
    var vm = new Vue({
      el: '#app',
      data: {
        borderStyle: '1px solid blue',
        widthStyle: '100px',
        heightStyle: '200px',
        objStyles: {
          border: '1px solid green',
          width: '200px',
          height: '100px'
        },
        overrideStyles: {
          border: '5px solid orange',
          backgroundColor: 'blue'
        }
      },
      methods: {
        handle: function(){
          this.heightStyle = '100px';
          this.objStyles.width = '100px';
        }
      }
    });
  </script>
</body>
</html>

```
## 11 表单
### 11.1 表单基本操作
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <style type="text/css">
  
  form div {
    height: 40px;
    line-height: 40px;
  }
  form div:nth-child(4) {
    height: auto;
  }
  form div span:first-child {
    display: inline-block;
    width: 100px;
  }
  </style>
</head>
<body>
  <div id="app">
    <form action="http://itcast.cn">
      <div>
        <span>姓名：</span>
        <span>
          <input type="text" v-model='uname'>
        </span>
      </div>
      <div>
        <span>性别：</span>
        <span>
          <input type="radio" id="male" value="1" v-model='gender'>
          <label for="male">男</label>
          <input type="radio" id="female" value="2" v-model='gender'>
          <label for="female">女</label>
        </span>
      </div>
      <div>
        <span>爱好：</span>
        <input type="checkbox" id="ball" value="1" v-model='hobby'>
        <label for="ball">篮球</label>
        <input type="checkbox" id="sing" value="2" v-model='hobby'>
        <label for="sing">唱歌</label>
        <input type="checkbox" id="code" value="3" v-model='hobby'>
        <label  for="code">写代码</label>
      </div>
      <div>
        <span>职业：</span>
        <select v-model='occupation' multiple>
          <option value="0">请选择职业...</option>
          <option value="1">教师</option>
          <option value="2">软件工程师</option>
          <option value="3">律师</option>
        </select>
      </div>
      <div>
        <span>个人简介：</span>
        <textarea v-model='desc'></textarea>
      </div>
      <div>
        <input type="submit" value="提交" @click.prevent='handle'>
      </div>
    </form>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      表单基本操作
    */
    var vm = new Vue({
      el: '#app',
      data: {
        uname: 'lisi',
        gender: 2,
        hobby: ['2','3'],
        // occupation: 3
        occupation: ['2','3'],
        desc: 'nihao'
      },
      methods: {
        handle: function(){
          // console.log(this.uname)
          // console.log(this.gender)
          // console.log(this.hobby.toString())
          // console.log(this.occupation)
          console.log(this.desc)

        }
      }
    });
  </script>
</body>
</html>
```
### 11.2 表单域修饰符用法
```markdown
v-model修饰符的使用
lazy:失去焦点是才会触发双向绑定,应用于<input>标签
number:应用于<input>标签,可以使String类型转换为number类型
trim:去除空格,应用于<input>标签
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <input type="text" v-model.number='age'>
    <input type="text" v-model.trim='info'>
    <input type="text" v-model.lazy='msg'>
    <div>{{msg}}</div>
    <button @click='handle'>点击</button>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      表单域修饰符
    */
    var vm = new Vue({
      el: '#app',
      data: {
        age: '',
        info: '',
        msg: ''
      },
      methods: {
        handle: function(){
          // console.log(this.age + 13)
          // console.log(this.info.length)
        }
      }
    });
  </script>
</body>
</html>
```
## 12 自定义指令
### 12.1 自定义指令基本用法
```markdown
1 为何需要自定义指令？
内置指令不满足需求
2 自定义指令的语法规则（获取元素焦点）
Vue.directive('focus' {
inserted: function(el) {
// 获取元素的焦点
el.focus();
}
}) 
3 自定义指令用法
 <input type="text" v-focus>
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <input type="text" v-focus>
    <input type="text">
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      自定义指令
    */
    Vue.directive('focus', {
      inserted: function(el){
        // el表示指令所绑定的元素
        el.focus();
      }
    });
    var vm = new Vue({
      el: '#app',
      data: {
        
      },
      methods: {
        handle: function(){
          
        }
      }
    });
  </script>
</body>
</html>

```
### 12.2 带参数的自定义指令
```markdown
带参数的自定义指令（改变元素背景色）
Vue.directive(‘color', {
inserted: function(el, binding) {
el.style.backgroundColor = binding.value.color;
}
})

 指令的用法
 <input type="text" v-color='{color:"orange"}'>
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <input type="text" v-color='msg'>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      自定义指令-带参数
    */
    Vue.directive('color', {
      bind: function(el, binding){
        // 根据指令的参数设置背景色
        // console.log(binding.value.color)
        el.style.backgroundColor = binding.value.color;
      }
    });
    var vm = new Vue({
      el: '#app',
      data: {
        msg: {
          color: 'blue'
        }
      },
      methods: {
        handle: function(){
          
        }
      }
    });
  </script>
</body>
</html>

```
### 12.3 局部指令用法
```markdown
directives: {
focus: {
// 指令的定义
inserted: function (el) {
el.focus()
}
}
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
  <div id="app">
    <input type="text" v-color='msg'>
    <input type="text" v-focus>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      自定义指令-局部指令
    */
    var vm = new Vue({
      el: '#app',
      data: {
        msg: {
          color: 'red'
        }
      },
      methods: {
        handle: function(){
          
        }
      },
      directives: {
        color: {
          bind: function(el, binding){
            el.style.backgroundColor = binding.value.color;
          }
        },
        focus: {
          inserted: function(el) {
            el.focus();
          }
        }
      }
    });
  </script>
</body>
</html>

```
## 13 计算属性
```markdown
1. 为何需要计算属性？
表达式的计算逻辑可能会比较复杂，
使用计算属性可以使模板内容更加简洁
2. 计算属性的用法
computed: {
reversedMessage: function () {
return this.msg.split('').reverse().join('')
}
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
  <div id="app">
    <div>{{msg}}</div>
    <div>{{reverseString}}</div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      计算属性
    */
    var vm = new Vue({
      el: '#app',
      data: {
        msg: 'Nihao'
      },
      computed: {
        reverseString: function(){
          return this.msg.split('').reverse().join('');
        }
      }
    });
  </script>
</body>
</html>

```
### 13.1 计算属性基本用法
```markdown
计算属性与方法的区别
计算属性是基于它们的依赖进行缓存的
方法不存在缓存
使用缓存的好处：对于耗时间的操作，如果结果相同的话，
不需要在重复之前的操作，直接从缓存中拿
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <div>{{reverseString}}</div>
    <div>{{reverseString}}</div>
    <div>{{reverseMessage()}}</div>
    <div>{{reverseMessage()}}</div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      计算属性与方法的区别:计算属性是基于依赖进行缓存的，而方法不缓存
    */
    var vm = new Vue({
      el: '#app',
      data: {
        msg: 'Nihao',
        num: 100
      },
      methods: {
        reverseMessage: function(){
          console.log('methods')
          return this.msg.split('').reverse().join('');
        }
      },
      computed: {
        reverseString: function(){
          console.log('computed') //基于缓存后只打印一次
          // return this.msg.split('').reverse().join('');
          var total = 0;
          for(var i=0;i<=this.num;i++){
            total += i;
          }
          return total;
        }
      }
    });
  </script>
</body>
</html>

```
## 14 侦听器
### 14.1 侦听器基本用法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200818103131461.png#pic_center)
```markdown
1. 侦听器的应用场景
数据变化时执行异步或开销较大的操作
2. 侦听器的用法
watch: {
firstName: function(val){
// val表示变化之后的值
this.fullName = val + this.lastName;
},
lastName: function(val) {
this.fullName = this.firstName + val;
}
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
  <div id="app">
    <div>
      <span>名：</span>
      <span>
        <input type="text" v-model='firstName'>
      </span>
    </div>
    <div>
      <span>姓：</span>
      <span>
        <input type="text" v-model='lastName'>
      </span>
    </div>
    <div>{{fullName}}</div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      侦听器
    */
    var vm = new Vue({
      el: '#app',
      data: {
        firstName: 'Jim',
        lastName: 'Green',
        // fullName: 'Jim Green'
      },
      computed: {
        fullName: function(){
          return this.firstName + ' ' + this.lastName;
        }
      },
      watch: {
        // firstName: function(val) {
        //   this.fullName = val + ' ' + this.lastName;
        // },
        // lastName: function(val) {
        //   this.fullName = this.firstName + ' ' + val;
        // }
      }
    });
  </script>
</body>
</html>
```
### 14.2 验证用户名是否可用
```markdown
需求：输入框中输入姓名，失去焦点时验证是否存在，如果已
经存在，提示从新输入，如果不存在，提示可以使用。

1 通过v-model实现数据绑定
2  需要提供提示信息
3 需要侦听器监听输入信息的变化
4 需要修改触发的事件
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <div>
      <span>用户名：</span>
      <span>
        <input type="text" v-model.lazy='uname'>
      </span>
      <span>{{tip}}</span>
    </div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*      
      侦听器
      1、采用侦听器监听用户名的变化
      2、调用后台接口进行验证
      3、根据验证的结果调整提示信息
    */
    var vm = new Vue({
      el: '#app',
      data: {
        uname: '',
        tip: ''
      },
      methods: {
        checkName: function(uname) {
          // 调用接口，但是可以使用定时任务的方式模拟接口调用
          var that = this;
          setTimeout(function(){
            // 模拟接口调用
            if(uname == 'admin') {
              that.tip = '用户名已经存在，请更换一个';
            }else{
              that.tip = '用户名可以使用';
            }
          }, 2000);
        }
      },
      watch: {
        uname: function(val){
          // 调用后台接口验证用户名的合法性
          this.checkName(val);
          // 修改提示信息
          this.tip = '正在验证...';
        }
      }
    });

  </script>
</body>
</html>

```
## 15 过滤器基本用法
### 15.1 过滤器基本用法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200819080835889.png#pic_center)

```markdown
格式化数据，比如将字符串格式化为首字母大写，将日期格式化为指定的格式等

自定义过滤器
Vue.filter(‘过滤器名称’, function(value){
// 过滤器业务逻辑
})

过滤器的使用
<div>{{msg | upper}}</div>
<div>{{msg | upper | lower}}</div>
<div v-bind:id=“id | formatId"></div> 

局部过滤器
filters:{
capitalize: function(){}
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
  <div id="app">
    <input type="text" v-model='msg'>
    <div>{{msg | upper}}</div>
    <div>{{msg | upper | lower}}</div>
    <div :abc='msg | upper'>测试数据</div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      过滤器
      1、可以用与插值表达式和属性绑定
      2、支持级联操作
    */
    // Vue.filter('upper', function(val) {
    //   return val.charAt(0).toUpperCase() + val.slice(1);
    // });
    Vue.filter('lower', function(val) {
      return val.charAt(0).toLowerCase() + val.slice(1);
    });
    var vm = new Vue({
      el: '#app',
      data: {
        msg: ''
      },
      filters: {
        upper: function(val) {
          return val.charAt(0).toUpperCase() + val.slice(1);
        }
      }
    });
  </script>
</body>
</html>
```
### 15.2 带参数的过滤器案例
```markdown
带参数的过滤器
Vue.filter(‘format’, function(value, arg1){
// value就是过滤器传递过来的参数
})
过滤器的使用
<div>{{date | format(‘yyyy-MM-dd')}}</div>
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <div>{{date | format('yyyy-MM-dd hh:mm:ss')}}</div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      过滤器案例：格式化日期
      
    */
    // Vue.filter('format', function(value, arg) {
    //   if(arg == 'yyyy-MM-dd') {
    //     var ret = '';
    //     ret += value.getFullYear() + '-' + (value.getMonth() + 1) + '-' + value.getDate();
    //     return ret;
    //   }
    //   return value;
    // })
    Vue.filter('format', function(value, arg) {
      function dateFormat(date, format) {
          if (typeof date === "string") {
              var mts = date.match(/(\/Date\((\d+)\)\/)/);
              if (mts && mts.length >= 3) {
                  date = parseInt(mts[2]);
              }
          }
          date = new Date(date);
          if (!date || date.toUTCString() == "Invalid Date") {
              return "";
          }
          var map = {
              "M": date.getMonth() + 1, //月份 
              "d": date.getDate(), //日 
              "h": date.getHours(), //小时 
              "m": date.getMinutes(), //分 
              "s": date.getSeconds(), //秒 
              "q": Math.floor((date.getMonth() + 3) / 3), //季度 
              "S": date.getMilliseconds() //毫秒 
          };

          format = format.replace(/([yMdhmsqS])+/g, function(all, t) {
              var v = map[t];
              if (v !== undefined) {
                  if (all.length > 1) {
                      v = '0' + v;
                      v = v.substr(v.length - 2);
                  }
                  return v;
              } else if (t === 'y') {
                  return (date.getFullYear() + '').substr(4 - all.length);
              }
              return all;
          });
          return format;
      }
      return dateFormat(value, arg);
    })
    var vm = new Vue({
      el: '#app',
      data: {
        date: new Date()
      }
    });
  </script>
</body>
</html>
```
## 16 Vue 生命周期
```markdown
生命周期钩子   ====>  生命周期函数  
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022222203228.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```markdown
# Vue生命周期总结
1.初始化阶段
beforeCreate(){ 
//1.生命周期中第一个函数,该函数在执行时Vue实例仅仅完成了自身事件的绑定和生命周期函数的初始化工作,Vue实例中还没有 Data el methods相关属性
            console.log("beforeCreate: "+this.msg);
        },
        created(){ //2.生命周期中第二个函数,该函数在执行时Vue实例已经初始化了data属性和methods中 相关方法
            console.log("created: "+this.msg);
        },
        beforeMount(){//3.生命周期中第三个函数,该函数在执行时Vue将El中指定作用范围作为模板编译
            console.log("beforeMount: "+document.getElementById("sp").innerText);
        },
        mounted(){//4.生命周期中第四个函数,该函数在执行过程中,已经将数据渲染到界面中并且已经更新页面
            console.log("Mounted: "+document.getElementById("sp").innerText);
        }
        
		2.运行阶段
			 	beforeUpdate(){//5.生命周期中第五个函数,该函数是data中数据发生变化时执行 这个事件执行时仅仅是Vue实例中data数据变化页面显示的依然是原始数据
            console.log("beforeUpdate:"+this.msg);
            console.log("beforeUpdate:"+document.getElementById("sp").innerText);
        },
        updated(){    //6.生命周期中第六个函数,该函数执行时data中数据发生变化,页面中数据也发生了变化  页面中数据已经和data中数据一致
            console.log("updated:"+this.msg);
            console.log("updated:"+document.getElementById("sp").innerText);
        },
        
		3.销毁阶段
			 	beforeDestory(){//7.生命周期第七个函数,该函数执行时,Vue中所有数据 methods componet 都没销毁

        },
        destoryed(){ //8.生命周期的第八个函数,该函数执行时,Vue实例彻底销毁

        }
```
```html
beforeCreate 在实例初始化之后，数据观测和事件配置之前被调用。
created 在实例创建完成后被立即调用。
beforeMount 在挂载开始之前被调用。
mounted el被新创建的vm.$el替换，并挂载到实例上去之后调用该钩子。
beforeUpdate 数据更新时调用，发生在虚拟DOM打补丁之前。
updated 由于数据更改导致的虚拟DOM重新渲染和打补丁，在这之后会调用该钩子。
beforeDestroy 实例销毁之前调用。
destroyed 实例销毁后调用。

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <div>{{msg}}</div>
    <button @click='update'>更新</button>
    <button @click='destroy'>销毁</button>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      Vue实例的生命周期
      
    */
    var vm = new Vue({
      el: '#app',
      data: {
        msg: '生命周期'
      },
      methods: {
        update: function(){
          this.msg = 'hello';
        },
        destroy: function(){
          this.$destroy();
        }
      },
      beforeCreate: function(){
        console.log('beforeCreate');
      },
      created: function(){
        console.log('created');
      },
      beforeMount: function(){
        console.log('beforeMount');
      },
      mounted: function(){
        console.log('mounted');
      },
      beforeUpdate: function(){
        console.log('beforeUpdate');
      },
      updated: function(){
        console.log('updated');
      },
      beforeDestroy: function(){
        console.log('beforeDestroy');
      },
      destroyed: function(){
        console.log('destroyed');
      }
    });
  </script>
</body>
</html>
```
## 17 图书管理
### 17.1 变异方法和替换数组
```markdown
1. 变异方法(修改原有数据)
push()
pop()
shift()
unshift()
splice()
sort()
reverse()

2. 替换数组(生成新的数组)
filter()
concat()
slice()
```
```html

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <div>
      <span>
        <input type="text" v-model='fname'>
        <button @click='add'>添加</button>
        <button @click='del'>删除</button>
        <button @click='change'>替换</button>
      </span>
    </div>
    <ul>
      <li :key='index' v-for='(item,index) in list'>{{item}}</li>
    </ul>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      Vue数组操作
      1、变异方法：会影响数组的原始数据的变化。
      2、替换数组：不会影响原始的数组数据，而是形成一个新的数组。
    */
    var vm = new Vue({
      el: '#app',
      data: {
        fname: '',
        list: ['apple','orange','banana']
      },
      methods: {
        add: function(){
          this.list.push(this.fname);
        },
        del: function(){
          this.list.pop();
        },
        change: function(){
          this.list = this.list.slice(0,2);
        }
      }
    });
  </script>
</body>
</html>
```
### 17.2 动态响应式数据处理
```markdown
Vue.set(vm.items, indexOfItem, newValue)
vm.$set(vm.items, indexOfItem, newValue)
参数一表示要处理的数组名称
参数二表示要处理的数组的索引
参数三表示要处理的数组的值
```
```html

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <ul>
      <li v-for='item in list'>{{item}}</li>
    </ul>
    <div>
      <div>{{info.name}}</div>
      <div>{{info.age}}</div>
      <div>{{info.gender}}</div>
    </div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      动态处理响应式数据
      
    */
    var vm = new Vue({
      el: '#app',
      data: {
        list: ['apple', 'orange', 'banana'],
        info: {
          name: 'lisi',
          age: 12
        }
      },
    });
    // vm.list[1] = 'lemon';
    // Vue.set(vm.list, 2, 'lemon');
    vm.$set(vm.list, 1, 'lemon');

    // vm.info.gender = 'male';
    vm.$set(vm.info, 'gender', 'female');

    
  </script>
</body>
</html>
```
### 17.3 图书管理案例-常用特性应用场景
```markdown
1. 图书列表
实现静态列表效果
基于数据实现模板效果
处里每行的操作按钮

2.添加图书
实现表单的静态效果
添加图书表单域数据绑定
添加按钮事件绑定
实现添加业务逻辑

3. 修改图书
修改信息填充到表单
修改后重新提交表单
重用添加和修改的方法

4. 删除图书
删除按钮绑定事件处理方法
实现删除业务逻辑

5. 常用特性应用场景
过滤器（格式化日期）
自定义指令（获取表单焦点）
计算属性（统计图书数量）
侦听器（验证图书存在性）
生命周期（图书数据处理）
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200819095213947.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```html

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <style type="text/css">
    .grid {
      margin: auto;
      width: 530px;
      text-align: center;
    }
    .grid table {
      border-top: 1px solid #C2D89A;
      width: 100%;
      border-collapse: collapse;
    }
    .grid th,td {
      padding: 10;
      border: 1px dashed #F3DCAB;
      height: 35px;
      line-height: 35px;
    }
    .grid th {
      background-color: #F3DCAB;
    }
    .grid .book {
      padding-bottom: 10px;
      padding-top: 5px;
      background-color: #F3DCAB;
    }
    .grid .total {
      height: 30px;
      line-height: 30px;
      background-color: #F3DCAB;
      border-top: 1px solid #C2D89A;
    }
  </style>
</head>
<body>
  <div id="app">
    <div class="grid">
      <div>
        <h1>图书管理</h1>
        <div class="book">
          <div>
            <label for="id">
              编号：
            </label>
            <input type="text" id="id" v-model='id' :disabled="flag" v-focus>
            <label for="name">
              名称：
            </label>
            <input type="text" id="name" v-model='name'>
            <button @click='handle' :disabled="submitFlag">提交</button>
          </div>
        </div>
      </div>
      <div class="total">
        <span>图书总数：</span>
        <span>{{total}}</span>
      </div>
      <table>
        <thead>
          <tr>
            <th>编号</th>
            <th>名称</th>
            <th>时间</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          <tr :key='item.id' v-for='item in books'>
            <td>{{item.id}}</td>
            <td>{{item.name}}</td>
            <td>{{item.date | format('yyyy-MM-dd hh:mm:ss')}}</td>
            <td>
              <a href="" @click.prevent='toEdit(item.id)'>修改</a>
              <span>|</span>
              <a href="" @click.prevent='deleteBook(item.id)'>删除</a>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      图书管理-添加图书
    */
    Vue.directive('focus', {
      inserted: function (el) {
        el.focus();
      }
    });
    Vue.filter('format', function(value, arg) {
      function dateFormat(date, format) {
        if (typeof date === "string") {
          var mts = date.match(/(\/Date\((\d+)\)\/)/);
          if (mts && mts.length >= 3) {
            date = parseInt(mts[2]);
          }
        }
        date = new Date(date);
        if (!date || date.toUTCString() == "Invalid Date") {
          return "";
        }
        var map = {
          "M": date.getMonth() + 1, //月份 
          "d": date.getDate(), //日 
          "h": date.getHours(), //小时 
          "m": date.getMinutes(), //分 
          "s": date.getSeconds(), //秒 
          "q": Math.floor((date.getMonth() + 3) / 3), //季度 
          "S": date.getMilliseconds() //毫秒 
        };
        format = format.replace(/([yMdhmsqS])+/g, function(all, t) {
          var v = map[t];
          if (v !== undefined) {
            if (all.length > 1) {
              v = '0' + v;
              v = v.substr(v.length - 2);
            }
            return v;
          } else if (t === 'y') {
            return (date.getFullYear() + '').substr(4 - all.length);
          }
          return all;
        });
        return format;
      }
      return dateFormat(value, arg);
    })
    var vm = new Vue({
      el: '#app',
      data: {
        flag: false,
        submitFlag: false,
        id: '',
        name: '',
        books: []
      },
      methods: {
        handle: function(){
          if(this.flag) {
            // 编辑图书
            // 就是根据当前的ID去更新数组中对应的数据
            this.books.some((item) => {
              if(item.id == this.id) {
                item.name = this.name;
                // 完成更新操作之后，需要终止循环
                return true;
              }
            });
            this.flag = false;
          }else{
            // 添加图书
            var book = {};
            book.id = this.id;
            book.name = this.name;
            book.date = 2525609975000;
            this.books.push(book);
            // 清空表单
            this.id = '';
            this.name = '';
          }
          // 清空表单
          this.id = '';
          this.name = '';
        },
        toEdit: function(id){
          // 禁止修改ID
          this.flag = true;
          console.log(id)
          // 根据ID查询出要编辑的数据
          var book = this.books.filter(function(item){
            return item.id == id;
          });
          console.log(book)
          // 把获取到的信息填充到表单
          this.id = book[0].id;
          this.name = book[0].name;
        },
        deleteBook: function(id){
          // 删除图书
          // 根据id从数组中查找元素的索引
          // var index = this.books.findIndex(function(item){
          //   return item.id == id;
          // });
          // 根据索引删除数组元素
          // this.books.splice(index, 1);
          // -------------------------
          // 方法二：通过filter方法进行删除
          this.books = this.books.filter(function(item){
            return item.id != id;
          });
        }
      },
      computed: {
        total: function(){
          // 计算图书的总数
          return this.books.length;
        }
      },
      watch: {
        name: function(val) {
          // 验证图书名称是否已经存在
          var flag = this.books.some(function(item){
            return item.name == val;
          });
          if(flag) {
            // 图书名称存在
            this.submitFlag = true;
          }else{
            // 图书名称不存在
            this.submitFlag = false;
          }
        }
      },
      mounted: function(){
        // 该生命周期钩子函数被触发的时候，模板已经可以使用
        // 一般此时用于获取后台数据，然后把数据填充到模板
        var data = [{
          id: 1,
          name: '三国演义',
          date: 2525609975000
        },{
          id: 2,
          name: '水浒传',
          date: 2525609975000
        },{
          id: 3,
          name: '红楼梦',
          date: 2525609975000
        },{
          id: 4,
          name: '西游记',
          date: 2525609975000
        }];
        this.books = data;
      }
    });
  </script>
</body>
</html>

```
## 18 Vue中组件(Component)
### 18.1 组件作用
```markdown
组件作用: 用来减少Vue实例对象中代码量,日后在使用Vue
开发过程中,可以根据 不能业务功能将页面中划分不同的多
个组件,然后由多个组件去完成整个页面的布局,便于日后使
用Vue进行开发时页面管理,方便开发人员维护。
```

### 18.2 组件使用

#### 18.2.1 全局组件注册
```markdown
说明:全局组件注册给Vue实例,日后可以在任意Vue实例
的范围内使用该组件
```
```js
//1.开发全局组件
		Vue.component('login',{
        template:'<div><h1>用户登录</h1></div>'
    });
//2.使用全局组件  在Vue实例范围内
		<login></login>  
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <button-counter></button-counter>
    <button-counter></button-counter>
    <button-counter></button-counter>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      组件注册
    */
    Vue.component('button-counter', {
      data: function(){
        return {
          count: 0
        }
      },
      template: '<button @click="handle">点击了{{count}}次</button>',
      methods: {
        handle: function(){
          this.count += 2;
        }
      }
    })
    var vm = new Vue({
      el: '#app',
      data: {
        
      }
    });
  </script>
</body>
</html>
```
```markdown
注意:
1.Vue.component用来开发全局组件 参数1: 组件的名称  
参数2: 组件配置{}  template:''用来书写组件的html代码  
template中必须有且只有一个root元素
2.使用时需要在Vue的作用范围内根据组件名使用全局组件
3.如果在注册组件过程中使用 驼峰命名组件的方式 在使用
组件时 必须将驼峰的所有单词小写加入-线进行使用
```
#### 18.2.2 组件注册注意事项-上
```markdown
data必须是一个函数
组件模板内容必须是单个跟元素
模板字符串需要浏览器提供支持(ES6语法)
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <button-counter></button-counter>
    <button-counter></button-counter>
    <button-counter></button-counter>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      组件注册注意事项
      1、组件参数的data值必须是函数
      2、组件模板必须是单个跟元素
      3、组件模板的内容可以是模板字符串

    */
    // Vue.component('button-counter', {
    //   data: function(){
    //     return {
    //       count: 0
    //     }
    //   },
    //   template: '<div><button @click="handle">点击了{{count}}次</button><button>测试</button></div>',
    //   methods: {
    //     handle: function(){
    //       this.count += 2;
    //     }
    //   }
    // })
    // -----------------------------------
    Vue.component('button-counter', {
      data: function(){
        return {
          count: 0
        }
      },
      template: `
        <div>
          <button @click="handle">点击了{{count}}次</button>
          <button>测试123</button>
        </div>
      `,
      methods: {
        handle: function(){
          this.count += 2;
        }
      }
    })
    var vm = new Vue({
      el: '#app',
      data: {
        
      }
    });
  </script>
</body>
</html>


```
#### 18.2.3 组件注册注意事项-下
```markdown
组件命名方式
短横线方式
Vue.component('my-component', { /* ... */ })
驼峰方式
Vue.component('MyComponent', { /* ... */ })
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <button-counter></button-counter>
    <hello-world></hello-world>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      组件注册注意事项
      如果使用驼峰式命名组件，那么在使用组件的时候，只能在字符串模板中用驼峰的方式使用组件，但是
      在普通的标签模板中，必须使用短横线的方式使用组件
    */
    Vue.component('HelloWorld', {
      data: function(){
        return {
          msg: 'HelloWorld'
        }
      },
      template: '<div>{{msg}}</div>'
    });
    Vue.component('button-counter', {
      data: function(){
        return {
          count: 0
        }
      },
      template: `
        <div>
          <button @click="handle">点击了{{count}}次</button>
          <button>测试123</button>
          <HelloWorld></HelloWorld>
        </div>
      `,
      methods: {
        handle: function(){
          this.count += 2;
        }
      }
    })
    var vm = new Vue({
      el: '#app',
      data: {
        
      }
    });
  </script>
</body>
</html>

```
#### 18.2.4 局部组件注册
```markdown
说明:通过将组件注册给对应Vue实例中一个components属
性来完成组件注册,这种方式不会对Vue实例造成累加
```

##### 18.2.4.1 第一种开发方式

```js
		//局部组件登录模板声明
    let login ={   //具体局部组件名称
        template:'<div><h2>用户登录</h2></div>'
    };
    
    const app = new Vue({
        el: "#app",
        data: {},
        methods: {},
        components:{  //用来注册局部组件
            login:login  //注册局部组件
        }
    });

	 //局部组件使用 在Vue实例范围内
	 <login></login>
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <hello-world></hello-world>
    <hello-tom></hello-tom>
    <hello-jerry></hello-jerry>
    <test-com></test-com>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      局部组件注册
      局部组件只能在注册他的父组件中使用
    */
    Vue.component('test-com',{
      template: '<div>Test<hello-world></hello-world></div>'
    });
    var HelloWorld = {
      data: function(){
        return {
          msg: 'HelloWorld'
        }
      },
      template: '<div>{{msg}}</div>'
    };
    var HelloTom = {
      data: function(){
        return {
          msg: 'HelloTom'
        }
      },
      template: '<div>{{msg}}</div>'
    };
    var HelloJerry = {
      data: function(){
        return {
          msg: 'HelloJerry'
        }
      },
      template: '<div>{{msg}}</div>'
    };
    var vm = new Vue({
      el: '#app',
      data: {
        
      },
      components: {
        'hello-world': HelloWorld,
        'hello-tom': HelloTom,
        'hello-jerry': HelloJerry
      }
    });
  </script>
</body>
</html>
```
##### 18.2.4.2 第二种开发方式
```js
  //1.声明局部组件模板  template 标签 注意:在Vue实例作用范围外声明
    <template id="loginTemplate">
        <h1>用户登录</h1>
    </template>
  
  //2.定义变量用来保存模板配置对象
      let login ={   //具体局部组件名称
          template:'#loginTemplate'  //使用自定义template标签选择器即可
      };
  
  //3.注册组件	
      const app = new Vue({
          el: "#app",
          data: {},
          methods: {},
          components:{  //用来注册局部组件
              login:login  //注册局部组件
          }
      });
  
   //4.局部组件使用 在Vue实例范围内
  	 <login></login>
```
## 19 组件间数据交互
### 19.1 父组件向子组件传值-基本使用
```markdown
1. 组件内部通过props接收传递过来的值
Vue.component(‘menu-item', {
props: ['title'],
template: '<div>{{ title }}</div>'
})
2. 父组件通过属性将值传递给子组件
<menu-item title="来自父组件的数据"></menu-item>
<menu-item :title="title"></menu-item>

作用:props用来给组件传递相应静态数据或者是动态数据的
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <div>{{pmsg}}</div>
    <menu-item title='来自父组件的值'></menu-item>
    <menu-item :title='ptitle' content='hello'></menu-item>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      父组件向子组件传值-基本使用
    */
    Vue.component('menu-item', {
      props: ['title', 'content'],
      data: function() {
        return {
          msg: '子组件本身的数据'
        }
      },
      template: '<div>{{msg + "----" + title + "-----" + content}}</div>'
    });
    var vm = new Vue({
      el: '#app',
      data: {
        pmsg: '父组件中内容',
        ptitle: '动态绑定属性'
      }
    });
  </script>
</body>
</html>
```
#### 19.1.1 通过在组件上声明静态数据传递给组件内部
```js
//1.声明组件模板配置对象
    let login = {
        template:"<div><h1>欢迎:{{ userName }} 年龄:{{ age }}</h1></div>",
        props:['userName','age']  //props作用 用来接收使用组件时通过组件标签传递的数据
    }

//2.注册组件
    const app = new Vue({
        el: "#app",
        data: {},
        methods: {},
        components:{
            login //组件注册
        }
    });

//3.通过组件完成数据传递
	<login user-name="小陈" age="23"></login>
```

```markdown
# 总结:
1.使用组件时可以在组件上定义多个属性以及对应数据
2.在组件内部可以使用props数组生命多个定义在组件上的属性名 
日后可以在组件中通过{{ 属性名 }} 方式获取组件中属性值
```

#### 19.1.2 通过在组件上声明动态数据传递给组件内部

```js
//1.声明组件模板对象
    const login = {
        template:'<div><h2>欢迎: {{ name }} 年龄:{{ age }}</h2></div>',
        props:['name','age']
    }
 
//2.注册局部组件
    const app = new Vue({
        el: "#app",
        data: {
            username:"小陈陈",
            age:23
        },
        methods: {},
        components:{
            login //注册组件
        }
    });

//3.使用组件
	 <login :name="username" :age="age"></login>  //使用v-bind形式将数据绑定Vue实例中data属性,日后data属性发生变化,组件内部数据跟着变化
```
### 19.2 props属性名规则
```markdown
在props中使用驼峰形式，模板中需要使用短横线的形式
字符串形式的模板中没有这个限制
Vue.component(‘menu-item', {
// 在 JavaScript 中是驼峰式的
props: [‘menuTitle'],
template: '<div>{{ menuTitle }}</div>'
})
<!– 在html中是短横线方式的 -->
<menu-item menu-title=“nihao"></menu-item>
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <div>{{pmsg}}</div>
    <menu-item :menu-title='ptitle'></menu-item>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      父组件向子组件传值-props属性名规则
    */
    Vue.component('third-com', {
      props: ['testTile'],
      template: '<div>{{testTile}}</div>'
    });
    Vue.component('menu-item', {
      props: ['menuTitle'],
      template: '<div>{{menuTitle}}<third-com testTile="hello"></third-com></div>'
    });
    var vm = new Vue({
      el: '#app',
      data: {
        pmsg: '父组件中内容',
        ptitle: '动态绑定属性'
      }
    });
  </script>
</body>
</html>

```
### 19.3 父组件向子组件传值-props属性值类型
```markdown
字符串 String
数值 Number
布尔值 Boolean 
数组 Array
对象 Object
:pnum='12'需要这样写，否则类型是string类型而不是number类型
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <div>{{pmsg}}</div>
    <menu-item :pstr='pstr' :pnum='12' pboo='true' :parr='parr' :pobj='pobj'></menu-item>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      父组件向子组件传值-props属性值类型
    */
    
    Vue.component('menu-item', {
      props: ['pstr','pnum','pboo','parr','pobj'],
      template: `
        <div>
          <div>{{pstr}}</div>
          <div>{{12 + pnum}}</div>
          <div>{{typeof pboo}}</div>
          <ul>
            <li :key='index' v-for='(item,index) in parr'>{{item}}</li>
          </ul>
            <span>{{pobj.name}}</span>
            <span>{{pobj.age}}</span>
          </div>
        </div>
      `
    });
    var vm = new Vue({
      el: '#app',
      data: {
        pmsg: '父组件中内容',
        pstr: 'hello',
        parr: ['apple','orange','banana'],
        pobj: {
          name: 'lisi',
          age: 12
        }
      }
    });
  </script>
</body>
</html>

```
### 19.4 prop的单向数据流
```markdown
单向数据流:所有的 prop 都使得其父子 prop 之间形成了一个单向
下行绑定：父级 prop 的更新会向下流动到子组件中，但是反过来
则不行。
```

```markdown
所有的 prop 都使得其父子 prop 之间形成了一个单向下行绑定：
父级 prop 的更新会向下流动到子组件中，但是反过来则不行。
这样会防止从子组件意外改变父级组件的状态，从而导致你的应
用的数据流向难以理解。

额外的，每次父级组件发生更新时，子组件中所有的 prop 都将
会刷新为最新的值。这意味着你不应该在一个子组件内部改
变 prop。如果你这样做了，Vue 会在浏览器的控制台中发
出警告。---摘自官网
```
### 19.5 子组件向父组件传值
```markdown
子组件通过自定义事件向父组件传递信息
<button v-on:click='$emit("enlarge-text", 0.1) '>扩大字体</button>

父组件监听子组件的事件
<menu-item v-on:enlarge-text='fontSize += $event'></menu-item>
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <div :style='{fontSize: fontSize + "px"}'>{{pmsg}}</div>
    <menu-item :parr='parr' @enlarge-text='handle'></menu-item>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      子组件向父组件传值-基本用法
      props传递数据原则：单向数据流
    */
    
    Vue.component('menu-item', {
      props: ['parr'],
      template: `
        <div>
          <ul>
            <li :key='index' v-for='(item,index) in parr'>{{item}}</li>
          </ul>
          <button @click='$emit("enlarge-text")'>扩大父组件中字体大小</button>
        </div>
      `
    });
    var vm = new Vue({
      el: '#app',
      data: {
        pmsg: '父组件中内容',
        parr: ['apple','orange','banana'],
        fontSize: 10
      },
      methods: {
        handle: function(){
          // 扩大字体大小
          this.fontSize += 5;
        }
      }
    });
  </script>
</body>
</html>

```
### 19.6 子组件向父组件传值-携带参数
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <div :style='{fontSize: fontSize + "px"}'>{{pmsg}}</div>
    <menu-item :parr='parr' @enlarge-text='handle($event)'></menu-item>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      子组件向父组件传值-携带参数
    */
    
    Vue.component('menu-item', {
      props: ['parr'],
      template: `
        <div>
          <ul>
            <li :key='index' v-for='(item,index) in parr'>{{item}}</li>
          </ul>
          <button @click='$emit("enlarge-text", 5)'>扩大父组件中字体大小</button>
          <button @click='$emit("enlarge-text", 10)'>扩大父组件中字体大小</button>
        </div>
      `
    });
    var vm = new Vue({
      el: '#app',
      data: {
        pmsg: '父组件中内容',
        parr: ['apple','orange','banana'],
        fontSize: 10
      },
      methods: {
        handle: function(val){
          // 扩大字体大小
          this.fontSize += val;
        }
      }
    });
  </script>
</body>
</html>

```
### 19.7 非父子组件间传值
```markdown
1. 单独的事件中心管理组件间的通信
var eventHub = new Vue()
2. 监听事件与销毁事件
eventHub.$on('add-todo', addTodo)
eventHub.$off('add-todo')
3. 触发事件
eventHub.$emit(‘add-todo', id)
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200821042519893.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <div>父组件</div>
    <div>
      <button @click='handle'>销毁事件</button>
    </div>
    <test-tom></test-tom>
    <test-jerry></test-jerry>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      兄弟组件之间数据传递
    */
    // 提供事件中心
    var hub = new Vue();

    Vue.component('test-tom', {
      data: function(){
        return {
          num: 0
        }
      },
      template: `
        <div>
          <div>TOM:{{num}}</div>
          <div>
            <button @click='handle'>点击</button>
          </div>
        </div>
      `,
      methods: {
        handle: function(){
          hub.$emit('jerry-event', 2);
        }
      },
      mounted: function() {
        // 监听事件
        hub.$on('tom-event', (val) => {
          this.num += val;
        });
      }
    });
    Vue.component('test-jerry', {
      data: function(){
        return {
          num: 0
        }
      },
      template: `
        <div>
          <div>JERRY:{{num}}</div>
          <div>
            <button @click='handle'>点击</button>
          </div>
        </div>
      `,
      methods: {
        handle: function(){
          // 触发兄弟组件的事件
          hub.$emit('tom-event', 1);
        }
      },
      mounted: function() {
        // 监听事件
        hub.$on('jerry-event', (val) => {
          this.num += val;
        });
      }
    });
    var vm = new Vue({
      el: '#app',
      data: {
        
      },
      methods: {
        handle: function(){
          hub.$off('tom-event');
          hub.$off('jerry-event');
        }
      }
    });
  </script>
</body>
</html>
```
### 19.8 组件中定义数据和事件使用
#### 19.8.1 组件中定义属于组件的数据
```js
//组件声明的配置对象
    const login = {
        template:'<div><h1>{{ msg }} 教育</h1><ul><li v-for="item,index in lists">{{ index }}{{ item }}</li></ul></div>',
        data(){   //使用data函数方式定义组件的数据   在templatehtml代码中通过插值表达式直接获取
            return {
                msg:"hello",
                lists:['java','spring','springboot']
            }//组件自己内部数据
        }
    }
```

#### 19.8.2 组件中事件定义

```js
 const login={
        template:'<div><input type="button" value="点我触发组件中事件" @click="change"></div>',
        data(){
            return {
                name:'小陈'
            };
        },
        methods:{
            change(){
                alert(this.name)
                alert('触发事件');
            }
        }
    }
```

```markdown
# 总结	
1.组件中定义事件和直接在Vue中定义事件基本一致 直接在
组件内部对应的html代码上加入@事件名=函数名方式即可
2.在组件内部使用methods属性用来定义对应的事件函数即
可,事件函数中this 指向的是当前组件的实例
```
#### 19.8.3 向子组件中传递事件并在子组件中调用该事件
```markdown
在子组件中调用传递过来的相关事件必须使用 this.$emit('函数名') 方式
调用
```

```js
//1.声明组件
    const login = {
        template:"<div><h1>教育 {{ uname }}</h1> <input type='button' value='点我' @click='change'></div>",
        data(){
            return {
                uname:this.name
            }
        },
        props:['name'],
        methods:{
            change(){
                //调用vue实例中函数
                this.$emit('aaa');  //调用组件传递过来的其他函数时需要使用 this.$emit('函数名调用')
            }
        }
    }
    
 //2.注册组件
    	const app = new Vue({
        el: "#app",
        data: {
            username:"小陈"
        },
        methods: {
            findAll(){  //一个事件函数  将这个函数传递给子组件
                alert('Vue 实例中定义函数');
            }
        },
        components:{
            login,//组件的注册
        }
    });

//3.使用组件
	<login  @find="findAll"></login>    //=====> 在组件内部使用  this.$emit('find')
```
## 20 插槽 
### 20.1 插槽基本用法
```markdown
插槽位置

Vue.component('alert-box', {
template: `
<div class="demo-alert-box">
<strong>Error!</strong>
<slot></slot>
</div>
`
})

插槽内容
<alert-box>Something bad happened.</alert-box>
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <alert-box>有bug发生</alert-box>
    <alert-box>有一个警告</alert-box>
    <alert-box></alert-box>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      组件插槽：父组件向子组件传递内容
    */
    Vue.component('alert-box', {
      template: `
        <div>
          <strong>ERROR:</strong>
          <slot>默认内容</slot>
        </div>
      `
    });
    var vm = new Vue({
      el: '#app',
      data: {
        
      }
    });
  </script>
</body>
</html>

```
### 20.2 具名插槽用法
```markdown
1. 插槽定义
<div class="container">
<header>
<slot name="header"></slot>
</header>
<main>
<slot></slot>
</main>
<footer>
<slot name="footer"></slot>
</footer>
</div>

2. 插槽内容
<base-layout>
<h1 slot="header">标题内容</h1>
<p>主要内容1</p>
<p>主要内容2</p>
<p slot="footer">底部内容</p>
</base-layout>
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <base-layout>
      <p slot='header'>标题信息</p>
      <p>主要内容1</p>
      <p>主要内容2</p>
      <p slot='footer'>底部信息信息</p>
    </base-layout>

    <base-layout>
      <template slot='header'>
        <p>标题信息1</p>
        <p>标题信息2</p>
      </template>
      <p>主要内容1</p>
      <p>主要内容2</p>
      <template slot='footer'>
        <p>底部信息信息1</p>
        <p>底部信息信息2</p>
      </template>
    </base-layout>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      具名插槽
    */
    Vue.component('base-layout', {
      template: `
        <div>
          <header>
            <slot name='header'></slot>
          </header>
          <main>
            <slot></slot>
          </main>
          <footer>
            <slot name='footer'></slot>
          </footer>
        </div>
      `
    });
    var vm = new Vue({
      el: '#app',
      data: {
        
      }
    });
  </script>
</body>
</html>

```
### 20.3 作用域插槽用法
```markdown
应用场景：父组件对子组件的内容进行加工处理
1. 插槽定义
<ul>
<li v-for= "item in list" v-bind:key= "item.id" >
<slot v-bind:item="item">
{{item.name}}
</slot>
</li>
</ul>

2. 插槽内容
<fruit-list v-bind:list= "list">
<template slot-scope="slotProps">
<strong v-if="slotProps.item.current">
{{ slotProps.item.text }}
</strong>
</template>
</fruit-list>
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<style type="text/css">
  .current {
    color: orange;
  }
</style>
<body>
  <div id="app">
    <fruit-list :list='list'>
      <template slot-scope='slotProps'>
        <strong v-if='slotProps.info.id==3' class="current">{{slotProps.info.name}}</strong>
        <span v-else>{{slotProps.info.name}}</span>
      </template>
    </fruit-list>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    /*
      作用域插槽
    */
    Vue.component('fruit-list', {
      props: ['list'],
      template: `
        <div>
          <li :key='item.id' v-for='item in list'>
            <slot :info='item'>{{item.name}}</slot>
          </li>
        </div>
      `
    });
    var vm = new Vue({
      el: '#app',
      data: {
        list: [{
          id: 1,
          name: 'apple'
        },{
          id: 2,
          name: 'orange'
        },{
          id: 3,
          name: 'banana'
        }]
      }
    });
  </script>
</body>
</html>
```
### 20.4 购物车案例
```markdown
1. 按照组件化方式实现业务需求
根据业务功能进行组件化划分
标题组件（展示文本）
列表组件（列表展示、商品数量变更、商品删除）
结算组件（计算商品总额）

1. 功能实现步骤
实现整体布局和样式效果
划分独立的功能组件
组合所有的子组件形成整体结构
逐个实现各个组件功能
标题组件
列表组件
结算组件
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200821072733323.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <style type="text/css">
    .container {
    }
    .container .cart {
      width: 300px;
      margin: auto;
    }
    .container .title {
      background-color: lightblue;
      height: 40px;
      line-height: 40px;
      text-align: center;
      /*color: #fff;*/  
    }
    .container .total {
      background-color: #FFCE46;
      height: 50px;
      line-height: 50px;
      text-align: right;
    }
    .container .total button {
      margin: 0 10px;
      background-color: #DC4C40;
      height: 35px;
      width: 80px;
      border: 0;
    }
    .container .total span {
      color: red;
      font-weight: bold;
    }
    .container .item {
      height: 55px;
      line-height: 55px;
      position: relative;
      border-top: 1px solid #ADD8E6;
    }
    .container .item img {
      width: 45px;
      height: 45px;
      margin: 5px;
    }
    .container .item .name {
      position: absolute;
      width: 90px;
      top: 0;left: 55px;
      font-size: 16px;
    }

    .container .item .change {
      width: 100px;
      position: absolute;
      top: 0;
      right: 50px;
    }
    .container .item .change a {
      font-size: 20px;
      width: 30px;
      text-decoration:none;
      background-color: lightgray;
      vertical-align: middle;
    }
    .container .item .change .num {
      width: 40px;
      height: 25px;
    }
    .container .item .del {
      position: absolute;
      top: 0;
      right: 0px;
      width: 40px;
      text-align: center;
      font-size: 40px;
      cursor: pointer;
      color: red;
    }
    .container .item .del:hover {
      background-color: orange;
    }
  </style>
</head>
<body>
  <div id="app">
    <div class="container">
      <my-cart></my-cart>
    </div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    
    var CartTitle = {
      props: ['uname'],
      template: `
        <div class="title">{{uname}}的商品</div>
      `
    }
    var CartList = {
      props: ['list'],
      template: `
        <div>
          <div :key='item.id' v-for='item in list' class="item">
            <img :src="item.img"/>
            <div class="name">{{item.name}}</div>
            <div class="change">
              <a href="" @click.prevent='sub(item.id)'>－</a>
              <input type="text" class="num" :value='item.num' @blur='changeNum(item.id, $event)'/>
              <a href="" @click.prevent='add(item.id)'>＋</a>
            </div>
            <div class="del" @click='del(item.id)'>×</div>
          </div>
        </div>
      `,
      methods: {
        changeNum: function(id, event){
          this.$emit('change-num', {
            id: id,
            type: 'change',
            num: event.target.value
          });
        },
        sub: function(id){
          this.$emit('change-num', {
            id: id,
            type: 'sub'
          });
        },
        add: function(id){
          this.$emit('change-num', {
            id: id,
            type: 'add'
          });
        },
        del: function(id){
          // 把id传递给父组件
          this.$emit('cart-del', id);
        }
      }
    }
    var CartTotal = {
      props: ['list'],
      template: `
        <div class="total">
          <span>总价：{{total}}</span>
          <button>结算</button>
        </div>
      `,
      computed: {
        total: function() {
          // 计算商品的总价
          var t = 0;
          this.list.forEach(item => {
            t += item.price * item.num;
          });
          return t;
        }
      }
    }
    Vue.component('my-cart',{
      data: function() {
        return {
          uname: '张三',
          list: [{
            id: 1,
            name: 'TCL彩电',
            price: 1000,
            num: 1,
            img: 'img/a.jpg'
          },{
            id: 2,
            name: '机顶盒',
            price: 1000,
            num: 1,
            img: 'img/b.jpg'
          },{
            id: 3,
            name: '海尔冰箱',
            price: 1000,
            num: 1,
            img: 'img/c.jpg'
          },{
            id: 4,
            name: '小米手机',
            price: 1000,
            num: 1,
            img: 'img/d.jpg'
          },{
            id: 5,
            name: 'PPTV电视',
            price: 1000,
            num: 2,
            img: 'img/e.jpg'
          }]
        }
      },
      template: `
        <div class='cart'>
          <cart-title :uname='uname'></cart-title>
          <cart-list :list='list' @change-num='changeNum($event)' @cart-del='delCart($event)'></cart-list>
          <cart-total :list='list'></cart-total>
        </div>
      `,
      components: {
        'cart-title': CartTitle,
        'cart-list': CartList,
        'cart-total': CartTotal
      },
      methods: {
        changeNum: function(val) {
          // 分为三种情况：输入域变更、加号变更、减号变更
          if(val.type=='change') {
            // 根据子组件传递过来的数据，跟新list中对应的数据
            this.list.some(item=>{
              if(item.id == val.id) {
                item.num = val.num;
                // 终止遍历
                return true;
              }
            });
          }else if(val.type=='sub'){
            // 减一操作
            this.list.some(item=>{
              if(item.id == val.id) {
                item.num -= 1;
                // 终止遍历
                return true;
              }
            });
          }else if(val.type=='add'){
            // 加一操作
            this.list.some(item=>{
              if(item.id == val.id) {
                item.num += 1;
                // 终止遍历
                return true;
              }
            });
          }
        },
        delCart: function(id) {
          // 根据id删除list中对应的数据
          // 1、找到id所对应数据的索引
          var index = this.list.findIndex(item=>{
            return item.id == id;
          });
          // 2、根据索引删除对应数据
          this.list.splice(index, 1);
        }
      }
    });
    var vm = new Vue({
      el: '#app',
      data: {

      }
    });

  </script>
</body>
</html>
```
## 21 前后端交互模式
### 21.1 接口调用方式
```markdown
原生ajax
基于jQuery的ajax
fetch
axos
```
### 21.2 URL地址格式
```markdown
1 传统形式的URL
格式： schema://host:port/path？query#fragment
schema：协议。例http https ftp等
host：域名或者lP地址
port：端口http默认端口80，可以省略
path：路径例如/abc/a/b/c
query：查询参数，例如 uname=lisi&age=12
fragment：锚点（哈希Hash），用定位页面的某个位置
符合规则的URL
http：/www.itcast.cn
http：//www.itcast.cn/java/web
http：//www.itcast.cn/java/web？flag=1
http：//www.itcastcn/java/web？flag=1#function

2 Restful形式的URL
HTTP请求方式
GET
POST添加
PUT修改
 DELETE删除
符合规则的URL地址
http：//www.hello.com/books GET
http：//www.hello.com/books POST
http：//www.hellocom/books/123 PUT
http：//www.hellocom/books/123 DELETE
```
### 21.3 Promise概述
```markdown
Promise是异步编程的种解决方案，从语法上讲，Promise是个对象，从它可以获取异步操作的消息。
使用Promise主要有以下好处
可以避免多层异步调用嵌套问题(回调地狱)
Promise对象提供了简洁的APl，使得控制异步操作更加容易
```

#### 21.3.1 异步编程与Promise概述
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div>前后端交互</div>
  <script type="text/javascript" src="js/jquery.js"></script>
  <script type="text/javascript">
    /*
      前后端交互-异步编程与Promise概述
    */
    // var ret = '---';
    // $.ajax({
    //   url: 'http://localhost:3000/data',
    //   success: function(data) {
    //     ret = data;
    //     console.log(ret)
    //   }
    // });
    // console.log(ret)

    // ----------------------------
    // $.ajax({
    //   url: 'http://localhost:3000/data',
    //   success: function(data) {
    //     console.log(data)
    //   }
    // });
    // $.ajax({
    //   url: 'http://localhost:3000/data1',
    //   success: function(data) {
    //     console.log(data)
    //   }
    // });
    // $.ajax({
    //   url: 'http://localhost:3000/data2',
    //   success: function(data) {
    //     console.log(data)
    //   }
    // });
    // -----------------------------------
    $.ajax({
      url: 'http://localhost:3000/data',
      success: function(data) {
        console.log(data)
        $.ajax({
          url: 'http://localhost:3000/data1',
          success: function(data) {
            console.log(data)
            $.ajax({
              url: 'http://localhost:3000/data2',
              success: function(data) {
                console.log(data)
              }
            });
          }
        });
      }
    });
    
    
    
  </script>
</body>
</html>

```
#### 21.3.2 Promise基本用法
```markdown
实例化 Promise对象，构造函数中传递函数，该函数中用于处理异步任务
resolve和 reject两个参数用于处理成功和失败两种情况，并邇过p.then获取处理结果
var p= new Promise(function(resolve,reject) {
	//成功时调用resolve()
	//失败时调用reject()
});
p.then(funciton(ret){
	//从 resolve得到常结果
},function(ret){
	//从 re]ect得到错措误信息
});
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
    /*
      Promise基本使用
    */
    // console.log(typeof Promise)
    // console.dir(Promise);

    var p = new Promise(function(resolve, reject){
      // 这里用于实现异步任务
      setTimeout(function(){
        var flag = false;
        if(flag) {
          // 正常情况
          resolve('hello');
        }else{
          // 异常情况
          reject('出错了');
        }
      }, 100);
    });
    p.then(function(data){
      console.log(data)
    },function(info){
      console.log(info)
    });
  </script>
</body>
</html>
```
#### 21.3.3 基于Promise发送Ajax请求并解决回调地狱问题
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  
  <script type="text/javascript">
    /*
      基于Promise发送Ajax请求
    */
    function queryData(url) {
      var p = new Promise(function(resolve, reject){
        var xhr = new XMLHttpRequest();
        xhr.onreadystatechange = function(){
          if(xhr.readyState != 4) return;
          if(xhr.readyState == 4 && xhr.status == 200) {
            // 处理正常的情况
            resolve(xhr.responseText);
          }else{
            // 处理异常情况
            reject('服务器错误');
          }
        };
        xhr.open('get', url);
        xhr.send(null);
      });
      return p;
    }
    // queryData('http://localhost:3000/data')
    //   .then(function(data){
    //     console.log(data);
    //   },function(info){
    //     console.log(info)
    //   });
    // ============================
    // 发送多个ajax请求并且保证顺序
    queryData('http://localhost:3000/data')
      .then(function(data){
        console.log(data)
        return queryData('http://localhost:3000/data1');
      })
      .then(function(data){
        console.log(data);
        return queryData('http://localhost:3000/data2');
      })
      .then(function(data){
        console.log(data)
      });
  </script>
</body>
</html>
```
#### 21.3.4 then参数中的函数返回值
```markdown
1.返回 Promise实例对象
返回的该实例对象会调用下一个then
2.返回普通值
返回的普通值会直接传递给下一个then，过then参数中函数的参数接收该值
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
    /*
      then参数中的函数返回值
    */
    function queryData(url) {
      return new Promise(function(resolve, reject){
        var xhr = new XMLHttpRequest();
        xhr.onreadystatechange = function(){
          if(xhr.readyState != 4) return;
          if(xhr.readyState == 4 && xhr.status == 200) {
            // 处理正常的情况
            resolve(xhr.responseText);
          }else{
            // 处理异常情况
            reject('服务器错误');
          }
        };
        xhr.open('get', url);
        xhr.send(null);
      });
    }
    queryData('http://localhost:3000/data')
      .then(function(data){
        return queryData('http://localhost:3000/data1');
      })
      .then(function(data){
        return new Promise(function(resolve, reject){
          setTimeout(function(){
            resolve(123);
          },1000)
        });
      })
      .then(function(data){
        return 'hello';
      })
      .then(function(data){
        console.log(data)
      })

  </script>
</body>
</html>
```
#### 21.3.5 Promise常用API-实例方法
```markdown
p.then()得到异步任务的正确结果
p.catch()获取异常信息
p.finally()成功与否都会执行(尚且不是正式标准)
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
    /*
      Promise常用API-实例方法
    */
    // console.dir(Promise);
    function foo() {
      return new Promise(function(resolve, reject){
        setTimeout(function(){
          // resolve(123);
          reject('error');
        }, 100);
      })
    }
    // foo()
    //   .then(function(data){
    //     console.log(data)
    //   })
    //   .catch(function(data){
    //     console.log(data)
    //   })
    //   .finally(function(){
    //     console.log('finished')
    //   });

    // --------------------------
    // 两种写法是等效的
    foo()
      .then(function(data){
        console.log(data)
      },function(data){
        console.log(data)
      })
      .finally(function(){
        console.log('finished')
      });
  </script>
</body>
</html>
```
#### 21.3.6 Promise常用API-对象方法
```markdown
Promise.all()并发处理多个异步任务，所有任务都执行完成
才能得到结果
Promise.race()并发处理多个异步任务，只要有一个任务完成
就能得到结果
Promise.all（[pl， p2， p31]）. then（（result）=> {
	console.log （result）
Promise.race（[p1， p2， p3）]）. then（（result => {
console. log（result）
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
  
  <script type="text/javascript">
    /*
      Promise常用API-对象方法
    */
    // console.dir(Promise)
    function queryData(url) {
      return new Promise(function(resolve, reject){
        var xhr = new XMLHttpRequest();
        xhr.onreadystatechange = function(){
          if(xhr.readyState != 4) return;
          if(xhr.readyState == 4 && xhr.status == 200) {
            // 处理正常的情况
            resolve(xhr.responseText);
          }else{
            // 处理异常情况
            reject('服务器错误');
          }
        };
        xhr.open('get', url);
        xhr.send(null);
      });
    }

    var p1 = queryData('http://localhost:3000/a1');
    var p2 = queryData('http://localhost:3000/a2');
    var p3 = queryData('http://localhost:3000/a3');
    // Promise.all([p1,p2,p3]).then(function(result){
    //   console.log(result)
    // })
    Promise.race([p1,p2,p3]).then(function(result){
      console.log(result)
    })
  </script>
</body>
</html>
```
### 21.4 FetchAPI基本使用
```markdown
基本特性
更加简单的数据获取方式，功能更强大、更灵活，可以看做
是xhr的升级版,基于 Promise实现

fetch('/abc'). then(data =>
	return data.text();
}). then（ret=>
	//注意这里得到的才是最终的数据
	console.log(ret);
});
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
    /*
      Fetch API 基本用法
    */
    fetch('http://localhost:3000/fdata').then(function(data){
      // text()方法属于fetchAPI的一部分，它返回一个Promise实例对象，用于获取后台返回的数据
      return data.text();
    }).then(function(data){
      console.log(data);
    })
  </script>
</body>
</html>
```
#### 21.4.1 FetchAPI参数传递
```markdown
1.常用配置选项
method(String):HTTP请求方法，默认为
GET(GET、POST、PUT、DELETE)
body(String):HTTP的请求参数
headers(Object)： Http的请求头，默认为{}
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
    /*
      Fetch API 调用接口传递参数
    */

    // GET参数传递-传统URL
    // fetch('http://localhost:3000/books?id=123', {
    //   method: 'get'
    // })
    //   .then(function(data){
    //     return data.text();
    //   }).then(function(data){
    //     console.log(data)
    //   });

    // GET参数传递-restful形式的URL
    // fetch('http://localhost:3000/books/456', {
    //   method: 'get'
    // })
    //   .then(function(data){
    //     return data.text();
    //   }).then(function(data){
    //     console.log(data)
    //   });

    // DELETE请求方式参数传递
    // fetch('http://localhost:3000/books/789', {
    //   method: 'delete'
    // })
    //   .then(function(data){
    //     return data.text();
    //   }).then(function(data){
    //     console.log(data)
    //   });

    // POST请求传参
    // fetch('http://localhost:3000/books', {
    //   method: 'post',
    //   body: 'uname=lisi&pwd=123',
    //   headers: {
    //     'Content-Type': 'application/x-www-form-urlencoded'
    //   }
    // })
    //   .then(function(data){
    //     return data.text();
    //   }).then(function(data){
    //     console.log(data)
    //   });

    // POST请求传参
    // fetch('http://localhost:3000/books', {
    //   method: 'post',
    //   body: JSON.stringify({
    //     uname: '张三',
    //     pwd: '456'
    //   }),
    //   headers: {
    //     'Content-Type': 'application/json'
    //   }
    // })
    //   .then(function(data){
    //     return data.text();
    //   }).then(function(data){
    //     console.log(data)
    //   });

    // PUT请求传参
    fetch('http://localhost:3000/books/123', {
      method: 'put',
      body: JSON.stringify({
        uname: '张三',
        pwd: '789'
      }),
      headers: {
        'Content-Type': 'application/json'
      }
    })
      .then(function(data){
        return data.text();
      }).then(function(data){
        console.log(data)
      });
  </script>
</body>
</html>
```
#### 21.4.2 Fetch响应结果的数据格式
```markdown
text(): 将返回体处理成字符串类型
json(): 返回结果和JSON.parse(responseText)一样
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
    /*
      Fetch响应结果的数据格式
    */
    fetch('http://localhost:3000/json').then(function(data){
      // return data.json(); 返回结果和JSON.parse(responseText)一样
      return data.text();
    }).then(function(data){
      // console.log(data.uname)
      // console.log(typeof data)
      var obj = JSON.parse(data);
      console.log(obj.uname,obj.age,obj.gender)
    })
  </script>
</body>
</html>
```
### 21.5 axios基本用法
```markdown
基于promise用于浏览器和node.js的http客户端
支持浏览器和node.js
支持promise
能拦截请求和响应
自动转换JSON数据
能转换请求和响应数据
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  
  <script type="text/javascript" src="js/axios.js"></script>
  <script type="text/javascript">
    axios.get('http://localhost:3000/adata').then(function(ret){
      // 注意data属性是固定的用法，用于获取后台的实际数据
      // console.log(ret.data)
      console.log(ret)
    })
  </script>
</body>
</html>
```

#### 21.5.1 axios请求传参
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  
  <script type="text/javascript" src="js/axios.js"></script>
  <script type="text/javascript">
    /*
      axios请求参数传递
    */
    // axios get请求传参
    // axios.get('http://localhost:3000/axios?id=123').then(function(ret){
    //   console.log(ret.data)
    // })
    // axios.get('http://localhost:3000/axios/123').then(function(ret){
    //   console.log(ret.data)
    // })
    // axios.get('http://localhost:3000/axios', {
    //   params: {
    //     id: 789
    //   }
    // }).then(function(ret){
    //   console.log(ret.data)
    // })

    // axios delete 请求传参
    // axios.delete('http://localhost:3000/axios', {
    //   params: {
    //     id: 111
    //   }
    // }).then(function(ret){
    //   console.log(ret.data)
    // })

    // axios.post('http://localhost:3000/axios', {
    //   uname: 'lisi',
    //   pwd: 123
    // }).then(function(ret){
    //   console.log(ret.data)
    // })
    // var params = new URLSearchParams();
    // params.append('uname', 'zhangsan');
    // params.append('pwd', '111');
    // axios.post('http://localhost:3000/axios', params).then(function(ret){
    //   console.log(ret.data)
    // })

    // axios put 请求传参
    axios.put('http://localhost:3000/axios/123', {
      uname: 'lisi',
      pwd: 123
    }).then(function(ret){
      console.log(ret.data)
    })

    

  </script>
</body>
</html>
```
#### 21.5.2 axios响应结果与全局配置
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  
  <script type="text/javascript" src="js/axios.js"></script>
  <script type="text/javascript">
    /*
      axios 响应结果与全局配置
    */
    // axios.get('http://localhost:3000/axios-json').then(function(ret){
    //   console.log(ret.data.uname)
    // })

    // 配置请求的基准URL地址
    axios.defaults.baseURL = 'http://localhost:3000/';
    // 配置请求头信息
    axios.defaults.headers['mytoken'] = 'hello';
    axios.get('axios-json').then(function(ret){
      console.log(ret.data.uname)
    })


  </script>
</body>
</html>
```
#### 21.5.3 axios并发请求
```markdown
并发请求:  将多个请求在同一时刻发送到后端服务接口,最
后在集中处理每个请求的响应结果
```
```js
 //1.创建一个查询所有请求
    function findAll(){
        return axios.get("http://localhost:8989/user/findAll?name=xiaochen");
    }

    //2.创建一个保存的请求
    function save(){
        return axios.post("http://localhost:8989/user/save",{
            username:"xiaochen",
            age:23,
            email:"xiaochen@zparkhr.com",
            phone:13260426185
        });
    }

    //3.并发执行
    axios.all([findAll(),save()]).then(
        axios.spread(function(res1,res2){  //用来将一组函数的响应结果汇总处理
            console.log(res1.data);
            console.log(res2.data);
        })
    );//用来发送一组并发请求
```


#### 21.5.4 axios拦截器
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200824110104862.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200824110137327.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  
  <script type="text/javascript" src="js/axios.js"></script>
  <script type="text/javascript">
    /*
      axios拦截器
    */
    axios.interceptors.request.use(function(config) {
      console.log(config.url)
      config.headers.mytoken = 'nihao';
      return config;
    }, function(err){
      console.log(err)
    })

    axios.interceptors.response.use(function(res) {
      // console.log(res)
      var data = res.data;
      return data;
    }, function(err){
      console.log(err)
    })
    axios.get('http://localhost:3000/adata').then(function(data){
      console.log(data)
    })
  </script>
</body>
</html>
```


### 21.6 async函数
#### 21.6.1 async/await函数基本用法
```markdown
async/await是ES7引入的新语法，可以更加方便的进行异步操作
async关键字用于函数上（async函数的返回值是Promise实例对象）
await关键字用于async函数当中(await可以得到异步的结果)
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  
  <script type="text/javascript" src="js/axios.js"></script>
  <script type="text/javascript">
    /*
      async/await 处理异步操作：
      async函数返回一个Promise实例对象
      await后面可以直接跟一个 Promise实例对象
    */
    axios.defaults.baseURL = 'http:localhost:3000';
    // axios.get('adata').then(function(ret){
    //   console.log(ret.data)
    // })

    // async function queryData() {
    //   var ret = await axios.get('adata');
    //   // console.log(ret.data)
    //   return ret.data;
    // }

    async function queryData() {
      var ret = await new Promise(function(resolve, reject){
        setTimeout(function(){
          resolve('nihao')
        },1000);
      })
      // console.log(ret.data)
      return ret;
    }
    queryData().then(function(data){
      console.log(data)
    })
  </script>
</body>
</html>
```
#### 21.6.2 async函数处理多个异步请求
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  
  <script type="text/javascript" src="js/axios.js"></script>
  <script type="text/javascript">
    /*
      async/await处理多个异步任务
    */
    axios.defaults.baseURL = 'http://localhost:3000';

    async function queryData() {
      var info = await axios.get('async1');
      var ret = await axios.get('async2?info=' + info.data);
      return ret.data;
    }

    queryData().then(function(data){
      console.log(data)
    })
  </script>
</body>
</html>
```
#### 21.6.3 基于接口的案例
##### 21.6.3.1 图书管理
```markdown
图书相关的操作基于后台接口数据进行操作
需要调用接口的功能点
图书列表数据加载 GET http://localhost3000/books
添加图书 POSTt http://localhost:3000/books
验证图书名称是否存在 GET http://localhost:3000/books/book/:name
编辑图书-根据|D查询图书信息 GET http：/localhost：3000books/:id
编辑图书-提交图书信息 PUT http://localhost:3000/books/:id
删除图书 DELETE http:/localhost:3000/books/:id
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020082507301133.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <link rel="stylesheet" type="text/css" href="css/index.css">
</head>
<body>
  <div id="app">
    <div class="grid">
      <div>
        <h1>图书管理</h1>
        <div class="book">
          <div>
            <label for="id">
              编号：
            </label>
            <input type="text" id="id" v-model='id' disabled="false" v-focus>
            <label for="name">
              名称：
            </label>
            <input type="text" id="name" v-model='name'>
            <button @click='handle' :disabled="submitFlag">提交</button>
          </div>
        </div>
      </div>
      <div class="total">
        <span>图书总数：</span>
        <span>{{total}}</span>
      </div>
      <table>
        <thead>
          <tr>
            <th>编号</th>
            <th>名称</th>
            <th>时间</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          <tr :key='item.id' v-for='item in books'>
            <td>{{item.id}}</td>
            <td>{{item.name}}</td>
            <td>{{item.date | format('yyyy-MM-dd hh:mm:ss')}}</td>
            <td>
              <a href="" @click.prevent='toEdit(item.id)'>修改</a>
              <span>|</span>
              <a href="" @click.prevent='deleteBook(item.id)'>删除</a>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript" src="js/axios.js"></script>
  <script type="text/javascript">
    /*
      图书管理-添加图书
    */
    axios.defaults.baseURL = 'http://localhost:3000/';
    axios.interceptors.response.use(function(res){
      return res.data;
    }, function(error){
      console.log(error)
    });
    Vue.directive('focus', {
      inserted: function (el) {
        el.focus();
      }
    });
    Vue.filter('format', function(value, arg) {
      function dateFormat(date, format) {
        if (typeof date === "string") {
          var mts = date.match(/(\/Date\((\d+)\)\/)/);
          if (mts && mts.length >= 3) {
            date = parseInt(mts[2]);
          }
        }
        date = new Date(date);
        if (!date || date.toUTCString() == "Invalid Date") {
          return "";
        }
        var map = {
          "M": date.getMonth() + 1, //月份 
          "d": date.getDate(), //日 
          "h": date.getHours(), //小时 
          "m": date.getMinutes(), //分 
          "s": date.getSeconds(), //秒 
          "q": Math.floor((date.getMonth() + 3) / 3), //季度 
          "S": date.getMilliseconds() //毫秒 
        };
        format = format.replace(/([yMdhmsqS])+/g, function(all, t) {
          var v = map[t];
          if (v !== undefined) {
            if (all.length > 1) {
              v = '0' + v;
              v = v.substr(v.length - 2);
            }
            return v;
          } else if (t === 'y') {
            return (date.getFullYear() + '').substr(4 - all.length);
          }
          return all;
        });
        return format;
      }
      return dateFormat(value, arg);
    })
    var vm = new Vue({
      el: '#app',
      data: {
        flag: false,
        submitFlag: false,
        id: '',
        name: '',
        books: []
      },
      methods: {
        handle: async function(){
          if(this.flag) {
            // 编辑图书
            var ret = await axios.put('books/' + this.id, {
              name: this.name
            });
            if(ret.status == 200){
              // 重新加载列表数据
              this.queryData();
            }
            this.flag = false;
          }else{
            // 添加图书
            var ret = await axios.post('books', {
              name: this.name
            })
            if(ret.status == 200) {
              // 重新加载列表数据
              this.queryData();
            }
          }
          // 清空表单
          this.id = '';
          this.name = '';
        },
        toEdit: async function(id){
          // flag状态位用于区分编辑和添加操作
          this.flag = true;
          // 根据id查询出对应的图书信息
          var ret = await axios.get('books/' + id);
          this.id = ret.id;
          this.name = ret.name;
        },
        deleteBook: async function(id){
          // 删除图书
          var ret = await axios.delete('books/' + id);
          if(ret.status == 200) {
            // 重新加载列表数据
            this.queryData();
          }
        },
        queryData: async function(){
          // 调用后台接口获取图书列表数据
          // var ret = await axios.get('books');
          // this.books = ret.data;

          this.books = await axios.get('books');
        }
      },
      computed: {
        total: function(){
          // 计算图书的总数
          return this.books.length;
        }
      },
      watch: {
        name: async function(val) {
          // 验证图书名称是否已经存在
          // var flag = this.books.some(function(item){
          //   return item.name == val;
          // });
          var ret = await axios.get('/books/book/' + this.name);
          if(ret.status == 1) {
            // 图书名称存在
            this.submitFlag = true;
          }else{
            // 图书名称不存在
            this.submitFlag = false;
          }
        }
      },
      mounted: function(){
        // var that = this;
        // axios.get('books').then(function(data){
        //   console.log(data.data)
        //   that.books = data.data;
        // })

        // axios.get('books').then((data)=>{
        //   console.log(data.data)
        //   this.books = data.data;
        // })

        this.queryData();
      }
    });
  </script>
</body>
</html>

```
## 22 路由
```markdown
路由是一个比较广义和抽象的概念，路由的本质就是对应关系。
在开发中，路由分为：
后端路由
前端路由
```
### 22.1 后端路由
```markdown
概念：根据不同的用户 URL 请求，返回不同的内容
本质：URL 请求地址与服务器资源之间的对应关系
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200822231239916.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
SPA（Single Page Application）
```
```markdown
后端渲染（存在性能问题）
Ajax前端渲染（前端渲染提高性能，但是不支持浏览器的前
进后退操作）
SPA（Single Page Application）单页面应用程序：整个网站
只有一个页面，内容的变化通过Ajax局部更新实现、同时支持
浏览器地址栏的前进和后退操作
SPA实现原理之一：基于URL地址的hash（hash的变化会导致
浏览器记录访问历
史的变化、但是hash的变化不会触发新的URL请求）
在实现SPA过程中，最核心的技术点就是前端路由
```
### 22.2 前端路由
```markdown
概念：根据不同的用户事件，显示不同的页面内容
本质：用户事件与事件处理函数之间的对应关系
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200822232738117.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

#### 22.2.1 模拟路由
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210106202347172.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
// 监听 window 的 onhashchange 事件，根据获取到的最新的 hash 值，切换要显示的组件的名称
window.onhashchange = function() {
 // 通过 location.hash 获取到最新的 hash 值
}
```
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <!-- 导入 vue 文件 -->
    <script src="./lib/vue_2.5.22.js"></script>
  </head>
  <body>
    <!-- 被 vue 实例控制的 div 区域 -->
    <div id="app">
      <!-- 切换组件的超链接 -->
      <a href="#/zhuye">主页</a> 
      <a href="#/keji">科技</a> 
      <a href="#/caijing">财经</a>
      <a href="#/yule">娱乐</a>

      <!-- 根据 :is 属性指定的组件名称，把对应的组件渲染到 component 标签所在的位置 -->
      <!-- 可以把 component 标签当做是【组件的占位符】 -->
      <component :is="comName"></component>
    </div>

    <script>
      // #region 定义需要被切换的 4 个组件
      // 主页组件
      const zhuye = {
        template: '<h1>主页信息</h1>'
      }

      // 科技组件
      const keji = {
        template: '<h1>科技信息</h1>'
      }

      // 财经组件
      const caijing = {
        template: '<h1>财经信息</h1>'
      }

      // 娱乐组件
      const yule = {
        template: '<h1>娱乐信息</h1>'
      }
      // #endregion

      // #region vue 实例对象
      const vm = new Vue({
        el: '#app',
        data: {
          comName: 'zhuye'
        },
        // 注册私有组件
        components: {
          zhuye,
          keji,
          caijing,
          yule
        }
      })
      // #endregion

      // 监听 window 的 onhashchange 事件，根据获取到的最新的 hash 值，切换要显示的组件的名称
      window.onhashchange = function() {
        // 通过 location.hash 获取到最新的 hash 值
        console.log(location.hash);
        switch(location.hash.slice(1)){
          case '/zhuye':
            vm.comName = 'zhuye'
          break
          case '/keji':
            vm.comName = 'keji'
          break
          case '/caijing':
            vm.comName = 'caijing'
          break
          case '/yule':
            vm.comName = 'yule'
          break
        }
      }
    </script>
  </body>
</html>

```

#### 22.2.2 Vue Router
```markdown
Vue Router（官网：https://router.vuejs.org/zh/）
是 Vue.js 官方的路由管理器。
它和 Vue.js 的核心深度集成，可以非常方便的用
于SPA应用程序的开发。


支持HTML5 历史模式或 hash 模式
支持嵌套路由
支持路由参数
支持编程式路由
支持命名路由
```


##### 22.2.2.1 vue-router的基本使用
```html
1. 引入相关的库文件
<!-- 导入 vue 文件，为全局 window 对象挂载 Vue 构造函数 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<!-- 导入 vue-router 文件，为全局 window 对象挂载 VueRouter 构造函数 -->
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>  //vue 路由js
2. 添加路由链接
<!-- router-link 是 vue 中提供的标签，默认会被渲染为 a 标签 -->
<!-- to 属性默认会被渲染为 href 属性 -->
<!-- to 属性的值默认会被渲染为 # 开头的 hash 地址 -->
<router-link to="/user">User</router-link>
<router-link to="/register">Register</router-link>

3. 添加路由填充位
<!-- 路由填充位（也叫做路由占位符） -->
<!-- 将来通过路由规则匹配到的组件，将会被渲染到 router-view 所在的位置 -->
<router-view></router-view>

4. 定义路由组件
 var User = {
 template: '<div>User</div>'
 }
 var Register = {
 template: '<div>Register</div>'
 }

5. 配置路由规则并创建路由实例
 // 创建路由实例对象
 var router = new VueRouter({
 // routes 是路由规则数组
 routes: [
 // 每个路由规则都是一个配置对象，其中至少包含 path 和 component 两个属性：
 // path 表示当前路由规则匹配的 hash 地址
 // component 表示当前路由规则对应要展示的组件
 {path:'/user',component: User},
 {path:'/register',component: Register}
 ]
 })

6. 把路由挂载到 Vue 根实例中
new Vue({
 el: '#app',
 // 为了能够让路由规则生效，必须把路由对象挂载到 vue 实例对象上
 router
 });
```
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <!-- 导入 vue 文件 -->
    <script src="./lib/vue_2.5.22.js"></script>
    <script src="./lib/vue-router_3.0.2.js"></script>
  </head>
  <body>
    <!-- 被 vm 实例所控制的区域 -->
    <div id="app">
      <router-link to="/user">User</router-link>
      <router-link to="/register">Register</router-link>

      <!-- 路由占位符 -->
      <router-view></router-view>
    </div>

    <script>
      const User = {
        template: '<h1>User 组件</h1>'
      }

      const Register = {
        template: '<h1>Register 组件</h1>'
      }

      // 创建路由实例对象
      const router = new VueRouter({
        // 所有的路由规则
        routes: [
          { path: '/user', component: User },
          { path: '/register', component: Register }
        ]
      })

      // 创建 vm 实例对象
      const vm = new Vue({
        // 指定控制的区域
        el: '#app',
        data: {},
        // 挂载路由实例对象
        // router: router
        router
      })
    </script>
  </body>
</html>

```
##### 22.2.2.2 router-link使用
```markdown
作用:用来替换我们在切换路由时使用a标签切换路由

好处:就是可以自动给路由路径加入#不需要手动加入
```
```html
<router-link to="/login" tag="button">我要登录</router-link>
<router-link to="/register" tag="button">点我注册</router-link>
```

```markdown
# 总结:
	1.router-link 用来替换使用a标签实现路由切换 好处是不需要书写#号直接书写路由路径
	2.router-link to属性用来书写路由路径   tag属性:用来将router-link渲染成指定的标签
```

##### 22.2.2.3 路由重定向
```markdown
路由重定向指的是：用户在访问地址 A 的时候，强制用户跳转到地址 C ，从而展示特定的组件页面；
通过路由规则的 redirect 属性，指定一个新的路由地址，可以很方便地设置路由的重定向：

var router = new VueRouter({
 routes: [
 // 其中，path 表示需要被重定向的原地址，redirect 表示将要被重定向到的新地址
 {path:'/', redirect: '/user'},
 {path:'/user',component: User},
 {path:'/register',component: Register}
 ]
 })
```
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <!-- 导入 vue 文件 -->
    <script src="./lib/vue_2.5.22.js"></script>
    <script src="./lib/vue-router_3.0.2.js"></script>
  </head>
  <body>
    <!-- 被 vm 实例所控制的区域 -->
    <div id="app">
      <router-link to="/user">User</router-link>
      <router-link to="/register">Register</router-link>

      <!-- 路由占位符 -->
      <router-view></router-view>
    </div>

    <script>
      const User = {
        template: '<h1>User 组件</h1>'
      }

      const Register = {
        template: '<h1>Register 组件</h1>'
      }

      // 创建路由实例对象
      const router = new VueRouter({
        // 所有的路由规则
        routes: [
          { path: '/', redirect: '/user'},
          { path: '/user', component: User },
          { path: '/register', component: Register }
        ]
      })

      // 创建 vm 实例对象
      const vm = new Vue({
        // 指定控制的区域
        el: '#app',
        data: {},
        // 挂载路由实例对象
        // router: router
        router
      })
    </script>
  </body>
</html>

```
##### 22.2.2.4 路由中参数传递      
###### 22.2.2.4.1 第一种方式传递参数-传统方式                                                            
```js
1. 通过?号形式拼接参数
    <router-link to="/login?id=21&name=zhangsan">我要登录</router-link>

2. 组件中获取参数
  const login = {
     template:'<h1>用户登录</h1>',
     data(){return {}},
     methods:{},
     created(){
       console.log("=============>"+this.$route.query.id+"======>"+this.$route.query.name);
     }
   };

```
###### 22.2.2.4.2 第二种方式传递参数 restful
```js
1. 通过使用路径方式传递参数
   <router-link to="/register/24/张三">我要注册</router-link>
   var router = new VueRouter({
     routes:[
       {path:'/register/:id/:name',component:register}   //定义路径中获取对应参数
     ]
   });

2. 组件中获取参数
   const register = {
     template:'<h1>用户注册{{ $route.params.name }}</h1>',
     created(){
       console.log("注册组件中id:   "+this.$route.params.id+this.$route.params.name);
     }
   };

```
##### 22.2.2.5 嵌套路由用法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210106204247133.png)

```html
1. 嵌套路由功能分析
点击父级路由链接显示模板内容
模板内容中又有子级路由链接
点击子级路由链接显示子级模板内容

2. 父路由组件模板
父级路由链接
 父组件路由填充位
<p>
 <router-link to="/user">User</router-link>
 <router-link to="/register">Register</router-link>
 </p>
 <div>
 <!-- 控制组件的显示位置 -->
 <router-view></router-view>
 </div>
3. 子级路由模板
子级路由链接
子级路由填充位

const Register = {
 template: `<div>
 <h1>Register 组件</h1>
 <hr/>
 <router-link to="/register/tab1">Tab1</router-link>
 <router-link to="/register/tab2">Tab2</router-link>
 <!-- 子路由填充位置 -->
 <router-view/>
 </div>`
 }
4. 嵌套路由配置
父级路由通过children属性配置子级路由
const router = new VueRouter({
 routes: [
 { path: '/user', component: User },
 {
 path: '/register',
 component: Register,
 // 通过 children 属性，为 /register 添加子路由规则
 children: [
 { path: '/register/tab1', component: Tab1 },
 { path: '/register/tab2', component: Tab2 }
 ]
 }
 ]
 })
```
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <!-- 导入 vue 文件 -->
    <script src="./lib/vue_2.5.22.js"></script>
    <script src="./lib/vue-router_3.0.2.js"></script>
  </head>
  <body>
    <!-- 被 vm 实例所控制的区域 -->
    <div id="app">
      <router-link to="/user">User</router-link>
      <router-link to="/register">Register</router-link>

      <!-- 路由占位符 -->
      <router-view></router-view>
    </div>

    <script>
      const User = {
        template: '<h1>User 组件</h1>'
      }

      const Register = {
        template: `<div>
          <h1>Register 组件</h1>
          <hr/>

          <!-- 子路由链接 -->
          <router-link to="/register/tab1">tab1</router-link>
          <router-link to="/register/tab2">tab2</router-link>

          <!-- 子路由的占位符 -->
          <router-view />
        <div>`
      }

      const Tab1 = {
        template: '<h3>tab1 子组件</h3>'
      }

      const Tab2 = {
        template: '<h3>tab2 子组件</h3>'
      }

      // 创建路由实例对象
      const router = new VueRouter({
        // 所有的路由规则
        routes: [
          { path: '/', redirect: '/user'},
          { path: '/user', component: User },
          // children 数组表示子路由规则
          { path: '/register', component: Register, children: [
            { path: '/register/tab1', component: Tab1 },
            { path: '/register/tab2', component: Tab2 }
          ] }
        ]
      })

      // 创建 vm 实例对象
      const vm = new Vue({
        // 指定控制的区域
        el: '#app',
        data: {},
        // 挂载路由实例对象
        // router: router
        router
      })
    </script>
  </body>
</html>

```
##### 22.2.2.6 vue-router动态路由匹配

```html
<!– 有如下 3 个路由链接 -->
<router-link to="/user/1">User1</router-link>
<router-link to="/user/2">User2</router-link>
<router-link to="/user/3">User3</router-link>

// 定义如下三个对应的路由规则，是否可行？？？不行。重复代码多。可以简写
{ path: '/user/1', component: User }
{ path: '/user/2', component: User }
{ path: '/user/3', component: User }

应用场景：通过动态路由参数的模式进行路由匹配
var router = new VueRouter({
 routes: [
 // 动态路径参数 以冒号开头
 { path: '/user/:id', component: User }
 ]
})

const User = {
 // 路由组件中通过$route.params获取路由参数
 template: '<div>User {{ $route.params.id }}</div>'
}

```
###### 22.2.2.6.1 动态路由匹配1
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <!-- 导入 vue 文件 -->
    <script src="./lib/vue_2.5.22.js"></script>
    <script src="./lib/vue-router_3.0.2.js"></script>
  </head>
  <body>
    <!-- 被 vm 实例所控制的区域 -->
    <div id="app">
      <router-link to="/user/1">User1</router-link>
      <router-link to="/user/2">User2</router-link>
      <router-link to="/user/3">User3</router-link>
      <router-link to="/register">Register</router-link>

      <!-- 路由占位符 -->
      <router-view></router-view>
    </div>

    <script>
      const User = {
        template: '<h1>User 组件 -- 用户id为: {{$route.params.id}}</h1>'
      }

      const Register = {
        template: '<h1>Register 组件</h1>'
      }

      // 创建路由实例对象
      const router = new VueRouter({
        // 所有的路由规则
        routes: [
          { path: '/', redirect: '/user'},
          { path: '/user/:id', component: User },
          { path: '/register', component: Register }
        ]
      })

      // 创建 vm 实例对象
      const vm = new Vue({
        // 指定控制的区域
        el: '#app',
        data: {},
        // 挂载路由实例对象
        // router: router
        router
      })
    </script>
  </body>
</html>

```
###### 22.2.2.6.2 动态路由匹配2
```markdown
路由组件传递参数
$route与对应路由形成高度耦合，不够灵活，
所以可以使用props将组件和路由解耦
```
```markdown
1.props的值为布尔类型
```
```markdown
const router = new VueRouter({
 routes: [
 // 如果 props 被设置为 true，route.params 将会被设置为组件属性
 { path: '/user/:id', component: User, props: true }
 ]
 })
const User = {
 props: ['id'], // 使用 props 接收路由参数
 template: '<div>用户ID：{{ id }}</div>' // 使用路由参数
 }
```
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <!-- 导入 vue 文件 -->
    <script src="./lib/vue_2.5.22.js"></script>
    <script src="./lib/vue-router_3.0.2.js"></script>
  </head>
  <body>
    <!-- 被 vm 实例所控制的区域 -->
    <div id="app">
      <router-link to="/user/1">User1</router-link>
      <router-link to="/user/2">User2</router-link>
      <router-link to="/user/3">User3</router-link>
      <router-link to="/register">Register</router-link>

      <!-- 路由占位符 -->
      <router-view></router-view>
    </div>

    <script>
      const User = {
        props: ['id'],
        template: '<h1>User 组件 -- 用户id为: {{id}}</h1>'
      }

      const Register = {
        template: '<h1>Register 组件</h1>'
      }

      // 创建路由实例对象
      const router = new VueRouter({
        // 所有的路由规则
        routes: [
          { path: '/', redirect: '/user'},
          { path: '/user/:id', component: User, props: true },
          { path: '/register', component: Register }
        ]
      })

      // 创建 vm 实例对象
      const vm = new Vue({
        // 指定控制的区域
        el: '#app',
        data: {},
        // 挂载路由实例对象
        // router: router
        router
      })
    </script>
  </body>
</html>

```
```markdown
2.props的值为对象类型
```
```markdown
 const router = new VueRouter({
 routes: [
 // 如果 props 是一个对象，它会被按原样设置为组件属性
 { path: '/user/:id', component: User, props: { uname: 'lisi', age: 12 }}
 ]
 })
const User = {
 props: ['uname', 'age'],
 template: ‘<div>用户信息：{{ uname + '---' + age}}</div>'
 }
```
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <!-- 导入 vue 文件 -->
    <script src="./lib/vue_2.5.22.js"></script>
    <script src="./lib/vue-router_3.0.2.js"></script>
  </head>
  <body>
    <!-- 被 vm 实例所控制的区域 -->
    <div id="app">
      <router-link to="/user/1">User1</router-link>
      <router-link to="/user/2">User2</router-link>
      <router-link to="/user/3">User3</router-link>
      <router-link to="/register">Register</router-link>

      <!-- 路由占位符 -->
      <router-view></router-view>
    </div>

    <script>
      const User = {
        props: ['id', 'uname', 'age'],
        template: '<h1>User 组件 -- 用户id为: {{id}} -- 姓名为:{{uname}} -- 年龄为：{{age}}</h1>'
      }

      const Register = {
        template: '<h1>Register 组件</h1>'
      }

      // 创建路由实例对象
      const router = new VueRouter({
        // 所有的路由规则
        routes: [
          { path: '/', redirect: '/user'},
          { path: '/user/:id', component: User, props: { uname: 'lisi', age: 20 } },
          { path: '/register', component: Register }
        ]
      })

      // 创建 vm 实例对象
      const vm = new Vue({
        // 指定控制的区域
        el: '#app',
        data: {},
        // 挂载路由实例对象
        // router: router
        router
      })
    </script>
  </body>
</html>

```
```markdown
3.props的值为函数类型
```
```markdown
const router = new VueRouter({
 routes: [
 // 如果 props 是一个函数，则这个函数接收 route 对象为自己的形参
 { path: '/user/:id',
 component: User,
 props: route => ({ uname: 'zs', age: 20, id: route.params.id })}
 ]
 })
const User = {
 props: ['uname', 'age', 'id'],
 template: ‘<div>用户信息：{{ uname + '---' + age + '---' + id}}</div>'
 }
```
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <!-- 导入 vue 文件 -->
    <script src="./lib/vue_2.5.22.js"></script>
    <script src="./lib/vue-router_3.0.2.js"></script>
  </head>
  <body>
    <!-- 被 vm 实例所控制的区域 -->
    <div id="app">
      <router-link to="/user/1">User1</router-link>
      <router-link to="/user/2">User2</router-link>
      <router-link to="/user/3">User3</router-link>
      <router-link to="/register">Register</router-link>

      <!-- 路由占位符 -->
      <router-view></router-view>
    </div>

    <script>
      const User = {
        props: ['id', 'uname', 'age'],
        template: '<h1>User 组件 -- 用户id为: {{id}} -- 姓名为:{{uname}} -- 年龄为：{{age}}</h1>'
      }

      const Register = {
        template: '<h1>Register 组件</h1>'
      }

      // 创建路由实例对象
      const router = new VueRouter({
        // 所有的路由规则
        routes: [
          { path: '/', redirect: '/user' },
          {
            path: '/user/:id',
            component: User,
            props: route => ({ uname: 'zs', age: 20, id: route.params.id })
          },
          { path: '/register', component: Register }
        ]
      })

      // 创建 vm 实例对象
      const vm = new Vue({
        // 指定控制的区域
        el: '#app',
        data: {},
        // 挂载路由实例对象
        // router: router
        router
      })
    </script>
  </body>
</html>

```
##### 22.2.2.7 命名路由
```markdown
为了更加方便的表示路由的路径，可以给路由规则起一个别名，
即为“命名路由”。
 const router = new VueRouter({
 routes: [
 {
 path: '/user/:id',
 name: 'user',
 component: User
 }
 ]
 })
 <router-link :to="{ name: 'user', params: { id: 123 }}">User</router-link>
 router.push({ name: 'user', params: { id: 123 }})
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <!-- 导入 vue 文件 -->
    <script src="./lib/vue_2.5.22.js"></script>
    <script src="./lib/vue-router_3.0.2.js"></script>
  </head>
  <body>
    <!-- 被 vm 实例所控制的区域 -->
    <div id="app">
      <router-link to="/user/1">User1</router-link>
      <router-link to="/user/2">User2</router-link>
      <router-link :to="{ name: 'user', params: {id: 3} }">User3</router-link>
      <router-link to="/register">Register</router-link>

      <!-- 路由占位符 -->
      <router-view></router-view>
    </div>

    <script>
      const User = {
        props: ['id', 'uname', 'age'],
        template: '<h1>User 组件 -- 用户id为: {{id}} -- 姓名为:{{uname}} -- 年龄为：{{age}}</h1>'
      }

      const Register = {
        template: '<h1>Register 组件</h1>'
      }

      // 创建路由实例对象
      const router = new VueRouter({
        // 所有的路由规则
        routes: [
          { path: '/', redirect: '/user' },
          {
            // 命名路由
            name: 'user',
            path: '/user/:id',
            component: User,
            props: route => ({ uname: 'zs', age: 20, id: route.params.id })
          },
          { path: '/register', component: Register }
        ]
      })

      // 创建 vm 实例对象
      const vm = new Vue({
        // 指定控制的区域
        el: '#app',
        data: {},
        // 挂载路由实例对象
        // router: router
        router
      })
    </script>
  </body>
</html>

```
##### 22.2.2.8 编程式导航
```markdown
页面导航的两种方式
声明式导航：通过点击链接实现导航的方式，叫做声明式导航
例如：普通网页中的 <a></a> 链接 或 vue 中的 
<router-link></router-link>
编程式导航：通过调用JavaScript形式的API实现导航的方式，
叫做编程式导航
例如：普通网页中的 location.href 
```
```html
常用的编程式导航 API 如下：
this.$router.push('hash地址')
this.$router.go(n)
 const User = {
 template: '<div><button @click="goRegister">跳转到注册页面</button></div>',
 methods: {
 goRegister: function(){
 // 用编程的方式控制路由跳转
 this.$router.push('/register');
 }
 }
 }
router.push() 方法的参数规则
 // 字符串(路径名称)
 router.push('/home')
 // 对象
 router.push({ path: '/home' })
 // 命名的路由(传递参数)
 router.push({ name: '/user', params: { userId: 123 }})
 // 带查询参数，变成 /register?uname=lisi
 router.push({ path: '/register', query: { uname: 'lisi' }})
```
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <!-- 导入 vue 文件 -->
    <script src="./lib/vue_2.5.22.js"></script>
    <script src="./lib/vue-router_3.0.2.js"></script>
  </head>
  <body>
    <!-- 被 vm 实例所控制的区域 -->
    <div id="app">
      <router-link to="/user/1">User1</router-link>
      <router-link to="/user/2">User2</router-link>
      <router-link :to="{ name: 'user', params: {id: 3} }">User3</router-link>
      <router-link to="/register">Register</router-link>

      <!-- 路由占位符 -->
      <router-view></router-view>
    </div>

    <script>
      const User = {
        props: ['id', 'uname', 'age'],
        template: `<div>
          <h1>User 组件 -- 用户id为: {{id}} -- 姓名为:{{uname}} -- 年龄为：{{age}}</h1>
          <button @click="goRegister">跳转到注册页面</button>
        </div>`,
        methods: {
          goRegister() {
            this.$router.push('/register')
          }
        },
      }

      const Register = {
        template: `<div>
          <h1>Register 组件</h1>
          <button @click="goBack">后退</button>
        </div>`,
        methods: {
          goBack() {
            this.$router.go(-1)
          }
        }
      }

      // 创建路由实例对象
      const router = new VueRouter({
        // 所有的路由规则
        routes: [
          { path: '/', redirect: '/user' },
          {
            // 命名路由
            name: 'user',
            path: '/user/:id',
            component: User,
            props: route => ({ uname: 'zs', age: 20, id: route.params.id })
          },
          { path: '/register', component: Register }
        ]
      })

      // 创建 vm 实例对象
      const vm = new Vue({
        // 指定控制的区域
        el: '#app',
        data: {},
        // 挂载路由实例对象
        // router: router
        router
      })
    </script>
  </body>
</html>
```
##### 22.2.2.9 基于vue-router的案例
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200825080855269.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
用到的路由技术要点：
路由的基础用法
嵌套路由
路由重定向
路由传参
编程式导航


根据项目的整体布局划分好组件结构，通过路由导航控制组件的显示
1. 抽离并渲染 App 根组件
2. 将左侧菜单改造为路由链接
3. 创建左侧菜单对应的路由组件
4. 在右侧主体区域添加路由占位符
5. 添加子路由规则
6. 通过路由重定向默认渲染用户组件
7. 渲染用户列表数据
8. 编程式导航跳转到用户详情页
9. 实现后退功能
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>基于vue-router的案例</title>
    <style type="text/css">
      html,
      body,
      #app {
        margin: 0;
        padding: 0px;
        height: 100%;
      }
      .header {
        height: 50px;
        background-color: #545c64;
        line-height: 50px;
        text-align: center;
        font-size: 24px;
        color: #fff;
      }
      .footer {
        height: 40px;
        line-height: 40px;
        background-color: #888;
        position: absolute;
        bottom: 0;
        width: 100%;
        text-align: center;
        color: #fff;
      }
      .main {
        display: flex;
        position: absolute;
        top: 50px;
        bottom: 40px;
        width: 100%;
      }
      .content {
        flex: 1;
        text-align: center;
        height: 100%;
      }
      .left {
        flex: 0 0 20%;
        background-color: #545c64;
      }
      .left a {
        color: white;
        text-decoration: none;
      }
      .right {
        margin: 5px;
      }
      .btns {
        width: 100%;
        height: 35px;
        line-height: 35px;
        background-color: #f5f5f5;
        text-align: left;
        padding-left: 10px;
        box-sizing: border-box;
      }
      button {
        height: 30px;
        background-color: #ecf5ff;
        border: 1px solid lightskyblue;
        font-size: 12px;
        padding: 0 20px;
      }
      .main-content {
        margin-top: 10px;
      }
      ul {
        margin: 0;
        padding: 0;
        list-style: none;
      }
      ul li {
        height: 45px;
        line-height: 45px;
        background-color: #a0a0a0;
        color: #fff;
        cursor: pointer;
        border-bottom: 1px solid #fff;
      }

      table {
        width: 100%;
        border-collapse: collapse;
      }

      td,
      th {
        border: 1px solid #eee;
        line-height: 35px;
        font-size: 12px;
      }

      th {
        background-color: #ddd;
      }
    </style>
    <script src="./lib/vue_2.5.22.js"></script>
    <script src="./lib/vue-router_3.0.2.js"></script>
  </head>
  <body>
    <!-- 要被 vue 实例所控制的区域 -->
    <div id="app">
      <!-- 路由占位符 -->
      <router-view></router-view>
    </div>

    <script>
      // 定义 APP 根组件
      const App = {
        template: `<div>
          <!-- 头部区域 -->
          <header class="header">传智后台管理系统</header>
          <!-- 中间主体区域 -->
          <div class="main">
            <!-- 左侧菜单栏 -->
            <div class="content left">
              <ul>
                <li><router-link to="/users">用户管理</router-link></li>
                <li><router-link to="/rights">权限管理</router-link></li>
                <li><router-link to="/goods">商品管理</router-link></li>
                <li><router-link to="/orders">订单管理</router-link></li>
                <li><router-link to="/settings">系统设置</router-link></li>
              </ul>
            </div>
            <!-- 右侧内容区域 -->
            <div class="content right"><div class="main-content">
              <router-view />
            </div></div>
          </div>
          <!-- 尾部区域 -->
          <footer class="footer">版权信息</footer>
        </div>`
      }

      const Users = {
        data() {
          return {
            userlist: [
              { id: 1, name: '张三', age: 10 },
              { id: 2, name: '李四', age: 20 },
              { id: 3, name: '王五', age: 30 },
              { id: 4, name: '赵六', age: 40 }
            ]
          }
        },
        methods: {
          goDetail(id) {
            console.log(id)
            this.$router.push('/userinfo/' + id)
          }
        },
        template: `<div>
        <h3>用户管理区域</h3>
        <table>
          <thead>
            <tr><th>编号</th><th>姓名</th><th>年龄</th><th>操作</th></tr>
          </thead>
          <tbody>
            <tr v-for="item in userlist" :key="item.id">
              <td>{{item.id}}</td>
              <td>{{item.name}}</td>
              <td>{{item.age}}</td>
              <td>
                <a href="javascript:;" @click="goDetail(item.id)">详情</a>
              </td>
            </tr>
          </tbody>
        </table>
      </div>`
      }

      const UserInfo = {
        props: ['id'],
        template: `<div>
          <h5>用户详情页 --- 用户Id为：{{id}}</h5>
          <button @click="goback()">后退</button>
        </div>`,
        methods: {
          goback() {
            // 实现后退功能
            this.$router.go(-1)
          }
        }
      }

      const Rights = {
        template: `<div>
        <h3>权限管理区域</h3>
      </div>`
      }
      const Goods = {
        template: `<div>
        <h3>商品管理区域</h3>
      </div>`
      }
      const Orders = {
        template: `<div>
        <h3>订单管理区域</h3>
      </div>`
      }
      const Settings = {
        template: `<div>
        <h3>系统设置区域</h3>
      </div>`
      }

      // 创建路由对象
      const router = new VueRouter({
        routes: [
          {
            path: '/',
            component: App,
            redirect: '/users',
            children: [
              { path: '/users', component: Users },
              { path: '/userinfo/:id', component: UserInfo, props: true },
              { path: '/rights', component: Rights },
              { path: '/goods', component: Goods },
              { path: '/orders', component: Orders },
              { path: '/settings', component: Settings }
            ]
          }
        ]
      })

      const vm = new Vue({
        el: '#app',
        router
      })
    </script>
  </body>
</html>

```

## 23 前端工程化
### 23.1 模块化相关规范
```markdown
传统开发模式的主要问题
命名冲突
文件依赖
通过模块化解决上述问题
模块化就是把单独的一个功能封装到一个模块（文件）中，
模块之间相互隔离，但是可以通过特定的接口公开内部成
员，也可以依赖别的模块
模块化开发的好处：方便代码的重用，从而提升开发效率，
并且方便后期的维护

浏览器端模块化规范
1. AMD
Require.js (http://www.requirejs.cn/)
2. CMD
3. Sea.js (https://seajs.github.io/seajs/docs/)
服务器端模块化规范
4. CommonJS
模块分为 单文件模块 与 包
模块成员导出：module.exports 和 exports
模块成员导入：require('模块标识符')
```
### 23.2 ES6模块化
```markdown
在 ES6 模块化规范诞生之前，Javascript 社区已经尝试并提出了 
AMD、CMD、CommonJS 等模块化规范。
但是，这些社区提出的模块化标准，还是存在一定的差异性与局限性、
并不是浏览器与服务器通用的模块化标准，例如：
AMD 和 CMD 适用于浏览器端的 Javascript 模块化
CommonJS 适用于服务器端的 Javascript 模块化
因此，ES6 语法规范中，在语言层面上定义了 ES6 模块化规范，
是浏览器端与服务器端通用的模块化开发规范。
ES6模块化规范中定义：
每个 js 文件都是一个独立的模块
导入模块成员使用 import 关键字
暴露模块成员使用 export 关键字
```
#### 23.2.1 Node.js 中通过 babel 体验 ES6 模块化
```markdown
1 npm install --save-dev @babel/core @babel/cli @babel/preset-env @babel/node
2 npm install --save @babel/polyfill
3 项目跟目录创建文件 babel.config.js
4 babel.config.js 文件内容如右侧代码
const presets = [
 ["@babel/env", {
 targets: {
 edge: "17",
 firefox: "60",
 chrome: "67",
 safari: "11.1"
 }
 }]
 ];
 module.exports = { presets };
5 通过 npx babel-node index.js 执行代码
```
#### 23.2.2 ES6 模块化的基本语法
```markdown
默认导出语法 export default 默认导出的成员
默认导入语法 import 接收名称 from '模块标识符'
// 导入模块成员
import m1 from './m1.js'
console.log(m1)
// 打印输出的结果为：
// { a: 10, c: 20, show: [Function: show] }
// 当前文件模块为 m1.js
// 定义私有成员 a 和 c
let a = 10
let c = 20
// 外界访问不到变量 d ,因为它没有被暴露出去
let d = 30
function show() {}
// 将本模块中的私有成员暴露出去，供其它模块使用
export default {
a,
c,
show
}
注意：每个模块中，只允许使用唯一的一次 export default，
否则会报错！


按需导出 与 按需导入
按需导出语法 export let s1 = 10
按需导入语法 import { s1 } from '模块标识符'
// 导入模块成员
import { s1, s2 as ss2, say } from './m1.js'
console.log(s1) // 打印输出 aaa
console.log(ss2) // 打印输出 ccc
console.log(say) // 打印输出 [Function: say]

// 当前文件模块为 m1.js
// 向外按需导出变量 s1
export let s1 = 'aaa'
// 向外按需导出变量 s2
export let s2 = 'ccc'
// 向外按需导出方法 say
export function say = function() {}
注意：每个模块中，可以使用多次按需导出

直接导入并执行模块代码
有时候，我们只想单纯执行某个模块中的代码，并不
需要得到模块中向外暴露的成员，此时，可以直接导
入并执行模块代码。
// 当前文件模块为 m2.js
// 在当前模块中执行一个 for 循环操作
for(let i = 0; i < 3; i++) {
console.log(i)
}
```
#### 23.2.3 属性的增强写法
```html
<script>
  // const obj = new Object()

  // const obj = {
  //   name: 'why',
  //   age: 18,
  //   run: function () {
  //     console.log('在奔跑');
  //   },
  //   eat: function () {
  //     console.log('在次东西');
  //   }
  // }

  // 1.属性的增强写法
  const name = 'why';
  const age = 18;
  const height = 1.88

  // ES5的写法
  // const obj = {
  //   name: name,
  //   age: age,
  //   height: height
  // }
  //ES6的写法
  // const obj = {
  //   name,
  //   age,
  //   height,
  // }
  //
  // console.log(obj);


  // 2.函数的增强写法
  // ES5的写法
  // const obj = {
  //   run: function () {
  //
  //   },
  //   eat: function () {
  //
  //   }
  // }
  //ES6的写法
  const obj = {
    run() {

    },
    eat() {

    }
  }
</script>
```
### 23.3 webpack
![*](https://img-blog.csdnimg.cn/2020082508334232.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
webpack 是一个流行的前端项目构建工具（打包工具），
可以解决当前 web 开发中所面临的困境。
webpack 提供了友好的模块化支持，以及代码压缩混淆、
处理 js 兼容问题、性能优化等强大的功能，从而让程序员把
工作的重心放到具体的功能实现上，提高了开发效率和项目
的可维护性。
目前绝大多数企业中的前端项目，都是基于 webpack 进行
打包构建的。
```
#### 23.3.1 webpack 的基本使用
```markdown
1. 创建列表隔行变色项目
- 新建项目空白目录，并运行 npm init –y 命令，初始化包管理配置文件 package.json
- 新建 src 源代码目录
- 新建 src -> index.html 首页
- 初始化首页基本的结构
- 运行 npm install jquery –S 命令，安装 jQuery
- 通过模块化的形式，实现列表隔行变色效果

2. 在项目中安装和配置 webpack
- 运行 npm install webpack webpack-cli –D 命令，安装 webpack 相关的包
- 在项目根目录中，创建名为 webpack.config.js 的 webpack 配置文件
- 在 webpack 的配置文件中，初始化如下基本配置：
module.exports = {
 mode: 'development' // mode 用来指定构建模式
}
④ 在 package.json 配置文件中的 scripts 节点下，新增 dev 脚本如下：
"scripts": {
"dev": "webpack" // script 节点下的脚本，可以通过 npm run 执行
}
⑤ 在终端中运行 npm run dev 命令，启动 webpack 进行项目打包。
webpack 的 4.x 版本中默认约定：
打包的入口文件为 src -> index.js
打包的输出文件为 dist -> main.js

如果要修改打包的入口与出口，可以在 webpack.config.js 中新增如下配置信息：
const path = require('path') // 导入 node.js 中专门操作路径的模块
module.exports = {
 entry: path.join(__dirname, './src/index.js'), // 打包入口文件的路径
 output: {
 path: path.join(__dirname, './dist'), // 输出文件的存放路径
 filename: 'bundle.js' // 输出文件的名称
 }
}

4. 配置 webpack 的自动打包功能
- 运行 npm install webpack-dev-server –D 命令，安装支持项目自动打包的工具
- 修改 package.json -> scripts 中的 dev 命令如下：
"scripts": {
 "dev": "webpack-dev-server" // script 节点下的脚本，可以通过 npm run 执行
}
- 将 src -> index.html 中，script 脚本的引用路径，修改为 "/buldle.js“
- 运行 npm run dev 命令，重新进行打包
- 在浏览器中访问 http://localhost:8080 地址，查看自动打包效果
注意：
webpack-dev-server 会启动一个实时打包的 http 服务器
webpack-dev-server 打包生成的输出文件，默认放到了项目根目录中，而且是虚拟的、看不见的
5. 配置 html-webpack-plugin 生成预览页面
- 运行 npm install html-webpack-plugin –D 命令，安装生成预览页面的插件
- 修改 webpack.config.js 文件头部区域，添加如下配置信息：
// 导入生成预览页面的插件，得到一个构造函数
const HtmlWebpackPlugin = require('html-webpack-plugin')
const htmlPlugin = new HtmlWebpackPlugin({ // 创建插件的实例对象
 template: './src/index.html', // 指定要用到的模板文件
 filename: 'index.html' // 指定生成的文件的名称，该文件存在于内存中，在目录中不显示
})

③ 修改 webpack.config.js 文件中向外暴露的配置对象，新增如下配置节点：
module.exports = {
 plugins: [ htmlPlugin ] // plugins 数组是 webpack 打包期间会用到的一些插件列表
}

6. 配置自动打包相关的参数
// package.json中的配置
 // --open 打包完成后自动打开浏览器页面
 // --host 配置 IP 地址
 // --port 配置端口
 "scripts": {
 "dev": "webpack-dev-server --open --host 127.0.0.1 --port 8888"
 },
 
webpack 中的加载器
1. 通过 loader 打包非 js 模块
在实际开发过程中，webpack 默认只能打包处理以 .js 后缀名结尾的模块，其他非 .js 后缀名结
尾的模块，webpack 默认处理不了，需要调用 loader 加载器才可以正常打包，否则会报错！
loader 加载器可以协助 webpack 打包处理特定的文件模块，比如：

less-loader 可以打包处理 .less 相关的文件

sass-loader 可以打包处理 .scss 相关的文件

url-loader 可以打包处理 css 中与 url 路径相关的文件
```
#### 23.3.2 loader 的调用过程
![*](https://img-blog.csdnimg.cn/20200826081023552.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
##### 23.3.2.1 loaderwebpack 中加载器的基本使用
```markdown
1. 打包处理 css 文件
- 运行 npm i style-loader css-loader -D 命令，安装处理 css 文件的 loader
- 在 webpack.config.js 的 module -> rules 数组中，添加 loader 规则如下：
// 所有第三方文件模块的匹配规则
 module: {
 rules: [
 { test: /\.css$/, use: ['style-loader', 'css-loader'] }
 ]
 }
其中，test 表示匹配的文件类型， use 表示对应要调用的 loader
注意：
use 数组中指定的 loader 顺序是固定的
多个 loader 的调用顺序是：从后往前调用

2. 打包处理 less 文件
- 运行 npm i less-loader less -D 命令
- 在 webpack.config.js 的 module -> rules 数组中，添加 loader 规则如下：
// 所有第三方文件模块的匹配规则
 module: {
 rules: [
 { test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] }
 ]
 }

3. 打包处理 scss 文件
- 运行 npm i sass-loader node-sass -D 命令
- 在 webpack.config.js 的 module -> rules 数组中，添加 loader 规则如下：
// 所有第三方文件模块的匹配规则
 module: {
 rules: [
 { test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader'] }
 ]
 }
4. 配置 postCSS 自动添加 css 的兼容前缀
- 运行 npm i postcss-loader autoprefixer -D 命令
- 在项目根目录中创建 postcss 的配置文件 postcss.config.js，并初始化如下配置：
 const autoprefixer = require('autoprefixer') // 导入自动添加前缀的插件
 module.exports = {
 plugins: [ autoprefixer ] // 挂载插件
 }
- 在 webpack.config.js 的 module -> rules 数组中，修改 css 的 loader 规则如下：
 module: {
 rules: [
 { test:/\.css$/, use: ['style-loader', 'css-loader', 'postcss-loader'] }
 ]
 }
5. 打包样式表中的图片和字体文件
- 运行 npm i url-loader file-loader -D 命令
- 在 webpack.config.js 的 module -> rules 数组中，添加 loader 规则如下：
 module: {
 rules: [
 {
 test: /\.jpg|png|gif|bmp|ttf|eot|svg|woff|woff2$/,
 use: 'url-loader?limit=16940'
 }
 ]
 }
 其中 ? 之后的是 loader 的参数项。
limit 用来指定图片的大小，单位是字节(byte),只有小于 limit 大小的图片，才会被转为 base64 图片

6. 打包处理 js 文件中的高级语法
- 安装babel转换器相关的包：npm i babel-loader @babel/core @babel/runtime -D
- 安装babel语法插件相关的包：npm i @babel/preset-env @babel/plugin-transformruntime @babel/plugin-proposal-class-properties –D
- 在项目根目录中，创建 babel 配置文件 babel.config.js 并初始化基本配置如下：
module.exports = {
 presets: [ '@babel/preset-env' ],
 plugins: [ '@babel/plugin-transform-runtime', '@babel/plugin-proposalclass-properties’ ]
 }
- 在 webpack.config.js 的 module -> rules 数组中，添加 loader 规则如下：
  // exclude 为排除项，表示 babel-loader 不需要处理 node_modules 中的 js 文件
 { test: /\.js$/, use: 'babel-loader', exclude: /node_modules/ }
```
#### 23.3.3  Vue 单文件组件
```markdown
单文件组件的组成结构
template 组件的模板区域
script 业务逻辑区域
style 样式区域
 <template>
 <!-- 这里用于定义Vue组件的模板内容 -->
 </template>
 <script>
 // 这里用于定义Vue组件的业务逻辑
 export default {
 data: () { return {} }, // 私有数据
 methods: {} // 处理函数
 // ... 其它业务逻辑
 }
 </script>
 <style scoped>
 /* 这里用于定义组件的样式 */
 </style>

webpack 中配置 vue 组件的加载器
- 运行 npm i vue-loader vue-template-compiler -D 命令
- 在 webpack.config.js 配置文件中，添加 vue-loader 的配置项如下：
const VueLoaderPlugin = require('vue-loader/lib/plugin')
module.exports = {
module: {
rules: [
// ... 其它规则
{ test: /\.vue$/, loader: 'vue-loader' }
]
},
plugins: [
// ... 其它插件
new VueLoaderPlugin() // 请确保引入这个插件！
]
}

在 webpack 项目中使用 vue
- 运行 npm i vue –S 安装 vue
- 在 src -> index.js 入口文件中，通过 import Vue from 'vue' 来导入 vue 构造函数
- 创建 vue 的实例对象，并指定要控制的 el 区域
- 通过 render 函数渲染 App 根组件
// 1. 导入 Vue 构造函数
import Vue from 'vue'
// 2. 导入 App 根组件
import App from './components/App.vue'
const vm = new Vue({
// 3. 指定 vm 实例要控制的页面区域
el: '#app',
// 4. 通过 render 函数，把指定的组件渲染到 el 区域中
render: h => h(App)
})

webpack 打包发布
上线之前需要通过webpack将应用进行整体打包，可以通过 package.json 文件配置打包命令：
// 在package.json文件中配置 webpack 打包命令
// 该命令默认加载项目根目录中的 webpack.config.js 配置文件
"scripts": {
// 用于打包的命令
"build": "webpack -p",
// 用于开发调试的命令
"dev": "webpack-dev-server --open --host 127.0.0.1 --port 3000",
},
```
## 24 Vue CLI 脚手架
### 24.1 什么是CLI
```markdown
命令行界面（英语：command-line interface，缩写：CLI）
是在图形用户界面得到普及之前使用最为广泛的用户界面，
它通常不支持鼠标，用户通过键盘输入指令，计算机接收到指令后，
予以执行。也有人称之为字符用户界面（CUI）
```

### 24.2 什么是Vue CLI
```markdown
Vue CLI 是一个基于 Vue.js 进行快速开发的完整系统。
使用Vue 脚手架之后我们开发的页面将是一个完整系统(项目)。
```

### 24.3 Vue CLI优势
```markdown
- 通过 vue-cli 搭建交互式的项目脚手架。bootstrap css js jquery js     通过执行命令方式下载相关依赖
- 通过 @vue/cli + @vue/cli-service-global 快速开始零配置原型开发    vue页面 vuejs  vuerouter        axios(一条命令)
- 一个运行时依赖 (@vue/cli-service)，该依赖：
  - 可升级；  一条命令
  - 基于 webpack 构建，并带有合理的默认配置；  webpack  项目打包方式     编译好的项目源码===>部署到服务器上直接使用
  - 可以通过项目内的配置文件进行配置；               默认配置文件,通过修改默认配置文件达到自己想要的项目环境            
  - 可以通过插件进行扩展。                                       vue v-charts  elementui 
- 一个丰富的官方插件集合，集成了前端生态中最好的工具。Nodejs(tomcat)  Vue VueRouter webpack yarn
- 一套完全图形化的创建和管理 Vue.js 项目的用户界面
```
### 24.4 Vue CLI安装
##### 24.4.1  环境准备
```markdown
1.下载nodejs
		http://nodejs.cn/download/
			windows系统:   .msi  安装包(exe)指定安装位置   .zip(压缩包)直接解压缩指定目录
		  mac os 系统:   .pkg  安装包格式自动配置环境变量  .tar.gz(压缩包)解压缩安装到指定名

2.配置nodejs环境变量
	windows系统:
	1.计算上右键属性---->  高级属性 ---->环境变量 添加如下配置:
				NODE_HOME=  nodejs安装目录
        PATH    = xxxx;%NODE_HOME%
    2.macos 系统
    		推荐使用.pkg安装直接配置node环境
 
3.验证nodejs环境是否成功
		node -v 
		
4.npm介绍
		node package mangager    nodejs包管理工具       前端主流技术  npm 进行统一管理
			maven 管理java后端依赖   远程仓库(中心仓库)      阿里云镜像
			npm   管理前端系统依赖    远程仓库(中心仓库)      配置淘宝镜像
		
5.配置淘宝镜像
	  npm config set registry https://registry.npm.taobao.org
	  npm config get registry

6.配置npm下载依赖位置
	 windows:
		npm config set cache "D:\nodereps\npm-cache"
		npm config set prefix "D:\nodereps\npm_global"
	 mac os:
	 	npm config set cache "/Users/chenyannan/dev/nodereps"
		npm config set prefix "/Users/chenyannan/dev/nodereps"

7.验证nodejs环境配置
	npm config ls
	
    ; userconfig /Users/chenyannan/.npmrc
    cache = "/Users/chenyannan/dev/nodereps"
    prefix = "/Users/chenyannan/dev/nodereps"
    registry = "https://registry.npm.taobao.org/"

```
##### 24.4.2 安装脚手架

```markdown
安装
1.x或2.x
npm install vue-cli -g
3.x以上
npm install -g @vue/cli  
# OR yarn global add @vue/cli
卸载
前提条件
自己电脑已经安装node.js和npm
卸载vue-cli(1.x 或2.x)
npm uninstall vue-cli -g 或yarn global remove vue-cli 
卸载cli3
npm uninstall -g @vue/cli 或 yarn global remove @vue/cli

升级
如需升级全局的 Vue CLI 包，请运行：
npm update -g @vue/cli
# 或者
yarn global upgrade --latest @vue/cli

解决npm下载速度慢的问题
使用淘宝定制的cnpm命令行工具替代默认安装npm
npm install -g cnpm --registry=https://registry.npm.taobao.org


创建vue-cli2项目
vue init webpack 项目名称

创建vue-cli3项目
图形界面的方式创建项目
vue ui
通过命令行创建（主流方式）
通过命令行创建（主流方式）

脚手架2启动方式
npm run dev
脚手架3启动方式
npm run serve

脚手架2打包
npm run build # 生成的是build文件
脚手架3打包
npm run build # 生成的是dist文件

脚手架2本机测试
serve build # 因为你最后直接给的是打包文件，交工之前直接测试一下，运行打包文件，查看项目是否完整
脚手架3本机测试
serve dist
```

##### 24.4.3 第一个vue脚手架项目
```markdown
1.创建vue脚手架第一个项目
	vue init webpack 项目名
2.创建第一个项目
	hello     ------------->项目名
    -build  ------------->用来使用webpack打包使用build依赖
    -config ------------->用来做整个项目配置目录
    -node_modules  ------>用来管理项目中使用依赖
    -src					 ------>用来书写vue的源代码[重点]
    	+assets      ------>用来存放静态资源 [重点]
      components   ------>用来书写Vue组件 [重点]
      router			 ------>用来配置项目中路由[重点]
      App.vue      ------>项目中根组件[重点]
      main.js      ------>项目中主入口[重点]
    -static        ------>其它静态
    -.babelrc      ------> 将es6语法转为es5运行
    -.editorconfig ------> 项目编辑配置
    -.gitignore    ------> git版本控制忽略文件
    -.postcssrc.js ------> 源码相关js
    -index.html    ------> 项目主页
    -package.json  ------> 类似与pom.xml 依赖管理  jquery 不建议手动修改
    -package-lock.json ----> 对package.json加锁
    -README.md         ----> 项目说明文件

3.如何运行在项目的根目录中执行
		npm run dev

4.如何访问项目
		http://localhost:8081    

5.Vue Cli中项目开发方式
	 注意: 一切皆组件   一个组件中   js代码  html代码  css样式
	 
	 	1. VueCli开发方式是在项目中开发一个一个组件对应一个业务功能模块,日后可以将多个组件组合到一起形成一个前端系统
	 	2. 日后在使用vue Cli进行开发时不再书写html,编写的是一个个组件(组件后缀.vue结尾的文件),日后打包时vue cli会将组件编译成运行的html文件	  
```

##### 24.4.4 Vue 脚手架的自定义配置
```markdown
1. 通过 package.json 配置项目
 // 必须是符合规范的json语法
 "vue": {
 "devServer": {
 "port": "8888",
 "open" : true
 }
 },
注意：不推荐使用这种配置方式。因为 package.json 主要用来管理包的配置信息；为了方便维护，推荐将 vue 脚
手架相关的配置，单独定义到 vue.config.js 配置文件中。

2. 通过单独的配置文件配置项目
- 在项目的跟目录创建文件 vue.config.js
- 在该文件中进行相关配置，从而覆盖默认配置
// vue.config.js
 module.exports = {
 devServer: {
 port: 8888
 }
 }
```
## 25 Vuex
### 25.1 组件之间共享数据的方式
```markdown
父向子传值：v-bind 属性绑定
子向父传值：v-on 事件绑定
兄弟组件之间共享数据： EventBus
- $on 接收数据的那个组件
- $emit 发送数据的那个组件
但是这种范围比较小,维护起来不方便,所以vuex就诞生了.
```
```markdown
Vuex 是实现组件全局状态（数据）管理的一种机制，可以方便
的实现组件之间数据的共享。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200523071951619.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)
### 25.2 Vuex实现组件之间共享数据
```markdown
使用 Vuex 统一管理状态的好处
- 能够在 vuex 中集中管理共享的数据，易于开发和后期维护
- 能够高效地实现组件之间的数据共享，提高开发效率
- 存储在 vuex 中的数据都是响应式的，能够实时保持数据与
页面的同步

什么样的数据适合存储到 Vuex 中
一般情况下，只有组件之间共享的数据，才有必要存储到 vuex 中；
对于组件中的私有数据，依旧存储在组件自身的 data 中即可。
```

### 25.3 Vuex 的基本使用
```markdown
1. 安装 vuex 依赖包
 npm install vuex --save
2. 导入 vuex 包
import Vuex from 'vuex'
Vue.use(Vuex)
3. 创建 store 对象
const store = new Vuex.Store({
 // state 中存放的就是全局共享的数据
 state: { count: 0 }
})
4. 将 store 对象挂载到 vue 实例中
new Vue({
 el: '#app',
 render: h => h(app),
 router,
 // 将创建的共享数据对象，挂载到 Vue 实例中
 // 所有的组件，就可以直接从 store 中获取全局的数据了
 store
})
```
#### 25.3.1 计数器
```html
App.vue

<template>
  <div>
    <my-addition></my-addition>

    <p>---------------------------------</p>

    <my-subtraction></my-subtraction>
  </div>
</template>

<script>
import Addition from './components/Addition.vue'
import Subtraction from './components/Subtraction.vue'

export default {
  data() {
    return {}
  },
  components: {
    'my-addition': Addition,
    'my-subtraction': Subtraction
  }
}
</script>

Addition.vue
<template>
  <div>
    <!-- <h3>当前最新的count值为：{{$store.state.count}}</h3> -->
    <h3>{{$store.getters.showNum}}</h3>
    <button @click="btnHandler1">+1</button>
    <button @click="btnHandler2">+N</button>
    <button @click="btnHandler3">+1 Async</button>
    <button @click="btnHandler4">+N Async</button>
  </div>
</template>

<script>
export default {
  data() {
    return {}
  },
  methods: {
    btnHandler1() {
      this.$store.commit('add')
    },
    btnHandler2() {
      // commit 的作用，就是调用 某个 mutation 函数
      this.$store.commit('addN', 3)
    },
    // 异步地让 count 自增 +1
    btnHandler3() {
      // 这里的 dispatch 函数，专门用来触发 action
      this.$store.dispatch('addAsync')
    },
    btnHandler4() {
      this.$store.dispatch('addNAsync', 5)
    }
  }
}
</script>

Subtraction.vue
<template>
  <div>
    <!-- <h3>当前最新的count值为：{{count}}</h3> -->
    <h3>{{showNum}}</h3>
    <button @click="btnHandler1">-1</button>
    <button @click="subN(3)">-N</button>
    <button @click="subAsync">-1 Async</button>
    <button @click="subNAsync(5)">-N Async</button>
  </div>
</template>

<script>
import { mapState, mapMutations, mapActions, mapGetters } from 'vuex'

export default {
  data() {
    return {}
  },
  computed: {
    ...mapState(['count']),
    ...mapGetters(['showNum'])
  },
  methods: {
    ...mapMutations(['sub', 'subN']),
    ...mapActions(['subAsync', 'subNAsync']),
    btnHandler1() {
      this.sub()
    }
  }
}
</script>

store.js

import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    count: 0
  },
  // 只有 mutations 中定义的函数，才有权利修改 state 中的数据
  mutations: {
    add(state) {
      // 不要在 mutations 函数中，执行异步操作
      // setTimeout(() => {
      //   state.count++
      // }, 1000)
      state.count++
    },
    addN(state, step) {
      state.count += step
    },
    sub(state) {
      state.count--
    },
    subN(state, step) {
      state.count -= step
    }
  },
  actions: {
    addAsync(context) {
      setTimeout(() => {
        // 在 actions 中，不能直接修改 state 中的数据；
        // 必须通过 context.commit() 触发某个 mutation 才行
        context.commit('add')
      }, 1000)
    },
    addNAsync(context, step) {
      setTimeout(() => {
        context.commit('addN', step)
      }, 1000)
    },
    subAsync(context) {
      setTimeout(() => {
        context.commit('sub')
      }, 1000)
    },
    subNAsync(context, step) {
      setTimeout(() => {
        context.commit('subN', step)
      }, 1000)
    }
  },
  getters: {
    showNum(state) {
      return '当前最新的数量是【' + state.count + '】'
    }
  }
})

main.js

import Vue from 'vue'
import App from './App.vue'
import store from './store'

Vue.config.productionTip = false

new Vue({
  store,
  render: h => h(App)
}).$mount('#app')


```
### 25.4 Vuex的核心概念
```markdown
State
Mutation
Action
Getter
```
```javascript
State
State 提供唯一的公共数据源，所有共享的数据都要统一放
到 Store 的 State 中进行存储。

// 创建store数据源，提供唯一公共数据
 const store = new Vuex.Store({
 state: { count: 0 }
 })
组件访问 State 中数据的第一种方式：
this.$store.state.全局数据名称

组件访问 State 中数据的第二种方式：
// 1. 从 vuex 中按需导入 mapState 函数
import { mapState } from 'vuex'
通过刚才导入的 mapState 函数，将当前组件需要的全局数据，
映射为当前组件的 computed 计算属性：
// 2. 将全局数据，映射为当前组件的计算属性
computed: {
 ...mapState(['count'])
}



Mutation
Mutation 用于变更 Store中 的数据。
- 只能通过 mutation 变更 Store 数据，不可以直接操作 Store 中的数据。
- 通过这种方式虽然操作起来稍微繁琐一些，但是可以集中监控所有数据的变化。

 // 定义 Mutation
 const store = new Vuex.Store({
 state: {
 count: 0
 },
 mutations: {
 add(state) {
 // 变更状态
 state.count++
 }
 }
 })
 // 触发mutation
 methods: {
 handle1() {
 // 触发 mutations 的第一种方式
 this.$store.commit('add')
 }
 } 
可以在触发 mutations 时传递参数
// 定义Mutation
 const store = new Vuex.Store({
 state: {
 count: 0
 },
 mutations: {
 addN(state, step) {
 // 变更状态
 state.count += step
 }
 }
 })

// 触发mutation
 methods: {
 handle2() {
 // 在调用 commit 函数，
 // 触发 mutations 时携带参数
 this.$store.commit('addN', 3)
 }
 } 

this.$store.commit() 是触发 mutations 的第一种方式，触发 mutations 的第二种方式：
// 1. 从 vuex 中按需导入 mapMutations 函数
import { mapMutations } from 'vuex'
通过刚才导入的 mapMutations 函数，将需要的 mutations 函数，映射为当前组件的 methods 方法：
// 2. 将指定的 mutations 函数，映射为当前组件的 methods 函数
methods: {
 ...mapMutations(['add', 'addN'])
}

Action
Action 用于处理异步任务。
如果通过异步操作变更数据，必须通过 Action，而不能使用 Mutation，但是在 Action 中还是要通过触发
Mutation 的方式间接变更数据。

// 定义 Action
 const store = new Vuex.Store({
 // ...省略其他代码
 mutations: {
 add(state) {
 state.count++
 }
 },
 actions: {
 addAsync(context) {
 setTimeout(() => {
 context.commit('add')
 }, 1000)
 }
 }
// 触发 Action
 methods: {
 handle() {
 // 触发 actions 的第一种方式
 this.$store.dispatch('addAsync')
 }
 } 
 }) 
触发 actions 异步任务时携带参数：
 // 定义 Action
 const store = new Vuex.Store({
 // ...省略其他代码
 mutations: {
 addN(state, step) {
 state.count += step
 }
 },
 actions: {
 addNAsync(context, step) {
 setTimeout(() => {
 context.commit('addN', step)
 }, 1000)
 }
 }
 })
// 触发 Action
 methods: {
 handle() {
 // 在调用 dispatch 函数，
 // 触发 actions 时携带参数
 this.$store.dispatch('addNAsync', 5)
 }
 } 
 
this.$store.dispatch() 是触发 actions 的第一种方式，触发 actions 的第二种方式：
// 1. 从 vuex 中按需导入 mapActions 函数
import { mapActions } from 'vuex'

通过刚才导入的 mapActions 函数，将需要的 actions 函数，映射为当前组件的 methods 方法：
// 2. 将指定的 actions 函数，映射为当前组件的 methods 函数
methods: {
 ...mapActions(['addASync', 'addNASync'])
}

Getter
Getter 用于对 Store 中的数据进行加工处理形成新的数据。
- Getter 可以对 Store 中已有的数据加工处理之后形成新的数据，类似 Vue 的计算属性。
- Store 中数据发生变化，Getter 的数据也会跟着变化。

// 定义 Getter
 const store = new Vuex.Store({
 state: {
 count: 0
 },
 getters: {
 showNum: state => {
 return '当前最新的数量是【'+ state.count +'】'
 }
 }
 })
使用 getters 的第一种方式：
this.$store.getters.名称
使用 getters 的第二种方式：
import { mapGetters } from 'vuex'
computed: {
 ...mapGetters(['showNum'])
}
```
### 25.5 基于 Vuex 的案例
```markdown
Todos
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200905134907323.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
1. 初始化项目
实现步骤
- 通过 vue ui 命令打开可视化面板，创建新项目 vuex-demo2
- 安装 vuex 依赖包 npm install vuex axios ant-design-vue –S
- 实现 Todos 基本布局（基于已有样式模板）

2. 完成具体功能
- 动态加载任务列表数据
- 实现文本框与store数据的双向同步
- 完成添加任务事项的操作
- 完成删除任务事项的操作
- 动态绑定复选框的选中状态
- 修改任务事项的完成状态
- 统计未完成的任务的条数
- 清除已完成的任务事项
- 实现任务列表数据的动态切换
```

```javascript
App.vue
<template>
  <div id="app">
    <a-input placeholder="请输入任务" class="my_ipt" :value="inputValue" @change="handleInputChange" />
    <a-button type="primary" @click="addItemToList">添加事项</a-button>

    <a-list bordered :dataSource="infolist" class="dt_list">
      <a-list-item slot="renderItem" slot-scope="item">
        <!-- 复选框 -->
        <a-checkbox :checked="item.done" @change="(e) => {cbStatusChanged(e, item.id)}">{{item.info}}</a-checkbox>
        <!-- 删除链接 -->
        <a slot="actions" @click="removeItemById(item.id)">删除</a>
      </a-list-item>

      <!-- footer区域 -->
      <div slot="footer" class="footer">
        <!-- 未完成的任务个数 -->
        <span>{{unDoneLength}}条剩余</span>
        <!-- 操作按钮 -->
        <a-button-group>
          <a-button :type="viewKey === 'all' ? 'primary' : 'default'" @click="changeList('all')">全部</a-button>
          <a-button :type="viewKey === 'undone' ? 'primary' : 'default'" @click="changeList('undone')">未完成</a-button>
          <a-button :type="viewKey === 'done' ? 'primary' : 'default'" @click="changeList('done')">已完成</a-button>
        </a-button-group>
        <!-- 把已经完成的任务清空 -->
        <a @click="clean">清除已完成</a>
      </div>
    </a-list>
  </div>
</template>

<script>
import { mapState, mapGetters } from 'vuex'

export default {
  name: 'app',
  data() {
    return {}
  },
  created() {
    this.$store.dispatch('getList')
  },
  computed: {
    ...mapState(['inputValue', 'viewKey']),
    ...mapGetters(['unDoneLength', 'infolist'])
  },
  methods: {
    // 监听文本框内容变化
    handleInputChange(e) {
      this.$store.commit('setInputValue', e.target.value)
    },
    // 向列表中新增 item 项
    addItemToList() {
      if (this.inputValue.trim().length <= 0) {
        return this.$message.warning('文本框内容不能为空！')
      }

      this.$store.commit('addItem')
    },
    // 很据Id删除对应的任务事项
    removeItemById(id) {
      // console.log(id)
      this.$store.commit('removeItem', id)
    },
    // 监听复选框选中状态变化的事件
    cbStatusChanged(e, id) {
      // 通过 e.target.checked 可以接受到最新的选中状态
      // console.log(e.target.checked)
      // console.log(id)
      const param = {
        id: id,
        status: e.target.checked
      }

      this.$store.commit('changeStatus', param)
    },
    // 清除已完成的任务
    clean() {
      this.$store.commit('cleanDone')
    },
    // 修改页面上展示的列表数据
    changeList(key) {
      // console.log(key)
      this.$store.commit('changeViewKey', key)
    }
  }
}
</script>

<style scoped>
#app {
  padding: 10px;
}

.my_ipt {
  width: 500px;
  margin-right: 10px;
}

.dt_list {
  width: 500px;
  margin-top: 10px;
}

.footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>

store.js

import Vue from 'vue'
import Vuex from 'vuex'
import axios from 'axios'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    // 所有的任务列表
    list: [],
    // 文本框的内容
    inputValue: 'aaa',
    // 下一个Id
    nextId: 5,
    viewKey: 'all'
  },
  mutations: {
    initList(state, list) {
      state.list = list
    },
    // 为 store 中的 inputValue 赋值
    setInputValue(state, val) {
      state.inputValue = val
    },
    // 添加列表项
    addItem(state) {
      const obj = {
        id: state.nextId,
        info: state.inputValue.trim(),
        done: false
      }
      state.list.push(obj)
      state.nextId++
      state.inputValue = ''
    },
    // 根据Id删除对应的任务事项
    removeItem(state, id) {
      // 根据Id查找对应项的索引
      const i = state.list.findIndex(x => x.id === id)
      // 根据索引，删除对应的元素
      if (i !== -1) {
        state.list.splice(i, 1)
      }
    },
    // 修改列表项的选中状态
    changeStatus(state, param) {
      const i = state.list.findIndex(x => x.id === param.id)
      if (i !== -1) {
        state.list[i].done = param.status
      }
    },
    // 清除已完成的任务
    cleanDone(state) {
      state.list = state.list.filter(x => x.done === false)
    },
    // 修改视图的关键字
    changeViewKey(state, key) {
      state.viewKey = key
    }
  },
  actions: {
    getList(context) {
      axios.get('/list.json').then(({ data }) => {
        // console.log(data)
        context.commit('initList', data)
      })
    }
  },
  getters: {
    // 统计未完成的任务的条数
    unDoneLength(state) {
      return state.list.filter(x => x.done === false).length
    },
    infolist(state) {
      if (state.viewKey === 'all') {
        return state.list
      }
      if (state.viewKey === 'undone') {
        return state.list.filter(x => !x.done)
      }
      if (state.viewKey === 'done') {
        return state.list.filter(x => x.done)
      }
      return state.list
    }
  }
})

main.js
import Vue from 'vue'
import App from './App.vue'

// 1. 导入 ant-design-vue 组件库
import Antd from 'ant-design-vue'
// 2. 导入组件库的样式表
import 'ant-design-vue/dist/antd.css'
import store from './store.js'

Vue.config.productionTip = false
// 3. 安装组件库
Vue.use(Antd)

new Vue({
  render: h => h(App),
  store
}).$mount('#app')
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复
vue2即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
