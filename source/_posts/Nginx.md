---
title: Nginx
date: 2022-03-12 21:34:37
updated: 2022-03-15 19:04:48
tags: Nginx
categories: 
  - ['Nginx']
cover: https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSAff1msXvuacYlIJ-bUL4Uece-YW30unNERQ&usqp=CAU

---

# Nginx

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

``` nginx
# nginx.config
# nginx 全局配置 start
#user  nobody;	# 运行用户
worker_processes  1; # 并发处理服务，值越大，并发数越多。

# 错误日志
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;	# pid 位置

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

	# 设置日志
  log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
  '$status $body_bytes_sent "$http_referer" '
  '"$http_user_agent" "$http_x_forwarded_for"';

  #access_log  logs/access.log  main;	# 设置日志存放位置

  sendfile        on;
  #tcp_nopush     on;

  #keepalive_timeout  0;
  keepalive_timeout  65;

  gzip  on;

  include servers/*.conf;	# 引入了 servers 目录下的全部 .conf 文件
}

```

``` nginx
# servers/<file name>.conf
server {
  listen 80;	# 监听 80 端口
  server_name localhost;	# 服务名，域名
  default_type text/html; # 为当前 server 设置类型，不设置默认使用全局默认类型
  
  location / {
		root /example;	# 静态服务器 example 目录
		index index.html index.htm;	# 默认访问 index.html 文件
	}
}
```

### location 优先级

``` nginx
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
```

### 其他

#### return

``` nginx
# 返回 http 状态码
location /example {
	# return <status code> <url>
	return 301 http://www.examole.com;
}
```

#### error_page

``` nginx
location /example {
	# error_page <status code> <url>
	error_page 404 /404.html;
}
```

#### rewrite

``` nginx
# 
location /example {
	# 
	rewrite 
}
```

#### log

``` nginx
# 需将 gzip 设置为 on
log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

access_log  /usr/local/etc/nginx/logs/host.access.log  main;

gzip  on;
```

#### 变量

``` nginx
# set $<variableName> <vlaue>
set $path /test
location /set {
	root $path
}

$args ：#这个变量等于请求行中的参数，同$query_string
$content_length ：请求头中的Content-length字段
$content_type ：请求头中的Content-Type字段
$document_root ：当前请求在root指令中指定的值
$host ：请求主机头字段，否则为服务器名称
$http_user_agent ：客户端agent信息
$http_cookie ：客户端cookie信息
$limit_rate ：这个变量可以限制连接速率
$request_method ：客户端请求的动作，通常为GET或POST
$remote_addr ：客户端的IP地址
$remote_port ：客户端的端口
$remote_user ：已经经过Auth Basic Module验证的用户名
$request_filename ：当前请求的文件路径，由root或alias指令与URI请求生成
$scheme ：HTTP方法（如http，https）
$server_protocol ：请求使用的协议，通常是HTTP/1.0或HTTP/1.1
$server_addr ：服务器地址，在完成一次系统调用后可以确定这个值
$server_name ：服务器名称
$server_port ：请求到达服务器的端口号
$request_uri ：包含请求参数的原始URI，不包含主机名，如：”/foo/bar.php?arg=baz”
$uri ：不带请求参数的当前URI，$uri不包含主机名，如”/foo/bar.html”
$document_uri ：与$uri相同
```





## Nginx 常用功能

### gzip

``` nginx
gzip on;	# on 开启 gzip
gzip_types text/plain application/json;	# 要使用 gzip 压缩的文件类型，text/html 会强制启用
gzip_proxied any;	# 默认 off，nginx做为反向代理时启用，用于设置启用或禁用从代理服务器上收到相应内容 gzip 压缩
gzip_vary on;	# 在响应头中添加 Vary: Accept-Encoding，代理服务器可根据请求头中的 Accept-Encoding 识别是否启用 gzip 压缩
gzip_comp_level 6;	# gzip 压缩比。该属性最大为 9，最小为 1，级别越高压缩率越大，压缩时间越长。
gzip_buffers 16 8k;	# 获取多少内存用于缓存压缩结果，16 8k 表示以 8k*16 为单位
gzip_min_length 1k;	# 允许压缩的页面最小字节数，页面字节数从header头中的 Content-Length 中进行获取。默认值是 0，不管页面多大都压缩。建议设置成大于 1k 的字节数，小于 1k 可能会越压越大
gzip_http_version: 1.1;	# 启用 gzip 所需的 HTTP 最低版本，默认 1.1
```



