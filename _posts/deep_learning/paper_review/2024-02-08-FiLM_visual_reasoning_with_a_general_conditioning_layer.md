---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments
title: '[AAAI`18] FiLM: Visual Reasoning with a General Conditioning Layer'
excerpt: "FiLM: Visual Reasoning with a General Conditioning Layer"
date: 2024-02-08
last_modified_at: 2024-02-08
categories:
  - deep_learning-paper_review
tags: 
  - [Visual Reasoning, Conditioning, Visual-Language Model(VLM)]

use_math: true
comments: true
share : false
---



<img src="/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/film.png" alt="Untitled (2)" style="zoom:67%;" />



본 연구의 주요 task는 visual reasoning으로, CLEVR benchmark에서 제안된 방법의 효과를 검증하였다.

> **CLEVR benchmark란?**
>
> ![clevr_benchmark](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/clevr_benchmark.png)
>
> <img src="/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/clevr_benchmark2.png" alt="img" style="zoom:67%;" />
>
> - Question answering을 통해 visual reasoning 성능을 평가하기 위한 벤치마크
>
> - 이미지는 서로 다른 성질(e.g. 모양, 재료, 색상, 크기)을 갖는 3D-rendered objects로 구성된다.
>
> - 질문은 여러 단계에 걸쳐 구성됨 (복잡함)
>
>   E.g. “How many green objects have the same size as the green metallic block?”
>
> - 시각자료(image)와 질문(text)이 주어졌을 때, 시각 자료에 기반하여 질문에 답(text)하는 것을 목표로 함



![data_bias](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/data_bias.png)

저자는 기존 방식들이 복잡한 기본 구조를 포착하기 보단 데이터 편향을 이용하는 경향이 있다는 논문을 인용하였다. 그러면서 학습 데이터에 편향하는 것이 아니라, 데이터에 내재된 구조 자체를 학습하는 방법으로 FiLM을 제안한다.



## Method

![introduction_film](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/introduction_film.png)

**FiLM (Feature-wise Linear Modulation)**은 임의의 입력을 이용하여 $\gamma$,$\beta$라는 conditioning vector를 생성하고, 이를 이용하여 주어진 입력에 대해 affine transformation $\gamma_{i,c}\bold{F}_{i,c}+\beta_{i,c}$을 수행하여 conditioning을 수행한다.

<img src="/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/single_film_layer.png" alt="single_film_layer" style="zoom:50%;" />

위 그림과 같이 FiLM은 단순한 affine transformation을 neural network의 중간 단계 features에 적용한다. 이때 affine transformation은 임의의 입력값(여기서는 question에 해당)에 의해 조건화된다. 



구체적으로 FiLM은 다음과 같이 동작한다:

![film_implementation](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/film_implementation.png)

FiLM learns functions f and h which output $\gamma_{i,c}$ and $\beta_{i,c}$ as a function of input $\bold{x}_i$:
$$
\gamma_{i,c}=f_c(\bold{x}_i), &  \beta_{i,c}=h_c(\bold{x}_i),\\
$$
where $\gamma_{i,c}$ and $\beta_{i,c}$ modulate a neural network’s activations $\bold{F}_{i,c}$, whose subscripts refer to the ith input’s cth feature of feature map, via a feature-wise affine transformation:
$$
\text{FiLM}(\bold{F}_{i,c}|\gamma_{i,c},\beta_{i,c})=\gamma_{i,c}\bold{F}_{i,c}+\beta_{i,c}.
$$


>  📎 본 논문에서는 편의를 위해 다음과 같이 이름 붙였다.
>
> - FiLM generator: $(\bold{\gamma}, \bold{\beta})$를 출력하는 함수 $f$ 와 $h$를 포함. 
>   ※ 효율을 위해서 $f$와 $h$가 파라미터를 공유하도록 설계도 가능 ※
>   - 텍스트를 입력하면 $\gamma$와 $\beta$를 출력함
> - FiLM-ed network: FiLM layers가 적용될 네트워크. 여기서 FiLM layers는 feature maps이 modulated되도록 해주는 역할을 함.
>   - 이미지와 텍스트 조건을 입력하면 답변(text)이 출력됨



