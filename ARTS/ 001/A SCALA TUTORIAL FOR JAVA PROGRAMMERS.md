# 1. A First Example

首先我们来看最常见的Hello World程序。

```scala
Object HelloWorld{
    def main(args: Array[String]) {
        println("Hello world!")
    }
}
```

和Java类似，main函数用来从命令行中读取参数，函数中包含一个打印语句println，main函数没有返回值，因此无需定义返回类型。

和Java不同的是，Object声明包含main方法，Object是一个单例类。上述代码声明了一个HelloWorld类以及这个类的实例，这个类的实例在第一次被用到的时候创建。

精明的读者可能发现，main函数并不是一个静态方法，这是因为在Scala中不存在静态方法和静态域。和使用静态成员不同的是，Scala程序员将这些成员放在单例类中。

## 编译程序

与Java类似，使用命令行的方式进行编译：

```
scalac HelloWorld.scala
```

上述操作之后会产生几个文件，其中HelloWorld.class 文件能够直接运行。

```
> scala -classpath . HelloWorld
Hello world!
```

# 2. 与Java进行交互

Scala一大长处就是能够方便的与Java代码进行交互,默认导入java.lang，其他的需要显式指定。

```scala
import java.util.{Date, Locale}
import java.text.DateFormat
import java.text.DateFormat._

object FrenchDate {
  def main(args: Array[String]) {
    val now = new Date
    val df = getDateInstance(LONG, Locale.FRANCE)
    println(df format now)
  }
}

```

Scala中使用_代替Java中的*，用于表示导入包中的所有名称。还有一个不同之处在于，使用{}将同一个包的导入放在一起。

# 3. 所有的都是对象

### 数字是对象

```
1 + 2 * 3 / x

(1).+(((2).*(3))./(x))
```

## 函数是对象

对Java程序员来说，惊讶的是函数竟然也是对象。函数能够作为参数传递给另一个函数，能够被储存在变量中，能够作为返回值从其他函数返回。

假设有以下的场景，我们需要定义一个timer函数，其功能是每秒产生一些动作。

```scala
object Timer {
  def oncePerSecond(callback: () => Unit) {
    while (true) { callback(); Thread sleep 1000 }
  }
  def timeFlies() {
    println("time flies like an arrow...")
  }
  def main(args: Array[String]) {
    oncePerSecond(timeFlies)
  }
}
```

### 匿名函数

```scala
object TimerAnonymous {
  def oncePerSecond(callback: () => Unit) {
    while (true) { callback(); Thread sleep 1000 }
  }
  def main(args: Array[String]) {
    oncePerSecond(() =>
      println("time flies like an arrow..."))
  }
}
```

对于上面的例子，对timeFlies进行匿名化处理。

#4. 类

```scala
class Complex(real: Double, imaginary: Double) {
  def re() = real
  def im() = imaginary
}
```

scala能够进行类型推断，本着简单性原则，通常会删除显示的类型声明。

## 没有参数的方法

上述代码中，对re 和 im的调用需要使用括号。

```scala
  def main(args: Array[String]) {
    val c = new Complex(1.2, 3.4)
    println("imaginary part: " + c.im())
  }
}
```

而如果对上述代码进行重写，则调用的时候无需使用括号。

```scala
class Complex(real: Double, imaginary: Double) {
  def re = real
  def im = imaginary
}
```

## 继承和重写

Scala中，所有的类继承自超级类，没有显示指定超级类的情况下，默认指定scala.AnyRef。强制使用overrride修饰符来定义重写的方法。

```scala
class Complex(real: Double, imaginary: Double) {
  def re = real
  def im = imaginary
  override def toString() =
    "" + re + (if (im < 0) "" else "+") + im + "i"
}
```

#5.Case Classes 和 模式匹配
例如定义一个用于进行数学表达式计算的类。

```scala
abstract class Tree
case class Sum(l: Tree, r: Tree) extends Tree
case class Var(n: String) extends Tree
case class Const(v: Int) extends Tree
```

case class 和 普通的class 有以下不同：

* 新建对象时，new 关键字不是必须的。Const(5)

* getter 函数默认已被定义好

* 默认的equals 和 hashCode函数已经提供

* 默认的toString函数已经提供

  ```scala
  def eval(t: Tree, env: Environment): Int = t match {
      case Sum(l, r) => eval(l, env) + eval(r, env)
      case Var(n) => env(n)
      case Const(v) => v
  }
  ```

    