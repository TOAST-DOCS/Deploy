## Dev Tool > Deploy > API Guide

사용자가 HTTP Request를 직접 구성하여 바이너리를 업로드 할 수 있는 API를 제공합니다.

## Ver 1.0

* 주요 개선 사항
    * REST 형태의 API
    * resultCode 세분화

| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.cloud.toast.com/api/v1.0/projects/{appkey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} |

<center>[표 1] Binary Upload Request URL</center>

### Parameter

| Name | Type | Description | Value | Required |
| ---- | ---- | ----------- | ----- | -------- |
| applicationType | String | 아티팩트의 타입 | client 또는 server | true |
| version | String | 업로드하는 바이너리의 버전, 미입력 시 timestamp로 대체 | - | false |
| description | String | 바이너리의 설명 | - | false |
| osType | String | applicationType이 client인 경우 바이너리 파일의 os 정보 | iOS 또는 Android 또는 etc | false |
| binaryFile | File | 바이너리 파일 객체 | - | true |
| metaFile | File | iOS인 경우 plist 파일 객체 | - | false |
| fix | Boolean | applicationType이 Client인 경우 Fix 여부 정보 | true/false | false |

<center>[표 2] Binary Upload Request Parameter</center>

### Sample Request For cUrl

```java
curl -X POST \
  https://api-tcd.cloud.toast.com/api/v1.0/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} \
  -H 'content-type: multipart/form-data' \
  -F 'binaryFile=@ojdbc14.jar' \
  -F applicationType=server \
  -F 'description=A binary file of some kind'
```

### Sample Request For JAVA

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

### Response(json)

| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | boolean | 업로드 결과 | true 또는 false |
| resultCode | String | 업로드 결과 메세지 | [오류 코드](/Dev%20Tool/Deploy/ja/error-code/) 참고 |
| downloadUrl | String | 업로드 바이너리의 다운로드 경로 | 해당 경로로 다운로드 가능 |
| binaryKey | String | 업로드한 바이너리의 키 | - |

<center>[표 3] Binary Upload Response(json)</center>

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

## 이전 버전

| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.cloud.toast.com/api/binary/upload/artifact/{artifactId} |

<center>[표 1] Binary Upload Request URL</center>

### Parameter

| Name | Type | Description | Value | Required |
| ---- | ---- | ----------- | ----- | -------- |
| appKey | String | 토스트 클라우드 앱키, 디플로이 상품 페이지에서 확인가능 | - | true |
| applicationType | String | 아티팩트의 타입 | client 또는 server | true |
| binaryGroupKey | long | 바이너리의 그룹 키 | 미입력 시 기본 그룹으로 지정 | false |
| version | String | 업로드하는 바이너리의 버전, 미입력 시 timestamp로 대체 | - | false |
| description | String | 바이너리의 설명 | - | false |
| osType | String | applicationType이 client인 경우 바이너리 파일의 os 정보 | iOS 또는 Android 또는 etc | false |
| binaryFile | File | 바이너리 파일 객체 | - | true |
| metaFile | File | iOS인 경우 plist 파일 객체 | - | false |
| fix | Boolean | applicationType이 Client인 경우 Fix 여부 정보 | true/false | false |

<center>[표 2] Binary Upload Request Parameter</center>

### Sample Request Code For JAVA

아래 코드는 HttpClient 라이브러리(httpclient 4.3.6)를 사용하여 API를 통해 바이너리를 업로드하는 코드의 예시입니다.

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

### Response(json)

| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccess | boolean | 업로드 결과 | true 또는 false |
| result | String | 업로드 결과 메세지 | isSuccess : true<br>\- 업로드된 바이너리의 키정보<br>isSuccess : false<br>\- INAVLID\_INFORMATION : 잘못된 파라미터 정보<br>\- BINARY\_UPLOAD\_ERROR : 바이너리 업로드 중 오류 발생<br>\- ALREADY\_UPLOADED\_VERSION : 바이너리 버전충돌 |

<center>[표 3] Binary Upload Response(json)</center>

### Response Sample

``` json
{
	"isSuccess" : true,
	"result" : 111
}
```