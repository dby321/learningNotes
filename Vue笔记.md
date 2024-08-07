
# Vue笔记

[黑马-Vue教程](https://www.bilibili.com/video/BV11s411A7h6/?spm_id_from=333.337.search-card.all.click&vd_source=f58f2e2144be4e99a8cf800afeecbbcb)

[尚硅谷-张天禹老师笔记+代码](https://github.com/yangxuesong0122/ssg--vue23-)

[Vue2官网](https://v2.cn.vuejs.org/)

> Vue.js是一个渐进式Javascript框架
>
> - 组件化模式 
> - 声明式编码 
> - 虚拟DOM+优秀的Diff算法

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
- 特定修饰符
  - `.lazy`在默认情况下，`v-model` 在每次 `input` 事件触发后将输入框的值与数据进行同步 (除了[上述](https://v2.cn.vuejs.org/v2/guide/forms.html#vmodel-ime-tip)输入法组合文字时)。你可以添加 `lazy` 修饰符，从而转为在 `change` 事件*之后*进行同步：
  - `.number`
  - ``.trim`


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

### vm.$refs

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
> 在简单项目中，使用 v-cloak 指令是解决屏幕闪动的好方法。但在大型、工程化的项目中（webpack、vue-router）只有一个空的 div 元素，元素中的内容是通过路由挂载来实现的，这时我们就不需要用到 v-cloak 指令。


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
    <button @click="addCount">加个一吧</button>
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
  <button @click="addCount">加个一吧</button>
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
- `native`组件绑定事件


### Vue键盘事件

[Vue官网-按键修饰符](https://cn.vuejs.org/v2/guide/events.html#%E6%8C%89%E9%94%AE%E4%BF%AE%E9%A5%B0%E7%AC%A6)

[Vue官网-Vue.config.keyCodes](https://cn.vuejs.org/v2/api/#keyCodes)

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
### watch和Vue.watch
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

### props

> [Vue官网 props](https://v2.cn.vuejs.org/v2/api/#props)

```js
// 简单语法
Vue.component('props-demo-simple', {
  props: ['size', 'myMessage']
})

// 对象语法，提供验证
Vue.component('props-demo-advanced', {
  props: {
    // 检测类型
    height: Number,
    // 检测类型 + 其他验证
    age: {
      type: Number,
      default: 0,
      required: true,
      validator: function (value) {
        return value >= 0
      }
    }
  }
})
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
### Vue.set和vm.$set
> [Vue官网-Vue.$set](https://cn.vuejs.org/v2/api/#Vue-set)
```js
this.$set(this.userInfo, 'id', 1);
```
### Vue.delete和vm.$delete
> [Vue官网-Vue.$delete](https://cn.vuejs.org/v2/api/#Vue-delete)
```js
this.$delete(this.userInfo, 'id');
```
### Vue.nextTick和vm.$nextTick
> [Vue官网-Vue.$nextTick](https://cn.vuejs.org/v2/api/#Vue-nextTick)
```js
this.$nextTick(() => {
  console.log('nextTick');
  this.userInfo.id = 1;
});
```

### 过滤器 filters和Vue.filter
> [Vue官网-过滤器](https://cn.vuejs.org/v2/guide/filters.html)
```js
filters: {
  formatDate: function (value) {
    return moment(value).format('YYYY-MM-DD HH:mm:ss');
  },
}
```


### 自定义指令 directives和Vue.directive
[Vue官网 自定义指令及其钩子函数](https://cn.vuejs.org/v2/guide/custom-directive.html#%E9%92%A9%E5%AD%90%E5%87%BD%E6%95%B0)

> 样式相关的在bind里面设置，行为相关的在inserted里面设置
> 定义的时候是focus 调用的时候是v-focus

一个指令定义对象可以提供如下几个钩子函数 (均为可选)：

- bind：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。

- inserted：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。

- update：所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新 (详细的钩子函数参数见下)。

- componentUpdated：指令所在组件的 VNode 及其子 VNode 全部更新后调用。

- unbind：只调用一次，指令与元素解绑时调用。

> 在很多时候，你可能想在 bind 和 update 时触发相同行为，而不关心其它的钩子。比如这样写：
  ```js
  Vue.directive('color-swatch', function (el, binding) {
    el.style.backgroundColor = binding.value
  })
  ```
> 自定义指令内的this指向window
```html
<input type="text" v-model="searchName" v-focus v-color="'red'" v-font-weight="900">

```

```js
    // 自定义全局指令 v-focus，为绑定的元素自动获取焦点：
    Vue.directive('focus', {
      inserted: function (el) { // inserted 表示被绑定元素插入父节点时调用
        el.focus();
      }
    });
    // 自定义局部指令 v-color 和 v-font-weight，为绑定的元素设置指定的字体颜色 和 字体粗细：
    directives: {
      color: { 
        bind(el, binding) {
          el.style.color = binding.value;
        },
      },
      'font-weight': function (el, binding2) { 
        el.style.fontWeight = binding2.value;
      }
    }

```



### `$event`

事件对象可以进行诸如DOM操作,获取自定义属性等

```html
<div data-aid(自定义属性)="123123" @click=eventFn($event)></div>
```
```js
eventFn(e){
  console.log(e.srcElement.style.background);
  console.log(e.srcElement.dataset.aid);
}
```
### `$mount`
> [Vue官网-Vue.$mount](https://cn.vuejs.org/v2/api/#vm-mount)
```js
this.$mount('#app');
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

### 混入 mixins和Vue.mixin

> [Vue官网 mixins](https://cn.vuejs.org/v2/guide/mixins.html)

> 如果你的混入包含一个 created 钩子，而创建组件本身也有一个，那么两个函数都会被调用。Mixin 钩子按照传入顺序依次调用，并在调用组件自身的钩子之前被调用。

```js
var mixin = {
  created: function () { console.log(1) }
}
var vm = new Vue({
  created: function () { console.log(2) },
  mixins: [mixin]
})
// => 1
// => 2
```

### 插件

> [Vue官网-插件](https://cn.vuejs.org/v2/guide/plugins.html)
- 使用插件
```js
Vue.use(myPlugin);
```
- 定义插件
```js
// 插件应该暴露一个 install 方法
// export default {};写成一个插件js文件也可以
var myPlugin = {
  install: function (Vue, options) {
    // 1.全局过滤器
    Vue.filter('mySlice', function (value) {
          return value.slice(0, 10);
          // return value.slice(0, 10) + '...';
    });
    // 2.全局指令
    Vue.directive('my-directive', {
      bind: function (el, binding, vnode, oldVnode) {
        // 1.指令绑定到元素上
        el.style.color = binding.value;
        // 2.指令绑定到元素上
        el.style.fontSize = binding.value + 'px';
        // 3.指令绑定到元素上
        el.style.backgroundColor = binding.value;
      },
      update: function (el, binding, vnode, oldVnode) {
        el.style.color = binding.value;
        el.style.fontSize = binding.value + 'px';
        el.style.backgroundColor = binding.value;
      },
    );
    // 3.全局混入
    Vue.mixin({
      created: function () {
        console.log('全局混入-created');
      },
      updated: function () {
        console.log('全局混入-updated');
      }
    );
    // 4.实例方法
    Vue.prototype.hello = function () {
      alert('hello');
      console.log(this);
    }
  });
  Vue.component('my-component', {
    data: function () {
      return {
        msg: 'hello'
      },
      created: function () {
        console.log('created');
      }
    });
};
```




## 2.Vue中的过渡和动画

> [Vue官网-动画](https://cn.vuejs.org/v2/guide/transitions.html)
>
> [animate.css](https://daneden.github.io/animate.css/)
>
> [Velocity.js 中文文档](http://www.5imoban.net/view/Velocity/index.html)

### CSS过渡

Vue 提供了 `transition` 的封装组件，在下列情形中，可以给任何元素和组件添加进入/离开过渡

- 条件渲染 (使用 `v-if`)
- 条件展示 (使用 `v-show`)
- 动态组件
- 组件根节点

这里是一个典型的例子：

```html
<div id="demo">
  <button @click="show = !show">
    Toggle
  </button>
  <transition name="fade">
    <p v-if="show">hello</p>
  </transition>
</div>
```

```js
new Vue({
  el: '#demo',
  data: {
    show: true
  }
})
```

```css
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
```

#### 初始渲染的过渡appear

```html
<transition appear>
  <!-- ... -->
</transition>
```

#### 过渡模式

```html
<transition name="fade" mode="out-in">
  <!-- ... the buttons ... -->
</transition>
```

#### 列表过渡

```html
<div id="list-demo" class="demo">
  <button @click="add">Add</button>
  <button @click="remove">Remove</button>
  <transition-group name="list" tag="p">
    <span v-for="item in items" v-bind:key="item" class="list-item">
      {{ item }}
    </span>
  </transition-group>
</div>
```

#### javascript钩子

可以在 attribute 中声明 JavaScript 钩子

```html
<transition
  @before-enter="beforeEnter"
  @enter="enter"
  @after-enter="afterEnter"
  @enter-cancelled="enterCancelled"

  @before-leave="beforeLeave"
  @leave="leave"
  @after-leave="afterLeave"
  @leave-cancelled="leaveCancelled"
>
  <!-- ... -->
</transition>
```

```js

// ...
methods: {
  // --------
  // 进入中
  // --------

  beforeEnter: function (el) {
    // ...
  },
  // 当与 CSS 结合使用时
  // 回调函数 done 是可选的
  enter: function (el, done) {
    // ...
    done()
  },
  afterEnter: function (el) {
    // ...
  },
  enterCancelled: function (el) {
    // ...
  },

  // --------
  // 离开时
  // --------

  beforeLeave: function (el) {
    // ...
  },
  // 当与 CSS 结合使用时
  // 回调函数 done 是可选的
  leave: function (el, done) {
    // ...
    done()
  },
  afterLeave: function (el) {
    // ...
  },
  // leaveCancelled 只用于 v-show 中
  leaveCancelled: function (el) {
    // ...
  }
}
```

### CSS动画

CSS 动画用法同 CSS 过渡，区别是在动画中 v-enter 类名在节点插入 DOM 后不会立即删除，而是在 animationend 事件触发时删除。

```html
<div id="example-2">
  <button @click="show = !show">Toggle show</button>
  <transition name="bounce">
    <p v-if="show">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris facilisis enim libero, at lacinia diam fermentum id. Pellentesque habitant morbi tristique senectus et netus.</p>
  </transition>
</div>
```

```js
new Vue({
  el: '#example-2',
  data: {
    show: true
  }
})
```

```css
.bounce-enter-active {
  animation: bounce-in .5s;
}
.bounce-leave-active {
  animation: bounce-in .5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(1);
  }
}
```

### animate.css

> [animate.css](https://daneden.github.io/animate.css/)

```html
<link href="https://cdn.jsdelivr.net/npm/animate.css@3.5.1" rel="stylesheet" type="text/css">

<div id="example-3">
  <button @click="show = !show">
    Toggle render
  </button>
  <transition
    name="custom-classes-transition"
    enter-active-class="animated tada"
    leave-active-class="animated bounceOutRight"
  >
    <p v-if="show">hello</p>
  </transition>
</div>
```

```js
new Vue({
  el: '#example-3',
  data: {
    show: true
  }
})
```

### velocity.js

> [Velocity.js 中文文档](http://www.5imoban.net/view/Velocity/index.html)

```js
// 无 jQuery 或 Zepto 时，Velocity()方法挂载在 window 对象上 (window.velocity)
// ( 第一个参数为原生js的dom选择器 )
Velocity(document.getElementById("dummy"), {
    opacity: 0.5
}, {
    duration: 1000
});
// 使用 jQuery 或 Zepto 时
$("#dummy").velocity({
    opacity: 0.5
}, {
    duration: 1000
});
```



## 3.Vue组件

> [Vue官网 Vue.extend](https://cn.vuejs.org/v2/api/#Vue-extend)
> [Vue官网-Vue.component](https://cn.vuejs.org/v2/api/#Vue-component)
> [Vue官网-组件](https://cn.vuejs.org/v2/guide/components.html)
### 组件的定义和使用

- 定义组件
```html
<template>
  <div>
  </div>
</template>

<script></script>
<style></style>
```

- 引用组件

  1. 引入组件`import xxx from "xxx"`
  2. 注册组件`conponents{'xxx':xxx}`
  3. 页面上使用组件`<xxx></xxx>`	



### 使用v-if和v-else来实现组件的切换

`13.组件切换-方式1.html`

1. 页面结构：

```html
<div id="app">
    <input type="button" value="toggle" @click="flag=!flag">
    <my-com1 v-if="flag"></my-com1>
    <my-com2 v-else="flag"></my-com2>
  </div>
```

2. Vue实例定义：

```js
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

```js
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

```html
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

```html
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

### 父子组件传值

- 父组件绑定动态属性`v-bind:xxx="数据名"`
- 子组件用props:[]来接收父组件传过来的 数据xxx

-----



- 父组件绑定动态方法`v-bind:xxx="方法名"`
- 子组件用props:[]来接收父组件传过来的 方法xxx

----



- 父组件绑定动态方法`v-bind:xxx="this"`
- 子组件用props:[]来接收父组件传过来的 父组件自己xxx

### `$parent`和`$children`

节制地使用 `$parent` 和 `$children` - 它们的主要目的是作为访问组件的应急方法。更推荐用 props 和 events 实现父子组件通信

### `$on`和`$off`和`$once`和`$emit`

> [Vue官网 $on和$off和$once和$emit](https://v2.cn.vuejs.org/v2/api/#%E5%AE%9E%E4%BE%8B%E6%96%B9%E6%B3%95-%E4%BA%8B%E4%BB%B6)

### 全局事件总线 非父子组件传值

1. 创建事件总线   main.js

```js
import Vue from 'vue'
// 创建事件总线   就相当于创建了一个新的vue实例
const bus = new Vue()
// 把bus挂载到了Vue的原型上, 保证所有的组件都能通过 this.$bus访问到事件总线
Vue.prototype.$bus = bus
```

2. 页面使用 发布事件 - 传递值

```js
// this.$bus.$emit('事件名', 额外参数)
this.$bus.$emit('send', 'hello')
```

3. 订阅事件 - 接收组件值

```js
// 1. 在created中订阅
// 2. 回调函数需要写成箭头函数
// this.$bus.$on('事件名', 事件回调函数)
this.$bus.$on('send', msg => {
  console.log(msg)
})
```

4. 事件解绑

```js
  beforeDestroy () {
    // 取消对bus事件的监听
    this.$bus.$off('send')
  }
```

### 消息订阅与发布 非父子组件传值

> [npm官网 pubsub-js](https://www.npmjs.com/package/pubsub-js)

1. 引入

```js
import PubSub from 'pubsub-js'
```

2. 订阅

```js
// create a function to subscribe to topics
var mySubscriber = function (msg, data) {
    console.log( msg, data );
};

// add the function to the list of subscribers for a particular topic
// we're keeping the returned token, in order to be able to unsubscribe
// from the topic later on
var token = PubSub.subscribe('MY TOPIC', mySubscriber);
```

3. 发布

```js
// publish a topic asynchronously
PubSub.publish('MY TOPIC', 'hello world!');

// publish a topic synchronously, which is faster in some environments,
// but will get confusing when one topic triggers new topics in the
// same execution chain
// USE WITH CAUTION, HERE BE DRAGONS!!!
PubSub.publishSync('MY TOPIC', 'hello world!');
```

4. 取消订阅

```js
// create a function to receive the topic
var mySubscriber = function (msg, data) {
    console.log(msg, data);
};

// add the function to the list of subscribers to a particular topic
// we're keeping the returned token, in order to be able to unsubscribe
// from the topic later on
var token = PubSub.subscribe('MY TOPIC', mySubscriber);

// unsubscribe this subscriber from this topic
PubSub.unsubscribe(token);
```



## 4.VueX
> [Vuex官网](https://v3.vuex.vuejs.org/zh/)
> Vuex 是一个专为 Vue.js 应用程序开发的状态管理库。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

- Action - 提交 mutation
- Mutation - 更改状态
- State - 存储状态
### Vuex使用
1. 安装Vuex
```js
npm install vuex
```
2. 在main.js中引入Vuex
```js
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
import store from './store'
new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app');
```
3. 在store文件夹中创建store.js
```js
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
export default new Vuex.Store({
  state: {
    count: 0
  },
  // 直接操作state，能被devtools监测
  mutations: {
    increment (state) {
      state.count++
      console.log(state.count)
    },
    decrement (state) {
      state.count--
      console.log(state.count)
    }
  },
  // 写业务逻辑，不能被devtools监测
  actions: {
    increment (context) {
      context.commit('increment')
      console.log(state.count)
    },
    decrement (context) {
      context.commit('decrement')
      console.log(state.count)
    }
  },
  // 类似于计算属性
  getters: {
    count (state) {
      return state.count
    }
  },
});
```
4. 使用Vuex数据和方法
```js
this.$store.state.count
this.$store.commit('increment')
this.$store.dispatch('increment')
```
### 简化Vuex使用
```js
import { mapState, mapMutations, mapActions, mapGetters } from 'vuex'
export default {
  computed: {
    ...mapState(['count']),
    ...mapMutations(['increment']),
    ...mapActions(['increment']),
    ...mapGetters(['count'])
  }
}
```
### Module
- 带namespaced的module
```js
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
 
const test1 = {
  namespaced: true,
  state: {
    name: 'moduleA',
    type: 'module A'
  },
  mutations: {
    updateNameByMutation(state, appendStr){
      state.name = state.name + " append Str: " + appendStr
    }
  },
  actions: {
    udpateNameByAction({commit}, appendStr) {
      commit("updateNameByMutation", appendStr)
    }
  },
  getters: {
    getNameA(state){
      return state.name
    }
  }
}
const test2 = {
  // 当namespaced=true 时， vuex, 将会自动给各自module 添加访问路径名。 方便区分moduel
  namespaced: true,
  state:{
    name: 'moduleB',
    type: 'module B'
  },
  mutations: {
    updateNameByMutation(state, appendStr){
      state.name = state.name + " append Str: " + appendStr
    }
  },
  actions: {
    // 如果不使用命名空间， 那么view 指向actions 的该方法时，会执行所有与指定action名相同的函数（即：这里module A,B 中该action都会执行）
    udpateNameByAction({commit}, appendStr){
      commit("updateNameByMutation", appendStr)
    }
  },
  getters: {
    getNameB(state){
      return state.name
    }
  }
}
 
const storeInstall =  new Vuex.Store({
   state: {
     name: 'i am root state name'
   },
   modules:{
    // 这里的路径名： test1, test2, 在view 中 通过 mapActions('test1', [actionName]) 使用并区分需要使用的module
    test1,
    test2
   }
})
 
export default storeInstall
```
- 使用Module
```js
<template>
  <div>
    <div>
    <h2>Page Test1</h2>
    </div>
    <div>
    <a href="javascript:" @click="changeName">udpate: 名称Name</a>  &nbsp; &nbsp;
    <a href="javascript:" @click="showName">显示更新后的Name</a> &nbsp; &nbsp;
    </div>
  </div>
</template>
<script>
import { mapState, mapActions } from 'vuex'
export default {
  data(){
    return {}
  },
  computed: {
    ...mapState('test1', {
      state: state => state
    })
  },
  methods: {
    // test1 模块路径名
    ...mapActions('test1', [
      'udpateNameByAction'
    ]),
    changeName(){
      this["udpateNameByAction"]('ha ha test1 udpate !!')
    },
    showName(){
      console.log(this.$store.state)
    },
  },
  mounted() {
    console.log("store name: ", this.$store)
    console.log("namespace test1 state: ", this.state)
  }
}
</script>
 
```

## 5.Vue-router
> [Vue-router官网](https://router.vuejs.org/zh/)
> Vue-router 是一个与 Vue.js 核心深度集成的路由器库。它为单页应用提供了导航，并支持异步组件。
> 每次切换路由，原组件会被销毁，重新渲染新的组件。

### Vue-router使用

1. 安装

```js
npm install vue-router
```
```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)
```
2. 创建路由
```js
const routes = [
  { path: '/', component: Home },
  { path: '/about', component: About }
]
const router = new VueRouter({
  routes
});
export default router
```
### 路由组件传参
> [Vue-router官网 路由组件传参](https://v3.router.vuejs.org/zh/guide/essentials/passing-props.html)
#### params 路经参数 类似java中的@PathVariable
```js
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', component: User }
  ]
})
```
#### query ?后面的参数 类似java中的@RequestParam
```js
const User = {
  template: `
  <div>
    <h2>User {{ $route.params.id }}</h2>
    <input v-model="userId">
    <button @click="changeId">changeId</button>
    <router-link :to="{ path: '/user', query: { id: userId }}">User {{ userId }}</router-link>
  <div>`,
}
const router = new VueRouter({
  routes: [
    { path: '/user', component: User }
    // query: { id: 1 }
  ]
})
```
#### props传参
##### props值为对象
1. 第一种写法：props值为对象，该对象中所有的key-value的组合最终都会通过props传给Detail组件
```js
children: [
    {
        name: 'xiangqing',
        path:'detail',
        component: Detail,
        // props对象中所有的key-value的组合最终都会通过props传给Detail组件
        props:{
            id: '888',
            title: '你好啊'
        }
    }
]
```
2. 在Detail组件中使用props接收传递来的数据
```html
<template>
  <div>
    <ul>
        <li>接收的id为{{id}}</li>
        <li>接收的title为{{title}}</li>
    </ul>
  </div>
</template>
 
<script>
export default {
    name: 'Detail',
    // 接收组件传递来的数据
    props: ['id', 'title']
}
</script>
```
##### props值为布尔值
1. 第二种写法：props值为布尔值，布尔值为true，则把路由收到的所有params参数通过props传给Detail组件
```js
{
    name: 'xiangqing',
    path:'detail/:id/:title', //使用占位符声明接收params参数
    component: Detail,
    // 第二种写法：props值为布尔值，布尔值为true，则把路由收到的所有params参数通过props传给Detail组件
    props: true
}
```

2. 传递params参数
```html
<li v-for="item in list" :key="item.id">
  <router-link :to="{
    name: 'xiangqing',
    params: {
      id: item.id,
      title: item.title
    }
  }">{{ item.title }}</router-link>
</li>
```
3. 组件内部使用props接收参数
```html
<template>
  <div>
    <ul>
        <li>接收的id为{{id}}</li>
        <li>接收的title为{{title}}</li>
    </ul>
  </div>
</template>
 
<script>
export default {
    name: 'Detail',
    // 接收组件传递来的数据
    props: ['id', 'title']
}
</script>
```
##### props值为函数
1. 第三种写法：props值为函数，该函数返回的对象中每一组key-value都会通过props传给Detail组件 
```js
{
    name: 'xiangqing',
    path:'detail/:id/:title', //使用占位符声明接收params参数
    component: Detail,
    // 第三种写法：props值为函数，该函数返回的对象中每一组key-value都会通过props传给Detail组件
    // props函数会自动调用并提供一个$route参数 可以通过$route来获取想要的数据传递给组件
    props($route) {
        return {
            id: $route.params.id,
            title: $route.params.title,
            // 还可以返回一些别的数据
            a: 1,
            b: "hello"
        }
    }
}
```

2. 组件内部使用props接收参数
```html
<template>
  <div>
    <ul>
        <li>接收的id为{{id}}</li>
        <li>接收的title为{{title}}</li>
        {{a}}-----{{b}}
    </ul>
  </div>
</template>
 
<script>
export default {
    name: 'Detail',
    // 接收组件传递的参数
    props: ['id', 'title', 'a', 'b']
}
</script>

```

### 命名路由
> [Vue-router官网 命名路由](https://router.vuejs.org/zh/guide/essentials/named-routes.html)
```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
```
要链接到一个命名路由，可以给 router-link 的 to 属性传一个对象：
```html
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
```
这跟代码调用 router.push() 是一回事：
```js
router.push({ name: 'user', params: { userId: 123 } })
```
### 命名视图
> [Vue-router官网 命名视图](https://v3.router.vuejs.org/zh/guide/essentials/named-views.html)
```html
<!-- UserSettings.vue -->
<div>
  <h1>User Settings</h1>
  <NavBar/>
  <router-view/>
  <router-view name="helper"/>
</div>
```
嵌套的视图组件在此已经被忽略了，但是你可以在这里 (opens new window)找到完整的源代码。

然后你可以用这个路由配置完成该布局：
```js
{
  path: '/settings',
  // 你也可以在顶级路由就配置命名视图
  component: UserSettings,
  children: [{
    path: 'emails',
    component: UserEmailsSubscriptions
  }, {
    path: 'profile',
    components: {
      default: UserProfile,
      helper: UserProfilePreview
    }
  }]
}
```
### 编程式路由导航
> [Vue-router官网 编程式导航](https://router.vuejs.org/zh/guide/essentials/navigation.html)
#### push()
```js
// 字符串
router.push('home')

// 对象
router.push({ path: 'home' })

// 命名的路由
router.push({ name: 'user', params: { userId: '123' }})

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
const userId = '123'
router.push({ name: 'user', params: { userId }}) // -> /user/123
router.push({ path: `/user/${userId}` }) // -> /user/123
// 这里的 params 不生效
router.push({ path: '/user', params: { userId }}) // -> /user
```
#### replace()
| 声明式                            | 编程式                |
| --------------------------------- | --------------------- |
| `<router-link :to="..." replace>` | `router.replace(...)` |

#### go()

这个方法的参数是一个整数，意思是在 history 记录中向前或者后退多少步，类似 `window.history.go(n)`。

例子

```js
// 在浏览器记录中前进一步，等同于 history.forward()
router.go(1)

// 后退一步记录，等同于 history.back()
router.go(-1)

// 前进 3 步记录
router.go(3)

// 如果 history 记录不够用，那就默默地失败呗
router.go(-100)
router.go(100)
```
### keep-alive缓存路由组件

#### 基本使用

```html
<template>
  <div id="app">
  	// 1. 将缓存 name 为 test 的组件
  	<keep-alive include='test'>
      <router-view/>
    </keep-alive>
	
	// 2. 将缓存 name 为 a 或者 b 的组件，结合动态组件使用
	<keep-alive include='a,b'>
  	  <router-view/>
	</keep-alive>
	
	// 3. 使用正则表达式，需使用 v-bind
	<keep-alive :include='/a|b/'>
  	  <router-view/>
	</keep-alive>	
	
	// 5.动态判断
	<keep-alive :include='includedComponents'>
  	  <router-view/>
	</keep-alive>
	
	// 5. 将不缓存 name 为 test 的组件
	<keep-alive exclude='test'>
  	  <router-view/>
	</keep-alive>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>
```

#### 配合router.meta缓存部分页面

1. 在 router 目录下的 index.js 文件里

```js
import Vue from 'vue'
import Router from 'vue-router'
const Home = resolve => require(['@/components/home/home'], resolve)
const Goods = resolve => require(['@/components/home/goods'], resolve)
const Ratings = resolve => require(['@/components/home/ratings'], resolve)
const Seller = resolve => require(['@/components/home/seller'], resolve)

Vue.use(Router)

export default new Router({
  mode: 'history',
  routes: [
    {
      path: '/',
      name: 'home',
      component: Home,
      redirect: 'goods',
      children: [
        {
          path: 'goods',
          name: 'goods',
          component: Goods,
          meta: {
        	keepAlive: false // 不需要缓存
      	  }
        },
        {
          path: 'ratings',
          name: 'ratings',
          component: Ratings,
          meta: {
        	keepAlive: true  // 需要缓存
      	  }
        },
        {
          path: 'seller',
          name: 'seller',
          component: Seller,
          meta: {
        	keepAlive: true  // 需要缓存
      	  }
        }
      ]
    }
  ]
})

```

2. 在 App.vue 里面

```html
<template>
  <div id="app">
  	<keep-alive>
      <router-view v-if="$route.meta.keepAlive"></router-view>
    </keep-alive>
    <router-view v-if="!$route.meta.keepAlive"></router-view>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

```

### actived()和deactived()

> 另外的两个生命函数钩子
>
> 激活 就是路由切换时触发 

### 路由守卫

> [Vue-router官网 导航守卫](https://v3.router.vuejs.org/zh/guide/advanced/navigation-guards.html)

> - `vue-router` 提供的导航守卫主要用来通过跳转或者取消的方式守卫路由
> - 也就是路由拦截
> - 可以通过路由守卫,来判断用户是否登录,该页面用户是否有访问该页面权限
> - 路由守卫分为`全局路由守卫`,`组件路由守卫`,`独享路由守卫`

#### 全局路由守卫

> 所谓全局路由守卫，相当于路由实例对象的保安，当你进行进行路由跳转时 ,都需要经过保安的检查
> 全局路由守卫有个两个：一个是`全局前置守卫`，一个是`全局后置守卫`

##### 全局前置守卫 beforeEach

1. beforeEach参数解析

   - beforEach有三个参数(to,from,next)


   - 参数中包括(完整的路由,路由名称,,meta,query,params,path)


   - to :即将要去往的路由


   - from :当前导航要离开的路由


   - next():判断路由是否通过


   - next()用法一: next()直接通过


   - next()用法二:next("/login"):强行跳转到指定页面


   - next()用法三: next(false):不允许跳转


2. 代码实现

  ```js
  //路由页面
    import Vue from 'vue'
    import VueRouter from 'vue-router'
    Vue.use(VueRouter)
    const routes = [...]
  
  const router = new VueRouter({
    routes
  })
  router.beforeEach((to, from, next) => {
    if (to.path == "/main") {
        const token = localStorage.getItem("token")
      if (token) {
          next()
      }else{
        next("/login")
      }
    }else{
        next()
    }
  })
  export default router
  ```

##### 全局解析守卫 beforeResolve

> 2.5.0 新增

在 2.5.0+ 你可以用 `router.beforeResolve` 注册一个全局守卫。这和 `router.beforeEach` 类似，区别是在导航被确认之前，**同时在所有组件内守卫和异步路由组件被解析之后**，解析守卫就被调用。

##### 全局后置守卫 afterEach

1. afterEach 解析
   - afterEach会在路由跳转后执行
   - afterEach()只有两个参数(to,from)
   - afterEach没有next()
   - afterEach对于分析、更改页面标题、声明页面等辅助功能以及许多其他事情都很有用。

#### 组件内的守卫

> - 组件路由守卫跟`data`,`methods`等同级
> - 组件路由守卫是写在每个单独的`Vue`文件里面的路由守卫
> - 组件路由守卫都只有`to`和`from`两个参数
> - 参数内容和`全局路由守卫`一样

##### beforeRouteEnter

- 在渲染该组件的对应路由前被调用
- 不能获取组件实例的 this
- 因为当该守卫执行时,组件实例还没有被创建

##### beforeRouteUpdate

- 当前的路由改变,但是组件被复用时调用
- 路由更新之前(组件没有变化但是路由变了--动态路由)调用
- 例如:一个有动态参数的路径,在两个页面之间跳转的时候;由于渲染了同样的组件,因此组件实例被复用,而beforeRouteUpdate会在这个时候调用
- 这个时候组件已经挂载好了,导航守卫可以访问组件实例的this

##### beforeRouteLeave

- 当导航离开渲染组件的对应路由时调用
- 在beforeRouteLeave可以访问组件实例的this

#### 路由独享的守卫

> - 在进入路由的时候触发
> - 不会在`params`,`query`,`hash`改变时触发
> - 只有从不同的路由导航进入才会触发
> - `beforeEnter`可以接收一个函数数组

- 普通使用

```js
//定义路由
const routes = [
  {
    path: '/home/:id',
    component: HomeDetails,
    beforeEnter: (to, from,next) => {
        //与全局路由守卫用法一致，但是只能针对一个页面使用
    },
  },
]

```

- 数组使用

```js
function removeQueryParams(to) {
  if (Object.keys(to.query).length)
    return { path: to.path, query: {}, hash: to.hash }
}

function removeHash(to) {
  if (to.hash) return { path: to.path, query: to.query, hash: '' }
}

const routes = [
  {
    path: '/home/:id',
    component: HomeDetails,
    beforeEnter: [removeQueryParams, removeHash],
  },
  {
    path: '/about',
    component: HomeDetails,
    beforeEnter: [removeQueryParams],
  },
]

```

#### 完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用 `beforeRouteLeave` 守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数，创建好的组件实例会作为回调函数的参数传入。

#### hash模式和history模式

> 默认是hash模式，url带有#
>
> 切换为history模式，url

##### NodeJS解决history模式页面刷新404

1. 安装 connect-history-api-fallback

```js
npm install connect-history-api-fallback --save
```

2. 在app.js文件中使用

```js
const history = require('connect-history-api-fallback')
app.use('/', history());
```

## 6.Vue请求数据

> [菜鸟教程-AJAX-向服务器发送请求](https://www.w3school.com.cn/ajax/ajax_xmlhttprequest_send.asp)

### 原生Ajax发起请求

> [菜鸟教程-Ajax教程](https://www.runoob.com/ajax/ajax-tutorial.html)

### jquery发起请求

> [菜鸟教程-jQuery - AJAX get() 和 post() 方法](https://www.runoob.com/jquery/jquery-ajax-get-post.html)

### window.fetch发起请求

在构建Javascript项目时，我们可以使用window对象，并且它带有许多可以在项目中使用的出色方法。这些功能之一是Fetch API，它提供了一种简单的全局 `.fetch()` 方法，这是一种从API异步获取数据的逻辑解决方案。

让我们看一下 `.fetch()` 方法的语法。

```javascript
fetch(url)
  .then((res) => 
    // handle response
  )
  .catch((error) => {
    // handle error
  })
```

在上面的示例中，您可以看到简单的获取GET请求的语法。在 `.fetch()` 方法中，我们有一个强制性参数**url**，它返回一个Promise，可以使用Response对象来解决。

`.fetch()` 方法的第二个参数是选项，它是可选的。如果我们不传递 `options`，请求总是GET，它从给定的URL下载内容。

在选项参数里面，我们可以传递方法或头信息，所以如果我们想使用POST方法或其他方法，我们必须使用这个可选的数组。

正如我之前提到的，Promise会返回Response对象，正因为如此，我们需要使用另一个方法来获取响应的主体。有几种不同的方法可以使用，取决于我们需要的格式：

- **response.json()**
- **response.text()**
- **response.formData()**
- **response.blob()**
- **response.arrayBuffer()**

让我们看一下带有可选参数的代码示例。

```javascript
fetch(url, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(data)
});
  .then((response) => response.json())
  .catch((error) => console.log(error))
```

在上面的代码示例中，你可以看到简单的POST请求，包括 `method`、`header` 和 `body` params。然后我使用 `json()` 方法将响应转换为JSON格式。

### 使用Vue-resource发起请求

> [Github-Vue-resource](https://github.com/pagekit/vue-resource)

### 使用axios发起请求

> [Github-axios](https://github.com/axios/axios)

## 7.Vue-cli

> [Vue-cli官网 vue.config.js基础配置](https://cli.vuejs.org/zh/config/#pages)
```js
module.exports = {
  pages: {
    index: {
      // page 的入口
      entry: 'src/index/main.js',
      // 模板来源
      template: 'public/index.html',
      // 在 dist/index.html 的输出
      filename: 'index.html',
      // 当使用 title 选项时，
      // template 中的 title 标签需要是 <title><%= htmlWebpackPlugin.options.title %></title>
      title: 'Index Page',
      // 在这个页面中包含的块，默认情况下会包含
      // 提取出来的通用 chunk 和 vendor chunk。
      chunks: ['chunk-vendors', 'chunk-common', 'index']
    },
    // 当使用只有入口的字符串格式时，
    // 模板会被推导为 `public/subpage.html`
    // 并且如果找不到的话，就回退到 `public/index.html`。
    // 输出文件名会被推导为 `subpage.html`。
    subpage: 'src/subpage/main.js',
    lintOnSave: false,
  }
}
```

## 8.Scss和Sass

- SCSS 需要使用分号和花括号而不是换行和缩进 **好用**
- Sass 使用换行和缩进而不是分号和花括号 **难用**

## 9.Vue原理

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

## 10.Element-UI

> [Element-UI官网](https://element.eleme.cn/#/zh-CN/component/quickstart)

# Vue3笔记



> 未完待续
