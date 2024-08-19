---
aliases:
- /ja/graphing/widgets/query_value/
description: 1 つのメトリクスクエリの集計値を表示する
further_reading:
- link: /ja/dashboards/graphing_json/
  tag: ドキュメント
  text: JSON を使用したダッシュボードの構築
title: クエリ値ウィジェット
widget_type: query_value
---

Query values display the current value of a given metric, APM, or log query. They come with conditional formatting (such as a green/yellow/red background) to convey whether the value is in the expected range. This can be supplemented with optional backgrounds of timeseries data. The values displayed by a query value do not require an instantaneous measurement.

The widget can display the latest value reported, or an aggregate computed from all query values across the time window. These visualizations provide a narrow but unambiguous window into your infrastructure query.

{{< img src="dashboards/widgets/query_value/query_value1.png" alt="Query value widget" style="width:80%;" >}}

## Setup

{{< img src="dashboards/widgets/query_value/query-value-widget-setup1.png" alt="Query value widget setup" style="width:80%;">}}

### Configuration

1. Choose the data to graph:
    * Metric: See the [Querying documentation][1] to configure a metric query.
    * Indexed Spans: See the [Trace search documentation][2] to configure an Indexed Span query.
    * Log Events: See the [Log search documentation][3] to configure a log event query.
2. Reduce the query values to a single value, calculated as the `avg`, `min`, `sum`, `max`, or `last` value of all data points in the specified timeframe.
3. Choose the units and the formatting. Autoformat scales the dashboard for you based on the units.
4. オプションで、表示される値に応じて条件付き書式を構成できます。詳しくは、[ビジュアルフォーマットルール](#visual-formatting-rules)を参照してください。
5. Optionally, overlay a timeseries background:
    * Min to Max: A scale graph from minimum to maximum.
    * Line: A scale graph to include zero (0).
    * Bars: Shows discrete, periodic measurements.

### Options

#### ビジュアルフォーマットルール

<div class="alert alert-info">ビジュアルフォーマットルールは、メトリクスの生の値に基づく必要があります。メトリクスの基本単位がナノ秒であり、Query Value が秒に自動フォーマットされる場合、条件付きルールはナノ秒に基づく必要があります。</div>

条件付きルールを使用して、Query Value ウィジェットの背景をカスタマイズできます。背景色、フォント色、またはカスタムイメージを追加することが可能です。カスタムイメージを使用する場合は、内部画像を参照するクロスオリジンリクエストをサポートするために内部サーバーを更新する必要があります。

{{< img src="dashboards/widgets/query_value/visual_formatting_rules_custom_img.png" alt="カスタムイメージの背景を持つ Query Value ウィジェットのビジュアルフォーマットルール" style="width:90%;" >}}

#### Context links

[Context links][4] are enabled by default, and can be toggled on or off. Context links bridge dashboard widgets with other pages in Datadog, or third party applications.

#### Global time

Choose whether your widget has a custom timeframe or the dashboard's global timeframe.

## API

This widget can be used with the **[Dashboards API][5]**. See the following table for the [widget JSON schema definition][6]:

{{< dashboards-widgets-api >}}

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/dashboards/querying/#overview
[2]: /ja/tracing/trace_explorer/query_syntax/#search-bar
[3]: /ja/logs/search_syntax/
[4]: /ja/dashboards/guide/context-links/
[5]: /ja/api/latest/dashboards/
[6]: /ja/dashboards/graphing_json/widget_json/
