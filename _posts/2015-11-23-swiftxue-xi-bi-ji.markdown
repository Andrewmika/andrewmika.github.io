---
layout: post
title: Swift学习笔记(一)
date: '2015-11-23 09:31:00'
tags:
- ios
- xue-xi-bi-ji
- swift
---

Swift 对空格有要求。

## Swift数据类型
- Int 相当于NSInteger,根据不同平台有不同的长度。
- 无符号类型UInt,尽量不要使用UInt。
- 浮点数，Double(64位浮点数)，Float（32位浮点数）。
- 布尔值，Swift有一个基本的布尔（Boolean）类型，叫做Bool。有两个布尔常量true,和False。在OC中为YES,NO.
- 字符串，String,OC中对应为NSString。
- 字符，Character,即单个字母。OC中对应为Char。
- 可选类型，Optional,用来处理值可能缺失的情况。表示有值或没有值

### 类型别名
typealias,对当前类型定义另一个名字。`typealias newStr = String`

### 类型安全
type safe，Swift会在编译代码时进行类型检查（type checks）

### 类型推断
如果没有显示指定类型，Swift会使用类型推断（type inference）来选择合适的类型。浮点数会优先推断为Double。

## Swift可选类型
Swift使用 ？ 作为命名类型Optional的简写，Optional是一个含有两种情况的枚举，None和Some(T),用来表示可能或可能没有值。任何类型都可以声明或隐式转换为可选类型，声明时要确保给`？`操作符一个合适的范围。类型和？之间没有空格。以下两种方式等价。

```
var optionalInt: Int?
var optionalInt2: Optional<Int>
```

声明可选变量如未给定初始值，默认为nil。

### 强制解析
当使用`？`操作符声明时，如果确定可选类型包含值一个非`nil`值，可以在变量后面加上`!`来强制解析（forced unwrapping）。如果值为`nil`则会导致运行时错误。

### 自动解析
声明可选变量时，使用`!`替换`？`,这样在解析可选变量时就可以不用加上`!`，以实现自动解析。

### 可选绑定
使用可选绑定（optional binding）来判断可选类型是否包含值，如果包含就把值复制给一个临时常量或变量。eg:

```
// 可选绑定
if let myInt = optionalInt {
    print(myInt)
}

```

## Swift 常量

Swift常量使用关键字`let`声明，必须初始值。`let constantName = <initial value>`。

### 类型标注
声明一个常量或者变量时候可以选择加上类型标注（type annotation），用于说明常量或者变量中要存储的值的类型。使用方法为在常量或者变量后加上**冒号**和**空格**，然后加上类型名称。 `var constantName: <data type> = <optional initial value>`

### 输出
Swift中使用`print`替换了原来的`println`函数来输出常量和变量。在字符串中可以使用`\()`来插入常量和变量。

## Swift字面量
字面量：能够直接了当的指出自己的类型并为变量进行赋值的值，如整型字面量、浮点型字面量、字符串型字面量和布尔型字面量。

字符串型字面量的转义

<table>
   <tr><th>转义字符</th><th> 含义</th></tr>
   <tr><td>\0</td><td> 空字符</td></tr>
   <tr><td>\\ </td><td> 反斜线 \</td></tr>
   <tr><td>\b</td><td> 退格(BS) ，将当前位置移到前一列</td></tr>
   <tr><td>\f</td><td> 换页(FF)，将当前位置移到下页开头</td></tr>
   <tr><td>\n</td><td> 换行符</td></tr>
   <tr><td>\r</td><td> 回车符</td></tr>
   <tr><td>\t</td><td> 水平制表符</td></tr>
   <tr><td>\v</td><td> 垂直制表符</td></tr>
   <tr><td>\'</td><td> 单引号</td></tr>
   <tr><td>\"</td><td> 双引号</td></tr>
   <tr><td>\000</td><td> 1到3位八进制数所代表的任意字符</td></tr>
   <tr><td>\xhh...</td><td> 1到2位十六进制所代表的任意字符</td></tr>
   <tr><td></td></tr>
