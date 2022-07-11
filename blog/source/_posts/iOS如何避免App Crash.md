---
title: iOS如何避免App Crash
date: 2016-05-03 14:26:32
tags: iOS
grammar_cjkRuby: true
---



**获取崩溃信息**

```objectivec
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{    
    NSSetUncaughtExceptionHandler(&uncaughtExceptionHandler);     
}

void uncaughtExceptionHandler(NSException *exception) {

     NSLog(@"CRASH: %@", exception);
     NSLog(@"Stack Trace: %@", [exception callStackSymbols]);         
}
```



```objectivec
-(BOOL) isValueOfKeyIsNull(id response){
        if([response==[NSNull null]]){
            return true;
        }
        return false;
}
```





使用`@try...@catch`避免崩溃

```objectivec
-(void)functionInsideWhichAppIsCrashing
{
    NSArray* arraytest = @[@"1",@"2"];
    @try
    {
        //Your crashing code goes here
        NSLog(@"Object: %@",arraytest[3]);
    }
    @catch (NSException *exception)
    {
        // Print exception information
        NSLog( @"NSException caught" );
        NSLog( @"Name: %@", exception.name);
        NSLog( @"Reason: %@", exception.reason );
    }
    @finally
    {
        // // Do whatever you want to do in crash situation
        NSLog( @"In finally block");
    }
}
```



```objectivec
+ (BOOL)nullValue:(id)value {
    return ((NSNull *)value == [NSNull null] || [@"<null>" isEqualToString:(NSString *)value] || [@"(null)" isEqualToString:(NSString *)value] || value == nil);
}
```



https://github.com/MarkQSchultz/QZObserver

https://github.com/PonyCui/KVSafe

https://github.com/wuwen1030/XTSafeCollection

https://github.com/jasenhuang/NSObjectSafe

https://github.com/deput/RWJSONAid

https://github.com/xuvw/HKSafePush

https://github.com/bigParis/SwizzlingMethod

https://github.com/LQQZYY/safetyDictionaryDemo/tree/master/NSDictionaryDemo

https://github.com/oenius/YGSafeKVO/tree/master/YGKVO

https://github.com/allenhsu/NSDictionary-Accessors

https://github.com/shaojiankui/NSDictionary-SafeAccess

https://github.com/gdier/SafeKVObject

https://github.com/fengchuanxiang/SafeCollections

https://github.com/LuKing4DB/SafeEX

https://github.com/MarkQSchultz/QZObserver

https://github.com/crazyhf/SafeKVOHandle