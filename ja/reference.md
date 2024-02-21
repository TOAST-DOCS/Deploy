## Dev Tools > Deploy > 機能詳細ガイド

この文書では、次のような内容を扱います。

* [メニュー説明](/Dev%20Tools/Deploy/ja/reference/#_1)
* [機能別説明](/Dev%20Tools/Deploy/ja/reference/#_18)

## メニュー説明

![deploy_ref_01_ja_20200527](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_01_ja_20200527.png)

### 1. アーティファクトメニュー領域

配布を管理するDeploy構成の基本単位です。
作成されたアーティファクトは、画面上部にリスト形式で表示されます。

### 2. 配布メニュー領域

配布を行うページでシナリオ作成と設定ができます。
シナリオにはJenkins-CLIテンプレート、バイナリ配布、ファイル配布があり、ユーザーが直接コマンドを入力でき、自由にスクリプトを実行できます。

* 配布シナリオ管理
    * コピー
    * 設定
    * 新規作成
    * アップロード/ダウンロード
        * シナリオ単位のインポート/エクスポート
* サーバーグループとシナリオを選択して配布
* サーバーグループ内サーバー同時/個別配布
* 配布ノートに配布詳細内容を入力すると、**配布履歴**タブの配布別結果参照ウィンドウで確認できます。 

#### 配布オプション

##### サーバーグループ

* 配布するサーバーグループを選択できます。サーバーグループは**サーバーグループ**タブで作成できます。

##### 実行サーバー

* **すべてのサーバー**
    * 指定されたサーバーグループのすべてのサーバーに配布します。
* **サーバー選択**
    * 指定されたサーバーグループの一部のサーバーを選択して配布します。

##### 同時実行数

指定されたサーバーグループ内のサーバーに、同時または個別配布します。

* 0：同時実行
* 1：1台ずつ配布
* N：同時配布数を指定

##### シナリオ実行失敗時

* **実行中断**
    * シナリオ実行中にエラーが発生すると、シナリオ実行が中断されます。
* **無中断**
    * エラーの有無に関係なく、シナリオが継続して実行されます。

#### 進行中の配布状態を確認

配布中のシナリオの進行状況を確認できます。

![deploy_ref_02_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_02_ja_20200527.png)

* 配布中のアーティファクトをクリックし、表示されるヘッダで確認したい配布項目をクリックします。
* 配布履歴画面で'deploying'状態をクリックします。

### 配布履歴
配布履歴と配布設定、配布ノートの詳細な内容を確認できます。
![deploy_ref_01_2021.png](https://static.toastoven.net/prod_tcdeploy/reference/deploy_ref_01_2021.png)
![deploy_ref_02_2021.png](https://static.toastoven.net/prod_tcdeploy/reference/deploy_ref_02_2021.png)
* 配布履歴と履歴別の詳細内容を確認
    * 配布結果
    * **結果参照**で、履歴別シナリオ設定内容とタスク別実行結果、配布ノートを確認できます。
    * 'deploying'状態の時にクリックすると、配布中の状態表示画面に移動します。

配布履歴を実行日およびサーバーグループを基準に検索できます。
![deploy_ref_03_2021.png](https://static.toastoven.net/prod_tcdeploy/reference/deploy_ref_03_2021.png)
![deploy_ref_04_2021.png](https://static.toastoven.net/prod_tcdeploy/reference/deploy_ref_04_2021.png)
* 配布履歴を実行日およびサーバーグループを基準に検索
    * サーバーグループ選択ウィンドウおよび実行日(開始日～終了日)で検索できます。
    * 実行日期間が1年を超えているため選択できません(例：2020-06-07~2021-06-17)。
     ![deploy_ref_08_2021.png](https://static.toastoven.net/prod_tcdeploy/reference/deploy_ref_08_2021.png)
0
検索された配布履歴をExcelファイルでダウンロードできます。
![deploy_ref_05_2021.png](https://static.toastoven.net/prod_tcdeploy/reference/deploy_ref_05_2021.png)
* 検索された配布履歴をExcelファイルでダウンロード
    * サーバーグループおよび実行日を選択した後、**ダウンロード**ボタンをクリックします。
    * **バイナリファイルがない履歴を除外**オプションを選択してダウンロードできます。
    * **バイナリファイルがない履歴を除外**を選択せずに**ダウンロード**をクリックした時
    ![deploy_ref_07_2021.png](https://static.toastoven.net/prod_tcdeploy/reference/deploy_ref_07_2021.png)
    * **バイナリファイルがない履歴を除外**を選択して**ダウンロード**をクリックした時
    ![deploy_ref_06_2021.png](https://static.toastoven.net/prod_tcdeploy/reference/deploy_ref_06_2021.png)
    
### バイナリグループ

**バイナリグループ**タブでは、バイナリをグループで管理できます。
Develop、Staging、Productなどのサーバー機器に配布されるバイナリを区分する時に活用できます。

![deploy_ref_04_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_04_ja_20200527.png)

* グループの作成/修正、バイナリファイルのアップロード/ダウンロードが行えます。

![deploy_24_202402.png](https://static.toastoven.net/prod_tcdeploy/deploy_24_202402.png)
* Clientタイプの時は、バイナリグループのパスワードを設定でき、共有されたクライアントダウンロードページのアクセスを制御できます。
    * バイナリグループを作成する時、**バイナリグループパスワード使用**を選択すると、パスワードを入力できます。
    * 該当グループダウンロードページは、NHN Cloudにログインしなくても、設定されたパスワードだけでアクセスできます。
* 自動削除設定に該当バイナリグループの自動削除ポリシーを設定できます。
    * 自動削除設定の各項目を空の値にしてバイナリグループを作成した場合、自動削除設定は適用されません。

### サーバーグループ

配布対象サーバーをグループで管理できます。
PhaseプロパティでDevelop、Staging、Productなどのサーバー機器を区分して活用できます。
Auto Scaleサービスのインスタンス拡張に応じて配布設定が行えます。
![deploy_ref_07_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_07_ja_20200527.png)

* グループ、サーバー情報を修正できます。
* グループで使用するシナリオを設定できます。

#### サーバーグループ追加

![deploy_ref_08_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_08_ja_20200527.png)

* **Deploy**の下にあるタブから、**配布**> **サーバーグループ作成**をクリックするか、**サーバーグループ**> **新規作成**をクリックします。
    * 名前(必須)、説明(任意)項目を入力します。
    * OSを選択した後、Shell Typeを指定します。リストから項目を選択するか、直接入力します。
    * Phaseを選択します。サーバー機器を区分します。指定しない時はNONEを選択します。
    * **作成**ボタンをクリックします。

#### サーバー情報追加/削除

**サーバーグループ作成(修正)**ウィンドウでサーバー情報の追加や削除ができます。

##### サーバー情報追加

1. 個別追加

![deploy_ref_08_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_08_ja_20200527.png)

* ホスト名(必須)、IPアドレス(必須)、OS(任意)を入力し、**追加**ボタンをクリックします。
* 下にあるサーバーリストに追加された内容を確認します。左にあるチェックボックスを選択したサーバーのみ登録されます。
* **作成**ボタンをクリックします。修正する時は**修正**ボタンをクリックします。

2. 大量追加

![deploy_ref_09_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_09_ja_20200527.png)

* **大量入力**チェックボックスを選択します。
* 下記のような形式でサーバー情報を入力します。
    ```
    test.host.name1;1.1.1.1;CentOS7.9;
    test.host.name2;2.2.2.2;
    ```

* **追加**ボタンをクリックします。
* サーバー情報が2個追加されたことを確認します。
* **作成**ボタンをクリックします。修正する時は**修正**ボタンをクリックします。

##### サーバー情報の削除

* 左にあるチェックボックスを選択解除するか、右にある**削除**ボタンをクリックします。
* **作成**ボタンをクリックします。修正する時は**修正**ボタンをクリックします。

#### オートスケール グループ追加
![autoscale_01.png](https://static.toastoven.net/prod_tcdeploy/reference/autoscale_01.png)
* **配布** > **サーバーグループ作成**をクリックするか、**サーバーグループ > 新規作成**
    * 名前(必須)、説明(任意)項目を入力します。
    * OSを選択した後、Shell Typeを指定します。リストから項目を選択するか、直接入力できます。
    * Phaseを選択します。サーバー機器を区分します。指定しない場合はNONEを選択します。
    * グループタイプで**オートスケーリングサーバーグループ**を選択します。
    * スケーリンググループは、Auto Scaleサービスで作成したスケーリンググループを選択します。
    * スケールアウトシナリオのスケーリンググループで、スケールアウトした時にインスタンスに実行するシナリオを選択します。
    * **作成** ボタンをクリックします。

##### スケールアウトシナリオ追加
![autoscale_02.png](https://static.toastoven.net/prod_tcdeploy/reference/autoscale_02.png)
* グループ作成ダイアログボックスで**シナリオ追加**ボタンをクリックします。
* 実行するシナリオがあるArtifactを選択します。
* 実行したシナリオを選択します。
* 複数のシナリオを選択できます。
* **確認**ボタンをクリックします。
* 追加されたシナリオ順序を指定するために実行優先順位を変更します。優先順位が同じ場合、ランダムな順序で実行されます。
* **作成**ボタンをクリックします。 

### リソース

リソースを管理できるページで、ファイル作成、アップロード、ダウンロード、修正や変更履歴の確認ができます。

![deploy_ref_10_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_10_ja_20200527.png)

## 機能別説明

ここでは、Getting Startedで扱わない機能と追加設定を詳しく説明します。

### バイナリ

バイナリは、アップロードされた配布対象ファイルです。

#### アップロード

バイナリをアップロードできる方法は2つです。

* APIアップロード
    * APIアップロードの詳細説明は、[APIガイドのBinary Upload API](/Dev%20Tools/Deploy/ja/api-guide/#binary-upload-api)で確認できます。
* コンソールでアップロード

![deploy_ref_11_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_11_ja_20200527.png)

1. **Deploy**の下にあるタブの中から**バイナリグループ**をクリックし、**アップロード**ボタンをクリックします。
2. **ファイル選択**ボタンをクリックし、バイナリファイルを選択します。
3. バージョン(任意)、説明(任意)情報を入力します。
4. **アップロード**ボタンをクリックします。

![deploy_ref_12_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_12_ja_20200527.png)

#### 1. ダウンロード

バイナリリスト右にあるダウンロードボタンをクリックします。

#### 2. バージョンfix

Client OSごとに、1個ずつバージョンfixができます。

* 状態区分
    * <span style="color:red">FIXED</span>(on)
        * クリックすると、該当バージョンをunfixします。
    * <span style="color:gray">FIXED</span>(off)
        * クリックすると該当バージョンをfixします。

#### 3. 配布

##### バージョン別配布

ClientバイナリのAll、Fixed、Recentバージョンを、希望する方式で配布できます。

* バイナリダウンロードリンク提供
* ダウンロードリンク通知機能の提供
    * 該当プロジェクトに登録されたメンバーにSMS、E-Mail送信

* 提供タイプ
    * All Version List
        * すべてのバージョンをダウンロードできるページの提供
    * Fixed Version
        * iOS、Android、etc
            * Fixedバージョンダウンロードページの提供(Fixedバージョンがある時)
    * Recent Version
        * iOS、Android、etc
            * 最新バージョンダウンロードページの提供
* 提供方法
    * SMS、E-Mail

##### 選択配布

特定バイナリダウンロードページをSMSやE-Mailで伝達できます。

![deploy_ref_13_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_13_ja_20200527.png)

1. **Deploy**の下にある**バイナリグループ**タブで、配布するバイナリファイルを選択します。
2. 右にある**送信**ボタンをクリックします。
3. **ダウンロードパス送信**ウィンドウで送信タイプと受信者を選択します。
    * SMS、E-mailのどちらか1つまたは両方を選択できます。
4. **送信**ボタンをクリックします。

指定した送信タイプで受信者にバイナリダウンロードページが伝達されます。

### タスク

タスクは、個別機能の実行および順序制御が可能なシナリオ構成要素です。

* Pre-run Task ：配布前の処理作業が可能なタスク
    * 対象：サーバーグループに保存しない別途のサーバー指定可
* Normal Task：配布プロセスのうち、実行されるタスク
    * 対象：サーバーグループまたはサーバーグループ内の指定サーバー

##### タスク利用ガイド

* タスクタイプ

![deploy_ref_14_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_14_ja_20200527.png)

1\. Pre-run Task
2\. Normal Task

* タスク順序
    * Pre-runタスクは、領域の上部にあります。
    * Pre-runタスクまたはNormalタスクの各領域内では、位置を自由に動かすことができます。

3\. タスク制御

* 各タスクの右上にある**∧**(上に移動)**∨**(下に移動)**X**(Task除去)ボタンで、タスクを制御できます。
* 予約語の使用
    * 詳細は下記Available Variablesを参照してください。

#### Pre-run Task

* Target Server：別途にTarget Serverを指定した一回性の処理ができます。

##### Jenkins-CLI Build

* ver. 2.46以前と/ver. 2.46以降のバージョンに区分されます。
* Jenkinsビルド設定の詳細説明は、[プラグイン使用ガイド](/Dev%20Tools/Deploy/ja/plugin-guide/)を参照してください。

![deploy_ref_15_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_15_ja_20200527.png)

**[ユーザー入力内容]**

* OS選択
* Run As入力
    * 実行アカウントを入力します。
* Jenkins Server
    * ドメインまたはIPを入力します。
* Server Charset
    * Jenkins-CLIを実行するサーバーの文字コードを入力します。何も値を入力しない場合、デフォルト値のUTF-8です。
* Jenkins Url
    * ビルドコマンドを実行するJenkinsサーバーアドレスを入力します。
* Jenkins Charset
    * Jenkinsサーバーの文字コードを入力します。何も値を入力しない場合、デフォルト値のUTF-8です。
* Java Home
    * Jenkins-CLIを実行するには、JDKまたはJREのHomeパスを入力します。
* Job Name
    * 実行するビルドJobの名前を入力します。
* Private Key Path
    * SSH Public Keyが設定されているJenkinsユーザーで認証するために、SSH Private Keyファイルの保存パスを入力します。
* User(ver. 2.46以上のバージョン必須値)
	* Public Keyが設定されているIDを入力します。
		* 参考事項
			* 2.46以前のバージョン
				* Public Keyが設定されている場合、Jenkinsで自動認証が行われます。
			* 2.46以上のバージョン
				* User(Jenkins admin ID)が追加され、該当IDにPublic Keyが設定された場合のみ認証が通ります。
* Build Parameter(任意)
    * ビルドに必要なパラメータを入力します。

##### User Command

* ユーザー定義Commandタスクです。
* Available Variables(予約語)を使用できます。
* エラー発生感知およびエラー発生時は、配布が中止されます。
    * **配布**タブの**シナリオ実行失敗時**で、**実行中断**が設定されている時

![deploy_ref_16_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_16_ja_20200527.png)

**[ユーザー入力内容]**

* OS選択
* Run As入力
    * 実行アカウントを入力します。
* Target Server
    * target server情報を入力します。サーバーグループに含まれているか、含まれていないサーバー情報も入力できます。
* Timeout(min)
    * 該当タスクの実行完了待機時間を指定します。最短1分、最長30分。
* Command
    * 実行するCommandを入力します。
    * 予約語を使用できます。下記のAvailable Variablesメニューを参照してください。

#### Normal Task

* Target Server：別途にTarget Serverを入力せず、サーバーグループで選択されたサーバー情報を使用します。

##### Binary Deploy

* **バイナリグループ**タブで管理するファイルを配布できます。

![deploy_ref_17_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_17_ja_20200527.png)

**[ユーザー入力内容]**

* Timeout(min)
    * 該当タスクの実行完了待機時間を指定します。最短1分、最長30分。

* Run As入力
    * 実行アカウントを入力します。

* バイナリ
    * 配布するバイナリファイルを3つのタイプで選択できます。
        * 最新バージョン使用
            * 最新バージョン使用を選択すると、最新バージョンが自動的に選択されます。(最新バージョンのファイルがある場合)
            * バイナリグループを選択できます。(選択されたグループの最新バージョンが使用される)
        * 選択バージョン使用
            * ![deploy_ref_18_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_18_ja_20200527.png)
                1. **最新バージョン使用**チェックボックスを選択解除します。
                2. **アップロード**ボタンをクリックします。
                3. **ファイル選択**ボタンをクリックして、バイナリファイルを選択します。
                4. 入力完了後、**アップロード**ボタンをクリックします。
                5. アップロード完了後、**バイナリ選択**ボタンをクリックします。
                6. 配布するバイナリバージョンを検索して選択します。
                7. **選択**ボタンをクリックします。

* Variable As
    * 該当バイナリのVariable名を指定します。 CommandでAvailable Variablesを使用できます。

* ターゲットディレクトリ
    * バイナリを配布するターゲットディレクトリを指定します。

##### File Deploy

* **Resource**タブで、管理するファイルを最新バージョンで配布できます。

![deploy_ref_19_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_19_ja_20200527.png)

**[ユーザー入力内容]**

* Timeout(min)
    * 該当タスクの実行完了待機時間を指定します。最短1分、最長30分。

* RunAs入力
    * 実行アカウントを入力します。

* ファイル
    * **ファイル選択**ボタンをクリックし、ファイルを選択します。
    
* ターゲットディレクトリ
    * ファイルを配布するターゲットディレクトリを指定します。

##### User Command

* ユーザー定義Commandタスクです。
* Available Variables(予約語)を使用できます。
* スクリプトエラー発生感知およびエラー発生時は、配布が中止されます。
    * タブメニューのうち、**配布 > シナリオ実行失敗時は実行中断**が設定されている時

![deploy_ref_20_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_20_ja_20200527.png)

**[ユーザー入力内容]**

* Timeout(min)
    * 該当タスクの実行完了待機時間を指定します。最短1分、最長30分。
* Run As入力
    * 実行アカウントを入力します。
* 実行するCommandを入力します。
    * Available Variablesを使用できます。
        * `$${binary.指定したVariables As入力。binaryGroupName}：バイナリに設定した変数名。選択されたバイナリのグループ名`

#### Available Variables

タスクで下記のような予約語を使用できます。

```
[Available Variables]

* 基本タイプ
$${artifact.name}：アーティファクト名
$${artifact.id}：アーティファクトID
$${serverGroup.name}：サーバーグループ名
$${serverGroup.id}：サーバーグループID
$${executor.account}：実行ユーザーのID
$${executor.maskedAccount}：実行ユーザーのマスキングされたID
$${timestamp}：タイムスタンプ
$${timestamp.date pattern}：指定したパターンのタイムスタンプ

* Binary DeployのVariables Asを使用できるタイプ
$${binary.binary variable as value.version}：バイナリに設定した変数名。選択されたバイナリのバージョン
$${binary.binary variable as value.key}：バイナリに設定した変数名。選択されたバイナリのキー
$${binary.binary variable as value.name}：バイナリに設定した変数名。選択されたバイナリの名前
$${binary.binary variable as value.targetDir}：バイナリに設定した変数名。選択されたバイナリの対象パス
$${binary.binary variable as value.binaryGroupKey}：バイナリに設定した変数名。選択されたバイナリのグループキー
$${binary.binary variable as value.binaryGroupName}：バイナリに設定した変数名。選択されたバイナリのグループ名
```

* 使用例
    * 基本タイプ
        * `$${timestamp} : 1514987008`
    * Binary DeployのVariables Asを使用できるタイプ
        * `$${binary.指定したVariables As入力.binaryGroupName}：該当バイナリのグループ名`

### リソース

リソースは任意で使用できるファイル管理機能です。

* ファイルグループ作成
* ファイル追加
* ファイル修正
* ファイルヒストリー
* ファイル履歴管理

#### ファイルグループ作成

![deploy_ref_21_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_21_ja_20200527.png)

1. **新しいファイルグループ**ボタンをクリックします。

2. 名前(必須)、説明(任意)情報を入力します。
3. **確認**ボタンをクリックします。

#### ファイル追加

##### ファイルアップロード

![deploy_ref_22_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_22_ja_20200527.png)

1. **ファイル追加**ボタンをクリックします。
2. **ファイルアップロード**作成方式を選択します。
3. ファイル選択および入力完了後、**アップロード**ボタンをクリックします。

ファイルグループ内でファイル名が重複する場合、アップロードが制限されます。

##### 新規作成

![eploy_ref_23_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_23_ja_20200527.png)

1. **ファイル追加**ボタンをクリックします。
2. **新規作成**作成方式を選択します。
3. **確認**ボタンをクリックします。
4. 新規作成ファイルの内容を入力します。
5. 入力完了後、**保存**ボタンをクリックします。
6. 作成が完了した画面です。

#### ファイル修正

* ファイル説明の修正
* ファイル内容の修正
* ファイルヒストリーで修正
* ファイルヒストリーの管理

##### ファイル説明の修正

ファイル説明を修正できます。

![deploy_ref_24_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_24_ja_20200527.png)

1. ファイル説明欄をクリックして修正します。
2. **説明修正**ボタンをクリックします。

##### ファイル内容の修正

![deploy_ref_25_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_25_ja_20200527.png)

1. ファイル内容を修正します。
2. **保存**ボタンをクリックします。

##### ファイルヒストリー

ファイルの作成/修正、ヒストリーの確認ができます。

![deploy_ref_26_ja_20200527.png](https://static.toastoven.net/prod_tcdeploy/ja/deploy_ref_26_ja_20200527.png)

1. **File History**ボタンをクリックして、リストを拡張します。
    * リスト確認
        * 最初の履歴：現在のバージョンを意味します。
        * 以降の履歴：以前のバージョンです。クリックすると詳細ウィンドウに内容が表示されます。

2. 確認したい項目をクリックすると、該当バージョンの詳細内容を確認できます。

##### ファイルヒストリー管理

ファイルヒストリー詳細履歴で、下記の機能を提供します。

* **最新バージョンで保存**ボタンをクリックすると、該当バージョンが最新バージョンで保存されます。
