---
aliases:
- /ja/graphing/widgets/top_list/
further_reading:
- link: /ja/dashboards/graphing_json/
  tag: ドキュメント
  text: JSON を使用したダッシュボードの構築
- link: /notebooks/
  tag: ドキュメント
  text: ノートブック
- link: /dashboards/guide/context-links/#overview/
  tag: Documentation
  text: コンテキストリンク
title: トップリストウィジェット
widget_type: トップリスト
---

The top list visualization enables you to display a list of tag values with the most or least of any metric or event value, such as highest consumers of CPU, hosts with the least disk space, or cloud products with the highest costs.

## Setup

{{< img src="dashboards/widgets/toplist/top_list_graph_display.png" alt="Configuration options for graph display highlighting Stacked, Relative display mode, and Visual Formatting Rules" style="width:100%;" >}}

### Configuration

1. Choose the data to graph:
    * Metric: See the [querying][1] documentation to configure a metric query.
    * Non-metric data sources: See the [Trace search documentation][2] or [Log search documentation][3] to configure an event query.

2. Optional: see additional [graph display](#graph-display) configurations. 

### Options

#### Graph display

Configure the optional Display Mode features to add context to your top list visualization.

* Display multiple stacked groups to show a break down of each dimension in your query. **Stacked** is enabled by default. You can switch to **Flat**.
* Select **Relative** display mode to show values as a percent of the total or **Absolute** display mode to show the raw count of data you are querying.</br>
   **Note**: Relative display is only available for count data, such as count metrics or log events.
* Configure conditional formatting in **Visual Formatting Rules** depending on your entries' values. 

#### Context links

[Context links][4] are enabled by default, and can be toggled on or off. Context links bridge dashboard widgets with other pages in Datadog, or third party applications.

#### Global time

On screenboards and notebooks, choose whether your widget has a custom timeframe or uses the global timeframe.

## API

This widget can be used with the **[Dashboards API][5]**. See the following table for the [widget JSON schema definition][6]:

{{< dashboards-widgets-api >}}

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/dashboards/querying/
[2]: /ja/tracing/trace_explorer/query_syntax/#search-bar
[3]: /ja/logs/search_syntax/
[4]: /ja/dashboards/guide/context-links
[5]: /ja/api/latest/dashboards/
[6]: /ja/dashboards/graphing_json/widget_json/