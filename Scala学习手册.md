### 第一部分 Scala 核心
### 第1章 可拓展语言概述
### 第2章 处理数据：字面量、值、变量和类型
### 第3章 表达式和条件式
### 第4章 函数
### 第5章 首类函数
### 第6章 常用集合
### 第7章 更多集合
### 第二部分 面向对象 Scala
### 第8章 类
### 第9章 对象、Case 类和 Trait
### 第10章 高级类型

### 第1章 可拓展语言概述

* Scala 是可拓展语言（SCAlabel Language）的缩写

#### 安装 Scala
#### 使用 Scala REPL


### 第2章 处理数据：字面量、值、变量和类型

* 字面量（字面数据）：直接出现在源代码中的数据
* 值：一个不可变的、有类型的存储单元，可以在定义时指定数据，不过不允许重新赋值
* 变量：一个可变的、有类型的存储单元，可以在定义时指定数据，而且可以在任意时间重新赋值
* 类型：所处理数据的种类，是对数据的一个定义或分类，包含方法来处理数据
* 如果某些值和变量不再使用，将由 Java 虚拟机的垃圾回收系统自动撤销其中存储的数据，不能手动撤销，也不需要这样做

```
// 值
scala> val x: Int = 5
x: Int = 5

scala> x
res0: Int = 5

scala> x * 2
res1: Int = 10

scala> x / 5
res2: Int = 1

scala> res0 * res1
res3: Int = 50

// 变量
scala> var a: Double = 2.72
a: Double = 2.72

scala> a = 355.0 / 113.0
a: Double = 3.1415929203539825

scala> a = 5
a: Double = 5
```

#### 值

* 不可变的、有类型的存储单元，也是存储数据的默认方法

```
// 语法：定义值
val <identifier>[: <type>] = <data>

scala> val x: Int = 20
x: Int = 20

scala> val greeting: String = "Hello, World"
greeting: String = Hello, World

scala> val atSymbol: Char = '@'
atSymbol: Char = @

// 类型推导：定义值的时候，指定类型是可选的，可以根据赋值来推导值的类型，无需显式指定
scala> val x = 20
x: Int = 20

// 类型推导不会覆盖显式类型，如果定义一个值时指定了类型，而这个类型与初始值不兼容，就会产生编译错误
scala> val x: Int = "Hello"
<console>:7: error: type mismatch;
 found   : String("Hello")
 required: Int
       val x: Int = "Hello"
```

#### 变量

* 一个唯一的标识符，对应一个已分配或保留的内存空间，可以存储值，还可以获取值
* 只要保留了内存空间，就可以反复地赋新值，因此这个内存空间的内容是动态的（或可变的）

```
// 语法：定义变量
var <identifier>[: <type>] = <data>

scala> var x = 5
x: Int = 5

scala> x = x * 4
x: Int = 20

// 变量可以重新赋值，但是不能改变为它指定的类型，所以不能将一个变量重新赋值为类型不兼容的数据
scala> var x = 5
x: Int = 5

scala> x = "what's up?"
<console>:8: error: type mismatch;
 found   : String("what\'s up?")
 required: Int
       x = "what's up?"
           ^
```

#### 命名

* 值和变量名应当以小写字母开头，其余单词的首字母大写（小驼峰命名）
* 类型和类应当以大写字母开头，其余单词首字母大写（大驼峰命名）

#### 类型

* 数值类型和非数值类型，可以用来定义值和变量
* 与 Java 和 C 不同，Scala 中没有基本类型的概念，Scala 只支持它自己的整数类 Int

核心数值类型：（上面级别低，从上到下可任意转换）
* Byte：有符号整数，1 字节，-127~128
* Short：有符号整数，2 字节，-32768~32767
* Int：有符号整数，4 字节，-2^31~2^31-1
* Long：有符号整数，8 字节，-2^63~2^63-1
* Float：有符号浮点数，4 字节，n/a~n/a
* Double：有符号浮点数，8 字节，n/a~n/a

字面量：（不区分大小写）
* 5 -> Int，默认为无修饰（前缀或后缀）的整数字面量
* 0x0f -> Int，0x 前缀表示十六进制记法
* 5l -> Long，后缀 l
* 5.0 -> Double，默认为无修饰的（前缀后缀）的小数字面量
* 5f -> Float，后缀 f
* 5d -> Double，后缀 d

核心非数值类型：
* Any：Scala 中所有类型的根，不可实例化
* AnyVal：所有值类型的根，不可实例化
* AnyRef：所有引用（非值）类型的根，不可实例化
* Nothing：所有类型的子类，不可实例化
* Null：所有指示 null 值的 AnyRef 类型的子类，不可实例化
* Char：Unicode 字符，可以实例化
* Boolean：true 或 false，可以实例化
* String：字符串（即文本），可以实例化
* Unit：指示没有值，不可实例化

类型操作：（val.method）
* asInstanceOf[<type>]：转换值类型，无法转换时会报错，尽量避免使用
* getClas：返回值类型
* isInstanceOf：如果这个值有给定的类型，则返回 true
* hashCode：返回哈希值
* to<type>：转换函数，将一个值转换为兼容的值

