<!-- pre-align:aligned sig=bfc4bcd52718 -->

## Dev Tools > Deploy > API v2.0ガイド
Deployでは、デプロイの実行や情報照会のためのAPIを提供しています。ユーザーがHTTPリクエストを独自に構成して使用できます。

<a id="basic-information"></a>

### 基本情報
<a id="endpoint"></a>

#### エンドポイント
```text
https://api-tcd.nhncloudservice.com
```

<a id="available-apis"></a>

#### 提供するAPIの種類
| Method | URI | 説明 |
| ------ | --- | --- |
| POST | /api/v2.0/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy | デプロイ実行API |
| GET | /api/v2.0/projects/{appKey}/artifacts | アーティファクト一覧照会API |
| GET | /api/v2.0/projects/{appKey}/artifacts/{artifactId}/server-groups | サーバーグループ一覧照会API |
| GET | /api/v2.0/projects/{appKey}/artifacts/{artifactId}/binary-groups | バイナリグループ一覧照会API |
| GET | /api/v2.0/projects/{appKey}/artifacts/{artifactId}/deploy-histories | デプロイ履歴照会API |
| GET | /api/v2.0/projects/{appKey}/artifacts/{artifactId}/binary-groups/{binaryGroupKey}/binaries | バイナリ一覧照会API |

<a id="api-request-path-variables"></a>

#### APIリクエストパス変数
| 値 | タイプ | 説明 |
| --- | --- | --- |
| appKey | String | 使用するDeployサービスのアプリケーションキー |
| artifactId | Number | 使用するアーティファクトのID |
| binaryGroupKey | Number | バイナリをアップロードするバイナリグループキー |
| serverGroupId | Number | デプロイ対象となるサーバーグループID |

<a id="execute-deployment"></a>

### デプロイ実行
* デプロイを実行するためのAPIです。
* アーティファクト `Command Type`がCloud Agentの場合のみデプロイ実行APIを提供します(SSHの場合は提供されません)。
* v2.0では、Autoscaleサーバーグループにもデプロイを実行できます。
* デプロイ実行APIはロールベースのアクセス制御(RBAC)を使用します。**Deploy ADMIN**ロールを保有するユーザーのみがデプロイ実行APIを使用できます。

<a id="version-20"></a>

#### Version 2.0
| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy |

##### Header
| Name | Description | Value |
| --- | --- | --- |
| Content-Type | ContentType | application/json |
| X-TC-AUTHENTICATION-ID | APIセキュリティ設定メニューのUser Access Key ID | {id} |
| X-TC-AUTHENTICATION-SECRET | APIセキュリティ設定メニューのSecret Access Key | {key} |

##### Parameter (Body)
| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| targetServerHostnames | String | サーバーグループ内で選択的にデプロイ対象とするサーバーのホスト名をカンマ(,)で区切って指定(サーバーグループ全体の場合は、全て入力) | hostname1, hostname2, hostname3(指定しない場合、サーバーグループ内の全てのサーバーにデプロイ) | false | サーバーグループに含まれる全てのサーバー |
| concurrentNum | Number | 並列で実行するデプロイ数 | 0以上の値、0の場合、サーバーグループ全体を同時実行 | false | 0 |
| nextWhenFail | Boolean | シナリオが失敗した場合、次のサーバーを実行するかどうか | true/false | false | false (実行中断) |
| deployNote | String | デプロイ時に作成する追加情報 |  | false |  |
| async | Boolean | デプロイ結果を待たずにレスポンスを受け取る | true/false | false | false |
| scenarioIds | String | 実行するシナリオのscenarioId | サーバーグループ内でカンマ(,)で区切られたシナリオID(指定しない場合、マッピングされている全てのScenarioID) | false(ただし、通常のDeploy時はtrueで1件のみ) | 指定しない場合は、マッピングされている全てのScenarioID |

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
* isSuccessful項目は、デプロイ実行の呼び出しの成否を確認するフィールド値です。デプロイ結果(成功、失敗)はdeployStatus項目で確認する必要があります。
* Autoscaleサーバーグループにデプロイを実行した場合、body値がList形式で返されます。

| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | デプロイ実行成否 | trueまたはfalse |
| resultCode | String | デプロイ実行結果メッセージ | [エラーコード](/Dev%20Tools/Deploy/ja/error-code/)参考 |
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

<a id="list-artifacts"></a>

### アーティファクト一覧の照会
* プロジェクトのアーティファクト一覧を照会するAPIです。

<a id="list-artifacts-version-20"></a>

