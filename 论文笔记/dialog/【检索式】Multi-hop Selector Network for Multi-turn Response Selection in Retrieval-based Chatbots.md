> > EMNLP2019

## 背景

对于多轮检索式的对话，以往的做法都是把候选回答和每一个上下文做匹配。上下文表达提供了丰富的信息来提取更多匹配信号，但它也包含噪声信号和不必要的信息（随着对话进行，可能已经发生话题转移）。

本文分析了使用过多上下文表达的边际效应，提出了**multi-hop selector network（MSN）**。先利用一个多跳选择器选择相关的表达作为上下文，然后再用过滤出来的上下文与候选答案进行匹配。



## 方法

<img src="../../images/image-20200226110905109.png" alt="image-20200226110905109" style="zoom:33%;" />

##### 问题描述

<img src="../../images/image-20200226110951887.png" alt="image-20200226110951887" style="zoom:33%;" />

##### 整体模型

- attentive module

  <img src="../../images/image-20200226111135256.png" alt="image-20200226111135256" style="zoom:33%;" />

- context selector

  每个上下文表达通过 attentive module，得到自己的上下文表示：

  <img src="../../images/image-20200226111247096.png" alt="image-20200226111247096" style="zoom:33%;" />

  <img src="../../images/image-20200226111355933.png" alt="image-20200226111355933" style="zoom:33%;" />

  - word 级别的selector

    <img src="../../images/image-20200226111430308.png" alt="image-20200226111430308" style="zoom:33%;" />

    <img src="../../images/image-20200226111458926.png" alt="image-20200226111458926" style="zoom:33%;" />

    <img src="../../images/image-20200226111515095.png" alt="image-20200226111515095" style="zoom:33%;" />

    <img src="../../images/image-20200226111529749.png" alt="image-20200226111529749" style="zoom:33%;" />

  - utterance 级别的selector

    <img src="../../images/image-20200226111652776.png" alt="image-20200226111652776" style="zoom:33%;" />

    <img src="../../images/image-20200226111709753.png" alt="image-20200226111709753" style="zoom:33%;" />

    <img src="../../images/image-20200226111723108.png" alt="image-20200226111723108" style="zoom:33%;" />

  - 结合 word 和 utterance 级别

  <img src="../../images/image-20200226111736145.png" alt="image-20200226111736145" style="zoom:50%;" />

  - hopk selector

    上述只是但是了hop1 selector。由于对某些数据，最后一个utterance可能很简短，没有足够的信息，因此提出了hopk selector，更改 K2。

    <img src="../../images/image-20200226112112792.png" alt="image-20200226112112792" style="zoom:50%;" />

    <img src="../../images/image-20200226112022052.png" alt="image-20200226112022052" style="zoom:33%;" />

- context fusion

  <img src="../../images/image-20200226112302064.png" alt="image-20200226112302064" style="zoom:50%;" />

  <img src="../../images/image-20200226112319140.png" alt="image-20200226112319140" style="zoom:50%;" />

- matching

  - origin matching

    <img src="../../images/image-20200226112436393.png" alt="image-20200226112436393" style="zoom:33%;" />

  - self matching

    <img src="../../images/image-20200226112501688.png" alt="image-20200226112501688" style="zoom:50%;" />

    <img src="../../images/image-20200226112515980.png" alt="image-20200226112515980" style="zoom:33%;" />

  - cross matching

    <img src="../../images/image-20200226112549151.png" alt="image-20200226112549151" style="zoom:50%;" />

    <img src="../../images/image-20200226112602254.png" alt="image-20200226112602254" style="zoom:33%;" />

- aggregation

  <img src="../../images/image-20200226112706129.png" alt="image-20200226112706129" style="zoom:33%;" />

  <img src="../../images/image-20200226112718051.png" alt="image-20200226112718051" style="zoom:33%;" />



## 实验

##### 对抗性实验

实验设置：在Ubuntu数据集上，保持训练集不变，在测试集上，从上下文中随机采样1-3个词，分别加到每个候选答案中，检测匹配模型的性能影响。

<img src="../../images/image-20200226101115264.png" alt="image-20200226101115264" style="zoom:50%;" />

##### 实验

1. 主试验

   ![image-20200226115256313](../../images/image-20200226115256313.png)

2. 消融实验

   <img src="../../images/image-20200226115329914.png" alt="image-20200226115329914" style="zoom:33%;" />

3. 关键超参数的实验

   <img src="../../images/image-20200226115403422.png" alt="image-20200226115403422" style="zoom:33%;" />



# 思考

motivation清晰，方法简洁，而且的确有效果。

