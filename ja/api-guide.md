## Dev Tools > Deploy > APIガイド
Deployでは、ユーザーがHTTP Requestを直接構成してバイナリアップロード、配布を実行するためのAPIを提供します。

### 基本情報
#### エンドポイント
```text
https://api-tcd.nhncloudservice.com
```

#### 提供するAPIの種類
| Method | URI | 説明 |
| ------ | --- | --- |
| POST | /api/v1.0/projects/{appkey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} | バイナリアップロードAPI |
| POST | /api/v1.0/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/scenario/{scenarioId}/deploy | 配布実行API |

#### APIリクエストパス変数
| 値 | タイプ | 説明 |
| --- | --- | --- |
| appKey | String | 使用するDeployサービスのアプリケーションキー |
| artifactId | Number | 使用するアーティファクトのID |
| binaryGroupKey | Number | バイナリをアップロードするバイナリグループキー |
| serverGroupId | Number | 配布対象となるサーバーグループID |
| scenarioId | Number | 配布するシナリオのID |

##### 変数別の値確認方法
<details> 
<summary>appKey確認方法</summary>
* `URL & Appkey` ボタンを押して確認できます。

![deploy_api_01_202402.png](https://static.toastoven.net/prod_tcdeploy/deploy_api_01_202402.png)
</details>
<details> 
<summary>artifactIdの確認方法</summary>
* アーティファクトリストのIDカラムで確認できます。

![deploy_api_02_202402.png](https://static.toastoven.net/prod_tcdeploy/deploy_api_02_202402.png)
</details>
<details> 
<summary>binaryGroupKey確認方法</summary>
* バイナリグループで選択した後、Key値を確認できます。

![deploy_api_03_202402.png](https://static.toastoven.net/prod_tcdeploy/deploy_api_03_202402.png)
* バイナリグループにマウスオーバーして確認することもできます。 (括弧番号)

![deploy_api_04_202402.png](https://static.toastoven.net/prod_tcdeploy/deploy_api_04_202402.png)
</details>
<details> 
<summary>serverGroupIdの確認方法</summary>
* サーバーグループでマウスオーバーして確認することもできます。 (括弧番号)

![deploy_api_05_202402.png](https://static.toastoven.net/prod_tcdeploy/deploy_api_05_202402.png)
</details>
<details> 
<summary>scenarioId確認方法</summary>
* シナリオを選択するとscenario IDを確認できます。

![deploy_api_06_202402.png](https://static.toastoven.net/prod_tcdeploy/deploy_api_06_202402.png)
</details>

### バイナリアップロード
#### Version 1.0
| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v1.0/projects/{appkey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} |

##### Parameter
| Name | Type | Description | Value | Required |
| ---- | ---- | ----------- | ----- | -------- |
| applicationType | String | アーティファクトのタイプ | clientまたはserver | true |
| version | String | アップロードするバイナリのバージョンが未入力の時は、timestampで代替(最大100文字) | - | false |
| description | String | バイナリの説明 | - | false |
| osType | String | applicationTypeがclientの場合、バイナリファイルのOS情報 | iOS、Androidまたはetc | false |
| binaryFile | File | バイナリファイルオブジェクト | - | true |
| metaFile | File | iOSの場合、plistファイルオブジェクト | - | false |
| fix | Boolean | applicationTypeがClientの場合、Fixするかの情報 | true/false | false |

##### Sample Request For cURL
``` java
curl -X POST \
  https://api-tcd.nhncloudservice.com/api/v1.0/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} \
  -H 'content-type: multipart/form-data' \
  -F 'binaryFile=@ojdbc14.jar' \
  -F 'applicationType=server' \
  -F 'description=A binary file of some kind'
```

##### Sample Request For JAVA
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
| isSuccessful | boolean | アップロード結果 | trueまたはfalse |
| resultCode | String | アップロード結果メッセージ | [エラーコード](/Dev%20Tools/Deploy/ja/error-code/)参照 |
| downloadUrl | String | アップロードバイナリのダウンロードパス | 該当パスでダウンロード可能 |
| binaryKey | String | アップロードしたバイナリのキー | - |

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

#### 以前のバージョン
| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/binary/upload/artifact/{artifactId} |

##### Parameter
| Name | Type | Description | Value | Required |
| ---- | ---- | ----------- | ----- | -------- |
| appKey | String | NHN Cloudクラウドアプリキー、Deployサービスページで確認可能 | - | true |
| applicationType | String | アーティファクトのタイプ | clientまたはserver | true |
| binaryGroupKey | long | バイナリのグループキー | 未入力の時、基本グループに指定 | false |
| version | String | アップロードするバイナリのバージョン、未入力の時、timestampで代替 | - | false |
| description | String | バイナリの説明 | - | false |
| osType | String | applicationTypeがclientの場合、バイナリファイルのOS情報 | iOS、Androidまたはetc | false |
| binaryFile | File | バイナリファイルオブジェクト | - | true |
| metaFile | File | iOSの場合、plistファイルオブジェクト | - | false |
| fix | Boolean | applicationTypeがClientの場合、Fixするかどうかの情報 | true/false | false |

##### Sample Request For JAVA
下記コードは、HttpClientライブラリ(httpclient 4.3.6)を使用して、APIを通してバイナリをアップロードするコードの例です。

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
| isSuccess | boolean | アップロード結果 | trueまたはfalse |
| result | String | アップロード結果メッセージ | isSuccess：true<br>\- アップロードされたバイナリのキー情報<br>isSuccess ： false<br>\- INAVLID\_INFORMATION：無効なパラメータ情報<br>\- BINARY\_UPLOAD\_ERROR：バイナリアップロード中にエラー発生<br>\- ALREADY\_UPLOADED\_VERSION：バイナリバージョン衝突 |

##### Response Sample
``` json
{
	"isSuccess" : true,
	"result" : 111
}
```

### 配布実行
* 配布を実行するためのAPIです。
* アーティファクト `Command Type`がCloud Agentの場合のみ配布実行APIを提供します(SSHの場合は提供されません)。

#### Version 1.0
| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v1.0/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/scenario/{scenarioId}/deploy |

##### Header
| Name | Description | Value |
| --- | --- | --- |
| Content-Type | ConentType | application/json |
| X-TC-AUTHENTICATION-ID | APIセキュリティ設定メニューのUser Access Key ID | {id} |
| X-TC-AUTHENTICATION-SECRET | APIセキュリティ設定メニューのSecret Access Key | {key} |

##### Parameter (Body)
| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| targetServerHostnames | String | サーバーグループ内で選択的に配布対象となる ',' で区切られたサーバーのホスト名(サーバーグループ全体の場合、すべて入力) | hostname1, hostname2, hostname3(ない場合、サーバーグループ内のサーバー全体を配布) | false | サーバーグループに含まれる全てのサーバー |
| concurrentNum | Number | 並列で実行する配布数 | 0以上の値、0の場合、サーバーグループ全体を同時実行 | false | 0 |
| nextWhenFail | Boolean | シナリオが失敗した場合、次のサーバーを実行するかどうか | true/false | false | false (実行中断) |
| deployNote | String | 配布に作成する追加情報 |  | false |  |
| async | Boolean | 配布結果を待たずにレスポンスを受け取る | true/false | false | false |

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
	"deployNote" : "{Note内容}",
	"async" : false
}'
```

##### Response(json)
* isSuccessful項目は配布実行呼び出しが成功したかどうかを確認するフィールド値で、deployStatus項目で配布結果(成功、失敗)を確認する必要があります。

| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | 配布実行成否 | trueまたはfalse |
| resultCode | String | 配布実行結果メッセージ | [エラーコード](/Dev%20Tools/Deploy/ko/error-code/)参考 |
| deployStatus | String | 配布状態 | success, failまたはdeploying(asyncオプションがtrueの場合)
| deployResult | List | サーバー別の配布結果 | - hostname:配布対象ホスト名(インスタンスID)<br>- status:配布結果<br>- taskResult:配布シナリオ内の各タスクの情報 |
| deployResultLocation | String | 配布が実行されたDeployサービスプロジェクトリンク | 該当リンクでDeployサービスプロジェクトのコンソールに接続可能 |

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
        "deployStatus": "{配布状態}",
        "deployResult": [
            {
                "deployKey": 52876,
                "hostname": "{ホスト名}",
                "status": "{配布結果}",
                "taskResult": [
					"..."
                ]
            }
        ],
        "deployResultLocation": "{配布が実行されたDeployサービスプロジェクトリンク}"
    }
}
```
