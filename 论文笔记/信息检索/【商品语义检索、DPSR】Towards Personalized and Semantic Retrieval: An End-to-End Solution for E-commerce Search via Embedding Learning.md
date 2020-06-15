> > SIGIR2020，京东

# 背景

搜索系统的三个模块：

<img src="../../images/image-20200613144630967.png" alt="image-20200613144630967" style="zoom:33%;" />

本文的关注点在于candidate retrieval部分，该部分存在的挑战在于：

1）语义检索（query和item之间存在词汇鸿沟，传统的检索都通过query重新来解决这个问题）；

2）个性化检索（已有的解决方案是基于规则的方法）



# 模型

![image-20200613145039753](../../images/image-20200613145039753.png)

##### 训练

<img src="../../images/image-20200613145315923.png" alt="image-20200613145315923" style="zoom:50%;" />

<img src="../../images/image-20200613145338942.png" alt="image-20200613145338942" style="zoom:50%;" />

##### 在线检索

<img src="../../images/image-20200613145403950.png" alt="image-20200613145403950" style="zoom:50%;" />

##### 负采样策略

两种采样策略的混合：batch内随机采样；batch内其他query的正例

混合超参数$\alpha$越大，负样本中随机采样的样本越多。$\alpha$越小，负样本中的样本更接近商品的真实的流行度（排序的时候就会降低流行商品的排序）。

<img src="../../images/image-20200613154205269.png" alt="image-20200613154205269" style="zoom:50%;" />



# 实验

实验结果

- 学到的item embedding的情况

<img src="../../images/image-20200613154534352.png" alt="image-20200613154534352" style="zoom:33%;" />

- 使用多头注意力的结果

  ![image-20200613154702112](../../images/image-20200613154702112.png)

  可以更好地解决多义query。

- 离线测试

  <img src="../../images/image-20200613154743241.png" alt="image-20200613154743241" style="zoom:40%;" />

  <img src="../../images/image-20200613154826301.png" alt="image-20200613154826301" style="zoom:50%;" />

- 在线A/B测试

  <img src="../../images/image-20200613154906752.png" alt="image-20200613154906752" style="zoom:50%;" />

  <img src="../../images/image-20200613154932730.png" alt="image-20200613154932730" style="zoom:50%;" />

- 效率

  <img src="../../images/image-20200613155006527.png" alt="image-20200613155006527" style="zoom:50%;" />

  <img src="../../images/image-20200613155024411.png" alt="image-20200613155024411" style="zoom:50%;" />



# 结论

- 通过对query tower使用multi-head来建模多个query向量表示，解决多义query问题，从而更好地理解用户意图。