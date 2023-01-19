IAM: admin
* secure root account
* control user's actions with IAM policy
* give user's IAM credentials

S3: hardware-storage 
* basic storage
* buckets
* key-value store 
* avaliability and durability
* storage classes [standard, IA(standard & 1AZ), intelligent tiering, glacier]
* secure the data
* make it consistent (consistency model)
* securing the bucket by blocking public access
* hosting a static website
* versioning the stored objects
* improving peformance of storage classes
* storage costs
* managing the object's lifecycle (lifecycle management)
* locking the objects in glacier valut
* encrypting the objects
* optimize the storage performance
* backing up the objects

EC2: VMs
* types [on-demand, reserved, spot, dedicated]
* price
* saving plans
* command-line
* giving IAM-roles
* security groups for protection
* bootscript
* retrive metadata
* save with userdata
* adding network cards [ENI, EN, EFA]
* add to a placment group (rack)
* strategy: solve licence issues using dedicated VMs
* strategy: reduce cost with spot VMs or Fleet of spot VMs
* strategy: create VM version of on-prem
* strategy: bring VMs to on-prem

EBS & EFS: cpu-storage 
* when we use EBS (mission critical)
* EBS types [SSD (general purpose-gp2, provisioned IOPS-io1), HDD (transmission optimized, cold)]
* when to use SSD IOPS or HDD transmission optimized storage
* attach volumes (virtual external hard-disk) to the cpu-storage
* create snapshots
* encrypting the volumes
* saving the data on the volumes, while the ec2 is hibernating
* when we use EFS 
* when we use FXs [windows, lustre]
* creating a launch template (AMI) for the ec2 
* startegy: diffrentiate between data stored on the ec2 storage (instance-store) and the EBS volume 
* backing up data using ``BACKUP``
* using ``BACKUP`` and ``ORGANIZATION``

DATABASE: relational (tables) and non-relational
* choosing a relational database engine [SQL server, Oracle, MySQL, PostgreSQL, MariaDB, Aurora]
* processing (online transactions) and (online analyticics) on databases (OLTP vs OLAP)
* improving a database read performance by creating read replcas
* when we use Aurora
* using a non-relational database [dynamoDB]
* processing transactions using dynamoDB
* backup and save data with dynamoDB
* make the data globally avaliable using dynamoDB streams and global tables
* when to use document databases [mongoDB, documentDB]
* running big-data workloads on NoSQL databases with ``CASSNDRA`` and ``KEYSPACES``
* create graphs with ``NEPTUNE``
* store ledgers with ``QUANTUM LEDGE DATABASES``
* analysing time series with ``TIMESTREAM``

VPC: Virtual datacenter network
* customize the features we need
* create a network
* using NAT gateway for internet access
* protect the resources with a security group
* control the traffic to the subnets with a Network_ACL
* set up private communication between our VPC and other AWS services using endpoints
* connect multiple VPC using peering
* expose the VPC resources to another VPC using a private link
* connect on-prem datacenter to VPC using direct connect
* simplify the on-prem to VPC connection using a router (transit gateway)
* use ``WAVELENGHT`` for mobile computing with 5G

ROUTE53: DNS 
* configure the DNS record types [SOA, NS record, A record, CNAME record]
* configure the Alias record
* register the domain name
* set up the routing policies [simple, weighted, failover, geolocation, geoproximity, latency, multi-value]

LOAD-BALANCING: ELB (for routing traffic)
* types [application (HTTP/HTTPS), network (TCP/UDP/TLS), classic]
* configure the load balancer to run health checks on the EC2
* set up Application LB for layer 7 routing
* configure the listeners, rules and target group on the ALB
* improve extreme performance with Network LB for layer 4 routing
* configure rules for recieved requests, listeners, target groups for the NLB
* select the protcols and open the ports
* encrypt the Network load balancer
* for legecy configuration, use a classic load balancer
* bind a user's browsing session to an ec2 using sticky sessions
* strategy: if we need to keep connection open when an ec2 becomes unhealthy, enable deregistration delay

HIGH-AVALIABILITY AND SCALING: 
* automatically scale the number of ec2 with all the necessary settings using launch templates
* use best practice to write the launch template
* strategy: add a collection of ec2 to make an autoscaling group, to make it easy to scale
* define the steps and set the scale limit 
* specify policies to scale out aggressively or scale in conservatively depending on the conditions [reactive, scheduled, predictive]
* scale RDS in the following ways [vertical, scaling storage, read replica, Multi-AZ, aurora for unpredictable workload]
* scale dynamoDB in the following ways [provisioned capacity for predictable workloads, on-demand for unpredictable workloads]

