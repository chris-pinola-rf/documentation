---
further_reading:
- link: https://www.datadoghq.com/blog/automate-end-to-end-processes-with-datadog-workflows/
  tag: ブログ
  text: Datadog Workflows でエンドツーエンドプロセスを自動化し、イベントに迅速に対応する
- link: /service_management/workflows/access/
  tag: ドキュメント
  text: アクセスと認証
- link: /security/cloud_security_management/workflows/
  tag: ドキュメント
  text: セキュリティワークフローの自動化
title: Workflow Automation を始める
---

## Overview

Workflow Automation を使用すると、Datadog のアラートやセキュリティシグナルに応じて、エンドツーエンドのプロセスを自動化できます。リアルタイムの可観測性データを活用することで、問題を迅速に解決し、システムの可用性とセキュリティを積極的に維持することが可能です。

Follow this guide to create a custom workflow triggered by a monitor alert. When it is triggered, the workflow creates a task in Jira and sends a notification to Slack with a link to the Jira ticket. This guide covers passing data from one step in your workflow to another step, saving and publishing your workflow, and viewing the workflow's run history.

## Prerequisites

開始する前に、Jira と Slack のインテグレーションを [Datadog アカウント][1]にインストールしておく必要があります。インストール手順については、[Slack][2] と [Jira インテグレーション][3]のドキュメントをご覧ください。

Account credentials and authentication that you set up in the Jira and Slack integration tiles automatically propagate to the Jira and Slack actions in Workflow Automation. Some integrations require additional configuration for authentication. For more information, see [Connections][4].

## ワークフローの構築

### モニタートリガーを使用してワークフローを作成する
ワークフローは、モニターやセキュリティシグナルなどのアラート、スケジュール、または手動でトリガーすることができます。このケースでは、モニタートリガーを使用してワークフローを作成します。

Create a workflow:
1. From the **[Workflow Automation][5]** page, click **New Workflow**.
1. Enter a name and description for the workflow. The example workflow uses the following name and description:<br>
   Name: `Create a Jira Ticket`.<br>
   Description: `Create a Jira issue and Slack notification when there is a monitor alert`.

Add and configure the monitor:
1. ワークフローキャンバスで、**Add Trigger** をクリックし、**Monitor** を選択します。
1. In the **Configure** tab, next to `@workflow-`, enter a unique ID for your workflow: `Create-Jira-Ticket`.<br>
   Workflow handles always begin with `@workflow-`. Later, you use this handle to connect the workflow to a monitor notification.
1. **Save** をクリックしてワークフローを保存します。

{{< img src="/getting_started/workflow_automation/trigger1.png" alt="Trigger for Workflows">}}

### Jira と Slack のアクションを追加する
Add and configure the Jira step:
1. On the workflow canvas, click the **+** icon.
1. Search for the Jira action and select **Create issue**.
1. On the workflow canvas, click the **Create issue** step.
1. In the **Configure** tab, select a **Jira Account**. The account should correspond to the Jira URL found in the **Accounts** section of the Jira integration tile.
1. Enter the **Project** and **Issue type** for the Jira issue that the workflow creates.
1. Enter a **Summary** and **Description** for the Jira issue, using context variables to pass in data from the monitor that triggers the workflow. You can access a context variable in a step by enclosing it in double braces (`{{`).<br><br>
   The following example description uses the source, trigger, and workflow variables to pass along:
   - the source that triggered the monitor alert
   - a link to the affected monitor
   - the workflow name, and the workflow ID

   ```
   The CPU usage is above the 95% threshold for {{ Trigger.hostname }}

   Please investigate - see this Datadog Dashboard to view all workflow executions:
   https://app.datadoghq.com/dash/integration/workflow_automation?refresh_mode=sliding&from_ts=1698164453793&to_ts=1698168053793&live=true.

   The workflow that created this Jira issue is
   {{ WorkflowName }} : {{ WorkflowId }}

   The monitor that triggered the workflow can be found here: {{ Source.url }}
   ```

   For more information on context variables, see **[Context variables][6]**.

1. Test the Jira action by clicking **Test** in the **Configure** tab. Testing the action creates a real Jira ticket.
1. **Save** をクリックしてワークフローを保存します。

Next, add the Slack step:
1. ワークフローキャンバス上のプラスアイコンをクリックして、別のステップを追加します。
1. Search for Slack and select **Send message**.
1. Enter the **Slack Workspace name**.
1. **Slack Channel name** を入力します。
1. For a more useful Slack notification, use step output variables. Step output variables allow you to pass data from the Jira step to the Slack step in your workflow. Use the following message text to include the Jira issue key, the monitor name, and the Jira issue in the Slack message:

   ```
   The monitor named {{ Source.monitor.name }} triggered and created a new Jira issue
   {{ Steps.Create_issue.issueKey }}: {{ Steps.Create_issue.issueUrl }}

   The workflow that created this Jira issue is {{ WorkflowName }}
   ```

