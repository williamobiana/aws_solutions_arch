## Managing Accounts with Organizations

What is organizations
key features
service control polices SCP

free governace tools to create multiple accounts

features
* Logging account (to collect logs from cloudTrail)
* programatic creation: create and destroy with API call
* reserved instances: RI can be shared accross multple other accounts
* consolidated billing: primary account for billing
* service control polices: creates limits on user permissions

### Exam tips
* to centralize logs and SCPs: we can use Organizations
* cost: easy to pay bills
* CloudTrail: centralize cloudTrail logs
* Sharing Reserved Instances: share RIs accross other accounts


## Sharing Resources with RAM
RAM resource access manager
benefits

allows to share resources in another account in our organization

we can share resources like
* Transist gateways
* VPC subnets
* licences
* Route53 resolver...etc

### Exam tips
* RAM is great to share resources accross same region
* VPC peering is best to share resources in diffrent regions
* VPC peering is ideal for seperate networks
* RAM is free, but we pay for the architecture
* RAM allows organization to share the architecture


## Cross-Account Role Access
creating a primary account for sign-in (with credentials)
and other accounts can assume-role

steps
create role
add account-ID
add permissions

### Exams tips
* Its is preffered to create roles than a new IAM credential
* audting or temporary credentials?: create a role for them
* roles are temporary


## Inventorty Management with AWS Config
what is config
state of architecture

inventorty management and control tool for our account and resources
* it can query
* we can set rules on our resources
* show history in the environment

### Exam tips
* Config enforce standards and rules
* Standards
* We can track Deleted Resources
* Enforcement (config and lambda can do this)
* we can roll up all rules in one region


## Offloading Active Directory to Directory store
Directory service is a fully managed version of active directory

types
* Managed Microsoft AD (migrating everything to AWS)
* AD conncetor (tunnels AWS to on-prem AD)
* Simple AD (simple)

### Exam tips
* know the 2 main types
* for AD used directory store over ec2
* it is ok to have AD on-prem


## Exploring with Cost Explorer
why we budget
cost explorer
features

cost explorer create report where the money is being spent

feature
* breaks down cost based on services
* breaks down cost based on timeframe
* breaks down cost based on filter parameters

### Exam tips
if it scenerio asks about cost, think cost explorer
* use tags to filter
* use cost explorer to budget
* predictive


## Using AWS budgets
for planning and tracking your spend

4 types of budget
* cost budget (how much are we spending)
* usage budget (how much are we using)
* reservation budget (are we being effiecnt with our RIs)
* savings plan budget (is it covered by our savings plan)

### Exam tips
* create budget with cost explore
* use tagging as a filter
* create alerts to monitor costs


## Cost and Usage Reports (CUR)
* its very comprehensive on cost and usage data
* publish billing reports to S3 for centralized collection
* breakdown costs down by the timespan (hour, day, month), service and resource, or by tags.
* daily csv (it reports to s3 1nce a day)
* intergration 

use case
* can be used for Org units or indiviual accounts
* track saving plans
* monitor on-demand capacity reservation
* comprehensive analysis of data transfer 
* comprehensive cost allocation tag resource spending

### Exam tips
* comprehensive
* centralized storage
* AWS organizations
* AWS intergrations
* Keyword and Scenarios


## Reduce compute spend using savings plans and Compute Optimizer
What is compute optimizer
* optimizes: configurations and utilization metrics of AWS resources
* reporting
* graphs
* informed decisions

resources the service works with
* EC2
* autoscaling group
* EBS
* Lambda

supported accounts
* standalone (without organization enabled)
* member account (single member account within an org)
* management organization account

Note: compute optimizer is disabled by default

savings plan
* flexible pricing
* lower prices
* variety
* sageMaker plans
* commitments are long term
* pricing plan (all, partial, no upfront)

saving plan types
* compute savings
* ec2 instance savings
* sageMaker savings

using and applying saving plans
* view recommendations
* it is automatically calculated
* add to cart and purchase
* RI have to be used up first before using savings plan
* consolidated billing family


