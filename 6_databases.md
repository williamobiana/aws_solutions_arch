## RDS (Relational Database Service) Overview

Relational databases
RDS advantage
OLTP vs OLAP
Multi-AZ
Unplanned Failures and Maintenance

Relational databases
* data is stored in tables (like a spreadsheet)
* rows contain the data items
* columns are the field in the tables

Relational database engines
* SQL server
* Oracle
* MySQL
* PostgreSQL
* MariaDB
* Aurora

RDS Advantages
* can be up and running in minutes
* multi-AZ
* failover capability
* automated backups

OLTP (online transaction processing) vs OLAP (online analytical processing)
OLTP
* RDS is generally used for online transaction processing (OLTP)
* OLTP processes data from transactions in real time (customer orders, banking transactions, payments and booking systems)
* it is all about data processing and completing large amounts of small transactions in real time

OLAP
* processing complex queries to analyze historical data
* it is all about data analysis using large amounts of data as well as complex queries that take a long time to complete
* RDS is not suitable for analyzing large amounts of data (we need to use a data warehouse like Redshift which is optimized for OLAP)

Multi-AZ RDS creates a copy of our prod database in another AZ
AWS handles the replication for us
when we write to our prod database, it will be copied to our backup database

RDS types that can be configured as Multi-AZ
All these can be in stand alone AZ EXCEPT aurora (aurora MUST be Multi-AZ)
* SQL server
* Oracle
* MySQL
* PostgreSQL
* MariaDB

RDS will automatically failover to standby during a failure so database operation can resume quickly without admin intervention
Multi-AZ is for disaster recovery, not for improving performance, we cannot connect to seconday datadabe when primary is active.

### Exam tips
* RDS database types (SQL, MySQL, postgreSQL, MariaDB, Oracle, aurora)
* RDS is for OLTP workloads (simple transactions)
* Not suitable for OLAP workloads (large analysis)


## Increase Read Performance with Read Replicas
what is a read replica
key facts

Read replica are use to increase performace in RDS
Read replicas are read-only copy of our primary database
it is great for read heavy workloads and takes the load off the primary database

Note: 
Multi-AZ is for disaster recovery only
Read replica is for boosting performance

Read replicas can be on same AZ or cross regional
Each read replica has its own DNS endpoint (1 for primary and 1 for replica)

Read replica can be promoted to become its own independent database, although this breaks the replication for the app.

Key facts
* scaling read performance: primarily used for scaling, no disaster recovery
* requires automatic backup: automatic backups must be enabled in order to deploy a read replica
* multiple read replicas are supported: allows up to 5 read replicas to each DB

### Exam tips
Multi-AZ:
* an exact copy of your production database in another availability zone
* used for disaster recovery
* in the event of a failure, RDS will automatically fail over to the standby instance

Read Replica:
* read only copy of your primary database in the same AZ, cross-AZ, or cross-region
* used to increase or scale read peformance
* great for read-heavy workloads and takes the load off your primary database for read-only workloads


## What is Amazon Aurora
What is aurora
Aurora performance
basics
scaling 
types of aurora replicas
aurora backups
aurora serverless

what is aurora?
aurora is a mySQL and postgreSQL compatible relational database engine that combines the speed and avaliability of high-end commercial database with the simplicity and cost-effectiveness of open-source databases

5x Performance
Aurora provides up to 5 times better performance than MySQL and 3 times better than PostgreSQL databases at a much lower price point, while delivering similar performance and availability.

things to know about aurora
* start with 10TB. scales in 10gb increments to 128TB (storage Auto scaling)
* compute resources can scale up to 96 vCPUs and 768 of memory
* 2 copies of your data are contained in each Availiability zone, with a minimum of 3 AZs. 6 copies of your data.

scaling aurora
* Aurora is designed to transparently handle the loss of up to 2 copies of data without affecting database write availability and up to 3 copies without affecting read availability.
* Aurora storage is also self-healing. Data blocks and disks are continuously scanned for errors and repaired automatically.

3 types of aurora replicas
* aurora replicas: you can currently have 15 read replicas
* mySQL replicas: you can currently have 5 read replicas with aurora MySQL
* postgreSQL: you can currently have 5 read replicas with Aurora postgreSQL

Backups with Aurora
* Automated backups are always enabled on aurora DB instances, backups do not impact database performance.
* You can also take snapshots with aurora, this also does not impact on performance.
* You can share Aurora snapshots with other AWS accounts

Aurora serverless
* aurora serverless db cluster automatically start up, shuts down, and scales capacity up or down based on your application's needs
* usecase: aurora serverless provides a relatively simple, cost-effective option for infrequent, intermittent or unpredictable workloads

### Exam tips
* 2 copies of our data are contained in each AZ with a minimum of 3 AZs (total of 6 copies of our data)
* You can share Aurora snapshots with other AWS accounts
* 3 types of aurora replicas (aurora replicas with automated failover, mySQL replicas, postgreSQL)
* aurora has automated backups turned on by default, we can take snapshots and share it
* we can use serverless for simple cost-effective option for infrequent, intermittent or unpredictable workloads


