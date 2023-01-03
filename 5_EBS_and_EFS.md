## EBS Overview
Understanding EBS volume
Mission Critical (where we would use ebs)
EBS volume types: solid state disk ssd
EBS volume types: hard disk drive hdd (MB/s-intensive)
IOPS vs Throughput


What are EBS (Elastic Block Storage) volumes
What are the diffrent types of EBS volumes with use case

EBS (elastic block storgae), are stroage volumes you can attach your ec2 to
just like a partition system disk, we can:
    create a file system
    run a database
    run an OS
    store data
    install applications

it is designed for mission critical workload
* production workloads
* highly avaliable: automatically replicated within a single AZ during hardware failure
* scalable: dynamically increase capacity and change volume type with no downtime or impact to performance

EBS volume types: general purpose SSD (gp2)
* 3 IOPS per GiB, up to maximum of 16000 IOPS per volume
* gp2 volumes smaller than 1 TB can burst up to 3000 IOPS
* good for boot volumes for dev and test apps that are latency sensitive

EBS volume types: general purpose SSD (gp3)
* predictable 3000 IOPS baseline performance and 125 mib/s regardless of volume type
* for apps that need high performance for low cost like mySQL, cassandra, hadoop analytics and virtual desktops
* can scale up to 16000 IOPS and 1000 mib/s for an additional fee
* top performing gp3 is 4xfaster than max gp2

Provisioned IOPS SSD (io1)
* high performance option (most expensive)
* up to 64000 IOPS per volume 50 IOPS per gib
* used if we need more than 16000 IOPS
* designed for i/o-intensive apps, large database, and latency-sensitive workloads

Provisioned IOPS SSD (io2)
* latest generation
* higher durability and more IOPS
* same price as io1
* up to 64000 IOPS per volume 500 IOPS per gib
* 99.999% durability
* designed for i/o-intensive apps, large database, latency-sensitive workloads and apps that need high durability

EBS volume types: hard disk drive hdd (MB/s-intensive)
Throughput Optimized HDD (st1)
* low cost hdd volume
* baseline throughput of 40 mb/s per TB
* ability to burst up to 250 mb/s per TB 
* max throughput of 500 mb/s per volume
* for frequently accessed throughput intersive workloads
* for big data, warehouse, ETL (extract, transform & load), and log processing
* cost effective way to store loads of data
* it cannot be a boot volume

Cold HDD (sc1)
* lowest cost option
* baseline throughput of 12 mb/s per TB
* ability to burst up to 80 mb/s per TB 
* max throughput of 250 mb/s per volume
* good choice for colder data requiring few scans per day
* good for apps that need lowest cost and performance is not a factor
* cannot be a boot volume

IOPS vs Throughput
IOPS
* measusre number of reads and writes operations per second
* important metric for quick transaction, low latency applications, transactional workloads
* ability to action read and writes very quickly
* choose provisioned IOPS SSD (io1 or io2)

Throughput
* measure number of bits reads or write per second (mb/s)
* important metric for large datasets, large i/o sizes, complex queries
* ability to deal with large dataset
* choose throughput optimized HDD (st1)

### Exam tips
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



## Volumes and Snapshots
what are volumes
what are snapshots
3 tips for snapshots
what to know about EBS volume

what are volumes?
* volumes exist on ebs, (like virtual hard disks)
* we need a minimum of 1 volume per ec2 (This is called the root device volume, where the OS is installed)

what are snapshots
* snapsots exists on s3 (it is a snapshot of the current volume state)
* it is a point in time copy of a volume
* snapshots are incremental (only changes that are made since the last snapshot are moved to s3)
* the first snapshot may take some time to create because it has no reference

3 tips for snapshots
* consistent snapshots: stop the ec2 and take a snap because ec2 save/caches its data when its not running
* encrypted snapshots: snap an encrypted ebs volume and the snap will be encrypted automatically
* sharing snapshots: you can share snaps in the region it was created, for multi-region we have to copy and paste manually

what to know about ebs volumes
* location: ebs volume should ALWAYS be in the same AZ as ec2
* resizing: you can resize the ebs on running ec2 (we have to extend the filesystem on the OS so that it can see the resized ebs)
* volume type: you can change volume type on running ec2 (eg change from gp2 to io2)

### Exam tips
* volumes exist on ebs, snapshots exist on s3
* snapshots are current volume state and are incremental
* first snap takes time, stop ec2 and detach volume before taking snapshot
* we can share snaps withn same region, for multi-region copy snap manually to region
* we can resize and change volume type on demand


## Protecting EBS volume with Encryption
EBS encryption
what happens when you encrypt
encryption explored
how to encrypt existing volumes

EBS encrypts volume with a data-key using AES-256 algorithm
ebs encryption uses KMS(key mgt service) and CMK(customer managed keys) when creating encrypted volumes and snaps

