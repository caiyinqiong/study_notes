> > SIGIR2018，QA，融合知识库的文本匹配

<center>Knowledge-aware A￿entive Neural Network for Ranking ￿estion Answer Pairs</center>

## 背景

人类进行文本理解时，<u>背景信息</u>和<u>文本之外的隐含关系</u>是非常重要的，但过去的研究很少关注到这些。

之前的研究表示，仅仅借助KG学习两个文本的单独表示，就比捕捉两个句子的交互关系 在QA匹配上性能更好。



本文提出了$\color{red}{KABLSTM(Knowledge-aware\ Attentive\ Bidirectional\ Long\ Short-Term\ Memory)模型}$，利用知识库中的外部知识来丰富句子的表达。

- 提出了一个**上下文感知的交互学习架构**，
- 包括使用一个**上下文指导的注意力CNN**来将外部知识整合进句子表达
- 和一个**知识感知的注意力机制**进行文本对之间的交互。



## 模型

>整体架构：
>
><img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107003437636.png" alt="image-20191107003437636" style="zoom:50%;" />
>
>1. BiLSTM（普通的双向拼接）
>
>   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107003709620.png" alt="image-20191107003709620" style="zoom:50%;" />
>
>2. Knowledge module：基于知识的句子表达学习
>
>   知识库中的实体表达使用预训练的 d_e 维 TransE向量。
>
>   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107004501087.png" alt="image-20191107004501087" style="zoom:50%;" />
>
>   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107004610203.png" alt="image-20191107004610203" style="zoom:50%;" />
>
>3. Knowledge-aware attention：基于上下文的句子表达学习
>
>   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107004710420.png" alt="image-20191107004710420" style="zoom:50%;" />
>
>   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107004742399.png" alt="image-20191107004742399" style="zoom:50%;" />
>
>   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107004936664.png" alt="image-20191107004936664" style="zoom:50%;" />
>
>   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107005117986.png" alt="image-20191107005117986" style="zoom:50%;" />
>
>4. 分类器
>
>   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107005153797.png" alt="image-20191107005153797" style="zoom:50%;" />
>
>   hidden layer的输入：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107005239648.png" alt="image-20191107005239648" style="zoom:33%;" />（X_feat是精确匹配等特征）

> loss：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191107005631660.png" alt="image-20191107005631660" style="zoom:50%;" />

## 实验

数据集：WikiQA、TRECQA

