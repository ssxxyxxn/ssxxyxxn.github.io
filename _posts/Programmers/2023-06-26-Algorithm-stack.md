---
title: "[JAVA] Stack 클래스 사용법"
excerpt: "프로그래머스 lv2 12973. 짝지어 제거하기"

categories:
  - Programmers
tags:
  - [Programmers]

permalink: /how_to_use_stack/

toc: true
toc_sticky: true

date: 2023-06-26
last_modified_at: 2023-06-26
---

![stack](/assets/images/posts_img/Programmers/how_to_use_stack/1.jpg)

# Stack 특징
1. LIFO(Last In First Out)선입후출(먼저 들어간 자료가 나중에 나옴) 구조
2. 재귀적 함수를 호출할 때 사용
3. 그래프의 [깊이 우선 탐색(DFS)]("https://gmlwjd9405.github.io/2018/08/14/programmers/how_to_use_stack-dfs.html", "dfs link")에서 사용
4. 스택은 모든 함수에 괄호!!붙이기!!

***

# 초기 선언
항상 java.util.*; 로 전체선언해두면 외우지 않아도 import 자동으로 된다.
```java
import java.util.Stack;
Stack<String> stack = new Stack<String>();
```

# 추가
순차적으로 추가됩니다.
![stack](/assets/images/posts_img/Programmers/how_to_use_stack/2.png){: width="350px"}
```java
stack.push("Hello");
```

# 삭제
pop 을 사용하면 가장 마지막의 원소 값이 제거 됩니다.
![stack](/assets/images/posts_img/Programmers/how_to_use_stack/3.png){: width="200px"}
```java
stack.pop();
```

# 출력
스택의 가장 마지막에 들어간 값을 출력하게 됩니다.
![stack](/assets/images/posts_img/Programmers/how_to_use_stack/4.png){: width="100px"}
```java
stack.peek();
```

# 존재 여부
stack이 비어있으면 true, 비어있지 않다면 false
```java
stack.empty();
```

# 크기
스택은 모든 함수에 괄호!!붙이기!!
```java
stack.size();
```

# 포함 여부
Hello가 있으면 true, 없으면 false
```java
stack.contains("Hello");
```