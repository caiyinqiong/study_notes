> >AAAI2018，抽取式QA、R^3、SR^2

# 思路

**R^3:**

提出ranker模块，而且基于强化学习联合训练ranker和reader。

ranker模块计算每个文档和query的相关性，根据相关度采样出一个passage输入到reader中。

reader模块对输入的一个passage计算answer span，根据与ground truth的指标作为reward，来指导ranker模块的训练。

**SR^2:**

ranker模块不是基于强化学习进行训练，而是分别训练ranker和reader模块。其中ranker模型用远程监督的方式训练（含有answer的视为正例passage，然后用KL散度损失函数。）



# 模型

用match-LSTM模型来表示passage和query。
ranker: 对每个passage得到一个固定长度的向量表示，再处理得到每个文档的相关性值。
reader: 一般的抽取式的阅读理解模型。



# 实验

数据集：Quasar-T、SQuAD(open)、WikiMovies、CuratedTREC、WebQuestion

对wikipedia文档构建的是基于sentence-level的索引。



# 启发、结论

用基于强化学习的方法联合训练ranker和reader，可以解决passage没有标签的情况以及用远程监督带来的噪声问题。