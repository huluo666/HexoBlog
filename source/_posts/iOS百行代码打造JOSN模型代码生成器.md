---
title: iOS百行代码打造JOSN模型代码生成器
tags:
categories: iOS
date: 2017-04-06 13:54:49
grammar_cjkRuby: true
---

Mac应用，采用xib方式，拖入2个NSTextView

代码比较简单，整个.m文件才百余行代码

`ViewController.h`

```objectivec
#import <Cocoa/Cocoa.h>

@interface ViewController : NSViewController

@property (unsafe_unretained) IBOutlet NSTextView *inputTextView;
@property (unsafe_unretained) IBOutlet NSTextView *outPutTextView;
- (IBAction)autoCodeCreate:(id)sender;

@end
```

`ViewController.m`

```objectivec
#import "ViewController.h"
#import "JSONUtils.h"

@interface  ViewController()
@property(nonatomic,strong) NSMutableArray *arrayModel;
@property(nonatomic,strong) NSDictionary *jsonDict;
@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
}


- (IBAction)autoCodeCreate:(id)sender {
    NSString *jsonStr = self.inputTextView.textStorage.string;
    //json字符串转json字典,可用
    NSDictionary *dic = [self objectFromJSONString:jsonStr];
    if(dic==nil){
        self.inputTextView.string = @"JSON格式错误";
        return;
    }
    self.jsonDict=dic;
    NSString *fileStr=[self autoCodeWithJsonDict:dic modelKey:nil];
    NSLog(@"%@", fileStr);
    [self.outPutTextView setString:fileStr];

    [self handleArrayModel];
}

-(void)handleArrayModel
{
    for (NSString *key in self.arrayModel) {
        NSDictionary *jsonDict=[self.jsonDict[key] firstObject];
        NSString *filestr0=[self autoCodeWithJsonDict:jsonDict modelKey:key];
        NSString *filestr1=[self.outPutTextView.string  stringByAppendingString:@"\n\n"];
        NSString *filestr =[filestr1 stringByAppendingString:filestr0];
        [self.outPutTextView setString:filestr];
    }
    self.arrayModel=nil;
}


-(NSString *)autoCodeWithJsonDict:(NSDictionary *)dic modelKey:(NSString *)classKey
{
    if (classKey.length==0||classKey==nil) {
        classKey=@"customModel";
    }
    NSArray *keyArray = [dic allKeys];
    NSString *fileStr=[NSString stringWithFormat:@"@interface %@ : NSObject \r\n",classKey];
    for(int i=0;i<keyArray.count;i++)
    {
        NSString *key = [keyArray objectAtIndex:i];
        NSLog(@"%@", key);
        id value = [dic objectForKey:key];

        if([value isKindOfClass:[NSString class]])
        {
            NSLog(@"string");
            fileStr = [NSString stringWithFormat:@"%@@property (strong,nonatomic) NSString *%@;\r\n",fileStr,key];
        }
        else if([value isKindOfClass:[NSNumber class]])
        {
            NSLog(@"int");
            fileStr = [NSString stringWithFormat:@"%@@property (strong,nonatomic) NSNumber *%@;\r\n",fileStr,key];
        }
        else if([value isKindOfClass:[NSArray class]])
        {
            NSLog(@"array");
            fileStr = [NSString stringWithFormat:@"%@@property (strong,nonatomic) NSArray *%@;\r\n",fileStr,key];
            //判断是否为字典数组
            id subvalue=[value lastObject];
            if ([subvalue isKindOfClass:[NSDictionary class]]) {
                [self.arrayModel addObject:key];
            }
        }
        else
        {
            NSLog(@"string");
            fileStr = [NSString stringWithFormat:@"%@@property (strong,nonatomic) NSString *%@;\r\n",fileStr,key];
        }
    }

    fileStr = [fileStr stringByAppendingString:@"@end"];
    return fileStr;
}


-(id)objectFromJSONString:(NSString *)jsonString{
    jsonString = [[jsonString stringByReplacingOccurrencesOfString:@" " withString:@""] stringByReplacingOccurrencesOfString:@" " withString:@""];
    NSLog(@"jsonString=%@",jsonString);
    NSData *jsonData = [jsonString dataUsingEncoding:NSUTF8StringEncoding];
    NSError *err;
    id jsondict = [NSJSONSerialization JSONObjectWithData:jsonData
                                                    options:NSJSONReadingMutableContainers
                                                      error:&err];
    if (err) {
        return nil;
    }else{
        return jsondict;
    }
}

- (NSMutableArray *)arrayModel {
	if(_arrayModel == nil) {
		_arrayModel = [[NSMutableArray alloc] init];
	}
	return _arrayModel;
}
@end
```

![UC20170406_140411](http://7xr7vj.com1.z0.glb.clouddn.com/UC20170406_140411.png)

​	大致原理就是根据value值判断数据类型，然后进行字符串拼接，加入`\r,\n,\t`等格式控制符，优化输出模型格式。如果需要需要生产文件，直接将结果写入文件即可。

​	当然其它功能代码自动生成原理基本一样。

#### 升级优化：

如果需要高亮或显示行号，可以直接使用如[JSDCocoaDemos](https://github.com/balthisar/JSDCocoaDemos)等第三方库，优化显示样式。

