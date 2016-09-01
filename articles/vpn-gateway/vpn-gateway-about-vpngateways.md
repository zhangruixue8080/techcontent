<properties 
   pageTitle="关于 VPN 网关 | Azure"
   description="了解 Azure 虚拟网络的 VPN 网关。"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags
	ms.service="vpn-gateway"
	ms.date="07/20/2016"
	wacn.date="08/29/2016"/>  


# 关于 VPN 网关

VPN 网关是设置的集合，这些设置用于在虚拟网络和本地位置之间发送网络流量。本文的各个部分将讨论与 VPN 网关相关的设置。VPN 网关用于站点到站点、点到站点以及 ExpressRoute 连接。VPN 网关还用于在 Azure 内的多个虚拟网络之间发送流量（VNet 到 VNet）。

可将 VPN 网关添加到虚拟网络以创建连接。每个虚拟网络只能有一个 VPN 网关，而且每个连接都有特定的配置步骤。有关连接关系图，请参阅 [VPN 网关连接拓扑](/documentation/articles/vpn-gateway-topology/)。

## <a name="gwsku"></a>网关 SKU

在创建 VPN 网关时，你需要指定想要使用的网关 SKU。网关 SKU 适用于 ExpressRoute 和 VPN 网关类型。网关 SKU 之间并无定价差异。有关定价的信息，请参阅 [VPN 网关定价](/pricing/details/vpn-gateway/)。有关 ExpressRoute 的详细信息，请参阅 [ExpressRoute 技术概述](/documentation/articles/expressroute-introduction/)。

共有 3 种 VPN 网关 SKU：

- 基本
- 标准
- HighPerformance

以下示例将 `-GatewaySku` 指定为 *Standard*。

	New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg -Location 'China North' -IpConfigurations $gwipconfig -GatewaySku Standard -GatewayType Vpn -VpnType RouteBased

###  <a name="aggthroughput"></a>按 SKU 和网关类型列出的估计聚合吞吐量


下表显示网关类型和估计的聚合吞吐量。此表适用于 Resource Manager 与经典部署模型。

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

## <a name="gwtype"></a>网关类型

网关类型指定网关本身如何进行连接，它是 Resource Manager 部署模型的必需配置设置。请勿将网关类型与 VPN 类型混为一谈，后者指定的是 VPN 的路由类型。`-GatewayType` 的可用值为：

- Vpn
- ExpressRoute


此 Resource Manager 部署模型示例将 -GatewayType 指定为 *Vpn*。在创建网关时，你必须确保用于配置的网关类型正确。

	New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg -Location 'China North' -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

## <a name="connectiontype"></a>连接类型

每个配置需要特定的连接类型。`-ConnectionType` 的可用 Resource Manager PowerShell 值为：

- IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient

在以下示例中，我们将创建站点到站点连接，这需要“IPsec”连接类型。

	New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg -Location 'China North' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

## <a name="vpntype"></a>VPN 类型

每个配置需要特定的 VPN 类型才能正常运行。如果要合并两个配置，例如创建连往相同 VNet 的站点到站点连接和点到站点连接，你必须使用同时符合这两个连接要求的 VPN 类型。

在点到站点和站点到站点并存连接的情况下，使用 Azure Resource Manager 部署模型时必须使用基于路由的 VPN 类型，而使用经典部署模式时必须使用动态网关。

在创建配置时，需选择连接所需的 VPN 类型。

有两种 VPN 类型：

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

此 Resource Manager 部署模型示例将 `-VpnType` 指定为 *RouteBased*。在创建网关时，你必须确保用于配置的 -VpnType 正确。

	New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg -Location 'China North' -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

##  <a name="requirements" id="gateway-requirements"></a>网关要求

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)]


## <a name="gwsub"></a>网关子网

若要配置 VPN 网关，需要先为 VNet 创建网关子网。网关子网必须命名为 *GatewaySubnet* 才能正常工作。此名称可以让 Azure 知道此子网将用于网关。<BR>如果使用的是经典管理门户，在门户界面中，网关子网将自动命名为 *Gateway*。这仅特定于在经典管理门户中查看网关子网。在此示例中，子网实际上是在 Azure 中作为 *GatewaySubnet* 创建的，可以在 Azure 门户预览和 PowerShell 中以这种方式进行查看。

网关子网的最小大小完全取决于你要创建的配置。尽管某些配置的网关子网最小可以创建为 /29，但建议创建 /28 或更大（/28、/27、/26 等）的网关子网。

创建较大的网关可防止违反网关大小限制。例如，如果你创建网关子网大小为 /29 的网关，并且想要配置站点到站点/ExpressRoute 并存配置，则必须删除网关、删除网关子网、将网关子网创建为 /28 或更大，然后重新创建网关。

如果一开始就创建较大的网关子网，以后就可以在向网络环境添加配置功能时节省时间。

以下示例显示了名为 GatewaySubnet 的网关子网。可以看到，CIDR 表示法指定了 /27，这可提供足够的 IP 地址供大多数现有配置使用。

	Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27

>[AZURE.IMPORTANT] 确保未对 GatewaySubnet 应用网络安全组 (NSG)，因为这可能会导致连接失败。



## <a name="lng"></a>本地网关

局域网网关通常是指你的本地位置。在经典部署模型中，局域网网关称为本地站点。指定局域网网关的名称（即本地 VPN 设备的公共 IP 地址），并指定位于本地位置的地址前缀。Azure 将查看网络流量的目标地址前缀、查阅你为局域网网关指定的配置，并相应地路由数据包。你可以根据需要修改这些地址前缀。


### 修改地址前缀 - Resource Manager

修改地址前缀的过程根据是否已创建 VPN 网关而有所不同。请参阅以下文章部分：[修改本地网关的地址前缀](/documentation/articles/vpn-gateway-create-site-to-site-rm-powershell/#modify)。

在以下示例中，你可以看到要指定名为 MyOnPremiseWest 的局域网网关，其中包含两个 IP 地址前缀。

	New-AzureRmLocalNetworkGateway -Name MyOnPremisesWest -ResourceGroupName testrg -Location 'China North' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')	

### 修改地址前缀 - 经典部署

如果需要在使用经典部署模型时修改本地站点，则可以使用经典管理门户中的“局域网”配置页，或直接修改网络配置文件 NETCFG.XML。



## 后续步骤

请参阅 [VPN 网关常见问题](/documentation/articles/vpn-gateway-vpn-faq/)了解详细信息之后，再继续规划和设计您的配置。





 

<!---HONumber=Mooncake_0822_2016-->