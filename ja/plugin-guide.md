## Dev Tools > Deploy > プラグイン使用ガイド

## Jenkins Plugin Guide

NHN Cloud Deploy Jenkinsアップロードプラグインを使用すると、Jenkinsのビルド結果物をNHN Cloud Deployサーバーにアップロードできます。

## Deploy <-> Jenkins連携方法

Jenkins build -> Deployにバイナリをアップロード(by plugin) -> 配布シナリオ実行(配布/終了/再起動/その他の事前作業および後処理)
これを行うために、次の順序で配布環境を構築する必要があります。

1. Deployにアーティファクトを作成
2. Jenkins build jobを設定
3. サーバーグループを作成
4. 配布シナリオを作成

## Jenkinsインストール

Jenkinsのインストールおよび詳細は、[https://jenkins.io/](https://jenkins.io/)で確認できます。

#### Jenkinsの最小要件

**Jenkins 1580.1**以降のバージョンが必要です。

#### プラグインインストール

1. **Jenkins管理 > プラグイン管理 > 詳細設定 > プラグインアップロード**メニューで、**tcdeploy-upload-jenkins.hpi**ファイルをアップロードします。
  ([tcdeploy-upload-jenkins.hpi](http://static.toastoven.net/prod_tcdeploy/plugins/tcdeploy-upload-jenkins-1.1.1.hpi)ダウンロードリンク)

    ![[図1]プラグインアップロード](http://static.toastoven.net/prod_tcdeploy/devguide/01.png)

2. インストールが完了したら、**インストールされたプラグインリスト**タブメニューで下記の図のようにインストールされた内容を確認できます。

    ![[図2]インストールされたプラグインリスト](http://static.toastoven.net/prod_tcdeploy/devguide/02.png)

3. 該当ビルド設定の**ビルド後の措置を追加**ボタンをクリックして、サーバーまたはクライアントタイプアプリケーションアップロードタスクを追加できます。

    ![[図3]インストールされたプラグインリスト](http://static.toastoven.net/prod_tcdeploy/devguide/03.png)

#### サーバー(server)タイプアプリケーションアップロードプラグイン設定

サーバータイプアプリケーションアップロードプラグインで、ビルド結果物をZIP形式に圧縮してNHN Cloud Cloud Deploy(TCD)サーバーにアップロードできます。

![[図4]サーバーアプリケーションアップロードプラグイン設定](http://static.toastoven.net/prod_tcdeploy/devguide/04_2.png)

* enable upload
    * プラグイン動作の有効化/無効化を決定するオプションです。プラグイン設定が保存された状態でプラグインの動作を無効化したい時に役立ちます。
* publish path
    * ビルドが完了したワークスペースで配布対象になる相対パスです。例では/を入力していますが、この場合、 JENKINS\_HOME/jobs/プロジェクト名/workspace以下の内容がアップロード対象パスになり、targetと入力するとJENKINS\_HOME/jobs/プロジェクト名/workspace/target以下の内容がアップロード対象パスになります。
* app key
    * 認証のためのapplication keyです。
* artifact id
    * アーティファクトIDです。数字のみ入力できます。
* binary group key
    * アップロードするバイナリグループのキーです。数字のみ入力できます。
* archive file name
    * 作成される .zipファイルの名前です。名前を指定しない場合、デフォルト名で自動登録されます。
        * 例)デフォルトファイル名tcdeploy-artifactid-[artifactId]-[yyyyMMddHHmmss].zip
* binary version
    * バイナリバージョンです。Jenkinsのビルド番号、ビルド開始日時などの表現式を指定できます。バージョンを指定しない場合、デフォルトバージョンで自動登録されます。
        * 表現式使用バージョン形式の例\) TCD\_SVR\_$\{BUILD\_NUMBER\}**$\{JENKINS\_PROJECT\_NAME\}**$\{BUILD\_START\_DATE\}
        * 表現式使用バージョン結果の例\) TCD\_SVR\_95\_\_Maven\_Project\_Test\_\_2015\-09\-14\_16\-46\-38
        * 注意)バージョンには禁止文字が指定されていて、スペースはアンダーバー(_)に置換され( ) - _ ~ . ,以外のASCII特殊文字は除去されます。
        * 注意)バージョンの最大文字数は、100文字です。
* description
    * アップロードに対する説明を追加します。
* include filter
    * ant-style globパターンを使用して、インクルードルールを定義できます。
        * 例) **/*.java,**/*.xml - すべての下層ディレクトリのjava、xmlファイルをインクルードします。
* exclude filter
    * ant-style globパターンを使用して、除外ルールを定義できます。
        * 例) **/.svn/** \- すべての下層ディレクトリのパス名のうち、\.svnを含むファイルおよびディレクトリを除外します。
* compression level
    * 圧縮レベルを3段階で設定できます。
        * Store：無圧縮、Normal：通常圧縮、Maximum：最大圧縮

**[サーバータイプアプリケーションアップロードビルドコンソール表示内容の例]**

```
[TCDeploy] Artifact id： [artifact id値表示部分]
[TCDeploy] App key： [application key値表示部分]
[TCDeploy] Application type： server
[TCDeploy] Description：テスト配布
[TCDeploy] Upload address： [アップロードアドレス表示部分]
[TCDeploy] Artifact version: TCD_SVR_95__Maven_Project_Test__2015-09-14_16-46-38
[TCDeploy] Deploy relative path: /
[TCDeploy] Ant include pattern: **/*.java,**/*.xml
[TCDeploy] Ant exclude pattern: **/.svn/**
[TCDeploy] Deploy relative path: file:/var/lib/jenkins/jobs/Maven%20Project%20Test/workspace/
[TCDeploy] ------------------------------- Zip Contents Start -------------------------------
	1:	pom.xml
	2:	src/main/java/a_dir/a_sub_dir/a.java
    ..........
	11:	src/main/resources/applicationContext.xml
	12:	src/main/resources/b_dir/b.xml
    ..........
	19:	src/test/resources/logback-test.xml
	20:	target/classes/applicationContext.xml
	..........
	27:	target/test-classes/logback-access.xml
	28:	target/test-classes/logback-test.xml
[TCDeploy] -------------------------------- Zip Contents End --------------------------------
[TCDeploy] Archive Compression Level: Normal
[TCDeploy] Archive temp file path: /tmp/1442216811482-0/Maven_Project_Test.zip
[TCDeploy] Archive temp file size: 33796
[TCDeploy] Archived content count: 28
[TCDeploy] ------------------------------- Upload Start -------------------------------
[TCDeploy] -------------------------------- Upload End --------------------------------
Finished: SUCCESS
```

artifact id、app keyなどのユーザー入力情報が表示され、圧縮ファイルに含まれたファイル内容を表示します。

#### クライアント(client)タイプアプリケーションアップロードプラグイン設定

クライアントタイプアプリケーションアップロードプラグインで、ビルドが成功した結果物のうち、ユーザーが指定した特定バイナリをTCDサーバーにアップロードできます。

![[図5]クライアントアプリケーションアップロードプラグイン設定](http://static.toastoven.net/prod_tcdeploy/devguide/05.png)

* enable upload
    * プラグイン動作の有効化/無効化を決定するオプションです。プラグイン設定が保存された状態を維持しながら、プラグインの動作は無効化したい時に役立ちます。
* app key
    * 認証のためのapplication keyです。
* artifact id
    * アーティファクトIDです。数字のみ入力できます。
* binary group key
    * アップロードするバイナリグループのキーです。数字のみ入力できます。
* publish path
    * ビルドが完了したワークスペースで配布対象になる相対パスです。例では/を入力していますが、この場合、 JENKINS\_HOME/jobs/プロジェクト名/workspace以下の内容がアップロード対象パスになり、targetと入力するとJENKINS\_HOME/jobs/プロジェクト名/workspace/target以下の内容がアップロード対象パスになります。
* binary kind
    * アップロードするバイナリがAndroidバイナリなのか、iOSバイナリなのか、その他(etc)のバイナリなのかを区分します。
* (ipa/apk/etc/plist) file path
    * アップロードするバイナリファイルのパスを含んだファイル名です。
        * その他(etc)バイナリは拡張子制限がありませんが、Android、iOSは拡張子制限があります。
        * iOSの時は .ipaバイナリファイルおよび .plistメタファイルを一緒にアップロードする必要があります。
        * サーバータイプアップロードタスクとは異なり、ビルドワークスペース外部のファイルパスも入力できます。
        * ワークスペースにすぐにアクセスするパス表現式です。次のように記述します。${WORKSPACE}/target/a.ipa
* binary version
    * バイナリバージョンです。サーバーアプリケーションのバージョンフォームと同じように処理されます。
* description
    * アップロードの説明を追加します。

**[ iOSクライアントタイプアプリケーションアップロードビルドコンソール表示内容の例]**

```
[TCDeploy] Artifact id： [artifact id値表示部分]
[TCDeploy] App key： [application key値表示部分]
[TCDeploy] Application type： client
[TCDeploy] Description：クライアントプログラムテストアップロード
[TCDeploy] Upload address： [アップロードアドレス表示部分]
[TCDeploy] Binary type： iOS
[TCDeploy] Binary version： IOS_59__build_iOS_Project_Test__2015-09-16_11-13-12
[TCDeploy] iOS binary file path： /tcdUploadTest/a.ipa
[TCDeploy] iOS meta file path： /tcdUploadTest/a.plist
[TCDeploy] ------------------------------- Upload Start -------------------------------
[TCDeploy] -------------------------------- Upload End --------------------------------
Finished: SUCCESS
```

artifact id、app keyなど、ユーザー入力情報や、アップロードするバイナリおよびメタファイルが表示されます。

## Jenkins-CLI Build Profile

ユーザーがJenkins-CLIを使用してビルドコマンド実行に役立つ、事前に定義されたProfileです。

#### 準備事項

1. SSH Pubilc/Private Key Pairの作成
    * ssh-keygenまたはPuTTY Key GeneratorなどでSSH Pubilc/Private Key Pairを作成し、Jenkinsのビルド実行アカウントにSSH Public Keyを保存、 SSH Private Keyファイルはタスク実行サーバーに保存。
        * 暗号文(passphrase)が設定されていないSSH Private Keyのみサポートします。
    * 例) http://[JENKINS_URL]/user/[ユーザー名]/configureページにアクセスし、SSH Public Keys項目に作成したSSH Public Keyの内容を保存して、作成したSSH Private Keyファイルは、タスク実行サーバーのユーザー指定パスに保存。

2. http keep alive timeoutの調整
    * Jenkinsサーバーのhttp keep alive timeout値を確認後、値を調整。
    * 例) JenkinsをRPMでインストールした場合、
        * /etc/sysconfig/jenkinsのJENKINS_ARGSにhttpKeepAliveTimeout=[適当なミリ秒値]オプション追加。

3. Stream例外発生関連
    * ビルドコンソール表示中にjava.io.StreamCorruptedExceptionが発生した場合、Jenkinsを実行するJVMオプションに-Dhudson.diyChunking=falseオプションを追加。
    * 例) JenkinsをRPMでインストールした場合。
        * /etc/sysconfig/jenkinsのJENKINS\_JAVA\_OPTIONSに -Dhudson.diyChunking=falseオプション追加。

#### Profile設定

![[図6] Jenkins-CLI Build Profile設定](http://static.toastoven.net/prod_tcdeploy/devguide/06.png)

**[ユーザー入力内容]**

* OS選択
* RunAs入力
    * 実行アカウントを入力します。
* JenkinsServer
    * ドメインまたはIPを入力します。
* Server Charset
    * Jenkins-CLIを実行するサーバーの文字コードを入力します。
   何も値を入力しない場合はデフォルト値のUTF-8
* Jenkins Url
    * ビルドコマンドを実行するJenkinsサーバーアドレスを入力します。
* Jenkins Charset
    * Jenkinsサーバーの文字コードを入力します。何も値を入力しない場合は、デフォルト値のUTF-8です。
* Java Home
    * Jenkins-CLIを実行するためのJDKまたはJREのHomeパスを入力します。
* Job Name
    * 実行するビルドJobの名前を入力します。
* Private Key Path
    * SSH Public Keyが設定されているJenkinsユーザーで認証するためのSSH Private Keyファイルの保存パスを入力します。
* User(ver. 2.46以上のバージョン必須)
	* Public Keyが設定されているIDを入力します。
		* 参考事項
			* 2.46以前のバージョン
				* Public Keyが設定されている場合、Jenkinsで自動認証を進行
			* 2.46以上のバージョン
				* User(Jenkins admin ID)が追加され、該当IDにPublic Keyが設定されている場合のみ認証通過
* Build Parameter(任意)
    * ビルドに必要なパラメータを入力します。
