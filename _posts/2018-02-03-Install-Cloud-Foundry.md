---
layout: post
title:  "安装和使用Cloud Foundry CLI"
date:   2018-02-3 10:00:00 +0800
categories: 
 - SAP
 - SAP Cloud Platform
 - Cloud Foundry
tags: 
 - SAP Cloud Platform
 - Cloud Foundry
---

## 安装
   以下是在CentOS 7 下安装步骤

   1. first configure the Cloud Foundry Foundation package repository
   `wget -O /etc/yum.repos.d/cloudfoundry-cli.repo https://packages.cloudfoundry.org/fedora/cloudfoundry-cli.repo`

   2. install the cf CLI (which will also download and add the public key to your system)
    `yum install cf-cli`

   3. 如果以上方法不行，可以下载  `cf-cli-installer_6.34.1_x86-64.rpm` 后，直接安装。

   4. 下载 MTA Plugin和Service Fabrik based B&R Plugin  
   `wget https://tools.hana.ondemand.com/additional/cfcliplugin/cf-cli-mta-plugin-1.0.2-linux-x86_64.bin`
   `wget https://tools.hana.ondemand.com/additional/cfcliplugin/service-fabrik-cli-plugin-1.0.3-linux-x86_64.tar.gz`
      
   5. 安装plugin 
    

## 使用 
   1. `cf api https://api.cf.eu10.hana.ondemand.com`

   2. `cf login`

   3. `cf push HelloDockerLi --docker-image shuuse/hellodocker:latest`
   
``
Pushing app HelloDockerLi to org P2000125067trial_trial / space dev as cncc2018@gmail.com...
Getting app info...
Creating app with these attributes...
+ name:           HelloDockerLi
+ docker image:   shuuse/hellodocker:latest
  routes:
+   hellodockerli.cfapps.eu10.hana.ondemand.com

Creating app HelloDockerLi...
Mapping routes...

Staging app and tracing logs...

Waiting for app to start...

name:              HelloDockerLi
requested state:   started
instances:         1/1
usage:             1G x 1 instances
routes:            hellodockerli.cfapps.eu10.hana.ondemand.com
last uploaded:     Sat 03 Feb 11:21:15 CST 2018
stack:             cflinuxfs2
docker image:      shuuse/hellodocker:latest
start command:     /entrypoint.sh

     state     since                  cpu    memory    disk      details
     #0   running   2018-02-03T03:21:41Z   0.0%   0 of 1G   0 of 1G
``
   
  4. 用 cf apps 看部署的应用
```
cf apps

Getting apps in org P2000125067trial_trial / space dev as cncc2018@gmail.com...
OK

name            requested state   instances   memory   disk   urls
HelloDockerLi   started           1/1         1G       1G     hellodockerli.cfapps.eu10.hana.ondemand.com

```     



## 更多参考资源:



