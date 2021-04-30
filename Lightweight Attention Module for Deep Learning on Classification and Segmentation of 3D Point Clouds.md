# Lightweight Attention Module for Deep Learning on Classification and Segmentation of 3D Point Clouds

*Yunhao Cui, Yi An, Member, IEEE, Wei Sun, Huosheng Hu, Senior Member, IEEE, and Xueguan Song*

年份：2020

期刊：IEEE TRANSACTIONS ON INSTRUMENTATION AND MEASUREMENT

IF：3.658



## 1、创新

1、将**深度可分离卷积**引入到PointNet等网络中，实现了轻量级+高效，Lightweight Module 。

2、为了弥补深度可分离卷积导致的精度下降的问题，引入了**channel attention模块**，提高了精度，Attention Module。

3、将两个模块（LM+AM）结合起来，使轻量级和高精度之间达到了平衡。

## 2、具体体现

### 1、深度可分离卷积

深度可分离卷积可以参考：[这篇](文档：深度可分离卷积.note
链接：http://note.youdao.com/noteshare?id=6ceae305e1d737aa2e9b62595e37c2ca&sub=CD32A9E2998E4E91BC725F195E6B4015)



![image-20210119153122238](https://gitee.com/suyunzzz/img/raw/master/img/20210119153122.png)

![image-20210119152838390](https://gitee.com/suyunzzz/img/raw/master/img/20210119152838.png)

![image-20210119152852203](https://gitee.com/suyunzzz/img/raw/master/img/20210119152852.png)

常规卷积的参数量：

![image-20210119153324394](https://gitee.com/suyunzzz/img/raw/master/img/20210119153324.png)

深度可分离卷积的参数量：

![image-20210119153348167](https://gitee.com/suyunzzz/img/raw/master/img/20210119153348.png)

### 2、channel attention模块

![image-20210119153207435](https://gitee.com/suyunzzz/img/raw/master/img/20210119153207.png)

M代表每一个通道上特征的均值：

![image-20210119153515767](https://gitee.com/suyunzzz/img/raw/master/img/20210119153515.png)

D代表每一个通道上特征的分布标准差：

![image-20210119153545265](https://gitee.com/suyunzzz/img/raw/master/img/20210119153545.png)

每一个通道上的"权重"：

![image-20210119153840521](https://gitee.com/suyunzzz/img/raw/master/img/20210119153840.png)

最终得到注意力权重：

![image-20210119153950460](https://gitee.com/suyunzzz/img/raw/master/img/20210119153950.png)

根据权重更新通道的特征：

![image-20210119154038147](https://gitee.com/suyunzzz/img/raw/master/img/20210119154038.png)



借助于PointNet的结构，将其中的mlp替换为本文提出的LAM：

![image-20210119154139100](https://gitee.com/suyunzzz/img/raw/master/img/20210119154139.png)

## 3、实验结果





![image-20210119154410066](https://gitee.com/suyunzzz/img/raw/master/img/20210119154410.png)

![image-20210119154423050](https://gitee.com/suyunzzz/img/raw/master/img/20210119154423.png)

![image-20210119154450767](https://gitee.com/suyunzzz/img/raw/master/img/20210119154450.png)

![image-20210119154502111](https://gitee.com/suyunzzz/img/raw/master/img/20210119154502.png)

![image-20210119154515597](https://gitee.com/suyunzzz/img/raw/master/img/20210119154515.png)

![image-20210119154541566](https://gitee.com/suyunzzz/img/raw/master/img/20210119154541.png)