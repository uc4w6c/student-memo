# Cloud Native Buildpacksで作る至高のコンテナイメージ for Spring Boot~ おそらくあなたの知らないBuildpack活用法 ~
VMware まき

3種類の代表的なJavaコンテナイメージ作成法
 - DockerFile
 - Cloud Native Buildpacks
    ソースコードからイメージ作成
    Spring Boot Maven/Gradleプラグインですぐ使える
    オススメ設定込(Opinionated)
 - Jib
    ソースコードからイメージ作成
    Docker Daemon不要
    高速


Production Readyなコンテナイメージを簡単に作りたい

適切なベースイメージ管理
効率的なレイヤー化
適切なJVMメモリ設定
 - コンテナメモリ!=ヒープ
ユーザ設定(rootはだめ)
などなど

CloudNativeBuildpacks

Platform APIでインターフェースが決まっている

SpringBootGradlePluginはPlatformAPIを呼び出す

Builder/Buildpack/Stack

Stack - ベースのOSイメージ(build image + run image)
Buildpack - バイナリ・スクリプトや設定ファイルを提供する

代表的なBuilder
 - Paketo Buildpacks
    Cloud Foundry Foundationで開発されているBuilder
    開発が活発で機能豊富
 - Heroku
 - Google Cloud


## Paketo Buildpack(Java)の機能

### Memory Calculator
コンテナのメモリサイズを指定すると、アプリのサイズに応じて自動的に必要なヒープ・ノンヒープのサイズを計算し、JVMオプションに設定してくれる

Heap
Non Heap
  - MetaSpace
  - ReservedCodeCache
  - DirectMemory
  - ThreadStack


### OOME時自動kill
 -XX:+ExitOnOutOfMemoryErrorオプションが自動で追加される。JVMのOut of Memory Errorが発生すると自動でコンテナがkillされる。(OOMEが発生して動作が不定になることを予防)

JVMがOutOfMemoryError発生してもコンテナがメモリ不足になるわけではなく、またヘルスチェックなどの軽量なものも動いてしまうことがあるためこのオプションは必須。

### Service Binding
外部サービスのインスタンスをBind可能なリソースとして扱い、このリソースをアプリケーションにBindすることで接続情報を注入する仕組み

### Kpackでまとめてイメージ管理
Kubernetesでまとめてイメージを管理できる

kindを使えばKubernatesを利用しなくてもkpackをインストールしてビルドサーバとして使える







