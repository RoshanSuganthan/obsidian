## Validation for complaint

| Condition | rrn | Status | RC | field to fetch|
| :--- | :----: | ---: | ---:| --:|
| condition 1 | 1234567 | Processing | 103,104 ||
| reversal txn 2 | 1234567 |  Failed| 104 ||
| no dispute | 1234567 |  Successful||
| TXN35621 | 1234567 |  Reversed| 102||

- [complaint](DRC/complaint.excalidraw)
- Debit Not Done During Transaction: 104
- Debit Reversal Not Done, but done now:103
- Debit Reversal Done Online for a failed Transaction: 102
- Doubt -> if not real time -> before response complaint check for auto conversion status
---
## Validation for Dispute
- Need to validate whether there is an complaint for the same transaction ID and rrn is resolved
- yes-> send 102
- no -> initiate reversal 
- if No complaint -> directly raise dispute
- 
- Need to check the status of the case as well.

---
### Validation For Reversal
- if the reversal is done - > send 102
- if reversal is not done - > initiate and send 103

---
### Validation for Refund
if refund is the done - > send 102
if not done -> initiate send 103

---
processed
successful





