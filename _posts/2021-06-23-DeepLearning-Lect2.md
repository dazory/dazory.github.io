---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments # single
title: '[DL-01] Lect2. Machine Learning Basics'
excerpt: "[AIAS] 인하대학교 홍성은 교수님의 인공지능 응용 시스템 수업을 듣고 공부한 자료입니다."
date: 2021-06-23
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

<!-- AIAS-Lect2_Machine Learning Basics -->

# Machine Learning Overview

이번 페이지에서는 Machine Learning의 전반적인 개요에 대해 알아본다. 



## Machine Learning의 분류

Machine Learning(ML)은 크게 세 파트로 분류할 수 있다.

1. Supervised Learning
2. Unsupervised Learning
3. Reinforcement Learning

위 세 종류의 학습 방법을 좀 더 자세히 알아보자.

### Supervised Learning (지도학습)

**Supervised Learning**은  <u>target value가 존재하는 학습 방식</u>으로, input값에 label을 할당한다. 예를 들어, 모델을 학습시킬 때 input값으로 강아지 이미지와 함께 “Dog”라는 label을 함께 주는 경우를 의미한다. supervised learning은 예측 결과가 연속적인지 혹은 불연속적인지 여부에 따라 classification model과 regression model로 나눌 수 있다. 간단히 말하자면, <u>예측 결과가 불연속</u>적인 **classification model**의 결과값은 class 형태이고 <u>예측 결과가 연속적</u>인 **regression model**은 결과값이 value의 형태이다.

> **주의사항:** *확률은 일반적으로 연속적인 형태를 띠므로 확률을 예측하는 문제는 regression model이라고 생각하기 쉽다. regression 문제가 아닌 것을 regression model로 푸는 경우 학습이 제대로 되지 않으므로 두 개념을 확실히 구분해야한다.*
>
> 예를 들어, ‘욕설을 필터링하는 문제’를 해결하는 모델을 만든다고 생각해보자. 이 문제에서 결과값은 욕설이냐(1) 아니냐(0)로 구분될 수 있다. 즉 불연속적인 결과값을 갖는 supervised learning 문제이다. 하지만 누군가는 이를 욕설일 확률(0~1 사이의 연속적인 값)문제로 해결하고자 할 수 있다. 이렇게 문제를 해결하려는 경우 두가지 문제에 직면할 수 있다. 첫번째는, 입력 데이터에 대해 label을 어떻게 주는가에 관한 문제이다. 예를 들어 “이런 ㅅㅂ”이라는 문장이 욕설일 확률은 얼마인가? 설령 입력데이터를 특정 확률로 라벨링을 했다 할지라도 어느 범위까지를 욕설로 분류할 것인가에 대한 문제에 직면한다. 50% 이상인 경우를 욕이라고 판단한다고 하더라도 데이터가 많아짐에 따라 그러한 구분선은 또다시 무의미해질 수도 있다. 따라서 이런 문제의 경우 애초에 regression model로 풀어야 한다.

classification model과 regression model은 문제에 따라 다음과 같이 분류될 수 있다.

#### Classification Model (분류모델)

Classification Model은 <u>예측값이 discrete한 경우</u>를 의미한다. classification model은 다음과 같이 4가지 model로 나뉠 수 있다.

1. **Binary Classification**: <u>두 가지 class중 하나의 class로 예측되는 경우</u>이다. 예를 들어, 입력된 이미지가 강아지인지 혹은 고양이인지 분류하는 경우에 해당한다.
2. **Multi-class Classification**: <u>두 가지 이상의 classes 중 하나의 class로 예측하는 경우</u>이다. 예를 들어, 입력된 이미지가 강아지인지 고양이인지 혹은 자동차인지로 분류하는 경우에 해당한다.
3. **Multi-label classification**: <u>하나 이상의 classes 중 하나 이상의 classes로 예측하는 경우</u>이다. 예를 들어, 특정 영화의 카테고리를 분류할 때 로맨스이면서 공포물일 수 있는 경우에 해당한다.
4. **Imbalanced Classification**: <u>각 class의 데이터 개수가 불균형한 경우</u>의 예측이다. 예를 들어, 백인 남성의 이미지만 유독 많은 경우, 입력된 사람 이미지에 대해 백인 남성으로 분류할 가능성이 높아지는 분류 모델에 해당한다. 이 경우 모델이 적절하게 학습되지 않을 확률이 높으므로 적절한 데이터 처리를 해줘야한다.

#### Regression Model (회귀모델)

Regression Model 은 <u>예측값이 continuous한 경우</u>를 의미한다. 

> **주의사항**: logistic regression은 분류모델에 주로 사용되는 알고리즘이다. 이름에 regression이 들어간다고 착각하지 말자.



### Unsupervised Learning (비지도학습)

