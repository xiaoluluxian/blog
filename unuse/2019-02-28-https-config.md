---
layout: post
title:  "HTTPS Config"
date:   2019-02-28 18:17:59 -0600
categories: document
tag: server

title: "HTTPS Config"
layout: post
date: 2019-02-28 18:17
#image: /assets/images/markdown.jpg
headerImage: false
tag:
- server
star: true
category: blog
author: Yingbo Yuan
description: Add https to web app
---
当发布完自己的项目之后，我们发现用浏览器打开的时候会提示网页不安全，这时候我们需要给我们的网站添加证书，也就是从http变成https。

---

### 添加域名

第一步是给我们的网页添加域名，因为https必须配置给一个有域名的网站。



我以Digital Ocean和GoDaddy为例。首先在Digital Ocean上找到

通过nginx配置之后，我们在浏览器里输入`ip+":"+port`就可以浏览我们的网站了，例如`123.45.987.258:8080`。

---
