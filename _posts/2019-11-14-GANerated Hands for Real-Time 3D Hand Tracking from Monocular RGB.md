---
layout: post
title: "GANerated Hands for Real-Time 3D Hand Tracking from Monocular RGB"  
date: 2019-11-14  
excerpt: "Hands estimation in Real-Time 3D Hand from Moncular RGB"
tags: [GAN, UNet, ResNet, Pose estimation]
paper: true
comments: true
---

## GANerated Hands for Real-Time 3D Hand Tracking from Monocular RGB
Original Paper Link: [here](https://handtracker.mpi-inf.mpg.de/projects/GANeratedHands/)
My implementation Link: [here](https://github.com/Ninebell/GaneratedHandsForReal_TIME)

##요약
> 해당 논문에서는 GeoConGAN 모델을 통해 Synth 이미지를 통해 Data를 확보하여 RegNet을 학습,
> Kinematic Skeleton fitting을 통해 손 관절의 2D Heatmap과 3D 좌표를 추적하는 기법에 대한 논문이다.

##논문 내용
1. GeoConGAN
    1. CycleGAN
    2. UNet
    
2. RegNet
    1. ResNet
    2. ProjLayer  
    
3. Kinematic Skeleton Fitting

##생각:

