---
layout: post 
title: Review - Robust Classification of EEG Signal for Brain Computer Interface
subtitle: P300 Speller
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Brain Computer Interface]
comments: true
use_math: true
excerpt_separator: <!--break-->
---
<div align=center><h2>Brain-Computer Interface P300 Speller 논문 리뷰</h2></div>
<!--break-->

----


 <br/>

## ABSTRACT
위 논문은 EEG의 Synchronous EEG 신호 중 P300을 이용해 6X6 단어 Matrix를 주시하며 생기는 신호를 이용한 text input application을 제안합니다.

## INTRODUCTION
P300 신호는 Subject가 원하는 자극이나 예상했던 자극을 받을 경우 300ms이후 의미있는 피크를 발생시키는 뇌파신호입니다.

해당 논문에선 유용한 BCI system이 realtime에서 적용되기 위한 학습시간 단축, 정확도 향상등을 focus했습니다.

## METHODOLOGY
### A. P300 Speller
Subject들은 6x6 Metrix에서 한 charactor를 집중합니다. 이후 Metrix의 row, column은 randomly하게 100ms 동안 총 12번 flash됩니다. 이 과정을 총 10 round진행하며, 학습과 평가를 진행합니다.

![image](https://user-images.githubusercontent.com/68190553/168705446-8c14a0c1-7245-42d1-b65d-c0721c002453.png){: .mx-auto.d-block :}
> User display in P300 Speller

### B. Data Acquisition System and Procedure
Subject들은 3ft 앞의 LCD패널을 주시하고 flash가 되는 순간의 500ms이후의 EEG 데이터를 수집합니다.
> P300: Subject가 원하는 자극이나 예상했던 자극을 받을 경우 300ms이후 의미있는 피크 발생

Train / Test(Online) 데이터 수집시엔 “THE QUICK BROWN FOX JUMPS OVER LAZY DOG 24 613 8579” 라는 문장에서 한 묶음(단어)씩 Subject에게 전달합니다.<br/>
단어를 전달 받은 Subject는 한 charactor를 집중해서 주시합니다. 주시하는 동안 LCD패널에 randomly하게 row와 colmun이 flash하며, Subject의 뇌파데이터를 수집합니다.

{: .box-warning} 
BCI(Brain Computer Interface)에선 task를 정의하고 이를 실험하여 결과를 증명하는 것이 중요하기 떄문에, 사전 Subject에 관한 정보와 실험과정을 상세하게 기재하곤합니다.

### C. Signal Processing and Classification
#### Signal Processing
EEG 데이터는 $F_{s}$ = 250Hz로 수집되었습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/168715590-7c509336-5c41-4ef3-b079-cf6756bfa9e7.png" width="80%"/>
</div>
Preprocessing 단계에선 Low Pass Filter를 사용해 뇌파 측정시 생기는 잡음을 처리했으며, 최적의 cutoff freqeuncy를 사용했습니다.

이후 데이터를 downsampling하기 위해 Moving Average Window를 사용했습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/168715401-2b200e7a-f992-4b12-851a-a5d7acadea77.png" width="60%"/>
</div>
Subject의 EEG를 측정할때 64개의 채널중에 C3, C4, Cz, CPz, FCz, P7, P8 주변의 25개의 채널만을 사용했습니다.

{: .box-warning} 
대뇌는 크게 전두엽(frontal), 두정엽(parietal), 후두엽(occipital), 측두엽(temporal)으로 나눠 져있으며, 후두엽(occipital)은 주로 시각적인 정보 처리를 위해 활성화 됩니다.<br/>
따라서 후두엽에 해당하는 P7, P8 부분의 데이터를 수집한것으로 추청됩니다.

이후 PCA를 통해 25channel 중, 20개의 eigenvalue만을 사용해 차원을 축소했습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/168730493-31aa3c2c-e3d4-4772-ab47-00a5d0a90544.png" width="70%"/>
</div>
> $u(n)$ : EOG Signal<br/>
> $w(n)$ : EEG Signal<br/>
> $n'$ = $n$-1

신호처리를 할때 데이터 속에는 자연스럽게 눈을 깜박이면서 생기는 artifact를 고려해 EOG 신호와 EEG신호를 linear superposition으로 데이터를 처리합니다.
추가적으로 inter-sample correlations을 제거해주는 과정으로 데이터를 최종적으로 처리해줍니다.

#### Classification

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/168731173-df53b992-bffa-4a95-8421-5c9b1ce4fa0f.png" width="50%"/>
</div>
위에서 처리한 데이터들을 Gaussian kernel을 이용한 SVM으로 적절한 margin값을 구하도록 학습시키고 Training과정은 마무리 됩니다.

데이터를 학습시키는 것에 주의할 점은, (Train/Test 모든 과정에서) Subject가 6X6 Matrix애서 intension할때 charactor일 확률을 데이터로 수집하는 것이 아닌 12번의 row, column이 flash할때 intension하고 있는 row, column에서 나온 뇌파를 학습합니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/168733321-45959c03-5011-4d5e-8888-4c6aa5205569.png" width="75%"/>
</div>

따라서 SVM model은 12개의 row, column을 flash할때의 데이터를 입력받고 12개의 row, column 이 target값일 확률을 반환하여 argmax로 정답을 예측합니다.

### D. Reducing Learning Time Requirement
#### Cons
해당 실험의 한가지 결점은 서로 다른 Subject끼리 방출하는 신호가 모델이 상대적이라 사용자마다 모델을 학습시켜야 한다는 점입니다.

#### Pros
논문에선 학습시간 (train model, train data acquisition) 을 줄이는 것과 정확도에 focusing 하고자 하여 2가지 케이스를 제시합니다.

> Case 1: 각 charactor마다 10round씩 진행했던 데이터를 전부 사용하지 않는다.<br/>
> Case 2: 수집한 데이터 41개를 전부다 사용하지 않는다

두 케이스에 따라 비슷한 결과이지만 학습속도를 많이 줄일 수 있다는 점을 강조했으며, 밑에서 마저 다뤄보겠습니다.


## RESULTS
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/168734959-25925d92-db39-453a-93e8-998aaab2aaba.png" width="100%"/>
</div>

Case 1, Case 2을 각각 줄이면서 학습시키고 평가한 figure입니다.
 논문에서는 위의 결과를 제시하고 charactor 25, round 7이상부턴 결과들이 비슷하다는 의견을 제시하고 굳이 모든 데이터를 학습시키지 않아도 된다고 주장했습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/168735432-fd3dcf11-3bc0-45d0-b63b-b93c55208d31.png" width="60%"/>
</div>

결론적으론 90%의 accuracy를 유지하면서 학습량은 58%줄였다는 결과를 보여줬습니다.

## DISCUSSION AND CONCLUSION
논문에선 이후 심각한 장애인들이 잠재적으로 분명하게 의사소통할 수 있는 채널을 제공한 날이 올것이라 암시하며 마무리됩니다.

---

## 고찰
대표적인 Synchronous 신호인 P300의 적용 매커니즘과 데이터 수집 환경 등의 플로우를 이해할 수 있었던 경험이었습니다.

하지만 뇌파가 사람들마다 달라서 모델을 다시 학습시켜야한다는 것을 보면서, 
더 일반적으로 뇌파 데이터를 처리하는 방법이나 모델을 고안해볼 필요가 있을 것 같습니다.

추가로 데이터를 임의로 줄여서 학습시키는 과정은 모델의 overfit 을 초래하지 않을까 우려스러운 관점이 있었습니다.


###### Reference
*  https://ieeexplore.ieee.org/document/1605260