---
layout: post
title: "3D Hand Shape and Pose Estimation"  
date: 2020-01-10  
excerpt: "Graph CNN을 이용한 3D Shape 데이터 생성 및 단일 이미지로 3D 정보 획득"
tags: [paper]
paper: true
comments: true
---

# 3D Hand Shape and Pose Estimation from a Single RGB Image

Original Paper Link: [here](https://arxiv.org/abs/1903.00812)

## Abstraction  
기존의 3D Hand Pose는 Key points만 예측을 하였다. 이와 달리 본 논문은 3D Mesh 까지 예측을 한다. 3D Mesh를 위해 Graph CNN
을 이용하고 fine tunning을 위해 Depth-map을 이용하였다. 이런 노력으로 state-of-the-art보다 나은 성능을 보였다.

## Introduction  
기존에 손의 Pose를 estimation을 할때 손의 모양, 자세 움직임, 폐색(occlusion)에 의해 높은 성능을 도달하기 힘들었다. 초기에는 
Depth-map을 이용하는 연구가 활발 하였지만 실내에서만 사용이 가능하다는 점, depth 센서가 필요하다는 점이 문제가 되어 monocular 
RGB만을 이용한 연구가 시작되었다. 그리고 초기에는 손의 pose에만 관심을 가지다가 AR/VR이 활발해지면서 실제 손의 shape를 추출할 
수 있는 방법에 대한 관심도 증가하게되었다.  

이 논문에서는 end-to-end로 3D mesh, 3D pose 예측 모델을 제안하고 predifine된 triagle mesh topology와 mesh vector 좌표 평가를
 목표로 한다.  
 이를 달성하기 위해서는 3가지 문제가 있다.
 - Output이 너무 고차원이다. 기존에 pose estimation은 output이 21x3으로 output이 크지 않았지만 3D shape의 경우 1280x3으로 
 output의 크기가 상당히 크게 된다. 이에 대한 문제를 Graph CNN을 통해 해결한다.
 
 - 실제 데이터가 없다. 많은 Hand pose 3D estimation 연구들이 가지는 문제점이다. 실제 손 이미지에 대하여 3D 좌표를 찍는다는것이 
 매우 힘든일이기에 많은 양의 데이터를 구하기가 힘이든다. 많은 연구들과 동일하게 가짜 이미지를 생성하고 진짜이미지로 변환 하는 작업을
  통해 데이터셋을 얻는다. 이 과정에서 Depth-map을 이용해 보완을 한다.

## Related work

### 3D Hand Shape and Pose Estimation from depth images
- hand model에 대하여 반복과정을 거쳐 shape를 만들어간다. LBS를 쓰는데 이거에의해 오히려 성능이 제한된다.

### 3D Hand pose estimation from RGB Image
- 일반적으로 Pose에만 집중이 되어있고 shape는 측정하지 않는다. end-to-end의 형태가 아닌 점이 단점이다.

### 3D human body shape and pose estimation from a single RGB Image
- SMPL을 이용해 2D Key point에 집중되거나 실루엣을 뽑아내기위해 성능이 맞춰져있다

### 3D Hand Shape and Pose Dataset Creation
- real-world 에서 3D mesh에 대한 Ground truth를 생성하는건 쉽지가 않다. 현재 있는 Dataset은 joint position에 대한 데이터만 
제공을 한다. 이로 인해 본 논문은 Maya를 이용해 직접 데이터를 만든다.
500개의 동작, 1000개의 카메라 뷰, 30개의 빛, 5개의 피부색을 이용하여 375,000가의 위조데이터를 만든다. 이중 60,000개를 
validation으로 사용한다. 그리고 583개의 실제 이미지도 순수 evaluation 목적을 위해 사용 한다.

## Method
### Overview
- 단일 RGB이미지에서 Full 3D mesh와 3D 손 좌표를 생성하는 방법에 대해 제안한다. 2 stacked hour glass 모델을 거쳐서 2D Heatmap을 추론하고. 추론된 heatmap은 latent vector로 인코딩되는데 이는 8개의 residual model, 4개의 pooling을 거친다. latent vector는 
Graph CNN의 입력으로 들어가 N vertice의 3D Hand Mesh를 생성하게된다. 3D 손 좌표는 추출된 3D Hand mesh vertices를 linear Graph CNN을 통해 추론된다.  
  
- 이 작업을 위해 먼저 위조 데이터셋에 대하여 모델을 학습시킨다. 이후 실제 이미지에 대하여 학습 시킨다(fine-tune). 위조 데이터의 경우 3D 좌표, 3D Mesh가 있어 end-to-end로 full supervised가 가능하다. 실제 이미지에 대해서는 3D Mesh와 3D 좌표가 없어 semi supervised로 훈련을 하고 이때는 depth camera를 이용한다. 이떄 평가를위해 3D Mesh를 생성하는데 3D Mesh에 대한 quality를 보장하기위에 pseudo-ground truth mesh를 생성한다. 이에 대한 내용은 이후 설명이 된다.  

### Grpah CNNs for Mesh and Pose Estimation  



{% capture images %}  
    /images/3d hand shape/fig_4.PNG  
{% endcapture %}

- 3D Hand mesh는 graph 구조로 나타낼수있다. 이를 위해 Chebyshev Spectral Graph CNN을 이용하여 3D 손좌표를 생성한다.  

- 3D mesh는 무방향 그래프 M = (V,E,W)로 표현이 가능하다. V ={v_i} N개의 Mesh의 vertice를 나타낸다. E={e_i} Mesh에 존재하는 edge를 뜻하고 W=(w_ij)로 mesh의 인접 행렬을 나타낸다. (i, j)의 edge가 존재한다면 w_ij=1 아니면 w_ij=0 으로 표기한다. 정규화 grpah Laplacian은 "L = I-D^(-1/2)*W*D(^-1/2)" 로 계산된다. D= diag(sig(w_ij)) 이고 W는 인접행렬을 뜻한다. M의 Laplacian L은 학습하는 동안 수정된다.  
- NxF 차원의 signal f=(f_1 ... f_N)^T  (그래프의 vertice)가 주어지면 3D Mesh의 N개의 vertice들을 F-dim으로 표현한다.
> f_out = sig( T_k(L) * f_in * theta_k)  
> T_k(x) = 2xT_(k-1) - T_(k-2)(x) // T_0 = 1, T_1 = x  
> L: Rescaled Laplacian := 2L/lambda_max - I_N // lambda_max: L의 최대 고유 벡터  
> theta_k: 학습 변수

- 위 식은 K-localized로 K 차원의 다각형 그래프 라플라시안으로 K개의 연관 노드에만 영향을 미친다.  이에 대하 더 깊은 이해는 [Convolutional neural networks on graphs with fast localized spectral filtering.]을 참고 한다.

- 이 과정에서 mesh generation을 위해 계층구조를 설계하였다. 이는 coarse에서 fine으로 나아가는 graph convolution을 수행한다. coarse grpah의 toplogies는 미리 계산되고 학습과 테스트 과정에소 고정되어 있다. Multilevel clustering algorithm을 통해 coarse의 자식들을 fine grphe에 맞게 계산 한다. camera의 intrinsic과 상관없이 추론하기 위해 UV 좌표계와 mesh의 depth를 출력한다. 이 값들은 camera intrinsic으로 3D 좌표계로 변환이 가능하다.  
  
- 3D 좌표에 대한 평가는 3D mesh vertice들을 simplifiedd Graph CNN을 통해 가능하다. 

### Fully-supervised Training on Synthetic Dataset


## Experiment

## Conclusion