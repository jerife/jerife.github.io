---
layout: post 
title: Review - An autonomous hybrid brain-computer interface system combined with eye-tracking in virtual environment
subtitle: BCI combined with eye-tracking in virtual environment
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Brain Computer Interface]
comments: true
use_math: true
excerpt_separator: <!--break-->
---
<div align=center><h2> An autonomous hybrid brain-computer interface system combined with eye-tracking in virtual environment 논문 리뷰</h2></div>
<!--break-->

----


 <br/>

## Introduction
전통적인 BCI 시스템은 실제 적용에 몇 가지 결함이 있으며 유연성, 견고성 및 성능의 개선이 필요합니다.

최근엔 여러 생체신호를 결합하는 연구가 좋은 성능을 보여주어 주목을 끌고 있습니다.<br/>
특히 이전의 eye tracking과 EEG가 결합된 hybrid BCI systems은 EEG data와 eye tracking data가 synchronous하게 수집되었습니다.

위처럼 synchronous한 시스템은 높은 ITR과 간단한 디자인을 갖지만, 시스템이 지속적으로 지시를 내려야 하고 사용자의 조작 시간이 엄격하게 제한됩니다.

따라서 본 연구는 조작 제어, 이동 등과 같은 가상 환경에서의 제어 작업을 위해 eye tracking과 SSVEP가 결합된 multi-modality asynchronous hybrid BCI system을 개발하는 것을 목표로 합니다.

1. sliding window 방식을 활용하여 시선 데이터의 변화(분산)를 분석하고 대상을 인식합니다.
(user는 자율적으로 작업을 수행하기 위해 시선을 조정할 수 있습니다.)
2. EEG와 시선 데이터를 결합하기 위해 particle swarm optimization (PSO) fusion 방법이 제안됩니다.

## Methods
### A. Autonomous hybrid brain-computer interface control system
데이터는 아래의 기기로 수집됩니다.
- EEG data 수집 기기 : wireless 8-channel Neuracle amplifier
- Eye gaze 수집 기기 : eye-tracking module: aGlass DKII

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/173763050-2fd67594-8d18-417a-a727-71ecbf814fbd.png" width="100%"/>
</div>
수집된 데이터는 preprocessing 과정을 거친 후, features extraction 되어 분류됩니다.<br/>
마지막으로 분류 결과는 VR기기를 통해 GUI로 피드백됩니다.

### B. Autonomous eye-gaze control and EEG/eye-gaze fusion
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/173763458-fcf2247d-5ef7-4075-b088-a8f8d55691e6.png" width="100%"/>
</div>
> procedure of the autonomous control and dual-modality fusion of EEG and eye-gaze

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/174251216-b4ec6510-790a-4a9b-8dab-00c3b1c868ad.png" width="60%"/>
</div>
먼저 실험이 시작되면 참가자는 "1" box를 2.5초간 응시합니다. 이 데이터는 eye-gaze의 변화의 threshold를 계산하기 위해 사용됩니다.

참가자의 eye-gaze features를 분석하기위해 sliding window가 사용되었습니다.
> eye-gaze features는 양쪽 눈에 해당하는 X,Y값의 평균과 분산을 포함함.

eye-gaze의 변화(분산)가 threshold보다 작고 참가자의 eye-gaze가 자극지역에 있을 경우 BCI system은 working state가 됩니다.<br/>
target recognition이 시작되고 시스템은 짧은 소리 신호와 함께 데이터가 수집, 분석되며 2.5초 후에 마찬가지로 짧은 소리 신호와 함께 수집이 끝났다고 인지시켜줍니다.

만약 자극 위치로부터 참가자의 eye-gaze point가 멀리있거나 eye-gaze가 수집되지 않거나 eye-gaze의 variance가 threshold보다 크다면 BCI system는 idle state가 됩니다.<br/>
> idle state동안엔 BCI system는 분석하지 않고 결과를 내지 않습니다. 

### C. Data acquisition
wireless EEG cap으로 PO5, PO3, POZ, PO4, PO6, O1, OZ, O2와 기준이될 PZ 신호가 1000Hz로 수집되었습니다.

eye-gaze data는 aGlass SDK를 통해 2차원 position으로 수집되었으며, 120Hz로 샘플링되었습니다.

### D. SSVEP paradigm
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/174258725-8017c0b3-6d91-4490-be6d-c174bf3a9f46.png" width="60%"/>
</div>
자극 interface는 10개의 flicker box로 구성되었으며, 0.5 Hz 차이를 기준으로 각각 8~12.5 Hz로 깜박입니다.

### E. Experimental procedures
실험은 5 session을 포함하며 각 session은 calibration block과 7 test block을 포함합니다.

#### Calibration block
calibration block에선 자극이 flicker되지 않습니다.
참가자는 1~10의 번호 box를 순서대로 응시하고 

eye-gaze data는 classification의 parameters를 위해 계산되며 classification의 parameters는 7 test blocks에서 사용됩니다.

각 번호 box에 대해 sliding window 방식으로 계산된 eye-gaze 데이터의 변화(분산)가 임계값보다 작을 때 대상이 안정적으로 숫자를 응시하고 있음을 나타냅니다.

#### Test block
Test block에선 1~10 box를 (순서대로 or 역순으로) 식별하는 실험(7 block)이 진행됩니다. 각 block은 10번의 trial을 포함합니다.

