---
layout: post
title:  "kotlin basic types"
date:   2020-05-16 10:18:50 +0800
categories: language
---

## Basic Types
在Kotlin中，所有的事物都在一定意义上都可以看作是对象，所以我们可以对任何变量调用成员函数和获取属性。但是一些类型会有特殊的内部表示，比如numbers、characters以及booleans在运行时以原始值（Primitive Value）表示（而不是类似于Java中的原始值包装器），但是在用户看来这些类型就是普通的类。

### Numbers
Kotlin提供一系列内置类型来表示数字。
对于整数：Byte(8bit)、Short(16bit)、Int(32bit)、Long(64bit)。

```kotlin
/*
  所有没有超过Int最大值数值字面量赋值给变量，
  变量被推断为Int类型。
  如果数值字面量超过Int最大，则推断为Long类型。
  要指定使用Long类型，数值字面量使用L为后缀
*/
val one = 1 // Int
val oneLong = 1L // Long
```

对于浮点数：Float(32bit)、Double(64bit).
根据IEEE 754标准，浮点数类型通过小数点位置区分，即可以存储的小数位数。// TODO：浮点数在bit级别上的表示

```kotlin
/*
  浮点数字面量默认为Double类型，
  除非使用F后缀指定为Float类型。
*/
val pi = 3.14 // Double
val e = 2.7182818284 // Double
val eFloat = 2.7182818284F // Float
```
不同于其它语言，Kotlin对于数值类型没有隐式转换。例如，一个接收Double类型作为参数的函数无法传入Float、Int或其他数值类型作为参数。
#### Literal Numbers（数值字面量）
支持十进制数、十六进制数、二进制数。
- Hexadecimals
  以0x开头，如0x0F表示十进制数15
- Binaries
  以0b开头，如0b00001011表示十进制数11
不支持八进制数值字面量。
#### Representation
在Java平台上，数值以JVM原始类型(primitive type)被存储，除非我们需要可空的数值型引用(Int?)(unless we need a nullable number reference)或者涉及到泛型(genric)。

```kotlin
val a: Int = 1000
println(a == a) // prints 'true'
val boxedA: Int? = a // Int?类型会自动包装
val anotherBoxedA: Int? = a
println(boxedA === anotherBoxedA) // prints 'false'，精确比较。===
println(boxedA == anotherBoxedA) // pirnts 'true'，自动解包比较数值(在类型相同的情况下，类型不同即使数值相同结果为false)。==
```
#### Explitct conversions
原则：数值小类型并不会**自动**转换为数值大类型(smaller types are **NOT** implicitly converted to bigger types)。
所以无法将Byte类型的数值在没有明确的转换下直接赋值给Int类型数值。

```kotlin
val b: Byte = 1 // OK, 字面值已经过静态检查
val i: Int = b // ERROR
val ii: Int = b.toInt() // OK: 明确指明类型拓展进行转换
val l = 1L + 3 // Long + Int => Long 数值字面量的隐式转换
```
所有数值类型都支持以下转换：
- toByte(): Byte
- toShort(): Short
- toInt(): Int
- toLong(): Long
- toFloat(): Float
- toDouble(): Double
- toChar(): Char
#### Operations
kotlin支持标准数值操作符(+ - * / %)，

### Chracters
字符通过Char类型表示。他们不能被直接当作数值(虽然在C语言中可以)。
```kotlin
fun check(c: Char) {
  if (c == 1) { // ERROR: 类型不匹配
    // ...
  }
}
```
字符字面量使用单引号表示，如'1'。特殊字符使用反引号(backslash)表示。支持以下类型转译：\t, \b, \n, \r, \', \", \\\\, \$。编码其它字符，使用Unicode转译序列语法，如'\uFF00'。

