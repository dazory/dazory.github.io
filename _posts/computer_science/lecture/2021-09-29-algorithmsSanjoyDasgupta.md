---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments
title: '[SanjoyDasgupta] Ch2. Divide-and-conquer algorithms'
excerpt: "Sanjoy Dasgupta가 집필한 Algorithms 교재를 공부한 내용입니다."
date: 2021-09-29
last_modified_at: 2021-09-29
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

# Ch2. Divide-and-conquer algorithms

**■ Divide-and-conquer:**

1. problem을 sub-problems으로 나눈다.
2. 재귀적으로 sub-problems을 해결한다.
3. answers를 모두 combining한다.

<br>

## 2.1. Multiplication

Divide-and-conquer 접근 방법이 어떠한 이점을 제공하는지 몇 가지 예시를 통해 살펴본다.

**■ 예시**  📢이어서 더 자세히 다루도록 한다.

* Multiplication
* Search
  * Binary Search
* Sort
  * Merge Sort

<br>

### The product of two complex numbers (a.k.a. Gauss's Trick)

$$
(a+bi)(c+di) = ac -bd +(bc+ad)i
$$

위 방정식은 4개의 곱($ac$, $bd$, $bc$, $ad$)으로 구성되어있지만, divide-and-conquer 방식으로 접근했을 때 아래와 같이 3개의 곱($ac$, $bd$, $(a+b)(c+d)$)으로 곱셈연산 횟수를 줄일 수 있다.

$$
(a+bi)(c+di) = ac -bd +\{ (a+b)(c+d) -ac -bd\}i\\
\text{(∵ (bc+ad) = (a+b)(c+d) -ac -bd )}
$$

big-O 관점에서 위와 같은 곱셈 연산 횟수 감소는 독창성 낭비라고 보여지지만, <u>재귀적인 관점</u>에서 불필요한 곱셈의 수를 줄이는 것은 매우 중요하다.

<br>

### Regular multiplication (정규곱셈)

$x$와 $y$가 두 개의 $n$-bit 정수이며 이때 $n$은 2의 제곱수라고 가정하자. divide-and-conquer하게 $x$와 $y$의 곱을 표현하고자 각각을 sub-problem으로 쪼갠 결과는 아래와 같다.

$$
x = 2^{n/2}·x_L + x_R\\
y = 2^{n/2}·y_L + y_R
$$

이를 이용하여 $x$와 $y$의 곱 $xy$를 표현하면 다음과 같다.

$$
xy = (2^{n/2}·x_L+x_R)(2^{n/2}·y_L+y_R)\\
= 2^{n}·x_Ly_L + 2^{n/2}(x_Ly_R +x_Ry_L) + x_Ry_R
$$

여기서 $x_Ly_L$, $x_Ly_R$, $x_Ry_L$, $x_Ry_R$은 재귀적으로 다시 호출될 수 있다. (아래 수도 알고리즘 참고)

```pseudocode
A divide-and-conquer algorithm for integer multiplication.

function multiply(x; y)
    Input: Positive integers x and y, in binary
    Output: Their product
    
    n = max(size of x, size of y)
    if n = 1: return xy
    
    xL, xR = leftmost [n/2], rightmost [n/2] bits of x
    yL, yR = leftmost [n/2], rightmost [n/2] bits of y
    P1 = multiply(xL; yL)
    P2 = multiply(xR; yR)
    P3 = multiply(xL + xR; yL + yR)

	return P1×2n + (P3-P1-P2)×2^{n/2} + P2
```

따라서 $n$-bit inputs에 대한 전반적인 running time $T(n)$은 다음과 같다.

$$
T(n) = 4T(n/2) + O(n)\\
∴ \text{complexity} = O(n^2)
$$

running time = $O(n^2)$은 전통적인 접근방법($O(n^2)$)과 비교했을 때 나아진 점이 없다. 따라서 앞서 배운 Gauss's trick(두 개의 복소수 곱)을 $xy$에 적용하면, 세 개의 곱($x_Ly_L$, $x_Ry_R$, $(x_L+x_R)(y_L+y_R)$)으로 표현될 수 있다.


