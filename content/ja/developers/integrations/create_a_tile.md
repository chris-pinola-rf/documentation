---
aliases:
- /ja/developers/marketplace/offering
description: インテグレーションタイルの開発・公開方法について説明します。
further_reading:
- link: https://www.datadoghq.com/blog/datadog-marketplace/
  tag: ブログ
  text: Datadog Marketplace で監視範囲を拡大する
- link: /developers/integrations/marketplace_offering
  tag: Documentation
  text: Datadog Marketplace で製品を作成する
title: タイルの作成
type: documentation
---
## 概要

このページでは、**Integrations** または **Marketplace** ページに表示される製品を代表するタイルの作成について、テクノロジーパートナーに説明します。

## インテグレーションタイル

The tile serves as a point of entry where customers can learn about your offering, see setup instructions, and install or purchase your offering to unlock out-of-the-box dashboards and additional assets.

{{< img src="developers/integrations/marketplace_or_integrations_tile.png" alt="Integrations や Marketplace ページで、サンプル製品の拡張タイルモーダル" style="width:100%" >}}

* For any offerings that **do not use** the Datadog Agent—including API-based integrations, professional services listings, and software licenses—you only need to create a tile and submit the tile-related files in order to publish your offering. This is called a _tile-only-listing_. Tile-ony listings apply in situations where Datadog does not host any of the code associated with the API-based integrations, and the other supported offering types do not require any code.

* **Agent ベースのインテグレーション**の場合、タイルを作成し、さらに、インテグレーションに関連するすべてのコード (およびタイル関連ファイル) を 1 つのプルリクエストで送信する必要があります。詳しくは、[Agent ベースのインテグレーションを作成する][27]をご覧ください。

<div class="alert alert-info">Integrations または Marketplace ページでタイルを作成する手順については、タブを選択します。</div>

{{< tabs >}}

{{% tab "Integrations ページでタイルを構築する" %}}

{{< img src="developers/integrations/integration_tile.png" alt="Integrations ページに表示される製品の例を表すタイル" style="width:25%" >}}

[**Integrations** ページ][103]にタイルを構築するには

<div class="alert alert-warning">If you have already gone through the steps to create an Agent integration and have built out the scaffolding, you can skip directly to <a href="#complete-the-necessary-integration-asset-files">completing the necessary integration asset files</a>.
</div>

1. `dd` ディレクトリを作成します。

   ```shell
   mkdir $HOME/dd
   ```

   Datadog Development Toolkit は、`$HOME/dd/` ディレクトリで作業していることを想定しています。これは必須ではありませんが、異なるディレクトリで作業する場合は、追加の構成手順が必要です。

2. [`integrations-extras` リポジトリ][102]をフォークします。
3. `integrations-extras` リポジトリを複製します。

   ```shell
   git clone git@github.com:DataDog/integrations-extras.git
   ```

## Datadog Development Toolkit をインストールして構成する

Agent Integration Developer Tool を使用すると、インテグレーションを開発する際に、インテグレーションタイルのアセットとメタデータのスケルトンを生成して、スキャフォールディングを作成することができます。ツールのインストール方法については、[Datadog Agent Integration Developer Tool をインストールする][101]を参照してください。

Agent Integration Developer Tool をインストールしたら、`integrations-extras` リポジトリ用に構成します。

デフォルトの作業用リポジトリとして `integrations-extras` を設定します。

```shell
ddev config set extras $HOME/dd/integrations-extras
ddev config set repo extras
```

`integrations-extras` ディレクトリの複製に `$HOME/dd` 以外のディレクトリを使用した場合は、以下のコマンドを使用して作業リポジトリを設定します。

```shell
ddev config set extras <PATH/TO/INTEGRATIONS_EXTRAS>
ddev config set repo extras
```

## インテグレーションタイルスキャフォールディングにデータを入力する

[Integrations ページ][102]ですぐに使えるようになる Datadog API インテグレーションについては、Datadog Development Toolkit を使用して、タイルのみの出品でスキャフォールディングを作成します。

