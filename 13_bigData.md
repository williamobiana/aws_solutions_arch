## Exploring large Redshift databases
3 Vs of Big Data
What is RedShift
RedShift Uses

3 big Vs
* Volume (Really big volume from terrabytes to petabytes)
* Variety (data from wide range of sources and formats)
* Velocity (Businesses require speed, Data needs to be collected, stored, processed and analyzed within a short period of time)

Redshift
* RedShift is a fully managed petabyte-scale data warehouse
* it is a very large relational-database traditionally used in big data applications

RedShift uses
* Cloud Data Warehouse: RedShift is where you can store massive amounts of data
* Size: it can hold up to 16pb of data. This means we dont have to split large datasets
* Relational: it is relational and uses standard SQL and BI tools to interact with it.
* Usage: While Redshift is fantastic tool for BI apps, it is not a replacement for standard RDS databases

### Exam tips
* redshiift is great for BI applications
* its relational
* it can store up to 16PB of data
* Not standard: Redshift and RDS each have thier use-case

## Processing Data with EMR (Elastic Map Reduce)
What is ETL (extract transform load)
What is EMR
EMR architeture

what is ETL
* Extract: mine raw data
* Transform: transform raw data to usable data
* Load: store data and deliver to clients

EMR (Elastic Map Reduce)
* big data platform that allows us to process big amounts of data using open source tools like spark, hive, HBase,.. etc
* it is an AWS ETL tool that manages the ETL process.

### Exam tips
* EMR is a managed fleet of ec2 instances running open-source tools
* ec2 rules apply (we can use Reduced Instances and Spot instances to reduce cost)
* EMR is used to process and move data
* The architecture lives inside a VPC


## Streaming Data with Kinesis
what is kinesis
kinesis data stream
kinesis data firehose
kinesis data analytics and SQL
kinesis vs SQS

What is kinesis
a big pipeline to your AWS account to collect, process and analyze real-time streaming data

data stream vs data firehose
data stream
* purpose: real-time streaming for ingesting data
* speed: real-time
* difficulty: you're responsible for creating the consumer and scaling the stream

data firehose
* purpose: data transfer tool to get information to s3, redshift, elasticsearch, or splunk
* speed: near real time (within 60 seconds)
* difficulty: plug and play with AWS architecture


* kinesis data stream is manual configurations of the producers, shards in the pipeline for processing, consumers to receive the data and endpoints.
* kinesis data firehose is AWS managed version of Kinesis data stream

kinesis data analytics and SQL
we can pair analytics with firehose or datastream using SQL
* easy: it is simple to connect
* no servers: it is serverless, it automatically handles scaling and provisioning of needed resources
* cost: you only pay for the amount of resources you consume as your data passes through

kinesis vs SQS
note: if we are looking for a message broker which service do we pick?
SQS
* SQS is a messaging broker, that is simple to use and doesnt require much configuration.
* it doesnt offer real-time message delivery

Kinesis
* more complicated to configure
* mostly used in big data applications
* provides real-time communication

### Exam tips
* if scenerio talks about real time: Think Kinesis
* kinesis is one of the few services that offer real-time speed
* if scenerio says near real time: Think kinesis data firehose
* if scenerio says real time: Think kinesis data stream
* if scnerio say streaming data: Think kinesis
* Kinesis & SQS are message brokers: but kinesis offers real time
* Transforming data: data analytics is the easiest way to process data going through kinesis using SQL
* Scaling: Data streams does not automatically scale. Data firehose does


## Athena and AWS Glue 
What is athena (Serverless SQL)
What is glue (serverless ETL)
Using them together

Athena and Glue are both serverless

Athena
* interactive query service that makes it easy to analyze data in S3 using SQL.
* This allows you to directly query data in your S3 bucket without loading it into a database

Glue
* serverless data intergration service that makes it easy to discover, prepare and combine data.
* it allows you to perform ETL workloads without managing underlying servers (ec2)

Athena+Glue
* S3 - GlueCrawlers - GlueCatalog - Athena - Quicksight
* S3 - GlueCrawlers - GlueCatalog - RedShift Spectrum

### Exam tips
* scenerio looking for serverless SQL: think athena
* serverless: both athena and glue is serverless
* overview: athena is serverless SQL, glue is serverless ETL
* better together: they work well together


## Visualizing Data with Quicksight
What is QuickSight
* fully managed business intelligence (BI) data visualization service. It allows you to easily create dashboards and share them

### Exam tips
* scenerio talking about sharing data, interpreting data, anything related to business intelligence: think QuickSight


## Moving Transforming data using AWS data pipeline
AWS Data Pipeline Definition
AWS Data Pipeline Overview
Components to know
Popular Use Cases

AWS data pipeline is a managed Extract, Transform, Load (ETL) service for automating movement and transformation of your data.

Overview
* Data driven: define data-driven workflows. Steps are dependent on previous tasks completing successfully
* Parameters: define your parameters for data transformations. AWS data pipeline enforces your chosen logic.
* Highly available: AWS hosts the infrastructure on highly available and distributed infrastructure. It is also fault tolerant.
* Handling failures: Automatically retries failed activites. Configure notifications via amazon SNS for failures or even successful tasks
* AWS storage services intergrates easily with Amazon DynamoDB, Amazon RDS, Amazon Redshift, and Amazon S3
* AWS Compute: works with ec2 and EMR for compute needs

Components
* pipeline definition: specify the business logic of data management needs
* managed compute: service will create ec2 instance for you or leverage existing ec2
* task runners: task runners (ec2) poll for different tasks and perform them when found
* data nodes: define the locations and types of data that will be input and output
Activites are pipeline components that define the work to perform

