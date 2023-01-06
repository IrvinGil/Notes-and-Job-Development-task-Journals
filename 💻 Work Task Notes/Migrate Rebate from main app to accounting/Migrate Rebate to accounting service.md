

### To-do:
- [ ] Domain Models
	- [ ] `Rebate`
	- [ ] `RebateAccountingDocument`
	- [ ] `RebateReversal`
	- [ ] `RebateReversalAccountingDocument`
- [ ] Controller
	- [ ] `RebateController`
- [ ] Service
	- [ ] `RebateService`
	- [ ] `RebateListenerService`
	- [ ] `RebateReversalListenerService`
- [ ] DTO
- [ ] Unit Test



### Decisions

- [ ] Decide whether to also implement an `AccountingDocument` with this job. But first identify what `Accounting Document `does.



## Services for Observing rebate 

- whitelabel local shared
- whitelabel UI
- whitelabel main app
as of now we only need to run the main services to observe the rebate process, but if the are going to be done then we can include the accounting service.
- accounting


## Logs from Terminal

Credit service when adding rebate:
```
2023-01-06 02:57:00.210  INFO 7 --- [nio-9093-exec-2] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Start completed.
2023-01-06 02:59:59.024  INFO 7 --- [nio-9093-exec-2] s.w.c.n.NormalResponseSentEventPublisher : "Published normal request CREDITED event for 1 REBATE."

```