---
further_reading:
- link: /api/latest/synthetics
  tag: API
  text: Synthetics API
- link: https://www.datadoghq.com/blog/private-synthetic-monitoring/
  tag: ブログ
  text: Datadog Synthetic のプライベートロケーションでオンプレミスアプリケーションをテスト
- link: /synthetics/browser_tests
  tag: ドキュメント
  text: ブラウザテストの詳細
title: プログラムによるブラウザテストの管理
---

## Overview

アプリケーションをエンドツーエンドで監視することは、ユーザーの体験を理解する上で非常に重要です。[Datadog テストレコーダー][1]を使用すると、これらの複雑なテストワークフローのための構成を簡素化することができます。しかし、Synthetics のリソースをプログラムで管理し、API または [Terraform][14] でブラウザテストを定義したいと思うかもしれません。

## Manage your browser tests with the API

Datadog recommends creating your browser tests in the Datadog UI first and retrieving your tests configurations with the API.

1. [Create a browser test][2] and [save a recording][3].
2. Use the [Get the list of all tests endpoint][4] to retrieve the list of all Synthetics tests.
3. Filter on `type: browser` and retrieve the `public_ids` of the browser tests you want to manage with the API. 
4. Use the [Get a browser test endpoint][5] to retrieve the configuration files of every browser test.

You can store the browser test configuration files for later usage or use them to duplicate, update, and delete your browser tests programmatically.

## Manage your browser tests with Terraform

[Datadog Terraform プロバイダー][6]を使用すると、Terraform 構成を通じて、ブラウザテストや関連付けられた Synthetics リソースをプログラムで作成・管理することができます。また、既存のリソースを Terraform 構成に[インポート][7]したり、既存のリソースを外部の[データソース][9]として参照することもできます。

### Browser tests

`type` を `browser` に設定した [Synthetic テストリソース][8]を使って、 Terraform でブラウザテストを作成・管理することができます。

### Private locations

カスタムロケーションやセキュリティ保護されたロケーションから Synthetic テストを実行する必要がある場合は、 [プライベートロケーションリソース][10]を使ってテストを実行するプライベートロケーションを作成・管理できます。詳しくは[プライベートロケーション][11]のページをご覧ください。

### Global and local variables

Use the [synthetics global variable resource][12] to create and manage synthetics global variables, which are variables that can be securely shared across tests. You can also create test-specific [local variables with builtins][15] by defining the [config_variable][16] nested schema with `type = "text"` in your synthetic test resources.

### Concurrency cap

[Synthetics 同時実行数の上限リソース][13]を使用すると、並列で実行される Synthetic テストの数を制限することができます。

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: https://chrome.google.com/webstore/detail/datadog-test-recorder/kkbncfpddhdmkfmalecgnphegacgejoa
[2]: /ja/getting_started/synthetics/browser_test#create-a-browser-test
[3]: /ja/getting_started/synthetics/browser_test#create-recording
[4]: /ja/api/latest/synthetics/#get-the-list-of-all-tests
[5]: /ja/api/latest/synthetics/#get-a-browser-test
[6]: https://registry.terraform.io/providers/DataDog/datadog/latest/docs
[7]: https://developer.hashicorp.com/terraform/cli/import
[8]: https://registry.terraform.io/providers/DataDog/datadog/latest/docs/resources/synthetics_test
[9]: https://developer.hashicorp.com/terraform/language/data-sources
[10]: https://registry.terraform.io/providers/DataDog/datadog/latest/docs/resources/synthetics_private_location
[11]: /ja/synthetics/private_locations
[12]: https://registry.terraform.io/providers/DataDog/datadog/latest/docs/resources/synthetics_global_variable
[13]: https://registry.terraform.io/providers/DataDog/datadog/latest/docs/resources/synthetics_concurrency_cap
[14]: https://www.terraform.io/
[15]: https://docs.datadoghq.com/ja/synthetics/api_tests/http_tests/?tab=requestoptions#create-local-variables
[16]: https://registry.terraform.io/providers/DataDog/datadog/latest/docs/resources/synthetics_test#nested-schema-for-config_variable