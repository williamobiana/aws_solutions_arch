## Decouplig workflows overview
tight coupling (ec2 FE - ec2 BE)
loose coupling (ELB FE - ec2 FE - ELB BE - ec2 BE)

Simple queue service (SQS)
fully managed message queuing service that enables us to decouple and scale microservices, distributed systems, and serverless applications.

Simple notification service (SNS)
SNS is a fully managed messaging service for both app 2 app (A2A) and app 2 person (A2P) communication.

API gateway
API gateway is a fully managed service that make it easy for developers to create, publish, maintain, monitor and secure APIs at any scale.

### Exam tips
* never tightly couple: avoid answers that include tight coupling (example: ec2 - ec2)
* while tight coupling is simpler for the architecture, it doesnt offer any meaningful benefits over loose coupling.
* always loosely couple: it is always the answer
* never tightly couple: 
* internal and external: every level should be loosely coupled
* one size doesnt fit all: ELB or SQS can be required depending on the suituation

## Messaging with SQS (Simple queue service)
poll based messaging
what is SQS
important settings
visibility timeout

what is poll based messaging
* resource writes a message, a messenger(SQS) picks it up and queue it.

SQS is a messaging queue that allow asynchronous(non-corodinated) processing of work. 

SQS settings
* delivery delay: default is 0, but can set up to 15 minutes
* message size: message can be up to 256 kb of text in any format
* encryption: message are encrypted in transit by default, but you can add at-rest
* message retention: default is 4 days, but we can set between 1 min and 14 days
* long vs short: Long polling isnt the default, but it should be.
* Queue depth: this can be a trigger for autoscaling

visibility timeout
a period of time during which SQS prevents other resources from receiving and processing a requested message.

### Exam tips
* SQS is important and is going to be featured on the exam
* what each SQS settings would do
* troublshoot: we need to pinpoint the issues
* size: messages are 256kb of text in any format.
* nothing lasts forever: messages can only live up to 14 days max
* polling: its important to know the diffrence between long and short polling


## Sidelining messages with dead letter queues
Dead letter queue
is an SQS queue that an SNS can target for messages that can't be delivered to subscribers successfully. Messages that can't be delivered due to client errors or server errors are held in the dead-letter queue for further analysis or reprocessing.

dead letter queue always have to be built before the main queue

### Exam tips
DLQs are the best sideline queue
if problem with a message in SQS: think DLQs and visibility timeout
* Monitor (make sure to set up an alarm and alert on queue depth)
* DLQs are sqs queues that are set to recieve the reject messages
* Same retention window: messages will be held up to 14 days
* Usuable with SNS: You can create SQS DLQ for SNS topics


## Ordered message with SQS FIFO
SQS message ordering
* standard
* FIFO

standard:
* best effort ordering
* duplicate messages
* nearly unlimited transactions pre second

FIFO (first in first out)
* guaranteed ordering
* no message duplication
* 300 messages per second
* fifo queue has to end in .fifo

### Exam tips
FIFO is the main option when it comes to ordering and duplication
FiFO Queue:
* performance: fifo queues do not have the same level of performance
* not the only way: you can order messages with SQS standard, but its on you to do it.
* message group ID: this ensures messages are processed one by one.
* cost: it costs more since AWS must spend compute power to deduplicate messages.

## Delivering Message with SNS
push-based messaging
what is SNS
important settings

SNS is push-based messaging service, that proactivly deliever messages to endpoints subscribed to it.
This can be used to alert a system or a person.

settings
* subscribers: what services will recive it (kinesis, lambda, email, sms, SQS, etc)
* message size: messages can be up to 256kb of txt in any format
* DLQ support: messages that fail to be delivered can be stored in SQS DLQ
* FIFO or standard: FIFO only supports SQS as a subscriber
* Encryption: messages are encrypted in transit by default, but you can add at-rest
* Access Policy: a resource policy can be added to a topic, similar to s3