1. `integrations-extras` ディレクトリの中にいることを確認します。

   ```shell
   cd $HOME/dd/integrations-extras
   ```

1. `-t tile` オプションを付けて `ddev` コマンドを実行します。

   ```shell
   ddev create -t tile "<Offering Name>"
   ```

[101]: https://docs.datadoghq.com/ja/developers/integrations/python
[102]: https://github.com/Datadog/integrations-extras
[103]: https://app.datadoghq.com/integrations

{{% /tab %}}

{{% tab "Marketplace でタイルを構築する" %}}

{{< img src="developers/integrations/marketplace_tile.png" alt="Marketplace ページに表示される製品の例を表すタイル" style="width:25%" >}}

To build a tile on the [**Marketplace** page][104]:

<div class="alert alert-warning">If you have already gone through the steps to create an Agent integration and have built out the scaffolding, you can skip directly to <a href="#complete-the-necessary-integration-asset-files">completing the necessary integration asset files</a>.
</div>

1. [Marketplace リポジトリ][101]へのアクセスリクエストは、[Marketplace 製品の構築][102]を参照してください。

1. `dd` ディレクトリを作成します。

   ```shell
   mkdir $HOME/dd
   ```

   Datadog Development Toolkit コマンドは、`$HOME/dd/` ディレクトリで作業していることを想定しています。これは必須ではありませんが、異なるディレクトリで作業する場合は、追加の構成手順が必要です。

1. マーケットプレイスのリポジトリへのアクセスが許可されたら、`dd` ディレクトリを作成し、`marketplace` リポジトリを複製します。

   ```shell
   git clone git@github.com:DataDog/marketplace.git
   ```

1. 作業するフィーチャーブランチを作成します。

    ```shell
    git switch -c <YOUR INTEGRATION NAME> origin/master
    ```

## Datadog Development Toolkit をインストールして構成する

Agent Integration Developer Tool を使用すると、インテグレーションを開発する際に、インテグレーションタイルのアセットとメタデータのスケルトンを生成して、スキャフォールディングを作成することができます。ツールのインストール方法については、[Datadog Agent Integration Developer Tool をインストールする][103]を参照してください。

Agent Integration Developer Tool をインストールしたら、`marketplace` リポジトリ用に構成します。

デフォルトの作業用リポジトリとして `marketplace` を設定します。

```shell
ddev config set marketplace $HOME/dd/marketplace
ddev config set repo marketplace
```

`marketplace` ディレクトリの複製に `$HOME/dd` 以外のディレクトリを使用した場合は、以下のコマンドを使用して作業リポジトリを設定します。

```shell
ddev config set marketplace <PATH/TO/MARKETPLACE>
ddev config set repo marketplace
```

## インテグレーションタイルスキャフォールディングにデータを入力する

Datadog Development Toolkit を使用して、タイルのみの出品でスキャフォールディングを作成します。

タイルのみの出品のスキャフォールディングを作成するには

1. `marketplace` ディレクトリの中にいることを確認します。

   ```shell
   cd $HOME/dd/marketplace
   ```

2. `-t tile` オプションを付けて `ddev` コマンドを実行します。

   ```shell
   ddev create -t tile "<Offering Name>"
   ```

[101]: https://github.com/Datadog/marketplace
[102]: https://docs.datadoghq.com/ja/developers/integrations/marketplace_offering
[103]: https://docs.datadoghq.com/ja/developers/integrations/python
[104]: https://app.datadoghq.com/marketplace

{{% /tab %}}
{{< /tabs >}}

## 必要なインテグレーションアセットファイルを完成させる

インテグレーションに必要な以下のアセットが揃っていることを確認してください。

{{% integration-assets %}}

### README

`README.md` ファイルを作成したら、以下のセクションを H2s (`##`) として追加し、内容を適宜記入します。

