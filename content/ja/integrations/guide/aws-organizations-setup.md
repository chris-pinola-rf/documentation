---
description: AWS 組織の Datadog AWS インテグレーションを設定するためのステップ
further_reading:
- link: https://docs.datadoghq.com/integrations/guide/aws-integration-troubleshooting/
  tag: ガイド
  text: AWS インテグレーションのトラブルシューティング
- link: https://www.datadoghq.com/blog/aws-monitoring/
  tag: ブログ
  text: AWS 監視のための主要なメトリクス
- link: https://www.datadoghq.com/blog/cloud-security-posture-management/
  tag: ブログ
  text: Datadog クラウドセキュリティポスチャ管理
- link: https://www.datadoghq.com/blog/datadog-workload-security/
  tag: ブログ
  text: Datadog クラウドワークロードセキュリティでリアルタイムにインフラストラクチャーを保護する
- link: https://www.datadoghq.com/blog/announcing-cloud-siem/
  tag: ブログ
  text: Datadog セキュリティモニタリングが新登場
title: AWS 組織向け AWS インテグレーションマルチアカウント設定
---

## Overview

This guide provides an overview of the process for setting up the [AWS Integration][8] with multiple accounts within an AWS Organization.

The CloudFormation StackSet template provided by Datadog automates the creation of the required IAM role and associated policies in every AWS account under an Organization or Organizational Unit (OU), and configures the accounts within Datadog, eliminating the need for manual setup. Once set up, the integration automatically starts collecting AWS metrics and events for you to start monitoring your infrastructure.

The Datadog CloudFormation StackSet performs the following steps:

1. Deploys the Datadog AWS CloudFormation Stack in every account under an AWS Organization or Organizational Unit.
2. Automatically creates the necessary IAM role and policies in the target accounts.
3. Automatically initiates ingestion of AWS CloudWatch metrics and events from the AWS resources in the accounts.
4. Optionally disables metric collection for the AWS infrastructure. This is useful for Cloud Cost Management (CCM) or Cloud Security Management Misconfigurations (CSM Misconfigurations) specific use cases.
5. Optionally configures CSM Misconfigurations to monitor resource misconfigurations in your AWS accounts.

**Note**: The StackSet does not set up log forwarding in the AWS accounts. To set up logs, follow the steps in the [Log Collection][2] guide.


## Prerequisites

1. **Access to the management account**: Your AWS user needs to be able to access the AWS management account.
2. **An account administrator has enabled Trusted Access with AWS Organizations**: Refer to [Enable trusted access with AWS Organizations][3] to enable trusted access between StackSets and Organizations, to create and deploy stacks using service-managed permissions.

## Setup

To get started, go to the [AWS Integration configuration page][1] in Datadog and click on **Add AWS Account(s)** -> **Add Multiple AWS Accounts** -> **CloudFormation StackSet**.

Click **Launch CloudFormation StackSet**. This opens the AWS Console and loads a new CloudFormation StackSet. Keep the default choice of `Service-managed permissions` on AWS.  

Follow the steps below on the AWS console to create and deploy your StackSet:

1. **Choose a Template**  
Copy the Template URL from the Datadog AWS integration configuration page to use in the `Specify Template` parameter in the StackSet.


2. **Specify StackSet details**
    - Select your Datadog API key on Datadog AWS integration configuration page and use it in the `DatadogApiKey` parameter in the StackSet.
    - Select your Datadog APP key on Datadog AWS integration configuration page and use it in the `DatadogAppKey` parameter in the StackSet.

    - *Optionally:*  
        a. Enable [Cloud Security Management Misconfigurations][5] (CSM Misconfigurations) to scan your cloud environment, hosts, and containers for misconfigurations and security risks.  
        b. Disable metric collection if you do not want to monitor your AWS infrastructure. This is recommended only for [Cloud Cost Management][6] (CCM) or [CSM Misconfigurations][5] specific use cases.

3. **Configure StackSet options**  
Keep the **Execution configuration** option as `Inactive` so the StackSet performs one operation at a time.

4. **Set deployment options**
    - You can set your `Deployment targets` to either deploy the Datadog integration across an Organization or one or more Organizational Units.


    - Keep `Automatic deployment` enabled in order to automatically deploy the Datadog AWS Integration in new accounts that are added to the Organization or OU.

    - Under **Specify regions**, select a single region in which you'd like to deploy the integration in each AWS account.   
      **NOTE**: The StackSet creates global IAM resources that are not region specific. If multiple regions are selected in this step, the deployment fails. 

    - Set the default settings under **Deployment options** to be sequential, so StackSets operations are deployed into one region at a time.

5. **Review**  
    Go to the **Review** page and click **Submit**. This launches the creation process for the Datadog StackSet. This could take several minutes depending on how many accounts need to be integrated. Ensure that the StackSet successfully creates all resources before proceeding.

    After the stacks are created, go back to the AWS integration config page in Datadog and click **Done**. It may take a few minutes to see metrics and events reporting from your newly integrated AWS accounts.


## Enable integrations for individual AWS services

See the [Integrations page][4] for a full listing of the available sub-integrations that can be enabled on each monitored AWS account. Any sub-integration sending data to Datadog is automatically installed when data is received from the integration.

## Send logs

The StackSet does not set up log forwarding in the AWS accounts. To set up logs, follow the steps in the [Log Collection][2] guide.

## Uninstall AWS Integration

To uninstall the AWS integration from all AWS accounts and regions in an Organization, first delete all StackInstances and then the StackSet. Follow the steps outlined in [Delete a stack set][7] to delete the created StackInstances and StackSet. 

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: https://app.datadoghq.com/integrations/amazon-web-services/
[2]: /ja/integrations/amazon_web_services/#log-collection
[3]: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-orgs-enable-trusted-access.html
[4]: /ja/integrations/#cat-aws
[5]: /ja/security/cloud_security_management/setup/
[6]: https://docs.datadoghq.com/ja/cloud_cost_management/?tab=aws
[7]: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-delete.html
[8]: https://docs.datadoghq.com/ja/integrations/amazon_web_services/