> >ACL2019



### **Motivation-论文解决什么问题**

现在人们越来越关注深度学习的可解释性，这已经成为社区很重要的主题。由于很多的分析都是基于注意力机制的，*假设注意力机制可以区分出信息的重要性*，但是尚没有几个关于注意力机制是否能为模型的输出做出很好的说明和解释的研究（2019 NAACL上有一篇 [Attention is not Explanation](https://www.aclweb.org/anthology/N19-1357.pdf)），所以**本文通过在已经训练的文本分类模型中操作注意力权重并分析其预测结果的差异，来检验该假设是否成立。**



### **Motivation-本文的方法思路**

本文的方法是通过修改模型的注意力层的某个或某些注意力权重，接着使用两种方法分析结果的差异。
如下图所示，对于已经训练好的模型的注意力层，一个是未修改的，一个是把某些权重（特别大的）置为0，然后重新进行归一化，两个模型都会有输出的logit和argmax之后的prediction。

[![Selection_317](http://forum.deepaccess.cn/uploads/default/optimized/1X/c41367a9a5bcf0676491e2ffca5bd0c770394943_2_378x375.png)Selection_317600×595 63 KB](http://forum.deepaccess.cn/uploads/default/original/1X/c41367a9a5bcf0676491e2ffca5bd0c770394943.png)

本文主要采用两个方法进行分析：

- 通过计算Jensen-Shannon（JS）散度来分析p和qI’,上图中的softmax的输出的不同，
- 分析argmax之后的softmax的预测结果是否有不同



### **Method-模型概述**

本文的分析主要使用文本分类的模型和数据集，使用了非常经典的结构 [HAN](https://www.aclweb.org/anthology/N16-1174/)
![Selection_318](http://forum.deepaccess.cn/uploads/default/original/1X/25ba96c2211253e1e880fb687697d7e9fada8a31.png)



### **Experiment-数据集**

实验所用数据集主要使用文本分类的四个典型的数据集 Yahoo Answers， IMDB， Amazon， Yelp

[![Selection_319](http://forum.deepaccess.cn/uploads/default/optimized/1X/32290172f9d9cb007bf4dc746f1006944d6614b4_2_690x154.png)](http://forum.deepaccess.cn/uploads/default/original/1X/32290172f9d9cb007bf4dc746f1006944d6614b4.png)

**Experiment-结果和分析**

**1.单个权重重要性分析**
第一组实验主要分析单个注意力权重的重要性，实验主要是将注意力层中最大的注意力权重置为0，然后重新归一化其他的权重；将其他的某一个注意力权重置为0，然后在进行重新归一化。这两种进行对比，公式如下（不知道为什么不和原始的对比，大概是没法画图了）

![image](http://forum.deepaccess.cn/uploads/default/original/1X/b64646250996f3b9290c890187f3200ea5427fc5.png)

- JL 散度分析，差异越大说明注意力的作用越大

[![Selection_320](http://forum.deepaccess.cn/uploads/default/optimized/1X/2b170a9c2465b81ce3581f802ac518dfa17971ef_2_345x375.png)](http://forum.deepaccess.cn/uploads/default/original/1X/2b170a9c2465b81ce3581f802ac518dfa17971ef.png)

上图的横坐标是最大的权重 `i*` 和 `r`之差，纵坐标即 `△ JS`

![image](http://forum.deepaccess.cn/uploads/default/original/1X/7ce0fa0eb3d1806c11fd8576e5e2634550fd3cbb.png)
此图的横坐标是最大的权重 `i*` 和 `r`之差，纵坐标是 `△ JS`的数量，是第一个图的具象版，所以可以看出来基本上绝大多数的在横坐标上0.4之前的`△ JS`都是0，只有当横坐标很大，到1的时候，差异才会体现出来，那基本是整个注意力权重分布都在一个神经元身上

第二组实验是看预测是否翻转
![image](http://forum.deepaccess.cn/uploads/default/original/1X/beced882f7d50b313439e0c89b988a8a050c477a.png)
上图也是一个对比图 很抽象，意思是说将最大的`i*` 置为0 是否导致最后预测结果翻转和将随机的`r`置为0是否会导致最后的结果翻转的频率统计。可以看出(No，No)的占的最高，也就说将某个权重置为0，不管是最大的还是别的，都不能导致结果改变。

**2.一组权重重要性分析**

思想就是我们期望在权重排名很高的那些能带来更多的结果翻转，如果不能或者说较低的带来更多的结果翻转，也就证实了模型预测的结果和注意力权重没有关系。
实验方法： 首先有一个排名，注意力大小的、梯度的、随机的、注意力和梯度结合的，从排名最高的位置开始删除表达，直到模型的决策发生变化为止。

[![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/e43415e41d5480ceccdbb2faab93530e0c4642ba_2_630x500.png)](http://forum.deepaccess.cn/uploads/default/original/1X/e43415e41d5480ceccdbb2faab93530e0c4642ba.png)

三种模型结构HANrnns，FLANrnns, FLANconvs在四个数据集上，在四种不同的ranking方法上的箱型图结果。



### **Highlight**

从本文做的两组实验来看，注意力并不能和可解释性相关联，也无法表达出信息重要性这一信号。与前文工作的不同在于主要是关注在较低的注意力权重可解释性上