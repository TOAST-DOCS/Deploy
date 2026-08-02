<!-- pre-align:aligned sig=28e392d2a044 -->

## Dev Tools > Deploy > API v2.1 ガイド
Deployでは、バイナリのアップロード、バイナリのダウンロード、デプロイの実行、情報の照会を行うためのAPIを提供しています。ユーザーがHTTPリクエストを独自に構成して使用できます。

<a id="basic-information"></a>

### 基本情報 { #basic-information }
<a id="endpoint"></a>

#### エンドポイント
```text
https://api-tcd.nhncloudservice.com
```

<a id="api-request-http-header"></a>

#### APIリクエスト HTTPヘッダ
```
X-NHN-AUTHORIZATION: Bearer {発行されたトークン}
```

<a id="authentication-and-authorization"></a>

#### 認証及び権限
Deployは、API呼び出し時の認証・認可にUser Access Keyトークンを使用します。
User Access Keyトークンは、User Access Keyを基に発行されるBearerタイプの一時的なアクセストークンです。
User Access Keyトークンの発行及び使用方法に関する詳細は、[User Access Keyトークン](/nhncloud/ja/public-api/user-access-key-token)をご参照ください。

Deploy APIは、ロールベースのアクセス制御(RBAC)を使用します。<br>
ユーザーは、APIを使用するために**Deploy ADMINロール**または**Deploy VIEWERロール**を保有している必要があります。

<a id="available-apis"></a>

#### 提供するAPIの種類
| メソッド | URI | 説明 |
| ------ | --- | --- |
| POST | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} | バイナリアップロードAPI |
| GET | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey}/binaries/{binaryKey} | バイナリダウンロードAPI |
| POST | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy | デプロイ実行API |
| GET | /api/v2.1/projects/{appKey}/artifacts | アーティファクト一覧照会API |
| GET | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/server-groups | サーバーグループ一覧照会API |
| GET | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-groups | バイナリグループ一覧照会API |
| GET | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/deploy-histories | デプロイ履歴照会API |
| GET | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-groups/{binaryGroupKey}/binaries | バイナリ一覧照会API |
| GET | /api/v2.1/projects/{appKey}/artifacts/{artifactId}/server-groups/{serverGroupId}/scenarios | シナリオ一覧照会 API |

<a id="api-request-path-variables"></a>

#### APIリクエストのパス変数
| 値 | タイプ | 説明 |
| --- | --- | --- |
| appKey | String | 使用するDeployサービスのAppkey |
| artifactId | Number | 使用するアーティファクトのID |
| binaryGroupKey | Number | バイナリをアップロードするバイナリグループのキー |
| binaryKey | Number | バイナリキー(アップロード時に発行) |
| serverGroupId | Number | デプロイ対象となるサーバーグループのID |

<a id="upload-binary"></a>

### バイナリのアップロード { #upload-binary }
<a id="version-21"></a>

#### Version 2.1
| HTTP Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey} |

##### Parameter
| Name | Type | Description | Value | Required |
| ---- | ---- | ----------- | ----- | -------- |
| applicationType | String | アーティファクトのタイプ | client または server | true |
| version | String | アップロードするバイナリのバージョン。未入力時はtimestampで代替(最大100文字) | - | false |
| description | String | バイナリの説明 | - | false |
| osType | String | applicationTypeがclientの場合、バイナリファイルのOS情報 | iOS, Android または etc | false |
| binaryFile | File | バイナリファイルオブジェクト | - | true |
| metaFile | File | iOSの場合、plistファイルオブジェクト | - | false |
| fix | Boolean | applicationTypeがclientの場合、Fixの有無に関する情報 | true/false | false |

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
| isSuccessful | boolean | アップロード結果 | true または false |
| resultCode | String | アップロード結果のメッセージ | [エラーコード](/Dev%20Tools/Deploy/ja/error-code/) を参照 |
| downloadUrl | String | アップロードバイナリのダウンロードパス | 該当パスからダウンロード可能 |
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
		"downloadUrl": "https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-group/{binaryGroupKey}/binaries/{uploadedBinaryKey}",
		"binaryKey": "{uploadedBinaryKey}"
	}
}
```

<a id="download-binary"></a>

### バイナリのダウンロード { #download-binary }
バイナリアップロードAPIのレスポンスとして受信したダウンロードパスから、バイナリファイルをダウンロードできます。

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
  -o {保存するファイル名}
```

##### Response
* バイナリファイルをダウンロードします。
* Content-Type: `application/octet-stream`

<a id="execute-deployment"></a>

### デプロイの実行 { #execute-deployment }
* デプロイ実行用のAPIです。
* アーティファクトの`Command Type`がCloud Agentの場合にのみ、デプロイ実行APIが提供されます(SSHの場合は提供されません)。
* v2.1では、Auto Scaleサーバーグループにもデプロイを実行できます。

<a id="execute-deployment-version-21"></a>

#### Version 2.1
| HTTP Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy |
##### Parameter(Body)

