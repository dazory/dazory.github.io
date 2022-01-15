---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments
title: '[ALGO-03] Lect3. 비선형 자료구조'
excerpt: "[ALGO] 성균관대학교 허재필 교수님의 K-mooc 알고리즘 강의를 듣고 공부한 자료입니다."
date: 2021-08-08
last_modified_at: 2021-08-08
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


<img src="/files/2021-08-08-cs-algorithm-algo03/image-20210802140417579.png" alt="image-20210802140417579" style="width:600px;" />

이번 시간에는 대표적인 <hl>비선형 자료구조</hl>인 <hl>트리와 그래프</hl>에 대해 알아본다.

<br>


# 트리 자료구조

트리는 <hl>노드들의 집합체</hl>로, <hl>계층적인 관계</hl>를 나타내는 자료구조이다. 디렉토리 구조는 대표적인 tree구조이다..

<img src="/files/2021-08-08-cs-algorithm-algo03/image-20210802141043317.png" alt="image-20210802141043317" style="width:400px;" />


<br>

## 특징

* 정보는 nodes에 저장된다.
* 첫 번째 노드는 <hl>root</hl>라고 불린다 <gray>(위 그림에서 A에 해당)</gray>
* 각 노드는 <hl>자손 노드(children)</hl>들을 가질 수 있다.
* root를 제외한 각 노드는 하나의 <hl>부모 노드(parent)</hl>를 가진다.

<br>

## 용어

* `Degree`: 해당 노드가 갖는 <hl>자식 노드의 개수</hl>

  * `Leaf 노드`: degree = 0인 노드
  * `Internal 노드`: degree != 0인 노드

* `Sibling`: 같은 부모 노드를 공유하는 <hl>자매 노드</hl>

* `Unordered tree`: <hl>자식 노드의 순서가 무시</hl>될 수 있는 트리 구조<br>
`Ordered tree`: <hl>자식 노드간에 순서가 존재</hl>하는 트리 구조

* `Path`: <hl>노드들의 시퀀스</hl> $(a_0, a_1, ..., a_n)$​​​​ (이때, $a_{k+1}$: $a_k$​​​​의 자식노드)

  * 예시: path(B,E,G)의 길이는 2이다.

    <img src="/files/2021-08-08-cs-algorithm-algo03/image-20210802142050795.png" alt="image-20210802142050795" style="width:300px;" />

* `depth`: <hl>루트노드부터 해당 노드까지의 경로의 길이</hl>

  * 예시: depth(B)=1, depth(E)=2, depth(F)=3

    <img src="/files/2021-08-08-cs-algorithm-algo03/image-20210802142602411.png" alt="image-20210802142602411" style="width:300px;" />

* `height`: 트리에 존재하는 <hl>가장 큰 depth값</hl>

  * 예시: <br>
  ⒜ 루트 노드만 존재하는 경우 height=0<br>
  ⒝ 아무 노드도 존재하지 않는 경우 heigth=-1

* `ancestor`: a는 b의 ancestor (노드 a에서 노드 b로 가는 path가 존재할 때)<br>
`descendant`: b는 a의 descendant (노드 a에서 노드 b로 가는 path가 존재할 때)<br>
`strictly descendant`: 자기 자신이 자기 자신의 자손이나 부모가 되는 것을 금지시키는 조건 <gray>※ 일반적인 경우, 자기 자신은 자기 자신의 ancestor이자 descendant이다. ※</gray>

<br>

## 표현

<img src="/files/2021-08-08-cs-algorithm-algo03/image-20210802151123625.png" alt="image-20210802151123625" style="width:400px;" />

각 노드가 <hl>자식 노드를 참조</hl>하고 있는 구조로 표현된다.

<br>
<br>

# 그래프 자료구조

그래프는 <hl>데이터 사이의 인접한 정보를 저장</hl>하는 자료구조이다.

<img src="/files/2021-08-08-cs-algorithm-algo03/image-20210802201823939.png" alt="image-20210802201823939" style="width:300px;" />

**■ 응용**

* SNS상에서 친구 관계
* 회로 사이의 component간의 연결성
* 선수과목 정보 표현

<br>

## 용어

* `vertex`: <hl>정점(node)</hl>, $V$라 표현

