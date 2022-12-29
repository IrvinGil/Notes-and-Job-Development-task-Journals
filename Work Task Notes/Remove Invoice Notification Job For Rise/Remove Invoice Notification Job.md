[[Task Note]]

contents of notification dir:
- [x] InvoiceEmailNotificationRecord | Entity
- [x] InvoiceEmailNotificationRecordType
- [x] InvoiceNotificationRecordRepository
- [x] RiseCustomerBillingInfoDTO
- [x] RiseInvoiceEmailNotificationJob

unit test: invoice.test.com.synacy.invoice.customer.notification
- [x] RiseInvoiceEmailNotificationJobSpec




note: find usages for each classess if there are any that are using an instance of the classes and make sure that no other class are using them before removing them.

---
```sql
INSERT INTO email_template (id, subject, body)
VALUES (nextval('email_template_sequence'), 'Your RISE Invoice $invoiceNumber is due in $daysUntilSent days', '<p >Dear valued customer,</p><br /><p ><br /></p><br /><p >We''d like to remind you that your invoice $invoiceNumber with an amount due worth $totalAmount<span> </span>$currency will be due on $dueDate.</p><br /><p >Please make sure to pay your bill on or before its due date to avoid penalties.</p><br /><p >Please disregard this email if you have paid your bill.</p><br /><p >If you need help in paying for your invoices, please feel free to reply to this email.</p><br /><p ><br /></p><br /><p >Thank you,</p><br /><p >RISE Collections Team</p><img src="https://pastepixel.com/image/hhqxNbsRZ6M38pkVyvzU.png" alt=""/>');

INSERT INTO email_notification_config (id, tenant_id, days_until_sent, send_from, carbon_copy, email_template_id, type, enabled)
VALUES (nextval('email_notification_config_sequence'), 123456, 3, 'team.mawa@synacy.com', 'team.mawa@synacy.com', currval('email_template_sequence'), 'BEFORE_DUE_DATE', TRUE);

INSERT INTO customer_invoice (id, team_id, tenant_id, start_date, end_date, amount_due, currency, status, invoice_id, date_created, payment_status, due_date)
VALUES (nextval('invoice_id_sequence'), 1234, 123456, now(), now(), 1234.567, 'USD', 'SENT', 'DEF456', now(), 'AWAITING_PAYMENT', now());

INSERT INTO email_notification_schedule (id, invoice_id, schedule_date, type, status, date_created, last_updated)
VALUES (nextval('email_notification_schedule_sequence'), 'DEF456', now(), 'BEFORE_DUE_DATE', 'PENDING', now(), now());

INSERT INTO team_billing_information (id, team_id, email_address, company_name)
VALUES (nextval('team_billing_information_sequence'), 1234, 'irvin.mercado@synacy.com', 'thatcompany');

INSERT INTO usd_to_php_exchange_rate (id, value, timestamp, published_date)
VALUES(nextval('usd_to_php_exchange_rate_sequence'), 10, now(), DATE_TRUNC('day', NOW()) - INTERVAL '8 hours');
```

Interpretation of the quires above ☝;
1. this query inserts the email template on the database that will be used by the email_sender service as reference for creating email.
2. this query inserts data about the email configuration and information/data that will be used on the creation of the email message event that will be send and stored on the AWS S3bucket as part of an SQS event.
3. this query inserts: information about the customer's invoice. Data on this table should be populated when an invoice for a customer is generated.
4. this query inserts: data on the schedule of email notification
5. inserts data on the team's billing information
6. inserts data on the exchange rate table: this will be used on some calculation of the invoice service.



---

aws --endpoint-url=http://localhost:4576 sqs send-message --queue-url http://localhost:4576/queue/Invoice-Develop-EmailNotificationQueue --message-body '{"emailNotificationScheduleId":1}' --message-attributes '{ "contentType": { "DataType":"String", "StringValue":"application/json;charset=UTF-8" } }'




