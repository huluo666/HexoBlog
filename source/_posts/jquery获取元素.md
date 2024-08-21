---
title: JQuery选择器（获取元素）
date: 2017-10-20 18:24:12
tags: Node.js
categories: Node.js
grammar_cjkRuby: true
---

示例html

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
	</head>
	<body>
		<p>hello world!</p>
		<p>hello world!!</p>
		<p>hello world!!!</p>
		<p class="test">test hello world</p>
		<h4 class="test">test title</h4>

		<input type="text" id="inputtextId1" class="form-control" placeholder="https://jsonblob.com/api/jsonBlob/xxx" value="123">
		<button>隐藏</button>
		<script>
			$(document).ready(function(){
				//用户点击按钮后，所有 <p> 元素都隐藏：
				$("button").click(function(){
					$("p").hide();
				});
			});
		</script>
	</body>
</html>
```

jQuery 中所有选择器都以美元符号开头：`$()`。

#### 一、元素选择器

jQuery 元素选择器基于元素名选取元素。

在页面中选取所有 `<p>` 元素:

```
$("p")
```



#### 二、id 选择器

jQuery `#id` 选择器通过 HTML 元素的 id 属性选取指定的元素,页面中元素的 id 应该是唯一的。

通过 id 选取元素语法如下：

```
$("#myDiv");//获取div元素
```

获取`textFiled`值

```
var tfValue10= $("input[id='inputtextId1']").val();  
var tfValue11= $("#inputtextId1").val();
var tfValue12= $("input[name='inputtextId1']").val();
var tfValue13= $("input[type='text']").val();
var tfValue14= $("input[type='text']").attr("value");
```



#### 三、`.class` 选择器

jQuery 类选择器可以通过指定的 class 查找元素。

```
$(".test")
```



赋值

输入框赋值方式,Input输入框ID为inputtextId

```
$("#inputtextId")[0].value="8888";
$("#inputtextId").val("88888");
```



参考文档

http://www.w3school.com.cn/jquery/jquery_selectors.asp

http://www.w3school.com.cn/jquery/jquery_ref_selectors.asp

http://www.runoob.com/jquery/jquery-selectors.html

http://www.cnblogs.com/keepfool/archive/2012/06/02/2532203.html