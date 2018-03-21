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

## 配置SSL

### 产生SSL证书和密钥(使用已有的java keytool 生成的keystore中的密钥和证书)
1. 列出keystore中的内容
```
keytool -keystore xxxx.com.cert.jks -v -list
```     

2. 从keystore中取出der格式的证书
```
keytool -keystore xxxx.com.cert.jks  -export -alias _.xxxx.com > xxxx.com.cacert
```

3. 转换为pem格式
```
openssl x509 -out xxxx.com.cert.pem  -outform pem -text -in xxxx.com.cacert -inform der
```
也可以用keytool命令加上-rfc选项直接生成pem格式的证书
```
keytool -keystore xxxx.com.cert.jks -export -alias _.xxxx.com  -rfc
```

以上是采用旧版keystore格式时使用的命令。
按keytool提示，把keystore格式转换为pkcs12
```
keytool -importkeystore -srckeystore xxxx.com.cert.jks -destkeystore xxxx.com.cert.p12 -deststoretype pkcs12 -destkeypass xxxx123
```
转换完成后，查看pkcs12查看pkcs12格式的证书库
```
keytool -deststoretype PKCS12 -keystore xxxx.com.cert.p12 -list
```

4. 用以下命令导出证书
导出服务器证书
```
openssl pkcs12 -in xxxx.com.cert.p12 -nokeys -clcerts -out server-ssl.crt
```
导出CA证书
```
openssl pkcs12 -in xxxx.com.cert.p12 -nokeys -cacerts -out ca-.crt
```

5. 提取密钥
```
openssl pkcs12 -nocerts -nodes -in xxxx.com.p12 -out server.key
```

### 产生迪菲－赫尔曼密钥交换算法要求的参数
```
cd /etc/nginx/ssl
openssl dhparam -out dhparam.pem 2048
```

### 配置nginx支持SSL
```
listen       443 ssl;

#    ssl on;
   ssl_certificate /etc/nginx/ssl/server-ssl.crt;
   ssl_certificate_key /etc/nginx/ssl/server.key;

   # Diffie-Hellman parameter for DHE ciphersuites, recommended 2048 bits
   ssl_dhparam /etc/nginx/ssl/dhparam.pem;
   ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
   ssl_ciphers 'ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSAAES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHERSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHEECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHERSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSAAES256-SHA:ECDHE-ECDSA-DES-CBC3-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:DES-CBC3-SHA:!DSS';
   ssl_prefer_server_ciphers on;

   server_name  api.f5.chemchina.com;

```

## 配置nginx反向代理
```
upstream sapapi {
        server 10.8.14.123:8082;
}

server {
  location /sapapi/ {
       proxy_set_header HOST $proxy_host;
       proxy_set_header X-Real-IP $remote_addr;
       proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
       rewrite    /sapapi/([^/]+) /api/$1 break;
       proxy_http_version 1.1;
       proxy_pass http://sapapi;
   }

}
```
使用`curl  -iv "http://127.0.0.1/sapapi/dqzl_kcsj?material=000000001180150601&plant=2771"` 访问nginx,
nginx 使用 http://10.8.14.123:8082/api/dqzl_kcsj?material=000000001180150601&plant=2771 访问后台。

## 参考
[nginx docker image](https://hub.docker.com/_/nginx/)    
[Keytool to OpenSSL Conversion tips](https://conshell.net/wiki/index.php/Keytool_to_OpenSSL_Conversion_tips)
