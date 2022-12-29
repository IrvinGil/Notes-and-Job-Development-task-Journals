Using the Command Line Interface (CLI), you can have control of AWS services via the command line. You can then create scripts which can really help with automation of commands on AWS services via the command line.

**AWS CLI Example Commands:**
- `aws ec2 describe` | Describe ec2 in your region
- `aws ec2 start-instance --instance-ids i-1348636c` | Start an instance with particular instance id.
- `aws sqs receive-message --queue-url https://queue.amazonaws.com/5464191318123/Test` | Receive via AWS SQS

---

# Cloud Formation - Introduction

- AWS cloud formation helps you to create templates to automate the creation of AWS resources. Using cloud formation, you can easily create development and test environments. Note that there is no additional charge for using the cloud formation service, you just pay for the underlying service created by cloud formation.

A cloud formation template has the following structure:
```json
[
	"AWSTemplateFormatVersion":"version date",
	"Description":"JSON String",
	"Metadata":[
		template metadata
	],
	"Parameters":[
		set of parameters
	],
	"Mapping":[
		set of mappings
	],
	"Conditions":[
		set of conditions
	],
	"Transform":[
		set of transforms
	],
	'Resources':[
		set of resources
	],
	"Outputs":[
		set of outputs
	]
	
]
```

>[!Note:]
>The last and most important part of the cloud formation template is the "resource" section. Where here it is determined on what resources are going to be part of your cloud formation. Resources might be e.g - EC2, S3 and more. And the resources section is also **mandatory**, while all other parts of the template are optional.

### Parts of a Cloud formation template

1. Resources - Specifies the stack resource and their properties, such as Amazon Elastic Cloud Compute (EC2) instance.
2. Parameters - Specifies values that you can pass in to your template runtime. Suppose you have a cloud formation template to spin up an EC2 instance, and you wanted the user to specify the region to launch them in. You can define a parameter for this.
3. Mapping - A mapping of keys and associate values that you can use to specify conditional parameter values, similar to lookup table. 
4. Outputs - Describe that values that are returned whenever you view your stack's properties.


**Sample of a Resource section in AWS cloud formation template:**
```json
[
	"Resources":{
		"MyEC2Instance":{
			"Type":"AWS::EC2::Instance",
			"Properties":{"ImageId":"ami-e5a51786"}
		}
	}
]
```

**Sample of Output section in AWS cloud formation template:**
```json
[
	"Outputs":{
		"Availability":{
			"Description":"Instance ID",
			"Value":{"Fn::GetAtt",["MyEC2Instance","AvailabiltyZone"]}
		}
	}
]
```

>[!Note:]
>You can view the official documentation about [AWS cloud formation here](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/).

---
# AWS Simple Notification Service (SNS)


- SNS is a push notification service that is available on AWS.
- In SNS service, you have the ability to publish messages to a topic. You can then have different and multiple subscribers to a topic.
-  The messages are distributed to the various subscribers.

### Simple Notification Service - Endpoints

- HTTP, HTTPS - Subscribers specify a URL as part of the subscription registration.
- Email, Email-JSON - Messages are sent to registered addresses as email. Email-JSON sends notification as JSON object, while email sends text-based email.
- Simple Queue Service (SQS) - Users can specify an SQS standard queue as the endpoint.
- SMS - Messages are sent to registered phone numbers as SMS text messages.

### Simple Notification Service - Message Format

1. **`MesssageId`** - A Universally unique identifier, unique for each notification published.
2. **`Timestamp`** - The time (in GMT) at which the notification was published.
3. **`TopicArn`** - The topic to which this message was published.
4. **`Type`** - The type of delivery message.
5. **`UnsubscribeURL`** - A link to unsubscribe the end-point from this topic.
6. **`Message`** - The payload(body) of the message.
7. **`Subject`** - The subject field.

>[!Note:]
>The endpoints that are subscribed to the SNS Topic will all instantly receive a message if there is any message published to the SNS Topic. May it be email or HTTP subscription. You can also easily subscribe endpoint via the AWS management console.


---
# AWS Simple Queue System (SQS)

SQS is a simple messaging system that is provided by AWS. It stored messages that can be shared by other systems. Messages can be processed at a later point in time. So basically, it provides you a system in where you can send messages and the message can be processed at any point in time by other systems.

SQS helps decouple systems, and it works best with distributed systems.


### SQS Use case scenario
![[sqs_sample_use_case.png]]
![[sqs_process_workflow.png]]

### AWS SQS parameters

