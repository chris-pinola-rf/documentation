---
algolia:
  category: Documentation
  rank: 70
  subcategory: Synthetic API テスト
  tags:
  - dns
  - dns テスト
  - dns テスト
aliases:
- /ja/synthetics/dns_test
- /ja/synthetics/dns_check
description: DNS レコードの解決可能性とルックアップ時間を監視します
further_reading:
- link: https://www.datadoghq.com/blog/introducing-synthetic-monitoring/
  tag: ブログ
  text: Datadog Synthetic モニタリングの紹介
- link: https://www.datadoghq.com/blog/monitor-dns-with-datadog/
  tag: ブログ
  text: Datadog による DNS モニタリング
- link: /getting_started/synthetics/api_test
  tag: Documentation
  text: API テストの概要
- link: /synthetics/private_locations
  tag: ドキュメント
  text: 内部エンドポイントの DNS 解決をテストする
- link: /synthetics/guide/synthetic-test-monitors
  tag: ドキュメント
  text: Synthetic テストモニターについて
title: DNS テスト
---

## Overview

DNS tests allow you to proactively monitor the resolvability and lookup times of your DNS records using any nameserver. If resolution is unexpectedly slow or a DNS server answers with unexpected A, AAAA, CNAME, TXT, or MX entries, Datadog sends you an alert with details on the failure, allowing you to quickly pinpoint the root cause of the issue and fix it.

