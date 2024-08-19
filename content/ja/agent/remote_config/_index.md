---
algolia:
  tags:
  - リモート構成
  - リモート構成
aliases:
- /ja/agent/guide/how_rc_works
- /ja/agent/guide/how_remote_config_works
further_reading:
- link: /security/application_security/how-appsec-works/#built-in-protection
  tag: Documentation
  text: アプリケーションセキュリティ モニタリングの仕組み
- link: /dynamic_instrumentation/?tab=configurationyaml#enable-remote-configuration
  tag: Documentation
  text: ダイナミックインスツルメンテーション
- link: /security/threats/setup
  tag: Documentation
  text: CSM Threats の設定
- link: https://www.datadoghq.com/blog/compliance-governance-transparency-with-datadog-audit-trail/
  tag: ブログ
  text: Datadog 監査証跡の使用
- link: https://www.datadoghq.com/blog/remote-configuration-for-datadog/
  tag: ブログ
  text: リモート構成で Datadog コンポーネントにリアルタイムの更新を適用
title: リモート構成
---

{{< site-region region="gov" >}}
<div class="alert alert-warning">選択した <a href="/getting_started/site">Datadog サイト</a> ({{< region-param key="dd_site_name" >}}) では Remote Configuration はサポートされていません。</div>
{{< /site-region >}}

## Overview
Remote Configuration is a Datadog capability that allows you to remotely configure and change the behavior of Datadog components (for example, Agents, tracing libraries, and Observability Pipelines Worker) deployed in your infrastructure, for select product features. Use Remote Configuration to apply configurations to Datadog components in your environment on demand, decreasing management costs, reducing friction between teams, and accelerating issue resolution times.

For Datadog security products, Application Security Management and Cloud Security Management Threats (CSM Threats), Remote Configuration-enabled Agents and compatible tracing libraries provide real-time security updates and responses, enhancing security posture for your applications and cloud infrastructure.

## How it works
Datadog Agent で Remote Configuration が有効になると、設定されている [Datadog サイト][1]を定期的にポーリングし、Remote Configuration が有効な Agent やトレーシングライブラリに適用すべき構成変更があるかどうかを判断します。

After you submit configuration changes in the respective Datadog product UI for a Remote Configuration-enabled product feature, the changes are stored in Datadog.

The following diagram illustrates how Remote Configuration works:

{{<img src="agent/remote_config/RC_Diagram_v5.png" alt="Users configure features in the UI, the config is stored in Datadog, the Agent requests config updates." width="90%" style="center">}}

1. You configure select product features in the Datadog UI.
2. The product feature configurations are securely stored within Datadog.
3. Agents in your environments securely poll, receive, and automatically apply configuration updates from Datadog. Tracing libraries, deployed in your environments, communicate with Agents to request and receive configuration updates from Datadog.

## 構成順序の優先順位
Fleet Automation に表示されるアクティブな構成では、優先順位の高いソースによって設定された構成が優先されます。

優先順位の高いソースから低いソースへの順序:

1. Remote Configuration
   **注**: リモート構成で適用された構成の変更は、ローカルコンフィギュレーションファイル (`datadog.yaml`) には表示されません。
2. Helm などのツールで設定された環境変数
3. ローカルまたは Ansible、Chef、Puppet などの構成管理ツールで管理されているコンフィギュレーションファイル (`datadog.yaml`)

優先順位の高いソースから発行された構成は、優先順位の低いソースから発行された構成をオーバーライドします。

## Supported products and feature capabilities
The following products and features are supported with Remote Configuration:

### Fleet Automation
**Datadog サイトから直接[フレアを送信][27]**します。ホストに直接アクセスすることなく、Datadog Agent をシームレスにトラブルシューティングします。

### Application Security Management (ASM)

- **1-click ASM activation**: Enable ASM in 1-click from the Datadog UI.
- **In-App attack patterns updates**: Receive the newest Web Application Firewall (WAF) attack patterns automatically as Datadog releases them, following newly disclosed vulnerabilities or attack vectors.
- **Protect**: Block attackers' IPs, authenticated users, and suspicious requests that are flagged in ASM Security Signals and Traces temporarily or permanently through the Datadog UI.

### Application Performance Monitoring (APM)

