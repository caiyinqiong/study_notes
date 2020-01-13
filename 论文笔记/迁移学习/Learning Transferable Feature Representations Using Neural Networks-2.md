

# [Transfer  ACL2019] Learning Transferable Feature Representations Using Neural Networks

```
论文作者：Himanshu S. Bhatt, Shourya Roy, Arun Rajkumar, Sriranjani Ramakrishnan
论文单位：AI Labs, American Express Bengaluru, India
论文地址：https://www.aclweb.org/anthology/P19-1404.pdf
代码地址：暂无
论文类别： 方法
```



### Motivation-论文解决了什么问题

对于迁移学习，学习到的表达中含有源域特有的特性会损失迁移到目标域后的性能，即会导致负迁移。

目前的已有工作中，为了目标域可适应而优化的表达通常会使在源任务上的性能有所下降。



### Motivation-本文的方法思路

本文提出了一个新的神经网络架构，显式地分开学习两部分表达，一部分是 source-specific representation，另一部分是 common representation。对于不同的任务，可以选择分开或者合并这两个表达，这样既可以提高在跨域任务（分开两个表达）上的性能，也不损失在源学习任务（合并两个表达）上的性能。



### Method-模型概述

**模型的输入**：（有标签的m个源域数据、无标签的 $m^{\prime}$ 个目标域数据）
$$
\begin{aligned}
S=\{&\left.\left(x_{i}^{S}, y_{i}^{s}\right)\right\}_{i=1}^{m} \sim\left(D_{s}\right)^{m} \\
T &=\left\{x_{i}^{t}\right\}_{i=1}^{m^{\prime}} \sim\left(D_{t}\right)^{m^{\prime}}
\end{aligned}
$$
**模型整体框架**：

<img src="https://i.loli.net/2019/12/23/s31iyFXSKa6pfcE.png" alt="image-20191223163202878.png" style="zoom:33%;" />

**模型的优化目标**：

1）**Source Classification**：最小化源域数据的误分类率。

从输入层到隐层：
$$
h(x)=\operatorname{sigm}(\mathbf{W} x+\mathbf{b})\\
h(x)=h_{s s}(x) \oplus h_{c}(x) \\
\operatorname{sigm}(a)=\left[\frac{1}{1+\exp \left(-a_{i}\right)}\right]_{i=1}^{|a|}
$$
从隐层到分类层：
$$
f(x)=\operatorname{softmax}(\mathbf{V} h(x)+\mathbf{c})
$$
目标函数：
$$
\ell(f(x), y)=\log \frac{1}{f_{y}(x)} \\
\min _{\mathbf{W}, \mathbf{V}, \mathbf{b}, \mathbf{c}}\left[\frac{1}{m} \sum_{i=1}^{m} \ell\left(f\left(x_{i}^{s}\right), y_{i}^{s}\right)\right]
$$
2）**Domain regressor**：对common representation，使其无法区分数据是来自源域/目标域；对source-specific representation，使其可以区分数据是来自源域/目标域。

将源域数据和目标域数据分别打上1和0的标签，则新构造的数据集为
$$
\left\{\left(\mathbf{x}_{i}^{s}, 1\right)\right\}_{i=1}^{m} \cup\left\{\left(\mathbf{x}_{j}^{t}, 0\right)\right\}_{j=1}^{m^{\prime}}
$$
对common representation（$\phi = h_{c}(x^{s})\ or\ h_{c}(x^{t})$），计算数据来自某域的概率：
$$
p(z=1 | \phi)=o(\phi)= \operatorname{sigm}\left(d+u^{T} \phi\right)
$$
对source-specific representation（$\phi^{\prime} = h_{ss}(x^{s})\ or\ h_{ss}(x^{t})$），计算数据来自某域的概率：
$$
p(z=1 | \phi^{\prime}）=o^{\prime}\left(\phi^{\prime}\right) =\operatorname{sigm}\left(d^{\prime}+u^{\prime T} \phi^{\prime}\right)
$$

目标函数：
$$
\begin{aligned}
\ell^{d}(o(\cdot), z) &=-z \log (o(\cdot))-(1-z) \log (1-o(\cdot))
\end{aligned}
$$
​     对common representation，最大化判断错误的概率：
$$
\begin{aligned}
 \max _{W^{\prime}, u, b, d} &\left(\frac{1}{m} \sum_{i=1}^{m} \ell^{d}\left(o\left(x_{i}^{s}\right), 1\right)+\frac{1}{m^{\prime}} \sum_{i=1}^{m^{\prime}} \ell^{d}\left(o\left(x_{i}^{t}\right), 0\right)\right)
\end{aligned}
$$
​    对source-specific representation，最小化判断错误的概率：
$$
\begin{aligned}
\min _{W^{\prime \prime}, u^{\prime}, b, d^{\prime}} &\left(\frac{1}{m} \sum_{i=1} \ell^{d^{\prime}}\left(o^{\prime}\left(x_{i}^{s}\right), 1\right)+\frac{1}{m^{\prime}} \sum_{i=1}^{m^{\prime}} \ell^{d^{\prime}}\left(o^{\prime}\left(x_{i}^{t}\right), 0\right)\right)
\end{aligned}
$$
3）**综合以上三个目标函数，得到模型总的优化目标**
$$
\min _{W, V, b, c}\left(\frac{1}{m} \sum_{i=1}^{m} \ell\left(f\left(x_{i}^{s}\right), y_{i}^{s}\right)\right) + \\
\lambda\max _{W^{\prime}, u, b, d} \left(\frac{1}{m} \sum_{i=1}^{m} \ell^{d}\left(o\left(x_{i}^{s}\right), 1\right)+\frac{1}{m^{\prime}} \sum_{i=1}^{m^{\prime}} \ell^{d}\left(o\left(x_{i}^{t}\right), 0\right)\right) + \\
\lambda\min _{W^{\prime \prime}, u^{\prime}, b, d^{\prime}} \left(\frac{1}{m} \sum_{i=1} \ell^{d^{\prime}}\left(o^{\prime}\left(x_{i}^{s}\right), 1\right)+\frac{1}{m^{\prime}} \sum_{i=1}^{m^{\prime}} \ell^{d^{\prime}}\left(o^{\prime}\left(x_{i}^{t}\right), 0\right)\right)
$$



### Experiment-实验

**实验数据集**：Amazon review dataset、Online social media（OSM） dataset

**实验一**：

![image-20191223220041257.png](https://i.loli.net/2019/12/23/ACPGeX1OkmbMS78.png)

- 通过 “Proposed”（仅使用common representation） 和 “SS+Common” 对比说明，源域特有的特性对目标域任务的性能的确会有负影响。
- 与其他针对域迁移的baseline（如 “BTDNNs”） 对比说明，显式分开学习两部分表达的优势。

**实验二：**

![image-20191223221045651.png](https://i.loli.net/2019/12/23/SlEwUxsFnI8QfOd.png)

- 实验结果表明，这样的方法也不会降低在源域任务上的性能。（注：在源域任务上，是结合source-specific representation 和 common representation 进行预测）

**实验三：**

- 论文中提到，common representation 的隐单元数 和 ss representation 的隐单元数对跨域性能的影响很大。当源域和目标域越相似时，common representation 的隐单元数 与 ss representation 的隐单元数之比 应该越大，这样会使跨域性能越好。

  

### Highlight

本文的亮点在于，抛弃原来域迁移学习的一般做法（比如，通过某些变换强制保持分布的一致性），显式地分开学习两部分表达，既提高了在跨域任务上的性能，也不损失在源域学习任务上的性能。

