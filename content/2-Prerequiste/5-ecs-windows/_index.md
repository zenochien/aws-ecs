---
title : "Learn how to create an Amazon ECS Windows task for the Fargate launch type"
date :  "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 2.5 </b> "
---

#### Step 1: Create the cluster

1. Open the console at https://console.aws.amazon.com/ecs/v2.

![Create Docker](/images/5/1.png?featherlight=false&width=90pc)

2. On the **Clusters** page, choose **Create cluster**.

![Create Docker](/images/5/2.png?featherlight=false&width=90pc)

3. Under **Cluster configuration**, for **Cluster name**, enter **ecs-windows**.

![Create Docker](/images/5/3.png?featherlight=false&width=90pc)

4. (Optional) To turn on Container Insights, expand Monitoring, and then turn on Use Container Insights.

![Create Docker](/images/5/4.png?featherlight=false&width=90pc)

5. Choose **Create**.

![Create Docker](/images/5/5.png?featherlight=false&width=90pc)

#### Step 2: Register a Windows task definition

1. Choose **Create new task definition**, **Create new task definition with JSON**.

![Create Docker](/images/5/6.png?featherlight=false&width=90pc)

2. Copy and paste the following example task definition into the box and then choose **Create**.

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
                    "hostPort": 80,
                    "containerPort": 80,
                    "protocol": "tcp"
                }
            ]
        }
    ],
    "memory": "4096",
    "cpu": "2048",
    "networkMode": "awsvpc",
    "family": "windows-simple-iis-2019-core",
    "executionRoleArn": "arn:aws:iam::012345678910:role/ecsTaskExecutionRole",
    "runtimePlatform": {"operatingSystemFamily": "WINDOWS_SERVER_2019_CORE"},
    "requiresCompatibilities": ["FARGATE"]
}
```

![Create Docker](/images/5/7.png?featherlight=false&width=90pc)

#### Step 3: Create a service with your task definition

1. From the Services tab, choose Create.
![Create Docker](/images/5/9.png?featherlight=false&width=90pc)

2. Under **Deployment configuration**, specify how your application is deployed.

![Create Docker](/images/5/10.png?featherlight=false&width=90pc)

3. Under **Networking**, you can create a new security group or choose an existing security group for your task.


![Create Docker](/images/5/11.png?featherlight=false&width=90pc)

4. Choose **Create**.

![Create Docker](/images/5/12.png?featherlight=false&width=90pc)

#### Step 4: View your service
1. In the Services tab, under Service name

![Create Docker](/images/5/13.png?featherlight=false&width=90pc)

2. On the task page, in the Configuration section, under **Public IP**, choose Open address.

![Create Docker](/images/5/14.png?featherlight=false&width=90pc)