<div class="callout">
    <b>[참고] Gauss's trick</b><br>
    (a+bi)(c+di)는 세 개의 곱(ac, bd, (a+b)(c+d))으로 표현된다.
</div>

<br>

따라서 다음과 같이 running time을 줄일 수 있다.

$$
T(n) = 3T(n/2) + O(n)\\
\text{∴ complexity = $O(n^{1.59})$}
$$

<br>

<br>

## 2.2. Recurrence relations

Divide-and-conquer 알고리즘은 일반적인 패턴을 갖는다. 이를 공식화해보자.

### 복잡도 공식화

**가정**

* $\text{size}=n$인 problem을
* $\text{size}=n/b$인 $a$개의 subproblems으로 재귀적으로 해결한다.
* 이때 추가적으로 발생하는 $\text{time}=O(n^d)$

$$
T(n) = a·T(n/b)+O(n^d)
$$

running time이 위와 같을 때 아래와 같이 overall running time을 계산할 수 있다.

$$
T(n) 
= a·T(\frac{n}{b})+O(n^d)\\
= a^2·T(\frac{n}{b^2})+\{ a·O({(\frac{n}{b})}^d) + O(n^d) \}\\
= a^3·T(\frac{n}{b^3})+\{ a^2·O({(\frac{n}{b^2})}^d) + a·O({(\frac{n}{b})}^d) + O(n^d) \}\\
...\\
= a^k·T(\frac{n}{b^k})+{\sum_{i=0}^{k-1}\{a^i·O((\frac{n}{b^i})^d)\}}
$$

이때, $b^k=n$ 즉, $k=\log_b(n)$일 때 위 방정식을 $T(1)$에 관해 표현할 수 있다.

$$
T(n)
= a^{\log_b(n)}·T(\frac{n}{b^{\log_b(n)}})
	+{\sum_{i=0}^{\log_b(n)-1}\{a^i·O((\frac{n}{b^i})^d)\}} \\
= a^{\log_b(n)}·T(1)
	+{(\sum_{i=0}^{\log_b(n)-1}(\frac{a}{b^d})^i)·O(n^d)} \\
= a^{\log_b(n)}·T(1)
	+{(\frac{ 1·( \frac{a}{b^d}^{\log_b(n)}-1 ) } { \frac{a}{b^d}-1 })·O(n^d)} \\
= n^{\log_b(a)}·T(1)
	+{(\frac{ 1·( n^{(\log_b(a)-d)}-1 ) } { \frac{a}{b^d}-1 })·O(n^d)} \text{(∵ 로그의 성질)}
$$

이때, 우항의 두 번째 term은 다음과 같이 쓸 수 있다.

$$
{(\frac{ 1·( n^{(\log_b(a)-d)}-1 ) } { \frac{a}{b^d}-1 })·O(n^d)}\\
= 
\begin{cases}
	O({ n^{(\log_b(a)-d)} }) · O(n^d) & \text{if $\log_b(a) > d$} \\
	\log_b(n) · O(n^d) & \text{if $\log_b(a) = d$} \\
	O(c) · O(n^d) & \text{if $\log_b(a) < d$} \\
\end{cases}\\
= 
\begin{cases}
	O( n^{(\log_b(a))} ) & \text{if $\log_b(a) > d$} \\
	O(n^d\log(n)) & \text{if $\log_b(a) = d$} \\
	O(n^d) & \text{if $\log_b(a) < d$} \\
\end{cases}\\
$$

따라서 최종적인 overall running time은 다음과 같다.

$$
T(n)
= 
\begin{cases}
	O( n^{\log_b(a)} + n^{\log_b(a)} ) & \text{if $\log_b(a) > d$} \\
	O( n^{\log_b(a)} + n^d\log(n)) & \text{if $\log_b(a) = d$} \\
	O( n^{\log_b(a)} + n^d) & \text{if $\log_b(a) < d$} \\
