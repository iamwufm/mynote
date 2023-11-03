---
author: wufm
title: Scala学习
rating: 5
time: 2023-10-28 周六
tags:
  - Scala
代码:
---
## 一、Scala概述

官网：[The Scala Programming Language (scala-lang.org)](https://www.scala-lang.org/)

Scala 是一种多范式的编程语言，其设计的初衷是要==集成面向对象编程和函数式编程==的各种特 性 。==Scala 运行于 Java平台== （ Java 虚 拟 机 ），==并兼容现有的 Java 程序== 。

==为什么要学Scala：==
- Spark—新一代内存级大数据计算框架，是大数据的重要内容。
- Spark就是使用Scala编写的。因此为了更好的学习Spark, 需要掌握Scala这门语言。
- Spark的兴起，带动Scala语言的发展！


代码量对比：（定义一个方法，把orders订单的产品信息放入一个集合中）

```java
public List<Product> getProducts(){  
    List<Product> products = new ArrayList<>();  
    for (Order order:orders)(  
            products.addAll (order.getProducts()));  
    return produets;  
}
```

```Scala
def products = orders.flatMap(0 => o.products)
```

学习目标：
- （初级）熟练使用 scala 编写 Spark 程序
- （中级）动手编写一个简易 Spark 通信框架
- （高级）为阅读 Spark 内核源码做准备
## 二、Scala安装

见[[Scala安装]]
## 三、Scala基础

代码包路径：com.bigdata.scala.basic
### 3.1 数据类型

Scala 和 Java 一样，有 7 种数值类型 Byte、Char、Short、Int、Long、Float 和 Double（无包装类型）和 Boolean、Unit 类型.

==注意:Unit 表示无值，和其他语言中 void 等同。用作不返回任何结果的方法的结果类型。Unit只有一个实例值，写成()。==

![[数据类型.png|400]]

### 3.2 变量的定义

格式：==var|val 变量名称 [: 数据类型 ]= 变量值==

- 定义变量使用 var 或者 val 关键字
- 使用 val 修饰的变量, 值不能为修改, 相当于 java 中 final 修饰的变量。对于对象来说，就是地址不可变
- 使用 var 修饰的变量, 值可以修改
- 定义变量时, 可以指定数据类型, 也可以不指定, 不指定时编译器会自动推测变量的数据类型

```scala
object ScalaVar {  
  def main(args: Array[String]): Unit = {  
  
    var name = "zhangsan"  
    name = "lisi"  
  
    val name2 = "zhaosi"  
    //    name2 = "wangwu"  
  
    var age: Int = 18  
  
    println(name)  
  
  }  
}
```

![[Pasted image 20231028132047.png|275]]
### 3.3 字符串的格式化输出

三种形式：

```scala
object ScalaPrint {  
  def main(args: Array[String]): Unit = {  
    val name = "巧克力"  
    val price = 2880.135  
    val url = "http://xx/sss"  
  
    println("--------1. 普通打印---------------")  
    println("产品名称：" + name, "价格：" + price)  
  
    println("--------2.文字'f'插值器允许创建一个格式化的字符串，类似于 C 语言中的 printf。---------------")  
    // 在使用'f'插值器时，所有变量引用都应该是 printf 样式格式说明符，如％d，％i，％f 等。  
    println(f"产品名称：$name%s，价格：$price%1.2f，网址是$url ")  
    println(f"产品名称：$name，价格：$price，网址是$url ")  
    printf("产品名称：%s，价格：%1.2f，网址是%s ", name, price, url) // 不会换行  
    println()  
  
    println("--------3.'s'允许在处理字符串时直接使用变量---------------")  
    // 字符串插入器还可以处理任意表达式。  
    println(s"产品名称：$name，价格：$price，网址是$url")  
    println(s"1+1=${1 + 1}")  
  
    val stu = new Student("taotao", 18)  
    println(s"${stu.name}")  
  
    println("--------多行字符串，在 Scala中，利用三个双引号包围多行字符串就可以实现---------------")  
    val str =  
      """  
        |select        
        |name,age        
        |from user;        
        """.stripMargin  
    println(str)  
  
  }  
}  
  
class Student(val name: String, var age: Int)
```

```txt
--------1. 普通打印---------------
(产品名称：巧克力,价格：2880.135)
--------2.文字'f'插值器允许创建一个格式化的字符串，类似于 C 语言中的 printf。---------------
产品名称：巧克力，价格：2880.14，网址是http://xx/sss 
产品名称：巧克力，价格：2880.135，网址是http://xx/sss 
产品名称：巧克力，价格：2880.14，网址是http://xx/sss 
--------3.'s'允许在处理字符串时直接使用变量---------------
产品名称：巧克力，价格：2880.135，网址是http://xx/sss
1+1=2
taotao
--------多行字符串，在 Scala中，利用三个双引号包围多行字符串就可以实现---------------

select
name,age
from user;
```

### 3.4 条件表达式

if ... else if ... else 代码较多时可以使用代码块{}

```scala
object ScalaIf {  
  
  def main(args: Array[String]): Unit = {  
    var age: Int = 18  
  
    // 如果年龄是18就是一枝花  
    var value = if (age == 18) "一枝花" else "不是一枝花"  
    printf(value)  
  
    var value2 = if (age == 18) {  
      "一枝花"  
      // 不需要写return，最后一行就是返回值  
      "哈哈哈"  
    }  
  
    var value3 = if (age > 10) age  
  
    var value4 = if (age < 18) "年龄小于18" else if (age == 18) "一枝花" else age  
  
  }  
  
}
```

### 3.5 循环语句/yeil关键字

在 scala 中有 for 循环和 while 循环，用 for 循环比较多

for 循环语法结构：for (i <- 表达式/数组/集合)

```scala
object ScalaFor {  
  def main(args: Array[String]): Unit = {  
    // 定义一个数组  
    val array = Array(1, 2, 3, 4, 5)  
  
    for (elem <- array) println(elem)  
    for (ele <- 0 to 4) println(ele) // 0 to 5 => 会生成一个范围集合 Range(0,1,2,3,4)    for (i <- 0 until 4) println(array(i)) // 0 until 4 => 会生成一个范围集合 Range(0,1,2,3)  
    // 嵌套循环  
    for (i <- 1 to 3; j <- 1 to 3 if i != j) {  
      println(s"10 * $i + $j =" + (10 * i + j) + "\t")  
    }  
  
    // 把结果放入一个新的集合中  
    // 选取偶数  
    val array2 = for (e <- array if e % 2 == 0) yield e  
  
    // 循环步长  
    for (i <- 1 to 5 by 2) println(i) // 1,3,5  
  
  }  
  
}
```

### 3.6 运算符/运算符重载

Scala 中的+ - * / %等操作符的作用与 Java 一样，位操作符 & | ^ >> <<也一样。只是有一点特别的：这些操作符实际上是方法。例如：

a + b 是如下方法调用的简写：a.+(b)

a 方法 b 可以写成 a.方法(b)
### 3.7 方法的定义与调用

格式：def methodName ([list of parameters]) [: return type] = {}

方法的返回值类型可以不写，编译器可以自动推断出来，但是对于递归函数，必须指定返回

```scala
object ScalaMethod {  
  def main(args: Array[String]): Unit = {  
    // 定义方法  
    def sum(num1: Int, num2: Int) = num1 + num2  
  
    // 调用  
    val result = sum(1, 4)  
  
    println(result)  
  
    def printHell1() = println("hello scala")  
    def printHello2 = println("hello spark")  
  
    // 调用  
    printHell1 // 没有参数输入，可以 printHell1 或 printHell1() 调用  
    printHell1()  
    printHello2 // 这个只能这样调用  
  
  }  
  
}
```

==方法可转换为函数==

![[Pasted image 20231028215400.png]]
### 3.8 函数的定义与调用

格式（关键 =>）：
- (函数参数列表) => 函数体
- 函数名称:(参数类型) =>函数返回值=(参数引用)=>函数体

```scala
object ScalaFunction {  
  def main(args: Array[String]): Unit = {  
    // 定义函数  
    val f1 = (x: Int) => x * 10  
  
    //调用  
    println(f1(5))  
  
    // 匿名函数  
    (x: Int, y: Int) => x * y  
  
      // 定义函数  
      val f2: (Int, Int) => Int = (x, y) => x * y  
  
      // 调用  
      println(f2(3, 5))  
  
  }  
}
```

![[Pasted image 20231028221131.png|400]]

无参函数

```scala
val f4:()=>Int=()=>2
```
### 3.9 传值调用与传名调用

* 传值调用：在值作为参数传递给方法  
* 传名调用：把方法名称（函数）作为参数传递给方法（即函数可以作为参数）

```scala
object CallByValueAndName {  
  // 钱包总金额  
  var money = 50;  
  
  // 每次扣5块钱  
  def huaQian(): Unit = {  
    money = money - 5  
  }  
  
  // 数钱，看看卡里还有多少  
  def shuQian(): Int = {  
    huaQian()  
    money  
  }  
  
  // 传值  
  def printByValue(x: Int) = {  
    for (a <- 0 until 3) println(s"每次都算算还剩：${x}元")  
  }  
  
  // 传名  
  // => 是一个函数的标志。需要一个没有参数，返回值为Int类型的函数  
  def printByName(x: => Int) = {  
    for (a <- 0 until 3) println(s"每次都算算还剩：${x}元")  
  }  
  
  def main(args: Array[String]): Unit = {  
    // 传值调用  
    // 1.计算shuQian的返回值=45  
    // 2.将45作为参数参数传入printByValue  
    printByValue(shuQian)  
  
    // 传名调用  
    // 将shuQian方法名称（会自动转为函数）传递到方法的内部执行  
    printByName(shuQian)  
  }  
  
}
```

![[Pasted image 20231030095216.png|300]]
### 3.10 可变参数函数

```scala
object ManyParams {  
  def main(args: Array[String]): Unit = {  
    // 方法的调用  
    methodManyParams("hello","scala","hello python")  
  }  
  
  def methodManyParams(a: String*): Unit ={  
    for (elem <- a) println(elem)  
  }  
}
```

```txt
hello
scala
hello python
```
### 3.11 默认参数值函数

```scala
object DefaultValueParams {  
  def main(args: Array[String]): Unit = {  
    // 调用  
    add()  
    add(4)  
    add(y = 10,x = 4)  
  }  
  
  def add(x: Int = 2, y: Int = 3): Int = {  
    println(s"x + y = ${x+y}")  
    x + y  
  }  
  
}
```

### 3.12 高阶函数

高阶函数: 参数是函数 或者 返回值是函数

```scala
object HightFunc {  
  def main(args: Array[String]): Unit = {  
    def f1(f: Int => String, v: Int) = f(v)  
  
    def f2(x: Int) = "[" + x.toString + "]"  
  
    // 调用方法  
    println(f1(f2, 4))  
  
    // 传递的函数有两个参数  
    def add(x: Int, y: Int, op: (Int, Int) => Int) = op(x, y)  
    val f3 = (x: Int, y: Int) => x + y  
    
    // 标准版  
    add(2, 3, f3)  
    add(2, 3, (x: Int, y: Int) => x + y)  
    // 参数的类型可以省略  
    add(2, 3, (x, y) => x + y)  
    // 如果参数只出现一次，则参数省略且后面参数可以用_代替  
    add(2, 3, _ + _)  
  
  }  
  
}
```

高阶函数案例

````tab
```text
tab: map 映射

```scala
object HightFuncDemo {  
  def main(args: Array[String]): Unit = {  
    val arr = Array(1, 2, 3, 4)
	println("-------------------map 映射------------------------")  
	  
	def map(arr: Array[Int], op: Int => Int) = {  
	  for (elem <- arr) yield op(elem)  
	}  
	// arr.map((x: Int) => x * 10)  
	map(arr, (x: Int) => x * 10)  
	val arr1 = map(arr, _ * 10)  
	println(arr1.mkString(","))
	 
   }  
  
}
```

tab: filter 过滤

```scala
object HightFuncDemo {  
  def main(args: Array[String]): Unit = {  
    val arr = Array(1, 2, 3, 4)
	println("-------------------filter 过滤------------------------")  
	//    arr.filter()  
	def filter(arr: Array[Int], op: Int => Boolean) = {  
	  val arrBuffer = ArrayBuffer[Int]()  
	  for (elem <- arr if op(elem)) {  
	    arrBuffer += elem  
	  }  
	  arrBuffer.toArray  
	}  
	  
	// arr.filter(_ > 2)  
	filter(arr, (x: Int) => x > 2)  
	val arr2 = filter(arr, _ > 2)  
	println(arr2.mkString(","))
	
	}  
}
```
tab: reduce 聚合

```scala
object HightFuncDemo {  
  def main(args: Array[String]): Unit = {  
    val arr = Array(1, 2, 3, 4)
	println("-------------------reduce 聚合------------------------")  
	  
	def reduce(arr: Array[Int], op: (Int, Int) => Int) = {  
	  var total = arr(0)  
	  for (elem <- 1 until arr.length) {  
	    total = op(total, arr(elem))  
	  }  
	  total  
	}  
	  
	//    arr.reduce(_ + _)  
	reduce(arr, (x, y) => x + y)  
	val result = reduce(arr, _ + _)  
	println(result)
	
	}  
}
```
````

### 3.13 部分参数应用函数

如果只传递几个参数并不是全部参数，那么将返回部分应用的函数。这样就可以方便地绑定一些参数，其余的参数可稍后填写补上。

```scala
object PartParamFunc {  
  def main(args: Array[String]): Unit = {  
  
    def add(x: Int, y: Int) = x + y  
  
    // 类似 val add1 = (x:Int)=> add(2,x)    
    val add1 = add(2,_:Int) // add1: Int => Int = $Lambda$1036/695660374@4345fd45  
  
    println(add1(4))  
  }  
  
}
```

### 3.14 柯里化(Currying)

柯里化(Currying)指的是将原来接受两个参数的函数变成新的接受一个参数的函数的过程。新的函数返回一个以原有第二个参数为参数的函数。

```scala
object ScalaCurrying {  
  
  def main(args: Array[String]): Unit = {  
    def add(x: Int, y: Int) = x + y  
  
    // 把add变成柯里化。经过柯里化之后，函数的通用性有所降低，但是适应性有所提高  
    def add1(x: Int)(y: Int) = x + y  
  
    println(add1(2)(3))  
  
    // 分析下其演变过程  
    def add3(x: Int) = (y: Int) => x + y  
  
    // (y: Int) => x + y 为一个匿名函数, 也就意味着 add 方法的返回值为一个匿名函数  
  
    val result = add3(5) // Int => Int = $Lambda$1076/1792476109@2c59b9f4  
  
    println(result(3))  
    println(result(8))  
  }  
  
}
```

### 3.15 偏函数

被包在花括号内没有 match 的一组 case 语句是一个偏函数，它是 PartialFunction[A, B]的一个实例，A 代表参数数据类型，B 代表返回类型，常用作输入模式匹配。

==偏函数：PartialFunction[参数类型，返回值类型]==

```scala
object ScalaPartialFunction {  
  
  def main(args: Array[String]): Unit = {  
  
    def func(str: String): Int = {  
      if (str.equals("a")) 97  
      else 0  
    }  
  
    // 偏函数 PartialFunction[String, Int] 中String为输入参数数据类型，Int为返回数据类型  
    def func1: PartialFunction[String, Int] = {  
      case "a" => 97  
      case _ => 0  
    }  
  
    // 偏函数  
    def func3: PartialFunction[Any, Any] = {  
      case i: Int => i * 10  
      case i:String => i  
      case _ => "hahaha"  
    }  
  
    println(func("a"))  
    println(func1("a"))  
    println(func1("b"))  
  
    println(func3("hello"))  
    println(func3(30))  
    println(func3(32.1))  
  }  
  
}
```
### 3.16 数组的定义

==长度不可变，内容可改==

```scala
object ArrayOpt {  
  def main(args: Array[String]): Unit = {  
  
    var arr = new Array[String](3)  
    arr(0) = "hello" // 修改内容  
    println(arr.toBuffer) // ArrayBuffer(null, null, null)  
  
    var arr1 = Array("hello", "scala") // ArrayBuffer(hello, null, null)  
    arr1(0) = "Python"  
    println(arr1.toBuffer) // ArrayBuffer(Python, scala)
  }  
  
}
```
### 3.17 map|flatten|flatMap|foreach 方法的使用

```scala
object ArrayOpt {

    println("=====================单词统计========================")  
    val lines: Array[String] = Array("hello hello tom", "hello jerry")  
  
    // 1.每个元素按空格切分 Array(Array(hello, hello, tom), Array(hello, jerry))    val wordsArr: Array[Array[String]] = lines.map((x: String) => x.split(" "))  
    // 可以简化为  
    //    lines.map(x => x.split(" "))  
//    lines.map(_.split(" "))  
  
    // 2. 把每个元素扁平化 Array[String] = Array(hello, hello, tom, hello, jerry)    val words: Array[String] = wordsArr.flatten  
  
    // 1和2可以写成  
    //    val words: Array[String] = lines.flatMap(_.split(" "))  
  
    // 3.把相同的元素聚合在一起 scala.collection.immutable.Map[String,Array[String]] =    // Map(hello -> Array(hello, hello, hello), tom -> Array(tom), jerry -> Array(jerry))    val groupBy: Map[String, Array[String]] = words.groupBy(x => x)  
  
    // 4.统计map的value值的长度 scala.collection.immutable.Map[String,Int] = Map(hello -> 3, tom -> 1, jerry -> 1)    val wordCount: Map[String, Int] = groupBy.mapValues(_.length)  
  
    // 5.排序。按value值升序  
    val result: List[(String, Int)] = wordCount.toList.sortBy(_._2)  
  
    // 遍历结果  
    result.foreach(println(_))  
  
  }  
  
}
```

![[Pasted image 20231030143444.png|700]]

![[Pasted image 20231030143502.png]]

## 四、Scala集合

代码包路径：com.bigdata.scala.collection

### 4.1 集合的概述

在Scala中，集合分为可变集合（mutable）和不可变集合(immutable)
- 可变集合：长度可变，内容可变
- 不可变集合：长度不可变，内容也不可变

Scala 的集合有三大类：序列 Seq、集 Set、映射 Map，所有的集合都扩展自 Iterable 特质。在 Scala 中集合有可变（mutable）和不可变（immutable）两种类型，immutable 类型的集合初始化后就不能改变了（注意与 val 修饰的变量进行区别）

![[集合.excalidraw|700]]

![[Pasted image 20231030210824.png]]

![[Pasted image 20231030210908.png]]

### 4.2 定长数组和变长数组

代码：com.bigdata.scala.collection.ArrayTest

- 数组：长度不可变，内容可变
- 数组缓存：长度可变，内容可变

````tab
tab: 定长数组
```scala
object ArrayTest {  
  
