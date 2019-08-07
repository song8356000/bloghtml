---
title: 安装ActiveBPEL引擎（tomcat5.5+mysql5.1+ActiveBPEL5.0.2）
date: 2017-03-23 17:03:23
tags: CSDN迁移
---
   因为要学习服务计算，想搭建个环境做服务组合，选择了开源的ActiveBPEL。开源的东西果然不是白白免费的，光[版本控制](http://lib.csdn.net/base/git)的问题就足够让人崩溃，用了一天半的时间才配好。其中最为难缠的是JNDI数据源配置，涉及到DBCP、JDBC和Tomcat，我更是在版本上吃足了苦头，好在终于守得云开见月明，引擎终于跑起来了。

 

 一、版本问题

 ActiveBPEL引擎选择的是5.0.2，需要Servlet容器支持，这里选择的是Tomcat。只有Tomcat5.5.27及以下的版本才可以，高一点版本与ActiveBPEL不兼容。

 ActiveBPEL的持久化需要JNDI数据源。在Tomcat中配置JNDI最好的参考资料是Tomcat的DataSource和Resources HOWTO文档。我的Tomcat的JRE是JDK1.5，而目前的DBCP有1.3和1.4两个版本，分别与JDBC3和JDBC4兼容，同时分别兼容JDK1.5及以下和JDK1.6。因为JDK是1.5，所以选了较低的版本。

 二、安装ActiveBPEL

 最好的参考文档自然是安装包自带的文档，位于根目录/doc下的install_engine.txt，E文的，不过很容易读懂。

 第一步，安装Tomcat并配置CATALINA_HOME环境变量。

 第二步，命令行下执行install.bat(windows)。 执行后在Tomcat根目录下生成bpr目录和webapps下四个部署文件，bpr下是ActiveBPEL的配置文件。

 随后，启动Tomcat和关闭Tomcat会自动启动和关闭ActiveBPEL引擎。

 Tomcat启动后，浏览器中键入[http://localhost:8080/BpelAdmin/](http://localhost:8080/BpelAdmin/)即看到管理界面，表示安装成功，引擎的状态为Running。这时HOME页中的Description表示采用的是In_Memory模式。

 三、ActiveBPEL的持久化

 参考文档是doc目录下的persistence_setup.txt文档。

 第一步建[数据库](http://lib.csdn.net/base/mysql)，采用mysql5.1。执行安装包目录下dist/sql/activebpel/ddl/ActiveBPEL-[MySQL](http://lib.csdn.net/base/mysql).sql建立activebpel数据库。Windows下默认不区分大小写。我在mysql命令行下用source命令完成。

 第二步在Tomcat中配置JNDI数据源。可能因为DBCP和JDBC版本的问题，开始按照参考文档，在Tomcat的Administration Web Application中配置JNDI数据源没有成功，后来直接在Tomcat下写的配置文件，没有再去验证Administration Web Application能否成功。步骤如下：

 1. 配置上下文内容如下。内容要保存在xml文件中，我试过两种方式。一种在CATALINA_HOME//conf/Catalina/localhost下的active-bpel.xml，另一种是Web Application的META-INF下的context.xml。推荐第二种，可以容易得移植到其他Web应用服务器中。

 <?xml version="1.0" encoding="UTF-8"?>  
 <Context>  
 <Resource  
 name="jdbc/ActiveBPELDB"  
 auth="[Container](http://lib.csdn.net/base/docker)"  
 type="javax.sql.DataSource"  
 driverClassName="com.mysql.jdbc.Driver"  
 username="root"  
 password="123"  
 maxIdle="4"  
 maxWait="10000"   
 url="jdbc:mysql://localhost:3306/ActiveBPEL?autoReconnect=true"  
 maxActive="10"/>  
 </Context>

 2. 设置对上下文中配置的资源的引用。通常情况下在Web Application的web.xml中配置引用如下：

 <resource-ref>  
 <description>activebpel</description>  
 <res-ref-name>jdbc/ActiveBPELDB</res-ref-name>  
 <res-type>javax.sql.DataSource</res-type>  
 <res-auth>Container</res-auth>  
 </resource-ref>

 ActiveBPEL略有区别。web.xml中不需要配置，要把dist/conf下的aeEngineConfig-Persisitent.xml拷贝到Tomcat目录下的bpr目录中，并删掉原来的aeEngineConfig.xml，将拷贝过来的文件改名为aeEngineConfig.xml。打开aeEngineConfig.xml，修改<entry name="DatabaseType" value="mysql" />。

 然后重启Tomcat，如果没有问题，[http://localhost:8080/BpelAdmin/](http://localhost:8080/BpelAdmin/)页面的引擎的状态为Running，这时HOME页中的Description表示采用的是Persistent模式。

 四、检测JNDI数据源

 如果配置出错，很可能是JNDI数据源出错，这里给出个检验的小例子。可以在myeclipse建个Web Project，访问数据源检验。index.jsp如下：

 <%@ page language="[Java](http://lib.csdn.net/base/javase)" import="java.util.*" pageEncoding="utf-8"%>  
 <%@ page import="java.sql.*" %>  
 <%@ page import="javax.sql.*" %>  
 <%@ page import="javax.naming.*" %>  
  
 <html>  
 <head>  
   
 <title>JNDI Test</title>  
   
 </head>  
   
 <body>  
 <%   
 Context initCtx = new InitialContext();  
 Context envCtx = (Context) initCtx.lookup("java:comp/env");  
 DataSource ds = (DataSource)envCtx.lookup("jdbc/ActiveBPELDB");  
 Connection con=ds.getConnection();   
 Statement stmt=con.createStatement();  
 ResultSet rs=stmt.executeQuery("select* from aemetainfo");  
 %>  
   
 <h2>Results</h2>  
   
 <table border="1" cellspacing="0" cellpadding="0" align="center">  
 <tr>  
 <th>property</th>  
 <th>value</th>  
 </tr>  
 <%while(rs.next()){%>  
 <tr>  
 <td><%=rs.getString(1)%></td>  
 <td><%=rs.getString(2)%></td>  
 </tr>  
 <%}%>  
 </table>

 <%con.close(); %>   
   
 </body>  
 </html>

   
 