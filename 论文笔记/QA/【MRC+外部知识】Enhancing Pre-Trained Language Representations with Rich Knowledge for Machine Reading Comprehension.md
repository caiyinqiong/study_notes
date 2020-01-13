# [MRC ACL2019] Enhancing Pre-Trained Language Representations with Rich Knowledge for Machine Reading Comprehension

```
论文作者：An Yang, Quan Wang, Jing Liu, Kai Liu, Yajuan Lyu, Hua Wu, Qiaoqiao She, Sujian Li
论文单位：Peking University, Baidu Inc.
论文地址：https://www.aclweb.org/anthology/P19-1226.pdf
代码地址：http://github.com/paddlepaddle/models/tree/develop/PaddleNLP/Research/ACL2019-KTNET
论文类别：方法
```



### Motivation-论文解决了什么问题

MRC任务不仅需要语言理解，还需要知识来支持复杂的推理。本文提出**KT-NET模型**（Knowledge and Text fusion NET），利用外部知识库来提升BERT用于MRC任务的性能。



### Motivation-本文的方法思路

本文提出的KT-NET模型可以有效结合预训练语言模型和外部知识库的优势，从而更好地解决MRC任务。基本思路是：

- 利用注意力机制从KB中选择想要的知识
- 把选择的知识融合进BERT中，进行上下文感知和知识感知的答案预测。



### Method-模型概述

<img src="https://i.loli.net/2020/01/03/h3CcylAdXnNut6I.png" alt="image-20200103175957431.png" style="zoom:40%;" />

#### 整体模型

输入：passage $P=\left\{p_{i}\right\}_{i=1}^{m}$ ，question $Q=\left\{q_{j}\right\}_{j=1}^{n}$

输出：answer $A=\left\{p_{i}\right\}_{i=a}^{b}$ （passage中的一个连续片段）

模型一共分为以下四个模块：

- BERT Encoding layer

  使用BERT对序列 $S=[\langle\mathrm{CLS}\rangle, Q,\langle\mathrm{SEP}\rangle, P,\langle\mathrm{SEP}\rangle]$ 进行编码，得到该层输出 
  $$
  \left\{\mathbf{h}_{i}^{L}\right\}_{i=1}^{m+n+3} \in \mathbb{R}^{d_{1}}
  $$

- Knowledge Integration layer

  对每个词 $s_i$ ，从知识库中检索出一组与其相关的概念集合 $C\left(s_{i}\right)$ 。对每个概念 $c_j \in C(s_i)$ ，其对应的KB编码为 $c_j \in \mathbb{R}^{d_{2}}$ 。（此外，知识库中存在一个标记节点，其对应的编码为 $\overline{\mathbf{c}} \in \mathbb{R}^{d_{2}}$ ）

  基于注意力机制，计算词 $s_i$ 的 knowledge state vector：
  $$
  \alpha_{i j} \propto \exp \left(\mathbf{c}_{j}^{\top} \mathbf{W h}_{i}^{L}\right)  \\
  \beta_{i} \propto \exp \left(\overline{\mathbf{c}}^{\top} \mathbf{W} \mathbf{h}_{i}^{L}\right) \\
  \mathbf{k}_{i}=\sum_{j} \alpha_{i j} \mathbf{c}_{j}+\beta_{i} \overline{\mathbf{c}}  \  \ \ \ \  \ \ \ \ \ 
  其中， \sum_{j} \alpha_{i j}+\beta_{i}=1
  $$
  将每个词的 BERT representation 与 knowledge state vector 串接起来，作为该层的输出
  $$
  \mathbf{u}_{i} =\left[\mathbf{h}_{i}^{L}, \mathbf{k}_{i}\right] \in \mathbb{R}^{d_{1}+d_{2}}
  $$

  >如果要结合多个知识库，则词 $s_i$ 对每个知识库形成一个knowledge state vector，最后把 BERT representation 与 所有的knowledge state vector 串接起来，作为该层输出。

