---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments
title: '[ALGO-05] Lect5. 함수의 점근적 분석'
excerpt: "[ALGO] 성균관대학교 허재필 교수님의 K-mooc 알고리즘 강의를 듣고 공부한 자료입니다."
date: 2021-08-22
last_modified_at: 2021-08-22
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

이번 시간에는 주어진 함수를 점근적으로 분석하는 방법에 대해 배운다.



# 함수의 점근적 분석법

함수의 <hl>점근적 분석(Asymptotic Analysis)</hl>과 이를 통한 함수들 사이의 <hl>점근적 대소 관계</hl>에 대해 학습한다.



## 배경

* 알고리즘을 직접 구현하여 비교하는 방식은 비용소모적이다.
* 점근적 분석법은 수학적으로 분석하여, <hl>두 알고리즘의 성능을 비교하기 위한 분석법</hl>이다.

<br>

**■ Motivation**: Linear Search vs. Binary Search

* **Linear Search** : Array에서 특정 값을 찾고자 할 때, <u>앞에서부터 순차적으로 테스트</u>하는 방법
* **Binary Search** : 정렬된 Array에서 특정 값을 찾고자 할 때, 중간값과 찾고자하는 값을 비교하여 <u>범위를 좁혀가며 테스트</u>하는 방법

<img class="center" src="/files/2021-08-22-cs-algorithm-algo05/image-20210821164918636.png" alt="image-20210821164918636" style="width:300px;" />

위 그래프는 Array의 사이즈에 대한 Searching Algorithm의 연산량 그래프이다. Array Size가 증가함에 따라 Linear Search 알고리즘에 비해 Binary Search 알고리즘의 연산량이 더 적은 것을 알 수 있다.

<br>

## 특징

### Quadratic Growth

<img class="center" src="/files/2021-08-22-cs-algorithm-algo05/image-20210821170237342.png" alt="image-20210821170237342" style="width:500px;" />

$f(n)=n^2$​, $g(n)=n^2-3n+2$​일 때, $N$​을 $(0,3)$​의 범위에서 관찰하면 $f(n)$​과 $g(n)$​ 그래프의 차이가 없지만,  $N$을 $(0,100)$​의 범위에서 관찰하면 두 그래프가 거의 비슷해진다. 

따라서 Asymptotic Analysis에서는 <hl><b>N이 무한히 커졌을 때</b> 두 알고리즘의 차이가 얼마나 심한지가 주요 관심사</hl>이자 핵심이다.

<br>

### Counting Instructions

<div class="quote-box">
	<hl>A보다 B 알고리즘이 더 느릴 때, 더 좋은 컴퓨터를 사용함으로써 B가 A보다 빨라질수도, 혹은 그렇지 않을 수도 있다.</hl>
</div>

**■ 두 알고리즘 모두 Leading Term이 $n^k$​인 경우** (Polynomial)

$$
f(n)=a_kn^k + a_{k-1}n^{k-1} + ...\\
g(n)=b_kn^k + b_{k-1}n^{k-1} + ...
$$

$M=\frac{a_k}{b_k}+1$일 때, n이 충분히 큰 경우 다음이 언제나 성립한다.

$$
f(n)<Mg(n)
$$

$f(n)$​​ 알고리즘을 구동시키는 컴퓨터보다 <u>$M$​​배 빠른 컴퓨터로 $g(n)$​​알고리즘을 구동시킨다면, $g(n)$​​알고리즘이 더 빠르게 동작</u>한다.

<br>

**■ 두 알고리즘의 승수가 다른 경우**

$$
f(n)=a·n^2\\
g(n)=b·n·\log{(n)}
$$

g(n)은 <u>절대 f(n)보다 빨라질 수 없다.</u>

<br>

### Weak Ordering

**■ Equivalent**

아래와 같이 f(n)과 g(n)의 비율이 상수로 수렴한다면, f와 g는 **동일한 복잡도**를 갖는다: **$f \sim g$​​.**

$$
\lim_{n→∞}{\frac{f(n)}{g(n)}}=c \ \ \ \ \text{where, 0<c<∞}
$$

<br>

**■ 대소관계**

아래와 같이 f(n)과 g(n)의 비율이 0에 수렴한다면, g의 복잡도가 더 크다: **$f < g$​​​​​.**

$$
\lim_{n→∞}{\frac{f(n)}{g(n)}}=0
$$

<br>

<br>

# 심볼 기반 함수의 점근적 바운드

함수의 점근적 바운드를 표현하는 <hl>5가지 심볼</hl>에 대해 이해한다.


## Notation

함수의 증가 양상을 다른 함수와의 비교로 표현하는 <hl>점근 표기법(Asymptotic Notation)은 대표적으로 다섯 가지 표기법</hl>이 있다.

* **Θ-Notation** (Theta Notation, 대문자 세타 표기법)
* **Big-O Notation** (Big O Notation, 대문자 O 표기법)
* **Big-Ω Notation** (Big Omega Notation, 대문자 오메가 표기법)
* **o Notation** (O Notation, 소문자 o 표기법)
* **ω Notation** (Omega Notation, 소문자 오메가 표기법)

