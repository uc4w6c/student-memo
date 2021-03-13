# Kotlin + Kotlessで始めるServerless アプリケーション開発
@bulbulpaul
AWS Japan
https://speakerdeck.com/bulbulpaul/kotlessdehazimeruserverlessapurikesiyonkai-fa


Kotless
JetBrainsが開発している OSS

## Kotlessが実現すること
・KotlinでのサーバレスAPIの開発をシンプルにする
 - APIフレームワークのように実現する
 - API Gateway + Lambdaのデプロイ

・Gradleコマンド(Plugin)でデプロイやローカル実行が可能
・デプロイは内部でTerraformを実行している(拡張も可能)
・AWS環境のみに対応。今後他のクラウドも対応予定


## APIコード例
```
@Get("/welcome")
fun wellcomeMessage(): String {
  return "Wellcome to nakanoshima.dev!!"
}
```


