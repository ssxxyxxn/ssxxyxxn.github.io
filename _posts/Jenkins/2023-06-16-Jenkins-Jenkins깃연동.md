---
title: "[Jenkins] Jenkins - Git 연동"
excerpt: "Jenkins Git 연동 방법"

categories:
  - Jenkins
tags:
  - [Jenkins]

permalink: /jenkins-git/

toc: true
toc_sticky: true

date: 2023-06-16
last_modified_at: 2023-06-16
---

# 1. Jenkins 계정생성
![계정생성](/assets/images/posts_img/Jenkins/jenkins-git/1.png)

# 2. 새로운 Item 생성
![계정생성](/assets/images/posts_img/Jenkins/jenkins-git/2.1.png)   
새로운 Item 클릭         
![계정생성](/assets/images/posts_img/Jenkins/jenkins-git/2.2.png)
1. Enter an item name : 화면상에 표시할 아이템명 입력   
2. Maven project 선택 후 OK 버튼 클릭

- - -

# item name 네이밍

기존에 있는 name 뒤에 _GIT을 붙인다.

ex) GX_ANYBIZ_PRD → GX_ANYBIZ_PRD_GIT

# Credentials 설정(깃랩, 젠킨스)

![Untitled](/assets/images/posts_img/jenkins/jenkins-git/Untitled.png)

![Untitled](/assets/images/posts_img/jenkins/jenkins-git/Untitled (1).png)

# 젠킨스 설정

- 새로운 아이템 등록(프로젝트명 입력 후 Freestyle Project 선택)
- [젠킨스 플러그인 설치 오류](https://www.notion.so/8227b0060ee041dcb0844c802c2d7d9a?pvs=21)

![Untitled](/assets/images/posts_img/jenkins/jenkins-git/Untitled (2).png)

![Untitled](/assets/images/posts_img/jenkins/jenkins-git/Untitled (3).png)

![Untitled](/assets/images/posts_img/jenkins/jenkins-git/Untitled (4).png)

![Untitled](/assets/images/posts_img/jenkins/jenkins-git/Untitled (5).png)


`참고`

Webhook 연동까지 생각했었기때문에 젠킨스에서 플러그인 다운받았음

→ Webhook  연동을 하지 않을거라면 플러그인 다운받지 않아도 됨