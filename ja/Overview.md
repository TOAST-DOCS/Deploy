<!-- pre-align:aligned sig=ecae7f837f9d -->

<a id="dev-tools-deploy-overview"></a>
## Dev Tools > Deploy > 概要 { #dev-tools-deploy-overview }

Deployサービスを使用すると、'1クリック'で素早くアプリケーションをデプロイできます。

* バイナリファイル
* マルチメディアファイル
* スクリプトファイル
* 実行ファイル
* その他ユーザーが定義したさまざまなファイル 

![[図1]サービス画像](https://static.toastoven.net/prod_tcdeploy/ja/01_ja.png)

<a id="benefits"></a>
## 利点 { #benefits }

* 簡単で便利なバイナリ管理
* クライアントへのバイナリデプロイ通知
    * メール
    * 文字メッセージ  
* デプロイバージョン別リアルタイムモニタリング
* デプロイ結果メール通知
* デプロイ前後のシナリオ設定
* ウェブコンソールサポート

![[図2]サービス画像](https://static.toastoven.net/prod_tcdeploy/ja/02_ja.png)

<a id="features"></a>
## 提供機能 { #features }

<a id="upload"></a>
### アップロード { #upload }
さまざまなアップロード方式を使用して簡単にファイルをアップロードできます。
* アップロードインターフェイス
    * ウェブコンソール
    * Jenkinsプラグイン
    * REST API

<a id="download"></a>
### ダウンロード { #download }
ダウンロードリンク通知機能を使用して、簡単にアプリのインストール/アップデートができます。

<a id="query-binaries"></a>
### バイナリ照会 { #query-binaries }
過去のバイナリデータを維持して、迅速にロールバックできます。

<a id="manage-deployment-projects"></a>
### デプロイプロジェクト管理 { #manage-deployment-projects }
プロジェクトのインフラ情報を連動して、簡単に設定できます。

<a id="manage-execute-deployment-scenario"></a>
### デプロイシナリオ(実行)管理 { #manage-execute-deployment-scenario }
ユーザー定義コマンドを使用して、デプロイ後のサーバー状態を確認できます。

<a id="manage-deployment-history"></a>
### デプロイヒストリー管理 { #manage-deployment-history }
デプロイヒストリーでデプロイ内容および以前のバイナリを管理するので、安定性を確保できます。

<a id="glossary"></a>
## 用語説明 { #glossary }

| 用語 | 説明 |
| --- | --- |
| アーティファクト | デプロイを管理するDeploy構成の基本単位 |
| サーバーグループ | シナリオ実行対象サーバーグループ |
| シナリオ| サーバーに実行するタスクの集合 |
| タスク | デプロイプロセスの実行単位 |
| バイナリ | ユーザーがアップロードしたデプロイ対象ファイル |
| リソース | ウェブコンソールで作成、修正、履歴管理が可能なデプロイ対象ファイル |
