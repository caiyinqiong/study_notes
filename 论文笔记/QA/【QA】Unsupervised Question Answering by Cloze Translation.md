> > ACL2019，无监督的MRC数据集生成

# Unsupervised Question Answering by Cloze Translation

论文作者：Patrick Lewis, Ludovic Denoyer, Sebastian Riedel
论文单位：Facebook AI Research
论文地址：https://arxiv.org/pdf/1906.04980.pdf
代码地址：https://github.com/facebookresearch/UnsupervisedQA
论文类别：方法



### Motivation-论文解决了什么问题

本文是为了解决对部分领域/语言，收集大规模的Extractive QA数据集不可行的问题。因此提出了一种无监督的方法来自动合成EQA的训练数据。



### Motivation-本文的方法思路

![image-20191226214409739.png](https://i.loli.net/2019/12/27/SBpZqRQEilIO9c3.png)

使用三步来生成EQA训练数据：

- 从目标域随机采样一个paragraph作为context（例如，English Wikipedia）
- 使用预训练的方法组件（NER、名词识别器）从context中采样一组candidate answers（名词短语、命名实体等），然后根据context和candidate answers形成完形填空问题
- 采用无监督的**完形填空问题-自然问句转换器**把完形填空问题转化成自然语言问题

>本文的关键就在于第三步，使用一个完形填空问题到自然语言问题的翻译器来进行问题转换。



### Method-模型概述

#### 文本生成

给定文档集合，从任何文档中均匀采样出一个合适长度的段落c。

#### 答案生成

融合一些先验，从段落c中生成答案a。

1. 名词短语：借助chunking algorithm，从段落c中抽取所有的名词短语，然后从该集合中均匀采样来生成候选答案。

2. 命名实体：借助NER系统，从段落c中提取所有的命名实体，然后从该集合中均匀采样。但这样的限制会降低问题的多样性。

#### 问题生成

- **完形填空问题生成**

  完形填空问题限制为答案所在的子句，且答案被mask掉了。（可参考图中的 $q^\prime$）

- **完形填空问题到自然语言问题的转换** （本文探索了4种转换方法）

  1. Identity Mapping

     随机选择或者用简单的启发式规则选择一个Wh*词来代替被mask掉的答案。

  2. Noisy Clozes

     删除完形填空问题中被mask掉的词，再对其使用一个简单的噪声函数，然后在句首添加一个wh*单词(随机选择或简单的启发式规则）和在句尾添加一个问号。其中噪声函数包括word dropout、word order permutation、word mask三个操作。

  3. Rule-Based

     本文使用一个现有的[陈述句-问题生成器](https://www.aclweb.org/anthology/N10-1086.pdf)，基于一组规则生成最佳候选问题。

  4. Seq2Seq

     - 完形填空问题语料收集：依照上述方式，将名词短语和命名实体视为答案，完形填空问题是包含答案的句子或从句。最终从Wikipedia中随机采样5M个完形填空问题，形成语料C。（当存在答案实体类型信息时，则使用特定类型的mask词，包括PERSON/PLACE/THING/TEMPORAL/NUMERIC）
     - 自然语言问题语料收集：从英文网页中爬取以Wh*开头，并以问句形式结尾的句子。最终获得100M+ 个英文问句，并采样出的5M个问句形成语料Q。
     - 翻译模型训练：采用无监督机器翻译的方法，使用语料C和Q训练 $p_{s \rightarrow t}\left(q | q^{\prime}\right)$ 和 $p_{t \rightarrow s}\left(q^{\prime} | q\right)$ 。其中模型的训练结合使用了降噪自编码（denosing auto-encoding）和在线回译（online back-translation）技术。

  >Wh* 启发式
  >
  >本文使用一种简单的启发式规则，以将每个答案类型映射到最合适的Wh单词。比如，答案类型为 ”TEMPORAL“，则映射为”when“。除了 Identity Mapping 和 Noisy Clozes 转换方法使用Wh启发式规则外，UNMT在训练时也采用Wh启发式方法。模型训练时，在目标问句的首部添加Wh所映射的答案类型。比如，问句以”when“起始，则在句首添加“TEMPORAL“。

#### 问答

上述的**文本-答案-问题生成**相当于一个无监督的生成模型。通过该生成模型，有2种方式得到抽取式问答的答案，

- 使用现有的QA模型训练一个单独的QA系统：本文的数据生成模型为QA模型提供训练数据。
- 使用后验概率：最大化答案生成该问题的后验概率 $\arg \max _{a^{\prime}} p\left(q | a^{\prime}, c\right)$ ，从而选出答案。



### Experiment-实验

**实验一：无监督的QA实验**

采用finetuning BERT 和 BiDAF+Self Attention 作为QA模型，使用本文提出的无监督生成模型得到的数据进行训练，然后在SQuAD test set上进行评估。

![image-20191227142447729.png](https://i.loli.net/2019/12/27/yWEFZwmSctXhn2b.png)

- 结果表明，本文提出的无监督训练方法的性能显著超出baseline及其他一些无监督模型，甚至一些早期的监督模型。

**实验二：Ablation Studies**

![image-20191227142824441.png](https://i.loli.net/2019/12/27/ES54Q1heVUKajXP.png)

- 最大后验概率方法与BERT-base和BiDAF+SA的结果对比说明，在单独的QA模型上训练比采用最大问题似然更有效。
- 在答案生成时，使用命名实体比名词短语更有效。
- 在完形填空问题生成时，使用从句比使用整个句子更有效。
- 在自然语言问题转换方式上，UNMT > Noisy Cloze > Identity > Rule-Based。



**实验三：少样本学习**

![image-20191227153208061.png](https://i.loli.net/2019/12/27/PeW3BuaHzFVDsd6.png)

- 实验结果表明，使用本文的方案先进行预训练，然后在极其少量的数据集上进行微调，即可取得很好的效果。



### Highlight

- 本文提出了一种无监督的方式来帮助生成EQA数据集。但是由于答案生成部分借助的先验是命名实体，所以个人觉得，生成的问题还是比较局限性的。
- 而且Ablation Studies中的部分结论，只是因为某些实验方案导致生成的问题分布更接近SQuAD的问题分布，或者更有利于后面在SQuAD上的评估。（比如SQuAD中有52.4%的答案都是命名实体）
- 不过总体而言，本文提出的无监督数据集生成方法，还是可以很好地帮助进行模型进行预训练。也容易在工业上落地。





- $q^\prime$ 是指完形填空问题，$q$ 是指自然语言问题。

- 关于c和q的对应关系：因为本文提出的生成数据集的流程是，先采样一个段落c，再从c中选择答案片段a，然后根据a和c生成完形填空问题 $q^\prime$ ，再通过一个机器翻译模型把完形填空问题 $q^\prime$ 转换成自然语言问题  $q$ ，即可得到<c,a,q>三元组，即c和q的对应关系是可得的。

- 关于翻译模型的训练，论文中没有具体的描述，大概是因为denosing auto-encoding和back-translation技术是无监督机器翻译中比较成熟且常用的方法了吧。具体可参考论文 [Phrase-Based & Neural Unsupervised Machine Translation](https://arxiv.org/pdf/1804.07755.pdf)，中文资料可参考 https://blog.csdn.net/Fly_TheWind/article/details/89927742。

  大概流程是：为了便于描述，我们以完形填空问题 $q^\prime$ 作为源语言，以自然语言问题 $q$ 作为目标语言。先用降噪自编码技术可以训练得到单语模型 $s \rightarrow s$ 和 $t \rightarrow t$ 。然后进行反向翻译过程，即，对于一个源语言输入 $q^\prime$ ，先用 $s \rightarrow t$ 翻译模型将其翻译成 $q$ ，然后用单语模型 $t \rightarrow t$ 对得到的 $q$ 进行校正，使其更符合语法和规范化，然后用 $t \rightarrow s$  翻译模型将校正后的 $q$ 翻译成源语言句子 $q^{\prime\prime}$ ，此时 $q^\prime$ 就可以作为这一过程的 ground truth，与 $q^{\prime\prime}$ 一起计算loss，从而进行模型的参数优化。

  

  

  

  

  
  
  
  
  
  
   















