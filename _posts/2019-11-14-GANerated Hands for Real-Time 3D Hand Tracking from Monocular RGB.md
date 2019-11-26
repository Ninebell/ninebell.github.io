---
layout: post
title: "GANerated Hands for Real-Time 3D Hand Tracking from Monocular RGB"  
date: 2019-11-14  
excerpt: "Hands estimation in Real-Time 3D Hand from Moncular RGB"
tags: [GAN, UNet, ResNet, Pose estimation]
paper: true
comments: true
---

# GANerated Hands for Real-Time 3D Hand Tracking from Monocular RGB
Original Paper Link: [here](https://handtracker.mpi-inf.mpg.de/projects/GANeratedHands/)  
My implementation Link: [here](https://github.com/Ninebell/GaneratedHandsForReal_TIME)

---

## Abstract
논문은 challenge problem인 단일 RGB영상으로 3D hand tracking을 다룬다. 
합성곱 신경망과 kinematic 3D hand model을 결합한 방법을 사용한다. 
kinematic 3D hand model은 occlusion 이미지와 다양한 각도의 영상에 대하여 
잘 대응할 수있게 하고 손의 움직임을 부드럽게 하기 위한 Model이다.  
논문에서 학습을 할 때 Cycle GAN과 U-net을 기반으로한 GeoConGAN을 활용한 
synthetic dataset을 보강하여 기존의 3D 손 데이터 셋이 부족한 문제를 해결하였다.  
---
  
## Introduction  
손의 3D 골격 추적기는 VR/AR에서 오랫동안 목표이다. 일반적으로 마커가 있는 장비나 Depth 카메라를 이용하거나 부수적인
장비들을 필요로 하는연구들이 진행되어 왔다. 하지만 이런 장비들은 비용이 비싸다는 점과 보편적이지 않고 모든 
장면들에 대해서 사용할수 없기에 이런 장비없이 활용 가능한 기술을 선호한다. 

그래서 논문은 단일 RGB 카메라로 occlusion과 clutter에 대하여 강경하게 대응하는
 3D 골격을 추적하기위한 방법을 다룬다. 기존의 RGB 카메라를 통한 3D 손 골격 추적은 한계가 명확하게 있다.
  단일 뷰가 아닌 멀티뷰가 필요로 한다거나 3D 추적은 불가능하고 2D만 가능하거나, 3D를 추적해도 
  occlusion에 대하여 반응하지 못하는 한계가 있다.  
  
 최근 신체 관절 추적기 관련 연구에서 CNN을 기반으로 한 2D, 3D 관절 모델을 활용하여 
 진행한였다. CNN을 학습 시키기 위해서는 레이블링이 된 데이터가 필요한데 2D 이미지는 문제가 없으나 3D의 경우 
 정확한 표시를 하기에는 문제가 있었다. 이를 해결하기 위해 기존의 image-to-image 변환 모델을 활용하여
 이미지에서 3D좌표 값을 가진 이미지를 뽑아내는 모델을 만들고 이를 활용해 3D 좌표에 대한 Dataset을 만들었다.
 
 __Summary__
 - 단일 RGB 이미지로부터 손의 3D 관절을 tracking 하는 첫 시스템.
 - 기하학적 형태가 유지되는 image-to-image GAN.
 - 위의 GAN을 활용하여 실제 이미지 손의 분포도와 유사한 synthetic 데이터셋 생성.
 - 3D 좌표값이 있는 새로운 RGB 데이터셋을 생성.
 
 ---
 
## Related work
- Multi-view methods  
>   Multi-view method는 occlusion 이미지에 대한 해결 방법으로 제안되었다. [Wang]()의 논문에서는 2개의 카메라를 이용해 DB에서 
    가장 유사한 손의 모양을 찾아 도출하는 방법을 사용하기도했다. [Oikonmidis]()의 논문에서는 8개의 카메라가 설치된 환경에서 손을 추적하고
    유사한 모습을 만드는 방법을 사용하였다, 등등 다양한 방법으로 여러 카메라를 이용한 손을 추적하거나 shape을 생성하는 논문들이 있었다.  
    여러 카메라를 이용한 환경 설정은 카메라간의 calibration을 조정하기 힘들고 제약된 상황에서만 사용할 수 있다는 단점이 있다.   
    최근 [Simon]()의 논문에서 2D, 3D 손의 모양을 대량 생성하는 방법을 제안했다. 하지만 2D에 중점을 두어 3D 데이터에 대한 방법은 불가능 하였다.  
    
     
- Monocular methods
>   3D 손 모양에 대한 monocular method는 무리한 장비 설치 없이 바로 사용이 가능하기 때문에 굉장히 활용적이다.    
    [Hamer]()의 논문에서 처음으로 손추적을 위한 RGB-D data를 만들어내는 방법을 제안했다. 이 논문에는 local optima 문제가 있어 [Keskin]()
    이 learning based discriminative method를 제안하였다. 이후로도 많은 논문들이 나왔고 generative, discriminative method를 섞은 방식이 
    가장 성능이 잘나왔다.  
    위의 여러 monocular RGB-D나 depth based 손 추정 평가 법의 진전에도, 장치가 범용적으로 동작하지 않는다. 특히 3D좌표에 대하여
    정확도가 떨어진다. 가장 가까운 물체로 부터 좌표를 얻거나 Z값을 고정 시켰기 때문이라 생각한다. [Zimmermann and Brox]()는 
    learning based method를 제안하였다. 해당 논문은 3D 관절이 표준 프레임에 상대적인 좌표를 제공하지만 절대적인 위치를 알수가 없는 문제가 있다.
    그리고 occlusion 이미지에 대하여 민감하게 반응한다. 그리고 추상적인 2D heatmap으로 부터 3D 예측을 하여 같은 2D 좌표에 대하여 3D 좌표를 
    구분할 수 없다.  
    이를 보완하여, 논문에서는 2D와 3D간의 joint learning을 활용하여 2D 좌표의 모호함을 해결하고 fitting framework를 합쳐 global 3D 좌표를 구한다.

- Training of learning-based methods
>   손 자세 추정을 위한 learning based 모델은 진짜 이미지의 분포와 유사하게 annotation 된 데이터가 부족하여 challenge 문제 중에 하나이다.
    이를 해결하기 위해 많은 방법들이 나왔지만 occlusion과 여러 카메라를 이용하는 문제등으로 인해 진짜 이미지 데이터셋 구축은 힘들었다.
    결과적으로 가짜 데이터셋을 구축하여 활용하는 방안으로 나아갔지만 이 또한 도메인간의 차이가 있어 실제 이미지에 대한 동작은 무리가 있다.  
    이러한 도메인간 차이를 해결하기 위해 여러 방안들이 나왔다. 이에도 real-synthetic data간의 pair가 존재해야하는 문제가 있었고 해결방안으로
    [Shrivastava]()의 논문에서 unpair한 데이터셋을 이용하여 학습하는 방법을 제안한다. 이 역시도 pixel간 유사성을 따진다는 문제 점이 있었다.
    Cycle GAN이 나오면서 이에 대한 문제가 완화되었다. 이를 더 가치있게 하기 위해 논문에서는 geometric consist를 추가하여 조금 더 annotation에
    정당성을 추가하였다.
    
    


---

## Method
- Hand tracking system
    - Generation of Training Data
        - Real hand image acquisiton
        - Synthetic hand image generation
        - Geometrically consistent CycleGAN(GeoConGAN)
    - Hand Joints Regression
    - Kinematic Skeleton Fitting
## Experiment

## Conclusion


