---
aliases:
- /ja/tracing/java
- /ja/tracing/languages/java
- /ja/agent/apm/java/
- /ja/tracing/setup/java
- /ja/tracing/setup_overview/java
- /ja/tracing/setup_overview/setup/java
- /ja/tracing/trace_collection/dd_libraries/java/
code_lang: java
code_lang_weight: 0
further_reading:
- link: https://github.com/DataDog/dd-trace-java
  tag: Source Code
  text: Datadog Java APM ソースコード
- link: tracing/glossary/
  tag: ドキュメント
  text: サービス、リソース、トレースの詳細
title: Java アプリケーションのトレース
type: multi-code-lang
---
## 互換性要件

最新の Java トレーサーは、バージョン 8 以上のすべての JVM をサポートしています。8 以下の JVM バージョンに関する追加情報は、[サポートする JVM ランタイム][10]をお読みください。

Datadog の Java バージョンとフレームワークのサポート一覧 (レガシーバージョンとメンテナンスバージョンを含む) については、[互換性要件][1]ページをご覧ください。

## はじめに

作業を始める前に、[Agent のインストールと構成][18]が済んでいることを確認してください。

### アプリケーションをインスツルメントする

Datadog Agent をインストールして構成したら、次はアプリケーションに直接トレーシングライブラリを追加してインスツルメントします。[互換性情報][1]の詳細をお読みください。

アプリケーションのトレースを開始するには

1. 最新のトレーサークラスファイルを含む `dd-java-agent.jar` を、Datadog ユーザーがアクセス可能なフォルダにダウンロードします。

{{< tabs >}}
{{% tab "Wget" %}}
   ```shell
   wget -O dd-java-agent.jar 'https://dtdg.co/latest-java-tracer'
   ```
{{% /tab %}}
{{% tab "cURL" %}}
   ```shell
   curl -Lo dd-java-agent.jar 'https://dtdg.co/latest-java-tracer'
   ```
{{% /tab %}}
{{% tab "Dockerfile" %}}
   ```dockerfile
   ADD 'https://dtdg.co/latest-java-tracer' dd-java-agent.jar
   ```
{{% /tab %}}
{{< /tabs >}}

   **注:** 特定の**メジャー**バージョンの最新ビルドをダウンロードするには、代わりに `https://dtdg.co/java-tracer-vX` リンクを使用してください。ここで `X` は希望するメジャーバージョンです。
例えば、バージョン 1 の最新ビルドには `https://dtdg.co/java-tracer-v1` を使用します。マイナーバージョン番号は含めてはいけません。または、特定のバージョンについては Datadog の [Maven リポジトリ][3]を参照してください。

   **Note**: Release Candidate versions are made available in GitHub [DataDog/dd-trace-java releases][21]. These have "RC" in the version and are recommended for testing outside of your production environment. You can [subscribe to GitHub release notifications][20] to be informed when new Release Candidates are available for testing. If you experience any issues with Release Candidates, reach out to [Datadog support][22].

2. IDE、Maven または Gradle アプリケーションスクリプト、`java -jar` コマンドから、Continuous Profiler、デプロイ追跡、ログ挿入（Datadog へログを送信する場合）を使用してアプリケーションを実行するには、`-javaagent` JVM 引数と、該当する以下のコンフィギュレーションオプションを追加します。

    ```text
    java -javaagent:/path/to/dd-java-agent.jar -Ddd.profiling.enabled=true -XX:FlightRecorderOptions=stackdepth=256 -Ddd.logs.injection=true -Ddd.service=my-app -Ddd.env=staging -Ddd.version=1.0 -jar path/to/your/app.jar
    ```
    イメージのサイズを削減し、モジュールを省略する必要性が強い場合は、[jdeps][19] コマンドを使って依存関係を特定することができます。しかし、必要なモジュールは時間の経過とともに変更される可能性がありますので、自己責任で行ってください。

   <div class="alert alert-danger">プロファイリングを有効にすると、APM バンドルによっては料金に影響が出る場合があります。詳しくは<a href="https://docs.datadoghq.com/account_management/billing/apm_tracing_profiler/">料金ページ</a>をご覧ください。</div>

