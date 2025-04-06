# 便捷

## 可变数量实参

==vararg==

```kotlin
// 对于不确定函数会传入多少参数时，可以用vararg
fun main() {
    val arr = arrayOf("Jack","HuTao")
    documentTest(*arr)
}
fun documentTest(vararg str:String){
    for (s in str) {
        println(s)
    }
}

// *arr应该是arr的指针吧
```



# 基础

## 包定义与导入

包的声明应处于源文件顶部

```kotlin
package my.demo
import kotlin.text.*
```

## 程序入口点

```kotlin
fun main() {
	println("Hello world!")
}

fun main(args: Array<String>) {
	println(args.contentToString())
}
```



## 函数

```kotlin
// 函数定义
fun function(variable : Int) : Unit{
    
}
```



## 变量

```kotlin
// 只读变量,指明类型
val readOnly:Int = 5

// 只读变量，自动推断出类型
val readOnly = 5

// 声明变量
val readOnly : Int
// 变量赋值
readOnly = 6

// 可重新赋值变量
var a = 6
```



## 类

### 使用类

```kotlin
// 类的属性可以在其声明或主体中列出
class Rectangle(var height: Double, var length: Double) {
	var perimeter = (height + length) * 2
}

fun main() {
val rectangle = Rectangle(5.0, 2.0)
println("The perimeter is ${rectangle.perimeter}")
}
```

### 继承类

```kotlin
// final为不可继承类，默认为final，如需让类可继承，用open标记
// 类之间的继承使用冒号（:）声明
open class Shape
class Rectangle(var height: Double, var length: Double): Shape() {
	var perimeter = (height + length) * 2
}
```



## 字符串模板

```kotlin
val a = 5
println("a = ${a}")
```



## 条件表达式

```kotlin
fun maxOf(a: Int, b: Int): Int {
	if (a > b) {
		return a
	} else {
		return b
	}
}


fun maxOf(a: Int, b: Int) = if (a > b) a else b
```



## when

类似switch

```kotlin
fun describe(obj: Any): String =
	when (obj) {
		1 -> "One"
		"Hello" -> "Greeting"
		is Long -> "Long"
		!is String -> "Not a string"
		else -> "Unknown"
	}

fun main() {
		println(describe(1))
		println(describe("Hello"))
		println(describe(1000L))
		println(describe(2))
		println(describe("other"))
}
```



## 区间

### 检测

使用 ==in== 操作符来检测某个数字是否在指定区间内。

```kotlin
fun main() {

		val x = 10
		val y = 9
    // 区间内
		if (x in 1..y+1) 
			println("fits in range")
    // 区间外
        if (x !in 1..y+1) 
			println("fits out of range")
	
}
```



### 区间迭代

```kotlin
// 不指定步长，默认为1，且顺序
for (x in 1..5) {
	print(x)
}

// 指定步长，且倒序
for (x in 9 downTo 0 step 3) {
	print(x)
}
```

## 集合

### 集合迭代

```kotlin
fun main() {
	val items = listOf("apple", "banana", "胡桃")

	for (item in items) {
		println(item)
	}

}
```

### in

```kotlin
// 使用 in 操作符来判断集合内是否包含某实例
val items = setOf("apple", "banana", "kiwifruit")

	when {
		"orange" in items -> println("juicy")
		"apple" in items -> println("apple is fine too")
}
```

### 过滤跟映射

```kotlin
// 使用 lambda 表达式来过滤（filter）与映射（map）集合
fun main(){
val fruits = listOf("banana", "avocado", "apple", "kiwifruit")
fruits
.filter { it.startsWith("a") }
.sortedBy { it }
.map { it.uppercase() }
.forEach { println(it) }
}
```



## 空值

### ?

```kotlin
// 当可能用 null 值时，必须将引用显式标记为可空。可空类型名称以问号（ ? ）结尾。
// 如果 str 的内容不是数字返回 null
fun parseInt(str: String): Int? {
	// ……
}

```



### 空检测

```kotlin
fun parseInt(str: String): Int? {
	return str.toIntOrNull()
}

fun printProduct(arg1: String, arg2: String) {
val x = parseInt(arg1)
val y = parseInt(arg2)
// 直接使用 `x * y` 会导致编译错误，因为它们可能为 null
if (x != null && y != null) {
// 在空检测后，x 与 y 会自动转换为非空值（non-nullable）
println(x * y)
}
else {
println("'$arg1' or '$arg2' is not a number")
}
}

fun main() {
printProduct("6", "7")
printProduct("a", "7")
printProduct("a", "b")
}

```