### FiLM generator

> 자세한 구현 코드는 아래 [Implementation/FiLM generator] 섹션에서 확인

FiLM generator는 $\gamma$와 $\beta$를 출력하는 함수 $f$와 $h$로 구성된다. 

![film_generator](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/film_generator.png)

[tokenize+encoding with vocab]된 questions_var로부터 [Embedding, GRU, Linear]를 이용하여 question embedding($\gamma, \beta\in\mathbb{R}^{N\times4\times128}$)을 생성한다. 여기서 $f$와 $h$는 neural networks와 같은 임의의 함수가 될 수 있다. 

1. 입력: x (N, T) ← `questions_var`
   - `torch.autograd.Variable` 클래스는 값(`data` ), 미분(`grad`), backward를 통한 미분을 계산한 함수(`grad_fn` )를 포함한다.
2. 인코딩: x →[Embedding]→ embed (N, D=100) →[GRU]→ encoded (N, hidden_size=512) Questions을 embedding하여 정수형태로 만든 뒤, GRU 네트워크에 입력하여 인코딩시킴
   - **Embedding**: tokens, e.g. Are, there, more, cubes, than, yellow, things,을 embedding함. i.e. 각 단어에 고유한 정수를 맵핑함.
   - **GRU**: 시계열 데이터 처리를 위한 네트워크. Question embedding를 final hidden state로 출력.
3. 디코딩: encoded →[Linear]→ film (N, num_modules * V_out = 4*256) 인코딩된 question embedding을 Linear layer에 통과시켜 conditioning을 위한 임베딩 생성
4. Reshape: film →[Reshape]→ film (N, num_modules=4, V_out=256)

> 🪨: 여기서 `num_modules` 은 $\gamma$, $\beta$를 이용하여 affine transformation을 수행할 ResBlock의 개수를 의미함. `V_out` 은 각 ResBlock의 channels을 의미함. i.e. 모든 ResBlock에 대해서, 각 channel별 출력 $\bold{F}_{i,c}\in\mathbb{R}^{h*w}$에 대해 affine transformation $\gamma_{i,c}\bold{F}_{i,c}+\beta_{i,c}$을 수행하는 것임.



### FiLMed network

> 자세한 구현 코드는 아래 [Implementation/FiLMed network] 섹션에서 확인

생성된 question embedding($\gamma, \beta\in\mathbb{R}^{N\times\verb|num_modules|\times C}$)을 이용하여 image로부터 얻어진 features $\bold{F}\in\mathbb{R}^{N\times C\times H\times W}$의 각 입력 별 채널 $\bold{F}_{i,c}\in\mathbb{R}^{H\times W}$에 대해 affine transformation $\gamma_{i,c}\bold{F}_{i,c}+\beta_{i,c}$을 $\verb|num_modules|$번 수행. 얻어진 결과를 classifier에 통과시켜 최종적인 결과 answer$\in\mathbb{R}^{N\times\verb|num_answers|}$를 출력.

![filmed_network](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/filmed_network.png)

1. 이미지를 CNN (Conv2d+BN+ReLU)에 통과시켜 feats $\bold{F}\in\mathbb{R}^{N\times 128\times 7*7}$을 출력
2. feats를 FiLMedResBlock에 입력시켜서 아래의 affine transformation을 수행:
   
    $$
    \text{FiLM}(\bold{F}_{i,c}|\gamma_{i,c},\beta_{i,c})=\gamma_{i,c}\bold{F}_{i,c}+\beta_{i,c}.
    $$
    
3. FiLMedResBlock의 출력을 classifier에 통과시켜서 answer $\verb|out|\in\mathbb{R}^{N\times\verb|num_answers|}$를 출력, 
where classifier=(Conv2d+BN+ReLU+MaxPool2d+Flatten+Linear+BN+ReLU+Linear)



### Summary


즉, 한마디로 요약하자면…

$i$번째 입력의 $c$번째 feature map에 대해, FiLM은 두 개의 파라미터 $\gamma_{i,c}$와 $\beta_{i,c}$를 이용하여 feature map을 다음과 같이 affine transformation 한다:

$$
\text{FiLM}(\bold{F}_{i,c}|\gamma_{i,c},\beta_{i,c})=\gamma_{i,c}\bold{F}_{i,c}+\beta_{i,c}.
$$

여기서..!

- 이러한 conditioning은 동일한 입력에 대해서도 수행할 수 있고, 다른 입력에 대해서도 수행할 수 있음. 여기서는 다른 입력(questions, image)에 대해 수행한 결과만을 제시하였음.

  ![other_input](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/other_input.png)
  

- FiLM은 modulated feature map 당 **단지 두 개의 파라미터만을 필요**로하므로, **scalable & computationally efficient** conditioning method이다. 또한, 이미지의 해상도에 따라 scale되지 않음!! (왜냐면 feature-map 당이니까…)

  보면, image로부터 얻어진 features $\bold{F}\in\mathbb{R}^{N\times C\times H\times W}$의 각 입력 별 채널 $\bold{F}_{i,c}\in\mathbb{R}^{H\times W}$에 대해 affine transformation $\gamma_{i,c}\bold{F}_{i,c}+\beta_{i,c}$을 $\verb|num_modules|$번 수행함. 즉, spatial dimension $H\times W$에 대해서 동일한 $\gamma,\beta$가 적용됨. i.e. FiLM은 입력 이미지의 크기에 무관!



## Analysis

### FiLM에서 $\gamma$와 $\beta$가 어떻게 feature map에 영향을 미칠까?

![film_parater_histogram](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/film_parameter_histogram.png)

![img](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/figure_5.png)

$$ \gamma\bold{F}+\beta $$

- $\gamma$:

  ![img](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/figure_5_gamma.png)

  - 0에서 가장 높은 빈도:

    - Question embedding $\gamma$를 이용하여 feature map 전체를 크게 억제하는 방법을 학습함
    - 높은 크기의 $\gamma$값을 갖는 feature map $\bold{F}_{i,c}$에 대해서는 선택적으로 상향조절을 함

  - $\gamma$의 36%는 음수임:

    - FiLM 뒤에서 바로 ReLU를 수행함.

      ![img](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/relu.png)

      ![img](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/filmed_network_relu.png)

    - 즉, $\gamma<0$인 부분은 ReLU를 통과하면서  $\gamma>0$인 부분과 상당히 다른 activation set을 출력하게 됨.

- $\beta$:

  ![img](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/figure_5_beta.png)

  - $\beta$의 76%는 음수를 가짐
    - $\gamma$와 마찬가지로 FiLM 뒤에있는 ReLU를 통과하면서 어떤 activations을 통과시킬지를 결정하게 됨



### FiLM에서 $\gamma$와 $\beta$ 중 뭐가 더 중요한 요소일까?

![img](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/figure_7.png)

- $\beta=\beta_{mean}$과 $\gamma=\gamma_{mean}$을 통해 학습 데이터 전체에 대한 평균으로 대체해서 추론해봤음. 이렇게 해서 test했을 때, $\beta=\beta_{mean}$는 성능이 1.0%만 감소했지만, $\gamma=\gamma_{mean}$는 성능이 65.4%나 감소했음
- $\gamma,\beta$에 Gaussian Noise를 추가해보았음. $\beta$보다 $\gamma$가 손상될 때 더욱 크게 성능이 감소함.

이 두 결과는 모두, FiLM에서 $\beta$보다 $\gamma$값이 더 중요함을 의미함.



### FiLM을 어떻게 디자인했을 때 잘 conditioning할 수 있을까?

![img](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/restricted_gamma.png)

- $\gamma$와 $\beta$ 중 하나를 상수로 고정해서 학습 시켜보았음 ⇒ $\gamma$가 $\beta$보다 중요함

