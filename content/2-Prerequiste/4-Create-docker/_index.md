---
title : "Install Docker on AL2023"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---

#### Creating a container image for use on Amazon ECS

To install Docker on an Amazon EC2 instance using an Amazon Linux 2023 AMI

1. Configure the SSH Connection



![Install Docker](/images/3/1.png?featherlight=false&width=90pc)

2. Update the installed packages and package cache on your instance.

```
sudo yum update -y
```

![Install Docker](/images/3/2.png?featherlight=false&width=90pc)

3. Install the most recent Docker Community Edition package.

```
sudo yum install docker
```

![Install Docker](/images/3/3.png?featherlight=false&width=90pc)

4. Start the Docker service.

```
sudo usermod -a -G docker ec2-user
```

![Install Docker](/images/3/4.png?featherlight=false&width=90pc)

5. Add the `ec2-user` to the docker group so you can execute `Docker` commands without using sudo.

```
sudo usermod -a -G docker ec2-user
```

![Install Docker](/images/3/5.png?featherlight=false&width=90pc)

6. Log out and log back in again to pick up the new docker group permissions. You can accomplish this by closing your current SSH terminal window and reconnecting to your instance in a new one. Your new SSH session will have the appropriate docker group permissions.

#### Create a Docker image

Amazon ECS task definitions use Docker images to launch containers on the container instances in your clusters. In this section, you create a Docker image of a simple web application, and test it on your local system or Amazon EC2 instance, and then push the image to the Amazon ECR container registry so you can use it in an Amazon ECS task definition.

**To create a Docker image of a simple web application**

1. Create a folder called `mkdir`

```
mkdir EC2-template
```
![Create Docker](/images/3/7.png?featherlight=false&width=90pc)

2. Create a file called `Dockerfile`

```
cd EC2-template
touch Dockerfile
```
![Create Docker](/images/3/8.png?featherlight=false&width=90pc)

3. Edit the `Dockerfile` you just created and add the following content.

```
vi Dockerfile
```
![Create Docker](/images/3/9.png?featherlight=false&width=90pc)

- Template Dockerfile 

```
FROM public.ecr.aws/amazonlinux/amazonlinux:latest

# Update installed packages and install Apache
RUN yum update -y && \
 yum install -y httpd

# Write hello world message
RUN echo 'Hello World!' > /var/www/html/index.html

# Configure Apache
RUN echo 'mkdir -p /var/run/httpd' >> /root/run_apache.sh && \
 echo 'mkdir -p /var/lock/httpd' >> /root/run_apache.sh && \
 echo '/usr/sbin/httpd -D FOREGROUND' >> /root/run_apache.sh && \
 chmod 755 /root/run_apache.sh

EXPOSE 80

CMD /root/run_apache.sh
```
![Create Docker](/images/3/10.png?featherlight=false&width=90pc)

4. Build the Docker image from your Dockerfile.

```
docker build -t hello-world .
```
![Create Docker](/images/3/11.png?featherlight=false&width=90pc)

5. List your container image.

```
docker images --filter reference=hello-world
```
![Create Docker](/images/3/12.png?featherlight=false&width=90pc)

6. Run the newly built image. The -p 80:80 option maps the exposed port 80 on the container to port 80 on the host system.

```
docker run -t 80:80 hello-world
```

![Create Docker](/images/3/12.png?featherlight=false&width=90pc)

**Note: Output from the Apache web server is displayed in the terminal window. You can ignore the "AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName' directive globally to suppress this message**
![Create Docker](/images/3/13.png?featherlight=false&width=90pc)

a. Install Apache

```
sudo yum install httpd -y
```
![Create Docker](/images/3/14.png?featherlight=false&width=90pc)

b. Verify the Installation and check if Apache is installed and running

```
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd
```
![Create Docker](/images/3/15.png?featherlight=false&width=90pc)

c. Locate the Apache Configuration File

```
ls /etc/httpd/conf/
```
![Create Docker](/images/3/16.png?featherlight=false&width=90pc)

d. Result
```
docker run -t 80:80 hello-world
```

![Create Docker](/images/3/18.png?featherlight=false&width=90pc)

7. If you are using an EC2 instance, this is the Public DNS value for the server, which is the same address you use to connect to the instance with SSH. Make sure that the security group for your instance allows inbound traffic on port 80.

![Create Docker](/images/3/19.png?featherlight=false&width=90pc)

#### Push your image to Amazon Elastic Container Registry

Amazon ECR is a managed AWS Docker registry service. You can use the Docker CLI to push, pull, and manage images in your Amazon ECR repositories. For Amazon ECR product details, featured customer case studies.

1. Create an Amazon ECR repository to store your `hello-world` image. Note the repositoryUri in the output.

```
aws ecr create-repository --repository-name hello-repository --region ap-southeast-1 
```
![Create Docker](/images/3/20.png?featherlight=false&width=90pc)

2. Tag the `hello-world` image with the `repositoryUri` value from the previous step.

```
docker tag hello-world 478371912360.dkr.ecr.ap-southeast-1.amazonaws.com/hello-repository
```
![Create Docker](/images/3/21.png?featherlight=false&width=90pc)

3. Run the **aws ecr get-login-password** command. Specify the registry URI you want to authenticate to. For more information, see [Registry Authentication](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html#registry_auth) in the *Amazon Elastic Container Registry User Guide*.

```
aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin 478371912360.dkr.ecr.ap-southeast-1.amazonaws.com/hello-repository
```
![Create Docker](/images/3/22.png?featherlight=false&width=90pc)

Output:

![Create Docker](/images/3/23.png?featherlight=false&width=90pc)

4. Push the image to Amazon ECR with the repositoryUri value from the earlier step.

```
docker push 478371912360.dkr.ecr.ap-southeast-1.amazonaws.com/hello-repository
```
![Create Docker](/images/3/24.png?featherlight=false&width=90pc)

![Create Docker](/images/3/25.png?featherlight=false&width=90pc)

![Create Docker](/images/3/26.png?featherlight=false&width=90pc)


#### Clean up

```
aws ecr delete-repository --repository-name hello-repository --region ap-southeast-1 --force
```

![Create Docker](/images/3/27.png?featherlight=false&width=90pc)

![Create Docker](/images/3/28.png?featherlight=false&width=90pc)
