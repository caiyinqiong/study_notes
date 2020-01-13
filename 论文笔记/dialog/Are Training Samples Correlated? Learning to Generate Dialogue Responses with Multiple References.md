> > ACL2019，dialog



## Motivation-论文解决什么问题

开放域对话生成，存在generic response的问题。作者认为这是由于之前方法，往往基于输入query到response的1-1映射。忽略了同一query实质上存在的多个references（1-n映射），且多个references之间的关系也没有得到挖掘。



## Motivation-本文的方法思路

依据上述观察，提出了一个两步式的生成框架。先生成一个query的多个回答的common feature，再在此基础上根据每个回答的distinct feature生成多个diverse且合适的回答。

![ScreenShot2020-01-03at22.07.48.png](http://forum.deepaccess.cn/uploads/default/optimized/1X/a72938dd99058ea143a3dd034d90bb2d8ead2e3a_2_548x499.png)



## Method-模型/方法概述



![IMG_698C65526096-1.jpeg](http://forum.deepaccess.cn/uploads/default/optimized/1X/d0e3d9e1350b18d6a27e0ea4083db39f715cd02c_2_690x347.jpeg)



## Experiment-实验

Automatic的指标上优势貌似不明显，人工评测的指标上是最优的。

![ScreenShot2020-01-03at22.24.21.png](http://forum.deepaccess.cn/uploads/default/optimized/1X/d012bf6e3af02c4aa92f7e7249d62c8b3aae87ed_2_690x340.png)



![ScreenShot2020-01-03at22.25.53.png](http://forum.deepaccess.cn/uploads/default/optimized/1X/db923319f7d90cae45359ee111d4bfcb412b6ad2_2_690x466.png)



## Highlight

利用了同一query的多个references，进行1-n的映射。

两步式的生成策略，common feature --> distinct feature。

额外的监督信息 Ldisc ：输入x和生成y的相似度、x和正确{y}的相似度，认为两者要一致。

Distinct feature建模时的loss。

利用两个网络分开建模有无gold answer的分布，最小化生成分布的差异来训练模型参数。

网络在training和testing阶段的不同。

自动化的评价指标上看不出明显提升，人工评价才能看出有提升，不知所述方法实际中是否真的有效。

