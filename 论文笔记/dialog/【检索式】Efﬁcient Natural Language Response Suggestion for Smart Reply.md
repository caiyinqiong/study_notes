> > 2017

# 背景
<img src="../../images/image-20200616153450662.png" alt="image-20200616153450662" style="zoom:33%;" />

总体系统架构如上图，本文主要关注实现高效的 response selection。

本文的方法使用基于n-gram的前向神经网络来建模文本的序列信息（虽然RNN和CNN也可以实现，但效率不够高）。



# 模型

- 基础模型

  <img src="../../images/image-20200616153728956.png" alt="image-20200616153728956" style="zoom:50%;" />

  <img src="../../images/image-20200616154016406.png" alt="image-20200616154016406" style="zoom:50%;" />

- 考虑email x的多个特征：使用一个两层次的模型架构

  <img src="../../images/image-20200616154050253.png" alt="image-20200616154050253" style="zoom:33%;" />

  <img src="../../images/image-20200616154229768.png" alt="image-20200616154229768" style="zoom:50%;" />

- 快速搜索算法（使用quantization技术提高效率）

  <img src="../../images/image-20200616154426016.png" alt="image-20200616154426016" style="zoom:50%;" />

  <img src="../../images/image-20200616154508190.png" alt="image-20200616154508190" style="zoom:50%;" />

  <img src="../../images/image-20200616154528838.png" alt="image-20200616154528838" style="zoom:33%;" />

- 训练

  <img src="../../images/image-20200616153756305.png" alt="image-20200616153756305" style="zoom:50%;" />

  使用K个样本（包括一个正例和K-1个负例）进行近似计算：

  <img src="../../images/image-20200616153822133.png" alt="image-20200616153822133" style="zoom:50%;" />

  <img src="../../images/image-20200616153833090.png" alt="image-20200616153833090" style="zoom:50%;" />

  负例采样策略：使用同batch内其他样本对的reply作为负例。

  解决负例采样带来的偏差：（因为负例中会有较多短的一般性的回复，导致这类回复会受到抑制）

  ![image-20200616154355961](../../images/image-20200616154355961.png)



# 实验

##### 数据集

300M个样本对

##### 对比模型（Joint model）

<img src="../../images/image-20200616161735007.png" alt="image-20200616161735007" style="zoom:33%;" /><img src="../../images/image-20200616161758468.png" alt="image-20200616161758468" style="zoom:33%;" />

##### 实验结果

- 离线评估

  ![image-20200616161557451](../../images/image-20200616161557451.png)

- 在线评估

  ![image-20200616161700027](../../images/image-20200616161700027.png)



# 结论