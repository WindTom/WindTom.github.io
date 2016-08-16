---
layout: post
title: "MITK--如何在源文件中引用自定义头文件"
description: ""
category: MITK
tags: [头文件,MITK]
---
* content
{:toc}

### 引用方法:

MITK编程中，我们常常需要自定义头文件，比如在VS工程里面给插件AddNoise创建一个头文件testHeadFile.h：

<div align="center">
    <img src="https://github.com/WindTom/imagestom/blob/master/custom1.png?raw=true">
</div>





但是这样做不好的地方在于：团队开发中，我们是上传MITK-ProjectTemplate源文件，其它成员下载源文件后CMake编译一下就能使用了。你自己在VS工程创建头文件后，VS并不会把它自动放入MITK-ProjectTemplate源文件里。所以其他成员在编译你上传的代码后，VS会提示找不到头文件。

正确的做法是：将testHeadFile.h复制到源文件 MITK-ProjectTemplate\Plugins\my.awesomeproject.AddNoise\src文件夹内。

同时修改MITK-ProjectTemplate\Plugins\my.awesomeproject.AddNoise文件夹下的files.cmake文件：set(SRC_CPP_FILES)里添加 itkAdditiveGaussianNoiseImageFilter.h和itkNoiseBaseImageFilter.h，如：

```C++
  set(SRC_CPP_FILES  
  itkAdditiveGaussianNoiseImageFilter.h  
  itkNoiseBaseImageFilter.h  
  testHeadFile.h   //在此处添加 
```

### 测试：

使用CMake重新编译MITK-ProjectTemplate，进行测试。

### 替代方法：

如果上述方法不行，可以使用下述方法：

同样，将testHeadFile.h复制到源文件 MITK-ProjectTemplate\Plugins\my.awesomeproject.AddNoise\src文件夹内。

修改MITK-ProjectTemplate\Plugins\my.awesomeproject.AddNoise文件夹内的files.cmake,找到下列语句并将testHeadFile.h语句添加进去。如：

```C++
set(MOC_H_FILES  
  src/internal/my_awesomeproject_AddNoise_Activator.h  
  src/internal/AddNoise.h  
  src/itkAdditiveGaussianNoiseImageFilter.h  
  src/itkNoiseBaseImageFilter.h  
  testHeadFile.h  //在此处添加  
)  
```