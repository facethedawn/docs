# JavaSctipt高级程序设计
## 1、什么是JavaScript
### 1.2 JavaScript实现
<!-- [[toc]] -->
完整的JavaScript实现包括：
* 核心（ECMAScript）
* 文档对象模型（DOM）
* 浏览器对象模型（BOM）

#### 1.2.2 DOM
文档对象模型（DOM，Document Object Model）是一个应用编程接口（API），用于在HTML中使用扩展的XML。

## 3、语言基础
### 3.1 语法
#### 标识符
标识符就是变量、函数、属性或者函数参数的名称。它可以是：
* 第一个字符必须是一个字母、下划线(_)或者美元符号($)
* 剩下的其他字符可以是字母、下划线、美元符号或数字。

### 3.3 变量
var、const、let。var在ECMAScript全版本中都可以使用，而const和let只能在ES6及以后的版本中使用。
#### var 关键字
> 虽然可以通过省略var操作符定义全局变量，但是不推荐。在局部作用域中定义全局变量很难维护。

##### var声明提升
使用var时，下面的代码不会报错。这是因为使用这个关键字声明的变量会自动提升到函数作用域顶部：
```js
function foo() {
console.log(age);
var age = 26;
}
foo(); // undefined
```
相当于
```js
function foo() {
var age;
console.log(age);
age = 26;
}
foo(); // undefined
```

#### let 声明
let声明的范围是块作用域，var声明的范围是函数作用域。块作用域是函数作用域的子集，因此适用于var的作用域同样也适用于let。
```js
if (true) {
var name = 'Matt';
console.log(name); // Matt
}
console.log(name); // Matt
```
```js
if (true) {
let age = 26;
console.log(age); // 26
}
console.log(age); // ReferenceError: age 没有定义
```
##### 暂时性死区
let与var的另一个重要区别就是let声明的变量不会再作用域中被提升
```js
// name 会被提升
console.log(name); // undefined
var name = 'Matt';
// age 不会被提升
console.log(age); // ReferenceError：age 没有定义
let age = 26;
```
在解析代码时，JavaScript 引擎也会注意出现在块后面的let 声明，只不过在此之前不能以任何方式来引用未声明的变量。在let 声明之前的执行瞬间被称为“**暂时性死区**”（temporal dead zone），在此阶段引用任何后面才声明的变量都会抛出ReferenceError。

##### 全局声明
与var 关键字不同，使用let 在全局作用域中声明的变量不会成为window 对象的属性（var 声明的变量则会）。

不过，let 声明仍然是在全局作用域中发生的，相应变量会在页面的生命周期内存续。因此，为了避免SyntaxError，必须确保页面不会重复声明同一个变量。

##### 条件声明
在使用var 声明变量时，由于声明会被提升，JavaScript 引擎会自动将多余的声明在作用域顶部合并为一个声明。因为let 的作用域是块，所以不可能检查前面是否已经使用let 声明过同名变量，同时也就不可能在没有声明的情况下声明它。
>不能使用let 进行条件式声明是件好事，因为条件声明是一种反模式，它让程序变得更难理解。

##### for循环中的let声明
在let出现之前，for循环定义的迭代变量会渗透到循环体的外部。
```js
for (var i = 0; i < 5; ++i) {
setTimeout(() => console.log(i), 0)
}
// 你可能以为会输出0、1、2、3、4
// 实际上会输出5、5、5、5、5
```
> 所有同步任务在主线程中执行，形成一个“执行栈”，而异步任务都会进入到任务队列中等待，只有当主线程里的同步任务都被执行完毕，异步任务才会进入主线程中被执行(来自网络)

```js
for (let i = 0; i < 5; ++i) {
setTimeout(() => console.log(i), 0)
}
// 会输出0、1、2、3、4
```
#### const 声明
const行为与let基本一致，唯一的重要区别是用它声明的变量时必须同时初始化变量，且尝试修改const声明的变量会导致运行时错误。

