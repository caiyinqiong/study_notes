>>SIGIR2018，利用知识库提高信息检索性能

<center>Towards Better Text Understanding and Retrieval through Kernel Entity Salience Modeling</center>

## 背景

在信息检索中，基本的流程是【文本理解】+【评估文本中term的重要度】，过去的很多研究都集中在更好的评估word的重要度，但忽略了在基于实体的表示中，实体的重要性评估方法。



本文提出了$\color{red}{KESM模型（kernel\ Entity\ Salience\ Model）}$来评估实体在文档中的重要性，从而可以提高对文本的理解，进而提高信息检索的性能。

基本思想是，通过建模 entity 与 document word 和 document entity 之间的交互关系，来评估entity在文档中的重要度。



## 模型

###KESM模型架构

> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108171418372.png" alt="image-20191108171418372" style="zoom:50%;" />
>
> 1. KEE（knowledge enriched embedding）
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108171522145.png" alt="image-20191108171522145" style="zoom:50%;" />
>
>    用知识库扩充实体表示（w_p表示实体e在freebase中的描述的前20个词）
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108171608512.png" alt="image-20191108171608512" style="zoom:50%;" />
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108171812631.png" alt="image-20191108171812631" style="zoom: 67%;" />
>
>    entity embedding 和 word embedding层都用128维的skip-gram word embedding进行初始化。
>
>    CNN层用宽度为3的128个卷积核。
>
> 2. KIM（kernel interaction model）
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108172125904.png" alt="image-20191108172125904" style="zoom:80%;" />
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108172202908.png" alt="image-20191108172202908" style="zoom:80%;" />
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108172230590.png" alt="image-20191108172230590" style="zoom: 50%;" />
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108172405487.png" alt="image-20191108172405487" style="zoom:50%;" />

###Entity Salience Estimation：

> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108172602679.png" alt="image-20191108172602679" style="zoom:50%;" />
>
> 训练：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108172635643.png" alt="image-20191108172635643" style="zoom:50%;" />

###Ad-hoc retrieval

> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108172659922.png" alt="image-20191108172659922" style="zoom:30%;" />
>
> 计算query中的每个实体和文档的KIM向量，然后再进行合并得到相似度值。
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108172727848.png" alt="image-20191108172727848" style="zoom:50%;" />
>
> 训练：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191108172904851.png" alt="image-20191108172904851" style="zoom:50%;" />
>
> 可以端到端训练整个KESM+W_r；也可以先在实体重要度任务上训练KESM，再在ad-hoc retrieval任务上训练W_r。



## 实验

1. 实体重要度任务

   数据集：New York Times、Semantic Scholar

2. ad-hoc retrieval 任务

   数据集：ClueWeb09、ClueWeb12



















