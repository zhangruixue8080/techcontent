<properties 
   pageTitle="使用经典部署模型配置站点到站点连接的强制隧道 | Azure"
   description="如何重定向或“强制”所有 Internet 绑定的流量路由回本地位置。"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
	ms.service="vpn-gateway"
	ms.date="08/10/2016"
	wacn.date="08/29/2016"/>  


# 使用经典部署模型配置强制隧道

> [AZURE.SELECTOR]
- [PowerShell - 经典](/documentation/articles/vpn-gateway-about-forced-tunneling/)
- [PowerShell - Resource Manager](/documentation/articles/vpn-gateway-forced-tunneling-rm/)

借助强制隧道，你可以通过站点到站点 VPN 隧道，将全部 Internet 绑定流量重定向或“强制”返回到本地位置，以进行检查和审核。这是很多企业 IT 策略的关键安全要求。

没有强制隧道，来自 Azure 中虚拟机的 Internet 绑定流量会始终通过 Azure 网络基础设施直接连接到 Internet。没有该选项，您无法对流量进行检查或审核。未经授权的 Internet 访问可能会导致信息泄漏或其他类型的安全漏洞。

本文将引导你配置虚拟网络（通过使用经典部署模型创建）的强制隧道。

**关于 Azure 部署模型**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

**强制隧道的部署模型和工具**

可为经典部署模型和 Resource Manager 部署模型配置强制隧道连接。有关详细信息，请参阅下表。我们将在由可用于此配置的新文章、新部署模型和其他工具时更新此表。当有文章可用时，我们将从表中直接链接到该文章。

[AZURE.INCLUDE [vpn-gateway-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)]


## 要求和注意事项

在 Azure 中，可通过虚拟网络用户定义路由配置强制隧道 (UDR)。将流量重定向到本地站点，这是 Azure VPN 网关的默认路由。以下部分列出了 Azure 虚拟网络路由和路由表的当前限制：


-  每个虚拟网络子网具有内置的系统路由表。系统路由表具有以下三组路由：

	- **本地 VNet 路由：**直接路由到同一个虚拟网络中的目标虚拟机
	
	- **本地路由：**路由到 Azure VPN 网关
	
	- **默认路由：**直接路由到 Internet。如果要将数据包发送到不包含在前面两个路由中的专用 IP 地址，数据包将被删除。


-  在发布的用户定义路由中，你可以创建路由表来添加默认路由，然后将路由表关联到虚拟网络子网，在这些子网启用强制隧道。

- 您需要在连接到虚拟网络的跨界本地站点中，设置一个“默认站点”。

- 强制隧道必须关联到具有动态路由 VPN 网关的 VNet，不能是静态网关。
 
- ExpressRoute 强制隧道不是通过此机制配置的，而是通过 ExpressRoute BGP 对等会话播发默认路由来启用的。有关详细信息，请参阅 [ExpressRoute 文档](/documentation/services/expressroute/)。



## 配置概述

在以下示例中，前端子网没有使用强制隧道。前端子网中的工作负载可以继续直接接受并响应来自 Internet 的客户请求。中间层和后端子网会使用强制隧道。任何从这两个子网到 Internet 的出站连接将通过一个 S2S VPN 隧道重定向或强制返回到本地站点。

这样，在继续支持所需的多层服务体系结构的同时，你可以限制并检查来自虚拟机或 Azure 云服务的 Internet 访问。如果在虚拟网络中没有面向 Internet 的工作负荷，也能选择对整个虚拟网络应用强制隧道连接。


![强制隧道](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)  




## 开始之前

在开始配置之前，请确认具有以下各项。

- Azure 订阅。如果你还没有 Azure 订阅，你可以注册一个[试用版](/pricing/1rmb-trial)。

- 已配置虚拟网络。

- 最新版本的 Azure PowerShell cmdlet。有关安装 PowerShell cmdlet 的详细信息，请参阅[如何安装和配置 Azure PowerShell](/documentation/articles/powershell-install-configure/)。


## 配置强制隧道

以下过程将帮助您为虚拟网络指定强制隧道。配置步骤与 VNet 网络配置文件相对应。



	<VirtualNetworkSite name="MultiTier-VNet" Location="China North">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
	</VirtualNetworkSite>

在本示例中，虚拟网络“MultiTier-VNet”具有三个子网：*前端*、*中间层*和*后端*子网，并且具有四个跨界连接：一个 *DefaultSiteHQ* 和三个 *Branches*。

以下步骤将 *DefaultSiteHQ* 设置为使用强制隧道的默认站点连接，并将中间层和后端子网配置为使用强制隧道。


1. 创建一个路由表。使用以下 cmdlet 创建路由表。

		New-AzureRouteTable -Name "MyRouteTable" -Label "Routing Table for Forced Tunneling" -Location "China North"

2. 将默认路由添加到路由表中。

	下面的示例将默认路由添加到在步骤 1 中创建的路由表。请注意，唯一支持的路由是“0.0.0.0/0”到“VPN 网关”下一跃点的目标前缀。
 
		Set-AzureRoute -RouteTable "MyRouteTable" -RouteName "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VPNGateway

3. 将路由表关联到子网。

	创建路由表并添加路由后，可以使用以下示例将路由表添加到 VNet 子网，或将路由表与 VNet 子网关联。下面的示例将“MyRouteTable”路由表添加到 VNet MultiTier-VNet 的中间层和后端子网。

		Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"

		Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"

4. 为强制隧道指定默认站点。

	在前面的步骤中，示例 cmdlet 脚本创建了路由表，并将路由表关联到两个 VNet 子网。剩下的步骤是在虚拟网络的多站点连接中，选择一个本地站点作为默认站点或隧道。

		$DefaultSite = @("DefaultSiteHQ")
		Set-AzureVNetGatewayDefaultSite -VNetName "MultiTier-VNet" -DefaultSite "DefaultSiteHQ"

## 其他 PowerShell cmdlet

### 删除路由表

	Remove-AzureRouteTable -Name <routeTableName>

### 列出路由表

	Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]

### 从路由表中删除路由

	Remove-AzureRouteTable -Name <routeTableName>

### 从子网中删除路由

	Remove-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### 列出与子网关联的路由表
	
	Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### 从 VNet VPN 网关中删除默认站点

	Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>

<!---HONumber=Mooncake_0822_2016-->