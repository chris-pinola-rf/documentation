---
aliases:
- /ja/logs/log_configuration/sensitive_data_detection
- /ja/account_management/org_settings/sensitive_data_detection
further_reading:
- link: /data_security/
  tag: ドキュメント
  text: データ関連リスクの低減
- link: /sensitive_data_scanner/regular_expression_syntax
  tag: ドキュメント
  text: カスタムスキャンルールのための正規表現構文
- link: https://www.datadoghq.com/blog/scaling-sensitive-data-scanner/
  tag: ブログ
  text: 機密データスキャナーを使用して、機密データの問題を大規模に発見、トリアージ、そして修復する
- link: https://www.datadoghq.com/blog/sensitive-data-scanner/
  tag: ブログ
  text: Datadog の機密データスキャナーで最新のデータコンプライアンス戦略を構築する
- link: https://www.datadoghq.com/blog/sensitive-data-management-best-practices/
  tag: ブログ
  text: 機密データ管理のベストプラクティス
- link: https://www.datadoghq.com/blog/data-security/
  tag: Blog
  text: データセキュリティでクラウドデータストア内の機密データを発見
title: Sensitive Data Scanner
---

## Overview

Sensitive data, such as credit card numbers, bank routing numbers, and API keys are often exposed unintentionally in application logs, APM spans, and RUM events, which can expose your organization to financial and privacy risks.

Sensitive Data Scanner is a stream-based, pattern matching service used to identify, tag, and optionally redact or hash sensitive data. Security and compliance teams can implement Sensitive Data Scanner as a new line of defense, helping prevent against sensitive data leaks and limiting non-compliance risks.

To use Sensitive Data Scanner, set up a scanning group to define what data to scan and then set up scanning rules to determine what sensitive information to match within the data.

This document walks you through the following:

- Sensitive Data Scanner の表示と設定に必要な権限。
- 機密データのスキャンのセットアップ。
- すぐに使えるダッシュボードを使用。

**Note**: See [PCI DSS Compliance][1] for information on setting up a PCI-compliant Datadog organization.

{{< img src="sensitive_data_scanner/sds_main_12_01_24.png" alt="The Sensitive Data Scanner page showing six out of the 12 active scanning groups" style="width:90%;">}}

## Set up Sensitive Data Scanner


機密データをマスキングできる場所は 2 つあります。

**クラウド:**

- **Sensitive Data Scanner in the Cloud** では、Datadog バックエンドにログを送信します。この方法では、ログがマスキングされる前にプレミスを離れます。組織ごとに複数のスキャングループを持つことができ、カスタムスキャンルールを作成できます。タグで機密データをマスキングすることも可能です。

**環境:**

{{< callout url="https://www.datadoghq.com/private-beta/sensitive-data-scanner-using-agent-in-your-premises/" >}}
Sensitive Data Scanner using the Agent は非公開ベータ版です。アクセスをリクエストするには、このフォームに記入してください。
{{< /callout >}}

- **Sensitive Data Scanner using the Agent** では、ログを Datadog バックエンドに送信する前に Datadog がログをマスキングし、マスキングされていないログはプレミス外に出る必要がなくなります。この方法では、組織ごとに 1 つのスキャングループに制限され、定義済みのライブラリルールしか使用できません。

- 環境内の機密データを下流の宛先に送信する前にマスキングする別の方法としては、[Observability Pipelines][14] を使用することがあります。

### Prerequisites

{{< tabs >}}
{{% tab "クラウド" %}}
デフォルトでは、Datadog 管理者ロールを持つユーザーは、スキャンルールを表示および設定するためのアクセス権を持っています。他のユーザーにアクセスを許可するには、**Compliance** で `data_scanner_read` または `data_scanner_write` の権限をカスタムロールに付与します。ロールと権限のセットアップ方法の詳細については、[アクセス制御][3]を参照してください。

{{< img src="sensitive_data_scanner/read_write_permissions.png" alt="データスキャナーの読み取り権限と書き込み権限が表示されているコンプライアンス権限セクション" style="width:80%;">}}

[1]: /ja/account_management/rbac/permissions/#compliance
[2]: /ja/account_management/rbac/
{{% /tab %}}
{{% tab "Agent の使用" %}}

1. 適切な権限を付与します。デフォルトでは、Datadog 管理者ロールを持つユーザーは、スキャンルールを表示および設定するためのアクセス権を持っています。他のユーザーにアクセスを許可するには、**Compliance** で `data_scanner_read` または `data_scanner_write` の権限をカスタムロールに付与します。ロールと権限のセットアップ方法の詳細については、[アクセス制御][3]を参照してください。

    {{< img src="sensitive_data_scanner/read_write_permissions.png" alt="データスキャナーの読み取り権限と書き込み権限が表示されているコンプライアンス権限セクション" style="width:80%;">}}
