# Vue3
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714183459870.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 1 认识Vue3
```markdown
1) 了解相关信息
Vue.js 3.0 "One Piece" 正式版在今年9月份发布
2年多开发, 100+位贡献者, 2600+次提交, 600+次PR
Vue3支持vue2的大多数特性
更好的支持Typescript

2) 性能提升:
打包大小减少41%
初次渲染快55%, 更新渲染快133%
内存减少54%
使用Proxy代替defineProperty实现数据响应式
重写虚拟DOM的实现和Tree-Shaking
3) 新增特性
Composition (组合) API

setup
	ref 和 reactive
	computed 和 watch
	新的生命周期函数
	provide与inject
	
新组件
	Fragment - 文档碎片
	Teleport - 瞬移组件的位置
	Suspense - 异步加载组件的loading界面
	
其它API更新
	全局API的修改
	将原来的全局API转移到应用对象
	模板语法变化
```
## 2 创建vue3项目
###  2.1 使用 vue-cli 创建
```markdown
安装
npm install -g @vue/cli
创建项目
vue create my-project
```
###  2.2 使用 vite 创建
```markdown
安装vue-cli到最新版本 (必须高于4.5.0)
npm init vite-app   <project-name>
cd  <project-name>
npm install 
npm run dev
```
## 3 Composition API的使用

```markdown
Options API 选项api
Options API的优点是容易学习和使用，代码有明确的书写位置
Options API的缺点就是相似逻辑不容易复用，在大项目中尤为明显。
Options API可以通过mixins提取相同的逻辑，但是容易发生命名
冲突且来源不清晰

Composition API 组合API
Composition API是根据逻辑功能来组织代码的，一个功能所
有的api放到一起
即便项目很大，功能很多，都能够快速的定位到该功能所有的API
Composition API提高了代码可读性和可维护性
Vue3.0中推荐使用composition API,也保留了options API。
```

### 3.1 setup
```markdown
Setup函数是一个新的组件选项,作为组件中composition
API的起点
从生命周期钩子的角度来看，setup会在beforeCreate
钩子函数之前执行。Setup中不能使用this，this指向undefined
新的option, 所有的组合API函数都在此使用, 只在初始化时
执行一次
函数如果返回对象, 对象中的属性或方法, 模板中可以直接使用
```
```html
<template>
  <div class="app"></div>
</template>

<script>
export default {
  setup() {
    // composition api的入口
    // 不能访问this
    console.log('setup执行了')
    console.log(this)
  },
  beforeCreate() {
    console.log('beforeCreate')
  }
}
</script>

<style></style>

```
#### 3.1.1 setup细节
```markdown
setup执行的时机

在beforeCreate之前执行(一次), 此时组件对象还没有创建
this是undefined, 不能通过this来访问
data/computed/methods / props
其实所有的composition API相关回调函数中也都不可以

setup的返回值

一般都返回一个对象: 为模板提供数据, 也就是模板中可以直接使
用此对象中的所有属性/方法
返回对象中的属性会与data函数返回对象的属性合并成为
组件对象的属性
返回对象中的方法会与methods中的方法合并成功组件
对象的方法

如果有重名, setup优先
注意:
一般不要混合使用: methods中可以访问setup提供的属性
和方法, 但在setup方法中不能访问data和methods
setup不能是一个async函数: 因为返回值不再是return的对象, 
而是promise, 模板看不到return对象中的属性数据


setup的参数

setup(props, context) / setup(props, {attrs, slots, emit})
props: 包含props配置声明且传入了的所有属性的对象
attrs: 包含没有在props配置中声明的属性的对象, 
相当于 this.$attrs
slots: 包含所有传入的插槽内容的对象, 相当于 this.$slots
emit: 用来分发自定义事件的函数, 相当于 this.$emit
```
```html
<template>
  <h2>App</h2>
  <p>msg: {{msg}}</p>
  <button @click="fn('--')">更新</button>

  <child :msg="msg" msg2="cba" @fn="fn"/>
</template>

<script lang="ts">
import {
  reactive,
  ref,
} from 'vue'
import child from './child.vue'

export default {

  components: {
    child
  },

  setup () {
    const msg = ref('abc')

    function fn (content: string) {
      msg.value += content
    }
    return {
      msg,
      fn
    }
  }
}
</script>
```
```html
<template>
  <div>
    <h3>{{n}}</h3>
    <h3>{{m}}</h3>

    <h3>msg: {{msg}}</h3>
    <h3>msg2: {{$attrs.msg2}}</h3>

    <slot name="xxx"></slot>

    <button @click="update">更新</button>
  </div>
</template>

<script lang="ts">

import {
  ref,
  defineComponent
} from 'vue'

export default defineComponent({
  name: 'child',

  props: ['msg'],

  emits: ['fn'], // 可选的, 声明了更利于程序员阅读, 且可以对分发的事件数据进行校验

  data () {
    console.log('data', this)
    return {
      // n: 1
    }
  },

  beforeCreate () {
    console.log('beforeCreate', this)
  },

  methods: {
    // update () {
    //   this.n++
    //   this.m++
    // }
  },

  // setup (props, context) {
  setup (props, {attrs, emit, slots}) {

    console.log('setup', this)
    console.log(props.msg, attrs.msg2, slots, emit)

    const m = ref(2)
    const n = ref(3)

    function update () {
      // console.log('--', this)
      // this.n += 2 
      // this.m += 2

      m.value += 2
      n.value += 2

      // 分发自定义事件
      emit('fn', '++')
    }

    return {
      m,
      n,
      update,
    }
  },
})
</script>
```
### 3.2  ref

