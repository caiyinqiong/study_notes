> > ACL2019，多跳MRC

## 论文解决了什么问题

目前来看，机器和人的阅读理解之间存在的差距主要有3点：

- Reasoning ability：单跳QA模型主要靠与question的匹配来寻找答案，实际并没有包含复杂的推理。因此，多跳QA是下一个要攻克的难题。

- Explainability：显式的推理路径对于判断QA系统的可靠性是非常重要的。HotpotQA数据集要求模型提供supporting sentence，这是无序的、句子级别的可解释性，但是人类能够对答案有一个有序的，实体级别的解释能力。
- Scalability：对于实际应用的QA系统，可扩展性是不可或缺的。目前的MRC一般都按照DrQA的范式，先检索再抽取答案，这是对单跳QA和可扩展的信息检索之间的一个折中方案。 但是对于人类，能够从巨大容量的记忆中轻而易举地推理出答案。

人的认知过程理论（**Dual process theory**）：

- 大脑对信息的认知分成 system 1 和 system 2两个阶段。
- system 1: 通过隐含的，无意识的，本能的注意力检索相关的信息，根据需求提供资源。
- system 2: 通过显式的，有意识的，可控的推理过程，在working memory中进行更深层次的探索和挖掘。
- 对于复杂推理，这两个系统迭代地协作完成 fast and slow thinking.



## 本文的方法思路

- 基于人的认知理论，本文提出了**Cognitive Graph QA (CogQA)**的框架：
  - System 1 从段落中抽取和question 相关的实体与候选答案，并编码他们的语义信息。抽取出的实体组织成 cognitive graph， 和 dual process theory 中的 working memory 类似。 
  - system 2 在基于认知图进行推理，收集线索并指导 system 1更好地抽取下一跳的实体。
  - 这一过程迭代进行，直到所有可能的答案都被发现后，基于system 2的推理选出最终答案。

- 针对于前文提出的三个挑战，本文的工作：
  - 推理性在于基于认知图的节点表示逐步地推理、得出答案；
  - 解释性在于认知图上显式的推理路径提供了有序的、实体级别的解释能力；
  - Scalability在于QA的时间开销并不会随着段落数的增长而显著增加。



## 模型/方法概述

认知图的每个节点是一个实体或者可能的答案
BERT 作为 System 1, GNN作为System 2， clues[x, G]是x的前置节点的段落句子

[![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/6ed62571e68833bd3549de11992156a47ea0b2d3_2_690x449.png)](http://forum.deepaccess.cn/uploads/default/original/1X/6ed62571e68833bd3549de11992156a47ea0b2d3.png)

**System 1**：

![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/7e7744f75b5c03513e40b5e70af27595261e2c23_2_345x53.png)

用BERT第三层到最后一层在位置0的输出作为sem[x, Q, clues] 的语义表达。
**System 2** 的一个功能是产生clues[x, G], 按照之前的描述，简单地获取x的前置节点段落的句子。另一个功能是更新X的隐层表示，以深入理解问题和实体x之间的关系，做进一步的推理，这里用了GNN的一个变体：
<img src="http://forum.deepaccess.cn/uploads/default/original/1X/a666bf99374418ee3f03230b97cf21fd7e0f3cbb.png" alt="image" style="zoom:50%;" />

A是图G的邻接矩阵，<img src="http://forum.deepaccess.cn/uploads/default/original/1X/2843a6569190a65fc1d089fd6b0a3c9fd8f8e06a.png" alt="image" style="zoom:50%;" /> 



## Experiment-实验

数据集：full-wiki setting of HotpotQA
实验结果：

![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/94a80b0d34f74802f59ebd438d39ec6718d22591_2_690x262.png)



## Highlight

- system1较为基础，考虑语义信息，system2 用GNN考虑推理实体间的关系。

- 总体来看，system1是负责构建图，而且基于BERT得到每个节点的初始表示；system2使用GNN在图上进行推理。同时，system2还可以提供反馈信号，供system1扩展图。。如此迭代。。来模拟人的认知过程。
- 它比其他基于实体构建图进而进行推理的方法，优势可能在于有与system1交替迭代的过程。