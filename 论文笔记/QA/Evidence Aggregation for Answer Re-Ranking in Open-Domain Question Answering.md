> > ICLR2019，开放域QA，answer rerank 

<center>Evidence Aggregation for Answer Re-Ranking in Open-Domain Question Answering</center>

## 背景

目前对开放域QA的研究范式是：

- 先用IR模型粗略地选取相关段落
- 再用RC模块从每个候选段落中推断出答案。



本文提出了2个answer rerank方法，来显式结合多段落信息进行答案生成。

利用多段落信息进行答案生成的理论依据（假设）是：1）多段落可以起到协同增强的作用；2）多段落可以起到信息互补的作用，他们可能覆盖question的不同部分。



本文模型的基本流程是：

- 使用一个标准的RC/QA模型，生成top-k候选答案（本文使用的是R^3模型）
- 使用本文提出的方法，结合多段落信息进行answer rerank



## 模型

#### 整体模型

<img src="../../../notes/images/image-20191205204502785.png" alt="image-20191205204502785" style="zoom:67%;" />

#### 方法1：strength-based rerank（基于协同增强假设）

- 统计每个answer在top-k候选答案中的出现次数
- 加和每个answer在top-k候选答案计算结果的置信度



#### 方法2：coverage-based rerank（基于信息互补假设）

- passage aggregation

  <img src="../../../notes/images/image-20191205204918216.png" alt="image-20191205204918216" style="zoom:50%;" />

- measuring aspect-level matching

  <img src="../../../notes/images/image-20191205204839087.png" alt="image-20191205204839087" style="zoom:40%;" />

  <img src="../../../notes/images/image-20191205205031061.png" alt="image-20191205205031061" style="zoom:40%;" />

  <img src="../../../notes/images/image-20191205205108454.png" alt="image-20191205205108454" style="zoom:40%;" />

  <img src="../../../notes/images/image-20191205205208586.png" alt="image-20191205205208586" style="zoom:33%;" />

- Measure the entire question matching

  <img src="../../../notes/images/image-20191205205321984.png" alt="image-20191205205321984" style="zoom:50%;" />

- re-rank object function

  <img src="../../../notes/images/image-20191205205411134.png" alt="image-20191205205411134" style="zoom:50%;" />

- 目标函数

  <img src="../../../notes/images/image-20191205205452090.png" alt="image-20191205205452090" style="zoom:50%;" />

#### 方法3：融合方法1和2

![image-20191205205657476](../../../notes/images/image-20191205205657476.png)



## 实验

源码：https://github.com/shuohangwang/mprc

数据集：Quasar-T、SearchQA、TriviaQA

结果：

<img src="../../../notes/images/image-20191205205752183.png" alt="image-20191205205752183" style="zoom:33%;" />



