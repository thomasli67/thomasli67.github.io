---
layout: post
title:  "SELinux与Docker"
date:   2018-03-19 14:45:00 +0800
categories:
 - SELinux
tags:
 - SElinux
 - Docker
---

## 问题
   使用Docker命令行参数` -v /www:/usr/share/nginx/html:ro`  后，用命令
   ```
   docker exec -ti my-docker bash
   ```
   进入容量后，执行`ls -l /usr/share/nginx/html `, 返回
   ```
   ls: cannot open directory '/usr/share/nginx/html': Permission denied
   ```

## 诊断和解决   
   以下是在CentOS 7 下配置、使用SELinux命令  

1. 查看当前SELinux配置
   使用getenforce命令，返回Enforcing、Permissive，或者 Disabled
   例如：
   ```
     getenforce
   ```
   返回
   ```
   Enforcing
   ```

2. 修改SELinux配置  
   查看和修改 `/etc/selinux/config`
```
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=enforcing
# SELINUXTYPE= can take one of these two values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected.
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
```
3. 显示SELinux的系统的详细状态报告
```
sestatus -v
```
返回  
```
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   enforcing
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Max kernel policy version:      28
....
```

4. 检查文件属性
```
 ls -Z /
```
 返回
```
......
 drwxrwxrwt. root root system_u:object_r:tmp_t:s0       tmp
drwxr-xr-x. root root system_u:object_r:usr_t:s0       usr
drwxr-xr-x. root root system_u:object_r:var_t:s0       var
drwxr-xr-x. root root unconfined_u:object_r:default_t:s0 www
```
查看/www
 ```
 ls -Z /www
 ```
 返回
 ```
 -rw-r--r--. root root unconfined_u:object_r:default_t:s0 beginners_guide.html
-rw-r--r--. root root unconfined_u:object_r:default_t:s0 index.html
 ```

5. 查看Dockers容器使用的label
```
ps -efZ | grep ngnix
```
返回
```
system_u:system_r:svirt_lxc_net_t:s0:c30,c173 root 16657 16637  0 14:24 ? 00:00:00 nginx: master process nginx -g daemon off;
system_u:system_r:svirt_lxc_net_t:s0:c30,c173 101 16683 16657  0 14:24 ? 00:00:00 nginx: worker process
```

6. 修改文件属性
Docker容器进程运行时使用 system_u:system_r:svirt_lxc_net_t:s0 标签
需要修改Docker容器使用的卷的文件属性
```
chcon -Rt svirt_sandbox_file_t  /www
```

## 另一种解决办法
docker 1.7 的-v可以使用z和Z选项，如
```
docker run --name my-nginx -p 80:80 -v /www2:/usr/share/nginx/html:z -d nginx
```
docker会自动执行`chcon -Rt svirt_sandbox_file_t /www2 `

使用大写Z选项时，docker会精确匹配容器进程的MCS label，自动执行类似`chcon -Rt svirt_sandbox_file_t -l s0:c1,c2 /www2`的命令。其中s0:c1,c2匹配容器进程的label，不同的容器label不同。

## 参考
[Wiki:Security-Enhanced Linux](https://en.wikipedia.org/wiki/Security-Enhanced_Linux)    
[Using Volumes with Docker can Cause Problems with SELinux](http://www.projectatomic.io/blog/2015/06/using-volumes-with-docker-can-cause-problems-with-selinux/)     
[Use volumes](https://docs.docker.com/storage/volumes/)
