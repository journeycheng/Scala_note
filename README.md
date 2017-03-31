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
# 单行输出
scala> print("My name is:"); print("JM")
My name is:JM

# 换行输出
scala> println("My name is:"); println("JM")
My name is:
JM

# 格式化输出
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

# 改变步长
scala> for(i <- 1 to 5 by 2) println(i)
1
3
5

# 过滤－－守卫表达式
scala> for(i <- 1 to 5 if i%2==0) println(i)
2
4

# 多个生成器，用分号隔开
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

# 创建可变映射
scala> val university2 = Map("THU"->"Tsinghua University", "PKU"->"Peking University")
university2: scala.collection.mutable.Map[String,String] = Map(THU -> Tsinghua University, PKU -> Peking University)

# 更新已有元素的值
scala> university2("PKU") = "Beijing University"

# 添加新元素
scala> university2("BUPT") = "Posts and Telecom"

# 使用+=操作添加新元素
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
