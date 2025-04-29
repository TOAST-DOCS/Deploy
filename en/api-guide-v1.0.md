## Dev Tools > Deploy > API v1.0 Guide
Deploy provides an API for uploading binaries, running deployments, and configuring HTTP requests directly by the user.

### Basic Information
#### Endpoint
```text
https://api-tcd.nhncloudservice.com
```

#### Types of Provided APIs
| Method | URI | Descriptions |
| ------ | --- | --- |
| POST | /api/v1.0/projects/{appkey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} | Binary upload API |
| POST | /api/v1.0/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/scenario/{scenarioId}/deploy | Deployment execution API |

#### API request path variables
| Value | Type | Description |
| --- | --- | --- |
| appKey | String | The app key for the Deploy service to use |
| artifactId | Number | The ID of the artifact to use |
| binaryGroupKey | Number | The binary group key to upload the binary to |
| serverGroupId | Number | Server group ID for the deployment target |
| scenarioId | Number | The scenario ID to deploy |

### Binary Upload
#### Version 1.0
| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v1.0/projects/{appkey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} |

#### Parameter
| Name | Type | Description | Value | Required |
| ---- | ---- | ----------- | ----- | -------- |
| applicationType | String | Type of an artifact | client or server | true |
| version | String | Version of uploaded binary; replaced with timestamp, if left empty | - | false |
| description | String | Description of a binary | - | false |
| osType | String | OS information of a binary file if the applicationType is client | iOS, Android, or others | false |
| binaryFile | File | Binary file object | - | true |
| metaFile | File | plist file object for iOS | - | false |
| fix | Boolean | Fix or not information, if the applicationType is client | true/false | false |

##### Sample Request For cURL
``` java
curl -X POST \
  https://api-tcd.nhncloudservice.com/api/v1.0/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} \
  -H 'content-type: multipart/form-data' \
  -F 'binaryFile=@ojdbc14.jar' \
  -F 'applicationType=server' \
  -F 'description=A binary file of some kind'
```

#### Sample Request For JAVA
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

String requestUri = "https://api-tcd.nhncloudservice.com/api/v1.0/projects/" + appKey + "/artifacts/" + artifactId + "/binary-group/" + binaryGroupKey;

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

##### Response(json)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | boolean | Uploading result | True or false |
| resultCode | String | Message for uploading result | See [Error Codes](/Dev%20Tools/Deploy/en/error-code/) |
| downloadUrl | String | Downloading path for uploaded binaries | Download is available in the path |
| binaryKey | String | Key of the uploaded binary | - |

##### Response Sample
``` json
{
	"header": {
		"isSuccessful": true,
		"serverTime": 1533526167415,
		"resultCode": "SUCCESS",
		"resultMessage": "success"
	},
	"body": {
		"downloadUrl": "https://api-tcd.nhncloudservice.com/api/v1.0/projects/{appkey}/artifacts/{artifactId}/binary-group/{binaryGroupKey}/binaries/{uploadedBinaryKey}",
		"binaryKey": "{uploadedBinaryKey}"
	}
}
```

#### Previous Version 
| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/binary/upload/artifact/{artifactId} |

##### Parameter
| Name | Type | Description | Value | Required |
| ---- | ---- | ----------- | ----- | -------- |
| appKey | String | Appkey of NHN Cloud; available on the Deploy page | - | True |
| applicationType | String | Type of an artifcat | Client or server | True |
| binaryGroupKey | long | Group key of a binary | Specified as default group, if left empty | False |
| version | String | Version of uploaded binary; replaced with timestamp, if left empty | - | False |
| description | String | Description of a binary | - | False |
| osType | String | OS information of a binary file, if the applicationType is client | iOS, Android, or others | False |
| binaryFile | File | Binary file object | - | True |
| metaFile | File | plist file object for iOS | - | False |
| fix | Boolean | Fix or not information, if the applicationType is client | true/false | False |

##### Sample Request For JAVA
Below is an example of uploading binaries via API by using HttpClient library (httpclient 4.3.6). 

