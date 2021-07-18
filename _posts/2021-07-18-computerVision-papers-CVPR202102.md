---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments
title: '[CVPR2021-02] CVPR2021-SLAM/AR/Robotics 논문 훑기'
excerpt: "[CVPR2021] CVPR2021에서 SLAM/AR/Robotics 주제을 abstract 위주로 살펴본다."
date: 2021-07-18
last_modified_at: 2021-07-18
categories:
  - computerVision-papers
tags: 
   - [Computer Vision, CVPR, Papers, Review]

use_math: true
comments: true
share : false
---


<!-- 3DVR2021-CVPR 2021 Workshop on 3D Vision and Robotics -->


**CVPR(IEEE Conference on Computer Vision and Pattern Recognition)**은 컴퓨터 비전과 패턴 인식 분야에서 세계적으로 최고 수준의(Top-tier) 학회이다. 이번 포스팅에서는 2021년도 CVPR 학회에서 SLAM/AR/Robotics 주제에 관한 몇 가지 논문들을 Abstract 위주로 살펴본다.


<br>


# 관심 논문

내가 관심있는 주제에 관련된 몇 가지 논문을 abstract 위주로 살펴본다.


## 👾 SLAM/AR/Robotics

<div class="index" style="background-color:white; color:#808080; padding:5px 15px; border: solid 1px #808080">
	<span style="font-weight:bold;">차례</span>
    <ol style="margin-top:0;">
        <li>Tangent Space Backpropagation for 3D Transformation Groups</li>
        <li>Visual Room Rearrangement</li>
        <li>GATSBI: Generative Agent-centric Spatio-temporal Object Interaction</li>
        <li>DexYCB: A Benchmark for Capturing Hand Grasping of Objects</li>
        <li>ContactOpt: Optimizing Contact to Improve Grasps</li>
        <li>ManipulaTHOR: A Framework for Visual Object Manipulation</li>
    </ol>
</div>



