>> **ACL2019**，**对RNN结构的改进**

------

<center>A Lightweight Recurrent Network for Sequence Modeling<center>

## 背景

RNN结构由于其弱并行的结构特性，导致计算不高效。

为了解决这个问题，之前也有人提出一些其他结构的改进方法，例如 ATR(addition-subtraction twin-gated recurrent unit)、SRU(simple recurrent unit)。



本文提出了$\color{red}{LRN（lightweight\ recurrent\ network）}$模型，结合了ATR和SRU结构的优势：

* 使用 input_gate 和 forget_gate 解决长期依赖和梯度消失/爆炸的问题

* 所有参数相关的运算都在循环外进行



##模型

####1. 模型结构

> 输入序列：![image-20191021184334619](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191021184334619.png)
>
> 模型结构：
>
> ![image-20191021185523066](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191021185523066.png)

#### 2. LRN 与 self-attention network 的关系

> LRN结构可以分解成：
>
> ![image-20191021191324711](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191021191324711.png)

* self-attention 的 weight 在所有 token 上是进行了L2标准化的。

* LRN 的 weight 是单向的，未标准化的，在所有通道上进行的。

#### 3. 长短期依赖

> ![image-20191021192234502](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191021192234502.png)

#### 4. 梯度消失/爆炸

> BPTT 的梯度消失和爆炸是因为连乘U导致的：
>
> ![image-20191021192411752](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191021192411752.png)
>
> ![image-20191021192424569](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191021192424569.png)

> LRN 的反向梯度：
>
> ![image-20191021192559675](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191021192559675.png)



## 实验

在6个NLP任务上进行实验，实验效果和现有的RNN差不多或略差，但效率有所提高。



