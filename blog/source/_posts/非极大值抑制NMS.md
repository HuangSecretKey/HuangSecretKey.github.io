---
title: 非极大值抑制NMS
date: 2021-01-12 20:03:35
tags: -深度学习
categories: -深度学习tricks
---

非极大值抑制是筛选出一定区域内属于同一种类得分最大的框

<!--more-->
非极大抑制的执行过程如下所示：
1、对所有图片进行循环。
2、找出该图片中得分大于门限函数的框。在进行重合框筛选前就进行得分的筛选可以大幅度减少框的数量。
3、判断第2步中获得的框的种类与得分。取出预测结果中框的位置与之进行堆叠。此时最后一维度里面的内容由5+num_classes变成了4+1+2，四个参数代表框的位置，一个参数代表预测框是否包含物体，两个参数分别代表种类的置信度与种类。
4、对种类进行循环，非极大抑制的作用是筛选出一定区域内属于同一种类得分最大的框，对种类进行循环可以帮助我们对每一个类分别进行非极大抑制。
5、根据得分对该种类进行从大到小排序。
6、每次取出得分最大的框，计算其与其它所有预测框的重合程度，重合程度过大的则剔除。

 先上nms的代码
 ```
def non_max_supression(boxes, num_classes, conf_thres=0.5, nms_thres=0.4):
    bs = np.shape(boxes)[0]
    #------------------------------------#
    # boxes的维度为（batch_size, all_boxes, 4+1+num_classes）
    # boxes的维度的代表分别是，图像的数量，图像中的所有框，框的位置，置信度和属于哪个物体的概率
    # 将框转换为左上角右下角的形式
    # 
    shape_boxes = np.zeros_like(boxes[:,:,:4])
    shape_boxes[:,:,0] = shape_boxes[:,:,0] - shape_boxes[:,:,2]/2
    shape_boxes[:,:,1] = shape_boxes[:,:,1] - shape_boxes[:,:,3]/2
    shape_boxes[:,:,2] = shape_boxes[:,:,2] + shape_boxes[:,:,2]/2
    shape_boxes[:,:,3] = shape_boxes[:,:,3] + shape_boxes[:,:,3]/2

    boxes[:,:,:4] = shape_boxes

    output = []

    for i in range(bs):
        # 对batch_size中的每张图片分别进行操作
        prediction = boxes[i]
        # 选取置信度大于设定阈值的预测框
        score = prediction[:,4]
        mask = score > conf_thres
        detections = prediction[mask]
        # 由于存在多个物体的预测概率，因此在每个框中，选择概率最大的那个，同时保留下类别
        class_conf = np.expand_dims(np.max(detections[:,5:],axis=-1), axis=-1)
        class_pred = np.expand_dims(np.argmax(detections[:,5:],axis=-1),axis=-1)
        # 将筛选后的物体的类别概率和类别种类与原始的框的位置在concatenate到一起
        detections = np.concatenate([boxes[:,:5], class_conf, class_pred],-1)
        # 如果有多个框同时预测为同一物体，则只取一个
        unique_class = np.unique(detections[:,-1])
        if len(unique_class)  == 0:
            continue
        best_box = []
        # 此时，只有不同种类的物体，但是不同的物体上应该有很多的框框，只需要计算框框之间的iou， 如果
        # 如果iou较大的话，说明此时与目前置信度最高的框框是重合的，需要舍去，否则就留下
        # 遍历所有的框框，可以完成非极大值的抑制。
        for c in unique_class:
            cls_mask = detections[:,-1] == c
            detection = detections[cls_mask]
            
            score = detection[:,4]

            arg_sort = np.argsort(score)[::-1]

            detection = detection[arg_sort]

            while len(detection) != 0:
                best_box.append(detection[0])
                if len(detection) == 1:
                    break
                ious = iou(best_box[-1], detection[1:])
                detection = detection[1:][ious<nms_thres]
        output.append(best_box)

        return np.array(output)
```
下面是求交并比iou的代码
```
def iou(b1, b2):
    # 同时b1框框与其他的框框的交并比
    b1_x1, b1_y1, b1_x2, b1_y2 = b1[0], b1[1], b1[2], b1[3]
    b2_x1, b2_y1, b2_x2, b2_y2 = b2[:,0], b2[:,1], b2[:,2], b2[:,3]
    #----------------------------------------------------------#
    # 计算左上角交点的最大值和右下角点的最小值，注意是坐标值！！！
    # 此时即可覆盖所有的情况
    # ---------------------------------------------------------#
    inter_rect_x1 = np.maximum(b1_x1, b2_x1)
    inter_rect_y1 = np.maximum(b1_y1, b2_y1)
    inter_rect_x2 = np.minimum(b1_x2, b2_x2)
    inter_rect_y2 = np.minimum(b1_y2, b2_y2)
    # 注意在图像坐标系中，坐标原点在左上角，因此，只需要保留左上角的最大值和右下角的最小值就可以得到交集
    inter_area = np.maximum(inter_rect_x2-inter_rect_x1, 0) *  np.maximum(inter_rect_y2 - inter_rect_y1, 0)

    area_b1 = (b1_x2 - b2_x1) * (b1_y2 - b1_y1)
    area_b2 = (b2_x2 - b2_x1) * (b2_y2 - b2_y1)

    iou = inter_area / np.maximum((area_b1+area_b2-inter_area), 1e-6)

    return iou
```
