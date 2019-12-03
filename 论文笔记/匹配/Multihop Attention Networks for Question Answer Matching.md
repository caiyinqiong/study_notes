> > SIGIR2018，QA answer selection，多跳匹配

<center>Multihop Attention Networks for Question Answer Matching</center>

## 背景

之前对answer selection task的做法是，把question表达成一个向量，然后使用attention机制与候选答案进行匹配（例如，融合attention的QA-LSTM模型）。。但通过单个向量表达，无法捕捉question和answer之间可能存在的复杂关系。



本文提出了**MAN（Multihop Attention Networks）模型**：

1. 不是把question压缩成单个向量，而是使用多个向量，分别关注question的不同部分。
2. 使用多步attention学习候选答案的表达。（每步使用co-attention + sequence-attention）



## 不同的Attention总结

<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115180907697.png" alt="image-20191115180907697" style="zoom:50%;" />

1. MLP attention

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115180946763.png" alt="image-20191115180946763" style="zoom:50%;" />

2. Bilinear Attention

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115181021198.png" alt="image-20191115181021198" style="zoom:50%;" />

3. Sequential Attention

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115181049070.png" alt="image-20191115181049070" style="zoom:50%;" />

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115181124120.png" alt="image-20191115181124120" style="zoom:50%;" />

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115181143977.png" alt="image-20191115181143977" style="zoom:50%;" />

4. self Attention

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115181220710.png" alt="image-20191115181220710" style="zoom:50%;" />



## 模型

> 整体框架：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115181332276.png" alt="image-20191115181332276" style="zoom:50%;" />
>
> 1. 使用预训练的词向量，question和 condidate answer 分别通过共享的BiLSTM。
>
> 2. 计算每一个迭代步的question representation（实验结果显示，2步较好，可能因为question较短），每次迭代得到的question向量表达关注了question的不同部分。
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115181704231.png" alt="image-20191115181704231" style="zoom:50%;" />
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115181728330.png" alt="image-20191115181728330" style="zoom:50%;" />
>
>    m^0以o^0进行初始化：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115181747029.png" alt="image-20191115181747029" style="zoom:50%;" />
>
> 3. 计算每一个迭代步的answer representation
>
>    本文提出了两种方案，使用不同的attention机制计算每一步的answer表达：
>
>    - sequential attention（第一个模型图）
>    - self-attention（第二个模型图）：使用self-attention，就意味着和question进行同样的操作。（此时question和answer共享attention层的参数。）
>
> 4. 计算每个迭代步的cos similarity
>
> 5. 计算最终的cos similarity：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115182229987.png" alt="image-20191115182229987" style="zoom:50%;" />

> 训练：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115182323971.png" alt="image-20191115182323971" style="zoom:50%;" /> （本文M=0.2）



## 实验

1. 数据集

   TRECQA、WikiQA、Insurance QA、FiQA

2. 实验结果

   > <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115182605219.png" alt="image-20191115182605219" style="zoom:50%;" /><img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115182640831.png" alt="image-20191115182640831" style="zoom:50%;" />
   >
   > <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115182848413.png" alt="image-20191115182848413" style="zoom:50%;" /><img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115182916289.png" alt="image-20191115182916289" style="zoom:50%;" />

3. 结论分析

   - 在同样的attention机制上，多跳的 attention network 比单跳的好。
   - 在AS（answer selection）任务上，sequential attention 比其他的注意力机制更好。

















