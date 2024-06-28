---
title : "Creating an Amazon ECS Windows task for the Fargate launch type with the AWS CLI"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---

The following steps help you set up a cluster, register a task definition, run a Linux task, and perform other common scenarios in Amazon ECS with the AWS CLI. Use the latest version of the AWS CLI. For more information on how to upgrade to the latest version, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).

*Topics*

[Step 1: Create a Cluster](#step-1-create-a-cluster)

[Step 2: Register a Windows Task Definition](#step-2-register-a-windows-task-definition)

[Step 3: List task definitions](#step-3-list-task-definitions)

[Step 4: Create a service](#step-4-create-a-service)

[Step 5: List services](#step-5-list-services)

[Step 6: Describe the Running Service](#step-6-describe-the-running-service)

[Step 7: Clean Up](#step-7-clean-up)

#### Step 1: Create a Cluster

Create your own cluster with a unique name with the following command:

```
aws ecs create-cluster --cluster-name fargate-cluster
```
![Create Docker](/images/9/1.png?featherlight=false&width=90pc)

#### Step 2: Register a Windows Task Definition
Before you can run a Windows task on your Amazon ECS cluster, you must register a task definition. Task definitions are lists of containers grouped together. The following example is a simple task definition that creates a web app. For more information about the available task definition parameters, see [Amazon ECS task definitions](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definitions.html).

```
mkdir tasks
cd tasks
touch fargate-task-windows.json
vi fargate-task-windows.json
```

**Copy**
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
    "executionRoleArn": "arn:aws:iam::478371912360:role/ecsTaskExecutionRole",
    "runtimePlatform": {"operatingSystemFamily": "WINDOWS_SERVER_2019_CORE"},
    "requiresCompatibilities": ["FARGATE"]
}
```

![Create Docker](/images/9/2.png?featherlight=false&width=90pc)

The above example JSON can be passed to the AWS CLI in two ways: You can save the task definition JSON as a file and pass it with the `--cli-input-json file://path_to_file.json` option.

To use a JSON file for container definitions:

```
aws ecs register-task-definition --cli-input-json file://$HOME/tasks/fargate-task-windows.json
```

![Create Docker](/images/9/3.png?featherlight=false&width=90pc)

#### Step 3: List task definitions

You can list the task definitions for your account at any time with the **list-task-definitions** command. The output of this command shows the family and revision values that you can use together when calling **run-task** or **start-task**.

```
aws ecs list-task-definitions
```

![Create Docker](/images/9/4.png?featherlight=false&width=90pc)

#### Step 4: Create a service

After you have registered a task for your account, you can create a service for the registered task in your cluster. For this example, you create a service with one instance of the sample-fargate:1 task definition running in your cluster. The task requires a route to the internet, so there are two ways you can achieve this. One way is to use a private subnet configured with a NAT gateway with an elastic IP address in a public subnet. Another way is to use a public subnet and assign a public IP address to your task. We provide both examples below. 

Example using a private subnet.

```
aws ecs create-service --cluster fargate-cluster --service-name fargate-service --task-definition windows-simple-iis-2019-core:4 --desired-count 1 --launch-type "FARGATE" --network-configuration "awsvpcConfiguration={subnets=subnet-0d9c29afae87d70bf,securityGroups=sg-01601d1110935869f}"
```

Example using a public subnet.

```
aws ecs create-service --cluster fargate-cluster --service-name fargate-service --task-definition windows-simple-iis-2019-core:4 --desired-count 1 --launch-type "FARGATE" --network-configuration "awsvpcConfiguration={subnets=subnet-0d9c29afae87d70bf,securityGroups=sg-01601d1110935869f,assignPublicIp=ENABLED}"
```

![Create Docker](/images/9/5.png?featherlight=false&width=90pc)

#### Step 5: List services

List the services for your cluster. You should see the service that you created in the previous section. You can take the service name or the full ARN that is returned from this command and use it to describe the service later.

```
aws ecs list-services --cluster fargate-cluster
```

![Create Docker](/images/9/6.png?featherlight=false&width=90pc)

#### Step 6: Describe the Running Service

Describe the service using the service name retrieved earlier to get more information about the task.

```
aws ecs describe-services --cluster fargate-cluster --services fargate-service
```

![Create Docker](/images/9/7.png?featherlight=false&width=90pc)

#### Step 7: Clean Up

When you are finished with this tutorial, you should clean up the associated resources to avoid incurring charges for unused resources.

Delete the service.

```
aws ecs delete-service --cluster fargate-cluster --service fargate-service --force
```

Delete the cluster.

```
aws ecs delete-cluster --cluster fargate-cluster
```














