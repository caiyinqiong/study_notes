> > 2019

# 分析实验

##### 数据集

##### 实验设置

##### 分析结果

- bert attention head的行为具有common pattern，例如attend to特定的位置偏差（next token或者previous token），或整个句子
- 大量的bert attention集中在[SEP] token，本文证明这是一种无操作(no-op)
- 同一层的head通常具有相似的behave
- 特定的head明显对应特定的关系，例如句法的特定方面



这篇文章主要非常详实地描述了BERT base Attention一些行为，发现不同的attention heads有一些有意义的行为。

1. 关注点在哪里?
   首先他们对每一层的每一个attention head行为进行了研究，发现可以根据attention heads 一般关注哪里进行分类，有一部分heads关注力比较分散,对句子中的每个词都进行了关注(初期和CLS最后); 中间层早期的heads把大部分的注意力集中到了下一个token. 而中间层后部分大部分的heads都把注意力放在了[SEP] 这个token上。这么多的关注力聚焦在[SEP]这个比较artificial token上是为什么呢? 可能是这是信息综合处；也可能这代表[SEP]是一个冗余信息垃圾桶。 为了测试这个两种可能性，研究员们分别采取了两种测量。1)测量[SEP]这个token是否把attention 放在了多个词上，如果这真的是一个信息综合处，那么[SEP]应该会的注意力会更均一地分布在不同词上。但他们发现[SEP]反而把大部分注意力都放在了自己本身这个token上。 2) 他们计算了masked language modeling 这个任务对于attention weights 的gradient loss。这个主要用来衡量,attention 的改变是如何影响performance的改变。在那些attention集中在[SEP]上的层上, gradient loss非常小，也就是attention的改变并不非常大的影响模型的output.
2. 