queue name of where invoice service : EmailNotificationListener polls:  http://localhost:4576/queue/Invoice-Develop-EmailNotificationQueue
queue name where email.sender service: EmailSenderProcessorListener polls: http://localhost:4576/queue/WLDevelop-EmailSender-ProcessSendEmail

---
Logs from invoice service when sqs event is sent to email_sender service:
11:14:09.176 [scheduling-1] INFO  c.s.i.c.CustomerInvoiceUpdaterJob - Updating customer invoices
11:14:36.228 [simpleMessageListenerContainer-13] INFO  c.s.i.e.n.EmailNotificationListener - Received email notification schedule with ID 1
11:14:36.493 [simpleMessageListenerContainer-13] INFO  c.s.i.e.n.EmailNotificationService - Email {sendTo=irvin.mercado@synacy.com, sendFrom=team.mawa@synacy.com, subject=Your RISE Invoice DEF456 is due in 3 days, initiator=invoice, carbonCopy=team.mawa@synacy.com, blindCarbonCopy=, body=<p >Dear valued customer,</p><br /><p ><br /></p><br /><p >We'd like to remind you that your invoice DEF456 with an amount due worth 1,234.57<span> </span>USD will be due on 09 Dec 2022.</p><br /><p >Please make sure to pay your bill on or before its due date to avoid penalties.</p><br /><p >Please disregard this email if you have paid your bill.</p><br /><p >If you need help in paying for your invoices, please feel free to reply to this email.</p><br /><p ><br /></p><br /><p >Thank you,</p><br /><p >RISE Collections Team</p><img src="https://pastepixel.com/image/hhqxNbsRZ6M38pkVyvzU.png" alt=""/>}
11:14:38.264 [simpleMessageListenerContainer-13] INFO  c.s.i.e.n.EmailNotificationService - Email Notification Schedule ID 1 sent.



---
Logs from email_sender service when sqs event is received:

11:14:38.238 [simpleMessageListenerContainer-5] INFO  c.s.e.e.EmailSenderProcessorListener - Received request, bucket: synacy-develop-invoice, key: queue-message/18e37ac6-3ee2-4866-ac4f-90fabcdcbffb_12_09_2022.json
11:14:39.451 [simpleMessageListenerContainer-5] INFO  c.s.e.e.EmailSenderProcessorListener - Received and processing request from invoice, send to: irvin.mercado@synacy.com full request details=> {"sendTo":"irvin.mercado@synacy.com","sendFrom":"team.mawa@synacy.com","subject":"Your RISE Invoice DEF456 is due in 3 days","initiator":"invoice","carbonCopy":"team.mawa@synacy.com","blindCarbonCopy":""}




---

aws --endpoint-url=http://localhost:4576 sqs receive-message --queue-url http://localhost:4576/queue/WLDevelop-EmailSender-ProcessSendEmail


aws --endpoint-url=http://localhost:4576 sqs receive-message --queue-url http://localhost:4576/queue/WLDevelop-EmailSender-ProcessSendEmail max-number-of-messages 10


---
```JSON
{
    "Messages": [
        {
            "MessageId": "c6c2101f-f275-4bef-8852-e6ec8928e12b",
            "ReceiptHandle": "c6c2101f-f275-4bef-8852-e6ec8928e12b#7ec5890c-0314-4b39-8b65-927d8430f424",
            "MD5OfBody": "e7a18cb53dd5c1aa9ce13f59f91d201a",
            "Body": "{\"s3Key\":\"queue-message/a2cd79cf-2596-4529-a2fe-ec85eb0d80d7_12_11_2022.json\",\"s3Bucket\":\"synacy-develop-invoice\"}"
        }
    ]
}
```

---
## Testing changes in natural way | running whitelabel services
- [ ] Just give the customer enough credit for a subscription and fast forward the clock to the next month billing cycle.

**Example:**

| Subscription date | Plan Renewal Date | Times |
|-------------------|-------------------|-------|
| Dec 12, 2020      | Jan 12, 2023      | 1     |
|                   | Feb 12, 2023      | 2     |