**Unsupervised Learning**은 discrete 값이든 continuous값이든 예측할 수 있는 범위가 있는 supervised learning과 달리 <u>target value가 없는 학습 방식</u>이다. supervised learning이 정답을 알려주는 학습법이라면, unsupervised learning은 <u>정답없이 input data의 특성만으로 특정 그룹으로 분류해주는 학습법</u>이다. 



### Reinforcement Learning (강화학습)

Reinforcement Learning의 가장 대표적인 예로는 alphaGo가 있다. 마치 당근과 채찍을 번갈아 가면서 주는 방식으로, machine이 어떠한 action을 취하면 그것에 대해 reward를 주는 식으로 학습이 진행된다. reinforcement learning에서 가장 중요한 부분은 <u>exploitation(익숙한 것)과 exploration(새로운 것) 사이의 균형</u>을 맞추는 것이다. machine이 exploitation만을 선택한다면 정답률은 100%에 가깝겠지만 실제로는 학습되고 있는 것이 없는 경우이다. 반대로 exploration만을 선택한다면 정답률이 바닥을 칠 것이다. 따라서 machine이 양쪽의 경우를 골고루 선택하여 학습하도록 하는것이 중요하다.





## Machine Learning vs. Deep Learning

머신러닝, 딥러닝, 인공지능.. 많이 들어는 봤는데 정확히 어떤 점에서 서로 다른지 헷갈린다. 이런 헷갈리는 개념에 대해서 확실히 하고 넘어가자. 먼저 AI, ML, DL의 관계를 보면 다음과 같다.

<img src="/files/image-20210416221939446.png" alt="image-20210416221939446" style="zoom:89%;" />

**Artificial Intelligence(AI, 인공지능)**은 이전까지는 인간만이 할 수 있다 여겨졌던 사고, 학습, 자기개발 등의 분야를 컴퓨터가 대체할 수 있도록 연구하는 분야이다. 기존에는 개발자가 입력과 출력사이의 관계를 직접 정의하여 명확한 규칙 아래에 모델이 돌아갔다면, AI는 자체적으로 구축한 규칙 하에 시스템이 돌아가게 된다. 즉, 이전까지는 사람에게 컴퓨터가 의존했다면 AI는 사람에 대한 의존도를 낮춘 방식이라고도 할 수 있다.

**Machine Learning(ML, 머신러닝)**은 AI의 하위 개념으로, 대량의 데이터와 알고리즘을 바탕으로 컴퓨터를 학습시켜 컴퓨터 스스로가 타겟 테스크를 수행하는 방식을 학습하는 것을 목표로 한다.

**Deep Learning(DL, 딥러닝)**은 ML의 하위 개념으로, 기계가 어떤 과제 수행을 위해 스스로 학습하도록 하는 neural networks에 기반을 둔다. ML의 경우 학습 데이터를 수동으로 제공하는 반면, DL은 분류에 사용할 데이터를 스스로 학습할 수 있다는 점에서 차이가 있다.



아래 이미지는 ML과 DL의 차이를 더욱 명확히 보여준다.

<img src="/files/image-20210416221744312.png" alt="image-20210416221744312" style="zoom:30%;" />

예를 들어 고양이 image가 입력될 때 "Cat"을 출력하도록 하는 시스템을 개발한다고 가정하자. 이때 ML의 경우 고양이에 관한 feature를 사람이 수동으로 입력해 줘야 한다. 이러한 과정을 feature extraction이라고 하는데, DL의 경우는 이러한 feature extraction 과정 마저도 machine이 알아서 처리해준다. 즉, feature extraction과정이 (user-defined) hyperparamters인지 (learnable) model paramters인지의 차이라고 볼 수 있다.

> **Model Parameters vs. Hyperparameters**
>
> model parameters는 기계가 학습할 수 있는 parameter를 의미하는 반면 hyperparamters는 사람이 직접 입력해주는 parameter를 의미한다. machine의 입장에서 사람은 초월적인(hyper) 존재이다. 따라서 machine은 hyperparameters를 건드릴 수 없고 본인이 control할 수 있는 model parameters 내에서 학습을 하게 된다.



## Data-Driven ML

ML은 많은 양의 데이터를 필요로 한다. 이때 data를 어떻게 활용하는 지(나누는 지)도 중요한 이슈이다. 다음으로 data를 활용하는 몇 가지 case에 대해 장단점을 살펴보도록 한다.

**Case1) Choose hyperparameters that work best on the data**

![image-20210416225312625](/files/image-20210416225312625.png)

모델을 학습시킬 때 내가 가진 모든 dataset을 활용하는 경우이다. 이 경우 개발자는 dataset으로 모델을 학습시킬 때 최적의 hyperparameters를 결정하게 된다. 이 경우  machine은 주어진 dataset에 대해서 최고의 성능(100%)을 발휘하게 된다. 하지만 학습에 사용되지 않은 새로운 data가 들어왔을 때 이 machine이 제대로 성능을 발휘하는 건지 확인 할 방법이 없다.

