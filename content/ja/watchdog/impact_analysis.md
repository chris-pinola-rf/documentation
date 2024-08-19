---
description: Watchdog Impact Analysis により、エンドユーザーに影響を与えるアプリケーションのパフォーマンス問題を発見できます。
further_reading:
- link: https://www.datadoghq.com/blog/watchdog-impact-analysis/
  tag: ブログ
  text: Watchdog Impact Analysis によるユーザーへの影響範囲の把握
- link: real_user_monitoring/explorer/watchdog_insights/
  tag: ドキュメント
  text: Watchdog Insights for RUM の詳細はこちら
- link: real_user_monitoring/connect_rum_and_traces/
  tag: ドキュメント
  text: RUM とトレースの接続
title: Watchdog Impact Analysis
---

## Overview

Whenever Watchdog finds an APM anomaly, it simultaneously analyzes a variety of latency and error metrics that are submitted from the RUM SDKs to evaluate if the anomaly is adversely impacting any web or mobile pages visited by your users. 

If Watchdog determines that the end-user experience is impacted, it provides a summary of the impacts in Watchdog APM Alert. This includes:

- A list of impacted RUM views
- An estimated number of impacted users
- A link to the list of impacted users, so that you can reach out to them, if needed. 

{{< img src="watchdog/watchdog_impact_analysis.mp4" alt="A user hovering over the users and views pills to show more information about the users impacted and the number of views impacted" video=true >}}

This feature is automatically enabled for all APM and RUM users. Whenever Watchdog APM alerts are associated with end-user impacts, affected **users** and **view paths** appear in the **Impacts** section of your Watchdog alerts. Click **users** to view the affected users' contact information if you need to reach out to them. Click **view paths** to access the impacted RUM views for additional information.

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}