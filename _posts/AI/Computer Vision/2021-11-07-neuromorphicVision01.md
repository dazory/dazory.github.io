---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments
title: '[NeuromorphicVision-001] 뉴로모픽 비전(Neuromorphic Vision)의 배경 및 개념'
excerpt: "뉴로모픽 비전의 배경과 기본 개념에 대해 알아본다."
date: 2021-11-07
last_modified_at: 2021-11-07
categories:
  - ai-computerVision
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

## 배경

### 기존 프레임 기반 이미지 센서

현재 가장 대중적인 이미지 센서는 다음과 같다.

* **CCD(*charge-coupled devices*)**
* **CMOS APS(*Active Pixel Sensors*)**

두 이미지 센서는 모두 시각적 정보를 연속적인 "스냅샷(*snapshot*)" 영상 (frames)의 형태로 취득하는 <u>프레임 기반 이미지 센서</u>이다. 따라서 다음의 한계를 수반한다.

* 고정된 프레임 속도(*frame rate*)로 동작함에 따라 발생하는 문제

  * 낮은 프레임 속도인 경우, 중요한 정보의 손실이 발생
  * 높은 프레임 속도인 경우, 과하게 불필요한 정보까지 취득

  ⇒ 데이터 집약적(*data-intensive*)이거나 지연에 민감한(*delay-sensitive*) 응용 분야(e.g., 고속 모터 제어, 자율 로봇 네비게이션)에서 치명적인 단점으로 작용

