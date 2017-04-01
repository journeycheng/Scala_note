# Scala

## 一、简介

- 多范式编程语言，继承了面向对象和函数式语言的特性
- 运行于Java平台
- 纯粹的面向对象编程语言，在Scala中，每个值都是对象，每个操作都是方法调用

- 优点：
  - 具备强大的并发性，支持函数式编程（没有可变变量，就不会有内存共享的问题），可以更好地支持分布式系统
  - 语法简洁，能提供优雅的API
  - 兼容Java，运行速度快，且能融合到Hadoop生态圈中
  - 提供了REPL(Read-Eval-Print Loop, 交互式解释器)，类似于Ipython，很大程度上提升开发效率
  

## 二、安装

- 下载SDK：http://www.scala-lang.org/download/
- 配置环境变量
```linux
$ vi ~/.bash_profile

# 输入以下内容，其中SCALA_HOME是放置Scala的位置
SCALA_HOME='/Users/journeycheng/software/Scala/scala-2.12.1/'
export PATH=$PATH:$SCALA_HOME/bin
```
- 打开一个新的终端，输入scala
```linux
$ scala
cat: /release: No such file or directory
Welcome to Scala 2.12.1 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_121).
Type in expressions for evaluation. Or try :help.

scala> 
scala> 8*2+5
scala> :quit
```
- Hello World
 - 新建一个文件test.scala，内容如下：
 ```scala
 object Helloworld {
     def main(args: Array[String]){
         println("Hello, World!")
     }
 }
 ```
  - 对象的命名Helloworld可以不用和文件名称一致
 - scalac编译，scala执行
 ```linux
 $ scalac test.scala
 $ scala -classpth . Helloworld
 Hello, World!
 ```

## 三、基础知识

### 3.1 变量

- **val：不可变**，在声明时必须被初始化，初始化以后就不能再被赋值
- **var：可变**，声明的时候需要进行初始化，初始化以后还可以再次对其赋值

- val变量
```scala
scala> val myStr = "Hello World"
myStr: String = Hello World

scala> val num = 1
num: Int = 1

scala> println(myStr)
Hello World
```

scala具有**类型推断能力**，可以自动推断出变量的类型。可以按照上述格式显式声明变量的类型。

- var变量
```scala
scala> var myAge: Int= 18
myAge: Int = 18

scala> myAge = 17
myAge: Int = 17
```
### 3.2 基本数据类型

- **Byte、Char、Short、Int、Long、Float、Double和Boolean**
- 这些类型都是“类”，并且是包scala的成员，比如Int的全名是scala.Int；对于字符串，Scala用java.lang.String类表示
- Scala没有提供++和--操作符，需要递增和递减时方法和python一样

### 3.3 Range

```scala
scala> 1 to 5
res2: scala.collection.immutable.Range.Inclusive = Range 1 to 5

scala> 1.to(5)
res3: scala.collection.immutable.Range.Inclusive = Range 1 to 5

scala> 1 until 5
res4: scala.collection.immutable.Range = Range 1 until 5

scala> 1 to 10 by 2
res5: scala.collection.immutable.Range = inexact Range 1 to 10 by 2
```

### 3.4 打印语句
```scala
// 单行输出
scala> print("My name is:"); print("JM")
My name is:JM

// 换行输出
scala> println("My name is:"); println("JM")
My name is:
JM

// 格式化输出
scala> val i=5; val j=8
i: Int = 5
j: Int = 8

scala> printf("My name is %s. I hava %d apples and %d egg.\n", "JM", i, j)
My name is JM. I hava 5 apples and 8 egg.
```

### 3.5 读写文件

- 使用java.io.PrintWriter实现把数据写入到文本文件
```scala
scala> import java.io.PrintWriter
import java.io.PrintWriter

scala> val out = new PrintWriter("output.txt")
out: java.io.PrintWriter = java.io.PrintWriter@4902c584

scala> for (i <- 1 to 5) out.println(i)

scala> out.close()
```

在终端上查看:

```linux
$ cat output.txt 
1
2
3
4
5
```

