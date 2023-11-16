
## Restrict Duplicate API hits
### Idempotent Tokens
- single thread
- use operation name and data to generate random idempotent String
- have a hashmap to add the used UUID
- then add the Used UUID once you generate the new one
- throw error with 409
-   From request Body generate a idempotent token
- save the token in Redis
-  Expire the Key after 1 min
-  Use Synch
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
- This can be achieved by storing a unique identifier (e.g., a request ID or a hash of the request data) for each incoming request and comparing it to previously processed requestsdummy code
```java
//package com.backspace.exception;  
//  
//import com.backspace.utils.ConstantsEnum;  
//import io.quarkus.runtime.util.ExceptionUtil;  
//import org.apache.commons.lang3.ObjectUtils;  
//  
//import javax.ws.rs.core.Response;  
//import javax.ws.rs.ext.ExceptionMapper;  
//import javax.ws.rs.ext.Provider;  
//import java.time.LocalDateTime;  
//import java.util.ArrayList;  
//import java.util.List;  
//import java.util.logging.Level;  
//import java.util.logging.Logger;  
//  
//@Provider  
//public class CustomExceptionHandler implements ExceptionMapper<CustomException> {  
//  
//  private static final Logger LOGGER = Logger.getLogger(CustomExceptionHandler.class.getName());  
//  
//  @Override  
//  public Response toResponse(CustomException exception) {  
//     LOGGER.log(Level.SEVERE, String.join(ConstantsEnum.LOG_SEPERATOR.getMessage(), "Exception occured",  
//           ExceptionUtil.generateStackTrace(exception)));  
//  
//     List<ErrorMessage> errorMessage = new ArrayList<ErrorMessage>();  
//     ExceptionResponse msg;  
//  
//     if( ObjectUtils.defaultIfNull(exception.getStatusCode(), 400)== 0) {  
//        errorMessage.add(ErrorMessage.builder().errorMessage(exception.getMessage()).build());  
//  
//        msg = ExceptionResponse.builder().timestamp(LocalDateTime.now())  
//              .status(exception.getStatusCode()).message(errorMessage).build();  
//        return Response.status(exception.getStatusCode()).entity(msg).build();  
//     }  
//  
//     errorMessage.add(ErrorMessage.builder().errorMessage("Please try again later").build());  
//  
//     msg = ExceptionResponse.builder().timestamp(LocalDateTime.now())  
//           .status(Response.Status.BAD_REQUEST.getStatusCode()).message(errorMessage).build();  
//     return Response.status(Response.Status.BAD_REQUEST).entity(msg).build();  
//  }  
//  
//}

``` 

in Global handler
```java
if(ObjectUtils.defaultIfNull(((CustomException) exception).getStatusCode(), 0)!= 0){  
    errorMessage.add(ErrorMessage.builder().errorMessage(exception.getMessage()).build());  
    msg = ExceptionResponse.builder().timestamp(LocalDateTime.now())  
          .status(((CustomException) exception).getStatusCode()).message(errorMessage).build();  
    return Response.status(((CustomException) exception).getStatusCode()).entity(msg).build();  
}

```