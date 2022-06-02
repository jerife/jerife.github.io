---
layout: post 
title: Review - A deep learning method for single-trial EEG classification in RSVP task based on spatiotemporal features of ERPs
subtitle: RSVP task based on spatiotemporal features of ERPs
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Brain Computer Interface]
comments: true
use_math: true
excerpt_separator: <!--break-->
---
<div align=center><h2>A deep learning method for single-trial EEG classification in RSVP task based on spatiotemporal features of ERPs 논문 리뷰</h2></div>
<!--break-->

----


 <br/>

## ABSTRACT

대부분의 CNN 모델은 event-related potentials (ERP) 구성 요소의 phase-locked(위상 고정) 특성을 잘 고려하지 않습니다.

따라서 본 논문은 phase-locked(위상 고정) 특성을 이용해 ERP 구성 요소의 공간적인(spatial) 특징을 다른 주기마다 학습합니다.


## Introduction

본 논문은 EEG classification 중 rapid serial visual presentation (RSVP) task를 다룹니다.

RSVP은 빠른 속도로 여러 이미지가 나타나며 User는 이미지를 보고 target과 non-target을 식별하는 task입니다.<br/>
이때 event-related potentials (ERP)는 이미지 stumli에 의해 유발됩니다.

{: .box-warning}
P300은 target 이미지에 의해 발생하지만 non-target 이미지에는 발생하지는 않습니다.

기존엔 아래와 같은 traditional machine learning로 task가 수행되었습니다.
- linear discriminant analysis (LDA)
- Fisher linear discriminant (FLD)
- Hierarchical discriminant component analysis (HDCA)
- spatially weighted FLD (SWFP)
- principal components analysis (PCA) 

linear한 모델들은 faster하고 robust하지만 performance가 떨어진다는 단점이 있습니다.

반면 deep learning은 강력한 non linear하게 계산하며 다차원 데이터에서 계층적이고 본질적인 특징을 더 깊이 추출할 수 있습니다.

그중 CNN은 EEG 연구에서 중요한 시간적(temporal) 및 공간적(spatial) 특징을 추출하기 위해 별도로 컨볼루션을 작동하도록 구성할 수 있습니다.

CNN 기반 deep learning 모델중엔 EEG classification를 제안하는 모델들이 있습니다.
- **[Shallow ConvNet / DeepConvNet]** : EEG를 각각의 주파수 band에 temporal convolution를 이용해 분해한 후 spatial convolution를 합니다.
- **[EEGNet]** : depthwise, separable convolution를 통해 temporal, spatial convolution을 강하게 매핑합니다.

그러나 대부분의 CNN 모델은 event-related potentials (ERP) 구성 요소의 phase-locked(위상 고정) 특성을 잘 고려하지 않습니다.

기존 모델들은 가중치가 공유되는 convolution layer를 이용해 같은 spatial filter로 convolution되기에 다른 ERP 구성 요소들로부터 차별적인 정보를 얻을 수 없습니다.

#### procedure
따라서 본 논문은 
1. convolution layer로 temporal정보를 추출하고 차원을 압축합니다.
2. ERP 구성요소는 상대적으로 안정적인 지연 시간 및 파형과 같은 phase-locked(위상 고정) 특성을 가지므로, 다른 ERP의 위상을 학습하기 위해 개별 spatial filter로 데이터의 다른 주기를 학습힙니다.
3. separable convolutional로 temporal feature을 추출한 뒤 FC layer와 softmax를 통해 classifer합니다.

## Material and methods
### A. Experiment procedure
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/171114941-cdeb591c-0dcd-4e31-bc89-5ff9b6e0e5d5.png" width="100%"/>
</div>

실험은 이미지 속에 보행자가 있는지 / 없는지 분류하는 task입니다.<br/>
4 ssesion으로 구성되있으며, 각 ssesion은 50 block으로 구성됩니다. 그리고 각 block은 100개의 이미지를 10Hz마다 보여줍니다.

