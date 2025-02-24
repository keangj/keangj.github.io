---
title: Dart
date: 2024-12-10 15:38:43
updated: 2024-12-20 10:22:52
tags: [Dart, Flutter]
categories: 
  - ['Dart']
  - ['Flutter']
cover: https://dart.cn/assets/img/logo/dart-logo-for-shares.png

---

# Dart

## 变量 var
### `var`
如果变量没有设置初始值，Dart 会将其推断为动态类型，允许赋值为任意类型的值。
``` dart
var a; // 没设置初始值时，a 为 null
print(a); // null
a = 'str';
a = 666;
a = true;
a = {};
a = [];
print(a); // []
```
如果设置了初始值，只能设置和初始值同样的类型
``` dart
var a = 'jay';
a = 'hi';
a = 888;  // Error: A value of type 'int' can't be assigned to a variable of type 'String'.
```
### `Object`
动态任意类型
编译阶段检查类型
``` dart
Object a = 'jay';
a = 123;
a = {};
a.p(); // Error
```
### `dynamic`
动态任意类型
编译阶段不检查检查类型
``` dart
dynamic a = 'jay';
a = 123;
a = [];
a.p();
```
## 常量 const
### `const`
``` dart
// 声明类型可以省略
const String a = 'jay';
const b = 'jay';

// 定义时必须赋初始值
const a = 123;
const b; // Error

// 初始化后不能再赋值
const a = 123;
a = 456; // Error

// 初始值需要确定的值
const b = DateTime.now();  // Error:Const variables must be initialized with a constant value.

// 值不可以被修改
const list = [1, 2, 3];
list[1] = 4; // Error

// 相同内容引用同一个内存空间
const a1 = [11 , 22];
const a2 = [11 , 22];
print(identical(a1, a2)); // true, identical 比较两个引用的是否是同一个对象判断是否相等

```
### `final`
``` dart
// 声明类型可以省略
final String a = 'jay';
final b = 'jay';

// 必须赋值后才能使用
final a;
print(a); // Error
a = 666;
print(a);

// 初始化后不能再赋值
final b = 'jay';
b = 'hi'; // Error

// 初始值不需要确定的值
final a = DateTime.now();

// 值可以被修改
const list = [1, 2, 3];
list[1] = 4;

// 相同内容不引用同一个内存空间
final a1 = [11 , 22];
final a2 = [11 , 22];
print(identical(a1, a2)); // false
```
## 数值 num
### 声明
``` dart
int a = 1;  // 整数值，其取值通常位于 -2^53 和 2^53 之间
double b = 0.1;  // 64-bit (双精度) 浮点数，符合 IEEE 754 标准
num c = 123;  // int 和 double 的父类
```
### 数值的转换
``` dart
// string => int
int a = int.parse("123");
// string => double
double c = double.parse("1.1");
// int => string
String d = 123.toString();
// double => string
String e = 1.1.toString();
// double => int
int f = 1.1.toInt();
```
## 布尔值 bool
``` dart
bool a = true;
```
## 字符串 String
### 声明
``` dart
// 单、双引号都可以
String str1 = 'jay,\'single\' "double"';
String str2 = "jay,'single' \"double\"";
```
### 模版字符串
``` dart
var a = 'jay';
String b = 'hi, ${a}';  // ${} 的内容简单时，{} 可以省略
print(b);  // hi, jay
```
### 字符串拼接
``` dart
var a = 'hi' ',' ' jay';
var b = 'hi' + ',' + ' jay';
var c = 'hi'
    ','
	' jay';
print(a);  // hi, jay
print(b);  // hi, jay
print(c);  // hi, jay

// 多行，单、双引号都可以
var str = '''
	hi\n,
	jay
'''；
print(str);
// hi
// ,
// jay

// 前面加 r，取消转译
var str2 = r'''
	hi\n,
	jay
''';
print(str2);
// hi\n,
// jay
```
### 字符串方法
``` dart
var a = 'hi, jay';
print(a.contains('j'));  // true
print(a.startsWith('hi'));  // true
print(a.endsWith('jay'));  // true
print(a.indexOf('jay'));  // 4

print(a.substring(1,5));  // i, j
var b = a.split(', ');
print(b);  // [hi, jay]

print(a.toLowerCase());  // hi, jay
print(a.toUpperCase());  // HI, JAY

print(' HI, JAY '.trim()); // HI, JAY
print(''.isEmpty);  // true
print(''.isNotEmpty);  // false

print(a.replaceAll('jay', 'tom'));  // hi, tom

// 拼接字符串
StringBuffer()
..write('hi')
..write(',')
..write('jay. ')
..writeAll(['hi', ',', 'tom'])  // hi,jay.hi,tom
```
## 日期时间 DateTime
### 创建时间
``` dart
// 当前时间
var now = new DateTime.now();
// 指定时间，接受参数年月日时分秒
var datetime = new DateTime(2000, 1, 1, 1, 10);
// UTC 时间
var utc = DateTime.utc(2000, 1, 1, 1, 10);
```
### 解析时间
``` dart
var date = DateTime.parse('2000-01-01 06:30:00Z');
var date2 = DateTime.parse('2000-01-01 06:30:00+0800');
print(date);  // 2000-01-01 06:30:00.000Z
print(date2);  // 1999-12-31 22:30:00.000Z
```
### 计算时间
``` dart
var date = DateTime.now().add(Duration(days: 1));
```
### 比较时间
``` dart
var now = DateTime.now();
var date = now.add(Duration(days: 1));
print(now.isAfter(date));  // false
print(now.isBefore(date));  // true

// 计算时间差
print(now.difference(date));  // -24:00:00.000000
```
### 时间戳
``` dart
var now = DateTime.now();
print(now.millisecondsSinceEpoch);  // 毫秒
print(now.microsecondsSinceEpoch);  // 微秒
```
## 列表 List
### 定义列表
``` dart
var list = [1, 2, 3];

List<int> list2 = [];
list2
	..add(1)
	..add(2)
	..add(3);

// 定长列表
var list = List<int>.filled(3, 0);
list[3] = 666;  // Error
```
### 生成数据
``` dart
var list = List.generate(3, (i) => i + 1);  // [1, 2, 3]
```
### 属性
``` dart
var list = [1, 2, 3];
print(list.first);  // 1，第一个对象
print(list.last);  // 3，最后一个对象
print(list.length);  // 3，长度
print(list.isEmpty);  // false，是否为空
print(list.isNotEmpty);  // true，是否不为空
print(list.reversed);  // (3, 2, 1)，反转
```
### 方法