```
// 创建不同类型的值，并自动转换为更高等级的类型
scala> val b: Byte = 10
b: Byte = 10

scala> val s: Short = b
s: Short = 10

scala> val d: Double = s
d: Double = 10.0

// 不允许从高等级类型自动转换到低等级类型，会丢失精度
scala> val l: Long = 20
l: Long = 20

scala> val i: Int = l
<console>:8:error: type mismatch;
 found   : Long
 required: Int
       val i: Int = l

// 可以用 toType 方法手动完成类型间的转换
scala> val l: Long = 20
l: Long = 20

scala> val i: Int = l.toInt
i: Int = 20

// 使用字面量直接赋值，通过类型推导得出各个值的适当类型
scala> val anInt = 5
anInt: Int = 5

scala> val yellowRgb = 0xffff00
yellowRgb: Int = 16776960

scala> val id = 100l
id: Long = 100

scala> val pi = 3.1416
pi: Double = 3.1416

// 1、String 类型：建立在 Java 的 String 基础上，另外增加了多行字面量和字符串内插等特性
// String 字面量要用双引号，特殊字符要用反斜杠转义
scala> val hello = "Hello There"
hello: String = Hello There

scala> val signature = "With Regards, \nyour friend"
signature: String = 
With Regards,
Your friend

// 比较两个 String 值，与 Java 不同，会检查真正的相等性，而不是对象引用相等性
scala> val greeting = "Hello, " + "World"
greeting: String = Hello, World

scala>val matched = (greeting == "Hello, World")
matehed: Boolean = true

// 用三重引号创建多行 String
scala> val greeting = """She suggested reformatting the file
     | by replacing tabs (\t) with newlines (\n);
     | "Why do that?", he asked. """

// 字符串相加
scala> val approx = 355/113f
approx: FLoat = 3.141593

scala> println("Pi, using 355/113, is about " + approx + ".")
Pi, using 355/113, is about 3.141593

// 2、字符串内插：在一个 String 中加入值或变量
scala> println(s"Pi, using 355/113, is about $approx.")
Pi, using 355/113, is about 3.141593

// 如果引用中有非字（nonword）字符（如算式），或者与周围文本无法区分，就需要使用大括号
scala> vall item = "apple"
item: String = apple

scala> s"How do you like them ${item}s?"
res0: String = How do you like them apples?

scala> s"Fish n chips n vinegar, ${"pepper "*3}salt"
res1: String = Fish n chips n vinegar, pepper pepper pepper salt

// 字符串内插的替代格式是 printf记法，适合控制数据格式化（如字符个数或小数值的显示）
scala> vall item = "apple"
item: String = apple

scala> f"I wrote a new $item%.3s today"
res2: String = I wrote a new app today

scala> f"Enjoying this $item ${355/113.0}%.5f times today"
res3: String = Enjoying this applle 3.14159 times today

// 3、正则表达式：由字符和标点符号组成的一个字符串，表示一个搜索模式
scala> "Froggy went a' courting" matches ".* courting"       // 匹配模式，返回true
scala> "milk, tea, muck" replaceAll ("m[^ ]+k", "coffee")    // 替换所有匹配文本
scala> "milk, tea, muck" replaceFirst ("m[^ ]+k", "coffee")  // 替换第一个匹配文本

// 语法：用正则表达式捕获值
val <Regex value>(<identifier>) = <input string>

scala> val input = "Enjoying this apple 3.14159 times today"
input: String = Enjoying this applle 3.14159 times today

// 使用多行字符串来存储正则表达式，因为它们是字面量，所以可以直接写反斜线，无需增加用来转义的反斜线
scala> val pattern = """.* apple ([\d.]+) times .*""".r
pattern: scala.util.matching.Regex = .* apple ([\d.]+) times .*

scala> val pattern(amountText) = input
amountText: String = 3.14159

scala> val amount = amountText.toDouble
amount: Double = 3.14159

// 4、核心非数值类型
// Char：作为 String 类型的基础，Char 包含单个字符，实际上是一个标量类型，可以与其他数字来回转换
scala> val c = 'A'
c: Char = 'A'

scala> val i: Int = c
i: Int = 65

scala> val t: Char = 116
t: Char = t

// Boolean：除了使用 true 和 false，还可以通过比较和布尔逻辑操作符得到布尔值
scala> val isTrue = !true
isTrue: Boolean = false

scala> val unequal = (5 != 6)
unequal: Boolean = true

// && 如果第一个参数足以判断值就不会计算第二个参数；& 对两个参数都进行检查
scala> val unequalAndLess = unequal & true
unequalAndLess: Boolean = true

// 与其他语言不同，非 null 字符不计算为 true，数字 0 也不等于 false
scala> val zero = 0
zero: Int = 0

// 如果需要将一个值的状态计算为布尔值，就要使用显式比较
scala> val isValid = zero > 0
isValid: Boolean = false

// Unit：表示没有数据的意思，通常用来定义函数和表达式，表示不返回任何结果
scala> val nada = ()
nada: Unit = ()

// 5、元组：包含一个或多个值的有序容器，这些值可以有不同的类型
// 元组提供了一种建立数据结构的通用方法，如果要对离散的元素进行分组以便处理会很有用
// 语法：创建元组
(<value1>, <value2>[,<value3>...])

scala> val info = (5, "Korben", true)
info: (Int, String, Boolean) = (5, Korben, true)

// 使用元素索引访问元组中的单个元素，第一个元素的索引为1
scala> val name = info._2
name: String = Korben

// 创建元组的另一种形式是利用关系操作符 ->，表示元组中的键值对
scala> val red = "red" -> "0xff0000"
red: (String, String) = (red, 0xff0000)

scala> val reversed = red._2 -> red._1
reversed: (String, String) = (0xff0000, red)
```

### 第3章 表达式和条件式

#### 表达式

* 返回一个值的代码单元，使用大括号将多行代码收集在一起为一个表达式块
* 关键是返回捕获和使用的值

```
// 1、表达式
scala> "hel" + 'l' + 'o'
res0: string = hello

// 语法：使用表达式定义值核对变量
val <identifier>[: <type>] = <expression>
var <idnetifier>[: <type>] = <expression>

// 2、表达式块
// 使用 x 的目的就是为了定义 amount
scala> val amount = { val x = 5 * 20; x + 10 }
amount: Int = 110

// 表达式块可以根据需要跨多行
scala> val amount = {
     |   val x = 5 * 20
     |   x + 10
     | }
amount: Int = 110

// 表达式块也可以嵌套
scala> { val a = 1; { val b = a * 2; { val c = b + 4; c } } }
res1: Int = 6

// 3、语句：不返回值的表达式，返回类型为 Unit
scala> val x = 1
x: Int = 1
```

#### if else 表达式块

```
// 1、语法：if 表达式
if (<Boolean expression>) <expression>

// 结果值的类型未指定，编译器会使用类型推导来确定最合适的类型
scala> val result = if ( false ) "what does this return?"
result: Any = ()

// 2、语法：if else 表达式
if (<Boolean expression>) <expression>
else <expression>

// 实际上 Scala 不需要三元表达式，if 和 else 块可以紧凑地写在一行上，并且已经是表达式了
scala> val x = 10; val y = 20;
scala> val max = if (x > y) x else y
max: Int = 20 
```

#### 匹配表达式

