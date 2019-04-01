---
title: "Nginx Config"
layout: post
date: 2019-02-22 11:13
#image: /assets/images/markdown.jpg
headerImage: false
tag:
- server
star: true
category: blog
author: Yingbo Yuan
description: Nginx config for deploying web apps
---
当我们需要发布基于NodeJS的前端项目到网络上让所有人都能看到的时候,我们可以使用nginx在服务器上帮忙代理我们的项目。

---

### 安装包

我以ubuntu为例，首先安装需要的软件
1. NodeJS
```
  sudo apt-get install -y nodejs
```

2. Nginx
```
  sudo apt-get install -y nginx
```

---

### 总体配置

进入nginx目录下找到配置文件
```
  cd /etc/nginx
  cat nginx.conf
```
文件如下所示

```
user www-data;
worker_processes auto;
pid /run/nginx.pid;
events {
	worker_connections 768;
	# multi_accept on;
}
http {
	##
	# Basic Settings
	##
	
	sendfile on;
	tcp_nopush on;
	tcp_nodelay on;
	keepalive_timeout 65;
	types_hash_max_size 2048;
	# server_tokens off;
	# server_names_hash_bucket_size 64;
	# server_name_in_redirect off;
	include /etc/nginx/mime.types;
	default_type application/octet-stream;
	##
	# SSL Settings
	##
	ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
	ssl_prefer_server_ciphers on;
	##
	# Logging Settings
	##
	access_log /var/log/nginx/access.log;
	error_log /var/log/nginx/error.log;
	##
	# Gzip Settings
	##
	gzip on;
	gzip_disable "msie6";
	##
	# Max POST upload size limit
	##
	# client_max_body_size 20m;
	# gzip_vary on;
	# gzip_proxied any;
	# gzip_comp_level 6;
	# gzip_buffers 16 8k;
	# gzip_http_version 1.1;
	# gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
	##
	# Virtual Host Configs
	##
	include /etc/nginx/conf.d/*.conf; # <== 确认是否被注释
	include /etc/nginx/sites-enabled/*; # <== 确认是否被注释
}
#mail {
#	# See sample authentication script at:
#	# http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript
# 
#	# auth_http localhost/auth.php;
#	# pop3_capabilities "TOP" "USER";
#	# imap_capabilities "IMAP4rev1" "UIDPLUS";
# 
#	server {
#		listen     localhost:110;
#		protocol   pop3;
#		proxy      on;
#	}
# 
#	server {
#		listen     localhost:143;
#		protocol   imap;
#		proxy      on;
#	}
#}
```

你需要确认 `include /etc/nginx/conf.d/*.conf; `和 `include /etc/nginx/sites-enabled/*; `这两行没有被注释掉，如果被注释了，取消注释即可。

---

### 项目配置

```
  cd /etc/nginx/conf.d
```

在该目录下添加项目的配置文件，可以以自己喜欢的方式命名。

比如 `example.com `是项目的域名，端口是`8080`， 那么我们可以用域名加端口的方式来命名配置文件`example.com_8080.conf`，当我们用nginx代理多个项目的时候，这样命名会方便我们区分不同的配置文件。

`nano example.com_8080.conf` 用该命令创建配置文件，然后把以下内容复制进去

```
  upstream example { # <== 应用名称
    server 127.0.0.1:8080; # <== node 监听的端口号
    keepalive 64;
}
server {
    listen 80; # <== 代理之后监听的端口号， 443是https端口，80是http端口
    server_name example.com www.example.com; # <== 服务器名称, 可以有多个域名，用空格隔开
    location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X-Nginx-Proxy true;
        proxy_set_header Connection "";
        proxy_pass http://example.com; # <== 应用名称
    }
}
```




