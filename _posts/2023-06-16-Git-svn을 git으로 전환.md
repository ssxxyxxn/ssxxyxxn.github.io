---
title: "[Git] SVN > Git 전환"
excerpt: "SVN을 Git으로 전환하는 방법"

categories:
  - Git
tags:
  - [Git]

permalink: /git/change-svn-to-git/

toc: true
toc_sticky: true

date: 2023-06-16
last_modified_at: 2023-06-16
---

SVN을 git으로 전환하는 방법 및 과정을 소개해드립니다.   
각자 개발하며 SVN 프로젝트가 있으실텐데,   
기존에 SVN에서 사용하던 user를 Git에서 사용할 계정으로 매핑도 하고,  
프로젝트 소스까지 한 방에 옮길 수 있습니다.

- - -

## 1. 준비물
- 마이그레이션 할 SVN 저장소의 URL
- 작업할 깃 프로젝트 소스 폴더 생성
- git bash 설치
- SVN 계정 ID/PW
- SVN 서버 접속
- Git repository 생성해두기
- Git repository 주소

- - - 

## 2. SVN Author과 Git Author 매핑
### SVN에 기록된 Author 이름 추출
SVN 서버에 접속 후
```shell
svn log --xml --quiet {SVN주소} | grep author | sort -u | perl -pe 's/.*>(.*?)<.*/$1 = /'
```
### users.txt 파일 생성
svn에서 나온 user명을 기준으로 작업할 폴더 내부에 users.txt 파일을 생성합니다.
```
user명 = 한글명 <git 아이디>
user명 = 한글명 <git 아이디>
```
- - -

## 3. SVN 저장소를 Git 저장소로 변환
작업할 깃 프로젝트 소스 폴더에 들어간 뒤 우클릭 후 git-bash를 엽니다.
```shell
git svn clone {SVN주소} --no-metadata -A ./users.txt ./{프로젝트 소스 저장할 폴더 명}
```
로그인 창이 뜨게 되면 SVN 계정 ID/PW 으로 로그인을 진행하면 됩니다.   
여기까지 진행하게 되면 SVN 프로젝트 소스가 폴더 안에 생성되게 됩니다.   

- - -

## 4. Git 에 push 하기
git-bash에서 이어서 작업합니다.   

- push할 폴더 이동 
```shell
cd {프로젝트 소스 저장할 폴더 명}
```
- Git 원격 저장소 추가
```shell
git remote add origin {Git repository 주소}
```
- 현재 프로젝트에 등록된 리모트 저장소를 확인 
```shell
git remote -v
```
- 만약 로컬 브랜치가 master로 되어있으면 main 브랜치 생성 
```shell
git checkout -b main
```
- push 전 pull 받기 
```shell
git pull origin main
```
- 수정한 README 파일 Staged Changes에 올리기  
```shell
git add -A
```
- staging 잘 됐는지 확인 
```shell
git status
```
- commit 하기
```shell
git commit -m "[변경사항] 1. README 수정”`
```
- Git에 push하기
```shell
git push origin main
```