- 使用Scala.io.Source读取文本文件中的行
```scala
scala> import scala.io.Source
import scala.io.Source

scala> val inputFile = Source.fromFile("output.txt")
inputFile: scala.io.BufferedSource = non-empty iterator

scala> val lines = inputFile.getLines
lines: Iterator[String] = non-empty iterator

scala> for (line<- lines) println(line)
1
2
3
4
5
```

## 四、控制结构

### 4.1 if 条件表达式
```scala
val x=6
if (x>0){
    println("This is a positive number")
} else{
    println("This is not a positive number")
}
```

if表达式的值可以赋值给变量
```scala
scala> val x = 6
x: Int = 6

scala> val a = if (x>0) 1  else -1
a: Int = 1
```

### 4.2 while循环
- while(){}
```scala
var i = 9
while (i > 0){
    i -= 1
    printf("i is %d\n", i)
}
```
- do{} while()
```scala
var i = 0
do{
    i += 1
    println(i)
}while(i<5)
```

### 4.3 for循环

- for循环语句格式
```scala
for (变量<-表达式) 语句块
```
 - 其中“变量<-表达式”被称为生成器
 
```scala
scala> for(i <- 1 to 5) println(i)
1
2
3
4
5

// 改变步长
scala> for(i <- 1 to 5 by 2) println(i)
1
3
5

// 过滤－－守卫表达式
scala> for(i <- 1 to 5 if i%2==0) println(i)
2
4

// 多个生成器，用分号隔开
scala> for(i <- 1 to 3; j <- 1 to 3) println(i*j)
1
2
3
2
4
6
3
6
9

scala> for(i<- 1 to 5 if i%2== 0; j <- 1 to 3 if j != i) println(i*j)
2
6
4
8
12
```

- for推导式
 - 对过滤后的结果进一步处理，采用**yield关键字**，对过滤后的结果构建一个集合
```scala
scala> for(i<- 1 to 5 if i%2==0) yield i
res22: scala.collection.immutable.IndexedSeq[Int] = Vector(2, 4)
```

## 五、数据结构

### 5.1 数组

- 定长数组
  - 整型数组
```scala
scala> val intValueArr = new Array[Int](3)   # 声明一个长度为3的整型数组，每个元素初始化为0
intValueArr: Array[Int] = Array(0, 0, 0)

scala> intValueArr(0) = 12
scala> intValueArr(1) = 13

scala> for (i <- intValueArr) println(i)
12
13
0
```
在Scala中，对数组元素的引用，**使用圆括号**

  - 字符串数组
```scala
scala> val myStrArr = new Array[String](3)
myStrArr: Array[String] = Array(null, null, null)

scala> myStrArr(0) = "BigData"
scala> myStrArr(1) = "Hadoop"
scala> myStrArr(2) = "Spark"

scala> for (i <- 0 to 2) println(myStrArr(i))
BigData
Hadoop
Spark
```

- 更简洁的数组声明和初始化方法
```scala
scala> val intValueArr = Array(12, 45, 33)
intValueArr: Array[Int] = Array(12, 45, 33)

scala> val myStrArr = Array("BigData", "Hadoop", "Spark")
myStrArr: Array[String] = Array(BigData, Hadoop, Spark)
```
Scala会自动根据提供的初始化数据来判断出数组的类型

### 5.2 列表List

- **列表中各个元素必须是相同类型**

```scala
scala> val intList = List(1,2,3)
intList: List[Int] = List(1, 2, 3)
```
- 列表有**头部 initList.head** 和**尾部 initList.tail**的概念，其中，**头部是一个元素，尾部还是一个列表**

- **:: 操作**，在列表的头部增加新元素，得到一个新的列表，原列表不会发生变化
```scala
scala> val intListOther = 0::intList
intListOther: List[Int] = List(0, 1, 2, 3)
```
- 采用右结合的方式构建列表，其中Nil表示空列表
```scala
scala> val intList = 1::2::3::Nil
intList: List[Int] = List(1, 2, 3)
```

