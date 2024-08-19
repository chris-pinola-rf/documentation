---
algolia:
  tags:
  - データベースモニタリング
  - dbm
cascade:
  algolia:
    rank: 70
description: Database Monitoring について学び、始めましょう
further_reading:
- link: https://www.datadoghq.com/blog/database-performance-monitoring-datadog
  tag: ブログ
  text: データベースのパフォーマンスを監視、視覚化する
- link: https://www.datadoghq.com/blog/sql-server-and-azure-managed-services-database-monitoring/
  tag: ブログ
  text: Datadog DBM で SQL Server や Azure のマネージドデータベースを監視する
- link: /database_monitoring/data_collected/
  tag: ドキュメント
  text: 収集データ
- link: /database_monitoring/troubleshooting/
  tag: ドキュメント
  text: トラブルシューティング
- link: https://dtdg.co/fe
  tag: Foundation Enablement
  text: データベースモニタリングのレベルアップのためのインタラクティブなセッションに参加できます
title: データベース モニタリング
---


{{< learning-center-callout header="イネーブルメントウェビナーセッションに参加" hide_image="true" btn_title="登録" btn_url="https://www.datadoghq.com/technical-enablement/sessions/?tags.topics-0=Database">}}
  Database Monitoring を使って、コストのかかるクエリや遅いクエリをすばやく特定する方法を学びましょう。ボトルネックに対処するために、実行の詳細を正確に調べましょう。
{{< /learning-center-callout >}}

Datadog Database Monitoring provides deep visibility into databases across all of your hosts. Dig into historical query performance metrics, explain plans, and host-level metrics all in one place, to understand the health and performance of your databases and troubleshoot issues as they arise.

## Getting started

Datadog Database Monitoring は、**Postgres**、**MySQL**、**Oracle**、**SQL Server**、**MongoDB** のセルフホストおよびマネージドクラウドバージョンをサポートします。Datadog Database Monitoring の使用を開始するには、データベースを構成し、Datadog Agent をインストールします。セットアップ手順については、データベーステクノロジーを選択してください。

### Postgres

{{< partial name="dbm/dbm-setup-postgres" >}}
<p></p>

### MySQL

{{< partial name="dbm/dbm-setup-mysql" >}}
<p></p>

### Oracle

{{< partial name="dbm/dbm-setup-oracle" >}}
<p></p>

### SQL Server

{{< partial name="dbm/dbm-setup-sql-server" >}}
<p></p>

### MongoDB

<div class="alert alert-info">Database Monitoring for MongoDB is in public beta. If you are interested in participating, reach out to your Datadog Customer Success Manager.</div>
{{< partial name="dbm/dbm-setup-mongodb" >}}
<p></p>

## Explore Datadog Database Monitoring

Navigate to [Database Monitoring][1] in Datadog.

### Dig into query performance metrics

The [Query Metrics view][2] shows historical query performance for normalized queries. Visualize performance trends by infrastructure or custom tags such as datacenter availability zone, and set alerts for anomalies.

- Identify slow queries and which queries are consuming the most time.
- Show database-level metrics not captured by APM such as rows updated/returned.
- Filter and group queries by arbitrary dimensions such as team, user, cluster, and host.

{{< img src="database_monitoring/dbm-query-metrics-2.png" alt="Database Monitoring" style="width:100%;">}}

### Explore query samples

The [Query Samples view][3] helps you understand which queries are running at a given time. Compare each execution to the average performance of the query and related queries.

- Identify unusually slow but infrequent queries not captured by metrics.
- Find outliers in a query's execution time or execution cost.
- Attribute a specific query execution to a user, application, or client host.

{{< img src="database_monitoring/dbm-query-sample-2.png" alt="Database Monitoring" style="width:100%;">}}

### Understand before you run

[Explain Plans][4] help you understand how the database plans to execute your queries.

- Step through each operation to identify bottlenecks.
- Improve query efficiency and save on costly sequential scans on large tables.
- See how a query's plan changes over time.

{{< img src="database_monitoring/dbm-explain-plan-3.png" alt="Database Monitoring" style="width:100%;">}}

### Visualize everything on enriched dashboards

Quickly pinpoint problem areas by viewing database and system metrics together on enriched integration dashboards for both self-hosted and cloud-managed instances. Clone dashboards for customization and enhancement with your own custom metrics. Click the **Dashboards** link at the top of the Query Metrics and Query Samples pages to go to the Database Monitoring dashboards.

{{< img src="database_monitoring/dbm-dashboard-postgres.png" alt="Database Monitoring" style="width:100%;">}}

### Optimize host health and performance

On the [Databases page][1], you can assess the health and activity of your database hosts. Sort and filter the list to prioritize hosts with triggered alerts, high query volume, and other criteria. Click on an individual host to view details such as its configuration, common blocking queries, and calling services. See [Exploring Database Hosts][5] for details.

{{< img src="database_monitoring/databases-list.png" alt="The Databases page in Datadog" style="width:90%;" >}}

## Further Reading

{{< learning-center-callout header="Try Monitoring a Postgres Database with Datadog DBM in the Learning Center" btn_title="Enroll Now" btn_url="https://learn.datadoghq.com/courses/database-monitoring">}}
  The Datadog Learning Center is full of hands-on courses to help you learn about this topic. Enroll at no cost to identify inefficiencies and optimize your Postgres database.
{{< /learning-center-callout >}}

{{< partial name="whats-next/whats-next.html" >}}

[1]: https://app.datadoghq.com/databases
[2]: /ja/database_monitoring/query_metrics/
[3]: /ja/database_monitoring/query_samples/
[4]: /ja/database_monitoring/query_metrics/#explain-plans
[5]: /ja/database_monitoring/database_hosts/