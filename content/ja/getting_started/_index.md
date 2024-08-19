---
aliases:
- /ja/overview
- /ja/getting_started/faq/
cascade:
  algolia:
    category: Getting Started
    rank: 50
disable_sidebar: true
further_reading:
- link: https://learn.datadoghq.com/
  tag: Learning Center
  text: Take a course to get started with Datadog
- link: https://datadoghq.com/blog/
  tag: Blog
  text: Learn about new Datadog products and features, integrations, and more
- link: https://app.datadoghq.com/help/quick_start
  tag: アプリ
  text: クイックスタートガイドを見る
title: はじめに
---

## What is Datadog?

Datadog is an observability platform that supports every phase of software development on any stack. The platform consists of many products that help you build, test, monitor, debug, optimize, and secure your software. These products can be used individually or combined into a customized solution.

The table below lists a few examples of Datadog products:

<table>
    <thead>
        <th>カテゴリー</th>
        <th>製品例</th>
    </thead>
    <tr>
        <td><p><strong>開発</strong></p></td>
        <td>
        <ul>
        <li><a href="/code_analysis/?tab=codevulnerabilities">Code Analysis</a> を使って、テキストエディタや GitHub 上でコードの脆弱性を明示します。</li>
        <li><a href="/coscreen/">CoScreen</a> を使って遠隔ペアプログラミングセッションを促進します。</li></ul>
        </td>
    </tr>
    <tr>
        <td><p><strong>テスト</strong></p></td>
        <td>
            <ul>
                <li><a href="/quality_gates/">Quality Gates</a> を使って、欠陥のあるコードが本番環境にデプロイされるのをブロックします。</li>
                <li><a href="/synthetics/">Synthetic Monitoring</a> を使って、世界中のユーザーをシミュレートし、Web アプリ、API、モバイルアプリケーションをテストします。</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><p><strong>監視</strong></p></td>
        <td>
            <ul>
                <li>処理、集計、<a href="/monitors/">アラート</a>をきめ細かく制御しながら、<a href="/logs/">ログ</a>、<a href="/metrics/">メトリクス</a>、<a href="/events/">イベント</a>、<a href="/tracing/glossary/#trace">ネットワークトレース</a>を取り込みます。</li>
                <li><a href="/profiler/">Continuous Profiler</a> を使ってホストのパフォーマンスを評価します。</li>
                <li><a href="/tracing/">Application Performance Monitoring</a> を使って、アプリケーションパフォーマンスを評価します。</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><p><strong>トラブルシューティング</strong></p></td>
        <td>
            <ul>
                <li><a href="/error_tracking/">エラー</a>や<a href="/service_management/incident_management/">インシデント</a>を管理し、問題をまとめ、修正を提案します。</li>
                <li><a href="/real_user_monitoring/">Real User Monitoring</a> を使ってユーザーの離脱を測定し、ユーザーの不満を検出します。</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><p><strong>セキュリティ</p></td>
        <td>
            <ul>
                <li><a href="/security/">Datadog Security</a> を使って脅威と攻撃を検出します。</li>
            </ul>
        </td>
    </tr>
</table>

Additionally, hundreds of [integrations][1] allow you to layer Datadog features over the technologies you already use. For example, the [AWS integration][2] collects logs, events, and metrics from more than 90 AWS services.

## Learn more

{{< learning-center-callout header="イネーブルメントウェビナーセッションに参加" hide_image="true" btn_title="登録" btn_url="https://www.datadoghq.com/technical-enablement/session/datadog-overview/">}}
  この基礎セミナーでは、「Datadog とは何か、そしてそれが自分にとって何をしてくれるのか？」という重要な質問に答える手助けをします。Datadog にデータを送信する方法や、さまざまな環境、アプリケーション、インフラストラクチャーの状態をよりよく理解するためにどのページを訪れるべきかを学びます。
{{< /learning-center-callout >}}

### Take a course
The Datadog Learning Center offers hands-on experience with the Datadog platform. The [Getting Started courses][3] cover observability practices, key Datadog concepts, and more.

For the fastest introduction to navigating Datadog, try the [Quick Start course][4].

### Dive deeper into a product area
{{< whatsnext desc="Get started with one of the guides below:">}}
{{< nextlink href="/getting_started/application" >}}<u>Datadog</u>: Discover how to use the Datadog UI: Dashboards, infrastructure list, maps, and more.{{< /nextlink >}}
{{< nextlink href="/getting_started/site" >}}<u>Datadog Site</u>: Select the appropriate Datadog site for your region and security requirements.{{< /nextlink >}}
{{< nextlink href="/getting_started/devsecops" >}}<u>DevSecOps Bundles</u>: Get started with the APM DevSecOps and Infrastructure DevSecOps bundles.{{< /nextlink >}}
{{< nextlink href="/getting_started/agent" >}}<u>Agent</u>: Send metrics and events from your hosts to Datadog.{{< /nextlink >}}
{{< nextlink href="/getting_started/api" >}}<u>API</u>: Get started with the Datadog HTTP API.{{< /nextlink >}}
{{< nextlink href="/getting_started/integrations" >}}<u>Integrations</u>: Learn how to collect metrics, traces, and logs with Datadog integrations.{{< /nextlink >}}
{{< nextlink href="/getting_started/tagging" >}}<u>Tags</u>: Start tagging your metrics, logs, and traces.{{< /nextlink >}}
{{< nextlink href="/getting_started/opentelemetry" >}}<u>OpenTelemetry</u>: Learn how to send OpenTelemetry metrics, traces, and logs to Datadog.{{< /nextlink >}}
{{< nextlink href="/getting_started/learning_center" >}}<u>Learning Center</u>: Follow a learning path, take a self-guided class or lab, and explore the Datadog certification program.{{< /nextlink >}}
{{< /whatsnext >}}

