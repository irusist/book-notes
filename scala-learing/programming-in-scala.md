## scala编程

源码地址： http://booksites.artima.com/programming_in_scala

函数式编程特点:

   *  函数是头等值，可以定义匿名函数，并随意地插入到代码的任何地方，就像使用42这样的字面量一样。
   *  方法不应有任何副作用，方法与其所在的环境交流的唯一方式应该是获得参数和返回结果，对任何输入来讲，都可以用方法的结果替代对它的调用，而不影响程序的语义。
   *  鼓励使用不可变数据结构和指称透明(referentially transparent, 无副作用)的方法

统一对象模型                          Smalltalk
嵌套的思想                           Algol, Simula, Beta. gbeta
方法调用和字段选择的统一访问原则        Eiffel
函数式编程                           SML, OCaml, F#(ML家族)
隐式参数                               Haskell
actor                               Erlang


在解释器里要分多行写一个String，可以直接回车，解析器会在下一行的前面自动加上|，如果要取消，可以按2次回车

def max(x : Int, y : Int) : Int = {
    if (x > y) x
    else y
}

def max(x : Int, y : Int) = if (x > y) x else y     # 如果可以类型推断，返回值的类型可以省略，如果返回值只有一个语句，则可以省略大括号
def greeting() = println("Hello World")     # ()Unit  Unit表示没有返回值

scala不能写成 i++或 ++i，要写成i = i + 1，或者i += 1

scala的if，while语句必须加括号

args.foreach(arg => println(arg)) 或  args.foreach((arg : String) => println(arg)) 如果函数的参数要加类型，必须加上括号
args.foreach(println)  如果函数只有一行语句，只带一个参数，就可以省略参数和成这样

函数字面量：(x : Int, y : Int) => x + y

// a是一个val,不是var
for (a <- args) {
    println(arg)
}


### 数组Array
val greetingStrings = new Array[String](3)
greetingStrings(0) = "Hello"
greetingStrings(1) = ","
greetingStrings(2) = "World!\n"

for (i <- 0 to 2)
    print(greetingStrings(i))

 * 方法若只有一个参数，调用的时候就可以省略点及括号，to是Int参数的方法，
这个语法只有在明确指定方法调用的接受者时才有效。不可以写成println 10，但可以写成Console println 10
    1 + 2等价于 (1).+(2)
 * 用括号传递给变量一个或多个值参数时，scala会把它转换成对apply方法的调用：
    greetingStrings(1)  转换成greetingStrings.apply(1)
 * 当对带有括号并包括一到若干参数的变量赋值时，编译器将时用对象的update方法对括号里的参数（索引值）和等号右边的对象执行调用。
    greetingStrings(0) = "Hello"  被转换成 greetingStrings.update(0, "Hello")
    val numNames = Array("zero", "one", "two") // 调用了创造并返回新数组的apply工厂方法，apply方法可以有不定个数的参数。定义在Array的伴生对象（companion
    object)中，完整写法为： val numNames = Array.apply("zero", "one", "two")

### 列表List
数组Array是可变的，List是不可变的：
val oneTwoThree = List(1, 2, 3)

::发音cons。它可以把新元素组合到现有列表的最前端，然后返回作为执行结果的新列表：
val twoThree = List(2, 3)
val oneTwoThree = 1 :: twoThree
println(oneTwoThree)   // List(1, 2, 3)

如果方法使用操作符来标注，如 a * b，那么左操作数是方法的调用者，可以改写成 a.*(b).除非方法名以冒号结尾。在这种情况下，方法被右操作数调用。

Nil是空列表的简写： 1 :: 2 :: 3 :: Nil

