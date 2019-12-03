> >SIGIR2018，排序模型

<center>Learning a Deep Listwise Context Model for Ranking Refinement</center>

## 背景

在信息检索任务中，通常是学习一个global ranking function。。但是，不同query的相关文档在特征空间可能有不同的分布。。

本文提出了**DLCM（Deep Listwise Context Model）模型**，借助query的 <u>local ranking context信息</u>（top-n相关文档）来改进 query 的 ranking list，即对top-n文档进行重排序。



## 模型

> 整体框架：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115220457674.png" alt="image-20191115220457674" style="zoom:50%;" />
>
> 1. nitial retrieval
>
>    - document representation = concat [z(0)；z(2)]
>
>      <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115220711202.png" alt="image-20191115220711202" style="zoom:50%;" />
>
>    - 普通的learn2rank算法，检索出top-n相关文档
>
> 2. Encoding Listwise local context
>
>    使用GRU从文档n到文档1进行编码，GRU的最后一个状态作为S_n。
>
> 3. Re-ranking with local context
>
>    拿GRU对每个文档的编码及向量S_n，对文档进行重排序：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115221010354.png" alt="image-20191115221010354" style="zoom:50%;" />

> 训练：
>
> 本文使用了三种loss函数（新提出了attention rank，相比另两种loss函数速度更快）
>
> - ListMLE
>
> - SoftRank
>
> - Attention Rank
>
>   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115221407629.png" alt="image-20191115221407629" style="zoom:50%;" />
>
>   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115221432075.png" alt="image-20191115221432075" style="zoom:50%;" />
>
>   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115221449124.png" alt="image-20191115221449124" style="zoom:70%;" />



## 实验

1. 数据集

   Microsoft30k、Microsoft10k、Yahoo！Webscope v2.0

2. 实验结果

   使用 SVMRank 和 LambdaMART 作为初始检索方法。

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115221702928.png" alt="image-20191115221702928" style="zoom:50%;" />

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115221739434.png" alt="image-20191115221739434" style="zoom:50%;" />

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115221805806.png" alt="image-20191115221805806" style="zoom:50%;" />

   