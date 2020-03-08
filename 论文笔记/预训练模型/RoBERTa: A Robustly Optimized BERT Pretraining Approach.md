> >ICLR2020

## Motivation- what&why

- 自我训练的方法，比如ELMo, GPT，BERT，XLM 以及XLNet等，带来了显著的性能提升，但要想确定这些方法的哪些方面对性能提升贡献最多是相当有挑战性的。由于训练在计算上成本很高，限制了可执行的调优量，而且常常使用不同大小的私有训练数据进行调优，限制了对建模进展效果的测量。
- 本文对BERT预训练模型(Devlin et al., 2019)进行了一项复制研究，包括 **仔细评估了超参数调优效果和训练集大小的影响** 。我们发现BERT明显训练不足，并提出了一个改进的训练BERT模型的方法，我们称之为 **RoBERTa** ，它可以 **达到或超过所有BERT后续方法(post-BERT)的性能** 。

## 实验设计 

- 本文的原始的BERT实验方式进行以下修改：(1)对模型进行更长时间、更大批量、更多数据的训练；(2)删除下一句预测的目标任务；(3)对更长序列进行训练；(4)动态改变应用于训练数据的masking模式。
- 实验参数主要遵循原始BERT优化的超参数，除了peak learning rate和warmup的步数，这两个参数分别针对每个设置进行调优。
- 本文还收集了一个与其他私有数据集大小相当的新数据集(CC-NEWS)，以便更好地控制训练集大小效果。

## 观察的现象和结论

- STATIC VS . DYNAMIC MASKING

  原始的BERT实现在数据预处理期间执行一次遮挡，从而产生一个静态遮挡(static mask)。为了避免在每个epoch中对每个训练实例使用相同的mask，我们将训练数据重复10次，以便在40个训练epoch中以10种不同的方式对每个序列进行遮挡。因此，在训练过程中，每个训练序列都使用相同的mask四次。结果如下：

  [![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/fdc58826cf9bf46a72d7da76a81787531998b97d_2_517x204.png)](http://forum.deepaccess.cn/uploads/default/original/1X/fdc58826cf9bf46a72d7da76a81787531998b97d.png)

- MODEL INPUT FORMAT AND NEXT SENTENCE PREDICTION

  本文将无NSP损失的训练与来自单个文档(doc - sentence)的文本块的训练进行比较。我们发现，与Devlin等人(2019)相比，该设置的性能优于最初发布的BERT结果，消除NSP损失达到或略微提高了下游任务性能。

  [![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/d95fb1e2b7b1834bf8f79e05e5580adfa2be2322_2_517x238.png)](http://forum.deepaccess.cn/uploads/default/original/1X/d95fb1e2b7b1834bf8f79e05e5580adfa2be2322.png)

- TRAINING WITH LARGE BATCHES

  本文比较了BERTBASE在增大 batch size时的复杂性和最终任务性能，控制了通过训练数据的次数。我们观察到，large batches训练提高了masked language modeling 目标的困惑度，以及最终任务的准确性。通过分布式数据并行训练，large batches也更容易并行化。

  [![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/b37defae4bfca373a918839d7baa728d5a2f4455_2_517x172.png)](http://forum.deepaccess.cn/uploads/default/original/1X/b37defae4bfca373a918839d7baa728d5a2f4455.png)

- TEXT ENCODING
  最初的BERT实现（Devlin等人，2019）使用了大小为30K的字符级BPE词汇表。 本文采用了Radford等人介绍的大小为50K的较大字节级BPE词汇。它使用字节而不是unicode字符作为基本子词单元，因此可以在不引入“unknown”标记的情况下对任何输入文本进行编码。

- 具体来说，RoBERTa采用了dynamic masking、没有NSP损失的完整句子、large mini-batches和更大的字节级BPE的训练。

- 本文预先训练了更多数据（16GB→160GB文本）和预训练更久（100K→300K→500K步），RoBERTa的开发集（Development set）结果：

  [![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/dd9cbbf6536adae3d45d7c2a2b936798e4215e8f_2_690x241.png)](http://forum.deepaccess.cn/uploads/default/original/1X/dd9cbbf6536adae3d45d7c2a2b936798e4215e8f.png)



## Highlight

- 本文重新评估了Bert的超参设置和其他实验设置（training the model longer, with bigger batches over more data; removing the next sentence prediction objective; training on longer sequences; and dynamically changing the masking pattern applied to the training data.），证明了Bert的预训练方式和其他模型相比仍具有相当地竞争力。