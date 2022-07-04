---
layout: post 
title: Review - An EEG/EMG/EOG-Based Multimodal Human-Machine Interface to Real-Time Control of a Soft Robot Hand
subtitle: EEG/EMG/EOG-Based Multimodal Human-Machine Interface
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Brain Computer Interface]
comments: true
use_math: true
excerpt_separator: <!--break-->
---
<div align=center><h2> An EEG/EMG/EOG-Based Multimodal Human-Machine Interface to Real-Time Control of a Soft Robot Hand 논문 리뷰</h2></div>
<!--break-->

----


 <br/>

## INTRODUCTION
뇌졸중 환자들은 재활치료를 위해 물리치료사를 필요로 하거나 입원을 해야하지만 이는 비효율적이고 노동적이라는 단점이 있습니다.

따라서 본 논문에선 BCI를 이용해 재활치료를 효과적으로 시행할 수 있는 EEG/EMG/EOG 기반의 3-mode soft hand robot을 제안합니다.

- EEG : 좌/우를 상상하는 것을 식별
- EMG : 손동작 식별
- EOG : 왼/오른쪽을 보는 움직임을 식별 + 두번 깜박임으로써 mode 전환

## Experiment Apparatus and Setup
### A. Apparatus
- 팔 근육의 움직임을 감지하기 위한 Myo Armband
- EEG/EOG를 기록하기 위한 Neuroscan NuAmps Express system

### B. Setup
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/177071885-05c50d3d-255f-4872-9d67-49a960f470e1.png" width="100%"/>
</div>
> A) 실험 환경<br/>
B) glove의 구조

EEG/EOG는 500Hz로 EMG의 경우 200Hz로 샘플링되었으며, 위 3가지 신호는 nortch filter로 50Hz의 성분을 제거했습니다.

## Experimental Procedure
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/177072169-222da5ef-d992-47f7-9508-ada818596fbd.png" width="100%"/>
</div>
> A) Training / Testing Procedure<br/>
B) Online(Testing) Cycle

Training시 EOG의 좌/우 응시, EEG의 좌/우 상상 데이터를 수집하고 각각 Threshold 기반 분류, SVM 을 통한 분류가 진행되며,<br/>
EMG의 경우 MYO API를 이용해 손동작을 5가지로 분류합니다.

Online의 경우 3가지 EEG/EMG/EOG의 mode가 있습니다.
EOG mode가 defualt로 설정되어있으며, 눈을 두번 깜박일때마다 mode가 전환됩니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/177080982-0f47e6af-93bd-452b-8226-1fb2e99c0ef3.png" width="80%"/>
</div>

위 table처럼 실험자가 각 mode에서 task를 실시하면 glove는 hand action을 취합니다.

## Detection of Movement Intention Using mHMI
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/177081270-1ecdd31d-61b8-4afe-a8a2-5beebd8263af.png" width="100%"/>
</div>
위 figure는 EEG/EMG/EOG의 mode가 어떻게 Training되고 Testing(real-time에 적용)되는지를 보여줍니다.

### A. EOG mode
먼저 EOG 신호는 zero-phase FIR lowpass filte with a lower cutoff frequency 0.05Hz 와 higher cutoff frequency 45Hz 로 filtering됩니다.

그 후 Daubechies Wavelet를 이용해 wavelet transform을 진행합니다.

EOG mode는 왼쪽 눈, 오른쪽 눈, 두 번 깜박임 등의 눈 움직임을 식별하기 위해 이중 Threshold 방법으로 설정됩니다.

### B. EEG mode
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/177085587-38bd5ea6-41e4-4038-bd3b-f6da91538cb3.png" width="100%"/>
</div>
> 위와 같이 rest time 2초를 기준으로 ERS, ERS로 평가되어집니다.

EEG신호는 zero-phase FIR lowpass filter with cut off frequency 0.05~45Hz로 filtering됩니다.

다음으로, EEG의 ERD/ERS는 휴식 상태의 처음 2초와 비교하여 power 감소(ERD) 또는 power 증가(ERS)로 평가되어집니다.

좌/우 MI에 의해 별도로 얻은 신호의 평균값을 계산하고 각 채널의 전력 변화는 평균값에 대해 db4 mother wavelet을 사용하여 5단계의 Daubechies wavelet을 통해 추출됩니다.

왼쪽(C3)과 오른쪽(C4) 피질 위에 위치한 중앙 영역에 각각 기록된 EEG의 평균값의 차이를 구하고, ERD와 ERS는 difference normalization를 사용하여 계산됩니다.

이후 Support Vector Machines (SVM)으로 분류됩니다.

### C. EMG
EMG의 경우 MYO를 팔에 착용하고 5가지 gesture를 취한 후 MYO API가 팔 근육의 신호를 분석하여 gesture를 분류합니다.


## On-Line Control Soft Robot Hand With the mHMI
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/177081290-131d98d6-0892-415a-8ee5-aca9a7daafe7.png" width="100%"/>
</div>
Online에서도 마찬가지로 위와 유사한 절차로 진행되며 데이터 전송 방법 detail등을 살펴볼 수 있습니다.

## RESULTS AND DISCUSSION
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/177086070-d7960e8b-81b5-4e89-8340-df815a3d0bab.png" width="80%"/>
</div>
> EEG mode (A,B) : fist, three finger grip<br/>
EOG mode (C,D) : push with bend five fingers, pull with pinch five fingers<br/>
EMG mode (E,F,G,H,I) : finger loosen up, ball pinch, tip pinch, multiple-tip pinch, cylindroids grip

입력값에 따라 air pump가 작동되어 soft hand robot이 환자의 gesture를 도와주는 figure입니다. 

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/177085961-cadf53ba-4eea-492d-bea5-ed889b5ce218.png" width="80%"/>
</div>
> EOG, EEG, EMG multimodal performance

## CONCLUSION
본 논문에선 MI, eye movements, hand gestures를 통해 robust하게 real-time에 적용할 수 있는 soft hand robot을 제안했습니다.

특히 EEG, EOG, EMG mode는 soft hand robot의 control commands를 증가시킬 수 있었습니다.
또한 double blinks을 이용한 mode 전환과 다양한 손동작을 쉽고 직관적이게 구현했습니다.

결과적으로 93.83%의 정확도와 47.41 bits/min의 ITR을 달성했으며, 제안된 soft hand robot은 불편한 손동작을 수행하는 데 도움을 제공합니다.

---

## 고찰
본 논문처럼 다양한 생체신호를 mode개념으로 변환하면서 mode마다 특정 생체신호만 분류할 수 있는 task를 적용해 연구할 수 있을 것이다.

VR과 접목하여, VR에서 실제로 움직이지 않고 BCI로 대체할 수 있는 움직임 관련 (eg. 걷기, 버튼 누르기, 물체 잡기/밀기/끌기 etc) 영역을 구체화하고 기존에 읽었던 논문들의 synchronous/asynchronous 개념을 다양하게 적용해 연구해볼 수 있을 것 같다.

###### Reference
* DOI: 10.3389/fnbot.2019.00007