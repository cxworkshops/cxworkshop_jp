---
layout: default
title: Lab 3 - 써드파티 코드 분석
---

# Lab 3: 써드파티 코드 분석
이 실습에서 SCA(Software Composition Analysis) 취약점의 몇 가지 예와 해결 방법을 살펴봅니다.

{: .important-title }
> 전제 조건
>
> 우리는 https://github.com/cxworkshops/totallysecureapp 에 있는 __totallysecureapp__ 프로젝트를 사용할 것입니다. 아직 준비하지 않았다면 [Lab 1](../lab1_setup/)에 정의된 대로 프로젝트를 로컬 컴퓨터에 복제합니다.

## 소개

애플리케이션에서 오픈 소스 소프트웨어의 채택이 가속화됨에 따라 조직은 오픈 소스 또는 써드파티 종속성을 사용할 때 애플리케이션 보안 문제에 직면합니다. Checkmarx 소프트웨어 구성 분석을 통해 조직은 너무 늦기 전에 프로젝트 내에서 타사 패키지 취약성과 라이선스 위험을 더 잘 추적, 평가 및 수정할 수 있습니다.

Checkmarx SCA는 모든 주요 생태계 및 패키지 관리자를 지원합니다. 지원되는 언어 및 패키지 관리자의 전체 목록은 [여기](https://checkmarx.com/resource/documents/en/34965-117822-supported-languages-and-package-managers.html) 에서 찾을 수 있습니다.


## SCA 결과 검토하기

Checkmarx VS Code 플러그인 내에서 [Lab 1](../lab1_setup/)에서 __프로젝트 연결__ 에서 설명한 대로 프로젝트, 브랜치, 스캔 결과에 연결이 잘되었는지 확인합니다.

1. VS Code의 왼쪽 메뉴에서 Checkmarx 플러그인으로 이동하여 sca 결과 및 HIGH 결과를 확장합니다.

    {: .note }
    SCS 악성 패키지는 항상 SCA HIGH 결과로 반환됩니다. __공급망 보안(SCS)__ 에 중점을 둔 다른 연구소에서 악성 패키지를 살펴봅니다.

    ![SCA High Results](./assets/images/sca_high_results.png "SCA High Results")

2. __Maven-mysql:mysql-connector-java-5.1.26__ 결과를 확장합니다.

3. __CVE-2018-3258__ 를 선택하고 VS Code내에 열린 창에서 설명을 검토합니다.

        Vulnerability in the MySQL Connectors component of Oracle MySQL (subcomponent: Connector/J). Supported versions that are affected are 8.0.12 and prior. Easily exploitable vulnerability allows low privileged attacker with network access via multiple protocols to compromise MySQL Connectors. Successful attacks of this vulnerability can result in takeover of MySQL Connectors. CVSS 3.0 Base Score 8.8 (Confidentiality, Integrity and Availability impacts). CVSS Vector: (CVSS:3.0/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H).

4. 권장되는 해결 방법은 __8.0.28 버전으로 업그레이드__ 하는 것입니다. 또한 deploy/pom.xml 내에 있는 취약한 패키지 경로에 대한 링크가 있습니다. __deploy/pom.xml__ 을 클릭하세요.

    ![SCA MySQL Result](./assets/images/sca_mysql_result.png "SCA MySQL Result")

5. __deploy/pom.xml__ 파일 내에서 MySQL 버전을 __5.1.26__ 에서 __8.0.28__ 버전으로 업데이트할 수 있습니다.

    {: .note }
    Checkmarx SCA에는 발견된 취약점을 해결하기 위해 패키지 버전을 업그레이드하라는 권장 사항이 종종 있지만 버전을 변경하는 것만큼 간단하지 않은 경우도 있습니다. 패키지 버전을 업그레이드할 때 테스트 또는 스테이징 환경에서 한 번에 하나씩 수행하고 회귀 테스트를 수행하여 애플리케이션 내에서 기능이 손상되거나 손실되지 않도록 하는 것이 좋습니다. 애플리케이션 기능이 영향을 받는 경우 내부 보안 조직과 협력하여 개선 계획을 정의하거나 취약성의 영향을 최소화하기 위한 다른 완화 단계를 구현하는 것이 좋습니다.

## SCA 결과에 대해 자세히 알아보기

1. VSCode의 Checkmarx 플러그인에 내장된 결과 정보 외에도 [Checkmarx's DevHub, devhub.checkmarx.com](https://devhub.checkmarx.com) 사이트에서 이용할 수 있는 풍부한 정보가 있습니다.:

    ![DevHub](./assets/images/devhub.png "DevHub")

2. [Vulnerabilities Database](https://devhub.checkmarx.com/cve-details/CVE-2018-3258/) 내에서 이와 동일한 결과를 볼 수 있습니다.

3. DevHub 외에도 Checkmarx는 프로젝트 내에서 이전에 발견된 패키지와 관련하여 새로운 취약점이 발생할 때 이메일 알림을 제공할 수 있습니다.

    ![SCA Email](./assets/images/sca_email.png "SCA Email")

    {: .note }
    Checkmarx는 JetBrains와 제휴하여 JetBrains IntelliJ IDEA Ultimate 사용자에게 SCA 스캔 결과를 무료로 제공합니다. IntelliJ IDEA Ultimate 작업 공간 내에서 번들로 제공되는 플러그인을 클릭하기만 하면 즉시 오픈 소스 위협 검색을 시작할 수 있습니다. 자세히 알아보시려면 [Checkmarx and Jetbrains Bundled Plugin](https://checkmarx.com/why-checkmarx/checkmarx-and-jetbrains/)을 확인해보세요.
        ![JetBrains IntelliJ IDEA Ultimate Package Checker Plugin](./assets/images/JetBrains_plugin.png "JetBrains IntelliJ IDEA Ultimate Plugin")


## 주요 테이크 아웃


- Checkmarx는 취약점 추적을 위해 타사 패키지에 대한 데이터베이스를 지속적으로 모니터링, 테스트 및 업데이트하고 있습니다.
- Checkmarx SCA는 이전에 식별된 패키지 내에서 새로운 취약성이 감지되면 귀하 또는 귀하의 보안 챔피언에게 자동으로 이메일을 보낼 수 있습니다.
- 취약점 데이타는 Checkmarx One VS Code플러그인 또는 [Checkmarx DevHub 취약점 데이타베이스](https://devhub.checkmarx.com/advisories/) 또는 Checkmarx One 웹 인터페이스 자체를 포함하여 여러 곳에서 확인 할 수 있습니다. 
