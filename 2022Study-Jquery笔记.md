# 2022Study-Jquery

## 原生JS的缺点

> 1. 原生的JS 的API太长太难记了
> 2. 不能添加多个入口函数`window.onload=function(){}`
> 3. 原生的JS有时候有代码冗余
> 4. 原生的JS有些属性或者方法有浏览器兼容性问题
> 5. 原生的JS 容错率比较低，前面的代码出现了问题，后面的代码也会出问题

## 体验Jquery与jquery的优势

```js
// 入口函数
$(document).ready(function(){
    $('#btnOne').click(function(){
        $('div').css('border','1px solid red');
    });
      $('#btnTwo').click(function(){
        $('div').text('我是设置的文本');
    });
});
// 又是一个入口函数
$(document).ready(function(){
   console.log('我又是一个入口函数');
});
```



> Jquery的优势：
>
> 1. 是可以写多个入口函数
> 2. jquery的api都比较好记
> 3. jquery代码简洁（隐式迭代）
> 4. jquery帮我们解决了浏览器兼容问题
> 5. 容错率较高，前面的代码出了问题，后面的代码不受影响



## Jquery简介

> 一个快速、小巧的js库

```js
$(document).ready(function(){
// 链式编程
$('div').width(100).height(100).css('backgroundColor','red').text('哈哈');
});
```

## Jquery的入口函数

```js
// 入口函数 方式1
$(document).ready(function(){

});
// 入口函数 方式2
$(function(){
    
})
```

> 1. Jquery的入口函数可以写多个
> 2. Juery的入口函数 执行时机 在window.onload之前
>    - Jquery入口函数要等待页面上DOM树加载完成后执行
>    - window.onload要等到页面上所有的资源（DOM树/外部css/js连接）都加载完毕后执行

## $是什么

> jquery的结构就是一个匿名的自执行函数
>
> $是一个函数

```js
(function(){
window.jQuery=window.$=jQuery
}())
```

> `$(function(){})`
>
> 如果参数传递的是一个匿名函数---入口函数
>
> ----
>
> `$('one')`   `$('<div>拉拉我是一个div</div>')`
>
> 如果参数传递的是一个字符串---选择器/创建一个标签
>
> -----
>
> `$(dom对象)`
>
> 如果参数是一个dom对象---转换为jquery对象

```js
var $div1=$('#one');
console.log($div1.__proto__===Array.prototype)// false，jquery对象是一个伪数组,jquery对象就是dom对象的一个包装集
```

> jquery对象转换成dom对象
>
> 1. 用下标来取出来 `$div1[0]`
> 2. 使用get()方法 `$div1.get(1)`

## Jquery重要方法

### css() 

> 注意：获取样式操作只会返回第一个元素对应的样式值
>
> 设置样式是设置行内样式
>
> 

`$().css('border-top-width')` IE中获取边框要给准确值

`$().css({width:300,backgroundColor:'300px'})`

## Jquery选择器 Selector

> 和css选择器略有不同

```css
li:nth-child(even) {
    background-color: lightyellow;
}
```

![1664009658550](images/1664009658550.png)

![1664009799388](images/1664009799388.png)

## Jquery事件 Events

![1664009893612](images/1664009893612.png)

## Jquery动画 Effects

## Jquery创建节点

```js
// 原生js
document.write();
document.createElement();
```

```js
//jquery
$().html();// 可以创建元素，可以解析标签
$('<a>xxx</a>')//也能创建元素，这种方法可以调用append，这种更灵活
```

`父节点.append()` 和`父节点.prepend()`有剪切的作用

`兄弟节点.before()`和`兄弟节点.after()`

`子节点.appendTo()`

## Jquery清空节点和移除节点和克隆节点

```js
$().empty();// 清除子节点
$().remove()// 移除自身
$().clone(false)// 克隆HTML结构，参数为true会克隆事件
```

## Jquery获取和设置表单元素的值

```js
$().val();
$().val('xxx');
```

## Jquery操作属性

```js
$().attr();// 如果元素原来没有属性，就添加属性
$().removeAttr();
```

## Jquery操作布尔属性

```js
$().prop();
```

## Jquery操作宽高

```js
$().width();
$().height();
$().innerWidth();// 返回元素的宽，包括内边距
$().innerHeight();
$().outerWidth();// 返回元素的宽，包括内边距和边框
$().outerHeight();
$().outerWidth(true);// 返回元素的宽，包括内边距和边框和外边距
$().outerHeight(true);
$(window).width();// 获取浏览器窗口可视区域的宽 
$(window).height();
$().offset().top;// 会获取元素距离document的top和left的值
$().offset().left;
$().position().top;// 会获取元素距离有定位的父元素的top和left的值
$().position().left;
$().scrollLeft();// 会获取元素左上顶点距离内容最左的距离，即被卷曲出去的距离，可以用来定位精灵图
$().scrollTop();
$(window).scrollLeft();//获取浏览器窗口左上顶点距离内容最左的距离，即被卷曲出去的距离
$(window).scrollTop();
```

> 固定定位和绝对定位会脱离文档流，相对定位和默认定位不会脱离文档流
>
> `position:static`就是默认定位

## Jquery事件注册on，事件解绑off

```js
$(自身节点).on('click',function(){});// on简单注册，不支持动态注册
$(父节点).on('click','子节点',function(){});// on委托注册，支持动态注册
$().off('click');// off解绑click事件
$().off();// off解绑所有事件
```



## Jquery触发事件

```js
$().trigger('click');//触发点击事件
```

## Jquery事件对象

```js
$().on('click',function(e){
	console.log(e);
    console.log(e.screenX,e.screenY,e.clientX,e.clientY);//screenX：触发事件位置距离屏幕最左上角 clientX：触发事件位置距离可视区左上角 pageX:触发事件位置距离页面左上角
    e.stopPropagation();// 阻止事件冒泡
    e.preventDefault();// 阻止默认行为
    // return false;// 既能阻止事件冒泡，又能阻止事件行为
    
});
$(document).on('keydown',function(e){
   console.log(e.keyCode);// 获取键盘按键 
});

```

## Jquery显式迭代

```js
$().each(function(index,element){
	// element是dom对象
});
```

## 查看Jquery版本 多库共存【用的少】

```js
console.log($.jquery)
```

## Jquery插件

> 如果想要Jquery实现背景色渐变动画，就需引入`jquery.color.js`