\end{cases}\\
= 
\begin{cases}
	O( n^{\log_b(a)} ) & \text{if $\log_b(a) > d$} \\
	O( n^d\log(n)) & \text{if $\log_b(a) = d$} \\
	O( n^d) & \text{if $\log_b(a) < d$} \\
\end{cases}\\
$$

<br>

재귀적 호출을 tree구조로 해석하면 다음과 같이도 이해할 수 있다.

<img src="/files/Untitled/image-20210907182424920.png" alt="image-20210907182424920" style="width:800px;" />

overall running time $T(n)$을 depth=$k$일 때의 time $T(\frac{n}{b^k})$로 나타내면 다음과 같이 쓸 수 있다.<br>(해석하자면, $T(\frac{n}{b^k})$짜리 nodes가 $a^k$만큼 있고, 여기에 초기 $O(n^d)$ complexity를 더한다.)

$$
T(n) = a^k × T(\frac{n}{b^k}) + O(n^d)
$$

(예시를 통해 재검토)

* $T(n) = 4T(n/2) + O(n)$일 때, (즉 $a=4, b=2, d=1$)
  
  $$
  T(n) = O( n^{\log_2(4)} ) = O(n^2) \ \ \text{(∵ $\log_2(4)>1$이므로 )}
  $$

* $T(n) = 3T(n/2) + O(n)$일 때, (즉 $a=3, b=2, d=1$)
  
  $$
  T(n) = O( n^{\log_2(3)} ) = O(n^{1.59}) \ \ \text{(∵ $\log_2(3)>1$이므로 )}
  $$

<br>

### Binary Search

divide-and-conquer 알고리즘은 궁극적으로 binary search라고 해석할 수 있다. 

**■ 예시**: 정렬된 keys $z[0, 1, ..., n-1]$를 포함하는 하나의 큰 파일에서 key값 $k$를 찾는 문제

Binary Search를 통해 이러한 문제를 해결하는 과정은 다음과 같다.

**● 수도 알고리즘**

```pseudocode
Binary_Search(z, 0, n-1, k):
	if(k==z[n/2]) { result=z[n/2]; }
	else if(k<z[n/2]) { Binary_Search(z, 0, n/2-1, k); }
	else if(k>z[n/2]) { Binary_Search(z, n/2, n-1, k); }
```

<br>

**● 복잡도**

overall running time $T(n)$은 다음과 같다.

$$
T(n) 
= T([n/2]) + O(1) & \text{즉, $a=1, b=2, d=0$}\\
= O(n^d\log(n)) = O(\log(n)) & \text{(∵ $\log_2(1) = 0$)}
$$

<div class="callout">
    <b>내 생각</b><br>
    결국, Binary Search는 주어진 elements를 2개로 나누며, 각 search에 O(1)의 시간이 걸리는것이다.<br>
    이를 divide-and-conquer로 확장시켜 생각한다면 주어진 elements를 몇 개로 쪼갤지(a), 이렇게 쪼개면 각 단위의 elements의 size는 어떻게 되는지(b), 또한 각 단위를 처리하는데에 얼만큼의 시간이 소요되는지(d)와 관련시킬 수 있을 것 같다.
</div>


<br>

<br>

## 2.3. Mergesort

Divide-and-conquer 알고리즘은 숫자들의 list를 정렬하는 문제에 쉽게 적용 가능하다. 가령 merge sort는 list를 절반씩 두 개로 쪼개고, 재귀적으로 각 절반씩에 대해 정렬을 하며, 그 결과로 반환된 sorted sublists를 merge(병합)한다.

<br>

### 알고리즘

mergesort의 알고리즘은 다음과 같다.

```pseudocode
function mergesort(a[1...n]):
	* Input:	An array of numbers a[1...n]
	* Output:	A sorted version of this array

	if n>1:
		return merge( mergesort(a[1..⌊n/2⌋)), mergesort(a[⌊n/2⌋+1...n]))
	else:
		return a
```

* input list의 element 개수가 2 이상이면, 절반씩 mergesort()한 뒤에 merge() 결과를 반환
* input list의 element 개수가 1 이하이면, input list를 그대로 반환

