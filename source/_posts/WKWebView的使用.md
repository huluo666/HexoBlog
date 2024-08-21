---
title: iOS WKWebView的使用
date: 2017-12-22 13:53:06
tags:
categorys: iOS
---

1、WKWebView与js交互

主要有两方面：1、js执行OC代码  2、oc调取写好的js代码

- ##### js调用OC：

  js是不能执行oc代码的，但是可以变相的执行，js可以将要执行的操作封装到网络请求里面，然后oc拦截这个请求，获取url里面的字符串解析即可，这里用到的代理协议。

  主要原理：通过 UIWebVIew 的代理方法截取 web 前端的跳转请求，通过识别与 web 前端约定好的自定义协议头来判断本次请求是否为 JS 调用 Native 的请求，来调用对应的 Native 方法。

  ​

```objectivec
UIWebview
- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType
{
  //拦截请求处理协议
}

WKWebView
- (void)webView:(WKWebView *)webView decidePolicyForNavigationAction:(WKNavigationAction *)navigationAction decisionHandler:(void (^)(WKNavigationActionPolicy))decisionHandler {
  //拦截请求处理协议
}
```

- ##### OC调用js：

  这里用到UIwebview的一个方法。示例代码:获取网页title：

  ```objectivec
  //返回值为 JS 执行结果，如果 JS 执行失败则返回 nil，如果 JS 执行没有返回值，则返回值为空字符串
  #pragma mark-----UIWebView WKNavigationDelegate -----
  - (void)webViewDidFinishLoad:(UIWebView*)webView
  {
      NSString* str = [self.webView stringByEvaluatingJavaScriptFromString:@"pageDidLoad()"];
      NSLog(@"%@", str);
      // 获取网页的title
  NSString *title = [self.webView stringByEvaluatingJavaScriptFromString:@"document.title"];
  }
  ```




```
pragma mark-----WKWebView WKNavigationDelegate -----
// self.webView.navigationDelegate=self;--WKNavigationDelegate
// javaScriptString 为待执行的 JS 语句
// completionHandler 为执行 JS 完毕后的回调，block 的第一个参数为执行结果，第二个参数为错误
-(void)webView:(WKWebView *)webView didFinishNavigation:(WKNavigation *)navigation
{
[self.webView evaluateJavaScript:@"pageDidLoad()" completionHandler:^(id _Nullable value, NSError* _Nullable error) {
  NSLog(@"%@", value);
}];
}
```



js检查是否安装QQ&&微信

```objectivec
  // 创建并配置 WKWebView 的相关参数
  WKWebViewConfiguration* config = [[WKWebViewConfiguration alloc] init];
  //检查是否安装微信&&QQ
  NSString *installJS = @"var pcaction={}; pcaction.isInstallWX=function(){return true};pcaction.isInstallQQ=function(){return false};";
  WKUserScript *installScript = [[WKUserScript alloc] initWithSource:installJS injectionTime:WKUserScriptInjectionTimeAtDocumentStart forMainFrameOnly:NO];
    [config.userContentController addUserScript:installScript];
  WKWebView *webView=[[WKWebView alloc]initWithFrame:self.view.bounds configuration:config];
```

滑动白屏，停止后才渲染页面问题？

https://stackoverflow.com/questions/39549103/wkwebview-not-rendering-correctly-in-ios-10





```
WKUserContentController:为JS提供了一个发送消息的通道并且可以向页面注入JS的类，WKUserContentController对象可以添加多个scriptMessageHandler；
addScriptMessageHandler:name:有两个参数，第一个参数是userContentController的代理对象，第二个参数是JS里发送postMessage的对象。添加一个脚本消息的处理器
同时需要在html的JS中添加：
window.webkit.messageHandlers.<name>.postMessage(<messageBody>)
才能起作用。
```