### 反向代理

``` nginx
server {
  listen 80;	# 监听 80 端口
  server_name localhost;	# 服务名，域名
  default_type text/html; # 为当前 server 设置类型，不设置默认使用全局默认类型
  
  # proxy_pass 为反向代理
  # proxy_pass url 末尾不带 '/' 时，连同匹配到的 /test/ 路径一起进行反向代理 
  location /test {	# /test/xxx => http://example.com/test/xxx
    proxy_pass http://example.com;
  }
  # proxy_pass url 末尾带 '/' 时，省略了匹配到的 /test/ 路径
  location /test/ { # /test/xxx => http://example.com/xxx
    proxy_pass http://example.com/;
  }

  location /api {	# 将 localhost:80/api 反向代理到 http://api.example.com
    proxy_pass http://api.example.com;
  }
}
```



### 访问限制

``` nginx
# 禁止访问 deny
location /example {
	root $doc_root;
	deny all;
}

location / {	# 规则自上而下匹配
	deny <ip>; # 禁止某个 ip
	allow <ip>; # 允许某个 ip
	deny all;
}
```



### 跨域

反向代理

``` nginx
server {
  listen 3000;
  server_name www.example.com;
  
  location ^~/api {	# /api/ 代理到 api.example.com/api
    proxy_pass api.example.com;
  }
}
```

设置请求头

``` nginx
location / {
  add_header Access-Control-Allow-Origin http://example.com always;	# 设置请求源。* 允许所有请求，带 cookie 的请求不支持 *
  add_header Access-Control-Allow-Methods "GET, POST, PUT, OPTIONS";	# 允许的方法
  add_header Access-Control-Allow-Headers "Accept,Accept-Encoding,Accept-Language,Connection,Content-Length,Content-Type,Host,Origin,Referer,User-Agent";	# 允许请求的 header
  add_header Access-Control-Allow-Credentials true;    # 带 cookie，Access-Control-Allow-Credentials 为 true Access-Control-Allow-Origin 和 Access-Control-Allow-Headers 不能为 *

  # 非简单请求的预检请求 OPTIONS 请求返回 204
  if ($request_method = 'OPTIONS') {
    add_header 'Access-Control-Max-Age' 86400;   # OPTIONS 请求的有效期，在有效期内不用发出另一条预检请求
    return 204;
  }
}
```



### HTTPS

``` nginx
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
  # 支持 ssl 协议
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

``` nginx
# servers/<file name>.conf
upstream balance {
# weight 为权重，权重越大访问概率越高；不配置权重默认配置为轮询。
	server 127.0.0.1:8080 weight=1;	
	server 127.0.0.1:8081;
	server 127.0.0.1:8082 backup;	#	backup 为备用服务器，只有其他服务器宕机的情况下才会使用。
}
upstream test {
	fair;	# 设置 fair 可以按服务器的响应时间来分配请求，响应时间短的优先分配, 需安装第三方插件。
	server localhost:8080;
	server localhost:8081;
}
upstream test2 {
	ip_hash;	# 
	server localhost:8080;
	server localhost:8081;
}
upstream test3 {
	hash $requst_url;	# url_hash 需安装第三方插件。
	hash_method crc32;
	server localhost:8080;
	server localhost:8081;
}

server {
  listen 80;	# 监听 80 端口
  server_name example.com;	# 服务名
  access_log off;

  location / {
  	proxy_pass balance;	# 反向代理到 balance 设置的两个服务器
  }

  location / {
  	proxy_pass http://balance;
  }
}
```



