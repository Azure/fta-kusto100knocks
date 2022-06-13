---
title: "基本編"
weight: 10
type: docs
description: "KUSTO の基本となるフィルターやソート、日付に関するクエリを学習します。"
draft: false
---


# はじめに

## 使い方
KQL を初めて使う方が KQL の書き方を身に着けるための演習用の問題集と回答、解説です。  
実際の KQL を使う場面で頻繁に出現するコマンド（演算子、関数）を様々な角度からいろいろな使い方で実行することで、手でコマンドを覚えてもらうような利用方法を想定しています。

## 用語
KQL では他のプログラム言語と同じように、各言語要素に対して名前がつけられています。この名前を覚えることは必須ではありませんが、学習効率を高めるためには意識しておいたほうが良い要素です。単に全てを「コマンド」とひとくくりにするのではなく、現在自分はどの要素が使っているのかを意識しながら KQL を書くと仕組みへの理解が速やかに深まります。
- 演算子、オペレーター： 意味は同じです。
- テーブル演算子： 最も多く使われるオペレーターです。テーブルの入力を受け付け、テーブルを出力します。処理の最初やパイプラインの次に現れる要素はテーブル演算子です。
- スカラー演算子： 主にテーブル オペレーターの中で使われるオペレーターです。数値演算子、論理演算子、文字列演算子などがあり、テーブルの各行の特定の列の値などデータ処理を行います。後述の関数と区別がつきにくいのですが、 between と not-between はスカラー演算子です。
- 関数、ファンクション： 意味は同じで特定の処理結果を返します。入力値に対して処理を実施するものや、入力値を取らずに値を返すものがあります。スカラー演算子と似ているので注意してください。


## リンク

- [Log Analytics DEMO workspace](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring_Logs/DemoLogsBlade)  
サンプル データが用意されているデモ ワークスペースです。このページの KQL はこのデータに対して実行することを想定しています。

