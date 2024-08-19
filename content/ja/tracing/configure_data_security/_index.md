---
aliases:
- /ja/tracing/security
- /ja/tracing/guide/security
- /ja/tracing/guide/agent_obfuscation
- /ja/tracing/guide/agent-obfuscation
- /ja/tracing/custom_instrumentation/agent_customization
- /ja/tracing/faq/if-i-instrument-a-database-with-datadog-apm-will-there-be-sensitive-database-data-sent-to-datadog
- /ja/tracing/setup_overview/configure_data_security/
description: クライアントライブラリまたは Agent を構成して、トレース内の機密データの収集を制御します。
further_reading:
- link: /data_security/pci_compliance/
  tag: ドキュメント
  text: PCI 準拠の Datadog 組織をセットアップする
title: データセキュリティ
---
## 概要

Datadog トレーシングライブラリは、インスツルメンテーションされたアプリケーションからデータを収集します。そのデータはトレースとして Datadog に送信され、個人を特定できる情報 (PII) のような機密データが含まれている可能性があります。機密データをトレースとして Datadog に取り込む場合は、[機密データスキャナー][12]を使って取り込み時に修復を追加することができます。また、トレースが Datadog に送信される前に、Datadog Agent またはトレーシングライブラリを構成して、機密データを収集時に修復することもできます。

ここで説明する構成がコンプライアンス要件をカバーしない場合は、[Datadog サポートチーム][1]にお問い合わせください。

### トレースデータに含まれる個人情報

Datadog の APM トレーシングライブラリは、アプリケーションに関する関連する観測可能性データを収集します。これらのライブラリは、トレースデータに含まれる何百もの一意の属性を収集するため、このページでは、従業員やエンドユーザーの個人情報を含む可能性のある属性を中心に、データのカテゴリーについて説明します。

以下の表は、トレーシングライブラリによって提供される自動インスツルメンテーションによって収集される個人データのカテゴリーについて、一般的な例を挙げて説明しています。

| カテゴリー            | 説明                                                                                                            |
|:--------------------|:-----------------------------------------------------------------------------------------------------------------------|
| 名前                | 社内ユーザー (従業員) またはエンドユーザーの氏名。                                                         |
| メール               | 社内ユーザー (従業員) またはエンドユーザーのメールアドレス。                                                     |
| クライアント IP           | 受信リクエストに関連するエンドユーザーの IP アドレス、または送信リクエストの外部 IP アドレス。 |
| データベースステートメント | 実行されたデータベースステートメントで使用されるリテラル、リテラル列、バインド変数。                           |
| 地理的位置情報 | 個人または世帯を特定するために使用できる経度と緯度の座標。                            |
| URI パラメーター      | URI パスまたは URI クエリの変数部分のパラメーター値。                                            |
| URI ユーザー情報        | ユーザー名を含むことができる URI のユーザー情報サブコンポーネント。                                                   |

以下の表は、データカテゴリーが収集されるかどうか、およびデフォルトで難読化されるかどうかに関する、各言語トレーシングライブラリのデフォルトの動作について説明しています。

{{% tabs %}}

{{% tab ".NET" %}}

| カテゴリー            | 収集                       | 難読化                      |
|:--------------------|:-------------------------------:|:-------------------------------:|
| 名前                | <i class="icon-check-bold"></i> |                                 |
| メール               | <i class="icon-check-bold"></i> |                                 |
| クライアント IP           | <i class="icon-check-bold"></i> |                                 |
| データベースステートメント | <i class="icon-check-bold"></i> | <i class="icon-check-bold"></i> |
| 地理的位置情報 |                                 |                                 |
| URI パラメーター      | <i class="icon-check-bold"></i> |                                 |
| URI ユーザー情報        |                                 |                                 |

{{% /tab %}}

{{% tab "Java" %}}

**注:** データベースステートメントはデフォルトでは収集されないため、有効にする必要があります。