what happend when you encrypt ebs volume
* data at rest is encypted in the volume
* all data in transit btw the ec2 and volume is encrypted
* all snaps are encrypted
* all volume created from snap is encrypted

encryption explored
* it is handled transparently
* it has minimal latency
* copying an unencrypted snap allows encyption
* snaps of encypted volumes are encypted
* we can encrypt root device volume on creation

4 steps to encrypt an unencypted volume
* create a snap of unencypted root device volume
* create a copy of snap and select the encrypt option
* create an AMI from the encrypted snap
* use AMI to launch new ec2

### Exam tips
* data at rest is encrypted inside the volume
* all data in transit btw the ec2 and volume is encrypted
* all snaps are encrypted
* all volume created from snap is encrypted

4 steps to encrypt an unencypted volume
* create a snap of unencypted root device volume
* create a copy of snap and select the encrypt option
* create an AMI from the encrypted snap
* use AMI to launch new ec2


## EC2 Hibernation
EBS behaviour review
what is ec2 hibernation
ec2 hibernation in action

if we stop an ec2, the data is kept on ebs and remain there until ec2 is restarted
if ec2 is terminated, by default ebs will be terminated

when we start ec2
* OS boots
* userdata(bootstrap) script is run
* applications starts

on hibernation command, ec2 hibernations saves content from RAM to the ebs and shutsdown

when we start ec2 from hibernation
* ebs is restored to previous state
* RAM content are reloaded
* processes running on ec2 are resumed
* previously attached volumes are reattached and ec2 retains its instanceID

With ec2 hibernation, the ec2 boots up faster (because OS doesnt need a reboot since RAM is preserved)
* this is useful for long running processes
* services that take time to initialize

### Exam tips
* ec2 hibernation preserves RAM on EBS
* much faster to boot up (becasue we do not reload OS)
* ec2 RAM must be less than 150gb
* ec2 types include (C types(3,4,5), M types(3,4,5) and R types(3,4,5))
* avalaible for windows, amazon linux2, and ubuntu
* ec2 cant be hibernated more than 60 days
* avalable for on-demand and reserved ec2


## EFS Overview
what is EFS
use cases
overview
performance
controling performance
storage tiers

what is EFS (Elastic file system)
* managed NFS (network file system) that can be mounted on many ec2
* works with different ec2 in multi-AZs (shared storage)
* highly avaliable, scalabe but expensive

use-case
* content management system: you can easily share content between ec2
* web servers: single folder structure for website

overview
* uses nfsv4 protocol
* compatible with linux ami (windows is not supported at this time)
* uses encryption at rest using KMS
* file system scales automatically as we add files
* pay per use

performance
* 1000s of concurrent connections (many ec2 can be attached to it)
* 10gbps throughput
* can be scaled to petabytes

controlling performance
* general purpose for CMS and webservers
* MaxI/O for big data, media processing etc

storage tiers
comes with storage tiers and lifecycle management(move from 1 storage to another after x days)
* standard (frequently accessed files)
* infrequently accessed files

### Exam tips
* supports nfsv4 protocol
* pay per use
* can be scaled to petabytes
* can support thousands of concurrent connections (many ec2 can be attached to it)
* data is stored accross multiple AZs with a region
* read after write consistency


## FSx Overview
FSx for windows
FSx for windows vs EFS
FSx for lustre
FSx for lustre performance

FSx for windows
Provides fully managed native microsoft file system (to easily move windows based apps with file storage to AWS)
FSx it is built on windows server

FSx for windows vs EFS
windows
* a managed windows server that runs windows server message block (SMB)files
* designed for windows and windows apps
* supports active directory users, access control list, groups, security policies, distributed file system namespaces and replication

efs
* managed NAS filer for ec2 based on NFSv4
* among the first network file sharing protocols native to linux and unix

FSx for lustre
* manage file system for compute intensive workload
* high performance compute
* AI/ML
* Media data processing workflows
* electronic design automation

FSx for lustre performance
* we can launch and run lustre file system for process huge datasets of gigabytes per sec

### Exam tips
* EFS: distributed, highly resilient storage for linux instances, and linux based apps
* FSx for windows: centralized storage for windows based apps (sharepoint, microsoft SQL server, other native microsoft apps)
* FSx for lustre: high speed, high capacity distributed storage for high performance computing, financial modelling, etc. (lustre can store data on s3)


## AMI (Amazon machine images): EBS vs Instance store
What is an AMI
5 things you can base your AMI on
EBS vs Instance store
Instance store volume
EBS volumes

AMI is amazon machine image, it provides information required to launce an ec2
we must specify an AMI when we launch an ec2

5 things to based AMI on are:
* region
* OS
* system architecture (32bit or 64bit)
* launch permissions
* storage for root device

EBS vs Instance store
all AMIs are either backed by EBS or instance store
* EBS: root device is from ebs volume, created from ebs snapshot
* instance store: root device is from a template and stored in s3