#### Version 2.0
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts |

##### Header
| Name | Description | Value |
| --- | --- | --- |
| X-TC-AUTHENTICATION-ID | APIセキュリティ設定メニューのUser Access Key ID | {id} |
| X-TC-AUTHENTICATION-SECRET | APIセキュリティ設定メニューのSecret Access Key | {key} |

##### Parameter (Query String)
| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| artifactName | String | アーティファクト名の検索 | 検索するアーティファクト名 | false | - |

##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts?artifactName={artifactName}' \
  -H 'X-TC-AUTHENTICATION-ID: {ID}' \
  -H 'X-TC-AUTHENTICATION-SECRET: {Key}'
```

##### Response(json)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | リクエストの成否 | true または false |
| resultCode | String | リクエスト結果のメッセージ | [エラーコード](/Dev%20Tools/Deploy/ja/error-code/) を参照 |
| artifacts | List | アーティファクト一覧 | 以下の項目を参照 |

**artifacts**

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | Number | アーティファクトID |
| name | String | アーティファクト名 |
| applicationType | String | アプリケーションのタイプ (server/client) |
| description | String | 説明 |
| createDate | Date | 作成日 |
| lastDeployDate | Date | 最終デプロイ日 |

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
                "description": "サーバーアーティファクト",
                "createDate": "2025-01-01T00:00:00+09:00",
                "lastDeployDate": "2025-03-01T12:00:00+09:00"
            }
        ]
    }
}
```

<a id="list-server-groups"></a>

### サーバーグループ一覧の照会
* アーティファクトに属するサーバーグループ一覧を照会するAPIです。

<a id="list-server-groups-version-20"></a>

#### Version 2.0
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/server-groups |

##### Header
| Name | Description | Value |
| --- | --- | --- |
| X-TC-AUTHENTICATION-ID | APIセキュリティ設定メニューのUser Access Key ID | {id} |
| X-TC-AUTHENTICATION-SECRET | APIセキュリティ設定メニューのSecret Access Key | {key} |

##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/server-groups' \
  -H 'X-TC-AUTHENTICATION-ID: {ID}' \
  -H 'X-TC-AUTHENTICATION-SECRET: {Key}'
```

##### Response(json)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | リクエストの成否 | true または false |
| resultCode | String | リクエスト結果のメッセージ | [エラーコード](/Dev%20Tools/Deploy/ja/error-code/) を参照 |
| serverGroups | List | サーバーグループ一覧 | 以下の項目を参照 |

**serverGroups**

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | Number | サーバーグループID |
| name | String | サーバーグループ名 |
| description | String | 説明 |
| osType | String | OSタイプ (LINUX/WINDOWS) |
| serverCount | Number | サーバー数 |

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
                "description": "本番サーバーグループ",
                "osType": "LINUX",
                "serverCount": 3
            }
        ]
    }
}
```

<a id="list-binary-groups"></a>

### バイナリグループ一覧の照会
* アーティファクトに属するバイナリグループ一覧を照会するAPIです。

<a id="list-binary-groups-version-20"></a>

#### Version 2.0
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/binary-groups |

##### Header
| Name | Description | Value |
| --- | --- | --- |
| X-TC-AUTHENTICATION-ID | APIセキュリティ設定メニューのUser Access Key ID | {id} |
| X-TC-AUTHENTICATION-SECRET | APIセキュリティ設定メニューのSecret Access Key | {key} |

##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/binary-groups' \
  -H 'X-TC-AUTHENTICATION-ID: {ID}' \
  -H 'X-TC-AUTHENTICATION-SECRET: {Key}'
```

##### Response(json)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | リクエストの成否 | true または false |
| resultCode | String | リクエスト結果のメッセージ | [エラーコード](/Dev%20Tools/Deploy/ja/error-code/) を参照 |
| binaryGroups | List | バイナリグループ一覧 | 以下の項目を参照 |

**binaryGroups**

| Name | Type | Description |
| ---- | ---- | ----------- |
| key | Number | バイナリグループキー |
| name | String | バイナリグループ名 |
| description | String | 説明 |
| regionCode | String | リージョンコード |
| createDate | Date | 作成日 |

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
                "description": "本番バイナリグループ",
                "regionCode": "KR1",
                "createDate": "2025-01-01T00:00:00+09:00"
            }
        ]
    }
}
```

<a id="list-deployment-history"></a>

### デプロイ履歴の照会
* アーティファクトのデプロイ履歴を照会するAPIです。
* 照会期間は最大1年まで指定できます。

