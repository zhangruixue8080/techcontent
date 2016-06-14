<properties linkid="" urlDisplayName="" pageTitle="MySQL Service Questions – Microsoft Azure Cloud" metaKeywords="Azure Cloud, technical documentation, documents and resources, MySQL, database, FAQ, Azure MySQL, MySQL PaaS, Azure MySQL PaaS, Azure MySQL Service, Azure RDS" description="Provides quick answers for common technical questions encountered by users when using MySQL Database on Azure. Contact technical support if you have any further questions." metaCanonical="" services="MySQL" documentationCenter="Services" title="" authors="v-chuw" solutions="" manager="RongYu" editor="" />

<tags ms.service="mysql" ms.date="" wacn.date="05/11/2016"/>

# MySQL on Azure service continuity solutions

This article summarizes a number of typical interruption events and corresponding solutions to protect service continuity with MySQL on Azure. It also introduces some key specific steps and indicators to recover from regional disasters.

## Overview ##
Service continuity refers to the flexible design, deployment, and execution of applications to avoid applications from being either temporarily or permanently unable to perform their service functions as a result of planned or unplanned interruption events. Unplanned interruption events include human error, temporary or permanent interruptions, and even regional disasters (which may cause a specific Azure region to suffer a large-scale loss of functionality). Planned events include redeploying applications in different regions and upgrading applications. The goal of service continuity is to reduce the effect of interruptions on application services and minimize the duration of the effects to avoid data loss.

Before we look at service continuity solutions, it is important to be familiar with the following concepts:

* Recovery time objective (RTO): The maximum acceptable time after an interruption incident occurs before the application fully recovers. RTO is used to measure the maximum availability loss during the outage.
* Recovery point objective (RPO): The maximum number of recent updates that may be lost (time intervals) after an interruption incident occurs before the application fully recovers. RPO is used to measure the maximum data loss during the outage.
* Estimated recovery time (ERT): The estimated length of time after a restore or failover request is sent before the database is fully available.

## Service continuity solutions ##
Following are common scenarios and corresponding solutions.

<table width="100%" border="1" cellspacing="0" cellpadding="0">
	<tr>
		<td>
			<b>Scenario</b>
		</td>
		<td>
			<b>Description </b>
		</td>
		<td>
			<b>Corresponding solution</b>
		</td>
	</tr>
	<tr>
		<td>
			Recover from service interruptions caused by human error
		</td>
		<td>
			An operational error by a database administrator in a production environment that causes the loss of some important data and requires rapid recovery.
		</td>
		<td>
			The backup and restore -- point-in-time restore feature of MySQL can roll back to any point in time during the last seven days. For the specific steps, see <a href="https://www.azure.cn/documentation/articles/mysql-database-point-in-time-restore" target="_blank">MySQL on Azure backup and restore—restore the database to any point in time</a>.
		</td>
	</tr>
	<tr>
		<td>
			Recover from service interruptions caused by a particular upgrade
		</td>
		<td>
			An operational error by a database administrator in a production environment that causes the loss of some important data and requires rapid recovery.
		</td>
		<td>
			Create a snapshot backup of the database before you upgrade. If the upgrade encounters a problem, you can then quickly restore the entire backup to the original instance or to a new instance. For the specific steps, see <a href="https://www.azure.cn/documentation/articles/mysql-database-point-in-time-restore" target="_blank">Backup and restore</a>.
		</td>
	</tr>
	<tr>
		<td>
			Recover from a regional disaster
		</td>
		<td>
			A regional disaster that causes widespread service interruptions from which you need to quickly perform offsite recovery.
		</td>
		<td>
			Two recovery solutions are provided so that you can choose the most appropriate solution based on the degree of urgency. If you are in a production environment, we recommend the self-service solution.<br>
			- Self-service solution: If a regional disaster occurs and you are unable to perform offsite recovery by using the Azure portal, you can use the PowerShell command line to restore the instance offsite.<br>
			- PaaS solution: The MySQL service will also perform offsite recovery of all affected instances when a serious regional disaster occurs. The key points are explained in the sections that follow.

		</td>
	</tr>
