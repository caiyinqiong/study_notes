> > ACL2019，预训练词向量，关系信息

<center>Relational Word Embeddings</center>

## 背景

word embedding编码了许多不同形式的知识（语言学特征和语义信息），但是没有很好地捕捉关系信息。

为了解决这个问题，之前有人提出结合外部的知识图谱进行关系信息学习，但这种方法一般是依靠提前定义好的离散关系类型，因此不是最优的方法。也有人提出，对每个词对，学习一个向量来表示他们之间的关系，但这样不如对每个word学习一个关系向量方便。



不管word embedding如何学习，如果初始意图是捕捉相似度，那学到的词向量捕捉关系的能力就会受到限制。因此，最好的方法是<u>单独学习一种 relational word embedding</u>。



本文提出了一种的方法，也是从**共现统计**中进行学习，得到额外的编码关系信息的word embedding，对标准的word embedding形成补充。两种词向量可以concat起来用于各种NLP任务。



##方法

#### 学习 relation vector（一对词的关系向量）

> 对频繁词形成一个词典V，对V形成的关系集合R中的每个词对学习一个关系向量。
>
> ![image-20191023220609993](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191023220609993.png)
>
> R是对每个词，挑选与其PMI值最大的top100的词形成的词对。
>
> ![image-20191023220828223](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191023220828223.png)



> ![image-20191023220949753](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191023220949753.png)
>
> ![image-20191023221045371](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191023221045371.png)



####学习relational word vector（词的关系词向量）

> ![image-20191023221131417](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191023221131417.png)
>
> 
>
> ![image-20191023221151636](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191023221151636.png)



## 实验

测试relational word embedding的实验：

1. 关系分类
2. 词汇特征分类