2. [リモート構成を有効にする][3]手順に従ってください。
3. Datadog Agent v7.54 以降をインストールします。

[1]: /ja/account_management/rbac/permissions/#compliance
[2]: /ja/account_management/rbac/
[3]: /ja/agent/remote_config/?tab=configurationyamlfile#enabling-remote-configuration
{{% /tab %}}
{{< /tabs >}}

### Add a scanning group

{{< tabs >}}
{{% tab "クラウド" %}}
スキャングループは、スキャンするデータを決定します。これは、ログ、APM、RUM、およびイベントのスキャンを有効にするためのクエリフィルターとトグルのセットで構成されています。クエリフィルターの詳細については、[ログ検索構文][2]のドキュメントを参照してください。

Terraform に関しては、[Datadog 機密データスキャナーグループ][3]のリソースを参照してください。

スキャングループをセットアップするには、以下の手順を実行します。

1. [Sensitive Data Scanner][1] 構成ページに移動します。
1. Click **Add scanning group**. Alternatively, click the **Add** dropdown menu on the top right corner of the page and select **Add Scanning Group**.
1. Enter a query filter for the data you want to scan. At the top, click **APM Spans** to preview the filtered spans. Click **Logs** to see the filtered logs.
1. Enter a name and description for the group.
1. Click the toggle buttons to enable Sensitive Data Scanner for the products you want (for example, logs, APM spans, RUM events, and Datadog events).
1. Click **Create**.

[1]: https://app.datadoghq.com/organization-settings/sensitive-data-scanner/configuration
[2]: /ja/logs/explorer/search_syntax/
[3]: https://registry.terraform.io/providers/DataDog/datadog/latest/docs/resources/sensitive_data_scanner_group
{{% /tab %}}
{{% tab "Agent の使用" %}}
<div class="alert alert-warning"><strong>注</strong>: Sensitive Data Scanner using the Agent は、1 つの組織につき 1 つのスキャングループのみをサポートします。</div>

スキャングループは、スキャンするログを決定します。ホストタグに基づいて適格な Agent に一致させるクエリフィルターで構成されます。

スキャングループをセットアップするには、以下の手順を実行します。

1. [Sensitive Data Scanner using the Agent][1] の構成ページに移動します。
1. Click **Add scanning group**. Alternatively, click the **Add** dropdown menu on the top right corner of the page and select **Add Scanning Group**.
1. スキャンするデータのクエリフィルターを入力します。Agent の一致にはホストレベルのタグのみを使用できます。最下部には、タグに一致する Agent の総数を含む、一致して対象となる Agent の数が表示されます。
1. Enter a name and description for the group.
1. Click **Save**.

[1]: https://app.datadoghq.com/organization-settings/sensitive-data-scanner/configuration/agent
[2]: /ja/logs/explorer/search_syntax/
[3]: https://registry.terraform.io/providers/DataDog/datadog/latest/docs/resources/sensitive_data_scanner_group
{{% /tab %}}
{{< /tabs >}}

デフォルトでは、新しく作成されたスキャングループは無効になっています。スキャングループを有効にするには、右側の対応するトグルをクリックします。

### Add scanning rules

{{< tabs >}}
{{% tab "クラウド" %}}
スキャンルールは、スキャングループで定義されたデータ内のどの機密情報をマッチングさせるかを決定します。Datadog のスキャンルールライブラリから事前定義されたスキャンルールを追加するか、正規表現パターンを使って独自のルールを作成することができます。データは、処理の中で取り込みが行われる際にスキャンされます。ログの場合、これはインデックス化やその他のルーティング関連の決定の前にスキャンが行われることを意味します。

Terraform に関しては、[Datadog Sensitive Data Scanner ルール][2]のリソースを参照してください。

スキャンルールを追加するには、以下の手順を実行します。

1. [Sensitive Data Scanner][1] 構成ページに移動します。
1. Click the scanning group where you want to add the scanning rules.
1. Click **Add Scanning Rule**. Alternatively, click the **Add** dropdown menu on the top right corner of the page and select **Add Scanning Rule**.
1. Select whether you want to add a library rule or create a custom scanning rule.

{{< collapse-content title="ライブラリルールからスキャンルールを追加" level="p" >}}

The Scanning Rule Library contains predefined rules for detecting common patterns such as email addresses, credit card numbers, API keys, authorization tokens, and more.