List的一些方法和作用：
方法名                                                 方法作用
List()或者Nil                                         空List
List("Cool", "tools", "rule")                       创建带有3个值Cool, tools, rule的新List[String]
val thrill = "Will" :: "fill" :: "until" :: Nil      创建带有3个值Will，fill，until的新列表List[String]
List("a", "b") ::: List("c", "d")                   叠加2个列表，返回带a, b, c, d的新List[String]
thrill(2)                                           返回在thrill列表上索引为2（基于0）的元素util
thrill.count(s => s.length == 4)                    计算长度为4的String元素个数：2
thrill.drop(2)                                      返回去掉前2个元素的thrill列表：List("until")
thrill.dropRight(2)                                 返回去掉后2个元素的thrill列表：List("Will")
thrill.exists(s => s == "until")                    判断是否有值为until的字符串元素在thrill里：true
thrill.filter(s => s.length == 4)                   返回长度为4的元素一次组成的新列表:List("Will", "fill")
thrill.forall(s => s.endWiths("l"))                 判断是否thrill列表里所有元素都以l结尾：true
thrill.foreach(s => print(s))                        对thrill列表每个字符串执行print语句: Willfilluntil
thrill.foreach(print)                               与前面一样
thrill.head                                         返回thrill第一个元素：Will
thrill.init                                          返回thrill列表最后一个以外其他元素组成的列表：List("Will", "fill")
thrill.isEmpty                                      返回thrill是否为空：false
thrill.last                                         返回thrill的最后一个元素:until
thrill.length                                       返回thrill的元素个数：3
thrill.map(s => s + "y")                            返回List("Willy", "filly", "untily")
thrill.mkString(", ")                               返回Will, fill, until
thrill.remove(s => s.length == 4)                   返回List("until")
thrill.reverse                                      返回List("until", "fill", "Will")
thrill.sort((s, t) => s.charAt(0).toLowerCase <
t.charAt(0).toLowerCase)                            返回List("fill", "until", "Will")
thrill.tail                                         返回List("fill", "until")

### 元组Tuple
元组是不可变的，与List不同，元组可以包含不同类型的元素
val pair = (99, "Luftballone")      // Scala类型推断出元组类型是Tuple2[Int, String]
println(pair._1)                    // _1表示第一个元素， _2表示第2个元素，.表示字段访问或者方法调用，访问_1和_2字段
println(pair._2)                    // 不是通过apply来访问，因为tuple里面的类型是不一样的

Scala库支持到Tuple32

### 集set，映射map
set和map有可变和不可变的

    var jetSet = Set("Boeing", "Airbus")  // 默认的Set是不可变的，对scala.collection.immutable.Set伴生对象调用了apply方法
    jetSet += "Lear"     // 调用的是 + 方法
    println(jetSet.contains("Cessna"))

    import scala.collection.mutable.Set
    val movieSet = Set("Hitch", "Poltergeist")
    movieSet += "Shrek"  // 调用 += 方法
    println(movieSet)

    import scala.collection.inmutable.HashSet
    val movieSet = HashSet("Hitch", "Poltergeist")

    import scala.collection.mutable.Map
    val treasureMap = Map[Int, String]()
    treasureMap += (1 -> "Go to island.")
    treasureMap += (2 -> "Find big X on ground")
    treasureMap += (3 -> "Dig.")
    println(treasureMap(2))

    val romanNumeral = Map(1 -> "1", 2 -> "II", 3 -> "III")
    println(romanNumeral(2))

Scala的任何对象都能调用->方法，并返回包含键值对的二元组

识别函数是否有副作用的地方就在于其结果类型是否为Unit，比如方法返回Unit，打印内容，则是有副作用的。无副作用的方法更容易测试

    import scala.io.Source

    def widthOfLength(s : String) = s.length.toString.length

    if (args.length > 0) {
        val lines = Source.fromFile(args(0)).getLines.toList

        val longestLine = lines.reduceLeft(
           (a, b) => if (a.length > b.length) a else b
        )
        val maxWidth = widthOfLength(longestLine)

        for (line <- lines) {
            val numSpaces = maxWidth- widthOfLength(line)
            val padding = " " * numSpaces
            print(padding + line.length + " | " + line)
        }
    } else
        Console.err.println("Please enter filename")

## 类和方法
Scala的默认访问级别是public，私有的要定义成private

    class A {
        private var sum = 0
    }

    val a = new A

Scala的方法参数否是val，不是var，如果想在方法里给参数重新赋值，会编译失败

如果没有发现任何显式的返回语句，Scala方法将返回方法中最后一次计算得到的值

假如某个方法仅计算单个结果表达式，则可以去掉花括号，如果表达式很短，可以把它放在def同一行里

返回Unit的方法可以去掉返回类型和等号，直接放在大括号里：
    def add(b : Byte) { sum += b}

如果去掉方法体前面的等号，那么方法的结果类型就必定是Unit，编译器会把方法体的任何类型转化为Unit
    def f() : Unit = "this string gets lost"  // 将String忽略，转化为Unit
    def g() {"this String gets lost too"} // 没有等号，转化为Unit

如果一行包含多条语句，则分号是必须的。
x
+ y 会被当成是2个语句，需要以下操作
(x
+ y) 或者将类似+这样的中缀操作符，通常放在行尾，如
x +
y +
z

