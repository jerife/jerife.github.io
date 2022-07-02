---
layout: post 
title: Review - Motion Imagery-BCI Based on EEG and Eye Movement Data Fusion
subtitle: MI-BCI Based on EEG and Eye Movement Data Fusion
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Brain Computer Interface]
comments: true
use_math: true
excerpt_separator: <!--break-->
---
<div align=center><h2> Motion Imagery-BCI Based on EEG and Eye Movement Data Fusion 논문 리뷰</h2></div>
<!--break-->

----


 <br/>

## INTRODUCTION
EEG data만 사용하는 모델의 경우 low SNR로 인한 성능이 좋지 않다는 단점이 있습니다.<br/>
하지만 좋은 EEG data를 얻기위해 좋은 EEG device를 필요로 하고 이는 효울성, 휴대성, 보편성이 좋지 않습니다.

따라서 본 논문은 user가 접할 수 있는 device를 통해 보편성과 효울성을 보장하며, eye-tracking을 접목한 기법을 제시합니다.

eye-tracking data는 동공 크기를 통해 다른 패턴을 구분하기 좋은 '집중 정도'와 '식별 정보'와 같은 특징들을 제공하며, 이는 MI를 더 쉽고 빠르게 감지하거나 정확도를 향상시킬 수 있습니다.

## EEG AND EYE MOVEMENT EXPERIMENTS
### A. Apparatus
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/176990089-af4184d3-62ec-4826-9f42-848550eee485.png" width="50%"/>
</div>
14 electrodes, 128Hz sampling 가능한 Emotiv라는 EEG device와 head-mounted eye tracker를 통해 eye movement를 기록했습니다.

eye tracker의 gaze fixation을 계산하기위해 pupilcenter-corneal-reflection algorithm(PCCR) 이 사용됐습니다.

### B. Procedure
실험 전, 참가자의 gaze fixation을 계산하기 위해 9-point calibration을 실시했습니다.
> ERD는 contralateral 피질에 발생할 수 있고 hand MI 특성상 $\mu, \beta$ rhythm에서 activity를 보일 수 있습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/176990079-76cac780-03dd-4dcc-9ae8-2ea27f6ad1d0.png" width="80%"/>
</div>
실험은 위와 같은 절차로 진행됩니다. 총 10 seesion의 실험이 진행되며, 각 session은 20 trial을 포함합니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/176990127-90777ff3-f543-4915-aee1-6d19cd22ca58.png" width="70%"/>
</div>
MI cue가 보이는 시간엔 EEG, eye movement data가 동시에 수집됩니다.

## EEG AND EYE MOVEMENT FUSION METHOD
### A. Data Preprocessing and Feature Extraction
#### 1) EEG Data Preprocessing and Feature Extraction
EEG data는 F3, F4, FC5, FC6에서 추출되었습니다. 

noise를 줄이고 SNR을 향상시키기 위해 high-pass filter with a cutoff frequency of 0.5 Hz를 적용했습니다.

EMG, EOG와 같은 artifact를 제거하기 위해 independent component analysis (ICA)와 같은 blind source separation technique을 적용했습니다.

MI data는 0.5 ~ 2.5s에서 유의마한 signal이 추출되어 해당하는 2초의 time window를 이용해 분석되었으며, wavelet transform (WT)를 기반으로하는 Multi-resolution analysis (MRA)로 특징을 추출했습니다.

그후 데이터는 $\alpha$ (8-13Hz), $\beta$ (13-30Hz), $\gamma$ (30-80Hz) wave와 논문에서 제안하는 wave(8-40Hz)로 분해됐습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/176991893-75eb70a6-b6a3-40e6-ac9c-ee102adaea2f.png" width="30%"/>
</div>
최종적으로 signal energy는 이후 classifier의 입력 벡터로 사용됩니다.

#### 2) Eye Movement Data Preprocessing and Feature Extraction
eye movement data를 계산하기위해 pupil center cornea reflection (PCCR)가 적용됐습니다.

