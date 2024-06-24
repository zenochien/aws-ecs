---
title : "Backup and restore"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 6. </b> "
---

#### Monitoring AWS RDS - Backup and Restore

1. On the AWS RDS interface, you can follow these steps to monitor:

   - Select **Databases**.
   - Choose the DB instance you've created.
   - Click on **Monitoring**.

   ![AWS RDS Monitoring](/images/5/00014.png?featherlight=false&width=90pc)
   ![AWS RDS Monitoring](/images/5/00015.png?featherlight=false&width=90pc)
   ![AWS RDS Monitoring](/images/5/00016.png?featherlight=false&width=90pc)
   ![AWS RDS Monitoring](/images/5/00017.png?featherlight=false&width=90pc)
   ![AWS RDS Monitoring](/images/5/00018.png?featherlight=false&width=90pc)

2. To view backup information for a DB instance in AWS RDS, follow these steps:

   - Log in to the AWS Management Console.
   - Select the "Amazon RDS" service from the list of services.
   - In the RDS dashboard, choose the DB instance you want to check.
   - On the DB instance management page, navigate to the "Maintenance & backups" tab.
   - Here, you can view information about automatic and manual backups. You can also configure and manage backup settings.

   ![AWS RDS Backup](/images/5/00019.png?featherlight=false&width=90pc)

3. View Snapshot Information.

   ![Snapshot Information](/images/5/00020.png?featherlight=false&width=90pc)

4. Select the DB snapshot you want to restore.

   - Under the Actions section, select **Restore snapshot**.

   ![Restore Snapshot](/images/5/00021.png?featherlight=false&width=90pc)

5. On the Restore snapshot page, enter a name for the DB instance you want to restore in the DB instance identifier field.

   - Choose other settings such as allocated memory size.
   - For more information on each setting, refer to [Settings for DB instances](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Limits.html#Limits.DBInstance).
   - Finally, select **Restore DB instance**.

   ![Restore Settings](/images/5/00022.png?featherlight=false&width=90pc)
   ![Restore Settings](/images/5/00023.png?featherlight=false&width=90pc)
   ![Restore Settings](/images/5/00024.png?featherlight=false&width=90pc)
   ![Restore Settings](/images/5/00025.png?featherlight=false&width=90pc)
   ![Restore Settings](/images/5/00026.png?featherlight=false&width=90pc)

6. Complete the restore snapshot process.

   ![Restore Complete](/images/5/00027.png?featherlight=false&width=90pc)

7. Verify that the database instance has been restored.

   ![Verify Restore](/images/5/00028.png?featherlight=false&width=90pc)
