---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments # single
title: '[DL-02] Lect3. Neural Networks (part1)'
excerpt: "[AIAS] 인하대학교 홍성은 교수님의 인공지능 응용 시스템 수업을 듣고 공부한 자료입니다."
date: 2021-07-10
last_modified_at: 2021-07-13
categories:
  - ai-deeplearning # Deep Learning
tags: 
   - [Deep Learning, AIAS]

use_math: true
comments: true
share : false
sidebar_main: true
---

<!-- AIAS-Lect3_Neural Networks (part1) -->



# Traditional ML Approaches

이전 시간을 통해 우리는 pixel 단위(밝기)로 학습하면 안되는 이유에 대해서 알아보았다.

> **까먹었을까봐 다시 언급**
>
> 사람이 볼 수 있는 image의 형태와 computer가 인식할 수 있는 image의 형태에는 차이가 있는데, 이러한 차이를 **semantic gap**이라고 한다. 예를 들어 아래와 같이 사람이 봤을 때의 고양이 사진은 컴퓨터 입장에서는 0~255사이의 숫자로 표현되는 8-bit unsigned integer로 인식된다. (...) semantic gap으로 인해 다음의 경우에서 제대로된 결과를 도출할 수 없다.
>
> 1. Viewpoint variation
>
> 2. Background Clutter
>
> 3. Illumination
>
>    ...

사람과 컴퓨터간의 이미지를 인식하는 방법 차이(semantic gap)로 인해 pixel 단위의 접근 방법은 이미지 인식 분야에 맞지 않았다. 따라서 컴퓨터가 이해할 수 있는 feature vector의 형태로 바꾸는 과정이 필요하다. "Simple is the best"라는 철학 아래에서 봤을 때 feature vector는 복잡한 것 보다 구분이 쉬운 형태가 좋다. 아래 이미지는 feature vector의 좋은 예와 나쁜 예를 보여준다.

![image-20210417170237468](/files/image-20210417170237468.png)

왼쪽 이미지의 경우 빨간점과 파란점을 linear classifier만으로 구분하기 어렵다는 한계가 있다. 따라서 이러한 feature vector를 어떠한 방법(f)으로 변형하여 오른쪽과 같이 linear classifier만으로 쉽게 판별 가능한 형태로 만드는 것이 feature extraction에서의 주요 목적이다.

## Examples of Image Feature

이미지에서 feature로 뽑을 수 있는 형태에는 어떤 것이 있는지 알아보자.

### Color Histogram

<img src="/files/image-20210417170525888.png" alt="image-20210417170525888" style="zoom:80%;" />

**color histogram**은 <u>이미지에서 각 color가 얼마만큼의 비율을 차지하고 있는가</u>를 보여주는 feature이다. 예를 들어 위 이미지는 "연두색 개구리가 초록색 풀 위에 앉아있는 이미지"이다. 이런 경우 초록계열의 컬러가 대부분이므로 히스토그램을 그렸을 때 초록 부분의 막대가 가장 높다. 이때 주의할 점은 color의 개수만 세면 안된다는 점이다. 입력 이미지는 크기가 제각각일 수 있으므로 color별 개수를 세고 나서 최종적으로 전체 pixel의 개수로 나눠주는 과정이 필수이다.



### Histogram of Oriented Gradients (HoG)

<img src="/files/image-20210417170838538.png" alt="image-20210417170838538" style="zoom:80%;" />

**histogram of oriented gradients(HoG)**는 <u>이미지상에서 밝기값이 급격히 변하는 정도인 gradient를 histogram으로 나타낸 것</u>이다. 예를 들어 위 개구리 이미지의 경우, 잎사귀의 뚜렷한 경계 부분과 개구리의 경계 부분이 gradient로 잘 표시된 것을 확인할 수 있다.



## Image Features

전통적인 ML 접근법 관점에서 바라봤을때 feature vectors는 다음과 같이 얻어진다.

<img src="/files/image-20210417171128863.png" alt="image-20210417171128863" style="zoom:80%;" />

먼저 이미지로부터 원하는 features(가령 color histogram, HoG 등)을 뽑아내고, 이를 이어붙인다. 이러한 과정을 **concatenation(연쇄)**라고 부른다.



## Image Features vs. ConvNets