```
// 1、语法：匹配表达式（如果有多个表达式，可以通过大括号转换为表达式块）
<expression> match {
  case <pattern match> => <expression>
  [case...]
}

scala> val x = 10; val y = 20;
scala> val max = x > y match {
     |   case true => x
     |   case false => y
     | }
max: Int = 20 

scala> val status = 500
scala> val message = status match {
     |   case 200 => "ok"
     |   case 400 => {
     |       println("ERROR - we called the service incorrectly")
     |       "error"
     |   }
     |   case 500 => {
     |       println("ERROR - the service encountered an error")
     |       "error"  // 最后一个表达式会作为返回值
     |   }
     | }
ERROR - the service encountered an error
message: String = error

// 2、语法：模式替换式（通过对多个模式重用相同的 case 块，可以避免重复代码）
case <pattern1> | <pattern2> .. => <one or more expression>

// 无法匹配时会返回 MatchError 错误，可以使用全匹配模式 match-all 兜底
scala> val day = "MON"
scala> val kind = day match {
     |   case "MON" | "TUE" | "WED" | "THU" | "FRI" => "weekday"
     |   case "SAT" | "SUN" => "weekend"
     | }
kind: String = weekday

// 3、通配模式：值绑定或变量绑定
// 语法：值绑定模式
case <identifier> => <one or more expresions>

// 尝试匹配一个特定的字面量，如果不匹配则使用值绑定确保能匹配所有其他可能的值
scala> val message = "Ok"
scala> val status = message match {
     |   case "Ok" => 200
     |   case other => {
     |     println(s"Couldn't parse $other")
     |     -1
     |   }
     | }           
status: Int = 200

// 语法：通配符模式（相当于匿名占位符，在运行时替换值，不能在右侧访问通配符）
case _ => <one or more expressions>

scala> val message = "Unauthorized"
scala> val status = message match {
     |   case "Ok" => 200
     |   case _ => {
     |     println(s"Couldn't parse $message")
     |     -1
     |   }
     | }
Couldn't parse Unauthorized
status: Int = -1

// 4、模式哨卫：向值绑定模式增加一个 if 表达式，从而增加条件逻辑
// 语法：模式哨卫
case <pattern> if <Boolean expression> => <one or more expressions>

scala> val response: String = null
scala> response match {
     |   case s if s != null => println(s"Received '$s'")
     |   case s => println("Error! Received a null response)
     | }
Error! Received a null response

// 5、模式变量：把输入值转换为一个不同类型的值，然后在 case 块中使用这个新值和类型
// 语法：模式变量
case <identifier>: <type> => <one or more expressions>

// 创建一个 Int，将它赋给更高层级的 Any，再用一个匹配表达式解析它真正的类型
scala> val x: Int = 12180; val y: Any = x
scala> y match {
     |   case x: String => s"'x'"
     |   case x: Double => f"$x%.2f"
     |   case x: Float => f"$x%.2f"
     |   case x: Long = > s"${s}l"
     |   case x: Int => s"${x}i"
     | }
res0: String = 12180i
```

#### 循环

```
// 1、语法：定义数值范围
<starting integer> [to|until] <ending integer> [by increment]

// 2、语法：基本 for 循环
for (<identifier> <- <iterator>) [yield] [<expression>]

// 打印简单周计划表
scala> for (x <- 1 to 7) { println(s"Day $x:") }

// yield 将返回值作为一个集合返回
scala> for (x <- 1 to 7) yield { s"Day $x:" }
res0: scala.collection.immutable.IndexedSeq[String] = Vector(Day 1:, ..., Day7:)

// 这个集合现在可以在其他 for 循环中用作一个迭代器
// 迭代器中只有一个输出命令，所以不需要大括号
scala> for (day <- res0) print(day + ", ")
Day 1:, Day 2:, Day 3:, Day 4:, Day 5:, Day 6:, Day 7:,

// 3、迭代器哨卫（过滤器）：可以为迭代器增加一个 if 表达式
// 可以跳过一次迭代，除非 if 表达式返回 true

// 语法：迭代器哨卫
for (<identifier> <- <iterator> if <Boolean expression>) ...

// 创建一个 3 的倍数的集合
scala> val threes = for (i <- 1 to 20 if i % 3 == 0) yield i
threes: scala.collection.immutable.IndexedSeq[Int] = Vector(3, 6, 9, 12, 15, 18)

// 迭代器哨卫也可以与迭代器分开，出现在单独的一行上
scala> val quote = "Faith,Hope,,Charity"
scala> for {
     |   t <- quote.split(",")
     |   if t != null
     |   if t.size > 0
     | }
     | { println(t) }
Faith
Hope
Charity

// 4、嵌套迭代器：增加到一个 for 循环的额外的迭代器，迭代总数随迭代器个数倍增
// 把它们增加到同一个循环中，与写为单独的嵌套循环有相同的效果

// 包含两个迭代器
scala> for { x <- 1 to 2 
     |       y <- 1 to 3 }
     | { print(s"($x,$y) ") }
(1,1) (1,2) (1,3) (2,1) (2,2) (2,3)

// 5、值绑定：有助于降低表达式块的规模和复杂性，可以用于嵌套迭代器、迭代器哨卫和其他绑定值
// 语法：for 循环中的值绑定
for (<identifier> <- <iterator>;; <identifier> = <expression>) ...

// 计算 2 的 0 次幂到 8 次幂，每个操作的结果绑定到当前迭代的值 pow
scala> val powersOf2 = for (i <- 0 to 8; pow = 1 << i) yield pow
powersOf2: scala.collection.immutable.IndexedSeq[Int] = Vector(1, 2, 4, 8, 16, 32, 64, 128, 256)

// 6、while 和 do/while 循环
// 与 for 循环不一样，它们不是表达式，不能用来获得值
// 语法：While 循环
while (<Boolean expression>) statement

// While 循环：将一个数反复递减，直到不再大于 0
scala> var x = 10; while (x > 0) x -= 1
x: Int = 0

// 类似的 do/while 循环，语句至少执行一次
scala> val x = 0
scala> do println(s"Here I am, x = $x") whiel (x > 0)
Here I am, x = 0
```


### 第4章 函数

* 可重用
* 可组合

纯函数：
* 有一个或多个输入参数
* 只使用输入参数完成计算
* 返回一个值
* 对于相同的输入总返回相同的值
* 不使用或影响函数之外的任何数据
* 不受函数之外的任何数据的影响

