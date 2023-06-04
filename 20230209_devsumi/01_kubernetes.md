# Kubernetesとカスタムコントローラーを活用したプラットフォーム開発・運用の勘所

## カスタムコントローラ

 - Deploymentを適用するといい感じにPodが起動する
  この仕組みは内部でコントローラパターンで実現している

 Reconciliation Loopの基本
  1. 指定された「望ましい状態(Desired State)」を取得する
  2. 実際の状態(Actual State)」を観測する
  3. 「実際の状態」を「望ましい状態」に一致するように変更する
  4. 1〜3を繰り返す

 - カスタムコントローラ
  - Reconciliation Loopを新たに実装する

 CRD(Custom Resource Definition)
  - カスタムリソースのフォーマットを定義するKubernetesリソース
  - CRDをKubernetesに適用すると、カスタムリソースをAPI Serverで管理できるようになる


