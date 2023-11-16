- put it in a java Library 
- generate a jar and use it across the microservice with the dependency
- convert all dependency api to this common imageutils in seperate package
Q-> seperate java project for image or add it in commons in util folder???

- UDM backend DEV - old category maping to be changed
- same scenario for customer portal
- what is the use of customer portal now- only for email via upload right?? we are exposing the questionniare api directly right 
- - -
ImageUpload model to be removed
- tightly coupled with document types
- sol: 
- - -
``` java
List<String> allowedExtensions = Arrays.asList("png", "jpg", "jpeg", "tiff", "tif", "pdf");
```
need to fetch the array from the DB
- sol:
- - - 
```java
image.setSize(file.length());
```
to  set the size we are passing image object to multiple function

- - - 
### Questionnaire upload file
- In future if we add multiple files how to handle
- this throws  and breaks out of  code
```java
if (!errorMessage.isEmpty()) {  
    if (!errorMessage.get().isEmpty()) {  
       throw new CustomException("Error in uploaded Files" + errorMessage.get());  
    }  
}
```
- username from where we will get from questionnaire??
```java
String imageOwner = StringUtils.isNotEmpty(userName) ? userName  
       : ConstantsEnum.CUSTOMER_FILE_UPLOAD.getMessage();
```

- shall i handle like this so that transactional can catch if case is not persisted
```java
 boolean wasAcksql = caseRepository.persist(caseDetail);
if(wasAck){
	 boolean wasAckmongo = commonService.persistCaseInMongo(caseDetail, ComplaintStatusEnum.VALID);  
	 if(wasAck){
	 auditTrailUtils.auditCapture(caseNumberList, caseIdlist, Constants.CHARGEBACK_CREATED); 
	 }
	 else{
	 throw new customException("");
	 }
	
}else{
throw new customException("");
}

```
- int length = 10; shall i keep in constants?? -> used in randomUtils
```
 int length = 10; 
 RandomStringUtils.random(length, true, false);
```
- Mongo panache query for saving in mongo - shall i use transactional annotation??
- image update -> need to be filtered with casenumber and then update
- caseid, case number and image validation
```
updateFilesInMongoDB(image, caseId, caseNumber);
```
- persistCaseInMongo-> casenumber to be added for invalid case 
 so that we can filter using casenumber and add api to fetch images via casenumber
- masking should be asynch
```
maskingUploadFile(fileRequest, fileName, id, imageOwner, size, caseId, stage, category);
```

### udm backend
- need to change the upload image service to common in utils
- should i need to handle it using category?? redundant code 
-  image fetch by casenumber
```

```


- - - 
# Question to be asked with the business team
- in future should we need to send multiple category and multiple image at the same time
-  tiff to be handled ?? internally in application not just masking

- - - 
- commons create a new package - class Name imageUtils()
- based on the commons use change the backend methods
-  to be asked with the business team for adding multiple cate- NOT needed
- tiff as well - yes for sending purpose and no for 
- username NOTAVAILABE  if not not avilable
- How to handle response for persist - > search in internet
- image error message in questionnaire need to be addressed properly
-  isMasked, isProcessed flag in backend as well as the 
- Error handling for masking service
- have pojo kind in python service which every field not used 
- to find source take it from the request header
- stage need to be handled in customer upload history and representment doc hist

