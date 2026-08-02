<!-- pre-align:aligned sig=28e392d2a044 -->

<a id="dev-tools-deploy-api-v21-guide"></a>
## Dev Tools > Deploy > API v2.1 Guide { #dev-tools-deploy-api-v21-guide }
Deploy provides APIs for binary upload, binary download, deployment execution, and information retrieval. You can configure and send HTTP requests directly.

<a id="basic-information"></a>
### Basic Information { #basic-information }
<a id="endpoint"></a>
#### Endpoint
```text
https://api-tcd.nhncloudservice.com
```

<a id="api-request-http-header"></a>
#### API Request HTTP Header
```
X-NHN-AUTHORIZATION: Bearer {issued token}
```

<a id="authentication-and-authorization"></a>
#### Authentication and Authorization
Deploy uses User Access Key tokens for authentication and authorization when calling the API.
A User Access Key token is a temporary, Bearer-type access token issued from a User Access Key.
For more information on issuing and using User Access Key tokens, see [User Access Key Token](/nhncloud/en/public-api/user-access-key-token).

Deploy APIs use role-based access control (RBAC).<br>
Users must have the **Deploy ADMIN role** or **Deploy VIEWER role** to use the APIs.

<a id="available-apis"></a>
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
| GET | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/server-groups/{serverGroupId}/scenarios | List Scenarios API |

<a id="api-request-path-variables"></a>
#### API Request Path Variables
| Value | Type | Description |
| --- | --- | --- |
| appKey | String | Appkey of the Deploy service to use |
| artifactId | Number | ID of the artifact to use |
| binaryGroupKey | Number | Key of the binary group to upload the binary to |
| binaryKey | Number | Binary key, issued upon upload |
| serverGroupId | Number | ID of the server group to deploy to |

<a id="upload-binary"></a>
### Upload Binary { #upload-binary }
<a id="version-21"></a>
#### Version 2.1
| HTTP Method | POST |
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
| resultCode | String | Upload result message | See [Error Codes](/Dev%20Tools/Deploy/en/error-code/) |
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

<a id="download-binary"></a>
### Download Binary { #download-binary }
You can download a binary file using the download path received in the response from the binary upload API.

<a id="download-binary-version-21"></a>
#### Version 2.1
| HTTP Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey}/binaries/{binaryKey} |

##### Sample Request For cURL
``` java
curl -X GET \
  https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey}/binaries/{binaryKey} \
  -H 'X-NHN-AUTHORIZATION: Bearer {token}' \
  -o {output filename}
```

##### Response
* Downloads a binary file.
* Content-Type: `application/octet-stream`

<a id="execute-deployment"></a>
### Execute Deployment { #execute-deployment }
* This API is used for deployment execution.
* The deployment execution API is only available when the artifact `Command Type` is Cloud Agent. (Not available for SSH.)
* In v2.1, deployment execution is also supported for Autoscale server groups.

<a id="execute-deployment-version-21"></a>
#### Version 2.1
| HTTP Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy |
##### Parameter(Body)

| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| targetServerHostnames | String | Hostnames of servers, separated by commas (,), that are selectively targeted for deployment within the server group (enter all if targeting the entire server group) | hostname1, hostname2, hostname3 (deploys to all servers in the server group if not specified) | false | All servers in the server group |
| concurrentNum | Number | Number of deployments to run in parallel | A value of 0 or greater; if 0, full concurrency across the server group | false | 0 |
| nextWhenFail | Boolean | Whether to run the next server when a scenario fails | true/false | false | false (execution stopped) |
| deployNote | String | Additional information written when deploying |  | false |  |
| async | Boolean | Receives a response without waiting for the deployment result | true/false | false | false |
| scenarioIds | String | Scenario scenarioId to run | Scenario IDs separated by comma (,) within the Server Group (if not specified, all mapped ScenarioIDs) | false (however, true for a general Deploy - only 1) | All mapped ScenarioIDs if not specified |
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

##### Response(json)
* The `isSuccessful` field indicates whether the execute deployment call was successful, and you must check the deployment result (success or failure) through the `deployStatus` field.
* If you deployed an Auto Scaling Server Group, the Body value exists as a List.

| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | Whether the deployment execution was successful | "true" or "false" |
| resultCode | String | Result message for deployment execution | See [Error Code](/Dev%20Tools/Deploy/en/error-code/) |
| deployStatus | String | Deployment status | success, fail, or deploying (when the async option is true) |
| deployResult | List | Deployment results by server | - hostname: Hostname of the deployment target (Instance ID)<br>- status: Deployment Result<br>- taskResult: Information on each task in the deployment scenario |
| deployResultLocation | String | Link to the Deploy service project where deployment was run | You can access the Deploy service project console using this link. |
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
			"deployStatus": "{Deployment Status}",
			"deployResult": [
				{
					"deployKey": 192349,
					"hostname": "{hostname}",
					"status": "{Deployment Result}",
					"taskResult": [
						"..."
					]
				}
			],
			"deployResultLocation": "{Link to the Deploy service project where the deployment was executed}"
		}
	]
}
```

<a id="list-artifacts"></a>
### List Artifacts { #list-artifacts }
* This API retrieves a list of artifacts in a project.

<a id="list-artifacts-version-21"></a>
#### Version 2.1
| HTTP Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts |
##### Parameter(Query String)

| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| artifactName | String | Search Artifact Name | Artifact name to search for | false | - |
##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts?artifactName={artifactName}' \
  -H 'X-NHN-AUTHORIZATION: Bearer {token}'
```

##### Response(json)

| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | Request success | `true` or `false` |
| resultCode | String | Request result message | See [Error Code](/Dev%20Tools/Deploy/en/error-code/) |
| artifacts | List | Artifact list | Refer to item below |
**artifacts**

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | Number | Artifact ID |
| name | String | Artifact name |
| applicationType | String | Application type (server/client) |
| description | String | Description |
| createDate | Date | Created on |
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

<a id="list-server-groups-version-21"></a>
#### Version 2.1
| HTTP Method | GET |
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
| isSuccessful | Boolean | Request success | `true` or `false` |
| resultCode | String | Request result message | See [Error Code](/Dev%20Tools/Deploy/en/error-code/) |
| serverGroups | List | Server Group List | See the items below |
**serverGroups**

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | Number | Server Group ID |
| name | String | Server Group Name |
| description | String | Description |
| osType | String | OS type (LINUX/WINDOWS) |
| serverCount | Number | Number of Servers |
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

<a id="list-binary-groups-version-21"></a>
#### Version 2.1
| HTTP Method | GET |
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
| isSuccessful | Boolean | Request success | `true` or `false` |
| resultCode | String | Request result message | See [Error Code](/Dev%20Tools/Deploy/en/error-code/) |
| binaryGroups | List | Binary Group List | See the items below |
**binaryGroups**

| Name | Type | Description |
| ---- | ---- | ----------- |
| key | Number | Binary group key |
| name | String | Binary Group Name |
| description | String | Description |
| regionCode | String | Region code |
| createDate | Date | Created on |
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

<a id="list-deployment-history-version-21"></a>
#### Version 2.1
| HTTP Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/deploy-histories |
##### Parameter(Query String)

| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| serverGroupId | Number | Server Group ID | If 0, retrieves all artifacts. | false | 0 |
| deploymentYearFrom | String | Query start date | yyyy-MM-dd format | false | Current date - 1 month |
| deploymentYearTo | String | Query end date | yyyy-MM-dd format | false | Current date |
| pageNum | Number | Page No. | A value of at least 1 | false | 1 |
| pageSize | Number | Items per page | A value of 1 or greater | false | 20 |
##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/deploy-histories?serverGroupId=0&deploymentYearFrom=2025-01-01&deploymentYearTo=2025-03-01&pageNum=1&pageSize=20' \
  -H 'X-NHN-AUTHORIZATION: Bearer {token}'
```

##### Response (JSON)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | Request success | `true` or `false` |
| resultCode | String | Request result message | See [Error Code](/Dev%20Tools/Deploy/en/error-code/) |
| totalCount | Number | Total count | - |
| deployHistories | List | Deployment history list | See the items below |
**deployHistories**

| Name | Type | Description |
| ---- | ---- | ----------- |
| deployKey | Number | Deploy Key |
| scenarioName | String | Scenario Name |
| serverGroupName | String | Server Group Name |
| serverGroupId | Number | Server Group ID |
| binaryVersion | String | Binary version |
| executeDate | Date | Executed Date |
| executeUser | String | Executed by |
| totalResult | String | Result (SUCCESS/FAIL/RUNNING) |
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
                "scenarioName": "Deployment Scenario",
                "serverGroupName": "Production Server Group",
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

<a id="list-binaries-version-21"></a>
#### Version 2.1
| HTTP Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-groups/{binaryGroupKey}/binaries |
##### Parameter(Query String)

| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| pageNum | Number | Page No. | A value of at least 1 | false | 1 |
| pageSize | Number | Items per page | A value of 1 or greater | false | 20 |
| sortKey | String | Sort by | VERSION, BINARY_KEY, UPLOAD_DATE | false | UPLOAD_DATE |
| sortDirection | String | Sort order | ASC, DESC | false | DESC |
| keyword | String | Search keyword for binary versions | Search keyword | false | - |
##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-groups/{binaryGroupKey}/binaries?pageNum=1&pageSize=20&sortKey=UPLOAD_DATE&sortDirection=DESC' \
  -H 'X-NHN-AUTHORIZATION: Bearer {token}'
```

##### Response (JSON)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | Request success | `true` or `false` |
| resultCode | String | Request result message | See [Error Code](/Dev%20Tools/Deploy/en/error-code/) |
| totalCount | Number | Total count | - |
| binaries | List | Binary list | See items below |
**binaries**

| Name | Type | Description |
| ---- | ---- | ----------- |
| binaryKey | Number | Binary Key |
| version | String | Binary version |
| binaryName | String | Binary File Name |
| binarySize | Number | Binary file size (bytes) |
| uploadDate | Date | Upload date |
| uploader | String | Uploader |
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

<a id="list-scenarios"></a>
### List Scenarios { #list-scenarios }
* This API views the list of scenarios mapped to a server group.

<a id="list-scenarios-version-21"></a>
#### Version 2.1
| HTTP Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/server-groups/{serverGroupId}/scenarios |

##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/server-groups/{serverGroupId}/scenarios' \
  -H 'X-NHN-AUTHORIZATION: Bearer {token}'
```

##### Response(json)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | Whether the request was successful | `true` or `false` |
| resultCode | String | Request result message | See [Error Code](/Dev%20Tools/Deploy/en/error-code/) |
| scenarios | List | List of scenarios | See below |

**scenarios**

| Name | Type | Description |
| ---- | ---- | ----------- |
| scenarioId | Number | Scenario ID |
| scenarioName | String | Scenario name |

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
        "scenarios": [
            {
                "scenarioId": 1,
                "scenarioName": "Deployment Scenario"
            }
        ]
    }
}
```
