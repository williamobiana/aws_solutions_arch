## Why do we automate
* To save time
* Security (its easier to prevent security incidents)
* Consistency

3 Services for Automation
* CloudFormation
* Elastic Beanstalk
* Systems Manager

### Exam tips
* select anaswers that doesn't use manual steps
* automate everything
* assign the right tool for the right job
* keep the benefits in mind (faster, secure, consistent)
* it allows to create an immutable infrastructure


## CloudFormation
Delarative programming language
Create and Deploy template
perfect for creating immutable infrastructure

### Exam tips
* know the layout of the template
* cross-region
* troubleshooting: in case of error, cloudformation rolls back to lask known good state
* API calls: it makes the same API calls we do manaually
* Immutable: can easily create and destroy our entire architecture


## Elastic Beanstalk
It is a platform-as-a-service tool
Automates the deployment and management of all the services needed to run an application 

### Exam tips
* it is a simple 1 stop solution
* it is a platform as a service
* supports containers, windows, and linux applications. 
* simple for web applications
* it is not serverless


## System Manager
collection of tools to automate the management of ec2 architecture (also on-prem)

features
* Automation documents (can control AWS resources)
* run command (run scripts in OS)
* patch manager (managing your application versions)
* parameter store (store secrets)
* hybrid activation (control on-prem)
* session manager (remotely connect and interact with your architecture)

### Exam tips
collection of tools to automate the management of ec2 architecture (also on-prem)

features
* Automation documents (can control AWS resources)
* session manager (remotely connect and interact with your architecture)
* parameter store (store secrets)


## Exam tips recap
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

