# [MRC ACL2019] Multi-hop Reading Comprehension through Question Decomposition and Rescoring

```
论文作者：Sewon Min, Victor Zhong, Luke Zettlemoyer, Hannaneh Hajishirzi
论文单位：University of Washington
论文地址：https://arxiv.org/pdf/1906.02916.pdf
代码地址：https://github.com/shmsw25/DecompRC
论文类别：方法
```



### Motivation-论文解决了什么问题

本文主要解决**多跳阅读理解**需要跨多段落进行推理和合并的问题。



### Motivation-本文的方法思路

本文提出了 **DecompRC模型**，核心思路是把组合问题分解成简单的、可以被现成的单跳RC模型解决的子问题。

DecompRC模型 回答问题的步骤是：

- 基于span预测，根据几种并行的推理类型，将原始的多跳问题分解为多个单跳子问题。
- 对每一种推理类型的分解结果，利用单跳RC模型回答每个子问题，并根据推理类型组合得到答案。
- 利用重打分函数判断哪种分解是最合适的，并将该分解的答案作为最终答案。

> 其中，推理类型包括 bridging、intersection、comparison。



### Method-模型概述

基于以上步骤，**DecompRC模型**的整体工作流程如下图：

<img src="https://i.loli.net/2020/01/03/xDfGHEC2mJqK8oQ.png" width="600" hegiht="400" align=center />

#### 问题分解

本文把子问题生成过程简化成**span预测问题**，使用400个标注样本训练得到一个完整的分解模型。其核心思想是，每个子问题都是通过复制并稍微编辑原问题中的一个关键片段形成的，且对不同的推理类型，有不同的片段抽取和编辑需求。

该模块的核心是训练一个 **$Pointer_c$ 模型**，输入是长度为n的原问题词序列 $S = \left[s_{1}, \dots, s_{n}\right]$ ，输出是c个位置点。基于这c个位置点，通过算法1就可以得到对应推理类型的子问题分解结果。

<img src="https://i.loli.net/2020/01/03/5zYM9p7ZFtd4IxB.png" width="400" hegiht="400" align=center />


其中， $Pointer_c$ 模型的计算过程为，

- 基于BERT得到序列S的编码：
  $$
  U=\mathrm{BERT}(S) \in \mathbb{R}^{n \times h}
  $$

- 计算得到 pointer score matrix：
  $$
  Y=\operatorname{softmax}(U W) \in \mathbb{R}^{n \times c}
  $$
  其中，c是一个超参数，对于不同的推理类型，设置不同的值。

- 得到联合概率最大的c个位置点作为输出：
  $$
  \mathrm{ind}_{1}, \ldots, \mathrm{ind}_{c}=\underset{i_{1} \leq \cdots \leq i_{c}}{\operatorname{argmax}} \prod_{j=1}^{c} \mathbb{P}\left(i_{j}=\mathrm{ind}_{j}\right) \\
  \mathbb{P}\left(i=\operatorname{ind}_{j}\right)=Y_{i j}
  $$

#### 单跳RC模型

单跳RC模型的目标是，对给定的子问题和N个段落 $S_{1}, \ldots, S_{N}$  ，计算得到answer和evidence。其中answer是来自某个段落的一个span，evidence是answer所在的段落。

对每个子问题，针对每个段落 $S_i $ 计算四个分数 $y_{i}^{\text {span }} y_{i}^{\text {yes }}, y_{i}^{\text {no }} \text { and } y_{i}^{\text {none }}$ （分别表示答案是否是该段落中的一个片段、YES、NO 或不存在），该子问题的最终答案是从具有最低的  $y_{i}^{\text {none }}$ 的段落选出的。基于每个子问题的答案，根据推理类型，组合得到在该种分解下原问题的答案。

该模块可以采用任何现成的单跳RC模型，本文采用的方法如下，

- 把某个子问题和段落 $S_i$ 串接起来，基于BERT得到他们的编码： 
  $$
  U_{i} \in \mathbb{R}^{m \times h}
  $$

- 计算在段落 $S_i$ ，四种答案的分数：
  $$
  \left[y_{i}^{\text {span }} ; y_{i}^{\text {yes }} ; y_{i}^{\text {no }} ; y_{i}^{\text {none }}\right]=\max \left(U_{i}\right) W_{1} \in \mathbb{R}^{4}
  $$
  其中，max表示max_pooling操作。