分号推断规则：除非以下情况的一种成立，否则行尾被认定是一个分号
    * 疑问行由一个不能合法作为语句结尾的字结束，如句点或中缀操作符
    * 下一行开始于不能作为语句开始的词
    * 行结束于括号( ...)或方框[...]内部，因为这些符号不可能容纳多个语句


用object关键字代替class关键字类定义单例对象，当单例对象与某个类共享一个名称时，它被称为是这个类的伴生对象(companion object)
。类和它的伴生对象必须定义在一个源文件里。类被称为这个单例对象的伴生类（companion class)。类和它的伴生对象可以相互访问其私有成员。

单例对象不带参数。类可以带参数，因为单例对象不是用new关键字实例化的，不能传递实例化参数。每个单例对象都被实现为虚构类(synthetic class)的实例，并指向静态的变量。
单例对象在第一次被访问的时候才会被初始化
虚构类的名字是对象名加上一个美元符号，单例对象ChecksumAccumulator的虚构类是ChecksumAccumulator$

不与伴生类共享名称的单例对象被称为独立对象(standalone object)，可定义工具类，或scala应用的入口点。

任何拥有合适签名的main方法（仅带一个参数Array[String]，结果类型为Unit）的单例对象都可以用作程序的入口点。

scala每个源文件都包含了java.lang，包scala,以及单例对象Predef的成员引用。

scalac命令
fsc（fast scala compiler）命令：第一次执行fsc时，会创建一个绑定在计算机端口上的本地服务器后台进程，然后它就会把文件列表通过端口发送给后台进程，由后台进程完成编译。
下一次执行fsc时，检测到后台进程已经存在，fsc只把文件列表发给后台进程，立刻开始编译文件。  fsc -shutdown

scala.Application特质：extends Application, 可以把想要执行的代码直接放在单例对象的花括号之间。但不能访问args参数



## 基本类型和操作
类型Byte Short Int Long和Char被称为整数类型(integral type)，整数类型加上Float和Double被称为数类型(numeric type)
String是java.lang包，Byte, Short, Int, Long, Char, Float, Double, Boolean是scala包的
Int(scala.Int)等价于int，但推荐写成Int，scala的基本类型与java的原始类型一致

### 数字
scala默认的数字是int，加上l或L是long，clojure默认的就是long

    val a : Byte = 127  // 定义成byte a ，使用bipush 127
    val b : Short = 234  // 定义成short b ，使用sipush 234
    val c : Char = 2     // 定义成char c，使用iconst_2

scala默认的浮点数是double，可以加D结尾，如果加f或F结尾，就是float，clojure默认的是double

### 字符
八进制表示字符：val c = '\101'  // c : Char = A
十六进制表示字符: val f = '\u00444'  // f : Char = D
Unicode形式的字符可以出现在scala的任何地方： val B\u0041\u0044 = 1 // BAD : Int = 1 ，这样可以在scala的源文件中使用非ASCII字符
字面量             含义
\n              换行(\u000A)
\b              回退(\u0008)
\t              制表符(\u0009)
\f              换页符(\u000C)
\r              回车(\u000D)
\"              双引号(\u0022)
\'              单引号(\u0027)
\\              反斜杠(\u005C)

