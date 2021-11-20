---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments
title: '[NeuromorphicVision-002] 뉴로모픽 비전(Neuromorphic Vision)의 현황(1)'
excerpt: "뉴로모픽 비전의 현황에 대해 알아본다."
date: 2021-11-21
last_modified_at: 2021-11-21
categories:
  - robotics-computerVision
tags: 
   - [robotics, robot vision, neuromorphic vision]

use_math: false
comments: true
share : false
---

<br>

랩세미나에서 Neuromorphic vision에 관한 발표를 보고 관심이 생겼다. 관련하여 배경과 주요 개념 및 현황을 조사하고자 본 글을 작성하게 되었다.

<br>

# Neuromorphic Vision

## 현황

기존의 프레임 기반 카메라의 한계를 극복하기 위해 생물학적 망막을 모사한 실리콘 망막(*silicon retina*)이 개발되었다.

* **Mahowald and Mead, silicon VLSI retina in 1991**

  적응적 광수용체와 픽셀의 2차원 육각격자의 칩으로 구성된다. 광수용체, 양극성 세포(*bipolar cells*), 그리고 수평 세포(*horizontal cells*)로 구성된 생물학적 망을 모방하였다.

* **Parvo-Magno retina, Zaghloul and Boahen, silicon VLSI retina**

  5개의 망막 층으로 모델링되었다.

* **Tobi Delbruck’s team, practicable silicon retina DVS(Dynamic Vision Sensor) based on biological principles**

  오늘날 대부분의 실리콘 망막은 이러한 Tobi Delbruck and Christoph Posch의 구조에서 파생되었다.

실리콘 망막이 현실세계에 실용적으로 적용되기 시작한 것은 Tobi Delbruck 팀의 DVS(*Dynamic Vision Sensor*) 이후였으며, 그 이전까지는 뉴로모픽 비전의 가능성을 증명하기 위해 이론적 위주로 개발되어왔다.

<br>

### Dynamic Vision Sensor (DVS)

#### 구조

DVS의 픽셀 회로는 <u>인간 망막의 삼층 모델을 모방</u>하였다.

![image-20211120231549490](/files/2021-11-21-cv-neuromorphic vision/image-20211120231549490.png)

* <p>광수용체 층(<span style="background-color:#A9ACFA; font-weight:bold;">Phtoreceptor layer</span>)</p>

   광신호에 의해 발생한 광전류 $I_{\text{ph}}$ 를 추적하여 전압 $V_{\text{log}}$ 로 표현한다.

* <p>외부 플렉시폼 층(<span style="background-color:#FFCAA3; font-weight:bold;">Outer plexiform layer</span>)</p>

  스파이크 이벤트 $v_{\text{diff}}$가 광전류 $I_{\text{ph}}$의 변화에 따라 반응한다.

  전류값이 증가하면(positive gradient) ON 스파이크, 감소하면(negative gradient) OFF 스파이크가 발생한다.

  ![image-20211120232424556](/files/2021-11-21-cv-neuromorphic vision/image-20211120232424556.png)

* <p>내부 플렉시폼 층(<span style="background-color:#FFF572; font-weight:bold;">Inner plexiform layer</span>)</p>

  스파이크 이벤트에 의해 ON 이벤트(빛 증가를 의미, 하얀색으로 표현됨)와 OFF 이벤트(빛 감소를 의미, 검정색으로 표현됨) 출력한다.

  ![image-20211120232538665](/files/2021-11-21-cv-neuromorphic vision/image-20211120232538665.png)

<br>

#### 통신

칩 와이어링(*Chip wiring*) 문제로 인해 각 픽셀은 고유 케이블을 가질 수 없다. 이러한 문제는 ***Address Event Representation (AER)*** 기술을 통해 해결할 수 있다.

![image-20211120233249017](/files/2021-11-21-cv-neuromorphic vision/image-20211120233249017.png)

1. 송신기(*transmitter*)에서 스파이크(*spikes*)에 의해 이진 이벤트(*binary events*)가 출력된다.
2. 송신기로부터 출력되는 이진 이벤트는 주소 인코더(*Address Encoder*)를 통해 이진 주소(*binary address*)로 변환된다.
3. 이진 주소는 버스 시스템을 통해 주소 디코더(*Address Decoder*)로 전달된다.
4. 주소 디코더는 이진 주소를 이진 이벤트로 변환시킨다.
5. 수신기(*receiver*)는 이진 이벤트를 스파이크로 변환한다.

이러한 과정을 통해 하나의 이벤트는 다음과 같이 정의된다.

$$
\text{An event} : (x,y,t,p)
$$

※ $(x,y)$: 픽셀 주소;  $t$: 타임스탬프;  $p$: 극성(positive/negative); ※

<br>

#### 장점

