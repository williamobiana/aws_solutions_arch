## VPC overview
VPC introduction
Networking
VPNs
Network diagram
VPC features
Comparing VPCs

VPC is a virtual datacenter in the cloud
it is a logically isolated part of the AWS cloud where you can define our own network
we will have complete control of the virtual network, including our own IP address range, subnets, route tables and network gateways

Network
We will have a fully customizable Network
we can leverage multiple layers of security, including SGs and Network ACL, to help control access to ec2 in each subnet
typically we have a 3 tier architecture
* web (public subnet)
* app (private subnet)
* database (private subnet)

VPNs
we can create a hardware virtual private network (VPN) connection between our datacenter and the VPC and leverage the cloud resources as an extention of our datacenter

To check out cidr ranges and avaliabe IPs based on the network mask, go to cidr.xyz

Network diagram
when creating the vpc, we can choose an ip address cidr range
10.0.0.0 - 10.255.255.255 (big corporations use this range)
172.16.0.0 - 172.31.255.255 
192.168.0.0 - 192.168.255.255 (home networks use this range)

we can use /16 network mask since it gives us the biggest number of avaliable ip addresses

when we create the vpc, the following will be auto-created
* security group
* network acl
* route table/router

we manually create:
subnets (we can use /24 netmask since we will not need so many ip addresses)
* internet gateway (igw)
* public route table


if we need to access the private subnet from our datacenter, 
* virtual private gateway connected to router

vpc features
* launch instance
* create custom IP address ranges
* configure route tables
* attach igw
* more control over our AWS resources
* provision network access control list (we can block specific ip address with it)

comparing VPCs
default vs custom
default:
* user friendly
* all subnets in default VPC have a route out to the internet
* each ec2 has a public and private ip address
custom:
* fully customizable
* takes time to set up

### Exam tips
* a vpc is a logical datacenter in AWS
* consists of igw/vgw, route tables, nACLs, subnets, security groups
* 1 subnet is always always in 1 AZ


## Steps to setting up a VPC network

when we create the vpc, the following will be auto-created
* default security group
* default network acl
* main route table/router

we manually create:
public subnet for web (we can use /24 netmask since we will not need so many ip addresses)
private subnet for app (we can use /24 netmask since we will not need so many ip addresses)
* internet gateway (igw) to access the internet
* public route table for explicit connection to the igw


* we can enable auto-assign IPv4 address to public subnet (so any ec2 added in the subnet will automatically recieve an eip)
* to make the public subnet accessable to/from the internet we attach the igw to the VPC
* since we have added an igw to the vpc, the vpc route table has become public and the all subnets in this route table are now public by default. (this is fine for the public subnet, but a problem for the private subnet)
* best practice is to create alternate route tables for the public and private subnet.
* the public route table we will define the route for the subnet to point to the igw, and associate the public subnet to it.
* create an ec2 and assign it to the public subnet

* create a private security group with inbound rules of your choice (ICMPv4 for pinging an ec2)
* create an ec2 and assign it to the private subnet, use the private security group
* test a ping

## Using NAT gateways for internet access
what is NAT gateway
Nat gateway tips

Network address translation (NAT) gateway are used by ec2 in private subnet to connect to the internet, while preventing any address from the internet from communicating with it.

we provision the NAT gateway in the public subnet

5 facts about NAT Gateway
* redundant inside the AZ
* starts at 5 Gbps and scales currently to 45 Gbps
* no need to patch 
* not associated with security groups
* they are automatically assigned a public Ip

### Exam tips
* redundant inside the AZ
* starts at 5 Gbps and scales currently to 45 Gbps
* no need to patch 
* not associated with security groups
* they are automatically assigned a public Ip


## Protecting your recource with security groups
Review Security groups
what are security groups

How comupters communicate
* Linux: ssh port 22
* Windows: rdp port 3389
* http: web port 80
* https: encrypted web port 443

security groups
* security groups are virtual firewalls for our ec2
* by default everything is blocked
* to allow every ip address to have access 0.0.0.0/0,
* to communicate with our ec2 we open the approriate port for ssh/rdp/http/https

Troubleshooting internet connectivity in order:
* start with route table
* network acl
* security group


security groups are STATEFUL:
* if you open a port in inbound rules, that same port will be open outbound rules (even if you explicitly block it in the outbound rules)


## Controlling subnet traffic with Network ACLs
what is a Network ACL
Network ACL overview
Network ACL tips

Network ACl is an optional layer of security for VPC that acts as a firewall for controlling traffic in and out of one or more subnets
we can set up network ACL with rules similar to our security groups

