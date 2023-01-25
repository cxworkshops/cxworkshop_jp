---
layout: default
title: Lab 5 - 인프라 코드 분석
---

# Lab 5: 인프라 코드 분석

{: .important-title }
> 전제 조건
>
> 우리는 https://github.com/cxworkshops/totallysecureapp 에 있는 __totallysecureapp__ 프로젝트를 사용할 것입니다. 아직 준비되지 않았다면 [Lab 1](../lab1_setup/)에 설명된 것 처럼 프로젝트를 로컬 컴퓨터에 복제합니다.

## 소개

애플리케이션 소스코드 정적 분석 외에도 Checkmarx는 인프라 코드 분석도 제공하고 있습니다. Checkmarx One 플랫폼 내에 IaC 스캔 엔진이 포함되어 있지만 Checkmarx KICS라는 오픈 소스 솔루션으로도 제공됩니다. KICS는 인프라 코드에서 보안 취약점, 규정 준수 문제 및 잘못된 인프라 구성을 찾습니다. 인프라 코드의 형태로는 Terraform, Kubernetes, Docker, AWS CloudFormation, Ansible, Helm, Google Deployment Manager, AWS SAM, Microsoft ARM, Microsoft Azure Blueprints, OpenAPI 2.0 and 3.0, Pulumi, Crossplane, Knative 맟 Serverless Framework을 지원합니다.

Checkmarx VS Code 플러그인을 사용하면 KICS는 인프라 코드 파일이 열리거나 저장될 때마다 자동으로 스캔할 수 있으며 이를 위해 Checkmarx 계정이 필요하지 않습니다.

## 자동 스캔 기능 활성화
KICS 자동 스캔 기능은 Checkmarx One 플러그인이 설치되면 기본값으로 활성화됩니다. 환경에서 활성화되었지는 확인하려면 다음단계를 따르십시오.:

1. VS Code 내에서 View > Extensions 으로 이동하여 검색 표시줄에 "Checkmarx" 라고 입력합니다.

    !["Manage Extensions"](./assets/images/manage_extensions.png "Manage Extensions")

2. 오른쪽의 작은 기어 아이콘을 클릭하고 __Extension Settings__ 을 선택합니다. __Checkmarx KICS: Activate KICS Auto Scanning__ 메뉴의 선택박스가 활성화되었는지 확인합니다.:

    !["KICS Auto Scan Setting"](./assets/images/kics_autoscan_setting.png "KICS Auto Scan Setting")

{: .note }
KICS 자동 스캔이 작동하려면 docker가 설치되어 있어야 합니다. 자세한 내용은 [Lab 1](../lab1_setup/)을 참조하시기 바랍니다.

## KICS 자동 스캔 실행 시키기

__totallysecureapp__ 프로젝트에는 많은 인프라 코드 파일이 있습니다.  이 실습에서는 프로젝트 폴더의 루트에 있는 __terraform.tf__ 파일을 사용하겠습니다.

1. VS Code 내에서 출력 창이 표시되는지 확인합니다.(기본값 위치: 아래쪽 창).  "OUTPUT"이 표시되지 않으면 __View__ > __Output__ 메뉴로 이동하거나 __Ctrl__+__K__ 키 후 __Ctrl__+__H__ 키를 입력하여 창을 전환할 수 있습니다. OUTPUT 창의 오른쪽 드롭다운 메뉴에 __Checkmarx__ 가 선택되어 있는지 확인합니다.

    !["Checkmarx Plugin Output"](./assets/images/checkmarx_plugin_output.png "Checkmarx Plugin Output")

2. VS Code의 파일 탐색기를 사용하여 프로젝트 루트에 있는 __terraform.tf__ 파일을 더블 클릭합니다.

3. Checkmarx 플러그인은 Terraform 파일을 인식하고 자동으로 KICS 스캔을 시작합니다. 스캔이 어떻게 시작되었는지 알리는 메시지가 출력 창에 표시되어야 합니다.:

        [INFO - 5:14:26 PM] Opened a supported file by KICS. Starting KICS scan

4.  The KICS 스캔이 실행되고 완료되면 VS Code의 출력 창에 결과 요약이 표시됩니다.

    !["KICS Autoscan Results"](./assets/images/KICS_autoscan_result.png "KICS Autoscan Results")

5.  추가적으로 VS Code는 심각도 등급과 함께 발견된 KICS 결과가 있는 라인에 강조 표시를 적용합니다.


## KICS 스캔 결과 교정하기

1.  이전에 연 Terraform 파일 내에서 빨간색으로 강조 표시되어야 하는 첫 번째 줄에 마우스를 올리면 발견된 취약점에 대한 설명 팝업이 나타납니다.

    !["KICS High Result"](./assets/images/kics_result_1.png "KICS High Result")

2.  설명 창 하단에 __Quick Fix__ 라는 제목의 링크가 있습니다. 그것을 클릭하십시오.

    {: .note }
    하단의 Quick Fix 링크를 보려면 VS Code 팝업에서 약간 아래로 스크롤해야 할 수도 있습니다.

3. 2~4개의 옵션이 표시됩니다 :
    - Apply fix to Ram account Password Policy Not Required Minimum Length
    - Apply fix to Ram Account Password Policy Max Password Age Unrecommended
    - Line: Apply all available fixes
    - File: Apply all available fixes


4. __File: Apply all available fixes__ 을 적용하여 모든 KICS 권장 사항을 적용합니다.

    !["KICS Quick Fix"](./assets/images/quick_fix.png "KICS Quick Fix")

5. KICS는 파일 내의 권장 사항을 자동으로 적용하며 밑줄 강조 표시가 사라지는 것을 볼 수 있습니다.

    !["KICS Fixed"](./assets/images/fixed.png "KICS Fixed")

    {: .note }
    자동 수정 기능으로 시간을 절약할 수 있지만 일반적으로 권장 사항을 한 번에 하나씩 적용하여 그 영향을 테스트하고 애플리케이션 또는 컨테이너 기능이 손상되지 않도록 하는 것이 좋습니다.

# 주요 테이크 아웃
- Checkmarx One 플랫폼 내에 IaC(Infrastructure-as-code) 스캔 엔진이 포함되어 있으며 Checkmarx One 외부에서는 KICS로 제공됩니다.
- KICS는 사용자 환경 내에서 직접 스캔을 수행할 수 있는 Checkmarx One과 독립적으로 사용할 수 있습니다.
- Checkmarx One 플러그인은 인식된 IaC 파일이 VS Code 내에서 열리거나 저장될 때 KICS 스캔을 수행하도록 자동으로 구성될 수 있습니다.
- KICS는 VS Code 내에서 직접 결과를 자동으로 강조 표시하고 열거할 수 있습니다.
- 사용자는 한 줄씩 또는 전체 파일에 걸쳐 권장 수정 사항을 자동으로 적용할 수 있습니다.

