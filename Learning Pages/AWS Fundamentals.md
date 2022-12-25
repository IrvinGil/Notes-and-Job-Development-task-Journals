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

> For further reading about AWS EC2, this is the link to the [official documentation](https://docs.aws.amazon.com/pdfs/AWSEC2/latest/UserGuide/ec2-ug.pdf#elastic-ip-addresses-eip).
---

# Elastic IP

> Part of EC2 section.

- Static IP address
- Each instance launched in a subnet has a public IP if configured to do so. This is used to talk to the internet.
- It also has a private IP address to talk with instances in the subnet and accross subnets.
- When the instance is stopped and started, the public IP changes.

### Use cases
1.  Example.com —Web server — 54.12.1.10. If the IP address changes
then you need to make constant DNS changes.
2.  Domain Controller or Exchange servers, so that all clients
don’t need to change their settings to contact these servers.
3. Recovery - So if one instance fials, you can assign the elastic IP to another instance.
---
# NACL - Network Access Control List

- Acts like a Firewall to control access to and from the subnet.
- If you get abnormal traffic from a set of IP instances , you can
block traffic from those IP Instances.
- Subnet comes with a default Network ACL. This allows all incoming
and outgoing traffic.
- You have/can create custom network ACL's
- Stateless — If traffic is allowed inbound, not necessary the same
will be allowed outbound.

NACL Rules are defined at a subnet level.
![[nacl_rules_illustration.png]]
*NACL rules illustration. (above)*

> [!Note:]
> **Inbound** refers to incomming traffic to your instance while the **outbound** refers to the outgoing traffic.

![[nacl_rules_example.png]]
*NACL rules examples. (above)*

### Things to note on NACL rules
1. If NACL rule has been found by AWS to be matched then all other rules that follows are discarded. (meaning if you refer to the image above, the `rule 100` of table 1 will be the first to be matched and then the * rule will not be read.)
2. Rules are also checked by aws based on their numbering/name. From least -> greatest value.
3. If you look at the 2nd table on the image above, the `rule 102` will not be read as any traffic from any source will be matched on the * rule.

To navigate to NACL : Navigate to ***Network and content delivery*** on the AWS console -> click on VPc and go to VPC dashboard -> navigate to **subnets** -> click on any of the subnets and you can see the **NACL** section.

![[nacl_on_aws_management_console.png]]

To change the NACL, navigate down the sidemenu on the left and then find **Network ACLs**. From there you can make modification on your current and existing network ACLs.

---
# Security Groups

- Is the next level of security, which is a firewall to control access to and from your instances. One thing to note is that **NACLs** are for your subnets and **Security Groups** are for your Instances.
- For Example: Web server - Only allow traffic to port 8o and 443. Or 3389 for RDP. Port 22 for SSH. Here you can define what ports and whats IP can be allowed access to your instances.
- Database server - Only allow traffic to port 3306 if you are hosting MySQL instances.\
- Security Groups are defined at the EC2 layer.

![[security_group_illustration.png]]

Security groups are stateful — if you send a request from your instance,
the response traffic for that request is allowed to flow in regardless of inbound security group rule. In Network ACL you have to look at the inbound and the outbound rules. But for the security groups, if you alow the inbound then the outbound will be automatically allowed. 

You can specify separate rules for inbound and outbound traffic

When you create a security group, it has no inbound rules.

By default, a security group includes an outbound rule that allows all onbound traffic.

To go to security groups on AWS, go to the EC2 dashboard and then find "Security groups" under **Network and Security** on the left sidebar menu. You can find all security groups defined in your VPC there.  

---
# Elastic Block Storage (EBS) Volumes

- this a way to add additonal volumes on you instances. (Storage for EC2 instances)
- Replicated by AWS within an AZ to provide high availability.
- Scale up and down required.
- Create point in time snapshots of the volume.
- You can keep the volume and attach it to other instances.
- EBS root volume - Set the Delete on termination flag to zero.
- You can also encrypt EBS volumes. Can be done during the creation of the volume.

### Types of EBS Volumes
**SSD Backed Storage** - used in Transactional Workloads - Depends on input/output (IOPS) - 
- General Purpose SSD (gp2) and 
- Provisioned IOPS SSD (iop1)

**HDD Backed Storage** - This is used basically for big data workloads. Primarily on throughput measured in MB/s.
- Optimized HDD (st1) and 
- Cold HDD (sc1)

To work with Elasti block storage,  go to the EC2 dashboard and navigate to "Volumes" under **Elastic Block store** on the left sidebar menu of the AWS management console.

![[ebs_on_aws_mangement_console.png]]

You can attach the volume to an instance by clicking on the volume and the "Actions" option. Note that you can attach a volume to a running instance without having to stop and restart the instance.

You can also change the size of your EBS volumes.

You can also create a snapshot of your EBS volumes. EBS snapshots are a point-in-time copy of the volume. This way you can copy the snapshots of your volumes to other regions to make them available for other instances. If the volume you choose is encrypted, then the snapshot of that volume will also be encypted and the same the other case. 