이때 merge 함수는 다음과 같다.

```pseudocode
function merge(x[1...k], y[1...l]):
	if k=0:	return y[1...l]
	if l=0:	return x[1...k]
	if x[1]≤y[1]:
		return x[1] & merge( x[2...k], y[1...l] )
	else:
		return y[1] & merge( x[1...k], y[2...l] )
```

* 두 input list 중 비어있는 list가 있다면, 비어있지 않은 다른 input list를 반환
* 두 input list가 모두 비어있지 않다면,
  * 두 input list 각각의 최소값(첫 번째 인덱스값)을 비교하여 적절히 concatenate and merge

※ 이때 operator &는 concatenation을 의미한다. ※

<br>

### 복잡도

mergesort의 overall running time $T(n)$은, mergesort의 재귀호출 시간 $2T(n/2)$와 merge의 시간복잡도 $O(n)$의 합으로 나타낼 수 있다.

※ 길이가 각각 $k$, $l$인 두 lists를 합치는 merge 알고리즘의 running time은 $O(k+l)=O(n)$. ※

$$
T(n) = 2T(n/2) + O(n) \\
= O(n^d\log(n)) \ \ \  \text{(∵$\log_2(2)==1$)} \\
= O(n\log(n)) \ \ \  \text{(∵ $a=2, b=2, c=1$)}
$$

즉, $T(n) = O(n\log(n))$.

<br>

### iteractive mergesort

위에서 언급한 알고리즘을 그림으로 도식화한 결과는 아래와 같다.

![image-20210908155859778](/files/2021-09-29-cs-algorithm-algorithmsSanjoyDasgupta/image-20210908155859778.png)

사실상 모든 과정이 merging만으로 수행됨을 확인할 수 있다. 이러한 관점에서 봤을 때 mergesort는 재귀적인 방법(recursive)이 아닌 반복적인 방법(iteractive)을 이용해볼 수 있다.

```pseudocode
function iterative-mergesort( a[1...n] )
	* Input:	elements a1, a2, ..., an to be sorted
	
	Q = [] (empty queue)
	for i=1 to n:
		inject( Q, [ai] )
		while │Q│>1:
			inject( Q, merge( eject(Q), eject(Q) ) )
	return eject(Q)
```

1. 비어있는 queue Q를 생성

2. 주어진 input list a[1...n]의 element a1부터 an까지에 대해 아래를 수행:

   Q의 elements 개수가 2개 이상이면 아래를 반복:

   2.1. Q를 두 번 eject하여 두 elements를 꺼내고 merge를 수행

   2.2. merge의 결과를 Q에 inject ※ 이때 두 elements가 merge되어 하나의 element가 된다. ※

3. Q의 element를 반환

※ inject: queue의 마지막에 새로운 원소 추가 (enqueue) ※<br>※ eject: queue의 시작 원소를 제거하고 반환 (dequeue) ※<br>※ queue의 구조: FIFO(First-In-First-Out) ※

<div class="callout">
    <b>재귀(recursive)와 반복(iteractive)</b><br>
    <b>재귀</b>는 적은 코드로 알고리즘을 작성할 수 있기 때문에 코드를 이해하고 유지하는 것이 중요한 경우에 주로 사용된다. function call에 따른 메모리 비용 및 CPU 사용 시간이 더 소요된다는 단점이 있다.<br>
    <b>반복</b>은 function call에 따른 메모리 비용이 없기 때문에 (즉, method 호출을 위한 메모리는 한 번만 필요하기 때문에) 성능 측면에서 더 유리하다.
</div>

<br>

위 알고리즘을 도식화 하면 다음과 같다.

![image-20210908165655430](/files/2021-09-29-cs-algorithm-algorithmsSanjoyDasgupta/image-20210908165655430.png)

<br>

### An $n\log(n)$ lower bound for sorting

Sorting algorithms은 trees로 묘사될 수 있다. 가령 3개의 elements $a_1, a_2, a_3$를 갖는 하나의 array의 정렬 과정은 다음과 같다.

