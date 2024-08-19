---
categories:
- コラボレーション
- notifications
custom_kind: integration
dependencies: []
description: Microsoft Teams で Datadog アラートとイベントの通知を受信
doc_link: https://docs.datadoghq.com/integrations/microsoft_teams/
draft: false
git_integration_title: microsoft_teams
has_logo: true
integration_id: ''
integration_title: Microsoft Teams
integration_version: ''
is_public: true
manifest_version: '1.0'
name: microsoft_teams
public_title: Datadog-Microsoft Teams インテグレーション
short_description: Microsoft Teams で Datadog アラートとイベントの通知を受信
version: '1.0'
---

<!--  SOURCED FROM https://github.com/DataDog/dogweb -->
## 概要

Microsoft Teams と統合して、以下のことができます。

- Microsoft Teams で Datadog アラートとイベントの通知を受信
- Microsoft Teams の中からインシデントを管理することができます。

## Microsoft Teams チャンネルへのモニター通知の送信

### セットアップ

Microsoft のテナントを Datadog に接続します。

1. Datadogで、[**Integrations > Microsoft Teams**][1] の順に移動します。
2. **Add Tenant** をクリックすると、Microsoft に移動します。
3. 画面の指示に従って、**OK** をクリックします。

Datadog 通知を受信させたいすべてのチームに Datadog アプリが追加されていることを確認します。

1. Microsoft Teams の左サイドバーで、**Apps** をクリックし、Datadog アプリを検索します。
2. **Add** ボタンの横にあるドロップダウン矢印をクリックし、**Add to a team** をクリックします。
3. Datadog 通知を受信させたいチームを選択します。
4. **Set up a bot** をクリックします。

ボットをチームに追加したら、Datadog で通知ハンドルを構成します。

1. 構成されたテナントの下で、**Add Handle** をクリックします。ハンドルに名前を付け、ドロップダウンメニューから希望のチームとチャンネルを選択し、**Save** をクリックします。

{{< site-region region="us,ap1,us5,us3,eu" >}}
### 旧来コネクタのテナントベースインテグレーションへの移行

Microsoft は、Microsoft Teams 用の Office 365 コネクタが非推奨となり、2025 年 12 月 31 日 (従来は 2024 年 10 月 1 日) に機能停止することを発表しました。コネクタの新規作成は 2024 年 8 月 15 日からブロックされます。詳細は同社の[ブログ記事][1]を参照してください。
旧来の Office 365 コネクタを現在使用しているすべての通知ハンドルを Datadog のテナントベースのインテグレーションに移行するには、以下の手順に従います。

