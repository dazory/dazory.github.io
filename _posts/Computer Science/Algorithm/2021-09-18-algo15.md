---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments
title: '[ALGO-15] Lect15. 동적계획법과 분할정복법의 차이 및 예제'
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
이번 시간에는 <hl>동적계획법과 분할정복법의 차이</hl>에 대해 이해하고, 예제 문제를 해결해본다.

<br>

# 동적계획법의 방법론 이해

<p><hl>동적계획법이 필요한 문제의 특성</hl> 및 <hl>분할정복법과의 차이점</hl>에 대해 학습한다.</p>

<br>

## Fibonacci Numbers

다이나믹 프로그래밍에 대해 이야기하기에 앞서 <hl>피보나치 수열</hl>에 대해 알아본다.

**■ Algorithm**

```c++
long long F(long long n)
{
	if(n<=1) return 1;
    return F(n-1)+F(n-2);
}
```

**● 예시**

1 → 1 → 2 → 3 → 5 → 8 → 13 → ...

<br>

■ Run-time

$$
T(n)=
\begin{cases}
	Θ(1) & \text{$n≤1$}\\
	T(n-1)+T(n-2)+Θ(1) & \text{$n>1$}
\end{cases}
$$

이러한 <hl>run time은 피보나치 수열만큼 증가함</hl>을 알 수 있다. 그 이유는 피보나치 함수 $F(50)$을 계산하기 위해 각 n에 대해 다음과 같이 반복적으로 호출되기 때문이다.

$$
F(45): \text{8번 호출}\\
F(40): \text{89번 호출}\\
F(30): \text{10,946번 호출}\\
F(20): \text{1,346,269번 호출}\\
F(10): \text{165,580,141번 호출}\\
F(1): \text{12,586,269,025번 호출}
$$

이러한 문제를 해결하기위한 방법으로 이전에 이미 구했던 결과를 저장하는 방법을 생각해볼 수 있다. 이러한 과정을 <i><b>MemorizationM</b></i>이라고 부른다. 

<br>

### Fibonacci with Memorization

<p><hl>Memorization을 사용하는 피보나치 함수</hl>는 다음과 같이 프로그래밍될 수 있다.</p>

```c++
long long F(long long n, bool isFirstCall = false)
{
    static long long *memo;
    if(isFirstCall)
    {
        if(memo!=NULL){delete[] memo;}
        if(n!=0)
        {
            memo = new long long[n+1];
            for(int i=0; i<n+1; i++){ memo[i]=0; }
        }
    }
    if(n<=1) return 1;
	if(memo[n]==0){ memo[n] = F(n-1) + F(n-2); }
    return memo[n];
}
```

<gray>※ global variable을 사용하는 것 보단 static을 사용하는 코딩 스타일이 더 좋다. ※</gray>

<br>

**● 알고리즘 해석**

1. `memo` static variable을 생성한다.

2. 처음 호출되는 경우 (메모리 할당이 필요):

   1. `memo`에 값이 있는 경우: `memo` static 변수를 모두 삭제한다.

   2. `n!=0`인 경우(계산이 필요):

      `memo` 변수의 크기를 +1 늘려준 뒤, `memo` array의 모든 값을 0으로 초기화한다.

3. `n<=1`인 경우: (계산 할 것도 없이) 1을 반환한다.

4. `memo[n]==0`인 경우(초기화된 직후): `memo[n]`의 위치에 `F(n-1)+F(n-2)`의 결과를 저장한다.

5. 최종적으로 `memo[n]`을 반환한다.

<br>

#### Top-down and Bottom-up Algorithms

**Top-down approach**: F(50)→F(49)→....

※ 대부분의 Divide-and-conquer algorithm이 Top-down 방식으로 수행된다. ※

<br>

**Bottom-up approach**: F(1)→F(2)→...

피보나치 수열 문제는 Top-down이 아닌 Bottom-up 방식으로도 해결할 수 있다.

e.g., Merge Sort: 미리 적당한 사이즈로 쪼개놓고, 위로 가는 방향

<br>

**● Fibonacci wiht Bottom-up approach**

```c++
long long F(long long n)
{
    if(n<=1){ return 1; }
    long long ret=1, prev=1;
    for(long long i=2; i<=n; i++)
    {
        ret = ret+prev;
        prev = ret-prev;
    }
    return ret;
}
```

※ 코드가 상당히 간단하다. ※

<br>

## Dynamic Programming

반복적으로 활용되는 <hl>sub-problem을 저장하여 재활용하여 문제를 해결하는 방법</hl>이 <i><b>다이나믹 프로그래밍</b></i>이다. 이때 주로 <hl>Memorization 방식으로 속도를 향상</hl>시킨다.

<br>
**■ Divide-and-conquer와의 차이점**

Divide-and-conquer: <hl>sub-problem간에 overlap이 거의 없다.</hl>

e.g., Merge Sort: 양쪽으로 나눈 array간에 공통적인 부분이 없다. 즉, 재활용할 여지가 없다.

Dynamic Programming: <hl>sub-problem에 overlap이 있다.</hl>

e.g., 피보나치 넘버: F(10)과같이 중복 문제가 계속 발생한다.

<gray>※ 이러한 중복 문제를 overlapping sub-problem이라고 한다. ※</gray>

<gray>※ overlapping sub-problem의 값을 미리 저장하고 재활용하는 것이 dynamic programming의 핵심이다. ※</gray>

<br>

<br>

# 행렬 체인 곱셈 문제의 동적계획법 기반 해결

<p><hl>Dynamic Programming으로 matrix multiplication하는 문제를 해결</hl>한다.</p>

행렬 체인 곱셈 문제의 정의와 AI분야에서의 중요성, 그리고 동적계획법 기반의 알고리즘을 학습한다.

(추후 보충 예정)

<br>

<br>

# References

1. [집콕]인공지능을 위한 알고리즘과 자료구조: 이론, 코딩, 그리고 컴퓨팅 사고-비선형 자료구조, K-MOOC: [http://www.kmooc.kr/courses/course-v1:SKKUk+SKKU_46+2021_T1/courseware/1158909c057347478d7386a2b7e97214/e9d736660cad4f76bcb9a05974fe0bf2/1?activate_block_id=block-v1%3ASKKUk%2BSKKU_46%2B2021_T1%2Btype%40vertical%2Bblock%4052b62ff6da2147c9a8a65a6056681d91](http://www.kmooc.kr/courses/course-v1:SKKUk+SKKU_46+2021_T1/courseware/1158909c057347478d7386a2b7e97214/e9d736660cad4f76bcb9a05974fe0bf2/1?activate_block_id=block-v1%3ASKKUk%2BSKKU_46%2B2021_T1%2Btype%40vertical%2Bblock%4052b62ff6da2147c9a8a65a6056681d91)

<br>

<br>  





