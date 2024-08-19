---
categories:
- ログの収集
- セキュリティ
custom_kind: integration
dependencies:
- https://github.com/DataDog/documentation/blob/master/content/en/integrations/carbon_black.md
description: Carbon Black Defense ログを収集する
doc_link: https://docs.datadoghq.com/integrations/carbon_black/
has_logo: true
integration_id: carbonblack
integration_title: Carbon Black
is_public: true
name: carbon_black
public_title: Datadog-Carbon Black インテグレーション
short_description: Carbon Black Defense ログを収集する
version: '1.0'
---

## Overview

Use the Datadog-Carbon Black integration to forward your Carbon Black EDR events and alerts as Datadog logs.


## Setup

### Installation

Datadog uses Carbon Black's event forwarder and Datadog's Lambda forwarder to collect Carbon Black events and alerts from your S3 bucket.

Carbon Black provides a [Postman collection][1] for the API that you use to create the Carbon Black event forwarder.

#### Configuration

1. [Install the Datadog Forwarder][2].
2. [AWS Management Console にバケットを作成][3]して、イベントを転送します。
3. [Carbon Black フォワーダーがデータを書き込めるように S3 バケットを構成します][4]。
   - **Important**: The S3 bucket must have a prefix with the keyword `carbon-black` in which the CB events come in. This allows Datadog to recognize the source of the logs correctly.
5. [Create an access level in the Carbon Black Cloud console][5].
6. [Create an API key in the Carbon Black Cloud console][6].
7. [Configure the API in Postman][7] by updating the value of the following Postman environment variables with the key created above: `cb_url`, `cb_org_key`, `cb_custom_id`, and `cb_custom_key`.
8. [Create two Carbon Black event forwarders][8] with different names for Carbon Black alerts (`"type": "alert"`) and endpoint events (`"type": "endpoint.event"`).
9. [Setup the Datadog Forwarder to trigger on the S3 bucket][9].


## Troubleshooting

Need help? Contact [Datadog support][10].

[1]: https://documenter.getpostman.com/view/7740922/SWE9YGSs?version=latest
[2]: /ja/logs/guide/forwarder/
[3]: https://community.carbonblack.com/t5/Developer-Relations/Carbon-Black-Cloud-Data-Forwarder-Quick-Setup-amp-S3-Bucket/td-p/89194#create-a-bucket
[4]: https://community.carbonblack.com/t5/Developer-Relations/Carbon-Black-Cloud-Data-Forwarder-Quick-Setup-amp-S3-Bucket/td-p/89194#configure-bucket-to-write-events
[5]: https://community.carbonblack.com/t5/Developer-Relations/Carbon-Black-Cloud-Data-Forwarder-Quick-Setup-amp-S3-Bucket/td-p/89194#create-access-level
[6]: https://community.carbonblack.com/t5/Developer-Relations/Carbon-Black-Cloud-Data-Forwarder-Quick-Setup-amp-S3-Bucket/td-p/89194#create-new-api-key
[7]: https://community.carbonblack.com/t5/Developer-Relations/Carbon-Black-Cloud-Data-Forwarder-Quick-Setup-amp-S3-Bucket/td-p/89194#configure-api-in-postman
[8]: https://community.carbonblack.com/t5/Developer-Relations/Carbon-Black-Cloud-Data-Forwarder-Quick-Setup-amp-S3-Bucket/td-p/89194#create-new-forwarder
[9]: /ja/logs/guide/send-aws-services-logs-with-the-datadog-lambda-function/?tab=awsconsole#collecting-logs-from-s3-buckets
[10]: /ja/help/