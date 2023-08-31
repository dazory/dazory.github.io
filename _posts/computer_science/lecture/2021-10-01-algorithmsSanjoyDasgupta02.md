---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments
title: '[SanjoyDasgupta] Ch3. Decompositions of graphs'
excerpt: "Sanjoy Dasgupta가 집필한 Algorithms 교재를 공부한 내용입니다."
date: 2021-10-01
last_modified_at: 2021-10-01
categories:
  - computerScience-lecture
tags: 
   - [SanjoyDasgupta]

use_math: false
comments: true
share : false
---

<div class="info-dialog">
    <div class="title">교재 정보</div>
    <div class="content">
        교재명: Algorithms<br>
        Author: Sanjoy Dasgupta<br>
        사이트: <a href="https://github.com/eherbold/berkeleytextbooks">https://github.com/eherbold/berkeleytextbooks</a>
    </div>
</div>

<br>
<br>

# Ch3. Decompositions of graphs

<br>

## 3.1. Why graphs?

**Graph(그래프)**를 통해 다양한 문제를 명확하고 정밀하게 표현할 수 있다. 이번 장에서는 그래프의 기본 연결 구조를 설명하는 알고리즘 중 가장 기본적인 알고리즘에 대해 알아본다.

> Graph에 관한 기본적인 개념은 이전에 포스팅했던 "kmooc algorithm 시리즈"를 참고하길 바란다.
>
> 관련링크: 

<br>

**● 예시** : Graph 자료구조의 사용 예시 - 지도의 구역 색칠하기 문제

<img src="/files/2021-10-01-cs-algorithm-algorithmsSanjoyDasgupta02
/image-20210910143211131.png" alt="image-20210910143211131" style="width:400px" />

가령 Figure 3.1.(a)와 같이 지도의 구역을 색칠하는 문제는 Figure 3.1.(b)와 같이 graph형태로 표현될 수 있다. 이렇게 표현하면, 각 구역이 어떤 구역과 맞닿아있는지를 쉽게 파악할 수 있어서 색상이 겹치지 않게 색칠하는 것이 가능해진다.

<br>

### 개념

$$
G=(V,E)
$$

※ $G$: graph,  $V$: vertices,  $E$: edges ※

<br>

**■ 분류**

Graph는 크게 두 가지로 분류 가능하다.

1. **무향 그래프 (Undirected graph)**

   ※ vertex $x$와 $y$가 symmetric(대칭) 관계로 edge $e$에 의해 연결될 때, $e=\{x,y\}$로 표현됨 ※

   e.g., 위 "지도 색칠 문제"가 이에 해당

2. **유향 그래프 (Directed graph)**

   ※ vertex $x$에서 $y$로 가는 directed edges $e$는 $e=(x,y)$로 표현됨 ※

   e.g., World Wide Web: Internet에서 각 site 정보를 vertex에 담고 있다.

<br>

### Representations

$n(=│V│)$개의 vertices $v_1, ..., v_n$를 갖는 graph $G$를 표현하는 방식에는 크게 *Adjacency matrix* 방식과 *Adjacency list* 방식이 있다.

**■ Adjacency matrix**

![image-20210910145759418](/files/2021-10-01-cs-algorithm-algorithmsSanjoyDasgupta02
/image-20210910145759418.png)

**adjacency matrix** 형태 ($n×n$ array)로 표현될 수 있다.

$$
a_{ij}
= 
\begin{cases}
	1 & \text{if there is an edge from $v_i$ to $v_j$}\\
	0 & \text{otherwise}
\end{cases}
$$

**● 특징**

* 특정 edge가 constant time 내에 확인된다 : $Θ(1)$ (😍: 빠르군!)
* $O(n^2)$의 메모리 공간을 요구한다. (🤔: edges 개수가 많지 않다면 공간 낭비가 심하군!)

<br>

**■ Adjacency list**

![image-20210910145903347](/files/2021-10-01-cs-algorithm-algorithmsSanjoyDasgupta02
/image-20210910145903347.png)