### 字符串
scala原始字符串引用一种特殊语法：以同一行里的三个引号(""")作为开始和结束，内部的原始字符串可以包含无论何种任意字符，包括新行，引号等特殊字符。

    println("""Welcome to Ultamix 3000.
                Type "HELP" for help.""")   // 在编译的时候自动加上\n, 空格，\t,\"等内容组成一个String
打印出：
Welcome to Ultamix 3000.
                Type "HELP" for help.

字符串类的stripMargin方法，把管道符号（|)放在每行前面，然后对整个字符串调用stripMargin:

    println("""|Welcome to Ultamix 3000.
               |Type "HELP" for help.""".stripMargin)  // 将String隐式转换成StringOps,调用StringOps的stripMargin方法

打印出：
Welcome to Ultamix 3000.
Type "HELP" for help.

### 符号字面量
符号字面量被写成'<标识符>。标识符可以是任何字母或数字的标识符，这种字面量被映射成预定义类scala.Symbol的实例。如;cymbal被编译器扩展为工厂方法调用：Symbol("cymbal")

    class S {
        var a = 'symbol
        println(a.name)

    生成java如下:
    public S() {
        super();
        a = scala.Symbol$.MODULE$.apply("symbol")
        scala.Predef$.println(this.a.name());
    }
    private scala.Symbol a;

    public scala.Symbol a() {
        return a;
    }

    public void a_$eq(scala.Symbol a) {
        this.a = a
    }



    final class A private (var name : String) extends Thread {
    }
    生成的java如下：
    public final class A extends java.lang.Thread {
        private java.lang.String name;
        public java.lang.String name() {
            return name;
        }

        public void name_$eq(java.lang.String name) {
            this.name = name;
        }

        private A(String name) {
            // this.name = name; 这步骤是在调用父类构造方法之前，在java中是定义实例变量的时候定义的，这里直接写死自己码，因为这个参数name是在定义类的时候传进来的
            super();
        }
    }


    var b = Symbol("test")
    生成的java代码如下：
    private scala.Symbol b = scala.Symbol$.MODULE$.apply("symbol");
    public scala.Symbol b() {
        return b;
    }
    public void b_$eq(scala.Symbol b) {
        this.b = b;
    }

val a = 'aSymbol，其中符号是被限定的(interned)，如果同一个符号字面量出现2次，两个字面量指向同一个Symbol对象，内部通过WeakHashMap实现

### 布尔字面量
true,false

### 操作符和方法
中缀操作符：

    val a = 1 + 2
    val b = 1 + 2L
    val s = "Hello, World"
    s indexOf 'o'
    s indexOf ('o', 5)

前缀操作符：标识符中能作为前缀操作符的只有+, -, !和~,如果定义方法unary_*，但是不能调用*p,可以调用p.unary_*，如果调用*p,scala会理解成*.p

    -2.0    // 相当于调用(2.0).unary_-，在前缀操作符前加上unary_

后缀操作符：后缀操作符是不用点或括号调用的不带任何参数的方法，在scala里，方法调用的空括号可以省略，一般是如果方法带有副作用就加上括号，如println(),如果没有副作用，就去掉括号

短路机制的工作原理：通常，进入方法之前会评估所有的参数，所有的scala方法都有延迟其参数评估乃至取消评估的机制，被称为传名参数(by-name parameter)


### 对象相等性：
==：首先检查左边是否为null，如果不是，调用左操作数的equals方法，scala的eq与java的==类似

### 操作符的优先级：
scala根据操作符的第一个字符判断方法的优先级，比如方法名始于*，就比开始于+的方法有更高的优先级，因此2+2*7就被评估为2+(2*7)
操作符的优先级：降序方式，同一行字符具有相同的优先级，
    所有其他特殊字符
    * / %
    + -
    :
    = !
    <>
    &
    ^
    |
    所有字母
    所有赋值操作符


比如：2 << 2 + 2 ，由于+比<要高，所以评估成2 << (2 + 2)

以等号结束的赋值操作符：如果操作符以等号字符（=）结束，且操作符并非比较操作符<=, >= == 或=，那么这个操作符的优先级与赋值符(=)相同。比如：
    x *= y + 1 等价于   x *= (y + 1)

当同样优先级的多个操作符并列出现在表达式里时，操作符的关联性决定了操作符分组的方式。scala里操作符的关联性取决与它的最后一个字符。
如任何以":"字符结尾的方法由它的右操作数调用，并传入左操作数，其他字符结尾的方法与之相。因此a ::: b变成b.:::(a)
不论操作符具有什么样的关联性，他的操作数总是从左到右评估的，因此a ::: b 应该被当作：{val x = a; b.:::(x)}

这种关联规则在同时使用多个具有相同优先级的操作符时也会起作用：如a ::: b ::: c会被当作a :::(b ::: c),而a * b * c被当作(a * b) * c

### 富包装器：
代码          结果      基本类型            富包装
0 max 5      5         Byte             scala.runtime.RichByte
0 min 5     0           Short           scala.runtime.RichShort
-2.7 abs     2.7        Int             scala.runtime.RichInt
-2.7 round   -3L        Long            scala.runtime.RichLong
1.5 isInfinity  false   Char            scala.runtime.RichChar
(1.0/0) isInfinity  true    String      scala.runtime.RichString
4 to 5      Range(4, 5, 6)   Float      scala.runtime.RichFloat
"bob" capitalize  "Bob"    Double       scala.runtime.RichDouble
"rebert" drop 2 "bert"      Boolean     scala.runtime.RichBoolean


## 函数式对象：

    // 如果类没有实体，就可以不用加花括号,如果n和d没有定义成val或var，则只生成包含2个参数的构造方法，但是不会生成字段和get，set方法，
    // 如果n或者d没有被定义成var或者val, 在类实体中有def定义的方法使用了n或d字段，则会生成n或d字段，但是不会生成这个字段的get，set方法,这个字段是private final的
    // 如果类有实体内容既不是字段，也不是方法定义，则会编译至构造器中。
    // 如果有方法体内有var或val定义其他字段或者非val, var, def定义的内容(如println)使用了n或者d，则不会生成n和d字段，在调用父类构造器之前把n和d这2个值赋值给其他字段或调用println
    // 在类实体中定义的var,val的赋值或者不是val,var,def定义的内容按顺序出现在构造器中调用父类构造器之后
    class Rational(var n : Int, d : Int) {
        override def toString = n + "/" + d   // 方法的override
        def this(n : Int) = this(n, 1)  // 用this来定义辅助构造器
    }
    生成如下代码：
    public class Rational {
        private int n;
        private final int d;
        public int n() {
            return n;
        }

        public void n_$eq(int n) {
            this.n = n;
        }

        public String toString() {
            scala.collection.mutable.StringBuilder builder = new scala.collection.mutable.StringBuilder();
            builder.append(n()).append("/").append(scala.runtime.BoxesRuntime.boxToInteger(this.d));
            return builder.toString();
        }

        public Rational(int n, int d) {
            this.n = n;
            this.d = d;
            super();
            // 。。。 var val 定义的内容或者不是var, val,def定义的内容，如直接写println(n)
        }
    }

Predef.require方法

### 辅助构造器
关键字this指向当前执行方法被调用的对象实例，如果使用在构造器里的话，就是正被构建的对象实例
scala里每个辅助构造器的第一个动作都是调用同类的别的构造器。scala的类里面只有主构造器可以调用超类的构造器。

### 标识符
字母数字标识符：以字母或下划线开始，之后可以跟字母，数字和下划线。$也被当作字母，但是被保留作为scala编译器产生的标识符用。程序中不应包含$字符，有可能与编译器产生的标识符发生名称冲突。
不建议在标识符结尾使用下划线。
scala定义常量只是第一个字母大写，使用驼峰式风格。
操作符标识符：编译器将->形式的操作符替换成$colon$minus$greater等形式
字面量标识符：用反引号`...`包括的任意字符串，如：`x` `<clinit>` `yield`, 即使包含在反引号的名称是scala保留字也是有效的。因为yield是scala保留字，需要这样调用Thread.`yield`()

### 操作符重载

### 隐式转换
    implicit def intToRational(x : Int) = new Rational(x)  // 定义了从Int到Rational的转换
要使隐式转换起作用，需要定义在作用范围之内。需要定义在被转换类之外。


## 内建控制结构
if while for try match : 控制结构都会产生值
while和do...while结构是循环，不是表达式，因为它们不能产生有意义的结果。结果类型是Unit，是表明存在并且唯一存在类型为Unit的值，称为unit value，写成()
()的存在是Scala的Unit不同于java的void的地方。

    def greet() {println("hi")}   // ()Unit
    greet()  == ()
    // hi
    // res0 : Boolean = true

对var再赋值等式本身也是unit值，

    var line = ""
    while ((line = readLine()) != "") // 在scala中会得到警告，!=比较类型为Unit和String的值永远返回true
        println("Read: " + line)      // line = readLine()的返回值是Unit

由于while循环不产生值，因此它进场被纯函数式语言所舍弃，这种语言只有表达式，没有循环。尽量不使用while控制结构

for表达式对任何种类的集合都有效，而不只是数组。 1 to 4 // Range(1, 2, 3, 4)  1 until 4 // Range(1, 2, 3)

    val filesHere = (new java.io.File(".")).listFiles
    for (file <- filesHere
         if file.isFile;
         if file.getName.endWiths(".scala")
    ) println(file)

如果在发生器中加入超过一个过滤器，if子句必须用分号分隔，
for表达式能产生有意义的值，其类型取决于for表达式<-子句的集合。

嵌套枚举：
    def fileLines(file : java.io.File) = scala.io.Source.fromFile(file).getLines.toList
    def grep(pattern : String) =
        for (
            file <- filesHere
            if file.getName.endsWith(".scala")
            line <- fileLines(file)
            if line.trim.matches(pattern)
        ) println(file + ": " + line.trim)

    val list = List(List(1, 2,3 ), List(4, 5, 6))
        for (
          a <- list;
          b <- a
        ) println(a + " " + b)   // 多个嵌套之间必须用分号分隔
可以用花括号代替小括号包裹发生器和过滤器。用花括号时可以省略分号

    val list = List(List(1, 2,3 ), List(4, 5, 6))
    for {
      a <- list
      b <- a
    } {
      println(a + " " + b)
      println("test")
    }



