DNS tests can run from both [managed](#select-locations) and [private locations][1] depending on your preference for running the test from outside or inside your network. DNS tests can run on a schedule, on-demand, or directly within your [CI/CD pipelines][2].

## Configuration

After choosing to create a `DNS` test, define your test's request.

### Define request

1. Specify the **Domain** you want your test to query. For example, `www.example.com`.
2. Specify the **DNS Server** to use (optional), it can be a domain name or an IP address. If not specified, your DNS test performs resolution using `8.8.8.8`, with a fallback on `1.1.1.1` and an internal AWS DNS server.
3. Specify your DNS Server **Port** (optional). If not specified, the DNS Server port defaults to 53.
4. Specify the amount of time in seconds before the test times out (optional).
5. **Name** your DNS test.
6. Add `env` **Tags** as well as any other tag to your DNS test. You can then use these tags to filter through your Synthetic tests on the [Synthetic Monitoring & Continuous Testing page][3].

{{< img src="synthetics/api_tests/dns_test_config_new.png" alt="Define DNS query" style="width:90%;" >}}

Click **Test URL** to try out the request configuration. A response preview is displayed on the right side of your screen.

### Define assertions

Assertions define what an expected test result is. After you click **Test URL**, basic assertions on `response time` and available records are added. You must define at least one assertion for your test to monitor.

| Type                | Record type                                                     | Operator                                           | Value type                 |
|---------------------|-----------------------------------------------------------------|----------------------------------------------------|----------------------------|
| response time       |                                                                 | `is less than`                                     | _Integer (ms)_             |
| every available record        | of type A, of type AAAA, of type CNAME, of type MX, of type NS, of type TXT | `is`, `contains`, <br> `matches`, `does not match` | _String_ <br> _[Regex][4]_ |
| at least one record | of type A, of type AAAA, of type CNAME, of type MX, of type NS, of type TXT | `is`, `contains`, <br> `matches`, `does not match` | _String_ <br> _[Regex][4]_ |

**Note**: SOA records are not available for testing using Synthetic tests.

**New Assertion** をクリックするか、応答プレビューを直接クリックすることで、API テストごとに最大 20 個のアサーションを作成できます。

{{< img src="synthetics/api_tests/assertions_dns.png" alt="DNS テストが成功または失敗するためのアサーションを定義する" style="width:90%;" >}}

アサーションで `OR` ロジックを実行するには、`matches regex` コンパレータを使用して、`(0|100)` のように同じアサーションタイプに対して複数の期待値を持つ正規表現を定義します。利用可能なすべてのレコード、あるいは少なくともひとつのレコードのアサーションの値が 0 あるいは 100 であれば、テスト結果は成功です。

テストがレスポンス本文にアサーションを含まない場合、本文のペイロードはドロップし、Synthetics Worker で設定されたタイムアウト制限内でリクエストに関連するレスポンスタイムを返します。

テストがレスポンス本文に対するアサーションを含み、タイムアウトの制限に達した場合、`Assertions on the body/response cannot be run beyond this limit` というエラーが表示されます。

### ロケーションを選択する

DNS テストを実行するための **Locations** を選択します。DNS テストは、パブリックドメインまたはプライベートドメインを監視するかに応じて、管理ロケーションおよび[プライベートロケーション][1]の両方から実行することができます。

{{% managed-locations %}} 

### テストの頻度を指定する

DNS テストは次の頻度で実行できます。

* **On a schedule**: 最も重要なサービスにユーザーが常にアクセスできるようにします。Datadog で DNS テストを実行する頻度を選択します。
* [**Within your CI/CD pipelines**][2]。
* **On-demand**: チームにとって最も意味のあるときにいつでもテストを実行します。

{{% synthetics-alerting-monitoring %}}

{{% synthetics-variables %}} 

### 変数を使用する

DNS テストの URL、高度なオプション、アサーションで、[**Settings** ページで定義されたグローバル変数][9]を使用することができます。

変数のリストを表示するには、目的のフィールドに `{{` と入力します。

## テストの失敗

テストが 1 つ以上のアサーションを満たさない場合、またはリクエストが時期尚早に失敗した場合、テストは `FAILED` と見なされます。場合によっては、エンドポイントに対してアサーションをテストすることなくテストが実際に失敗することがあります。

これらの理由には以下が含まれます。

`CONNRESET`
: 接続がリモートサーバーによって突然閉じられました。Web サーバーにエラーが発生した、応答中にシステムが停止した、Web サーバーへの接続が失われた、などの原因が考えられます。

`DNS`
: テスト URL に対応する DNS エントリが見つかりませんでした。テスト URL の構成の誤りまたは DNS エントリの構成の誤りの原因が考えられます。

`INVALID_REQUEST` 
: テストのコンフィギュレーションが無効です (URL に入力ミスがあるなど)。

`TIMEOUT`
: リクエストを一定時間内に完了できなかったことを示します。`TIMEOUT` には 2 種類あります。
  - `TIMEOUT: The request couldn't be completed in a reasonable time.` は、リクエストの持続時間がテスト定義のタイムアウト (デフォルトは 60 秒に設定されています) に当たったことを示します。
  各リクエストについて、ネットワークウォーターフォールに表示されるのは、リクエストの完了したステージのみです。例えば、`Total response time` だけが表示されている場合、DNS の解決中にタイムアウトが発生したことになります。
  - `TIMEOUT: Overall test execution couldn't be completed in a reasonable time.`  は、テスト時間 (リクエスト＋アサーション) が最大時間 (60.5s) に達したことを示しています。

## 権限

デフォルトでは、[Datadog 管理者および Datadog 標準ロール][10]を持つユーザーのみが、Synthetic DNS テストを作成、編集、削除できます。Synthetic DNS テストの作成、編集、削除アクセスを取得するには、ユーザーをこれら 2 つの[デフォルトのロール][10]のいずれかにアップグレードします。

[カスタムロール機能][11]を使用している場合は、`synthetics_read` および `synthetics_write` 権限を含むカスタムロールにユーザーを追加します。

### アクセス制限

アカウントに[カスタムロール][12]を使用しているお客様は、アクセス制限が利用可能です。

組織内の役割に基づいて、DNS テストへのアクセスを制限することができます。DNS テストを作成する際に、(ユーザーのほかに) どのロールがテストの読み取りと書き込みを行えるかを選択します。

{{< img src="synthetics/settings/restrict_access_1.png" alt="テストの権限の設定" style="width:70%;" >}}

## その他の参考資料

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/synthetics/private_locations
[2]: /ja/synthetics/cicd_integrations
[3]: /ja/synthetics/search/#search
[4]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions
[5]: /ja/monitors/notify/#configure-notifications-and-automations
[6]: https://www.markdownguide.org/basic-syntax/
[7]: /ja/monitors/notify/?tab=is_recoveryis_alert_recovery#conditional-variables
[8]: /ja/synthetics/guide/synthetic-test-monitors
[9]: /ja/synthetics/settings/#global-variables
[10]: /ja/account_management/rbac/
[11]: /ja/account_management/rbac#custom-roles
[12]: /ja/account_management/rbac/#create-a-custom-role