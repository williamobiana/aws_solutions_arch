## IAM
4 steps to Secure Root Account:
* Enable MFA on root account
* Create an admin group for your admins and assign the approriate permissions to the group
* Create user accounts for your admins
* Add the admin users to the admin group

We assign permissions using IAM policy documents consisting of JSON\
Example of a policy document:
````
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
````

* IAM is universal and not tied to a region
* The root account is the account used to sign up for AWS, secure it with MFA as soon as possible and do not use it for day to day
* New users have no permission by default when first created
* Access key ID and secret access key ARE NOT the same as username and password, they are used as programatic access to AWS CLI
* You can only view it once, if you lose it, you have to regenerate it. (so download and save in a secure location)
* Always setup password rotations, you can create and customize your own password rotations policies.
* IAM Federation, you can use your existing user account credentials when you combine it with AWS (eg: Microsoft Active Directory with AWS, Jumpcloud with AWS)
* Identity federation Uses SAML standard, which is Active directory


## S3 Exam Tips
Overview
* S3 is object based storage allowing to upload files
* It is NOT SUITABLE TO INSTALL an operating system OR RUN a database
* Files can be from 0 Bytes to 5 Terrabytes
* The total volume of data and number of object you can store is unlimited
* Files are stored in buckets
* S3 is a universal namespace
* S3 is avaliable on global regions
* The bucket has a unique name URL (https://``bucket_name``.s3.region.amazonaws.com/``key_name``)
* A succesful CLI or API upload will show a HTTP 200 status code

Object tips to remember:
    Key (name of object)
    Value (data of the object)
    VersionID (used when versioning is enabled)
    Metadata (information about the data, eg: content type, last modified, etc)

Secure the S3 bucket
* Buckets are PRIVATE BY DEFAULT including all objects inside it, we have to allow public access on both the bucket and object to make the bucket public
* We can make indiviual objects public using OBJECT_ACL
* We can make entire buckets public using BUCKET_POLICIES
* Successful upload receive HTTP 200 code

Hosting a static website
* Make entire bucket public using bucket policies
* We can use S3 to host static content ONLY
* S3 scales automatically with demand

Versioning
* All versions of the object are stored in s3 (this includes all writes (uploads) even if you delete the object)
* Its a great backup tool
* Once enabled, it cannot be disabled, only suspended
* It intergrates with lifecycle rules
* it supports multifactor authentication

Storage class
* recap different types of storage classes and use case
* recap different types of glacier options and use case

Lifecycle management
* Lifecycle rules automates the moving of objects between the different storage tiers
* it can be used in conjuction with versioning
* it can be applied to current or previous versions of an object

Object lock and glacier vault
* use s3 object lock to store object using ``WORM`` model (Write Once, Read Many)
* object lock can be on individual objects or applied across the bucket as a whole
* object lock comes in 2 modes: governance and compliance
* compliance mode: protected object cannot be overwritten by any use
* governance mode: protected object cannot be overwritten by most users, only user with permission can modify
* s3 glacier vault lock allows to deploy and enforce compliance of the s3 glacier vaults with a vault lock policy containg ``WORM``
* we can specify controls like ``WORM`` in a vault lock policy and lock the policy from future edits, once locked it cant be changed

Encryption
* encryption in transit (ssl/tls, https)
* encryption at rest (server side enryption)
    SSE-S3 (S3 manages keys, AES 256-bit encyption)
    SSE-KMS (AWS key management service managed keys)
    SSE-C (customer provided keys)
* encryption at rest (client side encryption: local encryption of files and upload to s3)
* To enforce encryption with bucket policy, we can create a bucket policy that denys any PUT request that doesnt include the (x-amz-server-side-encryption) parameter.

Optimizing
* s3 prefix is the /folder1/subfolder1 in the s3 bucketname, it doesnt include the object name
* the more prefix you use with the s3, the better performance you will get
* you can achieve a high number of requests 3500 PUT/COPY/POST/DELETE, 5500 GET/HEAD requests per second per prefix
* you can get better performance by spreading your reads across diffrent prefixes.
* if you are using SSE-KMS to encrypt objects, we should remember it has a KMS limit
* uploading/downloading will count tomwards the KMS quota
* we cannot currently request a quota increase for KMS
* region-specific, however it is either 5500, 10000, or 30000 requests per second
* use multi-parts uploads to increase performance when uploading files to s3
* recommended for files over 100MB
* use s3 byte range fetches to increase perfomance when downloading files to s3

Backing up data
* you can replicate objects from one bucket to another
* objects in an existing bucket are not replicated automatically
* delete markers are not replicated by default


## EC2 Exam Tips 
Overview)
* ec2 is like a VM, only hosted in AWS instead of my own datacenter.
* it allows us select the capacity we need right now
* grow or shrink when we need, there is no wasted capacity
* we pay for what we use
* it takes minutes to provision, instead of months
 
