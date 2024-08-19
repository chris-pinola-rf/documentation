---
aliases:
- /ja/logs/security/
further_reading:
- link: /data_security/
  tag: ドキュメント
  text: Datadog に送信されるデータの主要カテゴリを確認する
- link: /data_security/pci_compliance/
  tag: ドキュメント
  text: PCI 準拠の Datadog 組織をセットアップする
- link: https://www.datadoghq.com/blog/datadog-pci-compliance-log-management-apm/
  tag: ブログ
  text: Datadog から PCI に準拠したログ管理と APM を発表
title: ログ管理のデータセキュリティ
---

<div class="alert alert-info">This page is about the security of data sent to Datadog. If you're looking for cloud and application security products and features, see the <a href="/security/" target="_blank">Security</a> section.</div>

The Log Management product supports multiple [environments and formats][1], allowing you to submit to Datadog nearly any data you choose. This article describes the main security guarantees and filtering controls available to you when submitting logs to Datadog.

**Note**: Logs can be viewed in various Datadog products. All logs viewed in the Datadog UI, including logs viewed in APM trace pages, are part of the Log Management product.

## Information security

The Datadog Agent submits logs to Datadog either through HTTPS or through TLS-encrypted TCP connection on port 10516, requiring outbound communication (see [Agent Transport for logs][2]).

Datadog uses symmetric encryption at rest (AES-256) for indexed logs. Indexed logs are deleted from the Datadog platform once their retention period, as defined by you, expires.

## Logs filtering

In version 6 or above, the Agent can be configured to filter logs sent by the Agent to the Datadog application. To prevent the submission of specific logs, use the `log_processing_rules` [setting][3], with the **exclude_at_match** or **include_at_match** `type`. This setting enables the creation of a list containing one or more regular expressions, which instructs the Agent to filter out logs based on the inclusion or exclusion rules supplied.

## Logs obfuscation

As of version 6, the Agent can be configured to obfuscate specific patterns within logs sent by the Agent to the Datadog application. To mask sensitive sequences within your logs, use the `log_processing_rules` [setting][4], with the  **mask_sequences** `type`. This setting enables the creation of a list containing one or more regular expressions, which instructs the Agent to redact sensitive data within your logs.

## HIPAA-enabled customers

{{% hipaa-customers %}}

## PCI DSS compliance for Log Management

{{< site-region region="us" >}}

<div class="alert alert-warning">
ログ管理における PCI DSS 準拠は、<a href="/getting_started/site/">US1 サイト</a>の Datadog 組織でのみ利用可能です。
</div>

Datadog allows customers to send logs to PCI DSS compliant Datadog organizations upon request. To set up a PCI-compliant Datadog org, follow these steps:

{{% pci-logs %}}

See [PCI DSS Compliance][1] for more information. To enable PCI compliance for APM, see [PCI DSS compliance for APM][1].

[1]: /ja/data_security/pci_compliance/
[2]: /ja/data_security/pci_compliance/?tab=apm

{{< /site-region >}}

{{< site-region region="us3,us5,eu,gov,ap1" >}}

PCI DSS compliance for Log Management is not available for the {{< region-param key="dd_site_name" >}} site.

{{< /site-region >}}

## エンドポイントの暗号化

すべてのログ送信エンドポイントは暗号化されています。以下のレガシーエンドポイントはまだサポートされています。

* `tcp-encrypted-intake.logs.datadoghq.com`
* `lambda-tcp-encrypted-intake.logs.datadoghq.com`
* `gcp-encrypted-intake.logs.datadoghq.com`
* `http-encrypted-intake.logs.datadoghq.com`

### Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/logs/log_collection/
[2]: /ja/agent/logs/log_transport
[3]: /ja/agent/logs/advanced_log_collection/#filter-logs
[4]: /ja/agent/logs/advanced_log_collection/#scrub-sensitive-data-from-your-logs
[5]: /ja/logs/explorer/#share-views
[6]: https://www.datadoghq.com/legal/hipaa-eligible-services/