| 環境変数      | システムプロパティ                     | 説明|
| --------- | --------------------------------- | ------------ |
| `DD_ENV`      | `dd.env`                  | アプリケーション環境（`production`、`staging` など） |
| `DD_LOGS_INJECTION`   | `dd.logs.injection`     | Datadog のトレース ID とスパン ID に対する MDC キーの自動挿入を有効にします。詳細については、[高度な使用方法][6]を参照してください。 <br><br>**ベータ版**: バージョン 1.18.3 から、このサービスが実行される場所で [Agent リモート構成][16]が有効になっている場合、[サービスカタログ][17] UI で `DD_LOGS_INJECTION` を設定できます。 |
| `DD_PROFILING_ENABLED`      | `dd.profiling.enabled`          | Enable the [Continuous Profiler][5] |
| `DD_SERVICE`   | `dd.service`     | 同一のジョブを実行するプロセスセットの名前。アプリケーションの統計のグループ化に使われます。 |
| `DD_TRACE_SAMPLE_RATE` | `dd.trace.sample.rate` |   すべてのサービスのトレースのルートでサンプリングレートを設定します。<br><br>**ベータ版**: バージョン 1.18.3 から、このサービスが実行される場所で [Agent リモート構成][16]が有効になっている場合、[サービスカタログ][17] UI で `DD_TRACE_SAMPLE_RATE` を設定できます。     |
| `DD_TRACE_SAMPLING_RULES` | `dd.trace.sampling.rules` |   指定したルールに合致するサービスのトレースのルートでのサンプリングレートを設定します。    |
| `DD_VERSION` | `dd.version` |  アプリケーションのバージョン (例: `2.5`、`202003181415`、`1.3-alpha`) |

