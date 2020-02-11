# [MRC ICLR2020] Transformer-XH: Multi-hop question answering with eXtra Hop attention

```
论文作者：Chen Zhao, Chenyan Xiong, Corby Rosset, Xia Song, Paul Bennett, Saurabh Tiwary
论文单位：暂无
论文地址：https://openreview.net/pdf?id=r1eIiCNYwS
代码地址：暂无
论文类别：方法
```

## Motivation-论文解决了什么问题

Transformer 把自然语言看做token的序列进行建模已经取得了很大的成功。但是在许多NLP任务中，文本数据固有地表现出线性序列以外的结构，这些结构可以抽象为具有节点和边的树或图。

在多跳问答中，回答问题所需的证据通常分散在多个相关文档中，因此结构具有很重要的作用，需要通过多个文本之间的链接进行联合推理。

在过去的一些工作中，为了适应Transformer序列建模的特性，会把多跳问答转换成单跳问答来解决，但这样的做法通常会引入有问题的假设或者造成信息丢失。



## Motivation-本文的方法思路

本文提出了Transformer-XH模型，对Transformer进行改进，在层中引入 extra hop attention。通过这样的结构，建模文本片段时可以不仅考虑文本片段的内部信息，也可以融合其他文本段的全局信息，更好地适应结构化文本表示的建模任务。



## Method-模型概述

![image-20200208235846352.png](https://i.loli.net/2020/02/09/cR1iKzngVkpIHOQ.png)

#### Transformer-XH

> 普通的Transformer计算方式：
> $$
> \begin{aligned} H^{l} &=\operatorname{softmax}\left(\frac{Q \cdot K^{T}}{\sqrt{d_{k}}}\right) \cdot V^{T} \\ Q^{T} ; K^{T} ; V^{T} &=W^{q} \cdot H^{l-1} ; W^{k} \cdot H^{l-1} ; W^{v} \cdot H^{l-1} \end{aligned}
> $$
> 则第 $l$ 层的第 $i$ 个词的输出可以表示成：
> $$
> h_{i}^{l}=\sum_{j} \operatorname{softmax}_{j}\left(\frac{q_{i}^{T} \cdot k_{j}}{\sqrt{d_{k}}}\right) \cdot v_{j}
> $$

假设结构化文本数据包括一个节点集合 $\mathcal{X}=\left\{X_{1}, \ldots, X_{\tau}, \ldots X_{\zeta}\right\}$ 和一个边的矩阵 $E$ ，其中每个节点表示一个文本片段。我们的目标是结合每个序列内部的局部信息以及整个结构化文本的全局上下文得到表示 $\mathcal{H}=\left\{\tilde{H}_{1}, \ldots, \tilde{H}_{\tau}, \ldots \tilde{H}_{\zeta}\right\}$ 。

Transformer-XH中引入了两种attention机制：

- in-sequence attention

  和普通的Transformer计算方式一样。
  $$
  h_{\tau, i}^{l}=\sum_{j} \operatorname{softmax}_{j}\left(\frac{q_{\tau, i}^{T} \cdot k_{\tau, j}}{\sqrt{d_{k}}}\right) \cdot v_{\tau, j}
  $$

- extra hop attention
  $$
  \hat{h}_{\tau, 0}^{l}=\sum_{\eta ; e_{\tau \eta}=1} \operatorname{softmax}_{\eta}\left(\frac{\hat{q}_{\tau, 0}^{T} \cdot \hat{k}_{\eta, 0}}{\sqrt{d_{k}}}\right) \cdot \hat{v}_{\eta, 0}
  $$
  每个文本片段的第0个词是特殊符号[CLS].

  $\eta$ 表示与文本片段 $\tau$ 之间存在边的文本片段。

- 合并两者
  $$
  \begin{aligned} \tilde{h}_{\tau, 0}^{l} &=\text { Linear }\left(\left[h_{\tau, 0}^{l} \circ \hat{h}_{\tau, 0}^{l}\right]\right) \\ \tilde{h}_{\tau, i}^{l} &=h_{\tau, i}^{l} ; \forall i \neq 0 \end{aligned}
  $$

> 通过堆叠L层Transformer-XH，就可以获取L跳距离的文本片段信息。


#### 应用到多跳QA任务

任务描述：给定问题 $q$ ，从开放域文本集合中得到答案片段 $a$ 。

- 构建 evidence graph

  图中的节点由三部分构成：1）用DrQA中的方法检索得到的top100文档；2）通过实体链接得到的在 $q$ 中出现的实体的wiki页面；3）由BERT ranker得到1)中的top2文档和2)中每个实体的top1文档，再选择与这些文档具有1跳连接的wiki页面。

  边的构成方式有多种：1）通过Wikipedia链接的单向边；2）通过Wikipedia链接的双向边；3）全连接。

  每个节点的表示：由[SEP]相隔的question + anchor text + paragraph。

- 在 evidence graph 上应用 Transformer-XH

  L层Transformer-XH：
  $$
  \mathcal{H}^{L}=\text { Transformer-XH }(\mathcal{X}, E)
  $$
  两个额外的任务特定的层：
  $$
  \begin{array}{l}{p(\text { relevance } | \tau)=\operatorname{softmax}\left(\operatorname{Linear}\left(\tilde{h}_{\tau, 0}^{L}\right)\right)} \\ {p(\text { start } | \tau, i), p(\text { end } | \tau, j)=\operatorname{softmax}\left(\operatorname{Linear}\left(\tilde{h}_{\tau, i}^{L}\right)\right), \operatorname{softmax}\left(\operatorname{Linear}\left(\tilde{h}_{\tau, j}^{L}\right)\right)}\end{array}
  $$

- 训练

  Transformer部分使用预训练的BERT-base进行初始化，extra hop attention部分的参数从头开始训练。

  使用cross-entropy loss的多任务训练。

  在推断时，先选择相关性得分最高的文档，再在该文档中选择得分最高的片段作为答案。



## Experiment-实验

##### 数据集

HotpotQA FullWiki setting（evidence sentence 和 answer span 的多任务训练）

##### 结果

1. 在 HotpotQA 上的结果

   ![image-20200209001517779.png](https://i.loli.net/2020/02/09/BIWCzr6qSihXLEM.png)

2. 消融实验

   ![image-20200209002040326.png](https://i.loli.net/2020/02/09/CxEeo82d7tyFN3s.png)



## Highlight

在Transformer中加入extra hop attention是一种简单但是有效的方法来更好地建模结构化文本数据的表示，它可以处理Transformer-XL无法处理的非序列结构。