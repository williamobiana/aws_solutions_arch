## What is serverless
History of computing
Serverless benefits
Tools

Serverless
This means we focus on code and leave the physical management of the computers to AWS

Benefits
* Ease of use
* Event based: launched in response to an event
* Billing Model: Pay as you go

Tools
* Lambda (write your code, and build your function)
* Fargate (run your containers)

### Exam tips
Move away from unmanaged ec2, and focus on serverless


## Computing with Lambda
What is lambda
Building a function

What is lambda
Its a serverless compute service that lets you run code without provisioning or managing the underlying servers. Its like your running code without computers.

Building a function
* Runtime: we need to pick from avaliabe runtime or bring my own. this is the env the code will run in.
* permissions: if lambda function makes an API call, we have to attach a role.
* Networking: you can optionally define the VPC, subnet and security group your functions are part of
* Resources: defining the amount compute resources(memory, CPU, RAM) to run the code
* Trigger: Defining a trigger to kickoff the lambda event

### Exam tips
* when asked how we add features to AWS: Lambda is the answer
* Limitations: 10Gb is max ram, 15 mins is max time
* Triggers: starts lambda functions
* Microservices: lambda runs lightweight functions
* Ecosystem: Lmabda plays a major role in AWS ecosystem, it is easily intergrated with many services.
* Networking: Can run inside or outside a VPC


## Leveraging AWS serverless Application Repository
What is AWS serverless Application Repository
Publish and deploy

What is AWS serverless Application Repository
* Serverless Apps: allows users to easily find, deploy, and publish, thier own serverless applications.
* Sharing: Ability to provately or publicly share the apps
* Manifests: Upload your app and its manifest (AWS SAM Template)
* Intergrations: Intergrated with AWS Lambda service.

Publish and Deploy
Publish
* Publishing apps makes them available for others to find and deploy 
* Define apps with AWS SAM Templates
* Set to private by default 
* Must explicitly share if desired

Deploy
* Find and deploy published applications
* Browse public apps without needing an AWS account
* Browse within the AWS Lambda console
* Be careful of trusting all apps

### Exam tips
* Templates: define apps with AWS SAW Templates. they are private by default
* Publish and Deploy: choice of publishing your own applications or deploying publicly availalble ones
* AWS lambda: Heavily intergrated with AWS lambda



## Container Overview
What is a container
Container terminology

What is a container
A container is a standard unit of software that packages up code and all its dependencies, so the applications runs quickly and reliably from one computing environment to another.

Container terminology
* Dockerfile: text document that contains all commands and instructions to build an image
* Image: immutable file that contains the code libraries, dependencies and configuration files needed to run an application
* Registry: Stores docker images for distribution. They can be both private and public
* Container: A running copy of the image that has been created

### Exam tips
* Containers are more flexible
* immutable and easy to move around env
* High level: know the use case for dockerfile
* ease of use
* dev vs prod: containers can run on dev and prod effeciently


## Running Containers on ECS or EKS
Problems with containers
Why ECS
Using Kubernetes
ECS vs EKS

Common problems with containers is managing a large amount of them 

Why ECS
* Management of containers at scale: it approrately place the containers and keep them online
* ELB intergration: Containers are appropriately registered with the load balancers as they come online and go offline
* Role Intergration: Containers can have individual roles attached to them, making security effecient
* Ease of use: easy to set up and scale to handle any workload

Using Kubernetes
* While ECS is native to AWS, Kubernetes is open source
* Can be used on premise and on AWS
* AWS has a managed version of kubernetes called EKS

ECS vs EKS
ECS
* Properietary AWS management solution 
* Best use when all operation are run on AWS
* Simiple to use
* BUT doesnt work on-premise

EKS
* AWS managed version of kubernetes
* Best used when NOT all operations are run on AWS
* More work to configure and intergrate with AWS

ECS Demo 
* Configure the settings and Create a container (in the Task definition)
* Create a cluster and run the task for the container

