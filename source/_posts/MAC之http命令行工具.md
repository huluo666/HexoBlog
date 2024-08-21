---
title: MAC之http命令行工具curl
tags: shell
categories: shell
date: 2017-06-06 14:48:21
grammar_cjkRuby: true
---

MAC之http命令行工具curl

> curl是一个向服务器传输数据的工具，支持http、ftp、ftps、scp、sftp、tftp、telnet协议，它可以作为调试接口的利器

curl默认是GET方法，使用-X参数可以支持其他动词

```
curl -X POST www.baidu.com
curl -X DELETE www.baidu.com
```

**一、基本语法**

> -X/–request [GET|POST|PUT|DELETE|…] 用于指定请求方式
> -H/–header             设置请求头(header)信息
> -i/–include            请求接口后，输出响应的header信息
> -d/–data               设置请求中的参数
> -v/–verbose            输出更加详细的响应信息

```
curl -X GET "http://www.rest.com/api/users"
curl -X POST "http://www.rest.com/api/users"
curl -X PUT "http://www.rest.com/api/users"
curl -X DELETE "http://www.rest.com/api/users"
```

##### 一、Get请求

```shell
curl "http://www.baidu.com" 	#如果这里的URL指向的是一个文件或者一幅图都可以直接下载到本地
curl -i "http://www.baidu.com"  #显示全部信息
curl -I "http://www.baidu.com"  #只显示头部信息 HTTP response头信息
curl -v "http://www.baidu.com"  #显示get请求全过程解析
curl http://www.baidu.com/	    #打开网页
curl -o ./baidu.html http://www.baidu.com	#保存网页
```

```shell
curl -H "Content-Type:application/json" http://www.baidu.com #添加头
curl --user-agent "[User Agent]" [URL] 					     #添加user agent
```

post数据或get数据（get就是-X GET)

```shell
curl -i -H "Accept: application/json" -X POST -d "firstName=james" http://192.168.0.165/persons/person
```

##### 二、post请求

直接使用`curl`只是普通的`GET`请求

有两种方式进行`Post`请求：
一、使用`-d`参数，并在后边附上要`POST`的数据

**-d** 参数指定表单以POST的形式执行。

```shell
curl -d "param1=value1&param2=value2" "http://www.baidu.com"
```

二、直接使用`-X`参数指明使用请求方法

```shell
curl -X POST www.example.com
```

如果接口要求必须使用`Content-Type:application/json`。可以通过`-H`参数设置请求头：

`curl -H "Content-Type:application/json" -d '{"name": "wcp"}' -i www.example.com/users`

在http header加入的訊息

```
curl -v -i -H "Content-Type: application/json" http://www.example.com/users
```

```shell
huluo:~ luo.h$ curl -h
Usage: curl [options...] <url>
Options: (H) means HTTP/HTTPS only, (F) means FTP only
     --anyauth       Pick "any" authentication method (H)
 -a, --append        Append to target file when uploading (F/SFTP)
     --basic         Use HTTP Basic Authentication (H)
 ...
 -q                  Disable .curlrc (must be first parameter)
```

### HTTP GUI 工具

