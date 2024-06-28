---
title : "Creating an Amazon ECS Linux task for the Fargate launch type with the AWS CLI"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---

The following steps help you set up a cluster, register a task definition, run a Linux task, and perform other common scenarios in Amazon ECS with the AWS CLI. Use the latest version of the AWS CLI. For more information on how to upgrade to the latest version, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).

*Topics*

[Prerequisites](#prerequisites)

[Step 1: Create a Cluster](#step-1-create-a-cluster)

[Step 2: Register a Linux Task Definition](#step-1-create-a-cluster)

[Step 3: List Task Definitions](#step-2-register-a-linux-task-definition)

[Step 4: Create a Service](#step-3-list-task-definitions)

[Step 5: List Services](#step-5-list-services)

[Step 6: Describe the Running Service](#step-6-describe-the-running-service)

[Step 7: Test](#step-7-test)

[Step 8: Clean Up](#step-8-clean-up)

#### Prerequisites

This tutorial assumes that the following prerequisites have been completed.

- The latest version of the AWS CLI is installed and configured. For more information about installing or upgrading your AWS CLI, see Installing the AWS Command Line Interface.

- The steps in Set up to use Amazon ECS have been completed.

- Your AWS user has the required permissions specified in the **AmazonECS_FullAcces**s IAM policy example.

- You have a VPC and security group created to use. This tutorial uses a container image hosted on Amazon ECR Public so your task must have internet access. To give your task a route to the internet, use one of the following options.

    - Use a private subnet with a **NAT gateway** that has an elastic IP address.

    - Use a public subnet and assign a public IP address to the task.

- For more information, see Create a **virtual private cloud**.

- For information about security groups and rules, see, Default security groups for your VPCs and Example rules in the Amazon Virtual Private Cloud User Guide.

- If you follow this tutorial using a private subnet, you can use Amazon ECS Exec to directly interact with your container and test the deployment. You will need to create a task IAM role to use ECS Exec. For more information on the task IAM role and other prerequisites, see Using Amazon ECS Exec for debugging.

- (Optional) AWS CloudShell is a tool that gives customers a command line without needing to create their own EC2 instance. For more information, see What is AWS CloudShell? in the AWS CloudShell User Guide.


#### Step 1: Create a Cluster

Create your own cluster with a unique name with the following command:

```
aws ecs create-cluster --cluster-name fargate-cluster
```

**Output**

```
{
    "cluster": {
        "clusterArn": "arn:aws:ecs:ap-southeast-1:478371912360:cluster/fargate-cluster",
        "clusterName": "fargate-cluster",
        "status": "ACTIVE",
        "registeredContainerInstancesCount": 0,
        "runningTasksCount": 0,
        "pendingTasksCount": 0,
        "activeServicesCount": 0,
        "statistics": [],
        "tags": [],
        "settings": [
            {
                "name": "containerInsights",
                "value": "disabled"
            }
        ],
        "capacityProviders": [],
        "defaultCapacityProviderStrategy": []
    }
}
```
![Create Docker](/images/8/1.png?featherlight=false&width=90pc)


#### Step 2: Register a Linux Task Definition

Before you can run a task on your ECS cluster, you must register a task definition. Task definitions are lists of containers grouped together. The following example is a simple task definition that creates a PHP web app using the httpd container image hosted on Docker Hub. For more information about the available task definition parameters, see Amazon ECS task definitions. For this tutorial, the taskRoleArn is only needed if you are deploying the task in a private subnet and wish to test the deployment. Replace the taskRoleArn with the IAM task role you created to use ECS Exec as mentioned in Prerequisites.

```
mkdir tasks
cd tasks
touch fargate-task.json
vi fargate-task.json
```

**Copy**

```
 {
        "family": "sample-fargate",
        "networkMode": "awsvpc",
        "taskRoleArn": "arn:aws:iam::aws_account_id:role/execCommandRole", 
        "containerDefinitions": [
            {
                "name": "fargate-app",
                "image": "public.ecr.aws/docker/library/httpd:latest",
                "portMappings": [
                    {
                        "containerPort": 80,
                        "hostPort": 80,
                        "protocol": "tcp"
                    }
                ],
                "essential": true,
                "entryPoint": [
                    "sh",
                    "-c"
                ],
                "command": [
                    "/bin/sh -c \"echo '<html> <head> <title>Amazon ECS Sample App</title> <style>body {margin-top: 40px; background-color: #333;} </style> </head><body> <div style=color:white;text-align:center> <h1>Amazon ECS Sample App</h1> <h2>Congratulations!</h2> <p>Your application is now running on a container in Amazon ECS.</p> </div></body></html>' >  /usr/local/apache2/htdocs/index.html && httpd-foreground\""
                ]
            }
        ],
        "requiresCompatibilities": [
            "FARGATE"
        ],
        "cpu": "256",
        "memory": "512"
}
```

Save the task definition JSON as a file and pass it with the `--cli-input-json file://path_to_file.json` option. 

To use a JSON file for container definitions:

```
aws ecs register-task-definition --cli-input-json file://$HOME/tasks/fargate-task.json
```

![Create Docker](/images/8/3.png?featherlight=false&width=90pc)

#### Step 3: List Task Definitions

You can list the task definitions for your account at any time with the **list-task-definitions** command. The output of this command shows the family and revision values that you can use together when calling **run-task** or **start-task**.

```
aws ecs list-task-definitions
```

**Output**
```
{
    "taskDefinitionArns": [
        "arn:aws:ecs:ap-southeast-1:478371912360:task-definition/sample-fargate:1",
        "arn:aws:ecs:ap-southeast-1:478371912360:task-definition/sample-fargate:2",
        "arn:aws:ecs:ap-southeast-1:478371912360:task-definition/sample-fargate:3",
        "arn:aws:ecs:ap-southeast-1:478371912360:task-definition/windows-simple-iis-2019-core:2",
        "arn:aws:ecs:ap-southeast-1:478371912360:task-definition/windows-simple-iis-2019-core:3"
    ]
}
```
![Create Docker](/images/8/4.png?featherlight=false&width=90pc)

#### Step 4: Create a Service

After you have registered a task for your account, you can create a service for the registered task in your cluster. For this example, you create a service with one instance of the sample-fargate:1 task definition running in your cluster. The task requires a route to the internet, so there are two ways you can achieve this. One way is to use a private subnet configured with a NAT gateway with an elastic IP address in a public subnet. Another way is to use a public subnet and assign a public IP address to your task. We provide both examples below. 

Example using a private subnet. The `enable-execute-command` option is needed to use Amazon ECS Exec.

```
aws ecs create-service --cluster fargate-cluster --service-name fargate-service --task-definition sample-fargate:3 --desired-count 1 --launch-type "FARGATE" --network-configuration "awsvpcConfiguration={subnets=subnet-0d9c29afae87d70bf,securityGroups=sg-01601d1110935869f}" --enable-execute-command
```

![Create Docker](/images/8/5.png?featherlight=false&width=90pc)

Example using a public subnet.

```
aws ecs create-service --cluster fargate-cluster --service-name fargate-service --task-definition sample-fargate:3 --desired-count 1 --launch-type "FARGATE" --network-configuration "awsvpcConfiguration={subnets=subnet-0d9c29afae87d70bf,securityGroups=sg-01601d1110935869f,assignPublicIp=ENABLED}"
```

![Create Docker](/images/8/6.png?featherlight=false&width=90pc)


#### Step 5: List Services

List the services for your cluster. You should see the service that you created in the previous section. You can take the service name or the full ARN that is returned from this command and use it to describe the service later.

```
aws ecs list-services --cluster fargate-cluster
```

Output

```
{
    "serviceArns": [
        "arn:aws:ecs:ap-southeast-1:478371912360:service/fargate-cluster/fargate-service"
    ]
}
```
![Create Docker](/images/8/7.png?featherlight=false&width=90pc)

#### Step 6: Describe the Running Service

Describe the service using the service name retrieved earlier to get more information about the task.

```
aws ecs describe-services --cluster fargate-cluster --services fargate-service
```

![Create Docker](/images/8/8.png?featherlight=false&width=90pc)

#### Step 7: Test

**Testing task deployed using public subnet**

First, get the task ARN.

```
aws ecs list-tasks --cluster fargate-cluster --service fargate-service
```

![Create Docker](/images/8/9.png?featherlight=false&width=90pc)

Describe the task and locate the ENI ID. Use the task ARN for the tasks parameter.

```
aws ecs describe-tasks --cluster fargate-cluster --tasks arn:aws:ecs:us-east-1:123456789012:task/service/EXAMPLE
```

![Create Docker](/images/8/10.png?featherlight=false&width=90pc)

**Testing task deployed using private subnet**

Describe the task and locate managedAgents to verify that the `ExecuteCommandAgent` is running. Note the `privateIPv4Address` for later use.

```
aws ecs describe-tasks --cluster fargate-cluster --tasks arn:aws:ecs:ap-southeast-1:478371912360:task/fargate-cluster/427343222356404997dabf2dbdb2df8a
```

![Create Docker](/images/8/11.png?featherlight=false&width=90pc)

#### Step 8: Clean Up

When you are finished with this tutorial, you should clean up the associated resources to avoid incurring charges for unused resources.

**Delete the service.**
```
aws ecs delete-service --cluster fargate-cluster --service fargate-service --force
```

**Delete the cluster.**
```
aws ecs delete-cluster --cluster fargate-cluster
```