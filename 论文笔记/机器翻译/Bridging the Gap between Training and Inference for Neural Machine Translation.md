> > ACL2019



## Motivation-论文解决了什么问题

神经机器翻译模型根据之前的上下文信息依次解码出目标语言单词。在训练阶段，模型用真实的单词作为上下文信息进行预测，而在推断阶段，它必须从头开始生成整个序列，只能用前面时间步的预测结果作为上下文信息进行预测。这种训练和推断之间的上下文信息差异导致了错误的累积，使得模型必须在训练阶段未见过的情况下进行预测。此外，单词级别的训练要求预测序列和真实序列必须严格匹配，这使得翻译不同但合理的句子被过度校正。



## Motivation-本文的方法思路

为解决训练和推断之间的上下文信息差异，改善翻译结果的过度校正问题，本文提出首先从预测结果中选出oracle words，然后从oracle words和ground truth words中采样选取上下文单词。oracle words的选取方法分为单词级别和句子级别。在训练的初始阶段，模型以更大的概率选择ground truth words作为上下文，随着模型逐渐收敛，oracle words被选中的概率逐渐增大。通过这种采样选取上下文的方式，模型在推断阶段可以更好地处理错误，也能改善翻译结果的过度校正问题。



## Method-模型/方法概述

### RNN-based NMT Model

假设源语言句子为 x=x1,…,x|x| ，对应的目标语言句子为 y∗=y∗1,…,y∗|y∗| 。

#### Encoder

<img src="http://forum.deepaccess.cn/uploads/default/original/1X/020832f9a2c17327fb30efe7bc3d583c12b2fbe8.png" alt="image" style="zoom:33%;" />

#### Attention

<img src="http://forum.deepaccess.cn/uploads/default/original/1X/7288d5ba076e457dd0e9b66bd22e3270dbaa359e.png" alt="image" style="zoom:33%;" />

#### Decoder

<img src="http://forum.deepaccess.cn/uploads/default/original/1X/6e37af62e4ea7a12472d5607a2d99b7a6cb48e41.png" alt="image" style="zoom:33%;" />

### Sampling with Decay

公式(7)和(8)中的 yj−1 以 p 的概率选择真实的目标语言单词 y∗j−1 ，以 1−p 的概率选择上一时间步预测的目标语言单词 yoraclej−1 。
在训练的初始阶段，模型还未被训练好，因此要更多地选择 y∗j−1 作为 yj−1 ；在训练后期，要更多地选择 yoraclej−1 作为 yj−1 。因此，概率 p 随着训练进程的增加而降低。

[![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/c5fc017a043da4fb3af4e97663627c588abf1681_2_345x83.png)](http://forum.deepaccess.cn/uploads/default/original/1X/c5fc017a043da4fb3af4e97663627c588abf1681.png)

![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/eaeb180cf239a03a46bf6c92f50c7da5efe233b9_2_345x163.png)



### Oracle Word Selection

#### Word-Level Oracle

在第 j 个解码时间步，直接选择概率最高的单词作为 yoraclej−1 。为了提高鲁棒性，本文中加入了Gumbel噪声。
<img src="http://forum.deepaccess.cn/uploads/default/original/1X/29c568027ad750f52fd89b5f6d026db7b9870b0e.png" alt="image" style="zoom:33%;" />
<img src="http://forum.deepaccess.cn/uploads/default/original/1X/f399af5b2099bdc73d5e6de1eb271361966cbc25.png" alt="image" style="zoom:33%;" />

[![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/3a64ffbba9720a4089d51d09d3031601b655cfec_2_345x155.png)](http://forum.deepaccess.cn/uploads/default/original/1X/3a64ffbba9720a4089d51d09d3031601b655cfec.png)

#### Sentence-Level Oracle

首先利用beam search算法进行第一轮解码，得到 k 条最好的候选译文。然后计算这 k 条候选译文的BLEU值，选出得分最高的译文作为oracle sentence，记作 yS=(yS1,...,yS|yS|) 。最后进行第二轮解码，在第 j 个解码时间步，定义
<img src="http://forum.deepaccess.cn/uploads/default/original/1X/b4704d20f97347529641991de8e9fdcdda7034dc.png" alt="image" style="zoom:33%;" />

但Sentence-Level Oracle存在一个问题，用beam search算法得到的oracle sentence和ground truth sentence不一定等长。为解决这个问题，本文提出了force decoding，以保证这两条句子等长。

#### Force decoding

ground truth sentence的长度记作 |y∗| ，Force decoding的目标是使所有候选译文的长度都等于 |y∗| 。

- 如果在第 j(j≤|y∗|) 个解码时间步，预测概率分布 Pj 最高概率对应的单词是EOS，则选择第二高的概率对应的单词作为候选译文的第 j 个单词。
- 如果在第 |y∗|+1 个解码时间步，预测概率分布 P|y∗|+1 最高概率对应的单词不是EOS，则选择EOS作为候选译文的第 |y∗|+1 个单词。

## Experiment-实验

- 实验数据集
  Zh→En：NIST dataset
  En→De：WMT14 dataset

- 评价指标
  Zh→En：case-insensitive BLEU值，计算脚本mteval-v11b.pl。
  En→De：case-sensitive BLEU值，计算脚本multi-bleu.pl。

- 实验结果

  ![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/4783163d64992f76e279dc7b650925ca8e3da228_2_517x225.png)

  ![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/5ee697983e57cf453209df6f66dd5ecd0be78a98_2_316x250.png)

本文提出word-level oracle、sentence-level oracle和Gumbel noise来改善过度校正的问题，并通过实验分析了这些因素的影响程度。

![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/8e75e1582cd3e435bd0d5be604720da4c534ff31_2_345x187.png)


只用word-level oracle时，翻译效果提升1.21个BLEU值，当用sentence-level oracle时，又提升0.62个BLEU值，说明sentence-level oracle比word-level oracle更好。在此基础上在加入Gumbel noise，word-level oracle和sentence-level oracle分别提升0.56和0.53个BLEU值，说明加入Gumbel noise可以使采样更鲁棒。

本文还分析了不同的因素对收敛性的影响。

[![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/a43dd5f7b99c3d498385c365fb7b9b81cd55830f_2_282x500.png)](http://forum.deepaccess.cn/uploads/default/original/1X/a43dd5f7b99c3d498385c365fb7b9b81cd55830f.png)


采样和噪声虽然会降低收敛速度，但是会避免过拟合问题。



## Highlight

本文为解决神经机器翻译模型在训练和推断阶段因上下文差异导致的错误累积问题和翻译结果过度校正问题，提出在解码阶段，不仅要选择ground truth words作为上下文信息进行预测，还要以一定的概率选择oracle words作为上下文信息进行预测，其中oracle words可以从单词级别或句子级别中选择。实验证明sentence-level oracle比word-level oracle的效果好。