const 声明的限制只适用于它指向的变量的引用。换句话说，如果const 变量引用的是一个对象，
那么修改这个对象内部的属性并不违反const 的限制。

```js
const person = {};
person.name = 'Matt'; // ok
```

#### 声明风格及最佳实践
1. **不使用var**
2. **const优先，let次之。** 使用 const 声明可以让浏览器运行时强制保持变量不变，也可以让静态代码分析工具提前发现不合法的赋值操作。

### 3.4 数据类型
* 简单数据类型：Undefined、Null、Boolean、Number、String和Symbol
* 复杂数据类型：Object

#### typeof 操作符

* "undefined"表示值未定义；
* "boolean"表示值为布尔值；
* "string"表示值为字符串；
* "number"表示值为数值；
* "object"表示值为对象（而不是函数）或null；
* "function"表示值为函数；
* "symbol"表示值为符号。

> 严格来讲，函数在ECMAScript中被认为是对象，并不代表一种数据类型。可是函数也有自己特殊的属性。为此就有必要区分函数和其他对象。

#### Undefined 类型
Undefined 类型只有一个值，就是特殊值 undefined。当使用 var 或 let 声明了变量但没有初始化时，就相当于给变量赋予了 undefined 值。

#### Null 类型
Null 只有一个值，即特殊值 null。逻辑上讲，null 值表示一个**空对象指针**，这也是给 typeof 传一个nul会返回 "object" 的原因。

在定义将来要保存对象值的变量时，建议使用 null 来初始化，不要使用其他值。

#### Boolean 类型
布尔类型有两个字面值：true 和 false。
##### Boolean() 转型函数
```js
let message = 'hello world';
let messageAsBoolean = Boolean(message); 
```
##### 不同类型与布尔值之间的转换规则：

| 数据类型 |转换为true的值 | 转换为false的值 |
| --- | --- | --- |
| Boolean | true | false |
| String | 非空字符串 | ""(空字符串) | 
| Number | 非零整数（包括无穷值） | 0、NaN | 
| Object | 任意对象 | null | 
| Undefined | N/A | nudefined | 

#### Number 类型
##### 浮点值
要定义浮点值，数值中必须包含小数点，而且小数点后面必须至少有一个数字。虽然小数点前面不是必须有整数，但推荐加上。

因为存储浮点值使用的内存空间是存储整数值的两倍，所以ECMAScript 总是想方设法把值转换为整数。在小数点后面没有数字的情况下，数值就会变成整数。类似地，如果数值本身就是整数，只是小数点后面跟着0（如1.0），那它也会被转换为整数

不要用浮点值做算数计算。

##### NaN
NaN（Not a Number）。用于表示本来要返回数值的操作失败了（而不是抛出错误）

任何涉及NaN 的操作始终返回 NaN。NaN 不等于包括NaN 在内的任何值。

###### isNaN() 函数
接受一个参数，判断参数是否是 NaN。
```js
console.log(isNaN(NaN)); // true
console.log(isNaN(10)); // false，10 是数值
console.log(isNaN("10")); // false，可以转换为数值10
console.log(isNaN("blue")); // true，不可以转换为数值
console.log(isNaN(true)); // false，可以转换为数值1
```

##### 数值转换
Number() 、 parseInt() 和 parseFloat()

###### Number()
```js
let num1 = Number("Hello world!"); // NaN
let num2 = Number(""); // 0
let num3 = Number("000011"); // 11
let num4 = Number(true); // 1
```

