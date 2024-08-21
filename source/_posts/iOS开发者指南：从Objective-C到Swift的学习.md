---
title: iOS开发者指南：从objectivec到Swift的学习
tags: Swift
categories: iOS
date: 2017-01-28 14:55:56
grammar_cjkRuby: true
---

iOS开发者指南：从objectivec到Swift的学习【译】

原文 https://www.toptal.com/swift/from-objectivec-to-swift

​     2008年，苹果宣布并发布了iPhone SDK 2.0。这一事件又引发了软件开发的又一次革命，诞生了一批新的开发者。他们现在被认可为iOS开发者。

其中许多开发者以前从未使用过objectivec，这是苹果公司向他们提出的第一个挑战。尽管不熟悉的语法和手动内存管理，但它是非常成功的，帮助成千上万的应用程序上架到App Store。每个版本中，Apple都不断改进objectivec，添加代码块blocks和常量，通过自动引用计数简化内存管理，以及许多指示现代编程语言的其他功能。

​	经过6年对objectivec的改进和工作，苹果决定向开发者提出另一个挑战。 iOS开发者再一次需要学习一门新的编程语言：Swift。 Swift删除了不安全的指针管理，并引入了强大的新功能，同时保持与objectivec和C的交互。

​	Swift 1.0已经是一个稳定而强大的开发平台，在未来几年肯定会以有趣的方式发展。开始探索这个新语言是一个完美的时刻，因为它显然是iOS开发的未来。

本教程的目的是让objectivec开发人员快速了解新的Swift语言特性，帮助您进行下一步，并在日常工作中使用Swift。我不会花太多时间来解释objectivec，我将假定您熟悉iOS开发。

#### 尝试使用Swift VS objectivec

为了开始探索Swift，你需要做的就是从App Store下载Xcode，创建一个小程序来实验。本文中提到的所有示例都是这样完成的。

苹果的Swift主页是学习Swift编程的最佳参考。你会发现它是非常宝贵的，直到你完全按照Swift的发展速度，我相信你会经常回到这里。

#### 变量和常量

在Swift中声明变量是使用var关键字完成的。

```swift
var x = 1
var s = "Hello"
```

你会注意到两个变量x和s是不同的类型。 `x`是一个整数(Integer)，而`s`是一个字符串(String)。 Swift是一种类型安全的语言，它将从分配的值中推导出变量类型。如果你想让你的代码更具可读性，你可以选择注释变量的类型：

```swift
var y: Int
y = 2
```

常量是相似的，但是用`let`而不是`var`声明它们。常量的值不需要在编译时知道，但是你必须给它赋值一次。

```swift
let c1 = 1  //在编译时已知的常量

var v = arc4random()
let c2 = v // 只在运行时才知道的常量(Constant)
```

顾名思义，它们是不可变的，所以下面的代码会导致编译时错误。

```swift
let c = 1
c = 3        // error Connot assign to value:'c' is a 'let' constant
```

其他类型也可以声明为常量。例如，下面的代码将数组声明为一个常量，如果您尝试修改任何元素，那么Swift编译器将报告一个错误：

```swift
var arr2 = [4, 5, 6]
arr2[0] = 8
print (arr2)    // [8, 5, 6]

let arr = [1, 2, 3]
a[0] = 5    // error Use of unresolved identifier 'a'
```



#### 可选(Optionals)类型

常量声明时需要初始化，变量需要在使用前初始化。 那么objectivec中`nil`等价在哪呢？ Swift引入了可选值。 可选的值可以有一个值或`nil`。 如果你看看下面的代码，你会注意到`x`被指定了一个2014的可选值。这意味着Swift编译器意识到x也可能是`nil`。

```swift
var s = "2014"
var x = Int(s)
print(x)    // Optional(2014)
```

如果你在这个代码中进行了修改，并且把值`“abc”`赋值给`s`，而这个值不能被转换成Integer，你会注意到`x`现在是`nil`。

```swift
var s = "abc"
var x = Int(s)
print(x)    // nil
```

 `Int()` 函数的返回类型是`Int`？，它是一个可选的`Int`。让我们试着在`x`上调用一个标准函数：

```swift
var x = Int("2014")
print(x.successor()) // error
```

