---
layout: post
title:  "安装和配置Nginx Docker"
date:   2018-03-19 13:49:00 +0800
categories:
 - Nginx
tags:
 - Docker
 - Nginx
 - API Gateway   
---

##  安装
   下载nginx docker image
  ```
docker pull nginx
  ```

## 静态HTML文件
 ```
 docker run --name my-nginx -p 80:80 -v /www:/usr/share/nginx/html:ro -d nginx
 ```

## 复杂配置
```
docker run --name my-nginx -p 80:80 -p 443:443 -v /www:/usr/share/nginx/html:ro -v /mynginx/nginx.conf:/etc/nginx/nginx.conf:ro -d nginx
```

## 拷贝nginx缺省配置
```
docker run --name tmp-nginx-container -d nginx
docker cp tmp-nginx-container:/etc/nginx/nginx.conf /host/path/nginx.conf
docker rm -f tmp-nginx-containe
```

## 构建定制配置文件的image
1. 建立Dockerfile
```
FROM nginx
COPY nginx.conf /etc/nginx/nginx.conf
```
If you add a custom CMD in the Dockerfile, be sure to include -g daemon off; in the CMD in order for nginx to stay in the foreground, so that Docker can track the process properly (otherwise your container will stop immediately after starting)!

2. 构建
```
 docker build -t custom-nginx .
```

## 参考
[nginx docker image](https://hub.docker.com/_/nginx/)
