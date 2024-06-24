---
title : "Learn how to create an Amazon ECS Linux task for the Fargate launch type"
date :  "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---

Amazon Elastic Container Service (Amazon ECS) is a highly scalable, fast, container management service that makes it easy to run, stop, and manage your containers. You can host your containers on a serverless infrastructure that is managed by Amazon ECS by launching your services or tasks on AWS Fargate. For more information on Fargate, see [AWS Fargate for Amazon ECS](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html).

Get started with Amazon ECS on AWS Fargate by using the Fargate launch type for your tasks in the Regions where Amazon ECS supports AWS Fargate.

Complete the following steps to get started with Amazon ECS on AWS Fargate.

#### Step 1: Create the cluster

1. Open the console at https://console.aws.amazon.com/ecs/v2.

![Create Docker](/images/4/1.png?featherlight=false&width=90pc)

2. On the **Clusters** page, choose **Create cluster**.

![Create Docker](/images/4/2.png?featherlight=false&width=90pc)

3. Under **Cluster configuration**, for **Cluster name**, enter a unique name.

![Create Docker](/images/4/3.png?featherlight=false&width=90pc)

4. (Optional) To turn on Container Insights, expand **Monitoring**, and then turn on **Use Container Insights**.

![Create Docker](/images/4/4.png?featherlight=false&width=90pc)

#### Step 2: Create a task definition

1. Choose Create new **Task Definition**, **Create new revision with JSON**.

![Create Docker](/images/4/5.png?featherlight=false&width=90pc)

2. Copy and paste the following example task definition into the box and then choose **Create**.

```
{
    "family": "sample-fargate", 
    "networkMode": "awsvpc", 
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

![Create Docker](/images/4/6.png?featherlight=false&width=90pc)

#### Step 3: Create the service

1. From the Services tab, choose Create.
![Create Docker](/images/4/7.png?featherlight=false&width=90pc)
![Create Docker](/images/4/8.png?featherlight=false&width=90pc)

2. Under **Deployment configuration**, specify how your application is deployed.

![Create Docker](/images/4/9.png?featherlight=false&width=90pc)

3. Under **Networking**, you can create a new security group or choose an existing security group for your task.

![Create Docker](/images/4/10.png?featherlight=false&width=90pc)

4. Choose **Create**.

![Create Docker](/images/4/11.png?featherlight=false&width=90pc)

#### Step 4: View your service
1. In the Services tab, under Service name

![Create Docker](/images/4/12.png?featherlight=false&width=90pc)

2. On the task page, in the Configuration section, under **Public IP**, choose Open address.

![Create Docker](/images/4/13.png?featherlight=false&width=90pc)


#### Step 5: Clean up

1. On the Clusters page, select the cluster you created.

![Create Docker](/images/4/14.png?featherlight=false&width=90pc)

2. At the confirmation prompt, enter delete and then choose **Delete cluster**.

![Create Docker](/images/4/15.png?featherlight=false&width=90pc)

3. Choose Delete Cluster. At the confirmation prompt, enter delete **delete Cluster-99**, and then choose Delete.

![Create Docker](/images/4/16.png?featherlight=false&width=90pc)
