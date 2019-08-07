---
title: 将oracle的数据导入到mysql的三种方法
date: 2016-09-09 10:37:32
tags: CSDN迁移
---
   将oracle的数据导入到mysql的三种方法 **一.Navicat Premium中的数据迁移工具** 为了生产库释放部分资源，需要将API模块迁移到mysql中，及需要导数据。

 尝试了oracle to mysql工具，迁移时报错不说，这么大的数据量，用这种简陋的工具不大可靠。

 意外发现平时用的数据库视图工具Navicat Premium中有数据迁移工具，意外的好用。这个工具本身支持mysql，oracle，sqlLite，PostgreSql数据库，因此而也提供了在不同数据库之间迁移数据的功能。

 迁移之前，先确保你建立了这两个数据库的connection。选择Tools/DataTransfer。

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091633262.png) 

 选择源数据库，选择你要迁的表，目标数据库。

 选择周边。

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091633263.png) 

 迁移过程，左上角为进度。

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091633265.png) 

 **二.使用MySQL Migration Toolkit快速将Oracle数据导入MySQL**

 使用MySQL Migration Toolkit快速将Oracle数据导入MySQL

 上来先说点废话

 本人最近在学习一些数据库方面的知识，之前接触过Oracle和MySQL，最近又很流行MongoDB非关系型数据库，所以干脆一起研究一下，对比学习中找不同，首先说一下本人使用的数据库版本和可视化工具

 Oracle10G—PL/SQL Developer9

 MySQL5.5.29—MySQL Workbench6.0

 MongoDB2.4.9(32bit最大2G)—Robomongo0.8.4

 为了保持数据的一致，把现有Oracle中的一部分数据移植到MySQL中，百度之后发现MySQL Migration Toolkit不错，现将步骤写出跟大家分享

 一、安装MySQL Migration Toolkit

 先到http://dev.mysql.com/downloads/gui-tools/5.0.html[下载](http://www.2cto.com/soft)MySQL GUI Tools(其实就是一个MySQL管理工具)，其中就包括MySQL Migration Toolkit工具，一路next安装完毕

 二、第一次运行需要加载ojdbc14.jar包

 运行MySQL Migration Toolkit，一路“Next”到“Source Database”，在Database System中选择Oracle Database Server，如果第一次使用会告之要求加载驱动程序ojdbc14.jar，然后重新启动MySQL Migration Toolkit。

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091633267.jpg) 

 三、加载驱动程序之后，来到Source Database界面将变成如下的形式，在其中填写Oracle[数据库](http://www.2cto.com/database/)的连接信息，按“Next”继续。

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091633269.jpg) 

 四、在Target Database中默认Database System为MySQL Server，在Connection Parameters中填写相应的MySQL数据库的连接信息，按“Next”继续。

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091633270.jpg) 

 五、经过Connecting to Server测试通过后按“Next”，到Source Schemata Selection，点选准备进行数据迁移的数据库后按“Next”继续。

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091634271.jpg) 

 六、经过Reverse Engineering测试通过后按“Next”，在Object Type Selection，点Detailed selection按钮，在下方左侧列表中选择不进行迁移的表，将其放入右侧列表后，即左侧列表剩余的表都将进行数据迁移。选择好之后按“Next”继续。

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091634272.jpg) 

 七、在Object Mapping的Migration of type Oracle Schema，如果要设置参数，点Set Parameter按钮。如果默认数据库表为UTF8的话，则选择Multilanguage;如果默认数据库表为GBK的话，则需要选择User defined，并在下方填写charset=gbk, collation=gbk_general_ci。

 Migration of type Oracle Table中要设置参数点Set Parameter按钮。如果默认数据库表为UTF8的话，则选择Data consistency/multilanguage;如果默认数据库表为GBK的话，则需要选择User defined，并在下方填写addAutoincrement=yes, charset=gbk, collation=gbk_general_ci, engine=INNODB。选择好之后按“Next”继续。

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091634273.jpg) 

 八、经过Migration测试通过后，再到Manual Editing，在这里可以修改建表脚本。由于Oracle与MySQL之间语法规则的差异，通常需要对脚本的数据类型以及默认值进行调整，比如Oracle中通常会对Timestamp类型的数据设置默认值sysdate，但在MySQL中是不能识别的。在Filter中选择Show All Objects，然后在Migrated Objects中选择要修改脚本的表，再点击左下方的Advanced就可以进行脚本编辑了。修改完之后点击右侧Apply Changes按钮保存，按“Next”继续。

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091634274.jpg) 

 九、在Object Creation Options中，选择本地磁盘储存数据表结构，按“Next”继续。

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091635275.jpg) 

 十、经过Creating Objects创建所有表的结构完毕，表中并没有数据，按“Next”继续。

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091635276.jpg) 

 十一、一路“next”来到Data Mapping Options，选择本地磁盘储存数据表中的数据，按“Next”继续。

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091635277.jpg) 

 十二、经过Bulk Data Transfer创建所有表中的数据完毕，按“Next”继续。

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091636278.jpg) 

 十三、来到summary显示此次数据转换的信息，可以保存成文件，按“Finish”完成。

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091636280.jpg) 

 需要补充一点，在导大容量数据特别是CLOB数据时，可能会出现异常：“Packets larger than max_allowed_packet are not allowed”。这是由于MySQL数据库有一个系统参数max_allowed_packet，其默认值为1048576(1M)，可以通过如下语句在数据库中查询其值：show VARIABLES like '%max_allowed_packet%';修改此参数的方法是在[mysql](http://www.2cto.com/database/MySQL/)文件夹找到my.ini文件，在my.ini文件[mysqld]中添加一行：max_allowed_packet=16777216

 重启MySQL，这样将可以导入不大于16M的数据了，当然这数值可以根据需要作调整。

 十四、使用MySQL Workbench导入数据

 点击Data Import/Restore先导入一次表结构，再导入一次表数据，完成数据库迁移

 ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------  
**三. 先把oracle表中的数据另存在excel表中，再把excel表中数据导入到mysql中**

 这里要将oracle中表eventlogs的数据导入到mysql中。步骤如下：

 1、在PL/SQL中用select * 搜索到eventlogs表的所有数据，然后右键点击"Copy to Excel";如下图所示：

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091637281.jpg) 

 2、将数据保存为excel表，并重名;如下图：

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091637282.jpg) 

 3、打开mysql的可视化工具，我这里是Navicat,选择表，点击导入向导;如下图所示：

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091637283.jpg) 

 4、选择上图中"导入类型"的"execel文件",然后点击"下一步";如下图所示：

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091637284.jpg) 

 5、接下来会让你选择文件，选择你已经保存的excel文件，并选择“SQL Result”，如下图所示：

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091638285.jpg) 

 6、然后一直点下一步直至步骤6，填充目标栏位。第一个栏位一般是空的，根据你的需要填。我这里填为"_id"，并设为主键。如下图所示：

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091638286.jpg) 

 7、然后一直点击下一步，最后一步点击开始。会出现一个“sql result”的表，重命名为你想要的表即可。如图所示：

 

 ![\](http://www.2cto.com/uploadfile/Collfiles/20160601/20160601091638288.jpg) 

 数据已经导入了。其中表中数据类型若不合你的要求，你可以再设计表。

    
 