---
title : "Introduction ECS"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 1. </b> "
---

#### Amazon Elastic Container Service (ECS)

Amazon Elastic Container Service (Amazon ECS) is a fully managed container orchestration service that helps you easily deploy, manage, and scale containerized applications. As a fully managed service, Amazon ECS comes with AWS configuration and operational best practices built-in. It's integrated with both AWS and third-party tools, such as Amazon Elastic Container Registry and Docker. This integration makes it easier for teams to focus on building the applications, not the environment. You can run and scale your container workloads across AWS Regions in the cloud, and on-premises, without the complexity of managing a control plane.

#### Amazon ECS terminology and components

 There are three layers in Amazon ECS:

- Capacity - The infrastructure where your containers run

- Controller - Deploy and manage your applications that run on the containers

- Provisioning - The tools that you can use to interface with the scheduler to deploy and manage your applications and containers

The following diagram shows the Amazon ECS layers.

![Create a ECS](/images/1/1.png?featherlight=false&width=90pc)

#### Amazon ECS capacity

 Amazon ECS capacity is the infrastructure where your containers run. The following is an overview of the capacity options:

- Amazon EC2 instances in the AWS cloud. You choose the instance type, the number of instances, and manage the capacity.

- Serverless (AWS Fargate (Fargate)) in the AWS cloud

- Fargate is a serverless, pay-as-you-go compute engine. With Fargate you don't need to manage servers, handle capacity planning, or isolate container workloads for security.

- On-premises virtual machines (VM) or servers

- Amazon ECS Anywhere provides support for registering an external instance such as an on-premises server or virtual machine (VM), to your Amazon ECS cluster.

The capacity can be located in any of the following AWS resources:

- Availability Zones

- Local Zones

- Wavelength Zones

- AWS Regions

- AWS Outposts


#### Amazon ECS controller

The Amazon ECS scheduler is the software that manages your applications.

#### Amazon ECS provisioning

There are multiple options for provisioning Amazon ECS:

- AWS Management Console — Provides a web interface that you can use to access your Amazon ECS resources.

- AWS Command Line Interface (AWS CLI) — Provides commands for a broad set of AWS services, including Amazon ECS. It's supported on Windows, Mac, and Linux. For more information, see AWS Command Line Interface

- AWS SDKs — Provides language-specific APIs and takes care of many of the connection details. These include calculating signatures, handling request retries, and error handling. For more information, see AWS SDKs

- Copilot — Provides an open-source tool for developers to build, release, and operate production ready containerized applications on Amazon ECS. For more information, see Copilot on the GitHub website.

- AWS CDK — Provides an open-source software development framework that you can use to model and provision your cloud application resources using familiar programming languages. The AWS CDK provisions your resources in a safe, repeatable manner through AWS CloudFormation.

#### Application lifecycle

![Create a ECS](/images/1/2.png?featherlight=false&width=90pc)

You must architect your applications so that they can run on containers. A container is a standardized unit of software development that holds everything that your software application requires to run. This includes relevant code, runtime, system tools, and system libraries. Containers are created from a read-only template that's called an image. Images are typically built from a Dockerfile. A Dockerfile is a plaintext file that contains the instructions for building a container. After they're built, these images are stored in a registry such as Amazon ECR where they can be downloaded from.

After you create and store your image, you create an Amazon ECS task definition. A task definition is a blueprint for your application. It is a text file in JSON format that describes the parameters and one or more containers that form your application. For example, you can use it to specify the image and parameters for the operating system, which containers to use, which ports to open for your application, and what data volumes to use with the containers in the task. The specific parameters available for your task definition depend on the needs of your specific application.

After you define your task definition, you deploy it as either a service or a task on your cluster. A cluster is a logical grouping of tasks or services that runs on the capacity infrastructure that is registered to a cluster.

A task is the instantiation of a task definition within a cluster. You can run a standalone task, or you can run a task as part of a service. You can use an Amazon ECS service to run and maintain your desired number of tasks simultaneously in an Amazon ECS cluster. How it works is that, if any of your tasks fail or stop for any reason, the Amazon ECS service scheduler launches another instance based on your task definition. It does this to replace it and thereby maintain your desired number of tasks in the service.

The container agent runs on each container instance within an Amazon ECS cluster. The agent sends information about the current running tasks and resource utilization of your containers to Amazon ECS. It starts and stops tasks whenever it receives a request from Amazon ECS.

After you deploy the task or service, you can use any of the following tools to monitor your deployment and application:

- CloudWatch

- Runtime Monitoring
