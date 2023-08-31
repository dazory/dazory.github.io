---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments
title: '선형대수 개요'
excerpt: "선형대수의 주요 개념들을 간략하게 간추려 살펴본다."
date: 2022-01-16
last_modified_at: 2022-01-16
categories:
  - 'mathematics-linear_algebra'
tags: 
   - [Linear Algebra]

use_math: true
comments: true
share : false
---

# Linear Algebra

여기서는 선형대수(*Linear Algebra*)의 대략적인 개념을 살펴본다. 보다 자세한 개념은 이후의 포스팅을 통해 찬찬히 살펴보도록 한다.

<br>

## Representations

선형대수(*linear algebra*)는 선형 방정식과 선형 변환에 관한 수학의 한 분야이다.

* 선형 방정식 (*linear equation*)

  $$
  a_1x_1 + ... + a_nx_n = b
  $$

* 선형 변환 (*linear map*)

  $$
  (x_1, ..., x_n) \mapsto a_1x_1 + ... + a_nx_n
  $$

이때 선형 방정식과 선형 변환은 <u>벡터 공간(vector space)</u>과 <u>행렬(matrices)</u>을 통해 표현된다.

* 벡터 (*Vector*)
* 행렬 (*Matrix*)

<br>

### Vector

$$
\textbf{a} = (a_{1}, a_{2}, ..., a_{n})
$$

**벡터(*Vectors*)**는 <u>숫자들의 배열</u>이다.

<br>

#### Operations

##### Sum

: $\textbf{a}_1 + \textbf{a}_2$

<br>

##### Product

* **Scalar product**

  : $k\textbf{a}$

* **Dot product**

  : $\textbf{a}·\textbf{b} = \sum_{i}{a_i b_i}$ (Scalar)

  * If $\|\textbf{a}\| = 1$, then $\textbf{a}·\textbf{b}$ 는 $\textbf{a}$의 방향을 따라 $\textbf{b}$를 투영한 것의 길이
  * If $\textbf{a}·\textbf{b}=0$, then  $\textbf{a}$와 $\textbf{b}$는 orthogonal

<br>

#### Properties

* $\textbf{b}=\sum_{i}{k_ia_i}$를 만족하는 $\{k_i\}$가...
  * 존재 ⇒ Linearly Dependence
  * 존재하지 않음 ⇒ Linearly Independence
* n-차원 공간의 한 점으로 표현됨

<br>

<br>

### Matrices

$$
\textbf{A}^{n×m} = 
\begin{bmatrix}
	a_{11} & a_{12} & ... & a_{1m} \\
	a_{21} & a_{22} & ... & a_{2m} \\
	\vdots & \vdots & \ddots & \vdots \\
	a_{n1} & a_{n2} & ... & a_{nm}
\end{bmatrix}
$$

<br>

#### Properties

* Matrices & Vectors

  * $d$-차원 vector는 $d×1$ matrix로 표현 가능

  * **Column vectors**

    $$
    \textbf{a}_{*} = (\textbf{a}_{*1}, \textbf{a}_{*2}, ..., \textbf{a}_{*m})
    $$

  * **Row vectors**

    $$
    \textbf{a}_* = (\textbf{a}^T_{1*}; \textbf{a}^T_{2*}; ...; \textbf{a}^T_{n*};)
    $$

<br>

<br>

## Operations

행렬의 다양한 연산에 대해 알아보자

<br>

### Sum

#### The sum of two vectors

$$
\textbf{a}+\textbf{b} =
\begin{bmatrix} a_1\\ a_2\\ \vdots\\ a_n \end{bmatrix}
+
\begin{bmatrix} b_1\\ b_2\\ \vdots\\ b_n \end{bmatrix}
=
\begin{bmatrix} a_1+b_1\\ a_2+b_2\\ \vdots\\ a_n+b_n \end{bmatrix}
$$

<br>

#### The sum of two matrices

$$
\displaylines{
{\textbf{A}+\textbf{B}} =

{\begin{bmatrix}
	a_{11} & a_{12} & ... & a_{1m} \\
	a_{21} & a_{22} & ... & a_{2m} \\
	\vdots & \vdots & \ddots & \vdots \\
	a_{n1} & a_{n2} & ... & a_{nm}
\end{bmatrix}
+
\begin{bmatrix}
	b_{11} & b_{12} & ... & b_{1m} \\
	b_{21} & b_{22} & ... & b_{2m} \\
	\vdots & \vdots & \ddots & \vdots \\
	b_{n1} & b_{n2} & ... & b_{nm}
\end{bmatrix}}
\\
= 
{\begin{bmatrix}
	a_{11}+b_{11} & a_{12}+b_{12} & ... & a_{1m}+b_{1m} \\
	a_{21}+b_{21} & a_{22}+b_{22} & ... & a_{2m}+b_{2m} \\
	\vdots & \vdots & \ddots & \vdots \\
	a_{n1}+b_{n1} & a_{n2}+b_{n2} & ... & a_{nm}+b_{nm}
\end{bmatrix}}
}
$$

