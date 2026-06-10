## Dev Tools > Deploy > API v2.1 Guide
Deploy provides APIs for binary upload, binary download, deployment execution, and information retrieval. You can configure and send HTTP requests directly.

### Basic Information
#### Endpoint
```text
https://api-tcd.nhncloudservice.com
```

#### API Request HTTP Header
```
X-NHN-AUTHORIZATION: Bearer {issued token}
```

#### Authentication and Authorization
Deploy uses User Access Key tokens for authentication and authorization when calling the API.
A User Access Key token is a temporary, Bearer-type access token issued from a User Access Key.
For more information on issuing and using User Access Key tokens, see [User Access Key Token](/nhncloud/en/public-api/user-access-key-token).

Deploy APIs use role-based access control (RBAC).<br>
Users must have the **Deploy ADMIN role** or **Deploy VIEWER role** to use the APIs.

#### Available APIs
| Method | URI | Description |
| ------ | --- | --- |
| POST | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} | Binary upload API |
| GET | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey}/binaries/{binaryKey} | Binary download API |
| POST | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy | Deployment execution API |
| GET | /api/v2.1/projects/{appKey}/artifacts | Artifact list retrieval API |
| GET | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/server-groups | Server group list retrieval API |
| GET | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-groups | Binary group list retrieval API |
| GET | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/deploy-histories | Deployment history retrieval API |
| GET | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-groups/{binaryGroupKey}/binaries | Binary list retrieval API |

#### API Request Path Variables
| Value | Type | Description |
| --- | --- | --- |
| appKey | String | Appkey of the Deploy service to use |
| artifactId | Number | ID of the artifact to use |
| binaryGroupKey | Number | Key of the binary group to upload the binary to |
| binaryKey | Number | Binary key, issued upon upload |
| serverGroupId | Number | ID of the server group to deploy to |

### Upload Binary
#### Version 2.1
| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} |

##### Parameter
| Name | Type | Description | Value | Required |
| ---- | ---- | ----------- | ----- | -------- |
| applicationType | String | Type of the artifact | client or server | true |
| version | String | Version of the binary to upload; replaced with a timestamp if not entered (max 100 characters) | - | false |
| description | String | Description of the binary | - | false |
| osType | String | OS information of the binary file when applicationType is client | iOS, Android, or etc | false |
| binaryFile | File | Binary file object | - | true |
| metaFile | File | plist file object for iOS | - | false |
| fix | Boolean | Whether the binary is fixed when applicationType is client | true/false | false |

##### Sample Request For cURL
``` java
curl -X POST \
  https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} \
  -H 'content-type: multipart/form-data' \
  -F 'binaryFile=@ojdbc14.jar' \
  -F 'applicationType=server' \
  -F 'description=A binary file of some kind'
```

##### Response (JSON)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | boolean | Upload result | true or false |
| resultCode | String | Upload result message | See [Error Codes](/Dev%20Tools/Deploy/ko/error-code/) |
| downloadUrl | String | Download path of the uploaded binary | The binary can be downloaded via this path |
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
		"downloadUrl": "https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey}/binaries/{uploadedBinaryKey}",
		"binaryKey": "{uploadedBinaryKey}"
	}
}
```

### Download Binary
You can download a binary file using the download path received in the response from the binary upload API.

#### Version 2.1
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey}/binaries/{binaryKey} |

##### Sample Request For cURL
``` java
curl -X GET \
  https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey}/binaries/{binaryKey} \
  -H 'X-NHN-AUTHORIZATION: Bearer {token}' \
  -o {file name to save}
