---
title: TypeScript
date: 2019-07-26 17:18:47
tags: TypeScript
categories: 
  - ['Typescript']
cover: https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/typescript.png
---

# TypeScript

TypeScript 是 JavaScript 的超集

## TypeScript 支持的类型

``` ts
const undefine: undefined = undefined
const n: null = null
const bool: boolean = true
const number: number = 123
const string: string = 'string'
const big: bigint = 123n
const symbol: symbol = Symbol()
const obj: object = {}

// class 可以当作类型使用
class T {}
const a: T = new T()

```

### 数组

``` ts
const arr: number[] = [1, 2, 3]
const arr2: Array<number | string> = [1, '']	// 泛型
```

### 枚举

``` ts
```



### 函数

``` ts
// 函数
const fn: (a: number, b: number) => number = (a, b) => a + b
const fn2 = (a: number, b: number) => a + b
type Fn = (a: number, b: number) => number
const fn3:Fn = (a, b) => a + b
interface Fn2 {
  (a: number, b: number): number,
  attr: string
}
const fn4 =  (a: number, b: number) => number
fn4.attr = ''
```

### 函数重载

``` ts
function add(a: number, b:number): number
function add(a: string, b:string): string
function add(a: number | string, b: number | string): number | string {}	// 这里的类型需包含之前定义的类型，也可以是顶级类型 any unknown

add(1, 2)
add('hello', 'world')
add('hello', 1)	// error
```



### any

`any` 为顶级类型（top type），可以是任何值

``` ts
let value: any = undefined
value = null
value = ''
value = []
```

### Unknown

和 `any` 一样是顶级类型，所有的值都可以赋给 `unknown`。

``` ts
let value: unknow = undefined
value = null
value = ''
value = []
```

但 `unknown` 只能被赋值为 `unknow` 和 `any`

``` ts
let value: unknow
let value2: unknow = value
let value3: any = vlaue
let value4: boolean = value // error
let value5: object = value // error
```

`unknown` 不可以直接被操作

``` ts
let a: unknown 
type T = { name: string }
a.name	// 类型“unknown”上不存在属性“name”。ts(2339)
// 可以使用断言或类型收窄
(a as T).name
```

### never

never 和 any unknown 相反为底部类型（bottom type），不存在的值

``` ts
// never, 不能将任何值赋给 never
let a: never = 1	// error
let a: never = ''	// error
let a: never = []	// error
let a: never = {}	// error
type T = number & string	// 没有任何值属于 number 和 string 类型，所以 type T = never
```

### void

没有类型

``` ts
// 这个函数没有返回值
function fn (): void {}
```

### 元组

固定长度数组

``` ts
let arr: [number, string, boolean] = [1, '', true]
```



## 断言

### 类型断言

断言值为某种类型

``` ts
(<string>someValue).length;
(someValue as string).length;
// 当你在TypeScript里使用JSX时，只有 as语法断言是被允许的
```

### 非空断言

`value!` 断言当前值不为 `null` 和 `undefined`

``` ts
let value: number | null | undefined
let n: number = value	// error
let n2: number = value! // value 不为 null 和 undefined 只可能为 number
obj.fn!()	// 断言函数不为 undefined
// 确定赋值断言
let s!: string
let str: string
console.log(s)
console.log(str)	// error
```



## 接口

接口用来描述一个对象拥有哪些属性

``` ts
interface Person {
  name: string;
  age: number;
  say(content: string): string;
}
// 可以是函数
interface Fn {
  (a: number, b: number): number;
}
const fn = (a, b): Fn => {
  return a + b
}

// 函数的属性也可以是函数
interface Fn {
  (a: number, b: number): number;
  subtraction(a: number, b: number): number;
}
const fn = (): Fn => {
  const n: any = function (a: number, b: number): number {
    return a + b
  }
  n.subtraction = function (a: number, b: number): number {
    return a - b
  }
  return n
}
const add: Fn = fn()
add(1, 2)

// interface 可以是数组
interface Arr {
  [index: number]: string;
}

// 多个同名 interface 会合并为一个
interface Human {
  name: string;
}
interface Human {
  skin: string;
}

const person: Human = {
  name: 'jay',
  skin: 'yellow'
}
```

### 只读属性

``` ts
interface Person {
  readonly name: string;
  age: number;
}
```

### 可选属性

``` ts
interface Person {
  name?: string;
  age: number;
}
```

### 接口继承

``` ts
interface Animal {
  age: number;
}
interface Human extends Animal {
  name: string;
}

type Plant = { color: string; }
interface Grass extends Plant {
  category: string;
}
```

### implements

从 class 或 interface 实现所有的属性和方法，同时可以重写属性和方法

``` ts
interface Action {
  run(): void;
}
interface Language {
  say(): void;
}

class Car implements Action {
  run () {}
}
// 一个类可以 implements 多个 interface 或 class
class Human implements Action, Language {
  run () {}
  say () {}
}
```

### implements 和 extends 的区别

``` ts
// implements 只能用于 class，不能用于 interface
interface Action {
  run(): void;
}
class Vehicle {}
class Car implements Vehicle, Action {
  run () {}
}
// interface 不能 implements interface 或 class
interface Person implements Action {}	// error: Interface declaration cannot have 'implements' clause
interface Person implements Vehicle {}	// error: Interface declaration cannot have 'implements' clause

// interface 可以 extends interface 或 class
interface Person2 extends Action {}
interface Person2 extends Vehicle {}

// class 只能 extends class
class Person3 extends Action {}	// error: Cannot extend an interface 'Action'. Did you mean 'implements'
class Person3 extends Vehicle {}
```