<br>

<br>

### Multiplication

#### Scalar Multiplication

$$
\displaylines{
k\textbf{A} = 
\begin{bmatrix}
	ka_{11} & ka_{12} & ... & ka_{1m} \\
	ka_{21} & ka_{22} & ... & ka_{2m} \\
	\vdots & \vdots & \ddots & \vdots \\
	ka_{n1} & ka_{n2} & ... & ka_{nm}
\end{bmatrix}
}
$$

<br>

#### Matrix Vector Product

$$
\displaylines{
\textbf{A}\textbf{b} =
\begin{bmatrix} \textbf{a}^T_{1*} \\ \textbf{a}^T_{2*} \\ \vdots\\ \textbf{a}^T_{n*} \end{bmatrix} · \textbf{b}\\
=
\begin{bmatrix} \textbf{a}^T_{1*}·\textbf{b} \\ \textbf{a}^T_{2*}·\textbf{b} \\ \vdots\\ \textbf{a}^T_{n*}·\textbf{b} \end{bmatrix} \\
=
\sum_k{a_{*k}·b_{k}}
}
$$

> e.g.,
> $$
> \begin{bmatrix} 1&2&2\\2&3&1\\1&0&2 \end{bmatrix}
> \begin{bmatrix} 1\\ 3\\ 5 \end{bmatrix}
> =\begin{bmatrix} 
> 	\begin{bmatrix} 1&2&2 \end{bmatrix}
>     \begin{bmatrix} 1\\ 3\\5 \end{bmatrix} \\ 
>     \begin{bmatrix} 2&3&1 \end{bmatrix}
> 	\begin{bmatrix} 1\\ 3\\5 \end{bmatrix} \\ 
>     \begin{bmatrix} 1&0&2 \end{bmatrix}
>     \begin{bmatrix} 1\\ 3\\5 \end{bmatrix} \\ 
> \end{bmatrix}
> =
> \begin{bmatrix}
> 	1+6+10\\
> 	2+9+5\\
> 	1+0+10
> \end{bmatrix}
> =
> \begin{bmatrix} 17\\ 16\\ 11 \end{bmatrix}
> $$

* $\textbf{A}$의 column vectors가 reference system을 나타낸다면, $\textbf{A}\textbf{b}$는 $\{a_{*i}\}$에 따른 vector $\textbf{b}$의 global transformation이다.

<br>

#### Matrix Matrix Product

$$
\displaylines{
\textbf{C} = \textbf{A}\textbf{B} \\
=
\begin{bmatrix}
	a_{11} & a_{12} & ... & a_{1l} \\
	a_{21} & a_{22} & ... & a_{2l} \\
	\vdots & \vdots & \ddots & \vdots \\
	a_{n1} & a_{n2} & ... & a_{nl}
\end{bmatrix}
\begin{bmatrix}
	b_{11} & b_{12} & ... & b_{1m} \\
	b_{21} & b_{22} & ... & b_{2m} \\
	\vdots & \vdots & \ddots & \vdots \\
	b_{l1} & b_{l2} & ... & b_{lm}
\end{bmatrix}\\
=
\begin{bmatrix} 
	\textbf{a}^T_{1*}\\ \textbf{a}^T_{2*}\\ \vdots\\ \textbf{a}^T_{n*}
\end{bmatrix}
\begin{bmatrix}
	\textbf{b}^T_{*1} & \textbf{b}^T_{*2} & ... & \textbf{b}^T_{*m}
\end{bmatrix} \\
=

\begin{bmatrix} 
	\textbf{A}\textbf{b}^T_{*1} & \textbf{A}\textbf{b}^T_{*2} & ... & \textbf{A}\textbf{b}^T_{*m}
\end{bmatrix}
}
$$

: row vectors와 column vector의 dot product로 계산 가능하다.