```markdown
ref函数接受一个简单类型的值，返回一个可改变的
ref对象。返回的对象有唯一的属性 value
在setup函数中，通过ref对象的value属性可以访问到值
在模板中，ref属性会自动解套，不需要额外的.value
如果ref接受的是一个对象，会自动调用reactive
```
```html
<template>
  <h2>{{count}}</h2>
  <hr>
  <button @click="update">更新</button>
</template>

<script>
import {
  ref
} from 'vue'
export default {

  /* 在Vue3中依然可以使用data和methods配置, 但建议使用其新语法实现 */
  // data () {
  //   return {
  //     count: 0
  //   }
  // },
  // methods: {
  //   update () {
  //     this.count++
  //   }
  // }

  /* 使用vue3的composition API */
  setup () {

    // 定义响应式数据 ref对象
    const count = ref(1)
    console.log(count)

    // 更新响应式数据的函数
    function update () {
      // alert('update')
      count.value = count.value + 1
    }

    return {
      count,
      update
    }
  }
}
</script>
```
#### 3.2.1 模板refs
```markdown
为了获得对模板内元素或组件实例的引用，我们可
以像往常一样在 setup() 中声明一个 ref 并返回它
```
```html
<template>
  <div class="app">
    <h1 ref="hRef">钩子函数----123</h1>

    <Demo ref="dRef"></Demo>
  </div>
</template>

<script>
import Demo from './Demo.vue'
import { ref, provide, onMounted } from 'vue'
export default {
  components: {
    Demo
  },
  setup() {
    // 创建了一个空的ref
    const hRef = ref(null)
    const dRef = ref(null)

    onMounted(() => {
      console.log(hRef.value.innerHTML)
      console.log(dRef.value)
    })

    return {
      hRef,
      dRef
    }
  }
}
</script>

<style></style>
```
### 3.3 reactive
```markdown
Reactive函数接受一个普通对象，返回该对象的响应式代理。
```
```html
<template>
  <div class="app">
    <div>{{ car.brand }}----{{ car.price }}</div>
    <button @click="car.brand = '奔驰'">修改</button>
  </div>
</template>

<script>
import { reactive } from 'vue'
export default {
  setup() {
    // 1. setup需要返回值， setup中返回的值才能在模板中使用
    // 2. reactive中传入一个普通对象，返回一个代理对象
    // 3. 普通对象没有响应式，需要reactive
    const car = reactive({
      brand: '宝马',
      price: 100
    })

    return {
      car
    }
  }
}
</script>

<style></style>

```
### 3.4 Vue2.0和vue3.0响应式原理对比
#### 3.4.1 Vue2.0响应式原理
```markdown
Vue2.0中
响应式数据
核心:
对象: 使用ES5中的Object.defineProperty方法实现
对对象的已有属性值的读取和修改进行劫持(监视/拦截)
数组: 通过重写数组更新数组一系列更新元素的方法来实
现元素修改的劫持
缺点
对象直接新添加的属性或删除已有属性, 界面不会自动更新
直接通过下标替换元素或更新length, 界面不会自动更新 arr[1] = {}

解决方案
Vue2.0提供Vue.set方法用于动态给对象添加属性
Vue2.0提供Vue.delete方法用于动态删除对象的属性
重写vue中数组的方法，用于监测数组的变更
```

```markdown
Object.defineproperty
```
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <script>
      // vue会做一个数据劫持，，，监视数据的变化，一旦数据变化了，更新DOM
      const data = {
        name: 'zs',
        age: 18
      }

      for (let k in data) {
        let temp = data[k]
        Object.defineProperty(data, k, {
          get() {
            console.log(`我劫持了${k}的获取`)
            return temp
          },
          set(value) {
            console.log(`我劫持了${k}的设置，值${value}`)
            temp = value
          }
        })
      }

      // Object.defineProperty有缺点
      // 1. 无法监视到新增的属性的变更和删除
      // 2. 无法劫持到数组的下标和长度
    </script>
  </body>
</html>

```
#### 3.4.2 Vue3.0响应式原理
```html
核心:
通过Proxy(代理): 拦截对data任意属性的任意(13种)操作, 
包括属性值的读写, 属性的添加, 属性的删除等...
通过 Reflect(反射): 动态对被代理对象的相应属性进行特
定的操作

优点
可以检测到代理对象属性的动态添加和删除
可以监测到数组的下标和length属性的变更

缺点
ES6的proxy语法对于低版本浏览器不支持，IE11
Vue3.0会针对于IE11出一个特殊的版本用于支持ie11
```
```markdown
es6的proxy语法.html
```
```js
new Proxy(data, {
	// 拦截读取属性值
    get (target, prop) {
    	return Reflect.get(target, prop)
    },
    // 拦截设置属性值或添加新属性
    set (target, prop, value) {
    	return Reflect.set(target, prop, value)
    },
    // 拦截删除属性
    deleteProperty (target, prop) {
    	return Reflect.deleteProperty(target, prop)
    }
})

proxy.name = 'tom'   
```
```markdown
vue3中响应式原理
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Proxy 与 Reflect</title>
</head>
<body>
  <script>
    
    const user = {
      name: "John",
      age: 12
    };

    /* 
    proxyUser是代理对象, user是被代理对象
    后面所有的操作都是通过代理对象来操作被代理对象内部属性
    */
    const proxyUser = new Proxy(user, {

      get(target, prop) {
        console.log('劫持get()', prop)
        return Reflect.get(target, prop)
      },

      set(target, prop, val) {
        console.log('劫持set()', prop, val)
        return Reflect.set(target, prop, val); // (2)
      },

      deleteProperty (target, prop) {
        console.log('劫持delete属性', prop)
        return Reflect.deleteProperty(target, prop)
      }
    });
    // 读取属性值
    console.log(proxyUser===user)
    console.log(proxyUser.name, proxyUser.age)
    // 设置属性值
    proxyUser.name = 'bob'
    proxyUser.age = 13
    console.log(user)
    // 添加属性
    proxyUser.sex = '男'
    console.log(user)
    // 删除属性
    delete proxyUser.sex
    console.log(user)
  </script>
</body>
</html>
```
### 3.5 reactive与ref-细节
```markdown
是Vue3的 composition API中2个最重要的响应式API
ref用来处理基本类型数据, reactive用来处理对象
(递归深度响应式)
如果用ref对象/数组, 内部会自动将对象/数组转换
为reactive的代理对象
ref内部: 通过给value属性添加getter/setter来实
现对数据的劫持
reactive内部: 通过使用Proxy来实现对对象内部所有
数据的劫持, 并通过Reflect操作对象内部数据
ref的数据操作: 在js中要.value, 在模板中不需
要(内部解析模板时会自动添加.value)
```
```html
<template>
  <h2>App</h2>
  <p>m1: {{m1}}</p>
  <p>m2: {{m2}}</p>
  <p>m3: {{m3}}</p>
  <button @click="update">更新</button>
</template>

<script lang="ts">
import {
  reactive,
  ref
} from 'vue'

export default {

  setup () {
    const m1 = ref('abc')
    const m2 = reactive({x: 1, y: {z: 'abc'}})

    // 使用ref处理对象  ==> 对象会被自动reactive为proxy对象
    const m3 = ref({a1: 2, a2: {a3: 'abc'}})
    console.log(m1, m2, m3)
    console.log(m3.value.a2) // 也是一个proxy对象

    function update() {
      m1.value += '--'
      m2.x += 1
      m2.y.z += '++'

      m3.value = {a1: 3, a2: {a3: 'abc---'}}
      m3.value.a2.a3 += '==' // reactive对对象进行了深度数据劫持
      console.log(m3.value.a2)
    }

    return {
      m1,
      m2,
      m3,
      update
    }
  }
}
</script>
```
### 3.6 计算属性与监视
```markdown
computed函数:

与computed配置功能一致
只有getter
有getter和setter
watch函数

与watch配置功能一致
监视指定的一个或多个响应式数据, 一旦数据变化, 
就自动执行监视回调
默认初始时不执行回调, 但可以通过配置immediate为true, 
来指定初始时立即执行第一次
通过配置deep为true, 来指定深度监视

watchEffect函数
不用直接指定要监视的数据, 回调函数中使用的哪些响应式
数据就监视哪些响应式数据
默认初始时就会执行第一次, 从而可以收集需要监视的数据
监视数据发生变化时回调
```
```html
<template>
  <h2>App</h2>
  fistName: <input v-model="user.firstName"/><br>
  lastName: <input v-model="user.lastName"/><br>
  fullName1: <input v-model="fullName1"/><br>
  fullName2: <input v-model="fullName2"><br>
  fullName3: <input v-model="fullName3"><br>

