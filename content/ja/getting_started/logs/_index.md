---
further_reading:
- link: https://learn.datadoghq.com/courses/intro-to-log-management
  tag: ラーニングセンター
  text: ログ管理の紹介
- link: https://learn.datadoghq.com/courses/going-deeper-with-logs-processing
  tag: ラーニングセンター
  text: ログ処理を極める
- link: https://learn.datadoghq.com/courses/log-indexes
  tag: Learning Center
  text: インデックス化ログボリュームの管理と監視
- link: https://learn.datadoghq.com/courses/log-pipelines
  tag: Learning Center
  text: ログパイプラインの構築と管理
- link: https://learn.datadoghq.com/courses/integration-pipelines
  tag: Learning Center
  text: インテグレーションパイプラインですぐに使えるログの処理
- link: /logs/log_collection/
  tag: Documentation
  text: Log Collection & Integrations
- link: /getting_started/tagging/unified_service_tagging
  tag: Documentation
  text: Learn how to configure unified service tagging
- link: https://dtdg.co/fe
  tag: Foundation Enablement
  text: ログ管理を最適化するためのインタラクティブセッションにご参加ください
title: ログの使用を開始する
---

## Overview

Use Datadog Log Management, also called logs, to collect logs across multiple logging sources, such as your server, container, cloud environment, application, or existing log processors and forwarders. With conventional logging, you have to choose which logs to analyze and retain to maintain cost-efficiency. With Datadog Logging without Limits*, you can collect, process, archive, explore, and monitor your logs without logging limits.

This page shows you how to get started with Log Management in Datadog. If you haven't already, create a [Datadog account][1].

## Configure a logging source

With Log Management, you can analyze and explore data in the Log Explorer, connect [Tracing][2] and [Metrics][3] to correlate valuable data across Datadog, and use ingested logs for Datadog [Cloud SIEM][4]. The lifecycle of a log within Datadog begins at ingestion from a logging source.

{{< img src="/getting_started/logs/getting-started-overview.png" alt="Different types of log configurations">}}

### Server

There are several [integrations][5] available to forward logs from your server to Datadog. Integrations use a log configuration block in their `conf.yaml` file, which is available in the `conf.d/` folder at the root of your Agent's configuration directory, to forward logs to Datadog from your server.

```yaml
logs:
  - type: file
    path: /path/to/your/integration/access.log
    source: integration_name
    service: integration_name
    sourcecategory: http_web_access
```

To begin collecting logs from a server:

1. If you haven't already, install the [Datadog Agent][6] based on your platform.

    **Note**: Log collection requires Datadog Agent v6+.

2. Collecting logs is **not enabled** by default in the Datadog Agent. To enable log collection, set `logs_enabled` to `true` in your `datadog.yaml` file.

    {{< agent-config type="log collection configuration" filename="datadog.yaml" collapsible="true">}}

3. Restart the [Datadog Agent][7].

4. Follow the integration [activation steps][8] or the custom files log collection steps on the Datadog site.

    **Note**: If you're collecting logs from custom files and need examples for tail files, TCP/UDP, journald, or Windows Events, see [Custom log collection][9].

### Container

As of Datadog Agent v6, the Agent can collect logs from containers. Each containerization service has specific configuration instructions based where the Agent is deployed or run, or how logs are routed.

For example, [Docker][10] has two different types of Agent installation available: on your host, where the Agent is external to the Docker environment, or deploying a containerized version of the Agent in your Docker environment.

[Kubernetes][11] requires that the Datadog Agent run in your Kubernetes cluster, and log collection can be configured using a DaemonSet spec, Helm chart, or with the Datadog Operator.

To begin collecting logs from a container service, follow the [in-app instructions][12].

### Cloud

You can forward logs from multiple cloud providers, such as AWS, Azure, and Google Cloud, to Datadog. Each cloud provider has its own set of configuration instructions.

For example, ​AWS service logs are usually stored in S3 buckets or CloudWatch Log groups. You can subscribe to these logs and forward them to an Amazon Kinesis stream to then forward them to one or multiple destinations. Datadog is one of the default destinations for Amazon Kinesis Delivery streams.​

To begin collecting logs from a cloud service, follow the [in-app instructions][13].

### Client