pricing models are:
* on demand: pay by the second/hour depending on the type of instance you run. Great for flexibilty
* reserved: reserve capacity for 1 to 3 years. up to 72% discount on the hourly charge. Great for fixed requirements
* spot: purchase capacity at a discount of up to 90%. price flutuate with supply and demand. Great for apps with flexible start-end time
* dedicated: a pysical ec2 server dedicated for your use. the most expensive option. Great for server-bound licences or compliance requirments

Commandline
* Always give least privilage required to do the job
* Use groups (create groups with required policy permission and add users)
* Secret access key (password): if you lose it, you have to regenerate it again and run aws configure
* dont share keypairs (each dev should have thier own keypair)
* cli can be installed on local or used on an ec2

Roles
* Roles are preffered from a security persperctive
* Roles allow you access without the use of access keys and secret access keys
* Policies control role permissions
* We can update policies attached to roles and it takes immediate effect
* We can attach and detach a role to a running ec2 without stoping or terminating it

Security groups and bootstrap scripts
* changes to security group takes effect immediately
* you can have any number of ec2 within a security group
* you can have multiple security group attached to ec2
* all inbound traffic is blocked by default
* all outbound traffic is allowed
* a bootstrap script is a script that runs when the ec2 first runs, it passes user data to the ec2 to install apps, dependencies, updates and more

Metadata and userdata
* userdata is bootstrap scripts
* metadata is data about our ec2
* we can use bootstrap scripts (userdata) to access metadata

Networking in ec2
* ENI (elastic network interface): basic networking, seperate management network from production network at a low cost, use multiple ENI for each network
* EN (enhanced networking): when we need speed btw 10GBps - 100GBps for reliable high throughput
* EFA (elastic fabric adapter): when we need high performance computing and ML apps, when we need to do an OS-bypass.

Placement groups aka racks
* cluster: low network latency, high network throughput
* spread: indiviual critical ec2 instances
* partition: multiple ec2 instances, HDFS, HBase and Cassandra 
* cluster placement groups cant span multiple AZs, spread and partition can
* only certain ec2 types can be launched in a placement group (compute optimized, GPU, memory optimed, storage optimized)
* AWS recommends homogenous(same type) instance within cluster placement group
* we cant merge placement groups
* we can move or remove an exsisting instance into a placement group via CLI and SDK, we CANNOT do this via console.
* the instance must be in a stopped state before it can be moved

Licencing for dedicated hosts
* special licensing requirment are for dedicated hosts
* dedicated hosts are physical server with ec2 dedicated to us
* dedicated hosts allows us to use our exisiting licenses per-socket, per-core and per-VM. including windows server, microsoft sql server, SUSE linux enterprise server.

Spot instance and Spot fleets
* spot instance save up to 90% of the cost of on-demand instances
* useful for computing where we dont need persistent storage
* we can stop spot instance from terminating using a spot block
* spot fleet is a collection of spot instances and optionally on-demand instances

vCenter & VMware
* we can deploy vCenter on AWS using VMware
* perfect solution for extenting private cloud to AWS

Outposts
* extending AWS to our datacenter is called outpost
* outpost racks is for large deployments
* outpost server is for small deployments

## EBS_EFS Exam Tips
EBS Overview
EBS volume types: general purpose SSD (gp2)
* suitable for boot disk and general application
* up to 16000 IOPS per volume
* up to 99.9% durabililty

EBS volume types: general purpose SSD (gp3)
* suitable for high performance apps
* predictable 3000 IOPS baseline performance and 125 mib/s regardless of volume type
* up to 99.9% durabilty

Provisioned IOPS SSD (io1)
* suitable for OLTP (online transaction processing) and latenct sensitive apps
* high performance option (most expensive)
* up to 64000 IOPS per volume 
* 50 IOPS per gib
* up to 99.9% durability

Provisioned IOPS SSD (io2)
* latest generation
* suitable for OLTP (online transaction processing) and latenct sensitive apps
* same price as io1
* up to 64000 IOPS per volume 
* 500 IOPS per gib
* 99.999% durability

Throughput Optimized HDD (st1)
* max throughput of 500 mb/s per volume
* for big data, warehouse, ETL (extract, transform & load), and log processing
* it cannot be a boot volume
* up to 99.9% durabilty

Cold HDD (sc1)
* lowest cost option
* max throughput of 250 mb/s per volume
* good choice for less frequently accessed data (colder data)
* cannot be a boot volume
* up to 99.9% durability

Volumes and snapshots
* volumes exist on ebs, snapshots exist on s3
* snapshots are current volume state and are incremental
* first snap takes time, stop ec2 and detach volume before taking snapshot
* we can share snaps withn same region, for multi-region copy snap manually to region
* we can resize and change volume type on demand

Protecting ebs with encryption
* data at rest is encrypted inside the volume
* all data in transit between the ec2 and volume is encrypted
* all snaps are encrypted
* all volume created from snap is encrypted

4 steps to encrypt an unencypted volume
* create a snap of unencypted root device volume
* create a copy of snap and select the encrypt option
* create an AMI from the encrypted snap
* use AMI to launch new ec2