| カテゴリー            | 収集                       | 難読化                      |
|:--------------------|:-------------------------------:|:-------------------------------:|
| 名前                | <i class="icon-check-bold"></i> |                                 |
| メール               | <i class="icon-check-bold"></i> |                                 |
| クライアント IP           | <i class="icon-check-bold"></i> | <i class="icon-check-bold"></i> |
| データベースステートメント | <i class="icon-check-bold"></i> |                                 |
| 地理的位置情報 |                                 |                                 |
| URI パラメーター      | <i class="icon-check-bold"></i> | <i class="icon-check-bold"></i> |
| URI ユーザー情報        |                                 |                                 |

{{% /tab %}}

{{% tab "Node.js" %}}

**注:** URI パラメーターはデフォルトでは収集されないため、有効にする必要があります。

| カテゴリー            | 収集                       | 難読化                      |
|:--------------------|:-------------------------------:|:-------------------------------:|
| 名前                | <i class="icon-check-bold"></i> |                                 |
| メール               | <i class="icon-check-bold"></i> |                                 |
| クライアント IP           | <i class="icon-check-bold"></i> |                                 |
| データベースステートメント | <i class="icon-check-bold"></i> |                                 |
| 地理的位置情報 |                                 |                                 |
| URI パラメーター      | <i class="icon-check-bold"></i> | <i class="icon-check-bold"></i> |
| URI ユーザー情報        |                                 |                                 |

{{% /tab %}}

{{% tab "PHP" %}}

**注:** 名前とメールアドレスはデフォルトでは収集されないため、有効にする必要があります。

| カテゴリー            |            収集            |           難読化            |
|:--------------------|:-------------------------------:|:-------------------------------:|
| 名前                | <i class="icon-check-bold"></i> |                                 |
| メール               | <i class="icon-check-bold"></i> |                                 |
| クライアント IP           | <i class="icon-check-bold"></i> |                                 |
| データベースステートメント | <i class="icon-check-bold"></i> | <i class="icon-check-bold"></i> |
| 地理的位置情報 |                                 |                                 |
| URI パラメーター      | <i class="icon-check-bold"></i> | <i class="icon-check-bold"></i> |
| URI ユーザー情報        | <i class="icon-check-bold"></i> | <i class="icon-check-bold"></i> |

{{% /tab %}}

{{% tab "Python" %}}

**注:** クライアント IP、地理的位置、および URI パラメーターはデフォルトでは収集されないため、有効にする必要があります。

| カテゴリー            | 収集                       | 難読化                      |
|:--------------------|:-------------------------------:|:-------------------------------:|
| 名前                | <i class="icon-check-bold"></i> |                                 |
| メール               | <i class="icon-check-bold"></i> |                                 |
| クライアント IP           | <i class="icon-check-bold"></i> |                                 |
| データベースステートメント | <i class="icon-check-bold"></i> | <i class="icon-check-bold"></i> |
| 地理的位置情報 | <i class="icon-check-bold"></i> |                                 |
| URI パラメーター      | <i class="icon-check-bold"></i> | <i class="icon-check-bold"></i> |
| URI ユーザー情報        |                                 |                                 |

[1]: /ja/tracing/trace_collection/compatibility/python/#datastore-compatibility
{{% /tab %}}

{{% tab "Ruby" %}}

**注:** クライアント IP はデフォルトでは収集されないため、有効にする必要があります。

| カテゴリー            | 収集                       | 難読化                      |
|:--------------------|:-------------------------------:|:-------------------------------:|
| 名前                | <i class="icon-check-bold"></i> |                                 |
| メール               | <i class="icon-check-bold"></i> |                                 |
| クライアント IP           | <i class="icon-check-bold"></i> |                                 |
| データベースステートメント | <i class="icon-check-bold"></i> |                                 |
| 地理的位置情報 |                                 |                                 |
| URI パラメーター      | <i class="icon-check-bold"></i> | <i class="icon-check-bold"></i> |
| URI ユーザー情報        |                                 |                                 |

{{% /tab %}}

{{% tab "Go" %}}

**注:** クライアント IP はデフォルトでは収集されないため、有効にする必要があります。データベースステートメントは、Datadog Agent によって難読化されます。