###### parseInt()
考虑到用 Number() 函数转换字符串时相对复杂且有点反常规，通常在需要得到整数时可以优先使用parseInt() 函数。
```js
let num1 = parseInt("1234blue"); // 1234
let num2 = parseInt(""); // NaN
let num3 = parseInt("0xA"); // 10，解释为十六进制整数
let num4 = parseInt(22.5); // 22
let num5 = parseInt("70"); // 70，解释为十进制值
let num6 = parseInt("0xf"); // 15，解释为十六进制整数
let num7 = parseInt("AF", 16); // 175
let num8 = parseInt("AF"); // NaN
```
因为不传底数参数相当于让 parseInt() 自己决定如何解析，所以为避免解析出错，建议始终传给它第二个参数。

###### parseFloat()
parseFloat()只能解析十进制格式，并且会始终忽略字符串开头的零。
```js
let num1 = parseFloat("1234blue"); // 1234，按整数解析
let num2 = parseFloat("0xA"); // 0
let num3 = parseFloat("22.5"); // 22.5
let num4 = parseFloat("22.34.5"); // 22.34
let num5 = parseFloat("0908.5"); // 908.5
let num6 = parseFloat("3.125e7"); // 31250000
```

#### String 类型
字符串可以用双引号(")、单引号(')、反引号(`)标示。

##### 字符字面量

字符串长度可以通过 length 属性获取。
```js
let text = "This is the letter sigma: \u03a3.";
console.log(text.length); // 28
```

##### 字符串的特点
ECMAScript 中的字符串是不可变的（immutable），意思是一旦创建，它们的值就不能变了。要修改某个变量中的字符串值，必须先销毁原始的字符串，然后将包含新值的另一个字符串保存到该变量。

##### 转换为字符串
###### toString()方法
```js
let age = 11;
let ageAsString = age.toString(); // 字符串"11"
let found = true;
let foundAsString = found.toString(); // 字符串"true"
```
toString()方法可见于数值、布尔值、对象和字符串值。（没错，字符串值也有toString()方法，该方法只是简单地返回自身的一个副本。）null 和undefined 值没有toString()方法。
> 用加号操作符给一个值加上一个空字符串""也可以将其转换为字符串

##### 模版字面量
ES6新增。

与使用单引号或双引号不同，模板字面量保留换行字符，可以跨行定义字符。

##### 字符串插值
模板字面量最常用的一个特性是支持字符串插值，也就是可以在一个连续定义中插入一个或多个值。技术上讲，模板字面量不是字符串，而是一种特殊的JavaScript 句法表达式，只不过求值后得到的是字符串。

字符串插值通过在${}中使用一个JavaScript 表达式实现：
```js
let value = 5;
let exponent = 'second';
let interpolatedTemplateLiteral = `${ value } to the ${ exponent } power is ${ value * value }`;
console.log(interpolatedTemplateLiteral); // 5 to the second power is 25
```
##### 模板字面量标签函数
##### 原始字符串

#### Symbol 类型
ES6新增。符号是原始值，且符号实例是唯一、不可变的。符号的用途是确保对象属性使用唯一标识符，不会发生属性冲突的危险。
> 可以理解为独一无二的字符串

##### 符号的基本用法
```js
let sym = Symbol();
console.log(typeof sym); // symbol
```
可以使用 description 属性访问 Symbol 的描述。（ES2019开始。之前都是用toString()方法）
```js
let sym = Symbol('foo');
console.log(sym.toString()); // Symbol(foo)
console.log(sym.description); // foo
```
```js
let genericSymbol = Symbol();
let otherGenericSymbol = Symbol();
let fooSymbol = Symbol('foo');
let otherFooSymbol = Symbol('foo');
console.log(genericSymbol == otherGenericSymbol); // false
console.log(fooSymbol == otherFooSymbol); // false
```
##### 使用全局符号注册表






## BOM
### window对象
window 对象在浏览器中有两重身份，一个是ECMAScript 中的Global对象，另一个就是浏览器窗口的JavaScript 接口。
#### 窗口位置和像素比
```js
console.log(window.devicePixelRatio); //1
```
#### 窗口大小
* innerWidth
* innerHeight
* outerWidth
* outerHeight


























