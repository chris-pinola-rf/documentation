---
algolia:
  subcategory: Marketplace インテグレーション
app_id: fairwinds-insights
app_uuid: a488d774-fd45-4765-b947-e48792c6ab32
assets:
  dashboards:
    Insights Overview: assets/dashboards/overview.json
  integration:
    auto_install: false
    configuration: {}
    events:
      creates_events: true
    metrics:
      check: fairwinds.insights.action_items
      metadata_path: metadata.csv
      prefix: fairwinds.insights.
    service_checks:
      metadata_path: assets/service_checks.json
    source_type_id: 10112
    source_type_name: Fairwinds Insights
author:
  homepage: https://www.fairwinds.com
  name: Fairwinds
  sales_email: datadog-marketplace@fairwinds.com
  support_email: insights@fairwinds.com
  vendor_id: fairwinds
categories:
- コンテナ
- コスト管理
- kubernetes
- マーケットプレイス
- プロビジョニング
- セキュリティ
custom_kind: integration
dependencies: []
display_on_public_website: true
draft: false
git_integration_title: fairwinds_insights
integration_id: fairwinds-insights
integration_title: Fairwinds Insights
integration_version: ''
is_public: true
legal_terms:
  eula: assets/eula.pdf
manifest_version: 2.0.0
name: fairwinds_insights
pricing:
- billing_type: tag_count
  includes_assets: true
  metric: datadog.marketplace.fairwinds.insights
  product_id: insights
  short_description: Kubernetes セキュリティ・ガバナンスソフトウェア
  tag: insights_node
  unit_label: Kubernetes ノード
  unit_price: 100
public_title: Fairwinds Insights
short_description: 業務の遂行に不可欠な Kubernetes アプリケーションを保護、最適化します
supported_os:
- linux
- windows
- macos
tile:
  changelog: CHANGELOG.md
  classifier_tags:
  - Category::Containers
  - Category::Cost Management
  - Category::Kubernetes
  - Category::Marketplace
  - Category::Provisioning
  - Category::Security
  - Offering::Integration
  - Supported OS::Linux
  - Supported OS::Windows
  - Supported OS::macOS
  - Submitted Data Type::Metrics
  - Submitted Data Type::Events
  configuration: README.md#Setup
  description: 業務の遂行に不可欠な Kubernetes アプリケーションを保護、最適化します
  media:
  - caption: Fairwinds Insights は、セキュリティアラート、ガードレール、コンプライアンスの知見とコスト最適化のアドバイスを提供する
      Kubernetes ガバナンスおよびセキュリティソフトウェアです。Fairwinds Insights は Datadog と統合するため、一箇所ですべてのレポートを確認することができます。
    image_url: images/Video_Front_Cover.png
    media_type: ビデオ
    vimeo_id: 619368230
  - caption: Fairwinds Insights Admission Controller は、新しいリソースがクラスターに追加されるたびに実行されます。リソースが組織のポリシーに違反している場合、Admission
      Controller はそれを拒否し、クライアントに通知します。
    image_url: images/Fairwinds_Insights_Admission_Controller_Image_v1.png
    media_type: image
  - caption: Fairwinds Insights は、複数のクラスターをセキュリティ設定に照らして継続的に監視し、リスクを低減してベストプラクティスが守られているかどうかを確認します。Insights
      は、コンテナと Kubernetes のリスクをピンポイントで特定し、リスクに優先順位を付け、修正ガイダンスとステータスの追跡を提供します。
    image_url: images/Fairwinds_Insights_Automate_Kubernetes_Policies_Image_v1.png
    media_type: image
  - caption: チームは、OPA を介してカスタマイズされたポリシーを構築し、施行することができ、CI/CD パイプライン、アドミッションコントローラー、クラスター内エージェントを含む
      Fairwinds Insights のあらゆる部分に統合することができます。Insights には、OPA テンプレートのライブラリが含まれています。
    image_url: images/Fairwinds_Insights_Customize_Open_Policy_Agent_Image_v1.png
    media_type: image
  - caption: Insights は、CPU とメモリの使用状況を監視し、リソースの制限や要求に関する推奨事項を提供します。Kubernetes のワークロードの
      CPU とメモリの使用効率を最大化します。
    image_url: images/Fairwinds_Insights_Optimize_Kubernetes_Resources_Image_v1.png
    media_type: image
  - caption: Fairwinds Insights は CI/CD パイプラインに緊密に統合され、セキュリティを左遷します。DevOps チームは、CI/CD
      を通じて設定ミスを防ぎ、手動で介入することなく、開発者に修正アドバイスを提供することができます。開発者はセーフティネットがある状態で自由に開発することができます。
    image_url: images/Fairwinds_Insights_Shift_Left_Security_Image_v1.png
    media_type: image
  - caption: Fairwinds Insights は、コンテナランタイムの監視と CI/CD プロセスへの統合を実現します。Insights は、コンテナ内の既知の脆弱性を追跡し、発見された脆弱性を重要度によって優先順位付けし、改善策を提供します。Insights
      は、チケッティングや割り当てワークフローと統合し、修復のステータスを追跡します。
    image_url: images/Fairwinds_Insights_VulnerabilityScanning_Image_v1.png
    media_type: image
  overview: README.md#Overview
  resources:
  - resource_type: blog
    url: https://www.datadoghq.com/blog/fairwinds-insights-datadog-marketplace/
  - resource_type: documentation
    url: https://insights.docs.fairwinds.com/
  support: README.md#Support
  title: Fairwinds Insights
  uninstallation: README.md#Uninstallation
