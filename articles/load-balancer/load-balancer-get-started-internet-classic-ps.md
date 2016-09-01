<properties 
   pageTitle="开始使用 PowerShell 在经典模式下创建面向 Internet 的负载平衡器 | Azure"
   description="了解如何使用 PowerShell 在经典模式下创建面向 Internet 的负载平衡器"
   services="load-balancer"
   documentationCenter="na"
   authors="joaoma"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="load-balancer"
   ms.date="04/05/2016"
   wacn.date="08/29/2016" />  


# 开始在 PowerShell 中创建面向 Internet 的负载平衡器（经典）

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

>[AZURE.IMPORTANT]在使用 Azure 资源之前，请务必了解 Azure 当前使用两种部署模型：资源管理器部署模型和经典部署模型。在使用任何 Azure 资源之前，请确保你了解[部署模型和工具](/documentation/articles/azure-classic-rm/)。可以通过单击本文顶部的选项卡来查看不同工具的文档。本文介绍经典部署模型。你还可以[了解如何使用 Azure Resource Manager 创建面向 Internet 的负载平衡器](/documentation/articles/load-balancer-get-started-internet-arm-ps/)。

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]



## 使用 PowerShell 设置负载平衡器

若要使用 powershell 设置负载平衡器，请按照以下步骤进行操作：

1. 如果你从未使用过 Azure PowerShell，请参阅 [How to Install and Configure Azure PowerShell](/documentation/articles/powershell-install-configure/)（如何安装和配置 Azure PowerShell），并始终按照说明进行操作，以登录到 Azure 并选择你的订阅。


2. 创建虚拟机后，可以使用 PowerShell cmdlet 将负载平衡器添加到同一云服务中的虚拟机。

在下面的示例中，你会将名为“webfarm”的负载平衡器集添加到云服务“mytestcloud”（或 myctestcloud.chinacloudapp.cn），将负载平衡器的终结点添加到名为“web1”和“web2”的虚拟机。负载平衡器在端口 80 上接收网络流量，并在由本地终结点（在此示例中为端口 80）定义的虚拟机之间使用 TCP 进行负载平衡。


### 步骤 1
为第一个 VM“web1”创建负载平衡终结点

	Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

### 步骤 2 

使用相同的负载平衡器集名称为第二个 VM “web2”创建另一个终结点

	Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

## 从负载平衡器中删除虚拟机

可以使用 Remove-AzureEndpoint 从负载平衡器中删除虚拟机终结点

	Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin| Update-AzureVM

## 后续步骤

你还可以[开始创建内部负载平衡器](/documentation/articles/load-balancer-get-started-ilb-classic-ps)，并配置适合特定负载平衡器网络流量行为的[分发模式](/documentation/articles/load-balancer-distribution-mode)类型。

如果应用程序需要始终保持对负载平衡器后面的服务器的连接，你可以详细了解[负载平衡器的空闲 TCP 超时设置](/documentation/articles/load-balancer-tcp-idle-timeout)。该文章将有助于你了解使用 Azure Load Balancer 时的空闲连接行为。

<!---HONumber=Mooncake_0822_2016-->