  def main(args: Array[String]): Unit = {  
    println("===================定长数组==============================")  
    // 1.定义数组  
    // 初始化一个长度为5，其所有的元素均为0  
    //  相当于调用了数组的 apply 方法，直接为数组赋值  
    val arr1 = new Array[Int](5)  
  
    // 将数组转换为数组缓冲，就可以看得到原数组中的内容了  
    println(arr1.toBuffer)  
  
    // 2.定义数组  
    // 初始化一个长度为1，元素为2的数组  
    val arr2 = Array[Int](2)  
    println(arr2.toBuffer)  
  
    // 3.定义数组  
    // 定义一个长度为3的定长数组  
    val arr3 = Array("hadoop","storm","spark")  
    // 使用()来访问元素  
    println(arr3(2))  
  
  }  
  
}
```

tab: 变长数组

```scala
object ArrayTest {  
  
  def main(args: Array[String]): Unit = {  
    println("===================变长数组（数组缓冲）==============================")  
    // 如果想使用数组缓冲，需要导入scala.collection.mutable.ArrayBuffer包  
    import scala.collection.mutable.ArrayBuffer  
    val ab = ArrayBuffer[Int]()  
  
    // 追加元素  
    ab += 1  
    ab += (2,3,4,5)  
    ab ++= Array(6,7)  
    ab ++= ArrayBuffer(8,9)  
    println(ab)  
  
    // 在数组某个位置插入元素  
    // 在0的位置上插入 -1和0  
//    ab.insert(0,-1,0)  
  
    // 删除数组某个位置的元素  
    // 在第三个位置，删除2个元素  
    ab.remove(3,2)  
  
    println(ab)  
  
  }  
  
}
```
````

```txt
===================定长数组==============================
ArrayBuffer(0, 0, 0, 0, 0)
ArrayBuffer(2)
spark
===================变长数组（数组缓冲）==============================
ArrayBuffer(1, 2, 3, 4, 5, 6, 7, 8, 9)
ArrayBuffer(1, 2, 3, 6, 7, 8, 9)

