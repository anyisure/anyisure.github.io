---
title: nginx安装使用教程
tags:
  - nginx
  - devops
abbrlink: 1cb0de64
date: 2024-09-20 16:35:08
cover: ../img/nginx.png
---

> Nginx (engine x) 是一个高性能的HTTP和反向代理web服务器 ，同时也提供了IMAP/POP3/SMTP服务。用来做**负载均衡**及**反向代理**使用。

官网：https://nginx.org/

### 配置文件
- xx.conf
```conf
    server {
        listen       7001 ssl;
        server_name  _;

        ssl_certificate /cert/ssl/112.132.154.67.crt;
        ssl_certificate_key /cert/ssl/112.132.154.67.key;
        # ssl_trusted_certificate /path/to/intermediate.crt;
        ssl_protocols TLSv1.2 TLSv1.3;
        ssl_session_cache shared:SSL:1m;
        ssl_session_timeout  10m;
        ssl_ciphers HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers on;
       
        location / {
            proxy_pass              http://localhost:7011;
            proxy_set_header        Host $host:$server_port;
            proxy_set_header        X-Real-IP $remote_addr;
            proxy_set_header        X-Forwarded-Port $server_port;
            proxy_set_header        X-Forwarded-Proto https;
            proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_redirect http:// https://;

        }
    }

```
