---
title : "Configuring Amazon ECS to listen for CloudWatch Events events"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 4.4 </b> "
---

#### Step 1: Create the Lambda function

1. Open the AWS Lambda console at https://console.aws.amazon.com/lambda/
![Create Docker](/images/11/1.png?featherlight=false&width=90pc)

2. Choose Create function.
![Create Docker](/images/11/2.png?featherlight=false&width=90pc)

3. On the Author from scratch screen, do the following:

- For Name, enter a value.

- For Runtime, choose your version of Python, for example, Python 3.9.

- For Role, choose Create a new role with basic Lambda permissions.

- Choose Create function.

![Create Docker](/images/11/3.png?featherlight=false&width=90pc)

4. In the **Function code** section, edit the sample code to match the following example:

```
import json

def lambda_handler(event, context):
    if event["source"] != "aws.ecs":
       raise ValueError("Function only supports input from events with a source type of: aws.ecs")
       
    print('Here is the event:')
    print(json.dumps(event))
```

![Create Docker](/images/11/4.png?featherlight=false&width=90pc)

#### Step 2: Register an event rule

**To route events to your Lambda function**

1. Open the CloudWatch console at https://console.aws.amazon.com/cloudwatch/

2. On the navigation pane, choose Events, Rules, Create rule.

![Create Docker](/images/11/5.png?featherlight=false&width=90pc)

![Create Docker](/images/11/6.png?featherlight=false&width=90pc)

3. For **Event Source**, choose **ECS** as the event source. By default, the rule applies to all Amazon ECS events for all of your Amazon ECS groups. Alternatively, you can select specific events or a specific Amazon ECS group.

![Create Docker](/images/11/7.png?featherlight=false&width=90pc)

![Create Docker](/images/11/8.png?featherlight=false&width=90pc)

![Create Docker](/images/11/9.png?featherlight=false&width=90pc)

![Create Docker](/images/11/10.png?featherlight=false&width=90pc)

4. For Targets, choose Add target, for Target type, choose Lambda function, and then select your Lambda function.

![Create Docker](/images/11/11.png?featherlight=false&width=90pc)

5. Choose **Next**.

![Create Docker](/images/11/12.png?featherlight=false&width=90pc)

6. For Rule definition, type a name and description for your rule and choose **Create rule**.

![Create Docker](/images/11/13.png?featherlight=false&width=90pc)

#### Step 3: Create a task definition

1. Open the console at https://console.aws.amazon.com/ecs/v2

2. In the navigation pane **Task Definitions**, choose **Create new Task Definition, Create new revision with JSON**.

![Create Docker](/images/11/14.png?featherlight=false&width=90pc)

4. Copy and paste the following example task definition into the box and then choose **Create**.

![Create Docker](/images/11/15.png?featherlight=false&width=90pc)

#### Step 4: Test your rule

1. Open the console at https://console.aws.amazon.com/ecs/v2

2. Choose Task definitions.

![Create Docker](/images/11/16.png?featherlight=false&width=90pc)

3. Choose console-sample-app-static, and then choose Deploy, Run new task.

![Create Docker](/images/11/17.png?featherlight=false&width=90pc)

4. For Cluster, choose default, and then choose Deploy.

![Create Docker](/images/11/18.png?featherlight=false&width=90pc)

![Create Docker](/images/11/19.png?featherlight=false&width=90pc)

5. Open the CloudWatch console at https://console.aws.amazon.com/cloudwatch/

6. On the navigation pane, choose Logs and select the log group for your Lambda function (for example, /aws/lambda/my-function).

7. Select a log stream to view the event data.





