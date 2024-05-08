



[TOC]



# Vue

## Vue创建项目和项目结构分析

### Vue2.x环境搭建与运行初始项目

1. 安装node.js
   1. 略
2. 安装Vue-cli脚手架工具
   1. `npm install --global vue-cli`
3. 创建项目+安装依赖 方法1
   1. `vue init webpack vue-demo01`
   2. `cd vue-demo01`
   3. `cnpm install/npm install `（如果创建项目没有报错，可以省略这一步）
4. 创建项目+安装依赖 方法2
   1. `vue init webpack-simple vue-demo01 `（这样生成的项目结构更简洁）
   2. `cd vue-demo01`
   3. `cnpm install/npm install` （如果创建项目没有报错，可以省略这一步）
5. 运行项目
   1. `npm run dev`

### Vue3.x创建项目与运行初始项目

> [Vue-cli官网](https://cli.vuejs.org/zh/)

### Vue项目目录解析

- `node_modules` 是npm或cnpm安装的各种依赖
- `src`是写代码的主要位置
  - `.vue`由template script和style三部分组成
  - `assets`是放置资源的文件夹
  - `model`是JS文件的模块化封装目录
    - `storage.js`封装操作localstorage本地存储的方法(模块化的方法)【需要node.js基础,大佬已经封装好了，我只要用就好】
    - `VueEmit.js`封装广播
  - `components`是存放组件的文件夹
  - `router`是存放路由相关的文件夹
  - `vuex`是存放Vuex相关的文件夹
- `.babelrc`是Babel的配置文件，用来处理Webpack处理不了的高级语法
- `.editorconfig`是VSCode这个编辑器的配置文件
- `.gitignore`是git忽略的配置文件
- `index.html`是HTML入口文件
- `package.json`是项目的配置文件，管理项目的所有模块。上面包含模块名和**曾经安装的版本号**,以及一些其他的信息。
- `README.md`是项目的说明文件。
- `webpack.config.js`是Webpack打包工具的配置文件

## Vue.js基本操作

### Vue绑定数据`{{}}`

### Vue列表渲染`v-for`

可以循环JS中的数组，可以多层嵌套

### Vue绑定属性`v-bind:`或`:`

从后台循环出来的数据是动态的，不能在属性处写死，所以需要用属性绑定

### Vue绑定HTML`v-html`

`v-html="..."`可以在网页中正常显示HTML代码

### Vue绑定数据方法二`v-text`

`v-text="..."`

### Vue绑定Class`v-bind:class`

绑定Class是为了在某种情况下达到某种显示

`v-bind:class="{'red':key==0,'blue':!flag}"`



`.red{ color:blue} `

`.blue{ color:blue} `

### Vue绑定Style`v-bind:style`

绑定Style是为了使用data中的数据来设置相关的样式

`v-bind:style="{'width':boxWidth+'px'}"`

`data{...... boxWidth:300}`

### Vue双向数据绑定`v-model`

必须在表单`input` `select`等中使用双向数据绑定

### Vue获取DOM节点`$ref`

`<div ref="userinfo"></div>`

`this.$ref.userinfo`

### Vue点击事件`v-on:click`或`@click`与方法定义和传值

### Vue事件对象`$event`

事件对象可以进行诸如DOM操作,获取自定义属性等

`<div data-aid(自定义属性)="123123" v-on:click=eventFn($event)></div>`

`eventFn(e){`

`console.log(e.srcElement.style.background);`

`console.log(e.srcElement.dataset.aid);`

`}`

### Vue生命周期函数

主要关注：

- created()
- **mounted()**
- beforeDestory()

## Vue组件`component`

### 组件的定义和使用

- 定义组件

  - <template>
    <div>
       </div>
    </template>

  - <script></script>>

  - <style></style>>

- 引用组件

  1. 引入组件`import xxx from "xxx"`
  2. 注册组件`conponents{'v-xxx':xxx}`
  3. 页面上使用组件`<v-xxx></v-xxx>`	



### Vue父子组件传值

### 父组件给子组件传值

- 父组件绑定动态属性`v-bind:xxx="数据名"`
- 子组件用props:[]来接收父组件传过来的 数据xxx

### 父组件给子组件传方法 同时 子组件通过方法带参给父组件传值

- 父组件绑定动态方法`v-bind:xxx="方法名"`
- 子组件用props:[]来接收父组件传过来的 方法xxx

### 父组件直接把自己传给子组件

- 父组件绑定动态方法`v-bind:xxx="this"`
- 子组件用props:[]来接收父组件传过来的 父组件自己xxx

### props:[]可以验证数据的合法性

```js
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object,
  callback: Function,
  contactsPromise: Promise // or any other constructor
}
```

### 父组件主动获取子组件的数据和方法

`<v-header ref="header"></v-header>`

`this.$refs.header.msg`

### 子组件主动获取父组件的数据和方法

节制地使用 `$parent` 和 `$children` - 它们的主要目的是作为访问组件的应急方法。更推荐用 props 和 events 实现父子组件通信

### Vue非父子组件传值

1. 封装`VueEmit.js`

   ```javascript
   import Vue from 'vue';
   var VueEmit =new Vue();
   ```

2. 一个组件广播

   ​	

   ```js
   import VueEmit from '../mdoel/VueEmit.js'
   export default{
   	data(){
   	
   	},
   	methods:{
   		emitNews(){
   			VueEvent.$emit('to-news',this.msg(要传过去的数据));
   		}
   	}
   }
   ```

   

3. 一个组件接收

   ```js
   import VueEmit from '../mdoel/VueEmit.js'
   export default{
   	data(){
   	
   	},
   	mounted(){
   		VueEmit.$on('to-news',function(data){
   			console.log(data);
   		})
   	}
   }
   ```

## Vue路由`vue-router`



## Vue的UI框架

### ElementUI

### MintUI

### Vant

## VueX

### Vuex使用场景

比如首页组件和新闻组件都挂载到根下，挂载了首页就不挂载新闻

所以首页组件和新闻组件**既不是父子组件也不是非父子组件(比如兄弟组件)**

Vuex主要解决不同组件的数据共享(解决组件传值解决不了的**断链传数据**的问题)和数据持久化(就是本地存储)【localStorage和sessionStorage也可以解决上述问题】

- 大型项目用Vuex
- 小型项目用localStorage和sessionStorage

### Vuex起步

1. vuex文件夹中新建store.js

2. 安装vuex

3. ```js
   const store = new Vuex.Store({
    // 放数据 类似于data   
     state: {
       count: 0
     },
     // 放方法 类似于methods
     mutations: {
       increment (state) {
         state.count++
       }
     }
   })
   // 日常暴露出去
   export default store;
   ```

4. 你可以通过 `this.$store.state`(大地老师写法)或者`store.state`(官方文档写法) 来获取状态对象(其实就是可以调用state里面的数据)，以及通过 `this.$store.commit(increment)` 触发状态变更(其实就是可以调用mutations里面的方法)

### getter和action

用的很少，但是面试爱问

- `getters`
  - Vuex 允许我们在 store 中定义“getter”（可以认为是 store 的计算属性）。就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算
- `actions`
  - 提交的是`mutation`，而不是直接变更状态。
  - `action`可以包含任意异步操作

### 新闻数据持久化案例

```js
// payload负载 即mutations的方法第二个参数来传参，而第一个参数默认就是state
mutations: {
  increment (state, n) {
    state.count += n
  }
}
```

## Todolist案例

### Vue中的`Keycode`

### 本地存储

## 请求数据

#### axios

#### vue-resource

#### Jsonp



## Scss和Sass

- SCSS 需要使用分号和花括号而不是换行和缩进 **好用**
- Sass 使用换行和缩进而不是分号和花括号 **难用**

## Vue原理

### 数据代理

> Object.defineProperties()