| 名前 | タイプ | 説明 | 値 | 必須 | デフォルト値 |
| --- | --- | --- | --- | --- | --- |
| targetServerHostnames | String | サーバーグループ内で選択的にデプロイ対象となる、カンマ (,) 区切りのサーバーのホスト名（サーバーグループ全体の場合はすべて入力） | hostname1, hostname2, hostname3（指定しない場合はサーバーグループ内の全サーバーにデプロイ） | false | サーバーグループに含まれる全サーバー |
| concurrentNum | Number | 並列で実行するデプロイ数 | 0以上の値。0の場合、サーバーグループ全体を同時実行 | false | 0 |
| nextWhenFail | Boolean | シナリオ失敗時に次のサーバーを実行するかどうか | true/false | false | false(実行中断) |
| deployNote | String | デプロイ時に入力する追加情報 |  | false |  |
| async | Boolean | デプロイ結果を待たずに応答を受け取る | true/false | false | false |
| scenarioIds | String | 実行するシナリオのscenarioId | サーバーグループ内でカンマ(,)区切りのシナリオID（指定しない場合は、マッピングされているScenarioIDすべて） | false（ただし、通常のDeployの場合はtrue - 1個のみ） | 指定しない場合は、マッピングされているScenarioIDすべて |
##### Sample Request For cURL
``` java
curl --location 'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy' \
--header 'X-NHN-AUTHORIZATION: Bearer {token}' \
--header 'Content-Type: application/json' \
--data '{
	"targetServerHostnames" : "{ex. server1,server2}",
	"concurrentNum" : 1,
	"nextWhenFail" : false,
	"deployNote" : "{Noteの内容}",
	"async" : false,
	"scenarioIds" : "{ex. 1,2}"
}'
```

##### Response(json)
* isSuccessful 項目はデプロイ実行呼び出しの成否を確認するフィールド値であり、deployStatus 項目でデプロイ結果（成功、失敗）を確認する必要があります。
* Autoscale サーバーグループをデプロイした場合、Body の値は List 形式で存在します。

| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | デプロイ実行の成否 | true または false |
| resultCode | String | デプロイ実行結果メッセージ | [エラーコード](/Dev%20Tools/Deploy/ja/error-code/) 参照 |
| deployStatus | String | デプロイ状態 | success、fail、またはdeploying（asyncオプションがtrueの場合） |
| deployResult | List | サーバーごとのデプロイ結果 | - hostname: デプロイ対象のホスト名（インスタンスID）<br>- status: デプロイ結果<br>- taskResult: デプロイシナリオ内の各タスクの情報 |
| deployResultLocation | String | デプロイが実行されたDeployサービスプロジェクトのリンク | このリンクからDeployサービスプロジェクトのコンソールにアクセスできます |
##### レスポンスサンプル
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
			"deployResultLocation": "{デプロイが実行されたDeployサービスプロジェクトリンク}"
		}
	]
}
```

<a id="list-artifacts"></a>

### アーティファクト一覧の照会 { #list-artifacts }
* プロジェクトのアーティファクト一覧を照会するAPIです。

<a id="list-artifacts-version-21"></a>

#### Version 2.1
| HTTP Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts |
##### Parameter(Query String)

| 名前 | タイプ | 説明 | 値 | 必須 | デフォルト値 |
| --- | --- | --- | --- | --- | --- |
| artifactName | String | アーティファクト名検索 | 検索するアーティファクト名 | false | - |
##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts?artifactName={artifactName}' \
  -H 'X-NHN-AUTHORIZATION: Bearer {token}'
```

##### Response(json)

| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | リクエスト成否 | `true` または `false` |
| resultCode | String | リクエスト結果メッセージ | [エラーコード](/Dev%20Tools/Deploy/ja/error-code/) 参照 |
| artifacts | List | アーティファクト一覧 | 下記の項目を参照 |
**artifacts**

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | Number | アーティファクト ID |
| name | String | アーティファクト名 |
| applicationType | String | アプリケーションタイプ (server/client) |
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

### サーバーグループ一覧の照会 { #list-server-groups }
* アーティファクトに属するサーバーグループ一覧を照会するAPIです。

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

##### Response(json)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | リクエスト成否 | `true` または `false` |
| resultCode | String | リクエスト結果メッセージ | [エラーコード](/Dev%20Tools/Deploy/ja/error-code/) 参照 |
| serverGroups | List | サーバーグループ一覧 | 以下の項目を参照 |
**serverGroups**

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | Number | サーバーグループID |
| name | String | サーバーグループ名 |
| description | String | 説明 |
| osType | String | OS タイプ（LINUX/WINDOWS） |
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

### バイナリグループ一覧の照会 { #list-binary-groups }
* アーティファクトに属するバイナリグループ一覧を照会するAPIです。

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