| 名称         | 说明     |
| ---------- | ------ |
| add        | 添加     |
| addAll     | 添加多个   |
| insert     | 插入     |
| insertAll  | 插入多个   |
| indexOf    | 查询     |
| indexWhere | 按条件查询  |
| remove     | 删除     |
| removeAt   | 按位置删除  |
| fillRange  | 按区间填充  |
| getRange   | 按区间获取  |
| shuffle    | 随机变换顺序 |
| sort       | 排序     |
| sublist    | 创建子列表  |

``` dart
List<int> list = [];

// 添加
list
	..add(1)  // [1]
	..addAll([2, 3])  // [1, 2, 3]
	..insert(0, 0)  // [0, 1, 2, 3]
	..insertAll(4, [4, 5]);  // [0, 1, 2, 3, 4, 5]

// 查询
print(list.indexOf(2));  // 2
print(list.indexWhere((i) => i > 2));  // 3
print(list.indexWhere((i) {
	return i > 2;
}));  // 3

// 删除
list.remove(4);
print(list);  // [0, 1, 2, 3, 5]
list.removeAt(0);
print(list);  // [1, 2, 3, 5]

// 填充范围
list.fillRange(1, 3, 66);
print(list);  // [1, 66, 66, 5]
// 获取范围
print(list.getRange(1, 3));  // (66, 66)

// 洗牌，乱序
list.shuffle();
print(list);  // [66, 5, 1, 66]

// 排序
list.sort();
print(list);  // [1, 5, 66, 66]
list.sort();
print(list);  // [1, 5, 66, 66]
list.sort((a, b) => b - a);
print(list);  // [66, 66, 5, 1]

// 时间排序
List<DateTime> dtList = [];
dtList.addAll([
  DateTime.now(),
  DateTime.now().add(new Duration(days: -12)),
  DateTime.now().add(new Duration(days: -2))
  ]);
print(dtList); // [2024-11-12 23:01:47.396, 2024-10-31 23:01:47.396, 2024-11-10 23:01:47.396]

dtList.sort((a, b) => a.compareTo(b));
print(dtList); // [2024-10-31 23:01:47.396, 2024-11-10 23:01:47.396, 2024-11-12 23:01:47.396]

// 创建子列表
var l = [1, 2, 3, 4, 5, 6];
var sl = l.sublist(1, 3);
print(sl);  // [2, 3]

// 连接列表
var l1 = [1, 2, 3];
var l2 = [4, 5, 6];
print(l1 + l2);  // [1, 2, 3, 4, 5, 6]
```
## 集合 Map

