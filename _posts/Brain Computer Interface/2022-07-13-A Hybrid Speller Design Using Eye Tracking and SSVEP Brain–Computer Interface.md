---
layout: post 
title: Review - A Hybrid Speller Design Using Eye Tracking and SSVEP Brain–Computer Interface
subtitle: Hybrid Speller Design Using Eye Tracking and SSVEP
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Brain Computer Interface]
comments: true
use_math: true
excerpt_separator: <!--break-->
---
<div align=center><h2> A Hybrid Speller Design Using Eye Tracking and SSVEP Brain–Computer Interface 논문 리뷰</h2></div>
<!--break-->

----


 <br/>

## Introduction
기존의 BCI Speller는 낮은 ITR문제를 갖기에 최근 다양하게 mutilmodality하게 적용되고 있습니다.<br/>
본 논문에선 BCI의 SSVEP와 Eye movement를 통해 높은 정확도와 ITR 그리고 User에게 낮은 피로감을 주는 Speller를 제안합니다.

SSVEP는 robustness하고 많은 commands를 다룰수 있으며 높은 정확도와 ITR을 갖는다는 장점이 있지만,
너무 많은 commands로 flickering stimuli이 다양해지면 종종 피로감과 불편함을 초래합니다.
> low frequency of flickering는 눈에 피로감을 준다고 알려져있습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/178418653-e9110684-617e-4aad-8abd-e81d8b69f81c.png" width="90%"/>
</div>
> 48-target BCI speller

따라서 논문에선 Eye movement로 Speller의 영역을 감지하고, SSVEP를 통해 영역 속의 6개의 frequency target만을 응시하여 character를 선택하는 기법을 제시합니다.

## Methods
### A) Experimental Procedure
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/178421934-135fd6c3-b5d8-446d-b760-f93eeeae162e.png" width="70%"/>
</div>

위 그림처럼 speller는 6x8 stimulation matrix의 sub-matrix(target box)로 구성되어있으며, 각 sub-matrix는 3x2=6개의 target으로 구성되어있습니다.

특히 각 sub-matrix의 target들은 144Hz 모니터에 나눠지는 (144/11 = 13.0909, 144/10 = 14.40, 144/9 = 16.00, 144/8 = 18.00, 144/7 = 20.5714 and 144/6 = 24.00 Hz) 6개의 주파수들로 고정됩니다.

Eye tracker 는 sub-matrix(target box)를 식별하며 동시에 EEG data는 target frequency를 식별합니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/178421971-7acd96a8-f788-4330-a58d-78ece9e50ccd.png" width="70%"/>
</div>

#### Offine Experiment
Offine 실험은 3block을 포함하며 각 block은 48개의 target을 랜덤한 순서로 진행됩니다.
1. 먼저 0.5초간 48개의 target들 중 1개의 target이 빨간색을 띄우면 해당 target으로 시선을 움직임입니다.
2. 이후 참가자는 5초간 해당 (flickering하는) target을 계속 응시합니다.
3. 마지막 0.5초간 screen이 깜박입니다.

#### Online Experiment
Online실험은 1.5초로 진행되며 시선을 움직이는 시간 0.5초, target들이 flickering하는 시간 1초로 구성되어있습니다.

실험은 training/testing 실험으로 구성되어있으며 training 단계는 3block으로 진행됩니다.<br/>
testing 단계는 cued-spelling task, free-spelling task 두 가지로 나눠집니다.

cued-spelling task는 target이 빨간색을 띄우는 cue 사인이 있지만 free-spelling task의 경우 자유롭게 문장을 쓰는 task입니다.

#### Control Conditions &  Questionnaire
위의 제안된 실험을 완료한 경우, 3일 후 기존 Basic Speller와 기존 Hybrid EEG-Eye Tacking (본 논문에서 제안한 것 아님) 을 추가 실험 하여 제안된 Speller와 비교하고 설문조사가 실시됩니다.<br/>
또한 추가로 하는 두 실험의 각 target들은 0.2 Hz간격으로 7~16.4 Hz로 flicker합니다.

