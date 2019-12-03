>>ICLR2016，answer selection


<center>LSTM-based deep learning models for non-factoid answer selection</center>

## 背景

本文提出一个基本的QA-LSTM模型，然后又从两个角度出发提出了两种改进方法。



## 模型

### QA-LSTM

> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191106173147426.png" alt="image-20191106173147426" style="zoom:50%;" />
>
> BiLSTM（共享参数，作者给出的理由是效果更好，收敛更快） + cosine
>
> LSTM之后得到句子的表达有3种方式：
>
> - max pooling
> - avg pooling
> - concat 双向的最后一个 hidden state

### QA-LSTM/CNN

> ![image-20191106173407674](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191106173407674.png)
>
> LSTM之后用CNN + k-maxpooling 得到句子的表达

### Attention-based QA-LSTM

> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191106173509091.png" alt="image-20191106173509091" style="zoom:50%;" />
>
> 在LSTM之后使用注意力机制：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191106173632379.png" alt="image-20191106173632379" style="zoom:50%;" />
>
>  

### QA-LSTM/CNN with attention

> 融合前两种改进：
>
> - LSTM之后，对question使用avg-pooling得到question的句子表示，使用question的表示来计算更新的question表示。
> - 原始的LSTM的question输出 和 更新之后的answer输出，通过CNN分别得到两者的表达。



##实验

数据集：TRECQA、InsuranceQA