overview
* default nACL: by default, a VPC comes with a network ACL and it allows all inbound and outbound traffic
* custom nACL: custom nACL by default denies all inbound and outbound traffic until we add rules
* subnet associations: each subnet in VPC must be associated with network ACL, if we dont explicitly associate a subnet with network ACL, the subnet is automatically associated with the default network ACL.
* blocking IP address: we can block ip addresses using network ACL NOT security groups

tips
* we can associate network ACL with multiple subnets, but subnets can associate with only 1 Network ACL, when we associate a subnet with a nACL, the previous association is removed
* nACL contain a numbered list of rules that are evaluated in order, starting with the lowest number rule
* nACL have seperate inbound and outbound rules and each rule can either allow or deny traffic
* network ACLs are STATELESS (rules for inbound traffic can be influenced by rules for outbound traffic and vice versa)


## Controlling subnet traffic with Network ACL
Amazon recommends rule number is increments of 100s and a wildcard(*) to end in deny
same rules should be defined in inbound and outbound rules
emphereal ports can be added to outbound rules to accomodate clients with diffent OS (Linux, windows, mac) from 1024 - 65535

### Exam tips
* default nACL: by default, a VPC comes with a network ACL and it allows all inbound and outbound traffic
* custom nACL: custom nACL by default denies all inbound and outbound traffic until we add rules
* subnet associations: each subnet in VPC must be associated with network ACL, if we dont explicitly associate a subnet with network ACL, the subnet is automatically associated with the default network ACL.
* blocking IP address: we can block ip addresses using network ACL NOT security groups

tips
* we can associate network ACL with multiple subnets, but subnets can associate with only 1 Network ACL, when we associate a subnet with a nACL, the previous association is removed
* nACL contain a numbered list of rules that are evaluated in order, starting with the lowest number rule
* nACL have seperate inbound and outbound rules and each rule can either allow or deny traffic
* network ACLs are STATELESS (rules for inbound traffic can be influenced by rules for outbound traffic and vice versa)


## Private communications using VPC endpoints
what are vpc endpoints
vpc endpoint types

vpc endpoints enable us to privately connect to our vpc to AWS services. it is powered by privatelink without requiring igw
nat, vpn or AWS direct connect.

instances in our vpc do not require ip addresses to communicate with resources in the services (internal communication btw VPC and AWS services)

* endpoints are virtual devices
* they are scaled horizontaly
* they are redundant, highly avaliable vpc componets that allow communication between instances in vpc and services without imposing avalaibility risks or bandwidth constraints on our network traffic

vpc endpoint types
* interface endpoints: elastic network interface with private ip address, serving as an entry point for traffic headed to a supported service. They support a large number of AWS services.
* gateway endponts: gateway endpoint is virtual device we provision (similar to nat gateway). it supports connection to s3 and dynamoDB

### Exam tips
use case: when we want to connect to AWS services without leaving the amazon internal network
2 types of vpc endopint: interface and gateway
gateway endpoints: supports s3 and dynamoDB


## Building solutions across VPCs with peering
Multiple vpc
vpc peering
transitive peering

multiple vpc
if we need several vpc for diffrent environments, it may be necessary to connect the vpc to each other
* production web vpc
* content vpc
* intranet

vpc peering
* allows us to connect 1 vpc with another via a direct network route using private IP addresses
* instances behave as if they were on the same private network
* we can peer vpcs with other aws accounts as well as with other vpcs in the same account
* peering is in a star config (1 central vpc peering with other vpcs). No transitive peering
* you can peer between regions

### Exam tips
vpc peering
* allows us to connect 1 vpc with another via a direct network route using private IP addresses
* transitive peering is not supported (there must be 1 central vpc connecting others)
* we can peer between regions
* no overlapping CIDR address ranges


## Network Privacy with AWS private link
opening up our services in a vpc to another vpc
sharing applications across vpcs
using private link

to open our apps to other vpcs we can either
* open the vpc to the internet: (there will be security considerations, there will be a lot more to manage)
* use vpc peering: (we have to create and manage many different peering relationships, the entire network will be accessable)

using AWS private link
* the best way to expose a service vpc to customer vpc
* doesnt require vpc peering, no route tables, no nat gateways, no internet gateways etc
* requires a network load balancer on the service vpc and eni(elastic network interface) on the customer vpc

### Exam tips
* peering vpc to many customer vpcs: think privatelink
* doesnt require vpc peering, no route tables, no nat gateways, no internet gateways etc
* requires a network load balancer on the service vpc and eni(elastic network interface) on the customer vpc


## Securing your network with vpn cloudhub
vpn cloud hub

