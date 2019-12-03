> > ACL2017，SMN模型，多轮对话的response selection

<center> Sequential Matching Network: A New Architecture for Multi-turn Response Selection in Retrieval-Based Chatbots</center>

## 背景

以前的工作是把历史的对话表达concat起来 或者 形成一个上下文向量，再与候选回答进行匹配。

这样的做法的缺点是：

- 丢失了历史表达之间的关系
- 丢失了重要的上下文信息



为了解决这些问题，本文提出了**SMN模型（Sequential Matching Network）。



## 模型

>模型架构：
>
><img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117150056987.png" alt="image-20191117150056987" style="zoom:50%;" />
>
>1. Utterance-Response Matching
>
>   1.1 形成2个匹配矩阵
>
>   - word-word similarity：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117151841115.png" alt="image-20191117151841115" style="zoom:67%;" />
>
>   - sequence-sequence similarity：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117151923339.png" alt="image-20191117151923339" style="zoom:67%;" />
>
>     h向量时通过GRU输出的隐向量。
>
>   1.2 多层【conv + maxpooling】 + linear transformation
>
>2. Matching Accumulation
>
>   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117152215749.png" alt="image-20191117152215749" style="zoom:50%;" />
>
>3. Matching Prediction
>
>   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117152254209.png" alt="image-20191117152254209" style="zoom:50%;" />
>
>   L函数有3中定义方式：
>
>   - 使用最后一个输出状态
>
>   - 线性组合，学习一组权重
>
>   - attention机制：
>
>     <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117152414063.png" alt="image-20191117152414063" style="zoom:50%;" />

> 训练：
>
> cross entropy loss：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117152545516.png" alt="image-20191117152545516" style="zoom:50%;" />



## 实验

<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117152645472.png" alt="image-20191117152645472" style="zoom:50%;" />