可以明确的将单个字符转成Int数值。
```kotlin
fun decimalDigitValue(c: Char): Int{
  if (c !in '0'..'9') {
    throw IllegalArgumentException("out of range)
  }
  return c.toInt() - '0'.toInt() // Explicit conversions to number
}
```
与数值类型相似，如果使用可空引用，Char类型会自动被封包。Identity is not preserved by the boxing operation.
### Booleans
类型Boolean用来表示布尔值，有两个值：true和false。
Booleans are boxed if a nullable reference is needed.
### Arrays
Kotlin中的数组使用Array类呈现，该类有get、set函数、size属性等。
为了创建数组，我们可以使用库函数arrayOf()并且给它传递值，所以arrayOf(1, 2, 3)创建数组[1, 2, 3]。另外，arrayOfNulls()库函数可以用来创建指定大小的元素全部为null的数组。
创建数组另外的选项是使用Array构造器，它接收数组大小以及用来给相应index的元素初始化值的函数。如，
```kotlin
// create an Array<String> with values ["0", "1", "4", "9", "16"]
val asc = Array(5) {i -> (i * i).toString()} 
```
正如之前提到的，[]操作符代表调用Array类的get()和set()成员函数。
Kotlin数组是不可变的(invariant)，这意味着kotlin不允许我们将类型为Array<String>的数组赋值给Array<Any>，这阻止了运行时可能出现的错误。
#### 原始类型数组(primitive type arrays)
Kotlin同样有特殊的类来表达原始类型数组而没有装箱的开销(without boxing overhead)：ByteArray, Shrotarray, IntArray等等。这些类与Array类并没有继承关系，但是他们有完全相同的方法和属性。
```kotlin
val x: IntArray = intArrayOf(1, 2, 3)
x[0] = x[1] + x[2]

// Array of int of size 5 with values [0, 0, 0, 0, 0]
val arr = IntArray[5]

// e.g. initialize the value in the array with constant
// Array of int of size 5 with values [42, 42, 42, 42, 42]
val arr = IntArray(5) { 42 }


// 
```
### Strings
使用String类来表达字符串类型。字符串不可更改。字符串的元素是Character可以通过索引操作(indexing operator)获得：s[i]。字符串可以在for-loop中进行迭代。
```kotlin
for (c in str) {
  println(c)
}
```
可以使用+操作符进行字符串链接。同样可以将字符串与其他类型的值相连接，只要连接的第一个元素是字符串。
```kotlin
val s = "abc" + 1 // "abc1"
println(s + "def") // "abc1def"
```
注意在大多数情况下使用字符串模板或者原始字符串会更好，对比于使用字符串连接。
#### String literals字符串字面量
Kotlin有两种类型的字符串字面量：escaped string & raw string
```kotlin
val text = """
    |Hello,
    |World.
    """.trimMargin()
println(text)
// Hello, 
// World.
```
#### 字符串模板
类似于Javascript ES6字符串模板。
```kotlin
val i = 10
println("i=$i") // "i=10"
val s = "abc"
println("$s.length is ${s.length}") // "abc.length is 3"
```
字符串模板支持包括eacaped string & raw string。如果需要表示$字符，可以这样,
```kotlin
val s = "${'$'}9.99" // "$9.99"
```

## Packages
源文件以包声明开头。
```kotlin
package org.example

fun printMessage() { /*...*/ }
class Message { /*..*/ }

// ...
```
源文件的所有内容(比如上面的class和函数)被这个包声明持有。所以在上面的例子中，printMessage()的全称为org.example.printMessage，Message类的全称为org.example.Message。
如果没有指定包，文件内容归属于没有名字的默认包。
### Default Imports
许多包在每个kotlin文件中被默认导入。
- kotlin.*
- kotlin.annotation.*
- ...

### Imports 
除了默认导入(import)，每个文件可以拥有自己的导入指令。

