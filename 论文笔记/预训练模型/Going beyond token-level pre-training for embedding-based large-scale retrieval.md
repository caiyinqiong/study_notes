> > ICLR2020

## Motivation——论文解决什么问题

对于大规模的信息检索任务，一般是分为两步去做：从大规模语料中先检索出top-K相关的文档；再对top-K的文档做reranking，进行一个最终的排序。对于reranking任务，Bert类的模型已经有了很大的提升，参考MSMARCO任务，但是第一步检索目前来看除了BM25，还没有什么特别的进展。召回率和速度通常是判断第一步retrieval效果的指标，作者提出使用预训练的方法来做retrieval，结果表明比BM25效果要好很多。

## Motivation-本文的方法思路

本文使用两个类似bert的transformer encoder 模型，分别抽取query和document的embedding，这么做的好处是可以离线计算，提前把document的表达计算并索引，适合大规模retrieval的场景。然后使用合适的预训练任务，包括逆完形填空ICT，文章第一句话选择BFS和维基链接预测WLP，进行预训练，最后在Retrieval Question-Answer benchmark上进行验证。

## Method-方法概述

### 模型：

模型如下图所示：

[![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/fa64fce6fc981cb3f8122b9fb20dd1f333099189_2_690x373.png)](http://forum.deepaccess.cn/uploads/default/original/1X/fa64fce6fc981cb3f8122b9fb20dd1f333099189.png)

上图左是two-tower model，这种模型很常见，像Bert一样，作者使用Bert-base的结构，12层的transformer，embedding的维度变成了512而不是768，使用Bert的方法，利用[CLS] 的输出，进行一次pooling作为最终的输出。这种方法的好处是可以query和document可以独立进行编码，可以离线计算。右边的结构每一个document 和query都要重新计算一次，大大浪费了时间。

### 预训练任务

[![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/4f92237559af252afb07a1a563edb0a78941e543_2_690x414.png)](http://forum.deepaccess.cn/uploads/default/original/1X/4f92237559af252afb07a1a563edb0a78941e543.png)

1. Inverse Cloze Task(ICT)：给定一个维基百科的文档，P是文档中固定长度的passage，从该passage中随机选择一个sentence作为query，其余作为document组成q-d正例。该任务可以捕捉句子的语义上下文(一个document，一个passage，不同句子之间)。Note: 作者再训练的时候doc部分将文档的title拼到了一起
2. Body First Selection(BFS):由于维基百科的特性，每一个文档的第一段一般是具有概括意义的，可作为综述段，从第一段中随机选择一个sentence作为query，从该文档的其余随机选择一个passage作为d，组成q-d正例。该任务可以捕捉不同local passage之间的语义关系(一个document的，但是不同的passage)。
3. Wiki Link Prediction(WLP): query是维基百科一个文档中的第一段的随机一个sentence，但是document是该文档的链接所指向的文档中的随机的一段，组成q-d正例。该任务可以捕捉不同文档之间的语义关系（两个document之间）
4. Masked LM(MLM): Bert中的掩码的语言模型

### 构造的训练数据的统计：

![image](http://forum.deepaccess.cn/uploads/default/original/1X/399803292b5b34673f1094c46e24c9f0e5ac8dd2.png)

### 训练的方法

本质是一个分类任务，使用最大似然方法进行训练，但是loss function是beyond pair-wise方法的但还没达到list-wise的级别。

使用采样的softmax方法，softmax的分母是在一个mini-batch内的所有document，而不是整个语料库的所有文档，大大加快了训练的速度。
query: 64
document: 288
batch size: 8192
learning rate: 1e-4
warm-up ratio: 0.1
在32个TPU V3(64G) 训练了10w步,花费了2.5天

## Experiment-实验

数据集： 预训练任务使用Wikipedia，下游任务使用ReQA benchmark，原始的ReQA是(query，answer，passage), 作者首先将此转换为(query,sentence within answer,passage), 然后给定q，需要retrieval出(S,P),而并不仅仅是P出来。这个不算是open domin-qa，

#### 主实验

在实验设置中，作者将fine-tune的数据集的训练集和测试集按照不同的比例构造，目的是为了测试模型的泛化能力。

[![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/9d700f3765116625e42a62f2c201cd7e6488ebf1_2_690x432.png)](http://forum.deepaccess.cn/uploads/default/original/1X/9d700f3765116625e42a62f2c201cd7e6488ebf1.png)

[![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/9e5632f6795a3cd40262fc88a930c66f1fbe710f_2_690x438.png)](http://forum.deepaccess.cn/uploads/default/original/1X/9e5632f6795a3cd40262fc88a930c66f1fbe710f.png)

可以看出来使用了作者提出的三个预训练任务的模型泛化能力很强，在正常的训练测试中也是最好的。

其中No-SSL和MLM都是two-tower结构，一个是没有预训练的结构，一个是只是用MLM预训练的结。（题外话，No-SSl这个效果差主要是因为深层的transformer难以训练),倒数三个结构用来说明作者的三个预训练任务的有效性，感觉应该加上一个bert-base的baseline，但是作者没有，不知道为什么。

Bm25的有效主要是因为数据集的问题和答案的overlap很高导致的。

MLP这个结构最奇怪，作者拿了个最简单的MLP模型作为baseline，可能是因为这个结构虽小，但是很快，应该现实中也没有人这么用，所以这个baseline也显的很奇怪。

#### 消融实验

[![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/c8a1382140e9268c54005fae6d13251e45377016_2_690x325.png)](http://forum.deepaccess.cn/uploads/default/original/1X/c8a1382140e9268c54005fae6d13251e45377016.png)

消融实验主要用来检查模型的层数，预训练任务和最后的embedding的维度。

结论是12层最好，ICT>BFS>WLP, embedding的维度512，但是作者没有测试更高的维度。

#### open-domin实验

[![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/1bdc7a98882aa55b05b4025e592f5fea9f3010c2_2_690x283.png)](http://forum.deepaccess.cn/uploads/default/original/1X/1bdc7a98882aa55b05b4025e592f5fea9f3010c2.png)

作者采用完全的open-domin setting，利用DrQA，预处理然后下采样得到1百万的候选集合，加入到原始的候选集合中

## Highlight

- 本文首次提出了预训练retrieval场景下的方法，该方法在open-domin qa场景下证明有效。

- 但是在所有文档的离线计算中也会耗费大量的时间。

