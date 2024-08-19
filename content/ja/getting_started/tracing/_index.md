---
aliases:
- /ja/getting_started/tracing/distributed-tracing
further_reading:
- link: /tracing/
  tag: ドキュメント
  text: APM 機能の詳細
- link: /tracing/metrics/runtime_metrics/
  tag: ドキュメント
  text: ランタイムメトリクスの有効化
- link: /tracing/guide/#enabling-tracing-tutorials
  tag: Guides
  text: トレースを有効にするための様々な方法のチュートリアル
- link: https://learn.datadoghq.com/courses/intro-to-apm
  tag: Learning Center
  text: Introduction to Application Performance Monitoring
- link: https://dtdg.co/fe
  tag: Foundation Enablement
  text: APM の理解を深めるためのインタラクティブセッションに参加しましょう
title: APM トレーシングの概要
---

## Overview

Datadog Application Performance Monitoring (APM) は、アプリケーションを詳細に可視化することで、パフォーマンスのボトルネックを特定し、問題をトラブルシューティングし、サービスを最適化することを可能にします。

このガイドでは、APM の始め方と最初のトレースを Datadog に送信する方法を説明します。

1. Datadog APM をセットアップして、Datadog にトレースを送信します。
1. アプリケーションを実行してデータを生成します。
1. 収集したデータを Datadog で確認します。

## Prerequisites

To complete this guide, you need the following:

1. [Datadog アカウントの作成][1]をまだ行っていない場合は、作成します。
1. [Datadog API キー][2]を検索または作成します。
1. Linux ホストまたは VM を起動します。

## Create an application

Datadog で観測するアプリケーションを作成するには

1. Linux ホストまたは VM 上で、`hello.py` という名前の Python アプリケーションを新規作成します。例えば、`nano hello.py` とします。
1. 以下のコードを `hello.py` に追加します。

    {{< code-block lang="python" filename="hello.py" collapsible="true" disable_copy="false" >}}
  from flask import Flask
  import random

  app = Flask(__name__)

  quotes = [
      "成功するために努力するのではなく、むしろ価値あるものになるために努力しなさい。- アルバート・アインシュタイン",
      "できると信じれば、道は拓ける。- セオドア・ルーズベルト",
      "未来は、夢の美しさを信じる人のもの。- エレノア・ルーズベルト"
  ]

  @app.route('/')
  def index():
      quote = random.choice(quotes)+"\n"
      return quote

  if __name__ == '__main__':
      app.run(host='0.0.0.0', port=5050)
  {{< /code-block >}}

## Set up Datadog APM

アプリケーションのコードやデプロイプロセスを変更せずに Datadog APM をセットアップするには、Single Step APM Instrumentation を使用します。

<div class="alert alert-info"><strong>注</strong>: <a href="https://docs.datadoghq.com/tracing/trace_collection/automatic_instrumentation/single-step-apm/">Single Step APM Instrumentation</a> はベータ版です。または、<a href="https://docs.datadoghq.com/tracing/trace_collection/automatic_instrumentation/dd_libraries/">Datadog トレーシングライブラリ</a>を使用して APM をセットアップすることもできます。</div>

1. インストールコマンドを実行します。

   ```shell
    DD_API_KEY=<YOUR_DD_API_KEY> DD_SITE="<YOUR_DD_SITE>" DD_APM_INSTRUMENTATION_ENABLED=host DD_APM_INSTRUMENTATION_LIBRARIES=python:2 DD_ENV=<AGENT_ENV> bash -c "$(curl -L https://install.datadoghq.com/scripts/install_script_agent7.sh)"
    ```

   `<YOUR_DD_API_KEY>` を [Datadog API キー][2]、`<YOUR_DD_SITE>` を [Datadog サイト][7]、`<AGENT_ENV>` を Agent がインストールされている環境 (例えば `development`) に置き換えます。 


1. Restart the services on your host or VM.
1. Verify the Agent is running:

    ```shell
   sudo datadog-agent status
   ```

This approach automatically installs the Datadog Agent, enables Datadog APM, and [instruments][5] your application at runtime.

