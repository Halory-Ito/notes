## 概述

### 目的和条件

- kotlin的设计目的是取代Java，简化开发
- Kotlin编写的程序可以运行在任何Java能够运行的地方



### 应用场景

目前主要（这两种都需要Java虚拟机）：

- 服务器端编程。基于JavaEE的Web服务器端开发和数据库编程等
- Android应用开发

处于原型阶段：

- 编译成JavaScript。这样就可以用于Web前端开发
- 编译成Native代码，这样本地运行就不需要虚拟机了，类似C语言



### 语言特点

- 简洁
  - 相比于Java，会大大减少代码量
- 安全
  - 支持非空和可空类型，默认情况下Kotlin的数据类型声明的变量，都是不能接收空值的，这样可以避免发生空指针异常
- 类型推导
  - Kotlin编译器可以根据变量所在的上下文环境来推导出变量的数据类型，这样可以省略声明变量时明确指定数据类型
- 支持面向对象
- Java具有良好的互操作性
  - Kotlin不需要任何转换或包装就可以调用Java对象。==Kotlin完全可以使用现有的Java框架或库==
- 免费开源



### 运行过程

Java解释Kotlin字节码文件时，需要Kotlin运行时库支持才能正常运行





### 学习网站

[Kotlin源代码网址](https://github.com/JetBrains/kotlin)

[Kotlin官网](https://kotlinlang.org/)

[Kotlin官方参考文档](https://kotlinlang.org/docs/home.html)

[Kotlin标准库](https://kotlinlang.org/api/latest/jvm/stdlib/)

`Kotlin标准库`是Kotlin的API文档，需要重点关注



## 开发环境

**IDE**

IntelliJ IDEA可以编写一般的Kotlin程序，如果要编写Android应用程序则需要使用Android Studio

**编译器**

JetBrains提供了Kotlin编译器，这样开发人员就可以使用文本文件编写Kotlin程序，然后交给Kotlin进行编译

JDK也要下载安装，并配置环境变量

**文本编辑工具**

轻量级的文本编辑工具：Sublime Text

**运行Kotlin**

- 编译：`kotlinc HelloWorld.kt`
- 运行：`kotlin HelloWorldKt.class`



## 语法基础

### 标识符跟关键字

#### 标识符

- 区分大小写
- 首字符只能为_或者字母
- 硬关键字（Hard Keywords）不能作为标识符，软关键字跟修饰符关键字在它们的适用场景之外可以作为标识符使用
- 特定的标识符`field`和`it`，它们只能在特定的场景中使用，有特定的作用。而在其他的场景中可以作为标识符使用

> 注意：如果一定要使用关键字作为标识符，可以使用反引号 ``
>
> Kotlin语言中，字母采用的是双字节的Unicode

#### 关键字

- 硬关键字：as、if、for、run、val、var、package等
- 软关键字：by、catch、get、import、init等
- 修饰符关键字：abstract、const、data、final、infix等



### 变量和常量

#### 变量

变量通过`var`进行声明

```kotlin
var hello = "你好"
```

#### 常量和只读变量

这两类变量一经初始化后就无法被改变，通过`val`或者`const val`声明，区别如下：

- val：该关键字声明的是运行期常量，常量是在运行时初始化的
- const val：该关键字声明的是编译期常量，常量是在编译时初始化的，只能用于==顶层常量声明==或者声明对象中的==常量声明==，而且只能是String类型或基本数据类型

> 注：
>
> const val相当于Java中的public final static
>
> val 相当于 final

```kotlin
// 示例代码
const val MAX_COUNT = 1000						// 声明顶层常量
const val NAME = "胡桃"						   // 声明顶层常量

// 编译错误，编译期常量只能是String或者基本数据类型
const val NAME_ = StringBuilder("HelloWorld")	

// 声明对象
object User{
    const val MAX_COUNT = 100					// 声明对象中的声明常量
}

public fun main(){
    NAME = "hutao"								// 编译错误，常量值不能被修改
    val scoreForStudent: Float = 0.0f
    val y = 20
    y = 30											// 编译错误，常量值不能被修改
    const val x = 10							 // 编译错误，const val只能用于顶层声明
}
```



#### var or val？

当`var`跟`val`都能满足需求时，优先选择`val`

一方面`val`声明的是只读变量，可以避免程序在运行过程中错误地修改变量内容

另一方面在声明`引用类型`使用`val`时，对象的引用不会被修改，但是引用的内容可以修改，这样会更加安全，也符合函数式编程的技术要求

```kotlin
// val 声明的引用类型
class Person(val name: String, val age: Int)

public fun main() {
    val p1 = Person("hutao", 18)
    
    println(p1.name)
    println(p1.age)
    
    // 编译错误，通过val声明的p1的引用对象不能修改
    // 除非p1用var声明
    p1 = Person("Tom", 20)			
}
```

### 注释

```kotlin
// 单行注释


/*	多行注释  */
```



### 语句和表达式

#### 语句

一条语句可以加分号，也可以不加分号

但是有一种情况必须加分号，那就是多条语句写在一行时

```kotlin
val a1: Int = 10; var a2: Int = 20;
```

#### 表达式

> 原则上：在声明变量或者常量的时候不指定数据类型，除非需要指定特殊的数据类型，比如将 `var a3: Long = 10`
>
> 另外如果分号不是必须加的话，都不加分号，遵循Kotlin的简洁特性

```kotlin
public fun main() {
    val englishScore = 95
    val chineseScore = 98

    // if 控制结构表达式
    val result = if (englishScore < 60) "不及格" else "及格"
    println(result)

    val totalScore = sum(englishScore, chineseScore)
    println(totalScore)

    // try 表达式
    val score = try {
    	// TODO
    } catch (e: Exception) {
    	return
    }
}

```



### 包

#### 包的作用

防止类、接口、枚举、注释和函数等内容`命名冲突`

#### 包的定义

包使用`package`语句进行定义，且放在源文件的第一行，每个源文件只有一个包定义语句

```kotlin
// 代码文件：/com/anckson/entity/Date.kt
package com.anckson.entity

class Date {
    override fun toString(): String{
        return "今天是昨天的明天"
    }
}

fun add(a: Int, b: Int): Int = a + b
```

如果在源文件中没有定义包，那么文件中定义的类、接口、枚举、注释和函数等内容将会被放进一个无名的包中，也称为`默认包`

#### 包的引入

跟Java一样

```kotlin
import java.util.Date
```



### 数据类型

#### 基本数据类型

- 整数类型：Byte、Short、Int和Long，其中`Int`是默认类型
- 浮点类型：Float和Double，其中`Double`是默认类型
- 字符类型：Char
- 布尔类型：Boolean

> Kotlin没有对应的包装类，但是它会根据不同的场景将数据类型编译成Java中的数据类型或者包装类，比如Int，在声明变量时，它会被编译成int类型，当作为集合泛型类型参数时，会被编译成Integer类型



##### 整数类型

| 整数类型  | 字节 |             取值范围             |
| :-------: | :--: | :------------------------------: |
|   Byte    |  1   |             -128~127             |
|   Short   |  2   | -2<sup>15</sup>~2<sup>15</sup>-1 |
| Int(默认) |  4   | -2<sup>31</sup>~2<sup>31</sup>-1 |
|   Long    |  8   | -2<sup>63</sup>~2<sup>63</sup>-1 |

```kotlin
public fun main() {
  println("默认整数常量 = " + 16)
  val a: Byte = 16
  val b: Short = 16
  val c = 16
  val d = 16L
  // 使用字符串模板引用变量
  println("Byte整数		=	$a")
  println("Short整数		=	$b")
  println("Int整数		=	$c")
  println("Long整数		=	$d")

// 在数字常量添加下划线，增强可读性，整型跟浮点型都能用
  val e = 160_000_000L					// 表示数字160000000
  val f = 16_0000_0000L					// 表示数字1600000000
  println("数字常量添加下划线 = $e")

// 进制表示方式
  val decimalInt = 28					// 十进制
  val binaryInt1 = 0b11100				// 二进制
  val binaryInt2 = 0B11100				// 二进制
  val hexadecimalInt1 = 0x1C			// 十六进制
  val hexadecimalInt2 = 0x1C			// 十六进制

}
```

> 注意：Kotlin中不允许用l来表示Long类型，因为l长得跟1很像，容易混淆



##### 浮点类型

|   浮点类型   | 字节 |
| :----------: | :--: |
|    Float     |  4   |
| Double(默认) |  8   |

如果想要表示Float类型，则需要加f或F

```kotlin
public fun main() {
	println("默认浮点常量 = " + 360.66)
	val myMoney = 360.66f
	val yourMoney = 360.66
	val pi = 3.14159F
	println("Float = $myMoney")
	println("Double = $yourMoney")
	println("pi = $pi")

	// 指数表示方式
	val ourMoney = 3.36e2				// 表示366.0
	val interestRate = 1.56E-2			// 表示0.0156
}

```

> 注意：Kotlin中不能在Double类型的数后面加d或者D



##### 字符类型

Kotlin用Char来声明字符类型。

由于Kotlin字符采用双字节(16位)的Unicode编码，因而可以使用十六进制编码的形式表示，形如`\un`，其中n位十六位进制数

```kotlin
fun main() {
    val c1 = 'A'
    val c2 = '\u0041'
    val c3: Char = '花'
    println(c1)
    println(c2)
    println(c3)
}
```



##### 布尔类型

Kotlin中声明布尔类型的关键字是Boolean，它只有`true`跟`false`

> C语言中布尔类型是数值类型，只有 0 和 1
>
> Java和Kotlin的布尔类型不能用0和1替代

```kotlin
val isMan = true
val isWoman = false

// 下面会报错
val isMan1: Boolean = 1
val isWoman1: Boolean = 'A'
```



#### 数值类型之间的转换

Kotlin中，数值在赋值时采用的是==显示转换==，在数学计算时采用的是==隐式转换==

##### 赋值与显式转换

Kotlin是一种安全的语言，对于类型的检查非常严格，==对不同类型数值进行赋值是禁止的==

```kotlin
val byteNum: Byte = 16
val shortNum: Short = byteNum		// 编译错误
```

> 在Java中，赋值是可以隐式转换的，但是Kotlin不行，Kotlin想要实现赋值转换的话，需要用到显示转换函数

显示转换函数：

- toByte(): Byte
- toShort(): Short
- toInt(): Int
- toLong(): Long
- toFloat(): Float
- toDouble(): Double
- toChar(): Char

通过上述的七个转换函数，可以实现七种类型之间的==任意转换==

```kotlin
fun main() {
    val byteNum: Byte = 16
    val shortNum: Short = byteNum.toShort()
    var intNum = 16
    
    val longNum: Long = intNum.toLong()
    intNum = longNum.toInt()
    
    val doubleNum = 10.8
    println(doubleNum.toInt())
    
    val charNum = 'A'
    // 结果为 65
    println(charNum.toInt())
    println(charNum.code)
    // 2025年1月测试时，IDEA建议使用 charNum.code 而 charNum.toInt() 被废弃
    
    val llongNum = 6666666666L
    println("llongNum : $llongNum")
    // 结果为-1923267926，丢失精度
    println("llongNum.toInt : " + llongNum.toInt())
}
```



##### 数学计算与隐式转换

不必多说



#### 可空类型

Kotlin默认情况下声明的变量都是不能接收空值的，即==默认情况下所有数据类型都是非空类型==

```kotlin
var n: Int = 10
n = null	// 发生编译错误
```



##### 可空类型概念

但是有些场景确实没有数据，比如查询数据库的时候，查询不到符合某个条件的数据是很常见的

为此，Kotlin为每一种非空类型提供对于的可空类型（Nullable），就是在非空类型后面加上问号(?)表示可空类型

```kotlin
var n: Int? = 10
n = null	// 正常
```

可空类型在具体使用时会有一些限制：

- ==不能直接调用==可空类型对象的函数或属性
- 不能把可空类型数据赋值给非空类型变量
- 不能把可空类型数据传递给非空类型参数的函数

==为了突破限制==，Kotlin提供了如下运算符：

- 安全调用运算符（?.）: 如果对象为空，则返回`null`
- 安全转换运算符（as?）
- Elvis运算符（?:）: 如果对象为空，则返回冒号后面的值，比如`A?:B`，A为空则返回B.
- 非空断言（!!）: 如果对象为空，则抛出`空指针异常`

此外，还有一个`let函数`帮助处理可空类型数据



##### 安全调用运算符(?.)

安全调用运算符会判断可空类型变量是否为空，如是则返回null，否则返回调用结果

```kotlin
fun divide(n1: Int, n2: Int): Double? {
    if(n2 == 0)
    	return null
    return n1.toDouble() / n2
}

fun main() {
    val divNumber1 = divide(100,0)
    // 这里会判断divNumber1是不是空值，如果是则不会调用plus方法，直接返回null
    val result1 = divNumber1?.plus(100)	// null
    println(result1)
    
    val divNumber2 = divide(100,10)
    val result2 = divNumber2?.plus(100)	// 110.0
    println(result2)
}
```

> 扩展：plus函数是一种加法运算函数，它跟"+"的作用一样。而实际上，"+"是通过调用plus函数进行了运算符重载，以此实现加法运算



##### 非空断言运算符(!!)

非空断言就是断言可空类型变量不会为空。

调用过程是存在风险的，如果可空类型变量真的为空，则会抛出空指针异常；如果非，则可以正常调用函数或属性

```kotlin
fun divide(n1: Int, n2: Int): Double? {
    if(n2 == 0)
        return null
    return n1.toDouble() / n2
}

fun main() {
    val divNumber1 = divide(100,10)
    val result1 = divNumber1!!.plus(100)	// 110.0
    println(result1)
    val divNumber2 = divide(100,0)
    // 非空断言调用抛出异常
    val result2 = divNumber2!!.plus(100)	// 抛出异常
    println(result2)
}
/*
110.0
Exception in thread "main" java.lang.NullPointerException
	at HelloWorldKt.main(HelloWorld.kt:13)
	at HelloWorldKt.main(HelloWorld.kt)
 */
```



##### Elvis运算符(?:)

有时在可空类型表达式中，当表达式为空时，并不希望返回默认的空值，而是其他数值，此时就可以用Elvis运算符

`A?:B`，如果`A`不为空则为A，否则`B`

```kotlin
fun divide(n1: Int, n2: Int): Double? {
    if (n2 == 0)
        return null
    return n1.toDouble() / n2
}

fun main() {
    val divNumber1 = divide(100, 0)

    // 这里 divNumber1?.plus(100) 为 null
    // null ?: "这是空值哦"
    val result1 = divNumber1?.plus(100) ?: "这是空值哦"    
    println(result1)

    val divNumber2 = divide(100, 10)
    val result2 = divNumber2?.plus(100) ?: 0    
    println(result2)
}
/*
这是空值哦
110.0
*/
```



### 字符串

#### 字符串字面量

Kotlin中的字符串字面量有两种：

- 普通字符串，用双引号包裹
- 原始字符串，用三个双引号包裹

##### 普通字符串

不必多说

##### 原始字符串

主要的特点就是，换行不需要\n，直接就能换行

```kotlin
fun main() {
    val s1 = "Hello\nWorld"
    val s2 = """Hello
World"""
    println(s1 == s2)
    val s3 = """Hello
        World"""
    println(s1 == s3)
}
/*
true
false
*/
// 无论采用普通字符串表示，还是原始字符串表示，其实都一样
```

> `==`运算符可以比较基本数据类型和引用类型
>
> 引用类型比较两个对象内容是否相等，等价于equals函数



#### 不可变字符串

Kotlin中默认的字符串类是String，这是一种不可变字符串。

当不可变字符串进行拼接等修改操作时，会创建新的字符串对象。

而可变字符串不会创建新的字符串，在Kotlin中可变字符串类是StringBuilder

##### String

字符串字面量赋值已经熟得不能再熟了，这里重点介绍字符串转换函数

```kotlin
// 字节数组转换成字符串函数
fun String (
	bytes: ByteArray,				// 要转换的字节数组
    offset: Int,					// 字节数组开始索引，非必须
    length: Int,					// 转换字节的长度，非必须
    charset: Charset				// 解码字符集，非必须
): String


// 字符数组转换成字符串函数
fun String(
	chars: CharArray,				// 要转换的字符数组
    offset: Int,					// 字符数组开始索引，非必须
    length: Int						// 转换字符的长度，非必须
): String


// 可变字符串StringBuilder转换为字符串函数
fun String(stringBuilder: StringBuilder): String
```

示例代码如下：

```kotlin
fun main() {
    // 创建字符数组
    val chars = charArrayOf('a', 'b', 'c', 'd', 'e')

    val s1 = String(chars)                          // s1 = abcde
    val s2 = String(chars, 1, 4)        			// s2 = bcde
    println("s1 = $s1")
    println("s2 = $s2")

    // 创建字节数组
    val bytes = byteArrayOf(97, 98, 99)
    val s3 = String(bytes)                          // s3 = abc
    val s4 = String(bytes, 1, 2)      			    // s4 = bc
    println("s3 = $s3")
    println("s4 = $s4")
}
```



##### 字符串拼接

String虽然是不可变字符串，但是也可以进行字符串拼接，只不过会生产新的字符串对象

```kotlin
fun main() {
    var name = "胡桃"
    name = name + "赛高"
    println(name)
}
// 胡桃赛高
```



##### 字符串模板

就是用$来在字符串中使用变量或者常量的

```kotlin
// 假设stu是一个学生对象
val s = "我是 ${stu.name}，我今年 ${stu.age} 岁了，明年就 ${stu.age + 1}岁了"
```



##### 字符串查找

有`indexOf()`和`lastIndexOf()`方法

怎么用以后多写写就会了



##### 字符串比较

三种比较：

- 比较相等：`!=`、`==`、`equals()`
- 比较大小：`compateTo()`
- 比较前缀和后缀：`startsWith()`、`endsWith()`

```kotlin
// 比较相等
// !=和==底层实际上是通过调用equals函数来比较的
fun main() {
    val s1 = "hello"
    val s2 = "Hello"

    println(s1 == s2)
    // 忽略大小写
    println(s1.equals(s2,true))
}
/*
false
true
*/


// 比较大小，按照字典顺序
// 得到的是ASCII码的差值
fun main() {
    val s1 = "abcde"
    val s2 = "Accde"
    val s3 = "ABCDE"

    // s1 < s2
    println(s1.compareTo(s2))
    // s1 < s3
    println(s1.compareTo(s3))
    // s3 > s1
    println(s3.compareTo(s1))
    // 忽略大小写比较大小
    println(s1.compareTo(s3,true))
}
/*
32
32
-32
0
*/


// 比较前缀和后缀
fun main() {
    // 判断文件夹中的文件名
    val docFolder = arrayOf("java.docx", "JavaBean.docx", "JAVA.xlsx")
    var docCount = 0
    // 查找文件夹中word文档个数
    for(doc in docFolder) {
        if(doc.endsWith(".docx"))
            docCount++
    }
    println("文件夹中文档的数量为： $docCount")

    var javaCount = 0
    // 查找文件夹中以"java"开头的文件，不区分大小写
    for(java in docFolder)
        if(java.startsWith("java",true))
            javaCount++
    println("文件夹中java文件数量为：$javaCount")
}
/*
2
3
*/
```



##### 字符串截取

substring有三种：

- 指定整数区间截取字符串
- 从指定索引开始截取
- 指定开始索引跟结束索引，左右闭区间。

```kotlin
fun main() {
    val sourceStr = "There is a place where lovers go, to cry their troubles away."

    println(sourceStr.substring(9))
    println(sourceStr.substring(9,32))
    println(sourceStr.substring(9..32))
}
/*
a place where lovers go, to cry their troubles away.
a place where lovers go
a place where lovers go,
*/
```



#### 可变字符串

可变字符串在追加、删除、修改、插入和拼接等操作不会产生新的对象

##### StringBuilder

Kotlin提供的可变字符串就是StringBuilder，它有四种构造函数：

- StringBuilder()，创建空的StringBuilder对象，初始容量默认为16个字符
- StringBuilder(seq: CharSequence)
- StringBuilder(capacity: Int),创建指定容量的StringBuilder对象
- StringBuilder(str: String)，由指定字符串创建StringBuilder对象

```kotlin
fun main() {
    // 字符串长度length和字符串缓冲区容量
    val sb = StringBuilder()
    println("字符串长度：${sb.length}")
    println("字符串容量：${sb.capacity()}")

    val sb2 = StringBuilder("Hello")
    println("字符串长度：${sb2.length}")
    println("字符串容量：${sb2.capacity()}")

    // 字符串缓冲区初始容量是16，超过之后会扩容
    val sb3 = StringBuilder()
    for (i in 0..16) {
        sb3.append(8)
    }
    println("sb3 = $sb3")
    println("字符串长度：${sb3.length}")
    println("字符串容量：${sb3.capacity()}")
}
/*
字符串长度：0
字符串容量：16
字符串长度：5
字符串容量：21
sb3 = 88888888888888888
字符串长度：17
字符串容量：34
*/
```



##### 字符串操作

StringBuilder提供了很多修改字符串的函数，但是他们的返回值还是StringBuiler：

- append()
- insert()
- delete()
- replace()

```kotlin
fun main() {
    // 添加字符串、字符
    val sb1 = StringBuilder("Hello")
    sb1.append(" ").append("World\n")
    println(sb1)

    // 添加布尔值、转义符和空对象
    val sb2 = StringBuilder()
    val obj: Any? = null
    sb2.append(false).append('\t').append(obj)
    println("sb2 = $sb2")

    // 添加数值
    val sb3 = StringBuilder()
    for(i in 0..9)
        sb3.append(i)
    println("sb3 = $sb3")
    // 插入字符串（插入到指定位置）
    sb3.insert(4,"Kotlin")

    // 删除字符串，删除1到2之间的字符串，但不包含
    sb3.delete(1,2)
    println("sb3 = $sb3")

    // 替换字符串，3到9的部分（范围替换）替换成A
    sb3.replace(3,9,"A")
    println(sb3)
}
/*
Hello World

sb2 = false	null
sb3 = 0123456789
sb3 = 023Kotlin456789
023A456789
*/
```



#### 正则表达式

##### Regex类

Kotlin提供Regex类来使用正则表达式，有两种方式创建Regex对象：

- 构造函数Regex()
- toRegex扩展函数，由String提供

```kotlin
fun main() {
    val pattern = """\w+@[a-zA-Z_]+?\.[a-zA-Z]{2,3}"""
    val string = "2082004203@qq.com"
    val regex = Regex(pattern)
//    val regex = pattern.toRegex()

    println(regex.matches(string))

}
/*
下面的正则表达式用于校验邮箱格式
\w+@[a-zA-Z_]+?\.[a-zA-Z]{2,3}
*/
```



##### 字符串匹配

- matches(input: CharSequence): Boolean
  - 用于精确匹配函数
- containsMatchIn(input: CharSequence): Boolean
  - 包含匹配函数



##### 字符串查找

- find()
- findAll()



##### 字符串替换

- replace()



##### 字符串分割

- split()





### 运算符

#### 算数运算符

##### 一元运算符

| 运算符 | 说明     | 例子   |
| :----: | -------- | ------ |
| ==-==  | 取反符号 | b = -a |
|   ++   | 自增     | a++    |
|   --   | 自减     | b--    |

##### 二元运算符

| 运算符 | 说明 | 例子 |
| :----: | ---- | ---- |
|   +    |      |      |
|   -    |      |      |
|   *    |      |      |
|   /    |      |      |
|   %    |      |      |



##### 算术赋值运算符

| 运算符 | 说明 | 例子 |
| :----: | ---- | ---- |
|   +=   |      |      |
|   -=   |      |      |
|   *=   |      |      |
|   /=   |      |      |
|   %=   |      |      |



#### 关系运算符

| 运算符 | 说明                                                         | 例子 |
| :----: | ------------------------------------------------------------ | ---- |
|   ==   |                                                              |      |
|   !=   |                                                              |      |
|   >    |                                                              |      |
|   <    |                                                              |      |
|   >=   |                                                              |      |
|   <=   |                                                              |      |
|  ===   | 引用等于。用于引用类型比较，比较==两个引用是否是同一个对象== |      |
|  !==   | 与===相反                                                    |      |

#### 逻辑运算符

| 运算符 | 说明 | 例子 |
| :----: | ---- | ---- |
|   !    |      |      |
|   &&   |      |      |
|  \|\|  |      |      |



#### 位运算符

| 运算符 | 说明         | 例子                 |
| :----: | ------------ | -------------------- |
|  inv   | 按位取反     | x.inv()              |
|  and   | 进行位与运算 | x and y或 x.and(y)   |
|   or   | 进行位或运算 | x or y或 x.or(y)     |
|  xor   | 进行异或运算 | x xor y或 x.xor(y)   |
|  shr   | 有符号右移   | x shr y或 x.shr(y)   |
|  shl   | 左移         | x shl y或 x.shl(y)   |
|  ushr  | 无符号右移   | x ushr y或 x.ushr(y) |

> 注意：无符号右移仅被允许在 `Int` 和 `Long` 中使用，如果用于 Short 或者 Byte 的数据，则数据会被转换为 Int 后再进行无符号右移

有符号右移，相当于给操作数乘以 2<sup>n</sup>



#### 其他运算符





#### 运算符优先级



### 程序流程控制

#### 分支结构

##### if分支

if结构当语句使用，同Java，这里介绍if语句当表达式

if表达式中每个代码块的最后一个表达式就是它的返回值

```kotlin
fun main() {
    val score = 95

    // if-else
    val res1 = if(score < 60)
        println("不及格")
    else
        println("及格")
    // 该情况下res1其实没有值，因为println没有返回值

    val res2 = if(score < 60){
        println("不及格")
        "重新考试"
    }
    else {
        println("及格")
        "通过考试"
    }
    println("res2 = $res2")
    // 该情况下，单纯的一个字符串是可以作为返回值使用的

    // else-if
    val testScore = 76
    val grade = if(testScore >= 90)
        'A'
    else if(testScore >= 80)
        'B'
    else if(testScore >= 70)
        'C'
    else if(testScore >60)
        'D'
    else
        'F'
    println("Grade = $grade")

}
/*
及格
及格
res2 = 通过考试
Grade = C
/*
```





##### when分支

when类似Switch语句，但是跟Switch语句有很大差别：

- when语句可以使用各种类型的数据当作表达式，且比较的数据可以是离散的也可以是连续的
- 无需添加break
- 可以作为表达式使用

**when结构当作语句使用**

```kotlin
fun main() {
    val score = 75
    when (score / 10) {
        9 -> println("优")
        8 -> println("良")
        7, 6 -> println("中")
        else -> println("差")
    }

    val level = "优"
    var desc = ""
    when (level) {
        "优" -> desc = "90分以上"
        "良" -> desc = "80分到90分"
        "中" -> desc = "70分到80分"
        "差" -> desc = "低于60分"
    }
    println("说明： $desc")
}

/*
中
说明： 90分以上
*/
```



**when表示式**

```kotlin
fun main() {
    val score = 75
    val grade = when (score / 10) {
        9 -> "优"
        8 -> "良"
        7, 6 -> "中"
        else -> "差"
    }
    println("grade = $grade")

    val level = "优"
    val desc = when (level) {
        "优" -> "90分以上"
        "良" -> "80分到90分"
        "中" -> "70分到80分"
        "差" -> "低于60分"
        else -> "无法判断"
    }
    println("说明： $desc")
}
/*
grade = 中
说明： 90分以上
*/
```

> 注意：when语句当表达式使用时，最后必须要有else语句



#### 循环结构

##### while语句

```kotlin
fun main() {
    var i = 0
    while (i * i < 100_100)
        i++
    println("i = $i")
    println("i * i = ${i * i}")
}
/*
i = 317
i * i = 100489
*/
```



##### do-while语句

```kotlin
fun main() {
    var i = 0
    do {
        i++
    } while (i * i < 100_100)
    println("i = $i")
    println("i * i = ${i * i}")
}
/*
i = 317
i * i = 100489
*/
```



##### for语句

Kotlin的for语句相等于Java中的增强for，只用于对范围、数组或者集合进行遍历

```kotlin
// 范围
fun main() {
   for (num in 0..9)
       println("$num * $num = ${num * num}")
}
/*
0 * 0 = 0
1 * 1 = 1
2 * 2 = 4
3 * 3 = 9
4 * 4 = 16
5 * 5 = 25
6 * 6 = 36
7 * 7 = 49
8 * 8 = 64
9 * 9 = 81
*/


// 数组
fun main() {
   val numbers = intArrayOf(43,23,54,76,88,99)
    for (number in numbers) {
        println("Count is: $number")
    }
}

// 使用循环变量
fun main() {
   val numbers = intArrayOf(43,23,54,76,88,99)
    for (i in numbers.indices) {
        println("numbers[$i] = ${numbers[i]}")
    }
}

```



#### 跳转语句

##### break语句

break语句有带标签和不带标签两种，这里介绍带标签的。不带标签的break语句跟Java一样用

```kotlin
// 当y==x时，会把@labelOne标签指向的for语句结束掉
fun main() {
   labelOne@for(x in 0..4)
       for(y in 5 downTo 1) {
           if (y == x)
               break@labelOne
           println("(x,y) = ($x,$y)")
       }
    println("Game Over")
}
/*
(x,y) = (0,5)
(x,y) = (0,4)
(x,y) = (0,3)
(x,y) = (0,2)
(x,y) = (0,1)
(x,y) = (1,5)
(x,y) = (1,4)
(x,y) = (1,3)
(x,y) = (1,2)
Game Over
*/
```



##### continue语句

continue也有带标签跟不带标签的两种，这里记一下带标签的

```kotlin
fun main() {
   labelOne@for(x in 0..3)
       for(y in 4 downTo 1){
           if(y == x)
               continue@labelOne
           println("(x,y) = ($x,$y)")
       }
    println("Game Over")

}
/*
(x,y) = (0,4)
(x,y) = (0,3)
(x,y) = (0,2)
(x,y) = (0,1)
(x,y) = (1,4)
(x,y) = (1,3)
(x,y) = (1,2)
(x,y) = (2,4)
(x,y) = (2,3)
(x,y) = (3,4)
Game Over
*/
```



#### 使用区间

##### 区间表示

区间有开区间、半开区间和闭区间三种，其中：

- 闭区间，使用区间运算符（`..`）表示
- 半开区间使用中缀运算符（`until`）表示

```kotlin
fun main() {
    for (x in 0..5)
        print(x)
    println()
    for (x in 0 until 5)
        print(x)
    println()
    for (x in 'A'..'G')
        print(x)
    println()
    for (x in 'A' until 'G')
        print(x)
}
/*
012345
01234
ABCDEFG
ABCDEF
*/
```

> 注意：区间中的元素只能是==整数==或者==字符==类型
>
> Kotlin核心库中有三个闭区间类：IntRange、LongRange和CharRange



##### 使用 in 和 !in 关键字

`in`和`!in`可以分别用来判断一个数值是否在区间内、是否不在区间内

此外，这两个关键字还可以判断一个数值是否在集合或数组中

```kotlin
fun main() {
    var testScore = 80
    // 判断数值是否在区间内
    var grade = when (testScore) {
        in 90..100 -> "优"
        in 80 until 90 -> "良"
        in 60 until 80 -> "中"
        in 0 until 60 -> "差"
        else -> "无"
    }
    println("Grade = $grade")

    // 判断数值是否不在区间内
    if (testScore !in 60..100)
        println("不及格")

    // 判断数值是否在数组或集合中
    val strArray = arrayOf("刘备", "关羽", "张飞")
    val name = "赵云"
    if (name !in strArray)
        println("$name 不在队伍中")
}
/*
Grade = 良
赵云 不在队伍中
*/
```



### 函数

#### 函数声明

```kotlin
// 函数声明语法格式
fun 函数名(参数列表): 返回值类型{
    函数体
    return 返回值
}

// 参数列表
(var1: 数据类型, var2: 数据类型, ...)
```

> 如果函数没有返回值，则返回值类型可以省略，return语句也可以不要



```kotlin
fun main() {
   val area = fun1(20.0,480.0)
    println("面积为：$area")

}

fun fun1(width: Double, height: Double): Double{
    return width * height
}
/*
面积为：9600.0
*/
```



#### 返回特殊类型

##### Unit类型

没有返回数据的函数，其类型是`Unit`，相当于Java中的`void`

Kotlin中默认的返回类型为`Unit`

```kotlin
fun main() {
    fun1(20.0, 30.0)
    fun2(20.0, 30.0)
    fun3(20.0, 30.0)

}
// 保留返回值和return
fun fun1(width: Double, height: Double): Unit {
    val area = width * height
    println("面积为：$area")
    return
}

// 省略返回值类型
fun fun2(width: Double, height: Double) {
    val area = width * height
    println("面积为：$area")
    return
}

// 省略返回值类型和return
fun fun3(width: Double, height: Double) {
    val area = width * height
    println("面积为：$area")
}
/*
面积为：600.0
面积为：600.0
面积为：600.0
*/
```



##### Nothing类型

Nothing只用于==函数返回类型的声明==，不能用于变量声明

Nothing声明的函数永远不会正常返回数据，只会抛出异常

```kotlin
import java.io.IOException

fun main() {
    val date = readDate()
}

fun readDate(): Nothing{
    throw IOException()
}
/*
Exception in thread "main" java.io.IOException
	at HelloWorldKt.readDate(HelloWorld.kt:8)
	at HelloWorldKt.main(HelloWorld.kt:4)
	at HelloWorldKt.main(HelloWorld.kt)
*/
```



> 提示：使用Nothing的目的何在？有些框架，例如Junit单元测试框架，在测试失败时会调用Nothing返回类型的函数，通过它抛出异常使当前测试用例失败



#### 函数参数

##### 使用命名参数调用函数

```kotlin
fun callMe(name: String, age: Int){
    println("name = $name	age = $age")
}

fun main(){
    // 下面这个就是使用命名参数调用函数
    callMe(name = "胡桃", age = 18)
    // 传参数参数位置可调换
    callMe(age = 18, name = "胡桃")
}
```



##### 参数默认值

```kotlin
// 简单
```



##### 可变参数

当不确定参数数量时，可以用`vararg`关键字的方式表示可变参数

```kotlin
fun sum(vararg numbers: Double, mutiple: Int = 1): Double {
    var total = 0.0
    for (number in numbers) {
        total += number
    }
    return total * mutiple
}

fun main() {
    println(sum(10.0, 12.2, 13.2))
    println(sum(12.2, 10.2))

    val doubleArr = doubleArrayOf(50.0,60.0,0.0)
    println(sum(30.0,*doubleArr, mutiple = 2))
}
```

> 如果可变参数传递一个数组对象的话，需要用`*`将数组展开



#### 表达式函数体

函数体中的表达式能够表示成单个表达式时，可以用更加简单的形式

```kotlin
// 简化前
fun s(width: Double, height: Double): Double {
    val area = width * height
    return area
}

// 简化后
fun s0(width: Double, height: Double): Double = width * height

fun main() {
    println(s(10.0, 5.0))
    println(s0(10.0, 5.0))
}
```



#### 局部函数

之前声明的都是`顶层函数`，其实在类中或者另一个函数中也可以声明函数，叫做`局部函数`

```kotlin
fun cal(n1: Int, n2: Int, opr: Char): Int {
    val multiple = 2

    // 声明相加函数
    fun add(a: Int, b: Int): Int = (a + b) * multiple

    // 声明相减函数
    fun sub(a: Int, b: Int): Int = (a - b) * multiple

    return if (opr == '+') add(n1, n2) else sub(n1, n2)
}

fun main() {
    println(cal(10,5,'+'))
    // 不能直接用局部函数
//    add(10,5)
//    sub(10.5)
}
```



#### 匿名函数

匿名函数不需要函数名，但是需要`fun`关键字，参数列表和返回类型声明，且需要包含必要的`return`语句

```kotlin
fun cal(n1: Int, n2: Int, opr: Char): Int {
    val multiple = 2

    // 此时的resultNum为函数类型
    val resultNum = if (opr == '+') fun(a: Int, b: Int): Int {
        return (a + b) * multiple
    } else fun(a: Int, b: Int): Int = (a - b) * multiple

    return resultNum(n1, n2)
}

fun main() {
    println(cal(10, 5, '+'))
//    add(10,5)
//    sub(10.5)
}
```



#### 实用函数

```kotlin
// 1. 输入函数 readln()
fun main() {
    print("Enter your name: ")
    var a = readln()
    print(a)
}
/*
Enter your name: Jack
Jack
*/

// 2. 数学工具类 
import kotlin.math.*

fun main() {
    // 幂、绝对值、最值、算术平方根
    println("========== 幂、绝对值、最值、算术平方根 ==========")
    println(2.0.pow(4.0)) // 2^4
    println(abs(-5))
    println(max(19, 20))
    println(min(2, 4))
    println(sqrt(9.0))

    // 三角函数
    println("========== 三角函数 ==========")
    println(sin(PI / 2))
    println(cos(PI))
    println(tan(PI / 4))

    // 反三角函数
    println("========== 反三角函数 ==========")
    println(asin(1.0))
    println(acos(1.0))
    println(atan(0.0))

    // 向上、向下取整
    println("========== 向上、向下取整 ==========")
    println(ceil(4.5))
    println(floor(4.5))

}
```







## 面向对象与函数式编程

OOP

面向对象三个基本特性：封装性、继承性和多态性

### 面向对象编程

#### 类声明

Kotlin中的类有多种：标准类、枚举类、数据类、内部类、嵌套类和密封类等，通常用的是标准类

```kotlin
class 类名 {
    声明类的成员
}

// 类成员包括：构造函数、初始化代码块、成员函数、属性、内部类和嵌套类、对象表达式声明
```

#### 属性

##### 属性声明

语法格式：

```kotlin
var | val 属性名 [ : 数据类型] [ = 属性初始化 ]
	[getter 访问器]
	[setter 访问器]

// get()和set()方法内部提供了一个关键字 field，用于获取当前的变量值
var money: Int = 100
	get() {
        return field * 10
    }
	// 或者直接写 get() = field * 10
	set(value) {
            println("I changed")
            field = value
    }
fun main(){
    println(money)
    money = 250
    println(money)
}
/*
1000
I changed
2500
*/
```

> get()和set()方法只有顶层变量才有。



代码示例：

```kotlin
class Employee {
    var no: Int = 0
    // 可空类型
    var job: String? = null
    var firstName: String = "Tony"
    var lastName: String = "Guan"
    var fullName: String
    	// 重写getter访问器
        get() {
            return "$firstName.$lastName"
        }
    	// 重写setter访问器
        set(value) {
            val name = value.split(".")
            firstName = name[0]
            lastName = name[1]
        }

    var salary: Double = 0.0
        set(value) {			// 不接受负值，如果输入负数的话，默认为0.0
            if (value >= 0.0)
                field = value
        }
}

fun main() {
    val emp = Employee()
    println(emp.fullName)
    emp.fullName = "Tom.Guan"
    println(emp.fullName)

    emp.salary = -10.0
    println(emp.salary)
    emp.salary = 10.0
    println(emp.salary)
}
/*
Tony.Guan
Tom.Guan
0.0
10.0
*/
```



##### 延迟初始化属性

```kotlin
// 代码示例

// 员工类
class Employee {
    ...
    var dept = Department()
}

// 部门类
class Department {
    var no: Int = 0
    var name: String = ""
}

// 主函数
fun main() {
    val emp = Employee()
    ...
    println(emp.dept)
}
```

场景：在创建Employee对象时，需要实例化Employee属性，包括其中的dept属性。但是一个新员工还没有分配部门的话，这个dept属性就没有实例化的必要了，实例化还会占用内存。对于这种情况，在Kotlin可以将属性设置为延迟初始化(`lateinit`)，代码修改后如下：

```kotlin
// 员工类
class Employee {
    lateinit var dept: Department
}

// 部门类
class Department {
    var no: Int = 0
    var name: String = ""
}

// 主函数
fun main() {
    val emp = Employee()

    emp.dept = Department()
    println(emp.dept)
}
```

> 注意：延迟初始化的属性要求：==不能是可空的==、只能用==var声明==、==lateinit关键词应该在var的前面==



##### 委托属性

委托属性，用关键字`by`声明

```kotlin
import kotlin.reflect.KProperty

class User {
    /* 声明委托属性，by是委托运算符，Delegate是属性name委托的对象
    通过by运算符，属性name的setter访问器被委托给Delegate对象的setValue函数，
    属性name的getter访问器被委托给Delegate对象的getValue函数。
    此时的Delegate对象不必实现任何接口，只需要实现getValue和setValue函数即可
    */
    var name: String by Delegate()
}

class Delegate {
    /*
    这两个函数都有operator关键字修饰，operator所修饰的函数是运算符重载函数
    */
    operator fun getValue(thisRef: Any, property: KProperty<*>): String = property.name

    operator fun setValue(thisRef: Any?, property: KProperty<*>, value: String){
        println(value)
    }
}

fun main() {
    val user = User()
    // 给name赋值会调用setValue函数
    user.name = "Tom"
    // 读取name会调用geyValue函数
    println(user.name)
}
/*
Tom
name
*/
```



##### 惰性加载属性

实际开发中很少使用自己声明的委托属性，而是使用Kotlin标准库中提供的一些委托属性，如==惰性加载属性==和==可观察属性==

惰性加载属性与延迟初始化属性类似，只有第一次访问该属性时才进行初始化，但也有不同：

- 惰性加载属性使用`lazy`函数声明委托属性
- 惰性加载属性必须用`val`声明

```kotlin
// 示例代码
open class Employee{
    var no: Int = 0
    var firstName: String = "Tony"
    var lastName: String = "Hu"

    // 惰性加载
    val fullName: String by lazy {
        "$firstName.$lastName"
    }
    // 延迟初始化
    lateinit var dept: Department
}

class Department{
    var no: Int = 0
    var name: String = ""
}

fun main() {
    val emp = Employee()
    println(emp.fullName)

    val dept = Department()
    dept.no = 20
    emp.dept = dept
}
/*
Tony.Hu
*/
```



##### 可观察属性

委托对象监听属性的变化，当属性变化时委托对象会被触发

```kotlin
import kotlin.properties.Delegates

open class Employee{
    var no: Int = 0
    var firstName: String = "Tony"
    var lastName: String = "Hu"

    val fullName: String by lazy {
        "$firstName.$lastName"
    }
    lateinit var dept: Department
}

class Department{
    var no: Int = 0
    /*
    name为委托属性，by关键字后面的Delegates.observable函数有两个参数：
    第一个参数是委托属性的初始化值，第二个参数是属性变化事件的响应器，响应器是函数类型，
    具体调用时可使用Lambda表达式作为实际参数。
    在Lambda表达式中有三个参数，其中p是属性，oldValue是属性的旧值，newValue为属性的新值
    */
    var name: String by Delegates.observable("<无>"){
        p,oldValue,newValue ->
        println("$oldValue -> $newValue")
    }
}

fun main() {
    val dept = Department()
    dept.no = 20
    dept.name = "技术部"
    dept.name = "市场部"
}
```



#### 扩展

扩展机制，是指在原始类型的基础上添加新功能，是一种`轻量级的继承机制`。

原始类型称为“接收类型”

> Java程序员不太擅长使用扩展。
>
> 在设计基于Kotlin语言的程序时，优先使用扩展

##### 扩展函数

语法：

```kotlin
fun 接收类型.函数名(参数列表): 返回值类型 {
    函数体
    return 返回值
}
```

可见扩展函数和普通函数的区别是在函数名前面加上“接收类型”。

接收类型可以是Kotlin的任何数据类型，包括基本数据类型和引用数据类型

```kotlin
// 基本数据类型扩展
fun Double.interestBy(intersetRate: Double): Double{
    return this * intersetRate
}

// 自定义账户类
class Account {
    var amount: Double = 0.0
    var owner: String = ""
}

// 账户类扩展函数
fun Account.interestBy(intersetRate: Double): Double {
    return this.amount * intersetRate
}

fun main() {
    val interest1 = 10_000.00.interestBy(0.0668)
    println("利息1： $interest1")

    val account = Account()
    val interest2 = account.interestBy(0.0668)
    println("利息2： $interest2")
}

/*
利息1： 668.0
利息2： 0.0
*/
```

> 扩展函数机制，可以使得我们将第三方库强行进行扩展，来方便我们的操作
>
> this用来表示当前类型的接收对象



##### 扩展属性

在接收类型上扩展属性，语法如下：

```kotlin
var | val 接收类型.属性名 [ : 数据类型]
	[getter 访问器]
	[setter 访问器]
```

> Kotlin扩展属性没有支持字段，所以扩展属性不能初始化，不能使用field变量



```kotlin
class Department {
    var no: Int = 0
    var name: String = ""
}

var Department.desc: String
    get() {
        return "Department [no=${this.no}, name=${this.name}]"
    }

    set(value) {
        println(value)
    }

val Int.errorMessage: String
    get() = when (this){
        -7 -> "没有数据"
        -6 -> "日期没有输入"
        else -> ""
    }

fun main() {
    val message = (-7).errorMessage
    println("Error Code: -7， Error Message: $message")

    val dept = Department()
    dept.name = "程序员"
    dept.no = 100
    println(dept.desc)
}
```

> 类中没有定义这个属性，但是可以扩展一个属性出来，这个还是很有用的。比如实体类的字段不能够直接拿来当返回值用，必须写一个DTO，把id转换为对应的名字，如teacher_id转换为老师名称。有了扩展属性之后，我就不需要另外创建一个类了。



##### ==“成员优先”原则==

如果扩展属性或扩展函数时，接收类型已经有相同的属性和函数的话，那么在调用属性和函数时，调用的是接收类型的成员属性和函数。即“成员优先”。

```kotlin
class Department {
    var no: Int = 0
    var name: String = ""
    var desc: String = "成员：${no} - ${name}"

    fun display(): String {
        return "成员：[no = ${this.no}, name = ${this.name}]"
    }
}

// 某种程度上属于无效扩展
var Department.desc: String
    get() {
        return "扩展： [no=${this.no}, name=${this.name}]"
    }

    set(value) {
        println(value)
    }

// 某种程度上属于无效扩展
fun Department.display(): String{
    return "扩展：[no = ${this.no}, name = ${this.name}]"
}

// 有效扩展
fun Department.display(f: String): String{
    return "扩展：f,[no = ${this.no}, name = ${this.name}]"
}


fun main() {
    val dept = Department()
    dept.name = "taKiKu"
    dept.no = 100
    println(dept.desc)
    println(dept.display())
    println(dept.display("kimi no na wa"))
}

/*
成员：0 - 
成员：[no = 100, name = taKiKu]
扩展：f,[no = 100, name = taKiKu]
*/
```

> 没搞懂第一条打印语句，我应该已经给对象的属性重新赋值了，为什么这里的desc没有受到影响？



##### 定义中缀运算符

中缀运算符本质上是一个函数，通过`infix`关键字来修饰的函数，而且该函数==只能有一个参数==，==不能是顶层函数==，==只能为成员函数或扩展函数==

中缀运算符可以**让函数像运算符一样使用**

```kotlin
infix fun Double.interestBy(rate: Double): Double {
    return this * rate
}

class Department {
    var no: Int = 10

    // 定义中缀函数rp
    infix fun rp(times: Int) {
        // repeat函数是Kotlin标准库提供的内联函数，参数times为重复的次数
        repeat(times) {
            println(no)
        }
    }
}

fun main() {
    val in1 = 10_000.00.interestBy(0.0668)
    println("利息1：$in1")

    // 中缀运算符
    val in2 = 10_000.00 interestBy 0.0668
    println("利息2：$in2")

    
    val dept = Department()
    // 中缀运算符
    dept rp 3
}

/*
利息1：668.0
利息2：668.0
10
10
10
*/
```

> 中缀运算符，我觉得比较好的一个地方就是可以少打`.`和`()`，能够提高写代码的速度，我个人是有点不喜欢这两个符号的，感觉有点拖沓。如果这两个字符是句子的最后一个字符的话倒另当别论，但是它们一般都不是，敲完`.`后还要看看是什么属性什么函数，然后再接着写；敲完`()`后又要左移传参



```kotlin
repeat(times: Int, action:(Int) -> (Unit))
// 该函数可以反复执行action，times参数是执行的次数，action是最后一个参数，可以用尾随Lambda表达式的形式调用
```



#### 构造函数

##### 主构造函数

主构造函数涉及到两个关键字`constructor`和`init`。

主构造函数在类头中或类名的后面声明，使用关键字`constructor`声明

```kotlin
class Rectangle constructor(w: Int, h: Int) {
    var width: Int
    var height: Int
    var area: Int
    
    // 初始化代码块，可以执行一些对象创建后的初始操作
    init {
        width = w
        height = h
        area = w * h
    }
}
```

==但是==，像上面这样写的话会比较臃肿，为什么不专门把属性当作参数呢？于是Kotlin的设计者进行了优化：

```kotlin
// 优化后
class Rectangle constructor(var width: Int, var height: Int) {
   var area: Int
   init {
       area = width * height
   }
}
// Kotlin编译器会根据参数列表自动生成相应的属性
// 如果所有属性都在主构造函数中初始化，则init代码块可以省略
// 比如
class User constructor(val name: String, var password: String)


// 如果主构造函数没有注解或可见性修饰符，constrcutor关键字可忽略，如：
class User (val name: String, var password: String)


// 下面这个不能忽略，因为有private可见性修饰符
class User private constructor(val name: String, var password: String)
```

主构造函数也是函数，那么它也是可以给参数设定默认值的

```kotlin
class User (var name: String = "takiku", var password: String = "mitsuha")
```



##### 次构造函数

对于主构造函数，只能有一个，而且初始化时只有init代码块，这就不够灵活，此时就可以使用次构造函数，用关键字`constructor`声明

```kotlin
class Rectangle constructor(var width: Int, var height: Int) {
    var area: Int
    init {
        area = width * height
    }
    
    // 次构造函数，需要调用主构造函数初始化部分的属性
    // this(width, height)就是调用当前对象的主构造函数
    constructor(width: Int, height: Int, area: Int):this(width, height) {
        this.area = area
    }
    
    // 次构造函数，this(200, 100)调用当前对象的主构造函数
    constructor(area: Int): this(200,100) {
        this.area = area
    }
    
}

fun main() {
    val rect1 = Rectangle(100, 90)
    val rect2 = Rectangle(10, 9, 900)
    val rect3 = Rectangle(20000)
}
```

> 次构造函数的参数，最后都是给主构造函数服务的。可以认为次构造函数是从主构造函数中拆分出来的



##### 默认构造函数

即无参构造函数

```kotlin
class User {
    val username: String?
    val password: String?
    
    init {
        username = null
        password = null
    }
}

fun main() {
    // 无参构造函数
    val user = User()
}
```



#### 可见性修饰符

| 可见性 |  修饰符   |  类成员声明  |   顶层声明   |        说明        |
| :----: | :-------: | :----------: | :----------: | :----------------: |
|  公有  |  public   | 所有地方可见 | 所有地方可见 |     默认修饰符     |
|  内部  | internal  |  模块中可见  |  模块中可见  |  不同于Java中的包  |
|  保护  | protected |  子类中可见  |              | 顶层声明中不能使用 |
|  私有  |  private  |   类中可见   |  文件中可见  |                    |



#### 数据类

有时候需要一种数据容器在各个组件之间传递。之前只有参数或属性来保存数据，但是该不够完善，最好重写Any的三个函数：

- equals：比较其他对象是否与当前对象“相等”，==运算符重载equals函数
- hashCode：返回该对象的哈希码值，可以==提高对HashTable和HashMap对象的访问效率==
- toString：返回该对象的字符串表示



##### 声明数据类

在Java中可能真的需要自己手动重写，或者需要借助`@lombook`注解，但是在Kotlin中很简单，只需要在类头class前面加一个`data`就可以了

```kotlin
data class User(val name: String, var password: String)
// 自动重写这三个方法
```

> 注意：data类的参数列表必须要有var或val，而普通类可以省略



```kotlin
data class User(var name: String, var password: String)

fun main() {
    val user1 = User("takiku", "123456")
    val user2 = User("takiku", "123456")

    println(user1 == user2)
    println(user1.toString())
    println(user1.hashCode())
    println(user2.hashCode())
}
/*
true
User(name=takiku, password=123456)
-94101042
-94101042
*/
```



##### copy函数

数据类提供了一个copy函数，通过copy函数可以复制一个新的数据类对象，完全一样。

```kotlin
data class User(var name: String, var password: String)

fun main() {
    val user1 = User("takiku", "123456")
    val user2 = user1.copy()

    println(user1 == user2)
    println(user1.hashCode())
    println(user2.hashCode())
}
```



##### 解构数据类

解构数据类就是将一个对象的属性值，用单独的变量来接收

```kotlin
data class User(var name: String, var password: String)

fun main() {
    val user = User("Tony", "123")
    // name和password都解构
    val(name, password) = user
    println(name)
    println(password)
    
    // 不解构password
    val(name1, _) = user
    println(name1)
}
/*
Tony
123
Tony
*/
```



#### 枚举类

##### 声明枚举类

使用`enum`和`class`两个关键字来声明枚举类，语法：

```kotlin
enum class 枚举名 {
    枚举常量列表
}

// 示例
enum class WeekDays {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY
}

fun main() {
    val day = WeekDays.FRIDAY
    println(day)
}
/*
FRIDAY
*/
```



##### 枚举类构造函数

这里有个==不能省略分号==的特殊情况



##### 枚举类常用属性和函数



#### 嵌套类

##### 一般嵌套类

允许在一个类（外部类）的内部声明另外一个类（内部类）

```kotlin
class View {
    val x = 20

    class Button {
        fun onclick() {
            println("onClick...")
        }
    }

    fun test() {
        val button = Button()
        button.onclick()
    }
}

fun main() {
    val button = View.Button()
    button.onclick()

    val view = View()
    view.test()
}
```



##### 内部类



#### Object关键字

`object`关键字在声明一个类的同时创建这个类的对象，具体有三个方面的应用：

- 对象表达式
- 对象声明
- 伴生对象



##### 对象表达式

对象表达式用来替代Java中的匿名内部类，就是在声明一个匿名类的同时创建匿名类的对象

```kotlin
class View {
    fun handler(listener: OnClickListener) {
        listener.onClick()
    }
}

// 声明一个接口
interface OnClickListener {
    fun onClick()
}

fun main() {
    var i = 10
    val v = View()
    // 对象表达式作为函数参数
    v.handler(object : OnClickListener {
        override fun onClick() {
//            TODO("Not yet implemented")
            println("对象表达式作为函数参数...")
            println(++i)
        }
    })
}
/*
对象表达式作为函数参数...
11
*/
```

> 对象表达式可以实现接口，也可以继承具体类或抽象类，但是我没发现有什么具体用处，所以没仔细学



##### 对象声明

这里提到了单例设计模式。

在Kotlin中使用单例设计模式非常简单

```kotlin
interface DAOInterface {
    // 插入数据
    fun create(): Int

    // 查询所有数据
    fun findAll(): Array<Any>?
}

// 声明UserDAO单例对象
object UserDAO : DAOInterface {
    // 保存所有数据
    private var datas: Array<Any>? = null

    override fun findAll(): Array<Any>? {
        return datas
    }

    override fun create(): Int {
        return 0
    }
}

fun main() {
    UserDAO.create()
    var datas = UserDAO.findAll()
}

// 这个可以联想到在Controller中自动注入Service，两者类似
```



##### 伴生对象

有一个Account（银行账户）类

```kotlin
class Account {
    // 账户金额
    var amount: Double
    // 账户名
    var owner: String
    // 利率
    val interestRate: Double
}
```

这三个属性中，amount和owner是因人而异的，称为“实例属性”，但interestRate每个人都是相同的，称为“静态属性”

**声明伴生对象**

```kotlin
class Account {
    // 账户金额
    var amount = 0.0
    // 账户名
    var owner: String? = null
    
    // 实例函数
    fun messageWith(amt: Double): String {
        // 实例函数可以访问实例属性、实例函数、静态属性和静态函数
        val interest = Account.interestBy(amt)
        return "${owner}的利息是$interest"
    }
    
    
    companion object {
        // 静态属性利率
        var interestRate: Double = 0.0
        
        // 静态函数
        fun interestRate(amt: Double): Double {
            // 静态函数可以访问静态属性和其他静态函数
            return interestRate * amt
        }
        
        // 静态代码块
        init {
            println("静态代码块被调用...")
            
            // 初始化静态属性
            interestRate = 0.0668
        }
    }
    
}
```

`companion`和`object`用来声明伴生对象



##### 伴生对象非省略形式



##### 伴生对象扩展





### 继承和多态



#### kotlin中的继承

有两点跟Java不同：

- 一个类若要是可以被继承的，必须用`open`关键字声明
- 声明不用extends，而用`:`

```kotlin
// Person类，用open修饰，所以可以被继承
open class Person {
    val name: String
    .......
}

// Student类，继承了Person类
class Student : Person() {
    ...
}
```

> 在Kotlin中，类的继承只能是单继承，而多重继承可以通过实现多个接口来实现
>
> 即一个类只能继承一个父类，但是可以实现多个接口



#### 调用父类构造函数



##### 使用子构造函数



##### 使用次构造函数重载



##### 使用参数默认值调用构造函数



#### 重写成员属性和函数



##### 重写成员属性



##### 重写成员函数



#### 多态



##### 使用is和!is进行检查

有时候需要在运行时判断一个对象是否属于某个类型，这时可以使用`is`或`!is`运算符，语法如下：

```kotlin
obj is type
obj !is type
// 其中obj是一个对象，type是一个数据类型


// 示例
fun main() {
    val a = "天上人间"
    println(a is String)
}
/*
true
*/
```



##### 使用as和as?进行类型转换

之前说过数值类型的转换，==引用类型==也可以转换，但是必须是同一棵继承层次树中的引用类型才可以转换（向上转型或者向下转型，不能中间的转）

```kotlin
// Worker类和Student类都继承了Person类
val p1: Person = Student()
val p2: Person = Worker()

val p3 = Person()
val p4 = Student()
val p5 = Worker()

// 测试

// 向上转型
val p41: Person = p4
val p51 = p5 as Person

// 向下转型
val p11 = p1 as Student
val p21 = p2 as Worker

// 这里使用as会发生运行时异常
val p211 = p2 as? Student		
val p111 = p1 as? Worker
val p311 = p3 as? Student
```

> 如果使用as转换会发生运行时异常，则使用as?，会返回空值而不是抛出异常



#### 密封类

如果一个类的子类个数是有限的，那么Kotlin中可以把这种==父类==定义为密封类，使用`sealed`修饰



### 抽象类和接口

抽象类的修饰关键字为`abstract`

接口的关键字为`interface`

如果一个类实现了多个接口的话，用`,`隔开

```kotlin
class A : Any(), InterfaceA, InterfaceB {
    ...
}
```

kotlin中接口方法之间也可以被继承



### 高阶函数和Lambda表达式

#### 函数式编程简介

简而言之：变量可以存函数；一个变量可以当作函数使用

#### 高阶函数

一个函数可以作为另一个函数的参数，或者返回值，那么这个函数就是**高阶函数**



##### 函数类型

体现在参数类型和返回值类型

```kotlin
// 函数类型为：(Double, Double) -> Double
fun S(width: Double, height: Double): Double {
    
}

// 函数类型为：() -> Unit
fun sayHello() {
    println("Hello")
}
```



##### 函数字面量

函数字面量有三种表示：

- 函数引用。引用一个已经定义好的、有名字的函数
- 匿名函数。没有名字的函数
- Lambda表达式。一种匿名函数

```kotlin
// 如需增强可读性，可以给一个函数返回类型取一个别名
typealias twoInOneOut = (Int, Int) -> Int

// caculate()函数的返回值是 => 2个Int输入，1个Int输出的函数
fun calculate(opr: Char): twoInOneOut {
    fun add(a: Int, b: Int): Int {
        return a + b
    }

    fun sub(a: Int, b: Int): Int {
        return a - b
    }

    // 这里的result就是一个函数类型的变量，可以作为一个函数来使用
    val result: (Int, Int) -> Int =
        when (opr) {
            // 采用"双冒号加函数名"的方式引用函数
            '+' -> ::add
            '-' -> ::sub
            '*' -> {
                // 乘法匿名函数
                fun(a: Int, b: Int): Int {
                    return (a * b)
                }
            }
            // 除法Lambda表达式
            else -> { a, b -> (a / b) }
        }
    return result
}

fun main() {
    val f1 = calculate('+')
    println(f1(10, 5))
    val f2 = calculate('-')
    println(f2(10, 5))
    val f3 = calculate('*')
    println(f3(10, 5))
    val f4 = calculate('/')
    println(f4(10, 5))
}
```



##### 函数作为另一个函数返回值使用

```kotlin
// 计算矩形面积的函数
fun rectArea(width: Double, height: Double) = width * height
// 计算三角形面积的函数
fun triArea(bottom: Double, height: Double) = 0.5 * bottom * height

// 高阶函数（太简洁了），函数类型为：(Double, Double) -> Double
fun getArea(type: String) = when (type) {
    // 如果 type == "rect"，则返回 rectArea()
    "rect" -> ::rectArea
    else -> ::triArea
}
fun main() {
    var area: (Double, Double) -> Double = getArea("tri")
    println("底为 10，高为 20，计算三角形面积: ${area(10.0, 15.0)}")

    area = getArea("rect")
    println("宽为 10，高为 15，计算矩形面积: ${area(10.0, 15.0)}")
}
/*
底为 10，高为 20，计算三角形面积: 75.0
宽为 10，高为 15，计算矩形面积: 150.0
*/
```



##### 函数作为参数使用

```kotlin
fun rectArea(width: Double, height: Double) = width * height

fun triArea(bottom: Double, height: Double) = 0.5 * bottom * height

fun getArea(type: String) = when (type) {
    "rect" -> ::rectArea
    else -> ::triArea
}

fun getAreaByFunc(funcName: (Double, Double) -> Double, a: Double, b: Double) = funcName(a, b)

// ::triArea是函数引用
fun main() {
    var area: (Double, Double) -> Double = getArea("tri")
    println("底为 10，高为 20，计算三角形面积: ${getAreaByFunc(::triArea, 10.0, 15.0)}")

    area = getArea("rect")
    println("宽为 10，高为 15，计算矩形面积: ${getAreaByFunc(::rectArea, 10.0, 15.0)}")
}

```



#### Lambda表达式

Lambda表达式是一种匿名函数，可以作为表达式、函数参数和函数返回值使用。

==Lambda表达式的运算结果是一个函数==

##### 语法

```kotlin
{
    参数列表 ->
    	Lambda体
}
```

Lambda表达式和函数的区别：

- Lambda表达式的参数列表不需要括号
- Lambda表达式用==箭头符号==区别参数列表和Lambda体，而不是花括号
- Lambda表达式不需要声明返回类型
- Lambda表达式如果有return语句，则return后面的表达式就是返回值
- Lambda表达式如果没有return语句，则最后一个表达式为返回值

```kotlin
// 重构高阶函数一节中函数字面量的代码示例
fun calculate(opr: Char): (Int, Int) -> Int = when(opr){
    '+' -> {a: Int, b: Int -> a + b}
    '-' -> {a: Int, b: Int -> a - b}
    '*' -> {a: Int, b: Int -> a * b}
    else -> {a: Int, b: Int -> a / b}
}

fun main() {
    val f1 = calculate('+')
    println(f1(10, 5))
    val f2 = calculate('-')
    println(f2(10, 5))
    val f3 = calculate('*')
    println(f3(10, 5))
    val f4 = calculate('/')
    println(f4(10, 5))
}

/*
15
5
50
2
*/
```



##### 使用Lambda表达式

上面一个例子是Lambda表达式作为返回值使用，下面介绍Lambda表达式作为参数使用

```kotlin
fun calculatePrint(n1: Int, n2: Int, opr: Char, funN: (Int, Int) -> Int) = println("$n1 $opr $n2 = ${funN(n1, n2)}")

fun main() {
    // Lambda实参 建议从括号中移出
    calculatePrint(10, 5, '+') { a: Int, b: Int -> a + b }

    // 用变量名也行
    calculatePrint(10, 5, '+', funN = { a: Int, b: Int -> a + b })

    calculatePrint(10, 5, '+', { a: Int, b: Int -> a + b })
}

/*
10 + 5 = 15
10 + 5 = 15
10 + 5 = 15
*/
```



##### Lambda表达式简化写法

###### 参数类型推导简化

```kotlin
// 标准Lambda表达式
{a: Int, b: Int -> a + b}

// 简化版
{a, b -> a + b}
```

> 简化原因：Kotlin可以推导出参数a和b的类型

```kotlin
// 简化后的calculate函数
fun calculate(opr: Char): (Int, Int) -> Int = when (opr) {
    '+' -> { a, b -> a + b }
    '-' -> { a, b -> a - b }
    '*' -> { a, b -> a * b }
    else -> { a, b -> a / b }
}
```

> 太简洁了



###### 使用尾随Lambda表达式

Lambda表达式可以作为函数的参数传递，如果Lambda表达式很长，就会影响程序的可读性。

如果一个函数的最后一个参数是Lambda表达式，那么这个Lambda表达式可以放在函数括号之后

```kotlin
fun calculatePrint(n1: Int, n2: Int, opr: Char, funN: (Int, Int) -> Int) = println("$n1 $opr $n2 = ${funN(n1, n2)}")
fun calculatePrint1(funN: (Int, Int) -> Int) = println("10 + 5 = ${funN(10, 5)}")

fun main() {
    // 尾随Lambda表达式
    calculatePrint(10, 5, '+') { a: Int, b: Int -> a + b }

    // 尾随Lambda表达式
    calculatePrint1() { a, b -> a + b }

    // 如果只有Lambda表达式一个参数，则可以省略括号
    calculatePrint1 { a, b -> a + b }
}

/*
10 + 5 = 15
10 + 5 = 15
10 + 5 = 15
*/
```

> 尾随Lambda表达式容易为认为是函数声明



###### 省略参数声明

如果Lambda表达式的参数只有一个，并且可以根据上下文环境推导出它的数据类型，那么可以省略这个参数声明，在Lambda体中使用==隐式参数`it`==替代Lambda表达式的参数

```kotlin
import kotlin.reflect.typeOf

fun reserveAndPrint(str: String, funN: (String) -> String) {
    val result = funN(str)
    println(result)
}

fun main() {
    // 标准形式
    reserveAndPrint("hello", { s -> s.reversed() })
    // 简化形式
    reserveAndPrint("hello") { s -> s.reversed() }

    // 省略参数，使用it
    reserveAndPrint("hello", { it.reversed() })
    // 简化形式
    reserveAndPrint("hello") { it.reversed() }


    // 不能省略参数声明，res1未被指定数据类型，无法推导出Lambda表达式的参数数据类型
    val res1 = { a: Int -> println(a) }
    println("res1的函数类型为：${res1}")
    res1(12)
    // 可以省略参数声明，因为这里的res2被指定了数据类型为(Int) -> Unit，所以可以推出a的数据类型
    val res2: (Int) -> Unit = { println(it) }
    res2(30)
}
/*
olleh
olleh
olleh
olleh
res1的函数类型为：Function1<java.lang.Integer, kotlin.Unit>
12
30
*/
```



如果lambda表达式传入多个值，但是只希望使用其中的一个，则可以使用`_`来忽略传入的值：

```kotlin
fun main() {
    // 忽略第一个实参
    val func: (String, String) -> Unit = { _, b ->
        print(b)
    }

    // 执行函数
    func("Hello", "World")
}
// World
```



it隐式变量由Kotlin编译器生成的，它的使用有两个前提：

- Lambda表达式只有一个参数
- 可以根据上下文推导出参数类型

##### Lambda表达式与return语句

```kotlin
fun sum(vararg num: Int): Int {
    var total = 0
    num.forEach {
        println("num读出：$it")
        // 这里的return只是结束本轮的循环，如果没有指名标签，那么默认就是函数名，比如这里的forEach
        if(it == 10) return@forEach
        println("num取出：$it")
        total += it
    }
    return total;
}

fun main() {
    val n = sum(1,2,10,3)
    println(n)

    val add = label@{
        val a = 1
        val b = 2
        // return后，返回10，不会执行到a + b
        return@label 10
        a + b
    }
    // 调用函数add()，打印其返回值
    println(add())
    // 打印函数add()的类型
    println(add)
}
/*
num读出：1
num取出：1
num读出：2
num取出：2
num读出：10
num读出：3
num取出：3
6
10
Function0<java.lang.Integer>
*/
```



#### 闭包与捕获变量

闭包是一种特殊的函数，它可以访问函数体之外的变量，这个变量和函数一同存在，即使已经离开了它的原始作用域也不例外。这种特殊函数一般是局部函数、匿名函数或Lambda表达式。

闭包可以访问函数体之外的变量，这个过程称为==捕获变量==

```kotlin
// 全局变量
var value = 10
fun main() {
    // 局部变量
    var localValue = 20
    // 这里的闭包是捕获value和localValue的Lambda表达式
    val result = { a: Int ->
        value++
        localValue++
        val c = a + value + localValue
        println(c)
    }
    result(30)
    println("localValue = $localValue")
    println("value = $value")
}
/*
62
localValue = 21
value = 11
*/
```

> 在Java中，Lambda表达式捕获局部变量时，局部变量只能是final，且在Lambda表达式中只能读取局部变量，而不能修改局部变量。==在Kotlin中没有这种限制，可以修改和读取局部变量==

闭包捕获变量后，这些变量被保存在一个特殊的容器中。即使声明这些变量的原始作用域已经不存在，闭包体中仍然可以访问这些变量

```kotlin
fun makeArray(): (Int) -> Int {
    var ary = 0
    // 局部函数捕获变量
    fun add(element: Int): Int {
        ary += element
        return ary
    }

    return ::add
}

fun main() {
    val f1 = makeArray()
    println("---f1---")
    // 累加ary变量
    println(f1(10))
    // 累加ary变量
    println(f1(20))
    // 累加ary变量
    println(f1(30))
}
/*
---f1---
10
30
60
*/
```



<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240503112730971.png" alt="image-20240503112730971" style="zoom:80%;" />

> 这里的ary变量带了星号，不简单。好像是环境变量了？
>
> 每次调用f1时，ary变量的作用域已经不存在，但是ary变量值都能够被保持



改用匿名函数：

```kotlin
fun makeArray(): (Int) -> Int {
    var ary = 0
    // 匿名函数捕获变量
    return fun(element: Int): Int {
        ary += element
        return ary
    }
}

fun main() {
    val f1 = makeArray()
    println("---f1---")
    println(f1(10))
    println(f1(20))
    println(f1(30))
}
```

改用Lambda表达式：

```kotlin
fun makeArray(): (Int) -> Int {
    var ary = 0

    // Lambda表达式
    return { element ->
        ary += element
        ary
    }
}

fun main() {
    val f1 = makeArray()
    println("---f1---")
    println(f1(10))
    println(f1(20))
    println(f1(30))
}
```



#### 内联函数

Lambda表达式在编译时被编译为一个匿名类，每次调用函数时都会创建一个对象，如果被函数反复调用则创建很多对象，会带来运行时的额外开销。为了解决这个问题，Kotlin中可以将这种函数声明为内联函数。**内联函数可以给高阶函数带来比较好的优化**

> 内联函数在编译时不会生成函数调用代码，而是用函数体中实际代码替换每次函数

```kotlin
// 这是一个内联函数
fun main() {
    test()
}

inline fun test() {
    println("内联函数")
}

// 反编译后的结果大概为，不是函数调用，而是直接替换函数
fun main() {
    println("内联函数")
}

inline fun test() {
    println("内联函数")
}
```





##### 自定义内联函数

Kotlin标准库中提供了很多常用的内联函数，开发人员可以自定义内联函数，但是==如果函数参数不是函数类型==，不能接收Lambda表达式，那么==这种函数一般不声明为内联函数==。

用关键字`inline`声明内联函数

```kotlin
inline fun calculatePrint(funN: (Int, Int) -> Int) {
    println("${funN(10, 5)}")
}

fun main() {
    calculatePrint() { a, b -> a + b }
    calculatePrint() { a, b -> a - b }
}
/*
15
5
*/
```



##### 使用let函数

`let`函数的功能见注释

```kotlin
fun square(num : Int): Int = num * num

fun main() {
    val n1: Int? = 10
    val n2: Int? = null
    // 如果变量n1不为空，则执行后面的Lambda表达式，否则不执行。这里n1不为空，所以执行
    n1?.let { n -> println(square(n)) }
    // 如果变量n2不为空，则执行后面的Lambda表达式，否则不执行。这里n2为空，所以不执行
    n2?.let { println(it) }
}

/*
100
*/
```



##### 使用with和apply函数

当需要对一个对象设置多个属性或调用多个函数时，可以使用`with`或`apply`函数。

```kotlin
import java.awt.BorderLayout
import javax.swing.JButton
import javax.swing.JFrame
import javax.swing.JLabel

class MyFrame(title: String): JFrame(title) {
    init {
        // 创建标签
        val label = JLabel("Label")

        // 创建Button1
        val button1 = JButton()
        button1.text = "Button1"
        button1.toolTipText = "Button1"

        // 注册事件监听器，监听Button1单击事件
        button1.addActionListener{
            label.text = "单击Button1"
        }

        // 创建Button2
        val button2 = JButton().apply {
            text = "Button2"
            toolTipText = "Button2"

            // 注册事件监听器，监听Button2单击事件
            addActionListener{label.text = "单击Button2"}

            // 添加Button2到内容面板
            contentPane.add(this, BorderLayout.SOUTH)
        }

        with(contentPane) {
            // 添加标签到内容面板
            add(label, BorderLayout.NORTH)

            // 添加Button1到内容面板
            add(button1,BorderLayout.CENTER)
            println(height)
            println(this.width)
        }

        // 设置窗口大小
        setSize(350,120)

        // 设置窗口可见
        isVisible = true
    }
}

fun main() {
    MyFrame("MyFrame")
}
```



> 比较Button1和Button2，可以明显地看出：给Button1的属性赋值时，必须通过对象调用，比如button1.text = "Button1"，好比是Java中的setter()方法，特别累赘
>
> 而给Button2则不同，直接在一个代码块里写好就可以了：
>
> ```kotlin
>         val button2 = JButton().apply {
>             text = "Button2"
>             toolTipText = "Button2"
>         // 注册事件监听器，监听Button2单击事件
>         addActionListener{label.text = "单击Button2"}
> 
>         // 添加Button2到内容面板
>         contentPane.add(this, BorderLayout.SOUTH)
>         }
> ```

这里的button2变量其实可以不用声明出来，因为用了隐式变量it

```kotlin
        // 创建Button2
        JButton().apply {
            text = "Button2"
            toolTipText = "Button2"

            // 注册事件监听器，监听Button2单击事件
            addActionListener { label.text = "单击Button2" }

            // 添加Button2到内容面板
            contentPane.add(this, BorderLayout.SOUTH)
        }

```



### 泛型

泛型可以最大限度地重用代码、保护类型的安全以及提高性能。

泛型中的参数类型可以是任何大写或小写的英文字母。

#### 声明泛型函数

```kotlin
// 泛型函数
private fun <U> isEquals(a: U, b: U): Boolean = a == b
//private fun <T> isEquals(a: T, b: T): Boolean = a == b

// 使用泛型函数
fun main() {
    println(isEquals(1,5))
    println(isEquals(1,1))
    println(isEquals(1.0,5.0))
    println(isEquals(5.0,5.0))
}
```



#### 多类型参数

可以同时声明多种类型的泛型，用`,`隔开

```kotlin
private fun <T, U> callYou(name: T, age: U) = println("你好啊，$age 岁的 $name")

fun main() {
    println(callYou("胡桃", 19))
    println(callYou("鸡坤", 2.5))
}
/*
你好啊，19 岁的 胡桃
kotlin.Unit
你好啊，2.5 岁的 鸡坤
kotlin.Unit
*/
```



#### 泛型约束

声明泛型函数的例子中，并不是所有的类型都具有可比性，所以必须限定泛型的类型。

比如Int和Double等数字类型都继承了Number类，所以可以做个限定：

```kotlin
private fun <U:Number> isEquals(a: U, b: U): Boolean = a == b
//private fun <T> isEquals(a: T, b: T): Boolean = a == b

fun main() {
    println(isEquals(1,5))
    println(isEquals(1,1))
    println(isEquals(1.0,5.0))
    println(isEquals(5.0,5.0))
//    println(isEquals("hutao","hutao"))
}
```

不规范的就会报错：

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240503144317297.png" alt="image-20240503144317297" style="zoom:80%;" />

字符串理应也可以用来比较，所以用Number类限定泛型还是有缺陷的，这里就可以将泛型类型限定为`Comparable<T>`接口，所有可比较的对象都实现`Comparable<T>`，`Comparable<T>`本身也是泛型类型

```kotlin
import java.util.Date

private fun <U:Comparable<U>> isEquals(a: U, b: U): Boolean = a == b

fun main() {
    // 比较数字
    println(isEquals(1,1))
    println(isEquals(1.0,5.0))
    // 比较字符串
    println(isEquals("HuTao","HuTao"))
    // 比较日期
    println(isEquals(Date(),Date()))
}
/*
true
false
true
true
*/
```



#### 可空类型参数

如果没有函数泛型约束的话，可以传递任何类型的参数，包括可空和非空数据。

如果不想函数接收到任何可空类型数据，可以限定为`Any`。默认为`Any?`

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240503145527272.png" alt="image-20240503145527272" style="zoom:80%;" />



#### 泛型属性

泛型属性一定是扩展属性

```kotlin
// 获得第一个元素
val <T> ArrayList<T>.first: T?
    get() = if(this.size > 1) this[0] else null

// 获得第二个元素
val <T> ArrayList<T>.second: T?
    get() = if(this.size > 2) this[1] else null

fun main() {
    val array1 = ArrayList<Int>()
    println(array1.first)
    println(array1.second)

    val array2 = arrayListOf("A", null, "C", "D")
    println(array2.first)
    println(array2.second)
}

/*
null
null
A
null
*/
```



#### 泛型类（待补充）





#### 泛型接口（待补充）





### 数组和集合
