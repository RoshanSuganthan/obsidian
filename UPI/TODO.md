[] audit Capture
[ ] Logs
[ ] Route to Analyst FLOW 
[ ] Lombok
[ ] Method Name
[ ]   cbs - transaction to be pull as a list with rrn, 
[ ] Validation for RC 103,102,104, UT1-6
[ ] Implement Reqcheck transaction
[ ] add reversal and refund flow in kogito 

---
Doubts code level
- case constants should be in UPI commons
- change calling from one controller to another controller
- why visa Transaction and UPI transaction are in same table.
---
# Business level Doubts

| TXNID | rrn | Status | RC |
| :--- | :----: | ---: | ---:|
| TXn13245 | 1234567 | Processing | 103,104 |
| TXN35621 | 1234567 |  Failed| 104 |
| TXN35621 | 1234567 |  Successful|
| TXN35621 | 1234567 |  Reversed| 102|

- how to validate processing status  and resume the process-> 103,104 
 - for reversal and refund should we need to validate?? because there can already be an reversal initated or refund initiated but UPI asks us to make refund or reversal

---
# Frontend Level Doubts
- Need front end for UPI
- 