- - - 
# Code
```java
public class FileRequest {  
    private String file;  
    private String type;  
    private String name;  
    private String category;  
}

public class ImageUpload {

	@RestForm
	private List<FileRequest> files;
	
	@RestForm
    @PartType(MediaType.TEXT_PLAIN)
	public String userComments;

// for getting by caseId 
	@RestForm
    @PartType(MediaType.TEXT_PLAIN)
    public String imgCaseNumber;
    
    @RestForm
    @PartType(MediaType.TEXT_PLAIN)
    public boolean isCustomerpotalUrlActive;
// post call
@POST  
@Path("uploadImg/")  
@Consumes(MediaType.MULTIPART_FORM_DATA)  
public Response uploadImages(ImageUpload formData) {
	Image image = new Image();  
	Map<String, List<String>> errorMap = new HashMap<>();
	errorMap =  validateFiles(formData, image)
	//Optional<List<String>> errorFileList = validateFiles(formData, image)
	if (!errorMap.isEmpty()) {
					formData.files = formData.files.stream()
						.filter(fileUpload -> !(errorMap.get(formData.files[0]).contains(fileUpload.fileName())))
						.collect(Collectors.toList());
			}
	Image image = generateImageObject(formData,image);
	updateImageByCaseNumber(caseNumber, image);
	

}
//validate files
	public Map<String, List<String>> validateFiles(ImageUpload formData, Image image,Map<String, List<String>> errorMap) {
		for (FileRequest fileInBase64 : formData.files()) {
				File file = convertToFile(fileInBase64)
			boolean isValid = checkFile(fileInBase64.fileName(),file, fileInBase64.type(), image);
			if (!isValid) {
				errorFileList.add(file.fileName());
			}
		}
		errorMap.put(formData.files.category[0], errorFileList);
		return errorMap
	}
public File convertToFile(FileRequest fileRequest) {  
  
    // Decode Base64 string to byte array  
    byte[] decodedBytes = Base64.getDecoder().decode(fileRequest.getFile());  
    // String path = String.join("/", filepath, fileRequest.getName());  
    File file = new File(fileRequest.getName());  
    try {  
       OutputStream outputStream = new BufferedOutputStream(new FileOutputStream(file));  
       outputStream.write(decodedBytes);  
       outputStream.close();  
    } catch (Exception e) {  
       LOGGER.log(Level.SEVERE, "File not found exception occured" + e.getStackTrace());  
    }  
    return file;  
}
 


// save files 
public boolean saveFiles(ImageUpload formData, image) {
		try {
			for (FileRequest file : formData.files) {
				String originalFileName = file.getfileName();
				String contentType = file.gettype();
				File myObj = new File(file.filePath().toUri());
				// file size to be determined
				//long size = file.size(); 
				String fileName = getFileName(formData, originalFileName);
				LOGGER.log(Level.INFO, String.join(ConstantsEnum.LOG_SEPERATOR.getMessage(), "Generated file name: ",
						fileName, formData.imgCaseId, authToken.getRealm()));
				String imageOwner = (source.equals("portal")) ? authToken.getUserName() : "customer";
				//byte[] imageBytes = new byte[(int) myObj.length()];
				//imageBytes = Files.readAllBytes(myObj.toPath());
				//String bytesBase64 = Base64.getEncoder().encodeToString(imageBytes);
				int length = 10;
				String id = authToken.getRealm() + "_" + RandomStringUtils.random(length, true, false);
				LOGGER.log(Level.INFO, String.join(ConstantsEnum.LOG_SEPERATOR.getMessage(), "Generated ID for image: ",
						id, formData.imgCaseId, authToken.getRealm()));

				Image image = new Image(originalFileName, fileName, id, category, bytesBase64, imageOwner,
						LocalDateTime.now(), source, size, type, false);
				caseService.updateDisputeByCaseId(formData.imgCaseId, "image", image);
				LOGGER.log(Level.INFO, String.join(ConstantsEnum.LOG_SEPERATOR.getMessage(), "ImgCaseID: {0}",
						formData.imgCaseId, authToken.getRealm()));

				// call Masking micro service using Asynch
				try {
					ImageMasking imgMasking = new ImageMasking(originalFileName, fileName, id, category, bytesBase64,
							imageOwner, LocalDateTime.now(), source, size, type, formData.imgCaseId);

					CompletionStage<String> encodedString = maskingServiceClient.maskingImage(imgMasking);
					encodedString.thenAccept(response -> {
						// Handle the response here
						if (StringUtils.isNotBlank(response)) {
							LOGGER.log(Level.INFO,
									String.join(ConstantsEnum.LOG_SEPERATOR.getMessage(), "ImgCaseID: {0}",
											ConstantsEnum.LOG_SEPERATOR.getMessage(),
											"ImageId: {1} has been Successfully Masked", formData.imgCaseId, id,
											authToken.getRealm()));
						} else {
							LOGGER.log(Level.INFO, String.join(ConstantsEnum.LOG_SEPERATOR.getMessage(),
									"ImgCaseID: {0}", ConstantsEnum.LOG_SEPERATOR.getMessage(),
									"ImageId: {1} has not been Masked", formData.imgCaseId, id, authToken.getRealm()));
						}
					});

				} catch (Exception e) {
					LOGGER.log(Level.SEVERE, String.join(ConstantsEnum.LOG_SEPERATOR.getMessage(),
							"Exception occured in Masking()", ExceptionUtil.generateStackTrace(e)));
				}

			}
		} catch (Exception e) {
			// throw new CustomException("error in upload");
			LOGGER.log(Level.SEVERE, String.join(ConstantsEnum.LOG_SEPERATOR.getMessage(),
					"Exception occured in saveFiles()", ExceptionUtil.generateStackTrace(e)));
			return false;

		}
		return true;
	}



```