<a id="list-deployment-history-version-20"></a>

#### Version 2.0
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/deploy-histories |

##### Header
| Name | Description | Value |
| --- | --- | --- |
| X-TC-AUTHENTICATION-ID | APIセキュリティ設定メニューのUser Access Key ID | {id} |
| X-TC-AUTHENTICATION-SECRET | APIセキュリティ設定メニューのSecret Access Key | {key} |

##### Parameter (Query String)
| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| serverGroupId | Number | サーバーグループID | 0の場合はアーティファクト全体を照会 | false | 0 |
| deploymentYearFrom | String | 照会開始日 | yyyy-MM-dd 形式 | false | 現在日 - 1か月 |
| deploymentYearTo | String | 照会終了日 | yyyy-MM-dd 形式 | false | 現在日 |
| pageNum | Number | ページ番号 | 1以上の値 | false | 1 |
| pageSize | Number | 1ページあたりの件数 | 1以上の値 | false | 20 |

##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/deploy-histories?serverGroupId=0&deploymentYearFrom=2025-01-01&deploymentYearTo=2025-03-01&pageNum=1&pageSize=20' \
  -H 'X-TC-AUTHENTICATION-ID: {ID}' \
  -H 'X-TC-AUTHENTICATION-SECRET: {Key}'
```

##### Response(json)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | リクエストの成否 | true または false |
| resultCode | String | リクエスト結果のメッセージ | [エラーコード](/Dev%20Tools/Deploy/ja/error-code/) を参照 |
| totalCount | Number | 総件数 | - |
| deployHistories | List | デプロイ履歴一覧 | 以下の項目を参照 |

**deployHistories**

| Name | Type | Description |
| ---- | ---- | ----------- |
| deployKey | Number | デプロイキー |
| scenarioName | String | シナリオ名 |
| serverGroupName | String | サーバーグループ名 |
| serverGroupId | Number | サーバーグループID |
| binaryVersion | String | バイナリバージョン |
| executeDate | Date | 実行日時 |
| executeUser | String | 実行者 |
| totalResult | String | 実行結果 (SUCCESS/FAIL/RUNNING) |

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
                "scenarioName": "デプロイシナリオ",
                "serverGroupName": "本番サーバーグループ",
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

### バイナリ一覧の照会
* バイナリグループに属するバイナリ一覧を照会するAPIです。

<a id="list-binaries-version-20"></a>

#### Version 2.0
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/binary-groups/{binaryGroupKey}/binaries |

##### Header
| Name | Description | Value |
| --- | --- | --- |
| X-TC-AUTHENTICATION-ID | APIセキュリティ設定メニューのUser Access Key ID | {id} |
| X-TC-AUTHENTICATION-SECRET | APIセキュリティ設定メニューのSecret Access Key | {key} |

##### Parameter (Query String)
| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| pageNum | Number | ページ番号 | 1以上の値 | false | 1 |
| pageSize | Number | 1ページあたりの件数 | 1以上の値 | false | 20 |
| sortKey | String | ソート基準 | VERSION, BINARY_KEY, UPLOAD_DATE | false | UPLOAD_DATE |
| sortDirection | String | ソート方向 | ASC, DESC | false | DESC |
| keyword | String | バイナリバージョンの検索キーワード | 検索するキーワード | false | - |

##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.0/projects/{appKey}/artifacts/{artifactId}/binary-groups/{binaryGroupKey}/binaries?pageNum=1&pageSize=20&sortKey=UPLOAD_DATE&sortDirection=DESC' \
  -H 'X-TC-AUTHENTICATION-ID: {ID}' \
  -H 'X-TC-AUTHENTICATION-SECRET: {Key}'
```

##### Response(json)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | リクエストの成否 | true または false |
| resultCode | String | リクエスト結果のメッセージ | [エラーコード](/Dev%20Tools/Deploy/ja/error-code/) を参照 |
| totalCount | Number | 総件数 | - |
| binaries | List | バイナリ一覧 | 以下の項目を参照 |

**binaries**

| Name | Type | Description |
| ---- | ---- | ----------- |
| binaryKey | Number | バイナリキー |
| version | String | バイナリバージョン |
| binaryName | String | バイナリファイル名 |
| binarySize | Number | バイナリファイルのサイズ (bytes) |
| uploadDate | Date | アップロード日時 |
| uploader | String | アップローダー |
| description | String | 説明 |

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
                "description": "リリースバイナリ"
            }
        ]
    }
}
```
