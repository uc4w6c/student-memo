Dapr入門
谷本さん

Everforth/Acroquest Technology
アーキテクト/トラブルシューター/SIer


## 分散システム開発にはSpringが思考である
SringCloud マイクロサービス間連携に適切

サーディスディスカバリ、ロードバランシングなどなど

## 少し困ったことがありました

JavaをバージョンアップしようとするとSpringのバージョンアップも必要

## 個別にバージョンアップすれば良い
SpringCloudは「繋ぐ役目」なので複数バージョンの混在によって問題が起きることがある

問題を避けるためには、Java, SpringBoot, SpringCloudはまとめてバージョンアップした方が良い
-> だいたいアプリにも修正が必要になる


## Springの過去の欠点
- バージョンアップの作業が大変
- 異なる言語やフレームワークからの


## 本当に欲しかったもの
- 簡単にWebAPIを作る
- それをなにかのプラットフォームで走らせる
- とサービスディスカバリー、分散トレーシングなどができる


## Daprとは
Microsoftが開発中のOSS

アプリケーションとクラウドや任意のインフラの間に挟まるミドルウェア

各ミドルウェアやマネージドサービスに対するプロキシ、とも言える

- サービス呼び出し
- ステート管理
など

## Daprはサイドカーサービス
自分の作ったアプリケーションプロセスと1:1で起動する形

## Daprが良い理由
- SpringBoot + SpringCloudに比べてアプリケーションとアプリケーションインフラを疎結合にできる
 - 別プロセスに分離できる = バージョンアップも別タイミングでできる
- ほかのサイドカーサービスに比べて、アプリケーションよりのデザイン
  Envoyとかと比較して


## 調べてみました
## DaprとSpringの機能

Daprが提供する機能と、Springファミリーが提供する機能

サービス呼び出し: 


## コードで比べる

### ハローワールド

Dapr Port 9090, Spring Port 8090

dapr run --app-id dapr-example --app-port 8090 --port 9090 -- java -jar dapr-example-1.0.0.jar
で起動する

curl localhost:9090/v1.0/invoke/dapr-example/method/hello
(invokeの部分が色々ある)

## サービス呼び出し

/remote-hello: Port8081 ->  /hello: Port8080 (中身はAPサーバ2台構成)
 -> という呼び出しを行う例

### Eurekaの例

Spring Cloud Service Discovory (Eureka)を使用する

spring-remote -> Eureka <- spring-example

web fluxのwebClientを使う(最近はこちらが推奨されている)
webClient.get()           // HTTP GETメソッド指定
  .uri("http://~")        // URL
  .retrieve()  // 結果の取得
  .bodyToMono(Map.class)  // レスポンスをMono<Map>で取得
  .block();               // bvlockしてMapとして取得

### Daprの例
Dapr : Port 9091 - dapr-remote: Port 8091
->
Dapr: Port 9090 - dapr-example: Port 8090


@Value("http://localhost:${dapr.http.port}/v1.0")
private String baseUrl;

@GetMapping(~)
public ~
retrun webClient.get()
         // 以下が接続先のDaprURLを指定する
         .uri(baseUrl + "/invoke/dapr-example/method/hello")
         .retrieve()
         .bodyToMono(Map.class)
         .block();

Dapr SDK for Javaもあるよ


## メッセージング
- キューを利用して非同期メッセージングを行う

/publish spring-remote -> RabbitMQ -> /subscribe spring-example

Spring Cloud Streamを利用する

エンキュー側の側のコード

streamBridge.send("publish-out-0", message);
(StreamBridgeクラスを用いるのが最新)

"publish-out-0"の指定は以下
application.properties
  spring.cloud.stream.bindings.publish-out0.destrination=exchange-1


でキュー側のコード
@Bean
public Consumer<Map<String, ?>> subscribe() {
  return (map) -> {
    System.out.println("Spring's subscribe is cool");
    System.out.println(map);
  }

binding.subscribe-in-0.destination=exchange-1
binding.subscribe-in-0.group=queue-1


Daprの場合

Dapr - dapr


エンキュー側のコード

API呼び出しとほぼ同じ
"publish/rabbitmq/exchange-2
(/publish/(コンポーネント名)/()


components/rabbitmq.yaml
に各種接続先を設定する

spec:
  type: pubsub.rabbitmq


デキュー側のコード
@PortMapping("/subscribe")
public void subscribe(@RequestBody Map


## 分散トレーシング

@RequestHeader("traceparent") String traceId
/invoke/da

headerにtraceIdを付与してあげる

ZIPKIN
にて確認していた

複数サーバでの情報が可視化される

SpringCloudTraceと同じことができる！


## Daprの今後
1.0リリースに向けて動き始めた

先月1.0がリリースされた
毎月リリースするらしい

 - Daprを使えばサービス呼び出しやメッセージングをすべてHTTP/gRPCに統一できる






