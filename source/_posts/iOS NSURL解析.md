---
title: URL解析
tags: http
categories: http
date: 2017-12-28 16:22:26
---

```
NSLog(@"absoluteURL--%@",requestURL.absoluteURL);//绝对NSurl
NSLog(@"absoluteString--%@",requestURL.absoluteString);//绝对url字符串
NSLog(@"scheme--%@",requestURL.scheme);//协议
NSLog(@"user--%@",requestURL.user);//用户名
NSLog(@"password--%@",requestURL.password);//密码
NSLog(@"baseUrl--%@",requestURL.baseURL);//根目录、只有在通过跟目录生成的url中才会体现
NSLog(@"resourceSpecifier--%@",requestURL.resourceSpecifier);//资源说明符
NSLog(@"relativeString--%@",requestURL.relativeString);//相对路径（带参数）、如果没有baseUrl。则全部显示
NSLog(@"relativePath--%@",requestURL.relativePath);//相对路径、不带参数
NSLog(@"host--%@",requestURL.host);//主机域名
NSLog(@"port--%@",requestURL.port);//端口
NSLog(@"query--%@",requestURL.query);//参数
NSLog(@"fragment--%@",requestURL.fragment);//锚点、可以在网站打开时候直接移动至此
```



输出结果：

> pcautobrowser://information-article/11141215

```js
absoluteURL--pcautobrowser://information-article/11141215
absoluteString--pcautobrowser://information-article/11141215
scheme--pcautobrowser
user--(null)
password--(null)
baseUrl--(null)
resourceSpecifier--//information-article/11141215
relativeString--pcautobrowser://information-article/11141215
relativePath--/11141215
host--information-article
port--(null)
query--(null)
fragment--(null)
```



> pcautobrowser://model/76313?serialId=9550&serialName=A3&modelName=201Sportback

```json
absoluteURL--pcautobrowser://model/76313?serialId=9550&serialName=A3&modelName=201Sportback
absoluteString--pcautobrowser://model/76313?serialId=9550&serialName=A3&modelName=201Sportback
scheme--pcautobrowser
user--(null)
password--(null)
baseUrl--(null)
resourceSpecifier--//model/76313?serialId=9550&serialName=A3&modelName=201Sportback
relativeString--pcautobrowser://model/76313?serialId=9550&serialName=A3&modelName=201Sportback
relativePath--/76313
host--model
port--(null)
query--serialId=9550&serialName=A3&modelName=201Sportback
fragment--(null)
```



https://github.com/evgenyneu/ios-javascriptcore-demo



[JavaScript实现http地址自动检测并添加URL链接](http://www.zhangxinxu.com/wordpress/2010/04/javascript%E5%AE%9E%E7%8E%B0http%E5%9C%B0%E5%9D%80%E8%87%AA%E5%8A%A8%E6%A3%80%E6%B5%8B%E5%B9%B6%E6%B7%BB%E5%8A%A0url%E9%93%BE%E6%8E%A5/)

[Get parts of a NSURL in objectivec](https://stackoverflow.com/questions/3692947/get-parts-of-a-nsurl-in-objectivec)

[Best way to parse URL string to get values for keys?](https://stackoverflow.com/questions/8756683/best-way-to-parse-url-string-to-get-values-for-keys)

[Creating URL query parameters from NSDictionary objects in ObjectiveC](https://stackoverflow.com/questions/718429/creating-url-query-parameters-from-nsdictionary-objects-in-objectivec)

[Url minus query string in objectivec](https://stackoverflow.com/questions/4271916/url-minus-query-string-in-objectivec)

https://stackoverflow.com/questions/37684/how-to-replace-plain-urls-with-links

https://stackoverflow.com/questions/1500260/detect-urls-in-text-with-javascript