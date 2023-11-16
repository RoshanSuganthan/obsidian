- Logic
```
isDuplicate = FALSE; /*Initialize the value*/

  

duplicateRecord = Run a query on Complaint table with "where conditon transactionID, cardNumber"

  

If(Count of duplicateRecord > 0)

{

      isDuplicate = TRUE

}

  

if(reasonCode = Valid)

{

      If(isPartialCB = TRUE && isDuplicate = TRUE)

      {

            Duplicate Complaint --> Assigned to Analyst with Open status

      }

      else if(isPartialCB = FALSE && (Complaint table where conditon transactionID, cardNumber, ReasonCode))

      {

            Duplicate Complaint --> Raise the complaint with Closed status

      }

      else if(isPartialCB = FALSE && isDuplicate = TRUE)

      {

            Duplicate Complaint --> Assigned to Analyst with Open status

      }

      else if(isDuplicate = FALSE)

      {

            Call BRE logic and submit the case / assign to analyst accordingly

      }

}

else

{

      Case is invalid. Based on the BRE logic either "pend and close" or "directly close the case" accordingly.

}

```


- My code 
```
boolean isDuplicate = duplicateComplaintCheck(complaintCache);
if(!isDuplicate){ 
```

SQL Transaction
```
INSERT INTO `transaction_entity` (`transaction_id`, `transaction_status`, `merchant_name`, `merchant_number`, `transaction_amount`, `original_transaction_date`, `source_transaction_amount`, `destination_transaction_amount`, `pos_entry_mode`, `transaction_code`, `auth_code`, `card_type`, `mcc`, `issuer_bin`, `acquirer_bin`, `card_no`, `pos_condition_code`, `account_owner_prefix`, `account_owner_firstName`, `account_owner_lastName`, `cardholder_name`, `contact_email`, `contact_no`, `cardholder_address`, `mail_city`, `customer_id`, `pos_info`, `additional_data`, `original_trans_ref_num`, `upi_id`, `customer_name`, `transaction_type`, `remitter_bank`, `beneficiary_bank`, `customer_address`, `additional_info`, `stan`, `primary_account_number`, `local_transaction_time`, `local_transaction_date`, `terminal_verification_results`, `message_type_indicator`, `processing_code`, `response_code`, `transmission_date_time`, `verification_data`, `card_issuer_country_code`, `market_specific_data_identifier`, `payment_indicator`, `pos_environment`, `additional_auth_indicators`, `transaction_identifier`, `track_2_data`, `refUrl`, `customer_ref_number`, `ifsc`, `status_code`, `approval_number`, `currency`, `account_number`, `settlement_date`, `arn`, `issuer_online_transaction`, `transaction_currency_code`, `auth_rrn`, `settlement_rrn`, `vrol_case_no`, `representment_date`, `credit_received_date`, `network_submission_date`, `network_status`, `processing_date`, `product_id`, `acquirer_country_code`, `cardholder_billing_currency_code`, `cardholder_billing_amount`) VALUES
('6213', 'Approved', 'TestMerchant', '123abc', 1000, '2023-08-31 15:48:59.0', 10, 20, '05', '15', '456234', 'V', '9110', '123456', '123456', '40000XXXXX00002', '11', 'Mr', 'aj', 'mike', 'mike aj', 'mike@gmail.com', '1234567890', 'no.12, pearl city, chennai', 'chennai', '0009', '09454556787', '999000345678', '6001', NULL, NULL, NULL, NULL, NULL, NULL, 'MIVSwdfghjjdfgh', '111111', NULL, NULL, NULL, '0000000000000000000000010100000000000000', '0100', '001234', '00', '0920101010', NULL, 'USA', 'E', '03', 'I', '2', NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, '2023-09-25 13:49:23.0', NULL, NULL, '356', NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, '', NULL, NULL, 100.0000)

```

```
catch (Exception e){  
    LOGGER.log(Level.SEVERE, String.join(" ", ErrorMessage.FAILED_CREATING_CB.getError()));  
}  
LOGGER.log(Level.INFO, String.join(" ", LoggerMessage.EXITING.getMessage(), "fileChargeback",  
       LoggerMessage.FUNCTION.getMessage()));  
return Optional.of(fileChargeBackResponse);
```