각 vertex에 대해 $│V│$개의 **linked lists** 형태로 표현되며, 각 vertex는 연결된 vertices의 정보를 담고있다.


**● 특징**

* undirected graph의 경우 $2│E│$의, directed graph의 경우 $│E│$의 메모리 공간을 갖는다. : overall memory complexity = $O(│E│)$
* 특정 edge 탐색에는 $O(│V│)$의 시간 복잡도가 소요된다. (😅: adjacency matrix보단 느리지만 반복적인 탐색이 가능하므로 편리하군!)

<br>

**■ Adjacency matrix vs. Adjacency list**: 무엇을 사용할 것인가?

graph를 표현하는 두 가지 기법 *adjacency matrix*와 *adjacency list* 은 어떤 특정 방법이 더 좋다고 말할 수 없다. 상황에 따라 특정 방법이 유리할 수도, 불리할 수도 있기 때문이다.

<div class="callout">
    graph에서 edges의 최소 개수는 V개이며, 최대 개수는 V²개이다.
</div>


* **Case1. $│E│ \approx │V│^2$ : dense graph**

  edges의 개수가 최대값에 가까운 경우, graph를 "*dense*"하다고 한다. 

* **Case2. $│E│ \approx │V│$ : sparse graph**

  edges의 개수가 최소값에 가까운 경우, graph를 "*sparse*"하다고 한다.

graph의 edges 개수 $│E│$는 graph에서 더 좋은 알고리즘을 선택하는 데에 중요한 요소이다.

**● 예시** : World Wide Web

검색엔진은 약 80억개의 웹사이트를 갖고 있다. 이 경우 adjacency matrix보다 adjacency list로 구현하는 것이 더 효과적이다. adjacency matrix로 표현하는 경우 $\text{80억}×\text{80억}$개의 메모리가 필요한데, 80억 개의 웹사이트 규모에 비해 각 웹사이트를 연결하는 edges의 개수는 sparse하기 때문에 adjacency list로 구현하는 것이 메모리 낭비를 줄일 수 있는 방법이기 때문이다.

<br>

<br>

## 3.2. Depth-first search in undirected graphs

<br>

### 3.2.1. Exploring mazes

![image-20210910152431498](/files/2021-10-01-cs-algorithm-algorithmsSanjoyDasgupta02
/image-20210910152431498.png)

위 그림과 같이 미로에서 길찾는 문제는 길의 각 지점을 vertices로 삼아 graph로 표현함으로써 해결할 수 있다.

<br>

**■ node $v$에서 갈 수 있는 모든 nodes를 찾는 알고리즘**

```pseudocode
procedure explore(G, v): 
Input:	G=(V,E) is a graph; v∈V
Output:	visited(u) is set to true for all nodes u reachable from v

	visited(v) = true
	previsit(v)
	for each edge (v,u) ∈ E:
		if not visited(u):	explore(u)
	postvisit(v)
```

※ 여기서 `previsit()`, `postvisit()`에 대해서는 3.2.4에서 자세히 다룬다. ※

그래프로 표현한 미로 문제에서 node $A$에서 갈 수 있는 모든 nodes를 찾기위해 explore(G,A)를 수행한 결과는 다음과 같이 <u>tree edges</u>로 표현할 수 있다.

![image-20210910231631953](/files/2021-10-01-cs-algorithm-algorithmsSanjoyDasgupta02
/image-20210910231631953.png)

※ tree는 cycle이 없는 connected graph이다. <br>※ 여기서 점선은 이전에 방문한 nodes로 되돌아가는 <u>back edges</u>를 의미한다. ※

<br>

**● 알고리즘 동작**

1. vertex $v$를 방문했다고 표시

2. previsit($v$)

3. vertex $v$와 연결된 edges 집합 $E$에 있는 각 edge에 대해 아래 동작을 반복:

   3.1. edge $(v,u)$일때, vertex $u$를 방문하지 않았다면, vertex $u$에 대해 explore을 실행