特点：
* 更稳定，因为没有状态
* 与外部数据是正交的（如文件、数据库、套接字、全局变量或其他共享数据）
* 是不可破坏而且非破坏性的纯逻辑表达式

```
// 1、语法：定义无输入的函数
def <identifier> = <expression>

scala> def hi = "hi"
hi: string

scala> hi
res0: string = hi

// 2、语法：定义函数时指定返回类型
def <identifier>: <type> = <expression>

scala> def hi: String = "hi"
hi: String

// 3、语法：定义函数
def <identifier>(<identifier>: <type>[, ...]): <type> = <expression>

// return 返回值
scala> def multiplier(x: Int, y: Int): Int = { x * y }
multiplier: (x: Int, y: Int)Int

scala> multiplier(6,7)
res0: Int = 42

// return 提前退出函数
scala> def safeTrim(s: String): String = {
     |   if (s == null) return null
     |   s.trim()
     | }
```

#### 过程

* 定义：没有返回值的函数
* 可以显示指定返回类型 Unit，表示没有返回值

```
scala> def log(d: Double): Unit = println(f"Got value $d%.2f")
scala> log(2.23535)
Got value 2.24
```

#### 用空括号定义函数

```
scala> def hi(): String = "hi"
scala> hi()
res0: String = hi
```

#### 使用表达式块调用函数

```
scala> def formatEuro(amt: Double) = f"€$amt%.2f"
formatEuro: (amt: Dboule)String

scala> formatEuro(3.4645)
res0: String = €3.46

scala>formatEuro { val rate = 1.32; 0.235 + 0.7123 + rate * 5.32 }
res1: String = €7.97
```

#### 递归函数

* 调用自身的函数
* 可能要检查某类参数或外部条件来避免函数调用陷入无限循环
* 为迭代处理树结构或计算提供了一种很好地方法
* 不必使用可变的数据，因为每个函数调用自己的栈来存储函数参数

栈溢出：
* 调用函数的次数太多，耗尽了所有已分配的栈空间

用尾递归优化函数：
* 不会创建新的栈空间，而是使用当前函数的栈空间
* 只有最后一个语句是递归调用的函数才能由 Scala 编译器完成尾递归优化
* 可以利用函数注解来标注函数完成尾递归优化，如果不能完成，就会产生编译错误

```
// 递归函数：返回一个整数的给定正次数幂
scala> def power(x: Int, n: Int): Long = {
     |   if (n >= 1) x * power(x, n-1)
     |   else 1
     | }

scala> power(2,8)
res0: Long = 256

// 尾递归优化（此例无法完成，因为最后一个语句不是递归调用）
scala> @annotation.tailrec
scala> def power(x: Int, n: Int): Long = {
     |   if (n >= 1) x * power(x, n-1)
     |   else 1
     | }
<console>:9: error: could not optimize @tailrec annotated method power

// 修改一下，使函数可以进行尾递归优化
scala> @annotation.tailrec
scala> def power(x: Int, n: Int, t: int = 1): Long = {
     |   if (n < 1) t
     |   else power(x, n-1, x*t)
     | }
```

#### 嵌套函数

```
// 返回三个整数中的最大值
// 嵌套函数与外部函数同名，但是参数个数不同，所以不会发生冲突
// 即使函数名和参数类型相同，它们也不会冲突，因为局部（嵌套）函数优先于外部函数
scala> def max(a: Int, b: Int, c: Int) = {
     |   def max(x: Int, y: Int) = if (x > y) x else y
     |   max(a, max(b, c))
     | }
```

#### 用命名参数调用函数

```
scala> def greet(prefix: String, name: String) = s"$prefix $name"
scala> val greeting1 = greet("Ms", "Brown")
scala> val greeting2 = greet(name = "Brown", prefix = "Mr")
```

#### 有默认值的参数

```
// 可以为任意参数指定默认值
scala> def greet(prefix: String = "", name: String) = s"$prefix$name"
scala> greeting1 = greet(name = "Paul")

// 将必要的参数写在前面，就可以不写参数名，而且可以不指定默认值参数
scala> def greet(name: String, prefix: String = "") = s"$prefix$name"
scala> greeting2 = greet("Ola")
```

#### Vararg 参数

* 输入参数个数可变的函数

```
scala> def sum(items: Int*): Int = {
     |   var total = 0
     |   for (i <- items) total += i
     |   total
     | }
scala> sum(10, 20, 30)
scala> sum()
```

#### 参数组

* 把参数表分解为参数组，每个参数组用小括号分隔

```
scala> def max(x: int)(y: Int) = if (x > y) x else y
scala> val larger = max(20)(39)
```

#### 类型参数

* 表示值参数或返回值使用的类型

```
// 同一性函数：只返回它的输入
scala> def identity(s: String): String = s

// 使用类型参数定义的同一性函数，可以用于任何类型
scala> def identity[A](a: A): A = a
scala> val s: String = identity[String]("Hello")
scala> val d: Double = identity[Double](2.717)

// 编译器可以推导出类型参数
scala> val s = identity("Hello")
scala> val d = identity(2.717)
```

#### 方法和操作符

方法：
* 类中定义的一个函数，这个类的所有实例都会有这个方法

```
// String 和 Double 自带方法
scala> val s = "vacation.jpg"
scala> val isJPEG = s.endsWith(".jpg")

scala> val d = 65.642
scala> d.round
scala> d.floor
scala> d.compare(18.0)
scala> d.+(2.721)

// 用中缀操作符记法来调用对象的方法
scala> d compare 18.0
scala> d + 2.721
```

#### 编写可读的函数

* 函数简短，命名适当，含义明确
* 在适当的地方增加注释
* 向函数增加 Scaladoc 首部，可以据此生成 API 文档

```
scala> /**
     |  * Returns the input string without leading or trailing 
     |  * whitespace, or null if the input string is null.
     |  * @param s the input string to trim, or null.
     |  */
     | def safeTrim(s: String): String = {
     |   if (s == null) return null
     |   s.trim()
     | }
```


### 第5章 首类函数

首类函数：
* 函数不仅能得到声明和调用，还可以作为一个数据类型用在这个语言的任何地方
* 与其他数据类型一样，可以采用字面量形式创建，而不必指定标识符
* 可以存储在一个容器中，比如值、变量或数据结构
* 可以用作为另一个函数的参数或返回值

