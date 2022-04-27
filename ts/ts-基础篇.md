# 1 原始数据类型



## 1.1 布尔值

```typescript
let isDone: boolean = false;
```

**注意，使用构造函数 Boolean 创造的对象不是布尔值：**

```typescript
let createdByNewBoolean: boolean = new Boolean(1);
// Type 'Boolean' is not assignable to type 'boolean'.
```

`Boolean` 是一个包装器对象，`boolean` 是一个原始类型。 

事实上 `new Boolean()` 返回的是一个 `Boolean` 对象：

直接调用 `Boolean` 也可以返回一个 boolean 类型

```typescript
let createdByNewBoolean: boolean = Boolean(1);
```

在 TypeScript 中 boolean 是 JavaScript 中的基本类型，而 `Boolean` 是 JavaScript 中的构造函数。其他基本类型（除了 `null` 和 `undefined`）一样， 

```typescript
const str: string = new String('22') // false
const str: String = new String()  // true
```

## 1.2 数值

```ts
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
// ES6 中的二进制表示法
let binaryLiteral: number = 0b1010;
// ES6 中的八进制表示法
let octalLiteral: number = 0o744;
let notANumber: number = NaN;
let infinityNumber: number = Infinity;
```

## 1.3 字符串

```
let name: string = 'Tom';
let sentence: string = `Hello, my name is ${name}.I'll be ${age + 1} years old next month.`;
```

## 1.4 空值（void）

void 类型表示没有任何返回值的函数，或返回时为 null 或 undefined。

变量声明为 void类型，也只能将它赋值为 `undefined` 或 `null`

```typescript
function fun():void{}
const f = ():void=>{}
```

## 1.5 Null 和 Undefined

```typescript
let u: undefined = undefined;
let n: null = null
```

与 `void` 的区别是，`undefined` 和 `null` 是所有类型的子类型，例如：

```typescript
let num: number = null
let str: string = undefined
```

而 `void` 类型不能。



# 2 元组

## 2.1 用法

元组是为数组提供的一种类型规范

```typescript
let arr: [string, number] = ['tom', 3]
let arr2: [string, number]
arr2[0] = 'tom'
arr2[1] = 3
```

当添加越界元素时，被添加的元素的类型会被限制为元组中每个类型的联合类型

```typescript
const arr: [string, number] = ['tom', 3]
arr.push(true) 
// Argument of type 'true' is not assignable to parameter of type 'string | number'
```



# 3 枚举类型

枚举（Enum）类型用于取值被限定在一定范围内的时候，如一周七天，一些限定颜色

## 3.1 用法

若不设置值，枚举成员的值会从 0 开始递增，步长为 1

```typescript
enum Days { Sun, Mon, Tue, Wed, Thus, Fri, Sat }
```

未设置值的枚举成员会接着上一个枚举项递增，步长仍为 1

```typescript
enum Days { Sun = 1, Mon, Tue, Wed, Thus, Fri, Sat }
//Mon == 2, Tue == 3, Wed ==4, Thus == 5, Fri == 6, Sat == 7
```

若枚举成员前一个成员的值不是 number 的话，后面的无法获得初始值而导致无法进行递增，所以会报错

```typescript
enum Days { Sun = 1, Mon = '2', Tue, Wed, Thus, Fri, Sat } // Error 枚举成员必须具有初始化表达式。
```

若前一个为 `NaN` 或 `Infinity` ，则后面均为 NaN，除非手动赋值

```typescript
enum Days { Sun = 1, Mon = NaN, Tue, Wed, Thus, Fri, Sat } 
// Tue = NaN, Wed = NaN, Thus = NaN, Fri = NaN, Sat = NaN

enum Days { Sun = 1, Mon = Infinity, Tue, Wed, Thus, Fri, Sat } 
// Tue = Infinity, Wed = Infinity, Thus = Infinity, Fri = Infinity, Sat = Infinity
```

可见，枚举成语是有值的，若没有设置值，



## 3.2 反向映射

枚举成员会被赋值为从 `0` 开始递增的数字，同时也会对枚举值到枚举名进行反向映射：

```ts
enum Days { Sun, Mon, Tue, Wed, Thu, Fri, Sat }

console.log(Days['Sun'] === 0); // true
console.log(Days['Mon'] === 1); // true
console.log(Days['Tue'] === 2); // true
console.log(Days['Sat'] === 6); // true