```
### 4.3 Seq序列

- 不可变的序列——List
- 可变的序列——ListBuffer

````tab
tab:不可变的序列

```scala
object ImmutListTest {  
  
  def main(args: Array[String]): Unit = {  
    // 创建一个不可变的集合  
    val list1 = List(1, 2, 3)  
  
    // 在list1的前面插入新的元素，生成一个新的List  
    val list2 = 0 :: list1  
    val list3 = list1.::(0)  
    val list4 = 0 +: list1  
    val list5 = list1.+:(0)  
  
    // 将一个元素添加到list1的后面生成一个新的List  
    val list6 = list1 :+ 3  
  
    val list0 = List(4, 5, 6)  
    // 将两个元素合并成一个新的List  
    val list7 = list1 ++ list0  
    // 将list0插入到list1后面生成一个新的集合  
    val list8 = list1 ++: list0  
    // 将list0插入到list1前面生成一个新的集合  
    val list9 = list1.:::(list0)  
  
    // 创建一个空集合  
    val list10 = Nil  
  
    val list11 = 2 :: 3 :: Nil // List[Int] = List(2, 3)  
  
  }  
  
}
```

tab:可变序列

```scala
object MutListTest {  
  
  def main(args: Array[String]): Unit = {  
    // 导包  
    import scala.collection.mutable.ListBuffer  
  
    // 构建一个可变列表，初始化有三个元素1，2，3  
    val list0 = ListBuffer[Int](1, 2, 3)  
  
    // 创建一个空的可变列表  
    val list1 = new ListBuffer[Int]  
  
    // 向list1中追加元素，注意:没有生成新的集合  
    list1 += 4  
    list1.append(5)  
  
    // 将list0和list1合并成一个新的ListBuffer。注意：生成一个新的集合  
    val list2 = list0 ++ list1  
  
    // 将元素追加到list0的后面生成一个新的集合  
    val list3 = list0 :+ 5  
  
  }  
  
}
```

````
### 4.4 Set集

- 不可变的Set—HashSet
- 可变的Set—HashSet

````tab
tab:不可变的Set

```scala
object ImmutSetTest {  
  def main(args: Array[String]): Unit = {  
    // 导包  
    import scala.collection.immutable.HashSet  
    // 创建一个不可变的HHashSet  
    val set1 = new HashSet[Int]()  
  
    // 将元素和set1合并生成一个新的set  
    val set2 = set1 + 4  
    // set中的元素不能重复  
    val set3 = set1 ++ Set(5,6,7) ++ Set(5,6,1)  
    println(set3)  
  
  }  
}
```
tab:可变的Set

```scala
object MutSetTest {  
  def main(args: Array[String]): Unit = {  
    // 导包  
    import scala.collection.mutable.HashSet  
    // 创建一个可变的HashSet  
    val set1 = new HashSet[Int]()  
    // 向set1中添加元素  
    set1 += 2  
    set1.add(4)  
    set1 ++= Set(1,3,5)  
    println(set1)  
  
    // 删除一个元素  
    set1 -= 5  
    set1.remove(2)  
    println(set1)  
  
  }  
}
```
````


````tab
tab:不可变的Set

```txt
Set(5, 1, 6, 7)
```

tab:可变的Set

```txt
Set(1, 5, 2, 3, 4)
Set(1, 3, 4)
```
````
### 4.5 Map映射

- 不可变的Map—HashMap
- 可变的Map—HashMap

````tab
tab: 不可变的Map

```scala
object ImmutMapTest {  
  def main(args: Array[String]): Unit = {  
    // 导包  
    import scala.collection.immutable.HashMap  
    // 定义一个不可变的HashMap  
    val map1 = HashMap(("hadoop", 2), ("hello", 4))  
  
    // 遍历元素  
    for (elem <- map1.keySet) {  
      println(elem, "->", map1(elem))  
    }  
  
    map1.foreach{case(x,y) => println(x+"->"+y)}  
  
    for (elem <- map1) {println(elem._1,elem._2)}  
  
  }  
  
}
```
tab: 可变的Map

```scala
object MutMapTest {  
  def main(args: Array[String]): Unit = {  
    import scala.collection.mutable.HashMap  
    // 创建一个可变的HashMap  
    val map1 = new HashMap[String,Int]()  
  
    // 向map中添加数据  
    map1("spark") = 1  
    map1 += (("hadoop",2))  
    map1.put("storm",3)  
  
    println(map1) // Map(hadoop -> 2, spark -> 1, storm -> 3)  
  
    // 取值  
    map1.get("spark")  
    map1.getOrElse("python",5)  
  
    // 从map1中移除元素  
    map1 -= "spark"  
    map1.remove("hadoop")  
    println(map1) // Map(storm -> 3)  
  
  }  
  
}
```
````

````tab
tab:不可变的Map

```txt
(hadoop,->,2)
(hello,->,4)
hadoop->2
hello->4
(hadoop,2)
(hello,4)
```

tab:可变的Map

```txt
Map(hadoop -> 2, spark -> 1, storm -> 3)
Map(storm -> 3)
```
````

### 4.6 元组

Scala元组将固定数量的项目组合在一起，以便它们可以作为一个整体传递。与数组或列表不同，元组可以容纳不同类型的对象，但它们也是不可变的。

元组是类型Tuple1，Tuple2，Tuple3等等。目前在Scala中只能有22个上限，如果您需要更多个元素，那么可以使用集合而不是元组。

```scala
object TupleTest {  
  def main(args: Array[String]): Unit = {  
    // 定义元组  
    val t = (1,"hello",true)  
    // 或者  
    val t2 = new Tuple3(2,"hello",true)  
  
    // 访问tuple中的元素  
    println(t._2)  
  
    // 迭代元组  
    t.productIterator.foreach(println)  
  
    // 对偶元组  
    val t3 = (1,3)  
    // 交换元组的元素位置，生成新的元组  
    val swap: (Int, Int) = t3.swap  
  
    println(swap) // (3,1)  
  }  
  
}
```
### 4.7 集合常用的方法（重点）

map, flatten, flatMap, filter, sorted, sortBy, sortWith, grouped,fold(折叠), foldLeft, foldRight, reduce, reduceLeft, aggregate, union,intersect(交集), diff(差集), head, tail, zip, mkString, foreach, length, slice, sum

![[映射map.excalidraw|200]]

![[扁平化flatten.excalidraw]]

````tab
tab:集合计算简单函数

```scala
object ListMethodTest {
  def main(args: Array[String]): Unit = {

    val list = List(1, 2, 3, 4, 5, 6, 7, 8, 9)
    val nestedList = List(List(1, 2, 3), List(4, 5, 6), List(7, 8, 9))
    val wordList: List[String] = List("hello world", "hello scala", "hello scala")
    val list2 = List(2, 3, 4)

    println("--------------集合计算简单函数-----------------")
    println(list.sum) // 求和 45
    println(list.product) // 求乘积1*2*3..*9 362880
    println(list.max) // 求最大值 9
    println(list.min) // 最最小值 1
    println(list.length) // 元素的个数 9
    println(list.head) // 头部元素 1
    println(list.tail) // 尾部元素 List(2, 3, 4, 5, 6, 7, 8, 9)
    println(list.mkString(",")) // 转字符串 1,2,3,4,5,6,7,8,9
    list.foreach(println) // 遍历每个元素
    println(list.slice(0, 3)) // 截取集合的元素,从0位置开始取3个 List(1, 2, 3)
    println(list.sortBy(x => x)) // 按照元素大小升序排序 List(1, 2, 3, 4, 5, 6, 7, 8, 9)
    println(list.sortBy(x => -x)) // 按元素的大小降序排序 List(9, 8, 7, 6, 5, 4, 3, 2, 1)
    println(list.sortWith((x, y) => x < y)) // 按照元素大小升序排序
    println(list.sortWith(_ > _)) // 按元素的大小降序排序
    println(list.sorted) // 按照元素大小升序排序
    
  }

}
```

tab:映射过滤扁平化

```scala
object ListMethodTest {
  def main(args: Array[String]): Unit = {

    val list = List(1, 2, 3, 4, 5, 6, 7, 8, 9)
    val nestedList = List(List(1, 2, 3), List(4, 5, 6), List(7, 8, 9))
    val wordList: List[String] = List("hello world", "hello scala", "hello scala")
    val list2 = List(2, 3, 4)

    println("--------------映射:map-----------------")
    // 映射:map => 将集合中的每一个元素映射到某一个函数
    println(list.map(_ * 10)) // 每个元素×10 List(10, 20, 30, 40, 50, 60, 70, 80, 90)

    println("--------------过滤：filter-----------------")
    println(list.filter(_ % 2 == 0)) // 取偶数 List(2, 4, 6, 8)

    println("--------------扁平化:flatten-----------------")
    println(nestedList.flatten) // List(1, 2, 3, 4, 5, 6, 7, 8, 9)

    println("--------------扁平化+映射:flatMap => map + flatten-----------------")
    println(wordList.flatMap(_.split(" "))) // List(hello, world, hello, scala, hello, scala)
    println(wordList.map(_.split(" ")).flatten)
  }

}
```

tab:分组

```scala
object ListMethodTest {
  def main(args: Array[String]): Unit = {

    val list = List(1, 2, 3, 4, 5, 6, 7, 8, 9)
    val nestedList = List(List(1, 2, 3), List(4, 5, 6), List(7, 8, 9))
    val wordList: List[String] = List("hello world", "hello scala", "hello scala")
    val list2 = List(2, 3, 4)

    println("--------------分组:group-----------------")
    println(list.groupBy(_ % 2)) // 按奇偶分组 Map(1 -> List(1, 3, 5, 7, 9), 0 -> List(2, 4, 6, 8))
    for (elem <- list.grouped(3)) println(elem) //分三组 List(1, 2, 3) List(4, 5, 6) List(7, 8, 9)

  }

}
```

tab:简化（归约）

```scala
object ListMethodTest {
  def main(args: Array[String]): Unit = {

    val list = List(1, 2, 3, 4, 5, 6, 7, 8, 9)
    val nestedList = List(List(1, 2, 3), List(4, 5, 6), List(7, 8, 9))
    val wordList: List[String] = List("hello world", "hello scala", "hello scala")
    val list2 = List(2, 3, 4)

    println("--------------简化（归约）:Reduce-----------------")
    // 简化（归约）:Reduce => 通过指定的逻辑将集合中的数据进行聚合，从而减少数据，最终获取结果
    println(list2.reduce(_ + _)) // 底层是reduceLeft 集合所有的元素相加 2+3+4=9
    println(list2.reduceLeft(_ - _)) // 2-3-4=-5
    println(list2.reduceRight(_ - _)) // (2-(3-4)) = 3 // 先反转集合 [4,3,2]

    println("--------------折叠:Fold -----------------")
    // fold 方法使用了函数柯里化，存在两个参数列表
    // 第一个参数列表为 ： 零值（初始值）
    // 第二个参数列表为： 简化规则
    // 比reduce多了个初始值
    println(list2.fold(1)(_ + _)) // 底层是foldLeft 1+2+3+4 = 10
    println(list2.foldLeft(1)(_ - _)) // 1-2-3-4=-8
    println(list2.foldRight(1)(_ - _)) // 2-(3-(4-1)) = 2 // 先反转集合 [1,4,3,2]

    println("--------------aggregate -----------------")
    val result= list2.aggregate(1)(_ + _, _ + _) // seqOp相当于局部聚合，combOp：全局聚合
    println(result) // 10
  }

}
```

tab:两个集合常用操作

```scala
object ListMethodTest {
  def main(args: Array[String]): Unit = {

    val list = List(1, 2, 3, 4, 5, 6, 7, 8, 9)
    val nestedList = List(List(1, 2, 3), List(4, 5, 6), List(7, 8, 9))
    val wordList: List[String] = List("hello world", "hello scala", "hello scala")
    val list2 = List(2, 3, 4)

    println("--------------两个集合常用操作 -----------------")

    println(list union list2) // 两个集合的合并 List(1, 2, 3, 4, 5, 6, 7, 8, 9, 2, 3, 4)
    println(list intersect list2) // 两个集合的交集 List(2, 3, 4)
    println(list diff list2) // list 与 list2的差集 List(1, 5, 6, 7, 8, 9)
    println(list.zip(list2)) // 拉链 List((1,2), (2,3), (3,4))
  }

}
```
````

### 4.8 并行化集合par

为了充分使用多核CPU，提供了并行集合。调用集合的 par 方法, 会将集合转换成并行化集合。

```scala
object ParTest {  
  def main(args: Array[String]): Unit = {  
    // 不开启并行化操作  
    val result1 = (0 to 100).map(  
      case_ => Thread.currentThread().getName  
    ).distinct  
  
    // 开启并行化操作  
    val result2 = (0 to 100).par.map(  
      case_ => Thread.currentThread().getName  
    ).distinct  
  
    println(result1) // Vector(main)  
    println(result2) // ParVector(scala-execution-context-global-20, scala-execution-context-global-30, 。。。)  
  
  }  
}
```
### 4.9 Map和Option

在 Scala 中 Option 类型样例类用来表示可能存在或也可能不存在的值(==Option 的子类有 Some和 None==)。Some 包装了某个值，None 表示没有值。

```scala
object OptionTest {  
  def main(args: Array[String]): Unit = {  
    // 创建map集合  
    val mp = Map("a" -> 1, "b" -> 2, "c" -> 3)  
  
    // Map 的 get 方法返回的为 Option, 也就意味着 rv 可能取到也有可能没取到  
    val value: Option[Int] = mp.get("b")  
  
    // 如果 value=None 时，会出现异常情况  
    println(value.get)  
  
  }
```
### 4.10 案例wordCount

```scala
val words = Array("hello tom hello star hello sheep", "hello tao hello tom")  
  
// 方法一  
val result = words.flatMap(_.split(" ")) // 按空格切分，然后压平  
  .groupBy(x => x) // 分组  
  .mapValues(_.length) // 取value值算长度  
  .toList // 转集合  
  
println(result) // List((tao,1), (sheep,1), (star,1), (tom,2), (hello,5))  
  
// 方法二  
val result2 = words.flatMap(_.split(" "))  
  .groupBy(x => x)  
  .map(x => (x._1,x._2.length))  
  .toList  
  
println(result2) // List((tao,1), (sheep,1), (star,1), (tom,2), (hello,5))
```
## 五、Scala面向对象

### 5.1 面对对象的概述

### 5.2 单例对象

在 Scala 中，是没有 static 这个东西的，但是它也为我们提供了单例模式的实现方法，那就是使用关键字 object。
- 在scala中的object 是一个单例对象，没办法new
- object中定义的==成员变量 和 方法都是静态的==
- 可以通过对象名.方法 或者.成员变量

````tab
tab:ScalaStaticTest

```scala
object ScalaStaticTest {  
  
