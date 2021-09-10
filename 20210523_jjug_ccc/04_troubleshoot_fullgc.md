# JFR などのツールを用いて FullGC や OOME の原因を特定する流れ

 -  heapサイズを指定しないとマシンによって挙動が変わる

## 問題の特定をどうするか



## JFR

##  heapが上昇する原因調査
ヒープダンプを取得

-XX:+HeapDumpOutofMemoryError
かな？

MemoryAnalyzer
でヒープダウンプを解析可能

円グラフからどのオブジェクトがどのくらいのヒープを利用しているかがわかる

Actions->Histogram
でクラスごとのインスタンス数が観れる


## JFRでヒープ統計を有効にする


コンテナ環境でのJFR
https://speakerdeck.com/hhiroshell/jvm-on-kubernetes?slide=58

---
その他
JFRをコンテナ環境で使う場合は出力先をマウントした場所に、jcmdを使えるようにしておくのがいい。
ECSならEFSをマウントしておくとか
---