</template>

<script lang="ts">
/*
计算属性与监视
1. computed函数: 
  与computed配置功能一致
  只有getter
  有getter和setter
2. watch函数
  与watch配置功能一致
  监视指定的一个或多个响应式数据, 一旦数据变化, 就自动执行监视回调
  默认初始时不执行回调, 但可以通过配置immediate为true, 来指定初始时立即执行第一次
  通过配置deep为true, 来指定深度监视
3. watchEffect函数
  不用直接指定要监视的数据, 回调函数中使用的哪些响应式数据就监视哪些响应式数据
  默认初始时就会执行第一次, 从而可以收集需要监视的数据
  监视数据发生变化时回调
*/

import {
  reactive,
  ref,
  computed,
  watch,
  watchEffect
} from 'vue'

export default {

  setup () {
    const user = reactive({
      firstName: 'A',
      lastName: 'B'
    })

    // 只有getter的计算属性
    const fullName1 = computed(() => {
      console.log('fullName1')
      return user.firstName + '-' + user.lastName
    })

    // 有getter与setter的计算属性
    const fullName2 = computed({
      get () {
        console.log('fullName2 get')
        return user.firstName + '-' + user.lastName
      },

      set (value: string) {
        console.log('fullName2 set')
        const names = value.split('-')
        user.firstName = names[0]
        user.lastName = names[1]
      }
    })

    const fullName3 = ref('')

    /* 
    watchEffect: 监视所有回调中使用的数据
    */
    /* 
    watchEffect(() => {
      console.log('watchEffect')
      fullName3.value = user.firstName + '-' + user.lastName
    }) 
    */

    /* 
    使用watch的2个特性:
      深度监视
      初始化立即执行
    */
    watch(user, () => {
      fullName3.value = user.firstName + '-' + user.lastName
    }, {
      immediate: true,  // 是否初始化立即执行一次, 默认是false
      deep: true, // 是否是深度监视, 默认是false
    })

    /* 
    watch一个数据
      默认在数据发生改变时执行回调
    */
    watch(fullName3, (value) => {
      console.log('watch')
      const names = value.split('-')
      user.firstName = names[0]
      user.lastName = names[1]
    })

    /* 
    watch多个数据: 
      使用数组来指定
      如果是ref对象, 直接指定
      如果是reactive对象中的属性,  必须通过函数来指定
    */
    watch([() => user.firstName, () => user.lastName, fullName3], (values) => {
      console.log('监视多个数据', values)
    })

    return {
      user,
      fullName1,
      fullName2,
      fullName3
    }
  }
}
</script>
```
### 3.7 生命周期
```markdown
与 2.x 版本生命周期相对应的组合式 API
beforeCreate -> 使用 setup()
created -> 使用 setup()
beforeMount -> onBeforeMount
mounted -> onMounted
beforeUpdate -> onBeforeUpdate
updated -> onUpdated
beforeDestroy -> onBeforeUnmount
destroyed -> onUnmounted
errorCaptured -> onErrorCaptured


新增的钩子函数

组合式 API 还提供了以下调试钩子函数：

onRenderTracked
onRenderTriggered

```
```html
<template>
<div class="about">
  <h2>msg: {{msg}}</h2>
  <hr>
  <button @click="update">更新</button>
</div>
</template>

<script lang="ts">
import {
  ref,
  onMounted,
  onUpdated,
  onUnmounted, 
  onBeforeMount, 
  onBeforeUpdate,
  onBeforeUnmount
} from "vue"

export default {
  beforeCreate () {
    console.log('beforeCreate()')
  },

  created () {
    console.log('created')
  },

  beforeMount () {
    console.log('beforeMount')
  },

  mounted () {
    console.log('mounted')
  },

  beforeUpdate () {
    console.log('beforeUpdate')
  },

  updated () {
    console.log('updated')
  },

  beforeUnmount () {
    console.log('beforeUnmount')
  },

  unmounted () {
     console.log('unmounted')
  },
  

  setup() {
    
    const msg = ref('abc')

    const update = () => {
      msg.value += '--'
    }

    onBeforeMount(() => {
      console.log('--onBeforeMount')
    })

    onMounted(() => {
      console.log('--onMounted')
    })

    onBeforeUpdate(() => {
      console.log('--onBeforeUpdate')
    })

    onUpdated(() => {
      console.log('--onUpdated')
    })

    onBeforeUnmount(() => {
      console.log('--onBeforeUnmount')
    })

    onUnmounted(() => {
      console.log('--onUnmounted')
    })
    
    return {
      msg,
      update
    }
  }
}
</script>
```
```html
<template>
  <h2>App</h2>
  <button @click="isShow=!isShow">切换</button>
  <hr>
  <Child v-if="isShow"/>
</template>

<script lang="ts">
import Child from './Child.vue'
export default {

  data () {
    return {
      isShow: true
    }
  },

  components: {
    Child
  }
}
</script>
```
### 3.8 自定义hook函数
```html
使用Vue3的组合API封装的可复用的功能函数

自定义hook的作用类似于vue2中的mixin技术

自定义Hook的优势: 很清楚复用功能代码的来源, 更清楚易懂

需求1: 收集用户鼠标点击的页面坐标
```
```ts
hooks/useMousePosition.ts

import { ref, onMounted, onUnmounted } from 'vue'
/* 
收集用户鼠标点击的页面坐标
*/
export default function useMousePosition () {
  // 初始化坐标数据
  const x = ref(-1)
  const y = ref(-1)

  // 用于收集点击事件坐标的函数
  const updatePosition = (e: MouseEvent) => {
    x.value = e.pageX
    y.value = e.pageY
  }

  // 挂载后绑定点击监听
  onMounted(() => {
    document.addEventListener('click', updatePosition)
  })

  // 卸载前解绑点击监听
  onUnmounted(() => {
    document.removeEventListener('click', updatePosition)
  })

  return {x, y}
}

<template>
<div>
  <h2>x: {{x}}, y: {{y}}</h2>
</div>
</template>

<script>

import {
  ref
} from "vue"
/* 
在组件中引入并使用自定义hook
自定义hook的作用类似于vue2中的mixin技术
自定义Hook的优势: 很清楚复用功能代码的来源, 更清楚易懂
*/
import useMousePosition from './hooks/useMousePosition'

export default {
  setup() {

    const {x, y} = useMousePosition()

    return {
      x,
      y,
    }
  }
}
</script>
```
```ts
利用TS泛型强化类型检查

需求2: 封装发ajax请求的hook函数

hooks/useRequest.ts

import { ref } from 'vue'
import axios from 'axios'