Datadog permits log collection from clients through SDKs or libraries. For example, use the `datadog-logs` SDK to send logs to Datadog from JavaScript clients.

To begin collecting logs from a client, follow the [in-app instructions][14].

### Other

If you're using existing logging services or utilities such as rsyslog, Fluentd, or Logstash, Datadog offers plugins and log forwarding options.

If you don't see your integration, you can type it in the *other integrations* box and get notifications for when the integration is available.

To begin collecting logs from a cloud service, follow the [in-app instructions][15].

## Explore your logs

Once a logging source is configured, your logs are available in the [Log Explorer][16]. This is where you can filter, aggregate, and visualize your logs.

例えば、あるサービスから流れてくるログをさらに調査するには、`service` でフィルタリングします。さらに、`ERROR` などの `status` などでフィルタリングし、[Group into Patterns][17] を選択すると、サービスのどの部分で最も多くのエラーが記録されているかを確認することができます。

{{< img src="/getting_started/logs/error-pattern-2024.png" alt="Log Explorer でのエラーパターンによるフィルタリング">}}

ログを `Fields` に集計し、**トップリスト**として可視化すると、上位のログサービスを確認することができます。`info` や `warn` のようなソースを選択し、ドロップダウンメニューから **View Logs** を選択します。サイドパネルにはエラーに基づくログが表示されるため、注意が必要なホストやサービスをすぐに確認することができます。

{{< img src="/getting_started/logs/top-list-view-2024.png" alt="Log Explorer のトップリスト">}}

## What's next?

Once a logging source is configured, and your logs are available in the Log Explorer, you can begin to explore a few other areas of log management.

### Log configuration

* Set [attributes and aliasing][18] to unify your logs environment.
* Control how your logs are processed with [pipelines][19] and [processors][20].
* Logging without Limits* では、ログの取り込みとインデックス処理を分離しているため、[ログを構成][21]して、[インデックス化][22]するログ、[保持][23]するログ、[アーカイブ][24]するログを選択することができます。

### Log correlation

* [Connect logs and traces][2] to exact logs associated with a specific `env`, `service,` or `version`.
* If you're already using metrics in Datadog, you can [correlate logs and metrics][3] to gain context of an issue.

### Guides

* [ログ管理のベストプラクティス][25]
* [Logging without Limits*][26] の詳細
* [RBAC 設定][27]による機密ログデータの管理

## Further reading

{{< partial name="whats-next/whats-next.html" >}}

<br>
*Logging without Limits is a trademark of Datadog, Inc.

[1]: https://www.datadoghq.com
[2]: /ja/tracing/other_telemetry/connect_logs_and_traces/
[3]: /ja/logs/guide/correlate-logs-with-metrics/
[4]: /ja/security/cloud_siem/
[5]: /ja/getting_started/integrations/
[6]: /ja/agent/
[7]: https://github.com/DataDog/datadog-agent/blob/main/docs/agent/changes.md#cli
[8]: https://app.datadoghq.com/logs/onboarding/server
[9]: /ja/agent/logs/?tab=tailfiles#custom-log-collection
[10]: /ja/agent/docker/log/?tab=containerinstallation
[11]: /ja/agent/kubernetes/log/?tab=daemonset
[12]: https://app.datadoghq.com/logs/onboarding/container
[13]: https://app.datadoghq.com/logs/onboarding/cloud
[14]: https://app.datadoghq.com/logs/onboarding/client
[15]: https://app.datadoghq.com/logs/onboarding/other
[16]: /ja/logs/explorer/
[17]: /ja/logs/explorer/analytics/patterns/
[18]: /ja/logs/log_configuration/attributes_naming_convention/
[19]: /ja/logs/log_configuration/pipelines/
[20]: /ja/logs/log_configuration/processors/
[21]: /ja/logs/log_configuration/
[22]: https://docs.datadoghq.com/ja/logs/log_configuration/indexes
[23]: https://docs.datadoghq.com/ja/logs/log_configuration/flex_logs
[24]: https://docs.datadoghq.com/ja/logs/log_configuration/archives
[25]: /ja/logs/guide/best-practices-for-log-management/
[26]: /ja/logs/guide/getting-started-lwl/
[27]: /ja/logs/guide/logs-rbac/