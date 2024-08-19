---
aliases:
- /ja/integrations/hipchat/
categories:
- collaboration
- notifications
custom_kind: integration
dependencies: []
description: Send Datadog alerts and graphs to your team's Slack channel.
doc_link: https://docs.datadoghq.com/integrations/slack/
draft: false
git_integration_title: slack
has_logo: true
integration_id: ''
integration_title: Slack
integration_version: ''
is_public: true
manifest_version: '1.0'
name: slack
public_title: Datadog-Slack インテグレーション
short_description: Datadog のアラートとグラフをチームの Slack チャンネルに送信。
version: '1.0'
---

<!--  SOURCED FROM https://github.com/DataDog/dogweb -->
## 概要

Slack を Datadog に接続し、次の方法でチームのコラボレーションを支援します。

- Slack のプライベートチャンネルまたはパブリックチャンネルでグラフを共有できます。
- Slack 内で Datadog からのアラートや通知を受けることができます。
- トリガーとなるモニターをミュートし、Slack からインシデントを宣言できます。
- ログイベント、トレース、ダッシュボードウィジェットのプレビューを表示するリンクを自動的に展開します。

## Setup

{{< tabs >}}

{{% tab "Datadog for Slack" %}}

{{% site-region region="gov" %}}
<div class="alert alert-warning">
    <a href="https://www.datadoghq.com/blog/datadog-slack-app/">Datadog for Slack</a> は {{< region-param key="dd_site_name" >}} サイトでは利用できません。US1-FED サイトの Slack に通知を送信するには、Slack webhook (レガシー) を使用してください。
</div>
{{% /site-region %}}

### Datadog アプリを Slack ワークスペースにインストールします。

1. Slack の[インテグレーションタイル][1]で、**Configuration** をクリックし、次に **Connect Slack Account** をクリックします。
2. Datadog に Slack ワークスペースへのアクセス権限を付与するには、**Allow** をクリックしてください。この変更を承認するには、Slack ワークスペースの管理者が必要になる場合があります。アプリの権限の内訳とそのリクエストの理由については、[権限][2]を参照してください。

### 通知を受信できる Slack チャンネルの構成

1. Datadog からの通知を受信する Slack チャンネルを構成するには、[Slack インテグレーションタイル][1]を使用してください。
2. **プライベートチャンネル**を構成するには、Datadog アプリがそのチャンネルのメンバーである必要があります。Slack でチャンネルに移動し、`/invite @Datadog` を使用して、Datadog アプリをメンバーに追加してください。このステップが完了すると、チャンネルは自動的に [Slack インテグレーションタイル][1]に追加されます。

Slack インテグレーションがインストールされると、任意の Slack チャンネルで `/datadog` コマンドを使用できるようになります。利用できるアクションはチャンネルによって異なります。利用可能なコマンドをすべて表示するには `/datadog help` を使用してください。また、`/dd` エイリアスを使って `/datadog` コマンドを実行することも可能です。


[1]: https://app.datadoghq.com/integrations/slack
[2]: https://docs.datadoghq.com/ja/integrations/slack#permissions
{{% /tab %}}

{{% tab "Slack Webhook (Legacy)" %}}

### Installation

Datadog サイトの [Slack インテグレーションタイル][1]を使用してインテグレーションをインストールします。

### Configuration

1. In your Slack account, go to the [Datadog (Legacy) app][2].
2. Click **Install** > **Add Integration**, then copy the Slack **Webhook URL**.
3. [Slack インテグレーションタイル][1]で、**Configuration** をクリックし、**Add Account** をクリックします。
4. Enter a **Slack Account Name** of your choice.
5. Paste the webhook URL in the **Slack Account Hook** field.
6. Click **Save**.
7. Add your Slack **Channels to post to**:
  {{< img src="integrations/slack/slack_configuration.png" alt="Slack configuration" >}}

また、[モニター][3]や[イベント][4]から Slack に通知を送ることもできます。


[1]: https://app.datadoghq.com/integrations/slack
[2]: https://slack.com/apps/A0F7XDT7F-datadog-legacy
[3]: https://docs.datadoghq.com/ja/monitors/notifications/
[4]: https://docs.datadoghq.com/ja/service_management/events/explorer/notifications/
{{% /tab %}}
{{< /tabs >}}

