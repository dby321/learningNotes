# note-NodeJS

[Gitee-expressHjh/nodejs 笔记](https://gitee.com/coderhjhh/nodejs-/blob/master/%E4%B8%8A%E8%AF%BE%E7%AC%94%E8%AE%B0/nodeJS%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.md)

> 独立的 Chrome V8 Javascript 运行时环境
> Node.js 是 js 的运行平台

## 1.NodeJS 大局观

### NodeJS 提供了这些 API

- 文件读写 fs
- 网络操作 http
- 路径操作 url
- 路径处理 path
- 操作系统 os

### NodeJS 能做什么

- web 服务器后台
- 命令行工具
  - npm
  - hexo
  - webpack
  - gulp

### 安装 NodeJS

> https://nodejs.org/en/

### Hello NodeJS

```js
//浏览器中的JavaScript是没有文件操作能力的
//但是Node中的JavaScript具有文件操作能力
//fs是file-system的简写，就是文件系统的意思
//在Node中如果想要进行文件的操作就必须引用fs这个核心模块
//在fs这个和兴模块中，就提供了人所有文件操作相关的API
//例如 fs.readFile就是用来读取文件的

//  1.使用fs核心模块
var fs = require("fs");

// 2.读取文件
fs.readFile("./data/a.txt", function (err, data) {
  if (err) {
    console.log("文件读取失败");
  } else {
    console.log(data.toString());
  }
});
```

### CommonJS 模块规范

- 导出 module.exports

```js
module.exports = {
    foo = 'hello',
    add:function(){
        return x+y;
    }
};
```

- 导入 require

```js
var 自定义变量名 = require("模块");
```

### 进一步的例子

```js
// 引用服务
var http = require("http");
var fs = require("fs");
// 引用模板
var template = require("art-template");
// 创建服务
var server = http.createServer();
// 公共路径
var wwwDir = "D:/app/www";
server.on("request", function (req, res) {
  var url = req.url;
  // 读取文件
  fs.readFile("./template-apche.html", function (err, data) {
    if (err) {
      return res.end("404 Not Found");
    }
    res.end("hello");
    fs.readdir(wwwDir, function (err, files) {
      if (err) {
        return res.end("Can not find www Dir.");
      }
      // 使用模板引擎解析替换data中的模板字符串
      // 去xmpTempleteList.html中编写模板语法
      var htmlStr = template.render(data.toString(), {
        title: "D:/app/www/ 的索引",
        files: files,
      });
      // 发送响应数据
      res.end(htmlStr);
    });
  });
});
server.listen(3000, function () {
  console.log("running....");
});
```

## 2.Buffer

[菜鸟教程-Node.js Buffer(缓冲区)](https://www.runoob.com/nodejs/nodejs-buffer.html)

> Buffer 是 Node.js 中一个全局对象，用来处理二进制数据

- 创建 Buffer 类

```js
// 创建一个长度为 10、且用 0 填充的 Buffer。
const buf1 = Buffer.alloc(10);

// 创建一个长度为 10、且用 0x1 填充的 Buffer。
const buf2 = Buffer.alloc(10, 1);

// 创建一个长度为 10、且未初始化的 Buffer。
// 这个方法比调用 Buffer.alloc() 更快，
// 但返回的 Buffer 实例可能包含旧数据，
// 因此需要使用 fill() 或 write() 重写。
const buf3 = Buffer.allocUnsafe(10);

// 创建一个包含 [0x1, 0x2, 0x3] 的 Buffer。
const buf4 = Buffer.from([1, 2, 3]);

// 创建一个包含 UTF-8 字节 [0x74, 0xc3, 0xa9, 0x73, 0x74] 的 Buffer。
const buf5 = Buffer.from("tést");

// 创建一个包含 Latin-1 字节 [0x74, 0xe9, 0x73, 0x74] 的 Buffer。
const buf6 = Buffer.from("tést", "latin1");
```

- 写入缓冲区

```js
buf = Buffer.alloc(256);
len = buf.write("www.runoob.com");

console.log("写入字节数 : " + len);
```

## 3.fs 模块

[菜鸟教程-Node.js fs 模块](https://www.runoob.com/nodejs/nodejs-fs-module.html)

> fs 模块提供了一些方法用于读取和写入文件

### fs.writeFile()

```js
const fs=require('fs');
fs.writeFile('fs-test.txt','hello world',err=>{
    if(err){
        console.log(err);
        return;
    }
    console.log('写入成功');)
});
```

### fs.createWriteStream()

```js
let ws=fs.createWriteStream('fs-test.txt');
ws.write('hello world');
ws.write("hello world2")；
ws.close();
```

## 4.path 模块

[菜鸟教程-Node.js path 模块](https://www.runoob.com/nodejs/nodejs-path-module.html)

> path 模块提供了一些用于处理文件路径的小工具

### path.resolve()

```js
const path = require("path");
path.resolve("/foo/bar", "./baz");
// 返回: '/foo/bar/baz'

path.resolve("/foo/bar", "/tmp/file/");
// 返回: '/tmp/file'

path.resolve("wwwroot", "static_files/png/", "../gif/image.gif");
// 如果当前工作目录为 /home/myself/node，
// 则返回 '/home/myself/node/wwwroot/static_files/gif/image.gif'
```

### sep 分隔符

```js
const path = require("path");
console.log(path.sep); // windows \ linux /
```

### **filename 和**dirname

```js
const path = require("path");
console.log(__filename); // /Users/hjh/Desktop/nodejs/path.js
console.log(__dirname); // /Users/hjh/Desktop/nodejs
```

### path.parse()

```js
const path=require('path');
console.log(path.parse(__filename));
//     {
//     root: '/',
//     dir: '/Users/hjh/Desktop/nodejs',
//     base: 'path.js',
//     ext: '.js',
//     name: 'path'
//      }

}
```

## 5.http 模块

[菜鸟教程-Node.js http 模块](https://www.runoob.com/nodejs/nodejs-http-module.html)

> http 模块主要用于创建一个 http 服务器

```js
const http = require("http");
http
  .createServer((req, res) => {
    res.writeHead(200, { "Content-Type": "text/plain" });
    res.end("Hello World\n");
  })
  .listen(3000, "127.0.0.1");
console.log("Server running at http://127.0.0.1:3000/");
// 运行结果：
// Server running at http://127.0.0.1:3000/
// Hello World
```

## 6.Node.js 中的模块化

[菜鸟教程-Node.js 中的模块化](https://www.runoob.com/nodejs/nodejs-module-system.html)

> Node.js 中的模块化分为两种：CommonJS 和 ES6 模块化

### CommonJS 模块化

- 导出 module.exports

```js
module.exports = {
    foo = 'hello',
    add:function(){
        return x+y;
    }
};
```
- 导入 require
```js
var foo = require("./foo");
console.log(foo.foo);
```
### ES6 模块化
- 导出 export
```js
export var foo = 'hello';
export function add(x,y){
    return x+y;
}
```
- 导入 import
```js
import {foo,add} from './foo';
console.log(foo);
console.log(add(1,2));
```
## 7.NPM的使用
[菜鸟教程-NPM使用介绍](https://www.runoob.com/nodejs/nodejs-npm.html)
## 8.Express框架
[菜鸟教程-Express框架](https://www.runoob.com/nodejs/nodejs-express.html)
> 就是一个WEB应用开发框架，可以快速创建一个WEB应用
