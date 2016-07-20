---
layout: post
title: "Creating a 2D Active Shape Model in ITK Using PCA"
description: ""
category: MITK
tags: [ITK,ASM,PCA]
---

原帖地址：http://www.kitware.com/source/home/post/52

ITK is an excellent example of an open-source framework in extreme programming. Strengths of the toolkit include superb documentation and list serve support, and a supportive and vibrant programming community. One missing area of documentation in the ITK software guide includes the implementation of itk::GeodesicActiveContourShapePriorLevelSetImageFilter. The generation of the active shape model (ASM) in this example is provided with no example code or images. Here, we provide sample images and commented code to generate an ASM using itk::ImagePCAShapeModelEstimator. We use the challenging problem of segmenting the femoral condyle cartilage of the knee in our example.  

Many classic described segmentation algorithms such as active contours (snakes), level set, and watershed rely on edge-based criteria [1-4].  For segmentation of articular femoral knee cartilage, a solely edge-based algorithm is inadequate because of the poor contrast at the cartilage and soft tissue interfaces.  One approach is to use a priori information to create an active shape model to help guide segmentation [5].

The goal of an active shape model is to appropriately describe all of the allowed statistical variation of some generic shape.  The model is formed by using a training set of objects that are already segmented either manually or by some other automated or semi-automated method. The variation among the set of these previously segmented shapes is used to describe the variation of the shape model and therefore the training set needs to be a good representation of the overall distribution of allowable shapes.  This can be accomplished with a large sample size or good variation among the training images.

Principal component analysis (PCA) is used to decompose the large variation in the natural shapes of a given object into a set of variables that describe the majority of the variation.  The basic steps of PCA include aligning the training images, finding the corresponding landmarks, computing the covariance matrix of these landmarks, and finally finding the determinate of the covariance matrix.

In the first step of PCA, the training images should be transformed iteratively to maximize their overlapping area.  In the classical implementation, the N0 iteration consisted of the training images being registered with the first training image using a similarity transform that can rotate, scale, and translate the image.  After this first iteration, the mean image is computed.  For the following iterations, the training images are registered to the mean image, which is then recomputed. This process eventually converges and results in a set of training images aligned with maximal overlap. A subset of aligned training images used to create the femoral cartilage shape model are shown in Figure1. 