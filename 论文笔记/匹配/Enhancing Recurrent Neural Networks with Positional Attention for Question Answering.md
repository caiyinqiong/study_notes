> > SIGIR2017，RNN-POA，QA，基于交互的

<center>Enhancing Recurrent Neural Networks with Positional Attention for Question Answering</center>

## 背景

在QA任务上，相比不加attention的模型，加了attention之后，效果会有提升，，但对position information并没有充分的研究。



本文基于假设，如果question中的某个词出现在answer中，那answer中这个词附近的词应该得到更多的关注。提出了**RNN-POA模型（Positional Attention based RNN）**，融合question中的词在answer中的位置信息，得到answer的基于注意力的表示。



## 模型

> 模型框架：
>
> - question使用正常的LSTM+self-attention的方式得到question representation。
>
>   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117162739442.png" alt="image-20191117162739442" style="zoom:50%;" />
>
> - answer在LSTM之上，加入了pos-attention，使用下面的方式得到 position-aware answer representation。
>
> - 最后使用曼哈顿距离计算相似度：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117162635148.png" alt="image-20191117162635148" style="zoom:50%;" />
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117162312919.png" alt="image-20191117162312919" style="zoom:80%;" />
>
> 1. Position-aware Influence Propagation
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117162824414.png" alt="image-20191117162824414" style="zoom:50%;" />
>
>    u代表该词与question中出现在answer中的词之间的距离。
>
> 2. Position-aware Influence Vector
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117163027257.png" alt="image-20191117163027257" style="zoom:50%;" />
>
>    
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117163058191.png" alt="image-20191117163058191" style="zoom:50%;" />
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117163120078.png" alt="image-20191117163120078" style="zoom:50%;" />
>
> 3. Positional Attention
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117163356487.png" alt="image-20191117163356487" style="zoom:50%;" />
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117163414650.png" alt="image-20191117163414650" style="zoom:50%;" />
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117163432303.png" alt="image-20191117163432303" style="zoom:50%;" />



## 实验

1. TRECQA、WikiQA

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117161550779.png" alt="image-20191117161550779" style="zoom:50%;" /> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117161619522.png" alt="image-20191117161619522" style="zoom:50%;" />

2. 对σ的探究实验

<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117161642691.png" alt="image-20191117161642691" style="zoom:67%;" />









