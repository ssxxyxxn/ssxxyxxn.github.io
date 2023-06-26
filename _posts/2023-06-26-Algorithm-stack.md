---
title: "[Algorithm] Stack 클래스 사용법"
excerpt: "프로그래머스 lv2 12973. 짝지어 제거하기"

categories:
  - Algorithm
tags:
  - [Algorithm]

permalink: /algorithm/stack/

toc: false
toc_sticky: true

date: 2023-06-26
last_modified_at: 2023-06-26
---

![stack](/assets/images/posts_img/Algorithm/stack/1.jpg)

# Stack 특징
1. LIFO(Last In First Out)선입후출(먼저 들어간 자료가 나중에 나옴) 구조
2. 재귀적 함수를 호출할 때 사용
3. 그래프의 깊이 우선 탐색(DFS)에서 사용
4. 스택은 모든 함수에 괄호!!붙이기!!

[깊이 우선 탐색(DFS)](https://gmlwjd9405.github.io/2018/08/14/algorithm-dfs.html, "dfs link")


# 초기 선언
```java
import java.util.Stack;
Stack<String> stack = new Stack<String>();
```
항상 java.util.*; 로 전체선언해두면 외우지 않아도 import 자동으로 된다.

# 추가
```java
stack.push("Hello");
```
순차적으로 추가됩니다.
![stack](/assets/images/posts_img/Algorithm/stack/2.png)

# 삭제
```java
stack.pop();
```
pop 을 사용하면 가장 마지막의 원소 값이 제거 됩니다.
![stack](/assets/images/posts_img/Algorithm/stack/3.png)

# 출력
```java
stack.peek();
```
스택의 가장 마지막에 들어간 값을 출력하게 됩니다.
![stack](/assets/images/posts_img/Algorithm/stack/4.png)

# 존재여부
```java
stack.empty();
```
stack이 비어있으면 true, 비어있지 않다면 false

# 크기
```java
stack.size();
```
스택은 모든 함수에 괄호!!붙이기!!

# 포함여부
```java
stack.contains("Hello");
```
Hello가 있으면 true, 없으면 false