- 使用**:::操作符**对不同的列表进行连接得到新的列表
```scala
scala> val intList1 = List(1,2)
intList1: List[Int] = List(1, 2)
scala> val intList2 = List(3,4)
intList2: List[Int] = List(3, 4)

scala> val intList3 = intList1 ::: intList2
intList3: List[Int] = List(1, 2, 3, 4)
```
- 列表常用方法
 - 求和list.sum
 ```scala
 scala> intList.sum
 res36: Int = 6
 ```
 
 ### 5.3 元组tuple
 
 - 元组是不同类型的值的聚集，也就是可以包含不同类型的元素
 - 使用圆括号把多个元素包围起来就可以
 
 ```scala
 scala> val tuple = ("Spark", 2017, 3.31)
tuple: (String, Int, Double) = (Spark,2017,3.31)

scala> println(tuple._1)
Spark
```

### 5.4 集set

- 集是不重复元素的集合
- 列表中的元素是按照插入的先后顺序来组织的；集中的元素并不会记录元素的插入顺序，以**哈希方法对元素的值进行组织**，所以能快速地找到某个元素
- 分为可变集和不可变集，缺省情况下创建的是不可变集，通常使用不可变集
- 可变集和不可变集都有增删元素的操作，但是二者有很大的区别。
  - 对不可变集进行操作，会产生一个新的集，原来的集并不会发生变化
  - 对可变集进行操作，改变的是该集本身

- 创建不可变集***var*
```scala
scala> var mySet = Set("Hadoop", "Spark")
mySet: scala.collection.immutable.Set[String] = Set(Hadoop, Spark)

scala> mySet += "Scala"

scala> println(mySet.contains("Scala"))
true

scala> println(mySet)
Set(Hadoop, Spark, Scala)
```
这里使用的是**var变量**，如果使用val，在进行集修改的时候会报错

- 创建可变集，需要引入scala.collection.mutable.Set包
```scala
scala> import scala.collection.mutable.Set
import scala.collection.mutable.Set

scala> val myMutableSet = Set("Database", "BigData")
myMutableSet: scala.collection.mutable.Set[String] = Set(BigData, Database)

scala> myMutableSet += "Cloud Computing"
res41: myMutableSet.type = Set(BigData, Cloud Computing, Database)

scala> println(myMutableSet)
Set(BigData, Cloud Computing, Database)
```
这里声明的是**val变量，由于是可变集**，因此不会抱错

### 5.5 映射 Map

- Map是一系列键值对的集合，建立了键和值之间的对应关系
- 所有的值都是通过键来获取
- 包括可变和不可变两种，默认情况下创建的是不可变映射；**引入scala.collection.mutable.Map包创建可变映射**
- 不可变映射，无法更新映射中的元素，也无法增加新的元素

```scala
scala> val university = Map("THU"->"Tsinghua University", "PKU"->"Peking University")
university: scala.collection.immutable.Map[String,String] = Map(THU -> Tsinghua University, PKU -> Peking University)

scala> println(university("THU"))
Tsinghua University
```
- 检查Map中是否包含某个值，使用contains方法
```scala
scala> val thu = if (university.contains("THU")) university("THU") else 0
thu: Any = Tsinghua University

scala> println(thu)
Tsinghua University
```
- 可变映射
```scala
scala> import scala.collection.mutable.Map
import scala.collection.mutable.Map

// 创建可变映射
scala> val university2 = Map("THU"->"Tsinghua University", "PKU"->"Peking University")
university2: scala.collection.mutable.Map[String,String] = Map(THU -> Tsinghua University, PKU -> Peking University)

// 更新已有元素的值
scala> university2("PKU") = "Beijing University"

// 添加新元素
scala> university2("BUPT") = "Posts and Telecom"

// 使用+=操作添加新元素
scala> university2 += ("FDU" -> "FuDan University")
res48: university2.type = Map(BUPT -> Posts and Telecom, THU -> Tsinghua University, FDU -> FuDan University, PKU -> Beijing University)
```

- 循环遍历映射
```scala
for ((k, v) <- 映射) 语句块
```

