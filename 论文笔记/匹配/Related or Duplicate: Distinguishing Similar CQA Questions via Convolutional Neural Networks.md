> > SIGIR2018，CQA重复问题判断

<center>Related or Duplicate: Distinguishing Similar CQA ￿estions via Convolutional Neural Networks</center>

## 背景

之前一般使用人工提取的特征进行CQA中的重复问题检测，但一般难以区分<u>相关问题</u>和<u>重复问题</u>。



CQA中重复问题检测主要由两个挑战：

- 相关的问题通常有相似的用词，很容易被误识别成重复问题
- 重复问题有些是在语法和词汇上不相同的，容易被误判为非重复。

本文主要解决的是第一种挑战。



## 方法

1. **构造question-pair的相关性矩阵**

   输入时预训练的word embedding，一共构造6种相关性矩阵：

   - cosine distance
   - euclidean distance
   - correlation distance（$\color{red}{？？？}$）
   - mutual information
   - Indicator（两个词相同则为1，否则为0）
   - Dot Production

2. **多层CNN**

   普通的在单个文本上进行CNN，就只能进行1维的CNN，浪费了CNN的能力。

   本文使用10层【CNN + ReLU + maxpooling】的堆叠。

3. **分类器**

   输入：flatten最后一层CNN的输出

   分类器：FC + activation function + FC + Sigmoid（本任务是二分类，0.5作为判断类别的阈值）

   损失函数：

   > y_i是数据i的标签
   >
   > <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191104235538918.png" alt="image-20191104235538918" style="zoom:70%;" />



##实验

1. 数据集：Stack Exchange Data



## 想法

- $\color{red}{可以使用 answer 信息帮助判断问题的重复性！！！}$





