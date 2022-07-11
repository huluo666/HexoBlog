---
title: iOS直播 Mac下Nginx+Rtmp直播流服务器搭建
date: 2017-09-20 17:48:18
tags: 工具
categories: [iOS]
grammar_cjkRuby: true
---

iOS直播 Mac下Nginx+Rtmp直播流服务器搭建

### 1. 安装nginx、nginx-rtmp-module

```
1、brew install nginx						 //执行安装nginx 
2、brew install nginx-full --with-rtmp-module //执行安装nginx-rtmp-module
```

### 2. nginx.conf配置文件，配置RTMP、HLS

查找到nginx.conf配置文件（路径`/usr/local/etc/nginx/nginx.conf`），用文本编辑器配置RTMP、HLS。

```shell
# 在http节点后面加上rtmp配置：
rtmp {
  server {
     #监听的端口
      listen 1935;
      # RTMP 直播流配置
      application rtmplive {
          live on;
	      #为 rtmp 引擎设置最大连接数。默认为 off
	      max_connections 1024;
       }
	  # HLS 直播流配置
      application hls{
          live on;
          hls on;
          hls_path /usr/local/var/www/hls;
          hls_fragment 1s;
      }
   }
}
```



在http中添加 hls 的配置

```
location /hls {  
       # Serve HLS fragments  
       types {  
           application/vnd.apple.mpegurl m3u8;  
           video/mp2t ts;  
       }  
       root /usr/local/var/www;  
       #add_header Cache-Controll no-cache;
       expires -1;
   }
```



### 3. 重启nginx服务

重启nginx服务，浏览器中输入 [http://localhost:8080](http://localhost:8080/)，是否出现欢迎界面确定nginx重启成功。

```
nginx -s reload
```







六、直播流转换格式、编码推流

1.安装 FFmpeg 工具

```
brew install ffmpeg
```

2.推流MP4文件

视频文件地址：/Users/gao/Desktop/video/test.mp4
推流拉流地址：rtmp://localhost:1935/rtmplive/home，rtmp://localhost:1935/rtmplive/home

```
//RTMP 协议流
ffmpeg -re -i /Users/gao/Desktop/video/test.mp4 -vcodec libx264 -acodec aac -f flv rtmp://10.14.221.17:1935/rtmplive/home

//HLS 协议流
ffmpeg -re -i /Users/gao/Desktop/video/test.mp4 -vcodec libx264 -vprofile baseline -acodec aac -ar 44100 -strict -2 -ac 1 -f flv  -q 10 rtmp://10.14.221.17:1935/hls/test
```



`注意`： 当我们进行推流之后，可以安装[VLC](http://www.pc6.com/mac/112121.html)、ffplay（支持rtmp协议的视频播放器）本地拉流进行演示

3.FFmpeg推流命令

① 视频文件进行直播

```
ffmpeg -re -i /Users/gao/Desktop/video/test.mp4 -vcodec libx264 -vprofile baseline -acodec aac -ar 44100 -strict -2 -ac 1 -f flv  -q 10 rtmp://192.168.1.101:1935/hls/test
ffmpeg -re -i /Users/gao/Desktop/video/test.mp4 -vcodec libx264 -vprofile baseline -acodec aac -ar 44100 -strict -2 -ac 1 -f flv  -q 10 rtmp://10.14.221.17:1935/hls/test
```

② 推流摄像头＋桌面+麦克风录制进行直播

```
ffmpeg -f avfoundation -framerate 30 -i "1:0" \-f avfoundation -framerate 30 -video_size 640x480 -i "0" \-c:v libx264 -preset ultrafast \-filter_complex 'overlay=main_w-overlay_w-10:main_h-overlay_h-10' -acodec libmp3lame -ar 44100 -ac 1  -f flv rtmp://192.168.1.101:1935/hls/test
```





[FFmpeg常用推流命令](http://www.jianshu.com/p/d541b317f71c)

[在Mac上自己搭建直播服务器](http://www.jianshu.com/p/ec0e9bc6591d)