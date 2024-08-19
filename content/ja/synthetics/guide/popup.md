---
further_reading:
- link: https://www.datadoghq.com/blog/browser-tests/
  tag: ブログ
  text: Datadog ブラウザテストによるユーザーエクスペリエンスの監視
- link: synthetics/browser_tests
  tag: Documentation
  text: Synthetic ブラウザテストについて
title: ブラウザテストでのポップアップの処理
---
## Overview

Synthetic の[ブラウザテスト][5]で、モーダルやアプリケーションウィンドウなどのポップアップを管理する方法について説明します。

## Modals

### JavaScript

Synthetic browser tests automatically handle [JavaScript modals][1]:

 - `alert` モーダルは OK の場合は即座に却下されます。
 - Google Chrome または Microsoft Edge のテストでは、`prompt` モーダルが `Lorem Ipsum` で埋められます。
 - 確認を求める `confirm` モーダルは受け付けられます。

### Basic authentication

基本認証ポップアップの場合、ブラウザテスト構成の [**Advanced Options**][2] で関連する資格情報を指定します。

{{< img src="synthetics/guide/popup/http_authentication.png" alt="基本認証ポップアップ" style="width:90%" >}}

## Application pop-ups

### Anchored pop-ups

If a pop-up appears at a specific point of your journey, you can record a step to close it and allow this step to fail using the [corresponding option][3]. This way, your test knows how to behave in case a pop-up appears. If the pop-up does not show up, the step fails without causing the whole test to fail. 

{{< img src="synthetics/guide/popup/allow_fail_option.png" alt="ポップアップを処理するためにステップの失敗を許可する" style="width:60%" >}}

### Moving pop-ups

セッション中にポップアップが表示される時間を予測できない場合は、ブラウザテストの実行中にポップアップが表示されないようにするルールを作成してもらえないか、ポップアップを出すサードパーティーに確認してください。例えば、テストの [**Advanced Options** セクション][2]に挿入できるクッキーなどがあるかもしれません。

Alternatively, use one of these methods to ensure your pop-up is closed and your test is able to continue its journey:
  * Create a [JavaScript assertion][4] at the beginning of your browser test to regularly try to close the pop-up:

    ```javascript
    if (document.querySelector("<ELEMENT>")) {
      return true;
    } else {
      return new Promise((resolve, reject) => {
        const isPopupDisplayed = () => {
          if (document.querySelector("<ELEMENT>")) {
            clearInterval(popup);
            resolve(true);
          }
        };
        let popup = setInterval(isPopupDisplayed, 500);
      });
    }
    ```

  * Record steps to close the pop-up, add them between all your other browser test steps, and select the [**Allow this step to fail** option][3] for each of them.

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: https://javascript.info/alert-prompt-confirm
[2]: /ja/synthetics/browser_tests/#test-configuration
[3]: /ja/synthetics/browser_tests/advanced_options/#optional-step
[4]: /ja/synthetics/browser_tests/actions#test-your-ui-with-custom-javascript
[5]: /ja/synthetics/browser_tests