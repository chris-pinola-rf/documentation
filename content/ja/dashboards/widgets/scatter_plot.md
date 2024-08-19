---
aliases:
- /ja/graphing/widgets/scatter_plot/
description: 2 つのメトリクスとそれぞれの集計を使用して、選択したスコープをグラフ化する
further_reading:
- link: /ja/dashboards/graphing_json/
  tag: ドキュメント
  text: JSON を使用したダッシュボードの構築
title: 散布図ウィジェット
widget_type: scatterplot
---

散布図は、2 つの異なる変数セットで観測された変化の関係を識別します。視覚的かつ統計的な手段を提供し、2 つの変数間の関係の強さをテストします。散布図の可視化では、2 つの異なるメトリクスとそれぞれの集計に対して、選択したスコープをグラフ化することができます。

{{< img src="dashboards/widgets/scatterplot/scatterplot.png" alt="Scatter Plot" >}}

## Setup

### Configuration

1. Select a metric or other data set, and an aggregation for the X and Y axis.
1. Define the scope for each point of the scatter plot, such as `host`, `service`, `app`, or `region`.
1. Optional: enable a color-by tag.
1. Optional: set X and Y axis controls.
1. Choose whether your widget has a custom timeframe or the dashboard's global timeframe.
1. Give your graph a title or leave the box blank for the suggested title.

### Options

#### Context links

[Context links][1] are enabled by default, and can be toggled on or off. Context links bridge dashboard widgets with other pages in Datadog, or third party applications.

#### Global time

Choose whether your widget has a custom timeframe or the dashboard's global timeframe.

## API

This widget can be used with the **[Dashboards API][2]**. See the following table for the [widget JSON schema definition][3]:

{{< dashboards-widgets-api >}}

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/dashboards/guide/context-links/
[2]: /ja/api/latest/dashboards/
[3]: /ja/dashboards/graphing_json/widget_json/