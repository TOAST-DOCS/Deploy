## Dev Tools > Deploy > API v2.1 ガイド
Deployでは、バイナリのアップロード、バイナリのダウンロード、デプロイの実行、情報の照会を行うためのAPIを提供しています。ユーザーがHTTPリクエストを独自に構成して使用できます。

### 基本情報
#### エンドポイント
```text
https://api-tcd.nhncloudservice.com
```

#### APIリクエスト HTTPヘッダ
```
X-NHN-AUTHORIZATION: Bearer {発行されたトークン}
```

#### 認証及び権限
Deployは、API呼び出し時の認証・認可にUser Access Keyトークンを使用します。
User Access Keyトークンは、User Access Keyを基に発行されるBearerタイプの一時的なアクセストークンです。
User Access Keyトークンの発行及び使用方法に関する詳細は、[User Access Keyトークン](/nhncloud/ja/public-api/user-access-key-token)をご参照ください。

Deploy APIは、ロールベースのアクセス制御(RBAC)を使用します。<br>
ユーザーは、APIを使用するために**Deploy ADMINロール**または**Deploy VIEWERロール**を保有している必要があります。

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

#### APIリクエストのパス変数
| 値 | タイプ | 説明 |
| --- | --- | --- |
| appKey | String | 使用するDeployサービスのAppkey |
| artifactId | Number | 使用するアーティファクトのID |
| binaryGroupKey | Number | バイナリをアップロードするバイナリグループのキー |
| binaryKey | Number | バイナリキー(アップロード時に発行) |
| serverGroupId | Number | デプロイ対象となるサーバーグループのID |

### バイナリのアップロード
#### Version 2.1
| Http Method | POST |
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

### バイナリのダウンロード
バイナリアップロードAPIのレスポンスとして受信したダウンロードパスから、バイナリファイルをダウンロードできます。

#### Version 2.1
| Http Method | GET |
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

### デプロイの実行
* デプロイ実行用のAPIです。
* アーティファクトの`Command Type`がCloud Agentの場合にのみ、デプロイ実行APIが提供されます(SSHの場合は提供されません)。
* v2.1では、Auto Scaleサーバーグループにもデプロイを実行できます。

#### Version 2.1
| Http Method | POST |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy |

##### Parameter(Body)
| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| targetServerHostnames | String | サーバーグループ内で選択的にデプロイ対象とするサーバーのホスト名をカンマ(,)で区切って指定(サーバーグループ全体の場合は、全て入力) | hostname1, hostname2, hostname3(指定しない場合、サーバーグループ内の全てのサーバーにデプロイ) | false | サーバーグループに含まれる全てのサーバー |
| concurrentNum | Number | 並列で実行するデプロイ数 | 0以上の値。0の場合はサーバーグループ全体で同時に実行 | false | 0 |
| nextWhenFail | Boolean | シナリオ失敗時に次のサーバーを実行するかどうか | true/false | false | false(実行を中断) |
| deployNote | String | デプロイ時に作成する付加情報 |  | false |  |
| async | Boolean | デプロイ結果を待たずにレスポンスを受信 | true/false | false | false |
| scenarioIds | String | 実行するシナリオのscenarioId | サーバーグループ内でカンマ(,)で区切られたシナリオID(指定しない場合、マッピングされている全てのScenarioID) | false(ただし、通常のDeploy時はtrueで1件のみ) | 指定しない場合は、マッピングされている全てのScenarioID |

##### Sample Request For cURL
``` java
curl --location 'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/server-group/{serverGroupId}/deploy' \
--header 'X-NHN-AUTHORIZATION: Bearer {token}' \
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
* Auto Scaleサーバーグループにデプロイした場合、Body値がList形式で存在します。

| Name | Type | Description | Value |
| ---- | ---- | ----------- | ----- |
| isSuccessful | Boolean | デプロイ実行の成否 | true または false |
| resultCode | String | デプロイ実行結果のメッセージ | [エラーコード](/Dev%20Tools/Deploy/ja/error-code/) を参照 |
| deployStatus | String | デプロイステータス | success, fail または deploying (asyncオプションがtrueの場合) |
| deployResult | List | サーバー別のデプロイ結果 | - hostname: デプロイ対象のホスト名 (インスタンスID)<br>- status: デプロイ結果<br>- taskResult: デプロイシナリオ内の各タスクごとの情報 |
| deployResultLocation | String | デプロイが実行されたDeployサービスプロジェクトのリンク | 該当リンクからDeployサービスプロジェクトのコンソールにアクセス可能 |

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
			"deployStatus": "{デプロイステータス}",
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
			"deployResultLocation": "{デプロイが実行されたDeployサービスプロジェクトのリンク}"
		}
	]
}
```

### アーティファクト一覧の照会
* プロジェクトのアーティファクト一覧を照会するAPIです。

#### Version 2.1
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts |

##### Parameter(Query String)
| Name | Type | Description | Value | Required | Default Value |
| --- | --- | --- | --- | --- | --- |
| artifactName | String | アーティファクト名の検索 | 検索するアーティファクト名 | false | - |

##### Sample Request For cURL
``` java
curl -X GET \
  'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts?artifactName={artifactName}' \
  -H 'X-NHN-AUTHORIZATION: Bearer {token}'
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
| applicationType | String | アプリケーションのタイプ(server/client) |
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

### サーバーグループ一覧の照会
* アーティファクトに属するサーバーグループ一覧を照会するAPIです。

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
| isSuccessful | Boolean | リクエストの成否 | true または false |
| resultCode | String | リクエスト結果のメッセージ | [エラーコード](/Dev%20Tools/Deploy/ja/error-code/) を参照 |
| serverGroups | List | サーバーグループ一覧 | 以下の項目を参照 |

**serverGroups**

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | Number | サーバーグループID |
| name | String | サーバーグループ名 |
| description | String | 説明 |
| osType | String | OSタイプ(LINUX/WINDOWS) |
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

### バイナリグループ一覧の照会
* アーティファクトに属するバイナリグループ一覧を照会するAPIです。

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

### デプロイ履歴の照会
* アーティファクトのデプロイ履歴を照会するAPIです。
* 照会期間は最大1年まで指定できます。

#### Version 2.1
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/deploy-histories |

##### Parameter(Query String)
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
  'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/deploy-histories?serverGroupId=0&deploymentYearFrom=2025-01-01&deploymentYearTo=2025-03-01&pageNum=1&pageSize=20' \
  -H 'X-NHN-AUTHORIZATION: Bearer {token}'
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

### バイナリ一覧の照会
* バイナリグループに属するバイナリ一覧を照会するAPIです。

#### Version 2.1
| Http Method | GET |
| ----------- | ---- |
| Request URL | https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-groups/{binaryGroupKey}/binaries |

##### Parameter(Query String)
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
  'https://api-tcd.nhncloudservice.com/api/v2.1/projects/{appKey}/artifacts/{artifactId}/binary-groups/{binaryGroupKey}/binaries?pageNum=1&pageSize=20&sortKey=UPLOAD_DATE&sortDirection=DESC' \
  -H 'X-NHN-AUTHORIZATION: Bearer {token}'
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
| binarySize | Number | バイナリファイルのサイズ(bytes) |
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
