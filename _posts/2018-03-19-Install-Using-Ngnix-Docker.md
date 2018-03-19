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

## 参考
[]()
