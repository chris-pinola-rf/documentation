---
further_reading:
- link: /real_user_monitoring/
  tag: ドキュメント
  text: RUM とセッションリプレイについて
title: RUM とセッションリプレイの課金
---

## Overview

This page contains common questions and answers about billing topics for RUM & Session Replay.

## How is a session defined?

A session is a user journey on your web or mobile application. A session usually includes multiple page views with their associated telemetry.

## When does a session expire?

15 分間操作が行われないとセッションの期限が切れます。また、セッションの持続時間は 4 時間までに制限されています。4 時間経つと、新しいセッションが自動的に作成されます。

## Session Replay の記録時間はどのくらいですか？

Session Replay の記録時間はセッションの長さによって異なります。例えば、5～8 秒の短い Session Replay を観測している場合、それはユーザーが 5～8 秒後にセッションを終了したことを意味します。

## What data does Datadog RUM & Session Replay collect?

Datadog collects all the pages visited by your end users along with the telemetry that matters, such as resources loading (XHRs, images, CSS files, and JS scripts), frontend errors, crash reports, and long tasks. All of this is included in the user session. For Session Replay, Datadog creates an iframe based on snapshots of the DOM. Datadog charges per one thousand (1,000) sessions ingested in the Datadog Real User Monitoring (RUM) service.

## Does Datadog handle single page applications?

Yes, without any configuration on your side. Datadog RUM automatically tracks page changes.

## How do you view endpoint requests end-to-end?

With the out-of-the-box APM integration, you can tie any XHR or Fetch request to its corresponding backend trace.

## How do you view logs from the browser collector in RUM?

Browser logs are automatically tied to the corresponding RUM session, enabling you to monitor when they happen during the end user journey.

## Does Datadog use cookies?

Yes. Datadog uses cookies to stitch together the various steps of your users into a session. This process does not use cross-domain cookies, and it does not track the actions of your users outside your applications.

## My Usage page shows RUM sessions billed under the Browser RUM & Session Replay Plan, but I have not configured capturing session recordings for my application.

The **Browser RUM & Session Replay** Plan unlocks session recordings (replays).

- If you are collecting replays, you are billed for the sessions under the Replay Plan.

- If you want to disable session recordings from being captured, see the [Session Replay documentation][1].

## モバイルアプリケーションの Web ビューはセッションの記録と請求にどのような影響を与えますか？

モバイルアプリケーションに Web ビューが含まれており、Web アプリケーションとモバイルアプリケーションの両方を Datadog SDK でインスツルメントしている場合、ブリッジが作成されます。Web ビューを通して読み込まれた Web アプリケーション上のブラウザ SDK によって記録されたすべてのイベントは、モバイル SDK に転送されます。これらのイベントは、モバイルアプリケーション上で開始したセッションにリンクされます。

言い換えると、Datadog では 1 つの RUM モバイルセッションのみが表示されるため、請求対象のセッションはその 1 つだけとなります。

{{< img src="account_management/billing/rum/rum-webviews-impact-on-billing-2.png" alt="Web アプリケーションとモバイルアプリケーションの両方を Datadog SDK でインスツルメントした場合、モバイルセッションに対してのみ請求されます。" >}}

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/real_user_monitoring/session_replay/browser#disable-session-replay
