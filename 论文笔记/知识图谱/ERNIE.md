# [KG ACL2019] ERNIE: Enhanced Language Representation with Informative Entities

```
论文作者：Zhengyan Zhang, Xu Han, Zhiyuan Liu, Xin Jiang, Maosong Sun, Qun Liu
论文单位：清华大学
论文地址：https://arxiv.org/pdf/1905.07129.pdf
代码地址：https://github.com/thunlp/ERNIE
论文类别：方法
```



## Motivation-论文解决了什么问题

知识图谱可以提供丰富的结构化知识信息，帮助更好地进行语言理解。但目前的预训练语言模型中，很少有考虑融合知识图谱的。



## Motivation-本文的方法思路

本文使用大规模的文本语料和知识图谱来训练一个enhanced language representation model（ERNIE）。

在语言表达模型中融合外部知识的难点在于：

- 结构化知识编码：给定一个文本，如何有效地从KG中提取并编码和它相关的知识信息。
- 异质信息融合：语言表达的预训练过程和知识表达的过程不同，所以会编码到两个不同的向量空间，需要设计一个预训练目标来把词汇、句法和知识信息融合起来。

针对上述两个难点，本文的解决方案是：

- 对于给定文本，首先识别出其中的named entity mentions，然后把这些mentions对齐到知识图谱中对应的实体。使用类似TransE的算法得到知识图谱的编码，并把这些实体的编码作为ERNIE的输入。
- 除了 BERT 采用的 MLM 和 NSP 预训练任务，本文还设计了一个新的预训练任务 dEA 来更好地融合文本和知识信息。随机mask掉一些对齐的实体，让模型从知识图谱中选择合适的实体来完成对齐。



## Method-模型概述

输入的 token sequence = $\left\{w_{1}, \dots, w_{n}\right\}$ ，对应的 entity sequence = $\left\{e_{1}, \dots, e_{m}\right\}$ 。通常m<n，因为不是所有的token都有对应的entity。

