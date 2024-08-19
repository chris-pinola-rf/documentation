---
app_id: pulumi
app_uuid: 7604c52b-dc07-4854-a5e4-799ab62798d8
assets:
  integration:
    auto_install: true
    configuration: {}
    events:
      creates_events: false
    metrics:
      check: []
      metadata_path: metadata.csv
      prefix: pulumi.
    service_checks:
      metadata_path: assets/service_checks.json
    source_type_id: 10220
    source_type_name: Pulumi
author:
  homepage: https://github.com/DataDog/integrations-extras
  name: Pulumi
  sales_email: team@pulumi.com
  support_email: team@pulumi.com
categories:
- AWS
- 自動化
- クラウド
- 構成 & デプロイ
- developer tools
- orchestration
- プロビジョニング
custom_kind: integration
dependencies:
- https://github.com/DataDog/integrations-extras/blob/master/pulumi/README.md
display_on_public_website: true
draft: false
git_integration_title: pulumi
integration_id: pulumi
integration_title: Pulumi
integration_version: ''
is_public: true
manifest_version: 2.0.0
name: pulumi
public_title: Pulumi
short_description: 好きなプログラミング言語を使って、あらゆるクラウドに対応する Infrastructure as Code を実現
supported_os:
- linux
- windows
- macos
tile:
  changelog: CHANGELOG.md
  classifier_tags:
  - Category::AWS
  - Category::Automation
  - Category::Cloud
  - Category::Configuration & Deployment
  - Category::Developer Tools
  - Category::Orchestration
  - Category::Provisioning
  - Supported OS::Linux
  - Supported OS::Windows
  - Supported OS::macOS
  - Offering::Integration
  configuration: README.md#Setup
  description: 好きなプログラミング言語を使って、あらゆるクラウドに対応する Infrastructure as Code を実現
  media: []
  overview: README.md#Overview
  support: README.md#Support
  title: Pulumi
---

<!--  SOURCED FROM https://github.com/DataDog/integrations-extras -->


## Overview

[Pulumi][1] is a modern infrastructure as code platform that enables cloud engineering teams to define, deploy, and manage cloud resources on any cloud using their favorite programming languages.

The Pulumi integration is used to provision any of the cloud resources available in Datadog. This integration must be configured with credentials to deploy and update resources in Datadog.

**注**: インテグレーションを変更するには、AWS IAM 権限を設定する必要があります。AWS IAM 権限の設定方法は、[AWS インテグレーションドキュメント][2]を参照してください。

## Setup

### Installation

[Pulumi と Datadog のインテグレーション][3]では、Datadog SDK を使用してリソースの管理およびプロビジョニングを行います。

### Configuration

1. [無料または商用 Pulumi アカウントに登録します][4]

2. [Pulumi をインストールします][5]

3. Once obtained, there are two ways to set your Datadog authorization tokens for Pulumi:


Set the environment variables `DATADOG_API_KEY` and `DATADOG_APP_KEY`:

```
export DATADOG_API_KEY=XXXXXXXXXXXXXX && export DATADOG_APP_KEY=YYYYYYYYYYYYYY
```

Or, set them using configuration if you prefer that they be stored alongside your Pulumi stack for easier multi-user access:

```
pulumi config set datadog:apiKey XXXXXXXXXXXXXX --secret && pulumi config set datadog:appKey YYYYYYYYYYYYYY --secret
```

**Note**: Pass `--secret` when setting `datadog:apiKey` and `datadog:appKey` so that they are properly encrypted.

4. `pulumi new` を実行してインフラストラクチャースタックのプロジェクトディレクトリを初期化し、[API ドキュメント][6]に従って新しいメトリクス、モニター、ダッシュボード、その他のリソースを定義します。

5. Once you have defined your cloud resources in code, run `pulumi up` to create the new resources defined in your Pulumi program. 

## Data Collected

### Metrics

Pulumi does not include any metrics.

### Service Checks

Pulumi does not include any service checks.

### Events

Pulumi does not include any events.

## Troubleshooting

Need help? Contact [Datadog support][7].

[1]: https://pulumi.com
[2]: https://docs.datadoghq.com/ja/integrations/amazon_web_services/?tab=roledelegation#aws-iam-permissions
[3]: https://www.pulumi.com/docs/intro/cloud-providers/datadog/
[4]: https://www.pulumi.com/pricing/
[5]: https://www.pulumi.com/docs/get-started/
[6]: https://www.pulumi.com/docs/reference/pkg/datadog/
[7]: https://docs.datadoghq.com/ja/help/