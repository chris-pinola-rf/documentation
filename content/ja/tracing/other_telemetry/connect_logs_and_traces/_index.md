---
algolia:
  tags:
  - ログとトレース
aliases:
- /ja/tracing/advanced/connect_logs_and_traces/
- /ja/tracing/connect_logs_and_traces/
description: ログとトレースを接続して Datadog で関連付けます。
title: ログとトレースの相関
type: multi-code-lang
---

{{< img src="tracing/connect_logs_and_traces/trace_id_injection.png" alt="トレースのログ" style="width:100%;">}}

Datadog APM と Datadog Log Management の間の相関関係は、ログの属性としてトレース ID、スパン ID、`env`、`service`、`version` を挿入することで改善されています。これらのフィールドを使用すると、特定のサービスとバージョンに関連付けられた正確なログ、または観測された[トレース][1]に関連付けられたすべてのログを見つけることができます。

アプリケーションのトレーサを `DD_ENV`、`DD_SERVICE`、`DD_VERSION` で構成することをお勧めします。これは、`env`、`service`、`version` を追加する際のベストプラクティスです。詳細については、[統合サービスタグ付け][2]のドキュメントを参照してください。

Before correlating traces with logs, ensure your logs are either sent as JSON, or [parsed by the proper language level log processor][3]. Your language level logs _must_ be turned into Datadog attributes in order for traces and logs correlation to work.

To learn more about automatically or manually connecting your logs to your traces, select your language below:

{{< partial name="apm/apm-connect-logs-and-traces.html" >}}

[1]: /ja/tracing/glossary/#trace
[2]: /ja/getting_started/tagging/unified_service_tagging
[3]: /ja/agent/logs/#enabling-log-collection-from-integrations
