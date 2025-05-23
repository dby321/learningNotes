# React全家桶笔记

[尚硅谷-张天禹React全家桶](https://www.bilibili.com/video/BV1wy4y1D7JT)

[React官网](https://zh-hans.react.dev/learn)

![](https://pluspng.com/img-png/react-logo-png-react-js-logo-history-design-history-and-evolution-5500x3094.png)

## 1. React简介

React是用于构建用户界面的Javascript库，由Facebook开源，腾讯、阿里广泛使用，前端框架（React、Vue、Angular）三驾马车的老大。

拥有虚拟DOM和优秀的diff算法



react各个文件的作用：

- react.development.js：核心库
- react-dom.development.js：dom核心库

- babel.min.js：ES6=>ES5,JS=>JSX

## 2. HelloWorld

JSX(Javascript XML)

- XML早期用于存储和传输数据，现在用JSON

- JSX用于构建虚拟DOM，虚拟DOM本质上是一个Object,比真实DOM轻

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .sss{
            color: red;
        }
    </style>
</head>
<body>
    <!-- 准备好容器 -->
    <div id="test">
        
    </div>
</body>
<!-- 引入依赖 ,引入的时候，必须就按照这个步骤-->
<script src="../React-js/react.development.js" type="text/javascript"></script>
<script src="../React-js/react-dom.development.js" type="text/javascript"></script>

<script src="../React-js/babel.min.js"></script>
<!--这里使用了js来创建虚拟DOM-->
<script type="text/babel">
        const MyId = "title";
        const MyData = "Cyk";
        // 1.创建虚拟DOM[在这使用了js的语法]React.createElement(标签,标签属性,内容)
        const VDOM = (
            <h1 id = {MyId.toLocaleUpperCase()}>
                <span className = "sss" style = {{fontSize:'50px'}}>sss</span>   
                
            </h1>
        )
        // 2.渲染，如果有多个渲染同一个容器，后面的会将前面的覆盖掉
        ReactDOM.render(VDOM,document.getElementById("test"));
        
        /*
        JS表达式：返回一个值，可以放在任何一个需要值的地方  a  a+b  demo(a)  arr.map() function text(){}
        JS语句：if(){} for(){} while(){} swith(){} 不会返回一个值
        */

        //1.定义虚拟DOM，不能使用“”
        //2.标签中混入JS表达式的时候使用{}
        //3.样式的类名指定不要使用class，使用className
        //4.内敛样式要使用style={{样式:"值"}}
        //5.不能有多个根标签，只能有一个跟标签
        //6.标签必须闭合
        //7.如果小写字母开头，就将标签转化为html同名元素，如果html中无该标签对应的元素，就报错
        // 如果是大写字母开头，react就去渲染对应的组件，如果没有就报错
</script>

</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    
</head>
<body>
    <!-- 准备好容器 -->
    <div id="test">
        
    </div>
</body>
<!-- 引入依赖 ,引入的时候，必须就按照这个步骤-->
<script src="../React-js/react.development.js" type="text/javascript"></script>
<script src="../React-js/react-dom.development.js" type="text/javascript"></script>

<script src="../React-js/babel.min.js"></script>
<!--这里使用了js来创建虚拟DOM-->
<script type="text/babel">
        const data =["1","2","3"];
        // 1.创建虚拟DOM
        const VDOM = (
            <div>
                <h1>hhh</h1>
                <ul>
                    { //{}里面只能放表达式
                        //如果渲染的是一个列表，每一行必须要有一个key
                        data.map((item,index)=>{
                            return <li key = {index}>{item}</li>
                        })
                    }
                </ul>    
            </div>
        )
        // 2.渲染，如果有多个渲染同一个容器，后面的会将前面的覆盖掉
        ReactDOM.render(VDOM,document.getElementById("test"));
        

</script>

</html>
```

