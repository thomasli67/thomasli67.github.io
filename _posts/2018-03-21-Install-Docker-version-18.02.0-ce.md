---
layout: post
title:  "安装或升级到Docker 18.02.0-ce版"
date:   2018-03-21 16:45:00 +0800
categories:
 - Docker
tags:
 - Docker
---

以下是在CentOS 7下的安装过程

## 卸载之前安装的docker和kubernetes

   ```
   systemctl disable docker
   systemctl stop docker
   yum remove docker
   yum remove docker-common


   ```

## 安装最新版docker-ce   
```
wget -qO- https://get.docker.com/ | sh
```

## 启动docker引擎
```
systemctl start docker
systemctl enable docker
```

## 配置docker使用proxy
1. 建立如下目录
```
mkdir -p /etc/systemd/system/docker.service.d

```    

2. 建立配置文件
/etc/systemd/system/docker.service.d/http-proxy.conf
```
[Service]
Environment="HTTP_PROXY=http://10.8.7.19:3128/" "NO_PROXY=localhost,127.0.0.1,docker-registry.somecorporation.com"

```

3. 刷新系统、重启docker
```
systemctl daemon-reload
systemctl restart docker

```


## 参考
[Control Docker with systemd](https://docs.docker.com/config/daemon/systemd/)    
