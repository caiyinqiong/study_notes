> > ICML2014，doc2vec，Paragraph Vector 

<center>Distributed Representations of Sentences and Documents</center>
## 背景

文本的词袋特征的缺点：1）丢失词序信息；2）忽略了词义



本文提出了**Paragraph Vector（PV）算法**，一种<u>无监督</u>的方法来学习变长文本片段的定长特征表达。整体是借助word vector的思路，通过预测文档中的词进行训练，把每个文档表达成dense向量。



## 词向量的学习

> 整体模型图：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191116164222445.png" alt="image-20191116164222445" style="zoom:50%;" />
>
> 拿上下文的词来预测target word：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191116164253659.png" alt="image-20191116164253659" style="zoom:50%;" />
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191116164335810.png" alt="image-20191116164335810" style="zoom:50%;" />
>
> loss函数：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191116164405432.png" alt="image-20191116164405432" style="zoom:50%;" />



## 模型

#### PV-DM（Distributed Memory Model of Paragraph Vectors）

> 模型图：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191116164512727.png" alt="image-20191116164512727" style="zoom:50%;" />
>
> 思路：除了有一个词向量矩阵W，还有一个段落向量矩阵D。两者共同作用来预测某个段落中某个窗口中的target word，从而可以联合学习word embedding 和 paragraph embedding。。好处在于，pv记录了整个paragraph的主题，包含了滑窗内文本缺失的信息，预测起来会更好。
>
> 训练：类似于word embedding学习，只是h函数在concat的时候还加入了该paragraph的embedding。
>
> 推断：固定 word embedding 和 softmax 的参数，相当于在段落向量矩阵D中新加入一列。同样通过采样新段落中的多个窗口文本进行训练，最终得到新段落的向量表达。

#### PV-DBOW（Distributed Bag of Words version of Paragraph Vector）

> 模型图：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191116165212305.png" alt="image-20191116165212305" style="zoom:50%;" />
>
> 思路：总体思路是用pv预测该paragraph中的词（at each iteration of stochastic gradient descent, we sample a text window, then sample a random word from the text window and form a classiﬁcation task given the Paragraph Vector）
>
> 训练后只需要存下softmax的参数即可。对于新的paragraph要继续学习其向量表达。

**使用PV_DM训练的段落向量通常效果就很好了，但把PV-DM和PV-DBOW学到的段落向量concat起来效果会更稳定。**

## 实验

1. 情感分析
   - SST
   - IMDB
2. 信息检索



