---
title: update set命令用来修改表中的数据
date: 2016-08-01 16:30:00
tags: CSDN迁移
---
 版权声明：需要转载的请注明出处 https://blog.csdn.net/qq_22642239/article/details/52086818   
   **update set命令用来修改表中的数据。  
  
  
 update set命令格式：update 表名 set 字段=新值,… where 条件;  
  
  
 举例如下：  
 mysql> update MyClass set name='Mary' where id=1;  
  
  
 例子1，单表的MySQL UPDATE语句：  
 UPDATE [LOW_PRIORITY] [IGNORE] tbl_name SET col_name1=expr1 [, col_name2=expr2 ...] [WHERE where_definition] [ORDER BY ...] [LIMIT row_count];  
  
  
 例子2，多表的UPDATE语句：  
 UPDATE [LOW_PRIORITY] [IGNORE] table_references SET col_name1=expr1 [, col_name2=expr2 ...] [WHERE where_definition];  
  
  
 UPDATE语法可以用新值更新原有表行中的各列。SET子句指示要修改哪些列和要给予哪些值。WHERE子句指定应更新哪些行。如果没有WHERE子句，则更新所有的行。如果指定了ORDER BY子句，则按照被指定的顺序对行进行更新。LIMIT子句用于给定一个限值，限制可以被更新的行的数目。**  
   
 