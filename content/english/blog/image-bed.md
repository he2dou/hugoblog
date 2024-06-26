---
title: 免费的图床网站
date: 2023-11-30T05:00:00Z
categories:
  - Software
author: harry
image: /images/image-placeholder.png
---


以前的很多图床大多都GG了，还有一些个人搭建图床建议不要用。

<!--more-->
{{< toc >}}

## 聚合图床 【正在使用】

- 速度：集合多家图床，速度还是可以的~
- 上传时一张图片会分发至多个图床，同时图片会保存在本站服务器上
- [上传地址](https://www.superbed.cn/)

## imgur 【外网推荐】

著名的老牌国外图床，2009 年就开始运行了。图片存储稳定可靠。

- 速度：国外真的挺快，不过国内半墙
- CDN：FastlyCDN（这家 CDN 的很多节点都被墙了）
- HTTPS：支持（不支持 HTTP2）
- 域名：`i.imgur.com`
- [上传地址](https://imgur.com/)

追求国内访问速度的还是别用了吧，不过这家图床是真的足够稳定可靠。开放有 API（还有支持免费匿名上传图片的 ClientKey 可以申请），也有很多第三方插件可以用。

## sm.ms 【外网推荐】

土豪兽兽建的图床，2015 年开始正式运营。

- 速度：现在估计是被滥用了没那么快了 烧风购买了更多节点、修改了服务架构，现在全球速度还是不错的。
- CDN：烧风自建的 CDN，有香港阿里云、DigitalOcean 欧洲和 Linode 北美等节点
- HTTPS：HTTP 会被 301 跳转 HTTPS（支持 HTTP2）
- 域名：`ooo.0o0.ooo``i.loli.net`
- [上传地址](https://sm.ms/)

支持 API 操作，图片存储非常可靠，V2EX 钦点的图床。iOS 和 Android 应用 即将开发完毕已经分别上架 [iTunes](https://itunes.apple.com/app/sm-ms/id1268411917) 和 [Play Store](https://play.google.com/store/apps/details%3Fid%3Dsm.ms)，甚至有第三方做的 Telegram Bot。在众多公共图床中最看好它和 imgur。


## PostImage

- 速度：国外速度杠杠的，国内别被墙就好
- CDN：AdvancedHosted CDN
- HTTPS：支持
- 域名：`s1.postimg.org``s2.postimg.org` 等。
- [上传地址](https://postimages.org/)

PostImage 图床的介绍说是为了方便用户在 Facebook 和 Twitter 上传图。这个图床用的 CDN 服务商不太有名。

 [http://6.UPLOAD.CC](http://6.UPLOAD.CC)

- 速度：看下面一条用的 CDN
- CDN：CloudFlare
- HTTPS：支持
- 域名：`upload.cc`。
- [上传地址](https://upload.cc/)

这个图床是香港人开的，TOS 写的挺详细的。提供了 Android 版 APP（Google Play 上可下载），还提供有 Chrome 和 Firefox 的插件，挺方便的。

## ImgSafe

- 速度：看下面一条用的 CDN
- CDN：CloudFlare
- HTTPS：支持
- 域名：`i.imgsafe.org`。
- [上传地址](https://imgsafe.org/)

国外一家图床，网站首页有写着累计有图片托管在这个图床。不过 _据说_ 偶尔会发生图片丢失的情况；自己权衡。还有要注意的就是这个图床仅能通过拖动的方式上传图片，所以手机上就没法传图了。

## 小贱贱图床

- 速度：一般般~获取一个简单的外链，图床用的是微博空间，速度很快，但是图片清晰度会变低。
- 每日可以上传图片20张，上传后可以
- [上传地址](http://pic.xiaojianjian.net/)



## 偶流社区图床

- 免注册，有一定历史，比较可靠
- 限制：图片最大10M，不定期会清理垃圾文件
- [上传地址](https://upload.ouliu.net/)

## 路过图床

- 支持免注册上传图片，永久存储，支持HTTPS加密访问和调用图片，提供多种图片链接格式
- 限制：最大10M
- [上传地址](https://imgchr.com/)

## 极简图床

- 主要提供图片上传和管理界面，需要用户自己设置微博、七牛云或者阿里云OSS信息
- 目前站点维护，原因不详
- [上传地址](http://jiantuku.com/)

## postimg

- 国外的图床，速度也很快。永久存储免注册，图片链接支持https，可以删除上传的图片，提供多种图片链接格式
- 限制：匿名上传和免费帐户上传的图片仅限于12Mb和10k x 10k像素。高级帐户仅限于24Mb和10k x 10k像素。每批最多限制为1000张图像，如果不够可以创建一个帐户并将多批图像上传到同一个图库中
- [上传地址](https://postimages.org/)

## GitHub

GitHub 一直拥有各种奇怪的用途，被发掘出来当图床也见怪不怪了。

- 速度：国内可以接受，海外速度很快
- CDN：Fastly CDN，几个节点在国内都解禁了的
- HTTPS：支持（似乎不支持 HTTP2）
- 域名：`user-images.githubusercontent.com`

上传方式是新建一个 Repo，然后在 Issue 中传图（直接将图片拖动到 issue 输入框即可），GitHub 会将你的图片分发到 GitHub 用的 CDN 中。

这和使用 GitHub Raw 需要 GitHub 的服务器动态生成文件不同，user-image 这个子域名是 GitHub 专门为静态文件准备的，不会让当年某某抢票助手 CC GitHub 的事情重现的。  
当然，这个接口不是公开的。善待 GitHub。

## 奇虎图床（360旗下）

- 速度：全球的速度都很好
- CDN：奇虎自己的 CDN，部分节点和网宿合作
- HTTPS：支持（支持 HTTP2）
- 域名：

- `p1.qhimg.com``p2.qhimg.com``p3.qhimg.com` 等等
- `p1.qhmsg.com``p2.qhmsg.com``p3.qhmsg.com` 等等

  

在这些域名的前端散列符和根域名之间加上 `ssl`，如 `p1.ssl.qhmsg.com`，即可支持 HTTPS 和 HTTP2。

公开上传接口：位于 `wenda.so.com` `bbs.360.cn`。建议通过后者上传图片，一天只能上传 20 张图片，上传图片的方法就是在 360 论坛（Discuz 驱动）的发帖页面上传图片并插入帖子中，然后右键获得图片地址。图片托管在奇虎图床的风险相比微博图床是要低的，也没有发生过图片丢失的情况。

## boin_space

- 速度：国外速度还行，国内就有点鸡肋
- CDN：阿里云和cloudfare
- [上传地址](https://boin.space/)

## Qchan图床

- 由贴图库提供服务
- [上传地址](http://tuchuang.org/)

## Imgbb

- 最大 16 MB 图片大小. 直接的源图片链接, BBCode代码和HTML缩略图显示
- [上传地址](https://zh-cn.imgbb.com/)

## fb.pics

- 速度：国内可以使用，有点像那个imgbb，支持绝大部分社交平台分享链接，免费账号，25mb的限制，有时候yama会用
- [上传地址](http://photobucket.com/)

---
