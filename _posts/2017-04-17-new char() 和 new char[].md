---
title: new char() 和 new char[]
date: 2018-12-24 11:28:27
tags: CSDN迁移
---
   char *l[pc](https://www.baidu.com/s?wd=pc&amp;tn=SE_PcZhidaonwhc_ngpagmjz&amp;rsv_dl=gh_pc_zhidao) = new char('a'); //开辟一个内存单元，并用括号里的初始化  
 char *L[pc](https://www.baidu.com/s?wd=pc&amp;tn=SE_PcZhidaonwhc_ngpagmjz&amp;rsv_dl=gh_pc_zhidao)c = new char[15]; //开辟一个数组

 

 示例：

 new char(10) 这个用10来初始化你定义的指针所指向的那个char

 new char[10] 定义了一个有10个char元素的数组  
  
 释放内存的方法也不一样:  
 delete l[pc](https://www.baidu.com/s?wd=pc&amp;tn=SE_PcZhidaonwhc_ngpagmjz&amp;rsv_dl=gh_pc_zhidao);  
 delete [] lpcc;

   
 