**Case2) Split data into train and test, choose hyperparameters that work best on test data**

![image-20210416225923090](/files/image-20210416225923090.png)

case 1에서 새로운 data에 대해 모델의 성능을 확인할 수 없다는 문제점에 직면했다. 따라서 이번에는 dataset을 train용과 test용으로 나누었다. 이후 train data를 통해 모델을 학습시키고 해당 결과를 바탕으로 test data를 활용하여 최적의 hyperparameters를 결정한다. case1에 비해서 더 좋은 성능을 보이겠지만, 이 경우 역시 (test data에) 최적인 hyperparameters에 대해서 새로운 데이터가 들어왔을 때 성능을 평가할 수 없다.

**Case3) Split data into train, val, and test; choose hyperparameters on val and evaluate on test**

![image-20210416231207513](/files/image-20210416231207513.png)

dataset을 train, validation, test용으로 나누었다. 먼저 train data를 통해 모델을 학습시킨다. 이후 학습된 모델을 바탕으로 validation data를 통해 최적의 hyperparameters를 결정한다. 그렇게 결정된 모델에 대하여 test data를 통해 성능을 평가한다.

> 피터드러커는 "측정할 수 없다면 더이상의 발전은 가능하지 않다."고 말했다. 이처럼 성능평가는 공학분야에서 필수이다.

**Case4) Cross-Validation: Split data into folds, try each fold as validation and average the results**

![image-20210416231507727](/files/image-20210416231507727.png)

case1~3을 통해 기본적으로 dataset을 train, validation, test용으로 나눠야한다는 것을 확인했다. 이번에는 dataset을 쪼갤 때 일종의 트릭을 사용해본다. case3의 경우 내가 나눠 준 train, validation, test data가 타당한지 잘 모르겠다. <u>우연히 data를 잘 쪼개서 결과가 잘 나온 것일 수도 있지 않는가?</u> 따라서 이번에는 validation을 바꿔줘가며 학습을 수행한다. 이때 중요한 점은, <u>test data는 한 번 결정되면 절대 학습에 사용되어서는 안된다</u>는 점이다. 이렇게 dataset을 나누는 방식은 내가 가진 dataset이 너무 작을 경우에는 유용하지만, deep learning을 하는 경우 같은 data가 너무 반복적으로 사용되어 <u>시간이 오래 걸린다</u>는 단점으로 인해 잘 사용되지는 않는다.





# Linear Regression

ML의 경우 학습 방식에 따라 supervised learning, unsupervised learning, reinforcement learning으로 나눌 수 있다. supervised learning의 일종인 linear regression에 대해서 자세히 알아본다.

> **까먹었을까봐 다시 언급**
>
> supervised learning은  <u>target value가 존재하는 학습 방식</u>으로, input값에 label을 할당한다. ( ... ) 예측 결과가 연속적인지 혹은 불연속적인지 여부에 따라 classification model과 regression model로 나눌 수 있다. (...)
>
> Linear Regression은 그 중 예측 결과가 연속적인 regression model에 해당한다.



## Linear Regression이란?

![image-20210416232130428](/files/image-20210416232130428.png)

주어진 input x에 대하여 target으로 하는 y를 예측하는 supervised learning의 일종이다. 이때 x와 y간에 linear한 관계를 갖는다는 점이 특징이다.



## 학습 과정

Linear Regression 모델에 대하여 machine이 학습하는 과정을 살펴본다. 이전에 언급했듯, 주어진 dataset에 대하여 train, validation, test용으로 나누었다고 가정한다. 

### Training

![image-20210416232337967](/files/image-20210416232337967.png)

train data를 이용하여 모델을 학습시키는 과정이다. 위 그림에서 x는 **input data**, y는 **labels(ground truth)**를 의미한다. 먼저 주어진 n개의 samples data(x,y)에 대하여 학습을 통해 <u>model parameters를 결정</u>한다. linear regression model에서 학습할 때 사용되는 모델은 아래와 같다.
$$
\hat{y_i}=θ_0+\sum_{j=1}^{d}{x_{ij}θ_j}\\
x_{ij}: input\ data, features\\
θ_j: weights\ (i.e. model\ parameters)\\
d: input\ dimension
$$
앞서 machine learning은 feature extraction 과정이 개발자에 의해 수행된다고 언급했다. 즉 주어진 input data에 대하여 개발자가 j개의 features를 뽑아 j차원 input data를 만든다. 이를 같은 차원을 갖는 weights와 곱한 결과가 바로 예측 결과가 된다.