##### Response(json)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | リクエスト成否 | `true` または `false` |
| resultCode | String | リクエスト結果メッセージ | [エラーコード](/Dev%20Tools/Deploy/ja/error-code/) 参照 |
| binaryGroups | List | バイナリグループ一覧 | 下記の項目を参照 |
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
                "description": "運用バイナリグループ",
                "regionCode": "KR1",
                "createDate": "2025-01-01T00:00:00+09:00"
            }
        ]
    }
}
```

<a id="list-deployment-history"></a>

### デプロイ履歴の照会 { #list-deployment-history }
* アーティファクトのデプロイ履歴を照会するAPIです。
* 照会期間は最大1年まで指定できます。

<a id="list-deployment-history-version-21"></a>

#### Version 2.1
| HTTP Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/deploy-histories |
##### Parameter(Query String)

| 名前 | タイプ | 説明 | 値 | 必須 | デフォルト値 |
| --- | --- | --- | --- | --- | --- |
| serverGroupId | Number | サーバーグループ ID | 0 の場合、アーティファクト全件を照会 | false | 0 |
| deploymentYearFrom | String | 照会開始日 | yyyy-MM-dd 形式 | false | 現在日 - 1か月 |
| deploymentYearTo | String | 照会終了日 | yyyy-MM-dd 形式 | false | 現在日 |
| pageNum | Number | ページ番号 | 1以上の値 | false | 1 |
| pageSize | Number | ページあたりの件数 | 1以上の値 | false | 20 |
##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/deploy-histories?serverGroupId=0&deploymentYearFrom=2025-01-01&deploymentYearTo=2025-03-01&pageNum=1&pageSize=20' \
  -H 'X-NHN-AUTHORIZATION: Bearer {token}'
```

##### Response(json)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | リクエスト成否 | `true` または `false` |
| resultCode | String | リクエスト結果メッセージ | [エラーコード](/Dev%20Tools/Deploy/ja/error-code/) 参照 |
| totalCount | Number | 合計件数 | - |
| deployHistories | List | デプロイ履歴リスト | 下記の項目を参照 |
**deployHistories**

| Name | Type | Description |
| ---- | ---- | ----------- |
| deployKey | Number | デプロイキー |
| scenarioName | String | シナリオ名 |
| serverGroupName | String | サーバーグループ名 |
| serverGroupId | Number | サーバーグループ ID |
| binaryVersion | String | バイナリバージョン |
| executeDate | Date | 実行日 |
| executeUser | String | 実行者 |
| totalResult | String | 実行結果(SUCCESS/FAIL/RUNNING) |
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
                "serverGroupName": "運用サーバーグループ",
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

### バイナリ一覧の照会 { #list-binaries }
* バイナリグループに属するバイナリ一覧を照会するAPIです。

<a id="list-binaries-version-21"></a>

#### Version 2.1
| HTTP Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-groups/{binaryGroupKey}/binaries |
##### Parameter(Query String)

| 名前 | タイプ | 説明 | 値 | 必須 | デフォルト値 |
| --- | --- | --- | --- | --- | --- |
| pageNum | Number | ページ番号 | 1以上の値 | false | 1 |
| pageSize | Number | ページあたりの件数 | 1以上の値 | false | 20 |
| sortKey | String | ソート基準 | VERSION, BINARY_KEY, UPLOAD_DATE | false | UPLOAD_DATE |
| sortDirection | String | ソート方向 | ASC, DESC | false | DESC |
| keyword | String | バイナリバージョン検索キーワード | 検索するキーワード | false | - |
##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-groups/{binaryGroupKey}/binaries?pageNum=1&pageSize=20&sortKey=UPLOAD_DATE&sortDirection=DESC' \
  -H 'X-NHN-AUTHORIZATION: Bearer {token}'
```

##### Response(json)
| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | リクエスト成否 | `true` または `false` |
| resultCode | String | リクエスト結果メッセージ | [エラーコード](/Dev%20Tools/Deploy/ja/error-code/) 参照 |
| totalCount | Number | 合計件数 | - |
| binaries | List | バイナリ一覧 | 下記の項目を参照 |
**binaries**

| Name | Type | Description |
| ---- | ---- | ----------- |
| binaryKey | Number | バイナリキー |
| version | String | バイナリバージョン |
| binaryName | String | バイナリファイル名 |
| binarySize | Number | バイナリファイルのサイズ（bytes） |
| uploadDate | Date | アップロード日 |
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

### シナリオ一覧の照会
* サーバーグループにマッピングされたシナリオ一覧を照会するAPIです。

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
| isSuccessful | Boolean | リクエストの成否 | `true` または `false` |
| resultCode | String | リクエスト結果メッセージ | [エラーコード](/Dev%20Tools/Deploy/ja/error-code/) 参照 |
| scenarios | List | シナリオ一覧 | 以下の項目を参照 |

**scenarios**

| Name | Type | Description |
| ---- | ---- | ----------- |
| scenarioId | Number | シナリオID |
| scenarioName | String | シナリオ名 |

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
                "scenarioName": "デプロイシナリオ"
            }
        ]
    }
}
```
