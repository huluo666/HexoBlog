---
title: Tauri+Vue开发桌面应用
date: 2022-07-13 14:15:07
categories:
tags:
---



一、安装Rust开发环境

https://www.rust-lang.org/zh-CN/learn/get-started

```
$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
$ rust --version
$ rustup update stable
```



```sh
$ npm create tauri-app

Press any key to continue...
? What is your app name? AutoTask
? What should the window title be? 自动任务
? What UI recipe would you like to add? create-vite (vanilla, vue, react, svelte, preact, lit) 
(https://vitejs.dev/guide/#scaffolding-your-first-vite-project)
? Add "@tauri-apps/api" npm package? Yes
? Which vite template would you like to use? vue
```

调试

```
$ cd AutoTask
$ npm run tauri dev #调试
$ npm run tauri build #打包

```



https://element-plus.gitee.io/zh-CN/guide/installation.html

```
$ npm install element-plus --save
```



https://github.com/kenethrrizzo/learning-rust/search?q=show_city

https://github.com/datayang/tauri_demo

https://github.com/8mamo10