use-case
* Processing data in EMR using Hadoop streaming
* Importing or exporting DynamoDB data
* Copying CSV files or data between S3 buckets
* Exporting RDS data to S3
* Copying data to Redshift

### Exam tips
* Managed ETL: managed ETL workflows that automates movements and transformation of your data
* Data driven: data-driven workflows to create dependencies between tasks and activites
* Storage intergrations: intergrates with dynamoDB, RDS, Redshift and S3
* Compuute Intergrations: intergrate with ec2 and EMR for managed compute needs
* SNS: intergrate with SNS for any failure notifications. Use it for successes and other event-driven workloads
* Keywords: anything related to managed ETL services and automatic retries

## Implementing Amazon Managed Streaming for Apache Kafka
MSK overview
important components and concepts
resiliency in MSK
good to know
security and logging

MSK
* apache kafka: managed streaming service used for apache kafka
* control plane: provides control plane operations (create, update and delete clusters)
* data plane: leverage kafta data-plane operations for producing and consuming streaming data
* existing applications: allows open-source kafka alternatives to support existing apps, tools and plugins

important components and concepts
* broker nodes: specify the amount of broker nodes per AZ, you want at time of cluster creation
* zookeeper nodes: they are created for you
* producers, consumers and topics: data-plane operations allow creation of topics and ability to produce/consume data
* flexible cluster operations: perform cluster operations with the console, AWS CLI, or API within any SDK

resiliency in MSK
* automatic recovery: automatic detection and recovery from common failure scenarios. Minimal impact
* detection: detected broker failures result in mitigation or replacement of unhealthy nodes.
* reduce data: tries to reuse storage from older brokers during failures to reduce data needing replication
* time required: impact time is limited to however long it takes MSK to complete detection and recovery
* after recovery: producers and consumer apps continue to communicate with the same brokerIP as before

good to know
* MSK serverless: serverless cluster management
* fully compatible: fully compactible with kafka
* MSK connect: easily stream data

security and logging
* intergrate with KMS for server side encryption (SSE)
* encryption at rest by default
* uses TLS 1.2 for encryption in transit between brokers in clusters
* deliver broker logs to cloudwatch, s3, and kinesis data firehose
* metrics are gathered and sent to cloudwatch
* MSK api calls are logged to AWS CloudTrail

### Exam tips
Apache kafka: fully managed AWS streaming service used for apache kafka
* control plane: provides control plane operations (create, update and delete clusters)
* data plane: leverage kafta data-plane operations for producing and consuming streaming data
* automatic recovery: automatic detection and recovery from common failure scenarios. Minimal impact
* encryption: intergrate with KMS for server side encryption (SSE). uses TLS 1.2 for in transit communications
* logging: push broker logs to cloudwatch, s3, and kinesis data firehose. API calls are logged to CloudTrail


## Analyzing data with amazon OpenSearch service
what is open search
service features

open search
* OpenSearch is a managed service allowing you to run search and analytics engines for various use case
* it is the successor to amazon elasticsearch service

service features
* quick analysis: quickly ingest, search, and analyze data in your cluters. commonly part of an ETL process.
* scalable: easily scale cluster infrastructure running the opensearch services
* security: leverage IAM access control, VPC security groups, encryption in rest and in transit and field-level security
* stability: multi-AZ capable service with master nodes and automated snapshots
* flexible: it allows for SQL support for business intelligence (BI) apps
* intergratons: easily intergrate with Cloudwatch, CloudTrail, S3 and kinesis

### Exam tips
* OpenSearch service loves logs
* if given a scenerio about creating a logging solution involving visualization, log file analytics or BI reports: think OpenSearch
* Elasticsearch is also the same as OpenSearch

## Exam tips recap
4 questions to ask
* what kind of database works for this scenerio? RDS or Non-RDB
* how much data do we have? (what is the size-limit so we can pick the approraite service)
* is serverless required (do we need to implement serveless, ec2 or on-prem)
* how do we optimize cost (how do we implement the services in the cheapest way)

Redshift and EMR
* While Redshift is fantastic relational database for BI apps, it is not a replacement for standard RDS databases
* Redshift only supports single AZ deployments, you can create multiple clusters in different AZs but they are seperate deployments. Redshift is not highly avalaible.
* EMR is a managed fleet of ec2 instances running open-source tools. ec2 rules apply (we can use Reduced Instances and Spot instances to reduce cost)

Kinesis, Athena and Glue
* Kinesis is the only service with a real-time response. If the question asks for a real-time solution to processing or moving data, look for an answer that includes kinesis
* SQS and kinesis can both be queues. Each service has its pros and cons. SQS is easier and simpler and kinesis is faster and can store data for up to a year.
* Serverless SQL means Athena. Anytime serverless SQL or querying data in S3, think athena.
* Glue is serverless ETL. it can help create that schema for your data when paired with athena

Quicksight, OpenSearch/Elastic search (visualization)
* creating a dashboard: QuickSight is your go-to tool for visualizing data.
* OpenSearch is primarily used for analyzing log files and various documents, especially within an ETL process
* OpenSearch and ElasticSearch are very similar, they both follow the same concepts

Datapipeline
* it is a managed ETL service in AWS
* it implements automated workflows for movement and transformation of your data
* it intergrates with storage services (RDS and S3) and compute services (ec2 and EMR)
* it is data driven and task dependent in our ETL workloads

MSK (managed streaming for apache kafka)
* it is a managed service for building and running kafka streaming applications
* the service handles control plane operations for you (create, update and delete)
* you can manage data plane operations
* push broker logs to cloudwatch, s3, or kinesis data firehose
* API calls are all logged to cloudtrail












