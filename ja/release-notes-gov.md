## Dev Tools > Deploy > Release Notes

### 2024. 11. 26.
#### 機能改善
* 過去5年間の配布履歴のみ照会できるように変更

### 2024. 09. 10.
#### 機能改善
* Jenkins Pluginのバージョンが更新されました(バージョン1.1.3)。
    * バイナリグループキーが空の値の場合、Defaultバイナリグループにアップロードされるように修正
    * エンドポイントのデフォルト値がhttps://api-tcd.cloud.toast.comからhttps://api-tcd.nhncloudservice.comに変更

### 2024. 07. 09.
#### 機能改善
* **バイナリグループ**タブ内ソート機能を追加
* クライアントバイナリ配布UI改善
    * 通知受信グループにもダウンロードパスを送信可能
* **配布履歴**タブで**実行者**をクリックして名前を確認できる機能を追加

### 2024. 03. 26.
#### 機能改善
* NHN Cloud Instanceを配布する際、SSHによる配布のほか、NHN Cloud Agentによる配布機能を追加
    * InstanceにFloating IPを割り当てなくても配布可能

### 2024. 02. 27.
#### 機能改善
* サーバーグループ修正UIの改善
* 配布実行APIを追加

### 2024. 01. 23.
#### 機能改善
* バイナリグループの作成および修正時に自動削除ポリシーを設定するように変更

### 2023. 11. 28.
#### 機能改善
* Jenkins Pluginのバージョンが更新されました(バージョン1.1.2)。
  * Windows環境にインストールされたJenkinsでDeployにアップロードされないエラーを修正しました。
* API でバイナリをアップロードする際の容量制限を1GBから2GBに変更
#### バグ修正
* リソースファイルの時間情報が表示されないエラーを修正

### 2023. 06. 27.
#### バグ修正
* Windows Server 2019 Standardでバイナリが配布されないエラーを修正

### 2023. 05. 30.
#### 機能改善
* Jenkins Pluginのバージョンが更新されました(バージョン1.1.1)。
    * Jenkins AgentノードでDeployにアップロードできないエラーを修正

### 2023. 04. 25. 
#### バグ修正
* 配布シナリオ修正時にファイル選択ウィンドウでファイルリストが10個以上ロードされないバグを修正
* コンソールでリソースファイルのアップロードに失敗した時、ファイルの照会ができない問題を修正し、**アップロードキャンセル**ボタン追加

### 2022. 12. 27.
#### 機能改善
* **配布履歴**タブでダウンロードするコンテンツ(Excelファイル)のRow数を表示

### 2022. 10. 25.
#### 機能改善
* Deploy権限の細分化機能を追加
* Deploy ADMIN権限ユーザーにのみ配布実行通知を提供するように修正
* シナリオおよびTask名の文字数制限を30文字から50文字に変更
* 実行可能確認機能の改善
* Taskがないシナリオは実行できないように修正
#### バグ修正
* Taskおよびアーティファクト作成内のガイドリンクエラーを修正
* Deployが終了せず、Deploying状態が持続するエラーを修正

### 2022. 08. 23.
* APIエンドポイントのドメインがgov-api-tcd.cloud.toast.comからapi-tcd.gov-nhncloudservice.comに変更されました。

### 2022. 07. 26.
#### 機能改善
* バイナリアップロードが失敗したときの案内メッセージを追加
#### バグ修正
* 削除されたバイナリグループにアップロードできないように修正

### 2022. 04. 26.
#### 機能改善
* アーティファクトリスト検索時の使用クエリを改善
* Jenkins API BuildでJenkins Pipeline Jobを実行時、実行完了を待つように改善
* シナリオアップロード時のファイル名制限を削除

### 2022.01.25
#### 商品発売
* 配布とインストール利便性を提供するためのサービスです。
* ワンクリックで簡単かつ迅速にソフトウェアを配布することができます。
