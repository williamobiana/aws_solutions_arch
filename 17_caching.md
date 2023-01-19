## Caching Overview
what is caching
where to cache
caching solutions

caching is short term memory storage

where to cache
external: in front of the user
internally: in front of the database

Caching solution
CloudFront (external)
Elastic cache (internal)
DAX (caching for dynanamoDB)
Global accelerator

### Exam tips
* Pick solution that include caching
* Caching improves speed
* Caching solution (External or Internal)


## CloudFront
CloudFront is a CDN content delivery network, it takes stattic content and deliver them to customer globally

Settings
Security: Defaults to HTTPS connections, with ability to add custom SSL certificate
Global Distribution
Endpoint supoort: can be used for AWS and Non-AWS service endpoint
Expiring content: you force an expiration of content from the cache if you can't wait for TTL

### Exam tips 
* all external customer performance issue can be solved with caching
* it is speedy
* can block connection from a country
* on-site support
* works in all location


## Elastic Cache and DAX
Elastic cache is an AWS managed service of 2 open source technologies (memcache and Redis)

memcache vs redis (sits infront of RDS)
memcache
* simple caching
* not a DB
* no failover or multi-AZ
* no backups

Redis
* supports caching
* functions as a NoSQL DB
* has failover and multi-AZ support
* supports backups

DAX
* in memory-cache solution for dynamoDB
* lives in a VPC and can be highly avaliable
* we have full control on DAX

picking a cache
DAX
* specific to dynamoDB

elasticCache
* more flexibility and used with RDS

### Exam tips
* pick answers that include database caching
* use DAX for dynamodb
* use Elastic Cache for RDS
* Elastic cache as (memcache & redis)
* redis can susbsite for a NoSQL DB, dynamoDB is not avaliable


## Fixing IP cache with Global Accelerator
what is global accelerator
3 top feature

Global accelerator is a networking service in front of our loadbalancer for our apps, it solves the problem of IP caching

3 top features
* mask complex architecture
* speeds up traffic
* define weighted pools

### Exam tips
* Scenrio about IP caching: think global accelerator
* it makes connection faster
* we can set up weights
* you are provided 2 static IPs that dont change, and we can bring our own


## Exam tips recap
4 questions to ask
* can it be cached? (is there a situation in the scenerio to cache the data, speed up performance)
* what kind of cache do i need? (caching a database or customer facing)
* how does the content in cache get updated 
* does the cache add benefits from speed (security, IP cacheing)

CloudFront
* CloudFront is the only option to add HTTPS to a static website being hosted in an S3 bucket
* Always favor answers that include caching. 
* Global accelerator is solution for IP caching

Database caches
* in-memory database has 2 potential solutons, DynamoDB and redis
* Redis has more features than memcache. Redis can be used as a cache and Database, memcache is just a cache
* Backups are not supoorted on any solution except Redis








