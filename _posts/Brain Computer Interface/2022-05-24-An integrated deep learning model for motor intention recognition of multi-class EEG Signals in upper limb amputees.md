---
layout: post 
title: Review - An integrated deep learning model for motor intention recognition of multi-class EEG Signals in upper limb amputees
subtitle: Motor intention recognition with LSTM & SAE
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Brain Computer Interface]
comments: true
use_math: true
excerpt_separator: <!--break-->
---
<div align=center><h2>motor intention recognition of multi-class EEG Signals in upper limb amputees with deep learning 논문 리뷰</h2></div>
<!--break-->

----


 <br/>

## ABSTRACT
본 논문에선 기존의 EEG에서 몇가지 주파수만 추출해 학습시키는 방법에 대해 오류가능성을 제시합니다. 이유로는 EEG는 각각의 주파수엔 각각의 장점과 잠재성이 있을 수 있기 때문이라 했습니다.

따라서 본 논문에선 EEG에서 다뤄지는 주파수를 ($\delta, \theta, \alpha, \beta$) 모두 사용함과 동시에 LSTM + SAE Architecture 를 이용한 모델 학습법을 제시합니다.

## INTRODUCTION
상지 절단자는 상지보형물이 삶의 질을 향상시키는데 필수적이지만 6~60% 이상이 보형물 착용을 거절합니다.

거절의 이유로는 보형물의 통제 기능과 감각 피드백이 부족하기 때문입니다. 때문에 최근 수십년간 다중 자유도(MDoF)를 지원하기위해 다양한 제어법이 연구되었습니다.

그 중에는 EMG를 이용한 제어법이 존재합니다. EMG는 근육 수축 신호를 기록하는 기술이며, 이 신호를 통해 행동이나 패턴을 인식할수 있습니다.<br/>
하지만 상지절단자 중에는 팔이 전부 절단된 Case도 존재하기때문에 EMG를 적용할 수 없다는 큰 단점이 있었습니다.

팔이 전부 절단된 Case에 적용할 수 있는 TMR라는 기술도 존재합니다. TMR은 절단된 부위 주변의 신경을 이용해 제어하는 기술입니다. 하지만 이 기술을 수술의 규모도 클뿐더러 cost도 높아 보편적으로 사용하긴 힘듦니다.

따라서 BCI 방법론이 제안됩니다. BCI는 크게 3가지 범주로 구분될 수 있습니다.
1. microscopic: 침습적 기법 + 수술 필요
2. mesoscopic: 적은 칩습적 기법(ECoG) + 비 침습 high cost(fMRI, PET, ...)
3. macroscopic: 비침습 low cost (EEG)

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/169927793-e3a393a2-bfa0-4f15-a7fe-e126745309a2.png" width="60%"/>
</div>

{: .box-warning} 
EEG(macroscopic)는 두피에서 측정되는 신호이며 LPF역할을 두개골때문에 높은 주파수 성분이 분석되지 않습니다.
반면 ECoG(mesoscopic)는 두개골 안에서 측정되는 신호이기에 높은 주파수 성분이 분석됩니다.

본 논문에선 위 3가지 범주 중 macroscopic(EEG)를 활용하는 법을 제안합니다. 


<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/169930229-0238bb80-59af-49be-ab6f-832ee1a58642.png" width="100%"/>
</div>

최근엔 EEG분야에선 하나의 Frequency를 common spatial pattern (CSP)에 적용해 특성을 추출했지만 한가지의 Frequency만을 사용하는 것은 다른 측면의 Frequency를 모두 버리는 것이기 때문에 모든 Frequency를 사용하는 것을 제안합니다.

이후 전처리돠정은 Notch Filter를 이용한 잡음 제거와 BFP만을 이용했습니다.

time series 데이터처리에 특화된 LSTM모델을 채택했지만 RNN계열 특성상 random initialization of weights가 무작위로 초기화 돼 좋은 성능을 보이지 못한다고 합니다.

하지만 사전에 데이터들의 특성을 학습한 SAE모델은(pre-trained) random initialization of weights를 우회해서 학습할 수 있습니다. <br/>
따라서 random initialization of weights 부분을 비지도학습법 접근법을 이용해 SAE모델을 사용해 대체하는 것을 제안합니다.

또한 신호의 중복적인 특징을 제거할 수 있는 t-distributed stochastic neighbor embedding (t-SNE) 기법을 이용해 LSTM의 인풋 전 차원을 축소(임베딩)함으로써 성능을 향상 시켰습니다.




결과적으로 average performance of 99.01% for accuracy, 99.10% for precision, 99.09% for recall, 99.09% for f1_score, 99.77% for specificity, 99.0% for Cohen’s kappa를 달성했습니다.