### 声明
``` dart
var a = { 'key': 'value' };
```
### 弱类型
``` dart
var a = {};
a['name'] = 'jay';
a['age'] = 66;
a[1] = 123;
print(a); // {name: jay, age: 66, 1: 123}
```
### 强类型
``` dart
var a = Map<String, String>();
a['name'] = 'jay';
a['age'] = 66; // Error, int 类型无法赋值给 String 的变量
a[1] = 123; // Error
print(a);
```
### 属性
``` dart
var a = { 'name': 'jay' };
print(a.isEmpty); // false
print(a.isNotEmpty); // false
print(a.keys); // (name)
print(a.values); // (jay)
print(a.length); // 1
print(a.entries); // (MapEntry(name: jay))
```
### 方法

| 名称            | 说明         |
| ------------- | ---------- |
| addAll        | 添加         |
| addEntries    | 从入口添加      |
| containsKey   | 按 key 查询   |
| containsValue | 按 value 查询 |
| clear         | 清空         |
| remove        | 删除         |
| removeWhere   | 按条件删除      |
| update        | 更新某个       |
| updateAll     | 按条件更新      |

``` dart
var a = {};
var b = { 'key': 'value' };
// 添加数据
a.addAll({ 'name': 'jay', 'age': 66 }); // {name: jay, age: 66}
// 添加另一个 map
a.addEntries(b.entries); // {name: jay, age: 66, key: value}
// 检查某个 key 是否存在
print(a.containsKey('name')); // true
// 检查某个 value 是否存在
print(a.containsValue(66)); // true
// 更新数据
a.update('name', (val) => 'tom'); // {name: jay, age: 66, key: value} {name: tom, age: 66, key: value}
// 批量更新
a.updateAll((key, val) => "值为：$val"); // {name: 值为：tom, age: 值为：66, key: 值为：value}
// 删除数据
a.remove('name'); // {age: 值为：66, key: 值为：value}
// 按条件删除
a.removeWhere((key,val) => key == 'key'); // {age: 值为：66}
// 清空数据
a.clear(); // {}
```
## 集合 Set
### 声明
``` dart
var a = Set();
var b = <dynamic>{};
```
### 去重
``` dart
var list = ['tom', 'jerry', 'jay', 'tom'];
var names = <dynamic>{};
names.addAll(list);
print(names); // {tom, jerry, jay}
// 转 list
print(names.toList()); // [tom, jerry, jay]
```
### 属性
``` dart
var a = Set();
a.addAll(['tom', 'jerry', 'jay']);
print(a.first);  // tom，第一个值
print(a.last);  // jay，最后一个值
print(a.length);  // 3，长度
print(a.isEmpty);  // false，是否为空
print(a.isNotEmpty);  // true，是否不为空
```
### 方法

| 名称           | 说明         |
| ------------ | ---------- |
| addAll       | 添加         |
| contains     | 查询单个       |
| containsAll  | 查询多个       |
| difference   | 集合不同       |
| intersection | 交集         |
| union        | 联合         |
| lookup       | 按对象查询到返回对象 |
| remove       | 删除单个       |
| removeAll    | 删除多个       |
| clear        | 清空         |
| firstWhere   | 按条件正向查询    |
| lastWhere    | 按条件反向查询    |
| removeWhere  | 按条件删除      |
| retainAll    | 只保留几个      |
| retainWhere  | 按条件只保留几个   |

