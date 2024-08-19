---
aliases:
- /ja/graphing/widgets/alert_value/
description: システムで定義されているシンプルアラート型メトリクスモニター内のメトリクスの現在値をグラフ化する
further_reading:
- link: /ja/dashboards/graphing_json/
  tag: ドキュメント
  text: JSON を使用したダッシュボードの構築
title: アラート値ウィジェット
widget_type: alert_value
---

The Alert value widget displays the current query value from a simple-alert metric monitor. Simple-alert monitors have a metric query that is not grouped and returns one value. Use Alert value widgets in your dashboard to get an overview of your monitor behaviors and alert statuses.

{{< img src="dashboards/widgets/alert_value/alert_value_2023.png" alt="Three alert value widgets with three different monitor statuses for disk space, high cpu and checkout error rate" >}}

## Setup
{{< img src="dashboards/widgets/alert_value/alert_value_setup_2023.png" alt="Alert Value setup page for high cpu monitor" style="width:100%;">}}

### Configuration

1. Choose an existing metric monitor to graph.
2. Select the formatting to display:
    * Decimal
    * Units
    * Alignment
3. Give your graph a title.

## API

This widget can be used with the **[Dashboards API][1]**. See the following table for the [widget JSON schema definition][2]:

{{< dashboards-widgets-api >}}

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/api/v1/dashboards/
[2]: /ja/dashboards/graphing_json/widget_json/