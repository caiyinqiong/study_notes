>> SIGIR2018，文本匹配，answer selection，PI，对抗学习

<center>CAN: Enhancing Sentence Similarity Modeling with Collaborative and Adversarial Network</center>
## 背景

之前的大部分神经网络模型主要关注学习句子的一个好的表示，或者通过交互的方式捕捉文本对的相似or不相似，，但是忽略了文本对的 common feature。



本文提出$\color{red}{CAN（Collaborative\ and\ Adversarial\ Network）模型}$，在LSTM架构中加入一个 **common feature exactor** 来显式提取文本对的 common feature，其中包括一个生成器 和 一个判别器 来进行协同和对抗学习，



## 方法

####整体框架

> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191106154211160.png" alt="image-20191106154211160" style="zoom:30%;" />
>
> 输入是预训练的词向量
>
> 通过BiLSTM + attention分别得到两个句子的 k 维表示 H_X 和 H_Y。
>
> 相似度预测部分的计算及loss函数：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191106161155043.png" alt="image-20191106161155043" style="zoom:33%;" />
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191106154453662.png" alt="image-20191106154453662" style="zoom:33%;" />
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191106154818366.png" alt="image-20191106154818366" style="zoom:33%;" />
>
>  

####common feature exactor

> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191106154910555.png" alt="image-20191106154910555" style="zoom:30%;" />
>
> Elite hidden state是指句子对中共有的词对应的LSTM的输出。
>
> 对Elite hidden state进行maxpooling，得到 k 维 的 individual feature representation F_X 和 F_Y。
>
> 生成器计算及loss函数：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191106155225190.png" alt="image-20191106155225190" style="zoom:50%;" />
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191106160254283.png" alt="image-20191106160254283" style="zoom:50%;" />
>
> ！！！！理解：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191106160616186.png" alt="image-20191106160616186" style="zoom:50%;" />
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191106160540995.png" alt="image-20191106160540995" style="zoom:33%;" />
>
> 
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191106163135712.png" alt="image-20191106163135712" style="zoom:50%;" />
>
> 判别器计算及loss函数：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191106160334987.png" alt="image-20191106160334987" style="zoom:53%;" />
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191106155333932.png" alt="image-20191106155333932" style="zoom:70%;" />
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191106155837801.png" alt="image-20191106155837801" style="zoom:50%;" />

#### 学习过程

> 总loss函数：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191106161006445.png" alt="image-20191106161006445" style="zoom:50%;" />
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191106161031301.png" alt="image-20191106161031301" style="zoom:50%;" />



## 实验

1. 数据集

   answer selection：WikiQA、TRECQA

   PI：MSRP



## 想法

1. 直接把一个common feature vector 和 曼哈顿距离值进行concat，可能会导致对向量的bias。。。