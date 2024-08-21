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



## Python3脚本调用

```python
let _ = std::process::Command::new("python3")
.current_dir(path)
.args(&["-m", "http.server", "8000"])
.output().unwrap();


  Command::new("python3.6")
        .arg(os)
        .args(&[
            "--layers",
            &format!("{}", layers),
            "--width",
            &format!("{}", width),
            "--threshold",
            &format!("{}", threshold),
        ])
        .arg("--index")
        .arg(file_name.path())
        .stdout(Stdio::inherit())
        .stderr(Stdio::inherit())
        .output()
        .expect("Failed to execute Python script");
}


 let base_dir = env::var("PWD").unwrap_or(current_dir);

        let command_args = [&format!("{}/e2e/run.py", base_dir), "-c", config_path];

        let output = Command::new("python3")
            .args(&command_args)
            .current_dir(base_dir)
            .stdout(Stdio::inherit())
            .stderr(Stdio::inherit())
            .output()?;

        if output.status.success() {
            Ok(())
        } else {
            Err(Error::generic(eyre!(
                "Python E2E test exited with error code {:?}",
                output.status.code(),
            )))
        }
```

https://github.com/novifinancial/serde-reflection/blob/2ea672f8795a8d1b5c2825662ecf477cf302e023/serde-generate/tests/python_generation.rs

API文档：https://tauri.app/v1/api/js/modules/app



https://github.com/kenethrrizzo/learning-rust/search?q=show_city

https://github.com/datayang/tauri_demo

https://github.com/8mamo10
