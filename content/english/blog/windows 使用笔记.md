---
title: windows 使用笔记
date: 2022-04-04T05:00:00Z
image: /images/image-placeholder.png
categories:
  - 技术
tags:
  - windows
---

<!--more-->

{{< toc >}}
## 如何在windows使用gcc和make命令

使用clion设置mingw

- 把C:\Program Files\JetBrains\CLion 2022.3.3\bin\mingw\bin路径加入path中
- 复制mingw32-make，然后重命名为make

# 常用命令

### 设置cmd命令行终端的代理


```
set http_proxy=http://127.0.0.1:7890
set https_proxy=http://127.0.0.1:7890
```