</table>

布尔型字面量默认类型为`Bool`，包含三个值：**true** , **false** , **nil**

## Swift运算符
运算符是一个符号，用户告诉编译器执行一个数学或逻辑运算。包含

* 算数运算符
* 比较运算符
* 逻辑运算符
* 位运算符
* 赋值运算符
* 区间运算符(闭区间运算符：`...`。 半开区间运算符:`..<`。)
* 其他运算符(一元加、一元减、三元运算符)

## Swift条件语句

<table>
   <tr><th>语句</th><th>描述</th></tr>
   <tr><td>if </td><td>语句</td></tr>
   <tr><td>if  语句</td><td>由一个布尔表达式和一个或多个执行语句组成。</td></tr>
   <tr><td>if...else语句</td><td> if 语句 后可以有可选的 else 语句, else 语句在布尔表达式为 false 时执行。可用`？：`代替</td></tr>
   <tr><td>if...else if...else 语句</td><td> if 后可以有可选的 else if...else 语句, else if...else 语句常用于多个条件判断。</td></tr>
   <tr><td>内嵌 if 语句</td><td> 你可以在 if 或 else if 中内嵌 if 或 else if 语句。</td></tr>
   <tr><td>switch 语句</td><td> switch 语句允许测试一个变量等于多个值时的情况。</td></tr>
</table>

## Swift循环
### 循环类型
<table>
   <tr><td>循环类型</td><td>描述</td></tr>
   <tr><td>for-in </td><td> 遍历一个集合里面的所有元素，例如由数字表示的区间、数组中的元素、字符串中的字符。</td></tr>
   <tr><td>for 循环</td><td> 用来重复执行一系列语句直到达成特定条件达成，一般通过在每次循环完成后增加计数器的值来实现。</td></tr>
   <tr><td>while 循环</td><td> 运行一系列语句，如果条件为true，会重复运行，直到条件变为false。</td></tr>
   <tr><td>**repeat...while 循环**</td><td> 类似 while 语句区别在于判断循环条件之前，先执行一次循环的代码块。类似于do...while。</td></tr>
</table>

### 循环控制语句
<table>
   <tr><th>控制语句</th><th> 描述</th></tr>
   <tr><td>continue 语句</td><td> 告诉一个循环体立刻停止本次循环迭代，重新开始下次循环迭代。</td></tr>
   <tr><td>break 语句</td><td>中断当前循环。</td></tr>
   <tr><td>**fallthrough 语句** </td><td>如果在一个case执行完后，继续执行下面的case，需要使用fallthrough(贯穿)关键字。</td></tr>
   <tr><td></td></tr>
</table>

## Swift字符串
Swift字符串类型为`String`。

