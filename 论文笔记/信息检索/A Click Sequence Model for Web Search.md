> > SIGIR2018，信息检索，点击模型

<center>A Click Sequence Model for Web Search</center>

## 背景

在信息检索中，之前的研究大多集中在对<u>单次交互行为</u>的建模和预测。

从直观上，通常认为用户是按照搜索引擎返回结果的顺序从上到下进行点击的，但实际上并不成立。



本文是首次提出建模和预测<u>交互序列</u>（点击序列）。

本文提出了$\color{red}{CSM模型（click\ sequence\ model）}$，用来预测用户对于搜索引擎返回结果的交互序列。



## 模型

> 基本架构：【encoder-decoder + attention】
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108145552170.png" alt="image-20191108145552170" style="zoom:33%;" />
>
> 1. encoder
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108145626090.png" alt="image-20191108145626090" style="zoom:50%;" />
>
> 2. decoder（通过beam search得到最可能的K个点击序列）
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108145701782.png" alt="image-20191108145701782" style="zoom:50%;" />
>
> 目标函数：最大化观测点击序列的似然。

## 实验

1. 数据集：Yandex Relevance Prediction dataset
2. 评估任务
   - 预测点击序列
   - 预测点击次数
   - 预测用户是否按照返回的顺序进行点击