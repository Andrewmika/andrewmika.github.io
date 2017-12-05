---
layout: post
title: JavaScript基础学习笔记（一）
date: '2015-08-12 06:04:15'
tags:
- javascript
---

### JS用法
根据 <http://www.runoob.com/> 进行整理

#### 标签使用
`<script>code</script>`标识js的起始和结束，中间包含js代码；

#### 外部JS
JS脚本一般集中放置在`<head>`或`<body>`底部，便于管理。外部JS使用.js作为扩展名，并在`<script>`标签的属性中设置JS文件。eg：`<script src= "script.js"></script>`。

*外部脚本不能包含`<script>`标签*

### JS输出

####操作HTML元素
在HTML中的元素用"id"属性标明后，可在`<script>`中使用`document.getElementById(id)`方法来访问。eg:
`<script>
document.getElementById("demo").innerHTML = "段落已修改。";
</script>`

#### 写到HTML文档
可用`<document.write()>`将内容输出到文档中，如果在文档已加载完成后执行此代码，将覆盖原有HTML页面的内容。

#### 写到控制台
可用`<console.log()>`将内容写到控制台，用于调试。

### JS语法

#### JS字面量
字面量，常量，变量关系

```
var a；// a为变量
const int b = 10; // b 为常量，10为字面量
string str = "hello"; // str 为变量，hello为字面量
```
数字字面量（Number）：整数，小数，科学计数。`3.14，1001，123e5`；

字符串字面量（String）：使用单引号或双引号:`"Andrew",'Lars'`;

表达式字面量用于计算：`5 + 6，1 * 2`……；

数组（Array）字面量定义一个数组：`[40,100,1]`；

对象（Object）字面量定义一个对象：`{firstName:"Andrew",lastName:"Shen"}`;

函数（Function）字面量定义一个函数：`function myFunction(a,b){return a * b}`

#### 注释
使用`//`进行单行注释
使用 `/* code */` 进行多行注释

#### JS函数
JS语句可以卸载函数内，函数可以重复引用；引用一个函数=电泳函数

*JS对大小写敏感*

### JS语句

#### 语句标识符

<table border = 1>
<tr><th>语句</th><th>描述</th></tr>
<tr><td>break</td>
<td>用于跳出循环</td></tr>
<tr><td>catch</td>
<td>语句块，在 try 语句块执行出错时执行 catch 语句块。</td></tr>
<tr><td>continue</td>
<td>跳过循环中的一个迭代。</td></tr>
<tr><td>do</td>
<td>while | 执行一个语句块，在条件语句为 true 时继续执行该语句块。</td></tr>
<tr><td>for</td>
<td>在条件语句为 true 时，可以将代码块执行指定的次数。</td></tr>
<tr><td>for</td>
<td>in | 用于遍历数组或者对象的属性（对数组或者对象的属性进行循环操作）。</td></tr>
<tr><td>function</td>
<td>定义一个函数</td></tr>
<tr><td>if</td>
<td>else | 用于基于不同的条件来执行不同的动作。</td></tr>
<tr><td>return</td>
<td>退出函数</td></tr>
<tr><td>switch</td>
<td>用于基于不同的条件来执行不同的动作。</td></tr>
<tr><td>throw</td>
<td>抛出（生成）错误 。</td></tr>
<tr><td>try</td>
<td>实现错误处理，与 catch 一同使用。</td></tr>
<tr><td>var</td>
<td>声明一个变量。</td></tr>
<tr><td>while</td>
<td>当条件语句为 true 时，执行语句块。</td></tr>
</table>