## Monitors

With the Slack integration, you can receive monitor alerts and mute monitors directly from Slack. For detailed instructions on how to create monitors, see [Configuring Monitors][1]. To send monitor alerts to a Slack channel, invite Datadog to the channel first using the `/invite @Datadog` command.

### Notification messages

You can use the same rules, variables, and tags as standard Datadog [Notifications][2]. For example, this notification pings a team in a Slack channel called `infrastructure` when a monitor is renotifying:

```
CPU usage has exceeded {{warn_threshold}} on {{ @machine_id.name }}.
{{#is_renotify}}
Notifying @slack-infrastructure <!subteam^12345>
{{/is_renotify}}
```

#### Channels

To specify a Slack channel when configuring a notification message, type `@slack` in the monitor message box to see the available list of channels you can send the notification to.

**Note**: Trailing special characters in a channel name are not supported for Slack @-notifications. For example, `@----critical_alerts` works, but `@--critical_alerts--` does not.

#### @-mentions

Use the following commands to create @-mentions in notification messages:

*@users**
: コマンド: `<@username>`
: Slack のユーザー名を使って Slack ユーザーに通知します。ユーザー名は [Slack アカウント設定][3]の **Username** にあります。例: `@slack-SLACK_CHANNEL <@USERNAME>`、または `@slack-SLACK_ACCOUNT-SLACK_CHANNEL <@USERNAME>`

**@here**
: コマンド: `<!here>`
: アラートが送信されているチャンネルに参加しているすべてのオンラインメンバーに通知します。

**@channel**
: コマンド: `<!channel>`
: アラートが送信されているチャンネルに参加しているすべてのメンバーに通知します。

**@usergroups**
: コマンド: `<!subteam^GROUP_ID>`
: Slack のユーザーグループに属するメンバー全員に通知します。例えば、ID が `12345` のユーザーグループには `<!subteam^12345>` を使います。`GROUP_ID` を見つけるには、**More** > **Your organization** > **People** > **User groups** に移動します。ユーザーグループを選択し、省略記号をクリックして、**Copy group ID** を選択します。また、[`usergroups.list` API エンドポイントをクエリする][4]こともできます。

You can also use message template variables to dynamically build @-mentions. For example, if the rendered variable corresponds to a specific channel in Slack:

- `@slack-{{owner.name}}` sends notifications to the **#owner.name**'s channel.
- `@slack-{{host.name}}` sends notifications to the **#host.name** channel.

To create @-mentions that go to specific email addresses:

- `@team-{{team.name}}@company.com` sends an email to the team's mailing list.

### Monitor alerts in Slack

When a monitor alert is sent a Slack channel, it contains several fields:

- The notification message
- A snapshot (graph) of the query that triggered your monitor
- Related tags
- The names of the users or groups that were notified

Slack のモニターアラートメッセージに含まれるコンテンツをカスタマイズするには、[Slack インテグレーションタイル][5]に移動します。チャンネルごとに、各モニターアラートオプションのチェックボックスを選択またはクリアします。

{{< img src="integrations/slack/slack_monitor_options.png" alt="Slack インテグレーションタイルのモニターアラートメッセージオプション" style="width:90%;" >}}

### Migrate monitors from Slack Webhook (Legacy) to Datadog for Slack

If your monitors are using the legacy Slack webhooks, there are two ways you can update your monitors to be sent from the Slack app:

- **一括アップグレード**: [Slack インテグレーションタイル][5]の各 Slack アカウントの構成の上部にある **Upgrade** ボタンをクリックして、すべてのモニターを一括アップグレードします。
- **個別のアップグレード**: [Slack インテグレーションタイル][5]の新しい構成にチャンネルを手動で追加します。同じチャンネルへの重複参照を削除する必要があるかもしれません。

## Dashboards

You can post dashboard widget snapshots to any Slack channel. For a list of supported widgets, see [Scheduled Reports][6].

To share a dashboard widget in Slack:

- In Datadog, hover over a dashboard widget and press `CMD + C` or `CTRL + C`, or click the  **Copy** button from the share menu, and then paste the link into Slack.
- In a Slack channel, send the `/datadog dashboard` or `/datadog` command, and then click the **Share Dashboard Widget** button.

**Note:** Slack recently introduced a new version of Workflow Builder that does not yet support third-party app integrations including Datadog.

