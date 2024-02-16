In the last decades, small and large enterprises have invested heavily in developing bespoke applications. Since these applications have been built and enhanced over a period of time, they are complex and any form of re-engineering to convert it to smaller, modular and independently hosted services is difficult. Managed Container Services from cloud providers allows predictable and reproducible packaging of such apps. This project moved & deployed a classical web app to AWS ECS containers.

#Docker #ECS #LoadBalancer #Fargate

Here is thw architecture👇

<img src="https://github.com/hisujata/Deploying-web-app-on-AWS-using-ECS/blob/master/architecture.png">

Okay so lets get into it.

1)	Download the Dockerfile from 

https://d6opu47qoi4ee.cloudfront.net/project-container/Dockerfile

2)	Web application needs to be packaged as a Docker image running on Tomcat with Java
3)	Once the image is created, run and verify image by accessing web application using ec2 instance public-ip
4)	Sign up for docker hub and create public repository.
5)	Tag the image appropriately and push to Docker Hub Repository.
6)	Using ECS Fargate create a cluster, task and service(s)

Note:	
In case it is not possible to run this exercise on your laptop for some reason, it can be carried out in an EC2 instance running Ubuntu 18.04/20.04 (port 22 and 80 should be open).


Step 1 : Docker Image creation

#a Creation of Docker image

Instructions

1)	Download the docker file

wget https://d6opu47qoi4ee.cloudfront.net/project-container/Dockerfile

2) See the contents of the dockerfile

cat Dockerfile

3) Build the Docker image using the command below

docker build -t helloworld .

4) Run the image created above using the command given below

docker run -d -p 80:8080 helloworld

5) Verify that the application can be accessed from the machine using the URL below

<public IP address of instance>/HelloWorld

<img src="https://github.com/hisujata/Deploying-web-app-on-AWS-using-ECS/blob/master/Screenshot1.png">

<img src="https://github.com/hisujata/Deploying-web-app-on-AWS-using-ECS/blob/master/Screenshot2.png">

<img src="https://github.com/hisujata/Deploying-web-app-on-AWS-using-ECS/blob/master/Screenshot3.png">

<img src="https://github.com/hisujata/Deploying-web-app-on-AWS-using-ECS/blob/master/Screenshot4.png">


Step 2: Upload image to Dockerhub

#a	Image Upload to DockerHub
	
Instructions
	
1) Create a new free account at hub.docker.com
2) Create a new public repository in your Dockerhub account
3) Login to the DockerHub account from the CLI using the command below

docker login --username=<DockerHub username>

4) Tag the image using the command below

docker tag <image id> <dockerhub username>/<repository name>:latest

5) Push the image to Dockerhub

docker push <hub-user>/<repo-name>:latest	

<img src="https://github.com/hisujata/Deploying-web-app-on-AWS-using-ECS/blob/master/Screenshot5.png">

<img src="https://github.com/hisujata/Deploying-web-app-on-AWS-using-ECS/blob/master/Screenshot6.png">

<img src="https://github.com/hisujata/Deploying-web-app-on-AWS-using-ECS/blob/master/Screenshot7.png">

<img src="https://github.com/hisujata/Deploying-web-app-on-AWS-using-ECS/blob/master/Screenshot8.png">


Step 3: Creation of ECS cluster to run the image

#a Create the ECS Task, Service and Cluster			
Instructions	

1) Navigate to the ECS service in your AWS Educate account
2) Click on Get Started
3) Under Container Definition, select Custom->Configure

Enter the following values

- Image : docker.io/<dockerhub username>/<dockerhub repository>:latest
- Memory Limits : 256MB
- Port Mapping: 8080

Click on Update and then Next
4) Select Load Balancer Type =Application Load Balancer and Click on Next
5) Enter a Cluster Name and Click on Next
6) Click on Create
7) Wait a few minutes for the Cluster to be created

<img src="https://github.com/hisujata/Deploying-web-app-on-AWS-using-ECS/blob/master/Screenshot9.png">

<img src="https://github.com/hisujata/Deploying-web-app-on-AWS-using-ECS/blob/master/Screenshot10.png">


#b Verification of running container on ECS			

Instructions	

1) Navigate to EC2 using the Services Menu
2) Navigate to Load balancer
3) Select the Load Balancer that has been created by the ECS Cluster
4) Take note of the DNS of the load balancer and visit the below URL to verify that the 

ECS cluster is running the container

<DNS of load balancer>:8080/HelloWorld

<img src="https://github.com/hisujata/Deploying-web-app-on-AWS-using-ECS/blob/master/Screenshot11.png">

<img src="https://github.com/hisujata/Deploying-web-app-on-AWS-using-ECS/blob/master/Screenshot12.png">


Cloud is always pay per use model and all resources/services that we consume are chargeable. After completing the project, make sure to delete each resource created in reverse chronological order if you do not wish to use the resources.