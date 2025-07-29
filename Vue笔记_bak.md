# Vue.js

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

## Vue.js 基本代码 和 MVVM 之间的对应关系

![1633752812087](./images/1633752812087.png)


## Vue之 - `基本的代码结构`和`插值表达式{{}}`、`v-cloak`


## Vue指令之`v-text`和`v-html`


## Vue指令之`v-bind`

1. 直接使用指令`v-bind`

2. 使用简化指令`:`

3. 在绑定的时候，拼接绑定内容：`:title="btnTitle + ', 这是追加的内容'"`


## Vue指令之`v-on`和`跑马灯效果`

### 跑马灯效果

1. HTML结构：

```

<div id="app">

    <p>{{info}}</p>

    <input type="button" value="开启" v-on:click="go">

    <input type="button" value="停止" v-on:click="stop">

  </div>

```

2. Vue实例：

```js

	// 创建 Vue 实例，得到 ViewModel

    var vm = new Vue({

      el: '#app',

      data: {

        info: '猥琐发育，别浪~！',

        intervalId: null

      },

      methods: {

        go() {

          // 如果当前有定时器在运行，则直接return

          if (this.intervalId != null) {

            return;

          }

          // 开始定时器

          this.intervalId = setInterval(() => {

            this.info = this.info.substring(1) + this.info.substring(0, 1);

          }, 500);

        },

        stop() {

          clearInterval(this.intervalId);

        }

      }

    });

```







## Vue指令之`v-on的缩写`和`事件修饰符`

### 事件修饰符：