## 类型检测

==is== 操作符检测一个表达式是否某类型的一个实例

当一个变量或者表达式 已经判断出是什么类型 那么就会转换为什么类型

```kotlin
fun getStringLength(obj: Any): Int? {
		if (obj is String) {
// `obj` 在该条件分支内自动转换成 `String`
			return obj.length
		}
// 在离开类型检测分支后，`obj` 仍然是 `Any` 类型
		return null
}

```



# 习惯用法

## 检测元素是否在集合中

```kotlin
if ("john@example.com" in emailsList) { …… }
if ("jane@example.com" !in emailsList) { …… }
```



## 交换两个变量

```kotlin
var a = 1
var b = 2
a = b.also { b = a }
```

# 编码规范

## 类

实体类属性比较少的话，可以就写一行

```kotlin
class Person(id: Int, name: String)
```

属性比较多的话，就一行一个属性写

```kotlin
class Person(
	id: Int,
	name: String,
	surname: String
)
```

属性多，且继承了其他类、或者实现接口类的话：

```kotlin
class Person(
	id: Int,
	name: String,
	surname: String
) : Human(id, name),
	KotlinMaker { /*……*/ }
```

## 注解

无参注解可以放在同一行

```kotlin
@JsonExclude @JvmField
var x: String
```

## 尾部逗号

尾部逗号是一系列元素中最后一项之后的逗号符号：

```kotlin
class Person(
	val firstName: String,
	val lastName: String,
	val age: Int, // 尾部逗号
)
```

```kotlin
enum class Direction {
    NORTH,
    SOUTH,
    WEST,
    EAST,
}
```

使用尾部逗号的收益：

- 使版本控制差异更清晰——因为所有焦点都集中在变更的值上
- 使添加及重新排序元素更容易——操作元素时无需再添加或删除逗号
- 简化了代码生成逻辑（例如对象初始化器）。最后一个元素也可以有逗号

> 如需在 IntelliJ IDEA 格式化程序中启用尾部逗号，请转到 Settings/Preferences | Editor | Code Style | Kotlin， 打开 Other 选项卡并选择 Use trailing comma 选项



# 概念

## 类型

### 基本类型

#### 数字

##### 数字类型

```kotlin
// 整型
Byte,
Short,
Int,
Long,

// 浮点型
Float,
Double,

/*
字面常量

// Long类型
123L

// 十六进制
0x0F

// 二进制
0b0001011

// Float类型
123.5f 或者 123.5F

*/


// 下划线数字字面量
val a = 1_000_000
val b = 1000000
// a == b
```



##### 显示数字转换

```kotlin
// 假想的代码，实际上并不能编译：
val a: Int? = 1 // 一个装箱的 Int (java.lang.Integer)
val b: Long? = a // 隐式转换产生一个装箱的 Long (java.lang.Long)
print(b == a) // 惊！这将输出“false”鉴于 Long 的 equals() 会检测另一个是否也为 Long
```

所有数字类型都支持转换为其他类型：

- toByte(): Byte 
- toShort(): Short 
- toInt(): Int 
- toLong(): Long 
- toFloat(): Float 
- toDouble(): Double

```kotlin
// 示例，将Byte转换为Int
fun main() {

val b: Byte = 1 // OK, 字面值会静态检测

val a: Int = b.toInt()

}
```



#### 运算符



##### 位运算

只能应用于 ==Int== 与 ==Long==

```kotlin
val x = (1 shl 2) and 0x000FF000
```

位运算列表：

- shl(bits) – 有符号左移 
- shr(bits) – 有符号右移 
- ushr(bits) – 无符号右移 
- and(bits) – 位与 
- or(bits) – 位或 
- xor(bits) – 位异或 
- inv() – 位非



#### 布尔

==Boolean== 类型只有 ==true== 跟 ==false==

==Boolean?== 类型除了 ==true== 、==false==，还有 ==null==

```kotlin
fun main() {

	val myTrue: Boolean = true
	val myFalse: Boolean = false
	val boolNull: Boolean? = null
    
	println(myTrue || myFalse)
	println(myTrue && myFalse)
	println(!myTrue)

}
```