高阶函数：
* 接受其他函数作为参数，或者使用函数作为返回值
* map：将一个或多个项转换为一个新值和/或类型（将计算映射到大量分布式节点上）
* reduce：取一个函数参数，将一个包含多项的集合归约为一项（归约其结果）
* 优点：具体如何处理数据将作为实现细节，留给包含这个高阶函数的框架来完成

声明式编程：
* 要求使用高阶函数或其他机制声明要做的工作，而不手动实现

命令式编程：
* 与声明式编程相反，需要明确指定操作的逻辑流

函数字面量：
* 可以存储在函数值和变量中，也可以定义为一个高阶函数调用的一部分
* 任何接受函数参数类型的地方都可以使用函数字面量

占位符：
* 函数的显式类型在字面量之外指定
* 参数最多只使用一次

全函数/偏函数：
* 正确地支持满足输入参数类型的所有可能的值，则为全函数；否则为偏函数
* 例如计算平方根，如果入参为负数就无法正常工作，所以是偏函数

#### 函数类型和值

```
// 定义函数
scala> def double(x: int): int = x * 2

// 以下两种写法等价
// 有单个参数的函数类型可以省略小括号，等价于 Int => Int
scala> val myDouble: (Int) => Int = double
// 用下划线作为占位符，表示将来的一个函数调用，并且返回一个函数值
scala> val myDouble = double _

// 将函数值赋给一个新值
scala> val myDoubleCopy = myDouble
scala> myDoubleCopy(5)
res0: Int = 10

// 没有输入的函数类型
scala> def logStart() = "=" * 50
scala> val start: () => String = logStart
scala> println( start() )
```

#### 高阶函数

```
// 高阶函数的常见用法：在参数中调用其他函数
scala> def safeStringOp(s: String, f: String => String) = {
     |   if (s != null) f(s) else s
     | }
scala> def reverser(s: String) = s.reverse
scala> safeStringOp("Ready", reverser)
res0: ydaeR
```

#### 函数字面量

```
// 定义了一个有类型的输入参数 x 和函数体 (x*2)，共同组成函数字面量
scala> val doubler = (x: Int) => x * 2
scala> val doubled = doubler(22)

// 函数字面量实际上就是参数化表达式
scala> val greeter = (name: String) => s"Hello, $name"
scala> val hi = greeter("World")

// 定义函数，赋给一个函数值，用函数字面量重新定义
scala> def max(a: Int, b: Int) = if (a > b) a else b
scala> val maximize: (Int, Int) => Int = max
scala> val maximize = (a: Int, b: Int) => if (a > b) a else b

// 在更高阶函数调用中定义函数字面量
scala> safeStringOp("Ready", s => s.reverse)
```

#### 占位符语法

```
// 使用通配符取代命名参数
scala> var doubler: Int => Int = _ * 2

// 用占位符简化函数字面量写法（上例）
scala> safeStringOp("Ready", _.reverse)

// 多个占位符会按位置顺序替换输入参数
scala> def combination(x: Int, y: Int, f: (Int, Int) => Int) = f(x, y)
scala> combination(23, 12, _ * _)
res0: Int = 276

// 使用多个占位符会降低代码可读性
scala> def tripleOp(a: Int, b: Int, c: Int, f: (Int, Int, Int) => Int) = f(a,b,c)
scala> tripleOp(23, 92, 12, _ * _ + _)
res1: Int = 2130

// 使用两个类型参数重新定义，分别表示输入类型和返回值类型
scala> def tripleOp[A,B](a: A, b: A, c: A, f: (A, A, A) => B) = f(a,b,c)
scala> tripleOp[Int,Int](23, 92, 14, _ * _ + _)
scala> tripleOp[Int,Double](23, 92, 14, 1.0 * _ / _ / _)
scala> tripleOp[Int,Boolean](93, 92, 14, _ > _ + _)
```

#### 部分应用函数和柯里化

```
// 单个参数表认为是一个单独的函数调用
// 函数类型为 (Int, Int) => Boolean
scala> def factorOf(x: Int, y: Int) = y % x == 0

// 这个函数的一个快捷方式，所有参数都不保留
scala> val f = factorOf _
scala> val x = f(7, 20)
x: Boolean = false

// 部分应用函数：保留部分参数，使用通配符代替其中一个参数
scala> val multipleOf3 = factorOf(3, _: Int)
val y = multipleOf3(78)
y: Boolean = true

// 多个参数表的函数可以认为是多个函数的一个链
// 重写该函数，函数类型为 Int => Int => Boolean
scala> def factorOf(x: Int)(y: Int) = y % x == 0

// 部分应用函数的更简洁方法：使用有多个参数表的函数
// 函数柯里化：应用一个参数表中的参数，另一个参数表不应用
scala> val isEven = factorOf(2) _
scala> val z = isEven(32)
```

#### 传名参数

```
scala> def doubles(x: Int) = {
     |   println("Now doubling" + x)
     |   x * 2
     | }

scala> doubles(5)
Now doubling 5
res0: Int = 10

scala> def f(i: Int) = { printl(s"Hello from f($i)"); i }

// 在方法中调用函数值，会输出两次消息，因为调用了两次
scala> doubles( f(8) )
Hello from f(8)
Now doubling 8
Hello from f(8)
res1: Int = 16
```

#### 偏函数

```
scala> var statusHandler: Int => String = {
     |   case 200 => "Okay"
     |   case 400 => "Your Error"
     |   case 500 => "Our Error"
     | }
statusHandler: Int => String = <function1>

// 合法输入
scala> statusHandler(200)

// 非法输入，尽管入参满足类型，但无法匹配这个偏函数的任何一个 case 模式
scala> statusHandler(401)
```

#### 用函数字面量块调用高阶函数

