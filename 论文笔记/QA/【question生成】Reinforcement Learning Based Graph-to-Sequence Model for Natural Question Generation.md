# [QG ICLR2020] Reinforcement Learning Based Graph-to-Sequence Model for Natural Question Generation

```
论文作者：Yu Chen, Lingfei Wu, Mohammed J. Zaki
论文单位：暂无
论文地址：https://openreview.net/pdf?id=HygnDhEtvr
代码地址：暂无
论文类别：方法
```

## Motivation-论文解决了什么问题

本文关注的question generation（QG）是根据一个passage和一个answer去生成question。传统方法把QG任务形式化成seq2seq学习问题，但目前的方法存在的缺陷包括：

- 通常把文本数据视为词序列，忽略了潜在的结构信息
- 基于cross-entropy的序列训练有很大的局限性
- 生成question时没有充分利用answer的信息，忽略了passage和answer之间的语义关系



## Motivation-本文的方法思路

针对上述三个缺陷，本文提出了一个基于强化学习的Graph2Seq模型来用于问题生成。该模型包含：

- 根据文本序列和内在的结构信息，构建句法静态图和语义感知的动态图，使用基于Bidirectional Gated Graph Neural Network的编码器来编码passage，然后基于RNN进行解码得到question。
- 一个结合cross-entropy loss 和 RL loss 的evaluator，确保生成的文本是语法和语义有效的。
- 一个Deep Alignment Network把answer的信息融合到passage中。



## Method-模型概述

