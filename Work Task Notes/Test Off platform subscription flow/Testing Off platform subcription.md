[[taskNote]]


## Prerequisite:
- You can read on the context of what _off-platform_ on confluence [here](https://utinternational.jira.com/wiki/spaces/SKB/pages/1691254785/Pitch+-+Adding+handling+for+Off-Platform+Subscription+charges)
- **Technical Req:**
	1. **Master** branch of **`Whitelabel-local-shared`** | the original branch of overdraft changes is the `feature/add-batch-overdraft-processed-queue`.
	2. God branch of off platform subscription on **whitelabel-mainapp** (`feature/off-platform-subscription`).
	3. **Credit**, **accounting** service.
	4. Clean DB.
- Miro board for context [here]().
- Need to review test cases ↓:

# Off Platform Subscription Scenarios

-   Create Paid Plan A $10 (USD)
-   Create Paid Plan B $100 (USD)
-   Create Paid Plan C $100 (AUD)
-   Configure a tax setting on the tenant at 10%

```sql
INSERT INTO update_tax_event(id, date_occurred, is_inclusive, name, rate, tenant_id, updated_by_user_id)
VALUES ('f6b06b3d-e420-4379-8613-7da960e1ccf3', NOW(), false, 'Value Added Tax (VAT)', 10, 3, '0378bc66-38e6-4e93-8b9b-51f7f9d81c9b');
```

```sql
INSERT INTO update_tax_event(id, date_occurred, is_inclusive, name, rate, tenant_id, updated_by_user_id)
VALUES ('f6b06b3d-e420-4379-8613-7da960e1ccf3', NOW(), false, 'Value Added Tax (VAT)', 10, 3, '2910f887-010b-415b-abca-2a950651770a');
```

> Link to information and explanation for inserting [[Tax]]:

For reference, here is the table structure of `update_tax-event` table.
```bash
whitelabel=# select * from update_tax_event ;
 id | date_occurred | is_inclusive | name | rate | tenant_id | updated_by_user_id 
----+---------------+--------------+------+------+-----------+--------------------
(0 rows)
```

# Migration Process

We also need to test the migration flow. To do this, checkout to develop branch and subscribe two customers each to a plan. Then checkout to feature/off-platform-subscription and then do an upgrade scenario on one customer and test the other customer on the renewal flow. Make sure the _**PreChargedSubscription**_ table is populated with the list of existing subscriptions that way the app knows which customers need to be migrated to the new plan.

```sql
INSERT INTO pre_charged_subscription VALUES (nextval('pre_charged_subscription_sequence'),$version,$subscription_perioid,'$status');
```

```sql
INSERT INTO pre_charged_subscription VALUES (nextval('pre_charged_subscription_sequence'),0,1,'PENDING');
INSERT INTO pre_charged_subscription VALUES (nextval('pre_charged_subscription_sequence'),0,2,'PENDING');
INSERT INTO pre_charged_subscription VALUES (nextval('pre_charged_subscription_sequence'),0,3,'PENDING');
INSERT INTO pre_charged_subscription VALUES (nextval('pre_charged_subscription_sequence'),1,4,'PENDING');
INSERT INTO pre_charged_subscription VALUES (nextval('pre_charged_subscription_sequence'),1,5,'PENDING');
```

For reference, here is the table structure of `pre_charged_subsscription` table:

```bash
whitelabel_backup=# select * from pre_charged_subscription;
 id | version | subscription_period_id | status 
----+---------+------------------------+--------
(0 rows)

```


>[!Sample Query]
>`INSERT INTO pre_charged_subscription(id, version, subscription_period_id, status) VALUES (nextval('pre_charged_subscription_sequence'),0,1,'PENDING')`;

## Results after migration

### Upgrade

-   [ ] Confirm if charge is sent to Credit service
-   [ ] If user is postpaid, confirm if credit is deducted

### Renewal

-   [ ] Confirm if charge is sent to Credit service
-   [ ] If user is postpaid, confirm if credit is deducted

# Internet Plan

### **First Time Subscription + Renewal**

**When:**

-   Admin subscribes a customer to Paid Plan A for the first time

**Then**:

-   [ ] A SubscriptionRequest should be created for that charge with status `DONE`.
-   [ ] Customer Credits should be deducted $11 (including tax).
-   [ ] Subscrption’s `hasPendingChanges` boolean field should be set to `false`.
-   [ ] Check if the subscription period dates are correct and if they are active.

**When:**

-   Fast forward to the next renewal cycle.

**Then:**

-   [ ] A SubscriptionRequest should be created for that charge with status `DONE`.
-   [ ] Customer Credits should be deducted $11 (tax included)
-   [ ] Subscrption’s `hasPendingChanges` boolean field should be set to `false`.
-   [ ] Check if the subscription period dates are correct and if they are active.

### **Plan Upgrade**

**When:**

-   Admin subscribes customer to Paid Plan A

**Then:**

-   [ ] A SubscriptionRequest should be created for that charge with status `DONE`.
-   [ ] Customer Credits should be deducted $11 (tax included)
-   [ ] Subscription’s `hasPendingChanges` boolean field should be set to `false`.
-   [ ] Check if the subscription period dates are correct and if they are active.

**When:**

-   Fast forward 15 days after the subscription date.
-   Admin upgrades customer’s plan to Paid Plan B

**Then:**

-   [ ] A _SubscriptionRequest_ should be created for that charge with status `**DONE**`.
-   [ ] It will be recorded in _SubscriptionEvent_ with an event_type of **`UPGRADE`**
-   [ ] Customers Credits should be deducted an amount of $99 with tax inclusion

### Forced Upgrade

**When:**

-   Admin subscribes a customer to Plan A
-   Fast forward 15 days after the subscription date.
-   Admin upgrades customer’s plan to a Paid Plan which is not in sequence to Plan A

**Then:**

-   [ ] A _SubscriptionRequest_ should be created for that charge with status `**DONE**`.
-   [ ] It will be recorded in _SubscriptionEvent_ with an event_type of **`UPGRADE`**
-   [ ] Customer should be charged zero amount and will require manual calculation.

### **Downgrade**

**When:**

-   Admin subscribes a customer to Paid Plan B for the first time
-   Fast Forward 15 days after subscription
-   Update customer’s plan subscription to Plan A with lower prrice.

**Then:**

-   [ ] A _SubscriptionRequest_ should be created for that charge with status `**DONE**`.
-   [ ] It will be recorded in _SubscriptionEvent_ with an event_type of **`UPGRADE`**
-   [ ] Customer should be charged {still no idea}

### Reset subscription for a customer with one plan subscription

### Reset subscription for a customer with multiple plan subscription

### Multiple plan subscriptions + Renewal

### Multiple plan subscriptions with one plan upgrade.

### Multiple plan subscriptions with one plan downgrade.

### Multiple subscriptions with one scheduled subscription that’s before the renewal day.

### Scheduled subscription

### Subscription with burstable add-on

### Unsubscribe plan