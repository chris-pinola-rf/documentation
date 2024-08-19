---
algolia:
  category: Documentation
  rank: 70
  subcategory: Synthetic API テスト
  tags:
  - icmp
  - icmp テスト
  - icmp テスト
aliases:
- /ja/synthetics/icmp_test
- /ja/synthetics/icmp_check
description: ホストの可用性を監視し、ネットワークの問題を診断します。
further_reading:
- link: https://www.datadoghq.com/blog/introducing-synthetic-monitoring/
  tag: ブログ
  text: Datadog Synthetic モニタリングの紹介
- link: /getting_started/synthetics/api_test
  tag: Documentation
  text: API テストの概要
- link: /synthetics/private_locations
  tag: Documentation
  text: 内部エンドポイントで ICMP Ping を実行する
- link: /synthetics/guide/synthetic-test-monitors
  tag: ドキュメント
  text: Synthetic テストモニターについて
title: ICMP テスト
---

## 概要

ICMP テストを使用すると、ホストの可用性を監視し、ネットワーク通信の問題を診断できます。Datadog は、エンドポイントへの 1 つ以上の ICMP ping から受信した値をアサートすることにより、接続の問題、ラウンドトリップ時間のクォータを超えるレイテンシー、セキュリティファイアウォールコンフィギュレーションの予期しない変更を検出するのに役立ちます。テストでは、ホストに接続するために必要なネットワークホップ (TTL) の数を追跡し、traceroute の結果を表示して、パスに沿った各ネットワークホップの詳細を検出することもできます。

ICMP テストは、ネットワークの外部または内部のどちらからエンドポイントへの ICMP ping をトリガーするかに応じて、[管理ロケーション](#select-locations)および[プライベートロケーション][1]の両方から実行できます。ICMP テストは、定義されたスケジュールで、オンデマンドで、または [CI/CD パイプライン][2]内から実行できます。

## Configuration

After choosing to create an `ICMP` test, define your test's request.

### Define request

1. Specify the **Domain Name** or **IP address** to run your test on.
2. Select or deselect **Track number of network hops (TTL)**. When selected, this option turns on a "traceroute" probe to discover all gateways along the path to the host destination.
3. Select the **Number of Pings** to trigger per test session. By default, the number of pings is set to four. You can choose to decrease this number or increase it up to ten.
4. **Name** your ICMP test.
5. Add `env` **Tags** as well as any other tags to your ICMP test. You can then use these tags to filter through your Synthetic tests on the [Synthetic Monitoring & Continuous Testing page][3].

{{< img src="synthetics/api_tests/icmp_test_config.png" alt="Define ICMP request" style="width:90%;" >}}

Click **Test URL** to try out the request configuration. A response preview is displayed on the right side of your screen.

### Define assertions

Assertions define what an expected test result is. After you click **Test URL**, basic assertions on `latency`, `packet loss`, and `packet received` are added. You must define at least one assertion for your test to monitor.

| Type          | Aggregation    |Operator                                                                               | Value Type       |
|-----------------|----------------|------------------------------------------------------------------------|------------------|
| latency         | `avg`, `max`, `min`, or `stddev` (aka `jitter`) |`is less than`, `is less than or equal`, <br> `is`, `is more than`, `is more than or equal` | _integer (ms)_    |
| packet loss     | - |`is less than`, `is less than or equal`, `is`, `is more than`, `is more than or equal` | _percentage (%)_ |
| packet received | - |`is less than`, `is less than or equal`, `is`, `is more than`, `is more than or equal` | _integer_        |
| network hops    | - |`is less than`, `is less than or equal`, `is`, `is more than`, `is more than or equal` | _integer_        |

You can create up to 20 assertions per API test by selecting **New Assertion** or by selecting the response preview directly:

{{< img src="synthetics/api_tests/icmp_assertion.png" alt="Define assertions for your ICMP test to succeed or fail on" style="width:90%;" >}}

If a test does not contain an assertion on the response body, the body payload drops and returns an associated response time for the request within the timeout limit set by the Synthetics Worker.

If a test contains an assertion on the response body and the timeout limit is reached, an `Assertions on the body/response cannot be run beyond this limit` error appears.

### Select locations

Select the **Locations** to run your ICMP test from. ICMP tests can run from both managed and [private locations][1] depending on your preference for triggering trigger the ICMP pings from outside or inside your network.

{{% managed-locations %}}

### Specify test frequency

ICMP tests can run:

* **On a schedule** to ensure your most important services are always accessible to your users. Select the frequency at which you want Datadog to run your ICMP test.
* [**Within your CI/CD pipelines**][2].
* **On-demand** to run your tests whenever makes the most sense for your team.

{{% synthetics-alerting-monitoring %}}

{{% synthetics-variables %}}

### Use variables

You can use the [global variables defined on the **Settings** page][8] in the URL and assertions of your ICMP tests.

To display your list of variables, type `{{` in your desired field.

## Test failure

A test is considered `FAILED` if it does not satisfy one or more assertions or if the request prematurely failed. In some cases, the test can fail without testing the assertions against the endpoint.

These reasons include the following:

`DNS`
: DNS entry not found for the test URL. Possible causes include misconfigured test URL or the wrong configuration of your DNS entries.

## Permissions

By default, only users with the [Datadog Admin and Datadog Standard roles][9] can create, edit, and delete Synthetic ICMP tests. To get create, edit, and delete access to Synthetic ICMP tests, upgrade your user to one of those two [default roles][9].

If you are using the [custom role feature][10], add your user to any custom role that includes `synthetics_read` and `synthetics_write` permissions.

### Restrict access

Access restriction is available for customers using [custom roles][11] on their accounts.

You can restrict access to an ICMP test based on the roles in your organization. When creating an ICMP test, choose which roles (in addition to your user) can read and write your test.

{{< img src="synthetics/settings/restrict_access_1.png" alt="Set permissions for your test" style="width:70%;" >}}

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/synthetics/private_locations
[2]: /ja/synthetics/cicd_integrations
[3]: /ja/synthetics/search/#search
[4]: /ja/monitors/notify/#configure-notifications-and-automations
[5]: https://www.markdownguide.org/basic-syntax/
[6]: /ja/monitors/notify/?tab=is_recoveryis_alert_recovery#conditional-variables
[7]: /ja/synthetics/guide/synthetic-test-monitors
[8]: /ja/synthetics/settings/#global-variables
[9]: /ja/account_management/rbac/
[10]: /ja/account_management/rbac#custom-roles
[11]: /ja/account_management/rbac/#create-a-custom-role
