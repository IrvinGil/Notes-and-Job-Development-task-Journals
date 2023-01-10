This page is about initializing a new [[Amazon SQS]] queue on the whitelabel-local-shared service application.

## Prerequisite

- whitelabel-local-shared



## Instructions

>[!Note:]
>This part if for creating a queue only (where applications listens and polls on the queue).  

1. Open `whitelabel-local-shared` project and navigate to `scripts/localstack`. Here you will see the initialization scripts for all the queues run on local-shared service.

![[whitelabel-local-shared-queue-directory.png]]

3. Find the appropriate initializer script (example: if you want to create a queue that is related to accounting, then choose `local-stack-accounting-initializer.sh`). 

### Queue Initializer Code

1. The pattern for creating the initializer code for a queue is that you must first create the "dead-letter-queue" first.

```bash
awslocal sqs create-queue --queue-name Accounting-Develop-ProcessChargeRequest-DLQ
```
> *Sample DLQ*

2. And then you create the initializer code for the actual queue:

```bash
awslocal sqs create-queue --queue-name Accounting-Develop-ProcessChargeRequest --attributes '{ "RedrivePolicy": "{\"deadLetterTargetArn\":\"arn:aws:sqs:us-east-1:000000000000:Accounting-Develop-ProcessChargeRequest-DLQ\",\"maxReceiveCount\":\"4\"}", "MessageRetentionPeriod": "259200", "VisibilityTimeout": "60" }'
```
>*Sample code for the queue initializer*

Notice that on the `deadLetterArn`, the name of the dead-letter-queue that we created first is present. When creating your own queue intializer then just also copy the name of the DLQ that you first created. 

>[!Note:]
>Follow our [[Codding Standard]] for creating [[Amazon SQS]] initializer. [See here for link to confluence.](https://utinternational.jira.com/wiki/spaces/SKB/pages/3250880513/AWS+Naming+Conventions+V2#Queue-Names-and-Topic-Names)



# For creating a [[SNS Topic]] and Subscribing [[Amazon SQS]] on Local-shared

You can view the SNS topics are being created on the directory: `scripts/localstack/local-stack-1-topics-initializer.sh` . Just follow the codding standard for naming convention and creation of topic on local-shared. 

And for subscribing the existing queues to the created topic, see the script file:  `scripts/localstack/local-stack-whitelabel-subscription-initializer.sh` . 


### Pattern

1. Create the dead-letter-queue (DLQ)
2. Create the queue
3. Create the `sns subscribe` to subscribe the queue to the created topic.

```bash
# create DLQ
awslocal sqs create-queue --queue-name MainApp-Develop-BatchOverdraftChargeProcessed-DLQ  

# create queue and attach created DLQ
awslocal sqs create-queue --queue-name MainApp-Develop-BatchOverdraftChargeProcessed --attributes '{ "RedrivePolicy": "{\"deadLetterTargetArn\":\"arn:aws:sqs:us-east-1:123456789012:MainApp-Develop-BatchOverdraftChargeProcessed-DLQ\",\"maxReceiveCount\":\"4\"}", "MessageRetentionPeriod": "259200" }'  

# subscribe queue to SNS topic
awslocal sns subscribe --topic-arn arn:aws:sns:us-east-1:123456789012:WLDevelop-Credit-OverdraftChargeRequestProcessed --protocol sqs --notification-endpoint http://localhost:4576/queue/MainApp-Develop-BatchOverdraftChargeProcessed
```
>Sample code for creating queue and subscribing it to topic.