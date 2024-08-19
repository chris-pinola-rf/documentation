---
aliases:
- /ja/account_management/audit_logs/
further_reading:
- link: /account_management/audit_trail/events/
  tag: ドキュメント
  text: 監査証跡イベントについて
- link: /account_management/org_settings/
  tag: ドキュメント
  text: 組織設定について
- link: /data_security/pci_compliance/
  tag: ドキュメント
  text: PCI 準拠の Datadog 組織をセットアップする
- link: https://www.datadoghq.com/blog/compliance-governance-transparency-with-datadog-audit-trail/
  tag: ブログ
  text: Datadog Audit Trail で、チーム全体のコンプライアンス、ガバナンス、透明性を構築します
- link: https://www.datadoghq.com/blog/audit-trail-best-practices/
  tag: ブログ
  text: 監査証跡で重要な Datadog のアセットと構成を監視する
title: Datadog Audit Trail（監査証跡）
---

## Overview

As an administrator or security team member, you can use [Datadog Audit Trail][1] to see who is using Datadog within your organization and the context in which they are using Datadog. As an individual, you can see a stream of your own actions, too.

There are two types of events that can occur within an audit trail: **request events**, which translate all requests made to Datadog's API into customer records, or **product-specific events**.

For example, track **request events** so you can see what API calls led up to the event. Or, if you're an enterprise or billing admin, use audit trail events to track user events that change the state of your infrastructure.

In this circumstance, audit events are helpful when you want to know product-specific events such as:

  -  When someone changed the retention of an index because the log volume changed and, therefore, the monthly bill has changed.

  - Who modified processors or pipelines, and when they were modified, as a dashboard or monitor is now broken and needs to be fixed.

  - Who modified an exclusion filter because the indexing volume has increased or decreased and logs are unable to be found or your bill went up.

For security admins or InfoSec teams, audit trail events help with compliance checks and maintaining audit trails of who did what, and when, for your Datadog resources. For example, maintaining an audit trail:

- Of anytime someone updates or deletes critical dashboard, monitors, and other Datadog resources.

- For user logins, account, or role changes in your organization.

**Note**: See [PCI DSS Compliance][2] for information on setting up a PCI-compliant Datadog organization.

## Setup

Datadog Audit Trail を有効にするには、[組織設定][3]で *COMPLIANCE* の *Audit Trail Settings* を選択し、**Enable** ボタンをクリックします。

{{< img src="account_management/audit_logs/audit_trail_settings.png" alt="The Audit Trail Settings page showing it disabled" style="width:85%;">}}

To see who enabled Audit Trail:
1. [Events Explorer][4] に移動します。
2. Enter `Datadog Audit Trail was enabled by` in the search bar. You may have to select a wider time range to capture the event.
3. The most recent event with the title "A user enabled Datadog Audit Trail" shows who last enabled Audit Trail.

## Configuration


### Permissions
Audit Trail を有効または無効にできるのは、`Audit Trail Write` 権限を持つユーザーのみです。さらに、Audit Explorer で監査イベントを表示するには、`Audit Trail Read` 権限が必要です。

### Archiving

Archiving is an optional feature for Audit Trail. You can use archiving to write to Amazon S3, Google Cloud Storage, or Azure Storage and have your SIEM system read events from it. After creating or updating your archive configurations, it can take several minutes before the next archive upload is attempted. Events are uploaded to the archive every 15 minutes, so check back on your storage bucket in 15 minutes to make sure the archives are successfully being uploaded from your Datadog account.

Audit Trail のアーカイブを有効にするには、[組織設定][3]で *Compliance* の *Audit Trail Settings* を選択し、Archiving までスクロールダウンして Store Events のトグルボタンをクリックしてオンにします。

### Retention

Retaining events is an optional feature for Audit Trail. Scroll down to *Retention* and click the *Retain Audit Trail Events* toggle to enable.

The default retention period for an audit trail event is seven days. You can set a retention period between three and 90 days.

{{< img src="account_management/audit_logs/retention_period.png" alt="Audit Trail Retention setup in Datadog" style="width:80%;">}}

## Explore audit events

監査イベントを詳しく確認するには、[Audit Trail][1] セクションに移動します。このセクションは Datadog の[組織設定][3]からもアクセス可能です。 

{{< img src="account_management/audit_logs/audit_side_nav.png" alt="Audit Trail Settings in the Organization Settings menu" style="width:30%;">}}

Audit Trail イベントは、[Log Explorer][5] 内のログと同様の機能を備えています。

- Filter to inspect audit trail events by Event Names (Dashboards, Monitors, Authentication, and more), Authentication Attributes (Actor, API Key ID, User email, and more), `Status` (`Error`, `Warn`, `Info`), Method (`POST`, `GET`, `DELETE`), and other facets.

- Inspect related audit trail events by selecting an event and navigating to the event attributes tab. Select a specific attribute to filter by or exclude from your search, such as `http.method`, `usr.email`, `client.ip`, and more.

{{< img src="account_management/audit_logs/attributes.png" alt="Audit Trail in the Organization Settings menu" style="width:50%;">}}


