---
title: 使用navicat无法登陆oracle 数据库
date: 2016-09-09 15:49:04
tags: CSDN迁移
---
 版权声明：需要转载的请注明出处 https://blog.csdn.net/qq_22642239/article/details/52487683   
   在使用navicat 创建连接登录数据库时，出现错误cannot create oci environment 我安装的是 oracle11g

 下面是我的解决办法：

 首先打开 navicat premium 工具-》选项 如下图所示：

 ![](https://img-blog.csdn.net/20160909153448358?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 ![](https://img-blog.csdn.net/20160909153639923?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 在这里 我们可以点击OCI 会出现上图所示的界面

 在Oci library 这里面我们应该自己将合适的oci.dll 文件 的路径添加进来，当时我遇见这个问题的时候 我在我电脑上搜索了oci.dll文件 发现有好多，我也不知道该选哪一个，后来我发现，这个oci.dll 文件应该选择位于当时安装oracle数据库时的安装目录下的oci.dll，例如我的客户端的安装目录是D:\app\admin\product\instantclient_10_2，那么在这下面会有一个oci.dll文件，那么把这个oci.dll文件的路径写入刚才那个oci library 的地方就可以了，然后重新启动 navicat premium 即可。

   
 