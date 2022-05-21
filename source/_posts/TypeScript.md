---
title: TypeScript
date: 2019-07-26 17:18:47
tags: TypeScript
categories: 
  - ['Typescript']
cover: https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/typescript.png
sticky: 10
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
// 枚举成员的初始值会从 0 开始自增
enum Position {
  UP,
  DOWN,
  LEFT,
  RIGHT
}
console.log(Position.UP)		// 0
console.log(Position.DOWN)	// 1
console.log(Position.LEFT)	// 2
console.log(Position.RIGHT)	// 3
console.log(Position[0])	// 'UP'
console.log(Position[1])	// 'DOWN'
console.log(Position[2])	// 'LEFT'
console.log(Position[3])	// 'RIGHT'
```

也可以为枚举赋值

``` ts
// 字符串枚举
enum Position {
  UP = 'up',
  DOWN = 'down',
  LEFT = 'left',
  RIGHT = 'right'
}
console.log(Position.UP)		// 'up'
console.log(Position.DOWN)	// 'down'
console.log(Position.LEFT)	// 'left'
console.log(Position.RIGHT)	// 'right'
// 数字枚举
enum Num {
  ZERO = 0,
  ONE = 1,
  TWO = 2,
  THREE = 3,
}
console.log(Num.ZERO)		// 0
console.log(Num.ONE)		// 1
console.log(Num.TWO)		// 2
console.log(Num.THREE)	// 3
```

如果从中间开始赋值为字符串需要重新为之后的成员赋初始值

``` ts
enum Position {
  UP,
  DOWN = 'down',
  LEFT = 6,	// 必须指定初始值
  RIGHT
}
console.log(Position.UP)		// 0
console.log(Position.DOWN)	// 'down'
console.log(Position.LEFT)	// 6
console.log(Position.RIGHT)	// 7
```

计算成员

``` ts
enum Position {
  UP,
  DOWN = 'down'.length,	// 计算成员
  LEFT = 6,
  RIGHT
}
console.log(Position.UP)		// 0.xxx
console.log(Position.DOWN)	// 4
console.log(Position.LEFT)	// 6
console.log(Position.RIGHT)	// 7
```

常量枚举

``` ts
const enum Position {
  UP,
  DOWN,
  LEFT,
  RIGHT
}
// 常量枚举不能有计算成员
const enum Example {
  UP,
  DOWN = 'down'.length,	// error
}
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

### 循环引用自身

``` ts
interface Example {
  [key: string]: string | Example;
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

1. interface 使用 **extends** 继承，type 使用 **&** 联合类型继承
2. interface 可以通过重复声明来拓展，type 同名只能声明一次
3. interface 会创建新的类型，type 只是类型别名
4. Interface 可以循环引用自身，type 不可以



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

也可以是函数

``` ts
type F1 = (a: string, b: string) => void;
type F2 = (a: number, b: number) => void;
type F3 = (a: string, b: number) => void;

const example: F1 & F2 & F3 = (a: string | number, b: string | number) => {};
example(1, 1);
example('1', '1');
example('1', 1)
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

### 映射类型

```` ts
{ [P in K]: T }	// in 关键字用于遍历 K 类型中的所有类型
{ [P in K]?: T }	// ? 表示增加可选修饰符
{ [P in K]-?: T }	// -? 表示移除可选修饰符
{ readonly [P in K]: T }	// readonly 表示增加只读修饰符
{ -readonly [P in K]?: T }	// - readonly 表示移除只读修饰符

type T1 = { [P in 'a' | 'b']: string };	// type T1 = { a: string; b: string; }
type T2 = { [P in 'a' | 'b']: P };	// type T1 = { a: 'a'; b: 'b'; }
 
 // as, 使用 as 可以对映射类型中的键进行重新映射，NewKeyType 必须是 string | number | symbol （联合类型）的子类
// { [K in keyof T as NewKeyType]: T[K] }
type Person = {
    name: string;
    age: number;
}
 // type T3 = { jayname: string; jayage: string; }	// { string & K } 用来过滤 K 中非 string 类型的键
type T3 = { [K in keyof Person as `jay${string & K}` ]: string };	
````

### 索引类型

```ts
// 索引类型的 key 只能是 number string symble 和模版字面量类型
{ [key: keyType]: valueType }

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

### 条件类型

``` ts
// T extends U ? A : B，和三元表达式类似
type IsString<T> = T extends string ? true : false;
type T1 = IsString<number>	// type T1 = false
type T2 = IsString<string>	// type T1 = true
type T3 = IsString<'string'> // type T1 = true
type T4 = IsString<any> // type T1 = boolean，因同时返回 true false 结果为 boolean
type T5 = IsString<never> // type T1 = never，never被认为是空的联合类型
// never是所有类型的子类型
type A1 = never extends 'x' ? string : number; // string
```

分布式条件类型，在使用了 extends 关键字的条件类型中如被检查的类型（T）为裸类型（没有被数组、元组、Promise 包装过的类型，如：T[] [T] 等）被称为分布式条件类型

``` ts
// 在泛型类型中当被检查类型为联合类型，在运算过程中会被分解为多个分支
// X ｜ Y extends U ? A : B
// (X extends U ? A : B) | (Y extends U ? A : B)
type Example<T> = T extends string ? 'yes' : 'no';
type T1 = Example<number | string>; // type T1 = 'yes' | 'no'

// 被检查的类型 T 被 [] 包装过就不属于分布式条件类型
type Example2<T> = T[] extends number[] ? 'yes' : 'no';
type T2 = Example2<number | string>;  // type T2 = 'no'
```



### 泛型

``` ts
type Add<T> = (a: T, b: T) => T
const add: Add<number> = (a, b) => a + b	// 在使用时才确定泛型的类型
```



## 工具类型

### ReturnType<T>

返回 T 的返回值类型，T 为 (...args: any) => any

``` ts
type Result = ReturnType<() => number>	// type Result = number

function fn (str: string) {
  return str.toString()
}
type Result2 = ReturnType<typeof fn>	// type Result2 = string
```

### Record<K, T>

定义对象的 key value 类型

``` ts
const example: Record<string, number> = { jay: 1 };
const example2: Record<number, string> = { 0: '' };
type productType = 'food' | 'commodity' | 'clothe';
const example3: Record<productType, productType> = { 
  food: 'food',
  commodity: 'commodity',
  clothe: 'clothe'
};
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
interface Person {
  name: string;
  age?: number;
}
type Result = Required<Person>;	// type Result = { name: string; age: number; }
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

// 修改接口属性类型
interface Person {
  name: string;
  age: number;
  skin: string;
}
interface Example extends Omit<Person, 'age'> {
  age: string
}
const jay: Example = { name: 'jay', age: '18', skin: 'yellow'}
```

### Pick<T, K>

返回 T 中对应的 K 属性

``` ts
interface Person {
  name: string;
  age: number;
}
type Result = Pick<Person, 'name'>	// type Result = { name: string; }
```

### Exclude<T, U>

返回 T 中不包含 U 的

``` ts
type Result = Exclude<'a' | 'b' | 'c', 'c'>	// type Result = "a" | "b"
```

### Extract<T, U>

返回 T 和 U 的交集

``` ts
type Result = Extract<'a' | 'b' | 'c', 'c'>	// type Result = "c"
```

### NonNullable<T>

返回 T 并去除 undefined

``` ts
type Result = NonNullable<string | number | boolean | undefined>	// type Result = string | number | boolean
```



### infer

``` ts
// infer 只能在条件类型(T extends X ? Y : Z)的 extends 子句中才能使用
// infer 声明的变量只能在条件类型的 true 分支中使用

type Example<T> = T extends (infer U)[] ? U : T;	// U 用于存放推断的类型
// Example 传入 string[]，T 为 string[]
// T extends (infer U)[] 等于	string[] extends (infer U)[]，类型匹配 string 匹配 (infer U)，[] 匹配 []，因此 U 为 string。
type T = Example<string[]>	// type T = string

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
const v: Values = '';
const v2: Values = 123;

enum Gender {
  Male,
  Female
}
type G = keyof typeof Gender;	// type G = "Male" | "Female"
```

### extends

泛型约束

``` ts
type Gender = 'male' | 'female';
type Person<T extends Gender> = T;	// 约束 T 的类型必须为 Gender 的子类型
// Person 的泛型必须为 Gender
const example: Person<'male'> = 'male';
// ExampleType 的 T 如果存在于 Gender 就返回 T，否则返回 nerver
type ExampleType<T> = T extends Gender ? T : never
type ExampleType2 = ExampleType<'male'>	// type ExampleType2 = "male"
type ExampleType3 = ExampleType<null>	// type ExampleType2 = nerver
type ExampleType4 = ExampleType<boolean>	// type ExampleType2 = nerver
type ExampleType5 = ExampleType<'female' | 'male' | boolean | null>	// type ExampleType2 = "male" | 'female'

interface HasLength {
  length: number
}
// arg 必须包含 length
function example2<T extends HasLength> (arg: T): T {
  console.log(arg.length)
  return arg
}

// 包含关系，T 包含 U 为 true
type Example<T, U> = T extends U ? true : false;

interface A { a: string; }
interface B { a: string; b: string }
type T1 = Example<A, B>;	// false
type T2 = Example<A, A>;	// true
type T3 = Example<B, A>;	// true

// 被检查的类型不为泛型，仅条件判断
type T4 = 'a' extends 'a' ? 'yes' : 'no'; // 'yes'
type T5 = 'a' | 'b' extends 'a' ? 'yes' : 'no'; // 'no'
// 被检查的类型为泛型，为分布式条件类型
type Example2<T> = T extends 'a' ? 'yes' : 'no';
type T6 = Example2<'a' | 'b'> // type T6 = 'yes' | 'no'
```

### in

遍历类型

``` ts
type T1 = { [P in 'a' | 'b' ｜ 'c']: string }	// type T1 = { a: string; b: string; c: string; }
```

### typeof 

可以用来获取变量的类型

``` ts
const person = { name: 'jay', age: 18 };

type Person = typeof person;	// type Person = { name: string, age: number }
```



### as const

``` ts
// const animal = ['dog', 'cat', 'pig', 'bird'] 是一个变量数组， const animal: string[]，此时还可以修改数组。
// 当加了 as const 后就变成一个只读常量数组（元组），此时 animal 不能修改
const animal = ['dog', 'cat', 'pig', 'bird'] as const;	// const animal: readonly ["dog", "cat", "pig", "bird"]
type AnimalType = typeof animal[number];	// type AnimalType = "dog" | "cat" | "pig" | "bird"

// 没有 as const
const productType = ['food', 'commodity', 'clothe'];
console.log(productType.includes(''))	// 此处不会有提示和报错
// 添加 as const
const productType = ['food', 'commodity', 'clothe'] as const;
console.log(productType.includes(''))	// error: Argument of type '""' is not assignable to parameter of type '"food" | "commodity" | "clothe"'
```



## 其他

### 在 window 上添加变量声明

``` ts
declare global {
  interface Window {
    example: srting;
  }
}
```



