---
categories:
- ログの収集
- security
custom_kind: integration
dependencies: []
description: Google Workspace Alert Center からログを収集します。
doc_link: https://docs.datadoghq.com/integrations/google_workspace_alert_center/
draft: false
further_reading:
- link: https://www.datadoghq.com/blog/google-workspace-monitoring
  tag: ブログ
  text: Datadog を使用して Google Workspace を監視する
git_integration_title: google_workspace_alert_center
has_logo: true
integration_id: ''
integration_title: Google Workspace Alert Center
integration_version: ''
is_public: true
manifest_version: '1.0'
name: google_workspace_alert_center
public_title: Google Workspace Alert Center
short_description: Google Workspace Alert Center からログを収集します。
team: web-integrations
version: '1.0'
---

<!--  SOURCED FROM https://github.com/DataDog/dogweb -->
## 概要

Alert Center は、Google Workspace 全体にわたる重要なセキュリティ関連の通知、アラート、アクションの包括的なビューを提供します。Google Workspace Alert Center を Datadog と統合すると、次のことが可能になります。

- [Datadog のログ製品][1]を使用してアラートログを表示およびパースする。
- Google Workspace ドメインの[イベント][3]に[モニター][2]を設定する。
- Datadogの[セキュリティプラットフォーム][4]を活用して、Google Workspace ドメインに対する脅威を監視および検知する。

## Setup

### Installation

Datadog Google Workspace Alert Center インテグレーションは、サービスアカウントを使用して Google と Datadog の間の API 接続を作成します。以下では、サービスアカウントを作成し、Datadog にサービスアカウント認証情報を提供して、自動的に API 呼び出しを開始するための手順を説明します。

**Note**: Ensure that you have enabled the [Google Workspace Alert Center API][5].

1. Follow the [service account creation and authorization instructions][6]. You need
   super-admin access in order to complete these steps. Note the location where you save the private key JSON file as part of that process. Delegate domain-wide authority to the service account as described, granting the `https://www.googleapis.com/auth/apps.alerts` scope in the process. 
 1. From the `Service account details` page in your GCP console, click the `Create Google Workspace Marketplace-Compatible OAuth Client` button at the bottom of the `Advanced settings` section.
2. Navigate to the [Datadog Google Workspace Alert Center Integration tile][7].
3. On the **Configuration** tab, select _Upload Private Key File_ to integrate this project
   with Datadog. Select the private key JSON file you saved in the first step.
4. Enter the Subject Email, which is the email address for a user or robot account with
   Alert Center access. Do not use the email address associated with the service account itself. 
   The integration impersonates this user when making API calls.

If you want to monitor multiple projects, you can repeat the process above to use multiple
   service accounts.

### Configuration

Custom tags can also be specified per project. These tags are added to every log event
for that project in Datadog.

### Results

Wait at least five minutes to see [logs][1] coming in under the source `google.workspace.alert.center`. You may have to wait
longer if your environment generates Alert Center alerts infrequently.

## Data Collected

### Metrics

This Google Workspace Alert Center does not include metrics data.

### Events

For the full list of log events, see the [Google Workspace Alert Center documentation][8].

### Service Checks

The Google Workspace Alert Center integration does not include any service checks.

## Troubleshooting

Need help? Contact [Datadog support][9].

## Further reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/logs/
[2]: /ja/monitors/monitor_types/
[3]: /ja/events/
[4]: /ja/security_platform/
[5]: https://developers.google.com/admin-sdk/alertcenter/reference/rest
[6]: https://developers.google.com/identity/protocols/oauth2/service-account
[7]: http://app.datadoghq.com/integrations/google-workspace-alert-center
[8]: https://support.google.com/a/answer/9104586?hl=en&ref_topic=9105077
[9]: https://docs.datadoghq.com/ja/help/