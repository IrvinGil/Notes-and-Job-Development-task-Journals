# Regions and AZ's
- Region (e.i. Europe, Asia, US) is a separate geographic area
- Each region has multiple, isolate locations known as **Availability Zones**
- Each Availability zone is isolated, but the availabilty zones in a region are connected through low-latency links
- Each AZ is represented by a name.

![[region_and_az.png]]
> Illustration of how regions and AZ is setup

Each region has multiple isolated locations known as Availability zones (AZ), where each AZ is isolated but each zone in a region or connected through low latency links.  So in order to have fault tolerant applications, you can host resources in multiple AZ's so that if one AZ goes down you can be ensured that the other one will be available and your application won't be affected.


## VPC
VPC - Amazon Virtual Private Cloud
- Logically isolated section of the Amazon Web Services (AWS) cloud.
- Host your resources such as your virtual servers.
- Collection of subnets.
- Each subnet would then host your resources.
![[vpc_illustration.png]]
### Extra bits
- Each region has its own separate charge for AWS resources.
- Each region has separate services.
- You can’t change the size of the VPC after it is created.
- There is no charge for the VPC itself. You only get charged for the underlying resources in the VPC.
- You get a default VPC as part of your account.
- Each subnet corresponsds to one AZ

## Default VPC
- Gets created when the account is created. It has a CIDR block of 172.31.0.0/16.
- There is a default subnet within the VPC.
- You can start launching instances.
- It has an internet gateway.
- Pre-built NACL, Security Groups and DHCP options. 

One thing to note on VPCs is when the DNS resolution and DNS hostnames: when it is enabled(yes) it means that all EC2 instances launched in the VPC will automatically get a DNS host name given by AWS.
Subnets in the VPC hosts EC2 instances.

---
# EC2 Instances
EC2 - Elastic Cloud Compute

Web service that provides capacity on the cloud.
You can spin up Linux, Windows instances.

**Steps while configuring an EC2 Instance (Elastic Cloud Compute)**
1.  Choose an Amazon Machine Image (template to be used to create the instance that you want to create)
	- Linux, Windows
	- EBS (Elastic Block Storage) Backed AMI or an Instance Store AMI (Amazon Machine Images)
		
		**EBS Backed v/s Instance Store**
		- Instance Store —The storage is part of the physical server. So
		there is less latency since it is physically attached.
		- EBS Backed — Network attached storage.
		- Instance Store — Temporary storage so when the instance is stopped
		or terminated , the data is deleted.

> Latency -  the time taken for data to transfer from source to destination.
2. When you have choosen your desired AMI; you must specify the instance type.
	Different kinds of instance types:
	1. T2 - Burstable perforrmance
	2. M3, M4 - Good balance of compute, memory, and network resources.
	3. C3, C4 - Compute optimized.
	4. G2 - Graphic intensive applications.
3. Specify the subnet.
4. Enable public ip allocation (optional)
5. Then add the storage
6. Add tags.
7. Add the security groups
8. Craete or choose an existing key pair.

>[!Note:]
> - A keypair is essentially used to log in to your instances.
> - A Security group is used to control the traffic which flows in and out of your instance.
> - 

When connecting to the created instance, you would have to download a file provided by the aws to be able to connect the the EC2 instance and then configure the password to the instance using the the keypair that you have (either generated or created, depends on the choice you made during the creation of the instance).

---