``` java
String artifactId = "1";
String binaryName = "ojdbc14.jar";
String filePath = "/xxx/xxxx" + binaryName;
FileBody binaryFile = new FileBody(new File(filePath));

StringBody appKey = new StringBody("xxxxxxxxx", ContentType.TEXT_PLAIN);
StringBody applicationType = new StringBody("server", ContentType.TEXT_PLAIN);
StringBody description = new StringBody("A binary file of some kind", ContentType.TEXT_PLAIN);

String requestUri = "https://api-tcd.nhncloudservice.com/api/binary/upload/artifact/" + artifactId;

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

##### Response(json)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccess | boolean | Uploading result | True or false |
| result | String | Message for uploading result | isSuccess : True<br>\- Key information of uploaded binaries<br>isSuccess : False<br>\- INAVLID\_INFORMATION: Invalid parameter information <br>\- BINARY\_UPLOAD\_ERROR: Error occurred during uploading binaries <br>\- ALREADY\_UPLOADED\_VERSION: Conflicts between binary versions |

##### Response Sample
``` json
{
	"isSuccess" : true,
	"result" : 111
}
```

### Run Deployment
* API for running deployments.
* The artifact `Command Type`provides a deployment execution API only for Cloud Agent (not for SSH).

#### Version 1.0
| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v1.0/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/scenario/{scenarioId}/deploy |

##### Header
| Name | Description | Value |
| --- | --- | --- |
| Content-Type | ConentType | application/json |
| X-TC-AUTHENTICATION-ID | User Access Key ID in API Security Settings menu | {id} |
| X-TC-AUTHENTICATION-SECRET | Secret Access Key in API Security Settings menu | {key} |

##### Parameter (Body)
| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| targetServerHostnames | String | Hostnames of servers within the server group that are optionally targeted for deployment, separated by ',' (enter all if server group-wide) | hostname1, hostname2, hostname3 (if none, deploy all servers in the server group) | false | All servers included in a server group |
| concurrentNum | Number | Number of deployments to run in parallel | A value greater than or equal to 0, otherwise the entire server group runs concurrently | false | 0 |
| nextWhenFail | Boolean | Whether to run the following servers if the scenario fails | true/false | false | false (stop execution) |
| deployNote | String | Additional information to fill out at deployment time |  | false |  |
| async | Boolean | Get a response without waiting for deployment results | true/false | false | false |

##### Sample Request For cURL
``` java
curl --location 'https://api-tcd.nhncloudservice.com/api/v1.0/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/scenario/{scenarioId}/deploy' \
--header 'X-TC-AUTHENTICATION-ID: {ID}' \
--header 'X-TC-AUTHENTICATION-SECRET: {Key}' \
--header 'Content-Type: application/json' \
--data '{
	"targetServerHostnames" : "{ex. server1,server2}",
	"concurrentNum" : 1,
	"nextWhenFail" : false,
	"deployNote" : "{Note content}",
	"async" : false
}'
```

##### Response (json)
* The isSuccessful item is a field value that determines whether the deployment execution call was successful or not, and you should check the deployment result (successful, failed) via the deployStatus item.

| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | Deployment run successful or not | True or false |
| resultCode | String | Deployment run result messages | See [Error Codes](/Dev%20Tools/Deploy/en/error-code/) |
| deployStatus | String | Deployment status | success, fail, or deploying (with async option true) |
| deployResult | List | Deployment results per server | - hostname: Deployment target hostname (instance ID)<br>- status: Deployment results<br>- taskResult: Information for each task in the deployment scenario |
| deployResultLocation | String | Link for Deploy service project that was executed | Follow this link to access the Deploy Service project console |

##### Response Sample
``` json
{
    "header": {
        "isSuccessful": true,
        "serverTime": 1707278725614,
        "resultCode": "SUCCESS",
        "resultMessage": "success"
    },
    "body": {
        "deployKey": 52876,
        "deployStatus": "{Deployment status}",
        "deployResult": [
            {
                "deployKey": 52876,
                "hostname": "{Hostname}",
                "status": "{Deployment result}",
                "taskResult": [
					"..."
                ]
            }
        ],
        "deployResultLocation": "{Link to the Deploy service project where the deployment ran}"
    }
}
```