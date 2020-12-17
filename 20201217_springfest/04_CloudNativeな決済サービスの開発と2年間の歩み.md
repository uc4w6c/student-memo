# CloudNativeな決済サービスの開発と2年間の歩み
SBペイメントサービス
鈴木 順也(@suzukij)

## 
Tanzu Application Serviceo

RabbitMQ + SpringCloudStream
で非同期連携を実現


CircuitBreaker (Hystrix -> Resilience4J)
Hystrix は maintenance mode

Resilience3j
 - CircuitBreaker
 - 


CircuitBreaker
  2分間に100%のエラーが発生したら停止するように指定

Bulkhead(並列処理数の制限)
 同時接続数が設定値を超過したらリジェクト

  280まで許可 waitは0秒


Http Client(RestTemplate -> WebClient)

 - レスポンスのサイズが256kbを超えるとエラー。設定値で変えられる
 - ファイルディスクリプタのリークが発生
     SpringBootのv2.2.9/v2.3.2のアップデートで解消

CI/CDパイプライン
 Concourseを利用

ビルド時にいくつのJavaバージョンでテストしている。
 -> いつでもアップデートできるようにらしい。すごい

jMeater
 - Load test
   - Webスレッドプールが枯渇
   - コネクションプール問題など

 - Soak Test
   ピークの半分程度のリクエストを1週間続ける

   - 監視設定がいけてなかったりなどが検知できた

Load test and E2E test by JMeter
テストを流し続けて
スクリーンショットをスラックで流す


## Tracing
Zipkin

SpringCloudSleuth
によってアプリログに出力されるとレースIDで検索可能

Zipkinの
データストアにElasticsearchでKibana

## サービス目線のモニタリング