```
List<String> list = new ArrayList<>();  
list.add(complaintId.get());  
List<String> caseNumberList = new ArrayList<>();  
caseNumberList.add(caseNumber.get());  
  
auditTrailUtils.auditCapture(caseNumberList, list, Constants.DOESNT_EXCEEDS_TOTAL_TXN_AMOUNT);


---
List<String> list = new ArrayList<>();  
list.add(complaintId.get());  
  
List<String> caseNumberList = new ArrayList<>();  
caseNumberList.add(caseNumber.get());  
  
auditTrailUtils.auditCapture(caseNumberList, list, Constants.EXCEEDS_TOTAL_TXN_AMOUNT);


---
List<String> list = new ArrayList<>();  
list.add(complaintId.get());  
  
List<String> caseNumberList = new ArrayList<>();  
caseNumberList.add(caseNumber.get());  
  
auditTrailUtils.auditCapture(caseNumberList, list, Constants.PARTIAL_DISPUTE_ALREADY_EXIST);

---
List<String> list = new ArrayList<>();  
list.add(complaintId.get());  
  
List<String> caseNumberList = new ArrayList<>();  
caseNumberList.add(caseNumber.get());  
  
auditTrailUtils.auditCapture(caseNumberList, list, Constants.PARTIAL_DISPUTE_ALREADY_EXIST);
```

```public void duplicateValidation(Transaction transaction) {  
    ComplaintCache complaintCache = redisService.getCache(sessionContext.getSessionId());  
    try {  
       boolean isDuplicate = duplicateComplaintCheck(complaintCache);  
  
       if(isDuplicate){  
          complaintCache.setDuplicate(true);  
          //complaint.setDuplicate(true)  
          //added open status(now closed)          //complaint.setStatusEntity(StatusEntity.builder().statusId(StatusId.CLOSED_ID_DUPLICATE.getStatusId()).build());          complaintCache.setStatusId(StatusId.CLOSED_ID_DUPLICATE.getStatusId());  
          //call partial dispute scenario  
          if(complaintCache.isPartialDispute()){  
             //complaint.setPartialDispute(true);  
             complaintCache.setDuplicate(true);  
             Optional<Long> count=fetchPartialDisputeComplaint(complaintCache,false);  
             if(count.isPresent()){  
                if(count.get()>0){  
                   // close the complaint  
                   LOGGER.log(Level.INFO, String.join(Constants.LOG_SEPERATOR, "All Ready Dispute has been raised for this transaction:",complaintCache.getTransactionId()));  
                   //complaint.setStatusEntity(StatusEntity.builder().statusId(StatusId.CLOSED_ID_DUPLICATE.getStatusId()).build());  
                   complaintCache.setStatusId(StatusId.CLOSED_ID_DUPLICATE.getStatusId());  
                }  
                else{  
                   boolean isCpAllowed = fetchAndSumPartialDisputeCB(complaintCache,transaction);  
                   if(isCpAllowed){  
                      //initially status as open (now to close)  
                      //complaint.setStatusEntity(StatusEntity.builder().statusId(StatusId.CLOSED_ID_DUPLICATE.getStatusId()).build());                      complaintCache.setStatusId(StatusId.CLOSED_ID_DUPLICATE.getStatusId());  
                   }  
                   else {  
                      //complaint.setStatusEntity(StatusEntity.builder().statusId(StatusId.CLOSED_ID_DUPLICATE.getStatusId()).build());  
                      complaintCache.setStatusId(StatusId.CLOSED_ID_DUPLICATE.getStatusId());  
                   }  
                }  
  
             }  
  
          }  
          if(!complaintCache.isPartialDispute()){  
             Optional<Long> count=fetchPartialDisputeComplaint(complaintCache,true);  
             //List<Complaint> listofCases = complaintRepository.find("where isPartialDispute?=1 transaction?=2 and reasonCode?=3",true,complaintCache.getTransactionId(),complaintCache.getReasonCode()).list();  
             if(count.isPresent()){  
                if(count.get()>0){  
                   LOGGER.log(Level.INFO, String.join(Constants.LOG_SEPERATOR, LoggerMessage.ALREADY_PARTIAL_DISPUTE_EXIST.getMessage(),complaintCache.getTransactionId()));  
                   //complaint.setStatusEntity(StatusEntity.builder().statusId(StatusId.CLOSED_ID_DUPLICATE.getStatusId()).build());  
                   complaintCache.setStatusId(StatusId.CLOSED_ID_DUPLICATE.getStatusId());  
                }  
                else if(count.get()==0L){  
                   /* added for exact duplicate scenario same full partial dispute and same txn but may be different code */  
                   Optional<Long> countDuplicate = fetchPartialDisputeComplaint(complaintCache,complaintCache.isPartialDispute());  
                   if(countDuplicate.isPresent()){  
                      if(countDuplicate.get()>0){  
                         LOGGER.log(Level.INFO, String.join(Constants.LOG_SEPERATOR, LoggerMessage.ALREADY_PARTIAL_DISPUTE_EXIST.getMessage(),complaintCache.getTransactionId()));  
                         //complaint.setStatusEntity(StatusEntity.builder().statusId(StatusId.CLOSED_ID_DUPLICATE.getStatusId()).build());  
                         complaintCache.setStatusId(StatusId.CLOSED_ID_DUPLICATE.getStatusId());  
                      } else {  
                         /*for condition where count is 0*/  
                         //complaint.setDuplicate(false);                         complaintCache.setDuplicate(false);  
                      }  
  
                   }  
  
                }  
                // close the complaint  
                //do i need to add in audit trail??             }  
          }  
       }  
       redisService.updateCache()  
    }catch(Exception e){  
       LOGGER.log(Level.SEVERE, String.join(" ", ErrorMessage.FETCHING_PARTIAL_CASES_FAILED.getError()));  
    }  
}
```

	private boolean isDuplicate; --> complaintCache

