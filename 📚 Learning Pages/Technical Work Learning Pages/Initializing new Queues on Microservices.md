
The code for initializing [[Amazon SQS]] for Staging and Production Environment are located at the `deployment` directory of the application. And inside the queue directory: `java/com/synacy/accountingdeployment/application/queue`. `ProductionQueues` are for initializing the queues for **Production**. And `StagingQueues` are for initializing queues on **Staging** environment.

One merged to the main branch, our deployment pipeline on AWS will automatically initialize these queues.

>[!Reminder]
>We must also create queues for our development environment for testing on our local machines. See [[Creating New Queue on Whitelabel-Local-Shared]] for reference.