/* 
使用axios发送异步ajax请求
*/
export default function useUrlLoader<T>(url: string) {

  const result = ref<T | null>(null)
  const loading = ref(true)
  const errorMsg = ref(null)

  axios.get(url)
    .then(response => {
      loading.value = false
      result.value = response.data
    })
    .catch(e => {
      loading.value = false
      errorMsg.value = e.message || '未知错误'
    })

  return {
    loading,
    result,
    errorMsg,
  }
}
```
```ts
<template>
<div class="about">
  <h2 v-if="loading">LOADING...</h2>
  <h2 v-else-if="errorMsg">{{errorMsg}}</h2>
  <!-- <ul v-else>
    <li>id: {{result.id}}</li>
    <li>name: {{result.name}}</li>
    <li>distance: {{result.distance}}</li>
  </ul> -->

  <ul v-for="p in result" :key="p.id">
    <li>id: {{p.id}}</li>
    <li>title: {{p.title}}</li>
    <li>price: {{p.price}}</li>
  </ul>
  <!-- <img v-if="result" :src="result[0].url" alt=""> -->
</div>
</template>

<script lang="ts">
import {
  watch
} from "vue"
import useRequest from './hooks/useRequest'

// 地址数据接口
interface AddressResult {
  id: number;
  name: string;
  distance: string;
}

// 产品数据接口
interface ProductResult {
  id: string;
  title: string;
  price: number;
}

export default {
  setup() {

    // const {loading, result, errorMsg} = useRequest<AddressResult>('/data/address.json')
    const {loading, result, errorMsg} = useRequest<ProductResult[]>('/data/products.json')

    watch(result, () => {
      if (result.value) {
        console.log(result.value.length) // 有提示
      }
    })

    return {
      loading,
      result, 
      errorMsg
    }
  }
}
</script>
```
### 3.9 toRefs

```markdown
把一个响应式对象转换成普通对象，该普通对象的
每个 property 都是一个 ref
Reactive的响应式功能是赋予给对象的，但是如果给
对象解构或者展开的时候，会让数据丢失响应式的能力。
使用toRefs可以保证该对象展开的每一个属性都是响应式的
```
```html
<template>
  <div class="app">
    <div>我的金钱：{{ money }}</div>
    <div>{{ car.brand }} --- {{ car.price }}</div>
    <div>{{ name }}</div>
    <button @click="money++">修改</button>
    <button @click="name = 'ls'">修改</button>
  </div>
</template>

<script>
import { reactive, ref, toRefs } from 'vue'
export default {
  setup() {
    // 1. toRefs
    // const money = ref(100)
    // const car = reactive({
    //   brand: '宝马',
    //   price: 1000000
    // })
    // const name = ref('zs')

    const state = reactive({
      money: 100,
      car: {
        brand: '宝马',
        price: 1000000
      },
      name: 'zs'
    })

    return {
      // money,
      // car,
      // name
      ...toRefs(state)
    }
  }
}
</script>

<style></style>
```
### 3.10 shallowReactive 与 shallowRef
```ts
shallowReactive : 只处理了对象内最外层属性的响应式
(也就是浅响应式)

shallowRef: 只处理了value的响应式, 不进行对
象的reactive处理

什么时候用浅响应式呢?

一般情况下使用ref和reactive即可
如果有一个对象数据, 结构比较深, 但变化时
只是外层属性变化 ===> shallowReactive
如果有一个对象数据, 后面会产生新的对象来
替换 ===> shallowRef

<template>
  <h2>App</h2>

  <h3>m1: {{m1}}</h3>
  <h3>m2: {{m2}}</h3>
  <h3>m3: {{m3}}</h3>
  <h3>m4: {{m4}}</h3>

  <button @click="update">更新</button>
</template>

<script lang="ts">
import { reactive, ref, shallowReactive, shallowRef } from 'vue'
/* 
shallowReactive与shallowRef
  shallowReactive: 只处理了对象内最外层属性的响应式(也就是浅响应式)
  shallowRef: 只处理了value的响应式, 不进行对象的reactive处理
总结:
  reactive与ref实现的是深度响应式, 而shallowReactive与shallowRef是浅响应式
  什么时候用浅响应式呢?
    一般情况下使用ref和reactive即可,
    如果有一个对象数据, 结构比较深, 但变化时只是外层属性变化 ===> shallowReactive
    如果有一个对象数据, 后面会产生新的对象来替换 ===> shallowRef
*/

export default {

  setup () {

    const m1 = reactive({a: 1, b: {c: 2}})
    const m2 = shallowReactive({a: 1, b: {c: 2}})

    const m3 = ref({a: 1, b: {c: 2}})
    const m4 = shallowRef({a: 1, b: {c: 2}})

    const update = () => {
      // m1.b.c += 1
      // m2.b.c += 1

      // m3.value.a += 1
      m4.value.a += 1
    }

    return {
      m1,
      m2,
      m3,
      m4,
      update,
    }
  }
}
</script>
```
### 3.11 readonly 与 shallowReadonly
```ts
readonly:
深度只读数据
获取一个对象 (响应式或纯对象) 或 ref 并返回原
始代理的只读代理。
只读代理是深层的：访问的任何嵌套 property 也是只读的。
shallowReadonly
浅只读数据
创建一个代理，使其自身的 property 为只读，但
不执行嵌套对象的深度只读转换
应用场景:
在某些特定情况下, 我们可能不希望对数据进行
更新的操作, 那就可以包装生成一个只读代理对
象来读取数据, 而不能修改或删除
```
```ts
<template>
  <h2>App</h2>
  <h3>{{state}}</h3>
  <button @click="update">更新</button>
</template>

<script lang="ts">
import { reactive, readonly, shallowReadonly } from 'vue'
/*
readonly: 深度只读数据
  获取一个对象 (响应式或纯对象) 或 ref 并返回原始代理的只读代理。
  只读代理是深层的：访问的任何嵌套 property 也是只读的。
shallowReadonly: 浅只读数据
  创建一个代理，使其自身的 property 为只读，但不执行嵌套对象的深度只读转换 
应用场景: 
  在某些特定情况下, 我们可能不希望对数据进行更新的操作, 那就可以包装生成一个只读代理对象来读取数据, 而不能修改或删除
*/

export default {

  setup () {

    const state = reactive({
      a: 1,
      b: {
        c: 2
      }
    })

    // const rState1 = readonly(state)
    const rState2 = shallowReadonly(state)

    const update = () => {
      // rState1.a++ // error
      // rState1.b.c++ // error

      // rState2.a++ // error
      rState2.b.c++
    }
    
    return {
      state,
      update
    }
  }
}
</script>
```
### 3.12 toRaw 与 markRaw
```ts
toRaw
返回由 reactive 或 readonly 方法转换成响应式代
理的普通对象。
这是一个还原方法，可用于临时读取，访问不会被代
理/跟踪，写入时也不会触发界面更新。
markRaw
标记一个对象，使其永远不会转换为代理。返回对象本身
应用场景:
有些值不应被设置为响应式的，例如复杂的第三方类实例
或 Vue 组件对象。
当渲染具有不可变数据源的大列表时，跳过代理转换可以提高性能。

