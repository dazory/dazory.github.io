---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments
title: '[ALGO-14] Lect14. 의존관계가 정의된 과제 그래프 모델링과 위상정렬 및 임계경로'
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
이번 시간에는 <hl>의존관계가 정의된 과제들을 그래프로 모델링</hl>해보고 <hl>위상정렬 및 임계경로</hl>를 도출해본다.

<br>

# DAG 구조와 활용 분야

<p><hl>의존 관계가 정의 된 과제</hl>들의 <hl>방향-비순환(Directed Acyclic) 그래프</hl>로의 모델링, 위상정렬의 정의, 활용 분야 및 알고리즘에 대해 학습한다.</p>

<br>

## Directed Acyclic Graph, DAG

<p><b>DAG</b>는 <hl>사이클을 포함하지 않는 유향(방향이 있는) 그래프</hl>이다.</p>

**■ 특징**

* vertex $v_j$→ $v_k$인 경로가 존재한다면, $v_k$→$v_j$인 경로는 존재하지 않는다.

  (∵ 존재한다면, acyclic의 규칙이 깨지므로)



## Topological Sort

**■ 배경**

주어진 작업 셋 간에 dependencies(의존성)이 있는 경우, <hl>의존성을 무너뜨리지 않고 모든 작업을 마치는 방법에 대한 해결책</hl>이다. 이때 이러한 작업들은 <hl>Directed Acyclic Graph(DAG) 구조로 표현</hl>할 수 있다.

<br>

**■ 특징**

Topological Sorting의 결과는 <hl>unique하지 않다.</hl>

<br>

**■ 응용**

* 외출하기위해 옷을 입는 순서 (속옷→바지→티셔츠→...)
* 여러 개의 소스코드로 이루어진 프로그램의 컴파일 순서 결정
* 선행과목을 포함한 수강신청 (CS Basics→C Programming→Algorithms→...)

<br>

### Algorithm

DAG $V$에 대해 <hl>Topological Sort</hl>를 수행하는 과정은 다음과 같다.

1. DAG $V$를 복사하여 DAG $W$에 저장한다.

2. 아래의 과정을 반복한다 :

   1. DAG $W$ 내의 <hl>$\text{in-degree}=0$인 vertex $v$를 조사</hl>한다. (즉, source인 vertex)

      <gray>※ in-degree는 vertex를 기준으로 들어오는 화살표를 의미한다. ※</gray>

   2. source에 해당하는 vertex $v$를 <hl>topological sort의 다음 순서에 넣는다.</hl>

   3. DAG $W$에서 <hl>vertex $v$를 제거</hl>한다.

<br>

### Example

아래와 같이 12개의 vertex로 구성된 DAG를 Topological Sort한 결과를 구하라.

<img src="/files/2021-09-18-cs-algorithm-algo14/image-20210918193131798.png" alt="image-20210918193131798" style="width:200px;" />

**Result:**

$$
C,H,\ D,I,\ A,J,\ B,F,\  G,K,\ E,L
$$

※ vertex 선택 순서에 따라 topological sort의 결과는 달라질 수 있다. ※

<br>

### Implementation

* Initialization

  ```c++
  Type array[vertex_size];
  int ihead = 0, itail = -1;
  ```

* Testing if empty:

  ```c++
  ihead == itail + 1
  ```

* For push

  ```c++
  itail++;
  array[itail] = new vertex;
  ```

* For pop

  ```c++
  Type current_top = array[ihead];
  ihead++;
  ```

<div class="callout">
    topological sorting에 queue를 사용할 때 약간의 trick을 이용하여 topological sorting이 끝나면 queue의 sorting 순서대로 결과가 출력되게끔 만들 수 있다.
</div>

<br>

<img src="/files/2021-09-18-cs-algorithm-algo14/image-20210918193226392.png" alt="image-20210918193226392" style="width:400px;" />

<br>

### Analysis

1. vertex의 in-degree의 개수를 기록하는 table을 초기화: $Θ(V)$
2. 아래의 과정을 $V$번 반복: $Θ(V)$
   1. $\text{in-degree}=0$인 vertex를 찾기위해 in-degree table을 스캔 : $O(V)$
   2. 발견한 source vertex들을 queue에 모두 push && pop : $Θ(V)$
   3. source vertex들의 neighbors의 in-degree값에 -1 연산을 수행 : 
      * adjacency matrix를 사용하는 경우: $Θ(V)$
      * adjacency list를 사용하는 경우: $Θ(E)$

