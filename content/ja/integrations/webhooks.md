---
categories:
- developer tools
- notifications
custom_kind: インテグレーション
dependencies: []
description: 「Datadog のアラートやイベントで任意の Webhook を通知チャンネルとして使用します。」
doc_link: https://docs.datadoghq.com/integrations/webhooks/
draft: false
further_reading:
- link: https://registry.terraform.io/providers/DataDog/datadog/latest/docs/resources/webhook
  tag: Terraform
  text: 「Terraform で Webhook を作成・管理します」
git_integration_title: Webhooks
has_logo: true
integration_id: ''
integration_title: Webhooks
integration_version: ''
is_public: true
manifest_version: '1.0'
name: Webhooks
public_title: 「Datadog-Webhooks インテグレーション」
short_description: 「Datadog のアラートやイベントで任意の Webhook を通知チャンネルとして使用します。」
version: '1.0'
---

<!--  SOURCED FROM https://github.com/DataDog/dogweb -->
## 概要

Webhook を使用して、以下のことができます。

-   ご使用のサービスに接続できます。
-   メトリクスアラートがトリガーされたときにサービスにアラートを送信できます。

## セットアップ

[Webhooks インテグレーションタイル][1]に移動して、使用する Webhook の URL と名前を入力します。

## 使用方法

Webhook を使用するには、Webhook をトリガーするメトリクスアラートのテキストに `@webhook-<WEBHOOK_NAME>` を追加します。これにより、以下の内容を JSON 形式で含む POST リクエストが、設定した URL に向けてトリガーされます。各リクエストのタイムアウトは 15 秒です。Datadog は、内部エラー (不正な形式の通知メッセージなど) が発生した場合、または Webhook エンドポイントから 5XX 応答を受け取った場合にのみ、再試行を発行します。失敗した接続は 5 回再試行されます。

**注**: カスタムヘッダーは JSON フォーマットである必要があります。

ペイロードフィールドに独自のペイロードを指定して、リクエストに独自のカスタムフィールドを追加することもできます。ペイロードを URL エンコードする場合は、**Encode as form** をオンにし、JSON 形式でペイロードを指定します。以下のセクションの変数を使用できます。

### 変数

$AGGREG_KEY
: 一緒に属するイベントを集約するための ID。<br />
**例**: `9bd4ac313a4d1e8fae2482df7b77628`

$ALERT_CYCLE_KEY
: アラートがトリガーした時点から解決するまでイベントにリンクする ID。

$ALERT_ID
: アラートの ID。<br />
**例**: `1234`

$ALERT_METRIC
: アラートの場合はメトリクスの名前。<br />
**例**: `system.load.1`

$ALERT_PRIORITY
: アラートモニターの優先度。<br />
**例**: `P1`、`P2`

$ALERT_QUERY
: Webhook をトリガーしたモニターのクエリ。

$ALERT_SCOPE
: アラートをトリガーしたタグのカンマ区切りリスト。<br />
**例**: `availability-zone:us-east-1a, role:computing-node`

$ALERT_STATUS
: アラートステータスの概要です。<br />
**例**: `system.load.1 over host:my-host was > 0 at least once during the last 1m`
**注**: Logs Monitor アラートからの Webhook ペイロードでこの変数を入力するには、Webhook インテグレーションタイルで `$ALERT_STATUS` を手動で追加する必要があります。

$ALERT_TITLE
: アラートのタイトル。<br />
**例**: `[Triggered on {host:ip-012345}] Host is Down`

$ALERT_TRANSITION
: アラート通知のタイプ。<br />
**例**: `Recovered`、`Triggered`/`Re-Triggered`、`No Data`/`Re-No Data`、`Warn`/`Re-Warn`、`Renotify`

$ALERT_TYPE
: アラートのタイプ。<br />
**例**: `error`、`warning`、`success`、`info`

$DATE
: イベントが発生した日付 _(epoch)_。<br />
**例**: `1406662672000`