* The columns of $\textbf{C}$ : the transformation of the columns of $\textbf{B}$ through $\textbf{A}$.

  $$
  \textbf{C} = \textbf{A}\textbf{B} = 
  \begin{bmatrix}
  	\textbf{A}\textbf{b}_{*1} & \textbf{A}\textbf{b}_{*2} & ... & \textbf{A}\textbf{b}_{*m}
  \end{bmatrix}
  $$

  $$
  \textbf{c}_{*i} = \textbf{A}\textbf{b}_{*i}
  $$

<br>

<br>

### Rank

**랭크(*Rank*)** 는 <u>선형 독립인 행(또는 열)의 최대 개수</u> 혹은 <u>dimension of the image of the transformation $f(\textbf{x}) = \textbf{A}\textbf{x}$</u>을 의미한다.

> e.g., $rank(\textbf{A}) = 2$ when ...
> 
> $$
> A={\begin{bmatrix}1&2&0&1\\0&0&1&1\\0&0&0&0\\0&0&0&0\\\end{bmatrix}}
> $$

* **Properties**
  * $rank(\textbf{A})≥0$   ⇔   $\textbf{A}^{m×n}$ : null matrix
  * $rank(\textbf{A})≤ \min(m,n)$

* **Computations**
  * Gaussian elimination on the matrix
  * Counting the number of non-zero rows

<br>

<br>

### Inverse

$$
\textbf{A}\textbf{B} = \textbf{I}\\
\textbf{B}=\textbf{A}^{-1} (\text{inverse matrix of $\textbf{A}$)}
$$

* **조건**

  * $\textbf{A}$ : a square matrix of full rank

* **특징**

  * 역행렬이 존재한다면, 그 역행렬은 유일(unique)하다.
  * $\textbf{A}\textbf{A}^{-1} = \textbf{I} = \textbf{A}^{-1}\textbf{A}$
  * $(\textbf{A}\textbf{B})^{-1} = \textbf{B}^{-1}\textbf{A}^{-1}$
  * $(\textbf{A}+\textbf{B})^{-1} ≠ \textbf{A}^{-1} + \textbf{B}^{-1}$

  * $\textbf{A}$의 $i$th row와 $\textbf{A}^{-1}$의 $j$th column은..

    $$
    \begin{cases}
    	\text{orthogonal} & \text{if $i≠j$} \\
    	\text{their dot product is $1$} & \text{if $i=j$}
    \end{cases}
    $$

* **응용**

  * 선형방정식의 해 구하기 : $\textbf{x} = \textbf{A}^{-1}\textbf{b}$

<br>

<br>

### Determinant

$$
\displaylines{
\det(\textbf{A}) =
a_{11}\textbf{C}_{11} + a_{12}\textbf{C}_{12} + ... + a_{1n}\textbf{C}_{1n} \\
= \sum_{j=1}^n{a_{1j}\textbf{C}_{1j}}
}
$$

* **계산**

  * Cofactor expansion
  * Gauss elimination

* **특징**

  $\textbf{A}^{n×n}$이라고 할 때 ...

  * $\det(\textbf{A})≠0$ ⇔ $\textbf{A}$의 역행렬이 존재
  * **Row operations**
    * $\textbf{B}$ : $\textbf{A}$의 두 행을 interchanging한 결과라면 ⇒ $\det(\textbf{B}) = -\det(\textbf{A})$
    * $\textbf{B}$ : $\textbf{A}$의 한 행에 숫자 $c$를 곱한 결과하면 ⇒ $\det(\textbf{B}) = c·\det(\textbf{A})$
    * $\textbf{B}$ : $\textbf{A}$의 한 행의 배수를 다른 행에 더한 결과라면 ⇒ $\det(\textbf{B}) = \det(\textbf{A})$
  * $\det(\textbf{A}^T) = \det(\textbf{A})$
  * $\det(\textbf{A}^{-1}) = \frac{1}{\det(\textbf{A})}$
  * $\det(\textbf{P}\textbf{A}\textbf{P}^{-1}) = \det(\textbf{A})$
  * $\det(c\textbf{A}) = c^n \det(\textbf{A})$
  * $$
        \det(\text{diag}(a_{11},...,a_{nn})) 
        = \begin{vmatrix} 
            a_{11}&*&0\\
            *&\ddots&*\\
            0&*&a_{nn}
        \end{vmatrix} = a_{11}a_{22}···a_{nn}
    $$
  * $$
        \begin{vmatrix}
            a_{11}&&0\\
            \vdots&\ddots&&\\
            a_{n1}&...&a_{nn}
        \end{vmatrix}
        =a_{11}a_{22}···a_{nn}
    $$
  * $\det(\textbf{A}·\textbf{B}) = \det(\textbf{A}) · \det(\textbf{B})$
  * $\det(\textbf{A}+\textbf{B}) ≠ \det(\textbf{A}) + \det(\textbf{B})$

  <br>

