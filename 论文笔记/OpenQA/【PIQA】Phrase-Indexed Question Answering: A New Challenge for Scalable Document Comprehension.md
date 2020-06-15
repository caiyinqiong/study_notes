> > EMNLP2018，抽取式QA



# 背景

如果可以完全独立的编码document和question，则可以预计算document phrase（所有的候选answer span）的表示并离线建索引。

传统的方法：

<img src="../../images/image-20200529222119225.png" alt="image-20200529222119225" style="zoom:33%;" />

PIQA（Phrase-Index QA）：（a表示一个candidate phrase，即D中的一个span）

<img src="../../images/image-20200529222214847.png" alt="image-20200529222214847" style="zoom:33%;" />



# 模型

本文就该任务提出了几个baseline。

- LSTM baseline：embedding layer是CharCNN+Glove；然后将question和document分别通过LSTM层得到q_i和d_i；然后再对question进行self-attention，得到question的向量表示；对document进行self-attention。

  <img src="../../images/image-20200529222559880.png" alt="image-20200529222559880" style="zoom:50%;" />

  <img src="../../images/image-20200529222621204.png" alt="image-20200529222621204" style="zoom:50%;" />

  <img src="../../images/image-20200529222648291.png" alt="image-20200529222648291" style="zoom:50%;" />

  ![image-20200529222804154](../../images/image-20200529222804154.png)

  （q_s和q_e是使用两组不同的参数得到的，但论文中没说明只是w不同，还是LSTM参数也不共享）

- LSTM+SA：

  <img src="../../images/image-20200529222953905.png" alt="image-20200529222953905" style="zoom:50%;" />

- TFIDF：计算每个phrase（长度<=7）的TFIDF向量。



# 实验

在SQuAD1.1数据集上进行，考虑长度<=7的span。

<img src="../../images/image-20200529223359962.png" alt="image-20200529223359962" style="zoom:33%;" />

对每个phrase的表示单独训练一个分类器，设定阈值threshold过滤掉不太能作为answer的phrase，从而能够节省存储空间。

<img src="../../images/image-20200529223542951.png" alt="image-20200529223542951" style="zoom:33%;" />



# 结论

- 加入Phrase-Index的限制，会在一定程度上损失性能，但是会获得效率的提高。