if we have multiple websites with its own VPN connection, we can use AWS vpn Cloudhub to connect those sites together
* hub and spoke model (there must be 1 central vpc connect to others)
* low cost and easy to manage
* operates over the public internet, but all traffic between customer gateway and AWS VPN cloudhub is encrypted

### Exam tip
* if we have multiple websites with its own VPN connection, we can use AWS vpn Cloudhub to connect those sites together using a hub and spoke model
* low cost and easy to manage
* operates over the public internet, but all traffic between customer gateway and AWS VPN cloudhub is encrypted


## Connecting On-premises with direct connect
direct connect
private connectivity
types of connection
vpn vs Direct connect

direct connect is a cloud service solution that makes it easy to establish a dedicated network connection from on-prem to AWS

private connectivity
* using direct connect, we can establish private connectivity between aws and our datacenter
* in many cases, this can reduce our network costs, increase bandwidth throughtput, and provide a more consistent network experience than internet based connections

2 types of direct conncetion
dedicated connection: physical ethernet connection associated with a single customer 
hosted connection: physical ethernet connection that an AWS direct connect partner provisions on behalf of a customer. 

vpn vs direct
vpn: allows private communication, uses public internet, secure, slow
direct: fast, secure, relaible, able to take massive throughput

### Exam tip
* direct connect directly connects our datacenter to AWS
* useful for high-throughput workloads
* helpful when we need a stable and reliable secure connection


## Simplifying networks with transit gateway
transit gateway
transit gateway facts

connets vps and on-prem networks using a central hub
simplifies our network by acting as a cloud router

facts
* allows us to have transitive peering between many vpcs and on-prem datacenters
* works as a hub and spoke model
* works on a regional basis, but can be multi region
* can be used accross multiple AWS accounts using resource access manager

### Exam tips
* we can use route tables to limit how vpc communicate with each other
* works with direct connection as well as vpn connections
* supports IP multicast (not supported by any other AWS services)


## 5G Networking with AWS wavelenght
5G network
AWS wavelenght

5g provides devices with higher speed, lower latency and greater capacity than 4g

AWS wavelenght embeds compute and storage services in 5g, providing mobile edge computing infrastructure for deploying, and scaling ultra low latency apps

### Exam tips
* scenrio questions about mobile computing: think AWS wavelenght


## Exam Tips Recap
### Exam tips1 (vpc overview)
* a vpc is a logical datacenter in AWS
* consists of igw/vgw, route tables, nACLs, subnets, security groups
* 1 subnet is always always in 1 AZ

### Exam tips2 (Nat gateways)
* redundant inside the AZ
* starts at 5 Gbps and scales currently to 45 Gbps
* no need to patch 
* not associated with security groups
* they are automatically assigned a public Ip

High Avaliability with NAT gateways
if we have resources in multiple AZ and they share a NAT gateway, in the event the NAT gateway's avaliablity zone is down, resources in the other AZs lose internet access

to create an AZ independent architecture, create a NAT gateway in each AZ and configure routing to ensure resources use the NAT gateway in the same AZ

### Exam tips (Security group)
Security groups are STATEFUL:
* if you open a port in inbound rules, that same port will be open outbound rules (even if you explicitly block it in the outbound rules)

### Exam tips (Network ACL)
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

### Exam tip (direct connect)
* direct connect directly connects our datacenter to AWS
* useful for high-throughput workloads
* helpful when we need a stable and reliable secure connection

### Exam tips (endpoints)
use case: when we want to connect to AWS services without leaving the amazon internal network
2 types of vpc endopint: interface and gateway
gateway endpoints: supports s3 and dynamoDB

### Exam tips (vpc peering)
* allows us to connect 1 vpc with another via a direct network route using private IP addresses
* instances behave as if they were on the same private network
* we can peer vpcs with other aws accounts as well as with other vpcs in the same account
* peering is in a star config (1 central vpc peering with other vpcs). No transitive peering
* you can peer between regions

### Exam tips (privatelink)
* peering vpc to many customer vpcs: think privatelink
* doesnt require vpc peering, no route tables, no nat gateways, no internet gateways etc
* requires a network load balancer on the service vpc and eni(elastic network interface) on the customer vpc

### Exam tips (transit gateway)
* we can use route tables to limit how vpc communicate with each other
* works with direct connection as well as vpn connections
* supports IP multicast (not supported by any other AWS services)

### Exam tip (VPN hub)
* if we have multiple websites with its own VPN connection, we can use AWS vpn Cloudhub to connect those sites together using a hub and spoke model
* low cost and easy to manage
* operates over the public internet, but all traffic between customer gateway and AWS VPN cloudhub is encrypted

### Exam tips (mobile edge computing)
* scenrio questions about mobile computing: think AWS wavelenght