```java
// implement api to post by caseNumber
@GET
@Path("getImage/{caseNumber}")  
@Consumes(MediaType.MULTIPART_FORM_DATA)  
public Response uploadImages(ImageUpload formData) {
```







```java

@POST  
@Path("uploadImg/")  
@Consumes(MediaType.MULTIPART_FORM_DATA)  
public Response uploadImages(ImageUpload formData) {
	Image image = new Image();  
	Optional<Map<String, List<String>>> errorMessage = validateFiles(formData, Image);
	

}
// send the list of files
	public List<String> validateFiles(List<FileUpload> uploadedFiled, Image image) {
		List<String> errorFileList = new ArrayList<>();
		for (FileUpload file : uploadedFiled) {
			boolean isValid = checkFile(file.fileName(), new File(file.filePath().toUri()), file.contentType(), image);
			if (!isValid) {
				errorFileList.add(file.fileName());
			}
		}
		return errorFileList;
	}


public Optional<Map<String, List<String>>> fileValidation(List<FileRequest> fileReq, Image image) {  
    Map<String, List<String>> errorMap = new HashMap<>();  
    List<String> errorFileList = new ArrayList<>(); 
    for (FileRequest fileObj : fileReq) {  
       File file = fileUploadService.convertToFile(fileObj);  
  
       // Check file is valid or not  
       boolean isValid = fileUploadService.checkFile(file, fileObj.getType(), image);  
       if (!isValid) {  
          errorFileList.add(fileObj.getName());  
          errorMap.put(fileObj.getCategory(), errorFileList);  
       }  
    }  
    return Optional.ofNullable(errorMap);  
}

public File convertToFile(FileRequest fileRequest) {  
  
    // Decode Base64 string to byte array  
    byte[] decodedBytes = Base64.getDecoder().decode(fileRequest.getFile());  
    // String path = String.join("/", filepath, fileRequest.getName());  
    File file = new File(fileRequest.getName());  
    try {  
       OutputStream outputStream = new BufferedOutputStream(new FileOutputStream(file));  
       outputStream.write(decodedBytes);  
       outputStream.close();  
    } catch (Exception e) {  
       LOGGER.log(Level.SEVERE, "File not found exception occured" + e.getStackTrace());  
    }  
    return file;  
}

public boolean checkFile(File file, String type, Image image) {  
    boolean isValid = false;  
  
    // file size check  
    LOGGER.log(Level.INFO, "File size check for {0}", file.getName());  
    float size = (float) file.length();  
    image.setSize(file.length());  
    float sizeInMb = size / 1000000;  
    if (sizeInMb > 5) {  
       LOGGER.log(Level.WARNING, "Large file {0} of size {1}", new Object[] { file.getName(), sizeInMb });  
       return isValid;  
    }  
    LOGGER.log(Level.INFO, "File size check passed for {0}, size {1} MB",  
          new Object[] { file.getName(), sizeInMb });  
  
    // extension check  
    LOGGER.log(Level.INFO, "Extension check for {0}", file.getName());  
    List<String> allowedExtensions = Arrays.asList("png", "jpg", "jpeg", "tiff", "tif", "pdf");  
    if (!allowedExtensions.contains(type)) {  
       LOGGER.log(Level.WARNING, "Invalid extension for {0}", file.getName());  
       return isValid;  
    }  
    LOGGER.log(Level.INFO, "Extension check passed for {0}", file.getName());  
  
    // File magic check  
    LOGGER.log(Level.INFO, "File magic check for {0}", file.getName());  
    byte[] data = new byte[4];  
    try {  
       FileInputStream is = new FileInputStream(file);  
       is.read(data, 0, 4);  
       is.close();  
    } catch (FileNotFoundException e) {  
       LOGGER.log(Level.SEVERE, ErrorMessage.FILE_NOT_FOUND.getError() + e.getStackTrace());  
    } catch (IOException e) {  
       LOGGER.log(Level.SEVERE, ErrorMessage.IOEXCEPTION.getError() + e.getStackTrace());  
    }  
    /** png */  
    if (Byte.toUnsignedInt(data[0]) == 0x89 && Byte.toUnsignedInt(data[1]) == 0x50) {  
       LOGGER.log(Level.INFO, "File magic for {0}, expected 89 and 50, found {1} and {2}",  
             new Object[] { file.getName(), Integer.toHexString(Byte.toUnsignedInt(data[0])),  
                   Integer.toHexString(Byte.toUnsignedInt(data[1])) });  
       if (ConstantsEnum.PNG.getMessage().equals(type)) {  
          isValid = true;  
       }  
    }  
    /** jpeg jpg */  
    else if (Byte.toUnsignedInt(data[0]) == 0xFF && Byte.toUnsignedInt(data[1]) == 0xD8) {  
       LOGGER.log(Level.INFO, "file magic for {0}, expected ff and d8, found {1} and {2}",  
             new Object[] { file.getName(), Integer.toHexString(Byte.toUnsignedInt(data[0])),  
                   Integer.toHexString(Byte.toUnsignedInt(data[1])) });  
       if (ConstantsEnum.PNG.getMessage().equals(type) || ConstantsEnum.JPEG.getMessage().equals(type) || ConstantsEnum.JPG.getMessage().equals(type)) {  
          isValid = true;  
       }  
    }  
    /** pdf */  
    else if (Byte.toUnsignedInt(data[0]) == 0x25 && Byte.toUnsignedInt(data[1]) == 0x50) {  
       LOGGER.log(Level.INFO, "file magic for {0}, expected 25 and 50, found {1} and {2}",  
             new Object[] { file.getName(), Integer.toHexString(Byte.toUnsignedInt(data[0])),  
                   Integer.toHexString(Byte.toUnsignedInt(data[1])) });  
       if (ConstantsEnum.PDF.getMessage().equals(type)) {  
          isValid = true;  
       }  
    }  
    /** tiff */  
    else if (Byte.toUnsignedInt(data[0]) == 0x49 && Byte.toUnsignedInt(data[1]) == 0x49) {  
       LOGGER.log(Level.INFO, "file magic for {0}, expected 49 and 49, found {1} and {2}",  
             new Object[] { file.getName(), Integer.toHexString(Byte.toUnsignedInt(data[0])),  
                   Integer.toHexString(Byte.toUnsignedInt(data[1])) });  
       if (ConstantsEnum.TIFF.getMessage().equals(type)) {  
          isValid = true;  
       }  
    } else {  
       LOGGER.log(Level.WARNING, "Invalid file magic for {0}, found {1} and {2}",  
             new Object[] { file.getName(), Integer.toHexString(Byte.toUnsignedInt(data[0])),  
                   Integer.toHexString(Byte.toUnsignedInt(data[1])) });  
       return isValid;  
    }  
    LOGGER.log(Level.INFO, "File magic check passed for {0}", file.getName());  
    boolean isDeleted = file.delete();  
    if (!isDeleted) {  
       LOGGER.log(Level.WARNING, "Failed to delete temporary file for validation");  
    }  
    return isValid;  
}

//Save files
public boolean saveFiles(ImageUpload formData, List<FileUpload> uploadedFiled, String category, String type,
			String source) {
		try {
			for (FileUpload file : uploadedFiled) {
				String originalFileName = file.fileName();
				String contentType = file.contentType();
				File myObj = new File(file.filePath().toUri());
				long size = file.size();
				String fileName = getFileName(formData, originalFileName);
				LOGGER.log(Level.INFO, String.join(ConstantsEnum.LOG_SEPERATOR.getMessage(), "Generated file name: ",
						fileName, formData.imgCaseId, authToken.getRealm()));
				String imageOwner = (source.equals("portal")) ? authToken.getUserName() : "customer";
				byte[] imageBytes = new byte[(int) myObj.length()];
				imageBytes = Files.readAllBytes(myObj.toPath());
				String bytesBase64 = Base64.getEncoder().encodeToString(imageBytes);
				int length = 10;
				String id = authToken.getRealm() + "_" + RandomStringUtils.random(length, true, false);
				LOGGER.log(Level.INFO, String.join(ConstantsEnum.LOG_SEPERATOR.getMessage(), "Generated ID for image: ",
						id, formData.imgCaseId, authToken.getRealm()));

				Image image = new Image(originalFileName, fileName, id, category, bytesBase64, imageOwner,
						LocalDateTime.now(), source, size, type, false);
				caseService.updateDisputeByCaseId(formData.imgCaseId, "image", image);
				LOGGER.log(Level.INFO, String.join(ConstantsEnum.LOG_SEPERATOR.getMessage(), "ImgCaseID: {0}",
						formData.imgCaseId, authToken.getRealm()));

				// call Masking micro service using Asynch
				try {
					ImageMasking imgMasking = new ImageMasking(originalFileName, fileName, id, category, bytesBase64,
							imageOwner, LocalDateTime.now(), source, size, type, formData.imgCaseId);

					CompletionStage<String> encodedString = maskingServiceClient.maskingImage(imgMasking);
					encodedString.thenAccept(response -> {
						// Handle the response here
						if (StringUtils.isNotBlank(response)) {
							LOGGER.log(Level.INFO,
									String.join(ConstantsEnum.LOG_SEPERATOR.getMessage(), "ImgCaseID: {0}",
											ConstantsEnum.LOG_SEPERATOR.getMessage(),
											"ImageId: {1} has been Successfully Masked", formData.imgCaseId, id,
											authToken.getRealm()));
						} else {
							LOGGER.log(Level.INFO, String.join(ConstantsEnum.LOG_SEPERATOR.getMessage(),
									"ImgCaseID: {0}", ConstantsEnum.LOG_SEPERATOR.getMessage(),
									"ImageId: {1} has not been Masked", formData.imgCaseId, id, authToken.getRealm()));
						}
					});

				} catch (Exception e) {
					LOGGER.log(Level.SEVERE, String.join(ConstantsEnum.LOG_SEPERATOR.getMessage(),
							"Exception occured in Masking()", ExceptionUtil.generateStackTrace(e)));
				}

			}
		} catch (Exception e) {
			// throw new CustomException("error in upload");
			LOGGER.log(Level.SEVERE, String.join(ConstantsEnum.LOG_SEPERATOR.getMessage(),
					"Exception occured in saveFiles()", ExceptionUtil.generateStackTrace(e)));
			return false;

		}
		return true;
	}


```