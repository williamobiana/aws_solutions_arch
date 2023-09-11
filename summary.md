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

SECURITY: CLOUDTRAIL SHEILD WAF GUARDDUTY FIREWALL_MANAGER MACIE INSPECTOR KMS CLOUDHSM SECRETS MANAGER PARAMETER STORE PRESIGNED(COOKIES) IAM_DOCS CERTIFICATE_MANAGER AUDIT_MANAGERS ARTIFACTS COGNITO DETECTIVE NETWORK_FIREWALL SECURITY_HUB 
* log the API calls with cloudtrail
* protect the application on layer 3 and 4 with sheid
* filter the traffic on layer 7 with WAF (web app firewall)
* guard the network with threat detection using guardDuty
* use the central firewall manager to manage WAF on multiple AWS account
* monitor s3 buckets containing sensitive documents with MACIE
* perform the venureability scans on ec2 and VPC with incepector
* manage the encryption keys on KMS and CLOUDHSM 
* store the secrets credentials and perform key rotations in the secret manager
* strategy: to save costs on secret manager, use parameter store with limit of 10000 parameters
* temporarily share s3 objects using signed_URLs or cookies
* use the other user's security credentials, and other parameters to create the presigned_URL 
* if the user wants temporary access to multiple buckets, then use a presigned cookie
* create and manage SSL certificates with the certificate manager, intergate it with API_gate,load_balancer, the CLOUDFRONT dashboard to track the flow of traffic
* automate our auditing to ensure we are up to date with AUDIT_MANAGER
* download the reports with ARTIFACTS
* authenticate access to application using cognito
* with cognito, use userpools to get tokens, exchange tokens for credentials with identitypools, use credential to access the services
* analyze and identify root cause of suspicious activity using detective
* protect the vpc with network firewall, and use it together with firewall manager
* collect security data with security hub
* filter network traffic before it reached IGW with network manager, use it in combination with firewall manager to manage the WAF on multiple AWS account

MONITORING: CLOUDWATCH CLOUDWATCH_LOGS PROMETHEUS GRAFANA
* check logs on a high level with cloudwatch
* cloudWatch Logs, allows us to monitor, store and access logs files from diffrent sources.
* to process the logs and check on a high level, use cloudwatch
* to process the logs in real time, use kinesis
* to query the logs, use cloudwatch logs insights
* query the metrics and log of diffrent services and create a visualization dashboard with grafana
* to visualize containers, connect grafana to prometheus

AUTOMATION: CLOUDFORMATION ELASTIC_BEANSTALK SYSTEMS_MANAGER
* automate the creation and deployment of infrastruture with cloudformation
* automate the creation, deployment and management of all the services needed to run an application with Elastic_beanstalk
* organize the tools to automate the management of ec2 architecture with systems_manager

CACHING: CLOUDFRONT ELASTIC_CACHE DAX GLOBAL_ACCELERATOR
* use CloudFront CDN to deliver the stattic content to global users, 
* Elastic cache use 2 open source technologies (memcache and Redis)
* memcache is a simple cache
* redis is a cache with a NoSQL database
* DAX is an in-memory cache for DynamoDB
* setup a Global accelerator networking service in front of our loadbalancer for our app, to solve the problem of IP caching

GOVERNANCE: ORGANIZATIONS RAM ROLEACCESS CONFIG DIRECTORY STORE COST_EXPLORER BUDGETS CUR COMPUTE_OPTIMIZER TRUSTED_ADVISOR CONTROL_TOWER LICENCE_MANAGER HEALTH SERVICE_CATALOG PROTON WELL_ARCHITECTED_TOOL 
* centralize all the service control polices and the logs from cloudtrail in the main account using organizations
* strategy: if we need to share resources directly to other accounts in the same organization use resource account manager RAM, alternatively, we can assume the role of another account to access the resources using CROSS_ACCOUNT_ROLE_ACCESS. it is better than creating new IAM credentials for RAM
* if we need to set up a rule or configuration for a new account and manage any changes to the rule for compliance, use config
* strategy: when managing our users, use SSO for internal users and COGNITO for external users
* track costs with COST_MANAGEMENT, use in combination with EXPLORER, BUDGETS, COST, USAGE_REPORTS and COMPUTE_OPTIMIZER
* audit our best practice with trusted adviser
* manage all the accounts and licences with CONTROL_TOWER and LICENCE_MANAGER, we can add guardrails to control tower with SCPs and CONFIG
* strategy: we can provision pre-approved services with SERVICE_CATALOG, this will be written with CLOUDFORMATION. 
* manage our resource performance with health

MIGRATION: SNOWFAMILY DATASYNC TRANSFER_FAMILY STORAGE_GATEWAY MIGRATION_HUB APPLICATION_DISCOVERY_SERVICE APPLICATION_MIGRATION_SERVICE SERVER_MIGRATION_SERVICE
* depending on the TB of data we are moving, we will select a member of the snowfamily by size
* to merge our on-prem with AWS, we can use storage gateway, if we run out of space, we can use file gateway
* run a 1 time migrarion to EFx and FSx with data sync and if we need to transfer our old apps to AWS or allow the old apps use s3m then we can use TRANSFER_FAMILY
* use the migration hub to organize and manage the migration steps for databases and servers
* collect the usage and configuration data for our application, databases and servers with DISCOVERY_SERVICE and migrate them with MIGRATION_SERVICE

FRONTEND_WEB_MOBILE: AMPLIFY DEVICE_FARM PINPOINT
* use amplify to build full stack applications
* use device farm to test andriod, ios and web apps
* pinpoint is intended for marketers to communicate with users

ML: COMPREHEND KENDRA TEXTRACT TIME_SERIES FORECAST FRAUD_DETECTOR POLLY TRANSCRIBE LEX REKOGNITION SAGEMAKER
* analyse texts using AWS comprehend, kendra, textract
* predict time-series data using amazon forecast
* protect accounts with amazon fraud detector
* work with text and speech using AWS polly, transcribe, and lex to create alexa models
* analyzing images and facial regocnition with Rekognition
* use Sagemaker to train ML learning models

MEDIA: TRANSCODER KINESIS_VIDEO_STREAMS
* convert(transcode) media files with transcoder
* stream media with kinesis video streams






