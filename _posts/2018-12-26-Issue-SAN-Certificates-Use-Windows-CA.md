---
layout: post
title:  "配置和使用Windows CA 颁发SAN证书"
date:   2018-12-26 12:00:45 +0800
categories: [Microsoft, PKI]
tags: 
 - PKI
 - CA
 - SAN certificate
 - IIS
---


Chrome浏览器访问HTTPS网站时，需要服务器提供SAN(Subject Alternative Name)证书，否则会提示证书错误。Windows CA 缺省设置颁发的Web服务器证书不包括SAN扩展属性。以下记录了配置和使用企业CA（Enterprise Windows CA）颁发SAN证书的过程。环境为Windows 2016上运行的企业CA。    

## 1、配置CA服务器允许颁发SAN证书
在CA服务器上打开一个命令行窗口，执行如下命令：
```
certutil -setreq policy\EditFlags +EDITF_ATTRIBUTESUBJECTALTNAME2
```

![](/assets/images/issue-SAN-certificates-06.png)

重启Active Directory Certificates Services服务：

```
net stop certsvc
net start certsvc
```

## 2、申请证书、颁发证书以及安装证书

## 更多参考资源:

[Subject Alternative Name (SAN)](https://en.wikipedia.org/wiki/Subject_Alternative_Name)








