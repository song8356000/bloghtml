---
title: MySQL错误号码1862：your password has expired
date: 2018-03-13 10:20:17
tags: CSDN迁移
---
 版权声明：需要转载的请注明出处 https://blog.csdn.net/qq_22642239/article/details/79536748   
   **问题：**

 今天刚安装好 mysql5.7.16后，安装过程如下面链接[http://blog.csdn.net/qq_22642239/article/details/61919274](http://blog.csdn.net/qq_22642239/article/details/61919274) 安装完成后，我使用命令行可以登录，但是使用 navicat登录则出现如标题所示的 密码过期的错误。

 

 **解决方案：**

 

 

 mysql> set password =password("123456");

 Query OK, 0 rows affected, 1 warning (0.01 sec)  


 通过命令修改密码，然后重新使用navicat登录，成功，问题解决。

   
 