- **ランタイムの構成** (ベータ版): サービスを再起動することなく、[サービスカタログ][19]の UI 内からサービスのトレースサンプリングレート、ログ挿入の有効化、HTTP ヘッダータグを変更します。詳細については、[ランタイムの構成][22]を参照してください。
- **Remotely set Agent sampling rate** (Public Beta): Remotely configure the Datadog Agent to change its trace sampling rates and set rules to scale your organization's trace ingestion according to your needs, without needing to restart your Datadog Agent.


### Dynamic Instrumentation

- コードを変更することなく、ライブアプリケーションからクリティカルなメトリクス、トレース、ログを送信します。

### CSM Threats

- **Automatic default Agent rule updates**: Automatically receive and update the default Agent rules maintained by Datadog as new Agent detections and enhancements are released. See [Setting Up CSM Threats][3] for more information.
- **Automatic deployment of custom Agent rules**: Automatically deploy your custom Agent rules to designated hosts (all hosts or a defined subset of hosts).

### Observability Pipelines

- **[Observability Pipelines Worker][4] (OPW) をリモートでデプロイし、更新する**: Datadog UI でパイプラインを構築・編集し、環境内で稼働している OPW インスタンスに構成変更をロールアウトします。

## Security considerations

Datadog implements the following safeguards to protect the confidentiality, integrity, and availability of configurations received and applied by your Datadog components:

* Agents deployed in your infrastructure request configurations from Datadog.
* Datadog never sends configurations unless requested by Agents, and only sends configurations relevant to the requesting Agent.
* 構成リクエストは Agent から Datadog へ HTTPS (ポート 443) 経由で行われるため、ネットワークファイアウォールで追加のポートを開く必要はありません。
* The communication between your Agents and Datadog is encrypted using HTTPS, and is authenticated and authorized using your Datadog API key.
* Only users with the [`api_keys_write`][5] permissions are authorized to enable or disable Remote Configuration capability on the API key and use the supported product features.
* Datadog UI を通じて送信されたお客様の構成変更は、Agent とリクエスト元の Datadog コンポーネント上で署名および検証され、構成のインテグレーションが確認されます。

## Enabling Remote Configuration

### Prerequisites

- Datadog Agent バージョン `7.41.1` (APM サンプリングレートは `7.42.0`、APM Remote Instrumentation は `7.43.0`) 以上がホストまたはコンテナにインストールされていること。
- For Datadog products that use tracing libraries, you also need to upgrade your tracing libraries to a Remote Configuration-compatible version. For ASM Protection capabilities and ASM 1-click activation, see [ASM compatibility requirements][6]. For Dynamic Instrumentation, see [Dynamic Instrumentation prerequisites][20].

### Setup

To enable Remote Configuration:

1. Ensure your RBAC permissions include [`org_management`][7], so you can enable Remote Configuration for your organization.

2. Ensure your RBAC permissions include [`api_keys_write`][5], so you can create a new API key with the Remote Configuration capability, or add the capability to an existing API key. Contact your organization's Datadog administrator to update your permissions if you don't have it. A key with this capability allows you to authenticate and authorize your Agent to use Remote Configuration.

3. On the [Remote Configuration][8] page, enable Remote Configuration. This enables Datadog components across your organization to receive configurations from Datadog.
**注:** 2024 年 4 月 8 日以降、Remote Configuration は以下の場合にデフォルトでオンになります。
* すでに親組織レベルで Remote Configuration を有効にしている既存の Datadog のお客様が作成した新しい子組織で、**かつ**親組織と同じ Datadog サイト内にある組織。
* Datadog の新しいお客様によって作成された組織。

Remote Configuration の使用を停止するには、[停止セクション][23]を参照してください。

4. 既存の API キーを選択するか、新しい API キーを作成し、そのキーで Remote Configuration 機能を有効にします。新しい組織がステップ 3 の条件を満たしている場合、Remote Configuration はデフォルトで API キー上で有効になります。

   {{<img src="agent/remote_config/RC_Key_updated.png" alt="API Key properties with Remote Configuration capability Enable button." width="90%" style="center">}}

5. Update your Agent configuration file:
**注:** この手順は、Agent バージョン 7.46.0 以下でのみ必要です。Agent バージョン 7.47.0 以降では、`remote_configuration.enabled` は Agent 内でデフォルトで `true` に設定されています。Remote Configuration の使用を停止するには、[停止セクション][23]を参照してください。

