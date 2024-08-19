---
further_reading:
- link: https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Metric-Streams.html
  tag: Documentation
  text: メトリクスストリーム - Amazon CloudWatch
- link: https://www.datadoghq.com/blog/amazon-cloudwatch-metric-streams-datadog/
  tag: ブログ
  text: メトリクススストリームを使用して Amazon CloudWatch メトリクスを収集する
title: Amazon Data Firehose を使用した AWS CloudWatch メトリクスストリーム
---
{{% site-region region="us3,gov" %}}
AWS CloudWatch Metric Streams with Amazon Data Firehose is not available for your selected <a href="/getting_started/site">Datadog site</a> ({{< region-param key="dd_site_name" >}}).
{{% /site-region %}}

Using Amazon CloudWatch Metric Streams and Amazon Data Firehose, you can get CloudWatch metrics into Datadog with only a two to three minute latency. This is significantly faster than Datadog's default API polling approach, which provides updated metrics every 10 minutes. You can learn more about the API polling approach in the [Cloud Metric Delay documentation][1].

## Overview

{{< img src="integrations/guide/aws-cloudwatch-metric-streams-with-kinesis-data-firehose/metric_streaming_diagram.png" alt="Diagram of the metrics flow" responsive="true">}}

1. メトリクスをストリーミングする各 AWS アカウントとリージョンに CloudWatch メトリクスストリームを作成します。
   - オプションで、ストリーミングするためのネームスペースまたはメトリクスの限定されたセットを指定します。
2. メトリクスストリームを作成すると、Datadog はストリーミングされたメトリクスの受信をすぐに開始し、追加の構成を必要とせずにそれらを Datadog サイトに表示します。


### メトリクスストリーミングと API ポーリングの比較 {#streaming-vs-polling}

The following are key differences between using CloudWatch Metric Streams and API polling.

- **AWS でのネームスペースフィルタリング**: AWS インテグレーションページのネームスペースごとのデフォルトとアカウントレベルの設定は、API ポーリングアプローチにのみ適用されます。AWS アカウントの CloudWatch メトリクスストリーム設定を使用して、ストリームにネームスペース/メトリクスを含めたり除外したりするためのすべてのルールを管理します。

- **Metrics that report with more than a two hour delay**: API polling continues to collect metrics like `aws.s3.bucket_size_bytes` and `aws.billing.estimated_charges` after metric streaming is enabled, since these cannot be sent through CloudWatch Metric Stream.

#### Switching from API polling to metric streams
If you already receive metrics for a given CloudWatch namespace through the API polling method, Datadog automatically detects this and stops polling metrics for that namespace once you start streaming them. Leave your configuration settings in the AWS integration page unchanged; as Datadog continues to use API polling to collect custom tags and other metadata for your streamed metrics.

#### Switching back from metric streams to API polling

If you later decide you don't want to stream metrics for a given AWS account and region, or even just for a specific namespace, Datadog automatically starts collecting those metrics using API polling again based on the configuration settings in the AWS integration page. If you want to stop streaming all metrics for an AWS account and region, follow the instructions in the [Disable Metric Streaming section](#disable-metric-streaming) of this document.

### Billing

There is no additional charge from Datadog to stream metrics.

AWS charges based on the number of metric updates on the CloudWatch Metric Stream and the data volume sent to the Amazon Data Firehose. As such, there is a potential to see an increased CloudWatch cost for the subset of metrics you are streaming. For this reason, Datadog recommends using metric streams for the AWS metrics, services, regions, and accounts where you most need the lower latency, and polling for others. For more information, see [Amazon CloudWatch pricing][1].

ストリーム内の EC2 または Lambda メトリクスは、請求対象のホストと Lambda 呼び出しの数を増やす可能性があります (EC2 の場合、これらのホストと関数が AWS インテグレーションまたは Datadog Agent でまだ監視されていない場合)。

## Setup

### Before you begin

