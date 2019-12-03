> >SIGIR2018，弱监督训练，文本匹配

<center>Multi-level Abstraction Convolutional Model with Weak Supervision for Information Retrieval</center>

## 背景

过去的工作是对所有query使用同一个匹配函数，没有考虑用户可能需要不同层次的匹配。

- lexical query ---> lexical matching
- conceptual query ---> conceptual matching



本文提出了$\color{red}{MACM（multi-level\ abstraction\ convolutional\ model ）模型}$，生成和组合不同抽象层次的匹配值，来应对不同类型的查询。并采用弱监督的方法来进行模型训练。



## 模型

####模型架构

> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108223758946.png" alt="image-20191108223758946" style="zoom:50%;" />
>
> 根据query和document的预训练word embedding，计算交互矩阵
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108223913929.png" alt="image-20191108223913929" style="zoom:50%;" />
>
> 多层CNN+maxpooling（本文使用2层CNN）
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108223937132.png" alt="image-20191108223937132" style="zoom:50%;" />
>
> 计算各抽象层次的匹配值：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108224002389.png" alt="image-20191108224002389" style="zoom:50%;" />
>
> 计算各匹配值的权重：
>
> 计算各个权重的方式有根据问题的类型，或者根据匹配值的强度（本文采用这种）。
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108224124857.png" alt="image-20191108224124857" style="zoom:50%;" />
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108224143539.png" alt="image-20191108224143539" style="zoom:50%;" />
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108224201658.png" alt="image-20191108224201658" style="zoom:50%;" />
>
> 合并机制：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108224219144.png" alt="image-20191108224219144" style="zoom:50%;" />

#### 训练

> 对训练的每个query，用BM25检索top100，随机取样2个候选文档，BM25结果更大的作为正例，另一个作为负例。对评估和测试集的每个query，重排BM251000.
>
> loss：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108224614456.png" alt="image-20191108224614456" style="zoom:50%;" />



## 实验

1. 数据集：clueweb09B