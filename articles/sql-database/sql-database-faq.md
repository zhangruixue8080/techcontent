<properties 
   pageTitle="Azure SQL 数据库常见问题" 
   description="客户就云数据库、Azure SQL 数据库、Microsoft 的关系数据库管理系统 (RDBMS) 和云中“数据库即服务”经常提出的问题的解答。" 
   services="sql-database" 
   documentationCenter="" 
   authors="jeffgoll" 
   manager="jeffreyg" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.date="05/25/2016"
   wacn.date="07/18/2016"/>

# SQL 数据库常见问题

## SQL 数据库的使用情况如何体现在我的帐单上？ 
SQL 数据库以可预测的每小时费率收费，同时根据服务层 + 单一数据库的性能级别或每一弹性数据库池的 eDTU。实际使用量是每小时按比例计算的，因此你的帐单可能会显示一小时的分数。例如，如果某个数据库在一个月内存在了 12 小时，则你的帐单将显示 0.5 天的使用量。服务层 + 性能级别和每一池 eDTU 在帐单中划分，让你方便看到在单个月份中使用的数据库天数。

## 如果单一数据库活动的时间少于一小时，或使用更高服务层的时间少于一小时，会发生什么情况？
需要支付使用最高服务层数据库存在的时数 + 在该小时适用的性能级别，无论使用方式或数据库的活动状态是否少于一小时。例如，如果你创建了单一数据库，5 分钟后删除了它，则将按该数据库存在 1 小时收费。

示例
	
- 如果你创建了一个基本数据库并立即将其升级为标准版 S1，则第一小时将按标准版 S1 费率收费。

- 如果你在晚上 10:00 将数据库从基本版升级到高级版，并且升级过程在第二天凌晨 1:35 完成，则将从凌晨 1:00 开始按高级版费率向你收费。

- 如果你在上午 11:00 将数据库从“高级”降级到“基本”级别，并且在下午 2:15 完成降级，则会针对数据库以高级版费率收取到下午 3:00，之后将以基本版费率收费。

## 弹性数据库池的使用情况如何体现在我的帐单上，另外，更改每个池的 eDTU 会发生什么情况？
在帐单上，弹性数据库池收费显示为弹性 DTU (eDTU)，并在[定价页](/pricing/details/sql-database/)上以每一池 eDTU 递增显示。弹性数据库池没有每一数据库的费用。对于池存在的每个小时，你需要支付最高的 eDTU，无论使用量是多少，也不管池处于活动状态的时间少于一小时。

示例

- 如果在上午 11:18 以 200 eDTU 创建标准弹性数据库池，将五个数据库添加到池，你将被收取在该小时 200 eDTU 的费用，从上午 11:00 开始到当天剩余的时间。
- 在第 2 天上午 5:05，数据库 1 开始使用 50 eDTU 并稳定持有一天。数据库 2-5 在 0 和 80 eDTU 之间波动。在当天，添加全天使用不同 eDTU 的其他五个数据库。第 2 天将全天以 200 eDTU 计费。
- 在第 3 天的上午 5:00，你添加了另外 15 个数据库。数据库使用量全天增加，到下午 8:05 确定将池的 eDTU 从 200 增加为 400。以 200 eDTU 收费生效到下午 8:00，并在其余 4 小时增加到 400 eDTU。

## 如何在我的帐单上体现弹性数据库池中的活动异地复制的使用？
与单一数据库不同的是，对弹性数据库使用[活动异地复制](/documentation/articles/sql-database-geo-replication-overview/)对计费没有直接的影响。你只需支付对每个池（主池和辅助池）预配的 eDTU 费用

## 使用审核功能将对我的帐单产生什么影响？ 
审核功能是 SQL 数据库服务的内置功能，无需另行付费，基本、标准和高级数据库均提供此功能。但是，为了存储审核日志，审核功能将使用 Azure 存储帐户，而 Azure 存储空间中的表和队列费率根据审核日志的大小来应用。

## 如何找到单一数据库和弹性数据库池的正确服务层和性能级别？ 
有几个工具可供使用。

