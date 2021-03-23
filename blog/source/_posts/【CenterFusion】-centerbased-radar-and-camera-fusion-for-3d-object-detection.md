---
title: 【CenterFusion】 centerbased radar and camera fusion for 3d object detection
date: 2021-03-03 20:50:39
tags: [-paper reading, -3d detection]
categories: -三维目标检测
---
本文结合centernet和radar信息，在nuScences dataset上完成测试，创新在associated radar information with 2d image.
<!--more-->

### Authors:**

Authors:
Ramin Nabati, Hairong Qi
University of Tennessee Knoxville
{rnabati, hqi}@utk.edu


### **Abstract**

The perception system in autonomous vehicles is responsible for detecting and tracking the surrounding objects. This is usually done by taking advantage of several sensing modalities to increase robustness and accuracy, which makes sensor fusion a crucial part of the perception system. In this paper , we focus on the problem of radar and camera sensor fusion and propose a middle-fusion approach to exploit both radar and camera data for 3D object detection. Our approach, called CenterFusion, first uses a center point detection network to detect objects by identifying their center points on the image. It then solves the key data association problem using a novel frustum-based method to associate the radar detections to their corresponding object’s center point. The associated radar detections are used to generate radar-based feature maps to complement the image features, and regress to object properties such as depth, rotation and velocity. We evaluate CenterFusion on the challenging nuScenes dataset, where it improves the overall nuScenes Detection Score (NDS) of the state-of-the-art camera-based algorithm by more than 12%. We further show that CenterFusion significantly improves the velocity estimation accuracy without using any additional temporal information. The code is available at https://github.com/mrnabati/CenterFusi
### **Summary**

Related works
Mv3D extracts features from the front view and Bird's Eye View representation of the Lidar data, in addition to the RGB image.
The features obtained from the LiDAR's BEV are then used to generate 3D object proposals and a deep fusion network is used to combine features from each view and predict the object class and box orientations.

本文的关键是准确对准radar detection和物体，centernet的检测获得了物体的中心，二维图像的周围其他特征用于检测物体的其他属性，但是radar information特征需要映射到相关物体图像的中心上。
先用centernet在二维图像上获得中心和preliminary 3d box， 然后将radar information与原始场景中的物体精确联系起来，然后将radar detection的信息转换成特特征图concatenate到preliminary的heat  map上，并对原来的3d边界框refine，得到最后的结果。
本文的不足是，只能检测到在二维检测中检测到的物体，但是还需要3d的真值和2d的真值，在生成视锥的过程中，需要利用3d truth生成tight roi frustum。
本文是middle fusion的一类，创新点就是，将2d物体检测和radar检测的点associated起来，不过frustum的思想和pillar的思想都是之前有的，因此个人认为，radar information用的少和结果好是本文录用的主要原因。

### **Method**
![](001.png)
**1.center point detection。**

先用centernet在二维图像中做检测，backbone为DLA（Deep layer aggregation），生成2d bounding box和preliminary 3d bounding box

**2.radar fusion**

先利用center point detection中生成的2dbounding box生成一个frustum，然后利用depth信息进一步缩小视锥的大小，由于radar采集到的数据在z方向上不准，因此，将radar point用pillar表示，这样图像中检测到的物体，都会有roi 视锥表示
![](002.png)
**3.radar feature extraction**

得到radar detection结果后，然后利用radar检测的depth 和 vx vy信息作为图像检测中的补充信息，这样就增加3的channel的信息，但是w 和 h的尺寸需要和2d 特征图一致，利用如下公式可以求得。
![](003.png)
![](004.png)


### **Conclusion:**

1.认为本文方法是centernet在3d object detection的一个简单应用。

2.流程的上层部分与原文一致，但本文加入了 depth和radial velocity信息。

3.创新的部分为将2d box detection的结果用于associated object information，然后radar的information正则化成feature map concatenate到2d 网络生成特征图上。

本文用radar信息，对于这一领域，之前尚未有人做过，除此之外，本文的结果对比上，比原有的要好
但本文无法检测2d图像中检测不到的物体，且radar information可以认为是辅助。
