> > ACL2019，预训练方法，文本表示学习，文本分类任务

<center>Encouraging Paragraph Embeddings to Remember Sentence Identity Improves Classiﬁcation</center>

## 背景

<u>段落编码模型</u>对于下游的分类任务是很重要的，但是对于模型究竟编码了什么特征到文本的向量表示中，并不明确。



本文首先研究了之前的一个段落编码模型**CNN-DCNN模型（**该模型可以使得下游的分类任务的性能有提高），使用 sentence content probe task 发现它并没有可以很好的识别一个句子是否属于该段落，甚至在这方面的表现还不如CBOW模型。

而且本文发现，如果以 $\color{red}{sentence\ content\ task}$ 作为半监督的预训练任务，不仅可以<u>使下游的分类任务有性能提高，还可以使预训练速度有很大的提升，并且学到的表达具有更好的泛化能力</u>。



##CNN-DCNN模型

该模型是由 multi-layer convolutional encoder + multi-layer deconvolutional encoder 组成。

以 序列重建 作为预训练任务。

![image-20191023111251733](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191023111251733.png)

![image-20191023111001087](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191023111001087.png)



## sentence content task

![image-20191023144753563](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191023144753563.png)

输入：一个句子s和一个段落p的concat，分别通过encoder。（论文并没有详细说encoder是否共享参数。）

数据集的构造：对每个p，采样它其中的一个句子作为正例s，采样一个非p中的句子作为负例s。

分类器：一个FFN层（300d + ReLU），判别句子s是否是p中的句子。



##实验

#### 探针实验

对比实验：

1. CNN-DCNN：拿CNN部分作为encoder，以h为表达

2. CBOW：拿CNN-DCNN模型训练得到的词向量的平均作为表达

实验结果：使用sentence content task作为探针进行实验，发现CNN-DCNN的表现不如CBOW。

实验分析：相比CNN中复杂的非线性，CBOW更容易进行两个表达的匹配。

####下游实验

对比实验：

1. CNN-SC：拿CNN-DCNN的encoder部分，使用sentence content task作为预训练任务，然后进行下游任务。
2. CNN-R：原始的CNN-DCNN模型，使用序列重建作为预训练任务，然后进行下游任务。

下游任务：

1. Yelp Review Polarity （情感分类）
2. DBPedia （主题分类）
3. Yahoo！Answer （判断answer是否是正确的answer）

实验结果：

1. 即使不进行微调，对于下游任务，CNN-SC进行预训练也比CNN-R进行预训练表现更好。
2. 如果进行微调，CNN-SC使用更少的下游任务数据进行微调就可以获得更好的结果。



##自己的思考

以判断某个句子是否是该文本中的句子作为预训练任务，进行文本表示建模，，有点类似于 【ACL2019： Latent Retrieval for Weakly Supervised Open Domain Question Answering】中的  Inverse Cloze Task 预训练任务。。

