Hibernation
* ec2 hibernation preserves RAM on EBS
* much faster to boot up (becasue we do not reload OS)
* ec2 RAM must be less than 150gb
* ec2 types include (C types(3,4,5), M types(3,4,5) and R types(3,4,5))
* avalaible for windows, amazon linux2, and ubuntu
* ec2 cant be hibernated more than 60 days
* avalable for on-demand and reserved ec2

EFS
* supports nfsv4 protocol
* pay per use
* can be scaled to petabytes
* can support thousands of concurrent connections (many ec2 can be attached to it)
* data is stored accross multiple AZs with a region
* read after write consistency

FXs
* EFS: distributed, highly resilient storage for linux instances, and linux based apps
* FSx for windows: centralized storage for windows based apps (sharepoint, microsoft SQL server, other native microsoft apps)
* FSx for lustre: high speed, high capacity distributed storage for high performance computing, financial modelling, etc. (lustre can store data on s3)

Storage options
* s3: serveless object storage
* glacier: archive
* efs: centralized storage across multiple AZs
* FSx for lustre: for high computing
* ebs: persistent storage for ec2
* instance store: emphemeral storage for ec2
* FSx for windows: for microsoft windows

Ami
* instance store volumes are sometimes called ephemeral storage(data will not be saved if instance is stopped)
* instance store volumes cannot be stopped. if the underlying host fails, we lose all data
* ebs backed instances can be stopped. we will not lose the data on the instance if it is stopped
* we can reboot both EBS and instance store volume, and we will not lose our data
* by default, root volumes are deleted on termination. for EBS, we can tell AWS to keep our volume on termination

Backup
* consolidation: backup allows to consolidate our data accross multiple services (ec2, ebs, Fxs, etc)
* organization: we can use AWS organization + AWS backup to backup diffrent AWS services across multiple AWS accounts

Backup benefits:
* central management: single central backup for the organization across multiple services and accounts
* automation:  backup schedules, lifecycle policies
* improved compliance: enfored backup polices, encryption at rest and in-transit, alignment to regulatory compliance, easy auditing


## Database Exam Tips 
RDS overview
* RDS database types (SQL, MySQL, postgreSQL, MariaDB, Oracle, aurora)
* RDS is for OLTP workloads (simple transactions)
* Not suitable for OLAP workloads (large analysis)

Multi AZ & Read replicas
Key facts for read replicas:
* scaling read performance: primarily used for scaling, no disaster recovery
* requires automatic backup: automatic backups must be enabled in order to deploy a read replica
* multiple read replicas are supported: allows up to 5 read replicas to each DB

Multi-AZ:
* an exact copy of your production database in another availability zone
* used for disaster recovery
* in the event of a failure, RDS will automatically fail over to the standby instance

Read Replica:
* read only copy of your primary database in the same AZ, cross-AZ, or cross-region
* used to increase or scale read peformance
* great for read-heavy workloads and takes the load off your primary database for read-only workloads

Aurora
* 2 copies of our data are contained in each AZ with a minimum of 3 AZs (total of 6 copies of our data)
* You can share Aurora snapshots with other AWS accounts
* 3 types of aurora replicas (aurora replicas with automated failover, mySQL replicas, postgreSQL)
* aurora has automated backups turned on by default, we can take snapshots and share it
* we can use serverless for simple cost-effective option for infrequent, intermittent or unpredictable workloads

DynamoDB
4 facts about dynamoDB:
* stored on SSD storage
* spread across 3 geographical distinct data centers
* eventually consistent reads (default)
* strongly consistent reads

eventually consistent reads vs strongly consistent reads
* eventually: consistency across all copies of data is reached within 1 second (best read performance)
* strongly: consistent read that returns a result that reflects all writes that recieved a successful response prior to the read

When to use dynamoDB transactions
* Multiple "all or nothing" operations
* financial transactions
* fulfilling orders
* 3 options for reads: eventual consistency, strong consistency, and transactional
* 2 options for writes: standard and transactional
* up to 25 items or 4MB data per transaction

Scenrio questions for dynamoDB
* ACID requirments, think dynamoDB transactions
* dynamoDB provide deveopler with ACID (atomic, consistent, isolated, durable) across 1 or more tables within a single AWS account and region
* all or nothing transaction

DynamoDB on demand backup and restore
* full backups at any time
* zero impact on table performance or avaliablity
* consistent within seconds and retained until deleted
* operates within same region as the source table

DynamoDB point in time recovery
* protect against accidenty writes or delete
* restore to any point in the last 35 days
* incremental backups
* not enabled by default
* latest restorable: 5 minutes in the past

DynamoDB streams and global tables
streams:
* time ordered sequence of item-level changes in a table
* stored for 24 hours
* inserts, updates and delete
* combine with lambda functions for functionality like stored procedures

global tables:
* managed multi-master, multi-region replication
* globally distributed apps
* based on dynamoDB streams
* multi-region redundancy for disaster recovery or high availability
* no application rewrites
* replication latency is under 1 second

Migrating mongoDB with documentDB
moving mongoDB database to aws:
* mongo on prem - AWS database migration service - Amazon DoucmentDB