DVS 픽셀은 강도(*intensity*)의 변화가 문턱값을 초과하는 <u>이벤트에 자동적으로 반응</u>한다. 즉, 이벤트 기반이라는 점에서 본질적으로 프레임 기반 카메라 센서와 구분된다. 이러한 특성으로 인해 <u>이벤트 기반 뉴로모픽 비전 센서</u>는 다음의 장점을 갖는다.

* <b>에너지 친화적인 특성 (*Energy-friendly properties*)</b>

  이벤트가 발생하는 경우에만 자동적으로 반응하기 때문에 에너지 친화적이다. 이는 차량에 장착되는 온보드 장치에서 매우 중요한 특성이다.

* <b>저지연 (*Low latency*)</b>

  각 픽셀이 독립적으로 동작하므로 프레임의 전역 노출이 필요하지 않아 지연시간이 매우 짧다. 가령, 제어 시스템을 위한 시간을 많이 할당해주기 위해서는 객체 감지 시스템에서 시간 소요를 줄이는 것이 관건이므로 이러한 특성은 인식 시스템(*perception system*)에서 매우 중요하다.

* <b>높은 동적 영역 (*HDR, High dynamic range*)</b>

  DVS와 같은 이벤트 기반 뉴로모픽 비전 센서는 프레임 기반 카메라의 동적 영역(60dB)에 비해 높은 동적 영역 (120dB)을 갖는다 (HDR). 이는 매우 어둡거나 밝은 자극에 대해 로버스트한 인지 시스템 갭라로 이어지므로 터널과 같은 공간을 지나는 자율주행 시스템에서 중요하다.

* <b>마이크로초 해상도 (*Microsecond resolution*)</b>

  아날로그 회로에 의해 밝기 변화가 빠르게 취득되므로, 이벤트는 마이크로초 해상도를 가질 수 있다. 이는 위급한 주행 상황에서 매우 중요한 특성이다.

* <b>모션블러가 없음 (*No motion blur*)</b>

  빠르게 주행하는 환경에서 프레임 기반 카메라는 모션 블러를 발생시켜 인지 성능이 감소한다. 따라서 이벤트 기반 뉴로모픽 비전은 동적 모션을 매우 정교하게 취득할 수 있다.

<br>

### 알고리즘

#### 이벤트 노이즈 처리

이벤트 기반 뉴로모픽 비전 센서는 움직이는 물체로 인해 발생하는 밝기 변화를 잘 감지할 수 있다. 따라서 한편으로는 노이즈로 인한 밝기 변화에도 민감하기 때문에 원시 데이터(*raw data*)의 전처리가 중요하다. 이벤트 스트림에서 <u>이벤트 노이즈를 제거하는 방법</u>으로는 다음과 같이 두 가지 방법이 있다.

![image-20211120234736118](/files/2021-11-21-cv-neuromorphic vision/image-20211120234736118.png)

* <p><b>시공간 상관 필터 (<span style="color:red;">*The spatial-temporal correlation filter*</span>)</b></p>

  이벤트 $e_i=(x_i, y_i, t_i, p_i)$가 들어왔을 때, 현재 픽셀 위치 $(x_i, y_i)$의 가장 최근 주변 이웃들을 탐색한다 ($≤ \text{distance } D$). 즉, 다음의 수식이 만족될 때, 노이즈가 발생하지 않았다고 판단한다.

  $$
  t_i-t_n<d_t
  $$

  * $d_t$ : 미리 정의된 문턱값
  * $t_i$ : 이벤트의 타임스탬프
  * $t_n$​ : 가장 최근에 발생한 이웃 이벤트의 타임스탬프

  <br>

* <p><b>모션 일관성 필터 (<span style="color:#00B0F0;">The motion consistency filter</span>)</b></p>

  물체에 의한 움직임(*object motion*)인 경우, 로컬 영역에서 이전 이벤트와 일관된 움직임을 보여야한다. 따라서 속도 $(v_x, v_y)$를 통해 움직임 일관성을 평가하여 같은 움직임 영역인지를 판단한다. 만일 같은 평면을 공유하지 않으면 이벤트 노이즈(*event noise*)라 판단하여 제거한다.

  구체적으로 모션 일관성 평면은 다음과 같이 공식화될 수 있다.

  $$
  ax_i+by_i+ct_i+d=0
  $$

  * $(a,b,c,d)∈\mathbb{R}^4$ : 평면 $M$을 정의
  * $(x_i,y_i)$ : 이벤트 $e_i$의 좌표
  * $t_i$ : 이벤트 $e_i$ 의 타임스탬프

<br>

#### 비동기 이벤트의 표현

