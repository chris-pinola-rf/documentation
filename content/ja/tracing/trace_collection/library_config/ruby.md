---
code_lang: ruby
code_lang_weight: 30
further_reading:
- link: https://github.com/DataDog/dd-trace-rb/
  tag: Source Code
  text: ソースコード
- link: /tracing/trace_collection/trace_context_propagation/
  tag: Documentation
  text: トレースコンテキストの伝搬
- link: /opentelemetry/interoperability/environment_variable_support
  tag: Documentation
  text: OpenTelemetry Environment Variable Configurations
title: Ruby トレーシングライブラリの構成
type: multi-code-lang
---

After you set up the tracing library with your code and configure the Agent to collect APM data, optionally configure the tracing library as desired, including setting up [Unified Service Tagging][1].

For information about configuring the Ruby tracing library, see [Additional Ruby configuration][2].

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/getting_started/tagging/unified_service_tagging/
[2]: /ja/tracing/trace_collection/dd_libraries/ruby/#additional-configuration
[3]: /ja/opentelemetry/interoperability/environment_variable_support