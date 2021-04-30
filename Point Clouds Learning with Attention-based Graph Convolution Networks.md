# Point Clouds Learning with Attention-based Graph Convolution Networks

*期刊：NEUROCOMPUTING*

*年份：2020*

*作者：Xie, Zhuyang*

---

## 一、创新

1、引入注意力机制来分析点的局部特征之间的关系

2、引入全局图结构网络来补偿独立点之间的相对信息

## 二、具体体现

![image-20210322203258967](https://gitee.com/suyunzzz/img/raw/master/img/20210322203259.png)

### 1、Learning Local Structural feature

> sampleing，grouping，mlp，max

![image-20210322202534289](https://gitee.com/suyunzzz/img/raw/master/img/20210322202534.png)

### 2、Point Attention layer

> as the network depth increases, the receptive field of the network increases correspondingly, so that from local to global information can be learned.

![image-20210322203315182](https://gitee.com/suyunzzz/img/raw/master/img/20210322203315.png)



![image-20210322203338678](https://gitee.com/suyunzzz/img/raw/master/img/20210322203338.png)



其中，attention-score计算如下：

![image-20210322203421174](https://gitee.com/suyunzzz/img/raw/master/img/20210322203421.png)

### 3、Global Point Graph

> The experimental result shows that the global point graph can further combine the local information with the global information

![image-20210322204629495](https://gitee.com/suyunzzz/img/raw/master/img/20210322204629.png)

### 4、Attention-based Encoder-Decoder for Segmentation

![image-20210322214445653](https://gitee.com/suyunzzz/img/raw/master/img/20210322214445.png)

## 三、实验

### S3DIS

![image-20210322220752595](https://gitee.com/suyunzzz/img/raw/master/img/20210322220752.png)

![image-20210322220818873](https://gitee.com/suyunzzz/img/raw/master/img/20210322220819.png)