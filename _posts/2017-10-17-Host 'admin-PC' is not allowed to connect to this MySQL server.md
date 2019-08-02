---
title: Host 'admin-PC' is not allowed to connect to this MySQL server
date: 2016-08-04 18:46:27
tags: CSDN迁移
---
   http://blog.csdn.net/liuxiyangyang/article/details/8951262

 

 问题："Host 'admin-PC' is not allowed to connect to this MySQLserver" （其中，admin-PC为我的机器名）

 原因：安装MySQL时没有勾选“Enable root access from remote machines”

 解决办法：执行命令

 GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'mypassword';  


 flush privileges;

   
 