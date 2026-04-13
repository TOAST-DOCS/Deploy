## Dev Tools > Deploy > API v2.1 가이드
Deploy에서는 바이너리 업로드, 바이너리 다운로드, 배포 실행, 정보 조회를 위한 API를 제공합니다. 사용자가 HTTP 요청을 직접 구성하여 사용할 수 있습니다.

### 기본 정보
#### 엔드포인트
```text
https://api-tcd.nhncloudservice.com
```

#### API 요청 HTTP 헤더
```
X-NHN-AUTHORIZATION: Bearer {발급 받은 토큰}
```

#### 인증 및 권한
Deploy는 API 호출 시 인증/인가를 위해 User Access Key 토큰을 사용합니다.
User Access Key 토큰은 User Access Key를 기반으로 발급되는 Bearer 타입의 일시적 액세스 토큰입니다.
User Access Key 토큰 발급 및 사용에 대한 자세한 내용은 [User Access Key 토큰](/nhncloud/ko/public-api/user-access-key-token)을 참고하세요.

Deploy API는 역할 기반 접근 제어(RBAC)를 사용합니다.<br>
사용자는 API 사용을 위해 **Deploy ADMIN 역할** 또는 **Deploy VIEWER 역할**을 소유해야 합니다.

#### 제공하는 API 종류
| Method | URI | 설명 |
| ------ | --- | --- |
| POST | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} | 바이너리 업로드 API |
| GET | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey}/binaries/{binaryKey} | 바이너리 다운로드 API |
| POST | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy | 배포 실행 API |
| GET | /api/v2.1/projects/{appKey}/artifacts | 아티팩트 목록 조회 API |
| GET | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/server-groups | 서버 그룹 목록 조회 API |
| GET | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-groups | 바이너리 그룹 목록 조회 API |
| GET | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/deploy-histories | 배포 이력 조회 API |
| GET | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-groups/{binaryGroupKey}/binaries | 바이너리 목록 조회 API |

#### API 요청 경로 변수
| 값 | 타입 | 설명 |
| --- | --- | --- |
| appKey | String | 사용할 Deploy 서비스의 앱키 |
| artifactId | Number | 사용할 아티팩트의 아이디 |
| binaryGroupKey | Number | 바이너리를 업로드할 바이너리 그룹 키 |
| binaryKey | Number | 바이너리 키, 업로드 시 발급 |
| serverGroupId | Number | 배포 대상이 되는 서버 그룹 아이디 |

### 바이너리 업로드
#### Version 2.1
| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} |

##### Parameter
| Name | Type | Description | Value | Required |
| ---- | ---- | ----------- | ----- | -------- |
| applicationType | String | 아티팩트의 타입 | client 또는 server | true |
| version | String | 업로드하는 바이너리의 버전, 미입력 시 timestamp로 대체(최대 100자) | - | false |
| description | String | 바이너리의 설명 | - | false |
| osType | String | applicationType이 client인 경우 바이너리 파일의 OS 정보 | iOS, Android 또는 etc | false |
| binaryFile | File | 바이너리 파일 객체 | - | true |
| metaFile | File | iOS인 경우 plist 파일 객체 | - | false |
| fix | Boolean | applicationType이 client인 경우 Fix 여부 정보 | true/false | false |

##### Sample Request For cURL
``` java
curl -X POST \
  https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} \
  -H 'content-type: multipart/form-data' \
  -F 'binaryFile=@ojdbc14.jar' \
  -F 'applicationType=server' \
  -F 'description=A binary file of some kind'
```

##### Response(json)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | boolean | 업로드 결과 | true 또는 false |
| resultCode | String | 업로드 결과 메시지 | [오류 코드](/Dev%20Tools/Deploy/ko/error-code/) 참고 |
| downloadUrl | String | 업로드 바이너리의 다운로드 경로 | 해당 경로로 다운로드 가능 |
| binaryKey | String | 업로드한 바이너리의 키 | - |

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

### 바이너리 다운로드
바이너리 업로드 API의 응답으로 전달받은 다운로드 경로로 바이너리 파일을 다운로드할 수 있습니다.

#### Version 2.1
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey}/binaries/{binaryKey} |

##### Sample Request For cURL
``` java
curl -X GET \
  https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey}/binaries/{binaryKey} \
  -H 'X-NHN-AUTHORIZATION: Bearer {token}' \
  -o {저장할 파일명}
```

##### Response
* 바이너리 파일을 다운로드합니다.
* Content-Type: `application/octet-stream`

