---
title: 使用 Eclipse BPEL 插件开发和执行 WS-BPEL V2.0 业务流程
date: 2017-03-23 17:06:46
tags: CSDN迁移
---
   Apache Foundation 把它的 Web 服务业务流程执行语言（Web Services Business Process Execution Language，WS-BPEL）V2.0 实现称为 Orchestration Director Engine (ODE)。ODE 将执行 WS-BPEL 流程，这些流程能够与 Web 服务进行通信、发送和接收消息等。Eclipse BPEL 项目是一个相关的开源项目，该项目将为 WS-BPEL V2.0 流程的可视化开发提供一个 Eclipse 插件（如果您还不了解这项技术，请参阅 “WS-BPEL 是什么”）。

 撰写本文时，ODE V1.1 和 Eclipse BPEL 项目里程碑 M3 是最新版本。本文将检验这些产品并介绍如何使用 Apache ODE 和 Eclipse BPEL 项目创建您自己的 BPEL 流程并将其集成到应用程序中。

 如果您更为熟悉 BPMN，则可能需要查看由 Tyler Anderson 撰写并发表在 developerWorks 中的文章 “用 Eclipse 执行业务流程”，要处理业务流程，还需要查看使用 Eclipse STP BPMN Modeler 的教程（请查阅 [参考资料](https://www.ibm.com/developerworks/cn/opensource/os-eclipse-bpel2.0/#resources)）。

 
## 软件安装

 您的操作系统可以是近期版本的 Microsoft® Windows®、Linux® 或 Mac OS X。本文是使用 Linux 撰写的，因此您可能需要根据操作系统的风格调整文件位置。在为 Eclipse 安装 ODE 和 BPEL 之前，确保您的计算机已经安装了下列软件：

 
  1. Java™ V5.0 或更高版本
  2. Tomcat V5.5 或更高版本
  3. 安装了以下插件的 Eclipse V3.3.x： 
       * EMF V2.3.x
       * GEF V3.3.x
       * DTP STK V1.5.x
       * WTP (Web Tools Platform) V2.0.x请参考各个应用程序的安装指南进行安装（请参阅 [参考资料](https://www.ibm.com/developerworks/cn/opensource/os-eclipse-bpel2.0/#resources)）。

 
### Apache ODE 安装

 下载 ODE。启动 Apache Tomcat Web 容器并使用 Tomcat Manager（应当会在您的计算机中的 [http://localhost:8080/manager/html](http://localhost:8080/manager/html) 找到）部署发行版归档中的 ode.war 模块。要检查是否成功，请访问 [http://localhost:8080/ode/](http://localhost:8080/ode/)，该地址将显示您计算机的 ODE Web 服务的状态。

 
### Eclipse 的 BPEL 支持

 要安装 Eclipse BPEL 项目，请运行 Eclipse 应用程序，启动 Eclipse 更新管理器（通过单击菜单 Help > Software Update > Find & Install）并选择 New feature 来安装选项。单击 New Remote Site... 并把 URL http://download.eclipse.org/technology/bpel/update-site/ 添加到 New Update Site 对话框中，然后把站点命名为 BPEL。单击 Finish，选择最近的镜像，选择 BPEL Designer for Eclipse，同意许可证条款，单击Select All 选项，然后单击 Next 和 Finish。Eclipse 将提醒所有潜在的复制冲突，然后警告 BPEL 正被取消签名。如果提供者是 Eclipse.org，单击 Install。安装完成时，Eclipse 将询问您是否重新启动计算机。

   
 [回页首](https://www.ibm.com/developerworks/cn/opensource/os-eclipse-bpel2.0/#ibm-pcon)

 
## 创建一个简单的 BPEL 流程

 在此部分中，我们将创建一个简单的 BPEL 流程并尝试在 ODE 中运行它。此流程仅执行简单的字符串处理。记住，它只是一个简单示例，而且您可以使用 WS-BPEL V2.0 规范创建更加复杂的流程。

 
### Eclipse BPEL 编辑器

 要创建一个简单的 BPEL 流程，请运行 Eclipse 并单击 File > New > Other 菜单项，选择 BPEL 2.0 -> BPEL Project 选项并创建一个名为 HelloWorld 的新 BPEL 项目。创建了项目后，您就可以创建第一个 BPEL 流程了。再次单击 File > New > Other 菜单项并选择 BPEL 2.0 > New BPEL Process File 选项来启动 BPEL 流程创建向导。

 
##### 图 1. 创建 BPEL 流程文件

 ![创建 BPEL 流程文件](https://www.ibm.com/developerworks/cn/opensource/os-eclipse-bpel2.0/process-creation-dialog.jpg)让我们创建一个同样名为 HelloWorld 的同步流程。在向导的下一页中，您应当选择 BPEL 流程文件位置 —— 只需选择先前创建的项目并单击Finish。系统将创建样例流程。

 
##### 图 2. 创建一个同步流程

 ![创建一个同步流程](https://www.ibm.com/developerworks/cn/opensource/os-eclipse-bpel2.0/helloworld-process1.jpg)如示，新建流程有两个内部 BPEL 变量 —— input 和 output —— 以及带有一个接收元素和一个应答元素的序列。接收块负责接收输入的 BPEL 流程数据和变量输入的初始化。类似地，应答块旨在使用输出变量输出 BPEL 流程数据。

 同样在创建 BPEL 流程期间，向导创建了一个 WSDL 文件。此文件将描述输入和输出数据类型以及表示 BPEL 流程的端口类型。输入和输出数据类型都只包含单个字符串字段。

 我们已经准备好把一些数据处理添加到流程中。为此，请在  `receiveInput`  块和  `replyOutput`  块之间拖放一个 Assign 块。

 
##### 图 3. 拖放 Assign 块

 ![拖放 Assign 块](https://www.ibm.com/developerworks/cn/opensource/os-eclipse-bpel2.0/helloworld-process2.jpg)在放置了赋值块后，右键单击 Assign 块并选择 Show In Properties 上下文菜单项以显示 Properties 视图并选择 Details 选项卡。Assign 块应当会用一个初始的 XML 标记结构来初始化输出变量。它是大多数 BPEL 处理引擎必需的常用程序。单击 Detail 选项卡中的 New 按钮以开始创建第一个赋值程序。在 From 选择框中选择 Fixed Value 元素并在文本区域中输入下列行：

 ```
>tns:HelloWorldResponse xmlns:tns="http://www.ibm.com/wd2/ode/HelloWorld"> >tns:result/> >/tns:HelloWorldResponse>
```
   
 接下来，在 To 选择框中选择 Variable 元素并在变量树中选择 output/payload 变量，如下所示：

 
##### 图 4. 初始化输出变量

 ![初始化输出变量](https://www.ibm.com/developerworks/cn/opensource/os-eclipse-bpel2.0/assign-properties1.jpg)在初始化了输出变量后，我们可以创建一个新的赋值程序。对于样例流程，此程序将从输入变量中获取一个字符串值，把它与 “Hello” 问候语连接起来并把结果赋给输出变量字符串字段。为此，单击 New 按钮并在 From 选择框中选择 Expression 值。在下面显示的文本框中，输入以下表达式： `concat("Hello
 ", $input.payload/tns:input)` 。在 To 部分中，选择 output/payload/tns:result 变量，如下所示：

 
##### 图 5. 选择 output/payload/tns:result

 ![选择 output/payload/tns:result](https://www.ibm.com/developerworks/cn/opensource/os-eclipse-bpel2.0/assign-properties2.jpg)Assign 块已经就绪。正如您所见，Eclipse BPEL 插件将提供 XPath V1.0 突出显示功能和代码完成功能来简化 XPath 查询的创建操作（要了解关于 XPath 的更多信息，请参阅 [参考资料](https://www.ibm.com/developerworks/cn/opensource/os-eclipse-bpel2.0/#resources)）。

 
### WSDL 文件更改

 BPEL 流程的 WSDL 文件应当描述 BPEL 流程中使用的类型以及端口类型、绑定和流程的服务。由向导自动创建的 WSDL 已经包含一个端口类型，而且为了使流程可以运行，我们需要为它创建绑定和服务。

 在单独的编辑器中打开 WSDL 文件并查看端口类型图形化定义。

 
##### 图 6. 查看端口类型图形化定义

 ![查看端口类型图形化定义](https://www.ibm.com/developerworks/cn/opensource/os-eclipse-bpel2.0/helloworld-porttype.jpg)要创建 Web 服务绑定，请右键单击 WSDL 编辑器中的任意空白区域并选择 Add Binding 选项。在 Properties 视图中，把新建绑定重命名为 HelloWorldBinding 并选择 HelloWorld 作为新绑定的端口类型。单击 Generate Binding Content 按钮将显示 Binding Wizard 对话框。

 
##### 图 7. Binding Wizard 对话框

 ![Binding Wizard 对话框](https://www.ibm.com/developerworks/cn/opensource/os-eclipse-bpel2.0/helloworld-bindingwizard.jpg)在 Protocol 选择框中选择 SOAP 协议，在 SOAP Binding Options 部分中选中 document-literal 选项并单击 Finish。

 
##### 图 8. 绑定选项

 ![绑定选项](https://www.ibm.com/developerworks/cn/opensource/os-eclipse-bpel2.0/helloworld-bindingoptions.jpg)创建了绑定后，右键单击 WSDL 编辑器中的空白区域并选择 Add Service 菜单项来创建一个名为  `HelloWorldService`  的新绑定。然后把HelloWorldPort 指定为绑定的端口名称并把 URL [http://localhost:8080/ode/processes/HelloWorld](http://localhost:8080/ode/processes/HelloWorld) 指定为绑定的地址。同时选择HelloWorldBinding 作为新建服务的绑定。

 
##### 图 9. HelloWorldBinding

 ![HelloWorldBinding](https://www.ibm.com/developerworks/cn/opensource/os-eclipse-bpel2.0/helloworld-wsdl.jpg)WSDL 文件已经准备好被部署到 ODE 应用程序中。

 
### 部署描述符

 在准备好部署流程之前需要做的最后一件事是创建 ODE 描述符。描述符必须名为 deploy.xml 且必须放在存储 BPEL 和 WSDL 文件的目录中。只需创建一个新的 deploy.xml 文本文件并把以下内容放置在该文件中。

 
##### 清单 1. 创建 deploy.xml 文本文件

 ```
>?xml version="1.0" encoding="UTF-8"?> >deploy xmlns="http://ode.fivesight.com/schemas/2006/06/27/dd" xmlns:pns="http://www.ibm.com/wd2/ode/HelloWorld" xmlns:wns="http://www.ibm.com/wd2/ode/HelloWorld"> >process name="pns:HelloWorld"> >active>true>/active> >provide partnerLink="client"> >service name="wns:HelloWorldService" port="HelloWorldPort"/> >/provide> >/process> >/deploy>
```
   
 描述符将为可部署单元指定流程和服务列表。

   
 [回页首](https://www.ibm.com/developerworks/cn/opensource/os-eclipse-bpel2.0/#ibm-pcon)

 
## 使用 BPEL 流程

 现在，当 HelloWorld BPEL 流程就绪时，我们可以把它部署到 ODE 应用程序中并测试该流程。

 
### 把 BPEL 流程部署到 ODE 中

 ODE 支持热部署 BPEL 流程。要部署在先前部分中创建的流程，只需把包含流程的所有文件的文件夹复制到部署了 ODE 的 Apache Tomcat 的 webapps/ode/WEB-INF/processes 目录中。要监视部署流程，可以查看 Tomcat 根目录中的日志文件 logs/catalina.out，查找所有新输入。

 测试已部署流程的一种简单方法是使用 Eclipse Web Services Explorer 工具。右键单击 HelloWorld WSDL 文件并选择 Web Services > Test with Web Services Explorer 弹出式菜单项。Eclipse 将启动 Web Services Explorer 测试工具。使用此工具，把一些文本输入到输入参数中并调用 Process 操作。如图 10 所示，Web 服务将向您刚输入的文本返回问候语。

 
##### 图 10. 调用 WSDL 操作

 ![调用 WSDL 操作](https://www.ibm.com/developerworks/cn/opensource/os-eclipse-bpel2.0/process-test.jpg)
### 创建 BPEL 流程客户机

 在拥有了执行 BPEL 流程的 Web 服务后，我们可以把此流程集成到客户机应用程序中。让我们使用 Eclipse WTP 插件来生成基于 Axis 的 Web 服务客户机。在生成客户机代码之前，需要把 Tomcat 服务器添加到 Eclipse 服务器的列表中。单击 File > New > Other 菜单项并从列表中选择 Server 选项。然后，执行向导步骤来为 Eclipse 工作空间创建新 Tomcat 服务器。

 在创建了服务器后，右键单击 WSDL 文件并选择 Web Services > Generate Client 弹出式菜单项。此操作将启动 Web Service Client 向导，该向导将生成一个新项目，提供了处理 Web 服务所需的类。

 
##### 图 11. 设置 Web 服务客户机

 ![设置 Web 服务客户机](https://www.ibm.com/developerworks/cn/opensource/os-eclipse-bpel2.0/webclient-wizard.jpg)要使用生成的文件，请使用类似如下所示的代码：

 
##### 清单 2. 创建 Web 服务客户机

 ```
HelloWorldServiceLocator locator = new HelloWorldServiceLocator(); HelloWorldRequest hwRequest = new HelloWorldRequest(); hwRequest.setInput("developerWorks!"); HelloWorldResponse hwResponse = locator.getHelloWorldPort().process(hwRequest); String output = hwResponse.getResult();
```
   
 在执行了代码后，输出变量应当包含 BPEL 流程工作的结果。

   
 [回页首](https://www.ibm.com/developerworks/cn/opensource/os-eclipse-bpel2.0/#ibm-pcon)

 
## ODE 管理 API

 ODE 通过 Web 服务提供对一些应用程序管理功能的访问。通过使用 ODE，您可以控制部署到 ODE 中的流程及其实例，这些实例目前是在服务器中执行的。所有操作都是在位于 Tomcat 应用程序的文件夹 webapps/ode/WEB-INF 中的 pmapi.wsdl 文件中描述的。不幸的是，pmapi.wsdl 将使用旧 RPC 文档样式，并且，很难通过 Eclipse Web Services Explorer 测试工具使用它。

 访问管理 API 的最佳方式是使用 Axis2 库。特别是使用 ServiceClientUtil 类，该类是由 ode-axis2 库提供的。Axis2 将依赖于其他库，包括 Xerces、Stax 等。ode.war 归档中附带了大多数库，因此可以将其添加到项目依赖关系中。

 以下代码将演示如何提取当前流程实例的信息。

 
##### 清单 3. 提取当前流程实例的信息

 ```
ServiceClientUtil client = new ServiceClientUtil(); OMElement msg = client. buildMessage("listAllInstances", new String[] {}, new String[] {}); OMElement result = client.send(msg, "http://localhost:8080/ode/processes/InstanceManagement"); List>ProcessInfo> processes = new ArrayList>ProcessInfo>(); Iterator>OMElement> i = result.getChildElements(); while (i.hasNext()) { OMElement omInstanceInfo = i.next(); OMElement omProcessName = omInstanceInfo.getFirstChildWithName( new QName("http://www.apache.org/ode/pmapi/types/2006/08/02/", "process-name")); OMElement omStatus = omInstanceInfo.getFirstChildWithName( new QName("http://www.apache.org/ode/pmapi/types/2006/08/02/", "status")); OMElement omStarted = omInstanceInfo.getFirstChildWithName( new QName("http://www.apache.org/ode/pmapi/types/2006/08/02/", "dt-started")); OMElement omLastActive = omInstanceInfo.getFirstChildWithName( new QName("http://www.apache.org/ode/pmapi/types/2006/08/02/", "dt-last-active")); ProcessInfo process = new ProcessInfo(); process.setProcessName(omProcessName.getText()); process.setStatus(omStatus.getText()); process.setStarted(omStarted.getText()); process.setLastActive(omLastActive.getText()); processes.add(process); }
```
   
 示例将使用 axiom 库来检索信息并把它存储为  `ProcessInfo`  对象列表。可以从应用程序的任何其他部分使用这些对象。

   
 [回页首](https://www.ibm.com/developerworks/cn/opensource/os-eclipse-bpel2.0/#ibm-pcon)

 
## ODE 事件侦听程序

 ODE 允许您为 ODE 应用程序内的任何操作（例如启动和停止流程实例）开发侦听程序。要创建您自己的事件侦听程序，需要实现 ode-bpel-api.jar 库中定义的 org.apache.ode.bpel.iapi.BpelEventListener 接口。以下代码将演示一个输出到标准输出流传入事件的简单实现和  `startup()` 及  `shutdown()`  侦听程序方法的调用。

 
##### 清单 4.  `startup()`  和  `shutdown()`  侦听程序方法

 ```
/** * {@inheritDoc} */ public void onEvent(BpelEvent event) { System.out.println(event); } /** * {@inheritDoc} */ public void startup(Properties arg0) { System.out.println(this.getClass() + " startup"); } /** * {@inheritDoc} */ public void shutdown() { System.out.println(this.getClass() + " shutdown"); }
```
   
 当侦听程序类就绪后，需要把它放到 ODE 类路径中（您可以把带有侦听程序类的 JAR 文件放入 webapps/ode/WEB-INF/lib Tomcat 目录）。您还需要创建 webapps/ode/WEB-INF/conf/ode-axis2.properties 属性文件并将下列行添加到其中：

 ```
ode-axis2.event.listeners=com.ibm.wd2.bpel.eventlistener.WD2EventListener
```
   
 您的侦听程序类名称可以不同于上面的名称。

   
 