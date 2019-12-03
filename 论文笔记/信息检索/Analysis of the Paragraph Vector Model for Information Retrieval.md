> > ICTIR2016，IR

<center>Analysis of the Paragraph Vector Model for Information Retrieval</center>

## 背景

PV model 相当于文档级别的语言模型。结合PV模型和传统的语言模型（QL模型）到检索任务时，产生了不稳定的性能和有限的提升。



本文讨论了PV-DBOW模型的三个固有问题，限制了其在检索任务上的性能：

- PV-DBOW的训练过程会导致对短文本的过拟合，使最终的检索模型产生length bias（更倾向于检索到短文档）。
- PV-DBOW模型中，基于corpus频率的负采样导致了对词进行了类似ICF的加权机制，抑制了高频词的重要性。
- PV-DBOW没有捕捉词-上下文信息，导致其不能建模词的替代关系。

本文主要介绍了针对这三个问题，对PV-DBOW模型的改进，以及解释为什么这样的改进可以解决这些问题。



## 基于PV的检索模型

#### PV模型

<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191116192549878.png" alt="image-20191116192549878" style="zoom:50%;" />

<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191116192520062.png" alt="image-20191116192520062" style="zoom:50%;" />

#### PV-LM模型

we use PV-DBOW for language model smoothing in the query likelihood model (QL).

<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191116192113621.png" alt="image-20191116192113621" style="zoom:50%;" />

基于PV的检索模型的确可以对QL模型的性能有提升，但很不稳定。

<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191116191625672.png" alt="image-20191116191625672" style="zoom:50%;" />



## 模型的问题分析及改进

1. **Over-ﬁtting on Short Documents**

   原因分析：造成length bias的原因是，短文本的文本向量更容易和出现在该文本中的词的词向量对齐。

   解决方案：在loss函数中加L2正则化<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191116192420321.png" alt="image-20191116192420321" style="zoom:50%;" />

2. **Improper Noise Distribution**

   原因分析：

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191116192720846.png" alt="image-20191116192720846" style="zoom:50%;" />

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191116193358297.png" alt="image-20191116193358297" style="zoom:50%;" />

   解决方案：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191116192802010.png" alt="image-20191116192802010" style="zoom:50%;" />

3. **Insufﬁcient Modeling for Word Substitution**

   原因分析：PV-DBOW主要是利用词-文档矩阵。有研究表示，利用词-文档矩阵得到的向量表达只能捕捉词的syntagmatic relation，不能捕捉paradigmatic relation。

   syntagmatic relation：指总是出现在相同文本片段的词，有相似的语义。

   paradigmatic relation：指总是出现在相似的上下文，但不共现的词，有相似的语义。

   解决方案：

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191116193212596.png" alt="image-20191116193212596" style="zoom:50%;" />

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191116193243951.png" alt="image-20191116193243951" style="zoom:50%;" />

<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191116193301383.png" alt="image-20191116193301383" style="zoom:50%;" />



















