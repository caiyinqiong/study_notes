> > ACL2019，response selection

源码：https://github.com/CSLujunyu/Spatio-Temporal-Matching-Network

## 背景

对话任务的一个特点是谈论内容的话题是动态变化的，因此response selection时，匹配可能仅出现在最近一段上下文中。

本文提出了Spatio-Temporal Matching Network（STM），并把response selection问题形式化为多类别的分类问题。



## 方法

![image-20200203221420848](../../images/image-20200203221420848.png)

- 数据的形式：<img src="../../images/image-20200203222000333.png" alt="image-20200203222000333" style="zoom:33%;" />  其中，$\mathbf{D}=\left\{d_{0}, d_{1}, \dots, d_{m}\right\}$表示对话历史，$\mathbf{C}=\left\{c_{0}, c_{1}, \dots, c_{n}\right\}$表示候选回答集合，$R$表示标签。

- representation 模块

  使用L层BiGRU，得到

  <img src="../../images/image-20200203222202255.png" alt="image-20200203222202255" style="zoom:33%;" />

- 时空匹配模块

  <img src="../../images/image-20200203222256748.png" alt="image-20200203222256748" style="zoom:33%;" />

  <img src="../../images/image-20200203222514752.png" alt="image-20200203222514752" style="zoom:33%;" />

  对特征图使用堆叠的conv+pooling层，然后用一个全连接得到匹配值得分。

  <img src="../../images/image-20200203222637225.png" alt="image-20200203222637225" style="zoom:33%;" />



## 实验

<img src="../../images/image-20200203225434595.png" alt="image-20200203225434595" style="zoom:33%;" />