| ヘッダー名 | ヘッダー |
|-------------|--------|
| 概要 | ユーザーに提供する価値やメリット (例えば、すぐに使えるダッシュボード、ユーザーセッションのリプレイ、ログ、アラートなど) を `## Overview` ヘッダーの下に説明文を記述してください。<br><br>この情報は、タイルの **Overview** タブに表示されます。 |
| セットアップ | H3 の見出し (`###`) に分けられた情報を含む、製品を設定するために必要なすべてのステップを含みます。<br>標準的なトピックは以下の通りです。<br><br>- アプリ内インテグレーションタイルを使用してインテグレーションをインストールする。 <br>- Datadog の組織で適切なロールと権限でインテグレーションを構成する。<br>- インテグレーションを購入しインストールしたユーザーがアクセスできる、すぐに使える Datadog の機能 (メトリクス、イベント、モニター、ログ、ダッシュボードなど) にアクセスする。|
| アンインストール | 製品をアンインストールするためのすべてのステップを含めます。この情報は、タイルの **Configure** タブに表示されます。|
| 収集データ  | メトリクス、イベント、サービスチェック、ログなど、インテグレーションによって収集されるデータのデータタイプを指定します (該当する場合)。`metadata.csv` ファイルに追加されたメトリクスは、自動的にこのタブに表示されます。 <br><br> 製品がこのようなデータを提供しない場合は、Data Collected セクションを追加する必要はありません。 |
| サポート | サポートチームへのメール、自社のドキュメントやブログ記事へのリンク、その他のヘルプ情報などを含む連絡先情報を箇条書きで掲載します。 |

When adding links to the `README.md` file, format them using [reference-style links][30]. For example, instead of embedding the URL directly in the text, write `see the [official Datadog documentation][1]` and define the link reference at the bottom of the file like `[1]: https://docs.datadoghq.com/`.

For additional grammar and style advice, see also the [Datadog documentation contributors guidelines][31].
### Media carousel

A media carousel of images and a video is displayed on each tile, allowing users to better understand the functionality and value of your offering through visual aids. To add a video to your tile, send a copy or a download link of your video to <a href="mailto:marketplace@datadoghq.com">marketplace@datadoghq.com</a>. The Marketplace team uploads the video and provides a `vimeo_link` that should be added to the `manifest.json` file.

#### Video

The video must meet the following requirements:

| Video Requirements | Description                                                                           |
|--------------------|---------------------------------------------------------------------------------------|
| Type               | MP4 H.264                                                                             |
| Size               | The maximum video size is 1GB.                                                        |
| Dimensions         | The aspect ratio must be 16:9 exactly and the resolution must be 1920x1080 or higher. |
| Name               | The video file name must be `partnerName-appName.mp4`.                                |
| Video Length       | The maximum video length is 60 seconds.                                               |
| Description        | The maximum number of characters allowed is 300.                                      |

#### Images

Technology Partners can add up to eight images (seven if you are including a video) in a tile's media carousel.

The images must meet the following requirements:

| Image Requirements | Description                                                                                                                                       |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Type               | `.jpg` or `.png`.                                                                                                                                 |
| Size               | The average is around 500KB. The maximum image size is 1MB.                                                                                       |
| Dimensions         | The aspect ratio must be 16:9 exactly and fit these specifications:<br><br>- Width: 1440px<br>- Minimum height: 810px<br>- Maximum height: 2560px |
| Name               | Use letters, numbers, underscores, and hyphens. Do not use spaces.                                                                           |
| Color Mode         | RGB                                                                                                                                               |
| Color Profile      | sRGB                                                                                                                                              |
| Description        | The maximum number of characters allowed is 300.                                                                                                  |

Follow this template to define the `media` object in the `manifest.json` file which includes an image, a video thumbnail, and a video:

{{< code-block lang="json" filename="manifest.json" collapsible="true" >}}
"media": [
      {
        "media_type": "image",
        "caption": "A Datadog Marketplace Integration OOTB Dashboard",
        "image_url": "images/integration_name_image_name.png"
      },
      {
        "media_type": "video",
        "caption": "A Datadog Marketplace Integration Overview Video",
        "image_url": "images/integration_name_video_thumbnail.png",
        "vimeo_id": 123456789
      },
    ],
{{< /code-block >}}

For more information, see [Integrations Assets Reference][22].

## Open a pull request

