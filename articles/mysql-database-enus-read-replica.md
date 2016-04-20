<properties linkid="" urlDisplayName="" pageTitle="MySQL Service Questions – Microsoft Azure Cloud" metaKeywords="Azure Cloud, technical documentation, documents and resources, MySQL, database, FAQ, Azure MySQL, MySQL PaaS, Azure MySQL PaaS, Azure MySQL Service, Azure RDS" description="Provides quick answers for common technical questions encountered by users when using MySQL Database on Azure. Contact technical support if you have any further questions." metaCanonical="" services="MySQL" documentationCenter="Services" title="" authors="" solutions="" manager="" editor="" />

<tags ms.service="mysql" ms.date="" wacn.date="04/01/2016"/>

# MySQL master-slave replication and read-only instances
MySQL Database on Azure supports the use of the replicate function to create slave instances for a MySQL instance. All changes to the master instance will be replicated to the slave instance. You can use this function as a simple way to implement flexible expansion and overcome the access restrictions on an individual database instance, thereby reducing the running load and increasing availability.

  For business intelligence reports (BI) or database warehouse solutions, users generally want to run service report queries on independent read-only instances (rather than generating read-only instances of the database). The replicate function can also be used to migrate databases between the generation environment and the development environment. You can also use the replicate function to improve the generation environment’s availability - disaster tolerance. If an error occurs, you can upgrade read-only instances to replace the invalid master instance and switch the workload, thereby ensuring the availability and continuity of services.

When you create a slave instance, Azure first performs a backup of the master instance, and then creates a new read-only instance based on the backup to serve as the slave instance. After this, Azure will continually replicate all changes to the master instance on the slave instance.


Notes:

1. MySQL Database on Azure only supports the creation of slave instances for MySQL 5.6; MySQL 5.5 is not supported.
2. The slave instance must use the same version of MySQL as the master instance. MySQL Database on Azure does not support replication between different versions of MySQL.
3. MySQL Database on Azure currently only supports slave instances in the same location (data center) as the master instance.
4. MySQL Database on Azure currently supports a maximum of 5 slave instances of the same master instance.
5. In order to ensure that data remains consistent between the master and slave instances, slave instances are read-only instances. All MySQL links on slave instances are read-only links. You cannot create, modify or delete databases or database accounts on the slave instance. You can perform these operations on the master instance, and the system will automatically synchronize them to the slave instance(s).


## Create read-only (slave) instances
1.	Select an existing instance and click on the “Replicate” page.
2.	Click on “Add slave instance” at the bottom and enter the name of the slave server. Confirm that you understand the effect on the charges and then click on “Submit”.
![Add slave instances][1]

Note: As MySQL Database on Azure performs a backup of the master instance during the process of creating slave instances, in order to prevent the backup from failing, please ensure that there are currently no queries or changes on the master instance that require a long time to run.

## Monitoring the replication status of slave instances
Once the slave instance has been created, you can monitor the status of replication between the master instance and slave instance(s) in a variety of ways.

Select the master instance and click on the “Replicate” page. This page shows all of the master instance’s slave instances and their statuses.

![Monitor replication][2]

Select a slave instance and click on the “Replicate” page. This page shows the replication status of the slave instance and the name of the corresponding master instance.
![Replicate][3]

## Implement read/write separation in applications
A Java sample program for read/write separation at the application end is shown below:


	package test1;
	
	import java.sql.Connection;
	import java.sql.ResultSet;
	import java.sql.Statement;
	import java.util.Properties;
	
	import com.mysql.jdbc.Driver;
	import com.mysql.jdbc.ReplicationDriver;;
	
	public class ConnectionDemo {
	
	  public static void main(String[] args) throws Exception {
		
	    ReplicationDriver driver = new ReplicationDriver();
	    String url = "jdbc:mysql:replication://address=(protocol=tcp)(type=master)(host=masterhost)(port=3306)(user=masteruser),address=(protocol=tcp)(type=slave)(host=slavehost)(port=3306)(user=slaveuser)/yourdb";
	    Properties props = new Properties();    
	    props.put("password", "yourpassword");
	    try (Connection conn = driver.connect(url, props))
	    {
	    	// Perform read/write work on the master
	        conn.setReadOnly(false);
	        conn.setAutoCommit(false);
	        conn.createStatement().executeUpdate("update t1 set id = id+1;");
	        conn.commit();    
	
	        // Set up connection to slave;
	        conn.setReadOnly(true);
	        
	        // Now, do a query from a slave
	        try (Statement statement = conn.createStatement())
	    	{
	    		ResultSet res = statement.executeQuery("show tables");
	    		System.out.println("There are below tables:");
	    		while (res.next()) {
	    			String tblName = res.getString(1);
	    			System.out.println(tblName);
	    		}
	    	} 
	    }
	  }
	}



## Upgrade slave instances
You can upgrade a slave instance to an online read/write instance. Once the instance is upgraded, changes on the master instance will no longer be replicated to this instance. You can perform read/write operations on this instance.
1.	Select the slave instance you need to upgrade and click on the “Replicate” page.
2.	Click on “Upgrade” at the bottom of the page and then click confirm.
![Upgrade slave instances][4]

## Master-slave replication FAQs
• I want to change the performance version of the master instance, for example by going up from MS4 to MS5 or going down from MS6 to MS5. Will the version for slave instances be updated as well? The performance version for slave instances will not change with that of the master instance, but you can change the performance version for slave instances separately if necessary.

• I would like to use the master-slave replication function, but my database instance is on version 5.5; what should I do? Master-slave replication only supports version 5.6. You can manually upgrade the database implementation to version 5.6 first, and then use the replicate function.

• How do I upgrade the database from version 5.5 to version 5.6? We do not currently support one-key upgrades. The way around this is to export the data from the original version 5.5 instance using mysqldump, create a new version 5.6 database server instance, and then import the data; once it passes compatibility testing, you can migrate the application to the version 5.6 instance. If your original database is the generation environment or you cannot accept any downtime, you can manually create a snapshot on the original database, restore it to a completely new instance, and then perform the migration and upgrade on the restored instance. This will reduce the impact on the original database.



<!--Image references-->

[1]: ./media/mysql-database-read-replica/replica1.jpg
[2]: ./media/mysql-database-read-replica/replica2.jpg
[3]: ./media/mysql-database-read-replica/replica3.jpg
[4]: ./media/mysql-database-read-replica/replica4.jpg

<!---HONumber=Acom_0418_2016_MySql-->
