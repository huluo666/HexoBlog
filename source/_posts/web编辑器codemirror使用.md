---
title: web编辑器codemirror使用
date: 2016-05-04 15:13:38
tags: JavaScript
categories: [JavaScript]
grammar_cjkRuby: true
---

web编辑器codemirror使用

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<link href="https://cdn.bootcss.com/codemirror/5.28.0/theme/seti.min.css" rel="stylesheet">
		<link href="https://cdn.bootcss.com/codemirror/5.28.0/theme/solarized.min.css" rel="stylesheet">
		<link href="https://cdn.bootcss.com/codemirror/5.28.0/codemirror.min.css" rel="stylesheet">
		<script src="https://cdn.bootcss.com/codemirror/5.28.0/codemirror.min.js"></script>
		<script src="https://cdn.bootcss.com/codemirror/5.28.0/mode/clike/clike.min.js"></script>
	</head>
	<body>
		<textarea id="myCode">#input your Code here#</textarea>
		<script type="text/javascript">
		var editor = CodeMirror.fromTextArea(document.getElementById("myCode"), {
			lineNumbers: true, /* 定义是否显示行号 */
			mode: "javascript",  /* 定义语法的类型，如果是html则为：text/html */
			theme: "seti", /* 定义主题 */
			 //value: "设置显示的内容",
			//lineSeparator: 'CRLF', /*指定换行符*/
			indentUnit: 4,   /*设置缩进的字符数，默认为2*/
			smartIndent: true, /*自动缩进，默认为true*/
			//tabSize: 8, /*tab字符的宽度，默认为4*/
			lineWiseCopyCut: false,  /*true时，如果当前没有选中文本，会自动选中当前行*/
			dragDrop: false,   /*是否可以被拖拽*/
			autofocus: true
		});
		</script>
	</body>
</html>
```

加入选择主题

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<link href="https://cdn.bootcss.com/codemirror/5.28.0/theme/seti.min.css" rel="stylesheet">
		<link href="https://cdn.bootcss.com/codemirror/5.28.0/theme/solarized.min.css" rel="stylesheet">
		<link href="https://cdn.bootcss.com/codemirror/5.28.0/codemirror.min.css" rel="stylesheet">
		<script src="https://cdn.bootcss.com/codemirror/5.28.0/codemirror.min.js"></script>
		<script src="https://cdn.bootcss.com/codemirror/5.28.0/mode/clike/clike.min.js"></script>
		<link href="https://cdn.bootcss.com/codemirror/5.28.0/theme/base16-light.min.css" rel="stylesheet">
	</head>
	<body>
	<h2>Theme Demo</h2>
	<textarea id="jsoncode" name="jsoncode" disabled="disabled" readonly="readonly">#code#</textarea>
	<p>选择主题:
	<select id="select" onchange="selectTheme()">
		<option selected>default</option>
		<option>solarized</option>
		<option>seti</option>
		<option>base16-light</option>
	</select>
	</p>
	<script>
		var editor = CodeMirror.fromTextArea(document.getElementById("jsoncode"), {
			lineNumbers: true, /* 定义是否显示行号 */
			lineWrapping: true, /*自动换行*/
			styleActiveLine: true,
			matchBrackets: true,
			readOnly: true,
			theme: "solarized" /* 定义主题 */
		});
		var input = document.getElementById("select");
		function selectTheme() {
			var theme = input.options[input.selectedIndex].textContent;
			editor.setOption("theme", theme);
			location.hash = "#" + theme;
		}
		var choice = (location.hash && location.hash.slice(1)) ||
								 (document.location.search &&
									decodeURIComponent(document.location.search.slice(1)));
		if (choice) {
			input.value = choice;
			editor.setOption("theme", choice);
		}
		CodeMirror.on(window, "hashchange", function() {
			var theme = location.hash.slice(1);
			if (theme) { input.value = theme; selectTheme(); }
		});
	</script>
	</body>
</html>
```



支持语言列表

http://codemirror.net/mode/index.html

