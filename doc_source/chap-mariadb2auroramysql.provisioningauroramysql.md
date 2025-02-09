# Set up Aurora MySQL as a target database<a name="chap-mariadb2auroramysql.provisioningauroramysql"></a>

To provision Aurora MySQL as a target database, download the [AuroraMysql\_CF\.yaml template](https://aws-database-blog.s3.amazonaws.com/artifacts/mariadb-to-aurora-mysql-migration/AuroraMysql_CF.yaml)\. This template creates an Aurora MySQL database with required parameters\.

1. On the [AWS Management Console](https://console.aws.amazon.com), under **Services**, choose **CloudFormation**\.

1. Choose **Create stack**, and then choose **With new resources \(standard\)**\.

1. For **Specify template**, choose **Upload a template file**\.

1. Select **Choose file**\.

1. Choose the `AuroraMySQL.yaml` file\.

1. Choose **Next**\.

1. On the **Specify stack details** page, edit the predefined values as needed, and then choose **Next**:
   +  **Stack name** — Enter a name for the stack\.
   +  **CIDR** — Enter the CIDR IP range to access the instance\.
   +  **DBBackupRetentionPeriod** — The numbrer of days for backup retention\.
   +  **DBInstanceClass** — Enter the instance type of the database server\.
   +  **DBMasterPassword** — Enter the master password for the DB instance\.
   +  **DBMasterUsername** — Enter the master user name for the DB instance\.
   +  **DBName** — Enter the name of the database\.
   +  **DBSubnetGroup** — Enter the DB subnet group\.
   +  **Engine** — Enter the Aurora engine version; the default is `5.7.mysql-aurora.2.03.4`\.
   +  **PreferredBackupWindow** — Enter the daily time range in UTC during which you want to create automated backups\.
   +  **PreferredMaintenanceWindow** — Enter the weekly time range in UTC during which system maintenance can occur\.
   +  **VPCID** — Enter the ID for the VPC to launch your DB instance in\.

1. On the **Configure stack options** page, for **Tags**, specify any optional tags, and then choose **Next**\.

1. On the **Review** page, choose **Next**\.

1. Choose **Create stack**\.

After the Aurora MySQL database is created, log in to the Aurora MySQL instance:

```
$ mysql -h mysqltrg-instance-1.xxxxxxxxx.us-east-1.rds.amazonaws.com -u master -p migration -P 3306
MySQL [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| awsdms_control     |
| mysql              |
| performance_schema |
| source             |
| tmp                |
| webdb              |
+--------------------+
7 rows in set (0.001 sec)

MySQL [(none)]> create database migration;
Query OK, 1 row affected (0.016 sec)

MySQL [(none)]> use migration;
Database changed

MySQL [migration]> show tables;
Empty set (0.001 sec)
```

Use `mysql_tables_indexes.sql` to create table and index structures in Aurora MySQL\.

```
$ mysql -h mysqltrg-instance-1.xxxxxxxxx.us-east-1.rds.amazonaws.com  -u master -p migration -P 3306 < mysql_tables_indexes.sql
Enter password:
$
```

After the tables and indexes are successfully created, the next step is to set up and use AWS DMS\.