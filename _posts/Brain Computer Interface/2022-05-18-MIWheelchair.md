---
layout: post 
title: Review - Brain Controlled Wheelchairs A Robotic Architecture 
subtitle: MI based Wheelchair
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Brain Computer Interface]
comments: true
use_math: true
excerpt_separator: <!--break-->
---
<div align=center><h2>MI-based Brain-Controlled Wheelchairs 논문 리뷰</h2></div>
<!--break-->

----


 <br/>

## ABSTRACT
많은 환자들은 전동 휠체어 제어하지 못하거나, 제어하기엔 위험하다고 판단되어 전동휄체어를 처방받지 못한다고 합니다.

때문에 위 논문에선 BCI기반 Asynchronous한 MI(Motor Imagery)task와 주변의 환경요소를 이용해 휠체어를 제어하는 방법에 대해 제안합니다.

## INTRODUCTION
기존 P300을 이용한 휠체어 제어는 주변 환경을 반영한 Matrix를 집중하여 목표지점에 도달합니다.

논문에선 P300을 사용하면 더 유연하게 Command를 결정할수 있지만, 휠체어가 움직이는 시간보다 도착해서 멈추고 Command를 입력받는 시간(P300을 측정하는 시간)이 오래걸린다고 지적했습니다.

따라서 움직임(오른손, 왼손, 오른발, 왼발등)을 상상하면 발생하는 EEG 신호를 이용한 MI paradigm을 적용합니다. 
반대로 위 2개의 classes(좌, 우)에 해당하지 신호가 인식되지 않을 경우 휠체어는 센서를 통해 주변환경을 인지히고 자동으로 obstacle을 피하면서 앞으로 주행합니다.


## METHODOLOGY
### A. BCI Implementation
#### Signal Processing
EEG 데이터는 $F_{s}$ = 512Hz로 수집되었습니다.

이후 주변 전극들이 서로에게 영향을 주지 않도록 Laplacian filter를 적용해 잡음 처리를 향상시켰습니다.

필터링 이후  데이터의 PSD(Power Spectral Density)를 4~48Hz 밴드에서 2Hz씩 나눠 resolution했습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/168798556-313943d8-cf5c-4589-b191-b03c32ffb530.png" width="50%"/>
</div>

{: .box-note} 
오른손을 사용할땐 좌뇌(C2)가 활성화 되고 왼손을 사용할땐 우뇌(C1)가 활성화됩니다.

따라서 C1, C2 주변전극의 변화를 감지하기 위해 (뇌파를 이용해 가고자하는 방향을 예측하기위해) 62.5 ms(16 times/s) 마다 5 overlapped (25%), 500 ms의 Hanning windows를 이용한 Welch method를 채택했습니다.

전처리하고 변화를 매초 감지한 데이터들을 Canonical Variate Analysis으로 2 classes(좌, 우)의 차이를 극대화해줍니다.

#### Classification
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/168805280-8220b531-7532-4da2-aef4-980ef365e658.png" width="70%"/>
</div>

이 후, Gaussian classifier에 데이터가 input되고 모델이 예측하는 probability distribution값이 output되며, 정해진 threshold 이하의 값은 filtering됩니다.


### B. Wheelchair Hardware
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/168805605-10ada663-30fb-4a15-ad72-e47b950a8a06.png" width="80%"/>
</div>

BCI task에서 2 classes에 해당하지 않으면 obstacle을 자동으로 피하며 forward 한다고 위에서 언급했습니다.
자동으로 forward하는 부분은 Wheelchair task에서 처리하는 부분이므로 설정 환경을 알아보겠습니다.

1. 휠체어 뒤에 laptop을 설치해 BCI, Wheelchair의 결과를 feedback 받음
2. 결과에 따라 휠체의 바퀴를 얼마나 회전시킬지 정해주는 Wheel Encoders 사용
3. 주변 환경을 감지하고 labtop에 전달해줄 센서들 사용 (10개의 sonar sensor, webcam)
4. 사용자에게 BCI결과를 feedback해주는 display 2개 사용

### C. Shared Control Architecture

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/168932255-5b9d77cc-730a-4ec8-a5ab-2cab14c7afbb.png" width="50%"/>
</div>

