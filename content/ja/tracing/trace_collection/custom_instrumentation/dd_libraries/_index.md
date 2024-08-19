---
algolia:
  tags:
  - APM カスタムインスツルメンテーション
aliases:
- /ja/tracing/setup/php/manual-installation
- /ja/agent/apm/php/manual-installation
- /ja/tracing/guide/distributed_tracing/
- /ja/tracing/advanced/manual_instrumentation/
- /ja/tracing/advanced/opentracing/
- /ja/tracing/opentracing/
- /ja/tracing/manual_instrumentation/
- /ja/tracing/guide/adding_metadata_to_spans
- /ja/tracing/advanced/adding_metadata_to_spans/
- /ja/tracing/custom_instrumentation
- /ja/tracing/setup_overview/custom_instrumentation/undefined
- /ja/tracing/setup_overview/custom_instrumentation/
description: Datadog トレース内でインスツルメンテーションと可観測性をカスタマイズ。
further_reading:
- link: tracing/guide/instrument_custom_method
  text: カスタムメソッドをインスツルメントして、ビジネスロジックを詳細に可視化する
- link: tracing/connect_logs_and_traces
  text: ログとトレースの接続
- link: tracing/visualization/
  text: サービス、リソース、トレースの詳細
- link: https://www.datadoghq.com/blog/opentelemetry-instrumentation/
  text: Datadog および OpenTelemetry のイニシアティブのイニシアティブについて
title: Datadog ライブラリを使ったカスタムインスツルメンテーション
type: multi-code-lang
---

Custom instrumentation allows programmatic creation, modification, or deletion of traces to send to Datadog. This is useful for tracing in-house code not captured by automatic instrumentation, removing unwanted spans from traces, and for providing deeper visibility and context into spans, including adding any desired span tags.

Before instrumenting your application, review Datadog's [APM Terminology][2] and familiarize yourself with the core concepts of Datadog APM.

If you use an open standard to instrument your code, see [Instrumenting with OpenTracing][3] or [Instrumenting with OpenTelemetry][4].

{{< partial name="apm/apm-manual-instrumentation.html" >}}


<br>

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}


[2]: /ja/tracing/glossary
[3]: /ja/tracing/trace_collection/opentracing/
[4]: /ja/tracing/trace_collection/otel_instrumentation