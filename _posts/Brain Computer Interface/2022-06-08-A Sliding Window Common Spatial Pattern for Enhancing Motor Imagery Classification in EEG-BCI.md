---
layout: post 
title: Review - A Sliding Window Common Spatial Pattern for Enhancing Motor Imagery Classification in EEG-BCI
subtitle: Sliding Window Common Spatial Pattern
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Brain Computer Interface]
comments: true
use_math: true
excerpt_separator: <!--break-->
---
<div align=center><h2> Sliding Window Common Spatial Pattern for Enhancing Motor Imagery Classification in EEG-BCI 논문 리뷰</h2></div>
<!--break-->

----


 <br/>

## ABSTRACT
본 연구는 two sliding window 기술을 사용해 motor imagery (MI)의 binary classification task를 향상시킵니다.

1. 첫 번째 방법은 모든 sliding window의 예측 시퀀스의 가장 긴 연속 반복(longest consecutive repetition, LCR)을 계산하며 SW-LCR이라고 합니다.
2. 두 번째 방법은 모든 sliding window의 예측 시퀀스 모드(Mode)를 계산하며 SW-Mode라고 명명됩니다.

Common spatial pattern (CSP)는 각 시간마다의 window를 linear
discriminant analysis (LDA)로 classification하기 위한 특징을 추출할때 사용됩니다.

SW-LCR는 healthy individuals dataset에 좋은 성능을 보였으며 SW-Mode는 stroke patients’ dataset의 MI task에서 낮은편차를 보여주는 성능을 보여줬습니다.

결과적으로 SW-LCR 및 SW-Mode를 사용한 MI의 sliding window 기반 예측이 intertrial 및 intersession의 불일치에 대해 robust하므로 신경 재활 BCI 설정에서 안정적인 성능으로 이어질 수 있음을 보여줍니다.

## INTRODUCTION
BCI system의 큰 issue는 EEG가 non-stationarity하다는 점입니다.<br/>
이 문제를 해결하기 위해 data preprocessing 단계에서 EEG신호와 동시에 발생되는 low signal-to-noise (SNR)에 기여하는 EMG, EOG및 artifact를 제거해야한다고 하며, 논문에선 이후 EEG관련 모델과 task들에 대해 소개해줍니다.

하지만 흔하게 볼 수있는 EEG task model(논문에서 간단히 소개한 model)들의 중요한 단점은 EEG data의 앞뒤가 맞지않은 non-stationarity한 특징 때문에 intersession마다 classificaiton accuracy(CA)가 감소한다는 점입니다.

이 문제를 해결하기 위한 일반적인 접근 방식은 가장 일반화 가능한 feature를 찾는 것입니다. 그러나 주의할 점은 trial 기간 내의 시점 선택은 일반적으로 휴리스틱 방식으로 선택된다는 것입니다.

따라서 이러한 모순을 최소화하기 위해서 본 논문은 2가지 새로운 sliding window-based CSP 기술을 소개합니다.
- SW-LCR
- SW-Mode

## DATASET
### A. Healthy Individuals’ Data Set
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/172506865-2889fbf9-a781-48d0-adfe-e93c6451adf7.png" width="90%"/>
</div>
> Healthy Individuals’ Data Set Experiment Paradigm

본 dataset은 건강힌 개인의 cue-based BCI MI tasks의 결과를 포함하고있습니다.

- left hand (class 1)
- right hand (class 2)
- both feet (class 3)
- tongue (class 4)

mastoid(귀 밑 뒤통수쪽) 부근에 22개의 전극을 사용했으며, 각 전극은 3.5cm의 거리를 유지합니다.<br/>
50Hz norch filter와 0.5 ~ 100 Hz BPF를 적용하고 250Hz로 데이터가 샘플링됐습니다.

본 연구에선 4개의 classes를 사용해 6가지 조합을 classification합니다.

### B. Stroke Patients’ Data Set 
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/172531080-4c142268-092e-4b43-b980-804a415b61da.png" width="90%"/>
</div>
> Stroke Patients’ Data Set Experiment Paradigm

본 dataset은 뇌졸증 환자의 cue-based BCI MI tasks의 결과를 포함하고있습니다.

- left hand (class 1)
- right hand (class 2)

50Hz norch filter와 0.1 ~ 100 Hz BPF를 적용하고 500Hz로 데이터가 샘플링됐습니다.

## CSP
CSP는 class의 분산을 최소화하는 동시에 다른 class의 분산을 최대화하는 spatial filter를 학습하는 것을 목표로 합니다.

CSP는 최적하게 구별되는 MI-based BCI기반의 band-power feature를 얻습니다.
> 주어진 주파수 대역의 band-power는 선택한 대역에서 필터링된 EEG 신호의 분산을 제공합니다.

## LDA CLASSIFIER
LDA-based classifier는 차원을 줄여주는 동시에 클래스의 차별적인 정보를 보호합니다.

본 연구에서 LDA classifier는 가장 차별적인 것을 제공하는 feature들의 최적의 조합을 찾습니다. 

## METHODOLOGY
### A. Sliding Window
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/172533267-93362ffd-9272-4b34-8d40-fb49f50a2a47.png" width="80%"/>
</div>
> Dataset A의 경우 2초부터 Cue가 시작됨. 따라서 2초부터 window가 적용됨

본 논문에선 2초 짜리 window가 총 9개 사용되며, 각 window는 0.1초를 텀으로 사용됩니다.