### Exam tips
* alert = sns
* to know an event happened, SNS is the service to deliver that notification
* push based notifications: think sns
* cloudwatch: SNS and cloudwatch are the best friends and easiest way to alert on events
* subscriber options: (kinesis, lambda, email, sms, SQS, etc) based on usecase
* dont confuse SNS with SES (SES is email service for marketing and used as a distractor)
* no do overs: SNS will only retry HTTP(s) endpoints, and nothing else can be side lined to DLQs if we dont wanna lose the information


## Fronting applications with API gateway
what is API gateway?
notable features
API gateway in practice

API gateway is a fully managed service that allows us to easily publish, create, maintain, monitor and secure our API.
It allows us to put a safe "front door" on our application

Features
* Security: this allows us to protect our application using a web application firewall (WAF)
* stop abuse: users can easily implement DDoS protection and rate limiting to curb abuse of thier endpoints
* Ease to use: easy to build the calls that will start other AWS services in the account

### Exam tips
* API gateway is a safe and secure front door to our application
* it is the prefered method to get API calls into our application and AWS environment
* we should favor answers that include API gateway over those that hardcode access and secret keys
* API: if the scenerio is taking about api that we are building and managing ourselves: think API gateway
* DDoS: we can prevent DDoS attack, by fronting API gateway with a web application firewall (WAF)
* Versioning: it supports versioning of API
* No baking: Using API Gateway stops baking credentials into the code (we dont need access keys to access the application)


## Executing Batch Workloads Using AWS Batch
AWS batch service overview
Important components
Fargate or EC2 Compute Environments
AWS batch or AWS lambda
Managed and Unmanaged Compute Environments

AWS batch 
* Batched Workloads: Allows you to run batch computing workloads with AWS (run on EC2 or ECS/Fargate)
* Makes things simpler: removes any heavy lifting for configuration and management of infrastructure required for computing
* Automatically provision and scale: capable of provisioning accurately sized compute resources based on number of jobs submitted and optimizes the distribution of workloads
* No installation Required: allows us to skip installation and maintenance of batch computing software, so we can focus on obtaining and analyzing the results

Important components
* Jobs: Unit of work that are submitted to AWS batch (eg: shell scripts, executables, Docker Images)
* Job Definitions: Specify how our jobs are to be run (essentially, the blueprint for the resources in the job)
* Job Queues: Jobs get submitted to specific queues and reside there until scheduled to run in a compute environment
* Compute Environment: Set of managed or unmanaged compute resources used to run our jobs

Fargate or EC2 Compute Environments
Fargate is the recommended way of launching most batch jobs

When EC2 is recommended
* Custom API is needed: custom AMI can only be run on ec2
* vCPU requirments: anything needing more than 4 vCPUs need ec2
* Memory requirments: recommended for anything needing more than 30g memory
* GPU or Graviton CPU: if jobs require GPU, or Arm-based Graviton CPU
* linuxParameters: when using linux parameters
* number of Jobs: for large number of jobs, it will be dispacted at a higher rate than fargate

AWS batch vs AWS lambda
* Time Limit: Lambda has 15 mins time limit, Batch doesnt and can run longer than 15 min
* Disk space: Lambda has limited disk space, and EFS require functions to live within the VPC
* Runtime limitations: Lambda is fully serverless, but it has natively limited runtimes. (for custom longer runtimes we can use batch)
* Batch runtime: Batch uses docker, so any runtime can be used.
Depending on the usecase: for small jobs use lambda, for larger jobs use batch

Managed and Unmanaged Compute Environments
Managed
* AWS manages capacity and instance types
* Compute resources specs are defined when environment is created
* ECS instances are launched into VPC subnets (these instances will need network access to the ECS service endpoint)
* Default is the most recent and approved AWS ECS AMI image
* We can use our AMI
* We can leverage fargate, fargate spot, and regular spot instances to save on costs

Unmanaged
* We manage our own resources entirely
* AMI must meet AWS ECS AMI specs
* we manage everything
* Less commonly used
* Good choice for extremly complex or specific requirments

