---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments
title: '[ALGO-11] Lect11. 최소신장트리 문제에 대한 프림, 크루스칼 두 알고리즘'
excerpt: "[ALGO] 성균관대학교 허재필 교수님의 K-mooc 알고리즘 강의를 듣고 공부한 자료입니다."
date: 2021-09-06
last_modified_at: 2021-09-06
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

이번 시간에는 <hl>최소신장트리 문제</hl>에 대한 <hl>프림</hl>, <hl>크루스칼</hl> 두 알고리즘에 대해 배운다.

<br>

# 최소신장트리 문제와 프림 알고리즘

<p><hl>그래프에서 발생하는 여러가지 문제</hl>들을 해결하는 방법에 대해 알아본다.</p>

## Minimum Spanning Trees

**■ 정의**

<img src="/files/2021-09-06-cs-algorithm-algo11/image-20210911225434559.png" alt="image-20210911225434559" style="width:300px;" />

<b>Minimum Spanning Tree</b>란 Graph의 <hl>모든 nodes가 최소한의 weights로 연결되는 Tree 자료구조</hl>이다. 

<br>

**■ 쓰임**

각 도시를 연결짓는 도로를 건설하려고 할 때, <hl>건설 비용을 최소화하면서 모든 도시를 연결할 수 있는 길을 찾는 문제</hl>에서 Minimum Spanning Tree 자료구조를 사용할 수 있다.

<br>

### Spanning Tree

<img src="/files/2021-09-06-cs-algorithm-algo11/image-20210911225919799.png" alt="image-20210911225919799" style="width:250px;" />

<b>Spanning Tree</b>란 Graph의 <hl>모든 vertex가 연결되도록 구성된 Tree</hl>이다.

* vertex의 개수가 $V$개일 때, Tree는 언제나 $V-1$개의 edges를 가지므로, <hl>Graph로부터 $V-1$개의 edges를 고르는 문제</hl>라 생각할 수도 있다.

* Spanning Tree가 <hl>Weighted Tree인 경우</hl>, Spanning Tree의 weight는 <hl>모든 edges의 weight 합</hl>으로 정의된다.

  $$
  \text{Spanning Tree의 weight} = \sum_{i=1}^{V}{w_i}
  $$

  ※ Spanning Tree의 weigth를 최소화하는 문제를 <hl>Minimum Spanning Tree</hl>라고 한다. ※

* Spanning Tree가 <hl>Unweighted Tree</hl>인 경우, 모든 edges의 $\text{weigth}=1$이라 가정할 수 있다. 따라서 이 경우 <hl>Minimum Spanning Tree의 weight는 언제나 $V-1$</hl>이다.

<br>

**■ 특징**

* <p><hl>Unique하지 않다.</hl></p>
* $V-1$개의 edges를 선택했을 때, <hl>어떤 vertex를 root로 보느냐에 따라 다른 Tree가 된다.</hl>

<br>

### Algorithms

<p><hl>Minimum Spanning Tree문제</hl>를 해결하는 대표적인 알고리즘은 다음과 같다.</p>

* <hl>Prim's Algorithm</hl>
* <hl>Kruskal's Algorithm</hl>

<p><hl>두 알고리즘 모두 Greedy Algorithm</hl>임에도 불구하고 최적의 값을 도출할 수 있음이 증명되었다.</p>

<div class="callout">
    <b>Greedy Algorithm</b>이란?<br>
    <hl>매 순간 최선의 선택</hl>을 취하는 알고리즘
</div>


<br>

## Prim's Algorithm

### 개념

<p><hl>$N$개의 vertex를 갖는 graph로부터 minimum spanning tree를 결정하는 방법</hl>은 다음과 같다. $k$개의 vertex로 이루어진 minimum spanning tree가 주어졌다면, <hl>기존의 minimum spanning tree에 $k+1$번째 vertex를 추가해나가는 방식</hl>으로 $N$개의 vertex를 갖는 minimum spanning tree를 결정할 수 있다.</p>

이때 최소한의 weight를 갖도록 $k+1$번째 vertex를 선택하는 방법은, <hl>기존의 tree에 직접적으로 연결된 edges 중 최소한의 weight를 갖는 edge를 선택</hl>하는 것이다. 

<br>

**■ 예시**

<img src="/files/2021-09-06-cs-algorithm-algo11/image-20210911231030263.png" alt="image-20210911231030263" style="width:400px;" />

<p><hl>$e_k$</hl>를 minimum spanning tree와 $v_{k+1}$을 연결하는 <hl>최소의 weight를 가진 edge</hl>라고 할 때, 위 예시에서 $v_{k+1}$을 연결하기 위한 edge로 $e_k$를 선택하는 것이 가장 합리적인지를 생각해보자. </p>

$e_k$를 선택하지 않는다면 다른 어떤 경로($\tilde{e}$)를 통해 $v_{k+1}$으로 연결되어야 한다. 이때 $e_k$가 $v_k$를 연결하는 최소의 weight를 가졌으므로 다른 경로 $\tilde{e}$는 반드시 $e_k$보다 weight값이 클 수밖에 없다. 따라서 <hl>$e_k$를 선택하는 것이 가장 합리적인 결정</hl>이다.

<br>

### 특징

* <hl>어떤 vertex에서 시작하든지 상관 없다.</hl>

<br>

### Implementation

**■ 배경 지식**

* `distance`: <hl>현재 minimum spanning tree로부터 어떤 특정 vertex까지의 거리</hl> <gray>※초기값은 ∞. 단, root vertex의 distance=0 ※</gray>
* `visit`: 해당 vertex가 <hl>이미 minimum spanning tree에 포함되었는지</hl>를 체크하는 flag <gray>※ 초기값=0 ※</gray>
* `parent`: 어떤 vertex가 minimu spanning tree에 포함될 때, <hl>어떤 node와 직접적으로 연결되었는가</hl>를 체크하는 flag <gray>※ 초기값=NULL ※</gray>

<br>

**■ 알고리즘**

1. <p><hl>minimum distance를 갖는 unvisited vertex를 선택</hl>한다.</p>
2. 해당 vertex를 <hl>visited상태로</hl> 바꿔준다.
3. 각 인접 vertex에 대한 distance를 고려하여 <hl>기존의 distance값을 업데이트</hl>해준다.
4. ①모든 vertex가 visited상태이거나 ②모든 unvisited vertex의 distance값이 ∞가 될 때 까지 <hl>1~3의 과정을 반복</hl>한다. <gray>※ ②의 경우 unconnected graph ※</gray>

<br>

**■ 예제**

<img src="/files/2021-09-06-cs-algorithm-algo11/image-20210911231907393.png" alt="image-20210911231907393" style="width:400px;" />


<br>

### 복잡도

<img src="/files/2021-09-06-cs-algorithm-algo11/image-20210904010142704.png" alt="image-20210904010142704" style="width:700px;" />

최종적으로 Prim's Algorithm의 복잡도는 $Θ(V^2)$가 된다.

<br>

### 최적화

minimum distance를 구하기에 가장 적합한 자료구조는 min-heap 자료구조이다.

* min-heap의 nodes의 개수는 $V$개가 된다. ⇒ $Θ(V)$의 memory and run time
* 최소값을 구하는 데(Pop)에 걸리는 비용은 $\lg(V)$이다.
* 따라서 최소값을 찾는 데에 드는 총 $(V-1)\lg(V)$가 소요된다. (∵ V-1번 위 과정을 반복) ⇒ $O(V\lg(V))$
* 최소값을 구한 뒤, neighbors에 대해 distance를 update하는 데에 $E\lg(V)$가 소요된다. (∵ edge만큼 연산되며, 각 update는 $\lg(V)$가 소요됨) ⇒ $O(E\lg(V))$

따라서 total run time은 다음과 같다.
$$
O(V\lg(V) + E\lg(V)) = O(E\lg(V))
$$
※ 이떄 edges의 개수 $E$는 최대 $V^2$의 값을 가질 수 있다. 따라서 edges의 개수가 많은 경우 오히려 min-heap(priority queue)를 사용하지 않는 것이 더 효율적이다. ※

<br>

<br>

# Kruskal's Algorithm

크루스칼 알고리즘은 프림 알고리즘 보다 간단한 알고리즘이다.

크루스칼 알고리즘은 weight에 따라 edges를 정렬하고, 최소 weight부터 차례로 minimum spanning graph에 추가시켜 나간다. 이때 tree에 cycle이 생기지 않도록(∵ cycle이 생기면 tree가 아니라 graph가 되니까) 추가하는 것이 핵심이다.

<br>

**■ 예시**

<img src="/files/2021-09-06-cs-algorithm-algo11/image-20210911232526642.png" alt="image-20210911232526642" style="width:400px;" />

1. graph 내에 존재하는 모든 edges를 weight 순서대로 정렬한다.
2. 위에서부터 하나씩 순회하며 minimum spanning tree에 삽입 여부를 결정한다.
   1. 해당 edge를 포함시킬 때 cycle이 생성되면 무시한다. (skip)
   2. 해당 edge를 포함시킬 때 cycle이 생성되지 않으면 추가한다. (addition)
3. 2번 과정을 $V-1$개의 edges를 고를 때 까지 반복한다.

<br>

## 복잡도

1. edges를 정렬하기 위한 merge sort(or heap sort) : $O(E\lg(E))$
2. cycle 체크를 위한 DFS(or BFS) : $O(E)=O(V)$ (∵$ E<V$이므로)

따라서 최종적인 run-time은 다음과 같다.
$$
O(E\lg(E) + E·V)\\
= O(E·V)\ \ (∵ E=O(V^2)\text{이므로 }\lg(E)=\lg(V))
$$
<br>

## 특징

* 프림알고리즘($O(E\lg(V))$ 혹은 $O(V^2)$)에 비해 비효율적($O(E·V)$)이다.
* 알고리즘이 단순하다.

* cycle check를 위해 DFS(or BFS) 대신 disjoint set을 사용하면 complexity를 낮출 수 있다.

<br>

<br>

# References

1. [집콕]인공지능을 위한 알고리즘과 자료구조: 이론, 코딩, 그리고 컴퓨팅 사고-비선형 자료구조, K-MOOC: [http://www.kmooc.kr/courses/course-v1:SKKUk+SKKU_46+2021_T1/courseware/1158909c057347478d7386a2b7e97214/e9d736660cad4f76bcb9a05974fe0bf2/1?activate_block_id=block-v1%3ASKKUk%2BSKKU_46%2B2021_T1%2Btype%40vertical%2Bblock%4052b62ff6da2147c9a8a65a6056681d91](http://www.kmooc.kr/courses/course-v1:SKKUk+SKKU_46+2021_T1/courseware/1158909c057347478d7386a2b7e97214/e9d736660cad4f76bcb9a05974fe0bf2/1?activate_block_id=block-v1%3ASKKUk%2BSKKU_46%2B2021_T1%2Btype%40vertical%2Bblock%4052b62ff6da2147c9a8a65a6056681d91)

<br>

<br>  





