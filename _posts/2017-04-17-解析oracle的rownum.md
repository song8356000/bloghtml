---
title: 解析oracle的rownum
date: 2016-10-17 17:07:11
tags: CSDN迁移
---
   # 

 本人最近在使用oracle的rownum实现分页显示的时候，对rownum做了进一步的分析和研究。现归纳如下，希望能给大家带来收获。  
 对于rownum来说它是oracle系统顺序分配为从查询返回的行的编号，返回的第一行分配的是1，第二行是2，依此类推，这个伪字段可以用于限制查询返回的总行数，而且rownum不能以任何表的名称作为前缀。  
 举例说明：  
 例如表：student(学生)表，表结构为：  
 ID char(6) --学号  
 name VARCHAR2(10) --姓名   
 create table student (ID char(6), name VARCHAR2(100));  
 insert into sale values('200001',‘张一’);  
 insert into sale values('200002',‘王二’);  
 insert into sale values('200003',‘李三’);  
 insert into sale values('200004',‘赵四’);  
 commit;  
 (1) rownum 对于等于某值的查询条件  
 如果希望找到学生表中第一条学生的信息，可以使用rownum=1作为条件。但是想找到学生表中第二条学生的信息，使用rownum=2结果查不到数据。因为rownum都是从1开始，但是1以上的自然数在rownum做等于判断是时认为都是false条件，所以无法查到rownum = n（n>1的自然数）。  
 SQL> select rownum,id,name from student where rownum=1;（可以用在限制返回记录条数的地方，保证不出错，如：隐式游标）  
 SQL> select rownum,id,name from student where rownum=1;  
 ROWNUM ID NAME  
 ---------- ------ ---------------------------------------------------  
 1 200001 张一  
 SQL> select rownum,id,name from student where rownum =2;   
 ROWNUM ID NAME  
 ---------- ------ ---------------------------------------------------  
 （2）rownum对于大于某值的查询条件  
 如果想找到从第二行记录以后的记录，当使用rownum>2是查不出记录的，原因是由于rownum是一个总是从1开始的伪列，Oracle 认为rownum> n(n>1的自然数)这种条件依旧不成立，所以查不到记录  
 SQL> select rownum,id,name from student where rownum >2;  
 ROWNUM ID NAME  
 ---------- ------ ---------------------------------------------------  
 那如何才能找到第二行以后的记录呀。可以使用以下的子查询方法来解决。注意子查询中的rownum必须要有别名，否则还是不会查出记录来，这是因为rownum不是某个表的列，如果不起别名的话，无法知道rownum是子查询的列还是主查询的列。  
 SQL>select * from(select rownum no ,id,name from student) where no>2;  
 NO ID NAME  
 ---------- ------ ---------------------------------------------------  
 3 200003 李三  
 4 200004 赵四  
 SQL> select * from(select rownum,id,name from student)where rownum>2;  
 ROWNUM ID NAME  
 ---------- ------ ---------------------------------------------------  
 （3）rownum对于小于某值的查询条件  
 如果想找到第三条记录以前的记录，当使用rownum<3是能得到两条记录的。显然rownum对于rownum<n（(n>1的自然数）的条件认为是成立的，所以可以找到记录。  
 SQL> select rownum,id,name from student where rownum <3;  
 ROWNUM ID NAME  
 ---------- ------ ---------------------------------------------------  
 1 200001 张一  
 2 200002 王二  
 综上几种情况，可能有时候需要查询rownum在某区间的数据，那怎么办呀从上可以看出rownum对小于某值的查询条件是人为true的，rownum对于大于某值的查询条件直接认为是false的，但是可以间接的让它转为认为是true的。那就必须使用子查询。例如要查询rownum在第二行到第三行之间的数据，包括第二行和第三行数据，那么我们只能写以下语句，先让它返回小于等于三的记录行，然后在主查询中判断新的rownum的别名列大于等于二的记录行。但是这样的操作会在大数据集中影响速度。  
 SQL> select * from (select rownum no,id,name from student where rownum<=3 ) where no >=2;  
 NO ID NAME  
 ---------- ------ ---------------------------------------------------  
 2 200002 王二  
 3 200003 李三  
 （4）rownum和排序  
 Oracle中的rownum的是在取数据的时候产生的序号，所以想对指定排序的数据去指定的rowmun行数据就必须注意了。  
 SQL> select rownum ,id,name from student order by name;  
 ROWNUM ID NAME  
 ---------- ------ ---------------------------------------------------  
 3 200003 李三  
 2 200002 王二  
 1 200001 张一  
 4 200004 赵四  
 可以看出，rownum并不是按照name列来生成的序号。系统是按照记录插入时的顺序给记录排的号，rowid也是顺序分配的。为了解决这个问题，必须使用子查询  
 SQL> select rownum ,id,name from (select * from student order by name);  
 ROWNUM ID NAME  
 ---------- ------ ---------------------------------------------------  
 1 200003 李三  
 2 200002 王二  
 3 200001 张一  
 4 200004 赵四  
 这样就成了按name排序，并且用rownum标出正确序号（有小到大）

   
   
 