#### 字符（Char）

基本用法跟Java一致

如果字符变量的值是数字，那么可以使用 ==digitToInt()== 函数将其显式转换为 ==Int== 数字。

```kotlin
val a = '2'
val b : Int = a.digitToInt()
```



#### 字符串（String）

##### 普通字符串

可以使用索引访问字符串

```kotlin
fun main() {
	val str = "abcd"

	for (c in str) {
		println(c)
	}

}
```

字符串是不可变的。 一旦初始化了一个字符串，就不能改变它的值或者给它赋新值。==所有转换字符串的操作都以一个新的 String 对象来返回结果，而保持原始字符串不变==

```kotlin
fun main() {

	val str = "abcd"
	println(str.uppercase()) // 创建并输出一个新的 String 对象
    
	println(str) // 原始字符串保持不变

}
```



##### 字符串字面量

```kotlin
// 转义字符串，即可以包含转义字符
val s = "Hello, world!\n"

// 多行字符串，使用三引号，原原本本输出字符串
val text = """
|Tell me and I forget.
|Teach me and I remember.
|Involve me and I learn.
|(Benjamin Franklin)
""".trimMargin()

/*
如需删掉多行字符串中的前导空格，请使用 trimMargin() 函数
默认以竖线 | 作为边界前缀，但你可以选择其他字符并作为参数传入，比如 trimMargin(">") 。
*/

```



##### 字符串模板

```kotlin

val s = "abc"
println("$s.length is ${s.length}")

val price = """
${'$'}_9.99
"""
```



#### 数组（Array）

一般用集合

##### 创建数组

三种方式

```kotlin
// [1,4,8]
val arr1 = arrayOf(1,4,8)

// [null,null,null]，创建指定长度的空数组
val arr2 = arrayOfNulls(3)

// 
var exampleArray = emptyArray<String>()
```



##### 嵌套数组

```kotlin
fun main() {
//sampleStart
// Creates a two-dimensional array
val twoDArray = Array(2) { Array<Int>(2) { 0 } }
println(twoDArray.contentDeepToString())
// [[0, 0], [0, 0]]
// Creates a three-dimensional array
val threeDArray = Array(3) { Array(3) { Array<Int>(3) { 0 } } }
println(threeDArray.contentDeepToString())
// [[[0, 0, 0], [0, 0, 0], [0, 0, 0]], [[0, 0, 0], [0, 0, 0], [0, 0, 0]], [[0, 0,
0], [0, 0, 0], [0, 0, 0]]]
//sampleEnd
}
```



##### 访问与修改元素

```kotlin
val a = arrayOf(1,6,8)
a[1] = 3
println(a[1]) // 3
```



##### 比较数组

```kotlin
    val a = arrayOf(1,2,3)
    val b = arrayOf(1,2,3)

	// true
    println(a.contentEquals(b))
    println(a.contentDeepEquals(b))

    a[0] = 0
	// false
    println(a.contentEquals(b))
    println(a.contentDeepEquals(b))
```



> 两个函数的区别：
>
> 



##### 其他函数

**Sum**

用于计算数组的总和，只能用在数字类型

```kotlin
fun main() {
    val a = arrayOf(1,5,4,3)
    println(a.sum())
    // 13
}
```



**Shuffle**

对数组重新排序，类似洗牌

```kotlin
fun main() {
    val a = arrayOf(1,5,4,3,21,34,7)
    for( x in 1..3){
        a.shuffle()
        println(a.joinToString())
    }

    /*
    21, 7, 34, 4, 5, 1, 3
    3, 7, 21, 34, 5, 4, 1
    21, 1, 4, 34, 5, 7, 3
     */
}

```



##### 数组转集合

转成 List 或 Set

```kotlin
fun main() {
    val a = arrayOf(1,5,4,3,21,34,7)
    
    val toList = a.toList()
    val toSet = a.toSet()

}
```



转成 Map

```kotlin
// 只有键值对的数组才能转Map
fun main() {
    val a = arrayOf("HuTao" to "胡桃","XiAo" to "魈")
    val map = a.toMap()
    for (entry in map) {
        println("${entry.key} = ${entry.value} ")
    }
    /*
    HuTao = 胡桃 
    XiAo = 魈 
     */

}
```



#### 类型检测与类型转换

##### is & !is

