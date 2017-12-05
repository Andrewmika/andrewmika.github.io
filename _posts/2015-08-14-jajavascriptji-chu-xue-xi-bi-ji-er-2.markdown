---
layout: post
title: JavaScript基础学习笔记（二）
date: '2015-08-14 04:08:05'
---

### 函数定义
#### 函数的声明

```
function functionName(parameters){
	code;
}
```

#### 函数表达式
JS函数可以通过一个表达式定义，函数表达式可以存储在变量中。当存储在变量中之后，变量也可以作为一个函数使用。函数表达式实际是一个匿名函数，是执行语句，已分号结尾。

```
var x = function(a,b) {return a * b};
var z = x(4,3);
```
#### Function()构造函数
函数可用JS构造器来定义。Function（）。实际上不必使用构造函数，JS中应避免使用`new`关键字。

```
var mayFunction = new Function("a","b","return a * b");
var x = myFunction(4,3);
```

#### 函数提升（Hoisting）
提升（Hoisting）是JS默认将当前作用域提升到前面去的行为。
so，函数可以在声明之前调用。

#### 自调用函数
函数表达式可以 "自调用"。

自调用表达式会自动调用。

如果表达式后面紧跟 () ，则会自动调用。

Y不能自调用声明的函数。

通过添加括号，来说明它是一个函数表达式：

```
(function () {
    var x = "Hello!!";      // 我将调用自己
})();
```

#### 函数是对象
函数可以作为一个值使用。

JS函数带有属性和方法。
`arguments.lenth`属性返回函数调用过程接收到的参数个数：
`function myFunction(a, b) {
    return arguments.length;
}`

`toString()`方法将函数作为一个字符串返回：`var text = myFunction.toString()`;


### 函数参数
JS函数对参数的值不进行任何检查。

#### 显式参数和隐式参数
函数显式参数在函数定义时列出，隐式参数在函数调用时传递给函数真正的值。

#### 默认参数
如果函数在调用时缺少参数，参数会默认设置为undefined（undefined为FALSE）。建议最好为参数设置一个默认值。

简单方式：
`function myFunction(x, y) {
    y = y || 0;
}`

#### Arguments对象
JS函数有个内置的arguments对象，包含了函数调用的参数数组。

```
function findMax() {
    var i, max = 0;
    for (i = 0; i < arguments.length; i++) {
        if (arguments[i] > max) {
            max = arguments[i];
        }
    }
    return max;
}
```
### 函数的调用
JS函数有4种调用方式，每种方式的不同在于this的初始化。

this指向函数执行时的当前对象。

#### 全局对象
当函数没有被自身的对象调用时，this的值就是全局对象。在 web 浏览器中全局对象是浏览器窗口（window 对象）。该实例返回 this 的值是 window 对象。使用window对象作为一个变量容易造成程序崩溃。

```
function myFunction() {
    return this;
}
myFunction();                // 返回 window 对象
```
#### 函数作为方法调用
用实例创建一个对象，对象带有属性和方法。

```
var myObject = {
    firstName:"John",
    lastName: "Doe",
    fullName: function () {
        return this.firstName + " " + this.lastName;
    }
}
myObject.fullName();         // 返回 "John Doe"
```

#### 使用构造函数调用函数
如果哈数调用前使用了new关键字，则是调用了构造函数。

```
function myFunction(arg1,arg2){
	this.firstName = arg1;
	this.lastName = arg2;
}
var x = myFunction("John","Doe");
x.firstName; // 返回"John"
```

#### 作为函数方法调用函数
JS中函数是对象，他有自己的属性和方法。all(),apply()是预定义的函数方法，两个方法可用于调用函数，两个方法的第一个参数必须是对象本身。
```
function myFunction(a, b) {
    return a * b;
}
myFunction.call(myObject, 10, 2);      // 返回 20
// myArray = [10,2];
// myFunction.apply(myObject, myArray);   // 返回 20
```

### JS闭包
JS私有变量可以用到闭包。
变量声明如果不使用var关键字，那它就是一个全局变量。

自我调用函数，闭包：

```
var add = (function () {
    var counter = 0;
    return function () {return counter += 1;}
})();

add();
add();
add();

// 计数器为 3
```

变量add指定了函数自我调用的返回字值。

自我调用函数只执行一次。设置计数器为0。并返回函数表达式。

add变量可以作为一个函数使用。它可以访问函数上一层作用域的计数器。

这个叫做JS闭包。它使得函数拥有私有变量变成可能。计数器受匿名函数的作用域保护，只能通过add方法修改。

闭包可以访问上一层作用域里变量的函数，即时上一层函数已经关闭。

### HTML DOM
当网页被加载时，浏览器会创建页面的文档对象模型（Document Object Model）。

通过可编程对象模型，JavaScript获得了足够的能力来创建动态的HTML。

JavaScript 能够改变页面中的所有 HTML 元素

JavaScript 能够改变页面中的所有 HTML 属性

JavaScript 能够改变页面中的所有 CSS 样式

JavaScript 能够对页面中的所有事件做出反应

#### 查找HTML元素

通过ID查找：`var x = document.getElementById("intro");`

