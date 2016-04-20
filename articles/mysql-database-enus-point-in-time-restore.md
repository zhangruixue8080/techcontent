<properties linkid="" urlDisplayName="" pageTitle="MySQL Service Questions – Microsoft Azure Cloud" metaKeywords="Azure Cloud, technical documentation, documents and resources, MySQL, database, FAQ, Azure MySQL, MySQL PaaS, Azure MySQL PaaS, Azure MySQL Service, Azure RDS" description="Provides quick answers for common technical questions encountered by users when using MySQL Database on Azure. Contact technical support if you have any further questions." metaCanonical="" services="MySQL" documentationCenter="Services" title="" authors="v-chuw" solutions="" manager="RongYu" editor="" />

<tags ms.service="mysql" ms.date="" wacn.date="04/07/2016"/>

# Backup and restore MySQL Database on Azure

MySQL Database on Azure supports full backups (also known as snapshot backups) and incremental backups. Space used for backups does not count towards the storage limit to the database. You can go back to any time point in the last 7 days or any specific snapshot backup stored in the system.

## **Full backups**

MySQL Database on Azure automatically backs up your MySQL Database every day at the time that you specified. This is a full backup. Full backups are retained for 30 days.

You can also instantly perform a manual full backup up. You can generate a total of 10 manual full backups. Manual full backups are not automatically removed, so once you reach the limit, you will need to delete a manual full backup before you can create a new manual full backup.

Instant manual backup process:

1. Log in to the [Azure Management Portal](http://manage.windowsazure.cn/) and click on “MYSQL DATABASE ON AZURE” in the services list on the left.
2.	In the server list, click on the database server you want to restore to enter the server interface, then click on the “Backup” page.
3.	Press the “Instant backup” button and confirm the operation.

## **Incremental backups**

MySQL Database on Azure automatically backs up newly added sections (or changed sections) of the database for the last 7 days to in order to enable rollback based on any point in time. You do not need to do anything for this to happen.

## **Restore the database to any point in time**

MySQL Database on Azure supports restoring to any time point in the last 7 days.

Process for restoring to a particular time point:

1. Log in to the [Azure Management Portal](http://manage.windowsazure.cn/) and click on “MYSQL DATABASE ON AZURE” in the services list on the left.
2. In the server list, click on the database server you want to restore to enter the server interface. Click on the “Operation panel” page.
![Restore to any point in time][1]
3. Press the “Restore” button in the task menu at the bottom. A dialogue box will pop up.
4. The “Restore type” is already set to “Return to a time point” by default. Select the time point, the target database server instance and the version that you want to restore. Then press the “Finish” button.
![Restore to any point in time][2]
5. Go back to the server list page to confirm that the server was successfully created (the status has changed to “Running”). The data recovery process may take anything from a few minutes to several hours, depending on the time point you choose and the amount of data added to the database server that day.

You can access the new server instance using the original server instance account. However, you will need to change the server name prefix in the account to the new server name.

## **Restore a full backup to a new instance**

MySQL Database on Azure supports restoring a full backup to a new instance. The specific procedure for this is as follows:

1.	Log in to the [Azure Management Portal](http://manage.windowsazure.cn/) and click on “MYSQL DATABASE ON AZURE” in the services list on the left.
2.	In the server list, click on the database server you want to restore to enter the server interface, then click on the “Backup” page.
3.	Select the full backup you want to restore and press the “Restore” button.
![Perform a full backup to a new instance][3]
4.	In the dialogue box that pop up, the “Restore type” is set to “Return to a full backup” by default, the “Selected backup” is automatically set to the full backup selected, and “Restore as” is set to “New service” by default. Enter the name and version of the new service instance. Then press the “Finish” button.
![Perform a full backup to a new instance][4]
5.	Go back to the server list to confirm that the server was successfully created (the status has changed to “Running”). This process is generally completed within 2 minutes. You can access the new server instance using the original server instance account. However, you will need to change the server name prefix in the account to the new server name.

## **Restore a full backup to the original instance**

MySQL Database on Azure supports restoring a full backup to the original instance. The specific procedure for this is as follows:

1.	Log in to the [Azure Management Portal](http://manage.windowsazure.cn/) and click on “MYSQL DATABASE ON AZURE” in the services list on the left.
2.	In the server list, click on the database server you want to restore to enter the server interface, then click on the “Backup” page.
3.	Select the full backup you want to restore and press the “Restore” button.
4.	Select “Current service” from the “Restore as” options in the dialogue box that pops up. At the same time, you must confirm that “I understand that restoring the server will take several minutes, and that I will be unable to access the database server during this time.” Then press the “Finish” button.
![Perform a full backup to the original instance][5]
5.	Go back to the server list to confirm that the status of the server has changed to “Running”. This indicates that the server was successfully created.

<!--Image references-->

[1]: ./media/mysql-database-point-in-time-restore/Restore1.jpg
[2]: ./media/mysql-database-point-in-time-restore/Restore2.jpg
[3]: ./media/mysql-database-point-in-time-restore/Restore3.jpg
[4]: ./media/mysql-database-point-in-time-restore/Restore4.jpg
[5]: ./media/mysql-database-point-in-time-restore/Restore5.jpg

<!---HONumber=Acom_0418_2016_MySql-->