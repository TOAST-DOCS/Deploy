<!-- pre-align:aligned sig=a87ab717aea2 -->

<a id="dev-tools-deploy-setup-guide-before-use"></a>

## Dev Tools > Deploy > 使用前の設定ガイド

この文書では、次のような内容を説明します。

* [サービス使用前の必須事項](/Dev%20Tools/Deploy/ja/setup-guide/#pre-requisites-before-using-the-service)
* [NHN Cloud Agent使用のための準備](/Dev%20Tools/Deploy/ja/setup-guide/#prepare-to-use-nhn-cloud-agent)
* [SSH接続のための準備](/Dev%20Tools/Deploy/ja/setup-guide/#prepare-for-an-ssh-connection)

<a id="pre-requisites-before-using-the-service"></a>

## サービス使用前の必須事項 { #pre-requisites-before-using-the-service }

<a id="nhn-cloud-vm-server"></a>

### NHN Cloud VMサーバー { #nhn-cloud-vm-server }
![SSH接続必須](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_19_202307.png)

> NHN Cloud VMサーバーの場合、SSH接続またはNHN Cloud Agentでサーバーのデプロイコマンドを伝達します。
> SSH接続の場合ターゲットサーバーのIP、ポート、ファイアウォール例外処理などの[SSH接続のための準備](/Dev%20Tools/Deploy/ja/setup-guide/#prepare-for-an-ssh-connection)が必要です。
> NHN Cloud Agentの場合NHN Cloud Agentインストール、有効性確認などの[NHN Cloud Agentを使用するための準備](/Dev%20Tools/Deploy/ja/setup-guide/#prepare-to-use-nhn-cloud-agent)が必要です。

<a id="servers-other-than-nhn-cloud-vm"></a>

### NHN Cloud VM以外のサーバー { #servers-other-than-nhn-cloud-vm }
![SSH接続必須](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_20_202307.png)

> NHN Cloud VM以外のサーバーの場合、SSH接続のみでサーバーのデプロイコマンドを伝達できます。
> デプロイ前にデプロイターゲットサーバーとSSHで接続する必要があるため、
> ターゲットサーバーのIP、ポート、ファイアウォール例外処理などの[SSH接続のための準備](/Dev%20Tools/Deploy/ja/setup-guide/#prepare-for-an-ssh-connection)が必要です。

<a id="prepare-to-use-nhn-cloud-agent"></a>

## NHN Cloud Agentを使用するための準備 { #prepare-to-use-nhn-cloud-agent }

<a id="install-nhn-cloud-agent-by-operating-system"></a>

### OS別NHN Cloud Agentのインストール { #install-nhn-cloud-agent-by-operating-system }
* NHN Cloud Agentでデプロイコマンドを伝達するにはNHN Cloud Agentをインストールする必要があります。
* NHN Cloud Instanceサービスでインスタンスを作成する際、**追加設定** > **ユーザースクリプト**に下記のLinux、Windows OSに合ったインストールスクリプト内容を追加してインストールできます。
  ![ユーザースクリプト](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_21_202307.png)
* **追加設定** > **ユーザースクリプト**が使用できない場合は、直接インスタンスに接続してインストールスクリプトを実行します。

<a id="linux-installation-script"></a>

#### Linuxインストールスクリプト
```
#!/bin/bash
curl 'https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/qemu/cloud_agent_install_linux_1.0.0.sh' | sudo bash
```

<a id="windows-installation-scripts"></a>

#### Windowsインストールスクリプト
```
#ps1_sysnative
Invoke-WebRequest -UseBasicParsing 'https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/qemu/cloud_agent_install_windows_1.0.0.ps1' | Invoke-Expression
```

<a id="check-the-installation-of-nhn-cloud-agent"></a>

### NHN Cloud Agentのインストール確認 { #check-the-installation-of-nhn-cloud-agent }
* NHN Cloud Deployサービスで作成したインスタンスを追加してサーバーグループを作成します。
    * サーバーグループを作成する際、**OS**及び**Shell Type**を必ずご確認ください。**Shell Type**のデフォルト値は /bin/bash(Linux), powershell(Windows)です。

![deploy_14_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_14_202307.png)
![deploy_15_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_15_202307.png)


* **デプロイ**タブに移動した後、上記の手順で作成したサーバーグループを選択し、**シナリオ**項目で**新規作成**をクリックします。

![deploy_16_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_16_202307.png)

* 左側の入力欄にシナリオ名を入力し、**Taskの追加**をクリックして**Normal Task**の**User Command**を選択します。

![deploy_22_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_22_202307.png)

* **Command**に`pwd`などの影響を与えないコマンドを入力し、**作成**をクリックします。

![deploy_23_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_23_202307.png)

* **有効性の確認**をクリックします。

![deploy_17_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_17_202307.png)

![deploy_18_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_18_202307.png)

NHN Cloud Agentサービスのインストール及び有効性確認に成功しました。

<a id="prepare-for-an-ssh-connection"></a>

## SSH接続のための準備 { #prepare-for-an-ssh-connection }

<a id="requirements-for-each-os"></a>

### OS別要件 { #requirements-for-each-os }
<a id="linux"></a>

#### Linux
* curl 7.19.7-43バージョン以上

<a id="windows"></a>

#### Windows
* SSHインストール必要
    * OpenSSH_for_Windows_8.6p1, LibreSSL 3.3.3バージョン以上
        * Windows Server 2019を使用する場合、OpenSSHを別途インストールする必要があります
    * SSH Shell: PowerShellを指定

<a id="requirements-for-nhn-cloud-vm-deployment"></a>

### NHN Cloud VMデプロイ要件 { #requirements-for-nhn-cloud-vm-deployment }
<a id="assign-public-ip"></a>

#### グローバルIPの付与
* NHN CloudのVMインスタンスにデプロイするにはVMインスタンス[Floating IP](https://docs.nhncloud.com/ja/Compute/Instance/ja/console-guide/#ip_1)を作成してグローバルIPを付与する必要があります。

<a id="add-security-exceptions"></a>

#### セキュリティ例外追加
* デプロイするVMインスタンスの[セキュリティグループ](https://docs.nhncloud.com/ja/Compute/Instance/ja/console-guide/#_13)にDeployサービスIP(下記)をSSH Ruleに追加します。
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

<a id="requirements-for-server-deployment-other-than-nhn-cloud-vm"></a>

### NHN Cloud VM以外のサーバーのデプロイ要件 { #requirements-for-server-deployment-other-than-nhn-cloud-vm }
<a id="requirements-for-server-deployment-other-than-nhn-cloud-vm-assign-public-ip"></a>

#### グローバルIPの付与
* SSH接続のためにグローバルIPを付与する必要があります。

<a id="configure-firewalls-and-network-acl"></a>

#### ファイアウォール及びNetwork ACL設定
* 外部からアクセスできるように、以下のIPに対してネットワークとファイアウォールの例外設定を追加してください。
```
133.186.185.112/28
117.52.123.201/32
117.52.123.202/32
```

- - -

SSH接続の準備またはNHN Cloud Agentサービスインストール及び有効性確認が完了した場合、Deployサービスを使用してデプロイできます。
詳細については、[Deploy > コンソール使用ガイド](/Dev%20Tools/Deploy/ja/console-guide/)で確認できます。
