<properties
	pageTitle="有关 SQL 数据仓库服务的所有主题 | Azure"
	description="位于 http://azure.cn/documentation/articles/ 的有关 Azure SQL 数据仓库的所有主题的表格，包括标题和描述。"
	services="sql-data-warehouse"
	documentationCenter=""
	authors="barbkess"
	manager="jhubbard"
	editor=""/>

<tags
	ms.service="sql-data-warehouse"
	ms.date="07/17/2016"
	wacn.date="08/29/2016"/>


# 有关 Azure SQL 数据仓库服务的所有主题

本主题列出的每个主题都可以直接应用于 Azure **SQL 数据仓库**服务。您可以使用 **Ctrl+F** 来搜索此网页的关键字，以便查找当前感兴趣的主题。




## 概述

| &nbsp; | Url-Id | 标题 | 说明 |
| --: | :-- | :-- | :-- |
| 1 | overview-what-is | [什么是 Azure SQL 数据仓库？](/documentation/articles/sql-data-warehouse-overview-what-is/) | 企业级分布式数据库，可处理 PB 量级的关系数据和非关系数据。它是行业首个云数据仓库，可以在数秒内增长、收缩和暂停。 |
| 2 | overview-workload | [数据仓库工作负荷](/documentation/articles/sql-data-warehouse-overview-workload/) | 借助 SQL 数据仓库的弹性，你可以使用数据仓库单位 (DWU) 的可调缩放性扩大、收缩或暂停计算容量。本文介绍数据仓库指标以及它们如何与 DWU 相关。 |



## 入门

| &nbsp; | Url-Id | 标题 | 说明 |
| --: | :-- | :-- | :-- |
| 3 | authentication | [对 Azure SQL 数据仓库进行身份验证](/documentation/articles/sql-data-warehouse-authentication/) | 对 Azure SQL 数据仓库进行的 Azure Active Directory (AAD) 和 SQL Server 身份验证。 |
| 4 | best-practices | [Azure SQL 数据仓库最佳实践](/documentation/articles/sql-data-warehouse-best-practices/) | 开发 Azure SQL 数据仓库解决方案时应了解的建议和最佳实践。这些内容可帮助你取得成功。 |
| 5 | connection-strings | [Azure SQL 数据仓库的驱动程序](/documentation/articles/sql-data-warehouse-connection-strings/) | SQL 数据仓库的连接字符串和驱动程序 |
| 6 | connect-overview | [连接到 Azure SQL 数据仓库](/documentation/articles/sql-data-warehouse-connect-overview/) | 连接到 Azure SQL 数据仓库的连接概述 |
| 7 | get-started-connect-sqlcmd | [查询 Azure SQL 数据仓库 (sqlcmd)](/documentation/articles/sql-data-warehouse-get-started-connect-sqlcmd/) | 使用 sqlcmd 命令行实用工具查询 Azure SQL 数据仓库。 |
| 8 | get-started-create-database-tsql | [使用 Transact-SQL (TSQL) 创建 SQL 数据仓库数据库](/documentation/articles/sql-data-warehouse-get-started-create-database-tsql/) | 了解如何使用 TSQL 创建 Azure SQL 数据仓库 |
| 9 | get-started-load-with-polybase | [在 SQL 数据仓库中使用 PolyBase 加载数据](/documentation/articles/sql-data-warehouse-get-started-load-with-polybase/) | 了解什么是 PolyBase，以及如何将其用于数据仓库方案。 |
| 10 | get-started-provision-powershell | [使用 PowerShell 创建 SQL 数据仓库](/documentation/articles/sql-data-warehouse-get-started-provision-powershell/) | 使用 PowerShell 创建 SQL 数据仓库 |
| 11 | query-visual-studio | [查询 Azure SQL 数据仓库 (Visual Studio)](/documentation/articles/sql-data-warehouse-query-visual-studio/) | 使用 Visual Studio 查询 SQL 数据仓库。 |



## 开发