예를 들어, 빌딩의 온도(y)를 예측하는 모델을 만든다고 가정하자. 개발자는 빌딩의 온도를 결정하는 feature(요소)로 외부 온도(x1), 습도(x2), 사람의 수(x3), 태양 노출 정도(x4)를 결정하였다. 이것은 input data이자 features에 해당한다. 이런 input data x1, x2, x3, x4에 대하여 같은 차원(1×4)을 갖는 model paramters θn (n=1,2,3,4)를 결정한다. 개발자는 x와 y간에 linear한 관계가 있다고 가정하여 linear regression model을 사용할 수 있다.

![image-20210416234023563](/files/image-20210416234023563.png)

한편, bias(θ0)를 통해 input data에 대한 모델의 의존성을 줄여줄 필요가 있다. 예를 들어, bias가 없는 경우 model은 항상 (0,0) 지점을 지나게 된다. 이러한 경우를 방지하기 위해 linear  model에서는 항상 bias term을 두어 input data에 대한 의존성을 줄여준다.



### Testing

![image-20210416232520358](/files/image-20210416232520358.png)

이후 validation data를 이용하여 모델을 예측한다. 학습시 사용되지 않은 새로운 input data x\_{n+1}과 위 과정을 통해 결정된 model parameters를 통해 x\_{n+1}에 대한 label y_{n+1}을 예측한다. 

> 위 이미지에서 y_{n+1} 위에 "^(hat)" 모양이 있는 것을 확인할 수 있다. 이것은 해당 변수가 실제 real value가 아닌 estimation value(예측값)임을 보여주는 수학적 기호이자 약속이다.

### Loss Function

이렇게 예측한 estimation y값은 loss function에 적용된다. **Loss Function**이란 나의 예측값(estimation value)와 실제값(labels, ground truth)을 비교하여 예측값이 얼마나 정확한지를 판단하는 지표이다. 예를 들어 loss function으로 **euclidean distance(유클리디안 거리공식)**을 이용할 수 있다.
$$
J(θ)=\frac{1}{n}\sum_{i=1}^{n}{(\hat{y_i}-{y_i})^2}\\
J(θ): 주어진\ model\ parameter\ θ에서의\ loss\\
\hat{y_i}: estimation\ value\\
y_i: labels(=ground truth)\\
n: number\ of\ samples
$$

### Optimization

이러한 loss function 결과를 통해 model paramters를 조정하여 더 나은 학습 결과를 도출할 수 있다. 이때 loss function 결과를 통해 model parameters를 조정하는 과정을 **optimization**이라고 한다. 예를 들어 loss function에 **linear least squares** 를 적용하여 optimization과정을 수행할 수 있다.
$$
\min_θ{J(θ)}=\frac{1}{n}\sum_{i=1}^{n}{(\hat{y_i}-{y_i})^2}
$$
한편, 주어진 문제가 convex problem인 경우에는 유일한 closed-form solution을 얻을 수 있다.

> **Convex란?**
>
> convex는 한국말로 "볼록한"이라는 뜻이다. ML분야에서 convex problem는 아래로 볼록한 모델(가령 y=x^2)을 떠올리면 된다. 이 경우 minimum value가 유일하므로 ML하기 가장 좋은 경우라고 할 수 있다.



### 총 정리

위에서 언급한 Training-Testing-Loss Function-Optimization의 과정을 하나의 그림으로 표현하면 다음과 같다.

<img src="/files/image-20210416234539248.png" alt="image-20210416234539248" style="zoom:80%;" />





# Nearest Neighbors (최근접 이웃)

**Nearest Neighbors**는 supervised learning 관련된 알고리즘의 일종으로, 가장 고전적이며 직관적인 알고리즘이다. nearest neighbors 알고리즘은 데이터 간의 거리를 통해 입력 데이터는 이와 가장 가까운 데이터와 같은 label을 갖는다고 판단한다.

<img src="https://www.coryjmaklin.com/media/k-nearest-neighbor-python-1.png" alt="K Nearest Neighbor Algorithm In Python - Blog by Cory Maklin" style="zoom:30%;" />

nearest neighbor의 경우 학습 과정은 다음과 같다. 먼저 모든 training data와 labels를 memory에 저장한다. 이후 새로운 데이터(validation data)가 들어왔을 때 이와 모든 training data를 비교하여 가장 거리가 짧은 label을 estimation value로 도출한다.

![image-20210417002308762](/files/image-20210417002308762.png)

한편, 가장 가까운 점 하나로만 예측하자니 outlier가 있는 경우 예측률이 떨어진다. 따라서 K개의 가까운 점을 통해 결과를 도출하는 nearest neighbor인 **K-nearest neighbors**을 사용하기도 한다.



## Distance Metric

