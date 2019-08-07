---
title: oracle学习（1）--------windows环境下的安装和基本操作
date: 2016-07-28 15:02:34
tags: CSDN迁移
---
 版权声明：需要转载的请注明出处 https://blog.csdn.net/qq_22642239/article/details/52052773   
   **最近刚到公司上班，大致了解了一下公司的流程之后，发现数据库非常需要恶补一下，以前在学校学习的一些数据库都是SQLserver，以及一些增，删，改，查的基本操作，**

 **这些都是好早好早以前的事了，后来大部分时间都用来学习汇编，搞逆向去了，为了自己的发展必须要打好开发的基础，当然数据库是必要的，oracle对我来说确实感觉非常陌生以前基本没有接触过，经过最近几天的折腾，大概了解了一下oracle数据库的安装，简单的配置，毕竟还是小白，以后还需要继续恶补，下面就是最近自己的一些新的和收获。**

 **1. 首先是oracle的安装**

 ** 对于一个软件的安装，在我的印象中，貌似绝大多数软件都是非常容易安装的，一般安装软件会自己配置好自己的运行环境，自己创建并开启自己所需要的一些服务，以及在注册表中创建一些需要用到的项，等等，但是我在安装oracle的时候并没有那么顺利，我记得刚开始安装的时候使用的是公司的电脑（win7旗舰版64位，配置还凑合，组装的机器。。），首先我安装的版本是 win64_11gR2_database （当时10 那个版本用的很少，也就没用了）11g这个版本的如下图所示：**

 **![](https://img-blog.csdn.net/20160728111011894?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
**

 **  
**

 **相信安装过的都对这个界面很熟悉，直接点击下一步：![](https://img-blog.csdn.net/20160728112628450?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)**

 **上面这个界面我们可以根据自己的实际情况选择，如果是初次安装的话，就选择第一个就可以了，**

 **点击下一步：**

 **  
**

 **![](https://img-blog.csdn.net/20160728111529805?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
**

 **关于上面这个 有两个选项，就是一个是桌面类，一个是服务器类，其实我么你用的话完全可以选择桌面类，对以后使用也没什么影响（个人感觉），如果先泽服务器类的话，配置会更加详细，这个可以根据自己的实际情况来选择，这次我们就先选择桌面类，点击下一步：**

 **![](https://img-blog.csdn.net/20160728111533915?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
**

 **上面这些就是即将要创建的数据库的一些位置，字符类型，什么类型的数据库，这里我选择的是企业版，这里可以按照默认的就可以了，需要注意的就是管理口令，这个口令使以后登录数据库的时候的口令，也就是密码，这个一定要记住，用这个口令登录的用户名称有system sys 等这几个用户的口令都是这个自己设置的口令，这个口令的格式可以随意输入，虽然他会提示不符合规则，但是可以忽略，这个对于我们自己并非特别专业的人员来说并没有什么影响。点击下一步：**

 **![](https://img-blog.csdn.net/20160728112844294?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
**

 **出现上面这个界面，我们只需要等待一下就可以了，如果正常的话会出现这个界面：**

 **![](https://img-blog.csdn.net/20160728112848215?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
**

 **遇见这个界面，我们安静的等待就可以了，这个需要好长时间，这里面最容易出现错误，当时我安装的时候就在这里出现了一大堆的错误，首先是一个服务未响应或启动，在这里需要说明一下，oracle正常运行的话，所需要启动的服务是这几个：**

 ****

 **Oracle ORCL VSS Writer Service，OracleDBConsoleorcl，OracleJobSchedulerORCL，**

 ** OracleMTSRecoveryService，OracleOraDb11g_home1ClrAgent，OracleOraDb11g_home1TNSListener，OracleServiceORCL。其中OracleDBConsoleorcl，**

 ** OracleMTSRecoveryService，OracleOraDb11g_home1TNSListener，OracleServiceORCL是默认自动启动的，OracleJobSchedulerORCL是默认自动禁止的，其余的默认为**

 **这七个服务的含义分别为：**

 ** Oracle ORCL VSS Writer Service：Oracle卷映射拷贝写入服务，VSS(Volume Shadow Copy Service)能够让存储基础设备(比如磁盘，阵列等)创建高保真的时间点映像，即映射拷贝(shadow copy)。它可以在多卷或者单个卷上创建映射拷贝，同时不会影响到系统的系统能。(非必须启动)**

 ** OracleDBConsoleorcl：Oracle数据库控制台服务，orcl是Oracle的实例标识，默认的实例为orcl。在运行Enterprise Manager(企业管理器OEM)的时候，需要启动这个服务。(非必须启动)**

 ** OracleJobSchedulerORCL：Oracle作业调度(定时器)服务，ORCL是Oracle实例标识。(非必须启动)**

 ** OracleMTSRecoveryService：服务端控制。该服务允许数据库充当一个微软事务服务器MTS、COM/COM+对象和分布式环境下的事务的资源管理器。(非必须启动)**

 ** OracleOraDb11g_home1ClrAgent：Oracle数据库.NET扩展服务的一部分。 (非必须启动)**

 ** OracleOraDb11g_home1TNSListener：监听器服务，服务只有在数据库需要远程访问的时候才需要。(非必须启动，下面会有详细详解)。**

 ** OracleServiceORCL：数据库服务(数据库实例)，是Oracle核心服务该服务，是数据库启动的基础， 只有该服务启动，Oracle数据库才能正常启动。(必须启动)**

 ** 那么在开发的时候到底需要启动哪些服务呢?**

 ** 对新手来说，要是只用Oracle自带的sql*plus的话，只要启动OracleServiceORCL即可，要是使用PL/SQL Developer等第三方工具的话，OracleOraDb11g_home1TNSListener服务也要开启。OracleDBConsoleorcl是进入基于web的EM必须开启的，其余服务很少用。**

 **手动操作。** **我遇到的问题1**

 **我在安装的时候这个 OracleOraDb11g_home1TNSListener 服务有时候会启动不了，这个服务所依赖的是E:\app\admin\product\11.2.0\dbhome_2\BIN\TNSLSNR 这个路径下的一个可执行文件**

 **![](https://img-blog.csdn.net/20160728140931890?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
**

 **有时候会缺少这个文件导致服务不能正常运行。**

 **我遇到的问题2：**

 ****

 **OracleServiceORCL 这个服务无法正常启动，原因是因为这个服务所依赖的oracle.exe 这个可执行文件不能正常启动，**

 **![](https://img-blog.csdn.net/20160728141309942?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
**

 **我们可以点击这个服务，查看属性局可以看到这个服务得一些依赖的文件，当时我的电脑的环境出错，导致这个可执行文件不能正常运行，最后重新装了一下系统，解决了，有的时候是因为卸载以前装过的oracle不彻底导致，这几点都需要在出现安装错误的时候去考虑一下，当时我也是因为这几个问题折腾了好几天。**

 **  
**

 **  
**

 **如果上面都安装顺利的话，那么你的oracle的数据库的基本环境已经搭载好了，下面的就是需要创建新的用户，设置相应的权限，导入相应的数据库备份（.dmp文件），创建表空间等等的一些操作。为了适应自己的需求我以自己的一些实际操作过程大概说一下自己最近的一些操作：**

 ** 如果安装成功，那么就可以打开数据库的命令界面输入相应的数据库命令即可（有的人也许会问在哪里输入数据库的命令呢？ 其实为了方便可以直接win+R 输入cmd，进入黑框也就是windows的命令行界面，输入sqlplus 就会提示你属入用户名 在没有创建新的用户前可以输入system 这个不区分大小写，然后输入口令就是安装的时候设置的那个口令，然后就会进入数据库的命令行界面，![](https://img-blog.csdn.net/20160728143140046?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)）**

 **下面就可以输入相应的数据库命令了**

 **下面的一些操作是我使用的时候做的一些基本操作：**

 **首先创建表空间（就是开辟空间用来存放东西）**

 **创建采集流程库数据表空间   
 CREATE TABLESPACE HIIP_DEF LOGGING DATAFILE 'E:\app\admin\oradata\orcl\HIIP_DEF.dbf' SIZE 64M AUTOEXTEND ON NEXT 64M MAXSIZE UNLIMITED EXTENT MANAGEMENT LOCAL AUTOALLOCATE;   
 --创建采集流程库临时表空间   
 CREATE temporary TABLESPACE HIIP_TEMP tempfile 'E:\app\admin\oradata\orcl\HIIP_TEMP.dbf' SIZE 64M AUTOEXTEND ON NEXT 64M MAXSIZE UNLIMITED EXTENT MANAGEMENT LOCAL;  
**

 **-- 创建采集流程库用户 （创建一个新的登录用户，就像是system，但是还需要给这个创建的用户授予一些权限）  
 create user hiip identified by hiip  
 default tablespace HIIP_DEF  
 temporary tablespace HIIP_TEMP  
 profile DEFAULT  
 quota unlimited on HIIP_DEF;  
 -- Grant/Revoke object privileges 给用户授目标权限  
 grant select, insert, update, delete on SYS.DBA_JOBS to hiip;  
 -- Grant/Revoke role privileges 给用户授角色权限  
 grant connect to hiip;  
 grant dba to hiip;  
 grant resource to hiip;  
 -- Grant/Revoke system privileges 给用户授系统权限  
 grant create database link to hiip;  
 grant force any transaction to hiip;  
 grant unlimited tablespace to hiip;  
 grant execute any procedure to hiip;  
 grant create any table to hiip;  
 grant select any table to hiip;  
 grant insert any table to hiip;  
 grant delete any table to hiip;  
 grant update any table to hiip;  
**

 **需要注意的一点是oracle的命令要以“;”结尾，不然的话会出错误。**

 **  
**

 **下面介绍一下怎么将一个备份的数据库文件（.dmp）导入到需要的数据库中，在这里我讲一个数据库文件导入到hiip这个用户中**

 ** **

 ** 刚开始我也不知道怎么导入的，后来也是问别人的，那要怎么做呢，不是在这个命令行里边，从新打开一个CMD窗口，输入imp 命令**

 ** 就会出现输入用户名和口令的界面，输入刚才创建的用户名hiip输入口令，就会提示是否要导入相应的数据库文件，我么你直接输入yes就可以了，然后将需要导入的数据库的文件的路径输入到上面去，OK，enter 就可以了看见数据库文件正在被导入库中。**

 **如果出现权限问题，可以授予用户相关权限：**

 **GRANT CREATE USER,DROP USER,ALTER USER ,CREATE ANY VIEW ,  
 DROP ANY VIEW,EXP_FULL_DATABASE,IMP_FULL_DATABASE,  
 DBA,CONNECT,RESOURCE,CREATE SESSION TO 用户名字  
**

 **下面就是怎么将数据库的文件导出 那就是exp 命令 ，同样是在cmd下执行，导出的时候需要选则导出的内容，这个可以根据自己的情况选择，还需要输入导出的文件存放的位置和文件名称，这个输入之后就会自己创建，导出成功后就会出现XX.dmp文件 这个就是导出的数据库文件。**

 **  
 数据库的其他一些操作以后继续熟悉。**  


   
 