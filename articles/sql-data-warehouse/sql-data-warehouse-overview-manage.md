<!-- Remove portal -->
<properties
   pageTitle="在 Azure SQL 数据仓库中管理数据库 | Azure"
   description="管理 SQL 数据仓库数据库的概述。包括管理工具、DWU 和向外扩展性能，对查询性能进行故障排除，建立良好的安全策略，以及从数据损坏或区域中断还原数据库。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.date="06/13/2016"
   wacn.date="07/18/2016"/>

# 在 Azure SQL 数据仓库中管理数据库

SQL 数据仓库自动执行管理数据库的许多方面的操作。例如，若要缩放性能，你只需调整相应级别的计算资源并为这些资源付费，然后即可让 SQL 数据仓库执行向外扩展和缩减的所有工作。

你肯定需要监视工作负荷以确定所需性能，并对长时间运行的查询进行故障排除。你还需要执行几个安全任务来管理用户和登录名的权限。

本概述介绍管理 SQL 数据仓库的这些方面内容。

- 管理工具
- 缩放计算
- 暂停和恢复
- 性能最佳实践
- 监视查询
- “安全”
- 备份和还原

## 管理工具

可以使用多种工具来管理 SQL 数据仓库中的数据库。管理数据库时，你将为需要执行的每种类型的任务制定工具首选项。

<!-- ### Azure 门户
[Azure 门户][]是一个基于 Web 的门户，你可以从中创建、更新和删除数据库以及监视数据库资源。如果你刚开始使用 Azure、管理少量的数据仓库数据库或需要快速执行某些操作，该工具是理想之选。

若要开始使用 Azure 门户，请参阅[创建 SQL 数据仓库（Azure 门户）][]。 -->

### Visual Studio 中的 SQL Server Data Tools
使用 Visual Studio 中的 [SQL Server Data Tools][] (SSDT)，可以连接到你的数据库并对其进行管理和开发。如果你是熟悉 Visual Studio 或其他集成开发环境 (IDE) 的应用程序开发人员，请尝试使用 Visual Studio 中的 SSDT。

使用 SSDT 包含的 SQL Server 对象资源管理器，可以针对 SQL 数据仓库数据库进行可视化、连接和执行脚本。若要快速连接到 SQL 数据仓库，只需在 Azure 经典管理门户中查看数据库详细信息时，单击命令栏中的“在 Visual Studio 中打开”按钮。

若要开始使用 Visual Studio 中的 SSDT，请参阅[使用 Visual Studio 连接到 Azure SQL 数据仓库][]。

### 命令行工具
命令行工具最适合用于自动执行工作负荷。PowerShell 和 sqlcmd 是自动执行过程的两个很好方法。由于可为所需的作业编写脚本并自动执行此类作业，因此我们建议使用这些工具来管理大量的逻辑服务器，以及在生产环境中部署资源更改。

### 动态管理视图 

DMV 是管理 SQL 数据仓库的必备工具。在门户中显示的所有信息几乎都依赖于 DMV。若要查看 SQL 数据仓库 DMV 的列表，请参阅 [SQL 数据仓库系统视图][]。

若要开始，请参阅 [Connect and query with sqlcmd（使用 sqlcmd 进行连接和查询）][]和 [Create a database (PowerShell)（创建数据库 (PowerShell)）][]。

## 缩放计算

在 SQL 数据仓库中，你可以快速地进行性能缩放，只需增加或减少 CPU 计算资源、内存和 I/O 带宽即可。进行性能缩放时，只需调整 SQL 数据仓库分配给你的数据库的数据仓库单位 (DWU) 数即可。SQL 数据仓库可以快速地进行更改，并处理针对硬件或软件的所有基础更改。

若要了解有关缩放 DWU 的详细信息，请参阅[缩放性能][]。

##  暂停和恢复

为了节省成本，可以按需暂停和恢复计算资源。例如，如果你晚上和周末不使用数据库，那么可以在这些时间暂停数据库的使用，然后在白天时恢复使用。当数据库暂停时不对 DWU 进行收费。

有关详细信息，请参阅[暂停计算][]和[恢复计算][]。

## 性能最佳实践

开始使用一种新技术时，从一开始就发现最适用的提示和技巧可以节省大量时间。浏览我们的许多主题，你会发现最佳实践。

若要查看在开发工作负荷时最重要的注意事项的摘要，请参阅 [SQL 数据仓库最佳实践][]。

## 监视查询

有时查询运行时间太长，但你不能确定哪个是问题所在。SQL 数据仓库包含动态管理视图 (DMV)，可用于找出哪个查询用时过长。

若要查找长时间运行的查询，请参阅 [Monitor your workload using DMVs（使用 DMV 监视工作负荷）][]。

## “安全”

若要维护一个安全系统，必须警惕和防范任何类型的未经授权的访问。安全系统需要确保防火墙规则已到位，以便只有经过授权的 IP 地址才能连接。它需要对用户凭据进行相应身份验证。用户连接到数据库后，用户只应有权执行最小数量的操作。若要保护数据，可以使用加密。具有审核和跟踪功能也很重要，以便在有任何可疑活动时可以追溯事件。

若要了解管理安全性，请直接访问[安全性概述][]。

## 备份和还原

创建数据的可靠备份是任何生产数据库必不可少的组成部分。SQL 数据仓库可通过定期自动备份活动数据库来使数据保持安全。通过这些备份可以从损坏了数据或是意外删除了数据或数据库的情形中恢复。有关数据备份计划、保留策略以及如何还原数据库，请参阅[从快照还原][]。

## 后续步骤
使用好的数据库设计原则可更轻松地在 SQL 数据仓库中管理数据库。若要了解详细信息，请直接访问[开发概述][]。

<!--Image references-->

<!--Article references-->
[创建 SQL 数据仓库（Azure 门户）]: /documentation/articles/sql-data-warehouse-get-started-provision-powershell/
[Create a database (PowerShell)（创建数据库 (PowerShell)）]: /documentation/articles/sql-data-warehouse-get-started-provision-powershell/
[connection]: /documentation/articles/sql-data-warehouse-connect-overview/
[使用 Visual Studio 连接到 Azure SQL 数据仓库]: /documentation/articles/sql-data-warehouse-query-visual-studio/
[Connect and query with sqlcmd（使用 sqlcmd 进行连接和查询）]: /documentation/articles/sql-data-warehouse-get-started-connect-sqlcmd/
[开发概述]: /documentation/articles/sql-data-warehouse-overview-develop/
[Monitor your workload using DMVs（使用 DMV 监视工作负荷）]: /documentation/articles/sql-data-warehouse-manage-monitor/
[暂停计算]: /documentation/articles/sql-data-warehouse-manage-compute-overview/#pause-compute-bk/
[从快照还原]: /documentation/articles/sql-data-warehouse-restore-database-overview/
[恢复计算]: /documentation/articles/sql-data-warehouse-manage-compute-overview/#resume-compute-performance-bk/
[缩放性能]: /documentation/articles/sql-data-warehouse-manage-compute-overview/#scale-performance-bk/
[安全性概述]: /documentation/articles/sql-data-warehouse-overview-manage-security/
[SQL 数据仓库最佳实践]: /documentation/articles/sql-data-warehouse-best-practices/
[SQL 数据仓库系统视图]: /documentation/articles/sql-data-warehouse-reference-tsql-system-views/

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/zh-cn/library/mt204009.aspx

<!--Other web references-->
[Azure 门户]: https://manage.windowsazure.cn
<!---HONumber=Mooncake_0711_2016-->