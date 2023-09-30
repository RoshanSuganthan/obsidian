[] audit Capture
[ ] Logs
[ ] Route to Analyst FLOW 
[ ] Lombok
[ ] Method Name
[ ]  cbs - transaction to be pull as a list with rrn, 
[ ] Validation for RC 103,102,104, UT1-6
[ ] Implement Reqcheck transaction
[ ] add reversal and refund flow in kogito 

---
Doubts code level
- case constants should be in UPI commons
- change calling from one controller to another controller
- why visa Transaction and UPI transaction are in same table.
- 
---
# Business level Doubts

| TXNID | rrn | Status | RC |
| :--- | :----: | ---: | ---:|
| TXn13245 | 1234567 | Processing | 103,104 |
| TXN35621 | 1234567 |  Failed| 104 |
| TXN35621 | 1234567 |  Successful|
| TXN35621 | 1234567 |  Reversed| 102|

- transaction status - reversal is maintained or whether there will be a new  transaction of reversal (purchase txn, refund transaction)
- how to validate processing status  and resume the process-> 103,104 
 - for reversal and refund should we need to validate?? because there can already be an reversal imitated or refund initiated but UPI asks us to make refund or reversal
 - message ID to be generated with the for response ??? what purpose 
- how to handle duplicate? create a new record and send the previous status or create a new record and let the kogito take the decision as it goes.
- Original transaction ID when reversal/Refun d has to be done - orgTxnID and id in Txn tag
- 
---
# Frontend Level Doubts
- Need front end for UPI

---
### Roshan
- [ ] API gateway
- [ ] UPI commons
- [ ] KOGITO with validation


### KAMAL
- [ ] udm-odr-service 
	 - table, pojo change - case create change 
	 - implement an rest client to fetch transaction list
	 - implement an rest client to send response to kogito

### Sreemathi
- [ ] implement APIs 


