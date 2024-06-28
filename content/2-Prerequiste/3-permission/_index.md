---
title : "Create Role for IAM Permission"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---

#### Step 1: Create the Role Name "execCommandRole"

1. Go to the IAM Console:
- Open the [IAM Console](https://console.aws.amazon.com/iam/).

2. Create a New Role:
- Click on "**Roles**" in the left-hand menu.
- Click the "Create role" button.

3. Select Trusted Entity:
- Choose "**AWS service**".
- Select "**Elastic Container Service**".
- Choose "**Elastic Container Service Task**" and click "**Next: Permissions**".

#### Step 2: Attach Permissions Policies

1. Attach Policies:
- Attach the **AmazonECSTaskExecutionRolePolicy** to the role.
- Optionally, attach any other policies your task might need, such as **AmazonEC2ContainerRegistryReadOnly** if your task needs to pull images from ECR.

2. Click "**Next: Tags**":
- Optionally add tags to the role.
- Click "**Next: Review**".

3. Review and Create:
- Name the role **execCommandRole**.
- Review the settings and click "**Create role**".

#### Step 3: Ensure IAM User Has PassRole Permission

1. Go to the IAM Console:
- Select the IAM user or role that is launching the ECS task.

2. Add permissions:
- Ensure the user or role has a policy that includes the iam:PassRole permission for the execCommandRole.
- Choose **Specify permissions**, and choose "**JSON**"

Here's an example policy to attach:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "iam:PassRole",
      "Resource": "arn:aws:iam::478371912360:role/execCommandRole"
    }
  ]
}
```

#### Step 4: Create the Role Name "ecsTaskExecutionRole"

2. Create a New Role:
- Click on "**Roles**" in the left-hand menu.
- Click the "Create role" button.

3. Select Trusted Entity:
- Choose "**AWS service**".
- Select "**Elastic Container Service**".
- Choose "**Elastic Container Service Task**" and click "**Next: Permissions**".

#### Step 2: Attach Permissions Policies

1. Attach Policies:
Attach the **AmazonECSTaskExecutionRolePolicy** to the role. This policy provides the necessary permissions for ECS tasks to interact with other AWS services, such as logging to CloudWatch and pulling images from Amazon ECR.

2. Click "**Next: Tags**":
- Optionally add tags to the role.
- Click "**Next: Review**".

3. Review and Create:
- Name the role **ecsTaskExecutionRole**.
- Review the settings and click "**Create role**".

#### Step 3: Ensure IAM User Has PassRole Permission

1. Go to the IAM Console:
- Select the IAM user or role that is launching the ECS task.

2. Add permissions:
- Ensure the user or role has a policy that includes the iam:PassRole permission for the execCommandRole.
- Choose **Specify permissions**, and choose "**JSON**"

Here's an example policy to attach:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "iam:PassRole",
      "Resource": "arn:aws:iam::478371912360:role/ecsTaskExecutionRole"
    }
  ]
}
```
