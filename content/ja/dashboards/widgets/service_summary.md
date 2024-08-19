---
aliases:
- /ja/graphing/widgets/service_summary/
description: 選択したサービスのグラフをダッシュボードウィジェットに表示します。
further_reading:
- link: /ja/dashboards/graphing_json/
  tag: ドキュメント
  text: JSON を使用したダッシュボードの構築
- link: /tracing/services/service_page/
  tag: ドキュメント
  text: APM サービス詳細画面について
title: サービスサマリーウィジェット
widget_type: trace_service
---

サービスとは、例えば Web フレームワークやデータベースなど、同じ作業を行う一連のプロセスのことです。Datadog は、サービスページに表示されるような、サービス情報を表示するためのすぐに使えるグラフを提供します。サービスサマリーウィジェットを使用して、選択した[サービス][1]のグラフをダッシュボードに表示します。

{{< img src="dashboards/widgets/service_summary/service_summary.png" alt="service summary" >}}

## Setup

### Configuration

1. Select an [environment][2] and a [service][1].
2. Select a widget size.
3. Select the information to display:
    * Hits
    * Errors
    * Latency
    * Breakdown
    * Distribution
    * Resource (**Note**: You need to select the large widget size to see this option.)
4. Choose your display preference by selecting the number of columns to display your graphs across.
5. Enter a title for your graph.

## API

This widget can be used with the **[Dashboards API][3]**. See the following table for the [widget JSON schema definition][4]:

{{< dashboards-widgets-api >}}

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/tracing/services/service_page/
[2]: /ja/tracing/send_traces/
[3]: /ja/api/latest/dashboards/
[4]: /ja/dashboards/graphing_json/widget_json/