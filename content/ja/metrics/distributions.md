---
aliases:
- /ja/developers/faq/characteristics-of-datadog-histograms/
- /ja/graphing/metrics/distributions/
description: データセット全体のグローバルパーセンタイルを計算する
further_reading:
- link: /metrics/custom_metrics/dogstatsd_metrics_submission/
  tag: ドキュメント
  text: DogStatsD でのディストリビューションの使用
title: ディストリビューション
---
## Overview

Distributions are a metric type that aggregate values sent from multiple hosts during a flush interval to measure statistical distributions across your entire infrastructure.

Global distributions instrument logical objects, like services, independently from the underlying hosts. Unlike [histograms][1] which aggregate on the Agent-side, global distributions send all raw data collected during the flush interval and the aggregation occurs server-side using Datadog's [DDSketch data structure][2]. 

Distributions provide enhanced query functionality and configuration options that aren't offered with other metric types (count, rate, gauge, histogram):
* **Calculation of percentile aggregations**: Distributions are stored as DDSketch data structures that represent raw, unaggregated data such that globally accurate percentile aggregations (p50, p75, p90, p95, p99 or any percentile of your choosing with up to two decimal points) can be calculated across the raw data from all your hosts. Enabling percentile aggregations can unlock advanced query functionalities such as: 

  * **Single percentile value over any timeframe**:

     _"What has the 99.9th percentile load time for my application been over the past week?"_

  * **任意の時間枠における標準偏差**:

     _"過去 1 か月間におけるアプリケーションの CPU 消費量の標準偏差 (stddev) は何ですか？"_

  * **Percentile thresholds on metric monitors**:

    _"Alert me when the p95 of my application's request latency is greater than 200 ms for the last 5 min."_

  * **Threshold Queries**:

    _"I'd like to define a 30-day SLO where 95% of requests to my service are completed in under 5 seconds."_


* **Customization of tagging**: This functionality allows you to control the tagging scheme for custom metrics for which host-level granularity is not necessary (for example, transactions per second for a checkout service).

See the [Developer Tools section][1] for more implementation details. 

**Note:** Because distributions are a new metric type, they should be instrumented under new metric names during submission to Datadog.

## Enabling advanced query functionality

他のメトリクスタイプ、例えば `gauges` や `histograms` と同様に、ディストリビューションでも `count`、`min`、`max`、`sum`、`avg` の集計が可能です。ディストリビューションは、最初は他のメトリクスと同じようにタグ付けされ、カスタムタグはコードで設定されます。その後、メトリクスを報告したホストに基づいてホストタグに解決されます。

しかし、Metrics Summary ページでは、ディストリビューション上のすべてのクエリ可能なタグについて、グローバルに正確なパーセンタイル集計を計算するなどの高度なクエリ機能を有効にすることができます。これは、`p50`、`p75`、`p90`、`p95`、`p99`、またはユーザーが選択した任意のパーセンタイル (99.99 のように小数点以下 2 桁まで) の集計を提供するものです。高度なクエリを有効にすると、しきい値クエリおよび標準偏差も有効になります。

{{< img src="metrics/distributions/metric_detail_enable_percentiles.mp4" alt="A user enabling advanced percentiles and threshold query functionality by clicking on configure under the advanced section of a metric detail panel" video=true width=80% >}}

ディストリビューションメトリクスにパーセンタイル集計を適用することを選択した後、これらの集計がグラフ作成 UI で自動的に利用可能になります。