## Dynamo DB Overview
What is dynamo DB
High-level DynamoDB
Read Consistency
DynamoDB Accelerator (DAX)
on-demand capacity
security

What is dynamoDB
fast and flexible NoSQL database, fully managed and supports documents and key-value data models

4 facts about dynamoDB
* stored on SSD storage
* spread across 3 geographical distinct data centers
* eventually consistent reads (default)
* strongly consistent reads

eventually consistent reads vs strongly consistent reads
* eventually: consistency across all copies of data is reached within 1 second (best read performance)
* strongly: consistent read that returns a result that reflects all writes that recieved a successful response prior to the read

dynamoDB accelerator (DAX)
* fully managed, highly avaliable, in memory cache
* 10x performance improvement
* reduces request time from milliseconds to microseconds - even under load
* no need for developers to manage caching
* compactible with dynamoDB API calls

on demand capacity
* pay-per-request
* balance cost and performance
* no minimum capacity
* no charge for read/write - only storage and backup
* pay more per request than with provisioned capacity
* use for new product launches

security
* encryption at rest using KMS
* site to site VPN
* direct connect (DX)
* IAM polices and roles
* Fine grained access
* CloudWatch and CloudTrail
* VPC endpoints

### Exam tips
4 facts about dynamoDB
* stored on SSD storage
* spread across 3 geographical distinct data centers
* eventually consistent reads (default)
* strongly consistent reads

eventually consistent reads vs strongly consistent reads
* eventually: consistency across all copies of data is reached within 1 second (best read performance)
* strongly: consistent read that returns a result that reflects all writes that recieved a successful response prior to the read


## When do we Use DynamoDB Transactions
ACID diagram
ACID with dynamoDB
All or Nothing
Use cases
Transactions

ACID
is a methodology for using databases
* Atomic: all changes to data must be performed successfully or not at all
* Consistent: data must be in a consistent state before and after the transaction
* Isolated: no other process can change the data while transaction is running
* Durable: changes made by a transaction must persist

ACID with dynamoDB
* dynamoDB provide deveopler with ACID (atomic, consistent, isolated, durable) across 1 or more tables within a single AWS account and region
* we can use transactions when build apps that require coordinated inserts, deletes, or updates to multiple items as part of a single logical business operation

Acid means all or nothing
* All: Transaction succeeds across 1 or more tables
* Nothing: Transactions fails

Use-case
* processing financial transactions
* fulfilling and managing orders
* building multiplayer game engines
* coordinating actions across distributed components and services

Transactions
* Multiple "all or nothing" operations
* financial transactions
* fulfilling orders
* 3 options for reads: eventual consistency, strong consistency, and transactional
* 2 options for writes: standard and transactional
* up to 25 items or 4MB data per transaction

### Exam tips
* ACID requirments, think dynamoDB
* dynamoDB provide deveopler with ACID (atomic, consistent, isolated, durable) across 1 or more tables within a single AWS account and region
* all or nothing transaction


## Saving data with dynamoDM backups
ondemand backup and restore
point in time recovery

on demand backup and restore
* full backups at any time
* zero impact on table performance or avaliablity
* consistent within seconds and retained until deleted
* operates within same region as the source table

point in time recovery
* protect against accidenty writes or delete
* restore to any point in the last 35 days
* incremental backups
* not enabled by default
* latest restorable: 5 minutes in the past


## Taking your data global with dynamoDB streams and global tables
streams
global tables

streams
* time ordered sequence of item-level changes in a table
* stored for 24 hours
* inserts, updates and delete
* combine with lambda functions for functionality like stored procedures

global tables
* managed multi-master, multi-region replication
* globally distributed apps
* based on dynamoDB streams
* multi-region redundancy for disaster recovery or high availability
* no application rewrites
* replication latency is under 1 second

### Exam tips
* managed multi-master, multi-region replication
* globally distributed apps
* based on dynamoDB streams
* multi-region redundancy for disaster recovery or high availability
* no application rewrites
* replication latency is under 1 second


## Operating MongoDB-compatible Database in Amazon DocumentDB
What is mongoDB
Example
What is documentDB
why use it

mongoDB is document database that allows for scalability and flexibility with our data as well as robust querying and indexing

amazon documentDB allows us to run mongoDB on the cloud. it is a managed database service that scales with your workloads and safely and durably stores yuor database information.

why use it?
to get rid of operational overhead
get rid of manual workload 

### Exam tips
moving mongoDB database to aws
* mongo on prem - AWS database migration service - Amazon DoucmentDB


## Running Apache Cassandra Workloads with Amazon Keyspaces
what is cassandra 
what is keyspaces
why use it

cassandra is a distributed NoSQL database (runs on many machines). primarily used for big data solutions.

amazon keyspaces is a fully managed cassandra database service, that runs cassandra workload on AWS

why use keyspaces? because it is serverless, we only pay for the resources we use

### Exam tips
* migrating a big data cassandra cluster to AWS: think keyspaces


## Implementing Graph databases using Amazon Neptune
what is a graph database
neptune
use cases

graph database
data is stored as a relationship graph