编译器发出一个错误信号，因为x是可选的，可能是`nil`。我们必须首先测试`x`，并确保后继函数在实数上被调用，而不是`nil`：

```swift
var x = Int("2014")
if x != nil
{
  print(x!.successor())    // 2015
}
```

**请注意，我们必须通过附加感叹号 (!)来解包`x`**。 当我们确定x包含一个值时，我们可以访问它。 否则，我们将得到一个运行时错误。 我们也可对Swift调用进行**可选绑定**，将可选变量转换为非可选变量

```swift
let x = "123".toInt()
if let y = x
{
  print(y)
}
```

if语句中的代码只会在`x`有值时执行，并将其赋值给`y`。 请注意，我们不需要展开`y`，它的类型不是可选的，因为我们知道`x`不是`nil`。

苹果的Swift教程，阅读关于可选和新特性等更多细节

[Optional Chaining](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/OptionalChaining.html#//apple_ref/doc/uid/TP40014097-CH21-XID_356)



#### 字符串插值（拼接）

在objectivec中，字符串通常用`stringWithFormat：`方法完成：

```swift
NSString *user = @"Gabriel";
int days = 3;
NSString *s = [NSString stringWithFormat:@"posted by %@ (%d days ago)", user, days];
```

您也可以使用表达式:

```swift
let width = 2
let height = 3
let s = "Area for square with sides \(width) and \(height) is \(width*height)"
```

了解更多关于swift的字符串插值和其他新功能, [请点击这里 ](https://developer.apple.com/library/ios/documentation/swift/conceptual/swift_programming_language/StringsAndCharacters.html#//apple_ref/doc/uid/TP40014097-CH7-XID_418)。

#### 函数

Swift中的函数定义与C不同。函数定义如下：

```swift
func someFunction(s:String, i: Int) -> Bool
{
    ...    // code
}
```

Swift函数是一流的类型。这意味着您可以将函数分配给变量，将它们作为参数传递给其他函数，或者使它们返回类型：

```swift
func stringLength(s:String) -> Int
{
    return countElements(s)
}

func stringValue(s:String) -> Int
{
    if let x = s.toInt()
    {
        return x
    }
    return 0
}

func doSomething(f:String -> Int, s:String) -> Int
{
    return f(s).successor()
}

let f1 = stringLength
let f2 = stringValue

doSomething(f1, "123")    // 4
doSomething(f2, "123")    // 124
```

同样，Swift推断f1和f2（String - > Int）的类型，虽然我们可以明确地定义它们：

```swift
let f1:String -> Int = stringLength
```

函数也可以返回其他函数：

```swift
func compareGreaterThan(a: Int, b: Int) -> Bool
{
    return a > b
}

func compareLessThan(a: Int, b: Int) -> Bool
{
    return a < b
}

func comparator(greaterThan:Bool) -> (Int, Int) -> Bool
{
    if greaterThan
    {
        return compareGreaterThan
    }
    else
    {
        return compareLessThan
    }
}

let f = comparator(true)
println(f(5, 9))
```

Swift中的函数指南可以在[这里](https://developer.apple.com/library/ios/documentation/swift/conceptual/swift_programming_language/Functions.html#//apple_ref/doc/uid/TP40014097-CH10-XID_243)找到。



#### 枚举（Enumerations）

Swift中的枚举比objectivec中的枚举强大得多。 作为Swift的结构体(structs)，他们可以有方法，也可以传值

```swift
enum MobileDevice : String
{
    case iPhone = "iPhone", Android = "Android", WP8 = "Windows Phone8", BB = "BlackBerry"

    func name() -> String
    {
        return self.toRaw()
    }
}
let m = MobileDevice.Android
print(m.name())    // "Android"
```

与objectivec不同，swift枚举可以为每个成员赋值字符串(Strings)，字符(characters)或浮点数(floats)，除了整数(integers)。`toraw()`方法很方便的返回分配给每个成员的值。

枚举也可以参数化:

```swift
enum Location
{
    case Address(street:String, city:String)
    case LatLon(lat:Float, lon:Float)

    func description() -> String
    {
        switch self
        {
        case let .Address(street, city):
            return street + ", " + city
        case let .LatLon(lat, lon):
            return "(\(lat), \(lon))"
        }
    }
}

let loc1 = Location.Address(street: "2070 Fell St", city: "San Francisco")
let loc2 = Location.LatLon(lat: 23.117, lon: 45.899)
print(loc1.description())        // "2070 Fell St, San Francisco"
print(loc2.description())        // "(23.117, 45.988)"
```

更多关于枚举可用的信息 [在这里 ](https://developer.apple.com/library/ios/documentation/swift/conceptual/swift_programming_language/Enumerations.html#//apple_ref/doc/uid/TP40014097-CH12-XID_223)。

#### 元组（Tuples）

元组将多个值组合为一个复合值。元组中的值可以是任何类型，不必是彼此相同的类型。

```swift
let person = ("Gabriel", "Kirkpatrick")
print(person.0) // Gabriel
```

你也可以命名单个的元组元素：

```swift
let person = (first: "Gabriel", last: "Kirkpatrick")
print(person.first)
```

元组对于需要返回多个值的函数返回类型非常方便：

```swift
func intDivision(a: Int, b: Int) -> (quotient: Int, remainder: Int)
{
    return (a/b, a%b)
}
print(intDivision(11, 3))    // (3, 2)
let result = intDivision(15, 4)
print(result.remainder)    // 3
```

不像objectivec，swift支持switch语句中的模式(pattern)匹配：

```swift
let complex = (2.0, 1.1)    // real and imaginary parts

switch complex
{
    case (0, 0):
        println("Number is zero")
    case (_, 0):
        println("Number is real")
    default:
        println("Number is imaginary")
}
```

在第二种情况下，我们不关心数字的实际部分，所以我们使用`_`来匹配任何东西。您还可以检查每种情况下的附加条件。为此，我们需要将模式值绑定到常量：

```swift
let complex = (2.0, 1.1)

switch complex
{
    case (0, 0):
        println("Number is zero")
    case (let a, 0) where a > 0:
        println("Number is real and positive")
    case (let a, 0) where a < 0:
        println("Number is real and negative")
    case (0, let b) where b != 0:
        println("Number has only imaginary part")
    case let (a, b):
        println("Number is imaginary with distance \(a*a + b*b)")
}
```

注意我们只需要绑定我们要在比较或case语句中使用的值。阅读更多关于元组，[点击这里](https://developer.apple.com/library/prerelease/mac/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-XID_483)。

#### 类(Classes)和结构(Structures)

与objectivec不同，swift不需要为自定义的类和结构创建单独的接口和实现文件。当你学习Swift，你将学会在一个单独的文件中定义一个类或者一个结构体，并且这个类或者结构体的外部接口被自动地提供给其他代码使用。

### 定义类（Classes）

类定义很简单:

```swift
class Bottle
{
   var volume: Int = 1000

   func description() -> String
   {
       return "This bottle has \(volume) ml"
   }
}
let b = Bottle()
print(b.description())
```

正如你所看到的，声明(header)和实现(implementation)在同一个文件中。swift不再使用头文件和实现文件。让我们给我们的例子添加一个标签：

```swift
class Bottle
{
   var volume: Int = 1000
   var label:String

   func description() -> String
   {
       return "This bottle of \(label) has \(volume) ml"
   }
}
```

编译器会抱怨,因为是一个可选的变量标签和瓶时不会持有一个值实例化。 我们需要添加一个初始化程序:

编译器会抱怨，因为标签是一个非可选的变量，并且当一个Bottle被实例化时不会保存一个值。我们需要添加一个初始化程序(initializer)：

```swift
class Bottle
{
   var volume: Int = 1000
   var label:String

   init(label:String)
   {
       self.label = label
   }

   func description() -> String
   {
       return "This bottle of \(label) has \(volume) ml"
   }
}
```

或者，我们可以使用可选类型的属性，它不会被初始化。在下面的例子中，我们将卷设为一个可选的整数：

```swift
class Bottle
{
   var volume: Int?
   var label:String

   init(label:String)
   {
       self.label = label
   }

   func description() -> String
   {
        if self.volume != nil
        {
               return "This bottle of \(label) has \(volume!) ml"
           }
           else
           {
               return "A bootle of \(label)"
           }
   }
}
```

### 结构(Structures)

Swift语言也有 `结构体`,但是比objective - c更加灵活。 下面的代码教程定义了一个结构体：

```swift
struct Seat
{
    var row: Int
    var letter:String

    init (row: Int, letter:String)
    {
        self.row = row
        self.letter = letter
    }

    func description() -> String
    {
        return "\(row)-\(letter)"
    }
}
```

像swift中的类，结构可以有方法，属性，初始值设定项，并符合协议。类和结构之间的主要区别是类是通过引用传递的，而结构则是通过值传递的。

这个例子演示了通过引用传递类：

```swift
let b = Bottle()
print(b.description())    // "b" bottle has 1000 ml

var b2 = b
b.volume = 750
print(b2.description())    // "b" and "b2" bottles have 750 ml
```

如果我们用struct来尝试类似的情况，你会注意到变量是按值传递的：

```swift
var s1 = Seat(row: 14, letter:"A")
var s2 = s1
s1.letter = "B"
print(s1.description())    // 14-B
print(s2.description())    // 14-A
```

 什么时候应该使用struct，什么时候应该使用class？

​       就像在objectivec和c中一样，当你需要对几个值进行分组时，使用结构体，并期望它们被复制而不是被引用。 例如，复数，，2d或3d点或RGB颜色。

​       一个类的实例传统上被称为对象。 然而，Swift类和结构在功能上比其他语言更接近，而且许多功能可以应用于类或结构类型的实例。 正因为如此，在Swift引用中使用的更一般的术语是实例(instance)，它适用于这两者中的任何一个。

### 属性(Properties)

正如我们前面看到的，Swift中的属性在类或结构定义中用`var`关键字声明。 我们也可以用`let`语句声明常量。

```swift
struct FixedPointNumber
{
    var digits: Int
    let decimals: Int
}

var n = FixedPointNumber(digits: 12345, decimals: 2)
n.digits = 4567    // ok
n.decimals = 3     // error, decimals is a constant
```

另外请记住，强类型属性是强引用的，除非用weak关键字作为前缀。 但是，有一些weak的非可选属性，所以请阅读苹果Swift指南中的[自动引用计数章节](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/AutomaticReferenceCounting.html#//apple_ref/doc/uid/TP40014097-CH20-XID_88)。

### 计算属性(Computed Properties)

计算属性实际上并不存储一个值。 相反，它们提供了一个getter和一个可选的setter来间接检索和设置其他属性和值。

以下代码提供了一个计算`sign`值的示例：

```swift
enum Sign
{
    case Positive
    case Negative
}

struct SomeNumber
{
    var number:Int
    var sign:Sign
    {
        get
        {
            if number < 0
            {
                return Sign.Negative
            }
            else
            {
                return Sign.Positive
            }
        }

        set (newSign)
        {
            if (newSign == Sign.Negative)
            {
                self.number = -abs(self.number)
            }
            else
            {
                self.number = abs(self.number)
            }
        }
    }
}
```

我们也可以通过执行一个getter来定义只读属性：

```swift
struct SomeNumber
{
    var number:Int
    var isEven:Bool
    {
        get
        {
            return number % 2 == 0
        }
    }
}
```

在objectivec中，属性通常由一个实例变量持有，由编译器明确地声明或自动创建。 在Swift中，另一方面，一个属性没有相应的实例变量。 也就是说，属性的后备存储不能直接访问。 假设我们在objectivec中有这个

```swift
// .h
@interface OnlyInitialString : NSObject

@property(strong) NSString *string;

@end

// .m

@implementation OnlyInitialString

- (void)setString:(NSString *newString)
{
    if (newString.length > 0)
    {
        _string = [newString substringToIndex:1];
    }
    else
    {
        _string = @"";
    }
}

@end
```

因为在Swift中，计算属性没有后备存储，所以我们需要这样做：

```swift
class OnlyInitialString
{
    var initial:String = ""
    var string:String
    {
        set (newString)
        {
            if countElements(newString) > 0
            {
                self.initial = newString.substringToIndex(advance(newString.startIndex, 1))
            }
            else
            {
                self.initial = ""
            }
        }
        get
        {
            return self.initial
        }
    }
}
```

关于属性更详细地解释 [在这里](https://developer.apple.com/library/ios/documentation/swift/conceptual/swift_programming_language/Properties.html#//apple_ref/doc/uid/TP40014097-CH14-XID_368)