通过标签名查找:`var x = document.getElementById("main"); var y = x.getElementsByTagName("p") // 数组`

通过类名查找:`var x = document.getElementsByClassName("intro") // 数组`

#### 改变HTML
- 改变输出流：`document.write()`。绝对不要在文档加载完成后使用`document.write()`，会覆盖该文档。
- 改变HTML内容：`document.getElementById(id).innerHTML = new value`
- 改变HTML属性:`document.getElementById(id).attribute = new value`

#### 改变CSS
`document.getElementById(id).style.property = new style`

### DOM 事件

#### 对事件做出反应
如需在用户点击某个元素时执行代码，则添加JS代码：`onclick = JavaScript`

#### HTML DOM分配事件
分配onclick事件，eg:`<script>document.getElementById("myBtn").onclick=function(){displayDate()};</script>`

一些事件：

- onload,onunload
- onchange
- onmouseover,onmouseout
- onmousedown,onmouseup,onclick
- onfocus

### HTML DOM EventListener
#### addEventListener()
`element.addEventListener(event,function,userCapture)`

- event：事件类型，不要使用"on"前缀。
- function：事件触发后调用的函数。
- userCapture：布尔值，用于描述事件是冒泡还是捕获，默认为false，冒泡传递。可选。

addEventListener()允许向同个元素添加多个事件，且不会覆盖已存在的事件。

#### 事件冒泡，事件捕获
冒泡：内部元素的事件先被触发，再触发外部元素事件。

捕获：外部元素事件先被触发，再触发内部元素的事件。

#### removeEventListener()方法
用于移除由addEventListener()方法添加的事件句柄。

### DOM元素

#### 创建新的HTML元素（节点）
如需向HTML DOM添加新元素，必须首先创建该元素（元素节点），然后向一个已存在的元素追加该元素。

```
	<div id="div1">
	<p id="p1">This is a paragraph.</p>
	<p id="p2">This is another paragraph.</p>
	</div>
	<script>
	var para=document.createElement("p");
	var node=document.createTextNode("This is new.");
	para.appendChild(node);
	var element=document.getElementById("div1");
	element.appendChild(para);
	</script>
```

#### 删除已有HTML元素
```
	<div id="div1">
	<p id="p1">This is a paragraph.</p>
	<p id="p2">This is another paragraph.</p>
	</div>
	<script>
	var parent=document.getElementById("div1");
	var child=document.getElementById("p1");
	parent.removeChild(child);
	</script>
```

### JavaScript对象
JavaScript中的所有事务都是对象，并且允许自定义对象。对象只是带有属性和方法的特殊数据类型。

#### JavaScript类
JavaScript是面向对象的语言，但JavaScript不使用类。

在JavaScript中，不会创建类，也不会通过类来创建对象。JavaScript是基于prototype的，而不是基于类的。

### Number
属性：

- MAX_VALUE
- MIN_VALUE
- NEGATIVE_INFINITY // 服务穷大
- POSITIVE_INFINITY // 正无穷大
- NaN	// 非数字值
- prototype
- constructor

方法：

- toExponential()
- toFixed()
- toPrecision()
- toString()
- valueOf()

### String
属性：

- length
- prototype
- constructor

方法：

- charAt()
- charCodeAt()
- concat()
- fromCharCode()
- indexOf()		// 定位字符串中某个指定的字符首次出现的位置。
- lastIndexOf()		// 在字符串末尾开始查找字符串出现的位置。
- match()	// 内容匹配，如果找到则返回这个字符。
- replace()
- search()
- slice()
- split()	// 转为数组
- substr()
- substring()
- toLowerCase()
- toUpperCase()
- valueOf()

### Date
方法：

- Date()	// 获得当前日期
- getFullYear()	// 使用 getFullYear() 获取年份。
- getTime() 	// getTime() 返回从 1970 年 1 月 1 日至今的毫秒数。
- setFullYear()	// 使用 setFullYear() 设置具体的日期。
- toUTCString()	// 使用 toUTCString() 将当日的日期（根据 UTC）转换为字符串。
- getDay()	// 使用 getDay() 和数组来显示星期，而不仅仅是数字。

### Array

- 合并多个数组 - concat()
- 用数组的元素组成字符串 - join() 
- 删除数组的最后一个元素 - pop()
- 数组的末尾添加新的元素 - push()
- 将一个数组中的元素的顺序反转排序 - reverse()
- 删除数组的第一个元素 - shift()
- 从一个数组中选择元素 - slice()
- 数组排序（按字母顺序升序）- sort()
- 数字排序（按数字顺序升序）- sort()
- 数字排序（按数字顺序降序）- sort()
- 在数组的第2位置添加一个元素 - splice()
- 转换数组到字符串 -toString() 
- 在数组的开头添加新元素 - unshift()

### Boolean
如果布尔对象无初始值或者其值为:0,-0,null,"",false,undefined,NaN，那么对象的值为 false。否则，其值为 true（即使当自变量为字符串 "false" 时）！

### Math
`Math.round(2.5);`

- round()四舍五入
- random()0-1的随机数
- max()多个数中的最大数
- min()多个数中的最小数

### RegExp


### 浏览器BOM
所有浏览器都支持window对象，它表示浏览器窗口。