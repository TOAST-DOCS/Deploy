## Dev Tools > Deploy > API v2.0 Guide
A user directly configures the HTTP Request to provide an API for the distribution execution in DePloy.

### Basic Info
#### Endpoint
```text
https://api-tcd.nhncloudservice.com
```

#### Provided APIs Types
| Method | URI | Description |
| ------ | --- | --- |
| POST | /api/v2.0/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy | Deplyment execution API |

#### API Request Path Variables
| Value | Type | Description |
| --- | --- | --- |
| appKey | String | Appkey of the Deploy service to use |
| artifactId | Number | ID of the artifact to use |
| binaryGroupKey | Number | Binary group key to upload binary |
| serverGroupId | Number | Server group ID to be deplyed |

### 
* An API to execute deployment.
* Artifact 'Command Type' provides a deployment execution API for Cloud Agent (it is not provided for SSH.)
* Autoscale server group is also deployable in v2.0.

#### Version 2.0
| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy |

##### Header
| Name | Description | Value |
| --- | --- | --- |
| Content-Type | ConentType | application/json |
| X-TC-AUTHENTICATION-ID | User Access Key ID of the API security setting menu  | {id} |
| X-TC-AUTHENTICATION-SECRET | Secret Access Key of the API security setting menu | {key} |

##### Parameter (Body)
| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| targetServerHostnames | String | The host name of the server divided into ',', which is selectively deployed within the server group (all entered the server group) | hostname1, hostname2, hostname3 (If not present, deploy all servers within the server group) | false | Full server included in the server group |
| concurrentNum | Number | Number of deployments to run in parallel | A value greater than or equal to 0, if 0, the entire server group runs concurrently | false | 0 |
| nextWhenFail | Boolean | Whether to run the next server if the scenario fails | true/false | false | false (stop execution) |
| deployNote | String | Additional info provided during distribution |  | false |  |
| async | Boolean | Get a response without waiting for the deployment results | true/false | false | false |
| scenarioIds | String | Scenario scenarioId to be executed | Scenario IDs separated by commas within the server group (if none, all mapped ScenarioIDs) | false(However, only true - 1 in case of general Deploy) | If not present, the entire mapped ScenarioID |

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
	"deployNote" : "{Note content}",
	"async" : false,
	"scenarioIds" : "{ex. 1,2}"
}'
```

##### Response(json)
* The isSuccessful item is a field value that checks whether the deployment execution call was successful, and you must check the deployment result (success, failure) through the deployStatus item.
* When deploying an Autoscale server group, the body value exists in the form of a list.

| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | Deployment success status | true or false |
| resultCode | String | Deployment execution result message | Refer to the [Error code](/Dev%20Tools/Deploy/ko/error-code/) |
| deployStatus | String | Deployment status | success, fail or deploying(if the async option is true) |
| deployResult | List | Deployment result by server | - hostname: deployment target host name (instance ID)<br>- status: deployment result<br>- taskResult: Information for each task within the deployment scenario |
| deployResultLocation | String | Link to the deployed service project | Accessible to the Deploy service project console through this link |

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
			"deployStatus": "{Deployment status}",
			"deployResult": [
				{
					"deployKey": 192349,
					"hostname": "{Host name}",
					"status": "{Deployment result}",
					"taskResult": [
						"..."
					]
				}
			],
			"deployResultLocation": "{Link to the deployed service project excuted}"
		}
	]
}
```