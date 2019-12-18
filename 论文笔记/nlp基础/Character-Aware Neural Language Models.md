> >AAAI2016，基于character的word embedding

<center>Character-Aware Neural Language Models</center>

## 背景

通过neural language model得到的词向量无法捕捉字词信息（e.g.词素），稀少词的embedding学不好，尤其是对于形态丰富的语言。

**本文提出提出了一个语言模型，通过一个CNN层来利用字词信息，CNN的输出作为RNN语言模型的输入。**



## 模型

> 整体框架：
>
> <img src="../../images/image-20191217154312581.png" alt="image-20191217154312581" style="zoom:50%;" />
>
> - character embedding layer
>
> - CNN+maxpooling 层：不同尺寸的多个卷积核之后，使用maxpooling，得到h维的词向量表示y。
>
> - highway layer：
>
>   <img src="../../images/image-20191217154953884.png" alt="image-20191217154953884" style="zoom:50%;" />
>
> - RNN
>
>   如果词典过大，可以使用分层的softmax，计算方式如下：
>
>   <img src="../../images/image-20191217155240311.png" alt="image-20191217155240311" style="zoom:50%;" />
>
> loss函数：<img src="../../images/image-20191217155308995.png" alt="image-20191217155308995" style="zoom:43%;" />



## 实验

- 数据集：先在English Penn Treebank数据集上调参，之后在形态更丰富的其他语言数据集上进行实验。

<img src="../../images/image-20191217155840151.png" alt="image-20191217155840151" style="zoom:33%;" />

- 评估指标：困惑度 <img src="../../images/image-20191217155502951.png" alt="image-20191217155502951" style="zoom:33%;" />

- small 和 large 两个模型版本：

  <img src="../../images/image-20191217160009441.png" alt="image-20191217160009441" style="zoom:33%;" />

- 实验结果：

1. 在ETB 和 其他语言数据集上的结果

   <img src="../../images/image-20191217155713093.png" alt="image-20191217155713093" style="zoom:33%;" >

   <img src="../../images/image-20191217160040867.png" alt="image-20191217160040867" style="zoom:33%;" />                  <img src="../../images/image-20191217160132943.png" alt="image-20191217160132943" style="zoom:43%;" />

2. 利用PCA降维，得到n-gram表示的分布

   <img src="../../images/image-20191217160347650.png" alt="image-20191217160347650" style="zoom:33%;" />

3. highway layers的必要性

   **CNNs + Highway Network over characters can extract rich semantic and structural information.**

   ![image-20191217160413706](../../images/image-20191217160413706.png)

<img src="../../images/image-20191217160435636.png" alt="image-20191217160435636" style="zoom:50%;" />

 <img src="../../images/image-20191217160537918.png" alt="image-20191217160537918" style="zoom:50%;" />

4. 一些其他的结果

把CNN得到的表示和word-level embedding结合起来得到词的表示，在某些任务上性能反而会下降。说明在某些任务上，基于character得到词的表示就足够了。

