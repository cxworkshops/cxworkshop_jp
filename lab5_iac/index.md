---
layout: default
title: ラボ 5 - インフラコードの分析
---

# ラボ 5: インフラコードの分析

{: .important-title }
> 前提条件
>
> ここでは https://github.com/cxworkshops/totallysecureapp にある __totalsecureapp__ プロジェクトを使用します。まだ準備されていない場合は、[ラボ 1](../lab1_setup/) に定義されているようにプロジェクトをローカルパソコンに複製します。

## はじめに

アプリケーション・ソースコードの静的分析に加えて、Checkmarxはインフラコード分析も提供しています。Checkmarx OneプラットフォームにはIaCスキャンエンジンが含まれていますが、Checkmarx KICSというオープンソースソリューションとしても提供されます。KICS は、インフラコードでセキュリティーの脆弱性、コンプライアンスの問題、および誤ったインフラ構成を見つけます。インフラコードの形式は、Terraform、Kubernetes、Docker、AWS CloudFormation、Ansible、Helm、Google Deployment Manager、AWS SAM、Microsoft ARM、Microsoft Azure Blueprints、OpenAPI 2.0 and 3.0、Pulumi、Crossplane、Knative および Serverless Frameworkをサポートします.

Checkmarx VS Codeプラグインを使用すると、KICSはインフラコードファイルを開いたり保存したりするたびに自動的にスキャンできます。これにはCheckmarxアカウントは必要ありません。

## 自動スキャン機能有効化
KICS自動スキャン機能は、Checkmarx Oneプラグインがインストールされるとデフォルトで有効になります。環境で有効になっていることを確認するには、次の手順に従ってください。:

1. VS Code内で、View > Extensionsに移動し、検索バーに"Checkmarx"と入力します。

    !["Manage Extensions"](./assets/images/manage_extensions.png "Manage Extensions")

2. 右側の小さなギアアイコンをクリックし、 __Extension Settings__ を選択します。 __Checkmarx KICS: Activate KICS Auto Scanning__ メニューの選択ボックスが有効になっていることを確認します。:

    !["KICS Auto Scan Setting"](./assets/images/kics_autoscan_setting.png "KICS Auto Scan Setting")

{: .note }
KICS自動スキャンが動作するには、Dockerがインストールされている必要があります。詳細は[ラボ 1](../lab1_setup/)を参照してください。

## KICS自動スキャンの実行

__totallysecureapp__ プロジェクトには多くのインフラコードファイルがあります。この演習では、プロジェクト・フォルダのルートにある __terraform.tf__ ファイルを使用します。

1. VS Code内に出力ウィンドウが表示されていることを確認します（デフォルトの位置：下部ウィンドウ）。"OUTPUT"が表示されない場合は、 __View__ > __Output__ メニューに移動するか、 __Ctrl__+__K__ キーの後に __Ctrl__+__H__ キーを入力してウィンドウを切り替えることができます。OUTPUTウィンドウの右側のドロップダウンメニューから __Checkmarx__ が選択されていることを確認します。

    !["Checkmarx Plugin Output"](./assets/images/checkmarx_plugin_output.png "Checkmarx Plugin Output")

2. VS Codeのファイルエクスプローラを使用して、プロジェクトのルートにある __terraform.tf__ ファイルをダブルクリックします。

3. CheckmarxプラグインはTerraformファイルを認識し、自動的に KICSスキャンを開始します。スキャンがどのように開始されたかを知らせるメッセージが出力ウィンドウに表示されます。:

        [INFO - 5:14:26 PM] Opened a supported file by KICS. Starting KICS scan

4.  KICSスキャンが実行され、完了すると、VS Codeの出力ウィンドウに結果の要約が表示されます。

    !["KICS Autoscan Results"](./assets/images/KICS_autoscan_result.png "KICS Autoscan Results")

5.  さらに、VS Codeは、重大度評価と共に見つかったKICS結果がある行を強調します。


## KICSスキャン結果の修正

1.  以前開いたTerraformファイル内で赤で強調されている最初の行にマウスを置くと、見つかった脆弱性の説明に関するポップアップが表示されます。

    !["KICS High Result"](./assets/images/kics_result_1.png "KICS High Result")

2.  説明ウィンドウの下部に __Quick Fix__ というタイトルのリンクがあります。それをクリックしてください。

    {: .note }
    下部のQuick Fixリンクを表示するには、VS Codeポップアップで少し下にスクロールする必要があります。

3. 2〜4つのオプションが表示されます。:
    - Apply fix to Ram account Password Policy Not Required Minimum Length
    - Apply fix to Ram Account Password Policy Max Password Age Unrecommended
    - Line: Apply all available fixes
    - File: Apply all available fixes


4. __File: Apply all available fixes__ を適用して、すべてのKICS推奨事項を適用します。

    !["KICS Quick Fix"](./assets/images/quick_fix.png "KICS Quick Fix")

5. KICSはファイル内の推奨事項を自動的に適用し、アンダースコアで強調された赤い線が消えることがわかります。

    !["KICS Fixed"](./assets/images/fixed.png "KICS Fixed")

    {: .note }
    自動修正機能を使用すると時間を節約できますが、一般には推奨事項を一度に1つずつ適用してその影響をテストし、アプリケーションやコンテナの機能が損なわれないようにすることをお勧めします。

# メインポイント
- Checkmarx Oneプラットフォームには、Infrastructure-as-code（IaC）スキャンエンジンが含まれており、Checkmarx Oneの外ではKICSとして提供されています。
- KICSは、ユーザー環境内で直接スキャンを実行できるCheckmarx Oneとは独立的に使用できます。
- Checkmarx Oneプラグインは、認識されたIaCファイルがVS Code内で開かれたり保存されたりする際にKICSスキャンを実行するように自動的に設定できます。
- KICSは、VS Code内で直接結果を自動的に強調および列挙できます。
- ユーザーは、1行ずつまたはファイル全体にかけて推奨される修正事項を自動的に適用できます。