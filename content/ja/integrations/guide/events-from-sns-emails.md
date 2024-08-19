---
description: Amazon SNS からのメールを使って Datadog にイベントを送信する手順
further_reading:
- link: https://docs.datadoghq.com/integrations/amazon_web_services/
  tag: ドキュメント
  text: AWS インテグレーション
- link: https://docs.datadoghq.com/integrations/amazon_sns/#overview
  tag: ドキュメント
  text: SNS インテグレーション
- link: https://www.datadoghq.com/blog/monitor-aws-fully-managed-services-datadog-serverless-monitoring/
  tag: ブログ
  text: Amazon API Gateway、SQS、Kinesis などの、Datadog サーバーレスモニタリング
title: Amazon SNS のメールから Datadog のイベントを作成する
---

## Overview

You can create Datadog events with emails sent from an Amazon SNS topic. Use this guide to subscribe your Datadog account to your SNS topic and confirm the subscription.

## Setup

1. Create a dedicated email address from Datadog following the setup instructions in the [Events with email][1] guide. Copy the generated email address to your clipboard.
2. From the SNS topic that you want to subscribe to, click **Create subscription** and select `Email` as the protocol. Paste the email address from step 1 in the `Endpoint` field, configure other settings as desired, and click **Create subscription**.
3. In the Datadog [Events Explorer][2], search for an event with the subject `AWS Notification - Subscription Confirmation`. Copy the URL provided for confirmation.

{{< img src="integrations/guide/events_from_sns_emails/events_from_sns_emails.png" alt="The Datadog events explorer showing a detail view of an event with the subject AWS Notification - Subscription Confirmation and a URL highlighted next to the text Confirm Subscription" >}}

4. Open a new tab in your browser, and paste the URL into the address bar. The subscription is confirmed when the browser opens the URL.

### Validation

Return to your SNS topic in the AWS console and ensure that the subscription status is `Confirmed`. New messages published to the topic create events in Datadog.

## Use the events in Datadog

Configure alerting based on the emails from your SNS topic with an [event monitor][3]. Search and filter the events on the [Events Explorer][4], or use a [dashboard][5] to further analyze or display the events.

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/service_management/events/guides/email/
[2]: https://app.datadoghq.com/event/explorer
[3]: /ja/monitors/types/event/
[4]: /ja/events/explorer/
[5]: /ja/dashboards/