> NIPS2015，句子（段落、文档）编码

<center>Skip-Thought Vectors</center>

## 背景

文本提出，训练一个encoder-decoder模型，来重构encoded passage周围的句子。

而且介绍了一种词典扩展的方法，用以解决OOV的问题。



## 方法

####模型架构

> 输入：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117120327225.png" alt="image-20191117120327225" style="zoom:50%;" />
>
> Encoder：使用GRU作为编码器，以GRU最后一个隐状态作为S_i的句子表达h_i。
>
> Decoder：（两个decoder共享词嵌入向量V，其他的参数是分开的）
>
> 其中一个decoder预测下一个句子：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117120519717.png" alt="image-20191117120519717" style="zoom:50%;" />
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117120540677.png" alt="image-20191117120540677" style="zoom:50%;" />
>
> 另一个decoder以同样的方式预测前一个句子。
>
> loss：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117120900848.png" alt="image-20191117120900848" style="zoom:50%;" />

用以下游任务时，固定encoder，只在encoder之上训练一个分类器。



#### 词典扩展

<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191117121224329.png" alt="image-20191117121224329" style="zoom:50%;" />





## 实验





