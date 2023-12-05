---
title: 网站的开发日志
date: 2023-11-30T05:00:00Z
categories:
  - 工具
tags:
  - tool
author: harry
---

<img src="https://s2.loli.net/2023/12/05/Z8AmdwNJn2i3WIh.jpg" alt="网站更新日志">

以前的很多图床大多都GG了，还有一些个人搭建图床建议不要用。

<!--more-->


### 待迭代更新的功能
- h1,h2,h3样式修改
- 


---

### 【20231205】接入谷歌分析

**在网站head后加入如下js代码**

```js
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=xxxxxx"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'xxxxxx');
</script>
```

登录 **google analytics** 后台进行数据查看

https://analytics.google.com/analytics/web

---

### 【20231201】banner图制作 

- 使用 bing ai 服务

  1. 切换到外区vpn
  2. 打开官方网站 https://www.bing.com/


- 创建自己的提示词

> Generate a jpg image with 1024*1024 pixels, the text in the middle of the image is clickhouse , and the background is a random color.


- 对图片进行调整
  
  裁剪图片大小为 **1024/480**

---


### 【20231130】使用图床网站

- 【国外】https://imgur.com
- 【国内】https://sm.ms/