```

##### Response
* Downloads the binary file.
* Content-Type: `application/octet-stream`

### Execute Deployment
* This API is used for deployment execution.
* The deployment execution API is only available when the artifact `Command Type` is Cloud Agent. (Not available for SSH.)
* In v2.1, deployment execution is also supported for Autoscale server groups.

#### Version 2.1
| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy |

##### Parameter (Body)
| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| targetServerHostnames | String | Hostnames of servers within the server group to selectively deploy to, separated by commas (,) (enter all if deploying to the entire server group) | hostname1, hostname2, hostname3 (if not specified, deploys to all servers in the server group) | false | All servers in the server group |
| concurrentNum | Number | Number of deployments to run in parallel | A value of 0 or greater; if 0, all servers in the server group run simultaneously | false | 0 |
| nextWhenFail | Boolean | Whether to proceed to the next server if a scenario fails | true/false | false | false (stops execution) |
| deployNote | String | Additional information to include with the deployment | | false | |
| async | Boolean | Receives a response without waiting for the deployment result | true/false | false | false |
| scenarioIds | String | Scenario IDs to execute | Scenario IDs within the server group, separated by commas (,) (if not specified, all mapped scenario IDs) | false (however, true for standard deployment - only 1) | All mapped scenario IDs if not specified |

##### Sample Request For cURL
``` java
curl --location 'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy' \
--header 'X-NHN-AUTHORIZATION: Bearer {token}' \
--header 'Content-Type: application/json' \
--data '{
	"targetServerHostnames" : "{ex. server1,server2}",
	"concurrentNum" : 1,
	"nextWhenFail" : false,
	"deployNote" : "{Note content}",
	"async" : false,
	"scenarioIds" : "{ex. 1,2}"
}'
```

##### Response (JSON)
* The `isSuccessful` field indicates whether the deployment execution call was successful. Use the `deployStatus` field to check the deployment result (success or failure).
* If an Autoscale server group is deployed, the body value exists in List format.

| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | Whether the deployment execution was successful | true or false |
| resultCode | String | Deployment execution result message | See [Error Codes](/Dev%20Tools/Deploy/ko/error-code/) |
| deployStatus | String | Deployment status | success, fail, or deploying (when async option is true) |
| deployResult | List | Deployment result per server | - hostname: Hostname of the deployment target (instance ID)<br>- status: Deployment result<br>- taskResult: Information for each task in the deployment scenario |
| deployResultLocation | String | Link to the Deploy service project where the deployment was executed | Access the Deploy service project console via this link |

##### Response Sample
``` json
{
    "header": {
        "isSuccessful": true,
        "serverTime": 1707278725614,
        "resultCode": "SUCCESS",
        "resultMessage": "success"
    },
    "body": [
		{
			"deployKey": 192349,
			"deployStatus": "{deployment status}",
			"deployResult": [
				{
					"deployKey": 192349,
					"hostname": "{hostname}",
					"status": "{deployment result}",
					"taskResult": [
						"..."
					]
				}
			],
			"deployResultLocation": "{link to the Deploy service project where the deployment was executed}"
		}
	]
}
```

### List Artifacts
* This API retrieves a list of artifacts in a project.

#### Version 2.1
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts |

##### Parameter (Query String)
| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| artifactName | String | Search by artifact name | Name of the artifact to search for | false | - |

##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts?artifactName={artifactName}' \
  -H 'X-NHN-AUTHORIZATION: Bearer {token}'
```

##### Response (JSON)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | Whether the request was successful | true or false |
| resultCode | String | Request result message | See [Error Codes](/Dev%20Tools/Deploy/ko/error-code/) |
| artifacts | List | Artifact list | See below |

**artifacts**

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | Number | Artifact ID |
| name | String | Artifact name |
| applicationType | String | Application type (server/client) |
| description | String | Description |
| createDate | Date | Creation date |
| lastDeployDate | Date | Last deployment date |

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
        "artifacts": [
            {
                "id": 1,
                "name": "my-artifact",
                "applicationType": "server",
                "description": "Server artifact",
                "createDate": "2025-01-01T00:00:00+09:00",
                "lastDeployDate": "2025-03-01T12:00:00+09:00"
            }
        ]
    }
}
```

### List Server Groups
* This API retrieves a list of server groups belonging to an artifact.

#### Version 2.1
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/server-groups |

##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/server-groups' \
  -H 'X-NHN-AUTHORIZATION: Bearer {token}'
```

##### Response (JSON)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | Whether the request was successful | true or false |
| resultCode | String | Request result message | See [Error Codes](/Dev%20Tools/Deploy/ko/error-code/) |
| serverGroups | List | Server group list | See below |

**serverGroups**

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | Number | Server group ID |
| name | String | Server group name |
| description | String | Description |
| osType | String | OS type (LINUX/WINDOWS) |
| serverCount | Number | Number of servers |

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
        "serverGroups": [
            {
                "id": 1,
                "name": "my-server-group",
                "description": "Production server group",
                "osType": "LINUX",
                "serverCount": 3
            }
        ]
    }
}
```

### List Binary Groups
* This API retrieves a list of binary groups belonging to an artifact.

#### Version 2.1
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-groups |

##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-groups' \
  -H 'X-NHN-AUTHORIZATION: Bearer {token}'
```

##### Response (JSON)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | Whether the request was successful | true or false |
| resultCode | String | Request result message | See [Error Codes](/Dev%20Tools/Deploy/en/error-code/) |
| binaryGroups | List | Binary group list | See below |

