---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments
title: 'Tradeoffs in data augmentation: an empirical study'
excerpt: "Tradeoffs in data augmentation: an empirical study"
date: 2023-08-31
last_modified_at: 2023-08-31
categories:
  - deep_learning-paper_review
tags: 
  - [Data Augmentation, Metric]

use_math: false
comments: true
share : false
---

## Introduction

### Contributions

* Introduce interpretable and easy-to-compute measures: ***Affinity*** and ***Diversity***.
  * ***Affinity*** quantifies <u>how much an augmentation shifts the training data distribution</u> from that learned by a model.
  * ***Diversity*** quantifies <u>the complexity of the augmented data</u> with respect to the model and learning procedure.
* Show that <u>the best augmentation strategies jointly optimize the two</u> in the Affinity-Diversity plane.
* Performance can be improved and training accelerated by <u>turning off regularization</u> at an appropriate time (the connection between augmentation and other familiar forms of regularization).
* Show that performance is only improved when a transform increases <u>the total number of unique training examples</u>.



### Background

Data augmentation에 대한 두 가지 철학이 있음:

1. (Distributional similarity)"효과적인 augmentation은 original data와 유사한 distribution을 갖는다!!"

   :link: [1]

   ⇒ The heuristic of incurring minimal distribution shift from the training data: realistic samples

2. (Diversity) "효과적인 augmentation은 모델"

   :link: cutout, SpecAugment, mixup, etc.

   ⇒ Unrealistic distortions

Distributional similarity와 diversity의 관계를 알아보자!



## Methods

### Affinity: a simple metric for distribution shift

**Definition.**

*Affinity* is a simple metric to quantify **how augmentation shifts data with respect to the decision boundary** of the clean baseline model.

* Q. 학습된 모델은 종종 training data 분포에 민감하다?

  A. No! 학습 과정이 동적이므로, 입력 데이터 자체의 분포가 문제라기 보다는, 모델의 implicit biases가 성능에 영향을 미치는 것이다!

따라서, Affinity는 입력 데이터 분포 자체로부터 측정되면 안되고, 모델의 출력으로부터 측정되어야 한다.
이러한 배경을 바탕으로, Affinity $\mathcal{T}$는 다음과 같이 정의된다.
$$
\mathcal{T}[a; m; D_\text{val}] = \mathcal{A}(m, D'_\text{val}) / \mathcal{A}(m, D_\text{val}),
$$
where

* $D_\text{train}, D_\text{val}$: Training and validation datasets drawn IID from the same clean data distribution.
  $D'_\text{val}$: Validation datasets drawn from $D_\text{val}$ by applying a stochastic augmentation strategy $a$. 
* $m$: A model trained on $D_\text{train}$.
* $\mathcal{A}(m, D)$: The model's accuracy when evaluated on dataset $D$.

i.e. For the model $m$, which trained on clean data $D_\text{train}$, Affinity is the ratio between 

* $\mathcal{A}(m, D'_\text{val})$: the validation accuracy of a model tested on an augmented validation set.
* $\mathcal{A}(m, D_\text{val})$: the validation accuracy of a model tested on clean validation set.

i.e. $D_\text{train}$에서 모델을 학습한 뒤, $D'_\text{val}$에서의 validation accuracy와 $D_\text{val}$에서의 validation accuracy의 비율로 정의된다.



**Properties.**

* $\mathcal{T}=1$ means no shift
* $\mathcal{T}<1$ means that the augmented data is OOD for the model.

> 일반적으로 $\mathcal{A}(m,D'_\text{val}) < \mathcal{A}(m,D_\text{val})$ 이므로 $\mathcal{T}<1$임. 근데 data shift가 만일 없다고 치면, $\mathcal{A}(m,D'_\text{val}) \approx \mathcal{A}(m,D_\text{val})$니까 Affinity값이 $1$에 가까워질 것임.



**Visualization.**

<img src="/home/dshong/typora/resources/tradeoffs_in_data_augmentation__an_empirical_study/image-20230504172319399.png" alt="image-20230504172319399" style="zoom:80%;" />

회색 x와 o는 각각 two-class train data를 의미한다. 결정 경계는 x와 o의 중간 x shift = 0에 형성되는 것을 예상할 수 있다. (참고: 주어진 augmentation은 Gaussian의 mean shift를 통해 data shift를 제어한다. x shift는 classification task와 관련있는 방향으로의 입력 데이터에서의 data shift이고, y shift는 classification task와 무관한 방향으로의 입력 데이터에서의 data shift이다.)

* Affinity

  Data shift가 모델의 의사결정 경계를 기준으로 하는 경우에만 affinity값이 변한다.
  i.e. 모델의 의사결정 경계($x_\text{shift}=0$)로부터 멀어질수록, Affinity값이 감소한다 (⇔ more shift in the augmented data)

* KL Divergence

  Classification과 무관한 방향으로 데이터가 이동하는 경우에도 measurement값이 변한다.
  i.e. 모델의 의사결정 경계에 의존적이지 않고, classification과 무관한 방향으로 발생한 data shift에도 값이 변함. 



### Diversity: a measure of augmentation complexity

**Definition.**

*Diversity* is a metric to quantify the intuition that augmentations prevent models from over-fitting by **increasing the number of samples in the training set**.

Diversity $\mathcal{D}$는 다음과 같이 정의된다.
$$
\mathcal{D}[a; m; D_\text{train}] := \mathbb{E}_{D'_\text{train}}[L_\text{train}] / \mathbb{E}_{D_\text{train}}[L_\text{train}],
$$
where

* $L_\text{train}$: the training loss of a model trained on clean data $\mathcal{D}_\text{train}$.

i.e. For the model $m$, which trained on augmented data $D'_\text{train}$, Diversity is the ratio between 

* $\mathbb{E}_{D'_\text{train}}[L_\text{train}]$: the final training loss of a model trained on with a given augmentation $D'_\text{train}$.
* $\mathbb{E}_{D_\text{train}}[L_\text{train}]$: the final training loss of the model trained on clean data $D_\text{train}$.

i.e. $D'_\text{train}$에서 모델을 학습한 final training loss와, $D_\text{train}$에서 모델을 학습한 final training loss의 비율로 정의된다.



**Properties.**

* $\mathcal{D} = 1$ means same loss ⇔ no diversity

* $\mathcal{D}>1$ means more loss in augmented data ⇔ more diveristy

* Pros. It can capture model-dependent elements.

* Cons. The computational cost of evaluation is expensive. 

  * 뒤에서 대체로 더 적은 evaluation cost를 갖는 두 개의 alternative measures of diversity를 소개할 예정.

    1. 학습 초기 지점에서 이 metric을 사용하는 방법: 일반적으로 학습 초기의 training loss가 학습이 종료될 때의 값과 연관되어있음을 발견했다.

    2. The entropy of the transformed data $\mathcal{D}_\text{Ent}$: "augmentations with more degrees of freedom perform better"에서 영감 받음. 

       For discrete transformations, the conditional entropy of the augmented data is as follow:
       $$
       \mathcal{D}_\text{Ent} := H(X'|X) = -\mathbb{E}_X[{ \sum_{x'}{p(x'|X)\log{(p(x'|X))}} }],
       $$
       where $x\in X$: a clean training image, $x'\in X'$: an augmented image.

       이 metric은 어떠한 학습이나 model architecture에 관한 지식 없이도 평가할 수 있는 장점을 갖지만, continuous transformations에 대해 측정하는 것은 좀 더 까다롭다.





