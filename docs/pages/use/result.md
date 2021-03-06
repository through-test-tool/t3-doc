# Result

`ETTT`を実行した結果について説明します。

## Structure

結果ディレクトリ構成を以下に示します。

```
result_root_dirctory
 |
 |-- YYYYMMDD_HHMMSS #（1）
 |     |
 |     |-- report.html #（2）
 |     |
 |     `--{scenario_id}_YYYYMMDD_HHMMSS #（3）
 |           |
 |           |-- scenario.html #（4）
 |           |
 |           |-- details #（5）
 |           |     |
 |           |     `-- {custom_command_reports}_{execute_id}.html #（6）
 |           |
 |           `-- evidences #（7）
 |                 |
 |                 |-- {execute_id}_*.log #（8）
 |                 `-- etc...
```

`result_root_directory`は`ETTT`の起動時に指定することのできる出力ディレクトリになります。
その配下に決められたディレクトリ構成、ファイル名に従った結果ファイル群が出力されます。
ディレクトリ構成にあるそれぞれの要素について説明をします。

1. 結果ディレクトリとして実行日時ディレクトリが作成されます。
1. 総合レポートして`report.html`が出力されます。
1. 実行するシナリオ毎にシナリオIDと実行日時を合わせたディレクトリが作成されます。
1. シナリオ個別のレポートとして`scenario.html`が出力されます。
1. `details`はカスタム機能固有のレポートを出力するためのディレクトリです。
1. カスタムレポートを作成する機能を利用した際にレポートが出力されます。
1. 各種エビデンスが配置されるディレクトリです。
1. エビデンスには、必ず`execute_id`というUUIDにて生成したIDが割り振られます。

## Report

### All Report

総合レポートはツール実行単位に１回出力されるレポートです。
HTMLファイルの場合は以下のようなイメージです。

![report.html](/pages/use/images/report-001.png)

出力内容は、起動オプション、実行結果、時間等になります。
また、実行したシナリオの一覧を出力しておりそれらが正常終了したのか、異常終了したのかを判断しやすくしています。
シナリオIDはリンクになっており、シナリオレポートへ遷移します。

#### ステータス
ステータスは`ETTT`の実行ステータスとなり総合的に結果がいかがであったのかを表すものです。
上図の（１）に該当する部分になります。
取りうる値とそれぞれの意味については以下の通りです。

|ステータス|意味|終了コード|
|:---|:---|:---|
|WAIT|実行待機状態です|100|
|SKIP|実行をスキップされた状態です|100|
|RUNNING|実行中の状態です|100|
|SUCCESS|正常終了|0|
|ASSERT_ERROR|いずれかのシナリオにアサートエラーが存在します|2|
|ERROR|いずれかのシナリオにエラーが存在します|9|

基本的に、`SUCCESS`・`ASSERT_ERROR`・`ERROR`がレポートには出力されます。
`ASSERT_ERROR`と`ERROR`の違いについてですが、`ETTT`としては正常に処理を終了することはできたが
試験としてはアサートにてNG判断があった場合に`ASSERT_ERROR`となります。
それとは異なり、`ETTT`としては実行時のトラブルや予期せぬエラー（ツール故障を含む）、
利用方法の不正等によるエラーが発生した場合に`ERROR`となります。
それぞれアプリケーションとしての終了コードを分けていますので判断ポイントとして活用できます。

注意ポイントとしては、複数シナリオのいずれかで`ASSERT_ERROR`・`ERROR`が存在した場合には全て異常終了扱いになることです。


#### シナリオステータス

基本的には`ETTT`の総合ステータスと同一です。
総合ステータスは、シナリオステータスを束ねたものだからです。
レポート上では上図の（２）に該当する部分となります。
取りうる値とそれぞれの意味については以下の通りです。

|ステータス|意味|
|:---|:---|
|WAIT|実行待機状態です|
|SKIP|実行をスキップされた状態です|
|RUNNING|実行中の状態です|
|SUCCESS|正常終了|
|ASSERT_ERROR|シナリオにアサートエラーが存在します|
|ERROR|シナリオにエラーが存在します|

#### グローバル変数

グローバル変数は`ETTT`が起動中常に共有する変数領域です。
そのため総合レポートに出力しています。上図の（３）に該当する部分になります。
KeyとValueの組み合わせを定義しています。

### Scenario Report

シナリオレポートは、シナリオを実行毎に１回出力されるレポートです。
HTMLファイルの場合は以下のようなイメージです。

![report.html](/pages/use/images/scenario-report-001.png)