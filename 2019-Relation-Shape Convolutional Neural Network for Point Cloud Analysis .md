# Relation-Shape Convolutional Neural Network for Point Cloud Analysis 

*中科院 刘永成*

*CVPR 2019 oral*

代码：https://github.com/Yochengliu/Relation-Shape-CNN

---

## 创新

为了解决局部点云特征聚合的问题，设计一种点云的卷积，从邻域关系上更新中心点的Feature。

> 核心就是通过邻域点与中心点之间的低层次特征来提取高层次特征，最终得到该邻域点相对于中心点的权重，通过聚合所有的邻域点加权特征来更新中心点特征。



## 具体实现

![image-20210309120656065](https://gitee.com/suyunzzz/img/raw/master/img/20210309120656.png)



## 实验

分类任务：

![image-20210309120910771](https://gitee.com/suyunzzz/img/raw/master/img/20210309120910.png)

部件分割任务：

![image-20210309120931491](https://gitee.com/suyunzzz/img/raw/master/img/20210309120931.png)

法向量估计任务：

![image-20210309120951137](https://gitee.com/suyunzzz/img/raw/master/img/20210309120951.png)

消融实验：

![image-20210309121047983](https://gitee.com/suyunzzz/img/raw/master/img/20210309121048.png)



## 代码

```python
class RSConv(nn.Module):
    def __init__(self, in_channel, out_channel, hidden_channel):
        super().__init__()
        self.in_channel = in_channel
        self.out_channel = out_channel
        '''
        Relation convs, 3-layer mlp. 
        The input channel is 10.
        The output channel is the same as the input feature channel
        '''
        mlp_convs = []
        last_channel = 10
        mlp_layers = [hidden_channel,in_channel]
        rasing_layers = [out_channel]
        for i, num_channel in enumerate(mlp_layers):
            mlp_convs.append(nn.Conv2d(in_channels=last_channel, out_channels=num_channel, kernel_size=1))
            if i < len(mlp_layers) - 1:
                mlp_convs.append(nn.ReLU())
                mlp_convs.append(nn.BatchNorm2d(num_channel))
            last_channel = num_channel
        self.mlp_convs = nn.Sequential(*mlp_convs)
        '''
        Channel-raising mapping. Map the input channel to the output channel.
        '''
        channel_raising = []
        last_channel = in_channel
        for num_channel in rasing_layers:
            channel_raising.append(nn.Conv2d(in_channels=last_channel, out_channels=num_channel, kernel_size=1))
            channel_raising.append(nn.ReLU())
            channel_raising.append(nn.BatchNorm2d(num_channel))
            last_channel = num_channel
        self.channel_raising = nn.Sequential(*channel_raising)

    def forward(self, centroid_xyz, neighbors):
        """
        :param centroid_xyz: [B, npoint, C]
        :param neighbors: [B, npoint, nsample, C+D]
        :return:
        """
        C = centroid_xyz.shape[-1]
        nsample = neighbors.shape[2]
        assert C == 3

        '''Calculate relation h'''
        neighbors_xyz = neighbors[:, :, :, :C]  # [B, npoint, nsample, C]
        centroid_xyz = centroid_xyz.unsqueeze(dim=2)  # [B, npoint, 1, C]
        delta = centroid_xyz - neighbors_xyz  # [B, npoint, nsample, C]
        euclidean = torch.sqrt((delta ** 2).sum(dim=-1, keepdim=True))  # [B, npoint, nsample, 1]
        h = torch.cat([euclidean, delta, centroid_xyz.repeat(1, 1, nsample, 1), neighbors_xyz],
                      dim=-1)  # [B, npoint, nsample, 10]
        h = h.permute(0, 3, 1, 2)  # [B, 10, npoint, nsample]

        '''Apply RS-convolution'''
        h = self.mlp_convs(h)
        neighbors = neighbors.permute(0, 3, 1, 2)  # [B, C+D, npoint, nsample]
        aggregated = F.relu(torch.max(neighbors * h, dim=-1, keepdim=True)[0])  # [B, C+D, npoint, 1]

        '''Channel raising'''
        raised = self.channel_raising(aggregated) # [B, out_channel, npoint, 1]
        raised = raised.squeeze(dim=-1)  # [B, out_channel, npoint]
        return raised
```

