---
layout: post
title: "MITK编程流程"
description: ""
category: MITK
tags: [mitk,plugin]
---

MITK官方网站：www.mitk.org

使用MITK编程的一般流程为：




1. 创建自定义插件  
   使用PlugInGenerator创建一个自定义插件，注意插件的命名要符合MITKTemplate的命名方式，插件可以存放在任何位置，下一步会拷贝至template文件夹下。

2. 拷贝自定义插件  
   将生成的插件(Plugin)放进MITKTemplate文件夹中的Plugins文件夹下，并修改该文件夹下的Plugins.cmake，将自定义插件添加进去。

3. 使用CMake编译MITKTemplate
   注意将两个superbuild选项设为OFF，同时将编译好的ITK、VTK、DCMTK等路径填好。

4. 打开编译好的Template，使用StartVS_debug.bat，或者StartVS_release打开相应版本的工程。

5. 将AwsomeAPP设为启动项。

6. 运行测试工程。

7. 打开插件的.h和.cpp文件，编写自己的程序。