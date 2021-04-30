# PointConv



## 1、创新

1、将连续卷积扩展到三维点云，对中心点的邻域内点云进行卷积

2、提出高效的PointConv执行方式（没看懂）

3、将卷积推广到反卷积（和PointNet2类似）



## 2、具体体现

### 1、理论推导

将连续的卷积核看作是关于一个3D中心点邻域坐标的连续函数

![image-20210119213614271](https://gitee.com/suyunzzz/img/raw/master/img/20210119213614.png)

其中，W和F均为连续函数，(x,y,z)是参考点的坐标， (δx,δy,δz)表示邻域 G 中的 3D 点的相对坐标，F代表特征，W代表学习到的权重矩阵（也就是卷积核）。

为了针对不均匀的问题，增加一个逆密度矩阵S：

![image-20210119214000310](https://gitee.com/suyunzzz/img/raw/master/img/20210119214000.png)

![image-20210119214248037](https://gitee.com/suyunzzz/img/raw/master/img/20210119214248.png)

### 2、卷积操作

- 初始版本

![image-20210119214416004](https://gitee.com/suyunzzz/img/raw/master/img/20210119214416.png)



- 高效卷积

![image-20210119214501156](https://gitee.com/suyunzzz/img/raw/master/img/20210119214501.png)



### 3、反卷积

![image-20210119214450538](https://gitee.com/suyunzzz/img/raw/master/img/20210119214450.png)

## 3、实验

![image-20210119214536344](https://gitee.com/suyunzzz/img/raw/master/img/20210119214536.png)

![image-20210119214548359](https://gitee.com/suyunzzz/img/raw/master/img/20210119214548.png)

![image-20210119214605379](https://gitee.com/suyunzzz/img/raw/master/img/20210119214605.png)



![image-20210119214612208](https://gitee.com/suyunzzz/img/raw/master/img/20210119214612.png)

