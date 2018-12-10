# blog-springboot_on_GKE
Kubernetes settings and procedure to deploy 

## 環境について
以下のリポジトリで作ったSpringbootのアプリをデプロイする。
https://github.com/Taurin190/blog-springboot


アプリは、CouldSQLをdatabaseとして使用する。

## 環境構築手順
以下の手順が必要でした。
- クラスタの作成
- シークレットの作成
- デプロイメントを作成
- サービスの作成

### クラスタの作成
- クラスタ作成時にCloudSQLへのアクセスを許可する必要がある。
- ノードプールの「高度な編集」より、CloudSQLへのアクセスを許可に設定する
- クラスタはゾーンもしくはリージョンで選択できる。リージョンで選択すると1ゾーンあたり1ノード必要になるので注意
- ポッド数を制限したい場合はこの時点で制限しておく。
### シークレットの作成
このURLを参考に
https://cloud.google.com/sql/docs/mysql/connect-kubernetes-engine?hl=ja

- gcloud beta container clusters get-credentials taurin190 --region asia-northeast1-a --project taurin190com
- kubectl create secret generic cloudsql-instance-credentials --from-file=credentials.json=${JSON_NAME}
- kubectl create secret generic cloudsql-db-credentials --from-literal=username=${USERNAME} --from-literal=password=${PASSWORD}
### デプロイメントの作成
- コンテナを選んで作るだけ
- 環境変数をここで作成すると、configmapにしてくれる
- 作成したシークレットは後で変更する
### サービス作成
- ロードバランサでサービスを作る場合には特に問題はなく
- 80コンテナで開けているポートをコンテナのポートにマッピングするだけ