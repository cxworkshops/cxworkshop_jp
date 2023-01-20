---
layout: default
title: Lab 1 - 환경 설정
---

# Lab 1: 환경 설정

## 소개
이 실습에서는 IDE 및 Checkmarx 플러그인을 사용하여 환경을 구성하는 데 중점을 둘 것입니다. Checkmarx는 여러 IDE를 지원하지만 이러한 랩에서는 일반적으로 무료로 사용되는 Microsoft Visual Studio Code를 활용할 것입니다. Checkmarx는 다음 IDE와 플러그인을 통합했습니다.:

* Eclipse
* IntelliJ
* Visual Studio
* VS Code

자세히 알아보려면 [인기 IDE와의 Checkmarx 통합을 확인하세요.](https://checkmarx.com/why-checkmarx/integrations/checkmarx-integrations-with-ides/)

{: .warning }
이 실습에서는 [EasyBuggy](https://github.com/k-tamura/easybuggy) 를 기반으로 하는 알려진 취약한 Java 프로젝트를 사용하여 취약성 감지 및 수정 기능을 시연합니다. 이 응용 프로그램이 실행되면 이 Java 응용 프로그램은 메모리 누수, 교착 상태, JVM 충돌 등으로 인해 시스템 충돌이 발생할 수 있습니다. 이 실습에서는 소스 코드를 스캔하는 Checkmarx 솔루션만 사용하므로 그럴 이유가 없습니다. 또는 이 프로젝트를 실행해야 하며 __권장하지 않습니다__ . 프로젝트를 실행하고 싶다면 자신의 위험을 감수해야 합니다. 샌드박스 환경(예: VM 내)에서 수행하는 것이 좋습니다.

## VS Code 설치
아직 설치하지 않은 경우 첫 번째 단계는 VS Code를 설치하는 것입니다.

{: .note }
VS Code가 이미 설치되어 있으면 이 섹션을 건너뛸 수 있습니다. 실습이 끝나면 언제든지 Checkmarx 플러그인을 제거/비활성화할 수 있습니다.

1. [https://code.visualstudio.com/download](https://code.visualstudio.com/download) 로 이동 하여 운영 체제용 Visual Studio Code 설치 관리자를 다운로드합니다.
2. 컴퓨터에 VS Code 설치합니다.

![VS Code 설치](./assets/images/vscode_ubuntu_install.png "VS Code Install")


## Checkmarx 플러그인 설치
VS Code가 설치되면 Checkmarx 플러그인을 설치해야 합니다. Visual Studio Code 확장은 [Visual Studio Code marketplace](https://marketplace.visualstudio.com/items?itemName=checkmarx.ast-results)에서 사용할 수 있습니다.  Visual Studio Code 콘솔에서 직접 설치를 시작할 수 있습니다.

1. VS Code 열기
2. VS Code 내에서 Extension 아이콘을 클릭합니다.
3. 검색 프롬프트에 "Checkmarx"를 입력한 다음 확장에 대해 __Install__ 를 클릭합니다.
    ![Install Checkmarx Plugin](./assets/images/vscode_extensions.png "Checkmarx VX Code Plugin")

    {: .note }
    Checkmarx SAST xx가 아닌 "Checkmarx"라는 플러그인을 선택했는지 확인하십시오.

4. Checkmarx 확장이 설치되고 Checkmarx 아이콘이 왼쪽 탐색 패널에 나타납니다.

    ![Checkmarx Plugin Installed](./assets/images/cx_plugin_installed.png "Checkmarx Plugin Installed")


## Checkmarx 플러그인 설정하기
1. VS Code 콘솔에서 Checkmarx 확장 아이콘을 클릭한 다음 설정 열기 버튼을 클릭합니다. Checkmarx 설정 양식이 열립니다.
    ![Configure Checkmarx Plugin](./assets/images/cx_plugin_config.png "Configure Checkmarx Plugin")

2. Checkmarx AST 플러그인 섹션에서 다음 세부 정보를 입력합니다.:


    |         Item                          |          Value                |
    |:----------------------                |:-----------------------       |
    | Checkmarx KICS: Additional Parameters | \<leave blank\>                 |
    | Checkmarx AST: Additional Parameters  | \<leave blank\>                 |
    | Checkmarx AST: Api Key       | \<provided by proctor\>                |

3. 입력이 끝나면, Checkmarx 플러그인이 Checkmarx One 테넌트에 대해 인증합니다.

4. 플러그인 설정 화면 닫기

## 프로젝트 연결

1. 왼쪽 창의 __Project:__ 필드 위에 마우스를 놓고 연필 아이콘을 클릭한 다음 중간 검색 표시줄에 나타나는 프로젝트 이름 __cxworkshops/totallysecureapp__ 를 선택합니다.

    ![Select Project](./assets/images/cx_select_project.png "Select the Project")

    {: .note }
    > Checkmarx 플러그인 v2.0.11 릴리스 이후 플러그인을 Checkmarx One 테넌트에 연결하려면 AST/One API 키만 필요합니다. 프로젝트에 연결하려고 할 때 VSCode 내에서 404 오류가 표시되면 환경 변수가 API 키(cx_base_auth_uri, cx_base_uri, cx_tenant)에서 Uri/테넌트 이름을 재정의하기 때문일 수 있습니다. CLI를 통해 Checkmarx One에 연결한 적이 있는 경우 이러한 변수가 설정됩니다. 시스템에 있는 경우 checkmarxcli.yaml 파일을 삭제하여 이 문제를 해결할 수 있습니다.
    >
    > Mac OSX 및 Linux의 경우 이 파일은 ~/.checkmarx/checkmarxcli.yaml에서 찾을 수 있습니다.
    >
    > Windows의 경우 이 파일은 %UserProfile%\.checkmarx\checkmarxcli.yaml에서 찾을 수 있습니다.

2. 왼쪽 창의 __Branch:__ 필드 위에 마우스를 놓고 연필 아이콘을 클릭한 다음 중간 검색 표시줄에 나타나는 브랜치 __master__ 를 선택합니다.

    ![Select Branch](./assets/images/cx_select_branch.png "Select a branch")

3. 이제 Checkmarx 플러그인이 구성되었으며 왼쪽 창에 스캔 결과가 표시되어야 합니다.

    ![Configuration Finished](./assets/images/cx_scan_results.png "Configuration Finished")


## 로컬 컴퓨터에 프로젝트 복제

1. 터미널 또는 명령 프롬프트를 열고 쉽게 찾을 수 있는 디렉토리 또는 폴더로 이동합니다(예: Mac OSX 또는 Linux의 ~/ 또는 %UserProfile%/).

    ~[Terminal](./assets/images/terminal_dir.png "Terminal")

2. 스캔한 예제 프로젝트를 로컬 컴퓨터에 복제합니다..

        git clone https://github.com/cxworkshops/totallysecureapp.git

    {: .note }
    아직 설치되지 않은 경우 로컬 컴퓨터에 __git__ 을 설치해야 합니다. [이 가이드](https://github.com/git-guides/install-git) 를 사용 하여 운영 체제에 따른 설치 단계를 확인할 수 있습니다.

3. VS Code내에서 __File__ > __Open Folder__를 선택하고 __totallysecureapp__ 디렉토리를 선택합니다.

4.  개발자를 신뢰하는지 묻는 VS Code 메시지가 표시될 수 있습니다. 우리는 이 프로젝트 코드를 실행하지 않을 것이며 소스를 검토하기만 할 것이므로 안전하게 수락할 수 있습니다. 실습을 완료하면 프로젝트를 안전하게 삭제할 수 있습니다.

    ![VS Code Trust Project](./assets/images/vscode_trust.png "VS Code Trust Project")'


## (선택사항) 로컬 머신에 Docker 설치하기

{: .note }
Checkmarx IaC(Infrastructure-as-code)/KICS 자동 치료 기능(Auto Remediation)을 사용하려면 로컬 컴퓨터에 Docker를 설치해야 합니다.

### 윈도우/맥 사용자

Windows 또는 Mac 컴퓨터에 Docker를 설치하는 가장 쉬운 방법은 [Docker Desktop](https://www.docker.com/products/docker-desktop/)을 사용하는 것 입니다. 

{: .note }
로컬 시스템에 Docker Desktop을 설치하기 전에 Docker의 구독 및 라이센스 조건([Docker's Subscription and Licensing terms](https://www.docker.com/pricing/faq/))을 준수하는지 확인하십시오.

1. 이 예에서는 Ubuntu Linux 시스템에 도커를 설치합니다. 다음 명령을 사용하여 도커를 설치합니다.:

        sudo apt install docker.io

    ![ubuntu docker](./assets/images/docker_install.png "Docker Install")

2. 설치 요구 사항 및 종속 패키지를 검토하고 준비되면 'y'를 입력합니다.

3. Linux에서 실행 중인 경우 사용자가 도커를 루트가 아닌 사용자로 관리할 수 있는지 확인해야 합니다. 이를 수행하는 가장 쉬운 방법은 사용자를 docker 그룹에 추가하는 것입니다.:

        sudo gpasswd -a $USER docker

## 주요 테이크 아웃
- Checkmarx에는 모든 주요 IDE용 IDE 플러그인이 있습니다.
- Checkmarx One VS Code 플러그인은 Visual Studio Marketplace 내에서 사용할 수 있으며 Checkmarx SAST 플러그인과 다릅니다.
- Checkmarx One VS Code 플러그인은 하나의 필드(API 키)를 구성하여 Checkmarx One 인스턴스에 연결할 수 있습니다.
- Checkmarx 스캔 결과는 IDE 내에서 모두 검토할 수 있습니다.
- Docker는 VS Code 내에서 IaC/KICS 자동 치료(Auto Remediation)에 필요합니다.
