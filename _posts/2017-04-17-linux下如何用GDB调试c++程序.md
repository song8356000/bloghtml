---
title: linux下如何用GDB调试c++程序
date: 2019-02-22 13:56:00
tags: CSDN迁移
---
   [GDB](https://www.baidu.com/s?wd=GDB&amp;tn=24004469_oem_dg&amp;rsv_dl=gh_pl_sl_csd) 是GNU开源组织发布的一个强大的UNIX下的程序调试工具。或许，各位比较喜欢那种图形界面方式的，像VC、BCB等IDE的调试，但如果你是在 UNIX平台下做软件，你会发现GDB这个调试工具有比VC、BCB的图形化调试器更强大的功能。一般来说，GDB主要帮忙你完成下面四个方面的功能：

 
  1. 启动你的程序，可以按照你的自定义的要求随心所欲的运行程序。 
  3. 可让被调试的程序在你所指定的调置的断点处停住。（断点可以是条件表达式） 
  5. 当程序被停住时，可以检查此时你的程序中所发生的事。 
  7. 动态的改变你程序的执行环境。 从上面看来，GDB和一般的调试工具没有什么两样，基本上也是完成这些功能，不过在细节上，你会发现GDB这个调试工具的强大，大家可能比较习惯了图形化的调试工具，但有时候，命令行的调试工具却有着图形化工具所不能完成的功能。让我们一一看来。

 

 gdb基本命令列表： 

 ![](http://hi.csdn.net/attachment/201203/19/0_1332136060HnWO.gif)

 

 实例:

 1 新建一个源文件vi swap.cc

 ![](http://hi.csdn.net/attachment/201203/19/0_13321312233vfG.gif)

 源文件内容如下：

 #include<iostream>  
 using namespace std;  
 void swap(int &a,int &b)  
 {  
 int tmp;  
 tmp=a;  
 a=b;  
 b=tmp;  
 }

 int main()  
 {  
 int i,j;  
 cout<<endl<<"Input two int number:"<<endl;  
 cin>>i>>j;  
 cout<<"Before swap(),i="<<i<<" j="<<j<<endl;  
 swap(i,j);  
 cout<<"After swap(),i="<<i<<" j="<<j<<endl<<endl;  
 return 0;  
 }

 直接复制粘贴生成源文件

 2.生成可执行文件 g++ -g -o swap swap.cc，注意必须使用-g参数，编译会加入调试信息，否则无法调试执行文件

 ![](http://hi.csdn.net/attachment/201203/19/0_1332131674N111.gif)

 3.启动调试 gdb swap

 ![](http://hi.csdn.net/attachment/201203/19/0_1332131779zH88.gif)

 3.1 查看源文件 list 1，回车重复上一次指令

 ![](http://hi.csdn.net/attachment/201203/19/0_1332132852AmV1.gif)

 3.2设置调试断点 break 16，在第16行设置断点，info break查看断点信息（亦可使用缩写i b ）

 ![](http://hi.csdn.net/attachment/201203/19/0_1332133229I2aK.gif)

 ![](http://hi.csdn.net/attachment/201203/19/0_1332133538hH00.gif)

 3.3 调试 运行 输入run 或者r 

 ![](http://hi.csdn.net/attachment/201203/19/0_13321337928E0I.gif)

 3.3 单步调试，step 或者 s进入函数内部

 ![](http://hi.csdn.net/attachment/201203/19/0_1332133876DrOK.gif)

 3.4查看变量 print b 或者 p b

 ![](http://hi.csdn.net/attachment/201203/19/0_13321339794UxL.gif)

 3.5查看函数堆栈bt，退出函数finish

 ![](http://hi.csdn.net/attachment/201203/19/0_1332134294MXJX.gif)

 3.6 继续运行直到下一个断点或主函数结束continue或者c

 ![](http://hi.csdn.net/attachment/201203/19/0_13321344199VGT.gif)

 3.7 退出调试 输入q

 

 ![](http://hi.csdn.net/attachment/201203/19/0_1332134872aIf8.gif)

 

 文章转载自：[https://blog.csdn.net/stone_overlooking/article/details/78493331](https://blog.csdn.net/stone_overlooking/article/details/78493331)

   
 