1. Read the [Metric Streaming versus API polling](#streaming-vs-polling) section carefully to understand the differences before enabling Metric Streaming. 

2. If you haven't already, connect your AWS account to Datadog. For more information, see the [CloudFormation setup instructions][3].

### Installation

{{< tabs >}}
{{% tab "CloudFormation" %}}

Datadog recommends using CloudFormation because it's automatic and easier if you are using multiple AWS regions.

**Note**: Metric streaming to Datadog currently only supports OpenTelemetry v0.7 output format.

1. On your Datadog site, go to the **Configuration** tab of the [AWS integration page][1].
2. Click on the AWS account to set up metric streaming.
3. Under **Metric Collection**, click on **Automatically Using CloudFormation** under **CloudWatch Metric Streams** to launch a stack in the AWS console.
 {{< img src="integrations/guide/aws-cloudwatch-metric-streams-with-kinesis-data-firehose/metric-stream-setup.png" alt="The CloudWatch Metric Streams section of the Metric Collection tab of the AWS integration page with the Automatically Using CloudFormation button highlighted" responsive="true" style="width:60%;" >}}
4. Fill in the required parameters:
   - **ApiKey**: Add your [Datadog API key][2].
   - **DdSite**: Select your [Datadog site][3]. Your site is: {{< region-param key="dd_site" code="true" >}}
   - **Regions**: A comma-separated list of the regions you wish to set up for metrics streaming. For a full list of supported regions, see the AWS documentation on [Using metric streams][4].
5. Fill in the optional parameters:
   - **FilterMethod**: Include or Exclude list of namespaces to include for metrics streaming.
   - **First/Second/Third Namespace**: Specify the namespaces you wish to include or exclude. Note: The namespace values have to precisely match the values in the namespace column in AWS's documentation. For example, AWS/EC2.
6. Check the acknowledgment box that states, "I acknowledge that AWS CloudFormation might create IAM resources with custom names."
7. Click **Create Stack**.

### Results

Once the stack is successfully created, wait five minutes for Datadog to recognize the change. To validate completion, go to the **Metric Collection** tab in Datadog's [AWS integration page][1] and verify that the activated regions appear for the selected account.

{{< img src="integrations/guide/aws-cloudwatch-metric-streams-with-kinesis-data-firehose/active-region.png" alt="The CloudWatch Metric Streams section of the Metric Collection tab of the AWS integration page with one activated region" responsive="true" style="width:60%;">}}

[1]: https://app.datadoghq.com/integrations/amazon-web-services
[2]: https://app.datadoghq.com/organization-settings/api-keys
[3]: /ja/getting_started/site/
[4]: https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Metric-Streams.html
{{% /tab %}}
{{% tab "AWS Console" %}}

AWS Console を使用してメトリクスストリームをセットアップするには、各 AWS リージョンに対して [CloudWatch メトリクスストリーム][2]を作成します。

**Note**: Metric streaming to Datadog currently only supports OpenTelemetry v0.7 output format.

1. **Quick AWS Partner Setup** を選択し、ドロップダウンメニューから AWS パートナーの宛先として **Datadog** を選択します。
   {{< img src="integrations/guide/aws-cloudwatch-metric-streams-with-kinesis-data-firehose/metric-stream-partner-setup.png" alt="Cloudwatch メトリクスストリームのクイックパートナセットアップ" responsive="true" style="width:60%;">}}
2. メトリクスをストリーミングする Datadog サイトを選択し、[Datadog API キー][1]を入力します。
3. すべての CloudWatch メトリクスをストリーミングするか、特定のネームスペースのみをストリーミングするかを選択します。また、特定のメトリクスを除外するオプションもあります。モニタリングアカウントの場合は、[クロスアカウントストリーミング][5]を有効にすることもできます。
   {{< img src="integrations/guide/aws-cloudwatch-metric-streams-with-kinesis-data-firehose/metric-stream-namespace-filter.png" alt="Cloudwatch メトリクスストリーム" responsive="true" style="width:60%;">}}
4. Under **Add additional statistics**, include the AWS percentile metrics to send to Datadog. See the [CloudFormation template][3] for a list of the percentile metrics Datadog supports through polling.
   {{< img src="integrations/guide/aws-cloudwatch-metric-streams-with-kinesis-data-firehose/percentiles.png" alt="Percentiles" responsive="true" style="width:60%;">}}
5. Assign a name to your metric stream.
6. Click **Create metric stream**.

### Results

Once you see the Metric Stream resource has been successfully created, wait five minutes for Datadog to recognize the change. To validate completion, go to the **Metric Collection** tab in Datadog's [AWS integration page][4] and verify that the activated regions are enabled under **CloudWatch Metric Streams** for the specified AWS account.

{{< img src="integrations/guide/aws-cloudwatch-metric-streams-with-kinesis-data-firehose/active-region.png" alt="The CloudWatch Metric Streams section of the Metric Collection tab of the AWS integration page with one activated region" responsive="true" style="width:60%;">}}
**Note**: If you've already enabled polling CloudWatch APIs, the transition to streaming could cause a brief (up to five minutes) period where the specific metrics you are streaming are double-counted in Datadog. This is because of the difference in timing between when Datadog's crawlers are running and submitting your CloudWatch metrics, and when Datadog recognizes that you have started streaming those metrics and turn off the crawlers.

[1]: https://app.datadoghq.com/organization-settings/api-keys
[2]: https://console.aws.amazon.com/cloudwatch/home?region=us-east-1#metric-streams:streams/create
[3]: https://github.com/DataDog/cloudformation-template/blob/master/aws_streams/streams_single_region.yaml#L168-L249
[4]: https://app.datadoghq.com/integrations/amazon-web-services
[5]: https://docs.datadoghq.com/ja/integrations/guide/aws-cloudwatch-metric-streams-with-kinesis-data-firehose/#cross-account-metric-streaming
{{% /tab %}}
{{< /tabs >}}

### クロスアカウントメトリクスストリーミング
AWS リージョン内の複数の AWS アカウントにまたがる単一のメトリクストリームにメトリクスを含めるには、クロスアカウントメトリクストリーミングを使用します。これにより、共通の宛先のメトリクスを収集するために必要なストリームの数を減らすことができます。これを行うには、モニタリングアカウントに[ソースアカウントを接続][5]し、AWS モニタリングアカウントで Datadog へのクロスアカウントストリーミングを有効にします。

この機能を正しく動作させるには、モニタリングアカウントに以下の権限が必要です。
   * oam:ListSinks
   * oam:ListAttachedLinks

**注:** ストリーミングされたメトリクスのカスタムタグやその他のメタデータを収集するには、ソースアカウントを Datadog とインテグレーションしてください。

### Disable metric streaming

特定の AWS アカウントとリージョンに対してメトリクスストリーミングを完全に無効にするには、AWS メトリクスストリームとその関連リソースを削除する必要があります。Datadog のメトリクスの損失を防ぐために、これらの削除手順に注意深く従うことが重要です。

If you set streaming up with [CloudFormation](?tab=cloudformation#installation):
1. Delete the stack that was created during the setup.

If you set streaming up through the [AWS Console](?tab=awsconsole#installation):
1. Delete the CloudWatch Metric Stream linked to your delivery stream.
2. Delete all resources that were created while setting up the stream, including the S3 and Firehose IAM roles that are associated with the stream.

Once the resources are deleted, wait for five minutes for Datadog to recognize the change. To validate completion, go to the **Metric Collection** tab in Datadog's [AWS integration page][4] and verify that the disabled regions are not displayed under **CloudWatch Metric Streams** for the specified AWS account.

## Troubleshooting

To resolve any issues encountered while setting up Metric Streams or the associated resources, see [AWS Troubleshooting][5].

## Further Reading
 {{< partial name="whats-next/whats-next.html" >}}

[1]: https://aws.amazon.com/cloudwatch/pricing/
[2]: https://docs.datadoghq.com/ja/integrations/amazon_web_services/?tab=roledelegation#setup
[3]: https://app.datadoghq.com/integrations/amazon-web-services
[4]: https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-metric-streams-troubleshoot.html
[5]: https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Unified-Cross-Account-Setup.html