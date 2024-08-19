---
aliases:
- /ja/monitors/faq/how-do-i-monitor-ephemeral-servers-for-reboots
further_reading:
- link: /monitors/
  tag: ドキュメント
  text: モニターの作成方法
- link: /monitors/notify/
  tag: ドキュメント
  text: モニター通知の設定
title: エフェメラルサーバーの再起動を監視する
---

Ephemeral environments spin up and terminate hosts constantly, which can make it challenging to distinguish new hosts from rebooted hosts.

You can use a metric monitor on the `system.uptime` metric can to address this. The uptime metric is an ever increasing timer which resets to 0 when a host boots up. You can use the `diff()` function with the metric to distinguish between a new server, which has an uptime of 0 (new server), and a rebooted server, which will show a change (diff) from a running uptime value to a 0.

The example shown below captures how you can set this up:

{{< img src="monitors/guide/ephemeral_set_up.png" alt="ephemeral_set_up" >}}


{{< partial name="whats-next/whats-next.html" >}}