<template>
  <h2>{{state}}</h2>
  <button @click="testToRaw">测试toRaw</button>
  <button @click="testMarkRaw">测试markRaw</button>
</template>

<script lang="ts">
/* 
toRaw: 得到reactive代理对象的目标数据对象
*/
import {
  markRaw,
  reactive, toRaw,
} from 'vue'
export default {
  setup () {
    const state = reactive<any>({
      name: 'tom',
      age: 25,
    })

    const testToRaw = () => {
      const user = toRaw(state)
      user.age++  // 界面不会更新

    }

    const testMarkRaw = () => {
      const likes = ['a', 'b']
      // state.likes = likes
      state.likes = markRaw(likes) // likes数组就不再是响应式的了
      setTimeout(() => {
        state.likes[0] += '--'
      }, 1000)
    }

    return {
      state,
      testToRaw,
      testMarkRaw,
    }
  }
}
</script>
```
### 3.13 toRef
```ts
为源响应式对象上的某个属性创建一个 ref对象, 
二者内部操作的是同一个数据值, 更新时二者是同步的
区别ref: 拷贝了一份新的数据值单独操作, 更新时相互不影响
应用: 当要将 某个prop 的 ref 传递给复合函数时，toRef 
很有用
```
```ts
<template>
  <h2>App</h2>
  <p>{{state}}</p>
  <p>{{foo}}</p>
  <p>{{foo2}}</p>

  <button @click="update">更新</button>

  <Child :foo="foo"/>
</template>

<script lang="ts">
/*
toRef:
  为源响应式对象上的某个属性创建一个 ref对象, 二者内部操作的是同一个数据值, 更新时二者是同步的
  区别ref: 拷贝了一份新的数据值单独操作, 更新时相互不影响
  应用: 当要将某个 prop 的 ref 传递给复合函数时，toRef 很有用
*/

import {
  reactive,
  toRef,
  ref,
} from 'vue'
import Child from './Child.vue'

export default {

  setup () {

    const state = reactive({
      foo: 1,
      bar: 2
    })

    const foo = toRef(state, 'foo')
    const foo2 = ref(state.foo)

    const update = () => {
      state.foo++
      // foo.value++
      // foo2.value++  // foo和state中的数据不会更新
    }

    return {
      state,
      foo,
      foo2,
      update,
    }
  },

  components: {
    Child
  }
}
</script>
```
```ts
<template>
  <h2>Child</h2>
  <h3>{{foo}}</h3>
  <h3>{{length}}</h3>
</template>

<script lang="ts">
import { computed, defineComponent, Ref, toRef } from 'vue'

const component = defineComponent({
  props: {
    foo: {
      type: Number,
      require: true
    }
  },

  setup (props, context) {
    const length = useFeatureX(toRef(props, 'foo'))

    return {
      length
    }
  }
})

function useFeatureX(foo: Ref) {
  const lenth = computed(() => foo.value.length)

  return lenth
}

export default component
</script>
```
### 3.14 customRef
```ts
创建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显式控制
需求: 使用 customRef 实现 debounce 的示例
```
```ts
<template>
  <h2>App</h2>
  <input v-model="keyword" placeholder="搜索关键字"/>
  <p>{{keyword}}</p>
</template>

<script lang="ts">
/*
customRef:
  创建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显式控制

需求: 
  使用 customRef 实现 debounce 的示例
*/

import {
  ref,
  customRef
} from 'vue'

export default {

  setup () {
    const keyword = useDebouncedRef('', 500)
    console.log(keyword)
    return {
      keyword
    }
  },
}

/* 
实现函数防抖的自定义ref
*/
function useDebouncedRef<T>(value: T, delay = 200) {
  let timeout: number
  return customRef((track, trigger) => {
    return {
      get() {
        // 告诉Vue追踪数据
        track()
        return value
      },
      set(newValue: T) {
        clearTimeout(timeout)
        timeout = setTimeout(() => {
          value = newValue
          // 告诉Vue去触发界面更新
          trigger()
        }, delay)
      }
    }
  })
}

</script>
```
### 3.15 provide 与 inject
```ts
provide和inject提供依赖注入，功能类似 2.x 
的provide/inject

实现跨层级组件（祖孙）间通信

<template>
  <h1>父组件</h1>
  <p>当前颜色: {{color}}</p>
  <button @click="color='red'">红</button>
  <button @click="color='yellow'">黄</button>
  <button @click="color='blue'">蓝</button>
  
  <hr>
  <Son />
</template>

<script lang="ts">
import { provide, ref } from 'vue'
/* 
- provide` 和 `inject` 提供依赖注入，功能类似 2.x 的 `provide/inject
- 实现跨层级组件(祖孙)间通信
*/

import Son from './Son.vue'
export default {
  name: 'ProvideInject',
  components: {
    Son
  },
  setup() {
    
    const color = ref('red')

    provide('color', color)

    return {
      color
    }
  }
}
</script>
```
```ts
<template>
  <div>
    <h2>子组件</h2>
    <hr>
    <GrandSon />
  </div>
</template>

<script lang="ts">
import GrandSon from './GrandSon.vue'
export default {
  components: {
    GrandSon
  },
}
</script>
```
```ts
<template>
  <h3 :style="{color}">孙子组件: {{color}}</h3>
  
</template>

<script lang="ts">
import { inject } from 'vue'
export default {
  setup() {
    const color = inject('color')

    return {
      color
    }
  }
}
</script>
```
### 3.16 响应式数据的判断
```ts
isRef: 检查一个值是否为一个 ref 对象
isReactive: 检查一个对象是否是由 reactive 
创建的响应式代理
isReadonly: 检查一个对象是否是由readonly
创建的只读代理
isProxy: 检查一个对象是否是由 reactive 或者 
readonly 方法创建的代理
```
### 3.17 手写组合API
#### 3.17.1 shallowReactive 与 reactive
```ts
const reactiveHandler = {
  get (target, key) {

    if (key==='_is_reactive') return true

    return Reflect.get(target, key)
  },

  set (target, key, value) {
    const result = Reflect.set(target, key, value)
    console.log('数据已更新, 去更新界面')
    return result
  },

  deleteProperty (target, key) {
    const result = Reflect.deleteProperty(target, key)
    console.log('数据已删除, 去更新界面')
    return result
  },
}

/* 
自定义shallowReactive
*/
function shallowReactive(obj) {
  return new Proxy(obj, reactiveHandler)
}

/* 
自定义reactive
*/
function reactive (target) {
  if (target && typeof target==='object') {
    if (target instanceof Array) { // 数组
      target.forEach((item, index) => {
        target[index] = reactive(item)
      })
    } else { // 对象
      Object.keys(target).forEach(key => {
        target[key] = reactive(target[key])
      })
    }

    const proxy = new Proxy(target, reactiveHandler)
    return proxy
  }

  return target
}


/* 测试自定义shallowReactive */
const proxy = shallowReactive({
  a: {
    b: 3
  }
})

proxy.a = {b: 4} // 劫持到了
proxy.a.b = 5 // 没有劫持到