{{< img src="metrics/distributions/graph_percentiles.mp4" alt="Distribution metric aggregations" video=true" >}}

You can use percentile aggregations in a variety of other widgets and for alerting: 
* **Single percentile value over any timeframe**

   _"What has the 99.9th percentile request duration for my application been over the past week?"_ 

{{< img src="metrics/distributions/percentile_qvw.jpg" alt="A query value widget displaying a single value (7.33s) for the 99.99 percentile aggregation of a single metric" style="width:80%;">}}

* **Percentile thresholds on metric monitors**
  _"Alert me when the p95 of my application's request latency is greater than 200 ms for the last 5 min."_ 

{{< img src="metrics/distributions/percentile_monitor.jpg" alt="Percentile threshold being set with a dropdown for alert conditions in a monitor " style="width:80%;">}}

### Threshold Queries

<div class="alert alert-warning">
Threshold queries are in public beta.
</div>

DDSketch によって計算されたディストリビューションメトリクスのグローバルで正確なパーセンタイルを有効にすると、しきい値クエリが解放されます。ここでは、生のディストリビューションメトリクス値が数値のしきい値を超えるか下回る場合にその数を数えることができます。この機能を利用して、異常な数値のしきい値と比較したエラーや違反の回数をダッシュボードでカウントすることができます。また、しきい値クエリを使用して、「過去 30 日間に 95% のリクエストが 10 秒以内に完了した」というような SLO を定義することも可能です。

With threshold queries for distributions with percentiles, you do not need to predefine a threshold value prior to metric submission, and have full flexibility to adjust the threshold value in Datadog.

To use threshold queries: 

1. Enable percentiles on your distribution metric on the Metrics Summary page.
2. Graph your chosen distribution metric using the "count values..." aggregator.
3. Specify a threshold value and comparison operator.

{{< img src="metrics/distributions/threshold_queries.mp4" video=true alt="A timeseries graph being visualized using the count values aggregator, with a threshold of greater than 8 seconds" style="width:80%;" >}}

You can similarly create a metric-based SLO using threshold queries: 
1. Enable percentiles on your distribution metric on the Metrics Summary page.
2. 新しいメトリクスベースの SLO を作成し、「count values...」集計方法を使用して選択したディストリビューションメトリクスのクエリで、「good」イベントの数を分子として定義します。
3. Specify a threshold value and comparison operator.
{{< img src="metrics/distributions/threshold_SLO.jpg" alt="Threshold Queries for SLOs" style="width:80%;">}}

## Customize tagging

Distributions provide functionality that allows you to control the tagging for custom metrics where host-level granularity does not make sense. Tag configurations are _allowlists_ of the tags you'd like to keep. 

To customize tagging:

1. Click on your custom distribution metric name in the Metrics Summary table to open the metrics details sidepanel.
2. Click the **Manage Tags** button to open the tag configuration modal.
3. Click the **Custom...** tab to customize the tags you'd like to keep available for query. 

**注**: 許可リストベースのタグのカスタマイズでは、タグの除外はサポートされていません。`!` で始まるタグは追加できません。

{{< img src="metrics/distributions/dist_manage.jpg" alt="Configuring tags on a distribution with the Manage Tags button" style="width:80%;">}}

## Audit events
タグコンフィギュレーションまたはパーセンタイル集計の変更があった場合、[イベントエクスプローラー][3]にイベントが作成されます。このイベントでは、変更内容が説明され、変更を行ったユーザーが表示されます。

ディストリビューションメトリクスのタグ構成を作成、更新、または削除した場合、以下のイベント検索で例を見ることができます。
```text
https://app.datadoghq.com/event/stream?tags_execution=and&per_page=30&query=tags%3Aaudit%20status%3Aall%20priority%3Aall%20tag%20configuration
```

If you added or removed percentile aggregations to a distribution metric, you can see examples with the following event search:
```text
https://app.datadoghq.com/event/stream?tags_execution=and&per_page=30&query=tags%3Aaudit%20status%3Aall%20priority%3Aall%20percentile%20aggregations
```

## Further reading

{{< partial name="whats-next/whats-next.html" >}}


[1]: /ja/metrics/types/
[2]: https://www.datadoghq.com/blog/engineering/computing-accurate-percentiles-with-ddsketch/
[3]: https://app.datadoghq.com/event/explorer