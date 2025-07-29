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

# 

## 4. 类型推断

类型声明并不是必需的，如果没有，TypeScript 会自己推断类型。

```
let foo = 123;
```

上面示例中，变量`foo`并没有类型声明，TypeScript 就会推断它的类型。由于它被赋值为一个数值，因此 TypeScript 推断它的类型为`number`。

后面，如果变量`foo`更改为其他类型的值，跟推断的类型不一致，TypeScript 就会报错。

```
let foo = 123;
foo = 'hello'; // 报错
```



## 5. Typescript的编译

[TypeScript Playground（TS在线编译）](http://www.typescriptlang.org/play/)

### 5.1 tsc编译器

```
npm install -g typescript # 安装npm依赖

tsc -v # 检查版本
tsc -h # 帮助文档

tsc file1.ts file2.ts file3.ts # 编译多个文件
tsc file1.ts file2.ts --outFile app.js # 指定编译输出文件
tsc app.ts --outDir dist # 指定编译输出目录
tsc --target es2015 app.ts # 指定编译后的 JavaScript 版本。建议使用es2015，或者更新版本
tsc --noEmitOnError app.ts # 如果希望一旦报错就停止编译，不生成编译产物，可以使用--noEmitOnError参数。
tsc --noEmit app.ts # --noEmit参数，只检查类型是否正确，不生成 JavaScript 文件。
tsc --noImplicitAny app.ts # 只要推断出any类型就会报错
```

### 5.2 tsconfig.json

```
tsc file1.ts file2.ts --outFile dist/app.js
```

等效于

```json
{
  "files": ["file1.ts", "file2.ts"],
  "compilerOptions": {
    "outFile": "dist/app.js"
  }
}
```

编译时直接调用`tsc`就可以了

### 5.3 ts-node模块

```
npm install -g ts-node # 安装ts-node依赖
ts-node script.ts # 直接运行Typescript脚本
```

## 6. any的污染问题

`any`类型除了关闭类型检查，还有一个很大的问题，就是它会“污染”其他变量。它可以赋值给其他任何类型的变量（因为没有类型检查），导致其他变量出错。

```
let x:any = 'hello';
let y:number;

y = x; // 不报错

y * 123 // 不报错
y.toFixed() // 不报错
```

上面示例中，变量`x`的类型是`any`，实际的值是一个字符串。变量`y`的类型是`number`，表示这是一个数值变量，但是它被赋值为`x`，这时并不会报错。然后，变量`y`继续进行各种数值运算，TypeScript 也检查不出错误，问题就这样留到运行时才会暴露。

污染其他具有正确类型的变量，把错误留到运行时，这就是不宜使用`any`类型的另一个主要原因。

## 7. unkown类型

为了解决`any`类型“污染”其他变量的问题，TypeScript 3.0 引入了[`unknown`类型](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-0.html#new-unknown-top-type)。它与`any`含义相同，表示类型不确定，可能是任意类型，但是它的使用有一些限制，不像`any`那样自由，可以视为严格版的`any`。

`unknown`跟`any`的相似之处，在于所有类型的值都可以分配给`unknown`类型。

```
let x:unknown;

x = true; // 正确
x = 42; // 正确
x = 'Hello World'; // 正确
```

上面示例中，变量`x`的类型是`unknown`，可以赋值为各种类型的值。这与`any`的行为一致。

`unknown`类型跟`any`类型的不同之处在于，它不能直接使用。主要有以下几个限制。

首先，`unknown`类型的变量，不能直接赋值给其他类型的变量（除了`any`类型和`unknown`类型）。

```
let v:unknown = 123;

let v1:boolean = v; // 报错
let v2:number = v; // 报错
```

上面示例中，变量`v`是`unknown`类型，赋值给`any`和`unknown`以外类型的变量都会报错，这就避免了污染问题，从而克服了`any`类型的一大缺点。

其次，不能直接调用`unknown`类型变量的方法和属性。

```
let v1:unknown = { foo: 123 };
v1.foo  // 报错

let v2:unknown = 'hello';
v2.trim() // 报错

let v3:unknown = (n = 0) => n + 1;
v3() // 报错
```

上面示例中，直接调用`unknown`类型变量的属性和方法，或者直接当作函数执行，都会报错。

再次，`unknown`类型变量能够进行的运算是有限的，只能进行比较运算（运算符`==`、`===`、`!=`、`!==`、`||`、`&&`、`?`）、取反运算（运算符`!`）、`typeof`运算符和`instanceof`运算符这几种，其他运算都会报错。

```
let a:unknown = 1;

a + 1 // 报错
a === 1 // 正确
```

上面示例中，`unknown`类型的变量`a`进行加法运算会报错，因为这是不允许的运算。但是，进行比较运算就是可以的。

那么，怎么才能使用`unknown`类型变量呢？

答案是只有经过“类型缩小”，`unknown`类型变量才可以使用。所谓“类型缩小”，就是缩小`unknown`变量的类型范围，确保不会出错。

```
let a:unknown = 1;

if (typeof a === 'number') {
  let r = a + 10; // 正确
}
```

上面示例中，`unknown`类型的变量`a`经过`typeof`运算以后，能够确定实际类型是`number`，就能用于加法运算了。这就是“类型缩小”，即将一个不确定的类型缩小为更明确的类型。

下面是另一个例子。

```
let s:unknown = 'hello';

if (typeof s === 'string') {
  s.length; // 正确
}
```

上面示例中，确定变量`s`的类型为字符串以后，才能调用它的`length`属性。

这样设计的目的是，只有明确`unknown`变量的实际类型，才允许使用它，防止像`any`那样可以随意乱用，“污染”其他变量。类型缩小以后再使用，就不会报错。

总之，`unknown`可以看作是更安全的`any`。一般来说，凡是需要设为`any`类型的地方，通常都应该优先考虑设为`unknown`类型。

在集合论上，`unknown`也可以视为所有其他类型（除了`any`）的全集，所以它和`any`一样，也属于 TypeScript 的顶层类型。

## 8. never类型

为了保持与集合论的对应关系，以及类型运算的完整性，TypeScript 还引入了“空类型”的概念，即该类型为空，不包含任何值。

由于不存在任何属于“空类型”的值，所以该类型被称为`never`，即不可能有这样的值。

```
let x:never;
```

上面示例中，变量`x`的类型是`never`，就不可能赋给它任何值，否则都会报错。

`never`类型的使用场景，主要是在一些类型运算之中，保证类型运算的完整性，详见后面章节。另外，不可能返回值的函数，返回值的类型就可以写成`never`，详见《函数》一章。

如果一个变量可能有多种类型（即联合类型），通常需要使用分支处理每一种类型。这时，处理所有可能的类型之后，剩余的情况就属于`never`类型。

```
function fn(x:string|number) {
  if (typeof x === 'string') {
    // ...
  } else if (typeof x === 'number') {
    // ...
  } else {
    x; // never 类型
  }
}
```

上面示例中，参数变量`x`可能是字符串，也可能是数值，判断了这两种情况后，剩下的最后那个`else`分支里面，`x`就是`never`类型了。

`never`类型的一个重要特点是，可以赋值给任意其他类型。

```
function f():never {
  throw new Error('Error');
}

let v1:number = f(); // 不报错
let v2:string = f(); // 不报错
let v3:boolean = f(); // 不报错
```

上面示例中，函数`f()`会抛出错误，所以返回值类型可以写成`never`，即不可能返回任何值。各种其他类型的变量都可以赋值为`f()`的运行结果（`never`类型）。

为什么`never`类型可以赋值给任意其他类型呢？这也跟集合论有关，空集是任何集合的子集。TypeScript 就相应规定，任何类型都包含了`never`类型。因此，`never`类型是任何其他类型所共有的，TypeScript 把这种情况称为“底层类型”（bottom type）。

总之，TypeScript 有两个“顶层类型”（`any`和`unknown`），但是“底层类型”只有`never`唯一一个。

## 9. 额外的类型

### 9.1 bigint

bigint 类型包含所有的大整数。

```
const x:bigint = 123n;
const y:bigint = 0xffffn;
```

上面示例中，变量`x`和`y`就属于 bigint 类型。

bigint 与 number 类型不兼容。

```
const x:bigint = 123; // 报错
const y:bigint = 3.14; // 报错
```

上面示例中，`bigint`类型赋值为整数和小数，都会报错。

注意，bigint 类型是 ES2020 标准引入的。如果使用这个类型，TypeScript 编译的目标 JavaScript 版本不能低于 ES2020（即编译参数`target`不低于`es2020`）。

### 9.2 Symbol

symbol 类型包含所有的 Symbol 值。

```
const x:symbol = Symbol();
```

上面示例中，`Symbol()`函数的返回值就是 symbol 类型。
