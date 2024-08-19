---
aliases:
- /ja/monitors/monitor_status/
description: 時系列でのモニターステータスの概要を取得
further_reading:
- link: /monitors/
  tag: ドキュメント
  text: モニターの作成
- link: /monitors/notify/
  tag: ドキュメント
  text: モニター通知
- link: /monitors/manage/
  tag: ドキュメント
  text: モニターの管理
title: モニターステータス
---

## Overview

After [creating your monitor][1], use the monitor status page to view the status over time.

{{< img src="monitors/monitor_status/monitor_status_page.png" alt="monitor status page" >}}

## Header

The header contains the monitor's status, time of status, and monitor title. On the right are the **Mute**, **Resolve**, and settings cog buttons.

### Mute

Use the mute button to mute the entire monitor or partially mute it by setting a **Scope**. The available scopes are based on the monitor's group tags. See [Downtimes][2] for details on muting multiple scopes or monitors at the same time.

**Note**: Muting or unmuting a monitor with the UI deletes all scheduled downtimes associated with that monitor.

### Resolve

If your monitor is in an alert state, the **Resolve** button is visible. Use this button to resolve your monitor manually.

The monitor `resolve` function is artificially switching the monitor status to `OK` for its next evaluation. The next monitor evaluation is performed normally on the data the monitor is based on.

If a monitor is alerting because its current data corresponds to the `ALERT` state, `resolve` has the monitor follow the state switch `ALERT -> OK -> ALERT`. Therefore, using `resolve` is not appropriate for acknowledging the alert or telling Datadog to ignore the alert.

Manually resolving a monitor is appropriate for cases where data is reported intermittently. For example, after triggering an alert the monitor doesn't receive further data so it can no longer evaluate alerting conditions and recover to the `OK` state. In that case, the `resolve` function or the `Automatically resolve monitor after X hours` changes the monitor back to an `OK` state.

**Typical use case**: A monitor based on error metrics that are not generated when there are no errors (`aws.elb.httpcode_elb_5xx`, or any DogStatsD counter in your code reporting an error _only when there is an error_).

### Create an incident
Create an incident from a monitor by selecting **Declare incident**. Configure the *Declare Incident* pop-up modal with the severity level, notifications, and additional notes. For more information, see the [Incident Management][3] documentation.

### Settings

Click the settings cog to display the options available:

| Option | Description                                                                                                                                                                                                    |
|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Edit   | Edit the current monitor. See details in the [Configure Monitors][1] section.                                                                                                                                            |
| Clone  | Make a copy of the current monitor.                                                                                                                                                                            |
| Export | Export the JSON configuration for the current monitor. This option is also available when [creating your monitor][1]. If you manage monitors programmatically, define a monitor in the UI and export the JSON. |
| Delete | Delete the current monitor. You will be prompted to confirm the deletion.                                                                                                                                      |

## Properties

The properties section is the overview of your monitor's:

| Property     | Description                                                                           |
|--------------|---------------------------------------------------------------------------------------|
| Status       | Alert, Warn, No Data, or OK                                                           |
| Type         | Learn more about [monitor types][4].                                                  |
| ID           | Used for the [monitor API][5].                                                        |
| Date created | The date the monitor was created.                                                     |
| Author       | The person who created the monitor.                                                   |
| Tags         | The tags attached at the monitor level. Edit the tags by clicking on the pencil icon. |
| Query        | Learn more about [querying][6].                                                       |
| Message      | The message specified in the [notification][7] section of the monitor.                |

## Status and history

The status and history section displays the query and state changes of your monitor over time. To filter the information, use the search box, statuses, and time selector above the section.

### Status

The status graph shows your monitor's status over time, broken out by group. **Note**: If you see `None` or `no groups found`, one of the following situations may apply:

* The monitor is newly created and has not evaluated yet.
* The monitor's query was recently changed.
* The monitor's timeframe is too short for a metric that provides data infrequently.
* A host's name previously included in the query has changed. Hostname changes age out of the UI within 2 hours.
* The query you are filtering by is not working as expected.

ステータスグラフには、モニタークエリのディメンションではなく、アラート用に構成したディメンションが表示されます。例えば、モニタークエリは `service` と `host` でグループ化されていますが、`service` のアラートのみを受信したい場合、ステータスグラフはモニターのステータスを `service` でグループ化して表示します。`host` サブグループは、View all をクリックすると、各サブグループのステータスグラフを表示するパネルが開きます。アラートのグループ化の詳細については、[モニターの構成][14]を参照してください。

{{< img src="monitors/monitor_status/monitor_status_group_subgroup.png" alt="Monitor status grouped by service, highlighting option to view subgroups " style="width:100%;" >}}

#### Filter the monitor status by groups or events

To scope down the **Status & History** view to specific groups, use the filter field and enter the attributes you want to filter by. The group filter syntax follows the same principles of the [Monitor Search query][30]. Some best practices to follow:

- Filters are case sensitive, `env:prod` and `env:Prod` do not return the same monitor groups. Datadog recommends practicing uniformity in tags. For more information, see [Getting Started with Tags][31]. 
- Queries automatically append a wildcard. To apply specific filters, surround your query with double quotes (`"`).
  For example, take the following query which does not use double quotes:
  ```
  availability-zone:us-central1-a,instance-type:*,name:gke-demo-1
  ```
  The monitor returns the follow groups even though you expect the query to show one specific group.
  ```
  availability-zone:us-central1-a,instance-type:*,name:gke-demo-10
  availability-zone:us-central1-a,instance-type:*,name:gke-demo-12
  ```

  Surrounding the query with double quotes returns the expected group: 
  `"availability-zone:us-central1-a,instance-type:*,name:gke-demo-1"`

#### Investigate a Monitor in a Notebook

For further investigation into your metrics evolution, click **Open in a notebook** by the status graph. This generates an investigation [notebook][8] with a formatted graph of the monitor query.

{{< img src="monitors/monitor_status/notebook-button2.png" alt="Open in notebook button" style="width:90%;">}}

The notebook matches the monitor evaluation period time range and includes related logs where relevant.

#### Follow monitor group retention

Datadog keeps monitor groups available in the UI for 24 hours unless the query is changed. Host monitors and service checks that are configured to notify on missing data are available for 48 hours. If a monitor graph displays a dotted line and is marked as non-reporting, it can be for the following reasons:

- 新しいグループは、モニター作成後しばらくして評価されます。評価グラフには、期間の開始からグループが最初に評価されるまでの点線が表示されます。
- グループは報告を停止し、脱落し、その後再び報告を開始します。点線は、グループが脱落した時点から再び評価を開始する時点まで表示されます。

{{< img src="monitors/monitor_status/dotted-line.png" alt="グループ維持率を表示" style="width:90%;">}}

**Note**: Non-reporting is not the same as no data. Non-reporting status is specific to groups.

### History

The history graph shows the collected data aligned with the status graph. It shows the raw data points being submitted for the metric query in the monitor. The monitor status page uses the same timeseries graph widget that is used in Notebooks and Dashboards.

### Evaluation graph

The evaluation graph is specific to the monitor. It uses the same query logic as the history graph, however it is scoped to the timeframe bracket on the history graph. It has a fixed, zoomed window that corresponds to your monitor [evaluation window][9] to ensure the displayed points are aggregated correctly. For example, if the monitor is configured to evaluate the average of the query over the last 15 minutes, each datapoint in the evaluation graph shows the aggregate value of the metric for the previous 15 minute evaluation window.

This graph shows the results from the raw data points of a metric applied against the evaluation conditions you configure in the monitor. This visualization is different from the History graph because it's showing the value of the data after it has gone through the monitor query. 

{{< img src="monitors/monitor_status/status_monitor_history.mp4" alt="status monitor history" video="true" width="100%" >}}

## Events

Events generated from your monitor (alerts, warnings, recoveries, etc.) are shown in this section based on the time selector above the **Status & History** section. The events are also displayed in your [Events Explorer][10].

### Audit trail
Audit Trail automatically captures monitor changes for all monitor types and creates an event. This event documents the changes to the monitor.

例えば、モニターを編集した場合、監査証跡イベントには次のように表示されます。
 - 以前のモニター構成
 - 現在のモニター構成
 - 変更を行ったユーザー

 詳細については、[監査証跡][11]ドキュメントを参照し、[監査証跡のベストプラクティス][12]ブログをお読みください。

Datadog also provides a notification option for changes to monitors you create. At the bottom of the monitor editor, under **Define permissions and audit notifications**, select **Notify** in the dropdown next to: *If this monitor is modified, notify monitor creator and alert recipients.*.

The notify setting sends an email with the monitor audit event to all people who are alerted in the specific monitor as well as to the monitor creator. The monitor audit event also appears in the [Events Explorer][10].

## Export and import

You can obtain a JSON export of any monitor from the monitor's status page. Click the settings cog (top right) and choose **Export** from the menu.

メインナビゲーションで、*Monitors --> New Monitor --> Import* の順に選択し、JSON を使用して Datadog に[モニターをインポート][13]します。

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}


[1]: /ja/monitors/configuration/
[2]: /ja/monitors/downtimes/
[3]: /ja/service_management/incident_management/#from-a-monitor
[4]: /ja/monitors/types/
[5]: /ja/api/v1/monitors/
[6]: /ja/dashboards/querying/
[7]: /ja/monitors/notify/
[8]: /ja/notebooks
[9]: /ja/monitors/configuration/?tab=thresholdalert#evaluation-window
[10]: https://app.datadoghq.com/event/explorer
[11]: /ja/account_management/audit_trail/
[12]: https://www.datadoghq.com/blog/audit-trail-best-practices/
[13]: https://app.datadoghq.com/monitors#create/import
[14]: /ja/monitors/configuration/?tab=thresholdalert#configure-notifications-and-automations
[30]: /ja/monitors/manage/search/#query
[31]: /ja/getting_started/tagging/