**binaryGroups**

| Name | Type | Description |
| ---- | ---- | ----------- |
| key | Number | Binary group key |
| name | String | Binary group name |
| description | String | Description |
| regionCode | String | Region code |
| createDate | Date | Creation date |

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
        "binaryGroups": [
            {
                "key": 1,
                "name": "my-binary-group",
                "description": "Production binary group",
                "regionCode": "KR1",
                "createDate": "2025-01-01T00:00:00+09:00"
            }
        ]
    }
}
```

### List Deployment History
* This API retrieves the deployment history of an artifact.
* The query period cannot exceed 1 year.

#### Version 2.1
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/deploy-histories |

##### Parameter (Query String)
| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| serverGroupId | Number | Server group ID | If 0, retrieves all artifacts | false | 0 |
| deploymentYearFrom | String | Query start date | yyyy-MM-dd format | false | Current date - 1 month |
| deploymentYearTo | String | Query end date | yyyy-MM-dd format | false | Current date |
| pageNum | Number | Page number | A value of 1 or greater | false | 1 |
| pageSize | Number | Number of items per page | A value of 1 or greater | false | 20 |

##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/deploy-histories?serverGroupId=0&deploymentYearFrom=2025-01-01&deploymentYearTo=2025-03-01&pageNum=1&pageSize=20' \
  -H 'X-NHN-AUTHORIZATION: Bearer {token}'
```

##### Response (JSON)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | Whether the request was successful | true or false |
| resultCode | String | Request result message | See [Error Codes](/Dev%20Tools/Deploy/ko/error-code/) |
| totalCount | Number | Total count | - |
| deployHistories | List | Deployment history list | See below |

**deployHistories**

| Name | Type | Description |
| ---- | ---- | ----------- |
| deployKey | Number | Deployment key |
| scenarioName | String | Scenario name |
| serverGroupName | String | Server group name |
| serverGroupId | Number | Server group ID |
| binaryVersion | String | Binary version |
| executeDate | Date | Execution date |
| executeUser | String | Executed by |
| totalResult | String | Execution result (SUCCESS/FAIL/RUNNING) |

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
        "totalCount": 1,
        "deployHistories": [
            {
                "deployKey": 192349,
                "scenarioName": "Deployment scenario",
                "serverGroupName": "Production server group",
                "serverGroupId": 1,
                "binaryVersion": "1.0.0",
                "executeDate": "2025-03-01T12:00:00+09:00",
                "executeUser": "user@example.com",
                "totalResult": "SUCCESS"
            }
        ]
    }
}
```

### List Binaries
* This API retrieves a list of binaries belonging to a binary group.

#### Version 2.1
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-groups/{binaryGroupKey}/binaries |

##### Parameter (Query String)
| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| pageNum | Number | Page number | A value of 1 or greater | false | 1 |
| pageSize | Number | Number of items per page | A value of 1 or greater | false | 20 |
| sortKey | String | Sort key | VERSION, BINARY_KEY, UPLOAD_DATE | false | UPLOAD_DATE |
| sortDirection | String | Sort direction | ASC, DESC | false | DESC |
| keyword | String | Binary version search keyword | Keyword to search for | false | - |

##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-groups/{binaryGroupKey}/binaries?pageNum=1&pageSize=20&sortKey=UPLOAD_DATE&sortDirection=DESC' \
  -H 'X-NHN-AUTHORIZATION: Bearer {token}'
```

##### Response (JSON)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | Whether the request was successful | true or false |
| resultCode | String | Request result message | See [Error Codes](/Dev%20Tools/Deploy/ko/error-code/) |
| totalCount | Number | Total count | - |
| binaries | List | Binary list | See below |

**binaries**

| Name | Type | Description |
| ---- | ---- | ----------- |
| binaryKey | Number | Binary key |
| version | String | Binary version |
| binaryName | String | Binary file name |
| binarySize | Number | Binary file size (bytes) |
| uploadDate | Date | Upload date |
| uploader | String | Uploaded by |
| description | String | Description |

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
        "totalCount": 1,
        "binaries": [
            {
                "binaryKey": 100,
                "version": "1.0.0",
                "binaryName": "app-1.0.0.jar",
                "binarySize": 10485760,
                "uploadDate": "2025-03-01T12:00:00+09:00",
                "uploader": "user@example.com",
                "description": "Release binary"
            }
        ]
    }
}
```
