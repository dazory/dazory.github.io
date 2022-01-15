---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments
title: '[ALGO-13] Lect13. 다익스트라 알고리즘 및 복잡도'
excerpt: "[ALGO] 성균관대학교 허재필 교수님의 K-mooc 알고리즘 강의를 듣고 공부한 자료입니다."
date: 2021-09-18
last_modified_at: 2021-09-18
categories:
  - computerScience-algorithm
tags: 
   - [computer science, algorithm, ALGO]

use_math: false
comments: true
share : false
---

<div class="info-dialog">
    <div class="title">강의 정보</div>
    <div class="content">
        강의명: [집콕]인공지능을 위한 알고리즘과 자료구조: 이론, 코딩, 그리고 컴퓨팅 사고<br>
        교수: 성균관대학교 소프트웨어학과 허재필 교수님<br>
        사이트: <a href="http://www.kmooc.kr/courses/course-v1:SKKUk+SKKU_46+2021_T1/course/">http://www.kmooc.kr/courses/course-v1:SKKUk+SKKU_46+2021_T1/course/</a>
    </div>
</div>

<br>
<br>
이번 시간에는 <hl>다익스트라 알고리즘 및 복잡도</hl>에 대해 알아본다.

<br>

# 그래프 최단경로 알고리즘

<p><hl>그래프 내 최단경로 문제</hl>를 정의하고, 활용 분야 및 다익스트라 알고리즘의 핵심 아이디어를 이해한다.</p>

<br>

## Shortest Path

<p><hl>최단경로 문제</hl>를 해결하는 방법에는 대표적으로 <hl>Dijkstra's Algorithm</hl>이 있다. </p>

## Dijkstra's Algorithm

Prim's Algorithm과 유사하지만 <hl>모든 edges의 weights값이 양수여야만 한다는 제한조건</hl>을 갖는다.

<img src="/files/2021-09-18-cs-algorithm-algo13/image-20210912011036792.png" alt="image-20210912011036792" style="width:300px;" />

이처럼 Dijkstra's Algorithm은 <hl>최단경로를 하나씩 확정해 나가는 방법</hl>이다.

<br>

# 다익스트라 알고리즘과 복잡도

다익스트라 알고리즘의 구동 원리를 이해하고, 그래프 표현 자료구조 및 거리 저장 자료구조에 따른 복잡도 차이를 분석한다.

<br>

## Dijkstra's Algorithm

### 구성요소 

* Prim's Algorithm과 마찬가지로 초기에는 <hl>initial vertex에 대한 정보</hl>만을 가지고 있다.
* <p><hl>3개의 array(`distance`, `visit`, `parent`)</hl>를 갖는다.</p>

<br>

### 동작방식

1. 초기화
2. 아래의 동작을 모든 vertex를 방문할 때 까지 $V$번 반복한다.
   1. unvisited vertex 중 initial vertex로부터 <hl>가장 거리가 짧은 vertex를 방문(visit)</hl>한다.
   2. 방문한 vertex의 neighbors의 <hl>distance와 parent 정보를 업데이트</hl>한다.

※ shortest path의 거리가 무한대(∞)라면 해당 graph는 unconnected-graph이다. ※

<br>

### Example

**■ 예제1**

initial vertex=K일 때, K로부터 다른 모든 vertex까지의 최단경로를 구해본다.

<img src="/files/2021-09-18-cs-algorithm-algo13/image-20210912011223861.png" alt="image-20210912011223861" style="width:500px;" />

※ unvisited vertex의 distance = ∞라면, unconnected-graph이다. ※

<br>

**■ 예제2**: 최단거리뿐만 아니라 최단 경로도 고려하는 문제

<img src="/files/2021-09-18-cs-algorithm-algo13/image-20210912011507809.png" alt="image-20210912011507809" style="width:400px;" />

<p><hl>previous flag</hl>를 추가하여 <hl>역순으로 추적</hl>할 수 있다.</p>

<br>

### Analysis

1. Initialization: $Θ(V)$ (∵vertex의 개수 $V$만큼 array를 순회)
2. 아래의 동작을 $V-1$번 반복한다:
   1. unvisited vertex 중 initial vertex로부터 <hl>가장 거리가 짧은 vertex를 방문(visit)</hl>한다: $Θ(V)$ <br><gray>(∵ 현재까지 조회된 vertex들의 distance를 차례로 탐색)</gray>
   2. 방문한 vertex의 neighbors의 <hl>distance와 parent 정보를 업데이트</hl>한다:
      * adjacency matrix를 사용하는 경우: $Θ(V)$
      * adjacency list를 사용하는 경우: $Θ(E)$ (이때 $E<V^2$)

따라서 최종적으로 아래와 같은 <hl>복잡도</hl>를 갖는다.

$$
V+(V-1)·(V+V)\text{ or }V+(V-1)·(V)+E\\
∴ Θ(V^2)
$$

<br>

Dijkstra's algorithm을 적용할 때, 단순히 distance table을 앞에서부터 차례로 순회하는 것이 아니라 <hl>priority queue(min binary heap)</hl>과 같은 자료구조를 활용하면 보다 <hl>최적화</hl>할 수 있다.

1. Initialization: $Θ(V)$ (∵vertex의 개수 $V$만큼 array를 순회)

2. 아래의 동작을 $V$번 반복한다:

   1. unvisited vertex 중 initial vertex로부터 <hl>가장 거리가 짧은 vertex를 방문(visit)</hl>한다: $Θ(1)$ <br><gray>(∵ root로 바로 접근 가능)</gray>

   2. 방문한 vertex의 neighbors의 <hl>distance와 parent 정보를 업데이트</hl>한다: $Θ(\lg(V))$

      <gray>※ 이때, 업데이트 발생시, heap 내부 구조에서 또 다시 업데이트가 발생하여 다음의 추가 비용이 든다: $Θ(E\lg(V))$</gray>

따라서 최종적으로 아래와 같은 <hl>복잡도</hl>를 갖는다.

$$
V+V·(1+\lg(V))+E\lg(V)\\
∴ Θ(V\lg(V)+E\lg(V))\\
= \begin{cases}
	Θ(E\lg(V)) & \text{if $E<<V^2$}\\
	Θ(V^2\lg(V)) & \text{if $E≒V^2$}
\end{cases}
$$

즉, edges의 개수가 극단적으로 많지($Θ(V^2\lg(V))$) 않다면 <hl>priority queue</hl>를 사용하는 것($Θ(E\lg(V))$)이 일반적인 array를 사용하는 것($Θ(V^2))$)보다 더 <hl>효율적</hl>이다.

<br>

<br>

# References

1. [집콕]인공지능을 위한 알고리즘과 자료구조: 이론, 코딩, 그리고 컴퓨팅 사고-비선형 자료구조, K-MOOC: [http://www.kmooc.kr/courses/course-v1:SKKUk+SKKU_46+2021_T1/courseware/1158909c057347478d7386a2b7e97214/e9d736660cad4f76bcb9a05974fe0bf2/1?activate_block_id=block-v1%3ASKKUk%2BSKKU_46%2B2021_T1%2Btype%40vertical%2Bblock%4052b62ff6da2147c9a8a65a6056681d91](http://www.kmooc.kr/courses/course-v1:SKKUk+SKKU_46+2021_T1/courseware/1158909c057347478d7386a2b7e97214/e9d736660cad4f76bcb9a05974fe0bf2/1?activate_block_id=block-v1%3ASKKUk%2BSKKU_46%2B2021_T1%2Btype%40vertical%2Bblock%4052b62ff6da2147c9a8a65a6056681d91)

<br>

<br>  