| &nbsp; | Url-Id | 标题 | 说明 |
| --: | :-- | :-- | :-- |
| 12 | develop-best-practices-transactions | [优化 SQL 数据仓库的事务](/documentation/articles/sql-data-warehouse-develop-best-practices-transactions/) | 在 Azure SQL 数据仓库中编写有效事务更新的最佳做法指南 |
| 13 | develop-concurrency | [SQL 数据仓库中的并发性和工作负荷管理](/documentation/articles/sql-data-warehouse-develop-concurrency/) | 在开发解决方案之前，了解 SQL 数据仓库中的并发性和工作负荷管理。 |
| 14 | develop-ctas | [SQL 数据仓库中的 Create Table As Select (CTAS)](/documentation/articles/sql-data-warehouse-develop-ctas/) | 有关在开发解决方案时使用 Azure SQL 数据仓库中的 create table as select (CTAS) 语句编写代码的技巧。 |
| 15 | develop-dynamic-sql | [SQL 数据仓库中的动态 SQL](sql-data-warehouse-develop-dynamic-sql/) | 有关在开发解决方案时使用 Azure SQL 数据仓库中的动态 SQL 的技巧。 |
| 16 | develop-group-by-options | [SQL 数据仓库中的 Group By 选项](/documentation/articles/sql-data-warehouse-develop-group-by-options/) | 有关在开发解决方案时实现 Azure SQL 数据仓库中的 Group By 选项的技巧。 |
| 17 | develop-label | [在 SQL 数据仓库中使用标签检测查询](/documentation/articles/sql-data-warehouse-develop-label/) | 有关在开发解决方案时于 Azure SQL 数据仓库中使用标签检测查询的技巧。 |
| 18 | develop-loops | [SQL 数据仓库中的循环](sql-data-warehouse-develop-loops/) | 有关在开发解决方案时使用 Azure SQL 数据仓库中的 Transact-SQL 循环和替换游标的技巧。 |
| 19 | develop-stored-procedures | [SQL 数据仓库中的存储过程](/documentation/articles/sql-data-warehouse-develop-stored-procedures/) | 有关在开发解决方案时实现 Azure SQL 数据仓库中的存储过程的技巧。 |
| 20 | develop-transactions | [SQL 数据仓库中的事务](sql-data-warehouse-develop-transactions/) | 有关在开发解决方案时实现 Azure SQL 数据仓库中的事务的技巧。 |
| 21 | develop-user-defined-schemas | [SQL 数据仓库中用户定义的架构](/documentation/articles/sql-data-warehouse-develop-user-defined-schemas/) | 有关在开发解决方案时使用 Azure SQL 数据仓库中的 Transact-SQL 架构的技巧。 |
| 22 | develop-variable-assignment | [在 SQL 数据仓库中分配变量](/documentation/articles/sql-data-warehouse-develop-variable-assignment/) | 有关在开发解决方案时于 Azure SQL 数据仓库中分配 Transact-SQL 变量的技巧。 |
| 23 | develop-views | [SQL 数据仓库中的视图](sql-data-warehouse-develop-views/) | 有关在开发解决方案时使用 Azure SQL 数据仓库中的 Transact-SQL 视图的技巧。 |
| 24 | overview-develop | [SQL 数据仓库的设计决策和编程技术](/documentation/articles/sql-data-warehouse-overview-develop/) | SQL 数据仓库的开发概念、设计决策、建议和编程技术。 |



## 管理