1) 카메라로 user’s eye image를 캡쳐하고 이를 gray-scale image로 변환하고 3 × 3 Gaussian kernel function로 noise를 필터링합니다.

2) 데이터를 binarized 하고 image features를 추출합니다.주요 feature는 PCCR 벡터를 설정할 수 있는 동공 center와 Purkinje’s image의 center였습니다.

3) 마지막으로 표시된 고정 좌표를 계산하기 위해 매핑 모델이 사용되었습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/176993075-7f745e4e-787d-4c50-9f48-1acff35ec934.png" width="70%"/>
</div>
> Fixation count (C)<br/>
Fixation duration (D)<br/>
Fixation coordinates (F)<br/>
Pupil diameter (P)<br/>
Saccade length (S)

동공 반응이 눈 움직임 기능의 한 종류로서 인지 처리와 행동의 작동 강도를 설명하는 중요한 지표로 사용될 수 있다는 것을 이전 논문 결과를 바탕으로,

본 논문에선 MI 분류를 위해 동공 직경, 고정 좌표, 눈 움직임의 길이, 고정 기간, 고정 횟수의 5가지 안구 운동 기능을 eye movement features로 선택했습니다.

가장 대비적인 특징을 보여주는 위의 5가지의 eye movement features들 중 좌/우 분류에 도움을 주는 feature들을 찾기위해 SVM으로 실험한 결과 위 도표와 같은 결과가 나왔으며,

본 논문에선 fixation coordinates (F), pupil diameter (P), saccade length (S)를 eye movement features로 선정했습니다.

또한 각 feature들을 조합해 여러 실험을 해본 결과, pupil diameter가 다른 eye movement features보다 MI와 관련된 유용한 정보를 포함할 수 있음을 나타냅니다.

밑에선 Fusion on Feature Layer vs Fusion on Decision Layer의 실험 결과에 대해 다룹니다.

### B. Fusion on Feature Layer
Fusion on Feature Layer의 경우 heterogeneous feature fusion 기법을 이용했습니다.

Feature Layer에서 fusion할 경우 data의 정규화가 필요하기에 min-max 기법을 채택했습니다.

이후 데이터들은 convolution layers, three max-pooling layers, a fully-connected layer를 포함하는 7개 layer의 CNN model을 통해 분류되어집니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/176994322-ea45cb81-36a4-401b-8102-676196bbe8d4.png" width="70%"/>
</div>
결과는 두 데이터를 fusion했을 때 더 좋은 성능을 보임을 알 수 있었습니다.

### C. Fusion on Decision Layer
Fusion on Decision Layer의 경우 D-S evidence-based fusion기법을 이용했습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/176994378-669065e1-fff2-4787-864b-79f464139b2d.png" width="70%"/>
</div>

Fusion on Feature Layer의 경우보다 Fusion on Decision Layer가 더 좋은 성능을 보임을 확인할 수 있었습니다.

## DISCUSSION & CONCLUSION
본 논문은 fixation coordinates, saccade length, pupil diameter과 같은 eye movement features와 EEG features를 조합했습니다.<br/>
또한 두 feature를 feature layer보다 decision layer에서 fusion하는 것이 다른 실험들과 비교해봐도 더 좋은 성능을 보였습니다.

하지만 user의 보편성과 휴대성에 중점을 둔 해당 실험의 특성상 eye tracker의 불편함과 같은 한계가 있었음을 언급하고 이후 더 많은 class를 classification 해야한다고 언급하며 논문이 마무리됩니다.

---

## 고찰
EEG와 eye movement를 synchronous하게 적용해 Fixation count, Fixation duration, Fixation coordinates, Pupil diameter, Saccade length와 같은 eye movement의 특징을 fusion하여 multimodality하게 모든 BCI분야에 접목시킬 수 있을 것 같다.


###### Reference
* DOI: 10.1109/TNSRE.2020.3048422