![image-20210908170206501](/files/2021-09-29-cs-algorithm-algorithmsSanjoyDasgupta/image-20210908170206501.png)

이때 tree의 depth는 알고리즘의 the worst-case time complexity를 의미한다. 따라서 tree구조로 나타내면 알고리즘이 얼마나 최적화되었는지를 보여줄 수 있다. 

<br>

**■ $n$-elements sorting algorithms**

가령, $n$개의 elements에 대해 정렬알고리즘을 사용할 때 어떠한 비교 sequence에 의해 정렬된 결과 (e.g., $\{a_1, a_2, ..., a_n\}$)는 tree의 leaves에 저장된다. $\text{depth}=d$일 때 nodes의 개수는 $2^d$이므로, leaves의 총 개수는 $2^h$(h=height)이다. 한편, 가능한 모든 정렬의 개수는 $n!$이므로 다음과 같이 height를 표현할 수 있다.

$$
h 
= \log(n!) \ \ \  \text{(∵ $2^h=n!$)} \\
≥ c·n\log(n) \ \ \  \text{for all c≥0 (∵ $n!≥(\frac{n}{2})^{\frac{n}{2}})$}\\
∴ h=Ω(n\log(n))
$$

위 결과에 의해 $n$개의 elements를 정렬하는 알고리즘의 worst-case는 $Ω(n\log(n))$에 비교되어야 함을 알 수 있다.

<br>

**● 예시**

mergesort의 overall time complexity는 $O(n\log(n))$으로, 위에서 구한 worst-case time complexity $Ω(n\log(n))$과 일치한다. 즉, 적어도 정렬알고리즘의 worst-case time complexity는 $Ω(n\log(n))$보다 빠를 수 없는데, mergesort가 이를 충족한다.

<br>

<br>

## 2.4. Medians

### Get Medians with sorting

중앙값을 찾는 가장 간단한 방법은 단순히 sorting하여 중앙값을 반환하는 것이다. 하지만 이러한 방식은 단점이 명확하다.

* too slow : $O(n\log(n))$의 time complexity가 소요된다.
* too much : 중앙값을 찾기 위해 중앙값이 아닌 모든 요소를 정렬해야한다.

<br>

### Get Medians with Selection

selection을 통해 sorting보다 더 효율적으로 median값을 찾을 수 있다.

<br>

**■ Selection**

```pseudocode
SELECTION
	Input:	A list of numbers S; an integer k
	Output: The kth smallest element of S
```

※ 이때 $k=1$은 $S$에서 최소값을 의미하며, $k=⌊│S│/2⌋$는 $S$에서 median을 의미한다. ※

<br>

**■ A randomized divide-and-conquer algorithm for selection**

selection을 위해 divide-and-conquer 접근 방법을 사용해볼 수 있다. 

**● 예시**: 어떤 값 $v$에 대해 list $S$를 세 개의 분류로 쪼개는 방법

<img src="/files/2021-09-29-cs-algorithm-algorithmsSanjoyDasgupta/image-20210908184709360.png" alt="image-20210908184709360" style="zoom:80%;" />

$v=5$일때 주어진 list $S$는 다음과 같이 쪼개질 수 있다.

![image-20210908184836478](/files/2021-09-29-cs-algorithm-algorithmsSanjoyDasgupta/image-20210908184836478.png)

※ $S_L, S_v, S_R$ : 각각 $S$보다 작은값들, 같은 값들, 큰 값들의 집합 ※

이때 median값을 search하는 과정은 이러한 sublists 중 하나로 범위를 좁혀내려가며 얻어질 수 있다. 가령, $S$ 중 8번째로 작은 값은 $S_R$에서 세 번째로 작은 값임을 알 수 있다 ($S_L$과 $S_v$의 elements 개수가 5개이므로 8번째로 작은 값은 $S_R$에 속할 수 밖에 없다).

이를 일반화하면 다음과 같다.

