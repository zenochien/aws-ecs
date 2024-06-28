---
title : "Creating an Amazon ECS task for the EC2 launch type with the AWS CLI"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 4.3 </b> "
---

[Step 1: Create a Cluster](#step-1-create-a-cluster)

[Step 2: Launch an Instance with the Amazon ECS AMI](#step-2-launch-an-instance-with-the-amazon-ecs-ami)

[Step 3: Register a Task Definition](#step-3-register-a-task-definition)

[Step 4: Run a Task](#step-4-run-a-task)

#### Step 1: Create a Cluster

Create your own cluster with a unique name with the following command:

```
aws ecs create-cluster --cluster-name MyCluster
```

![Create Docker](/images/10/1.png?featherlight=false&width=90pc)

#### Step 2: Launch an Instance with the Amazon ECS AMI

You must have an Amazon ECS container instance in your cluster before you can run tasks on it. If you do not have any container instances in your cluster, see [Launching an Amazon ECS Linux container instance](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/launch_container_instance.html) for more information.

![Create Docker](/images/10/2.png?featherlight=false&width=90pc)
![Create Docker](/images/10/3.png?featherlight=false&width=90pc)
![Create Docker](/images/10/4.png?featherlight=false&width=90pc)
![Create Docker](/images/10/5.png?featherlight=false&width=90pc)
![Create Docker](/images/10/6.png?featherlight=false&width=90pc)
![Create Docker](/images/10/7.png?featherlight=false&width=90pc)


#### Step 3: Register a Task Definition

Before you can run a task on your ECS cluster, you must register a task definition. Task definitions are lists of containers grouped together. The following example is a simple task definition that uses a busybox image from Docker Hub and simply sleeps for 360 seconds. For more information about the available task definition parameters.

```
cd tasks
touch sleep360.json
vi sleep360.json
```

**Copy**

```
{
    "containerDefinitions": [
        {
            "name": "sleep",
            "image": "busybox",
            "cpu": 10,
            "command": [
                "sleep",
                "360"
            ],
            "memory": 10,
            "essential": true
        }
    ],
    "family": "sleep360"
}
```

The above example JSON can be passed to the AWS CLI in two ways: You can save the task definition JSON as a file and pass it with the --cli-input-json file://path_to_file.json option. Or, you can escape the quotation marks in the JSON and pass the JSON container definitions on the command line as in the below example. If you choose to pass the container definitions on the command line, your command additionally requires a --family parameter that is used to keep multiple versions of your task definition associated with each other.

To use a JSON file for container definitions:

```
aws ecs register-task-definition --cli-input-json file://$HOME/tasks/sleep360.json
```

![Create Docker](/images/10/8.png?featherlight=false&width=90pc)

To use a JSON string for container definitions:

```
aws ecs register-task-definition --family sleep360 --container-definitions "[{\"name\":\"sleep\",\"image\":\"busybox\",\"cpu\":10,\"command\":[\"sleep\",\"360\"],\"memory\":10,\"essential\":true}]"
```

![Create Docker](/images/10/9.png?featherlight=false&width=90pc)

#### Step 4: Run a Task

After you have registered a task for your account and have launched a container instance that is registered to your cluster, you can run the registered task in your cluster. For this example, you place a single instance of the sleep360:1 task definition in your default cluster.

```
aws ecs run-task --cluster default --task-definition sleep360:1 --count 1
```





