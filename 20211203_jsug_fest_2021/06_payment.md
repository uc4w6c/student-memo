# 決済システムで学ぶレジリエントなサービスのいろは
SBペイメントサービス株式会社
高野 はいね


## タイムアウト設定



## リトライ

restTemplateでタイムアウトをキャッチ
Spring retryでリトライを行う。

@EnableRetryを付与することでRetryできる

@Retryableで具体的な設定
新しいExceptionを作成してRetry可能なExceptionを判断

## 冪等

冪等ができるシステムを作るために必要な要素

 1. 冪等キーを要求する
 2. 冪等キーとAppへのリクエスト内容の組み合わせを保管する
 3. 冪等キーとAppからのレスポンス内容の組み合わせを保管する


## 分散トレーシングで処理の流れを追う

Spring Cloud Sleuthを追加するとMDCでTraceIdを作成する

Zipkin
spring cloud sleuth zipkinを追加するとトレースデータをZipkinサーバに送信してくれる



## 後回しで良いものは非同期にする

RabbitMQ
StreamBridgeを使うことでProducerを実装可能

## 更新頻度が低いデータはキャッシュで持つ

spring cache(spring-boot-starter-cache)
使いづらい時があるから
caffeine というライブラリを使うこともある


## サーキットブレーカー

Resilience4j
サーキットブレーカーだけでなく


サーキットブレーカーは3つの状態を遷移する
 - CLOSE: 正常に通信できている状態
 - OPEN: 通信に障害があり通信できない状態。
 - HALF OPEN: OPEN状態が一定期間たつとこの状態になる。通信を再度試してみて問題あればOPEN, 問題なければ(障害回復すれば) CLOSE状態となる


## バルクヘッドで対向システムを守る

ECサイト 1TPS -> 決済システム -> 1TPS 決済機関
となる。

これがスパイクなどがあると100TPS をそのまま決済機関に流してしまう。

対応方法としてはResilience4jで利用可能
application.yml
resilience4j:
  buildhead: 
の設定で可能





