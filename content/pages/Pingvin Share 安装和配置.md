---
tags:
- Tech
- FOSS
date: 2023-01-08
title: Pingvin Share 安装和配置
categories:
lastMod: 2025-10-12
--- 
## 安装

下载docker-compose.yml，如果想要修改端口，需要更改左边的一个。

按官网提示使用docker：

```shell-session
$ docker compose up -d
```

安装完成，服务将默认运行在localhost:3000。

配置nginx，在http块中加一个server块：

```nginx
http {
  #...
  server {
      listen 80;
      server_name    [url];

      location / {
          proxy_pass    http://127.0.0.1:3000;
          proxy_set_header    Host    $proxy_host;
          proxy_set_header    X-Real-IP    $remote_addr;
          proxy_set_header    X-Forwarded-For    $proxy_add_x_forwarded_for;
      }
  }
}
```

测试，上线：

```shell-session
$ nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
$ systemxtl restart nginx
```

实地测试发现可能由于我nginx配置的问题，下载时重定向会出现[URL]/[URL]/share/api/...的情形，从而404。因此应用中app_url应设置为空，而不是[URL]。
 