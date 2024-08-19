---
aliases:
- /ja/graphing/widgets/iframe/
description: Datadog のダッシュボードに iframe を含める
further_reading:
- link: /ja/dashboards/graphing_json/
  tag: ドキュメント
  text: JSON を使用したダッシュボードの構築
title: Iframe ウィジェット
widget_type: iframe
---

An inline frame (iframe) is a HTML element that loads another HTML page within the document. The iframe widget allows you to embed a portion of any other web page on your dashboard.

## Setup

{{< img src="dashboards/widgets/iframe/iframe_setup.png" alt="Iframe setup" style="width:80%;">}}

Enter the URL of the page you want to display inside the iframe. If you do not use an HTTPS URL, you may have to configure your browser to allow non-secure content.

## API

This widget can be used with the **[Dashboards API][1]**. See the following table for the [widget JSON schema definition][2]:

{{< dashboards-widgets-api >}}

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/api/latest/dashboards/
[2]: /ja/dashboards/graphing_json/widget_json/