---
layout: post 
title: Review - Hybrid EEG-EOG-based BCI system for Vehicle Control
subtitle: EEG-EOG-based BCI system for Vehicle Control
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Brain Computer Interface]
comments: true
use_math: true
excerpt_separator: <!--break-->
---
<div align=center><h2> Hybrid EEG-EOG based BCI system for Vehicle Control 논문 리뷰</h2></div>
<!--break-->

----


 <br/>

## INTRODUCTION
기존의 EEG를 기반으로하는 real-time MI task는 잠재력을 갖지만, low spatial resolution와 low signal-to-noise ratio (SNR)을 겪고 있습니다. 

본 논문은 desired movements를 정확하게 분리하고 빠르게 계산하는 real-time system(Vehicle Control System)을 개발하는 것을 목표로 합니다.

Hybrid systems은 MI brain activities를 다른 기능과 함께 사용하여 보다 의미 있고 강력한 제어 신호를 생성할 수 있습니다.

특히 EOG는 대부분의 장애인들이 눈을 움직일 수 있기 때문에 장애인들에게 가장 유망한 생리학적 신호 중 하나로 여겨집니다.

기존의 EEG, EEG-EOG based hybrid SOTA 기술들은 feature extraction, classification의 방법론에 집중을 합니다.<br/>
하지만 본 논문은 널리 알려진 ML기법을 사용해 많은 계산시간 없이도 높은 정확도를 보이는 real-time vehicle control EEG-EOG hybrid system을 제안합니다.


## STATE-OF-THE-ART FOR HYBRID EEG-EOG BASED BCI SYSTEMS
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/174475521-00393708-7790-4c3c-8648-d1159fabe84d.png" width="100%"/>
</div>
> Summary of the state-of-the-art techniques

EOG-EEG 기반으로하는 아래와 같은 시스템들도 존재합니다.

1. P300 EEG를 기반으로하고 EOG로 특정 target을 선택하는 시스템
2. EOG로 output commands를 해석하고 EEG를 사용하여 subject가 의식적으로 눈의 움직임을 제어할 수 있는지 여부를 결정하는 시스템
3. [machine control] EOG를 사용해 좌, 우를 분류하고 EEG를 사용해 전진, 정지, 완전 정지를 분류하는 시스템

### PROPOSED EEG-EOG BASED HYBRID SYSTEM
본 논문에선 EOG를 통해 좌,우를 분류하고 EEG를 통해 전진, 후진, 정지 를 분류합니다.

본 hybrid system은 EOG와 EEG는 개별된 알고리즘으로 분류한 후 결과를 결합하는 mechanism 입니다.

### A. Data Collection
먼저 BCI competition IV 2a data를 사용해 사용할 모델을 train, test했습니다.

BCI competition IV 2a data는 EEG와 EOG를 포함한 실험 dataset이며, 최고의 feature extraction과 classification의 조합을 찾는 것이 목적이었습니다.

이후 실험자 13명을 통해 아래와 같은 지시를 통해 데이터를 수집하고 train, test했습니다.
- 모니터에 left, right cue가 나타나면 눈을 해당방향으로 돌립니다.
- 모니터에 up cue가 나타나면 입속의 혀를 위로 올립니다.
- 모니터에 downward cue가 나오면 뒤꿈치룰 붙인상태에서 발을 들어올리는 상상을 합니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/174477161-95327b97-7611-4b17-bb22-1925a3718841.png" width="80%"/>
</div>
데이터 수집과정은 위와 같으며, EEG와 다르게 EOG의 경우 처음 3초간의 데이터로 정규화되고 데이터 수집시간의 중간 1초만의 데이터를 사용합니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/174478713-73bc0dfd-8b7a-4108-a5dc-8acbc365bbd7.png" width="40%"/>
</div>
EEG의 경우 SOTA 방법들과 같이 12개의 중간 부분에 전극을 부착했습니다.<br/>
반면 EOG의 경우 "crow's feet"(코와 반대 부분인 양쪽 눈 옆)이라는 부분에 전극을 부착했습니다.

### B. Pre-Processing & Feature Extraction
BCI competition data와 in-house data는 모두 SNR을 높이기 위해 전처리 됩니다.

8−35Hz의 BPF가 적용 되며 z-score 정규화를 적용합니다.

EEG의 feature는 FBCSP 기법을 통해 추출되고,<br/>
EOG는 초기 3초로 정규화 되고, 수집된 데이터중 1초만 사용됩니다. 이후  좌,우 EOG data의 차이값의 평균이 계산되어지며 이것이 EOG의 feature로 사용됩니다.

### C. Classification
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/174478101-6f7a420b-ab5b-4da6-be52-e00ee0417a3d.png" width="70%"/>
</div>
EOG로 좌, 우를 분류하기 위해서 차이값의 평균을 통해 참가자들마다 threshold 값을 구했습니다.
> threshold 값은 좌우 눈의 움직임에서 오는 평균 차이의 중간값임

threshold 값이 클 경우 우측, 작을 경우 좌측으로 이동하며, 같을 경우 SVM로 EEG를 전진, 후진으로 분류합니다. 

## RESULTS & DISCUSSION
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/174478477-54208918-0780-4244-aea4-9031ad5fff9a.png" width="80%"/>
</div>

## CONCLUSION
본 논문은 전반적인 정확도와 계산 속도를 향상시키며, EOG로 좌우를 control하고 EEG를 통해 전/후진을 control하는 task를 실험했습니다.

in-house data로 87.3%의 accuracy를 달성했으며, 이후 적절하고 더 많은 피실험자의 훈련을 통해 더욱 정확도를 향상시킬 수 있을 것이라합니다.

---

## 고찰
본 논문에서 언급한 것 처럼 FE, CLS 기법에 대한 insight를 얻진 못했지만, VR과 접목한다면 EOG로도 VR Locomotion 제어에 적용할 여지가 있을 것 같다.

예를 들어 논문과 같이 전후좌우를 조정할 수도 있을 것이고, 몸통을 시선방향(얼굴방향)으로 움직일 때의 눈주변 근육을 조사해 몸통의 방향을 회전 시킬지 분류할 수도 있을 것이며,

이외에도 EOG를 통해 버튼을 누르거나 특정 interface를 불러오는 식의 sub task도 처리할 수 있을 것이다.

###### Reference
* DOI: 10.1109/BCI51272.2021.9385300