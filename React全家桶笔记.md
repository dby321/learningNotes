# React全家桶笔记

[尚硅谷-张天禹React全家桶](https://www.bilibili.com/video/BV1wy4y1D7JT)

[React官网](https://zh-hans.react.dev/learn)

![](https://pluspng.com/img-png/react-logo-png-react-js-logo-history-design-history-and-evolution-5500x3094.png)

## 1. React 简介

React是用于构建用户界面的Javascript库，由Facebook开源，腾讯、阿里、美团广泛使用，前端框架（React、Vue、Angular）三驾马车的老大。

拥有虚拟DOM和优秀的diff算法



react各个文件的作用：

- react.development.js：核心库
- react-dom.development.js：dom核心库

- babel.min.js：ES6=>ES5,JS=>JSX

## 2. React 建立虚拟DOM并渲染

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

## 3. React 组件

### 函数式组件

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
    
    <div id = "div">
        
    </div>

</body>
<!-- 引入依赖 ,引入的时候，必须就按照这个步骤-->
<script src="../React-js/react.development.js" type="text/javascript"></script>
<script src="../React-js/react-dom.development.js" type="text/javascript"></script>

<script src="../React-js/babel.min.js" type="text/javascript"></script>


<!--这里使用了babel用来解析jsx语法-->
<script type="text/babel">
        // 1.创建函数式组件 
        function Demo(){
            console.log(this); //这个this是undefined,因为babel编译之后开启了严格模式
            return <h2>w shi yi ge zu jian</h2>
        }
        //函数式组件还可以添加参数
        function Show(props){
            return <h1>Hello {props.age}</h1>
        }
        //2.渲染组件

        ReactDOM.render(<Demo />,document.getElementById("test"));
        ReactDOM.render(<Show age="ss" />,document.getElementById("div"));
        //注意事项：
        /*
        1.组件的首字母必须是大写，并且有返回值
        2.在渲染的时候必须<组件名 />
        */

       //执行过程：
       //1.React解析组件标签，找到相应的组件。
       //2.发现组件是函数定义的，随后调用函数，将返回的虚拟DOM转化为真实DOM,随后呈现在页面中。

        //在类中，一般方法是放在原型对象上的，供实例使用
        //方法中的this主要取·决于是谁调用了他
</script>



</html>
```

### 类式组件

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

<script src="../React-js/babel.min.js" type="text/javascript"></script>


<!--这里使用了babel用来解析jsx语法-->
<script type="text/babel">
        // 1.创建类式组件 [必须继承React.Component]
        class MyComponent extends React.Component{
            render(){
                //render是放在原型对象上的，供实例对象使用
                //render的this，MyComponent的实例对象【组件实例对象】
                return <h1>这个是类组件</h1>
            }
        }
        
        //2.渲染组件
        ReactDOM.render(<MyComponent />,document.getElementById("test"))

       

       //执行过程：
       //1.React解析组件标签，找到相应的组件
       //2.发现组件是类定义的，随后new出来的类的实例，并通过该实例调用到原型上的render方法
       //3.将render返回的虚拟DOM转化为真实的DOM,随后呈现在页面中


       //复杂组件：有状态的
       //简单组件：没有状态的
       //组件的状态里面保存着数据 state
</script>



</html>
```

