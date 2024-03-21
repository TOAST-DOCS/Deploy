@@ -0,0 +1,122 @@
## Dev Tools > Deploy > 使用前の設定ガイド

この文書では、次のような内容を説明します。

* [サービス使用前の必須事項](/Dev%20Tools/Deploy/ko/setup-guide/#_1)
* [NHN Cloud Agent使用のための準備](/Dev%20Tools/Deploy/ko/setup-guide/#cloud-agent)
* [SSH接続のための準備](/Dev%20Tools/Deploy/ko/setup-guide/#ssh)

## サービス使用前の必須事項

### NHN Cloud VMサーバー
![SSH接続必須](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_19_202307.png)

> NHN Cloud VMサーバーの場合、SSH接続またはNHN Cloud Agentでサーバーの配布コマンドを伝達します。
> SSH接続の場合ターゲットサーバーのIP、ポート、ファイアウォール例外処理などの[SSH接続のための準備](/Dev%20Tools/Deploy/ko/setup-guide/#ssh)が必要です。
> NHN Cloud Agentの場合NHN Cloud Agentインストール、有効性確認などの[NHN Cloud Agentを使用するための準備](/Dev%20Tools/Deploy/ko/setup-guide/#cloud-agent)が必要です。

### NHN Cloud VM以外のサーバー
![SSH接続必須](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_20_202307.png)

> NHN Cloud VM以外のサーバーの場合、SSH接続のみでサーバーの配布コマンドを伝達できます。
> 配布前に配布ターゲットサーバーとSSHで接続する必要があるため、
> ターゲットサーバーのIP、ポート、ファイアウォール例外処理などの[SSH接続のための準備](/Dev%20Tools/Deploy/ko/setup-guide/#ssh)が必要です。

## NHN Cloud Agentを使用するための準備

### OS別NHN Cloud Agentのインストール
* NHN Cloud Agentで配布コマンドを伝達するにはNHN Cloud Agentをインストールする必要があります。
* NHN Cloud Instanceサービスでインスタンスを作成する際、**追加設定** > **ユーザースクリプト**に下記のLinux、Windows OSに合ったインストールスクリプト内容を追加してインストールできます。
  ![ユーザースクリプト](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_21_202307.png)
* **追加設定** > **ユーザースクリプト**が使用できない場合は、直接インスタンスに接続してインストールスクリプトを実行します。

#### Linuxインストールスクリプト
```
#!/bin/bash
curl 'https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/qemu/cloud_agent_install_linux_1.0.0.sh' | sudo bash
```

#### Windowsインストールスクリプト
```
#ps1_sysnative
Invoke-WebRequest -UseBasicParsing 'https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/qemu/cloud_agent_install_windows_1.0.0.ps1' | Invoke-Expression
```

### NHN Cloud Agentのインストール確認
* NHN Cloud Deployサービスで作成したインスタンスを追加してサーバーグループを作成します。
    * サーバーグループを作成する際、**OS**及び**Shell Type**を必ずご確認ください。**Shell Type**のデフォルト値は /bin/bash(Linux), powershell(Windows)です。

![deploy_14_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_14_202307.png)
![deploy_15_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_15_202307.png)


* **配布**タブに移動した後、上記の手順で作成したサーバーグループを選択し、**シナリオ**項目で**新規作成**をクリックします。

![deploy_16_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_16_202307.png)

* 左側の入力欄にシナリオ名を入力し、**Taskの追加**をクリックして**Normal Task**の**User Command**を選択します。

![deploy_22_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_22_202307.png)

* **Command**に`pwd`などの影響を与えないコマンドを入力し、**作成**をクリックします。

![deploy_23_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_23_202307.png)

* **有効性の確認**をクリックします。

![deploy_17_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_17_202307.png)

![deploy_18_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_18_202307.png)

NHN Cloud Agentサービスのインストール及び有効性確認に成功しました。

## SSH接続のための準備

### OS別要件
#### Linux
* curl 7.19.7-43バージョン以上

#### Windows
* SSHインストール必要
    * OpenSSH_for_Windows_8.6p1, LibreSSL 3.3.3バージョン以上
        * Windows Server 2019を使用する場合、OpenSSHを別途インストールする必要があります
    * SSH Shell: PowerShellを指定

### NHN Cloud VM配布要件
#### グローバルIPの付与
* NHN CloudのVMインスタンスに配布するにはVMインスタンス[Floating IP](https://docs.nhncloud.com/ko/Compute/Instance/ko/console-guide/#ip_1)を作成してグローバルIPを付与する必要があります。

#### セキュリティ例外追加
* 配布するVMインスタンスの[セキュリティグループ](https://docs.nhncloud.com/ko/Compute/Instance/ko/console-guide/#_13)にDeployサービスIP(下記)をSSH Ruleに追加します。
```
133.186.185.112/28
117.52.123.201/32
117.52.123.202/32
```
##### 参考)セキュリティ例外を追加する方法

![deploy_01_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_01_202307.png)

1. NHN Cloudコンソールの**Network**サービスのうち**Security Groups**を選択します。
2. 現在のVMに設定されているセキュリティグループを選択するか、**+ セキュリティグループを作成**をクリックして新規セキュリティグループを作成します。
3. **+** ボタンをクリックします。
    * 方向:受信を選択します。
    * IPプロトコル:ユーザー定義TCPを選択します。
    * ポート: 22を入力します。(SSH Port)
    * 遠隔: CIDRにIPを入力します。帯域を入力することもできます。(例：133.186.185.112/28)

### NHN Cloud VM以外のサーバーの配布要件
#### グローバルIPの付与
* SSH接続のためにグローバルIPを付与する必要があります。

#### ファイアウォール及びNetwork ACL設定
* 外部からアクセスできるように、以下のIPに対してネットワークとファイアウォールの例外設定を追加してください。
```
133.186.185.112/28
117.52.123.201/32
117.52.123.202/32
```

- - -

SSH接続の準備またはNHN Cloud Agentサービスインストール及び有効性確認が完了した場合、Deployサービスを使用して配布できます。
詳細については、[Deploy > コンソール使用ガイド](/Dev%20Tools/Deploy/ko/console-guide/)で確認できます。
