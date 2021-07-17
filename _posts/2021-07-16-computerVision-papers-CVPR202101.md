---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments
title: '[CVPR2021-01] CVPR2021 트렌드 및 수상 논문 살펴보기'
excerpt: "[CVPR2021] CVPR2021 주요 트렌드와 수상 논문을 살펴본다."
date: 2021-07-16
last_modified_at: 2021-07-16
categories:
  - computerVision-papers
tags: 
   - [Computer Vision, CVPR, Papers, Review]

use_math: true
comments: true
share : false
---


<!-- 3DVR2021-CVPR 2021 Workshop on 3D Vision and Robotics, since 2021-06-19 -->


**CVPR(IEEE Conference on Computer Vision and Pattern Recognition)**은 컴퓨터 비전과 패턴 인식 분야에서 세계적으로 최고 수준의(Top-tier) 학회이다. 이번 포스팅에서는 2021년도 CVPR 학회에서 수상한 몇 가지 논문과 내가 관심있는 주제에 대한 몇 가지 논문들을 Abstract 위주로 살펴본다.



# 트렌드

> 이번 CVPR에 가장 많은 논문을 제출한 국내 기관은 총 20편 기록을 세운 연세대다. 다음으로 서울대가 17편, KAIST가 10편 논문을 발표했다.
>
> 기업 중에서는 네이버가 총 8개로 최다 논문 채택 수를 기록했다. 네이버랩스 유럽에서 발표한 논문 중 하나는 심층발표 세션에 채택됐다. 카카오는 이번 CVPR에서 2개 논문을 발표했는데, 이 중 하나인 HOTR 논문은 심층발표 세션에서 공개됐다.

