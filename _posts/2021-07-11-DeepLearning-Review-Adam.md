---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

use_math: true
layout: post-with-comments
title: '[REVIEW-01] Adam: A Method for Stochastic Optimization'
excerpt: "[논문 리뷰] Adam 논문 리뷰"
date: 2021-07-11
last_modified_at: 2021-07-11
categories:
  - Deep Learning
tags: 
   - [Deep Learning, optimizer]

comments: true
share : false
---

<!-- [REVIEW-01] Adam: A Method for Stochastic Optimization -->

# 배경

training samples가 너무 많은 경우, 모든 training samples 각각에 대한 loss를 계산한 뒤 평균을 내는 방법은 연산량이 너무 많아 비효율적이다. 이러한 배경에서 **stochastic gradient descent(SGD)**가 탄생했다. 하지만 SGD는 gradient 기반 optimizer이므로 local minimum에 갇힐 수 있다는 문제점을 가지고 있다. 

<img src="/files/image-20210418203310926.png" alt="image-20210418203310926"/>

위 이미지는 local minimum(혹은 saddle point)에 갇히는 경우를 나타낸다. gradient값이 일시적으로 0인 경우, gradient descent가 최적화를 중지한다는 문제를 해결하고자 여러가지 optimizers가 제안되었다.

아래 이미지는 여러가지 optimizers의 장단점을 잘 보여줘서 가져와봤다.

<img src="/files/image-20210418210542140.png" alt="image-20210418210542140"/>

이 중 Adam optimizer는 "무엇을 써야할 지 모른다면 Adam을 써라!" 라는 말이 있을 정도로 대부분의 상황에서 무난하게 잘 적용된다. 이번 포스팅에서는 이 중 Adam optimizer에 관한 논문을 읽고 리뷰해보려고 한다.


<br>


# 리뷰

## 연구 문제

<span style="color:gray">이 저자가 바라보는 문제는 무엇인가? 무엇을 해결하고 싶었던 걸까?</span>

모수(parameter)란 모집단의 특성을 대표하는 수치이다. 모수는 보통 알려져 있지 않으므로 과학과 공학 분야에서 다루는 대부분의 최적화 문제에서는 모수를 추정하여 문제를 해결할 수밖에 없다. 따라서 최적화 문제에서 모수가 불안정하다는 특성으로 인해 objective functions은 First order method로 다루어야 한다. 이러한 stochastic gradient 기반 최적화를 위해 기존에는 다양한 optimizers가 제시되었다(SGD, AdaGrad, RMSProp 등). 이 논문에서는 <u>기존의 optimizers(AdaGrad, RMSProp)의 장점을 취하고 단점을 보완하는 새로운 stochastic optimization method로 Adam(Adaptive Momen)</u>을 제안한다.

먼저, 기존의 **SGD**방식은 <span style="background-color:rgba(255, 255, 102, .5)">모든 parameter에 대하여 같은 learning rate를 곱한다는 단점</span>이 있다. 이러한 특징은 ill-condition일 경우 학습효율의 저하로 이어진다. 이러한 단점을 극복하기 위해 과거의 gradient를 참고하는 gradient accumulation variable(이하 GAV)을 도입한 **AdaGrad** 방식이 제안되었다. AdaGrad 방식은 변화량이 큰 parameter에 대해 GAV로 나누어 step size를 작게 변화시킨다. 따라서 sparse gradients에 대해 잘 작동한다. 하지만 <span style="background-color:rgba(255, 255, 102, .5)">iteration이 증가함에 따라 GAV의 값이 증가하여 step size가 너무 작아진다는 한계</span>가 있다. **RMSProp** 방식은 exponential moving average개념을 도입함으로써 이러한 한계를 보완하였다. **Adam**은 <span style="background-color:rgba(255, 255, 102, .5)">이러한 RMSProp 방식에 momentum을 조합하고 bias-correction 개념을 추가하여 확장한 방법이다. </span>


<br>


## 방법
<span style="color:gray">문제를 해결하기 위해서 어떤 방법을 이용했나? 방법을 만들 때 참고한 이전 방법이 있었나?</span>

Adam은 RMSProp와 momentum을 조합한 뒤 bias-correction 개념을 추가한 방식이다. 다음 [참고1]은 RMSProp 알고리즘을 나타낸다.

<img src="/files/REVIEW-01-Adam-0001.PNG" alt="REVIEW-01-Adam-0001"/>


