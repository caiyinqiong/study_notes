> > ACL2019，迁移学习

------

<center>Learning Transferable Feature Representations Using Neural Networks</center>

## 背景

对于迁移学习，学习到的表达中含有的源域的专有特性会损失迁移到目标域后的性能。而且，为了目标域可适应而优化的表达通常会使在源任务上性能有所下降。



本文提出$\color{red}{显式地分开学习两部分表达}$，一部分是 `source-specific representation`，另一部分是 `common representation`。对于不同的任务，可以选择分开这两个表达或者合并这两个表达，这样既可以提高在跨域任务（分开两个表达）上的性能，也不损失在源学习任务（合并两个表达）上的性能。



## 模型

网络有两个优化目标：

1）分类目标：最小化在源数据上的误分类率。

2）域区分目标（对抗学习的思路）：对common representation，使其无法区分数据是来自源域or目标域；对source-specific representation，使其可以区分数据是来自源域or目标域。

> 模型的输入是一对源数据和目标数据。
>
> ![image-20191022115019553](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191022115019553.png)

> 表达层：
>
> ![image-20191022122254055](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191022122254055.png)
>
> ![image-20191022122318368](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191022122318368.png)

> 源域分类层：
>
> ![image-20191022122349421](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191022122349421.png)

> 域回归层：（z=1表示数据来自源域，z=0表示数据来自目标域）
>
> ![image-20191022123835833](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191022123835833.png)
>
> 
>
> ![image-20191022123917114](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191022123917114.png)
>
> 

> 总目标函数：
>
> ![image-20191022122516801](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191022122516801.png)
>
> 分类的目标函数：
>
> ![image-20191022124025212](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191022124025212.png)
>
> 回归的目标函数：
>
> ![image-20191022124048212](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191022124048212.png)