```html
<script>
    //OC调用JS的方法列表
    function alertMobile() {
        //这里已经调用过来了 但是搞不明白为什么alert方法没有响应
        //alert('我是上面的小黄 手机号是:13300001111')
        document.getElementById('mobile').innerHTML = '我是上面的小黄 手机号是:13300001111'
    }
    function alertName(msg) {
        //alert('你好 ' + msg + ', 我也很高兴见到你')
        document.getElementById('name').innerHTML = '你好 ' + msg + ', 我也很高兴见到你'
    }
    function alertSendMsg(num,msg) {
        //window.alert('这是我的手机号:' + num + ',' + msg + '!!')
        document.getElementById('msg').innerHTML = '这是我的手机号:' + num + ',' + msg + '!!'
    }

    //JS响应方法列表
    function btnClick1() {
        window.webkit.messageHandlers.showMobile.postMessage(null)
    }
    function btnClick2() {
        window.webkit.messageHandlers.showName.postMessage('xiao黄')
    }
    function btnClick3() {
        window.webkit.messageHandlers.showSendMsg.postMessage(['13300001111', 'Go Climbing This Weekend !!!'])
    }
</script>
```





[主题 : WKWebView 与js交互问题 已解决](http://www.cocoachina.com/bbs/read.php?tid-1712564-page-2.html)

[主题 : wkwebview怎么注入一个js对象，让h5那边可以轮询检测到](http://www.cocoachina.com/bbs/read.php?tid-1722961.html)

[WKWebView evaluate JavaScript return value](https://stackoverflow.com/questions/26778955/wkwebview-evaluate-javascript-return-value)



`WKWebView`

https://github.com/My-Old-Driver/WKWebView

https://github.com/devedbox/AXWebViewController

https://github.com/li6185377/IMYWebView

https://github.com/giveMeHug/SDWebView

https://github.com/LSure/SureWebViewController

https://github.com/dlwj15/wkwebview

https://github.com/huos3203/WKWebView-JS

基于WKWebView和UIWebView实现的仿微信WebView功能的页面加载库

https://github.com/DoTalkLily/LYWebViewController

https://github.com/JixinZhang/WKWebProgressViewDemo

https://github.com/hdq135/JsWebView
jsbridge 接口对象和webview对象交互用的工具。 JsfFromOCClass 把oc代码转化为js代码，只转化以“js_”开头的代码 JsWebView 配置webview注入js代码，并处理js调用原生接口


ios android 共用appInterface.jsCallApp方法,无需多平台区别

- https://github.com/sfwan2014/WKWebViewDemo



wkwebview

[iOS中JS与OC相互调用的方式](https://www.zybuluo.com/Sweetfish/note/501575)



[WKWebView控件和JS脚本传参及交互](http://boyers.coding.me/2017/07/07/iOS/WKWebView%E6%8E%A7%E4%BB%B6%E5%92%8CJS%E8%84%9A%E6%9C%AC%E4%BC%A0%E5%8F%82%E5%8F%8A%E4%BA%A4%E4%BA%92/)

https://github.com/Xcoder1011/WebView-JS

https://github.com/giveMeHug/SDWebView

https://github.com/zengweizhen/WKWebView

https://github.com/hongruqi/WTWebView

http://iliunian.cn/14684585476236.html

http://www.cnblogs.com/coolwxb/p/6125920.html

[JS与OC互相调用的一百种方法(包括WKWebView和UIWebView)](http://blog.csdn.net/hmh007/article/details/53126809)

[【iOS】WKWebView 使用及注意事项](https://www.jianshu.com/p/86d99192df68)

https://www.jianshu.com/p/39401c9e5ea3

[OC与JS代码交互原理(混合开发，包含UIWebView、WKWebView)](https://www.jianshu.com/p/64b4f01ad064)

[iOS开发 - WKWebView使用详解](https://www.jianshu.com/p/e537e6587274)

http://peilinghui.com/2017/05/05/%E5%AD%A6%E4%B9%A0iOS%E4%B8%ADJS%E4%B8%8EOC%E7%9B%B8%E4%BA%92%E8%B0%83%E7%94%A8%E7%9A%84%E6%96%B9%E5%BC%8F/

JS与原生OC互相调用的Demo

- https://github.com/Haley-Wong/JS_OC

https://stackoverflow.com/questions/9473582/ios-javascript-bridge