DECOUPLING WORKFLOW: ``SQS``, ``SNS``, ``API-GATEWAY``, ``BATCH``, ``AMAZON-MQ``, ``STEP-FUNCTIONS``, ``APPFLOW``
* strategy: use loose coupling as an effecient way for services to communucate with each other in the architecture
* manage the incoming/outgoing messages in the architecture, by adding them to ``SQS`` queue.
* configure the ``SQS`` settings
* configure the ``SQS dead-letter-queue`` for messages that couldnt be delivered and needs to be re-processed
* organize the messages in an ordered manner to prevent duplication using the ``SQS FIFO`` settings
* send notifications to services/devices using ``SNS``
* configure the ``SNS`` settings
* strategy: ``SNS`` + ``CLOUDWATCH`` = get notification and check the logs
* manage the communication between services in the architecture with ``API-GATEWAY``
* security strategy: add a ``WAF`` in front of ``API-GATEWAY`` to prevent DDoS attacks
* run short workloads/tasks using ``LAMBDA``
* run long workloads/tasks in batches/bulk by using ``BATCH``,
* strategy: let AWS manage the compute resources for the workload with ``FARGATE``, or manage manually for custom configs
* exchange the messages between services using a ``AMAZON-MQ`` message broker for existing infrastructure
* exchange the messages betwen services using ``SNS+SQS`` for new applications
* automate a step by step complex workflow of services with ``STEP-FUNCTIONS``
* collect data from SaaS applications and other AWS services using ``AppFlow``

SERVERLESS: ``LAMBDA``, ``FARGATE``, ``ECS``, ``EKS``, ``EVENTBRIDGE/CLOUDWATCH EVENT``, ``GLUE`` ``ECR``, ``AURORA``, ``X-RAY``, ``APPSYNC``
* build a ``LAMBDA`` function to automate simple workflows
* Leverage the AWS serverless application repository framework/template
* use ECS for containerization
* use EKS for conttainerization, if it is on-prem, open-source, requires kubernetes
* strategy: let AWS manage the compute resources for the workload with ``FARGATE`` on ECS or EKS
* create rules to pass an event from a source to end point using EventBridge (cloudwatch for events)
* if ETL jobs are required in the archtecture, consider using ``GLUE`` as a response to the event and hold the architecture together
* store the docker image on ECR and assign polices and rules
* for unpredictable workloads on a database, opt for serveless version of ``AURORA`` with automatic scaling
* collect application data for viewing, filtering and analyzing with ``X-RAY``
* if we need to query the API with GraphQL we can use ``APPSYNC``

BIGDATA: ``REDSHIFT``, ``EMR``, ``KINESIS``, ``ATHENA``, ``GLUE``, ``QUICKSIGHT``, ``OPENSEARCH/ELASTICSEARCH``, ``DATAPIPELINE``, ``MSK``
* deploy a data warehouse using ``REDSHIFT``
* run ETL workloads and process big data on ``EMR``
* stream real-time data with ``KINESIS``
* strategy: for simple message brokers use ``SNS+SQS``, for complex use ``AmazonMQ``, for real-time use ``KINESIS``
* strategy: to transform and query the data, use SQL in ``KINESIS``
* to run serverless SQL queries use ``ATHENA``
* to run serveless ETL workloads use ``GLUE``
* strategy: sample ``GLUE`` architecture with ``ATHENA`` [S3 - GlueCrawlers - GlueCatalog - Athena - QuickSight]
* strategy: sample ``GLUE`` architecture with ``REDSHIFT`` [S3 - GlueCrawlers - GlueCatalog - RedShift Spectrum]
* visualize the data for business interlligence using ``QUICKSIGHT``
* build out an automated data pipeline to manage ETL workloads with ``DATAPIPELINE`` and store in a database
* manage data streaming using ``MSK`` (managed streaming for Kafka) and send logs for analysis or storage
* search for log files, analyze the log files, visualize and create BI reports with ``OPENSEARCH``

SECURITY: 












## Word Association
S3 = storage
EBS/EFS = storgage
EC2 = VM
RDS = Database (MySQL/relational)
Aurora = 5x RDS Database
DynamoDB = Database (NoSQL/non-relational)
VPC = datacenter/network
SQS = queue
SNS = notification
API-GATEWAY = communication
CLOUDWATCH = check logs (high level)
CLOUDTRAIL = record details of the logs (granular)
EVENTBRIDGE = customized version of cloudwatch for checking events a.k.a cloudwatch events
SNS + CLOUDWATCH = get notifaction and check the logs
SES = email service
WAF = firewall
WAF + API-GATEWAY = firewall in front of communication interface
LAMBDA = automated short and small tasks
BATCH = automaed long and heavy tasks in batches
AmazonMQ = message broker
ApacheMQ = message broker engine
RabbitMQ = message broker engine
SNS+SQS = simpler alternative for AmazonMQ
STEP-FUNCTIONS = automated complex workflow/tasks of services
APPFLOW = collect data from 3rd party apps
AURORA SERVERLESS = unpreditctable workload for databases
APPSYNC = GraphQL API query
REDSHIFT = data warehouse
EMR = run ETL workloads and process big data
KINESIS = real time data streaming
KINESIS DATASTREAM = real time data streaming for processing in compute
KINESIS FIREHOSE= real time data streaming for transfer to storage
ATHENA = serveless SQL
GLUE = serverless ETL
ATHENA+GLUE = run ETL workload and query with SQL
QUICKSIGHT = BI visualization tool
DATAPIPELINE = automated data pipeline to manage ETL workloads
EMR+GLUE = process bigdata and run ETL workload
MSK = manage streaming for Kafka
OPENSEARCH = search, analyze, visualize log files for BI reports






