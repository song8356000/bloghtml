---
title: ActiveBpel部署运行BPEL流程实例
date: 2017-03-23 17:00:03
tags: CSDN迁移
---
   本文接收使用ActiveBpel开发，部署和运行BPEL流程实例。  
 本文的例子工程和服务工程请见附件。  
 1. 安装ActiveBpel5.0.2  
 要安装ActiveBpel5.0.2，需要先安装JDK1.5，Tomcat。注意这里是JDK1.5版本的，ActiveBpel5.0.2不支持JDK1.5以上的版本。  
 （1） 安装JDK1.5  
 安装jdk后设置JAVA_HOME= C:\Program Files\Java\jdk1.6.0_07，这是我的jdk的安装目录。  
 （2） 安装Tomcat5.5.27  
 具体的安装过程这里就不再叙述，安装完成后，设置环境变量CATALINA_HOME= E:\apache-tomcat-5.5.27，这是我的安装目录。  
 （3） 安装ActiveBpel5.0.2  
 设置完成CATALINA_HOME环境变量后，就可以安装ActiveBpel5了，解压rar包后安装目录下有install.bat，运行这个文件，即可安装成功。  
 2. 设计BPEL流程  
 设计BPEL流程需要安装BPEL流程设计工具，本文用到的BPEL设计工具是ActiveBpel Designer。  
 本文开发一个简单的流程实例，该Bpel流程只调用一个简单的echo服务，该服务输入一个字符串，返回的结果是同样的一个字符串。因此整个Bpel流程的结果也是一个这样的字符串。这个echo服务是mule开发的服务，工程名称echoService，工程见压缩包。  
 服务实现代码很简单，如下。  
 package cn.xidian.repace.zhaolong;  
 public class EchoImp implements InterfaceEcho{  
 public String echo（String str）{  
 return str;  
 }  
 }  
 要开发BPEL流程，需要有相关的WSDL文件，该Bpel工程用到两个wsdl文件。一个是Bpel文件本身的wsdl文件BpelEchoTest.wsdl，一个是被调用服务的wsdl文件EchoTest.wsdl。  
 Bpel本身的wsdl文件BpelEchoTest.wsdl如下。  
 <?xml version="1.0" encoding="UTF-8"?>  
 <wsdl:definitions targetNamespace="" xmlns:impl="" xmlns:bpws="-process/" xmlns:intf="" xmlns:wsdlsoap="" xmlns:apachesoap="-soap" xmlns:soapenc="" xmlns:plnk="-link/" xmlns:xsd="" xmlns:wsdl="" xmlns="">  
 <wsdl:message name="bpelResponse">  
 <wsdl:part name="bpelReturn" type="xsd:string"/>  
 </wsdl:message>  
 <wsdl:message name="bpelRequest">  
 <wsdl:part name="in0" type="xsd:string"/>  
 </wsdl:message>  
 <wsdl:portType name="BpelProxy0">  
 <wsdl:operation name="bpel" parameterOrder="in0">  
 <wsdl:input name="bpelRequest" message="impl:bpelRequest"/>  
 <wsdl:output name="bpelResponse" message="impl:bpelResponse"/>  
 </wsdl:operation>  
 </wsdl:portType>  
 <wsdl:binding name="bpelTestSoapBinding" type="impl:BpelProxy0">  
 <wsdlsoap:binding transport="" xmlns:wsdlsoap=""/>  
 <wsdl:operation name="bpel">  
 <wsdlsoap:operation soapAction="" xmlns:wsdlsoap=""/>  
 <wsdl:input name="bpelRequest">  
 <wsdlsoap:body encodingStyle="" namespace="" use="encoded" xmlns:wsdlsoap=""/>  
 </wsdl:input>  
 <wsdl:output name="bpelResponse">  
 <wsdlsoap:body encodingStyle="" namespace="" use="encoded" xmlns:wsdlsoap=""/>  
 </wsdl:output>  
 </wsdl:operation>  
 </wsdl:binding>  
 <wsdl:service name="bpelTest">  
 <wsdl:port name="bpelTest" binding="impl:bpelTestSoapBinding">  
 <wsdlsoap:address location="" xmlns:wsdlsoap=""/>  
 </wsdl:port>  
 </wsdl:service>  
 <plnk:partnerLinkType name="MyBpelPLT" xmlns:plnk="-link/">  
 <plnk:role name="bpel">  
 <plnk:portType name="impl:BpelProxy0"/>  
 </plnk:role>  
 </plnk:partnerLinkType>  
 </wsdl:definitions> 

   
 开发好echoService mule服务后，运行该服务，在浏览器输入服务地址，获取wsdl文件，另存为EchoTest.wsdl文件。  
 使用ActiveBpel Designer开发流程，工程名是TestBpel，bpel文件名是mybpel.bpel，图形如下所示。  
 mybpel.bpel代码如下所示。  
 <?xml version="1.0" encoding="UTF-8"?>  
 <!--  
 BPEL Process Definition  
 Edited using ActiveBPEL（tm） Designer Version 2.1.0 （）  
 -->  
 <process xmlns="-process/" xmlns:bpws="-process/" xmlns:ns1="" xmlns:xsd="" name="mybpel" suppressJoinFailure="yes" targetNamespace=//mybpel">  
 <partnerLinks>  
 <partnerLink myRole="bpel" name="MyBpelPLT" partnerLinkType="ns1:MyBpelPLT"/>  
 <partnerLink name="MyEchoPLT" partnerLinkType="ns1:MyEchoPLT" partnerRole="echo1"/>  
 </partnerLinks>  
 <variables>  
 <variable messageType="ns1:bpelRequest" name="bpelRequest"/>  
 <variable messageType="ns1:bpelResponse" name="bpelResponse"/>  
 <variable messageType="ns1:echoRequest" name="echoRequest"/>  
 <variable messageType="ns1:echoResponse" name="echoResponse"/>  
 </variables>  
 <sequence>  
 <receive createInstance="yes" operation="bpel" partnerLink="MyBpelPLT" portType="ns1:BpelProxy0" variable="bpelRequest"/>  
 <assign>  
 <copy>  
 <from part="in0" variable="bpelRequest"/>  
 <to part="in0" variable="echoRequest"/>  
 </copy>  
 </assign>  
 <invoke inputVariable="echoRequest" operation="echo" outputVariable="echoResponse" partnerLink="MyEchoPLT" portType="ns1:EchoProxy0"/>  
 <assign>  
 <copy>  
 <from part="echoReturn" variable="echoResponse"/>  
 <to part="bpelReturn" variable="bpelResponse"/>  
 </copy>  
 </assign>  
 <reply operation="bpel" partnerLink="MyBpelPLT" portType="ns1:BpelProxy0" variable="bpelResponse"/>  
 </sequence>  
 </process>  
 3. 部署流程  
 开发完成流程后就可以部署流程了。选择File/New/Deployment Descriptor，新建流程描述符，选择该工程下的mybpel.bpel文件，点击“下一步”。见到下图，进行如下图设置。  
 然后将该工程export，生成一个bpr文件，文件名为TestBpel.bpr，将该文件放到Tomcat目录下的bpr文件夹下，这个文件夹是安装activebpel生成的。  
 在浏览器中输//localhost:8080/active-bpel/servlet/AxisServlet 即可查看到该流程服务的项，如下图所示。  
 4. 运行流程  
 部署完成流程后，就可以运行了。运行有两种方式，一种是代码方式，一种是使用soup发送方式，发送soup消息给该流程，返回结果。  
 第一种代码方式是普通的方式，使用Axis2的服务调用方式调用bpel流程服务即可，这里就不再详细描述，请见我的另一篇文章：使用Eclipse+Axis2构建Web Service应用（）。  
 第二种方式是使用soupui软件，安装好soupui软件后，新建工程，输入名称和刚才的bpel流程地址//localhost:8080/active-bpel/services/MyBpelPLTService?wsdl，输入数据。把<in0 xsi:type="xsd:string">?</in0>处的？换为任意一个字符串，右方就会输出这个字符串。 运行成功。   
 