이벤트 기반 뉴로모픽 비전 센서는 단지 픽셀 레벨의 변화만을 전송하므로 희소한(*sparse*) 비동기 이벤트 스트림 데이터가 출력된다. 이는 <u>CNN(*Convolutional Neural Network*) 기반 아키텍처와 같은 표준 비전 파이프라인에 직접적으로 적용할 수 없는 형태</u>이다. 따라서 인코딩 기법을 통해 <u>비동기 이벤트를 동기 이미지 혹은 그리드(*grid*) 표현으로 변환</u>해주어야 한다.

여기서는 대표적인 최신 인코딩 기법으로 **공간적 인코딩(*Spatial encoding*)**과 **시공간적 인코딩(*Spatial-temporal encoding*)** 기법을 소개한다.

* **공간적 인코딩(*Spatial encoding*)**

  픽셀 위치 $(x_i, y_i)$에서의 이벤트 데이터를 고정된 시간 간격 (e.g., 30ms) 혹은 고정된 이벤트 개수(e.g., 500events)으로 저장하여 이벤트 스트림을 이벤트 프레임으로 변환해준다. 이벤트 프레임에서 픽셀 값은 일반적으로 마지막 이벤트의 polarity (positive event=1, negative event=-1) 혹은 고정된 간격에서 이벤트 개수의 확률적 특성으로 표현된다. 

  1. **상수 시간 프레임 (*constant time frame*)**

     $$
     F_j^t=\textbf{card}{(e_i \mid T·(j-1) ≤t_i ≤T·j)}
     $$

     * $F_j^t$​ : 시간 간격 $T$​의 $j$​번째 프레임
     * $\textbf{card}()$​ : 집합의 카디널리티(*cardinality*)
     * $e_i$ : 이벤트 스트림의 $i$번째 이벤트

     <br>

  2. **상수 개수 프레임 (*constant count frame*)**

     $$
     F_j^e=\textbf{card}{(e_i \mid E·(j-1) ≤i≤E·j)}
     $$

     * $F_j^e$ : 이벤트 $E$를 포함하는 $j$번째 프레임

     <br>

  3. **이벤트 개수 프레임 (*event count frame*)**

     ![image-20211120235613145](/files/2021-11-21-cv-neuromorphic vision/image-20211120235613145.png)

     $$
     {Hist}^+(x,y)=\sum_{p_i=+1, t_i∈T}{δ(x-x_i, y-y_i)}
     $$

     * ${Hist}^+(x,y)$ : positive event에 대한 히스토그램
     * $δ$​ : 크로네커 델타 함수(*Kronecker delta function*)

     <br>

* **시공간적 인코딩(*Spatial-temporal encoding*)**

  이벤트의 시공간 정보를 결합한 방식으로, 이벤트를 간결한 표현으로 변환해준다.

  1. **활성 이벤트의 표면, *Surface of active events (SAE)***

     픽셀값을 표현할 때, 강도(*intensity*)값 대신에 타임 스탬프값을 사용한다.

     $$
     \text{SAE}:t_i↦P(x_i,y_i)
     $$

     * $t_i$ : 각 픽셀에서 가장 최근에 발생한 이벤트의 타임스탬프

     $(x_i,y_i)$에서 발생한 이전의 이벤트 정보를 무시한다는 단점이 있다.

     <br>

  2. **Leaky integrate-and-fire (LIF)**

     ![image-20211120235918979](/files/2021-11-21-cv-neuromorphic vision/image-20211120235918979.png)

     LIF는 생물학적 인지 원리와 계산 요소에서 영감을 받은 인공 뉴런으로, 뉴런이 DVS에서 발생된 입력 스파이크(이벤트)를 수신하여 막 전위(*membrane potential*)을 수정한다.막 전위가 미리 정의된 문턱값을 초과하면 스파이크 자극이 출력으로 전송된다. 

     LIF 뉴런은 다음과 같이 모델링될 수 있다.

     $$
     τ \frac{dV}{dt}=-(V(t)-V_{\text{reset}} )+RI(t)
     $$

     * $V(t)$ : 막전위
     * $I(t)$​ : 전체 시냅스 전류
     * $R$​ : 막저항
     * $τ$ : 막 시간 상수

     막전위가 threshold voltage $V_th$에 도달하면 reset voltage $V_{\text{reset}}$​ 으로 리셋되며 뉴런이 스파이크를 출력한다 (fire). 또한 LIF 뉴런은 이벤트 데이터 표현뿐만아니라 스파이킹 신경망(SNN, *spiking neural network*)의 기본 유닛으로도 사용된다.
     
     <br>

  3. **복셀 그리드 (*Voxel grid*)**

     ![image-20211121000151382](/files/2021-11-21-cv-neuromorphic vision/image-20211121000151382.png)

     시간 도메인에서 이벤트 스트림의 해상도를 향상시키기 위한 이벤트 표현법이다. $N$개의 이벤트 집합 $(x_i, y_i,t_i,p_i )_{i∈[1,N]}$이 주어졌을 때, 시간 차원을 $B$개의 빈(*bins*)으로 나누어 범위를 $[0,B-1]$으로 스케일링한다.

     이때 이벤트 복셀 그리드는 다음과 같이 정의된다.

     $$
     \hat{t}=(B-1)\frac{(t_i-t_1)}{(t_N-t_1)}\\
     V(x,y,t)= \sum_i^N{p_i k(x-x_i )k(y-y_i )k(t-\hat{t})}\\
     k(z)=max⁡{(0, 1-\mid z \mid)}
     $$

     * $k(z)$ : trilinear voting kernel

     <br>

  4. **이벤트 스파이크 텐서 (EST, *Event spike tensor*)**

     EST는 end-to-end로 학습된 표현법이다. 주어진 시간 간격 $T$에서, EST는 컨볼루션 신호(convolved signal)를 샘플링하여 형성된다. 

     $$
     S_±[x,y,t]= \sum_{e_i∈p_±}{f_± (x_i, y_i, t_i ) k_c (x-x_i, y-y_i, t-t_i)}
     $$

     * $f_± (x_i,y_i,t_i)$​ : 각 이벤트에 할당된 측정값
     * $k_c$ : 이벤트 스트림으로부터 의미있는 신호를 얻기 위한 커널 컨볼루션 함수

     <br>

