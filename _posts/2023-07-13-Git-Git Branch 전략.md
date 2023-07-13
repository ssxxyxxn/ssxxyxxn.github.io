---
title: "[Git] Git Branch 전략"
excerpt: "Git Branch 전략"

categories:
  - Git
tags:
  - [Git]

permalink: /git_branch

toc: true
toc_sticky: true

date: 2023-07-02
last_modified_at: 2023-07-02
---
# Git Branch 전략이란?

**여러 개발자가 하나의 저장소를 사용하는 환경에서 저장소를 효과적으로 활용하기 위한 work-flow다.**

브랜치의 생성, 삭제, 병합 등 git의 유연한 구조를 활용해서, 각 개발자들의 혼란을 최대한 줄이며 다양한 방식으로 소스를 관리하는 역할을 한다.

즉, **브랜치 생성에 규칙을 만들어서 협업을 유연하게 하는 방법론**을 말한다.

브랜치 전략은 Git flow, Github flow, GitLab flow 3개가 가장 널리 사용된다.

# Git flow

![gitflow.png](/assets/images/posts_img/gitbranch/gitflow.png)

Git flow는 Vincent Driessen이 2010년에 제안한 Branch Model을 기반으로 만들어졌으며 **현재는 많은 기업에서 표준으로 사용하는 브랜치 전략**이다.

## 크게 **5개의 브랜치**를 운영하며 관리를 한다

1. 메인 브랜치 : master, develop
2. 보조 브랜치 : feature, release, hotfix

## 메인 브랜치들의 특징

1. master와 develop 브랜치가 있다
2. 두 개의 브랜치는 항상 남아있는다
3. master 브랜치 : 제품으로 배포할 수 있는 브랜치
4. develop 브랜치 : 개발자들이 개발을 하는 브랜치

## 보조 브랜치의 특징

1. feature, release(QA), hotfix 브랜치가 있다.
2. 사용을 마치면 브랜치를 삭제한다.

### Feature 브랜치

1. 브랜치가 분기/머지 되는 곳 : develop
2. 이름 : master, develop, release-*, hotfix-*를 제외한 이름 가능, 보통 feature-*
3. feature 브랜치는 하나의 새로운 기능을 만들 때 생성한다.
4. develop에 merge 후에는 삭제한다.

### Release 브랜치

1. 브랜치가 분기 되는 곳 : develop
2. 브랜치가 머지 되는 곳 : develop & master
3. 이름 : release-*
4. 다음 버전 출시를 위해 QA를 진행하는 브랜치
5. 버그 수정 및 버전 번호, 빌드 날짜와 같은 메타 데이터를 준비하며 기능 개발은 금지된다.

### Hotfix 브랜치

1. 브랜치가 분기 되는 곳 : master
2. 브랜치가 머지 되는 곳 : develop & master
3. 이름 : hotfix-*
4. Production에 버그가 발생하면 빠른 수정을 위해 생성하는 브랜치
5. Production 코드를 수정하는 중에도 develop에서는 계속 개발을 할 수 있다는 장점이 있다.
6. master로 merge 후에는 tag 명령을 통해 이전 버전보다 높은 버전을 명시한다. 예) 1.2 -> 1.2.1

## Git flow 흐름

![gitflow 브랜치 전략 예시.png](/assets/images/posts_img/gitbranch/gitflow 브랜치 전략 예시.png)

### 신규 기능 개발

1. 개발자는 develop 브랜치로부터 본인이 신규 개발할 기능을 위한 feature 브랜치를 생성한다.
2. feature 브랜치에서 기능을 완성하면 develop 브랜치에 merge를 진행하게 된다.

### **운영 서버로 배포**

1. feature 브랜치들이 모두 develop 브랜치에 merge 되었다면 QA를 위해 release 브랜치를 생성한다.
2. release 브랜치를 통해 오류가 확인된다면 release 브랜치 내에서 수정을 진행한다.
3. QA와 테스트를 모두 통과했다면, 배포를 위해 release 브랜치를 master 브랜치 쪽으로 merge하며,
4. 만일 release 브랜치 내부에서 오류 수정이 진행되었을 경우 동기화를 위해 develop 브랜치 쪽에도 merge를 진행한다.

### **배포 후 관리**

1. 만일 배포된 운영 서버(master)에서 버그가 발생된다면, hotfix 브랜치를 생성하여 버그 픽스를 진행한다.
2. 그리고 종료된 버그 픽스를 master와 develop 양 쪽에 merge하여 동기화 시킨다.

# Github flow

![github flow.png](/assets/images/posts_img/gitbranch/github flow.png)

1. Git flow가 Github에서 사용하기에는 복잡하다고 나온 브랜치 전략이다.
2. master 브랜치를 중심으로 운영되며, 기능 개발(feature)/버그 수정(hotfix) 등의 작업용 브랜치를 구분하지 않는 단순한 구조이다.
3. 수시로 배포가 일어나는 프로젝트에 유용하다.

## GitHub flow 정책

1. 기능 개발, 버그 픽스 등 어떤 이유로든 master로부터 새로운 브랜치를 생성하는 것으로 시작된다.
2. 브랜치는 로컬에 commit하고, 정기적으로 원격 브랜치에 push한다.
3. 피드백이나 도움이 필요하거나, 코드 병합할 준비가 되었다면 pull request를 만든다.
(pull request는 코드 리뷰를 도와주는 시스템)
4. 다른 사람이 변경된 코드를 검토한 뒤 승인하면 master에 병합한다.
5. 병합된 master는 즉시 배포할 수 있으며, 배포 해야만 한다.

# Gitlab flow

## pre-production 브랜치가 없는 전략

![gitlab flow1.png](/assets/images/posts_img/gitbranch/gitlab flow1.png)

## pre-production 브랜치를 두어 staging 단계를 가지는 전략

![gitlab flow.png](/assets/images/posts_img/gitbranch/gitlab flow.png)

Github flow는 너무 간단해서 배포, 릴리즈 등의 조금 복잡한 이슈를 보완하기 위해 나온 전략입니다.

## feature 브랜치

모든 기능 구현은 feature 브랜치에서 시작합니다. feature 브랜치는 master 브랜치에서 분기되고 머지됩니다.

## master 브랜치

gitlab flow의 master 브랜치 역할은 git flow의 develop 브랜치와 동일합니다. master 브랜치는 feature 브랜치에서 병합된 기능에 대해 test를 진행합니다. 전체적인 테스트가 진행되어 기능에 대한 보장이 되었다면 production 브랜치로 머지합니다.

만약 staging 단계를 원한다면 pre-production 브랜치로 머지를 진행합니다.

## production 브랜치

gitlab flow의 production 브랜치 역할은 git flow의 master 브랜치와 동일합니다. 테스트가 끝난 기능에 대해 배포를 하기 위한 브랜치입니다.

## pre-production 브랜치

master → production 브랜치 사이에 pre-production 브랜치를 두어 변경 사항을 바로 production에 배포하지 않고 test server에 배포하여 통합 테스트를 진행하거나 시간을 두고 반영하는 브랜치입니다.