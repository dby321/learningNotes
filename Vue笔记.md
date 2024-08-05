
# Vue笔记

[黑马-Vue教程](https://www.bilibili.com/video/BV11s411A7h6/?spm_id_from=333.337.search-card.all.click&vd_source=f58f2e2144be4e99a8cf800afeecbbcb)

[尚硅谷-张天禹老师笔记+代码](https://github.com/yangxuesong0122/ssg--vue23-)

> Vue.js是一个渐进式Javascript框架
> 尤雨溪受到AngularJS的启发，开发了Seed框架，后来命名为Vue.js。
> 组件化模式 声明式编码 虚拟DOM+优秀的Diff算法

## 1.Vue.js基本操作

> 容器和Vue实例是一一对应的。

### `{{}}`

### `v-bind:`或`:`

### `v-html`

`v-html="..."`可以在网页中正常显示HTML代码

### `v-text`

`v-text="..."`

### `v-model`

- 必须在表单`input` `select`等中使用双向数据绑定
- .lazy,.number,.trim修饰符

### `v-if` `v-else-if` `v-else` `v-show`

> 一般来说，v-if 有更高的切换消耗而 v-show 有更高的初始渲染消耗。
>
> v-if会重新删除元素，v-show只是切换了元素的display:none的样式。
>
> 因此，
>
> 如果需要**频繁切换**用 **v-show** 较好，
>
> 如果在**运行时条件不大可能改变** 用**v-if** 较好。

### `v-for`和`key`

> [Vue官网-列表渲染](https://cn.vuejs.org/v2/guide/list.html)

1. 迭代数组

```html
<ul>
  <li v-for="(item, index) in list" :key="item.id">索引：{{index}} --- 姓名：{{item.name}} --- 年龄：{{item.age}}</li>
</ul>
```

2. 迭代对象中的属性

```html
	<!-- 循环遍历对象身上的属性 -->

    <div v-for="(value, name, index) in userInfo" :key="value.id">{{value}} --- {{name}} --- {{index}}</div>
```

3. 迭代数字

```html
<p v-for="i in 10">这是第 {{i}} 个P标签</p>
```
> 2.2.0+ 的版本里，**当在组件中使用** v-for 时，v-bind:key现在是必须的，值为number或者string

### `Vue.$refs`

```html
<div ref="userinfo"></div>
```
```js
this.$refs.userinfo
```
### `v-on:click`或`@click`
> 获取事件对象event
```html
<div @click="show(num,$event)">点击</div>
```
```js
show(num, event) {
  console.log(num, event);
}
```
### `v-cloak`
> 在简单项目中，使用 v-cloak 指令是解决屏幕闪动的好方法。但在大型、工程化的项目中（webpack、vue-router）只有一个空的 div 元素，元素中的内容是通过路由挂载来实现的，这时我们就不需要用到 v-cloak 指令咯。


```html
<div id="app" v-cloak>
    {{context}}
</div>
```
```css
[v-cloak]{
    display: none;
}
```
### `v-once`
> `v-once`指令用于DOM元素上，只渲染一次，后续更新不会生效。
```html
<div>
    <div v-once>{{count}}</div>
    <div >{{count}}</div>
    <button v-on:click="addCount">加个一吧</button>
</div>
```
```js
data: {
  context: 'hello world'
}
```
### `v-pre`
> `v-pre`指令用于DOM元素上，用于跳过该元素的编译过程，即跳过解析指令和插值表达式，该元素和它的子元素将被原样渲染。
```html
<div v-pre>
  <div>{{count}}</div>
  <button v-on:click="addCount">加个一吧</button>
</div>
```
### Vue事件修饰符

> [Vue官网-事件修饰符](https://cn.vuejs.org/v2/guide/events.html#%E4%BA%8B%E4%BB%B6%E4%BF%AE%E9%A5%B0%E7%AC%A6)

- `prevent`阻止默认事件
- `stop`阻止事件冒泡
- `once`事件只触发一次
- `self`event.target为当前时才触发
- `passive`移动端常用，先执行默认事件再触发事件回调函数
- `capture`捕获模式
- `prevent.stop` 阻止默认事件和冒泡


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
```html
<div @keydown.ctrl.shift.s="save">
</div>
```
```js
save() {
  
}
```

### computed

> 底层使用Object.defineProperties的getter和setter
> 计算属性便于vue-devtools观察

[掘金-vue (computed)计算属性用法总结](https://juejin.cn/post/7176807184368402487)
- 计算属性基础写法
```js
computed: {
  fullName: function () {
    return this.firstName + ' ' + this.lastName
  }
}
```
- 计算属性完整写法
```js
computed: {
  // 当计算属性被访问时，会调用get方法，当计算属性被修改时，会调用set方法
  fullName: {
    get: function () {
      return this.firstName + ' ' + this.lastName;
    }
    set: function (newValue) {
      var arr = newValue.split(' ');
      this.firstName = arr[0];
      this.lastName = arr[1];
    } 
  }
}
```
### watch和Vue.$watch
```js
watch: {
  fullName: function (newValue, oldValue) {
    console.log('fullName被修改了');
    console.log('fullName被修改前：' + oldValue);
    console.log('fullName被修改后：' + newValue);
    this.firstName = newValue.split(' ')[0];
    this.lastName = newValue.split(' ')[1];
    return newValue.toUpperCase();
  }
  fullName2: {
    handler: function (newValue, oldValue) {
      console.log('fullName2被修改了');
      console.log('fullName2被修改前：' + oldValue);
      console.log('fullName2被修改后：' + newValue);
      this.firstName = newValue.split(' ')[0];
      this.lastName = newValue.split(' ')[1];
      return newValue.toUpperCase();
    },
    deep: true// 深度监听,默认为false
    immediate: true// 立即执行handler，默认为false
    sync: true// 异步执行handler，默认为true
  }
}
```

### Vue绑定class样式

1. 数组

```html
<h1 :class="['red', 'thin']">这是一个邪恶的H1</h1>
```

2. 数组中使用三元表达式

```html
<h1 :class="['red', 'thin', isactive?'active':'']">这是一个邪恶的H1</h1>
```

3. 数组中嵌套对象

```html
<h1 :class="['red', 'thin', {'active': isactive}]">这是一个邪恶的H1</h1>
```

4. 直接使用对象【推荐使用】

```html
<h1 :class="{red:true, italic:true, active:true, thin:true}">这是一个邪恶的H1</h1>
```



### Vue使用style样式

1. 直接在元素上通过 `:style` 的形式，书写样式对象【如果属性名包含 - 则需加上'  '】

```html
<h1 :style="{color: 'red', 'font-size': '40px'}">这是一个善良的H1</h1>
```

2. 将样式对象，定义到 `data` 中，并直接引用到 `:style` 中

```js
data: {
        h1StyleObj: { color: 'red', 'font-size': '40px', 'font-weight': '200' }
}
```

```html
<h1 :style="h1StyleObj">这是一个善良的H1</h1>
```

3. 在 `:style` 中通过数组，引用多个 `data` 上的样式对象

```js
data: {
        h1StyleObj: { color: 'red', 'font-size': '40px', 'font-weight': '200' },
        h1StyleObj2: { fontStyle: 'italic' }
}
```

```html
<h1 :style="[h1StyleObj, h1StyleObj2]">这是一个善良的H1</h1>
```
### Vue.$set
> [Vue官网-Vue.$set](https://cn.vuejs.org/v2/api/#Vue-set)
```js
this.$set(this.userInfo, 'id', 1);
```
### Vue.$delete
> [Vue官网-Vue.$delete](https://cn.vuejs.org/v2/api/#Vue-delete)
```js
this.$delete(this.userInfo, 'id');
```
### Vue.$nextTick
> [Vue官网-Vue.$nextTick](https://cn.vuejs.org/v2/api/#Vue-nextTick)
```js
this.$nextTick(() => {
  console.log('nextTick');
  this.userInfo.id = 1;
});
```

### filters
> [Vue官网-过滤器](https://cn.vuejs.org/v2/guide/filters.html)
```js
filters: {
  formatDate: function (value) {
    return moment(value).format('YYYY-MM-DD HH:mm:ss');
  },
}
```


### directives

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



### `$event`

事件对象可以进行诸如DOM操作,获取自定义属性等

```html
<div data-aid(自定义属性)="123123" v-on:click=eventFn($event)></div>
```



```js
eventFn(e){
  console.log(e.srcElement.style.background);
  console.log(e.srcElement.dataset.aid);
}
```

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

## 2.Vue中的动画

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

## 3.Vue组件`component`

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

- 复习Object.defineProperties()

```js
const object = {};
let number=16;
Object.defineProperties(object,"age", {
  value:42,
  enumerable:false,//默认为false
  writable:true,//默认为false,控制属性是否可以修改
  configurable:true,//默认为false,控制属性是否可以被删除
  // getter 动态获取值
  get:function(){
    return number;
  },
  // setter 动态设置值
  set:function(newValue){
    number=newValue;
  }
});
console.log(Object.keys(object));
```

- Vue实现数据代理

> Vue通过Object.defineProperty()将data中的属性添加到Vue实例的属性上，并使用getter/setter来拦截对Vue实例属性的访问和修改。