- [Kusto Query Language](https://docs.microsoft.com/ja-jp/azure/data-explorer/kusto/query/)  
Azure Data Explorer の KQL のリファレンスです。Log Analytics ではこのなかの一部が実装されています。


# 基本の KQL

## 目的の情報がどこにあるかを探す
Azure Monitor のログを検索するためにはワークスペースにどのようなテーブルがあり、各テーブルはどのような構造のレコードを格納しているかを知る必要があります。ここでは基本的なオペレーターを使用して、目的の情報を含んだテーブルを見つけたり、よく使われる テーブルを題材に、探し出したテーブルに含まれるデータの構造を調べる方法を学習します。

{{<kql lang="ja" category="Basic" kind="filter">}}

### 参考ドキュメント

- Azure リソース ログの共通およびサービス固有のスキーマ
  - https://docs.microsoft.com/azure/azure-monitor/essentials/resource-logs-schema
- Azure アクティビティ ログのイベント スキーマ
  - https://docs.microsoft.com/azure/azure-monitor/essentials/activity-log-schema
- search 演算子
  - https://docs.microsoft.com/azure/data-explorer/kusto/query/searchoperator?pivots=azuredataexplorer
- distinct 演算子
  - https://docs.microsoft.com/azure/data-explorer/kusto/query/distinctoperator
- where 演算子
  - https://docs.microsoft.com/azure/data-explorer/kusto/query/whereoperator




## 日付を指定・書式化する
Azure Monitor のログには、標準で定義されている列があり、データソースにより生成される時刻を示す `TimeGenerated` 列 によってログレコードの時刻が確認できます。`TimeGenerated` 列を使用することで、時刻を基にしたフィルターやレコードの件数を確認できます。

{{<kql lang="ja" category="Basic" kind="datetime">}}

### 参考ドキュメント
- 日付と時刻の操作
  - https://docs.microsoft.com/ja-jp/azure/data-explorer/kusto/query/samples?pivots=azuremonitor#date-and-time-operations
- Azure Monitor ログ内の標準列
  - https://docs.microsoft.com/ja-jp/azure/azure-monitor/logs/log-standard-columns#timegenerated
- Azure Monitor Log Analytics のログ クエリのスコープと時間範囲
  - https://docs.microsoft.com/ja-jp/azure/azure-monitor/logs/scope#time-range
- datetime データ型
  - https://docs.microsoft.com/azure/data-explorer/kusto/query/scalar-data-types/datetime
- Timespan データ型
  - https://docs.microsoft.com/ja-jp/azure/data-explorer/kusto/query/scalar-data-types/timespan

## データを整える
Azure Monitor に保存されるログは生成元のリソースに応じて多様なフィールドをもち、多様な分析を行うことができます。一方、全てのフィールドを表示してしまう、テーブルに非常に多くの列が表示され、結果のデータセットは大きく、見づらくなってしまいます。ここではデータから必要なフィールドだけを選び出したり、フィールドの名前を変更して分析しやすくしたりなど、データを整型する方法について学習します。


{{<kql lang="ja" category="Basic" kind="column">}}


## データを並び替え・集約する
ログは特定の目的のために並び替えを行ったり、データの特定の側面にフォーカスするために集計をする必要があります。ここでは `summarize` 演算子と、集計関数を使用して、ログやメトリックに含まれる情報を整理する方法について学習します。

{{<kql lang="ja" category="Basic" kind="sort">}}

### 参考ドキュメント
- summarize 演算子
  - https://docs.microsoft.com/ja-jp/azure/data-explorer/kusto/query/summarizeoperator
- 集計関数
  - https://docs.microsoft.com/ja-jp/azure/data-explorer/kusto/query/aggregation-functions

## 可視化する
Azure Monitor ではクエリで取得したデータセットのテーブルを見やすく表示することができます。ここでは `render` 演算子を使用して、データセットを棒グラフや折れ線グラフとして可視化する方法を学習します。

{{<kql lang="ja" category="Basic" kind="visualize">}}

### 参考ドキュメント
- render 演算子
  - https://docs.microsoft.com/ja-jp/azure/data-explorer/kusto/query/renderoperator?pivots=azuremonitor

## テーブルを結合する
数のテーブルに分散した特定のリソースの情報を分析したいようなケースではテーブルを結合することができます。よく似た（あるいは全く同じ性質の）テーブル同士を結合したデータセットに対して分析を行ったり、あるいはリソースへの追加となる情報を持つテーブルから特定の情報を選び出し、データをエンリッチすることができます。ここではテーブルを結合するための演算子を学習します。


{{<kql lang="ja" category="Basic" kind="join">}}

### 参考ドキュメント
- union 演算子
  - https://docs.microsoft.com/ja-jp/azure/data-explorer/kusto/query/unionoperator?pivots=azuremonitor
- join 演算子
  - https://docs.microsoft.com/ja-jp/azure/data-explorer/kusto/query/joinoperator?pivots=azuremonitor

## データをフォーマットする
Azure Monitor に保存されるログは、全部が分析のために最適な形になっているわけではありません。具体的にはログの生成元が XML や JSON、あるいは構造化されていないただのテキストデータを生成していて、そのデータは複数のフィールドとして加工されずにどこか一つのフィールドに格納されていることがあります。ここでは、そのようなフィールドを扱いやすく加工するための方法について学習します。

{{<kql lang="ja" category="Basic" kind="format">}}

### 参考ドキュメント
- parse 演算子
  - https://docs.microsoft.com/ja-jp/azure/data-explorer/kusto/query/parseoperator




## その他
ここでは複雑なクエリを書く際に頻繁に使われる記法や、必要とされる頻度は多くないものの特定のな問題を効率よく解決するための KQL を紹介します。

色々な分析の中で

{{<kql lang="ja" category="Basic" kind="other">}}


参考ドキュメント
- externaldata 演算子
  - https://docs.microsoft.com/ja-jp/azure/data-explorer/kusto/query/externaldata-operator?pivots=azuremonitor