* **응용**

  * 기하적 분석
    * 역행렬의 존재 판별 : $\det(\textbf{A})≠0$ ⇒ 역행렬이 존재
    * 선형변환의 스케일 성분 : $\text{area, volume, ...} = \lvert \det(\textbf{A}) \rvert $
    * 도형의 방향 보존 : $\det(\textbf{A})>0$ ⇒ 방향이 보존
  * Eigenvalues 계산 : $D(\lambda) = \det(\textbf{A}-\lambda \textbf{I})=0$

<br>

<br>

### 특수행렬

#### Orthogonal Matrix

모든 column(or row) vectors가 직교(*orthonormal*)하는 $\textbf{Q}$를 **직교 행렬(*orthogonal*)**이라고 한다.

$$
q^T_{*i}·q_{*j} = 
\begin{cases}
	1 & \text{if $i=j$}\\
	0 & \text{if $i≠j$}
\end{cases},
 ∀i,j
$$

※ i.e., 본인과 같은 column(row) vector와의 내적은 1이고, 그 외는 0 ※

* **특징**

  * 선형 변환으로서 norm은 보존된다. (norm = 1)

  * $\textbf{Q}\textbf{Q}^T = \textbf{Q}^T\textbf{Q} = \textbf{I}$

  * 행렬식은 unity norm(±1)을 갖는다 :

    $$
    1 = \det(\textbf{I}) = \det(\textbf{Q}^T\textbf{Q})=\det(\textbf{Q})\det(\textbf{Q}^T)=\det(\textbf{Q})^2
    $$

<br>

#### Symmetric Matrix

**대칭행렬(*symmetric matrix*)**은 정방행렬에서 대각선에 대칭인 두 원소가 같은 행렬을 의미한다.

> e.g., 대칭행렬의 예시
> 
> $$
> \begin{bmatrix}
> 	2&1\\ 1&5
> \end{bmatrix},
> \begin{bmatrix}
> 	3&6&1\\ 6&2&2\\ 1&2&4
> \end{bmatrix}
> $$

* **특징**
  * 실수인 고유값을 갖는다.

<br>

#### Positive Definite Matrix

**양의 정부호 행렬(*Positive Definite Matrix*)** 은 모든 고유값(*eigenvalues*)이 양수인 대칭행렬(*symmetric matrix*)이다. (i.e., 대칭 행렬의 특수한 케이스)

$$
\textbf{M} > 0 \\ 
\text{iff } \textbf{z}^T\textbf{M}\textbf{z}>0, ∀z≠0\\
$$

* **예시**

  * 양의 정부호행렬의 예시1

    $$
    \textbf{M}_1 = \begin{bmatrix} 1&0\\0&1\end{bmatrix} \text{일 때, }\\
    \begin{bmatrix}z_1 & z_2\end{bmatrix}
    \begin{bmatrix}1&0\\0&1\end{bmatrix}
    \begin{bmatrix}z_1\\z_2\end{bmatrix}
    = z_1^2+z_2^2 > 0
    $$

  * 양의 정부호행렬의 예시2

    $$
    \textbf{M}_2 = \begin{bmatrix}2&-1&0\\-1&2&-1\\0&-1&2\end{bmatrix} \text{일 때,}\\
    \begin{bmatrix}z_1 & z_2 & z_3\end{bmatrix}
    \begin{bmatrix}2&-1&0\\-1&2&-1\\0&-1&2\end{bmatrix}
    \begin{bmatrix}z_1\\z_2\\z_3\end{bmatrix} \\
    = 
    2z_1^2-2z_1z_2 + 2z_2^2 - 2z_2z_3 + 2z_3^2\\
    = z_1^2 + (z_1-z_2)^2 + (z_2-z_3)^2 + z_3^2 >0
    $$

* **특징**

  * **Invertible**, with positive definite inverse

  * 모든 실수 eigenvalues > 0

  * **Trace** is > 0

    > **대각합(*Trace*)**은 정사각행렬의 주대각성 성분의 합을 의미한다.

  * **Cholesky** decomposition $\textbf{A} = \textbf{L}\textbf{L}^T$ 

    ※ $\textbf{L}$ : 하삼각행렬

    > **숄레스키 분해(*Cholesky decomposition*)**는 양의 정부호행렬(positive-definite matrix)의 분해에서 사용된다. 
    > 
    > $$
    > \textbf{A} = \textbf{L}\textbf{L}^*
    > $$
    > 
    > 이때 $\textbf{A}$의 모든 성분이 실수이면, $\textbf{L}$의 모든 성분도 실수이므로 다음과 같이 분해된다. 
    > 
    > $$
    > $\textbf{A}=\textbf{L}\textbf{L}^T
    > $$

