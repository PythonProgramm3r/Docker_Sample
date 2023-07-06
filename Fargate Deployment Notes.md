## [What is Fargate](https://docs.aws.amazon.com/AmazonECS/latest/userguide/what-is-fargate.html) :
- AWS Fargate is a technology that you can use with Amazon ECS to run containers without having to manage servers or clusters of Amazon EC2 instances. With Fargate, you no longer have to provision, configure, or scale clusters of virtual machines to run containers. 
   This removes the need to choose server types, decide when to scale your clusters, or optimize cluster packing.
### **Benefits of Fargate**
* **Serverless computing:**
	- AWS Fargate require servers to function, but they spare users the effort of having to deploy and manage these servers themselves.
* **Data security:** 
	- AWS Fargate can operate each container in an isolated runtime environment, complete with its memory and CPU resources, making it considerably more challenging for hackers to compromise the entire system.
---
#### Requirements:
* AWS User with root/appropriate permissions
* Solution to containerize 
---
### Hosting an image with ECR :

* What is ECR
	* [AWS ECR Documentation](https://docs.aws.amazon.com/ecr/)
	- Amazon Elastic Container Registry (ECR) is a fully managed container registry that makes it easy to store, manage, share, and deploy your container images and artifacts anywhere.
- [Amazon ECR & CLI Start guide](https://docs.aws.amazon.com/AmazonECR/latest/userguide/getting-started-cli.html) .


	
#### **Benefits :**
* **Fully managed** 
	- Amazon ECR eliminates the need to operate and scale the infrastructure required to power your container registry.
* **Secure** 
	- Amazon ECR transfers your container images over HTTPS and automatically encrypts your images at rest.
* **Highly available** 
	- Amazon ECR has a highly scalable, redundant, and durable architecture, so your container images are highly available and accessible.
* **Simplified workflow** 
	- Amazon Elastic Container Registry integrates with Amazon EKS, Amazon ECS, Amazon Lambda, and the Docker CLI, allowing you to simplify your development and production workflows.
* **Public collaboration** 
	- Amazon ECR Public makes it easy to publicly share container software worldwide for anyone to download. Anyone with or without an AWS account can search the Amazon ECR Public Gallery for public container software, and pull artifacts for use.
---
### How to push an image to ECR :
- [ECR Initial Usage Documentation](https://docs.aws.amazon.com/AmazonECR/latest/public/public-getting-started.html)
	* You can build, tag, and push a container image using the Docker CLI. You can use a container image that you have built from a Dockerfile or one that you pulled from another registry, such as a private Amazon ECR repository or Docker Hub and then push the tagged image to your public repository.

#### Steps (Overview) : 

	1. Select the public repository you created and choose View push commands to view the steps to push an image to your new repository.
	
	2. Run the login command that authenticates your Docker client to your registry by pasting the command from the console into a terminal window. This command provides an authorization token that is valid for 12 hours.
	
	3. (Optional) If you have a Dockerfile for the image to push, build the image and tag it for your new repository. Pasting the docker build command from the console into a terminal window. Make sure that you are in the same directory as your Dockerfile.
	
	4. Tag the image with the URI of your public registry and your new repository by pasting the docker tag command from the console into a terminal window. The console command assumes that your image was built from a Dockerfile in the previous step. If you did not build your image from a Dockerfile, replace the first instance of repository:latest with the image ID or image name of your local image to push.
	
	5. Push the newly tagged image to your repository by pasting the docker push command into a terminal window.
		
* View these commands on your [ECR Repository](https://us-east-1.console.aws.amazon.com/ecr/repositories?region=us-east-1)
- Use the following [steps](https://github.com/PythonProgramm3r/Independent-Study/blob/main/Fargate%20Deployment%20Notes.md#Pushing-your-image-) to push an image to your repository. 
- Additional registry authentication methods, including the Amazon ECR credential helper, 
[See Registry Authentication](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html#registry_auth).		
	
---
#### Pushing your image :

***Click on your repository name of choice and then "View push commands" which should generate a step by step guide:*** 
![Push Commands][Push Commands]

1. Retrieve an authentication token and authenticate your Docker client to your registry.Use AWS Tools for PowerShell:
	- (Get-ECRLoginCommand).Password | docker login --username AWS --password-stdin ..
2. Build your Docker image using the following command. For information on building a Docker file from scratch see the [instructions](https://github.com/PythonProgramm3r/Independent-Study/blob/main/Fargate%20Deployment%20Notes.md#Container-with-application). 
   You can skip this step if your image is already built:
	- docker build -t your-image-name
3. After the build completes, tag your image so you can push the image to this repository:
	- docker tag your-image-name:latest 951977081116.dkr.ecr.us-east-1.amazonaws.com/your-image-name:latest
4. Run the following command to push this image to your newly created AWS repository:
	- docker push 951977081116.dkr.ecr.us-east-1.amazonaws.com/your-image-name:latest
---	

### VPC on AWS :
- Amazon Virtual Private Cloud ([Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)) enables you to launch AWS resources into a virtual network that you've defined. This virtual network closely resembles a traditional network that you'd operate in your own data center, with the benefits of using the scalable infrastructure of AWS.
 * [AWS VPC Usage Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/configure-your-vpc.html)
- An [internet gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html) is a horizontally scaled, redundant, and highly available VPC component that allows communication between your VPC and the internet. 
- A [security group](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) controls the traffic that is allowed to reach and leave the resources that it is associated with. For example, after you associate a security group with an EC2 instance, it controls the inbound and outbound traffic for the instance.
- A [subnet](https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html) is a range of IP addresses in your VPC. You can launch AWS resources into a specified subnet. Use a public subnet for resources that must be connected to the internet, and a private subnet for resources that won't be connected to the internet.
---	
   
### ECS, EKS, and Fargate 
* [Fargate]() is a serverless container compute engine used to launch and execute containers without providing or maintaining EC2 instances. As a result, users do not need to worry about instances or servers; instead, they must define their resource requirements. Amazon ECS and AWS EKS support Fargate.
* **Fargate vs S3 buckets for hosting**
  - Fargate is automatically scaling, allocating, distributing, etc. but costs more than S3 buckets which are more general cloud storage service. 
  - Budget contraints clearly displays S3 is better to use on static webpages that get less than 20,000,000 views per month. Whereas Fargate is much better suited to for apps that use a lot of other AWS services and other components. 
* **Fargate vs [ECS](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html) (Elastic Container Service)**
  - The main difference between AWS Fargate and ECS is that AWS Fargate requires a container orchestration service, either EKS or ECS, to function properly, and Amazon ECS can orchestrate containers via AWS Fargate, but you can also use it with Amazon EC2 or the on-premises option AWS Outposts.
  - Fargate essientially manages the cluster scaling and orchestration, whereas ECS is used to for a more detailed control. S
  - **_While many people are confused by the question “Fargate vs. ECS,” the truth is that it is not the most appropriate comparison to make. Instead, the question is whether Fargate and ECS should be used together._**
  - This webpage describes the differences and why it's more ECS vs EKS using fargate vs vanilla ECS & EKS :
	- https://dzone.com/articles/eks-vs-ecs-vs-fargate-vs-kubernetes-aws-containers
	
  
* **Fargate vs [EKS](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html) (Elastic Kubernetes Service)**
* **EKS Benefits**
  - EKS uses Kubernetes, it’s more flexible, meaning you could migrate your workload to another platform easier than you could from ECS, making it more suitable for complex multi-cloud workloads.
  - EKS ultimately offers more control (over cluster management and scheduling) than ECS.
  - EKS is better suited to complex applications
 
* **Conclusion**
  - As this clearly shows,running a static webpage is much cheaper using a S3 bucket and route 53 services than spinning up clusters but obviously there are many limitations to using a storage rather than a dedicated server to deliver content. 
	
  - If you have a simpler application and want to incorporate a lot of AWS services, ECS is probably the tool for you.
  - If you have a more complex project, and particularly if you want to use a multi-cloud approach, EKS is the right choice – and obviously if you want to use Kubernetes.

### Container with application :
* [AWS Container Documentation](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-container-image.html)
* Use the above guide to walkthrough generating/building your container image.

### Fargate / ECS Deployment Definitions :
* [Cluster](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/clusters.html) is a logical grouping of tasks or services
 - Clusters are Region-specific.
 - The following are the possible states that a cluster can be : 
   - ACTIVE The cluster is ready to accept tasks and, if applicable, you can register container instances with the cluster.
   - PROVISIONING The cluster has capacity providers associated with it and the resources needed for the capacity provider are being created.
   - DEPROVISIONING The cluster has capacity providers associated with it and the resources needed for the capacity provider are being deleted.
   - FAILED The cluster has capacity providers associated with it and the resources needed for the capacity provider have failed to create.
   - INACTIVE The cluster has been deleted. Clusters with an INACTIVE status may remain discoverable in your account for a period of time. However, this behavior is subject to change in the future, so you should not rely on INACTIVE clusters persisting.
* [Task](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definitions.html) definition is required to run Docker containers in Amazon ECS
   - The Docker image to use with each container in your task
   - How much CPU and memory to use with each task or each container within a task
   - The launch type to use, which determines the infrastructure that your tasks are hosted on
   - The Docker networking mode to use for the containers in your task
   - The logging configuration to use for your tasks
   - Whether the task continues to run if the container finishes or fails
   - The command that the container runs when it's started
   - Any data volumes that are used with the containers in the task
   - The IAM role that your tasks use
* [Service](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_services.html?icmpid=docs_ecs_hp_deploy)
   - A service enables you to run and maintain a specified number of instances of a task definition simultaneously in an Amazon ECS cluster. A service is ideal for long running stateless services and applications. If any of your service tasks fail or stop, the service scheduler launches another instance of your task definition to replace it. This helps to maintain your desired number of tasks in the service. 

---	
## Deploying an Image to Fargate (AKA Running a Task)
- Once your [cluster is built](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create_cluster.html) , go to your cluster's page. 
- On your cluster page, there is a tasks tab. Click on that and then click "Run new task"
- To build and configure your tasks use this [guide](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-task-definition-classic.html)
- Fargate Specific Launch Steps are on the Fargate launch type tab
- [roles for tasks documentation](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html) 
 
---
## Issues and Spinning Down Tasks :

### Budget :
![3 Month budget usage stack][budget_usage_stack]
![3 Month budget usage line][budget_usage_line]
![3 Month budget usage bar][budget_usage_bar]

### Stop Task :
- To stop a task, simply navigate to your cluster page's task tab. Click on your intended task to stop, and then click the stop task. This will trigger an event, that will ask to validate your stop request. 

### Rockin my sockets : 
![socket permissions issue][socket_permissions]
![dockerd to the rescue][dockerd_magic]

### Successful Image Build : 
![docker buiding image][docker_build_image]

### Logon CLI : 
![logon issue][Logon_issues]
---	
### Resources :
- https://dzone.com/articles/eks-vs-ecs-vs-fargate-vs-kubernetes-aws-containers
- https://gcpfirebase.com/aws-fargate-vs-ecs/
- https://github.com/PythonProgramm3r/Independent-Study/raw/main/Docker%20Projects/AWS%20Deploying%20Docker.one
- https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet
- https://towardsdatascience.com/deploying-a-docker-container-with-ecs-and-fargate-7b0cbc9cd608
- https://docs.aws.amazon.com/
    
[budget_usage_stack]: https://github.com/PythonProgramm3r/Independent-Study/blob/83eea41f7638b135bee2887ba5cccf6cb38e74cd/md_image/budget_usage_stack.png
[budget_usage_line]: https://github.com/PythonProgramm3r/Independent-Study/blob/6d62df472792f9b52ed7759df79c0216510ae423/md_image/budget_usage_line.png
[budget_usage_bar]: https://github.com/PythonProgramm3r/Independent-Study/blob/6d62df472792f9b52ed7759df79c0216510ae423/md_image/budget_usage_bar.png
[socket_permissions]: https://github.com/PythonProgramm3r/Independent-Study/blob/6d62df472792f9b52ed7759df79c0216510ae423/md_image/socker_permissions.png
[dockerd_magic]: https://github.com/PythonProgramm3r/Independent-Study/blob/6d62df472792f9b52ed7759df79c0216510ae423/md_image/dockerd_socket_patch.png
[docker_build_image]: https://github.com/PythonProgramm3r/Independent-Study/blob/6d62df472792f9b52ed7759df79c0216510ae423/md_image/build_complete.png
[Logon_issues]: https://github.com/PythonProgramm3r/Independent-Study/blob/6d62df472792f9b52ed7759df79c0216510ae423/md_image/Logon_issues.png
[Push Commands]: https://github.com/PythonProgramm3r/Independent-Study/blob/6d62df472792f9b52ed7759df79c0216510ae423/md_image/Pushing_image_cmds.png


   


   


   