### Saved views

Efficient troubleshooting requires your data to be in the proper scope to permit exploration, have access to visualization options to surface meaningful information, and have relevant facets listed to enable analysis. Troubleshooting is contextual, and Saved Views make it easier for you and your teammates to switch between different troubleshooting contexts. You can access Saved Views in the upper left corner of the Audit Trail explorer.

All saved views, that are not your default view, are shared across your organization:

* **Integration saved views** come out-of-the-box with Audit Trail. These views are read-only, and identified by the Datadog logo.
* **カスタム保存ビュー**はユーザーによって作成されます。組織内のユーザーなら誰でも編集可能です ([読み取り専用ユーザー][6]を除く)。また、作成者のアバターによって識別されます。エクスプローラーの現在のコンテンツから新しいカスタム保存ビューを作成するには、**Save** ボタンをクリックします。

At any moment, from the saved view entry in the Views panel:

* **Load** or **reload** a saved view.
* **Update** a saved view with the configuration of the current view.
* **Rename** or **delete** a saved view.
* **Share** a saved view through a short-link.
* **Star** (turn into a favorite) a saved view so that it appears on top of your saved view list, and is accessible directly from the navigation menu.

**注**: インテグレーションの保存ビューおよび[読み取り専用ユーザー][6]に対しては、更新、名前変更、および削除のアクションは無効になっています。


### Default view

{{< img src="logs/explorer/saved_views/default.png" alt="Default view" style="width:50%;" >}}

The default view feature allows you to set a default set of queries or filters that you always see when you first open the Audit Trail explorer. You can come back to your default view by opening the Views panel and clicking the reload button.

Your existing Audit Trail explorer view is your default saved view. This configuration is only accessible and viewable to you, and updating this configuration does not have any impact on your organization. You can **temporarily** override your default saved view by completing any action in the UI or by opening links to the Audit Trail explorer that embed a different configuration.

At any moment, from the default view entry in the Views panel:

* **Reload** your default view by clicking on the entry.
* **Update** your default view with the current parameters.
* **Reset** your default view to Datadog's defaults for a fresh restart.

### Notable Events

Notable events are a subset of audit events that show potential critical configuration changes that could impact billing or have security implications as identified by Datadog. This allows org admins to hone in on the most important events out of the many events generated, and without having to learn about all available events and their properties.

{{< img src="account_management/audit_logs/notable_events.png" alt="The audit event facet panel showing notable events checked" style="width:30%;">}}

Events that match the following queries are marked as notable.

| Description of audit event                                          | Query in audit explorer                           |
| ------------------------------------------------------------------- | --------------------------------------------------|
| Changes to log-based metrics | `@evt.name:"Log Management" @asset.type:"custom_metrics"` |
| Changes to Log Management index exclusion filters | `@evt.name:"Log Management" @asset.type:"exclusion_filter"` |
| Changes to Log Management indexes | `@evt.name:"Log Management" @asset.type:index` |
| Changes to APM retention filters | `@evt.name:APM @asset.type:retention_filter` |
| Changes to APM custom metrics | `@evt.name:APM @asset.type:custom_metrics` |
| Changes to metrics tags | `@evt.name:Metrics @asset.type:metric @action:(created OR modified)` |
| Creations and deletion of RUM applications | `@evt.name:"Real User Monitoring" @asset.type:real_user_monitoring_application @action:(created OR deleted)` |
| Changes to Sensitive Data Scanner scanning groups | `@evt.name:"Sensitive Data Scanner" @asset.type:sensitive_data_scanner_scanning_group` |
| Creation or deletion of Synthetic tests | `@evt.name:"Synthetics Monitoring" @asset.type:synthetics_test @action:(created OR deleted)` |

### Inspect Changes (Diff)

The Inspect Changes (Diff) tab in the audit event details panel compares the configuration changes that were made to what was previously set. It shows the changes made to dashboard, notebook, and monitor configurations, which are represented as JSON objects.

{{< img src="account_management/audit_logs/inspect_changes.png" alt="The audit event side panel showing the changes to a composite monitor configuration, where the text highlighted in green is what was changed and the text highlighted in red is what was removed." style="width:70%;">}}

## リファレンステーブルに基づく監査イベントのフィルター

<div class="alert alert-warning">リファレンステーブルはベータ版です。40,000 行を超えるリファレンステーブルは、イベントのフィルタリングには使用できません。リファレンステーブルの作成および管理方法の詳細については、<a href="https://docs.datadoghq.com/integrations/guide/reference-tables/">リファレンステーブルによるカスタムメタデータの追加</a>を参照してください。</div>

リファレンステーブルを使用すると、メタデータと監査イベントを組み合わせることができ、Datadog ユーザーの行動を調査するための詳細な情報を提供できます。リファレンステーブルに基づくクエリフィルターを追加して、検索クエリを実行します。この機能の有効化と管理の詳細については、[リファレンステーブル][2]ガイドを参照してください。

リファレンステーブルを使用したクエリフィルターを適用するには、クエリエディタ横の `+ Add` ボタンをクリックし、**Join with Reference Table** を選択します。以下の例では、認可されていない IP アドレスから Datadog にアクセスしているユーザーによって変更されたダッシュボードを検索するために、リファレンステーブルのクエリフィルターを使用しています。