## Auditing with trusted advisor
it is a fully managed auditing tool helping you follow the best practices

5 areas it looks at
* cost optimization: (are you wasting money on resources)
* performance: is the arch standard to manage performance
* security: is there vunrability
* fault tolerance
* service limits

### Exam tips
* automate response
* alerts with SNS
* cost (more features are in the business and enterprise support plan)
* know its limit (it cant fix problems, we need lambda for that)
* automate with eventBridge/Cloudwatch events


## Enforcing account gorvenance via AWS control tower
* Control Tower is used to govern multiple account environments
* orchestration service that automates account creation and security contorls 
* extension: Extends AWS organizations to prevent goverance drift and leverage guard rails
* new aws accounts: users can provison new accounts quickly, using account factory (central admin established compliance polices)
* summary: simplest way to create and manage sercure, compliant, multi-account environment based on best practices

features
* landing zone: well-architected multi-account environment based on compliance and security best practises
* Guardrails: rules providing continous governance
* Account Factory: template for standarizing configs for new accounts
* CloudFormation StackSet: automate deployment of templates for governance resources
* Shared accounts: 3 accounts created during landing zone creation

Guardrail
preventive vs detective
preventitive: 
* disallows violating actions
* uses service control polices
* status can be enforced or not enables
* supported in all regions

detects:
* detects and alert no compliant resources
* uses AWS config rules
* status can be clear, in violation or not enabled
* suppored only in certain region

### Exam tips
* Governace (to govern multi-accounts)
* Accounts (automate account deployment)
* Shared accounts (managing accounts, log archive, audit)
* Preventive Guardrail (uses SCP to prevent non compliant actions)
* Detective Guardrail (uses AWS Config rules to detect and alert on violating actions or changes)


## Managing software licences with AWS licence Manager
* simplifies managing software licenses
* centrally manage AWS accounts and on-prem
* control and visibility on licences and enabling licence usage limits
* reduces overages and penalties via inventory tracking and rule-based controls
* versatile (supports software based on vCPU, physical cores, sockets and number of machines)


## Monitoring Health Events in the AWS personal health dashboard (AWS health)
* visibilty of resorce performance, and avaliability of services or accounts
* view how health events affect you
* maintains timeliness and relevant information with the events
* be prepared and automate
* gives notification and alerts

you can automate actions based on incoming events using amazon eventbridge

AWS health concepts
* health event
* account specific events
* public events
* health dashboard
* event type code
* event type category
* event status
* affected entities

### Exam tips
health visibility
alerting
automate actions based on incoming event using eventbridge to trigger lambda
specific and public events


### Standardizing Deployments using AWS service catalog and AWS proton
AWS service catalog
* create and manage catalogs of approved IT services
* multipurpose
* centralized to maintain compliance
* end user friendly
* end-user provision cloudformation templates

benefits
* restricts launching product to a list pre-approved solution
* end users can browse products and deploy  approved services on thier own
* access control : add constriants and resources tags to grant access to products using AWS IAM
* versioning 

AWS Proton
service that creates and manage infrastructure and deployment tooling for users as well as serverless and container based applications
* automate infrastructure as code provisioning and deployments
* define standardized infrastructure for your serverless and container based apps
* use templates to define and manage apps stacks that contain all components
* Proton automatically provisons resources, configures CI/CD and deploys the code
* supports CloudFormation and Terraform IaC providers
* empowers developers to use as a self-service tool


## Optimizing Architectures with AWS Well-Architected Tool
6 pillars of the AWS well-architected Framework
* operational excellence
* reliability
* security
* Performance Efficiency
* Cost Optimization
* Sustainability

tool
* Provides consistent process for measuring cloud architectures
* enable assistance with documenting workloads and architectures
* guides for making workloads reliable, secure, efficient and cost effective
* measures workloads based on years of AWS best pracitces
* intended for specific audiences (technical teams, CTOs, architecture and ops teams)


## Exam tips recap
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












