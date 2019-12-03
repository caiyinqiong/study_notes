> ACL2015，基于交互，PI

<center>Convolutional Neural Network for Paraphrase Identiﬁcation</center>

## 背景

本文提出了**Bi-CNN-MI**模型，使用CNN学习<u>多粒度的句子表达</u>，并在<u>各层次建模交互特征</u>。

而且考虑到MSRP数据集过小的问题，提出了先进行无监督的语言模型预训练，从而得到更好的word embedding和卷积核参数的初始化。

## 模型

> 模型架构：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107150755839.png" alt="image-20191107150755839" style="zoom:50%;" />
>
> 1. CNN-SM（CNN sentence model）
>
>    左右两个句子的sentence model部分共享参数。
>
>    多个【conv + avg + tanh + dynamic k-max pooling】模块的堆叠。
>
>    - conv：使用多个卷积核，宽卷积，而且不是现在的卷积方式，而是在每个维度进行乘法和加法操作，输入的word embedding是S=L✖️d维，卷积核尺寸M=m✖️d维，输出是C=(L+m-1)✖️d维。
>
>    - avg：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107151351224.png" alt="image-20191107151351224" style="zoom:50%;" />
>
>    - tanh：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107151442297.png" alt="image-20191107151442297" style="zoom:50%;" />
>
>      参数b的维度是d/2，对各列计算tanh时公用一组b向量。
>
>    - dynamic k-max pooling
>
>      <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107151655890.png" alt="image-20191107151655890" style="zoom:50%;" />
>
> 2. CNN-IM（CNN interaction model ）
>
>    - 交互矩阵的计算方式：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107151847876.png" alt="image-20191107151847876" style="zoom:50%;" />
>
>      除了unigram层次，其他各层由于使用多个卷积核，每个句子在每层卷积都会输出多个feature map。对两个句子相应的两个feature map计算一个交互矩阵，即每个层次会有多个交互矩阵。
>
>    - 由于交互矩阵的尺寸与输入句子长度有关，无法直接flatten之后输入到分类层，因此会对每个交互矩阵使用dynamic pooling的方式变换到定长的矩阵。

> 训练
>
> - 使用metaoptimize.com/projects/wordreprs/初始化word embedding
>
> - 因为MSRP的数据量较少，本文提出在【无标签的MSRP数据+100000个其他数据集的句子】上使用无监督的方式（noise-contrastive estimation）训练CNN-LM，来预训练 word embedding 和 卷积核参数。
>
>   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107162044802.png" alt="image-20191107162044802" style="zoom:50%;" />
>
> - 再正常地在MSRP数据上端到端训练整个CNN-IM模型。这部分有使用数据增强，对每条（label，s1, s2）增加一条（label，s2, s1）。

## 实验

数据集：MSRP

实验结果：从对比实验可以看出short-ngram和long-ngram两个层次的特征是最重要的。

























