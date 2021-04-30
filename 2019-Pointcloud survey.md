

# Pointcloud Survey



![image-20210326213349586](https://gitee.com/suyunzzz/img/raw/master/img/20210326213349.png)



![image-20210326214022598](https://gitee.com/suyunzzz/img/raw/master/img/20210326214022.png)



## Projection-based 

### Multi-view Representation

### Spherical Representation



### Volumetric Representation



### Permutohedral Lattice Representation





### Hybrid Representation

## Point-based



### Pointwise MLP Methods

>To capture wider context for each point and learn richer local structures,several dedicated networks have been introduced, including methods based on neighboring feature pooling, attention-based aggregation, and local-global feature concatenation.

#### Neighboring feature pooling

PointNet2

PointSIFT

[Know what your neighbors do: 3D semantic segmentation of point clouds]

>[169] utilized K-means clustering and KNN to separately define two neighborhoods in the world space and learned feature space. Based on the assumption that points from the same class are expected to be closer in feature space, a pairwise distance loss and a centroid loss are introduced to further regularize feature learning

PointWeb

> 通过密集构建局部一个局部全连接网络来探寻局部区域内所有点对之间的关系；
>
> 提出Adaptive Feature Adjustment模块，以实现信息交互和特征优化；

Shellconv

> 首先查询一系列的多尺度同心球，然后maxpooling对不同的shells进行特征聚合，MLP和1D卷积被用来获取最终的输出

RandLA-Net

> 使用随机采样在内存和计算效率上取得了卓越的结果；
>
> 局部特征聚合模块用来捕获、保存几何特征

#### Attention-based aggregation

[Modeling point clouds with self-attention and gumbel subset sampling]

> 为了刻画点之间的关系，提出group shuffle attention
>
> 提出了置换不变性、task-agnostic、可微分的Gumbel Subset Sampling (GSS) ，来替换FPS，该模块对噪声不敏感且可以选择点云中具有代表性的集合

[LSANet: Feature learning on point sets by local spatial attention]

> 提出Local Spatial Aware（LSA）模块基于空间布局和局部结构来学习空间注意力权重

[Pooling scores of neighboring points for improved 3D point cloud segmentation]

> 提出Attention-based Score Refinement（ASR）模块处理网络得到的分割结果
>
> 该模块可以集成在现存的网络以提高分割结果



#### Local-global concatenation

[Y. Zhao, T. Birdal, H. Deng, and F. Tombari, “3D point capsule networks,” in CVPR, 2019.]

> 提出置换不变性的网络结合局部结构和全局上下文

[Y. Wang, Y. Sun, Z. Liu, S. E. Sarma, M. M. Bronstein, and J. M. Solomon, “Dynamic graph CNN for learning on point clouds,” ACM TOG, 2019.]、[R. Arandjelovic, P. Gronat, A. Torii, T. Pajdla, and J. Sivic, “NetVLAD: CNN architecture for weakly supervised place recognition,” in CVPR, 2016, pp. 5297–5307.]

> 重复地捕获局部信息和场景级别的全局特征

### Point Convolution Methods

[B.-S. Hua, M.-K. Tran, and S.-K. Yeung, “Pointwise convolutional neural networks,” in CVPR, 2018.]

> point-wise convolution operator, where the neighboring points are binned into kernel cells and then convolved with kernel weights

[S. Wang, S. Suo, W.-C. Ma, A. Pokrovsky, and R. Urtasun, “Deep parametric continuous convolutional neural networks,” in CVPR, 2018, pp. 2589–2597.]

> proposed a network called PCCN based on parametric continuous convolution layers. The kernel function of this layer is parameterized by MLPs and spans the continuous vector space.

[H. Thomas, C. R. Qi, J.-E. Deschaud, B. Marcotegui, F. Goulette, and L. J. Guibas, “KPConv: Flexible and deformable convolution for point clouds,” arXiv preprint arXiv:1904.08889, 2019.]

> proposed a Kernel Point Fully Convolutional Network (KP-FCNN) based on Kernel Point Convolution (KPConv). Specifically, the convolution weights of KPConv are determined by the Euclidean distances to kernel points, and the number of kernel points is not fixed. The positions of the kernel points are for-mulated as an optimization problem of best coverage in a sphere space. Note that the radius neighbourhood is used to keep a consistent receptive field, while grid subsampling is used in each layer to achieve high robustness under varying densities of point clouds

[F. Engelmann, T. Kontogianni, and B. Leibe, “Dilated point convolutions: On the receptive field of point convolutions,” arXiv preprint arXiv:1907.12046, 2019.]

> 做了丰富的消融实验和可视化结果来表明感受野的大小在基于聚合的方法上的表现性能
>
> 提出Dilated Point Convolution (DPC)以聚合dilated neighbouring features，而不是KNN
>
> 该模块可以集成到现存的基于聚合的网络上



### RNN-based Methods



### Graph-based Methods

> To capture the underlying shapes and geometric structures of 3D point clouds, several methods resort to graph networks

[L. Landrieu and M. Simonovsky, “Large-scale point cloud semantic segmentation with superpoint graphs,” in CVPR, 2018, pp. 4558–4567.]

>将点云转换为一系列相互联系的简单形状的超点，并使用属性有向图（i.e.,superpoint graph）以捕获结构和上下文信息。这样大尺度的点云分割问题就变成了三个子问题，i.e.,geometrically homogeneous partition、superpoint embedding、contextual segmentation。

[L. Landrieu and M. Boussaha, “Point cloud oversegmentation with graph-structured deep metric learning,” arXiv preprint arXiv:1904.02113, 2019.]

> 为了进一步提高partition分割步骤，提出了将点云分割为超点的框架。
>
>  This problem is formulated as a deep metric learning problem structured by an adjacency graph
>
> In addition, a graph-structured contrastive loss is also proposed to help the recognition of borders between objects.

[K. Zhiheng and L. Ning, “PyramNet: Point cloud pyramid attention network and graph embedding module for classification and segmentation,” arXiv preprint arXiv:1906.03299, 2019. ]

> 为了在高维空间中更好地捕获局部几何关系，基于Graph Embedding Module（GEM）和Pyramid Attention Network（PAN）的PyramNet被提出
>
> GEM将点云看作directed acyclic graph并且利用协方差矩阵替换欧氏距离作为相识度矩阵的构成。
>
> PAN Module 使用4个不同尺度的卷积核提取不同语义密度的特征

[L. Wang, Y. Huang, Y. Hou, S. Zhang, and J. Shan, “Graph attention convolution for point cloud semantic segmentation,” in CVPR, 2019, pp. 10 296–10 305.]

> Graph Attention Convolution（GAC）在局部邻域内有选择地学习特征
>
> This operation is achieved by dynamically assigning attention weights to different neighbouring points and feature channels based on their spatial positions and feature differences.
>
> 根据空间位置和特征差异对邻域点中不同的点赋予不同的权重









