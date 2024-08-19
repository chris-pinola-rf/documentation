---
app_id: Scylla
app_uuid: 1d655820-3010-4ae3-8273-a3798321d4d4
assets:
  dashboards:
    Scylla Overview: assets/dashboards/overview.json
  integration:
    auto_install: true
    configuration:
      spec: assets/configuration/spec.yaml
    events:
      creates_events: false
    metrics:
      check: scylla.node.operation_mode
      metadata_path: metadata.csv
      prefix: Scylla.
    service_checks:
      metadata_path: assets/service_checks.json
    source_type_id: 10087
    source_type_name: Scylla
  monitors:
    '[Scylla] Server is shutting down': assets/monitors/instance_down.json
author:
  homepage: https://www.datadoghq.com
  name: Datadog
  sales_email: info@datadoghq.com (日本語対応)
  support_email: help@datadoghq.com
categories:
- キャッシュ
- data stores
- ログの収集
custom_kind: integration
dependencies:
- https://github.com/DataDog/integrations-core/blob/master/scylla/README.md
display_on_public_website: true
draft: false
git_integration_title: Scylla
integration_id: Scylla
integration_title: Scylla
integration_version: 2.7.2
is_public: true
manifest_version: 2.0.0
name: Scylla
public_title: Scylla
short_description: クラスターのリソース、レイテンシー、健全性などを追跡
supported_os:
- linux
- windows
- macos
tile:
  changelog: CHANGELOG.md
  classifier_tags:
  - Category::Caching
  - Category::Data Stores
  - Category::Log Collection
  - Supported OS::Linux
  - Supported OS::Windows
  - Supported OS::macOS
  - Offering::Integration
  configuration: README.md#Setup
  description: クラスターのリソース、レイテンシー、健全性などを追跡
  media: []
  overview: README.md#Overview
  support: README.md#Support
  title: Scylla
---

<!--  SOURCED FROM https://github.com/DataDog/integrations-core -->


## Overview

This Datadog-[Scylla][1] integration collects a majority of the exposed metrics by default, with the ability to customize additional groups based on specific user needs.

Scylla is an open-source NoSQL data store that can act as "a drop-in Apache Cassandra alternative." It has rearchitected the Cassandra model tuned for modern hardware, reducing the size of required clusters while improving theoretical throughput and performance.

## Setup

Follow the instructions below to install and configure this check for an Agent running on a host.

### Installation

The Scylla check is included in the [Datadog Agent][2] package. No additional installation is needed on your server.

### Configuration

1. Scylla のパフォーマンスデータを収集するには、Agent の構成ディレクトリのルートにある `conf.d/` フォルダーの `scylla.d/conf.yaml` ファイルを編集します。使用可能なすべての構成オプションについては、[サンプル scylla.d/conf.yaml][3] を参照してください。以前にこのインテグレーションを実装したことがある場合は、[旧バージョンの例][4]を参照してください。

2. [Restart the Agent][5].

##### Log collection

Scylla には複数の出力モードがあり、実行中の環境に応じて異なります。アプリケーションによるログ生成の詳細は、[Scylla ドキュメント][6]を参照してください。

1. Collecting logs is disabled by default in the Datadog Agent, enable it in your `datadog.yaml` file:

      ```yaml
       logs_enabled: true
     ```

2. Uncomment and edit the logs configuration block in your `scylla.d/conf.yaml` file. Change the `type`, `path`, and `service` parameter values based on your environment. See the [sample scylla.d/conf.yaml][3] for all available configuration options.

      ```yaml
       logs:
         - type: file
           path: <LOG_FILE_PATH>
           source: scylla
           service: <SERVICE_NAME>
           #To handle multi line that starts with yyyy-mm-dd use the following pattern
           #log_processing_rules:
           #  - type: multi_line
           #    pattern: \d{4}\-(0?[1-9]|1[012])\-(0?[1-9]|[12][0-9]|3[01])
           #    name: new_log_start_with_date
     ```

3. [Restart the Agent][5].

To enable logs for Kubernetes environments, see [Kubernetes Log Collection][7].

### Validation

[Agent の status サブコマンドを実行][8]し、Checks セクションで `scylla` を探します。

## Data Collected

### Metrics
{{< get-metrics-from-git "scylla" >}}


### Events

The Scylla check does not include any events.

### Service Checks
{{< get-service-checks-from-git "scylla" >}}


## Troubleshooting

Need help? Contact [Datadog support][11].


[1]: https://scylladb.com
[2]: https://app.datadoghq.com/account/settings/agent/latest
[3]: https://github.com/DataDog/integrations-core/blob/master/scylla/datadog_checks/scylla/data/conf.yaml.example
[4]: https://github.com/DataDog/integrations-core/blob/7.50.x/scylla/datadog_checks/scylla/data/conf.yaml.example
[5]: https://docs.datadoghq.com/ja/agent/guide/agent-commands/#start-stop-and-restart-the-agent
[6]: https://docs.scylladb.com/getting-started/logging/
[7]: https://docs.datadoghq.com/ja/agent/kubernetes/log/
[8]: https://docs.datadoghq.com/ja/agent/guide/agent-commands/#agent-status-and-information
[9]: https://github.com/DataDog/integrations-core/blob/master/scylla/metadata.csv
[10]: https://github.com/DataDog/integrations-core/blob/master/scylla/assets/service_checks.json
[11]: https://docs.datadoghq.com/ja/help/