console.log(Days[0] === 'Sun'); // true
console.log(Days[1] === 'Mon'); // true
console.log(Days[2] === 'Tue'); // true
console.log(Days[6] === 'Sat'); // true
```

事实上，上面的例子会被编译为：

```ts
var Days;
(function(Days) {
  Days[(Days['Sun'] = 0)] = 'Sun';
  Days[(Days['Mon'] = 1)] = 'Mon';
  Days[(Days['Tue'] = 2)] = 'Tue';
  Days[(Days['Wed'] = 3)] = 'Wed';
  Days[(Days['Thu'] = 4)] = 'Thu';
  Days[(Days['Fri'] = 5)] = 'Fri';
  Days[(Days['Sat'] = 6)] = 'Sat';
})(Days || (Days = {}));
```

如果未手动赋值的枚举项与手动赋值的重复了，TypeScript 是不会察觉到这一点的：

```ts
enum Days { Sun = 3, Mon = 1, Tue, Wed, Thu, Fri, Sat }

console.log(Days['Sun'] === 3); // true
console.log(Days['Wed'] === 3); // true
console.log(Days[3] === 'Sun'); // false
console.log(Days[3] === 'Wed'); // true
```



## 3.3 常数项和计算所得项

**常数项：**

当满足以下条件时，枚举成员被做当是常数：

* 它是枚举的第一个成员且没有初始化器，这种情况下它被赋予值`0`

* 它不带有初始化器且它之前的枚举成员是一个*数字*常量。 这种情况下，当前枚举成员的值为它上一个枚举成员的值加1。

* 枚举成员使用*常量枚举表达式*初始化。 常量枚举表达式是TypeScript表达式的子集，它可以在编译阶段求值。 当一个表达式满足下面条件之一时，它就是一个常量枚举表达式：

  1. 一个枚举表达式字面量（主要是字符串字面量或数字字面量）
  2. 一个对之前定义的常量枚举成员的引用（可以是在不同的枚举类型中定义的）
  3. 带括号的常量枚举表达式
  4. 一元运算符`+`, `-`, `~`其中之一应用在了常量枚举表达式
  5. 常量枚举表达式做为二元运算符`+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `>>>`, `&`, `|`, `^`的操作对象。

  若常量枚举表达式求值后为`NaN`或`Infinity`，则会在编译阶段报错。

**计算所得项**

所有其他情况的枚举成员被当作是需要计算得出的值

[枚举](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Enums.html)

## 3.4 常数枚举

常数枚举是使用 `const enum` 定义的枚举类型

```ts
const enum Directions { Up, Down, Left, Right }
```

常数枚举与普通枚举的区别是，**它会在编译阶段被删除**，并且不能包含计算成员。

假如包含了计算成员，则会在编译阶段报错

## 3.5 外部枚举

外部枚举是使用 `declare enum` 定义的枚举类型

```ts
declare enum Directions { Up, Down, Left, Right }
let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

`declare` 定义的类型只会用于**编译时检查**，编译结果中会删除。

上例的编译结果是：

```ts
var directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

外部枚举与声明语句一样，常出现在声明文件中。

同时使用 `declare` 和 `const` 也是可以的：

```ts
declare const enum Directions { Up, Down, Left, Right }

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

编译结果：

```
var directions = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];
```



# 4. 任意类型（any）

任意类型（Any）用来表示允许赋值为任意类型。

变量如果在声明的时候，未指定其类型，那么它会被识别为任意值类型：



# 5. never 类型

## 5.1 用法

`never` 类型表示那些永不存在的值的类型。

`never`类型表示的是那些永不存在的值的类型。 例如， `never`类型是那些总是会抛出异常或**根本就不会有返回值的函数表达式或箭头函数表达式**的返回值类型； 变量也可能是 `never`类型，当它们被永不为真的类型保护所约束时。

**`never`类型是任何类型的子类型，也可以赋值给任何类型**；然而，没有类型是`never`的子类型或可以赋值给`never`类型（除了`never`本身之外）。 即使 `any`也不可以赋值给`never`。

```ts
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}
```

## 5.2 与 `void` 的差异

声明为 void 类型的变量，只能赋予 undefined 和 null。因此一个函数如果返回值是void类型，返回值只能是null或undefined

never 类型表示永远不会有值的一种类型，从概念上就和void是不一样的。比如一个总是会抛出错误的函数，或者是一个从来不会有返回值的函数(函数的默认返回值undefined)。



# 6. 数组

## 6.1 数组成员类型

```ts
let fibonacci: number[] = [1, 1, 2, 3, 5];
```

`number` 类型的数组，不能包含其他类型

## 6.2 数组泛型

```ts
// 表示声明的数组必须是数字类型
let fibonacci: Array<number> = [1, 1, 2, 3, 5];

// 表示声明的数组可以是任意类型
let foo: Array<any>;

// 表示声明的数组元素必须是包含 name 与 age 的对象，并且 name 为字符串，age 为数字
let bar: Array<{name: string; age: number}>;

// 表示数组元素必须为 name 的对象爱哪个，age 可选
let baz: Array<{name: string; age?: number}>
```

## 6.3 类数组

比如 `arguments` ，类数组不是数组类型，不能用普通数组的方式来描述，而应该用接口

```ts
function sum() {
  let args: {
    [index: number]: number;
    length: number;
    calle: Funcition;
  } = arguments;
}
```

事实上常用的累数组都有自己的接口定义，如 `IArguments`、`NodeList`、`HTMLCollection` 等。

其中 `IArguments` 是 TypeScript 中定义好了的类型。

```ts
function add (){
  let arg: IArguments = arguments
}
```



# 7 函数的类型

## 7.1 函数声明

输入和输出都要考虑到

输入多余的（或者少于要求的）参数，是不被允许的

```ts
function sum(x: number, y: number): number {
  return x + y;
} // true

function sum(x: number, y: number): number {
  return x + y;
}
sum(1, 2, 3);
// index.ts(4,1): error TS2346: Supplied parameters do not match any signature of call target.
```

## 7.2 函数表达式

注意不要混淆了 TypeScript 中的 `=>` 和 ES6 中的 `=>`。

在 TypeScript 的类型定义中，`=>` 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。

```ts
let mySum: (x: number, y: number) => number = function(x: number, y: number): number {
  return x + y;
};
```

## 7.3 用接口定义函数的形状

```ts
interface SearchFunc {
  (source: string, subString: string): boolean;
}
let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
  return source.search(subString) !== -1;
};
```

## 7.3 可选参数

与接口中的可选属性类似，我们用 `?` 表示可选的参数：

```ts
function buildName(firstName: string, lastName?: string) {
  if (lastName) {
    return firstName + ' ' + lastName;
  } else {
    return firstName;
  }
}
```

**需要注意的是，可选参数必须接在必需参数后面。换句话说，可选参数后面不允许再出现必需参数了：**

```ts
function buildName(firstName?: string, lastName: string) {
  if (firstName) {
    return firstName + ' ' + lastName;
  } else {
    return lastName;
  }
}
// index.ts(1,40): error TS1016: A required parameter cannot follow an optional parameter
```

## 7.5 参数默认值

TypeScript 会将添加了默认值的参数识别为可选参数：

```ts
function buildName(firstName: string, lastName: string = 'Cat') {
  return firstName + ' ' + lastName;
}

let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
```

此时就不受「可选参数必须在必需参数后面」的限制了：

```ts
function buildName(firstName: string = 'Tom', lastName: string) {
  return firstName + ' ' + lastName;
}

