---
cascade:
  algolia:
    rank: 70
further_reading:
- link: /integrations/kafka/
  tag: ドキュメント
  text: Kafka インテグレーション
- link: /integrations/amazon_sqs/
  tag: ドキュメント
  text: Amazon SQS インテグレーション
- link: /tracing/service_catalog/
  tag: Documentation
  text: サービスカタログ
- link: https://www.datadoghq.com/blog/data-streams-monitoring/
  tag: ブログ
  text: Datadog Data Streams Monitoring でストリーミングデータパイプラインのパフォーマンスを追跡し、改善する
- link: https://www.datadoghq.com/blog/data-streams-monitoring-apm-integration/
  tag: ブログ
  text: Datadog Data Streams Monitoring を利用して、APM から直接ストリーミングデータパイプラインのトラブルシューティングを行う
- link: https://www.datadoghq.com/blog/data-streams-monitoring-sqs/
  tag: ブログ
  text: Data Streams Monitoring による SQS の監視
title: データストリーム モニタリング
---


{{% site-region region="gov,ap1" %}}
<div class="alert alert-warning">
    Data Streams Monitoring is not available for the {{< region-param key="dd_site_name" >}} site.
</div>
{{% /site-region %}}

Data Streams Monitoring provides a standardized method for teams to understand and manage pipelines at scale by making it easy to:
* Measure pipeline health with end-to-end latencies for events traversing across your system.
* Pinpoint faulty producers, consumers or queues, then pivot to related logs or clusters to troubleshoot faster.
* Prevent cascading delays by equipping service owners to stop backed up events from overwhelming downstream services.

## Setup

To get started, follow the installation instructions to configure services with Data Streams Monitoring:

{{< partial name="data_streams/setup-languages.html" >}}

<br/>

| Runtime | Supported technologies |
|---|----|
| Java/Scala | Kafka (self-hosted, Amazon MSK, Confluent Cloud / Platform), RabbitMQ, HTTP, gRPC, Amazon SQS |
| Python | Kafka (self-hosted, Amazon MSK, Confluent Cloud / Platform), RabbitMQ, Amazon SQS |
| .NET | Kafka (self-hosted, Amazon MSK, Confluent Cloud / Platform), RabbitMQ, Amazon SQS |
| Node.js | Kafka (self-hosted, Amazon MSK, Confluent Cloud / Platform), RabbitMQ, Amazon SQS |
| Go | All (with [manual instrumentation][1]) |

## Explore Data Streams Monitoring

### Measure end-to-end pipeline health with new metrics

Once Data Streams Monitoring is configured, you can measure the time it usually takes for events to traverse between any two points in your asynchronous system:

| Metric Name | Notable Tags | Description |
|---|---|-----|
| data_streams.latency | `start`, `end`, `env` | End to end latency of a pathway from a specified source to destination service. |
| data_streams.kafka.lag_seconds | `consumer_group`, `partition`, `topic`, `env` | Lag in seconds between producer and consumer. Requires Java Agent v1.9.0 or later. |
| data_streams.payload_size | `consumer_group`, `topic`, `env` | Incoming and outgoing throughput in bytes.|


You can also graph and visualize these metrics on any dashboard or notebook:

{{< img src="data_streams/data_streams_metric_monitor.png" alt="Datadog Data Streams Monitoring monitor" style="width:100%;" >}}

### Monitor end-to-end latency of any pathway

Depending on how events traverse through your system, different paths can lead to increased latency. With the [**Measure** tab][7], you can select a start service and end service for end-to-end latency information to identify bottlenecks and optimize performance. Easily create a monitor for that pathway, or export to a dashboard.

Alternatively, click a service to open a detailed side panel and view the **Pathways** tab for latency between the service and upstream services.

### Alert on slowdowns in event-driven applications

Slowdowns caused by high consumer lag or stale messages can lead to cascading failures and increase downtime. With out-of-the-box alerts, you can pinpoint where bottlenecks occur in your pipelines and respond to them right away. For supplementary metrics, Datadog provides additional integrations for message queue technologies like [Kafka][4] and [SQS][5].

Through Data Stream Monitoring's out-of-the-box recommended monitors, you can setup monitors on metrics like consumer lag, throughput, and latency in one click. 

{{< img src="data_streams/add_monitors_and_synthetic_tests.png" alt="Datadog Data Streams Monitoring Recommended Monitors" style="width:100%;" caption="Click 'Add Monitors and Synthetic Tests' to view Recommended Monitors" >}}

### Attribute incoming messages to any queue, service, or cluster

High lag on a consuming service, increased resource use on a Kafka broker, and increased RabbitMQ or Amazon SQS queue size are frequently explained by changes in the way adjacent services are producing to or consuming from these entities.

Click on the **Throughput** tab on any service or queue in Data Streams Monitoring to quickly detect changes in throughput, and which upstream or downstream service these changes originate from. Once the [Service Catalog][2] is configured, you can immediately pivot to the corresponding team's Slack channel or on-call engineer.

By filtering to a single Kafka, RabbitMQ, or Amazon SQS cluster, you can detect changes in incoming or outgoing traffic for all detected topics or queues running on that cluster:

### Quickly pivot to identify root causes in infrastructure, logs, or traces 

Datadog automatically links the infrastructure powering your services and related logs through [Unified Service Tagging][3], so you can easily localize bottlenecks. Click the **Infra**, **Logs** or **Traces** tabs to further troubleshoot why pathway latency or consumer lag has increased.

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/data_streams/go#manual-instrumentation
[2]: /ja/tracing/service_catalog/
[3]: /ja/getting_started/tagging/unified_service_tagging
[4]: /ja/integrations/kafka/
[5]: /ja/integrations/amazon_sqs/
[6]: /ja/tracing/trace_collection/runtime_config/
[7]: https://app.datadoghq.com/data-streams/measure