$$
\text{selection}(S,k)
=
\begin{cases}
	\text{selection}(S_L, k) & \text{if $k≤ │S_L│$} \\
	v & \text{if $│S_L│<k≤│S_L│+│S_v│$} \\
	\text{selection}(S_R, k-│S_L│-│S_v│) & \text{if $k>│S_L│+│S_v│$}
\end{cases}
$$

<br>

**■ 복잡도**

이때 세 개의 sublists는 linear time안에 계산된다 (더욱이, 계산은 in-place로 동작하며 추가적인 memory allocating이 필요없다👍). 따라서 적절하게 sublist를 선택한다면 계산해야하는 elements의 개수를 $│S│$에서 적어도 $\max{(│S_L│, │S_R│)}$만큼은 감소시킬 수 있다.

이상적인 상황($│S_L│, │S_R│ ≒ \frac{1}{2}│S│$)을 가정하면, selection의 overall running time은 다음과 같다.

$$
T(n) = T(n/2)+O(n)\\
∴ T(n) = O(n)
$$

※ 단, 위 복잡도는 이상적인 상황을 가정하였음에 유의해야한다. ※

<br>

**● $v$에 따른 복잡도**

알고리즘의 복잡도는 결국 $v$값에 따라 달라진다.

* Best-case: $│S_L│, │S_R│ ≒ \frac{1}{2}│S│$
  
  $$
  T(n) = T(n/2) + O(n)\\
  ∴ T(n) = O(n)
  $$

* Worst-case: $v$가 매번 최대값 혹은 최소값인 경우
  
  $$
  T(n) = \sum_{i=\frac{n}{2}}^{n}i = O(n^2)
  $$

* Average-case: 

  $S(25\%) ≤ v ≤ S(75\%)$라면( $S$의 50% 부분에 해당), $│S_L│, │S_R│ ≤ \frac{3}{4}│S│$가 된다.

  이때 *Lemma*에 의해  2번 split operation을 수행하면 평균적으로 전체 risk가 최대 $\frac{3}{4}$ 크기로 감소한다.

  <div class="callout">
      <b>Lemma란?</b><br>
      On average a fair coin needs to be tossed two times before a "heads" is seen.<br>
  	즉, 동전이 앞면이기 위해서는 평균적으로 2번 던져야 한다.
  </div>

<br>

  따라서 Average-case의 time complexity는 다음과 같다.
  
  $$
  T(n) ≤ T(\frac{3}{4}n) + O(2n)\\
  ∴T(n) = O(n)
  $$

<br>

### The Unix sort command

**■ Mergesort vs. Median-finding** : 서로 다른 성질

* mergesort : 일단 생각없이 쪼개고, 이후에 열심히 정렬하면서 합침 (the most convenient way)
* median-finding : 처음부터 신중하게 쪼개어 나감

<br>

**■ Quicksort ≈ Median-finding** : 

Quicksort는 median-finding 알고리즘의 방식과 유사하게 동작한다. median algorithm의 $v$ 대신 $\text{pivot}$이라는 변수를 도입하여, $\text{pivot}$을 기준으로 list를 쪼갠다. 이후 sublists 각각에 대해 정렬을 반복하는 과정을 거친다.

**● Time Complexity**

Quicksort의 time complexity는 다음과 같다.

* Worst-case : $Θ(n^2)$ (median-finding과 동일)
* Average-case : $O(n\log(n))$ (≠median-finding $O(n^2)$)

Quicksort는 sorting algorithms 중에서 가장 빠른 알고리즘이다.

<br>

<br>

## 2.5. Matrix Multiplication

두 개의 $n×n$ matrices $X$와 $Y$의 곱은 $n×n$ matrix $Z=XY$로 표현된다.

![image-20210908221921228](/files/2021-09-29-cs-algorithm-algorithmsSanjoyDasgupta/image-20210908221921228.png)

$$
Z_{ij} = \sum_{k=1}^n{X_{ik}Y_{kj}}
$$

※ 일반적으로 $XY≠YX$이다. 즉, matrix multiplication은 commutative하지 않다. ※

