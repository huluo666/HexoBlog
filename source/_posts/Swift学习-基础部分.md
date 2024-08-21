---
title: Swift学习-基础部分
date: 2016-03-28 09:09:25
tags:
categorys:
---

与Swift最新版同步，当前代码版本Swift4.0

```swift
//http://www.swift51.com/swift4.0/chapter2/01_The_Basics.html
import UIKit

//#MARK:- 常量与变量
var str = "Hello, playground"
let a=1 //声明常量
var b=2 //声明变量

//声明多个常量或者多个变量
let x1 = 0.0, y1 = 0.0, z1 = 0.0
var x = 0.0, y = 0.0, z = 0.0
var c:String //声明变量类型
c="hello"


/** 常量和变量的命名*/
//你可以用任何你喜欢的字符作为常量和变量名，包括 Unicode 字符：
let π = 3.14159
let 你好 = "你好世界"
let 🐶🐮 = "dogcow"
let dogcat = "🐶🐹"

var friendlyWelcome = "Hello!"
friendlyWelcome = "Bonjour!"

let languageName = "Swift"
//languageName = "Swift++"
// 这会报编译时错误 - languageName 不可改变
print(friendlyWelcome)
print(languageName)
print("friendlyWelcome is \(friendlyWelcome)")

let message = "\(3) times 2.5 is \(Double(5) * 2.5)" //字符串插值
print("friendlyWelcome is \(message)")
print("friendlyWelcome is \(friendlyWelcome)")

//分号-与其他大部分编程语言不同，Swift 并不强制要求你在每条语句的结尾处使用分号（;），当然，你也可以按照你自己的习惯添加分号。有一种情况下必须要用分号，即你打算在同一行内写多条独立的语句：
let cat = "🐱"; print(cat)

let meaningOfLife = 42
let meaningOfLife2 = 42.000
type(of: meaningOfLife) //int类型
type(of: meaningOfLife2)   //double类型

//#MARK:- 整数和浮点数转换
let three = 3
let pointOneFourOneFiveNine = 0.14159
let pi = Double(three) + pointOneFourOneFiveNine
//pi 等于 3.14159，所以被推测为 Double 类型
let integerPi = Int(pi)
let floatPi = Float(pi)
let i = 1
if i == 1 {
    // 这个例子会编译成功
}

//let i1 = 1
//if i1 {
//    // 这个例子不会通过编译，会报错
//}


//#MARK:- 元组
let http404Error = (404, "Not Found")
// http404Error 的类型是 (Int, String)，值是 (404, "Not Found")

let (statusCode, statusMessage) = http404Error
print("The status code is \(statusCode)")
// 输出 "The status code is 404"
print("The status message is \(statusMessage)")
// 输出 "The status message is Not Found"

//如果你只需要一部分元组值，分解的时候可以把要忽略的部分用下划线（_）标记：
let (justTheStatusCode, _) = http404Error
print("The status code is \(justTheStatusCode)")
// 输出 "The status code is 404"
print("The status code is \(http404Error.0)")
// 输出 "The status code is 404"
print("The status message is \(http404Error.1)")

//定义元组的时候给单个元素命名：
let http200Status = (statusCode: 200, description: "OK",otherInfo:"http")
print("The status code is \(http200Status.statusCode)")
// 输出 "The status code is 200"
print("The status message is \(http200Status.description)")

//String->Int Int->String
let possibleNumber = "123"
let convertedNumber = Int(possibleNumber)
let aNumber = 5
let aNumberString = String(aNumber);


let number = NSNumber(value: aNumber)
let formatter = NumberFormatter()
formatter.minimumIntegerDigits = 2
let strValue3 = formatter.string(from: number)
let strValue4 = String(format:"%03d",aNumber) //保留3位整数，不足补0

//#MARK:- 可选类型
var serverResponseCode: Int? = 404
// serverResponseCode 包含一个可选的 Int 值 404
serverResponseCode = nil
// serverResponseCode 现在不包含值
//nil不能用于非可选的常量和变量。如果你的代码中有常量或者变量需要处理值缺失的情况，请把它们声明成对应的可选类型。
var surveyAnswer: String?
//如果你声明一个可选常量或者变量但是没有赋值，它们会自动被设置为 nil：

//当你确定可选类型确实包含值之后，你可以在可选的名字后面加一个感叹号（!）来获取值。这个惊叹号表示“我知道这个可选有值，请使用它。”这被称为可选值的强制解析（forced unwrapping）：
if convertedNumber != nil {
    print("convertedNumber has an integer value of \(convertedNumber!).")
}

let possibleString: String? = "An optional string."
let forcedString: String = possibleString! // 需要感叹号来获取值
let assumedString: String! = "An implicitly unwrapped optional string."
let implicitString: String = assumedString  // 不需要感叹号

// 可选类型表示允许常量或者变量没有值，即nil，可选类型用？表明
var d: String?    // 可选变量，会自动将其值设置为nil
var e: Int? = 404 // 可选变量，这样可以把nil赋值给该变量
print(d ?? 7)
print(e!)  // 需要!来获取值
// 隐式解析可选类型，用!声明，表明强制要求该变量一定有值。一个隐式解析可选类型其实就是一个普通的可选类型，只是可以被当作非可选类型来使用，如果该变量没有值，那么去获取的时候就会报错
let f: String? = "test"
print(f as Any)    // 不需要!
```

