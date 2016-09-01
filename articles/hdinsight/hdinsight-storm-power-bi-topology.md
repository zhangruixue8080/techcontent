<properties
	pageTitle="将 Apache Storm 与 Power BI 配合使用 | Azure"
	description="使用 HDInsight 中 Apache Storm 群集上运行的 C# 拓扑中的数据创建 Power BI 报表。"
	services="hdinsight"
	documentationCenter=""
	authors="Blackmist"
	manager="paulettm"
	editor="cgronlun"
	tags="azure-portal"/>

<tags
	ms.service="hdinsight"
	ms.date="05/27/2016"
	wacn.date="07/18/2016"/>

# 使用 Power BI 直观显示 Apache Storm 拓扑中的数据

Power BI 允许你以可视方式将数据显示为报表。使用适用于 Storm on HDInsight 的 Visual Studio 模板，可以轻松地将 Apache Storm on HDInsight 群集上运行的拓扑中的数据存储到 SQL Azure，然后使用 Power BI 将数据可视化。

在本文档中，你将学习如何使用 Power BI 基于 Apache Storm 生成的数据创建报表，然后将其存储到 Azure SQL 数据库中。

## 先决条件

- Azure 订阅。请参阅[获取 Azure 试用版](/pricing/1rmb-trial/)。

