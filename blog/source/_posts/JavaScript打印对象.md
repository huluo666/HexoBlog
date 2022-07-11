---
title: JavaScript打印对象循环引用问题
date: 2017-10-10 11:24:12
tags: Node.js
categories: Node.js
grammar_cjkRuby: true
---

直接打印obj

```
console.log(JSON.stringify(a));// 报错
```

出现错误`TypeError: Converting circular structure to JSON`

解决：使用第三方库`CircularJSON`，`json-stringify-safe`，`util.inspect`，代替 `JSON.stringify` 

安装任意一个即可

```shell
$ npm install json-stringify-safe --save
#$ npm install circular-json --save
#$ npm install util --save

#npm view circular-json version  //查看circular-json库最新版本
```

使用

```javascript
// load circular-json module
var CircularJSON = require('circular-json');
console.log(CircularJSON.stringify(a));

// load util module
var util = require('util');
console.log(util.inspect(a,{depth:null})); //depth:null 展开全部层级

// load json-stringify-safe module
var stringify = require('json-stringify-safe');
console.log(stringify(a));

console.log(JSON.parse(stringify(a));//格式化输出
```



https://stackoverflow.com/questions/11616630/json-stringify-avoid-typeerror-converting-circular-structure-to-json 