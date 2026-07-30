<!-- pre-align:aligned sig=cb53ec3bd366 -->

<a id="dev-tools-deploy-api-v20-guide"></a>

## Dev Tools > Deploy > API v2.0 Guide
Deploy provides APIs for deployment execution and information retrieval. You can configure and send HTTP requests directly.

<a id="basic-information"></a>

### Basic Information { #basic-information }
<a id="endpoint"></a>

#### Endpoint
```text
https://api-tcd.nhncloudservice.com
```

<a id="available-apis"></a>

#### Available APIs
| Method | URI | Description |
| ------ | --- | --- |
| POST | /api/v2.0/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy | Deployment execution API |
| GET | /api/v2.0/projects/{appKey}/artifacts | Artifact list retrieval API |
| GET | /api/v2.0/projects/{appKey}/artifacts/{artifactId}/server-groups | Server group list retrieval API |
| GET | /api/v2.0/projects/{appKey}/artifacts/{artifactId}/binary-groups | Binary group list retrieval API |
| GET | /api/v2.0/projects/{appKey}/artifacts/{artifactId}/deploy-histories | Deployment history retrieval API |
| GET | /api/v2.0/projects/{appKey}/artifacts/{artifactId}/binary-groups/{binaryGroupKey}/binaries | Binary list retrieval API |

<a id="api-request-path-variables"></a>

#### API Request Path Variables
| Value | Type | Description |
| --- | --- | --- |
| appKey | String | Appkey of the Deploy service to use |
| artifactId | Number | ID of the artifact to use |
| binaryGroupKey | Number | Key of the binary group to upload the binary to |
| serverGroupId | Number | ID of the server group to deploy to |

<a id="execute-deployment"></a>

### Execute Deployment { #execute-deployment }
* This API is used for deployment execution.
* The deployment execution API is only available when the artifact `Command Type` is Cloud Agent. (Not available for SSH.)
* In v2.0, deployment execution is also supported for Autoscale server groups.
* The deployment execution API uses role-based access control (RBAC). Only users with the **Deploy ADMIN** role can use the deployment execution API.

<a id="version-20"></a>

#### Version 2.0
| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy |

##### Header
| Name | Description | Value |
| --- | --- | --- |
| Content-Type | Content type | application/json |
| X-TC-AUTHENTICATION-ID | User Access Key ID in the API security settings menu | {id} |
| X-TC-AUTHENTICATION-SECRET | Secret Access Key in the API security settings menu | {key} |

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
curl --location 'https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy' \
--header 'X-TC-AUTHENTICATION-ID: {ID}' \
--header 'X-TC-AUTHENTICATION-SECRET: {Key}' \
--header 'Content-Type: application/json' \
--data '{
	"targetServerHostnames" : "{ex. server1,server2}",
	"concurrentNum" : 1,
	"nextWhenFail" : false,
	"deployNote" : "{content for Note}",
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
| resultCode | String | Deployment execution result message | See [Error Codes](/Dev%20Tools/Deploy/en/error-code/) |
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

<a id="list-artifacts"></a>

### List Artifacts { #list-artifacts }
* This API retrieves a list of artifacts in a project.

<a id="list-artifacts-version-20"></a>

#### Version 2.0
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts |

##### Header
| Name | Description | Value |
| --- | --- | --- |
| X-TC-AUTHENTICATION-ID | User Access Key ID in the API security settings menu | {id} |
| X-TC-AUTHENTICATION-SECRET | Secret Access Key in the API security settings menu | {key} |

##### Parameter (Query String)
| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| artifactName | String | Search by artifact name | Name of the artifact to search for | false | - |

##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts?artifactName={artifactName}' \
  -H 'X-TC-AUTHENTICATION-ID: {ID}' \
  -H 'X-TC-AUTHENTICATION-SECRET: {Key}'
```

##### Response (JSON)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | Whether the request was successful | true or false |
| resultCode | String | Request result message | See [Error Codes](/Dev%20Tools/Deploy/en/error-code/) |
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

<a id="list-server-groups"></a>

### List Server Groups { #list-server-groups }
* This API retrieves a list of server groups belonging to an artifact.

<a id="list-server-groups-version-20"></a>

#### Version 2.0
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/server-groups |

##### Header
| Name | Description | Value |
| --- | --- | --- |
| X-TC-AUTHENTICATION-ID | User Access Key ID in the API security settings menu | {id} |
| X-TC-AUTHENTICATION-SECRET | Secret Access Key in the API security settings menu | {key} |

##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/server-groups' \
  -H 'X-TC-AUTHENTICATION-ID: {ID}' \
  -H 'X-TC-AUTHENTICATION-SECRET: {Key}'
```

##### Response (JSON)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | Whether the request was successful | true or false |
| resultCode | String | Request result message | See [Error Codes](/Dev%20Tools/Deploy/en/error-code/) |
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

<a id="list-binary-groups"></a>

### List Binary Groups { #list-binary-groups }
* This API retrieves a list of binary groups belonging to an artifact.

<a id="list-binary-groups-version-20"></a>

#### Version 2.0
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/binary-groups |

##### Header
| Name | Description | Value |
| --- | --- | --- |
| X-TC-AUTHENTICATION-ID | User Access Key ID in the API security settings menu | {id} |
| X-TC-AUTHENTICATION-SECRET | Secret Access Key in the API security settings menu | {key} |

##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/binary-groups' \
  -H 'X-TC-AUTHENTICATION-ID: {ID}' \
  -H 'X-TC-AUTHENTICATION-SECRET: {Key}'
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

<a id="list-deployment-history"></a>

### List Deployment History { #list-deployment-history }
* This API retrieves the deployment history of an artifact.
* The query period cannot exceed 1 year.

<a id="list-deployment-history-version-20"></a>

#### Version 2.0
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/deploy-histories |

##### Header
| Name | Description | Value |
| --- | --- | --- |
| X-TC-AUTHENTICATION-ID | User Access Key ID in the API security settings menu | {id} |
| X-TC-AUTHENTICATION-SECRET | Secret Access Key in the API security settings menu | {key} |

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
  'https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/deploy-histories?serverGroupId=0&deploymentYearFrom=2025-01-01&deploymentYearTo=2025-03-01&pageNum=1&pageSize=20' \
  -H 'X-TC-AUTHENTICATION-ID: {ID}' \
  -H 'X-TC-AUTHENTICATION-SECRET: {Key}'
```

##### Response (JSON)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | Whether the request was successful | true or false |
| resultCode | String | Request result message | See [Error Codes](/Dev%20Tools/Deploy/en/error-code/) |
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

<a id="list-binaries"></a>

### List Binaries { #list-binaries }
* This API retrieves a list of binaries belonging to a binary group.

<a id="list-binaries-version-20"></a>

#### Version 2.0
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/binary-groups/{binaryGroupKey}/binaries |

##### Header
| Name | Description | Value |
| --- | --- | --- |
| X-TC-AUTHENTICATION-ID | User Access Key ID in the API security settings menu | {id} |
| X-TC-AUTHENTICATION-SECRET | Secret Access Key in the API security settings menu | {key} |

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
  'https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/binary-groups/{binaryGroupKey}/binaries?pageNum=1&pageSize=20&sortKey=UPLOAD_DATE&sortDirection=DESC' \
  -H 'X-TC-AUTHENTICATION-ID: {ID}' \
  -H 'X-TC-AUTHENTICATION-SECRET: {Key}'
```

##### Response (JSON)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | Whether the request was successful | true or false |
| resultCode | String | Request result message | See [Error Codes](/Dev%20Tools/Deploy/en/error-code/) |
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