이전에 ML과 DL의 차이에 대해 간략히 소개한 적이 있다. ML에서는 개발자가 feature extraction의 과정을 거친 뒤 컴퓨터에게 학습을 시키는 반면, DL의 경우 feature extraction과정을 model parameters의 영역에 두어 컴퓨터에게 맡긴다. 그렇다면 **전통적인 ML관점에서의 image features**방식과 **DL관점에서의 convolution networks** 중에서 어느 방법이 더 좋은 것일까?

완전한 정답은 없다. ML의 image features방법은 데이터가 너무 작거나 한정적인 경우에는 DL보다 오히려 적합할 수 있다. 하지만 요즘은 데이터의 양이 워낙 방대하기때문에 대부분의 경우 DL의 convolution networks가 월등하게 더 좋은 성능을 낸다. 







# Introduction to Neural Networks (NN)

이번 페이지에서는 neural networks의 기본 개념에 대해 알아보는 시간을 갖는다.



## Tensors

1차원 데이터를 우리는 "scalar"라고 부른다. 2차원 데이터는 "vector"혹은 "matrix"라고 부를 수 있다. 그렇다면 3차원 이상의 데이터는 어떻게 부를 수 있을까? 3차원 이상의 데이터는 "tensor"라고 표현한다. 아래 그림은 tensor의 차원에 대해 잘 보여준다.

![image-20210417171957054](/files/image-20210417171957054.png)

일반적으로 3차원까지는 인간의 직감으로 충분히 상상할 수 있다. 하지만 차원이 확장됨에따라 직관적인 판단이 어려워진다. 차원에 대해 가장 쉽게 직감할 수 있는 예시는 바로 image이다. image는 3차원으로 표현이 가능하다(너비×높이×채널). 이러한 image를 시간에 따라 표현하면 video가 되는데, 이를 4차원이라 볼 수 있다. 



## Neural Networks (NN)

이전까지는 컴퓨터가 인간을 절대 이길 수 없다는 생각이 지배적이었다. 컴퓨터는 단순히 정해진 규칙에 따른 연산만을 수행하는 기계였기 때문이다. 하지만 인간의 사고를 모방하는 artificial neural networks(ANNs)의 개념이 등장하면서 컴퓨터의 영역은 단순 연산에서 더욱 확장되었다. **artificial neural networks(ANNs)**은 인간의 신경전달세포인 neuron의 동작을 모방한 network 구조이다. 줄여서 **neural networks(NNs)**라고 부르기도 한다. 

이전까지는 ML의 가장 단순한 형태인 linear classifier에 대해서 배웠다. linear classifier는 입력과 출력간에 linear관계가 있음을 가정하고 아래의 식을 linear score function으로 둔다.
$$
Linear\ score\ function:\ f=Wx
$$
하지만 실제로는 linear한 문제보다 nonlinear한 문제가 더 많다. 따라서 nonlinear 문제를 풀 수 있는 방식으로 nonlinear function의 일종인 **activation function**이 등장했다.
$$
2-layer\ Neural\ Network:\ f=W_2\max{(0, W_1x)}
$$
위 수식은 2개의 layers를 갖는 neural network의 function을 의미한다. 여기서 max()는 activation function의 일종으로 음수가 나오지 않게 해주는 역할을 하면서 nonlinearity를 부여한다. 이를 더욱 확장하여 3개의 layers를 갖는 NN에 적용하면 아래와 같은 꼴을 갖는다.
$$
3-layer\ Neural\ Network:\ f=W_3\max{(0, W_2\max{(0, W_1x)})}
$$
이때 3-layer NN에 activation functions이 두 개인 것을 확인할 수 있다. 왜 굳이 activation functions은 추가 layer 당 한 개씩 있어야 하는 걸까? 아래와 같은 형식은 안되는 걸까?
$$
f=W_2W_1x
$$
답은 "안된다"이다. 위 수식은 겉 보기엔 2-layer NN처럼 보인다. 하지만 결국 아래의 과정에 의해 1-layer임을 알 수 있다.
$$
f=W_2W_1x=W_3x\ (∵W_3=W_2W_1)
$$

## Activation Functions

**Activation functions**에 대해 자세히 알아보자. 앞서 NN은 인간의 neuron의 동작을 벤치마킹하여 등장한 network 구조임을 배웠다. activation function의 등장 배경을 이해하기에 앞서 neuron에 동작 방식을 먼저 이해해보자.

### Neuron

