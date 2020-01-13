>>ACL2019，多段落MRC



## Motivation-论文解决了什么问题

- 多个支撑文本的阅读理解，要求跨多个文本寻找答案，以往模型往往得到每个文本的表达，然后再进行文本之间的交互，缺乏细粒度多个文本之间的信息交互。
- 本文提出一种动态自我注意力机制，减少注意力机制计算代价和显存，帮助进行词粒度的文本交互，提升模型效果



## Method-模型/方法概述

##### 动态自我注意力模块：首先从长文本序列中评估词的重要性，选出topk的词集合，然后在该集合上进行自我注意力机制的计算。（可以节省资源）

<img src="../../images/image-20200107192907460.png" alt="image-20200107192907460" style="zoom:33%;" />

##### 整体模型

![image-20200107193101159](../../images/image-20200107193101159.png)



## Experiment-实验

- 实验数据集：SearchQA、Quasar-T、WikiHop（多选QA数据集，评估指标为acc）
- 实验及结论

1. 在三个数据集上的结果

   <img src="http://forum.deepaccess.cn/uploads/default/optimized/1X/5335934e67a1c69880e099a8491c09166d266c3c_2_466x500.png" alt="image" style="zoom:60%;" />         <img src="../../images/image-20200107195955940.png" alt="image-20200107195955940" style="zoom:50%;" />

2. 消融实验
   <img src="http://forum.deepaccess.cn/uploads/default/original/1X/87c31481c9738dd12a5bd64e1858c5bb958ad065.png" alt="image" style="zoom:50%;" />

3. 在SQUAD上验证所提结构的速度快，内存消耗小
   <img src="http://forum.deepaccess.cn/uploads/default/original/1X/0e8d316ce6ce7ed5d9faf37c0696009cd43fe4ee.png" alt="image" style="zoom:50%;" />

4. 验证超参数K的作用：K增大到一定程度后，效果不再增长。通过K可以控制速度、内存、精度的trade-off；图b说明使用的passage越多，效果越好。

   <img src="http://forum.deepaccess.cn/uploads/default/optimized/1X/e1cb7c8ddd73e3b9c6948ddb56294054eaa0826f_2_369x500.png" alt="image" style="zoom:80%;" />



## Highlight

- 论文提出一种新颖的快速计算self attention的方法，相比进行attention weight的截断，作者直接进行token选择，减少计算量同时效果也有提升。