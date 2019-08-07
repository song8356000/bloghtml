---
title: power designer 连接数据库以及    Could not Initialize JavaVM!  错误的解决
date: 2016-09-13 17:38:14
tags: CSDN迁移
---
 版权声明：需要转载的请注明出处 https://blog.csdn.net/qq_22642239/article/details/52526906   
   最近使用 power designer 时遇见了好多问题，下面分两个方面说明。

 一 . 首先使用power designer 怎么连接数据库的表等数据：

 这里面我分oracle和mysql两种数据库来说：

 1.oracle数据库

 首先打开power designer 依次点击 文件--->Reverse Engineer --->database... 如下图所示：![](https://img-blog.csdn.net/20160913165348096?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

 

 

 然后弹出以下界面：

 

 ![](https://img-blog.csdn.net/20160913165547316?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 

 在Model name 那里面 填写的是建立的物理模型的名称（这个可以自己建立，不在这里说明），DBMS这一个下拉列表里 我们可以选择我们当前电脑装的是什么版本的oracle 数据库，比如这里我装的是oracle 11g 直接选择点击确定即可。

 

 ![](https://img-blog.csdn.net/20160913170103041?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 

 在这里 我们应该直接点击 Using a data source 下面 那一行 右边的一个小圆柱体，弹出如下界面

 ![](https://img-blog.csdn.net/20160913170124695?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 

 从上面这张图中 我们可以看出在 Data source 下面有三种选择前两种是使用ODBC连接，我们卡一点击Configure... 进入配置界面，如下图所示第三种方式：

 

 ![](https://img-blog.csdn.net/20160913170548009?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 上图中我们可以点击第二个小图标（一个小圆柱体）添加数据源，如下图格式所示

 ![](https://img-blog.csdn.net/20160913170940693?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 上面这个 是根据自己数据库的信息填写的，填写完后可以测试一下连接是否成功（Test Connection...）

 

 如果出现 Could not Initialize JavaVM! 则说明JAVA环境配置错误，首先power designer需要在32位jdk环境下运行，可以先下载32位的jdk（如果用odbc连接那么另当别论）,

 然后设置环境变量 ，在这里我是建立了一个批处理文件，直接设置环境变量

 set JAVA_HOME=C:\Program Files (x86)\Java\jdk1.8.0_25  
  
  
 set CLASSPATH=%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar  
  
  
 set path=C:\Program Files (x86)\Java\jdk1.8.0_25\bin  


 除了 上面这几个还需要在 工具--->常规选项--->varibles 里面设置一下四个选项（JAR,JAVA,JAVAC,JAVADOC）的路径如下图所示

 

 ![](https://img-blog.csdn.net/20160913171909331?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

 他们的路径就是安装的32位的jdk里面的bin目录下的 对应名称的可执行文件

 这样设置过后应该就可以了连接成功了，剩下的，直接点击确定，默认操作就可以了。

 

 二. 下面介绍一下怎么连接到mysql数据库

 在上面的截图中我们看见可以使用ODBC连接，使用mysql 我们使用第一种方式连接如图：

 

 ![](https://img-blog.csdn.net/20160913170124695?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 

 使用ODBC首先需要自己配置数据源,关于配置数据源，首先我们可以这样如下图：

 ![](https://img-blog.csdn.net/20160913172751030?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 

 点击Microsoft ODBC 管理员弹出如下窗口：

 ![](https://img-blog.csdn.net/20160913172925704?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 

 点击 添加 如下图所示：

 ![](https://img-blog.csdn.net/20160913173111239?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 

 如上图所示 根据自己的mysql安装版本选择，以及字符集，我的选择如上，点击完成，弹出如下窗口：

 ![](https://img-blog.csdn.net/20160913173309443?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 根据自己的mysql 数据库信息填写，可以测试一下连接是否成功，成功即可，然后以下步骤确定即可， 

 

 指定的数据库中的数据会被导入到相应的对象空间中，显示出来，如下图是显示的数据库中表的信息：

 ![](https://img-blog.csdn.net/20160913173645851?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 

 好了 ，到此结束。

   
 