![image-20200209194431073.png](https://i.loli.net/2020/02/09/pvi3G2Jb4tYEBe6.png)

#### 问题描述

给定passage词序列 $X^{p}=\left\{x_{1}^{p}, x_{2}^{p}, \ldots, x_{N}^{p}\right\}$ 和 answer词序列 $X^{a}=\left\{x_{1}^{a}, x_{2}^{a}, \ldots, x_{L}^{a}\right\}$ ，生成question词序列 $\hat{Y}=\left\{y_{1}, y_{2}, \ldots, y_{T}\right\}$ 。

#### Deep Alignment Network

![image-20200209194728653.png](https://i.loli.net/2020/02/09/wQkyVlXihe8O4db.png)

- 定义soft-alignment 函数（如图2所示）
  $$
  \tilde{\mathbf{H}}^{p}=\operatorname{Align}\left(\mathbf{X}^{p}, \mathbf{X}^{a}, \tilde{\mathbf{X}}^{p}, \tilde{\mathbf{X}}^{a}\right)=\operatorname{CAT}\left(\tilde{\mathbf{X}}^{p} ; \mathbf{H}^{p}\right)=\operatorname{CAT}\left(\tilde{\mathbf{X}}^{p} ; \tilde{\mathbf{X}}^{a} \boldsymbol{\beta}^{T}\right) \\
  \boldsymbol{\beta} \propto \exp \left(\operatorname{ReLU}\left(\mathbf{W X}^{p}\right)^{T} \operatorname{ReLU}\left(\mathbf{W X}^{a}\right)\right)
  $$

- word-level Alignment

  计算 contextual passage embedding：
  $$
  \tilde{\mathbf{H}}^{p}=\operatorname{Align}\left(\mathbf{G}^{p}, \mathbf{G}^{a},\left[\mathbf{G}^{p} ; \mathbf{B}^{p} ; \mathbf{L}^{p}\right], \mathbf{G}^{a}\right)  \\
  \overline{\mathbf{H}}^{p} = BiLSTM(\tilde{\mathbf{H}}^{p})\in \mathbb{R}^{\bar{F} \times N}
  $$
  其中 $G^p$ 表示预训练的glove embedding，$B^p$ 表示BERT embedding， $L^p$ 表示语言学特征（例如NER、POS）

  计算contextual answer embedding：
  $$
  \mathbf{H}^{a} = [\mathbf{G}^{p} ; \mathbf{B}^{p} ] \in \mathbb{R}^{d^{\prime} \times L} \\
  \overline{\mathbf{H}}^{a} = BiLSTM(\mathbf{H}^{a})\in \mathbb{R}^{\bar{F} \times L}
  $$

- hidden-level Alignment
  $$
  \mathbf{X}= BiLSTM\left(\operatorname{Align}\left(\left[\mathbf{G}^{p} ; \mathbf{B}^{p} ; \overline{\mathbf{H}}^{p}\right],\left[\mathbf{G}^{a} ; \mathbf{B}^{a} ; \overline{\mathbf{H}}^{a}\right], \overline{\mathbf{H}}^{p}, \overline{\mathbf{H}}^{a}\right)\right) \in \mathbb{R}^{\bar{F} \times N}
  $$

#### Bidirectional Graph2seq Generator

- passage graph 构建

  passage中的每个词作为图中的每个节点。本文探索两种构图方式：

  1）基于句法的静态图：基于依存关系分析构建有向无权图。先得到每个句子的依存解析树，再把相邻句子的依存解析树连接起来。

  2）语义感知的动态图：基于passage词之间的语义关系构建有向带权重图。先计算稠密邻接矩阵，再用KNN进行稀疏化，最后进行两个方向的规范化。
  $$
  \mathbf{A}=\operatorname{ReLU}\left(\mathbf{U} \tilde{\mathbf{H}}^{p}\right)^{T} \operatorname{Re} \mathrm{L} \mathrm{U}\left(\mathbf{U} \tilde{\mathbf{H}}^{p}\right), \quad \overline{\mathbf{A}}=\mathrm{k} \mathrm{N} \mathbf{N}(\mathbf{A}), \quad \mathbf{A}^{\dashv}, \mathbf{A}^{\vdash}=\operatorname{softmax}\left(\left\{\overline{\mathbf{A}}, \overline{\mathbf{A}}^{T}\right\}\right)
  $$

- 基于GNN的编码

  每个节点的表达用 $X$ 进行初始化。各跳之间共享参数，n跳之后每个节点的表达为 $\mathbf{h}_{v}^{n}$ 。整个图的表达 $\mathbf{h}^{\mathcal{G}}$ 为每个节点表达进行线性映射后再进行maxpooling。每一跳的计算过程如下：

  对静态图：

  ![](https://cdn.mathpix.com/snip/images/9em_eoOuQOqO9uPsQ0-TLoOZ2Ac3SiuUg-98-qDthw4.original.fullsize.png)

  对动态图：

  ![](https://cdn.mathpix.com/snip/images/jIkwXg-UmgL854ZDtFfdrQxOiQoOrqe7TneH_csBLhU.original.fullsize.png)

  每跳之后融合每个节点的双向编码：
  $$
  \mathbf{h}_{\mathcal{N}}^{k}=\operatorname{Fuse}\left(\mathbf{h}_{\mathcal{N}_{\dashv(v)}}^{k}, \mathbf{h}_{\mathcal{N}_{\vdash(v)}}^{k}\right) \\
  \$$
  \text { Fuse }(\mathbf{a}, \mathbf{b})=\mathbf{z} \odot \mathbf{a}+(1-\mathbf{z}) \odot \mathbf{b}, \quad \mathbf{z}=\sigma\left(\mathbf{W}_{z}[\mathbf{a} ; \mathbf{b} ; \mathbf{a} \odot \mathbf{b} ; \mathbf{a}-\mathbf{b}]+\mathbf{b}_{z}\right)
  \$$
  $$
  更新每个节点的表达：
  $$
  \mathbf{h}_{v}^{k}=\operatorname{GRU}\left(\mathbf{h}_{v}^{k-1}, \mathbf{h}_{\mathcal{N}}^{k}\right)
  $$

- 基于RNN的解码

  使用 带有copy机制 和 converage机制 的 attention-based LSTM 解码器。把$\mathbf{h}^{\mathcal{G}}$ 分别通过两个单独的全连接层作为初始隐状态。基于节点表达 $\mathbf{h}_{v}^{n}$ 进行attention。

#### Hybrid Evaluator

对于每个样本，模型输出两个序列：1）根据每步解码的概率分布，进行多项式采样的结果 $Y^s$ ；2）贪心搜索结果 $\hat{Y}$。
$$
\mathcal{L}_{r l}=\left(r(\hat{Y})-r\left(Y^{s}\right)\right) \sum_{t} \log P\left(y_{t}^{s} | X, y_{<t}^{s}\right)
$$
其中， $r(Y)$ 表示 序列 $Y$ 的reward，是根据某些指标与ground truth $Y^{*}$ 计算得到的。

本文采用的reward函数为：
$$
r(Y)=f_{\text {eval }}\left(Y, Y^{*}\right)+\alpha f_{\text {sem }}\left(Y, Y^{*}\right)
$$
其中， $f_{eval}$ 是基于BLEU-4指标，$f_{sem}$ 是基于 WMD（word moves distance）指标。

#### 训练

- 第一阶段，先用cross-entropy loss进行训练
  $$
  \mathcal{L}_{l m}=\sum_{t}-\log P\left(y_{t}^{*} | X, y_{<t}^{*}\right)+\lambda \text { covloss }_{t}  \\
  \text { covloss }_{t} = \sum_{i} \min \left(a_{i}^{t}, c_{i}^{t}\right)
  $$
  其中， $a_{i}^{t}$ 是t时刻，对输入序列的attention向量中的第i个元素。

- 第二阶段，用mixed loss进行微调
  $$
  \mathcal{L}=\gamma \mathcal{L}_{r l}+(1-\gamma) \mathcal{L}_{l m}
  $$



## Experiment-实验

##### 数据集

SQuAD

##### 实验结果

1. 自动评估与人工评估结果

   ![image-20200209220431527.png](https://i.loli.net/2020/02/09/fz9pWgy5qaFB26r.png)

2. 消融实验

   ![image-20200209220510764.png](https://i.loli.net/2020/02/09/LhytYwqi5dJDj9g.png)

   - 从结果来看静态构图法比动态构图法更好。这是因为静态构图方法编码了有用的领域知识，当缺少先验知识时，效果并不一定。同时动态构图法也更简单。
   - 去掉DAN后效果下降明显，说明在QG任务中answer的重要性。
   - 从结果看，Graph2Seq相比Seq2Seq更有优势，说明考虑文本结构的有效性。
   - 在GNN编码passage时，使用双向会更有效。
   - 使用RL微调模型可以进一步提高模型性能。



## Highlight

分析了在QG任务上其他工作的缺陷，并提出有针对性的模块进行解决。









