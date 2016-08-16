---
layout: post
title: "如何使用MITK定义的插件"
description: ""
category: MITK
tags: [mitk,qt]
---
* content
{:toc}

使用MITK编辑界面的时候，QT Designer中只显示 QT 自带的组件，但如果你想用MITK中的QT组件，比如QmitkDataStorageComboBox ，应该怎么做呢？

方法是在QT Designer中创建一个普通的combo box，然后右键点击，选择“提升为”，在弹出对话框中，“提升的类名称：”QmitkDataStorageComboBox，头文件QmitkDataStorageComboBox.h，然后点击添加就可以了。非常方便！