---
title:  "AIAS Machine Learning Basics"
excerpt: "인하대학교 홍성은 교수님의 인공지능 응용 시스템 수업을 듣고 공부한 자료입니다."

categories:
  - Blog
tags:
  - Blog
last_modified_at: 2021-04-08T08:06:00-05:00
---

AIAS(2021-1)

# Machine Learning Basics

## 1. ML Overview
### 머신러닝의 분류
머신러닝은 크게 Supervised Learning, Unsupervised Learning, Reinforcement Learning으로 분류된다.

- **Supervised Learning(지도학습)**: target value가 존재하는 학습 방식으로, input값에 label을 할당한다. 예를 들어, 모델을 학습시킬 때 input값으로 강아지 이미지와 함께 "Dog"라는 label을 함께 주는 경우를 의미한다. supervised learning은 예측 결과가 연속적인지 혹은 불연속적인지 여부에 따라 classification model과 regression model로 나눌 수 있다. 간단히 말하자면, 예측 결과가 불연속적인 classification model의 결과값은 class 형태이고 예측 결과가 연속적인 regression model은 결과값이 value의 형태이다. 
	> **주의사항:**
	> 확률은 일반적으로 연속적인 형태를 띠므로 확률을 예측하는 문제는 regression model이라고 생각하기 쉽다. regression 문제가 아닌 것을 regression model로 푸는 경우 학습이 제대로 되지 않으므로 두 개념을 확실히 구분해야한다.

	예를 들어, '욕설을 필터링하는 문제'를 해결하는 모델을 만든다고 생각해보자. 이 문제에서 결과값은 욕설이냐(1) 아니냐(0)로 구분될 수 있다. 즉 불연속적인 결과값을 갖는 supervised learning 문제이다. 하지만 누군가는 이를 욕설일 확률(0~1 사이의 연속적인 값)문제로 해결하고자 할 수 있다. 이렇게 문제를 해결하려는 경우 두가지 문제에 직면할 수 있다. 첫번째는, 입력 데이터에 대해 label을 어떻게 주는가에 관한 문제이다. 예를 들어 "이런 ㅅㅂ"이라는 문장이 욕설일 확률은 얼마인가? 설령 입력데이터를 특정 확률로 라벨링을 했다 할지라도 어느 범위까지를 욕설로 분류할 것인가에 대한 문제에 직면한다. 50% 이상인 경우를 욕이라고 판단한다고 하더라도 데이터가 많아짐에 따라 그러한 구분선은 또다시 무의미해질 수도 있다. 따라서 이런 문제의 경우 애초에 regression model로 풀어야 한다.

	classification model과 regression model은 문제에 따라 다음과 같이 분류될 수 있다.
	
	- **Classification Model (분류모델)**: 예측값이 discrete한 경우
		- `Binary Classification`: '두 가지 class중 하나의 class로' 예측되는 경우이다. 예를 들어, 입력된 이미지가 강아지인지 혹은 고양이인지 분류하는 경우에 해당한다.
		- `Multi-class Classification`: '두 가지 이상의 classes 중 하나의 class로' 예측하는 경우이다. 예를 들어, 입력된 이미지가 강아지인지 고양이인지 혹은 자동차인지로 분류하는 경우에 해당한다.
		- `Multi-label classification`: '하나 이상의 classes 중 하나 이상의 classes로' 예측하는 경우이다. 예를 들어, 특정 영화의 카테고리를 분류할 때 로맨스이면서 공포물일 수 있는 경우에 해당한다.
		- `Imbalanced Classification`: '각 class의 데이터 개수가 불균형한 경우'의 예측이다. 예를 들어, 백인 남성의 이미지만 유독 많은 경우, 입력된 사람 이미지에 대해 백인 남성으로 분류할 가능성이 높아지는 분류 모델에 해당한다. 이 경우 모델이 적절하게 학습되지 않을 확률이 높으므로 적절한 데이터 처리를 해줘야한다.

		[[Classification Model 관련 자세한 내용](https://machinelearningmastery.com/types-of-classification-in-machine-learning/)]
	- **Regression Model (회귀모델)**: 예측값이 continuous한 경우
		> **주의사항**:
		> logistic regression은 분류모델에 주로 사용되는 알고리즘이다. 이름에 regression이 들어간다고 착각하지 말자. 
	
- **Unsupervised Learning(비지도학습)**: discrete 값이든 continuous값이든 예측할 수 있는 범위가 있는 supervised learning과 달리 target value가 없는 학습 방식이다. supervised learning이 정답을 알려주는 학습법이라면, unsupervised learning은 정답없이 input data의 특성만으로 특정 그룹으로 분류해주는 학습법이다. 예를 들어, 넷프
- 시청했던 영화 목록(*input*)을 바탕으로 사용자 그룹으로 묶어 영화를 추천해주는(*output*) 시스템)
- **종류**
	**① Clustering Model (분류 모델)**   
   1) **Hard Clustering**
   입력된 object(*input*)가 특정 집단(*cluster*)에 속하는지 속하지 않는지
   2) **Soft Clustering**
   입력된 object(*input*)가 집단에 속하는 정도(


### ML과 DL의 차이
![](https://blogfiles.pstatic.net/MjAxODExMjVfMTkw/MDAxNTQzMTA5MTQ0MTI1.Nh_1wXOWOINqeK_zC7b9YwuQIomSGhHfZne-8098jQIg.uJudwM-jUn-LRcBZdPUAmcpy3IXvGnd6ofpC2ecVNJ8g.PNG.redserpent/%EB%A8%B8%EC%8B%A0%EB%9F%AC%EB%8B%9D%2C_%EB%94%A5%EB%9F%AC%EB%8B%9D.PNG?type=w3)

**머신러닝(ML)**
- Feature Extraction과정에서 User가 정해준 hyperparameters*에 의존
		`hyperparameter: 모델링시 사용자가 직접 세팅해주는 값`
- Image features의 경우
	    
- 
**딥러닝(DL)**:  feature extraction과 classification 과정에서 모델의 parameter가 알아서 학습됨

	parameter란?
		모델 내부에서 결정되는 변수
 