4. postvisit($v$)

<br>

**● 알고리즘 증명**

graphs 연구에서 종종 사용되는 능동적 유도 과정을 통해 위 알고리즘이 vertex $v$에서 갈 수 있는 모든 vertices를 탐색가능함을 증명해본다.

![image-20210910231135433](/files/2021-10-01-cs-algorithm-algorithmsSanjoyDasgupta02
/image-20210910231135433.png)

알고리즘의 타당성을 위해, explore($v$)를 통해 직접적으로 연결되지 않은 vertex $u$를 탐색할 수 있음을 증명하면 된다.

1. explore($v$)는 $v$와 직접적으로 연결된 vertex $z$를 탐색할 수 있다.
   이를 통해 explore($z$)를 재귀적으로 호출하게 된다.
2. 따라서 vertex $v$와는 직접적으로 연결되지 않지만 vertex $v$의 neighbors와 직접 연결된 vertex $w$를 탐색할 수 있다.
   이를 통해 explore($w$)를 재귀적으로 호출하게 된다.
3. 이러한 행위를 반복함으로써 vertex $v$와 직접 연결되지는 않지만 도달할 수 있는 모든 vertices(e.g., $u$)를 탐색할 수 있다. 

<br>

### 3.2.2. Depth-first search

**depth-first search (DFS)** 알고리즘은 explore을 반복적으로 수행함으로써 전체 graph를 순회한다.

**■ 알고리즘**

```pseudocode
procedure dfs(G):
	for all v ∈ V:
		visited(v) = false
	for all v ∈ V:
		if not visited(v):	explore(v)
```

<br>

**■ 실행시간 분석**

1. 각 node에 대해 고정적으로 실행되는 $O(1)$의 작업(e.g., visited (혹은 pre/postvisit))

   ⇒ total complexity = $O(│V│)$

2. 가보지 않은 곳으로 가기 위해 인접 edges를 탐색하기 위한 loop (in explore function)

   ⇒ total complexity = $O(│E│)$ (∵ 각 edge마다 2번씩 탐색되므로)

따라서 DFS의 overall running time complexity = $O(│V│ + │E│) $

<br>

### 3.2.3. Connectivity in undirected graphs

undirected graph의 Connectivity(연결성)에 대해 알아본다.

* **Connectedg graph** : 어떤 node에서 다른 node로 가는 path가 언제나 존재하는 경우

* **Unconnected graph** : 어떤 node에서 다른 node로 가는 path가 존재하지 않을 수도 있는 경우

  * *connected components*(= subgraph)로 구성된다.

    ※ connected components는 분리된 connected regions을 의미한다. ※

DFS 알고리즘은 각 node $v$에 정수 `ccnum[v]`값을 할당함으로써 graph의 연결성을 검증하는 수단으로 사용될 수 있다. (즉, connected graph라면 ccnum[v]의 값이 $│V│-1$개일 것이다.)

```pseudocode
procedure previsit(v):
	ccnum[v] = cc
```

※ cc의 초기값은 0이며, DFS가 explore을 호출할 때마다 하나씩 값이 증가된다. ※

<br>

### 3.2.4. Previsit and postvisit orderings

*explore* 알고리즘에서 수행되는 `previsit()`과 `postvisit()`의 동작에 대해서 자세히 살펴본다. DFS알고리즘은 undirected graph의 연결성뿐만 아니라 보다 다양한 문제에서 사용될 수 있는데, 이러한 다재다능성은 previsit과 postvisit에 의해 실현된다. 가령, 각 node에 대해 previsit과 postvisit은 다음의 두 중요한 events 시간을 기록한다.

* **previsit** : the moment of first discovery (해당 node를 처음으로 발견된 순간)

  ```pseudocode
  procedure previsit(v):
  	pre[v] = clock
  	clock = clock + 1
  ```

* **postvisit** : the moment of final departure (해당 node를 떠나는 순간)

  ```pseudocode
  procedure postvisit(v):
  	post[v] = clock
  	clock = clock + 1
  ```