</table>

## Recover from a regional disaster ##

MySQL on Azure now provides offsite recovery features to help you maintain service continuity when regional disasters occur. Examples of a regional disaster might be the loss of power or a fire in a large area of computer rooms, an earthquake, or other unanticipated events.

### There are currently two solution types for disaster recovery: ###
 
* MySQL service-level disaster recovery: When a disaster occurs, MySQL on Azure services will initiate an emergency response and determine whether the cause of the disaster can be quickly restored (in a shorter time than the RPO). If it is not possible to perform a quick restore, MySQL on Azure will perform an offsite database restore on all affected instances. The point in time to be restored will be the closest possible restore point to the time at which the fault occurred.

* Self-service disaster recovery: If you are using a production environment with higher requirements in terms of recovery times, you can use the PowerShell command line to manually restore the affected instances offsite when a disaster occurs.


### The principle of offsite recovery: ###
If a disaster occurs, you can use self-service to designate the point in time that you want to restore to. Alternatively, the PaaS service restore solution will designate, by default, a point in time when the data that is affected by the interruption was intact. MySQL on Azure will then do everything possible to restore data based on this point in time or the closest restorable point in time for the instance’s designated region. The base level of MySQL on Azure will store the user data in Azure Blob storage and will use read-only access to cross-regional redundancy levels. This process makes sure that there are three data copies both locally and offsite and that the offsite copies are set to read-only access. When you perform an offsite restore operation and local storage is available, MySQL on Azure will copy the data from the local end to the target region based on the user’s designated point in time and then perform database recovery. If local storage is not available, MySQL on Azure will perform the database recovery offsite by using the data that's nearest to the designated point in time after storage-layer replication.

### Offsite recovery performance indicators: ###
ERT<3 hours，RPO< 1 hour. <br>
>[AZURE.NOTE] ERT, RTO, and RPO are project indicators that are intended only for reference purposes. These indicators only appear in regional disasters and are not part of the MySQL database service’s service level agreement (SLA).

### User self-service process: ###
If a disaster occurs and you can use the Azure portal, you can use the offsite restore process in [backup and restore](/documentation/articles/mysql-database-point-in-time-restore) to perform the operation. However, if regional disasters occur frequently, it will not be possible to obtain correct information on the instance in the Azure portal. In such a situation, we recommend that you perform an offsite restore operation on the instance by using PowerShell:

```
New-AzureRmResource -ResourceType "Microsoft.MySql/servers" -ResourceName <ResourceName> -ApiVersion 2015-09-01 -ResourceGroupName <ResourceGroupName> -Location <TargetLocation> -SkuObject @{name=<targetSKU>} -Properties @{creationSource=@{server='<SourceServerName>';region='<SourceLocation>';timepoint='<TimeTag>'};version = '<version number>'}
```

>[AZURE.NOTE] 
1. Timepoint is an optional value. If you do not enter this value, the default is the current point in time. If you want to enter this value, you must use the date-time format in JSON, for example, 2016-05-06T08:00:00. We use UTC time universally.<br>
2. Instances that were created by using the Azure portal will be allocated to the “Default-MySQL-ChinaNorth” and “Default-MySQL-ChinaEast” resource groups by default on the basis of geographic location.

After the restored instance has been created, you need to add the current IP to the firewall whitelist for the new instance. You will also need to manually update the connection strings for the applications and database with the hostname of the new instance to restore the application-layer services.

## Common service continuity problems ##
1. Is restoring from offsite copies supported?<br> Only offsite recovery is currently supported. We do not currently support offsite replication functions; that is, restoration from offsite copies is not supported.

<!---HONumber=Acom_0606_2016_MySql-->
