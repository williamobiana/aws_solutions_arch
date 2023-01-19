## DDos Overview
what is a DDoS attack
layer 4 attacks
amplification attacks
layer 7 attacks

* disributed denial of service (DDoS) attack is an attack is an attack that attempts to make your website/application unavailable to your end users
* this can be achieved by multiple mechanisms, such as large packet floods, by using a combination of reflection and amplification techniques, or by using large botnets.

layer 4 attack
* a layer DDoS attack is often referred to as a SYN flood. It works at the transport layer (TCP)
* To establish a TCP connection a 3-way handshake takes place. The client sends a SYN packet to a server, the server replies with a SYN-ACK, and the clients then responds to that with an ACK

SYN Flood
* a SYN flood uses the built in patience of the TCP stack to overwhelm a server by sending a large number of SYN packets and then ignoring the SYN-ACKs returned by the server
* This causes the server to use up resources waiting for a set amount of time for the anticipated ACK that should come from a legitimate client

What can happen?
* there are only so many concurrent TCP connections that a web or app server can have open, so an attacker sends enough SYN packets to a server, it can easily eat through the allowed number of TCP connections.
* This then prevents legitimate requests from being answered by the server.

What is an amplification attack
* Amplification/reflection attacks can include things such as NTP, SSDP, DNS, CharGEN, SNMP attacks
* This is where an attacker may send a third-party server (such as an NTP server) a request using a spoofed IP address

* That server will then respond to that request with a greater payload than the initial request (usually within the region of 28-54 times larger than the request) to the spoofed IP address.
* This means that if the attacker sends a packet with a spoofed IP address of 64bytes, the NTP server would respond with up to 3456 bytes of traffic

What is a layer 7 attack
* A layer 7 attack occurs where a web server receives a flood of GET or POST requests, usually from a botnet or a large number of compromised computers.

### Exam tips
* A distributed denial of service (DDoS) attack attempts to make your website/application unavailable to your end users
* Common DDoS attacks include layer 4 attacks such as SYN floods or NTP amplification attacks.
* Common layer 7 attacks include floods of GET/POST requests.


## Logging API calls with CloudTrail
What is CloudTrail
CloudTrail logging

What is Cloud Trail
* CloudTrail increases visibility to your user and resource activity by recording AWS management console and API calls
* You can identify which users and accounts called AWS, the source IP address from which the calls were made, and when the calls occurred.

CloudTrail logging
* Metadata around API calls
* The identity of the API caller
* The time of the API call
* The source IP address of the API caller
* The request parameters
* The response elements returned by the service

### Exam tips
what cloudtrail allows
* after-the-fact incident investigation
* near real-time intrusion detection
* industry and regulatory compliance
* Cloudtrail is CCTV for you AWS account
* it logs all API calls made to the account and stored in S3


## Protecting applications with shield
what is sheild
what is shield advanced
what are the costs

AWS sheild
* free ddos protection
* protects all AWS customers on Elastic Load Balancing (ELB), CloudFront, and Route53
* Protects against SYN/UDP floods, reflection attacks and other layer 3 and layer 4 attacks

AWS sheild advanced
* Provides enhanced protections for your applications running on ELB, Cloudfront and Route53 against larger and more sophisticated attacks
* Offers always-on, flow-based monitoring of network traffic and active application monitoring to provide near real-time notifications of DDoS attacks
* Gives you 24/7 access to the DDoS Response Team (DRT) to help manage and mitigate application layer DDoS attacks
* Protects your AWS bill against higher fees due to Elastic Load Balancing (ElB), Cloudfront, and Route53 usage spikes during a DDoS attack

costs
* starndard is free and enabled by default
* Advanced costs 3000 USD/month

### Exam tips
* shield protects layer 3 and layer 4 only
* protects agaisnt DDoS attacks
* cost 3000usd/month and gives 24/7 DDoS response team.


## Filtering Traffic with AWS WAF
What is WAF (web application firewall)
Layer 7
Behaviors
Conditions

What is AWS WAF
* WAF is a web application firewall that lets you monitor the HTTP and HTTPS requests that are forwarded to amazon CloudFront or an Application Load Balancer
* WAF also lets you control access to your content
* we can configure conditions such as what IP addresses are allowed to make this request or what query string parameters need to be passed for the request to be allowed
* The Application load balancer or cloudfront will either allow this content to be recieved or give an HTTP 403 status code

WAF operates on Layer 7 (for layer 7 attacks)

Behaviors
3 different behaviours
* Allow all requests excepts the ones you specify
* Block all requests except the ones you specify
* Count the requests that match the properties you specify

conditions
* you can define conditions for web requests such as:
* IP addresses that request originate from
* Country that requests originate from
* values in request headers
* Presence of SQL code that is likely to be malicious (known as SQL injection)
* Presence of a script that is likely to be malicious (known as cross-site scritping)
* Strings that appear in requests - either specific strings or strings that match regular expression patterns