tree edges에 previsit과 postvisit에 의해 기록된 시간을 표시하면 다음과 같다.

![image-20210910234710727](/files/2021-10-01-cs-algorithm-algorithmsSanjoyDasgupta02
/image-20210910234710727.png)

※ 예를 들어, node $I$는 5번째 순간에 최초로 탐색되었으며, 8번째 순간에 $I$ 너머의 모든 vertices에 대해 탐색이 완료되어 explore()실행이 종료되었음을 알 수 있다. ※

<br>

**■ 특징**

어떤 nodes $u$와 $v$에 대해 두 개의 구간 [$\text{pre($u$)}$, $\text{post($u$)}$]와 [$\text{pre($v$)}$, $\text{post($v$)}$]는 서로 disjoint(분리되)거나 하나가 다른 하나를 포함하는 관계이다.

※ ∵ DFS알고리즘은 stack구조를 이용하여 구현되는데, stack은 LIFO(Last-In First-Out)으로 동작한다. 따라서 [$\text{pre($u$)}$, $\text{post($u$)}$]는 본질적으로 vertex $u$가 stack에 있는 동안의 시간을 의미한다. ※

<br>

<br>

## 3.3. Depth-first search in directed graphs

이전까지는 undirected graph에 대한 DFS에 대해 살펴보았다. 이번에는 edges가 방향을 갖는 directed graphs에 대해 DFS를 적용해본다.

### 3.3.1. Types of edges

**■ 배경지식**: tree에서 nodes간의 관계에 대한 terminology(용어)

![image-20210911000711232](/files/2021-10-01-cs-algorithm-algorithmsSanjoyDasgupta02
/image-20210911000711232.png)

※ 위 이미지를 기준으로 각 terminology에 대해 설명한다. ※

* $A$: search tree의 ***root***. 즉, 이 외의 모든 nodes가 *descendant(자손)*이다.
* $E$: ***descendants***로 nodes $F, G, H$ 를 갖는다. 반대로, 이 세 nodes $F,G,H$에 대한 ***ancestor***이다.
* $C$: node $D$의 ***parent(부모)***;   $D$: node $C$의 ***child(자식)***.

즉, 요약하자면 $v_{\text{parent}}→v_{\text{child}}$이다. 

<br>

**■ Types of edges**

undirected graph에 대해 DFS를 적용할 때, edges를 다음과 같이 두 종류로 분류할 수 있었다.

* **tree edges** 
* **nontree edges**

이와 달리 undirected graph에서는 edges를 다음과 같이 네 종류로 분류할 수 있다.

![image-20210911001103123](/files/2021-10-01-cs-algorithm-algorithmsSanjoyDasgupta02
/image-20210911001103123.png)

* **Tree edges** : DFS forest의 부분에 해당하는 edges  ($v_{\text{parent}}→v_{\text{child}}$)

  ※ solid한 실제 edges에 해당. 이 외의 모든 edge 종류들은 점선에 해당하는 가상의 edges이다. ※

* **Forward edges** : DFS tree에서 어떤 한 node에서 *nonchild* descendant로 향하는 edges ($v_{\text{ancestor}}→v_{\text{nonchild descendant}}$)

* **Back edges** : DFS tree에서 ancestor를 향하는 edges ($v_{\text{descendant}}→v_{\text{ancestor}}$)

* **Cross edges** : descendant으로도 ancestor으로도 향하지 않는 edges. 즉, 이미 완벽하게 탐색 완료된(= already postvisited) node를 향하는 edges

<br>

**■ Ancestor와 descendant의 관계**

Ancestor와 descendant의 관계는 `pre`와 `post` 값을 통해 바로 읽을 수 있다. 가령, edge $(u,v)$의 edge 종류는 다음과 같이 판별 가능하다.

* **Tree edges (or Forward edges)**: $u→v$

  $$
  [u\ [v\ v]\ u]
  $$

* **Back edges** : $v→u$

  $$
  [v\ [u\ u]\ v]
  $$

