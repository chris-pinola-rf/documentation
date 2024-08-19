---
algolia:
  subcategory: Marketplace インテグレーション
app_id: insightfinder-insightfinder-license
app_uuid: 6f2fcb70-c087-412a-b221-360ba6a1c68f
assets:
  integration:
    auto_install: false
    configuration: {}
    events:
      creates_events: false
    service_checks:
      metadata_path: assets/service_checks.json
    source_type_id: 10299
    source_type_name: InsightFinder ライセンス
author:
  homepage: https://insightfinder.com/
  name: InsightFinder
  sales_email: info@insightfinder.com
  support_email: support@insightfinder.com
  vendor_id: insightfinder
categories:
- アラート設定
- マーケットプレイス
- notifications
- ai/ml
custom_kind: integration
dependencies: []
display_on_public_website: true
draft: false
git_integration_title: insightfinder_insightfinder
integration_id: insightfinder-insightfinder-license
integration_title: InsightFinder
integration_version: ''
is_public: true
legal_terms:
  eula: assets/eula.pdf
manifest_version: 2.0.0
name: insightfinder_insightfinder
pricing:
- billing_type: tag_count
  includes_assets: true
  metric: datadog.marketplace.insightfinder.insightfinder
  product_id: insightfinder
  short_description: AIOps が提供するインシデント予測・調査プラットフォーム
  tag: ノード
  unit_label: 監視されるノード
  unit_price: 10
public_title: InsightFinder
short_description: インシデント調査・予防のための人間中心型 AI プラットフォーム
supported_os:
- linux
- windows
- macos
tile:
  changelog: CHANGELOG.md
  classifier_tags:
  - Category::Alerting
  - Category::Marketplace
  - Category::Notifications
  - Category::AI/ML
  - Offering::Software License
  - Supported OS::Linux
  - Supported OS::Windows
  - Supported OS::macOS
  configuration: README.md#Setup
  description: インシデント調査・予防のための人間中心型 AI プラットフォーム
  media:
  - caption: インシデント予測・調査のための AIOps プラットフォーム「InsightFinder」。
    image_url: images/InsightFinder_healthview.png
    media_type: image
  - caption: 全体的な健全性を把握することができる AIOps プラットフォーム「InsightFinder」のダッシュボード。
    image_url: images/InsightFinder_dashboard.png
    media_type: image
  - caption: InsightFinder によるインシデント調査・アクション。
    image_url: images/InsightFinder_investigation.png
    media_type: image
  - caption: InsightFinder 予測。
    image_url: images/InsightFinder_prediction.png
    media_type: image
  - caption: InsightFinder メトリクス異常検出。
    image_url: images/InsightFinder_metric.png
    media_type: image
  - caption: InsightFinder ログ分析。
    image_url: images/InsightFinder_log.png
    media_type: image
  - caption: Datadog で入力された InsightFinder OOTB ダッシュボード。
    image_url: images/InsightFinder_dd_dashboard.png
    media_type: image
  overview: README.md#Overview
  resources:
  - resource_type: blog
    url: https://www.datadoghq.com/blog/resolve-incidents-faster-with-insightfinder/
  support: README.md#Support
  title: InsightFinder
  uninstallation: README.md#Uninstallation
---

<!--  SOURCED FROM https://github.com/DataDog/marketplace -->


## Overview
DevSecOps, DataOps, MLOps, IT operations, and SRE teams rely on [InsightFinder][1] as the one-stop AI intelligence engine to predict and prevent infrastructure, data, and security problems in complex modern IT architectures. Powered by unique patented capabilities for incident prediction, unsupervised machine learning, and pattern-driven auto-remediation, the InsightFinder platform continuously learns from machine data to identify and fix all problems before they impact business.

Customers gain value quickly, starting with an InsightFinder free trial and the company’s pre-built integrations with Datadog and other popular tools for DevSecOps, IT operations management (ITOM), and IT service management (ITSM).

## Support

For support or feature requests, contact InsightFinder through the following channels:

- Email: [support@insightfinder.com][4]
- Contact [Datadog Support][5]

### Further Reading

Additional helpful documentation, links, and articles:

- [Identify and resolve incidents faster with InsightFinder’s offering in the Datadog Marketplace][6]

[1]: https://insightfinder.com/
[2]: https://app.insightfinder.com/
[3]: https://insightfinder.com/datadog-integration/
[4]: mailto:support@insightfinder.com
[5]: https://docs.datadoghq.com/ja/help/
[6]: https://www.datadoghq.com/blog/resolve-incidents-faster-with-insightfinder/
---
This application is made available through the Marketplace and is supported by a Datadog Technology Partner. <a href="https://app.datadoghq.com/marketplace/app/insightfinder-insightfinder-license" target="_blank">Click Here</a> to purchase this application.