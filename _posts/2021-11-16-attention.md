---
layout: post 
title: Introduce to Attention Mechanism
subtitle: Attention is 
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Natural Language Processing]
comments: true
excerpt_separator: <!--break-->
---
<div align=center><h1>Attention Mechanism 이해하기</h1></div>
<!--break-->

----

 <br/>
# Attention란?
###### 우리는 이전에 언어처리에 관한 Seq2Seq모델을 ["Introduce to Seq2Seq"](https://jerife.github.io/2021-06-08-seq2seq/)이글에서 다뤘습니다. <br/> <br/>
![image](https://user-images.githubusercontent.com/68190553/121048577-e2bcc580-c7f1-11eb-9c39-2cb0f717ce2d.gif){: .mx-auto.d-block :} 
###### Seq2Seq의 한계는 Encoder -> Decoder로 보내는 Context Vector의 크기가 고정되었다는 것입니다. <br/> <br/>
<img width="27" alt="seq2seq_img_4" src="https://user-images.githubusercontent.com/68190553/121125066-e2a7de80-c860-11eb-8fd7-20e3e2ebefb5.png">{: .mx-auto.d-block :}
###### Context Vector는 입력값(“je suis étudiant”)의 모든 정보를 모아 놓은 동시에, 고정된 크기의 벡터이기도 합니다.<br/> <br/>
###### Context Vector는 많은양의 데이터도 꾹꾹 눌러서 압축시켜야하기 때문에, 정보 손실이 있다는 단점이 있었죠? 
###### 뿐만아니라 RNN의 Gradient Vanishing의 단점도 해결해야할 숙제였습니다.<br/> <br/>
###### 이 한계를 극복하게해준 Mechanism이 ["Attention Is All You Need "](https://arxiv.org/abs/1706.03762) 논문에서 제안된 바로 **"Attention"** 입니다.<br/> <br/>


## [Attention Mechanism] 
###### Attention Mechanism은 디코더에서 출력 단어를 예측하는 매 시점마다, 인코더에서의 전체 입력 문장을 다시 한 번 참고한다는 점입니다.<br/> <br/>
###### Attention은 "집중"라는 의미를 지니고있습니다. Attention Mechanism에서도 입력 문장을 다시 한 번 참고할때, 해당 시점에서 예측해야할 단어와 연관이 있는 입력 단어 부분을 좀 더 집중(attention)해서 보게 됩니다.<br/> <br/>
<img width="355" alt="attention_img_1" src="https://user-images.githubusercontent.com/68190553/141890357-2914bc79-9795-4213-a0f2-447504f61c89.png">{: .mx-auto.d-block :}


