## Basics
原文地址：http://twitter.github.io/scala_school/basics.html

### 关于
前几周会包括一些基本的语法和概念，接下来会开始多做练习。一些例子是在解析器运行的，一些例子是写在源文件当中。有一个可用的解析器可以容易查找问题。
#### 为什么使用scala
  * 有表现力的
    * 头等函数
    * 闭包
  * 简洁的
    * 类型推断
    * 函数字面量语法
  * 与java的互操作性
    * 可以复用java类库
    * 可以复用java的一些工具
    * 没有性能损失
#### 怎么使用scala
  * 编译成字节码
  * 与其他标准JVM一起工作
    * 或者一些非标准JVM，比如Dalvik
    * Scala编译器是Java编译器的作者写的

#### 思考scala
Scala不是一个更好的Java，应该用一个不一样的想法来学习，会从这些教程中得到更多。

#### 使用解析器
使用包含的`sbt console`命令

    $ sbt console

    [...]

    Welcome to Scala version 2.10.2 (Java HotSpot(TM) Server VM, Java 1.7.0_21).
    Type in expressions to have them evaluated.
    Type :help for more information.

    scala>
### 表达式

    scala> 1 + 1
    res0: Int = 2
res0是一个解析器自动生成的值，用来表示表达式的结果。它是int类型的，包含了值为2的Integer类型。几乎所有的东西在Scala中都是表达式。

### 值

    scala> val two = 1 + 1
    two: Int = 2
定义成val的变量不可以变化。

### 变量
如果需要修改值，可以使用`var`来代替。

    scala> var name = "steve"
    name: String = steve

    scala> name = "marius"
    name: String = marius
### 函数
可以使用def来创建函数

    scala> def addOne(m : Int) : Int = m + 1
    addOne: (m: Int)Int
在Scala，需要为方法参数指定类型签名。解析器返回相同的类型签名：

    scala> val three = addOne(2)
    three: Int = 3
当函数没有参数的时候，可以省略括号

    scala> def three() = 1 + 2
    three: ()Int

    scala> three()
    res0: Int = 3

    scala> three
    res1: Int = 3
### 匿名函数
可以创建匿名函数：

    scala> (x : Int) => x + 1
    res2: Int => Int = <function1>
这个函数将1加到名字为x的Int当中。

    scala> res2(1)
    res3: Int = 2
可以传递匿名函数或者将它们保存为vals

    scala> val addOne = (x : Int) => x + 1
    addOne: Int => Int = <function1>

    scala> addOne(1)
    res4: Int = 2
如果函数是由多个表达式组成，可以使用{}

    def timesTwo(i: Int): Int = {
      println("hello world")
      i * 2
    }
对匿名函数同样也适用

    scala> { i: Int =>
      println("hello world")
      i * 2
    }
    res0: (Int) => Int = <function1>

当传递一个匿名函数作为一个参数时，你会经常看到这种语法。

### 部分应用
可以使用一个下划线来部分应用一个函数，这样会得到另外一个函数。scala的下环线在不同的环境有不同的意思。但是通常可以认为是未命名的通配符。
`{ _ + 2}`中的下划线表示一个未命名的参数，可以像这样来使用：

    scala> def adder(m : Int, n : Int) = m + n
    adder: (m: Int, n: Int)Int

    scala> val add2 = adder(2, _ : Int)
    add2: Int => Int = <function1>

    scala> add2(3)
    res6: Int = 5
可以部分应用参数列表中的任意参数，不只是最后一个。

### 柯里化函数
有时候应用一些参数,而其他参数留给在后面提供是合理的。
这里是一个例子，一个函数将2数字相乘。在调用的时候，可以选择哪个是乘数，在后面调用的时候，可以选择一个被乘数。

    scala> def multiply(m : Int)(n : Int) : Int = m * n
    multiply: (m: Int)(n: Int)Int
你可以直接调用，包含2个参数

    scala> multiply(2)(3)
    res7: Int = 6
