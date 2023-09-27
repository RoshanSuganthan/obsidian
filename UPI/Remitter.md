# 18.3 COMPLAINT RET

### UPI Sends the Request to Remitter
- Since Bene issued RET
```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ns2:ReqComplaint
xmlns:ns2="http://npci.org/upi/schema/"
xmlns:ns3="http://npci.org/cm/schema/">
<Head ver="2.0" ts="2021-02-22T14:44:02+05:30" orgId="NPCI" 
msgId="5t0yaaWjrmU43CCOehH" prodType="UPI"/>
<Txn id="MMM0000000000005t0ya2TDrPdehbBI5VK" note="complain" 
refId="P1705110000338829715467" 
refUrl="http://www.npci.org.in/" ts="2020-11-18T20:01:51.891+05:30" type="REVERSAL" 
orgTxnId="MMM0000000000005t0ya2TDrPdegTWA4vK" 
custRef="032320100028" orgTxnDate="2021-02-22T14:40:46+05:30" orgRrn="105314960800" 
initiationMode="U1" subType="REMITTER" purpose="00" 
refCategory="00"/>
<Complaint reqAdjFlag="RET" reqAdjCode="115" reqAdjAmount="5.70" 
orgSettRespCode="RB" currCycle="N"/>
</ns2:ReqComplaint>

```

### Remitter Responds to UPI

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:RespComplaint
xmlns:ns2="http://npci.org/upi/schema/">
<Head msgId="PTMIN0114b4bd0aab9bdcf8cbe4de39a2fd" orgId="159761"
 prodType="UPI" ts="2021-02-22T14:43:55+05:30" ver="2.0"/>
<Txn custRef="105314960800" id="MMM0000000000005t0ya2TDrPdehbBI5VK"
initiationMode="U1" note="complain" orgMsgId="5t0yaaWjrmU43CCOehH"
 orgRrn="105314960800" orgTxnDate="2021-02-22T14:40:46+05:30"
 orgTxnId="MMM0000000000005t0ya2TDrPdegTWA4vK" purpose="00"
 refCategory="00" refId="P1705110000338829715467"
 refUrl="http://www.npci.org.in/" subType="REMITTER"
 ts="2020-11-18T20:01:51.891+05:30" type="REVERSAL"/>
<Complaint currCycle="N" orgSettRespCode="RB" reqAdjAmount="5.70"
 reqAdjCode="115" reqAdjFlag="RET"/>
<Resp Result="SUCCESS" reqMsgId="5t0yaaWjrmU43CCOehH">
<Ref IFSC="PYTM0123456" acNum="917417356866" addr="test@mypsp"
 adjAmt="5.70" adjCode="501" adjFlag="RRC"
 adjRefId="PTM38899977839" adjRemarks="Remitter Response."
 adjTs="2021-02-22T14:40:46+05:30" approvalNum="977839" code="0000"
 procStatus="COMPLETED" regName="Ram" seqNum="1" type="REMITTER"/>
</Resp>
</ns2:RespComplaint>
```

# 18.1 COMPLAINT DRC
### UPI Sends the Request to Remi
```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ns2:ReqComplaint
xmlns:ns2="http://npci.org/upi/schema/"
xmlns:ns3="http://npci.org/cm/schema/">
<Head ver="2.0" ts="2021-02-20T19:02:46+05:30" orgId="NPCI" 
msgId="5t0yaaWjrmTZAJjBdLo" prodType="UPI"/>
<Txn id="PTM5a6d0ab4e34544fab19dca35f755lij" note="complain" 
refId="PTM5a6d0ab4e34544fab19dca35f75554a4" refUrl="http://www.paytm.com"
ts="2021-02-20T19:02:40+05:30" type="COMPLAINT" 
orgTxnId="PTM5a6d0ab4e34544fab19dca35f75554a4" custRef="105199974203" 
orgTxnDate="2021-02-20T18:59:49+05:30" orgRrn="105199974203" initiationMode="U1"
subType="REMITTER" purpose="00" refCategory="00"/>
<Complaint reqAdjFlag="PBRB" reqAdjCode="U005"
reqAdjAmount="14.00" orgSettRespCode="RR" currCycle="N"/>
</ns2:ReqComplaint> 
```

Remi to UPI
```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:RespComplaint
xmlns:ns2="http://npci.org/upi/schema/">
<Head msgId="XYD0000000000005t0yaaTz3NnVMsyDnW4" orgId="700004"
 prodType="UPI" ts="2021-01-08T10:49:39+05:30" ver="2.0"/>
<Txn custRef="105199974203" id="PTM5a6d0ab4e34544fab19dca35f755lij"
 initiationMode="U1" note="complain" orgRrn="105199974203"
 orgTxnDate="2021-02-20T18:59:49+05:30" orgTxnId="PTM5a6d0ab4e34544fab19dca35f75554a4" purpose="00" refCategory="00" refId="PTM5a6d0ab4e34544fab19dca35f75554a4" refUrl="http://www.paytm.com" subType="REMITTER" ts="2021-02-20T19:02:40+05:30" type="COMPLAINT"/><Complaint currCycle="Y" orgSettRespCode="00" reqAdjAmount="2.00"
 reqAdjCode="U009" reqAdjFlag="PBRB"/>
<Resp reqMsgId="5t0yaaWjrmTZAJjBdLo" result="SUCCESS">
<Ref IFSC="AABF0003002" acNum="300210100001299" addr="resh@upi"
 adjAmt="2.00" adjCode="102" adjFlag="DRC"
 adjRefId="P1705110000338829745321" adjRemarks="DRC Initiated"
 adjTs="2020-12-28T00:00:41+05:30" approvalNum="651725"
 procStatus="COMPLETED" seqNum="1" settCurrency="INR" type="REMITTER"/>
</Resp>
</ns2:RespComplaint>
```