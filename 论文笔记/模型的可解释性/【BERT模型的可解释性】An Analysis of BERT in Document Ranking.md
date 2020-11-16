> > SIGIR2020，BERT用于passage re-ranking的可解释性分析

# 分析实验

##### 数据集

MSMARCO passage ranking

##### 分析结果

- [CLS]/[SEP]/标点符号词由于他们的高df值，得到了多余的attention weights。

- 底层（前两层）的交互不重要，query和document在学习他们自己的表示；中间层捕捉交互信号在学习上下文特定的表示；在最后几层，严重依赖交互来预测相关性分值。

  <img src="../../images/image-20200923142854083.png" alt="image-20200923142854083" style="zoom:50%;" />

- 很多document的信息被attention到query token上，但是document并没有得到很多query的信息，说明可以学习一个representation-focused模型，离线计算doc表示。