<br>

#### Rotation Matrix

**회전변환행렬(*rotation matrix*)**은 $\det=±1$을 만족하는 직교행렬(orthonormal matrix)이다.

* **예시**

  * **2D rotations**

    $$
    R(\theta) = 
    \begin{bmatrix}
    	\cos(\theta) & -\sin(\theta)\\
    	\sin(\theta) & \cos(\theta)
    \end{bmatrix}
    $$

  * **3D rotations along the main axes**

    $$
    R_x(\theta) = 
    \begin{bmatrix}
    	1&0&0\\
    	0&\cos(\theta)&-\sin(\theta)\\
    	0&\sin(\theta)&\cos(\theta)
    \end{bmatrix}
    $$

    $$
    R_y(\theta) = 
    \begin{bmatrix}
    	\cos(\theta)&0&-\sin(\theta)\\
    	0&1&0\\
    	\sin(\theta)&0&\cos(\theta)
    \end{bmatrix}
    $$

* **특징**

  * **Not commutative**

    > e.g.,
    > $$
    > R_x(\frac{\pi}{4})·R_y(\frac{\pi}{4}) = ... = \begin{bmatrix}-1.414\\ 0.586\\ 3.414\end{bmatrix}\\
    > ≠ R_y(\frac{\pi}{4})·R_x(\frac{\pi}{4}) = ... = \begin{bmatrix}-1.793\\ 0.707\\ 3.207\end{bmatrix}
    > $$

<br>

<br>

## Affine Transformations

> **아핀 변환(*Affine Transformation*)**이란, 기하학적 성질을 보존하는 두 아핀 공간 사이의 함수이다.
>
> ※ 점, 직선, 평면이 보존되는 선형 매핑 ※

3차원 변환을 묘사하는 가장 쉽고 대중적인 방법은 행렬을 이용하는 것이다.

$$
\textbf{A} = \begin{bmatrix}\textbf{R}&\textbf{t}\\\textbf{0}&\textbf{1}\end{bmatrix} \textbf{A}^{-1}
= \begin{bmatrix}
	\textbf{R}^T & -\textbf{R}^T\textbf{t} \\
	\textbf{0} & \textbf{1}
\end{bmatrix} \textbf{p}
=
\begin{bmatrix}\textbf{t}\\\textbf{1}\end{bmatrix}
$$

* $\textbf{R}$ : Rotation matrix
* $\textbf{t}$ : Translation vector

<br>

### Combining Transformations

변환 행렬(*transformation matrices*)를 여러 개 결합하여(chaining) 간단하게 하나의 변환 행렬로 표현할 수 있다. 예를 들어 다음과 같이 로봇과 센서, 객체에 대한 행렬이 주어졌다고 가정하자.

<img src="/files/2022-01-16-00_Overall_Linear_Algebra/image-20220114223349649.png" alt="image-20220114223349649" style="width:400px;" />

* $\textbf{A}$ : the pose of a robot in the space

* $\textbf{B}$ : the position of a sensor on the robot

* The sensor perceives an object at a given location $\textbf{p}$, in its own frame

  ※ The sensor has no clue on where it is in the world ※

이때 global frame에서 object의 위치는 다음의 과정으로 구할 수 있다.

* $\textbf{B}\textbf{p}$ : the pose of the object wrt the robot
* $\textbf{A}\textbf{B}\textbf{p}$ : the pose of the object wrt the world

<br>

<br>

## References

1. 01.Linear Algebra, Introduction to Mobile Robotics - SS 2021, [http://ais.informatik.uni-freiburg.de/teaching/ss21/robotics/](http://ais.informatik.uni-freiburg.de/teaching/ss21/robotics/0).
2. "[선형대수학] 양의 정부호 행렬(positive definite matrix)이란?.txt", bskyvision, 2017년 11월 22일 수정, 2022년 01월 14일 접속, [https://bskyvision.com/205](https://bskyvision.com/205)
3. "[기계학습] Positive-Definite Matrix 란?", 자연의 원리에 귀를 기울이다 네이버 블록, 2017년 12월 7일 수정, 2022년 01월 14일 접속, [https://m.blog.naver.com/sw4r/221157302215](https://m.blog.naver.com/sw4r/221157302215)

<br>

<br>