```kotlin
fun main() {
    // 这里其实是智能转换
    val obj:Any = "123"
    if (obj is String) {
        print(obj.length)
    }
    if (obj !is String) { // 与 !(obj is String) 相同
        print("Not a String")
    } else {
        print(obj.length)
    }
}
```



##### 用于when表达式

```kotlin
when (x) {
        is Int -> print(x + 1)
        is String -> print(x.length + 1)
        is IntArray -> print(x.sum())
    }
```

##### 智能转换适用场景

![[val和var的使用场景.png]]



### 流程控制



#### 条件与循环

##### if

在 Kotlin 中， ==if== 是一个表达式：它会返回一个值，它可以当作三元运算符使用

```kotlin
fun main() {
    val num = readln().toInt()
    val str = d(num)
    println(str)

}
fun d(num:Int) =
    if(num <18)
    "未满十八岁"
else
    "已经成年"
```



##### when

可作为switch使用，但是比switch灵活，它可以将任何的表达式作为分支条件

```kotlin
when (x) {
	s.toInt() -> print("s encodes x")
	else -> print("s does not encode x")
}
```



还可以检测一个值在不在区间内

```kotlin
when (x) {
	in 1..10 -> print("x is in the range")
	in validNumbers -> print("x is valid")
	!in 10..20 -> print("x is outside the range")
	else -> print("none of the above")
}

```



或者是判断类型

```kotlin
fun hasPrefix(x: Any) = when(x) {
	is String -> x.startsWith("prefix")
	else -> false
}
```



##### for

**类似迭代器遍历**

```kotlin
fun main() {
    val str = "HuTao"
    for (c in str) {
        println(c)
    }
    /*
    H
    u
    T
    a
    o
     */

}
```

![[Pasted image 20240623170321.png]]

**区间遍历**

```kotlin
fun main() {
    for (x in 1..3)
        print("$x ")
    // 1 2 3
    
    for (x in 10 downTo 0 step 2)
        print("$x ")
    // 10 8 6 4 2 0
}
```



**数组遍历**

```kotlin
// 法一，用区间
fun main() {
    val a = arrayOf("a", "b", "c")
    println("a.indices = ${a.indices}")
    for (i in a.indices) {
        print("${a[i]} ")
        // a.indices = 0..2
		// a b c 
    }
}


// 法二，用下标
fun main() {
    val a = arrayOf("a", "b", "c")
    for ((index, value) in a.withIndex()) {
        println("a[$index] = $value")
    }
    /*
    a[0] = a
    a[1] = b
    a[2] = c
     */
}

```



##### while

没什么变化



##### ==标签==

**break、continue标签**

在 Kotlin 中任何表达式都可以用标签来标记

格式为：标识符@，比如 abc@

我们可以用标签限定 ==break== 或者 ==continue== ：

```kotlin
// 标签放在外层循环
fun main() {
    val a = arrayOf(1, 4, 6, 3, 8, 7, 5)
    loop@ for (x in a)
        for (y in a)
            if (y == 3)
                break@loop
            else
                print("$y ")
    // 1 4 6
}

/*
由于我们的loop标签是打在外层循环的
因此不管内层循环有多少层，
遇到 break@loop，
都会终止整个循环

*/

// 标签放在内层循环
fun main() {
    val a = arrayOf(1, 4, 6, 3, 8, 7, 5)
    for (x in a)
        loop@ for (y in a)
            if (y == 3)
                break@loop
            else
                print("$y ")
    // 1 4 6 1 4 6 1 4 6 1 4 6 1 4 6 1 4 6 1 4 6 
}
```



**返回到标签**

有点难懂(P466)



### 异常

#### 异常类

Kotlin 中所有异常类继承自 ==Throwable== 类。每个异常都有消息、堆栈回溯信息以及可选的原因

使用 ==throw== 表达式来抛出异常：

```kotlin
fun main() {

	throw Exception("Hi There!")

}
```

使用 ==try …… catch== 表达式来捕获异常：

```kotlin
try {
	// 一些代码
	} catch (e: SomeException) {
	// 处理程序
	} finally {
	// 可选的 finally 块
}

```

**Try 是一个表达式**

try 是一个表达式，意味着它可以有一个返回值：

```kotlin
val a: Int? = 
try {
    input.toInt() 
} catch (e: NumberFormatException) { 
    null 
}
```

#### 受检异常

