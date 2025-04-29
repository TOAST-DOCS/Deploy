## Dev Tools > Deploy > API v1.0 가이드
Deploy에서는 사용자가 HTTP Request를 직접 구성하여 바이너리 업로드, 배포 실행을 위한 API를 제공합니다.

### 기본 정보
#### 엔드포인트
```text
https://api-tcd.gov-nhncloudservice.com
```

#### 제공하는 API 종류
| Method | URI | 설명 |
| ------ | --- | --- |
| POST | /api/v1.0/projects/{appkey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} | 바이너리 업로드 API |
| POST | /api/v1.0/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/scenario/{scenarioId}/deploy | 배포 실행 API |

#### API 요청 경로 변수
| 값 | 타입 | 설명 |
| --- | --- | --- |
| appKey | String | 사용할 Deploy 서비스의 앱키 |
| artifactId | Number | 사용할 아티팩트의 아이디 |
| binaryGroupKey | Number | 바이너리를 업로드할 바이너리 그룹 키 |
| serverGroupId | Number | 배포 대상이 되는 서버 그룹 아이디 |
| scenarioId | Number | 배포할 시나리오의 아이디 |

### 바이너리 업로드
#### Version 1.0
| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.gov-nhncloudservice.com/api/v1.0/projects/{appkey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} |

##### Parameter
| Name | Type | Description | Value | Required |
| ---- | ---- | ----------- | ----- | -------- |
| applicationType | String | 아티팩트의 타입 | client 또는 server | true |
| version | String | 업로드하는 바이너리의 버전, 미입력 시 timestamp로 대체(최대 100자) | - | false |
| description | String | 바이너리의 설명 | - | false |
| osType | String | applicationType이 client인 경우 바이너리 파일의 OS 정보 | iOS, Android 또는 etc | false |
| binaryFile | File | 바이너리 파일 객체 | - | true |
| metaFile | File | iOS인 경우 plist 파일 객체 | - | false |
| fix | Boolean | applicationType이 Client인 경우 Fix 여부 정보 | true/false | false |

##### Sample Request For cURL
``` java
curl -X POST \
  https://api-tcd.gov-nhncloudservice.com/api/v1.0/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} \
  -H 'content-type: multipart/form-data' \
  -F 'binaryFile=@ojdbc14.jar' \
  -F 'applicationType=server' \
  -F 'description=A binary file of some kind'
```

##### Sample Request For JAVA
아래 코드는 HttpClient 라이브러리(httpclient 4.3.6)를 사용하여 API를 통해 바이너리를 업로드하는 코드의 예시입니다.

``` java
String appKey = "xxxxxxxxx";
String artifactId = "1";

String binaryName = "ojdbc14.jar";
String binaryGroupKey = "4";
String filePath = "/xxx/xxxx" + binaryName;
FileBody binaryFile = new FileBody(new File(filePath));

StringBody applicationType = new StringBody("server", ContentType.TEXT_PLAIN);
StringBody description = new StringBody("A binary file of some kind", ContentType.TEXT_PLAIN);

String requestUri = "https://api-tcd.gov-nhncloudservice.com/api/v1.0/projects/" + appKey + "/artifacts/" + artifactId + "/binary-group/" + binaryGroupKey;

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
		"downloadUrl": "https://api-tcd.gov-nhncloudservice.com/api/v1.0/projects/{appkey}/artifacts/{artifactId}/binary-group/{binaryGroupKey}/binaries/{uploadedBinaryKey}",
		"binaryKey": "{uploadedBinaryKey}"
	}
}
```

#### 이전 버전
| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.gov-nhncloudservice.com/api/binary/upload/artifact/{artifactId} |

##### Parameter
| Name | Type | Description | Value | Required |
| ---- | ---- | ----------- | ----- | -------- |
| appKey | String | NHN Cloud 앱키, Deploy 서비스 페이지에서 확인 가능 | - | true |
| applicationType | String | 아티팩트의 타입 | client 또는 server | true |
| binaryGroupKey | long | 바이너리의 그룹 키 | 미입력 시 기본 그룹으로 지정 | false |
| version | String | 업로드하는 바이너리의 버전, 미입력 시 timestamp로 대체 | - | false |
| description | String | 바이너리의 설명 | - | false |
| osType | String | applicationType이 client인 경우 바이너리 파일의 OS 정보 | iOS, Android 또는 etc | false |
| binaryFile | File | 바이너리 파일 객체 | - | true |
| metaFile | File | iOS인 경우 plist 파일 객체 | - | false |
| fix | Boolean | applicationType이 Client인 경우 Fix 여부 정보 | true/false | false |

##### Sample Request For JAVA
아래 코드는 HttpClient 라이브러리(httpclient 4.3.6)를 사용하여 API를 통해 바이너리를 업로드하는 코드의 예시입니다.

``` java
String artifactId = "1";
String binaryName = "ojdbc14.jar";
String filePath = "/xxx/xxxx" + binaryName;
FileBody binaryFile = new FileBody(new File(filePath));

StringBody appKey = new StringBody("xxxxxxxxx", ContentType.TEXT_PLAIN);
StringBody applicationType = new StringBody("server", ContentType.TEXT_PLAIN);
StringBody description = new StringBody("A binary file of some kind", ContentType.TEXT_PLAIN);

String requestUri = "https://api-tcd.gov-nhncloudservice.com/api/binary/upload/artifact/" + artifactId;

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
| isSuccess | boolean | 업로드 결과 | true 또는 false |
| result | String | 업로드 결과 메시지 | isSuccess : true<br>\- 업로드된 바이너리의 키 정보<br>isSuccess : false<br>\- INAVLID\_INFORMATION: 잘못된 파라미터 정보<br>\- BINARY\_UPLOAD\_ERROR: 바이너리 업로드 중 오류 발생<br>\- ALREADY\_UPLOADED\_VERSION: 바이너리 버전 충돌 |

##### Response Sample
``` json
{
	"isSuccess" : true,
	"result" : 111
}
```

### 배포 실행
* 배포 실행을 위한 API입니다.
* 아티팩트 `Command Type`이 Cloud Agent의 경우만 배포 실행 API를 제공합니다.(SSH의 경우 제공되지 않습니다.)

#### Version 1.0
| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.gov-nhncloudservice.com/api/v1.0/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/scenario/{scenarioId}/deploy |

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

##### Sample Request For cURL
``` java
curl --location 'https://api-tcd.gov-nhncloudservice.com/api/v1.0/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/scenario/{scenarioId}/deploy' \
--header 'X-TC-AUTHENTICATION-ID: {ID}' \
--header 'X-TC-AUTHENTICATION-SECRET: {Key}' \
--header 'Content-Type: application/json' \
--data '{
	"targetServerHostnames" : "{ex. server1,server2}",
	"concurrentNum" : 1,
	"nextWhenFail" : false,
	"deployNote" : "{Note 내용}",
	"async" : false
}'
```

##### Response(json)
* isSuccessful 항목은 배포 실행 호출에 성공했는지 유무를 확인하는 필드값이며 deployStatus 항목을 통해 배포 결과(성공, 실패)를 확인해야 합니다.

| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | 배포 실행 호출 성공 여부 | true 또는 false |
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
    "body": {
        "deployKey": 52876,
        "deployStatus": "{배포 상태}",
        "deployResult": [
            {
                "deployKey": 52876,
                "hostname": "{호스트명}",
                "status": "{배포 결과}",
                "taskResult": [
					"..."
                ]
            }
        ],
        "deployResultLocation": "{배포 실행된 Deploy 서비스 프로젝트 링크}"
    }
}
```