* `Objects`: 저장하고자 하는 객체. 유한개의 <hl>nodes(혹은 vertices)의 집합</hl>

* `edge`: <hl>vertex 간의 연결</hl>, $E$라 표현

* `Relationship`: 유한개의 <hl>edges(혹은 arcs, links)의 집합</hl>

* `degree`: <hl>이웃 vertex의 개수</hl>

  * 예시: degree($v_1$)=3

    <img src="/files/2021-08-08-cs-algorithm-algo03/image-20210802202941366.png" alt="image-20210802202941366" style="width:300px;" />
    

* `neighbor`: <hl>인접한(adjacent) vertex의 집합</hl>

* `sub-graph`: original graph에서 <hl>일부 vertex와 edge를 sampling</hl>하여 얻을 수 있는 그래프

* `path`: <hl>vertex간의 연결</hl>: $(v_0, v_1, ..., v_k)$

  * `trivial path`: <hl>length=0</hl>인 path
  * `simple path`: 경로상에, 처음과 마지막을 제외하고 중복이 없는 경우<br>
    `simple cycle`: <hl>처음과 마지막 vertex가 일치</hl>하는 <hl>simple path</hl>
    * 예시<br>
      case1) A-B-C : simple path<br>
      case2) A-B-C-A : simple path && simple cycle<br>
      case3) A-B-C-B-A : simple path아님

* `connectedness(연결성)`: graph의 vertex끼리 어떤 path로든 <hl>연결되어있는지</hl> 여부

  * 예시: 아래 그래프에서 흰색 영역에 존재하는 path가 끊어지면 conntedness 성질이 사라진다.

    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f4/Network_Community_Structure.svg/220px-Network_Community_Structure.svg.png" alt="img" style="width:200px;" />


<br>

## 유형

그래프의 속성에 따라 다양한 종류의 그래프가 존재한다.

* `Undirected graph`: edge에 <hl>방향이 없는</hl> 무향 그래프
* `directed graph`: edge에 <hl>방향성이 존재</hl>하는 유향 그래프
* `Weighted graph`: <hl>가중치</hl>가 있는 그래프
* `Tree`: <hl>unique path</hl>를 갖는 conncted graph
* `forest`: <hl>tree들의 모음</hl>

<br>

### 1. Undirected Graphs

<img src="/files/2021-08-08-cs-algorithm-algo03/image-20210802204012542.png" alt="image-20210802204012542" style="width:300px;" />

vertices의 모음으로 표현되는 <hl>방향이 없는 그래프</hl>

<br>

**■ 특징**

$V={v_1, v_2, ..., v_n}$일 때


* vertices의 개수 $│V│=n$

* vertices를 연결하는 edges E = $\{v_i, v_j\}$​   <gray>※ 순서가 없는 쌍 ※</gray>

<br>

**■ 예시**

연결성을 표현하기 위해 `adjacency matrix`나 `adjacency list`를 사용할 수 있다.

<img src="/files/2021-08-08-cs-algorithm-algo03/image-20210802152506507.png" alt="image-20210802152506507" style="width:300px;" />

* 9개의 vertex가 존재: $V=\{v_1, v_2, ..., v_9\}$​​, $│V│=9$​​
* 5개의 edge가 존재: $E=\{\{v_1, v_2\}, \{v_3,v_5\}, \{v_4, v_8\}, \{v_4, v_9\}, \{v_6, v_9\}\}$, $│E│=5$

<br>

**■ 최대 edge개수**

자기 자신으로 가는 edge는 없다고 가정하면, <hl>undirected graph에서 최대한 많은 edge의 개수</hl>는 다음과 같다.

$$
|E|≤
_VC_2 =
\binom{|V|}{2} =
\frac{|V|(|V|-1|)}{2} =
O(|V|^2)
$$
<br>

### 2. Directed Graphs

<img src="/files/2021-08-08-cs-algorithm-algo03/image-20210802210956503.png" alt="image-20210802210956503" style="width:300px;" />

vertices의 모음으로 표현되는 <hl>방향이 있는 그래프</hl>

<br>

**■ 용어**

