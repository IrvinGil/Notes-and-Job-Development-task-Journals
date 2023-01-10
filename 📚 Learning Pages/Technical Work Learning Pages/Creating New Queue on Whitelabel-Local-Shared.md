This page is about initializing a new [[Amazon SQS]] queue on the whitelabel-local-shared service application.

## Prerequisite

- whitelabel-local-shared



## Instructions

1. Open `whitelabel-local-shared` project and navigate to `scripts/localstack/queues`. Here you will see the initialization scripts for all the queues run on local-shared service.

![local shared queue directory](whitelabel-local-shared-queue-directory.png)

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

Notice that 