<br>

**■ 배경**

꽤 오랜시간동안 matrix multiplication은 $O(n^3)$의 복잡도를 갖는다고 여겨졌다 (∵ $n^2$개의 entries가 계산되고 각 계산은 $O(n)$이 소요되기 때문). 하지만 독일 수학자 Volker Strassen에 의해 divide-and-conquer 알고리즘을 적용하면 더 효율적인 알고리즘으로 만들 수 있다는 것이 증명되었다.

<br>

### Volker Strassen's Idea

**■ 이전까지의 생각**

$X$와 $Y$를 각각 4개의 $\frac{n}{2}×\frac{n}{2}$ blocks으로 쪼개면 다음과 같이 표현 가능하다.

$$
X=
\begin{bmatrix}
	A & B\\
	C & D
\end{bmatrix}
,
Y=
\begin{bmatrix}
	E & F\\
	G & H
\end{bmatrix}\\

∴ XY = 
\begin{bmatrix}
	AE+BG & AF+BH\\
	CE+DG & CF+DH
\end{bmatrix}
$$

이때의 시간 복잡도는 다음과 같다.

$$
T(n) = 8T(\frac{n}{2}) + O(n^2)\\
∴ T(n) = O(n^3)\\

\text{(∵ $\frac{n}{2}$의 크기를 갖는 8개의 submatrix 곱과 $O(n^2)$ 복잡도를 갖는 덧셈연산)}
$$

<br>

**■ Volker Strassen의 생각**

Volker는 $n$ 크기 matrix를 7개의 subproblems으로 쪼갬으로써 복잡도를 줄였다.

$$
T(n) = 7T(\frac{n}{2}) + O(n^2)\\
∴ T(n) = O(n^{\log_2{7}}) ≈ O(n^{2.81})
$$

▶ 자세히: 7개의 subproblems으로 쪼개는 방법

$$
XY = 
\begin{bmatrix}
	P_5 + P_4 - P_2 + P_6 & P_1 + P_2\\
	P_3 + P_4 & P_1 + P_5 - P_3 - P_7
\end{bmatrix}
$$

이때 $P_1 = A(F-H), P_2 = (A+B)H, P_3 = (C+D)E, P_4 = D(G-E)$, $P_5 = (A+D)(E+H), P_6 = (B-D)(G+H), P_7 = (A-C)(E+F)$ .

<br>

<br>

## 2.6. The fast Fourier transform

지금까지 integers와 matrices에 대해 divide-and-conquer 알고리즘이 얼마나 유익한지를 살펴보았다. 이번에는 polynomials에 대해 divide-and-conquer 알고리즘을 적용해보자.

### Polynomials with Divide-and-conquer

두 degree-$d$ polynomials $A(x), B(x)$의 곱은 degree-$2d$ polynomial $C(x)$로 표현된다.

$$
A(x) = a_0 + a_1x + ... + a_dx^d\\
B(x) = b_0 + b_1x + ... + b_dx^d\\
C(x)=A(x)·B(x) = c_0 + c_1x + ... + C_{2d}x^{2d}\\
\text{이때 $c_k = a_0b_k + a_1b_{k-1} + ... + a_kb_0 = \sum_{i=0}^k{a_ib_{k-i}}$}
$$

※ for $i>d$, take $a_i$ and $b_i$ to be zero. ※

이때, $c_k$를 계산하기 위해서는 $O(k)$단계가 소요되므로 $2d+1$개의 coefficients($c_0, c_1, ... c_{2d}$)를 계산하기 위해서는 $Θ(d^2)$의 시간이 소요된다.

<br>

### Fast Fourier Transform

Fast fourier transform을 이용하면 더 빠른 계산을 수행할 수 있다. 자세한 과정은 생략한다.

<br>

<br>

## Exercise

(추후 추가 예정)


<br>

<br>

# References

1. eherbold/berkeleytextbooks, github: [https://github.com/eherbold/berkeleytextbooks](https://github.com/eherbold/berkeleytextbooks)

<br>

<br>  