Cassandra and AWS keyspaces
* migrating a big data cassandra cluster to AWS: think keyspaces

Neptune
* neptune is a distractor question, unless they specifically ask about graph database

QLDB
* QLDB is a distractor question, unless they specifically ask about immutable database

Timestream
* storing a large amount of time stream data for analysis: think timestream


## VPC Exam Tips
VPC overview
* a vpc is a logical datacenter in AWS
* consists of igw/vgw, route tables, nACLs, subnets, security groups
* 1 subnet is always in 1 AZ

Nat gateways
* redundant inside the AZ
* starts at 5 Gbps and scales currently to 45 Gbps
* no need to patch 
* not associated with security groups
* they are automatically assigned a public Ip

High Avaliability with NAT gateways
* if we have resources in multiple AZ and they share a NAT gateway, in the event the NAT gateway's avaliablity zone is down, resources in the other AZs lose internet access
* to create an AZ independent architecture, create a NAT gateway in each AZ and configure routing to ensure resources use the NAT gateway in the same AZ

Security group
* Security groups are STATEFUL
* if you open a port in inbound rules, that same port will be open outbound rules (even if you explicitly block it in the outbound rules)

Network ACL overview
* default nACL: by default, a VPC comes with a network ACL and it allows all inbound and outbound traffic
* custom nACL: custom nACL by default denies all inbound and outbound traffic until we add rules
* subnet associations: each subnet in VPC must be associated with network ACL, if we dont explicitly associate a subnet with network ACL, the subnet is automatically associated with the default network ACL.
* blocking IP address: we can block ip addresses using network ACL NOT security groups

Network ACL tips
* we can associate network ACL with multiple subnets, but subnets can associate with only 1 Network ACL, when we associate a subnet with a nACL, the previous association is removed
* nACL contain a numbered list of rules that are evaluated in order, starting with the lowest number rule
* nACL have seperate inbound and outbound rules and each rule can either allow or deny traffic
* network ACLs are STATELESS (rules for inbound traffic can be influenced by rules for outbound traffic and vice versa)

Direct connect
* direct connect directly connects our datacenter to AWS
* useful for high-throughput workloads
* helpful when we need a stable and reliable secure connection

Endpoints
* use case: when we want to connect to AWS services without leaving the amazon internal network
* 2 types of vpc endopint: interface and gateway
* gateway endpoints: supports s3 and dynamoDB

VPC peering
* allows us to connect 1 vpc with another via a direct network route using private IP addresses
* instances behave as if they were on the same private network
* we can peer vpcs with other aws accounts as well as with other vpcs in the same account
* peering is in a star config (1 central vpc peering with other vpcs). No transitive peering
* you can peer between regions

Privatelink
* peering vpc to many customer vpcs: think privatelink
* doesnt require vpc peering, no route tables, no nat gateways, no internet gateways etc
* requires a network load balancer on the service vpc and eni(elastic network interface) on the customer vpc

Transit gateway
* we can use route tables to limit how vpc communicate with each other
* works with direct connection as well as vpn connections
* supports IP multicast (not supported by any other AWS services)

VPN hub
* if we have multiple websites with its own VPN connection, we can use AWS vpn Cloudhub to connect those sites together using a hub and spoke model
* low cost and easy to manage
* operates over the public internet, but all traffic between customer gateway and AWS VPN cloudhub is encrypted

Mobile edge computing with wavelenght
* scenrio questions about mobile computing: think AWS wavelenght



## Route 53 Exam Tips
alias record vs CNAME
* alias record (unique to AWS) can map DNS name to AWS resource and sub-domains
* CNAME cannot (it can only translate DNS for diffrent devices)
* if given a choice between alias and CNAME in a question, Alias is more correct
* common DNS record types (SOA, CNAME, NS, A)

Common dns record types: 
SOA
NS Records
A Records
CNAME Records

SOA (start of authority)
the SOA record stores information about:
* the name of the server that supplied the data for the zone. (name of server is gotten from .com record and stored in NS record)
* the administrator of the zone
* the current version of the data file
* the default number of second for the time-to-live(TTL) file on resource records

NS record stands for name server records 
* NS records are used by top level domain servers to direct traffic to the content DNS server that contains the authoritative DNS records
* (user) - (.com) - (NS record) - (SOA)

