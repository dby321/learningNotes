# note-NodeJS
[Gitee-expressHjh/nodejs 笔记](https://gitee.com/coderhjhh/nodejs-/blob/master/%E4%B8%8A%E8%AF%BE%E7%AC%94%E8%AE%B0/nodeJS%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.md)
> 独立的Chrome V8 Javascript运行时环境
> Node.js是js的运行平台
## 1.NodeJS 大局观
### NodeJS提供了这些API
- 文件读写 fs
- 网络操作 http
- 路径操作 url
- 路径处理 path
- 操作系统 os
### NodeJS能做什么
- web服务器后台
- 命令行工具
  - npm
  - hexo
  - webpack
  - gulp
### 安装NodeJS
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
var fs = require('fs');

// 2.读取文件
fs.readFile('./data/a.txt',function(err,data){
   if(err){
        console.log('文件读取失败');
   }
    else{
         console.log(data.toString());
    }
})
```
### CommonJS模块规范
- 导出module.exports
```js
module.exports = {
    foo = 'hello',
    add:function(){
        return x+y;
    }
};
```
- 导入require
```js
var 自定义变量名 = require('模块')
```
### 进一步的例子
```js
// 引用服务
var http = require('http');
var fs = require('fs');
// 引用模板
var template = require('art-template');
// 创建服务
var server = http.createServer();
// 公共路径
var wwwDir = 'D:/app/www';
server.on('request', function (req, res) {
    var url = req.url;
    // 读取文件
    fs.readFile('./template-apche.html', function (err, data) {
        if (err) {
            return res.end('404 Not Found');
        }
        res.end("hello")
        fs.readdir(wwwDir, function (err, files) {
            if (err) {
                return res.end('Can not find www Dir.')
            }
            // 使用模板引擎解析替换data中的模板字符串
            // 去xmpTempleteList.html中编写模板语法
            var htmlStr = template.render(data.toString(), { 
                title: 'D:/app/www/ 的索引',
                files:files 
            });
            // 发送响应数据
            res.end(htmlStr);
        })
    })
});
server.listen(3000, function () {
    console.log('running....');
})
```