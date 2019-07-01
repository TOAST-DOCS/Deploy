## Dev Tool > Deploy > APIガイド

ユーザーがHTTP Requestを直接構成してバイナリをアップロードできるAPIを提供します。

## Ver 1.0

* 主な改善事項
    * REST形式のAPI
    * resultCodeの細分化

| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.cloud.toast.com/api/v1.0/projects/{appkey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} |

### Parameter

| Name | Type | Description | Value | Required |
| ---- | ---- | ----------- | ----- | -------- |
| applicationType | String | アーティファクトのタイプ | clientまたはserver | true |
| version | String | アップロードするバイナリのバージョン、未入力の時はtimestampで代替 | - | false |
| description | String | バイナリの説明 | - | false |
| osType | String | applicationTypeがclientの場合、バイナリファイルのOS情報 | iOS、Androidまたはetc | false |
| binaryFile | File | バイナリファイルオブジェクト | - | true |
| metaFile | File | iOSの場合、plistファイルオブジェクト | - | false |
| fix | Boolean | applicationTypeがClientの場合、Fixするかの情報 | true/false | false |

### Sample Request For cUrl

``` java
curl --tlsv1.2 -X POST \
  https://api-tcd.cloud.toast.com/api/v1.0/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} \
  -H 'content-type: multipart/form-data' \
  -F 'binaryFile=@ojdbc14.jar' \
  -F 'applicationType=server' \
  -F 'description=A binary file of some kind'
```

### Sample Request For JAVA

下記コードは、HttpClientライブラリ(httpclient 4.3.6)を使用して、APIを通してバイナリをアップロードするコードの例です。

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
| isSuccessful | boolean | アップロード結果 | trueまたはfalse |
| resultCode | String | アップロード結果メッセージ | [エラーコード](/Dev%20Tool/Deploy/ja/error-code/)参照 |
| downloadUrl | String | アップロードバイナリのダウンロードパス | 該当パスでダウンロード可能 |
| binaryKey | String | アップロードしたバイナリのキー | - |

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

## 以前のバージョン

| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.cloud.toast.com/api/binary/upload/artifact/{artifactId} |

### Parameter

| Name | Type | Description | Value | Required |
| ---- | ---- | ----------- | ----- | -------- |
| appKey | String | TOASTクラウドアプリキー、Deployサービスページで確認可能 | - | true |
| applicationType | String | アーティファクトのタイプ | clientまたはserver | true |
| binaryGroupKey | long | バイナリのグループキー | 未入力の時、基本グループに指定 | false |
| version | String | アップロードするバイナリのバージョン、未入力の時、timestampで代替 | - | false |
| description | String | バイナリの説明 | - | false |
| osType | String | applicationTypeがclientの場合、バイナリファイルのOS情報 | iOS、Androidまたはetc | false |
| binaryFile | File | バイナリファイルオブジェクト | - | true |
| metaFile | File | iOSの場合、plistファイルオブジェクト | - | false |
| fix | Boolean | applicationTypeがClientの場合、Fixするかどうかの情報 | true/false | false |

### Sample Request For JAVA

下記コードは、HttpClientライブラリ(httpclient 4.3.6)を使用して、APIを通してバイナリをアップロードするコードの例です。

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
| isSuccess | boolean | アップロード結果 | trueまたはfalse |
| result | String | アップロード結果メッセージ | isSuccess：true<br>\- アップロードされたバイナリのキー情報<br>isSuccess ： false<br>\- INAVLID\_INFORMATION：無効なパラメータ情報<br>\- BINARY\_UPLOAD\_ERROR：バイナリアップロード中にエラー発生<br>\- ALREADY\_UPLOADED\_VERSION：バイナリバージョン衝突 |

### Response Sample

``` json
{
	"isSuccess" : true,
	"result" : 111
}
```