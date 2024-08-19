---
aliases:
- /ja/graphing/widgets/note/
description: ダッシュボードウィジェットにテキストを表示します。
further_reading:
- link: /ja/dashboards/graphing_json/
  tag: ドキュメント
  text: JSON を使用したダッシュボードの構築方法について
title: ノート &amp; リンクウィジェット
widget_type: note
---

The **Notes & Links** widget is similar to the [free text widget][1] but contains more formatting and display options. 

**Note**: The Notes & Links widget does not support inline HTML.

## Setup

1. Enter the text you want to display. Markdown is supported.
2. Select a preset template or customize the display options. 
3. Select a text size and the widget's background color.
4. To adjust the position of the text, click on the **Alignment** buttons. To not include padding, click **No Padding**.
5. To include a pointer, click **Show Pointer** and select a position from the dropdown menu.

{{< img src="dashboards/widgets/note/overview.png" alt="Adding text in the Markdown field of the Notes & Links widget editor" style="width:90%;" >}}

When you are ready to create the widget, click **Save**.

This widget supports template variables. Use the `$<VARIABLE_NAME>.value` syntax to dynamically update the widget content.

{{< img src="dashboards/widgets/note/template_variable.png" alt="Using template variables in the Markdown field of the Notes & Links widget editor" style="width:90%;" >}}

In this example, `$env.value` updates the value of a link to the selected environment.

## API

This widget can be used with the **[Dashboards API][2]**. See the following table for the [widget JSON schema definition][3]:


{{< dashboards-widgets-api >}}

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/dashboards/widgets/free_text/
[2]: /ja/api/latest/dashboards/
[3]: /ja/dashboards/graphing_json/widget_json/