1. To test the action, click **Test** in the **Configure** tab. Testing an action creates a real Slack message.
1. ワークフローの名前をモニターの通知ドロップダウンに表示するには、ワークフローを保存して公開します。ワークフローのページから **Publish** をクリックします。

## ワークフローをテストして公開する

<div class="alert alert-info">アクティブな Slack および Jira アカウントに接続されたワークフローをテストすると、実際の Slack メッセージや Jira チケットが作成されます。</div>

Click **Save** to apply your changes to the workflow. Next, manually trigger the workflow to test it.

手動でワークフローをトリガーする場合は、ワークフローページから **Run** をクリックし、トリガー変数の値を入力します。

{{< img src="/getting_started/workflow_automation/run_workflow.png" alt="ワークフローを手動でトリガーする" style="width:90%;" >}}

Confirm that running the workflow creates a Jira ticket and sends a Slack message.

スケジュールされたワークフローやトリガーされたワークフローは、公開するまで自動的にトリガーされません。ワークフローのページから **Publish** をクリックして公開してください。

## ワークフローをトリガーするモニターを更新する

1. Datadog の [Monitors ページ][7]に移動します。
1. Find the monitor you'd like to use to trigger the workflow and edit it, or create a new monitor.
1. **Configure notifications &amp; automations** セクションで、**Add Workflow** をクリックします。
1. ワークフローのメンション名 (`@workflow-Create-Jira-Ticket`) を使用してワークフローを検索し、ドロップダウンから選択します。
   - ワークフローにトリガー変数を渡すには、`@workflow-name(key=value, key=value)` という構文のカンマ区切りリストを使用します。例えば、`@workflow-Create-Jira-Ticket(hostname={{host.name}})` のようになります。
1. ワークフローとこのモニターのすべての通知をテストするには、**Test Notifications** をクリックします。
1. モニターを保存。

{{< img src="/getting_started/workflow_automation/monitor_trigger.png" alt="モニターからワークフローをトリガー">}}

モニターのしきい値に達するたびに、モニターはワークフローの実行をトリガーします。

## 実行履歴

ワークフローをトリガーした後、**Run History** ビューで進捗状況を確認したり、失敗したステッ プをデバッグすることができます。実行されたステップを選択すると、入力、出力、実行コンテキスト、エラーメッセージを確認できます。以下の例は、無効な Jira 構成のために失敗したステップを示しています。

{{< img src="/getting_started/workflow_automation/testing_the_workflow.mp4" alt="ワークフローテストのプレビュー" style="width:100%" video=true >}}

ワークフローを編集するには、**Configuration** をクリックします。実行履歴ビューに戻るには、**Run History** をクリックします。

以前のワークフロー実行のリストや、各実行の成功・失敗を確認するには、初期の実行履歴ビューを使用します。ワークフローキャンバスをクリックすることで、いつでも初期の実行履歴に戻ることができます。

## 考察

モニターがワークフローをトリガーすると、エンジニアリングチームがレビューできるように Jira 課題が作成されます。以下に Jira 課題の例を示します。

{{< img src="/getting_started/workflow_automation/jira_ticket.png" alt="ワークフローから生成される Jira チケット">}}

ワークフローは、Jira 課題とモニターアラートをチームに通知する Slack メッセージも作成します。以下は Slack の通知例です。

{{< img src="/getting_started/workflow_automation/slack-message.png" alt="ワークフローから生成される Slack メッセージ">}}

## 次のステップ

* [アクションカタログ][8]で、利用可能なすべてのワークフローアクションのリストを確認する。
* すぐに使える[ブループリント][9]からワークフローを構築する。
* [HTTP アクション][10]を使用して、任意のエンドポイントにリクエストを行う。
* [データ変換][11]アクションを実装して、ワークフローを流れる情報に対して必要な処理を実行する。

## その他の参考資料

{{< partial name="whats-next/whats-next.html" >}}


[1]: https://www.datadoghq.com/free-datadog-trial/
[2]: /ja/integrations/slack/
[3]: /ja/integrations/jira/
[4]: /ja/service_management/workflows/connections/
[5]: https://app.datadoghq.com/workflow
[6]: /ja/workflows/build/#context-variables
[7]: https://app.datadoghq.com/monitors/manage
[8]: /ja/service_management/workflows/actions_catalog/
[9]: /ja/workflows/build
[10]: /ja/service_management/workflows/actions_catalog/generic_actions/#http
[11]: /ja/service_management/workflows/actions_catalog/generic_actions/#data-transformation