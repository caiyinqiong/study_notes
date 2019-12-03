> > SIGIR2018，对话系统，结合外部知识

<center>Response Ranking with Deep Matching Networks and External Knowledge in Information-seeking Conversation Systems</center>

## 背景

目前大部分的对话系统研究都是开放域的闲聊 或者 针对特定任务的，，应该更关注对<u>信息查找式的对话系统</u>（Information-seeking Conversation Systems）的研究。。。而且目前的方法都缺少<u>对外界知识的建模</u>

【信息查找式的对话系统】是通过对话交互的形式，满足用户的信息需求。

目前对话系统的方法包括：

- 基于生成的
- 基于检索的（单跳、多跳）



本文提出，在深度匹配网络上融合外部知识：

- 伪相关反馈（考虑到部分候选答案太短，对深度模型有负影响。。本文提出使用候选答案作为query，在外部语料集合上进行查询，用伪相关文档来提高候选答案的表达）
- 类似QA的知识蒸馏（在信息检索中，通常需要词汇匹配和语义匹配信号，，但信息查找式的对话系统中的答案排序需要的匹配模式不同。。本文提出从外部的QA piar中提取question 和 answer 词之间的correspondence match信号）



## 模型

#### 外部知识

<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115122304145.png" alt="image-20191115122304145" style="zoom:50%;" />

#### 整体框架

> 三个模块：
>
> 1. Information retrieval (IR) module
>
>    以候选答案作为查询，从外部语料中检索相关的QA pair集合。
>
> 2. External knowledge extraction (KE) module
>
>    从外部预料的检索结果中提取有用的信息，包括词分布、词共现矩阵等。
>
> 3. Deep matching network (DMN) module
>
>    从外部知识、对话表达、候选答案进行建模，计算匹配值。

#### DMN-PRF

> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115122722488.png" alt="image-20191115122722488" style="zoom:50%;" />
>
> 外部资源引入方法：
>
> 1. 把外部语料的body域建索引
> 2. 以每个候选答案作为查询，从外部语料中检索BM25 top10文档，从top10伪相关文档中提取词频最大的10个词，追加到候选后面。
>
> 深度匹配模型：
>
> 1. 对最近的10个历史对话，每个对话表达和候选答案形成2个匹配矩阵（一个是词向量+点乘得到，另一个是将词向量作为BiGRU的输入，对BiGRU的序列输入进行点乘得到）
>
> 2. 多层【conv + ReLU + MaxPooling】的堆叠
>
> 3. 对卷积模块的输出进行flatten，作为BiGRU的输入，再通过MLP得到最终匹配值
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115123452338.png" alt="image-20191115123452338" style="zoom:50%;" />
>
> 训练：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115123538950.png" alt="image-20191115123538950" style="zoom:50%;" />

#### DMN-KD

> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115123625088.png" alt="image-20191115123625088" style="zoom:50%;" />
>
> 外部资源引入方法：
>
> 1. 从外部资源中检索出BM25 top-p（p=10）相关的QA pair：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115123913284.png" alt="image-20191115123913284" style="zoom:40%;" />
>
> 2. 利用词的共现信息构建 correspondence match 矩阵
>
>    <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115123752948.png" alt="image-20191115123752948" style="zoom:50%;" />
>
> 深度匹配模型：
>
> 相比DMN-PRF，多了一个匹配矩阵（extracted QA correspondence knowledge），其他保持一样。



## 实验

1. 数据集

   MSDialog、UDC、AliMe

2. 对比实验

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115131927229.png" alt="image-20191115131927229" style="zoom:50%;" />

3. Abliation 实验

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191115132020536.png" alt="image-20191115132020536" style="zoom:50%;" />



## 下一步研究方向

$\color{red}{建模用户意图！！！}$