Kotlin没有受检异常



#### Nothing类型

throw 表达式的类型是 Nothing 类型。 这个类型没有值，而是用于标记永远不能达到的代 码位置。 在你自己的代码中，你可以使用 Nothing 来标记一个永远不会返回的函数：

```kotlin
fun fail(message: String): Nothing {
throw IllegalArgumentException(message)
}
```

当你调用该函数时，编译器会知道在该调用后就不再继续执行了：

```kotlin
fun main() {
    println(1)
    val name = fail("胡桃太可爱了")
    println(2)
}

fun fail(message: String): Nothing {
    throw IllegalArgumentException(message)
}
```



![[Pasted image 20240623170513.png]]

如果用 null 来初始化一个要推断类型的值，而又没有其他信息可用于确定更具 体的类型时，编译器会推断出 Nothing? 类型：

```kotlin
val x = null // “x”具有类型 `Nothing?`
val l = listOf(null) // “l”具有类型 `List<Nothing?>
```



### 包与导入

源文件通常以包声明开头:

```kotlin
package org.example
fun printMessage() { /*……*/ }
class Message { /*……*/ }
// ……
```

如果没有指明包，该文件的内容属于无名字的==默认包==。



#### 导入

##### 默认导入

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240212194158200.png" alt="image-20240212194158200" style="zoom:80%;" />

另外：

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240212194219966.png" alt="image-20240212194219966" style="zoom:80%;" />



##### import

如果出现==名字冲突==，可以使用 as 关键字在本地重命名冲突项来消歧义：

```kotlin
import org.example.Message // Message 可访问
import org.test.Message as TestMessage 
// TestMessage 代表“org.test.Message”
```



### 类与对象

#### 类

##### 声明类

```kotlin
class Person { /*……*/ }
```



##### 构造函数

在 Kotlin 中的一个类有一个==主构造函数==并可能有一个或多个==次构造函数==

主构造函数在类头中声明：

```kotlin
class Person constructor(firstName: String) { /*……*/ }
/*
如果主构造函数没有任何注解或者可见性修饰符，可以省略这个 constructor 关键字。
*/
class Person(firstName: String) { /*……*/ }
```

属性声明时，可以用尾部逗号，该属性可以是val也可以是var：

```kotlin
class Person(
	val firstName: String,
	val lastName: String,
	var age: Int, // 尾部逗号
) { /*……*/ }
```

如果构造函数有注解或可见性修饰符，这个 constructor 关键字是必需的，并且这些修饰符在它前面：

```kotlin
class Customer public @Inject constructor(name: String) { /*……*/ }
```



##### ==次构造函数==

首先，使用次构造函数时候必须调用主构造函数

```kotlin
data class User(val username: String, val password: String) {
    constructor(username: String, password: String, createTime: LocalDateTime) : this(username, password)
    constructor(username: String, password: String, country: String, city: String) : this(username, password)
}

fun main() {
    val user1 = User("胡桃", "HuTao")
    val user2 = User("胡桃", "huTao", LocalDateTime.now())
    val user3 = User("胡桃", "HuTao", "璃月", "往生堂")
    println(user1)
    println(user2)
    println(user3)
}

/*
User(username=胡桃, password=HuTao)
User(username=胡桃, password=huTao)
User(username=胡桃, password=HuTao)
*/
```



然后，想要打印对象时有重写`toString`方法的话，就需要在类前加一个 ==data== ，但是这只能打印主构造函数的参数列表



如果一个非抽象类没有声明任何（主或次）构造函数，它会有一个生成的不带参数的主构造函数（无参构造函数）。



##### 创建类的实例

只需像普通函数一样调用构造函数：

```kotlin
val invoice = Invoice()
val customer = Customer("Joe Smith")
```

> Kotlin没有 new 关键字



##### 抽象类

感觉没什么用，Java从来没用过



#### 继承

Kotlin最大的父类为 ==Any== ，Java中的为 ==Object==

`Any`有三个方法：

- equals()
- hashCode()
- toString()

默认情况下类是`final`，无法继承。如果要继承，则需要用`open`标记:

```kotlin
open class Base(p: Int)
class Derived(p: Int) : Base(p)
```



#### 覆盖

重写的前提是类为 ==open==

##### 覆盖方法

```kotlin
open class Shape {
open fun draw() { /*……*/ }
fun fill() { /*……*/ }
    }