### **Tangent Space Backpropagation for 3D Transformation Groups**
   Zachary Teed, Jia Deng
   ([link](https://arxiv.org/abs/2103.12032))

   **배경**

   * 3D 변환(3D transformation) 그룹은 3D vision과 로봇공학에서 널리 사용되지만, vector spaces를 형성하지 않고 대신 smooth manifolds에 놓인다.
   * Euclidean 공간에 3D변환을 포함하는 표준 역전파(backpropagation) 접근 방법은, 수치적인 어려움을 겪고 있다.

   **해결**

   * 3D 변환의 그룹 구조를 활용하고, manifolds의 접선 공간(tangent spaces)에서 역전파를 수행하는 새로운 라이브러리를 소개한다.
   * 3D 변환 그룹 SO, SE, 그리고 Sim을 포함하는 computation graphs에 대해 역전파를 수행하는 문제를 해결한다.

   **결론**

   * 이러한 접근 방법은 수치적으로 더 안정적이며, 구현이 더 쉽고, 다양한 작업 세트에 유용하다는 것을 보였다.
   * plug-and-play Pytorch 라이브러리는 [여기](https://github.com/princeton-vl/lietorch)에서 사용 가능하다.

   <div class="callout" style="background-color:#E9E9E9; color:#808080; border-radius:10px; padding:5px 15px;">
       <span style="font-weight:bold;">요약</span><br>
   	3D 변환 그룹은 3D vision과 로봇 공학에서 널리 사용되지만 vector spaces를 형성하지 않고 manifolds 공간에 놓인다. 따라서 Euclidean 공간에서 backpropagation을 수행해야 하는 어려움이 있는데, 이러한 문제를 3D 변환 그룹의 구조를 활용하고 manifolds의 접선 공간에서 backpropagation을 수행하는 라이브러리로 해결하였다. 수치적으로 더 안정적이며, 구현이 쉽고, 다양한 작업 세트에 유용함을 증명하였다.
   </div>

   <br>

### **Visual Room Rearrangement**
   Luca Weihs, Matt Deitke, Aniruddha Kembhavi, Roozbeh Mottaghi
   ([link](https://arxiv.org/abs/2103.16544))

   <img src="https://ai2thor.allenai.org/static/0f682c0103df1060810ad214c4668718/2f1b1/rearrange-cover2.jpg" alt="img" style="width:600px;" />

   **배경**

   * 최근 Embodied AI 분야에서 상당한 진전이 있었다. 동시에, 많은 연구원들이 embodied agents가 완전히 보이지 않는 환경 내에서 탐색하고(navigate) 상호작용하도록 하는 모델과 알고리즘을 개발하였다.

   **해결: RoomR**

   * 재배열(Rearrangement) 작업을 위한 새로운 데이터 세트와 baseline 모델을 제안한다. 그중 특히 Room Rearrangement 작업에 초점을 맞춘다.
   
      : agent가 방을 탐색하고 objects의 초기 구성을 기록하는 것으로 시작한다. 이후 agent를 제거하고 방 안의 일부 objects의 포즈와 상태(e.g., open/closed)를 변경한다. agent는 방안의 모든 objects의 초기 구성을 복원해야 한다.

   * RoomR이라 불리는 제안된 데이터 세트는 120개의 장면에서 72개의 서로 다른 object 타입을 포함하는 6,000개의 고유한 재배열 설정이 포함되어있다.

   **결론**

   * 실험을 통해 navigation과 object interaction을 포함하는 이러한 interactive task 문제를 해결했음을 밝혔다. 이는, embodied tasks에 대한 최신 최첨단 기술의 기능을 넘어서는 결과이다. 또한 여전히 이러한 유형의 작업에서 완벽한 성능을 달성하는 것과는 거리가 멀다. 
   * 코드와 데이터 세트는 [여기](https://ai2thor.allenai.org/rearrangement/)에서 사용할 수 있다.

   <div class="callout" style="background-color:#E9E9E9; color:#808080; border-radius:10px; padding:5px 15px;">
       <span style="font-weight:bold;">요약</span><br>
   	Room Rearrangement 작업에 초점을 맞춘 새로운 데이터 세트와 baseline 모델을 제안한다. 실험을 통해 navigation과 object interaction을 포함하는 interactive 작업에서 최첨단 기술의 능력을 넘어섬을 밝혔다.
   </div>

   <br>

### **GATSBI: Generative Agent-centric Spatio-temporal Object Interaction**
   Cheol-Hui Min, Jinseok Bae, Junho Lee, Young Min Kim
   ([link](https://arxiv.org/abs/2104.04275))

   **배경**

   * 비전 기반 의사결정 시나리오에서, agent는 다중 entities가 서로 상호작용하는 복잡한 고차원 observations에 직면한다.
   * agent는 필수 구성 요소를 식별하고 시간 범위를 따라 일관되게 전파하는 visual observation에 대한 좋은 scene representation이 필요하다. 

   **해결: GATSBI**

   * GATSBI는 일련의 raw observations을 agents의 행동의 시공간적 맥락을 완전히 포착하는 구조화된 잠재 표현으로 변환할 수 있는 생성모델이다.
   * GATSBI는 활성 agent, 정적 배경, 그리고 수동 오브젝트를 분리하기 위해 unsupervised object-centric scene representation learning을 활용한다. 이후, 분해된 entities간의 인과 관계(causal relationships)를 반영하는 상호작용을 모델링하고, 물리적으로 그럴듯한 미래의 상태를 예측한다.

   **결론**

   * 제안된 모델은 서로 다른 종류의 로봇과 오브젝트가 서로 동적으로 상호작용하는 다양한 환경에 대해 일반화된다.
   * GATSBI는 최첨단 기술에 비해 장면 분해(scene decomposition)와 비디오 예측(video prediction)에서 월등한 성능을 달성하였다.

   <div class="callout" style="background-color:#E9E9E9; color:#808080; border-radius:10px; padding:5px 15px;">
       <span style="font-weight:bold;">요약</span><br>
   	비전 기반 의사 결정 시나리오에서, agent는 복잡한 고차원 관찰에 직면한다. 이때 agent는 필수 구성 요소를 식별하고, 시간이 지나도 일관되게 전파되는 시각적 관찰에 대한 좋은 장면 표현을 필요로한다. 일련의 raw observations을 시공간적 맥락을 포함하는 구조화된 잠재 표현으로 변환할 수 있는 생성 모델인 GATSBI를 통해 이러한 문제를 해결한다. GATSBI는 최첨단 기술에 비해 장면 분해와 비디오 예측에서 월등한 성능을 달성하였다.
   </div>

   <br>

### **DexYCB: A Benchmark for Capturing Hand Grasping of Objects**
   Yu-Wei Chao, Wei Yang, Yu Xiang, Pavlo Molchanov, Ankur Handa, Jonathan Tremblay, Yashraj S. Narang, Karl Van Wyk, Umar Iqbal, Stan Birchfield, Jan Kautz, Dieter Fox
   ([link](https://arxiv.org/abs/2104.04631))

   <img src="/files/2021-07-18-computerVision-papers-CVPR202102/dex-ycv-render.gif" alt="dex-ycv-render" style="width:600px;" />

   **배경**

   * 3D 객체 포즈 추정과 3D 손 포즈 추정은 비전에서 풀리지 않은 두가지 주요한 문제이다. 전통적으로 이러한 문제는 개별적으로 다루어졌지만, 많은 중요한 응용 문제에서 두 가지 기능을 동시에 사용할 필요가 있다. 예를 들어, 로봇 공학에서, 물체의 손 조작을 신뢰성있게 모션캡쳐하는 것이 시범(human demonstration)으로부터 학습하는 것과 유창하고 안전한 인간-로봇 상호작용하는 것 모두에 중요하다.

   **해결: DexYCB**

   * DexYCB는 물체를 손으로 잡는 것을 캡쳐하기 위한 새로운 데이터세트이다.

   **결론**

   * 우선, 교차 데이터 세트 평가(cross-dataset evaluation)를 통해 DexYCB를 관련된 항목과 비교한다.

     이후, 세 가지 관련 tasks에 대한 최첨단 접근 방식의 철저한 벤치마크를 제시한다: 2차원 객체 및 키포인트 감지, 6차원 객체 포즈 추정, 그리고 3차원 손 포즈 추정

     마지막으로, 새로운 로봇공학 관련 tasks를 평가한다: human-to-robot object handover에서 안전한 로봇의 grasps를 생성

     그 결과, 데이터 세트가 중요한 측면에서 발전을 주도할 것이라 예상한다.

   * 데이터 세트와 코드는 [여기](https://dex-ycb.github.io/)에서 사용할 수 있다.

   <div class="callout" style="background-color:#E9E9E9; color:#808080; border-radius:10px; padding:5px 15px;">
       <span style="font-weight:bold;">요약</span><br>
   	3차원 객체 포즈 추정과 3차원 손 포즈 추정은 비전 문제에서 매우 중요한 두 가지 문제이다. 이전까지 개별적으로는 많이 다뤄져 왔으나, 많은 응용분야에서는 두 기능을 동시에 사용할 필요가 있다. 논문은 물체를 손으로 잡는 것을 캡쳐하는 새로운 데이터 세트 DexYCB를 제안하여 이러한 문제를 해결한다. 실험 결과는 로봇 공학 관련된 tasks에서 발전을 주도할 것을 시사한다.
   </div>

   <br>

### **ContactOpt: Optimizing Contact to Improve Grasps**
   Patrick Grady, Chengcheng Tang, Christopher D. Twigg, Minh Vo, Samarth Brahmbhatt, Charles C. Kemp
   ([link](https://arxiv.org/abs/2104.07267))

   **배경**

   * 손과 물체간의 물리적인 접촉은 human grasps에서 중요한 역할을 한다. 

   **해결: ContactOpt**

   * hand mesh와 object mesh가 주어졌을 때, 접촉 데이터의 ground truth에 대해 학습된 딥 모델이 메시 표면에 걸쳐 바람직한 접촉을 추론한다.

     이후, ContactOpt는 미분 가능한 contact model을 사용하여 원하는 접촉을 달성하기 위해 손 포즈를 효율적으로 최적화한다. 

     특히, 이러한 contact model은 메시 상호 침투(mesh interpenetration)를 야기하여 손의 변형 가능한 연조직(soft tissue)을 근사화한다.

   **결론**

   * 물체와의 접촉을 예상하기 위해 손 포즈를 최적화하는 방식을 통해, 이미지 기반 방법을 통해 추론된 손 포즈를 개선할 수 있음을 보였다.
   * 이러한 방법이 ground truth contact과 더 잘 일치하는 grasps 결과를 도출하며, 더 낮은 kinematic error를 갖고, 인간 참여자가 더 선호하는 방법이라 평가된다.

   <div class="callout" style="background-color:#E9E9E9; color:#808080; border-radius:10px; padding:5px 15px;">
       <span style="font-weight:bold;">요약</span><br>
   	손과 물체의 물리적 접촉은 human grasps에서 중요한 역할을 한다. hand mesh와 object mesh가 주어졌을 때, ground truth를 통해 딥 모델이 mesh 표면에 걸쳐 바람직한 contact 결과를 추론하도록 한다. 이후 ContactOpt를 이용하여 손 포즈를 효율적으로 최적화함으로써 손 포즈 추론 결과를 개선할 수 있음을 보였다. 이러한 방법은 더 좋은 일치율을 보이며, 더 낮은 kinematic error를 가지고, 인간 참여자가 더 선호하는 방법이라는 점에서 장점을 갖는다.
   </div>

   <br>

### **ManipulaTHOR: A Framework for Visual Object Manipulation**
   Kiana Ehsani, Winson Han, Alvaro Herrasti, Eli VanderBilt, Luca Weihs, Eric Kolve, Aniruddha Kembhavi, Roozbeh Mottaghi
   ([link](https://arxiv.org/abs/2104.11213))

   **배경**

   * Embodied AI 도메인은 특히 환경 내에서 agents를 탐색하는 데(navigating)에서 상당한 발전이 있었다.
   * 이러한 초기 성공은 agent가 그들의 환경 내의 물체와 적극적으로 상호작용해야하는 작업을 커뮤니티가 처리할 수 있도록 building blocks을 마련하였다.
   * 물체 조작(object manipulation)은 로봇 공학 커뮤니티 내에서 확립된 연구 영역으로, 조작기 동작(manipulator motion), 잡기(grasping), 그리고 긴 수평선 계획(long-horizon planning)을 포함한 여러 문제를 제기한다. 특히 이러한 문제는 시각적으로 풍부하고 복잡한 장면, 모바일 에이전트를 이용한 조작(탁상조작(tabletop manipulation)과 반대되는 말), 그리고 보이지 않는 환경 및 물체에 대한 일반화와 관련되어 자주 간과되는 실제 설정과 연관되어 있다.

   **해결**

   * 물리적 기반의 시각적으로 풍부한 AI2-THOR 프레임워크를 기반으로 구축된 object manipulation을 위한 프레임워크를 제안하고, ArmPointNav로 알려진 Embodied AI 커뮤니티에 새로운 도전을 제시한다.
   * 이러한 작업은 유명한 포인트 탐색 작업(point navigation task)을 물체 조작(object manipulation)으로 확장하며, 3차원 장애물 회피, 폐색(occlusion)이 있는 상태에서 개체 조작, 그리고 장기적인 계획이 필요한 다중 객체 조작을 포함한 새로운 과제를 제공한다.

   **결론**

   * PointNav 챌린지에서 성공한 인기있는 학습 패러다임은, 가능성을 보여주었지만 개선의 여지가 많다.

   <div class="callout" style="background-color:#E9E9E9; color:#808080; border-radius:10px; padding:5px 15px;">
       <span style="font-weight:bold;">요약</span><br>
   	손과 물체의 물리적 접촉은 human grasps에서 중요한 역할을 한다. hand mesh와 object mesh가 주어졌을 때, ground truth를 통해 딥 모델이 mesh 표면에 걸쳐 바람직한 contact 결과를 추론하도록 한다. 이후 ContactOpt를 이용하여 손 포즈를 효율적으로 최적화함으로써 손 포즈 추론 결과를 개선할 수 있음을 보였다. 이러한 방법은 더 좋은 일치율을 보이며, 더 낮은 kinematic error를 가지고, 인간 참여자가 더 선호하는 방법이라는 점에서 장점을 갖는다.
   </div>



<br>



## 👻 AR

<div class="index" style="background-color:white; color:#808080; padding:5px 15px; border: solid 1px #808080">
	<span style="font-weight:bold;">차례</span>
    <ol style="margin-top:0;">
        <li>Stay Positive: Non-Negative Image Synthesis for Augmented Reality</li>
		<li>HDR Environment Map Estimation for Real-Time Augmented Reality</li>
    	<li>NeuralHumanFVV: Real-Time Neural Volumetric Human Performance Rendering using RGB Cameras</li>
    </ol>
</div>



### **Stay Positive: Non-Negative Image Synthesis for Augmented Reality**
   Katie Luo, Guandao Yang, Wenqi Xian, Harald Haraldsson, Bharath Hariharan, Serge Belongie
   ([link](https://openaccess.thecvf.com/content/CVPR2021/html/Luo_Stay_Positive_Non-Negative_Image_Synthesis_for_Augmented_Reality_CVPR_2021_paper.html))

   **배경**

   * 광학 투시(optical see-through) 및 프로젝터 증강 현실과 같은 애플리케이션에서, 이미지를 생성하는 것은 기존의 이미지에만 빛을 추가할 수 있는 비음성(non-negative) 이미지 생성 문제를 해결하는 것과 같다.

     그러나 대부분의 이미지 생성 기법은, 각 픽셀에 임의의 색상을 할당할 수 있다고 가정하기 때문에 이러한 문제 설정에 적합하지 않다.

   * 사실, 기존 방법의 순진한 적용은 빛을 추가함으로써 더 어두운 픽셀을 생성할 수 없기 때문에 MNIST digits와 같이 단순한 도메인에서도 실패한다.

   **해결**

   * 인간의 시각 시스템은 밝기와 대비의 특정 공간 구성을 포함한 착시 현상에 속을 수 있다. 이러한 동작을 활용하여 무시할 수 있는 인공물(artifacts)로 고품질 이미지를 생성할 수 있다는 점이 주요 인사이트이다.

     예를 들어, 주변의 픽셀을 밝게 만들어 더 어두운 패치의 환상을 만들 수 있다.

   * semantic과 non-negativity constraints 모두를 충족하는 이미지를 생성하기 위한 신박한 최적화 절차를 통해 문제를 해결할 수 있다.

   **결론**

   * 이러한 접근 방식은 기존의 최첨단 방식을 통합할 수 있으며, image-to-image translation과 style transfer를 포함한 다양한 tasks에서 훌륭한 성능을 보여준다.

   <div class="callout" style="background-color:#E9E9E9; color:#808080; border-radius:10px; padding:5px 15px;">
       <span style="font-weight:bold;">요약</span><br>
   	증강현실과 같은 어플리케이션에서 이미지를 생성하기 위해서는 기존의 이미지에 빛만을 추가할 수 있다. 이러한 제약은 빛 추가를 통해 더 어두운 픽셀을 생성할 수 없다는 문제로 이어진다. 주변의 픽셀을 밝게 만들어 더 어둡게 보이도록 하는것과 같이 인간의 시각 시스템에 착시를 불러일으키는 최적화 절차를 통해 이러한 문제를 해결할 수 있다. 이러한 접근 방식은 기존의 방식을 통합할 수 있으며, 다양한 tasks에서 좋은 성능을 보였다.
   </div>

   <br>

### **HDR Environment Map Estimation for Real-Time Augmented Reality**
   Gowri Somanath, Daniel Kurz
   ([link](https://arxiv.org/abs/2011.10687))

   <img src="/files/2021-07-18-computerVision-papers-CVPR202102/image-20210714215521977.png" alt="image-20210714215521977" style="width:600px;" />

   **배경**

   * 다양한 기하학적 물체 렌더링을 지원하기 위해서는 환경맵이 반드시 high dynamic range이어야 하며, 장면에서 물체와 특징을 표현하기 위해 충분히 고해상도 이미지를 가져야 한다.

   **해결**

   * 실시간으로 좁은 시야의 LDR 카메라 영상으로부터 HDR 환경 맵을 추정하는 방법을 제안한다.
   * 이를 통해 증강 현실을 사용하여 실제 물리적 환경으로 렌더링된 거울(mirror)부터 확산(diffuse)까지, 모든 재료 마감의 가상 물체에 지각적으로 훌륭한 반사와 음영이 가능하다.
   * 이러한 방식은 효율적인 CNN 아키텍처인 EnvMapNet을 기반으로 하며, 생성된 이미지에 대한 ProjectionLoss와 적대적인 교육에 대한 ClusterLoss라는 두 가지 신박한 losses와 함께 end-to-end로 학습된다.

   **결론**

   * 최첨단 방식과의 정성적·정량적 비교를 통해, 이러한 알고리즘이 추정된 광원의 방향 오차를 50% 이상 감소시키고, 3.7배 더 낮은 Frechet Inception Distance(FID)를 달성함이 입증되었다.
   * 또한, iPhone XS 환경에서 9ms 미만으로 이러한 NN 모델을 실행시킬 수 있으며, 이전까지 실제 환경에서 볼 수 없었던 시각적으로 일관된 가상 객체를 실시간으로 렌더링할 수 있는 모바일 어플리케이션을 선보였다.

   <div class="callout" style="background-color:#E9E9E9; color:#808080; border-radius:10px; padding:5px 15px;">
       <span style="font-weight:bold;">요약</span><br>
   	다양한 기하학적 물체를 렌더링하기 위해서는 HDR이어야 하며 충분히 고해상도 이미지를 가져야 한다. EnvMapNet을 기반으로 하며 ProjectionLoss와 ClusterLoss라는 두 가지 신박한 losses를 통해 좁은 시야의 LDR영상으로부터 HDR 환경 맵을 추정하는 방법을 통해 이러한 문제를 해결한다. 이러한 방식은 추정된 광원의 방향에 대한 오차를 50%이상 감소시켰으며, 3.7배 더 낮은 Frechet Inception Distance를 달성하였다.
   </div>

   <br>


### **NeuralHumanFVV: Real-Time Neural Volumetric Human Performance Rendering using RGB Cameras**
   Xin Suo, Yuheng Jiang, Pei Lin, Yingliang Zhang, Kaiwen Guo, Minye Wu, Lan Xu
   ([link](https://arxiv.org/abs/2103.07700))

   <img src="/files/2021-07-18-computerVision-papers-CVPR202102/image-20210714231452206.png" alt="image-20210714231452206" style="width:600px;" />

   **배경**

   * human activities의 4차원 재구성 및 렌더링은 몰입형 VR/AR 경험을 위해 매우 중요하다. 
   * 최근 발전은 여전히 sparse multi-view RGB 카메라의 입력 이미지에 존재하는 세부 수준으로 정밀한 형상(geometry)과 텍스처 결과를 복구하지 못한다.

   **해결: NeuralHumanFVV**

   * NeuralHumanFVV는 임의의 새로운 관점에서 human activities에 대한 고품질 기하학과 사실적인 텍스처 모두를 생성하기 위한, real-time neural human performance 캡쳐 및 렌더링 시스템이다.
   * 실시간 내재된 기하학 추론을 위한 계층적 샘플링 전략뿐만 아니라, 새로운 관점에서 고해상도(e.g., 1k) 및 사실적인 텍스처 결과를 생성하는 새로운 신경 혼합 방식(neural blending scheme)를 제안한다.
   * 또한, neural normal blending을 채택하여, 기하학적 디테일을 향상시키고 이러한 신경 기하학 및 텍스처 렌더링을 다중 작업 학습 프레임워크로 공식화하였다.

   **결론**

   * 광범위한 실험을 통해, 이러한 접근 방식이 도전적인 human performances를 위한 고품질 기하학 및 사실적인 자유로운 시점 구성을 달성할 수 있음을 보여주었다.

   <div class="callout" style="background-color:#E9E9E9; color:#808080; border-radius:10px; padding:5px 15px;">
       <span style="font-weight:bold;">요약</span><br>
   	human activities의 4차원 재구성 및 렌더링은 몰입형 AR/VR 경험에 매우 중요하다. 하지만 여전히 sparse multi-view RGB 카메라를 통해서는 정밀한 geometry와 텍스처 결과를 복구할 수 없다. 임의의 새로운 관점에서 human activities에 대한 고품질 기하학과 사실적인 텍스처 모두를 생성할 수 있는 NeuralHumanFVV를 통해 이러한 문제를 해결하였다.
   </div>



<br>

---

다음 포스팅에서 3D vision 주제에 관한 논문들을 다루도록 한다.





<br>





# References

1. CVPR 2021: [http://cvpr2021.thecvf.com/](http://cvpr2021.thecvf.com/)
2. brunch/kakao-it: [https://brunch.co.kr/@kakao-it/297](https://brunch.co.kr/@kakao-it/297)
3. CVPR-2021-Paper-Satistics : [https://github.com/hoya012/CVPR-2021-Paper-Statistics?fbclid=IwAR0MGG3x-9bDU8YjVp-UGHcJDAXUspNwJ3Iy-o17oi7UFbTSFGcKS_OqbaQ](https://github.com/hoya012/CVPR-2021-Paper-Statistics?fbclid=IwAR0MGG3x-9bDU8YjVp-UGHcJDAXUspNwJ3Iy-o17oi7UFbTSFGcKS_OqbaQ)
4. 52CV/CVPR-2021-Papers: [https://github.com/52CV/CVPR-2021-Papers](https://github.com/52CV/CVPR-2021-Papers)
5. amusi/CVPR2021-Papers-with-Code: [https://github.com/amusi/CVPR2021-Papers-with-Code](https://github.com/amusi/CVPR2021-Papers-with-Code)



<br>