### 字符串创建
```
var stringA = "I`m string" // 字面量创建
var stringB = String("I`m string too") // String实例化，不推荐
``` 

### 空字符串
创建空字符串：

```
var stringA = "" // 字面量创建
var stringB = String() // String实例化创建
```

`isEmpty`可用来判断字符串是否为空！

### 字符串连接
字符串可通过`+`来连接

### 字符串长度
字符串可通过`String.characters.count`属性来计算

### 字符串比较
字符串通过`==`来比较是否相等

### 字符串函数和运算符

<table>
   <tr><th>序号</th><th> 函数/运算符 & 描述</th></tr>
   <tr><td>1</td><td> isEmpty 判断字符串是否为空，返回布尔值</td></tr>
   <tr><td>2</td><td> hasPrefix(prefix: String) ，检查字符串是否拥有特定后缀</td></tr>
   <tr><td>3</td><td> hasSuffix(suffix: String) ，检查字符串是否拥有特定后缀。</td></tr>
   <tr><td>4</td><td> Int(String) ，转换字符串数字为整型。 实例: `let myString: String = "256" let myInt: Int? = Int(myString)`</td></tr>
   <tr><td>5</td><td> String.characters.count ,计算字符串的长度</td></tr>
   <tr><td>6</td><td>utf8 ,您可以通过遍历 String 的 utf8 属性来访问它的 UTF-8 编码</td></tr>
   <tr><td>7</td><td> utf16 ,您可以通过遍历 String 的 utf8 属性来访问它的 UTF-16 编码</td></tr>
   <tr><td>8</td><td> unicodeScalars 您可以通过遍历String值的unicodeScalars属性来访问它的 Unicode 标量编码.</td></tr>
   <tr><td>9</td><td> + 连接两个字符串，并返回一个新的字符串</td></tr>
   <tr><td>10</td><td> += 连接操作符两边的字符串并将新字符串赋值给左边的操作符变量</td></tr>
   <tr><td>11</td><td> == 判断两个字符串是否相等</td></tr>
   <tr><td>12</td><td> < 比较两个字符串，对两个字符串的字母逐一比较。</td></tr>
   <tr><td>13</td><td> != 比较两个字符串是否不相等。</td></tr>
</table>

## swift字符
Swift字符数据类型为`Character`.Swift无法创建空的Character类型的变量或常量。

遍历字符串中的字符

```
for ch in "I`m string".characters {
    print(ch)
}
```
### 字符串连接字符
可以使用String的`append(c: Character)`来连接字符

```
var char: Character = "A"
stringA.append(char)
print(stringA)
```
## Swift数组
Swift中数组的数据类型为`Array`,使用有序列表存储同一类型的多个值。Swift中的数组元素类型没有限制。

### 创建数组
```
var emptyArray = [someType]() // 空数组
var sizeArray = [someType](count: 3, repeatedValue: 0) // 大小为3，初始值为0的数组
var someArray = [1,2,3] // 直接创建

```

### 访问数组
根据数组的索引来访问数组的元素。`var someVar = someArray[index]`

使用`count`属性来计算数组元素个数

使用`isEmpty`属性判断数组是否为空，返回布尔值。

### 修改数组
* 通过索引来直接更改元素的值
* 通过`append()`方法或者赋值运算符`+=`在数组末尾添加元素。

### 遍历数组
* 使用for in 遍历
* 使用`enumerate()`方法进行数组遍历。

### 合并数组
使用`+`来合并两种相同类型的数组。新数组的类型会从两个数组的数据类型中推断出来。

## Swift字典
Swift字典用来存储无序的相同类型数据的集合。Swift中的Key和Value的类型没有限制。

### 创建字典
与数组相似。

```
var emptyDict = [keyType : valueType]() // 空字典
var someDict: [Int : Int] = [1:11, 2: 22] // 字典实例
```

### 访问字典
通过字典的索引Key来访问数组元素。`var someVar = someDict[key]`

使用只读属性`count`计算字典有多少键值对

使用只读属性`isEmpty`属性来判断字典是否为空，返回布尔值。

### 修改字典
* 通过下标key增加或修改字典。key存在则修改，key不存在则增加
* 通过`updateValue(value: Value, forKey key: Key) -> Value?`来增加或修改字典。规则同上。

### 移除Key-Value
* 通过设置下标key的Value为nil来移除。
* 通过`removeValueForKey(key: Key) -> Value?`来移除，若key存在则返回value值，反之返回nil.

### 遍历字典
* 使用for-in循环遍历
* 使用`enumerate()`进行遍历，返回字典索引和键值对

```
for (key , value) in someDict {
    print("\(key):\(value)")
}

for (index , value) in someDict.enumerate() {
    print("\(index):\(value)")
}

```

### 字典转换为数组
提取字典的键值，并转换为独立的数组。

```
let arrayKeys = [Int](someDict.keys)
let arrayValues = [Int](someDict.values)
```

-----
参考：

[Swift 教程 | 菜鸟教程](http://www.runoob.com/swift/swift-tutorial.html)