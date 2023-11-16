- [ ] 18/10/23 - Need to restrict the multiple case creation 
- Logic -
		Transaction Id
		is Partial Dispute
		Reason code 
		amount - to be added in the complaintCache
		- how to check if this partial dispute is also duplicate??
- how to compare two sessionIds
	- [ ] Scenario 1 - New transaction partial dispute true -> same reason code 
	- [ ] Scenario 2 - New transaction partial dispute false -> same reason code 
	- [ ] Scenario 3 - Old transaction partial dispute true -> same reason code
	- [ ] Scenario 4 - Old transaction partial dispute false -> same reason code
	- [ ] Scenario 1 - New transaction partial dispute true ->  
	- [ ] Scenario 2 - New transaction partial dispute false -> 
	- [ ] Scenario 3 - Old transaction partial dispute true ->
	- [ ] Scenario 4 - Old transaction partial dispute false ->

## Restrict Duplicate API hits
### Idempotent Tokens
- single thread
- use operation name and data to generate random idempotent String
- have a hashmap to add the used UUID
- then add the Used UUID once you generate the new one
- throw error with 409
```java
import java.util.UUID;

public class IdempotentUUIDGenerator {
    public static UUID generateIdempotentUUID(String operationName, String requestData) {
        // Combine the operation name and request data to create a unique identifier
        String uniqueString = operationName + requestData;
        
        // Generate a UUID based on the unique string
        UUID idempotentUUID = UUID.nameUUIDFromBytes(uniqueString.getBytes());
        
        return idempotentUUID;
    }

    public static void main(String[] args) {
        String operation = "create_user";
        String requestData = "{\"username\":\"john_doe\",\"email\":\"john@example.com\"}";

        UUID idempotentToken = generateIdempotentUUID(operation, requestData);
        System.out.println(idempotentToken);
    }
}

```
### Request Deduplication:
- This can be achieved by storing a unique identifier (e.g., a request ID or a hash of the request data) for each incoming request and comparing it to previously processed requests
### Request Queuing:

	
	
- [ ] 25/10/23
-  From request Body generate a idempotent token
- save the token in Redis
-  Expire the Key after 1 min
-  Use Synch 
```javascript
var myHeaders = new Headers();
myHeaders.append("x-session", "a34ad1d6-3838-4ab2-94ac-415cf02cd7ec");
myHeaders.append("x-api-key", "bst.W0jW18ArJhHH078Hueh9CxUmiPerBk2xvN5bsB74sJ");
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
  "transactionId": "6001",
  "questionAns": "Y",
  "questionId": "3",
  "comments": "test",
  "isPartialDispute": false,
  "disputeAmount": "104"
});

var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: raw,
  redirect: 'follow'
};

fetch("http://localhost:9090/questionnaire", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

- duplicate is true (patialdispute,txnid,reasoncode)
- duplicate is false
- partial= false and txnid and reason code same ---> mark this as a duplicate
- partial = true and txnid and reason code is same

```java
    if(complaintCache.isPartialDispute()){//           LOGGER.log(Level.INFO, String.join(" ", LoggerMessage.EXITING.getMessage(), "duplicateComplaintCheck",  
//                 LoggerMessage.FUNCTION.getMessage()));

//     LOGGER.log(Level.INFO, String.join(" ", LoggerMessage.EXITING.getMessage(), "duplicateComplaintCheck",  
//              LoggerMessage.FUNCTION.getMessage()));  
          //return true;
```