* `in_degree`: 방향이 해당 <hl>node로 향하는</hl> edge의 개수<br>
  `out_degree`: 해당 <hl>node에서 나오는 방향</hl>인 edge의 개수

  * 예시: in_degree($v_1$)=1, out_degree($v_1$)=2

    <img src="/files/2021-08-08-cs-algorithm-algo03/image-20210802211314168.png" alt="image-20210802211314168" style="width:200px;" />


* `source`: <hl>in_degree=0</hl>인 node<br>
  `sink`: <hl>out_degree=0</hl>인 node

* `strongly connected`: <hl>방향성을 고려</hl>했을 때 <hl>모든 pair간에 경로가 존재</hl>하는 경우<br>
  `weakly connected`: <hl>방향성을 무시</hl>할 때 <hl>모든 pair간에 경로가 존재</hl>하는 경우

* `directed acyclic graph (DAG, 유향 비순환 그래프)`: <hl>순환하지 않는 유향 그래프</hl>

  예시: 

  <img src="/files/2021-08-08-cs-algorithm-algo03/220px-Topological_Ordering.svg.png" alt="img" style="width:150px;" />

<br>

### 3. Weighted Graphs

<img src="/files/2021-08-08-cs-algorithm-algo03/image-20210802204557687.png" alt="image-20210802204557687" style="width:300px;" />

edge가 <hl>연결성</hl>뿐만 아니라 <hl>가중치</hl>도 표현하는 그래프

<br>

**■ 응용**

vertex간의 거리나 에너지 소모등을 표현하는 경우에 주로 사용되며, 이러한 그래프를 통해 <hl>shortest path 문제</hl>를 해결할 수 있다.

<br>

**■ path length**

단순히 연결된 path의 개수로 표현되는 un-weighted graphs와 달리, weighted graph는 <hl>path의 가중치의 합으로 length</hl>가 표현된다.

예시: $\text{length of }(v_1, v_3, v_5, v_4)=5.1+1.3+1.1=7.5$

<img src="/files/2021-08-08-cs-algorithm-algo03/image-20210802204836778.png" alt="image-20210802204836778" style="width:300px;" />


<br>

### 4. Tree

graph가 <hl>다음의 조건을 만족</hl>하는 경우, tree라고 부를 수 있다.

1. cycle이 없음
2. 연결 그래프

<br>

**■ 특징**

* <p><hl>unique path</hl>를 갖는다. ⇒ 따라서 $│E│=│V│-1$</p>

  <div class="callout">root를 제외한 모든 node가 하나의 parent와 연결된 path를 가지며, parent가 누구냐에 따라 구조가 달라진다고 생각하면 |E|=|V|-1의 결과를 쉽게 얻을 수 있다.</div>

* <p><hl>하나의 edge를 추가하면 cycle</hl>이 생긴다.</p>

* <p><hl>하나의 edge를 제거하면</hl> disconnected graph가 되며, <hl>두 개의 tree로 분리</hl>된다.</p>

<br>

### 5. Forest

<img src="/files/2021-08-08-cs-algorithm-algo03/image-20210802210313744.png" alt="image-20210802210313744" style="width:300px;" />

<p><hl>하나 이상의 tree로 이루어진 집합</hl>을 forest라고 한다.</p>

<br>

**■ 특징**

* <p><hl>cycle이 없다</hl> ⇒ 따라서 $│E│<│V│$ </p>

  <div class="callout">forest에서 root를 하나만 남도록 연결하면(edge를 추가하면) 하나의 tree가 된다. 즉, │E│=│V│-1인 tree의 edge 개수보다 forest의 edge개수가 더 적으니 언제나 │E│<│V│</div>

* <p><hl>tree의 개수 = $│V│-│E│$</hl></p>

  <div class="callout">|V|에서 |E|를 빼면 root의 개수가 되므로 tree의 개수는 |V|-|E|</div>

* <p><hl>edge를 제거하면 tree가 하나 더 생성</hl>된다.</p>

<br>

## 표현

그래프를 표현하는 대표적인 방식은 다음과 같다.

### 1. Binary-relation list

<p><hl>edge를 나열</hl>하여 graph를 표현한다.</p>


**■ 예시**

<img src="/files/2021-08-08-cs-algorithm-algo03/image-20210802212242204.png" alt="image-20210802212242204" style="width:300px;" />

$\{(1,2), (71,4), (3,5), (4,2), (4,5), (5,2), (5,3), (6,9), (7,9), (8,4)\}$