你可以提供第一个参数，部分应用第2个参数/

    scala> val timesTwo = multiply(2) _
    timesTwo: Int => Int = <function1>

    scala> timesTwo(3)
    res8: Int = 6

你可以对任意有多个参数的函数柯里化。可以试试之前的`adder`方法

    scala> (adder _).curried
    res9: Int => (Int => Int) = <function1>


    class C {
          def adder(m : Int, n : Int) = m + n
          val c = (adder _)
    }
    会生成以下的java代码：
    public class C {
      private final scala.Function2<java.lang.Object, java.lang.Object, java.lang.Object> c;
      public int adder(int m, int n) {
            return m + n;
      }
      public scala.Function2<java.lang.Object, java.lang.Object, java.lang.Object> c() {
            return this.c;
      }
      public C() {
            super();
            this.c = new C$$anonfun$1(this);
      }
    }

    public final class C$$anonfun$1 extends scala.runtime.AbstractFunction2$mcIII$sp implements scala.Serializable {
      public static final long serialVersionUID;
      private final C $outer;
      public final int apply(int m, int n) {
            return this.apply$mcIII$sp(m, n);
      }
      public int apply$mcIII$sp(int m, int n) {
            return this.$outer.adder(m, n);
      }

      public final java.lang.Object apply(java.lang.Object m, java.lang.Object n) {
            return scala.runtime.BoxesRunTime.boxToInteger(
                this.apply(
                 scala.runtime.BoxesRunTime.unboxToInt(m),
                 scala.runtime.BoxesRunTime.unboxToInt(n))
            )
      }
      public C$$anonfun$1(C c) {
            if c == null throw new NullPointerException();
            this.$outer = c;
            super();
      }
    }

Function2[T1, T2, R]有方法curried，返回T1 => T2 => R，tupled方法，返回Tuple2[T1, T2] => R

### 可变参数
对于有相同类型的方法，有一种特殊的语法。要对几个string调用String的`capitalize`方法，可以写成:

    def capitalizeAll(args: String*) = {
      args.map { arg =>
        arg.capitalize
      }
    }
    capitalizeAll: (args: String*)Seq[String]

    scala> capitalizeAll("rarity", "applejack")
    res2: Seq[String] = ArrayBuffer(Rarity, Applejack)

### 类

    scala> class Calculator {
         |   val brand : String = "HP"
         |   def add(m : Int, n : Int) : Int = m + n
         | }
    defined class Calculator

    scala> val calc = new Calculator
    calc: Calculator = Calculator@5f701b

    scala> calc.add(1, 2)
    res1: Int = 3

    scala> calc.brand
    res2: String = HP
使用def来定义方法，val来定义字段。方法只是访问类的状态的函数。

### 构造器
构造器不是特殊的方法，它们是在类中方法定义之外代码。现在，我们扩展Calculator类，用一个构造器参数来初始化内部状态。

    class BasicCalculator(brand: String) {
      /**
       * A constructor.
       */
      val color: String = if (brand == "TI") {
        "blue"
      } else if (brand == "HP") {
        "black"
      } else {
        "white"
      }

      // An instance method.
      def add(m: Int, n: Int): Int = m + n
    }
注意注释的两种不同形式。
可以使用这个构造器来构造一个实例：

    scala> val calc = new BasicCalculator("HP")
    calc: Calculator = Calculator@fdb2d2

    scala> calc.color
    res3: String = black

### 表达式
BasicCalculator的例子表示Scala怎么是面向表达式的。color的值被限定在if/else表达式。Scala是高度面向表达式的：大部分的东西是表达式而不是语句。

