> > ACL2019，基于检索的对话系统，预训练

<center>Training Neural Response Selection for Task-Oriented Dialogue Systems</center>

## 背景

<u>response selection</u>的对话系统范式不需要域本体的人工工程，也不需要进行文本生成，还避开了专门的决策制定模块的构建，，但是由于域内标注数据的匮乏，构建针对特定领域的基于检索的对话系统还是困难的。



本文提出两段式的训练过程：先在广泛域的语料上**预训练**一个基于IR的response selection模型，再在目标域上**微调**。

##模型

#### 预训练

数据集：Reddit数据集（预处理后得到727M个 comment-response 对）

> 模型：
>
> 这种基于表达的方式可以预先计算好，节省在线时间。
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191024212534928.png" alt="image-20191024212534928" style="zoom:50%;" />
>
> 激活函数：swish(x) = x * sigmoid(β*x)  （本文设置β=1）
>
> 相似度的计算：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191024212758381.png" alt="image-20191024212758381" style="zoom:70%;" />
>
> 目标函数：（本文采用随机的负采样，也可以使用更复杂的负采样方法）
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191024221236416.png" alt="image-20191024221236416" style="zoom:50%;" />



####微调

> 几种微调的方法做对比实验：
>
> ![image-20191024222441984](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191024222441984.png)





##实验





