---
algolia:
  tags:
  - npm
  - ネットワークパフォーマンスモニタリング
aliases:
- /ja/monitors/network_flow_monitors/
- /ja/graphing/infrastructure/network_performance_monitor/
- /ja/network_performance_monitoring/
description: インフラストラクチャー上のポイントツーポイントコミュニケーションのメトリクスを調べます。
further_reading:
- link: https://www.datadoghq.com/blog/cloud-network-monitoring-datadog/
  tag: ブログ
  text: Datadog NPM でクラウドアーキテクチャとアプリの依存関係を監視する
- link: https://www.datadoghq.com/blog/network-performance-monitoring
  tag: ブログ
  text: ネットワークパフォーマンスモニタリング
- link: https://www.datadoghq.com/blog/npm-windows-support/
  tag: ブログ
  text: ネットワークパフォーマンスモニタリングで Windows ホストを監視する
- link: https://www.datadoghq.com/blog/cloud-service-autodetection-datadog/
  tag: ブログ
  text: クラウドサービスの自動検出でクラウドエンドポイントの健全性を監視する
- link: https://www.datadoghq.com/blog/npm-best-practices/
  tag: ブログ
  text: Datadog NPM を始めるためのベストプラクティス
- link: https://www.datadoghq.com/blog/monitor-consul-with-datadog-npm/
  tag: ブログ
  text: Datadog NPM が Consul ネットワーキングに対応
- link: https://www.datadoghq.com/blog/npm-story-centric-ux/
  tag: ブログ
  text: NPM のストーリー中心 UX でネットワーク調査を迅速に開始
- link: https://www.datadoghq.com/blog/monitor-dns-logs-for-network-and-security-datadog/
  tag: Blog
  text: ネットワークとセキュリティ分析のための DNS ログの監視
title: ネットワークパフォーマンスモニタリング
---

## Overview

{{< vimeo url="https://player.vimeo.com/progressive_redirect/playback/670228207/rendition/1080p/file.mp4?loc=external&signature=42d4a7322017fffa6d5cc2e49ddbb7cfc4c6bbbbf207d13a5c9830630bda4ece" poster="/images/poster/npm.png" >}}

Datadog Network Performance Monitoring (NPM) gives you visibility into your network traffic between services, containers, availability zones, and any other tag in Datadog. Connection data at the IP, port, and PID levels is aggregated into application-layer dependencies between meaningful client and server endpoints, which can be analyzed and visualized through a customizable [network page][1] and [network map][2]. Use flow data along with key network traffic and DNS server metrics to:

* Pinpoint unexpected or latent service dependencies
* Optimize costly cross-regional or multi-cloud communication
* Identify outages of cloud provider regions and third-party tools
* Troubleshoot client-side and server-side DNS server issues

NPM makes it simple to monitor complex networks with built in support for Linux and [Windows OS][3] as well as containerized environments that are orchestrated and [instrumented with Istio service mesh][4].

さらに、NPM の機能である[ネットワークパス][5]が非公開ベータ版として提供されており、これによりネットワーク内のホップバイホップのトラフィックを確認することができます。

{{< whatsnext desc="このセクションには、次のトピックが含まれています。">}}
    {{< nextlink href="network_monitoring/performance/setup" >}}<u>セットアップ</u>: ネットワークデータを収集するように Agent を構成します。{{< /nextlink >}}
    {{< nextlink href="network_monitoring/performance/network_analytics" >}}<u>ネットワーク分析</u>: 利用可能な各クライアントとサーバー間のネットワークデータをグラフ化します。{{< /nextlink >}}
    {{< nextlink href="network_monitoring/performance/network_map" >}}<u>ネットワークマップ</u>: タグ間でネットワークデータをマッピングします。{{< /nextlink >}}
    {{< nextlink href="monitors/types/network_performance/" >}}<u>推奨モニター</u>: 推奨される NPM モニターを構成します。{{< /nextlink >}}
{{< /whatsnext >}}

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: https://app.datadoghq.com/network
[2]: https://app.datadoghq.com/network/map
[3]: https://www.datadoghq.com/blog/npm-windows-support/
[4]: https://www.datadoghq.com/blog/monitor-istio-with-npm/
[5]: /ja/network_monitoring/network_path/