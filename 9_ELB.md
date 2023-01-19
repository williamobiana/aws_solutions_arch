## ELB Overview

What is Elastic Load Balancing 
Types of Load balancers
Health checks

ELB automatically distributes incoming traffic across multiple targets like ec2. This can be done across multiple AZs

3 types of load balancing
* Application load balancer: best for http/https traffic, operate on layer 7, application aware (intelligent load balancer)
* Network load balancer: operates at connection level(layer 4), capaple of handling millions of request per second, while managing low latency, performance load balancer 
* Classic load balancer: legacy load balancers, we can load balance http/https apps and use layer 7 specific features such as x-forwarded and sticky sessions. classic/test/dev load balancer

Health checks
All AWS loadBalancers can be configured with health checks. Health checks periodically send requests to load balancers registered instances to test their status.

the status of the healthy instance at time of health check is InService.
the status of unhealthy instance at time of health check is OutOfService.

the load balancer performs health checks on all registered instances (to check weather it is in healthy or unhealthy state)
the load balancer route request only to the healthy instance (if instance is unhealthy, it stops routing traffic to it)
the load balancer will resume routing requests to the instance when it has been restored to a healthy state.

### Exam tips
3 types of load balancers
* Application load balancer: best for http/https traffic, operate on layer 7, application aware (intelligent load balancer)
* Network load balancer: performance load balancer 
* Classic load balancer: classic/test/dev load balancer

Health checks
* we can use health checks to route our traffic to instances or targets that are healthy


## Application load balancers
Layer 7 load balancing
Listeners, rules and target groups
Path based routing
application load balancer diagram
limitations of application load balancers

layer 7 load balancing
* ALB functions at layer 7, when LB recieves request, it evalutes based on difined rule, and selects ec2 to route to.

Listerners
* A listerner checks for connection requests using configured protocols and ports
* we define rules that determine how LB routes requests to registered targets
* each rule consists of: a priority, one or more actions, one or more conditions

Rules
* when the conditions of a rule are met, actions is performed (we must define a deafult rule and optional additional rules)

target group
* each target group routes requests to 1 or more registered targets, using specified protocol and port number

Limitations
* only supports http and https
* for https, we must have SSL/TLS certificate on load balancer

Demo
create vpc
create 3 subnets
create 3 ec2 instance for each subnet
create target group
create application load balancer

### Exam tips
* Listeners: a listerner checks for connection requests using configured protocols and ports
* Rules: Determine how the LB routes requests to registered target, each rule consists of: a priority, one or more actions, one or more conditions
* Target groups: each target group routes requests to 1 or more registered targets, using specified protocol and port number

Limitations
* only supports http and https
* for https, we must have SSL/TLS certificate on load balancer
* LB uses certificate to terminate frontend connection and decrypt request from user before sending them to target


## Extreme performance with network load balancers
layer 4 loadbalancing
request receieved, listeners, target groups
ports and protocols
usecase
encryption

layer 4 loadbalancing
* network loadbalancer operates on layer 4 (OSI: transport layer)

request recived
* when loadbalancer receives a connection request, it selects a target from target group for the default rule
* it attempts to open a TCP connection to the selected target on the port specified in the listener config

listeners
* checks for connection request from clients using the protocol and port we configure
* the listener on a network LB then forwards the request to the target group. There are no rules, unlike with Application LB.

Target group
* Each target group routes requests to 1 or more regiestered targets, such as ec2, using the protpcol and port number we specify

ports and protocols
* protocols: TCP, TLS, UDP, TCP_UDP
* ports: 1-65535

Encryption
* we can use TLS listener to offload the work of encryption and decryption to the LB so the app can focus on business logic
* if the listener port is TLS, we must deploy exactly 1 SSL server certificate on the listener

usecase
* best suited for load balancing TCP traffic when extreme performance is required.

### Exam tips
* network loadbalancer operates on layer 4 (OSI: transport layer)
* use when extreme performance is required.
* use when we need protocol not supported by ALB
* network loadbalancer can decrypt traffic, we will need to install the certificate on the load balancer