### Θ-Notation

<img class="center" src="/files/2021-08-22-cs-algorithm-algo05/image-20210822124652179.png" alt="image-20210822124652179" style="width:300px;" />

**■ 정의**

$$
f(n)=Θ(g(n))\\
\text{$0≤c_1g(n)≤f(n)≤c_2g(n)$, for all $n≥n_0$}
$$

위 조건을 만족하는 $c_1, c_2$가 존재할 때, 다음과 같이 표현할 수 있다.

* $f(n)$: $g(n)$과 <hl>같은 증가 속도</hl>를 갖는다.
* $f(n)$: $Θ(g(n))$에 속한다.
* $g(n)$: $f(n)$에 대한 <hl>Aymptotically Tight Bound</hl>

<br>

**■ 예시**

$$
f(n)=Θ(g(n))\text{: a polynomial of degree k}\\
⇔ f(n)=Θ(n^k)
$$

* $\frac{1}{2}n^2 - 3n = Θ(n^2)$

  ∵ $\frac{1}{14}n^2 ≤ \frac{1}{2}n^2 - 3n ≤ n^2$, for all $n≥7$

* $an^2 + bn +c = Θ(n^2), a>0$​​​

  ∵ $\frac{a}{4}n^2 ≤ an^2 + bn +c ≤ \frac{7a}{4}n^2$, for all $n≥2\max{(\frac{│b│}{a}, \sqrt{\frac{│c│}{a}})}$

<br>

### Big-O Notation

<img class="center" src="/files/2021-08-22-cs-algorithm-algo05/image-20210822125417266.png" alt="image-20210822125417266" style="width:300px;" />

**■ 정의**

$$
f(n)=O(g(n))\\
\text{$0≤f(n)≤cg(n)$, for all $n≥n_0$}
$$

위 조건을 만족하는 양의 실수 $c, n_0$​​가 존재할 때, 다음과 같이 표현할 수 있다.

* $g(n)$: $f(n)$​​에 대한 <hl>Aymptotically Upper Bound</hl>

<br>

**■ 특징**

* $f(n)=Θ(g(n)) ⇒ f(n)=O(g(n))$​
* $O$: <hl>worst-case</hl> running time을 bound하기에 적합하다.

<br>

**■ 예시**

* $ 2n^2 + 8n - 2 = \text{$O(n^2)$, $O(n^3)$, $O(n^4)$, ...}$​​​​

<br>

### Big-Ω Notation

<img class="center" src="/files/2021-08-22-cs-algorithm-algo05/image-20210822125811504.png" alt="image-20210822125811504" style="width:300px;" />

**■ 정의**

$$
f(n)=Ω(g(n))\\
\text{$0≤cg(n)≤f(n)$, for all $n≥n_0$}
$$

위 조건을 만족하는 양의 실수 $c, n_0$​​가 존재할 때, 다음과 같이 표현할 수 있다.

* $g(n)$: $f(n)$​​에 대한 <hl>Aymptotically Lower Bound</hl>

<br>

**■ 특징**

* $f(n)=Θ(g(n)) ⇒ f(n)=Ω(g(n))$​​
* $Ω$: <hl>best-case</hl> running time을 bound하기에 적합하다.

<br>

### o-Notation

<img class="center" src="/files/2021-08-22-cs-algorithm-algo05/image-20210822135612432.png" alt="image-20210822135612432" style="width:300px;" />

**■정의**

$$
f(n)=o(g(n))\\
\text{$0≤f(n)≤cg(n)$, for all $n≥n_0$ and all constant $c>0$}
$$

위 조건을 만족하는 양의 실수 $c, n_0$​​가 존재할 때, 다음과 같이 표현할 수 있다.

* $g(n)$: $f(n)$​​​​​에 대한 <hl>Upper Bound이면서 Asymptotically Tight하지 않다</hl>.

  <gray>※ Asymptotically Tight하다는 것은, <b>하한과 상한 경계가 모두 존재</b>함을 의미한다. ※ </gray>

<br>

**■ 특징**

* Big-O Notation은 Aysmptotically Tight할 수도, 안 할 수도 있지만
  <hl>Little-o Notation은 언제나 Aysmptotically Tight하지 않다.</hl>

<br>

**■ 예시**

* $2n=O(n^2), o(n^2)$​​​​​
* $2n^2=O(n^2)$  but $2n^2 ≠o(n^2)$

<br>

### ω-Notation

<img class="center" src="/files/2021-08-22-cs-algorithm-algo05/image-20210822135529174.png" alt="image-20210822135529174" style="width:300px;" />

**■ 정의**

$$
f(n)=ω(g(n))\\
\text{$0≤f(n)≤cg(n)$, for all $n≥n_0$ and all constant $c>0$}
$$

위 조건을 만족하는 양의 실수 $c, n_0$​​가 존재할 때, 다음과 같이 표현할 수 있다.

* $g(n)$: $f(n)$에 대한 <hl>Lower Bound이면서 Asymptotically Tight하지 않다.</hl>

