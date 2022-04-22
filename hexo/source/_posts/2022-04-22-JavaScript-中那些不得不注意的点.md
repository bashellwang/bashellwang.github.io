---
title: JavaScript 中那些不得不注意的点
date: 2022-04-22 15:38:35
tags:
- JavaScript
---



> 什么是 JavaScript，它有什么特点？对于客户端原生开发者来说，在学习 JavaScript 过程中，有哪些不得不注意的点？

### 背景
JavaScript 是一种轻量级的脚本语言，同时也是一门动态类型语言。对于 Android 或 iOS
 开发者来说，JavaScript 的动态类型特性会让我们在学习过程中遇到各种“坑”，下面将学习过程中需要注意的点罗列出来作为后续备忘使用。(JavaScript 基于 ECMAScript 5.1 版本，更多历史背景可参考 [JavaScript 语言的历史](https://wangdoc.com/javascript/basic/history.html))

<!-- more -->
 
#### 动态语言和静态语言
##### 动态语言
是一类在运行时可以改变其结构的语言：例如新的函数、对象、甚至代码可以被引进，已有的函数可以被删除或是其他结构上的变化。通俗点说就是在运行时代码可以根据某些条件改变自身结构。

主要动态语言：Object-C、C#、JavaScript、PHP、Python、Erlang。

##### 静态语言
与动态语言相对应的，运行时结构不可变的语言就是静态语言。如Java、C、C++。

##### 注意

很多人认为解释型语言都是动态语言，这个观点是错的！Java 是解释型语言但是不是动态语言，Java 不能在运行的时候改变自己结构。反之成立吗？动态语言都是解释型语言。也是错的！Object-C 是编译型语言，但是他是动态语言。得益于特有的 run time 机制（准确说 run time 不是语法特性是运行时环境，这里不展开）OC 代码是可以在运行的时候插入、替换方法的。

#### 动态类型语言和静态类型语言
##### 动态类型语言
动态类型语言和动态语言是完全不同的两个概念。动态类型语言是指在运行期间才去做数据类型检查的语言，说的是`数据类型`，动态语言说的是运行是改变结构，说的是`代码结构`。

动态类型语言的数据类型不是在编译阶段决定的，而是把类型绑定延后到了运行阶段。

主要语言：Python、Ruby、Erlang、JavaScript、swift、PHP、Perl。

##### 静态类型语言
静态语言的数据类型是在编译其间确定的或者说运行之前确定的，编写代码的时候要明确确定变量的数据类型。

主要语言：C、C++、C#、Java、Object-C。

#### 动态类型语言和静态类型语言优缺点
| 语言 | 优点 | 缺点 |
|----|----|----|
|静态类型| 结构规范，便于调试，方便类型安全 | 额外编写类型相关代码，代码量增加，注意判断变量类型 |
|动态类型| 提高了编码灵活性，代码量少，方便阅读 | 不便调试，易发生类型相关错误 |

以上我们基本了解了动态语言和静态语言、动态类型语言和静态类型语言以及它们的区别。对于 Java、Object-C 静态类型语言开发者，初学 JavaScript 动态类型语言，可能会有一些点需要特别注意，以下我们将常见易混淆的点进行相关罗列说明。


### 数据类型

* 数值（number）：整数和小数（比如1和3.14）。
* 字符串（string）：文本（比如 Hello World ）。
* 布尔值（boolean）：表示真伪的两个特殊值，即 true（真）和 false（假）。
* undefined：表示“未定义”或不存在，即由于目前没有定义，所以此处暂时没有任何值。
* null：表示空值，即此处的值为空。
* 对象（object）：各种值组成的集合。
* Symbol ：表示独一无二的值，ES6 引入，此文不考虑。

通常，数值、字符串、布尔值这三种类型，合称为原始类型（primitive type）；对象是合成类型（complex type）。

#### 变量
##### 变量提升
JavaScript 引擎的工作方式是，先解析代码，获取所有被声明的变量，然后再一行一行地运行。这造成的结果，就是所有的变量的声明语句，都会被提升到代码的头部，这就叫做变量提升(hoisting)。

```
console.log(a);
var a = 1;
```
上面代码首先使用 console.log 方法，在控制台（console）显示变量 a 的值。这时变量 a 还没有声明和赋值，所以这是一种错误的做法，但是实际上不会报错。因为存在变量提升，真正运行的是下面的代码。

```
var a;
console.log(a);
a = 1; // undefined
```

#### null 和 undefied
`undefined` 和 `null` 一般认为是两个特殊值。`if` 语句中，它们均被认为是 `false`; 相等运算符 `==` 两者是相等的。

```
if (!undefined) {
	console.log('undefined is false');
}
// undefined is false

undefined == null // true
undefined === null // false
```
在转为数值时，`null` 会转为 0，`undefined` 会转为 `NaN`。

#### 布尔值
如果 JavaScript 预期某个位置应该是布尔值，会将该位置上现有的值自动转为布尔值。转换规则是除了下面六个值被转为 false，其他值都视为 true。

* `undefined`
* `null`
* `false`
* `0`
* `NaN`
* `""` 或 `''` (空字符串)

更多详情可参考：[null, undefined 和布尔值](https://wangdoc.com/javascript/types/null-undefined-boolean.html)

#### 数值

##### 整数和浮点数
JavaScript 内部，所有数字都是以 64 位浮点数形式储存，即使整数也是如此。由于浮点数不是精确值，涉及小数的比较和运算需要特别小心。

```
1 === 1.0 // true

0.1 + 0.2 // 0.30000000000000004

0.1 + 0.2 === 0.3 // false

(0.3 - 0.2) === (0.2 - 0.1) // false

0.2 + 0.3 === 0.5 // true

0.2 + 0.2 === 0.4 // true
```

##### 正零和负零
JavaScript 内部实际上存在 2 个 0：一个是 +0，一个是 -0，区别就是 64 位浮点数表示法的符号位不同。它们是等价的。

几乎所有场合，正零和负零都会被当作正常的 0。唯一区别的场合，是 `+0` 或 `-0` 当作分母，返回值不一样，一个是 `+Infinity`，另一个是 `-Infinity`。

```
-0 === +0 // true
0 === +0 // true
0 === -0 // true

(1 / +0) === (1 / -0) // false
```

##### NaN
`NaN` 是 JavaScript 的特殊值，表示“非数字”（Not a Number），主要出现在将字符串解析成数字出错的场合，一些数学函数运算结果也会出现 `NaN`。

```
5 - 'x' // NaN
Math.acos(2) // NaN
Math.log(-1) // NaN
Math.sqrt(-1) // NaN

0 / 0 // NaN

// NaN 不是独立的数据类型，而是一个特殊数值，它的数据类型依然属于Number
typeof NaN // 'number'

// NaN不等于任何值，包括它本身。
NaN === NaN // false

// NaN在布尔运算时被当作 false。
Boolean(NaN) // false

// NaN 与任何数（包括它自己）的运算，得到的都是 NaN。
NaN + 32 // NaN
NaN - 32 // NaN
NaN * 32 // NaN
NaN / 32 // NaN
```

##### 与数值相关的方法
`parseInt` 方法用于将字符串转为整数。

```
parseInt('123') // 123

// 如果字符串头部有空格，空格会被自动去除。
parseInt('   81') // 81

//如果parseInt的参数不是字符串，则会先转为字符串再转换。
parseInt(1.23) // 1
// 等同于
parseInt('1.23') // 1

// 字符串转为整数的时候，是一个个字符依次转换，如果遇到不能转为数字的字符，就不再进行下去，返回已经转好的部分。
parseInt('8a') // 8
parseInt('12**') // 12
parseInt('12.34') // 12
parseInt('15e2') // 15
parseInt('15px') // 15

// 如果字符串的第一个字符不能转化为数字（后面跟着数字的正负号除外），返回 NaN。
parseInt('abc') // NaN
parseInt('.3') // NaN
parseInt('') // NaN
parseInt('+') // NaN
parseInt('+1') // 1

// 如果字符串以 0x 或 0X 开头，parseInt 会将其按照十六进制数解析。
parseInt('0x10') // 16

// 如果字符串以 0 开头，将其按照 10 进制解析。
parseInt('011') // 11

// 对于那些会自动转为科学计数法的数字，parseInt 会将科学计数法的表示方法视为字符串，因此导致一些奇怪的结果
parseInt(1000000000000000000000.5) // 1
// 等同于
parseInt('1e+21') // 1

parseInt(0.0000008) // 8
// 等同于
parseInt('8e-7') // 8
```

`parseFloat` 方法用于将一个字符串转为浮点数。

```
parseFloat('3.14') // 3.14

// 如果字符串符合科学计数法，则会进行相应的转换。
parseFloat('314e-2') // 3.14
parseFloat('0.0314E+2') // 3.14

// 如果字符串包含不能转为浮点数的字符，则不再进行往后转换，返回已经转好的部分。
parseFloat('3.14more non-digit characters') // 3.14

// 会自动过滤字符串前导的空格。
parseFloat('\t\v\r12.34\n ') // 12.34

// 如果参数不是字符串，或者字符串的第一个字符不能转化为浮点数，则返回NaN。
parseFloat([]) // NaN
parseFloat('FF2') // NaN
parseFloat('') // NaN

// 与 Number 函数的区别
parseFloat(true)  // NaN
Number(true) // 1

parseFloat(null) // NaN
Number(null) // 0

parseFloat('') // NaN
Number('') // 0

parseFloat('123.45#') // 123.45
Number('123.45#') // NaN
```

`isNaN()` 方法可以用来判断一个值是否为NaN。

```
isNaN(NaN) // true
isNaN(123) // false

// 但是，isNaN只对数值有效，如果传入其他值，会被先转成数值。也就是说，isNaN 为 true 的值，有可能不是 NaN，而是一个字符串。
isNaN('Hello') // true
// 相当于
isNaN(Number('Hello')) // true

// 出于同样的原因，对于对象和数组，isNaN也返回true。
isNaN({}) // true
// 等同于
isNaN(Number({})) // true

isNaN(['xzy']) // true
// 等同于
isNaN(Number(['xzy'])) // true

// 但是，对于空数组和只有一个数值成员的数组，isNaN 返回false。
isNaN([]) // false
isNaN([123]) // false
isNaN(['123']) // false

// 因此，使用isNaN之前，最好判断一下数据类型。
// 判断NaN更可靠的方法是，利用NaN为唯一不等于自身的值的这个特点，进行判断。
function myIsNaN(value) {
  return value !== value;
}
```

`isFinite` 方法返回一个布尔值，表示某个值是否为正常的数值。

```
// 除了Infinity、-Infinity、NaN 和 undefined 这几个值会返回 false，isFinite 对于其他的数值都会返回 true。
isFinite(Infinity) // false
isFinite(-Infinity) // false
isFinite(NaN) // false
isFinite(undefined) // false
isFinite(null) // true
isFinite(-1) // true
```
更多详情请参考：[数值](https://wangdoc.com/javascript/types/number.html)

#### 对象
对象（object）是 JavaScript 语言的核心概念，也是最重要的数据类型。

什么是对象？简单说，对象就是一组“键值对”（key-value）的集合，是一种无序的复合数据集合。

##### 属性的删除
`delete` 命令用于删除对象的属性，删除成功后返回 `true`。

```
var obj = { p: 1 };
Object.keys(obj) // ["p"]

delete obj.p // true
obj.p // undefined
Object.keys(obj) // []

// 注意，删除一个不存在的属性，delete不报错，而且返回true。(此处，上一步已经删除 p 属性，再删除返回 true)
delete obj.p // true
```

##### 属性是否存在：in 运算符
`in` 运算符用于检查对象是否包含某个属性（注意，检查的是键名，不是键值），如果包含就返回 true，否则返回 false。

`in` 运算符的一个问题是，它不能识别哪些属性是对象自身的，哪些属性是继承的。

##### 属性的遍历
`for...in` 循环用来遍历一个对象的全部属性。

* 它遍历的是对象所有可遍历（enumerable）的属性，会跳过不可遍历的属性
* 它不仅遍历对象自身的属性，还遍历继承的属性。

##### with 语句
`with` 语句格式如下

```
with (对象) {
  语句;
}
```
它的作用是操作同一个对象的多个属性时，提供一些书写的方便。注意，如果 with 区块内部有变量的赋值操作，必须是当前对象已经存在的属性，否则会创造一个当前作用域的全局变量。

```
var obj = {};
with (obj) {
  p1 = 4;
  p2 = 5;
}

obj.p1 // undefined
p1 // 4
```
这是因为 with 区块没有改变作用域，它的内部依然是当前作用域。这造成了with语句的一个很大的弊病，就是绑定对象不明确。这非常不利于代码的除错和模块化，编译器也无法对这段代码进行优化，只能留到运行时判断，这就拖慢了运行速度。因此，建议不要使用 with 语句，可以考虑用一个临时变量代替 with。

更多详情可参考：[对象](https://wangdoc.com/javascript/types/object.html)

#### 函数
JavaScript 语言将函数看作一种值，与其它值（数值、字符串、布尔值等等）地位相同。凡是可以使用值的地方，就能使用函数。比如，可以把函数赋值给变量和对象的属性，也可以当作参数传入其他函数，或者作为函数的结果返回。函数只是一个可以执行的值，此外并无特殊之处。

由于函数与其他数据类型地位平等，所以在 JavaScript 语言中又称函数为第一等公民。

##### 函数名的提升
JavaScript 引擎将函数名视同变量名，所以采用 function 命令声明函数时，整个函数会像变量声明一样，被提升到代码头部。所以，下面的代码不会报错。但是，如果采用赋值语句定义函数，由于“变量提升”，JavaScript 就会报错。

```
f();
function f() {}

g();
var g = function (){};
// TypeError: undefined is not a function
```

注意，如果像下面例子那样，采用 function 命令和 var 赋值语句声明同一个函数，由于存在函数提升，最后会采用 var 赋值语句的定义。

```
var f = function () {
  console.log('1');
}

function f() {
  console.log('2');
}

f() // 1
```

##### length 属性
函数的 length 属性返回函数预期传入的参数个数，即函数定义之中的参数个数。不管调用时输入了多少个参数，length 属性始终等于定义时参数个数。

```
function f(a, b) {}
f.length // 2
```

##### 立即调用的函数表达式（IIFE）
根据 JavaScript 的语法，圆括号 () 跟在函数名之后，表示调用该函数。参考：[立即调用的函数表达式](https://wangdoc.com/javascript/types/function.html#%E7%AB%8B%E5%8D%B3%E8%B0%83%E7%94%A8%E7%9A%84%E5%87%BD%E6%95%B0%E8%A1%A8%E8%BE%BE%E5%BC%8Fiife)

更多详情请参考：[函数](https://wangdoc.com/javascript/types/function.html)

#### 数组
数组（array）是按次序排列的一组值。每个值的位置都有编号（从 0 开始），整个数组用方括号表示。本质上，数组属于一种特殊的对象。typeof 运算符会返回数组的类型是 object。

##### 数组 length 属性
数组的 length 属性，返回数组的成员数量。length 属性的值总是比最大的那个整数键大 1。另外，这也表明数组是一种动态的数据结构，可以随时增减数组的成员。

```
var arr = [ 'a', 'b', 'c' ];
arr.length // 3

// 设置数组 length，可以达到删除效果
arr.length = 2;
arr // ["a", "b"]

// 清空数组的一个有效方法，就是将 length 属性设为 0。
arr.length = 0;
arr // []

// 如果人为设置 length 大于当前元素个数，则数组的成员数量会增加到这个值，新增的位置都是空位。
arr.length = 3;
arr[1] // undefined

// 由于数组本质上是一种对象，所以可以为数组添加属性，但是这不影响 length 属性的值
// 下面代码将数组的键分别设为字符串和小数，结果都不影响 length 属性。因为，length 属性的值就是等于最大的数字键加 1，而这个数组没有整数键，所以 length 属性保持为 0。
var a = [];
a['p'] = 'abc';
a.length // 0

a[2.1] = 'abc';
a.length // 0
```

##### in 运算符
检查某个键名是否存在的运算符in，适用于对象，也适用于数组。

```
// 由于键名都是字符串,数值会转成字符串
var arr = [ 'a', 'b', 'c'];
2 in arr  // true
'2' in arr // true
4 in arr // false

// 如果数组的某个位置是空位，in 运算符返回 false。
var arr = [];
arr[100] = 'a';

100 in arr // true
1 in arr // false
```
##### 数组的空位
当数组的某个位置是空元素，即两个逗号之间没有任何值，我们称该数组存在空位（hole）。

```
var a = [1, , 1];
a.length // 3

// 需要注意的是，如果最后一个元素后面有逗号，并不会产生空位。也就是说，有没有这个逗号，结果都是一样的。
var a = [1, 2, 3,];

a.length // 3
a // [1, 2, 3]

// 数组的空位是可以读取的，返回undefined。
var a = [, , ,];
a[1] // undefined

// 使用 delete 命令删除一个数组成员，会形成空位，并且不会影响 length 属性。
var a = [1, 2, 3];
delete a[2];

a[2] // undefined
a.length // 3
```
数组的某个位置是空位，与某个位置是 undefined，是不一样的。如果是空位，使用数组的 forEach 方法、for...in 结构、以及 Object.keys 方法进行遍历，空位都会被跳过。如果某个位置是 undefined，遍历的时候就不会被跳过。

##### 类似数组的对象
如果一个对象的所有键名都是正整数或零，并且有 length 属性，那么这个对象就很像数组，语法上称为“类似数组的对象”（array-like object）。但是，“类似数组的对象”并不是数组，因为它们不具备数组特有的方法。对象没有数组的 push 方法，使用该方法就会报错。

“类似数组的对象”的根本特征，就是具有 length 属性。只要有 length 属性，就可以认为这个对象类似于数组。但是有一个问题，这种length 属性不是动态值，不会随着成员的变化而变化。

```
var obj = {
  0: 'a',
  1: 'b',
  2: 'c',
  length: 3
};

obj[0] // 'a'
obj[1] // 'b'
obj.length // 3
obj.push('d') // TypeError: obj.push is not a function
```

#### 算术运算符

##### 加法运算符
加法运算符（`+`）是最常见的运算符，用来求两个数值的和。

```
1 + 1 // 2

// 布尔值相加或数值和布尔值相加，布尔值会自动转成数值进行相加
true + true // 2
1 + true // 2

// 比较特殊的是，如果是两个字符串相加，这时加法运算符会变成连接运算符，返回一个新的字符串，将两个原字符串连接在一起。
'a' + 'bc' // "abc"

// 如果一个运算子是字符串，另一个运算子是非字符串，这时非字符串会转成字符串，再连接在一起。
1 + 'a' // "1a"
false + 'a' // "falsea"

// 加法运算符是在运行时决定，到底是执行相加，还是执行连接。也就是说，运算子的不同，导致了不同的语法行为，这种现象称为“重载”（overload）
'3' + 4 + 5 // "345"
3 + 4 + '5' // "75"

// 除了加法运算符，其他算术运算符（比如减法、除法和乘法）都不会发生重载。它们的规则是：所有运算子一律转为数值，再进行相应的数学运算。
1 - '2' // -1
1 * '2' // 2
1 / '2' // 0.5

// 对象相加：如果运算子是对象，必须先转成原始类型的值，然后再相加。
var obj = { p: 1 };
obj + 2 // "[object Object]2"
```

##### 余数运算符
余数运算符（`%`）返回前一个运算子被后一个运算子除，所得的余数。

```
12 % 5 // 2

// 需要注意的是，运算结果的正负号由第一个运算子的正负号决定。
-1 % 2 // -1
1 % -2 // 1
```

##### 比较运算符

```
// 字符串按照字典顺序进行比较
'cat' > 'dog' // false
'cat' > 'catalog' // false
'cat' > 'Cat' // true

// 非字符串的比较 -- 如果两个运算子都是原始类型的值，则是先转成数值再比较。
5 > '4' // true
true > false // true
2 > true // true

// 非字符串的比较 -- 如果运算子是对象，会转为原始类型的值，再进行比较。
// 对象转换成原始类型的值，算法是先调用 valueOf 方法；如果返回的还是对象，再接着调用 toString 方法，
var x = [2];
x > '11' // true
// 等同于 [2].valueOf().toString() > '11'
// 即 '2' > '11'

x.valueOf = function () { return '1' };
x > '11' // false
// 等同于 [2].valueOf() > '11'
// 即 '1' > '11'

{ x: 2 } >= { x: 1 } // true
// 等同于 { x: 2 }.valueOf().toString() >= { x: 1 }.valueOf().toString()
// 即 '[object Object]' >= '[object Object]'


// 这里需要注意与 NaN 的比较。任何值（包括 NaN 本身）与 NaN 使用非相等运算符进行比较，返回的都是 false。
1 > NaN // false
1 <= NaN // false
'1' > NaN // false
'1' <= NaN // false
NaN > NaN // false
NaN <= NaN // false
```

##### 严格相等运算符
JavaScript 提供两种相等运算符：`==` 和` ===`。

简单说，它们的区别是相等运算符（`==`）比较两个值是否相等，严格相等运算符（`===`）比较它们是否为“同一个值”。如果两个值不是同一类型，严格相等运算符（`===`）直接返回 false，而相等运算符（`==`）会将它们转换成同一个类型，再用严格相等运算符进行比较。

```
// 如果两个值的类型不同，直接返回false。
1 === "1" // false
true === "true" // false

// 同一类型的原始类型的值（数值、字符串、布尔值）比较时，值相同就返回true，值不同就返回false。
1 === 0x1 // true

// 需要注意的是，NaN与任何值都不相等（包括自身）。另外，正0等于负0。
NaN === NaN  // false
+0 === -0 // true

// 两个复合类型（对象、数组、函数）的数据比较时，不是比较它们的值是否相等，而是比较它们是否指向同一个地址。
{} === {} // false
[] === [] // false
(function () {} === function () {}) // false

var v1 = {};
var v2 = v1;
v1 === v2 // true

// 注意，对于两个对象的比较，严格相等运算符比较的是地址，而大于或小于运算符比较的是值。
var obj1 = {};
var obj2 = {};

obj1 > obj2 // false
obj1 < obj2 // false
obj1 === obj2 // false

// undefined和null与自身严格相等。
undefined === undefined // true
null === null // true
```

##### 相等运算符
相等运算符用来比较相同类型的数据时，与严格相等运算符完全一样。

比较不同类型的数据时，相等运算符会先将数据进行类型转换，然后再用严格相等运算符比较。详情可参考：[相等运算符](https://wangdoc.com/javascript/operators/comparison.html#%E7%9B%B8%E7%AD%89%E8%BF%90%E7%AE%97%E7%AC%A6)


##### 布尔运算符

且运算符(`&&`) 往往用于多个表达式的求值。

它的运算规则是：如果第一个运算子的布尔值为 true，则返回第二个运算子的值（注意是值，不是布尔值）；如果第一个运算子的布尔值为 false，则直接返回第一个运算子的值，且不再对第二个运算子求值。这种跳过第二个运算子的机制，被称为`短路`。

且运算符可以多个连用，这时返回第一个布尔值为 false 的表达式的值。如果所有表达式的布尔值都为 true，则返回最后一个表达式的值。

```
't' && '' // ""
't' && 'f' // "f"
't' && (1 + 2) // 3
'' && 'f' // ""
'' && '' // ""

var x = 1;
(1 - 1) && ( x += 1) // 0
x // 1

true && 'foo' && '' && 4 && 'foo' && true
// ''

1 && 2 && 3
// 3
```

或运算符（`||`）也用于多个表达式的求值。它的运算规则是：如果第一个运算子的布尔值为 true，则返回第一个运算子的值，且不再对第二个运算子求值；如果第一个运算子的布尔值为 false，则返回第二个运算子的值。

或运算符可以多个连用，这时返回第一个布尔值为 true 的表达式的值。如果所有表达式都为 false，则返回最后一个表达式的值。

```
't' || '' // "t"
't' || 'f' // "t"
'' || 'f' // "f"
'' || '' // ""

false || 0 || '' || 4 || 'foo' || true
// 4

false || 0 || ''
// ''
```

更多运算符详情请参考：[运算符](https://wangdoc.com/javascript/operators/index.html)

#### 数据类型转换

强制转换主要指使用 Number()、String() 和 Boolean() 三个函数，手动将各种类型的值，分别转换成数字、字符串或者布尔值。

##### Number ()
Number 函数将字符串转为数值，要比 parseInt 函数严格很多。基本上，只要有一个字符无法转成数值，整个字符串就会被转为 NaN。

```
// 数值：转换后还是原来的值
Number(324) // 324

// 字符串：如果可以被解析为数值，则转换为相应的数值
Number('324') // 324

// 字符串：如果不可以被解析为数值，返回 NaN
Number('324abc') // NaN

// 空字符串转为0
Number('') // 0

// 布尔值：true 转成 1，false 转成 0
Number(true) // 1
Number(false) // 0

// undefined：转成 NaN
Number(undefined) // NaN

// null：转成0
Number(null) // 0

// 另外，parseInt 和 Number 函数都会自动过滤一个字符串前导和后缀的空格。
parseInt('\t\v\r12.34\n') // 12
Number('\t\v\r12.34\n') // 12.34

Number({a: 1}) // NaN
Number([1, 2, 3]) // NaN
Number([5]) // 5
```
Number 背后的转换规则比较复杂。

第一步，调用对象自身的 valueOf 方法。如果返回原始类型的值，则直接对该值使用 Number 函数，不再进行后续步骤。

第二步，如果 valueOf 方法返回的还是对象，则改为调用对象自身的 toString 方法。如果 toString 方法返回原始类型的值，则对该值使用 Number 函数，不再进行后续步骤。

第三步，如果 toString 方法返回的是对象，就报错。

##### Boolean()
Boolean() 函数可以将任意类型的值转为布尔值。除了以下五个值的转换结果为 false，其他的值全部为 true。

* `undefined`
* `null`
* `0`（包含`-0`和`+0`）
* `NaN`
* `''`（空字符串）

```
Boolean(undefined) // false
Boolean(null) // false
Boolean(0) // false
Boolean(NaN) // false
Boolean('') // false

Boolean(true) // true
Boolean(false) // false

Boolean({}) // true
Boolean([]) // true
Boolean(new Boolean(false)) // true
```
更多详情请参考：[数据类型转换](https://wangdoc.com/javascript/features/conversion.html)


#### 语法

分号表示一条语句的结束。JavaScript 允许省略行尾的分号。以下三种情况，语法规定不需要在结尾添加分号，如果添加也不会出错，因为解释引擎会把这个分号解释为空语句。

* for 和 while 循环
* 分支语句：if，switch，try
* 函数声明语句

除了上面三种情况，所有语句都应该使用分号。同时，对于大多数情况，JavaScrip 会自动添加分号。这种语法特性被称为 “分号的自动添加”(`Automatic Semicolon Insertion，简称 ASI`)。由于解释引擎自动添加分号的行为难以预测，因此编写代码的时候不应该省略行尾的分号。

详情参考：[行尾的分号](https://wangdoc.com/javascript/features/style.html#%E8%A1%8C%E5%B0%BE%E7%9A%84%E5%88%86%E5%8F%B7)


#### 标准库对象及方法

##### Array 对象

数组构造函数的行为不统一：[Array 构造函数](https://wangdoc.com/javascript/stdlib/array.html#%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0)

Array 的一些操作符，哪些改变原数组，哪些不改变原数组 [array](https://wangdoc.com/javascript/stdlib/array.html)

| 方法 | 作用 | 是否改变原数组 |
|----|----|----|
| valueOf() | 对该对象求值，不同对象的实现不一致，数组的valueOf方法返回数组本身 |否|
| toString() | 是对象的通用方法，数组的toString方法返回数组的字符串形式 |否|
| push() | 用于在数组的末端添加一个或多个元素，并返回添加新元素后的数组长度 |是|
| pop() | 用于删除数组的最后一个元素，并返回该元素 |是|
| shift() | 用于删除数组的第一个元素，并返回该元素 |是|
| unshift() | 用于在数组的第一个位置添加元素，并返回添加新元素后的数组长度 |是|
| join() | 以指定参数作为分隔符，将所有数组成员连接为一个字符串返回。如果不提供参数，默认用逗号分隔 | 否 |
| concat() | 用于多个数组的合并。它将新数组的成员，添加到原数组成员的后部，然后返回一个新数组 | 否 |
| reverse() | 用于颠倒排列数组元素，返回改变后的数组 | 是 |
| slice()  | 用于提取目标数组的一部分，返回一个新数组 | 否 |
| splice()  | 用于删除原数组的一部分成员，并可以在删除的位置添加新的数组成员，返回值是被删除的元素 | 是|
| sort()  | 对数组成员进行排序，默认是按照字典顺序排序 |是|
| map() | 将数组的所有成员依次传入参数函数，然后把每一次的执行结果组成一个新数组返回 |否|
| forEach() | 对数组的所有成员依次执行参数函数，但是不返回值，只用来操作数据 |否|
| filter() | 用于过滤数组成员，满足条件的成员组成一个新数组返回 |否|
| some()  | 类似“断言”（assert），返回一个布尔值，表示判断数组成员是否符合某种条件。只要一个成员的返回值是true，则整个some方法的返回值就是true，否则返回false |否|
| every() | 类似“断言”（assert），返回一个布尔值，表示判断数组成员是否符合某种条件; 所有成员的返回值都是true，整个every方法才返回true，否则返回false |否|
| reduce()  |依次处理数组的每个成员，最终累计为一个值。从左到右处理（从第一个成员到最后一个成员） |否|
| reduceRight()  | 依次处理数组的每个成员，最终累计为一个值。从右到左（从最后一个成员到第一个成员）|否|
| indexOf()  | 返回给定元素在数组中第一次出现的位置，如果没有出现则返回-1 |否|
| lastIndexOf()  | 返回给定元素在数组中最后一次出现的位置，如果没有出现则返回-1 |否|

##### Number 对象

Number 对象的实例方法，数值需要放在括号里，不然会被 JavaScript 引擎解释成小数点，从而报错。
只要能够让 JavaScript 引擎不混淆小数点和对象的点运算符，各种写法都能用。除了为数值加上括号，还可以在数值后面加两个点，JavaScript 会把第一个点理解成小数点，把第二个点理解成调用对象属性，从而得到正确结果

[toFixed()](https://wangdoc.com/javascript/stdlib/number.html#numberprototypetofixed)
toFixed() 方法先将一个数转为指定位数的小数，然后返回这个小数对应的字符串。由于浮点数的原因，小数 5 的四舍五入是不确定的，使用的时候必须小心。

##### String 对象

String 实例方法，substring 违反直觉，建议使用 slice 方法。[substring](https://wangdoc.com/javascript/stdlib/string.html#stringprototypesubstring)


### 总结

本文简单说明了动态类型语言、静态类型语言的区别，归纳总结了 JavaScript 学习过程中容易混淆以及出错的点，尤其是数据类型和运算符这块。对于初学 JavaScript 语言者，希望可以起到一定的帮助。

### 参考资料
1. [编译型语言、解释型语言、静态类型语言、动态类型语言概念与区别 ](https://www.cnblogs.com/zy1987/p/3784753.html?utm_source=tuicool&utm_medium=referral)
2. [JavaScript 教程](https://wangdoc.com/javascript/index.html)
