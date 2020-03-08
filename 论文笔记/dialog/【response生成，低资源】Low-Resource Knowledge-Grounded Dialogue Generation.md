> > ICLR2020

```
论文作者：Xueliang Zhao, Wei Wu, Chongyang Tao, Can Xu, Dongyan Zhao, Rui Yan
论文单位：清华, 微软
论文地址：https://openreview.net/forum?id=rJeIcTNtvS
代码/数据地址：
论文类别：方法
```

## Motivation-论文解决什么问题

Ungrounded对话系统缺乏对讨论主题的知识，表现不尽人意。本文关注基于文档的（Document-grounded）对话生成。

该任务数据集要求对话与文档匹配，不易大量获取。

现有SOTA模型仅是在小训练集合上过拟合。表现为，将模型运用于训练数据外的领域（domain）时，表现急剧下降。

面对新领域或新语言，限于人力，很难获取到足够的训练数据。

## Motivation-本文的方法思路

将response decoder分解，使得基于知识的对话部分的参数少且独立。这样，模型的绝大部分参数就可以用相对容易得到的ungrounded dialogue和plain text来训练。

## Method-模型/方法概述

[![IMG_4A64A0CA00DB-1.jpeg](http://forum.deepaccess.cn/uploads/default/optimized/1X/64157bb2de469679bf515a1f5b3d27db59c5e70d_2_690x345.jpeg)](http://forum.deepaccess.cn/uploads/default/original/1X/64157bb2de469679bf515a1f5b3d27db59c5e70d.jpeg)

1. 先用ungrounded dialogue数据集训练context encoder、response decoder、context processor。
2. 固定response decoder参数，用ungrounded dialogue数据集中的所有utterances训练response decoder、language model。
3. 用plain text训练knowledge encoder，当作双向语言模型训练。
4. 固定上述所有参数，用相对较少的数据训练knowledge processor的参数。

## Experiment-实验

[![IMG_603BA0036B9E-1.jpeg](http://forum.deepaccess.cn/uploads/default/optimized/1X/012cab66cffc21b350b864d1bdbbd889a9190416_2_690x193.jpeg)](http://forum.deepaccess.cn/uploads/default/original/1X/012cab66cffc21b350b864d1bdbbd889a9190416.jpeg)

[![IMG_4D300940FF03-1.jpeg](http://forum.deepaccess.cn/uploads/default/optimized/1X/2493fdd6bf24e60f250844a5dd3526c06fe3546a_2_690x366.jpeg)](http://forum.deepaccess.cn/uploads/default/original/1X/2493fdd6bf24e60f250844a5dd3526c06fe3546a.jpeg)

> Test **Seen** contains new dialogues with topics appearing in the training set, while topics in Test **Unseen** never appear in the training set and the validation set, and thus the data allow us to examine the **generalization** ability of models.

所提模型在大多数指标上都SOTA，即使是在1/8训练数据的情况下。

Incremental Transformer with Deliberation Decoder (ITDD)发表于ACL 2019。其PPL在Test Seen上表现好，在Test Unseen上表现不佳，很可能是由于其two-pass decoder导致了模型过拟合。



需要注意，所提模型相对baselines，还使用了大量ungrounded dialogue和plain text。关于这点，论文“尽全力”使得Transformer Memory Network (TMN)模型也使用了上述数据，但是效果其效果仍不如本文方法。

[![Screen Shot 2020-01-14 at 22.27.47.png](http://forum.deepaccess.cn/uploads/default/optimized/1X/4f32b79343a115d95b33afa71a83092f9e798d69_2_690x225.png)](http://forum.deepaccess.cn/uploads/default/original/1X/4f32b79343a115d95b33afa71a83092f9e798d69.png)



除了固定实现训练好的绝大部分参数，还尝试了不固定即参数全部微调的训练方法。结果表明，在document-grounded对话数据足够的情况下，fine-tune会有一些提升。但在low-resource情况下，训练数据较少，不固定、较多参数的模型可能在这些数据上过拟合，导致表现下降。

![Screen Shot 2020-01-14 at 22.34.47.png](http://forum.deepaccess.cn/uploads/default/optimized/1X/9e36586fdcdd3f4f9a9fdbbb729542e03937b681_2_690x229.png)



还与MASS进行了比较。

> To adapt it to the knowledge-grounded dialogue generation task, we concatenate the knowledge sentences and conversational history as a long context as the input of the encoder.

本文模型优于MASS。

![Screen Shot 2020-01-14 at 22.36.49.png](http://forum.deepaccess.cn/uploads/default/optimized/1X/0443e61aef134836582e8db19a284748143c67e8_2_690x345.png)



## Highlight

将decoder分解，使得可利用大量相对易获取的数据，训练模型的绝大部分参数。

固定上述参数，余下的少量参数用少量训练数据训练。适用于low-resource的任务。

实验证明该方法可行，并且取得了SOTA的表现。

实验相对公平、对比详尽。

感觉想法很elegant、且很实用。泛用性也比较好，很多地方都可以借鉴。