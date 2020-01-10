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
- 

## Experiment

## Conclusion
