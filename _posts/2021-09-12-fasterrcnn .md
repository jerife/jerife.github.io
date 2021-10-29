---
layout: post 
title: Introduce to Faster R-CNN
subtitle: 2-Stage Detection Model (3)
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Computer Vision, Object Detection]
comments: true
excerpt_separator: <!--break-->
---
<div align=center><h1>Faster R-CNN 이해하기</h1></div>
<!--break-->

----

 <br/>
# Faster R-CNN이란?
###### R-CNN의 연산량 한계를 극복시킨 Fast R-CNN에서 Region Proposal부분에 Nerual Network 구조를 적용 시켜 더 빠른 속도를 자랑하는 모델입니다.  <br/> <br/>
> ###### Fast R-CNN + RPN(Region Proposal Network) 

![fasterrcnn_img_1](https://user-images.githubusercontent.com/68190553/132939467-1957f674-2050-4d60-8dfa-e24d62f772a1.png){: .mx-auto.d-block :} <br/>

### 2-Stage Detection Model의 메커니즘 (Faster R-CNN)

* **1) Region Proposal Network**<br/>
* **2) Anchor Box**<br/>
* **3)  Region Proposal Network Loss Function**<br/>


### [1] Region Proposal Network
![fasterrcnn_img_2](https://user-images.githubusercontent.com/68190553/132939744-4aa27ce8-e913-4306-bdf1-abe3a66698c3.png){: .mx-auto.d-block :} <br/>
###### 우리는 1개의 이미지를 FPN(Feature Pytamid Network / CNN Model)에 입력해주면, Feature map을 얻게 될 것입니다. 여기서 Fast R-CNN는 기존 이미지에서 2000개의 Region Proposal 영역을 Crop후 ROI Pooling 과정을 거쳐왔습니다. <br/> <br/>
###### 반면 Faster R-CNN은 **RPN(Region Proposal Network)라는 부분에서 Region Proposal 영역을 얻고 Crop후 ROI Pooling 과정**을 거칩니다. <br/> <br/>
###### 즉, 따로 2000개의 영역을 알고리즘으로 추천 받지 않고 Deep Learning 기술을 통해 추천 받기에 시간적으로 큰 개선을 줍니다. <br/> <br/>

##### **HOW?** <br/>

### [2-1] Anchor Box
![fasterrcnn_img_3](https://user-images.githubusercontent.com/68190553/132940137-0f770866-355f-4d3e-a310-3470ba4b7276.jpeg){: .mx-auto.d-block :} <br/>
###### 여기서 영역을 추천 받을 수 있는 혁신적인 방법인 **"Anchor Box"**를 이용하는 매커니즘이 등장합니다. <br/> <br/>
###### 먼저 Anchor Box를 이해하기 위해, FPN에서 나온 Featrue map의 Shape을 [512, 5, 5] (Channel, width, height) 라 가정 합시다. <br/> <br/>
###### Anchor Box는 이 Featrue map의 1칸 마다 9개의 임의의 상자(사진 속 상자)를 갖습니다. 즉,  [512, 5, 5]] Shape를 갖는다면, 한 Channel 당 5x5= 25(25개의 칸) x 9(9개의 상자) = 225개의 Anchor Box를 갖는 것이죠. <br/> <br/>
![fasterrcnn_img_5](https://user-images.githubusercontent.com/68190553/132977409-ac367f12-7559-419e-8164-acc6091cc907.png){: .mx-auto.d-block :}
> ###### 사진 속 이미지의 Feature map의 Size는 13x13입니다. <br/>

###### 이 225개의 Anchor Box들은 원본 이미지(FPN에 입력되기전 순수한 이미지)에 적절한 알고리즘으로 투영되며, 학습시 Target값에 해당하는 Bounding Box와 가장 인접한 Anchor Box와 Loss값으로 가중치가 업데이트 됩니다.<br/> <br/>


### [2-2] RPN의 학습
###### Faster R-CNN의 RPN은 Anchor Box를 통해 (1)Image Pos/Neg Classification, (2)Bounding Box Regression 두가지를 예측합니다. <br/>
![fasterrcnn_img_4](https://user-images.githubusercontent.com/68190553/132977908-7b6fec9d-282d-490a-929f-3c16add40457.png){: .mx-auto.d-block :} <br/>
* ###### [1] Image Pos/Neg Classification
###### 1. 원본 이미지의 Target값과 IOU가 0.7이상인 Anchor Box는 Positive
###### 2. IOU가 가장 높은 Anchor Box는 Positive
###### 3. IOU가 0.3보다 낮은  Anchor Box는 Negative

* ###### [2] Bounding Box Regression
###### 1. Positive인  Anchor Box와의 좌표값 차이를 최소화하는 Regression
<img width="449" alt="fasterrcnn_img_9" src="https://user-images.githubusercontent.com/68190553/132977931-64ab7161-118d-452f-a239-d8498fa1e6ae.png">{: .mx-auto.d-block :} 
> ###### Prediect(RPN에서 Proposal)한 Bounding Box의 값 : p=(px,py,pw,ph) <br/>
> ###### Ground Truth의 값 g=(gx,gy,gw,gh)

###### 먼저 Bounding Box의 값인 P(X,Y,W,H)를 G에 가깝게 만들어주기위해 dx, dy, dw, dw라는 함수에 P값을 넣어줍니다. 즉, 우리는 d함수들이 G에 가깝게 이동시킬 수 있게 학습시키는 것입니다. <br/><br/>
###### 이때 X,Y는 점이기 때문에 위치만 이동시켜주고, W,H는 이미지의 크기에 맞게 조정해야하기 때문에 그에 맞는 계산을 추가해줍니다. <br/>

<img width="170" alt="fasterrcnn_img_10" src="https://user-images.githubusercontent.com/68190553/132978085-6dba8ffd-1d1d-42b5-a97a-3d2fbf0e586c.png">{: .mx-auto.d-block :} <br/>
###### 이제 P를 변환시킨 G로 이동시키기 위한 이동량인 tx, ty, tw, th를 정의해줍니다. <br/> <br/>
<img width="321" alt="fasterrcnn_img_11" src="https://user-images.githubusercontent.com/68190553/132978095-2def57ab-6340-475d-a3d9-b6e8ccf0ca3d.png">{: .mx-auto.d-block :} 
###### 정의해준 t 값과, 위의 연산으로 Bounding Box Regression을 진행 해주며, 이 Regression는 RPN의 학습시 사용 됩니다. <br/> <br/>

#### [3] Region Proposal Network Loss Function
![fasterrcnn_img_12](https://user-images.githubusercontent.com/68190553/132979260-a0acf85e-b8d0-444c-8546-a4e13f8cf61a.png){: .mx-auto.d-block :}
> ###### RPN은 위와 같은 Loss Function을 통해 학습을 진행하게 됩니다.<br/>

###### Image Pos/Neg Classification부분은 흔히 사용 되는 Cross Entropy 함수가 사용 되며, Bounding Box Regression부분은 위에서 정리한대로 사용 됩니다. <br/> <br/>
###### λ 기호는 Balancing Value이며, 값을 적당히 조절해주는 파라미터입니다. 논문에선 λ 을 10으로 정의했습니다. <br/> <br/>

----

###### R-CNN계열 Detect Model은 여기까지 이해하는 것으로 마치며, 이후 1-Stage Detection Model에 대해 정리하겠습니다. <br/> <br/>

> Reference
> * https://lilianweng.github.io/lil-log/2017/12/31/object-recognition-for-dummies-part-3.html