Shared Control은 위의 두 input을 바탕으로(User와 주변 환경) 어떤 action을 취할지 결정해줍니다.
특히 주변 환경(sonar 센서와 cam으로 얻어진 값)이 occupancy grid를 어떻게 상호작용하며 update하는지 알아보겠습니다.

#### Computer Vision-Based Obstacle Detection
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/168933833-e0421d37-caf9-4959-b741-ba0d81b8f8ae.png" width="100%"/>
</div>
2개의 Cam은 아래의 Computer Vision 기술을 바탕으로 Obstacle을 Detection 합니다. 

1. Canny Edge Detection (b)
2. Euclidean distance 를 이용해 주변 Edge값으로 pixel이 주어짐  (c)
3. Watershed segmentation algorithm을 이용해 비슷한 pixel끼리 average (d)
4. segment된 이미지에서 가장 많은 부분을 차지하는 segmentation을 floor로 인식하고 나머지는 obstacle로 간주함 (e)

#### Updating the Occupancy Grid
논문에선 Borenstein and Y. Koren, “The vector field histogram—fast obstacle avoidance for mobile robots," 내용을 기반으로 Occupancy Grid를 Update 합니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/168941655-409457c4-8295-463f-a386-d56233e540c4.png
" width="50%"/>
</div>

특히 Computer Vision으로 얻어진 Image는 픽셀의 각 column으로부터 가장 가까운 Obstacle 감지하기 위해 아래에서 위로 스캔됩니다.
그리고 휠체어와 Obstacle의 예상 거리는 camera distortion parameters(Camera Calibration)을 이용해 추정됩니다.

sonar 센서는 6m거리까지 감지할 수 있지만 휠체어의 낮은 부분에 설치되어있어 0.5m까지의 거리만을 유효한 데이터로 사용합니다. 유사하게 Computer Vision으로 얻어진 Obstacle은  0.5~3m 까지의 거리만을 유효한 데이터로 사용합니다  

### D. Experimental Environment
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/168942048-7e53e831-82c2-4dc1-9200-b6b015f85973.png" width="70%"/>
</div>

MI기반 BCI만 사용했을 경우와 논문에서 제안한 방법을 사용하는 경우를 비교하기 위한 실험을 진행했습니다.

먼저 Subjects는 실제 운전하기 전에 정지되어있는 휠체어에서 display의 커서를 큐 화살표로 표시된 방향으로 이동시키는 MI task를 수행했습니다. (online BCI session)

이후 논문에서 소개한 방법의 휠체어에 익숙해지도록 15~30분을 제공하고 마찬가지로 online BCI session을 진행했습니다.

## RESULTS
운전을 완료하는 시간이 P300과 같은 Synchornous한 EEG를 사용했을 때보다도 훨씬 빠르다는 성공적인 결과를 도출했습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/168942068-acb2e8e4-220a-4a5f-b295-0e3b52283e2d.png
" width="60%"/>
</div>

또한 위에서 언급한 벤치마킹 실험에서도 BCI만을 사용한 실험과 논문에서 제안한 실험을 사용했을 경우의 결과는 유의미합니다.


## DISCUSSION AND CONCLUSION
위 결과를 통해서도 BCI 기술만 사용할 경우 운전시간보다도 생각하는 EEG를 측정하는 시간이 더 오래걸린다는 것을 볼 수 있었습니다.

이후에는 실제 환자를 대상으로 실험을 진행하는 중이라고 하며, 환자는 실제로 빠르게 지치기 때문에 환자의 상태를 고려할수 있는 공유제어 시스템을 개발중이라합니다.

---

## 고찰
BCI를 이용한 제어 시스템을 구축할 때 주변 환경까지 고려할 수 하는 것을 보고 관심분야인 Virtual 환경에서도 이를 적용해볼수 있을 것 같습니다.

이후 유사한 task를 구현하게 된다면 논문에서의 occupancy grid나 휠체어의 바퀴의 회전각처럼 매번 Update 해야하는 Mechanism에 대해 연구할 필요가 있어 보입니다.

###### Reference
*  https://ieeexplore.ieee.org/document/6476692