### 题外话：函数和方法
函数和方法是深度可交互的。因为函数和方法是如此相似，你可能忘记你所调用的是一个函数还是方法。当你偶然遇见方法和函数的不同时，那就可能会使你感到困惑。

    scala> class C {
         |   var acc = 0
         |   def minc = { acc += 1 }
         |   val finc = { () => acc += 1 }
         | }
    defined class C

    scala> val c = new C
    c: C = C@1af1bd6

    scala> c.minc // calls c.minc()

    scala> c.finc // returns the function as a value:
    res2: () => Unit = <function0>
当你调用一个函数没有带括号时，你可能会想起Whoops，我原以为我明白Scala函数是如何工作的，但是我猜不是。可能他们有时候需要括号？你可能理解函数，但是使用的是一个方法。

实际上，你可能用Scala做大量的事，而对函数和方法仍然是模糊不清的。如果你是个Scala的新手，读一读[explanations of the differences](https://www.google.com/search?q=difference+scala+function+method),
你可能遇到困难，那并不意味着你对使用Scala有困难。那只是意味着函数和方法很巧妙，说明是深入Scala语言的部分。

### 继承

    class ScientificCalculator(brand: String) extends Calculator(brand) {
      def log(m: Double, base: Double) = math.log(m) / math.log(base)
    }
参考深入Scala指出，如果子类与父类不是完全不同的，那么用[Type alias](http://twitter.github.io/effectivescala/#Types%20and%20Generics-Type%20aliases)
比`extends`更好。Scala一览描述[Subclassing](http://www.scala-lang.org/old/node/125)

### 重载方法

    class EvenMoreScientificCalculator(brand: String) extends ScientificCalculator(brand) {
      def log(m: Int): Double = log(m, math.exp(1))
    }

### 抽象类
可以定义一个抽象类，一个只定义一些方法但不实现它们的类。子类继承这个抽象类，定义了这些方法。不能创建一个抽象类的实例。

    scala> abstract class Shape {
         |     def getArea() : Int
         | }
    defined class Shape

    scala> class Circle(r : Int) extends Shape {
         |     def getArea(): Int = {r * r * 3}
         | }
    defined class Circle

    scala> val s = new Shape
    <console>:8: error: class Shape is abstract; cannot be instantiated
           val s = new Shape
                   ^

    scala> val c = new Circle(2)
    c: Circle = Circle@1fcf4be

### 特质
traits是字段和行为的集合，可以继承或混入到类当中。

    trait Car {
      val brand: String
    }

    trait Shiny {
      val shineRefraction: Int
    }

    class BMW extends Car {
      val brand = "BMW"
    }
一个类可以继承多个traits用关键字`with`

    class BMW extends Car with Shiny {
      val brand = "BMW"
      val shineRefraction = 12
    }

在深入Scala中有相关介绍：[trait](http://twitter.github.io/effectivescala/#Object oriented programming-Traits)
什么时候用Trait来代替抽象类呢？如果你想要定义一个类似接口的类型，你可能发现很难在Trait和抽象类中选择。任何一个都可以在一个类型中定义一些行为，让
继承者定义其他一些行为。一些规则：
   * 优先使用trait，一个类可以很方便地继承多个traits,但一个class只能继承一个类。
   * 如果需要一个构造器参数，使用抽象类。抽象类的构造器可以有多个参数，而trait构造器不能。如果定义`trait t(i : Int) {}`：这里的i是不合法的。
更深入的内容查看[stackoverflow:Scala traits vs abstract classes](http://stackoverflow.com/questions/1991042/scala-traits-vs-abstract-classes)
[Difference between Abstract Class and Trait](http://stackoverflow.com/questions/2005681/difference-between-abstract-class-and-trait)
和[Programming in Scala: To trait, or not to trait?](http://www.artima.com/pins1ed/traits.html#12.7)

### 类型
在之前，我们看到用`Int`来定义了一个函数，它是一个Number类型。函数可以是范型，工作在任意类型的。这是一个例子：

    trait Cache[K, V] {
      def get(key: K): V
      def put(key: K, value: V)
      def delete(key: K)
    }

方法也可以有类型参数：

    def remove[K] (key : K)