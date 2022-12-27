# Introduction

- Object storage (Key, Value) — reliable , scalable , low latency
- S3 provide a web interface that can work with storage.
- You can store data of virtually any format.
- Virtually unlimited amount of storage
- S3 objects can vary from 0 to 5 TB
- Different Classes
	- Standard S3
	- Standard - IA (infrequent access) - less frequently accessed data.
	- Amazon Glacier - long term archive.

The basic difference between **infrequent access** and **glacier** is the cost. Where the cost of Infrequent access storage is less than standard S3 and similarly for **glacier**. 

**Reduced Redundancy Storage (RRS)** - Another s3 storage option for storing noncritical, reproducible data at lower levels of redundancy. This is a lower cost storage. But since it's on lower level of redundancy, you can lose objects on this storage device. Hence, you need to have mitigation plans to ensure that you can recover lost objects.

S3 data is organized as a key-based store. When you specify a key, you will get the value associated with the key.

AWS S3 provides read-after-write consistency for PUTS of all new objects access all regions. This means that after you put an object in S3 and if you do a read operation, you will be guaranteed to have the latest version of the object. And also eventually consistency of overwrite PUTS and DELETES.

AWS check's your data and writes them to multiple locations on one availability zone and some to multiple locations so that the data can be durable. So if data is lost on one section, the data can actually be retrieved from another area.

> You can read further on the [official AWS S3 documentation here](https://docs.aws.amazon.com/s3/index.html).
---
# Working with S3

- S3 objects are stored in buckets
- Bucket name needs to be unique across all regions
- There is a bucket [naming convention](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-s3-bucket-naming-requirements.html)
- Properties for the bucket
- Properties for each object
---
# S3 Object Versioning


- Multiple versions of the same object. Helps from accidental deletion. 
- Can retrieve previous versions of the object.
-  Versioning is disabled by default. You need to enable it.
- If versioning is enabled after objects are uploaded, then the older objects have a version ID of null.
- Versioning can only be suspended after it has been enabled.

In order to enable versioning of a s3 bucket: go to S3 -> select a bucket and then ->  go to "properties" options and select "edit" on bucket versioning.

>[!Note]
>Once S3 versioning is enabled, it can't be disabled again. It can only be suspended.

---
# S3 Access Control List


- Amazon S3 Access Control Lists (ACLs) enable you to manage access to buckets and objects.
- Each bucket and object has an ACL attached to it.
- When a request is received against a resource, Amazon S3 checks the corresponding ACL to verify the requester has the necessary access permission.

### To edit and modify the Access control List
1. Navigate to S3 and click to select the object
2. then click on the "Permissions" section on the options.
3. Scroll down to the "Access control list (ACL)" section.
 ---
# S3 Bucket Policy

- Access policies are basically used to control access to your bucket.
- The bucket policy, written in JSON, provides access to the objects stored in the bucket. Bucket policies don't apply to objects owned by other accounts.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Statement1",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::419985026277:root"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::synacy-develop-credit/*"
        }
    ]
}
```

You can control access to a bucket using either bucket policy of an IAM (Identity and Access Management). 

A quick way to make bucket policy is to use the AWS [bucket policy generator](https://awspolicygen.s3.amazonaws.com/policygen.html) which offers a user interface, so you can easily configure your desired bucket policy.

---
# Static Hosting using S3

- S3 in addition to hosting objects can also host static websites and client side scripting.
- Does not support dynamic content.
- Enable Website hosting: `<bucket-name>.s3-website-<AWS-region>.amazonaws.com`.
- Use in conjunction with *Route53* to ensure your custom domain name and points to the s3 website bucket via an Alias record.

### Static hosting using S3 steps


1. Create an Amazon s3 bucket and configure it as a website.
2. Add a bucket policy that makes the content public.
3. Upload an Index document
4. Test your website using the Amazon s3 bucket website endpoint.


To make a s3 bucket to allow static website hosting: go to s3 and choose the bucket -> go to "properties" and navigate to "Static website hosting" and enable it. You need to have index.html to be used as your default document and to be shown when you browse the site (meaning you have to upload an index.html file).

---
# S3 Encryption

- Encryption is done via "Advanced Encryption Standard - 256" by default.
- Server side encryption - Encrypted when it is written to the disks. This is then decrypted by AWS before the object is returned.
	- S3 Managed Keys
	- KMS (Key Managed Keys)
	- Customer provided Keys
- Client side encryption - You manage the encryption keys and the process. In this type of encryption, you manage the keys and the entire process of encryption, so you encrypt the object and manage the tools, and then you upload the object to S3. And when you get the object back, it is your job to make sure the object is decrypted.

To edit the encryption on an object in s3: Go to the AWS management console and go to s3 -> click and select the object and then navigate to "properties" -> scroll down to the "Default encryption" section.

# S3 Further Concepts

### S3 Objects Partition
Objects are partitioned in s3 based on the keys
```bash
111/data/1.png
112/data/2.png
113/data/2.png
```
The object with prefixes above will all be stored on one partition. That's because they all start with sequential numbers.

If you have a numerous number of objects, take for example it may number to millions then it is better to vary the prefixes.

>[!Better Practice:]
Better to keep the prefix variations
>```bash
111/data/1/png
211/data/2.png
311/data/2.png
>```



>[!S3 Error Codes]
> As a developer working with applications it is important to know the error codes and responses by AWS s3. Here is the official documentation for the [AWS S3 error responses](https://docs.aws.amazon.com/AmazonS3/latest/API/ErrorResponses.html).

### S3 Multi-part file upload

- Used to break a large file into chunks and upload them individually. Now, if you have a big file that needs to be uploaded to s3, the option given by AWS is to use multipart file upload. 
- Once all the parts are uploaded and the parts are assembled by AWS, you get the object successfully written to S3. You cannot have a partial object made into s3, all the objects need to be uploaded.
- This is recommended for objects of size greater than 100 MB.





