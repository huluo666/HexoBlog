---
title: JSON数据压缩
date: 2017-12-27 16:13:38
tags: iOS
categories: [iOS]
grammar_cjkRuby: true
---

```
<script src="https://cdn.bootcss.com/lz-string/1.4.4/base64-string.js"></script>
```

```javascript
// sample json object
var jsonobj = {'sample': 'This is supposed to be ling string', 'score': 'another long string which is going to be compressed'}

// compress string before storing in localStorage
localStorage.setItem('mystring', LZString.compress(JSON.stringify(jsonobj)));

// decompress localStorage item stored
var string = LZString.decompress(localStorage.getItem('mystring'))

// parse it to JSON object
JSON.parse(string);
```

https://stackoverflow.com/questions/20773945/storing-compressed-json-data-in-local-storage

https://stackoverflow.com/questions/294297/javascript-implementation-of-gzip