- $\gamma$값의 범위를 제한두어서 학습 시켜 보았음

  : $\sigma(\gamma)\in(0,1)$;  $tanh(\gamma)\in(-1,1)$;  $\exp(\gamma)\in(0,\infin)$;

  - $\sigma(\gamma)\in(0,1)$ & $tanh(\gamma)\in(-1,1)$:

    $\gamma=1$로 학습하는 것 만큼이나 성능을 하락시킴. i.e. 큰 $\gamma$값을 통해 feature map을 큰 규모로 확장시키는 것이 중요함.

  - $\exp(\gamma)\in(0,\infin)$:

    $\gamma$가 음수나 0의 값을 갖는 것은 매우 중요.

  > 🪨 요약하자면, conditioning을 잘하기 위해서 (1) $\gamma$는 큰 규모의 값을 가져야 하며 (2) 음수나 0의 값을 가져야 한다.



### FiLM을 ResBlock에서 다른 위치에 두었을 때 어떤 영향이 있을까?

![image-20240209182726098](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/moving_film.png)

- 여기서 포인트는 FiLM을 BN의 앞에 두던 뒤에 두던, ReLU의 앞에 두던 뒤에 두던, 성능에 큰 영향 없음

  <img src="/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/filmed_network_relu.png" alt="img" style="zoom:50%;" />

  - Best architecture →[Conv]—$\cdot$→[ReLU]→[Conv]→[BN]→**[FiLM]**→[ReLU]→(+)→
  - FiLM after residual connection →[Conv]—$\cdot$→[ReLU]→[Conv]→[BN]→[ReLU]→(+)→**[FiLM]**→
  - FiLM after ResBlock ReLU-2 ← 거의 비슷한 성능 →[Conv]—$\cdot$→[ReLU]→[Conv]→[BN]→[ReLU]→**[FiLM]**→(+)→
  - FiLM after ResBlock Conv-2 ← 거의 비슷한 성능 →[Conv]—$\cdot$→[ReLU]→[Conv]→**[FiLM]**→[BN]→[ReLU]→(+)→
  - FiLM before ResBlock Conv-1 ← 성능하락 있음 →**[FiLM]**→[Conv]—$\cdot$→[ReLU]→[Conv]→[BN]→[ReLU]→(+)→
  - 즉, FiLM은 BN에 대해 의존하는 것처럼 보이진 않음. 따라서 BN을 쓰지 않는 다른 네트워크, e.g. LayerNorm을 쓰는 경우,에서도 사용할 수 있을 것이다!



### Modules에 FiLM을 몇 번 적용하는지가 성능에 미치는 영향?

![image-20240209182923156](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/removing_film.png)

- 하나라도 FiLM이 있으면 됨: `No FiLM in ResBlock 2-4`  vs. `No FiLM in ResBlock 1-4`



### Modules의 개수가 성능에 미치는 영향?

![image-20240209183029702](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/num_modules.png)

한개만 아님 됨.



### 아키텍처가 성능에 미치는 영향?

![image-20240209183112852](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/miscellanenous.png)

- Residual connection이 없으면 성능이 크게 감소함. Global max-pooling을 마지막에 쓰는데, 그때 낮은 수준과 높은 수준의 추론을 통해 반복적으로 중요한 위치의 특징을 사용하여 최종 결정을 내림을 시사. i.e. Residual 안쓰면 낮은 수준에서의 결과의 영향이 작아지는데, 그러면 상대적으로 높은 수준의 결과만 사용하게 됨. Residual을 쓴다는 것은 낮은 수준의 결과와 높은 수준의 결과를 함께 사용하여 추론함을 의미.

  ![img](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/tsne_film.png)

  > 🪨 Global max-pooling:Ablation
  >
  > ![img](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/global_averaging_pooling.png)



### 전체적으로 요약하자면…

1. Conditioning에 중요한 건 $\beta$보단 $\gamma$.
   - 이때 $\gamma$는 큰 규모의 값을 가지며 negative값을 가질 수 있을 때 성능이 더 좋음. ⇒ 큰 규모의 값을 통해 feature map에서 중요도를 scaling하고, negative 값을 통해 activation에서 서로 다른 영향을 만들어내기 때문으로 해석 가능