### Exam tips for ECS
* ECS is prefered for containers
* only EXCEPTION for EKS is open-source, kubernetes, on-premise
* preference: generally favor using AWS-designed services over 3rd party
* open-source: open-source, kubernetes, on-premise
* High level: ECS and EKS manage creation and running of our containers
* Lenght: great for 1 time or long running applications



## Removing servers with Fargate
Problems with servers
What is fargate
ec2 vs fargate
fargate vs lambda

Problems with servers include modifying or deleting

What is fargate
Fargate is a serverless compute engine for containers that work with ECS and EKS (aka it is a feature of ECS and EKS)

Ec2 vs Fargate
EC2
* you are responsible for underlying OS
* ec2 pricing (we can use spot)
* can handle long-running containers
* multiple containers can share the same host

Fargate
* no OS access, it is done automatically for us
* we pay based on resources allocated and time it ran
* for short-running tasks
* runs on isolated environments

Fargate vs Lambda
Fargate (if i need to work with containers)
* we can use fargate when we have more consistent workloads
* allows docker to be used accross the organization, and developers have more control

Lambda (if i need to work on small light weight tasks)
* we can use lambda for unpredictable or inconsistent workloads
* perfect for applications that can be expressed as a single function

Fargate Demo 
* Configure the settings and Create a container (in the Task definition)
* Create a cluster and run the task for the container

### Exam tips
when to use lambda vs fargate vs ec2
* Lambda (if i need to work on small light weight tasks)
* Fargate (if i need to work with containers that done need all the time)
* EC2 (if i need to work with containers that run all the time)
* lose the servers: fargate is serverless
* required tools:  fargate is a feature of ECS and EKS
* fargate vs ec2: fargate is more expensive but easy to use
* fargate vs lambda: fargate is for containers, lambda is for short simple functions


## Amazon EventBridge (CloudWatch Events)
What is EventBridge?
Creating a Rule

What is EventBridge?
EventBridge is a serverless event bus. It allows you to pass an event from a source to an endpoint. (glue holding serverless together)

Creating a Rule
* Define a pattern: should it be invoked, scheduled or event based?
* Select event bus: AWS based, custom event, or from a partner?
* Select target: what happens when the event kicks in?
* Tag: Tag everything
* Test: test to make sure every thing is working well

### Exam tips
* API Calls: use-case is triggering lambda functions when AWS API calls happen
* High Level: keep it high level
* New vs Old: Eventbridge is the new name
* Speed: this is the fastest way to respond to things happening in the environment


## Storing Custom Docker Images in Amazon ECR
service overview
components
features
intergrations

service overview
registry: managed container image registry
private: private container image repository with IAM permissons
supported formats: open container initiative (OCI) images, Docker images and OCI artifiacts

componets
* registry: private registry provided to each AWS account, create 1 or more for image storage
* authorization token: authentication token required for pushing and pulling images to and from registries
* repository: contains all docker images, OCI images and OCI artifacts
* repository policy: control all access to repos and images
* image: container images that get pushed and pulled from your repository
ECR public is a similar service for public image repositories

features
Lifecycle policies:
* helps management of images in repo
* define rules for unused imaged clean up
* ability to test rules before apply

Image scanning:
* helps identify software vulnerabilities in your container images
* repo can be set to scan on push
* retrieve result of scan for each image

Sharing:
* cross-region support
* cross-account support
* configured per repo and per region

Cache Rules:
* Pull through cache rules allow for caching public repos privately
* ECR periodically reaches out to check current caching status

Tag Mutability:
* prevents image tags from being overwritten 
* configured per repo

intergrations
* we can leverage ECR images within your own container infrastructue
* use container images in ECS container definitions
* pul images for your EKS clusters
* Amazon linux containers can be used locally for software development

