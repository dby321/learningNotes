# note-Typescript

[菜鸟教程-TS 教程](https://www.runoob.com/typescript/ts-tutorial.html)
[菜鸟教程-TS 数据类型](https://www.runoob.com/typescript/ts-type.html)
[阮一峰-TS教程](https://wangdoc.com/typescript/)
| 任意类型   | any       | 声明为 any 的变量可以赋予任意类型的值。                                                                                                                                                                                                    |
| ---------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 数字类型   | number    | 双精度 64 位浮点值。它可以用来表示整数和分数。`let binaryLiteral: number = 0b1010; // 二进制 let octalLiteral: number = 0o744;    // 八进制 let decLiteral: number = 6;    // 十进制 let hexLiteral: number = 0xf00d;    // 十六进制`      |
| 字符串类型 | string    | 一个字符系列，使用单引号（**'**）或双引号（**"**）来表示字符串类型。反引号（**`**）来定义多行文本和内嵌表达式。`let name: string = "Runoob"; let years: number = 5; let words: string = `您好，今年是 ${ name } 发布 ${ years + 1} 周年`;` |
| 布尔类型   | boolean   | 表示逻辑值：true 和 false。`let flag: boolean = true;`                                                                                                                                                                                     |
| 数组类型   | 无        | 声明变量为数组。`// 在元素类型后面加上[] let arr: number[] = [1, 2]; // 或者使用数组泛型 let arr: Array<number> = [1, 2];`                                                                                                                 |
| 元组       | 无        | 元组类型用来表示已知元素数量和类型的数组，各元素的类型不必相同，对应位置的类型需要相同。`let x: [string, number]; x = ['Runoob', 1];    // 运行正常 x = [1, 'Runoob'];    // 报错 console.log(x[0]);    // 输出 Runoob`                    |
| 枚举       | enum      | 枚举类型用于定义数值集合。`enum Color {Red, Green, Blue}; let c: Color = Color.Blue; console.log(c);    // 输出 2`                                                                                                                         |
| void       | void      | 用于标识方法返回值的类型，表示该方法没有返回值。`function hello(): void {    alert("Hello Runoob"); }`                                                                                                                                     |
| null       | null      | 表示对象值缺失。                                                                                                                                                                                                                           |
| undefined  | undefined | 用于初始化变量为一个未定义的值                                                                                                                                                                                                             |
| never      | never     | never 是其它类型（包括 null 和 undefined）的子类型，代表从不会出现的值。                                                                                                                                                                   |

## TS 类型声明

```js
// 属性名后面加上？表示可选属性
let b:{name:string,age?:number};
b = {name:'Runoob'};
b = {name:'Runoob',age:18};
// 属性名后面加上：any表示任意类型
let c={name:string,[propName:string]:any};
c = {name:'Runoob',age:18};
c = {name:'Runoob',age:18,sex:'男'};
// 函数类型
let d:(a:number,b:number)=>number
d=function(a,b){
    return a+b
}
// 起别名
type Mytype=string
let z:Mytype

type Mytype=1|2|3|4
let z:Mytype
```

## TS 编译选项

```
tsc app.ts -t es5 -m commonjs
tsc app.ts --target es5 --module commonjs
tsc app.ts --target es5 --module commonjs --outFile app.js
tsc app.ts --target es5 --module commonjs --out app.js
<!-- 监听文件变化 -->
tsc app.ts -w
<!-- 编译所有文件 需要创建tsconfig.json文件 -->
tsc
```

### TS 配置文件

```js
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true
  },
  "include": [
    "./src/**/*"
    ],
  "exclude": [
    "./node_modules"
    ],
  "compileOnSave": true,
  // 合并输出文件
  "outFile": "./dist/app.js",
}
```
