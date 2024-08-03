# note-es6笔记
[菜鸟教程-ES6教程](https://www.runoob.com/w3cnote_genre/es6)

- [hello-es6](#hello-es6)
    - [1.ES6怎么来的](#1es6%E6%80%8E%E4%B9%88%E6%9D%A5%E7%9A%84)
    - [2.ES6兼容性](#2es6%E5%85%BC%E5%AE%B9%E6%80%A7)
    - [3.变量 let 和 常量 const](#3%E5%8F%98%E9%87%8F-let-%E5%92%8C-%E5%B8%B8%E9%87%8F-const)
    - [4.函数-箭头函数](#4%E5%87%BD%E6%95%B0-%E7%AE%AD%E5%A4%B4%E5%87%BD%E6%95%B0)
    - [5.函数-参数](#5%E5%87%BD%E6%95%B0-%E5%8F%82%E6%95%B0)
    - [6.解构赋值](#6%E8%A7%A3%E6%9E%84%E8%B5%8B%E5%80%BC)
    - [7.数组](#7%E6%95%B0%E7%BB%84)
    - [8.字符串](#8%E5%AD%97%E7%AC%A6%E4%B8%B2)
    - [9.面向对象-基础](#9%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1-%E5%9F%BA%E7%A1%80)
    - [10.面向对象应用](#10%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E5%BA%94%E7%94%A8)
    - [11.json](#11json)
    - [12.Promise](#12promise)
    - [13.generator-认识生成器函数](#13generator-%E8%AE%A4%E8%AF%86%E7%94%9F%E6%88%90%E5%99%A8%E5%87%BD%E6%95%B0)
    - [14.generator-yield是啥](#14generator-yield%E6%98%AF%E5%95%A5)
    - [15.generator-实例](#15generator-%E5%AE%9E%E4%BE%8B)
    - [16.ES7 预览](#16es7-%E9%A2%84%E8%A7%88)

----

## 1.ES6怎么来的

- ECMAScript 和 JavaScript
    - ECMA 是标准，JS 是实现
    - ECMAScript 简称 ECMA 或 ES

- 历史版本
    - 1996, ES1.0 Netscape 将 JS 提交给 ECMA 组织，ES 正式出现
    - 1999, ES3.0 被广泛支持
    - 2011, ES5.1 成为 ISO 国际标准
    - 2015, ES6.0 正式发布

## 2.ES6兼容性

- ES6(ES2015) 支持的环境 IE10+, Chrome, FireFox, 移动端, NodeJS
- 解决不兼容办法，编译、转换
    - 在线转换
    - 或者提前编译

- [Babel 中文网](https://www.babeljs.cn)
    - [Babel 入门教程 阮一峰](http://www.ruanyifeng.com/blog/2016/01/babel.html)
    - Babel 是一个 JavaScript 编译器
    - 一个广泛使用的转码器，可以将ES6代码转为ES5代码，从而在现有环境执行
    - 现在就用 ES6 编写程序，而不用担心现有环境是否支持

## 3.变量 let 和 常量 const

- var 的问题
    - 可以重复声明，没有报错和警告
    - 无法限制修改
    - 没有块级作用域， `{ }`

- let 和 const
    - 不能重复声明
    - 都是块级作用域, `{ }` 块内声明的，块外无效
    - let 是变量，可以修改
    - const 是常量，不能修改

- 块级作用域举例
    - 原来用 var 的方式，结果弹出的都是 3
    - 或者将变量 封装到函数里，限制作用域，但比较麻烦
    - 用 let 最简单，直接 var 改 let，解决作用域问题

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        window.onload= function () {
            /*
            var aBtn = document.getElementsByTagName('input')
            for (var i=0; i < aBtn.length; i++) {
                aBtn[i].onclick = function () {
                    alert(i)
                }
            }*/
            var aBtn = document.getElementsByTagName('input')
            for (let i = 0; i < aBtn.length; i++) {
                aBtn[i].onclick = function () {
                    alert(i)
                }
            }
            /*
            var aBtn = document.getElementsByTagName('input')
            for (var i = 0; i < aBtn.length; i++) {
                // 封装到函数里，限制作用域
                (function (i) {
                    aBtn[i].onclick = function () {
                        alert(i)
                    }
                })(i)
            }*/
        }
    </script>
</head>
<body>
    <input type="button" value="按钮1">
    <input type="button" value="按钮2">
    <input type="button" value="按钮3">
</body>
</html>
```

## 4.函数-箭头函数

- 箭头函数，就是函数的简写
    - 如果只有一个参数，`()` 可以省
    - 如果只有一个`return`，`{}`可以省

```js
// 普通函数
function name() {

}
// 箭头函数，去掉 function， 加上 =>
() => {

}
```

```js
let show1 = function () {
    console.log('abc')
}

let show2 = () => {
    console.log('abc')
}

show1() // 调用函数
show2()

let show4 = function (a) {
    return a*2
}

let show5 = a => a * 2  //简洁，类似python lambda 函数

console.log(show4(10))
console.log(show5(10))
```

## 5.函数-rest参数

- 参数扩展／展开 `...args`
    - 收集剩余的参数，必须当到最后一个参数位置
    - 展开数组，简写，效果和直接把数组的内容写在这儿一样
- 默认参数

```js
function show(a, b, ...args) {
    console.log(a)
    console.log(b)
    console.log(args)
}
console.log(show(1, 2, 3, 4, 5))

let arr1 = [1, 2, 3]
let arr2 = [4, 5, 6]
let arr3 = [...arr1, ...arr2]
console.log(arr3)

function show2(a, b=5, c=8) {
    console.log(a, b, c)
}
show2(88, 12)
```

## 6.解构赋值

```js
let [a, b, c] = [1, 2, 3]
console.log(a, b, c)

let {x, y, z} = {x: 1, y: 2, z: 3}
console.log(x, y, z)

let [json, arr, num, str] = [{ a: 1, b: 2 }, [1, 2, 3], 8, 'str']
console.log(json, arr, num, str)
```

- 解构赋值
    - 左右两个边结构必须一样
    - 右边必须是个东西
    - 声明和赋值赋值不能分开，必须在一句话里

## 7.数组

- 新增4个方法
- map 映射  一个对一个

```js
let arr = [12, 5, 8]
let result = arr.map(function (item) {
    return item*2
})
let result2 = arr.map(item=>item*2) // 简写
console.log(result)
console.log(result2)

let score = [18, 86, 88, 24]
let result3 = score.map(item => item >= 60 ? '及格' : '不及格')
console.log(result3)

// 结果
[ 24, 10, 16 ]
[ 24, 10, 16 ]
[ '不及格', '及格', '及格', '不及格' ]
```

- reduce 汇总  一堆出来一个
    - 用于比如，算个总数，算个平均

```js
var arr = [1, 3, 5, 7]
var result = arr.reduce(function (tmp, item, index) {
    //tmp 上次结果，item当前数，index次数1开始
    console.log(tmp, item, index)
    return tmp + item
})
console.log(result)

var arr = [1, 3, 5, 7]
var result = arr.reduce(function (tmp, item, index) {
    if (index != arr.length - 1) { // 不是最后一次
        return tmp + item
    } else {
        return (tmp + item)/arr.length
    }
})
console.log(result)  // 平均值
```

- filter 过滤器 保留为true的

```js
var arr = [12, 4, 8, 9]
var result = arr.filter(item => (item % 3 === 0) ? true : false)
console.log(result)
var result = arr.filter(item => item % 3 === 0)
console.log(result)

var arr = [
    { title: '苹果', price: 10 },
    { title: '西瓜', price: 20 },
]
var result = arr.filter(json => json.price >= 20)
console.log(result)
```

- forEach 循环迭代

```js
var arr = [12, 4, 8, 9]
var result = arr.forEach(item => console.log(item))
var result = arr.forEach((item, index)=>console.log(item, index))
```

## 8.字符串

- 多了两个新方法
    - `startsWith`
    - `endsWith`

```js
var url = 'http://qq.com'
console.log(url.startsWith('http'))
console.log(url.endsWith('com'))
// 都是 true
```

- 字符串模版
    - 使用反引号，`${变量}`
    - 可以折行

```js
let a = 12
let str1 = `asdf${a}`
console.log(str1)

let title = '标题'
let content = '内容'
let str = `<div>
<h1>${title}</h1>
<p>${content}</p>
`
console.log(str)
<div>
<h1>标题</h1>
<p>内容</p>
```

## 9.面向对象-基础

- 原来写法
    - 类和构造函数一样
    - 属性和方法分开写的

```js
// 老版本
function User(name, pass) {
    this.name = name
    this.pass = pass
}

User.prototype.showName = function () {
    console.log(this.name)
}
User.prototype.showPass = function () {
    console.log(this.pass)
}

var u1 = new User('able', '1233')
u1.showName()
u1.showPass()
// 老版本继承
function VipUser(name, pass, level) {
    User.call(this, name, pass)
    this.level = level
}
VipUser.prototype = new User()
VipUser.prototype.constructor = VipUser
VipUser.prototype.showLevel = function () {
    console.log(this.level)
}

var v1 = new VipUser('blue', '1234', 3)
v1.showName()
v1.showLevel()

```

- 新版面向对象
    - 有了 class 关键字、构造器
    - class 里面直接加方法
    - 继承，super 超类==父类

```js
class User {
    constructor(name, pass) {
        this.name = name
        this.pass = pass
    }

    showName() {
        console.log(this.name)
    }
    showPass() {
        console.log(this.pass)
    }
}

var u1 = new User('able2', '111')
u1.showName()
u1.showPass()

// 新版本继承
class VipUser extends User {
    constructor(name, pass, level) {
        super(name, pass)
        this.level = level
    }
    showLevel(){
        console.log(this.level)
    }
}

v1 = new VipUser('blue', '123', 3)
v1.showLevel()
```

## 10.面向对象应用

- [React](https://www.reactjscn.com)
    - 用于构建用户界面的 JavaScript 库
    - 组件化，一个组件就是一个 class
    - JSX == bable == browser.js

## 11.json

- JSON 格式
    - JavaScript Object Notation 的缩写，是一种用于数据交换的文本格式
    - JSON 是 JS对象 的严格子集
    - JSON 的标准写法
    - 只能用双引号
    - 所有的key都必须用双引号包起来

- JSON 对象
    - JSON 对象是 JavaScript 的原生对象，用来处理 JSON 格式数据，有两个静态方法
    - JSON.parse(string) ：接受一个 **JSON 字符串**并将其转换成一个 JavaScript **对象**。
    - JSON.stringify(obj) ：接受一个 JavaScript **对象**并将其转换为一个 **JSON 字符串**。

```js
var json = {a: 12, b: 5}
var str = 'hi,' + JSON.stringify(json)
var url = 'http://www.xx.com/' + encodeURIComponent(JSON.stringify(json))
console.log(str)
console.log(url)

var str = '{"a": 12, "b": 4, "c": "abc"}'
var json = JSON.parse(str)
console.log(json)
hi,{"a":12,"b":5}
http://www.xx.com/%7B%22a%22%3A12%2C%22b%22%3A5%7D
{ a: 12, b: 4, c: 'abc' }
```



## 12.Promise

- 异步和同步
    - 异步，操作之间没有关系，同时执行多个操作， 代码复杂
    - 同步，同时只能做一件事，代码简单

- Promise 对象
    - 用同步的方式来书写异步代码
    - Promise 让异步操作写起来，像在写同步操作的流程，不必一层层地嵌套回调函数
    - 改善了可读性，对于多层嵌套的回调函数很方便
    - 充当异步操作与回调函数之间的中介，使得异步操作具备同步操作的接口

- Promise 也是一个构造函数
    - 接受一个回调函数f1作为参数，f1里面是异步操作的代码
    - 返回的p1就是一个 Promise 实例
    - 所有异步任务都返回一个 Promise 实例
    - Promise 实例有一个then方法，用来指定下一步的回调函数

```js
function f1(resolve, reject) {
  // 异步代码...
}
var p1 = new Promise(f1);
p1.then(f2); // f1的异步操作执行完成，就会执行f2。
```

- Promise 使得异步流程可以写成同步流程

```js
// 传统写法
step1(function (value1) {
  step2(value1, function(value2) {
    step3(value2, function(value3) {
      step4(value3, function(value4) {
        // ...
      });
    });
  });
});

// Promise 的写法
(new Promise(step1))
  .then(step2)
  .then(step3)
  .then(step4);
```

- Promise.all(promiseArray)方法
    - 将多个Promise对象实例包装，生成并返回一个新的Promise实例
    - promise数组中所有的promise实例都变为resolve的时候，该方法才会返回
    - 并将所有结果传递results数组中
    - promise数组中任何一个promise为reject的话，则整个Promise.all调用会立即终止，并返回一个reject的新的promise对象

```js
var p1 = Promise.resolve(1),
    p2 = Promise.resolve(2),
    p3 = Promise.resolve(3);
Promise.all([p1, p2, p3]).then(function (results) {
    console.log(results);  // [1, 2, 3]
});
```

- Promise.race([p1, p2, p3])
    - Promse.race就是赛跑的意思
    - 哪个结果获得的快，就返回那个结果
    - 不管结果本身是成功状态还是失败状态

## 13.generator——生成器函数

- generator 生成器函数
    - 普通函数，一路到底
    - generator函数，中间可以停，到哪停呢，用 yield 配合，交出执行权
    - yield 有 放弃、退让、退位的意思
    - 需要调用next()方法启动执行，需要遇到 yield 停, 踹一脚走一步
    - generator函数前面加一个 `*` 两边可以有空格，或靠近函数或`function`
    - 背后实际生成多个小函数，实现走走停停

```js
function show() {
    console.log('a')
    console.log('b')
}
show() // 普通函数

function *show2() {
    console.log('1')
    yield
    console.log('2')
}
let genObj = show2()
genObj.next() // 1
genObj.next() // 2
genObj.next() // 最后了，没有结果
```

------------------------------

- `yield`
    - 既可传参，又可以返回
    - 第一个`next()`传参无效，只用来启动

- 如果函数前漏掉 `*`
    - 就是普通函数
    - 如果有`yield`会报错， `ReferenceError: yield is not defined`
    - yield 只能在Generator函数内部使用

```js
function * show() {
    console.log('1')
    var a = yield
    console.log('2')
    console.log(a)
}
// yield 传参
var gen = show()
gen.next() // 1
gen.next() // 2 和 undefined 因为没有传参，yield没有返回值
var gen = show()
gen.next(10) // 1 第一次执行到yield，但没有执行赋值
gen.next(20) // 2 和 20

function* show2() {
    console.log('1')
    yield 10
    console.log('2')
}
// yield 返回
var gen = show2()
var res1 = gen.next()
console.log(res1) // { value: 10, done: false }
var res2 = gen.next()
console.log(res2)
// { value: undefined, done: true } 最后的value需要return返回
```

---------------------------------

- Promise 适合一次读一组
- generator 适合逻辑性的

```js
// 带逻辑-generator
runner(function * () {
    let userData = yield $.ajax({url: 'getUserData'})

    if (userData.type == 'VIP') {
        let items = yield $.ajax({url: 'getVIPItems'})
    } else {
        let items = yield $.ajax({url: 'getItems'})
    }
})
```

```js
// yield 实例，用同步方式写异步
server.use(function * () {
    let data = yield db.query(`select * from user_table`)
    this.body = data
})
```

## 14.Symbol基本数据类型
```js
let s=Symbol("尚硅谷");
let s2=Symbol("尚硅谷");
console.log(s===s2);// false
let s3=Symbol.for("尚硅谷");
let s4=Symbol.for("尚硅谷");
console.log(s3===s4);// true
```
## 15.iterator迭代器
- `for of遍历`

## 16.异步编程———Promise
> 异步编程像同步编程一样，但是还是异步编程
- 异步Ajax
```js
var xhr = new XMLHttpRequest();
 
xhr.onload = function () {
    // 输出接收到的文字数据
    document.getElementById("demo").innerHTML=xhr.responseText;
}
 
xhr.onerror = function () {
    document.getElementById("demo").innerHTML="请求出错";
}
 
// 发送异步 GET 请求
xhr.open("GET", "https://www.runoob.com/try/ajax/ajax_info.txt", true);
xhr.send();
```
- 优雅的异步——jquery调用
```js
$.get("https://www.runoob.com/try/ajax/demo_test.php",function(data,status){
    alert("数据: " + data + "\n状态: " + status);
});
```
- 回调地狱
```js
setTimeout(function () {
    console.log("First");
    setTimeout(function () {
        console.log("Second");
        setTimeout(function () {
            console.log("Third");
        }, 3000);
    }, 4000);
}, 1000);
```
- 解决回调地狱
```js
new Promise(function (resolve, reject) {
    setTimeout(function () {
        console.log("First");
        resolve();
    }, 1000);
}).then(function () {
    return new Promise(function (resolve, reject) {
        setTimeout(function () {
            console.log("Second");
            resolve();
        }, 4000);
    });
}).then(function () {
    setTimeout(function () {
        console.log("Third");
    }, 3000);
});
```
- Promise基础用法
```js
new Promise(function (resolve, reject) {
    var a = 0;
    var b = 1;
    if (b == 0) reject("Divide zero");
    else resolve(a / b);
}).then(function (value) {
    console.log("a / b = " + value);
}).catch(function (err) {
    console.log(err);
}).finally(function () {
    console.log("End");
});
```
- Promise.all的用法
```js
new Promise
.all([runAsync1(), runAsync2(), runAsync3()])
.then(function(results){
    console.log(results);
});
```
- Ajax的实战应用
```js
function a(){
      return new Promise(function(res,rej){
        $.ajax({
          url:"a接口",
          type: "GET",
          async:true,
          dataType:"json",
          success:function(data){
            console.log(data,"a");
            res(data);
          }
        })
      });
    }
    function b(data){
      console.log(data,"data");
      return new Promise(function(res,rej){
        $.ajax({
            url:"b接口",
            type: "POST",
            async:true,
            data:JSON.stringify(data),
            dataType:"json",
            success:function(data){
              console.log(data,"b");
              res();
            }
          })
      });
    }
    $("#btn").click(function(){
      a().then(function (data){
        b(data);
      }).then(function(){
      })
    })
```
- 设计Promise函数
```js
function print(delay, message) {
    return new Promise(function (resolve, reject) {
        setTimeout(function () {
            console.log(message);
            resolve();
        }, delay);
    });
}
print(1000, "First").then(function () {
    return print(4000, "Second");
}).then(function () {
    print(3000, "Third");
});
```
- 让异步编程更美观——使用async和await
```js
async function asyncFunc() {
    await print(1000, "First");
    await print(4000, "Second");
    await print(3000, "Third");
}
asyncFunc();
```
- 接收返回值的async和await
```js
async function asyncFunc() {
    let value = await new Promise(
        function (resolve, reject) {
            resolve("Return value");
        }
    );
    console.log(value);
}
asyncFunc();
```
## 17.类的继承新写法
- 旧写法——基于原型的继承
```js
function Animal(name) {
  this.name = name;
}
 
Animal.prototype.eat = function() {
  console.log(this.name + " is eating.");
};
 
function Dog(name, breed) {
  Animal.call(this, name);
  this.breed = breed;
}
 
// 建立原型链，让 Dog 继承 Animal
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;
 
Dog.prototype.bark = function() {
  console.log(this.name + " is barking.");
};
 
var dog = new Dog("Buddy", "Labrador");
dog.eat();  // 调用从 Animal 继承的方法
dog.bark(); // 调用 Dog 的方法
```
- 新写法——基于类的继承
```js
class Site {
  constructor(name) {
    this.sitename = name;
  }
  present() {
    return '我喜欢' + this.sitename;
  }
}
 
class Runoob extends Site {
  constructor(name, age) {
    super(name);
    this.age = age;
  }
  show() {
    return this.present() + ', 它创建了 ' + this.age + ' 年。';
  }
}
 
let noob = new Runoob("菜鸟教程", 5);
document.getElementById("demo").innerHTML = noob.show();
```
- getter和setter
```js
class Runoob {
  constructor(name) {
    this.sitename = name;
  }
  get s_name() {
    return this.sitename;
  }
  set s_name(x) {
    this.sitename = x;
  }
}
 
let noob = new Runoob("菜鸟教程");
 
document.getElementById("demo").innerHTML = noob.s_name;
```

## 18.模块化——export和import
[菜鸟教程-ES6 模块](https://www.runoob.com/w3cnote/es6-module.html)
```js
/*-----export [test.js]-----*/
let myName = "Tom";
let myAge = 20;
let myfn = function(){
    return "My name is" + myName + "! I'm '" + myAge + "years old."
}
let myClass =  class myClass {
    static a = "yeah!";
}
export { myName, myAge, myfn, myClass }
 
/*-----import [xxx.js]-----*/
import { myName, myAge, myfn, myClass } from "./test.js";
console.log(myfn());// My name is Tom! I'm 20 years old.
console.log(myAge);// 20
console.log(myName);// Tom
console.log(myClass.a );// yeah!
```
## 19.Map和Set
[菜鸟教程-ES6 Map 与 Set](https://www.runoob.com/w3cnote/es6-map-set.html)
## 20.Reflect和Proxy
[菜鸟教程-ES6 Reflect 与 Proxy](https://www.runoob.com/w3cnote/es6-reflect-proxy.html)
- 基本使用
```js
let target = {
    name: 'Tom',
    age: 24
}
let handler = {
    get: function(target, key) {
        console.log('getting '+key);
        return target[key]; // 不是target.key
    },
    set: function(target, key, value) {
        console.log('setting '+key);
        target[key] = value;
    }
}
let proxy = new Proxy(target, handler)
proxy.name     // 实际执行 handler.get
proxy.age = 25 // 实际执行 handler.set
// getting name
// setting age
// 25
 
// target 可以为空对象
let targetEpt = {}
let proxyEpt = new Proxy(targetEpt, handler)
// 调用 get 方法，此时目标对象为空，没有 name 属性
proxyEpt.name // getting name
// 调用 set 方法，向目标对象中添加了 name 属性
proxyEpt.name = 'Tom'
// setting name
// "Tom"
// 再次调用 get ，此时已经存在 name 属性
proxyEpt.name
// getting name
// "Tom"
 
// 通过构造函数新建实例时其实是对目标对象进行了浅拷贝，因此目标对象与代理对象会互相
// 影响
targetEpt
// {name: "Tom"}
 
// handler 对象也可以为空，相当于不设置拦截操作，直接访问目标对象
let targetEmpty = {}
let proxyEmpty = new Proxy(targetEmpty,{})
proxyEmpty.name = "Tom"
targetEmpty // {name: "Tom"}
```
- 私有属性保护实例
```js
let proxy = new Proxy({}, {
  get(target, propKey, receiver) {
      // 实现私有属性读取保护
      if(propKey[0] === '_'){
          throw new Error(`Invalid attempt to get private     "${propKey}"`);
      }
      console.log('Getting ' + propKey);
      return target[propKey];
  }
});
 
let obj = Object.create(proxy);
obj.name
// Getting name
```
- 拦截不合法的赋值
```js
let validator = {
    set: function(obj, prop, value) {
        if (prop === 'age') {
            if (!Number.isInteger(value)) {
                throw new TypeError('The age is not an integer');
            }
            if (value > 200) {
                throw new RangeError('The age seems invalid');
            }
        }
        // 对于满足条件的 age 属性以及其他属性，直接保存
        obj[prop] = value;
    }
};
let proxy= new Proxy({}, validator)
proxy.age = 100;
proxy.age           // 100
proxy.age = 'oppps' // 报错
proxy.age = 300     // 报错
```
- setter和getter的最后一个参数receiver
```js
const handler = {
    set: function(obj, prop, value, receiver) {
        obj[prop] = receiver;
    }
};
const proxy = new Proxy({}, handler);
proxy.name= 'Tom';
proxy.name=== proxy // true
 
const exam = {}
Object.setPrototypeOf(exam, proxy)
exam.name = "Tom"
exam.name === exam // true
```