{{< tabs >}}
{{% tab "Configuration YAML file" %}}
Add the following to your configuration YAML file, specifying the API key that has Remote Configuration capability enabled:
```yaml
api_key: xxx
remote_configuration:
  enabled: true
```

{{% /tab %}}
{{% tab "Environment variable" %}}
Add the following to your Datadog Agent manifest, specifying the API key that has Remote Configuration capability enabled:
```yaml
DD_API_KEY=xxx
DD_REMOTE_CONFIGURATION_ENABLED=true
```

{{% /tab %}}
{{% tab "Helm" %}}
Add the following to your Helm chart, specifying the API key that has Remote Configuration capability enabled:
```yaml
datadog:
  apiKey: xxx
remoteConfiguration:
  enabled: true
```

{{% /tab %}}
{{< /tabs >}}


6. 変更を有効にするために、Agent を再起動します。

After you perform these steps, your Agent requests its configuration from Datadog, and the features that use remote configuration are enabled:
- [CSM Threats default agent rules][9] update automatically as released.
- [APM Agent レベルのサンプリングレート][10]が適用されます。
- [Dynamic Instrumentation][11] is enabled.
- [ASM 1-Click enablement, IP blocking, and attack pattern updates][12] are enabled.

## Best practices

### Datadog Audit Trail

[Datadog Audit Trail][14] を使用して、組織のアクセスや Remote Configuration が有効なイベントを監視します。Audit Trail により、管理者やセキュリティチームは、Datadog API およびアプリケーションキーの作成、削除、および変更を追跡することができます。Audit Trail が構成されると、Remote Configuration が有効な機能に関連するイベントや、誰がこれらの変更をリクエストしたかを表示できます。Audit Trail により、イベントのシーケンスを再構築し、Remote Configuration の堅牢な Datadog モニタリングを確立することができます。

### Monitors

Configure [monitors][14] to receive notifications when an event of interest is encountered.

## Troubleshooting

If you experience issues using Remote Configuration, use the following troubleshooting guidelines. If you need further assistance, contact [Datadog support][15].

### Restart the Agent

Agent の構成が [`datadog.yaml`][16] ファイルで更新された後、この変更を有効にするために Agent を再起動します。

### Datadog Remote Configuration のエンドポイントが環境から到達可能であることを確認する

リモート構成を使用するには、環境にデプロイされた Agent と観測可能性パイプラインワーカーの両方が Datadog Remote Configuration [エンドポイント][17]に通信します。環境と Datadog 間のプライベートネットワーク接続のために、Remote Configuration Virtual Private Cloud [エンドポイント][25]に接続することもできます。アウトバウンド HTTPS が環境から Remote Configuration エンドポイントにアクセスできることを確認します。Datadog と環境の間にプロキシがある場合は、[プロキシ設定][18]を更新して Remote Configuration のエンドポイントを組み込んでください。

### Enable Remote Configuration at the organization level

Datadog UI の[組織][8]レベルでリモート構成を有効にするには、**Organization Settings** の [Remote Configuration Setup][26] ページに移動します。これにより、認可された Datadog コンポーネントが、サポートされている機能の構成やセキュリティ検出ルールを Datadog からリモートで受信できるようになります。組織レベルで Remote Configuration を有効にできるのは、[`org_management`][7] の RBAC 権限を持つユーザーのみです。

### Enable Remote Configuration on the API key

Agent が構成とセキュリティ検出ルールを受け取るための認証と認可、および Observability Pipelines Worker が構成を受け取るための許可を行うには、関連する API キーの Remote Configuration を有効にします。API キーの Remote Configuration を有効にできるのは、[`api_keys_write`][5] の RBAC 権限を持つユーザーのみです。

**Note:** If you have [`api_keys_write`][5] RBAC permission, but are missing Remote Configuration [Organization][8] level permissions, you cannot enable Remote Configuration on a new or an existing API Key. You only have permission to disable Remote Configuration on an existing API Key.

### Agent とトレーシングライブラリの Remote Configuration ステータスの確認

[Remote Configuration UI][8] から、Agent とトレーシングライブラリの Remote Configuration ステータスを確認できます。

