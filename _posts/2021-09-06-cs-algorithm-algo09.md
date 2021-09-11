---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments
title: '[ALGO-09] Lect9. 힙 자료구조의 정의와 연산 및 복잡도 분석'
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

이번 시간에는 <hl>힙 자료구조의 정의와 연산 및 복잡도 분석</hl>에 대해 배운다.

<br>

# 힙 자료구조의 정의와 연산

<p><hl>이진힙 자료구조</hl>의 정의와 <hl>Push, Pop 연산 방법</hl>에 대해 학습한다.</p>

<br>

## Binary Heap

<b>Binary Heap</b>은 <hl>Tree에서 최소값, 최대값을 빠르게 찾을 수 있도록 하는 자료구조</hl>이다. Binary Heap은 특성에 따라 다음과 같이 분류될 수 있다.

* **Min Heap**: (sub) tree의 모든 root node가 자손보다 언제나 값이 작은 tree
* **Max Heap**: (sub) tree의 모든 root node가 자손보다 언제나 값이 큰 tree

<gray>※ sibling끼리는 어떠한 관계성이 존재하지 않는다. ※</gray>



**■ (예시) Binary Min Heap**

<img src="/files/2021-09-06-cs-algorithm-algo09/image-20210902221741019.png" alt="image-20210902221741019" style="width:400px;" />

<br>

## Operations

<p><hl>Binary Heap에서 수행할 수 있는 연산들</hl>은 다음과 같다.</p>

* **Top**: 가장 root node의 값을 찾음
* **Pop**: root에 있는 값을 제거
* **Push**: 어떤 임의의 값을 Heap의 특성을 깨지 않으면서 삽입

<br>

### Top

**■ 방법**

tree의 <hl>root node의 값에 접근</hl>한다.

<br>

### Pop

**■ 방법**

<img src="/files/2021-09-06-cs-algorithm-algo09/image-20210902222851134.png" alt="image-20210902222851134" style="width:900px;" />

1. Root Node의 값을 제거 (그 결과, Root Node의 값이 비게 됨)
2. Root Node의 자식 Nodes 중 더 작은 값을 Root Node로 Promotion (그 결과, 자식 Node의 값이 비게 됨)
3. 2번 과정을 반복

<br>

### Push

Heap에 어떤 값을 삽입하는 방법에는 크게 두 가지가 있다.

1. Leaf Node에 삽입하는 방법 (item이 적당한 위치를 찾을 때까지 올라가는 방법)
2. Root Node에 삽입하는 방법 (item이 적당한 위치를 찾을 때까지 내려가는 방법)

우리는 이 중 일반적으로 자주 사용되는 <hl>① Leaf Node에 삽입하는 방법</hl>에 대해 알아본다.

<br>

**■ 방법**

<img src="/files/2021-09-06-cs-algorithm-algo09/image-20210902224039160.png" alt="image-20210902224039160" style="width:700px;" />

1. 아무 Leaf Node에 item을 삽입한다.
2. Binary Heap의 조건을 만족할 때까지 해당 item을 Promotion한다. (부모와 자리를 바꿈)
3. 2번 과정을 반복하여 Binary Heap의 조건을 만족하도록 만든다.

<div class="callout">
    <b>Percolation(삼투압 작용)</b><br>
    Binary Min Heap에서 Push를 할 때, 규칙에 의해 Node가 아래로 내려오지는 못하고 위로만 올라가는 현상을 의미
    (혹은 위로 올라가지는 못하고 밑으로만 내려가는 현상을 의미)
</div>


<br>

<br>

# 힙 자료구조의 구현 및 복잡도

<p><hl>완전 이진트리 기반의 힙 자료구조</hl>의 구현 방법 및 <hl>각 연산의 평균/최악의 복잡도</hl>에 대해 학습한다.</p>

<br>

## Perfect Binary Trees

Min Heap을 구현할 때, Perfect Binary Tree에 가깝도록 유지하면, 보다 효율적인 구현이 가능해진다. 따라서 Binary Heap을 구현하기에 앞서, <hl>Perfect Binary Tree에 대한 개념</hl>을 알아본다.



**■ 정의**

<img src="/files/2021-09-06-cs-algorithm-algo09/image-20210902224913408.png" alt="image-20210902224913408" style="width:700px;" />

Binary Tree의 $\text{height}=h$일 때, <hl>모든 leaf nodes가 다음의 조건을 만족한다.</hl>

$$
\text{depth}=h
$$

<br>

**● 재귀적인 정의**

<img src="/files/2021-09-06-cs-algorithm-algo09/image-20210902225246461.png" alt="image-20210902225246461" style="width:600px;" />

* $h=0$인 경우: node가 1개만 있는 tree
* $h≥1$인 경우: sub-tree가 모두 perfect binary tree여야 함

<br>

**■ 특징**

Perfect Binary Tree의 $\text{height}=h$일 때, <hl>nodes의 총 개수 $n$</hl>은 다음과 같다.

$$
n=2^h-1
$$

∵ 1, 3, 7, 15, 31, 63, ...

<br>

## Complete Binary Trees

min heap을 perfect binary tree에 가깝게 구현하면 좋지만, 데이터가 삽입/삭제되는 과정에 의해 nodes의 개수를 언제나 $n=2^h-1$에 맞출 수 없다. 따라서 <hl>perfect binary tree에 가까운 Complete Binary Tree 형태로 min heap을 구현하는 대안</hl>을 떠올려볼 수 있다.