```
public boolean duplicateComplaintCheck(ComplaintCache complaintCache) {  
    LOGGER.log(Level.INFO, String.join(" ", LoggerMessage.INSIDE.getMessage(), "duplicateComplaintCheck",  
          LoggerMessage.FUNCTION.getMessage()));  
    try {  
       //List<Complaint> listOfCases = complaintRepository.find("from Complaint where transaction.transactionId =? 1 ",complaintCache.getTransactionId()).list();  
       Long count = complaintRepository.find("from Complaint where transaction.transactionId =? 1 ",complaintCache.getTransactionId()).count();  
       //not a dulpicate cases  
       return (count>0L);  
    }catch (Exception e){  
       LOGGER.log(Level.SEVERE, String.join(" ", ErrorMessage.DUPLICATE_CHECK_FAILED.getError()));  
    }  
    LOGGER.log(Level.INFO, String.join(" ", LoggerMessage.EXITING.getMessage(), "duplicateComplaintCheck",  
          LoggerMessage.FUNCTION.getMessage()));  
     return false;  
    }  
  
    public Optional<Long> fetchPartialDisputeComplaint(ComplaintCache complaintCache, boolean isPartialDisputeCheck) {  
       LOGGER.log(Level.INFO, String.join(" ", LoggerMessage.INSIDE.getMessage(), "fetchPartialDisputeComplaint",  
             LoggerMessage.FUNCTION.getMessage()));  
       try {  
             //List<Complaint> listofCases = complaintRepository.find("from Complaint where isPartialDispute =? 1 and transaction.transactionId =? 2 and disputeCategoryEntity.reasonCodeId is not null  ", isPartialDisputeCheck,complaintCache.getTransactionId()).list();  
          Long count = complaintRepository.find("from Complaint where isPartialDispute =? 1 and transaction.transactionId =? 2 and disputeCategoryEntity.reasonCodeId is not null  ", isPartialDisputeCheck,complaintCache.getTransactionId()).count();  
          return Optional.ofNullable(count);  
          //return Optional.ofNullable(listofCases);  
       } catch (Exception e) {  
          LOGGER.log(Level.SEVERE, String.join(" ", ErrorMessage.FETCHING_PARTIAL_CASES_FAILED.getError()));  
       }  
       LOGGER.log(Level.INFO, String.join(" ", LoggerMessage.EXITING.getMessage(), "fetchPartialDisputeComplaint",  
             LoggerMessage.FUNCTION.getMessage()));  
          return Optional.empty();  
    }  
public boolean fetchAndSumPartialDisputeCB(ComplaintCache complaintCache,Transaction transaction) {  
    LOGGER.log(Level.INFO, String.join(" ", LoggerMessage.INSIDE.getMessage(), "fetchAndSumPartialDisputeCB",  
          LoggerMessage.FUNCTION.getMessage()));  
    try {  
       //List<CaseDetail> sum= caseRepository.find("select sum(disputeAmount) from CaseDetail where isPartialDispute =? 1 and transaction.transactionId =? 2 and statusEntity.statusId not in (1002,1003,1011,1012,1014)  ", true,complaintCache.getTransactionId()).list();  
       List<CaseDetail> listOfCb= caseRepository.find("from CaseDetail where isPartialDispute =? 1 and transaction.transactionId =? 2 and statusEntity.statusId not in (1002,1003,1011,1012,1014)  ", true,complaintCache.getTransactionId()).list();  
       LOGGER.log(Level.INFO, String.join(" ", LoggerMessage.INSIDE.getMessage(), "fetchAndSumPartialDisputeCB",  
             LoggerMessage.FUNCTION.getMessage()));  
       Double sum = 0D;  
       if(listOfCb.size()>0){  
          for(CaseDetail caseDetail : listOfCb){  
             sum += caseDetail.getDisputeAmount();  
          }  
          if((sum + complaintCache.getDisputeAmount())<=transaction.getTransactionAmount()){  
             return true;  
          }  
       }  
       return false;  
    } catch (Exception e) {  
       LOGGER.log(Level.SEVERE, String.join(" ", ErrorMessage.FETCHING_PARTIAL_CASES_FAILED.getError()));  
    }  
    LOGGER.log(Level.INFO, String.join(" ", LoggerMessage.EXITING.getMessage(), "fetchAndSumPartialDisputeCB",  
          LoggerMessage.FUNCTION.getMessage()));  
    return false;  
}
```

