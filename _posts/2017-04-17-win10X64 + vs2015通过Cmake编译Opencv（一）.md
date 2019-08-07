---
title: win10X64 + vs2015通过Cmake编译Opencv（一）
date: 2017-07-04 15:25:49
tags: CSDN迁移
---
   # 写在前面的话：

 为什么需要使用Cmake编译安装？在我目前的印象中主要有两个原因：一是能够看[OpenCV](http://lib.csdn.net/base/opencv)的源代码；而是能够生成能在没有[opencv](http://lib.csdn.net/base/opencv)环境的电脑系统下运行的可执行文件。同时建议大家先看看第七点，也就是我在编译安装过程中遇到过什么错误，是什么原因造成的之后再开始编译安装，这样能避免重复我的错误

 
# []()1.安装vs2015

 我的VS2015是中文专业版，修改了路径，直接默认安装；之后在网上找了一个秘钥解除30天的试用期的限制。

 
# []()2.解压OpenCV3.1.0

 直接运行.exe文件即可，将解压的文件夹放在自己想要的目录中。解压结果是一个名为opencv的文件夹，内容如下：   
![opencv](https://img-blog.csdn.net/20161111143646857)

 
# []()3.安装Cmake

 到[这里](https://cmake.org/download/)下载Cmake文件，下载cmake-3.6.1-win64-x64.msi安装（版本不对会出错，详情见后文，这里截图都是用我最开始错误版本的截图）。

 
# []()4.Cmake编译Opencv

 运行cmake软件，制定source code路径为OpenCV解压所得的sources文件夹路径，在选定生成路径，如下图：

 ![cbuild](https://img-blog.csdn.net/20161111160335423)

 点击configure，选定编译器为Visual Studio 14 2015，点击finish。

 如果没有错误的话，会出现configure done，忘了截图   
 手动勾选两项内容：   
 BUILD_EXAMPLES   
 BUILD_opencv_world

 再次点击Configure，又会出现configure done，结果如下：

 ![configure done](https://img-blog.csdn.net/20161111203337511)

 再点击generate，生产sln工程

 ![generate](https://img-blog.csdn.net/20161111203642559)

 
# []()5.打开OpenCV.sln工程

 打开生产的sln工程，如下图：

 ![sln](https://img-blog.csdn.net/20161111204524328)

 点击生产->批生成，选择如下：

 注意：不要勾选ALL_BUILD对应的两项，这是我首次尝试的时候勾选的，失败了。成功的那次没有勾选   
![生成](https://img-blog.csdn.net/20161111204918048)

 结果如下：

 ![生成结果](https://img-blog.csdn.net/20161112165503737)

 
# []()6.配置

 1.设置环境变量   
 右击 我的电脑->属性->高级系统设置->环境变量->系统变量->编辑Path->新建， 添加路径：D:\OpenCV3.1.0\opencv\cbuild\install\x64\vc14\bin

 ![右键](https://img-blog.csdn.net/20161112172917387)

 ![高级系统设置](https://img-blog.csdn.net/20161112173038185)   
![环境变量](https://img-blog.csdn.net/20161112173054966)   
![系统变量](https://img-blog.csdn.net/20161112173108326)   
![新建](https://img-blog.csdn.net/20161112173124644)

 2.配置vs2015   
 新建一个控制台应用程序，勾选空项目。（用vs新建工程默认不截图了。。。）

 在 属性管理器->Debug | x64->Microsoft.Cpp.x64.user 上 右键->属性 打开属性页

 ![这里写图片描述](https://img-blog.csdn.net/20161112181748658)  
![这里写图片描述](https://img-blog.csdn.net/20161112181907393)

 C/C++–> 常规 –> 附加包目录，添加

 ![这里写图片描述](https://img-blog.csdn.net/20161112180315715)

 链接器—>附加库目录

 ![这里写图片描述](https://img-blog.csdn.net/20161112180456423)

 链接器—>输入   
 注意带d和不带d的顺序，应该会有影响   
![这里写图片描述](https://img-blog.csdn.net/20161112180710348)

 
# []()7.测试

 在该工程的cpp文件中添加如下代码

 
```
#include <opencv2\opencv.hpp>

using namespace cv;

int main(int argc, char** argv)
{
    Mat img = imread("test.jpg");

    imshow("img", img);
    waitKey(0);

    return 0;
}
```

  * 1
  * 2
  * 3
  * 4
  * 5
  * 6
  * 7
  * 8
  * 9
  * 10
  * 11
  * 12
  * 13
  * 1
  * 2
  * 3
  * 4
  * 5
  * 6
  * 7
  * 8
  * 9
  * 10
  * 11
  * 12
  * 13相应路径下有名为test.jpg的图片

 ![这里写图片描述](https://img-blog.csdn.net/20161112182039930)

 运行（有可能需要重启）

 结果如下：   
![这里写图片描述](https://img-blog.csdn.net/20161112181025181)

 
# []()8.错误总结

 
## []()错误一：vs2015模块不全

 如果vs安装有问题，就会出现如下错误：   
 Error in configuration process， project files may be invalid.   
 No CMAKE_CXX_COMPILER could be found.

 ![error1](https://img-blog.csdn.net/20161111160611221)   
![error](https://img-blog.csdn.net/20161111160629424)

 错误提示为没有找到相应的编译器，这是由于vs2015相应模块没有安装的问题，点击新建项目，在已安装模块的Visual C++下吧相应的模块安装完毕就能解决这个错误

 ![模块](https://img-blog.csdn.net/20161111192814935)

 
## []()错误二：缺少一些文件

 再次出现如下错误：

 ![ffpemg](https://img-blog.csdn.net/20161111193124452)

 这是由于cmake时需要下载opencv_ffmpeg_64.dll，opencv_ffmpeg.dll以及ippicv_windows_20151201.zip，但是自动下载会出错。所以需要先下好，放到相应的位置。文件下载在[这里](http://download.csdn.net/detail/u013832707/9680596)，里面有使用方法

 
## []()错误三：camke版本问题

 我最开始下载的目前最新的版本cmake-3.7.0-rc3-win64-x64.msi。Cmake能够通过，但是打开Opencv.sln之后批生成是无法成功的，错误如下

 ![这里写图片描述](https://img-blog.csdn.net/20161112181145952)  
![这里写图片描述](https://img-blog.csdn.net/20161112181201921)

   
 