---
algolia:
  tags:
  - metrics without limits
aliases:
- /ja/metrics/faq/metrics-without-limits/
- /ja/metrics/guide/metrics-without-limits-getting-started/
further_reading:
- link: https://www.datadoghq.com/blog/metrics-without-limits
  tag: ブログ
  text: Metrics without LimitsTM でカスタムメトリクスのボリュームをダイナミックにコントロール
- link: https://dtdg.co/fe
  tag: Foundation Enablement
  text: インタラクティブなセッションに参加して、メトリクスの可能性を最大限に引き出しましょう
title: Metrics without LimitsTM
---

## Overview

Metrics without LimitsTM provides you flexibility and control over your custom metrics volumes by decoupling custom metric ingestion and indexing. You only pay for custom metric tags that are valuable to your organization.

Metrics without LimitsTM は、アプリ内ですべてのメトリクスタイプのタグを構成する機能を提供します。また、コードを再デプロイまたは変更することなく、カウント、レート、ゲージの集計をカスタマイズできます。Metrics without LimitsTM を使用すると、アプリ内でタグの許可リストを構成して、Datadog プラットフォーム全体でクエリ可能な状態を維持することができます。これは、アプリケーションレベルまたはビジネスメトリクス (例えば、`host`) に関連付けられた不要なタグを自動的に削除します。また、アプリ内でタグのブロックリストを構成して、タグを迅速に削除して除外することもできます。これらの構成機能は [Metrics Summary][1] ページにあります。

This page identifies key components of Metrics without LimitsTM that can help you manage your custom metrics volumes within your observability budget.

### Configuration of tags

#### タグの許可リスト
1. 任意のメトリクス名をクリックして、その詳細サイドパネルを開きます。
2. **Manage Tags** -> **"Include Tags..."** をクリックして、ダッシュボード、ノートブック、モニター、その他の Datadog 製品でクエリ可能にしておきたいタグを構成します。
3. タグの許可リストを定義します。
デフォルトでは、タグ構成モーダルには、過去 30 日間にダッシュボード、ノートブック、モニター、または API でアクティブにクエリされたタグの Datadog 推奨許可リストが事前に入力されます。推奨タグは、グラフ線アイコンで区別されます。
4. この潜在的なタグ構成から得られる、インデックス化されたカスタムメトリクスの*推定新規ボリューム*を確認します。
5. Click **Save**.

{{< img src="metrics/mwl_example_include_tags-compressed.mp4" alt="許可リストによるタグの構成" video=true style="width:100%" >}}

メトリクス API を通じて、タグの構成を[作成][2]、[編集][3]、[削除][4]、および[影響を見積もる][5]ことができます。

#### タグのブロックリスト
1. 任意のメトリクス名をクリックして、その詳細サイドパネルを開きます。
2. **Manage Tags** -> **"Exclude Tags..."** をクリックして、クエリしたくないタグを削除します。
3. タグのブロックリストを定義します。ブロックリストに定義されたタグは、ダッシュボードやモニターでクエリ**できません**。過去 30 日間にダッシュボード、ノートブック、モニター、API でアクティブにクエリされたタグは、グラフ線アイコンで区別されます。
5. この潜在的なタグ構成から得られる、インデックス化されたカスタムメトリクスの*推定新規ボリューム*を確認します。
6. Click **Save**.

{{< img src="metrics/mwl-example-tag-exclusion-compressed.mp4" alt="タグ除外によるタグの構成" video=true style="width:100%" >}}

メトリクス API でパラメーター `exclude_tags_mode: true` を設定して、タグのブロックリストを[作成][2]および[編集][3]します。

When configuring tags for counts, rates, and gauges, the most frequently queried time/space aggregation combination is available for query by default.

### Configure multiple metrics at a time

[一括メトリクスタグ構成機能][7]を使用して、カスタムメトリクスのボリュームを最適化します。メトリクスのネームスペースを指定するには、Metrics Summary の **Configure Tags** をクリックします。そのネームスペースのプレフィックスに一致するすべてのメトリクスを、**Include tags...** の下にある同じ許可リストまたは **Exclude tags...** の下にある同じブロックリストで構成できます。

API を介して、複数のメトリクスに対してタグを[構成][13]および[削除][14]できます。複数のメトリクスに対して[タグのブロックリストを構成][13]するには、API 上でパラメーター `exclude_tags_mode: true` を設定します。

### Refine and optimize your aggregations

