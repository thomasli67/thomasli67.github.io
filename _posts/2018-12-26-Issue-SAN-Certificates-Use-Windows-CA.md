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
用浏览器打开CA服务器WEB地址，https://issuingca.corp.cncic.cn/certsrv
点击“申请证书（Request a certificate）” 、 “高级证书申请（advanced certificate request）”、“  
创建并向此 CA 提交一个申请（Request and submit a request to this CA）”
在“证书模板（Certificate Template）” 选择“My Web服务器”，填写需要的信息，其中“属性” 填写SAN信息：
san:dns=*.corp.cncic.cn&dns=*.corpaddins.cncic.cn

![](/assets/images/issue-SAN-certificates-01.png)

![](/assets/images/issue-SAN-certificates-02.png)

点击提交后，

![](/assets/images/issue-SAN-certificates-03.png)

登录到CA服务器，可以看到提交的证书申请

![](/assets/images/issue-SAN-certificates-03-1.png)

在CA上颁发证书后，再回到申请证书的浏览器

![](/assets/images/issue-SAN-certificates-03-2.png)

选择“安装此证书”，查看新安装的证书，可以看到SAN属性

![](/assets/images/issue-SAN-certificates-04.png)

为了在其他服务器安装此证书，选择导出证书（包括私钥）

![](/assets/images/issue-SAN-certificates-05.png)

![](/assets/images/issue-SAN-certificates-05-1.png)

在其他服务器IIS上安装此证书

![](/assets/images/issue-SAN-certificates-07.png)

## 更多参考资源:

[Subject Alternative Name (SAN)](https://en.wikipedia.org/wiki/Subject_Alternative_Name)