Before you open a pull request, run the following command to catch any problems with your integration:

```
ddev validate all <INTEGRATION_NAME>
```

Complete the following steps:

1. Commit all changes to your feature branch.
2. Push your changes to the remote repository.
3. Open a pull request that contains your integration tile's asset files (including images) in the [`marketplace`][18] or [`integrations-extras`][26] repository.

After you've created your pull request, automatic checks run to verify that your pull request is in good shape and contains all the required content to be updated.

## Review process

Once your pull request passes all checks, reviewers from the `Datadog/agent-integrations`, `Datadog/ecosystems-review`, and `Datadog/documentation` teams provide suggestions and feedback on best practices.

Once you have addressed the feedback and re-requested reviews, these reviewers approve your pull request. Contact the Marketplace team if you would like to preview the tile in your sandbox account. This allows you to validate and preview your tile before your tile goes live to all customers.

## Troubleshoot errors

Out-of-the-box integrations in the `integrations-extras` repository can run into validation errors when the forked repository is out of date with the origin.

To resolve validation errors, update the forked repository on the GitHub web app:

1. In [GitHub][29], navigate to your forked `integrations-extras` repository.
1. Click **Sync fork** and click **Update branch**.

To rebase and push changes:

1. Update your local `master` branch:
   ```shell
   git checkout master
   git pull origin master
   ```
1. Merge `master` into your feature branch:
   ```shell
   git checkout <your working branch>
   git merge master
   ```
1. If there are any merge conflicts, resolve them. Then, run `git push origin <your working branch>`.

## Go-to-Market (GTM) opportunities

Datadog offers GTM support for Marketplace listings only. To learn more about the Datadog Marketplace, see [Create a Marketplace Offering][28].

## Further reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: https://app.datadoghq.com/marketplace/
[2]: https://docs.datadoghq.com/ja/developers/custom_checks/prometheus/
[3]: https://docs.datadoghq.com/ja/developers/integrations/agent_integration/#write-the-check
[4]: https://docs.datadoghq.com/ja/developers/dogstatsd/
[5]: https://docs.datadoghq.com/ja/api/latest/metrics/
[6]: https://docs.datadoghq.com/ja/logs/faq/partner_log_integration/
[7]: https://docs.datadoghq.com/ja/api/latest/events/
[8]: https://docs.datadoghq.com/ja/api/latest/service-checks/
[9]: https://docs.datadoghq.com/ja/tracing/guide/send_traces_to_agent_by_api/
[10]: https://docs.datadoghq.com/ja/api/latest/incidents/
[11]: https://docs.datadoghq.com/ja/api/latest/security-monitoring/
[12]: https://docs.datadoghq.com/ja/developers/integrations/
[13]: https://docs.datadoghq.com/ja/developers/#creating-your-own-solution
[14]: https://docs.datadoghq.com/ja/api/latest/
[15]: https://docs.datadoghq.com/ja/developers/integrations/oauth_for_integrations/
[16]: https://docs.datadoghq.com/ja/developers/datadog_apps/
[17]: https://app.datadoghq.com/apps/
[18]: https://github.com/Datadog/marketplace
[19]: https://docs.datadoghq.com/ja/developers/integrations/marketplace_offering/#request-access-to-marketplace
[20]: https://www.python.org/downloads/
[21]: https://pypi.org/project/datadog-checks-dev/
[22]: https://docs.datadoghq.com/ja/developers/integrations/check_references/#manifest-file
[23]: https://datadoghq.com/blog/
[24]: https://github.com/DataDog/integrations-extras/tree/master/vantage
[25]: https://docs.datadoghq.com/ja/developers/integrations/python
[26]: https://github.com/Datadog/integrations-extras
[27]: https://docs.datadoghq.com/ja/developers/integrations/agent_integration/
[28]: https://docs.datadoghq.com/ja/developers/integrations/marketplace_offering/
[29]: https://github.com/
[30]: https://www.markdownguide.org/basic-syntax/#reference-style-links
[31]: https://github.com/DataDog/documentation/blob/master/CONTRIBUTING.md