### Exam tips AWS batch
* Long-running Workloads: perfect for long-running and event-driven workloads. Anything requiring more than 15 minutes
* Managed services: AWS handles infrastructure creating and configurations
* Jobs and definitions: Jobs are work submitted to AWS batch, Job definetions are blueprints for the job, All jobs are placed in queues  
* Batch or Lambda: Lambda has limitations like runtimes, execution time limits, and file storage sizes.
* What type of compute: use case determine when to use fargate or ECS ec2 instances. Consider the number of jobs and specific resource requirments (eg: memory and GPU)
* Manage and unmanged: managed leverages AWS managing capacity and compute. They are the most common. Unmanaged allows us to manage EVERTHING. they are used for specific scnerios.


## Brokering Messages with AmazonMQ 
AmazonMQ Overview
SNS + SQS vs AmazonMQ
configuring brokers

AmazonMQ Overview
* Message broker: Managed broker service allowing easier migration of exisiting applications to the AWS cloud
* Variety: Leverages multiple programming languages, operating systems, and messaging protocols
* Engine types: currently supports ApacheMQ or RabbitMQ engine types
* Managed Services: Allows us to easily leverage existing apps without managing and maintaining our own system

SNS+SQS vs AmazonMQ
* Topics and Queues: Offered in both. Allows for 1-to-1 or 1-to-many messaging designs
* Existing application: if migrating exisiting app with a message system in place, we should consider AmazonMQ
* New applications: if new applications, we can consider SNS+SQS because its simpler to use, highly scalable and simple API
* Public accesibility: AmazonMQ REQUIRES private networking (VPC, Direct Connect or VPN). SNS+SQS are publicly accesible by default
AmazonMQ has NO default AWS intergrations

Configuring brokers
* Single instance brokers: 1 broker lives in 1 AZ. perfect use for dev-env. RabbitMQ has a Network load balancer in front
* Highly available: AmazonMQ offers highly avaliable architectures to minimize downtime during maintenance. Architecture depends on the broker engine type.
* AmazonMQ for ActiveMQ: With active/standby deployments, one instance will remain avaliable at all times. Configure network of brokers with separate maintenance windows.
* AmazonMQ for RabbitMQ: Cluster deployments are logical groupings of 3 broker nodes across multiple-AZs sitting behind a Network Load balancer.

### Exam tips for AmazonMQ
* Service Review: Managed broker service for easily migrating message broker system to the AWS cloud
* Engine Types: Allows us to leverage both Apache ActiveMQ or RabbitMQ engine types
* Specific Messaging Protocols: Anything specific to JMS or messaging protocols like AMQP 0-9-1, AMQP 1.0, MQTT, OpenWire and STOMP
* New or Existing Applications: New applications should try and leverage SNS with SQS. Exisiting applications with messaging systems may be a better fit for AmazonMQ.
* Private Networking Only: Amazon MQ restricts access to private networking. Must hava VPC connectivity (eg: Direct Connect or VPN)
* Single instance broker or Highly avaliable: Single instance broker configs are perfect for dev-env, each engine type also offers thier own type of highly avaliable congiuration.
* watch for keyword ApacheMQ, RabbitMQ, or any of the tips: think AmazonMQ


## Coordinating Distributed Applications with AWS Step Functions
AWS step functions overview
executions
workflows
state and state machines
intergated AWS services
different states

AWS step functions overview
* Orchestration: serverless orcestration service combining diffrent AWS services for business applications
* Graphical console: comes with graphical console for easier app workflow views and flows
* Componets: main componets are state machine and tasks
* State machine: a particular workflow with diffrent event-driven steps
* Task: Specific state within a workflow (state machine) representing a single unit of work
* State: every single step within a workflow is consisdered a state

Execution
AWS step function have 2 diffrent types of workflows:
* standard
* express

Workflow (each workflow has executions)
Executions are instances where you run your workflows in order to perform your tasks
standard: 
* have an exactly-once execution
* can run for up to one year
* useful for long running workflows that need to have an auditable history
* rates up to 2000 executions per sec
* pricing based per state transition 

express:
* at least once workflow execution
* can run up to 5 mins
* useful for high-event-rate-workloads
* examples are IoT data streaming and ingestion
* pricing is based on number of execution, durations and memory consumed

State and State Machines
* Flexible: Leverage state to either make decision based on input, perform certain actions or pass output.
* Language: States and workflows are defined in Amazon state language
* State: States are elements within your state machine. They are referred to by a name.
* Example: Think about an online pickup order. Each step in that workflow is considered a state.