{{< img src="integrations/slack/dashboard_share.mp4" alt="Sharing a dashboard widget in Slack" video="true" width=90% >}}

## Home Tab

Use the **Home** tab on the Datadog App in Slack to view your starred dashboards, notebooks, and services. You can also view a list of monitors that were triggered in the past 24 hours and their associated Slack channels. If you're a member of more than one Datadog account, filter the tab by switching between accounts.

{{< img src="integrations/slack/datadog_homepage.mp4" alt="Datadog homepage starring items and seeing triggered monitors" video="true" width=90% >}}

## Incidents

Anyone in your Slack org can declare an incident, regardless of whether they have access to Datadog. When a new incident is created, a corresponding Slack channel `#incident-(unique number ID)` is created, and a message is sent to the channel telling you the new incident channel to use. The channel topic changes with the incident.

### Incident commands

To declare a new incident from Slack:

```
/datadog incident 
```

To update the incident state (such as severity):

```
/datadog incident update
```

To list all open (active and stable) incidents:

```
/datadog incident list
```

To send the message to the Incident Timeline, use the message actions command (the three vertical dots that appear hovering over a message sent in an #incident channel).

{{< img src="integrations/slack/incidents2.png" alt="Slack configuration" style="width:60%;">}}

### Global incident updates channel

A global incident updates channel provides your team with organization-wide visibility into the status of all incidents directly from your Slack workspace. Select which channel in your workspace to post these updates to, and the channel receives the following posts: 

- Newly declared incidents.
- Changes to severity, status transition, and incident commander.
- アプリ内の[インシデント][7]の概要ページへのリンク。
- Link to join dedicated incident Slack channels.

To set up a global incident updates channel:

1. Datadog で、[**Incidents** > **Settings** > **Integrations**][8] ページに移動します。
2. Slack セクションで、**Send all incident updates to a global channel** (すべてのインシデント更新をグローバルチャンネルに送信する) トグルをクリックします。
3. Select the Slack workspace and Slack channel where you want the incident updates to be posted.

#### Manage incident tasks

By using Slack actions and the `/datadog` Slack commands, you can create and manage incident tasks directly from Slack. Incident task commands must be used in an incident channel.

##### Slack actions

To create a task using Slack actions, hover over any message sent in an incident channel. On hover, three dots appear to the right of the message, allowing you to **Add Task to Incident**.

##### Slack commands

To create a task for an incident, use the `/datadog task` command. A modal appears that allows you to include a description of the task, assign teammates, and set a due date. 

To show a list of all tasks created for the incident, use the `/datadog task list` command. Use this list to mark tasks as complete or reopen them.

作成されたすべてのタスクは、インシデントの **Remediation** タブで管理できます。詳細については、[Incident Management][9] を参照してください。

## Permissions

Datadog for Slack requires the following OAuth Scopes. See the [Slack permission scopes documentation][10] for more information.

### Bot Token Scopes

| Scopes                   | Request Reason                                                                                                 |
|--------------------------|----------------------------------------------------------------------------------------------------------------|
| `channels:join`          | Automatically join public channels configured in the Slack integration tile in Datadog.                        |
| `channels:manage`        | Create channels to manage and remediate incidents using Datadog Incident Management.                           |
| `channels:read`          | Provides channel name auto-complete suggestions in the Slack integration tile in Datadog.                      |
| `chat:write`             | Receive Datadog alerts and notifications in approved channels and conversations.                               |
| `commands`               | Enables the /datadog command, and its /dd alias, to perform actions in Datadog.                                |
| `groups:read`            | Provides channel name auto-complete suggestions for private channels in the Slack integration tile in Datadog. |
| `im:history`             | Allows Datadog to send messages to you in the Messages tab, for example, onboarding instructions.              |
| `im:read`                | Enables the /datadog command, and /dd alias, to perform actions in Datadog from direct messages.               |
| `im:write`               | Receive messages, prompts, and errors from the Datadog bot related to your Datadog account.                    |
| `links:read`             | Unfurls Datadog links in conversations with additional information like graphs and log samples.                |
| `links:write`            | Unfurls Datadog links in conversations with additional information like graphs and log samples.                |
| `mpim:read`              | Enables the /datadog command, and /dd alias, to perform actions in Datadog from group direct messages.         |
| `reactions:write`        | Adds an emoji reaction to messages that have been added to an incident timeline by shortcut.                   |
| `team:read`              | Keep the Slack integration tile in Datadog up to date with the state of your workspace.                        |
| `users:read`             | Perform actions from Slack as a Datadog user associating with Datadog account.                                 |
| `users:read.email`       | Adding messaging and users for incidents created outside of Slack in Datadog.                                  |
| `workflow.steps:execute` | Automatically send messages with Datadog dashboard widgets from a Slack Workflow Step.                         |

### Optional Bot Token Scopes

Datadog for Slack offers features that require enabling additional optional Bot Token Scopes. These scopes are added dynamically based on feature enablement and are not added during the initial installation.

| Scopes              | Request Reason                                                                               |
|---------------------|----------------------------------------------------------------------------------------------|
| `channels:history`  | Automatically sync messages from an incident channel to the incident timeline.               |
| `groups:write`      | Create private channels to manage and remediate incidents using Datadog Incident Management. |
| `pins:write`        | Create pins in incident channels for relevant Datadog incident links and resources.          |
| `bookmarks:write`   | Bookmark important links in an incident channel during the response process.                  |
| `bookmarks:read`    | Edit bookmarks for important links when they change.                                          |

### User Token Scopes

| Scopes   | Request Reason                                                            |
|----------|---------------------------------------------------------------------------|
| `openid` | Perform actions in Datadog from Slack by connecting your Datadog account. |


### Optional User Token Scopes

Datadog for Slack offers features that require enabling additional optional User Token Scopes. These scopes are added dynamically based on feature enablement and are not added during the initial installation.

| Scopes           | Request Reason                                                    |
|------------------|-------------------------------------------------------------------|
| `auditlogs:read` | Collect enterprise grid audit logs to view in Datadog Cloud SIEM. |

## Enterprise Grid audit logs

Ingest events and actions that occur within your Slack Enterprise Grid.

### Start collecting Slack audit logs

Only owners of an Enterprise Grid organization may authorize Datadog to collect Slack audit logs.

1. On the [Slack integration tile][5], click the **Audit Logs** tab.
2. Click **Connect Enterprise Grid** to be redirected to Slack for authorization.

### Collected events and actions

- User management events, such as user creation, deletion, and updates. This includes changes to user roles, permissions, and profiles.
- Workspace and channel management events, including actions related to the creation, modification, and deletion of channels and workspaces. It also tracks changes in workspace settings and permissions.
- File and app management events, including tracking the upload, download, and deletion of files, as well as monitoring the installation, update, and removal of Slack apps and integrations.
- Security and compliance events, including login attempts, password changes, and two-factor authentication events, as well as compliance-related actions like data exports and access to sensitive information.
- Audit trail of administrative actions, including changes made by Slack admins and workspace owners, such as policy updates, security settings changes, and other administrative modifications.
- External sharing and collaboration events, including the creation of shared channels, external invitations, and guest account activities.

Each event captured provides detailed insights, including:

- Action: What activity was performed.
- Actor: The user in the workspace who generated the event.
- Entity: The thing the actor has taken action upon.
- Context: The location (workspace or enterprise) where the actor took action on the entity.

For more information, see the [official Slack documentation][11].

## Data Collected

### Metrics

The integration for Slack does not provide any metrics.

### Events

The integration for Slack does not include any events.

### Service Checks

The integration for Slack does not include any service checks.

## Troubleshooting

Need help? Contact [Datadog support][12].

[1]: https://docs.datadoghq.com/ja/monitors/configuration/
[2]: https://docs.datadoghq.com/ja/monitors/notifications/
[3]: http://slack.com/account/settings
[4]: https://api.slack.com/methods/usergroups.list
[5]: https://app.datadoghq.com/integrations/slack
[6]: https://docs.datadoghq.com/ja/dashboards/scheduled_reports/
[7]: https://app.datadoghq.com/incidents
[8]: https://app.datadoghq.com/incidents/settings#Integrations
[9]: https://docs.datadoghq.com/ja/service_management/incident_management/
[10]: https://api.slack.com/scopes
[11]: https://api.slack.com/admins/audit-logs-call
[12]: https://docs.datadoghq.com/ja/help/