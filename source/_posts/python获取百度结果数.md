---
title: python获取百度结果数
date: 2018-01-21 16:36:31
tags:
categorys: python
---

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-  
import urllib
import urllib2
import requests
import re

def download_html(keywords):
	# 抓取参数 https://www.baidu.com/s?wd=testRequest
	key = {'wd': keywords}
	# 请求Header
	headers = {'User-Agent': '(Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/603.3.8 (KHTML, like Gecko) Version/10.1.2 Safari/603.3.8'}
	# 抓取数据内容
	web_content = requests.get("https://www.baidu.com/s?",params=key, headers=headers, timeout=4)
	return web_content.text
content=download_html('ios')
regexName = unicode("百度为您找到相关结果约(.+?)个", "utf8") #中文需要转码
num = re.search(regexName,content).group(1)
num_int = int(num.replace(',',''))
print(num_int)
```

`user-agent` 大全 http://www.fynas.com/ua

注意：使用正则匹配中文需求对中文正则表达式进行转码否则，无法正常匹配

比如Python源码的头文件中声明的编码方式为UTF-8，那么中文需要转成对应的UTF-8编码

```python
# -*- coding: UTF-8 -*-  

regexName = unicode("百度为您找到相关结果约(.+?)个", "utf8") #中文需要转码
```