<img src="/files/2021-09-06-cs-algorithm-algo09/image-20210902225342423.png" alt="image-20210902225342423" style="width:300px;" />

<b>Complete Binary Tree</b>는 <hl>Perfect Binary Tree를 지향</hl>하는, Perfect Binary Tree로 변해가는 과정이라 생각할 수 있다. 즉, <hl>왼쪽부터 Leaf Node를 채워 나가며 Perfect Binary Tree가 되도록하는 Tree 형태</hl>를 Complete Binary Tree라고 한다.

<br>

**■ 특징**

<p><hl>Complete Binary Tree의 heigth $h$</hl>는 다음과 같다.</p>

$$
h=[\log(n)]
$$

<br>

### Operations

<p><hl>Complete Binary Tree의 형태를 최대한 유지하며 연산을 수행하는 방법</hl>에 대해 알아본다.</p>

#### Push

<img src="/files/2021-09-06-cs-algorithm-algo09/image-20210911221817909.png" style="width:400px;"/>

1. 가장 왼쪽에 위치한 leaf node의 sibling node의 위치에 item을 삽입한다.
2. item이 적당한 자리를 찾을 때 까지 promotion한다.

<br>

#### Pop

<img src="/files/2021-09-06-cs-algorithm-algo09/image-20210911222740861.png" alt="image-20210911222740861" style="width:400px;;" />

1. Root Node의 값을 제거 (그 결과, Root Node의 값이 비게 됨)
2. 가장 마지막 leaf node의 값을 root node 자리에 채워 넣음
3. 기존의 leaf node를 올바른 위치로 percolation down한다.

<br>

## Heap의 구현

<p><hl>Tree를 BFS order로 순회를 한 결과를 array에 채워나가는 방법으로 Heap를 구현</hl>한다.</p>

**● 예시**

<img src="/files/2021-09-06-cs-algorithm-algo09/image-20210911223837945.png" alt="image-20210911223837945" style="width:400px;" />

※ 편의상 array 0은 비웠다. (Trivial Cost)※

<br>

**■ 특징**

* <hl>자식 node에 접근하는 방법</hl>

  <img src="/files/2021-09-06-cs-algorithm-algo09/image-20210911223801704.png" alt="image-20210911223801704" style="width:400px;" />
  
  $$
  \begin{cases}
  	\text{(왼쪽 자식 node의 index)} = \text{(본인 index)}*2\\
  	\text{(오른쪽 자식 node의 index)} = \text{(본인 index)}*2 + 1
  \end{cases}
  $$

* <hl>부모 node에 접근하는 방법</hl>

  $$
  \text{(부모 node의 index)} = [\text{(본인 index)}/2]
  $$

<br>

### Operations

#### Search

※ $\text{Time Complexity}=Θ(1)$ ※

<br>

#### Pop

※ $\text{Time Complexity}=Θ(\lg(n))$ (∵ 최악의 경우, height만큼 올라갔다 height만큼 내려온다.) ※

<br>

#### Push

※ $\text{Time Complexity}=Θ(1) \sim Θ(\lg(n))$ (∵ 최선의 경우 움직이지 않고, 최악의 경우 heigth만큼 움직인다.)  ※

※ 평균적인 Time Complexity는 다음과 같다. ※

$$
\text{Time Complexity} = 
\frac{1}{n}\sum_{k=1}^{h}{(h-k)2^k} = 
\frac{2^{h+1}-h-2}{n} = 
\frac{n-h-1}{n} = Θ(1)
$$

따라서 <hl>Push는 평균적으로 Constant Time안에 수행</hl>된다.

<br>

#### Remove

* 해당 item이 존재하는지 여부를 판단하는 Time Complexity = $O(n)$

* 가장 큰 값을 갖는 item을 제거하는 Time Complexity = $O(n)$

  ※ 큰 item은 대부분 leaf level에 존재하므로 $\frac{n}{2}$만큼의 탐색 시간이 걸린다. ※


<br>

### Run-time Analysis

|        | Average     | Worst       |
| ------ | ----------- | ----------- |
| Search | $O(n)$      | $O(n)$      |
| Push   | $O(1)$      | $O(\lg(n))$ |
| Pop    | $O(\lg(n))$ | $O(\lg(n))$ |
| Remove | $O(n)$      | $O(n)$      |

<br>

<br>

# References

1. [집콕]인공지능을 위한 알고리즘과 자료구조: 이론, 코딩, 그리고 컴퓨팅 사고-비선형 자료구조, K-MOOC: [http://www.kmooc.kr/courses/course-v1:SKKUk+SKKU_46+2021_T1/courseware/1158909c057347478d7386a2b7e97214/e9d736660cad4f76bcb9a05974fe0bf2/1?activate_block_id=block-v1%3ASKKUk%2BSKKU_46%2B2021_T1%2Btype%40vertical%2Bblock%4052b62ff6da2147c9a8a65a6056681d91](http://www.kmooc.kr/courses/course-v1:SKKUk+SKKU_46+2021_T1/courseware/1158909c057347478d7386a2b7e97214/e9d736660cad4f76bcb9a05974fe0bf2/1?activate_block_id=block-v1%3ASKKUk%2BSKKU_46%2B2021_T1%2Btype%40vertical%2Bblock%4052b62ff6da2147c9a8a65a6056681d91)

<br>

<br>