momentum은 현재 parameter의 업데이트에 이전 gradient의 값을 반영한다. Adam은 RMSProp방식과 momentum을 조합하였다. [참고1]의 4번 과정에 의하면 r값을 이전 값들을 축적한 r와 g⊙g에 대한 exponential moving average의 값으로 업데이트 한다. 이는 Adam에서 2nd moment estimate이라고 해석할 수 있다. 즉, Adam에서는 exponential decay rates β_1,β_2와 moment variable m_t, g_t (각각 1st, 2nd moment variable)가 있다고 할 때, [참고1]의 4번 과정이 아래와 같이 수정된다.

<img src="/files/REVIEW-01-Adam-0002.PNG" alt="REVIEW-01-Adam-0002"/>


Adam algorithm에서는 초기에([참고1]의 1번 과정에서) moment variable m_t, g_t를 각각 0으로 초기화해준다. 이는 학습 초기 가중치가 0으로 편향되는 문제를 야기할 수 있다. 따라서 아래와 같이 bias-correction 개념을 적용한다.

<img src="/files/REVIEW-01-Adam-0003.PNG" alt="REVIEW-01-Adam-0003"/>


한편 AdaGrad의 parameter 업데이트 공식은 아래와 같다. (ε: global learning rate, g: gradient, δ: small constant. r: gradient accumulation variable) 

<img src="/files/REVIEW-01-Adam-0004.PNG" alt="REVIEW-01-Adam-0004"/>


Adam은 parameter를 업데이트 하는 방법으로 AdaGrad의 업데이트 공식을 이용했다. Adam의 parameter 업데이트 공식은 아래와 같다. (α: step size, (m_t ) ̂: biased 1st moment estimate, ε: small constant(약 10^(-8)), (v_t ) ̂: biased 2nd moment estimate)

<img src="/files/REVIEW-01-Adam-0005.PNG" alt="REVIEW-01-Adam-0005"/>


위의 설명을 참고하여 [참고1]의 RMSProp 알고리즘에 반영하면 아래와 같은 Adam algorithm을 도출할 수 있다.

<img src="/files/REVIEW-01-Adam-0006.PNG" alt="REVIEW-01-Adam-0006"/>


## 분석

<span style="color:gray">이 방법이 좋다는 것을 어떻게 증명하였나?</span>

논문에서는 도입한 1st, 2nd moment estimation을 통해 최적의 parameter를 구하는 과정을 SNR의 개념으로 쉽게 설명한다. (m_t ) ̂와 (v_t ) ̂는 확률적 기대값이 각각 gradient의 1차, 2차 moment이다. 이는 gradient에 대한 모평균, 모분산을 의미한다. 이러한 논리 과정을 통해 논문에서는 (m_t ) ̂/√((v_t ) ̂ )을 signal-to-noise로 해석하여 [참고2]의 5번 과정에 대한 직관적인 해석을 돕는다.


Adam은 현재 parameter값 근처에 trust region을 생성한다. 이를 통해 step size에 대한 적절한 scale을 미리 알 수 있어서 Adam 방식을 신뢰할 수 있게 된다. 
논문에서는 (1-β_1 )>√(1-β_2 )인 경우와 그렇지 않은 경우, 두 가지 경우에 대해 effective step size의 magnitude값(|∆_t|)을 살펴봄으로써 수식적으로 증명한다.


Adam 방법이 실제 상황에서 잘 적용되는지를 증명하기 위해 저자는 실험적인 방식을 이용한다. 널리 사용되는 machine learning models에 대해 Adam optimizer를 적용한 결과를 다른 stochastic optimizers를 적용한 결과와 비교한 결과, Adam이 다른 방법에 비해 좋은 성과를 보였다. 또한 VAE(Variational Auto-Encoder)을 서로 다른 β_1, β_2에 대해 학습시킨 결과를 통해 β_2의 값이 커질수록(1에 가까워질 수록) bias-correction 개념의 도입으로 인한 loss값 감소의 효과가 커짐을 증명하였다.


<br>


# References

1. Adam: A Method for Stochastic Optimization : [https://arxiv.org/abs/1412.6980](https://arxiv.org/abs/1412.6980)

2. 자습해도 모르겠던 딥러닝, 머리속에 인스톨 시켜드립니다. : [https://www.slideshare.net/yongho/ss-79607172](https://www.slideshare.net/yongho/ss-79607172)
