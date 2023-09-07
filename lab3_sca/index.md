---
layout: default
title: ラボ 3 - サードパーティコード分析
---

# ラボ 3: サードパーティコード分析
この演習では、SCA（Software Composition Analysis）の脆弱性の内、いくつかの例と解決策について説明します。

{: .important-title }
> 前提条件
>
> ここでは https://github.com/cxworkshops/totallysecureapp にある __totalsecureapp__ プロジェクトを使用します。まだ準備されていない場合は、[ラボ 1](../lab1_setup/) に定義されているようにプロジェクトをローカルパソコンに複製します。

## はじめに

アプリケーションでのオープンソースソフトウェアの採用が加速するにつれて、組織はオープンソースまたはサードパーティの依存関係を使用する時にアプリケーションセキュリティの問題に直面します。Checkmarx のソフトウェア構成分析（SCA）により、組織は遅れる前に、プロジェクト内でサードパーティのパッケージの脆弱性とライセンスのリスクをよりよく追跡、評価、修正できます。

Checkmarx SCAはすべての主要なエコシステムとパッケージマネージャをサポートしています。サポートされている言語とパッケージマネージャの完全なリストは[ここ](https://checkmarx.com/resource/documents/en/34965-117822-supported-languages-and-package-managers.html)にあります。


## SCAの結果確認

Checkmarx VS Codeプラグイン内で、[ラボ 1](../lab1_setup/)の __プロジェクトとのリンク__ にて説明されているように、プロジェクト、ブランチ、スキャン結果への連携が正常であることを確認します。

1. VS Codeの左側のメニューからCheckmarxプラグインに移動し、SCA結果とHIGH結果を展開します。

    {: .note }
    SCSの悪意あるパッケージは常にSCA HIGHの結果として返されます。 __サプライチェーンセキュリティ（SCS）__ に焦点を当てた他の研究所からの悪意あるパッケージも対象とします。

    ![SCA High Results](./assets/images/sca_high_results.png "SCA High Results")

2. __Maven-mysql:mysql-connector-java-5.1.26__ 結果を展開します。

3. __CVE-2018-3258__ を選択し、VS Code内に開いたウィンドウで説明を確認します。

        Vulnerability in the MySQL Connectors component of Oracle MySQL (subcomponent: Connector/J). Supported versions that are affected are 8.0.12 and prior. Easily exploitable vulnerability allows low privileged attacker with network access via multiple protocols to compromise MySQL Connectors. Successful attacks of this vulnerability can result in takeover of MySQL Connectors. CVSS 3.0 Base Score 8.8 (Confidentiality, Integrity and Availability impacts). CVSS Vector: (CVSS:3.0/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H).

4. 推奨される解決策は、 __バージョン8.0.28にアップグレード__ することです。また、deploy/pom.xml内にある脆弱なパッケージパスへのリンクもあります。 __deploy/pom.xml__ をクリックしてください。

    ![SCA MySQL Result](./assets/images/sca_mysql_result.png "SCA MySQL Result")

5. __deploy/pom.xml__ ファイル内でMySQLバージョンを __5.1.26__ から __8.0.28__ バージョンに更新できます。

    {: .note }
    Checkmarx SCAには、見つかった脆弱性を解決するためにパッケージのバージョンをアップグレードすることをお勧めしますが、バージョンを変更するのと同様には簡単でないことを知っています。パッケージのバージョンをアップグレードするときは、テスト環境またはステージング環境で一度に1つずつ遂行し、回帰テストを遂行してアプリケーション内で機能が損なわれたり失われたりしないようにすることをお勧めします。アプリケーションの機能が影響を受ける場合は、内部セキュリティ組織と協力して改善計画を定義したり、脆弱性の影響を最小限に抑えるために他の緩和策を実装することをお勧めします。

## SCA結果の詳細

1. VSCodeのCheckmarxプラグインに組み込まれた結果情報に加えて、[Checkmarx's DevHub, devhub.checkmarx.com](https://devhub.checkmarx.com)サイトで利用可能な豊富な情報があります。:

    ![DevHub](./assets/images/devhub.png "DevHub")

2. [Vulnerabilities Database](https://devhub.checkmarx.com/cve-details/CVE-2018-3258/)内で同じ結果を確認することができます。

3. DevHubに加えて、Checkmarxはプロジェクト内で以前に発見されたパッケージに関する新しい脆弱性が発生した時に電子メール通知を提供することができます。

    ![SCA Email](./assets/images/sca_email.png "SCA Email")

    {: .note }
   CheckmarxはJetBrainsと提携して、JetBrains IntelliJ IDEA UltimateユーザーにSCAスキャン結果を無料で提供します。IntelliJ IDEA Ultimateワークスペース内でバンドルされているプラ​​グインをクリックするだけで、すぐにオープンソースの脅威検索を開始できます。詳しくは、[Checkmarx and Jetbrains Bundled Plugin](https://checkmarx.com/why-checkmarx/checkmarx-and-jetbrains/)をご覧ください。
        ![JetBrains IntelliJ IDEA Ultimate Package Checker Plugin](./assets/images/JetBrains_plugin.png "JetBrains IntelliJ IDEA Ultimate Plugin")


## メインポイント


- Checkmarxは、脆弱性を追跡するためにサードパーティのパッケージに対するデータベースを継続的に監視、テスト、および更新しています。
- Checkmarx SCAは、以前に検知されたパッケージ内で新しい脆弱性が検知されたら、自動的に電子メールを送信することができます。
- 脆弱性データは、Checkmarx One VS Codeプラグイン、[Checkmarx DevHubの脆弱性データベース](https://devhub.checkmarx.com/advisories/) またはCheckmarx One Webインターフェースなど、さまざまな場所から確認できます。