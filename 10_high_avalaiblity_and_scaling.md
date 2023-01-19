## Horizontal vs Vertical Scaling Overview
vertical scaling
horizontal scaling
3 W's of scaling

what do we scale?
how do we define the template?

where does it make sense to scale? (where in the vpc?, how many AZ? what loadbalance do we use? should we scale out database or webservers?)

when do we scale?
how do we know we need more? 

### Exam tips
anytime we see a scaling, keep the 3 W's in mind
* what do we scale
* where do we scale
* when do we scale

## What are launch templates/configurations
What are we scaling?
Templates vs configuration

WHAT: What are we scaling with?
What is a launch template: specifies all the needed settings for building an ec2 (it is a collection of settings we have configured)
Templates:
* are more than autoscaling
* supports versioning
* more granularity
* AWS recommended

Configurations:
* only for autoscaling
* immutable
* limit config options
* not typically used

### Exam tips
What makes a template
* all the basics (ami, instance type, security groups,)
* templates include vpc networking, configs do not.
* Note: its not good practice to include a vpc networking, because we wont be able to use auto-scaling groups.
* launch templates are flexible and best practice 
* launch configs are older
* user data is included in the template for bootstraping
* we can modify launch templates, and version them. (config are immutable and cant be modified)

## Scaling ec2 with Autoscaling
Autoscaling groups
autoscaling steps
setting capacity limit

WHERE: Where does it make sense to scale?
Auto scaling is a collection of ec2 treated as a group for scaling and management

Autoscaling steps
* define the template: we have to pick from our avaliable launch template 
* networking and purchaing: pick our networking space and purchasing options (using multiple AZ allows for high availaibility)
* ELB configuration: ec2 can be registered behind the LB, autoscaling group respect the LB health checks
* set scaling policies: min, max and desired capacity needs to be set to ensure we have the right amount of resources
* notifications: SNS allows know when a scaling occurs

setting capacity limits (auto scaling restrictions to stop us from scaling too much)
* minimum: lowest num of ec2
* maximum: highest num of ec2
* desired: how many ec2 i want right now

### Exam tips
* autoscaling is vital for creating a highly avaliable application
* remember to select answers that spread resources out over multiple avaliability zones and use load balancers
* networking: ASG will contain the location were the ec2 will be deployed
* ELB: its vital to select LB for the ec2
* Limits: min, max and desired are 3 important settings
* notifications: SNS allows know when a scaling occurs
* balancing: auto scaling will balance the ec2 across AZs

## Autoscaling polices
step scaling
instance warm-up and cooldown
scaling types

WHEN: When do we scale?
scaling out: add instances when memory is a certain percentage range
scaling in: remove instances when memory is at a certain percentage range

we use scaling policies to define the scale in and scale out parameters

* instance warm-up: is a window of time when the ec2 instances are installing the applications and dependencies before attaching it to the loadbalancers during a scaling out. (else, it will fail healthcheck and be terminated)
* cooldown: pauses autoscaling for set amount of time to accomodate spikes in scaling events 
* avioding trashing: to create ec2 quickly and spin down slowly.

scaling types
reactive scaling: responding to workload and determine if we need to create more resources
scheduled scaling: great for predictable workload, getting resources ready for a scaling event.
predictive scaling: uses ML to determine when to scale, reevaluaed every 24hrs to create a forecast for the next 48hrs

Steady state ASG: typically used for legacy apps on ec2 where (min, max, desired is 1) on failure ec2 deploys in a new AZ from list og the selected AZ for the vpc

## Exam tips
cost implications of Autoscaling for (min, max and desired capacity)
reasons why we might not want to change the numbers

using autoscaling policies:
* scaleout aggresively 
* scalein conservatively
* provisioning (taking advantage of customised AMI)
* costs (using reserve ec2/spot to cover minimum count)
* cloudwatch (alerts for autoscale)

## scaling relational databases
4 ways to scale

types of scaling to adjust our RDS performance
* vertical scaling
* scaling storage (its only able to go up, but not down)
* read replicas (create read only replicas to spread out workload)
* aurora serverless (we can offload scaling to AWS with unpredictable workloads)

### Exam tips
scaling vs refactoring 
* refactoring and changing to dynamoDB is a viable scaling choice
* it wont work that easily in the real world, but in the exam, switching database types can solve the problem

relational database scaling
* read replicas: read-heavy workload
* careful with storage: RDS storage only scales up (it wont scale back down)
* vertical scaling
* Multi-AZ: unless its a dev environment, it should always be turned on
* Aurora everything: use aurora if the suituation calls for a RDS

## scaling non-relational databases
scaling options

dynamoDB is simplified 
provisioned
* use case: generally predictable workload
* effort to use: need to review past usages to set upper and lower scaling bounds
* cost: most cost-effective model

on-demand
* use case: sporadic workload
* effort to use: select on demand
* cost: pay small amount of money per read and write. Less cost effective

### Exam tips
keep cost in mind rather than performance when scaling dynamoDB
* predictable workload: Pick provisioned capacity
* sporadic(unpredictable): Pick on-demand

non-relational database scaling
* access patterns: know if its predictable or unpredictable
* design matters: avoiding hot keys will also lead to better performance
* switching: we can switch, but only once per 24hours per table
* cost: keep cost in mind

## Exam tips recap
### questions to ask
* is it highly avalaible?
* should it horizontal or vertial: horizontal is most approrate
* is it cost effective
* would switching database fix the problem

### what to know about autoscaling
* Autoscaling is only for ec2: no other service can be scaled using auto scaling. other services might have a built-in option, but they arent included in ASG.
* Get ahead of the workload: whenever possible, favour solutions that are predictive rather than reactive.
* Bake AMI to reduce build times: we can avoid long provisioning time by putting everything into an AMI. This is better than using user data whenever possible.
* Spread out: Spread out ASG over Multiple-AZs (at least 2)
* Steady state groups: allow us to create a suituation where failure of a legacy app that cant be scaled, can automatically recover from failure.
* ELBs are essential: ensure health checks are enabled, else instances wont be terminated and replaced when they fail health checks

### scaling databases
* RDS has the most database scaling options
* Horizontal scaling is usually preferred over vertical
* Read replicas are your friend
* DynamoDB scaling comes down to access patterns