깊게 들어가면 끝이 없으므로 여기서는 간단하게 뉴런의 동작 원리를 이해하는 정도로 넘어가겠다. 인간은 천억개의 뉴런을 가지며, 뉴런 다발을 통해 자극을 각 기관에 전달한다. 

![Neurone 3D GIF | Gfycat](https://thumbs.gfycat.com/AdvancedSillyDogwoodtwigborer-size_restricted.gif)

이때 하나의 뉴런에서 자극이 일어난다고 가정하자. 뉴런은 자극이 특정 값 이상인 경우 축색돌기를 통해 바깥으로 신경전달물질을 내보내어 다른 뉴런을 또다시 자극한다. NN은 <u>뉴런의 특정값 이상의 자극이 들어올 때 외부로 자극을 보내는 동작 방식</u>에서 착안하였다. 아래 이미지는 NN의 동작 원리를 보여준다.

<img src="https://thumbs.gfycat.com/DeadlyDeafeningAtlanticblackgoby-size_restricted.gif" alt="최고 Neural Network GIF들 | Gfycat" style="zoom:50%;" />

어떤 이미지가 입력되면(자극), 각 node(뉴런)에서 자극을 받고 자극이 특정값 이상이 되면 다음 node(뉴런)을 다시 자극한다. 이때 특정값 이상이 되면 출력을 내보내는 역할을 하는 함수가 필요한데, 이를 **activation function**이라고 한다.

### Examples

activation function의 종류는 다양하다. 각 activation function별 특징에 대해서 살펴보자.

#### Sigmoid

![image-20210417180619029](/files/image-20210417180619029.png)
$$
σ(x)=\frac{1}{1+e^{-x}}
$$
**sigmoid** 함수는 출력값이 0~1 사이로 표현되며, 입력값이 특정 범위보다 작으면 0(에 수렴), 크면 1(에 수렴)이 된다는 특징을 갖는다. 이러한 특징으로 인해 sigmoid함수는 다음의 문제를 갖는다.

1. 입력의 절대값이 큰 경우 0또는 1로 출력값이 수렴하여 **gradient를 소멸시키는 문제**가 발생한다. (backpropagation을 다룰 때 자세히 설명한다.)
2. 원점이 중심이 아니므로 sigmoid함수의 출력의 평균값은 0.5이다. 따라서 각 layer를 통과할 때마다 출력이 입력보다 커질 가능성이 높아지는 **bias shift**문제가 있다.

이러한 이유로 인해 요즘은 잘 쓰이지 않는 방식이다.



#### tanh

![image-20210417180921015](/files/image-20210417180921015.png)
$$
tanh(x)
$$
**tanh** 함수는 출력값이 -1~1 사이로 표현되며, 입력값이 특정 범위보다 작으면 -1, 크면 1이 된다는 특징을 갖는다. tanh()역시 sigmoid함수와 유사한 문제로 인해 요즘은 잘 쓰이지 않는 함수이다.



#### ReLU

![image-20210417181031814](/files/image-20210417181031814.png)
$$
\max{(0,x)}
$$
**ReLU** 함수는 출력값이 0 이상으로 표현된다. 입력값이 음수인 경우 0을 출력하며, 음수가 아닌 경우는 입력 그대로를 출력한다. 음수가 아닌 입력에 대해서는 단순히 입력값을 그대로 출력하기 때문에 계산속도가 빠르기 때문에 요즘 가장 많이 사용되는 activation function이다. 하지만 ReLU함수 역시 이러한 특성으로 인해 다음의 문제가 있다.

1. **dead ReLU**

   학습시, 일부 뉴런이 0만을 출력하여 이후 뉴런에서 전혀 활성화가 되지 않는 문제

이러한 dead ReLU문제를 해결하기 위해 ReLU함수의 다향한 변형 모델이 제시되었다.



#### Leaky ReLU

![image-20210417181217753](/files/image-20210417181217753.png)
$$
\max(αx,x)
$$
**Leaky ReLU**는 ReLU함수의 변형 함수이다. 입력이 음수인 경우, 0를 출력하던 ReLU 함수와 달리 작은 weigth를 주어 음수 출력을 보다 작게 스케일링하여 출력한다. 이를 통해 음수 입력값에 대해서 항상 0을 출력함으로써 발생했던 dead ReLU문제가 어느정도 해소된다. 



#### Maxout

$$
\max(w_1^Tx + b_1, w_2^Tx+b_2)
$$

**Maxout** 역시 ReLU함수의 변형 모델이다. 



#### ELU (Exponential Linear Unit)

![image-20210417181451672](/files/image-20210417181451672.png)
$$
\left\{
	\begin{matrix}
	x\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ x≥0\\
	α(e^x-1)\ \ \ \ \ x<0
	\end{matrix}
\right.
$$
입력값이 음수인 경우, 출력값의 평균이 0에 가까우므로 bias shift문제를 방지하여 gradient 소실 문제를 줄여준다 (일반적인 경우 α=1로 정의). 또한 음수 입력값에도 출력이 0은 아니므로 ReLU함수의 문제점인 dead ReLU가 발생하지도 않는다.



> **어떤 activation function을 사용할까?**
>
> CS231n 강의에 의하면 sigmoid함수는 사용하지 말고, 일반적인 경우 ReLU를 먼저 사용해 보고 그 다음으로 LeakyReLU나 ELU와 같이 ReLU Family를 사용할 것을 권장한다. 



## Neural Networks: Architectures

NN의 일반적인 구조는 다음과 같다.

![image-20210417182941755](/files/image-20210417182941755.png)

위와 같은 구조를 **3-layer Neural Network** 혹은 **2-hidden-layer Neural Network**라고 부른다. 

### 코드 구현

**forward-pass of a 3-layer neural network**

```python
# forward-pass of a 3-layer neural network:
f = lambda x: 1.0/(1.0 + np.exp(-x)) # activation function (use sigmoid)
x = np.random.randn(3,1) # random input vector of three numbers (3x1)
h1 = f(np.dot(W1, x)+b1) # calculate first hidden layer activations (4x1)
h2 = f(np.dot(W2,h1)+b2) #calculate second hidden layer activations (4x1)
out = np.dot(W3, h2)+b3 # output neuron (1x1)
```



**full implementation of training a 2-layer Neural Network**

```python
import numpy as np
from numpy.random import randn

N, D_in, H, D_out = 64, 1000, 100, 10
x, y = randn(N, D_in), randn(N, D_out)
w1, w2 = rand(D_in, H), randn(H, D_out)

for t in range(2000):
    h = 1 / (1 + np.exp(-x.dot(w1)))
    y_pred = h.dot(w2)
    loss = np.square(y_pred - y).sum()
    print(t, loss)
    
    grad_y_pred = 2.0 * (y_pred - y)
    grad_w2 = h.T.dot(grad_y_pred)
    grad_h = grad_y_pred.dot(w2.T)
    grad_w1 = x.T.dot(grad_h*h*(1-h))
    
    w1 -= 1e-4 * grad_w1
    w2 -= 1e-4 * grad_w2
```





# Gradient Descent

이전 시간에 linear classifier에 대해 알아 보았다. linear classifier의 동작 원리는 아래 그림과 같이 요약할 수 있다.

![image-20210417012403686](/files/image-20210417012403686.png)

feed-forward의 출력값(estimation scores)을 실제 target scores와 loss function을 통해 비교하여 모델의 성능을 평가한다. loss를 줄이는 방향으로 back propagationi이라는 과정을 통해 model parameters를 업데이트 하면서 모델을 학습 시킨다. 이때 loss를 줄이는 방향으로 모델을 학습시킬 수 있는 방법인 "**back propagation**"에 대해서 자세히 알아보자.



## How to update the model parameters

Back propagation(optimization)을 하는 알고리즘에는 gradient descent(경사 하강법)가 있다. **gradient**는 기울기를 의미하는데, 기울기(gradient)가 내려가는 방향으로 model parameters를 update함으로써 optimization이 달성된다. 

### Strategy #1: Random search

model parameters가 random하게 업데이트되며 학습한다고 가정하자. 이에 대한 코드는 아래와 같다.

```python
# assume X_train is the data where each column is an example (e.g. 3073 x 50,000)
# assume Y_train are the labels (e.g. 1D array of 50,000)
# assume the function L evaluates the loss function

bestloss = float("inf") # Python assigns the highest possible float value
for num in xrange(1000):
    W = np.random.randn(10, 3073) * 0.0001 # generate random parameters
    loss = L(X_train, Y_train, W) # got the loss over the entire training set
    if loss < bestloss: # keep track of the best solution
        bestloss = loss
        bestW = W
    print 'in attempt %d the loss was %f, best %f' % (num, loss, bestloss)
    
# prints:
# in attempt 0 the loss was 9.401632, best 9.401632
# in attempt 1 the loss was 8.959668, best 8.959668
# in attempt 2 the loss was 9.044034, best 8.959668
# in attempt 3 the loss was 9.278948, best 8.959668
# in attempt 4 the loss was 8.857370, best 8.857370
# in attempt 5 the loss was 8.943151, best 8.857370
# in attempt 6 the loss was 8.605604, best 8.605604
# ... (trunctated: continues for 1000 lines )
```

위와 같은 학습과정을 거친 뒤 test set에 대하여 모델의 성능을 검증해보았다. 코드는 아래와 같다.

```python
# Assume X_test is [3073 x 10000], Y_test [10000 x 1]
scores = Wbest.dot(Xte_cols) # 10 x 10000, the class scores for all test examples
# find the index with max score in arch column (the predicted class)
Yte_predict = np.argmax(scores, axis = 0)
# and calculate accuracy (fraction of predictions that are correct)
np.mean(Yte_predict == Yte)
# returns 0.1555
```

결과적으로 random search 학습방법은 약 15.5%의 정확도를 달성하였다. 나쁘지는 않으나 좋지는 않는 성적이다.

### Stategy #2: Follow the slope

두번째 전략으로 slope(경사)를 따라 내려가는 방법을 사용해본다. 1차원의 경우 기울기에 대한 수식은 아래와 같다.
$$
\frac{df(x)}{dx} = \lim_{h→0}{\frac{f(x+h)-f(x))}{h}}
$$
다중 차원의 경우, gradient는 각 차원에 대한 partial derivatives의 벡터가 될 것이다. 이를 수식으로 표현하면 아래와 같다.
$$
x→▽f(x)=
\left(
	\begin{matrix}
		\frac{∂f(x)}{∂x_1}\\
		\frac{∂f(x)}{∂x_2}\\
		...\\
		\frac{∂f(x)}{∂x_n}\\
	\end{matrix}
\right)
$$
(gradient dW를 W에 업데이트하는 과정은 Lect3_55page를 참고)



이렇듯 random하게 model parameters를 학습하는 알고리즘보다는 gradient가 하강하는 방향으로 model parameters를 학습하는 것이 더 좋은 성능을 도출한다.



##  SGD ( Stochastic Gradient Descent)

위에서 gradient descent 알고리즘에 대해서 살펴 보았다. gradient descent 알고리즘은 loss function을 계산할 때 전체 training data을 사용하여 step을 결정한다. 이러한 방법은 step 방향에 대한 정확도는 높여주지만 한 step을 계산하는 데에 너무 많은 연산이 필요하다는 단점이 있다. 이러한 단점을 개선한 것이 바로 stochastic gradient descent알고리즘이다. stochastic은 "확률적" 이라는 뜻을 가졌는데, 이름에서 알 수 있듯이 model parameters를 업데이트하는 과정에서 "전체" training data가 아닌 "일부" training data만을 사용하여 하나의 step을 업데이트 할 때 보다 덜 정확하지만 빠른 연산 속도를 통해 더 나은 성능을 도출하자는 철학을 가졌다.



### 용어 정리

SGD에서 새롭게 알게 될 용어들에 대해 간단히 살펴보자.

#### Epoch

**Epoch**은 한국말로 "시대"를 뜻한다. AI에서 epoch은 <u>전체 dataset에 대하여 한 번 학습을 완료한 상태</u>(forward & backward pass과정을 거친 상태)를 의미한다.



#### Batch

**batch**는 <u>나누어진 dataset</u>을 의미한다. gradient descent에서는 전체 dataset을 한꺼번에 학습시키는데, 이로인해 발생하는 비효율 문제를 해소하고자 나온 알고리즘이 바로 stochastic gradient descent이었다. 여기서 SGD는 전체 dataset을 잘게 쪼개어 batch 단위로 학습시킨다고 볼 수 있다. batch를 **mini-batch**라고도 부르며, 일반적으로 사이즈는 32, 64, 128개를 주로 사용한다.



#### Iteration

**iteration**은 한국말로 "되풀이"라는 뜻이다. 전체 dataset을 쪼개어 batch 단위로 학습을 수행할 때, 1 epoch을 달성하기 위해서는 여러 번의 실행이 필요하다. 이러한 <u>실행 반복</u>을 iteration이라고 한다. 전체 data의 양이 N개라고 할 때 batch size가 n이면, iteration은 N/n이 되는 관계이다.



### 개념정리

Stochastic Gradient Descent (SGD)에 대한 loss function은 아래와 같다.
$$
L(W)=\frac{1}{N}\sum_{i=1}^{N}{L_i(x_i,y_i,W)+λR(W)}\\
▽_WL(W)=\frac{1}{N}\sum_{i=1}^{N}{▽_WL_i(x_i,y_i,W)+λ▽_WR(W)}
$$

```python
# Vanilla Minibatch Gradient Descent

while True:
    data_batch = sample_training_data(data, 256) # sample 256 examples
	weights_grad = eveluate_gradient(loss,fun, data_batch, weights)
    weights += - step_size * weights_grad # perform parameter update
```







# Computational Graph & Backpropagation

앞서 optimization을 하는 방법 중 하나로 gradient descent와 같은 방법을 언급했다. 하지만 다양한 꼴의 function에 대해 gradient를 구하기란 쉽지 않다. 지금까지 배운 functions만 나열해도 아래와 같다.
$$
s = f(x; W_1,W_2)=W_2\max{(0,W_1x)} ···· Nonlinear\ score\ function\\
L_i = \sum_{j≠y_i}{\max(0, s_j-s_{y_i}+1)} ···· SVM\ Loss\ on\ predictions\\
R(W) = \sum_k{W_k^2}···Regularization\\
L=\frac{1}{N}\sum_{i=1}^N{L_i+λR(W_1)+λR(W_2)} ····Total\ loss:data\ loss+regularization
$$
가장 좋지 않은 방법은 일일히 손으로 gradient를 계산하는 것이다. 이러한 방법은 너무 많은 matrix 계산을 필요로 할 뿐만 아니라 network에서 일부분을 수정하는 경우 전체 gradient를 다시 계산해야 하는 문제에 직면한다. 또한 너무 복잡한 모델에 대해서는 계산 자체가 쉽지 않다. 이러한 단점을 해소하기 위해 등장한 개념이 바로 **computational graphs**와 **backpropagation**이다.



## Backpropagation

간단한 예제에 대해 backpropagation을 구해보자.

<img src="/files/backpropagation(1).jpg" alt="backpropagation(1)" style="zoom:50%;" />

위 이미지는 model parameters x, y, z를 학습하는 방법에 대한 문제이다. x, y, z의 초기값(연두색)이 각각 -2, 5, -4일 때 출력값은 -12이다. 각 node별로 gradient를 계산함으로써 보다 간편하게 backpropagation할 수 있다. 먼저 출력쪽(f)에서의 gradient를 구하면 아래와 같다.
$$
gradient\ of\ f : \frac{∂f}{∂f}=1
$$
df/df=1를 항상 만족하므로 마지막 출력쪽에서의 gradient는 언제나 1이다. 이를 이용하여 q와 z에서의 gradient를 구하면 아래와 같다.
$$
gradient\ of\ q : \frac{∂f}{∂q}=\frac{∂f}{∂f}*\frac{∂f}{∂q}=1*\frac{∂qz}
{∂q}=1*z=-4\\
gradient\ of\ z : \frac{∂f}{∂z}=\frac{∂f}{∂f}*\frac{∂f}{∂z}=1*\frac{∂qz}{∂z}=1*q=3\\
$$
이를 이용하여 x, y에서의 gradient df/dx, df/dy를 chain rule을 활용하여 아래와 같이 계산할 수 있다.
$$
gradient\ of\ x : \frac{∂f}{∂x}=\frac{∂f}{∂q}*\frac{∂q}{∂x}=\frac{∂qz}{∂q}*\frac{∂(x+y)}{∂x}=z*1=-4\\
gradient\ of\ y : \frac{∂f}{∂y}=\frac{∂f}{∂q}*\frac{∂q}{∂y}=\frac{∂qz}{∂q}*\frac{∂(x+y)}{∂x}=z*1=-4\\
$$

## Chain Rule

Chain Rule의 개념을 통해 각 node별 gradient 계산 과정을 더 잘 이해할 수 있다. 아래 이미지는 chain rule을 그림으로 표현한 것이다.

<img src="/files/backpropagation(2).jpg" alt="backpropagation(2)" style="zoom:50%;" />

먼저 node f 기준으로 봤을 때 출력단에서 되돌아오는 gradient를 **upstream gradient**라고 한다. 최종 loss L을 f의 출력 z로 미분한 결과에 해당한다. node f를 기준으로 했을 때 입력단으로 되돌아가는 gradient는 **downstream gradient**라고 부른다. downstream gradient는 최종 loss L을 입력 x나 y로 미분한 결과에 해당한다. downstream gradient를 계산하기 위해서는 local gradient가 필요하다. **local gradient**는 node f 기준으로 output z에 대한 input x(or y)의 미분 결과이다. downstream gradient는 upstream gradient와 local gradient의 곱으로 표현할 수 있다. 이러한 관계를 **chain rule**이라고 부른다.



## Example

아래와 같은 예시를 통해 좀 더 복잡한 loss function에 대한 backpropagation을 계산해 보자.

<img src="/files/DL-Lect3/슬라이드16.JPG" alt="슬라이드16" style="zoom:50%;" />

위와 같은 loss function f(w,x)가 있을 때 각 node에 대한 local gradient는 다음과 같이 계산된다.

<img src="/files/DL-Lect3/슬라이드17.JPG" alt="슬라이드17" style="zoom:50%;" />

한편, 이러한 loss function에서 현재 model parameters가 아래와 같다고 가정해보자.

<img src="/files/DL-Lect3/슬라이드18.JPG" alt="슬라이드18" style="zoom:50%;" />

이때 loss function 최종 출력 부분에서의 upstream gradient값이 1.00이므로 chain rule에 의하여 아래와 같이 backpropagation을 계산할 수 있다.

<img src="/files/DL-Lect3/슬라이드19.JPG" alt="슬라이드19" style="zoom:50%;" />

이 부분은 위에서 설명했던 chain rule 공식 (downstream gradient = upstream gradient * local gradient)만 잘 활용한다면 쉽게 계산할 수 있다. 한편, 위 loss function의 일부 nodes는 아래와 같이 하나의 function 단위(sigmoid)로 묶일 수 있다.

<img src="/files/image-20210418000421846.png" alt="image-20210418000421846" style="zoom:50%;" />

sigmoid 함수 σ(x)의 local gradient값은 (1-σ(x))σ(x)이다. 이런 부분은 nodes를 하나로 치환해서 바라보는 것이 더 쉽다.



## Patterns in gradient flow

자주 사용되는 node에 대하여 local gradients는 다음과 같다.

<img src="/files/image-20210418000554100.png" alt="image-20210418000554100" style="zoom:80%;" />



## Backpropagation Implementation

**Modularized API**를 활용하여 forward & backprop 를 구현한 코드는 아래와 같다 (pseudo)

```python
class ComputationalGraph(object):
    # ...
    def forward(inputs):
        # 1. [pass inputs to input gates ...]
        # 2. forward the computational graph:
        for gate in self.graph.nodes_topologically_sorted():
            gate.forward()
        return loss # the final gate in the graph outputs the loss
    def backward():
        for gate in reversed(self.graph.nodes_topologically_sorted()):
            gate.backward() # little piece of backprop (chain rule applied)
        return inputs_gradients
```





# Regularization

지금까지 optimization(backpropagation)을 통해 model parameters를 업데이트하는 과정을 살펴 보았다. 그렇다면 loss가 최소가 되는 model parameters는 unique할까? 이전에 봤던  SVM classifier를 예로 들어 생각해보자. SVM classifier의 loss는 아래 그림과 같이 hinge loss의 형태로 도식화 할 수 있었다.

![image-20210418001154690](/files/image-20210418001154690.png)

이러한 간단한 예시만 보아도, 다른 모든 class가 정답 scores보다 margin만큼 작다면 언제든지 loss=0이 된다는 것을 확인할 수 있다. 즉, loss를 최소화하는 model parameters는 유일하지 않다. 그렇다면 어떤 model parameters가 가장 좋을까? 이를 설명하기 위해 regularization의 개념을 알 필요가 있다.

## 개념

우리가 일반적으로 알던 loss function은 아래와 같은 형태이다.
$$
L(W)=\frac{1}{N}\sum_{i=1}^N{L_i(f(x_i,W),y_i)}=(Data\ loss)
$$
위 수식은 data loss만을 고려한 loss function이다. **data loss**란 <u>training data에 대해 model의 성능을 나타내는 지표</u>이다. 이러한 loss function에 regularization 개념을 추가하여 아래와 같이 확장할 수 있다.
$$
L(W)=\frac{1}{N}\sum_{i=1}^N{L_i(f(x_i,W),y_i)+λR(W)}=(Data\ loss)+(Regularization)
$$
여기서 λ(regularization strength)는 regularization의 정도를 나타내는 hyperparameter이며 **Regularization**은 <u>model이 training data에 대하여 너무 잘 맞는 **overfitting**을 방지하기 위해 추가되는 항</u>이다. 

### Overfitting

간단한 예시를 통해 overfitting의 개념을 알아보자. 아래와 같은 training data에 대하여 모델을 학습시킨다고 가정하자.

<img src="/files/image-20210418001823625.png" alt="image-20210418001823625" style="zoom:80%;" />

여기서 model parameters에 의한 수식은 아래와 같이 두 가지(이상)의 형태 (f1, f2)로 표현된다. f1은 loss=0인 상태이며, f2는 오히려 f1보다 loss가 더 큰 상태이다.

<img src="/files/image-20210418001924021.png" alt="image-20210418001924021" style="zoom:80%;" />

일반적인 경우 머신러닝 분야는 보다 simple한 모델을 선호하는 경향이 있다. 이는 현실계에서 대부분의 현상이 관성(원래 성질을 유지하려는 속성)이라는 속성을 갖기 때문이라 설명할 수 있다. 이러한 관점에서 봤을 때 f1보다 f2가 더 좋은 model parameters를 갖는다. 가령 아래와 같이 data가 추가될 경우를 상상할 때 이러한 관점이 더욱 명확해진다.

<img src="/files/image-20210418002140440.png" alt="image-20210418002140440" style="zoom:80%;" />

즉, model이 training data에 대해 너무 잘 맞으면(overfit, loss=0)  새로운 데이터가 들어왔을 때 오히려 성능이 떨어지는 현상을 **overfitting**현상이라고 하며, 이러한 현상을 방지하기 위해 loss function에 regularization term을 두어 오히려 training data에 대해 성능을 떨어뜨리는 방식으로 전체 성능을 높힐 수 있다.

## Examples

대표적인 regularization function에 대해서 살펴보자.



### Simple examples

비교적 간단한 형태의 regularization functions은 L1, L2, Elastic net이 있다. 예를 들어, x, w1, w2가 아래와 같을 때 더 나은 weights를 구해보자.
$$
x=[1,1,1,1]\\
w_1=[1,0,0,0]\\
w_2=[0.25, 0.25, 0.25, 0.25]
$$

#### L2 regularization

$$
R(W) = \sum_k\sum_l{W_{k,l}^2}
$$

위 예시에 대하여 L2 regularization을 적용한 결과는 아래와 같다.
$$
R(w_1)=(1-1)^2+(1-0)^2+(1-0)^2+(1-0)^2=3\\
R(w_2)=(1-0.25)^2+(1-0.25)^2+(1-0.25)^2+(1-0.25)^2=2.25
$$
L2 regularization은 weights가 <u>spread out</u>(퍼진) 형태를 선호한다. 따라서 w2의 model parameters가 더 좋은 parameter임을 알 수 있다.



#### L1 regularization

$$
R(W)=\sum_k\sum_l|W_{k,l}|
$$

위 예시에 대하여 L1 regularization을 적용한 결과는 아래와 같다.
$$
R(w_1)=|1-1|+|1-0|+|1-0|+|1-0|=3\\
R(w_2)=|1-0.25|+|1-0.25|+|1-0.25|+|1-0.25|=	3
$$
xL1 regularization에 의하면 w1와 w2의 성능은 같다.



#### Elastic net (L1+L2)

$$
R(W)=\sum_k\sum_l{βW_{k,l}^2+|W_{k,l}|}
$$

Elastic net은 L1 regularization과 L2 regularization을 합친 형태이다.



### Complex examples

비교적 복잡한 형태의 regularization functions에는 dropout, batch normalization, stochastic depth, fractional pooling등이 있다. 아래의 regularization 방식에 대해서는 추후 더 자세히 다루도록 한다.

#### Dropout

#### Batch normalization

#### Stochastic depth

#### Fractional pooling

