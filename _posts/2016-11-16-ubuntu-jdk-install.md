---
layout: post
title: "ubuntu 中安装和配置 java JDK"
description: ""
category: 数据挖掘
tags: [java,linux,ubuntu]
---
* content
{:toc}

本文参考资料： [1](http://www.cnblogs.com/bluestorm/archive/2012/05/10/2493592.html)





##下载jdk

jdk-8u111-linux-x64.tar.gz [下载地址1](http://www.oracle.com/technetwork/cn/java/javase/downloads/jdk8-downloads-2133151-zhs.html)

##解压缩
 tar -zxvf jdk-8u111-linux-x64.tar.gz
 将解压好的文件夹用最高权限复制到/usr/lib/jvm中
sudo cp -r jdk1.8.0 /usr/lib/jvm
## 配置环境变量
cd /etc
sudo gedit profile

在末尾加上:
export JAVA_HOME=/usr/lib/jvm/jdk1.8.0
export JRE_HOME=${JAVA_HOME}/jre 
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH

然后保存关闭，使用source更新下
source profile

使用env命令察看JAVA_HOME的值
$ env
如果JAVA_HOME=/usr/lib/jvm/jdk1.7.0_21，说明配置成功。

## 将系统默认的jdk修改过来
$ sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0/bin/java 300 

输入sun jdk前的数字就好了
$ sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0/bin/javac 300 

$ sudo update-alternatives --config java 
$ sudo update-alternatives --config javac

##查看是否安装成功
然后再输入java -version,看到如下信息，就说明改成sun的jdk了:
java version "1.8.0"
Java(TM) SE Runtime Environment (build 1.8.0)
Java HotSpot(TM) Server VM (build 23.0-b21, mixed mode)

 

可能会存在的问题：
1.提示缺失libjli.so无法启动……，碰到这个问题是你下载的JavaJDK压缩包不完整，或者你的解压方式不对导致，直接解压到当前路径，然后拷贝到你需要的目录，JDK的安装目录可以随便选择，比如你可以放在HOME目录下。
libjli.so文件在：～/jdk1.8.0/jre/lib/i386/jli/libjli.so
2.可能无法配置成功，需要卸载以前安装的OpenJDK，具体可以命令行移除
3.不同版本的JDK，版本号如上修改即可
