## CloudWatch

CloudWatch
* Monitoring and obersavabity platform
* collecting metrics and analyzing the data from common services like (EC2, RDS, Lambda, CloudTrail, on-prem)

Features
* System Metrics (checking resource metrics)
* Application Metrics (install agent in ec2, for checking for things like Apache and other application metrics)

Types of metrics
* Default metrics: CPU, Network bandwith
* Custom metrics (install an agent): ec2 memory, EBS storage capacity

### Exam tips
* Cloudwatch is used for monitoring
* what checks do you get by default
* alarms: there is no deafult, you have to create the alarm yourself
* you can see default metrics for resource like cpu utilization, but cannot see deep like memory utilization unless you install a cloudwatch agent
* the more managed the service by AWS, the more default checks.
* standard vs detailed: stardard reporting interval is every 5 minutes, for detailed metrics as low as 1 minute would have a cost


## Application monitoring with CloudWatch Logs
CloudWatch Logs, allows us to monitor, store and access logs files from diffrent sources
we can query these logs for insights

3 CloudWatch terms
* Log Event: when an action happens on a service (eg: EC2)
* Log Stream: a collection of a log event from a single source (1 ec2 instance)
* Log group: a collection of log streams  (example is High Avaliblity: multiple ec2 instance log streams for an application)

Features
* Filter patterns for specific data in the logs
* Cloudwatch logs insights by using SQL commands to query the logs
* Alarms

Note: any resource that needs to write to cloudwatch logs need role-based permissions

demo
* install cloudwatch agent
* configure the agent with the configure wizard

### Exam tips
* if we need to process the logs, then it goes to cloudwatch
* if we dont need to only store the logs, then it goes to s3
* if we need to process the logs in real time, then we use Kinesis
* we collect metrics and analyzing the data from common services like (EC2, RDS, Lambda, CloudTrail, on-prem)
* we can create cloudwatch alarm based on the filter patterns we have created
* it is agent based, it must be installed and configured
* using SQL queries, is done with cloudwatch logs insights


## Monitoring with Amazon managed service for prometheus and Amazon managed Grafana
Amazon Managed Grafana
* fully managed AWS service for secure data visulazation.
* we can query, corrolate and visualize operation metrics, logs and traces from diffrent sources

Overview
* ease to use
* we can create workspace (each workspace is a server), for grafana activities
* AWS managed
* secure (SSL, auditing)
* pricing is based on active user per workspace
* data sources: (CloudWatch, prometeous, OpenSearch, Timestream)

usecase
* container metric visualization: connect to promethous for visualization of ECS, EKS, Kubernetes cluster
* IoT: monitoring Iot and edge device data
* Troubleshooting: centralizing dashboard for effecient troubleshooting

Amazon Managed Prometheus
* serverless, Prometheus-compatible service used for securely monitoring container metrics at scale

Overview
* Open-Source Prometheus
* Prometheus data model with AWS-managed scaling and availability
* Automatic scaling
* Designed for high avaliability: replicated across 3 AZs
* works with clusters (EKS or on-prem kubernetes)
* PromQL: uses PromQL query language
* Data rentention: strored in workspace for 150 days (deleted afterwards)

### Exam tips
Amazon Managed Grafana
* fully managed AWS service for secure data visulazation.
* we can query, corrolate and visualize operation metrics, logs and traces from diffrent sources

Amazon Managed Prometheus
* serverless, Prometheus-compatible service used for securely monitoring container metrics at scale

Security and Scaling
* AWS handles high avalaibilty and auto scaling, while we can leverage VPC endpoints for secure VPC access

Grafana data sources
* CloudWatch, prometeous, OpenSearch, Timestream etc

Prometheus Uses
* works with clusters (EKS or on-prem kubernetes)

Grafana Uses
* container metric visualization, IoT, Troubleshooting


## Exam tips recap
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

