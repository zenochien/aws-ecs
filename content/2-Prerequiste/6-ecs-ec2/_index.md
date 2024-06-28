---
title : "Learn how to create an Amazon ECS Windows task for the EC2 launch type"
date :  "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 2.6 </b> "
---

#### Step 1: Create a cluster

1. On the **Clusters** page, choose **Create cluster**.

![Create Docker](/images/6/1.png?featherlight=false&width=90pc)

2. Under **Cluster configuration**, for **Cluster name**, enter a unique name.

![Create Docker](/images/6/2.png?featherlight=false&width=90pc)

3. To add Amazon EC2 instances to your cluster, expand Infrastructure, and then select **Amazon EC2 instances**.

![Create Docker](/images/6/2.png?featherlight=false&width=90pc)

4. To create a Auto Scaling group, from Auto Scaling group (ASG), select **Create new group**
- For **Operating system/Architecture**, choose the Amazon ECS-optimized AMI for the Auto Scaling group instances.
- For **EC2 instance type**, choose the instance type for your workloads. 
- For **SSH key pair**, choose the pair that proves your identity when you connect to the instance.

![Create Docker](/images/6/3.png?featherlight=false&width=90pc)

5. (Optional) To change the VPC and subnets where your tasks and services launch, under **Networking**
- To remove a subnet, under **Subnets**, choose **X** for each subnet that you want to remove.
- To change to a VPC other than the **default** VPC, under **VPC**, choose an existing VPC, and then under **Subnets**, select each subnet.

![Create Docker](/images/6/4.png?featherlight=false&width=90pc)

6. (Optional) To turn on Container Insights, expand **Monitoring**, and then turn on **Use Container Insights** and Choose **Create**.

![Create Docker](/images/6/5.png?featherlight=false&width=90pc)

#### Step 2: Register a task definition

1. In the navigation pane, choose Task Definitions and choose **Create new task definition**, **Create new task definition** with JSON.

![Create Docker](/images/6/6.png?featherlight=false&width=90pc)

2. Copy and paste the following example task definition into the box, and then choose **Create**.

```
{
    "containerDefinitions": [
        {
            "command": ["New-Item -Path C:\\inetpub\\wwwroot\\index.html -Type file -Value '<html> <head> <title>Amazon ECS Sample App</title> <style>body {margin-top: 40px; background-color: #333;} </style> </head><body> <div style=color:white;text-align:center> <h1>Amazon ECS Sample App</h1> <h2>Congratulations!</h2> <p>Your application is now running on a container in Amazon ECS.</p>'; C:\\ServiceMonitor.exe w3svc"],
            "entryPoint": [
                "powershell",
                "-Command"
            ],
            "essential": true,
            "cpu": 2048,
            "memory": 4096,
            "image": "mcr.microsoft.com/windows/servercore/iis:windowsservercore-ltsc2019",
            "name": "sample_windows_app",
            "portMappings": [
                {
                    "hostPort": 443,
                    "containerPort": 80,
                    "protocol": "tcp"
                }
            ]
        }
    ],
    "memory": "4096",
    "cpu": "2048",
    "family": "windows-simple-iis-2019-core",
    "executionRoleArn": "arn:aws:iam::012345678910:role/ecsTaskExecutionRole",
    "runtimePlatform": {"operatingSystemFamily": "WINDOWS_SERVER_2019_CORE"},
    "requiresCompatibilities": ["EC2"]
}
```

![Create Docker](/images/6/7.png?featherlight=false&width=90pc)
