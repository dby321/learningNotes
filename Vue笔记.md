

# Vue笔记

[黑马-Vue教程](https://www.bilibili.com/video/BV11s411A7h6/?spm_id_from=333.337.search-card.all.click&vd_source=f58f2e2144be4e99a8cf800afeecbbcb)

[尚硅谷-张天禹老师笔记+代码](https://github.com/yangxuesong0122/ssg--vue23-)

## 课程介绍与学习须知

前5天： 都在学习Vue基本的语法和概念；打包工具 Webpack , Gulp
后5天： 以项目驱动教学；


### 什么是Vue.js

+ Vue.js 最火的前端框架，React.js最流行前端框架（React除了开发网站，还可以开发手机App， Vue语法也是可以用于进行手机App开发的，需要借助于[Weex](https://weex.apache.org/zh/guide/introduction.html)

  > Weex 是使用流行的 Web 开发体验来开发高性能原生应用的框架

+ Vue.js 是前端的**主流框架之一**，和Angular.js、React.js 一起，并成为前端三大主流框架！

+ Vue.js 是一套构建用户界面的框架，**只关注视图层**，它不仅易于上手，还便于与第三方库或既有项目整合。（Vue有配套的第三方类库，可以整合起来做大型项目的开发）

+ 前端的主要工作？主要负责MVC中的V这一层；主要工作就是和界面打交道，来制作前端页面效果；

### 为什么要学习流行框架

 + 企业中，使用框架，能够提高开发的效率；

 + 提高开发效率的发展历程：原生JS -> Jquery之类的类库 -> 前端模板引擎 -> Angular.js / Vue.js

   > 原生JS【有浏览器的兼容问题】
   >
   > Jquery【屏蔽浏览器兼容性，不方便操作DOM】
   >
   > 前端模板引擎 【方便操作DOM，会渲染整个页面效率低】
   >
   > Angular.js / Vue.js 【减少不必要的DOM操作；提高渲染效率；双向数据绑定】

   

 + Vue的核心概念：用户不再操作DOM，专注于写业务逻辑

### 框架和库的区别

 + 框架：是一套完整的解决方案；对项目的侵入性较大，项目如果需要更换框架，则需要重新架构整个项目。

   + node 中的 express

 + 库（插件）：提供某一个小功能，对项目的侵入性较小，如果某个库无法完成某些需求，可以很容易切换到其它库实现需求。

   - 从Jquery 切换到 Zepto
   - 从 EJS 切换到 art-template

   > node【运行在服务器端的JS】
   >
   > express【是一个简洁而灵活的 node.*js* Web应用框架】
   >
   > Jquery【JS代码库 浏览器兼容好】
   >
   > Zepto【抛弃低级浏览器适配，轻便，用于移动端】
   >
   > EJS【嵌入式JS模板引擎】
   >
   > art-template【另一个简约超快的模板引擎】



### Node（后端）中的 MVC 与 前端中的 MVVM 之间的区别

 + MVC 是后端的分层开发概念；
   + Model：主要处理数据的CRUD
   + View：前端的页面
   + Controller：数据的业务逻辑处理（如登录，注销）
 + MVVM是前端视图层的概念，主要关注于 视图层分离，也就是说：MVVM把前端的视图层，分为了 三部分 Model, View , VM ViewModel
 + 为什么有了MVC还要有MVVM

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

### Vue指令之`v-if`和`v-show`



> 一般来说，v-if 有更高的切换消耗而 v-show 有更高的初始渲染消耗。
>
> v-if会重新删除元素，v-show只是切换了元素的display:none的样式。
>
> 因此，
>
> 如果需要**频繁切换**用 **v-show** 较好，
>
> 如果在**运行时条件不大可能改变** 用**v-if** 较好。

### Vue指令之`v-for`和`key`属性

> [Vue官网-列表渲染](https://cn.vuejs.org/v2/guide/list.html)

1. 迭代数组

```html
<ul>
  <li v-for="(item, i) in list">索引：{{i}} --- 姓名：{{item.name}} --- 年龄：{{item.age}}</li>
</ul>
```

2. 迭代对象中的属性

```html
	<!-- 循环遍历对象身上的属性 -->

    <div v-for="(val, key, i) in userInfo">{{val}} --- {{key}} --- {{i}}</div>
```

3. 迭代数字

```html
<p v-for="i in 10">这是第 {{i}} 个P标签</p>
```



> 2.2.0+ 的版本里，**当在组件中使用** v-for 时，v-bind:key现在是必须的，值为number或者string



当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用 “**就地复用**” 策略。如果数据项的顺序被改变，Vue将**不是移动 DOM 元素来匹配数据项的顺序**， 而是**简单复用此处每个元素**，并且确保它在特定索引下显示已被渲染过的每个元素。

为了给 Vue 一个提示，**以便它能跟踪每个节点的身份，从而重用和重新排序现有元素**，你需要为每项提供一个唯一 key 属性。

### Vue获取DOM节点`$ref`

`<div ref="userinfo"></div>`

`this.$ref.userinfo`

### Vue点击事件`v-on:click`或`@click`与方法定义和传值

### Vue滚动条事件`@scroll`和鼠标滚轮事件`@wheel`

### Vue事件修饰符

> [Vue官网-事件修饰符](https://cn.vuejs.org/v2/guide/events.html#%E4%BA%8B%E4%BB%B6%E4%BF%AE%E9%A5%B0%E7%AC%A6)

- `prevent`阻止默认事件
- `stop`阻止事件冒泡
- `once`事件只触发一次
- `self`event.target为当前时才触发
- `passive`移动端常用，先执行默认事件再触发事件回调函数
- `capture`捕获模式

### Vue键盘事件

[Vue官网-按键修饰符](https://cn.vuejs.org/v2/guide/events.html#%E6%8C%89%E9%94%AE%E4%BF%AE%E9%A5%B0%E7%AC%A6)

[Vue官网-Vue.config.keyCodes](https://cn.vuejs.org/v2/api/#keyCodes)

[CSDN-Vue中监听键盘事件](https://blog.csdn.net/xiaxiangyun/article/details/80404768)

> 1. 有的键盘按键不能绑定事件，比如调整音量和背光
>
> 2. tab键是视障人士用的，只有keydown能触发
>
> 3. Ctrl、Alt、Meta、Shift
>
> - 配合keyup使用，是按下这个键再按下其他键时释放后才触发（组合键）
> - 配合keydown就正常触发



### Vue计算属性computed

> 底层使用Object.defineProperties的getter和setter
>
> 计算属性便于vue-devtools观察

[掘金-vue (computed)计算属性用法总结](https://juejin.cn/post/7176807184368402487)

### Vue监听属性watch

[cnblogs-vue2.0中watch总结：普通监听和深度监听](https://www.cnblogs.com/toggle/p/9884059.html)

### Vue使用class样式

> 绑定样式要加“ ”   使用变量不用加“ ”

1. 数组

```
<h1 :class="['red', 'thin']">这是一个邪恶的H1</h1>
```

2. 数组中使用三元表达式

```
<h1 :class="['red', 'thin', isactive?'active':'']">这是一个邪恶的H1</h1>
```

3. 数组中嵌套对象

```
<h1 :class="['red', 'thin', {'active': isactive}]">这是一个邪恶的H1</h1>
```

4. 直接使用对象【推荐使用】

```
<h1 :class="{red:true, italic:true, active:true, thin:true}">这是一个邪恶的H1</h1>
```



### Vue使用style样式

1. 直接在元素上通过 `:style` 的形式，书写样式对象【如果属性名包含 - 则需加上'  '】

```
<h1 :style="{color: 'red', 'font-size': '40px'}">这是一个善良的H1</h1>
```

2. 将样式对象，定义到 `data` 中，并直接引用到 `:style` 中

 + 在data上定义样式：

```
data: {
        h1StyleObj: { color: 'red', 'font-size': '40px', 'font-weight': '200' }
}
```

 + 在元素中，通过属性绑定的形式，将样式对象应用到元素中：

```
<h1 :style="h1StyleObj">这是一个善良的H1</h1>
```

3. 在 `:style` 中通过数组，引用多个 `data` 上的样式对象

 + 在data上定义样式：

```
data: {
        h1StyleObj: { color: 'red', 'font-size': '40px', 'font-weight': '200' },
        h1StyleObj2: { fontStyle: 'italic' }
}
```

 + 在元素中，通过属性绑定的形式，将样式对象应用到元素中：

```
<h1 :style="[h1StyleObj, h1StyleObj2]">这是一个善良的H1</h1>
```

### Vue过滤器filters

[CSDN-Vue知识点总结(8)——filter过滤器](https://blog.csdn.net/m0_46171043/article/details/111768819)

### Vue自定义指令

#### 全局自定义指令 和 局部(私有)自定义指令

> 样式相关的在bind里面设置，行为相关的在inserted里面设置
>
> 定义的时候是focus 调用的时候是v-focus
>
> 注意：自定义指令的时候写**‘font-weight’**，而el.style.**fontWeight**
>
> [Vue官网 自定义指令 钩子函数](https://cn.vuejs.org/v2/guide/custom-directive.html#%E9%92%A9%E5%AD%90%E5%87%BD%E6%95%B0)

```
    // 自定义全局指令 v-focus，为绑定的元素自动获取焦点：
  

    Vue.directive('focus', {

      inserted: function (el) { // inserted 表示被绑定元素插入父节点时调用

        el.focus();

      }

    });



    // 自定义局部指令 v-color 和 v-font-weight，为绑定的元素设置指定的字体颜色 和 字体粗细：

      directives: {

        color: { // 为元素设置指定的字体颜色

          bind(el, binding) {

            el.style.color = binding.value;

          },

        },

        'font-weight': function (el, binding2) { // 自定义指令的简写形式，等同于定义了 bind 和 update 两个钩子函数

          el.style.fontWeight = binding2.value;

        }

      }

```

2. 自定义指令的使用方式：

```
<input type="text" v-model="searchName" v-focus v-color="'red'" v-font-weight="900">

```



### Vue事件对象`$event`

事件对象可以进行诸如DOM操作,获取自定义属性等

`<div data-aid(自定义属性)="123123" v-on:click=eventFn($event)></div>`

`eventFn(e){`

`console.log(e.srcElement.style.background);`

`console.log(e.srcElement.dataset.aid);`

`}`

### Vue生命周期函数

- 创建期间的生命周期函数：
  - beforeCreate：实例刚在内存中被创建出来，此时，还没有初始化好 data 和 methods 属性
  - **created**：【操作data和methods最早的阶段】实例已经在内存中创建OK，此时 data 和 methods 已经创建OK，此时还没有开始 编译模板
  - beforeMount：模板编译完成，尚未挂载到页面
  - **mounted**：【操作页面DOM最早的阶段 】模板编译完成，也挂载到了页面
- 运行期间的生命周期函数：
  - **beforeUpdate**：【状态更新 data新，页面旧】状态更新之前执行此函数， 此时 data 中的状态值是最新的，但是界面上显示的 数据还是旧的，因为此时还没有开始重新渲染DOM节点
  - updated：【状态更新 data新，页面新】实例更新完毕之后调用此函数，此时 data 中的状态值 和 界面上显示的数据，都已经完成了更新，界面已经被重新渲染好了！
- 销毁期间的生命周期函数：
  - **beforeDestroy**：【Vue实例还可用】实例销毁之前调用。在这一步，实例仍然完全可用。
  - destroyed：【Vue实例已被销毁】Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。

## Vue中的动画

> [Vue官网-动画](https://cn.vuejs.org/v2/guide/transitions.html)

### 使用v-if和v-show来实现动画

`02.动画-不使用动画.html`

### 使用过渡类名

`03.动画-使用过渡类名实现动画.html`

> [Vue官网-过渡类名](https://cn.vuejs.org/v2/guide/transitions.html#%E8%BF%87%E6%B8%A1%E7%9A%84%E7%B1%BB%E5%90%8D)

### 使用自定义过渡类名

`04.动画-修改v-前缀.html`

> [Vue官网-自定义过渡类名](https://cn.vuejs.org/v2/guide/transitions.html#%E8%87%AA%E5%AE%9A%E4%B9%89%E8%BF%87%E6%B8%A1%E7%9A%84%E7%B1%BB%E5%90%8D)

### 使用第三方 CSS 动画库

`05.动画-使用第三方类实现动画.html`

> [Animate.css官网](https://daneden.github.io/animate.css/)

### 使用动画钩子函数

`06.动画-使用钩子函数模拟小球半场动画.html`

> [Vue官网-JavaScript钩子(动画生命周期函数)](https://cn.vuejs.org/v2/guide/transitions.html#JavaScript-%E9%92%A9%E5%AD%90)

### 使用transition-group实现列表v-for的动画

`07.动画-列表动画.html`

> [Vue官网-列表过渡](

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

## Vue组件的切换

### 使用v-if和v-else来实现组件的切换

`13.组件切换-方式1.html`

1. 页面结构：

```
<div id="app">
    <input type="button" value="toggle" @click="flag=!flag">
    <my-com1 v-if="flag"></my-com1>
    <my-com2 v-else="flag"></my-com2>
  </div>
```

2. Vue实例定义：

```
<script>
    Vue.component('myCom1', {
      template: '<h3>奔波霸</h3>'
    })

    Vue.component('myCom2', {
      template: '<h3>霸波奔</h3>'
    })

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        flag: true
      },
      methods: {}
    });
  </script>
```

### 使用`:is`属性来切换不同的子组件,并添加切换动画

`14.组件切换-方式2.html` `15.组件切换-切换动画.html`

1. 组件实例定义方式：

```
  // 登录组件
    const login = Vue.extend({
      template: `<div>
        <h3>登录组件</h3>
      </div>`
    });
    Vue.component('login', login);

    // 注册组件
    const register = Vue.extend({
      template: `<div>
        <h3>注册组件</h3>
      </div>`
    });
    Vue.component('register', register);

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: { comName: 'login' },
      methods: {}
    });
```

2. 使用`component`标签，来引用组件，并通过`:is`属性来指定要加载的组件：

```
  <div id="app">
    <a href="#" @click.prevent="comName='login'">登录</a>
    <a href="#" @click.prevent="comName='register'">注册</a>
    <hr>
    <transition mode="out-in">
      <component :is="comName"></component>
    </transition> 
  </div>
```

3. 添加切换样式：

```
  <style>
    .v-enter,
    .v-leave-to {
      opacity: 0;
      transform: translateX(30px);
    }

    .v-enter-active,
    .v-leave-active {
      position: absolute;
      transition: all 0.3s ease;
    }

    h3{
      margin: 0;
    }
  </style>
```



## Vue父子组件传值

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

## Vue请求数据

> [菜鸟教程-AJAX-向服务器发送请求](https://www.w3school.com.cn/ajax/ajax_xmlhttprequest_send.asp)

### 原生Ajax发起数据请求(已废弃)

> [菜鸟教程-Ajax教程](https://www.runoob.com/ajax/ajax-tutorial.html)

### 使用Vue-resource发起数据请求(已废弃)

> [Github-Vue-resource](https://github.com/pagekit/vue-resource)

### 使用axios发起数据请求

> [Github-axios](https://github.com/axios/axios)

## Scss和Sass

- SCSS 需要使用分号和花括号而不是换行和缩进 **好用**
- Sass 使用换行和缩进而不是分号和花括号 **难用**

## Vue原理

### 数据代理

> Object.defineProperties的get配置可以动态获取值 

```js
const object1 = {};

Object.defineProperties(object1, {
  property1: {
    value: 42,
    writable: true,
  },
  property2: {},
});

console.log(object1.property1);
// Expected output: 42
```



## Node简记

> express相当于前端的web框架
>
> nodemon相当于前端的热部署工具
>
> app.use注册全局中间件，但是在vue中app.use是注册插件
>
> - [掘金-Express中间件总结](https://juejin.cn/post/7150224098466430983)
>
> - 中间件通常是独立功能的函数，可以在应用的任何地方被 `use` 或 `middlewareFunction` 调用。插件则可能是更复杂的库，提供一系列中间件、错误处理或者服务级别的功能。
