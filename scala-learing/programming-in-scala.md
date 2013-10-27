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

### if语句
if语句的判断必须加上括号

### while语句
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

### for表达式
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
             // 此处必须要加分号，否则会报error: ')' expected but '<-' found. line <- fileLines(file) ，因为<-必须替换成对应for后面的括号。
            if file.getName.endsWith(".scala");
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
for语句中可以通过等号（=）把结果绑定到新的变量实现，绑定的变量被当作val引入和使用，不过不带关键字val

    def grep(pattern : String) =
        for (
            file <- filesHere
            if file.getName.endsWith(".scala");
            line <- fileLines(file);
            // 此处必须加上分号，否则报error: value trimmed is not a member of List[String]
            // possible cause: maybe a semicolon is missing before `value trimmed'? trimmed = line.trim ]
           // 编译器会把trimmed当做fileLines(file)的方法调用。
            trimmed = line.trim   // 绑定变量，不用加val，默认是val，于<-的左侧的值一样
            if trimmed.matches(pattern)
        ) println(file + ": " + trimmed)


在for语句中，可以创建一个值去记住每一次的迭代，只要在for表达式之前加上关键字yield。

   def scalaFiles =
        for (
            file <- filesHere
            if file.getName.endsWith(".scala")
        ) yield file  // 当表达式完成时，结果将是包含了所有产生值的集合对象，对象的类型基于枚举子句处理的集合类型，在这里是Array[File]

for {子句} yield {循环}  yield在整个循环的前面，即使循环体是被花括号包围的代码块，也一定要把yield放在左括号之前，而不是代码块的最后一个表达式之前。
如果yield后面的语句只有一句，可以不用加花括号，如果有多句，必须加花括号，返回的是循环语句的返回值的一个Array数组。

    def grep(pattern : String) =
        for (
          file <- filesHere
          if file.getName.endsWith(".scala");

          line <- fileLines(file);
          trimmed = line.trim
          if trimmed.matches(pattern)
        ) yield {
            trimmed.length
        }

### try表达式
抛出异常：throw new IllegalArgumentException
在scala里，throw也是有结果类型的表达式，抛出异常的类型是Nothing
捕获异常：使用模式匹配

    try {
        val f = new FileReader("input.txt")
        // 使用并关闭文件
    } catch {
        case ex : FileNotFoundException => //处理丢失的文件
        case ex : IOException => // 处理其他IO错误
    }
