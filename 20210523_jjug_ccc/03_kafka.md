# Red HatのKafkaとサーバーレスコンピューティングでバッチ処理をニアリアルタイム化しよう
Red Hat 暮林さん


## Quarkus

## Knative

 - Knative Serving
    サーバレス環境
    軽量なコンテナアプリをサーバレス動作
    ゼロスケール
    リクエスト数とコネクション数などに応じてスケールアウト・スケールイン

 - Knative Eventing
    CloudEventsの仕様に則って

## イベントフロー
イベントソース -> チャネル -> サービス

チェネル: 永続化
チャネル->サービス間 : 配信先の設定でエラー時のリトライ処理などが設定可能

## Kafka

### ログベースメッセージブローカーの特徴

トラディショナルなメッセージブローカー
 - メッセージのようなもの
 - 

ログメッセージブローカー(kafka)
 - テーブルブローカーのようなもの
 - Consumerがメッセージを取得しても、BrokerにMessageが残る
 - Consumer側で読み出し位置を管理
 - 分散トランザクションに


メリット
 - リソースが空いていればすぐ処理される。
 - リソースが空いていない場合は後回しで処理される
 - サービス間の緩衝材になれる

いままで夜間バッチでやっていた部分をニアリアルで実行可能になる