> [Vue官网-事件修饰符](https://cn.vuejs.org/v2/guide/events.html#%E4%BA%8B%E4%BB%B6%E4%BF%AE%E9%A5%B0%E7%AC%A6)

+ .stop       阻止冒泡【冒泡是从内向外执行】【彻底阻止冒泡】
+ .prevent    阻止默认事件【<a href="">的默认事件是跳转】
+ .capture    添加事件侦听器时使用事件捕获模式【捕获是从外向内执行】
+ .self       只当事件在该元素本身（比如不是子元素）触发时触发回调【阻止仅仅是自己的冒泡】
+ .once       事件只触发一次  



## Vue指令之`v-model`双向数据绑定

> v-model只能运用在表单元素中
>

## 简易计算器案例

1. HTML 代码结构

```html

  <div id="app">

    <input type="text" v-model="n1">

    <select v-model="opt">

      <option value="0">+</option>

      <option value="1">-</option>

      <option value="2">*</option>

      <option value="3">÷</option>

    </select>

    <input type="text" v-model="n2">

    <input type="button" value="=" v-on:click="getResult">

    <input type="text" v-model="result">

  </div>

```

2. Vue实例代码：

```js

	// 创建 Vue 实例，得到 ViewModel

    var vm = new Vue({

      el: '#app',

      data: {

        n1: 0,

        n2: 0,

        result: 0,

        opt: '0'

      },

      methods: {

        getResult() {

          switch (this.opt) {

            case '0':

              this.result = parseInt(this.n1) + parseInt(this.n2);

              break;

            case '1':

              this.result = parseInt(this.n1) - parseInt(this.n2);

              break;

            case '2':

              this.result = parseInt(this.n1) * parseInt(this.n2);

              break;

            case '3':

              this.result = parseInt(this.n1) / parseInt(this.n2);

              break;

          }

        }

      }

    });

```









## Vue中使用样式



### 使用class样式

> 绑定样式要加“ ”   使用变量不用加“ ”

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



### 使用内联样式

1. 直接在元素上通过 `:style` 的形式，书写样式对象【如果属性名包含 - 则需加上'  '】
```html
<h1 :style="{color: 'red', 'font-size': '40px'}">这是一个善良的H1</h1>
```

2. 将样式对象，定义到 `data` 中，并直接引用到 `:style` 中
 + 在data上定义样式：
```js
data: {
        h1StyleObj: { color: 'red', 'font-size': '40px', 'font-weight': '200' }
}
```
 + 在元素中，通过属性绑定的形式，将样式对象应用到元素中：
```html
<h1 :style="h1StyleObj">这是一个善良的H1</h1>
```

3. 在 `:style` 中通过数组，引用多个 `data` 上的样式对象
 + 在data上定义样式：
```js
data: {
        h1StyleObj: { color: 'red', 'font-size': '40px', 'font-weight': '200' },
        h1StyleObj2: { fontStyle: 'italic' }
}
```
 + 在元素中，通过属性绑定的形式，将样式对象应用到元素中：
```html
<h1 :style="[h1StyleObj, h1StyleObj2]">这是一个善良的H1</h1>
```



## Vue指令之`v-for`和`key`属性

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







## Vue指令之`v-if`和`v-show`



> 一般来说，v-if 有更高的切换消耗而 v-show 有更高的初始渲染消耗。
>
> v-if会重新删除元素，v-show只是切换了元素的display:none的样式。
>
> 因此，
>
> 如果需要**频繁切换**用 **v-show** 较好，
>
> 如果在**运行时条件不大可能改变** 用**v-if** 较好。







## 品牌管理案例



### 删除品牌



### 根据条件筛选品牌

在2.x版本中[手动实现筛选的方式](https://cn.vuejs.org/v2/guide/list.html#显示过滤-排序结果)：

+ 筛选框绑定到 VM 实例中的 `searchName` 属性：

```html

<hr> 输入筛选名称：

<input type="text" v-model="searchName">

```

+ 在使用 `v-for` 指令循环每一行数据的时候，不再直接 `item in list`，而是 `in` 一个 过滤的methods 方法，同时，把过滤条件`searchName`传递进去：

```html

<tbody>

      <tr v-for="item in search(searchName)">

        <td>{{item.id}}</td>

        <td>{{item.name}}</td>

        <td>{{item.ctime}}</td>

        <td>

          <a href="#" @click.prevent="del(item.id)">删除</a>

        </td>

      </tr>

    </tbody>

```

+ `search` 过滤方法中，使用 数组的 `filter` 方法进行过滤：

```js

search(name) {

  return this.list.filter(x => {

    return x.name.indexOf(name) != -1;

  });

}

```







## Vue调试工具`vue-devtools`的安装步骤和使用

> [Vue.js devtools - 翻墙安装方式 - 推荐](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd?hl=zh-CN)
>
> [Vue教程-Vue.js devtools安装](https://www.bilibili.com/video/av27969216/?p=25)





## 过滤器

概念：Vue.js 允许你自定义过滤器，**可被用作一些常见的文本格式化**。过滤器可以用在两个地方：**mustache 插值和 v-bind 表达式**。过滤器应该被添加在 JavaScript 表达式的尾部，由管道符|指示；

### 私有过滤器

1. HTML元素：

```html

<td>{{item.ctime | dataFormat('yyyy-mm-dd')}}</td>

```

2. 私有 `filters` 定义方式：


```js

filters: { // 私有局部过滤器，只能在 当前 VM 对象所控制的 View 区域进行使用

    dataFormat(input, pattern = "") { // 在参数列表中 通过 pattern="" 来指定形参默认值，防止报错

      var dt = new Date(input);

      // 获取年月日

      var y = dt.getFullYear();

      var m = (dt.getMonth() + 1).toString().padStart(2, '0');

      var d = dt.getDate().toString().padStart(2, '0');



      // 如果 传递进来的字符串类型，转为小写之后，等于 yyyy-mm-dd，那么就返回 年-月-日

      // 否则，就返回  年-月-日 时：分：秒

      if (pattern.toLowerCase() === 'yyyy-mm-dd') {

        return `${y}-${m}-${d}`;

      } else {

        // 获取时分秒
        // 

        var hh = dt.getHours().toString().padStart(2, '0');

        var mm = dt.getMinutes().toString().padStart(2, '0');

        var ss = dt.getSeconds().toString().padStart(2, '0');



        return `${y}-${m}-${d} ${hh}:${mm}:${ss}`;

      }

    }

  }

```









### 全局过滤器

```js

// 定义一个全局过滤器

Vue.filter('dataFormat', function (input, pattern = '') {

  var dt = new Date(input);

  // 获取年月日

  var y = dt.getFullYear();

  var m = (dt.getMonth() + 1).toString().padStart(2, '0');

  var d = dt.getDate().toString().padStart(2, '0');



  // 如果 传递进来的字符串类型，转为小写之后，等于 yyyy-mm-dd，那么就返回 年-月-日

  // 否则，就返回  年-月-日 时：分：秒

  if (pattern.toLowerCase() === 'yyyy-mm-dd') {

    return `${y}-${m}-${d}`;

  } else {

    // 获取时分秒

    var hh = dt.getHours().toString().padStart(2, '0');

    var mm = dt.getMinutes().toString().padStart(2, '0');

    var ss = dt.getSeconds().toString().padStart(2, '0');



    return `${y}-${m}-${d} ${hh}:${mm}:${ss}`;

  }

});

```



> 注意：当有局部和全局两个名称相同的过滤器时候，会以就近原则进行调用，即：局部过滤器优先于全局过滤器被调用！



## 键盘修饰符以及自定义键盘修饰符

> [Vue官网-按键修饰符](https://cn.vuejs.org/v2/guide/events.html#%E6%8C%89%E9%94%AE%E4%BF%AE%E9%A5%B0%E7%AC%A6)
>
> [Vue官网-Vue.config.keyCodes](https://cn.vuejs.org/v2/api/#keyCodes)
>
> [js 里面的键盘事件对应的键码](http://www.cnblogs.com/wuhua1/p/6686237.html)

```html
@keyup.enter="add"
<input type="text" v-model="name" @keyup.113="add">
```



### 2.x中自定义键盘修饰符



1. 通过`Vue.config.keyCodes.名称 = 按键值`来自定义案件修饰符的别名：

```js

Vue.config.keyCodes.f2 = 113;

```

2. 使用自定义的按键修饰符：

```html

<input type="text" v-model="name" @keyup.f2="add">

```





## [自定义指令](https://cn.vuejs.org/v2/guide/custom-directive.html)

### 全局自定义指令 和 局部(私有)自定义指令

> 样式相关的在bind里面设置，行为相关的在inserted里面设置
>
> 定义的时候是focus 调用的时候是v-focus
>
> 注意：自定义指令的时候写**‘font-weight’**，而el.style.**fontWeight**
>
> [Vue官网 自定义指令 钩子函数](https://cn.vuejs.org/v2/guide/custom-directive.html#%E9%92%A9%E5%AD%90%E5%87%BD%E6%95%B0)

```js

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

```html

<input type="text" v-model="searchName" v-focus v-color="'red'" v-font-weight="900">

```



## [vue实例的生命周期](https://cn.vuejs.org/v2/guide/instance.html#实例生命周期)

![1633752983227](images/1633752983227.png)

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

## 发起数据请求( get, post, jsonp)

> [菜鸟教程-AJAX-向服务器发送请求](https://www.w3school.com.cn/ajax/ajax_xmlhttprequest_send.asp)

### 原生Ajax发起数据请求(已废弃)

> [菜鸟教程-Ajax教程](https://www.runoob.com/ajax/ajax-tutorial.html)

### 使用Vue-resource发起数据请求(已废弃)

> [Github-Vue-resource](https://github.com/pagekit/vue-resource)

### 使用axios发起数据请求

> [Github-axios](https://github.com/axios/axios)

## 品牌管理改造

`01.vue-resource改造品牌列表案例.html`

> 1. 调用 发起**数据请求的函数如getProductList()**，最好在**created()时调用**，因为此时data和methods已经初始化完毕。不要在mounted()时调用。
> 2. 发起数据请求的url地址中的 **服务器根地址** 最好进行**全局配置**,以防止服务器地址改变引起的大量代码修改。`Vue.http.options.root='http://www.liulongbin.top:3005/';`
> 3. 使用Vue-resource发送post请求中的 第三个参数 **{emulateJSON：true}**最好进行**全局配置`Vue.http.options.emulateJSON="true";`

> **查看项目文档的目录**：使用Word打开项目文档---视图---导航窗格 
>
> **安装必要插件**：Bootstrap 3 Snippets
>
> **导包**： 
> 因为要向后台发起数据请求，所以需要导入Vue-resource包`<script src="./lib/vue-2.4.0.js"></script>`
>
> 因为要使界面美观等，需要导入Bootstrap包`<link rel="stylesheet" href="./lib/bootstrap-3.3.7.css">`

## Vue中的动画

> [Vue官网-动画](https://v2.cn.vuejs.org/v2/guide/transitions.html#CSS-%E5%8A%A8%E7%94%BB)

## Vue组件的定义

什么是组件： 组件的出现，就是为了拆分Vue实例的代码量的，能够让我们以不同的组件，来划分不同的功能模块，将来我们需要什么样的功能，就可以去调用对应的组件即可；
组件化和模块化的不同：

- 模块化： 是从代码逻辑的角度进行划分的；方便代码分层开发，保证每个功能模块的职能单一；
- 组件化： 是从UI界面的角度进行划分的；前端的组件化，方便UI组件的重用；

### 全局Vue组件的定义

> 1. 组件注册的时候是驼峰命名的话，使用的时候要改成-；如果组件注册没用使用驼峰命名，则不需要改变 直接引用
> 2. 组件中的DOM结构，有且只能有唯一的根元素（Root Element）来进行包裹！
> 3. 组件中的data必须是方法，并返回对象

#### 使用 Vue.extend 配合 Vue.component

`08.组件-创建组件的方式1.html`



```js
var login = Vue.extend({
      template: '<h1>登录</h1>'
    });
Vue.component('login', login);
```

#### 直接使用 Vue.component

`08.组件-创建组件的方式1.html`

```js
Vue.component('register', {
      template: '<h1>注册</h1>'
    });
```

#### 将模板字符串，定义到script标签中

`09.组件-创建组件的方式2.html`

```html
<script id="tmpl">
      <div><a href="#">登录</a> | <a href="#">注册</a></div>
</script>
```

同时，需要使用 Vue.component 来定义组件：

```js
Vue.component('account', {
      template: '#tmpl'
    });
```

### 私有(局部)Vue组件的定义

`10.组件-创建组件的方式3.html`

1. 组件实例定义方式：

```html
<script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {},
      components: { // 定义子组件
        account: { // account 组件
          template: '<div><h1>这是Account组件{{name}}</h1><login></login></div>', // 在这里使用定义的子组件
          components: { // 定义子组件的子组件
            login: { // login 组件
              template: "<h3>这是登录组件</h3>"
            }
          }
        }
      }
    });
  </script>
```

2. 引用组件：

```js
<div id="app">
    <account></account>
  </div>
```

### Vue组件中的data和methods

`11.组件-组件中的data和methods.html`

1. 在组件中，`data`需要被定义为一个方法，例如：

```js
Vue.component('account', {
      template: '#tmpl',
      data() {//组件中的data必须是一个方法，返回一个对象
        return {
          msg: '大家好！'
        }
      },
      methods:{
        login(){
          alert('点击了登录按钮');
        }
      }
    });
```

2. 在子组件中，如果将模板字符串，定义到了script标签中，那么，要访问子组件身上的`data`属性中的值，需要使用`this`来访问；

### 为什么组件中的data属性必须定义为一个方法并返回一个对象

`12.组件-why components data must be a function.html`

## Vue组件的切换

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

```css
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

## Vue组件的传值和传方法

### 父组件向子组件传值

```js
<script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        msg: '这是父组件中的消息'
      },
      components: {
        son: {
          template: '<h1>这是子组件 --- {{finfo}}</h1>',
          // 2. 在子组件内部，使用props:['xxx']来接收父组件传递过来的数据
          // 3. 之后可以在template中{{xxx}}来进行使用props接收到的数据
          props: ['finfo'] //props里面的数据是只读的，修改会报警告
        }
      }
    });
 </script>
```

```html
<div id="app">
	// 1. 可以在引用子组件<son></son>的时候，可以用 属性绑定:xxx="父组件中的data"的形式传递到子组件内部
    <son v-bind:finfo="msg"></son>
  </div>
```

### 父组件向子组件传方法  同时 子组件 通过方法传参 向父组件传值

`04.组件-父组件把方法传递给子组件.html`

```js
<script>
	var com2={
		template:'#tmp1',
		// 4.在子组件中的methods中使用 this.$emit来触发父组件传递过来的方法func
		// emit从第二个参数开始就是传参
		data(){
			return{
				sonmsg:{name:'小头儿子'，age:6}
			}
		}
		methods:{
			myclick(){
				this.$emit('func',123,this.sonmsg)
			}
		}
	}

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data:{
      	datamsgFromSon:null
      }
      methods:{
      // 1.父组件定义一个方法
      	show(data，data2){
      		console.log("调用了父组件的show方法" + data + data2);
      		// 5.父组件拿到子组件传递过来的值(子组件 通过方法传参 向父组件传值)
      		this.datamsgFromSon=data2;
      	}
      }
      components: {
        com2
    });
</script>
```

```html
<template id="tmp1">
	<div>
		<h1>这是子组件</h1>
		// 3.在子组件中定义一个按钮来测试 父组件传递过来的方法
		<input type="button" value="子组件的按钮，点击触发父组件传递过来的方法" 		@click="myclick">
	</div>
</template>


<div id="app">
	// 2.通过 @func="xxx"来传递父组件的方法
	// v-on:简写成@
	<com2 @func="show"></com2>
</div>
```





## 使用 `this.$refs` 来获取DOM元素和组件

```html
  <div id="app">
    <div>
      <input type="button" value="获取元素内容" @click="getElement" />
      <!-- 使用 ref 获取元素 -->
      <h1 ref="myh1">这是一个大大的H1</h1>

      <hr>
      <!-- 使用 ref 获取子组件 -->
      <my-com ref="mycom"></my-com>
    </div>
  </div>

  <script>
    Vue.component('my-com', {
      template: '<h5>这是一个子组件</h5>',
      data() {
        return {
          name: '子组件'
        }
      }
    });

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {
        getElement() {
          // 通过 this.$refs 来获取元素
          console.log(this.$refs.myh1.innerText);
          // 通过 this.$refs 来获取组件
          console.log(this.$refs.mycom.name);
        }
      }
    });
  </script>
```

## 路由vue-router

### 什么是路由

1. **后端路由**：对于普通的网站，所有的超链接都是URL地址，所有的URL地址都对应服务器上对应的资源；
2. **前端路由**：对于单页面应用程序来说，主要通过URL中的hash(#号)来实现不同页面之间的切换，同时，hash有一个特点：HTTP请求中不会包含hash相关的内容；所以，单页面程序中的页面跳转主要用hash实现；
3. 在单页面应用程序中，这种通过hash改变来切换页面的方式，称作前端路由（区别于后端路由）；

### 在 vue 中使用 vue-router

1. 导入 vue-router 组件类库：

```html
<!-- 1. 导入 vue-router 组件类库 -->
  <script src="./lib/vue-router-2.7.0.js"></script>
```

2. 使用 router-link 组件来导航

```html
<!-- 2. 使用 router-link 组件来导航 -->
<router-link to="/login">登录</router-link>
<router-link to="/register">注册</router-link>
```

3. 使用 router-view 组件来显示匹配到的组件

```html
<!-- 3. 使用 router-view 组件来显示匹配到的组件 -->
<router-view></router-view>
```

4. 创建使用`Vue.extend`创建组件 也可以从其他文件import进来

```js
// 4.1 使用 Vue.extend 来创建登录组件
var login = Vue.extend({
  template: '<h1>登录组件</h1>'
});

// 4.2 使用 Vue.extend 来创建注册组件
var register = Vue.extend({
  template: '<h1>注册组件</h1>'
});

const Foo = { template: '<div>foo</div>' }
const Bar = { template: '<div>bar</div>' }
```

5. 创建一个路由 router 实例，通过 routers 属性来定义路由匹配规则

```js
// 5. 创建一个路由 router 实例，通过 routers 属性来定义路由匹配规则
var router = new VueRouter({
routes: [
//路由的重定向
{ path:'/',redirect:'/login' }
{ path: '/login', component: login },
{ path: '/register', component: register }
]
});
```

6. 使用 router 属性来使用路由规则

```js
// 6. 创建 Vue 实例，得到 ViewModel
var vm = new Vue({
el: '#app',
router: router // 使用 router 属性来使用路由规则，也可简写为router
});
```

### 设置路由高亮

#### 通过定义 外部样式 router-link-active 来设置路由高亮

```
<style>
    /* 设置路由高亮 */
    .router-link-active,
    .myactive
    {
      color: red;
      font-weight: 800;
      font-style: italic;
      font-size: 80px;
      text-decoration: underline;
      background-color: green;
    }
<style>
```

#### 通过在router实例中设置 linkActiveClass属性(也要定义外部样式) 来设置路由高亮

```
//myactive同样在外部样式中定义

var router=new VueRouter({
routes:[
{...},
{...},
{...}
],
linkActiveClass:"myacive"
})
```



### 设置路由切换动画

```
<style>
	// 设置transition的生命周期函数
    .v-enter,
    .v-leave-to {
      opacity: 0;
      transform: translateX(140px);
    }

    .v-enter-active,
    .v-leave-active {
      transition: all 0.5s ease;
    }
  </style>
```

```
// 给router-view套上transition
<transition mode="out-in">
      <router-view></router-view>
 </transition>
```

### 在路由规则中使用query方式传递路由参数

```
 <!-- 如果在路由中，使用 查询字符串?aaa=bbb&ccc=ddd 给路由传递参数，不需要修改路由规则的path属性 -->
<router-link to="/login?id=10&name=zs">登录</router-link>
```

```
<script>
    // 在template中 访问msg使用{{msg}}省略了this.  
    // 在template中 访问this.$route.query.id使用{{$route.query.id}}也省略了this
    var login = {
      template: '<h1>登录 --- {{ $route.query.id }} --- {{ $route.query.name }}</h1>',
    }
 </script>
```

### 在路由规则中使用params方式传递路由参数

```
<router-link to="/login/12/ls">登录</router-link>
```

```
<script>
    var login = {
      template: '<h1>登录 --- {{ $route.params.id }} --- {{ $route.params.name }}</h1>',
    }

    var router = new VueRouter({
      routes: [
      	//使用:id这种方法需要改变路由匹配规则的path属性
        { path: '/login/:id/:name', component: login }
      ]
    })
  </script>
```

### 使用 `children` 属性实现路由嵌套

> 记得放两个router-view坑

```
  <div id="app">
    <router-link to="/account">Account</router-link>

    <router-view></router-view>
  </div>

  <script>
    // 父路由中的组件
    const account = Vue.extend({
      template: `<div>
        这是account组件
        <router-link to="/account/login">login</router-link> | 
        <router-link to="/account/register">register</router-link>
        <router-view></router-view>
      </div>`
    });

    // 子路由中的 login 组件
    const login = Vue.extend({
      template: '<div>登录组件</div>'
    });

    // 子路由中的 register 组件
    const register = Vue.extend({
      template: '<div>注册组件</div>'
    });

    // 路由实例
    var router = new VueRouter({
      routes: [
        { path: '/', redirect: '/account/login' }, // 使用 redirect 实现路由重定向
        {
          path: '/account',
          component: account,
          children: [ // 通过 children 数组属性，来实现路由的嵌套
         	// 注意，子路由的开头位置，不要加 / 路径符
            { path: 'login', component: login }, 
            { path: 'register', component: register }
          ]
        }
      ]
    });

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {},
      components: {
        account
      },
      router: router
    });
  </script>
```

### 命名视图实现经典布局

> [弹性布局](https://www.jianshu.com/p/2bcac1b556d9)

1. 标签代码结构：

```
<div id="app">
    <router-view></router-view>
    <div class="content">
      <router-view name="a"></router-view>
      <router-view name="b"></router-view>
    </div>
  </div>
```

2. JS代码：

```
<script>
    var header = Vue.component('header', {
      template: '<div class="header">header</div>'
    });

    var sidebar = Vue.component('sidebar', {
      template: '<div class="sidebar">sidebar</div>'
    });

    var mainbox = Vue.component('mainbox', {
      template: '<div class="mainbox">mainbox</div>'
    });

    // 创建路由对象
    var router = new VueRouter({
      routes: [
        {
          path: '/', components: {
            'default': header,
            'a': sidebar,
            'b': mainbox
          }
        }
      ]
    });

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {},
      router
    });
  </script>
```

3. CSS 样式：

```
  <style>
    .header {
      border: 1px solid red;
    }

    .content{
      display: flex;
    }
    .sidebar {
      flex: 2;
      border: 1px solid green;
      height: 500px;
    }
    .mainbox{
      flex: 8;
      border: 1px solid blue;
      height: 500px;
    }
  </style>
```

## `watch`属性的使用

> 想要实现 `firstname` 和 `lastname` 两个文本框的内容改变，则`fullname`的文本框中的值也跟着改变
>
> 通过监听`@keyup`键盘弹起事件可以实现

> watch可以监听非DOM元素的事件，比如监听路由地址的改变

1. 监听`data`中属性的改变：

```
<div id="app">
    <input type="text" v-model="firstName"> +
    <input type="text" v-model="lastName"> =
    <span>{{fullName}}</span>
  </div>

  <script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        firstName: 'jack',
        lastName: 'chen',
        fullName: 'jack - chen'
      },
      methods: {},
      watch: {
        'firstName': function (newVal, oldVal) { // 第一个参数是新数据，第二个参数是旧数据
          this.fullName = newVal + ' - ' + this.lastName;
        },
        'lastName': function (newVal, oldVal) {
          this.fullName = this.firstName + ' - ' + newVal;
        }
      }
    });
  </script>
```

2. 监听路由的改变(只能使用watch来监听)：

```
<div id="app">
    <router-link to="/login">登录</router-link>
    <router-link to="/register">注册</router-link>

    <router-view></router-view>
  </div>

  <script>
    var login = Vue.extend({
      template: '<h1>登录组件</h1>'
    });

    var register = Vue.extend({
      template: '<h1>注册组件</h1>'
    });

    var router = new VueRouter({
      routes: [
        { path: "/login", component: login },
        { path: "/register", component: register }
      ]
    });

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {},
      router: router,
      watch: {
        '$route': function (newVal, oldVal) {
          if (newVal.path === '/login') {
            console.log('这是登录组件');
          }
        }
      }
    });
  </script>
```

## `computed`计算属性的使用

1. 默认只有`getter`的计算属性：

```
<div id="app">
    <input type="text" v-model="firstName"> +
    <input type="text" v-model="lastName"> =
    <span>{{fullName}}</span>
  </div>

  <script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        firstName: 'jack',
        lastName: 'chen'
      },
      methods: {},
      computed: { 
      // 计算属性； 特点：当计算属性中所以来的任何一个 data 属性改变之后，都会重新触发 本计算属性 的重新计算，	  // 从而更新 fullName 的值，求值完毕后悔缓存起来，避免之后使用的时候再次求值
      // 虽然写的像方法，但是引用的时候就当成 普通属性 去使用就好了
        fullName() {
          return this.firstName + ' - ' + this.lastName;
        }
      }
    });
  </script>
```

2. 定义有`getter`和`setter`的计算属性：

```
<div id="app">
    <input type="text" v-model="firstName">
    <input type="text" v-model="lastName">
    <!-- 点击按钮重新为 计算属性 fullName 赋值 -->
    <input type="button" value="修改fullName" @click="changeName">

    <span>{{fullName}}</span>
  </div>

  <script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        firstName: 'jack',
        lastName: 'chen'
      },
      methods: {
        changeName() {
          this.fullName = 'TOM - chen2';
        }
      },
      computed: {
        fullName: {
          get: function () {
            return this.firstName + ' - ' + this.lastName;
          },
          set: function (newVal) {
            var parts = newVal.split(' - ');
            this.firstName = parts[0];
            this.lastName = parts[1];
          }
        }
      }
    });
  </script>
```

## `watch`、`computed`和`methods`之间的对比

1. `computed`进行简单运算和字符串操作，主要当作属性来使用；
2. `methods` 业务逻辑
3. `watch`监听某些特定数据的变化，从而进行某些具体的业务逻辑操作；可以看作是`computed`和`methods`的结合体；