class Circle() : Shape() {
override fun draw() { /*……*/ }
}

```

标记为 override 的成员本身是开放的，因此可以在子类中覆盖。如果你想禁止再次覆盖， 使用 final 关键字:

```kotlin
open class Rectangle() : Shape() {
final override fun draw() { /*……*/ }
}
```



##### 覆盖属性

即重新声明属性、属性值

```kotlin
open class Shape {
open val vertexCount: Int = 0
}
class Rectangle : Shape() {
override val vertexCount = 4
}
```

可以在主构造函数中使用 `override` 作为属性声明的一部分：

```kotlin
interface Shape {
val vertexCount: Int
}
class Rectangle(override val vertexCount: Int = 4) : Shape // 总是有 4 个顶点
```



##### 覆盖规则

如果一个类从它的直接超类继承相同成员的多个实现，它必须覆盖这个成员并提供其自己的实现（也许用继承来的其中之一）

```kotlin
open class Rectangle {
open fun draw() { /* …… */ }
}
interface Polygon {
fun draw() { /* …… */ } // 接口成员默认就是“open”的
}
class Square() : Rectangle(), Polygon {
// 编译器要求覆盖 draw()：
override fun draw() {
super<Rectangle>.draw() // 调用 Rectangle.draw()
super<Polygon>.draw() // 调用 Polygon.draw()
}
}

```



#### 属性

##### 自定义getter

```kotlin
class Rectangle(val width: Int, val height: Int) {
val area: Int
    get() = this.width * this.height
}

// 如果数据类型可以推断出来，可省略
val area get() = this.width * this.height
```



##### 自定义setter

```kotlin
var stringRepresentation: String
	get() = this.toString()
	set(value) {
	setDataFromString(value) // 解析字符串并赋值给其他属性
}

// set的参数名称是 value
```



##### `幕后字段、属性`

不知道什么时候会用



##### 延迟初始化

一般属性声明必须在构造函数中初始化，但是使用了 `lateinit` 关键字标注，就只要求在使用前初始化就行了

但是该修饰符只能用在类体中的属性（不是主构造函数）、顶层属性和局部变量，且是非空类型、不能是原生类型

```kotlin
data class User(
    val username: String,
    val password: String,
) {
    lateinit var hobby:String
}

fun main() {
    val user1 = User("胡桃", "HuTao")
    if(user1.username == "胡桃")
        user1.hobby = "殡仪"
    else user1.hobby = "原神"
    println(user1.hobby)
}

// 殡仪
```



检测 ==lateinit== 是否初始化

```kotlin
user1::hobby.isInitialized()
```



#### 接口

##### 定义接口

使用 ==interface== 定义：

```kotlin
interface MyInterface {
		fun bar()
		fun foo() {
		// 可选的方法体
	}
}
```



##### 实现接口

```kotlin
class Child : MyInterface {
	override fun bar() {
		// 方法体
	}
}
```



##### 接口中的属性

在接口中声明的属性要么是抽象的，要么提供==访问器的实现==

```kotlin
interface File {
    val fileSize:Long
    fun addFile()
}

class FileServiceImpl:File{
    // 实现接口的属性
    override val fileSize: Long
        get() = 1_000_000
    // 实现接口的方法
    override fun addFile() {
        println("添加文件")
    }
}

fun main() {
    // 创建接口实现类的对象
    val a = FileServiceImpl()
    a.addFile()
    println(a.fileSize)
}
/*
添加文件
1000000
*/
```



##### 函数式接口

只有一个抽象方法的接口，称为 ==函数式接口== 或 ==单一抽象方法（SAM）接口==

函数式接口可以有多个非抽象成员，但==只能有一个抽象成员==。

```kotlin
fun interface KRunnable {
	fun invoke()
}
```



##### 可见性修饰符

private、protected、internal、public

默认是 ==public==

- public：声明随处可见
- private：只在声明它的文件内可见
- internal：在相同的模块内随处可见
- protect 不适用于顶层声明

```kotlin
// 文件名：example.kt

private fun foo() {
    
} // 在 example.kt 内可见

public var bar: Int = 5 // 该属性随处可见
    private set // setter 只在 example.kt 内可见
