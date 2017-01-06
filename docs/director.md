# Citrix Director SDK 准则

# 简介

Monitor Service API 在使用 Windows Communication Foundation (WCF) Data Services（在处理和合并期间填充）的 SQL Server 数据库的基础上构建。

Monitor Service API 使用开放数据协议 (OData)。此协议是一个用于查询和更新数据的 Web 协议，并基于诸如 HTTP 之类的 Web 技术而构建。

Monitor Service API 是一个基于 REST 的 API，可以使用 OData 使用者程序进行访问。OData 使用者程序是占用使用 OData 协议显示的数据的应用程序。

您可以使用 API 执行以下操作：

- 分析历史趋势以便进行未来规划

- 对连接失败和计算机故障进行细致的故障排除

- 提取信息以用于其他工具和流程；例如，可以使用 Microsoft Excel 的 PowerPivot 表以不同的方式显示数据

- 基于 API 提供的数据构建自定义用户界面

# 可使用 API 获得的数据

以下类型的数据可通过 Monitor Service API 获得：

- <span id="par_richtext_0" class="anchor"></span>与连接失败相关的数据

- 处于故障状态的计算机

- 会话使用情况

- 登录持续时间

- 负载平衡数据

- 应用于计算机的修补程序

- 托管应用程序的使用情况

