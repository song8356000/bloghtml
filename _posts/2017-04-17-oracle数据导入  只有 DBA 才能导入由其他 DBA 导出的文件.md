---
title: oracle数据导入  只有 DBA 才能导入由其他 DBA 导出的文件
date: 2016-09-14 16:30:56
tags: CSDN迁移
---
 版权声明：需要转载的请注明出处 https://blog.csdn.net/qq_22642239/article/details/52539221   
   出现这个问题应该是 当前用户的权限不够，

 

 在 system 或者sys 用户下 输入：

 

 grant dba to 用户名；

 

 授予权限即可

 

   
 