CVPR 채택 논문의 제목 키워드를 추출하여 트렌드를 분석한 결과(출처: [이호성 코그넥스 리서치 엔지니어](https://github.com/hoya012/CVPR-2021-Paper-Statistics?fbclid=IwAR0MGG3x-9bDU8YjVp-UGHcJDAXUspNwJ3Iy-o17oi7UFbTSFGcKS_OqbaQ))는 다음과 같다.

## 주요 키워드

CVPR 2021 채택 논문의 주요 키워드를 시각화한 결과는 다음과 같다.

<img src="https://github.com/hoya012/CVPR-2021-Paper-Statistics/raw/main/2021_cvpr/keyword_cloud.png" alt="img" style="width:800px;" />

이를 CVPR 2020 채택 논문과 비교한 결과는 다음과 같다.

<img src="https://github.com/hoya012/CVPR-2021-Paper-Statistics/raw/main/2021_cvpr/top_keywords_2020%2B2021.png" alt="img" style="width:800px" />

* Image, detection, 3d, object, video, segmentation 등은 이전 년도와 마찬가지로 주요 키워드를 차지했다.

* unsupervised, self-supervised, semi-supervised는 이전과 비교했을 때 대략 1.5배 빈도수가 증가했다.

  * unsupervised : 52 -> 77

  * self-supervised: 33 -> 50

  * semi-supervised: 25 -> 35





<br>





# CVPR'21 PAPER AWARDS

## Best Student Paper Honorable Mentions

1. **Less is More: CLIPBERT for Video-and-Language Learning via Sparse Sampling**  
   Jie Lei, Linjie Li, Luowei Zhou, Zhe Gan, Tamara L. Berg, Mohit Bansal, Jingjing Liu
   ([link](https://openaccess.thecvf.com/content/CVPR2021/html/Lei_Less_Is_More_ClipBERT_for_Video-and-Language_Learning_via_Sparse_Sampling_CVPR_2021_paper.html))

   <img src="/files/2021-07-16-computerVision-papers-CVPR202101/image-20210712104655664.png" alt="image-20210712104655664" style="width:600px;" />

   

   

   

   **배경**

   * 일반적인 video-and-language 학습 방법에 의하면, vision models의 dense video features과 language models의 text features로부터 뉴런 모델을 학습시킨다.

   * (문제점) 이러한 feature extractors는 일반적으로 target domains와 다른 tasks에 대해 독립적으로 학습되므로, 이러한 고정된 features는 downstream tasks에 대해 차선책으로 사용된다.

     또한 dense video features의 높은 연산 과부하로인해 feature extractors를 기존의 접근 방식에 직접적으로 연결하여 finetuning하기 어렵다.

   * (해결책) 이러한 딜레마를 해결하기위해, 포괄적인 프레임워크인 CLIPBERT을 제안한다.

   **해결: CLIPBERT**

   * video-and-language tasks를 학습하기 위한 합리적인 학습 방법이다. 
   * 각 학습 단계에서, 비디오로부터 sparse sampling된 하나 혹은 아주 짧은 비디오 클립만을 사용한다.

   **결론**

   * 6 datasets에 대해 test-to-video 검색 및 video 질의응답에 대한 실험에 의하면, CLIPBERT는 기존의 방식(전체 비디오를 활용)보다 더 높은 정확도를 달성하였다.

     즉, 전체 비디오로부터 offline features를 추출하는 방식(기존)에 비교했을 때, 단지 몇 개의 드문드문하게 샘플링된 클릭만 사용하는 것(CLIPBERT)이 "less is more, 덜한 것이 더한 것이다"를 증명하는 셈이다.

   * 3초짜리 GIF 영상부터 180초짜리 유튜브 영상까지, 서로다른 도메인과 길이를 갖는 데이터셋에 대해 실험한 결과는 이러한 접근 방식이 일반화가능하다는 것을 증명한다.

   <div class="callout" style="background-color:#E9E9E9; color:#808080; border-radius:10px; padding:5px 15px;">
       <span style="font-weight:bold;">요약</span><br>
       video-and-language 학습방법은 video로부터 추출한 특징과 text로부터 추출한 특징을 cross-modal modeling하여 결과를 도출한다. 기존에는 전체 비디오에 대해 특징을 추출하여 사용했으나, 연산 과부하로인해 정밀튜닝이 어렵다는 문제가 있다. 이러한 문제를 해결하기위해 비디오를 짧은 클립 영상으로 나눈 뒤 사용하는 CLIPBERT 모델을 제안하였다.
   </div>

    <br>


2. **Binary TTC: A Temporal Geofence for Autonomous Navigation**
   Abhishek Badki, Orazio Gallo, Jan Kautz, Pradeep Sen
   ([link](https://openaccess.thecvf.com/content/CVPR2021/html/Badki_Binary_TTC_A_Temporal_Geofence_for_Autonomous_Navigation_CVPR_2021_paper.html))

   **배경**

   * Time-to-contact(TTC)은 경로 계획(path planning)을 위한 강력한 도구이다.

     > **Time-to-contact(TTC)란?**<br>
     > "물체가 관찰자의 평면과 충돌하는 시간"이다.<br>
     > e.g., 빛(물체)이 사물에 반사되어 우리 눈(관찰자의 평면)에 충돌하는데, 충돌에 걸리는 시간을 TTC라고 한다.

     **TTC의 장점**

     1. 물체의 깊이(depth), 속도(velocity), 그리고 가속도(acceleration) 보다 더 많은 정보를 제공한다.
     2. 보정되지 않은 단안 카메라(a monocular, uncalibrated camera)만 있으면 된다.

     **(문제점) TTC의 단점**

     1. 각 픽셀에 대한 TTC의 회귀(regressing)가 항상 바로 오는 것은 아니다.
     2. 대부분의 기존 방식은 장면에 대한 추정을 과하게 단순화한다.

   **해결: Binary TTC**

   * 일련의 더 단순한 이진 분류를 통해 TTC를 추정하여 TTC의 기존 문제점을 해결한다.

   **결론**

   * 관찰자가 특정시간 내에 장애물과 충돌하는지 여부는 때로 정확한 픽셀당 TTC를 아는 것보다 더 중요하며, 이는 짧은 지연시간으로 측정할 수 있다.

     이러한 시나리오에서, Binary TTC 방법은 기존의 방법보다 25배 이상 빠른 6.4ms geofence를 제공한다.

     또한 계산하기 위한 예산이 충분한 경우, 연속값을 포함하는 임의의 미세 양자화(fine quantization)를 이용하여 픽셀당 TTC를 추정할 수 있다.

   * 충분히 높은 프레임 속도로 TTC정보(binary 혹은 coarsely quantized)를 제공하는 최초의 방법이라 생각된다.

   <div class="callout" style="background-color:#E9E9E9; color:#808080; border-radius:10px; padding:5px 15px;">
       <span style="font-weight:bold;">요약</span><br>
       TCC는 다른 방법에 비해 더 많은 정보를 제공한다는 점과 단지 단안 카메라만 있으면 된다는 점에서 장점을 갖는다. 하지만 TTC는 regression이 언제나 직접적이지 않으며 추정을 과하게 단순화한다는 단점이 있다. 이러한 단점을 해결하기위해 이진 분류로 TTC를 추정하는 방법을 제안한다. 
   </div>
  <br>
    

3. **Real-Time High-Resolution Background Matting**
   Shanchuan Lin, Andrey Ryabtsev, Soumyadip Sengupta, Brian Curless, Steve Seitz, Ira Kemelmacher-Shlizerman
   ([link](https://openaccess.thecvf.com/content/CVPR2021/html/Lin_Real-Time_High-Resolution_Background_Matting_CVPR_2021_paper.html))

   <img src="/files/2021-07-16-computerVision-papers-CVPR202101/image-20210713102115311-1626414735343.png" alt="image-20210713102115311" style="width:600px;" />

   **배경**

   * background matting 기술에서 실시간으로 고해상도 이미지를 처리하며, 가닥 수준의 머리카락 세부 사항을 유지하면서 고품질의 알파 매트(alpha matte)를 계산하는 것은 어렵다.

   **해결: 실시간, 고해상도 배경 교체 기술**

   * 최신 GPU에서, 30fps(4K 해상도) 혹은 60fps(HD)로 동작한다.

   * background matting를 기반으로 동작한다.

     > **background matting란?**
     > 배경의 추가 프레임이 캡쳐되어 알파 매트(alpha matte)와 전경 레이어(foreground layer)에 대한 정보를 제공하는 데에 사용된다.

   * <img src="/files/2021-07-16-computerVision-papers-CVPR202101/image-20210713102646869.png" alt="image-20210713102646869" style="width:600px;" />

     베이스 네트워크는 저해상도 결과를 계산하며, 이는 두 번째 네트워크에 의해 선택적 패치에 대해 고해상도로 작동한다.

   * 2개의 대규모 비디오 및 이미지 매트 데이터셋인 VideoMatte240K와 PhotoMatte 13K/85를 소개한다.

   **결론**

   * 이전의 최첨단 배경 매트 기술과 비교했을 때, 더 높은 품질의 결과를 산출함과 동시에 속도와 해상도 측면에서 모두 극적인 향상을 가져왔다.

   <div class="callout" style="background-color:#E9E9E9; color:#808080; border-radius:10px; padding:5px 15px;">
       <span style="font-weight:bold;">요약</span><br>
       기존의 배경 매팅 기술에서 실시간으로 고해상도 이미지를 처리하면서 세부사항을 유지하는 알파 매트를 계산하는 것이 매우 어려웠다. 첫 번째 네트워크에서 대략적으로 결과를 계산하고 두 번째 네트워크에서 선택적 패치에 대해 세부적으로 결과를 계산함으로써, background matting을 기반으로 한 실시간 고해상도 배경 교체 기술을 달성했다.
   </div>



<br>



## Best Student Paper

1. **Task Programming: Learning Data Efficient Behavior Representations**
   Jennifer J. Sun, Ann Kennedy, Eric Zhan, David J. Anderson, Yisong Yue, Pietro Perona
   ([link](https://openaccess.thecvf.com/content/CVPR2021/html/Sun_Task_Programming_Learning_Data_Efficient_Behavior_Representations_CVPR_2021_paper.html))

   **배경**

   * 전문 도메인 지식은 보통 심층적인 분석을 위해 도메인 전문가로부터 training sets에 정확하게 주석을 달아야하지만, 부담스럽고 시간이 많이 소요되는 일이다.
   * 이러한 문제는 video tracking data에서, 행위자의 관심있는 움직임이나 행동이 감지되는 자동화된 행동 분석에서 두드러지게 발생한다.

   **해결: TREBA**

   * annotation을 위한 수고를 덜기 위해 제안된, multi-task self-supervised learning 기반으로 하는 행동 분석을 위한, annotation-sample 효율적인 궤적 임베딩을 학습하는 방법이다.
   * 도메인 전문가에 의해 구조화된 지식을 명시적으로 인코딩하는 프로그램을 사용하는 "task programming"이라는 프로세스를 통해, 도메인 전문가가 효율적으로 설계할 수 있습니다.
   * data annotation time 대신, 적은 수의 프로그래밍된 작업에 시간을 사용함으로써, 전체 도메인 전문가의 노력을 줄일 수 있다.

   **결론**

   * 행동을 식별하기 위해 전문 도메인 지식이 사용되는 신경과학의 데이터를 통해 이러한 trade-off를 평가한다.

   * 생쥐와 초파리, 두 개의 도메인에 걸쳐 세 가지 데이터셋에 대한 실험 결과를 제시한다.

     TREBA의 임베딩을 사용하여, 최첨단 기능에 비교했을 때, 정확도를 손상시키지 않으며 annotation 부담을 최대 10배까지 줄였다.

     결과적으로, 작업 프로그래밍(task programming)과 자체 감독(self-supervision) 방식이 도메인 전문가의 annotation 부담을 줄여주는 효과적인 방법임을 시사한다.

   <div class="callout" style="background-color:#E9E9E9; color:#808080; border-radius:10px; padding:5px 15px;">
       <span style="font-weight:bold;">요약</span><br>
       전문 도메인 지식은 도메인 전문가에 의해 주석되어야 하는데, 이는 매우 비용 소모적이라는 문제가 있다. multi-task self-supervised learning 기반으로 동작하는 TREBA 방법에서, task programming이라는 프로세스를 통해 도메인 전문가에 의해 구조화된 지식을 명시적으로 인코딩함으로써 이러한 문제를 해결한다.
   </div>



<br>



## Best Paper Honorable Mentions

1. **Exploring Simple Siamese Representation Learning**
   Xinlei Chen, Kaiming He
   ([link](https://openaccess.thecvf.com/content/CVPR2021/html/Chen_Exploring_Simple_Siamese_Representation_Learning_CVPR_2021_paper.html))

   **배경**

   * Siamese networks는 unsupervised visual representation learning을 위한 다양한 최근 모델에서 일반적인 구조가 되었다.

     이러한 구조는, 솔루션 붕괴를 막기 위한 특정 조건에 따라, 하나의 이미지의 두 가지 augmentations 사이의 유사성을 극대화한다.

   **해결**

   * 단순한 Siamese networks가 다음의 것들을 사용하지 않아도 의미있는 표현을 학습할 수 있다는 놀라운 경험적 결과를 보고한다.

     (1) 음수 샘플 쌍 (negative sample pairs)
     (2) 대규모 배치 (large batches)
     (3) 운동량 인코더 (momentum encoders)

   **결론**

   * 실험은, loss와 structure에 대한 collapsing 솔루션이 존재하지만, collapsing을 막기 위해서는 stop-gradient 연산의 역할이 필수적임을 보여준다.

     논문은, stop-gradient의 의미에 대한 가설을 제공하고 이를 확인하는 개념 증명(proof-of-concept) 실험을 추가로 보여준다.

   * 논문의 "SimSiam" 방법은 ImageNet과 downstream tasks에서 경쟁력있는 결과를 달성한다.

   * 이러한 간단한 기준이 unsupervised representation learning에 대한 Siamese 아키텍처의 역할에 대해 다시 한 번 생각해볼 수 있는 동기가 되기를 바란다.

   <div class="callout" style="background-color:#E9E9E9; color:#808080; border-radius:10px; padding:5px 15px;">
       <span style="font-weight:bold;">요약</span><br>
       unsupervised representation learning에서는 일반적으로 Siamese 네트워크 구조가 사용된다. 하지만 논문은 Siamese 네트워크에서 음수 샘플 쌍, 대규모 배치, 운동량 인코더는 필수적인 조건이 아님을 실험을 통해 밝혔다. 또한 붕괴를 막기 위해서는 stop-gradient 연산이 필수적임을 밝히며 이에 대한 가설을 증명했다.
   </div>
  <br>
    

2. **Learning High Fidelity Depths of Dressed Humans by Watching Social Media Dance Videos**
   Yasamin Jafarian, Hyun Soo Park
   ([link](https://openaccess.thecvf.com/content/CVPR2021/html/Jafarian_Learning_High_Fidelity_Depths_of_Dressed_Humans_by_Watching_Social_CVPR_2021_paper.html))

   **배경**

   * 옷입은 인간의 기하학 학습에서 주요 과제는, ground truth data (e.g., 3D scanned models)의 제한된 가용성에 있다. 이는 실제 이미지에 적용할 때 3D human reconstruction의 성능 저하를 초래한다.

   **해결**

   * 이러한 문제를 해결하기 위해 새로운 데이터 리소스를 활용하였다.
     : 다양한 외모, 옷 스타일, 퍼포먼스, 정체성을 아우르는 여러 개의 소셜 미디어 댄스 비디오

     각 비디오는 3D ground truth geometry가 부족한 한 사람의 몸과 옷의 역동적인 움직임을 묘사한다.

   * 이러한 비디오를 사용하기 위해, 지역변환(local transformtation)을 사용하는 새로운 방법을 제시한다. 이러한 방법은, 이미지에서 예측된 사람의 local geometry를 다른 시간의 다른 이미지의 것으로 변환한다. 

   * 이러한 변환을 통해 예측된 geometry는 다른 이미지로부터 변형된 geometry에 의해 self-supervised 될 수 있다.

   * 추가로, 기하학적 일관성을 극대화하여 로컬 텍스처, 주름 및 음영에 대해 매우 민감하게 반응하는 표면 법선(surface normals)와 함께, 깊이를 공동으로 학습한다.

   **결론**

   * 이러한 방법은 end-to-end 학습이 가능하므로, 입력 실제 이미지에 대해 충실한 정밀 geometry를 예측하는, 충실도 높은 깊이 추정이 가능하다.
   * 이러한 방법은, 실제 이미지와 렌더링된 이미지 모두에서 최첨단 human depth 추정과 human shape 복구 접근 방식을 능가한다.

   <div class="callout" style="background-color:#E9E9E9; color:#808080; border-radius:10px; padding:5px 15px;">
       <span style="font-weight:bold;">요약</span><br>
       옷을 입은 인간의 기하학 학습에서는 ground truth 데이터가 제한된 가용성을 갖는다는 한계가 있다. 이를 해결하기 위해 여러 개의 소셜 미디어 댄스 비디오를 활용하는 방안을 제시한다. 이러한 방법은 최첨단 human depth 추정과 human shape 복구 접근 방식보다 좋은 성능을 달성했다.
   </div>



<br>



## Best Paper

1. **GIRAFFE: Representing Scenes As Compositional Generative Neural Feature Fields**
   Michael Niemeyer, Andreas Geiger
   ([link](https://openaccess.thecvf.com/content/CVPR2021/html/Niemeyer_GIRAFFE_Representing_Scenes_As_Compositional_Generative_Neural_Feature_Fields_CVPR_2021_paper.html))

   **배경**

   * Deep generative models은 고해상도에서 사실적인 이미지 합성(photorealistic image synthesis)를 가능하게 해준다. 하지만, 너무 많은 응용 프로그램에 대해 충분하지 않기 때문에, content creation 또한 제어할 수 있어야 한다.

   * 최근 몇 가지 연구에서 데이터 변동의 근본적인 요인을 푸는 방법을 연구하고 있으나, 대부분은 2차원에서 동작하므로 우리의 세계가 3차원이라는 사실을 무시한다. 

     또한 장면의 구성적인 특성을 고려한 연구는 거의 존재하지 않는다.

   **해결**

   * 주요 가설은 "합성 3D 장면 표현을 생성 모델에 통합하면, 더 제어 가능한 이미지 합성으로 이어질 것"이다.
   * 장면을 합성 생성 신경 기능 필드(compositional generative neural feature fields)로 표현함으로써, 추가적인 supervision 없이 구조화되지 않은 이미지 컬렉션으로부터 학습하는 동안, 하나 또는 다중 객체를 배경 뿐만 아니라 개별 객체의 모양 및 외형으로부터 분리할 수 있다. 
   * 이러한 장면 표현을 neural rendering pipeline과 결함하여, 더 빠르고 사실적인 이미지 합성 모델을 생성한다.

   **결론**

   * 실험에 의해, 이러한 모델이 개별 객체를 풀 수 있고, 장면에서의 변환 및 회전뿐만 아니라 카메라 포즈를 변경할 수도 있음을 보였다.

   <div class="callout" style="background-color:#E9E9E9; color:#808080; border-radius:10px; padding:5px 15px;">
       <span style="font-weight:bold;">요약</span><br>
       Deep generative models은 고해상도에서 사실적인 이미지 합성을 가능하게 해주지만, content 생성 제어에 대한 연구는 많이 이루어지지 않았다. 논문은, 합성 3D 장면 표현을 생성 모델에 통합함으로써 이를 해결하였다. 실험을 통해 이러한 모델이 개별 객체를 풀고, 장면에서의 변환과 회전뿐만 아니라 카메라의 포즈도 변경할 수 있음을 보였다.
   </div>


<br>

---

아직 모르는게 많다보니, 보다 다양한 주제의 논문을 읽어보고 싶다. 원래는 포스팅 하나에 전부 다루고싶었지만 분량상 나누어 다룰 예정이다. 다음 포스팅에서 내가 관심있는 주제에 관한 CVPR2021 논문을 이어서 다루도록 한다.





<br>





# References

1. CVPR 2021: [http://cvpr2021.thecvf.com/](http://cvpr2021.thecvf.com/)
2. brunch/kakao-it: [https://brunch.co.kr/@kakao-it/297](https://brunch.co.kr/@kakao-it/297)
3. CVPR-2021-Paper-Satistics : [https://github.com/hoya012/CVPR-2021-Paper-Statistics?fbclid=IwAR0MGG3x-9bDU8YjVp-UGHcJDAXUspNwJ3Iy-o17oi7UFbTSFGcKS_OqbaQ](https://github.com/hoya012/CVPR-2021-Paper-Statistics?fbclid=IwAR0MGG3x-9bDU8YjVp-UGHcJDAXUspNwJ3Iy-o17oi7UFbTSFGcKS_OqbaQ)
4. 52CV/CVPR-2021-Papers: [https://github.com/52CV/CVPR-2021-Papers](https://github.com/52CV/CVPR-2021-Papers)
5. amusi/CVPR2021-Papers-with-Code: [https://github.com/amusi/CVPR2021-Papers-with-Code](https://github.com/amusi/CVPR2021-Papers-with-Code)



<br>

