---
layout: default
title: Lab 4 - 악성 패키지 감지
---

# Lab 4: 악성 패키지 감지
이 랩에서는 소프트웨어 공급망(SCS) 조사 결과의 몇 가지 예와 이를 수정하는 방법을 살펴봅니다. 

{: .important-title }
> 전제 조건
>
> 우리는 https://github.com/cxworkshops/totallysecureapp 에 있는 __totallysecureapp__ 프로젝트를 사용할 것입니다. 아직 준비하지 않았다면 [Lab 1](../lab1_setup/)에 정의된 대로 프로젝트를 로컬 컴퓨터에 복제합니다.

{: .note }
이 랩에서 악성 패키지를 검토하는 동안에는 프로젝트 코드를 실행하지 않을 것입니다. 시스템에 악성 패키지가 실행되는 위험이 없습니다.

## 소개
좋은 개발자는 효율적인 개발자이며 효율적인 개발자가 되는 일부는 모든 프로젝트 또는 솔루션에 대한 바퀴를 다시 발명하지 않습니다. 결과적으로 우리 중 많은 사람들이 무료로 사용할 수 있는 오픈 소스 소프트웨어 및/또는 패키지의 이점을 활용하여 시간과 노력을 절약하고 기능과 기능을 더 빨리 달성할 수 있습니다. 이러한 오픈 소스 솔루션은 상당한 시간, 노력 및 골칫거리를 줄여주지만 다른 사람의 코드를 우리 프로젝트로 가져오면 우리가 모든 코드를 직접 개발했다면 직면하지 않았을 잠재적인 위험과 취약성에 노출됩니다.

## SCS 결과 검토하기
SCS 결과는 Checkmarx SCA 스캔 엔진에 포함됩니다. Checkmarx VS Code 플러그인내에서 [Lab 1](../lab1_setup/)에 __프로젝트 연결__ 에서 설명한 대로 프로젝트와 브랜치 및 스캔 결과그 잘 연결되었는지 확인합니다.

### 타이포스쿼팅(TypoSquatting)

1. VS Code의 왼쪽 메뉴에서 Checkmarx 플러그인으로 이동하여 메뉴를 sca > Malicious Package > HIGH results로 확장합니다.


    ![SCA High Results](./assets/images/scs_high_results.png "SCA High Results")

2. Npm-momnet-2.29.1 결과를 확장하고 __Cxa45b0853-bee2__ 결과를 선택합니다. 악성 패키지에 대한 설명과 함께 맨 오른쪽에 새 창이 어떻게 열리는지 확인하십시오.

    ![Cxa45b0853-bee2](./assets/images/Cxa45b0853-bee2.png "Cxa45b0853-bee2")

    {: .note }
    > 현재까지 공통 표준인 CVE가 있는 취약점과 달리 악성 패키지에 대한 표준화된 범용 ID는 없습니다. 예를들어 동일한 사건의 ID는 제품별로 아래와 같이 달리 나타내고 있습니다.:
    >
    > - cx-2021-b8833-be2146 (Checkmarx)
    > - SNYK-JS-RC-1911120 (Snyk)
    > - sonatype-2021-1696 (Sonatype)
    > - GHSA-g2q5-5433-rhrf (GitHub)

3. 악성 패키지의 설명에 따르면 이것이 악성 패키지이며 __타이포스쿼팅(Typo Squatting)__ 공격을 사용하고 있음을 알 수 있습니다.  패키지 제작자는 개발자가 "moment"를 실수로 잘못 입력하여 해당 악성 패키지는 대신 임포트하기를 바라고 있습니다. 이 악성 패키지는 내부 HTML 본문을 삭제하고 앱을 충돌시키는 악성 메소드를 포함하고 있습니다.  Vulnerable Package Paths창에 링크된 package.json를 클릭하고 패키지 이름을 __momnet__ 에서 __moment__ 로 변경하여 이 악성 패키지를 교정할 수 있습니다.:


    ![momnet](./assets/images/momnet.png "momnet")

### 레포재킹(RepoJacking)

1. Checkmarx 플러그인 내의 sca > Vulnerability > HIGH 결과 아래에서 __Npm-ua-parser-js-0.7.29__ 결과를 확장하고 __Cxbd45c2b9-4622__ 선택합니다.

    ![ua-parser](./assets/images/ua-parser.png "ua-parser")

2. 악성 패키지 설명을 검토하여 ua-parser-js 에 세가지 버전의 악성코드가 배포된 것을 알 수 있습니다. 그 세가지 버전(0.7.29, 0.8.0, and 1.0.0)은 __Repojacking__ 이라고도 하는 계정 탈취의 결과였습니다.  교정 조언에서 0.7.30 버전으로 수정하여 악성코드의 영향이 없는 버전으로 업그레이드할 수 있음을 확인할 수 있습니다. 

3. "Upgrade to Version"을 클락하면 Checkmarx 플러그인이 자동으로 package.json내의 패키지 버전을 0.7.30으로 업데이트 합니다.

    !["ua-parser-fix](./assets/images/ua-parser-fix.png "ua-parser-fix")


## 주요 테이크 아웃

- 공급망 보안(SCS) 결과는 Checkmarx SCA 결과에 포함됩니다.
- CVE가 있는 취약점과 달리 악성 패키지에는 표준화된 식별자가 없습니다.
- 공급망 공격은 놀라울 정도로 정교하지 않지만 쉽게 통합될 수 있습니다.
- Checkmarx VS Code 플러그인은 특정 패키지 버전의 자동 치료를 허용합니다.
