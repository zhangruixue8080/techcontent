### 使用 PowerShell 验证连接

可使用 `Get-AzureRmVirtualNetworkGatewayConnection` cmdlet（无论是否有 `-Debug`）来验证连接是否已成功。

1. 使用以下 cmdlet 示例，配置符合自己需要的值。如果出现提示，请选择“A”运行“所有”。在此示例中，`-Name` 指所创建的，并要测试的连接的名称。

		Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG

2. cmdlet 运行完毕后，查看该值。在以下示例中，连接状态显示为“已连接”，且可以看到入口和出口字节数。

		Body:
		{
		  "name": "MyGWConnection",
		  "id":
		"/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/connections/MyGWConnection",
		  "properties": {
		    "provisioningState": "Succeeded",
		    "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
		    "virtualNetworkGateway1": {
		      "id":
		"/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/virtualNetworkGa
		teways/vnetgw1"
		    },
		    "localNetworkGateway2": {
		      "id":
		"/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/localNetworkGate
		ways/LocalSite"
		    },
		    "connectionType": "IPsec",
		    "routingWeight": 10,
		    "sharedKey": "abc123",
		    "connectionStatus": "Connected",
		    "ingressBytesTransferred": 33509044,
		    "egressBytesTransferred": 4142431
		  }

### 使用 Azure 门户预览验证连接

在 Azure 门户预览中，可通过导航到该连接来查看连接状态。有多种方法可执行此操作。下面是一种导航到连接的方法。

1. 在 [Azure 门户预览](http://portal.azure.cn)中，导航到“虚拟网络网关”。单击网关名称。
2. 在该窗格的“设置”下，单击“连接”。可查看每个连接的状态。
3. 有关连接的详细信息，请单击连接名称。在连接的“概要”页面中，注意“连接状态”。成功连接后，状态将是“已成功”和“已连接”。可通过查看“数据输入”和“数据输出”了解数据流动。

	在以下示例中，“连接状态”为“未连接”。

	![验证连接](./media/vpn-gateway-verify-connection-rm-include/connectionverify450.png)  

<!---HONumber=Mooncake_0822_2016-->