image data는 행과 열을 갖는 metric형태이다. 이러한 metric 형태의 데이터에 대하여 data간의 거리를 판별하는 방법을 **distance metric**이라고 한다. 두 data간 거리를 구하는 방식은 다양한데, 대표적인 예로 L1과 L2 distance가 있다.

### L1 (Manhattan) distance

L1 distance를 수식으로 표현하면 아래와 같다.
$$
d_1(I_1,I_2)=\sum_{p}{|I_1^p-I_2^p|}\\
I: image\ metric\\
p: pixel\\
d: distance
$$
<img src="/files/image-20210417002712816.png" alt="image-20210417002712816" style="zoom:80%;" />

L1 distance의 입력(I)와 출력(d)간 관계를 그림으로 표현한 것은 아래와 같다.

![image-20210417002638881](/files/image-20210417002638881.png)

### L2 (Euclidean) distance

$$
d_2(I_1,I_2)=\sqrt{\sum_{p}{(I_1^p-I_2^p)^2}}
$$

L2 distance의 입력(I)와 출력(d)간 관계를 그림으로 표현한 것은 아래와 같다.

![image-20210417003006005](/files/image-20210417003006005.png)

### L1 distance vs. L2 distance

예를 들어, 1-Nearest Neighbors 알고리즘에 대하여 L1, L2 distance를 적용한 경우 결과는 아래와 같다.

<img src="/files/image-20210417003100252.png" alt="image-20210417003100252" style="zoom:80%;" />

사실상 결과에 있어서 큰 차이는 없다.

> 하지만 일반적인 경우 L2 distance를 더 많이 사용한다.



## Hyperparameters

nearest neighbors알고리즘에서의 hyperparameters는 무엇일까?

> **까먹었을까봐 다시 언급**
>
> model parameters는 기계가 학습할 수 있는 parameter를 의미하는 반면 hyperparamters는 사람이 직접 입력해주는 parameter를 의미한다.

nearest neighbors 알고리즘에서 몇 개의 neighbors에 대해 거리를 조사할 지(k), 어떤 distance 공식을 사용할지(d1 or d2?)등이 바로 hyperparameters에 해당한다. 사람이 정해주는 이러한 hyperparameters에 따라 모델의 성능이 크게 달라질 수 있음에 유의해야 한다.



## Speed Problem

nearest neighbors는 속도 측면에서의 치명적인 단점으로 인해 image classification 문제에서 잘 사용되지 않는다. 예를 들어 N개의 examples에 대해 nearest neighbors 알고리즘을 사용하는 경우를 생각해보자. nearest neighbors의 training은 단순 memory 저장밖에 없다. 따라서 O(1)의 시간이 소요된다. 반면 predict과정에서는 저장된 N samples에 대하여 일일히 비교해야 하므로 O(N)의 시간이 소요된다. ML에서는 학습시에 걸리는 시간보다는 실제 예측을 하는데에 걸리는 시간을 최대한 줄이는 것이 주요 목적이다. 따라서 이러한 관점에서 봤을 때 nearest neighbors 알고리즘은 실용적이지 않다.



## Semantic Gap Problem

Image classification 문제는 computer vision분야의 핵심 과제이다. nearest neighbors 알고리즘이 image classification 문제에 적용되는 경우 semantic gap 문제가 발생할 수 있다.

사람이 볼 수 있는 image의 형태와 computer가 인식할 수 있는 image의 형태에는 차이가 있는데, 이러한 차이를 **semantic gap**이라고 한다. 예를 들어 아래와 같이 사람이 봤을 때의 고양이 사진은 컴퓨터 입장에서는 0~255사이의 숫자로 표현되는 8-bit unsigned integer로 인식된다.

<img src="/files/image-20210417000344726.png" alt="image-20210417000344726" style="zoom:80%;" />

예를 들어, RGB 이미지 포맷에서 각 pixel의 값은 red, green, blue channel에 대한 밝기 값을 의미하는데, 이러한 밝기값을 feature로 두고 nearest neighbors 알고리즘으로 학습시킨다면 semantic gap으로 인해 다음의 경우에서 제대로된 결과를 도출할 수 없다.

1. **Viewpoint variation**

   같은 물체를 보더라도 카메라를 살짝 이동한 경우(전체 pixel값의 변화)

2. **Background Clutter**

   <img src="/files/image-20210417001539020.png" alt="image-20210417001539020" style="zoom:80%;" />

   물체가 배경과 비슷한 색을 가진 경우

3. **Illumination**

   ![image-20210417001646401](/files/image-20210417001646401.png)

   이미지에서 그림자나 역광이 발생하는 경우

4. **Occlusion**

   ![image-20210417001740624](/files/image-20210417001740624.png)

   물체가 어떤 장애물에 의해 일부가 가려진 경우

