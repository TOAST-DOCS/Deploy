## Dev Tool > Deploy > API Guide

The API allows user-configured HTTP Request to upload binaries. 

## Ver 1.0

* Major Improvements 
    * API of the REST-format 
    * Diversified resultCode 

| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.cloud.toast.com/api/v1.0/projects/{appkey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} |

### Parameter

| Name | Type | Description | Value | Required |
| ---- | ---- | ----------- | ----- | -------- |
| applicationType | String | Type of an artifact | client or server | true |
| version | String | Version of uploaded binary; replaced with timestamp, if left empty | - | false |
| description | String | Description of a binary | - | false |
| osType | String | OS information of a binary file if the applicationType is client | iOS, Android, or others | false |
| binaryFile | File | Binary file object | - | true |
| metaFile | File | plist file object for iOS | - | false |
| fix | Boolean | Fix or not information, if the applicationType is client | true/false | false |

### Sample Request For cUrl

``` java
curl -X POST \
  https://api-tcd.cloud.toast.com/api/v1.0/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} \
  -H 'content-type: multipart/form-data' \
  -F 'binaryFile=@ojdbc14.jar' \
  -F 'applicationType=server' \
  -F 'description=A binary file of some kind'
```

> To call Deploy API with curl, add the --tlsv1.2 option.

### Sample Request For JAVA

Below is an example of uploading binaries via API by using HttpClient library (httpclient 4.3.6). 

``` java

String appKey = "xxxxxxxxx";
String artifactId = "1";

String binaryName = "ojdbc14.jar";
String binaryGroupKey = "4";
String filePath = "/xxx/xxxx" + binaryName;
FileBody binaryFile = new FileBody(new File(filePath));

StringBody applicationType = new StringBody("server", ContentType.TEXT_PLAIN);
StringBody description = new StringBody("A binary file of some kind", ContentType.TEXT_PLAIN);

String requestUri = "https://api-tcd.cloud.toast.com/api/v1.0/projects/" + appKey + "/artifacts/" + artifactId + "/binary-group/" + binaryGroupKey;

HttpPost method = new HttpPost(requestUri);

HttpEntity reqEntity = MultipartEntityBuilder.create()
		.addPart("binaryFile", binaryFile)
		.addPart("applicationType", applicationType)
		.addPart("description", description)
        .build();

method.setEntity(reqEntity);

try {
	CloseableHttpClient httpclient = HttpClients.createDefault();
	ResponseHandler<String> responseHandler = new ResponseHandler<String>() {
		public String handleResponse(final HttpResponse response) throws IOException {
			int status = response.getStatusLine().getStatusCode();
			if (status == HttpStatus.SC_OK) {
				HttpEntity entity = response.getEntity();
				return entity != null ? EntityUtils.toString(entity) : null;
			} else {
				throw new ClientProtocolException("Unexpected response status: " + status);
			}
		}
	};

	String responseBody = httpclient.execute(method, responseHandler);
	if (responseBody == null) {
		throw new Exception("Empty Response Data Error");
	}

	if (responseBody.contains("false")) {
		throw new Exception("Upload Fail Result Code : " + responseBody);
	}
	logger.debug("Result : " + responseBody);
} catch (Exception e) {
	logger.error("Fail to request : " + e.getMessage(), e);
}
```

### Response (json)

| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | boolean | Uploading result | True or false |
| resultCode | String | Message for uploading result | See [Error Codes](/Dev%20Tool/Deploy/en/error-code/) |
| downloadUrl | String | Downloading path for uploaded binaries | Download is available in the path |
| binaryKey | String | Key of the uploaded binary | - |

### Response Sample

``` json
Result: {
	"header": {
		"isSuccessful": true,
		"serverTime": 1533526167415,
		"resultCode": "SUCCESS",
		"resultMessage": "success"
	},
	"body": {
		"downloadUrl": "https://api-tcd.cloud.toast.com/api/v1.0/projects/{appkey}/artifacts/{artifactId}/binary-group/{binaryGroupKey}/binaries/{uploadedBinaryKey}",
		"binaryKey": "{uploadedBinaryKey}"
	}
}
```

## Previous Version 

| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.cloud.toast.com/api/binary/upload/artifact/{artifactId} |

### Parameter

| Name | Type | Description | Value | Required |
| ---- | ---- | ----------- | ----- | -------- |
| appKey | String | Appkey of TOAST Cloud; available on the Deploy page | - | True |
| applicationType | String | Type of an artifcat | Client or server | True |
| binaryGroupKey | long | Group key of a binary | Specified as default group, if left empty | False |
| version | String | Version of uploaded binary; replaced with timestamp, if left empty | - | False |
| description | String | Description of a binary | - | False |
| osType | String | OS information of a binary file, if the applicationType is client | iOS, Android, or others | False |
| binaryFile | File | Binary file object | - | True |
| metaFile | File | plist file object for iOS | - | False |
| fix | Boolean | Fix or not information, if the applicationType is client | true/false | False |

### Sample Request For JAVA

Below is an example of uploading binaries via API by using HttpClient library (httpclient 4.3.6). 

``` java
String artifactId = "1";
String binaryName = "ojdbc14.jar";
String filePath = "/xxx/xxxx" + binaryName;
FileBody binaryFile = new FileBody(new File(filePath));

StringBody appKey = new StringBody("xxxxxxxxx", ContentType.TEXT_PLAIN);
StringBody applicationType = new StringBody("server", ContentType.TEXT_PLAIN);
StringBody description = new StringBody("A binary file of some kind", ContentType.TEXT_PLAIN);

String requestUri = "https://api-tcd.cloud.toast.com/api/binary/upload/artifact/" + artifactId;

HttpPost method = new HttpPost(requestUri);

HttpEntity reqEntity = MultipartEntityBuilder.create()
		.addPart("binaryFile", binaryFile)
		.addPart("appKey", appKey)
		.addPart("applicationType", applicationType)
		.addPart("description", description)
        .build();

method.setEntity(reqEntity);

try {
	CloseableHttpClient httpclient = HttpClients.createDefault();
	ResponseHandler<String> responseHandler = new ResponseHandler<String>() {
		public String handleResponse(final HttpResponse response) throws IOException {
			int status = response.getStatusLine().getStatusCode();
			if (status == HttpStatus.SC_OK) {
				HttpEntity entity = response.getEntity();
				return entity != null ? EntityUtils.toString(entity) : null;
			} else {
				throw new ClientProtocolException("Unexpected response status: " + status);
			}
		}
	};

	String responseBody = httpclient.execute(method, responseHandler);
	if (responseBody == null) {
		throw new Exception("Empty Response Data Error");
	}

	if (responseBody.contains("false")) {
		throw new Exception("Upload Fail Result Code : " + responseBody);
	}
	logger.debug("Result : " + responseBody);
} catch (Exception e) {
	logger.error("Fail to request : " + e.getMessage(), e);
}
```

### Response (json)

| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccess | boolean | Uploading result | True or false |
| result | String | Message for uploading result | isSuccess : True<br>\- Key information of uploaded binaries<br>isSuccess : False<br>\- INAVLID\_INFORMATION: Invalid parameter information <br>\- BINARY\_UPLOAD\_ERROR: Error occurred during uploading binaries <br>\- ALREADY\_UPLOADED\_VERSION: Conflicts between binary versions |

### Response Sample

``` json
{
	"isSuccess" : true,
	"result" : 111
}
```