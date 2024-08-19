---
description: セルフホストの Oracle のデータベースモニタリングのインストールと構成
further_reading:
- link: /integrations/oracle/
  tag: Documentation
  text: Basic Oracle インテグレーション
title: セルフホストの Oracle のデータベースモニタリングの設定
---

{{% dbm-oracle-definition %}}

Agent は読み取り専用ユーザーとしてデータベースにログインし、直接テレメトリーを収集します。

## はじめに

{{% dbm-supported-oracle-versions %}}

{{% dbm-supported-oracle-agent-version %}}

パフォーマンスへの影響
: Database Monitoring のデフォルトの Agent 構成は保守的ですが、収集間隔やクエリのサンプリングレートなどの設定を調整することで、よりニーズに合ったものにすることができます。ほとんどのワークロードにおいて、Agent がデータベース上のクエリ実行時間に与える影響は 1% 未満、CPU 使用率も 1% 未満です。<br/><br/>
Database Monitoring は、ベース Agent 上のインテグレーションとして動作します ([ベンチマークを参照][6])。

プロキシ、ロードバランサー、コネクションプーラー
: Agent は、監視対象のホストに直接接続する必要があります。Agent をプロキシ、ロードバランサー、コネクションプーラーを経由してデータベースに接続しないようご注意ください。また、各 Agent は基礎となるホスト名を把握し、フェイルオーバーの場合でも常に 1 つのホストのみを使用する必要があります。Datadog Agent が実行中に異なるホストに接続すると、メトリクス値の正確性が失われます。

データセキュリティへの配慮
: Agent がお客様のデータベースからどのようなデータを収集するか、またそのデータの安全性をどのように確保しているかについては、[機密情報][5]を参照してください。

## セットアップ

Oracle データベースで Database Monitoring を有効にするには、次の手順を完了してください。

1. [Datadog ユーザーの作成](#create-the-datadog-user)
1. [ユーザーにデータベースへのアクセス権を付与する](#grant-the-user-access-to-the-database)
1. [ビューの作成](#create-a-view)
1. [Agent をインストールする](#install-the-agent)
1. [Agent の構成](#configure-the-agent)
1. [Oracle インテグレーションをインストールまたは検証する](#install-or-verify-the-oracle-integration)
1. [セットアップの検証](#validate-the-setup)

### Datadog ユーザーの作成

レガシー Oracle インテグレーションがすでにインストールされている場合は、ユーザーがすでに存在するため、この手順をスキップします。

サーバーに接続するための読み取り専用ログインを作成し、必要な権限を付与します。

{{< tabs >}}
{{% tab "マルチテナント" %}}
```SQL
CREATE USER c##datadog IDENTIFIED BY &password CONTAINER = ALL ;

ALTER USER c##datadog SET CONTAINER_DATA=ALL CONTAINER=CURRENT;
```
{{% /tab %}}

{{% tab "Non-CDB" %}}
```SQL
CREATE USER datadog IDENTIFIED BY &password ;
```
{{% /tab %}}

{{% tab "Oracle 11" %}}
```SQL
CREATE USER datadog IDENTIFIED BY &password ;
```
{{% /tab %}}
{{< /tabs >}}

### ユーザーにデータベースへのアクセス権を付与する

`sysdba` としてログオンし、以下の権限を付与します。

{{< tabs >}}

{{% tab "マルチテナント" %}}
{{% dbm-oracle-multitenant-permissions-grant-sql %}}
{{% /tab %}}

{{% tab "非 CDB" %}}
{{% dbm-oracle-non-cdb-permissions-grant-sql %}}
{{% /tab %}}

{{% tab "Oracle 11" %}}
{{% dbm-oracle-11-permissions-grant-sql %}}
{{% /tab %}}

{{< /tabs >}}

### Securely store your password
{{% dbm-secret %}}

### Create a view

Log on as `sysdba`, create a new `view` in the `sysdba` schema, and give the Agent user access to it:

{{< tabs >}}

{{% tab "Multi-tenant" %}}
{{% dbm-multitenant-view-create-sql %}}
{{% /tab %}}

{{% tab "Non-CDB" %}}
{{% dbm-non-cdb-view-create-sql %}}
{{% /tab %}}

{{% tab "Oracle 11" %}}
{{% dbm-oracle-11-view-create-sql %}}
{{% /tab %}}

{{< /tabs >}}

### Install the Agent

For installation steps, see the [Agent installation instructions][1].

### Configure the Agent

Create the Oracle Agent conf file `/etc/datadog-agent/conf.d/oracle.d/conf.yaml`. See the [sample conf file][4] for all available configuration options.

**Note:** The configuration subdirectory for the Agent releases below `7.53.0` is `oracle-dbm.d`.

{{< tabs >}}
{{% tab "Multi-tenant" %}}
```yaml
init_config:
instances:
  - server: '<HOSTNAME_1>:<PORT>'
    service_name: "<CDB_SERVICE_NAME>" # The Oracle CDB service name
    username: 'c##datadog'
    password: 'ENC[datadog_user_database_password]'
    dbm: true
    tags:  # Optional
      - 'service:<CUSTOM_SERVICE>'
      - 'env:<CUSTOM_ENV>'
  - server: '<HOSTNAME_2>:<PORT>'
    service_name: "<CDB_SERVICE_NAME>" # The Oracle CDB service name
    username: 'c##datadog'
    password: 'ENC[datadog_user_database_password]'
    dbm: true
    tags:  # Optional
      - 'service:<CUSTOM_SERVICE>'
      - 'env:<CUSTOM_ENV>'
```

The Agent connects only to the root multitenant container database (CDB). It queries the information about PDB while connected to the root CDB. Don't create connections to individual PDBs.
{{% /tab %}}

{{% tab "Non-CDB" %}}
{{% dbm-oracle-selfhosted-config %}}
{{% /tab %}}

{{% tab "Oracle 11" %}}
{{% dbm-oracle-selfhosted-config %}}

{{% /tab %}}
{{< /tabs >}}

Once all Agent configuration is complete, [restart the Datadog Agent][9].

### Install or verify the Oracle integration

#### First-time installations

On the Integrations page in Datadog, install the [Oracle integration][7] for your organization. This installs an [Oracle dashboard][2] in your account that can be used to monitor the performance of your Oracle databases.

#### Existing installations

{{% dbm-existing-oracle-integration-setup %}}

### Validate the setup

[Run the Agent's status subcommand][8] and look for `oracle` under the **Checks** section. Navigate to the [Dashboard][2] and [Databases][3] page in Datadog to get started.

## Custom queries

Database Monitoring supports custom queries for Oracle databases. See the [conf.yaml.example][4] to learn more about the configuration options available.

<div class="alert alert-warning">Running custom queries may result in additional costs or fees assessed by Oracle.</div>

[1]: https://app.datadoghq.com/account/settings/agent/latest?platform=overview
[2]: https://app.datadoghq.com/dash/integration/30990/dbm-oracle-database-overview
[3]: https://app.datadoghq.com/databases
[4]: https://github.com/DataDog/datadog-agent/blob/main/cmd/agent/dist/conf.d/oracle.d/conf.yaml.example
[5]: /ja/database_monitoring/data_collected/#sensitive-information
[6]: /ja/database_monitoring/agent_integration_overhead/?tab=oracle
[7]: https://app.datadoghq.com/integrations/oracle
[8]: /ja/agent/configuration/agent-commands/#agent-status-and-information
[9]: /ja/agent/guide/agent-commands/#start-stop-and-restart-the-agent

## Further reading

{{< partial name="whats-next/whats-next.html" >}}