---
title: win32汇编 invoke  和 call区别
date: 2016-05-08 09:56:29
tags: CSDN迁移
---
   **INVOKE 的语法如下：   
  
  
 INVOKE expression [，arguments]   
  
  
 expression 既可以是一个函数名也可以是一个函数指针。参数由逗号隔开。  
  
  
 INVOKE是编译器支持的伪指令,会检查参数.   
  
  
 CALL会直接去栈里取参. INVOKE最后也会变成 PUSH PUSH ... CALL 的形式  
  
  
  
  
  
  
  
  
  
  
 所以最好用invoke 调用函数，编译器会帮你检查参数是否传对。**  
   
 