### B) Method
본 연구에선 Eye Tracking data와 CCA Algorithm를 사용해 target sub-matrix와 frequency of SSVEPs를 식별합니다.

offline과 online 실험에서 data epoch은 각각 0.14~5.14s, 0.14~1.14s로 추출되었습니다.
EEG data의 경우 notch filter of 50 Hz가 적용되었고 300 Hz로 down-sampling되었으며 band-pass-filtered from 12 to 110 Hz이 적용되었습니다.

#### Sub-Matrix Detection
Eye tracker data는 pixel단위로 저장되며, gaze-direction data의 평균은 target을 분류하는 feature로 계산되었습니다.

#### SSVEP Detection
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/178440529-0b6768d6-3241-4f54-a2dd-fad7240da73e.png" width="85%"/>
</div>
CCA는 두 dataset 사이의 유사성을 추출해내는 기법이며, 두 다차원 변수를 고려해 x와 y의 correlation을 최대로하는 weight vectors를 찾습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/178442878-1ffe5307-de76-4f4d-aee9-a0e0d08a7907.png" width="50%"/>
</div>
SSVEPs의 주파수를 식별하기 위해 CCA는 EEG signals X와 reference signals(each stimulus frequency) Yk 사이의 canonical correlation P을 계산합니다.

마지막으로 두 algorithm에 의해 target sub-matrix와 target frequency가 식별되면 해당하는 character가 output으로 선택됩니다.

## Results
### A) Offline Data Analysis
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/178628657-5b97eeb0-d088-4afd-955b-c4134b357222.png" width="100%"/>
</div>
> 각 Experiment의 설문 결과

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/178628610-a7b7b32e-40dc-4283-9f33-2df87b4a33dd.png" width="100%"/>
</div>
> 각 Experiment의 Accuracy, ITR, Correct 결과

### B) Online Data Analysis
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/178629189-14eeb551-4285-4eab-a56c-ff2f4b269f07.png" width="100%"/>
</div>
> Online data 중 cued-spelling task 결과

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/178629240-d4d04cb2-86ca-4fbf-9be1-6ed980d0a332.png"
    width="100%"/>
</div>
> Online data 중 free-spelling task 결과

## Discussion
Eye tracker를 이용해 Target box를 인지함으로써 48개의 frequency box를 모두 분류하지 않고 Target box 속에 해당하는 오직 6개만의 frequency box만을 분류하면 되기 때문에

모든 target을 다른 frequency로 사용하는 Speller보다도 좋은 정확도와 눈의 피로감을 줄이고 편안한 결과를 제공했습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/178629428-5d090522-de8f-4b8f-a827-5ee5bd6e3686.png"
    width="80%"/>
</div>
> SSVEP와 eye tracking 예측값의 Confusion matrix

실제로 SSVEP task의 경우 좋은 performance를 보이며,
Eye tracking의 경우 all correct한 결과를 볼 수 있습니다.

## Conclusions
결론적으로 본 논문은 Eye tracking과 13~24Hz의 자극을 갖는 SSVEP BCI를 결합한 새로운 Speller를 디자인하여 User의 불편함을 줄이고 정확도와 ITR을 높였습니다.

제안된 Speller의 핵심은 6개의 주파수만으로 48개의 target을 분류했고 기존의 basic, hybrid speller들과 비교해 좋은 performance를 보인다는 점입니다.

---

## 고찰
VR의 특정 버튼을 누르는 task와 접목한다면, 먼저 Eye tracking data로 버튼 주위를 인지하고 있음을 확인한 후 세부 option들을 SSVEP task를 활용해 접목할 수 있을 것 같다.

이외에도 VR속에서의 움직임(eg. 걷기, 손동작) 또한 Eye tracking data로 특정 area를 먼저 인식한 후 MI task를 적용하는 시도 또한 해볼 수 있을 것이다.


###### Reference
* DOI: 10.3390/s20030891