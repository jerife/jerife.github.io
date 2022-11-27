---
layout: post  
title: Introduce to EfficientDet
subtitle: Compound Scaling 1-Stage Detection Model
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Computer Vision, Object Detection]
comments: true
excerpt_separator: <!--break-->
---
<div align=center><h1>EfficientDet 이해하기</h1></div>
<!--break-->

----
 
 <br/>

## EfficientDet이란?
EfficientDet은 1-Stage Detection 모델이며, EfficientNet의 Compound Scaling과 Feature Pyramid Network(FPN)에 변화를 주어 SOTA를 달성한 특징이 있습니다.

<img width="832" alt="efficientdet_img_1" src="https://user-images.githubusercontent.com/68190553/141724446-87c80822-8acc-4f15-8684-26db5da4de4e.png">{: .mx-auto.d-block :}

위 모델은 EfficientDet의 구조입니다. 대부분의 1-Stage Detection Model과 같이 "Backbone", "Neck", "Head"로 나눠지는 모습을 볼 수 있습니다.

방금 언급한 EfficientDet의 특징 이외에도 모델의 Backbone에 EfficientNet을 사용한 점도 있겠군요.<br/>
이제 "FPN의 변화(BiFPN)", "Compound Scaling" 순으로 알아보겠습니다.



