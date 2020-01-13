> >ACL2019，开放域QA，外部知识

代码/数据地址：https://github.com/xwhan/Knowledge-Aware-Reader/tree/master



## Motivation-论文解决了什么问题

KB 通常不完整且不足以涵盖开放域问题所需的全部证据。另一方面，互联网上大量的无结构文本可以轻松涵盖广泛的知识。因此，可以用文本数据扩展 KB。

## Motivation-本文的方法思路

本文提出了一种新的端到端模型，该模型包括

- 一个简单而有效的子图阅读器（subgraph reader），该阅读器可以从与问题相关的 KB 子图中积累每个 KB 实体的知识 ;
- 知识感知型文本阅读器（knowledge-aware text reader），该阅读器通过条件门控机制有选择地结合所学的有关实体的知识库知识。



## Method-模型概述



[![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/500901524cef02073b21895228321aedadd03796_2_689x295.jpeg)

模型主要由两部分组成：graph-attention based KB reader 和 knowledge-aware text reader。

### SubGraph Reader

子图阅读器使用图注意机制来积累每个子图实体 e 来自其链接邻居 Ne 的知识。 图注意力机制的设计特别考虑了两个重要方面：（1）邻居关系是否与问题相关；（2）邻居实体是否是问题提到的主题实体。 子图阅读器最终为每个实体输出向量表示。

#### Question-Relation Matching

匹配问题和 KB 关系。先用 shared LSTM 编码 Q 和 relation，然后通过自注意力机制得到 relation 的编码，再用 relation 去 attend 问题，最后算点积。

#### Extra Attention over Topic Entity Neighbors

这里作者还加入一个特征，用于判断任意实体 e 的邻居是否和 Q 中出现的主题实体相链接。该指标定义为 I[ei∈E0] 。 直观地，如果一个邻居链接到出现在问题中的主题实体，则其可能比其他非主题邻居对问题的回答更为相关。 所以最后的分数为：

~s(ri,ei)∝exp(I[ei∈E0]+sri)

#### Information Propagation from Neighbors

为了有效利用邻居元组，定义传播规则为：

→e′=γe→e+(1−γe)∑(ei,ri)∈Ne~s(ri,ei)σ(We[→ri;→ei])

其中 γe 是门控函数，用来控制保留原始实体表示信息。公式的后半部分则是邻居元组的信息。

### Knowledge-Aware Text Reader

使用的是 DrQA。

#### Query Reformulation in Latent Space

这里主要为了将 KB 信息引入到 Q 中。首先对 Q 自注意力得到新向量，然后得到 Q 中所有主题实体的表示，然后将他们整合：

→q′=γq→q+(1−γq)tanh(Wq[→q,→eq,→q−→eq])

#### Knowledge-aware Passage Enhancement

使用 bi-LSTM 来编码检索回来的 passages。对于一个 passage token wdi 和 token 特征 fdwi 和链接实体 $e_{w_i}$，条件门控函数为：

→idwi=γd→e′wi+(1−γd)→fdwi, where γd=sigmoid(Wgd[→q⋅→e′wi;→q⋅→fdwi])

#### Entity Info Aggregation from Text Reading

最后将 →idwi 输入到 biLSTM 中得到新向量，并且和 q 算了 attention。对于某个实体 e 和包含 e 的所有文档： De={d|e∈d} ，我们简单地通过将链接文档的表示平均为 →ed=1|De|∑d∈De→d 来汇总信息。

#### Answer Prediction

得到了两个实体表示 →e′ 和 $\vec{e^{d}}$，在通过下式得到该实体是答案的概率：

se=σs(→q′TWs[→e′;→e→d])



## Experiment-实验

### 数据集

WebQSP

### 实验结果

[![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/abe348442727bfd3b929932a83094da655fbb496_2_690x256.png)


可以看到， SGREADER 在不完整的 KB setting 下获得更好的结果，而完整 KB 则与其他模型相当。

使用 KAR EADER 增强 SGR EADER，可以在不完整 KB 的情况下持续改进。 与集成模型相比，也有一定的竞争力。

### Ablation Study

[![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/1a3ac5f519b4680465b5dbdb18834aed8cf37977_2_413x189.png)

我们可以看到重构查询和知识增强对性能至关重要。 此外，条件门控机制也很重要。 潜在的原因是，如果没有问题信息，则门控机制可能会引入一些不相关且具有误导性的知识。



## Highlight

- 结合 incomplete QA 和 text 文档信息

