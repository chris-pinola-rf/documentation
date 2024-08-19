---
aliases:
- /ja/graphing/widgets/group/
description: ダッシュボードウィジェットでウィジェットをグループ化します。
further_reading:
- link: /ja/dashboards/graphing_json/
  tag: ドキュメント
  text: JSON を使用したダッシュボードの構築
title: グループウィジェット
widget_type: グループ
---

## Overview
<div class="alert alert-info">Screenboard widgets cannot be placed in groups. </a></div>

The groups widget allows you to keep similar graphs together on your dashboard. Each group has a custom header, can hold one to many graphs, and is collapsible. Use groups to organize the widgets on your dashboard.

## Setup

1. Add several widgets to your dashboard.
2. Select multiple widgets with the click and drag feature, or press Shift and click.
3. Click on the **Group** option.
  {{< img src="dashboards/widgets/group/widget-group-button.png" alt="Group option that appears after you select multiple widgets" style="width:100%;" >}}
4. Click the pencil icon in the upper right corner of your group to choose a name and apply a style to your group.

## API
<div class="alert alert-info">Screenboard widgets cannot be placed in groups. </a></div>

This widget can be used with the **[Dashboards API][2]**. See the following table for the [widget JSON schema definition][3]:

{{< dashboards-widgets-api >}}

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/dashboards/#get-started
[2]: /ja/api/latest/dashboards/
[3]: /ja/dashboards/graphing_json/widget_json/