---
title: 如何在linux系统下读写windows上的共享文件夹
date: 2019-02-21 18:42:55
tags: CSDN迁移
---
 版权声明：需要转载的请注明出处 https://blog.csdn.net/qq_22642239/article/details/87866424   
   首先在windows上建立共享文件夹比如我这里是共享的是vs2017文件夹

 如图：

 

 ![](https://img-blog.csdnimg.cn/20190221183608669.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIyNjQyMjM5,size_16,color_FFFFFF,t_70)

 ![](https://img-blog.csdnimg.cn/20190221183624594.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIyNjQyMjM5,size_16,color_FFFFFF,t_70)

 在这里可以添加多个共享的文件夹。

 

 然后进入虚拟机linux系统：

 在root模式下

 终端中输入

 mount -t vmhgfs .host:/ /mnt/hgfs

 如果做了以上两步，系统没有提示任何问题，那么恭喜你，设置完毕。

 [root@ /]# cd /mnt/hgfs

 就可以看到共享文件夹了，在界面模式下可以直接进入/mnt/hgfs 下进行查看，可以同步windows文件夹，这样传输文件就方便多了。

 

 

 

 如果出现问题 可以参考如下解决方法：转载自：[https://www.cnblogs.com/vincentfu/p/5402666.html](https://www.cnblogs.com/vincentfu/p/5402666.html)

 2，有问题，安装vmware-tools

 我们得重新搭建这个环境了，以下是Centos6.7在VMware7.0上设置hgfs文件共享的过程

 2.1 选中要安装VMware tools的linux虚拟机，然后点击VMware Workstation顶部的“虚拟机”按钮，选中“安装VMware tools”按钮，此时VMware tools的安装包就会默认放在/dev/cdrom目录中。

 2.2 进入该Linux系统中的命令行，建立一个挂载cdrom的目录，如：

 [root@ /]# mkdir /mnt/cdrom

 2.3 将/dev/cdrom目录挂载到刚建立的/mnt/cdrom目录，这样在/mnt/cdrom目录就能看见VMware tools的安装包，但是这个安装包是只读的，

 必须拷贝到用户自己的目录中才可以正常使用。

 [root@ /]# mount /dev/cdrom /mnt/cdrom

 2.4 拷贝/mnt/cdrom目录下的VMware tools的安装包到用户自己的目录，比如/home/vincent目录

 [root@ /]# cd /mnt/cdrom/  
 [root@ cdrom]# cp VMwareTools-8.1.3-203739.tar.gz /home/vincent

 2.5 解压该工具包文件

 [root@master cdrom]# cd /home/vincent

 [root@localhost vincent]# tar -zxvf VMwareTools-8.1.3-203739.tar.gz

 2.6 解压后在该目录下会出现一个vmware-tools-distrib文件夹，进入该文件夹，执行vmware-install.pl命令

 [root@master ~]# cd vmware-tools-distrib/

 [root@master vmware-tools-distrib]# ./vmware-install.pl

 2.7 然后改软件就会自动安装，当然安装的过程会询问安装目录、安装模块等，如果你想自己设定，可以根据提示设置，否则，可以全部按回车键。

 若是可以顺利完成以上步骤，不提示错误，然后请按照文章最开始的介绍设置即可。

 但是，出现以下字样，那么还得再进行一步工作。 

 
```
 Searching for a valid kernel header path...
```
 
```
 The path "" is not valid.
```
 
```
 Would you like to change it? [yes]

3，安装kernel-headers, kernel-devel(ps:因为找不到内核头文件路径)
```
 3.1 [root@ /]# yum update kernel -y  
 3.2 [root@ /]# yum install kernel-headers(kernel-devel/gcc make)   
 3.3 [root@ /]# init 6  
 3.4 [root@ /]# uname -r  
 3.5 [root@ /]# rpm -qa|grep –e kernel-headers –e kernel-devel

 
```
 说明：如果是ubuntu系统，有网友总结如下
```
 Ubutu系统问题  
 1. 更新或安装linux headers  
 sudo apt-get update && sudo apt-get install build-essential linux-headers-$(uname -r)

 2. 关联文件，就是因为找不到这个几个文件，vmware tools才认为路径无效的。  
 cd /lib/modules/$(uname -r)/build/include/linux  
 sudo ln -s ../generated/utsrelease.h  
 sudo ln -s ../generated/autoconf.h  
 sudo ln -s ../generated/uapi/linux/version.h

   
 