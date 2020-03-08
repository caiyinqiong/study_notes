> > EMNLP2019

## 背景

本文考虑document-level的翻译做不好是因为无法很好地考虑全局上下文。

本文提出HM-GDC（hierarchical modeling of global document context）翻译模型：

- 采用分层的encoder模型（sentence-encoder + document-encoder）
- 显式地在词级别的上下文表示中加入融合了全局上下文的词表示
- 考虑到域内的平行语料不多，所以采用先预训练部分模块，再训练剩余模型（同时微调其他模块）



## 方法

![image-20200228122017792](../../images/image-20200228122017792.png)

##### 输入：

<img src="../../images/image-20200228122601155.png" alt="image-20200228122601155" style="zoom:33%;" />

##### Encoder：

- sentence encoder：编码句子的内部表示

<img src="../../images/image-20200228122642904.png" alt="image-20200228122642904" style="zoom:33%;" />

- document encoder：编码句子间的表示

<img src="../../images/image-20200228122708139.png" alt="image-20200228122708139" style="zoom:33%;" />

- 在词的表示中融入 global context

  <img src="../../images/image-20200228122816834.png" alt="image-20200228122816834" style="zoom:33%;" />

##### Decoder：

<img src="../../images/image-20200228123058907.png" alt="image-20200228123058907" style="zoom:50%;" />

<img src="../../images/image-20200228122932946.png" alt="image-20200228122932946" style="zoom:50%;" />

<img src="../../images/image-20200228123212422.png" alt="image-20200228123212422" style="zoom:50%;" />



## 训练

<img src="../../images/image-20200228122057865.png" alt="image-20200228122057865" style="zoom:50%;" />

两阶段的训练：

- 预训练：大量的域外sentence-level的平行语料 $D_s$ 训练黄色模块

  <img src="../../images/image-20200228122243594.png" alt="image-20200228122243594" style="zoom:33%;" />

- 少量的域内document-level的平行语料 $D_d$ 训练蓝色模块，同时黄色模块一起微调

  <img src="../../images/image-20200228122320857.png" alt="image-20200228122320857" style="zoom:33%;" />



## 实验

![image-20200228123325461](../../images/image-20200228123325461.png)



## 思考

1. 除了使用两层的encoder，而且公式5和6，显式地将global context融入到词的表示中，帮助进行词级别翻译时的消歧。
2. 这样的分块训练方法在ICLR2020的《Low-Resource Knowledge-Grounded Dialogue Generation》中也出现过。