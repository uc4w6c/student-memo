# LINEの予約プラットフォームにおけるSpring WebFluxとKotlin Coroutineの活用事例
小山 淳平(LINE株式会社)


なぜ非同期アーキテクチャを採用したのか
 - リソース効率が良い
   - C10K問題
   - 将来的な負荷の増加に対して安心感
 - WebFluxについてチーム内の他プロダクトでの利用経験があった
 - 実装難易度が高い

  -> Coroutineを使うと同期的なスタイルで実装できる
   - Kotlin 1.3でCoroutineがStableに
     - 各種非同期ライブラリへの互換機能があった

## gRPCの採用
Protocol BuffersのIDL(インターフェース記述言語)を使って楽をするのがメイン
 - gRPC Stub(Client)が生成されるので個々のClient実装が不要
    - 社内の関連システムには生成されたStub classをjarで配布(社内maven repo)
    - プラットフォーム内のアプリケーションはmono repoなのでgradle moduleとしてbuild時に参照
 - プラグインによるカスタマイズ
    - IDLからのドキュメント生成

gRPCとSpringのIntegration
 - ArmeriaのgRPC及びSpring supportを利用


## Tips

### Tips1
WebFlux -> Coroutineへのリクエストコンテキスト伝搬及びMDCへの反映

 - ここではコンテキスト = APIリクエストの情報などの処理のコンテキストを指す
 - コンテキスト伝搬は処理(1リクエストなど)のObservabilityをあげるために必要

### Tips2 分散トレーシング

 - 分散システムにおけるObservability向上のため
 - Zipkinを利用
 - TraceContextの伝搬はリクエストコンテキストと同様に必要
 - brave-context-slf4jのMDCScopeDecoratorでTraceIdなどをMDCにput
  -> ログを社内のログ基盤に流す
  -> TraceIdを使ってコンポーネント横断でリクエストをトレーシング


### Tips3: Blockingなコードは書かない
JDBCはブロッキング・・・どうする
 - R2DBCは未採用
    - プロジェクト当初はmysqlのdriverもなく時期尚早だった
    - mybatis(R2DBC未対応)の地検が多かった
 - なのでJDBC接続している
    - DBにクエリする処理を呼ぶ部分は実行スレッドを変えている(Dispatchers.IO)

ブロッキングなコードの絵ｋンチ
BlockHoundを利用
 - ノンブロッキングな処理におけるブロッキングな処理のランタイムでの検知とフックを提供してくれるライブラリ
 - active profileで開発環境のみ稼働するように制御

### Tips4: spring-tx: TransactionManagerとCoroutine
- トランザクション境界を明示的に限定したいのでTransactionTemplateを使っている
  - TransactionTemplateはPlatformTransactionManagerに依存

### Tips5: WebFluxでのCoroutineの実行スレッド
- デフォルトではDispatchers.Unconfinedが指定されている。


### Tips6: その他Coroutineの利用例
async with callbackなmethodをsuspend functionに書き換える(suspendCoroutine)