한 ssesion에 75개의 target 이미지가 랜덤하게 들어가며 P300 요소와 집중도가 떨어지지 않게하기 위해 각 block엔 1~2개의 target 이미지만 들어갑니다.

실험은 검은화면이 1초 유지되고 이후  0.2초깜박인 후에 위 내용의 task가 10초동안(100장의 이미지) 실행되고 5초간 휴식합니다.<br/>
이 과정은 50(blocks) * 4(sessions) 진행됩니다.

### B. EEG acquisition and preprocessing
EEG는 60 Ag/AgCl electrodes에서 얻어졌으며 1000Hz로 샘플링 되었습니다.<br/>
EEGLAB의 FIR filters로 0.3–28Hz BPF가 적용 되었습니다.
데이터는 1024개(대략 1.xx초)의 segment로 나눠졌습니다. 이 segment는 이미지 자극이 시작하기(onset) 0.2초 전에 진폭의 평균만큼 빼졌습니다.<br/>
이후 1024개(대략 1.xx초)의 segment는 128 샘플로 downsample 되었습니다.(1개당 대략 0.008x초) 총 60개의 전극 x 128샘플의 shape를 갖습니다.

### C. Neural network architecture
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/171521307-7da18cd9-1669-4f0e-b311-7bea163738fd.png" width="100%"/>
</div>

모델의 모듈은 크게 4가지로 구성되어있습니다.

#### Module 1
Conv2D(kernel size: 1,32)는 256ms 윈도우 사이즈이며 224ms overlap됩니다.<br/>
이 Conv2D는 temporal feature 추출할뿐 아니라 input data를 다른 주기로 나눌 수 있으며 temporal 차원을 줄일 수 있습니다. 또한 이 과정은 각각의 ERP구성을 적절한 spatial filter로 학습할 수 있습니다.<br/>
이후 분포가 shift되는 것을 방지하기 위한 batch normalization (BN)와 linear activation function인 ELU를 사용해 negative value of loss가 되는 것을 방지했습니다.

#### Module 2
Module 2는 permute를 이용해 sptial feature를 추출했습니다.<br/>
먼저 DepthwiseConv2D(kernel size: 60,1)로 각각의 spatial feature를 학습하기 위해 permute layer에서 eeg_channel x filter x period (60x8x25)로 차원을 변경합니다.<br/>
이후 DepthwiseConv2D를 진행함으로써 25개의 spatial filter를 얻습니다. 다른 주기를 학습하는 각각의 spatial filter는 설계는 ERP의 phase-locked(위상 고정) 특성을 활용합니다.<br/>
이 후 permute를 이용해 원래 차원으로 수정해준 수 Module 1과 마찬카지로 BN, ELU layer를 거칩니다.

#### Module 3
SeparableConv2D(kernel size: 1,9: Conv2D와 같지만 연산량을 줄여줌) temporal features를 추출합니다.
이 후 BN, ELU layer를 거치고 global average pooling layer를 이용해 차원을 줄이고 overfiting을 방지합니다.

#### Module 4
Module 4는 마지막에 2개의 값만 나오게 해주는  FC layer와 softmax를 이용해 두 classes에 대한 확률값을 반환해줍니다.


### D. Training settings and implementations
- model의 모든 parameter는 Glorot Uniform method에 의해 초기화
- class의 불군형을 맞추기위해 class에 가중치를 부여함
- optimizer: Adam
- loss function: cross-entropy
- scheduler: 5 epoch마다 valid loss가 향상되지 않으면 lr를 반씩 줄임, 20 epoch동안 valid loss가 향상되지 않으면 학습 멈춤
- batchsize: 64
- cross-validation: 4 fold(4 session중 1개는 train, 1개는 valid, 2개는 test)

