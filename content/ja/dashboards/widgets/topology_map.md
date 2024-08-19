---
aliases:
- /ja/dashboards/widgets/service_map
description: 1 つのサービスについて、それを呼び出したすべてのサービスおよびそれから呼び出されたすべてのサービスを表すマップを表示する
further_reading:
- link: /ja/dashboards/graphing_json/
  tag: ドキュメント
  text: JSON を使用したダッシュボードの構築
- link: /tracing/services/services_map/
  tag: ドキュメント
  text: サービスマップ
title: Topology Map ウィジェット
widget_type: topology_map
---

The Topology Map widget displays a visualization of data sources and their relationships to help understand how data flows through your architecture. 

## Setup

### Configuration

1. Choose the data to graph:
    * Service Map: The node in the center of the widget represents the mapped service. Services that call the mapped service are shown with arrows from the caller to the service. To learn more about the Service Map, reference the [Service Map feature of APM][1].

2. Enter a title for your graph.

## API

This widget can be used with the **[Dashboards API][2]**. See the following table for the [widget JSON schema definition][3]:

{{< dashboards-widgets-api >}}

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/tracing/services/services_map/
[2]: /ja/api/latest/dashboards/
[3]: /ja/dashboards/graphing_json/widget_json/