/* 测试自定义reactive */
const obj = {
  a: 'abc',
  b: [{x: 1}],
  c: {x: [11]},
}

const proxy = reactive(obj)
console.log(proxy)
proxy.b[0].x += 1
proxy.c.x[0] += 1
```
#### 3.17.2 shallowRef 与 ref
```ts
/*
自定义shallowRef
*/
function shallowRef(target) {
  const result = {
    _value: target, // 用来保存数据的内部属性
    _is_ref: true, // 用来标识是ref对象
    get value () {
      return this._value
    },
    set value (val) {
      this._value = val
      console.log('set value 数据已更新, 去更新界面')
    }
  }

  return result
}

/* 
自定义ref
*/
function ref(target) {
  if (target && typeof target==='object') {
    target = reactive(target)
  }

  const result = {
    _value: target, // 用来保存数据的内部属性
    _is_ref: true, // 用来标识是ref对象
    get value () {
      return this._value
    },
    set value (val) {
      this._value = val
      console.log('set value 数据已更新, 去更新界面')
    }
  }

  return result
}

/* 测试自定义shallowRef */
const ref3 = shallowRef({
  a: 'abc',
})
ref3.value = 'xxx'
ref3.value.a = 'yyy'


/* 测试自定义ref */
const ref1 = ref(0)
const ref2 = ref({
  a: 'abc',
  b: [{x: 1}],
  c: {x: [11]},
})
ref1.value++
ref2.value.b[0].x++
console.log(ref1, ref2)
```
#### 3.17.3 shallowReadonly 与 readonly
```ts
const readonlyHandler = {
  get (target, key) {
    if (key==='_is_readonly') return true

    return Reflect.get(target, key)
  },

  set () {
    console.warn('只读的, 不能修改')
    return true
  },

  deleteProperty () {
    console.warn('只读的, 不能删除')
    return true
  },
}

/* 
自定义shallowReadonly
*/
function shallowReadonly(obj) {
  return new Proxy(obj, readonlyHandler)
}

/* 
自定义readonly
*/
function readonly(target) {
  if (target && typeof target==='object') {
    if (target instanceof Array) { // 数组
      target.forEach((item, index) => {
        target[index] = readonly(item)
      })
    } else { // 对象
      Object.keys(target).forEach(key => {
        target[key] = readonly(target[key])
      })
    }
    const proxy = new Proxy(target, readonlyHandler)

    return proxy 
  }

  return target
}

/* 测试自定义readonly */
/* 测试自定义shallowReadonly */
const objReadOnly = readonly({
  a: {
    b: 1
  }
})
const objReadOnly2 = shallowReadonly({
  a: {
    b: 1
  }
})

objReadOnly.a = 1
objReadOnly.a.b = 2
objReadOnly2.a = 1
objReadOnly2.a.b = 2
```
#### 3.17.4 isRef, isReactive 与 isReadonly
```ts
/* 
判断是否是ref对象
*/
function isRef(obj) {
  return obj && obj._is_ref
}

/* 
判断是否是reactive对象
*/
function isReactive(obj) {
  return obj && obj._is_reactive
}

/* 
判断是否是readonly对象
*/
function isReadonly(obj) {
  return obj && obj._is_readonly
}

/* 
是否是reactive或readonly产生的代理对象
*/
function isProxy (obj) {
  return isReactive(obj) || isReadonly(obj)
}


/* 测试判断函数 */
console.log(isReactive(reactive({})))
console.log(isRef(ref({})))
console.log(isReadonly(readonly({})))
console.log(isProxy(reactive({})))
console.log(isProxy(readonly({})))
```
### 3.18 Composition API VS Option API
```markdown
1) Option API的问题
在传统的Vue OptionsAPI中，新增或者修改一个需求，
就需要分别在data，methods，computed里修改 ，
滚动条反复上下移动

2) 使用Compisition API
我们可以更加优雅的组织我们的代码，函数。让相关功
能的代码更加有序的组织在一起
```
## 4 其它新组合和API
### 4.1 新组件
#### 4.1.1 Fragment(片断)
```ts
在Vue2中: 组件必须有一个根标签
在Vue3中: 组件可以没有根标签, 内部会将多个标签包含在
一个Fragment虚拟元素中
好处: 减少标签层级, 减小内存占用

<template>
    <h2>aaaa</h2>
    <h2>aaaa</h2>
</template>
```
#### 4.1.2 Teleport(瞬移)
```ts
Teleport 提供了一种干净的方法，让组件的html在
父组件界面外的特定标签 (很可能是body)下插入显示
```
```ts
ModalButton.vue
<template>
  <button @click="modalOpen = true">
      Open full screen modal! (With teleport!)
  </button>

  <teleport to="body">
    <div v-if="modalOpen" class="modal">
      <div>
        I'm a teleported modal! 
        (My parent is "body")
        <button @click="modalOpen = false">
          Close
        </button>
      </div>
    </div>
  </teleport>
</template>

<script>
import { ref } from 'vue'
export default {
  name: 'modal-button',
  setup () {
    const modalOpen = ref(false)
    return {
      modalOpen
    }
  }
}
</script>


