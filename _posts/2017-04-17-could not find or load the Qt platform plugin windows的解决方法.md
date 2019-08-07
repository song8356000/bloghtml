---
title: could not find or load the Qt platform plugin windows的解决方法
date: 2018-09-19 18:10:50
tags: CSDN迁移
---
   VS+Qt5环境下建立一个Qt工程，在本机运行没问题，可是把.exe和用到的.dll打包发到别人电脑上却运行不了，报错如下：

 ![](https://img-blog.csdn.net/20170503103608656)

 

 为什么会这样？这是因为程序运行需要Qt本身的一些dll，把这些缺失的dll补上就可以了。

 

 需要注意的是：一般遇到这个报错，是缺少plugins文件夹下的platforms和imageformats两个文件夹内的dll，但是一定不要直接把这两个文件夹下的dll直接扔到.exe同级目录下，而是要把这两个文件夹直接扔到.exe同级目录下：

 ![](https://img-blog.csdn.net/20170503103613609)

 

 再试试在其他电脑上运行.exe，是不是已经可以正常运行了

 

 另外有人可能会有疑问：为什么在自己的电脑上并没有把platforms和imageformats这两个文件夹放到.exe同级目录下，直接运行.exe也没问题？

 

 这是因为在创建项目时已经通过配置Qt Project Settings为项目指明了搜索路径，所以在本机运行时不需要把这些Qt本身自带的dll扔到和.exe同级目录下。

 

 温馨提示：platforms和imageformats这两个文件夹一般在Qt的安装目录下直接搜索就可以找到，比如，我安装的是Qt5.6.2版本，安装在D盘，那么

 我电脑上的platforms和imageformats文件夹所在路径为：

 D:\Qt5.6.2\5.6\msvc2013\plugins\platforms

 D:\Qt5.6.2\5.6\msvc2013\plugins\platforms

 转载地址：[https://blog.csdn.net/u012043391/article/details/71107724](https://blog.csdn.net/u012043391/article/details/71107724)

   
 