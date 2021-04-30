# PASS3D: Precise and Accelerated Semantic Segmentation for 3D PointCloud

*Xin Kong1, Guangyao Zhai1, Baoquan Zhong1 and Yong Liu1;2*

*2019 IEEE/RSJ International Conference on Intelligent Robots and Systems （IROS）*

---



*截止到2020-9-16，代码还未开源，视频演示https://www.youtube.com/watch?v=cukEqDuP_Qw.*

---



## 一、Abstract

1、将传统的地面分割、聚类算法与深度学习方法的鲁棒性结合起来

2、提出了一个数据增强的方法，以增强网络对所有类别尤其是非刚物体的识别能力

![image-20200916214600158](https://gitee.com/suyunzzz/img/raw/master/img/20200916214600.png)

## 二、PASS3D FRAMEWORK

![image-20200916214621315](https://gitee.com/suyunzzz/img/raw/master/img/20200916214621.png)



### A. Stage-1: Accelerated cluster proposal

1、地面拟合

2、基于rings的聚类

3、根据cluster的点的数量和包围盒大小，对cluster进行过滤。为了弥补点云越远越稀疏的问题，引入了自适应点数阈值

![image-20200916204000874](https://gitee.com/suyunzzz/img/raw/master/img/20200916204000.png)

因为在地面分割时候会将地物分为地面，所以作者扩大包围盒并且将更多的点合并到refined proposals。

![image-20200916214538087](https://gitee.com/suyunzzz/img/raw/master/img/20200916214538.png)



### B. Stage-2: Point-wise semantic segmentation

#### 1) Data preparation:

##### Coordinate transformation

cluster分布在各个位置，神经网络很难收敛，考虑到这个问题，作者对每一个cluster的包围盒的底部任意顶点作为局部坐标系的原点，这样就把包围盒放在了八个卦限中的一个卦限，这个操作不会影响精度，使神经网络更多的考虑点的相对位置，而不是绝对位置。

![image-20200916205557903](https://gitee.com/suyunzzz/img/raw/master/img/20200916205557.png)



##### Data augmentation

对于在局部坐标系中的数据不均衡问题，作者举了一个栗子:

>For example, cars in training samples with heading directions
>along the local x-axis are more than ones along the local
>y-axis, which should not affect the neural network.

为了解决这个问题，作者提出了他的方法--直接处理proposal而不是增强整个场景:

>we propose a data augmentation method dealing with our proposals, which is more efficient and targeted.

具体的方法就是对一个cluster在其局部坐标系进行旋转和镜像对称：

![image-20200916210503632](https://gitee.com/suyunzzz/img/raw/master/img/20200916210503.png)

通过使用这种方法，我们丰富了非重复性和不对称的数据。

#### 2) Learning-based semantic segmentation:

作者使用的backbone是PointNet2，可以灵活替换，直接对refined cluster进行处理，如果cluster的NUM>采样点N(比如1024等)，那么随机采样到N，如果NUM<采样点N，那么随机重复到N。

考虑到cluster的NUM会影响神经网络的分类处理，作者定义了一个特征：

![image-20200916211307606](https://gitee.com/suyunzzz/img/raw/master/img/20200916211307.png)

以进行补偿。

最终输入到网络的特征向量就是：

![image-20200916211507502](https://gitee.com/suyunzzz/img/raw/master/img/20200916211507.png)

*其中xyz代表局部坐标系中的坐标，i代表归一化的反射强度，n代表cluster中的点数量。*

## 三、EXPERIMENTS

1、作者使用了focal-loss，但是好像没有什么提升。

2、为什么Car的performance比较差？作者认为：

>One of the possible reasons why  the semantic segmentation performance of  'Car'  categories is worse than others is that our stage-1 clusters the adjacent cars and trees into one proposal, resulting in a missed detection.

3、Evaluation metrics：

>iou、recall、precision

4、作者是这样评估stage-1的performance的：

>evaluated by calculating the recall of point-wise foreground label.
>
>We put ‘Car’, ‘Pedestrian’ and ‘Cyclist’ classes as foreground while other classes as background.
>
>With only proposing about 30 clusters per frame, our method achieves 89.5% point-wise recall in 5ms.

5、作者通过减少传入到stage-2的点数，提升了整体速度：

>We only pass about points in the entire scene to 5k the stage-2 neural network,while other methods [11], [20] usually pass all points (~30k) into the network.

6、与SqueezeSeg对比

![image-20200916214701546](https://gitee.com/suyunzzz/img/raw/master/img/20200916214701.png)

## 四、CONCLUSION

提出一种**两阶段**的语义分割方法，**将传统几何方法的高效和深度学习方法的鲁棒性结合起来**；提出一种数据增强方法，充分利用点云的优势，对非刚点云(行人、骑车的人)的语义分割表现得到了amazing promotion。