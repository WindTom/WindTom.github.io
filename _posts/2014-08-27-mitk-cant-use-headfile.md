---
layout: post
title: "MITK中本来存在的头文件无法使用"
description: ""
category: MITK
tags: [mitk,cmake]
---
* content
{:toc}

在使用MITK 插件的时候，我们会发现本来存在的头文件，mitk工程里却无法include进来，原因是project---properties---C/C++---General----Additional Include Directories里并没有把相关头文件的文件路径添加进去。





比如ITK Modules中Group Filtering内的 Module ITKColormap，我们无法引用里面的 itkScalarToRGBColormapImageFilter.h

比较方便的办法是，找到插件源文件下的CMakeLists.txt文件，在里面加入`PACKAGE_DEPENDS ITK || ITKColorma`。CMakeLists.txt内容如下

```
project(my_awesomeproject_myLevelSet)  
find_package (OPENCV REQUIRED)  
include_directories(${OPENCV_INCLUDE_DIRS})   
  
MACRO_CREATE_MITK_CTK_PLUGIN(  
  EXPORT_DIRECTIVE MYLEVELSET_EXPORT  
  EXPORTED_INCLUDE_SUFFIXES src  
  MODULE_DEPENDS MitkQtWidgetsExt MitkSegmentation  
  PACKAGE_DEPENDS ITK|ITKColormap  
```