[Paw2](http://luckymarmot.com/paw)
[HTTP Client](http://ditchnet.org/httpclient/)
[Cocoa Rest Client](http://ditchnet.org/httpclient/)
https://github.com/jakubroztocil/httpie
https://github.com/mmattozzi/cocoa-rest-client
https://github.com/ACENative/ACEView
https://curl.haxx.se/docs/manpage.html#-d


```shell
curl --data  'username=kele8&password=123456test' 'https://v46.pcauto.com.cn/passport3/rest/login_new.jsp'
curl -X POST -d "username=kele8" -d "password=123456test" https://v46.pcauto.com.cn/passport3/rest/login_new.jsp

//保存cookie
curl -D sugarcookies -X POST -d "username=kele8" -d "password=123456test" https://v46.pcauto.com.cn/passport3/rest/login_new.jsp
```


```shell
# 使用`&`串接多個参数
curl -X POST -d "username=kele8&password=123456test" yourURl
# 也可使用多个`-d`，效果同上
curl -X POST -d "username=kele8" -d "password=123456test"
curl -X POST -d "param1=a 0space"
# "a space" url encode后空白字元会编码成'%20'为"a%20space"，编码后的参数可以直接使用
curl -X POST -d "param1=a%20space"

curl -i -X POST -b "common_session_id1=BA9A8CCFE21CDD38D4A79EC8CE10DA45955B13C980CF88CE6293706A3549C7B83EC4C96FE99E0313" -d username=kele8 -d password=123456test -c  ~/cookie.txt  https://v46.pcauto.com.cn/passport3/rest/login_new.jsp

curl  "common_session_id1=BA9A8CCFE21CDD38D4A79EC8CE10DA45955B13C980CF88CE6293706A3549C7B83EC4C96FE99E0313" -X POST -d "nodeId=13814&topicId=14140213&content=uuu]"

    curl -X POST -c cookies.txt -u "Uern@me:P@ssw0rd" https://login.three.ie/

http://v71.pcauto.com.cn/zxapi/1/topic/live/node/update.ajax
```

### httpPie

`httpPie`相比`Curl`的好处是：返回值信息有语法高亮、对返回的JSON字符串自动格式化。

##### httpie安装

```shell
$ brew install httpie
```

##### httpPie使用

```shell
http -f POST www.baidu.com name=name  #模拟表单提交
http -v www.baidu.com     #显示详细的请求信息
http -h www.baidu,com     #仅显示Header
http -b www.baidu.com     #仅显示Body
http -d www.baidu.com     #下载文件

http PUT www.baidu.com name=name password=pwd  #传递json类型参数（字符串）
http PUT www.baidu.com age:=28 				   #非字符串类型使用：=分割
http --form POST www.baidu.com name='name'     #模拟form的POST请求
http -f POST www.baidu.com/files  name='name' file@~/test.txt  #模拟form文件上传
http www.baidu.com User-Agent:Txl/1.0 'Cookie:a=b;b=c' Referer:http://www.baidu.com/
# 修改请求头，使用：分割

http -a username:password www.baidu.com   #认证
http --auth--type=digest -a user:pwd www.baidu.com  #认证
http --proxy=http:http://192.168.1.1:8080 www.baidu.com
# 使用HTTP代理
```

了解http原理， [Introduction to HTTP](https://launchschool.com/books/http) 是一个不错的选择，也可以参考它的中文翻译版: [HTTP 下午茶](https://www.kancloud.cn/kancloud/tealeaf-http)


##### cookie操作

```shell
curl -D sugarcookies -X POST -d "username=kele8" -d "password=123456test" https://v46.pcauto.com.cn/passport3/rest/login_new.jsp
curl -D sugarcookies -d "username=kele8" -d "password=123456test" https://v46.pcauto.com.cn/passport3/rest/login_new.jsp
curl -b sugarcookies  https://v80.pcauto.com.cn/xsp/s/club/v4.7/getNewMsgCount.xsp?userId=43970286
curl -b '/Users/pconline/Documents/sugarcookies' https://v80.pcauto.com.cn/xsp/s/club/v4.7/getNewMsgCount.xsp?userId=43970286
curl -b sugarcookies https://v80.pcauto.com.cn/xsp/s/club/v4.7/getNewMsgCount.xsp?userId=43970286
curl  -H  "" https://v80.pcauto.com.cn/xsp/s/club/v4.7/getNewMsgCount.xsp?userId=43970286
```

其它字段释义

```shell
-v/--verbose 			小写的v参数，用于打印更多信息，包括发送的请求信息，这在调试脚本是特别有用。
-m/--max-time <seconds> 指定处理的最大时长
-H/--header <header> 	指定请求头参数
-s/--slient 			减少输出的信息，比如进度
--connect-timeout <seconds> 指定尝试连接的最大时长
-x/--proxy <proxyhost[:port]> 指定代理服务器地址和端口，端口默认为1080
-T/--upload-file <file> 指定上传文件路径
-o/--output <file> 		指定输出文件名称
-d/--data/--data-ascii <data> 指定POST的内容
--retry <num>			指定重试次数
-e/--referer <URL> 		指定引用地址
-I/--head			    仅返回头部信息，使用HEAD请求
```



```shell
-w  定义curl的输出格式，
%{http_code}则为获取curl获取URL的http状态码
时间指标解释 ：
time_connect    建立到服务器的 TCP 连接所用的时间
time_starttransfer    在发出请求之后，Web 服务器返回数据的第一个字节所用的时间
time_total   完成请求所用的时间
在 发出请求之后，Web 服务器处理请求并开始发回数据所用的时间是
（time_starttransfer）1.044 - （time_connect）0.244 = 0.8 秒
客户机从服务器下载数据所用的时间是
（time_total）2.672 - （time_starttransfer）1.044 = 1.682 秒
其他常用常用http变量
http_code:http返回类似404,200,500等
time_total:总相应时间
time_namelookup:域名解析时间
time_connect:连接到目标地址耗费的时间
time_pretransfer:从执行到开始传输文件的时间间隔
time_starttransfer:从执行到开始传输文件的时间间隔
size_download:下载网页或文件大小
size_upload:上传文件大小
size_header:响应头
size_request:发送请求参数大小
speed_download：传输速度
speed_upload:平均上传速度
content_type:下载文件类型.
```

更多Http，Curl讲解
http://php.net/manual/zh/function.curl-getinfo.php
https://orrsella.com/2014/10/06/http-request-diagnostics-with-curl/
https://ec.haxx.se/usingcurl-verbose.html
https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html
[MAC之http命令行工具curl](http://coderlt.coding.me/2016/03/22/mac-command-curl/)
[HTTPie:超爽的HTTP命令行客户端](https://tonydeng.github.io/2015/07/10/httpie-howto/)
http://lingxiankong.github.io/blog/2014/08/19/curl-httpie/
http://www.ruanyifeng.com/blog/2011/09/curl.html
curl命令用法
http://ju.outofmemory.cn/entry/84875
http://jockchou.github.io/blog/2016/01/23/curl.html
使用CURL来调试你的应用
http://stormzhang.com/devtools/2014/11/07/use-curl-debug/
http://man.linuxde.net/curl
[httpie人性化curl工具使用详解](https://xin053.github.io/2016/08/15/httpie%E4%BA%BA%E6%80%A7%E5%8C%96curl%E5%B7%A5%E5%85%B7%E4%BD%BF%E7%94%A8%E8%AF%A6%E8%A7%A3/)
Curl with Cookies and Headers
http://joelpm.com/curl/tools/2010/06/17/curl-with-cookies-and-headers.html
使用curl指令測試REST服務
http://blog.kent-chiu.com/2013/08/14/testing-rest-with-curl-command.html
https://github.com/atg/taskit
https://github.com/jakubroztocil/httpie