* **Cross edges** : 

  $$
  [u\ u]\ [v\ v] \text{ or } [v\ v]\ [u\ u]
  $$

<br>

### 3.3.2. Directed acyclic graphs

**■ Acyclicity Test with DFS**

directed graph는 path의 종류에 따라 두 종류로 구분할 수 있다.

* ***cyclic* graph**: circular(순환하는) path $v_0→v_1→...→v_k→v_0$를 갖는 graph
* ***acyclic* graph**: cycle이 없는 graph

이때, 한 번의 DFS알고리즘을 통해 graph의 acyclicity를 확인할 수 있다.

$$
\text{directed graph가 cycle을 갖는다.} ⇔ \text{DFS에서 back edge가 존재한다.}
$$

※ ∵ $(u,v)$가 back edge라면, search tree에 $v→u$ 경로와 함께 cycle을 구성할 수 있기 때문에 ※<br>※ ∵ graph가 어떤 cycle $v_0→v_1→...→v_k→v_0$를 갖는다면, 이 cycle의 첫 번째 node $v_0$는 다른 모든 nodes를 descendants로 갖는데, 마지막 $v_k$는 $v_0$로 향하는 back edge를 갖는다. ※

<br>

**■ Directed Acyclic Graphs (DAGs)**

**Directed Acyclic Graphs (DAGs)**은 인과관계, 계층구조, 시간 종속성과 같은 관계를 모델링하기에 좋다. 어떤 task를 수행하기 전에 다른 task를 할 수 없다는 제약조건은 두 nodes간의 edge 방향으로 간편하게 표현되기 때문이다. 

※ 반면 그래프가 Directed Cyclic Graphs인 경우에는 이러한 순서가 의미가 없게 된다. ※

작업의 순서는 *linearize*(혹은 *topologically sort*)를 통해 표현할 수 있으며, 이는 DFS로 구현 가능하다. 이때, linearize는 post numbers를 내림차순으로 나열함으로써 구현된다.

※ DAGs는 back edges를 가질 수 없다는 점을 기억하자. ※<br>※ DFS를 이용하여 dag의 nodes를 sort하는 데에는 linear time이 소요된다. (∵ linearize) ※

<br>

**● DAG의 특징**

* *dag*에서 모든 edge는 작은 post number를 갖는 node로 향한다.

  ∵ linearized가 post numbers의 내림차순으로 구현되므로

* 모든 *dag*는 적어도 하나의 source와 적어도 하나의 sink를 갖는다.

  ∵ linearization은 ① source를 찾아 출력하고 graph에서 삭제 ② graph가 빌 때까지 이러한 과정을 반복하는 과정을 통해 구현될 수 있기 때문에

<br>

## 3.4. Strongly connected components

### 3.4.1. Defining connectivity for directed graphs

**■ Connectivity in directed graphs**

undirected graphs에서 connectivity는 각각의 connected components에 대해 DFS를 수행함으로써 증명되었다. 반면, **directed graphs에서 connectivity**는 다음과 같이 정의된다.

$$
\text{path $u→v$와 path $v→u$가 모두 존재할 때, 두 nodes $u, v$는 connected이다.}
$$

이러한 정의를 통해 ***strongly connected components***의 개념을 정의할 수 있다.

<br>

**■ Strongly Connected Components**

![image-20210911005311792](/files/2021-10-01-cs-algorithm-algorithmsSanjoyDasgupta02
/image-20210911005311792.png)

위 그림에서 (a)는 directed graph를 보여주며, 이를 구성하는 *strongly connected components*를 하나의 single meta-node로 표현한 결과(***meta-graph***)는 (b)이다. 이때 이 meta-graph는 dag임이 명확하다 (∵ nodes간에 cycle이 존재한다는 것은 strongly connected component임을 의미하므로 meta-node로 치환된다).

**● directed graph의 특징**

모든 directed graph는 strongly connected components로 구성된 하나의 dag로 표현될 수 있다. 이러한 특징은 directed graph가 두 가지 연결성 구조를 가지고 있음을 시사한다.

