---
further_reading:
- link: /tracing/guide/alert_anomalies_p99_database/
  tag: 3 分
  text: データベースサービスの異常な p99 レイテンシーに関するアラート
- link: /tracing/guide/week_over_week_p50_comparison/
  tag: 2 分
  text: サービスのレイテンシーを前週と比較する
- link: /tracing/guide/apm_dashboard/
  tag: 4 分
  text: ダッシュボードを作成して、APM メトリクスを追跡、関連付ける
- link: /tracing/guide/
  tag: ''
  text: すべてのガイド
title: ウェブサービスの最も遅いエンドポイントで最も遅いトレースをデバッグする
---

_3 minutes to complete_

{{< img src="tracing/guide/slowest_request_daily/slowest_trace_1_cropped.mp4" video="true" alt="最も遅いトレースを特定し、そのホストメトリクスを解明する" style="width:90%;">}}

With Datadog APM, you can investigate the performance of your endpoints, identify slow requests, and investigate the root cause of latency issues. This example shows the slowest [trace][1] of the day for an e-commerce checkout endpoint and how it slows down because of high CPU usage.

1. **[サービスカタログ][2]を開きます**。

    このページには、Datadog にデータを送信しているすべてのサービスのリストが含まれています。なお、キーワード検索、`env-tag` によるフィルタリング、時間枠の設定が可能です。

2. **Search for a relevant and active web service and open the Service Page**.

   この例では、テクノロジースタックの主要サーバーであり、ほとんどのサードパーティサービスへのコールを制御している `web-store` サービスを使用しています。

    {{< img src="tracing/guide/slowest_request_daily/slowest_trace_2_cropped.png" alt="最も遅いトレースを特定し、その原因となっているボトルネックを解明する" style="width:90%;">}}

    スループット、レイテンシー、エラー率の情報に加えて、サービス詳細ページには、そのサービスで特定されたリソース (API エンドポイント、SQL クエリ、Web リクエストなどの主要な操作) の一覧も含まれています。

3. **Sort the Resource table by p99 latency** and click into the slowest resource.
    **Note**: If you cannot see a p99 latency column, you can click on the cog icon `Change Columns` and flip the switch for `p99`.

    The [Resource][4] page contains high-level metrics about this resource like throughput, latency, error rate, and a breakdown of the time spent on each downstream service from the resource. In addition, it contains the specific [traces][1] that pass through the resource and an aggregate view of the [spans][5] that make up these traces.

     {{< img src="tracing/guide/slowest_request_daily/slowest_trace_3_cropped.png" alt="最も遅いトレースを特定し、その原因となっているボトルネックを解明する" style="width:90%;">}}

4. Set the time filter to `1d One Day`. Scroll down to the Traces table and **sort it by duration**, hover over the top trace in the table and **click View Trace**

    これはフレームグラフとその関連情報です。ここでは、トレース内の各ステップの時間とエラーの有無を確認でき、遅いコンポーネントやエラーを起こしやすいコンポーネントを特定するのに役立ちます。フレームグラフは、拡大、スクロール、探索が自然に行えます。フレームグラフの下には、関連するメタデータ、ログ、およびホスト情報が表示されます。

    フレームグラフは、エラーが発生しているスタックや、遅いスタックを正確に特定するのに最適です。エラーは赤くハイライト表示され、時間は横向きスパンで表されます。つまり、長いスパンが最も遅いスタックということになります。フレームグラフの活用については、[トレースビューガイド][6]を参照してください。

    また、フレームグラフの下には、すべてのタグ ([カスタムタグ][7]を含む) が表示されます。ここからは、関連するログ ([ログをトレースに接続][8]してある場合) や、CPU 使用率やメモリ使用量などのホストレベルの情報も確認できます。

    {{< img src="tracing/guide/slowest_request_daily/slowest_trace_4_cropped.png" alt="最も遅いトレースを特定し、その原因となっているボトルネックを解明する" style="width:90%;">}}

5. **Click into the Host tab**, observe the CPU and memory performance of the underlying host while the request was hitting it.
6. **Click Open Host Dashboard** to view all relevant data about the host

Datadog APM では、インフラストラクチャーメトリクスやログなど、他の Datadog のメトリクスや情報がシームレスに統合されています。フレームグラフを使用すればこのような情報や、トレースと一緒に送信しているあらゆる[カスタムメタデータ][7]を利用できます。

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/tracing/glossary/#trace
[2]: https://app.datadoghq.com/services
[3]: /ja/tracing/glossary/#services
[4]: /ja/tracing/glossary/#resources
[5]: /ja/tracing/glossary/#spans
[6]: /ja/tracing/trace_explorer/trace_view/?tab=spanmetadata
[7]: /ja/tracing/guide/adding_metadata_to_spans/
[8]: /ja/tracing/other_telemetry/connect_logs_and_traces/