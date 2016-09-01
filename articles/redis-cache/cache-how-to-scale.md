<properties 
	pageTitle="如何缩放 Azure Redis 缓存 | Azure" 
	description="了解如何缩放你的 Azure Redis 缓存实例" 
	services="redis-cache" 
	documentationCenter="" 
	authors="steved0x" 
	manager="douge" 
	editor=""/>

<tags
	ms.service="cache"
	ms.date="06/16/2016"
	wacn.date="07/25/2016"/>

# 如何缩放 Azure Redis 缓存

>[AZURE.NOTE] Azure Redis 缓存缩放功能目前处于预览状态。

Azure Redis 缓存具有不同的缓存产品/服务，使缓存大小和功能的选择更加灵活。如果创建缓存后，你的应用程序的要求发生更改，可以使用 Azure PowerShell 缩放缓存的大小。

## 何时缩放

[访问 Redis 缓存监视数据](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring)示例演示了如何访问 Azure Redis 缓存的监视数据。

你可以使用以下指标帮助确定是否需要进行缩放。

-	Redis 服务器负载
-	内存用量
-	网络带宽
-	CPU 使用率

如果确定缓存不再满足应用程序的要求，可以更改到适合应用程序的更大或更小缓存定价层。有关确定应使用哪个缓存定价层的详细信息，请参阅[我应当使用哪些 Redis 缓存产品/服务和大小](/documentation/articles/cache-faq/#what-redis-cache-offering-and-size-should-i-use)。

## <a name="how-to-automate-a-scaling-operation"></a> 如何自动执行缩放操作

你可以使用 Azure Redis 缓存 PowerShell cmdlet、Azure CLI 和 Azure 管理库 (MAML) 进行缩放。

-	[使用 PowerShell 进行缩放](#scale-using-powershell)
-	[使用 Azure CLI 进行缩放](#scale-using-azure-cli)
-	[使用 MAML 进行缩放](#scale-using-maml)

### <a name="scale-using-powershell"></a> 使用 PowerShell 进行缩放

修改 `Size`、`Sku` 或 `ShardCount` 属性后，可以在 PowerShell 中使用 [Set-AzureRmRedisCache](https://msdn.microsoft.com/zh-cn/library/azure/mt634518.aspx) cmdlet 缩放 Azure Redis 缓存实例。以下示例演示了如何将名为 `myCache` 的缓存缩放为 2.5 GB 缓存。

	Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

有关使用 PowerShell 进行缩放的详细信息，请参阅[使用 PowerShell 缩放 Redis 缓存](/documentation/articles/cache-howto-manage-redis-cache-powershell/#scale)。

### <a name="scale-using-azure-cli"></a> 使用 Azure CLI 进行缩放

若要使用 Azure CLI 缩放 Azure Redis 缓存实例，请调用 `azure rediscache set` 命令并传入所需的配置更改，包括新大小、sku 或群集大小，具体取决于所需的缩放操作。

有关使用 Azure CLI 进行缩放的详细信息，请参阅[更改现有 Redis 缓存的设置](/documentation/articles/cache-manage-cli/#scale)。

### <a name="scale-using-maml"></a> 使用 MAML 进行缩放

若要使用 [Azure 管理库 (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/) 缩放 Azure Redis 缓存实例，请调用 `IRedisOperations.CreateOrUpdate` 方法并传入 `RedisProperties.SKU.Capacity` 的新大小。

    static void Main(string[] args)
    {
        // For instructions on getting the access token, see
        // /documentation/articles/cache-configure/#access-keys
        string token = GetAuthorizationHeader();

        TokenCloudCredentials creds = new TokenCloudCredentials(subscriptionId,token);

        RedisManagementClient client = new RedisManagementClient(creds);
        var redisProperties = new RedisProperties();

        // To scale, set a new size for the redisSKUCapacity parameter.
        redisProperties.Sku = new Sku(redisSKUName,redisSKUFamily,redisSKUCapacity);
        redisProperties.RedisVersion = redisVersion;
        var redisParams = new RedisCreateOrUpdateParameters(redisProperties, redisCacheRegion);
        client.Redis.CreateOrUpdate(resourceGroupName,cacheName, redisParams);
    }

有关详细信息，请参阅[使用 MAML 管理 Redis 缓存](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML)示例。

## 关于缩放的常见问题

以下列表包含有关 Azure Redis 缓存缩放的常见问题的解答。

-	[可以向上缩放到高级缓存，或在其中向下缩放吗？](#can-i-scale-to-from-or-within-a-premium-cache)
-	[缩放后，我是否需要更改缓存名称或访问密钥？](#after-scaling-do-i-have-to-change-my-cache-name-or-access-keys)
-	[缩放的工作原理？](#how-does-scaling-work)
-	[在缩放过程中是否会丢失缓存中的数据？](#will-i-lose-data-from-my-cache-during-scaling)
-	[在缩放过程中，自定义数据库设置是否会受影响？](#is-my-custom-databases-setting-affected-during-scaling)
-	[在缩放过程中，缓存是否可用？](#will-my-cache-be-available-during-scaling)
-	[不支持的操作](#operations-that-are-not-supported)
-	[缩放需要多长时间？](#how-long-does-scaling-take)
-	[如何判断缩放何时完成？](#how-can-i-tell-when-scaling-is-complete)
-	[为何此功能处于预览状态？](#why-is-this-feature-in-preview)

### <a name="can-i-scale-to-from-or-within-a-premium-cache"></a> 可以向上缩放到高级缓存，或在其中向下缩放吗？

-	不能从**高级**缓存向下缩放到**基本**或**标准**定价层。
-	可以从一个**高级**缓存定价层缩放到另一个高级缓存定价层。
-	不能从**基本**缓存直接缩放到**高级**缓存。必须先在一个缩放操作中从**基本**缩放到**标准**，然后再在后续的缩放操作中从**标准**缩放到**高级**。
-	如果在创建**高级**缓存时启用了群集，则可以[更改群集大小](/documentation/articles/cache-how-to-premium-clustering/#cluster-size)。目前，不能在创建时没有群集的以前存在的缓存上启用群集。

    有关详细信息，请参阅[如何为高级 Azure Redis 缓存配置群集](/documentation/articles/cache-how-to-premium-clustering/)。

### <a name="after-scaling-do-i-have-to-change-my-cache-name-or-access-keys"></a> 缩放后，我是否需要更改缓存名称或访问密钥？

不需要，在缩放操作期间缓存名称和密钥不变。

### <a name="how-does-scaling-work"></a> 缩放的工作原理？

-	将**基本**缓存缩放为不同大小时，将关闭该缓存，同时使用新的大小预配一个新缓存。在此期间，缓存不可用，且缓存中的所有数据都将丢失。
-	将**基本**缓存缩放为**标准**缓存时，将预配副本缓存并将主缓存中的数据复制到副本缓存。在缩放过程中，缓存仍然可用。
-	将**标准**缓存缩放为不同大小或缩放到**高级**缓存时，将关闭其中一个副本，同时将其重新预配为新的大小，将数据转移，然后，在重新预配另一个副本之前，另一个副本将执行一次故障转移，类似于一个缓存节点发生故障时所发生的过程。

### <a name="will-i-lose-data-from-my-cache-during-scaling"></a> 在缩放过程中是否会丢失缓存中的数据？

-	将**基本**缓存缩放为新的大小时，所有数据都将丢失，且在缩放操作期间缓存将不可用。
-	将**基本**缓存缩放为**标准**缓存时，通常将保留缓存中的数据。
-	将**标准**缓存扩展为更大大小或更大层，或者将**高级**缓存扩展为更大大小时，通常将保留所有数据。将**标准**或**高级**缓存缩小到更小大小时，数据可能会丢失，具体取决于与缩放后的新大小相关的缓存中的数据量。如果缩小时数据丢失，则使用 [allkeys lru](http://redis.io/topics/lru-cache) 逐出策略逐出密钥。

### <a name="is-my-custom-databases-setting-affected-during-scaling"></a> 在缩放过程中，自定义数据库设置是否会受影响？

某些定价层具有不同的数据库限制，因此，如果在缓存创建过程中为 `databases` 设置配置了自定义值，则在向下缩放时需注意一些注意事项。

-	缩放到的定价层的 `databases` 限制低于当前层：
	-	如果你使用的是默认 `databases` 数（对于所有定价层来说为 16），则不会丢失数据。
	-	如果你使用的是在你要缩放到的层的限制内的自定义 `databases` 数，则将保留此 `databases` 设置并且不会丢失数据。
	-	如果你使用的是超出新层限制的自定义 `databases` 数，则 `databases` 设置将降低到新层的限制，并且已删除数据库中的所有数据都将丢失。
-	所缩小到的定价层的 `databases` 限制等于或高于当前层时，将保留 `databases` 设置并且不会丢失数据。

注意，当标准和高级缓存有 99.9% 可用性 SLA 时，则没有数据丢失方面的 SLA。

### <a name="will-my-cache-be-available-during-scaling"></a> 在缩放过程中，缓存是否可用？

-	**标准**和**高级**缓存在缩放操作期间保持可用。
-	**基本**缓存在执行缩放为不同大小的缩放操作过程中处于脱机状态，但在从**基本**缓存缩放为**标准**缓存时仍然可用。

### <a name="operations-that-are-not-supported"></a> 不支持的操作

-	不能从较高的定价层缩放到较低的定价层。
    -    不能从**高级**缓存向下缩放到**标准**或**基本**缓存。
    -    不能从**标准**缓存向下缩放到**基本**缓存。
-	可以从**基本**缓存缩放为**标准**缓存，但不能同时更改大小。如果你需要不同大小，则可以执行后续缩放操作以缩放为所需大小。
-	不能从**基本**缓存直接缩放到**高级**缓存。必须在一个缩放操作中从**基本**缩放到**标准**，然后在后续的缩放操作中从**标准**缩放到**高级**。
-	不能从较大的大小减小为 **C0 (250 MB)** 大小。

如果缩放操作失败，该服务将尝试还原操作并且缓存将还原为初始大小。

### <a name="how-long-does-scaling-take"></a> 缩放需要多长时间？

缩放大约需要 20 分钟，具体取决于缓存中的数据量。

### <a name="how-can-i-tell-when-scaling-is-complete"></a> 如何判断缩放何时完成？

可以使用以下 PowerShell 命令来获取缓存的状态：

	Get-AzureRmRedisCache -ResourceGroupName myGroup -Name myRedisCache

在大约 20 分钟后，可以运行以下命令查看缓存大小是否已更改。

### <a name="why-is-this-feature-in-preview"></a> 为何此功能处于预览状态？

我们现在发布此功能是为了获取反馈。基于该反馈，我们将很快发布此功能的正式版本。





  
<!-- IMAGES -->
[redis-cache-pricing-tier-part]: ./media/cache-how-to-scale/redis-cache-pricing-tier-part.png

[redis-cache-pricing-tier-blade]: ./media/cache-how-to-scale/redis-cache-pricing-tier-blade.png

[redis-cache-scaling]: ./media/cache-how-to-scale/redis-cache-scaling.png

<!---HONumber=AcomDC_0718_2016-->