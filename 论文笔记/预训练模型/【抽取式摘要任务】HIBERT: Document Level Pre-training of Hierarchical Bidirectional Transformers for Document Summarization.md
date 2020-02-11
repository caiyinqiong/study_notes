> > ACL2019



## Motivation-论文解决什么问题

生成式摘要大都基于Seq2Seq，尽管采用了copy机制、coverage model、RL等技术，其生成的摘要在语法、语义上仍没有保证。抽取式摘要目前看来更可靠。

抽取式摘要通常建模为（带长度限制的）句子排序问题。目前普遍做法是：先用一个hierarchical LSTM编码文档，再用另一个LSTM预测每个句子的二元标签。抽取式摘要的所需的二元标签并不包括在大多摘要数据集中，因此通常采用一些基于规则的方法获得。例如：选择句子集合，使其与reference summaries的ROUGE值最大化。这样得到的标签可能不准确，为复杂模型的训练带来了极大挑战。

在initial experiment中我们发现，在第二个epoch后，我们的模型在训练集上快速过拟合。这说明，训练集很可能没有被充分利用。



## Motivation-本文的方法思路

将LSTM替换为了transformer。因为后者近期在多个任务上表现比LSTM更好。

先对复杂的部分（hierarchical encoder）用无标注数据进行预训练，再以此为基础通过句子标签二元分类微调。提出了一种hierarchical document encoder的预训练方式，而不是以预训练word embedding或sentence encoder为目标。前者对摘要任务是十分重要的。



## Method-模型/方法概述

### 预训练

[![img](http://forum.deepaccess.cn/uploads/default/optimized/1X/1c499b33baea43b31d1c1739190809942d9d692e_2_387x500.jpeg)1000×1290 483 KB](http://forum.deepaccess.cn/uploads/default/original/1X/1c499b33baea43b31d1c1739190809942d9d692e.jpeg)



### 摘要任务微调

[![img](http://forum.deepaccess.cn/uploads/default/optimized/1X/3c454bbee354d91ded2428b93768cca7f46f4621_2_488x500.jpeg)1000×1024 271 KB](http://forum.deepaccess.cn/uploads/default/original/1X/3c454bbee354d91ded2428b93768cca7f46f4621.jpeg)



## Experiment-实验

SOTA。

HeriTransfomer是没有预训练的，说明预训练是有效的。

[![img](http://forum.deepaccess.cn/uploads/default/optimized/1X/08b1ff9d34bfeb9d25eefdfc0782cf2ff6868334_2_341x500.jpeg)1000×1465 425 KB](http://forum.deepaccess.cn/uploads/default/original/1X/08b1ff9d34bfeb9d25eefdfc0782cf2ff6868334.jpeg)



## Highlight

针对摘要这个具体任务，设计了目标为预训练hierarchical document encoder的预训练方式，而不是一般、通常、general的以预训练word embedding或sentence encoder为目标的预训练任务。

通过ablation证明了所设计预训练任务的有效性。