---
further_reading:
- link: https://www.datadoghq.com/blog/route-logs-with-datadog-log-forwarding/
  tag: GitHub
  text: Datadog ログ転送でサードパーティシステムにログをルートする
- link: /logs/log_collection
  tag: ドキュメント
  text: ログの収集開始
- link: /logs/log_configuration/pipelines
  tag: ドキュメント
  text: ログパイプラインについて
- link: /observability_pipelines/
  tag: ドキュメント
  text: Forward logs directly from your environment with Observability Pipelines
title: カスタム宛先へのログの転送
---

{{% site-region region="gov" %}}
<div class="alert alert-warning">
ガバメントサイトでは、ログ転送はご利用いただけません。詳しくはアカウント担当者にお問い合わせください。
</div>
{{% /site-region %}}

## 概要

ログ転送を使用すると、Datadog から Splunk、Elasticsearch、HTTP エンドポイントなどのカスタム宛先にログを送信することができます。つまり、[ログパイプライン][1]を使用して、Datadog でログを一元的に収集、処理、標準化することが可能です。その後、Datadog から他のツールにログを送信し、各チームのワークフローをサポートします。取り込まれたログは、インデックスの有無にかかわらず、カスタム宛先に転送することができます。ログは JSON 形式で転送され、GZIP で圧縮されます。

**Note**: Only Datadog users with the [`logs_write_forwarding_rules`][2] permission can [create][6], [edit][7], and [delete][8] custom destinations for forwarding logs.

{{< img src="logs/log_configuration/forwarding/forwarding_page.png" alt="The Log Forwarding page, showing custom destinations highlighted. The list of destinations includes Splunk (filtered by service:logs-processing), HTTP Endpoint (filtered by source:okta OR source:paloalto), and Elasticsearch (filtered by team:acme env:prod)." >}}

If a forwarding attempt fails (for example: if your destination temporarily becomes unavailable), Datadog retries periodically for 2 hours using an exponential backoff strategy. The first attempt is made following a 1-minute delay. For subsequent retries, the delay increases progressively to a maximum of 8-12 minutes (10 minutes with 20% variance).

The following metrics report on logs that have been forwarded successfully, including logs that were sent successfully after retries, as well as logs that were dropped.

- datadog.forwarding.logs.bytes
- datadog.forwarding.logs.count


## Set up log forwarding to custom destinations

1. Add webhook IPs from the {{< region-param key="ip_ranges_url" link="true" text="IP ranges list">}} to the allowlist.
2. Navigate to [Log Forwarding][4].
3. Select **Custom Destinations**.
4. Click **New Destination**.
5. Enter the query to filter your logs for forwarding. See [Search Syntax][5] for more information.
6. Select the **Destination Type**.

{{< img src="logs/log_configuration/forwarding/tag-forwarding.png" alt="The destination configuration page, showing the steps to set up a new destination." style="width:70%;">}}

{{< tabs >}}
{{% tab "HTTP" %}}

6. Enter a name for the destination.
7. In the **Define endpoint** field, enter the endpoint to which you want to send the logs. The endpoint must start with `https://`.
    - For example, if you want to send logs to Sumo Logic, follow their [Configure HTTP Source for Logs and Metrics documentation][1] to get the HTTP Source Address URL to send data to their collector. Enter the HTTP Source Address URL in the **Define endpoint** field.
8. In the **Configure Authentication** section, select one of the following authentication types and provide the relevant details:
    - Basic Authentication: Provide the username and password for the account to which you want to send logs.
    - Request Header: Provide the header name and value. For example, if you use the Authorization header and the username for the account to which you want to send logs is `myaccount` and the password is `mypassword`:
        - Enter `Authorization` for the **Header Name**.
        - The header value is in the format of `Basic username:password`, where `username:password` is encoded in base64. For this example, the header value is `Basic bXlhY2NvdW50Om15cGFzc3dvcmQ=`.

[1]: https://help.sumologic.com/docs/send-data/hosted-collectors/http-source/logs-metrics/
{{% /tab %}}

{{% tab "Splunk" %}}

6. Enter a name for the destination.
7. In the **Configure Destination** section, enter the endpoint to which you want to send the logs. The endpoint must start with `https://`. For example, enter `https://<your_account>.splunkcloud.com:8088`.
    **Note**: `/services/collector/event` is automatically appended to the endpoint.
8. In the **Configure Authentication** section, enter the Splunk HEC token. See [Set up and use HTTP Event Collector][1] for more information about the Splunk HEC token.
    **Note**: The [indexer acknowledgment][2] needs to be disabled.

[1]: https://docs.splunk.com/Documentation/Splunk/9.0.1/Data/UsetheHTTPEventCollector
[2]: https://docs.splunk.com/Documentation/Splunk/9.0.3/Data/AboutHECIDXAck
{{% /tab %}}

{{% tab "Elasticsearch" %}}

6. Enter a name for the destination.
7. In the **Configure Destination** section, enter the following details:
    a. The endpoint to which you want to send the logs. The endpoint must start with `https://`. An example endpoint for Elasticsearch: `https://<your_account>.us-central1.gcp.cloud.es.io`.
    b. The name of the destination index where you want to send the logs.
    c. Optionally, select the index rotation for how often you want to create a new index: `No Rotation`, `Every Hour`, `Every Day`, `Every Week`, or `Every Month`. The default is `No Rotation`.
8. In the **Configure Authentication** section, enter the username and password for your Elasticsearch account.

{{% /tab %}}
{{< /tabs >}}

9. In the **Select Tags to Forward** section:
  a. Select whether you want **All tags**, **No tags**, or **Specific Tags** to be included.
  b. Select whether you want to **Include** or **Exclude specific tags**, and specify which tags to include or exclude.
10. Click **Save**.

On the [Log Forwarding][4] page, hover over the status for a destination to see the percentage of logs that matched the filter criteria and have been forwarded in the past hour.

## Edit a destination
1. Navigate to [Log Forwarding][4].
2. Select **Custom Destinations** to view a list of all existing destinations.
3. Click the **Edit** button for the destination you want to edit.
4. Make the changes on the configuration page.
5. Click **Save**.

## Delete a destination
1. Navigate to [Log Forwarding][4].
2. Select **Custom Destinations** to view a list of all existing destinations.
3. Click the **Delete** button for the destination that you want to delete, and click **Confirm**. This removes the destination from the configured list of destinations and logs are no longer forwarded to it.

## Further reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/logs/log_configuration/pipelines/
[2]: /ja/account_management/rbac/permissions/?tab=ui#log-management
[4]: https://app.datadoghq.com/logs/pipelines/log-forwarding/custom-destinations
[5]: /ja/logs/explorer/search_syntax/
[6]: /ja/logs/log_configuration/forwarding_custom_destinations#set-up-log-forwarding-to-custom-destinations
[7]: /ja/logs/log_configuration/forwarding_custom_destinations#edit-a-destination
[8]: /ja/logs/log_configuration/forwarding_custom_destinations#delete-a-destination