#### Exsample 

{: .box-note}
first window (T1): 2.0~4.0 s <br/>
second window (T2): 2.1~4.1 s<br/>
ninth window (T9): 2.8~4.8 s<br/>

multichannel EEG signal는 BPF를 적용하는 것이 좋다는 타 논문을 바탕으로, EEG data는 8~30 Hz band의 BPF가 적용됩니다.

### B. SW-LCR
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/172533210-5d5fb907-4701-4868-a527-2386537a2d59.png" width="100%"/>
</div>
SW-LCR는 예측의 마지막 단계에서 사용됩니다.
LCR는 sequence 값에서 가장 긴 subsequence의 값을 의미합니다.

만약 subsequence의 길이가 같은 것이 2개 이상이라면 맨 처음의 subsequence를 사용합니다.

#### Exsample 1

{: .box-note}
LDA Classifier output(sequence) : "121 111 222” <br/>
LCR output : "1" (LDA output에서 "1111"라는 subsequence가 가장 길기 때문)

#### Exsample 2

{: .box-note}
LDA Classifier output(sequence) : "121 222 111” <br/>
LCR output : "2" (LDA output에서 "222"라는 subsequence가 더 먼저 발견됐기 때문)

### C. SW-Mode
가장 긴 subsequence의 값을 선택하는 LCR와는 달리 SW-Mode는 sequence 속에서 가장 많은 값은 선택하는 방법입니다.

#### Exsample 1

{: .box-note}
LDA Classifier output(sequence) : "111 112 222” <br/>
Mode output : "1" (LDA output에서 "1"개수가 5개이고 "2"의 개수는 4개이기 때문)

#### Exsample 2

{: .box-note}
LDA Classifier output(sequence) : “122 121 211” <br/>
Mode output : "1"

특히 Mode 방법은 task를 집중할수 없거나 오랫동안 집중하기 어려운 환자들에게 중요합니다.

위와 같이 "1"과 "2"가 섞이면서(집중을 못한 경우) 더 많은 값이 있는 것을 선택해 LCR일 경우 생기는 misclassification 문제를 줄일 수 있습니다.

### D. Disscusion
SW-LCR 및 SW-Mode 기반 전략의 주요 장점은 백그라운드에서 사용되는 feature extraction 또는 classification algorithm에 관계없이 single-trial prediction에 대한 CA를 개선할 수 있다는 것입니다.

이는 제안된 sliding window 기반 방법이 trial마다 그리고 session마다 다를 수 있는 단일 시점에 초점을 맞추지 않고 시행 내 다른 시간 segment에서 feature를 찾기 때문입니다.

하지만 단점으로는 realtime BCI에 sliding window를 적용할 경우 지연시간이 생긴다는 점이 있습니다.

## RESULTS AND DISCUSSION
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/172544645-10eb8169-8ccb-4583-811d-d28ba7352868.png" width="50%"/><img src="https://user-images.githubusercontent.com/68190553/172544730-f3efc2a6-1338-48f8-945e-ffce23e26238.png" width="50%"/>
</div>
> Healthy Individuals’ Dataset의 SW-LCR & SW-Mode 결과 보고

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/172544995-7dd7c5f4-59c0-403a-a293-764f9120ee30.png" width="70%"/>
</div>
> Stroke Patients’ Dataset의 다른 Method와 벤치마킹 비교

- M1, M3, M4: CSPs + variations
- M2 : multivariate EMD (MEMD)-based filtering + Riemannian geometry (RG)

## CONCLUSION
EEG data는 session, subject간에 서로 non-stationary합니다.

본 논문에선 위 문제를 해결할 수 있는 intertrial, intersession에 robust한 sliding window-based CSP 기술(SW-LCR & SW-Mode) 소개했습니다.

특히 SW-Mode의 경우 stroke patients’ dataset에서 SOTA모델보다도 좋은 결과를 보였으며, SW-LCR는 healthy individuals dataset에서 좋은 성능을 보였습니다.

결과적으로 healthy individuals, stroke patients dataset에서 accuracy 80%정도와 kappa 0.6이상을 기록했으며 표준편차도 감소했습니다.<br/>
이는 뇌졸중 환자의 EEG 데이터가 겪는 lower SNR와 higher non-stationarity를 해결할 수도 있기에 중요한 점입니다.

결론적으로 SW-LCR 및 SW-Mode는 MI-BCI의 성능을 향상시킬 수 있는 잠재력을 가지고 있으며 신뢰할 수 있는 신경 재활 BCI application을 위한 기반을 마련할 수 있습니다.

---

## 고찰
Motor Intention / Imagery의 경우 키보드와 같은 즉각적인 반응이 있는 input system처럼 Real-time에 적용하기가 쉽지 않다. 

따라서 본 논문의 Sliding Window기법을 좀더 slice하게 이용해 Real-time에 적용하는 것을 시도해볼 수 있을 것 같다.

더하여 본 논문에선 Deep-Learning 기반 EEGNet의 성능이 더 안 좋다했지만, 정교한 Deep-Learning 기술 접목시켜볼수도 있을것 같다.
> $\delta, \theta, \alpha, \beta$로 데이터 분해, RNN(LSTM), AutoEncoder, Residual block, Dense block, Inception block, attention etc

###### Reference
* DOI: 10.1109/TIM.2021.3051996