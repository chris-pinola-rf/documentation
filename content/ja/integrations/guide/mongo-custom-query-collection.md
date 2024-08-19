---
further_reading:
- link: /ja/integrations/mongo/
  tag: ドキュメント
  text: MongoDB インテグレーションについて
title: MongoDB カスタムメトリクスを収集
---

## Overview

[MongoDB インテグレーション][8]でカスタムメトリクスを収集するには、[Agent の構成ディレクトリ][1]のルートにある `conf.d/mongo.d/conf.yaml` ファイルの `custom_queries` オプションを使用します。詳細については、サンプルの [mongo.d/conf.yaml][2] を参照してください。

## Configuration

`custom_queries` has the following options:

* **`metric_prefix`**: Each metric starts with the chosen prefix.
* **`query`**: This is the [Mongo runCommand][3] query to execute as a JSON object. **Note**: The Agent only supports `count`, `find`, and `aggregates` queries.
* **`database`**: メトリクスを収集する MongoDB データベースです。
* **`fields`**: Ignored for `count` queries. This is a list representing each field with no specific order. Ignores unspecified and missing fields. There are three required pieces of data for each `fields`:
  * `field_name`: This is the name of the field from which to fetch the data.
  * `name`: This is the suffix to append to the metric_prefix to form the full metric name. If `type` is `tag`, this column is treated as a tag and applied to every metric collected by this particular query.
  * `type`: 送信メソッド (`gauge`、`count`、`rate` など)。これを `tag` に設定して、行の各メトリクスにこの列の項目の名前と値でタグ付けすることもできます。`count` タイプを使用して、同じタグを持つか、タグのない複数の行を返すクエリの集計を実行できます。
* **`tags`**: A list of tags to apply to each metric (as specified above).
* **`count_type`**: `count` クエリに対してのみ、カウント結果を送信するメソッド (`gauge`、`count`、`rate` など) として機能します。非カウントクエリでは無視されます。

## Examples

以下の例では、次の Mongo コレクション `user_collection` が使用されます。

```text
{ name: "foo", id: 12345, active: true, age:45, is_admin: true}
{ name: "bar", id: 67890, active: false, age:25, is_admin: true}
{ name: "foobar", id: 16273, active: true, age:35, is_admin: false}
```

Choose the type of query you would like to see an example for:

{{< tabs >}}
{{% tab "Count" %}}

To monitor how many users are active at a given time, your [Mongo count command][1] would be:

```text
db.runCommand( {count: user_collection, query: {active:true}})
```

Which would correspond to the following `custom_queries` YAML configuration inside your `mongo.d/conf.yaml` file:

```yaml
custom_queries:
  - metric_prefix: mongo.users
    query: {"count": "user_collection", "query": {"active":"true"}}
    count_type: gauge
    tags:
      - user:active
```

これにより、`user:active` の 1 つのタグを持つ 1 つの `gauge` メトリクス `mongo.users` が生成されます。

**Note**: The [metric type][2] defined is `gauge`.

[1]: https://docs.mongodb.com/manual/reference/command/count/#dbcmd.count
[2]: /ja/metrics/types/
{{% /tab %}}
{{% tab "Find" %}}

To monitor the age per user, your [Mongo find command][1] would be:

```text
db.runCommand( {find: user_collection, filter: {active:true} )
```

Which would correspond to the following `custom_queries` YAML configuration inside your `mongo.d/conf.yaml` file:

```yaml
custom_queries:
  - metric_prefix: mongo.example2
    query: {"find": "user_collection", "filter": {"active":"true"}}
    fields:
      - field_name: name
        name: name
        type: tag
      - field_name: age
        name: user.age
        type: gauge

```

This would emit one `gauge` metric `mongo.example2.user.age` with two tags: `name:foo` and `name:foobar`

**Note**: The [metric type][2] defined is `gauge`.

[1]: https://docs.mongodb.com/manual/reference/command/find/#dbcmd.find
[2]: /ja/metrics/types/
{{% /tab %}}
{{% tab "Aggregate" %}}

To monitor the average age for an admin and a non-admin user, your [Mongo aggregate command][1] would be:

```text
db.runCommand(
              {
                'aggregate': "user_collection",
                'pipeline': [
                  {"$match": {"active": "true"}},
                  {"$group": {"_id": "$is_admin", "age_avg": {"$avg": "$age"}}}
                ],
                'cursor': {}
              }
            )
```

Which would correspond to the following `custom_queries` YAML configuration inside your `mongo.d/conf.yaml` file:

```yaml
custom_queries:
  - metric_prefix: mongo.example3
    query: {"aggregate": "user_collection","pipeline": [{"$match": {"active": "true"}},{"$group": {"_id": "$is_admin", "age_avg": {"$avg": "$age"}}}],"cursor": {}}
    fields:
      - field_name: age_avg
        name: user.age
        type: gauge
      - field_name: _id
        name: is_admin
        type: tag
    tags:
      - test:mongodb
```

This would emit one `gauge` metric `mongo.example3.user.age` with two tags: `is_admin:true` and `is_admin:false` representing the average age of users for each tags.

[1]: https://docs.mongodb.com/manual/reference/command/aggregate/#dbcmd.aggregate
{{% /tab %}}
{{< /tabs >}}

**Note**: After updating the Mongo YAML file, [restart the Datadog Agent][4].

### Validation

結果を確認するには、[メトリクスエクスプローラー][5]を使用してメトリクスを検索します。

### Debugging

[Run the Agent's status subcommand][6] and look for `mongo` under the Checks section. Additionally, the [Agent's logs][7] may provide useful information.

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/agent/guide/agent-configuration-files/#agent-configuration-directory
[2]: https://github.com/DataDog/integrations-core/blob/master/mongo/datadog_checks/mongo/data/conf.yaml.example
[3]: https://docs.mongodb.com/manual/reference/command
[4]: /ja/agent/guide/agent-commands/#restart-the-agent
[5]: /ja/metrics/explorer/
[6]: /ja/agent/guide/agent-commands/#agent-status-and-information
[7]: /ja/agent/guide/agent-log-files/
[8]: /ja/integrations/mongodb