---

<!--  SOURCED FROM https://github.com/DataDog/marketplace -->


## Overview

Software to protect and optimize your mission-critical Kubernetes applications

#### Streamline handoffs from development to operations

* Define and control custom policies across multiple clusters
* Enforce guardrails and best practices with an admission controller
* Integration container scanning and deployment validation into CI/CD workflows

#### Monitor and optimize Kubernetes costs

* Gain visibility into workload resource usage and estimated costs
* Determine the right CPU and memory settings for your workloads

#### Save time

* Integrate Kubernetes configuration recommendations with your existing Datadog dashboards
* Improve collaboration with Slack integration

#### Reduce risk

* Monitor containers for known vulnerabilities
* Validate Kubernetes deployment configurations

## Data Collected

### Metrics

Action Items from Fairwinds Insights will appear in Datadog with tags so that you can perform any analysis you need.

### Service Checks

Fairwinds Insights does not include any service checks.

### Events

* An initial Event will appear when you first setup the integration
* Event per new Action Item in Fairwinds Insights
* Event per event fixed Aciton Item in Fairwinds Insights

## Support

For support or requests, contact Fairwinds through the following channels:

- Phone: +1 617-202-3659 
- Email: [sales@fairwinds.com][2]

### Frequently Asked Questions

**How does Fairwinds Insights work?**

Fairwinds Insights provides a unified, multi-cluster view into three categories of Kubernetes configuration issues: security, efficiency, and reliability. Fairwinds Insights makes it easy to deploy multiple open-source tools through a single helm installation. This one-time install helps engineers avoid custom work for installing and configuring each tool. The software also adds policy management capabilities so engineering teams can define and enforce guardrails for deployments into Kubernetes clusters.

**What’s a plugin?**

Fairwinds Insights refers to the tools integrated with the software as ‘plugins.’

**What’s an Agent?**

Fairwinds Insights refers to the included helm chart as the ‘Fairwinds Insights Agent.’

**What happens to my data?**

Fairwinds Insights aggregates findings from each plugin and publishes it into a multi-cluster view for easy consumption, prioritization, and issue tracking.

**What plugins does Fairwinds Insights include?**

Fairwinds Insights provides integrations for a variety of great open source tools you use today, including [Polaris](https://github.com/FairwindsOps/polaris), [Goldilocks](https://github.com/FairwindsOps/goldilocks/), [Open Policy Agent](https://www.openpolicyagent.org/), and [Trivy Container Scanning](https://github.com/aquasecurity/trivy). For the complete list, please visit the [Fairwinds Insights documentation center](https://insights.docs.fairwinds.com/). Just a few of the example findings are listed below:

* Container vulnerabilities
* Security issues with Kubernetes deployments (e.g., deployments configured to run as root)
* Cluster-level weaknesses (e.g., exposed pods, information disclosures, etc.)
* Kubernetes CVEs
* Automated notification of Helm charts that are out of date
* Custom Kubernetes policies and configuration checks

### Refund Policy

Insights Cancellation and Refund Policy:

Fairwinds Insights is provided as a month-to-month subscription that you, the customer, may discontinue at any time in the ways made available to you through your DataDog Marketplace account. If you discontinue your subscription, you will be billed only for the remainder of the monthly billing period then in effect. Insights does not provide refunds of any fees already paid.

### Further Reading

Additional helpful documentation, links, and articles:

- [Monitor Kubernetes with Fairwinds Insights' offering in the Datadog Marketplace][2]
- [Fairwinds Insights Documentation][3]

[1]: https://insights.fairwinds.com
[2]: https://www.datadoghq.com/blog/fairwinds-insights-datadog-marketplace/
[3]: https://insights.docs.fairwinds.com/
---
This application is made available through the Marketplace and is supported by a Datadog Technology Partner. <a href="https://app.datadoghq.com/marketplace/app/fairwinds-insights" target="_blank">Click Here</a> to purchase this application.