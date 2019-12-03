> Soft-Cosine Semantic Similarity

<center>SimBow at SemEval-2017 Task 3: Soft-Cosine Semantic Similarity between Questions for Community Question Answering</center>

## 背景

用TFIDF向量表示文本，然后用cos相似度计算相关性的缺点：无法融入语义相关，只能是精确匹配。

<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191027113305924.png" alt="image-20191027113305924" style="zoom:50%;" />



本文提出了一种**Soft-Cosine Semantic Similarity**的度量方式。



## 方法

> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191027113415577.png" alt="image-20191027113415577" style="zoom:50%;" />
>
> 矩阵M的设计方法：
>
> 1. 使用word2vec分布式向量
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191027113516832.png" alt="image-20191027113516832" style="zoom:50%;" />
>
> 2. 使用编辑距离
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191027113806431.png" alt="image-20191027113806431" style="zoom:50%;" />



