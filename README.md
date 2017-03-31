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

- val
 - 不可变，在声明时必须被初始化，初始化以后就不能再被赋值
- var
 - 可变，声明的时候需要进行初始化，初始化以后还可以再次对其赋值

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

 