```
// 例1
scala> def safeStringOp(s: String)(f: String => String) = {
     |   if (s != null) f(s) else s
     | }

scala> val uuid = java.util.UUID.randomUUID.toString
uuid: String = bfe1ddda-92f6-4c7a-8bfc-f946bdac7bc9

// 更清晰的调用，值参数放在小括号里传入，函数参数作为独立的函数字面量块传入
scala> val timedUUID = safeStringOp(uuid) { s =>
     |   val now = System.currentTimeMillis
     |   val timed = s.take(24) + now
     |   timed.toUpperCase
     | }
timedUUID: String = BFE1DDDA-92F6-4C7A-8BFC-1394546043987

// 例2
scala> def timer[A](f: => A): A = {
     |   def now = System.currentTimeMillis
     |   var start = now; val a = f; val end = now
     |   println(s"Executed in ${end - start} ms")
     |   a
     | }
timer: [A](f: => A)A

// 表达式块型高阶函数调用：将单独的代码块包围在工具函数中
scala> val veryRandomAmount = timer {
     |   util.Random.setSeed(System.currentTimeMillis)
     |   for (i <- 100000) util.Random.nextDouble
     |   util.Random.nextDouble
     | }
Executed in 13 ms
veryRandomAmount: Double = 0.5070558765221892

// 好处：
// 管理数据库事务，即高阶函数打开回话、调用函数参数，然后用一个 commit 或者 rollback 结束事务
// 重新尝试处理可能的错误，将函数参数调用指定次数，直到不再产生错误
// 根据局部、全局或外部值（例如一个数据库设置或环境变量）有条件地调用函数参数
```

### 第6章 常用集合

核心数据结构：
* 有序集合 List
* 无序集合 Map
* 无序集合 Set
* 继承于 Iterable 根类型
* 在创建之后不能改变，不能调整大小也不能改变内容
* 不可变集合优先于可变集合，这也是函数式编程中的最佳实践，可以提高代码稳定性，在并发代码中使用会更安全

与其他语言中的集合的区别：
* 支持不可变数据结构和高阶操作
* 用匿名函数迭代处理或映射集合
* 确保集合以及匿名函数输入和返回的类型
* 提供类型安全高阶函数的集合支持声明式编程，能够创建具有表述性的代码，而且几乎没有运行时类型转换错误
* 集合是一元的（monadic），支持以一种高级、类型安全的方式将操作串联在一起

#### 列表、集和映射

```
// 例1
scala> val numbers = List(32, 95, 24, 21, 17)
numbers: List[Int] = List(32, 95, 24, 21, 17)

scala> var total = 0; for (i <- numbers) { total += i }
total: Int = 189

scala> val total = numbers.reduce( (a: Int, b: Int) => a + b )
total: Int = 189

// 例2
scala> val colors = List("red", "green", "blue")
colors: List[String] = List(red, green, blue)

scala> println(s"I have ${colors.size} colors: $colors")
I have 3 colors: List(red, green, blue)

scala> colors.head
res0: String = red

scala> colors.tail
res1: List[String] = List(green, blue)

scala> colors(1)
res2: String = green

scala> clolrs(2)
res3: String = blue

scala> for (c <- colors) { println(c) }
red
green
blue

scala> val sizes = colors.map( (c: String) => c.size )
sizes: List[Int] = List(3, 5, 4)

// 例3
scala> val unique = Set(10, 20, 30, 20, 20, 10)
unique: scala.collection.immutable.Set[Int] = Set(10, 20, 30)

scala> val sum = unique.reduce( (a: Int, b: Int) => a + b )
sum: Int = 60

// 例4
scala> val colorMap = Map("red" -> 0xFF0000, "green" => 0xFF00, "blue" => 0xFF)
colorMap: scala.collection.immutable.Map[String,Int] = Map(red -> 16711680, green -> 65280, blue -> 255)

scala> val redRGB = colorMap("red")
redRGB: Int = 16711680

scala> val cyanRGB = colorMap("green") | colormap("blue")
cyanRBG: Int = 65535

scala> val hasWhite = colorMap.contains("white")
hasWhite: Boolean = false

scala> for (pairs <- colorMap) { println(pairs) }
(red,16711680)
(green,65280)
(blue,255)
```

#### List里有什么？

```
// 1、创建 List
// 标准做法：作为一个函数来调用这个集合，并提供必要的内容
scala> val colors = List("red", "green", "blue")
colors: List[String] = List(red, green, blue)

// 可以在集合中存储任何类型的值，比如可以创建一个集合的集合
scala> val oddsAndEvents = List(List(1, 3, 5), List(2, 4, 6))
oddsAndEvents: List[List[Int]] = List(List(1, 3, 5), List(2, 4, 6))

// 看上去类似 Map 的 List
scala> val keyValues = List(('A',65), ('B',66), ('C',67))
keyValues: List[(Char, Int)] = List(('A',65), ('B',66), ('C',67))

// 2、访问列表中的单个元素
// 可以作为一个函数调用这个列表，并提供一个索引号（从0开始）
scala> val primes = List(2, 3, 5, 7, 11, 13)
primes: List[Int] = List(2, 3, 5, 7, 11, 13)

scala> val first = primes(0)
first: Int = 2

scala> val fourth = primes(3)
fourth: Int = 7

// 表头是列表的第一项，表尾是其余项
// List 是不可变的递归数据结构，所以列表中的每一项都有表头和越来越短的表尾
scala> val first = primes.head
first: Int = 2

scala> val remaining = primes.tail
remaining: List[Int] = List(3, 5, 7, 11, 13)

// 3、遍历列表
// 使用 while 循环遍历列表
scala> val i = primes
scala> while(! i.isEmpty) { print(i.head + ","); i = i.tail }
2, 3, 5, 7, 11, 13,

// 使用递归形式遍历列表
scala> def visit(i: List[Int]) {
     |   if (i.size > 0) { print(i.head + ","); visit(i.tail) }
     | }
visit: (i: List[Int])Unit
scala> visit(primes)
2, 3, 5, 7, 11, 13,

// 更高效地检查列表是否到达末尾，所有列表都有一个 Nil 实例作为终结点
scala> while(i != Nil) { print(i.head + ","); i = i.tail }
2, 3, 5, 7, 11, 13,

// Nil 是 List[Nothing] 的一个单例实例，创建一个空列表，实际上会返回 Nil
scala> val l: List[Int] = List()
scala> l == Nil
res0: Boolean = true

// 不论数据的类型是什么，List 总是以 Nil 结尾
scala> val m: List[String] = List("a")
scala> m.head
res1: String = a
scala> m.tail == Nil
res2: Boolean = true

// 4、Cons操作符
// 使用右结合的 cons(construct的缩写) 操作符来构建列表
scala> val numbers = 1 :: 2 :: 3 :: Nil
numbers: List[Int] = List(1, 2, 3)

scala> val first = Nil.::(1)
first: List[Int] = List(1)

scala> first.tail == Nil
res0: Boolean = true

// 在现有的列表前面追加一个值，并创建一个新的列表（不可变列表的递归性和可重用性）
scala> val second = 2 :: first
second: List[Int] = List(2, 1)

scala> second.tail == first
res1: Boolean = true
```

