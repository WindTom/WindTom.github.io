---
layout: post
title: "MITK中如何给图像添加高斯噪声"
description: ""
category: MITK
tags: [高斯噪声,图像,MITK]
---
* content
{:toc}

最近由于需要，要将读入的RGB图像转化成灰度图像，然后对图像加噪声，以验证去噪算法的效率。ITK在最新的4.6.0中就集成了Image Noise一个模块，里面有给图像加高斯噪声、斑点噪声、椒盐噪声和计算PSNR（峰值信噪比）的算法，但MITK使用的是ITK4.5.0的库，我们无法直接调用这些加噪Filter。




要想使用这些Filter方法也很简单。

1.下载ITK4.6.0，ITK4.6.0\Modules\Filtering\ImageNoise\include文件里就是所需要的所有头文件（图一），或者直接网上找所需要的头文件。
  <div align="center">
    <img src="https://github.com/WindTom/imagestom/blob/master/guassian1.png?raw=true">

    <p>图一：ImageNoise文件夹内的加噪Filter </p>
  </div>

比如我新建了一个AddNoise插件：

2.在插件内新建一个同名的头文件，如itkAdditiveGaussianNoiseImageFilter.h，并将内容复制进去。然后再把itkAdditiveGaussianNoiseImageFilter.hxx复制到新建的itkAdditiveGaussianNoiseImageFilter.h文件夹下(我的为Plugins\my.awesomeproject.AddNoise)。

我们注意到itkAdditiveGaussianNoiseImageFilter.h还引用了itkNoiseBaseImageFilter.h，这个头文件ITK4.5.0里也没有，可以用同样的方法加进来。

3.这些都做完后，我们发现itkAdditiveGaussianNoiseImageFilter.h头文件里的代码出错了：
PCH warning: header stop cannot be in a macro or #if block. An intellisense PCH file was not generated
方法是在头文件最上面加入一行代码：#pragma once
这样就可以了，插件完美运行，附截图一张：

   <div align="center">
      <img src="https://github.com/WindTom/imagestom/blob/master/guassian2.png?raw=true">
   </div>