## Run the application

When you set up Datadog APM with Single Step Instrumentation, Datadog automatically instruments your application at runtime.

To run `hello.py`:

1. Create a Python virtual environment in the current directory:

   ```shell
   python3 -m venv ./venv
   ```

1. Activate the `venv` virtual environment:

   ```shell
   source ./venv/bin/activate
   ```

1. Install `pip` and `flask`:

   ```shell
   sudo apt-get install python3-pip
   pip install flask
   ```

1. Set the service name and run `hello.py`:

   ```shell
   export DD_SERVICE=hello
   python3 hello.py
   ```

## Test the application

Test the application to send traces to Datadog:

1. In a new command prompt, run the following:

   ```shell
   curl http://0.0.0.0:5050/
   ```
1. Confirm that a random quote is returned.
   ```text
   Believe you can and you're halfway there. - Theodore Roosevelt
   ```

Each time you run the `curl` command, a new trace is sent to Datadog.

## Explore traces in Datadog

1. In Datadog, go to [**APM** > **Services**][3]. You should see a Python service named `hello`:

   {{< img src="/getting_started/apm/service-catalog.png" alt="Service Catalog shows the new Python service." style="width:100%;" >}}

1. Select the service to view its performance metrics, such as latency, throughput, and error rates.
1. Go to [**APM** > **Traces**][4]. You should see a trace for the `hello` service:

   {{< img src="/getting_started/apm/trace-explorer.png" alt="Trace explorer shows the trace for the hello service." style="width:100%;" >}}

1. Select a trace to see its details, including the flame graph, which helps identify performance bottlenecks.

## Advanced APM setup

Up until this point, you let Datadog automatically instrument the `hello.py` application using Single Step Instrumentation. This approach is recommended if you want to capture essential traces across common libraries and languages without touching code or manually installing libraries.

However, if you need to collect traces from custom code or require more fine-grained control, you can add [custom instrumentation][6].

To illustrate this, you will import the Datadog Python tracing library into `hello.py` and create a custom span and span tag.

To add custom instrumentation:

1. Install the Datadog tracing library:

   ```shell
   pip install ddtrace
   ```

1. Add the highlighted lines to the code in `hello.py` to create a custom span tag `get_quote` and a custom span tag `quote`:

   {{< highlight python "hl_lines=3 15 17" >}}
    from flask import Flask
    import random
    from ddtrace import tracer

    app = Flask(__name__)

    quotes = [
        "Strive not to be a success, but rather to be of value. - Albert Einstein",
        "Believe you can and you're halfway there. - Theodore Roosevelt",
        "The future belongs to those who believe in the beauty of their dreams. - Eleanor Roosevelt"
    ]

    @app.route('/')
    def index():
        with tracer.trace("get_quote") as span:
            quote = random.choice(quotes)+"\n"
            span.set_tag("quote", quote)
            return quote

    if __name__ == '__main__':
        app.run(host='0.0.0.0', port=5050)
   {{< /highlight >}}

1. Run `hello.py` in the virtual environment from earlier:
   ```shell
   ddtrace-run python hello.py
   ```
1. Run a few `curl` commands in a separate command prompt:
   ```shell
   curl http://0.0.0.0:5050/
   ```
1. In Datadog, go to [**APM** > **Traces**][4].
1. Select the **hello** trace.
1. Find the new custom `get_quote` span in the flame graph and hover over it:

   {{< img src="/getting_started/apm/custom-instrumentation.png" alt="The get_quote custom span displays in the flame graph. On hover, the quote span tag is displayed. " style="width:100%;" >}}

1. Notice that the custom `quote` span tag displays on the **Info** tab.


## Further reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: https://www.datadoghq.com/free-datadog-trial/
[2]: https://app.datadoghq.com/organization-settings/api-keys/
[3]: https://app.datadoghq.com/services
[4]: https://app.datadoghq.com/apm/traces
[5]: /ja/tracing/glossary/#instrumentation
[6]: /ja/tracing/trace_collection/custom_instrumentation/
[7]: /ja/getting_started/site/