| &nbsp; | Url-Id | 标题 | 说明 |
| --: | :-- | :-- | :-- |
| 25 | manage-compute-overview | [管理 Azure SQL 数据仓库中的计算能力（概述）](/documentation/articles/sql-data-warehouse-manage-compute-overview/) | Azure SQL 数据仓库中的性能横向扩展功能。通过调整 DWU 数目进行横向扩展，或者通过暂停和恢复计算资源来节省成本。 |
| 26 | manage-compute-powershell | [管理 Azure SQL 数据仓库中的计算能力 (PowerShell)](/documentation/articles/sql-data-warehouse-manage-compute-powershell/) | 用于管理计算能力的 PowerShell 任务。通过调整 DWU 缩放计算资源。或者，暂停和恢复计算资源来节省成本。 |
| 27 | manage-compute-rest-api | [管理 Azure SQL 数据仓库中的计算能力 (REST)](/documentation/articles/sql-data-warehouse-manage-compute-rest-api/) | 用于管理计算能力的 PowerShell 任务。通过调整 DWU 缩放计算资源。或者，暂停和恢复计算资源来节省成本。 |
| 28 | manage-compute-tsql | [管理 Azure SQL 数据仓库中的计算能力 (T-SQL)](/documentation/articles/sql-data-warehouse-manage-compute-tsql/) | 可通过调整 DWU 来提高性能的 Transact-SQL (T-SQL) 任务。通过在非高峰期缩减性能来节省成本。 |
| 29 | manage-monitor | [使用 DMV 监视工作负荷](sql-data-warehouse-manage-monitor/) | 了解如何使用 DMV 监视工作负荷。 |
| 30 | overview-manage | [在 Azure SQL 数据仓库中管理数据库](sql-data-warehouse-overview-manage/) | 管理 SQL 数据仓库数据库的概述。包括管理工具、DWU 和向外扩展性能，对查询性能进行故障排除，建立良好的安全策略，以及从数据损坏或区域中断还原数据库。 |
| 31 | overview-manage-user-queries | [在 Azure SQL 数据仓库中监视用户查询](/documentation/articles/sql-data-warehouse-overview-manage-user-queries/) | 在 Azure SQL 数据仓库中监视用户查询时的注意事项、最佳实践和任务的概述 |
| 32 | restore-database-overview | [还原 Azure SQL 数据仓库（概述）](/documentation/articles/sql-data-warehouse-restore-database-overview/) | 在 Azure SQL 数据仓库中恢复数据库时的数据库还原选项概述。 |
| 33 | restore-database-portal | [还原 Azure SQL 数据仓库（门户）](/documentation/articles/sql-data-warehouse-restore-database-portal/) | 用于还原 SQL 数据仓库的 Azure 门户任务。 |
| 34 | restore-database-powershell | [还原 Azure SQL 数据仓库 (PowerShell)](/documentation/articles/sql-data-warehouse-restore-database-powershell/) | 用于还原 SQL 数据仓库的 PowerShell 任务。 |
| 35 | restore-database-rest-api | [还原 Azure SQL 数据仓库 (REST API)](/documentation/articles/sql-data-warehouse-restore-database-rest-api/) | 用于还原 SQL 数据仓库的 REST API 任务。 |



## 表和索引

| &nbsp; | Url-Id | 标题 | 说明 |
| --: | :-- | :-- | :-- |
| 36 | tables-data-types | [SQL 数据仓库中表的数据类型](/documentation/articles/sql-data-warehouse-tables-data-types/) | Azure SQL 数据仓库表的数据类型入门。 |
| 37 | tables-distribute | [在 SQL 数据仓库中分布表](/documentation/articles/sql-data-warehouse-tables-distribute/) | 开始在 Azure SQL 数据仓库中分布表。 |
| 38 | tables-index | [为 SQL 数据仓库中的表编制索引](/documentation/articles/sql-data-warehouse-tables-index/) | Azure SQL 数据仓库中的表索引入门。 |
| 39 | tables-overview | [概述 SQL 数据仓库中的表](/documentation/articles/sql-data-warehouse-tables-overview/) | Azure SQL 数据仓库表入门。 |
| 40 | tables-partition | [对 SQL 数据仓库中的表进行分区](/documentation/articles/sql-data-warehouse-tables-partition/) | Azure SQL 数据仓库中的表分区入门。 |
| 41 | tables-statistics | [管理 SQL 数据仓库中表的统计信息](/documentation/articles/sql-data-warehouse-tables-statistics/) | Azure SQL 数据仓库中表的统计信息入门。 |
| 42 | tables-temporary | [SQL 数据仓库中的临时表](/documentation/articles/sql-data-warehouse-tables-temporary/) | 开始使用 Azure SQL 数据仓库中的临时表。 |



## 集成

