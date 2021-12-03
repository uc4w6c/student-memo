# Spring Web MVCのマイクロサービス化の経緯と今
Line福岡  平井さん

https://speakerdeck.com/line_developers/spring-web-mvc-microservices-then-and-now


## Problems
 - Server
   - 年賀イベントやメッセージ自動配信によるリクエストスパイク
   - 複雑なマイクロサービス上でのボトルネック、エラー調査
 - Client
   - カスケード障害
     - 一つの障害がシステム全体に影響を及ぼす
 - Development
   - 複数同時並行で開発、テスト、リリースする仕組みが必要


## Solution
### Server
 - 年賀イベントやメッセージ自動配信によるリクエストスパイク
    - 耐えられるアーキテクチャの採用
 - 複雑な



#### Armeria
 - LINEのオープンソースソフトウェア
 - Java, Netty ベースのフレームワーク
   - Non-Blocking I/O
   - HTTP/2
   - RPC(Thrift, gRPC), RESTサーバ/クライアント
   - SpringBoot, Spring MVCとの統合
   - Micrometerによるメトリクス収集
   - Zipkin


#### CircuitBreaker

https://armeria.dev/docs/client-circuit-breaker


#### Armeria - Client
 - Armeria clientの機能
   - RPC(Thrift, gRPC), REST APIクライアント
   - Retrofit との統合
   - クライアントサイドロードバランシング
     - DNSサービスディスカバリ

##### Client-side load balancing
通常のLBはサーバサイドLB

 - リクエストするクライアント側で接続先サーバを決定
   - 接続先サーバリストの保持が必要
   - 動的にサーバリストを更新するため、サービスレジストリを使用
 - Pros
   - サーバサイドロードバランシングと比べ、単一障害点が少ない
   - マイクロサービスがシンプルになる

Central Dogma <- (Sync) DeploymentSystem

 - サーバはデフォルト、ラウンドロビンでアクセス
 - Armeria のバージョンアップで、Ramping upの設定が追加(起動直後のリクエスト割合を少なくする)

 - Ramping up strategy
   - サービスイン直後のサーバは、JITの問題で遅い。少ないリクエストにすることでバランスをよくする


## Development

### Feature toggle
設定ファイル
 - Cons
    - 環境ごとに切り替えられるが、デプロイが必要
    - より細かい単位、特定ユーザーや国単位で切り替えたい

Service registroyにフィーチャートグルを格納
 - Service registoryには、Central Dogmaを使用

Central Dogma: https://line.github.io/centraldogma/
 - LINEのオープンソースソフトウェア

DSLとして、PlanOutを使用
 - FacebookがA/Bテスト用に開発したフレームワーク
 - Javaの実装であるPlanOut4jをライブラリとして使用
   -> 内部でフォークして利用している