neptune is AWS graph database service

use-case for neptune
build connections between identities
build knowledge graph applications
detect fraud patterns
security graphs to improve IT security

### Exam tips
* neptune is a distractor question, unless they specifically ask about graph database


## Leveraging Amazon Quantum Ledger Database (QLDB) for Ledger Database
What is a ledger database
Amazon Quantum ledger database QLDB
QLDB use case

A ledger is a NoSQL database that is IMMUTABLE, transparent, and cryptographic transaction log owned by one authority
you cannot update a record (immutable), instead it will add a new record to the database

used for cypto currencies
tracking cargo in logistics
creationg and distributon in big pharma

QLDB is a fully managed ledger database

use case
* store financial transactions
* store record for supply chain
* maintain claim history
* centralized digital records

### Exam tips
* QLDB is a distractor question, unless they specifically ask about immutable database


## Analyzing Time-series data with amazon timestream
time series data
examples
amazon timestream

time series data are data points that are logged over a series of point in time, allowing us to track our data.

examples
IoT (iot sensors tracking data)
Analytics (large website tracking incoming and outgoing traffic)
devops applications (apps that change in response to users needs)

amazon timestream
fulling managed serverless database service for time-stream data. we can analyse a large amount of data very fast for a cheaper cost than traditional databases.

### Exam tips
* storing a large amount of time stream data for analysis: think timestream


## Exam Tips Recap
### Exam tips1 (RDS overview)
* RDS database types (SQL, MySQL, postgreSQL, MariaDB, Oracle, aurora)
* RDS is for OLTP workloads (simple transactions)
* Not suitable for OLAP workloads (large analysis)

### Exam tips2 (Multi AZ, read replicas)
Key facts for read replicas
* scaling read performance: primarily used for scaling, no disaster recovery
* requires automatic backup: automatic backups must be enabled in order to deploy a read replica
* multiple read replicas are supported: allows up to 5 read replicas to each DB

Milti-AZ vs Read Replica
Multi-AZ:
* an exact copy of your production database in another availability zone
* used for disaster recovery
* in the event of a failure, RDS will automatically fail over to the standby instance

Read Replica:
* read only copy of your primary database in the same AZ, cross-AZ, or cross-region
* used to increase or scale read peformance
* great for read-heavy workloads and takes the load off your primary database for read-only workloads

### Exam tips3 (aurora)
* 2 copies of our data are contained in each AZ with a minimum of 3 AZs (total of 6 copies of our data)
* You can share Aurora snapshots with other AWS accounts
* 3 types of aurora replicas (aurora replicas with automated failover, mySQL replicas, postgreSQL)
* aurora has automated backups turned on by default, we can take snapshots and share it
* we can use serverless for simple cost-effective option for infrequent, intermittent or unpredictable workloads

### Exam tips4 (DynamoDB)
4 facts about dynamoDB
* stored on SSD storage
* spread across 3 geographical distinct data centers
* eventually consistent reads (default)
* strongly consistent reads

eventually consistent reads vs strongly consistent reads
* eventually: consistency across all copies of data is reached within 1 second (best read performance)
* strongly: consistent read that returns a result that reflects all writes that recieved a successful response prior to the read

### Exam tips5 (when to use dynamoDB transactions)
Transactions
* Multiple "all or nothing" operations
* financial transactions
* fulfilling orders
* 3 options for reads: eventual consistency, strong consistency, and transactional
* 2 options for writes: standard and transactional
* up to 25 items or 4MB data per transaction

Scenrio questions
* ACID requirments, think dynamoDB transactions
* dynamoDB provide deveopler with ACID (atomic, consistent, isolated, durable) across 1 or more tables within a single AWS account and region
* all or nothing transaction

### DynamoDB on demand backup and restore
on demand backup and restore
* full backups at any time
* zero impact on table performance or avaliablity
* consistent within seconds and retained until deleted
* operates within same region as the source table

### DynamoDB point in time recovery
point in time recovery
* protect against accidenty writes or delete
* restore to any point in the last 35 days
* incremental backups
* not enabled by default
* latest restorable: 5 minutes in the past

### Exam tips6 (DynamoDB streams and global tables)
streams
* time ordered sequence of item-level changes in a table
* stored for 24 hours
* inserts, updates and delete
* combine with lambda functions for functionality like stored procedures

global tables
* managed multi-master, multi-region replication
* globally distributed apps
* based on dynamoDB streams
* multi-region redundancy for disaster recovery or high availability
* no application rewrites
* replication latency is under 1 second

### Exam tips7 (migrating mongoDB with documentDB)
moving mongoDB database to aws
* mongo on prem - AWS database migration service - Amazon DoucmentDB

### Exam tips8 (cassandra and AWS keyspaces)
* migrating a big data cassandra cluster to AWS: think keyspaces

### Exam tips9 (neptune)
* neptune is a distractor question, unless they specifically ask about graph database

### Exam tips10 (QLDB)
* QLDB is a distractor question, unless they specifically ask about immutable database

### Exam tips11 (timestream)
* storing a large amount of time stream data for analysis: think timestream
