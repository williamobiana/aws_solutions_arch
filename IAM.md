## Securiing the Root Account to AWS
What is IAM\
Understanding IAM\
What is the Root Account

IAM (Identity Access Management)\
Allows to manage users and their level of access to the AWS console
* create users and grant permissions to those users
* create groups aand roles
* control access to resources

Understanding IAM
* Crutial for administrating a company's AWS account

What is the Root Account
* This is the email used to sign up for AWS
* It has full admin access to AWS, and 
* It MUST be secure.

### Exam Tips
4 steps to Secure Root Account:
* Enable MFA on root account
* Create an admin group for your admins and assign the approriate permissions to the group
* Create user accounts for your admins
* Add the admin users to the admin group


## Controling users actions with IAM policy documents
Permissions with IAM\
JSON example of policy documents\
How policy documents work

How do we control permissions using IAM?\
We assign permissions using policy documents, these policy documents are made up of JSON

Example of a policy document:
````
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
````

We can assign the policy document to:
* Groups
* Users
* Roles
Best practice is to assign the policy document to a group and add a user to the group (the user will inherit the permission policy from the group)

### Exam Tips
We assign permissions using IAM policy documents consisting of JSON


## Permanent IAM Credentials

Building blocks of IAM (Groups, User, Roles)\
Policies best practices\
Users and People best practices\
The principle of least privilage

Building blocks
* User = a person
* Group our users by diffrent job functions (admins, developers, etc)
* Roles are internal usages within AWS (it allows an AWS resource to access another AWS resource, eg ec2 accessing s3)

Recap: It is best practice for user to inherit permission policy from groups\
Note: you can assign to users but it become complex to manage as users may change in an organization.

Recap: 
* a user = a person
* We should share user accounts with multiple people (because we cannot track who made changes in the console)
* When creating a new user, by DEFAULT the user has no permissions or privilages
* Identity providers makes it easy to manage multiple AWS accounts and provide user with single-sign-on (SSO) to access their accounts from a single source. (eg: Active Directory Federation Services using SAML)

The principle of least privilage\
Only give the minimum amount of privilage/permission a user needs to do the job.

### Exam Tips
* IAM is universal and not tied to a region
* The root account is the account used to sign up for AWS, secure it with MFA as soon as possible and do not use it for day to day
* New users have no permission by default when first created
* Access key ID and secret access key ARE NOT the same as username and password, they are used as programatic access to AWS CLI
* You can only view it once, if you lose it, you have to regenerate it. (so download and save in a secure location)
* Always setup password rotations, you can create and customize your own password rotations policies.
* IAM Federation, you can use your existing user account credentials with AWS when you combine it with AWS (eg: Microsoft Active Directory with AWS, Jumpcloud with AWS)
* Identity federation Uses SAML standard, which is Active directory


## Exam Tips Recap
### Exam Tips
4 steps to Secure Root Account:
* Enable MFA on root account
* Create an admin group for your admins and assign the approriate permissions to the group
* Create user accounts for your admins
* Add the admin users to the admin group

### Exam Tips
We assign permissions using IAM policy documents consisting of JSON\
Example of a policy document:
````
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
````

### Exam Tips
* IAM is universal and not tied to a region
* The root account is the account used to sign up for AWS, secure it with MFA as soon as possible and do not use it for day to day
* New users have no permission by default when first created
* Access key ID and secret access key ARE NOT the same as username and password, they are used as programatic access to AWS CLI
* You can only view it once, if you lose it, you have to regenerate it. (so download and save in a secure location)
* Always setup password rotations, you can create and customize your own password rotations policies.
* IAM Federation, you can use your existing user account credentials with AWS when you combine it with AWS (eg: Microsoft Active Directory with AWS, Jumpcloud with AWS)
* Identity federation Uses SAML standard, which is Active directory

