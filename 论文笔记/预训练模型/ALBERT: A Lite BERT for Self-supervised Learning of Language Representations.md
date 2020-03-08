> > ICLR2020

- Motivation - 论文解决什么问题

  - 在预训练自然语言表示时增加模型大小通常会提高下游任务的性能。 但是，由于GPU / TPU内存的限制，更长的训练时间以及意外的模型降级，在某些时候，进一步的模型增加变得更加困难。
  - 本文主要通过两种方法来减少模型参数：1）factorized embedding parameterization. 2）cross-layer parameter sharing。

- Method-模型/方法概述ALBERT的总体架构和Bert类似，主要有以下三个特点：

  - Factorized embedding parameterization
    之前的模型的embedding size E 和 hidden size H 相等，对模型自身和实际应用都不是最优的。本文首先将one-hot 向量映射到低维的空间 E，再映射到隐层空间 H，这样使得embedding parameters从 O(V*H) 降到 O(V*E+E*H) 。

  - Cross-layer parameter sharing
    ALBERT默认在层间共享所有参数。

  - Inter-sentence coherence loss
    本文认为NSP将topic prediction和coherence prediction合并，而前者较后者简单，因此最终学习的只是topic的信息。本文将其替换为setence-order prediction，具体来说正例和NSP相同，负例将正例的两个segments颠倒，这迫使模型学习关于话语级连贯性的细粒度区别。被证明提高了下游任务的性能。

  - ALBERT和BERT的参数比较：

    [![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/a81d9fc6d89be1216e46a747746af465074514d6_2_690x172.png)](http://forum.deepaccess.cn/uploads/default/original/1X/a81d9fc6d89be1216e46a747746af465074514d6.png)

- Experiment-实验

  - MLM任务采用n-gram masking, batch size 4096, learning rate 0.00176, training steps 125000.

  - 只有 BERT-Large 70% 的参数量，ALBERT-xxlarge 能实现显著的性能提升:

    [![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/59f33287a6d88a702ab1f1963b1b8857f7bfd506_2_690x154.png)](http://forum.deepaccess.cn/uploads/default/original/1X/59f33287a6d88a702ab1f1963b1b8857f7bfd506.png)

  - 研究者还注意到，即使在训练了 100 万步之后，最大的模型仍然没有过拟合。因此，他们决定删除 dropout，以进一步提高模型能力。

    [![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/88c1351b76896c34545b4ad67c602a9ee6e6ee79_2_690x80.png)](http://forum.deepaccess.cn/uploads/default/original/1X/88c1351b76896c34545b4ad67c602a9ee6e6ee79.png)

  - ALBERT 在 GLUE、SQuAD 和 RACE 基准测试中都取得了 SOTA 结果:

    [![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/6493734e4582ea8476e5040fac698035eb2fff00_2_690x255.png)](http://forum.deepaccess.cn/uploads/default/original/1X/6493734e4582ea8476e5040fac698035eb2fff00.png)

    [![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/50aa5e9c4ce3aed2914f36e698bf62c57ab66270_2_690x283.png)](http://forum.deepaccess.cn/uploads/default/original/1X/50aa5e9c4ce3aed2914f36e698bf62c57ab66270.png)

- Highlight - 论文的亮点是什么？

  - 本文对减少bert的参数以提高参数有效性提出了新方案。