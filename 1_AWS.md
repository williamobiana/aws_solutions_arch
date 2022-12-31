# Fundamentals

## Shared Responsibilty model for AWS
AWS Responsibilty:
Responsible for Security OF the cloud (Physical Security)
* Regions (City with 2 or more AZs)
* Avaliability Zones (Datacenters with redundant power, networking and connectivity)
* Edge Location (Cachecenters like CloudFront & AWS Content Delivery Network)
* Compute mantainance
* Storage
* Database
* Networking

Customer Responsibilty:
Responsible for Security IN the cloud 
* Customer data 
* Platform
* Application
* IAM (identity & access management)
* OS configuration
* Network & firewall configuration
* Client side and Server side incryption
* Network traffic protection

Shared Responsibilty:
Encryption is a shared Responsibilty


## Key Services 
1. Compute
2. Storage
3. Database
4. Networking

Compute:
* EC2
* Lambda (serverless EC2)
* Elastic Beanstalk (provisioning engine for applications)

Storage:
* S3
* EBS (virtual harddisk)
* EFS
* FSx
* Storage gateway

Database: (store and retrive information)
* RDS (Relational Database)
* DynamoDB (Non-Relational Database)
* RedShift (Data Warehousing)

Networking:
* VPCs (virtual Datacenters)
* Direct Connect
* Route53 (DNS)
* API Gateway (serverless)
* AWS Global Accelerator


## Well-Architected Framework
* Whitepapers (AWS Well-Architected Framework)
* Well-Architected Framework Pillars
    1. Operational Excellence
    2. Performance Efficiency
    3. Security
    4. Cost Optimization
    5. Reliability
    6. Sustainability