1. [セットアップ手順](#setup) に従って Microsoft テナントを Datadog に接続します。
2. 旧来の Office 365 コネクタが構成されているすべてのチームに Datadog アプリを追加します。
3. [Microsoft Teams インテグレーションタイル][2]の各レガシー通知コネクタハンドルについて:
   1. 構成されているテナントの下にある **Add Handle** をクリックします。
   2. 新しいハンドルに、コネクタハンドルと同じ名前を付けます。例えば、旧来のコネクタハンドルの名前が `channel-123` の場合、テナント構成に `channel-123` という名前で新しいハンドルを作成します。
   3. 旧来のコネクタハンドルがメッセージを送信していたドロップダウンメニューから希望するチームとチャンネルを選択し、**Save** をクリックします。この新しいハンドルは既存の旧来のコネクタハンドルをオーバーライドします。

[1]: https://devblogs.microsoft.com/microsoft365dev/retirement-of-office-365-connectors-within-microsoft-teams/
[2]: https://app.datadoghq.com/integrations/microsoft-teams
{{< /site-region >}}

### Connector setup (legacy)
<div class="alert alert-info">
Legacy notification handles are not affected by the new setup <em>unless</em> you use the same <code>@teams-HANDLE_NAME</code>, in which case the new configuration overrides the legacy configuration.
</div>

1. Choose the `...` button next to the channel name in the list of channels and then choose **Connectors**.

    {{< img src="integrations/microsoft_teams/microsoft_team_step_1_v2.png" alt="Microsoft Teams step 1" >}}

2. Search for Datadog and click **Configure**.

    {{< img src="integrations/microsoft_teams/microsoft_team_step_2_v2.png" alt="Microsoft Teams step 2" >}}

3. In the connector configuration modal, copy the webhook URL.
4. In Datadog, navigate to [**Integrations > Microsoft Teams**][1].
5. On the Configuration tab, click **Add Handle**, give the handle a name, and paste the webhook URL.
6. In the connector configuration modal, click **Save**.

### Usage

From a Datadog monitor, send a notification to Microsoft Teams using the [`@-notification` feature][2]. Send the notification to the address `@teams-<HANDLE>`, replacing `<HANDLE>` with the name of your Microsoft Teams handle.

## Datadog Incident Management in Microsoft Teams

### Account setup

First, install the Datadog App in Microsoft Teams:

1. Open Microsoft Teams.
2. In the vertical toolbar, click **Apps**.
3. Search for "Datadog" and click on the tile.
4. Click **Add** to install the Datadog App. Next to the "Add" button, open the dropdown and select **Add to a team**.

{{< img src="integrations/microsoft_teams/microsoft_teams_install_datadog_in_teams_v2.png" alt="Datadog install app tile in Microsoft Teams" >}}

5. On the dropdown menu, select the team that the App should be added to, then click **Set Up** to complete the installation.


Next, connect your Microsoft tenant to Datadog:

1. In Datadog, navigate to the [Microsoft Teams Integration Tile][1].
2. Click **Add Tenant**, which redirects you to Microsoft.
3. Follow the prompts and click **OK**.

Some Datadog Incident Management features need permission to perform actions on your tenant, for example, creating a new
team for an incident. You need someone who is authorized to consent on behalf of the Microsoft organization to
grant tenant-wide admin consent, such as a user assigned the *Global Admin* role. View [Microsoft Entra ID documentation][3] for more
information on who can grant tenant-wide admin consent to the Datadog application.

To grant consent:

1. Navigate to the [Microsoft Teams Integration Tile][1] in Datadog.
2. For the tenant in which you want to use Incident Management, click the gear icon on the right-hand side.
3. **Authorize Tenant** をクリックすると、Microsoft にリダイレクトされます。テナント全体の管理者同意を付与できるユーザーが、この手順を実行する必要があります。このユーザーは Datadog アカウントを持っている必要がありますが、Datadog アカウントで使用するメールアドレスは Microsoft アカウントのメールアドレスと一致する必要はありません。
4. Follow the prompts and click **OK**.

### User setup

Performing actions in Datadog from Microsoft Teams requires you to connect your Datadog and Microsoft Team accounts.

To connect your account from Microsoft Teams:

1. Open Microsoft Teams.
2. Start a chat with the Datadog bot by clicking on the `...` button in the vertical toolbar and selecting Datadog.
3. Type "accounts" and hit enter.
   {{< img src="integrations/microsoft_teams/microsoft_teams_connect_account_from_teams.png" alt="Connect accounts from Microsoft Teams" >}}

4. The Datadog bot will respond with instructions on how to connect your accounts. Click **Connect Datadog Account**.
5. The Datadog bot will then send a message containing a link to connect your accounts. Click the link and follow the prompts.
6. You will be redirected back to the [Microsoft Teams Integration Tile][1].
7. Create an application key by clicking **Create** in the prompt on the [Microsoft Teams Integration Tile][1].


You can also connect your accounts from Datadog:

1. In Datadog, navigate to the [Microsoft Teams Integration Tile][1].
2. Click **Connect** in the tenant listed.
3. Follow the prompts and click **OK**.
5. From the [Microsoft Teams Integration Tile][1], create an application key by clicking **Create** in the above prompt.

{{< img src="integrations/microsoft_teams/microsoft_teams_connect_account_from_datadog_v2.png" alt="Connect accounts from Datadog Microsoft Teams integration tile" >}}

### Usage

#### Dashboards

You can post dashboard widget snapshots on any team or chat. For a list of supported widgets, see [Scheduled Reports][4].

To share a dashboard widget in Teams:

1. In Datadog, hover over a dashboard widget and press `CMD + C` or `CTRL + C`, or click the **Copy** button from the share menu.
1. Paste the link into Teams.

{{< img src="integrations/microsoft_teams/dashboard_share.png" alt="Sharing a dashboard widget in Microsoft Teams">}}

#### Incidents

To declare a new incident from Microsoft Teams:

1. Start a conversation in any team.
2. Type `@Datadog` or use the `...` button to open the **Messaging extensions** menu and select the **Datadog** App.
3. Select **Create an Incident**.
4. Complete the form with your desired information.
5. Click **Create**.

Anyone in your Microsoft Teams tenant can declare an incident, regardless of whether they have access to Datadog.

When a new incident is created, a corresponding team named `incident-(unique number ID)` is created.

To update an incident, follow a similar process as creation:

1. Start a conversation while in an incident team.
2. Type `@Datadog` or use the `...` button to open the **Messaging extensions** menu and select the **Datadog** App.
3. Select **Update Incident**.
4. Complete the form with your desired information.
5. Click **Update**.

List all open (active and stable) incidents with:

```
@Datadog list incidents
```

Use the "More actions" menu on any message inside an incident team on the far right to send that message to the incident Timeline.

#### Incident updates channel
Using an incident updates channel provides your stakeholders with organization-wide visibility into the status of all incidents directly from Microsoft Teams. Select which team and channel in your account to post these updates to, and the channel receives the following posts:

   - Newly declared incidents.
   - Changes to severity, status transition, and incident commander.
   - Links to the incident's overview page in App.
   - Link to join the dedicated incident team.

Once the Microsoft Teams App has been installed, you can navigate to the **Incident Settings** page. From this, you can scroll down to the **Incident Updates** Channel section and begin the set-up flow.

#### How to set up an incident channel:

1. Navigate to [Incidents Settings][5].
2. Under the Microsoft Teams section, select your connected Microsoft Teams tenant.
3. Toggle on **Automatically create a Microsoft Teams channel for every incident**.
4. Select the Team in which you want to automatically create new channels.
5. Save your settings.

{{< img src="integration/microsoft_teams/ms_teams_incident_updates_v2.png" alt="Microsoft Teams インシデント更新チャンネル設定." >}}

## Data collected

### Metrics

The Microsoft Teams integration does not provide any metrics.

### Events

The Microsoft Teams integration does not include any events.

### Service checks

The Microsoft Teams integration does not include any service checks.

## Permissions

The Microsoft Teams integration receives the following permissions for Teams it has been added to. For more information, see [Microsoft App permission reference][6].

| Permission description                                                                                                                                                                   | Request Reason                                                                           |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| Receive messages and data that I provide to it.                                                                                                                                          | Users can interact with the Datadog app in personal chat.                                |
| Send me messages and notifications.                                                                                                                                                      | Users can interact with the Datadog app in personal chat.                                |
| Access my profile information such as my name, email address, company name, and preferred language.                                                                                      | Enable users to configure Microsoft Teams notifications and workflows within the Datadog UI. |
| Receive messages and data that team or chat members provide to it in a channel or chat.                                                                                                  | Users can interact with Datadog through @Datadog commands.                           |
| Send messages and notifications in a channel or chat.                                                                                                                                    | Send Datadog notifications to configured targets.                                        |
| Access information from this team or chat such as team or chat name, channel list, and roster (including team or chat member's names and email addresses) - and use this to contact them. | Enable users to configure Microsoft Teams notifications and workflows within Datadog. |


Additional permissions are needed to use Incident Management features in the Microsoft Teams integration. These must be authorized by a user with tenant-wide permissions (see [Datadog Incident Management in Microsoft Teams: Account setup](#account-setup) for detailed instructions).
For more information on these permissions, see the [Microsoft Graph permission reference][7].

<table>
  <tr>
    <td style="width:40%;"><strong>API / 権限の名前</strong></td>
    <td style="width:15%;"><strong>タイプ</strong></td>
    <td><strong>リクエスト理由</strong></td>
  </tr>
  <tr>
    <td style="width:40%;"><code>ChannelSettings.ReadWrite.All</code></td>
    <td style="width:15%;">アプリケーション</td>
    <td>Datadog Incident Management を使用してインシデントを解決するために、チャンネルを作成および変更します。</td>
  </tr>
  <tr>
    <td style="width:40%;"><code>GroupMember.Read.All</code></td>
    <td style="width:15%;">アプリケーション</td>
    <td>Datadog Incident Management の構成用に、チームおよびチャンネル名のオートコンプリート候補を提供します。</td>
  </tr>
  <tr>
    <td style="width:40%;"><code>Team.Create</code></td>
    <td style="width:15%;">アプリケーション</td>
    <td>Datadog Incident Management を使用してインシデントを管理および解決するために、チームを作成します。</td>
  </tr>
  <tr>
    <td style="width:40%;"><code>TeamMember.ReadWrite.All</code></td>
    <td style="width:15%;">アプリケーション</td>
    <td>Datadog Incident Management を使用してインシデントを管理するために、ユーザーをチームに追加します。</td>
  </tr>
  <tr>
    <td style="width:40%;"><code>TeamsAppInstallation.ReadWrite.All</code></td>
    <td style="width:15%;">アプリケーション</td>
    <td>Datadog Incident Management によって作成されたチームに Datadog アプリを追加します。</td>
  </tr>
  <tr>
    <td style="width:40%;"><code>TeamSettings.ReadWrite.All</code></td>
    <td style="width:15%;">アプリケーション</td>
    <td>インシデントチームの状態で Datadog Incident Management を最新に保ちます。</td>
  </tr>
</table>

## Troubleshooting

### Using SSO

Use the following steps to set new channel connectors:

1. Login to Datadog, then complete setup steps 1 and 2.

2. After setup step 3 redirects you to Datadog from the MS Teams page, open a new tab and log into Datadog with your SSO. Then perform setup step 4 separately.

### インテグレーションタイルにチームが表示されないのはなぜですか？
テナントを Datadog に追加する前にボットをチームに追加した場合、Datadog はチーム参加イベントを見逃し、チームの存在を知ることができません。
以下のいずれかを試すことができます。
- Datadog アプリをチームから削除し、再度追加します。**注**: この操作により、そのチームの構成コネクタが削除されます。そのチームのすべてのコネクタをテナントベースのインテグレーションに移動する準備ができたら、このアクションを実行してください。
1. 左サイドバーのチーム名の横にある 3 つの点をクリックします。
2. **Manage Team** をクリックします。
3. **Apps** というラベルの付いたタブに移動します。
4. Datadog アプリの横にある 3 つの点をクリックします。
5. Click **Remove**.
6. [セットアップ手順](#setup)に従って Datadog アプリを追加します。
- Datadog が利用可能なすべての Microsoft Teams チャンネルをクロールするための管理者同意を認可します。
1. Navigate to the [Microsoft Teams Integration Tile][1] in Datadog.
2. 管理者同意を付与したいテナントで、右側の歯車アイコンをクリックします。
3. **Authorize Tenant** をクリックすると Microsoft にリダイレクトされます。この手順を実行するには、テナント全体に対する管理者同意を与えることができるユーザーが必要です。**注**: Microsoft のユーザーが Datadog のアカウントを持っている必要はありません。
4. Follow the prompts and click **OK**.
5. 数時間以内にクローラーが実行され、利用可能なすべてのチームとチャンネルが入力されます。

### プライベートチャンネルはボットでサポートされていますか？
[Microsoft Teams][8] のプライベートチャンネルの制限により、プライベートチャンネルはボットでサポートされていません。


Need help? Contact [Datadog support][9].

[1]: https://app.datadoghq.com/integrations/microsoft-teams
[2]: https://docs.datadoghq.com/ja/monitors/notifications/#notification
[3]: https://learn.microsoft.com/en-us/azure/active-directory/manage-apps/grant-admin-consent?pivots=ms-graph#prerequisites
[4]: https://docs.datadoghq.com/ja/dashboards/scheduled_reports/
[5]: https://app.datadoghq.com/incidents/settings#Integrations
[6]: https://learn.microsoft.com/en-us/microsoftteams/app-permissions#what-can-apps-do-in-teams
[7]: https://learn.microsoft.com/en-us/graph/permissions-reference
[8]: https://learn.microsoft.com/en-us/microsoftteams/private-channels#private-channel-limitations
[9]: https://docs.datadoghq.com/ja/help/
