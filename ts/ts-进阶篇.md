# 1. 泛型

在定义函数，接口或类的时候，不预先指定具体的类型，而是使用的时候再指定类型。

## 1.1 基础用法

```ts
function createArray<T>(length: number, value: T): Array<T> {
  let result: T[] = [];
  for (let i = 0; i < length; i++) {
    result[i] = value;
  }
  return result;
}
// 当然，也可以不手动指定，而让类型推论自动推算出来：
createArray<string>(3, 'x'); // ['x', 'x', 'x']

```

## 1.2 多个参数类型

```ts
function swap<T, U>(tuple: [T, U]): [U, T] {
  return [tuple[1], tuple[0]];
}
swap([7, 'seven']); // ['seven', 7]
```

## 1.3 泛型约束

```ts
function loggingIdentity<T>(arg: T): T {
  console.log(arg.length);
  return arg;
}
// index.ts(2,19): error TS2339: Property 'length' does not exist on type 'T'.
```

```ts
interface Lengthwise {
  length: number;
}
function loggingIdentity<T extends Lengthwise>(arg: T): T {
  console.log(arg.length);
  return arg;
}
```

## 1.4 泛型接口

```ts
interface CreateArrayFunc<T> {
  (length: number, value: T): Array<T>;
}

let createArray: CreateArrayFunc<any>;
createArray = function<T>(length: number, value: T): Array<T> {
  let result: T[] = [];
  for (let i = 0; i < length; i++) {
    result[i] = value;
  }
  return result;
};

createArray(3, 'x'); // ['x', 'x', 'x']
```

## 1.5 泛型参数的默认类型

```ts
function createArray<T = string>(length: number, value: T): Array<T> {
  let result: T[] = [];
  for (let i = 0; i < length; i++) {
    result[i] = value;
  }
  return result;
}
```



# 2. 类型断言

类型断言（Type Assertion）可以用来手动指定一个值的类型。

## 2.1 语法

```ts
<类型>值;

// 或

值 as 类型;
```

> 在 tsx 语法（React 的 JSX 语法的 TS 版）中必须用后一种。



## 2.2 场景

```ts
function getLength(something: string | number): number {
  if (something.length) {
    return something.length;
  } else {
    return something.toString().length;
  }
}

// index.ts(2,19): error TS2339: Property 'length' does not exist on type 'string | number'.
//   Property 'length' does not exist on type 'number'.
// index.ts(3,26): error TS2339: Property 'length' does not exist on type 'string | number'.
//   Property 'length' does not exist on type 'number'.
```

此时可以使用类型断言，将 `something` 断言成 `string`：

```ts
function getLength(something: string | number): number {
  if ((<string>something).length) {
    return (<string>something).length;
  } else {
    return something.toString().length;
  }
}
```

## 2.3 注意

类型断言不是类型转换，断言成一个联合类型中不存在的类型是不允许的。

```ts
function toBoolean(something: string | number): boolean {
  return <boolean>something;
}
// index.ts(2,10): error TS2352: Type 'string | number' cannot be converted to type 'boolean'.
//   Type 'number' is not comparable to type 'boolean'.
```



# 3. 类型推论

```ts
let num = 'seven';
num = 7;

// 等价于

let num: string = 'seven';
num = 7;
```

所以，第一种情况下会报错，因为TypeScript 会在没有明确的指定类型的时候推测出一个类型，这就是类型推论。

如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成 `any` 类型而完全不被类型检查：

```ts
let num;
num = 'seven';
num = 7;
```



# 4. 类型保护

类型断言来进行类型判断，但是有个问题就是每个分支都需要进行类型判断。

TypeScript 的类型保护机制：一次判断，整个作用域/所有分支有效。

类型保护（Type Guards）就是一些表达式，会在运行时检查以确保在某个作用域内的类型。

## 4.1 typeof 类型保护

```ts
function fn(param: string | number) {
  if (typeof param === 'string') {
    console.log('string');
  }
  if (typeof param === 'number') {
    console.log('number');
  }
}
```

## 4.2 instanceof 类型保护

类似 typeof

## 4.3 自定义类型保护

 `parameterName is Type`

```ts
let isNumber: (value: any) => value is number;

let foo: string | number;
if (isNumber(foo)) {
  // 缩窄到 number
  foo.toFixed(2);
} else {
  // 通过类型保护，编译器知道不是 number 就是 string
  foo.toUpperCase();
}
```



# 5. 类型兼容性

类型兼容性用于确定一个类型是否能赋值给其他类型。

