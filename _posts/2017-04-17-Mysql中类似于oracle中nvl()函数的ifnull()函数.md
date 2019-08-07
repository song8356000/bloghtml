---
title: Mysql中类似于oracle中nvl()函数的ifnull()函数
date: 2016-10-12 15:03:55
tags: CSDN迁移
---
   **IFNULL(expr1,expr2)   
 如果expr1不是NULL，IFNULL()返回expr1，否则它返回expr2。IFNULL()返回一个数字或字符串值，取决于它被使用的上下文环境。   
 mysql> select IFNULL(1,0);  
 -> 1  
 mysql> select IFNULL(0,10);  
 -> 0  
 mysql> select IFNULL(1/0,10);  
 -> 10  
 mysql> select IFNULL(1/0,'yes');  
 -> 'yes'  
   
 IF(expr1,expr2,expr3)   
 如果expr1是TRUE(expr1<>0且expr1<>NULL)，那么IF()返回expr2，否则它返回expr3。IF()返回一个数字或字符串值，取决于它被使用的上下文。   
 mysql> select IF(1>2,2,3);  
 -> 3  
 mysql> select IF(1<2,'yes','no');  
 -> 'yes'  
 mysql> select IF(strcmp('test','test1'),'yes','no');  
 -> 'no'  
  
  
 expr1作为整数值被计算，它意味着如果你正在测试浮点或字符串值，你应该使用一个比较操作来做。   
  
  
 mysql> select IF(0.1,1,0);  
 -> 0  
 mysql> select IF(0.1<>0,1,0);  
 -> 1  
  
  
 在上面的第一种情况中，IF(0.1)返回0，因为0.1被变换到整数值, 导致测试IF(0)。这可能不是你期望的。在第二种情况中，比较测试原来的浮点值看它是否是非零，比较的结果被用作一个整数。   
  
  
 CASE value WHEN [compare-value] THEN result [WHEN [compare-value] THEN result ...] [ELSE result] END   
   
 CASE WHEN [condition] THEN result [WHEN [condition] THEN result ...] [ELSE result] END   
 第一个版本返回result，其中value=compare-value。第二个版本中如果第一个条件为真，返回result。如果没有匹配的result值，那么结果在ELSE后的result被返回。如果没有ELSE部分，那么NULL被返回。   
 mysql> SELECT CASE 1 WHEN 1 THEN "one" WHEN 2 THEN "two" ELSE "more" END;  
 -> "one"  
 mysql> SELECT CASE WHEN 1>0 THEN "true" ELSE "false" END;  
 -> "true"  
 mysql> SELECT CASE BINARY "B" when "a" then 1 when "b" then 2 END;  
 -> NULL  
  
  
  
  
 原文出自【比特网】，转载请保留原文链接：http://server.chinabyte.com/21/2648521.shtml**   
 