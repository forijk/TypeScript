# TypeScript 总结

- TypeScript 是 JavaScript 的类型的超集，它可以编译成纯 JavaScript。编译出来的 JavaScript 可以运行在任何浏览器上。TypeScript 编译工具可以运行在任何服务器和任何系统上。TypeScript 是开源的。

## 优势

- 增加代码可读性和可维护性
- 拥有活跃的社区
- TypeScript 是 JavaScript 的超集，.js 文件可以直接重命名为 .ts 即可
- 即使不显式的定义类型，也能够自动做出类型推论
- 可以定义从简单到复杂的一切类型
- 即使 TypeScript 编译报错，也可以生成 JavaScript 文件
- 兼容第三方库，即使第三方库不是用 TypeScript 写的，也可以> 编写单独的类型文件供 TypeScript 读取

## 基础

    原始数据类型
    任意值
    类型推论
    联合类型
    接口
    数组
    函数
    类型断言
    声明文件
    内置对象

- 原始数据类型：布尔值、数值、字符串、空值void、Null、Undefined、任意值any

- 类型推论
    - 如果没有明确的指定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出一个类型。

- 联合类型

```javascript
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

- 接口 interface 
    - 可选属性
    - 任意属性
    - 只读属性

- 数组

定义数组：有多种定义方式 
1. 「类型 + 方括号」表示法

let fibonacci: number[] = [1, 1, 2, 3, 5];
1
数组的项中不允许出现其他的类型 
2. 使用数组泛型（Generic） Array 来表示数组

let fibonacci: Array<number> = [1, 1, 2, 3, 5];

用接口表示数组

interface NumberArray {
  [index: number]: number;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5];
//NumberArray 表示：只要 index 的类型是 number，那么值的类型必须是 number。

- 函数

```javascript
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};

```

- 函数重载 
重载允许一个函数接受不同数量或类型的参数时，作出不同的处理。

function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {...}

- 类型断言（Type Assertion）可以用来绕过编译器的类型推断，手动指定一个值的类型（即程序员对编译器断言）。

类型断言不是类型转换。

```javascript
（1）语法:

<类型>值
// 或
值 as 类型
// 在TSX语法 (React的JSX语法的TS版）中必须用后一种
（2）例子：使用类型断言，将一个联合类型的变量指定为一个更加具体的类型。

function toBoolean(something: string | number): boolean {
  return <string>something;
}

function getLength(something: string | number): number {
  let length=something.length;//error
  let xlength=something.length;//OK
}
```

- 声明文件

当时用第三方库时，我们需要引用它的声明文件。

申明语句：使用declare关键字来定义类型

如：我们需要使用第三方库Jquery获取一个id是foo的元素

declare var jQuery: (string) => any;//类型声明
jQuery('#foo');//单独书写时typescript并不识别$或者jquery

通常我们会将类型声明放在一个单独的文件中，约定声明文件以.d.ts为后缀。

// jQuery.d.ts

declare var jQuery: (string) => any;

使用声明文件：用「三斜线指令」表示引用了声明文件

/// <reference path="./jQuery.d.ts" />

jQuery('#foo');

TypeScript2.0推荐使用@types来管理，即使用nm安装对应声明模块

如：npm install @types/jquery --save-dev

官方搜索声明文件链接：http://microsoft.github.io/TypeSearch/

- 内置对象
    ES标准提供的内置对象有： 
    Boolean、Error、Date、RegExp 等
    DOM 和 BOM 提供的内置对象有： 
    Document、HTMLElement、Event、NodeList 等

注意： Node.js不是内置对象的一部分，需要引入声明文件： 
npm install @types/node --save-dev

## 进阶

    类型别名
    字符串字面量类型
    元组
    枚举
    类
    类与接口
    泛型
    声明合并
    扩展阅读

### 类型别名

使用 ‘type’ 创建类型别名，类型别名用来给一个类型起个新名字。类型别名常用于联合类型。

type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
function getName(n: NameOrResolver): Name {...}

### 字符串字面量

字符串字面量类型用来约束取值只能是某几个字符串中的一个。

类型别名与字符串字面量类型都是使用 ‘type’ 进行定义。

```javascript
type EventNames = 'click' | 'scroll' | 'mousemove';
function handleEvent(ele: Element, event: EventNames) {
  // do something
}