####  对代码进行折行
在文本字符串中可使用`\`对代码进行换行。

### JS变量
变量是用于存储信息的*容器*

#### 变量规则
- 变量必须已字母开头。
- 变量能以$和_开头。
- 变量名称对大小写敏感。

####  变量的声明和创建
使用`var`来声明变量。声明时可以对其赋值。如果未声明直接创建则变量为全局变量：eg:`name = Andrew`,nama为全局变量.


### JS数据类型

- 字符串（String）
- 数字（Number）
- 布尔（Boolean）
- 数组（Array）
- 对象（Object）
- 空（Null）
- 未定义（undefined）

#### JS拥有动态类型！！
JS变量可用作不同的类型，厉害。

```
var x;               // x 为 undefined
var x = 5;           // 现在 x 为数字
var x = "John";      // 现在 x 为字符串
```

#### String
字符串可以是引号中的任意文本，可使用`''或""`；

#### Number
JS只有一种数据类型，Number全搞定。

#### Boolean
布尔只有两个值，true 和 false

#### Array
array创建:

```
var array = new Array();
array[0] = "Andrew";
array[1] = "Lars";
```

`var array = new Array("Andrew","Lars");`

`var array = ["Andrew","Lars"];`

#### Object
JS对象是拥有属性和方法的数据。
对象有花括号分隔，内部以键值对（name:value）的形式定义，属性由逗号分隔。
`var person = {firstName:"Andrew", lastName:"Shen"}`;

寻址方式：`name = person.lastName; 或 name = person["lastName"]`

方法创建：`methodName : funciton() {code}`

对象方法调用,通过添加（）调用 `objectName.methodName()`'

#### Undefined 和 Null
Undefined表示变量不含有值。

通过将变量的值设为null来清空变量

#### 声明变量类型,关键词"new"来声明
```
var carname=new String;
var x=      new Number;
var y=      new Boolean;
var cars=   new Array;
var person= new Object;
```

### JS函数

#### 函数的语法
```
function functionName(){
	code lines;
}
```
JS大小写敏感，function一定为小写。

#### 带参函数
声明：

```
function myFunction(var1,var2)
{
代码
}
```
调用：`myFunction(args1,args2)`

#### 带有返回值的函数
*直接使用**return**即可以实现*

```
function myFunction()
{
var x=5;
return x;
}
```

### JS作用域

#### 局部作用域
变量在函数内声明：局部变量。只能在函数内部访问。
局部变量在函数执行完毕后销毁。

#### 全局变量
变量在函数外定义，或者在函数内没有声明，则为全局变量。在网页中所有脚本和函数均可使用。
全局变量在页面关闭后销毁。

#### HTML中的全局变量
HTML中，全局变量是window对象，所有数据变量都属于window对象。

### JS事件

#### HTML事件
HTML元素可以添加事件属性，使用JS代码来添加HTML元素。
单引号：`<some-HTML-element some-event='some JavaScript'>`

双引号：`<some-HTML-element some-event="some JavaScript">`

eg:`<button onclick='this.innerHTML=Date()'>The time is?</button>`,*this*改变自身元素内容。

#### 常见HTML事件
事件  |  描述
-  |  -
onchange  |  HT元素改变
onclick | 用户点击HTML元素
onmouseover | 用户在一个HTML元素上移动鼠标
onmouseoout | 用户从一个HTML元素上移开鼠标
onkeydown | 用户按下键盘按键
onload | 浏览器已完成页面的加载

### JS字符串
用于存储和处理文本。不要创建String对象，会拖慢执行速度，可能产生其他副作用。

#### 特殊字符转义
代码 | 输出
- | ——
\' | 单引号
\" | 双引号
\\ | 反斜杠
\n | 换行
\r | 回车
\t | tab(制表符)
\b | 退格符
\f | 换页符

#### 字符串属性和方法
**属性**：

属性 | 描述
- | -
constructor | 返回创建字符串属性属性的函数
length | 返回字符串的长度
prototype | 允许您向对象添加属性和方法

**方法**

方法 | 描述
- | -
charAt() | 返回指定索引位置的字符
charCodeAt() | 返回指定索引位置字符的 Unicode 值
concat() | 连接两个或多个字符串，返回连接后的字符串
fromCharCode() | 将字符转换为 Unicode 值
indexOf() | 返回字符串中检索指定字符第一次出现的位置
lastIndexOf() | 返回字符串中检索指定字符最后一次出现的位置
localeCompare() | 用本地特定的顺序来比较两个字符串
match() | 找到一个或多个正则表达式的匹配
replace() | 替换与正则表达式匹配的子串
search() |  检索与正则表达式相匹配的值
slice() | 提取字符串的片断，并在新的字符串中返回被提取的部分
split() | 把字符串分割为子字符串数组
substr() | 从起始索引号提取字符串中指定数目的字符
substring() | 提取字符串中两个指定的索引号之间的字符
toLocaleLowerCase() | 根据主机的语言环境把字符串转换为小写，只有几种语言（如土耳其语）具有地方特有的大小写映射
toLocaleUpperCase() | 根据主机的语言环境把字符串转换为大写，只有几种语言（如土耳其语）具有地方特有的大小写映射
toLowerCase() | 把字符串转换为小写
toString() | 返回字符串对象值
toUpperCase() | 把字符串转换为大写
trim() | 移除字符串首尾空白
valueOf() | 返回某个字符串对象的原始值

### JS运算符
JS运算符与一般编程语言相似。

#### +运算符
**+**运算符用于把文本值或字符串变量加起来\连接起来。

可对字符串和数字进行加法运算：`z = "hello" + 5`,z = hello5

### 比较运算符，条件语句，循环语句
与一般编程语言相似。

### typeof操作符
检测变量的数据类型。

*用typeof检测null返回的是objet。null是一个只有一个值的特殊类型。标识一个空对象引用*

*typeof一个没有值的变量会返回undefined，undefined是一个没有设置值的变量*

### JS类型转换
Number()转换为数字，String()转换为字符串，Boolean()转换为布尔值。

5种数据类型：String，Number，Boolean, Object, Function.

3种对象类型：Object，Date，Array

2种不包含任何值的数据类型：Null,Undefined

#### 自动转换类型
当JS尝试操作一个『错误』的数据类型时，会自动转换为『正确』的数据类型。

当你尝试输出一个对象或变量时，JS会自动调用变量的toString()方法。

### JS正则表达式（regex\regexp\RE）
正则表达式是由一个字符序列形成的搜索模式。

当你在文本中搜索数据是，你可以用搜索模式来描述你要查询的内容。

正则表达式可以是一个简单的字符，或一个更复杂的模式。

正则表达式可用于所有文本搜索和文本替换的操作。

#### 语法
`/pattern/modifiers;`

eg:`var patt = /w3cschool/i`

####使用字符串方法
JS中，正则表达式常用于字符串方法：**search()**,**replace()**.

eg:

```
var str = "Visit w3cschool";
var n = str.search(/w3cschool/i);
```

```
var str = "Visit Microsoft!";
var res = str.replace(/microsoft/i, "w3cschool"); // 不区分大小写
```

#### 正则表达式修饰符
修饰符 可以在全局搜索中不区分大小写:

修饰符 |	描述
- | -
i |	执行对大小写不敏感的匹配。
g |	执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。
m |	执行多行匹配。

####正则表达式模式
方括号用于查找某个范围内的字符：

表达式 |	描述
- | -
[abc] |	查找方括号之间的任何字符。
[0-9] |	查找任何从 0 至 9 的数字。
(x|y) |	查找任何以 | 分隔的选项。

元字符是拥有特殊含义的字符：

元字符 |	描述
- | -
\d |	查找数字。
\s |	查找空白字符。
\b |	匹配单词边界。
\uxxxx |	查找以十六进制数 xxxx 规定的 Unicode 字符。

量词:

量词 |	描述
- | -
n+ |	匹配任何包含至少一个 n 的字符串。
n* |	匹配任何包含零个或多个 n 的字符串。
n? |	匹配任何包含零个或一个 n 的字符串。

#### test()
test()是一个正则表达式。

test()方法用于检测一个字符串是否匹配某个模式，如果字符串中含有匹配的文本，则返回true，否则返回false.

#### exec()
exec()方法是一个正则表达式。

exec() 方法用于检索字符串中的正则表达式的匹配。

该函数返回一个数组，其中存放匹配的结果。如果未找到匹配，则返回值为 null。

### JS表单验证

#### 必填项目

```
	<!DOCTYPE html>
	<html>
	<head>
	<script>
	function validateForm()
	{
	var x=document.forms["myForm"]["fname"].value;
	if (x==null || x=="")
	  {
	  alert("姓必须填写");
	  return false;
	  }
	}
	</script>
	</head>
	<body>
	<form name="myForm" action="demo-form.php" onsubmit="return validateForm()" method="post">
	姓: <input type="text" name="fname">
	<input type="submit" value="提交">
	</form>
	</body>
	</html>

```

#### Email验证

```
	<!DOCTYPE html>
	<html>
	<head>
	<script>
	function validateForm()
	{
	var x=document.forms["myForm"]["email"].value;
	var atpos=x.indexOf("@");
	var dotpos=x.lastIndexOf(".");
	if (atpos<1 || dotpos<atpos+2 || dotpos+2>=x.length)
	  {
	  alert("Not a valid e-mail address");
	  return false;
	  }
	}
	</script>
	</head>
	<body>
	<form name="myForm" action="demo-form.php" onsubmit="return validateForm();" method="post">
	Email: <input type="text" name="email">
	<input type="submit" value="Submit">
	</form>
	</body>
	</html>
```

### void

#### void(0)
void 是 JavaScript 中非常重要的关键字，该操作符指定要计算一个表达式但是不返回值。

void(0) 计算为 0，但 Javascript 上没有任何效果。

#### href="#"与href="javascript:void(0)"的区别
`#`包含了一个位置信息，默认的锚是#top 也就是网页的上端。

而javascript:void(0), 仅仅表示一个死链接。

在页面很长的时候会使用 # 来定位页面的具体位置，格式为：# + id。

如果你要定义一个死链接请使用 javascript:void(0) 。