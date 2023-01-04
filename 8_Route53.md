## Route53 Overview
what is DNS
IPv4 vs IPv6
Top level domains
Domain Registrars
Common DNS Record types
what is a TTL
Alias Records
Routing Policies

what is DNS
converting a human readable domain name to an IP address
Ip addresses are used by computers to identify each other on the network.
the address come in either IPv4 or IPv6

IPv4 is 32 bit address
IPv6 is 128 but address

Top level domains
in a domain, there is a string of characters seperated by dots "." (periods) eg: cloudguru.com, bbc.co.uk
* the last word in a domain name represents the top-level domain.
* the second word in a domain name is known as second-level domain name (this is optional).

top level domain names are controlled by the internet assigned numbers authority (IANA) in a root zone database, which is essentially a database of all avaliable top-level domains.

domain registrars
is an authority that can assign domain names directly under one or more top-level domains.
these domains are registered with interNIC, a service of ICANN, which enforces uniqueness of domain names across the internet

5 popular domain registras
* domain.com
* godaddy
* hoover
* AWS
* Namecheap

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

Alias Records
* Alias record are used to map AWS resources record sets in a hosted zone to load balancers, cloudfront or S3 that are configured as websites
* Alias records work like a CNAME record (because you can map 1 DNS name to another DNS name)
* CNAME cannot be used for naked domain names (zone apex record), you CANNOT have a CNAME with domain name http://websitename.com
* A record can have naked domain names (zone apex record), because they can map to the services

What is route53
* this is amazon DNS service
* it allows to register domain names
* manage and create DNS record

7 Routing policies avaliable with Route53
* simple routing
* weighted routing
* latency based routing
* failover routing
* geolocation routing
* geoproximity routing (Traffic Flow Only)
* Multivalue answer routing

### Exam tips
* alias record vs CNAME
* alias record can map DNS name to AWS resource
* CNAME cannot (it can only translate DNS for diffrent devices)
* if given a choice between alias and CNAME in a question, Alias is more correct
* common DNS record types (SOA, CNAME, NS, A)


## Registering a Domain name
we can buy domain name from AWS
it can take up to 3 days

## Using simple routing policy
we can only have 1 record with multiple ip addresses
if we specify multiple values in a record, Route53 returns all value to user in a random order.

## Using weighted routing policy
it allows us to split our traffic based on diffrent weights assigned
* spliting percentages of traffic to diffrent ec2: think weight routing policy

health checks
* we can set health checks on individual record set (incase 1 ec2 goes down, it redirect the traffic to the other ec2)
* if a record fails a health check, it will be removed from route53 until it passes the health check
* we can set SNS notification to alert us about failed health checks

## faiover routing policy
failover is used when we want to create active/passive setup
if primary ec2 fails health check, traffic is rerouted to secondary ec2


## geolocation routing policy
lets us choose which ec2 the traffic will be sent based on the location of the users (DNS queries)

use-cases:
queries from a region go to the closest ec2 in that region (the app running in the ec2 might be customized for language, currency, etc)

## geoproximity routing policy
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

## latency routing policy
allow routing of traffic based on lowest latency (delay) for the end user (which region will give them the fastest time)

* to use latency routing, we create a latency resource record set for the ec2 (or elb) in each region that hosts our website
* when Route53 recieves a query for the site, it selects the latency resource record set for the region that gives the user the lowest latency.

## Multivalue answer routing policy
lets us configure route53 to return mutiple values such as ip address for servers, in response to DNS queries

we can specify multiple values for any record, but also run health check of each resource, so route53 returns only values for healthy resources


## Exam Tips Recap
### Exam tips
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

## Registering a Domain name
we can buy domain name from AWS
it can take up to 3 days

## Health checks
* we can set health checks on individual record set (incase 1 ec2 goes down, it redirect the traffic to the other ec2)
* if a record fails a health check, it will be removed from route53 until it passes the health check
* we can set SNS notification to alert us about failed health checks

## Using simple routing policy
we can only have 1 record with multiple ip addresses
if we specify multiple values in a record, Route53 returns all value to user in a random order.

## Using weighted routing policy
it allows us to split our traffic based on diffrent weights assigned
* spliting percentages of traffic to diffrent ec2: think weight routing policy

## faiover routing policy
failover is used when we want to create active/passive setup
if primary ec2 fails health check, traffic is rerouted to secondary ec2

## geolocation routing policy
lets us choose which ec2 the traffic will be sent based on the location of the users (DNS queries)

use-cases:
queries from a region go to the closest ec2 in that region (the app running in the ec2 might be customized for language, currency, etc)

## geoproximity routing policy
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

## latency routing policy
allow routing of traffic based on lowest latency (delay) for the end user (which region will give them the fastest time)
* to use latency routing, we create a latency resource record set for the ec2 (or elb) in each region that hosts our website
* when Route53 recieves a query for the site, it selects the latency resource record set for the region that gives the user the lowest latency.

## Multivalue answer routing policy
* lets us configure route53 to return mutiple values such as ip address for servers, in response to DNS queries
* we can specify multiple values for any record, but also run health check of each resource, so route53 returns only values for healthy resources