$EMAIL
: Webhook をトリガーしたイベントをポストしたユーザーの電子メール。

$EVENT_MSG
: イベントのテキスト。<br />
**例**: `@webhook-url Sending to the webhook`

$EVENT_TITLE
: イベントのタイトル。<br />
**例**: `[Triggered] [Memory Alert]`

$EVENT_TYPE
: イベントのタイプ。<br />
[イベントタイプ](#event-types)の一覧は、[例](#examples)をご覧ください。

$HOSTNAME
: イベントに関連付けられたサーバーのホスト名 (ある場合)。

$ID
: イベントの ID。<br />
**例**: `1234567`

$INCIDENT_ATTACHMENTS
: インシデントの添付 (事後分析やドキュメントなど) のある JSON オブジェクトのリスト。<br />
**例**: `[{"attachment_type": "postmortem", "attachment": {"documentUrl": "https://app.datadoghq.com/notebook/123","title": "Postmortem IR-1"}}]` 

$INCIDENT_COMMANDER
: JSON オブジェクトとインシデントコマンダーのハンドル、uuid、名前、メール、およびアイコン

$INCIDENT_CUSTOMER_IMPACT
: インシデントの顧客への影響のステータス、期間、スコープを含む JSON オブジェクト。<br />
**例**: `{"customer_impacted": true, "customer_impact_duration": 300 ,"customer_impact_scope": "scope here"}`

$INCIDENT_FIELDS
: 各インシデントのフィールドを値にマッピングする JSON オブジェクト。<br />
**例**: `{"state": "active", "datacenter": ["eu1", "us1"]}`

$INCIDENT_INTEGRATIONS
: Slack や Jira など、インシデントのインテグレーションを持つ JSON オブジェクトのリスト。<br />
**例**: `[{"uuid": "11a15def-eb08-52c8-84cd-714e6651829b", "integration_type": 1, "status": 2, "metadata": {"channels": [{"channel_name": "#incident-1", "channel_id": "<channel_id>", "team_id": "<team_id>", "redirect_url": "<redirect_url>"}]}}]`

$INCIDENT_MSG
: インシデント通知のメッセージ。<br />

$INCIDENT_PUBLIC_ID
: 関連するインシデントのパブリック ID。<br />
**例**: `123`

$INCIDENT_SEVERITY
: インシデントの重大度。

$INCIDENT_STATUS
: インシデントのステータス。

$INCIDENT_TITLE
: インシデントのタイトル。

$INCIDENT_TODOS
: インシデントの修復タスクを持つ JSON オブジェクトのリスト。<br />
**例**: `[{"uuid": "01c03111-172a-50c7-8df3-d61e64b0e74b", "content": "task description", "due_date": "2022-12-02T05:00:00+00:00", "completed": "2022-12-01T20:15:00.112207+00:00", "assignees": []}]`

$INCIDENT_URL
: インシデントの URL。<br />
**例**: `https://app.datadoghq.com/incidents/1`

$INCIDENT_UUID
: 関連するインシデントの UUID。<br />
**例**: `01c03111-172a-50c7-8df3-d61e64b0e74b`

$LAST_UPDATED
: イベントが最後に更新された日付。

$LINK
: イベントの URL。<br />
**例**: `https://app.datadoghq.com/event/jump_to?event_id=123456`

$LOGS_SAMPLE
: ログモニターアラートからのログサンプルを含む JSON オブジェクト。サンプルメッセージの最大長は 500 文字です。<br />
**例**:<br />
: {{< code-block lang="json">}}
{
  "columns": [
    "Time",
    "Host"
  ],
  "label": "Sample Logs",
  "rows": [
    {
      "items": [
        "15:21:18 UTC",
        "web"
      ],
      "message": "[14/Feb/2023:15:21:18 +0000] \"GET / HTTP/1.1\" 200"
    },
    {
      "items": [
        "15:21:13 UTC",
        "web00"
      ],
      "message": "[14/Feb/2023:15:21:13 +0000] \"GET / HTTP/1.1\" 200"
    }
  ]
}
{{< /code-block >}}

$METRIC_NAMESPACE
: メトリクスがアラートの場合は、メトリクスのネームスペース

$ORG_ID
: オーガニゼーションの ID。<br />
**例**: `11023`

$ORG_NAME
: オーガニゼーションの名前。<br />
**例**: `Datadog`

$PRIORITY
: イベントの優先度。<br />
**例**: `normal` または `low`

$SECURITY_RULE_NAME
: セキュリティルールの名前。

$SECURITY_SIGNAL_ID
: シグナルの一意の識別子。<br />
**例**: `AAAAA-AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA`

$SECURITY_SIGNAL_SEVERITY
: セキュリティシグナルの重大度。<br />
**例**: `medium`

$SECURITY_SIGNAL_TITLE
: セキュリティシグナルのタイトル。

$SECURITY_SIGNAL_MSG
: セキュリティシグナルのメッセージ。

$SECURITY_SIGNAL_ATTRIBUTES
: セキュリティシグナルの属性。<br />
**例**: `{"network":{"client":{"ip":"1.2.3.4"}}, "service": ["agent"]}`

$SECURITY_RULE_ID
: セキュリティルール ID。<br />
**例**: `aaa-aaa-aaa`

$SECURITY_RULE_MATCHED_QUERIES
: セキュリティルールに関連するクエリ。<br />
**例**: `["@evt.name:authentication"]`

$SECURITY_RULE_GROUP_BY_FIELDS
: キーと値のペアによるセキュリティグループ。<br />
**例**: `{"@usr.name":"john.doe@your_domain.com"}`

$SECURITY_RULE_TYPE
: セキュリティルールの種類。<br />
**例**: `log_detection`

$SNAPSHOT
: イベントにスナップショットが含まれている場合の画像の URL。<br />
**例**: `https://p.datadoghq.com/path-to-snapshot`

$SYNTHETICS_TEST_NAME
: Synthetics テストの名前。

$SYNTHETICS_FIRST_FAILING_STEP_NAME 
: Synthetics テストの最初の失敗したステップの名前。

$SYNTHETICS_SUMMARY
: Summary of Synthetic test details.<br />
**Example**:
: {{< code-block lang="json">}}
{
  "result_id": "1871796423670117676",
  "test_type": "browser",
  "test_name": "Test name",
  "date": "Nov 05, 2021, 09:49AM UTC",
  "test_url": "https://app.datadoghq.com/synthetics/edit/apc-ki3-jwx",
  "result_url": "https://app.datadoghq.com/synthetics/details/anc-ki2-jwx?resultId=1871796423670117676",
  "location": "Frankfurt (AWS)",
  "browser": "Chrome",
  "device": "Laptop Large",
  "failing_steps": [
    {
      "error_message": "Error: Element's content should contain given value.",
      "name": "Test span #title content",
      "is_critical": true,
      "number": "3.1"
    }
  ]
}
{{< /code-block >}}

$TAGS
: Comma-separated list of the event tags.<br />
**Example**: `monitor, name:myService, role:computing-node`

$TAGS[key]
: Value of the `key` tag. If there is no `key` tag or the `key` tag has no value, this expression evaluates to an empty string.
**Example**: If `$TAGS` includes `role:computing-node`, then `$TAGS[role]` evaluates to `computing-node`

$TEXT_ONLY_MSG
: Text of the event without Markdown formatting.

$USER
: User posting the event that triggered the webhook.<br />
**Example**: `rudy`

$USERNAME
: Username of the user posting the event that triggered the webhook.

### Custom variables

In addition to the list of built-in variables, you can create your own custom ones in the integration tile. You can use these variables in webhook URLs, payloads, and custom headers. A common use case is storing credentials, like usernames and passwords.

You can also hide custom variable values for extra security. To hide a value, select the **hide from view** checkbox when you edit or add a custom variable:

{{< img src="/integrations/webhooks/webhook_hidefromview.png" alt="Hide from view checkbox masks custom variable values" style="width:100%;" >}}

### Authentication

#### HTTP Basic Authentication

If you want to post your webhooks to a service requiring authentication, you can use basic HTTP authentication by modifying your URL from `https://my.service.example.com` to `https://<USERNAME>:<PASSWORD>@my.service.example.com`.

#### OAuth 2.0 Authentication

If you want to post your webhooks to a service that requires OAuth 2.0 authentication, you can setup an Auth Method. An Auth Method includes all of the information required to obtain an OAuth token from your service. Once an Auth Method is configured and associated with a webhook, Datadog will handle obtaining the OAuth token, refreshing it if necessary, and adding it to the webhook request as a Bearer token.

To add an Auth Method, click the Auth Methods tab then click the New Auth Method button. Give the Auth Method a descriptive name, then enter the following information:

* Access Token URL
* Client ID
* Client Secret
* Scope (optional)
* Audience (optional)

Click Save to create the Auth Method. To apply this Auth Method to a webhook, go back to the Configuration tab and select an existing webhook configuration and click the Edit button. The Auth Method that you created should now appear in the Auth Method select list.

### Multiple webhooks

In a monitor alert, if 2 or more webhook endpoints are notified, then a webhook queue is created on a per service level. For instance, if you reach out to PagerDuty and Slack, a retry on the Slack webhook does not affect the PagerDuty one.

However, in the PagerDuty scope, certain events always go before others—specifically, an "Acknowledge" payload always goes before "Resolution". If an "Acknowledge" ping fails, the "Resolution" ping is queued due to the retry logic.

## Examples

### Sending SMS through Twilio

Use as URL:
`https://<ACCOUNT_ID>:<AUTH_TOKEN>@api.twilio.com/2010-04-01/Accounts/<ACCOUNT_ID>/Messages.json`

and as a payload:

```json
{
    "To": "+1347XXXXXXX",
    "From": "+1347XXXXXX",
    "Body": "$EVENT_TITLE \n Related Graph: $SNAPSHOT"
}
```

Replace `To` with your phone number and `From` with the one Twilio attributed to you. Check the **Encode as form** checkbox.

### Creating an issue in Jira

Use as URL:
`https://<JIRA_USER_NAME>:<JIRA_PASSWORD>@<YOUR_DOMAIN>.atlassian.net/rest/api/2/issue`

and as a payload:

```json
{
    "fields": {
        "project": {
            "key": "YOUR-PROJECT-KEY"
        },
        "issuetype": {
            "name": "Task"
        },
        "description": "There's an issue. See the graph: $SNAPSHOT and event: $LINK",
        "summary": "$EVENT_TITLE"
    }
}
```

Do not check the "Encode as form" checkbox.

### List of event types in the Webhooks payload {#event-types}

| Event Type | Associated Monitors |
| ---------  | ------------------- |
| `ci_pipelines_alert` | CI Pipelines |
| `ci_tests_alert` | CI Tests |
| `composite_monitor` | Composite |
| `error_tracking_alert` | Error Tracking |
| `event_alert` | Event using V1 endpoint |
| `event_v2_alert` | Event with V2 endpoint |
| `log_alert` | Logs |
| `monitor_slo_alert` | Monitor based SLO |
| `metric_slo_alert` | Metric based SLO |
| `outlier_monitor` | Outlier |
| `process_alert` | Process |
| `query_alert_monitor` | Metric, Anomaly, Forecast |
| `rum_alert` | RUM |
| `service_check` | Host, Service Check |
| `synthetics_alert` | Synthetics |
| `trace_analytics_alert` | Trace Analytics |

## Further reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: https://app.datadoghq.com/integrations/webhooks