## Results

### Augmentation performance is determined by both affinity and diversity

<img src="/resources/tradeoffs_in_data_augmentation__an_empirical_study/image-20230505151832242.png" alt="image-20230505151832242" style="zoom:80%;" />

* (a) Affinity와 Diversity를 각각 지표로 사용한 경우.

  * (좌) Affinity만으로는 accuracy를 완전히 예측할 수 없다. High test accuracy를 가짐에도 low affinity (more data shifted)를 가질 수 있기 때문.
  * (우) Diversity만으로는 accuracy를 완전히 예측할 수 없다. High accuracy를 가짐에도 low diversity (decreased the number of samples)를 가질 수 있기 때문.

* (b) & (c) Affinity와 Diversity를 함께 고려할 때, accuracy를 더욱 잘 예측할 수 있다.

  * Diversity가 고정되어 있을 때, Affinity가 커질수록(→) relative accuracy가 증가한다.
  * Affinity가 고정되어 있을 때, Diversity가 커질수록(↑) relative accuracy가 증가한다.

  i.g. Affinity와 Diversity가 함께 증가하는 방향(↗)으로 relative accuracy가 증가한다.



<img src="/resources/tradeoffs_in_data_augmentation__an_empirical_study/image-20230505152843722.png" alt="image-20230505152843722" style="zoom:80%;" />

이러한 관계를 더욱 잘 정량화하기 위해, pairs of augmentation strategies $(a, a')$에 대해 다음 조건을 만족하도록 하였다.
$$
\{ (a, a'): \mathcal{A}(a) > \mathcal{A}(a') \text{ and } \mathcal{T}(a) > \mathcal{T}(a') \text{ or } \mathcal{D}(a) > \mathcal{D}(a') \}.
$$
i.e. $\mathcal{A}(a) > \mathcal{A}(a')$: more data shifted;  $\mathcal{T}(a) > \mathcal{T}(a')$: more Affinity in augmented data;  $\mathcal{D}(a) > \mathcal{D}(a')$: more diverse samples in augmented data;



















