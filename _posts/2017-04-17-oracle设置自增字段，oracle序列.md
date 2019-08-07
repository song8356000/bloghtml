---
title: oracle设置自增字段，oracle序列
date: 2016-10-24 16:36:57
tags: CSDN迁移
---
   如果没有表可以先创建个表

 **Sql代码 **

 ****

 
```
. ###建表###  
2.   
3. CREATE TABLE "NWVIDEO"."DEVICE"  
4. (  
5. "DID" NUMBER NOT NULL,  
6. "TYPE" NUMBER(3) NOT NULL,  
7. "BODY" VARCHAR2(100) NOT NULL,   
8. "HITS" NUMBER(10) DEFAULT 0 NOT NULL,   
9. PRIMARY KEY("ID")  
10.)  
```
  
  


 然后是设置序列

 **Sql代码 **

 ****

 
```
1. ###创建序列###   
2.   
3. CREATE SEQUENCE "NWVIDEO"."NWVIDEO_DEVICE_DID_SEQ"   
4. INCREMENT BY 1     --每次加1个  
5. START WITH 1           --从1开始计数  
6. NOMAXVALUE            --不设置最大值  
7. MINVALUE 1                --最小值为1  
8. NOCYCLE                    --一直累加，不循环  
9. CACHE 50                   --设置缓存为50  
 建立触发器
```
  
  


 **Sql代码 **

 ****

 
```
1. ###建自动更新的触发器###  
2.   
3. CREATE OR REPLACE TRIGGER "NWVIDEO"."NWVIDEO_DEVICE_DID_TRIGGER"  
4. BEFORE INSERT  
5. ON "NWVIDEO"."DEVICE"  
6. FOR EACH ROW  
7. DECLARE  
8. next_did NUMBER;  
9. BEGIN  
10.--Get the next id number from the sequence  
11.SELECT "NWVIDEO_device_did_seq".NEXTVAL     -- 这里涉及到oracle序列，详细可以参考<a target=_blank href="http://blog.csdn.net/qq_22642239/article/details/52912893">oracle序列详解</a>
12.INTO next_did  
13.FROM dual;                  -- 这里涉及到oracle  的dual表，详细可以查看 <a target=_blank href="http://blog.csdn.net/qq_22642239/article/details/52913071">ORACLE dual 表详解</a>
14.--Use the sequence number as the primary key  
15.--for the record being inserted.  
16.:new.did := next_did;  
17.END;  
```
  
  


 如果did字段是主键的话

 可以建一个主键保护器

 **Sql代码 **

 ****

 
```
1. ###建保护PRIMARYKEY的触发器###   
2.   
3. CREATE OR REPLACE TRIGGER "TEST"."NWVIDEO_DID_UPDATE_TRIGGER"  
4. BEFORE UPDATE OF "DID" ON "NWVIDEO"."DEVICE"  
5. FOR EACH ROW  
6. BEGIN  
7. RAISE_APPLICATION_ERROR (-20000,'vwvideo_device_did_update_trigger:Updates of the DID field'||'are not allowed.');   
8. END;  
 
```
  
  


 

 

   
 