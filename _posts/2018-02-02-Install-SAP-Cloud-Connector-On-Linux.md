---
layout: post
title:  "在Linux上安装SAP Cloud Connect"
date:   2018-01-31 15:00:45 +0800
categories: 
 - SAP
 - SAP Cloud Platform
tags: 
 - SAP Cloud Platform
 - On-premise Connectivity
---

## 安装
   以下是在CentOS 7 下安装步骤

    1. 安装java 1.8.0  
   `yum install java-1.8.0-openjdk.x86_64`


    2. 下载 sapcc-2.10.2-linux-x64.zip
    
   
    3. 安装  
    `rpm -i com.sap.scc-ui-2.10.2-1.x86_64.rpm`


    4. 启动  
   `service  scc_daemon start` 
    

## 配置 
   1. 用浏览器打开 https://host:8433

   2. 用  Administrator / manage 登陆

   3. 修改口令 chem123
   
   4. 配置与SAP Cloud的连接  
     Region Host : 
     Subaccount : `bf875ea9-aa87-49e7-bcb2-2d602c64a70b`
     Subaccount User : `trial`
     Password : 
     



## SAP Cloud Platform 试用
###  Cloud Foundry 试用

1. 缺省子账号 trial  

``

区域: 欧洲（法兰克福）

全局账户: P2000125067trial

子账户: trial

组织: P2000125067trial_trial

空间: dev  

``

![](/assets/images/sap-cloud-try-01.png)

trial sub account ID: `bf875ea9-aa87-49e7-bcb2-2d602c64a70b`

2. 建立新的账号
  
账号名：lizhaohui

子域: p2000125067poc 

ID： 


## 更多参考资源:

[SAP Cloud Platform Connectivity](https://help.sap.com/viewer/cca91383641e40ffbe03bdc78f00f681/Cloud/en-US/e54cc8fbbb571014beb5caaf6aa31280.html)