### 배포 실행
* 배포 실행을 위한 API입니다.
* 아티팩트 `Command Type`이 Cloud Agent인 경우에만 배포 실행 API를 제공합니다(SSH의 경우 제공되지 않습니다.).
* v2.1에서는 Autoscale 서버 그룹도 배포 실행 가능합니다.

#### Version 2.1
| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy |

##### Parameter(Body)
| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| targetServerHostnames | String | 서버 그룹 내에서 선택적으로 배포 대상이 되는 쉼표(,)로 구분된 서버의 호스트명(서버 그룹 전체인 경우 모두 입력) | hostname1, hostname2, hostname3(없을 시 서버 그룹 내 서버 전체 배포) | false | 서버 그룹에 포함된 전체 서버 |
| concurrentNum | Number | 병렬로 실행할 배포 수 | 0 이상의 값, 0인 경우 서버 그룹 전체 동시 실행 | false | 0 |
| nextWhenFail | Boolean | 시나리오 실패 시 다음 서버 실행 여부 | true/false | false | false(실행 중단) |
| deployNote | String | 배포 시 작성하는 부가 정보 |  | false |  |
| async | Boolean | 배포 결과를 기다리지 않고 응답을 받음 | true/false | false | false |
| scenarioIds | String | 실행할 시나리오 scenarioId | 서버 그룹 내에서 쉼표(,)로 구분된 시나리오 ID(없을 시 매핑되어 있는 ScenarioID 전체) | false(단, 일반 Deploy 시 true - 1개만) | 없을 시 매핑되어 있는 ScenarioID 전체 |

##### Sample Request For cURL
``` java
curl --location 'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy' \
--header 'X-NHN-AUTHORIZATION: Bearer {token}' \
--header 'Content-Type: application/json' \
--data '{
	"targetServerHostnames" : "{ex. server1,server2}",
	"concurrentNum" : 1,
	"nextWhenFail" : false,
	"deployNote" : "{Note 내용}",
	"async" : false,
	"scenarioIds" : "{ex. 1,2}"
}'
```

##### Response(json)
* isSuccessful 항목은 배포 실행 호출에 성공 여부를 확인하는 필드값이며 deployStatus 항목을 통해 배포 결과(성공, 실패)를 확인해야 합니다.
* Autoscale 서버 그룹을 배포했을 경우 Body 값이 List 형태로 존재합니다.

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

### 아티팩트 목록 조회
* 프로젝트의 아티팩트 목록을 조회하는 API입니다.

#### Version 2.1
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts |

##### Parameter(Query String)
| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| artifactName | String | 아티팩트 이름 검색 | 검색할 아티팩트 이름 | false | - |

##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts?artifactName={artifactName}' \
  -H 'X-NHN-AUTHORIZATION: Bearer {token}'
```

##### Response(json)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | 요청 성공 여부 | true 또는 false |
| resultCode | String | 요청 결과 메시지 | [오류 코드](/Dev%20Tools/Deploy/ko/error-code/) 참고 |
| artifacts | List | 아티팩트 목록 | 아래 항목 참고 |

**artifacts**

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | Number | 아티팩트 ID |
| name | String | 아티팩트 이름 |
| applicationType | String | 애플리케이션 유형(server/client) |
| description | String | 설명 |
| createDate | Date | 생성일 |
| lastDeployDate | Date | 마지막 배포일 |

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
                "description": "서버 아티팩트",
                "createDate": "2025-01-01T00:00:00+09:00",
                "lastDeployDate": "2025-03-01T12:00:00+09:00"
            }
        ]
    }
}
```

### 서버 그룹 목록 조회
* 아티팩트에 속한 서버 그룹 목록을 조회하는 API입니다.

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

##### Response(json)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | 요청 성공 여부 | true 또는 false |
| resultCode | String | 요청 결과 메시지 | [오류 코드](/Dev%20Tools/Deploy/ko/error-code/) 참고 |
| serverGroups | List | 서버 그룹 목록 | 아래 항목 참고 |