### Exam tips
* WAF operates at layer 7
* scenerio on how to block layer 7 DDoS attacks, sql-injections, cross-site mapping: think WAF
* WAF can block access to specific contries and IP addresses
* Guarding your network with GuardDuty


## Guarding your network with guardDuty
What is guardDuty
features
setting up GuardDuty

GuardDuty is a threat detection service that uses ML to continously monitor for malicious behaviour
* Unusual API calls, calls from a known malicious IP
* Attempts to disable Cloud Trail logging
* Unauthorized deployments
* Compromised instances

Features
* alerts apper in guardDuty console and cloudwatch events
* recieves feed from 3rd parties like proofpoint and crowdstrike as well as AWS security about malicious domains and IP addresses
* monitors cloudTraillogs VPC flow logs and DNS logs

GuardDuty features
* Centralize threat detection across multiple AWS accounts
* automated response using CloudWatch events and lambda
* Machine learning and anomaly detection

Setting up guardDuty
Threat detection
* 7-14 days to set a baseline - what is normal behavior on your account?
* once active, you will see finding on the GuardDuty concole and cloudwatch events, only if guadDuty detech behaviour it considers a threat

Pricing
30 days free, then charge based on
* Quantity of cloudtrail events
* Volume of DNS and VPC flow logs data

### Exam tips
* GuardDuty is a threat detection service that uses ML to continously monitor for malicious behaviour
* uses AI to learn what normal behaviour look like and alert of abnormal behaviour
* updates a DB of known malicious domains 
* monitors cloudtrail logs, VPC flow logs and DNS logs
* findings appear in the guardDuty dashboard


## Cenralizing WAF management with AWS firewall manager
what is firewall manager
manage security across multiple accounts
benefits

Firewall manager
* it is a security management service in a single pane of glass. that allows you to centrally set up and manage firewall rules across multiple AWS accounts and applications in AWS organizations
* we can create WAF rules for Application Load Balancers, API gateways and CloudFront distributions.
* we can also mitigate DDoS attacks using AWS Shield Advanced for your Application Load Balancers, ElasticIP, cloudFront distributions, etc.

Benefits
* Simplify Management of Firewall Rules across your accounts
* Ensure compliance of exisiting and new applications

### Exam tips
* if scenrio talks about multiple AWS accounts that need to be secure centrally: think AWS firewall manager


## Monitoring S3 Buckets with Macie
what is macie
personal identifiable information (PII)
automated analysis of data
macie alert

* Macie uses ML and pattern matching to discover sensitive data stored in s3
* alerts if buckets are unencrypted or public
* alerts if bucket is shared with external party
* great for frameworks like HIPAA and GDPR

* we can filter and search the alerts
* alerts sent to eventbridge can be intergrated with security incident and event management system
* can be intergrated with security hub, and step functions

### Exam tips
* automated way of disovering personal identification
* uses AI to analyze data in s3, and help identify PII, PHI and financial data
* great for HIPAA and GDPR compliance, and preventing indentify theft
* alerts sent to eventbridge can be intergrated with security incident and event management system
* Automate remediation actions with service like step functions


## Secure our OS using Inspector
what is inspector
findings
types

it is automated security assesment service
it automatically assesses applications for vulnerabilites or deviations from best practices

2 types of assesment
* Network assesment (agent is NOT required): checks if ports can be reached from outside vpc
* Host assesment (agent is required): vulnerable software, host hardening and security best practices

how it works
* create assesment target
* install agents on ec2 instances
* create assesment template
* perform assesment run
* review findings against rules

### Exam tips
inspector is used to perform vulnerability scans on ec2 and VPC


## Managing Encryption Keys with KMS and CloudHSM
KMS
Encryption keys
CloudHSM
KMS vs CloudHSM

Key Management Service for creating and controlling encryption keys to access AWS services

CMK is (customer master key), it can incrypt and decrypt data
You control the lifecycle of the CMK and who can use it

HSM is hardware sercurity model, it is physical computing device that safeguards and manage digital keys, it can encrypt and decrypt data. it contains 1 or more cryptoprocessor chips.

3 ways to generate CMK
* AWS creates CMK in KSM (we can perform key rotations every year)
* import from my key managment infrastructure
* generate and use in a CloudHSM

we can manage access to the KMS CMKs with resource-based policies

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

### Exam tips
* KMS is a managed service that makes it easy to create and control the encryption keys used to encrypt your data
* you start using the service by requesting the creation of CMK, you control the lifecycle of the CMK as well as who can use or manage it.

3 ways to generate CMK
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


## Stroing secrets in secrets manager
what is secrets manager
what we can store
automatic rotation