举个例子
```scala
scala> for((k, v) <- university) printf("%s is: %s\n", k, v)
THU is: Tsinghua University
PKU is: Peking University
```
- 只打印所有键
```scala
scala> for(k <- university.keys) println(k)
THU
PKU
```

- 只打印所有的值
```scala
scala> for(v <- university.values) println(v)
Tsinghua University
Peking University
```

### 5.6 迭代器 Iterator
- 迭代器不是一个集合，但是提供了访问集合的一种方法
- 当构建一个集合需要很大的开销（比如把一个文件的所有行都写入内存），迭代器就可以发挥很好的作用
- 两个基本操作:hasNext用于检测迭代器是否还有下一个元素，next或者next()返回下一个元素

```scala
scala> var iter = Iterator("Hadoop", "Spark", "Scala")
iter: Iterator[String] = non-empty iterator

scala> while (iter.hasNext){
     |     println(iter.next)
     | }
Hadoop
Spark
Scala
```

上述操作执行结束后，迭代器会移动到末尾，就不能再使用了

- for循环方式
```scala
cala> var iter = Iterator("Hadoop", "Spark", "Scala")
iter: Iterator[String] = non-empty iterator

scala> for (elem <- iter){
     |     println(elem)
     | }
Hadoop
Spark
Scala
```

## 六、类 class

### 6.1 class模版

```scala
class className{
    private 字段
    def 方法
}
```
```scala
class Counter{
    private var value = 0
    def increment(): Unit = {value += 1}
    def current(): Int = {value}
}

val myCounter = new Counter
myCounter.increment()
println(myCounter.current)
```
- 把value字段设置为private，外界无法访问，只有在类内部可以访问该字段；如果字段前面没有修饰符，默认为public，外部也可以访问该字段
- increment()是方法，没有参数，冒号后面的Unit表示不返回任何值；方法的返回值，不需要return，方法的最后一个值就是方法的返回值
- 如果方法中只有一行语句，可以直接去掉大括号
- 如果没有返回值，可以去掉**: Unit = **
- 调用无参方法时，可以省略方法名后面的圆括号

```linux
$ scala test.scala
1
```
- 需要将声明都封装在对象中才能被编译（执行scalac）
```scala
class Counter{
    private var value = 0
    def increment(step: Int): Unit = {value += step}
    def current(): Int = {value}
}

object MyCounter{
    def main(args: Array[String]){
        val myCounter = new Counter
        myCounter.increment(5)
        println(myCounter. current)
    }
}
```
```linux
$ scalac test.scala
$ scala -classpath . MyCounter
5
```

### 6.2 getter和setter方法
 
- 给类中的字段设置值以及读取值
- 读取值：定义一个方法，方法的名称就是我们想要的字段的名称
- 设置值：定义一个方法
```scala
class Counter{
    private var privateValue = 0
    def value = privateValue
    def value_=(newValue: Int){
        if (newValue > 0) privateValue = newValue
    }
    def increment(step: Int): Unit = {value += step}
    def current(): Int = {value}
}

object MyCounter{
    def main(args: Array[String]){
        val myCounter = new Counter
        println(myCounter.value)
        myCounter.value = 3
        println(myCounter.value)
        myCounter.increment(1)
        println(myCounter. current)
    }
}
```
测试
```linux
$ scalac test.scala 
$ scala -classpath . MyCounter
0
3
4
```

### 6.3 辅助构造器
- Scala构造器包含1个主构造器和若干个辅助构造器
- 辅助构造器的名称为this
- 每个辅助构造器都必须调用一个此前已经定义的辅助构造器或主构造器
- 有点函数重构的意思

