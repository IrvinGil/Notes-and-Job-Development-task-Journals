To delete a message from an Amazon SQS queue using Localstack, you can use the sqs delete-message command. Here is an example of how to do it:
```bash
aws --endpoint-url=http://localhost:4576 sqs delete-message \
    --queue-url http://localhost:4576/queue/my-queue \
    --receipt-handle <receipt-handle>
```
In this example, my-queue is the name of the SQS queue, and <receipt-handle> is the receipt handle of the message you want to delete. The receipt-handle is a unique identifier for the message that is returned when you receive the message from the queue. You can use the sqs receive-message command to receive messages from the queue and get their receipt handles.

Keep in mind that the aws command-line interface (CLI) must be installed and configured on your local machine, and the --endpoint-url parameter must be set to the Localstack endpoint for SQS (http://localhost:4576 in the example above).


[[AWShowto]]