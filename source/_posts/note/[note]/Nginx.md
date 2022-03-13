---
title: Nginx
date: 2022-03-12 21:34:37
tags: Nginx
cover: 

---

# Nginx

## 基本

正向代理 代理客户端

反向代理 代理服务端

### Nginx 命令

``` sh
nginx -v # 查看 nginx 版本
nginx -V # 查看版本和配置选项信息
nginx	# 启动
nginx -s reopen # 重启
nginx -s stop	# 立即停止
nginx -s quit # 等待进程结束后关闭
nginx -s reload	# 重新加载配置
nginx -T # 查看最终配置
nginx -t # 测试配置文件是否有语法错误
nginx -c <fileName> # 指定配置文件
ps aux|grep nginx	# 查看进程
```

## Nginx 基本配置

### 默认配置

``` sh
# nginx.config
# nginx 全局配置 start
#user  nobody;
worker_processes  1; # 并发处理服务，值越大，并发数越多。

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;

# nginx 全局配置 end

events {	# 事件
	worker_connections  1024;	# 最大连接数
}


http {	# http 配置
  include       mime.types;
  default_type  application/octet-stream;	# 设置默认类型
  server_names_hash_bucket_size 256;
  proxy_buffer_size 10240k;
  proxy_buffers 16 10240k;
  proxy_busy_buffers_size 20480k;
  proxy_temp_file_write_size 20480k;

  log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
  '$status $body_bytes_sent "$http_referer" '
  '"$http_user_agent" "$http_x_forwarded_for"';

  #access_log  logs/access.log  main;

  sendfile        on;
  #tcp_nopush     on;

  #keepalive_timeout  0;
  keepalive_timeout  65;

  gzip  on;

  include servers/*.conf;	# 引入了 servers 目录下的全部 .conf 文件
}

```

``` sh
# servers/<file name>.conf
server {
  listen 80;	# 监听 80 端口
  server_name localhost;	# 服务名，域名
  default_type text/html; # 为当前 server 设置类型，不设置默认使用全局默认类型
  
  # set $<variableName> <vlaue>
  set $path /test
  location /set {
  	root $path
  }
  
	# location[= | ~ | ~* | ^~] <path> { ... }
	# = 进行普通字符精确匹配。也就是完全匹配。
	# ^~ 前缀匹配。如果匹配成功，则不再匹配其他location。
	# ~ 表示执行一个正则匹配，区分大小写
	# ~* 表示执行一个正则匹配，不区分大小写
	# /xxx/ 常规字符串路径匹配
	# / 通用匹配，任何请求都会匹配到
  location / {	# 优先级最低
  	root /example;	# 静态服务器 example 目录
  	index index.html index.htm;	# 默认访问 index.html 文件
  }
  
  # proxy_pass url 末尾不带 '/' 时，连同匹配到的 /test/ 路径一起进行反向代理 
  location /test {	# /test/xxx => http://example.com/test/xxx
  	proxy_pass http://example.com;
  }
  # proxy_pass url 末尾带 '/' 时，省略了匹配到的 /test/ 路径
  location /test/ { # /test/xxx => http://example.com/xxx
  	proxy_pass http://example.com/;
  }

  location /api {	# 将 localhost:80/api 反向代理到 http://api.example.com
  	proxy_pass http://api.example.com;	# proxy_pass 为反向代理
  }
  
  location = /example { # '=' 优先级最高
  	# ...
  }
  location ^~ /example {	# '^~' 优先级次高
  	# ...
  }
  location ~ <RegExp> {	# 正则表达式 优先级次次高
  	# ...
  }
  # 同优先级，匹配第一个
  location ~ ^/\w {	# 第一个被匹配
  	# ...
  }
  location ~ ^/\[a-z] {
  	# ...
  }
}
```



## Nginx 常见功能

### 访问限制



### 跨域



### 合并请求



### HTTPS

``` sh
server {
	# 服务端口为 443，开启 ssl
  listen 443  ssl http2;
  # 配置域名
  server_name example.com;
	# 配置 ssl 证书
  ssl_certificate      ssl/xxx.pem;
  ssl_certificate_key  ssl/xxx2.pem;
  # 缓存有效期
  ssl_session_timeout  5m;
  # 使用服务器端端首选算法
  ssl_prefer_server_ciphers on;
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  # 加密算法
  ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;


  location / {
  	# 配置静态文件路径
    root /data/www;
  }
}
```



### 负载均衡

``` sh
# servers/<file name>.conf
upstream balance {
# weight 为权重，权重越大访问概率越高。
	server 127.0.0.1:8080 weight=1;	
	server 127.0.0.1:8081;
}
server {
  listen 80;	# 监听 80 端口
  server_name example.com;	# 服务名
  access_log off;

  location / {
  	proxy_pass balance;	# 反向代理到 balance 设置的两个服务器
  }

  location / {
  	proxy_pass http://balance/;
  }
}
```

### 动静分离





