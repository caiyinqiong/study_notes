> > CIKM2018，问题标题生成

<center>Question Headline Generation for News Articles</center>

## 背景

**问题标题生成** 是 **问题生成** 的一种特定形式。

标题生成可视为一个文本摘要问题，只是要求生成的结果是一个短文本句子。



本文提出了$\color{red}{DASeq2Seq模型}$，特点在于：

- 在encoder阶段使用self-attention（global self-attention or distributed self-attention）。
- 在decoder阶段的每一段使用 vocabulary gate，判断该词应该来自 question vocabulary 还是generic vocabulary。



## 模型

> ### Encoder
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191120174244523.png" alt="image-20191120174244523" style="zoom:50%;" />
>
> 1. 使用 hierarchical encoder framework：
>
>    - 首先每个句子的词作为输入，通过BiGRU得到句子的表达向量
>    - 再把句子的表达向量作为输入，得到整个文章的表达向量c
>    - 使用attention机制，得到attention-based atricle representation c'
>
> 2. global self-attention mechanism
>
>    基本假设：如果一个句子可以反映整个文章的主题，则句子时重要的。文章的表达是由重要句子组成的。
>
>    用EM的思想通过迭代得到attention-based atricle representation。
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191120174742517.png" alt="image-20191120174742517" style="zoom:50%;" />
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191120174810600.png" alt="image-20191120174810600" style="zoom:50%;" />
>
> 3. distributed self-attention mechanism
>
>    基本假设：如果一个句子和许多重要的句子相关，则该句子是重要的。
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191120175058300.png" alt="image-20191120175058300" style="zoom:50%;" />
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191120175031157.png" alt="image-20191120175031157" style="zoom:50%;" />
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191120174810600.png" alt="image-20191120174810600" style="zoom:50%;" />



> ### Decoder
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191120174309689.png" alt="image-20191120174309689" style="zoom:50%;" />
>
> 1. 先进行词典选择：question vocabulary and generic vocabulary
>
> ​       来自question vocabulary的概率: <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191120175237430.png" alt="image-20191120175237430" style="zoom:50%;" />
>
> 2. t 时刻的预测词：
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191120183218589.png" alt="image-20191120183218589" style="zoom:50%;" />
>
>    
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191120175325925.png" alt="image-20191120175325925" style="zoom:50%;" />
>
>    
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191120183240761.png" alt="image-20191120183240761" style="zoom:50%;" />



> 训练：
>
> cross entropy loss：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191120185230368.png" alt="image-20191120185230368" style="zoom:50%;" />  （q_t是0/1）



## 实验

<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191120190659043.png" alt="image-20191120190659043" style="zoom:50%;" />



