---
layout: post
title: "MITK自定义插件CMake编译时出错"
description: ""
category: MITK 
tags: [cmake,mitk,plugin]
---
使用PluginGenerator1.4.0生成出来的MITK插件并不能直接放到projectTemplate里使用。原因是CMake编译的时候出错，会提示你修改刚刚生成插件中的CMakeLists.txt。

解决方法是：找到该txt，将MODULE_DEPENDIENCES QMITKExt 改为 MODULE_DEPENDS MitkQtWidgetsExt，保存后再次CMake编译就不会出错了。
     
备注：CMake编译指的是编译整个projectTemplate，不是编译一个单独的插件。 