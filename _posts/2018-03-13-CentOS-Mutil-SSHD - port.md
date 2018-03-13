---
layout: post
title:  "在CentOS Linux系统上，为SSHD添加新的端口 "
date:   2018-03-13 09:45:00 +0800
categories:
 - Linux
 - Configure
tags:
 - Linux
 - SELinux
---

## 配置
   以下是在CentOS 7 下配置步骤

   1. 编辑/etc/ssh/sshd_config
   ```
   vi /etc/ssh/sshd_config
   ```
   添加
   ```
   Port 222
   Port 2222
   ```

   2. 设置SELinux
   ```
   semanage port -a -t ssh_port_t -p tcp 222
   semanage port -a -t ssh_port_t -p tcp 2222
   ```
   如果没有semanage命令，用下面命令安装
   ```
   yum -y install policycoreutils-python
   ```

   3. 调整防火墙
   ```
   firewall-cmd --permanent --zone=public --add-port=2222/tcp
   firewall-cmd --permanent --zone=public --add-port=222/tcp
   firewall-cmd --reload
   ```

   4. 重启sshd
   ```
   systemctl restart sshd
   ```

   5. 检查sshd是否监听新端口
   ```
    netstat -antp | grep sshd
   ```    

   6. 测试
   ```
   ssh -p 2222 test@192.168.xx.xx
   ```  

## 参考
[CentOS 7 / RHEL 7 : change OpenSSH port number(SELINUX enabled )](http://sharadchhetri.com/2014/10/15/centos-7-rhel-7-change-openssh-port-number-selinux-enabled/)