let tomcat = buildName('Tom', 'Cat');
let cat = buildName(undefined, 'Cat');
```

## 7.6 剩余参数

在 ES6 中，可以使用 `...rest` 的方式获取函数中的剩余参数（`rest` 参数）：

```ts
function push(array, ...items) {
  items.forEach(function(item) {
    array.push(item);
  });
}
```

事实上，`items` 是一个数组。所以我们可以用数组的类型来定义它：

```ts
function push(array: any[], ...items: any[]) {
  items.forEach(function(item) {
    array.push(item);
  });
}
```

## 7.7 重载

重载允许一个函数接受不同数量或类型的参数时，作出不同的处理。

```ts
function reverse(x: number | string): number | string {
  if (typeof x === 'number') {
    return Number(
      x
        .toString()
        .split('')
        .reverse()
        .join('')
    );
  } else if (typeof x === 'string') {
    return x
      .split('')
      .reverse()
      .join('');
  }
}
```

然而这样有一个缺点，就是不能够精确的表达，输入为数字的时候，输出也应该为数字，输入为字符串的时候，输出也应该为字符串。

这时，我们可以使用重载定义多个 `reverse` 的函数类型：

```ts
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
  if (typeof x === 'number') {
    return Number(
      x
        .toString()
        .split('')
        .reverse()
        .join('')
    );
  } else if (typeof x === 'string') {
    return x
      .split('')
      .reverse()
      .join('');
  }
}
```



# 8 对象的类型/接口

## 8.1 基础用法

```ts
interface Person {
  name: string;
  age: number;
}
let tom: Person = {
  name: 'Tom',
  age: 25,
};
```

定义的对象的属性比接口多或者少都是不允许的。可以是用可选属性。

## 8.2 可选属性

```ts
interface Person {
  name: string;
  age?: number;
}
let tom: Person = {
  name: 'Tom',
};
```

这时仍然不允许添加接口未定义的属性。

## 8.3 任意属性

```ts
interface Person {
  name: string;
  age?: number;
  [propName: string]: any;
}
```

需要注意的是，一旦定义了任意属性，那么确定属性和可选属性的类型都必须是任意属性的类型的子集：

像下面这样是会报错的

```ts
interface Person {
    name: string;
    age?: number; // 类型“number”的属性“age”不能赋给“string”索引类型“string”。
    [prop:string]: string;
}
```

所以 `[prop:string]: string;` 应写成 ` [prop:string]: string | number` 或者 `any`

## 8.4 只读属性

对象中的一些字段只能在创建的时候被赋值，那么可以用 `readonly` 定义只读属性。

**注意**，只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候：



# 9. 类

## 9.1 public、private、protected

- public 公有属性
  - 可以在任何地方被访问到
- private 私有属性
  - 实例对象无法直接存取
  - 子类不允许访问
  - 修饰构造函数时，该类不允许被继承或实例化
- protected 保护属性
  - 允许子类访问
  - 修饰构造函数时，该类只允许继承

## 9.2 参数属性

修饰符和 `readonly` 还可以使用在构造函数参数中，等同于类中定义该属性同时给该属性赋值，使代码更简洁。

```ts
class Animal {
  // public name: string
  public constructor(public name) {
    // this.name = name;
  }
}
```

## 9.3 只读修饰符

只读属性关键字，只允许出现在**属性声明**或**索引签名**或**构造函数**中。

注意如果 `readonly` 和其他访问修饰符同时存在的话，需要写在其后面。

```ts
class Animal {
  // public readonly name;
  public constructor(public readonly name) {
    // this.name = name;
  }
}
```

## 9.4 抽象类

`abstract` 关键词

抽象类中的抽象方法必须由子类实现

```ts
abstract class Animal {
  public name;
  public constructor(name) {
    this.name = name;
  }
  public abstract sayHi();
}
class Cat extends Animal {
  public sayHi() { // 如果这里不定义，会报错
    console.log(`Meow, My name is ${this.name}`);
  }
}
let cat = new Cat('Tom');
```

需要注意的是，即使是抽象方法，TypeScript 的编译结果中，仍然会存在这个类

## 9.5  类的类型

给类加上 TypeScript 的类型很简单，与接口类似：

```ts
class Animal {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  sayHi(): string {
    return `My name is ${this.name}`;
  }
}
let a: Animal = new Animal('Jack');
console.log(a.sayHi()); // My name is Jack

```



# 10. 类型别名

类型别名用来给一个类型起个新名字。

```ts
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;

function getName(n: NameOrResolver): Name {
  if (typeof n === 'string') {
    return n;
  } else {
    return n();
  }
}
```

上例中，我们使用 `type` 创建类型别名。

类型别名常用于联合类型。



# 11. 声明合并

如果定义了两个相同名字的函数、接口或类，那么它们会合并成一个类型。

## 11.1 函数的合并

可以使用重载定义多个函数类型。

## 11.2 接口的合并

接口中的属性在合并时会简单的合并到一个接口中

注意，合并的属性的类型必须是唯一的

```ts
interface Alarm {
  price: number;
}
interface Alarm {
  weight: number;
}
```

相当于：

```ts
interface Alarm {
  price: number;
  weight: number;
}
```

```ts
interface Alarm {
  price: number;
}
interface Alarm {
  price: string; // 类型不一致，会报错
  weight: number;
}
```

**接口中方法的合并，与函数的合并一样：**

```ts
interface Alarm {
  price: number;
  alert(s: string): string;
}
interface Alarm {
  weight: number;
  alert(s: string, n: number): string;
}
```

相当于：

```ts
interface Alarm {
  price: number;
  weight: number;
  alert(s: string): string;
  alert(s: string, n: number): string;
}
```



[参考文档](https://tsejx.github.io/typescript-guidebook/syntax/basics/basic-types)