* 具有 [Power BI](https://powerbi.com) 访问权限的 Azure Active Directory 用户

* Visual Studio（以下版本之一）

    * Visual Studio 2012 [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)

    * Visual Studio 2013 [Update 4](http://www.microsoft.com/download/details.aspx?id=44921) 或 [Visual Studio 2013 Community](http://download.microsoft.com/download/7/1/B/71BA74D8-B9A0-4E6C-9159-A8335D54437E/vs_community.exe)

    * [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)

* HDInsight Tools for Visual Studio：有关安装方面的信息，请参阅 [HDInsight Tools for Visual Studio 入门](/documentation/articles/hdinsight-hadoop-visual-studio-tools-get-started/)。

## 工作原理

本示例包含一个可随机生成 Internet Information Services (IIS) 日志数据的 C# Storm 拓扑。此数据将写入 SQL 数据库，随后用于在 Power BI 中生成报表。

下面是实现本示例的主要功能的文件列表。

* **SqlAzureBolt.cs**：将 Storm 拓扑中生成的信息写入 SQL 数据库。

* **IISLogsTable.sql**：Transact-SQL 语句，用于生成数据所要存储到的数据库。

> [AZURE.WARNING] 在 HDInsight 群集上启动该拓扑之前，必须先在 SQL 数据库中创建表。

## 下载示例

下载 [HDInsight C# Storm Power BI 示例](https://github.com/Azure-Samples/hdinsight-dotnet-storm-powerbi)。若要下载该示例，请使用 [git](http://git-scm.com/) 复制/克隆它，或使用“下载”链接下载 .zip 存档。

## 创建数据库

1. 遵循 [SQL Database tutorial](/documentation/articles/sql-database-get-started/)（SQL 数据库教程）文档中的步骤创建新的 SQL 数据库。

2. 遵循 [Connect to a SQL Database with Visual Studio](/documentation/articles/sql-database-connect-query/)（使用 Visual Studio 连接到 SQL 数据库）文档中的步骤连接到该数据库。

4. 在对象资源管理器中右键单击该数据库，然后创建__新查询__。将下载的项目中包含的 __IISLogsTable.sql__ 文件的内容粘贴到查询窗口中，然后使用 Ctrl+Shift+E 执行查询。你应会收到已成功完成命令的消息。

    完成此操作后，数据库中会出现名为 __IISLOGS__ 的新表。

## 配置示例

1. 替换以下示例中“{”与“}”之间的值，以构造正确的 __ADO.NET（SQL 身份验证）__信息。

		Server=tcp:{your_dns}.database.chinacloudapi.cn,1433;Data Source={your_dns}.database.chinacloudapi.cn;Initial Catalog=j{your_dns};Persist Security Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;

1. 在 Visual Studio 中打开该示例。在“解决方案资源管理器”中打开“App.config”文件，然后找到以下条目：

        <add key="SqlAzureConnectionString" value="##TOBEFILLED##" />
    
    将 __##TOBEFILLED##__ 值替换为上一步骤中复制的数据库连接字符串。将 __{your\_username}__ 和 __{your\_password}__ 替换为数据库的用户名和密码。

2. 保存并关闭文件。

## 部署示例

1. 在“解决方案资源管理器”中，右键单击“StormToSQL”项目，然后选择“提交到 Storm on HDInsight”。从“Storm 群集”下拉对话框中选择 选择 HDInsight 群集。

    > [AZURE.NOTE] 可能需要在几秒钟后，“Storm 群集”下拉对话框中才会填充服务器名称。
    ><p> 如果出现提示，请输入你 Azure 订阅的登录凭据。如果你有多个订阅，请登录包含 Storm on HDInsight 群集的订阅。

2. 成功提交拓扑之后，应该会出现群集的“Storm 拓扑”。从列表中选择 SqlAzureWriterTopology 条目，以查看有关正在运行的拓扑的信息。

    ![拓扑列表和已选择的拓扑](./media/hdinsight-storm-power-bi-topology/topologyview.png)

    可以使用此视图查看有关拓扑的信息，或双击条目（例如 SqlAzureBolt）以查看拓扑中组件的具体信息。

3. 在拓扑运行几分钟后，请返回到用于创建数据库的 SQL 查询窗口。将现有语句替换为以下内容。

        select * from iislogs;
    
    使用 Ctrl+Shift+E 执行查询，你应会收到类似于下面的结果。
    
        1	2016-05-27 17:57:14.797	255.255.255.255	/bar	GET	200
        2	2016-05-27 17:57:14.843	127.0.0.1	/spam/eggs	POST	500
        3	2016-05-27 17:57:14.850	123.123.123.123	/eggs	DELETE	200
        4	2016-05-27 17:57:14.853	127.0.0.1	/foo	POST	404
        5	2016-05-27 17:57:14.853	10.9.8.7	/bar	GET	200
        6	2016-05-27 17:57:14.857	192.168.1.1	/spam	DELETE	200

    这是从 Storm 拓扑写入的数据。

## 创建报告

1. 连接到 Power BI 的 [Azure SQL 数据库连接器](https://app.powerbi.com/getdata/bigdata/azure-sql-database-with-live-connect)。

2. 在“数据库”中，选择“获取”。

3. 选择“Azure SQL 数据库”，然后选择“连接”。

4. 输入信息以连接到 Azure SQL 数据库。可以通过访问 [Azure 经典管理门户](https://manage.windowsazure.cn)并选择你的 SQL 数据库来找到此信息。

    > [AZURE.NOTE] 还可以通过在连接对话框中使用“启用高级选项”，来设置刷新间隔和自定义筛选器。

5. 连接后，你将看到一个与所连接到的数据库同名的新数据集。选择该数据集以开始设计报表。

3. 在“字段”中展开“IISLOGS”条目。选中“URISTEM”对应的复选框。随后将创建新报表，其中列出了数据库中记录的 URI 词干（/foo、/bar 等等）。

    ![创建报表](./media/hdinsight-storm-power-bi-topology/createreport.png)

5. 接下来，将“方法”拖到报表中。报表将会更新，以列出词干和用于 HTTP 请求的对应 HTTP 方法。

    ![添加方法数据](./media/hdinsight-storm-power-bi-topology/uristemandmethod.png)

4. 从“可视化”列中选择“字段”图标，然后选择“值”部分中“方法”旁边的向下箭头。从显示的列表中选择“计数”。随后将更改报表以列出特定 URI 的访问次数。

    ![更改为方法计数](./media/hdinsight-storm-power-bi-topology/count.png)

6. 接下来，选择“堆积柱形图”以更改信息的显示方式。

    ![更改为堆积图](./media/hdinsight-storm-power-bi-topology/stackedcolumn.png)

7. 创建符合需要的报表后，请使用菜单中的“保存”项输入名称并保存报表。

## 停止拓扑

在你停止该拓扑或者删除 Storm on HDInsight 之前，该拓扑会一直运行。执行以下步骤可停止拓扑。

1. 在 Visual Studio 中，返回到拓扑查看器并选择拓扑。

2. 选择“终止”按钮即可停止拓扑。

    ![拓扑摘要中的终止按钮](./media/hdinsight-storm-power-bi-topology/killtopology.png)

## 删除群集

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## 后续步骤

在本文档中，你已了解如何将数据从 Storm 拓扑发送到 SQL 数据库，然后使用 Power BI 可视化数据。有关如何在 Storm on HDInsight 中使用其他 Azure 技术的信息，请参阅以下文章：

* [Storm on HDInsight 的示例拓扑](/documentation/articles/hdinsight-storm-example-topology/)

<!---HONumber=Mooncake_0711_2016-->