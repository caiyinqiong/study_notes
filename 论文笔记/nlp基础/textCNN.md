> > 2014，文本分类

<center>Convolutional Neural Networks for Sentence Classiﬁcation</center>



## 模型

><img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107134931055.png" alt="image-20191107134931055" style="zoom:50%;" />
>
>1. 词向量使用两个channel，一个固定，一个微调
>
>   使用预训练的word2vec向量
>
>2. CNN + Maxpooling
>
>   一个卷积核同时作用在两个chennel的相同位置上，得到的2个值相加，作为一个feature值的输出。
>
>   一层CNN使用不同尺寸的卷积核（3，4，5），每种卷积核100个。 
>
>   **（使用多种尺寸的卷积核非常重要！！！）**
>
>   对每个卷积核的输出特征向量进行maxpooling，最终得到句子的向量表示。
>
>3. 分类层
>
>   dropout + FC + softmax

训练时加L2正则化。