{{< img src="account_management/audit_logs/reference_tables.png" alt="リファレンステーブル検索オプションがハイライトされた Datadog Audit Trail Explorer" border="true" popup="true" style="width:100%;" >}}

### API キー監査

<div class="alert alert-warning">API キー監査は非公開ベータ版です。</div>

ログ管理ユーザーは、Audit Trail を使用して API キーの使用状況を監査できます。API キー監査では、ログに `datadog.api_key_uuid` タグが付与されており、そのタグにはログ収集に使用された API キーの UUID が含まれています。この情報を基に、以下の点を判断します。
- 組織やテレメトリーソース全体での API キーの使用状況。
- API キーのローテーションと管理。

## Create a monitor

監査証跡イベントの特定のタイプや証跡の属性別にモニターを作成するには、[監査証跡モニターに関するドキュメント][7]をご参照ください。例: 特定のユーザーログが発生したときにトリガーするモニターを設定、ダッシュボードが削除されたとき用のモニターを設定など。

## Create a dashboard or a graph

Give more visual context to your audit trail events with dashboards. To create an audit dashboard:

1. Datadog で[新しいダッシュボード][8]を作成します。
2. 視覚化を選択します。監査イベントを[トップリスト][9]、[時系列][10]、[リスト][11]で視覚化することができます。
3. [データのグラフを作成][12]: 編集で、データソースとして *Audit Events* を選択し、クエリを作成します。監査イベントはカウント別に絞り込まれ、ファセット別にグループ化できます。ファセットと上限を選択します。
{{< img src="account_management/audit_logs/audit_graphing.png" alt="Set Audit Trail as a data source to graph your data" style="width:100%;">}}
4. Set your display preferences and give your graph a title. Click the *Save* button to create the dashboard.

## Create a scheduled report

Datadog Audit Trail allows you to send out audit analytics views as routinely scheduled emails. These reports are useful for regular monitoring of the Datadog platform usage. For example, you can choose to get a weekly report of the number of unique Datadog user logins by country. This query allows you to monitor anomalous login activity or receive automated insight on usage.

To export an audit analytics query as a report, create a timeseries, top list, or a table query and click **More...** > **Export as scheduled report** to start exporting your query as a scheduled report.

**注**: **List** ビューには、スケジュールされたレポートへのエクスポートオプションはありません。

{{< img src="account_management/audit_logs/scheduled_report_export.png" alt="Export as scheduled report function in the More... dropdown menu" style="width:90%;" >}}

1. Enter a name for the dashboard, which is created with the query widget. A new dashboard is created for every scheduled report. This dashboard can be referenced and changed later if you need to change the report content or schedule.
2. Schedule the email report by customizing the report frequency and time frame.
3. Add recipients that you want to send the email to.
4. Add any additional customized messages that needs to be part of the email report.
5. Click **Create Dashboard and Schedule Report**.

{{< img src="account_management/audit_logs/export_workflow.png" alt="Exporting a audit analytics view into a scheduled email" style="width:80%;" >}}

## Download Audit Events as CSV

Datadog Audit Trail では、最大 10 万件の監査イベントを CSV ファイルとしてローカルにダウンロードすることができます。これらのイベントは、ローカルで分析したり、別のツールにアップロードしてさらに分析したり、セキュリティおよびコンプライアンス演習の一環として適切なチームメンバーと共有したりすることができます。

監査イベントを CSV 形式でエクスポートするには
1. 興味のあるイベントをキャプチャする適切な検索クエリを実行します
2. Add event fields as columns in the view that you want as part of CSV
3. Click on Download as CSV
4. Select the number of events to export and export as CSV

## Out-of-the-box dashboard

Datadog Audit Trail には、インデックス保持の変更、ログパイプラインの変更、ダッシュボードの変更など、さまざまな監査イベントを表示する[すぐに使えるダッシュボード][13]が付属しています。このダッシュボードを複製して、監査ニーズに合わせてクエリや視覚化をカスタマイズすることができます。

{{< img src="account_management/audit_logs/audit_dashboard.png" alt="Audit Trail dashboard" style="width:100%;">}}

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: https://app.datadoghq.com/audit-trail
[2]: /ja/data_security/pci_compliance/
[3]: https://app.datadoghq.com/organization-settings/
[4]: https://app.datadoghq.com/event/explorer
[5]: /ja/logs/explorer/
[6]: https://docs.datadoghq.com/ja/account_management/rbac/permissions/?tab=ui#general-permissions
[7]: /ja/monitors/types/audit_trail/
[8]: /ja/dashboards/
[9]: /ja/dashboards/widgets/top_list/
[10]: /ja/dashboards/widgets/timeseries/
[11]: /ja/dashboards/widgets/list/
[12]: /ja/dashboards/querying/#define-the-metric/
[13]: https://app.datadoghq.com/dash/integration/30691/datadog-audit-trail-overview?from_ts=1652452436351&to_ts=1655130836351&live=true