- **Default visibility timeout** - default timeout when a message becomes invisible after being read. Default is 30 seconds, and you can set the visibility timeout yourself.
- **Message retention** - How long the message can be in the queue. Default is 4 days.
- **Maximum message size** - The maximum message size can be 256kb.
- **Receive Message wait time** - Default value is 0 seconds. This relates to **short polling**. So even if the message is not in the queue, the queue is polled. To allow for less CPU cycles, one can enable **long polling** and set the value to greater than 0 seconds.

>[!Note:]
>You can configure the settings/parameter of queue that you created and change the parameters according to your preffered settings.

### AWS Simple Queue System - CLI

1. Getting the URL for the queue:
`aws sqs --region ap-southeast-1 get-queue-url --queue-name Demo`

2. Send a message to the queue:
`aws sqs send-message --queue-url https://ap-southeast-1.queue.amazonaws.com/o853636324145/Demo --message-body "Test Message 1"`

3. Receive Message:
`aws sqs receive-message --queue-url https://ap-southeast-1.queue.amazonaws.com/o853636324145/Demo --attribtue-names All --message-attribute-names All --max-number-of-messages`

>[!Note:]
>You can view the other CLI commands for AWS SQS on the [official documentation](https://docs.aws.amazon.com/cli/latest/reference/sqs/).

---
# Elastic Beanstalk

- Helps provision an environment with a fast turn around time.
- Helps get your development team working with a quick up and running development environment.
- Elastic Beanstalk automatically handles the details of capacity provisioning, load balancing, scaling and application health monitoring.
- Elastic beanstalk supports applications developed in Java, PHP, .NET, Node.js, Python, Go, Ruby and Docker.

![[aws_elastic_beanstalk.png]]

### Steps for creating an Elastic Beanstalk

1. Create an Application
2. Upload an application version in the form of an application source bundle (for example: a java .war file) to elastic beanstalk.
3. Elastic Beanstalk automatically launches an environment and creates and configures the AWS resources.
4. You can manage new versions of your applications and manage multiple environments.

When you successfully created an Elastic beanstalk environment, you can change the configuration of the development environment.

Also note that the version label of your source code must be named properly . e.g - `ecommerce-sourcev1`, `ecommerce-sourcev2`. This is tied to versioning.

---
# AWS Software Development Kit (SDK)

- In order to work with the various developer artifacts available on AWS, we need to have the access keys, in which you can get on the credentials section.

### How it works

The AWS SDK for Java simplifies the use of AWS Services by providing a set of libraries that are consistent and familiar for Java developers. It provides support for API lifecycle consideration such as credential management, retries, data marshaling, and serialization. The AWS SDK for Java also supports higher level abstractions for simplified development.

AWS SDK also have support for many more programming languages.

>[!Note:]
>For code example on using the AWS SDK on your application projects, check it out on the [official documentation repository](https://github.com/awsdocs/aws-doc-sdk-examples). Just choose on the repository folders what language are you using.

---
# AWS Simple Workflow Service

- AWS Simple Workflow Service helps to coordinate work between distributed components of an application.
- Can have workflows and can also have human intervention as part of that workflow. So take, for example, there is a particular workflow of actions like in a bank transaction which requires some human intervention before it can be approved, this can be part of the workflow.
- Difference from SQS - Ensures that a task is executed only once. Since in SQS, there is no guaranteed that a message will only be processed once.

So in a simple workflow service: you have tasks, and you define them as activities. 
You have a decider task which is used to decide whether tasks should run worker tasks based on a certain logic.

### Kinds of task in SWF

1. Worker task
2. Decider task

The worker task is the one that actually does the work. And the decider task decide whether a particular work should be run or not, which is based on a certain logic that you will embed into the simple workflow service (SWS).

---
# AWS Elastic Load Balancer

- Elastic load balancer distributes traffic amongst EC2 Instances
- The elastic load balancer itself is highly available.
- Can combine with auto-scaling service to auto scale instances when the load increases. In this way, whenever the load on your application increases, you can have auto-scaling spin up new EC2 Instances. And the new EC2 instance will automatically be registered to the elastic load balancer.
- Can combine with Route53 to point your domain name to the elastic load balancer endpoint.

![[elastic_load_balancer_placement.png]]

So if you have private and public subnets in which your public subnets are hosting your web service. You can have an elastic load balancer sit in between your public subnets and the internet. So this will be the endpoint which is actually exposed to the internet and to your users. The **ELB** will then be responsible for routing the requests to the desired EC2 instance, and this will be based on its own algorithm to find out which instance should handle the request.