scala不需要捕获检查异常(checked exception),或把它们声明在throws 子句中，可以用@throws 注解声明throws子句。但这不是必须的。
finally子句：

    val file = new FileReader("input.txt)
    try {
        //
    } finally {
        file.close()
    }

try-catch-finally表达式会产生值，返回的结果是，如果没有抛出异常，返回try子句，如果抛出异常并被捕获，则返回catch子句，如果异常被抛出但
没有被捕获，表达式就没有返回值。由finally子句计算得到的值，即使有也会被抛弃。
在java里，如果finally子句包含了返回语句，或抛出一个异常，这个返回值或异常将会凌驾于任何值钱在try代码块或某个catch子句里产生的值或异常之上。

    def f() : Int = try {return 1} finally {return 2}  // 返回2
    def g() : Int = try {1} finally {2}  // 返回1

### 匹配表达式match
match表达式类似于switch

        val fileArgs = if (args.length > 0) args(0) else ""
        fileArgs match {
            case "salt" => println("pepper")
            case "chips" => println("salsa")
            case "egge" => println("bacon")
            case _ => println("huh?")
        }

任何类型的常量，或其他什么东西，都能当成scala里做比较用的样本（case），而不是java的case语句里的整数类型和枚举类型。每个case语句中没有break。break是隐含的。
match表达式也能产生值，

        val fileArgs = if (args.length > 0) args(0) else ""
        def friend =
            fileArgs match {
                case "salt" => "pepper"
                case "chips" => "salsa"
                case "egge" => "bacon"
                case _ => "huh?"
        }

可以用if来替换每个continue，用布尔变量替换每个break，布尔变量用来说明while是否继续。

        // java代码：
        int i = 0;
        boolean found = false;
        while (i < args.length) {
            if (args[i].startsWith("-") {
                i = i + 1;
                continue;
            }

            if (args[i].endsWith(".scala")) {
                found = true;
                break;
            }

            i = i + 1;
        }

        // scala代码1
        var i = 0
        var found = false
        while (i < args.length && !found) {
            if (!args(i).startsWith("-")) {
                if (args(i).endsWith(".scala")) {
                    found = true
                }
            }

            i = i + 1
        }

        // scala代码2
        def searchFrom(i : Int) =
            if (i >= args.length) -1
            else if (args(i).startsWith("-") searchFrom(i + 1)
            else if (args(i).endsWith(".scala") i
            else searchFrom(i + 1)
        val i = searchFrom(0)

### 变量范围
可以在内部范围内定义与外部范围里名称相同的变量：

    val a = 1;
    {
        val a = 2 //编译通过，定义在这的a是不通过的变量，仅在花括号内部有效。内部变量隐蔽了同名的外部变量。
        println(a) // 打印2
    }
    println(a)  // 打印1

   // 在解释器中执行
   scala> val a = 1
   a : Int = 1
   scala> val a = 2
   a : Int = 2
   scala> println(a)
   2
   在解释器中，可以随意重用变量名，解释器为每次输入的新语句都创建了新的嵌套范围，上面的行为相当于：
   val a = 1;
   {
        val a = 2;
        {
            println(a)
        }
   }

1 to 10
Array.mkString

## 函数和闭包

### 本地函数：
可以把函数定义在函数内部，这样本地函数仅在包含它的代码块中可见，本地函数可以访问包含其函数的参数   (字节码实现?)

    def processFile(fileName: String, width: Int) {
        def processLine(line: String) {
          if (line.length > width) {
            print(fileName + ": " + line)
          }
        }

        val source = Source.fromFile(fileName)
        for (line <- source.getLines()) {
          processLine(line)
        }
  }

### 头等函数
函数字面量被编译进类，并在运行期实例化为函数值(function value)，任何函数值都是某个扩展了scala包的若干FunctionN特质之一的类的实例。如：
Function0是没有参数的函数，Function1是有一个参数的函数，每个FunctionN特质有一个apply方法用来调用函数
函数字面量于值的区别在于函数字面量存在于源码，而函数值作为对象存在于运行期。

函数字面量例子：

        (x : Int) => x + 1
函数值是对象，所以可以存入变量，它们也是函数，所以可以用括号函数调用它们。

        scala> var increase = (x : Int) => x + 1
        increase: (Int) => Int = <function>
        scala> increase(10)
        res0 : Int = 11

        class F {
            var release = (x : Int) => x + 1
            var test = (x : Int, y : Int) => x + y
            release(2)
        }

        会生成如下的java代码：
        F.class:
        public class F {
            private scala.Function1<java.lang.Object, java.lang.Object> release = new F$$anonfun$1(this);
            private scala.Function2<java.lang.Object, java.lang.Object, java.lang.Object> test;
            public scala.Function1<java.lang.Object, java.lang.Object> release() {
                return release;
            }
            public void release_$eq(scala.Function1<java.lang.Object, java.lang.Object> release) {
                this.release = release;
            }
            public scala.Function2<java.lang.Object, java.lang.Object, java.lang.Object> test() {
                return test;
            }
            public void test_$eq(scala.Function2<java.lang.Object, java.lang.Object, java.lang.Object> test) {
                this.test = test;
            }

            public F() {
                super();
                this.release = new F$$anonfun$1(this);
                this.test = new F$$anonfun$2(this);
                release().apply$mcII$sp(2);
            }
        }

        F$$anonfun$1.class
        public final class F$$anonfun$1 extends scala.runtime.AbstractFunction1$mcII$sp implements scala.Serializable
         {
            public static final long serialVerionID = 0L;
            public final int apply(int x) {
                return apply$mcII$sp(x);
            }

            public int apply$mcII$sp(int x) {
                return x + 1;
            }

            public final java.lang.Object(java.lang.Object x) {
                return scala.runtime.BoxesRunTime.boxToInteger(apply(scala.runtime.BoxesRuntime.unboxToInt(x)))
            }
         }
         public F$$anonfun$1(F f) {
            super();
         }

         F$$anonfun$2.class
         public final class F$$anonfun$2 extends scala.runtime.AbstractFunction2$mcIII$sp implements scala.Serializable
          {
             public static final long serialVerionID = 0L;
             public final int apply(int x, int y) {
                 return apply$mcIII$sp(x, y);
             }

             public int apply$mcIII$sp(int x, int y) {
                 return x + y;
             }

             public final java.lang.Object(java.lang.Object x, java.lang.Object y) {
                 return scala.runtime.BoxesRunTime.boxToInteger(apply(scala.runtime.BoxesRuntime.unboxToInt(x),
                 scala.runtime.BoxesRuntime.unboxToInt(y)))
             }
          }
          public F$$anonfun$2(F f) {
             super();
          }

如果函数字面量包含多条语句，可以用花括号包住函数体，一行放一条语句，函数的返回值是最后一行表达式产生的值。
所有的集合类都能用到foreach方法，foreach方法定义在特质Iterable中，它是List, Set, Array, 和Map的共有超特质。

###　函数字面量的短格式
  * 去掉参数类型

        someNumbers.filter((x) => x > 0)    // 根据someNumbers能够推断出x是什么类型，被称为目标类型化：target typing，可以先去掉类型，再看是否出错，慢慢调试
  * 某种参数的类型是被推断的，省略其外的括号：

        someNumbers.filter(x => x > 0)

### 占位符语法：
可以把下划线当做一个或更多参数的占位符，只要每个参数在函数字面量内仅出现一次：

        someNumbers.filter(_ > 0)
有时下划线当做参数的占位符，编译器可能无法推断缺失的参数类型，如写成_ +　_

        val f = _ + _  // 错误
        val f = (_ : Int) + (_ : Int) // 这时可以用冒号指定类型，此处f被定义成含有2个Int类型参数的函数，多个下划线，表示多个参数，
### 部分应用函数
可以用单个下划线替换整个参数列表：例如写成println(_),或者println_:

        someNumbers.foreach(println _)

        def sum(a : Int, b : Int, c : Int) = a + b + c
        sum(1, 2, 3)
部分应用(partially applied function)函数是一种表达式，不需要提供函数需要的所有参数，

        def sum(a : Int, b : Int, c : Int) = a + b + c
        val a = sum _    // 这里缺失了3个参数，所以_代表3个参数，sum和_中间必须有空格,在这里会产生一个Function3的类，即P$$anonfun$1
        // a : (Int, Int, Int) => Int = <function>
        // a(1, 2, 3)
        val b = sum(1, (_ : Int), 3)

        生成的java如下：
        P.class:
        public class P {
          private final scala.Function3<java.lang.Object, java.lang.Object, java.lang.Object, java.lang.Object> a;
          private final scala.Function1<java.lang.Object, java.lang.Object> b;

          public int sum(int x, int y , int z) {
            return x +　y + z;
          }
          public scala.Function3<java.lang.Object, java.lang.Object, java.lang.Object, java.lang.Object> a() {
            return a;
          }

          public scala.Function1<java.lang.Object, java.lang.Object> b() {
            return b;
          }

          public P() {
            super();
            this.a = new P$$anonfun$2(this);
            this.b = new P$$anonfun$1(this);
          }
        }

        P$$anonfun$1.class
         public final class P$$anonfun$1 extends scala.runtime.AbstractFunction1$mcII$sp implements scala.Serializable {
           public static final long serialVersionUID;
           private final P $outer;
           public final int apply(int x) {
                return  apply$mcII$sp(x);
           }

           public int apply$mcII$sp(int x) {
                return $outer.sum(1, x, 3);
           }

           public final java.lang.Object apply(java.lang.Object x) {
                return scala/runtime/BoxesRunTime.boxToInteger(apply(
                scala/runtime/BoxesRunTime.unboxToInt(x));
           }
           public P$$anonfun$1(P p) {
              if p == null throw new NullPointerException();
              $outer = p;
              super();
           }
         }


         P$$anonfun$1.class ,注意，这个类继承的是AbstractFunction3，而不是AbstractFunction3$mcIIII$sp，所以没有apply$mcIIII$sp方法，为什么呢？
         // 因为AbstractFunction1和AbstractFunction2都有@specialized设置，而AbstractFunction3没有
         public final class P$$anonfun$2 extends scala.runtime.AbstractFunction3<java.lang.Object, java.lang.Object, java.lang.Object, java.lang.Object> implements scala.Serializable {
           public static final long serialVersionUID;
           private final P $outer;
           public final int apply(int x , int y, int z) {
                return $outer.sum(x, y, z);
           }
           public final java.lang.Object apply(java.lang.Object x, java.lang.Object y, java.lang.Object z) {
                return scala/runtime/BoxesRunTime.boxToInteger(apply(
                scala/runtime/BoxesRunTime.unboxToInt(x),
                scala/runtime/BoxesRunTime.unboxToInt(y),
                scala/runtime/BoxesRunTime.unboxToInt(z));
           }
           public P$$anonfun$2(P p) {
              if p == null throw new NullPointerException();
              $outer = p;
              super();
           }
         }

可以通过提供某些但不是全部需要的参数表达一个偏函数：

         val b = sum(1, _ : Int, 3)  // b : (Int) => Int = <function>,这时产生的P$$anonfun$1的apply方法只有一个参数，并且是一个Function1

如果增在写一个省略所有参数的偏程序表达式，如println _或sum _，而且在代码的那个地方正需要一个函数，可以去掉下划线。

        somNumbers.foreach(println)  // 这里可以去掉下划线，因为foreach的参数需要的是一个函数
        val c = sum  // 这里不能省略下划线，因为这里没有需要一个函数


### 闭包：
使用的实例是那个在闭包被创建的时候活跃的。

      def makeIncreaser(more : Int) = (x : Int) => x + more // 每次函数被调用时都会创建一个新的闭包，每个闭包都会访问闭包创建时活跃的more变量。
      val inc1 = makeIncreaser(1)
      val inc2 = makeIncreaser(2)

      以上代码生成以下的java代码：
      C.class
      public class C {
        private final scala.Function1<java.lang.Object, java.lang.Object> inc1;

        private final scala.Function1<java.lang.Object, java.lang.Object> inc2;

        public scala.Function1<java.lang.Object, java.lang.Object> makeIncreaser(int more) {
            return new C$$anonfun$makeIncreaser$1(this, more)
        }

        public scala.Function1<java.lang.Object, java.lang.Object> inc1() {
            return inc1;
        }
        public scala.Function1<java.lang.Object, java.lang.Object> inc2() {
            return inc2;
        }

        public C() {
            super();
            this.inc1 = makeIncreaser(1);
            this.inc2 = makeIncreaser(2);
        }
      }

      C$$anonfun$makeIncreaser$1.class
      public final class C$$anonfun$makeIncreaser$1 extends scala.runtime.AbstractFunction1$mcII$sp implements scala.Serializable {
        public static final long serialVersionUID;

        private final int more$1;

        public final int apply(int x) {
            return apply$mcII$sp(x);
        }
        public int apply$mcII$sp(int x) {
            return more$1 + x;
        }

        public final java.lang.Object apply(java.lang.Object x) {
            return scala/runtime/BoxesRunTime.boxToInteger(
                apply(scala/runtime/BoxesRunTime.unboxToInt(x))
            )
        }

        public C$$anonfun$makeIncreaser$1(C c, int more) {
            this.more$1 = more;
            super();
        }
      }

### 重复参数：
可以指明函数的最后一个参数是重复的，从而允许客户向函数传入可变长度参数列表。要想标注一个重复参数，可在参数类型之后放一个星号。

    def echo(args : String*) = for (arg <- args) println(arg)

函数内部，重复参数的类型是申明参数类型的数组，因此，echo函数里被申明类型String*，而args的类型实际上是Array[String],
如果把一个数组作为参数传入，需要在这个数组参数后面添加一个冒号和一个_*符号，这个特性与python的可变参数类似。

    val arr = Array("What", "up", "doc?")
    echo(arr:_*)

###　尾递归

    版本１：
    def approximate(guess : Double) : Double =
        if (isGoodEnough(guess)) guess
        else approximate(improve(guess))

    版本2：
    def approximate(initialGuess : Double) : Double = {
        var guess = initialGuess
        while(!isGoodEnough(guess)) {
            guess = improve(guess)
        }
        guess
    }

    // 以上2个方法经过编译器优化之后的字节码是一样的

递归调用是approimate函数体执行的最后一件事，在它们最后一个动作调用自己的函数，被称为尾递归：tail recursive
scala编译器检测到尾递归就用新值更新函数参数，然后把它替换成一个会到函数开头的跳转。字节码实现时怎么样的？
如果方法是尾递归，就无须付出任何运行期开销。（不会产生栈溢出）

### 尾递归函数追踪

    // 这个函数不是尾递归，因为在递归调用之后执行了递增操作，如果执行它，抛出异常时可以得到预期的堆栈错误信息（可以显示调用了几次boom方法）
    def boom(x : Int) : Int =
        if (x == 0) throw new Exception("boom!")
        else boom(x - 1) + 1

    // 如果改成尾递归，抛出异常时就不能得到全部堆栈信息，只有一个bang调用
    def bang(x : Int) : Int =
        if (x == 0) throw new Exception("bang！")
        else bang(x - 1)
可以关掉尾递归优化，-g:notailcalls  把这个参数传给scala的shell或者scalac编译器，之后就可以得到完整的堆栈信息。

### 尾递归的局限：
scala仅优化了直接递归调用使其返回同一个函数，如果递归是间接的，就不能优化了，如下：

    def isEven(x : Int) : Boolean =
        if (x == 0) true else isOdd(x - 1)
    def isOdd(x : Int) : Boolean =
        if (x == 0) false else isEven(x - 1)
如果最后一个调用是一个函数值也不能得到尾递归优化，尾递归调用优化限定了方法或嵌套函数必须在最后一个操作调用本身，如下：

    def funValue = nestedFun _
    def nestedFun(x : Int) {
        if (x != 0) {
            println(x); funValue(x - 1)
        }
    }

## 控制抽象
### 柯里化：
柯里化的函数被应用于多个参数列表，而不是仅仅一个，

    def plainOldSum(x : Int, y : Int) = x + y

    // 柯里化后，实际上接连调用了2个传统函数，第一个函数调用带单个名为x的Int参数，并返回第2个函数的函数值。第2个函数带Int参数y。
    def curriedSum(x : Int)(y : Int) = x + y  // curriedSum : (Int)(Int)Int
    // curriedSum(1)(2)
    // 会拆成以下2个函数调用
    def first(x : Int) = (y : Int) => x + y
    val second = first(1)
    second(2)

    val onePlus = curriedSum(1)_ // 这里的下划线是第2个参数的占位符 onePlus(2)



    def curriedSum(x : Int)(y : Int) = x + y
    def onePlus = curriedSum(1)_
    生成的java代码如下：
    public class C {
      public int curriedSum(int x, int y) {
            return x + y
      }

      public scala.Function1<java.lang.Object, java.lang.Object> onePlus() {
            return new C$$anonfun$onePlus$1(this);
      }

      public C() {
            super();
      }

    public final class C$$anonfun$onePlus$1 extends scala.runtime.AbstractFunction1$mcII$sp implements scala.Serializable {
      public static final long serialVersionUID;
      private final C $outer;
      public final int apply(int x) {
            return apply$mcII$sp(x);
      }
      public int apply$mcII$sp(int x) {
           return $outer.curriedSum(1, x);
      }

      public final java.lang.Object apply(java.lang.Object x) {
           return scala/runtime/BoxesRunTime.boxToInteger(
                apply(scala/runtime/BoxesRunTime.unboxToInt(x));
      }
      public C$$anonfun$onePlus$1(C c) {
            if c == null throw new NullPointerException();
            this.$outer = c;
            super();
      }
     }

### 编写新的控制结构：

    def withPrintWriter(file : File, op : PrintWriter => Unit) {
        val writer = new PrintWriter(file)
        try {
            op(writer)
        } finally {
            writer.close()
        }
    }

    // 可以按如下方法调用：
    withPrintWriter(
        new File("date.txt"),
        writer => writer.println(new java.util.Date)
    )

scala的任何方法调用，如果确实只传入一个参数，就能可选地使用花括号替代小括号包围参数

    println("Hello World!")可以写成println {"Hello World!"}

柯里化可以将多个参数转化为只有一个参数。它内部是使用多个参数来组织成柯里化的多组参数，所以上面的例子可以写成：

    // 这个方法与上面的2个参数的withPrintWriter方法等价
    def withPrintWriter(file : File)(op : PrintWriter => Unit) {
        val writer = new PrintWriter(file)
        try {
            op(writer)
        } finally {
            writer.close()
        }
    }

    // 利用函数只有一个参数可以将圆括号替换成花括号的特性，调用这个函数可以写成：
    val file = new File("date.txt");
    withPrintWriter(file) {
       writer => writer.println(new java.util.Date)
    }


### 传名参数：

    var assertionsEnable = true
    def myAssert(predicate : () => Boolean) =
        if (assertionsEnabled && !predicate()) {
            throw new AssertionError
        }
    myAssert(() => 5 > 3)

是实现一个传名函数，要定义参数的类型开始于=>而不是()=>，例如，可以改变类型：()=>Boolean为=>Boolean，上面的例子可以写成

    def byNameAssert(predicate: => Boolean) =
        if (assertionsEnabled && !predicate) {
            throw new AssertionError
        }
    byNameAssert(5 > 3) // 传名类型中，空的参数列表，()被省略，它仅在参数中被允许


## 组合与继承：
用abstract来定义一个抽象类，一个方法只要没有实现（即没有等号或方法体），它就是抽象的。

### 无参方法：

    // 无参方法
    def height : Int = contents.length
    def width : Int = if (height == 0) 0 else contents(0).length

    // 空括号方法
    def height() : Int = contents.length

无论何时，只要方法中没有参数，并且方法仅能通过读取所包含对象的属性去访问可变状态（特指方法不能改变可变状态），就使用无参方法。
这个惯性支持统一访问原则，就是说客户代码不应由属性是通过字段实现还是方法实现而受影响，例如，我们可以选择把width和height作为字段而不是方法实现.可以将它们改成val


scala鼓励使用将不带参数且没有任何副作用的方法定义为无参方法的风格。即省略空括号，但是永远不要定义没有括号的带副作用的方法，无论何时当调用有副作用的方法时应加上空括号。


### 扩展类：用extends
如果省略了extends子句，scala编译器将隐式地假设类扩展自scala.AnyRef

scla里的字段和方法属于相同的命名空间，超类将contents定义为方法，可以在值类将contents定义为val，scala里禁止在同一个类里用相同名称定义字段和方法。
scala仅有2个命名空间：
    值（字段，方法，包还有单例对象）
    类型（类和特质名）


### 参数化字段
scala的参数化字段可以添加private，protected（可以授权给子类访问），或override修饰符：
参数化字段与类的字段的区别是：参数化字段的初始化是在构造器调用父构造器之前，类的字段的初始化时再调用父构造器之后，并且参数化字段会在类的构造函数中，它初始化为构造函数的参数。

    class Cat (
        val dangerous = false
    )

    class Tiger (
        override val dangerous : Boolean,
        private val age : Int
    ) extends Cat

### 调用超类构造器
要调用超类构造器，只要简单地把要传递的参数或参数列表放在超类名之后的括号里即可，

    class ArrayElement(val contents : Array[String])
    class LineElement(s : String) extends ArrayElement(Array(s))

    生成java代码如下：
    public class ArrayElement {
      private final java.lang.String[] contents;
      public java.lang.String[] contents(){
            return contents;
      }

      public ArrayElement(java.lang.String[] contents) {
          this.contents = contents;
          super();
      }
    }

    public class LineElement extends ArrayElement {
      public LineElement(java.lang.String s) {
        String[] array = new String[1];
        array[0] = s;
        super((String[]) (Object[]) array));
      }
    }

###　override修饰符
scala要求，若子类成员所有重写了父类的具体成员则必须带有这个修饰符，若成员实现的是同名的抽象成员时，则这个修饰符是可选的。
若成员并未重写或实现什么基类里的成员则禁用这个修饰符。

这个特性可以避免为父类增加方法，子类如果有相同的方法，可以在编译器出错，而java则会返回错误的结果

###　多态
与java一样

###　final
修饰函数和类，作用于java一样

scala中的数组继承自scala.Seq类，Array类有一些方法：++ zip,转换为一个二元的数组（Tuple2）

    Array(1, 2, 3) zip Array("a", "b") // 生成Array((1, "a"), (2, "b"))

## scala的层级
在scala里，每个类都继承自通用的名为Any的超类。Nothing是所有其他类的子类

顶层的是Any类：定义了以下的方法：

    final def ==(that : Any) : Boolean     // == 总是与equals相同， !=总是于equals相反
    final def !=(that : Any) : Boolean
    def equals(that : Any) : Boolean
    def hashCode : Int
    def toString : String

Any有2个子类：AnyVal和AnyRef。
AnyVal是Scala里每个内建值类的父类，有9个这样的值：Byte, Short, Char, Int, Long, Float, Double, Boolean和Unit
不能用new创造这些类的实例，值类都是定义成既是抽象的又是final的

Byte隐式转换为Short
Short隐式转换为Int
Char隐式转换为Int
Int隐式转换为Long
Long隐式转换为Float
Float隐式转换为Double

    42 max 43 // 43
    42 min 43 // 42
    1 until 5 // Range(1, 2, 3, 4)
    1 to 5 // Range(1, 2, 3, 4, 5)
    3.abs   // 3
    (-3).abs  // 3
方法min, max, until, to和abs都定义在类scala.runtime.RichInt里，并且有一个从类Int到RichInt隐式转换，当在Int上调用的方法没有定义在Int中，但定义在
RichInt中，就应用这个转换。

AnyRef：这个是Scala里所有引用类的基类。实际上是java.lang.Object的别名，因此java里写的类和scala里写的都继承自AnyRef
scala类与java类的不同在于它们还继承自一个名为ScalaObject的特别记号特质，是想要通过ScalaObject包含的scala编译器定义和实现的方法让scala程序的执行更高效。
ScalaObject只包含一个方法，名为$tag，在内部使用以加速模式匹配。

    boolean isEqual(int x, int y) {
        return x == y;
    }
    isEqual(421, 421)  // java中返回true

    boolean isEqual(Integer x, Integer y) {
        return x == y;
    }
    isEqual(421, 421) // java中返回false

    def isEqual(x : Int, x : Int) = x == y
    isEqual(421, 421)   // scala中返回true

    def isEqual(x : Any, x : Any) = x == y
    isEqual(421, 421)   // scala中返回true

对值类型来说，==被设计成是自然（数学或布尔）相等，对于引用类型，==被视为继承自Object的equals方法的别名
可以用eq和ne来比较引用是否相等。


### 底层类型
Null类是null引用对象的类型，它是每个引用类（就是说，每个继承自AnyRef的类）的子类。Null不兼容值类型，例如，不能把null值赋给整数变量。

    val i : Int = null  // type mismatch,found Null(null), required Int\
Nothing类型在Scala的类层级的最底端，它是任何其他类型的子类型，根本没有这个类型的任何值，Nothing可以表示不正常的终止

    def error(message : String) : Nothing =
        throw new RuntimeException(message)   // 异常的返回类型是Nothing
    def divide(x : Int, y : Int) : Int =
        if( y != 0) x / y
        else error("can't divide by zero") // Nothing是Int的子类


## 特质
特质的默认超类是AnyRef,，特质能定义字段和方法，可以用特质的定义做任何用类定义能做的事。

    trait T {
          def test() {
              println("this is a test")
          }
    }

    会生成如下的java代码
    public interface T {
      public abstract void test();
    }

    public abstract class T$class {
      public static void test(T) {
        scala.Prefef$.MODULE$.println("this is a test");
      }

      public static void $init$(T) {

      }

可以使用extends或with关键字，把特质混入类中。

     class T1 extends T
     生成的java代码如下：
     public class T1 implements T {
       public void test() {
             T$class.test(this);
       }
       public T1() {
            super();
            T$class.$init$(this);
       }
     }

特质也是一个类型，内部实现的是一个接口
可以用extends指明待扩展的超类，用with混入特质。如果想混入多个特质，都加载with子句就可以了

    trait T {
          def test() {
              println("this is a test")
          }
    }

    class T1
    // 如果定义一个特质，并且在它内部定义一个抽象方法，那么java中只会生成这个特质相关的接口和方法，如T3，但是不会生成T3$class(它只会在有实现的时候生成）
    // 那么继承这个特质的类就必须定义为abstract或者实现T3中的抽象方法，或者继承一个实现了T3抽象方法的父类。
    trait T3
    class T2 extends T1 with T with T3

    生成的java代码如下，其中T和T$class如上：
    public class T1 {
       public T1() {
            super();
       }
     }

     public interface T3 {

     }

     public class T2 extends T1 implements T,T3 {
       public void test() {
            return T$class.test(this);
       }
       public T2() {
            super();
            T$class.$init$(this);
       }
     }

如果类重写了特质的方法：

    trait T {
      def test() {
          println("this is a test")
      }
    }

    class T1
    class T2 extends T1 with T {
          override def test() {     // 这里必须要加上override
              println("implemention")
          }
    }

    生成的java如下：
    public interface T {
      public abstract void test();
    }
    public abstract class T$class {
      public static void test(T) {
        scala.Predef$.MODULE$.println("this is a test");
      }
      public static void $init$(T) {

      }
    }

    public class T1 {
      public T1() {
            super();
      }
    }

    public class T2 extends T1 implements T {
      public void test() {
          scala.Predef$.MODULE$.println("implemention");
      }
      public T2() {
            super();
            T$class.$init$(this);
      }
    }

特质和类有以下区别：
  * 特质不能有任何类参数，即传递给类的主构造器的参数，因为特质是不能new的，不能这样定义特质：trait NoPoint(x : Int, y : Int)
  * 不论类在哪个角落，super调用都是静态绑定的，而在特质中，它们是动态绑定的，

### 瘦接口对阵胖接口
特质的一种主要应用时可以根据类已有的方法自动为类添加方法，也就是说，特质可以丰富一个瘦接口，把它变为胖接口
要使用特日志丰富接口，只要简单地定义一个具有少量抽象方法的特质----特质接口的瘦部分，和潜在的大量具体方法，所有的都实现在抽象方法之上。然后就可以
把丰富了的特质混入到类中，实现接口的瘦部分，并最终获得具有全部胖接口内容的类。

相当于在java中，定义一个抽象类，瘦部分相当于未实现的抽象方法，胖部分相当于已经实现的方法（模板模式），然后定义一些具体的子类来继承这个抽象类。
但是在java中只能继承一个父类，而在scala中，可以有多个特质，它们相当于有实现的接口，接口的实现的方法是通过一个辅助类T$class的静态方法来提供的。


### Ordered特质
Ordered特质没有定义equals方法，因为无法做到，使用compare实现equals需要检查传入对象的类型。但是因为类型擦出，Ordered本身无法做到这点。

### 用特质来做可堆叠的改变

    abstract class A {
            def test()
    }

    trait B extends A{
          abstract override def test() {     // 这里必须定义成abstract的，因为下面调用了super.test()
                   super.test()   // 调用了super.test，所以必须继承一个类，由于该特质继承了类A、所以混入这个特质的类也必须继承类A
              println("B")
          }
    }

    trait C extends A {
          abstract override def test() {
                   super.test()           // 这里定义成abstract，则在字节码的C类中会生成一个抽象方法C$$super$test。这个方法在混入这个特质的类(E)中实现。
              println("C")                // 在C$class类中的test实现首先会调用混入这个特质的类(E)的C$$super$test方法的实现，再调用println
          }
    }

    class D extends A {
          override def test() {
              println("D")
          }
    }

    class E extends D with B with C

    编译成java代码
    public abstract class A {
      public abstract void test();

      public A() {
            super();
      }
    }

    public interface B {
      public abstract void B$$super$test();
      public abstract void test();
    }

    public abstract class B$class {
      public static void test(B b) {
            b.B$$super$test();  // 这个方法在类E实现了
            scala.Predef$.MODULE$.println("B")
      }
      public static void $init$(B) {

      }
    }

    public interface C {
      public abstract void C$$super$test();
      public abstract void test();
    }

    public abstract class C$class {
      public static void test(C c) {
          c.C$$super$test(); // 这个方法在类C实现了
          scala.Predef$.MODULE$.println("C");
      }


      public static void $init$(C) {

      }
    }

    public class D extends A {
      public void test() {
         scala.Predef$.MODULE$.println("D");
      }

      public D() {
            super();
      }
    }


    public class E extends D implements B,C {
      public void C$$super$test() {      // 这个是实现定义在特质C中的C$$super$test()的方法，它会调用顺序排在它前面的特质或类(B)的test方法
         B$class.test(this);
      }

      public void B$$super$test() {   // 这个是实现定义在特质B中的B$$super$test的方法，它会调用顺序排在它前面的特质或类(D)的test方法
           super.test();
      }

      public void test() {
           C$class.test(this);

      }
      public E() {
            super();
            B$class.$init$(this);
            C$class.$init$(this);
      }
    }

引入堆叠特质的调用顺序是从右到左。相当于多个特质有一个相同的方法，它们可以从右往左一直嵌套执行。（利用super），而多重继承则有重复方法时不知道执行哪一个。

在任何的线性化中，某个类总是被线性化在其所有超类和混入特质之前。

    class Animal
    trait Furry extends Animal
    trait HasLegs extends Animal
    trait FourLegged extends HasLegs
    class Cat extends Animal with Furry with FourLegged

    // Animal线性化：Animal -> AnyRef -> Any
    // Furry线性化（Animal已经线性化的要排除在外）:Furry -> Animal -> AnyRef -> Any
    // FourLegged线性化： FourLegged -> HasLegs -> Animal -> AnyRef -> Any
    //  Cat线性化顺序为：Cat -> FourLegged -> HasLegs（此处会先处理超类）-> Furry -> Animal -> AnyRef -> Any
    // 当这些类和特质中的任何一个通过super调用了方法，那么被调用的实现将是它线性化的右侧的第一个实现。

invokevirtual掉用比invokeinterface性能好。特质调用的是invokeinterface


## 包和引用

    package a {
        package b {
            // 在a.b包中
            class A
            package c {
                // 在a.b.c包中
                class B
            }
        }
    }

    package a.b {
        class A
        package c {
            class B
        }
    }

    package a {
        package b {
            class A
        }

        package c {
            class B {
                val a = new b.A
            }
        }
    }
scala在所有用户可创建的包之外提供了名为_root_的包，任何能写出来的顶层包都被当做是_root_包的成员。
scala中import a.b._表示引入a.b包中所有的，替换java的*，因为*是scala的合法标识符
scala的import可以出现在任何地方，而不是仅仅在编译单元的开始处。它们可以指向任何值

    def showFruit(fruit : Fruit) {
        import fruit._                  // 这里引入了Fruit类的所有成员，把对象当成一个模块
        println(name + "s are " + color)
    }
scala可以引用包自身，而不只是除了包之外的其他成员。

    import java.util.regex
    class AStarB {
        val pat = regex.Pattern.compile("a*b")
    }

scala的引用与java的不同：
  * 可以出现在任何地方
  * 可以指向的是（单例或正统）对象及包。
  * 可以重命名或隐藏一些被引用的成员。

     import Fruits.{Apple, Orange} // 只使用Apple和Orange2个类
     // 引用Apple和Orange2个类，并且Apple重命名为McIntosh，这个对象可以用Fruits.Apple或McIntosh访问
     import Fruits.{Apple => McIntosh, Orange}
     import java.{sql => S}  // S.Date
     import Fruits.{_} // 引用Fruits所有成员，与import Fruits._一样
     import Fruits.{Apple => A, _} // 引用所有成员，Apple重命名为A
     import Fruits.{Pear => _, _} // 引用所有成员，Pear除外  <原始名> => _表示排除原始名引用

引用选择器：
   * 简单名x，把x包含进引用
   * 重命名子句x => y
   * 影藏子句x => _
   * 全包括_,全包括必须是引用选择的最后一个
import p._等价于import p.{_},  import p.n 等价于 import p.{n}

### 隐式引用

    // 这3个出现在靠后位置的引用将覆盖靠前德尔引用,如StringBuilder出现在java.lang和scala包中，但最终引入的是scala.StringBuilder
    import java.lang._
    import scala._
    import Predef._


### 访问修饰符
包，类或对象的成员都可以用访问修饰符private和protected做标记，

私有成员：标记为private的成员仅在包含了成员定义的类或对象内部可见

    class Outer {
        class Inner {
            private def f() {println("f")}
            class InnerMost {
                f()  // OK
            }
        }
        (new Inner).f() // 错误，f不可访问
    }
保护成员：保护成员只在定义了成员的类的子类中可以被访问，同一个包的其他类不可以访问

    package p {
        class Super {
            protected def f() {println("f")}
        }

        class Sub extends Super {
            f()
        }

        class Other {
            (new Super).f()  // 不可访问
        }
    }
公开成员：没有任何标记为private或protected的成员都是公开的。这样的成员在任何地方都可以被访问

### 保护的作用域：
Scala里的访问修饰符可以通过使用限定词强调。格式为private[X]或protected[X]的修饰符表示"直到"X的私有或保护，这里的X指代某个所属的包，类或单例对象。

    package bobsrockets {
        package navigation {
            // 说明Navigator对包含在bobsrockets包里的所有类和对象可见, 所有在bobsrockets包之外的代码都不能访问Navigator
            private[bobsrockets] class Navigator {
                // useStarChart能被Navigator所有子类及包含在navigation包里的所有代码访问，与java的protected一致
                protected[navigation] def useStarChart() {}
                class LegOfJourney {
                    // 表示distance对Navigator内的所有代码可见
                    private[Navigator] val distance = 100
                }
                // speed仅能在包含了定义的同一个对象中被访问，这中定义被称为对象私有，
                private[this] var speed = 100
            }
        }

        package launch {
            import navigation._
            object Vehicle {
                // guide自在launch包可见，相当于java的包私有访问
                private[launch] val guide = new Navigator
            }
        }
    }

类的所有访问权限都对伴生对象开放，反过来也是如此，对象可以访问所有它的伴生类的私有成员

## 断言和单元测试
assert 在jvm中通过-da -ea来关闭和打开
ensuring函数：通过隐式转换，可以对任意类型调用这个函数

scalaTest: Suite, FunSuite, Spec

    class ElementSuite extends Suite {
        def testUniformElement() {
            val ele = elem('x', 2, 3)
            assert(ele.width == 2)
        }
    }

    (new ElementSuite).execute()

    class ElementSuite extends FunSuite {
        // 使用柯里化和传名参数，参数是一个 => Unit
        test("elem result should have passed width") {
            val ele = elem('x', 2, 3)
            assert(ele.width == 2)
        }
    }

    // 判断花括号之间的代码是否返回2
    except(2) {
        ele.width
    }

    // 判断是否抛出IllegalArgumentException
    intercept(classOf[IllegalArgumentException]) {
        elem('x', -2, 3)
    }

    class ElementSpec extends Spec {
        describe("A UniformElement") {
            it("should have a width equal to the passed value") {
                val ele = elem('x', 2, 3)
                assert(ele.width == 2)
            }

            it("should have a height equal to the passed value") {
                val ele = elem('x', 2, 3)
                assert(ele.height == 2)
            }

            it("should throw an IAE if passed a negative width") {
                intercept(classOf[IllegalArgumentException]) {
                    val ele = elem('x', -2, 3)
                }
            }
        }
    }

JUnit：与java的一样,可以使用org.scalatest.junit.JUnit3Suite
TestNG：org.scalatest.testing.TestNGSuite
specs测试框架：http://code.google.com/p/specs

    object ElementSpecification extends Specification {
        "A UniformElement" should {
            "have a width equal to the passed value" in {
                val ele = elem('x', 2, 3)
                ele.width must be_==(2)
            }

            "have a height equal to the passed value" in {
                val ele = elem('x', 2, 3)
                ele.height must be_==(3)
            }

            "throw an IAE if passed a negative width in {
                elem('x', -2, 3) must
                    throwA(IllegalArgumntException)
            }
        }
    }

ScalaCheck：ScalaCheck属性被表示为用所需的测试数据做参数的函数值，这些所需的测试数据由ScalaCheck产生

    import org.scalatest.prop.FunSuite
    import org.scalacheck.Prop._
    import Element.elem

    class ElementSuite extends FunSuite {
        test("elem result should have passed width", (w : Int) => {
            // ==>是含义操作符，说明当左边的表达式为真时，那么右边的表达式也必须为真
            w > 0 ==> (elem('x', w, 3).width == w)
        }

        test("elem result should have passed height", (h : Int) => {
                    h > 0 ==> (elem('x', 2, h).height == h)
                }
        )
    }


    import org.scalatest.junit.JUnit3Suite
    import org.scalatest.prop.Checkers
    import org.scalacheck.Prop._
    import Element.elem

    class ElementSuite extends JUnit3Suite with Checkers {
        def testUniformElement() {
            check((w : Int) => w > 0 ==> (elem('x', w, 1).width == w))
            check((h : Int) => h > 0 ==> (elem('x', 2, h).height == h))
        }
    }

在ScalaTest中，可以通过在Suite内部嵌套Suite来管理较大的测试集。一个Suite在执行的时候会把内嵌的Suite当做它的测试执行。
可以手动或自动嵌套测试集，手动嵌套的话，要在Suite里重载nestedSuites方法。自动嵌套的话，准备一个与ScalaTest的Runner同名的包，保存需要自动发现的Suite，
把它们内嵌到根Suite下。然后执行根Suite

ScalaTest的Runner应用可以在命令行或ant任务中调用，必须指定要运行的测试集，可以显式说明测试集名称或说明想要Runner执行自动发现的测试集名称前缀。

    scala -cp scalatest-0.9.4.jar org.scalatest.tools.Runner -p "scalatest-0.9.4-tests.jar" -s org.scalatest.SuiteSuite

## 样本类和模式匹配
    // x，y相当于一般类的定义成val x : Int, val y : Int,它会生成x和y字段，并且生成它们的get，set方法
    // case class混入了scala.Product特质
    case class A(x : Int, y : Int)
    生成的java代码如下：
    public class A implements scala.Product,scala.Serializable {
      private final int x;

      private final int y;

      public static scala.Function1<scala.Tuple2<java.lang.Object, java.lang.Object>, A> tupled() {
            return A$.MODULE$.tupled();
      }

      public static scala.Function1<java.lang.Object, scala.Function1<java.lang.Object, A>> curried() {
            return A$.MODULE$.curried();
      }
      public int x() {
            return x;
      }
      public int y() {
            return y;
      }
      public A copy(int x, int y) {
            return new A(x, y);
      }

      public int copy$default$1() {
            return x();
      }

      public int copy$default$2() {
            return y();
      }
      public java.lang.String productPrefix() {
            return "A";
      }

      public int productArity() {
            return 2;
      }

      public java.lang.Object productElement(int x) {
            int temp = x;
            switch(temp) {
                case 0:
                    return scala.runtime.BoxesRunTime.boxToInteger(x());
                case 1:
                    return scala.runtime.BoxesRunTime.boxToInteger(y());
                default:
                    new java.lang.IndexOutOfBoundsException(scala.runtime.BoxesRunTime.boxToInteger(x).toString())
            }
      }
        Code:
           0: iload_1
           1: istore_2
           2: iload_2
           3: tableswitch   { // 0 to 1
                         0: 49
                         1: 39
                   default: 24
              }
          24: new           #55                 // class java/lang/IndexOutOfBoundsException
          27: dup
          28: iload_1
          29: invokestatic  #61                 // Method scala/runtime/BoxesRunTime.boxToInteger:(I)Ljava/lang/Integer;
          32: invokevirtual #64                 // Method java/lang/Object.toString:()Ljava/lang/String;
          35: invokespecial #67                 // Method java/lang/IndexOutOfBoundsException."<init>":(Ljava/lang/String;)V
          38: athrow
          39: aload_0
          40: invokevirtual #47                 // Method y:()I
          43: invokestatic  #61                 // Method scala/runtime/BoxesRunTime.boxToInteger:(I)Ljava/lang/Integer;
          46: goto          56
          49: aload_0
          50: invokevirtual #44                 // Method x:()I
          53: invokestatic  #61                 // Method scala/runtime/BoxesRunTime.boxToInteger:(I)Ljava/lang/Integer;
          56: areturn

      public scala.collection.Iterator<java.lang.Object> productIterator() {
            return scala.runtime.ScalaRunTime$.MODULE$.typedProductIterator(this);
      }
      public boolean canEqual(java.lang.Object object) {
            return object instanceOf A;
      }

      public int hashCode() {
           int i = -889275714;
           i = scala.runtime.Statics.mix(i, x());
           i = scala.runtime.Statics.mix(i, y());
           return scala.runtime.Statics.finalizeHash(i, 2);
      }

      public java.lang.String toString() {
            return  scala.runtime.ScalaRunTime$.MODULE$._toString(this);
      }

      public boolean equals(java.lang.Object object) {
            if (this == object) {
                return true;
            }

            Object o = object;   // astore_2
            if (o instanceOf A) {
                A a = (A) object //astore 4
                if (this.x() == a.x() && this.y() == a.y() && a.canEqual(this)) {
                    return true;
                }
            }

            return false;
      }
        Code:
           0: aload_0
           1: aload_1
           2: if_acmpeq     72
           5: aload_1
           6: astore_2
           7: aload_2
           8: instanceof    #2                  // class A
          11: ifeq          19
          14: iconst_1
          15: istore_3
          16: goto          21
          19: iconst_0
          20: istore_3
          21: iload_3
          22: ifeq          76
          25: aload_1
          26: checkcast     #2                  // class A
          29: astore        4
          31: aload_0
          32: invokevirtual #44                 // Method x:()I
          35: aload         4
          37: invokevirtual #44                 // Method x:()I
          40: if_icmpne     68
          43: aload_0
          44: invokevirtual #47                 // Method y:()I
          47: aload         4
          49: invokevirtual #47                 // Method y:()I
          52: if_icmpne     68
          55: aload         4
          57: aload_0
          58: invokevirtual #102                // Method canEqual:(Ljava/lang/Object;)Z
          61: ifeq          68
          64: iconst_1
          65: goto          69
          68: iconst_0
          69: ifeq          76
          72: iconst_1
          73: goto          77
          76: iconst_0
          77: ireturn

      public A(int x, int y) {
           this.x = x;
           this.y = y;
           super();
           scala.Product$class.$init$(this);
      }
    }


    public final class A$ extends scala.runtime.AbstractFunction2<java.lang.Object, java.lang.Object, A> implements scala.Serializable {
      public static final A$ MODULE$ = new A$();

      public final java.lang.String toString() {
            return "A";
      }

      public A apply(int x, int y) {
            return new A(x, y);
      }
      public scala.Option<scala.Tuple2<java.lang.Object, java.lang.Object>> unapply(A a) {
            if (a != null) {
                return new scala.Some(new scala.Tuple2$mcII$sp()(a.x(), a.y()));
            }

            return scala.None$.MODULE$;
      }
        Code:
           0: aload_1
           1: ifnonnull     10
           4: getstatic     #36                 // Field scala/None$.MODULE$:Lscala/None$;
           7: goto          32
          10: new           #38                 // class scala/Some
          13: dup
          14: new           #40                 // class scala/Tuple2$mcII$sp
          17: dup
          18: aload_1
          19: invokevirtual #43                 // Method A.x:()I
          22: aload_1
          23: invokevirtual #45                 // Method A.y:()I
          26: invokespecial #46                 // Method scala/Tuple2$mcII$sp."<init>":(II)V
          29: invokespecial #49                 // Method scala/Some."<init>":(Ljava/lang/Object;)V
          32: areturn

      private java.lang.Object readResolve() {
            return MODULE$;
      }

      public java.lang.Object apply(java.lang.Object x, java.lang.Object y) {
           return apply(
                   scala.runtime.BoxesRunTime.unboxToInt(x),
                   scala.runtime.BoxesRunTime.unboxToInt(y)
           )
      }

      private A$() {
            super();
            this.MODULE$ = this;
      }
    }

特点
  * case class会添加与类名一致的工厂方法，可以用Var("x")来构造Var对象以替代new Var("x")，只是在编译器上判断了可以省略new关键字，并没有调用apply方法
  * case class会添加它的字段，get，set方法，如上例的x和y
  * 编译器添加了toString，hashCode,equals的自然实现


### 模式匹配
选择器 match {备选项}  备选项开始于case关键字，=>隔开了模式和表达式
类似于"+"或1这样的常量模式匹配的值等于用==判断相等的常量，类似于e这样的变量模式匹配所有的值，通配模式（_)同样也匹配所有值，不过没有引入指向那个值的变量名。
构造器模式类似于UnOp("-", e)。这种模式匹配所有类型为UnOp，并且第一个参数匹配"-",第2个参数匹配e的值。

    def simplifyTop(expr : Expr) : Expr = expr match {
        case UnOp("-"， UnOp("-", e)) => e
        case BinOp("+", e, Number(0)) => e
        case BinOp("*", e, Number(1))  => e
        case _ => expr
    }

match于switch的区别：
  * match是scala的表达式，它始终以值作为结果
  * 自动加break
  * 如果没有模式匹配，MatchError会被抛出

### 模式的种类
  * 通配模式：_，匹配任意对象
  * 常量模式：匹配自身，任何字面量都可以用作常量，任何的val或单例对象也可以被用作常量，如单例对象Nil是只匹配空列表的模式
  * 变量模式

        import Math.{E, Pi}
        E match {
            case Pi => "strange math? Pi = " + Pi
            case _ => "OK"
        }                  // "OK"
    scala编译器如何知道Pi是从java.lang.Math对象引入的常量，而不是代表选择器自身的变量？scala使用了一个简单的文字规则，用小写字母开始的简单名被当做是模式变量，
    所有其他的引用被认为是常量

    以下方法可以给模式常量使用小写字母名：
       * 如果常量是某个对象的字段，可以在其之上用限定符前缀，如this.pi或obj.pi
       * 用反引号包住变量，例如`pi`会被解释为常量，而不是变量
  * 构造器模式：样本类，能深层检查
  * 序列模式：

        expr match {
            case List(0, _, _) => println("found it!")
            case _ =>
        }
        // 如果想要匹配一个不指定长度的序列，可以指定_*作为模式的最后元素，这种模式匹配序列中零到任意数量的元素
        expr match {
            case List(0, _*) => println("found it!")
            case _ =>
        }
  * 元组模式

        def tupleDemo(expr : Any) =
            expr match {
                case (a, b, c) => println("matched " + a + b + c)
                case _ =>
            }
  * 类型模式：当做类型测试和类型转换的简易替换

        def generalSize(x : Any) = x match {
            case s : String => s.length
            case m : Map[_, _] => m.size
            case _ => -1
        }
测试expr是String类型的： expr.isInstanceOf[String], 要转换为String类型写成：expr.asInstanceOf[String]

        if (x.isInstanceOf[String]) {
            val s = x.asInstanceOf[String]
            s.length
        } else ...

不能使用模式匹配来匹配特定的元素类型，因为通过类型擦除，运行期没有保存类型信息，调用scala -unchecked

        // 运行scala -unchecked
        // 以下方法会得到警告：non variable type-=argument Int in type pattern is unchecked since it is eliminated by erasure
        def isIntIntMap(x : Any) = x match {
            case m : Map[Int][Int] => true
            case _ => false
        }

        isIntIntMap(Map(1 -> 1) // true
        isIntIntMap(Map("abc" => "abc")  // true
数组不会被擦除，它被特殊处理了，因此它可以作为模式匹配：

        def isStringArray(x : Any) = x match {
            case a : Array[String, String] => "yes"
            case _ => "no"
        }

变量绑定：写上变量名，一个@符号，以及这个模式。

        expr match {
            case UnOp("abs" e @ UnOp("abs", _)) => e // 这个e代表了UnOp("abs", _)整个模式
        }

模式守卫：
scala的模式要求是线性的，模式变量仅允许在模式中出现一次，如case BinOp("+", x, x)就会出错

        def simplifyAdd(e : Expr) = e match {
            // 这里只能用x和y，不能用x和x
            case BinOp("+", x, y) if x == y => BinOp("*", x, Number(2))
            case _ => e
        }
模式守卫接在模式之后，开始于if，守卫可以是任意的引用模式中变量的布尔表达式。

        // 仅匹配正整数
        case n : Int if 0 < n => ...
        // 仅匹配以字母'a'开始的字符串
        case s : String if s(0) == 'a' => ...

全匹配模式一般放在最后面

### 封闭类：
封闭类除了类定义所在的文件之外不能再添加任何新的子类。只要把关键字sealed放在最顶层类的前边即可
如果定义了封闭类，在写模式匹配的时候丢失了若干可能样本的模式匹配，并且没有写通配符，则编译器会发出警告：提示哪些类还没有被列出
如果确定不会产生其他的类，则可以在选择表达式上加上@unchecked注解，这样编译器对于随后的模式的穷举性检查将会被抑制掉。

        // 给e定义了@unchecked注解，后面就不会检查还有其他的模式
        def describe(e : Expr) : String = (e : @unchecked) match {
            case Number(_) => "a number"
            case Var(_) => "a variable"
        }

### Option类型
scala为可选值定义了一个名为Option的标准类型，这种值可以有两种形式，可以是Some(x)的形式，其中x是实际值，或者也可以是None对象，代表缺失值。
scala的集合类的某些标准操作会产生可选值，例如scala的Map的get方法会在发现了指定键的情况下产生Some(value)，在没有找到指定键的时候产生None

    val capitals = Map("France" -> "Paris", "Japan" -> "Tokyo")
    capitals get "France"  // Some(Paris)
    capitals get "North Fole" // None
分离可选值的最通常方法是通过模式匹配：

    def show(x : Option[String]) = x match {
        case Some(s) => s
        case None => "?"
    }

## 模式无处不在
### 模式在变量定义中

    val myTuple = (123, "abc")
    val (number, string) = myTuple  // number = 123, string = abc
    生成的java代码如下：
    public class P {
      private final scala.Tuple2<java.lang.Object, java.lang.String> myTuple;
      private final scala.Tuple2<java.lang.Object, java.lang.String> x$1;
      private final int number;
      private final java.lang.String string;
      public scala.Tuple2<java.lang.Object, java.lang.String> myTuple() {
            return myTuple;
      }

      public int number() {
            return number;
      }
      public java.lang.String string() {
            return string;
      }

      public P() {
            super();
            // Tuple2是case class，所以可以直接省略new
            this.myTuple = new Tuple2(scala.runtime.BoxesRunTime.boxToInteger(123),"abc");
            scala.Tuple2 tuple = myTuple();
            if (tuple == null) {
                throw new scala.MatchError(tuple);
            }

            // 使用的是模式匹配：case Tuple2(number, string) => number = number; string = string
            int first = tuple._1$mcI$sp();
            int second = (String) tuple._2();
            this.x$1 = new Tuple2(scala.runtime.BoxesRunTime.boxToInteger(first), second);
            this.number = x$1._1$mcI$sp();
            this.string = (String) x$1._2();
      }
    }

### 用作偏函数样本序列：
花括号内的样本序列可以用在能够出现函数字面量的任何地方。实质上，样本序列就是函数字面量。
函数字面量只有一个入口点和参数列表，样本序列可以有多个入口点，每个都有自己的参数列表，每个样本都是函数的一个入口点，参数也被模式化所转化。
每个入口点的函数体都在样本的右侧。样本序列会生成一个scala.Function1<scala.Option<java.lang.Object>, java.lang.Object>的子类，这个类有个apply方法

    // 这个定义会生成一个偏函数M$$anonfun$1，withDefault是这个类型，它是scala.Function1<scala.Option<java.lang.Object>,
    // java.lang.Object>的子类，有个apply方法
    val withDefault : Option[Int] => Int = {
        case Some(x) => x
        case None => 0
    }

    生成的java代码如下：
    public class M {
      private final scala.Function1<scala.Option<java.lang.Object>, java.lang.Object> withDefault;

      public scala.Function1<scala.Option<java.lang.Object>, java.lang.Object> withDefault() {
            return withDefault;
      }
      public M() {
            super();
            this.withDefault = new M$$anonfun$1(this);
      }
    }

    public final class M$$anonfun$1 extends scala.runtime.AbstractFunction1<scala.Option<java.lang.Object>, java.lang.Object> implements scala.Serializable {
      public static final long serialVersionUID;

      public final int apply(scala.Option<java.lang.Object> option) {
           scala.Option temp = option;
           if (temp instanceOf scala.Some) {
                return scala.runtime.BoxesRunTime.unboxToInt(((scala.Some) temp).x())
           } else {
                scala.None$.MODULE$
           }
           // ...
      }
        Code:
           0: aload_1
           1: astore_2
           2: aload_2
           3: instanceof    #21                 // class scala/Some
           6: ifeq          30
           9: aload_2
          10: checkcast     #21                 // class scala/Some
          13: astore_3
          14: aload_3
          15: invokevirtual #25                 // Method scala/Some.x:()Ljava/lang/Object;
          18: invokestatic  #31                 // Method scala/runtime/BoxesRunTime.unboxToInt:(Ljava/lang/Object;)I
          21: istore        4
          23: iload         4
          25: istore        5
          27: goto          60
          30: getstatic     #37                 // Field scala/None$.MODULE$:Lscala/None$;
          33: aload_2
          34: astore        6
          36: dup
          37: ifnonnull     49
          40: pop
          41: aload         6
          43: ifnull        57
          46: goto          63
          49: aload         6
          51: invokevirtual #43                 // Method java/lang/Object.equals:(Ljava/lang/Object;)Z
          54: ifeq          63
          57: iconst_0
          58: istore        5
          60: iload         5
          62: ireturn
          63: new           #45                 // class scala/MatchError
          66: dup
          67: aload_2
          68: invokespecial #48                 // Method scala/MatchError."<init>":(Ljava/lang/Object;)V
          71: athrow

      public final java.lang.Object apply(java.lang.Object object) {
            return scala.runtime.BoxesRunTime.boxToInteger(apply((scala.Option) object));
      }
      public M$$anonfun$1(M) {
            super();
      }


      react {
            case (name : String, actor : Actor) => {
                actor ! getip(name)
                act()
            }
            case msg => {
                println("Unhandled message: " + msg)
                act
            }
      }

样本序列可以用作这样的偏函数，如果把函数应用在它不支持的值上，会产生一个运行期异常：

    // 编译它，编译器会正确地提示说匹配并不全面：warning: match is not exhaustive! missing combination Nil
    val second = List[Int] => Int = {
        case x :: y :: _ => y
    }
    // 传递一个三元素的列表不会出现问题，如果传给它一个空列表就会报MatchError

如果想要检查是否一个偏函数有定义，必须首先告诉编译器知道正在使用的是偏函数，类型List[Int] => Int包含了不管是否为偏函数的，从整数列表到整数的所有函数。
仅包含从整数列表到整数的偏函数的，应该写成PartialFunction[List[Int], Int]

    // 使用偏函数类型
    val second = PartialFunction[List[Int], Int] = {
        case x :: y :: _ => y
    }

    // 偏函数有一个isDefinedAt方法，用来测试是否函数对某个特定值有定义
    second.isDefinedAt(List(5, 6, 7))  // true
    second.isDefinedAt(List()) // false

    // scala编译器把这样的表达式转译成偏函数的时候，会对模式执行两次翻译：其中一次是真实函数的实现，另一次是测试函数是否（对特定参数）有定义的实现，
    // 上面的second会被翻译成如下代码：
    new PartialFunction[List[Int], Int] {
        def apply(xs : List[Int]) = xs match {
            case x :: y :: _ => y
        }

        def isDefinedAt(xs : List[Int]) = xs match {
            case x :: y :: _ => true
            case _ => false
        }
    }

    // 这种翻译只有在函数字面量的申明类型为PartialFunction的时候才会生效，如果申明类型只是Function1或根本不存在，那么函数字面量就会代而转译为完整的函数。

### for表达式的模式

    // capitals是一个map
    for ((country, city) <- capitals) println("the capital of " + country + " is " + city)

    // 不能匹配模式的值被丢弃
    val results = List(Some("apple"), None, Some("orange"))
    for (Some(fruit) <- results) println(fruit)  // apple, orange

## 使用列表
列表是不可变的，列表具有递归结构（如：链接列表：linked list)，数组是连续的

### List类型：
和数组一样，列表是同质的（homogeneous)，列表的所有元素都具有相同的类型，元素类型为T的列表类型写成List[T]:
列表是协变的（covariant)，对任意类型T的List[T]来说，List[Nothing]都是其子类。

    // List()是List[Nothing]的，它是List[String]的子类，所以以下的语句是成立的
    val x : List[String] = List()

### 构造列表：
所有的列表都是由两个基础构造块Nil和::(发音为cons)构造出来的，Nil代表空列表，中缀操作符::，表示列表从前端扩展

    List(1, 2, 3)创建了列表1 :: 2 :: 3 :: Nil

### 列表的基本操作
  * head：返回列表的第一个元素
  * tail：返回除第一个之外所有元素组成的列表
  * isEmpty：如果列表为空，则返回true
head和tail方法仅能够作用在非空列表上，如果执行在空列表上，会抛出异常。

    Nil.head  // java.util.NoSuchElementException : head of empty list

case object可以直接用这个object名就表示这个对象，如Nil，就是case object

    case object C
    class B {
          val c = C
          val s = C.toString()
    }
    生成的java代码如下：
    public final class C {
      public static java.lang.String toString() {
            return C$.MODULE$.toString();
      }
      public static int hashCode() {
            return C$.MODULE$.hashCode();
      }
      public static boolean canEqual(java.lang.Object object) {
            return C$.MODULE$.canEqual(object);
      }
      public static scala.collection.Iterator<java.lang.Object> productIterator() {
            return C$.MODULE$.productIterator();
      }
      public static java.lang.Object productElement(int x) {
            return C$.MODULE$.productElement(x);
      }
      public static int productArity() {
            return C$.MODULE$.productArity();
      }
      public static java.lang.String productPrefix() {
            return C$.MODULE$.productPrefix();
      }
    }

    public final class C$ implements scala.Product,scala.Serializable {
      public static final C$ MODULE$ = new C$();

      public java.lang.String productPrefix() {
            return "C";
      }
      public int productArity() {
            return 0;
      }
      public java.lang.Object productElement(int index) {
            return new java/lang/IndexOutOfBoundsException(scala.runtime.BoxesRunTime.boxToInteger(index).toString());
      }
        Code:
           0: iload_1
           1: istore_2
           2: new           #27                 // class java/lang/IndexOutOfBoundsException
           5: dup
           6: iload_1
           7: invokestatic  #33                 // Method scala/runtime/BoxesRunTime.boxToInteger:(I)Ljava/lang/Integer;
          10: invokevirtual #36                 // Method java/lang/Object.toString:()Ljava/lang/String;
          13: invokespecial #39                 // Method java/lang/IndexOutOfBoundsException."<init>":(Ljava/lang/String;)V
          16: athrow

      public scala.collection.Iterator<java.lang.Object> productIterator() {
            return scala.runtime.ScalaRunTime$.MODULE$.typedProductIterator(this);
      }
      public boolean canEqual(java.lang.Object object) {
            return object instanceOf C$;
      }
      public int hashCode() {
            return 67;
      }
      public java.lang.String toString() {
            return "C";
      }
      private java.lang.Object readResolve() {
            return MODULE$;
      }
      private C$() {
            super();
            this.MODULE$ = this;
            Product$class.$init$(this);
      }
    }

    public class B {
      private final C$ c;
      private final java.lang.String s;

      public C$ c() {
            return this.c;
      }

      public java.lang.String s() {
            return this.s;
      }
      public B() {
            super();
            this.c = C$.MODULE$;
            this.s = C$.MODULE$.toString();
      }
    }

    // 插入排序
    def isort(xs : List[Int]) : List[Int] =
        if (xs.isEmpty) Nil
        else insert(xs.head, isort(xs.tail))
    def insert(x : Int, xs : List[Int]) : List[Int] =
        if (xs.isEmpty || x <= xs.head) x :: xs
        else xs.head :: insert(x, xs.tail)

###　列表模式：

    val List(a, b, c) = fruit

    class L {
        val list = List("a", "b", "c")
        val List(a, b, c) = list
        val x :: y :: rest = list
    }

    生成的java文件为：
    public class L {
      private final scala.collection.immutable.List<java.lang.String> list;

      private final scala.Tuple3<java.lang.String, java.lang.String, java.lang.String> x$1;

      private final java.lang.String a;

      private final java.lang.String b;

      private final java.lang.String c;

      private final scala.Tuple3<java.lang.String, java.lang.String, scala.collection.immutable.List<java.lang.String>> x$2;

      private final java.lang.String x;

      private final java.lang.String y;

      private final scala.collection.immutable.List<java.lang.String> rest;

      public scala.collection.immutable.List<java.lang.String> list();
        Code:
           0: aload_0
           1: getfield      #26                 // Field list:Lscala/collection/immutable/List;
           4: areturn

      public java.lang.String a();
        Code:
           0: aload_0
           1: getfield      #31                 // Field a:Ljava/lang/String;
           4: areturn

      public java.lang.String b();
        Code:
           0: aload_0
           1: getfield      #33                 // Field b:Ljava/lang/String;
           4: areturn

      public java.lang.String c();
        Code:
           0: aload_0
           1: getfield      #35                 // Field c:Ljava/lang/String;
           4: areturn

      public java.lang.String x();
        Code:
           0: aload_0
           1: getfield      #37                 // Field x:Ljava/lang/String;
           4: areturn

      public java.lang.String y();
        Code:
           0: aload_0
           1: getfield      #39                 // Field y:Ljava/lang/String;
           4: areturn

      public scala.collection.immutable.List<java.lang.String> rest();
        Code:
           0: aload_0
           1: getfield      #41                 // Field rest:Lscala/collection/immutable/List;
           4: areturn

      public L();
        Code:
           0: aload_0
           1: invokespecial #45                 // Method java/lang/Object."<init>":()V
           4: aload_0
           5: getstatic     #51                 // Field scala/collection/immutable/List$.MODULE$:Lscala/collection/immutable/List$;
           8: getstatic     #56                 // Field scala/Predef$.MODULE$:Lscala/Predef$;
          11: iconst_3
          12: anewarray     #58                 // class java/lang/String
          15: dup
          16: iconst_0
          17: ldc           #59                 // String a
          19: aastore
          20: dup
          21: iconst_1
          22: ldc           #60                 // String b
          24: aastore
          25: dup
          26: iconst_2
          27: ldc           #61                 // String c
          29: aastore
          30: checkcast     #63                 // class "[Ljava/lang/Object;"
          33: invokevirtual #67                 // Method scala/Predef$.wrapRefArray:([Ljava/lang/Object;)Lscala/collection/mutable/WrappedArray;
          36: invokevirtual #71                 // Method scala/collection/immutable/List$.apply:(Lscala/collection/Seq;)Lscala/collection/immutable/List;
          39: putfield      #26                 // Field list:Lscala/collection/immutable/List;
          42: aload_0
          43: aload_0
          44: invokevirtual #73                 // Method list:()Lscala/collection/immutable/List;
          47: astore_1
          48: getstatic     #51                 // Field scala/collection/immutable/List$.MODULE$:Lscala/collection/immutable/List$;
          51: aload_1
          52: invokevirtual #77                 // Method scala/collection/immutable/List$.unapplySeq:(Lscala/collection/Seq;)Lscala/Some;
          55: astore_2
          56: aload_2
          57: invokevirtual #83                 // Method scala/Option.isEmpty:()Z
          60: ifne          345
          63: aload_2
          64: invokevirtual #87                 // Method scala/Option.get:()Ljava/lang/Object;
          67: ifnull        345
          70: aload_2
          71: invokevirtual #87                 // Method scala/Option.get:()Ljava/lang/Object;
          74: checkcast     #89                 // class scala/collection/LinearSeqOptimized
          77: iconst_3
          78: invokeinterface #93,  2           // InterfaceMethod scala/collection/LinearSeqOptimized.lengthCompare:(I)I
          83: iconst_0
          84: if_icmpne     345
          87: aload_2
          88: invokevirtual #87                 // Method scala/Option.get:()Ljava/lang/Object;
          91: checkcast     #89                 // class scala/collection/LinearSeqOptimized
          94: iconst_0
          95: invokeinterface #96,  2           // InterfaceMethod scala/collection/LinearSeqOptimized.apply:(I)Ljava/lang/Object;
         100: checkcast     #58                 // class java/lang/String
         103: astore_3
         104: aload_2
         105: invokevirtual #87                 // Method scala/Option.get:()Ljava/lang/Object;
         108: checkcast     #89                 // class scala/collection/LinearSeqOptimized
         111: iconst_1
         112: invokeinterface #96,  2           // InterfaceMethod scala/collection/LinearSeqOptimized.apply:(I)Ljava/lang/Object;
         117: checkcast     #58                 // class java/lang/String
         120: astore        4
         122: aload_2
         123: invokevirtual #87                 // Method scala/Option.get:()Ljava/lang/Object;
         126: checkcast     #89                 // class scala/collection/LinearSeqOptimized
         129: iconst_2
         130: invokeinterface #96,  2           // InterfaceMethod scala/collection/LinearSeqOptimized.apply:(I)Ljava/lang/Object;
         135: checkcast     #58                 // class java/lang/String
         138: astore        5
         140: new           #98                 // class scala/Tuple3
         143: dup
         144: aload_3
         145: aload         4
         147: aload         5
         149: invokespecial #101                // Method scala/Tuple3."<init>":(Ljava/lang/Object;Ljava/lang/Object;Ljava/lang/Object;)V
         152: astore        6
         154: aload         6
         156: putfield      #103                // Field x$1:Lscala/Tuple3;
         159: aload_0
         160: aload_0
         161: getfield      #103                // Field x$1:Lscala/Tuple3;
         164: invokevirtual #106                // Method scala/Tuple3._1:()Ljava/lang/Object;
         167: checkcast     #58                 // class java/lang/String
         170: putfield      #31                 // Field a:Ljava/lang/String;
         173: aload_0
         174: aload_0
         175: getfield      #103                // Field x$1:Lscala/Tuple3;
         178: invokevirtual #109                // Method scala/Tuple3._2:()Ljava/lang/Object;
         181: checkcast     #58                 // class java/lang/String
         184: putfield      #33                 // Field b:Ljava/lang/String;
         187: aload_0
         188: aload_0
         189: getfield      #103                // Field x$1:Lscala/Tuple3;
         192: invokevirtual #112                // Method scala/Tuple3._3:()Ljava/lang/Object;
         195: checkcast     #58                 // class java/lang/String
         198: putfield      #35                 // Field c:Ljava/lang/String;
         201: aload_0
         202: aload_0
         203: invokevirtual #73                 // Method list:()Lscala/collection/immutable/List;
         206: astore        7
         208: aload         7
         210: instanceof    #114                // class scala/collection/immutable/$colon$colon
         213: ifeq          335
         216: aload         7
         218: checkcast     #114                // class scala/collection/immutable/$colon$colon
         221: astore        8
         223: aload         8
         225: invokevirtual #117                // Method scala/collection/immutable/$colon$colon.hd$1:()Ljava/lang/Object;
         228: checkcast     #58                 // class java/lang/String
         231: astore        9
         233: aload         8
         235: invokevirtual #120                // Method scala/collection/immutable/$colon$colon.tl$1:()Lscala/collection/immutable/List;
         238: astore        10
         240: aload         10
         242: instanceof    #114                // class scala/collection/immutable/$colon$colon
         245: ifeq          335
         248: aload         10
         250: checkcast     #114                // class scala/collection/immutable/$colon$colon
         253: astore        11
         255: aload         11
         257: invokevirtual #117                // Method scala/collection/immutable/$colon$colon.hd$1:()Ljava/lang/Object;
         260: checkcast     #58                 // class java/lang/String
         263: astore        12
         265: aload         11
         267: invokevirtual #120                // Method scala/collection/immutable/$colon$colon.tl$1:()Lscala/collection/immutable/List;
         270: astore        13
         272: new           #98                 // class scala/Tuple3
         275: dup
         276: aload         9
         278: aload         12
         280: aload         13
         282: invokespecial #101                // Method scala/Tuple3."<init>":(Ljava/lang/Object;Ljava/lang/Object;Ljava/lang/Object;)V
         285: astore        14
         287: aload         14
         289: putfield      #122                // Field x$2:Lscala/Tuple3;
         292: aload_0
         293: aload_0
         294: getfield      #122                // Field x$2:Lscala/Tuple3;
         297: invokevirtual #106                // Method scala/Tuple3._1:()Ljava/lang/Object;
         300: checkcast     #58                 // class java/lang/String
         303: putfield      #37                 // Field x:Ljava/lang/String;
         306: aload_0
         307: aload_0
         308: getfield      #122                // Field x$2:Lscala/Tuple3;
         311: invokevirtual #109                // Method scala/Tuple3._2:()Ljava/lang/Object;
         314: checkcast     #58                 // class java/lang/String
         317: putfield      #39                 // Field y:Ljava/lang/String;
         320: aload_0
         321: aload_0
         322: getfield      #122                // Field x$2:Lscala/Tuple3;
         325: invokevirtual #112                // Method scala/Tuple3._3:()Ljava/lang/Object;
         328: checkcast     #124                // class scala/collection/immutable/List
         331: putfield      #41                 // Field rest:Lscala/collection/immutable/List;
         334: return
         335: new           #126                // class scala/MatchError
         338: dup
         339: aload         7
         341: invokespecial #129                // Method scala/MatchError."<init>":(Ljava/lang/Object;)V
         344: athrow
         345: new           #126                // class scala/MatchError
         348: dup
         349: aload_1
         350: invokespecial #129                // Method scala/MatchError."<init>":(Ljava/lang/Object;)V
         353: athrow
    }

List(...)是由开发库定义的抽取器(extractor)模式的实例。“cons”模式x :: xs是中缀操作符模式的特例，如果被看做是表达式，那么中缀操作与方法调用等价。
但对于模式来说，如果被当作模式，那么类似p op q这样的中缀操作符等价于op(p, q)，中缀操作符op被当做构造器模式。在x :: xs中被看作::(x, xs)，::的全称是
scala.::，它是可以创建非空列表的类，List中的::方法目的是实例化scala.::的对象。

    // 模式匹配的插入排序
    def isort(xs : List[Int]) : List[Int] = xs match {
        case List() => List()
        case x :: xsl => insert(x, isort(xsl))
    }

    def insert(x : Int, xs : List[Int]) : List[Int] = xs match {
        case List() => List(x)
        case y :: ys => if (x <= y) x :: xs
                        else y :: insert(x, ys)
    }

### List类的一阶方法
一阶方法是指不以函数做入参的方法。

连接列表：:::，它的两个操作数都是列表

    List(1, 2) ::: List(3, 4, 5)   // List(1, 2, 3, 4, 5)

    // ::: 方法用模式匹配实现
    def append[T](xs : List[T], ys : List[T]) = xs match {
        case List() => ys
        case x :: xsl => x :: append(xsl, ys)
    }

计算列表的长度：length方法，这个方法是循环调用列表的元素的，因此是跟列表长度成正比，而isEmpty是循环列表，判断到Iterator的hasNext就为false

    List(1, 2, 3).length // 3
访问列表尾部：init方法和last方法：head和tail运行的时间是常量，init和last需要遍历列表，时间与列表的长度成正比。

    val abcde = List('a', 'b', 'c', 'd', 'e')
    abcde.last   // 'e'
    abcde.init   // List('b', 'c', 'd', 'e')
    // 对空列表调用last和init会抛出UnsupportedOperationException

head：第一个元素
last：最后一个元素
tail：除了第一个元素的剩余元素
init：除了最后一个的剩余元素

反转列表：reverse方法：如果出于某种原因，某种算法需要频繁访问列表的尾部，那么可以首先把列表反转过来，然后再处理。

  * reverse是其自身的反转xs.reverse.reverse 等价于 xs
  * reverse把init转为tail，把last转为head，除了其中的元素是反的：

        xs.reverse.init  等价于  xs.tail.reverse
        xs.reverse.tail  等价于  xs.init.reverse
        xs.reverse.head  等价于  xs.last
        xs.reverse.last  等价于  xs.head

        // 反转的实现,效率比较低，因为:::方法要遍历列表，它的复杂度为：n + (n-1) + ... + 1 = (1 + n) *n/2
        def rev[T](xs : List[T]) : List[T] = xs match {
            case List() => xs
            case xs :: xsl => rev(xsl) ::: List(x)
        }
前缀与后缀：drop, take和spitAt
xs take n 返回xs列表的前n个元素，如果n大于xs.length，则返回整个xs
xs drop n 返回xs列表除了前n个元素之外的所有元素，如果n大于xs.length，则返回空列表。
spitAt操作在指定位置拆分列表，并返回对偶列表（Tuple2)

    xs spitAt n 等价于 (xs.take n, xs drop n) // spitAt避免了对列表xs的二次遍历。

元素选择:apply方法和indices方法
apply方法实现了随机元素的选择，一般用数组中的同名方法，这个方法的效率与n成正比，xs apply n 等价于 (xs drop n).head

    abcde apply 2  // c
    abcde(2)  // 对象作为方法调用在函数位置时，直接调用apply方法
indices方法可以返回指定列表的所有有效索引值组成的列表

zip函数：可以把两个列表组成一个对偶列表：

    abcde.indices zip abcde  // List((0,a), (1,b), (2,c), (3,d), (4,e))
如果两个列表的长度不一致，那么任何不能匹配的元素将被丢掉

zipWithIndex方法更为高效，它能把列表的每个元素与元素在列表中的位置值组成一对。

    abcde.zipWithIndex  // List((a,0), (b,1), (c,2), (d,3), (e,4))

显示列表：toString方法和mkString方法,addString方法

    val buf = new StringBuilder
    abcde addString (buf, "(", ";", ")")

转换列表：iterator, elements, toArray, copyToArray,Array.toList

    // 归并排序
    def msort(less : (T, T) => Boolean)(xs : List[T]) : List[T] = {
        def merge(xs : List[T], ys : List[T]) : List[T] = (xs, ys) match {
            case (Nil, _) => ys
            case (_, Nil) => xs
            case (x :: xsl, y :: ysl) =>
                if (less(x, y)) x :: merge(xsl, ys)
                else y :: merge(xs, ysl)
        }

        val n = xs.length / 2
        if (n == 0) xs
        else {
            val (ys, zs) = xs splitAt n
            merge(msort(less)(ys), msort(less)(zs))
        }
    }

### List类的高阶方法
高阶方法：高阶函数是以其他函数作为参数的函数。
#### 列表间映射：map, flatMap和foreach
flatMap的右操作元是能够返回元素列表的函数，它对列表的每个元素调用该方法，然后连接所有方法的结果并返回

    // List.range(a, b)包含a，但不包含b
    List.range(1, 5) flatMap (
        i => List.range(1, i) map (j => (i, j))
    )
    // List((2, 1), (3, 1), (3, 2), (4, 1), (4, 2), (4, 3))
    for (i <- List.range(1, 5); j <- List.range(1, i)) yield (i, j)
foreach的右操作元是过程（返回类型为Unit的函数）。操作的结果仍是Unit不会产生结果列表。
    var sum = 0
    List(1, 2, 3, 4, 5) foreach (sum += _)

#### 列表过滤：filter, partition, find, takeWhile, dropWhile和span
xs filter p操作把类型为List[T]的列表xs和类型为T => Boolean的论断函数作为操作元，产生xs中符合p(x)为true的所有元素x组成的列表
partition方法与filter类似，不过返回的是列表对，其中一个包含所有论断为真的元素，另一个包含所有论断为假的元素。等价于

    xs partition p 等价于 (xs filter p, xs filter (!p(_))

find方法同样于filter类似，不过返回的是第一个满足给定论断的元素，而非全部。xs find p操作返回可选值。如果xs中存在元素x使得p(x)为真，Some(x)将返回，
否则，若p对所有元素都不成立，None将返回.
xs takeWhile p操作返回列表xs中最长的能够满足p的前缀，xs dropWhile p操作移除最长的能够满足p的前缀。
span方法把takeWhile和dropWhile组成一个操作，就像splitAt。 xs span p 等价于 (xs takeWhile p, xs dropWhile p)

    List(1, 2, 3, -4, 5) span (_ > 0)  // (List(1, 2, 3), List(-4, 5))
#### 列表的论断：forall，exists
xs forall p：如果列表的所有元素满足p则返回true
xs exists p：xs中只要有一个值满足论断p就返回true

#### 折叠列表：/:和:\

    sum(List(a, b, c))  等价于  0 + a + b + c
    def sum(xs : List[Int]) : Int = (0 /: xs) (_ + _)
    product(List(a, b, c)  等价于  1 * a * b * c
    def product(xs : List[Int]) : Int = (1 /: xs) (_ * _)
    (List(a, b, c) :\ s) (op) 等价于  op(a, op(b, op(c, z)))

     // flattenRight性能更高，因为xs ::: ys的耗时与第一个参数xs的长度成正比,而flattenLeft的左操作数越来越大
    def flattenLeft(xss : List[List[T]]) =
        (List[T]() /: xss) (_ ::: _)
    def flattenRight(xss : List[List[T]] =
        (xss :\ List[T]()) (_ ::: _)
另一种reverse实现：

    def reverseLeft[T](xs : List[T]) =
        (List[T]() /: xs) ((ys, y) => y :: ys)

#### 列表排序:sort

    List(1, -3, 4, 2, 6) sort (_ < _)

### List对象的方法
#### 通过元素创建列表：List.apply

        List(1, 2, 3) 等价于 List.apply(1, 2, 3)
#### 创建数值范围：List.range
#### 创建统一的列表：List.make

        List.make(5, 'a')   // List(a, a, a, a, a)
#### List.unzip

        val zipped = "abcde".toList zip List(1, 2, 3)
        List.unzip(zipped)  // (List(a, b, c), List(1, 2, 3))
#### 连接列表：List.flatten, List.concat

        val xss = List(List('a', 'b'), List('c'), List('d', 'e'))
        List.flatten(xss)   // List(a, b, c, d, e)

        List.concat(List('a', 'b'), List('c'))  // List(a, b, c)
        List.concat(List(), List('a'), List('c'))  // List(a, b, c)
        List.concat()  // List[Noting] List()

#### 映射及测试配对列表:List.map2, List.forall2, List.exists2

        List.map2(List(10, 20), List(3, 4, 5)) (_ * _) // List(30, 80)
        List.forall2(List("abc", "de"), List(3, 2)) (_.length == _)  // true
        List.exists2(List("abc", "de"), List(3, 2)) (_.length != _)   // false

scala的类型推断是基于流的，在m(args)的方法调用中，（类型）推断器首先检查方法m是否有已知类型，如果有，那么这个类型将被用来做参数预期类型的推断。
如：abcde sort (_ > _)中，abcde的类型是List[Char],因此就明确了sort的参数类型为(Char, Char) => Boolean，并且产生的结果类型为List[Char]
推断器能推断出(_ > _)可以被扩展为(x : Char, x : Char) => x > y.

而msort((x : Char, y : Char) => x > y)(abcde)不能用msort(_ > _)(abcde),因为不能根据msort来知道确切的类型，可以如下解决：

    msort[Char](_ > _)(abcde)或者
    def msortSwapped[T](xs : List[T])(less : (T, T) => Boolean) : List[T] = {//}
通常，一旦需要推断多态方法类型参数的任务时，类型推断器就只会参考第一个参数列表中所有的值的参数类型，但不会参考之后其他的参数。

如果需要把参数设计为若干非函数值及一个函数值的某种多态方法，需要把函数参数独自放在柯里化参数列表的最后面，
这样一来，方法的正确实例类型就可以通过非函数参数推断出来，并且这个类型可以转而用来完成函数参数的类型检查。

(xs :\ z) (op)这里(xss :\ List()) (_ ::: _)会编译失败，因为z是空列表List(),没有任何附加类型信息，所以它的类型就被推断为List[Nothing],
这样，推断器ui认为折叠操作的类型B是List[Nothing]，结果折叠的(_ ::: _)操作被预期为如下类型：

        (List[T], List[Nothing]) => List[Nothing]，这意味着输入，输出始终是空列表，
在柯里化方法中，方法类型仅取决于第一段参数

函数式语言如ML或Haskell采用的是全局化的Hindley-Milner类型推断方法。

## 集合类型
scala.Iterable：Iterable是主要特质，它同时还是可变和不可变序列(scala.collection.Seq), 集（scala.collection.Set),
以及映射(scala.collection.Map)的超特质
序列是有序的集合，例如数组和列表，集可以通过==方法确定对每个对象最多只包含一个，映射则包含了键值映射关系的集合.

特质Iterator扩展了AnyRef，Iterable与Iterator之间的差异在于特质Iterable指代的是可以被枚举的类型（如集合类型），而特质Iterator是用来执行枚举操作的机制
Iterator可以被枚举若干次，但Iterator仅能使用一次，一旦使用Iterator枚举遍历了集合对象，就不能再使用它了。


### 序列
序列是继承特质Seq的类，序列的元素的有序的。

   * 列表：List，索引访问比较慢
   * 数组：Array，索引高效访问,可以使用返回数组的java方法。
   * 列表缓存：ListBuffer，可变对象，在scala.collection.mutable包中。能够支持常量时间的添加和前缀操作。元素的添加食用+=，前缀食用++操作符。调用toList方法得到List
使用ListBuffer代替List的另一个理由是为了避免堆栈溢出的风险。即使可以使用前缀的方法以正确的次序构建列表，但是递归算法不是尾递归，可以用for表达式或while循环及ListBuffer替代。
List类能够提供的对列表头部，而非尾部的快速访问，因此，如果需要通过向结尾添加对象的方式建造列表，可以先对表头前缀元素的方式反向构造列表，完成之后再调用reverse使得元素反转。
数组缓存
   * 数组缓存：ArrayBuffer,与数组类似，只是额外还允许在序列的开始或结束的地方添加和删除元素。所有的Array操作都被保留，新的添加
和删除操作平均为常量时间，但偶尔会因为实现申请分配新的数组以保留缓存内容而需要线性的处理时间。

    import scala.collection.mutable.ArrayBuffer
    // 创建ArrayBuffer时，必须指定它的类型参数，可以不用指定长度。ArrayBuffer可以自动调整分配的空间
    val buff = new ArrayBuffer[Int]()
    // 用+=操作添加元素
    buf += 12
    buf += 15
    buf // ArrayBuffer(12, 15)
    // 所有数组能用的方法ArrayBuffer都能使用
    buf.length
    buf(0)
   * 队列：Queue，有可变和不可变的序列

        import scala.collection.immutable.Queue
        val empty = new Queue[Int]
        val has1 = empty.enqueue(1)
        val has123 = has1.enqueue(List(2, 3))
        val (element, has23) = has123.dequeue
        // 可变队列使用+=及++=来添加元素，对于可变队列，dequeue方法将只从队列移除元素头并返回。
   * 栈：Stack，有可变和不可变版本，push,pop,top方法
   * 字符串：经RichString隐式转换，RichString的类型为Seq[Char]

        // String并没有定义exists方法，隐式转换会将它转换为RichString类
        def hasUpperCase(s : String) = s.exists(_.isUpperCase)

### 集Set和映射Map
默认使用的是不可变版本，

    object Predef {
        // type关键字用来把Set和Map定义为不可变集和映射特质的全引用名的别名
        type Set[T] = scala.collection.immutable.Set[T]
        type Map[K, V] = scala.collection.immutable.Map[K, V]
        // val把Set和Map初始化为指向不可变Set和Map的单例对象。
        val Set = scala.collection.immutable.Set
        val Map = scala.collection.immutable.Map
    }

    import scala.collection.mutable
    val text = "See Spot run. Run, Spot, Run!"
    val wordsArray = text.split("[ !.,]+")
    val words = mutable.Set.empty[String]
    for (word <- wordsArray) words += word.toLowerCase
Set的常用操作
操作                                      行为
val nums = Set(1, 2, 3)                   创建不可变集(nums.toString返回Set(1, 2, 3)
nums + 5                                    添加元素（返回Set(1, 2, 3, 5)
nums - 3                                    删除元素（返回Set(1, 2))
nums ++ List(5, 6)                          添加多个元素Set(1, 2, 3, 5, 6)
nums -- List(1, 2)                          删除多个元素Set(3)
nums ** Set(1, 3, 5, 7)                     获得交集Set(3)
nums.size                                   返回对象数量:3
nums.contains(3)                            检查是否包含（true）
import scala.collection.mutable             引用可变集合类型
val words = mutable.Set.empty[String]       创建空可变集（words.toString返回Set())
words += "the"                              添加元素)words.toString返回Set(the))
words -= "the"                              如果存在元素，则删除(words.toString返回Set())
words ++= List("do", "re", "mi")            添加多个元素(words.toString返回Set(do, re, mi))
wrods --= List("do", "re")                  删除多个元素(words.toString返回Set(mi))
words.clear                                 删除所有元素(words.toString返回Set())

     import scala.collection.mutable
     val map = mutable.Map.empty[String, Int]
     map("hello") = 1
     map("hello")   // 1
#### 映射常用操作
操作                                         行为
val nums = Map("i" -> 1, "ii" -> 2)         创建不可变映射(nums.toString返回Map(i->1, ii->2)
nums + ("vi" -> 6)                          添加条目（返回Map(i->1, ii->2, vi->6)
nums - "ii"                                 删除条目（返回Map(i->1)）
nums ++ List("iii"->3, "v"->5)              添加多个条目（返回Map(i->1, ii->2, iii->3, v->5)
nums -- List("i", "ii")                     删除多个条目（返回Map())
nums.size                                   返回映射条目(2)
nums.contains("ii")                         true
nums("ii")                                  2
nums.keys                                   返回键枚举器（返回字符串i和ii的Iterator)
nums.keySet                                 Set(i, ii)
nums.values                                 返回枚举器（返回整数1和2的Iterator)
nums.isEmpty                                指明映射是否为空（false）
import scala.collection.mutable
val words = mutable.Map.empty[String, Int]
words += ("one" -> 1)                       words.toString返回Map(one->1)
words -= "one"                              words.toString返回Map()
words ++= List("one"->1, "two"->2, "three"->3)  words.toString返回Map(one->1, two->2, three->3)
words --= List("one", "two")                words.toString返回Map(three->3)

scala.collection.mutable.Set()工厂方法返回scala.collection.mutable.HashSet
scala.collection.mutable.Map()工厂方法返回scala.collection.mutable.HashMap
#### 默认的不可变集实现
元素数量               实现
0                       scala.collection.immutable.EmptySet
1                       scala.collection.immutable.Set1
2                       scala.collection.immutable.Set2
3                       scala.collection.immutable.Set3
4                       scala.collection.immutable.Set4
5或更多                  scala.collection.immutable.HashSet
默认的不可变映射实现
元素数量                实现
0                       scala.collection.immutable.EmptyMap
1                       scala.collection.immutable.Map1
2                       scala.collection.immutable.Map2
3                       scala.collection.immutable.Map3
4                       scala.collection.immutable.Map4
5                       scala.collection.immutable.HashMap
如果向EmptySet添加元素返回Set1，Set1添加元素返回Set2，Set2删除元素返回Set1

#### 有序的（Sorted）集和映射
SortedSet,SortedMap特质，这2个特质分别由类TreeSet和TreeMap实现，使用红黑树保存元素和键
集的元素类型或映射的键类型必须要么混入，要么能隐式转换成Ordered特质。这些类只有不可变的版本。

    import scala.collection.immutable.TreeSet
     // ts : scala.collection.immutable.SortedSet[Int] = Set(0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
    val ts = TreeSet(9, 3, 1, 8, 0, 2, 7, 4, 6, 5)
    // cs : scala.collection.immutable.SortedSet[Char] = Set(f, n, u)
    val cs = TreeSet('f', 'u', 'n')

    import scala.collection.immutable.TreeMap
    // scala.collection.immutable.SortedMap[Int, Char] = Map(1->x, 3->x, 4->x)
    var tm = TreeMap(3 -> 'x', 1 -> 'x', 4 -> 'x')
    // // scala.collection.immutable.SortedMap[Int, Char] = Map(1->x, 2->x, 3->x, 4->x)
    tm += (2 -> 'x')

#### 同步集和映射
SynchronizedMap特质

    import scala.collection.mutable.{Map, SynchronizedMap, HashMap}
    object MapMaker {
        def makeMap : Map[String, String] = {
            new HashMap[String, String] with SynchronizedMap[String, String] {
                //  在键查询不到值时返回这个方法的内容
                override def default(key : String) = "Why do you want to know?"
            }
        }
    }

    // 把不可变集合义成val，则不能有+=方法，因为不可变Set没有这个方法，如果定义成var则可以，因为他转换成 a = a + b
    // 操作符适用于+=, -=, ++=, --=
    var people = Set('a', 'b')
    people += 'c' // 转换成people = people + 'c'

scala以等号结尾的都可以做方法优化

#### 初始化集合

    val stuff = mutable.Set[Any](42) // 可以添加任何数据
    val colors = List("blue", "yellow", "red", "green")
    val treeSet = TreeSet[String]() ++ colors // 不能直接用val treeSet = TreeSet(colors)

List.toArray, Array.toList操作需要复制集合的所有元素，对大型集合而言，速度比较慢

可变集合转换为不可集合：可以先创建空不可变集合，把可变集合的元素用++操作符添加进去

    // scala.collection.mutable.Set[String]
     val treeSet = TreeSet[String]() ++ colors
     // mutaSet : scala.collection.mutable.Set[String]
     val mutaSet = mutabl.Set.empty ++ treeSet
     // immutaSet : scala.collection.immutable.Set[String]
     val immutSet = Set.empty ++ mutaSet
     // muta : scala.collection.mutable.Map[String, Int]
     val muta = mutable.Map("i"->1)
     // immu : scala.collection.immutable.Map[String, Int]
     val immu = Map.empty ++ muta

### 元组
元组可以保存不同类型的对象:(1, "hello", Console)
访问元组的元素，用方法_1访问第一个元素，_2访问第2个元素

    val (word, idx) = longest // 元组的模式匹配
    val word, idx = longest // 将word和idx分别赋值longest

## 有状态的对象
在类中定义的var变量x的get方法名为x，set方法名为x_=（编译成字节码是x_$eq, 符号=转义成$eq)，字段x的访问权限是private[this],
get方法和set方法与var x的访问权限相同，public/protected/private

可以直接定义getter和setter方法以取代定义var变量，通过直接定义这些访问方法，可以按照自己的意愿解释对变量的访问和赋值操作：

    class Time {
        private[this] var h = 12
        private[this] var n = 12

        // 重新定义getter和setter方法，编译器会优先取自己的定义的，不会再额外生成,这里重新定义了hour和minute的方法
        def hour : Int = h
        def hour_=(x : Int) {
            require(0 <= x && x < 24)
            h = x
        }

        def minute = m
        def minute_= (x : Int) {
            require(0 <= x && x < 60)
            m = x
        }
    }

可以只定义getter和setter方法而不定义关联的字段

    class Thermometer {
        // 这里的_表示变量的初始化值，在这里Float的初始化值为0，因此把0赋值给celsius
        // 对数值来说，初始化值为0，布尔值是false，引用类型为null，与java一样
        // scala中不能省略= _，如果写成var celsius : Float,这将定义为抽象变量，而不是未初始化的变量,他的getter和setter方法都是abstract的
        var celsius : Float = _
        def fahrenheit = celsius * 9 / 5 + 32
        def fahrenheit_= (f : Float) {
            celsius = (f - 32) * 5 / 9
        }
        override def toString = fahrenheit + "F/" + celsius + "C"
    }

    val t = new Thermometer
    t.celsius = 100  // 这里调用celsius_=()方法
    t.fahrenheit = -40  // 这里调用fahrenheit_=()方法

### Simulation API
org.stairwaybook.simulation.Simulation

    // 把Action定义为带空参数列表并返回Unit的过程的别名，编译器只是将出现Action的地方替换成scala.Function0<scala.runtime.BoxedUnit>
    type Action = () => Unit

延迟方法：抽象方法，具体的定义将推迟到子类完成，延迟方法的名称开始于大写字母，因为它们代表的是常量。
又因为它们是方法，所以可以在子类中重载。

    // http://sale.jd.com/act/H15YfXP3hJFx6b.html

## 类型参数化
私有构造器：把private修饰符添加在类参数列表的前边，私有构造器只能被类本身和伴生对象访问。

    class Queue[T] private (
        private val leading : List[T],
        private val trailing : List[T]
    )
工厂方法：定义与类同名的Queue对象及apply方法

    // 把这个对象放在类Queue的同一个源文件中，就变成了类的伴生对象。伴生对象可以访问类的私有构造方法
    object Queue {
        // 用初始元素'xs'构造队列
        def apply[T](xs : T*) = new Queue(xs.toList, Nil)
    }

可将类定义成内部的私有类，再提供类公共接口的特质。

### 变化型注解
Queue是特质，Queue[String]，Queue[Any]是类型。Queue也被称为类型构造器。
协变（covariant）：如果S是类型T的子类型，那么Queue[S]可以被当做Queue[T]的子类型
默认的泛型类型是非协变的(nonvariant)子类型化

定义协变的：trait Queue[+T] {...}
定义逆变的：(contravariant)： trait Queue[-T] {...}  // 如果T是S的子类型，则Queue[S]是Queue[T]的子类型

    // 如果定义如下的Cell为协变的，则编译器会报错
    // covariant.scala:4: error: covariant type T occurs in contravariant position in type T of value x
    //         def set(x : T) {current = x}
    //                 ^
    // one error found
    class Cell[+T] (init : T)  {
        private[this] var current = init
        def get = current
        // 不允许使用+号注解的类型参数用作方法参数类型
        // 方法参数是负的位置？
        def set(x : T) {current = x}
    }


    // covariant.scala:3: error: contravariant type T occurs in covariant position in type => T of method get
    //           def get = current
    //               ^
    //   one error found
    class Cell[-T] (init : T)  {
            private[this] var current = init
            // 不允许使用-号注解的类型参数用作方法返回的类型。
            // 方法返回值是正的位置？
            def get = current
            def set(x : T) {current = x}
    }

Java中的数组是协变的。

    // 能编译通过，但执行时会抛出java.lang.ArrayStoreException
    // java在运行时保留了数组的元素类型，每次数组元素更新时，新元素都会用存储的类型校验合法性，如果不是该类型的实例，就会抛出ArrayStore异常
    String[] a1 = {"abc"};
    Object[] a2 = a1;
    a2[0] = Integer.valueOf(17);
    String s = a1[0];

    // java中保留数组是协变的是因为希望有一个通用处理数组的简单方法，例如想要能够编写方法排序数组的所有元素，使用以下的方法签名带入对象数组
    void sort(Object[] a, Comparator comp) {...}
    // 数组的协变被用来确保任意参考类型的数组都可以传入排序方法。现在有泛型，数组的协变已经没用，只是为了兼容旧版本

在scala中，Array是非协变的。为了兼容java代码，需要使用对象数组作为模拟泛型数组的手段，可以把T类型的数组造型为任意T的超类型的数组：

    // 编译时scala编译器成功，运行时也是成功的。因为运用了java数组是协变的特性。
    val a1 = Array("abc")
    val a2 : Array[Object] = a1.asInstanceOf[Array[Object]]

编译器实现的检查变化型注解的机制：（不太明白，需要看源码了解？或用多个例子说明）
Scala编译器会把类或特质结构体的所有位置分类为正，负，或中立。所谓位置，是指类（包括类和特质）的结构体内可能会用到类型参数的地方。例如，任何方法
的值参数都是这种位置，因为方法值参数具有类型，所以类型参数可以出现在这个位置。

编译器检查类的类型参数的每一个用法。注解了+号的类型参数只能被用在正的位置上，而注解了-号的类型参数只能用在负的位置上。没有变化型注解的类型参数可以用在任何位置，
因此它是唯一能被用在类结构体的中性位置上的类型参数。

为了对这些位置分类，编译器首先从类型参数的声明开始，然后进入更深的内嵌层。处于声明类的最顶层被划为正的位置。默认情况下，更深的内嵌层的位置的分类会与它的外层一致。
也有几种例外会改变具体的分类。方法值参数位置是方法外部位置的翻转类型。（正的翻转为负的，负的翻转为正的，中性的不变）

除了方法值参数位置外，方法的类型参数的当前类别也会同时被翻转。而类型的类型参数位置，如C[Arg]中的Arg，也有可能被翻转，这取决于对应类型参数的变化型。如果C的
类型参数标注了+号，那么类别保持不变，如果C的类型参数标注了-号，那么当前类别被翻转，如果C的类型参数没有变化型注解，那么当前类型将改为中性。

    // 类型参数W。以及2个值参数，volume, listener的位置都是负的。meow的结果类型，第一个Cat[U, T]参数的位置是负的，因为Cat的第一个类型参数T被标注了-号
    // 这个参数中的类型U重新转为正的位置（两次翻转）,而参数中的类型T仍然为负的位置
    // 方法的类型参数，如W会翻转，方法的参数会翻转，方法的返回值不会变，默认内嵌的与外部的一致，但是如Cat类型的，如果定义成+的则不变，如果定义成-的则翻转，没有变化型注解则改为中性。
    abstract class Cat[-T, +U] {
        def meow[W-](volume :　T-, listener : Cat[U+, T-]-) : Cat[Cat[U+, T-]-, U+]+
    }


### 下界
可以通过把append变为多态以使其泛型化（即提供给append方法类型参数）并使用它的类型参数下界，

    class Queue[+T] (
        private val leading : List[T],
        private val trailing : List[T]) {
            // 定义了append类型参数U，并通过语法U >: T，定义了T为U的下界，因此，U必须是T的超类型。（对自身来讲，既是子类型也是超类型，即使T是U的下界，仍然能够把T传给append方法）
            // 这里发生了下界的翻转，类型参数U处于负的位置（1次翻转），而下界(>: T)处于正的位置（2次翻转）
            def append[U >: T] (x : U) = new Queue[U](leading, x :: trailing)
        }
        // Fruit有2个子类型：Apple和Orange，可以调用append(Orange)把Orange加入到Queue[Apple]，返回Queue[Apple]，这里通过泛型，把Orange当做Apple

         class T[+A, -B] {
              // error: contravariant type B occurs in covariant position in type  >: B of type C
              //      def test[C >: B](c : C) : C
              //               ^
              // one error found
              // 由于C是非协变的，所以C可以出现在任何地方
              // C的位置是负的，而下界>:会翻转类型，所以B的位置翻转成正的位置，而B是逆变的，所以出错。如果是下界，则不会翻转类型
              // 方法类型中好像不能定义协变或逆变？？？
              def test[C >: B](c : C) : C
         }

类型系统的通用原则：如果能在需要类型U的值的地方替换成类型T的值，那么类型T是类型U的子类的假设就是安全的。这被称为里氏替代原则(Liskov Substitution Principle LSP)
原则的前提是T要支持U所支持的操作并且对于T的所有操作来说，与U的对应操作比较，需求的要更少，而提供的要更多。（参数是需求的东西，结果是提供的东西）
Function1[A, B]的定义：

    trait Function1[-S, +T] {
        def apply(x : S) : T
    }
Function1的继承关系与它的返回值的继承关系一样

对象私有成员private[this]仅能在被定义的对象内部访问，而在它们被定义的同一个对象内访问这些变量并不会引起与变化型有关的问题，
scala变化型规则包含了关于对象私有定义的特例：当检查到带有+/-号的类型参数只出现在具有相同变化型分类的位置上时，这种定义将被忽略。

    //  covariant type T occurs in contravariant position in type T of value a_=
    //  class B[+T] (
    //        ^
    //  one error found
    // 在生成的set方法中用T作为参数了，参数是逆变的。
    // 在这里把a定义成val则不会出现这个问题，因为val不会有set方法
    // 或者将a定义成private[this] var a : T，因为对象私有定义有特例。
    class B[+T] (
          var a : T
    )

## 抽象成员
抽象成员可以是类型(T),方法,val,以及var

    trait Abstract {
        type T     // 类型
        def transform(x : T) T // 方法
        val initial : T  // val
        var current : T  // var
    }

    class Concrete extends Abstract {
        type T = String
        def transform(x : String) = x + x
        val initial = "hi"
        var current = initial
    }

抽象类型是指不带具体定义的，（由type关键字）声明为类或特质成员的类型。上面的T

### 抽象val
抽象val: val initial : String，它指定了val的名称和类型，但不指定值。该值必须由子类的具体val定义提供。
抽象val声明类似于抽象的无参方法声明：def initial : String

抽象的val的任何实现都必须是val类型的定义，不可以是var或def，抽象方法声明可以被实现为具体的方法定义或具体的val定义。

### 抽象var
抽象var只声明名称和类型，没有初始值

    trait AbstractTime {
        var hour : Int
        var minute : Int
    }
    与下面的等价
    trait AbstractTime {
        def hour : Int // hour 的getter方法
        def hour_=(x : Int) // hour的setter方法
        def minute : Int // minute的getter方法
        def minute_=(x : Int) // minute的setter方法
    }

抽象val可用作超类的参数的角色

    trait RationalTrait {
        val numerArg : Int
        val denomArg : Int
    }
    // 实现上面的特质：这个表达式可以产生混入了特质并被结构体定义的匿名类实例。类似于new Rational(1, 2)
    new RationalTrait {
        val numerArg = 1
        val denomArg = 2
    }

    // 两个表达式expr1,expr2会在类Rational初始化之前计算。
    new Rational(expr1, expr2)

    // 表达式expr1, expr2被作为匿名类初始化的一部分计算，匿名类的初始化在RationalTrait之后
    // 在RationalTrait初始化的时候，numrArg和denomArg的值为默认值0
    new RationalTrait {
        val numerArg = expr1
        val denomArg = expr2
    }


    trait T {
        val a : Int
        val b : Int
        println("t")
    }

    class C {
        def test() {
            new T {
                val a = 1
                val b = 2
            }
        }
    }
    这个代码生成的java代码如下：
    public interface T {
      public abstract int a();
      public abstract int b();
    }

    public abstract class T$class {
      public static void $init$(T) {
          scala.Predef$.MODULE$.println("t");
      }
    }

    public class C {
      public void test() {
            new C$$anon$1(this);
      }
      public C() {
            super();
      }
    }

    public final class C$$anon$1 implements T {
      private final int a;
      private final int b;
      public int a() {
            return a;
      }
      public int b() {
            return b;
      }

      // 在这里，混入了trait之后，先调用类的父类构造方法，再调用被混入的trait的$init$方法（非val, var, def定义）
      // 最后调用类自身的一些字段赋值，初始化等。
      public C$$anon$1(C) {
            super();

            T$class.$init$(this);
            this.a = 1;
            this.b = 2;
      }
    }

### fields预初始化字段
预初始化字段，可以在调用超类之前初始化子类的字段。操作方式是把字段定义加上化括号，放在超类构造器调用之前。
预初始化字段并不仅限于匿名类，它们还可以被用作于对象或有名称的子类。

       class C {
           def test() {
               new  {
                   val a = 1
                   val b = 2
               } with T
           }
       }
       生成的java代码如下：
       public class C {
         public void test() {
             new C$$anon$1(this);
         }

         public C() {
                super();
         }
       }

       public final class C$$anon$1 implements T {
         private final int a;
         private final int b;
         public int a() {
                return a;
         }
         public int b() {
                return b;
         }

         // 这里字段的初始化在调用被混入trait之前。
         public C$$anon$1(C) {
                this.a = 1;
                this.b = 2;
                super();
                T$class.$init$(this);
         }
       }

对不是匿名类的一般类也同样适用：将初始化内容放在extends 之后，trait名之前

    object O extends {
        val a = 1
        val b = 2
    } with T

    生成的java代码如下：
    public final class O {
      public static int b() {
            return O$.MODULE$.b();
      }
      public static int a() {
            return O$.MODULE$.a();
      }
    }

    public final class O$ implements T {
      public static final O$ MODULE$ = new O$();
      private final int a;
      private final int b;
      public int a() {
            return a;
      }
      public int b() {
            return b;
      }
      private O$() {
            this.a = 1;
            this.b = 1;
            super();
            this.MODULE$ = this;
            T$class.$init$(this);
      }
    }


    object O T {
        val a = 1
        val b = 2
    }
    生成的java代码如下：
    public final class O {
      public static int b() {
         return O$.MODULE$.b();
      }
      public static int a() {
            return O$.MODULE$.a();
      }
    }

    public final class O$ implements T {
      public static final O$ MODULE$ = new O$();
      private final int a;
      private final int b;
      public int a() {
            return a;
      }
      public int b() {
            return b;
      }

      // 默认情况下，字段的赋值是在调用了父类构造方法和trait的初始化方法之后
      private O$() {
            super();
            this.MODULE$ = this;
            T$class.$init$(this);
            this.a = 1;
            this.b = 2;
      }
    }

由于预初始化的字段在超类构造其调用之前被初始化，因此它们的初始化器不能引用正被构造的对象。如果有引用this的这种初始化
构造器，那么实际指向的是包含了正被构造的类或对象的对象，而不是被构造对象本身。

    // 这里的this代表的是T的对象，而不是T的对象，如果T没有a和b字段，会报错
    class T {
        def test() {
            new {
                // this通过匿名类调用外部类得到，匿名类的构造方法用外部类的引用作为参数
                val a = this.a
                val b = this.b
            } with T
        }
    }

    // 这里的类参数只提供参数，没提供字段，在构造器中被numerArg和denomArg调用
    class RationalClass(n : Int, d : Int) extends {
        val numerArg = n
        val denomArg = d
    } with RationalTrait {
        def +(that : RationalClass) =
            new RationalClass(numer * that.denom + that.numer * denom, denom * that.denom)
    }

### 懒加载
如果把lazy修饰符放在val定义的前面，那么右侧的初始化表达式将直到val第一次被使用的时候才计算。 通过设置一个volatile的布尔变量控制第一次访问执行

    object Demo {
        val x = {println("initializing x"); "done"}
    }
    class L {
          Demo
         val d =  Demo.x
    }

    编译成java代码如下
    public final class Demo {
      public static java.lang.String x() {
            return Demo$.MODULE$.x();
      }
    }

    public final class Demo$ {
      public static final Demo$ MODULE$ = new Demo$();
      private final java.lang.String x;
      public java.lang.String x() {
            return this.x;
      }
      private Demo$() {
            super();
            this.MODULE$ = this;
            scala.Predef$.MODULE$.println("initializing x");
            this.x = "done";
      }
    }

    public class L {
      private final java.lang.String d;
      public java.lang.String d() {
            return this.d;
      }
      public L() {
            super();
            // 调用Demo
            Demo$.MODULE$;
            // 调用Demo.x
            this.d = Demo$.MODULE$.x();
      }
    }




    object Demo {
        lazy val x = {println("initializing x"); "done"}
    }
    class L {
          Demo
          val d =  Demo.x
    }

    生成的java代码如下：
    public final class Demo {
      public static java.lang.String x() {
            return Demo$.MODULE$.x();
      }
    }

    // 一部分字节码未解析完成
    public final class Demo$ {
      public static final Demo$ MODULE$ = new Demo$();
      private java.lang.String x;
      private volatile boolean bitmap$0;
      private java.lang.String x$lzycompute() {
            synchronized (this) {
                if (!this.bitmap$0) {
                    scala.Predef$.MODULE$.println("initializing x");
                    this.x = "done";
                    // 布尔值在虚拟机内部是以0或1来表示的，1表示true，0表示false
                    this.bitmap$0 = true;
                }

                scala.runtime.BoxedUnit.UNIT
            }

            return this.x;
      }
        Code:
           0: aload_0
           1: dup
           2: astore_1
           3: monitorenter
           4: aload_0
           5: getfield      #20                 // Field bitmap$0:Z
           8: ifne          30
          11: aload_0
          12: getstatic     #25                 // Field scala/Predef$.MODULE$:Lscala/Predef$;
          15: ldc           #27                 // String initializing x
          17: invokevirtual #31                 // Method scala/Predef$.println:(Ljava/lang/Object;)V
          20: ldc           #33                 // String done
          22: putfield      #35                 // Field x:Ljava/lang/String;
          25: aload_0
          26: iconst_1
          27: putfield      #20                 // Field bitmap$0:Z
          30: getstatic     #41                 // Field scala/runtime/BoxedUnit.UNIT:Lscala/runtime/BoxedUnit;
          33: pop
          34: aload_1
          35: monitorexit
          36: aload_0
          37: getfield      #35                 // Field x:Ljava/lang/String;
          40: areturn
          41: aload_1
          42: monitorexit
          43: athrow
        Exception table:
           from    to  target type
               4    36    41   any

      public java.lang.String x() {
            if (!this.bitmap$0) {
                return this.x$lzycompute();
            } else {
                return this.x;
            }
      }
        Code:
           0: aload_0
           1: getfield      #20                 // Field bitmap$0:Z
           4: ifeq          14
           7: aload_0
           8: getfield      #35                 // Field x:Ljava/lang/String;
          11: goto          18
          14: aload_0
          15: invokespecial #46                 // Method x$lzycompute:()Ljava/lang/String;
          18: areturn

      private Demo$() {
            super();
            this.MODULE$ = this;
      }
        Code:
           0: aload_0
           1: invokespecial #49                 // Method java/lang/Object."<init>":()V
           4: aload_0
           5: putstatic     #51                 // Field MODULE$:LDemo$;
           8: return
    }

### 抽象类型

    class Food
    abstract class Animal {
        def eat(food : Food)
    }

    c;ass Grass extends Food
    class Cow extends Animal {
        override def eat(food : Grass) {}  // 不能编译
    }
    // 编译上面代码，出现下面的错误
    abstract.scala:7: error: class Cow needs to be abstract, since method eat in class Animal of type (food: Food)Unit is not defined
    (Note that Food does not match Grass: class Grass is a subclass of class Food, but method parameter types must match exactly.)
    class Cow extends Animal {
          ^
    abstract.scala:8: error: method eat overrides nothing.
    Note: the super classes of class Cow contain the following, non final members named eat:
    def eat(food: Food): Unit
          override def eat(food : Grass) {}
                       ^
    two errors found


    // 如果能通过，则定义下面的代码：
    class Fish extends Food
    val bessy : Animal = new Cow
    bessy eat (new Fish)  // 牛吃鱼，产生错误

    // 修改成如下的：
    class Food
    abstract class Animal {
        type SuitableFood <: Food
        def eat(food : SuitableFood)
    }
    class Grass extends Food
    class Cow extends Animal {
        type SuitableFood = Grass
        override def eat(food : Grass) {}
    }


    class E
    class F extends E
    abstract class A {
            // 编译器会在编译期将X这个type转换为E
          type X <: E
          def test(b : X)
    }

    class B extends A {
          type X = F
          override def test(b : F) {}
          // 或者定义成override def test(b : X) {}
    }

    生成如下的字节码：
    public class E {
        public E() {
            super();
        }
    }

    public class F extends E {
        public F() {
            super();
        }
    }

    public abstract class A {
      public abstract void test(E);
      public A() {
            super();
      }
    }

    public class B extends A {
      public void test(F) {

      }
      // 会额外生成一个继承父类的方法，这个方法利用checkCast来转换类型。
      public void test(E e) {
            this.test((F)e);
      }

      public B() {
            super();
      }
    }

### 路径依赖类型

        class DogFood extends Food
        class Dog extends Animal {
            type SuitableFood = DogFood
            override def eat(food : DogFood) {}
        }

        val bessy = new Cow
        val lassie = new Dog
        // 出错
        lassie eat (new bessy.SuitableFood)  // 这里bessy.SuitableFood == Grass
        // 正确
        val bootsie = new Dog
        lassie eat (new bootsie.SuitableFood)  // 这里bootsie.SuitableFood = DogFood

scala的内部类：

        class Outer {
            class Inner
        }
        // 内部类的表达形式为Outer#Inner.而不是java的Outer.Inner
        val o1 = new Outer
        val o2 = new Outer
o1.Inner和o2.Inner是两个路径依赖类型（并且是不同的类型）两种类型都符合于（即继承于）更一般的类型Outer#Inner（该类型代表任意Outer类型外部对象的Inner类）
相反，o1.Inner类型是指特定(o1引用的)外部对象的Inner类，类型o2.Inner指的是不同的特定（o2引用的）外部对象的Inner类。

Scala与java一样，内部类实例持有外部类实例的引用，使得内部类可以执行类似于访问其外部类的成员的操作，因此不能在没有通过某种方式指定外部类实例的情况下实例化内部类。

实例化内部类的方式之一是直接在外部类的方法体中完成。这种情况下将使用当前外部类实例（this引用的对象）
方式之二是使用路径依赖类型，例如，因为类型o1.Inner命名了特定外部对象，所以可以用法如下方式实现实例化：new o1.Inner

### 枚举
想要创建新的枚举，只需定义扩展scala.Enumeration这个类的对象即可。

    object Color extends Enumeration {
        val Red = Value
        val Green = Value
        val Blue = Value
    }

    // 可以简化
    object Color extends Enumeration {
        val Red, Green, Blue = Value
    }

    // Color.Read与Direction.Value的值是不一样的，因为两种类型的路径部分不同
    object Direction extends Enumeration {
        val North, East, South, West = Value
    }
枚举可以用for，map， flatMap,filter等

    object E extends Enumeration {
          val A, B, C, D, E = Value
    }
    生成的java代码如下：
    public final class E {
      public static scala.Enumeration$Value E() {
            return E$.MODULE$.E();
      }
      public static scala.Enumeration$Value D() {
            return E$.MODULE$.D();
      }

      public static scala.Enumeration$Value C() {
            return E$.MODULE$.C();
      }
      public static scala.Enumeration$Value B() {
            return E$.MODULE$.B();
      }
      public static scala.Enumeration$Value A() {
            return E$.MODULE$.A();
      }
      public static scala.Enumeration$Value withName(java.lang.String name) {
            return E$.MODULE$.withName(name);
      }
      public static scala.Enumeration$Value apply(int index) {
            return E$.MODULE$.apply(index);
      }
      public static int maxId() {
            return E$.MODULE$.maxId();
      }
      public static scala.Enumeration$ValueSet values() {
            return E$.MODULE$.values();
      }
      public static java.lang.String toString() {
            return E$.MODULE$.toString();
      }
    }

    public final class E$ extends scala.Enumeration {
      public static final E$ MODULE$ = new E$();
      private final scala.Enumeration$Value A;
      private final scala.Enumeration$Value B;
      private final scala.Enumeration$Value C;
      private final scala.Enumeration$Value D;
      private final scala.Enumeration$Value E;
      public scala.Enumeration$Value A() {
            return this.A;
      }
      public scala.Enumeration$Value B() {
            return this.B;
      }
      public scala.Enumeration$Value C() {
            return this.C;
      }
      public scala.Enumeration$Value D() {
            return this.D;
      }
      public scala.Enumeration$Value E() {
            return this.E;
      }
      private E$() {
            super();
            this.MODULE$ = this;
            this.A = this.Value();
            this.B = this.Value();
            this.C = this.Value();
            this.D = this.Value();
            this.E = this.Value();

      }
    }

scala不能创建抽象类型的实例，也不能把抽象类型当作其他类的超类型，如定义type A <: B，则不能创建A的实例，也不能将A作为其他类的超类。
Scala的formatted方法，与java的String.format一致

## 隐式转换和参数

    // 定义String到RandomAccessSeq[Char]的隐式转换
    implicit def stringWrapper(s : String) =
        new RandomAccessSeq[Char] {
            def length = s.length
            def apply(i : Int) = s.charAt(i)
        }
    // 可以显式转换
    stringWrapper("abc123") exists (_.isDigit)
    // 也可以隐式转换，编译器会转换成上面的方法调用
    "abc123" exists (_.isDigit)

    def printWithSpace(seq : RandomAccessSeq[Char]) = seq mkString " "
    // 可以直接传入String参数
    printWithSpace("xyz")

### 隐式转换规则
   * 标记规则：只有标记为implicit的定义才是可用的，可以使用它标记任何变量，函数，或对象定义。
   * 作用域规则：插入的隐式转换必须以单一标识符的形式处于作用域中，或与转换的源或目标类型关联在一起。
   scala编译器将仅考虑处于作用域之内的隐式转换，从而，为了使隐式转换可用，必须以某种方式把它带入作用域之内。此外，隐式转换必须以单一标识符的形式进入作用域。
   编译器不能插入形式为someVariable.convert的转换，例如不能把x + y扩展为someVariable.convert(x) + y。常常使用import Premable
   ._访问库的隐式转换（Premable是定义隐式转换的对象）

   单一标识符规则有一个例外。编译器还将在源类型或转换的期望目标类型的伴生对象中寻找隐式定义。例如，如果尝试传递Dollar对象给入参为Euro的方法，源类型为Dollar，目标类型为Euro，
   因此可以把从Dollar到Euro的隐式转换打包到Dollar或Euro类的伴生对象中。

        object Dollar {
            implicit def dollarToEuro(x : Dollar) : Euro = ...
        }
        class Dollar {
            ...
        }
   * 无歧义规则：隐式转换唯有不存在其他可插入转换的前提下才能插入。如果编译器有2个可选方法修正x + y，那么它会报错并拒绝在两者之间做出选择（这里没有选择最佳匹配避免代码出错）
   * 单一调用规则：只会尝试一个隐式操作。编译器不会把x + y重写成convert1(convert2(x)) + y,编译器不会尝试在某个隐式操作期间再添加隐式转换。
   * 显式操作先行规则，若编写的代码类型检查无误，则不会尝试任何隐式操作。
   * 命名隐式转换：隐式转换可以任意命名。
   * 隐式操作在哪里尝试：转换为期望类型，指定（方法）调用者的转换，隐式参数。

### 隐式转换为期望类型：

    implicit def doubleToInt(x : Double) = x.toInt
    // 这里会隐式调用val i : Int = doubleToInt(3.5)
    // 在编译器之外，可以通过一个import语句或通过继承把doubleToInt带入作用域
    // 通常将较小的类型转换为较大的类型，而不是相反
    val i : Int = 3.5

### 转换（方法调用的）接收者（方法调用的对象）

    // 与新类型的交互操作
    class Rational(n : Int, d : Int) {
        ...
        def + (that : Rational) : Rational = ...
        def - (that : Int) : Rational = ...
    }

    // 将Int作为新类Rational使用
    implicit def intToRational(x : Int) = new Rational(x, 1)
    // 由于Int没有方法+(Rational), 则调用隐式转换可以直接在Int上调用+(Rational)
    val oneHalf = new Rational(1, 2)
    1 + oneHalf


    // 模拟新的语法, ->语法的定义
    Map(1 -> "one", 2 -> "two", 3 -> "three")
    package scala
    object Predef {
        class ArrowAssoc[A](x : A) {
            def -> [B](y : B) : Tuple2[A, B] = Tuple2(x, y)
        }
        implicit def any2ArrowAssoc[A](x : A) : ArrowAssoc[A] = new ArrowAssoc(x)
    }
一旦看到调用的方法不像是存在于接收者类中，就有可能在使用隐式操作。

### 隐式参数
编译器有时候会用someCall(a)(b)替代someCall(a),或者用new SomeClass(a)(b)替代new SomeClass(a)，从而通过添加缺失的参数列表以满足函数调用。

    class PreferredPrompt(val preference : String)
    object Greeter {
        // prompt参数是隐式提供的，也可以显式提供
        def greet(name : String)(implicit prompt : PreferredPrompt) {
            println("Welcome: " + name + ". The system is ready.")
            println(prompt.preference)
        }
    }

    val bobePrompt = new PreferredPrompt("relax> ")
    Greeter.greet("Bob")(bobePrompt)

    // 为了让编译器隐式提供参数，必须首先定义期望类型的变量，
    object JoesPrefs {
        // 这里必须定义为implicit,并且它必须以单一标识符处于作用域之内
        implicit val prompt = new PreferredPrompt("Yes, master> ")
    }
    import JoesPrefs._ // 如果没有引入，则不能隐式转换
    Greeter.greet("Joe")

隐式参数可以带多个参数：

    class PreferredPrompt(val preference : String)
    class PreferredDrink(val preference : String)

    object Greeter {
        def greet(name : String)(implicit prompt : PreferredPrompt, drink : PreferredDrink) {
            println("Welcome, " + name + ". The system is ready.")
            print("But while you work.")
            println("Why not enjoy a cup of " + drink.preference + "?")
            println(prompt.preference)
        }
    }

    object JoesPrefs {
        // 这里定义成var或val都可以
        // prompt和drink的名称不是固定的，可以随便定义，参数隐式转换是通过类型来匹配的
        implicit val prompt = new PreferredPrompt("yes, master> ")
        implicit val drink = new PreferredDrink("tea")
    }

    import JoesPrefs._
    Greeter.greet("Joe")

求列表中的最大元素：

    // 隐式参数如果是一个类型的话，则定义隐式参数需要是类型，如果是方法的话，则需要定义def
    // 这里是一个T => Ordered[T]的方法
    def maxListImpParm[T](elements : List[T])(implicit orderer : T => Ordered[T]) : T =
        elements match {
          case List() => throw new IllegalArgumentException("empty list")
          case List(x) => x
          case x :: rest =>
                // 这里orderer是隐式提供的，所以可以省略
                val maxRest = maxListImpParm(rest)
              // val maxRest = maxListImpParm(rest)(orderer)
              // orderer是隐式提供的，所以可以省略
              if (x > maxRest) x else maxRest
              // if (orderer(x) > maxRest) x else maxRest
        }

    // 编译器会把Int，Double, String等对象隐式插入了orderer函数(T => Ordered[T])。分别转换成RichInt, RichDouble, RichString
    maxListImpParm(List(1, 5, 10, 13))
    maxListImpParm(List(1.5, 2, 10.8, 3.1212))
    maxListImpParm(List("one", "two", "three"))

隐式参数的样式规则：最好对隐式参数的类型使用自定义命名的类型。如果上面的orderer修改成(T, T) => Boolean,则会太普通，容易引起代码混乱
至少用一个角色确定的名称为隐式参数的类型命名（如Ordered)

### 视界

    //  这里T <% Ordered[T]表示任何的T，只要能被当作Ordered[T]即可，尽管Int不是Ordered[Int]的子类型，但是只要可以通过隐式转换就可以
    // 如果类型T碰巧已经是Ordered[T]类型，也仍人可以把List[T]传给maxList方法，编译器将调用声明在Predef中的隐式鉴别函数：
    // implicit def identity[A](x : A) : A = x
    def maxList[T <% Ordered[T]](elements : List[T]) : T =
        elements match {
            case List() => throw new IllegalArgumentException("empty list")
            case List(x) => x
            case x :: rest =>
                val maxRest = maxList(rest)
                if (x > maxRest) x else maxRest
        }

### 隐式操作调试：
   * 显式写出方法，看是否类型匹配
   * -Xprint:typer选项可以显示编译器正在插入的隐式转换，运行scalac

        package a

        object Implicit extends  Application {

          def maxListImpParm[T](elements : List[T])(implicit orderer : T => Ordered[T]) : T =
            elements match {
              case List() => throw new IllegalArgumentException("empty list")
              case List(x) => x
              case x :: rest =>
                  val maxRest = maxListImpParm(rest)(orderer)
                  if (orderer(x) > maxRest) x else maxRest
            }

          maxListImpParm(List(1, 5, 10, 13))
          maxListImpParm(List(1.5, 2, 10.8, 3.1212))
          maxListImpParm(List("one", "two", "three"))
        }

        编译：
        scalac -Xprint:typer a/implicit.scala

        编译器输出内容
        [[syntax trees at end of                     typer]] // implicit.scala
        package a {
          object Implicit extends AnyRef with Application {
            def <init>(): a.Implicit.type = {
              Implicit.super.<init>();
              ()
            };
            def maxListImpParm[T >: Nothing <: Any](elements: List[T])(implicit orderer: T => Ordered[T]): T = elements match {
              case immutable.this.List.unapplySeq[T](<unapply-selector>) <unapply> () => throw new scala.`package`.IllegalArgumentException("empty list")
              case immutable.this.List.unapplySeq[T](<unapply-selector>) <unapply> ((x @ _)) => x
              case (hd: T, tl: List[T])scala.collection.immutable.::[?T1]((x @ _), (rest @ _)) => {
                val maxRest: T = Implicit.this.maxListImpParm[T](rest)(orderer);
                if (orderer.apply(x).>(maxRest))
                  x
                else
                  maxRest
              }
            };
            Implicit.this.maxListImpParm[Int](immutable.this.List.apply[Int](1, 5, 10, 13))({
              ((x: Int) => scala.this.Predef.intWrapper(x))
            });
            Implicit.this.maxListImpParm[Double](immutable.this.List.apply[Double](1.5, 2.0, 10.8, 3.1212))({
              ((x: Double) => scala.this.Predef.doubleWrapper(x))
            });
            Implicit.this.maxListImpParm[String](immutable.this.List.apply[String]("one", "two", "three"))({
              ((x: String) => scala.this.Predef.augmentString(x))
            })
          }
        }

        warning: there were 1 deprecation warning(s); re-run with -deprecation for details
        one warning found

## 实现列表
List的层级：
scala.List[+T]<sealed abstract>
scala.::[T]<final case>
scala.Nil<case object>
x :: xs 被当作样本类::的构造器调用：::(x, xs)

    abstract class Fruit
    class Apple extends Fruit
    class Orange extends Fruit
    val apples = new Apple :: Nil   // apples : List[Apple] = List(Apple@478212)
    val fruits = new Orange :: apples  // fruits : List[Fruit] = List(Orange@123123, Apple@478212)
    // 因为::方法的定义为def ::[B >: A] (x: B): List[B] = new scala.collection.immutable.::(x, this)

scala.collection.mutable.ListBuffer  +=和toList是常量时间操作
private[scala] var tl: List[B]表示tl只能在scala包中的类访问，包括scala包的子包。如scala.collection.mutable.ListBuffer，ListBuffer类访问了tl元素

ListBuffer的一些字段和方法：
start : List[A]         指向存储于缓冲的所有元素的列表
last0 : ::[A]           指向列表中的最后一个::单元
exported : Boolean      说明缓冲是否曾经通过使用toList操作转换为列表
toList
+=                      只有在列表缓冲被转换为列表之后（调用toList之后）还要扩展（调用+=）的情况下才需要复制元素

## 重访for表达式
所有的yield产生结果的for表达式都会被编译器转译为高阶方法map，flatMap及filter的组合使用,所有不带yield的for循环都会被转译为仅对高阶函数filter和foreach的调用

    //  由生成器，一个定义，一个过滤器组成
    for (p <- persons; n = p.name; if (n startsWith "To")) yield n
    for {
        p <- persons // 生成器
        n = p.name // 定义
        if (n startsWith "To") // 过滤器
    } yield n

### 皇后问题
### 使用for表达式做查询
for等同于数据库查询语言的常用操作
### for表达式的转译：
   * 转译一个生成器的for表达式

        for (x <- expr1) yield expr2
        // 转译为
        expr1 .map(x => expr2)
   * 转译以生成器和过滤器开始的for表达式

        for (x <- expr1; if expr2) yield expr3
        // 转译为
        for (x <- expr1 filter (x =>expr2)) yield expr3
        // 最终转译为
        expr1 filter (x => expr2) map (x => expr3)

        // 如果在过滤之后有更多的元素，同样的转译方案将再次应用。如果seq是任意序列的生成器，定义及过滤器：
        for (x <- expr1; if expr2; seq) yield expr3
        // 转译为
        for (x <- expr1 filter expr2; seq) yield expr3
   * 转译以两个生成器开始的for表达式

        // seq是任意序列的生成器，定义及过滤器，实际上，seq可能是空的，这种情况下，expr2后面就没有分号了
        for (x <- expr1; y <- expr2; seq) yield expr3
        // 转译为
        expr1 .flatMap (x => for (y <- expr2; seq) yield expr3)


        for (b1 <- books; b2 <- books if b1 != b2;
            a1 <- b1.authors; a2 <- b2.authors if a1 == a2) yield a1
        如何而转译？
   * pattern转译

        for ((x1, ... xn) <- expr1) yield expr2
        // 转译为
        expr1.map (case (x1, ... xn) => expr2)

        for (pat <- expr1) yield expr2
        // 转译为
        expr1 filter (
            case pat => true
            case _ => false
        ) map {
            case pat => expr2
        }
   * 转译定义

        for (x <- expr1; y = expr2; seq) yield expr3
        // 转译为
        for ((x, y) <- for (x <- expr1) yield (x, expr2)); seq) yield expr3
### 转译for循环

    for (x <- expr1) body
    // 被转译为
    expr1 foreach (x => body)

    for (x <- expr1; if expr2; y <- expr3) body
    // 被转译为
    expr1 filter (x => expr2) foreach (x => expr3 foreach (y => body))

### 反其道而行之

    object Demo {
        def map[A, B](xs : List[A], f : A => B) : List[B] =
            for (x <- xs) yield f(x)
        def flatMap[A, B] (xs : List[A], f : A => List[B]) : List[B] =
            for (x <- xs; y <- f(x)) yield y
        def filter[A](xs : List[A], p : A => Boolean) : List[A] =
            for (x <- xs; if p(x)) yield x
    }

### 泛化的for
for表达式能用在列表和数组上，是因为列表和数组都定义了map, flatMap和filter操作，又因为列表和数组定义了foreach方法，所以对这些数据类型的for循环也可以
除了列表和数组，还有其他类型支持这四种方法从而可以使用for表达式：
Range
Iterator
Stream
Set

要支持for表达式和for循环，需要定义map,flatMap,filter,foreach四个方法，也可以只定义这些方法的子集，从而部分支持for表达式或循环
  * 如果类型只定义了map，可以允许单一生成器组成的for表达式
  * 如果定义了flatMap和map，可以允许若干生成器组成的for表达式
  * 如果定义了foreach，允许for循环（可以有单一或若干生成器）
  * 如果定义了filter，for表达式中允许以if开头的过滤表达式


for表达式的转译发生在类型检查之前，只须for表达式展开的结果通过类型检查即可，scala没有定义for表达式自身的类型指定规则。

    abstract class C[A] {
        def map[B](f : A => B) : C[B]
        def flatMap[B] (f : A => C[B]) : C[B]
        def filter(p : A => Boolean) : C[A]
        def foreach(b : A => Unit) : Unit
    }


## 抽取器(Extractors)
scala里的抽取器是具有名为unapply成员方法的对象，通常抽取器对象还会定义可以构建值的对偶方法apply，但并非必须。

    object EMail {
        // 注入方法（可选的）
        def apply(user : String, domain : String) = user + "@" + domain
        // 抽取方法（规定的)
        def unapply(str : String) : Option[(String, String)] = {
            val parts = str split "@"
            // 当把元组作为唯一的参数传递给函数的时候，Some对于元组(user, domain)的应用可以省略一对括号，所以Some(user, domain)与Some((user, domain))同义
            if (parts.length == 2) Some(parts(0), parts(1)) else None
        }
    }

一旦模式匹配碰到的是抽取器对象所指的模式，它就会在选择器表达式中调用抽取器的unapply方法

    // 这里case遇到EMail是一个抽取器，所以会对selectorString调用unapply方法
    // 对EMail.unapply调用返回的或是None，或是Some(u, d)，对于Some类型值来说，u是地址的用户部分，d是域部分，如果是None，说明模式不能匹配，然后
    // 系统尝试其他的模式或失败返回MatcherError异常
    selectorString match {case EMail(user, domain) => ...}

    // 模式匹配器会首先检查给定值x是否符合EMail的unapply方法的参数类型String，如果符合，则x值会转型为String，模式匹配按照之前那样进行，如果不符合，模式立刻失败
    val x : Any = x match {case EMail(user, domain) => ...}

可以不定义注入方法，只定义抽取方法，对象本身被称为抽取器，与是否具有apply方法无关
如果包含了注入方法，那么就应该与抽取方法呈对偶关系：

    EMail.unapply(EMail.apply(user, domain))
    应该返回
    Some(user, domain)

### 0或1个变量的模式


## 使用XML
scala允许在任何可以存在表达式的地方以字面量的形式键入XML，编译过程会进入XML输入模式并把内容读取为XML，直到发现匹配了前面开始标签的结束标签为止。

    // res0 : scala.xml.Ele = ...
    <a>
        This is some XML.
        Here is a tage; <atag/>
    </a>

scala.xml.Ele
scala.xml.Node：所有XML节点类的抽象超类
scala.xml.Text：只包含文本的节点，例如<a>stuff</a>的stuff部分是Text类的
scala.NodeSeq：保存节点序列，可以把单个的Node看作是仅有一个元素的NodeSeq

可以在XML字面量中使用花括号{}做转义符，执行Scala代码

    <a> {"Hello" + ", World!"} </a>

转义字符可以包括任意的scala内容，乃至更多的XML字面量。因此，随着内嵌级别的增加，代码可以在XML与普通Scala代码之间来回切换。

    val yearMade = 1955
    <a> { if (yearMade < 2000) <old>{yearMade}</old> else xml.NodeSeq.Empty }</a>
    // res2 : scala.xml.Elem = <a> <old>1955<old> </a>

转义括号内的表达式并非一定要运行处XML节点才行。它可以返回任何scala值，这种情况下，结果将被转变为字符串，然后以文本节点的形式插入：

    <a> { 3 + 3} </a>
    // res3 : scala.xml.Elem = <a> 6 </a>

如果以打印的方式输出返回节点，那么文本中所有的<, >以及&字符将被转义

    <a> {"</a>potential security hole<a>"} </a>
    // res4 : scala.xml.Elem = <a> &lt;/a&gt;potential security hole&lt;a&gt; </a>

如果用低级字符串操作创建XML,要始终使用XML字面量，而不是字符串添加的方式创建XML

    "<a>" + "</a>potential security hole<a>" + "</a>"
    res5 : String = <a></a>potential security hole<a></a>

### 序列化
要把一个类实例转换为XML，只要添加使用了XML字面量和转义括号的toXML方法

    abstract class CCTherm {
        val description : String
        val yearMade : Int
        val dateObtained : String
        val bookPrice : Int
        val purchasePrice : Int
        val condition : Int

        def toXML =
            <cctherm>
                <decription>{description}</decription>
                <yearMade>{yearMade}</yearMade>
                <dateObatined>{dataObtained}</dataObtained>
                <bookPrice>{bookPrice}</bookPrice>
                <purchasePrice>{purchasePrice}</purchasePrice>
                <condition>{condition}</condition>
            </cctherm>
    }

    val therm = new CCTherm {
        val description  = "hot dog #5"
        val yearMade  = 1952
        val dateObtained = "March 14, 2006"
        val bookPrice = 2199
        val purchasePrice = 500
        val condition = 9
    }
    therm.toXML
    // res6 : scala.xml.Elem =
       <cctherm>
           <decription>hot dog #5</decription>
           <yearMade>1952</yearMade>
           <dateObatined>March 14, 2006</dataObtained>
           <bookPrice>2199</bookPrice>
           <purchasePrice>500</purchasePrice>
           <condition>9</condition>
       </cctherm>

如果向要在XML文本中包含花括号，而不是用于scala代码的转义，只要在一行中写两次花括号即可

    <a> {{{{brace yourself!}}}} </a>
    // res7 : scala.xml.Elem = <a> {{brace yourself!}} </a>

### 拆解XML

   * 抽取文本：通过对XML节点调用text方法可以取回节点内去除了所有元素标签之后的全部文本

        <a>Sounds <tag/> good</a>.text
        // res8 : String = Sounds good
        // 所有的编码字符会自动解码
        <a> input  ----&gt; output </a>.text
        // res9 : String = input ----> output
   * 抽取子元素，如果想要通过标签名找到子元素，只要调用 \ 加标签名即可

        <a><b><c>hello</c></b></a> \ "b"
        // res10 : scala.xml.NodeSeq = <b><c>hello</c></b>
        // 可以用 \\ 替代 \ , 执行深度搜索寻找子元素等内容，\和\\代替XPath的/和//，因为//是scala的注释的开始
        <a><b><c>hello</c></b></a> \ "c"
        // res11 : scala.xml.NodeSeq =
        <a><b><c>hello</c></b></a> \\ "c"
        // res12 : scala.xml.NodeSeq = <c>hello</c>
        <a><b><c>hello</c></b></a> \ "a"
        // res13 : scala.xml.NodeSeq =
        <a><b><c>hello</c></b></a> \\ "a"
        // res14 : scala.xml.NodeSeq = <a><b><c>hello</c></b></a>

   * 抽取属性：可以使用\和\\方法抽取标签属性，只要在属性名之前加上 @ 即可

        val joe = <employee
            name="Joe"
            rank="Code monkey"
            serial="123"/>
        // joe : scala.xml.Elem = <employee rank="code monkey" name="Joe" serial="123"></employee>
        joe \ "@name"
        // res15 : scala.xml.NodeSeq = Joe
        joe \ "@serial"
        // res16 : scala.xml.NodeSeq = 123
### 反序列化

    def fromXML(node : scala.xml.Node) : CCTherm =
        new CCTherm {
            val description = (node \ "description").text
            val yearMade = (node \ "yearMade").text.toInt
            val dateObtained = (node \ "dataObtained").text
            val bookPrice = (node \ "bookPrice").text.toInt
            val purchasePrice = (node \ "purchasePrice").text.toInt
            val condition = (node \ "condition").text.toInt
        }
### 加载和保存
要把XML转换为字符串，可以直接调用toString方法
要把XML转换为字节文件，可以使用XML.saveFull命名。需要选择的部分包括：文件名，要保存的节点及字符编码。第四个参数是决定是否在文件头写上包含字符编码的XML声明
第五个参数是这个XML的文档类型，可以指定null，任由文档类型为未指定

    scala.xml.XML.saveFull("therm1.xml", node, "UTF-8", true, null)

因为文件包含了所有加载器需要知道的东西，只要调用XML.loadFile和文件名即可。

    val loadnode = xml.XML.loadFile("therm1.xml")
    // loadnode : scala.xml.Elem = ...


### XML的模式匹配
XML模式看上去很像XML字面量，主要差别在于如果插入转义符号{}，那么{}内的代码不是表达式而是模式，{}内嵌的模式可以使用所有的scala模式语言，包括绑定新变量，
执行类型检查，以及使用_和_*忽略内容

    def proc(node : scala.xml.Node) : String =
        node match {
            case <a>{contents}</a> => "It's an a: " + contents
            case <b>{contents}</b> => "It's an b: " + contents
            case _ => "It's something else."
        }
如果想要函数能够匹配proc(<a>a <em>red</em> apple</a>)或proc(<a/>)，可以执行对节点序列而不是单个节点的匹配。任意序列XML节点的模式写做 _*。

    def proc(node : scala.xml.Node) : String =
        node match {
            case <a>{contents @ _*}</a> => "It's an a: " + contents
            case <b>{contents @ _*}</b> => "It's a b: " + contents
            case _ => "It's something else."
        }
    proc(<a>a <em>red</em> apple</a>)
    // res21 : String = It's an a: ArrayBuffer(a, <em>red</red>, apple)
    proc(<a/>)
    // res22 : String = It's an a: Array()

XML模式可以与for表达式一起工作

    val catalog = ...
    // 以下会打印空白
    catalog match {
        case <catalog>{therms @ _*}</catalog> =>
            // 这里在每个节点中间还有空白节点，必须要注意
            for (therm <- therms)
                println("processing: " + (therm \ "description").text)
    }

    // 以下不会打印空白
    catalog match {
        case <catalog>{therms @ _*}</catalog> =>
            for (therm @ <cctherm>(_*)</cctherm> <- therms)
                println("processing: " + (therm \ "description").text)
    }


## 使用对象的模块化编程

    class Food(val name : String) {
          override def toString = name
    }
    生成java代码如下：
    public class Food {
      private final java.lang.String name;

      public java.lang.String name() {
            return this.name;
      }
      public java.lang.String toString() {
            return this.name();
      }
      public Food(java.lang.String name) {
            this.name = name;
            super();
      }
    }

    abstract class Food(val name : String) {
       override def toString = name
    }
    生成java代码如下：
     public abstract class Food {
       private final java.lang.String name;

       public java.lang.String name() {
             return this.name;
       }
       public java.lang.String toString() {
             return this.name();
       }
       public Food(java.lang.String name) {
             this.name = name;
             super();
       }
     }

     object Apple extends Food("Apple")
     生成的java代码如下
     public final class Apple {
       public static java.lang.String toString() {
            return Apple$.MODULE$.toString();
       }
       public static java.lang.String name() {
            return Apple$.MODULE$.name();
       }
     }

     public final class Apple$ extends Food {
       public static final Apple$ MODULE$ = new Apple$();
       private Apple$() {
            super("Apple")
            this.MODULE$ = this;
       }
     }

单例对象可以作为一个模块
可以把模板模式的模板代码放在抽象类中，再把模板模式中的不同代码的方法放在object中实现，object继承这个抽象类。
模块常常过于庞大，而不适于放在单个文件中，如果发生这种情况，可以使用特质把模块拆分成多个文件，可以把一个单独的模块放在一个特质当中。然后混入这个特质。

自身类型（self type）；自身类型是在类中提到this时，对于this的假设性类型。自身类型指定了对于特质能够混入的具体类的需求。如果特质仅用于混入另一个或几个特质，
那么可以指定那些假设性的特质。

    trait SimpleFoods {
        object Pear extends Food("Pear")
        def allFoods = List(Apple, Pear)
        def allCategories = Nil
    }

    trait SimpleRecipes {
        this : SimpleFoods =>

        object FruitSalad extends Recipe {
            "fruit salad",
            // 任何混入了SimpleRecipes的具体类都必须同时是SimpleFoods的子类型，则相当于Pear是子类的一个成员。
            List(Apple, Pear),      // 默认情况下，Apple在当前包下，但是Pear在SimpleFoods中，通过自身类型可以访问到，这里的Pear的引用隐含地被认为是this.Pear
            "Mix it all together."
        }

        def allRecipes = List(FruitSalad)
    }


在不同包里实例化同一个类，生成的对象不是同一个对象，会带上它的包，类路径。是如何实现的？？？？

   abstract class A {
        case class Case(name : String)
   }

   class B extends A {
        val b = Case("B")
   }

   class C extends A {
        val c = Case("C")
   }
结尾.type表示它是一个单例类型。单例类型及其特殊，只保存一个对象。如val database : db.type = db


## 对象相等性

重写equals方法时有四个常见陷阱可能会造成不一致：

   1. 定义equals方法时采用了错误的方法签名：  def equals(other : Any) : Boolean （用equals不是用==，用Any不是用具体的类）
   2. 修改了equals方法但没有同时修改hashCode
   3. 用可变字段定义equals方法
   4. 未能按等同关系定义equals方法
根据Scala.Any中的equals的方法的契约规定，equals方法必须对非null对象实现等同关系。
   * 它必须是自反射的，对任何非空值x，表达式x.equals(x)应该返回true
   * 它是对称的，对任何非空值x和y，x.equals(y)当且仅当y.equals(x)返回true时返回true，常用作子类和父类的比较，如果子类有更多的字段，这样父类与子类就不是对称的。
   * 它是可传递的，对任何非空值x，y和z，如果x.equals(y)返回true，且y.equals(z)返回true，则x.equals(z)应返回true
   * 它是一致性的，对任何非空值x和y，多次调用x.equals(y)都应一致的返回true或一致的返回false，只要用于对象的equals比较的信息没有被修改过

一旦类重定义了equals（以及hashCode)，它应该同时明确指出该类的对象不与任何定义了不同相等性方法的超类的对象相等。
通过给每个重定义equals方法的类添加一个canEqual方法来实现: def canEqual(other : Any) : Boolean
如果other对象是重定义了canEqual方法的类的实例，则该方法应该返回true，否则应返回false。equals方法中调用canEqual来确保对象可以双向进行比较。

    class Point(val x : Int, val y : Int) {
        override def hashCode = 41 * (41 + x) + y
        override def equals(other : Any) = other match {
            case that : Point =>
                (that canEqual this) && (this.x == that.x) && (this.y == that.y)
            case _ => false
        }

        def canEqual(other : Any) = other.isInstanceOf[Point]
    }

    class ColoredPoint(x : Int, y : Int, val color : Color.Value) extends Point(x, y) {
        override def hashCode = 41 * super.hashCode + color.hashCode
        override def equals(other : Any) = other match {
            case that : ColoredPoint =>
                (that canEqual this) && (super.equals(that) && this.color == that.color
            case _ => false
        }

        override def canEqual(other : Any) =
            other.isInstanceOf[ColoredPoint]
    }

canEqual的方式违反了LSP原则，因为它不能在超类的地方用子类代替。

### 定义带参数类型的相等性
参数化类型的元素类型在编译器的擦除阶段被抹掉，这些信息在运行期无法被检查
在模式匹配中，要匹配一个类参数，可以使用小写的字母（小写表示变量）或 _(匹配任意值)

    // 模式匹配
    case that : Branch[_]
    // 存在类型简写
    other.asInstanceOf[Branch[_]]

### equals和hashCode的制作方法
  1. 如果在非final类中重写equals方法，应该创建canEqual方法，如果equals的定义是继承自AnyRef（即equals没有在类继承关系的上方被重新定义），则
canEqual的定义将会是新的，否则它将重写之前同名方法的定义。def canEqual(other : Any) : Boolean =
  2. 如果参数对象是当前类的实例，则canEqual方法应该返回true（即canEqual定义所在的类），否则应返回false

        other.isInstance[Rational]
  3. 在equals方法中，记得声明传入的对象类型为Any

        override def equals(other : Any) : Boolean =
  4. 将equals的方法体写为单个match表达式，而match的选择器应为传递给equals的对象:

        other match {
            case that : Ration =>
        }
  5. match表达式应有2个case，第一个声明为定义equals方法的类的类型模式：case that : Rational =>
  6. 在这个case的语句中，编写一个表达式，把两个对象要相等必须为true的独立表达式以逻辑与的方式结合起来。如果重写的equals方法并非是AnyRef的那一个，则要包含对
  超类equals方法的调用 ：

        super.equals(that) &&
  如果为首个引入canEqual的类定义equals方法，应该调用其参数的canEqual方法，将this作为参数传递进去

        (that canEqual this) &&
  重写的equals方法也应该包含canEqual的调用，除非它们包含了对super.equals的调用。在后面这个情形中，canEqual测试已经会在超类调用中完成。最后，对每个与相等
  性相关的字段，验证本对象的字段与传入对象的字段是相等的。

        number == that.number && denom == that.denom
  7. 对第二个case，用一个通配的模式返回false

        case _ => false

hashCode方法的要求：将对象中用在equals方法里计算相等性的每个字段（是为相关字段）都包含进来。对每个相关字段，不管类型是什么，都可以调用hashCode来计算出一个哈希码。
为计算整个对象的哈希码，对第一个字段的哈希码加上41，再乘以41，再加上第二个字段的哈希码，再乘以41，再加上第三个字段的哈希码，再乘以41，如此继续下去。

        // 5个名为a, b, c, d, e的相关字段的对象哈希码
        override def hasCode : Int =
            41 * (
                41 * (
                    41 * (
                        41 * (
                            41 * a.hashCode
                        ) + b.hashCode
                    ) + c.hashCode
                ) + d.hashCode
            ) + e.hashCode

也可以不对Int，Short，Byte和Char字段调用hashCode，Int的哈希码是Int的值，Short，Byte和Char的哈希码也会自动变宽为Int
之所以选择41作为乘数是因为它是个奇质数，可以用别的数字，不过仍应该是奇质数，以防最小化溢出时潜在的信息丢失。给最里层的值加上41是为了减少第一个乘法得到0的可能性，
这是假定第一个字段更有可能是0而非-41，选择41只是为了好看，也可以用任何非0的整数。

如果equals方法将super.equals(that)调用作为其计算的一部分，应该以调用super.hashCode开始hashCode计算。

    override def hashCode : Int =
         41 * (
            41 * (
                super.hashCode
            ) + number
         ) + denom

对于Array，它们在计算哈希码时并不会考虑元素，因此，对数组而言，应该将每个元素当做是对象的字段，主动调用对每个元素的hashCode，或者将数组传递给单例对象
java.util.Arrays的某一个hashCode方法，如果字段是List,Set,Map或元组，这些类的equals和hashCode方法被重写过，会考虑包含的元素。

如果发现一个特定的哈希码计算影响到程序的性能，可以考虑将哈希码缓存起来。如果对象是不可变的，可以在对象创建时计算哈希码并保存到一个字段中，可以简单的通过用
val而不是用def重写hashCode来做到

    override val hashCode : Int =
        41 * (
            41 * number
        ) + denom

java.util.HashMap好像有缓存hashCode？判断耗用时间比较多的程序是否都可以用缓存？？？

可以将可比较的对象的类定义为样本类，这样，scala编译器会自动地添加正确的符合各项要求的equals和hashCode方法。

## 结合Scala和Java

### 在java中使用scala
scala的类，方法，字符串，异常等都和它们在java中的对应概念一样编译成相同的java字节码

   * 值类型：类似Int这样的值类型翻译成java有两种不同的方式，只要可能，编译器会将Scala的Int翻译为java的int以获得更好的性能，
   如果编译器不确定某个对象是不是值类型，而是会使用对象并依赖相应的包装类，如java.lang.Integer这样的包装类允许一个值类型被包装在java对象中。
   * 单例对象：Scala对单例对象的翻译采用了静态和实例方法相结合的方式。对每一个Scala单例对象，编译器都会为这个对象创建一个名称后面加美元符号的java类。
   这个类拥有Scala单例对象的所有方法和字段，这个Java类同时还有一个名为MODULE$的静态字段，保存该类在运行期创建的一个实例.

           object App {
                def main(args : Array[String]) {
                    println("Hello, World!")
                }
           }
           // scala会生成一个java类App$
           public final class App$ extends java.lang.Object implements scala.ScalaObject {
                public static final App$ MODULE$;
                public static {};
                public App$();
                public void main(java.lang.String[]);
                public int $tag();
           }

           // 如果没有一个名为App的类，Scala就会生成一些静态方法转发到App$的方法
           // 如果有一个名为App的类，Scala会创建一个相应的App类保存定义的App类的成员，它就不会添加任何转发到同名单例对象的方法，Java代码必须通过MODULE$字段来访问这个单例。
           public final class App extends java.lang.Object {
                public static final int $tag();
                public static final void main(java.lang.String[]);
           }
   * 作为接口的特质：编译任何特质都会创建一个同名的java接口，这个接口可以作为java类型使用，可以通过这个类型变量来调用scala对象的方法。可以用scala语法来编写java接口

### 注解
有一些注解编译器在针对java平台编译时会产生额外的信息，当编译器看到这样的注解时，会首先根据一般的scala原则去处理，然后针对java做一些额外的工作

   * 过期：对任何标记为@deprecated的方法或类，编译器会为产出的代码添加java自己的过期注解。因此，java编译器能够在java代码访问过期scala方法时给出过期警告。
   * volatile字段：scala中标记为@volatile的字段会在产出的代码中添加java的volatile修饰符。因此，scala中的volatile字段与java的处理机制完全一致。而对volatile字段
   的访问也是完全根据java内存模型所规定的volatile字段处理原则来进行排列的。
   * 序列化：scala的三个标准序列化注解全部都翻译成java中对等的语法结构。@serializable类会被加上java的Serializable接口。@SerialVersionUID(1234L)
   会被转换成如下Java字段定义：

        private final static long SerialVersionUID = 1234L;
   任何标记为@transient的变量会加上Java的transient修饰符。

        看有人把volatile写成了transient，却不能测试出来，volatile如何才能测试出来呢？？？？

Scala并不检查抛出的异常是否被代码捕获，就是说，scala的方法并没有与java中的throws声明向对应的定义。所以scala方法都被翻译成没有申明抛出异常的java方法。
java字节码的验证器不会检查这些声明，java编译器会检查，但验证器不会。

scala这样做是因为在java中，经常会有捕获了异常但是抛出异常的处理，仅仅是为了编译通过
在与java的对接中，可能会需要编写描述方法可能会抛出哪些异常的对java友好的注解。

    import java.io._
    class Reader(fname : String) {
        private val in = new BufferedReader(new FileReader(fname))

        @throw(classOf[IOException]
        def read() = in.read()
    }
    // 翻译成java代码如下：
    public class Reader extends java.lang.Object implements scala.ScalaObject {
        public Reader(java.lang.String);
        public int read() throws java.io.IOException;
        public int $tag();
    }

### java注解
java框架中的注解可以直接在scala代码中使用。任何java框架都会看到编写的注解，就好像是用java编写的一样。

    import org.junit.Test
    import org.junit.Assert.assertEquals

    class SetTest {

        @Test
        def testMultiAdd {
            val set = Set() + 1 + 2 + 3 + 1 + 2 + 3
            assertEquals(3, set.size)
        }
    }

    // scala -cp junit-4.3.1.jar:. org.junit.runner.JUnitCoreSetTest


### 编写自己的注解
为了让注解对java反射可见，必须用java的语法编写并用javac编译。

    import java.lang.annotation.*;
    @Retention(RetentionPolicy.RUNTIME)
    @Target(ElementType.METHOD)
    public @interface Ignore {}

    // 使用注解
    object Tests {
        @Ignore
        def testData = List(0, 1, -1, 5, -5)
    }

在使用java注解时，必须遵循它们所规定的限制，如：在注解参数中，只能使用常量，而不能使用表达式

### 存在类型
存在类型的通用形式如下：

        type forSome {declarations}
type部分是任意的scala类型，而declarations部分是一个抽象val和type的列表。这个定义解读为：声明的变量和类型是存在但未知的，正如类中的抽象成员那样。
这个类型进而被允许引用这些声明的变量和类型，虽然编译器并不知道它们具体指向什么。

    // java中的Iterator<?>写为
    Iterator[T] forSome {type T}
    // 这是一个T的Iterator，而T是某种类型T，具体T是什么未知，可以是任何类型，不过对这个特定的Iterator而言，它是固定不变的。

    // JAVA中的Iterator<? extends Component>写为
    Iterator[T] forSome {type T <: Component}

    // Iterator[_] 的含义与Iterator[T] forSome {type T}相同


如果在可以使用类型的地方使用下划线，则scala会做出一个存在类型。每个下划线在forSome语句中变成一个类型参数，
因而，如果在同一个类型中使用两个下划线，则得到与在forSome语句中使用两个类型相同的效果。

还可以在使用占位符语法是插入上界和下界，只需要简单地给下划线加上它们，而不必在forSome语句中这样做。

    // Iterator[_ <: Component] 与 Iterator[T] forSome {type T <: Component}一样

主要用在scala与java中的通配符交互中。在scala中一般不使用存在类型，而使用抽象类型的type Elem，然后由具体的子类去定义这个Type Elem = T

## Actor和并发
actor是一个类似线程的实体，它有一个用来接收消息的邮箱。实现actor的方法是继承scala.actors.Actor并完成其act方法.

    import scala.actors._

    object SillyActor extends Actor {
        def act() {
            for (i <- 1 to 5) {
                println("I'm acting!")
                Thread.sleep(1000)
            }
        }
    }

    // 通过调用actor的start方法来启动它，这个启动java线程类似：
    SillyActor.start()
    // SillyActor这个actor的运行独立于运行命令行的线程，actor在运行时也是相互独立的。

    // 可以用对象scala.actor.Actor中名为actor工具的方法创建actor：
    // 这个actor在定义以后立即启动，无须另外调用start方法
    import scala.actors.Actor._
    val seriousActors = actor {
        for (i <- 1 to 5) {
            println("That is the question.")
            Thread.sleep(1000)
        }
    }

Actor通过相互发送消息的方式来通信，可以使用 ! 方法来发送消息：SillyActor ! "hi there"

    val echoActor = actor {
        while (true) {
            // 与match语法类似
            receive {
                case msg => println("received message: " + msg)
            }
        }
    }

当actor发送消息时，它不会阻塞，而当actor接收到消息时，它也不会被打断。发送的消息在接收actor的邮箱中等待处理，直到actor调用receive方法。

    echoActor ! "hi there";   echoActor ! 15

偏函数(PartialFunction特质的实例）并不是完整的函数，也就是说，它并不对所有的输入值都有定义。除了接受单个参数的apply方法之外，偏函数还提供一个isDefinedAt方法，
同样只接受单个参数，如果偏函数能够处理传给isDefinedAt的值，则这个方法返回true。这个的值传递给apply是安全的，如果传递给apply的值会让isDefinedAt方法返回false，apply则会抛出一个异常。

actor只会处理传给receive方法的偏函数中的某个样本相匹配的消息。对邮箱中的每个消息，receive都会先调用传入的偏函数isDefinedAt方法来决定它是否与某个样本匹配，然后处理该消息。
receive方法将选定邮箱中第一个让isDefinedAt返回true的消息，将这个消息传递给偏函数的apply方法。而偏函数的apply方法将会处理这个消息。
如果邮箱中没有让isDefinedAt返回true的消息，则被调用receive的actor将会阻塞。直到收到匹配的消息。

    val intActor = actor {
        receive {
            case x : Int => println("Got an Int: " + x)
        }
    }

    // intActor ! "Hello"
    // intActor ! Math.Pi
    // intActor ! 12

### 把原生线程当做actor
Actor子系统会管理一个或多个原生线程供自己使用，只要用的是显式定义的actor，就不需要关心它们和线程的对应关系是怎样的。
该子系统也支持：每个原生线程也可以被当做actor来使用，不过不能直接使用Thread.current。因为它并不具备必要的方法。应该使用Actor.self来将当前线程作为actor来查看。

    import scala.actors.Actor._
    self ! "hello"
    self.receive {case x => x}

receiveWithin方法：这个方法可以指定一个以毫秒计的超时时限。如果在解释器命令行中使用receive的话，receive将会阻塞命令行，直到有消息到来，对self.receive而言，这可能永远等下去。

    self.receiveWithin(1000) {case x => x}

### 通过重用线程获得更好的性能
每个actor都必须得到自己的线程，这样每个act方法才能有机会运行。切换线程通常需要数百甚至数千条处理器指令。
react方法：和receive一样，react带一个偏函数,不同的是，react在找到并处理消息后并不返回，它的返回类型是Nothing，它对消息处理器求值，然后就结束了。（react在完成时抛出异常）

由于react方法不需要返回，其实现不需要保留当前线程的调用栈。因此，actor库可以在下一个被唤醒的线程中重用当前的线程。
如果程序中的每个actor都使用react而不是receive的话，理论上只需要一个线程就能满足程序的全部actor的需要。
如果计算机有多个处理核心，actor子系统将在可能的情况下使用足够多的线程充分来利用所有核心。

由于react不返回，接受消息的消息处理器现在必须同时处理消息并执行actor所余下的工作。通常的做法是用一个顶级的工作方法--比如act自身---供消息处理器在完成时调用。

    object NameResolver extends Actor {
        import java.net.{InetAddress, UnKnownHostException}

        def act() {
            react {
                case (name : String, actor : Actor) =>
                    actor ! getIp(name)
                    act()
                case "EXIT" => println("Name resolver exiting.")
                // 这里的偏函数方法体不用加花括号（{}）？
                case msg =>
                    println("Unhandled message: " + msg)
                    act()
            }
        }

        def getIp(name : String) : Option[InetAddress] = {
            try {
                Some(InetAddress.getByName(name))
            } catch {
                case _ : UnKnowHostException => None
            }
        }
    }

    // NameResolver.start()
    // NameResolver ! ("www.scala-lang.org", self)      // 这里向当前线程发送了一个InetAddress的消息
    // self.receiveWithin(0) {case x => x}    // res2 : Any = Some(www.scala-lang.org/128.178.154.102)

    // NameResolver ! ("wwwwwww.scala-lang.org", self)
    // self.receiveWithin(0) {case x => x}  // res4 : Any = None


    // Actor.loop函数重复执行一个代码块，哪怕代码调用的是react
    def act() {
        loop {
           react {
                case (name : String, actor : Actor) =>
                    actor ! getIp(name)
                case msg =>
                    println("Unhandled message: " + msg)
           }
        }
    }

### 良好的actor风格

   * Actor不应阻塞

        val sillyActor2 = actor {
            def emoteLater() {
                val mainActor = self
                actor {
                    // 助手actor，与主actor一起启动，等待1秒，再发消息给主actor，确保主actor先执行
                    Thread.sleep(1000)
                    mainActor ! "Emote"
                }
            }

            var emoted = 0
            emoteLater()

            loop {
                react {
                    case "Emote" =>
                        println("I'm acting!")
                        emoted += 1
                        if (emoted < 5)
                            emoteLater()
                    case msg =>
                        println("Received: " + msg)
                }
            }
        }


        // sillyActor2 ! "hi there"
        Received : hi there
        I'm acting!
        I'm acting!
        I'm acting!
   react的工作原理：
        当调用一个actor的start时，start方法会以某种方式来确保最终会有某个线程来调用那个actor的act方法。如果act方法调用了react，则react方法会在actor
        的邮箱中查找传递给偏函数的能够处理的消息。（和receive一样，传递待处理消息给偏函数的isDefinedAt方法）如果找到一个可以处理的消息，react会安排一个在未来某个时间处理读消息
        的计划并抛出异常。如果它没有找到这样的消息，它会将actor置于冷存储状态，在它通过邮箱收到更多消息时重新激活，并抛出异常。不论哪种情况，react都会以这个异常的方式完成其执行，act方法也
        随之结束。调用act的线程会捕获这个异常，忘掉这个actor，并继续处理其他事务。需要在偏函数中再次调用act方法，或使用某种其他的手段让react再此被调用。（直接调用act或用loop调用react）

   * 只通过消息与actor通信
   * 优选不可变消息：每个act方法实际上被局限在一个线程总中，react虽然可能在不同线程，但有足够的同步机制。
   确保消息对象是线程安全的最佳途径是在消息中只使用不可变对象。任何只有val字段且这些字段只引用到不可变对象的类的实例都是不可变的，一个简单的定义这样的消息类的方式是把它们定义成样本类。
   如果actor发送的是可变的，未同步的对象作为消息，并在此以后永不读写这个对象，也没问题。但这样未来维护者可能出问题。
   可以发送给其他的actor一个可变对象的一个副本，如数组是可变的，可以发送arr.clone或arr.toList数据
   * 让消息自包含

        // 使用样本类代替元组
        import scala.actors.Actor._
        import java.net.{InetAddress, UnKnownHostException}

        case class LookupIp(name : String, respondTo : Actor)
        case class LookupResult(
            name : String
            address : Option[InetAddress]
        )

        object NameResolver2 extends Actor {
            def act() {
                loop {
                    react {
                        case LookupIp(name, actor) =>
                            actor ! LookupResult(name, getIp(name))
                    }
                }
            }
        }

        def getIp(name : String) : Option[InetAddress] = {
            ...
        }

## 连结符解析
C语言解析器：Yacc，Bison
java语言解析器：ANTLR
扫描生成器：Lex,Flex或JFlex
正则语法（regular grammar）
上下文无关语法（context-free grammar）

    expr ::= term {"+" term | "-" term}.
    term ::= factor {"*" factor | "/" factor}.
    factor ::= floatingPointNumber | "(" expr ")".

    // | 表示备选产出，{...}表示重复（0次或多次）。 [...]表示可选项

    import scala.util.parsing.combinator._

    class Arith extends JavaTokenParsers {
        def expr : Parser[Any] = term~rep("+"~term | "-"~term)
        def term : Parser[Any] = factor~rep("*"~factor | "/"~factor)
        def factor : Parser[Any] = floatingPointNumber | "("~expr~")"
    }


从上下文无关语法生成内容：
  * 每个产出都变成一个方法，因此需要在它前面加上def
  * 每个方法的结果类型都是Parser[Any]，因此需要将 ::= 符号修改为 : Parser[Any] =
  * 在语法定义中，顺序的组合是隐含的，但在程序中，它由一个显式的操作符 ~ 表示。因此需要在产出的每两个连续的符号间插入一个 ~。
  * 重复使用rep(...)表示而不是{...}。可选项使用opt(...)表示而不是[...]
  * 位于每个产出最后的句点(.)被去掉---也可以写上一个分号（;）

###　运行解析器

    object ParseExpr extends Arith {
        def main(args : Array[String]) {
            println("input : " + args(0))
            println(parseAll(expr, args(0)))
        }
    }

    // scala ParseExpr "2 * (3 + 7)"
    // [1.12] parsed : ((2~List(....   表示解析到第一行的第12列
    // scala ParseExpr "2 * (3 + 7))"
    // [1.12] failure:

### 基本的正则表达式解析器

    object MyParsers extends RegexParsers {
        val ident : Parser[String] = """[a-ZA-Z_]\w*""".r
    }

### JSON

    value ::= obj | arr | stringLiteral | floatingPointNumber | "null" | "true" | "false".
    obj ::= "{" [members] "}".
    arr ::= "[" [values] "]".
    members ::= member {"," member}.
    member ::= stringLiteral ":" value.
    value ::= value {"," value}.

    import scala.util.parsing.combinator._
    class JSON extends JavaTokenParsers {
        def value : Parser[Any] = obj | arr | stringLiteral | floatingPointNumber | "null" | "true" | "false"
        // repsep(member, ",")解析的是一个逗号分割的member词的序列
        def obj : Parser[Any] = "{"~repsep(member, ",")~"}"
        def arr : Parser[Any] = "["~repsep(value, ",")~"]"
        def member : Parser[Any] = stringLiteral~":"~value
    }

    import java.io.FileReader
    object ParseJSON extends JSON {
        def main(args : Array[String]) {
            val reader = new FileReader(args(0))
            println(parseAll(value, reader)
        }
    }

### 解析器输出

   * 每个写作字符串的解析器(如：“{” 或“:” 或"null")返回解析的字符串本身
   * 正则表达式解析器，如"""[a-zA-Z_]\w*""".r也返回解析后的字符串本身。同样的规则适用于诸如stringLiteral或floatPointNumber等的正则表达式解析器，它们
   继承自JavaTokenParsers特质
   * 顺序组合P~Q返回P和Q两个结果。这些结果通过同样写作~的样本类实例中返回。因此如果P返回“true”而Q返回“?”，那么顺序组合P~Q返回~("true", "?"),打印为(true~?)
   * 备选组合P | Q 返回P或者Q的成功结果
   * 重复项rep(P)或repsep(P, separator)返回所有P的运行结果的列表。
   * 可选项opt(P)返回一个scala的Option类型的实例。如果P成功得到结果R，它就返回Some(R)，如果失败，则返回None

解析器连结符：^^
^^操作符对解析器的结果进行转型。使用这个操作符的表达式的格式为P ^^ f，其中P是解析器而f是函数。P ^^ f解析的句子和P没什么两样。只要P返回某个结果R，P ^^ f的结果就是f(R)

   floatingPointNumber ^^ (_.toDouble)
   "true" ^^ (x => true)

    // "{"~ms~"}"就相当于 ~(~("{", ms), "}")
   def obj : Parser(Map[String, Any]) =
        "{"~repsep(member, ",")~"}"  ^^ {case "{"~ms~"}" => Map() ++ ms}

   // ~操作符产出的结果是同样名为~的样本类实例。以下是Parsers特质的内部类
   case class ~[+A, +B] (x ; A, y : B) {
        override def toString() = "(" + x + "~" + y + ")"
   }

~>和<~解析器连结符：这两个连结符都表示和~一样的顺序组合，不过~>只保留它右操作元的结果，<~只保留它左操作元的结果。

    def obj : Parser[Map[String, Any]] =
        "{"~> repsep(member, ",")  <~"}"  ^^ (Map() ++ _)

~, ^^和|的优先级是从左到右递减的。

    import scala.util.parsing.combinator._

    class JSON1 extends JavaTokeParsers {
        def obj : Parser[Map[String, Any]] =
            "{"~>repsep(member, ",") <~"}" ^^ (Map() ++ _)
        def arr : Parser[List[Any]] =
            "["~>repsep(value, ",") <~"]"
        def member : Parser[(String, Any)] =
            stringLiteral~":"~value ^^ {case name~":"~value => (name, value)}
        // 这里如果是以中缀结尾的话，就不需要加上括号
        def value : Parser[Any] = (
                obj
            |   arr
            |   stringLiteral
            |   floatingPointNumber ^^ (_.toDouble)
            |   "null" ^^ (x => null)
            |   "true"   ^^ (x => true)
            |   "false"  ^^ (x => false)
        )
    }

解析器连结符汇总：
"..."               字面量
"...".r             正则表达式
P~Q                 顺序组合
P<~Q, P~>Q          顺序组合，只保留左或右
P|Q                 备选项
opt(P)              可选项
rep(P)              重复项
repsep(P, Q)        交错在一起的重复项
P ^^ f              结果转换


scala假定任何从句法上是独立语句的两行之间都有一个分号，除非第一行以中缀结尾，或者两行被括在括弧或方括号中。

    // 在这种情况下，就不需要在value解析器主体之外加上括弧了。
    def value : Parser[Any] =
        obj |
        arr |
        stringLiteral |

    def value : Parser[Any] =
            obj    // 这里由于不是括在括弧或方括号中，也不是以中缀结尾，所以编译器默认会加上分号(;)
         |  arr


在符号名称与由字母和数字组成的名称之间进行选择

   * 在符号名称已经具备普遍含义的情况下使用符号名称。如用+，而不用add
   * 否则，如果想要让代码对普通读者而言更易于理解，则优选由字母和数字组成的名称
   * 仍然可以为特定领域的库选用符号名称，如果这样明显更可读，且并不指望那些不具备该领域扎实基础的普通读者能够立即理解这些代码的话。

连结符解析框架

    package scala.util.parsing.combinator
    trait Parsers {
        //...
    }

解析器定义：

    type Parser[T] = Input => ParseResult[T]

解析器输入：

    type Input = Reader[Elem]
    type Elem

解析器结果

    sealed abstract class ParseResult[+T]
    // 解析器分析输入流的某个部分，然后在结果中返回剩余的部分
    case class Success[T](result : T, in : Input) extends ParseResult[T]
    case class Failure(msg : String, in : Input) extends ParseResult[Nothing]  、

给this起别名

    abstract class Parser[+T] extends (Input => ParseResult[T]) {
        p =>
        //
        def apply(in : Input) : ParseResult[T]
        def ~ ...
        df |  ...
    }
诸如 id =>  这样紧跟在类模板起始花括号之后的语句将标识符id在类中定义为this的别名。可以使用id.m或this.m来访问类的对象级私有成员m，这两种写法完全等同。

    class Outer { outer =>
        class Inner {
            println(Outer.this eq outer)  // 打印：true，都指向外部类的对象
        }

### 单语言符号解析器
Parsers类定义的通用解析器elem可以被用来解析任何单个语言符号：

    def elem(kind : String, p : Elem => Boolean) =
        new Parser[Elem] {
            def apply(in Input) =
                if (p(in.first)) Success(in.first, in.rest)
                else Failure(kind + " excepted", in)
        }

### 顺序组合
~连结符
Scala总是将二元的类型操作如A op B解析为参数化的类型op[A, B]。二元模式P op Q同样被解释为一个函数应用op(P, Q)

    abstract class Parser[+T] .. {p =>
        // 这里T~U是一个更可读的参数化类型~[T, U]的简写。
        def ~ [U] (q : => Parser[U]) = new Parser[T~U] {
            def apply(in : Input) = p(in) match {
                case Success(x, in1) =>
                    q(in1) match {
                        case Success(y, in2) => Success(new ~(x, y), in2)
                        case Failure => failure
                    }
                case Failure => failure
            }
        }

        def <~ [U](q : => Parser[U]) : Parser[T] =
            (p~q) ^^ {case x~y => x}
        def ~> [U](q : => Parser[U]) : Parser[T] =
            (p~q) ^^ {case x~y => y}

###　备选组合
P | Q：如果P成功，则整个解析器成功并返回P的结果，否则则在于P相同的输入上尝试Q，Q的结果就是整个解析器的结果

    def | (q : Parser[T]) = new Parser[T] {
        def apply(in : Input) = p(in) match {
            case s1 @ Success(_, _) => s1
            case failure => q(in)
        }
    }
### 处理递归

    // ~和|的参数q是传名的，如果是传值的，则这个定义读不到任何信息，且会立即造成栈溢出
    def parens = floatingPointNumber | "("~parens~")"

### 结果转换
P ^^ f

    def ^^[U] (f : T => U) : Parser[U] = new Parser[U] {
        def apply(in : Input) = p(in) match {
            case Success(x, in1) => Success(f(x), in1)
            case failure => failure
        }
    }

### 不读取任何输入的解析器

    def success[T](v : T) = new Parser[T]{
        def apply(in : Input) = Success(v, in)
    }

    def failure(msg : String) = new Parser[Nothing] {
        def apply(in : Input) = Failure(msg, in)
    }

### 可选项和重复项

    // 在Parsers特质中
    def opt[T](p : => Parser[T]) : Parser[Option[T]] = (
            p ^^ Some(_)
        |   success(None)
    )

    def rep[T](p : Parser[T]) : Parser[List[T]] = (
            p~rep(p) ^^ {case x~xs => x :: xs}
         |  success(List())
    )

    def repsep[T, U](p : Parser[T]. q : Parser[U])) : Parser[List[T]] = (
            p~rep(q ~>p) ^^ {case r~rs => r :: rs}
         |  success(List())
    )

### 字符串字面量和正则表达式

    // "("~expr~")" 自动扩展为literal("(")~expr~literal(")")
    implicit def literal(s : String) : Parser[String]
    implicit def regex(r : Regex) : Parser[String]

RegexPatters特质同时还处理掉了符号间的空白，如果需要空白，可以重写protected val whiteSpace = """\s+""".r

### 词法分析和解析
词法分析通常分为两个阶段：词法分析器（lexer）阶段识别出输入中的每个单词并将它们归类为不同的语言符号类别，这个阶段也被称为词法分析。
在这之后是句法分析，分析语言符号的序列。句法分析有时也被称为解析

词法和句法分析的工具：
scala.util.parsing.combinator.lexical
scala.util.parsing.combinator.syntactical

### 错误报告

scala解析库实现了一个简单的启发式算法：在所有失败当中，选择出现在输入最后未知的那一个，解析器选取仍然合法的最长的前缀，然后发出一个错误消息，描述
为什么对这个前缀的解析不能再走得更远了。如果在最后这个位置存在多个失败点，则选择最后被访问到的那一个。

    def value : Parser[Any] = obj | arr | stringLiteral | floatingPointNumber | "null" |
        "true" | "false" | failure("Illegal start of value")

### 回溯 LL(1)
解析器连结符在选择不同的解析器时采用回溯的方式。在表达式P | Q中，如果P失败了，则Q会运行与P相同的输入。哪怕P在失败之前已经解析出了某些语言符号也是如此。
这样同样的语言符号会再次被Q解析。

回溯只对如此公式化语法定义增加了少量限制，以便它能被解析。本质上讲，只需要避免左递归的产出

    expr ::= expr "+" term | term.
总是会失败，因为expr立即调用了自身因此不会继续往前。回溯也会带来巨大的消耗，因为同样的输入可能被解析多次

    expr ::= term "+" expr | term.
如果expr解析器应用到诸如(1 + 2) * 3，首先会尝试第一个选择，当匹配到+符号会失败。然后第2个成功了。这个词句被解析了2次。

通常可以改变语法定义来避免回溯

    expr ::= term ["+" expr].
    expr ::= term {"+" expr}.

可以用~!显式地指定某个语法定义是LL(1)的。这个操作符就像顺序组合~，不过永远不会回溯到未读但已经被解析过的输入元素上。

LARLR解析算法

解析器优化：
解析器不再表示为从输入到解析结果的函数，而是表示为一棵树，每个构建步骤都表示为样本类，举例来说，顺序组合可以用样本类Seq表示，备选项可以用Alt表示，等等
最外面的解析器方法phase可以将解析器的这个符号表示用标准的解析器生成算法，进而转换成高效的解析表。


## GUI编程
import scala.swing._

    import scala.swing._

    object FirstSwingApp extends SimpleGUIApplication {
        def top = new MainFrame {
            // 调用title_=方法，由父类var title定义的
            title = "First Swing App"
            // 调用contents_=方法，由父类var contents定义的
            contents = new Button {
                text = "Click me"
            }
        }
    }


    object SecondSwingApp extends SimpleSwingApplication {

      def top = new MainFrame {
        title = "Second Swing App"
        val button = new Button {
          text = "Click me"
        }

        val label = new Label {
          text = "No button clicks registered"
        }

        contents = new BoxPanel(Orientation.Vertical) {
          contents += button
          contents += label
          border = Swing.EmptyBorder(30, 30, 10, 30)
        }
      }
    }

### 处理事件

在scala中。订阅一个事件源source的方法是调用listenTo(source)，从一个事件源取消订阅的方法deafTo(source)

    object ReactiveSwingApp extends SimpleSwingApplication {

      def top = new MainFrame {
        title = "Reactive Swing App"
        val button = new Button {
          text = "Click me"
        }

        val label = new Label {
          text = "No button clicks registered~"
        }

        contents = new BoxPanel(Orientation.Vertical) {
          contents += button
          contents += label
          border = Swing.EmptyBorder(30, 30, 10, 30)
        }

        listenTo(button)
        var nclicks = 0
        reactions += {
          case ButtonClicked(b) =>
            nclicks += 1
            label.text = "Number of button clicks: " + nclicks
        }
      }


import swing._
import event._
与下面的等价，因为包在scala中是嵌套的。
import scala.swing._
import scala.swing.event._

        import swing._
        import event._

        object TempConverter extends SimpleSwingApplication {

          def top = new MainFrame {
            title = "Celsius/Fahrenheit Converter"
            object celsius extends TextField {columns = 5}
            object fahrenheit extends TextField {columns = 5}

            // FlowPanel表示根据框架的宽度用一行或多行一个接一个地显示所有元素
            contents = new FlowPanel {
              contents += celsius
              contents += new Label(" Celsius = ")
              contents += fahrenheit
              contents += new Label(" Fahrenheit")
              border = Swing.EmptyBorder(15, 10, 10, 10)
            }

            listenTo(celsius, fahrenheit)
            reactions += {
                // 这里用反引号表示某个常量，如果不用的话，会便是成一个变量
                // 如果控件定义成Fahrenheit，以大写开头，则表示常量，不需要加反引号。这就是变量用大写开头的原因，表示会在偏函数的case中用到
              case EditDone(`fahrenheit`) =>
                val f = fahrenheit.text.toInt
                val c = (f - 32) * 5/ 9
                celsius.text = c.toString
              case EditDone(`celsius`) =>
                val c = celsius.text.toInt
                val f = c * 9 / 5 + 32
                fahrenheit.text = f.toString
            }
          }
        }


可以用this(a, b)来调用本身类的apply方法


## scala脚本

#! /bin/sh
exec scala "$0" "$@"
!#
println("Hello, " + args(0) + "!")

windows
::#!
@echo off
call scala %0 %*
goto :eof
::!#


     class M {
        {
            case Some(x) => x
            case None => 0
        }
     }


     会生成一个匿名内部类M$$anonfun$1，这个匿名内部类的构造方法有一个参数就是M，传M传入这个匿名类中。
     这个匿名类继承 scala.runtime.AbstractFunction1<scala.Option<java.lang.Object>, java.lang.Object>
     因此这个匿名类有一个apply方法，接收一个参数scala.Option<java.lang.Object>

    class M {
        val a : PartialFunction[Option[Int], Int] = {
            case Some(x) => x
            case None => 0
        }
    }
    // 生成的匿名类 M$$anonfun$1继承scala.runtime.AbstractPartialFunction$mcIL$sp<scala.Option<java.lang.Object>>
    // 有applyOrElse和isDefinedAt方法




    class M {
      val withDefault : PartialFunction[Option[Int], Int] = {
            case Some(x) => x
            case None => 0
        }
    }
    生成如下java代码
    public class M {
      private final scala.PartialFunction<scala.Option<java.lang.Object>, java.lang.Object> withDefault;

      public scala.PartialFunction<scala.Option<java.lang.Object>, java.lang.Object> withDefault();
        Code:
           0: aload_0
           1: getfield      #14                 // Field withDefault:Lscala/PartialFunction;
           4: areturn

      public M();
        Code:
           0: aload_0
           1: invokespecial #20                 // Method java/lang/Object."<init>":()V
           4: aload_0
           5: new           #22                 // class M$$anonfun$1
           8: dup
           9: aload_0
          10: invokespecial #25                 // Method M$$anonfun$1."<init>":(LM;)V
          13: putfield      #14                 // Field withDefault:Lscala/PartialFunction;
          16: return
    }

    public final class M$$anonfun$1 extends scala.runtime.AbstractPartialFunction$mcIL$sp<scala.Option<java.lang.Object>> implements scala.Serializable {
      public static final long serialVersionUID;

      public final <A1 extends scala/Option<java/lang/Object>, B1 extends java/lang/Object> B1 applyOrElse(A1, scala.Function1<A1, B1>);
        Code:
           0: aload_1
           1: astore_3
           2: aload_3
           3: instanceof    #21                 // class scala/Some
           6: ifeq          35
           9: aload_3
          10: checkcast     #21                 // class scala/Some
          13: astore        4
          15: aload         4
          17: invokevirtual #25                 // Method scala/Some.x:()Ljava/lang/Object;
          20: invokestatic  #31                 // Method scala/runtime/BoxesRunTime.unboxToInt:(Ljava/lang/Object;)I
          23: istore        5
          25: iload         5
          27: invokestatic  #35                 // Method scala/runtime/BoxesRunTime.boxToInteger:(I)Ljava/lang/Integer;
          30: astore        6
          32: goto          80
          35: getstatic     #41                 // Field scala/None$.MODULE$:Lscala/None$;
          38: aload_3
          39: astore        7
          41: dup
          42: ifnonnull     54
          45: pop
          46: aload         7
          48: ifnull        62
          51: goto          71
          54: aload         7
          56: invokevirtual #47                 // Method java/lang/Object.equals:(Ljava/lang/Object;)Z
          59: ifeq          71
          62: iconst_0
          63: invokestatic  #35                 // Method scala/runtime/BoxesRunTime.boxToInteger:(I)Ljava/lang/Integer;
          66: astore        6
          68: goto          80
          71: aload_2
          72: aload_1
          73: invokeinterface #53,  2           // InterfaceMethod scala/Function1.apply:(Ljava/lang/Object;)Ljava/lang/Object;
          78: astore        6
          80: aload         6
          82: areturn

      public final boolean isDefinedAt(scala.Option<java.lang.Object>);
        Code:
           0: aload_1
           1: astore_2
           2: aload_2
           3: instanceof    #21                 // class scala/Some
           6: ifeq          14
           9: iconst_1
          10: istore_3
          11: goto          48
          14: getstatic     #41                 // Field scala/None$.MODULE$:Lscala/None$;
          17: aload_2
          18: astore        4
          20: dup
          21: ifnonnull     33
          24: pop
          25: aload         4
          27: ifnull        41
          30: goto          46
          33: aload         4
          35: invokevirtual #47                 // Method java/lang/Object.equals:(Ljava/lang/Object;)Z
          38: ifeq          46
          41: iconst_1
          42: istore_3
          43: goto          48
          46: iconst_0
          47: istore_3
          48: iload_3
          49: ireturn

      public final boolean isDefinedAt(java.lang.Object);
        Code:
           0: aload_0
           1: aload_1
           2: checkcast     #62                 // class scala/Option
           5: invokevirtual #66                 // Method isDefinedAt:(Lscala/Option;)Z
           8: ireturn

      public final java.lang.Object applyOrElse(java.lang.Object, scala.Function1);
        Code:
           0: aload_0
           1: aload_1
           2: checkcast     #62                 // class scala/Option
           5: aload_2
           6: invokevirtual #70                 // Method applyOrElse:(Lscala/Option;Lscala/Function1;)Ljava/lang/Object;
           9: areturn

      public M$$anonfun$1(M);
        Code:
           0: aload_0
           1: invokespecial #72                 // Method scala/runtime/AbstractPartialFunction$mcIL$sp."<init>":()V
           4: return
    }