| カテゴリー                | 収集                       | 難読化                      |
|:------------------------|:-------------------------------:|:-------------------------------:|
| 名前                    |                                 |                                 |
| メール                   |                                 |                                 |
| クライアント IP               | <i class="icon-check-bold"></i> |                                 |
| データベースステートメント     | <i class="icon-check-bold"></i> |                                 |
| 地理的位置情報     |                                 |                                 |
| クライアント URI パス         | <i class="icon-check-bold"></i> |                                 |
| クライアント URI クエリ文字列 | <i class="icon-check-bold"></i> |                                 |
| サーバー URI パス         | <i class="icon-check-bold"></i> | <i class="icon-check-bold"></i> |
| サーバー URI クエリ文字列 | <i class="icon-check-bold"></i> | <i class="icon-check-bold"></i> |
| HTTP 本文               | <i class="icon-check-bold"></i> | <i class="icon-check-bold"></i> |
| HTTP クッキー            | <i class="icon-check-bold"></i> | <i class="icon-check-bold"></i> |
| HTTP ヘッダー            | <i class="icon-check-bold"></i> | <i class="icon-check-bold"></i> |

{{% /tab %}}

{{% tab "Nginx" %}}

| カテゴリー                | 収集                       | 難読化 |
|:------------------------|:-------------------------------:|:----------:|
| 名前                    |                                 |            |
| メール                   |                                 |            |
| クライアント IP               | <i class="icon-check-bold"></i> |            |
| データベースステートメント     |                                 |            |
| 地理的位置情報     |                                 |            |
| クライアント URI パス         | <i class="icon-check-bold"></i> |            |
| クライアント URI クエリ文字列 | <i class="icon-check-bold"></i> |            |
| サーバー URI パス         |                                 |            |
| サーバー URI クエリ文字列 |                                 |            |
| HTTP 本文               |                                 |            |
| HTTP クッキー            |                                 |            |
| HTTP ヘッダー            |                                 |            |

{{% /tab %}}

{{% tab "Kong" %}}

| カテゴリー                | 収集                       | 難読化 |
|:------------------------|:-------------------------------:|:----------:|
| 名前                    |                                 |            |
| メール                   |                                 |            |
| クライアント IP               | <i class="icon-check-bold"></i> |            |
| データベースステートメント     |                                 |            |
| 地理的位置情報     |                                 |            |
| クライアント URI パス         | <i class="icon-check-bold"></i> |            |
| クライアント URI クエリ文字列 |                                 |            |
| サーバー URI パス         |                                 |            |
| サーバー URI クエリ文字列 |                                 |            |
| HTTP 本文               |                                 |            |
| HTTP クッキー            |                                 |            |
| HTTP ヘッダー            |                                 |            |

{{% /tab %}}

{{% tab "Envoy" %}}

| カテゴリー                | 収集                       | 難読化 |
|:------------------------|:-------------------------------:|:----------:|
| 名前                    |                                 |            |
| メール                   |                                 |            |
| クライアント IP               | <i class="icon-check-bold"></i> |            |
| データベースステートメント     |                                 |            |
| 地理的位置情報     |                                 |            |
| クライアント URI パス         |                                 |            |
| クライアント URI クエリ文字列 |                                 |            |
| サーバー URI パス         |                                 |            |
| サーバー URI クエリ文字列 |                                 |            |
| HTTP 本文               |                                 |            |
| HTTP クッキー            |                                 |            |
| HTTP ヘッダー            |                                 |            |

{{% /tab %}}

{{% /tabs %}}

Datadog Application Security Management (ASM) を使用している場合、トレーシングライブラリは HTTP リクエストデータを収集し、セキュリティトレースの性質を理解するのに役立ちます。Datadog ASM は特定のデータを自動的に削除し、独自の検出ルールを構成することができます。これらのデフォルトと構成オプションの詳細については、 Datadog ASM [データプライバシー][13]のドキュメントを参照してください。

## Agent

### リソース名

Datadog のスパンには、機密データを含む可能性のあるリソース名属性が含まれています。Datadog Agent は、いくつかの既知のケースに対してリソース名の難読化を実装しています。

