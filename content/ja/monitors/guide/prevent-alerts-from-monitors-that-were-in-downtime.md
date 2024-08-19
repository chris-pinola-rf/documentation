---
aliases:
- /ja/monitors/faq/why-did-i-get-a-recovery-event-from-a-monitor-that-was-in-a-downtime-when-it-alerted/
- /ja/monitors/faq/i-have-a-downtime-scheduled-on-my-monitor-why-did-it-still-alert/
further_reading:
- link: /monitors/
  tag: ドキュメント
  text: モニターの作成方法
- link: /monitors/notify/
  tag: ドキュメント
  text: モニター通知の設定
- link: /monitors/downtimes/
  tag: ドキュメント
  text: ダウンタイムについて
title: ダウンタイムになったモニターからのアラートを防止する
---

When a group is [downtimed][1] and transitions from **`OK`** to one of a **`ALERT`**, **`WARNING`**, or **`NO DATA`** state, this event is suppressed from notifying you. 
When this downtime ends or is cancelled, this allows both re-notify events (if configured) and recovery events to then be sent. 

An option is to resolve the monitor prior to cancelling the downtime to suppress recovery notifications. However, any groups that were in a non-**`OK`** state could switch back to their previous state, resulting in another notification.

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/monitors/downtimes/