5. **Intra-class variation**

   <img src="/files/image-20210417001934609.png" alt="image-20210417001934609" style="zoom:80%;" />

   같은 label("cat")을 가졌지만 색상이 다른 경우

위의 경우는 모두 단순 nearest neighbors 알고리즘으로 밝기값을 비교하는 것 만으로는 판별이 불가능하다.



## 결론

위에서 언급하였듯이, pixel distance를 이용한 k-nearest neighbors은 매우 느린 test time 문제(speed problem)와 pixels에 대한 distance metrics의 효용성 문제(semantic gap problem)로 인해 image classification 문제에서는 사용되지 않는다. 아래 이미지는 여러 이미지에 대해 k-nearest neighbors 알고리즘을 사용한 결과이다.

<img src="/files/image-20210417004319299.png" alt="image-20210417004319299" style="zoom:80%;" />

이처럼 k-nearest neighbors은 전혀 다른 이미지(boxed, shifted, tinted)에 대해 original 과 같은 distance를 갖는다는 치명적인 문제가 있다.





# Dimensionality Reduction/Expansion

단순하게 생각했을 때 input data의 차원이 높아질 수록(features의 개수가 많아질수록) 모델의 성능이 더 좋아지지 않을까 생각할 수 있다. 하지만 결과적으로 봤을 때 input data의 차원이 높아질수록 원하는 결과 도출을 위해서 더 많은 데이터가 필요해진다. 이러한 개념은 curse of dimensionality로 설명할 수 있다.

## Curse of dimensionality (차원의 저주)

**Curse of dimensionality**는 데이터 학습을 위한 <u>차원이 증가함에 따라</u>, 해당 차원을 표현할 수 있는 데이터가 요구되어 <u>더 많은 학습 데이터를 필요로하는 현상</u>을 말한다. 예를 들어, 4개의 data points가 있을 때 예측이 가능하다고 가정해보자.

<img src="/files/image-20210417005706801.png" alt="image-20210417005706801" style="zoom:80%;" />

1차원인 경우 4개의 data points만으로 충분히 예측가능하다. 하지만 2차원으로 확장됨에 따라 최소 4^2개의 data points가 필요하게 되고, 차원이 확장됨에 따라 요구되는 data points의 개수가 늘어남을 자연스럽게 연상할 수 있다. 이러한 curse of dimensionality를 그래프로 표현하면 다음과 같다.

![image-20210417005847451](/files/image-20210417005847451.png)

x축은 dimensionality(features의 개수)를, y축은 classifier performance(성능)을 나타낸다. 초기에는 차원이 증가할 수록 성능이 향상되는듯 보이지만, 어느 지점을 넘어서게 되면 input data 양의 한계로인해 오히려 성능이 떨어짐을 확인할 수 있다.

이러한 curse of dimensionality를 고려하여 모델을 설계하기 위해 다양한 차원 축소/확장 방법이 제시되었다.

## Dimensionality Reduction

학습데이터가 한정되어있을 때 차원을 줄일 목적으로 다양한 방법이 제안되었다.

### PCA (Principal Component Analysis)

![image-20210417010214788](/files/image-20210417010214788.png) <img src="/files/image-20210417010653758.png" alt="image-20210417010653758" style="zoom:80%;" />

**Principal Component Analysis(PCA)**는 분산을 최대한 보존하며 <u>서로 직교하는 새로운 기저</u>를 찾아 고차원 공간의 표본들을 선형 연관성이 없는 저차원 공간으로 변형하는 기법이다. 이때 분산을 최대한 보존한다는 말의 뜻은, 데이터의 변동성이 가장 큰 방향으로 축을 설정하여 데이터를 표현한다는 의미이다. 



### LDA (Linear Discriminant Analysis)

![image-20210417010845623](/files/image-20210417010845623.png)<img src="/files/image-20210417010917069.png" alt="image-20210417010917069" style="zoom:80%;" />

**Linear Discriminant Analysis(LDA)**는 PCA보다 분류에 최적화된 차원축소 기법이다. class가 최대한으로 분류되도록 축을 분리한다는 특징이 있다.



### t-SNE (Stochastic Neighbor Embedding)

![image-20210417011203838](/files/image-20210417011203838.png)

**Stochastic Neighbor Embedding(SNE)**은 고차원의 원 공간에 존재하는 데이터 x의 이웃간 거리를 최대한 보존하는 방법이다. 즉, 확률적으로 가장 유사한 축으로 분리한다는 특징이 있다.



## Dimensionality Expansion

한편, 입력 데이터가 충분한 경우, 차원 확장을 통해 결과를 더 개선시킬 수 있다. 





# Linear Classifier

앞서 supervised learning의 일종인 linear regression에 대해서 알아보았다. 이번에는 불연속적인 예측결과를 다루는 classification model 중 가장 대표적인 linear classification에 대해서 알아본다.

