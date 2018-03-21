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
返回
```
Enter keystore password:
Keystore type: JKS
Keystore provider: SUN

Your keystore contains 1 entry

Alias name: _.xxxx.com
Creation date: Dec 8, 2017
Entry type: PrivateKeyEntry
Certificate chain length: 2
Certificate[1]:
Owner: CN=*.xxxx.com, OU=xxxx, O=中国xxxx, L=北京, ST=北京, C=CN
Issuer: CN=GeoTrust SSL CA - G3, O=GeoTrust Inc., C=US
Serial number: 631155e6775b3b43002a64a6108a2511
Valid from: Mon Mar 13 08:00:00 CST 2017 until: Fri Jun 12 07:59:59 CST 2020
Certificate fingerprints:
         MD5:  xx:xx:xx:xx:xx:E7:53:43:0C:CC:C2:80:6B:B9:1E:xx
         SHA1: xx:xx:xx:F0:CB:61:FB:BB:xx:B6:48:4C:D2:65:A0:65:36:12:AA:25
         SHA256: xx:17:FF:D4:2D:xx:52:79:41:4D:xx:29:8E:E1:05:2B:88:7B:0B:3C:1D:23:A8:B3:51:D9:1C:4C:6A:74:F5:16
Signature algorithm name: SHA256withRSA
Subject Public Key Algorithm: 2048-bit RSA key
Version: 3

Extensions:

#1: ObjectId: 1.3.6.1.4.1.11129.2.4.2 Criticality=false
0000: 04 82 01 E3 01 E1 00 76   00 DD EB 1D 2B 7A 0D 4F  .......v....+z.O
0010: A6 20 8B 81 AD 81 68 70   7E 2E 8E 9D 01 D5 5C 88  . ....hp......\.
0020: 8D 3D 11 C4 CD B6 EC BE   CC 00 00 01 5A C5 AE 0F  .=..........Z...
 ...
01D0: 05 A3 D1 9A BA 32 30 68   46 AE EF D7 CD 8E FA 07  .....20hF.......
01E0: 13 C1 EF E7 4A B8 7B                               ....J..


#2: ObjectId: 1.3.6.1.5.5.7.1.1 Criticality=false
AuthorityInfoAccess [
  [
   accessMethod: ocsp
   accessLocation: URIName: http://gn.symcd.com
,
   accessMethod: caIssuers
   accessLocation: URIName: http://gn.symcb.com/gn.crt
]
]

#3: ObjectId: 2.5.29.35 Criticality=false
AuthorityKeyIdentifier [
KeyIdentifier [
0000: XX 6F XX 96 F4 85 XX 72   3C 30 7D 23 DA 85 78 9B  .o....?r<0.#..x.
0010: XX 7C 5A 7C                                        ..Z.
]
]

#4: ObjectId: 2.5.29.19 Criticality=false
BasicConstraints:[
  CA:false
  PathLen: undefined
]

#5: ObjectId: 2.5.29.31 Criticality=false
CRLDistributionPoints [
  [DistributionPoint:
     [URIName: http://gn.symcb.com/gn.crl]
]]

#6: ObjectId: 2.5.29.32 Criticality=false
CertificatePolicies [
  [CertificatePolicyId: [2.23.140.1.2.2]
[PolicyQualifierInfo: [
  qualifierID: 1.3.6.1.5.5.7.2.1
  qualifier: 0000: 16 33 68 74 74 70 73 3A   2F 2F 77 77 77 2E 67 65  .3https://www.ge
0010: 6F 74 72 75 73 74 2E 63   6F 6D 2F 72 65 73 6F 75  otrust.com/resou
0020: 72 63 65 73 2F 72 65 70   6F 73 69 74 6F 72 79 2F  rces/repository/
0030: 6C 65 67 61 6C                                     legal

], PolicyQualifierInfo: [
  qualifierID: 1.3.6.1.5.5.7.2.2
  qualifier: 0000: 30 35 0C 33 68 74 74 70   73 3A 2F 2F 77 77 77 2E  05.3https://www.
0010: 67 65 6F 74 72 75 73 74   2E 63 6F 6D 2F 72 65 73  geotrust.com/res
0020: 6F 75 72 63 65 73 2F 72   65 70 6F 73 69 74 6F 72  ources/repositor
0030: 79 2F 6C 65 67 61 6C                               y/legal

]]  ]
]

#7: ObjectId: 2.5.29.37 Criticality=false
ExtendedKeyUsages [
  serverAuth
  clientAuth
]

#8: ObjectId: 2.5.29.15 Criticality=true
KeyUsage [
  DigitalSignature
  Key_Encipherment
]

#9: ObjectId: 2.5.29.17 Criticality=false
SubjectAlternativeName [
  DNSName: *.xxx.com
  DNSName: xxx.com
]

Certificate[2]:
Owner: CN=GeoTrust SSL CA - G3, O=GeoTrust Inc., C=US
Issuer: CN=GeoTrust Global CA, O=GeoTrust Inc., C=US
Serial number: 23a6f
Valid from: Wed Nov 06 05:36:50 CST 2013 until: Sat May 21 05:36:50 CST 2022
Certificate fingerprints:
         MD5:  xx:xx:B6:85:9F:D7:64:E7:87:EE:42:FD:3A:93:05:xx
         SHA1: xx:EA:EE:3F:7F:2A:94:49:CE:BA:FE:EC:68:FD:D1:84:F2:01:xx:A7
         SHA256: xx:45:41:EC:DF:88:ED:99:xx:xx:AD:E3:EC:DD:EF:27:A2:6B:A1:B4:44:80:A1:95:C0:A8:DA:DA:E2:52:1D:xx
Signature algorithm name: SHA256withRSA
Subject Public Key Algorithm: 2048-bit RSA key
Version: 3

Extensions:

#1: ObjectId: 1.3.6.1.5.5.7.1.1 Criticality=false
AuthorityInfoAccess [
  [
   accessMethod: ocsp
   accessLocation: URIName: http://g2.symcb.com
]
]

#2: ObjectId: 2.5.29.35 Criticality=false
AuthorityKeyIdentifier [
KeyIdentifier [
0000: C0 7A 98 68 8D 89 FB AB   05 64 0C 11 7D AA 7D 65  .z.h.....d.....e
0010: B8 CA CC 4E                                        ...N
]
]

#3: ObjectId: 2.5.29.19 Criticality=true
BasicConstraints:[
  CA:true
  PathLen:0
]

#4: ObjectId: 2.5.29.31 Criticality=false
CRLDistributionPoints [
  [DistributionPoint:
     [URIName: http://g1.symcb.com/crls/gtglobal.crl]
]]

#5: ObjectId: 2.5.29.32 Criticality=false
CertificatePolicies [
  [CertificatePolicyId: [2.16.840.1.113733.1.7.54]
[PolicyQualifierInfo: [
  qualifierID: 1.3.6.1.5.5.7.2.1
  qualifier: 0000: 16 25 68 74 74 70 3A 2F   2F 77 77 77 2E 67 65 6F  .%http://www.geo
0010: 74 72 75 73 74 2E 63 6F   6D 2F 72 65 73 6F 75 72  trust.com/resour
0020: 63 65 73 2F 63 70 73                               ces/cps

]]  ]
]

#6: ObjectId: 2.5.29.15 Criticality=true
KeyUsage [
  Key_CertSign
  Crl_Sign
]

#7: ObjectId: 2.5.29.17 Criticality=false
SubjectAlternativeName [
  CN=SymantecPKI-1-539
]

#8: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: D2 6F F7 96 F4 85 3F 72   3C 30 7D 23 DA 85 78 9B  .o....?r<0.#..x.
0010: A3 7C 5A 7C                                        ..Z.
]
]



*******************************************
*******************************************



Warning:
The JKS keystore uses a proprietary format. It is recommended to migrate to PKCS12 which is an industry standard format using "keytool -importkeystore -srckeystore chemchina.com.cert.jks -destkeystore chemchina.com.cert.jks -deststoretype pkcs12".

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