  def main(args: Array[String]): Unit = {  
    // 查看成员变量  
    println(ScalaStatic.name)  
    ScalaStatic.age = 20  
    println(ScalaStatic.age)  
  
    // 查看成员方法  
    ScalaStatic.saySomething("今天有点冷")  
    ScalaStatic.apply("西红柿炒番茄")  
  
    // 默认调用就是apply方法  
    ScalaStatic("油焖大虾") // 语法糖（suger）  
  
  }  
  
}
```

tab:ScalaStatic

```scala
object ScalaStatic {  
  
  // 成员变量  
  val name: String = "张三"  
  var age: Int = 18  
  
  // 成员方法  
  def saySomething(msg: String) = {  
    println(msg)  
  }  
  
  // ScalaStatic("油焖大虾") 默认就是调用apply方法  
  // 语法糖（sugar）  
  def apply(food: String) = {  
    println(s"米饭一碗 $food")  
  }  
}
```
````

### 5.3 类

```txt
在Scala中定义类用class关键词修饰
这个类默认会有一个空参构造器

定义在类名称后面的构造器叫主构造器，一个类可以有多个辅助构造器（在辅助构造器中必须先调用类的主构造器）
类的主构造器中的属性会定义成类的成员变量

如果主构造器中成员属性没有val | var 修饰的话，该属性不能被访问，相当于对外没有提供get方法
如果成员属性使用var修饰的话，相当于对外提供了get和set方法
如果成员属性使用了val修饰的话，相当于对外提供了get方法

private修饰：

1.类的构造器访问权限
private在主构造器之前，这说明该类的主构造器是私有的，外部类或者外部对象不能访问（只能在这个类的内部以及其伴生类对象中可以访问修改）
也适用于辅助构造器

2.类的成员属性访问权限
如果类成员属性是private修饰的，它的set和get方法都是私有的，外部不能访问（只能在这个类的内部以及其伴生类对象中可以访问修改）

3.类的访问权限：
类的前面加上private:外部类或者外部对象不能访问（只能在这个类的内部以及其伴生类对象中可以访问修改）
类的前面加上private[this]:标识这个类在当前包下都可见，当前包下的子包不可见
类的前面加上private[包名]：标识这个类在当前包及其子包下都可见
```
#### 5.3.1 定义类

````tab
tab:TeacherTest

```scala
object TeacherTest {  
  def main(args: Array[String]): Unit = {  
    // 创建对象，也可以写成 new Teacher()    
    val teacher = new Teacher  
    teacher.name = "张三"  
  
    teacher.sayHello()  
  }  
  
}
```

tab:Teacher

```scala
/**  
 * Desc: 定义类  
 * 如果成员属性使用var修饰的话，相当于对外提供了get和set方法  
 * 如果成员属性使用了val修饰的话，相当于对外提供了get方法  
 */  
class Teacher {  
  
  // _ 表示一个占位符, 编译器会根据你变量的具体类型赋予相应初始值  
  // 注意: 使用_ 占位符是, 变量类型必须指定  
  var name: String = _  
  val age: Int = 18 // 不可修改  
  
  def sayHello() = {  
    println(s"hello,$name")  
  }  
}
```
````
#### 5.3.2 主构造器/辅助构造器

````tab
tab:Teacher1Test

```scala
object Teacher1Test {  
  def main(args: Array[String]): Unit = {  
    // 创建对象  
    val teacher = new Teacher1("张三",20)  
  
    println(teacher.name)  
  
    // 创建对象  
    val teacher2 = new Teacher1("lisi",30,"男","广东省")  
  
    println(teacher2.prov)  
  
  }  
}
```

tab:Teacher1

```scala
/**  
 * Desc: 主构造器和辅助构造器  
 * 定义在类名称后面的构造器叫主构造器，一个类可以有多个辅助构造器（在辅助构造器中必须先调用类的主构造器）  
 * 类的主构造器中的属性会定义成类的成员变量  
 * 如果主构造器中成员属性没有val | var 修饰的话，该属性不能被访问，相当于对外没有提供get方法  
 */  
class Teacher1(var name: String, age: Int) {  
  var sex: String = _  
  var prov: String = _  
  
  // 定义辅助构造器  
  def this(name: String, age: Int, sex: String) = {  
    this(name, age) // 在辅助构造器中必须先掉主构造器  
    this.sex = sex  
  }  
  
  def this(name: String, age: Int, sex: String, prov: String) = {  
    this(name, age, sex) // 在上面一个辅助构造器中调用了主构造器  
    this.prov = prov  
  }  
}
```
````
#### 5.3.3 访问权限

==（1）主构造器的访问权限：==

````tab
tab:Test
```scala
object Test {  
    def main(args: Array[String]): Unit = {  
  
    val teacher = new Teacher2("张三", 18,"男")  
  
    println(teacher.name)  
  }  
}
```
tab:Teacher2

```scala
/**  
 * Desc:构造器的访问权限  
 * private 在主构造器之前，这说明该类的主构造器是私有的，外部类或者外部对象不能访问(只能在这个类的内部以及其  
 * 伴生类对象中可以访问修改)  
 * 也适用于辅助构造器  
 */  
class Teacher2 private(val name: String, var age: Int) {  
  var gender: String = _  
  