### 交集联合
``` dart
var a = <String>{'jay', 'tom', 'jerry', 'echo'};
var b = <String>{'jay', 'bob', 'trump', 'echo'};

// 交集
print(a.intersection(b)); // {jay, echo}
// 联合
print(a.union(b)); // {jay, tom, jerry, echo, bob, trump}
```
## 枚举 enum
### 声明
``` dart
enum StatusType {
	success,
	error,
	failed,
}
```
## 注释 comments
### 单行注释
``` dart
// ...
```
### 多行注释
``` dart
/*
...
...
*/

```
### 文档注释
``` dart
/// ...
```
## 函数 Function
### 定义
``` dart
// 函数名前边为返回类型，声明 add 函数，接收一个 int 参数，并返回 int 值
int add(int n) {
	return x + 1;
}

add(1);
```
### 可选参数
``` dart
// 声明 example 函数，接收 int 类型的参数 a；int 类型的可选参数 b 其默认值为 2；int 类型的可选参数 c 其默认值为 3，返回一个 int 类型的值
int example(int a, [int b = 2, int c = 3]) {
	return a + b + c;
}

example(1); // 结果为 6
example(1, 3); // 结果为 7
example(1, 3, 5); // 结果为 9
```
### 命名参数
``` dart
int example(int a, {int b = 2, int c = 3}) {
	return a + b + c;
}

example(1); // 结果为 6
example(1, c: 5); // 结果为 8
example(1, b: 3); // 结果为 7
example(1, b: 3, c: 5); // 结果为 9
```
### 返回函数
``` dart
// 函数 example 返回了一个匿名函数
Function example(int a) {
	return (int b){
		return a + b;
	};
}

example(1)(1); // 2
```
## 操作符

| 操作符                                                    | 说明   |
| ------------------------------------------------------ | ---- |
| `expr++` `expr--` `()` `.` `?.`                        | 后缀操作 |
| `-expr` `!expr` `~expr` `++expr` `--expr`              | 前缀操作 |
| `*` `/` `%` `~/`                                       | 乘除   |
| `+` `-`                                                | 加减   |
| `<<` `>>`                                              | 位移   |
| `&`                                                    | 按位与  |
| `^`                                                    | 按位异或 |
| \|                                                     | 按位或  |
| `>=` `>` `<=` `<` `as` `is` `is!`                      | 类型操作 |
| == `!=`                                                | 相等   |
| `&&`                                                   | 逻辑与  |
| \|\|                                                   | 逻辑或  |
| `??`                                                   | 是否为空 |
| `expr1 ? expr2 : expr3`                                | 三目运算 |
| `..`                                                   | 级联   |
| =  *=  /=  ~/=  %=  +=  -=  <<=  >>=  &=  ^=  \|=  ??= | 赋值   |
**上边**的优先级**高于下边**的优先级，**左边**的优先级**高于右边**的优先级

### 类型判定操作符

| 操作符   | 说明                 |
| ----- | ------------------ |
| `as`  | 类型转换               |
| `is`  | 如果对象是指定的类型返回 True  |
| `is!` | 如果对象是指定的类型返回 False |

``` dart
int n = 123;
String str = 'jay';
print(n as Object);
print(str is String); // true
print(str is! String); // false
```
### 三元运算
``` dart
bool isTrue = true;
String str = isTrue ? 'T' : 'F';
print(str); // T
```
## 流程控制
### if else
``` dart
String name = 'jay';
bool b = true;
if (b) {
	print('true');
} else {
	print('false');
}
if (name == 'jay') {
	print('jay');
}
```
### for
``` dart
// 遍历
var list = [1, 2, 3, 4, 5];
for (var i = 0; i < list.length; i++) {
  print(i);
}
```
### while
``` dart
int i = 0;
while(i < 5) {
  print(i);
  i++;
}
```
### do while
``` dart
// 先执行一次才判断 while 的表达式
bool isRunning = true;
do {
  print('is running');
  isRunning = false;
} while (isRunning);
```
### switch case
``` dart
String name = 'jay';
switch (name) {
  case 'jay':
    print('jay');
    break;
  default:
    print('not find');
}

```
### break
``` dart
for (var i = 0; i < 5; i++) {
	if (i == 2) {
		break; // 跳出循环
	}
	print(i); // 0 1
}

int i = 0;
while(i < 5) {
	if (i < 2) {
		break; // 跳出循环
	}
	print(i); // 0 1
	i++;
}
```
### continue
``` dart
for (var i = 0; i < 5; i++) {
	if (i == 2) {
		continue; // 跳出循环
	}
	print(i); // 0 1 3 4
}

String command = 'close';
switch (command) {
  case 'open':
    print('open');
    break;
  case 'close':
    print("close"); // 执行
    continue doClear;
  case 'close2':
    print('close2');
    continue doClear;
  doClear:
  case 'doClose':
    print('doClose'); // 执行
    break;
  default:
    print('other');
}
```
## 错误处理 error