{{< whatsnext desc="Platform Services:">}}
{{< nextlink href="/getting_started/dashboards" >}}<u>Dashboards</u>: Create, share, and maintain dashboards that answer the work questions that matter to you.{{< /nextlink >}}
{{< nextlink href="/getting_started/monitors" >}}<u>Monitors</u>: Set up alerts and notifications so that your team knows when critical changes occur.{{< /nextlink >}}
{{< nextlink href="/getting_started/incident_management" >}}<u>Incident Management</u>: Communicate and track problems in your systems.{{< /nextlink >}}
{{< nextlink href="/getting_started/workflow_automation" >}}<u>Workflow Automation</u>: Automate end-to-end processes in response to alerts and security signals.{{< /nextlink >}}
{{< /whatsnext >}}

{{< whatsnext desc="製品:">}}
{{< nextlink href="/getting_started/containers" >}}<u>コンテナ</u>: Agent オートディスカバリーと Datadog オペレーターの使用方法をご紹介します。{{< /nextlink >}}
{{< nextlink href="/getting_started/serverless" >}}<u>Serverless for AWS Lambda</u>: サーバーレスインフラストラクチャーからメトリクス、ログ、トレースを収集する方法をご紹介します。{{< /nextlink >}}
{{< nextlink href="/getting_started/service_catalog" >}}<u>サービスカタログ</u>: サービスカタログで、サービスの所有権、信頼性、パフォーマンスを大規模に管理します。 {{< /nextlink >}}
{{< nextlink href="/getting_started/tracing" >}}<u>トレーシング</u>: Agent をセットアップして、小さなアプリケーションをトレースします。{{< /nextlink >}}
{{< nextlink href="/getting_started/profiler" >}}<u>Profiler</u>: Continuous Profiler を使用して、コードのパフォーマンス問題を発見し、修正します。{{< /nextlink >}}
{{< nextlink href="/getting_started/database_monitoring" >}}<u>Database Monitoring</u>: データベースの健全性とパフォーマンスを表示し、発生した問題を迅速にトラブルシューティングします。{{< /nextlink >}}
{{< nextlink href="/getting_started/synthetics" >}}<u>Synthetic Monitoring</u>: Synthetic テストを使って、API エンドポイントと主要なビジネスジャーニーのテストと監視を開始します。{{< /nextlink >}}
{{< nextlink href="/getting_started/continuous_testing" >}}<u>Continuous Testing</u>: CI パイプラインや IDE でエンドツーエンドの Synthetic テストを実行します。{{< /nextlink >}}
{{< nextlink href="/getting_started/session_replay" >}}<u>Session Replay</u>: Session Replay を利用して、ユーザーが製品とどのようにやり取りしているかを詳細に確認します。{{< /nextlink >}}
{{< nextlink href="/getting_started/application_security" >}}<u>Application Security Management</u>: ASM を使ってチームを活性化するためのベストプラクティスをご紹介します。{{< /nextlink >}}
{{< nextlink href="/getting_started/cloud_security_management" >}}<u>Cloud Security Management</u>: CSM を使ってチームを活性化するためのベストプラクティスをご紹介します。{{< /nextlink >}}
{{< nextlink href="/getting_started/cloud_siem" >}}<u>Cloud SIEM</u>: Cloud SIEM を使ってチームを活性化するためのベストプラクティスをご紹介します。{{< /nextlink >}}
{{< nextlink href="/getting_started/logs" >}}<u>Logs</u>: 最初のログを送信し、ログ処理を使ってログを充実させましょう。{{< /nextlink >}}
{{< nextlink href="/getting_started/ci_visibility" >}}<u>CI Visibility</u>: CI プロバイダーとのインテグレーションをセットアップすることで、CI パイプラインデータを収集します。{{< /nextlink >}}
{{< nextlink href="/getting_started/test_visibility" >}}<u>Test Visibility</u>: Datadog でテストサービスをセットアップして、CI テストデータを収集します。{{< /nextlink >}}
{{< nextlink href="/getting_started/intelligent_test_runner" >}}<u>Intelligent Test Runner</u>: コード変更に関連するテストのみを実行することで、テストスイートを最適化し、CI コストを削減します。{{< /nextlink >}}
{{< nextlink href="/getting_started/code_analysis" >}}<u>Code Analysis</u>: リポジトリに品質やセキュリティ上の問題がないか分析します。{{< /nextlink >}}
{{< /whatsnext >}}

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/getting_started/integrations/
[2]: /ja/integrations/amazon_web_services/
[3]: https://learn.datadoghq.com/collections/getting-started
[4]: https://learn.datadoghq.com/courses/course-quickstart