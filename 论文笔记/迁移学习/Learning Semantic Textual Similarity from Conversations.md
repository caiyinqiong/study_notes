> > ACL2018，sentence-level 的语义相似度，迁移学习，多任务学习，句子表达编码

<center>Learning Semantic Textual Similarity from Conversations</center>

## 背景

本文提出了一种方法来<u>学习句子层次的表达</u>，进而用来<u>计算句子层次的语义相似度</u>。



具体方法是，先在对话数据集上，基于response selection目标函数训练编码器，然后再**迁移**到具体的句子相似度计算任务上。（基于的假设是，语义相似的句子有相似的response分布，因此，通过response selection目标函数可以学到好的句子表示） 

除此之外，本文还提出，用对话数据集和SNLI数据集进行**多任务学习**，可以有效提高句子编码的质量。



##方法

#### 模型整体架构

> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191025144512934.png" alt="image-20191025144512934" style="zoom:50%;" />
>
> 分别将输入的两个句子通过编码器得到各自的向量表示（response句子通过一个额外的FFN层），然后使用点乘操作计算两者的相似度，进行排序。
>
> loss函数：最大化正确答案的log似然。
>
> 注：两边的encoder共享参数



####编码器：DAN 和 Transformer

>![image-20191025144854303](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191025144854303.png)
>
>本文对比了DAN和Transformer两种编码器，结果是Transformer比DAN好很多，所以之后的实验都采用Transformer编码器。
>
>对于DAN：
>
><img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191025145125290.png" alt="image-20191025145125290" style="zoom:50%;" />
>
>对于Transformer：
>
>是多层Transformer的堆叠，positional embedding采用sine/cosine的形式。
>
>最后是把整个序列的编码进行average-pooling，得到句子的编码表示。



#### 多任务学习

> ![image-20191025145523749](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191025145523749.png)
>
> 多任务学习可以帮助学习不同的语义现象。



## 实验

####训练：

使用Reddit数据集作为对话数据集进行模型训练。

####迁移后的评估实验：

1. STS（Semantic Textual Similarity）

   一种是不微调，直接使用训练得到的encoder分别得到两个句子的编码表示，然后计算相似度<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191025151937634.png" alt="image-20191025151937634" style="zoom:28%;" />；

   另一种是在STS训练集上微调，对编码表示做一次变换操作，再计算相似度。

   实验结果：进行微调之后要好一些；使用多任务学习的比仅使用对话数据集学习的encoder效果更好。

2. similatiry question retrieval（SemEval 2017’s Community Question Answering）

   不微调，对两个句子的编码表示使用cos相似度。

   实验结果：使用多任务学习的比仅使用对话数据集学习的encoder效果更好。

   ​                   即使不进行微调，也可以取得不错的结果（说明在对话数据集上的训练的确学到了不错的句子表达。）

####其他的实验：

用不同比例的SNLI数据集进行多任务学习，发现差不多40%的SNLI数据集就可以帮助提高大部分性能。

<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191025150712261.png" alt="image-20191025150712261" style="zoom:40%;" />

















