## Payment Service
```
04:44:34.951 [http-nio-9091-exec-1] INFO  o.a.c.c.C.[Tomcat].[localhost].[/] - Initializing Spring DispatcherServlet 'dispatcherServlet'
04:44:34.952 [http-nio-9091-exec-1] INFO  o.s.web.servlet.DispatcherServlet - Initializing Servlet 'dispatcherServlet'
04:44:34.954 [http-nio-9091-exec-1] INFO  o.s.web.servlet.DispatcherServlet - Completed initialization in 2 ms
04:52:30.147 [simpleMessageListenerContainer-9] INFO  c.s.w.p.stripe.api.StripeApiService - Attempting to charge customerId: cus_N6zQYLIbupe0pc with amount: 24.95 with paymentId: 12346
04:52:31.148 [simpleMessageListenerContainer-9] INFO  c.s.w.p.stripe.api.StripeApiService - Successfully charged customerId: cus_N6zQYLIbupe0pc with amount: 24.95 with paymentId: 12346 and chargeId: ch_3MMlRKBA3Urobhh30LY93Cn2
04:52:35.064 [simpleMessageListenerContainer-10] INFO  c.s.w.payment.aws.sns.SnsService - Publishing event STRIPE_CHARGE_SUCCEEDED : 1
04:52:35.100 [scheduling-1] INFO  c.s.w.p.s.e.j.StripePaymentGatewayEventPublisherJob - Queued 1 charge succeeded events for publishing.
04:52:36.330 [simpleMessageListenerContainer-11] INFO  c.s.w.p.a.AllocatedAutoReloadProcessorListener - Received an allocated payment event for team: 11 with payment: 12346
04:59:38.898 [simpleMessageListenerContainer-12] INFO  c.s.w.p.stripe.api.StripeApiService - Attempting to charge customerId: cus_N6zQYLIbupe0pc with amount: 75.0 with paymentId: 12347
04:59:40.212 [simpleMessageListenerContainer-12] INFO  c.s.w.p.stripe.api.StripeApiService - Successfully charged customerId: cus_N6zQYLIbupe0pc with amount: 75.0 with paymentId: 12347 and chargeId: ch_3MMlYFBA3Urobhh31CPkPPaC
04:59:45.031 [simpleMessageListenerContainer-13] INFO  c.s.w.payment.aws.sns.SnsService - Publishing event STRIPE_CHARGE_SUCCEEDED : 2
04:59:45.076 [scheduling-1] INFO  c.s.w.p.s.e.j.StripePaymentGatewayEventPublisherJob - Queued 1 charge succeeded events for publishing.
04:59:46.229 [simpleMessageListenerContainer-14] INFO  c.s.w.p.a.AllocatedAutoReloadProcessorListener - Received an allocated payment event for team: 11 with payment: 12347

```



## SMS-Billing Service
```
2023-01-05 04:43:52.241  INFO 7 --- [nio-9097-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
2023-01-05 04:43:52.242  INFO 7 --- [nio-9097-exec-1] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
2023-01-05 04:43:52.245  INFO 7 --- [nio-9097-exec-1] o.s.web.servlet.DispatcherServlet        : Completed initialization in 3 ms
2023-01-05 04:52:41.351  INFO 7 --- [enerContainer-5] .s.p.TeamPlanSubscriptionUpdaterListener : Processing event: SUBSCRIPTION_SUCCEEDED : 10
2023-01-05 04:52:41.356  INFO 7 --- [enerContainer-6] c.s.w.s.t.TeamContextPopulatorListener   : Processing event with subject: SUBSCRIPTION_SUCCEEDED : 10
2023-01-05 04:52:41.644  INFO 7 --- [enerContainer-6] c.s.w.s.t.TeamContextPopulatorListener   : Added TeamContext. TenantId:2, TeamId:11
2023-01-05 04:52:41.651  INFO 7 --- [enerContainer-5] .s.p.TeamPlanSubscriptionUpdaterListener : Added TeamPlanSubscription. TeamId:11, SmsPlanId:4
2023-01-05 04:59:51.250  INFO 7 --- [enerContainer-7] .s.p.TeamPlanSubscriptionUpdaterListener : Processing event: SUBSCRIPTION_SUCCEEDED : 12
2023-01-05 04:59:51.274  INFO 7 --- [enerContainer-8] c.s.w.s.t.TeamContextPopulatorListener   : Processing event with subject: SUBSCRIPTION_SUCCEEDED : 12
2023-01-05 04:59:51.283  INFO 7 --- [enerContainer-8] c.s.w.s.t.TeamContextPopulatorListener   : No changes to be made. TeamContext for team 11 already exists.
2023-01-05 04:59:51.292  INFO 7 --- [enerContainer-7] .s.p.TeamPlanSubscriptionUpdaterListener : Updated TeamPlanSubscription. TeamId:11, New SmsPlanId:5

```



## SMS-Broadcast Service
```
04:52:41.498 [simpleMessageListenerContainer-22] INFO  c.s.w.s.b.p.TeamPlanSubscriptionUpdaterListener - Processing event: SUBSCRIPTION_SUCCEEDED : 10
04:52:41.502 [simpleMessageListenerContainer-23] INFO  c.s.w.s.b.t.TeamContextPopulatorListener - Processing event with subject: SUBSCRIPTION_SUCCEEDED : 10
04:52:41.850 [simpleMessageListenerContainer-23] INFO  c.s.w.s.b.t.TeamContextPopulatorListener - Added TeamContext. TenantId:2, TeamId:11
04:52:41.861 [simpleMessageListenerContainer-22] INFO  c.s.w.s.b.p.TeamPlanSubscriptionUpdaterListener - Added TeamPlanSubscription. TeamId:11, SmsPlanId:4
04:59:51.318 [simpleMessageListenerContainer-24] INFO  c.s.w.s.b.p.TeamPlanSubscriptionUpdaterListener - Processing event: SUBSCRIPTION_SUCCEEDED : 12
04:59:51.334 [simpleMessageListenerContainer-25] INFO  c.s.w.s.b.t.TeamContextPopulatorListener - Processing event with subject: SUBSCRIPTION_SUCCEEDED : 12
04:59:51.344 [simpleMessageListenerContainer-25] INFO  c.s.w.s.b.t.TeamContextPopulatorListener - No changes to be made. TeamContext for team 11 already exists.
04:59:51.363 [simpleMessageListenerContainer-24] INFO  c.s.w.s.b.p.TeamPlanSubscriptionUpdaterListener - Updated TeamPlanSubscription. TeamId:11, New SmsPlanId:5

```

