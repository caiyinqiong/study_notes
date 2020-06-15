> > ACL2020

源码：https://github.com/StonyBrookNLP/deformer

# 背景

本文提出了DeFormer，在低层解耦question和document的编码。

这样结构的优势在于：1）可以预先计算并存储document的表示，加快在线计算速度。2）DeFormer和原模型（e.g. BERT、XLNet）的结果类似，可以使用预训练的参数进行初始化。

这样结构的可行性在于：1）之前的研究表明低层捕捉的是局部信号（e.g. 语法层面），高层捕捉的是全局信号（e.g.语义层面）； 2）通过飞行试验表明，Bert的低层学习的document表示和question的关系不大。

<img src="../../images/image-20200609145343936.png" alt="image-20200609145343936" style="zoom:33%;" />



# 模型

<img src="../../images/image-20200609145416818.png" alt="image-20200609145416818" style="zoom:33%;" />

<img src="../../images/image-20200609145430295.png" alt="image-20200609145430295" style="zoom:50%;" />

<img src="../../images/image-20200609145449340.png" alt="image-20200609145449340" style="zoom:50%;" />

##### 训练

先用Bert预训练参数进行初始化。为了减小DeFormer和原Transformer之间的差异，增加两个类似蒸馏的辅助loss。

- 知识蒸馏损失

<img src="../../images/image-20200609145528560.png" alt="image-20200609145528560" style="zoom:50%;" />

- 每层表示的相似度损失

<img src="../../images/image-20200609145551595.png" alt="image-20200609145551595" style="zoom:50%;" />

- 总目标函数

<img src="../../images/image-20200609145618815.png" alt="image-20200609145618815" style="zoom:50%;" />



# 实验

##### 数据集

SQuAD1.1、RACE、BoolQ、MNLI、QQP

##### 实验结果

<img src="../../images/image-20200609150432371.png" alt="image-20200609150432371" style="zoom:33%;" />

<img src="../../images/image-20200609150447715.png" alt="image-20200609150447715" style="zoom:33%;" />

<img src="../../images/image-20200609150535140.png" alt="image-20200609150535140" style="zoom:33%;" />

##### 消融实验

<img src="../../images/image-20200609150603589.png" alt="image-20200609150603589" style="zoom:50%;" />

<img src="../../images/image-20200609150631122.png" alt="image-20200609150631122" style="zoom:50%;" />

![image-20200609173406415](../../images/image-20200609173406415.png)



# 结论