<br>

**■ 특징**

* Ω-Notation은 Aysmptotically Tight할 수도, 안 할 수도 있지만
  <hl>ω-Notation은 언제나 Aysmptotically Tight하지 않다.</hl>

**■ 예시**

* $2n^2=Ω(n), ω(n)$​​​
* $2n^2=Ω(n)$​​​  but $2n^2 ≠ω(n)$​​​

<br>

### 정리

<img class="center" src="/files/2021-08-22-cs-algorithm-algo05/image-20210822140700223.png" alt="image-20210822140700223" style="width:300px;" />

* 일반적으로는 tight boundary를 갖는 <b>Θ-Notation</b>과 worst-case를 가정하는 <b>Big-O Notation</b>을 주로 사용한다.

<br>

 **Notations Defined with Limit**

<img class="center" src="/files/2021-08-22-cs-algorithm-algo05/image-20210822142124620.png" alt="image-20210822142124620" style="width:500px;" />

$$
\begin{cases}
f(n)=o(g(n)) : \lim_{n→∞}{\frac{f(n)}{g(n)}} = 0\\
f(n)=O(g(n)) : \lim_{n→∞}{\frac{f(n)}{g(n)}} < ∞\\
f(n)=Θ(g(n)) : 0 < \lim_{n→∞}{\frac{f(n)}{g(n)}} < ∞\\
f(n)=Ω(g(n)) : 0 < \lim_{n→∞}{\frac{f(n)}{g(n)}}\\
f(n)=ω(g(n)) : \lim_{n→∞}{\frac{f(n)}{g(n)}} = ∞\\
\end{cases}
$$

<br>

**■ 특징**

* **Transitivity**

  $$
  \text{$f(n)=Θ(g(n))$ and $g(n)=Θ(h(n))$}\\
  \text{⇔ $f(n)=Θ(h(n))$ for Θ, O, o, Ω, ω}
  $$

* **Reflexivity**

  $$
  \text{$f(n)=Θ(f(n))$ for Θ, O, Ω}
  $$

* **Symmetry**

  $$
  \text{$f(n)=Θ(g(n))$ ⇔ $g(n)=Θ(f(n))$}
  $$

* **Transpose Symmetry**

  $$
  \text{$f(n)=Θ(g(n))$ ⇔ $g(n)=Ω(h(n))$}\\
  \text{$f(n)=o(g(n))$ ⇔ $g(n)=ω(h(n))$}
  $$

<br>

## Common Classes

일반적으로 자주 사용되는 몇몇 Notation은 다음과 같이 이름붙여진다.

* $Θ(1)$ : constant
* $Θ(\log(n))$​​ : logarithmic
* $Θ(n)$​​ : linear
* $Θ(n\log(n))$​ : n log n
* $Θ(n^2)$​​ : quadratic
* $Θ(n^3)$​​ : cubic
* $Θ(2^n, e^n, 4^n, ...)$​​ : exponential

<br>

**■ Weak Ordering**

<img class="center" src="/files/2021-08-22-cs-algorithm-algo05/image-20210822143409082.png" alt="image-20210822143409082" style="width:400px;" />

$$
Θ(1) < Θ(\log(n)) < Θ(n) < Θ(n\log(n)) < Θ(n^2) < Θ(n^3) < Θ(4^n)
$$

<br>

## 응용

* n개의 floating point value들이 있는 list를 합하는 경우:

  n번의 operation이 필요하므로 $Θ(n)$ algorithm

* run-time이 $O(n^d)$인 경우, polynomial time complexity를 갖는다고 말할 수 있다.

  <gray>※ 일반적으로, polynomial time complexity를 가지면 Efficient(효율적)이다. ※</gray>

* polynomial-time을 갖지 않는다고 알려진 (혹은 모르는) algorithms을 Intractable하다고 한다.

  <gray>e.g., : Traveling salesman problem(TSP)는 아직 polynomial-tim을 갖는 알고리즘이 알려지지 않았다. 지금까지 알려진 best algorithm은 $Θ(n^2 2^n)$이다.</gray>

<br>

# References

1. [집콕]인공지능을 위한 알고리즘과 자료구조: 이론, 코딩, 그리고 컴퓨팅 사고-비선형 자료구조, K-MOOC: [http://www.kmooc.kr/courses/course-v1:SKKUk+SKKU_46+2021_T1/courseware/1158909c057347478d7386a2b7e97214/e9d736660cad4f76bcb9a05974fe0bf2/1?activate_block_id=block-v1%3ASKKUk%2BSKKU_46%2B2021_T1%2Btype%40vertical%2Bblock%4052b62ff6da2147c9a8a65a6056681d91](http://www.kmooc.kr/courses/course-v1:SKKUk+SKKU_46+2021_T1/courseware/1158909c057347478d7386a2b7e97214/e9d736660cad4f76bcb9a05974fe0bf2/1?activate_block_id=block-v1%3ASKKUk%2BSKKU_46%2B2021_T1%2Btype%40vertical%2Bblock%4052b62ff6da2147c9a8a65a6056681d91)


<br>
<br>