#### 列表算术运算

```
// 为列表追加单个元素（右结合操作符）
1 :: 2 :: Nil

// 在列表前面追加另一个列表（右结合操作符）
List(1,2) ::: List(2,3)

// 为列表追加另一个集合
List(1,2) ++ Set(3,4,3)

// 比较集合类型和内容，如果都相同则返回 true
List(1,2) == List(1,2)

// 返回不包含重复元素的列表版本
List(3,5,4,3,4).distinct

// 从列表中删除前 n 个元素（操作符记法，操作参数为 n，没有操作参数时必须使用点记法）
List('a','b','c','d') drop n

// 从列表返回经过一个 true/false 函数验证的元素
List(23,8,14,21) filter (_ > 18)

// 将一个列表的列表转换为元素列表（点记法）
List(List(1,2),List(3,4)).flatten

// 根据一个 true/false 函数的结果，将元素分组为由两个列表组成的一个元组
List(1,2,3,4,5) partition(_ < 3)

// 逆置列表
List(1,2,3).reverse

// 返回列表的一部分，从第一个索引到第二个索引（但不包括第二个索引本身）
List(2,3,5,7) slice (1,3)

// 按给定函数返回的值对列表排序
List("apple","to") sortBy (_.size)

// 按自然值对列表排序
List("apple","to").sorted

// 给定一个索引，根据元素位于索引前面还是后面，拆分为由两个列表构成的一个元组
List(2,3,5,7) splitAt 2

// 从列表中抽取前 n 个元素
List(2,3,5,7,11,13) take 3

// 将两个列表合并为一个元组列表，每个元组包含两个列表中各个索引的相应元素
List(1,2) zip List("a","b")

// 高阶函数的例子
scala> val f = List(23,8,14,21) filter (_ > 18)
f: List[Int] = Lis(23,21)

scala> val p = List(1,2,3,4,5) partition (_ < 3)
p: (List[Int], List[Int]) = (List(1,2), List(3,4,5))

scala> val s = List("apple","to") sortBy (_.size)
s: List[String] = List(to, apple)

// 作用在列表前面（::、drop和take）的操作不会产生性能问题，因为无需遍历列表
// 反之，作用在列表末尾的操作（+:、dropRight和takeRight），就需要遍历列表，并将内容复制到新列表
scala> val appended = List(1,2,3,4) := 5
appended: List[Int] = List(1,2,3,4,5)

scala> val suffix = appended takeRight 3
suffix: List[Int] = List(3,4,5)

scala> val middle = suffix dropRight 2
middle: List[Int] = List(3)
```

#### 映射列表

```
// 列表映射：将指定函数应用于列表中的每一个元素，再将结果保存到新列表
scala> List(0,1,0) collect {czse 1 => "ok"}
res0: List[String] = List(ok)

scala> List("milk,tea") flapMap (_.split(','))
res1: List[String] = List(milk,tea)

scala> List("milk","tea") map (_.toUpperCase)
res2: List[String] = List(MILK,TEA)
```


#### 归约列表

```
// 列表归约：将列表收缩为单个值

// 1、数学归约操作（例如查找一个列表的总和）
// 查找列表中的最大值
List(41,59,26).max

// 查找列表中的最小值
List(41,59,26).min

// 将列表中的数相乘
List(5,6,7).product

// 对列表中的数求和
List(5,6,7).sum

// 2、逻辑归约操作（例如确定一个列表是否包含某个给定的元素）
// 检查列表中是否包含这个元素
List(34,29,18) contains 29

// 测试列表是否以给定列表开头
List(0,4,3) startsWith LIst(0)

// 检查列表是否以给定列表结尾
List(0,4,3) endsWith LIst(4,3)

// 检查谓词是否至少对列表中的一个元素返回true
List(24,17,32) exists (_ < 18)

// 检查谓词是否对列表中的每个元素都返回true
List(24,17,32) forall (_ < 18)

// 使用三种不同的写法在一个验证结果列表中搜索false项
scala> val validations = List(true,true,false,true,true,true)
validations: List[Boolean] = List(true,true,false,true,true,true)

scala> val valid1 = !(validations contains false)
scala> val valid2 = validations forall (_ == true)
scala> val valid3 = validations.exists(_ == false) == false

// 自定义实现列表归约操作
// 迭代处理一个累加器（accumulator）变量，包含目前为止的当前结果，基于当前元素更新累加器
scala> def contains(x: Int, l: List[Int]): Boolean = {
     |   var a: Boolean = false
     |   for (i <- l) { if (!a) a = (i == x) }
     |   a
     | }

scala> val included = contains(19, List(46,19,92))
included: Boolean = true

// 把 contains 逻辑挪到函数参数中，就可以重用函数，从而支持其他列表归约操作（例如布尔列表）
scala> def boolReduce(l: List[Int] start: Boolean)(f: (boolean, Int) => Boolean): Boolean = {
     |   var a = start
     |   for (i <- l) a = f(a, i)
     |   a
     | }

scala> val included = boolReduce(List(46,19,92), false) { (a, i) =>
     |   if (a) a else (i == 19)
     | }
included: Boolean = true

// 进一步改写函数 sum，使它适用于任何类型的列表和从左到右归约操作（使用类型参数会导致代码可读性降低）
scala> def reduceOp[A,B](l: List[A], start: B)(f: (B, A) => B): B = {
     |   var a = start
     |   for (i <- l) a = f(a, i)
     |   a
     | }

scala> var included = reduceOp(List(46,19,92), false) { (a, i) =>
     |   if (a) a else (i == 19)
     | }
included: Boolean = true

// 归约函数：求和
// 使用占位符语法，因为函数参数只访问一次
scala> val answer = reduceOp(List(11.3,23.5,7.2),0.0)(_ + _)
answer: Double = 42.0

// 3、列表折叠：根据输入函数归约列表的高级函数
// 处理顺序：不限定/从左到右/从右到左，分布式系统不限定顺序，其余情况一般从左到右（列表遍历次数更少）
// 给定起始值和归约函数，归约列表
List(4,5,6).fold(0)(_ + _)

// 给定起始值和归约函数，从左到右归约列表
List(4,5,6).foldLeft(0)(_ + _)

// 给定起始值和归约函数，从右到左归约列表
List(4,5,6).foldRight(0)(_ + _)

// 给定归约函数，从第一个元素开始归约列表
List(4,5,6).reduce(_ + _)

// 给定归约函数，从第一个元素开始从左到右归约列表
List(4,5,6).reduceLeft(_ + _)

// 给定归约函数，从第一个元素开始从右到左归约列表
List(4,5,6).reduceRight(_ + _)

// 给定起始值和归约函数，返回各个累加值的一个列表
List(4,5,6).scan(0)(_ + _)

// 给定起始值和归约函数，从左到右返回各个累加值的一个列表
List(4,5,6).scanLeft(0)(_ + _)

// 给定起始值和归约函数，从右到左返回各个累加值的一个列表
List(4,5,6).scanRight(0)(_ + _)

// 使用列表折叠实现 contains 操作
scala> val included = List(46,19,92).foldLeft(false) { (a, i) => 
     |   if (a) a else (i == 19)
     | }
included: Boolean = true

// 使用列表折叠实现 sum 操作（reduceLeft 函数使用了列表第一个元素作为起始值，而不是作为一个参数，操作更简短）
scala> val answer = List(11.3,23.5,7.2).reduceLeft(_ + _)
answer: Double = 42.0
```

