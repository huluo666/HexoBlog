---
title: javascript HTTP网络请求
date: 2018-02-06 09:39:10
tags: JavaScript
---

​     作为一名移动开发者，很多时候需要对网络数据进行处理，而Xcode每次修改都需要重新编译，这大大降低了我们开发的敏捷性。python对网络数据的处理是很简单的，但iOS开发对python的支持不是很够，只能用第三方框架，而且API数量和使用方便性很低。对于JavaScript，iOS7之后苹果就推出了JavaScriptCore.framework这个框架,这个框架为大家在与JS交互上提供了很大帮助,可以在html界面上调用OC方法并传参,也可以在OC上调用JS方法并传参，可以很方便的实现OC/swift与js交互。

JavaScript中的网络分为两大类：AJAX（浏览器）和HTTP客户端（服务器），下面是对这2种请求的分析比较

#### 一、AJAX / HTTP库比较（译文摘要）

原文链接：https://www.javascriptstuff.com/ajax-libraries/

JavaScript中的网络分为两类：`AJAX`（浏览器）和`HTTP`客户端（服务器）。

有时你只需要其中的一个，有时需要两个（例如在一个同构/通用的应用程序）。

无论哪种方式，你都希望拥有**简洁的语法**。大多数开发人员发现`XMLHttpRequest`API太冗长了。

很多开发人员使用`jQuery`，但是如果您只需要`AJAX`功能，加载整个库看起来可能很浪费。

我整理了一个列表来帮助你为你的项目选择一个**最合适**的JavaScript网络请求库。之后，我会举几个具体的场景来推荐相应库。

|                                          | Support          | Features     |      |                |          |         |                 |                      |
| ---------------------------------------- | ---------------- | ------------ | ---- | -------------- | -------- | ------- | --------------- | -------------------- |
|                                          | Chrome &Firefox1 | All Browsers | Node | Concise Syntax | Promises | Native2 | Single Purpose3 | Formal Specification |
| [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) | ✓                | ✓            |      |                |          | ✓       | ✓               | ✓                    |
| [Node HTTP](https://nodejs.org/api/http.html) |                  |              | ✓    |                |          | ✓       | ✓               | ✓                    |
| [fetch()](https://developer.mozilla.org/en-US/docs/Web/API/GlobalFetch/fetch) | ✓                |              |      | ✓              | ✓        | ✓       | ✓               | ✓                    |
| [Fetch polyfill](https://github.com/github/fetch) | ✓                | ✓            |      | ✓              | ✓        |         | ✓               | ✓                    |
| [node-fetch](https://github.com/bitinn/node-fetch) |                  |              | ✓    | ✓              | ✓        |         | ✓               | ✓                    |
| [isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) | ✓                | ✓            | ✓    | ✓              | ✓        |         | ✓               | ✓                    |
| [superagent](https://github.com/visionmedia/superagent) | ✓                | ✓            | ✓    | ✓              |          |         | ✓               |                      |
| [axios](https://github.com/mzabriskie/axios) | ✓                | ✓            | ✓    | ✓              | ✓        |         | ✓               |                      |
| [request](https://github.com/request/request) |                  |              | ✓    | ✓              |          |         | ✓               |                      |
| [jQuery](https://jquery.com/)            | ✓                | ✓            |      | ✓              |          |         |                 |                      |
| [reqwest](https://github.com/ded/reqwest) | ✓                | ✓            | ✓    | ✓              | ✓        |         | ✓               |                      |

更多查看原文：https://www.javascriptstuff.com/ajax-libraries/

jQuery中的ajax与原生js中的ajax对比？

jquery是基于原生ajax的一个js封装库，不同浏览器对ajax的实现可能不同，jQuery解决了兼容问题。当然封装后语法更简单，使用更方便。



jQuery网络请求

**1、ajax()方式**

```javascript
function requestData(id) {  
          var params = JSON.stringify({"name": "张山", "password": "1112222"});
          $.ajax({  
              type: "post",  
              url: "https://www.baidu.com/",  
              dataType: "json",  
              data: params,  
              cache: false,  
              timeout: 10 * 1000,  
              success: function(data) {  
                   alert("数据+ data");
              },  
              error: function(XMLHttpRequest, textStatus, errorThrown) {  

              }  
          });  
      };  
```

**2.$.get()**

```javascript
function requestData(id) {
    var url: "https://www.baidu.com/";
    var params = {
        "id": id
    };
    $.get(url, params,
        function(data) {
    			alert("数据+ data");
        }, "json");
};
```
**3.$.post()**

```javascript
function requestData(id) {
    var url: "https://www.baidu.com/";
    var params = {
        "id": id
    };
    $.post(url, params,
        function(data) {
			alert("数据+ data");
	}, "json");
};
```


**4.$.jQuery.getJSON()**

函数没有type参数，返回的是json类型的，不需要转换。

```javascript
$.getJSON("test.js", function(json){  
  alert("JSON Data: " + json);  
}); 
```



示例

```html
<!DOCTYPE html>
<html>
<head>
<script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>
<script>
//在文档加载后激活函数：
$(document).ready(function(){
	$("button").click(function(){
		$.get("https://www.baidu.com/",function(data,status){
			alert("数据：" + data + "\n状态：" + status);
		});
    });
});
</script>
</head>
<body>
<button>向页面发送 HTTP GET 请求，然后获得返回的结果</button>
</body>
</html>
```

如果使用本地文件然后浏览器打开调试会出现`No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'null' is therefore not allowed access.`错误

https://stackoverflow.com/questions/8456538/origin-null-is-not-allowed-by-access-control-allow-origin