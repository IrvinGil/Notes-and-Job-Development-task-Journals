Linked to this note: [[Testing Off platform subcription]]

### When testing migration Process
1. First checkout the **develop** branch and perform the tax insertion from there. Reason for this is because if you run `feature/off-platform-subscription` branch, there will be an error in flyway migrate.  (Missing this step could lead to restarting your steps in testing.)





### Draft Notes
---

psql -h localhost -U postgres -c 'create database whitelabel_oldflow_backup with template whitelabel' && psql -h localhost -U postgres -c 'create database invoice_oldflow_backup with template invoice'

---

INSERT INTO pre_charged_subscription(id, version, subscription_period_id, status) VALUES (nextval('pre_charged_subscription_sequence'), 4, 4, 'PENDING');

INSERT INTO pre_charged_subscription(id, version, subscription_period_id, status) VALUES (nextval('pre_charged_subscription_sequence'), 4, 5, 'PENDING');