```kotlin
import org.example.Message // Message is now accessible without qualification.
import org.example.* // every thing in "org.example.*" is now accessible.
import org.test.Message as testMessage // testMessage stands for 'org.test.Message'
```
import关键字不局限于导入类，可以用它来导入其它声明：
- 文件顶层的函数以及属性(top-level functions and properties)
- 在对象里声明的函数以及属性(function and properties declared in object declaration)
- enum constants
### Visibility of Top-level Declarations
If a top-level declaration is marked private, it is private to the file it's declared in

## Control Flow: if, when, for, while
### If Expression
在Kotlin中，if是一个表达式(expression)，它返回一个值。因此没有类似于C语言的三元操作符（condition? then : else），因为在这种情况下if能够充当这种角色。
```kotlin
// Traditional usage
var max = a
if (a < b) max = b

// with else
var max: Int
if (a > b) {
  max = a
} else {
  max = b
}

// As Expression
val max = if (a > b) a else b
```
如果if分支可以是代码块，这时候代码块最后一个表达式的值就是if表达式的值。
```kotlin
val max = if (a > b) {
  println("choose a")
  a
} else {
  println("choose b")
  b
}
```
如果你将if用作为表达式(expression)而不是语句(statement)，那么表达式需要有else分支。
### When Expression
when语句取代类C语言的switch操作符。
```
when (x) {
  1 -> print("x == 1")
  2 -> print("x == 2")
  else -> { 
      // Note the block
      print("x is neither 1 nor 2")
  }
}
```
when按顺序匹配所有分支，直到满足某个分支的条件，执行这个分支的代码。
when可以被用作表达式或者语句。如果它被用做表达式，符合条件的分支的值会被作为整个表达式的值。If it is used as a statement, the values of individual branches are ignored.(就像if那样，每个分支可以是代码块，其值是代码块最后一个表达式的值)
当其它分支的条件都不能被满足时，else分支会被执行。当when被用作表达式，else分支是必需的，除非编译器可以证明所有可能的情况完全被分支的条件满足。
如果多种条件应该以相同的方式处理，这些条件应当通过逗号进行组合。
```kotlin
when (x) {
  1, 2 -> print("x == 1 or x == 2")
  else -> print("otherwise")
}
```
可以使用任意表达式作为分支条件（不仅是常量）。
```kotlin
when (x) {
  parseInt(s) -> print("s encodes x")
  else -> print("s does not encode x")
}
```
同样可以检查一个值是否属于一个范围或者集合。
```kotlin
when (x) {
  in 1..10 -> print("x is in the range")
  in validNumbers -> print("x is valid")
  !in 10..20 -> print("x is outside the range")
  else -> print("none of the above")
}
```
另外可以检查一个值 is or !is 一种特定的类型。值得一提的是，由于smart casts，你可以访问类型的方法或者属性而不需要额外检查。
```kotlin
fun hasPrefix(x: Any) = when(x) {
  is String -> x.startWith("prefix")
  else false
}
```
when也可以用来取代if-else-if链。如果when没有参数提供，分支条件为简单的bool表达式，当条件为true时分支代码将会被执行。
```kotlin
when {
  x.isOdd() -> print("x is odd")
  y.isEven() -> print("y is even")
  else -> print("x + y is even")
}
```
### For Loops
for循环对提供迭代器(iterator)的任何东西进行迭代(iterate)。相当于C#语言中的foreach循环。其语法如下：
```kotlin
for (item in collection) print(item)
```
body可以是代码块
```kotlin
for (item in collection) {
  print(item)
}
```
如何定义迭代器？// TODO
迭代数字使用range expression
```kotlin
for (i in 1..3) {
  println(i)
}
for (i in 6 downTo 0 setp 2) {
  println(i)
}
```
一个遍历range或者数组的for循环会被编译成基于index的循环，并不会创建iterator object。
遍历数组index
```kotlin
for (i in array.indices) {
  println(array[i])
}
``
另外，使用withIndex库函数
```kotlin
for ((index, value) in array.withIndex()) {
  println("the element at $index is $value")
}
### While Loops
while和do..while与Java并无二异。
### Break and continue in loops。
break和continue与Java并无二异。