* **SQL numeric literals and bind variables are obfuscated** (SQL の数値リテラルとバインド変数は難読化されます): 例えば、以下のクエリ `SELECT data FROM table WHERE key=123 LIMIT 10` は、クエリスパンのリソース名を設定する前に、`SELECT data FROM table WHERE key = ? LIMIT ?` に難読化されます。
* **SQL literal strings are identified using standard ANSI SQL quotes** (SQL リテラル文字列は、標準的な ANSI SQL 引用符**を使用して識別されます): これは、文字列はシングルクォート (`'`) で囲まなければならないことを意味します。SQL の種類によっては、オプションで文字列のダブルクォート（`"`）をサポートしていますが、ほとんどの場合、ダブルクォートされたものは識別子として扱われます。Datadog の難読化ツールは、これらを文字列ではなく識別子として扱い、難読化を行いません。
* **Redis queries are quantized by selecting only command tokens** (Redis のクエリはコマンドトークンのみを選択して量子化されます): 例えば、以下のクエリ `MULTI\nSET k1 v1\nSET k2 v2` は `MULTI SET SET` に量子化されます。

### トレースの難読化

Datadog Agent は、リソース名に含まれない機密[トレース][2]データも難読化します。難読化ルールは、環境変数または `datadog.yaml` コンフィギュレーションファイルを使用して構成できます。

以下のメタデータを難読化できます。

* MongoDB クエリ
* ElasticSearch リクエスト本文
* Redis コマンド
* MemCached コマンド
* HTTP URL
* スタックトレース

**注:** 難読化はシステムのパフォーマンスに影響を与えたり、機密情報ではない重要な情報を隠したりする可能性があります。セットアップにどのような難読化が必要かを検討し、適切に構成をカスタマイズしてください。

**注:** 複数のタイプのサービスに、同時に自動スクラビングを使用することができます。`datadog.yaml` ファイルの `obfuscation` セクションでそれぞれを構成します。
{{< tabs >}}

{{% tab "MongoDB" %}}

`mongodb` 型の[スパン][1]内の MongoDB クエリはデフォルトで難読化されます。

```yaml
apm_config:
  enabled: true

  ## (...)

  obfuscation:
    mongodb:
      ## "mongodb" 型のスパンに対する難読化ルールを構成します。デフォルトでは有効になっています。
      enabled: true
      keep_values:
        - document_id
        - template_id
      obfuscate_sql_values:
        - val1
```

これは環境変数 `DD_APM_OBFUSCATION_MONGODB_ENABLED=false` で無効にすることもできます。

* `keep_values` または環境変数 `DD_APM_OBFUSCATION_MONGODB_KEEP_VALUES` - Datadog Agent のトレース難読化から除外するキーのセットを定義します。設定しない場合は、すべてのキーが難読化されます。
* `obfuscate_sql_values` または環境変数 `DD_APM_OBFUSCATION_MONGODB_OBFUSCATE_SQL_VALUES` - Datadog Agent のトレース難読化に含めるキーのセットを定義します。設定しない場合は、すべてのキーが難読化されます。

[1]: /ja/tracing/glossary/#spans
{{% /tab %}}
{{% tab "ElasticSearch" %}}

`elasticsearch` 型の[スパン][1]内の ElasticSearch リクエスト本文はデフォルトで難読化されます。

```yaml
apm_config:
  enabled: true

  ## (...)

  obfuscation:
    elasticsearch:
      ## "elasticsearch" 型のスパンに対する難読化ルールを構成します。デフォルトでは有効になっています。
      enabled: true
      keep_values:
        - client_id
        - product_id
      obfuscate_sql_values:
        - val1
```

これは環境変数 `DD_APM_OBFUSCATION_ELASTICSEARCH_ENABLED=false` で無効にすることもできます。

* `keep_values` または環境変数 `DD_APM_OBFUSCATION_ELASTICSEARCH_KEEP_VALUES` - Datadog Agent のトレース難読化から除外するキーのセットを定義します。設定しない場合は、すべてのキーが難読化されます。
* `obfuscate_sql_values` または環境変数 `DD_APM_OBFUSCATION_ELASTICSEARCH_OBFUSCATE_SQL_VALUES` - Datadog Agent のトレース難読化に含めるキーのセットを定義します。設定しない場合は、すべてのキーが難読化されます。