### F. Data preprocessing
EEG epochs은 pre-trigger 1s ~ post-trigger 2.5s로 추출되었습니다. <br/>
[6 ~ 40 Hz], [14 ~ 40 Hz], [22 ~ 40 Hz]의 multiple band-pass filters가 적용되었습니다.<br/>
눈을 깜박이거나 감고 있어서 발생한 zero / null 값은 제거되었습니다.

### G. Feature extraction of EEG data and eye-gaze data
#### EEG features
1) EEG featuress는 filter bank canonical correlation
analysis (FBCCA)를 통해 추출되었습니다. FBCCA는 SSVEPs의 주파수 탐지를 향상시킵니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/174264676-9953afdb-1c80-4152-929b-a3281d65a22a.png" width="60%"/>
</div>
> $X_{SBn}, n = 1, 2,…, N$ : sub-band components 

2) 각 sub-band component와 모든 자극 주파수에 해당하는 reference signal ($Y_{fk} , k = 1, 2,…, 10$, flicker box가 10개) 의 correlation value는 standard CCA에 의해 계산되었습니다. correlation vector $ρ_{k}$는 위와 같이 정의 됩니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/174270211-ddf21677-e358-4e08-9baf-2a837b403a6d.png" width="25%"/>
</div>
3) 마지막 EEG feature는 각 trial에 해당하는 correlation vector **$ρ_{j}$** = [$ρ_{j}^{1}, ρ_{j}^{2}, ..., ρ_{j}^{10}$]
입니다.(j번째 trial의 correlation vector)<br/>
여기서 $ρ_{j}^{k}$는 sub-band components에 일치하는 weighted sum of squares of correlation 값을 의미합니다.

#### Eye-gaze features
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/174270997-5abdc98c-1a66-4e31-9e14-5ade1db7c587.png" width="65%"/>
</div>
> $x_{j}$: j번째 eye-gaze trial<br/>
> $C_{i}$: i번째 category의 flicker box<br/>
> $a_{i}$: i번째 category의 prior probability<br/>
> $μ_{i}, \Sigma_{n=1}$: i번째 category의 mean vector와 가우시안 분포의 covariance matrix

Eye-gaze feature는 Bayes기법에 의해 얻어졌습니다.
Eye-gaze data가 독립 다변량 정규 분포를 따른다고 가정했습니다. 따라서 단순화된 Gaussian mixture model(GMM)를 사용할 수 있습니다.


### H. Feature fusion and classification
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/174273120-6fd32768-6746-4315-8943-363e56d3842c.png" width="50%"/>
</div>
> **F**: 10개의 category의 예측확률을 포함하는 fusion vector<br/>
> $ρ_{j}, p_{j}$: EEG features, Eye-gaze features<br/>
> $\beta_{1}, \beta_{2}$: EEG features와 eye-gaze features의 fusion weights

EEG and eye-gaze를 융합하는 기법으로 particle swarm optimization (PSO)가 사용됐습니다.

PSO를 통해 EEG features의 correlation vector와 Eye-gaze features의 posterior probability가 분석되었습니다.

$\beta_{1}, \beta_{2}$는 the PSO method에 의해 정의됩니다.

## Results
### A. Classification performance of single-modality methods and fusion methods
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/174279895-31f77e3f-ad52-4bf9-850b-69f755548172.png" width="100%"/>
</div>

### B. Performance of fusion methods for different data lengths
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/174280404-75472ab0-fc00-42b7-a268-5362ad052925.png" width="100%"/>
</div>

### C. Effects of different data lengths on single-modality methods and PSO fusion method
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/174280203-630d863f-e63c-4bc4-9a3e-67bbcc0f1198.png" width="80%"/>
</div>

## Discussion & Conclusions
본 연구에선 hybrid BCI system combined with eye
tracking 이 제안되었으며 기존의 asynchronous system보다 더 나은 accuracy, ITR, short actual time (for single-trial)을 보였습니다.

기존 몇몇의 asynchronous system은 눈을 뜨고있는 시간으로 trigger를 판단하지만 이는 잘못 trigger될 수도 있지만,<br/>
본 연구에선 eye-gaze의 변화(분산)를 분석하는 sliding window로 trigger를 판단합니다.

추가적으로 몇몇의 asynchronous system은 eye tracking으로 target을 선택하고 EEG data로 classification합니다.<br/>
하지만 이런 시스템은 multi-modality를 잘 사용하지 못했습니다.

따라서 본 논문에선 EEG data, eye-gaze data를 융합한 optimal PSO fusion 기법이 제안되었습니다.

결론적으로 본 논문에서 제시한 hybrid BCI system는 single-modality뿐 아니라 다른 fusion methods보다도 좋은 performance를 보였습니다.

---

## 고찰
eye-gaze를 VR locomotion에 적용해본다면, EEG data뿐아니라 eye-gaze data를 이용한 multi-modality를 사용해 VR에서의 이동을 classification할 수 있을 것입니다.

또한 VR에서 이동할때 몸통을 얼굴의 정면방향으로 움직이는(도는) 방법을 multi-modality를 적용한 MI task로도 시도해볼 수 있습니다.

뿐만 아니라 사람들이 몸통을 돌릴때의 눈의 변화를 관측하여 그에 맞는 task를 적용해 classification할 수 있을 것 같습니다.
> 본 논문에선 sliding window를 이용해 눈의 움직임의 변화와 threshold로 trigger를 판단했음

###### Reference
* DOI: 10.1016/j.jneumeth.2021.109442