> **까먹었을까봐 다시 언급**
>
> supervised learning은  <u>target value가 존재하는 학습 방식</u>으로, input값에 label을 할당한다. ( ... ) 예측 결과가 연속적인지 혹은 불연속적인지 여부에 따라 classification model과 regression model로 나눌 수 있다. (...)
>
> Linear classification은 그 중 예측 결과가 불연속적인 classification model에 해당한다.



## Linear Classification이란?

![image-20210416232130428](/files/image-20210416232130428.png)

주어진 input x에 대하여 target으로 하는 y를 예측하는 supervised learning의 일종이다. 이때 x와 y간에 linear한 관계를 갖는다는 점이 특징이다. linear regression과 다른 점은 출력이 연속적인 value가 아닌 label의 형태(가령 "cat", "dog", "ship")로 나타난다는 점이다.



## Linear Classifier

value가 아닌 class를 어떻게 기계적으로 표현할 수 있을까? linear classifier는 각 class별로 score를 두어 가장 높은 score가 estimation value가 되게끔하여 불연속적인 결과값을 도출한다. 예를 들어 아래 이미지는 input image x에 대해 고양이인지, 강아지인지, 배인지를 구분하는 linear classifier를 보여준다.

![image-20210417011850007](/files/image-20210417011850007.png)

가령 cat score=1000, dog score=200, ship score=100인 경우 classifier는 결과로 "cat"을 도출한다. 이 경우 학습이 잘 된 모델이라 볼 수 있다. 반면 cat score=100, dog score=800, ship score=300인 경우에는 결과가 "dog"이므로 예측이 틀린 경우라 할 수 있다.



## 학습 과정

Linear Classification 모델에 대하여 machine이 학습하는 과정을 살펴본다. 이전에 언급했듯, 주어진 dataset에 대하여 train, validation, test용으로 나누었다고 가정한다. 

### Feed-forward

input image가 가령 2×2의 size를 갖는다고 할 때, 이를 flatten하여(길게 펴서) 4×1의 input data로 만들 수 있다. 결과로 3개의 class에 대한 scores를 도출해야 하므로 weights는 3×4의 사이즈를 갖게 된다. linear 공식을 이용하여 아래와 같이 각 class별 **scores**를 도출한다.
$$
f(X,W)=XW+b\\
X: input\ data\\
W: model\ parameters(weights)\\
b: biase\\
f: scores
$$

### Loss Function

feed-forward의 결과(f, scores)와 target scores를 비교하는 loss function을 이용하여 모델의 성능을 평가한다. 이때 target scores는 정답인 class의 score값이 최대인 scores를 의미한다.



### Back Propagation

Loss function의 결과를 바탕으로 모델을 최적화(optimization)하기 위해 back propagation을 수행한다. 이러한 과정으로 얻어진 결과를 weights에 update하여 모델에 반영한다.



### 총 정리

위에서 언급한 Feed-forward - Loss Function - Back Propagation의 과정을 하나의 그림으로 표현하면 다음과 같다.



![image-20210417012403686](/files/image-20210417012403686.png)

## Linear Classifier의 한계

Linear classifier는 입력과 출력이 linear관계를 갖는 모델에 한해서만 잘 동작한다. 가령 아래의 경우에 대해서는 linear classifier만으로 판별이 불가능하다.

![image-20210417013205526](/files/image-20210417013205526.png)

따라서 Nonlinearity한 관계를 갖는 문제를 해결할 수 있는 모델이 필요했다. nonlinearity는 layer 뒤에 activation function을 추가함에 따라 달성되는데 이와 관련해서는 이후에 좀 더 자세히 살펴보도록 한다.





# SVM Classifier

**Support Vector Machine  Classifier (SVM classifier)** 는 주어진 dataset을 바탕으로, 새로운 data가 어떤 class에 해당하는지를 판단하는 비확률적 binary linear classification model을 만든다. SVM은 linear classification과 더불어 nonlinear classification에서도 사용될 수 있다. 아래의 예시를 통해 SVM classifier에 대해 자세히 알아보자. f(x,w)을 통해 각각의 고양이, 자동차, 개구리 이미지에 대해서 위와 같은 scores를 도출했다고 가정하자.

![image-20210417013602522](/files/image-20210417013602522.png)

