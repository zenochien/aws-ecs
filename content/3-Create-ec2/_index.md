---
title : "Create EC2 instance"
date :  "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

#### Creating an AWS Instance

To create a Linux instance using AWS Management Console, follow the instructions below. This guide is designed to help you quickly create your first instance, so it doesn't include all possible options. For information on advanced options, refer to the [Launch Instance documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html).

1. **Access AWS Console**

   - Open a web browser and go to the Amazon EC2 console at [https://console.aws.amazon.com/ec2/](https://console.aws.amazon.com/ec2/).

2. **Choose Launch instance**

   - On the EC2 console dashboard, in the **Launch instance** box, select **Launch instance**, then choose **Launch instance** from the options that appear.

   ![Create a VPC](/images/3/0001.png?featherlight=false&width=90pc)

3. **Name your instance**

   - Under the **Name and tags** section, for **Name**, enter a descriptive name for your instance.

   ![Create a VPC](/images/3/0002.png?featherlight=false&width=90pc)

4. **Select an image (Amazon Machine Image - AMI)**

   - Under **Application and OS Images (Amazon Machine Image)**, follow these steps:
     - Choose **Quick Start**, then select **Amazon Linux**. This is the operating system (OS) for your instance.
     - From the **Amazon Machine Image (AMI)**, choose an HVM version of **Amazon Linux 2023**. Note that these AMIs are marked as **Free tier eligible**. An Amazon Machine Image (AMI) is a basic configuration used as a template for your instance.

   ![Create a VPC](/images/3/0003.png?featherlight=false&width=90pc)

5. **Choose instance type**

   - Under the **Instance type** section, from the **Instance type** list, you can select the hardware configuration for your instance. Choose the **t2.micro** instance type, which is pre-selected by default. The **t2.micro** instance type qualifies for free usage in the AWS Free Tier. In regions where **t2.micro** isn't available, you can use **t3.micro** in the AWS Free Tier. For more information, refer to [AWS Free Tier](https://aws.amazon.com/free/).

   ![Create a VPC](/images/3/0004.png?featherlight=false&width=90pc)

6. **Choose Key Pair**

   - Under the **Key pair (login)** section, for **Key pair name**, select the key pair you created during setup.

   - **Warning**: Do not select **Proceed without a key pair (Not recommended)**. If you launch an instance without a key pair, you won't be able to connect to it.

   ![Create a VPC](/images/3/0005.png?featherlight=false&width=90pc)

7. **Configure Security Group**

   - Under **Network settings**, select **Edit**. For **Security group name**, you'll see that the guide has created and selected a security group for you. You can use this security group or choose one you created during setup using the following steps:

     - Select **Select existing security group**.
     - From **Common security groups**, choose your security group from the list of available security groups.

   ![Create a VPC](/images/3/0006.png?featherlight=false&width=90pc)

   ![Create a VPC](/images/3/0007.png?featherlight=false&width=90pc)

8. **Review and launch the instance**

   - Keep the default choices for other settings of your instance.
   - Review a summary of your instance configuration in the **Summary panel**, and when you're ready, choose **Launch instance**.

   ![Create a VPC](/images/3/0008.png?featherlight=false&width=90pc)

9. **Confirmation and verification**

   - A confirmation page will indicate that your instance is starting. Select **View all instances** to close the confirmation page and return to the console interface.
   - On the Instances screen, you can see the status of the startup process. There is a brief time for the instance to start. When you launch an instance, its initial state is **pending**. After the instance starts, its status will change to **running**, and it will receive a public DNS name. If the **Public IPv4 DNS** column is hidden, select the gear icon (Settings) in the upper right corner, enable **Public IPv4 DNS**, and select **Confirm**.
   - It may take a few minutes for the instance to be ready for you to connect to. Check if your instance has passed the status check; you can see this information in the **Status check** column.

   ![Create a VPC](/images/3/0009.png?featherlight=false&width=90pc)

   ![Create a VPC](/images/3/00010.png?featherlight=false&width=90pc)

#### Connecting to EC2 Instance via SSH using Mobaxterm

To connect to an Amazon Elastic Compute Cloud (EC2) instance via SSH using Mobaxterm, follow these steps:

1. **Download and Install Mobaxterm**:

   - First, download Mobaxterm from the official website: [Mobaxterm Website](https://mobaxterm.mobatek.net/download-home-edition.html).
   - After downloading, install Mobaxterm on your computer.

2. **Open Mobaxterm**:

   - Launch Mobaxterm after installation.

3. **Create a New SSH Connection**:

   - In Mobaxterm, you can create a new SSH connection by clicking the **Session** icon in the upper-left corner of the interface.

4. **Configure the SSH Connection**:

   - In the configuration window, enter the following information:
     - **Remote Host**: Enter the IP address or domain name of the EC2 instance you want to connect to.
     - **Port**: The default is 22 (SSH port).
     - **User**: The username on the EC2 instance (usually **ec2-user** or **ubuntu**).
     - **Advanced SSH settings**: Here, you can provide an SSH key (private key) if you're using key-based authentication instead of a password.

   ![Create a VPC](/images/3/00011.png?featherlight=false&width=90pc)

5. **Connect to the EC2 Instance**:

   - Click **OK** to save the configuration, and then click the connect icon to establish an SSH connection to your EC2 instance.

6. **Enter Password (if applicable)**:

   - If you're using a password instead of an SSH key, Mobaxterm will prompt you to enter the user's password on the EC2 instance.

7. **Successful Connection**:

   - If all the information is correct, you will successfully connect to the EC2 instance and can perform remote server tasks.

   - Remember that you need to have access permissions to the EC2 instance and have configured its Security Group to allow SSH connections from your IP address.

   ![Create a VPC](/images/3/00012.png?featherlight=false&width=90pc)