**serverGroups**

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | Number | 서버 그룹 ID |
| name | String | 서버 그룹 이름 |
| description | String | 설명 |
| osType | String | OS 유형(LINUX/WINDOWS) |
| serverCount | Number | 서버 수 |

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
                "description": "운영 서버 그룹",
                "osType": "LINUX",
                "serverCount": 3
            }
        ]
    }
}
```

### 바이너리 그룹 목록 조회
* 아티팩트에 속한 바이너리 그룹 목록을 조회하는 API입니다.

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

##### Response(json)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | 요청 성공 여부 | true 또는 false |
| resultCode | String | 요청 결과 메시지 | [오류 코드](/Dev%20Tools/Deploy/ko/error-code/) 참고 |
| binaryGroups | List | 바이너리 그룹 목록 | 아래 항목 참고 |

**binaryGroups**

| Name | Type | Description |
| ---- | ---- | ----------- |
| key | Number | 바이너리 그룹 키 |
| name | String | 바이너리 그룹 이름 |
| description | String | 설명 |
| regionCode | String | 리전 코드 |
| createDate | Date | 생성일 |

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
                "description": "운영 바이너리 그룹",
                "regionCode": "KR1",
                "createDate": "2025-01-01T00:00:00+09:00"
            }
        ]
    }
}
```

### 배포 이력 조회
* 아티팩트의 배포 이력을 조회하는 API입니다.
* 조회 기간은 최대 1년을 초과할 수 없습니다.

#### Version 2.1
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/deploy-histories |

##### Parameter(Query String)
| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| serverGroupId | Number | 서버 그룹 ID | 0인 경우 아티팩트 전체 조회 | false | 0 |
| deploymentYearFrom | String | 조회 시작일 | yyyy-MM-dd 형식 | false | 현재일 - 1개월 |
| deploymentYearTo | String | 조회 종료일 | yyyy-MM-dd 형식 | false | 현재일 |
| pageNum | Number | 페이지 번호 | 1 이상의 값 | false | 1 |
| pageSize | Number | 페이지당 건수 | 1 이상의 값 | false | 20 |

##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/deploy-histories?serverGroupId=0&deploymentYearFrom=2025-01-01&deploymentYearTo=2025-03-01&pageNum=1&pageSize=20' \
  -H 'X-NHN-AUTHORIZATION: Bearer {token}'
```

##### Response(json)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | 요청 성공 여부 | true 또는 false |
| resultCode | String | 요청 결과 메시지 | [오류 코드](/Dev%20Tools/Deploy/ko/error-code/) 참고 |
| totalCount | Number | 전체 건수 | - |
| deployHistories | List | 배포 이력 목록 | 아래 항목 참고 |

**deployHistories**

| Name | Type | Description |
| ---- | ---- | ----------- |
| deployKey | Number | 배포 키 |
| scenarioName | String | 시나리오 이름 |
| serverGroupName | String | 서버 그룹 이름 |
| serverGroupId | Number | 서버 그룹 ID |
| binaryVersion | String | 바이너리 버전 |
| executeDate | Date | 실행일 |
| executeUser | String | 실행자 |
| totalResult | String | 실행 결과(SUCCESS/FAIL/RUNNING) |

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
                "scenarioName": "배포 시나리오",
                "serverGroupName": "운영 서버 그룹",
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

### 바이너리 목록 조회
* 바이너리 그룹에 속한 바이너리 목록을 조회하는 API입니다.

#### Version 2.1
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-groups/{binaryGroupKey}/binaries |

##### Parameter(Query String)
| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| pageNum | Number | 페이지 번호 | 1 이상의 값 | false | 1 |
| pageSize | Number | 페이지당 건수 | 1 이상의 값 | false | 20 |
| sortKey | String | 정렬 기준 | VERSION, BINARY_KEY, UPLOAD_DATE | false | UPLOAD_DATE |
| sortDirection | String | 정렬 방향 | ASC, DESC | false | DESC |
| keyword | String | 바이너리 버전 검색 키워드 | 검색할 키워드 | false | - |

##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-groups/{binaryGroupKey}/binaries?pageNum=1&pageSize=20&sortKey=UPLOAD_DATE&sortDirection=DESC' \
  -H 'X-NHN-AUTHORIZATION: Bearer {token}'
```

##### Response(json)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | 요청 성공 여부 | true 또는 false |
| resultCode | String | 요청 결과 메시지 | [오류 코드](/Dev%20Tools/Deploy/ko/error-code/) 참고 |
| totalCount | Number | 전체 건수 | - |
| binaries | List | 바이너리 목록 | 아래 항목 참고 |

**binaries**

| Name | Type | Description |
| ---- | ---- | ----------- |
| binaryKey | Number | 바이너리 키 |
| version | String | 바이너리 버전 |
| binaryName | String | 바이너리 파일명 |
| binarySize | Number | 바이너리 파일 크기(bytes) |
| uploadDate | Date | 업로드일 |
| uploader | String | 업로더 |
| description | String | 설명 |

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
                "description": "릴리즈 바이너리"
            }
        ]
    }
}
```