What is an A Record
* An A record (address record) is the fundermental type of DNS record.
* The A record is used by a computer to translate the name of the domain to an IP address.
* it is the most common type of DNS record (http://websitename.com will point to http://1.2.3.4)

What is TTL (time-to-live aka how long it takes to cache your DNS records on the cloud)
* the length that the dns record is cached on either the server or user local pc = TTL(in seconds)
* the lower the TTL, the faster the change of DNS records on the internet 

What is CNAME?
* a CNAME (canonical name) can be used to resolve one domain name to another (translating one webadress to another).
* example: using http://m.websitename.com or http://mobile.websitename.com for mobile version of website

7 Routing policies avaliable with Route53
* simple routing
* weighted routing
* latency based routing
* failover routing
* geolocation routing
* geoproximity routing (Traffic Flow Only)
* Multivalue answer routing

Registering a Domain name
* we can buy domain name from AWS
* it can take up to 3 days

Health checks
* we can set health checks on individual record set (incase 1 ec2 goes down, it redirect the traffic to the other ec2)
* if a record fails a health check, it will be removed from route53 until it passes the health check
* we can set SNS notification to alert us about failed health checks

Using simple routing policy
* we can only have 1 record with multiple ip addresses
* if we specify multiple values in a record, Route53 returns all value to user in a random order.

Using weighted routing policy
it allows us to split our traffic based on diffrent weights assigned
* spliting percentages of traffic to diffrent ec2: think weight routing policy

Faiover routing policy
* failover is used when we want to create active/passive setup
* if primary ec2 fails health check, traffic is rerouted to secondary ec2

Geolocation routing policy
* lets us choose which ec2 the traffic will be sent based on the location of the users (DNS queries)

use-cases:
queries from a region go to the closest ec2 in that region (the app running in the ec2 might be customized for language, currency, etc)

Geoproximity routing policy
we can use route53 traffic flow to build a routing system that uses a combo of
* geolocation
* latency (delay)
* avaliablity 
to route traffic from user to cloud/on-prem endpoints
we can build it from scratch or edit a template

Geoproximity routing (traffic flow only)
* lets route53 route traffic based on location of users and resources
* we can optionally route more/less traffic based on bias (the bias expands or shrinks the region size for traffic to resource)
* to use geoproximity routing, we must use route53 traffic flow

Latency routing policy
allow routing of traffic based on lowest latency (delay) for the end user (which region will give them the fastest time)
* to use latency routing, we create a latency resource record set for the ec2 (or elb) in each region that hosts our website
* when Route53 recieves a query for the site, it selects the latency resource record set for the region that gives the user the lowest latency.

Multivalue answer routing policy
* lets us configure route53 to return mutiple values such as ip address for servers, in response to DNS queries
* we can specify multiple values for any record, but also run health check of each resource, so route53 returns only values for healthy resources


## Elastic Load Balancer Exam Tips
3 types of load balancers
* Application load balancer: best for http/https traffic, operate on (layer 7), application aware (intelligent load balancer)
* Network load balancer: performance load balancer (layer 4)
* Classic load balancer: classic/test/dev load balancer (layer4/7)

Health checks
* we can use health checks to route our traffic to instances or targets that are healthy

Application Load_balancer (ALB)
* Listeners: a listerner checks for connection requests using configured protocols and ports
* Rules: Determine how the LB routes requests to registered target, each rule consists of: a priority, one or more actions, one or more conditions
* Target groups: each target group routes requests to 1 or more registered targets, using specified protocol and port number

Limitations
* only supports http and https
* for https, we must have SSL/TLS certificate on load balancer
* LB uses certificate to terminate frontend connection and decrypt request from user before sending them to target

Network load_balancer (NLB)
* network loadbalancer operates on layer 4 (OSI: transport layer)
* use when extreme performance is required.
* use when we need protocol not supported by ALB
* network loadbalancer can decrypt traffic, we will need to install the certificate on the load balancer

Classic load_balancer (classic LB)
* 504 error means the gateway has timed out. (this means the app is not responding within the idle timeout period)
* Troubleshoot the app (check if its webserver or db server)
* if we need the ipv4 address of the end user, (look for the x-forwared-for header)

Sticky session
* sticky sessions enable users to stick to same ec2
* useful if we are storing data locally on that ec2
* if we remove ec2 from pool, the LB will still direct traffic to it, unless we disable sticky sessions

Sticky session for ALB
we can enable sticky sessions on ALB, but traffic will be sent to the target group (ec2 can be terminated and replaced as long as it is in the target group)

Deregistration delay/connection draining
* enable deregistration delay: keep existing connections open if ec2 becomes unhealthy
* disable deregistration delay: if we want LB to immediately close connection to ec2 that are deregistring or unhealthy


## High Avaliability Exam Tips
* is it highly avalaible?
* should it horizontal or vertial: horizontal is most approrate
* is it cost effective
* would switching database fix the problem

What to know about autoscaling
* Autoscaling is only for ec2: no other service can be scaled using auto scaling. other services might have a built-in option, but they arent included in ASG.
* Get ahead of the workload: whenever possible, favour solutions that are predictive rather than reactive.
* Bake AMI to reduce build times: we can avoid long provisioning time by putting everything into an AMI. This is better than using user data whenever possible.
* Spread out: Spread out ASG over Multiple-AZs (at least 2)
* Steady state groups: allow us to create a suituation where failure of a legacy app that cant be scaled, can automatically recover from failure.
* ELBs are essential: ensure health checks are enabled, else instances wont be terminated and replaced when they fail health checks
* notifications: SNS allows know when a scaling occurs

Scaling databases
* RDS has the most database scaling options
* Horizontal scaling is usually preferred over vertical
* Read replicas are your friend
* DynamoDB scaling comes down to access patterns (predictable workload: Pick provisioned capacity, sporadic(unpredictable): Pick on-demand)


## Decoupling Workflow Exam tips 
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



## Serverless Exam tips 
4 question to ask
* is the app a right fit for container or should it be hosted on ec2
* do we need the servers that run the app or should we be serverless?
* is it AWS specific?
* how long does the code need to run?

Lambda 
* when asked how we add features to AWS: Lambda is the answer
* lambda loves roles: always attach a role to the lambda function
* Be familar with lambda Triggers: starts lambda functions (s3, eventbridge, etc)
* Limitations: 10Gb is max ram, 15 mins is max time
* Any API call can be made

Containers and Images
* open-source: Think Kubernetes
* if scenerios talks about running on-prem and AWS: think EKS or EKS anywhere
* Fargate cannot work alone: we must use ECS or EKS to use fargate 
* Containers offer flexibility: they can handle any workload, it is preferred to use containers than ec2
* know the basics: dockerfile - build image - upload to repo - run on host
* AWS managed image registry for image storage or image vunerebility scanning: think Amazon ECR

Aurora serverless
* on-demand or auto scaling database: think aurora serverless databases
* variable traffic or workloads, designing new apps with unknow workload, traffic spike to database: think aurora serverless
* capcity planning for new apps with database, dev environment: likely aurora serverless

X-ray
* AWS X-ray: Used to gain app insights using requests and responses of services and functions at diffrent points in an application flow
* Traces and downstreams response times: if these terms are brought up in the question, think x-ray
* AWS lambda or API gateway insights: X-ray intergrates natively with these services and can easily help you gain deeper insights and understanding of your workload requests and responses.

Appsyn
* managed grapgQL: any scenrio with managed graphQL interfaces for frontend apps/dev think AWS AppSync


## Big Data Exam tips
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


## Security Exam tips
3 tips for DDoS
* DDoS attempts to make app unavaliable to users
* common DDoS attacks are layer 4 such as SYN-floods or NTP amplification attacks
* common layer 7 attacks include floods of GET/POST requests

Cloudtrail
* after-the-fact incident investigation
* near real-time intrusion detection
* industry and regulatory compliance
* Cloudtrail is CCTV for you AWS account
* it logs all API calls made to the account and stored in S3

Shield (Layer 4)
* shield protects layer 3 and layer 4 only
* protects agaisnt DDoS attacks
* cost 3000usd/month and gives 24/7 DDoS response team.

WAF (Layer 7)
3 different behaviours
* Allow all requests excepts the ones you specify
* Block all requests except the ones you specify
* Count the requests that match the properties you specify

WAF tips
* WAF operates at layer 7
* scenerio on how to block layer 7 DDoS attacks, sql-injections, cross-site mapping: think WAF
* WAF can block access to specific contries and IP addresses
* Guarding your network with GuardDuty

Firewall manager
* if scenrio talks about multiple AWS accounts that need to be secure centrally: think AWS firewall manager

GuardDuty
* GuardDuty is a threat detection service that uses ML to continously monitor for malicious behaviour
* uses AI to learn what normal behaviour look like and alert of abnormal behaviour
* updates a DB of known malicious domains 
* monitors cloudtrail logs, VPC flow logs and DNS logs
* findings appear in the guardDuty dashboard (cloudwatch can be used to trigger a lambda funtion to address a threat)

Macie
* automated way of disovering personal identification
* uses AI to analyze data in s3, and help identify PII, PHI and financial data
* great for HIPAA and GDPR compliance, and preventing indentify theft
* alerts sent to eventbridge can be intergrated with security incident and event management system
* Automate remediation actions with service like step functions

Inspector
* inspector is used to perform vulnerability scans on ec2 and VPC
* ec2: is called host assesment
* vpc: is called network assesment
* run 1nce or weekly

KMS and CloudHSM
* KMS is a managed service that makes it easy to create and control the encryption keys used to encrypt your data
* you start using the service by requesting the creation of CMK (customer managed keys), you control the lifecycle of the CMK as well as who can use or manage it.

3 ways to generate CMK (customer managed keys)
* AWS creates CMK in KSM (we can perform key rotations every year)
* import from my key managment infrastructure
* generate and use in a CloudHSM

3 ways to conrol permission
* use key polices
* use IAM policies + key polices
* use grants + key polices

KMS vs CloudHSM
KMS
* shared tenancy of underlying hardware
* automatic key rotation
* automatic key generation

CloudHSM
* dedicated HSM
* full control of hardware, users, groups etc
* no automatic key rotation

Secrets manager
* secrets manager can be used to securely store your application secrets: database credentials, API keys, SSH keys, passwords etc.
* application use the secrets manager API
* rotating credentials is super easy, but be careful
* when enabled, secrets manager will rotate credentials immediately
* make sure all applications instances are configured to use secreats manager before enabling credential rotation

Parameter store
* If scenrio ask to choose between parameter store and secrets manager. choose parameter store to minimize cost
* if we need more than 10000 parameters and key rotation, choose secrets manager

Presigned URLs
* all objects in s3 are private by default.
* you can share object with others by creating a presigned url using thier own security credentials.

Advanced IAM polices
* not explicitly allowed == implicitly denied
* explicitly denied > everything else
* only attached polices have an effect
* multiple policies can join all policies
* AWS polices vs customer managed

Certificate Manager
* create, manage and deploy public and private SSL certificate for your aws environment
* supported services to intergrate with: ELB, Cloudfront, API gateway

benefits
* cost: it is free, but you still pay for resources
* automated renewals and updated deployment
* easy to set up: remove all manual process, with creating csr, generating key-pair

Audit Manager
* automated service that continously audit AWS usage to keep you compliant with industry standards and regulations (GDPR, HIPAA)

Artifarts
* asking about audits and download reports: think artifacts
* this is usually a disractor question

Cognito
* user pools: list of users that have sign-up and sign-in options for application
* identity pools: list of users you give access to other AWS services
* you can use userpool and identity pool together or seperately

cognito flow
* dropbox - userpool (recieves token) - dropbox
* dropbox - identity pool (exchange token for access credentials) - dropbox
* dropbox - AWS services (using credentials)

Detective
* Detects root cause
* dont confuse with inspector
* it is usually a distractor

AWS Network firewall
* filtering network traffic before it reached IGW, or intrusion prevention system: think network firewall

Security hub
* single place to view all security alerts from multiple security services and multiple accounts: AWS security hub


## Monitoring Exam tips
4 questions to ask
* What is the best tool to monitor with (AWS or 3rd party)
* Is that metric avaliable by default (if not, how can i create a custom metric?)
* Where can i find the logs? (store as applications logs in log stream/ log group? or centralize in s3 like cloudtrail logs)
* do i need to adjust my alarm treshold? 

Cloudwatch
* Cloudwatch is used for monitoring on anything alarm related
* standard vs detailed: stardard reporting interval is every 5 minutes, for detailed metrics as low as 1 minute would have a cost

Application monitoring with CloudWatch Logs
* if we need to process the logs, then it goes to cloudwatch
* we collect metrics and analyzing the data from common services like (EC2, RDS, Lambda, CloudTrail, on-prem)
* using SQL queries, is done with cloudwatch logs insights
* if we need to process the logs in real time, then we use Kinesis

Visualizing data and monitoring containers at scale
* Grafana is best tool for container metric visualization, IoT, Troubleshooting
* Monitoring at scale? think Prometheus, it works with clusters (EKS or on-prem kubernetes)
* They are both managed services. AWS handles high avalaibilty and auto scaling


## Automation Exam tips
4 questions to ask
* can you automate? (cloudformation, Elastic beanstalk, systemsmanager)
* what kind of automation is gonna work in this particular scenerio
* is the automation repeatable
* how would this work cross-region or cross account (by creating templates for secrets)

CloudFormation
* know the layout of the template
* Immutable infrastructure is prefeered (it can easily be disarded)
* Mapping and parameter store (to store templates and secerts with IDs)

Elastic Beanstalk and session manager
* it is a simple 1 stop solution that Automates the deployment and management of all the services needed to run an application
* Automation documents are the primary method used in scenerios asking to configure inside the ec2
* we can use system manager features like Automation documents, session manager, parameter store (store secrets)


## Caching Exam tips 
4 questions to ask
* can it be cached? (is there a situation in the scenerio to cache the data, speed up performance)
* what kind of cache do i need? (caching a database or customer facing)
* how does the content in cache get updated 
* does the cache add benefits from speed (security, IP cacheing)

CloudFront
* CloudFront is the only option to add HTTPS to a static website being hosted in an S3 bucket
* Always favor answers that include caching. 
* Global accelerator is solution for IP caching

Database caches
* in-memory database has 2 potential solutons, DynamoDB and redis
* Redis has more features than memcache. Redis can be used as a cache and Database, memcache is just a cache
* Backups are not supoorted on any solution except Redis


## Governance Exam tips
4 questions to ask
* can it be centralized (RAM or control tower)
* how do we standardize (control tower using guardrails for infrastructe deployments or account deployments)
* how do we enforce standard that we want (SCP, AWS config rules, GuardRails & Control Tower, cloudtrail for auditing history)
* are users internal or external (how do we want to grant them access to resources and account)

Organizations
* Service control policies (SCPs) have the ultimate say as to whether an API call goes through.
* They are the only way to restrict what the root account can do
* centralized logs are always the right answer. CloudTrail offers support to log everything into a single AWS account
* Isolate workloads into separate accounts is a great way to add more layers of security and controls. Avoid answers that lump everything together

AWS config
* Standardization: Anytime a 'rule' needs to be set up for an account, think about using Config to check for compliance
* Automate the response: Config offers the ability to automatically remediate problems using automation documents
* Know what changed: Configs is the one-stop shop to see what changed. it will provide you with a history of all your architecture.

Authentication
* User management: requires the right tool. Make sure you are using AWS SSO for internal user management and Cognito for external
* Active Directory: is a common topic that should immediately make you think directory service. if its a lift and shift, pick the managed microsoft AD. if AD is staying on-premises, select AD connector.
* cross-account role access: is always a better solution than creating unnessasry IAM credentials

Cost management
* Tracking cost: combination of tags, Cost Explorer and AWS budgets
* Get ahead of problems by creating proactive alerts. when users get to the 80% threshold, tell someone via SNS
* Automate the responses
* Detailed reports and exploring cost: involves AWS cost and Usage Reports
* Use Compute Optimizer

Trusted Advisor
* Its free (butr you need business or Enterprise to get more capability)
* There are limits: (Its strictly an auditing tool)
* Automate the response. Use EventBridge to kick off Lambda function to solve the problem for you.

Accounts and licences
* Aws control tower can be used to implement compliance and account governance within multi-account environment using automated account setups.
* Leverage preventive or detective guardrails within Control Tower. They are implemented via SCPs and AWS config
* AWS license manager provides a simplified way to manage licences from supported vendors to prevent licence abuse and overcharges. it works in AWS and on-premise.

Infrastructure and Deployments
* AWS service catalogs allow end users to provision pre-approved products and services via shared catalogs portofolios. These are written in Cloudformation.
* AWS Proton can empower developers to automate the provisioning of thier entire application stack for container-based or serverless architectures.
 
The Well Architected tool
* Use the well architected tool to document architectural decisions ant thier measuremments against well established industry best practises.

AWS health
* AWS health is a dashboard and service meant to provide notifications of both public and account-specific events within AWS
* Questions about service alert or notifications of ec2 hardware maintenance reboots will leverage AWS health in some manner


## Migration Exam tips
4 questions to ask
* where are we going? (moving from on-prem to AWS, AWS - onprem, btw cloud vendors, 3rd party saas)
* how do we get there (snowball, migration with DMS)
* is it all at once (in 1 big move or in increments)
* is it partial migration (s3 upload, multi-part, server for service migration)

Snowfamily
* snowball: based on how much data you are moving (perfect for TB of data)
* it is best used with slow or no-internet

Storage gateway
* hybrid (on-prem + cloud)
* out of space: use file gateway 
* it is a VM (on-prem)

DataSync and transfer family
* agent based solution for 1 time migration of files to AWS
* EFS and FSx are locations for datasync to transfer files into
* transfer family legacy file transfer protocols to give older application the ability to read and write from s3

Migration Hub
* migration hub organize all your migration steps
* Database Migration service is the Go To Tool for database migration
* Server migration service is the tool to migrate out of the data center to AWS

Application Discovery Service
* Quickly migrate entire applications to the AWS cloud
* Agentless discovery can be used via OVA file deployment to vSphere
* Agent-based discovery collectes detailed information of VMs on both linux and Windows

Application Migration service
* AWS MGN
* automates lift & shift services for migrating infrastructure to AWS
* replicates source servers into AWS for non-disruptive cutovers
* RTO of minutes and RPO of sub-seconds


## Frontend Web & Mobile overview
Amplify
amplify offers tools that build out full-stack applications
2 services for developers
* amplify hosting
* amplify studio
management server-side rendering in AWS, easy mobile development, running full-stack applications.

Device farm
* device farm is an app testing service for testing and interacting with android, ioS and web apps
* it is also usable on phones and tablets hosted by AWS

it offers:
* Automated testing
* remote access testing via browser

Pinpoint
* pinpoint enables you to engage with customers through a variety of diffrent messaging channels
* intended for marketers, business users and developers

features
* projects
* channels
* segments
* campaigns
* journeys
* message template
* machine learning

where to use it
* for marketing
* for transactions
* for bulk messages


## Machine Learning Overview
Comprehend
comprehend uses NLP (natural language processing) to understand your text
uses
* call center analytics
* index and search product review
* legal briefs management
* processing financial documents

Kendra
allows you to create intelligent search service using ML models
uses
* research and development
* improve customer interactions
* minimize regulatory and compliance risk
* increase employee productivity

Textract
automatically extract text, handwritting and data from a scanned doc using ML
uses
* fiancial services
* health care and life sciences
* public sector

Time series data
* data points that are logged over a series of time allowing you to track your data

Forecast
* time-series forecast service to give you insights on your data

Fraud detector
is a service that uses ML for fraud detection in your data
uses
* fraud detection

Transcribe: convert speech to text
Lex: chatbot
Polly: voice response (this is how alexa is made)
Rekognition: automates facial recognition
Translate: language translation

SageMaker
features
* ground truth: labels jobs
* notebook: manage jupiter notebook
* training: train models
* inference: package and deploy

deployment types
* offline: works on stored data
* online: works on live stream

stages
* create a model
* create an endpoint configuration
* create an endpoint

model training
model creation

sageMaker neo customize ml models for diff ec2 types

to decrease cost
Elastic inference (EI) reduces delay and increase transmission of the inference deployed on sageMaker
it autoscales and is highly avaliabity


## media
transcoder: convert(transcode) media files
kinesis video streams: stream media 