Intergrated AWS services
* Lambda, Batch, DynamoDB, StepFunctions, ECS,..... etc

Diffrent States
* Pass: Passes any input directly to its output - no work done
* Task: Single unit of work performed (lambda, Batch and SNS)
* Choice: Added branching logic to state machines
* Wait: Creates a specified time delay within the state machine
* Succeed: Stops execution successfully
* Fail: Stops executions and marks them as failures
* Parallel: Runs parallel branches of executions within state machines
* Map: Runs a set of steps based on elements of an input arrary

### Exam tips
* Whats is AWS step functions: Serverless orchestration service meant for event-driven task executions using AWS services. Comes with a graphical interface.
* Execution types: Standard workflows are good for long-running, auditable executions. Express workflows are good for high event rate executions.
* Amazon states language: All state machines are written in amazon states language format.
* States: states are elements with our state machine. These are things and actions that happen with workflow
* Many integrations: So many AWS services intergration are avaliable. Common ones are AWS Lambda, API Gateway and Amazon EventBridge
* State types: There are currently 8 diffrent state types (pass, task, choice, wait, succeed, failm parallel, map)


## Ingesting data from SaaS Applications to AWS with Amazon AppFlow
What is AppFlow
Important Terms and Concepts
Usecase

What is Appflow
* Intergration: Fully managed intergration service for exchanging data between SaaS apps and AWS services
* Ingest Data: Pulls data records from third-party SaaS vendors and stores them in Amazon S3
* Bi-directional: Bi-directional data transfer with limited combinations

Important Terms and Concepts
* Flow: Flows transfer data between sources and destinations; a variety of SaaS apps are supported.
* Data Mapping: Determines how our source data is stored within our destinations
* Filters: Criteria to control which data is transforred from a source to a destination
* Trigger: How the flow stared. Supported types(Run on demand, Run on event, Run on schedule)

UseCase
* Transfering salesforce records to Amazon Redshift
* Ingesting and analysing Slack convrsation in S3
* Migrating Zendesk and other help desk support tickets to Snowflake
* Transferring aggregation data on a scheduled basis to s3 (up to 100gb per flow)

### Exam tips
What is appFlow?: fully managed intergration service for transferring data to and from SaaS vendors and applications
Bi-directional: Flows can be bi-directional between AWS services and SaaS applications. We should Ensure our configuration is supported.
Exam scenerio: Needing easy (managed) and fast transfer of SaaS or 3rd party vendor data into AWS services


### Exam tips recap
4 questions to ask
* are the workload synchronus or asynchronus?
* what type of decoupling make sense? (SNS, SQS)
* does the order of message matter? (FIFO or standard)
* what type of application load will we see?

SQS
* SQS can duplicate messages (if this is happening consistently, check for misconfigured visibility timeout, or if developer failed to make the delete API call)
* Queues are not bi-directional (if you need to communicate with the source, you'll need a 2nd queue)
* Know the default settings
* Nothing last forever (message in the SQS can only last for 14 days)
* Does order matter (if order matters, use FIFO)

SNS and API gateway
* Proactive notification (push notifications): think SNS
* Cloudwatch + SNS: notification from cloudwatch goes with SNS
* APIgateway: acts as a secure front door for external communcation to the application

AWS Batch
* Long running batch workloads (>15mins): if less than simple workloads <15mins AWS lambda is used
* Queued workloads: Batch uses queues
* On-demand alternative to AWS lamnda

AmazonMQ
* managed message broker service
* supports RabbitMQ or ActiveMQ engine types
* Specific messaging protocols: (JMS, AMQP 0-9-1, AMQP 1.0, MQTT, OpenWire and STOMP)

AWS Step-Functions
* Serverless orchestration service
* Different workflow decision requirments: whenever the solution requires different states or logic during workflows (eg: condition checks, failure catches, wait periods)

AWS AppFlow
* SaaS data ingestion: for architecure requiring data ingestion from external apps to AWS 
* Bi-directional: transfers data to and fro external apps (scenerio: data from 3rd party saas living in s3 on a regular basis)

