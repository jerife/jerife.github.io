---
layout: post  
title: Efficient Net 이해하기
subtitle: Compound Scaling CNN Model
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Computer Vision]
comments: true
---

# EfficientNet이란?
###### EfficientNet은 기본 CNN모델(ResNet, MobileNet, etc..)와 같은 Feature Extractor 모델입니다. 즉 이미지를 input으로 받고 특징을 output으로 내보내는 모델이죠.<br/> <br/>
###### 그렇다면 기존 모델들과 무슨 차이점이 있기에 SOTA를 달성했을까요?<br/> <br/>
-----
![efficientnet_img_2](https://user-images.githubusercontent.com/68190553/139193693-0c5b58cd-990b-450e-a98d-13e73ef282c4.png){: .mx-auto.d-block :}
###### 우리는 기존 CNN모델을 향상시키기위해, 위 사진처럼 (a)모델을 (b)/(c)/(d) 와 같이 scaling하며 모델을 수정해왔습니다. 특히 이 과정에(b/c/d)서 주로 1가지 방법으로만 scaling해 모델을 성능을 기대해왔습니다.<br/> <br/>
###### 하지만 EfficientNet에선 (b)/(c)/(d)과정을 골고루 조합하면, 즉 (e)방법으로 적은 연산량으로 더 좋은 성능을 낼수있음을 증명했습니다.<br/> <br/>
##### EfficientNet에선 이를 **"Compound Scaling"** 라합니다. <br/> <br/> <br/> 


## [Compound Scaling]
![efficientnet_img_3](https://user-images.githubusercontent.com/68190553/139195588-84b59886-a118-4184-8f63-83ae00f4d54c.png){: .mx-auto.d-block :}
###### Compound Scaling과 비교하기전 기존 Scaling를 참고해보겠습니다. 위 사진 3개는 각각 width, depth, resoultion을 점점 늘려가면서 실험한 결과입니다.<br/> <br/>
###### 일정 수준까지 빠르게 상승하다, 일정 Accuracy(80% 쯤)에서 머무르는 것을 알 수 있습니다. <br/> <br/>
![4](https://user-images.githubusercontent.com/68190553/139196343-99d0578e-7307-4711-a4b2-d3ab3c13b581.png){: .mx-auto.d-block :}
###### 이번엔 depth와 resolution을 함께 조정한 Compound Scaling을 살펴보겠습니다.<br/> <br/>
###### 기존 Scaling으로 했을 때는 일정 Accuracy(80% 이하)에서 머물렀던 반면, Compound Scaling시 기존보다 3%프로 가량의 Accuracy가 상승한 것을 볼 수 있습니다. <br/> <br/> 
![efficientnet_img_4](https://user-images.githubusercontent.com/68190553/139203032-1fcea2dc-ad0e-4590-923d-f8fa8c52ac99.png){: .mx-auto.d-block :}
###### EfficientNet에선 Compound Scaling을 적용하기 위해 위의 식을 적용했습니다. width, depth, resoultion를 각각 α, β, γ로 지정하여 Φ를 제곱승 해줍니다. <br/> <br/>
###### 이때 Φ는 모델의 크기에 따라 조절할 수 있는 변수입니다.(늘릴수록 연산량 증가) 논문에선 α=1.2, β=1.1, γ=1.15를 사용하며,Φ를 조절함으로써 모델 사이즈도 조절합니다. <br/> <br/>
###### 추가적으로 논문에선 α, β^2, γ^2 = 2로 제한함으로써  FLOPs가 (α, β^2, γ^2)^2배 만큼만 증가하게 제한을 뒀습니다. 하지만 우리는 Φ를 조절함으로써 FLOPs을 증가시킬 수 있습니다.
 <br/> 


## [Architecture]
###### EfficientNet의 이론을 알았으니, 어떤 모델에 사용되는지 알아보겠습니다.
![efficientnet_img_5](https://user-images.githubusercontent.com/68190553/139201539-a7c8fe3b-bc0e-4b56-b003-7f944639a8c5.png){: .mx-auto.d-block :}
###### EfficientNet은 MobileNetV2과 MnasNet랑 유사한 모델을 사용하고있습니다. 하지만 이 모델을 바탕으로 Compound Scaling한다는 점이 차이점이죠. 
###### 사실 모델의 Architecture보단 Compound Scaling의 매커니즘이 큰 주목받고 있는 모델이랍니다.<br/> <br/>

----- 

## [Performance]
![efficientnet_img_6](https://user-images.githubusercontent.com/68190553/139202754-0f131c96-c269-4ec1-9ec2-6e14dbddd46a.png){: .mx-auto.d-block :}
###### 마지막으로 논문에서 제공한 performance에 대해 이야기하고 마치겠습니다. 
###### 위 사진들은 각각의 Scaling시 나온 Featuremap입니다. Compound Scaling이 가장 특징을 잘 추출해내는 것을 알 수 있습니다.<br/> <br/>
![efficientnet_img_7](https://user-images.githubusercontent.com/68190553/139203576-2f44a51b-f828-4fc2-906b-24646970edf6.png){: .mx-auto.d-block :}
###### 마지막은 다른 모델들과 비교한 EfficientNet의 performance입니다. 다른 CNN(Feature Extractor)보다 적은 연산량으로 좋은 성능을 낸다는것을 알수있죠.<br/> <br/>
###### 이상으로 EfficientNet에 대한 설명을 마치겠습니다.
