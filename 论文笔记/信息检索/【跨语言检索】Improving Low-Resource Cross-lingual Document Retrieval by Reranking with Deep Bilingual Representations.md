> >ACL2019



## 论文解决什么问题

跨语言检索(CLIR） 是通过query 去对外语文档排序的问题，传统方法这样分两步做: 机器翻译 + 单语言信息检索。 一般上更进一步可以分成query-query翻译、doc-doc翻译，以及检索模块这样三个。

跨语言检索传统方法有，神经排序模型有， 神经跨语言排序模型没有，已有问题如下：
1、在跨语言的背景下，双语的query-doc 语料不知道怎么直接应用之前的神经IR模型去学习query-doc直接的relevance.
2、 这个任务的资源（语料）非常少

作者的贡献在于：
1、没人做的神经跨语言排序模型
2、针对性解决资源少的问题



## 本文的方法思路

![image](http://forum.deepaccess.cn/uploads/default/original/1X/72c132decb2e84df9d914909b33660b1bb67802d.png)

### 双语Q-D pair 怎么用的问题：

双语 query 、 document , Q和D 两两匹配， [source Q- Source D、 S Q- target D、 T Q - S D 、T Q - T D ] ， 用四个组件（是四个神经网络）去学习这四部分，最后的relevance score 是这四部分的组合。

### 语料资源少的问题：

1、先用 query likelihood 做一遍 initial retrival [增加语料的质量] ，
再rerank 。 并且rerank 学的时候加入QL模型的score作为额外特征 [补足特征]。
2、神经模型，初始层是word embedding， 预训练的相当于额外的语料。



## 方法概述

s(Q,D)=s(Q,D)+s(Q,Dˆ)+s(Qˆ,Dˆ)+s(Qˆ,D) ， 上面说的，最后score是这四个score相加
模型结构上沿用了前人一篇 结合PACRR-DRMM 的工作
俩模型：
1、POSIT-DRMM

[![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/022e1c71aa65fd23ca18e0ab9b5af1164a84eb89_2_690x129.png)](http://forum.deepaccess.cn/uploads/default/original/1X/022e1c71aa65fd23ca18e0ab9b5af1164a84eb89.png)

预训练的word2vec(source、target) , 双向lstm， 做点乘归一化，max pooling, k-max pooling， MLP， 最后的term-gating是DRMM里那种考虑了query 词权重的机制
2、POSIT-DRMM

![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/c95c49d4257c38142184045be81c0f37a738e911_2_689x95.png)

结构和前人论文里的一样



## 实验

用Indri 来做QL
额外特征（QL的score , IDF)

### Cross-lingual Word Embeddings:

前人工作， [MUSE](http://github.com/facebookresearch/MUSE) 实现

### MATERIAL数据集介绍

[MATERIAL](http://www.iarpa.gov/index.php/research-programs/material)

[![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/d265ae4f6c39fde792e0c0d305abde67927648fb_2_517x120.png)](http://forum.deepaccess.cn/uploads/default/original/1X/d265ae4f6c39fde792e0c0d305abde67927648fb.png)

EN：英语
SW：Swahili 斯瓦西里语
TL： Tagalog 塔加拉族语
SO：Somali 索马里语
可以看到语料是较少的

### 对比baseline

[![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/ba53b4a649b35c61fd3be8fde852f4ba4d32e309_2_517x243.png)](http://forum.deepaccess.cn/uploads/default/original/1X/ba53b4a649b35c61fd3be8fde852f4ba4d32e309.png)


传统方法包含
1、query 用DBQT、PSQ的方法翻译过去(长度短) ，再用Indri检索
2、document 用 SMT 和NMT的方法翻译过来（长度长），再用Indri检索
Deep Relevance Ranking的baseline都是用SMT把文档翻译过来再在单语上检索的

结论：
1、这个任务整体水平现状较差，跨语言检索难度大
2、直接搬neural ranking 的模型还不如传统方法
3、加特征效果明显
4、ensemble效果明显



## Highlight

神经跨语言排序模型没人做， 任务型驱动的一种思路
现实问题语料少要解决