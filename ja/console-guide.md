## Dev Tool > Deploy > コンソール使用ガイド

この文書では、次のような内容を扱います。

* [サービス使用前の必須事項](/Dev%20Tool/Deploy/ja/console-guide/#_1)
* [Deployコンソール画面](/Dev%20Tool/Deploy/ja/console-guide/#_4)
* [Client Application](/Dev%20Tool/Deploy/ja/console-guide/#client-application)
* [Server Application](/Dev%20Tool/Deploy/ja/console-guide/#server-application)

(ここで扱わない機能は、[機能詳細ガイド](/Dev%20Tool/Deploy/ja/reference/)で確認できます。)

## サービス使用前の必須事項

![SSH接続必須](http://static.toastoven.net/prod_tcdeploy/getstarted/console_ssh_required.png)


> TOAST Deployは、SSH接続でサーバーの配布コマンドを伝達します。 
> 配布前の配布target serverとSSHで接続する必要があるため
> target serverのIP、ポート、ファイアウォール例外処理などのSSH接続のための準備が必要です。


### OS別要求事項
#### Linux
* curl 7.19.7-43バージョン以上

#### Window
* SSHインストール必要
    * SSH Shell: PowerShell指定

### TOAST VM配布要求事項
#### 公認IP付与
* TOASTのVMインスタンスに配布するには、VMインスタンス[Floating IP](https://docs.toast.com/ja/Compute/Instance/ja/console-guide/#ip_1)を作成して、公認IPを付与する必要があります。

#### セキュリティー例外の追加
* 配布するVMインスタンスの[セキュリティーグループ](https://docs.toast.com/ja/Compute/Instance/ja/console-guide/#_13)に、DeployサービスIP(下記)をSSH Ruleで追加します。
```
133.186.185.112/28
133.186.188.208/28
```
##### 参考)セキュリティー例外追加方法

![deploy_01_201812](https://static.toastoven.net/prod_tcdeploy/deploy_01_201812.png)

1. TOASTコンソールの**Compute**サービスの中から**Instance**を選択します。
2. 現在VMに設定されているセキュリティーグループを選択するか、**+ Security Group作成**をクリックして新規セキュリティーグループ(Security Group)を作成します。
3. **+ Rule追加**ボタンをクリックします。 
    * Rule：SSHを選択します。
    * CIDRにIPを入力します。
    * 帯域を入力することもできます(例：133.186.188.208/28)。

### TOAST VM以外のサーバー配布要求事項
#### 公認IP付与
* SSH接続のために公認IPを付与する必要があります。

#### ファイアウォールおよびNetwork ACL設定
* 外部からアクセスできるように、下記IPに対してネットワークとファイアウォール例外設定を追加してください。
```
133.186.185.112/28
133.186.188.208/28
```

## Deployコンソール画面

次は、Deployサービスのコンソール画面です。

![deploy_02_201812](https://static.toastoven.net/prod_tcdeploy/deploy_02_201812.png)

## Client Application

クライアントアプリケーション配布設定は、大きく分けてアーティファクト設定とバイナリアップロードの段階を経ます。

### アーティファクト設定

![deploy_03_201812](https://static.toastoven.net/prod_tcdeploy/deploy_03_201812.png)

1. **Deploy**画面左上で**作成**ボタンをクリックします。
2. アーティファクトタイプは**Client Application**を選択します。
    - 名前(必須)、説明(任意)、port(必須)を入力します。
3. **作成**ボタンをクリックします。

### バイナリ設定

#### アップロード

*iOSは.ipa、.plistファイルを、Androidは.apkファイルをそれぞれアップロードします。
* etcの場合は、Windowsなどのその他OSのインストールアプリケーション用途に使用します。

![deploy_04_201812](https://static.toastoven.net/prod_tcdeploy/deploy_04_201812.png)

1. **Deploy**画面の下にあるタブで、**バイナリグループ > Default**をクリックします。
   新しいバイナリグループを作成するには、**新規作成**ボタンをクリックします。
2. 右にある**アップロード**ボタンをクリックします。
3. **バイナリアップロード**ウィンドウで**ファイル選択**ボタンをクリックし、バイナリファイルを選択します。
    * iOS：.ipaファイル(必須)、.plistファイル(必須)
        * .plist：ダウンロードページで、インストールのために使用します。ファイル内のダウンロードURLは任意入力です。
    * Android：.apkファイル(必須)
    * バージョン(任意)、説明(任意)情報入力
4. 入力完了後、**アップロード**ボタンをクリックします。

#### 配布

特定バイナリダウンロードページをSMSやE-mailに伝達できます。

![deploy_05_201812](https://static.toastoven.net/prod_tcdeploy/deploy_05_201812.png)

1. 右にある**送信**ボタンをクリックします。
2. **ダウンロードパス送信**ウィンドウで送信タイプと受信者を選択し、**送信**ボタンをクリックします。
    * SMS、E-mailのどちらか1つまたは両方を選択できます。

指定した送信タイプで、受信者にバイナリダウンロードページが伝達されます。

## Server Application

サーバーアプリケーション配布設定(アーティファクト、サーバーグループ、シナリオ)、バイナリアップロード、配布段階を経ます。

### アーティファクト設定

![deploy_06_201812](https://static.toastoven.net/prod_tcdeploy/deploy_06_201812.png)

1. リスト上にある**作成**ボタンをクリックします。
2. アーティファクトタイプは**Server Application**を選択します。
    - 名前(必須)、説明(任意)、port(必須)項目を入力します。
3. **アーティファクト作成**ウィンドウで**作成**ボタンをクリックします。

### サーバーグループ設定

配布するサーバーを管理できる機能です。

![deploy_07_201812](https://static.toastoven.net/prod_tcdeploy/deploy_07_201812.png)

1. **Deploy**画面下にあるタブで、**サーバーグループ > 新規作成**をクリックします。
2. **サーバーグループ作成**ウィンドウで、新規作成するサーバーグループを設定します。
    * 名前(必須)、説明(任意)を入力します。
    * OSを選択し、Shell Typeを指定します。 Shell Typeは**Shell Type**リストから選択するか、直接入力できます。
    * Phaseを選択します。サーバー機器を区分します。指定しない場合はNONEを選択します。
    * サーバー追加
        * サーバーを追加する方法は、下記の2つです。詳細は[機能詳細ガイドサーバーグループメニュー](/Dev%20Tool/Deploy/ja/reference/#_11)で確認できます。
            * 大量追加
            * 個別追加
         * ホスト名(必須)、IPアドレス(必須)、OS(任意)を入力し、**追加**ボタンをクリックします。
         * 下記のサーバーリストに追加された内容を確認します。左にあるチェックボックスが選択されたサーバーのみ登録されます。

3. 入力完了後、**作成**ボタンをクリックします。

### シナリオ作成

![deploy_08_201812](https://static.toastoven.net/prod_tcdeploy/deploy_08_201812.png)

1. **Deploy**画面の下にあるタブで、**配布 > 新規作成**ボタンをクリックします。
2. 下に追加されたシナリオ領域にシナリオ名(任意)を入力します。
3. **作成**ボタンをクリックします。

### タスク追加

タスクは、個別機能を実行して順序を制御できるシナリオ構成要素です。
タスクの種類は、下記の2つです。

* pre-run Task：配布前の実行機能
* Normal Task：配布時の実行機能

希望するタスクを選択して使用できます。ここでは配布時に必要な基本的なタスクを扱います。
その他のタスクは[機能詳細ガイドのタスクメニュー](/Dev%20Tool/Deploy/ja/reference/#_25)で確認できます。

配布テストのために、下記の3つのタスクを追加します。

#### 1. User Command追加

* 配布時に実行されるユーザー定義Commandタスクです。
* Available Variablesを使用できます。
    * Available Variables：予約語。詳細は[機能詳細ガイドのタスクメニュー](/Dev%20Tool/Deploy/ja/reference/#_25)で確認できます。

![deploy_09_201812](https://static.toastoven.net/prod_tcdeploy/deploy_09_201812.png)

1. **配布**タブシナリオ領域の右にある**Task追加**ボタンをクリックします。
2. **Normal Task**の下にある**User Command**をクリックします。
3. 新しいタスク内容を入力します。
    * Timeout(min)
        * 該当タスクの実行完了待機時間を指定します。最短1分、最長30分。
    * Run As
        * 実行アカウントを入力します。
    * Command
        * 実行するコマンド文を入力します。

4. 入力/変更完了後、**適用**ボタンをクリックします。 

#### 2. Binary Deploy追加

アップロードしたバイナリファイルの配布内容を設定できるタスクです。

![deploy_10_201812](https://static.toastoven.net/prod_tcdeploy/deploy_10_201812.png)

1. **Task追加**ボタンをクリックして、**Normal Task**の下にある**Binary Deploy**をクリックします。
2. 新しいタスク内容を入力します。
    * Timeout(min)
        * 該当タスクの実行完了待機時間を指定します。最短1分、最長30分。
    * Run As
        * 実行アカウントを入力します。
3. バイナリファイルをアップロードするには、右にある**アップロード**ボタンをクリックします。
4. バイナリファイル情報を入力します。
    * **ファイル選択**ボタンをクリックして、バイナリファイルを選択します。
    * バージョン(任意)、説明(任意)項目を入力します。
5. **アップロード**ボタンをクリックします。 
6. アップロード完了後、**バイナリ選択**ボタンをクリックします。
7. 希望するバイナリバージョンを選択します。
    * 複数のバージョンがある時は、検索機能を活用します。
8. **選択**ボタンをクリックします。
  <br/>
   * Variable As
       * 該当バイナリのVariable名を指定し、User Commandでバイナリ情報を使用できます。詳細は[機能詳細ガイド](/Dev%20Tool/Deploy/ja/reference/)のタスクメニュー下段で確認できます。
   * ターゲットディレクトリ
       * バイナリを配布するターゲットディレクトリを指定します。

#### 3. User Command追加

![deploy_11_201812](https://static.toastoven.net/prod_tcdeploy/deploy_11_201812.png)

1. **Task追加**ボタンをクリックし、**Normal Task**下にある**User Command**をクリックします。
2. 新しいタスク内容を入力します。
    * Timeout(min)
        * 該当タスクの実行完了待機時間を指定します。最短1分、最長30分。
    * Run As
        * 実行アカウントを入力します。
    * Command
        * 実行するコマンド文を入力します。
3. 入力/変更完了後に**適用**ボタンをクリックします。 

### 実行

![deploy_12_201812](https://static.toastoven.net/prod_tcdeploy/deploy_12_201812.png)

1. 右にある**実行**ボタンをクリックして配布を要請します。
2. 配布実行情報を入力します。
    * 配布ノート(任意)および認証方法を指定します。
    * **Password**パスワードを入力するか、.pemファイルを選択してアップロードします。
3. 入力完了後、**確認**ボタンをクリックします。

![deploy_13_201812](https://static.toastoven.net/prod_tcdeploy/deploy_13_201812.png)

1. 配布進行状況を確認できます。
2. 配布完了を確認します。
    * 各タスクが正常に実行されたかどうかは、exit codeで確認できます。
3. 詳細結果を確認するには、**結果表示**ボタンをクリックします。
4. **結果表示**ウィンドウで配布結果を確認します。
    * 各タスク実行に対する詳細(リターン値、 exit code、エラー内容など)を確認できます。

- - -

サーバーにファイルを配布しました！
TOAST Deployは、他にも多くの機能をサポートしています。詳細は[機能詳細ガイド](/Dev%20Tool/Deploy/ja/reference/)で確認できます。