### 错误类型
- Exception

| 名称                           | 说明         |
| ----------------------------- | ---------- |
| `DeferredLoadException`       | 延迟加载错误    |
| `FormatException`             | 格式错误      |
| `IntegerDivisionByZeroException` | 整数除零错误    |
| `IOException`                 | IO 错误      |
| `IsolateSpawnException`       | 隔离产生错误    |
| `TimeoutException`            | 超时错误      |

- Error

| 名称                                | 说明              |
| --------------------------------- | --------------- |
| `AbstractClassInstantiationError` | 抽象类实例化错误        |
| `ArgumentError`                   | 参数错误            |
| `AssertionError`                  | 断言错误            |
| `AsyncError`                      | 异步错误            |
| `CastError`                       | Cast 错误         |
| `ConcurrentModificationError`     | 并发修改错误          |
| `CyclicInitializationError`       | 周期初始错误          |
| `FallThroughError`                | Fall Through 错误 |
| `JsonUnsupportedObjectError`      | JSON 不支持错误      |
| `NoSuchMethodError`               | 没有这个方法错误        |
| `NullThrownError`                 | Null 错误错误       |
| `OutOfMemoryError`                | 内存溢出错误          |
| `RemoteError`                     | 远程错误            |
| `StackOverflowError`              | 堆栈溢出错误          |
| `StateError`                      | 状态错误            |
| `UnimplementedError`              | 未实现的错误          |
| `UnsupportedError`                | 不支持错误           |
### 抛出错误
``` dart
  // Exception 对象
  throw new FormatException('这是一个格式错误提示');

  // Error 对象
  throw new OutOfMemoryError();

  // 任意对象
  throw '这是一个异常';
```
### 捕获错误
``` dart
try {
  // throw new FormatException('这是一个格式错误提示');
  throw new OutOfMemoryError();
} on OutOfMemoryError {
  // on 可以捕获到某种类型的错误
  print('没有内存了');

  rethrow; // 重新抛出错误
} catch (e) {
  print(e); // 捕获到的错误
} finally {
  print('finally'); // finally 永远执行
}

```
## 类 class
### 声明
``` dart
class Human {
  String name;
  num age;
  Map other;

  // 构造函数，() 中的参数需要调用时提供，: 后定义初始化数据
  Human(this.name, this.age) : other = {'skin': 'yellow'};

  @override
  String toString() {
    // 重写 toString 方法
    return '$name, $age, $other';
  }

  // callable
  call(String msg) {
	  print('$msg');
  }
}

void main() {
  var jay = Human('jay', 18);
  print(jay); // jay, 18, {skin: yellow}
  jay('hello world'); // hello world，callable 调用
}
```
### 命名构造函数
``` dart
class Human {
  String name;
  num age;
  Map other;

  Human.fromJson(Map json)
      : name = json['name'],
        age = json['age'],
        other = {'skin': 'yellow', 'death': json['age'] + 100};

  @override
  String toString() {
    // 重写 toString 方法
    return '$name, $age, $other';
  }
}

void main() {
  var jay = Human.fromJson({'name': 'jay', 'age': 18});
  print(jay); // jay, 18, {skin: yellow, death: 118}
}
```
### 重定向构造函数
``` dart
class Human {
  String name;
  num age;
  Map other;
  Human(this.name, this.age) : other = {'skin': 'yellow'};
  Human.fromJson(Map json) : this(json['name'], json['age']);

  @override
  String toString() {
    // 重写 toString 方法
    return '$name, $age, $other';
  }
}

void main() {
  var jay = Human.fromJson({'name': 'jay', 'age': 18});
  print(jay); // jay, 18, {skin: yellow}
}
```
### get set 方法
``` dart
class Human {
  String? _name; // ? 是可选

  // set name(String val) => _name = val; 匿名函数
  set name(String val) {
    _name = val;
  }
  
  // String get name => 'name is $_name'; 匿名函数
  String get name {
    return 'name is $_name';
  }
}

void main() {
  var p = Human();
  p.name = 'jay';
  print(p.name); // name is jay
}
```
### 静态变量
``` dart
class Human {
  // 静态变量
  static String name = 'default name';
  // 静态方法
  static void printName() {
    print('name is $name');
  }
}

void main() {
  print(Human.name); // default name
  Human.printName(); // name is default name
}
```
### 抽象类
``` dart
// 在普通类前加 abstract
abstract class Human {
  String name = 'default name';
  void printName() {
    print('name is $name');
  }
}

// 继承
class Man extends Human {
}

// implements, 通过 @override 实现 Human 内部变量方法
class Woman implements Human {
  @override
  String name;

  Woman(this.name);

  @override
  void printName() {
    print('this woman\'s name is $name');
  }
}

var jay = Man();
jay.printName(); // name is default name

var echo = Woman('echo');
echo.printName(); // This woman's name is echo
  
// 抽象类不能直接实例化
var p = Human(); // Error

```
### 实现多个抽象类
``` dart
abstract class IHuman {
  String name = 'default name';
  String printName() {
    return 'name is $name';
  }
}

abstract class ITools {
  String toolName = 'default toolName';
  String useTools() {
    return 'using tools';
  }
}

class Man implements IHuman, ITools {
  @override
  String name;
  String toolName;
  Man(this.name, this.toolName);

  @override
  String printName() {
    return 'This man\'s name is $name';
  }

  @override
  String useTools() {
    return '$name using a $toolName';
  }
}

void makeHumanInfo(IHuman p) => print(p.printName());
void makeToolsInfo(ITools p) => print(p.useTools());

void main() {
  var jay = Man('jay', 'pen');
  print(jay.printName());
  print(jay.useTools());

  makeHumanInfo(jay);
  makeToolsInfo(jay);
}
```
### 继承
``` dart
class Human {
  String name;
  Human(this.name);

  void sayHi() {
    print('hello');
  }

  void run() {
    print('run');
  }

  void useTools() {
    print('using tools');
  }
}

class Man extends Human {
  Man(String name) : super(name); // 调用父类的构造函数

  @override
  void run() {
    super.run(); // super 对象可以访问父类
    print('this man can run');
  }

  @override
  void useTools() {
    print('this man can use tools');
  }

  @override
  void noSuchMethod(Invocation mirror) {
    print('noSuchMethod');
  }
}

void main() {
  var p = Man('jay');
  p.sayHi(); // hello
  p.run(); // run  // this man can run
  p.useTools(); // This man can use tools

  dynamic tom = Man('tom');
  tom.xxx(); // noSuchMethod 调用未实现的方法
}
```