有关 Monitor Service 架构的详细信息，请参阅 [Monitor Service 架构](http://docs.citrix.com/en-us/xenapp-and-xendesktop/7-5/cds-ms-odata-wrapper/cds-ms-odata-data.html)。

要确定 Monitor Service OData API 返回的值，请参阅 [Citrix Monitor 数据模型](http://support.citrix.com/help/monitorserviceapi/7.6/html/d787ce1d-02c0-6165-fc99-c46592a5d112.htm)

# 如何访问 Monitor Service 数据

## 数据访问权限

要使用 Monitor Service OData API，您必须是 XenApp 或 XenDesktop 管理员。 要调用 API，您需要具有只读权限；但是，返回的数据由 XenApp 或 XenDesktop 管理员角色和权限决定。

例如，交付组管理员可以调用 Monitor Service API，但他们可以获取的数据由使用 Citrix Studio 设置的交付组访问权限控制。

有关 XenApp 或 XenDesktop 管理员角色和权限的详细信息，请参阅[委派管理](http://docs.citrix.com/en-us/xenapp-and-xendesktop/7-6/xad-security-article/xad-delegated-admin.html)。

## 数据访问安全性

如果选择使用 SSL，则必须在站点内的所有 Delivery Controller 上配置 SSL。不能同时使用 SSL 和非 SSL。

要使用 SSL 为 Monitor Service 端点提供安全保护，必须执行以下配置。 某些步骤需要在每个站点一次性完成，其余步骤则必须在站点中托管 Monitor Service 的每台计算机上执行。 各步骤的说明如下所述。

### <span id="par_richtext_1" class="anchor"><span id="_Toc464843969" class="anchor"></span></span>第 1 部分：在系统中注册证书

  1. <span id="par_richtext_2" class="anchor"></span>使用可信证书管理器创建证书。证书必须与您希望用于 OData SSL 的计算机上的端口相关联。

  2. 配置 Monitor Service，以使用此端口进行 SSL 通信。操作步骤取决于您的环境及其与证书结合使用的方式。以下示例说明了如何配置端口 449：

- <span id="par_richtext_3" class="anchor"></span>将证书与某个端口相关联：

> netsh http add sslcert ipport=0.0.0.0:449 certhash=97bb629e50d556c80528f4991721ad4f28fb74e9
> 
> appid='{00000000-0000-0000-0000-000000000000}'

**提示**：*在 PowerShell 命令窗口中，请务必在 appID 中的 GUID 两边加单引号（如上文所示），否则命令将不起作用。 请注意，在此示例中添加了换行符，这只是为了方便阅读。*

### <span id="par_richtext_4" class="anchor"><span id="_Toc464843970" class="anchor"></span></span>第 2 部分：修改 Monitor Service 的配置设置

  1. <span id="par_richtext_5" class="anchor"></span>在站点中的任意 Delivery Controller 上，运行一次以下 PowerShell 命令。此命令删除 Monitor Service 在 Configuration Service 中的注册。

> asnp citrix.\*
> 
> \$serviceGroup = get-configregisteredserviceinstance -servicetype Monitor | Select -First 1 ServiceGroupUid
> 
> remove-configserviceGroup -ServiceGroupUid \$serviceGroup.ServiceGroupUid

  1. 在站点中的所有 Controller 上执行以下操作：

- 使用 cmd 提示符查找已安装的 Citrix Monitor 目录（通常位于 C:\\Program Files\\Citrix\\Monitor\\Service 中）。在该目录下，运行：

> Citrix.Monitor.Exe -CONFIGUREFIREWALL -ODataPort 449 -RequireODataSsl

- 运行以下 PowerShell 命令：

> asnp citrix.\*（如果尚未在此窗口中运行）
> 
> get-MonitorServiceInstance | register-ConfigServiceInstance
> 
> Get-ConfigRegisteredServiceInstance -ServiceType Config | Reset-MonitorServiceGroupMembership

## 数据访问协议

Monitor Service API 是一个基于 REST 的 API，可以使用 OData 使用者程序进行访问。 OData 使用者程序是占用使用 OData 协议显示的数据的应用程序。 从简单的 Web 浏览器到可利用 OData 协议的所有功能的自定义应用程序，OData 使用者程序的复杂程度各不相同。

Monitor Service 数据模型的每一部分都可访问，并且可以根据 URL 进行过滤。OData 以 URL 格式提供查询语言，可用于从服务检索条目。

查询在服务器端进行处理，并且可以使用客户端上的 OData 协议进行进一步过滤。<span id="par_richtext_7" class="anchor"></span>建模数据分为以下三类：聚合数据（汇总表）、对象（计算机和会话等）的当前状态以及日志数据（实际上是指历史事件，例如连接）。<span id="par_richtext_8" class="anchor"></span>**注意**：OData 协议不支持枚举；在其位置使用整数。 要确定 Monitor Service OData API 返回的值，请参阅** **[Monitor Service 数据模型](http://support.citrix.com/help/monitorserviceapi/7.6/)。

### OData 协议是什么？

[OData](http://www.odata.org/)（开放数据协议）是一种 [OASIS 标准](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=odata)，用于定义构建和使用 RESTful API 的一组最佳做法。 OData 可帮助您在构建 RESTful API 的过程中重点关注业务逻辑，不必担心用于定义请求和响应头、状态代码、HTTP 方法、URL 约定、媒体类型、有效负载格式及查询选项等各种方法。

OData 还提供跟踪变更、定义可重用过程的功能/操作以及发送异步/批处理请求的指导。

OData RESTful API 易于使用。通过 OData 元数据（即，API 数据模型的计算机可读描述），可以创建功能强大的通用客户端代理和工具。

OData 使用者程序：<http://www.odata.org/ecosystem/#consumers>

客户端库：<http://www.odata.org/libraries/>

基础教程：<http://www.odata.org/getting-started/basic-tutorial/>

### 

### WCF 是什么？

OData 提供程序 Citrix Monitor Service API 是通过 WCF 实现的。

Windows Communication Foundation (WCF) 是 Microsoft 用于构建面向服务的应用程序的统一编程模型。

有关 WCF 的更多详细信息，请参阅 [Windows Communication Foundation](https://msdn.microsoft.com/library/dd456779.aspx)

## 示例

以下示例说明如何使用 OData API 导出 Monitor Service 数据。本主题还提供可用数据集的 URL 列表。

### 示例 1 - 原始 XML

  1. 将每个数据集的 URL 置于正在使用 XenApp 或 XenDesktop 站点的相应管理权限运行的 Web 浏览器中。 Citrix 建议使用安装了 Advanced Rest Client 加载项的 Chrome 浏览器。

  2. 查看来源。

### 示例 2 - 在 Excel 中使用 PowerPivot

  1. 安装 Microsoft Excel。

  2. 按照此处的说明安装 PowerPivot（具体取决于您使用的是 2010 版还是 2013 版）：<https://support.office.com/en-us/article/Start-Power-Pivot-in-Microsoft-Excel-2013-add-in-a891a66d-36e3-43fc-81e8-fc4798f39ea8>。

  3. 打开 Excel（使用 XenApp 或 XenDesktop 站点的相应管理权限运行）。

#### 使用 Excel 2010

  1. 单击“PowerPivot”选项卡。

  2. 单击 PowerPivot 窗口。

  3. 单击功能区中的**从数据馈送**。

  4. 选择友好的连接名称（例如：XenDesktop Monitoring Data）并输入数据馈送 URL：http://{dc-host}/Citrix/Monitor/OData/v1/Data（如果使用的是 SSL，请输入 https:）。

  5. 单击**下一步**。

  6. 选择要导入到 Excel 的表并单击**完成**。这样便可检索到数据。

#### 使用 Excel 2013

  1. 单击“数据”选项卡。

  2. 依次选择“自其他来源”>“来自 OData 数据馈送”

  3. 输入数据馈送 URL：http://{dc-host}/Citrix/Monitor/OData/v1/Data（如果使用的是 SSL，请输入 https:）并单击**下一步**。

  4. 选择要导入到 Excel 的表并单击**下一步**。

  5. 接受默认名称或自定义名称，然后单击**完成**。

  6. 选择**仅连接**或**透视报表**。这样便可检索到数据。

现在，可以使用 PowerPivot 通过数据透视表和数据透视图查看和分析数据。 有关详细信息，请参阅 Learning Center：<http://www.microsoft.com/en-us/bi/LearningCenter.aspx>

### <span id="par_richtext_9" class="anchor"><span id="_Toc464843977" class="anchor"></span></span>示例 3 - LinqPad

  1. <span id="par_richtext_10" class="anchor"></span>从 [http://www.linqpad.net](http://www.linqpad.net/) 下载并安装最新版本的 LinqPad。

  2. 使用 XenApp 或 XenDesktop 站点的相应管理权限运行 LinqPad。

> 提示：最简便的方法是在 Delivery Controller 上下载、安装并运行。

  1. 单击“添加连接”链接。

  2. 选择 WCF Data Services 5.1 (OData 3) 并单击**下一步**。

  3. 输入数据馈送 URL：<span id="OLE_LINK2" class="anchor"><span
id="OLE_LINK3"
class="anchor"></span></span>http://{dc-host}/Citrix/Monitor/OData/v1/Data（如果使用的是 SSL，请输入 https:）。 如有必要，请输入用户名和密码以访问 Delivery Controller。 单击**确定**。

  4. 现在可以针对数据馈送运行 LINQ 查询，并根据需要导出数据。 例如，在“目录”上单击鼠标右键并选择 **Catalogs.Take(100)**。 此操作将返回数据库中前 100 个目录。 选择“导出”>“导出到具有格式设置的 Excel”。

### 示例 4 – 客户端库

#### 

Citrix Monitor Service 当前支持 OData 协议 V1-V3。因此，在各种编程平台中实现 OData 使用者程序时，请选择正确的客户端库。

#### 示例 4.1 – C\#/.NET

##### 从 .NET 客户端调用 OData 服务 (C\#)

<https://www.asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client>

代码片段： <br />

        /* GET http://{dc-host}/Citrix/Monitor/Odata/v3/Data/Catalogs */
        private static string ListAllCatalgs(MonitorService.DatabaseContext context)
        {
            StringBuilder sb = new StringBuilder();
            Foreach (var c in context.Catalogs)
            {
                sb.Append(DisplayCatalog(c));
            }
            return sb.ToString();
        }
    

<br />

        /* GET http://Url/Machines()?$select=Name, IPAddress */
        private static void ListMachineNames(MonitorService.DatabaseContext context)
        {
            var machines = from m in context.Machines select new { Name = m.Name, IP = m.IPAddress };
            foreach (var m in machines)
            {
                if (m.Name != null && m.IP != null)
                {
                    Console.WriteLine("{0} : {1}", m.Name, m.IP);
                }
            }
        }
    

<br />

        /* use the LINQ Skip and Take methods to skips the first 40 results and takes the next 10 */
        /* GET http://{dc-host}/Citrix/Monitor/Odata/v3/Machines()?$orderby=Id desc&$skip=40&$top=10 */
        private static void ListMachinesPaged(MonitorService.DatabaseContext context)
        {
            var machines =
                (from m in context.Machines
                 orderby m.Id descending
                 select m).Skip(40).Take(10);
    
            foreach (var m in machines)
            {
                Console.WriteLine("{0}, {1}", m.Name, m.IPAddress);
            }
        }
    
    

<br />

        /* GET http://Url/Catalogs()?$filter=Name eq '$Name'*/
        private static void ListCatalogByName(MonitorService.DatabaseContext context, string name)
        {
            var catalog = context.Catalogs.Where(c => c.Name == name).SingleOrDefault();
            if (catalog != null)
            {
                DisplayCatalog(catalog);
            }
        }
    

<br />

#### 示例 4.2 – JavaScript

TODO

#### 示例 4.3 – Java

TODO

#### 示例 4.4 – Object-C<span id="OLE_LINK1" class="anchor"></span>TODO

# 附录：

### 可用数据集的 URL<table align=left border=0 cellpadding=0 cellspacing=0 class=MsoNormalTable style=width:509.3pt;border-collapse:collapse;margin-left:6.75pt;margin-right:6.75pt summary="">

<td style="width:350.6pt;border:solid #fff 1pt;background:#D7D7D7;padding:4.5pt 4.5pt 4.5pt 4.5pt">
  <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>URL
  
  <td style="width:158.7pt;border:solid #fff 1pt;border-left:none;background:#D7D7D7;padding:4.5pt 4.5pt 4.5pt 4.5pt">
    <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>说明<tr>
      <td style="width:350.6pt;border:solid #fff 1pt;border-top:none;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
        <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>http://{dc-host}/Citrix/Monitor/OData/v1/Data/Catalogs
        
        <td style="width:158.7pt;border-top:none;border-left:none;border-bottom:solid #fff 1pt;border-right:solid #fff 1pt;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
          <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>站点中的目录映像
          
          <tr>
            <td style="width:350.6pt;border:solid #fff 1pt;border-top:none;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
              <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>http://{dc-host}/Citrix/Monitor/OData/v1/Data/ConnectionFailureCategories
              
              <td style="width:158.7pt;border-top:none;border-left:none;border-bottom:solid #fff 1pt;border-right:solid #fff 1pt;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>连接失败类型分组
                
                <tr>
                  <td style="width:350.6pt;border:solid #fff 1pt;border-top:none;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                    <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>http://{dc-host}/Citrix/Monitor/OData/v1/Data/ConnectionFailureLogs
                    
                    <td style="width:158.7pt;border-top:none;border-left:none;border-bottom:solid #fff 1pt;border-right:solid #fff 1pt;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                      <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>站点中每个连接失败的日志
                      
                      <tr>
                        <td style="width:350.6pt;border:solid #fff 1pt;border-top:none;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                          <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>http://{dc-host}/Citrix/Monitor/OData/v1/Data/Connections
                          
                          <td style="width:158.7pt;border-top:none;border-left:none;border-bottom:solid #fff 1pt;border-right:solid #fff 1pt;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                            <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>表示会话的初始连接或重新连接
                            
                            <tr>
                              <td style="width:350.6pt;border:solid #fff 1pt;border-top:none;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>http://{dc-host}/Citrix/Monitor/OData/v1/Data/DesktopGroups
                                
                                <td style="width:158.7pt;border-top:none;border-left:none;border-bottom:solid #fff 1pt;border-right:solid #fff 1pt;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                  <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>站点中的交付组
                                  
                                  <tr>
                                    <td style="width:350.6pt;border:solid #fff 1pt;border-top:none;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                      <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>http://{dc-host}/Citrix/Monitor/OData/v1/Data/FailureLogSummaries
                                      
                                      <td style="width:158.7pt;border-top:none;border-left:none;border-bottom:solid #fff 1pt;border-right:solid #fff 1pt;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                        <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>按时间段和交付组记录的失败/故障（连接/计算机）计数
                                        
                                        <tr>
                                          <td style="width:350.6pt;border:solid #fff 1pt;border-top:none;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                            <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>http://{dc-host}/Citrix/Monitor/OData/v1/Data/Hypervisors
                                            
                                            <td style="width:158.7pt;border-top:none;border-left:none;border-bottom:solid #fff 1pt;border-right:solid #fff 1pt;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                              <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>站点中的主机（虚拟机管理程序）
                                              
                                              <tr>
                                                <td style="width:350.6pt;border:solid #fff 1pt;border-top:none;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                                  <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>http://{dc-host}/Citrix/Monitor/OData/v1/Data/LoadIndexes
                                                  
                                                  <td style="width:158.7pt;border-top:none;border-left:none;border-bottom:solid #fff 1pt;border-right:solid #fff 1pt;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                                    <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>从 Virtual Delivery Agent (VDA) 接收的负载指数数据
                                                    
                                                    <tr>
                                                      <td style="width:350.6pt;border:solid #fff 1pt;border-top:none;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                                        <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>http://{dc-host}/Citrix/Monitor/OData/v1/Data/LoadIndexSummaries
                                                        
                                                        <td style="width:158.7pt;border-top:none;border-left:none;border-bottom:solid #fff 1pt;border-right:solid #fff 1pt;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                                          <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>按时间段和计算机记录的负载指数平均值
                                                          
                                                          <tr>
                                                            <td style="width:350.6pt;border:solid #fff 1pt;border-top:none;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                                              <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>http://{dc-host}/Citrix/Monitor/OData/v1/Data/MachineFailureLogs
                                                              
                                                              <td style="width:158.7pt;border-top:none;border-left:none;border-bottom:solid #fff 1pt;border-right:solid #fff 1pt;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                                                <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>站点中按开始日期和结束日期记录的每个计算机故障的日志
                                                                
                                                                <tr>
                                                                  <td style="width:350.6pt;border:solid #fff 1pt;border-top:none;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                                                    <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>http://{dc-host}/Citrix/Monitor/OData/v1/Data/Machines
                                                                    
                                                                    <td style="width:158.7pt;border-top:none;border-left:none;border-bottom:solid #fff 1pt;border-right:solid #fff 1pt;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                                                      <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>站点中的计算机
                                                                      
                                                                      <tr>
                                                                        <td style="width:350.6pt;border:solid #fff 1pt;border-top:none;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                                                          <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>http://{dc-host}/Citrix/Monitor/OData/v1/Data/SessionActivitySummaries
                                                                          
                                                                          <td style="width:158.7pt;border-top:none;border-left:none;border-bottom:solid #fff 1pt;border-right:solid #fff 1pt;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                                                            <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>按时间段和交付组记录的会话计数和登录数据
                                                                            
                                                                            <tr>
                                                                              <td style="width:350.6pt;border:solid #fff 1pt;border-top:none;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                                                                <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>http://{dc-host}/Citrix/Monitor/OData/v1/Data/Sessions
                                                                                
                                                                                <td style="width:158.7pt;border-top:none;border-left:none;border-bottom:solid #fff 1pt;border-right:solid #fff 1pt;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                                                                  <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>表示连接到桌面的用户
                                                                                  
                                                                                  <tr>
                                                                                    <td style="width:350.6pt;border:solid #fff 1pt;border-top:none;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                                                                      <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>http://{dc-host}/Citrix/Monitor/OData/v1/Data/TaskLogs
                                                                                      
                                                                                      <td style="width:158.7pt;border-top:none;border-left:none;border-bottom:solid #fff 1pt;border-right:solid #fff 1pt;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                                                                        <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>已作为内部 Monitoring Service 的一部分运行的所有任务及其状态的日志
                                                                                        
                                                                                        <tr>
                                                                                          <td style="width:350.6pt;border:solid #fff 1pt;border-top:none;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                                                                            <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>http://{dc-host}/Citrix/Monitor/OData/v1/Data/Users
                                                                                            
                                                                                            <td style="width:158.7pt;border-top:none;border-left:none;border-bottom:solid #fff 1pt;border-right:solid #fff 1pt;background:#EEE;padding:4.5pt 4.5pt 4.5pt 4.5pt">
                                                                                              <p class=MsoNormal style=margin-top:0;margin-right:-3.75pt;margin-bottom:0;margin-left:0;margin-bottom:.0001pt;line-height:normal>已在站点中启动会话的用户</table>