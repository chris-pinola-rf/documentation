---
aliases:
- /ja/continuous_integration/dora_metrics/setup/
further_reading:
- link: /dora_metrics/
  tag: Documentation
  text: Learn about DORA Metrics
title: DORA メトリクスのセットアップ
---

{{< site-region region="gov" >}}
<div class="alert alert-warning">DORA Metrics is not available in the selected site ({{< region-param key="dd_site_name" >}}) at this time.</div>
{{< /site-region >}}

<div class="alert alert-warning">DORA Metrics is in public beta.</div>

## Overview

The four DORA Metrics are calculated based on two types of events:

- [**デプロイメントイベント**][8]: 特定の環境でサービスに新しいデプロイメントが発生したことを示します。
- [**インシデントイベント**][9]: 特定の環境でサービスに新しい障害が発生したことを示します。

各イベントタイプは異なるデータソースに対応しています。

## データソースの構成

### デプロイメント
{{< whatsnext desc="デプロイメントイベントは、デプロイメントの頻度、変更リードタイム、変更失敗率を計算するために使用されます。デプロイイベントのデータソースをセットアップするには、それぞれのドキュメントを参照してください。" >}}
  {{< nextlink href="/dora_metrics/deployments/apm" >}}APM Deployment Tracking{{< /nextlink >}}
  {{< nextlink href="/dora_metrics/deployments/deployment_api" >}}Deployment Event API または datadog-ci CLI{{< /nextlink >}}
{{< /whatsnext >}}

### 障害
{{< whatsnext desc="障害イベントは、インシデントイベントを通して解釈され、変更失敗率や平均復旧時間の計算に使用されます。障害イベントのデータソースをセットアップするには、該当するドキュメントをご覧ください。">}}
  {{< nextlink href="/dora_metrics/failures/pagerduty" >}}PagerDuty{{< /nextlink >}}
  {{< nextlink href="/dora_metrics/failures/incident_api" >}}Incident Event API{{< /nextlink >}}
{{< /whatsnext >}}

## Limitations
- 最初にデータソースオプション (APM Deployment Tracking や PagerDuty など) を選択すると、DORA Metrics はその時点からデータを反映し始めます。ソース A からソース B に切り替え、その後ソース A に戻した場合、ソース A の履歴データは最初に選択された時点からのみ使用可能です。
- 同じサービスに対するデプロイやインシデントは、同じ秒には発生しません。

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[3]: /ja/dora_metrics/
[4]: /ja/service_management/events/explorer/
[5]: /ja/api/latest/metrics/#query-timeseries-points
[6]: /ja/api/latest/metrics/#query-timeseries-data-across-multiple-products
[7]: /ja/dora_metrics/data_collected/
[8]: /ja/dora_metrics/deployments/
[9]: /ja/dora_metrics/failures/