또한 기존의 이미지 센서는 시각정보를 디지털 정보로 받아들이기 위한 **2차원 이미지 센서 어레이(*2D image sensors array*, CCD or APS)**, 시각 정보를 저장하기 위한 **메모리 유닛(*memory unit*)**, 그리고 컴퓨터비전 알고리즘을 실행하기 위한 **프로세싱 유닛(*processing unit*)**으로 구성된다. 이러한 폰 노이만 구조<sup>[[개념]](#footnote_폰노이만)</sup>는 이미지 센서와 프로세싱 유닛 간의 데이터 이동에 따라 다음의 한계를 수반한다.

  * 지연 (*latency*)

  * 높은 통신 대역폭(*communication bandwidth*)이 요구됨

  * 높은 전력 소모 (*high power consumption*)

<br>

### 이벤트 기반 뉴로모픽 비전

고전적인 이미지 센서와 달리 생물학적 시스템, 예를 들어 인간의 시각 시스템은 시각 정보가 지나가는 **렌즈(*lens*)**, 들어온 정보를 인지하고 전처리하는 **망막(*retina*)**, 그리고 추출된 정보가 지나가는 **시신경(*optic nerves*)**과 최종적인 처리를 위한 **시각피질(*visual cortex*)**로 구성된다. 뉴로모픽 비전 센서는 이 중 **망막(*retina*)**의 역할에서 아이디어를 얻었다.

* 망막의 역할
  * 불필요한 시각 정보를 버림
  * 뇌에서 더 많은 정보 처리(e.g., 패턴 인식, 해석)를 하도록 가속화해줌

즉, 망막은 인간이 시각적 정보를 동시에 센싱하고 전처리할 수 있도록 해준다.

**뉴로모픽 비전(*Neuromorphic Vision*)**은 이러한 <u>생물학적 비전 시스템을 모방</u>하여 기존의 디지털 비전 센서들이 갖고있는 문제점을 해결하고자 하였다. 즉, 저전력(*low-power*) 고효율(*high-efficient*) 이미지 처리를 목표로 한다.

<br>

<br>

## 개념

뉴로모픽 비전은 **뉴로모픽 비전 칩(*Neuromorphic vision chips*)**을 통해 달성된다. 뉴로모픽 비전 칩은 여러 개의 특수 목적 비전 센서를 활용하여 고차원의 입력을 저차원의 출력으로 감소시켜준다.

### 구조

뉴로모픽 비전 칩은 입력을 전처리하기 위해 다양한 특수목적 비전 센서를 여러 개 사용한다. 

* 특수목적 센서의 예시
  * 운동방향 센서(*direction-of-motion sensors*)
  * 속도센서(*velocity sensors*)
  * 추적센서(*tracking sensors*)

이러한 각 지각 센서(*perceptive sensors*)를 통해 고차원의 입력을 저차원의 출력으로 감소시킬 수 있다.

<br>

### 구현

* 뉴런 ⇔ 트랜지스터 회로
* 시냅스 ⇔ 시냅스 모방 소자

뉴로모픽 비전 칩의 기능은 트랜지스터 혹은 새로운 장치 기반의 고전적인 디지털 VLSI<sup>[[개념]](#footnote_VLSI)</sup>기술로 제작된 아날로그 회로를 통해 구현된다. VLSI 기술을 사용하여 크기가 작고 밀집된 형태로 제작할 수 있으므로 기존의 프레임 기반 이미지 센서에 비해 이점을 갖는다.

<br>

### 특징

* **아날로그 방식(*analogue*)** : 뉴로모픽 비전 칩의 기능은 아날로그 회로를 통해 구현된다.
* **에너지 효율적(*energy-efficiently processing*)** : 디지털 시스템에 비해 10,000배 낮은 전력 소비
* **빠른 속도로 동작 (*quickly processing*)** : 폰 노이만 구조가 아닌 병렬 구조로 연산을 수행하기 때문에 병목 현상을 해결하였다.
* **높은 동적 범위 (HDR, *high dynamic range*)**
* **높은 시간적 해상도 (high temporal resolution)**
* **비동기(*asynchronously processing*)** : 비동기 이벤트 스트림을 생성하여 환경을 감지 및 인식
* **사건 중심(*event-driven processing*)** : 프레임 기반이 아닌 사건 기반의 환경 인식
* self-adaptive
* 확장성(*scalable*)

<br>

<br>

# (부록)관련 개념


## 집적회로 (IC, *Integrated Circuit*)

**IC(*Integrated Circuit*, 집적회로)**란, 반도체에 만든 전자회로의 집합을 의미한다.

### 집적도에 따른 분류

IC는 <u>동일한 면적에 얼마나 많은 논리 소자를 집적했는지에 관한 지표</u>인 **집적도**에 따라 분류할 수 있다.

* **SSI (*Small Scale Integration*)** : 소규모 집적회로 (~100개의 게이트)(e.g., 기본적인 게이트 기능, 플립플롭)
* **MSI (*Medium Scale Integration*)** : 중간 규모 집적회로 (100~1,000개의 게이트) (e.g., 인코더, 디코더, 카운터, 레지스터, 멀티플레서, 디멀티플렉서, 소형 기억 장치)
* **LSI (*Large Scale Integration*)** : 대규모 집적회로 (1,000~10,000개의 게이트) (e.g., 컴퓨터의 메인 메모리, 계산기의 부품)
* <a name="footnote_VLSI">**VLSI (*Very Large Scale Integration*)**</a> : 초대규모 집적회로 (10,000~1,000,000개의 게이트) (e.g., 대규모 메모리, 대형 마이크로프로세서, 단일 칩 마이크로프로세서)
  * CMOS 기술의 설계 규칙 규격화로 그 제조 기술이 널리 보급되었음
* **ULSI (*Ultra Large Scale Integration*)** : (1,000,000~개의 게이트) (e.g., 인텔의 486, 펜티엄의 ULSI)

최근에는 하나의 집적회로에 최소 억 단위개의 소자가 집적된다. 따라서 이러한 분류는 점점 의미가 없어지고 있는 추세이다.

<br>

### 제작 구성에 따른 분류

집적회로는 회로의 구성 요소에 따라 다음과 같이 분류할 수 있다.

* **저항-트랜지스터 로직 (*Resistor-transistor logic*, RTL)**

  ![img](https://upload.wikimedia.org/wikipedia/commons/thumb/0/01/Register_transfer_level_-_example_toggler.svg/300px-Register_transfer_level_-_example_toggler.svg.png)

  * 초기의 상용 논리군으로, 디지털 게이트의 기본 동작을 설명하기에 좋은 출발점이다.
  * 최근에는 거의 쓰이지 않는다.

* **다이오드-트랜지스터 로직 (*Diode-transistor logic*, DTL)**

  ![img](https://upload.wikimedia.org/wikipedia/commons/thumb/0/02/DTL_NAND_Gate.svg/220px-DTL_NAND_Gate.svg.png)

  * 최근에는 거의 쓰이지 않는다.

* **트랜지스터-트랜지스터 로직 (*Transistor-transistor logic, TTL*)**

  * 양극성 트랜지스터와 NAND 회로를 사용한다.
  * 가격이 저렴하며 논리회로 구현에 많이 사용된다.

* **이미터-결합 논리 (*Emitter-coupled logic*, ECL)**

  * 기본회로로 NOR 회로를 사용한다.
  * 게이트 지연시간이 적어 고속회로로 사용된다.

* **금속-산화물 반도체 (*Metal-oxide semiconductor*, MOS)**

  * 금속 산화물을 사용하며, 절연체를 입혀서 구성되므로 집적도가 높다.
  * 전력소모가 적으며 제조가 쉽다.

* **상보형 MOS (*Complementary metal-oxide semiconductor*, CMOS)**

  * 인버터회로에 P-채널과 N-채널 트랜지스터를 같이 구성한다.
  * 소비전력이 적다.
  * 속도가 느리다.

<br>

## 기타

* <a name="footnote_폰노이만">**폰 노이만 구조**</a> : 대부분의 컴퓨터가 따르고 있는 기본 구조로, 주기억 장치, 중앙 처리 장치, 입출력 장치로 구성된다. 폰 노이만 구조 기반 반도체 칩은 정보를 순차적으로 처리하므로 대량의 비정형 정보에 대해 병목현상의 한계를 갖는다.

<br>

<br>

# References

* Indiveri, Giacomo, and Rodney Douglas. "Neuromorphic vision sensors." *Science* 288.5469 (2000): 1189-1190.
* Liao, Fuyou, Feichi Zhou, and Yang Chai. "Neuromorphic vision sensors: Principle, progress and perspectives." *Journal of Semiconductors* 42.1 (2021): 013105.


<br>


---

다음 포스팅에서는 뉴로모픽 비전의 현황과 전망, 그리고 사례에 대해 알아본다.


<br>

<br>