以下の表は、Agent の各ステータスの意味を示しています。

  | Agent Status     | Description                                      |
  |------------------|--------------------------------------------------|
  | CONNECTED      | The Agent deployed in your environment is able to reach, authenticate, and authorize successfully to Datadog. This is the optimal state you want your Agents to be in for Remote Configuration.                                               |
  | UNAUTHORIZED          | The Agent deployed in your environment is able to reach Datadog but is not able to authenticate and authorize with Datadog for Remote Configuration operation. The most likely cause is the API Key used by the Agent is not Remote Configuration-enabled. To fix the issue, enable Remote Configuration capability on the API Key used by the Agent.                                                 |
  | CONNECTION ERROR        |   環境にデプロイされた Agent は、`datadog.yaml` コンフィギュレーションファイルで `remote_config.enabled` が true に設定されていますが、Remote Configuration サービスで Agent が見つかりません。最も考えられる原因は、Agent が Remote Configuration の[エンドポイント][17]に到達できないことです。この問題を解決するには、環境から Remote Configuration エンドポイントへの送信 HTTPS アクセスを許可します。このステータスは、Agent のバージョンが `7.45.0` 以上の場合に表示されます。
  | DISABLED       |   The Agent deployed in your environment has `remote_config.enabled` set to false in its `datadog.yaml` configuration file. Set `remote_config.enabled` to true if you want to enable Remote Configuration on the Agent. This status displays when the Agent version is `7.45.0` or higher. |
  | NOT CONNECTED       | The Agent cannot be found in the Remote Configuration service and could have `remote_config.enabled` set to true or false in its `datadog.yaml` configuration file. Check your local Agent configuration or your proxy settings. This status displays when the Agent version is higher than `7.41.1` but lower than `7.45.0`.            |
  | UNSUPPORTED AGENT   | The Agent is on a version that is not Remote Configuration capable. To fix this issue, update the Agent to the latest available version. |

以下の表は、トレーシングライブラリの各ステータスの意味を示しています。

  | トレーシングライブラリのステータス| Description                                      |
  |------------------|--------------------------------------------------|
  | CONNECTED      | トレーシングライブラリは、関連付けられた Agent を介して Remote Configuration サービスに正常に接続されています。これは、トレーシングライブラリが Remote Configuration に最適な状態です。                                               |
  | UNAUTHORIZED          | トレーシングライブラリは API キーに `Remote Config Read` 権限がない Agent に関連付けられています。この問題を解決するには、トレーシングライブラリに関連付けられている Agent が使用している API キーで Remote Configuration 機能を有効にする必要があります。|
  | CONNECTION ERROR        |   環境にデプロイされたトレーシングライブラリは、`datadog.yaml` コンフィギュレーションファイルで remote_config.enabled が true に設定された Agent と関連付けられていますが、Remote Configuration サービスで Agent が見つかりません。これの最も考えられる原因は、関連付けられた Agent が Remote Configuration の[エンドポイント][17]に到達できないことです。この問題を解決するには、環境から Remote Configuration エンドポイントへの送信 HTTPS アクセスを許可する必要があります。
  | DISABLED       |   環境にデプロイされたトレーシングライブラリは、`datadog.yaml` コンフィギュレーションファイルで `remote_config.enabled` が false に設定された Agent に関連付けられています。これは故意に設定されたか、間違って設定された可能性があります。関連付けられた Agent で Remote Configuration を有効にするには、`remote_config.enabled` を true に設定してください。  |
  | NOT CONNECTED       | Remote Configuration サービスでトレーシングライブラリが見つからず、`datadog.yaml` コンフィギュレーションファイルで `remote_config.enabled` が true または false に設定されている Agent に関連付けられます。ローカルの Agent の構成かプロキシ設定を確認してください。|
  | UNSUPPORTED AGENT   | トレーシングライブラリが、Remote Configuration ができない Agent に関連付けられています。この問題を解決するには、関連付けられた Agent のソフトウェアを最新のバージョンに更新してください。 |
  | NOT DETECTED   | トレーシングライブラリが Remote Configuration をサポートしていません。この問題を解決するには、トレーシングライブラリのソフトウェアを最新のバージョンに更新してください。 |
  | UNKNOWN   | トレーシングライブラリのステータスが不明で、Agent がトレーシングライブラリに関連付けられているかどうか判断できません。例えば、Agent が AWS Fargate のようなフルマネージドサーバーレスコンテナサービス上にデプロイされていることが考えられます。 |