```scala
class Counter{
    private var value = 0
    private var name = ""
    private var mode = 1
    
    def this(name: String){   # 第一个辅助构造器
        this()    # 调用主构造器
        this.name = name
    }

    def this(name: String, mode: Int){
        this(name)    # 调用前一个辅助构造器
        this.mode = mode
    }

    def increment(step: Int): Unit = {value += step}
    def current(): Int = {value}
    def info(): Unit = {printf("Name:%s and mode is %d\n", name, mode)}
}

object MyCounter{
    def main(args: Array[String]){
        val myCounter1 = new Counter
        val myCounter2 = new Counter("Runner")
        val myCounter3 = new Counter("Timer",2)

        myCounter1.info()
        myCounter1.increment(1)
        printf("Current Value is: %d\n", myCounter1.current)

        myCounter2.info()
        myCounter2.increment(2)
        printf("Current Value is: %d\n", myCounter2.current)

        myCounter3.info()
        myCounter3.increment(3)
        printf("Current Value is: %d\n", myCounter3.current)
    }
}

```

- 编译和运行
```linux
$ scalac test.scala

$ scala -classpath . MyCounter
Name: and mode is 1
Current Value is: 1
Name:Runner and mode is 1
Current Value is: 2
Name:Timer and mode is 2
Current Value is: 3
```

### 6.4 主构造器
- Scala的每个类都有主构造器
- Scala的主构造器是整个类体，需要在类名称后面罗列出构造器所需的所有参数，这些参数被编译成字段，字段的值就是创建对象时传入的参数的值

```scala
class Counter(val name: String, val mode:Int){
    private var value = 0
    def increment(step: Int): Unit={value += step}
    def current(): Int = {value}
    def info(): Unit={printf("Name: %s and mode is %d\n", name, mode)}
}

object MyCounter{
    def main(args: Array[String]){
        val myCounter = new Counter("Timer", 2)
        myCounter.info
        myCounter.increment(1)
        printf("Current Value is: %d\n", myCounter.current)
    }
}
```
编译和执行
```linux
$ scalac test_2.scala

$ scala -classpath . MyCounter
Name: Timer and mode is 2
Current Value is: 1
```

## 七、对象 object

- 采用object关键字
### 7.1 单例对象
```scala
object Person{
    private var lastId = 0
    def newPersonId() = {
        lastId += 1
        lastId
    }
}

printf("The first person id is %d.\n",  Person.newPersonId())
printf("The second person id is %d.\n", Person.newPersonId())
printf("The third person id is %d.\n", Person.newPersonId())
```

```linux
$ scala test_3.scala 
The first person id is 1.
The second person id is 2.
The third person id is 3.
```

### 7.2 伴生对象

- 单例对象与某个类具有相同的名称
- 类和它的伴生对象必须存在于同一个文件中，而且可以相互访问私有成员（字段和方法）

```scala
class Person{
    private val id = Person.newPersonId()  # 调用了伴生对象中的方法
    private var name = ""

    def this(name: String){
        this()
        this.name = name
    }
    def info() {printf("The id of %s is %d.\n", name, id)}
}

object Person{
    private var lastId = 0
    def newPersonId() = {
        lastId += 1
        lastId
    }
    
    def main(args: Array[String]){
        val person1 = new Person("Jm")
        val person2 = new Person("Mx")
        
        person1.info
        person2.info
    }
}
```

```linux
$ scalac test_3.scala

$ scala Person
The id of Jm is 1.
The id of Mx is 2.
```
object中方法带private和不带private的区别：
```linux
$ javap Person
Compiled from "test_3.scala"
public class Person {
  public static void main(java.lang.String[]);
  public void info();
  public Person();
  public Person(java.lang.String);
}


$ javap Person
Compiled from "test_3.scala"
public class Person {
  public static void main(java.lang.String[]);
  public static int newPersonId();
  public void info();
  public Person();
  public Person(java.lang.String);
}
```
- 经过编译后，伴生类Peroson中成员和伴生对象Person中的成员都被合并到一起，如果不去掉private修饰符，作为伴生对象的私有方法，在javap反编译后，在执行结果中是看不到private修饰的方法的

### 7.3 应用程序对象

- 每个Scala应用程序必须从一个对象的main方法开始

### 7.4 apply方法和update方法

## 8. 继承
- 重写一个非抽象方法必须使用override修饰符
- 只有主构造器可以调用超类的主构造器
- 在子类中重写超类的抽象方法，不需要使用override关键字
- 可以重写超类中的字段