#### 转换集合

```
// 1、集合转换操作
// 作为不可变的集合，List、Map 和 Set 不能由空集合构建，更适合从现有的集合创建

// 使用给定分隔符将一个集合呈现为Set
List(24,99,104).mkString(", ")

// 将不可变集合转换为可变的集合
List('f','t').toBuffer

// 将集合转换为列表
Map("a" → 1, "b" → 2).toList

// 将二元元组的集合转换为映射
Set(1 → true, 3 → true).toMap

// 将集合转换为Set
List(2,5,5,3,2).toSet

// 将集合呈现为String，包括集合的类型
List(2,5,5,3,2).toString

// 2、Java 和 Scala 集合兼容性
// 由于 Scala 在 JVM 上编译和运行，经常需要与 JDK 以及其他 Java 库进行交互
scala> import collection.JavaConverters._

// Scala 集合转换为 Java 集合
List(12,29).asJava

// Java 集合转换为 Scala 集合
new java.util.ArrayList(5).asScala
```

#### 使用集合的模式匹配

```
// 模式匹配是 Scala 的核心特性，可以缩短和简化逻辑，而在其他语言中需要大量代码才能实现这些逻辑
scala> val statuses = List(500, 404)

// 使用匹配表达式来匹配单个值
scala> val msg = statuses.head match {
     |   case x if x < 500 => "okay"
     |   case _ => "whoah, an error"
     | }
msg: String = whoah, an error

// 使用模式哨卫匹配集合中的单个值
scala> val msg = statuses match {
     |   case x if x contains(500) => "has error"
     |   case _ => "okay"
     | }
msg: String = has error

// 要匹配整个集合，可以使用一个新集合作为模式
scala> val msg = statuses match {
     |   case List(404, 500) => "not found & error"
     |   case List(500, 404) => "error & not found"
     |   case List(200, 200) => "okay"
     |   case _ => "not sure what happened"
     | }
msg: String = error & not found

// 使用值绑定在模式哨卫中将值绑定到集合中的一些或所有元素
scala> val msg = statuses match {
     |   case List(500, x) => s"Error followed by $x"
     |   case List(e, x) => s"$e was followed by $x"
     | }
msg: String = Error followed by 404

// 作为模式匹配表头和表尾元素
scala> val head - List('r','g','b') match {
     |   case x :: xs => x
     |   case Nil => ' '
     | }
head: Char = r

// 元组的模式匹配功能，元组可以支持不同类型的值
scala> val code = ('h', 204, true) match {
     |   case (_, _, false) => 501
     |   case ('c', _, true) => 302
     |   case ('h', x, true) => x
     |   case (c, x, true) => {
     |     println(s"Did not expect code $c")
     |     x
     |   }
     | }
code Int = 204
```


### 第7章 更多集合

#### 可变集合

```
// 1、创建一个不可变的映射，然后变换这个映射
scala> val m = Map("AAPL" -> 597, "MSFT" -> 40)
m: scala.collection,immutable.Map[String, Int] = Map(AAPL -> 597, MSFT -> 40)

scala> val n = m - "AAPL" + ("GOOG" -> 521)
n: scala.collection,immutable.Map[String, Int] = Map(MSFT -> 40, GOOG -> 521)

// 2、创建新的可变集合
// 各类不可变集合的可变类型（不可变集合类会自动增加到当前命名空间，而可变集合类则不会）
collection.immutable.List -> collection.mutable.Buffer  // 列表
collection.immutable.Set  -> collection.mutable.Set     // 集
collection.immutable.Map  -> collection.mutable.Map     // 映射

// 构建一个可变的整数列表（从一个元素开始）
scala> val nums = collection.mutable.Buffer(1)
nums: scala.collection.mutable.Buffer[Int] = ArrayBuffer(1)

scala> for (i <- 2 to 10) nums += i
scala> println(nums)
Buffer(1,2,3,4,5,6,7,8,9,10)

// 构建一个可变的整数列表（从空集合开始）
// 需要为新列表指定类型参数，集和映射也类似
scala> val nums = collection.mutable.Buffer[Int]()
nums: scala.collection.mutable.Buffer[Int] = ArrayBuffer()

scala> for (i <- 1 to 10) nums += i
scala> println(nums)
Buffer(1,2,3,4,5,6,7,8,9,10)

// 把可变的缓冲区转换会不可变的列表
// 集和映射也类似，对应方法为 toSet 和 toMap
scala> val l = nums.toList
l: List[Int] = List(1,2,3,4,5,6,7,8,9,10)

// 3、从不可变集合创建可变集合
scala> val m = Map("AAPL" -> 597, "MSFT" -> 40)
m: scala.collection,immutable.Map[String, Int] = Map(AAPL -> 597, MSFT -> 40)

scala> val b = m.toBuffer
b: scala.collection.mutable.Buffer[(String, Int)] = ArrayBuffer((AAPL,597), (MSFT,40))

scala> b trimStart 1
scala> b += ("GOOG" -> 521)
```