secrets manager is a service that sercuely stores, encrypts, and rotates your database credentials and other secrets.
* encryption iin transit and at rest using KMS
* automatically rotates credentials
* apply fine-grained access control using IAM polices
* costs money but is highly scalable

automatic rotation
* if we enable rotations, secrets manager immediately rotates the secret once to test the configuration
* Ensure all your applications that use these credentials are updated to retrieve the credentials from the secret using secrets manager
* if we are still using embedded credentials, disable rotation

### Exam tips
* secrets manager can be used to securely store your application secrets: database credentials, API keys, SSH keys, passwords etc.
* application use the secrets manager API
* rotating credentials is super easy, but be careful
* when enabled, secrets manager will rotate credentials immediately
* make sure all applications instances are configured to use secreats manager before enabling credential rotation


## Storing secrets in parameters store
AWS systems manager that provides hierarchical storage for configuration data management and secrets management
Parameter store is free

Limitation of parameter store
* limit at 10000 parameters
* no key rotation

### Exam tips
* If scenrio ask to choose between parameter store and secrets manager. choose parameter store to minimize cost
* if we need more than 10000 parameters and key rotation, choose secrets manager


## Temporarily sharing s3 objects using presigned URLs or cookies
* all objects in s3 are private by default.
* only object owner has access, he can share object with others by creating a presigned url using thier own security credentials.
* to create a presigned url, you need a security creds, specify buckek name and object key, indicate the HTTP method, and expiry time
* the presigned url are vaild for a specified duration
* presigned cookies can be used if the you want to provide access to multiple locked files, the cookie is saved on the user's computer
you can use the CLI to presign the object


## Advanced IAM policy Documents
Amazon resource names (ARNs)
IAM Policies
Permission boundaries

ARN idenfies resources within amazon
arn:partition:service:region:account_id:resource or resource_type:resource

:: (2 colons means omitted value)

IAM policies
recap anatomy of IAM policy

permission boundaries allow or deny permissions to particular resources

### Exam tips
* not explicitly allowed == implicitly denied
* explicitly denied > everything else
* only attached polices have an effect
* multiple policies can join all policies
* AWS polices vs customer managed


## AWS certificate manager
* create, manage and deploy public and private SSL certificate for your aws environment

benefits
* cost: it is free, but you still pay for resources
* automated renewals and updated deployment
* easy to set up: remove all manual process, with creating csr, generating key-pair

### Exam tips
* supported services to intergrate with: ELB, Cloudfront, API gateway

benefits
* cost: it is free, but you still pay for resources
* automated renewals and updated deployment
* easy to set up: remove all manual process, with creating csr, generating key-pair


## Auditing continuosly with AWS Audit Manager
automated service that continously audit AWS usage to keep you compliant with industry standards and regulations

usecase:
* transition from manual to automated evidence collection
* continous auditiing and compliance
* internal risk assesment


## download complaince documents with AWS artifacts
asking about audits and download reports: think artifacts
this is usually a disractor question


## Authenticating access with amazon cognito
* cognito provides authentication, authorization and user mangement for apps without custom code

usecase
* authentication
* 3rd party authentication
* access server side resources
* access to AWS appsync resources

types of pools
* user pools: list of users that have sign-up and sign-in options for application
* identity pools: list of users you give access to other AWS services

cognito flow
* dropbox - userpool (recieves token) - dropbox
* dropbox - identity pool (exchange token for access credentials) - dropbox
* dropbox - AWS services (using credentials)

### Exam tips
* user pools: list of users that have sign-up and sign-in options for application
* identity pools: list of users you give access to other AWS services
* you can use userpool and identity pool together or seperately

cognito flow
* dropbox - userpool (recieves token) - dropbox
* dropbox - identity pool (exchange token for access credentials) - dropbox
* dropbox - AWS services (using credentials)


## Analyzing root cause using amazon detective
detective can analyze, investigate and quickly identify root cause of suspicious activity
it is usually a distractor question

### Exam tips
* Detects root cause
* dont confuse with inspector
* it is usually a distractor


## Protecting VPC with Network Firewall
* AWS Network firwall with physical firewall to vpc
* it is a managed infrastructure
* works with AWS firewall manager

use-case
* filter internet traffic
* filter outbound traffic
* inspect vpc-vpc traffic

### Exam tips
* filtering network traffic before it reached IGW, or intrusion prevention system: think network firewall


## Leveraging AWS security hub for collecting security data
single place to view all security alerts from security services (guardDuty, inspector, macie and firewall manager).
works accross multiple accounts

use-case
* corrolate security findings for insight
* automated checks for security management

### Exam tips
* single place to view all security alerts from multiple security services and multiple accounts: AWS security hub


## Exam tips recap
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
