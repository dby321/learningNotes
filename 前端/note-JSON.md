    
# JSON(JavaScript Object Notation)
## JSON的作用
- JS的对象---JSON--->Java的对象
- JS的对象---JSON--->PHP的对象
- xxx的对象---JSON--->yyy的对象
## JSON的分类
1. 对象
- `   var obj='{"name":"孙悟空","age":18,"gender":"男"}'   `
2. 数组
- `   var arr='{1,2,3,"hello",true}'                      `
## JSON中允许的值
1. 字符串
2. 数值
3. 布尔值
4. null
5. 对象(不能是含有函数的对象)
6. 数组
## JSON的相互转换
- JSON工具类
- JSON字符串--->JS对象 `JSON.parse(obj)` `JSON.parse(arr)`
- JS对象--->JSON字符串  ` var obj2={name:"董滨雨"} `   ` JSON.stringify(obj2) ` 