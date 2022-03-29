koichi sakata

### JEP416: Reimplement Core Reflection with Method Handles
リフレクション機能のクラス(Method, Constructor, Field)をメソッドハンドルを使って再実装する

再実装する理由
 1. Valhallaで言語機能の追加(value class や primitive class)を予定
 2. 従来のコードでは内部的に3つのコードパスがある
 3. Valhallaへの対応で3つすべてを更新するのは高コストとなる
 4. 現段階でメソッドハンドルを使うように統一して備える


### JEP421: Deprecate Finalization for Removal
finalize()メソッドを削除を前提とした非推奨APIに
 - すでにJava9から非推奨
 - 18で@Deprecated(forRemoval=true)にしたということ
 - Java実行時オプションで--finalization=enabled/disabledを設定しないといけなくなる


### コンテナ環境でのUnified Logging(ULログ)
 - JVM内部のすべてのコンポーネントで共通的に利用する
 - Java9から

ULログに複数行エントリが出力されてしまう

#### ULに新オプションを導入した
ULのoutput-optionsにfoldmultilines=trueを指定するとULログのエントリを常に1行で出力する


### HSDISの改善
HotSpot Disassembler
 -  ディスアセンブラ、逆アセンブラ

JITコンパイル

機械語からアセンブリ言語にディスアセンブルする
JITコンパイラが生成したコードを確認できる
 - 特定のメソッドがコンパイルされているかを確認したい場合
とくに想定するCPU命令を使っているかを確認したいとき
 - 特定箇所にパフォーマンス問題があるとき
 - 実行時オプション とくに

どんな改善点なのか
別ディレクトリでビルドするのではなくOpenJDK本体と同じようにビルドできるようになった
ディスアセンブルのバックエンドにGNU BinutilsだけでなくLLVMとCapstoneも使えるようになった





