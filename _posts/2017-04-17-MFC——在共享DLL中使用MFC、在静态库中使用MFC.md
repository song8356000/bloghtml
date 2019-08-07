---
title: MFC——在共享DLL中使用MFC、在静态库中使用MFC
date: 2018-10-30 15:13:57
tags: CSDN迁移
---
   前言  
 我们在使用Microsoft Visual Studio软件（也就是我们常常说的VS）时，其中项目属性中有一项叫做“MFC的使用”，里面包含有不同的设置，会影响我们所编写的程序的使用，今天我就遇到了这个情况，我们一起来总结一下，避免犯下相同的错误。

 内容  
 昨天写了一个小程序，使用的是MFC应用程序的工程，工具的版本为VS2013，工程参数都是默认的，其中有一项设置叫做“MFC的使用”，默认设置是“在共享DLL中使用MFC”，我虽然看到了但是没有放在心上，编码期间解决了一个MFC使用多字节字符集报错的问题，此处不展开，后续再表，就这样程序编写完成，非常完美！交给了一个叫做“小闪电”的用户，然后她怀着万分激动地心情拿到公司电脑一使用，结果报错了！！！

 ![](https://img-blog.csdnimg.cn/20181030145225416.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIyNjQyMjM5,size_16,color_FFFFFF,t_70)

 “无法启动此程序，因为计算机中丢失mfc120.dll。尝试重新安装改程序以解决此问题。”，看到这个问题我首先想到的就是动态链接库的事情，因为我使用的是VS2013正好使用的是12.0版本的库文件，所以说应该是目标机上没有这个库，当然可以手动下载这个库文件，添加至“C:\Windows\System32”目录，但是在程序开发工程中，你无法要求所有的用户都会这样做，只能从自身找解决方法了。

 当然我也仔细查了一下这个叫做“MFC的使用”的参数，它其实包括3个选项，具体如截图：

 ![](https://img-blog.csdnimg.cn/20181030145254943.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIyNjQyMjM5,size_16,color_FFFFFF,t_70)

 使用标准Windows库   
 在共享DLL中使用MFC   
 在静态库中使用MFC  
 这三种当然有着不同的意义：

 第一种顾名思义，是使用WINDOWS SDK API库，不使用MFC类。话说一个MFC应用程序不使用MFC类是个什么情况，一开始我也想不通，后来我新建了一个MFC应用程序的工程，然后把这这项参数填成这一种，然后程序编译失败，具体错误如下图，这就说明问题了，如果是MFC工程必须选择第二项或者第三项，而第一项“使用标准Windows库”是为非MFC工程准备的（不知理解是否正确，请大神指教）。

 ![](https://img-blog.csdnimg.cn/20181030145340956.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIyNjQyMjM5,size_16,color_FFFFFF,t_70)  
 第二种指的是打包时一些MFC的DLL的内容没有被包含在EXE文件中，所以EXE文件较小，但是运行时要求系统中要有相关的DLL文件。这就是我的程序一开始选择的选项，要求目标计算机中至少要包含“mfc120.dll”库文件，否则无法使用。

 第三种是将DLL中的相关代码写进EXE文件中，文件较大，但是可以在没有相关DLL的机器上运行。个人感觉程序测试期间使用这个选项应该比较好，起码可以保证程序的正常运行。

 总结  
 MFC应用程序的工程，关于“MFC的使用”属性，应该选择“在共享DLL中使用MFC”或者“在静态库中使用MFC”。  
 “使用标准Windows库” 选项只能用在非MFC工程中，如果用在MFC工程会导致代码编译报错。  
 “在共享DLL中使用MFC” 选项生成的程序可执行文件比较小，但是要求目标机器必须装有必要的库文件。  
 “在静态库中使用MFC” 选项生成的程序可执行文件几乎所有的Windows都可以执行，但是程序较大一些，其中包含必要的库文件，可以基本保证在别的机器上正常运行。  
 ---------------------   
 作者：AlbertS   
 来源：CSDN   
 原文：https://blog.csdn.net/albertsh/article/details/52838419   
 版权声明：本文为博主原创文章，转载请附上博文链接！

   
 