1. In the **Add library rules to the scanning group** section, select the library rules you want to use.
1. In the **Define rule target and action** section, select if you want to scan the **Entire Event** or **Specific Attributes**.
    - イベント全体をスキャンする場合、オプションで特定の属性をスキャン対象から除外できます。
    - 特定の属性をスキャンする場合は、スキャンする属性を指定します。
{{% sds-scanning-rule %}}
1. Click **Add Rules**.

{{< /collapse-content >}}
{{< collapse-content title="カスタムスキャンルールの追加" level="p" >}}
正規表現パターンを使用してカスタムスキャンルールを作成し、機密データをスキャンできます。

1. In the **Define match conditions** section, specify the regex pattern to use for matching against events in the **Define regex** field. Enter sample data in the **Regex tester** field to verify that your regex pattern is valid.
    Sensitive Data Scanner supports Perl Compatible Regular Expressions (PCRE), but the following patterns are not supported:
    - Backreferences and capturing sub-expressions (lookarounds)
    - Arbitrary zero-width assertions
    - Subroutine references and recursive patterns
    - Conditional patterns
    - Backtracking control verbs
    - The `\C` "single-byte" directive (which breaks UTF-8 sequences)
    - The `\R` newline match
    - The `\K` start of match reset directive
    - Callouts and embedded code
    - Atomic grouping and possessive quantifiers
{{% sds-scanning-rule %}}
1. Click **Add Rule**.
{{< /collapse-content >}}

**Notes**:

- 追加または更新するルールは、ルールが定義された後に Datadog に送られるデータにのみ影響します。
- Sensitive Data Scanner does not affect any rules you define on the Datadog Agent directly.
- ルールが追加されたら、スキャングループのトグルが有効になっていることを確認してスキャンを開始します。

機密データの問題をトリアージするための [Summary][4] ページの使い方について、詳しくは[機密データの問題を調査する][3]を参照してください。

[1]: https://app.datadoghq.com/organization-settings/sensitive-data-scanner/configuration
[2]: https://registry.terraform.io/providers/DataDog/datadog/latest/docs/resources/sensitive_data_scanner_rule
[3]: /ja/sensitive_data_scanner/investigate_sensitive_data_issues/
[4]: https://app.datadoghq.com/organization-settings/sensitive-data-scanner/summary

{{% /tab %}}
{{% tab "Agent の使用" %}}

スキャンルールは、スキャングループによって定義されたデータ内のどの機密情報を対象にするかを決定します。Datadog Agent は、ログが Datadog プラットフォームに送信される前に、ログ収集中にローカル環境でデータをスキャンします。

<div class="alert alert-warning"><strong>注</strong>: Sensitive Data Scanner using the Agent は、Datadog のスキャンルールライブラリから定義済みのスキャンルールのみをサポートします。スキャンルールの総数は 20 個に制限されています。</div>

スキャンルールを追加するには、以下の手順を実行します。

1. [Sensitive Data Scanner using the Agent][1] の構成ページに移動します。
1. Click **Add Scanning Rule**. Alternatively, click the **Add** dropdown menu on the top right corner of the page and select **Add Scanning Rule**.
1. **Add library rules to the scanning group** セクションで、使用するライブラリルールを選択します。既存のライブラリルールを検索するには、**Filter library rules** 入力を使用します。ルール名の横には、各ルールの定義済みタグのリストがあります。
1. **Define rule target and action** セクションで、一致した機密情報に対して実行するアクションを選択します。**注**: マスキング、部分的なマスキング、およびハッシュ化はすべて不可逆アクションです。
    - **Redact**: Replaces all matching values with the text you specify in the **Replacement text** field.
    - **Partially Redact**: Replaces a specified portion of all matched data. In the **Redact** section, specify the number of characters you want to redact and which part of the matched data to redact.
    - **Hash**: Replaces all matched data with a unique identifier. The UTF-8 bytes of the match is hashed with the 64-bit fingerprint of FarmHash.