<br>

#### 데이터 세트

이벤트 기반 뉴로모픽 비전, 뉴로로보틱스, 그리고 자율주행 차량의 연구를 위한 다양한 데이터세트가 존재한다. 

1. **DET 데이터세트**

   ![image-20211121000729709](/files/2021-11-21-cv-neuromorphic vision/image-20211121000729709.png)

   * 차선 검출을 위한 데이터 세트
   * 터널, 다리, 육교, 그리고 도시 공간에서 주행한 다양한 교통 장면을 포함
   * 라벨링된 1,280×800 pixels의 5,424 event frames을 포함
     * 학습 세트: 2,716 frames<br>검증 세트: 873 frame<br>테스트 세트: 1,835 frames
     * 두 종류로 라벨: '차선이 아닌 픽셀'과 '차선인 픽셀'

     <br>

2. **N-CARS 데이터세트**

   ![image-20211121000839918](/files/2021-11-21-cv-neuromorphic vision/image-20211121000839918.png)

   * DVS를 통해 도시환경에서 기록된 데이터를 제공
   * 12,336 자동차 샘플과 11,693 배경(noncar) 샘플을 포함
     * 학습 세트: 7,940 자동차 샘플+ 7,842 배경 샘플<br>테스트 세트: 그 외의 모든 샘플들

   * 각 예들은 잘못된 예제를 수동으로 보정한 반자동 프로토콜에 의해 라벨링된다.

   <br>

3. **MVSEC 데이터세트**

   ![image-20211121000953107](/files/2021-11-21-cv-neuromorphic vision/image-20211121000953107.png)

   * 다중 센서로 3차원 인식을 하기 위해 생성된 MultiVehicle Stereo Event Camera dataset
   * 동기화된 스테레오 이벤트 기반 뉴로모픽 비전 시스템에 대한 첫 번째 데이터 세트
   * 보정된(calibrated) 라이다 시스템으로부터 생성된 ground-truth 깊이 데이터를 포함
   * 다양한 조도 및 주행 속도 환경에서의 긴 야외 시퀀스로 구성됨
   * 이벤트 기반 시각적 주행거리 측정(*visual odometry*), 위치 파악(*localization*), 장애물 회피(*obstacle avoidance*), 그리고 3차원 재구성(*3D reconstruction*) 평가에 사용할 수 있음

   <br>

4. **DDD17 데이터세트**

   ![image-20211121001020199](/files/2021-11-21-cv-neuromorphic vision/image-20211121001020199.png)

   * DAVIS 센서로 취득된 첫 번재 대규모 공공 데이터
   * 스위스에서 독일까지 주행하며 고속도로와 도시 환경에서 기록한 데이터를 포함
   * 1,000km 이상의 거리를 커버하는 서로 다른 날씨, 길, 그리고 빛 환경에서 수집된 12시간 이상의 데이터
   * 속력, GPS 위치, 운전자 조향, throttle 그리고 브레이크와 같은 차량 데이터가 함께 수집됨

<br>

<br>

# References

* Chen, Guang, et al. "Event-based neuromorphic vision for autonomous driving: a paradigm shift for bio-inspired visual sensing and perception." IEEE Signal Processing Magazine 37.4 (2020): 34-49.


<br>


---

다음 포스팅에서는 뉴로모픽 비전의 현황과 전망, 그리고 사례에 대해 알아본다.


<br>

<br>