## FPN의 변화(BiFPN)
### 1) Cross-Scale Connection
FPN의 변화라고해서 당황하셨을 수도 있습니다. 먼저 FPN에 대해선, ["Introduce to Yolo v3"](https://jerife.github.io/2021-10-09-yolov3/) 이글에서 언급했으니, 간단히 짚고 넘어가겠습니다.

![yolo_img_3](https://user-images.githubusercontent.com/68190553/136657129-9be97224-d7bc-4dd3-8fc5-c9bbc4ed1296.png){: .mx-auto.d-block :}

우리는 Backbone에서 여러단계를 거쳐 최종 Feature map을 출력합니다. 하지만 FPN은 중간 중간의 Feature map을 재사용해, 예측에 이용합니다.

이런 Feature map은 층이 깊어질수록 사이즈가 작아지면서 추상적인 특징을 띄우며, 중간의 Feature map은 최종 Feature map보다 사실적인 특징을 가지고 있습니다.

때문에, 중간의 Feature map을 사용함으로써 더 유의미한 특징으로 학습할수있다는 장점이 있습니다.


<img width="813" alt="efficientdet_img_7" src="https://user-images.githubusercontent.com/68190553/141730844-57cf901a-3b32-4785-9e83-c0b31db03050.png">{: .mx-auto.d-block :} 

금방 위에서 설명한 내용이 위 사진속 (a)에 해당합니다. EfficientDet에 사용하는 FPN은 위사진 속에서 (d)에 해당하며 "BiFPN"이라 부릅니다.
사실 BiFPN는 PANet(그림b)과 NAS-FPN(그림c)의 특징을 사용하고있습니다. 


#### [1] PANet의 bottom-up pathway
> PANet(그림b)의 구조를 기존의 FPN과 다르게 최종 feature map이 중간 feature map으로 합쳐지는 것(위에서 아래로 합쳐짐) 뿐만아니라, 
> 중간 feature map이 최종 feature map으로 합쳐집니다. (아래에서 위로 합쳐짐)
> 이는 서로의 특징을 더욱 다양하게 적용시킬 수 있다는 장점이 있습니다.

#### [2] NAS-FPN의 skip connection
> ResNet의 skip connection이 기억 나십니까? ResNet에선 이전 layer의 특징을 한번 더 상기시키위해 이전 layer와 이후 layer를 합쳐주는 과정을 거쳤습니다.
> NAS-FPN는 FPN부분에서 이를 적용해 마찬가지로 이전의 특징을 한번 더 상기시킬 수 있다는 장점이 있습니다.

때문에 BiFPN은 양방향으로 정보가 흐른다는 장점을 가지고 있으며, 이를 **Cross-Scale Connection**라 합니다.


### 2) Weighted Feature Fusion
다음으로 Weighted Feature Fusion에 대해 다뤄보겠습니다. FPN내의 모든 input feature map(서로 resolution이 다름)은 서로 합쳐질 때 같은 가중치를 가지고 합쳐지는데, 논문에선 이를 문제삼았습니다. 

<img width="424" alt="efficientdet_img_10" src="https://user-images.githubusercontent.com/68190553/141735609-9fa96d6b-b425-44c9-80fd-b2447440b9a6.png">{: .mx-auto.d-block :}

##### **"왜 특징의 크기가 다른데 합칠때도 가중치의 크기가 같은가?"** 

그래서 BiFPN에선 각각의 input feature map의 중요도에 따라 가중치를 부여합니다.

<img width="521" alt="efficientdet_img_9" src="https://user-images.githubusercontent.com/68190553/141735672-2ec0ea88-7fc1-4d64-b01d-0681a9736c8c.png">{: .mx-auto.d-block :}

논문에서 Weighted Feature Fusion으로 적용된 위 수식은 "Fast normalized fusion"이라합니다. 가중치들은 ReLU를 거치기 때문에 0이상이며, 분모에 입실론을 넣어서, 가중치의 값이 0~1사이값이 되도록 normalize해줍니다.

<img width="349" alt="efficientdet_img_11" src="https://user-images.githubusercontent.com/68190553/141741769-13701e2d-6b22-418e-a5d2-a9d29e69f186.png">{: .mx-auto.d-block :}

계산은 위와 같이 진행되며, FPN에서 각 input feature map에 가중치(weight) 곱해서 합친다는것을 알 수 있습니다. 

지금까지 BiFPN의 특징에 대해 정리했으며, 이제 Compound Scaling에 대해서 이야기해보겠습니다. 



## Compound Scaling
![efficientnet_img_2](https://user-images.githubusercontent.com/68190553/139193693-0c5b58cd-990b-450e-a98d-13e73ef282c4.png){: .mx-auto.d-block :}

이전 포스팅인 ["Introduce to EfficientNet"](https://jerife.github.io/2021-10-28-efficientnet/) 에서도 짚었지만, 간단히 다시 짚고 넘어가겠습니다.

우리는 기존 CNN모델을 향상시키기위해,"Depth", "Width", "Resolution"중 1가지만을 scaling하며 모델을 수정해왔습니다.

하지만 EfficientNet에선 위 세가지 과정을 골고루 조합하면, 더 적은 연산량으로 더 좋은 성능을 낼수있음을 증명했습니다.

그리고 EfficientNet에선 이를 **"Compound Scaling"** 라 했습니다. 
 

<img width="343" alt="efficientdet_img_2" src="https://user-images.githubusercontent.com/68190553/141728385-3d554d52-acfd-4df8-b5f8-97b3f7a03c8e.png">{: .mx-auto.d-block :}
> Compound coefficient(ϕ)에 따른 BiFPN의 width와 depth를 조절

<img width="218" alt="efficientdet_img_3" src="https://user-images.githubusercontent.com/68190553/141728401-6c701901-c2a8-4bc1-b782-cd81c5ae17a4.png">{: .mx-auto.d-block :}
> Compound coefficient(ϕ)에 따른 Box/class Network 조절 

<img width="191" alt="efficientdet_img_4" src="https://user-images.githubusercontent.com/68190553/141728416-c7743e4c-0f76-42ec-873b-c3c74b1f1b50.png">{: .mx-auto.d-block :}
> Compound coefficient(ϕ)에 따른 Resolution 조절

논문에선 Compound Scaling을 위해 Backbone도 EfficientNet을 사용했으며, 
EfficientNet의 Compound coefficient에 따라 위의 식들과 같이 "BiFPN", "Box/class Network", "Resolution"도 같이 조절합니다.

<img width="423" alt="efficientdet_img_5" src="https://user-images.githubusercontent.com/68190553/141728430-01414d00-6671-4bf2-b7b3-43cfc2c97957.png">{: .mx-auto.d-block :}

위 사진을 보시면  EfficientDet은 Backbone인 "EfficientNet", 그 밖의 "BiFPN", "Box/class Network", "Resolution"이 Compound coefficient(ϕ)에 따라 함께 조절된다는 것을 더 명확하게 이해할 수 있습니다.


-----
### Performance
<img width="416" alt="efficientdet_img_6" src="https://user-images.githubusercontent.com/68190553/141729540-cc2c7d71-d2a9-4e06-ab79-8fd0c994d538.png">{: .mx-auto.d-block :}

마지막으로 논문에서 제공한 performance에 대해 이야기하고 마치겠습니다. 
EfficientDet은 BiFPN과 Compound Scaling을 활용해 빠른 처리속도와 정확도를 자랑합니다. 특히 Compound Scaling에 따라 정확도가 점차 올라간 모습을 확인할 수 있죠.
이상으로 EfficientDet에 대한 설명을 마치겠습니다.


###### Reference
* https://arxiv.org/abs/1911.09070