internal val baz = 6 // 相同模块内可见
```



##### 扩展函数

比如给 `MutableList<Int>` 扩展一个 ==huTao== 函数

```kotlin
fun MutableList<Int>.huTao(index1: Int, index2: Int) {
    val tmp = this[index1] // “this”对应该列表
    this[index1] = this[index2]
    this[index2] = tmp
}

fun main() {
    val a = mutableListOf(1,2,3,4,5,6)
    a.huTao(1,4)
    for (i in a) {
        println(i)
    }
}
// 1 5 3 4 2 6
```



扩展一般都写在顶层



#### 数据类

用 `data` 标注的类，就是数据类，类似==@Data==注解，它也会自动生成：

- .equals()/.hashCode()
- .toString()
- .componentN()
- .copy()

数据类的要求：

- 主构造函数需要==至少有一个参数==。 
- 主构造函数的所有参数需要标记为 ==val== 或 ==var== 。



##### copy()

复制一个对象，同时可以选择修改它的一些属性

```kotlin
data class File(val filename:String,val fileType:String)

fun main() {
    val file1 = File("提瓦特指南","PDF")
    val file2 = file1.copy(fileType = "txt")
    println(file1)
    println(file2)
}

/*
File(filename=提瓦特指南, fileType=PDF)
File(filename=提瓦特指南, fileType=txt)
/*
```

用来测试的话挺方便



##### 解构

就是 component() 方法

它可以将一个对象的属性，拿出来单独作为一个变量（或者多个）来使用

```kotlin
data class File(val filename:String,val fileType:String)

fun main() {
    val file1 = File("提瓦特指南","PDF")
    // 这里解构
    val (filename,fileType) = file1
    println("filename = $filename")
    println("fileType = $fileType")
}

/*
filename = 提瓦特指南
fileType = PDF
*/
```



#### 密封类

==sealed== 标注

```kotlin
sealed interface Error
sealed class IOError(): Error
```

感觉用不上



#### 泛型

Kotlin 中的类可以有类型参数，与 Java 类似

```kotlin
class Box<T>(t: T) {
	var value = t
}

fun main(){
    val box: Box<Int> = Box<Int>(1)
}
```



##### 型变（协变、逆变）

Joshua Bloch 称那些你只能从中==读取==的对象为==生产者==，并称那些只能向其==写入==的对象为==消费者==



##### 声明处型变

即在声明参数时，指明该类是生产者还是消费者

==out==，将标识为==协变==，即只能读取，不能修改。生产者

```kotlin
interface Source<out T> {
	fun nextT(): T
}
```

==in== ，将标识为==逆变==，即只能修改，不能读取。消费者

```kotlin
interface Comparable<in T> {
	operator fun compareTo(other: T): Int
}
```



##### 类型投影

难懂

##### 星投影

难啃



##### 泛型函数

即函数也可以使用泛型：

```kotlin
// 万能输出器
fun <T> print(objs: T) {
    println(objs)
}


// 扩展泛型函数
fun <T> T.print(objs: T){
    println(objs)
}

fun main() {
    val printUtil:Any = ""
    val aNumber = 6
    val aString = "胡桃"

    // 重点是：任何类型的变量都可以使用这个函数
    print(aNumber)
    print(aString)

    // 扩展泛型函数
    printUtil.print(aNumber)
    printUtil.print(aString)
}

/*
6
胡桃
6
胡桃
*/
```



##### 泛型约束

就是只有指定的一些类型才能传参进来

最常见的约束类型是 ==上界== ，对应Java中的 `extends`

```kotlin
fun <T : Comparable<T>> sort(list: List<T>) { …… }
```

冒号之后指定的类型是上界，表明只有 ==Comparable\<T\>== 的子类型可以替代 ==T==

```kotlin
sort(listOf(1, 2, 3)) 
// OK。Int 是 Comparable<Int> 的子类型
sort(listOf(HashMap<Int, String>())) 
// 错误：HashMap<Int, String> 不是 Comparable<HashMap<Int, String>> 的子类型
```



默认的上界是 ==Any?==

尖括号只能指定一个上界，否则需要单独的一个where子句，来指定多个上界：

```kotlin
fun <T> copyWhenGreater(list: List<T>, threshold: T): List<String>
        where T : CharSequence,
              T : Comparable<T> {
    return list.filter { it > threshold }.map { it.toString() }
}

// 类型 T 必须 既实现了 CharSequence 也实现了 Comparable
```



##### 类型擦除
