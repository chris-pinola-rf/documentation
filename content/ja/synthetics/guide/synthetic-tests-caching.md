---
further_reading:
- link: synthetics/browser_tests
  tag: ドキュメント
  text: ブラウザテストの設定
- link: /synthetics/api_tests/http_tests
  tag: ドキュメント
  text: HTTP テストを構成する
title: Synthetic テストでキャッシュの問題を回避する
---

## Overview

このガイドでは、Synthetic テストを使用する際にキャッシュの問題を回避する方法を説明します。

## API tests

### HTTP tests

[ローカル変数][1]を利用し、ランダムな文字列を生成してペイロードに含めることで、[HTTP テスト][2]がキャッシュシステムを使用しないことを確実にできます。

## Browser tests

ブラウザは各テスト実行後に終了するため、ブラウザテストでクライアントサイドのキャッシュ関連の問題が発生するのを回避できます。

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/synthetics/api_tests/http_tests?tab=requestoptions#create-local-variables
[2]: /ja/synthetics/api_tests/http_tests