일반적으로 multiclass에 대한 total loss는 아래와 같이 개별 loss의 평균을 통해 계산할 수 있다.
$$
L=\frac{1}{N}\sum_i{L_i(f(x_i,W),y_i)}\\
L: total\ loss\\
N: number\ of\ samples\\
L_i: the\ loss\ between\ f(x_i,W)\ and\ y_i
$$
이때 <u>SVM Classifier는 아래와 같은 loss function을 활용</u>한다.
$$
L_i=\sum_{j≠y_i}\max{(0, w_j^Tx_i-w_{y_i}^Tx_i+△)}\\
$$
가령, 고양이 이미지에 대한 loss를 구한다고 할 때, 아래와 같이 계산될 수 있다.
$$
L_1=\sum_{j≠y_i}{\max{(0, w_j^Tx_1-w_{y_1}^Tx_1+△)}}\\
=\max{(0, 5.1-3.2+△)}+\max{(0, -1.7-3.2+△)}\\
=\max{(0, 1.9+△)}+\max{(0, -4.9+△)}\\
$$
이때 △(delat)=1이라 하면 아래의 결과를 얻는다.
$$
L_1=\max{(0, 1.9+1)}+\max{(0, -4.9+1)}\\
=\max{(0, 2.9)}+\max{(0, -3.9)}\\
=2.9+0=2.9
$$
따라서 고양이 이미지에 대한 loss는 2.9가 된다. 같은 방식으로 자동차 이미지, 개구리 이미지에 대한 loss를 구하면 각각 0과 12.9가 된다.

![image-20210417014927632](/files/image-20210417014927632.png)

따라서 이때의 총 loss L은 (2.9 + 0 + 12.9)/3 = 5.27이다.

## Hinge  Loss

SVM loss 공식은 아래와 같다.
$$
L_i=\sum_{j≠y_i}\max{(0, w_j^Tx_i-w_{y_i}^Tx_i+△)}
$$
이때 delta(△)값에 따라 loss function 수행 결과가 달라진다. 아래의 그림은 delta=1인 경우, delta와 loss간의 상관관계를 잘 보여준다.

![image-20210417015222732](/files/image-20210417015222732.png)

delta값은 얼마나 더 엄격한 모델을 만들 것인가와 관련되어 있다. 가령, 고양이 이미지에 대해 cat score=3.2, car score=3.2인 경우 delta=0이면 loss=0이 된다. 하지만 우리의 목적은 고양이 이미지에 대해 확실하게 "cat"이라 분류하는 것이므로 delta값을 두어 cat score와 other scores간에 확실한 차이가 있는 경우에만 loss=0(정답)이라고 판별하는 것이다.



## Example Code

```python
def L_i_vectorized(x, y, W):
    scores = W.dot(x) # First calculate scores
    margins = np.maximum(0, scores - scores[y] + 1) # Then calculate the margins s_{j} - s_{y_i} + 1
    margins[y] = 0 # only sum j is not y_i, so when j=y_i, set to zero.
    loss_i = np.sum(margins) # sum across all j
    return loss_i
```





#  Softmax Classifier

scores기반 SVM Classifier은 결과값(score)의 범위에 제한이 없다는 단점이 있었다. 따라서 score가 음수일 수도 있고 아주 큰 값일 수도 있다. 이러한 단점을 개선하여 0과 1사이의 확률값으로 scores를 표현한 것이 바로 **softmax classifier**이다. 아래의 예시를 통해 자세히 살펴보자.

![image-20210417020409533](/files/image-20210417020409533.png)



## 계산 과정

1. **Unnormalized log-probabilities / logits**

   위와 같이 f(X,W)에 의해 결과값(scores)가 도출되었을 때 이를 unnormalized log-probabilities라고 한다.

2. **unnormalized probabilities**
   $$
   exp(s=f(x_i,W))
   $$
   ​		먼저 scores에 exp()를 취하여 음수값을 갖지 않도록 한다.

3. **Probabilities**
   $$
   P(Y=k|X=x_i)=\frac{\exp^s{k}}{\sum_j{\exp^s{j}}}
   $$
   이후 unnormalized probabilities를 normalize하여 총 합이 1인 일종의 확률값으로 표현한다.

최종적으로 softmax classifier는 아래의 loss function을 갖는다고 말할 수 있다.
$$
L_i = -\log{P(Y=y_i|X=x_i)}=-\log{\frac{\exp^s{k}}{\sum_j{\exp^s{j}}}}
$$
위 예제의 경우 고양이 이미지에 대한 cat, car, frog의 loss를 계산하면 아래와 같다.
$$
L_1=-\log{\frac{\exp{3.2}}{\exp{3.2}+\exp{5.1}+\exp{-1.7}}}\\
=-\log{\frac{24.5}{24.5+164.0+0.18}}\\
=-\log{0.13}=2.04
$$
이때 주의할 점은, <u>정답("Cat")이 아닌 다른 class에 대한 probabilities는 전혀 고려하지 않는다</u>는 점이다. 이미 softmax의 loss function 자체에 other class가 고려되어있으므로 모든 class에 대한 loss의 평균값을 계산하여 total loss를 구할 필요 없이, 단순히 정답 class에 대한 loss만을 구하면 그것이 total loss가 된다는 뜻이다.