  // 辅助构造器，使用def this  
  // 在辅助构造器中必须先调用类的主构造器  
  def this(name: String, age: Int, gender: String) {  
    this(name, age)  
    this.gender = gender  
  }  
}  
  
object Teacher2 {  
  def main(args: Array[String]): Unit = {  
    val teacher = new Teacher2("张三", 18)  
    println(teacher.name)  
  }  
}
```
````

==（2）类的成员属性访问权限==

````tab
tab:Test

```scala
object Test {  
  def main(args: Array[String]): Unit = {  
    val teacher = new Teacher3("李四",20)  
//    println(teacher.name) // 报错，不能访问  
//    teacher.gender = "男"// 报错，不能访问  
//    println(teacher.gender)// 报错，不能访问  
  }  
}
```

tab:Teacher3

```scala
/**  
 * Desc:类的成员属性访问权限  
 * 如果类成员属性是private修饰的，它的set和get方法都是私有的，外部不能访问（只能在这个类的内部以及其伴生类对象中可以访问修改）  
 */  
class Teacher3(private val name: String, var age: Int) {  
  private var gender: String = _  
}  
  
object Teacher3{  
  def main(args: Array[String]): Unit = {  
    val teacher = new Teacher3("李四",20)  
    println(teacher.name)  
    teacher.gender = "男"  
    println(teacher.gender)  
  }  
}
```
````

==（3）类的访问权限==

````tab
tab:Test

```scala
package com.bigdata.scala  
  
import com.bigdata.scala.objectOriented.Teacher4

object Test {  
  def main(args: Array[String]): Unit = {  
    val teacher = new Teacher4("李四", 20)  
  }  
}
```

tab:Teacher4

```scala
package com.bigdata.scala.objectOriented
/**  
 * Desc:类的访问权限  
 * 类的前面加上private:外部类或者外部对象不能访问（只能在这个类的内部以及其伴生类对象中可以访问修改）  
 * 类的前面加上private[this]:标识这个类在当前包下都可见，当前包下的子包不可见  
 * 类的前面加上private[包名]：标识这个类在当前包及其子包下都可见  
 */  
private[scala] class Teacher4(val name: String, var age: Int) {  
  var gender: String = _  
}
```
````
#### 5.3.4 伴生类/apply 方法

在 Scala 中, ==当单例对象与某个类共享同一个名称时，他被称作是这个类的伴生对象==。**必须在同一个源文件里定义类和它的伴生对象**。类被称为是这个单例对象的伴生类。类和它的伴生对象可以互相访问其私有成员

```scala
/**  
 * Desc:伴生类/apply 方法  
 */  
class Teacher5(var name: String, var age: Int) {  
}  
  
// 在伴生对象中可以访问类的私有成员方法和属性  
object Teacher5 {  
  def apply(): Teacher5 = {  
    // 初始化工作  
    new Teacher5("张三", 20)  
  }  
    
  def main(args: Array[String]): Unit = {  
    // object 类() 默认调用的 apply()方法  
    // 不加()时不会调用  
    // 等同于Teacher5.apply()  
    val tea = Teacher5() //语法糖(sugar)  
    println(tea.name)  
  }  
  
}
```

### 5.4 特质

==使用的关键字是 trait ==

Scala Trait(特质) 相当于 Java 的接口，实际上它比接口还功能强大。与接口不同的是，==它还可以定义属性和方法的实现==。
一般情况下 Scala 的类只能够继承单一父类，但是如果是 ==Trait(特质) 的话就可以继承多个==，实现了多重继承。

````tab
tab:ScalaTraitImpl-继承

```scala
/**  
 * Desc:Trait(特质) 的话就可以继承多个，with 实现了多重继承  
 */  
//编译器在编译会从右往左进行编译检查  
object ScalaTraitImpl extends ScalaTrait with FlyTrait with StudentTrait {  
  
  override var name: String = "张三"  
  
  // 定义T的类型  
  override type T = Int  
  
  // 如果特质中这个方法没有实现，可以不加override  
  override def hello(name: String): Unit = {  
    println(s"hello,$name")  
  }  
  // 重写某个方法，快捷键ctrl+o  
  override def small(name: String): Unit = {  
    println(s"丁丁 对 $name 哈哈大笑")  
  }  
  
  def main(args: Array[String]): Unit = {  
    ScalaTraitImpl.hello("lisi")  
    ScalaTraitImpl.fly("小猪")  
    ScalaTraitImpl.learn(123)  
    println(ScalaTraitImpl.animalName)  
    println(ScalaTraitImpl.name)  
  }  
}
```
tab:Person-继承

```scala
/**  
 * Desc:动态的混入N个特质  
 */  
object Person {  
  def main(args: Array[String]): Unit = {  
    // 在创建对象的时候，动态混入N个特质  
    val stu = new Student() with FlyTrait with ScalaTrait {  
      override var name: String = "张三"  
  
      override def hello(name: String): Unit = {  
        println(s"hello,${name}")  
      }  
    }  
  
    stu.hello(stu.name)  
  }  
  
}  
class Student {}
```
tab:ScalaTrait-特质

```scala
/**  
 * Desc:特质（interface）  
 * 在Scala中特质中可以定义有实现的方法，也可以定义没有实现的方法  
 */  
trait ScalaTrait {  
  
  // 成员变量  
  var name: String  
  var age: Int = 18  
  
  // 没有实现的方法  
  def hello(name: String)  
  
  // 实现的方法  
  def small(name: String) = {  
    println(s"老赵对 $name 妩媚一笑")  
  }  
}
```
tab:FlyTrait-特质

```scala
/**  
 * Desc:特质（interface）  
 * 在Scala中特质中可以定义有实现的方法，也可以定义没有实现的方法  
 *  
 * 被 final 修饰的类不能被继承；  
 * 被 final 修饰的属性不能重写；  
 * 被 final 修饰的方法不能被重写  
 */  
trait FlyTrait {  
  final val animalName: String = "bird"  
  
  final def fly(name: String) = {  
    println(s"看，${name}在飞")  
  }  
  
}
```
tab:StudentTrait-特质

```scala
/**  
 * Desc:特质（interface）  
 * 在Scala中特质中可以定义有实现的方法，也可以定义没有实现的方法  
 *  
 * 可以通过 type 关键字来声明类型。  
 * // 把 String 类型用 S 代替  
 * type S = String  
 * val name: S = "小星星"  
 * println(name) 
 */
 trait StudentTrait {  
  
  type T  
  
  def learn(s: T) = {  
    println(s)  
  }  
  
}
```
````

### 5.5 抽象类

==使用的关键字是 abstract ==

在 Scala 中, 使用 abstract 修饰的类称为抽象类. 在抽象类中可以定义属性、未实现的方法和具体实现的方法。

````tab
tab:AbsClassImpl

```scala
/**  
 * Desc:在scala中第一个继承抽象类或者特质，只能使用关键字extends  
 * 如果想继承多个特质的话，可以在extends之后使用with关键字  
 */  
object AbsClassImpl extends AbsClass with FlyTrait {  
  override def eat(food: String): String = {  
    s"$food 炒着吃"  
  }  
  
  def main(args: Array[String]): Unit = {  
    AbsClassImpl.swimming("漂着")  
  }  
}
```

tab:AbsClass

```scala
/**  
 * Desc:使用关键字abstract 定义一个抽象类  
 * 可以定义属性、未实现的方法和具体实现的方法  
 */  
abstract class AbsClass {  
  
  def eat(food: String): String  
  
