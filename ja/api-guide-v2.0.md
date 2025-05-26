## Dev Tools > Deploy > API v2.0ガイド
Deployでは、ユーザーがHTTPリクエストを直接構成してデプロイを実行するためのAPIを提供します。

### 基本情報
#### エンドポイント
```text
https://api-tcd.nhncloudservice.com
```

#### 提供するAPIの種類
| Method | URI | 説明 |
| ------ | --- | --- |
| POST | /api/v2.0/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy | デプロイ実行API |

#### APIリクエストパス変数
| 値 | タイプ | 説明 |
| --- | --- | --- |
| appKey | String | 使用するDeployサービスのアプリケーションキー |
| artifactId | Number | 使用するアーティファクトのID |
| binaryGroupKey | Number | バイナリをアップロードするバイナリグループキー |
| serverGroupId | Number | デプロイ対象となるサーバーグループID |

### デプロイ実行
* デプロイを実行するためのAPIです。
* アーティファクト `Command Type`がCloud Agentの場合のみデプロイ実行APIを提供します(SSHの場合は提供されません)。
* v2.0では、Autoscaleサーバーグループにもデプロイを実行できます。

#### Version 2.0
| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy |

##### Header
| Name | Description | Value |
| --- | --- | --- |
| Content-Type | ConentType | application/json |
| X-TC-AUTHENTICATION-ID | APIセキュリティ設定メニューのUser Access Key ID | {id} |
| X-TC-AUTHENTICATION-SECRET | APIセキュリティ設定メニューのSecret Access Key | {key} |

##### Parameter (Body)
| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| targetServerHostnames | String | サーバーグループ内で選択的にデプロイ対象となる ',' で区切られたサーバーのホスト名(サーバーグループ全体の場合、すべて入力) | hostname1, hostname2, hostname3(ない場合、サーバーグループ内のサーバー全体をデプロイ) | false | サーバーグループに含まれる全てのサーバー |
| concurrentNum | Number | 並列で実行するデプロイ数 | 0以上の値、0の場合、サーバーグループ全体を同時実行 | false | 0 |
| nextWhenFail | Boolean | シナリオが失敗した場合、次のサーバーを実行するかどうか | true/false | false | false (実行中断) |
| deployNote | String | デプロイ時に作成する追加情報 |  | false |  |
| async | Boolean | デプロイ結果を待たずにレスポンスを受け取る | true/false | false | false |
| scenarioIds | String | 実行するシナリオscenarioId | サーバーグループ内で(,)で区切られたシナリオID(ない場合、マッピングされている全てのScenarioID) | false(ただし、通常のDeploy時はtrue - 1件のみ) |ない場合はマッピングされている全てのScenarioID |

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
	"deployNote" : "{Note内容}",
	"async" : false,
	"scenarioIds" : "{ex. 1,2}"
}'
```

##### Response(json)
* isSuccessful項目はデプロイ実行呼び出しが成功したかどうかを確認するフィールド値で、deployStatus項目でデプロイ結果(成功、失敗)を確認する必要があります。
* Autoscaleサーバーグループにデプロイを実行した場合、body値がList形式で返されます。

| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | デプロイ実行成否 | trueまたはfalse |
| resultCode | String | デプロイ実行結果メッセージ | [エラーコード](/Dev%20Tools/Deploy/ko/error-code/)参考 |
| deployStatus | String | デプロイ状態 | success, failまたはdeploying(asyncオプションがtrueの場合)
| deployResult | List | サーバー別のデプロイ結果 | - hostname:デプロイ対象ホスト名(インスタンスID)<br>- status:デプロイ結果<br>- taskResult:デプロイシナリオ内の各タスクの情報 |
| deployResultLocation | String | デプロイが実行されたDeployサービスプロジェクトリンク | 該当リンクでDeployサービスプロジェクトのコンソールに接続可能 |

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
			"deployStatus": "{デプロイ状態}",
			"deployResult": [
				{
					"deployKey": 192349,
					"hostname": "{ホスト名}",
					"status": "{デプロイ結果}",
					"taskResult": [
						"..."
					]
				}
			],
			"deployResultLocation": "{デプロイされたDeployサービスプロジェクトリンク}"
		}
	]
}
```
