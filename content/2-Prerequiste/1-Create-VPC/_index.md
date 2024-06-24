---
title : "Create a VPC"
date :  "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

#### Creating a VPC, Subnets, and VPC Resources Using the Console

Use the following procedure to create a VPC along with additional VPC resources you need to run your application, such as subnets, route tables, Internet gateways, and NAT gateways. For VPC configuration examples, refer to the VPC Examples.

1. Open the Amazon VPC console [here](https://console.aws.amazon.com/vpc/).

![Create a VPC](/images/2/1.png?featherlight=false&width=90pc)

2. On the VPC dashboard, choose **Create VPC.**

3. For **Resources to create**, choose **VPC and more.**

![Create a VPC](/images/2/2.png?featherlight=false&width=90pc)

4. Keep the **Name tag auto-generation** option selected to create Name tags for VPC resources, or deselect it to provide your own Name tags for VPC resources.

5. For the IPv4 CIDR block range, enter an IPv4 address range for your VPC. A VPC must have an IPv4 address range.

6. (Optional) To support IPv6 traffic, select **IPv6 CIDR block**, an IPv6 range provided by Amazon.

7. Choose a Tenancy option. This option determines whether the EC2 instances you launch into the VPC will run on hardware shared with other AWS accounts or on dedicated hardware for your use. If you select VPC Tenancy as **Default**, EC2 instances launched into this VPC will use the Tenancy attribute you specify when launching instances - See more details in the Amazon EC2 User Guide for Linux Instances. If you select VPC Tenancy as **Dedicated**, the instances will always run as Dedicated Instances on hardware dedicated to you. If you're using AWS Outposts, your Outpost must have private connectivity; you must use the default Tenancy.

8. For **Number of Availability Zones (AZs)**, we recommend providing subnets in at least two Availability Zones for a production environment. To select AZs for your subnets, expand **Customize AZs**. Otherwise, let AWS choose for you.

![Create a VPC](/images/2/3.png?featherlight=false&width=90pc)

9. To configure your subnets, select values for **Number of public subnets** and **Number of private subnets.** To select IP address ranges for your subnets, expand **Customize subnets CIDR blocks.** Otherwise, let AWS choose for you.

10. (Optional) If resources in a private subnet need public internet access via IPv4, for NAT gateways, choose the number of AZs you want to create NAT gateways in. In a production environment, we recommend deploying a NAT gateway in each AZ with resources that need public internet access. Note that there are associated costs with NAT gateways. For more information, see Pricing.

11. (Optional) If resources in a private subnet need public internet access via IPv6, for Egress-only Internet Gateway, select **Yes.**

12. (Optional) If you need direct access to Amazon S3 from your VPC, select **VPC endpoints, S3 Gateway.** This creates a VPC endpoint for Amazon S3. For more information, see VPC Endpoint Services in the AWS PrivateLink User Guide.

13. (Optional) For DNS options, both domain resolution options are enabled by default. If the defaults don't meet your needs, you can disable these options.

14. (Optional) To add a tag to your VPC, expand **Additional tags,** choose **Add new tag,** and enter a tag key and tag value.

15. In the **Preview** pane, you can see the relationship diagram of the VPC resources you've configured. Solid lines represent relationships between resources. Dotted lines represent network traffic to NAT gateways, Internet gateways, and gateway endpoints. After creating the VPC, you can view your VPC resources in this format anytime using the Resource Map tab. For more information, see Viewing Resource Maps of Your VPC Resources.

16. When you've completed configuring your VPC, choose **Create VPC.**

![Create a VPC](/images/2/4.png?featherlight=false&width=90pc)

#### Create a security group

Security groups act as a firewall for associated container instances, controlling both inbound and outbound traffic at the container instance level. You can add rules to a security group that enable you to connect to your container instance from your IP address using SSH. You can also add rules that allow inbound and outbound HTTP and HTTPS access from anywhere. Add any rules to open ports that are required by your tasks. Container instances require external network access to communicate with the Amazon ECS service endpoint. 

1. Open the Amazon VPC console [https://console.aws.amazon.com/vpc/](https://console.aws.amazon.com/vpc/).

2. In the navigation pane, choose **Security group**.

![Create a SG](/images/2/5.png?featherlight=false&width=90pc)

3. Select your name and choose VPC

![Create a SG](/images/2/6.png?featherlight=false&width=90pc)

4. Select **Add rule** on Inbound rule

HTTP rule: 
- Type: HTTP
- Source: Anywhere (0.0.0.0/0)

>> This option automatically adds the 0.0.0.0/0 IPv4 CIDR block as the source. This is acceptable for a short time in a test environment, but it's unsafe in production environments. In production, authorize only a specific IP address or range of addresses to access your instance.

HTTPS rule

- Type: HTTPS
- Source: Anywhere (0.0.0.0/0)

>> This is acceptable for a short time in a test environment, but it's unsafe in production environments. In production, authorize only a specific IP address or range of addresses to access your instance.

SSH rule

- Type: SSH

>> Source: Custom, specify the public IP address of your computer or network in CIDR notation. To specify an individual IP address in CIDR notation, add the routing prefix /32. For example, if your IP address is 203.0.113.25, specify 203.0.113.25/32. If your company allocates addresses from a range, specify the entire range, such as 203.0.113.0/24.

**Important**: *For security reasons, we don't recommend that you allow SSH access from all IP addresses **(0.0.0.0/0)** to your instance, except for testing purposes and only for a short time.*

![Create a SG](/images/2/7.png?featherlight=false&width=90pc)

5. Select **Create sucurity group** that complete

![Create a SG](/images/2/8.png?featherlight=false&width=90pc)
![Create a SG](/images/2/9.png?featherlight=false&width=90pc)