- Self-matching layer

  使用self-attention机制，使得 context components $\mathbf{h}_{i}^{L}$ 和 knowledge components $\mathbf{k}_{i}$ 可以进一步交互。

  计算直接交互：
  $$
  r_{i j}=\mathbf{w}^{\top}\left[\mathbf{u}_{i}, \mathbf{u}_{j}, \mathbf{u}_{i} \odot \mathbf{u}_{j}\right] \\
  \begin{aligned} a_{i j} &=\frac{\exp \left(r_{i j}\right)}{\sum_{j} \exp \left(r_{i j}\right)} \\ 
  \mathbf{v}_{i} &=\sum_{j} a_{i j} \mathbf{u}_{j} \end{aligned}
  $$
  计算间接交互：
  $$
  \begin{aligned} \overline{\mathbf{A}} &=\mathbf{A}^{2} \\ 
  \overline{\mathbf{v}}_{i} &=\sum_{j} \bar{a}_{i j} \mathbf{u}_{j} \end{aligned} \\
  其中，A \ 是由\ a_{ij} \ 组成的矩阵。
  $$
  该层的最终输出：
  $$
  \mathbf{o}_{i} = \left[\mathbf{u}_{i}, \mathbf{v}_{i}, \mathbf{u}_{i}-\mathbf{v}_{i}, \mathbf{u}_{i} \odot \mathbf{v}_{i}, \overline{\mathbf{v}}_{i}, \mathbf{u}_{i}-\overline{\mathbf{v}}_{i}\right] \in \mathbb{R}^{6 d_{1}+6 d_{2}}
  $$

- Output layer

  计算输出答案在passage中的起止位置：
  $$
  p_{i}^{1}=\frac{\exp \left(\mathbf{w}_{1}^{\top} \mathbf{o}_{i}\right)}{\sum_{j} \exp \left(\mathbf{w}_{1}^{\top} \mathbf{o}_{j}\right)}, \quad p_{i}^{2}=\frac{\exp \left(\mathbf{w}_{2}^{\top} \mathbf{o}_{i}\right)}{\sum_{j} \exp \left(\mathbf{w}_{2}^{\top} \mathbf{o}_{j}\right)}
  $$

Loss 函数：
$$
\mathcal{L}=-\frac{1}{N} \sum_{j=1}^{N}\left(\log p_{y_{j}^{1}}^{1}+\log p_{y_{j}^{2}}^{2}\right)
$$

#### 知识库编码

本文使用WordNet和NELL两个知识库，每个知识库都以 <subject, relation, object> 的三元组形式存储。

给定三元组<s, r, o>，我们想要学习它们三个对应的embedding，然后就可以通过函数 $f(s, r, o)=\mathbf{s}^{\top} \operatorname{diag}(\mathbf{r}) \mathbf{o}$ 计算其有效性（ $\mathbf{s}, \mathbf{r}, \mathbf{o} \in \mathbb{R}^{d_{2}}$ ）。其中在知识库中存在的三元组会具有更高的有效性得分。本文采用 [Yang 等人](https://arxiv.org/pdf/1412.6575.pdf) 工作中的方法，使用 margin-based ranking loss 学习每个实体和关系的表达。

#### 相关概念检索

对于WordNet知识库：对给定的词，返回其同义词集作为相关概念集合。

对于NELL知识库：首先识别给定passage和question中的命名实体，然后使用字符串匹配的方法用识别出的命名实体去检索相关的NELL concepts作为相关概念集合。（注：同一个命名实体中的所有词共享相同的相关概念集合）



### Experiment-实验

**数据集**：ReCoRD、SQuAD1.1

预训练的KB Embedding：本文采用 [Yang and Mitchell](https://arxiv.org/pdf/1902.09091.pdf) 等人发布的预训练好的100维 KB Embedding，在MRC任务上训练时是固定的。

预训练的BERT模型：本文采用 cased_large_BERT model，在MRC任务上训练时是可调的。

**实验一：**

<img src="https://i.loli.net/2020/01/03/BWasvzAUfKgD1HP.png" alt="image-20200103221214398.png" style="zoom:40%;" />

<img src="https://i.loli.net/2020/01/03/V9NCBGKuEl8t3wd.png" alt="image-20200103221302885.png" style="zoom:40%;" />

- KT-NERT 与 BERT 的结果比较，说明加入知识库的确可以提高性能。
- WordNet 和 NELL 两个知识库都可以提高BERT对MRC任务的性能，但具体哪种融合方式更好是视数据集情况而定的。

**实验二：消融实验 —— Question/Passage 表达**

<img src="https://i.loli.net/2020/01/03/bEG2rkQZv97sPNA.png" alt="image-20200103222604993.png" style="zoom:50%;" />

- 对BERT模型，一个passage word和所有的question word有相似的匹配度。说明BERT模型在MRC任务上经过微调后，对每个question word有相似的表示，该表示可能编码了整个question的含义。
- 但KT-NERT模型的结果表明，融合了知识后可以得到更正确的表示，从而更好地匹配question和passage。



### Highlight

本文提出了KT-NET模型，使用外部知识库进一步增强预训练的语言模型，从而提高在MRC任务上的性能。