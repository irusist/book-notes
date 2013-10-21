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
                            return release;
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
            return return  scala.runtime.ScalaRunTime$.MODULE$._toString(this);
      }

      public boolean equals(java.lang.Object object) {
            if (this == object) {
                return true;
            }

            Object o = object;
            if (o instanceOf A)
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
      public scala.Option<scala.Tuple2<java.lang.Object, java.lang.Object>> unapply(A) {

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