- 对于本地数据库，请使用 [DTU 选型顾问](http://dtucalculator.azurewebsites.net)，它会建议所需的数据库和 DTU，并为弹性数据库池评估多个数据库。
- 如果单一数据库可因池受益，当 Azure 的智能引擎发现了担保的历史使用模式时，将对数据库建议弹性数据库池。请参阅 [Monitor and manage an elastic database pool with the Azure PowerShell（使用 Azure PowerShell 监视和管理弹性数据库池）](/documentation/articles/sql-database-elastic-pool-manage-powershell/)。有关如何自行进行数学计算的详细信息，请参阅[弹性数据库池的价格和性能注意事项](/documentation/articles/sql-database-elastic-pool-guidance/)
- 若要确定是否需要向上或向下调整单一数据库，请参阅[单一数据库的性能指南](/documentation/articles/sql-database-performance-guidance/)。

## 可以按何种频率更改单一数据库的服务层或性能级别？ 
使用 V12 数据库，你可以（在基本、标准和高级之间）更改服务层或服务层内的性能级别（例如 S1 到 S2），次数随意。对于早期版本的数据库，你可以在 24 小时内更改服务层或性能级别总共四次。

##可以按何种频率调整每个池的 eDTU？ 
次数随意。

## 更改单一数据库的服务层次或性能级别，或将数据库移入和移出弹性数据库池需要多长时间？ 
更改数据库的服务层和移入和移出池需要在平台上以后台操作的形式复制数据库。这可能需要几分钟至几小时的时间，具体取决于数据库的大小。在这两种情况下，数据库在移动期间保持联机和可用。有关更改单一数据库的详细信息，对于弹性数据库，请参阅[弹性池参考](/documentation/articles/sql-database-elastic-pool/#latency-of-elastic-pool-operations)

##何时应该使用单一数据库或弹性数据库？ 
一般而言，弹性数据库池针对典型的[软件即服务 (SaaS) 应用程序模式](/documentation/articles/sql-database-design-patterns-multi-tenancy-saas-applications/)而设计，该模式中每个客户或租户有一个数据库。购买单独的数据库并超量设置以满足每个数据库的可变和峰值需求通常不够经济高效。使用池可以管理池的整体性能，数据库将自动扩展和收缩。

如果 Azure 的智能引擎发现了担保的使用模式，将为数据库建议池。有关在单一数据库与弹性数据库之间进行选择的详细指导，请参阅[弹性数据库池的价格和性能注意事项](/documentation/articles/sql-database-elastic-pool-guidance/)。

## 具有高达备份存储的最大已设置数据库存储两倍的容量是什么意思？ 
备份存储是与用于[时间点还原](/documentation/articles/sql-database-point-in-time-restore/)和[异地还原](/documentation/articles/sql-database-geo-restore/)的自动数据库备份关联的存储。Azure SQL 数据库提供了高达你的备份存储的最大已设置数据库存储两倍的容量，不需要支付额外的成本。例如，如果你有一个标准数据库实例并且设置的数据库大小为 250 GB，则会向你提供 500 GB 的备份存储并且不额外收费。如果数据库超过提供的备份存储，则可以选择与 Azure 支持联系来缩短保留期，或针对按标准读取访问地域冗余存储 (RA-GRS) 费率计费的额外备份存储支付费用。有关 RA-GRS 计费的更多信息，请参阅“存储定价详细信息”。

## 我正在从 Web/企业版迁移到新服务层，我需要了解哪些信息？
Azure SQL Web 和企业数据库现已停用。基本、标准、高级和弹性层将取代即将停用的 Web 和企业数据库。我们制作了额外的常见问题解答，以帮助你完成此过渡期。[Web 和 Business Edition 停用常见问题](/documentation/articles/sql-database-web-business-sunset-faq/)

## 当在相同的 Azure 地理位置内的两个区域之间进行异地复制数据库时，哪些是预期的复制延迟？  
我们目前支持 5 秒的 RPO，并且只要地域辅助数据库承载于 Azure 建议的配对区域并且属于相同的服务层，则复制延迟就会少于该时间。

## 在主数据库所在的同一区域中创建地域辅助数据库时，哪些是预期的复制滞后？  
根据经验数据，在使用 Azure 建议的配对区域时，内部区域和区域之间的复制延迟没有太多差别。

## 如果两个区域之间存在网络故障，那么在设置了异地复制的情况下，重试逻辑是如何运作的？  
如果存在断开连接，我们会每 10 秒钟重试一次以重新建立连接。

## 为了保证复制主数据库上的关键更改，我该做什么？
地域辅助数据库是非同步副本，我们不尝试将其与主数据库保持完全同步。但我们提供了一种强制执行同步的方法。它旨在确保复制关键更改（例如，密码更新）。因为它将阻止调用线程，直到所有提交的事务得到复制，因此它会影响性能。有关详细信息，请参阅 [sp\_wait\_for\_database\_copy\_sync](https://msdn.microsoft.com/zh-cn/library/dn467644.aspx)。

## 哪些工具可用于监视主数据库与地域辅助数据库之间的复制延迟？
我们通过 DMV 显示主数据库与地域辅助数据库之间的实时复制延迟。有关详细信息，请参阅 [sys.dm\_geo\_replication\_link\_status](https://msdn.microsoft.com/zh-cn/library/mt575504.aspx)。

<!---HONumber=Mooncake_0711_2016-->