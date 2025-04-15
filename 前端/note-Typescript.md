# note-Typescript

[菜鸟教程-TS 教程](https://www.runoob.com/typescript/ts-tutorial.html)
[阮一峰-TS教程](https://wangdoc.com/typescript/)
[Quick Reference-TypeScript 备忘清单](https://quickref.cn/docs/typescript.html)

[TypeScript入门教程](https://ts.xcatliu.com/introduction/index.html)


## 1. 基础类型

```ts
let a:number;
let b:string;
let c:boolean;
let d:any;
let e:number[]=[1,2,3];
let f:Array<number>=[1,2,3];
let g:string[]=[1,2,3];
let h:any[]=[1,2,3];
let i:10;// 字面量 很少使用
let i:number|string;
let j:number|string|boolean;
let k:number|string|boolean|number[];
```
- `unknown`是一个类型安全的any，它比any更安全，它不能赋值给任何类型，但是可以赋值给unkown类型。
```ts
let a:unknown;
a="hello";
let s:string;
s=a as string;
```
- `void`主要用来表示函数没有返回值，也可以用来表示undefined和null。
- `never`表示永远不会返回。
- `object`表示任意对象,包含类、接口、函数、数组、元组等。更常用的是`{}`
  - `?`表示可选属性
  - `[propName:string]:any`表示任意属性
```ts
let a:{name:string,age?:number};
a={name:"hello",age:18};
a={name:"hello"};
let b:{name:string,[propName:string]:any};
b={name:"hello",age:18};
b={name:"hello",age:18,sex:"男"};
```
- `function`函数类型 
```ts
let a:(x:number,y:number)=>number;
a=(x,y)=>x+y;
```
- `enum`枚举类型，用来定义一个取值被限定在一定范围内的变量。比如，性别，星期，月份等。
```ts
enum Gender{Male,Female}
let a:Gender=Gender.Male;
```
- `tuple`元组类型，用来表示一个已知长度的数组。
```ts
let a:[string,number]=["hello",18];
let b:[string,number,boolean]=["hello",18,true];
let c:[string,number,boolean,number[]]=["hello",18,true,[1,2,3]];
```
## 2. 类型别名
```ts
type Gender=1|2;
type Person={
  name:string;
  age:number;
  gender:Gender;
  address:string;
}
let a:Person={
  name:"hello",
  age:18,
  gender:1,
  address:"beijing"
};
```
## 3. TS自动监视文件变化
- `tsc -w`
- `tsc --watch`
- `tsc` 默认只监视ts文件，不监视js文件，如果需要监视js文件，需要加上`--outDir`参数。
  - 需要配置`tsconfig.json`文件 里面什么都不写就能监视
  ```json
  {
  }
  ```
