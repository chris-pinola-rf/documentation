---
aliases:
- /ja/logs/processing
description: ログコンフィギュレーションページからログを処理、強化、制御、管理します
further_reading:
- link: /data_security/pci_compliance/
  tag: ドキュメント
  text: PCI 準拠の Datadog 組織をセットアップする
- link: https://www.datadoghq.com/blog/logging-without-limits/
  tag: ブログ
  text: Logging without Limits* の詳細
- link: https://www.datadoghq.com/blog/log-pipeline-scanner-datadog/
  tag: ブログ
  text: Datadog Log Pipeline Scanner でログ処理を調査する
- link: /logs/guide/
  tag: ガイド
  text: Datadog を使用したロギングに関する追加ガイド
title: ログコンフィギュレーション
---

## 概要

Datadog Logging without Limits* は、ログの取り込みとインデックス作成を切り離します。[**Logs > Pipelines**][1] のログ構成ページから、インデックスを作成して保持するログ、またはアーカイブするログを選択し、トップレベルで設定と制御を管理します。

**注**: PCI 準拠の Datadog 組織をセットアップするための情報は、[PCI DSS 準拠][2]をご覧ください。

## Configuration options

- Control how your logs are processed with [pipelines][3] and [processors][4].
- Set [attributes and aliasing][5] to unify your logs environment.
- [Generate metrics from ingested logs][6] as cost-efficient way to summarize log data from an entire ingested stream.
- Institute fine-grained control over your log management budget with [log indexes][7].
- Forward ingested logs to your own cloud-hosted storage bucket to keep as an [archive][8] for future troubleshooting or compliance audits.
- [Rehydrate an archive][9] to analyze or investigate log events that are older or excluded from indexing.
- Restrict [logs data access][10] with restriction queries.

## Log Explorer

Once you've completed configuration, start investigating and troubleshooting logs in the [Log Explorer][11].

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

<br>
*Logging without Limits is a trademark of Datadog, Inc.

[1]: https://app.datadoghq.com/logs/pipelines
[2]: /ja/data_security/pci_compliance/
[3]: /ja/logs/log_configuration/pipelines
[4]: /ja/logs/log_configuration/processors
[5]: /ja/logs/log_configuration/attributes_naming_convention/
[6]: /ja/logs/log_configuration/logs_to_metrics/
[7]: /ja/logs/log_configuration/indexes
[8]: /ja/logs/log_configuration/archives/
[9]: /ja/logs/log_configuration/rehydrating
[10]: /ja/logs/guide/logs-rbac/
[11]: /ja/logs/explorer/
