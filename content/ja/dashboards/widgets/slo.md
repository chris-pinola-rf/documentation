---
aliases:
- /ja/monitors/monitor_uptime_widget/
- /ja/monitors/slo_widget/
- /ja/graphing/widgets/slo/
- /ja/dashboards/faq/how-can-i-graph-host-uptime-percentage/
description: SLO の追跡
further_reading:
- link: https://www.datadoghq.com/blog/slo-monitoring-tracking/
  tag: ブログ
  text: Datadog ですべての SLO のステータスを追跡する
- link: /dashboards/guide/slo_graph_query
  tag: Documentation
  text: メトリクスベースの SLO クエリをスコープする
title: SLO ウィジェット
widget_type: slo
---

SLO (サービスレベル目標) とは、顧客の成功を最大限にするために、アクティビティ、機能、プロセスごとに達成すべき合意された目標です。SLO はサービスのパフォーマンスや健全性を表します。SLO ウィジェットは、既存の SLO のステータス、予算、残りのエラーバジェットを可視化します。ウィジェット内のタイムウィンドウに基づいてグループを並べ替えることができ、SLO のすべての基礎グループを表示します。このウィジェットを使用して、最も重要な SLO 情報を含む意味のあるダッシュボードを作成します。
- **ウィジェットですべての SLO グループを直接表示します**: ウィジェットは SLO グループに関連する重要な情報を提供するため、多くのグループを含む SLO に役立ちます。
- **ウィジェットで SLO グループを並べ替える順序を設定します**: すべての SLO タイプについて、ウィジェットの利用可能なタイムウィンドウに基づいてグループを並べ替えます。異なる期間で最もパフォーマンスの良い SLO グループと悪い SLO グループを迅速に特定します。
- **SLO のデータが欠落している期間を簡単に特定できます**: すべての SLO タイプについて、SLO ウィジェットはデータが欠落している期間を「-」で表示します。ウィンドウ全体でデータが欠落している場合、そのタイムウィンドウには「-」が表示されます。

## Setup

SLO ウィジェットを使用して、ダッシュボード上で[サービスレベル目標 (SLO)][1] を視覚化します。

{{< img src="/dashboards/widgets/slo/slo-summary-widget-new.png" alt="メトリクスベースの SLO サマリーウィジェットグラフエディタ " >}}

### Configuration

1. ドロップダウンメニューから SLO を選択します。
2. **メトリクスベースおよび Time Slice SLO の場合**: タグでクエリをフィルタリングし、[テンプレート変数][2]を活用して動的に結果をスコープすることができます。
    - テンプレート変数を活用するには、*filter by* フィールドを使用して、ウィジェットが表示する SLO ステータスをスコープします。たとえば、`filter by $env` は、ダッシュボードで *env* テンプレート変数に選択したすべての値に SLO クエリをスコープします。
    - Add additional scope and context to your SLO metric queries even if the tags were not included in the original SLO configuration. For example, if the original SLO query is `sum:trace.flask.request.hits{*} by {resource_name}.as_count()` and you filter by `env:prod` in the widget, your data will be scoped to only that from your `prod` environment.
3. タイムウィンドウは 3 つまで設定できます。
4. 表示設定を選択します。

### Options

#### タイムウィンドウを設定する

タイムウィンドウは以下から 3 つまで選択できます。
- **Rolling time windows**: 7 日、30 日、90 日
- **Calendar time windows**: 週から日、前の週、月から日、前の月
- **Global time**: このオプションにより、任意の期間にわたって SLO のステータスとエラーバジェットを表示できます。モニターベースの SLO では、最大 3 か月の履歴情報を表示できます。 Time Slice およびメトリクスベースの SLO の場合、サポートされる履歴ビューはアカウントのメトリクス保持期間に一致します (デフォルトでは 15 か月) 。

**注**: エラーバジェットを表示し、`Global time` SLO ステータス値を緑または赤に色分けするには、SLO ターゲットを指定する必要があります。SLO 入力ターゲットが指定されていない場合、SLO ステータスのみが表示され、フォントの色はグレーのままです。

#### Display preferences

`Show error budget` オプションを切り替えて、残りのエラーバジェットを表示するか非表示にするかを選択します。

複数のグループを持つ SLO や複数のモニターを持つモニターベースの SLO を可視化している場合、`View mode` を選択します。

- グループを持つ SLO (グループを持つメトリクスベースまたは Time Slice の SLO、または単一モニターをグループに分割したモニターベースの SLO) には、以下の 3 つの表示モードがあります。
  - `Overall`: displays the overall SLO status percentages and targets
  - `Groups`: displays a table of status percentages for each group
  - `Both`: displays both the overall SLO status percentages and targets and table of status percentages for each group

- For monitor-based SLOs configured with multiple monitors, there are the following three view modes:
  - `Overall`: displays the overall SLO status percentages and targets
  - `Monitors`: displays a table of status percentages for each monitor
  - `Both`: displays both the overall SLO status percentages and targets and table of status percentages for each monitor

`View mode` を `Groups`、`Monitors`、または `Both` に設定した場合
- デフォルトでは、グループは最小のタイムウィンドウでステータスの昇順にソートされます。ダッシュボードにウィジェットを追加した後、ウィジェット UI から構成されたタイムウィンドウのステータスでソートできます。
- ウィジェットには以下が表示されます。
  + メトリクスベースおよび Time Slice の SLO の場合、SLO のすべての基礎グループが表示されます。
  + 複数のモニターを持つモニターベースの SLO の場合、SLO のすべての基礎モニターが表示されます。
  + 単一モニターベースの SLO でグループがある場合、SLO で特定のグループが選択されていれば最大 20 グループが表示されます。特定のグループが選択されていない場合、SLO の*すべての*基礎グループが表示されます。

**注**: グループがあるモニターベースの SLO では、最大 5,000 グループを含む SLO に対してすべてのグループを表示できます。5,000 を超えるグループを含む SLO の場合、SLO はすべてのグループに基づいて計算されますが、UI にはグループは表示されません。

## API

This widget can be used with the **[Dashboards API][3]**. See the following table for the [widget JSON schema definition][4]:

{{< dashboards-widgets-api >}}

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/service_management/service_level_objectives/
[2]: /ja/dashboards/template_variables/
[3]: /ja/api/latest/dashboards/
[4]: /ja/dashboards/graphing_json/widget_json/