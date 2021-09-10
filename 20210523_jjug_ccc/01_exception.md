# 今どき？のJavaにおける例外処理についての考察

## Java例外処理の設計思考
 - 例外は予期できないものに利用する。
    在庫なし、ファイル終点到達などでは例外は使わない
 - チェック例外を活用して

参考: プログラミングJava第三版

## 標準設計思想の問題点
 - throws句すらノイズ
    - どう処理すればいいか悩む
    - DBアクセスエラーとか、SQL発行のたびに別々に例外処理をやる必要ある？
     -> 結果握りつぶすコードが・・・

 - try catchは可動性を低下させる。

 - レイヤードアーキテクチャで上位レイヤーに下位レイヤーの関心ごとが漏れる
     SQL RuntimeExceptionなど

## フレームワークによる解決と現代の原則
 - SpringFrameworkではインフラ層からスローするのは全てRuntimeException
 - トランザクションロールバックなどの例外処理はAOPやフィルター/インターセプターで実施する
   -> 例外型チェックの厳密性をすててAOPによる柔軟性で対処する

### メリット
 - ドメイン層やアプリケーション層には例外処理が消え、本質的な関心ごとに集中できる
 - ルールにしたがって、共通的に例外処理を適用できる
 - 後から柔軟に変更できる

## しかし一部の処理では例外を個別ロジックで扱いたい
 - ロックエラーや重複キーエラーなどのDB制約エラーを個別に扱いたい
 - 詳細な原因情報を取得し、ユーザーに表示したい
 - エラー発生時の特殊な処理を行いたい(取り消し処理など)

 -> Javaの標準的な考え方ならチェック例外を利用

Springでもロールバックするにはロールバック対象を厳密に指定する必要がある。
 @Transactional


## 
 - try catchは中心となる処理と周辺的な例外処理を切り分けて使うもの
 - ビジネスロジックでエラー状態を扱うとき、それはユーザーにとって利便性を提供するなど、周辺的な関心ごとではなく、ビジネス成功に直結する中心的な関心ごとであるはず
 - アプリケーション層/インフラ層で扱わなければならないエラー状態は例外をスローするのではなく、戻り値として返却し、

## Result型みたいなものを使うのはよくない

Result<T>
みたいなやつは使う型がめんどくさい(Exceptionと同じやん)

## ほかの言語は?

Golang

file, err = os.Create(filename)
if err == nil {
    return
}

関数型言語

 - 静的型付けの関数型の思想を持った言語ではエラーの可能性を持つ型を用意し、パターンマッチングで例外処理を記述することができるものが多い

Elm

--- 3通りの列挙型で表現データ構造もそれぞれ異なる
type Model
  = Failuer String
  | Loading
  | Success String

showResult : Model -> String
showResult model = 
    case model of 
        Loading -> "Now Loading"
        Failuer 


## JavaでElmのような書き方を書く場合

public interface Result {
  class Success implements Try {}
  class Failuer implements Try {}
}

if (result instanceof Result.Success) {
    Success s = (Result.Success)result;
} else {
    Filauer f = (Result.Failuer)result;
    f.getMessage();
}

## Javaの構文の発展の方向
 - 関数型プログラミングへ

 - パターンマッチングによるエラー処理に向けて
  - switch
  - record
  - sealed class
  - pattern matching


## Java16では？

public interface Result {
    record Success(String msg) implements Result{}
    record Failure(String msg) implements Result{}
}

public class Controller {
    @Get
    public String getMessage() {
        Result