## Results
### A. Classification performance
모델 성능 비교는 traditional methods(HDCA/SWFP), deep learning models (DeepConvNet/EEGNet)으로 진행됐습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/171528194-93b0234c-319a-4f31-852c-a7635ad188da.png" width="100%"/>
</div>
> true positive rate (TPR),false positive rate (FPR), F1-Score and area under the
curve (AUC)

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/171528384-2d04f66a-85f4-43f7-abc5-a426720b2105.png" width="100%"/>
</div>
> parameter 수 비교

### B. Role of permute layers

논문에서 제시한 PLNet의 permute layer는 depthwise convolution이 서로 다른 periods에 대한 spatial filter를 학습할 수 있게 하기에 중요하다고 합니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/171528438-b24a1702-826a-4081-9713-3eee762572a0.png" width="100%"/>
</div>
> permute layer 추가/미추가 결과 비교

### C. Features analysis
PLNet으로 학습된 특징들을 시각화하기위해 topographies, saliency maps 두가지 방법으로 접근했습니다.

#### 1) topographies
먼저 Spatial topographies를 사용하기 위해 25개의 spatial filter의 가중치 중에서 가장 discriminative한 특성을 보이는  9, 13, 17번쨰 filter를 시각화 했습니다.
> 9번째 filter: 256–512 ms<br/>
> 13번째 filter: 384–640 ms<br/>
> 17번째 filter: 512–768 ms

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/171535058-30450548-34ef-42f8-b8d9-137333725164.png" width="80%"/>
</div>
특히 9번째 filter(256–512 ms)는 P300 신호이며 대부분의 subject들이 큰 가중치를 보였습니다.

#### 2) saliency map
saliency map은 PLNet으로 학습된 temporal features를 분석하기 위한 방법입니다.

saliency map은 Backpropagation 알고리즘을 통해 input에 대한 모델 output의 기울기를 계산하여 얻어집니다.<br/>
saliency map의 기울기 크기는 분류하기 위한 특징의 기여도를 나타냅니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/171541047-e2622320-f625-4a43-aa06-fd745407d9b0.png" width="100%"/>
</div>
> 본 논문에선 Pz채널의 평균을 사용함

결과를 보면 일반적으로 10Hz의 SSVEP의 특징을 보이고 N200, P300의 ERP 구성을 찾을 수 있었습니다.<br/>
더 큰 기울기는 300–700 ms에 나타나며, 이는 패러다임에 의해 유발되는 P300 및 기타 이후 구성 요소에 해당합니다.<br/>
대부분의 subject의 기울기에서 P100 및 N200과 같은 100~300ms 사이의 일부 ERP 구성 요소도 확인됩니다.

{: .box-note}
결과적으로 PLNet이 ERP 구성 요소의 discriminative한 특징을 효과적으로 추출할수 있다는 것을 보여줍니다.

## Discussion
PLNet의 Convolution은 temporal을 압축하고(Conv2d), 다른 주기를 바탕으로 spatial filter를 학습함(depthwise_Conv2d)으로써 ERP 구성 요소의 phase-locked(위상 고정) 특성 포착 할 수 있었습니다.

결론적으로 기존의 EEGNet 및 DeepConvNet과 같은 딥러닝 모델보다 우수한 성능을 얻었습니다.
> FPR의 감소, PR, F1-Score, AUC 증가

이후 PLNet이 phase-locked components를 기반으로 하는 패러다임에 적용되길 바라며 EEG연구에 reference가 될수도 있을것이라 하며 마무리 됩니디.

---

## 고찰
제어분야(VR, Real Time)와 RSVP를 접목시킨다면, 집중해야하는 곳으 응시해 P300을 발생시켜 특정 task를 수행 시킬 수도 있을 것 같다.<br/>
(eg. VR display에서 우측상단 모서리의 검정색 톱니바퀴를 응시하다가 갑자기 흰색 톱니바퀴가 등장할때 나오는 신호를 분류해 설정화면을 보여줌,
p300신호를 이용한 인지능력 활용등)

###### Reference
* https://pubmed.ncbi.nlm.nih.gov/34284365/