```java
  
                //Optional<Long> count = fetchPartialDisputeComplaint(complaintCache, true);  
                //List<Complaint> listofCases = complaintRepository.find("where isPartialDispute?=1 transaction?=2 and reasonCode?=3",true,complaintCache.getTransactionId(),complaintCache.getReasonCode()).list();//              if (count.isPresent()) {  
//                 if (count.get() > 0) {  
//                    LOGGER.log(Level.INFO, String.join(Constants.LOG_SEPERATOR, LoggerMessage.ALREADY_PARTIAL_DISPUTE_EXIST.getMessage(), complaintCache.getTransactionId()));  
//                    //complaint.setStatusEntity(StatusEntity.builder().statusId(StatusId.CLOSED_ID_DUPLICATE.getStatusId()).build());  
//                    complaintCache.setInvalid(true);  
//                    complaintCache.setStatusId(StatusId.CLOSED_ID_DUPLICATE.getStatusId());  
//                    complaintCache.setErrorMessage(LoggerMessage.ALREADY_PARTIAL_DISPUTE_EXIST.getMessage());  
//                 } else if (count.get() == 0L) {  
//                 // close the complaint  
//                 //do i need to add in audit trail??  
//                 }  
//              }  
  
                /* added for exact duplicate scenario same full partial dispute and same txn but may be different code */                //Optional<Long> countDuplicate = fetchPartialDisputeComplaint(complaintCache, complaintCache.isPartialDispute());//              if (countDuplicate.isPresent()) {  
//                 if (countDuplicate.get() > 0) {  
//                    LOGGER.log(Level.INFO, String.join(Constants.LOG_SEPERATOR, LoggerMessage.ALREADY_PARTIAL_DISPUTE_EXIST.getMessage(), transactionId));  
//                    //complaint.setStatusEntity(StatusEntity.builder().statusId(StatusId.CLOSED_ID_DUPLICATE.getStatusId()).build());  
//                    complaintCache.setInvalid(true);  
//                    complaintCache.setStatusId(StatusId.CLOSED_ID_DUPLICATE.getStatusId());  
//                    complaintCache.setErrorMessage(LoggerMessage.SAME_DISPUTE_EXIST.getMessage());  
//                    }  
////                   } else {  
////                      /*for condition where count is 0*/  
////                      //complaint.setDuplicate(false);  
////                      complaintCache.setInvalid(false);  
////                      complaintCache.setDuplicate(false);  
////                   }  
//  
//              }
```

```java
//Optional<Long> count = fetchPartialDisputeComplaint(complaintCache, false);  
//              if (count.isPresent()) {  
//                 if (count.get() > 0) {  
//                    // close the complaint  
//                    LOGGER.log(Level.INFO, String.join(Constants.LOG_SEPERATOR, "All Ready Dispute has been raised for this transaction:", complaintCache.getTransactionId()));  
//                    //complaint.setStatusEntity(StatusEntity.builder().statusId(StatusId.CLOSED_ID_DUPLICATE.getStatusId()).build());  
//                    complaintCache.setStatusId(StatusId.CLOSED_ID_DUPLICATE.getStatusId());  
//                    complaintCache.setErrorMessage(LoggerMessage.ALREADY_PARTIAL_DISPUTE_EXIST.getMessage());  
//                    complaintCache.setInvalid(true);  
//                 } else {  
//  
//                 }  
//  
	//              }
```

```java
//Map for returning the service response
HashMap<String, Boolean> hashMap = new HashMap<>();  
hashMap.put("isDuplicate",false);  
hashMap.put("CreateChargeBack",true);  
hashMap.put("isInvalid",false);

```

---
## UDM Code