### Exam tips
* container image storage: AWS-manged container image registry service
* Supported Formats: docker images, OCI images, OCI compactible artifacts
* Lifecycle Polies: expire and remove unused or older books
* Image Scanning: Scan on push repository settings allow for identifying software vulnerabilites in your container image
* Tag mutability: helps prevent image tags from being overwritten
* Keywords to look for: questions related to manageing container image registry, OCI repo or image intergration with ECS and EKS


## Using Open-Source Kubernetes in EKS distro
* EKS-D is a kubernetes distro based on and used by EKS
* similarites: it has the same versions and dependencies with EKS
* diffrences: EKS-D is fully managed by you, EKS is managed my AWS
* Where: EKS-D can run anywhere
* Your Responsibility: you are fully responsible for upgrading and managing your platforms

### Exam tips
* keywords: look for questions related to self managed kubernetes deployments similar to EKS
* uses: use EKS-D when you need to run versioned deployments of clusters outside of AWS managed services
* not common: It does not commonly appear on exams and functions like EKS 


## Orchestrating Containers Outside AWS using EKS and ECS anywhere
EKS anywhere overview
EKS anywhere concept
ECS anywhere overview
ECS anywhere requirements

EKS anywhere overview
* On-premises EKS: way to manage kubernetes clusters with the same practices used for EKS
* EKS distro: based on EKS  distro, allows for deployment, usage, and management methods for clusters in data centers
* lifecycle: offers full lifecycle management of multiple k8s clusters, operates independently of AWS

EKS anywhere concepts
* control plane: control plane is managed by the customer
* location: control plane is located in customer data center or operation center
* updates: updates are done via manual CLI or FLux
* Curated Packages: offer extended core functionalites of k8s clusters
* Enterprise subscription: packages require enterprise subscription

ECS anywhere overview
* management of container based apps on-premise
* no orchestration needed: no need to install and operate local container orchestration software, meaning more operational efficiency
* completely managed: completely managed solution enabling standardization of container management across environmnets
* inbound traffic: no ELB support makes inbound traffic requiremnts less efficient
* external: new launch type noted as external for creating services or running tasks

ECS anywhere requirements
* Agents: you must have the SSM agent, ECS agent, and Docker installed
* you must register external instances as SSM Managed Instances
* Easily create an installation script within the console
* Scripts contain SSM activation keys and commands for required software
* Execute scripts on your on-premises VMs or bare-metal servers
* Deploy containers using the EXTERNAL launch type

### Exam tips 
EKS anywhere
* not commonly used
* based on EKS distro
* leverage AWS efficiency

ECS anywhere
* it is a feature within ECS
* allows for AWS managed ocrcehstration
* requirements: you must have the SSM agent, ECS agent, and Docker installed
* register & deploy: execute scripts containing required steps and use External launch type


## Autoscaling database on demand with aurora serverless
aurora serverless overview
aurora serverless concepts
use cases

aurora is a database
aurora provisioned is manual
Aurora serverless is AWS managed

aurora serverless
* on demand: database are avaliable on demand and can be autoscaled
* can be automated: automated monitoring, and adjusting capacity for databases
* capacity can be adjusted based on app demands
* billing: charged only for resources consumed by DB clusters, per second billing
* budget friendly: manage cost with auto-scaling and per second billing

concepts
* Aurora capacity units (ACUs): measurements on how our clusters scale
* set a min and max ACU for scaling requirments - can be 0
* allocated by AWS managed warm pools
* combo of 2Gib MEM, matching CPU and network capability
* same data resiliency as Aurora provisioned: 6 copies of data across 3 AZs
* Multi-AZ deployment for establishing highly available clusters

use case:
* unpredictable and variable workloads
* multi-tenant apps (diffrenct apps can use the same DB)
* new apps
* dev & test of new features
* mixed use apps
* capacity planning (verifing the optimal capacity)

