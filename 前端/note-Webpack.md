# Webpack笔记
[Webpack5中文官网](https://www.webpackjs.com/guides/getting-started/)

[webpack超详细教程，学webpack看这一篇就够了！（上）](https://blog.csdn.net/2301_78542842/article/details/138397841)

[webpack超详细教程，学webpack看这一篇就够了！（下）](https://blog.csdn.net/2301_78542842/article/details/138402844)

> Webpack是一个模块打包工具，可以打包js、css、图片、字体等资源，并且可以自动构建依赖关系，可以生成静态资源，可以生成打包后的文件，可以生成打包后的文件
## 1. 起步使用

1. 创建项目，安装webpack和webpack-cli

```bash
mkdir webpack-demo
cd webpack-demo
npm init -y
npm install webpack webpack-cli --save-dev
```

2. 在src文件夹下创建main.js

3. 打包

```bash
npx webpack ./.\src\main.js --mode=development
```

4. 查看文件目录

```bash
$ tree -L 2
.
|-- dist
|   `-- main.js
|-- node_modules
|   |-- @discoveryjs
|   |-- ...
|-- package-lock.json
|-- package.json
`-- src
    |-- js
    `-- main.js

95 directories, 4 files

```

## 2. 六大核心概念

[Webpack中文网 概念](https://www.webpackjs.com/concepts/)

## 3. webpack.config.js

[Webpack中文网 loaders 处理scss sass less等](https://www.webpackjs.com/loaders/)

[CSDN-webpack处理图片资源（jpeg,jpg,png等）](https://blog.csdn.net/jieyucx/article/details/131091801)

- 基础使用

```js
const path = require('path') // 引用path模块
module.exports = {  // 这里是commrnt.js语法
    // 入口文件
    entry:"./src/index.js",
    // 打包后的出口文件
    output:{
        // 输出的路径  是绝对路径(导入path模块) 这里是用node来做的
        path:path.resolve(__dirname,'build'),
        // 输出的文件名称
        filename:'build.js',
    },
    // 使用开发模式打包
    mode:"development"
}

```

- 综合使用配置loader

```js
const path = require('path') // 引用path模块
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 引入html-webpack-plugin

module.exports = {  // 这里是commrnt.js语法
    // 入口文件
    entry:"./src/index.js",
    // 打包后的出口文件
    output:{
        // 输出的路径  是绝对路径(导入path模块) 这里是用node来做的
        path:path.resolve(__dirname,'build'),
        // 输出的文件名称
        filename:'build.js',
        // 自动清空打包内容
        clean:true
    },
    // loader的配置
    module:{
        rules:[
            // 详细loader配置

            // 打包css的配置
            {
                // 使用正则表达式,匹配那些文件
                test: /\.css$/,
                use:[
                    // use数组中loader执行顺序, 从右到左, 从下到上, 依次执行
                    // 创建style标签, 将js中的样式资源插入进行, 添加到head中生效
                    'style-loader',
                    // 将css文件变成commitjs模块加载js中, 里面的内容是样式字符串
                    'css-loader'
                ]
            },
            // 打包scss的配置
            {
                test: /\.scss$/,
                // 注意 是sass-loader ，不是 scss-loader
                // 是数组,代表的是要使用多个loader处理,用use
                use: [ 
                    'style-loader', 
                    'css-loader', 
                    // 将scss文件编译成css文件
                    'sass-loader' 
                ]
            },
            {
                // 问题:默认处理不了html中img图片
                // 处理图片资源
                test: /\.(jpg|png|gif)/,
                // 使用一个loader时,直接写就行了
                loader:'url-loader',
                options:{
                    // 图片大小小于8KB,就会被base64处理
                    // 优点:减少请求数量(减轻服务器压力)
                    // 缺点:图片体积会增大, 就会导致文件请求速度更慢
                    limit: 8 * 1024,
                    // 问题: 因为url-loader默认使用es6模块化解析, 
                    // 而html-loader引入图片是commonjs
                    // 解析时会出问题: [object Module]
                    // 解决: 关闭url-loader的es6模块化, 使用commonjs解析
                    esModule: false,
                    // 给图片进行重命名
                    // [hash:10]取图片的hash的前10位
                    // [ext]去文件原来扩展名
                    name: '[hash:10].[ext]'
                }
            },
            {
                test:/\.html$/,
                // 处理html文件的img图片 (负责引入img, 从而能被url-loader进行处理)
                loader:'html-loader'
            }
        ]
    },
    // plugins的配置
    plugins:[
        // 详细plugins的配置

        // 因为是直接引的,所以 new 就好了
        // 功能:默认会创建一个空的HTML文件,自动引入打包输出的所有资源(JS/CSS)
        new HtmlWebpackPlugin({
			// 复制 './src/index.html'文件,并自动引入打包输出的所有资源(JS/CSS)
			template:'./src/index.html'
		})
    ],  
    // 使用开发模式打包
    mode:"development",

    // 开发服务器 devServer:用来自动化(自动编译,自动打开浏览器,自动刷新浏览器)
    // 用简单概括就是: 热更新
    // 特点:只会在内存中编译打包,不会有任何输出
    // 启动指令: webpack-dev-server 你也可以在package.json文件中重新修改指令
	devServer:{
        contentBase:path.resolve(__dirname,'build'),
        // 启动gzip压缩
        compress:true,
        // 修改端口号
        port:8080,
        //自动打开浏览器
		open:true
    }
}

```



## 附录

### tree命令打印目录结构

[tree windows安装](https://sourceforge.net/projects/gnuwin32/)

- 需要配置环境变量
- 在git bash中启动使用
- [tree命令使用参考](https://www.cnblogs.com/ricolee/p/cmd-tree.html)

### -S和-D的区别

  npm安装时-S -D作用及区别:

- -S 即--save（保存）包名会被注册在package.json的dependencies里面，在生产环境下这个包的依赖依然存在
- -D 即--dev（生产）包名会被注册在package.json的devDependencies里面，仅在开发环境下存在的包用-D，如babel,sass-loader这些解析器

### 安装loader 

- 添加能使css一起打包的插件:` npm i css-loader style-loader -D`
- 添加能使scss一起打包的插件: `npm install sass-loader node-sass webpack -D`  因为你刚刚下载过 css-loader style-loader所以不用再次下载了
- 添加能使html一起打包的插件: `npm i html-webpack-plugin -D`
- 添加能使图片打包的插件:` npm i url-loader file-loader -D`
- 添加能使html中图片打包的插件: `npm i html-loader -D`
- 添加能使浏览器自动更新(刷新)的插件: `npm i webpack-dev-server -D`

### 打包命令

- 开发环境:` webpack ./src/index.js -o ./build/build.js --mode=development`
- 生产环境: `webpack ./src/index.js -o ./build/build.js --mode=production`