[1]: /ja/tracing/glossary/#spans
{{% /tab %}}
{{% tab "Redis" %}}

`redis` 型の[スパン][1]内の Redis コマンドはデフォルトで難読化されます。

```yaml
apm_config:
  enabled: true

  ## (...)

  obfuscation:
    ## "redis" 型のスパンに対する難読化ルールを構成します。デフォルトでは有効になっています。
    redis:
      enabled: true
      remove_all_args: true
```

これは環境変数 `DD_APM_OBFUSCATION_REDIS_ENABLED=false` で無効にすることもできます。

* `remove_all_args` または環境変数 `DD_APM_OBFUSCATION_REDIS_REMOVE_ALL_ARGS` - true の場合、redis コマンドのすべての引数を単一の "?" に置き換えます。デフォルトでは無効です。

[1]: /ja/tracing/glossary/#spans
{{% /tab %}}
{{% tab "MemCached" %}}

`memcached` 型の[スパン][1]内の MemCached コマンドはデフォルトで難読化されます。

```yaml
apm_config:
  enabled: true

  ## (...)

  obfuscation:
    memcached:
      ## "memcached" 型のスパンに対する難読化ルールを構成します。デフォルトでは有効になっています。
      enabled: true
```

これは環境変数 `DD_APM_OBFUSCATION_MEMCACHED_ENABLED=false` で無効にすることもできます。

[1]: /ja/tracing/glossary/#spans
{{% /tab %}}
{{% tab "Http" %}}

`http` または `web` 型の[スパン][1]内の HTTP URL はデフォルトでは難読化されません。

**注:** URL の Userinfo 内のパスワードは Datadog によって収集されません。

```yaml
apm_config:
  enabled: true

  ## (...)

  obfuscation:
    http:
      ## URL 内のクエリ文字列の難読化を有効にします。デフォルトでは無効になっています。
      remove_query_string: true
      remove_paths_with_digits: true
```

* `remove_query_string` または環境変数 `DD_APM_OBFUSCATION_HTTP_REMOVE_QUERY_STRING`: true の場合、URL (`http.url`) 内のクエリ文字列を難読化します。
* `remove_paths_with_digits` または環境変数 `DD_APM_OBFUSCATION_HTTP_REMOVE_PATHS_WITH_DIGITS`: この値が true の場合、URL (`http.url`) のパスセグメントで数字のみを含むものは "?" に置き換えられます。

[1]: /ja/tracing/glossary/#spans
{{% /tab %}}
{{% tab "Stack Traces" %}}

デフォルトでは無効になっています。

`remove_stack_traces` パラメーターを true に設定すると、スタックトレースが削除され、`?` で置換されます。

```yaml
apm_config:
  enabled: true

  ## (...)

  obfuscation:
    ## スタックトレースを削除し、"?" に置き換えることを有効にします。デフォルトでは無効になっています。
    remove_stack_traces: true # default false
```

This can also be enabled with the environment variable `DD_APM_OBFUSCATION_REMOVE_STACK_TRACES=true`.

{{% /tab %}}
{{< /tabs >}}

### タグの置換

[スパン][4]のタグから機密データをスクラブするには、[datadog.yaml コンフィギュレーションファイル][5]の `replace_tags` 設定、または `DD_APM_REPLACE_TAGS` 環境変数を使用します。設定または環境変数の値は、タグ内で機密データの置換方法を指定するパラメーターグループの一覧です。パラメーターは以下のとおりです。

* `name`: 置換するタグのキー。すべてのタグを照合するには、`*` を使用します。リソースを照合するには、`resource.name` を使用します。
* `pattern`: 照合対象の正規表現パターン。
* `repl`: 置換文字列。

例:

{{< tabs >}}
{{% tab "datadog.yaml" %}}