### 8.1 抽象类
- 创建一个抽象类，这个类可以被其他类继承
- 定义一个抽象类，需要使用关键字abstract
- 定义一个抽象类的抽象方法，不需要关键字abstract，只要把方法体空着，不写方法体就可以
- 抽象类中定义的字段，只有没有给出初始化值，就表示为一个抽象字段；但是，抽象字段必须要声明类型
- 抽象类不能直接被实例化
```scala
abstract class Car{
    val carBrand: String # 抽象字段
    def info() # 抽象方法
    def greeting() {println("Welcome to my car!")}
}
```
### 8.2 扩展类
- 定义扩展类，扩展（继承）抽象类
- 重写超类字段，需要使用override字段
- 重写超类的抽象方法，不需要使用override关键字，如果加上，编译也不会报错
- 重写超类的非抽象方法，必须使用override关键字
```scala
class BMWCar extends Car{
    override val carBrand = "BMW"
    def info() {printf("This is a %s car. It is on sale", carBrand)}

    override def greeting() {println("Welcome to my BMW car!")}
}

class BYDCar extends Car{
    override val carBrand = "BYD"
    def info() {printf("This is a %s car. It is on sale", carBrand)}

    override def greeting() {println("Welcome to my BYD car!")}
}

object MyCar{
    def main(args: Array[String]){
        val myCar1 = new BMWCar()
        val myCar2 = new BYDCar()

        myCar1.greeting()
        myCar1.info()
        myCar2.greeting()
        myCar2.info()
    }

}
```

```linux
$ scalac test_4.scala

$ scala -classpath . MyCar
Welcome to my BMW car!
This is a BMW car. It is on saleWelcome to my BYD car!
```

## 九、特质trait

## 十、模式匹配

### 10.1 简单匹配

```scala
val colorNum = 1
val colorStr = colorNum match{
    case 1 => "red"
    case 2 => "green"
    case 3 => "yellow"
    case _ => "Not Allowed"
}

println(colorStr)
```

```linux
$ scala test_5.scala
red
```

```scala
val colorNum = 4
val colorStr = colorNum match{
    case 1 => "red"
    case 2 => "green"
    case 3 => "yellow"
    case unexpected => unexpected + " is Not Allowed"
}

println(colorStr)
```

```linux
$ scala test_5.scala
4 is Not Allowed
```

### 10.2 类型模式
- 对表达式的类型进行匹配

```scala
for (elem <- List(9, 12.3, "Spark", "Hadoop", 'Hello)){
    val str  = elem match{
        case i: Int => i + " is an int value."
        case d: Double => d + " is a double value."
        case "Spark" => "Spark is found."
        case s: String => s + " is a string value."
        case _ => "This is an unexpected value."
    }

    println(str)
}
```

```linux
$ scala test_5.scala 
9 is an int value.
12.3 is a double value.
Spark is found.
Hadoop is a string value.
This is an unexpected value.
```

### 10.3 守卫（guard）语句

```scala
for (elem <- List(1,2,3,4)){
    elem match {
        case _ if (elem % 2 == 0) => println(elem + " is even.")
        case _ => println(elem + " is odd.")
    }
}
```

```linux
$ scala test_5.scala 
1 is odd.
2 is even.
3 is odd.
4 is even.
```

## 十一、函数式编程

- 函数式编程一个重要特性就是值不可变性，这对于编写可扩展的并发程序而言可以带来巨大好处，因为它避免了对公共的可变状态进行同步访问控制的复杂问题

### 11.1 匿名函数
```scala
(参数) => 表达式  // 如果参数只有一个，参数的圆括号可以省略
```
- 直接把匿名函数存放到变量中
```scala
scala> val myNumFunc: Int => Int = (num: Int) => num*2
myNumFunc: Int => Int = $$Lambda$1043/1689723487@33db72bd

scala> println(myNumFunc(3))
6

// 类型推断机制，但不能全部省略
scala> val myNumFunc = (num: Int) => num*2
myNumFunc: Int => Int = $$Lambda$1057/1069350529@7a1f45ed

scala> println(myNumFunc(3))
6
```

