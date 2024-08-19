---
app_id: amazon-privatelink
app_uuid: a61f0481-5b69-4c7b-9777-12d626f2486d
assets:
  dashboards:
    Amazon PrivateLink: assets/dashboards/amazon_privatelink_dashboard.json
  integration:
    auto_install: true
    events:
      creates_events: false
    metrics:
      check:
      - aws.privatelinkendpoints.active_connections
      metadata_path: metadata.csv
      prefix: aws.privatelink
    service_checks:
      metadata_path: assets/service_checks.json
    source_type_id: 360
    source_type_name: Amazon PrivateLink
author:
  homepage: https://www.datadoghq.com
  name: Datadog
  sales_email: info@datadoghq.com
  support_email: help@datadoghq.com
categories:
- metrics
- aws
- cloud
- log collection
- network
custom_kind: integration
dependencies: []
display_on_public_website: true
draft: false
git_integration_title: amazon_privatelink
integration_id: amazon-privatelink
integration_title: Amazon PrivateLink
integration_version: ''
is_public: true
manifest_version: 2.0.0
name: amazon_privatelink
public_title: Amazon PrivateLink
short_description: AWS PrivateLink のキーメトリクスを追跡します。
supported_os: []
tile:
  changelog: CHANGELOG.md
  classifier_tags:
  - Category::Metrics
  - Category::AWS
  - Category::Cloud
  - Category::Log Collection
  - Category::Network
  - Offering::Integration
  configuration: README.md#Setup
  description: Track key AWS PrivateLink metrics.
  media: []
  overview: README.md#Overview
  support: README.md#Support
  title: Amazon PrivateLink
---

<!--  SOURCED FROM https://github.com/DataDog/integrations-internal-core -->
## Overview

AWS PrivateLink provides private connectivity between VPCs, AWS services, and your on-premises networks.

Enable this integration to monitor the health and performance of your VPC endpoints or endpoint services with Datadog.

**Important:** If you would like to send telemetry data to Datadog through PrivateLink, follow [these instructions][1].

## Setup

### Installation

If you haven't already, set up the [Amazon Web Services integration][2] first.

### Metric collection

1. In the [AWS integration page][3], ensure that `PrivateLinkEndPoints` and `PrivateLinkServices` are enabled
   under the `Metric Collection` tab.
2. Install the [Datadog - AWS PrivateLink integration][4].

## Data Collected

### Metrics
{{< get-metrics-from-git "amazon_privatelink" >}}


### Events

The AWS PrivateLink integration does not include any events.

### Service Checks

The AWS PrivateLink integration does not include any service checks.

## Troubleshooting

Need help? Contact [Datadog support][6].

[1]: https://docs.datadoghq.com/ja/agent/guide/private-link/
[2]: https://docs.datadoghq.com/ja/integrations/amazon_web_services/
[3]: https://app.datadoghq.com/integrations/amazon-web-services
[4]: https://app.datadoghq.com/integrations/amazon-privatelink
[5]: https://github.com/DataDog/dogweb/blob/prod/integration/amazon_privatelink/amazon_privatelink_metadata.csv
[6]: https://docs.datadoghq.com/ja/help/