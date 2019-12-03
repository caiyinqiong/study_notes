> > 2016，文本匹配

<center>Sentence Similarity Learning by Lexical Decomposition and Composition</center>

## 背景

过去的文本匹配研究中，仅考虑了两个文本的相似部分，而忽略了不相似部分，但这部分也可以为最终判断提供线索和信息。



本文提出了一个方法，通过 分解和组合句子中的词汇语义 来综合考虑 相似部分和不相似部分，基本步骤：

- 每个词表达成一个向量，基于句子j中的所有词对句子i的每个词计算一个语义匹配向量；
- 基于语义匹配向量，将每个词向量分解成相似部分和不相似部分；
- 通过一个two-channel CNN来组合相似部分和不相似部分的信息
- 基于组合特征向量计算最终的相似度得分



## 模型

> 模型架构：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107175207877.png" alt="image-20191107175207877" style="zoom:40%;" />
>
> 1. word representaiton
>
>    使用预训练的word2vec词向量
>
> 2. Semantic Matching
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107175350574.png" alt="image-20191107175350574" style="zoom:50%;" />
>
>    可以设计的匹配函数：
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107175550337.png" alt="image-20191107175550337" style="zoom:50%;" />
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107175635252.png" alt="image-20191107175635252" style="zoom:50%;" />
>
> 3. Decomposition
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107175418829.png" alt="image-20191107175418829" style="zoom:50%;" />
>
>    可以设计的分解函数：
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107175732898.png" alt="image-20191107175732898" style="zoom:50%;" />
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107175824722.png" alt="image-20191107175824722" style="zoom:50%;" />
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107175854796.png" alt="image-20191107175854796" style="zoom:50%;" />
>
> 4. Composition
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107175448787.png" alt="image-20191107175448787" style="zoom:50%;" />
>
>    可以设计的组合函数：
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107175934956.png" alt="image-20191107175934956" style="zoom:50%;" />
>
> 5. similarity assessoing
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107175511785.png" alt="image-20191107175511785" style="zoom:50%;" />
>
>    可以设计的分类器：FC+sigmoid



## 实验

1. answer selection：QASent、WikiQA
2. PI：MSRP