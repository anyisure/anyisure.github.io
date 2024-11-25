---
title: IOT开发
tags:
  - IoT
  - 物联网
---

# OPC
> OPC（OLE for Process Control）是一种通信协议，主要用于工业自动化领域，允许不同的应用程序和设备之间进行数据交换。OPC协议基于微软的OLE（Object Linking and Embedding）技术，提供了一种标准的数据访问接口，使得不同的设备和应用程序可以通过这个接口进行通信，而不需要关心底层的通信协议。

## OPC服务器
> 目前支持OPC服务器的组态软件有很多种，其中四种软件即：Intellution公司的`iFIX(3.5)`、GE公司的`Cimplicity(6.0)`、Wonderware公司的`InTouch(9.5)`以及Siemens公司的`WinCC(6.0)`应用最广、功能最强。Intellution公司和Wonderware公司是专门从事监控软件工作的，在市场占领绝大部分份额；Cimplicity和WinCC是GE和Siemens公司自动化产品的配套产品。

### iFlX
> 支持双向OPC支持所有类型的`ActiveX`、`OLE`，对不健全的控件所引发的错误进行保护，对控件的属性操作完全控制。有全面解决扩展点的报警、报警记录、历史记录的方法，有查找替换功能，可以替换整个图画以及画面中的对象的属性、组态点信息，对于同类型物体，避免重复组态。内嵌VBA，具有自己的内部函数，又有广泛的VB函数，功能扩展更为有利。编辑与运行是切换进行的，这有利于对现场生产安全的保障；有独立的报警监视程序，支持在线修改，具有画面分层功能，运行时可以根据程序很方便地更换对象的连接数据源，可以使控制更灵活。支持`Oracle、SQL Server 2000、Access`等关系型数据库。
### Cimplicity
> 支持OPC服务器，编辑与运行分开，有独立的报警、历史趋势运行管理程序，`内嵌VBA`，具有自己的内部函数，又有广泛的VB函数，组VBA与通用运行方式不一样，支持`ActiveX、OLE插入`，但对控件其中的一些属性进行了锁定。点的扩展功能与iFIX一样强大，但对于扩展点的报警设定比较难解决，输出问题，历史记录是没问题的。支持Oracle，SQLServer 2000，Access关系型数据库。
### InTouch：
> 提供双向OPC支持，支持`ActiveX`控件，但不具有第三方控件的出错保护，不健全的控件会造成系统出错。采用有限的内部函数，其功能也只是常用监控的功能，复杂一点的功能如报表就只能借助于其他工具。支持关系型数据库。
### WinCC
> 双向OPC支持，支持`ActiveX`。使用内部语言，环境如同C语言。同样使得其功能扩展变得容易。最新的WinCC 6.0只支持连接`SQL2000`数据库。

## OPC DA

## OPC UA调试工具
### Prosys OPC UA Simulation Server
Prosys OPC 

[OPC UA Simulation Server 下载地址](https://downloads.prosysopc.com/opc-ua-simulation-server-downloads.php)

[OPC UA Simulation Server 使用手册](https://downloads.prosysopc.com/opcua/apps/JavaServer/dist/5.4.6-148/Prosys_OPC_UA_Simulation_Server_UserManual.pdf)

[Prosys OPC UA Browser 下载地址](https://downloads.prosysopc.com/opc-ua-browser-downloads.php)

[Prosys OPC UA Browser 用户手册](https://downloads.prosysopc.com/opcua/apps/UaBrowser/dist/4.4.0-126/Prosys_OPC_UA_Browser_UserManual.pdf)

### MatrikonOPCExplorer
> MatrikonOPCExplorer是一个专门为OPC开发设计的工具，它集成了多种功能，包括数据浏览、实时监控、数据记录等。通过这个工具，开发者可以轻松地与OPC服务器进行交互，查看和修改数据，从而快速定位和解决问题。


### Kepware KEPServerEX
> KEPServerEX是一款通用的OPC服务器软件，它可以与各类设备和控制器进行通信，并提供OPC DA、OPC UA、OPC XML-DA等接口。KEPServerEX提供了一个内置的客户端，用于测试和调试OPC服务器。

[下载地址](https://www.ptc.com/cn/products/kepware/kepserverex/demo-download/thank-you)

### Softing OPC Toolbox
>Softing OPC Toolbox提供了一套强大的工具，用于开发、测试和运行OPC相关应用程序。它包含了一个用于测试和监视OPC服务器的工具，可以向服务器发送读写命令，并实时显示服务器的响应。

### OPC Foundation Sample Client
> OPC Foundation提供了一个官方的Sample Client，用于测试OPC服务器。这个工具基于OPC标准，可以连接到任何符合OPC规范的服务器，并进行通信和读写操作。

### Unified Automation UaExpert
> UaExpert是一个功能丰富的OPC UA客户端，可用于测试和调试OPC UA服务器。它可以连接到OPC UA服务器，读取和修改OPC UA节点的值，查看服务器的状态和性能，并提供了丰富的监控和调试功能。

