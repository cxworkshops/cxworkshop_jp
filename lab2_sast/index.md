---
layout: default
title: ラボ 2 - 開発コードの分析
---

# ラボ 2: 開発コードの分析

{: .important-title }
> 前提条件
>
> ここでは https://github.com/cxworkshops/totallysecureapp にある __totalsecureapp__ プロジェクトを使用します。まだ準備されていない場合は、[ラボ 1](../lab1_setup/) に定義されているようにプロジェクトをローカルパソコンに複製します。

## はじめに
SAST（Static Application Security Testing）を使用すると、開発者とセキュリティチームがアプリケーションのソースコードをスキャンしてアプリケーション内の脆弱性を引き起こす弱点を見つけ、索引付けして列挙できます。Checkmarxは[30個以上の言語とフレームワーク](https://checkmarx.com/resource/documents/en/34965-46283-supported-code-languages-and-frameworks-for-9-5-0.html)をサポートし、設定可能なスキャンプリセットを活用して、AppSecと開発チームが結果を理解し、関心のある結果に集中できるようにします。

Checkmarx SASTは、バイナリではなくソースコードをスキャンして、チームが不完全または部分的なコードをスキャンできるようにすることで、セキュリティに対する反復的なアプローチを提供します。また、Checkmarx OneはSCM（GitHub、GitLab、BitBucket、Azure）とシームレスに連携されるため、プッシュイベントとプルリクエストイベント中にスキャンを自動的にトリガーできるため、スキャンを手動でトリガーまたはスケジュールする必要はありません。

デフォルトでは、Checkmarx OneのSASTは最適化されたプリセットを使用します。このプリセットには、Checkmarx AppSec Acceleratorチームが、長年の研究と顧客との直接的な経験を通じて、誤検出を最小限に抑えながら重要な脆弱性を特定することとの最適なバランスであると考える脆弱性のサブセットが含まれています.

プリセットに加えて、Checkmarx SASTはクエリをカスタマイズする機能をサポートしており、組織がクエリを追加または変更して誤検出を減らし、カスタムサニタイザーへのクエリを追加したり、誤検出を解決したりできます。

## TotallySecureAppプロジェクトをオープン

1. VS Code内で __File__ > __Open Folder...__ に移動し、前にtotallysecureappを複製したフォルダを選択します。(例: ~/totallysecureapp または %USERPROFILE%\totallysecureapp)

    ![VS Code Open Folder](./assets/images/vscode_openfolder.png "VS Code Open Folder")

2. VS Codeは、プロジェクトファイルを表示するエクスプローラ内で __totallysecureapp__ フォルダを開きます。

    ![VS Code Explorer](./assets/images/vscode_explorer.png "VS Code Explorer")

## SAST結果の確認

1. VS Code Checkmarxプラグイン内で最新のスキャン結果を展開させます。

    ![VS Code Plugin](./assets/images/vscode_cx_plugin.png "VS Code Plugin")

2. __M__, __L__, 及び __I__ のアイコンをクリックして __HIGH__ の結果以外のすべてをフィルタリングし、スキャン結果の __Reflected XSS All Clients (/OpenRedirectController.java:32)__ を選択します。

    ![SAST Filter](./assets/images/sast_filter.png "SAST Filter")

3. __sast > HIGH__ メニューを展開してSASTスキャン結果を確認します。
4. __SQL Injection (/SQLInjectionController.java:30)__ 結果を選択します。

    ![SAST High](./assets/images/sast_high.png "SAST High")

5. 攻撃ベクトルと共に検知された脆弱性の詳細な説明を提供するVS Codeが右側にあるウィンドウから開きます。

    ![SQL Injection](./assets/images/sqli.png "SQL Injection")

6. Checkmarxはソースコードをスキャンしてデータフローグラフを作成するため、脆弱性のソースとシンクを検知し、脆弱性を解決するための最適な修正個所を特定できます。最初の攻撃ベクトルリンク __1. "name" ...ggy4sb/vulnerabilities/SQLInjectionController.java [30:88]__ をクリックすると、VS Codeが特定のファイルを開き、Checkmarxが脆弱性のソースとして検知したパラメータを強調します。

    ![SQL Injection Best Fix Location](./assets/images/sqli_bfl.png "SQL Injection Best Fix Location")

7. コードをみると、名前を直接入力し、トリムして空白か否かのみをチェックしていることを、結果内で確認できます。それ以外の場合は、後でSQLクエリの一部として実行されるように入力をそのまま渡します。これはSQLインジェクション攻撃に繋がる危険性があります。SQLインジェクション攻撃でのリスクの詳細説明は、一番右ペインにある __Learn More__ から確認できます。:

    ![SQL Injection Learn More](./assets/images/sqli_learnmore.png "SQL Injection Learn More")

8. コードサンプルをクリックして、コード内でサニタイザーを実装し、SQLインジェクションの脆弱性を軽減する方法の例を確認します。

    ![SQL Injection Code Sample](./assets/images/sqli_code_sample.png "SQL Injection Code Sample")


## 結果のトリアージ
Checkmarx SASTはデータフローの可視性があるため、サニタイザーの存在を確認してソースコードの脆弱性を検知できますが、制御フローベースの有効性検証を検知する機能は比較的に制限されています。おそらく、私たちはコードが脆弱性にさらされないようにするためにアプリケーション内に有効性検証を持っているでしょう。IDE 内で結果を"悪用できない"とマークすることで、今後のスキャン時に返されないように結果をトリアージできます。

1. __Reflected XSS All Clients__ 結果のいずれかを選択します。

2. 右側のウィンドウで、2番目のドロップダウンを __To Verify__ から __Not Exploitable__ に変更し、次のコメントを入力します。

        We have a validator in our code to ensure that this variable will only include known inputs.

    ![Triage Result](./assets/images/triage.png "Triage Results")

3. __Update__ をクリックします。

4. 結果はSAST Highのリストから消えます。再確認するには、フィルタアイコンを選択して __"Not Exploitable"__ 以外のすべてのオプションを選択解除します。
    
    ![Not Exploitable](./assets/images/not_exploitable.png "Not Exploitable")



## メインポイント
- Checkmarx SASTはバイナリではなくソースコードをスキャンし、開発チームがコードをビルドしながら繰り返してスキャンできるようにします。
- Checkmarx Oneはすべての主要SCMと連携され、プッシュおよびプルリクエストイベントのスキャンを自動的にトリガーできます。
- SAST結果はIDE内で直接表示できるため、結果を確認するために他のツールやサイトに移動する必要はありません。
- コード内でのコントロールを実装するために、IDE内で詳細な分析と推奨事項と共に"Best Fix Location"を表示します。
- IDE 内で結果を"not exploitable"とマークすることで、今後のスキャンで返されないように結果をトリアージできます。