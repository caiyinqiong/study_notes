> > ACL2019，预训练模型，对话系统

<center>Pretraining Methods for Dialog Context Representation Learning</center>

## 背景

目前预训练模型在很多NLP任务上都取得很大的提升，但是对于预训练方法的性质并没有很深刻的理解。

目前的EMLo、GPT、BERT等预训练方法大多都基于$\color{blue}{语言模型的变体}$（预测前一个词，下一个词，被mask的词），很少直接设计用来提取 discourse-level 信息的的预训练任务。 

BERT中的NSP预训练任务是首次显式地考虑和利用$\color{blue}{discourse-level 的关系}$。



<u>对话系统</u>有其独特的特性，需要专门为对话任务设计的预训练方法。

本文研究了不同的方法来<u>预训练 discourse-level 的语言表达</u>，并提出了两种新的预训练任务。



##模型

#### 预训练任务

1) **next utterance generation**：给定上文的多轮对话`[u_1, ……，u_{T-1}]`和 `k` 个候选，从候选中检索出正确的下一个表达 `u_T`。

> ![image-20191022190406442](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191022190406442.png)
>
> 
>
> ![image-20191022190323774](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191022190323774.png)



2) **discourse-level retrieval**：给定上文的多轮对话`[u_1, ……，u_{T-1}]`，生成下一个表达 `u_T`。

> 使用 Encoder-Decoder 的结构，前两步和next utterance generation的做法相同，得到`h_{T-1}`，然后使用BiLSTM解码生成下一个utterance。
>
> 
>
> loss函数是每一步预测的loss之和，假设`u_T=[w_k, ……, w_N]`，则
>
> ![image-20191022191013034](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191022191013034.png)



3) **masked utterance retrieval**（本文新提出的）：给定多轮对话的上下文`[u_1, ……，u_{T}]`，其中某个表达`u_t`是被mask掉的，从`k`个候选中检索出正确的被mask掉的表达 `u_t`。

> ![image-20191022230755868](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191022230755868.png)
>
> 
>
> ![image-20191022230854476](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191022230854476.png)
>
> 
>
> ![image-20191022230921890](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191022230921890.png)



4) **inconsistency identification**（本文新提出的）：给定多轮对话的上下文`[u_1, ……，u_{T}]`，其中某个表达`u_t`是被随机替换掉的，此任务用于发现不一致的表达。

>![image-20191022231212440](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191022231212440.png)
>
>
>
>![image-20191022231307992](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191022231307992.png)



## 实验

#### 下游任务

在MultiWoz数据集上进行。

1）next utterance generation

2）next utterance retrieval 

3）dialog act prediction

4）belief state prediction

#### 实验结果

1. 相比不使用任何预训练的，有了预训练后可以更快收敛。
2. NUG预训练学到的表达的泛化能力和表达能力较好。

3. InI 和 MUR 预训练任务为每个utterance学到了很强的局部表达。

（缺陷：总体而言，这几个下游任务和预训练任务都是非常相似的）

