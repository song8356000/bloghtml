---
title: Linux之centos包管理rpm、yum、tar
date: 2019-04-15 18:01:31
tags: CSDN迁移
---
   **rpm包是二进制格式，无需编译安装便可使用，tar包是源码格式，需要编译安装才可使用**

 
### **rpm包管理：**

 rpm:redhat package manager，红帽的包管理器，其主要的操作参数有如下：

 -i,安装

 -v,显示安装过程的信息

 -h,显示安装的进度

 -e,删除rpm包，不会删除其依赖关系

 --nodeps,前置安装，忽略安装包的依赖关系

 --test,测试安装包的依赖性

 -qa,显示所有安装的rpm包的软件

 -ql,列出该软件的相关文件目录

 
### **yum，基于rpm包管理，能够从指定服务器中自动下载rpm包并安装，可以自动处理包的依赖关系**

 yum provides */命令,知道命令不知道其命令的安装包，查询该安装包

 yum -y install 包,自动应答yes并且安装该包

 yum remove 包,删除该安装包，并且自动删除其依赖关系

 
### **tar ，相关命令如下：**

 -c,打包

 -x,解包

 -j,tar.bz2,压缩比高

 -z,tar.gz

 -v,显示过程的文件名

 -f,后接要处理的文件或目录名

 -C,指定解压目录

 --exclude,排除打包目录下的哪些目录或者文件不放入打包

 注意：一般用 tar进行目录的打包

 本文转载自：[https://www.cnblogs.com/pangzhi/p/5538669.html](https://www.cnblogs.com/pangzhi/p/5538669.html)

   
 