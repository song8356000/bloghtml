---
title: win10 使用cmake编译opencv源码生成VS工程（二）
date: 2017-07-06 15:34:52
tags: CSDN迁移
---
 版权声明：需要转载的请注明出处 https://blog.csdn.net/qq_22642239/article/details/74548658   
   本文章是对 [](http://blog.csdn.net/qq_22642239/article/details/74331387)[win10X64 + vs2015通过Cmake编译Opencv（一）](http://blog.csdn.net/qq_22642239/article/details/74331387) 这篇博文的补充说明，具体是一些细节方面的详细描述，可以更好的理解 使用cmake 编译opencv源码 生成对应VS版本的工程。其实opencv的每个版本都可以生成相应的VS版本的工程（release和debug x86或者64，看具体自己怎么配置），比如说 现在的opencv3.2.0版本 可以生成VS2015的工程 也可以生成 vs2013的工程，等等，本次测试就是在 win10 环境下 使用cmake3.9.0 配置 opencv3.2.0版本生成vs2015工程。

 下面补充几点：

 一. 编译opencv源码 所需文件下载地址

 通过[win10X64 + vs2015通过Cmake编译Opencv（一）](http://blog.csdn.net/qq_22642239/article/details/74331387) 的介绍 ，大部分问题已经可以解决了，基本的就不多说了，在这里需要补充的就是，在使用cmake自动下载F:\opencv\opencv-3.2-source code\opencv\sources\3rdparty\ffmpeg目录下的

 

 ![](https://img-blog.csdn.net/20170706151222589?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjI2NDIyMzk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 和 F:\opencv\opencv-3.2-source code\opencv\sources\3rdparty\ippicv\downloads\windows-04e81ce5d0e329c3fbc606ae32cad44d 目录下的

 

 ![](https://img-blog.csdn.net/20170706151226773?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjI2NDIyMzk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 这四个文件的时候有时候 会出现超时的情况，因为这是从国外下载的，在国内下载 你懂得，在[篇（一）](http://blog.csdn.net/qq_22642239/article/details/74331387)中 介绍的是直接从相应地址下载，这个是可行的，但是作者没有提供下载地址，这里提供一下github下载地址 

 opencv 源码github下载地址：[https://github.com/opencv/opencv/releases/tag/3.2.0](https://github.com/opencv/opencv/releases)

 opencv 编译所需资源github下载地址：[https://github.com/opencv/opencv_3rdparty/branches/all](https://github.com/opencv/opencv_3rdparty/branches/all)

 

 地址已经有了 ，我们可以从中下载相应的文件（文件不对应是不会编译通过的，这个要选对）

 

 二. 对比源码中的cmake文件 中的的MD5值 确定下载文件是否与 相应opencv版本对应

 

 怎么对比所下载文件与编译所需的文件一样呢？

 打开 opencv源代码目录，如下图：

 ![](https://img-blog.csdn.net/20170706152502337?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjI2NDIyMzk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 ![](https://img-blog.csdn.net/20170706152521653?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjI2NDIyMzk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 

 使用记事本或者UE打开ffmpeg.cmake文件和 downloader.cmake文件，如下图所示：

 ![](https://img-blog.csdn.net/20170706152854930?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjI2NDIyMzk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 

 ![](https://img-blog.csdn.net/20170706152858417?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjI2NDIyMzk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 

 

 在以文本打开的那两个文件中发现 需要下载的那四个文件的 MD5值（具体格式不多说了），有了MD5值，我么可以先从上面提供的github上面下载相应的文件，检测下载到的文件的MD5值，与 上面的正确的MD5值对比，一样的话，放到相应的目录下，然后cmake重新configure OK 各项工程都可以编译了， 后续的就不啰嗦了，都懂。

   
 