### Exam tips for aurora serverless
* on-demand: database are avaliable on demand and can be autoscaled
* Aurora capacity units (ACUs): measurements on how our clusters scale, min ACU can be 0
* budget friendly: per second billing
* exam scenerios: based around scenerios where we are not sure about sizing, dev & test env
* same data resiliency as Aurora provisioned: 6 copies of data across 3 AZs
* Multi-AZ deployment for establishing highly available clusters


## Using x-ray for app insights
service overview
exam concepts
daemon
intergration

service overview
* app insights: collects app data for viewing, filtering and gaining insights about requests and responses
* downstream: view calls to downstream resources and other microservices APIs or databases
* traces: recieves traces from your apps for allowing insights
* multiple options: intergrated services can add tracing headers, send trace data, run x-ray daemon

exam concept
* segments: data containing resource name, request details, and other info
* subsegment: granular information and details
* service graph: graphical representation of interacting services in requests
* traces: traceID tracks path of request and traces collect all segments in a request
* tracing header extra HTTP header containing sampling decissions and traceID
* tracing header containing added information is named X-Amnz-Trace-Id

AWS x-ray daemon
* AWS software application that listen on UDP port 2000. 
* it collects raw segment data and send it to the AWS X-Ray API
* when the daemon is running, it works along with the X-ray SDKs

Intergration
* EC2
* ECS
* Lambda
* Elastic BeanStalk
* API gateway
* SNS+SQS

### Exam tips
* app insights for collects app data for viewing, filtering and gaining insights about requests and responses: Think AWS X-ray
* segments: data containing resource name, request details, and other info
* traces: traceID tracks path of request and traces collect all segments in a request
* tracing header extra HTTP header containing sampling decissions and traceID
* intergration (EC2, ECS, Lambda, Elastic BeanStalk, API gateway, SNS+SQS)
* exam keywords: Scenerios involving app request insights, viewing response times of downstream resources, HTTP response analysis


## Deploying GraphQL interfaces in AWS AppSync
What is AWS AppSync
* robust and scalable graphQL interface for application developers
* combines data from multiple sources (dynamoDB, Lambda)
* enables data interaction for devs via graphQL
* GraphQL: data language that enables apps to fetch data from servers
* Seamless intergration with React, React Native, iOS, and Android

### Exam tips
scalable GraphQL interface
Service for scalable GraphQL interface for application developers
keywords: GraphQL, fetching app data, declarative coding and frontend app data fetching





## Exam tips recap
4 question to ask
* is the app a right fit for container or should it be hosted on ec2
* do we need the servers that run the app or should we be serverless?
* is it AWS specific?
* how long does the code need to run?

### Lamnda 
* when asked how we add features to AWS: Lambda is the answer
* lambda loves roles: always attach a role to the lambda function
* Be familar with lambda Triggers: starts lambda functions (s3, eventbridge, etc)
* Limitations: 10Gb is max ram, 15 mins is max time
* Any API call can be made

### Containers and Images
* open-source: Think Kubernetes
* if scenerios talks about running on-prem and AWS: think EKS or EKS anywhere
* Fargate cannot work alone: we must use ECS or EKS to use fargate 
* Containers offer flexibility: they can handle any workload, it is preferred to use containers than ec2
* know the basics: dockerfile - build image - upload to repo - run on host
* AWS managed image registry for image storage or image vunerebility scanning: think Amazon ECR

### Aurora serverless
* on-demand or auto scaling database: think aurora serverless databases
* variable traffic or workloads, designing new apps with unknow workload, traffic spike to database: think aurora serverless
* capcity planning for new apps with database, dev environment: likely aurora serverless

### X-ray
* AWS X-ray: Used to gain app insights using requests and responses of services and functions at diffrent points in an application flow
* Traces and downstreams response times: if these terms are brought up in the question, think x-ray
* AWS lambda or API gateway insights: X-ray intergrates natively with these services and can easily help you gain deeper insights and understanding of your workload requests and responses.

### Appsyn
* managed grapgQL: any scenrio with managed graphQL interfaces for frontend apps/dev think AWS AppSync




