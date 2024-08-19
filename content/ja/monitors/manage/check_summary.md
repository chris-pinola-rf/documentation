---
aliases:
- /ja/monitors/check_summary/
description: Datadog に報告されているすべてのチェックをリストします。
further_reading:
- link: /monitors/
  tag: ドキュメント
  text: モニターの作成方法
- link: /monitors/notify/
  tag: ドキュメント
  text: モニター通知の設定
- link: /monitors/manage/
  tag: ドキュメント
  text: モニターの管理
title: チェック内容のサマリー
---

## Overview

Datadog checks report a status on each run. The [check summary page][1] displays your checks reported in the past day. Potential statuses are:

- `OK`
- `WARNING`
- `CRITICAL`
- `UNKNOWN`

## Search

To find a specific check, use the `filter checks` search box on the check summary page. Click on a check name to see the statuses and tags associated with the check. Filter the list further by using the `filter checks` search box inside the check panel:

{{< img src="monitors/check_summary/check_search.png" alt="Check details" style="width:100%;">}}

## Dashboards

To view your check status on a dashboard, utilize the [Check Status Widget][2].

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: https://app.datadoghq.com/check/summary
[2]: /ja/dashboards/widgets/check_status/