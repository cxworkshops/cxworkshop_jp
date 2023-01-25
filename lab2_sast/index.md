---
layout: default
title: Lab 2 - 개발 코드 분석
---

# Lab 2: 개발 코드 분석

{: .important-title }
> 전제 조건
>
> 우리는 https://github.com/cxworkshops/totallysecureapp 에 있는 __totalsecureapp__ 프로젝트를 사용할 것 입니다. 아직 준비하지 않은 경우 [Lab 1](../lab1_setup/) 에 정의된 대로 프로젝트를 로컬 컴퓨터에 복제합니다. 

## 소개
SAST(Static Application Security Testing)을 사용하면 개발자와 보안 팀이 애플리케이션 소스 코드를 스캔하여 애플리케이션 내 취약성을 유발하는 약점을 찾고 색인화하고 열거할 수 있습니다. Checkmarx는 [30개 이상의 언어와 프레임워크](https://checkmarx.com/resource/documents/en/34965-46283-supported-code-languages-and-frameworks-for-9-5-0.html)를 지원하고 구성 가능한 스캔 프리셋를 활용하여 AppSec과 개발 팀이 결과를 이해하고 관심있는 결과에만 집중할 수 있도록 합니다.

Checkmarx SAST는 바이너리가 아닌 소스 코드를 스캔하여 팀이 불완전하거나 부분적인 코드를 스캔할 수 있도록 하여 보안에 대한 반복적인 접근 방식을 제공합니다. 또한 Checkmarx One은 SCM(GitHub, GitLab, BitBucket, Azure)과 원활하게 통합되기 때문에 푸시 및 풀 요청 이벤트 중에 스캔을 자동으로 트리거할 수 있으므로 스캔을 수동으로 트리거하거나 예약할 필요가 없습니다.

기본적으로 Checkmarx One의 SAST는 최적화된 프리셋을 사용합니다. 이 프리셋에는 Checkmarx AppSec Accelerator 팀이 수년간의 연구와 고객과의 직접적인 경험을 통해 오탐을 최소화하면서 중요한 취약성을 식별하는 것 사이의 최적의 균형이라고 생각하는 취약성의 하위 집합이 포함되어 있습니다.

프리셋 외에도 Checkmarx SAST는 쿼리를 사용자 지정하는 기능을 지원하여 조직이 쿼리를 추가 또는 수정하여 오탐을 줄이고, 사용자 정의 새니타이저에 대한 쿼리를 추가하거나, 오탐을 해결할 수 있도록 합니다.

## TotallySecureApp 프로젝트 열기

1. VS Code 내에서 __File__ > __Open Folder...__ 로 이동하고 이전에 totallysecureapp를 복제한 폴더를 선택합니다. (예. ~/totallysecureapp 또는 %USERPROFILE%\\totallysecureapp)

    ![VS Code Open Folder](./assets/images/vscode_openfolder.png "VS Code Open Folder")

2. VS Code는 프로젝트 파일을 볼 수 있는 탐색기 내에서 __totallysecureapp__ 폴더를 엽니다.

    ![VS Code Explorer](./assets/images/vscode_explorer.png "VS Code Explorer")

## SAST 결과 검토하기

1. VS Code Checkmarx 플러그인 내에서 최신 스캔 결과를 확장합니다.

    ![VS Code Plugin](./assets/images/vscode_cx_plugin.png "VS Code Plugin")

2. __M__, __L__, 및 __I__ 아이콘을 클릭하여 __HIGH__ 결과 이외의 모든 것을 필터링하고 스캔 결과 __Reflected XSS All Clients (/OpenRedirectController.java:32)__ 를 선택합니다.

    ![SAST Filter](./assets/images/sast_filter.png "SAST Filter")

3. __sast > HIGH__ 메뉴를 확장하여 SAST 스캔 결과를 검토합니다.
4. __SQL Injection (/SQLInjectionController.java:30)__ 결과 선택합니다.

    ![SAST High](./assets/images/sast_high.png "SAST High")

5. 공격 벡터와 함께 식별된 취약성에 대한 자세한 설명을 제공하는 VS Code가 오른쪽에 있는 창이 열립니다.

    ![SQL Injection](./assets/images/sqli.png "SQL Injection")

6. Checkmarx는 소스 코드를 스캔하고 데이터 흐름 그래프를 작성하기 때문에 취약점의 소스와 싱크를 식별하고 취약점을 해결하기 위한 수정 사항을 구현하는 것이 가장 좋은 위치를 식별할 수 있습니다. 첫번째 공격 벡터 링크 __1. "name" ...ggy4sb/vulnerabilities/SQLInjectionController.java [30:88]__ 를 클릭하면 VS Code가 특정 파일을 열고 Checkmarx가 취약점의 소스로 식별한 매개변수를 강조 표시합니다.

    ![SQL Injection Best Fix Location](./assets/images/sqli_bfl.png "SQL Injection Best Fix Location")

7. 코드를 검토하면 결과 내에서 이름을 직접 입력하고 다듬고 공백인지 확인하는 것을 볼 수 있습니다. 그렇지 않으면 나중에 SQL 쿼리의 일부로 실행되도록 입력을 전달합니다. SQL 인젝션 공격에서 위험에 대한 자세한 설명을 보려면 맨 오른쪽 창에서 __Learn More__ 를 클릭하십시오.:

    ![SQL Injection Learn More](./assets/images/sqli_learnmore.png "SQL Injection Learn More")

8. 코드 샘플을 클릭하여 코드 내에서 새니타이저를 구현하여 SQL 주입 취약성을 완화하는 방법의 예를 확인하십시오.

    ![SQL Injection Code Sample](./assets/images/sqli_code_sample.png "SQL Injection Code Sample")


## 결과 분류하기
Checkmarx SAST는 데이터 흐름에 대한 가시성이 있기 때문에 새니타이저가 있는지 확인하여 소스 코드의 취약점을 식별할 수 있지만 제어 흐름 기반 유효성 검사를 감지하는 기능은 상대적으로 제한됩니다. 아마도 우리는 코드가 취약성에 노출되지 않도록 보장할 수 있는 유효성 검사를 응용 프로그램 내에 가지고 있을 것입니다. 우리는 IDE 내에서 결과를 "악용할 수 없음"으로 표시하여 향후 스캔 시 반환되지 않도록 결과를 분류할 수 있습니다.

1. __Reflected XSS All Clients__ 결과 중 하나를 선택합니다.

2. 오른쪽 창에서 두 번째 드롭다운을 __To Verify__ 에서 __Not Exploitable__ 로 변경하고 다음과 같은 코멘트를 입력합니다.

        We have a validator in our code to ensure that this variable will only include known inputs.

    ![Triage Result](./assets/images/triage.png "Triage Results")

3. __Update__ 를 클릭합니다.

4. 결과는 이제 SAST High 결과에서 사라집니다. 결과를 다시 검토하려면 필터 아이콘을 선택하고 __"Not Exploitable"__ 이외의 모든 옵션을 선택 취소할 수 있습니다.
    
    ![Not Exploitable](./assets/images/not_exploitable.png "Not Exploitable")



## 주요 테이크 아웃
- Checkmarx SAST는 바이너리가 아닌 소스 코드를 스캔하여 개발 팀이 코드를 빌드하면서 반복적으로 스캔할 수 있도록 합니다.
- Checkmarx One은 모든 주요 SCM과 통합되며 푸시 및 풀 요청 이벤트에 대한 스캔을 자동으로 트리거할 수 있습니다.
- SAST 결과는 IDE 내에서 직접 볼 수 있으므로 결과를 검토하기 위해 다른 도구나 사이트로 이동할 필요가 없습니다.
- 코드 내에서 컨트롤을 구현하기 위해 IDE 내에서 자세한 분석 및 권장 사항과 함께 "Best Fix Location"을 볼 수 있습니다.
- 우리는 IDE 내에서 결과를 "not exploitable"으로 표시하여 향후 스캔 시 반환되지 않도록 결과를 분류할 수 있습니다.