  def swimming(style: String) = {  
    println(s"$style 这么游")  
  }  
  
}
```
````

### 5.6 继承

继承是面向对象的概念，用于代码的可重用性。 被扩展的类称为超类或父类, 扩展的类称为派生类或子类。Scala 可以通过使用 extends 关键字来实现继承其他类或者特质。

- with 后面只能是特质
- 父类已经实现了的功能, 子类必须使用 override 关键字重写
- 父类没有实现的方法，子类必须实现

### 5.7 样例类/样例对象

==使用的关键字是 case ==

其重要的特征就是支持模式匹配，样例类默认是实现了序列化接口的

```scala
// 样例类： case class 类名(属性....)  
// 支持模式匹配，默认实现了Serializable接口  
// 类名的定义必须是驼峰式，属性名称第一个字母小写  
case class Message(sender: String, messageContent: String)  
  
// 样例对象：case object 对象名  
// 支持模式匹配，默认实现了Serializable接口  
// 样例对象不能封装数据  
case object CheckHeartBeat  
  
object TestCaseClass{  
  def main(args: Array[String]): Unit = {  
    // 可以使用 new 关键字创建实例, 也可以不使用  
    val message = new Message("刘亦菲","今天晚上不吃饭")  
    val message2 = Message("刘亦菲","今天晚上吃饭")  
  }  
}
```
### 5.8 模式匹配match case

````tab
tab:内容

```scala
/**  
 * Desc:模式匹配 match case  
 * 一旦一个case 匹配上了，就不会再往下匹配了  
 */  
object ScalaMatchCase {  
  def main(args: Array[String]): Unit = {  
  
    println("---------------------匹配字符串内容-------------------------")  
  
    def contentMatch(str: String) = str match {  
      case "hello" => println("hello")  
      case "Dog" => println("DOg")  
      case "1" => println("1")  
      case "1" => println("2")  
      case _ => println("匹配不上") // _ 任意内容  
    }  
  
    contentMatch("hello")  
    contentMatch("Dog")  
    contentMatch("1")  
    contentMatch("you")  
  
  }  
}  
```
tab:数据类型
```scala
/**  
 * Desc:模式匹配 match case  
 * 一旦一个case 匹配上了，就不会再往下匹配了  
 */  
object ScalaMatchCase {  
  def main(args: Array[String]): Unit = {  
	println("---------------------匹配数据类型-------------------------")  
	  
	def typeMatch(x: Any) = x match {  
	  case x: Int => println(s"Int $x")  
	  case y: Long => println(s"Long $y")  
	  case b: Boolean => println(s"boolean $b")  
	  case _ => println("匹配不上")  
	}  
	  
	typeMatch(1)  
	typeMatch(10L)  
	typeMatch(true)  
	typeMatch("scala") 
  
  }  
}  
```
tab:数组

```scala
/**  
 * Desc:模式匹配 match case  
 * 一旦一个case 匹配上了，就不会再往下匹配了  
 */  
object ScalaMatchCase {  
  def main(args: Array[String]): Unit = {  
	println("---------------------匹配Array-------------------------")  
	  
	def arrayMatch(arr: Any) = arr match {  
	  case Array(0) => println("只有一个0元素的数组")  
	  case Array(0, _) => println("以0开头的，拥有两个元素的数组")  
	  case Array(1, _, 3) => println("以1开头，3结尾，中间为任意元素的三个元素的数组")  
	  case Array(8, _*) => println("以8开头，N个元素的数组") // _* 标识0个或者多个任意类型的数据  
	}  
	  
	arrayMatch(Array(0))  
	arrayMatch(Array(0, "1"))  
	arrayMatch(Array(1, true, 3))  
	arrayMatch(Array(8))  
	arrayMatch(Array(8, 9, 10, 100))
  
  }  
}  
```
tab:列表

```scala
/**  
 * Desc:模式匹配 match case  
 * 一旦一个case 匹配上了，就不会再往下匹配了  
 */  
object ScalaMatchCase {  
  def main(args: Array[String]): Unit = {  
	println("---------------------匹配List-------------------------")
	def listMatch(list: Any) = list match {  
	  case 0 :: Nil => println("只有一个0元素的List")  
	  case 7 :: 9 :: Nil => println("只有7和9元素的List")  
	  case x :: y :: z :: Nil => println("只有三个元素的List")  
	  case m :: n if n.length > 0 => println("---------") // 拥有head和tail的数组  
	  case _ => println("匹配不上")  
	}  
	  
	listMatch(List(0))  
	listMatch(List(7, 9))  
	listMatch(List(8, 9, 6666))  
	listMatch(List(8, 9, 6666, 333, 555))  
	listMatch(List(666))
  
  }  
}  
```
tab:元组

```scala
/**  
 * Desc:模式匹配 match case  
 * 一旦一个case 匹配上了，就不会再往下匹配了  
 */  
object ScalaMatchCase {  
  def main(args: Array[String]): Unit = {  
	println("---------------------匹配元组-------------------------")  
	  
	def tupleMatch(tuple: Any) = tuple match {  
	  case (0, _) => println("元组的第一个元素为0，第二个元素为任意类型的数据，且只有两个元素")  
	  case (x, m, k) => println("拥有三个元素的元组")  
	  case (_, "AK47") => println("AK47")  
	}  
	  
	tupleMatch((0, 1))  
	tupleMatch(("2", 1, true))  
	tupleMatch((ScalaMatchCase, "AK47"))
  
  }  
}  
```
tab:对象

```scala
/**  
 * Desc:模式匹配 match case  
 * 一旦一个case 匹配上了，就不会再往下匹配了  
 */  
object ScalaMatchCase {  
  def main(args: Array[String]): Unit = {  
	println("---------------------匹配对象-------------------------")  
	  
	def objMatch(obj: Any) = obj match {  
	  case SendHeartBeat(x, y) => println(s"$x $y")  
	  case CheckTimeOutWorker => println("checkTimeOutWorker")  
	  case "registerWorker" => println("registerWorker")  
	}  
	  
	objMatch(SendHeartBeat("appuid11111", System.currentTimeMillis()))  
	objMatch(SendHeartBeat("xx", 100L))  
	objMatch(CheckTimeOutWorker)  
	objMatch("registerWorker")
  
  }  
}  

case class SendHeartBeat(id: String, time: Long)  
  
case object CheckTimeOutWorker
```

````

## 六、Scala的Akka Actor

### 6.1 Akka 中 Actor 模型概述

写并发程序很难。程序员不得不处理线程、锁和竞态条件等等，这个过程很容易出错，而且会导致程序代码难以阅读、测试和维护。
Akka 是 JVM 平台上构建==高并发、分布式和容错应用的工具包==。Akka 用 Scala 语言写成，同时提供了 Scala 和 JAVA 的开发接口。


Akka 处理并发的方法基于 Actor 模型。在基于 Actor 的系统里，所有的事物都是 Actor，就好像在面向对象设计里面所有的事物都是对象一样。但是有一个重要区别，那就是 ==Actor 模型是作为一个并发模型设计和架构的==，而面向对象模式则不是。==Actor 与 Actor 之间只能通过消息通信==。

- 对并发模型进行了更高的抽象
- 异步、非阻塞、高性能的事件驱动编程模型
- 轻量级事件处理（1GB 内存可容纳百万级别个 Actor）

**为什么 Actor 模型是一种处理并发问题的解决方案？**

==处理并发问题就是如何保证共享数据的一致性和正确性==，为什么会有保持共享数据正确性这个问题呢？无非是我们的程序是多线程的，多个线程对同一个数据进行修改，若不加同步条件，势必会造成数据污染。那么我们是不是可以转换一下思维，用单线程去处理相应的请求，但是又有人会问了，若是用单线程处理，那系统的性能又如何保证。==Actor 模型的出现解决了这个问题，简化并发编程，提升程序性能==

![[Pasted image 20231101163028.png|375]]

从图中可以看到，Actor 与 Actor 之前只能用消息进行通信，当某一个 Actor 给另外一个 Actor发消息，消息是有顺序的，只需要将消息投寄的相应的邮箱，至于对方 Actor 怎么处理你的消息你并不知道，当然你也可等待它的回复。

Actor 是 ActorSystem 创建的，ActorSystem 的职责是负责创建并管理其创建的 Actor，ActorSystem 的单例的，一个 JVM 进程中有一个即可，而 Acotr 是多例的。

pom.xml

```xml
<!-- 添加scala的依赖 -->  
<dependency>  
    <groupId>org.scala-lang</groupId>  
    <artifactId>scala-library</artifactId>  
    <version>2.12.18</version>  
</dependency>

<!-- 添加akka的actor依赖 -->  
<dependency>  
    <groupId>com.typesafe.akka</groupId>  
    <artifactId>akka-actor_2.12</artifactId>  
    <version>2.8.5</version>  
</dependency>  
  
<!-- 多进程之间的Actor通信 -->  
<dependency>  
    <groupId>com.typesafe.akka</groupId>  
    <artifactId>akka-remote_2.12</artifactId>  
    <version>2.8.5</version>  
</dependency>
```
### 6.2 Akka案例——HelloActor

![[actor通信模型.excalidraw|600]]

```scala
import akka.actor.{Actor, ActorSystem, Props}  
  
/**  
 * Desc: 向自己发送信息  
 * 两个重点：  
 * 获取actorRef(相当于代理对象)，发送信息 actorRef ！ 信息  
 * 接受信息receive  
 * */// 继承actor  
class HelloActor extends Actor{  
  
  // 接收消息的  
  override def receive: Receive = {  
    // 接收消息并处理  
    case "你好帅" => println("净说实话，我喜欢你这种人！")  
    case "丑" => println("滚犊子")  
    case "stop" => {  
      context.stop(self) // 停止自己的actorRef  
      context.system.terminate() // 关闭ActorSystem  
    }  
  }  
}  
object HelloActor {  
  
  private val nbFactory = ActorSystem("NBFactory") //工厂  
  private val helloActorRef = nbFactory.actorOf(Props[HelloActor],"helloActor") // 获取actorRef  
  
  def main(args: Array[String]): Unit = {  
    // 给自己发送信息  
    helloActorRef ! "你好帅"  
    helloActorRef ! "丑"  
    helloActorRef ! "stop"  
  }  
}
```
### 6.3 Akka案例——PingPong

````tab
tab:PingPong

```scala
import akka.actor.{ActorSystem, Props}  
  
/**  
 * Desc:两个人打乒乓球  
 */  
object PingPong {  
  
  def main(args: Array[String]): Unit = {  
    val nbFactory = ActorSystem("NBFactory") //工厂  
  
    // 通过actorSystem创建ActorRef  
    val fGActorRef = nbFactory.actorOf(Props[FengGeActor], "fengGe") // 获取actorRef  
    val lGActorRef = nbFactory.actorOf(Props(new LongGeActor(fGActorRef)), "longGe") // 获取actorRef  
  
    // 发送信息  
    fGActorRef ! "start"  
    lGActorRef ! "start"  
  
  }  
}
```
tab:LongGeActor

```scala
import akka.actor.{Actor, ActorRef}  
  
/**  
 * Desc: 两个人打乒乓球-龙哥  
 */  
class LongGeActor(val fg: ActorRef) extends Actor {  
  // 接收消息的  
  override def receive: Receive = {  
    case "start" => {  
      println("龙哥：I'm OK!")  
      fg ! "啪" // 给fg发消息  
    }  
    case "啪啪" => {  
      println("龙哥：你真猛！！！")  
      Thread.sleep(1000) // 休息1s  
      fg ! "啪" // 给fg发消息  
    }  
  }  
}
```
tab:FengGeActor

```scala
import akka.actor.Actor  
  
/**  
 * Desc:两个人打乒乓球-峰哥  
 */  
class FengGeActor extends Actor{  
  override def receive: Receive = {  
    case "start" => println("峰哥：I'm OK!")  
    case "啪" => {  
      println("峰哥：那必须滴！")  
      Thread.sleep(1000)//休息1s  
      sender() ! "啪啪" // 回复给他发送消息的人  
    }  
  }  
}
```
````
### 6.4 Akka案例——聊天模型

![[Actor聊天模型.excalidraw|500]]


````tab
tab:Edu360Server-服务端

```scala
import akka.actor.{Actor, ActorSystem, Props}
import com.typesafe.config.ConfigFactory

/**
 * Desc:阿里智能机器人-Server-Actor
 */
class Edu360Server extends Actor{
  // 用来接收客户端发送过来的问题
  override def receive: Receive = {
    case "start" => println("老娘已就绪！")
    case ClientMessage(msg) => {
      println(s"收到客户端的消息：$msg")
      msg match {
        case "你叫啥？" => sender()! ServerMessage("铁扇公主")
        case "你是男是女？" => sender()!ServerMessage("老娘是女的")
        case "你有男票嘛？" => sender()!ServerMessage("没有")
        case _ => sender()! ServerMessage("What you say?")// sender()发送端的代理对象，发送到客户端的mailBox中->客户端的receive
      }
    }
  }
}
object Edu360Server{
  def main(args: Array[String]): Unit = {
    val host = "127.0.0.1"
    val port = 8088

    val config = ConfigFactory.parseString(
      s"""
         |akka{
         |	actor{
         |		provider="akka.remote.RemoteActorRefProvider"
         |		allow-java-serialization=on
         |		}
         |	remote{
         |		enabled-transports=["akka.remote.netty.tcp"]
         |		artery{
         |			transport=tcp
         |			canonical.hostname=$host
         |			canonical.port=$port
         |			}
         |		}
         |}
         """.stripMargin)

    // 指定IP和端口
    val actorSystem = ActorSystem("Server",config)

    val serverActorRef = actorSystem.actorOf(Props[Edu360Server],"shanshan")

    // 给自己发一条信息
    serverActorRef ! "start" // 到自己的mailBox -> receive方法
  }
}
```
tab:ClientActor-客户端

```scala

import akka.actor.{Actor, ActorSelection, ActorSystem, Props}
import com.typesafe.config.ConfigFactory

import scala.io.StdIn

/**
 * Desc:客户端-Client-Actor-01
 * 启动多个，改下port和Client-Actor-01的值
 */
class ClientActor(host: String, port: Int) extends Actor {
  var serverActorRef: ActorSelection = _ // 服务端的代理对象

