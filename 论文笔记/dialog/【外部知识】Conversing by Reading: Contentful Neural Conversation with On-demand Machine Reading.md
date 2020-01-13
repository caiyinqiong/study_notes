> >ACL2019，对话，加入外部知识

代码：https://github.com/qkaren/converse_reading_cmr



## Motivation-论文解决什么问题

Neural conversation models已经能够有效学习到如何产生流畅的回答，他们的主要问题在于要知道what to say，才能使得对话contentful、不空洞、有意义。

近期方法尝试，在decoder端加入外部知识进行进行条件生成，来解决此问题。这样尽管有些效果，但是却不足以提供足够的inductive bias；模型无法实现外部知识和回答生成之间深层次、精确的整合。



## Motivation-本文的方法思路

借鉴MRC的方法。将response generation和on-demand machine reading联合建模，作者称该方法为CMR (Conversation with on-demand Machine Reading)。其关键在于，为对话模型提供即时的（on the fly）、相关的长篇文本作为一种外部知识源。

提供了一个新的、大规模的document-grounded conversation的数据集。



## Method-模型/方法概述

![IMG_2F8E1DFE291A-1.jpeg](http://forum.deepaccess.cn/uploads/default/optimized/1X/6aceafe9324d7cded4d72c2019c4296bf2870b38_2_690x350.jpeg)



## Experiment-实验

多方面优于baseline。

![ScreenShot2020-01-03at23.12.35.png](http://forum.deepaccess.cn/uploads/default/optimized/1X/8cbde8bc81d3e9199923e61ff1987c2723c3bd87_2_690x280.png)]



## Highlight

新数据集。

有了外部知识后，以MRC的思路来做response generation。