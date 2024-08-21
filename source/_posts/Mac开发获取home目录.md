---
title: Mac开发获取home目录
date: 2018-02-08 14:36:37
tags:
categorys:
---

获取home目录

```objectivec
#include <unistd.h>
#include <sys/types.h>
#include <pwd.h>
#include <assert.h>

NSString *RealHomeDirectory() {
    struct passwd *pw = getpwuid(getuid());
    assert(pw);
    return [NSString stringWithUTF8String:pw->pw_dir];
}


   NSString *home=[[[NSProcessInfo processInfo] environment] objectForKey:@"HOME"];
```

https://stackoverflow.com/questions/9553390/how-do-i-get-the-users-home-directory-in-objectivec-in-a-sandboxed-app

https://stackoverflow.com/questions/3020187/getting-home-directory-in-mac-os-x-using-c-language
