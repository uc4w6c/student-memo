# Spring ObservabilityとこれからのMicrometer
VMware Tommy Ludwig

## オブザーバビリティはなんでやる？
 1. ビジネスのため(お金)


本番の真実
 - 障害や性能低下は絶対に起きる
   100%防ぐのは無理 + 無駄
 - 仕様書通りに出来上がらないことがある
   テストは通ったのに・・・
 - 過去の現実は今の現実と限らない
   何も変えていないはずなのに・・・
   インフラ・データ量・ユーザー行動によったり

オブザーバビリティの目的
 - 問題を早く検知、そして解消する
   売り上げへのダメージを最小化
 - 根本原因の調査を楽にする
   従来のやり方では辛い
 - リスク低下 = 安全に継続的デリバリー
   ダッシュボードやログを見張るのやめましょ
 - コンプライアンス
   本当に何がどこで動いていてどこと通信している？


オブザーバビリティはいつ、どこで、誰がやる？
開発ライフサイクルであらゆるところで常に全員がやるいい

要件定義の段階から (最初から)
仕様書にSLAを明記する (具体的に)  -> このAPIは99%が1秒以内に返すことみたいな
アラートと手順書にも (もれなく)


オブザーバビリティを実現する手段
Logs, metrics, tracingの概念

 - Logs
   なじみの手段
   エラーやリクエスト、任意のメッセージ
   並列処理がたくさんあると見づらい

   リクエストが増えるにつれてログ(コスト)が増える

 - Metrics
   数値を集計する
   アラートやダッシュボードに適切
   メモリ・CPU使用率やHTTPリクエストのレイテンシとスループット
   過去のトレンドを把握・比較

   リクエストが増えるにつれてメトリクスの量は増えない

 - Tracing
   各サービスでかかった時間とサービスの呼び出し関係を可視化する
   TraceIDが伝搬されてリクエストの追跡ができる
   ログにTraceIDを入れて連携可能

   リクエストが触れるにつれてトレースが増える
   対策としてサンプリング(一部のトレースだけ保存)


## オブザーバビリティの目的
 - 問題を早く検知、そして解消する
    メトリクスのアラートで検知
    影響範囲を把握

 - 根本原因の調査を楽にする
    トレースで原因を特定
    必要あればログ

 - リスク低下 = 安全に継続的デリバリー
    Canary release
    メトリクス比較で自動ロールバック

 - コンプライアンス
    トレースで呼び出し関係を知る
    メトリクスで稼働中のアプリと使用状況を確認


## オブザーバビリティを実現する手段
Springユーザが簡易に実現するためのプロジェクト

 - Micrometer(metrics)
   Spring Boot 2.0から採用される
   メトリクスを抽象化したAPI
   SLF4Jのメトリクス版
   メトリクスの送り先によってMeterRegistryの実装を使い分ける
   多くのライブラリのメトリクスが用意されている
   Timer, Counter, Gaugeなどのmeter種類ある

 - Spring Boot(auto-config)
   Dependencyを追加
   Configuration Propertiesを設定

 - Spring Cloud Sleuth(tracing)
   Dependencyを追加
   Configuration Propertiesを設定
   Springプロジェクト以外でトレーシングしたい箇所があればSleuthのAPIを利用

https://qiita.com/hmachi/items/069d8865eb142a561dbc


## SpringOneで発表された情報

Spring6ではSpring Observabilityというテーマがある

metrics/tracingを一つのAPIですべてのSpringプロジェクトに

フレームワークにObservabilityのために必要な抽象化。

https://youtu.be/QMCYmaPa


## なぜSpring Observability?

今まではSleuthとMicrometerがあって、なぜ新しくSpring Observabilityを？
 - Spring Cloud SleuthはSpring Cloudのプロジェクト
 - Spring CloudじゃないSpring プロジェクトがそれを使えない
 - MicrometerとSleuthは似たコードが各プロジェクトに対して重複する
 - ずれと漏れがある
 - フレームワークのWebMvcやWebFluxのobservabilityコードがSpringBootにある

## 今のSpring Observabilityの予定
メトリクスとトレーシングが重なるのは処理時間を図るところ
MicrometerのTimerを利用すれば一石二鳥？

従来の使い方とほとんど同じ
ただし、事情によってScopeという新しいAPIも使う必要がある


## Micrometer2

2022/11がGAの予定

Spring Observability
 - TimerHandler/Sampleでトレーシングなどをゲット
 - sleuth -> micrometer-tracing

Java baseline
 8 or 11 or 17

LongTaskTimerをTimerに取り入れる
 - よく誤解されるMeter
 - ネーミングをミスってしまった

アイディア募集中！


## Micrometer1.9
2022/5 GA

Exemplars
 - メトリクスとトレーシングを更に連携できる
 - ヒストグラムにトレーシング情報を追加
 - グラフで可視化した際時にトレーシングにスムーズに移行できる