| &nbsp; | Url-Id | 标题 | 说明 |
| --: | :-- | :-- | :-- |
| 43 | integrate-azure-stream-analytics | [将 Azure 流分析与 SQL 数据仓库配合使用](/documentation/articles/sql-data-warehouse-integrate-azure-stream-analytics/) | 有关在开发解决方案时将 Azure 流分析与 Azure SQL 数据仓库配合使用的技巧。 |
| 44 | integrate-power-bi | [将 Power BI 与 SQL 数据仓库配合使用](sql-data-warehouse-integrate-power-bi/) | 有关在开发解决方案时将 Power BI 与 Azure SQL 数据仓库配合使用的技巧。 |
| 45 | overview-integrate | [在 SQL 数据仓库中利用其他服务](/documentation/articles/sql-data-warehouse-overview-integrate/) | 用于集成 SQL 数据仓库的工具以及提供相应解决方案的合作伙伴。 |



## 加载

| &nbsp; | Url-Id | 标题 | 说明 |
| --: | :-- | :-- | :-- |
| 46 | load-from-azure-blob-storage-with-polybase | [将数据从 Azure Blob 存储载入 SQL 数据仓库 (PolyBase)](/documentation/articles/sql-data-warehouse-load-from-azure-blob-storage-with-polybase/) | 了解如何使用 PolyBase 将数据从 Azure Blob 存储载入 SQL 数据仓库。将公共数据中的一些表载入 Contoso 零售数据仓库架构。 |
| 47 | load-from-sql-server-with-azcopy | [将数据从 SQL Server 载入 Azure SQL 数据仓库 (AZCopy)](/documentation/articles/sql-data-warehouse-load-from-sql-server-with-azcopy/) | 使用 bcp 将数据从 SQL Server 导出到平面文件，使用 AZCopy 将数据导入 Azure Blob 存储，使用 PolyBase 将数据引入 Azure SQL 数据仓库。 |
| 48 | load-from-sql-server-with-bcp | [将数据从 SQL Server 载入 Azure SQL 数据仓库（平面文件）](/documentation/articles/sql-data-warehouse-load-from-sql-server-with-bcp/) | 对于少量的数据，可以使用 bcp 将数据从 SQL Server 导出到平面文件，然后将数据直接导入 Azure SQL 数据仓库。 |
| 49 | load-from-sql-server-with-integration-services | [将数据从 SQL Server 载入 Azure SQL 数据仓库 (SSIS)](/documentation/articles/sql-data-warehouse-load-from-sql-server-with-integration-services/) | 演示如何创建 SQL Server Integration Services (SSIS) 包，以便将数据从各种数据源移动到 SQL 数据仓库。 |
| 50 | load-from-sql-server-with-polybase | [在 SQL 数据仓库中使用 PolyBase 加载数据](/documentation/articles/sql-data-warehouse-load-from-sql-server-with-polybase/) | 使用 bcp 将数据从 SQL Server 导出到平面文件，使用 AZCopy 将数据导入 Azure Blob 存储，使用 PolyBase 将数据引入 Azure SQL 数据仓库。 |
| 51 | load-polybase-guide | [在 SQL 数据仓库中使用 PolyBase 的指南](/documentation/articles/sql-data-warehouse-load-polybase-guide/) | 有关在 SQL 数据仓库方案中使用 PolyBase 的指导原则和建议。 |
| 52 | load-sample-databases | [将示例数据载入 SQL 数据仓库](/documentation/articles/sql-data-warehouse-load-sample-databases/) | 将示例数据载入 SQL 数据仓库 |
| 53 | load-with-bcp | [使用 bcp 加载数据](/documentation/articles/sql-data-warehouse-load-with-bcp/) | 了解什么是 bcp，以及如何将它用于数据仓库方案。 |
| 54 | overview-load | [将数据载入 Azure SQL 数据仓库](/documentation/articles/sql-data-warehouse-overview-load/) | 了解将数据载入 SQL 数据仓库的常见方案。这些常见方案包括使用 PolyBase、Azure Blob 存储、平面文件以及磁盘寄送。也可使用第三方工具。 |



## 迁移

