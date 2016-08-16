---
layout: post
title: "标准Lena测试图像下载"
description: ""
tagline:
category: 图像
tags: [Lena,image]
---

在做图像处理的时候常常用到Lena图像，但网上提供的测试图标准不一。这是Rice University提供的标准测试图，省去大家四处寻找之苦。另外，仔细阅读这个链接，你还会发现其他有用信息^.^
   
原文链接： http://www.ece.rice.edu/~wakin/images/



--
This test image was taken by our group in Duncan Hall at Rice University. The Matlab file for the 1024x1024 image is [here](http://www.ece.rice.edu/~wakin/images/paint.mat).

There seem to be many versions of the Lena (aka "Lenna") test image available. This problem was noted by Shapiro in his 1993 zerotree paper, and it remains surprisingly true today. This web page is an attempt to clear up some of the confusion (and hopefully not add to it).

The files on this page are given in lossless compression formats. These can be imported into Matlab or opened using photo-editing software. The imread command in Matlab is often useful for importing files such as tiff or bmp. (You may need to convert the data to type double after loading it). For example:

    lena512 = imread('lena512.bmp');

    lena512 = double(lena512);

To write an image to a file, use imwrite:

    imwrite(uint8(lena512),'lena512.bmp','bmp');

# 512x512 Color (24-bit) #
This seems to be a pretty widely accepted standard, which originated from a scan of the published photograph (see The Lenna Story). This version is also provided at the USC-SIPI Image Database.

   TIFF: lena512color.tiff (787kB)

# 512x512 Grayscale (8-bit) #

There doesn't seem to be as much agreement on this version of the image. At some point, the original color image needed to be converted to grayscale. As Shapiro noted, options included taking only the green component of the RGB representation, or using a luminance-only version. You could also average the RGB components, etc.

I'm not sure how the following version was generated, but it is provided by the Image Communications Lab at UCLA (they apparently obtained it from RPI).

Each of the following files should contain the same information. If there is truly a standard version of the 512x512 Grayscale Lena, this seems to be it.

BMP: lena512.bmp (263kB)

PGM: lena512.pgm (262kB)

MATLAB: lena512.mat (262kB)

# Comparing Different Versions #

To illustrate the importance of using a standard version, let's look at a few different ones that are floating around. We'll compress each of them using a JPEG2000 coder (obtained from The JasPer Project).

You can see the Matlab versions of the images here. If you click on the following links, you can see standard jpg files. (These are just for viewing and comparision purposes - use the matlab files for experiments!)

Image 1 is the standard which is discussed in the above section. Image 2 is another version which I've seen, and it looks and behaves very similar to the standard. Image 3 is obtained by taking the green component from the 24-bit original (described above). It appears darker than the others, and it is harder to compress. Image 4 is yet another version that seems to be floating around. Visually it is very different from the others... ...

![](http://i.imgur.com/Wx8GoSJ.jpg)