You can further adjust your custom metrics filters by opting in to more [metrics aggregations][6] on your count, gauge, or rate metrics. To preserve the mathematical accuracy of your queries, Datadog only stores the most frequently queried time/space aggregation combination for a given metric type: 

- Configured counts and rates are queryable in time/space with SUM
- Configured gauges are queryable in time/space with AVG

You can add or remove aggregations at any time with no required Agent or code-level changes. 

The tag configuration modal pre-populates with an allowlist of aggregations that have been actively queried on dashboards, notebooks, monitors and through API in the past 30 days (colored in blue with an icon). You can also include your own additional aggregations.

## Metrics without LimitsTM billing

Configuring your tags and aggregations gives you control over which custom metrics can be queried -- ultimately reducing your billable count of custom metrics. Metrics without LimitsTM decouples ingestion costs from indexing costs. You can continue sending Datadog all of your data (everything is ingested) and you can specify an allowlist of tags you want to remain queryable in the Datadog platform. If the volume of data Datadog is ingesting for your configured metrics differs from the smaller, remaining volume you index, you can see two distinct volumes on your Usage page as well as the Metrics Summary page. 

- **Ingested Custom Metrics**: The original volume of custom metrics based on all ingested tags.
- **Indexed Custom Metrics**: The volume of custom metrics that remains queryable in the Datadog platform (based on any Metrics without LimitsTM configurations) 

**Note: Only configured metrics contribute to your Ingested custom metrics volume.** If a metric is not configured with Metrics without LimitsTM, you're only charged for its indexed custom metrics volume.

Learn more about [Custom Metrics Billing][8].

## Getting started with Metrics without LimitsTM

1. Configure your Top 20 metrics on your [Plan & Usage page][9] from the Metrics Summary page or by using the [API][2].
   You can use bulk metric configuration (`*` syntax) to quickly configure tags on multiple metrics. Datadog notifies you when the bulk configuration job is completed.

**Note:** If you're using the [Create Tag Configuration API][2], use the [tag configuration cardinality estimator API][5] first to validate the potential impact of your tag configurations prior to creating tag configurations. If the UI or the estimator API returns a resulting number of indexed that is larger than ingested, do not save your tag configuration.

2. Configure your unqueried metrics with empty tag configurations.

   As your teams continue cleaning up noisy metrics that are never queried in the Datadog platform, you can instantly minimize the costs of these unqueried metrics by configuring them with an empty allowlist of tags. 

   Ask your Customer Success Manager for an unqueried metrics report.

3. Review your usage and billing. After configuring your metrics, the impact of your changes can be validated in three ways: 

   - Prior to saving your configuration, the tag configuration cardinality estimator returns the estimated resulting number of indexed custom metrics which should be lower than your ingested custom metrics volumes.
   - After saving your configuration, the Metrics Summary details sidepanel should show that your indexed custom metrics are lower than your ingested custom metrics volume.
   - 24 hours after you've saved your configuration, you can also view the impact on your Plan & Usage page's **Top Custom Metrics** table. There should be reduction in the volume of custom metrics between the **Month-to-Date** tab and the **Most Recent Day** tab of this table.

## Best practices

- You can set up alerts on your real-time [estimated custom metrics usage][10] metric so that you can correlate spikes in custom metrics with configurations.

- [Role based access control][11] for Metrics without LimitsTM is also available to control which users have permissions to use this feature that has billing implications.

- Audit events allow you to track any tag configurations or percentile aggregations that have been made that may correlate with custom metrics spikes. Search for "tags:audit" and "queryable tag configuration" or "percentile aggregations" on your [Events Stream][12]

\*Metrics without Limits is a trademark of Datadog, Inc.

## Further reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: https://app.datadoghq.com/metric/summary
[2]: /ja/api/latest/metrics/#create-a-tag-configuration
[3]: /ja/api/latest/metrics/#update-a-tag-configuration
[4]: /ja/api/latest/metrics/#delete-a-tag-configuration
[5]: /ja/api/latest/metrics/#tag-configuration-cardinality-estimator
[6]: /ja/metrics/#time-and-space-aggregation
[7]: /ja/metrics/summary/#configuration-of-multiple-metrics
[8]: /ja/account_management/billing/custom_metrics/
[9]: https://app.datadoghq.com/billing/usage
[10]: /ja/account_management/billing/usage_metrics/
[11]: /ja/account_management/rbac/permissions/?tab=ui#metrics
[12]: https://app.datadoghq.com/event/stream
[13]: /ja/api/latest/metrics/#configure-tags-for-multiple-metrics
[14]: /ja/api/latest/metrics/#delete-tags-for-multiple-metrics