> > 句子编码，迁移学习

<center>Universal Sentence Encoder</center>

## 背景

本文提出了两个模型，可以学习到可迁移的句子编码表示。



## 模型

#### Encoder

1. DAN（计算简单，但在文本分类的下游任务上表现还不错）

   word 和 bigram 的 embedding 先一起进行平均，然后通过多层FFN。

2. Transformer（效果好，但计算代价比较大）

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191025221041920.png" alt="image-20191025221041920" style="zoom:50%;" />

#### 预训练任务及数据集

论文似乎没太说清楚任务（猜测是Skip-Thought + NLI）。

Skip-Thought属于无监督，可以采用任意的数据集；NLI使用SNLI的数据集



#### 迁移任务及迁移学习模型

下游任务包括情感分类、文本分类、句子对的语义相似度计算等。

对于单句子分类：编码结果通过一个任务特定的DNN。

对于句子对任务：编码结果直接计算sim：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191025221720472.png" alt="image-20191025221720472" style="zoom:35%;" />



## 实验

实验结果：

1. 使用句子编码的迁移学习比词向量迁移更好，但两者结合（concat）起来更好。



