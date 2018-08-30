---
layout: post
title:  "windows下安装python的C扩展编译环境之一 Python 2.7"
date:   2018-08-30 10:02:00 +0800
categories:
 - Python
tags:
 - Python
 - C/C++
---


在windows下使用pip安装一些python的第三方库，有很多使用C写了一些扩展，需要使用VC++ Compiler 来编译安装，否则就会出现“error: command 'cl.exe' failed:”。最常见的编译器是Visual Studio C ++。Python 2.7 使用的是 VS 2008编译的，所以Python 2.7默认只能认出VS 2008。

### 下载和安装、测试

1、下载 VCForPython27.msi 并安装

2、安装 Visual C++ 2008 64-bit 编译环境

3、进入Visual C++ 2008 64-bit Command Prompt
测试c命令行程序(console)编译

```c
#include <stdio.h>
int main()
{
   // printf() displays the string inside quotation
   printf("Hello, World!");
   return 0;
}
```

编译 hello.c

```cmd
cl hello.c
```

编译c++

```cpp
#include <iostream>
using namespace std;
int main(int argc, char* argv[])
{ 
    cout << "hello, world for c++!" << endl;
    return 0;
}
```

4、测试c windows程序

```c
#ifndef UNICODE
#define UNICODE
#endif

#include<windows.h>


int WINAPI wWinMain(HINSTANCE hinstance, HINSTANCE hprevinstance, PWSTR szCmdLine, int nCmdShow)
{
MessageBox(NULL,L"Hello World",L"Hello",MB_OK);
return 0;

}
```

编译时要加上link程序用的到的win32库，如kernel32.lib user32.lib gdi32.lib winspool.lib comdlg32.lib advapi32.lib shell32.lib ole32.lib oleaut32.lib uuid.lib odbc32.lib odbccp32.lib OpenGL32.Lib

```cmd
cl hellomsg.c user32.lib
```

### 安装没有wheel需要编译的python包
安装VS2008后，安装pycrypto时，使用

```cmd
python setup.py build
```

报错 error: Unable to find vcvarsall.bat
问题原因：    
Look in the setup.py file of the package you are trying to install. If it is an older package it may be importing distutils.core.setup() rather than setuptools.setup().

I ran in to this (in 2015) with a combination of these factors:

The Microsoft Visual C++ Compiler for Python 2.7 from http://aka.ms/vcpython27

An older package that uses distutils.core.setup()

Trying to do python setup.py build rather than using pip.

If you use a recent version of pip, it will force (monkeypatch) the package to use setuptools, even if its setup.py calls for distutils. However, if you are not using pip, and instead are just doing python setup.py build, the build process will use distutils.core.setup(), which does not know about the compiler install location.

解决办法：
Solution
Step 1: Open the appropriate Visual C++ 2008 Command Prompt

Open the Start menu or Start screen, and search for "Visual C++ 2008 32-bit Command Prompt" (if your python is 32-bit) or "Visual C++ 2008 64-bit Command Prompt" (if your python is 64-bit). Run it. The command prompt should say Visual C++ 2008 ... in the title bar.

Step 2: Set environment variables

Set these environment variables in the command prompt you just opened.
```cmd
SET DISTUTILS_USE_SDK=1
SET MSSdk=1
```
Reference http://bugs.python.org/issue23246

Step 3: Build and install

cd to the package you want to build, and run python setup.py build, then python setup.py install. If you want to install in to a virtualenv, activate it before you build.

如果build pycrypto报编译错误    
tomcrypt_cipher.h(546) : error C2133: 'cipher_descriptor' : unknown size
可以修改
To fix the error, edit line 546 of "tomcrypt_cypher.h" to remove "cipher_descriptor[]". This variable doesn't seem to be declared, used, or otherwise exist outside of the header here, and pycrypto passed the full test suite after being compiled with these changes.

1. 安装

build可以成功,使用如下命令安装

```cmd
python setup.py install
```

2. 源码打包

```cmd
python setup.py sdist
```

在dist目录下生成源代码包pycrypto-2.7a1.zip

3. 二进制打包

```cmd
python setup.py bdist
```

在dist目录下生成windows下二进制包pycrypto-2.7a1.win-amd64.zip

新版setup.py(使用from setuptools import setup, Extension，旧版from distutils.core import setup, Extension),如编译dulwith模块

1. egg

```cmd
python setup.py bdist_egg
```

dist 下生成dulwich-0.19.6-py2.7-win-amd64.egg

2. wheel

```cmd
python setup.py bdist_wheel
```

dist 下生成dulwich-0.19.6-cp27-cp27m-win_amd64.whl

### 注释

vc版本与vs版本对应关系如下所示：

* Visual Studio 6 ： vc6
* Visual Studio 2003 ： vc7
* Visual Studio 2005 ： vc8
* Visual Studio 2008 ： vc9
* Visual Studio 2010 ： vc10
* Visual Studio 2012 ： vc11
* Visual Studio 2013 ： vc12
* Visual Studio 2014 ： vc13
* Visual Studio 2015 ： vc14
* Visual Studio 2017 ： vc15

注意：Python 3.5升级了distutils，默认使用_msvccompiler.py，在这个文件中可以找到：“ if version >= 14 and version > best_version: ”这里的14说明VS版本要在14以上才可以。所以根据这句，我们要安装最新的Visual Studio2015。

### 微软官方解决方案

| Python Version| You will need |
| ------------- |---------------|
| 3.5 and later |Update: Install Visual Studio 2017, select the Python development workload and the Native development tools option.    [Visual C++ Build Tools 2015](http://go.microsoft.com/fwlink/?LinkId=691126) or [Visual Studio 2015](https://visualstudio.com/) |
| 3.3 and 3.4   | [Windows SDK for Windows 7 and .NET 4.0](https://www.microsoft.com/download/details.aspx?id=8279) (Alternatively, Visual Studio 2010 if you have access to it)      |
| 2.6 to 3.2    | [Microsoft Visual C++ Compiler for Python 2.7](https://www.microsoft.com/download/details.aspx?id=44266)      |

### 参考

[Microsoft Visual C++ Compiler for Python 2.7](http://www.microsoft.com/en-us/download/details.aspx?id=44266)  
[How to deal with the pain of “unable to find vcvarsall.bat”](https://blogs.msdn.microsoft.com/pythonengineering/2016/04/11/unable-to-find-vcvarsall-bat/)  
[Python Wheels](https://pythonwheels.com/)    
[PEP 427 -- The Wheel Binary Package Format 1.0](https://legacy.python.org/dev/peps/pep-0427/)  
[Python on Wheels](http://lucumr.pocoo.org/2014/1/27/python-on-wheels/)