<style>
.modal {
  position: absolute;
  top: 0; right: 0; bottom: 0; left: 0;
  background-color: rgba(0,0,0,.5);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.modal div {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: white;
  width: 300px;
  height: 300px;
  padding: 5px;
}
</style>
```
```ts
App.vue

<template>
  <h2>App</h2>
  <modal-button></modal-button>
</template>

<script lang="ts">
import ModalButton from './ModalButton.vue'

export default {
  setup() {
    return {
    }
  },

  components: {
    ModalButton
  }
}
</script>
```
#### 4.1.3 Suspense(不确定的)
```ts
们允许我们的应用程序在等待异步组件时渲染一些后备内容，
可以让我们创建一个平滑的用户体验
```
```ts
<template>
  <Suspense>
    <template v-slot:default>
      <AsyncComp/>
      <!-- <AsyncAddress/> -->
    </template>

    <template v-slot:fallback>
      <h1>LOADING...</h1>
    </template>
  </Suspense>
</template>

<script lang="ts">
/* 
异步组件 + Suspense组件
*/
// import AsyncComp from './AsyncComp.vue'
import AsyncAddress from './AsyncAddress.vue'
import { defineAsyncComponent } from 'vue'
const AsyncComp = defineAsyncComponent(() => import('./AsyncComp.vue'))
export default {
  setup() {
    return {
     
    }
  },

  components: {
    AsyncComp,
    AsyncAddress
  }
}
</script>
```
```ts
AsyncComp.vue
<template>
  <h2>AsyncComp22</h2>
  <p>{{msg}}</p>
</template>

<script lang="ts">

export default {
  name: 'AsyncComp',
  setup () {
    // return new Promise((resolve, reject) => {
    //   setTimeout(() => {
    //     resolve({
    //       msg: 'abc'
    //     })
    //   }, 2000)
    // })
    return {
      msg: 'abc'
    }
  }
}
</script>
```
```ts
AsyncAddress.vue

<template>
<h2>{{data}}</h2>
</template>

<script lang="ts">
import axios from 'axios'
export default {
  async setup() {
    const result = await axios.get('/data/address.json')
    return {
      data: result.data
    }
  }
}
</script>
```

### 4.2 其他新的API
```ts
全新的全局API
createApp()
defineProperty()
defineAsyncComponent()
nextTick()

将原来的全局API转移到应用对象
app.component()
app.config()
app.directive()
app.mount()
app.unmount()
app.use()

模板语法变化
v-model的本质变化
prop：value -> modelValue；
event：input -> update:modelValue；
.sync修改符已移除, 由v-model代替
v-if优先v-for解析
```
## 5 综合案例todolist
### 5.1 版本1
```html
<template>
  <section class="todoapp">
    <header class="header">
      <h1>todos</h1>
      <input
        class="new-todo"
        placeholder="What needs to be done?"
        autofocus
        v-model="todoName"
        @keyup.enter="addTodo"
      />
    </header>
    <!-- This section should be hidden by default and shown when there are todos -->
    <section class="main">
      <input
        id="toggle-all"
        class="toggle-all"
        type="checkbox"
        v-model="checkAll"
      />
      <label for="toggle-all">Mark all as complete</label>
      <ul class="todo-list">
        <li
          :class="{
            completed: item.done,
            editing: item.id === currentId
          }"
          v-for="item in list"
          :key="item.id"
        >
          <div class="view">
            <input class="toggle" type="checkbox" v-model="item.done" />
            <label @dblclick="showTodo(item.id, item.name)">{{
              item.name
            }}</label>
            <button class="destroy" @click="delTodo(item.id)"></button>
          </div>
          <input
            class="edit"
            v-model="currentName"
            @keyup.esc="currentId = ''"
            @keyup.enter="editTodo(item)"
          />
        </li>
      </ul>
    </section>
    <!-- This footer should hidden by default and shown when there are todos -->
    <footer class="footer" v-if="list.length > 0">
      <!-- This should be `0 items left` by default -->
      <span class="todo-count"
        ><strong>{{ leftCount }}</strong> item left</span
      >
      <!-- Remove this if you don't implement routing -->
      <ul class="filters">
        <li>
          <a class="selected" href="#/">All</a>
        </li>
        <li>
          <a href="#/active">Active</a>
        </li>
        <li>
          <a href="#/completed">Completed</a>
        </li>
      </ul>
      <!-- Hidden if no completed items are left ↓ -->
      <button class="clear-completed" v-if="isShow" @click="clearTodo">
        Clear completed
      </button>
    </footer>
  </section>
</template>

<script>
import { reactive, toRefs, computed, watch } from 'vue'
export default {
  setup() {
    const state = reactive({
      todoName: '',
      // 用于记录需要修改的任务的id
      currentId: '',
      // 用于记录需要修改的任务的名字
      currentName: '',
      // 需要完成的任务列表
      list: JSON.parse(localStorage.getItem('todos')) || []
    })

    const delTodo = (id) => {
      // console.log('删除', id)
      state.list = state.list.filter((item) => item.id !== id)
    }

    const addTodo = () => {
      const todo = {
        id: Date.now(),
        name: state.todoName,
        done: false
      }
      state.list.unshift(todo)
      state.todoName = ''
    }

    const showTodo = (id, name) => {
      state.currentId = id
      state.currentName = name
      console.log(state.currentId)
      console.log(state.currentName)
    }

    const editTodo = (todo) => {
      todo.name = state.currentName
      state.currentId = ''
    }

    const clearTodo = () => {
      state.list = state.list.filter((item) => !item.done)
    }

    const computedData = reactive({
      leftCount: computed(() => {
        return state.list.filter((item) => !item.done).length
      }),
      isShow: computed(() => {
        return state.list.some((item) => item.done)
      }),
      checkAll: computed({
        get: () => {
          return state.list.every((item) => item.done)
        },
        set: (value) => {
          state.list.forEach((item) => (item.done = value))
        }
      })
    })

    watch(
      () => state.list,
      (value) => {
        localStorage.setItem('todos', JSON.stringify(state.list))
      },
      {
        // 深度监听
        deep: true
      }
    )
    return {
      ...toRefs(state),
      ...toRefs(computedData),
      delTodo,
      addTodo,
      showTodo,
      editTodo,
      clearTodo
    }
  }
}
</script>
```
### 5.1 版本2
```markdown
组件层次关系
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021071417403974.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```html
Header.vue
<template>
  <div class="todo-header">
    <input
      type="text"
      placeholder="请输入你的任务名称，按回车键确认"
      v-model="title"
      @keyup.enter="add"
    />
  </div>
</template>
<script lang="ts">
import { defineComponent, ref } from 'vue'
export default defineComponent({
  name: 'Header',
  props: {
    addTodo: {
      type: Function,
      required: true, // 必须
    },
  },

  setup(props) {
    // 定义一个ref类型的数据
    const title = ref('')

    // 回车的事件的回调函数,用来添加数据
    const add = () => {
      // 获取文本框中输入的数据,判断不为空
      const text = title.value
      if (!text.trim()) return
      // 此时有数据,创建一个todo对象
      const todo = {
        id: Date.now(),
        title: text,
        isCompleted: false,
      }
      // 调用方法addTodo的方法
      props.addTodo(todo)
      // 清空文本框
      title.value = ''
    }
    return {
      title,
      add,
    }
  },
})
</script>
<style scoped>
/*header*/
.todo-header input {
  width: 560px;
  height: 28px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 7px;
}

.todo-header input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
    0 0 8px rgba(82, 168, 236, 0.6);
}
</style>

```
```html
List.vue
<template>
  <ul class="todo-main">
    <Item
      v-for="(todo, index) in todos"
      :key="todo.id"
      :todo="todo"
      :deleteTodo="deleteTodo"
      :updateTodo="updateTodo"
      :index="index"
    />
  </ul>
</template>
<script lang="ts">
import { defineComponent } from 'vue'
// 引入子级组件
import Item from './Item.vue'
export default defineComponent({
  name: 'List',
  components: {
    Item,
  },
  props: ['todos', 'deleteTodo', 'updateTodo'],
})
</script>
<style scoped>
/*main*/
.todo-main {
  margin-left: 0px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0px;
}

.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
}
</style>

```
```html
Item.vue
<template>
  <li
    @mouseenter="mouseHandler(true)"
    @mouseleave="mouseHandler(false)"
    :style="{ backgroundColor: bgColor, color: myColor }"
  >
    <label>
      <input type="checkbox" v-model="isComptete" />
      <span>{{ todo.title }}</span>
    </label>
    <button class="btn btn-danger" v-show="isShow" @click="delTodo">
      删除
    </button>
  </li>
</template>
<script lang="ts">
import { defineComponent, ref, computed } from 'vue'
// 引入接口
import { Todo } from '../types/todo'
export default defineComponent({
  name: 'Item',
  props: {
    todo: {
      type: Object as () => Todo, // 函数返回的是Todo类型
      required: true,
    },
    deleteTodo: {
      type: Function,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    },
    updateTodo: {
      type: Function,
      required: true,
    },
  },

  setup(props) {
    // 背景色
    const bgColor = ref('white')
    // 前景色
    const myColor = ref('black')
    // 设置按钮默认不显示
    const isShow = ref(false)
    // 鼠标进入和离开事件的回调函数
    const mouseHandler = (flag: boolean) => {
      if (flag) {
        // 鼠标进入
        bgColor.value = 'pink'
        myColor.value = 'green'
        isShow.value = true
      } else {
        // 鼠标离开
        bgColor.value = 'white'
        myColor.value = 'black'
        isShow.value = false
      }
    }
    // 删除数据的方法
    const delTodo = () => {
      // 提示
      if (window.confirm('确定要删除吗?')) {
        props.deleteTodo(props.index)
      }
    }
    // 计算属性的方式---来让当前的复选框选中/不选中
    const isComptete = computed({
      get() {
        return props.todo.isCompleted
      },
      set(val) {
        props.updateTodo(props.todo, val)
      },
    })
    return {
      mouseHandler,
      bgColor,
      myColor,
      isShow,
      delTodo,
      isComptete,
    }
  },
})
</script>
<style scoped>
/*item*/
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  /* display: none; */
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}
</style>

```
```html
Footer.vue
<template>
  <li
    @mouseenter="mouseHandler(true)"
    @mouseleave="mouseHandler(false)"
    :style="{ backgroundColor: bgColor, color: myColor }"
  >
    <label>
      <input type="checkbox" v-model="isComptete" />
      <span>{{ todo.title }}</span>
    </label>
    <button class="btn btn-danger" v-show="isShow" @click="delTodo">
      删除
    </button>
  </li>
</template>
<script lang="ts">
import { defineComponent, ref, computed } from 'vue'
// 引入接口
import { Todo } from '../types/todo'
export default defineComponent({
  name: 'Item',
  props: {
    todo: {
      type: Object as () => Todo, // 函数返回的是Todo类型
      required: true,
    },
    deleteTodo: {
      type: Function,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    },
    updateTodo: {
      type: Function,
      required: true,
    },
  },

  setup(props) {
    // 背景色
    const bgColor = ref('white')
    // 前景色
    const myColor = ref('black')
    // 设置按钮默认不显示
    const isShow = ref(false)
    // 鼠标进入和离开事件的回调函数
    const mouseHandler = (flag: boolean) => {
      if (flag) {
        // 鼠标进入
        bgColor.value = 'pink'
        myColor.value = 'green'
        isShow.value = true
      } else {
        // 鼠标离开
        bgColor.value = 'white'
        myColor.value = 'black'
        isShow.value = false
      }
    }
    // 删除数据的方法
    const delTodo = () => {
      // 提示
      if (window.confirm('确定要删除吗?')) {
        props.deleteTodo(props.index)
      }
    }
    // 计算属性的方式---来让当前的复选框选中/不选中
    const isComptete = computed({
      get() {
        return props.todo.isCompleted
      },
      set(val) {
        props.updateTodo(props.todo, val)
      },
    })
    return {
      mouseHandler,
      bgColor,
      myColor,
      isShow,
      delTodo,
      isComptete,
    }
  },
})
</script>
<style scoped>
/*item*/
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  /* display: none; */
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}
</style>
```
```html
App.vue

<template>
  <div class="todo-container">
    <div class="todo-wrap">
      <Header :addTodo="addTodo" />
      <List :todos="todos" :deleteTodo="deleteTodo" :updateTodo="updateTodo" />
      <Footer
        :todos="todos"
        :checkAll="checkAll"
        :clearAllCompletedTodos="clearAllCompletedTodos"
      />
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent, onMounted, reactive, toRefs, watch } from 'vue'
// 引入直接的子级组件
import Header from './components/Header.vue'
import List from './components/List.vue'
import Footer from './components/Footer.vue'
// 引入接口
import { Todo } from './types/todo'
import { saveTodos, readTodos } from './utils/localStorageUtils'

export default defineComponent({
  name: 'App',
  // 注册组件
  components: {
    Header,
    List,
    Footer,
  },
  // 数据应该用数组来存储,数组中的每个数据都是一个对象,对象中应该有三个属性(id,title,isCompleted)
  // 把数组暂且定义在App.vue父级组件

  setup() {
    // 定义一个数组数据
    // const state = reactive<{ todos: Todo[] }>({
    //   todos: [
    //     { id: 1, title: '奔驰', isCompleted: false },
    //     { id: 2, title: '宝马', isCompleted: true },
    //     { id: 3, title: '奥迪', isCompleted: false },
    //   ],
    // })

    const state = reactive<{ todos: Todo[] }>({
      todos: [],
    })
    // 界面加载完毕后过了一会再读取数据
    onMounted(() => {
      setTimeout(() => {
        state.todos = readTodos()
      }, 1000)
    })

    // 添加数据的方法
    const addTodo = (todo: Todo) => {
      state.todos.unshift(todo)
    }
    // 删除数据的方法
    const deleteTodo = (index: number) => {
      state.todos.splice(index, 1)
    }
    // 修改todo的isCompleted属性的状态
    const updateTodo = (todo: Todo, isCompleted: boolean) => {
      todo.isCompleted = isCompleted
      console.log(todo)
    }
    // 全选或者是全不选的方法
    const checkAll = (isCompleted: boolean) => {
      // 比阿尼数组
      state.todos.forEach((todo) => {
        todo.isCompleted = isCompleted
      })
    }
    // 清理所有选中的数据
    const clearAllCompletedTodos = () => {
      state.todos = state.todos.filter((todo) => !todo.isCompleted)
    }

    // 监视操作:如果todos数组的数据变化了,直接存储到浏览器的缓存中
    // watch(()=>state.todos,(value)=>{
    //   // 保存到浏览器的缓存中
    //   localStorage.setItem('todos_key',JSON.stringify(value))
    // },{deep:true})

    // watch(
    //   () => state.todos,
    //   (value) => {
    //     // 保存到浏览器的缓存中
    //     saveTodos(value)
    //   },
    //   { deep: true }
    // )

    watch(() => state.todos, saveTodos, { deep: true })

    return {
      ...toRefs(state),
      addTodo,
      deleteTodo,
      updateTodo,
      checkAll,
      clearAllCompletedTodos,
    }
  },
})
</script>
<style scoped>
/*app*/
.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>
```
```ts
todo.ts
// 定义一个接口,约束state的数据类型
export interface Todo {
  id: number,
  title: string,
  isCompleted: boolean
}
```

```ts
localStorageUtils.ts
import { Todo } from '../types/todo'
// 保存数据到浏览器的缓存中
export function saveTodos(todos: Todo[]) {
  localStorage.setItem('todos_key', JSON.stringify(todos))
}
// 从浏览器的缓存中读取数据
export function readTodos(): Todo[] {
  return JSON.parse(localStorage.getItem('todos_key') || '[]')
}
```
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复vue3
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
