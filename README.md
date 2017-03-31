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

// 输入以下内容，其中SCALA_HOME是放置Scala的位置
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