1. 상위 레벨에 존재하는 하나의 dag. (linearized 가능하다.)
2. 하위 레벨에 존재하는 dag의 nodes. (더 자세히 조사할 수 있다.)

<br>

### 3.4.2. An efficient algorithm

Directed Graph를 strongly connected components로 분해하는 것은 여러 유용한 정보를 제공해준다.

※ 운좋게도, 발전된 DFS를 통해 분해 과정에 linear time이 소요된다. ※

**● 특징 1.** explore subroutine이 node $u$에서 시작할 때, 이 subroutine은 $u$에서 도달가능한 모든 nodes를 방문한 시점에 종료된다.

이러한 특징으로 인해 *sink* strongly connected component 내부의 어떤 node에서 explore를 수행하면 정확하게 해당 component만을 얻을 수 있다. <br>⇒ sink strongly connected component를 알고있을 때, 하나의 strongly connected component를 찾는 방법

<br>

**● 특징 2.** DFS를 통해 구한 *최대 post number*에 해당하는 node는 *source* strongly connected component에 속한다.

이를 통해 graph의 source strongly connected components 내에 있는 node를 찾을 수 있다.

**● 특징 3.** $C$와 $C'$가 strongly connected components일 때, $C$에 있는 어떤 node에서 $C'$에 있는 다른 node로 가는 edge가 존재한다면, $C$에서의 최대 post number는 $C'$에서의 최대 post number보다 크다.

> 증명:
>
> * $C$가 $C'$보다 더 먼저 탐색된 경우, 특징1$^*$에 의해 도달가능한 모든 nodes를 방문한 뒤 (즉, $C'$가 모두 탐색된 후)에 $C$에 대한 탐색이 종료된다. 따라서 늦게 종료된 $C$의 post number가 $C'$의 post number보다 크다. 
> * $C'$가 $C$보다 더 먼저 탐색된 경우, 특징1$^*$에 의해 $C'$가 이미 종료된 후에 $C$가 탐색된다.

따라서 strongly connected components는 내부의 최대 post number를 내림차순으로 정렬하여 linearized할 수 있다. 

<br>

특징2를 활용하여 sink strongly connected component를 알 수 있다. $G^R$과 $G$는 같은 strongly connected components를 가지므로 특징2를 활용하여 $G^R$에 대해 source strongly connected components를 구하면 이는 $G$의 sink strongly connected component가 된다.

![image-20210911012216827](/files/2021-10-01-cs-algorithm-algorithmsSanjoyDasgupta02
/image-20210911012216827.png)

※ 이때 $G^R$은 graph $G$의 reverse graph이다. ※

<br>

따라서 DFS를 이용하여 directed graph를 strongly connected components로 분해하는 알고리즘은 다음과 같다.

1. graph $G^R$에 대해 DFS를 수행한다.
2. step1 과정을 통해 얻은 post numbers의 내림차순에 해당하는 nodes 순서대로, grpah $G$에 대해 undirected connected components algorithm(from Section 3.2.2)를 수행한다.

※ linear-time에 수행된다. ※

<br>

![image-20210911012641728](/files/2021-10-01-cs-algorithm-algorithmsSanjoyDasgupta02
/image-20210911012641728.png)

위 graph에 이 알고리즘을 적용하는 과정은 다음과 같다.:

1. graph $G^R$에 대해 DFS를 수행하여 얻은 post numbers의 내림차순 결과는 다음과 같다.:

   $G, I, J, L, K, H, D, C, F, B, E, A.$

2. 위 결과 순서대로 graph G에 대해 undirected connected components algorithm 수행 결과는 다음과 같다.:

   $\{G,H,I,J,K,L\}, \{D\}, \{C,F\}, \{B,E\}, \{A\}$

<br>

<br>


# References

1. eherbold/berkeleytextbooks, github: [https://github.com/eherbold/berkeleytextbooks](https://github.com/eherbold/berkeleytextbooks)

<br>

<br>  





