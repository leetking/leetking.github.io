---
layout: default
title: "Hello Jekyll"
date: 2017-04-23 10:35:06
author: alpha 0x00
categories: jekyll
---

## 使用

### 草稿

```shell
$ jekyll serve
$ # 或者 jekyll build --drafts
```

### 头信息
```
---
xxx
---
```

## 功能配置测试

### 代码高亮
```c
#include <stdio.h>
int main(int argc, char **argv)
{
    printf("Hello Jekyll\n");
    return 0;
}
```

```java
package com.cwnu.foo;

public class Main {
    public void main(String[] args) {
        System.out.println("Hello World!\n");
    }
}
```

```lua
local rest = require("test")
test.print("Hello World!\n")
```

### Tex代码

