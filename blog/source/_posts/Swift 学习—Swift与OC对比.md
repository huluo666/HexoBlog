---
title: Swift 学习—Swift与OC对比
tags: iOS
categories: iOS
date: 2017-12-28 16:22:26
---

[iOS:OC开发中的写法与Swift中写法的对比](https://www.jianshu.com/p/08fb33a346c6)

[Swift 和 OC 对比](https://www.jianshu.com/p/4a99b5405e11)

函数OC

```objectivec
//不带参数
- (void)say{
    NSLog(@"hello");
}
//带有一个参数
- (void)sayWithName:(NSString *)name{
    NSLog(@"hello %@", name);
}

//带有多个参数
- (void)sayWithName:(NSString *)name age:(NSInteger)age{
    NSLog(@"hello %@ , I'm %tu years old", name, age);
}

//有返回值
- (NSString *)info{
    return @"name = CDH, age = 20";
}

//有返回值,并且带有返回值
- (NSString *)infoWithName:(NSString *)name age:(NSInteger)age{
    return [NSString stringWithFormat:
    @"name = %@,
    age = %tu", name, age];
}
```



Swift

```swift
//无参无返回值
func say() -> Void{
    print("hello")
}
say()
//输出结果: hello

func say1() {  //如果没有返回值可以不写
        print("hello")
}
say1()
//输出结果: hello

//有参无返回值
func sayWithName(name:String){
    print("hello \(name)")
}
sayWithName("CDH")
//输出结果: hello CDH

//带有多个参数
func sayWithName(name:String, age:Int){
    print("hello \(name) , I'm \(age) years old ")
}
sayWithName("CDH", age: 20)
//输出结果: hello CDH , I'm 20 years old

//无参有返回值
func info() -> String{
    return "name = cdh, age = 20"
}
print(info())
//输出结果: name = cdh, age = 20

//有参有返回值
func info(name:String, age:Int) -> String{
    return "name = \(name), age = \(age)"
}
print(info("cdh", age:20))
//输出结果: name = cdh, age = 20
```
