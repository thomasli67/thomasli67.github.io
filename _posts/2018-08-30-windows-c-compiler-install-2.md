---
layout: post
title:  "windows下安装python的C扩展编译环境之二 Python 3.5"
date:   2018-08-30 10:02:00 +0800
categories:
 - Python
tags:
 - Python
 - C/C++
---


在windows下使用pip安装一些python的第三方库，有很多使用C写了一些扩展，需要使用VC++ Compiler 来编译安装，否则就会出现“error: command 'cl.exe' failed:”。最常见的编译器是Visual Studio C ++。

对于python3.5 则需安装微软提供的编译器——Visual C++ Build Tools

下载地址： https://www.visualstudio.com/downloads/#build-tools-for-visual-studio-2017
安装的时候记得选 Windows 8.1 SDK 和 Windows 10 SDK ，这样不装 VS2015 也可以编译 pip 中有 C 代码的包了。


。。。待完善
  


## 参考
[Visual Studio Build Tools now include the VS2017 and VS2015 MSVC Toolsets](https://blogs.msdn.microsoft.com/vcblog/2017/11/02/visual-studio-build-tools-now-include-the-vs2017-and-vs2015-msvc-toolsets/)    