  // 在receive方法之前调用
  override def preStart(): Unit = {
    // akka://Server@127.0.0.1:8088
    serverActorRef = context.actorSelection(s"akka://Server@${host}:${port}/user/shanshan")
  }

  // mailBox -> receive
  override def receive: Receive = {
    case "start" => println("牛魔王系列已启动")
    case msg: String => {
      serverActorRef ! ClientMessage(msg) // 把客户端输入的内容发送给 服务端（actorRef）--》服务端的mailbox中 -> 服务端的receive
    }
    case ServerMessage(msg) => println(s"收到服务端消息：$msg")
  }
}

object ClientActor {
  def main(args: Array[String]): Unit = {
    val host = "127.0.0.1"
    val port = 8089

    val config = ConfigFactory.parseString(
      s"""
         |akka{
         |	actor{
         |		provider="akka.remote.RemoteActorRefProvider"
         |		allow-java-serialization=on
         |		}
         |	remote{
         |		enabled-transports=["akka.remote.netty.tcp"]
         |		artery{
         |			transport=tcp
         |			canonical.hostname=$host
         |			canonical.port=$port
         |			}
         |		}
         |}
        """.stripMargin)

    val clientSystem = ActorSystem("client", config)

    // 服务端的地址
    val serverHost = "127.0.0.1"
    val serverPort = 8088
    // 创建dispatch | mailbox
    val actorRef = clientSystem.actorOf(Props(new ClientActor(serverHost, serverPort)), "Client-Actor-02")

    actorRef ! "start" // 自己给自己发送了一条消息 到自己的mailbox => receive

    while (true) {
      val question = StdIn.readLine() // 同步阻塞的， shit
      actorRef ! question // mailbox -> receive
    }
  }
}

```
tab:MessageProtocol-封装消息的类

```scala
/**  
 * Desc: 封装信息。样例类实现了Serializable接口  
 */  
  
// 服务端发送给客户端的消息格式  
case class ServerMessage(msg: String)  
  
// 客户端发送给服务器端的消息格式  
case class ClientMessage(msg: String)
```
````
### 6.5 Akka案例——Spark底层通信示例

![[Spark的Master和Worker通信过程概述.excalidraw|600]]
````tab
````

````tab
tab:SparkMaster

```scala
import akka.actor.{Actor, ActorSystem, Props}
import com.typesafe.config.ConfigFactory

/**
 * Desc:Spark-Master
 *1.Worker会向Master注册自己（内存，核数，...）
 *2.Master收到Worker的注册信息之后，会告诉Worker已注册成功
 *3. Worker收到Master的注册成功消息之后，会定期向master汇报自己的状态，也就向Master报活（心跳）
 *4.Master收到Worker的心跳信息之后，定期的更新Worker的状态，如心跳发送时间
 *5.Master会定期监测发送过来的心跳时间和当前时间的差，如果大于5分钟，则认为Worker已死。然后Master再分任务的时候就不会给该Worker下发任务
 *
 * 打包放入服务器运行：
 * java -cp 包名.jar 类名 参数1 参数2
 * (在pom文件指定了类名。可以采用这个方式)
 * java -jar 包名.jar  参数1 参数2
 */
class SparkMaster extends Actor {

  // 存储worker的信息的
  val id2WorkerInfo = collection.mutable.HashMap[String, WorkerInfo]()

  //    override def preStart(): Unit = {
  //        context.system.scheduler.schedule(0 millis, 6000 millis, self, RemoveTimeOutWorker)
  //    }

  // 接收消息
  override def receive: Receive = {
    // 收到Worker发过来的注册信息
    case RegisterWorkerInfo(workId, ram, core) => {
      // 将worker的信息存储起来，存储到HashMap
      if (!id2WorkerInfo.contains("workId")) {
        val workerInfo = new WorkerInfo(workId, ram, core)
        id2WorkerInfo += ((workId, workerInfo))

        // 2.Master收到Worker的注册信息之后，会告诉Worker已注册成功
        sender() ! RegisteredWorkerInfo
      }
    }

    // 4.Master收到Worker的心跳信息之后，定期的更新Worker的状态，如心跳发送时间
    case HearBeat(workId) => {
      // master收到worker的心跳消息之后，更新woker的上一次心跳时间
      val workerInfo = id2WorkerInfo(workId)
      // 更改心跳时间
      val currentTime = System.currentTimeMillis()
      workerInfo.lastHeartBeatTime = currentTime
    }
    // 5.Master会定期监测发送过来的心跳时间和当前时间的差，如果大于5分钟，则认为Worker已死。然后Master再分任务的时候就不会给该Worker下发任务
    case CheckTimeOutWorker => {
      import context.dispatcher
      import scala.concurrent.duration._ // 导入时间单位
      // master 启动一个定时器，每隔6秒给自己发一条信息
      context.system.scheduler.scheduleAtFixedRate(0 millis, 6000 millis, self, RemoveTimeOutWorker)
    }
    case RemoveTimeOutWorker => {
      // 将hashMap中的所有的value都拿出来，查看当前时间和上一次心跳时间的差 3秒
      val workerInfos = id2WorkerInfo.values
      val currentTime = System.currentTimeMillis()
      //  过滤超时的worker
      workerInfos
        .filter(workerInfo => currentTime - workerInfo.lastHeartBeatTime > 3000)
        .foreach(workerInfo => id2WorkerInfo.remove(workerInfo.workId))

      println(s"-----还剩 ${id2WorkerInfo.size} 存活的Worker-----")
    }
  }
}

object SparkMaster {
  def main(args: Array[String]): Unit = {
    // 检验参数
    if (args.length != 3) {
      println(
        """
          |请输入参数：<host> <port> <masterName>
          """.stripMargin)
      sys.exit() // 退出程序
    }

    val host = args(0) // 如 "127.0.0.1"
    val port = args(1) // 如 8088
    val masterName = args(2) // 如 "Master"

    val config = ConfigFactory.parseString(
      s"""
         |akka{
         |	actor{
         |		provider="akka.remote.RemoteActorRefProvider"
         |		allow-java-serialization=on
         |		}
         |	remote{
         |		enabled-transports=["akka.remote.netty.tcp"]
         |		artery{
         |			transport=tcp
         |			canonical.hostname=$host
         |			canonical.port=$port
         |			}
         |		}
         |}
         """.stripMargin)

    val actorSystem = ActorSystem("sparkMaster", config)

    val masterActorRef = actorSystem.actorOf(Props[SparkMaster], masterName)

    // 自己给自己发送一个消息，去启动一个调度器，定期的检测HashMap中超时的worker
    masterActorRef ! CheckTimeOutWorker
  }
}
```
tab:SparkWorker

```scala

import java.util.UUID

import akka.actor.{Actor, ActorSelection, ActorSystem, Props}
import com.typesafe.config.ConfigFactory

/**
 * Desc:Spark-Worker
 *1.Worker会向Master注册自己（内存，核数，...）
 *2.Master收到Worker的注册信息之后，会告诉Worker已注册成功
 *3. Worker收到Master的注册成功消息之后，会定期向master汇报自己的状态，也就向Master报活（心跳）
 *4.Master收到Worker的心跳信息之后，定期的更新Worker的状态，如心跳发送时间
 *5.Master会定期监测发送过来的心跳时间和当前时间的差，如果大于5分钟，则认为Worker已死。然后Master再分任务的时候就不会给该Worker下发任务
 */
class SparkWorker(masterUrl: String) extends Actor {

  var masterProxy: ActorSelection = _
  val workId = UUID.randomUUID().toString

  // 在执行receive前执行
  override def preStart(): Unit = {
    // 获取Master的actorRef
    masterProxy = context.actorSelection(masterUrl)
  }

  // 接收消息
  override def receive: Receive = {
    case "started" => {
      // 自己已就绪
      // 1.Worker会向Master注册自己（id,内存，核数，...）
      masterProxy ! RegisterWorkerInfo(workId, 32 * 1024, 8)
    }
    // 3. Worker收到Master的注册成功消息之后，会定期向master汇报自己的状态，也就向Master报活（心跳）
    case RegisteredWorkerInfo => {
      import context.dispatcher
      import scala.concurrent.duration._ // 导入时间单位
      // master 启动一个定时器，每隔1.5秒给自己发一条信息
      context.system.scheduler.scheduleAtFixedRate(0 millis, 6000 millis, self, SendHeartBeat)
    }
    case SendHeartBeat => {
      // 开始向master发送心跳了
      println(s"------- $workId 发送心跳-------")
      masterProxy ! HearBeat(workId) // 此时master将会收到心跳信息
    }
  }
}

object SparkWorker {

  def main(args: Array[String]): Unit = {

    // 检验参数
    if(args.length != 4) {
      println(
        """
          |请输入参数：<host> <port> <workName> <masterURL>
                """.stripMargin)
      sys.exit() // 退出程序
    }

    val host = args(0)// 如 "127.0.0.1"
    val port = args(1) // 如 8089
    val workName = args(2) // 如 "Worker"
    val masterURL = args(3) // akka://sparkMaster@127.0.0.1:8088/user/Master

    val config = ConfigFactory.parseString(
      s"""
         |akka{
         |	actor{
         |		provider="akka.remote.RemoteActorRefProvider"
         |		allow-java-serialization=on
         |		}
         |	remote{
         |		enabled-transports=["akka.remote.netty.tcp"]
         |		artery{
         |			transport=tcp
         |			canonical.hostname=$host
         |			canonical.port=$port
         |			}
         |		}
         |}
         """.stripMargin)

    val actorSystem = ActorSystem("sparkWorker", config)

    // 创建自己的actorRef
    val workerActorRef = actorSystem.actorOf(Props(new SparkWorker(masterURL)), workName)

    // 给自己发送一个以启动的消息，标识自己已经就绪了
    workerActorRef ! "started"
  }
}


```
tab:MssgeProtocol

```scala
// worker向master注册自己（id,内存，核数）  
case class RegisterWorkerInfo(workId: String, ram: Int, core: Int)  
  
// worker 给自己发送消息，告诉自己说要开始周期性的向master发送心跳消息  
case object SendHeartBeat  
  
// worker给master发送心跳信息  
case class HearBeat(id: String)  
  
// 存储worker信息的类  
class WorkerInfo(val workId: String, ram: Int, core: Int){  
  // 最后的心跳时间  
  var lastHeartBeatTime: Long = _  
}  
  
// master向worker发送注册成功消息  
case object RegisteredWorkerInfo  
  
//master给自己发送一个检查超时worker的信息,并启动一个调度器，周期新检测删除超时worker  
case object CheckTimeOutWorker  
  
// master发送给自己的消息，删除超时的worker  
case object RemoveTimeOutWorker
```
````
## 七、Scala 高级语法

### 7.1 隐式（implicit）

隐式可分为：
1. 隐式参数
2. 隐式转换类型
3. 隐式类

scala 有很多自带的隐式，如Predef
#### 7.1.1 隐式参数

```scala
/**  
 * Desc:隐式参数  
 * 定义方法时，可以把参数列表标记为 implicit，表示该参数是隐式参数  
 *  
 * 当调用包含隐式参数的方法时，如果当前上下文中有合适的隐式值，则编译器会自动为该  
 * 组参数填充合适的值，且上下文中只能有一个符合预期的隐式值（其实是根据上面内容）。如果没有编译器会抛出  
 * 异常。当然，标记为隐式参数的我们也可以手动为该参数添加默认值。  
 */  
object ImplicitParams {  
  
  implicit val str = "hello,baby"  
  //    implicit val str2 = "明晚十点见" 有两个相同数据类型的变量，say 执行会报错，不知道要用哪一个变量  
  
  //    定义方法时，可以把参数列表标记为 implicit，表示该参数是隐式参数  
  def say(implicit content: String) = println(content)  
  
  def say2(implicit age: Int = 28) = println(age)  
  
  // 方法的参数如果有多个隐式参数的话，只需要使用一个implicit关键字即可  
  // 隐式参数列表必须放在方法的参数列表后面  
  def addPlus(a: Int)(implicit b: Int, c: Int) = a + b + c  
  
  def main(args: Array[String]): Unit = {  
  
    say // hello,baby  
    say2 // 28 优先用输入的值，再次时隐式变量值，最后时默认值  
  
    implicit val num = 3  
    println(addPlus(2)) // 8  
  }  
}
```

#### 7.1.2 隐式转换类型

```scala
/**  
 * Desc:隐式转换类型  
 */  
object ImplicitTpyeConv {  
  
  implicit val fdouble2Int = (double: Double) => {  
    println("---隐式函数---")  
    double.toInt  
  }  
  
  implicit def double2Int(double: Double) = {  
    println("---隐式方法---")  
    double.toInt  
  }  
  
  def main(args: Array[String]): Unit = {  
    println("-------------隐式类型转换---------")  
    // age是一个Int类型，但是赋值的时候却是一个浮点型，此刻编译器会在当前上下文中找一个隐式转换，找一个能把浮点型变成Int的隐式转换  
    // 优先隐式函数，再次是函数方法  
    val age: Int = 20.5  
    println(age)  
  
	}
}
```

```txt
-------------隐式类型转换---------  
---隐式函数---  
20    
```

#### 7.1.3 隐式类

````tab
tab:ImplicitClassTest

```scala
object ImplicitClassTest {  
  
  // 隐式类-只能在静态对象中使用  
  implicit class FileRead(file: File) {  
  
    def read = Source.fromFile(file).mkString  
  
  }  
  def main(args: Array[String]): Unit = {  
    val file = new File("C:\\Alearning\\data\\mr\\wc\\input\\test1.txt")  
  
    println(s"FileContent = ${file.read}")  
  
    // 导包  
    import ImplicitClass._  
    // 可以在单例对象中定义隐式方法或函数，然后导包就可以用了  
    println("总行数", file.count())  
    file.saySomeThing()  
  
  }  
  
}
```

tab:ImplicitClass

```scala
import java.io.{BufferedReader, File, FileReader}  
  
object ImplicitClass {  
  //  定义了一个隐式方法，将file类型变成RichFile  
  implicit def file2RichFile(file: File) = new RichFile(file)  
  
}  
  
  
class RichFile(file: File) {  
  /**  
   * 返回文件的记录行数  
   *  
   * @return  
   */  
  def count(): Int = {  
    val fileReader = new FileReader(file)  
    val bufferedReader = new BufferedReader(fileReader)  
    var sum = 0  
    try {  
      var line = bufferedReader.readLine()  
      while (line != null) {  
        sum += 1  
        line = bufferedReader.readLine()  
      }  
    } catch {  
      case _: Exception => sum  
    } finally {  
      fileReader.close()  
      bufferedReader.close()  
    }  
    sum  
  }  
  
  def saySomeThing() = {  
    println("稍等下，有话要对你说。。。")  
  }  
  
}
```
````

```txt
FileContent = hello world
hello linux
hello python
(总行数,3)
稍等下，有话要对你说。。。
```
### 7.2 泛型

通俗的讲，比如需要定义一个函数，函数的参数可以接受任意类型。我们不可能一一列举所有的参数类型重载函数。

那么程序引入了一个称之为泛型的东西，==这个类型可以代表任意的数据类型==。

例如 List，在创建 List 时，可以传入整形、字符串、浮点数等等任意类型。那是因为 List 在类定义时引用了泛型。

```scala
/**  
 * Desc:泛型  
 * 就是类型约束  
 * 这个类型可以代表任意的数据类型，一般用T表示  
 */  
// 约束content变量的类型为泛型  
class Message[T](content: T)  
  
// 定义一个泛型类衣服  
class Clothes[A, B, C](val clothType: A, val color: B, val size: C)  
  
// 枚举类  
object ClothesEnum extends Enumeration {  
  type ClothesEnum = Value  
  val 上衣, 内衣, 裤子 = Value  
  
}  
  
object ScalaFanXing {  
  
  def main(args: Array[String]): Unit = {  
    val message1: Message[String] = new Message[String]("hello world")  
    val message2: Message[Int] = new Message(18)  
  
    val clth1 = new Clothes[ClothesEnum, String, Int](ClothesEnum.上衣, "black", 150)  
    println(clth1.clothType)  
  
    val clth2 = new Clothes(ClothesEnum.上衣, "black", "M")  
    println(clth2.size)  
  }  
}
```
### 7.3 类型约束

````tab
tab:ScalaUpperLowerBounds

```scala
package com.bigdata.scala.highLevel

/**
 * Desc:上下界
 * [T <: xx] =》T都是xx的子类或者说xx就T的上界（Upper Bounds） 类似 java <T extends xx>
 * [T >: xx] =》T都是xx的父类或者说xx就T的下界（lower bounds）类似 java <T super D>
 */
object ScalaUpperLowerBounds {

  def main(args: Array[String]): Unit = {
    val cmpInt = new CmpLong(8L, 9L)
    println(cmpInt.bigger)

    //    val cmpComm1 = new CmpComm1(8, 9) // 上界的时候会报错
    val cmpComm1 = new CmpComm1(Integer.valueOf(8), Integer.valueOf(10))
    println(cmpComm1.bigger)

    val cmpComm11 = new CmpComm1(new Students1("Tom", 18), new Students1("Jim", 20))
    println(cmpComm11.bigger)

    val cmpComm2 = new CmpComm2(8, 9)
    println(cmpComm2.bigger)

    val cmpComm3 = new CmpComm3(8, 9)
    println(cmpComm3.bigger)

    val cmpComm4 = new CmpComm4(8, 9)
    println(cmpComm4.bigger)


    import ImplicitClass._
    val cmpComm41 = new CmpComm4(new Students2("Tom", 18), new Students2("Jim", 20))
    println(cmpComm41.bigger)


  }

}

class Students2(val name: String, val age: Int) {
  override def toString: String = this.name + "\t" + this.age
}

class Students1(val name: String, val age: Int) extends Ordered[Students1] {
  override def compare(that: Students1): Int = this.age - that.age

  override def toString: String = this.name + "\t" + this.age
}

class CmpInt(a: Int, b: Int) {
  def bigger = if (a > b) a else b
}

class CmpLong(a: Long, b: Long) {
  def bigger = if (a > b) a else b
}

// 如果还要定义 Double 类型的比较，也许还需要比较 2 个类的比较，咋办，还重复的劳动吗？
// 其实我们可以使用泛型, 但是泛型类型必须实现了 Comparable, 相当于约束了泛型的范围


// 不会发生隐式转换，除非用户显示的指定
// T 实现了 Comparable 接口
class CmpComm1[T <: Comparable[T]](o1: T, o2: T) {
  def bigger = if (o1.compareTo(o2) > 0) o1 else o2
}

/**
 * <% 视图界定 view bounds
 * 会发生隐式转换
 */
class CmpComm2[T <% Comparable[T]](o1: T, o2: T) {
  def bigger = if (o1.compareTo(o2) > 0) o1 else o2
}

/**
 * Ordered 继承了 Comparable
 * 跟 CmpComm2 一样的效果
 */
class CmpComm3[T <% Ordered[T]](o1: T, o2: T) {
  def bigger = if (o1 > o2) o1 else o2
}

// Comparator 传递比较器
/**
 * 上下文界定
 * 也会隐式转换
 * CmpComm4 - CmpComm6效果一样
 */
class CmpComm4[T: Ordering](o1: T, o2: T)(implicit cmptor: Ordering[T]) {
  def bigger = if (cmptor.compare(o1, o2) > 0) o1 else o2
}

class CmpComm5[T: Ordering](o1: T, o2: T) {
  def bigger = {
    def inner(implicit cmptor: Ordering[T]) = cmptor.compare(o1, o2)

    if (inner > 0) o1 else o2
  }
}

class CmpComm6[T: Ordering](o1: T, o2: T) {
  def bigger = {
    val cmptor = implicitly[Ordering[T]]
    if (cmptor.compare(o1, o2) > 0) o1 else o2
  }
}
```

tab:ImplicitClass

```scala
import java.io.{BufferedReader, File, FileReader}

object ImplicitClass {
  //  定义了一个隐式方法，将file类型变成RichFile
  implicit def file2RichFile(file: File) = new RichFile(file)

  // 隐式将Student -> Ordered[Student]
  implicit def student2OrderedStu(stu: Students2) = new Ordered[Students2]{
    override def compare(that: Students2): Int = stu.age - that.age
  }

  // 一个隐式对象实例 -> 一个又具体实现的Comparator
  implicit val comparatorStu = new Ordering[Students2] {
    override def compare(x: Students2, y: Students2): Int = x.age - y.age
  }
}
```
````