## METHODOLOGY
### A. Experimental

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/169930987-be513447-b466-43be-9603-c5c61e57d484.png
" width="80%"/>
</div>

Train 데이터 수집은 55cm 앞의 모니터에서 지시하는 손움직임(classes) 생각하는 task입니다. 5초간 휴식하고 5초간 실험을 진행하며 총 50세트를 실시합니다.

반면 Test 데이터 수집에선 모든 5초간 휴식하고 5초간 실험을 10세트를 진행합니다.

데이터는 1000$F_{s}$로 수집되었으며

EEG epochs은 5번의 실험 데이터 + 120ms plus(실험 시작하기 직전의 데이터까지)으로 5120 사이즈로 사용됩니다.



### B. Integrated LSTM-SAE
#### Data Preprocessing
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/169933992-4cd1527f-e2bc-4b32-97e9-35ece1910e02.png" width="100%"/>
</div>


EEG 데이터는 2가지의 전처리 과정만 거칩니다.
1. 50Hz Notch Filter를 이용한 artifact 제거
2. 구체적인 Frequency범위를 사용하기 위해Band Pass Filter를 이용한 $\delta, \theta, \alpha, \beta1, \beta2, \beta3$로 분해
> $ \beta2, \beta3$는 각각 지속적인 주의 처리와 정신 경각으로 특징을 갖습니다.

#### Data Embedding
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/169934197-8ab1a2a9-8e17-4de0-b17b-3ffb923f1110.png" width="50%"/>
</div>
전처리된 데이터는 t-SNE을 통해 2차원으로 차원축소(임베딩)됩니다.<br/>
t-SNE을 사용할 경우 신호의 중복적인 특징을 제거할 수 있으며, 2차원적으로 시각화했을 경우 직관적으로 이해할 수 있습니다.

#### Modeling
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/169934517-f654c0a1-02ec-4578-851a-2ac36fb51193.png" width="100%"/>
</div>
> (LSTM에 관한 설명은 [이전 포스팅](https://jerife.github.io/2021-06-06-lstm/)에 설명돼있기에 생략하겠습니다.)

이후 LSTM 기반의 SAE 모델에 데이터를 입력한 후, FC Layer에서 classes를 예측하며 마무리됩니다.


## RESULTS

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/169934946-d32607bb-e4b2-46fd-89d7-392dbf29dfa9.png" width="90%"/>
</div>

결과는 위 표와 같이 대부분이 0.99대로 높은 성능임을 보여줍니다.

## DISCUSSION
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/169935625-de935990-d4a8-4677-bfa7-88adc8622f3b.png" width="90%"/>
</div>

같은 데이터로 실험한 모델 중에서도 time series 데이터처리에 특화된 LSTM + 데이터의 feature 잘 encoder 해주는 SAE모델의 조합이 가장 놓은 성능을 보입니다.

논문에선 기존 CSP / PSD등을 이용한 handcraft Feature Extraction의 경우에는 높은 차원이나 집약적인 데이터를 계산할경우 계산의 힘이 떨어질수 있다고 했습니다.

따라서 handcraft방식이 아닌 Auto encoder를 이용한 Feature Extraction 방식이 유리하다고 언급했습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/169935659-60ec50cf-b0d9-4052-a396-d71560d7371e.png" width="95%"/>
</div>

그 밖에도 같은 task의 SOTA 모델보다도 높은 Accuracy를 보여줬습니다.

## CONCLUSION
저자들은 연구의 한계와 함께 Future work를 제안하며 논문을 마쳤습니다.

1. Subject의 수가 부족한 한계 -> 이상적인 모델을 위해 Subject의 수가 증가하길 희망
2. Classes가 5개이기 때문에 나중에 더 많은 classes 추가 희망
3. 모델이 현재 데이터셋에 맞춰져 있기 때문에, 혹시 데이터가 추가 된다면 LSTM + SAE 가 더 광범위하게 적용되기를 희망

---

## 고찰
Subject마다 Day마다 EEG 데이터가 다르게 측정될 수 있는데 정확한 실험인가에 대한 의구심이 들어 교수님께 자문을 구한 결과 앞으로의 BCI task의 goal 중 하나라고 하셨다.

또한 Modeling 과정에서 마지막에 Softmax가 아닌 Sigmoid를 사용해 Synchronous하게 여러개의 classes를 동시에 사용할수도 있을 것 같다는 생각이 들었으며, 이후 연구과정에서 이를 고려해볼 필요가 있을 것 같다.

###### Reference
*  https://ieeexplore.ieee.org/document/6476692
* https://www.researchgate.net/figure/a-Schematic-of-neural-signals-EEGs-ECoGs-LFPs-and-spikes-and-their-properties-b_fig1_321764286