- answer span 的起止位置计算：
  $$
  \begin{aligned} p_{i}^{\text {start }} &=\operatorname{softmax}\left(U_{i} W_{\text {start }}\right) \in \mathbb{R}^{n} \\ p_{i}^{\text {end }} &=\operatorname{softmax}\left(U_{i} W_{\text {end }}\right) \in \mathbb{R}^{n} \end{aligned} \\
  \operatorname{start}_{i}, \text { end }_{i}=\underset{j \leq k}{\operatorname{argmax}} p_{i}^{\text {start}}(j) \  p_{i}^{\text {end}}(k)
  $$

> 该模型是使用single-hop QA dataset进行预训练的，该数据集由SQuAD数据集及Hotpot数据集中单跳可解样本组成。预训练结束后，在解决多跳问题时参数是固定的。

#### 打分函数

每种分解包括对应推理类型 $t$ 下得到的子问题、该分解下得到的对原问题的 $answer_t$ 及 $evidence_t$ ​。打分函数用于判断哪种分解方式是最好的，并把最好的分解方式下得到的答案作为最终输出答案。

打分函数的计算方式如下，

- 把子问题、推理类型、answer、evidence 串接起来，基于BERT得到他们的编码：
  $$
  U_{t} \in \mathbb{R}^{l \times h}
  $$

- 计算对该分解方式的打分：
  $$
  p_{t}=\operatorname{sigmoid}\left(W_{2}^{T} \max \left(U_{t}\right)\right) \in \mathbb{R}
  $$

> 在推断阶段，将具有最大的 $p_t$ 的推理类型确定为最终推理类型，其对应的答案作为最终答案。



### Experiment-实验

**数据集**：HotpotQA（该数据集中的问题分为bridge和comparison两种推理类型，其中 dev set 和 test test 分为Distractor setting 和 Full wiki setting 两种不同的设置 ）

**实验一：在Hotpot上的实验**

<img src="https://i.loli.net/2020/01/03/vYRt2hEZeTDBfNj.png" width="500" hegiht="500" align=center />

<img src="https://i.loli.net/2020/01/03/XDNya7tJGElMbpI.png" width="300" hegiht="200" align=center />

- 在两种不同的设置下，DecompRC模型比其他baseline表现更好。
- 虽然BERT在单跳阅读理解数据集上训练获得了很高的F1分数，但在单跳不可解的样本中，DecompRC模型的性能远优于BERT。

**实验二：消融实验**

1. 子问题生成（span预测）模块

   <img src="https://i.loli.net/2020/01/03/t3Z2JUBWu9YaHPm.png" width="300" hegiht="400" align=center />

   - 实验结果表明，基于span预测的子问题生成和人工编写的子问题一样有效。
- 通过少量标注数据即可得到效果比较好的span预测模型，主要是因为BERT通过语言建模学习到了句子的语法信息。
  
2. 打分函数（分解方式决策）

   <img src="https://i.loli.net/2020/01/03/sMTKQRCtAO1XvNU.png" width="400" hegiht="400" align=center />

   > Pipeline方法：
   >
   > 根据问题S的编码 $\tilde{S} \in \mathbb{R}^{n \times h}$ ，计算问题所属推理类型的概率：
   > $$
   > p_{t}=\operatorname{softmax}\left(W_{3} \max (\tilde{S})\right) \in \mathbb{R}^{4}
   > $$
   > 4维向量 $p_t$ 分别表示问题S属于bridging, intersection, comparison 和 original 的概率。

   - Oracle方法提供了该模块的性能上界。实验结果说明，DecompRC模型在该模块的性能还有上升空间。
   - Pipeline方法较差的结果说明，DecompRC模型使用分解之后得到的信息（answer、evidence）可以帮助更好地决策最佳分解方式。



### Highlight

- 本文提出的DecompRC模型可以把多跳组合问题分解为简单的单跳子问题，进而可以通过解决这些子问题来得到原问题的答案。
- 但本文还存在一些局限性，比如一些问题并不是容易分解的（甚至不可分解），或者其推理类型不在本文所讨论的推理类型内。

