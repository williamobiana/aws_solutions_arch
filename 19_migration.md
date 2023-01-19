## Migrating Data with AWS Snow Family
How do we migrate data
Snow family
Snowcone
Snowball Edge
Snowmobile

snow family
* stacks of harddrives given to you by AWS
* where you load up your applicatioons and send them across to AWS

Snowcone (small)
Snawball edge (mid)
Snowmobile (Large)

### Exam tips
* transporting data: use a member of the snowfamily


## Storage Gateway
hybrid cloud storage service to merge on-prem resource with AWS
useful for 1 time migrations or long-term pairing

file gateway
* file gateway is for caching local files
* NFS/SMB mount that backup data to s3
* keeps a local copy of recently used files
* extends on-premise storage
* helps with migrations to AWS

volume gateway
* volume gateway is for backing up drives
* backs up disk in iSCSI mount
* cached or stored mode
* create EBS snapshots
* perfect for backup and migration

tape gateway
* replace physical tapes
* doesnt change current workflow
* Encrypted communication

Storage gateway is hybrid storage
* for scenerios with data migration, think storage gateway

## AWS DataSync
Agent-based solution for migrating on-premises storage to AWS.
Allows for easily move data between NFS and SMB shares into the cloud

it is a migration tool for a 1 time migration
it is a distractor

## AWS transfer family
easily move files in and out of S3 & EFS using
* secure file transfer protocol (SFTP)
* file transfer protocol over SSL (FTPS)
* file transfer protocol (FTP)

for bring legacy application storage to the cloud

## Moving to the cloud using migration hub
Hub to track progress of application migration to the cloud
it intergrates with:
Server Migration Service (SMS)
Database Migration Service (DMS)

Server migration service converts your VMware architecture to VMami
DMS converts oracle/MySql database to aurora


## Migrating Workloads to AWS using Application Discovery Service or Application Migration Service
Application Discovery Service
Plan all migration to AWS by collecting on-prem usage and config data 
simplify migration needs
tracks and groups the servers

discovery types
* agentless
* agent based (installed agent on servers)

Application Migration Service (AWS MGN)
* automate migration quickly to AWS
* used for physical, virtual and cloud servers
* replicates source server into AWS

features
* Recovery time objective RTO
* Recovery point objective RPO


## Migrating databases from on-prem to AWS using AWS Database Migration Service (DMS)
service
how it works
concepts
schema conversion tool
migration types
migrating large stores with snowball

DMS
* DMS is a migration tool for migrating databases to/from the cloud
* at least 1 endpoint must be AWS
* can be 1 time migration or on-going
* can translate the scehma to new platforms
* we gain the advantage of AWS cost, effecinecy etc

how it works
* DMS is server running replication software
* create source and target connections reffered to as endpoints
* schedule tasks
* create table and primary keys
* create tables before hand
* leverage SCT

concepts
* migrate database with same engine or different engines, 
* at least one endpoint should be pointing to AWS

SCT schema conversion tool
* convert exisiting database schema from one engine to another
* convert OLAP and OLTP databases and data warehouse
* can be used by aupported AWS RDS engine, aurora, Redshift
* can be used with database running on ec2 or stored in s3

migration types
* full load
* full load + change data capture (CDC)
* CDC only
CDC guarantees transactional integrity

migrating large stores with snowball
* Terrabytes
* bandwith throttles
* snowball edge
* SCT
* load converted data
* CDC compactible


## AWS migration hub AWS server migration service
migration hub
migration phases
server migration service (SMS)
sms use case

Migration Hub
* place to discover existing servers, plan your migration efforts, track migration statues
* visualize connection and server/database statuses
* start migrations immediately or group servers into application groups
* intergrates with application migration service or database migration service
* discovers and plans migration

Migration Phases
* Discover
* Migrate
* Track

Server migration service
* automates migration
* supports VMs
* incremental replication

use cases
* simplifies migrations
* multi-server migrations
* wide support
* incremental testing
* minimize downtime



## Exam tips recap
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