| &nbsp; | Url-Id | 标题 | 说明 |
| --: | :-- | :-- | :-- |
| 55 | migrate-code | [将 SQL 代码迁移到 SQL 数据仓库](/documentation/articles/sql-data-warehouse-migrate-code/) | 有关在开发解决方案时将 SQL 代码迁移到 Azure SQL 数据仓库的技巧。 |
| 56 | migrate-data | [迁移数据](/documentation/articles/sql-data-warehouse-migrate-data/) | 有关在开发解决方案时将数据迁移到 Azure SQL 数据仓库的技巧。 |
| 57 | migrate-migration-utility | [数据仓库迁移实用程序（预览版）](/documentation/articles/sql-data-warehouse-migrate-migration-utility/) | 迁移到 SQL 数据仓库。 |
| 58 | migrate-schema | [将架构迁移到 SQL 数据仓库](/documentation/articles/sql-data-warehouse-migrate-schema/) | 有关在开发解决方案时将架构迁移到 Azure SQL 数据仓库的技巧。 |
| 59 | overview-migrate | [将解决方案迁移到 SQL 数据仓库](/documentation/articles/sql-data-warehouse-overview-migrate/) | 有关将解决方案转移到 Azure SQL 数据仓库平台的迁移指南。 |



## 引用

| &nbsp; | Url-Id | 标题 | 说明 |
| --: | :-- | :-- | :-- |
| 60 | overview-reference | [SQL 数据仓库参考主题](/documentation/articles/sql-data-warehouse-overview-reference/) | SQL 数据仓库的参考内容链接。 |
| 61 | reference-powershell-cmdlets | [适用于 SQL 数据仓库的 PowerShell cmdlet 和 REST API](/documentation/articles/sql-data-warehouse-reference-powershell-cmdlets/) | 了解 Azure SQL 数据仓库的最常用 PowerShell cmdlet，包括如何暂停和恢复数据库。 |
| 62 | reference-tsql-language-elements | [语言元素](/documentation/articles/sql-data-warehouse-reference-tsql-language-elements/) | 用于 SQL 数据仓库的 Transact-SQL 语言元素的参考内容链接列表。 |
| 63 | reference-tsql-statements | [Transact-SQL 主题](/documentation/articles/sql-data-warehouse-reference-tsql-statements/) | SQL 数据仓库使用的 Transact-SQL 主题参考内容的链接。 |
| 64 | reference-tsql-system-views | [系统视图](/documentation/articles/sql-data-warehouse-reference-tsql-system-views/) | SQL 数据仓库的系统视图内容链接。 |



## “安全”

| &nbsp; | Url-Id | 标题 | 说明 |
| --: | :-- | :-- | :-- |
| 65 | encryption-tde-tsql | [透明数据加密 (TDE) 入门](/documentation/articles/sql-data-warehouse-encryption-tde-tsql/) | SQL 数据仓库透明数据加密 (TDE) TSQL 入门 |
| 66 | overview-manage-security | [保护 SQL 数据仓库中的数据库](/documentation/articles/sql-data-warehouse-overview-manage-security/) | 有关在开发解决方案时保护 Azure SQL 数据仓库中的数据库的技巧。 |



## 其他

| &nbsp; | Url-Id | 标题 | 说明 |
| --: | :-- | :-- | :-- |
| 67 | install-visual-studio | [安装适用于 SQL 数据仓库的 Visual Studio 2015 和 SSDT](/documentation/articles/sql-data-warehouse-install-visual-studio/) | 安装适用于 Azure SQL 数据仓库的 Visual Studio 和 SQL Server 开发工具 (SSDT) |
| 68 | service-capacity-limits | [SQL 数据仓库容量限制](/documentation/articles/sql-data-warehouse-service-capacity-limits/) | SQL 数据仓库的连接、数据库、表和查询的最大值。 |
| 69 | 故障排除 | [排查 Azure SQL 数据仓库问题](/documentation/articles/sql-data-warehouse-troubleshoot/) | 排查 Azure SQL 数据仓库问题。 |

<!---HONumber=Mooncake_0822_2016-->