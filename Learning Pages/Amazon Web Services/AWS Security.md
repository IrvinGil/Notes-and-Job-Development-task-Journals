# Shared Responsibility Model

What is pertinent to AWS to take care off. And what does the customer take care off.
![[aws_shared_responsibility_model.png]]

We can see that AWS is responsible for the underlying hardware that provides your computer storage, database and networking services. And it is also responsible for ensuring the uptime of its regions and availability zones and edge locations.

On the customer's side, the customer is responsible for their customer data, platform applications and the identity and access management. And so the customer needs to ensure to create the right users, groups, roles, permissions in order to ensure security of their AWS infrastructure.

| Responsibility of AWS                                                           | Responsibility of the Customer              |
|---------------------------------------------------------------------------------|---------------------------------------------|
| Decommissioning of old storage devices                                          | Encryption of data at rest                  |
| Physical Security to the data centers                                           | Patching of the underlying Operating System |
| Taking care of the underlying physical hardware that hosts the virtual machines | Security Groups for EC2 Instances           |

Penetration Testing - This is the testing of a system, network or web application to find vulnerabilities that an attacker could exploit.
You need to get prior request from AWS before conducting a penetration test.

---
# AWS IAM

Identity and Access Management (IAM):

- You can log in via the console through a username and password.
- You can use secret keys to access services via the CLI or SDK (Software development kit).
- Services can use Roles, and this is the preferred method. e.g - EC2 instance which wants to access an S3 bucket.

**User** - A person or service that is used to interact with AWS.
**Groups** - A collection of users.
**Roles** - A set of permission assigned to a group of users. e.g - Operations Team
**Temporary Access Credentials** - This is when an application requires a resource for a brief period of time.
**The root user** - Complete access to all services, created when the account is created.

![[how_aws_accesses_access.png]]
*Flow of how AWS assesses access to services and resources. (above)*

### Creating IAM users
1. Click on your profile and click on security credentials. Navigate to the "users" section on the left sidebar menu. And click the "Add user" button.
2. 
