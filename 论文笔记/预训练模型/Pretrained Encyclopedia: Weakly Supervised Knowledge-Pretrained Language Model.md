> > ICLR2020

## 论文解决了什么问题

提出一种弱监督方法提升预训练模型对于entity知识的捕捉。
同类型工作如ERNIE

## 方法思路

提出将部分实体替换成同类型实体，在mask language model基础上加入实体是否被替换的分类任务。

## 方法概述

- 数据准备: 采用英语wiki数据，切成512chunk，通过实体linking找到实体。

- 替换策略： 从wikidata找到实体同类实体进行替换；不替换临近实体，每个文本chunk替换十次产生10条负例数据。

- 模型结构: bert base结构

- 训练目标： MLM(5%)+实体是否被替换的分类损失

  具体过程如下图

  [![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/344afb056460072ab2a5ec4f200a14449d931944_2_690x367.png)](http://forum.deepaccess.cn/uploads/default/original/1X/344afb056460072ab2a5ec4f200a14449d931944.png)



## 实验

实验分为四个部分

#### 验证以前预训练没有学到实体知识

采用fact完成任务：将关系三元组的主语谓词转换成问题，宾语为答案（加入过个和宾语同类实体作为候选集，最终任务为排序形式）。比较BERT GPT-2和本文模型的zero-shot性能。

[![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/9fd9dde3d672e9be6fc5c27419578b6558c27a3c_2_690x292.png)](http://forum.deepaccess.cn/uploads/default/original/1X/9fd9dde3d672e9be6fc5c27419578b6558c27a3c.png)

这个实验其实没啥意义，作者模型必然稳赢，因为作者构造的预训练任务和这个fact完成任务非常类似。

#### 验证模型在下游任务有效-open-domain qa

在四个opendomain qa上与bert进行比较
首先squad上有一定提升，作者自己重新训练bert比原bert有3个点提升，作者就是多训练了1m步。
![image](http://forum.deepaccess.cn/uploads/default/original/1X/5c670e97ad79d715b109698e4c7c8c0c6dbe5dc8.png)
在open domain数据集合上和bert seini相比有提升

[![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/5db25b3c48affa70d86e4259baa829ef2d50e0c7_2_690x293.png)](http://forum.deepaccess.cn/uploads/default/original/1X/5db25b3c48affa70d86e4259baa829ef2d50e0c7.png)

加入ranking score有进一步提升， 即为每个passage额外打分。

#### 验证模型在下游任务有效 entity typing

任务是给定句子和entitymention判定entity的类型
跟ERNIE进行了比较，取得提升
![image](http://forum.deepaccess.cn/uploads/default/original/1X/b1a5442c873702898145edbe1121f92dffab9a6d.png)

#### 验证设计loss的有效性

消融实验：对比Bert表明所提loss有效，对比15%MLM得出5%MLM更好，验证BERT多些训练步效果更好，但是在实体相关任务上MLM训练多了效果不变好。只用所提loss有效性差。

[![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/7c0c370f5733fb81cbebca3bd2ac64095ad0caaa_2_690x198.png)](http://forum.deepaccess.cn/uploads/default/original/1X/7c0c370f5733fb81cbebca3bd2ac64095ad0caaa.png)



## highlight

实验支撑完善
通过替换创造弱监督任务是一个有用的思路