Instance store volume (aka ephemeral storage: data will not be saved if instance is stopped)
* it cannot be stopped
* if underlying host fails (physical server), we lose our data
* we can reboot instance without losing our data
* if we delete instace, we lose the instance store volume

EBS volumes
* ec2 from ebs volume can be stopped
* we do not lose data on the ec2
* we can reboot ebs without losing data
* by default root device volume will be deleted on termination, but we can store the root volume in EBS volumes

### Exam tips
* instance store volumes are sometimes called ephemeral storage(data will not be saved if instance is stopped)
* instance store volumes cannot be stopped. if the underlying host fails, we lose all data
* ebs backed instances can be stopped. we will not lose the data on the instance if it is stopped
* we can reboot both EBS and instance store volume, and we will not lose our data
* by default, root volumes are deleted on termination. for EBS, we can tell AWS to keep our volume on termination

## AWS Backup
What is AWS backup
AWS backup with organizations
benefits

what is AWS backup
* backup allows to consolidate our data accross multiple services (ec2, ebs, Fxs, etc)
* it can also back up other services like RDS and dynamoDB

AWS backup with organizations (multiple consolidated AWS accounts)
* backups can be used with AWS organizations to backup multiple accounts 
* it gves centralized control across all AWS services, in multiple AWS accounts across the entire AWS organization

Benefits
* central management: single central backup for the organization across multiple services and accounts
* automation:  backup schedules, lifecycle policies
* improved compliance: enfored backup polices, encryption at rest and in-transit, alignment to regulatory compliance, easy auditing

### Exam tips
consolidation: backup allows to consolidate our data accross multiple services (ec2, ebs, Fxs, etc)
organization: we can use AWS organization + AWS backup to backup diffrent AWS services across multiple AWS accounts
benefits:
* central management: single central backup for the organization across multiple services and accounts
* automation:  backup schedules, lifecycle policies
* improved compliance: enfored backup polices, encryption at rest and in-transit, alignment to regulatory compliance, easy auditing


## Exam Tips Recap
### Exam tips1 (overview)
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

### Exam tips2 (volumes and snapshots)
* volumes exist on ebs, snapshots exist on s3
* snapshots are current volume state and are incremental
* first snap takes time, stop ec2 and detach volume before taking snapshot
* we can share snaps withn same region, for multi-region copy snap manually to region
* we can resize and change volume type on demand

### Exam tips3 (protecting ebs with encryption)
* data at rest is encrypted inside the volume
* all data in transit between the ec2 and volume is encrypted
* all snaps are encrypted
* all volume created from snap is encrypted

4 steps to encrypt an unencypted volume
* create a snap of unencypted root device volume
* create a copy of snap and select the encrypt option
* create an AMI from the encrypted snap
* use AMI to launch new ec2

### Exam tips4 (hibernation)
* ec2 hibernation preserves RAM on EBS
* much faster to boot up (becasue we do not reload OS)
* ec2 RAM must be less than 150gb
* ec2 types include (C types(3,4,5), M types(3,4,5) and R types(3,4,5))
* avalaible for windows, amazon linux2, and ubuntu
* ec2 cant be hibernated more than 60 days
* avalable for on-demand and reserved ec2

### Exam tips5 (efs)
* supports nfsv4 protocol
* pay per use
* can be scaled to petabytes
* can support thousands of concurrent connections (many ec2 can be attached to it)
* data is stored accross multiple AZs with a region
* read after write consistency

### Exam tips6 (FXs)
* EFS: distributed, highly resilient storage for linux instances, and linux based apps
* FSx for windows: centralized storage for windows based apps (sharepoint, microsoft SQL server, other native microsoft apps)
* FSx for lustre: high speed, high capacity distributed storage for high performance computing, financial modelling, etc. (lustre can store data on s3)

### Storage options
* s3: serveless object storage
* glarcier: archive
* efs: centralized storage across multiple AZs
* FSx for lustre: for high computing
* ebs: persistent storage for ec2
* instance store: emphemeral storage for ec2
* FSx for windows: for microsoft windows

### Exam tips7 (ami)
* instance store volumes are sometimes called ephemeral storage(data will not be saved if instance is stopped)
* instance store volumes cannot be stopped. if the underlying host fails, we lose all data
* ebs backed instances can be stopped. we will not lose the data on the instance if it is stopped
* we can reboot both EBS and instance store volume, and we will not lose our data
* by default, root volumes are deleted on termination. for EBS, we can tell AWS to keep our volume on termination

### Exam tips8 (backup)
consolidation: backup allows to consolidate our data accross multiple services (ec2, ebs, Fxs, etc)
organization: we can use AWS organization + AWS backup to backup diffrent AWS services across multiple AWS accounts
benefits:
* central management: single central backup for the organization across multiple services and accounts
* automation:  backup schedules, lifecycle policies
* improved compliance: enfored backup polices, encryption at rest and in-transit, alignment to regulatory compliance, easy auditing
