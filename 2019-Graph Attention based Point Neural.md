# Graph Attention based Point Neural Network for Exploiting Local Feature of Point Cloud

*作者：Can Chen*

*时间：2019*

*期刊：Neurocomputing*

*地址：https://github.com/FrankCAN/GAPointNet*

---

## 一、创新点

1、提出GAPLayer来获取语义注意力特征

2、提出自注意力机制使得GAPLayer计算注意力权重

3、提出注意力池化层在局部聚合特征

4、将GAPLayer和注意力池化层嵌入到MLP中，以从无序点云更好地提取无序特征





## 二、具体体现



### 1、GAPLayer

*自注意力机制通过考虑每个点自身的几何信息来学习self-coefficients，邻域注意力机制通过考虑邻域来关注local-coefficient，这样就构成一个 Single-head GAPLayer，将多个 Single-head GAPLayer并联，得到GAPLayer。*

![image-20210321110816820](https://gitee.com/suyunzzz/img/raw/master/img/20210321110816.png)

![image-20210321110846766](https://gitee.com/suyunzzz/img/raw/master/img/20210321110846.png)

### 2、Attention pooling layer

将局部的邻域特征进行聚合（大概就是这个意思，具体还得看[代码](https://github.com/FrankCAN/GAPointNet/blob/9fb9fd4577950b29f996baa5135927e13df45408/part_seg/part_seg_model.py#L38)）

```python
locals_max_transform = tf.reduce_max(tf.concat(local_features, axis=-1), axis=-2, keep_dims=True)
```



![image-20210321111531892](https://gitee.com/suyunzzz/img/raw/master/img/20210321111531.png)

### 3、GAPNet architecture

与PointNet类似的结构，但是有三点不同：

1. 使用了attention-aware空间变换网络，来获取旋转不变性。
2. 使用GAPLayer来获取了丰富的局部特征，而不是处理独立的点
3. 使用attention-layer来聚合局部特征，最终拼接实现全局特征

![image-20210321113022701](https://gitee.com/suyunzzz/img/raw/master/img/20210321113022.png)



## 三、实验



### 1、分类实验

> leads to the fact that our model achieves the best trade-off between accuracy and complexity.

![image-20210321113001266](https://gitee.com/suyunzzz/img/raw/master/img/20210321113001.png)

消融实验

![image-20210321113347544](https://gitee.com/suyunzzz/img/raw/master/img/20210321113347.png)

### 2、部件分割实验

![image-20210321113457927](https://gitee.com/suyunzzz/img/raw/master/img/20210321113457.png)