## 工厂函数
### 调用子类
``` dart
abstract class Human {
  void run();
  void useTools();
  factory Human(String gender) {
    switch (gender) {
      case 'man':
        return Man();
      case 'woman':
        return Woman();
      default:
        throw 'gender error';
    }
  }
}

class Man implements Human {
  @override
  void run() {
    print('this man can run');
  }

  @override
  void useTools() {
    print('this man can use tools');
  }
}

class Woman implements Human {
  @override
  void run() {
    print('this woman can run');
  }

  @override
  void useTools() {
    print('this woman can use tools');
  }
}

void main() {
  var man = Human('man');
  var woman = Human('woman');
//   var x = Human('x');
  man.run(); // this man can run
  man.useTools(); // this man can use tools
  woman.run(); // this woman can run
  woman.useTools(); // this woman can use tools
//   x.run(); // 抛出错误 gender error
}
```
### 单例模式
``` dart
class Human {
  // 创建静态方法
  static final Human _instance = Human._internal();
  Human._internal();

  // 工厂方法
  factory Human() {
    return _instance;
  }

  void useTools() {
    print('use tools');
  }
}

void main() {
  var p1 = Human();
  var p2 = Human();
  print(identical(p1, p2)); // true

  Human().useTools(); // use tools
}
```
### 减少重复实例对象
``` dart
class Human {
  String _name;
  Human(this._name);

  factory Human.fromJson(Map<String, dynamic> json) =>
      Human(json['name'] as String);

  // 不使用 factory 写法
//   static fromJson(Map<String, dynamic> json) => Human(json['name'] as String);

  void printName() {
    print('$_name');
  }
}

void main() {
  var p = Human.fromJson({'name': 'jay'});
  p.printName(); // jay
}
```
## 混入 mixin
``` dart
// mixin 内部不能定义构造函数
mixin Animal {
  void sleep() {
    print('Animal sleep');
  }
}
mixin Human {
  void sleep() {
    print('Human sleep');
  }

  void useTools() {
    print('Human use tools');
  }
}

// on 关键字限定 Human
mixin People on Human {
  void sleep() {
    // 加了 on 关键字才能使用 super 访问 on 后边类的方法
    super.sleep();
    print('People sleep');
  }

  void run() {
    print('People run');
  }
}

// with 可以加入多个 mixin
// People 使用 on 限定了 Human，所以 People 必须在 Human 后边
class Man with Animal, Human, People {}

void main() {
  var p = Man();
  // Animal Human People 都有 sleep 方法。所以会按照 with 的顺序，最后的覆盖之前的
  p.sleep(); // Human sleep super.sleep() 打印 // People sleep
  p.run(); // People run
  p.useTools(); // Human use tools
}

```
## 泛型
### 声明
``` dart
// dart 泛型和 ts 一样
void main() {
  var str = <String>[];
  str.add('hi');
  // str.add(123); // Error, int 不能赋值给 String

  var map = <String, String>{};
  map['a'] = 'hi';
  // map[123] = 123; // Error
}
```
### 函数泛型
``` dart
V returnValue<K, V>(K key, V val) {
  return val;
}

void main() {
  returnValue('name', 123);
  returnValue<String, int>('name', 123);
}
```
### 构造函数泛型
``` dart
class Phone<T> {
  final T number;
  Phone(this.number);
}

void main() {
  var number = Phone('12333333');
  print(number);
  var number2 = Phone(123444445);
  print(number2);
}
```
### 泛型约束
``` dart
class Man {
  void sleep() {
    print('Man sleep');
  }
}

class Human<T extends Man> {
  final T people;
  Human(this.people);
}
void main() {
  var p = Man();
  var h = Human<Man>(p);
  h.people.sleep();
}
```
## 异步 async
### 异步回调
``` dart
import 'dart:html'; // 用于发起 HTTP 请求

void main() {
  // 和 js Promise.then 类似
  HttpRequest.getString(
    'https://api.github.com/search/users?q=jay',
  ).then((response) {
    print(response);
  });
}
```
### async await
``` dart
import 'dart:html'; // 用于发起 HTTP 请求

void main() async {
  // 和 js 的 async await 类似
  final data = await HttpRequest.getString(
    'https://api.github.com/search/users?q=jay',
  );
  print(data); // 显示原始数据
}
```
## 生成器
### 同步生成器
``` dart
Iterable<int> naturalsTo(int n) sync* {
  print('start');
  int k = 0;
  while (k < n) {
    yield k++; // yield 会等待 moveNext 指令
  }
  print('end');
}

main() {
  var it = naturalsTo(3).iterator;
  it.moveNext(); // start
  print(it.current); // 0
  it.moveNext();
  print(it.current); // 1
  it.moveNext();
  print(it.current); // 2
  it.moveNext(); // end
  //     while (it.moveNext()) {
  //       print(it.current);
  //     }
}
```
### 异步生成器
``` dart
import 'dart:async';

Stream<int> asynchronousNaturalsTo(int n) async* {
  print('start');
  int k = 0;
  while (k < n) {
    yield k++;
  }
  print('end');
}

main() {
  // 流监听
  //   asynchronousNaturalsTo(3).listen((onData) {
  //     print(onData);
  //   });

  // 流监听 StreamSubscription 对象
  StreamSubscription subscription = asynchronousNaturalsTo(3).listen(null);
  subscription.onData((value) {
    print(value);
    // subscription.pause();
  });
}
```
### 递归生成器
``` dart
Iterable<int> naturalsDownFrom(int n) sync* {
  print('start');
  if (n > 0) {
    yield n;
    // yield* 以指针的方式传递递归对象，而不是整个同步对象
    yield* naturalsDownFrom(n - 1);
  }
  print('end');
}

main() {
  var it = naturalsDownFrom(3).iterator;

  it.moveNext(); // start
  print(it.current); // 3
  it.moveNext(); // start
  print(it.current); // 2
  it.moveNext(); // start
  print(it.current); // 1
  it.moveNext(); // start // end // end // end // end
  //   while (it.moveNext()) {
  //     print(it.current);
  //   }
}
```
## 类型定义 typedef
``` dart
// typedef 关键字，类似 ts 的 type
typedef MyMap = Map<String, String>;
typedef MyList = List<int>;
typedef MyAdd = int Function(int a, int b);

class MyClass {
  // void Function(int a, int b) add;
  MyAdd add;
  MyClass(this.add);
}

void main() {
  MyMap m = {'name': 'jay'};
  m['age'] = '18';
  print(m); // {name: jay, age: 18}

  MyList list = [1, 2, 3];
  list.add(4);
  print(list); // [1, 2, 3, 4]

  MyAdd add = (a, b) => a + b;
  print(add(1, 2)); // 3

  MyClass c = MyClass(add);
  print(c.add(6, 6)); // 12
}

```
## 空安全
### 可以为空
``` dart
// <type>? 可选，可以为空
String? name;
List<String>? list; // List 可以为空
List<String?> list2;// List 里的属性可以为空
```
### late
``` dart
// late 关键字，声明一个不可为空的变量，在声明后初始化
late String name; // 可以在声明时不初始化，在之后再初始化。之后不初始化会报错
```
### 断言值不为为空
``` dart
// <value>! 断言值不为为空
String? name;
// name 是现在是空，用 ！ 断言它不为空，类型检查不会报错，但运行时因为 name 实际为空，所以会报错
String userName = name!;
```
### 不为空时执行
``` dart
// <value>?. 类似 ts 可选链
// ?. 前的值不为空才执行
a?.b;
```
### ??
``` dart
// <value> ?? <value2>
// ?? 前边的值存在表达式结果就是前边的值，否则就是后边的值
String type = status ?? 'default';
```
## 拓展 extension
``` dart
import 'package:intl/intl.dart';

// extension 拓展 DateTime
extension ExDateTime on DateTime {
  /// 方法，格式化日期 yyyy-MM-dd
  String toDateString({String format = 'yyyy-MM-dd'}) =>
      DateFormat(format).format(this);

  // 属性
  int get nowTicket => this.microsecondsSinceEpoch;
}

main() {
  var now = DateTime.now();

  print(now.toDateString());
  print(now.nowTicket);
}

```
## 导入库 import
``` dart
// 使用 import 关键字导入包
// 导入核心包
import 'dart:io';
// 导入第三方包
import 'package:intl/intl.dart';
// 导入本地包
// demo_project 是 pubspec.yaml 文件中 name 定义的
import 'package:demo_project/demo.dart';
// as 关键字为导入的包添加别名
import 'package:demo_project/demo.dart' as d;

// 筛选包内容
// show 关键字指定导入的包名
import 'package:demo_project/demo.dart' show <package name>;
// hide 关键字指定不导入的包名
import 'package:demo_project/demo.dart' hide <package name>;

// deferred 关键字包延迟加载
import 'package:demo_project/demo.dart' deferred as d;
main() async {
  await d.loadLibrary(); // 在需要使用包的时候用 loadLibrary 加载包
}
```
通过 `dart pub get` 安装 pubspec.yaml 文件里的依赖
