---
title: C++指针delete后赋值为NULL的一些问题
date: 2019-06-13 16:06:06
tags: CSDN迁移
---
   **C++标准规定：delete空指针是合法的。**

 ** p=NULL后p指向0x0这个地址，NULL其实就是0x0，多次对0x0进行操作，系统默认合法。**

 对于非空指针delete后若未赋值为NULL，p将成为一个非法指针（野指针），后续代码如果使用到该指针有可能会造成系统崩溃（内存不可以读不可写），或者，破坏自身有效内存数据（释放后，又在申请作为别的用途，恰巧系统分配了同一块内存），再次delete系统会报异常甚至崩溃。

 所以delete指针后赋值为NULL或0是个好习惯。

 

 感谢原作者分享！

 [http://blog.sina.com.cn/s/blog_744b4e1e0102x5ed.html](http://blog.sina.com.cn/s/blog_744b4e1e0102x5ed.html)

   
 