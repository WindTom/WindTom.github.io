---
layout: post
title: "MITK插件找不到itk的头文件"
description: ""
category: MITK
tags: [MITK,itk,头文件,插件]
---

插件中编码时，想使用itk中的算法，就必须先引用该算法头文件，但很有可能VS找不到这个头文件。你可能会选择在工程设置中手动添加头文件，但这种方法非常费力，且新建一个工程时又得重新添加。
好的办法是，找到projectTemplate中Plugins文件夹下的该插件文件夹，如my.awesomeproject.AddNoise.

找到CMakeLists.txt文档，将最后一句改为MODULE_DEPENDS MitkQtWidgetsExt MitkSegmentation。然后重新编译projectTemplate即可。
     
附：（插件下CMakeLists.txt文档中的内容）

```
project(my_awesomeproject_AddNoise)  
	  
   MACRO_CREATE_MITK_CTK_PLUGIN(  
   EXPORT_DIRECTIVE ADDNOISE_EXPORT  
   EXPORTED_INCLUDE_SUFFIXES src  
   MODULE_DEPENDS MitkQtWidgetsExt MitkSegmentation  
)
```