1. オプションとして、指定された正規表現パターンに一致する値を含むイベントに関連付ける追加タグを追加します。Datadog は、デフォルトで `sensitive_data` および `sensitive_data_category` のタグを追加します。これらのタグは、検索、ダッシュボード、およびモニターで使用できます。タグを利用して機密情報を含むログへのアクセス権を決定する方法については、[機密データを含むログへのアクセス制御](#control-access-to-logs-with-sensitive-data)を参照してください。
1. Click **Save**.

**Notes**:

- 追加または更新するルールは、ルールが定義された後に Datadog に送られるデータにのみ影響します。
- ルールが追加されたら、スキャングループのトグルが有効になっていることを確認してスキャンを開始します。

[1]: https://app.datadoghq.com/organization-settings/sensitive-data-scanner/configuration/agent
[2]: https://registry.terraform.io/providers/DataDog/datadog/latest/docs/resources/sensitive_data_scanner_rule
{{% /tab %}}
{{< /tabs >}}

#### 除外されるネームスペース

Datadog プラットフォームが機能上必要とする予約キーワードがあります。これらの単語がスキャンされるログ内にある場合、一致した単語の後の 30 文字は無視され、マスキングされません。例えば、ログの `date` という単語の後に来るのは通常イベントのタイムスタンプです。もしタイムスタンプが誤ってマスキングされると、ログの処理に問題が生じ、後でクエリを実行することができなくなります。そのため、除外されるネームスペースの動作は、製品機能にとって重要な情報が意図せずにマスキングされるのを防ぐためのものです。

除外されるネームスペースは以下のとおりです。

- `host`
- `hostname`
- `syslog.hostname`
- `service`
- `status`
- `env`
- `dd.trace_id`
- `trace_id`
- `trace id`
- `dd.span_id`
- `span_id`
- `span id`
- `@timestamp`
- `timestamp`
- `_timestamp`
- `Timestamp`
- `date`
- `published_date`
- `syslog.timestamp`
- `error.fingerprint`
- `x-datadog-parent-id`

### スキャンルールの編集

{{< tabs >}}
{{% tab "クラウド" %}}

1. [Sensitive Data Scanner][1] 構成ページに移動します。
1. 編集するスキャンルールにカーソルを合わせ、**Edit** (鉛筆) アイコンをクリックします。

   **Define match conditions** セクションには、カスタムルールに記述した正規表現、または選択したライブラリスキャンルールの説明が、一致した機密情報の例と共に表示されます。
1. ルールがデータに一致することを確認するには、**Add sample data** セクションにサンプルを提供します。ルールがサンプルデータに一致すると、入力フィールドの横に緑色の **Match** ラベルが表示されます。
1. **Create keyword dictionary** では、検出精度を向上させるためのキーワードを追加できます。例えば、16 桁の Visa クレジットカード番号をスキャンする場合、`visa`、`credit`、`card` のようなキーワードを追加できます。
1. キーワードが一致する前に出現しなければならない文字数を選択します。デフォルトでは、キーワードは一致する前の 30 文字以内に出現する必要があります。
1. オプションとして、**Define rule target and action** で、ルールに一致する値を含むイベントに関連付けるタグを編集します。Datadog では、検索、ダッシュボード、およびモニターで使用できる `sensitive_data` および `sensitive_data_category` のタグの使用を推奨しています。タグを利用して機密データを含むログへのアクセス権を決定する方法については、[機密データを含むログへのアクセス制御](#control-access-to-logs-with-sensitive-data)を参照してください。
1. **Set priority level** では、ビジネスニーズに基づいて値を選択します。
1. Click **Update**.

[1]: https://app.datadoghq.com/organization-settings/sensitive-data-scanner/configuration
{{% /tab %}}
{{% tab "Agent の使用" %}}

1. [Sensitive Data Scanner using the Agent][1] の構成ページに移動します。
1. 編集するスキャンルールにカーソルを合わせ、**Edit** (鉛筆) アイコンをクリックします。

   **Define match conditions** セクションには、選択したライブラリスキャンルールの説明が、一致した機密情報の例と共に表示されます。
1. **Create keyword dictionary** では、検出精度を向上させるためのキーワードを追加できます。例えば、16 桁の Visa クレジットカード番号をスキャンする場合、`visa`、`credit`、`card` のようなキーワードを追加できます。
1. キーワードが一致する前に出現しなければならない文字数を選択します。デフォルトでは、キーワードは一致する前の 30 文字以内に出現する必要があります。
1. Click **Save**.

[1]: https://app.datadoghq.com/organization-settings/sensitive-data-scanner/configuration/agent
{{% /tab %}}
{{< /tabs >}}


### Control access to logs with sensitive data

To control who can access logs containing sensitive data, use tags added by the Sensitive Data Scanner to build queries with role-based access control (RBAC). You can restrict access to specific individuals or teams until the data ages out after the retention period. See [How to Set Up RBAC for Logs][10] for more information.

### Redact sensitive data in tags

{{< tabs >}}
{{% tab "クラウド" %}}
タグに含まれる機密データを秘匿化するには、タグを属性に[リマップ][2]してから、その属性を秘匿化する必要があります。リマッピング中にタグが保存されないように、リマッパープロセッサーで `Preserve source attribute` のチェックを外してください。

To remap the tag to an attribute:

1. [ログパイプライン][3]に移動します。
2. Click **Add Processor**.
3. Select **Remapper** in the processor type dropdown menu.
4. Name the processor.
5. Select **Tag key(s)**.
6. Enter the tag key.
7. Enter a name for the attribute the tag key is remapped to.
8. Disable **Preserve source attribute**.
9. Click **Create**.

To redact the attribute:

1. [スキャングループ][1]に移動します。
2. Click **Add Scanning Rule**.
3. Check the library rules you want to use.
4. Select **Specific Attributes** for **Scan entire event or portion of it**.
5. Enter the name of the attribute you created earlier to specify that you want it scanned.
6. Select the action you want when there's a match.
7. オプションで、タグを追加します。
8. Click **Add Rules**.

[1]: https://app.datadoghq.com/organization-settings/sensitive-data-scanner/configuration
[2]: /ja/logs/log_configuration/processors/?tab=ui#remapper
[3]: https://app.datadoghq.com/logs/pipelines
{{% /tab %}}
{{% tab "Agent の使用" %}}
この関数は Sensitive Data Scanner using the Agent では使用できません。
{{% /tab %}}
{{< /tabs >}}

## Data Security

<div class="alert alert-warning">Data Security は非公開ベータ版です。非公開ベータ版に登録するには、<a href="https://www.datadoghq.com/private-beta/data-security">こちらからサインアップ</a>してください。</div>

[Sensitive Data Scanner][8] と [Cloud Security Management][16] を有効にしている場合、Data Security を使用して機密データを特定し、AWS S3 バケットと RDS インスタンスに影響するセキュリティ問題を修正できます。

Data Security は、クラウド環境に[エージェントレススキャナー][17]をデプロイすることで、機密データをスキャンします。これらのスキャンインスタンスは、[Remote Configuration][18] を介してすべての S3 バケットと RDS インスタンスのリストを取得し、すべてのデータストア内の CSV や JSON などのテキストファイルとテーブルを時間経過とともにスキャンするように設定されています。Data Security は、Sensitive Data Scanner が提供するルールを活用して一致するものを見つけます。一致するものが見つかると、その場所がスキャンインスタンスから Datadog に送信されます。データストアとそのファイルは、お客様の環境でのみ読み取られ、機密データが Datadog に返送されることはありません。

機密データとの一致を表示すると同時に、Data Security は、機密データストアに影響を与える Cloud Security Management によって検出されたセキュリティ問題も表示します。問題をクリックすると、Cloud Security Management 内でトリアージと修復を続けることができます。

## Out-of-the-box dashboard

Sensitive Data Scanner を有効にすると、[すぐに使えるダッシュボード][13]が自動的にアカウントにインストールされ、機密データの診断結果の要約を見ることができます。このダッシュボードにアクセスするには、**Dashboards > Dashboards List** に移動し、 "Sensitive Data Scanner Overview" を検索します。

{{<img src="sensitive_data_scanner/sdslight.png" alt="Sensitive Data Scanner Overview ダッシュボード" style="width:80%;">}}

## Sensitive Data Scanner を無効にする

Sensitive Data Scanner を完全にオフにするには、各スキャングループのトグルを **off** に設定して無効化します。

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/data_security/pci_compliance/
[2]: /ja/account_management/rbac/permissions/#compliance
[3]: /ja/account_management/rbac/
[4]: /ja/logs/explorer/search_syntax/
[5]: https://registry.terraform.io/providers/DataDog/datadog/latest/docs/resources/sensitive_data_scanner_group
[6]: https://app.datadoghq.com/organization-settings/sensitive-data-scanner/configuration
[7]: https://registry.terraform.io/providers/DataDog/datadog/latest/docs/resources/sensitive_data_scanner_rule
[8]: /ja/sensitive_data_scanner/investigate_sensitive_data_issues/
[9]: https://app.datadoghq.com/organization-settings/sensitive-data-scanner/summary
[10]: /ja/logs/guide/logs-rbac/
[11]: /ja/logs/log_configuration/processors/?tab=ui#remapper
[12]: https://app.datadoghq.com/logs/pipelines
[13]: https://app.datadoghq.com/dash/integration/sensitive_data_scanner
[14]: /ja/observability_pipelines/sensitive_data_redaction/
[16]: /ja/security/cloud_security_management
[17]: /ja/security/cloud_security_management/setup/agentless_scanning
[18]: /ja/agent/remote_config