![image-20191227191758293.png](https://i.loli.net/2019/12/27/OD8NZdoFs3fKM1h.png)

#### 模型架构

模型整体由 **T-Encoder** 和 **K-Encoder** 构成。

- **输入**：token input = token embedding + segment embedding + positional embedding 

  ​            entity input = $\left\{e_{1}, \dots, e_{m}\right\}   $ （使用TransE预训练得到）

- **T-Encoder**：用于捕捉基本的词汇和语法信息。

  N 层 Bi-Transformer 的堆叠，得到：

  ​                        $\left\{\boldsymbol{w}_{1}, \ldots, \boldsymbol{w}_{n}\right\}=\mathrm{T}\text{-Encoder}\left(\left\{w_{1}, \ldots, w_{n}\right\}\right)$

- **K-Encoder**：整合额外的知识信息到文本信息，使两者到同一个特征空间。

  M 层 Bi-Transformer 的堆叠，得到： 

  ​         $\begin{aligned}\left\{\boldsymbol{w}_{1}^{o}, \ldots, \boldsymbol{w}_{n}^{o}\right\},\left\{\boldsymbol{e}_{1}^{o}, \ldots, \boldsymbol{e}_{n}^{o}\right\} &=\mathrm{K}\text{-Encoder}\left( \left\{\boldsymbol{w}_{1}, \ldots, \boldsymbol{w}_{n}\right\},\left\{\boldsymbol{e}_{1}, \ldots, \boldsymbol{e}_{m}\right\}\right) \end{aligned}$
  
  其中，每一层作为一个Aggregator，
  
  $$
  \begin{aligned}\left\{\boldsymbol{w}_{1}^{(i)}, \ldots, \boldsymbol{w}_{n}^{(i)}\right\},\left\{\boldsymbol{e}_{1}^{(i)}, \ldots, \boldsymbol{e}_{m}^{(i)}\right\}=& \text { Aggregator }(\\ &\left.\left\{\boldsymbol{w}_{1}^{(i-1)}, \ldots, \boldsymbol{w}_{n}^{(i-1)}\right\},\left\{\boldsymbol{e}_{1}^{(i-1)}, \ldots, \boldsymbol{e}_{m}^{(i-1)}\right\}\right) \end{aligned}
  $$
  Aggregator 的具体计算步骤：
  $$
  \begin{aligned}
  &\left\{\tilde{\boldsymbol{w}}_{1}^{(i)}, \ldots, \tilde{\boldsymbol{w}}_{n}^{(i)}\right\} = MultiAtten \left(\left\{\boldsymbol{w}_{1}^{(i-1)}, \ldots, \boldsymbol{w}_{n}^{(i-1)}\right\}\right) \\
  &\left\{\tilde{\boldsymbol{e}}_{1}^{(i)}, \ldots, \tilde{\boldsymbol{e}}_{m}^{(i)}\right\} = MultiAtten \left(\left\{\boldsymbol{e}_{1}^{(i-1)}, \ldots, \boldsymbol{e}_{m}^{(i-1)}\right\}\right) \end{aligned}
  $$
  ​           对于有对应实体的token：（对于文本中的命名实体短语，KG中的实体只对齐到短语中的第一个词）
  $$
  \begin{aligned}\boldsymbol{h}_{j} &=\sigma\left(\tilde{\boldsymbol{W}}_{t}^{(i)} \tilde{\boldsymbol{w}}_{j}^{(i)}+\tilde{\boldsymbol{W}}_{e}^{(i)} \tilde{\boldsymbol{e}}_{k}^{(i)}+\tilde{\boldsymbol{b}}^{(i)}\right) \\ \boldsymbol{w}_{j}^{(i)} &=\sigma\left(\boldsymbol{W}_{t}^{(i)} \boldsymbol{h}_{j}+\boldsymbol{b}_{t}^{(i)}\right) \\ \boldsymbol{e}_{k}^{(i)} &=\sigma\left(\boldsymbol{W}_{e}^{(i)} \boldsymbol{h}_{j}+\boldsymbol{b}_{e}^{(i)}\right) \end{aligned}
  $$
  ​          对于没有对应实体的token：
  $$
  \begin{aligned} \boldsymbol{h}_{j} &=\sigma\left(\tilde{\boldsymbol{W}}_{t}^{(i)} \tilde{\boldsymbol{w}}_{j}^{(i)}+\tilde{\boldsymbol{b}}^{(i)}\right) \\ \boldsymbol{w}_{j}^{(i)} &=\sigma\left(\boldsymbol{W}_{t}^{(i)} \boldsymbol{h}_{j}+\boldsymbol{b}_{t}^{(i)}\right) \end{aligned}
  $$
  
- **输出**：$\left\{\boldsymbol{w}_{1}^{o}, \ldots, \boldsymbol{w}_{n}^{o}\right\},\left\{\boldsymbol{e}_{1}^{o}, \ldots, \boldsymbol{e}_{n}^{o}\right\}$

#### 预训练

除了BERT的MLM和NSP两个预训练任务，本文还设计了一个 denoising entity auto-encoder (dEA) 任务，用以训练模型对实体信息的感知和对齐。

对于一个给定的文本-实体对应序列，5% 的情况下实体被替换为一个随机实体，15% 的情况下实体被mask掉，80% 的情况下保持不变。

考虑到知识图谱中整个实体集合是非常大的，不利于softmax的计算。因此，只是基于给定实体序列而不是整个知识图谱集合进行实体对齐预测。对于一个词 $w_i$，实体对齐分布为：

​                                               $p\left(e_{j} | w_{i}\right)=\frac{\exp \left(\operatorname{linear}\left(\boldsymbol{w}_{i}^{o}\right) \cdot \boldsymbol{e}_{j}\right)}{\sum_{k=1}^{m} \exp \left(\operatorname{limear}\left(\boldsymbol{w}_{i}^{o}\right) \cdot \boldsymbol{e}_{k}\right)}$

上式的概率也用于计算dEA的交叉熵损失。模型整体的预训练loss为dEA、MLM、NSP的loss之和。

#### 微调

![image-20191227202343044.png](https://i.loli.net/2019/12/27/TNRpXg3ZDUtaGxm.png)

- 对于一般的NLP任务：采用和BERT相同的方式。
- 对于Relation Classiﬁcation任务：在头实体和尾实体前后分别加【HD】和【TL】标志。最后用【CLS】处的token embedding 进行分类。
- 对于Entity Typing任务：是Relation Classiﬁcation任务的简化版本，在实体前后加【ENT】标志。



## Experiment-实验

**预训练**

- transformer 层的参数是采用BERT的预训练参数进行初始化。
- 使用English Wikipedia作为预训练语料。entity embedding也是基于Wikidata，使用TransE算法训练得到的，而且entity embedding在模型训练时是固定的。
- 部分超参数：N=6，M=6，token embedding dim=768，entity embedding dim=100，T-Encoder heads=12， K-Encoder heads=4，max_seq_len=256

**实验一：Entity Typing（识别实体的语义类型）**

在 FIGER 和 Open Entity 数据集上进行微调。

![image-20191227211207982.png](https://i.loli.net/2019/12/27/zl6nyBewO4o7kNf.png)

![image-20191227211330880.png](https://i.loli.net/2019/12/27/l6Mto5TUZaLdArQ.png)

- 在FIGER上的结果表明，BERT 在macro和micro指标上与 NFGEC 相当，但是在 Acc 上表现较差，因为 BERT 强大的拟合能力使其学习到了一些远监督的错误标签。但 ERNIE 在Acc指标上有显著提升，说明外部知识可以帮助 ERNIE 避免拟合噪声标签。

**实验二：Relation Classiﬁcation**

在 FewRel 和 TACRED 数据集上进行微调。

![image-20191227212138570.png](https://i.loli.net/2019/12/27/twDZgSCaUNifYzc.png)

- 因为FewRel没有足够的训练数据，导致从头开始训练的CNN表现很差。相比而言，BERT 和 ERNIE 表现出极大的优势，说明对于小数据集而言，融合外部知识非常重要。

**实验三：GLUE**

![image-20191227212721799.png](https://i.loli.net/2019/12/27/ynwiBI9De3ZJQRd.png)

- ERNIE 在大数据集上与 BERT_BASE 结果一致，但在小数据集上结果不太稳定。总体而言，两者性能相当。一方面说明GLUE任务不需要外部知识，另一方面说明ERNIE在融合了异质信息后并没有损失文本信息。

**实验四：Ablation Study**

![image-20191227213218619.png](https://i.loli.net/2019/12/27/lGOzTPxwhicQtr2.png)

- 即使没有实体序列输入，dEA 仍然可以在预训练阶段将知识信息融合到语言表示中，使 F1 得分比 BERT 高0.9%。
- 在有实体序列输入时，如果没有dEA预训练任务，ERNIE只能略微利用这些信息，将 F1 得分相比 BERT 提升0.7%。



## Highlight

- 本文提出了 ERNIE模型，可将知识信息整合进语言模型中。为了更好地融合来自文本和知识图谱的异构信息，本文还提出了新的预训练任务 dEA。

