---
aliases:
- /ja/graphing/widgets/image/
description: Datadog のダッシュボードにイメージまたは gif 画像を含める
further_reading:
- link: /ja/dashboards/graphing_json/
  tag: ドキュメント
  text: JSON を使用したダッシュボードの構築
title: イメージウィジェット
widget_type: image
---

The image widget allows you to embed an image on your dashboard. An image can be a PNG, JPG, or animated GIF, hosted where it can be accessed by URL.

{{< img src="dashboards/widgets/image/image.mp4" alt="Image" video="true" style="width:80%;" >}}

## Setup

{{< img src="dashboards/widgets/image/image_setup.png" alt="Image setup" style="width:80%;">}}

1. Enter your image URL.
2. Choose an appearance:
    * Zoom image to cover whole title
    * Fit image on tile
    * Center image on tile

## API

This widget can be used with the **[Dashboards API][1]**. See the following table for the [widget JSON schema definition][2]:

{{< dashboards-widgets-api >}}

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/api/latest/dashboards/
[2]: /ja/dashboards/graphing_json/widget_json/