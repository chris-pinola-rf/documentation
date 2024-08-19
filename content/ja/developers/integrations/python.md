---
description: Datadog Agent Integration Developer Tool をインストールします。
title: Datadog Agent Integration Developer Tool をインストールする
---
このドキュメントでは、Agent ベースのインテグレーションで動作する Python 環境のセットアップ方法 (インタープリターと開発者ツールのインストールを含む) について説明します。

## Install Python

多くのオペレーティングシステムには、Python がプリインストールされています。しかし、デフォルトでインストールされている Python のバージョンは、最新の Agent で使用されているものとは異なる場合があります。インテグレーションを実行するために必要なものがすべて揃っていることを確認するために、専用の Python インタープリターをインストールしてください。

{{< tabs >}}

{{% tab "MacOS" %}}
Install Python 3.11 using [Homebrew][1]:

1. Update Homebrew:
   ```
   brew update
   ```

2. Install Python:
   ```
   brew install python@3.11
   ```

3. Check the Homebrew installation output and run any additional commands recommended by the installation script.

4. Python のバイナリが `PATH` にインストールされていることと、正しいバージョンがインストールされていることを確認してください。
   ```
   which python3.11
   ```

   You should see the following output depending on your Mac architecture:
   - ARM (M1+) machines:
     ```
     /opt/homebrew/bin/python3.11
     ```
   - MacOS on Intel machines:
     ```
     /usr/local/bin/python3.11
     ```

[1]: https://brew.sh/
{{% /tab %}}

{{% tab "Windows" %}}
1. Download the [Python 3.11 64-bit executable installer][1] and run it.
1. Select the option to add Python to your PATH.
1. Click **Install Now**.
1. After the installation has completed, restart your machine.
1. Verify that the Python binary is installed in your `PATH`:
   ```
   > where python

   C:\Users\<USER>\AppData\Local\Programs\Python\Python39\python.exe
   ```

[1]: https://www.python.org/downloads/release/python-3115/
{{% /tab %}}

{{% tab "Linux" %}}
For Linux installations, avoid modifying your system Python. Datadog recommends installing Python 3.11 using [pyenv][1] or [miniconda][2].

[1]: https://github.com/pyenv/pyenv#automatic-installer
[2]: https://conda.io/projects/conda/en/stable/user-guide/install/linux.html
{{% /tab %}}

{{< /tabs >}}

## 開発者ツールのインストール

`ddev` CLI をインストールするには 2 つの方法があります。

### GUI を使ってインストール

