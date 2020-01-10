---
layout: post  
title: "Introduction into 'Dive into deep learning'"    
date: 2019-12-10   
excerpt: "intro"  
tags: [d2l]  
d2l: true  
comments: true  
---


# 1 | Introudction  

기존의 프로그래밍의 동작 구조는 몇 가지 경우가 있는데    
1. 사용자와 어플리케이션 간의 상호작용.  
2. 어플리케이션과 사용자 데이터베이스 서버 간의 상호작용.  
3. 핵심 부분인 '동작 로직'.  


핵심부분을 설계하기 위해서는 일반적으로 발생하는 모든 경우에 대하여 생각하고 수행해봐야한다. 만약에 모든 경우에 대하여 설계가 가능하다면
machine learning은 필요치 않을거다.

다행히 ML 과학 커뮤니티에서는 많은 작업들이 100% 자동화 작업이 불가능하다 한다. 예를 들면 
- 일정한 Form이 없는 설문에 대한 자동응답  
- 모든 사람이 인지할 수 있는 그림을 그리는 프로그램
- 사용자가 좋아할만한 제품을 소개시켜주는 프로그램  

많은 경우 프로그램의 상황의 변화에 따라 프로그램도 변화를 해야하기에 자주 바뀌게 된다. 이에 반해 ML은 경험을 바탕으로 하기에 새로운 프로그램을 만들 이유가 없다.
ML은 경험이 많을수록 (관찰 값이나 환경의 반응) 성능이 향상된다. 앞으로 책에서는 ML의 기본을 가르친다. 특히 deep learning에 초점을 맞춘다.

## 1.1 A motivating Example  
문자 인식에 대한 프로그램을 예로 들 수 있다. (Hello, Siri/ Okay, Google/ Alexa 등등)
프로그램을 작성 시에 input과 output을 어떻게 mapping을 할건지 알 수 없다. 내가 Alexa를 이해 할 수 있어도 컴퓨터가 이해하게 하기는 힘들다.
ML식 접근 방법은 wake word를 인식하게 시도하지않는다. 대신에 parameter에 의해 결정되도록 한다.
그리고 가장 높은 확률이 나오는 parameter를 결정하도록 dataset을 사용한다. 각 과정마다 성능을 비교해가며 향상시켜 간다.

parameters를 조작기로 보면 된다. parameter에 따라 input, output mapping의 set을 모델의 family라 칭한다. parameter를 고르기 위해 dataset을 
사용하는 meta-program을 leraning algorithm이라 칭한다.

learning algorithm을 바로 시행하기 전에 문제에 대해 먼저 정의 해야한다. input, output의 특성을 고정하고 model family를 정해야한다.  
wake words의 경우 audio input을 받아서 yes, no를 output으로 만들어낸다. 

model의 family를 찾으면 knob하나만 결정하면된다. Alexa를 들으면 yes를 답하도록. knob을 다르게 바꾸면 Apricot에 yes를 반응 하도록 할수도 있다.
Alexa에 반응하나 Apricot에 반응하나 과정은 비슷하기에 가능하다. 하지만 입력과 출력이 바뀌면 model역시 바뀌어야한다.

knob이 랜덤하게 결정되면 alexa, Apricot이나 다른 영단어에 대해 제대로 반응하지 못한다. Deep learning에서는 learning이 knob을 제대로 설정
하게 한다. 

training process는 아래와 같다.
1. 모델을 랜덤하게 초기화 한다.
2. 라벨링이 된 데이터 몇개를 고른다.
3. knob을 조정한다.
4. 모델이 알맞게 동작할때까지 반복한다.

{% capture images %}  
    /images/d2l/training_process.PNG  
{% endcapture %}

Deep learning은 machine learing 문제를 해결하기 위한 방법중의 하나이다. 그래서 Deep learning만 얘기하는게 아니라 machine learning
도 함께 얘기한다. machine learning 문제 해결 방법중 Deep learning이 중요한 이유가 있다.

1. raw audio sginal, raw pixel 값은 일반적인 ML 방법론에서는 알맞지가 않다. Deep learning은 계산을 휘한 수많은 layer로 이루어 
져 있어서 정확도가 deep 하다. 예전에는 이런 shallow model에 대하여 데이터를 변동하는 방법을 고안하기도 했다. deep learning의 장점중
하나는  shallow model을 대체 할뿐만 아니라 feature engineering 과정도 대체한다.
2. domain-specific preprocessing을 대체한다. 특히 분야에 대한 경계없이 많은 분야에 적용이 가능하다.


## 1.2 The Key Componets: Data, Models and Algorithms