```yaml
apm_config:
  replace_tags:
    # タグ "http.url" 内で、文字列 `token/` で始まるすべての文字を "?" で置換します
    - name: "http.url"
      pattern: "token/(.*)"
      repl: "?"
    # リソース名の末尾の "/" 文字を削除します
    - name: "resource.name"
      pattern: "(.*)\/$"
      repl: "$1"
    # 任意のタグに存在する "foo" のすべての出現を "bar" に置換します
    - name: "*"
      pattern: "foo"
      repl: "bar"
    # すべての "error.stack" タグの値を削除します
    - name: "error.stack"
      pattern: "(?s).*"
    # エラーメッセージ内の数字の連続を置換します
    - name: "error.msg"
      pattern: "[0-9]{10}"
      repl: "[REDACTED]"
```

{{% /tab %}}
{{% tab "環境変数" %}}

```json
DD_APM_REPLACE_TAGS=[
      {
        "name": "http.url",
        "pattern": "token/(.*)",
        "repl": "?"
      },
      {
        "name": "resource.name",
        "pattern": "(.*)\/$",
        "repl": "$1"
      },
      {
        "name": "*",
        "pattern": "foo",
        "repl": "bar"
      },
      {
        "name": "error.stack",
        "pattern": "(?s).*"
      },
      {
        "name": "error.msg",
        "pattern": "[0-9]{10}",
        "repl": "[REDACTED]"
      }
]
```

{{% /tab %}}
{{% tab "Kubernetes" %}}

Set the `DD_APM_REPLACE_TAGS` environment variable:
- For Datadog Operator, in `override.nodeAgent.env` in your `datadog-agent.yaml`
- For Helm, in `agents.containers.traceAgent.env` in your `datadog-values.yaml`
- For manual configuration, in the `trace-agent` container section of your manifest

```yaml
- name: DD_APM_REPLACE_TAGS
  value: '[
            {
              "name": "http.url",
              "pattern": "token/(.*)",
              "repl": "?"
            },
            {
              "name": "resource.name",
              "pattern": "(.*)\/$",
              "repl": "$1"
            },
            {
              "name": "*",
              "pattern": "foo",
              "repl": "bar"
            },
            {
              "name": "error.stack",
              "pattern": "(?s).*"
            },
            {
              "name": "error.msg",
              "pattern": "[0-9]{10}",
              "repl": "[REDACTED]"
            }
          ]'
```

#### Examples

Datadog Operator:

```yaml
apiVersion: datadoghq.com/v2alpha1
kind: DatadogAgent
metadata:
  name: datadog
spec:
  override:
    nodeAgent:
      env:
        - name: DD_APM_REPLACE_TAGS
          value: '[
                   {
                     "name": "http.url",
                  # (...)
                  ]'
```

Helm:

```yaml
agents:
  containers:
    traceAgent:
      env:
        - name: DD_APM_REPLACE_TAGS
          value: '[
                   {
                     "name": "http.url",
                  # (...)
                  ]'
```

[1]: /ja/containers/kubernetes/installation/?tab=daemonset
[2]: /ja/containers/kubernetes/installation/?tab=helm
{{% /tab %}}
{{% tab "docker-compose" %}}

```docker-compose.yaml
- DD_APM_REPLACE_TAGS=[{"name":"http.url","pattern":"token/(.*)","repl":"?"},{"name":"resource.name","pattern":"(.*)\/$","repl":"$1"},{"name":"*","pattern":"foo","repl":"bar"},{"name":"error.stack","pattern":"(?s).*"},{"name":"error.msg","pattern":"[0-9]{10}","repl":"[REDACTED]"}]
```

{{% /tab %}}
{{< /tabs >}}

### Ignore resources

For an in depth overview of the options to avoid tracing specific resources, see [Ignoring Unwanted Resources][6].

If your services include simulated traffic such as health checks, you may want to exclude these traces from being collected so the metrics for your services match production traffic.

The Agent can be configured to exclude a specific resource from traces sent by the Agent to Datadog. To prevent the submission of specific resources, use the `ignore_resources` setting in the `datadog.yaml` file . Then create a list of one or more regular expressions, specifying which resources the Agent filters out based on their resource name.

If you are running in a containerized environment, set `DD_APM_IGNORE_RESOURCES` on the container with the Datadog Agent instead. See the [Docker APM Agent environment variables][7] for details.

