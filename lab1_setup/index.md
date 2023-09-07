---
layout: default
title: ラボ 1 - 環境設定
---

# ラボ 1: 環境設定

## はじめに
この演習では、IDEとCheckmarxプラグインを使用して環境を構成することに焦点を当てます。Checkmarxは複数のIDEをサポートしていますが、このラボでは一般的に無料で使用されるMicrosoft Visual Studio Codeを利用します。Checkmarxは他にも次のIDEと連携できます。:

* Eclipse
* IntelliJ
* Visual Studio
* VS Code

詳しくは、[人気IDEとのCheckmarx連携を確認してください。](https://checkmarx.com/why-checkmarx/integrations/checkmarx-integrations-with-ides/)

{: .warning }
この演習では、[EasyBuggy](https://github.com/k-tamura/easybuggy)に基づく既知の脆弱なJavaプロジェクトを使用して脆弱性検出と修正機能を実演します。このアプリケーションが実行されると、このJavaアプリケーションは、メモリリーク、デッドロック、JVMクラッシュなどによってシステムクラッシュが発生する可能性があります。この演習では、ソースコードをスキャンするCheckmarxソリューションのみを使用するため、プロジェクトを実行する理由も必要もありませんし、__お勧めしません__。もし、プロジェクトを実行したい場合は、ご自身の責任で行ってください。その時は、サンドボックス環境（VM内など）で実行することをお勧めします。

## VS Codeのインストール
まだインストールされていない場合、最初のステップはVS Codeをインストールすることです。

{: .note }
VS Codeがすでにインストールされている場合は、このセクションをスキップできます。演習が終わったら、いつでもCheckmarxプラグインをアンインストール/無効にできます。

1. [https://code.visualstudio.com/download](https://code.visualstudio.com/download)にアクセスして、OS用のVisual Studio Codeインストーラをダウンロードします。
2. パソコンにVS Codeをインストールします。

![VS Codeインストール](./assets/images/vscode_ubuntu_install.png "VS Code Install")


## Checkmarxプラグインのインストール
VS Codeがインストールされたら、Checkmarxプラグインをインストールする必要があります。Visual Studio Code拡張機能は [Visual Studio Code marketplace](https://marketplace.visualstudio.com/items?itemName=checkmarx.ast-results)で使用できます。  Visual Studio Codeコンソールから直接インストールが開始できます。

1. VS Codeを開いてください。
2. VS Code内でExtensionsアイコンをクリックします。
3. 検索ボックスに"Checkmarx"と入力し、一覧から表示された拡張機能の __Install__ ボタンをクリックします。
    ![Checkmarxプラグインのインストール](./assets/images/vscode_extensions.png "Checkmarx VX Code Plugin")

    {: .note }
    Checkmarx SAST xxではなく「Checkmarx」というプラグインを選択したことを確認してください。

4. Checkmarx拡張機能がインストールされ、Checkmarxアイコンが左側のナビゲーションパネルに表示されます。

    ![インストール済みのCheckmarxプラグイン](./assets/images/cx_plugin_installed.png "Checkmarx Plugin Installed")


## Checkmarxプラグインの設定
1. VS Codeコンソールで、Checkmarx拡張機能のアイコンをクリックし、"設定を開く"ボタンをクリックします。Checkmarx設定フォームが開きます。
    ![Checkmarxプラグインの設定](./assets/images/cx_plugin_config.png "Configure Checkmarx Plugin")

2. Checkmarx ASTプラグイン・セクションで、次の詳細を入力します。:


    |         アイテム                       |          価値                |
    |:----------------------                |:-----------------------       |
    | Checkmarx KICS: 追加パラメータ         | \<空白のままにしておきます\>                 |
    | Checkmarx AST: 追加パラメータ          | \<空白のままにしておきます\>                 |
    | Checkmarx AST: Apiキー                | \<ファシリテーターより提供\>                |

3. 入力が完了すると、CheckmarxプラグインはCheckmarx Oneテナントに対して認証を行います。

4. プラグイン設定画面を閉じます。

## プロジェクトのリンク

1. 左ペインの __Project:__ フィールドの上にマウスを置き、鉛筆アイコンをクリックして、検索バーに表示されるプロジェクト名 __cxworkshops/totallysecureapp__ を選択します。

    ![プロジェクト選択](./assets/images/cx_select_project.png "Select the Project")

    {: .note }
    > Checkmarxプラグインv2.0.11のリリース以降、プラグインをCheckmarx Oneテナントに連携するには、AST/One APIキーのみが必要です。プロジェクトに連携しようとした時にVSCode内に404エラーが表示される場合、環境変数がAPIキー（cx_base_auth_uri、cx_base_uri、cx_tenant）でUri/テナント名をオーバーライドした可能性があります。CLIを介してCheckmarx Oneに連携したことがある場合、これらの変数が設定されます。システムからcheckmarxcli.yamlファイルを削除してこの問題を解決できます。
    >
    > Mac OSXおよびLinuxの場合、このファイルは ~/.checkmarx/checkmarxcli.yaml にあります。
    >
    > Windowsの場合、このファイルは %UserProfile%.checkmarx\checkmarxcli.yaml にあります。

2. 左ペインの __Branch:__ フィールドの上にマウスを置き、鉛筆アイコンをクリックし、検索バーに表示されるブランチ __master__ を選択します。

    ![ブランチを選択](./assets/images/cx_select_branch.png "Select a branch")

3. Checkmarxプラグインが設定され、左ペインにスキャン結果が表示されます。

    ![設定完了](./assets/images/cx_scan_results.png "Configuration Finished")


## ローカルパソコンへのプロジェクト複製

1. ターミナルまたはコマンドプロンプトを開き、見つけやすいディレクトリまたはフォルダに移動します（例: Mac OSXまたはLinuxの〜/または％UserProfile％/）。

    ![ターミナル](./assets/images/terminal_dir.png "Terminal")

2. スキャンしたサンプルプロジェクトをローカルパソコンに複製します。

        git clone https://github.com/cxworkshops/totallysecureapp.git

    {: .note }
    まだインストールされていない場合は、ローカルパソコンに __git__ をインストールする必要があります。[このガイド](https://github.com/git-guides/install-git)を使用して、オペレーティングシステムに応じたインストール手順を確認できます。

3. VS Code内で、 __File__ > __Open Folder__ を選択し、 __totallysecureapp__ ディレクトリを選択します。

4.  開発者を信頼しているかを尋ねるVS Codeメッセージが表示されることがあります。このプロジェクトコードを実行せず、ソースを確認するだけなので安全に信頼することができます。演習が終わったら、プロジェクトを安全に削除できます。

    ![VS Codeのプロジェクト信頼](./assets/images/vscode_trust.png "VS Code Trust Project")'


## (オプション) ローカルマシンにDockerをインストール

{: .note }
Checkmarx Infrastructure-as-code（IaC）/KICS自動修正機能（Auto Remediation）を使用するには、ローカルパソコンにDockerをインストールする必要があります。

### Windows/Macユーザー

WindowsまたはMacのパソコンにDockerをインストールする最も簡単な方法は、[Docker Desktop](https://www.docker.com/products/docker-desktop/)を使用することです。

{: .note }
ローカルシステムにDocker Desktopをインストールする前に、Dockerの購読およびライセンス条件([Docker's Subscription and Licensing terms](https://www.docker.com/pricing/faq/))に準拠していることを確認してください。

1. この例では、Ubuntu LinuxシステムにDockerをインストールします。次のコマンドを使用してDockerをインストールします。:

        sudo apt install docker.io

    ![ubuntu docker](./assets/images/docker_install.png "Docker Install")

2. インストール要件と依存パッケージを確認し、準備ができたら"y"と入力します。

3. Linuxで実行している場合は、ユーザーがDockerをroot以外のユーザーとして管理できることを確認する必要があります。これを行う最も簡単な方法は、ユーザーをDockerグループに追加することです。:

        sudo gpasswd -a $USER docker

## メインポイント
- Checkmarxには、すべての主要なIDE用のIDEプラグインがあります。
- Checkmarx One VS CodeプラグインはVisual Studio Marketplace内で使用でき、Checkmarx SASTプラグインとは異なります。
- Checkmarx One VS Codeプラグインは、1つのフィールド（APIキー）を設定してCheckmarx Oneインスタンスに連携できます。
- Checkmarxスキャンの結果は、すべてIDE内で確認できます。
- Dockerは、VS Code内のIaC/KICS自動修正 (Auto Remediation) に必要です。