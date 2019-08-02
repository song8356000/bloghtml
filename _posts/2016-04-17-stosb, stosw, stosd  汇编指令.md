---
title: stosb, stosw, stosd  汇编指令
date: 2016-05-19 10:01:17
tags: CSDN迁移
---
   ## stosb, stosw, stosd 汇编基础

 **我们来学习下另一组与字符串处理的指令。这组指令需要以指定的字符填充整个字符串或数组时比较有用。那么我们今天学习的这组指令就是stosb, stosw, stosd。这三个指令把al/ ax/ eax的内容存储到edi指向的内存单元中，同时edi的值根据方向标志的值增加或者减少。 同REP前缀联合使用的时候，这组指令需要填充整个字符串或数组时候比较有用。例如我们的MS提供的RtlZeroMemory函数，用的就是这三组指令来填充的。。  
  
  
 举个例子：假设此时我声明了一个变量   
  
 szBuffer db 'hello world', 0   
  
 我想通过一个循环将其hello world的这个字符串填充为0.   
  
 那么此刻我们的代码该怎么写，大家先思考下。   
  
  
 呵呵，那我就来给大家演示下：   
  
 mov edi, szBuffer   
 xor eax, eax   
 mov ecx, 11   
 cld   
 rep stosb   
  
 此刻执行完以上指令后，就会将我们szBuffer标号处(数据偏移)的内存单元用al来进行填充掉。   
  
 上面呢就是简单的一个用0填充的过程，当然上面这个我们是用字节来填充的。而且没有加判断，那么下面我给大家一个之前写的一个ZeroMemoy过程，这个做了很多判断以及优化了下，给大家学习下。比微软提供的库函数执行效率要高一些。  
  
 proc ZeroMemory lpBuffer:DWORD, BufferSize:DWORD   
 push edi   
 mov edi, [lpBuffer]   
 mov ecx, [BufferSize]   
 xor eax, eax   
 mov edx, ecx   
 cld   
 and edx, 3   
 shr ecx, 4   
 rep stosd   
 mov ecx, edx   
 rep stosb   
 pop edi   
 ret   
 endp **  


   
   
 