```text
###### @param ignore_resources - list of strings - optional

###### A list of regular expressions can be provided to exclude certain traces based on their resource name.

###### All entries must be surrounded by double quotes and separated by commas.

###### ignore_resources: ["(GET|POST) /healthcheck","API::NotesController#index"]

```

## Library

### HTTP

Datadog is standardizing [span tag semantics][3] across tracing libraries. Information from HTTP requests are added as span tags prefixed with `http.`. The libraries have the following configuration options to control sensitive data collected in HTTP spans.

#### Redact query strings

The `http.url` tag is assigned the full URL value, including the query string. The query string could contain sensitive data, so by default Datadog parses it and redacts suspicious-looking values. This redaction process is configurable. To modify the regular expression used for redaction, set the `DD_TRACE_OBFUSCATION_QUERY_STRING_REGEXP` environment variable to a valid regex of your choice. Valid regex is platform-specific. When the regex finds a suspicious key-value pair, it replaces it with `<redacted>`.

If you do not want to collect the query string, set the `DD_HTTP_SERVER_TAG_QUERY_STRING` environment variable to `false`. The default value is `true`.

#### Collect headers

To collect trace header tags, set the `DD_TRACE_HEADER_TAGS` environment variable with a map of case-insensitive header keys to tag names. The library applies matching header values as tags on root spans. The setting also accepts entries without a specified tag name, for example:

```
DD_TRACE_HEADER_TAGS=CASE-insensitive-Header:my-tag-name,User-ID:userId,My-Header-And-Tag-Name
```

### Processing

Some tracing libraries provide an interface for processing spans to manually modify or remove sensitive data collected in traces:

* Java: [TraceInterceptor interface][9]
* Ruby: [Processing Pipeline][10]
* Python: [Trace Filtering][11]

## Telemetry collection

Datadog may gather environmental and diagnostic information about your tracing libraries for processing; this may include information about the host running an application, operating system, programming language and runtime, APM integrations used, and application dependencies. Additionally, Datadog may collect information such as diagnostic logs, crash dumps with obfuscated stack traces, and various system performance metrics.

You can disable this telemetry collection using either of these settings:

{{< tabs >}}
{{% tab "datadog.yaml" %}}

```yaml
apm_config:
  telemetry:
    enabled: false
```

{{% /tab %}}
{{% tab "Environment variables" %}}

```bash
export DD_INSTRUMENTATION_TELEMETRY_ENABLED=false
```

{{% /tab %}}
{{< /tabs >}}

## PCI DSS compliance for compliance for APM

{{< site-region region="us" >}}

<div class="alert alert-warning">
PCI compliance for APM is only available for Datadog organizations in the <a href="/getting_started/site/">US1 site</a>.
</div>

To set up a PCI-compliant Datadog org, follow these steps:

{{% pci-apm %}}

See [PCI DSS Compliance][1] for more information. To enable PCI compliance for logs, see [PCI DSS compliance for Log Management][2].

[1]: /ja/data_security/pci_compliance/
[2]: /ja/data_security/pci_compliance/?tab=logmanagement

{{< /site-region >}}

{{< site-region region="us2,us3,us5,eu,gov" >}}
PCI compliance for APM is not available for the {{< region-param key="dd_site_name" >}} site.
{{< /site-region >}}

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/help/
[2]: /ja/tracing/glossary/#trace
[3]: /ja/tracing/trace_collection/tracing_naming_convention/#http-requests
[4]: /ja/tracing/glossary/#spans
[5]: /ja/agent/configuration/agent-configuration-files/#agent-main-configuration-file
[6]: /ja/tracing/guide/ignoring_apm_resources/
[7]: /ja/agent/docker/apm/?tab=standard#docker-apm-agent-environment-variables
[8]: /ja/tracing/guide/send_traces_to_agent_by_api/
[9]: /ja/tracing/trace_collection/custom_instrumentation/java/#extending-tracers
[10]: /ja/tracing/trace_collection/custom_instrumentation/ruby/?tab=activespan#post-processing-traces
[11]: https://ddtrace.readthedocs.io/en/stable/advanced_usage.html#trace-filtering
[12]: /ja/sensitive_data_scanner/
[13]: /ja/security/application_security/how-appsec-works/#data-privacy
