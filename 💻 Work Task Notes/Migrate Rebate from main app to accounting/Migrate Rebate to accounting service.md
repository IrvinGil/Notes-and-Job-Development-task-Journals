>[!Notice:]
>A notion page related about this task has been made. [View here](https://www.notion.so/synacy-appdev/Accounting-UI-UX-discussion-fad29640a11e42b2993c598f14aad300).

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


# Function/Methods

## RebateService

### Dependencies
- TenantService
- CurrentUserService
- UserService
- TeamService
- NotificationService
- ValidateAndSaveService | for saving data. Not totally coupled (may just be replaced by repository class)

functions as service for rebate and rebateReversal
- [x] public createRebate
- [x] public createRebateReversal
- [x] public fetchRebateById
- [x] public updateSuccessfulRebateAndCreateAccountingDocument *(might not be needed)*
- [x] public updateFailedRebate
- [x] public fetchRebateReversalById
- [x] public fetchPaginatedSuccessfulRebateByTeam
- [x] public generateAdminRebateDTO
- [ ] updateSuccessfulRebateReversalAndCreateAccoutingDocument  *(might not be needed)*
- [ ] updateFailedRebateReversal
- [ ] sendNotificationForSuccessfulRebate
- [ ] isRebateAlreadyReversed

### Notes

1. instead of fetch filter used in the main app, we will use custom page request on for getting paginated data.

### AdminRebateDTO
>turned to `RebateDisplayDTO` in accounting service


## RebateController

- [ ] create a customPage function


## RebateRepository

Sample of custom query
```java
	@Query(value = "SELECT count(id) FROM Product where featured = :featured")
	public Long count(@Param("featured") boolean featured);
```