如 `string` 类型与 `number` 类型不兼容

## 5.1 安全性





# 6. 交叉类型

```ts
type A = { a: string }

type B = { b: number }

type C = A & B

const obj1:A = { a: 'str' }

const obj2:B = { b: 12 }

const obj3:C = { a: '2', b: 1 }

```



# 7. 联合类型

```ts
interface A {
  name: string
}

interface B {
  age: number
}

type C = A | B

const c: C = {
  name: '',
  age: 0
}
```

```ts
type A = string | number

const a: A = 'name'

const b: A = 122
```

```ts
function generate(p: string[] | string){
  if(typeof p === 'string' ) {
    p.split('')
  }else {
    p.forEach(item => item)
  }
}
```



# 8. 索引类型 - keyof

```ts
type TestType = keyof any // type TestType = string | number | symbol
```

```ts
function pluck<T, K extends keyof T>(o: T, names: K[]): T[K][] {
  return names.map(name => o[name])
}

interface Person {
  name: string
  age: number
  sex: boolean
}

const p1: Person = {
  name: 'lison',
  age: 12,
  sex: true
}

console.log(pluck(p1, ['name', 'age']));
```



# 9. 映射类型

`Readonly` `Partial` `Pick` `Record`

```ts
interface Person {
  name: string
  age: number
  sex: boolean
}

type ReadonlyType<T> = {
  readonly [K in keyof T]: T[K]
}

type ReadonlyPerson = ReadonlyType<Person> 
/**
type ReadonlyPerson = {
    readonly name: string;
    readonly age: number;
    readonly sex: boolean;
}
**/
```

```ts
interface Person {
  name: string
  age: number
  sex: boolean
}

type ReadonlyType<T> = {
  readonly [K in keyof T]: T[K]
}

type PartialType<T> = {
  [K in keyof T]?: T[K]
}

type PartialPerson = PartialType<Person>
/**
	type PartialPerson = {
    name?: string | undefined;
    age?: number | undefined;
    sex?: boolean | undefined;
}
**/
```

```ts
interface Person {
  name: string
  age: number
  sex: boolean
  address: string,
}

type PickType<T, K extends keyof T> = {
  [P in K]: T[P]
}

type Tuple = 'name' | 'age'

type PlainPerson = PickType<Person, Tuple>
/**
  type PlainPerson = {
      name: string;
      age: number;
  }
**/
```

```ts
type TestRecord = Record<'prop1' | 'prop2' | 'prop3', string>
/**
	type TestRecord = {
    prop1: string;
    prop2: string;
    prop3: string;
	}
**/
```



# 10. 条件类型

# 11. This 类型

# 12. 字面量类型

```ts
type A = 'hello'

let a: A = 'hi' // error: Type '"hi"' is not assignable to type '"hello"'

let b: A = 'hello'  // true
```

它们本身并不是很实用，但是可以在一个联合类型中组合创建一个强大的（实用的）抽象：

```ts
type OneToFive = 1 | 2 | 3 | 4 | 5;
type Bools = true | false;
```

```ts
type Direction = 'north' | 'east' | 'south' | 'west'

function getDirection(d: Direction): string{
  return d
}
getDirection('east') // true
getDirection('East') // false Argument of type '"East"' is not assignable to parameter of type 'Direction'
```

**请看此例：**

```ts
function fn(foo: 'foo'){
  return foo
}

const test = {
  a: 'foo'
}

fn(test.a) // error: Argument of type 'string' is not assignable to parameter of type '"foo"'.
```

这是由于 `someProp` 被推断为 `string`，我们可以采用一个简单的类型断言来告诉 ts：

```ts
function fn(foo: 'foo'){
  return foo
}

const test = {
  a: 'foo'
}

fn(test.a as 'foo') // error: Argument of type 'string' is not assignable to parameter of type '"foo"'.
```

# 13. 可辨识联合

```ts
interface Circle {
  kind: 'circle'
  radius: number
}

interface Rect {
  kind: 'rect'
  width: number,
  height: number
}

interface Square {
  kind: 'square'
  size: number
}

type Shape = Circle | Rect | Square

function getArea(s: Shape): number{
  if(s.kind == 'circle') {
    return s.radius ** 2 * Math.PI
  } else if(s.kind == 'rect') {
    return s.height * s.width
  } else if(s.kind == 'square') {
    return s.size * s.size
  }else{
    const e: never = s
    return e
  }
}
```



# 14. 命名空间和模块 

# 15. 实用工具类型