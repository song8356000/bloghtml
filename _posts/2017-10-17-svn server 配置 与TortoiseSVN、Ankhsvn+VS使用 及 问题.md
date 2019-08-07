---
title: svn server 配置 与TortoiseSVN、Ankhsvn+VS使用 及 问题
date: 2018-05-07 17:10:46
tags: CSDN迁移
---
   ## Svn服务器与客户端安装

1. 下载安装VisualSvn-Server服务端。（过程略）

2. 下载安装TortoiseSVN客户端。（过程略）

3. 下载安装vs插件AnkhSvn。（过程略）


## []()在服务器中建立仓库

打开visualSVN ServerManager ，右击Repositories—新建—Repository，在弹出的对话框中输入仓库名（recharge）

![](https://img-my.csdn.net/uploads/201304/21/1366511283_4564.png)  


![](https://img-my.csdn.net/uploads/201304/21/1366511298_9244.png)  



## []()安全性设置

在左侧的Users上点击右键—新建—User,在弹出的对话框中添加用户名和密码：（注意用户名和密码区分大小写）

![](https://img-my.csdn.net/uploads/201304/21/1366511311_5763.png)

![](https://img-my.csdn.net/uploads/201304/21/1366511321_2533.png)  



## []()将源代码迁入到svn服务器中

找到你新建的项目文件夹（项目已经包含在里面），右击—TortoiseSVN—Import,在弹出的对话框中输入仓库所在的url，点击ok完成迁入源代码到svn服务器中。

![](https://img-my.csdn.net/uploads/201304/21/1366511332_2725.png)  


![](https://img-my.csdn.net/uploads/201304/21/1366511345_8196.png)  



## []()设置项目使用权限

在svn中权限分为三种noaccess（不可用）,read and write（既可读又可修改）,read only（只读）。

在需要添加权限的文件上右击—所有任务—ManageSecurity,在弹出的对话框中点击Add按钮，在弹出的对话框中选择添加的用户名点击OK按钮完成，在Security窗口中选择权限，点击确定按钮完成。

![](https://img-my.csdn.net/uploads/201304/21/1366511365_7186.png)  


![](https://img-my.csdn.net/uploads/201304/21/1366511372_1818.png)  


![](https://img-my.csdn.net/uploads/201304/21/1366511383_1684.png)  



## []()将源代码迁入VisualStudio2010中

打开Visual Studio2010，工具—选项—SourceControl—插件选择，选择Ankhsvn。

![](https://img-my.csdn.net/uploads/201304/21/1366511394_3644.png)  


点击文件—Subversion—openfromSubversion，在弹出的窗口中输入仓库的url后会弹出输入用户名和密码对话框，输入用户名和密码选择要迁入的代码文件将代码迁入到Visual studio中。在解决方案资源管理其中可以看到迁入的项目文件，如果项目文件前有问号跟感叹号则代表此文件当前用户无访问权限。

![](https://img-my.csdn.net/uploads/201304/21/1366511404_4235.png)  


![](https://img-my.csdn.net/uploads/201304/21/1366511414_4738.png)  


![](https://img-my.csdn.net/uploads/201304/21/1366511906_8185.png)  



## []()从服务器中更新项目为最新版本

右击解决方案资源管理器中的项目，选择update protect to latest version.

![](https://img-my.csdn.net/uploads/201304/21/1366511425_6849.png)  



## []()项目修改后提交到服务器中

项目修改后，项目文件前面的对勾显示为橘红色，在解决方案资源管理器中选择某一个项目文件或整个项目文件，右击—Commit完成提交。如果此文件对于当前用户是只读的，提交时系统会提示禁止修改等信息。

![](https://img-my.csdn.net/uploads/201304/21/1366512212_2539.png)  



## []()查看版本更新历史

右击——ViewHistory,在弹出的对话框中可以查看版本更新时间、作者、版本信息等。

   
 