## Classic load balancers
classic loadbalancers
x-forwarded-for
gateway timeouts

legacy loadbalancers 
* we can loadbalance http/https apps and use layer 7 features like x-forwared and sticky session.
* we can also use strict layer 4 load balancing for apps that rely on the TCP protocol

x-forwarded-for
* when traffic is sent from a load balancer, the ec2 access logs contain the IP address of the proxy or load balancer only
* to see the original IP address of the user, the x-forwarded-for request header is used.

gateway timeouts
* if apps stop responding, the classic LB responds with a 504 error.
* this means the app is ahving issues
* this could be either at the web server layer or database layer

### Exam tips
* 504 error means the gateway has timed out. (this means the app is not responding within the idle timeout period)
* Troubleshoot the app (check if its webserver or db server)
* if we need the ipv4 address of the end user, (look for the x-forwared-for header)


## Getting stuck with sticky sessions
What are sticky sessions?
Classic load balancers route each request independently to the registered ec2 instance with the smallest load

* Sticky sessions allow us to bind a user's session to a specific ec2 (stick a user to an ec2)
* this can cause issues if the ec2 has been terminated (it will generate error)
* to solve this we have to disable sticy sessions

we can enable sticky sessions on ALB, but traffic will be sent to the target group (ec2 can be terminated and replaced as long as it is in the target group)

### Exam tips
* sticky sessions enable users to stick to same ec2
* useful if we are storing data locally on that ec2
* if we remove ec2 from pool, the LB will still direct traffic to it, unless we disable sticky sessions


## Leaving the Load Balancer with deregistration delay
what is deregistration delay? (connection draining)
* Allows LB to keep existing connections open if ec2 are deregistered or become unhealthy
* this enable LB to complete in-flight requests made to ec2 that de-registering or unhealthy
* we can disable deregistration delay if we want the LB to immediately close connections to the ec2 that are deregistering or have become unhealthy

### Exam tips
* enable deregistration delay: keep existing connections open if ec2 becomes unhealthy
* disable deregistration delay: if we want LB to immediately close connection to ec2 that are deregistring or unhealthy


## Exam Tips Recap
### Exam tips
3 types of load balancers
* Application load balancer: best for http/https traffic, operate on (layer 7), application aware (intelligent load balancer)
* Network load balancer: performance load balancer (layer 4)
* Classic load balancer: classic/test/dev load balancer (layer4/7)

Health checks
* we can use health checks to route our traffic to instances or targets that are healthy

### Exam tips (ALB)
* Listeners: a listerner checks for connection requests using configured protocols and ports
* Rules: Determine how the LB routes requests to registered target, each rule consists of: a priority, one or more actions, one or more conditions
* Target groups: each target group routes requests to 1 or more registered targets, using specified protocol and port number

Limitations
* only supports http and https
* for https, we must have SSL/TLS certificate on load balancer
* LB uses certificate to terminate frontend connection and decrypt request from user before sending them to target

### Exam tips (NLB)
* network loadbalancer operates on layer 4 (OSI: transport layer)
* use when extreme performance is required.
* use when we need protocol not supported by ALB
* network loadbalancer can decrypt traffic, we will need to install the certificate on the load balancer

### Exam tips (classic LB)
* 504 error means the gateway has timed out. (this means the app is not responding within the idle timeout period)
* Troubleshoot the app (check if its webserver or db server)
* if we need the ipv4 address of the end user, (look for the x-forwared-for header)

### Exam tips (sticky session)
* sticky sessions enable users to stick to same ec2
* useful if we are storing data locally on that ec2
* if we remove ec2 from pool, the LB will still direct traffic to it, unless we disable sticky sessions

Sticky session for ALB
we can enable sticky sessions on ALB, but traffic will be sent to the target group (ec2 can be terminated and replaced as long as it is in the target group)

### Exam tips (deregistration delay/connection draining)
* enable deregistration delay: keep existing connections open if ec2 becomes unhealthy
* disable deregistration delay: if we want LB to immediately close connection to ec2 that are deregistring or unhealthy