2. FiLM 안쓰면 conditioning 성능 크게 떨어짐. 몇 개를 쓰든 크게 상관 없지만 적어도 하나는 써야 함.
3. Residual connection을 사용해야 lower→higher 전반적인 features를 모두 이용하여 보다 복합적인 추론이 가능해지는 것으로 보임.



## When?

- 주어진 입력(e.g. image)을 임의의 입력(e.g. question)을 이용하여 conditioning하고 싶을 때 사용할 수 있음.

  ![other_input](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/other_input.png)

  - E.g. image stylization: 주어진 입력이 content image, 임의의 입력이 style image인 경우

    [[참고](https://github.com/ndb796/Deep-Learning-Paper-Review-and-Practice/blob/master/code_practices/AdaIN_Style_Transfer_Tutorial.ipynb)]

    ![image-20240209183435478](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/image_stylization.png)

    정규화된 주어진 입력 $x$ (=$\frac{x-\mu(x)}{\sigma(x)}$)을 임의의 입력 $y$(=style input)를 이용하여 affine transformation $\sigma\bar{x}+\mu$을 수행

    - 여기서 $\bar{x}\in\mathbb{R}^{1\times 512\times 64\times 86}$; $y\in\mathbb{R}^{1\times 512\times 64\times 86}$; $\sigma(y)\in\mathbb{R}^{1\times 512\times1\times1}$; $\mu(y)\in\mathbb{R}^{1\times 512\times1\times1}$

      - $x$

        ![image-20240209183539439](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/image_stylization_x.png)

      - $y$

        ![image-20240209183630780](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/image_stylization_y.png)

    FiLM과 거의 유사. 각 입력에 대해 channel별로 동일한 gamma(=$\sigma(y)$)와 beta(=$\mu(y)$)를 이용하여 affine transformation하는 것임. 다른 점이 있다면, AdaIN은 mean, std를 이용하여 gamma, beta를 계산하고, FiLM은 어떠한 함수($f,h$)를 이용하여 gamma, beta를 계산한다는 것임. 즉, FiLM이 AdaIN의 더욱 일반화된 버전이긴 함.



## If?

- Simple: 적용하기 아주 쉽긴 함. 그리고 일반화된 방법론이라 어떤 입력이든 전부 다 적용 가능.

- Effective

  - Ablations와 아키텍처 수정에 대해 강인한 성능을 보임: 아키텍처 조금 수정한다고 성능이 크게 하락되진 않음.

    ![image-20240209183734397](/home/dshong/.config/Typora/typora-user-images/image-20240209183734397.png)

  - 아주 적은 예시의 새로운 데이터나 zero-shot과 같은 문제에 대해 잘 일반화됨

    ![img](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/illustration.png)

    CLEVR-CoGenT는 일반화 능력을 보기 위한 benchmark

    - A: 모든 cube가 [gray/blue/brown/yellow]; 모든 cylinder가 [red/green/purple/cyan]
    - B: 모든 cylinder가 [gray/blue/brown/yellow]; 모든 cube가 [red/green/purple/cyan]

    A에서 학습하고 B에서 test를 수행. 이를 통해 네트워크가 얼마나 bias되었는지 확인 가능.

    ![img](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/figure_9.png)

    제일 성능 좋음. 사실 bias가 아예 안끼기는 쉽지 않지만, 어쨋든 좀 덜 bias되었다는 걸 보여줌.

- Scalable & Computational efficient

  - Modulated feature map 당 단지 두 개의 파라미터 $\gamma$ & $\beta$만을 필요로 한다 ⇒ scalable!!
  - 이미지의 해상도에 따라 scale되지 않는다. ($\because$ feature-map 당 파라미터를 필요로 하므로)



## Takeaways

1. 임의의 입력에 대해 conditioning하고싶을 때 간단하게 가져다가 쓰기 좋아보임. simple & robust & scalable해서..!
   1. 역시 가져다 쓰기 편한게 제일이다..!
2. 분석을 좀 잘한듯. Ablation study도 글코 t-SNE도 글코.. 주장하는 바가 실험으로 잘 보여서 읽으면 대부분 납득이 됨.
3. 사실 크게 새로운 내용은 없는듯..ㅎㅅㅎ;
   1. 나중에 가져다 쓰려면 기술적인 부분, e.g. residual connection의 역할, $\gamma$의 디자인 초이스, etc,을 잘 기억했다가 활용하는 건 좋을듯.



## Implementation

### FiLM generator

```python
# scripts/train_model.py
from torch.autograd import Variable

# Set up model
program_generator = FiLMGen(...)
pg_optimizer = optim_method(program_generator.parameters(), ...)

execution_engine = FiLMedNet(...)
ee_optimizer = optim_method(execution_engine.parameters(), ...)

for _ in range(epochs):
	for batch in train_loader:
		questions, _, feats, answers, programs, _ = batch
		# questions = {Tensor: (bs, max_questions_length)=(64, 46)}
		# fest = {Tensor: (bs, c, h, w) = (64, 1024, 14, 14)}
		# answers = {Tensor: (bs,) = (64,)} <- ground-truth로 사용됨
		# programs = {Tensor: (bs, max_programs_length) = (64, 27)} <- film에서는 사용하지 않음.

		questions_var = Variable(questions.cuda())
		feats_var = Variable(feats.cuda()) 
    answers_var = Variable(answers.cuda()) 
		programs_var = Variable(programs.cuda())
 
		# program_generator = FiLM(): encoded question을 입력하여 gamma, beta를 출력
		film = program_generator(questions_var)
		# execution_engine = FiLMedNet(): 이미지로부터 features F를 출력하고, 이를 gamma와 beta를 이용하여 conditioning
		scores = execution_engine(feats_var, film) 
		# scores = (bs, num_answers) = (64, 32)
		
		loss = torch.nn.CrossEntropyLoss()(scores, answers_var)
				
		pg_optimizer.zero_grad()
		ee_optimizer.zero_grad()
		loss.backward()
```





### FiLMed network

```python
# scripts/train_model.py
from torch.autograd import Variable

# Set up model
program_generator = FiLMGen(...)
****pg_optimizer = optim_method(program_generator.parameters(), ...)

**execution_engine = FiLMedNet(...)**
ee_optimizer = optim_method(execution_engine.parameters(), ...)

for _ in range(epochs):
	for batch in train_loader:
		questions, _, feats, answers, programs, _ = batch
		questions_var = Variable(questions.cuda())
		film = program_generator(questions_var)
		scores = **execution_engine**(feats_var, film) # FiLMedNet()
		
		loss = loss_fn(scores, answers_var)
				
		pg_optimizer.zero_grad()
		ee_optimizer.zero_grad()
		loss.backward()

# vr/models/filmed_net.py
class FiLMedNet(nn.Module):
		def forward(self, x, film):
				# x = feats = (N, ??, module_H, module_W)
				# film = (N, num_modules, V_out) = (N, 4, 256)

				# Prepare FiLM layers
				gammas, betas = torch.split(film[:, :, :2*module_dim], self.module_dim, dim=-1)
				# gammas = (N, num_modules=4, module_dim=128)
				# betas = (N, num_modules=4, module_dim=128)
```

1. Prepare FiLM layers: film → $\gamma\in\mathbb{R}^{N\times 4\times 128}$, $\beta\in\mathbb{R}^{N\times 4\times 128}$

   ```python
   				# Propagate up image features CNN
   				batch_coords = self.coords.unsqueeze(0).expand(torch.Size((x.size(0), *self.coords.size())))
   				x = torch.cat([x, batch_coords], 1)
   				# x = (N, ?? + 2, 7, 7)
   				
   				# self.stem = [Conv2d(feature_dim=2048, module_dim=128, kernel_size=3, stride=1, padding=1), BN(module_dim=128), ReLU]
   				# x = (N, ?? + 2, 7, 7) = (N, 2048, 7, 7)
   				feats = self.stem(x)
   				# feats = [N, 128, 7, 7]
   				N, _, H, W = feats.size()
   ```

2. Propagate up image features CNN: x →[Conv2d, BN, ReLU]→ feats (N, 128, 7, 7)

   ![img](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/img.png)

   이미지로 부터 features 출력

   ```python
   				# Propagate up the network from low-to-high numbered blocks
   				feats1 = self.**function_modules**[0](feats, gammas[:,0,:], betas[:,0,:], batch_coords)
   				feats2 = self.**function_modules**[1](feats1, gammas[:,1,:], betas[:,1,:], batch_coords)
   				feats3 = self.**function_modules**[2](feats2, gammas[:,2,:], betas[:,2,:], batch_coords)
   				feats4 = self.**function_modules**[3](feats3, gammas[:,3,:], betas[:,3,:], batch_coords)
   				
   				# Run the final classifier over the resultant, post-modulated features.
   				**final_module_output** = torch.cat([feats4, batch_coords], 1)
   				# self.classifier:
   				#   [Conv2d, BN, ReLU]->[maxpool]->[Flatten]->
   				#   ->[Linear]->[Linear]-> (N, num_answers)
   				out = self.**classifier**(final_module_output)
   				
   				return out
   ```

3. Propagate up the network from low-to-high numbered blocks

   ![img](/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/film_part1.png)

   1. CNN의 출력인 feats를 ResBlock에 차례로 입력

      ```python
      function_modules[i] = FiLMedResBlock(...)
      ```

   2. ResBlock의 출력을 Classifier에 입력

      ```python
      [Conv2d(module_C=(128+2)*7*7, proj_dim=512)]
      →[BN]→[ReLU]→ (N, proj_dim=512, 7, 7)
      →[maxpool2d(7)]→ (N, proj_dim=512)
      →[Flatten()]→ (N, proj_dim=512)
      →[Linear(proj_dim=512, fc_dim=1024)]→[BN]→[ReLU]
      →[Linear(fc_dim=1024, num_answers)]→ (N, num_answers)
      ```

   - `FiLMedResBlock().forward()`

     <img src="/home/dshong/git/dazory.github.io/resources/film_visual_reasoning_with_a_general_conditioning_layer/filmed_network_relu.png" alt="img" style="zoom:30%;" />

     ```python
     # vr/models/filmed_net.py
     layer_output = self.**function_modules**[fn_num](
     										module_inputs[:, fn_num], # x
     										gammas[:, fn_num, :], # gammas
     										betas[:, fn_num, :], # betas
     										batch_coords) # extra_channels
     
     class FiLMedResBlock(nn.Module):
     		def __init__(self, in_dim=module_dim=128):
     				self.condition_method = 'bn-film'
     				self.extra_channel_freq = 1
     				self.with_input_proj = 1
     				self.with_batchnorm = 1
     				# self.condition_pattern = [1, 1, 1, 1]
     				self.with_cond = self.condition_pattern[self.module_num_layers * fn_num: self.module_num_layers * (fn_num + 1)]
     				self.input_proj = nn.Conv2d(
     						in_dim + num_extra_channels, # =128+2
     						in_dim, kernel_size=1, padding=0)
     				self.with_residual = 1
     				
     				if out_dim is None: 
     						out_dim = in_dim 
     				self.conv1 = nn.Conv2d(in_dim, out_dim, kernel_size=3, padding=1)
     				
     
     		def forward(self, x, 
     						gammas=None, betas=None, 
     						extra_channels=None, cond_maps=None):
     				# x <- feats (N, module_dim, H, W) = (N, 128, 7, 7)
     				# extra_channels <- batch_coords = (N, 2)
     				x = torch.cat([x, extra_channels], 1) 
     				# x = (N, module_dim+2, H, W) = (N, 130, 7, 7)
     				
     				# self.input_proj = Conv2d(130, 128)
     				x = F.relu(**self.input_proj**(x))
     				out = x
     				# out = (N, module_dim, H, W) = (N, 128, 7, 7)
     
     				# ResBlock body
     				out = self.conv1(out) # Conv2d(128, 128)
     				out = self.bn1(out)
     				out = self.**film**(out, gammas, betas)
     				out = F.relu(out)
     
     				# ResBlock remainder
     				if self.with_residual:
     						out = x + out
     				
     				return out
     ```







