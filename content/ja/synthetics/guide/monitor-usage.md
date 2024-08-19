---
description: Synthetic テストで作成された推定使用量メトリクスについて説明します。
further_reading:
- link: https://www.datadoghq.com/blog/test-creation-best-practices/
  tag: ブログ
  text: エンドツーエンドテスト作成のベストプラクティス
- link: /synthetics/api_tests
  tag: ドキュメント
  text: API テストを作成する
- link: /synthetics/multistep
  tag: ドキュメント
  text: マルチステップ API テストを作成する
- link: /synthetics/browser_tests
  tag: Documentation
  text: ブラウザテストを作成する
title: 推定使用量メトリクスを使用する
---

## Overview 

Synthetic tests come with [estimated usage metrics][1] that allow you to keep track of your usage. These metrics notably enable you to:

* Understand how your usage evolves over time.
* Visualize which teams, applications, or services are contributing the most to your Synthetics usage.
* Alert on unexpected usage spikes that can impact your billing.

To visualize or alert on your Synthetics usage, use the following queries:

* [Single][2] and [Multistep API tests][3]: `sum:datadog.estimated_usage.synthetics.api_test_runs{*}.as_count()`

* [Browser tests][4]: `sum:datadog.estimated_usage.synthetics.browser_test_runs{*}.as_count()`.

For a higher level of refinement, scope or group these metrics by tags associated with your test, such as `team` or `application`. 

You can graph and monitor these metrics against static thresholds as well as use machine learning based algorithms like [anomaly detection][5] or [forecast][6] to ensure you do not get alerted for expected usage growth.

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/account_management/billing/usage_metrics/#types-of-usage
[2]: /ja/synthetics/api_tests
[3]: /ja/synthetics/multistep
[4]: /ja/synthetics/browser_tests
[5]: /ja/monitors/types/anomaly/
[6]: /ja/monitors/types/forecasts