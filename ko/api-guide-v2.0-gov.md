## Dev Tools > Deploy > API v2.0 가이드
Deploy에서는 사용자가 HTTP Request를 직접 구성하여 배포 실행을 위한 API를 제공합니다.

### 기본 정보
#### 엔드포인트
```text
https://api-tcd.gov-nhncloudservice.com
```

#### 제공하는 API 종류
| Method | URI | 설명 |
| ------ | --- | --- |
| POST | /api/v2.0/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy | 배포 실행 API |

#### API 요청 경로 변수
| 값 | 타입 | 설명 |
| --- | --- | --- |
| appKey | String | 사용할 Deploy 서비스의 앱키 |
| artifactId | Number | 사용할 아티팩트의 아이디 |
| binaryGroupKey | Number | 바이너리를 업로드할 바이너리 그룹 키 |
| serverGroupId | Number | 배포 대상이 되는 서버 그룹 아이디 |

### 배포 실행
* 배포 실행을 위한 API입니다.
* 아티팩트 `Command Type`이 Cloud Agent의 경우만 배포 실행 API를 제공합니다.(SSH의 경우 제공되지 않습니다.)
* v2.0에서는 Autoscale 서버 그룹도 배포 실행 가능합니다.

#### Version 2.0
| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.gov-nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy |

##### Header
| Name | Description | Value |
| --- | --- | --- |
| Content-Type | ConentType | application/json |
| X-TC-AUTHENTICATION-ID | API 보안 설정 메뉴의 User Access Key ID | {id} |
| X-TC-AUTHENTICATION-SECRET | API 보안 설정 메뉴의 Secret Access Key | {key} |

##### Parameter (Body)
| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| targetServerHostnames | String | 서버 그룹 내에서 선택적으로 배포 대상이 되는 ','으로 구분된 서버의 호스트명(서버 그룹 전체인 경우 모두 입력) | hostname1, hostname2, hostname3(없을 시 서버 그룹 내 서버 전체 배포) | false | 서버 그룹에 포함된 전체 서버 |
| concurrentNum | Number | 병렬로 실행할 배포 수 | 0 이상의 값, 0인 경우 서버 그룹 전체 동시 실행 | false | 0 |
| nextWhenFail | Boolean | 시나리오 실패 시 다음 서버 실행 여부 | true/false | false | false (실행 중단) |
| deployNote | String | 배포 시 작성하는 부가 정보 |  | false |  |
| async | Boolean | 배포 결과를 기다리지 않고 응답을 받음 | true/false | false | false |
| scenarioIds | String | 실행할 시나리오 scenarioId | 서버 그룹 내에서 ','으로 구분된 시나리오 ID(없을 시 매핑되어 있는 ScenarioID 전체) | false(단, 일반 Deploy 시 true - 1개만) | 없을 시 매핑되어 있는 ScenarioID 전체 |

##### Sample Request For cURL
``` java
curl --location 'https://api-tcd.gov-nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy' \
--header 'X-TC-AUTHENTICATION-ID: {ID}' \
--header 'X-TC-AUTHENTICATION-SECRET: {Key}' \
--header 'Content-Type: application/json' \
--data '{
	"targetServerHostnames" : "{ex. server1,server2}",
	"concurrentNum" : 1,
	"nextWhenFail" : false,
	"deployNote" : "{Note 내용}",
	"async" : false,
	"scenarioIds" : "{ex. 1}"
}'
```

##### Response(json)
* isSuccessful 항목은 배포 실행 호출에 성공했는지 유무를 확인하는 필드값이며 deployStatus 항목을 통해 배포 결과(성공, 실패)를 확인해야 합니다.
* Autoscale 서버 그룹을 배포 했을 경우 body 값이 List 형태로 존재합니다.

| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | 배포 실행 성공 여부 | true 또는 false |
| resultCode | String | 배포 실행 결과 메시지 | [오류 코드](/Dev%20Tools/Deploy/ko/error-code/) 참고 |
| deployStatus | String | 배포 상태 | success, fail 또는 deploying(async 옵션 true일 경우) |
| deployResult | List | 서버별 배포 결과 | - hostname: 배포 대상 호스트명(인스턴스 ID)<br>- status: 배포 결과<br>- taskResult: 배포 시나리오 내 각 태스크별 정보 |
| deployResultLocation | String | 배포 실행된 Deploy 서비스 프로젝트 링크 | 해당 링크로 Deploy 서비스 프로젝트 콘솔 접속 가능 |

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
			"deployStatus": "{배포 상태}",
			"deployResult": [
				{
					"deployKey": 192349,
					"hostname": "{호스트명}",
					"status": "{배포 결과}",
					"taskResult": [
						"..."
					]
				}
			],
			"deployResultLocation": "{배포 실행된 Deploy 서비스 프로젝트 링크}"
		}
	]
}
```