{{< tabs >}}
{{% tab "MacOS" %}}
1. ブラウザで `.pkg` ファイルをダウンロードします: [ddev-{{&lt; sdk-version "integrations-core" &gt;}}.pkg](https://github.com/DataDog/integrations-core/releases/download/ddev-v{{&lt; sdk-version "integrations-core" &gt;}}/ddev-{{&lt; sdk-version "integrations-core" &gt;}}.pkg)
2. ダウンロードしたファイルを実行し、画面の指示に従います。
3. ターミナルを再起動します。
4. `ddev` コマンドが `PATH` に追加されていることを確認するために、以下のコマンドを実行して `ddev` バージョンを取得します。
   ```shell
   ddev --version
   {{< sdk-version "integrations-core" >}}
   ```
{{% /tab %}}

{{% tab "Windows" %}}
1. ブラウザで以下の `.msi` ファイルのいずれかをダウンロードします。
     - [ddev-{{< sdk-version "integrations-core" >}}-x64.msi (64-bit)](https://github.com/DataDog/integrations-core/releases/download/ddev-v{{< sdk-version "integrations-core" >}}/ddev-{{< sdk-version "integrations-core" >}}-x64.msi)
     - [ddev-{{< sdk-version "integrations-core" >}}-x86.msi (32-bit) ](https://github.com/DataDog/integrations-core/releases/download/ddev-v{{< sdk-version "integrations-core" >}}/ddev-{{< sdk-version "integrations-core" >}}-x86.msi)
2. ダウンロードしたファイルを実行し、画面の指示に従います。
3. ターミナルを再起動します。
4. `ddev` コマンドが `PATH` に追加されていることを確認するために、以下のコマンドを実行して `ddev` バージョンを取得します。
   ```shell
   ddev --version
   {{< sdk-version "integrations-core" >}}
   ```
{{% /tab %}}
{{< /tabs >}}

### コマンドラインからインストール

{{< tabs >}}
{{% tab "MacOS" %}}
1. Download the file using the `curl` command. The -L option allows for redirects, and the -o option specifies the file name to which the downloaded package is written. In this example, the file is written to `ddev-{{< sdk-version "integrations-core" >}}.pkg` in the current directory.
   ```shell
   curl -L -o ddev-{{< sdk-version "integrations-core" >}}.pkg https://github.com/DataDog/integrations-core/releases/download/ddev-v{{< sdk-version "integrations-core" >}}/ddev-{{< sdk-version "integrations-core" >}}.pkg
   ```
2. ダウンロードした `.pkg` ファイルをソースとして指定して、macOS 標準の [`installer`](https://ss64.com/osx/installer.html) プログラムを実行します。インストールするパッケージの名前を `-pkg` パラメーターで指定し、パッケージをインストールするドライブを `-target /` パラメーターで指定します。ファイルは `/usr/local/ddev` にインストールされ、シェルに `/usr/local/ddev` ディレクトリを追加するように指示するエントリが `/etc/paths.d/ddev` に作成されます。これらのフォルダに書き込み権限を与えるには、コマンドに `sudo` を含める必要があります。
   ```shell
   sudo installer -pkg ./ddev-{{< sdk-version "integrations-core" >}}.pkg -target /
   ```
3. ターミナルを再起動します。
4. シェルが `PATH` にある `ddev` コマンドを見つけて実行できることを確認するには、以下のコマンドを使用します。
   ```shell
   ddev --version
   {{< sdk-version "integrations-core" >}}
   ```
{{% /tab %}}

{{% tab "Windows" %}}
1. Windows 標準の [`msiexec`](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/msiexec) プログラムを使用して、`.msi` ファイルの 1 つをソースとして指定して、インストーラーをダウンロードして実行します。パラメーターに `/passive` と `/i` を指定して、無人での通常のインストールをリクエストします。
   - `x64`:
      ```shell
      msiexec /passive /i https://github.com/DataDog/integrations-core/releases/download/ddev-v{{< sdk-version "integrations-core" >}}/ddev-{{< sdk-version "integrations-core" >}}-x64.msi
      ```
   - `x86`:
      ```shell
      msiexec /passive /i https://github.com/DataDog/integrations-core/releases/download/ddev-v{{< sdk-version "integrations-core" >}}/ddev-{{< sdk-version "integrations-core" >}}-x86.msi
      ```
2. ターミナルを再起動します。
3. シェルが `PATH` にある `ddev` コマンドを見つけて実行できることを確認するには、以下のコマンドを使用します。
   ```shell
   ddev --version
   {{< sdk-version "integrations-core" >}}
   ```
{{% /tab %}}
{{< /tabs >}}

### スタンドアロンバイナリからインストール

プラットフォームとアーキテクチャに対応するアーカイブをダウンロードしたら、バイナリを `PATH` にあるディレクトリに展開し、バイナリの名前を `ddev` に変更します。

{{< tabs >}}
{{% tab "MacOS" %}}
- [ddev-{{< sdk-version "integrations-core" >}}-aarch64-apple-darwin.tar.gz](https://github.com/DataDog/integrations-core/releases/download/ddev-v{{< sdk-version "integrations-core" >}}/ddev-{{< sdk-version "integrations-core" >}}-aarch64-apple-darwin.tar.gz)
- [ddev-{{< sdk-version "integrations-core" >}}-x86_64-apple-darwin.tar.gz](https://github.com/DataDog/integrations-core/releases/download/ddev-v{{< sdk-version "integrations-core" >}}/ddev-{{< sdk-version "integrations-core" >}}-x86_64-apple-darwin.tar.gz)
{{% /tab %}}

{{% tab "Windows" %}}
- [ddev-{{< sdk-version "integrations-core" >}}-x86_64-pc-windows-msvc.zip](https://github.com/DataDog/integrations-core/releases/download/ddev-v{{< sdk-version "integrations-core" >}}/ddev-{{< sdk-version "integrations-core" >}}-x86_64-pc-windows-msvc.zip)
- [ddev-{{< sdk-version "integrations-core" >}}-i686-pc-windows-msvc.zip](https://github.com/DataDog/integrations-core/releases/download/ddev-v{{< sdk-version "integrations-core" >}}/ddev-{{< sdk-version "integrations-core" >}}-i686-pc-windows-msvc.zip)
{{% /tab %}}

{{% tab "Linux" %}}
- [ddev-{{< sdk-version "integrations-core" >}}-aarch64-unknown-linux-gnu.tar.gz](https://github.com/DataDog/integrations-core/releases/download/ddev-v{{< sdk-version "integrations-core" >}}/ddev-{{< sdk-version "integrations-core" >}}-aarch64-unknown-linux-gnu.tar.gz)
- [ddev-{{< sdk-version "integrations-core" >}}-x86_64-unknown-linux-gnu.tar.gz](https://github.com/DataDog/integrations-core/releases/download/ddev-v{{< sdk-version "integrations-core" >}}/ddev-{{< sdk-version "integrations-core" >}}-x86_64-unknown-linux-gnu.tar.gz)
- [ddev-{{< sdk-version "integrations-core" >}}-x86_64-unknown-linux-musl.tar.gz](https://github.com/DataDog/integrations-core/releases/download/ddev-v{{< sdk-version "integrations-core" >}}/ddev-{{< sdk-version "integrations-core" >}}-x86_64-unknown-linux-musl.tar.gz)
- [ddev-{{< sdk-version "integrations-core" >}}-i686-unknown-linux-gnu.tar.gz](https://github.com/DataDog/integrations-core/releases/download/ddev-v{{< sdk-version "integrations-core" >}}/ddev-{{< sdk-version "integrations-core" >}}-i686-unknown-linux-gnu.tar.gz)
- [ddev-{{< sdk-version "integrations-core" >}}-powerpc64le-unknown-linux-gnu.tar.gz](https://github.com/DataDog/integrations-core/releases/download/ddev-v{{< sdk-version "integrations-core" >}}/ddev-{{< sdk-version "integrations-core" >}}-powerpc64le-unknown-linux-gnu.tar.gz)
{{% /tab %}}
{{< /tabs >}}