# Webpack笔记
## Webpack5图片loader的改变
```$xslt
module: {
        rules: [
            {
                test: /\.(png|jpg|gif)$/,
                type: 'asset/resource',
            },
    }
```
## 导入后缀的简写
```$xslt
resolve:{
        extensions:[".js",".vue"]
    }
```

## 通过起别名，使用runtime-compiler而不是runtime-only

```$xslt
 resolve:{
        alias:{
             "vue$":"vue/dist/vue.esm.js"
         }
    },
```
## webpack-dev-server版本问题
"webpack": "^5.58.2",
"webpack-cli": "^4.9.0",
"webpack-dev-server": "^3.11.2"
## webpackMerge not a function问题
```WebpackMerge.merge()```
