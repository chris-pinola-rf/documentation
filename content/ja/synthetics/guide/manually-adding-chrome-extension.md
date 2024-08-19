---
further_reading:
- link: https://www.datadoghq.com/blog/browser-tests/
  tag: ブログ
  text: Datadog ブラウザテストによるユーザーエクスペリエンスの監視
- link: /synthetics/browser_tests
  tag: Documentation
  text: ブラウザテストを作成する
title: ブラウザテストのための Chrome 拡張機能を手動で追加する
---

## Overview

If you are unable to download applications directly from the Chrome Web Store because of security reasons, leverage Datadog's extension detection system, available for the Datadog Synthetics Chrome Extension v3.1.6+, to record Synthetic browser tests.

1. Datadog テストレコーダー拡張機能の[最新の CRX ファイル][1]をダウンロードします。
2. Upload this CRX file to your internal application store and repackage the extension. The new extension's icon appears in the Chrome browser next to your extensions.

   {{< img src="synthetics/guide/manually_adding_chrome_extension/icon.png" alt="the icon that appears in your browser" style="width:100%;" >}}

3. [テストの構成を定義][3] (テスト名、タグ、場所、頻度など) し、**Save Details &amp; Record Test** をクリックして、[ブラウザ テスト][2]を作成します。記録を開始するには、まず、[Datadog テストレコーダー拡張機能][4]をダウンロードします。
4. Click the recorder extension icon on the top right hand corner of your browser. The Datadog test recorder extension automatically detects the extension uploaded in your internal application store.
5. [ブラウザテストの手順の記録][5]を開始し、終了したら **Save Recording** をクリックします。

   {{< img src="synthetics/guide/manually_adding_chrome_extension/record_test.png" alt="record your browser tests" style="width:100%;" >}}

**注:** Datadog は、テストレコーダー拡張機能のアップデートを [Chrome Web Store][4] で公開しています。ブラウザテストを記録するために、内部の拡張機能を手動で更新することができます。

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: https://github.com/DataDog/synthetics-browser-extension
[2]: https://app.datadoghq.com/synthetics/browser/create
[3]: /ja/synthetics/browser_tests/#test-configuration
[4]: https://chrome.google.com/webstore/detail/datadog-test-recorder/kkbncfpddhdmkfmalecgnphegacgejoa?hl=en
[5]: /ja/synthetics/browser_tests/#record-your-steps