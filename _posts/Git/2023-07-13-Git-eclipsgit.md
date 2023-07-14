---
title: "[Git] 이클립스에서 Git사용하기"
excerpt: "반드시 설치해야 할 VSCode 확장 프로그램"

categories:
  - Git
tags:
  - [Git]

permalink: /how_to_use_git

toc: true
toc_sticky: true

date: 2023-07-13
last_modified_at: 2023-07-13
---
# 프로젝트 세팅

## 1. GitLab

![Untitled](/assets/images/posts_img/Git/how_to_use_git/Untitled.png)

- Clone 클릭 후 Https 주소 복사

## 2. 이클립스

- Window > Show View > Other : Git 검색 후 Git Repositories, Git Staging 탭 생성
    
    ![Untitled2](/assets/images/posts_img/Git/how_to_use_git/Untitled (2).png)
    
- Git Repositories 탭에서 우클릭 후 [Paste Repository Path or URI] 클릭
- 생성된 프로젝트 우클릭 후 [Import Projects…] 클릭 후 import 해오기
    
    ![Untitled](/assets/images/posts_img/git/how_to_use_git/Untitled (3).png)
    

# Local Branch 생성 후 개발

<aside>
💡 무조건 새로운 개발 건이 있을 때마다 local 환경에서 branch 생성 후 개발 진행한다.

</aside>

1. Git Repositories 탭에서 Local 클릭 > Switch To > New Branch… 클릭
    
    ![Untitled](/assets/images/posts_img/git/how_to_use_git/Untitled (4).png)
    
2. 원격 저장소에 있는 master를 기준으로 브랜치를 생성한다.
    - Select… 클릭
    
    ![Untitled](/assets/images/posts_img/git/how_to_use_git/Untitled (5).png)
    
    - Remote Tracking > origin/master 클릭 후 확인
    
    ![Untitled](/assets/images/posts_img/git/how_to_use_git/Untitled (6).png)
    
    - Source 가 origin/master로 변경 되었는지 확인한다.
    
    ![Untitled](/assets/images/posts_img/git/how_to_use_git/Untitled (7).png)
    
    - Branch name 은 <feature/개발할 내용> or <hotfix/오류 수정할 내용> 으로 적는다.
    - Configure upstream for push and pull 체크 해제 후 Finish 클릭
        
        GIT과 연동해서 해당 branch로 push&pull을 가능하게 할 건지 묻는 것
        
        GIT에 commit 시키고 pull도 받을 거면 체크해주고 그냥 로컬에서 사용할거면 체크하지 않아도 된다.
        

# 프로젝트 소스에서 Commit/Push 하기

## Commit

1. Git Staging 탭 클릭
    
    ![Untitled](/assets/images/posts_img/git/how_to_use_git/Untitled (8).png)
    
2. Unstages Changes 에서 Staged Changes 로 반영할 소스만 + 버튼으로 옮긴다.
3. Commit Message 는 다음과 같은 내용들로 분류를 달고 구체적으로 반영하는 내용을 적는다.
    
    [기능추가]
    [변경사항]
    [오류해결]
    [기타]
    
4. 다 작성했으면 Commit 버튼을 누른다.

## Push

    ![Untitled](/assets/images/posts_img/git/how_to_use_git/Untitled.jpeg)

    ![Untitled](/assets/images/posts_img/git/how_to_use_git/Untitled (9).png)

1. push 할 브랜치 선택([Local Branch 생성 후 개발] 에서 생성한 브랜치)
2. Add Spec 
3. HEAD 소스는 삭제
4. Finish
5. 프로젝트 명 우클릭
6. Team > Remote > Push…
7. Next >

# Gitlab에서 Merge 하기

## Merge Requests(develop)

    ![Untitled](/assets/images/posts_img/git/how_to_use_git/Untitled (10).png)

- develop에 먼저 반영 및 테스트 진행

## Merge Requests(master)

    ![Untitled](/assets/images/posts_img/git/how_to_use_git/Untitled (11).png)

1. Merge Requests 작성하기
2. merge할 브랜치 선택([Local Branch 생성 후 개발] 에서 생성한 브랜치)
    1. 브랜치 변경은 Change branches 클릭
3. Target 브랜치는 우선 무조건 develop에 먼저 반영 및 테스트 후에 master에 반영한다.
    1. 개발/알파에 반영: develop 선택
    2. 운영 반영: master 선택
4. (master merge 시) 코드리뷰 진행하기 위해서 설정한다.
    
    a. Assignee : 본인
    
    b. Reviewer : 팀장
    
5. branch 삭제되지 않기 위해 체크 해제
6. 코드리뷰 후 Merge를 한다.
    
    ![Untitled](/assets/images/posts_img/git/how_to_use_git/Untitled (12).png)