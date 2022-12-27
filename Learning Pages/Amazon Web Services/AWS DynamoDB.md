# Introduction

What is DynamoDB? - It's basically a fully managed NoSQL database that's been provided by AWS.
- Standard database — Oracle and Microsoft SQL
- NoSQL — MongoDB, CouchDB. Key aspect is that you don’t need to have a structure predefined for the tables in the database.

**Database that AWS provide and support:**
1. AWS RDS — PostgreSQL, MySQL, MariaDB, Oracle and
Microsoft SOL Server.
2. AWS DynamoDB — Fully managed NoSQL Database

### Aspects of DynamoDB
- It is a full managed service. In the AWS RDS(Relational Database Service) - e.g. Oracle, you need to use the service provide by AWS to provision the RDS or install one yourself on an EC2 instance. But in the case of DynamoDB, you can just start to directly create tables.
- You don't need to worry about patching or scalability. Everything is managed by AWS. Reduces the administrative burden on the organization. So if the table grows, the space is automatically added by AWS.
- Also, the data is replicated across 3 facilities across regions to ensure high availability and durability of data.
---
# Tables and Items

- An item is a collection of attributes. So items are like rows in a traditional database.
- An attribute is a combination of a name and a value. This is somewhat like the column of a traditional database.

| Id | Name |
|----|------|
| 1  | Mark | 
| 2  | Ted  |
- Attributes - (Id, Name)
- Items - (`[1, "Mark"]`, `[2, "Ted"]`).

### Partition and Sort Keys

| Id | Name  | Designation       |
|----|-------|-------------------|
| 1  | Mark  | Software Engineer |
| 2  | Ted   | Manager           |
| 3  | Smith | Senior Manager    |
| 4  | Zoe   | Senior Manager    |

1. Id is the partition key - this should be unique.
2. Name is the sort key - sorting done for each partition key.

![[partition_keys_best_practice.png]]

---
# Provisioned throughput for DynamoDB

>[!Note:]
>Throughput means input/output.

Provision throughput is basically the amount of read and write capacity that you provide to your DynamoDB tables. The input and output is an important factor

1. Read capacity - Number of 4 KB reads per second. There is a charge for the read capacity.
	Let's say you provide a read capacity of 5. That means you have a total read capacity of ( 4 KB * 5) = 20 KB reads per second.

If you have items of more size being read per second, then they will be queued and have an effect on your application. So you need to decide on the capacity based on your application requirements.

### Read Capacity Consistency
1. Eventual consistency - This is where AWS guarantees that when a write operation occurs on an item, the data across multiple copies will eventually be consistent. This means that subsequent read after a write can return stale data.
2. Strong consistency - Here, AWS guarantees that the read will always return consistent data.
Strong consistency reads are more expensive than eventual consistency reads. On string consistent reads is equal to two eventual consistent reads.

We know that dynamoDB replicates the data across multiple locations to ensure the data's durability, availability, and scalability. So when you perform a write operation, it takes time for the data to be synchronized across all locations. And so when your application performs a write operation, and then it performs a read operation right away. There is no guarantee that you will get the most recent data, your application could still get stale data. But it just takes a fraction of a second for it to become consistent.
In the Strong consistence capacity, however, this is where the read operation will always have consistent data. Because, after a write operation, AWS makes sure that the data is replicated to all locations before the data is returned to the application for a read operation.

>[!Note:]
> Consistence reads are more expensive than eventual read. One Strong Consistence read is equal to two Eventual consistence reads.


**Example:**
If you have a requirement where each read is 6 KB and you need 100 strong consistent reads.

First, you need to take 6 KB to the nearest 4 KB = 2

Then since you need 100 strong consistent reads then read capacity = 100 * 2 = 200 reads capacity.

If you then need Eventual consistent reads, then you just need to divide the number by 2 = 100. Hence, you just need to provision 100 as the read capacity.



### Write Capacity
Is the number of 1 KB writes per second. There is a charge for the Write capacity.

Let's say you provide a write capacity of 5. That means you have a total read capacity of 1 KB * 5 = 5 KB writes per sec.

So you need to decide the capacity based on your application requirements.