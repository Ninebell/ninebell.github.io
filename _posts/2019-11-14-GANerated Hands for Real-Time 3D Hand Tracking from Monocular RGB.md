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



## Abstract
논문은 challenge problem인 단일 RGB영상으로 3D hand tracking을 다룬다. 
합성곱 신경망과 kinematic 3D hand model을 결합한 방법을 사용한다. 
kinematic 3D hand model은 occlusion 이미지와 다양한 각도의 영상에 대하여 
잘 대응할 수있게 하고 손의 움직임을 부드럽게 하기 위한 Model이다.  
논문에서 학습을 할 때 Cycle GAN과 U-net을 기반으로한 GeoConGAN을 활용한 
synthetic dataset을 보강하여 기존의 3D 손 데이터 셋이 부족한 문제를 해결하였다.
  
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
 
## Related work

## Method

## Experiment

## Conclusion


