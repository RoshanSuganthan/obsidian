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

### UPI sends the Status Update to Remitter
```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ns2:RespComplaint
xmlns:ns2="http://npci.org/upi/schema/"
xmlns:ns3="http://npci.org/cm/schema/">
<Head ver="2.0" ts="2021-02-20T17:11:22+05:30" orgId="NPCI" 
msgId="5t0yaaWjrmTZoXFR4hA" prodType="UPI"/>
<Txn id="PTM9a1ef7bfa7b64d84abc14d9a0d58e789" note="complain" 
refId="PTM9a1ef7bfa7b64d84abc14d9a0d58e784" 
refUrl="http://www.paytm.com" ts="2021-02-20T17:11:15+05:30" type="STATUSUPDATE" 
orgMsgId="PTMIN0114ba576e00849c3adb29d6c2cfbe"
orgTxnId="PTM9a1ef7bfa7b64d84abc14d9a0d58e784" custRef="105199973904" 
orgTxnDate="2021-02-20T17:06:46+05:30" 
orgRrn="105199973904" initiationMode="U1" subType="REMITTER" purpose="00" 
refCategory="00"/>
<Complaint reqAdjFlag="PBRB" reqAdjCode="U010" reqAdjAmount="30.00" 
orgSettRespCode="RB" currCycle="N"/>
<Resp reqMsgId="PTMIN0114ba576e00849c3adb29d6c2cfbe" crn="UPI21022053584">
<Ref type="BENEFICIARY" addr="resh@upi" approvalNum="651725" 
IFSC="AABF0003002" adjAmt="13.00" 
adjTs="2020-05-11T00:00:41+05:30" adjRefId="P1705110000338829745321" 
adjFlag="TCC" adjCode="102" adjRemarks="Credit Reversal done online now"/>
</Resp>
</ns2:RespComplaint>
```

### UPI sends the response to Remitter for 18.4 ReqComplaint (Auto Conversion - Complaint to chargeback)

```xml
<ns2:RespComplaint xmlns:upi=“http://npci.org/upi/schema/”>
<Head ver=“2.0” ts=“2020-05-13T00:00:41+05:30” orgId=“180002”
msgId=“SBI4a69d250abe6433899c2f5a08fc0d008” prodType="UPI" />
<Txn id=“SBIwf6fdfde87454cc084071ca3725e2979” ts=“2020-05-11T00:00:41+05:30”
custRef=“013100747891” refId=“P1705110000338829715467”
refUrl=“http://www.npci.org.in/” refCategory=“00” note = ""
initiationMode=“U0” purpose=“00” type="DISPUTE" 
subtype=“STATUSUPDATE”
orgTxnId=“YBL4a69d250abe6433899c2f5a081000033882”
orgRrn=“013100747891” orgTxnDate=“2020-05-12T00:00:41+05:30” >
</Txn>
<Complaint reqAdjFlag= "B" reqAdjCode="1061" reqAdjAmount = "500.00"
orgSettRespCode = "00" currCycle = “N” />
</Complaint>
<Resp reqMsgId="NPCI4a69d250a899c2f5a08fc0d008" crn="UPI2005138765432"> 
<Ref type="PAYEE" procStatus = "COMPLETED" seqNum=""addr=”resh@upi” adjAmt="500.00" settCurrency="INR" IFSC = “SBIN0001501” acNum = “21341651725361524” approvalNum=”“ adjTs="2020-05-11T00:00:41+05:30" adjRefId="P1705110000338829745321"adjFlag="B" adjCode="1061" adjRemarks="Chargeback Raised">											</Ref>
</Resp>
</ns2:RespComplaint>			
```

### 18.5 ReqComplaint (Pre Approved Online Refund – RRC 501)
- UPI send to remitter
```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ns2:ReqComplaint
xmlns:ns2="http://npci.org/upi/schema/"
xmlns:ns3="http://npci.org/cm/schema/">
<Head ver="2.0" ts="2021-02-20T11:49:56+05:30" orgId="NPCI" 
msgId="5t0yaaWjrmTYR0vRRWE" prodType="UPI"/>
<Txn id="PTM0662ec43548044f38eb64369f0acGFRI" note="complain" 
refId="MMM0000000000005t0ya2TDrPd93Rgh2Ra" refUrl="http://www.paytm.com" 
ts="2021-02-20T11:49:49+05:30" type="REFUND" 
orgTxnId="MMM0000000000005t0ya2TDrPd93Rgh2Ra" custRef="105111366009" 
orgTxnDate="2021-02-20T11:43:03+05:30" orgRrn="105111366009" initiationMode="U2" 
subType="REMITTER" purpose="00" refCategory="00"/>
<Complaint reqAdjFlag="REF" reqAdjCode="1064" reqAdjAmount="13.00" orgSettRespCode="00" currCycle="N"/>
</ns2:ReqComplaint>undefined</ns2:ReqComplaint>




```

- Response for Reversal
```xml
<ns2:RespComplaint
xmlns:ns2="http://npci.org/upi/schema/">
<Head msgId="PTMIN011424ccb1fc5fcef2f86eefb4410b" orgId="159761"
 prodType="UPI" ts="2021-02-20T11:49:49+05:30" ver="2.0"/>
<Txn custRef="105111366009" id="PTM0662ec43548044f38eb64369f0acGFRI"
 initiationMode="U2" note="complain" orgMsgId="5t0yaaWjrmTYR0vRRWE"
 orgRrn="105111366009" orgTxnDate="2021-02-20T11:43:03+05:30"
 orgTxnId="MMM0000000000005t0ya2TDrPd93Rgh2Ra" purpose="00"
 refCategory="00" refId="MMM0000000000005t0ya2TDrPd93Rgh2Ra"
 refUrl="http://www.paytm.com" subType="REMITTER"
 ts="2021-02-20T11:49:49+05:30" type="REFUND"/>
<Complaint currCycle="N" orgSettRespCode="00" reqAdjAmount="13.00"
 reqAdjCode="1064" reqAdjFlag="REF"/>
<Resp Result="SUCCESS" reqMsgId="5t0yaaWjrmTYR0vRRWE">
<Ref IFSC="PYTM0123456" acNum="917417356866" addr="test@mypsp"
 adjAmt="13.00" adjCode="501" adjFlag="RRC"
 adjRefId="PTM38899973101" adjRemarks="Remitter Response."
 adjTs="2021-02-20T11:43:03+05:30" approvalNum="973101" code="0000"
 procStatus="COMPLETED" regName="Ram" seqNum="1" type="REMITTER"/>
</Resp>
</ns2:RespComplaint>
```