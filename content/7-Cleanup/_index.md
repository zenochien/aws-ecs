---
title : "Clean up resources"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 7. </b> "
---

#### Resource Cleanup**

1. To delete the VPC and related resources:
   - Delete the DB subnet group.
   - Open the Amazon RDS console.
   - In the navigation pane, select Subnet groups.
   - Choose the DB subnet group related to the lab.
   - Select Delete, then choose Delete in the confirmation window.
   - Delete security groups.
   - Open the Amazon VPC console.
   - Choose VPC Dashboard, then select Security Groups.
   - Select the security group related to the lab.
   - Choose Actions, select Delete security groups, and then choose Delete to confirm.
   - Delete NAT gateway.
   - Open the Amazon VPC console.
   - Choose VPC Dashboard, then select NAT Gateways.
   - Select the NAT Gateway related to the lab.
   - Choose Actions, select Delete NAT gateway.
   - Confirm the deletion and select Delete.
   - Delete the VPC.
   - Open the Amazon VPC console.
   - Choose VPC Dashboard, then select VPC.
   - Select the VPC you want to delete.
   - From Actions, select Delete VPC.
   - On the confirmation page, enter delete and then choose Delete.

2. **Release the Elastic IP addresses**
   - Open the Amazon EC2 console.
   - Select Amazon EC2 Dashboard, then choose Elastic IPs.
   - Select the Elastic IP address related to the lab.
   - From Actions, select Release Elastic IP addresses.
   - On the confirmation page, choose Release.

3. **Terminate EC2 instance**
   - Access the EC2 Management Console.
   - In the left navigation bar, select Instances.
   - Select all EC2 Instances related to the lab.
   - Click Actions.
   - Click Manage Instance State.
   - Select Terminate.
   - Click Change State.

4. **Delete DB Instance**
   - Access the RDS Management Console.
   - In the left navigation bar, select Databases.
   - Select all DB Instances related to the lab.
   - Click Actions.
   - Click Delete.
   - Uncheck Create final snapshot? and acknowledge that automated backups, including system snapshots and point-in-time recovery, will no longer be available.
   - Enter delete me in the empty field.
   - Click Delete.

5. **Delete DB Snapshots**
   - Access the RDS Management Console.
   - In the left navigation bar, select Snapshots.
   - Select all snapshots related to the lab.
   - Click Actions.
   - Click Delete snapshot.
   - Click Delete.