최종적으로 (adjacency list를 사용하는 경우) Topological Sorting은 다음의 complexity를 갖는다.

* adjacency list를 사용하는 경우, $Θ(\lvert{V}\rvert+\lvert{E}\rvert)$.
* adjacency matrix를 사용하는 경우, $Θ(\lvert{V}\rvert^2)$.
* memory requirements = $Θ(\lvert{V}\rvert)$.

<br>

<br>

# Critiacal Path, 임계 경로

<p><hl>tasks에 대해 dependency와 수행시간이 정해져있을 때</hl>> DAG에서 Critical Path(임계 경로)와 Critical Time(임계 시간)을 구하는 알고리즘에 대해 배운다.</p>

<br>

**■ 주요 아이디어**

아래와 같은 dependency를 갖는 tasks가 주어졌을 때, <hl>모든 작업을 마치는 데에 필요한 최소 시간</hl>을 구해보자.

<img src="/files/2021-09-18-cs-algorithm-algo14/image-20210918193424196.png" alt="image-20210918193424196" style="width:150px;" />

* 한 번에 하나의 작업만 수행 가능한 경우:

  A→B→C→D→E의 순서로(사실 어떤 순서든 상관이 없다) 작업을 처리한다면 총 소요시간은 다음과 같다.

  $$
  0.3+0.5+0.4+0.1+0.7=2.0(\text{sec})
  $$

* B와 D를 동시에 수행할 수 있는 경우(<hl>병렬처리</hl>):

  (B,D)→A→C→E

  $$
  \max(0.7,\ \min{(0.7, 0.5)}+0.3+0.4+0.1)=1.3(\text{sec})
  $$

  ※ 모든 Tasks가 끝나는 가장 빠른 시간을 <i><b>Critical Time</b></i>이라고 한다. ※ 

  ※ Ciritical Time에 해당하는 경로를 <b><i>Critical Path</i></b>라고 한다. ※

<br>

## Algorithm

<img src="/files/2021-09-18-cs-algorithm-algo14/image-20210918193449940.png" alt="image-20210918193449940" style="width:400px;" />

<img src="/files/2021-09-18-cs-algorithm-algo14/image-20210918193515037.png" alt="image-20210918193515037" style="width:400px;" />

전체 tasks가 끝나기 위해서는 39.4sec의 시간이 소요되고, 각각의 task는 다음의 Ciritical Time을 갖는다.

| A    | B    | C    | D    | E    | F    |
| ---- | ---- | ---- | ---- | ---- | ---- |
| 5.2  | 11.3 | 31.3 | 39.4 | 26.6 | 17.1 |

<i>Ciritical Path</i>는 <hl>previous Task를 이용하여 D를 기준으로 계산</hl>하면 된다.

$$
D←C←E←F←Φ : \text{Ciritical Path}
$$

이 Critical Path가 tasks를 모두 수행하기 위해 가장 오래 걸리는 tasks에 대한 순서이다. 따라서 <hl>전체 공정을 개선하고싶다면 FECD 중 무언가를 개선해야한다.</hl>

<br>

<br>

# References

1. [집콕]인공지능을 위한 알고리즘과 자료구조: 이론, 코딩, 그리고 컴퓨팅 사고-비선형 자료구조, K-MOOC: [http://www.kmooc.kr/courses/course-v1:SKKUk+SKKU_46+2021_T1/courseware/1158909c057347478d7386a2b7e97214/e9d736660cad4f76bcb9a05974fe0bf2/1?activate_block_id=block-v1%3ASKKUk%2BSKKU_46%2B2021_T1%2Btype%40vertical%2Bblock%4052b62ff6da2147c9a8a65a6056681d91](http://www.kmooc.kr/courses/course-v1:SKKUk+SKKU_46+2021_T1/courseware/1158909c057347478d7386a2b7e97214/e9d736660cad4f76bcb9a05974fe0bf2/1?activate_block_id=block-v1%3ASKKUk%2BSKKU_46%2B2021_T1%2Btype%40vertical%2Bblock%4052b62ff6da2147c9a8a65a6056681d91)

<br>

<br>  