<br>

**■ 특징**

* 메모리 사용량 = $│E│$​
* 어떤 두 vertex간에 edge가 존재하는지 확인하기 위한 계산량 $≤│E│$​​​​​
* 어떤 vertex의 neighbor를 구하기 위한 계산량 $≤│E│$​​

<br>

### 2. Adjacency Matrix

vertex $v_j$​​에서 $v_k$​​로 가는 <hl>edge가 존재할 때 $(v_j, v_k)$​​의 값을 True로</hl> 지정한다.

<br>

**■ 특징**

* 메모리 사용량 = $│V│^2$
* 어떤 두 vertex 간에 edge가 존재하는지 확인하기 위한 계산량 = 1
* 어떤 vertex의 neighbor를 구하기 위한 계산량 = $O(│V│)$
  <gray>※ computational overhead ※</gray>
* weighted graph인 경우, true/false 대신 weight 값으로 표현

<br>

### 3. Adjacency List

일반적인 알고리즘에서 가장 많이 사용되는 그래프 표현법으로, <hl>각 노드를 기준으로 자기 자신과 연결된 vertex를 list형태</hl>로 표현한다.


**■ 예시**

<img src="/files/2021-08-08-cs-algorithm-algo03/image-20210802213201489.png" alt="image-20210802213201489" style="width:300px;" />

<table>
  <tr>
    <th style="background-color:black; color:white; text-align:center;">vertex</th>
    <th style="background-color:black; color:white; text-align:center;">adjacent to</th>
  </tr>
  <tr><td style="width:100px; text-align:center; border: 1px solid #333;">v1</td><td style="width:100px; text-align:center; border: 1px solid #333;">v2, v4</td></tr>
  <tr><td style="width:100px; text-align:center; border: 1px solid #333;">v2</td><td style="width:100px; text-align:center; border: 1px solid #333;"> </td></tr>
  <tr><td style="width:100px; text-align:center; border: 1px solid #333;">v3</td><td style="width:100px; text-align:center; border: 1px solid #333;">v5</td></tr>
  <tr><td style="width:100px; text-align:center; border: 1px solid #333;">v4</td><td style="width:100px; text-align:center; border: 1px solid #333;">v2, v5</td></tr>
  <tr><td style="width:100px; text-align:center; border: 1px solid #333;">v5</td><td style="width:100px; text-align:center; border: 1px solid #333;">v2, v3</td></tr>
  <tr><td style="width:100px; text-align:center; border: 1px solid #333;">v6</td><td style="width:100px; text-align:center; border: 1px solid #333;">v9</td></tr>
  <tr><td style="width:100px; text-align:center; border: 1px solid #333;">v7</td><td style="width:100px; text-align:center; border: 1px solid #333;">v9</td></tr>
  <tr><td style="width:100px; text-align:center; border: 1px solid #333;">v8</td><td style="width:100px; text-align:center; border: 1px solid #333;">v4, v5</td></tr>
  <tr><td style="width:100px; text-align:center; border: 1px solid #333;">v9</td><td style="width:100px; text-align:center; border: 1px solid #333;"> </td></tr>
</table>

<br>

**■ 특징**

* 메모리 사용량 = $│V│ < │E│$​

<br>

# References

1. [집콕]인공지능을 위한 알고리즘과 자료구조: 이론, 코딩, 그리고 컴퓨팅 사고-비선형 자료구조, K-MOOC: [http://www.kmooc.kr/courses/course-v1:SKKUk+SKKU_46+2021_T1/courseware/1158909c057347478d7386a2b7e97214/e9d736660cad4f76bcb9a05974fe0bf2/1?activate_block_id=block-v1%3ASKKUk%2BSKKU_46%2B2021_T1%2Btype%40vertical%2Bblock%4052b62ff6da2147c9a8a65a6056681d91](http://www.kmooc.kr/courses/course-v1:SKKUk+SKKU_46+2021_T1/courseware/1158909c057347478d7386a2b7e97214/e9d736660cad4f76bcb9a05974fe0bf2/1?activate_block_id=block-v1%3ASKKUk%2BSKKU_46%2B2021_T1%2Btype%40vertical%2Bblock%4052b62ff6da2147c9a8a65a6056681d91)