追加の[コンフィギュレーションオプション](#configuration) は以下で説明されています。


### Java トレーサーを JVM に追加する

アプリケーションサーバーのドキュメントを使用して、`-javaagent` およびその他の JVM 引数を渡す正しい方法を確認してください。一般的に使用されるフレームワークの手順は次のとおりです。

{{< tabs >}}
{{% tab "Spring Boot" %}}

アプリの名前が `my_app.jar` の場合は、以下を含む `my_app.conf` を作成します。

```text
JAVA_OPTS=-javaagent:/path/to/dd-java-agent.jar
```

詳細については、[Spring Boot のドキュメント][1]を参照してください。


[1]: https://docs.spring.io/spring-boot/docs/current/reference/html/deployment.html#deployment-script-customization-when-it-runs
{{% /tab %}}
{{% tab "Tomcat" %}}

#### Linux

To enable tracing when running Tomcat on Linux:

1. Open your Tomcat startup script file, for example `setenv.sh`.
2. Add the following to `setenv.sh`:
   ```text
   CATALINA_OPTS="$CATALINA_OPTS -javaagent:/path/to/dd-java-agent.jar"
   ```

#### Windows (Tomcat as a Windows service)

To enable tracing when running Tomcat as a Windows service:

1. Open a Command Prompt.
1. Run the following command to update your Tomcat service configuration:
    ```shell
    tomcat8 //US//<SERVICE_NAME> --Environment="CATALINA_OPTS=%CATALINA_OPTS% -javaagent:\"c:\path\to\dd-java-agent.jar\""
    ```
   Replace `<SERVICE_NAME>` with the name of your Tomcat service and replace the path to `dd-java-agent.jar`.
1. Restart your Tomcat service for changes to take effect.

#### Windows (Tomcat with environment setup script)

To enable tracing when running Tomcat with an environment setup script:

1. Create `setenv.bat` in the `./bin` directory of the Tomcat project folder, if it doesn't already exist.
1. Add the following to `setenv.bat`:
   ```text
   set CATALINA_OPTS=%CATALINA_OPTS% -javaagent:"c:\path\to\dd-java-agent.jar"
   ```
If the previous step doesn't work, try adding the following instead:
```text
set JAVA_OPTS=%JAVA_OPTS% -javaagent:"c:\path\to\dd-java-agent.jar"
```

{{% /tab %}}
{{% tab "JBoss" %}}

- In standalone mode:

  Add the following line to the end of `standalone.conf`:

```text
JAVA_OPTS="$JAVA_OPTS -javaagent:/path/to/dd-java-agent.jar"
```

- In standalone mode and on Windows, add the following line to the end of `standalone.conf.bat`:

```text
set "JAVA_OPTS=%JAVA_OPTS% -javaagent:X:/path/to/dd-java-agent.jar"
```

- In domain mode:

  Add the following line in the file `domain.xml`, under the tag server-groups.server-group.jvm.jvm-options:

```text
<option value="-javaagent:/path/to/dd-java-agent.jar"/>
```

For more details, see the [JBoss documentation][1].


[1]: https://access.redhat.com/documentation/en-us/red_hat_jboss_enterprise_application_platform/7.0/html/configuration_guide/configuring_jvm_settings
{{% /tab %}}
{{% tab "Jetty" %}}

If you use `jetty.sh` to start Jetty as a service, edit it to add:

```text
JAVA_OPTIONS="${JAVA_OPTIONS} -javaagent:/path/to/dd-java-agent.jar"
```

If you use `start.ini` to start Jetty, add the following line (under `--exec`, or add `--exec` line if it isn't there yet):

```text
-javaagent:/path/to/dd-java-agent.jar
```

{{% /tab %}}
{{% tab "WebSphere" %}}

In the administrative console:

1. Select **Servers**. Under **Server Type**, select **WebSphere application servers** and select your server.
2. Select **Java and Process Management > Process Definition**.
3. In the **Additional Properties** section, click **Java Virtual Machine**.
4. In the **Generic JVM arguments** text field, enter:

```text
-javaagent:/path/to/dd-java-agent.jar
```

For additional details and options, see the [WebSphere docs][1].

[1]: https://www.ibm.com/support/pages/setting-generic-jvm-arguments-websphere-application-server
{{% /tab %}}
{{< /tabs >}}

**Note**

- If you're adding the `-javaagent` argument to your `java -jar` command, it needs to be added _before_ the `-jar` argument, as a JVM option, not as an application argument. For example:

   ```text
   java -javaagent:/path/to/dd-java-agent.jar -jar my_app.jar
   ```

     For more information, see the [Oracle documentation][7].

- Never add `dd-java-agent` to your classpath. It can cause unexpected behavior.

## Automatic instrumentation

Automatic instrumentation for Java uses the `java-agent` instrumentation capabilities [provided by the JVM][8]. When a `java-agent` is registered, it can modify class files at load time.

**Note:** Classes loaded with remote ClassLoader are not instrumented automatically.

Instrumentation may come from auto-instrumentation, the OpenTracing API, or a mixture of both. Instrumentation generally captures the following info:

- Timing duration is captured using the JVM's NanoTime clock unless a timestamp is provided from the OpenTracing API
- Key/value tag pairs
- Errors and stack traces which are unhandled by the application
- A total count of traces (requests) flowing through the system

## Configuration

If needed, configure the tracing library to send application performance telemetry data as you require, including setting up Unified Service Tagging. Read [Library Configuration][9] for details.

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}


[1]: /ja/tracing/compatibility_requirements/java
[2]: https://app.datadoghq.com/apm/service-setup
[3]: https://repo1.maven.org/maven2/com/datadoghq/dd-java-agent
[4]: /ja/account_management/billing/apm_tracing_profiler/
[5]: /ja/profiler/
[6]: /ja/tracing/other_telemetry/connect_logs_and_traces/java/
[7]: https://docs.oracle.com/javase/7/docs/technotes/tools/solaris/java.html
[8]: https://docs.oracle.com/javase/8/docs/api/java/lang/instrument/package-summary.html
[9]: /ja/tracing/trace_collection/library_config/java/
[10]: /ja/tracing/trace_collection/compatibility/java/#supported-jvm-runtimes
[11]: /ja/tracing/trace_collection/library_injection_local/
[16]: /ja/agent/remote_config/
[17]: https://app.datadoghq.com/services
[18]: /ja/tracing/trace_collection/automatic_instrumentation/?tab=datadoglibraries#install-and-configure-the-agent
[19]: https://docs.oracle.com/en/java/javase/11/tools/jdeps.html
[20]: https://docs.github.com/en/account-and-profile/managing-subscriptions-and-notifications-on-github/managing-subscriptions-for-activity-on-github/viewing-your-subscriptions
[21]: https://github.com/DataDog/dd-trace-java/releases
[22]: https://docs.datadoghq.com/ja/getting_started/support/