handleEvent(document.getElementById('hello'), 'scroll');  // 没问题
handleEvent(document.getElementById('world'), 'dbclick'); // 报错，event 不能为 'dbclick'
```

### 元组

数组合并了相同类型的对象，而元组（Tuple）合并了不同类型的对象。

元组起源于函数编程语言（如 F#）,在这些语言中频繁使用元组。

（1）定义元组：

let tuple: [string, number];

let xtuple: [string, number] = ['Xcat Liu', 25];

（2）元组的赋值：

1.定义元组时赋值

let xtuple: [string, number] = ['Xcat Liu', 25];

2.使用元组索引赋值，索引从0开始，可只赋值其中一项

xcatliu[0] = 'Xcat Liu';

3.直接对元组赋值，需要提供所有元组类型中指定的项。

let xcatliu: [string, number];
xcatliu = ['Xcat Liu', 25];

### 枚举

（1）定义

使用 ‘enum’ 关键字来定义枚举类型；

enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};

（2）枚举项赋值

1.默认赋值

枚举成员会被赋值为从 0 开始递增的数字，同时也会对枚举值到枚举名进行反向映射。如编译结果所示

var Days;
(function (Days) {
  Days[Days["Sun"] = 0] = "Sun";
  Days[Days["Mon"] = 1] = "Mon";
  Days[Days["Tue"] = 2] = "Tue";
  Days[Days["Wed"] = 3] = "Wed";
  Days[Days["Thu"] = 4] = "Thu";
  Days[Days["Fri"] = 5] = "Fri";
  Days[Days["Sat"] = 6] = "Sat";
})(Days || (Days = {}));

2.手动赋值

（1）使用数字类型

enum Days {Sun = 7, Mon = 1, Tue, Wed, Thu, Fri, Sat};
1
此时，未手动赋值的枚举项会接着上一个枚举项以1递增（使用小数或负数也如此），即：Tue=2，Wed=3，Thu=4，Fri=5，Sat=6。

枚举项的值重复仍可通过编译，只是值被覆盖了。

enum Days {Sun = 3, Mon = 1, Tue, Wed, Thu, Fri, Sat};
//原先Days[3]的值为Sun，现在为Wed
1
2
（2）不使用数字

手动赋值的枚举项可以不是数字，此时需要使用类型断言来让tsc无视类型检查 (编译出的js仍然是可用的)。

enum Days {Sun = 7, Mon, Tue, Wed, Thu, Fri, Sat = <any>"S"};

### 类与接口

一个类可以实现多个接口

interface Alarm {
  alert();
}

interface Light {
  lightOn();
  lightOff();
}

class Car implements Alarm, Light {
  alert() {
    console.log('Car alert');
  }
  lightOn() {
    console.log('Car light on');
  }
  lightOff() {
    console.log('Car light off');
  }
}

接口继承接口

interface Alarm {
  alert();
}

interface LightableAlarm extends Alarm {
  lightOn();
  lightOff();
}

接口继承类

class Point {
  x: number;
  y: number;
}

interface Point3d extends Point {
  z: number;
}

let point3d: Point3d = {x: 1, y: 2, z: 3};

- 实例属性
- ES6 中实例的属性只能通过构造函数中的 this.xxx 来定义，ES7 提案中可以直接在类里面定义：

```javascript
class Animal {
    name = 'Jack';
​
    constructor() {
        // ...
    }
}
​
let a = new Animal();
console.log(a.name); // Jack
```

- 静态属性

- ES7 提案中，可以使用 static 定义一个静态属性：

```javascript
class Animal {
    static num = 42;
​
    constructor() {
        // ...
    }
}
​
console.log(Animal.num); // 42
```

- TypeScript 可以使用三种访问修饰符（Access Modifiers），分别是 public、private 和 protected。

  - public 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 public 的
  - private 修饰的属性或方法是私有的，不能在声明它的类的外部访问
  - protected 修饰的属性或方法是受保护的，它和 private 类似，区别是它在子类中也是允许被访问的

- 修饰器，这是 ES7 的一个提案 ​ http://es6.ruanyifeng.com/#docs/decorator
- ​Mixins：一种编程模式，与 TypeScript 没有直接关系，可以参考 ES6 中 Mixin 模式的实现 ​http://es6.ruanyifeng.com/#docs/class#Mixin模式的实现

### 泛型

泛型（Generics）是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。

泛型使用在函数上

（1）定义返回值

function createArray<T>(length: number, value: T): Array<T> {
  let result = [];
  for (let i = 0; i < length; i++) {
    result[i] = value;
  }
  return result;
}

createArray<string>(3, 'x'); // ['x', 'x', 'x']

（2）多个类型参数

定义泛型的时候，可以一次定义多个类型参数：

function swap<T, U>(tuple: [T, U]): [U, T] {
  return [tuple[1], tuple[0]];
}

swap([7, 'seven']); // ['seven', 7]

泛型约束

（1）在函数内部使用泛型变量的时候，由于事先不知道它是哪种类型，所以不能随意的操作它的属性或方法：

function loggingIdentity<T>(arg: T): T {
  console.log(arg.length);//error
  return arg;
}

interface Lengthwise {
  length: number;
}

function loggingIdentity2<T extends Lengthwise>(arg: T): T {
  console.log(arg.length);//OK
  return arg;
}

loggingIdentity(7);//error，传入的 arg 不包含 length

（2）多个类型参数之间也可以互相约束：

function copyFields<T extends U, U>(target: T, source: U): T {
  for (let id in source) {
    target[id] = (<T>source)[id];
  }
  return target;
}

let x = { a: 1, b: 2, c: 3, d: 4 };

copyFields(x, { b: 10, d: 20 });

上例中，我们使用了两个类型参数，其中要求 T 继承 U，这样就保证了 U 上不会出现 T 中不存在的字段。

泛型接口

（1）反省参数在接口中的函数上：

interface CreateArrayFunc {
  <T>(length: number, value: T): Array<T>;
}

let createArray: CreateArrayFunc;
createArray = function<T>(length: number, value: T): Array<T> {
  let result = [];
  for (let i = 0; i < length; i++) {
    result[i] = value;
  }
  return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']

（2）泛型参数在接口名上：

interface CreateArrayFunc<T> {
  (length: number, value: T): Array<T>;
}

let createArray: CreateArrayFunc<any>;//注意这里需要定义泛型的类型
createArray = function<T>(length: number, value: T): Array<T> {
  let result = [];
  for (let i = 0; i < length; i++) {
    result[i] = value;
  }
  return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']

注意，此时在使用泛型接口的时候，需要定义泛型的类型。
泛型类

与泛型接口类似，泛型也可以用于类的类型定义中：

class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };

### 声明合并

如果定义了两个相同名字的函数、接口或类，那么它们会合并成一个类型。

函数的合并
​之前学习过，我们可以使用重载定义多个函数类型：

function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
接口的合并
接口中的属性在合并时会简单的合并到一个接口中：

interface Alarm {
    price: number;
}
interface Alarm {
    weight: number;
}
相当于：

interface Alarm {
    price: number;
    weight: number;
}
注意，合并的属性的类型必须是唯一的：

interface Alarm {
    price: number;
}
interface Alarm {
    price: number;  // 虽然重复了，但是类型都是 `number`，所以不会报错
    weight: number;
}
interface Alarm {
    price: number;
}
interface Alarm {
    price: string;  // 类型不一致，会报错
    weight: number;
}
​
// index.ts(5,3): error TS2403: Subsequent variable declarations must have the same type.  Variable 'price' must be of type 'number', but here has type 'string'.
接口中方法的合并，与函数的合并一样：

interface Alarm {
    price: number;
    alert(s: string): string;
}
interface Alarm {
    weight: number;
    alert(s: string, n: number): string;
}
相当于：

interface Alarm {
    price: number;
    weight: number;
    alert(s: string): string;
    alert(s: string, n: number): string;
}
类的合并
类的合并与接口的合并规则一致。