### 传 interface 未定义的属性

- 类型断言

  ``` ts
  interface Person {
    name: string;
    age: number;
  }
  const jay: Person = {
    name: 'jay',
    age: 18,
    skin: 'yellow'
  } as Person
  ```

- 索引签名

  ``` ts
  interface Person {
    name: string;
    age: number;
    [key: string]: any;
  }
  ```

### interface 和 type 区别







## 高级类型

### 交叉类型

多个类型合并为一个类型

``` ts
interface X {
  a: string;
  b: number;
}

type Y = {
  b: number;
  c: string;
}

const z: X & Y = {
  a: 'hi',
  b: 233,
  c: 'hello'
}
```

多类型同属性合并，类型为 `never`

``` ts
interface X {
	a: string;
	b: string;
}

interface Y {
  b: number;
  c: number;
}

const z: X & Y = {
  a: 'string',
  b: 'string',	// error, b 的类型为 never
  c: 123
}
```



### 联合类型

值可以是定义的某一种类型

``` ts
interface x {
  a: string;
  b: number;
}

interface y {
  b: number;
  c: string;
}

const z1: x | y = {
  a: 'hi',
  b: 666
}
const z2: x | y = {
  b: 233,
  c: 'hello'
}
```

### 类型别名
为类型起一个新的名字

```ts
type str = string;
const name:str = 'jay';
```

### 字面量类型

```ts
type gender = 'male' | 'female';
interface human {
  gender: 'male' | 'female';
}
```

### this 类型

指定 this 的类型

```js
function sayHi (this: number, name: string) {
  console.log(`hi, ${name}`);
}
sayHi('jay') // error
sayHi.call(666, 'jay')  // TS 不会检查 call 的类型

function sayHi (this: number | void, name: string) {
  console.log(`hi, ${name}`);
}
sayHi('jay')
```

### 索引类型

```ts
interface CreateObjectOptions {
  [K: string]: any;
}
function createObject (options: CreateObjectOptions) {

}
createObject({
  obj: {},
  name: 'jay',
  xxx: 123
})

function pluck<T, K extends keyof T>(object: T, keys: Array<K>): T[k][] {
  // T 等于  {name: String, age: Number}
  // keyof T 等于  'name' | 'age'
  // K extends keyof T 等于  'name' | 'age'
  // extends keyof 表示有其中之一的 key，in keyof 表示全部的 key 都要有
  return keys.map(key => object[key])
}
pluck({ name: 'jay', age: 18}, ['name', 'age'])
```

### Readonly<T>

返回 T 并将所有属性的类型变为只读

```ts
interface Person {
  name: string;
  age: number;
}
const p1: Person = {
  name: 'jay',
  age: 18
}
p1.age = 20;
const p2: Readonly<Person> = {
  name: 'jay',
  age: 18
}
p2.age = 22 // error
```

### ReadonlyArray<T>

只读的数组

```ts
let arr: ReadonlyArray<number> = [1, 2, 3]
arr[0] = 6 // error
arr.push(6)	// error
```

### Required<T>

返回 T 并将类型所有变为 required

``` ts

```

### Partial<T>

返回 T 并将类型所有变为可选

```ts
interface Person {
  name: string;
  age: number;
}
type Pserson2 = Partial<Person>;
// Person2 为 Person3 的简写
interface Person3 {
  name?: string;
  age?: number;
}
```

### Omit<T, K>

返回 T 并去除 K 属性

``` ts
interface Person {
  name: string;
  age: number;
  useTools: () => void;
}
type Cat = Omit<Person, 'useTools'>
const cat: Cat = {
  name: 'tom',
  age: 6
}
// 去除多个属性
type Dog = Omit<Person, 'useTools' | 'name'>
const dog: Dog = {
  age: 6
}
```

### Pick<T, K>

返回 T 中对应的 K 属性

``` ts

```

### Exclude<T, U>

``` ts
```

### Extract<T, U>

``` ts
```

### NonNullavle<T>

``` ts
```

### ReturnType<T>

``` ts
```

### infer

``` ts

```

### keyof

获取类型的所有 key

``` ts 
interface Person {
  name: string;
  age: number;
  useTools: () => void;
}
type Keys = keyof Person;	// type keys = 'name' | 'age'
type Values = keyof { [key: string]: Person };  // type Values = string | number

const k: Keys = 'name';
const k2: Keys = 'age';

const v: Values = ''
const v2: Values = 123
```

### extends

``` ts
```

### in

包含某属性

``` ts
interface Person {
    name: string;
    age: number;
}
const jay: Person = {
    name: 'jay',
    age: 18
}
console.log('age' in jay)	// true
console.log('color' in jay)	// false
```

### typeof 

可以用来获取变量的类型

``` ts
const person = { name: 'jay', age: 18 };

type Person = typeof person;	// type Person = { name: string, age: number }
```



### 可识别类型

1.有一个共有的字段
2.共有字段是可穷举的

```ts
type Props = {
  status: false;
  name: string;
  age: number;
} | {
  status: true;
  name: string;
}

const example: Props = {
  status: false,
  name: 'jay',
  age: 18
}
const example2: Props = {
  status: true,
  name: 'jay'
}
function fn (params: Props) {
  if (params.status === true) {
    console.log(params.name);
  } else {
    console.log(params.age);
  }
}
```

### 泛型

``` ts
type Add<T> = (a: T, b: T) => T
const add: Add<number> = (a, b) => a + b	// 在使用时才确定泛型的类型
```

