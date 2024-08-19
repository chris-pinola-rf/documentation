---
description: Synthetic の一般的なメトリクスをモニターで使用する方法について説明します。
further_reading:
- link: /monitors/types/metric/
  tag: ドキュメント
  text: メトリクスモニターについて
- link: /monitors/manage/
  tag: ドキュメント
  text: モニターの管理方法について
- link: /synthetics/metrics/
  tag: ドキュメント
  text: Synthetic モニタリングメトリクスについて
title: 推定使用量メトリクスを使用する
---

## Overview

You can use [metrics][1] generated from your Synthetic tests to create [metric monitors][2] in addition to the [Synthetic monitor created with your test][3].

{{< img src="synthetics/guide/using-synthetic-metrics/metric-monitor.png" alt="Example metric monitor that alerts when too many tests are failing in CI" style="width:95%;" >}}

With metric monitors, you can accomplish the following:

- Monitor the total response time
- Scope on specific HTTP timings such as DNS, the DNS resolution, and TCP connection
- Access tags added to metrics coming from Synthetic tests 

This guide demonstrates how to set up a metric monitor using a general metric such as `synthetics.test_runs`. 

## Create a metric monitor


{{< img src="synthetics/guide/using-synthetic-metrics/metric-monitor-setup.png" alt="Example metric monitor that alerts when too many tests are failing in CI" style="width:95%;" >}}

1. To create a metric monitor, navigate to [Monitors > New Monitor][4] and click **Metric**. 

2. Select a detection method to customize your monitor's alerting conditions. For this example, you can create a threshold alert metric monitor.

   Threshold Alert
   : An alert is triggered whenever a metric crosses a threshold.

   Change Alert
   : An alert is triggered when the delta between values is higher than the threshold.

   Anomaly Detection
   : An alert is triggered whenever a metric deviates from an expected pattern.

   Outliers Alert
   : An alert is triggered whenever one member in a group behaves differently from its peers.

   Forecast Alert
   : An alert is triggered whenever a metric is forecast to cross a threshold in the future.

3. In the **Define the metric** section, enter a Synthetic Monitoring metric such as `synthetics.test_runs`, where you can filter on status, response codes, and retry behavior.

4. Set the alerting conditions and add a notification message.

5. Set editing permissions and click **Create**.

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/synthetics/metrics/
[2]: /ja/monitors/types/metric/
[3]: /ja/synthetics/guide/synthetic-test-monitors/
[4]: https://app.datadoghq.com/monitors/create/metric