### 11.2 闭包
```scala
object MyTest{
    def main(args: Array[String]) : Unit = {
        def plusStep(step: Int) = (num: Int) => num + step
        val myFunc = plusStep(3) // 给自由变量step赋值
        println(myFunc(10))  
    }
}
```
- plusStep函数
    - step：自由变量，只有在运行的时候才能确定
    - num：函数变量，只有在调用的时候才被赋值
    - 闭包，从开放到封闭的过程
```linux
$ scala test_6.scala
13
```
- 再看一个例子
```
scala> var more = 1
more: Int = 1

scala> val addMore = (x: Int) => x + more
addMore: Int => Int = $$Lambda$1060/361712894@35f639fa

scala> addMore(10)
res2: Int = 11

scala> more = 9
more: Int = 9

scala> addMore(10)
res3: Int = 19
```
- 每个闭包都会访问闭包创建时活跃的自由变量more

### 11.3 遍历操作
- 列表遍历
```scala
scala> val list  = List(1,2,3,4)
scala> for (elem <- list) println(elem)
1
2
3
4

scala> list.foreach(elem => println(elem))
1
2
3
4
```
- 映射遍历
```scala
scala> val university = Map("TUH" -> "Tsinghua University", "PKU" -> "Peking University")
university: scala.collection.immutable.Map[String,String] = Map(TUH -> Tsinghua University, PKU -> Peking University)

scala> for ((k, v) <- university) printf("Code is: %s and name is: %s\n", k, v)
Code is: TUH and name is: Tsinghua University
Code is: PKU and name is: Peking University

scala> for(k <- university.keys) println(k)
TUH
PKU

scala> for (v <- university.values) println(v)
Tsinghua University
Peking University

scala> university foreach {case(k, v) => println(k +":" + v)}
TUH:Tsinghua University
PKU:Peking University

scala> university foreach {kv => println(kv._1 + ":" + kv._2)}
TUH:Tsinghua University
PKU:Peking University
```

### 11.4 map操作
- map操作是针对集合的典型的变换操作，它将某个函数应用到集合中的每个元素，并产生一个结果集合
```scala
scala> val books = List("Hadoop", "Hive", "HDFS")
books: List[String] = List(Hadoop, Hive, HDFS)

scala> books.map(s => s.toUpperCase)
res12: List[String] = List(HADOOP, HIVE, HDFS)
```
- flatMap是map的一种扩展，函数对每个输入返回一个集合，flapMap把生成的多个集合合并为一个集合
```scala
scala> books flatMap (s => s.toList)
res13: List[Char] = List(H, a, d, o, o, p, H, i, v, e, H, D, F, S)

scala> books.flatMap(s => s.toList)
res14: List[Char] = List(H, a, d, o, o, p, H, i, v, e, H, D, F, S)
```
```scala
a 方法 b
a.方法(b)
```

### 11.5 filter操作
- 遍历一个集合从中获取满足制定条件的元素组成的一个集合

```scala
scala> val universityOfTsu = university filter {kv => kv._2 contains "Tsinghua"} 
universityOfTsu: scala.collection.immutable.Map[String,String] = Map(TUH -> Tsinghua University)

scala> println(universityOfTsu)
Map(TUH -> Tsinghua University)

scala> val universityOfP = university filter {kv => kv._2 startsWith "P"}
universityOfP: scala.collection.immutable.Map[String,String] = Map(PKU -> Peking University)

scala> println(universityOfP)
Map(PKU -> Peking University)
```

### 12.6 reduce操作
- reduce二元操作对集合中的元素进行归约
```scala
scala> val list = List(1, 2, 3, 4)
list: List[Int] = List(1, 2, 3, 4)

scala> list.reduceLeft(_ + _)
res17: Int = 10

scala> list.reduceRight(_ + _)
res18: Int = 10

scala> list.reduce(_ + _)  // 默认left
res19: Int = 10
```

### 12.7 fold操作
- 类似于reduce。fold操作需要从一个初始的“种子”值开始，并以该值作为上下文，处理集合中的每个元素
```scala
scala> list.fold(10)(_*_)
res21: Int = 240
```