## Remote Configuration の停止

Remote Configuration の使用を停止するには、組織レベルで Remote Configuration を無効にします。オプションで、API キーレベルと Agent レベルで Remote Configuration を無効にすることもできます。

### 組織レベル

[Remote Configuration][8] ページで組織レベルで Remote Configuration を無効にします。これにより、組織全体の Datadog コンポーネントが Datadog から構成を受信できなくなります。組織レベルで Remote Configuration を無効にするには、[`org_management`][7] 権限が必要です。

### API キーレベル
[API Keys][24] ページで選択した API キーを無効にします。API キーの Remote Configuration を無効にするには、[`api_keys_write`][5] 権限が必要です。

### Agent レベル
Starting with Agent version 7.47.0, `remote_configuration.enabled` is set to `true` by default in the Agent. This setting causes the Agent to request configuration updates from the Datadog site.

To receive configurations from Datadog, you also need to take the following steps:
- Enable Remote Configuration at the organization level.
- Enable Remote Configuration capability on your API Key from the Datadog UI.
- Allow outbound HTTPS access to Remote Configuration [endpoints][17] from your environment.

If you don't want your Agent to send configuration requests to Datadog, you can set `remote_configuration.enabled` to `false` in the Agent.

{{< tabs >}}
{{% tab "Configuration YAML file" %}}
Change `remote_configuration.enabled` from `true` to `false` in your [configuration YAML file][101]:
```yaml
remote_configuration:
  enabled: false
```

[101]: /ja/agent/configuration/agent-configuration-files/?tab=agentv6v7#agent-main-configuration-file
{{% /tab %}}
{{% tab "Environment variable" %}}
Add the following to your Datadog Agent manifest:
```yaml
DD_REMOTE_CONFIGURATION_ENABLED=false
```

{{% /tab %}}
{{% tab "Helm" %}}
Add the following to your Helm chart:
```yaml
datadog:
  remoteConfiguration:
    enabled: false
```

{{% /tab %}}
{{< /tabs >}}

## サポートされる環境

Remote Configuration は、Datadog Agent がデプロイされている環境で動作します。Remote Configuration は、AWS Fargate などのサーバーレスコンテナクラウドサービスをサポートしています。Remote Configuration は、サーバーレスコンテナ管理アプリ (AWS App Runner、Azure Container Apps、Google Cloud Run) や、コンテナパッケージングでデプロイされた関数 (AWS Lambda、Azure Functions、Google Cloud Functions) には対応していません。

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/getting_started/site/
[3]: /ja/security/threats/setup
[4]: /ja/observability_pipelines/#observability-pipelines-worker
[5]: /ja/account_management/rbac/permissions#api-and-application-keys
[6]: /ja/security/application_security/
[7]: /ja/account_management/rbac/permissions#access-management
[8]: https://app.datadoghq.com/organization-settings/remote-config
[9]: /ja/security/default_rules/#cat-workload-security
[10]: /ja/tracing/trace_pipeline/ingestion_controls/#managing-ingestion-for-all-services-at-the-agent-level
[11]: /ja/dynamic_instrumentation/?tab=configurationyaml#enable-remote-configuration
[12]: /ja/security/application_security/how-appsec-works/#built-in-protection
[13]: /ja/account_management/audit_trail
[14]: /ja/monitors/
[15]: /ja/help/
[16]: /ja/agent/remote_config/?tab=configurationyamlfile#setup
[17]: /ja/agent/configuration/network
[18]: /ja/agent/configuration/proxy/
[19]: /ja/tracing/service_catalog/
[20]: /ja/dynamic_instrumentation/?tab=configurationyaml#prerequisites
[21]: /ja/agent/configuration/agent-configuration-files/?tab=agentv6v7#agent-main-configuration-file
[22]: /ja/tracing/trace_collection/runtime_config/
[23]: /ja/agent/remote_config/?tab=configurationyamlfile#opting-out-of-remote-configuration-at-the-agent-level
[24]: https://app.datadoghq.com/organization-settings/api-keys
[25]: /ja/agent/guide/
[26]: https://app.datadoghq.com/organization-settings/remote-config/setup?page_id=org-enablement-step
[27]: /ja/agent/fleet_automation/#send-a-remote-flare