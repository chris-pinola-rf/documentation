---
further_reading:
- link: /tracing/trace_collection/dd_libraries/cpp/
  tag: ドキュメント
  text: C++ によるトレースアプリケーションの詳細
title: C++ による APM の設定
---

## Overview

This guide expands on the [C++ APM docs][1] to provide step-by-step instructions on how to set up a simple C++ app with APM on your VM for troubleshooting.

## Setting up your box

### Basic environment

First, spin up a fresh `ubuntu/jammy64` Vagrant box and `ssh` into it with:

```bash
vagrant init ubuntu/jammy64
vagrant up
vagrant ssh
```

Next, install the agent with the [instructions in the UI][2].

### Prepping for C++

Install `g++` and `cmake` with:

```shell
sudo apt-get update
sudo apt-get -y install g++ cmake
```

以下のコマンドで `dd-trace-cpp` ライブラリをダウンロードしてインストールします。

```shell
wget https://github.com/DataDog/dd-trace-cpp/archive/v0.2.0.tar.gz -O dd-trace-cpp.tar.gz
```

GitHub からレート制限のメッセージが表示された場合は、数分待ってからもう一度コマンドを実行してください。

`tar` ファイルをダウンロードした後、解凍します。

```bash
tar zxvf dd-trace-cpp.tar.gz -C ./dd-trace-cpp/ --strip-components=1
```

最後に、以下のコマンドでライブラリをビルドしてインストールします。

```bash
cd dd-trace-cpp
cmake -B build .
cmake --build build -j
cmake --install build
```

## Building a simple app

Create a new file called `tracer_example.cpp` and populate it with the below code:

```cpp
#<datadog/tracer.h> を含めます
#<iostream> を含めます
#<string> を含めます

int main(int argc, char* argv[]) {
  datadog::tracing::TracerConfig tracer_config;
  tracer_config.service = "compiled-in example";

  const auto validated_config = dd::finalize_config(tracer_options);
  if (!validated_config) {
    std::cerr << validated_config.error() << '\n';
    return 1;
  }

  dd::Tracer tracer{*validated_config};

  // いくつかのスパンを作成します。
  {
    datadog::tracing::SpanConfig options;
    options.name = "A";
    options.tags.emplace("tag", "123");
    auto span_a = tracer.create_span(options);

    auto span_b = span_a.create_child();
    span_b.set_name("B");
    span_b.set_tag("tag", "value");
  }

  return 0;
}
```

This creates a tracer that generates two spans, a parent span `span_a` and a child span `span_b`, and tags them.

次に、コンパイルして `libdd_trace_cpp` に対してリンクします。

```shell
g++ -std=c++17 -o tracer_example tracer_example.cpp -ldd_trace_cpp
```

Finally, run the app with:

```shell
LD_LIBRARY_PATH=/usr/local/lib/ ./tracer_example
```

## Sending traces

Now that an app exists, you can start sending traces and see the Trace Agent in action.

First, tail the Trace Agent log with:

```shell
tail -f /var/log/datadog/trace-agent.log
```

Next, open a new tab and run the example a couple times:

```shell
LD_LIBRARY_PATH=/usr/local/lib/ ./tracer_example
```

On the Trace Agent tab, you will see something similar to:

```text
2019-08-09 20:02:26 UTC | TRACE | INFO | (pkg/trace/info/stats.go:108 in LogStats) | [lang:cpp lang_version:201402 tracer_version:0.2.0] -> traces received: 1, traces filtered: 0, traces amount: 363 bytes, events extracted: 0, events sampled: 0
```

その後、サービスは Datadog のサービスカタログに表示されます。

{{< img src="tracing/guide/setting_up_APM_with_cpp/apm_services_page.png" alt="APM Services Page" >}}

Click on the service to view your traces.

{{< img src="tracing/guide/setting_up_APM_with_cpp/traces_ui.png" alt="APM Traces UI" >}}

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/tracing/setup/cpp/
[2]: https://app.datadoghq.com/account/settings/agent/latest?platform=ubuntu